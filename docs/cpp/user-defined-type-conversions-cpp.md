---
title: Заданные пользователем преобразования типов (C++)
ms.date: 11/04/2016
f1_keywords:
- explicit_cpp
helpviewer_keywords:
- constructors [C++], and constants
- conversion functions [C++]
- explicit keyword [C++]
- type conversion
- constructors [C++], drawbacks
- conversion constructors
- type conversion [C++], explicit conversion
- coercion [C++]
- conversions [C++], explicit
- objects [C++], converting
- conversion functions [C++], rules for declaring
- declaring functions [C++], conversion functions
- functions [C++], conversion
- converting objects
- constructors [C++], conversion
- conversions [C++], by constructors
- data type conversion [C++], explicit
ms.assetid: d40e4310-a190-4e95-a34c-22c5c20aa0b9
ms.openlocfilehash: e7889a7365a6b3a362804d3dad4b2fefc3780d01
ms.sourcegitcommit: 1f009ab0f2cc4a177f2d1353d5a38f164612bdb1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/27/2020
ms.locfileid: "87227040"
---
# <a name="user-defined-type-conversions-c"></a>Заданные пользователем преобразования типов (C++)

*Преобразование* создает новое значение некоторого типа из значения другого типа. *Стандартные преобразования* встроены в язык C++ и поддерживают встроенные типы. Кроме того, можно создавать *пользовательские преобразования* для выполнения преобразований в типы, из или между определяемыми пользователем типами.

