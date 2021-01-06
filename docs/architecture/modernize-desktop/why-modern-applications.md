---
title: 为什么使用新式桌面应用程序
description: 了解当今世界 Windows 窗体、WPF 和 UWP 等桌面技术。
ms.date: 09/16/2019
ms.openlocfilehash: f8b70ba9e0ee97a6e0938e3219ecd0d2324248ae
ms.sourcegitcommit: 9a4488a3625866335e83a20da5e9c5286b1f034c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2020
ms.locfileid: "97866424"
---
# <a name="why-modern-desktop-applications"></a>为什么使用新式桌面应用程序

## <a name="introduction"></a>简介

### <a name="a-story-of-one-company"></a>一家公司的故事

在早期2000s，一家跨国公司开始开发一个分布式桌面解决方案，以在公司的不同分支之间交换信息，并对集中单元执行优化操作。 他们选择了一个名为 Windows 窗体的全新框架， (其应用程序开发也称为 WinForms) 。 多年来，项目发展为具有数百行代码的成熟、经过充分测试且经过时间考验的应用程序。 经过的时间和 .NET Framework 2.0 不再是热新技术。 正在处理此应用程序的开发人员面临着困境。 他们希望在开发中使用最新的技术堆栈，并使其应用程序的外观和 "感受"。 同时，他们不想放弃在15年内生成的优秀产品，并从头开始重写整个应用程序。

### <a name="your-story"></a>故事

你可能会发现自己在同一船上，在这种情况下，你可以在很成熟的 Windows 窗体或 Windows Presentation Foundation (WPF) 应用程序，这些应用程序已证明了他们的可靠性。 您可能想要继续使用这些应用程序。 同时，由于这些应用程序是在一段时间之前编写的，因此它们可能缺少各种功能，如新式外观、性能、与新设备和平台功能的集成等，这为他们提供了 "老式技术"。 还有另一个问题需要您作为开发人员。 在处理旧的 .NET Framework 版本并维护一段时间之前编写的应用程序时，可能会觉得你不会学习新的技术，也不会缺少构建现代技术技能的知识。 如果这是你的故事，这本书非常适合你！

## <a name="desktop-applications-nowadays"></a>桌面应用程序如今

在 Internet 引发之前，桌面应用程序是构建软件系统的主要方法。 开发人员可以选择任何编程语言，如 COBOL、Fortran、VB6 或 c + +。 但在开发小工具或复杂的分布式体系结构的位置上，它们都是桌面应用程序。

然后，Internet 技术开始骇人听闻开发世界，并通过类似于简单的部署和简化分发过程的优势来赢得越来越多的工程师。 在将 web 应用程序部署到生产环境中时，所有用户都获得了自动更新，这对软件灵活性产生了巨大影响。

不过，Internet 基础结构、基础协议和 HTTP 和 HTML 等标准并非专门用于构建复杂的应用程序。 事实上，我们的主要开发工作只是要为 web 应用程序指定桌面应用程序所具有的相同功能，如快速数据输入和状态管理。

尽管 web 和移动应用程序的发展速度令人难以置信，但对于某些任务，桌面应用程序在效率和性能方面仍保留了这一位置。 这说明了有数百万个开发人员正在用 WPF 和 WinForms 生成项目，并且这些应用程序的数量不断增长。

下面是在开发中选择桌面应用程序的一些原因：

- 桌面应用程序能够更好地与用户的 PC 交互。
- 用于复杂计算的桌面应用程序的性能远远高于 web 应用程序的性能。
- 在客户端上运行自定义逻辑是可行的，但使用 web 应用程序会更难。
- 在桌面应用程序中使用多线程处理更为简单且更有效。
- 用于设计用户界面 (Ui) 的学习曲线并不不足。 对于 WinForms，与 Windows 窗体设计器的拖放体验完全直观。
- 无需设置服务器基础结构，也无需关心连接性问题、防火墙和浏览器兼容性，就可以轻松地开始编写和测试算法。
- 与 web 调试相比，调试功能更强大。
- 可以轻松访问硬件设备，如相机、蓝牙或卡读取器。
- 由于该技术已有一段时间了，因此有很多专家和知识库可用于开发桌面应用程序。

正如您所看到的，出于许多原因，桌面开发非常有用。 这项技术经过了成熟和时间的测试，开发周期速度很快，调试功能功能强大，而且可以更轻松地开始使用桌面应用程序。

