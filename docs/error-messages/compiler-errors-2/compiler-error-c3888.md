---
title: Ошибка компилятора C3888
ms.date: 11/04/2016
f1_keywords:
- C3888
helpviewer_keywords:
- C3888
ms.assetid: 40820812-79c5-4dcd-a19d-b4164d25fc8a
ms.openlocfilehash: 40156dfaad5965d30a32d3aa2ac574a5f91999ba
ms.sourcegitcommit: 857fa6b530224fa6c18675138043aba9aa0619fb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2020
ms.locfileid: "80176408"
---
# <a name="compiler-error-c3888"></a>Ошибка компилятора C3888

"имя": выражение константы, связанное с этими данными-членом литерала, не поддерживается в C++/CLI

Член данных *имя* , объявленный с помощью ключевого слова [literal](../../extensions/literal-cpp-component-extensions.md) , инициализирован со значением, не поддерживаемым компилятором. Компилятор поддерживает только целочисленные константы, перечисления и строковые типы. Возможной причиной ошибки **C3888** может быть то, что член данных инициализирован с помощью байтового массива.

### <a name="to-correct-this-error"></a>Исправление ошибки

1. Проверьте, имеет ли объявленный член данных литерала поддерживаемый тип.

## <a name="see-also"></a>См. также раздел

[literal](../../extensions/literal-cpp-component-extensions.md)
