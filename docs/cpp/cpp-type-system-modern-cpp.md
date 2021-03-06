---
title: Система типов C++
ms.date: 11/19/2019
ms.topic: conceptual
ms.assetid: 553c0ed6-77c4-43e9-87b1-c903eec53e80
ms.openlocfilehash: b49dfccc7f815bb13a23f4a334066fa5a8ba5c00
ms.sourcegitcommit: f2a135d69a2a8ef1777da60c53d58fe06980c997
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/03/2020
ms.locfileid: "87521217"
---
# <a name="c-type-system"></a>Система типов C++

Концепция *типа* очень важна в C++. Каждая переменная, аргумент функции и возвращаемое значение функции должны иметь тип, чтобы их можно было скомпилировать. Кроме того, перед вычислением каждого выражения (включая литеральные значения) компилятор неявно назначает ему тип. Некоторые примеры типов включают **`int`** в себя хранение целочисленных значений, **`double`** Хранение значений с плавающей запятой (также известных как *скалярные* типы данных) или класс стандартной библиотеки [std:: basic_string](../standard-library/basic-string-class.md) для хранения текста. Вы можете создать собственный тип, определив **`class`** или **`struct`** . Тип определяет объем памяти, выделяемой для переменной (или результата выражения), типы значений, которые могут храниться в этой переменной, способ интерпретации значений (например, битовые шаблоны), и операции, допустимые с переменной. Эта статья содержит неформальный обзор основных особенностей системы типов C++.

## <a name="terminology"></a>Терминология

**Переменная**: символическое имя количества данных, чтобы имя можно было использовать для доступа к данным, на которые он ссылается в области кода, где он определен. В C++ *переменная* обычно используется для ссылки на экземпляры скалярных типов данных, тогда как экземпляры других типов обычно называются *объектами*.

**Объект**. для простоты и согласованности в этой статье используется *объект* term для ссылки на любой экземпляр класса или структуры, и когда он используется в общем смысле, включает все типы, даже скалярные переменные.

**Тип POD** (обычные старые данные): Эта неофициальная Категория типов данных в C++ относится к скалярным типам (см. раздел фундаментальные типы) или к *классам Pod*. Класс POD не содержит статических данных-членов, которые не являются типами POD, а также не содержит пользовательских конструкторов, пользовательских деструкторов или пользовательских операторов присваивания. Кроме того, класс POD не имеет виртуальных функций, базового класса и ни закрытых, ни защищенных нестатических данных-членов. Типы POD часто используются для внешнего обмена данными, например с модулем, написанным на языке С (в котором имеются только типы POD).

## <a name="specifying-variable-and-function-types"></a>Указание типов переменных и функций

C++ — это *строго типизированный* язык, который также является *статически типизированным*; Каждый объект имеет тип, и этот тип никогда не изменяется (не следует путать с статическими объектами данных). При объявлении переменной в коде необходимо либо явно указать ее тип, либо использовать **`auto`** ключевое слово, чтобы указать компилятору вывести тип из инициализатора. При объявлении функции в коде необходимо указать тип каждого аргумента и его возвращаемое значение или **`void`** значение, если функция не возвращает никакого значения. Исключением является использование шаблонов функции, которые допускают аргументы произвольных типов.

После объявления переменной изменить ее тип впоследствии уже невозможно. Однако можно скопировать значения переменной или возвращаемое значение функции в другую переменную другого типа. Такие операции называются *преобразованиями типов*, которые иногда являются обязательными, но также являются потенциальными источниками потери или неправильности данных.

При объявлении переменной типа POD настоятельно рекомендуется инициализировать ее, т. е. указать начальное значение. Пока переменная не инициализирована, она имеет "мусорное" значение, определяемое значениями битов, которые ранее были установлены в этом месте памяти. Необходимо учитывать эту особенность языка C++, особенно при переходе с другого языка, который обрабатывает инициализацию автоматически. При объявлении переменной типа, не являющегося классом POD, инициализация обрабатывается конструктором.

В следующем примере показано несколько простых объявлений переменных с небольшим описанием для каждого объявления. В примере также показано, как компилятор использует сведения о типе, чтобы разрешить или запретить некоторые последующие операции с переменной.

```cpp
int result = 0;              // Declare and initialize an integer.
double coefficient = 10.8;   // Declare and initialize a floating
                             // point value.
auto name = "Lady G.";       // Declare a variable and let compiler
                             // deduce the type.
auto address;                // error. Compiler cannot deduce a type
                             // without an intializing value.
age = 12;                    // error. Variable declaration must
                             // specify a type or use auto!
result = "Kenny G.";         // error. Can’t assign text to an int.
string result = "zero";      // error. Can’t redefine a variable with
                             // new type.
int maxValue;                // Not recommended! maxValue contains
                             // garbage bits until it is initialized.
```