Microsoft 在过去的几年中引入了许多 UI 桌面技术，从1995到通用 Windows 平台在2016发布的 (UWP) 。

![Microsoft 桌面技术](./media/why-modern-applications/microsoft-desktop-technologies.png)

根据2016年4月发布的 Telerik 发布的调查，生成 Windows 桌面应用程序的最常用技术是 Windows 窗体、WPF 和 UWP。

![显示 Windows 窗体、WPF 和 UWP 作为最常用桌面技术的 Telerik 调查](./media/why-modern-applications/telerik-survey.png)

您可以使用 c # 和 Visual Basic 在其中任意进行开发，但我们将更深入地探讨。

### <a name="windows-forms"></a>Windows 窗体

第一次发布于2002，Windows 窗体是一种托管框架，是在 Windows 图形设备接口 (GDI) 引擎上构建的最早、最常用的桌面技术。 它为在 Visual Studio 中开发用户界面提供了一个流畅的拖放体验。  同时，Windows 窗体依赖 Visual Studio 设计器作为你开发用户界面的主要方式，因此，从代码创建可视化组件并不简单。

下面的列表总结了 Windows 窗体的主要特征：

- 具有大量代码示例和文档的成熟技术。
- 功能强大且高效的设计器。 设计 UI "代码" 不是很方便。
- 由于设计器的拖放体验，学习起来简单直观。
- 在任何 Windows 版本上受支持。
- 在 .NET Core 3.0 及更高版本上受支持。

### <a name="wpf"></a>WPF

WPF 根据 XAML 语言规范，区分 UI 与代码之间的清晰分离。 XAML 提供了类似于构建大型应用程序的模板、样式和绑定等功能。 与 Windows 窗体一样，它是一个托管框架，但设计是模块化且可重用。

下面是 WPF 的主要功能：

- 成熟的技术。
- 设计器可用，但开发人员通常更倾向于使用声明性 XAML 通过代码创建设计。
- 学习曲线的陡峭不是 Windows 窗体。
- 在任何 Windows 版本上受支持。
- 在 .NET Core 3.0 及更高版本上受支持。

### <a name="uwp"></a>UWP

UWP 并不只是一个显示框架（如 WPF 和 Windows 窗体），但它也是一个平台本身。 此平台有：

- 它自己的 API 集 (Windows 运行时 API) 。
- 新的部署系统 (.MSIX) 
- 一种新式应用程序生命周期模型， (降低电池消耗) 。
- 新的资源管理系统 (基于 PRI 文件) 。

创建该平台是为了支持各种类型的输入系统 (如墨迹、触摸、游戏板、鼠标、键盘、注视等) ，在性能和电池使用不足的情况下，所有形式因素都是如此。 由于这些原因，Windows 10 操作系统的 shell 使用了 UWP 平台的组成部分。

![UWP 结构](./media/why-modern-applications/uwp-structure.png)

UWP 包含基于 XAML 的表示框架（如 WPF），但它有一些重要的差异，例如：

- 应用程序是在应用容器中执行的。 应用容器控制 UWP 应用可访问的资源。
- 仅在 Windows 10 上受支持。
- 可以通过 Microsoft Store 部署应用，以便更轻松地进行部署。
- 作为 Windows 运行时 API 的一部分设计。
- 包含一组丰富的丰富内置控件以及通过 Microsoft UI 库 NuGet 包提供的其他控件 (WinUI 库) 每隔几个月更新一次。

## <a name="a-tale-of-two-platforms"></a>两个平台的 tale

在过去的20年中，虽然 UI 桌面技术不断增长并遵循从 Windows 窗体到 UWP 的路径，但该硬件还会从具有小型 CRT 监视器的重型 PC 单元不断发展到高 DPI 监视器，并具有触控和墨迹等不同的数据输入技术。 这些更改导致创建了两个不同的概念：桌面应用程序和新式应用程序。 现代应用程序是一种考虑不同设备外形规格、各种输入和输出方法并利用新式桌面功能的应用程序。 另一方面， (传统) 桌面应用程序是一种应用程序，该应用程序需要具有最适合鼠标和键盘的控件密度的实体 UI。

下表描述了这两个概念之间的差异：

