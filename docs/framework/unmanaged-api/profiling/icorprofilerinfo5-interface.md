---
description: 了解详细信息： ICorProfilerInfo5 接口
title: ICorProfilerInfo5 接口
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo5
api_location:
- mscorwks.dll
api_type:
- COM
ms.assetid: 7bd48c34-37ed-4230-9eec-39a17280f05d
topic_type:
- apiref
ms.openlocfilehash: c89faf3db08adeee3ffcfbb755a5da5ae44b3c7d
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/06/2021
ms.locfileid: "99737267"
---
# <a name="icorprofilerinfo5-interface"></a>ICorProfilerInfo5 接口

[仅在 .NET Framework 4.5.2 及更高版本中受支持]  
  
 [ICorProfilerInfo4](icorprofilerinfo4-interface.md)的子类，它提供了一些方法，代码探查器可以使用这些方法与公共语言运行 (时) ，以控制事件监视。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[GetEventMask2 方法](icorprofilerinfo5-geteventmask2-method.md)|获取探查器要为其接收来自 CLR 的通知的当前事件类别。|  
|[SetEventMask2 方法](icorprofilerinfo5-seteventmask2-method.md)|设置一个指定事件类型的值，探查器将为该类事件接收来自 CLR 的事件通知。|  
  
## <a name="remarks"></a>备注  

 此接口上的可用方法旨在替换 [ICorProfilerInfo：： GetEventMask](icorprofilerinfo-geteventmask-method.md) 和 [ICorProfilerInfo：： SetEventMask](icorprofilerinfo-seteventmask-method.md) 方法。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v452plus](../../../../includes/net-current-v452plus-md.md)]  
  
## <a name="see-also"></a>请参阅

- [分析接口](profiling-interfaces.md)
