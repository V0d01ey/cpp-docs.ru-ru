---
title: Функции (C++)
ms.date: 11/19/2018
helpviewer_keywords:
- defaults, arguments
- function definitions
- function definitions, about function definitions
- default arguments
- declarators, functions
ms.assetid: 33ba01d5-75b5-48d2-8eab-5483ac7d2274
ms.openlocfilehash: 5beadbbf283a64f12dab7f0ee39a267ec1797861
ms.sourcegitcommit: 1f009ab0f2cc4a177f2d1353d5a38f164612bdb1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/27/2020
ms.locfileid: "87213441"
---
# <a name="functions-c"></a>Функции (C++)

Функции — это блоки кода, выполняющие определенные операции. Если требуется, функция может определять входные параметры, позволяющие вызывающим объектам передавать ей аргументы. При необходимости функция также может возвращать значение как выходное. Функции полезны для инкапсуляции основных операций в едином блоке, который может многократно использоваться. В идеальном случае имя этого блока должно четко описывать назначение функции. Следующая функция принимает два целых числа от вызывающего объекта и возвращает их сумму; *a* и *b* — это *Параметры* типа **`int`** .

```cpp
int sum(int a, int b)
{
    return a + b;
}
```

Функцию можно вызывать или *вызывать*из любого числа мест в программе. Значения, передаваемые в функцию, являются *аргументами*, типы которых должны быть совместимы с типами параметров в определении функции.

```cpp
int main()
{
    int i = sum(10, 32);
    int j = sum(i, 66);
    cout << "The value of j is" << j << endl; // 108
}
```

Длина функции практически не ограничена, однако для максимальной эффективности кода целесообразно использовать функции, каждая из которых выполняет одиночную, четко определенную задачу. Сложные алгоритмы лучше разбивать на более короткие и простые для понимания функции, если это возможно.

Функции, определенные в области видимости класса, называются функциями-членами. В C++, в отличие от других языков, функции можно также определять в области видимости пространства имен (включая неявное глобальное пространство имен). Такие функции называются *бесплатными функциями* или *функциями, не являющимися членами*. Они широко используются в стандартной библиотеке.

Функции могут быть *перегружены*, а это значит, что разные версии функции могут совместно использовать одно и то же имя, если они отличаются числом и (или) типом формальных параметров. Дополнительные сведения см. в разделе [перегрузка функций](../cpp/function-overloading.md).

## <a name="parts-of-a-function-declaration"></a>Части объявления функции

Минимальное *объявление* функции состоит из возвращаемого типа, имени функции и списка параметров (который может быть пустым) вместе с дополнительными ключевыми словами, которые предоставляют компилятору дополнительные инструкции. В следующем примере показано объявление функции:

```cpp
int sum(int a, int b);
```

Определение функции состоит из объявления, а также *тела*, который является кодом между фигурными скобками:

```cpp
int sum(int a, int b)
{
    return a + b;
}
```

Объявление функции, за которым следует точка с запятой, может многократно встречаться в разных местах кода программы. Оно необходимо перед любыми вызовами этой функции в каждой записи преобразования. По правилу одного определения, определение функции должно фигурировать в коде программы лишь один раз.

При объявлении функции необходимо указать:

1. Возвращаемый тип, указывающий тип значения, возвращаемого функцией, или **`void`** значение, если значения не возвращаются. В C++ 11 **`auto`** является допустимым возвращаемым типом, который указывает компилятору вывести тип из оператора return. В C++ 14 `decltype(auto)` также разрешено. Дополнительные сведения см. в подразделе "Выведение возвращаемых типов" ниже.

1. Имя функции, которое должно начинаться с буквы или символа подчеркивания и не должно содержать пробелов. В стандартной библиотеке со знака подчеркивания обычно начинаются имена закрытых функций-членов или функций, не являющихся членами и не предназначенных для использования в вашем коде.

1. Список параметров, заключенный в скобки. В этом списке через запятую указывается нужное (возможно, нулевое) число параметров, задающих тип и, при необходимости, локальное имя, по которому к значениям можно получить доступ в теле функции.

