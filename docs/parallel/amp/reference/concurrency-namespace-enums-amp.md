---
title: Перечисления пространства имен Concurrency (AMP)
ms.date: 11/04/2016
f1_keywords:
- amp/Concurrency::access_type
- amp/Concurrency::queuing_mode
ms.assetid: 4c87457e-184f-4992-81ab-ca75e7d524ab
ms.openlocfilehash: 3dbb8f265706f7a4c369c80d3050cd1bfd2f5acb
ms.sourcegitcommit: ec6dd97ef3d10b44e0fedaa8e53f41696f49ac7b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/25/2020
ms.locfileid: "88845098"
---
# <a name="concurrency-namespace-enums-amp"></a>Перечисления пространства имен Concurrency (AMP)

[Перечисление access_type](#access_type)\
[Перечисление queuing_mode](#queuing_mode)

## <a name="access_type-enumeration"></a><a name="access_type"></a> Перечисление access_type

Тип перечисления, используемый для обозначения различных типов доступа к данным.

```cpp
enum access_type;
```

### <a name="values"></a>Значения

|Имя|Описание|
|----------|-----------------|
|`access_type_auto`|Автоматически выберите оптимальное значение `access_type` для ускорителя.|
|`access_type_none`|Специальное. Выделение доступно только в ускорителе, а не на ЦП.|
|`access_type_read`|Используемый. Выделение доступно для ускорителя и доступно для чтения на ЦП.|
|`access_type_read_write`|Используемый. Выделение доступно для ускорителя и доступно для записи в ЦП.|
|`access_type_write`|Используемый. Выделение доступно для ускорителя и доступно для чтения и записи в ЦП.|

## <a name="queuing_mode-enumeration"></a><a name="queuing_mode"></a> Перечисление queuing_mode

Указывает режимы работы в очереди, которые поддерживаются ускорителем.

```cpp
enum queuing_mode;
```

### <a name="values"></a>Значения

|Имя|Описание|
|----------|-----------------|
|`queuing_mode_immediate`|Режим очереди, указывающий, что любые команды, например [Parallel_for_each функции (C++ amp)](concurrency-namespace-functions-amp.md#parallel_for_each), отправляются соответствующему устройству ускорителя, как только они возвращаются вызывающему.|
|`queuing_mode_automatic`|Режим очереди, указывающий, что команды ставятся в очередь в очереди команд, соответствующей объекту [accelerator_view](accelerator-view-class.md) . Команды отправляются на устройство при вызове [accelerator_view:: Flush](accelerator-view-class.md#flush) .|

## <a name="see-also"></a>См. также раздел

[Пространство имен Concurrency (C++ AMP)](concurrency-namespace-cpp-amp.md)