Стандартные преобразования выполняют преобразование между встроенными типами, между указателями или ссылками на типы, связанные наследованием, в и из указателей void и в пустой указатель. Дополнительные сведения см. в разделе [стандартные преобразования](../cpp/standard-conversions.md). Пользовательские преобразования выполняют преобразование между пользовательскими типами или между пользовательскими и встроенными типами. Их можно реализовать как [конструкторы преобразования](#ConvCTOR) или как [функции преобразования](#ConvFunc).

Преобразования могут быть явными, когда программист вызывает преобразование одного типа в другой (как в приведении или прямой инициализации) или неявными, когда язык или программа вызывают типы, которые отличаются от заданных программистом.

Попытка неявного преобразования выполняется, когда

- тип аргумента, предоставленного для функции, не совпадает с соответствующим параметром;

- тип значения, возвращаемого функцией, не совпадает с типом возвращаемого значения функции;

- тип выражения инициализатора не совпадает с типом инициализируемого объекта;

- тип результата выражения, которое управляет условным оператором, циклической конструкцией или параметром, не совпадает с тем, который требуется для управления;

- тип операнда, предоставленного для оператора, не совпадает с соответствующим параметром операнда. Для встроенных операторов тип обоих операндов должен совпадать; он преобразуется в общий тип, который может представлять оба операнда. Дополнительные сведения см. в разделе [стандартные преобразования](standard-conversions.md). Для пользовательских операторов тип каждого операнда должен совпадать с соответствующим параметром операнда.

Если не удается выполнить неявное преобразование с помощью стандартного преобразования, компилятор может использовать пользовательское преобразование, за которым (при необходимости) будет следовать дополнительное стандартное преобразование.

Если на сайте преобразования есть два и более пользовательских преобразования, выполняющих одно преобразование, преобразование называется неоднозначным. Неоднозначность подразумевает ошибку, так как компилятор не может определить, какое из доступных преобразований выбрать. Тем не менее, не будет ошибкой определить несколько способов выполнения одного преобразования, так как набор доступных преобразований может отличаться в разных участках исходного кода, например в зависимости от того, какие файлы заголовков входят в исходный файл. Пока на сайте преобразования доступно только одно преобразование, о неоднозначности речь не идет. Существует несколько путей возникновения неоднозначных преобразований, однако самые распространенные перечислены ниже.

- Множественное наследование. Преобразование определено в нескольких базовых классах.

- Вызов неоднозначной функции. Преобразование определено как конструктор преобразования типа целевого объекта и как функция преобразования типа источника. Дополнительные сведения см. в разделе [функции преобразования](#ConvFunc).

Неоднозначность, как правило, можно устранить, просто более полно указав имя соответствующего типа или выполнив явное приведение для пояснения намерения.

Конструкторы преобразования и функции преобразования подчиняются правилам управления доступом членов, однако доступность преобразований учитывается, только если можно определить неоднозначное преобразование. Это означает, что преобразование может быть неоднозначным, даже если уровень доступа конкурирующего преобразования будет блокировать его использование. Дополнительные сведения о специальных возможностях членов см. в разделе [Управление доступом к членам](../cpp/member-access-control-cpp.md).

## <a name="the-explicit-keyword-and-problems-with-implicit-conversion"></a>Ключевое слово explicit и проблемы с неявным преобразованием

По умолчанию при создании пользовательского преобразования компилятор может использовать его для выполнения неявных преобразований. Иногда это совпадает с вашими намерениями, но в других случаях простые правила, которые определяют выполнение неявных преобразований компилятором, могут привести к тому, что он примет нежелательный код.

Один известный пример неявного преобразования, который может вызвать проблемы, — преобразование в **`bool`** . Существует множество причин, по которым может потребоваться создать тип класса, который может использоваться в логическом контексте, например, чтобы его можно было использовать для управления **`if`** оператором или циклом, но когда компилятор выполняет определенное пользователем преобразование в встроенный тип, компилятор может применить дополнительное стандартное преобразование впоследствии. Назначение этого дополнительного стандартного преобразования заключается в том, чтобы сделать возможным продвижение от **`short`** до **`int`** , но оно также открывает дверцу для менее очевидных преобразований, например из **`bool`** в **`int`** , что позволяет использовать тип класса в целочисленных контекстах, которые никогда не предполагались. Эта конкретная проблема известна как *проблема безопасного логического*типа. Проблема такого рода заключается в том, что **`explicit`** ключевое слово может помочь.

**`explicit`** Ключевое слово сообщает компилятору, что указанное преобразование нельзя использовать для выполнения неявных преобразований. Если вы хотите использовать синтаксис неявных преобразований до того **`explicit`** , как было введено ключевое слово, пришлось либо принять непредвиденные последствия неявного преобразования, либо использовать в качестве обходного пути менее удобные функции именованного преобразования. Теперь с помощью **`explicit`** ключевого слова можно создавать удобные преобразования, которые можно использовать только для выполнения явных приведений или прямой инициализации, что не приводит к возникновению проблем, содержащиеся с проблемой безопасного логического типа.

**`explicit`** Ключевое слово можно применять к конструкторам преобразования с c++ 98, а также к функциям преобразования, начиная с c++ 11. В следующих разделах содержатся дополнительные сведения об использовании **`explicit`** ключевого слова.

## <a name="conversion-constructors"></a><a name="ConvCTOR"></a>Конструкторы преобразования

Конструкторы преобразования определяют преобразование из пользовательских или встроенных типов в пользовательские типы. В следующем примере демонстрируется конструктор преобразования, который преобразует из встроенного типа **`double`** в определяемый пользователем тип `Money` .

```cpp
#include <iostream>

class Money
{
public:
    Money() : amount{ 0.0 } {};
    Money(double _amount) : amount{ _amount } {};

    double amount;
};

void display_balance(const Money balance)
{
    std::cout << "The balance is: " << balance.amount << std::endl;
}

int main(int argc, char* argv[])
{
    Money payable{ 79.99 };

    display_balance(payable);
    display_balance(49.95);
    display_balance(9.99f);

    return 0;
}
```

Обратите внимание, что первый вызов функции `display_balance`, которая принимает аргументы типа `Money`, не требует преобразования, так как аргумент принадлежит к правильному типу. Однако при втором вызове требуется `display_balance` Преобразование, так как тип аргумента, **`double`** со значением `49.95` , не является функцией, которую требует функция. Функция не может использовать это значение напрямую, но из-за преобразования из типа аргумента — **`double`** в тип соответствующего параметра — `Money` — временное значение типа `Money` формируется из аргумента и используется для завершения вызова функции. При третьем вызове `display_balance` функции Обратите внимание, что аргумент не является **`double`** , но вместо него имеет **`float`** значение `9.99` – и, однако, функция может быть завершена, так как компилятор может выполнить стандартное преобразование — в данном случае —, **`float`** **`double`** а затем выполнить пользовательское преобразование из **`double`** в для `Money` завершения необходимого преобразования.

### <a name="declaring-conversion-constructors"></a>Объявление конструкторов преобразования

Следующие правила применяются к объявлению конструктора преобразования.

- Целевым типом преобразования является сконструированный пользовательский тип.

- Конструкторы преобразований, как правило, принимают только один аргумент типа источника. Однако конструктор преобразования может указывать дополнительные параметры, если у каждого из них есть значение по умолчанию. Тип источника остается типом первого параметра.

- Конструкторы преобразований, как и все конструкторы, не указывают тип возвращаемого значения. Указание типа возвращаемого значения в объявлении является ошибкой.

- Конструкторы преобразования могут быть явными.

### <a name="explicit-conversion-constructors"></a>Явные конструкторы преобразования

Объявляя конструктор преобразования как **`explicit`** , его можно использовать только для выполнения непосредственной инициализации объекта или для выполнения явного приведения. Это не дает функциям, которые принимают аргумент типа класса, также неявно принимать аргументы типа источника конструктора преобразования, а также блокирует инициализацию копирования типа класса из значения типа источника. В следующем примере демонстрируется определение явного конструктора преобразования и влияние на правильный синтаксис кода.

```cpp
#include <iostream>

class Money
{
public:
    Money() : amount{ 0.0 } {};
    explicit Money(double _amount) : amount{ _amount } {};

    double amount;
};

void display_balance(const Money balance)
{
    std::cout << "The balance is: " << balance.amount << std::endl;
}

int main(int argc, char* argv[])
{
    Money payable{ 79.99 };        // Legal: direct initialization is explicit.

    display_balance(payable);      // Legal: no conversion required
    display_balance(49.95);        // Error: no suitable conversion exists to convert from double to Money.
    display_balance((Money)9.99f); // Legal: explicit cast to Money

    return 0;
}
```

В этом примере обратите внимание, что явный конструктор преобразования можно использовать для выполнения прямой инициализации типа `payable`. Если же вы попытаетесь выполнить инициализацию копирования `Money payable = 79.99;`, это приведет к ошибке. Первый вызов `display_balance` не включает преобразование, так как указан аргумент правильного типа. Второй вызов `display_balance` является ошибкой, так как конструктор преобразования нельзя использовать для выполнения неявного преобразования. Третий вызов `display_balance` является допустимым из-за явного приведения к `Money` , но обратите внимание, что компилятор по-прежнему помог выполнить приведение путем вставки неявного приведения **`float`** в **`double`** .

Несмотря на то, что использование неявных преобразований кажется удобным, в результате могут возникать трудновыявляемые ошибки. Как показывает опыт, лучше всего объявлять все конструкторы преобразований явными за исключением тех случаев, когда необходимо, чтобы определенное преобразование выполнялось неявно.

## <a name="conversion-functions"></a><a name="ConvFunc"></a>Функции преобразования

Функции преобразования определяют преобразования из пользовательского в другие типы. Эти функции иногда называют "операторами приведения", так как они, наряду с конструкторами преобразования, вызываются, когда значение приводится к другому типу. В следующем примере демонстрируется функция преобразования, преобразующая из определяемого пользователем типа, `Money` в встроенный тип **`double`** :

```cpp
#include <iostream>

class Money
{
public:
    Money() : amount{ 0.0 } {};
    Money(double _amount) : amount{ _amount } {};

    operator double() const { return amount; }
private:
    double amount;
};

void display_balance(const Money balance)
{
    std::cout << "The balance is: " << balance << std::endl;
}
```

Обратите внимание, что переменная `amount` -член сделана закрытой, а открытая функция преобразования в тип **`double`** введена только для возврата значения `amount` . В функции `display_balance` неявное преобразование возникает, когда значение `balance` направляется в стандартный вывод с помощью оператора вставки в поток `<<`. Так как для определяемого пользователем типа не определен оператор вставки потока, `Money` но имеется один для встроенного типа **`double`** , компилятор может использовать функцию преобразования из `Money` в **`double`** для удовлетворения оператора вставки потока.

Функции преобразования наследуются производными классами. Функции преобразования в производном классе переопределяют наследуемую функцию преобразования, только когда выполняют преобразование в точно такой же тип. Например, определяемая пользователем функция преобразования оператора производного класса **int** не переопределяет (или даже не влияет) на определяемую пользователем функцию преобразования **оператора**базового класса Short, даже если стандартные преобразования определяют отношение преобразования между **`int`** и **`short`** .

### <a name="declaring-conversion-functions"></a>Объявление функций преобразования

Следующие правила применяются к объявлению функции преобразования.

- Целевой тип преобразования должен быть объявлен до объявления функции преобразования. Классы, структуры, перечисления и определения типа нельзя объявлять в объявлении функции преобразования.

    ```cpp
    operator struct String { char string_storage; }() // illegal
    ```

- Функции преобразования не принимают аргументов. Указание любых параметров в объявлении является ошибкой.

- Функции преобразования имеют тип возвращаемого значения, задаваемый именем функции преобразования, которое также является именем типа целевого объекта преобразования. Указание типа возвращаемого значения в объявлении является ошибкой.

- Функции преобразования могут быть виртуальными.

- Функции преобразования могут быть явными.

### <a name="explicit-conversion-functions"></a>Явные функции преобразования

Если функция преобразования объявлена как явная, ее можно использовать только для выполнения явного приведения. Это не дает функциям, которые принимают аргумент типа целевого объекта функции преобразования, также неявно принимать аргументы типа класса, а также блокирует инициализацию копирования экземпляров типа целевого объекта из значения типа класса. В следующем примере демонстрируется определение явной функции преобразования и влияние на правильный синтаксис кода.

```cpp
#include <iostream>

class Money
{
public:
    Money() : amount{ 0.0 } {};
    Money(double _amount) : amount{ _amount } {};

    explicit operator double() const { return amount; }
private:
    double amount;
};

void display_balance(const Money balance)
{
    std::cout << "The balance is: " << (double)balance << std::endl;
}
```

Здесь оператор функции преобразования **Double** был явно объявлен, а явное приведение к типу **`double`** было представлено в функции `display_balance` для выполнения преобразования. Если пропустить это преобразование, компилятор не сможет найти подходящий оператор вставки в поток `<<` для типа `Money` и может возникнуть ошибка.
