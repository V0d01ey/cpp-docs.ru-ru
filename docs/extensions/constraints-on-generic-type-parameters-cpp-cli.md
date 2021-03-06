---
title: Ограничения, применяемые к параметрам универсальных типов (C++/CLI)
ms.date: 10/12/2018
ms.topic: reference
f1_keywords:
- where
helpviewer_keywords:
- where keyword [C++]
- constraints, C++
ms.assetid: eb828cc9-684f-48a3-a898-b327700c0a63
ms.openlocfilehash: 829f11c9f0c3935f9a415cae381cfc12d88df18a
ms.sourcegitcommit: c1fd917a8c06c6504f66f66315ff352d0c046700
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/16/2020
ms.locfileid: "90686760"
---
# <a name="constraints-on-generic-type-parameters-ccli"></a>Ограничения, применяемые к параметрам универсальных типов (C++/CLI)

В объявлениях универсального типа или метода можно указывать параметр типа с ограничениями. Ограничение — это требование, которому должны удовлетворять типы, используемые в качестве аргументов типа. Например, ограничение может требовать, чтобы аргумент типа реализовывал определенный интерфейс или наследовался от определенного класса.

Ограничения не являются обязательными; отсутствие ограничения на параметр эквивалентно ограничению этого параметра значением <xref:System.Object>.

## <a name="syntax"></a>Синтаксис

```cpp
where type-parameter: constraint list
```

### <a name="parameters"></a>Параметры

*Тип-параметр*<br/>
Один из ограниченных параметров типа.

*constraint list*<br/>
*constraint list* — это разделенный запятыми список спецификаций ограничений. Этот список может содержать интерфейсы, которые должны быть реализованы параметром типа.

Список также может содержать класс. Чтобы аргумент типа удовлетворял ограничению базового класса, он должен быть того же класса, что и ограничение, или производным от ограничения.

Можно также ввести **gcnew()**, чтобы указать, что аргумент типа должен иметь открытый конструктор без параметров, **ref class**, чтобы указать, что аргумент типа должен быть ссылочным типом, включая любой тип класса, интерфейса, делегата или массива, или **value class**, чтобы указать, что аргумент типа должен быть типом значения. Можно указать любой тип значения \<T> , кроме Nullable.

В качестве ограничения можно также указать универсальный параметр. Аргумент типа, указанный для ограничиваемого типа, должен иметь тип ограничения или наследоваться о него. Это называется открытым ограничением типа.

## <a name="remarks"></a>Remarks

Предложение ограничения состоит из ключевого слова **where**, за которым следуют параметр типа, двоеточие (**:**) и ограничение, определяющее характер ограничения параметра типа. **where** — контекстно-зависимое ключевое слово. Подробные сведения см. в статье [Context-Sensitive Keywords (C++/CLI and C++/CX)](context-sensitive-keywords-cpp-component-extensions.md) (Контекстно-зависимые ключевые слова (C++/CLI and C++/CX)). Несколько предложений **where** следует разделять пробелом.

Ограничения применяются к параметрам типа для задания ограничений на типы, которые могут использоваться в качестве аргументов для универсальных типов и методов.

Ограничения класса и интерфейса определяют, что типы аргументов должны быть указанного класса или унаследованы от него либо должны реализовывать указанный интерфейс.

Применение ограничений к универсальному типу или методу позволяет коду в этом типе или методе использовать известные возможности ограниченных типов. Например, можно объявить универсальный класс так, чтобы параметр типа реализовывал интерфейс `IComparable<T>`:

```cpp
// generics_constraints_1.cpp
// compile with: /c /clr
using namespace System;
generic <typename T>
where T : IComparable<T>
ref class List {};
```

Это ограничение требует, чтобы аргумент типа, используемый для `T`, реализовывал `IComparable<T>` во время компиляции. Оно также позволяет вызывать методы интерфейса, например `CompareTo`. Для вызова методов интерфейса приведение типа в экземпляре параметра типа не требуется.

Вызывать статические методы в классе аргумента типа с помощью параметра типа нельзя. Их можно вызывать только с использованием фактического имени типа.

