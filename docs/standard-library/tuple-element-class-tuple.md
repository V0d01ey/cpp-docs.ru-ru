---
title: Класс tuple_element
ms.date: 11/04/2016
f1_keywords:
- utility/std::tuple_element
helpviewer_keywords:
- std::tuple_element
ms.assetid: 4c51a6c1-ce81-462f-8c6c-291d69f2b77c
ms.openlocfilehash: be9d9fe56d35e96e4179eb511edccd475a369f32
ms.sourcegitcommit: 43cee7a0d41a062661229043c2f7cbc6ace17fa3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/13/2020
ms.locfileid: "92008293"
---
# <a name="tuple_element-class"></a>Класс tuple_element

Создает оболочку для элемента `tuple` . Специализации создают программу-оболочку для элементов `array` и `pair`.

## <a name="syntax"></a>Синтаксис

```cpp
// CLASS tuple_element (find element by index)
template <size_t Index, class Tuple>
   struct tuple_element;

// tuple_element for const
template <size_t Index, class Tuple>
   struct tuple_element<Index, const Tuple>;

// tuple_element for volatile
template <size_t Index, class Tuple>
   struct tuple_element<Index, volatile Tuple>;

// tuple_element for const volatile
template <size_t Index, class Tuple>
   struct tuple_element<Index, const volatile Tuple>;

// Helper typedef
template <size_t Index, class Tuple>
   using tuple_element_t = typename tuple_element<Index, Tuple>::type;

// Specialization for arrays
template <size_t Index, class Elem, size_t Size>
   struct tuple_element<Index, array<Elem, Size>>;

// Specializations for pairs
// struct to determine type of element 0 in pair
template <class T1, class T2>
   struct tuple_element<0, pair<T1, T2>>;

// struct to determine type of element 1 in pair
template <class T1, class T2>
   struct tuple_element<1, pair<T1, T2>>;
```

### <a name="parameters"></a>Параметры

*Номер*\
Индекс указанного элемента.

*Кортеж*\
Тип кортежа.

*Elem*\
Тип элемента массива.

*Изменять*\
Размер массива.

*T1*\
Тип первого элемента в паре.

*T2*\
Тип второго элемента в паре.

## <a name="remarks"></a>Комментарии

Шаблон класса `tuple_element` имеет вложенное typedef `type` , который является синонимом для типа по индексу *индекса* *кортежа*типа кортежа.

Определение типа `tuple_element_t` является удобным псевдонимом для `tuple_element<Index, Tuple>::type`.

Специализация шаблона класса для массивов предоставляет интерфейс в `array` качестве кортежа `Size` элементов, каждый из которых имеет один и тот же тип. Каждая специализация имеет вложенное typedef `type` , который является синонимом для типа элемента *индекса* объекта `array` , с сохранением любых квалификаций const-volatile.

Специализации шаблона для типов `pair` предоставляют по одному определению типа члена, `type`, которое является синонимом типа элемента в указанной позиции в паре с сохранением всех квалификаторов const и(или) volatile. Определение типа `tuple_element_t` является удобным псевдонимом для `tuple_element<N, pair<T1, T2>>::type`.

Используйте [ &lt; служебную программу &gt; Get](../standard-library/utility-functions.md#get) для возврата элемента в указанной позиции или указанного типа.

## <a name="example-get-an-element-from-a-tuple"></a>Пример. получение элемента из кортежа

```cpp
#include <tuple>
#include <string>
#include <iostream>

using namespace std;
typedef tuple<int, double, string> MyTuple;

int main() {
    MyTuple c0{ 0, 1.5, "Tail" };

    tuple_element_t<0, MyTuple> val = get<0>(c0); //get by index
    tuple_element_t<1, MyTuple> val2 = get<1>(c0);
    tuple_element_t<2, MyTuple> val3 = get<string>(c0); // get by type

    cout << val << " " << val2 << " " << val3 << endl;
}
```

```Output
0 1.5 Tail
```

## <a name="example-get-an-element-from-an-array"></a>Пример. получение элемента из массива

```cpp
#include <array>
#include <iostream>

using namespace std;
typedef array<int, 4> MyArray;

int main()
{
    MyArray c0 { 0, 1, 2, 3 };

    for (const auto& e : c0)
    {
        cout << e << " ";
    }
    cout << endl;

    // display first element "0"
    tuple_element<0, MyArray>::type val = c0.front();
    cout << val << endl;
}
```

```Output
0 1 2 3
0
```

## <a name="example-get-an-element-from-a-pair"></a>Пример. получение элемента из пары

```cpp
#include <utility>
#include <iostream>

using namespace std;

typedef pair<int, double> MyPair;
int main() {
    MyPair c0(0, 1.333);

    // display contents "0 1"
    cout << get<0>(c0) << " ";
    cout << get<1>(c0) << endl;

    // display first element "0 " by index
    tuple_element<0, MyPair>::type val = get<0>(c0);
    cout << val << " ";

    // display second element by type
    tuple_element<1, MyPair>::type val2 = get<double>(c0);
    cout << val2 << endl;
}
```

```Output
0 1.333
0 1.333
```

## <a name="requirements"></a>Требования

**Заголовок:**\<tuple>

**Заголовок:** \<array> (для специализации массива)

**Заголовок:** \<utility> (для пар специализаций)

**Пространство имен:** std
