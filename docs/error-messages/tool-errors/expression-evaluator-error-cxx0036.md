---
title: Ошибка вычислителя выражений CXX0036
ms.date: 11/04/2016
f1_keywords:
- CXX0036
helpviewer_keywords:
- CXX0036
- CAN0036
ms.assetid: 383404be-df5b-4eec-b113-df21bb5d269d
ms.openlocfilehash: 164fd9ee00071e218e5bb4f3ab00febc618725a7
ms.sourcegitcommit: 857fa6b530224fa6c18675138043aba9aa0619fb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2020
ms.locfileid: "80195504"
---
# <a name="expression-evaluator-error-cxx0036"></a>Ошибка вычислителя выражений CXX0036

bad context {...} спецификация

Это сообщение может быть создано одной из нескольких ошибок при использовании оператора контекста ( **{}** ).

- Синтаксис оператора контекста ( **{}** ) был задан неправильно.

   Синтаксис оператора контекста:

     {*Function*,*модуль*,*DLL*} *выражение*

   Указывает контекст *выражения*. Оператор контекста имеет тот же приоритет и использование, что и приведение типа.

   Конечные запятые можно опустить. Если какая-либо *функция*, *модуль*или *Библиотека DLL* содержит литеральную запятую, необходимо заключить все имена в круглые скобки.

- Имя функции написано неправильно или не существует в указанном модуле или библиотеке динамической компоновки.

   Так как язык C является языком с учетом регистра, *функция* должна быть указана в конкретном регистре, так как она определена в источнике.

- Не удалось найти модуль или библиотеку DLL.

   Проверьте полный путь к указанному модулю или библиотеке DLL.

Эта ошибка идентична CAN0036.
