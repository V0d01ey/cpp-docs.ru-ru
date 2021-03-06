---
title: Классы хранения в C
ms.date: 08/31/2018
helpviewer_keywords:
- static variables, lifetime
- storage classes
- lifetime, variables
- specifiers, storage class
- storage class specifiers, C storage classes
- storage duration
ms.assetid: 893fb929-f7a9-43dc-a0b3-29cb1ef845c1
ms.openlocfilehash: 4f793e8485628faf0a80445ce0414835e3b71d1f
ms.sourcegitcommit: 1f009ab0f2cc4a177f2d1353d5a38f164612bdb1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/27/2020
ms.locfileid: "87217172"
---
# <a name="c-storage-classes"></a>Классы хранения в C

"Класс хранения" переменной определяет время существования элемента: глобальное или локальное. В языке С это время существования называется "статическое" или "автоматическое". Элемент с глобальным временем существования присутствует и имеет значение на протяжении всего исполнения программы. Все функции имеют глобальное время существования.

Автоматические переменные или переменные с локальным временем существования получают новое место хранения всякий раз, когда контроль исполнения переходит к блоку, в котором они определены. Когда выполнение возвращается, переменные более не имеют понятных значений.

C предоставляет следующие описатели класса хранения:

## <a name="syntax"></a>Синтаксис

*storage-class-specifier*:<br/>
&nbsp;&nbsp;&nbsp;&nbsp; **`auto`**<br/>
&nbsp;&nbsp;&nbsp;&nbsp; **`register`**<br/>
&nbsp;&nbsp;&nbsp;&nbsp; **`static`**<br/>
&nbsp;&nbsp;&nbsp;&nbsp; **`extern`**<br/>
&nbsp;&nbsp;&nbsp;&nbsp; **`typedef`**<br/>
&nbsp;&nbsp;&nbsp;&nbsp; **`__declspec (`** *extended-decl-modifier-seq* **`)`**  /\* Относится только к системам Майкрософт \*/

За исключением **`__declspec`** , в составе *declaration-specifier* в объявлении можно использовать только один описатель *storage-class-specifier*. Если нет спецификации класса хранения, объявления в блоке создают автоматические объекты.

Элементы, объявленные с описателем **`auto`** или **`register`** , имеют локальное время существования. Элементы, объявленные с описателем **`static`** или **`extern`** , имеют глобальное время существования.

Поскольку **`typedef`** и **`__declspec`** семантически отличаются от других четырех терминалов *storage-class-specifier*, они рассматриваются отдельно. Подробные сведения о **`typedef`** см. в статье [Объявления `typedef`](../c-language/typedef-declarations.md). Подробные сведения о **`__declspec`** см. в статье [Расширенные атрибуты классов хранения](../c-language/c-extended-storage-class-attributes.md).

Размещение объявлений переменных и функций в файлах исходного кода также влияет на класс хранения и видимость. Объявления вне определений функций отображаются на внешнем уровне. Объявления в составе определений функций отображаются на внутреннем уровне.

Точное значение каждого определителя класса хранения зависит от двух факторов:

- уровня отображения объявления (внешний или внутренний)

- типа объявляемого элемента (переменная или функция)

В статьях [Описатели классов хранения для объявлений внешнего уровня](../c-language/storage-class-specifiers-for-external-level-declarations.md) и [Описатели классов хранения для объявлений внутреннего уровня](../c-language/storage-class-specifiers-for-internal-level-declarations.md) описаны терминалы *storage-class-specifier* для каждого типа объявлений и поведение по умолчанию в случае отсутствия *storage-class-specifier* в декларации переменной. Статья [Описатели классов хранения с объявлениями функций](../c-language/storage-class-specifiers-with-function-declarations.md) поможет разобраться с описателями storage-class, используемыми с функциями.

## <a name="see-also"></a>См. также

[Объявления и типы](../c-language/declarations-and-types.md)
