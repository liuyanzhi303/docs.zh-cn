---
description: 了解有关以下内容的详细信息：存储扩展性
title: 存储扩展性
ms.date: 03/30/2017
ms.assetid: 7c3f4a46-4bac-4138-ae6a-a7c7ee0d28f5
ms.openlocfilehash: f04c466224aacd1c8f755e7aa60b18846d0c7180
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/06/2021
ms.locfileid: "99755221"
---
# <a name="store-extensibility"></a>存储扩展性

<xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore> 允许用户提升自定义的特定于应用程序的属性，这些属性可用于查询持久性数据库中的实例。 提升属性的操作将使属性值可用在数据库的特殊视图中。 这些提升的属性（可用于用户查询的属性）可以是简单的类型，如 Int64、Guid、String 和 DateTime，也可以是序列化的二进制类型 (byte[])。

<xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore> 类具有 <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore.Promote%2A> 方法，可以使用该方法将某个属性提升为可以在查询中使用的属性。 以下示例是存储扩展性的一个端对端示例。

1. 在此示例方案中，文档处理 (DP) 应用程序具有多个工作流，每个工作流都使用自定义的活动进行文档处理。 这些工作流具有一组状态变量，最终用户需要看到这些变量。 为了实现此目的，DP 应用程序提供了一个类型为 <xref:System.Activities.Persistence.PersistenceParticipant> 的实例扩展，活动使用该实例扩展来提供状态变量。

    ```csharp
    class DocumentStatusExtension : PersistenceParticipant
    {
        public string DocumentId;
        public string ApprovalStatus;
        public string UserName;
        public DateTime LastUpdateTime;
    }
    ```

2. 然后，将新的扩展添加到主机。

    ```csharp
    static Activity workflow = CreateWorkflow();
    WorkflowApplication application = new WorkflowApplication(workflow);
    DocumentStatusExtension documentStatusExtension = new DocumentStatusExtension ();
    application.Extensions.Add(documentStatusExtension);
    ```

     有关添加自定义持久性参与者的详细信息，请参阅 [持久性参与者](persistence-participants.md) 示例。

3. DP 应用程序中的自定义活动在 **Execute** 方法中填充各种状态字段。

    ```csharp
    public override void Execute(CodeActivityContext context)
    {
        // ...
        context.GetExtension<DocumentStatusExtension>().DocumentId = Guid.NewGuid();
        context.GetExtension<DocumentStatusExtension>().UserName = "John Smith";
        context.GetExtension<DocumentStatusExtension>().ApprovalStatus = "Approved";
        context.GetExtension<DocumentStatusExtension>().LastUpdateTime = DateTime.Now();
        // ...
    }
    ```

4. 当工作流实例到达持久性点时， **DocumentStatusExtension** 持久性参与者的 **CollectValues** 方法会将这些属性保存到持久性数据集合中。

    ```csharp
    class DocumentStatusExtension : PersistenceParticipant
    {
        const XNamespace xNS = XNamespace.Get("http://contoso.com/DocumentStatus");

        protected override void CollectValues(out IDictionary<XName, object> readWriteValues, out IDictionary<XName, object> writeOnlyValues)
        {
            readWriteValues = new Dictionary<XName, object>();
            readWriteValues.Add(xNS.GetName("UserName"), this.UserName);
            readWriteValues.Add(xNS.GetName("ApprovalStatus"), this.ApprovalStatus);
            readWriteValues.Add(xNS.GetName("DocumentId"), this.DocumentId);
            readWriteValues.Add(xNS.GetName("LastModifiedTime"), this.LastUpdateTime);

            writeOnlyValues = null;
        }
        // ...
    }
    ```

    > [!NOTE]
    > 持久性框架通过 **SaveWorkflowCommand. InstanceData** 集合将所有这些属性传递给 **SqlWorkflowInstanceStore** 。

5. DP 应用程序初始化 SQL 工作流实例存储并调用 **升级** 方法来升级此数据。

    ```csharp
    SqlWorkflowInstanceStore store = new SqlWorkflowInstanceStore(connectionString);

    List<XName> variantProperties = new List<XName>()
    {
        xNS.GetName("UserName"),
        xNS.GetName("ApprovalStatus"),
        xNS.GetName("DocumentId"),
        xNS.GetName("LastModifiedTime")
    };

    store.Promote("DocumentStatus", variantProperties, null);
    ```

    根据此促销信息， **SqlWorkflowInstanceStore** 将数据属性置于 [InstancePromotedProperties](#InstancePromotedProperties) 视图的列中。

6. 为了从提升表中查询某个数据子集，DP 应用程序在提升视图之上添加一个自定义视图。

    ```sql
    create view [dbo].[DocumentStatus] with schemabinding
    as
        select  P.[InstanceId] as [InstanceId],
            P.Value1 as [UserName],
            P.Value2 as [ApprovalStatus],
            P.Value3 as [DocumentId],
            P.Value4 as [LastUpdatedTime]
    from [System.Activities.DurableInstancing].[InstancePromotedProperties] as P
    where P.PromotionName = N'DocumentStatus'
    go
    ```

## <a name="systemactivitiesdurableinstancinginstancepromotedproperties-view"></a><a name="InstancePromotedProperties"></a> [InstancePromotedProperties] 视图

|列名|列类型|说明|
|-----------------|-----------------|-----------------|
|InstanceId|GUID|此提升所属的工作流实例。|
|PromotionName|nvarchar(400)|提升本身的名称。|
|Value1、Value2、Value3、..、Value32|sql_variant|已提升属性本身的值。 除了长度超过 8000 个字节的二进制 Blob 和字符串之外，多数 SQL 基元数据类型都可适合 sql_variant。|
|Value33、Value34、Value35、…、Value64|varbinary(max)|显式声明为 varbinary(max) 的已提升属性的值。|