Ограничение не может быть типом значения, включая встроенные типы, такие как **`int`** или **`double`** . Поскольку типы значений не могут иметь производные классы, ограничению может удовлетворять только один класс. В этом случае универсальный шаблон можно переписать с заменой параметра типа определенным типом значения.

Ограничения требуются в некоторых случаях, поскольку компилятор не допускает использование методов и других функций неизвестного типа, если ограничениями не определено, что неизвестный тип поддерживает методы или интерфейсы.

Для одного параметра типа можно определить несколько ограничений в списке, разделенном запятыми.

```cpp
// generics_constraints_2.cpp
// compile with: /c /clr
using namespace System;
using namespace System::Collections::Generic;
generic <typename T>
where T : List<T>, IComparable<T>
ref class List {};
```

При наличии нескольких параметров типа для каждого из них следует использовать по одному предложению **where**. Пример:

```cpp
// generics_constraints_3.cpp
// compile with: /c /clr
using namespace System;
using namespace System::Collections::Generic;

generic <typename K, typename V>
   where K: IComparable<K>
   where V: IComparable<K>
ref class Dictionary {};
```

Ограничения в коде следует использовать в соответствии с указанными ниже правилами.

- Ограничения можно перечислять в любом порядке.

- Ограничения также могут быть типами классов, например абстрактными базовыми классами. Однако они не могут быть типами значений и запечатанными классами.

- Ограничения сами не могут быть параметрами типа, но могут содержать параметры типа в открытом сконструированном типе. Пример:

    ```cpp
    // generics_constraints_4.cpp
    // compile with: /c /clr
    generic <typename T>
    ref class G1 {};

    generic <typename Type1, typename Type2>
    where Type1 : G1<Type2>   // OK, G1 takes one type parameter
    ref class G2{};
    ```

## <a name="examples"></a>Примеры

В следующем примере демонстрируется использование ограничений для вызова экземплярных методов для параметров типа.

```cpp
// generics_constraints_5.cpp
// compile with: /clr
using namespace System;

interface class IAge {
   int Age();
};

ref class MyClass {
public:
   generic <class ItemType> where ItemType : IAge
   bool isSenior(ItemType item) {
      // Because of the constraint,
      // the Age method can be called on ItemType.
      if (item->Age() >= 65)
         return true;
      else
         return false;
   }
};

ref class Senior : IAge {
public:
   virtual int Age() {
      return 70;
   }
};

ref class Adult: IAge {
public:
   virtual int Age() {
      return 30;
   }
};

int main() {
   MyClass^ ageGuess = gcnew MyClass();
   Adult^ parent = gcnew Adult();
   Senior^ grandfather = gcnew Senior();

   if (ageGuess->isSenior<Adult^>(parent))
      Console::WriteLine("\"parent\" is a senior");
   else
      Console::WriteLine("\"parent\" is not a senior");

   if (ageGuess->isSenior<Senior^>(grandfather))
      Console::WriteLine("\"grandfather\" is a senior");
   else
      Console::WriteLine("\"grandfather\" is not a senior");
}
```

```Output
"parent" is not a senior
"grandfather" is a senior
```

Если в качестве ограничения используется параметр универсального типа, такое ограничение называется открытым ограничением типа. Открытые ограничения типа применимы, когда функция-член со своим параметром типа должна ограничивать этот параметр параметром содержащего типа.

В приведенном ниже примере `T` — открытое ограничение типа в контексте метода `Add`.

Открытые ограничения типа также можно использовать в определениях универсальных классов. Применение открытых ограничений типа с универсальными классами ограничено, поскольку в отношении таких ограничений компилятор может предполагать только то, что они являются производными от <xref:System.Object>. Открытые ограничения типа следует использовать в универсальных классах в тех случаях, когда необходимо обеспечить отношение наследования между двумя параметрами типа.

```cpp
// generics_constraints_6.cpp
// compile with: /clr /c
generic <class T>
ref struct List {
   generic <class U>
   where U : T
   void Add(List<U> items)  {}
};

generic <class A, class B, class C>
where A : C
ref struct SampleClass {};
```

## <a name="see-also"></a>См. также

[Универсальные шаблоны](generics-cpp-component-extensions.md)