## <a name="fundamental-built-in-types"></a>Базовые (встроенные) типы

В отличие от некоторых других языков, в C++ нет универсального базового типа, от которого наследуются все остальные типы. Язык включает множество *фундаментальных типов*, известных также как *встроенные типы*. К ним относятся такие числовые типы **`int`** , как, **`double`** ,, **`long`** **`bool`** , а также **`char`** **`wchar_t`** типы и для символов ASCII и Юникода соответственно. Большинство интегральных фундаментальных типов (за исключением **`bool`** **`double`** **`wchar_t`** типов, и связанных) имеют **`unsigned`** версии, которые изменяют диапазон значений, которые может хранить переменная. Например, объект **`int`** , хранящий 32-разрядное целое число со знаком, может представлять значение от-2 147 483 648 до 2 147 483 647. Объект **`unsigned int`** , который также хранится как 32-бит, может хранить значение от 0 до 4 294 967 295. Общее количество возможных значений в каждом случае одинаково, отличается только диапазон.

Базовые типы распознаются компилятором, в котором предусмотрены встроенные правила, управляющие операциями, выполняемыми с такими типами, а также преобразованием в другие базовые типы. Полный список встроенных типов, а также их размер и числовые ограничения см. [в разделе Встроенные типы](../cpp/fundamental-types-cpp.md).

На следующем рисунке показаны относительные размеры встроенных типов в реализации Microsoft C++:

![Размер в байтах построенного&#45;в типах](../cpp/media/built-intypesizes.png "Размер в байтах построенного&#45;в типах")

В следующей таблице перечислены наиболее часто используемые фундаментальные типы и их размеры в реализации Microsoft C++:

| Тип | Размер | Комментировать |
|--|--|--|
| **`int`** | 4 байта | Выбор по умолчанию для целочисленных значений. |
| **`double`** | 8 байт | Выбор по умолчанию для значений с плавающей запятой. |
| **`bool`** | 1 байт | Представляет значения, которые могут быть или true, или false. |
| **`char`** | 1 байт | Используйте для символов ASCII в старых строках в стиле C или в объектах std::string, которые никогда не будут преобразовываться в Юникод. |
| **`wchar_t`** | 2 байта | Представляет "расширенные" символы, которые могут быть представлены в формате Юникод (UTF-16 в Windows, в других операционных системах возможно другое представление). Это символьный тип, используемый в строках типа `std::wstring`. |
| **`unsigned char`** | 1 байт | C++ не имеет встроенного типа Byte.  Используется **`unsigned char`** для представления байтового значения. |
| **`unsigned int`** | 4 байта | Вариант по умолчанию для битовых флагов. |
| **`long long`** | 8 байт | Представляет очень большие целочисленные значения. |

Другие реализации C++ могут использовать разные размеры для определенных числовых типов. Дополнительные сведения о размерах и отношениях размеров, необходимых стандарту C++, см. [в разделе Встроенные типы](fundamental-types-cpp.md).

## <a name="the-void-type"></a>Тип void

**`void`** Тип является специальным типом; нельзя объявить переменную типа **`void`** , но можно объявить переменную типа `void *` (указатель на **`void`** ), которая иногда необходима при выделении необработанной памяти (нетипизированной). Однако указатели на **`void`** не являются строго типизированными и обычно их использование не рекомендуется в современных C++. В объявлении функции **`void`** возвращаемое значение означает, что функция не возвращает значение. это общее и допустимое использование **`void`** . Хотя в языке C обязательны функции, которые имеют нулевые параметры для объявления **`void`** в списке параметров, `fou(void)` такой подход не рекомендуется в современных C++ и должен быть объявлен `fou()` . Дополнительные сведения см. в разделе [преобразования типов и типизация](../cpp/type-conversions-and-type-safety-modern-cpp.md).

## <a name="const-type-qualifier"></a>Квалификатор типа const

Любой встроенный или пользовательский тип может квалифицироваться ключевым словом const. Кроме того, функции-члены могут быть **`const`** полными и даже **`const`** перегруженными. Значение **`const`** типа не может быть изменено после инициализации.

```cpp

const double PI = 3.1415;
PI = .75 //Error. Cannot modify const variable.
```

**`const`** Квалификатор широко используется в объявлениях функций и переменных, и "правильность констант" является важной концепцией в C++; по сути это означает **`const`** , что во время компиляции эти значения не изменяются случайным образом. Дополнительные сведения см. в статье [`const`](../cpp/const-cpp.md).

**`const`** Тип отличается от неконстантной версии, например, имеет тип, отличный **`const int`** от **`int`** . Оператор C++ можно использовать **`const_cast`** в редких случаях, когда необходимо удалить *const-rvalue характеристики* из переменной. Дополнительные сведения см. в разделе [преобразования типов и типизация](../cpp/type-conversions-and-type-safety-modern-cpp.md).

## <a name="string-types"></a>Строковые типы

Строго говоря, язык C++ не имеет встроенного строкового типа; **`char`** и **`wchar_t`** хранения одиночных символов. необходимо объявить массив этих типов для приблизительной строки, добавив завершающее значение null (например, ASCII `'\0'` ) к элементу массива, который находится за последним допустимым символом (также называется *строкой в стиле C*). Строки в стиле C требовали написания гораздо большего объема кода или использования внешних библиотек служебных функций. Но в современных C++ у нас есть стандартные библиотеки типов `std::string` (для символьных строк 8-разрядных **`char`** типов) или `std::wstring` (для строк символов 16-разрядного **`wchar_t`** типа). Эти контейнеры стандартной библиотеки C++ можно рассматривать как собственные строковые типы, так как они являются частью стандартных библиотек, включенных в совместимую среду сборки C++. Просто используйте директиву `#include <string>`, чтобы эти типы были доступны в программе. (Если используется MFC или ATL, `CString` класс также доступен, но не является частью стандарта C++.) Использование массивов символов, заканчивающихся нулем (приведенных выше строк в стиле C), не рекомендуется в современных C++.

## <a name="user-defined-types"></a>Определяемые пользователем типы

При определении,, **`class`** **`struct`** **`union`** или **`enum`** , эта конструкция используется в остальной части кода так, как если бы это был фундаментальный тип. Он имеет известный размер в памяти, и в его отношении действуют определенные правила проверки во время компиляции и во время выполнения в течение срока использования программы. Основные различия между базовыми встроенными типами и пользовательскими типами указаны ниже:

- Компилятор не имеет встроенных сведений о пользовательском типе. Он узнает о типе при первом обнаружении определения во время процесса компиляции.

- Пользователь определяет, какие операции можно выполнять с типом и как его можно преобразовать в другие типы, задавая (через перегрузку) соответствующие операторы, либо в виде членов класса, либо в виде функций, не являющихся членами. Дополнительные сведения см. в разделе [перегрузка функций](function-overloading.md) .

## <a name="pointer-types"></a>типы указателей

Датировано назад к самой ранней версии языка C, C++ позволяет объявить переменную типа указателя с помощью специального декларатора **`*`** (звездочка). Тип указателя хранит адрес расположения в памяти, в котором хранится фактическое значение данных. В современных C++ они называются *необработанными указателями*и доступны в коде с помощью специальных операторов **`*`** (звездочки) или **`->`** (тире с символом «больше»). Это называется *разыменованием*, и какой из используемых объектов зависит от того, выполняется ли разыменование указателя на скаляр или указатель на член в объекте. Работа с типами указателя долгое время была одним из наиболее трудных и непонятных аспектов разработки программ на языках C и C++. В этом разделе приводятся некоторые факты и рекомендации по использованию необработанных указателей, если вы хотите, но в современной версии C++ больше не требуется (или рекомендуется) использовать необработанные указатели для владения объектами, так как при развитии [интеллектуального указателя](../cpp/smart-pointers-modern-cpp.md) (см. Дополнительные сведения в конце этого раздела). Все еще полезно и безопасно использовать необработанные указатели для отслеживания объектов, но если требуется использовать их для владения объектом, необходимо делать это с осторожностью и после тщательного анализа процедуры создания и уничтожения объектов, которые им принадлежат.

Первое, что необходимо знать, — это то, что при объявлении переменной необработанного указателя выделяется только память, необходимая для хранения адреса расположения памяти, на который будет ссылаться указатель при разыменовывании. Выделение памяти для самого значения данных (также называемое *резервным хранилищем*) еще не выделено. Другими словами, объявив переменную необработанного указателя, вы создаете переменную адреса памяти, а не фактическую переменную данных. Разыменовывание переменной указателя до проверки того, что она содержит действительный адрес в резервном хранилище, приведет к неопределенному поведению (обычно неустранимой ошибке) программы. В следующем примере демонстрируется подобная ошибка:

```cpp
int* pNumber;       // Declare a pointer-to-int variable.
*pNumber = 10;      // error. Although this may compile, it is
                    // a serious error. We are dereferencing an
                    // uninitialized pointer variable with no
                    // allocated memory to point to.
```

Пример разыменовывает тип указателя без выделения памяти для хранения фактических целочисленных данных или без выделенного допустимого адреса памяти. В следующем коде исправлены эти ошибки:

```cpp
    int number = 10;          // Declare and initialize a local integer
                              // variable for data backing store.
    int* pNumber = &number;   // Declare and initialize a local integer
                              // pointer variable to a valid memory
                              // address to that backing store.
...
    *pNumber = 41;            // Dereference and store a new value in
                              // the memory pointed to by
                              // pNumber, the integer variable called
                              // "number". Note "number" was changed, not
                              // "pNumber".
```

В исправленном примере кода используется локальной память стека для создания резервного хранилища, на который указывает указатель `pNumber`. Базовый тип используется для простоты. На практике резервное хранилище для указателей — это наиболее часто определяемые пользователем типы, динамически выделяемые в области памяти, называемой *кучей* (или *свободным хранилищем*) с помощью **`new`** ключевого выражения (в программировании в стиле c `malloc()` использовалась старая функция библиотеки времени выполнения c). После выделения эти переменные обычно называются объектами, особенно если они основаны на определении класса. Память, выделенная с помощью, **`new`** должна быть удалена соответствующим **`delete`** оператором (или, если вы использовали `malloc()` функцию для ее выделения, функция времени выполнения C `free()` ).

Однако можно легко забыть удалить динамически выделенный объект, особенно в сложном коде, который вызывает ошибку ресурса, называемую *утечкой памяти*. По этой причине в современном С++ настоятельно не рекомендуется использовать необработанные указатели. Почти всегда лучше обернуть необработанный указатель в [Интеллектуальный указатель](../cpp/smart-pointers-modern-cpp.md), который автоматически освобождает память при вызове его деструктора (когда код выходит за пределы области для смарт-указателя); с помощью смарт-указателей вы практически устраняете целый класс ошибок в программах на C++. В следующем примере предположим, что `MyClass` — это пользовательский тип, который имеет открытый метод `DoSomeWork();`

```cpp
void someFunction() {
    unique_ptr<MyClass> pMc(new MyClass);
    pMc->DoSomeWork();
}
  // No memory leak. Out-of-scope automatically calls the destructor
  // for the unique_ptr, freeing the resource.
```

Дополнительные сведения о смарт-указателях см. в разделе [интеллектуальные указатели](../cpp/smart-pointers-modern-cpp.md).

Дополнительные сведения о преобразовании указателей см. в разделе [преобразования типов и типизация](../cpp/type-conversions-and-type-safety-modern-cpp.md).

Дополнительные сведения об указателях в целом см. в разделе [указатели](../cpp/pointers-cpp.md).

## <a name="windows-data-types"></a>Типы данных Windows

В классическом программировании Win32 для C и C++ большинство функций используют определяемые Windows определяемые пользователем `#define` типы и макросы (определенные в `windef.h` ) для указания типов параметров и возвращаемых значений. Эти типы данных Windows обычно представляют собой специальные имена (псевдонимы), предоставленные встроенным типам C/C++. Полный список определений типов и определения препроцессора см. в разделе [типы данных Windows](/windows/win32/WinProg/windows-data-types). Некоторые из этих определений типов, такие как `HRESULT` и `LCID` , являются полезными и описательными. Другие, например `INT` , не имеют специального смысла и являются просто псевдонимами для основных типов C++. Прочие типы данных Windows имеют имена, которые сохранились с эпохи программирования на языке C для 16-разрядных процессоров, и не имеют смысла или значения на современном оборудовании или в современных операционных системах. Существуют также специальные типы данных, связанные с библиотекой среда выполнения Windows, которые перечислены как [Среда выполнения Windows базовых типов данных](/windows/win32/WinRT/base-data-types). В современном языке C++ в целом рекомендуется использовать базовые типы C++, за исключением случаев, когда тип Windows сообщает некоторые дополнительные сведения о том, как следует интерпретировать значение.

## <a name="more-information"></a>Дополнительные сведения

Дополнительные сведения о системе типов C++ см. в следующих разделах.

[Типы значений](../cpp/value-types-modern-cpp.md)\
Описывает *типы значений* , а также проблемы, связанные с их использованием.

[Преобразования типов и безопасность типов](../cpp/type-conversions-and-type-safety-modern-cpp.md)\
Описание типовых проблем преобразования типов и способов их избежать.

## <a name="see-also"></a>См. также

[Добро пожаловать в C++](../cpp/welcome-back-to-cpp-modern-cpp.md)<br/>
[Справочник по языку C++](../cpp/cpp-language-reference.md)<br/>
[Стандартная библиотека C++](../standard-library/cpp-standard-library-reference.md)
