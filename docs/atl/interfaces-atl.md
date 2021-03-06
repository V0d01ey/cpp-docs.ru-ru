---
title: Интерфейсы (ATL)
ms.date: 11/04/2016
ms.topic: reference
helpviewer_keywords:
- COM interfaces
- interfaces, COM
ms.assetid: de6c8b12-6230-4fdc-af66-a28b91d5ee55
ms.openlocfilehash: 56d5a010bc28257dce181ee33e0ddf74655ccd3c
ms.sourcegitcommit: c123cc76bb2b6c5cde6f4c425ece420ac733bf70
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2020
ms.locfileid: "81319386"
---
# <a name="interfaces-atl"></a>Интерфейсы (ATL)

Интерфейс — это способ, с которым объект предоставляет свою функциональность внешнему миру. В COM интерфейс представляет собой таблицу указателей (например, таблицу c'vtable) для функций, реализованных объектом. Таблица представляет интерфейс и функции, на которые она указывает, являются методами этого интерфейса. Объект может выставлять напоказ столько интерфейсов, сколько пожелает.

Каждый интерфейс основан на фундаментальном интерфейсе COM, [IUnknown](../atl/iunknown.md). Методы позволяют навигации `IUnknown` на другие интерфейсы, подвергающиеся воздействию объекта.

Кроме того, каждому интерфейсу предоставляется уникальный идентификатор интерфейса (IID). Эта уникальность упрощает поддержку версии интерфейса. Новая версия интерфейса — это просто новый интерфейс с новым IID.

> [!NOTE]
> II для стандартных интерфейсов COM и OLE предопределены.

## <a name="see-also"></a>См. также раздел

[Введение в модель COM](../atl/introduction-to-com.md)<br/>
[Объекты com и интерфейсы](/windows/win32/com/com-objects-and-interfaces)
