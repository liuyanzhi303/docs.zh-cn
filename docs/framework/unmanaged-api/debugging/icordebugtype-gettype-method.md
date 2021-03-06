---
description: 了解详细信息： ICorDebugType：： GetType 方法
title: ICorDebugType::GetType 方法
ms.date: 03/30/2017
api_name:
- ICorDebugType.GetType
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugType::GetType
helpviewer_keywords:
- ICorDebugType::GetType method [.NET Framework debugging]
- GetType method, ICorDebugType interface [.NET Framework debugging]
ms.assetid: d6e64534-4d47-4ad0-a340-7590e07e2b4a
topic_type:
- apiref
ms.openlocfilehash: 922791e51855badfb1fd548e08953a2f660f971a
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/06/2021
ms.locfileid: "99658212"
---
# <a name="icordebugtypegettype-method"></a>ICorDebugType::GetType 方法

获取一个 CorElementType 值，该值描述 <xref:System.Type> 此 ICorDebugType 所表示 (CLR) 的公共语言运行时的本机类型。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetType (  
    [out] CorElementType   *ty  
);  
```  
  
## <a name="parameters"></a>参数  

 `ty`  
 弄一个指针，指向 `CorElementType` 枚举的值，该值指示 <xref:System.Type> 此表示的 CLR `ICorDebugType` 。  
  
## <a name="remarks"></a>备注  

 如果的值 `ty` 为 ELEMENT_TYPE_CLASS 或 ELEMENT_TYPE_VALUETYPE，则可以调用 [ICorDebugType：： GetClass](icordebugtype-getclass-method.md) 方法来获取泛型类型的非实例化类型; 否则，不会调用 `ICorDebugType::GetClass` 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]
