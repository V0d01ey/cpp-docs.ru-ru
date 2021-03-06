---
title: Спецификаторы классов хранения для объявлений внешнего уровня
ms.date: 11/04/2016
helpviewer_keywords:
- external definitions
- linkage [C++], external
- external linkage, variable declarations
- declaring variables, external variables
- declarations [C++], external
- declarations [C++], specifiers
- external declarations
- static variables, external declarations
- variables, visibility
- external linkage, storage-class specifiers
- referencing declarations
- visibility, variables
- static storage class specifiers
ms.assetid: b76b623a-80ec-4d5d-859b-6cef422657ee
ms.openlocfilehash: 6c30b8a12c0bf26bc35905872fb6fa527b367ef4
ms.sourcegitcommit: 1f009ab0f2cc4a177f2d1353d5a38f164612bdb1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/27/2020
ms.locfileid: "87229471"
---
# <a name="storage-class-specifiers-for-external-level-declarations"></a>Спецификаторы классов хранения для объявлений внешнего уровня

Внешние переменные — это переменные в области видимости файла. Они определяются вне конкретной функции и потенциально доступны для множества функций. Функции можно определить только на внешнем уровне, и, следовательно, они не могут быть вложенными. По умолчанию все ссылки на внешние переменные и функции с одинаковым именем являются ссылками на один и тот же объект, что означает, что они имеют *внешнее связывание*. (Чтобы переопределить такое поведение, можно использовать ключевое слово **`static`** .)

Объявления переменных на внешнем уровне являются определениями переменных (*определяющие объявления*) или ссылками на переменные, определенные в другом месте (*ссылающиеся объявления*).

Внешнее объявление переменной, которое также инициализирует переменную (неявно или явно), является определяющим объявлением переменной. Определение на внешнем уровне может принимать различные формы.

- Переменная, объявляемая с использованием описателя класса хранения **`static`** . Вы можете явно инициализировать переменную **`static`** с использованием константного выражения, как описано в статье [Инициализация](../c-language/initialization.md). Если опустить инициализатор, переменная инициализируется значением 0 по умолчанию. Например, следующие два оператора считаются определениями переменной `k`.

    ```C
    static int k = 16;
    static int k;
    ```

- Переменная, которая явно инициализируется на внешнем уровне. Например, `int j = 3;` — это определение переменной `j`.

В объявлениях переменных на внешнем уровне (т. е. вне любых функций) можно либо использовать описатель класса хранения **`static`** или **`extern`** , либо полностью опустить его. Вы не можете использовать терминальные описатели *`storage-class-specifier`* **`auto`** и **`register`** на внешнем уровне.

После определения переменной на внешнем уровне она становится видимой во всей оставшейся части записи преобразования. Переменная невидима до ее объявления в том же исходном файле. Кроме того, она будет невидима в других исходных файлах программы, пока объявление не сделает ее видимой, как описано ниже.

Ниже приводятся правила в отношении **`static`** .

- Переменные, объявленные вне всех блоков без ключевого слова **`static`** , всегда сохраняют свои значения в течение всего хода выполнения программы. Чтобы ограничить доступ переменных к определенной записи преобразования, необходимо использовать ключевое слово **`static`** . Это позволяет применить к ним *внутреннее связывание*. Чтобы сделать их глобальными для всей программы, опустите явный класс хранения или укажите ключевое слово **`extern`** (см. правила в следующем списке). Это позволяет применить к ним *внешнее связывание*. Внутренняя и внешняя компоновки также описаны в разделе [Компоновка](../c-language/linkage.md).

- Указать переменную на внешнем уровне можно только один раз в рамках программы. Можно указать другую переменную с тем же именем и описатель класса хранения **`static`** в другой записи преобразования. Так как каждое определение **`static`** является видимым только в пределах собственной записи преобразования, конфликт не произойдет. Это удобный способ скрыть имена идентификаторов, которые должны совместно использоваться функциями в одной единице трансляции, но должны быть невидимы для других таких единиц.

- Описатель класса хранения **`static`** также может применяться к функциям. Если функция объявлена как **`static`** , ее имя невидимо вне файла, в котором она объявлена.

Ниже приведены правила использования **`extern`** .

- Описатель класса хранения **`extern`** объявляет ссылку на переменную, определенную в другом месте. С помощью объявления **`extern`** можно сделать определение в другом исходном файле видимым или сделать переменную видимой до ее определения в том же исходном файле. После объявления ссылки на переменную на внешнем уровне переменная становится видимой во всей остальной части единицы трансляции, в которой находится объявленная ссылка.

- Чтобы ссылка **`extern`** стала допустимой, необходимо объявить переменную, на которую она ссылается, один и только один раз на внешнем уровне. Это определение (без класса хранения **`extern`** ) может находиться в любой из единиц трансляции, составляющих программу.

## <a name="example"></a>Пример

В примере ниже показаны внешние объявления.

```C
/******************************************************************
                      SOURCE FILE ONE
*******************************************************************/
#include <stdio.h>

extern int i;                // Reference to i, defined below
void next( void );           // Function prototype

int main()
{
    i++;
    printf_s( "%d\n", i );   // i equals 4
    next();
}

int i = 3;                  // Definition of i

void next( void )
{
    i++;
    printf_s( "%d\n", i );  // i equals 5
    other();
}

/******************************************************************
                      SOURCE FILE TWO
*******************************************************************/
#include <stdio.h>

extern int i;              // Reference to i in
                           // first source file
void other( void )
{
    i++;
    printf_s( "%d\n", i ); // i equals 6
}
```

Два исходных файла в этом примере содержат три внешних объявления `i`. Только одно объявление является определяющим. Данное объявление

```C
int i = 3;
```

определяет глобальную переменную `i` и инициализирует ее начальным значением 3. Ссылочное объявление `i` вверху первого исходного файла, которое использует **`extern`** , делает глобальную переменную видимой до объявления, которое ее определяет. Ссылающееся объявление `i` во втором исходном файле также делает переменную видимой в этом исходном файле. Если определяющий экземпляр переменной не указан в записи преобразования, компилятор предполагает, что имеется ссылающееся объявление

```C
extern int x;
```

и что определяющая ссылка

```C
int x = 0;
```

отображается в другой записи преобразования программы.

Все три функции (`main`, `next` и `other`) выполняют одну и ту же задачу: они расширяют переменную `i` и отображают ее значение. Отображаются значения 4, 5 и 6.

Если бы переменная `i` не была инициализирована, ей автоматически было бы присвоено значение 0. В этом случае были бы отображены значения 1, 2 и 3. Дополнительные сведения об инициализации переменных см. в разделе [Инициализация](../c-language/initialization.md).

## <a name="see-also"></a>См. также

[Классы хранения в C](../c-language/c-storage-classes.md)