|   | 新式应用程序 | 桌面应用程序 |
| --- | --- | --- |
| 安全性 | 包含执行 &amp; 的重要基础知识。 旨在确保用户的隐私性、管理电池寿命，并专注于保护设备安全。  | 用户 &amp; 管理员安全级别。 你具有对注册表和硬盘驱动器文件夹的本机访问权限。 |
| 部署 | 安装和更新由平台进行管理。   | MSI、自定义安装 &amp; 程序更新。 传统上，开发人员和 IT 经理非常头痛。  |
| 分发 | 受信任 &amp; 的分发签名包。 分发是从受信任的源执行的，从不从 web 进行。  | Web，SCCM &amp; 自定义分发。 不控制已安装的内容，会影响整个计算机。   |
| UI | 新式 UI。 不同的输入机制、墨迹、触摸、游戏板、键盘、鼠标等。  | Windows 窗体、WPF、MFC。 设计用于密集 UI 的鼠标和键盘，并可充分利用桌面的工作效率。  |
| 数据 | 具有见解的云优先数据。 云中的真实来源。 了解应用会发生什么情况以及如何执行。  | 本地数据。 传统桌面应用程序通常需要一些本地数据。  |
| 设计 | 旨在重复使用。 在不同的平台、前端和后端之间重复使用，在很多位置运行资产。  | 仅适用于 Windows 桌面  |

作为向开发人员提供生成应用程序的最佳工具的承诺的一部分，Microsoft 投入了大量精力来实现这些概念，我们甚至可以更深入地介绍平台，使开发人员能够获得两全其美。 为此，Microsoft 在两个平台之间执行了双向操作。

![新式应用程序与桌面应用程序之间的双向工作量](./media/why-modern-applications/bidirectional-effort.png)

1. 将桌面应用程序方案移动到新式应用程序平台。 传统的桌面开发仍然很常见，因为它能很好地解决某些情况。 使用这些常见桌面方案并将其引入新式桌面平台，使平台完全可用是有意义的。

    ![将桌面应用程序方案移动到新式应用程序平台](./media/why-modern-applications/desktop-to-modern.png)

1. 将新式应用程序功能移到桌面应用程序中。 对于需要一种方法来利用新式功能而不是从头开始进行重写的现有桌面应用程序，将新式应用程序平台中的功能推送到桌面应用程序中。

    ![将新式应用程序功能移到桌面应用程序中](./media/why-modern-applications/modern-to-desktop.png)

在本书中，我们将重点介绍第二部分，并展示如何实现现有桌面应用程序的现代化。

## <a name="paths-to-modernization"></a>现代化的路径

本指南的结构反映了三个不同的轴来完成现代化：新式功能、部署和安装。

### <a name="modern-features"></a>新式功能

假设你有一个有效的 Windows 窗体应用程序，你的公司的销售代表将使用该应用程序来填写客户订单。 新的要求是为了使客户能够使用 tablet 笔对订单进行签名。 墨迹在当前的操作系统和技术中是本机的，但它在开发应用程序时不可用。

此路径将向你展示如何在现有桌面开发中利用新式桌面功能。

### <a name="deployment"></a>部署

新式开发周期旨在为每个用户部署新版本的应用程序提供灵活性。 由于 Windows 窗体和 WPF 应用程序基于必须存在于计算机上的 .NET Framework 的特定版本，因此它们不能利用新的 .NET Framework 版本功能，而无需 IT 人员干预就会对同一台计算机上运行的其他应用产生副作用。 它限制了开发人员的创新步调，使其保持在过时版本的 .NET Framework 上。

自 .NET Core 3.0 的启动以来，你可以利用一种新的方法来并行部署多个版本的 .NET Core，并指定每个应用程序应面向哪个版本的 .NET Core。 这样，你就可以在一个应用程序中使用最新功能，同时相信你不会中断任何其他应用程序。

### <a name="installation"></a>安装

桌面应用程序始终依赖于某种安装过程，然后用户才能开始使用它们。 从 MSI 和 ClickOnce 到自定义安装程序甚至是 XCOPY 部署，这一事实进入了游戏一系列技术。 这些方法中的任何一种都涉及微妙的问题，因为应用程序需要一种方法来访问计算机上的共享资源。 有时，安装需要访问注册表以插入或更新新键值，有时会更新主应用程序所引用的共享 Dll。 这会给用户带来麻烦，因为这会让用户感觉到，一旦您安装了某个应用程序，您的计算机将永远不会相同，即使以后卸载也是如此。

本书介绍了如何使用 .MSIX 安装应用程序的新方法，以解决前面所述的问题。 你将了解如何轻松地设置应用程序的打包、安装和更新。

>[!div class="step-by-step"]
>[上一页](index.md)
>[下一页](whats-new-dotnet-core.md)