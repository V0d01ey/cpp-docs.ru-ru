---
title: '#Директива define (C/C++)'
ms.date: 08/29/2019
f1_keywords:
- '#define'
helpviewer_keywords:
- define directive (#define), syntax
- preprocessor, directives
- define directive (#define)
- '#define directive, syntax'
- '#define directive'
ms.assetid: 33cf25c6-b24e-40bf-ab30-9008f0391710
ms.openlocfilehash: e9e5b7a02ee55c05aa44278fbceb9c42f372c443
ms.sourcegitcommit: a1676bf6caae05ecd698f26ed80c08828722b237
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91506666"
---
# <a name="define-directive-cc"></a>Директива #define (C/C++)

**#Define** создает *макрос*, который представляет собой ассоциацию идентификатора или параметризованного идентификатора со строкой токена. После определения макроса компилятор может подставить строку токена для каждого обнаруженного идентификатора в исходном файле.

## <a name="syntax"></a>Синтаксис

> **#define** *identifier* *токена идентификатора — строка*<sub>OPT</sub>\
> **идентификатор #define** *identifier* **(** *идентификатор*<sub>OPT</sub>**,** ... **,** *идентификатор*<sub>OPT</sub> **)** *— строка с*<sub>неявной</sub> строкой

## <a name="remarks"></a>Remarks

Директива **#define** заставляет компилятор заменить *строку токена* для каждого вхождения *идентификатора* в исходном файле. *Идентификатор* заменяется только в том случае, если он формирует маркер. Это значит, что *идентификатор* не заменяется, если он присутствует в комментарии, в строке или в составе более длинного идентификатора. Дополнительные сведения см. в разделе [токены](../cpp/character-sets.md).

Аргумент *строки токена* состоит из ряда токенов, таких как ключевые слова, константы или полные инструкции. Один или несколько пробельных символов должны отделять *строку токена* от *идентификатора*. Эти пробелы не считаются частью замененного текста, как и все остальные пробелы, следующие за последним токеном текста.

A `#define` без *строки токена* удаляет вхождения *идентификатора* из исходного файла. *Идентификатор* остается определенным и может быть проверен с помощью `#if defined` `#ifdef` директивы и.

Вторая форма синтаксиса определяет макрос, подобный функции, с параметрами. Эта форма допускает использование необязательного списка параметров, которые должны находиться в скобках. После определения макроса каждое последующее вхождение *идентификатора* *(*<sub>неявное</sub>значение,..., *идентификатор*<sub>OPT</sub> ) заменяется версией аргумента *-строки токена* , которая содержит фактические аргументы, подставляемые для формальных параметров.

Формальные имена параметров отображаются в *строке токена* для обозначения мест, в которых заменяются фактические значения. Имя каждого параметра может встречаться несколько раз в *строке токена*, а имена могут отображаться в любом порядке. Число аргументов в вызове должно соответствовать числу параметров в определении макроса. Надлежащее использование скобок обеспечит правильную обработку сложных фактических аргументов.

Формальные параметры в списке разделяются запятыми. Все имена в списке должны быть уникальными, и список должен быть заключен в скобки. Ни один из пробелов не может разделять *идентификатор* и открывающую круглую скобку. Использование сцепления строк — перед символом новой строки разместите обратную косую черту ( `\` ) для длинных директив на нескольких исходных строках. Область действия формального имени параметра расширяется до новой строки, в которой заканчивается *Строка токена*.

Если макрос определен во второй форме синтаксиса, последующие текстовые экземпляры, за которыми находится список аргументов, указывают на вызов макроса. Фактические аргументы, которые следуют за экземпляром *идентификатора* в исходном файле, сопоставляются с соответствующими формальными параметрами в определении макроса. Каждый формальный параметр в *строке токена* , не предшествующий оператору строковый ( `#` ), преобразования ( `#@` ) или Token-Place ( `##` ) или не заканчивающийся `##` оператором, заменяется соответствующим фактическим аргументом. Перед заменой директивой формального параметра все макросы в фактическом аргументе разворачиваются. (Операторы описаны в разделе [Операторы препроцессора](../preprocessor/preprocessor-operators.md).)

Следующие примеры макросов с аргументами иллюстрируют вторую форму синтаксиса **#define** :

```C
// Macro to define cursor lines
#define CURSOR(top, bottom) (((top) << 8) | (bottom))

// Macro to get a random integer with a specified range
#define getrandom(min, max) \
    ((rand()%(int)(((max) + 1)-(min)))+ (min))
```

Аргументы с побочными эффектами иногда приводят к тому, что макросы дают непредвиденные результаты. Данный формальный параметр может присутствовать более одного раза в *строке токена*. Если этот формальный параметр заменяется выражением с побочными эффектами, выражение с такими эффектами может вычисляться несколько раз. (См. примеры в разделе Оператор, посвященный [вставлению токена (# #)](../preprocessor/token-pasting-operator-hash-hash.md).)

Директива `#undef` приводит к тому, что определение препроцессора идентификатора забывается. Дополнительные сведения см. [в директиве #undef](../preprocessor/hash-undef-directive-c-cpp.md) .

Если имя определяемого макроса выполняется в *строке токена* (даже в результате расширения другого макроса), оно не разворачивается.

Вторая **#define** макроса с тем же именем создает предупреждение, если вторая последовательность токенов не совпадает с первой.

**Блок, относящийся только к системам Microsoft**

Если новое определение синтаксически совпадает с исходным, Microsoft C и C++ позволяют переопределить макрос. Другими словами, два определения могут иметь разные имена параметров. Это поведение отличается от ANSI C, которое требует лексического идентичности двух определений.

Например, следующие два макроса идентичны, за исключением имен параметров. ANSI C не допускает такое переопределение, но Microsoft C/C++ компилирует его без ошибок.

```C
#define multiply( f1, f2 ) ( f1 * f2 )
#define multiply( a1, a2 ) ( a1 * a2 )
```

С другой стороны, следующие два макроса неидентичны и приводят к выдаче предупреждения в Microsoft C и C++.

```C
#define multiply( f1, f2 ) ( f1 * f2 )
#define multiply( a1, a2 ) ( b1 * b2 )
```

**Завершение блока, относящегося только к системам Майкрософт**

В этом примере показана директива **#define** :

```C
#define WIDTH       80
#define LENGTH      ( WIDTH + 10 )
```

Первый оператор определяет идентификатор `WIDTH` как целочисленную константу 80, а затем `LENGTH` задается в виде `WIDTH` и целочисленной константы 10. Каждое вхождение `LENGTH` заменяется на (`WIDTH + 10`). В свою очередь, каждое вхождение `WIDTH + 10` заменяется выражением (`80 + 10`). Скобки вокруг `WIDTH + 10` имеют важное значение, поскольку управляют интерпретацией в операторах, например в следующем:

```C
var = LENGTH * 20;
```

После этапа предварительной обработки этот оператор принимает следующий вид:

```C
var = ( 80 + 10 ) * 20;
```

что равно 1800. Без скобок результат будет следующим:

```C
var = 80 + 10 * 20;
```

результатом вычисления которого является 280.

**Блок, относящийся только к системам Microsoft**

Определение макросов и констант с помощью параметра компилятора [/d](../build/reference/d-preprocessor-definitions.md) аналогично использованию директивы предварительной обработки **#define** в начале файла. С помощью параметра /D можно определить до 30 макросов.

**Завершение блока, относящегося только к системам Майкрософт**

## <a name="see-also"></a>См. также

[Директивы препроцессора](../preprocessor/preprocessor-directives.md)
