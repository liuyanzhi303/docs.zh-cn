---
description: 了解详细信息： ICorDebugCodeEnum：： Next 方法
title: ICorDebugCodeEnum::Next 方法
ms.date: 03/30/2017
api_name:
- ICorDebugCodeEnum.Next
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugCodeEnum::Next
helpviewer_keywords:
- ICorDebugCodeEnum::Next method [.NET Framework debugging]
- Next method, ICorDebugEnum interface [.NET Framework debugging]
ms.assetid: 644ece86-384d-4c63-9fba-52c789616ff7
topic_type:
- apiref
ms.openlocfilehash: 51d46718891ce3df537c675175eacc4e33b92f79
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/06/2021
ms.locfileid: "99764744"
---
# <a name="icordebugcodeenumnext-method"></a>ICorDebugCodeEnum::Next 方法

从当前位置开始，从枚举中获取指定的 "ICorDebugCode" 实例数。

## <a name="syntax"></a>语法

```cpp
HRESULT Next (
    [in] ULONG  celt,
    [out, size_is(celt), length_is(*pceltFetched)]
        ICorDebugCode *values[],
    [out] ULONG *pceltFetched
);
```

## <a name="parameters"></a>参数

`celt`  
中 `ICorDebugCode` 要检索的实例数。

`values`  
弄指针的数组，其中每个都指向一个 `ICorDebugCode` 对象。

`pceltFetched`  
弄一个指针，指向 `ICorDebugCode` 实际返回的实例数。 如果为1，则此值可以为 null `celt` 。

## <a name="requirements"></a>要求

**平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。

**标头**：CorDebug.idl、CorDebug.h

**库：** CorGuids.lib

**.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]
