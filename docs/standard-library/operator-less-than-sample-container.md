---
title: оператор&lt; (&lt;образец контейнера&gt;)
ms.date: 11/04/2016
f1_keywords:
- std::operator<
- operator<
- std.<
- <
- std.operator<
- std::<
helpviewer_keywords:
- < operator, comparing specific objects
- operator<, valarrays
- < operator
- operator <, valarrays
ms.assetid: 31027dd6-53be-428b-b950-1dcb25393597
ms.openlocfilehash: 6ef43fb762c4da71062fc846048f21c0112bfafc
ms.sourcegitcommit: 857fa6b530224fa6c18675138043aba9aa0619fb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2020
ms.locfileid: "80215271"
---
# <a name="operatorlt-ltsample-containergt"></a>оператор&lt; (&lt;образец контейнера&gt;)

> [!NOTE]
> Этот раздел находится в документации Майкрософт C++ как нефункциональный пример контейнеров, C++ используемых в стандартной библиотеке. Дополнительные сведения см. в разделе [Контейнеры стандартной библиотеки C++](../standard-library/stl-containers.md).

Перегрузка **оператора <** для сравнения двух объектов [контейнера](../standard-library/sample-container-class.md)шаблона класса.

## <a name="syntax"></a>Синтаксис

```cpp
template <class Ty>
bool operator<(
    const Container <Ty>& left,
    const Container <Ty>& right);
```

## <a name="return-value"></a>Возвращаемое значение

Возвращает `lexicographical_compare(left.begin, left.end, right.begin, right.end)`.

## <a name="see-also"></a>См. также раздел

[\<образец контейнера>](../standard-library/sample-container.md)\
[начать](../standard-library/container-class-begin.md)\
[end](../standard-library/container-class-end.md)