Необязательные элементы объявления функции:

1. **`constexpr`**, который указывает, что возвращаемое значение функции является константой, которое может быть вычислено во время компиляции.

    ```cpp
    constexpr float exp(float x, int n)
    {
        return n == 0 ? 1 :
            n % 2 == 0 ? exp(x * x, n / 2) :
            exp(x * x, (n - 1) / 2) * x;
    };
    ```

1. Спецификация компоновки **`extern`** или **`static`** .

    ```cpp
    //Declare printf with C linkage.
    extern "C" int printf( const char *fmt, ... );

    ```

   Дополнительные сведения см. в разделе [Преобразование единиц и компоновки](../cpp/program-and-linkage-cpp.md).

1. **`inline`**, который указывает компилятору заменить каждый вызов функции на код самой функции. Подстановка может улучшить эффективность кода в сценариях, где функция выполняется быстро и многократно вызывается во фрагментах, являющихся критическими для производительности программы.

    ```cpp
    inline double Account::GetBalance()
    {
        return balance;
    }
    ```

   Дополнительные сведения см. в разделе [встроенные функции](../cpp/inline-functions-cpp.md).

1. **`noexcept`** Выражение, указывающее, может ли функция вызывать исключение. В следующем примере функция не создает исключение, если `is_pod` выражение принимает значение **`true`** .

    ```cpp
    #include <type_traits>

    template <typename T>
    T copy_object(T& obj) noexcept(std::is_pod<T>) {...}
    ```

   Дополнительные сведения см. на веб-сайте [`noexcept`](../cpp/noexcept-cpp.md).

1. (Только функции-члены) Квалификаторы ОПС, которые указывают, является ли функция **`const`** или **`volatile`** .

1. (Только функции-члены) **`virtual`** , **`override`** или **`final`** . **`virtual`** Указывает, что функцию можно переопределить в производном классе. **`override`** означает, что функция в производном классе переопределяет виртуальную функцию. **`final`** означает, что функция не может быть переопределена в любом последующем производном классе. Дополнительные сведения см. в разделе [виртуальные функции](../cpp/virtual-functions.md).

1. (только функции-члены) **`static`** применение к функции-члену означает, что функция не связана ни с одним экземпляром объекта класса.

