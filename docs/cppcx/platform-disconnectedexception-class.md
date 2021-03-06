---
title: Класс Platform::DisconnectedException
ms.date: 12/30/2016
ms.topic: reference
f1_keywords:
- VCCORLIB/Platform::DisconnectedException
- VCCORLIB/Platform::DisconnectedException::DisconnectedException
helpviewer_keywords:
- Platform::DisconnectedException
ms.assetid: c25e0d64-5bff-4c21-88e5-c4ec2776fa7f
ms.openlocfilehash: 56276a7d09cc82e05886c2c67a4784993083adb5
ms.sourcegitcommit: 0ab61bc3d2b6cfbd52a16c6ab2b97a8ea1864f12
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "62387621"
---
# <a name="platformdisconnectedexception-class"></a>Класс Platform::DisconnectedException

Вызывается, если прокси-объект COM пытается выполнить ссылку на сервер COM, которые больше не существует

## <a name="syntax"></a>Синтаксис

```
public ref class DisconnectedException : COMException,    IException,    IPrintable,    IEquatable
```

### <a name="remarks"></a>Примечания

Если класс A ссылается на другой класс (класс B) в отдельном процессе, то классу A требуется прокси-объект для связи с находящимся вне процесса сервером COM, который содержит класс B. Иногда серверу может не хватать памяти, а класс A об этом не знает. В этом случае вызывается исключение RPC_E_DISCONNECTED и преобразуется в исключение Platform::DisconnectedException. Один из сценариев, в котором это происходит, связан с ситуацией, когда источник события вызывает предоставленного ему делегата, но делегат был удален в некоторой точке после своей подписки на событие. Когда это происходит, источник события удаляет делегата из своего списка вызова.

Дополнительные сведения см. в описании класса [COMException](../cppcx/platform-comexception-class.md) .

### <a name="requirements"></a>Требования

**Минимальный поддерживаемый клиент:** Windows 8

**Минимальный поддерживаемый сервер:** Windows Server 2012

**Пространство имен:** Platform

**Метаданные:** platform.winmd

## <a name="see-also"></a>См. также

[Класс Platform::COMException](../cppcx/platform-comexception-class.md)