1. (Только функции-члены, не являющиеся статическими) Квалификатор ref, указывающий компилятору, какую перегрузку функции следует выбрать, когда неявный параметр объекта ( `*this` ) является ссылкой rvalue или ссылкой lvalue. Дополнительные сведения см. в разделе [перегрузка функций](function-overloading.md#ref-qualifiers).

На следующем рисунке показаны компоненты определения функции. Затененная область является телом функции.

![Части определения функции](../cpp/media/vc38ru1.gif "Части определения функции") <br/>
Части определения функции

## <a name="function-definitions"></a>Определения функций

*Определение функции* состоит из объявления и тела функции, заключенной в фигурные скобки, которые содержат объявления переменных, инструкции и выражения. В следующем примере показано полное определение функции:

```cpp
    int foo(int i, std::string s)
    {
       int value {i};
       MyClass mc;
       if(strcmp(s, "default") != 0)
       {
            value = mc.do_something(i);
       }
       return value;
    }
```

Переменные, объявленные в теле функции, называются локальными. Они исчезают из области видимости при выходе из функции, поэтому функция никогда не должна возвращать ссылку на локальную переменную.

```cpp
    MyClass& boom(int i, std::string s)
    {
       int value {i};
       MyClass mc;
       mc.Initialize(i,s);
       return mc;
    }
```

## <a name="const-and-constexpr-functions"></a>функции const и constexpr

Функцию-член можно объявить как, **`const`** чтобы указать, что функция не может изменять значения каких-либо элементов данных в классе. Объявляя функцию-член как **`const`** , вы помогаете компилятору обеспечить *правильность константы*. Если кто-то по ошибке пытается изменить объект с помощью функции, объявленной как **`const`** , возникает ошибка компилятора. Дополнительные сведения см. в разделе [const](const-cpp.md).

Объявите функцию, как **`constexpr`** если бы получаемое значение могло быть определено во время компиляции. Функция constexpr обычно выполняется быстрее, чем обычная функция. Дополнительные сведения см. на веб-сайте [`constexpr`](constexpr-cpp.md).

## <a name="function-templates"></a>Шаблоны функций

Шаблоны функций подобны шаблонам классов. Их задача заключается в создании конкретных функций на основе аргументов шаблонов. Во многих случаях шаблоны могут определять типы аргументов, поэтому их не требуется явно указывать.

```cpp
template<typename Lhs, typename Rhs>
auto Add2(const Lhs& lhs, const Rhs& rhs)
{
    return lhs + rhs;
}

auto a = Add2(3.13, 2.895); // a is a double
auto b = Add2(string{ "Hello" }, string{ " World" }); // b is a std::string
```

Дополнительные сведения см. в статье [шаблоны функций](../cpp/function-templates.md) .

## <a name="function-parameters-and-arguments"></a>Параметры и аргументы функций

У функции имеется список параметров, в котором через запятую перечислено необходимое (возможно, нулевое) число типов. Каждому параметру присваивается имя, по которому к нему можно получить доступ в теле функции. В шаблоне функции могут указываться дополнительные типы или значения параметров. Вызывающий объект передает аргументы, представляющие собой конкретные значения, типы которых совместимы со списком параметров.

По умолчанию аргументы передаются функции по значению, то есть функция получает копию передаваемого объекта. Копирование крупных объектов может быть ресурсозатратным и неоправданным. Чтобы аргументы передавались по ссылке (в частности в ссылке lvalue), добавьте в параметр квантификатор ссылки.

```cpp
void DoSomething(std::string& input){...}
```

Если функция изменяет аргумент, передаваемый по ссылке, изменяется исходный объект, а не его локальная копия. Чтобы предотвратить изменение такого аргумента функцией, укажите для параметра значение const&:

```cpp
void DoSomething(const std::string& input){...}
```

**C++ 11:**  Чтобы явно отреагировать на аргументы, передаваемые по ссылке rvalue или lvalue-Reference, используйте двойной амперсанд для параметра, чтобы указать универсальную ссылку:

```cpp
void DoSomething(const std::string&& input){...}
```

Функция, объявленная с ключевым словом Single **`void`** в списке объявлений параметров, не принимает аргументов, если ключевое слово **`void`** является первым и единственным членом списка объявлений аргумента. Аргументы типа **`void`** в любом расположении в списке выдают ошибки. Пример:

```cpp

// OK same as GetTickCount()
long GetTickCount( void );
```

Обратите внимание, что, хотя недопустимо указывать **`void`** аргумент, за исключением описанного здесь, типы, производные от типа **`void`** (например, указатели на **`void`** и массивы **`void`** ), могут отображаться в любом месте списка объявлений аргумента.

### <a name="default-arguments"></a>Аргументы по умолчанию

Последним параметрам в сигнатуре функции можно назначить аргумент по умолчанию, т. е. вызывающий объект сможет опустить аргумент при вызове функции, если не требуется указать какое-либо другое значение.

```cpp
int DoSomething(int num,
    string str,
    Allocator& alloc = defaultAllocator)
{ ... }

// OK both parameters are at end
int DoSomethingElse(int num,
    string str = string{ "Working" },
    Allocator& alloc = defaultAllocator)
{ ... }

// C2548: 'DoMore': missing default parameter for parameter 2
int DoMore(int num = 5, // Not a trailing parameter!
    string str,
    Allocator& = defaultAllocator)
{...}
```

Дополнительные сведения см. в разделе [аргументы по умолчанию](../cpp/default-arguments.md).

## <a name="function-return-types"></a>типов возвращаемых функциями значений;

Функция не может возвращать другую функцию или встроенный массив. Однако он может возвращать указатели на эти типы или *лямбда-выражение*, которое создает объект функции. За исключением этих случаев, функция может возвращать значение любого типа в области или не может возвращать значение, в этом случае возвращаемым типом является **`void`** .

### <a name="trailing-return-types"></a>Завершающие возвращаемые типы

"Обычные" возвращаемые типы расположены слева от сигнатуры функции. *Завершающий возвращаемый тип* расположен в правой части сигнатуры и предшествует **`->`** оператору. Завершающие возвращаемые типы особенно полезны в шаблонах функций, когда тип возвращаемого значения зависит от параметров шаблона.

```cpp
template<typename Lhs, typename Rhs>
auto Add(const Lhs& lhs, const Rhs& rhs) -> decltype(lhs + rhs)
{
    return lhs + rhs;
}
```

Если **`auto`** используется в сочетании с завершающим возвращаемым типом, он просто выступает в качестве заполнителя для любого результата выражения decltype и не выполняет выведение типов.

## <a name="function-local-variables"></a>Локальные переменные функции

Переменная, объявленная внутри тела функции, называется *локальной переменной* или просто *локальным*. Нестатические локальные переменные видны только в теле функции. Если локальные переменные объявляются в стеке, они исчезают из области видимости при выходе из функции. Когда вы конструируете локальную переменную и возвращаете ее по значению, компилятор обычно может выполнить *оптимизацию именованного возвращаемого значения* , чтобы избежать ненужных операций копирования. Если локальная переменная возвращается по ссылке, компилятор выдаст предупреждение, поскольку любые попытки вызывающего объекта использовать эту ссылку произойдут после уничтожения локальной переменной.

В C++ локальные переменные можно объявлять как статические. Переменная является видимой только в теле функции, однако для всех экземпляров функции существует только одна копия переменной. Локальные статические объекты удаляются во время завершения, определенного директивой `atexit`. Если статический объект не был создан из-за того, что поток кода программы обошел соответствующее объявление, попытка уничтожения этого объект не предпринимается.

## <a name="type-deduction-in-return-types-c14"></a><a name="type_deduction"></a>Вычисление типов в возвращаемых типах (C++ 14)

В C++ 14 можно использовать, **`auto`** чтобы дать компилятору инструкцию вывести возвращаемый тип из тела функции без необходимости предоставления завершающего возвращаемого типа. Обратите внимание, что всегда выводится **`auto`** на возврат по значению. Используйте `auto&&`, чтобы дать компилятору команду выведения ссылки.

В этом примере **`auto`** будет выведена как неконстантная копия значения суммы LHS и RHS.

```cpp
template<typename Lhs, typename Rhs>
auto Add2(const Lhs& lhs, const Rhs& rhs)
{
    return lhs + rhs; //returns a non-const object by value
}
```

Обратите внимание, что не **`auto`** сохраняет константу-rvalue характеристики типа, который он выводит. Для функций перенаправления, возвращаемое значение которых должно сохранять аргументы const-rvalue характеристики или ref-rvalue характеристики из своих аргументов, можно использовать **`decltype(auto)`** ключевое слово, которое использует **`decltype`** правила вывода типа и сохраняет все сведения о типе. **`decltype(auto)`** может использоваться как обычное возвращаемое значение в левой части или как завершающее возвращаемое значение.

В следующем примере (на основе кода из [N3493](https://wg21.link/n3493)) показано, как **`decltype(auto)`** использовать, чтобы обеспечить точную пересылку аргументов функции в возвращаемый тип, который не известен до создания экземпляра шаблона.

```cpp
template<typename F, typename Tuple = tuple<T...>, int... I>
decltype(auto) apply_(F&& f, Tuple&& args, index_sequence<I...>)
{
    return std::forward<F>(f)(std::get<I>(std::forward<Tuple>(args))...);
}

template<typename F, typename Tuple = tuple<T...>,
    typename Indices = make_index_sequence<tuple_size<Tuple>::value >>
   decltype( auto)
    apply(F&& f, Tuple&& args)
{
    return apply_(std::forward<F>(f), std::forward<Tuple>(args), Indices());
}
```

## <a name="returning-multiple-values-from-a-function"></a><a name="multi_val"></a>Возврат нескольких значений из функции

Существует несколько способов вернуть более одного значения из функции:

1. Инкапсулирует значения в именованном классе или объекте структуры. Требует, чтобы определение класса или структуры было видимым для вызывающего объекта:

    ```cpp
    #include <string>
    #include <iostream>

    using namespace std;

    struct S
    {
        string name;
        int num;
    };

    S g()
    {
        string t{ "hello" };
        int u{ 42 };
        return { t, u };
    }

    int main()
    {
        S s = g();
        cout << s.name << " " << s.num << endl;
        return 0;
    }
    ```

1. Возвращает объект "канал" std:: Tuple или std::p Air:

    ```cpp
    #include <tuple>
    #include <string>
    #include <iostream>

    using namespace std;

    tuple<int, string, double> f()
    {
        int i{ 108 };
        string s{ "Some text" };
        double d{ .01 };
        return { i,s,d };
    }

    int main()
    {
        auto t = f();
        cout << get<0>(t) << " " << get<1>(t) << " " << get<2>(t) << endl;

        // --or--

        int myval;
        string myname;
        double mydecimal;
        tie(myval, myname, mydecimal) = f();
        cout << myval << " " << myname << " " << mydecimal << endl;

        return 0;
    }
    ```

1. **Visual Studio 2017 версии 15,3 и более поздних** версий (доступно в [`/std:c++17`](../build/reference/std-specify-language-standard-version.md) ): используйте структурированные привязки. Преимущество структурированных привязок заключается в том, что переменные, хранящие возвращаемые значения, инициализируются одновременно с объявлением, что в некоторых случаях может быть значительно более эффективным. В операторе `auto[x, y, z] = f();` скобки представляют и инициализируют имена, которые находятся в области действия для всего блока Function.

    ```cpp
    #include <tuple>
    #include <string>
    #include <iostream>

    using namespace std;

    tuple<int, string, double> f()
    {
        int i{ 108 };
        string s{ "Some text" };
        double d{ .01 };
        return { i,s,d };
    }
    struct S
    {
        string name;
        int num;
    };

    S g()
    {
        string t{ "hello" };
        int u{ 42 };
        return { t, u };
    }

    int main()
    {
        auto[x, y, z] = f(); // init from tuple
        cout << x << " " << y << " " << z << endl;

        auto[a, b] = g(); // init from POD struct
        cout << a << " " << b << endl;
        return 0;
    }
    ```

1. Помимо использования возвращаемого значения, можно "возвращать" значения, определив любое количество параметров для использования передачи по ссылке, чтобы функция могла изменять или инициализировать значения объектов, предоставляемых вызывающим объектом. Дополнительные сведения см. в разделе [аргументы функции ссылочного типа](reference-type-function-arguments.md).

## <a name="function-pointers"></a>Указатели функций

Как и в C, в C++ поддерживаются указатели на функции. Однако более типобезопасной альтернативой обычно служит использование объекта-функции.

Рекомендуется **`typedef`** использовать для объявления псевдонима для типа указателя функции при объявлении функции, возвращающей тип указателя функции.  Например.

```cpp
typedef int (*fp)(int);
fp myFunction(char* s); // function returning function pointer
```

Если оно не используется, то правильный синтаксис объявления функции можно вывести из синтаксиса декларатора для указателя на функцию, заменив идентификатор (в приведенном выше примере — `fp`) на имя функции и список аргументов, как показано выше:

```cpp
int (*myFunction(char* s))(int);
```

Предыдущее объявление эквивалентно объявлению, **`typedef`** приведенному выше.

## <a name="see-also"></a>См. также раздел

[Перегрузка функций](../cpp/function-overloading.md)<br/>
[Функции с переменными списками аргументов](../cpp/functions-with-variable-argument-lists-cpp.md)<br/>
[Явно заданные по умолчанию и удаленные функции](../cpp/explicitly-defaulted-and-deleted-functions.md)<br/>
[Подстановка с зависящим от аргументом именем (Поиск Koenig) для функций](../cpp/argument-dependent-name-koenig-lookup-on-functions.md)<br/>
[Аргументы по умолчанию](../cpp/default-arguments.md)<br/>
[Встроенные функции](../cpp/inline-functions-cpp.md)
