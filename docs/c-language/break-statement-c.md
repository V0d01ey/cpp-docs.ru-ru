---
title: Оператор break (C)
ms.date: 11/04/2016
f1_keywords:
- break
helpviewer_keywords:
- break keyword [C]
ms.assetid: 015627c7-0924-49b2-a4d1-c77edf5eae6a
ms.openlocfilehash: 5322f8cb5218882d891e2cd66704f9dbfd1bc149
ms.sourcegitcommit: c1fd917a8c06c6504f66f66315ff352d0c046700
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/16/2020
ms.locfileid: "90683472"
---
# <a name="break-statement-c"></a>Оператор break (C)

Оператор **`break`** завершает выполнение ближайшего внешнего оператора **`do`** , **`for`** , **`switch`** или **`while`** , в котором он находится. Управление передается оператору, который расположен после завершенного оператора.

## <a name="syntax"></a>Синтаксис

*оператор-перехода*:<br/>
&nbsp;&nbsp;&nbsp;&nbsp;**break ;**

Оператор **`break`** часто используется для заверения обработки определенного блока в операторе **`switch`** . Если внешний оператор итерации или оператор **`switch`** отсутствует, создается ошибка.

Если используются вложенные операторы, то **`break`** завершает выполнение только того оператора **`do`** , **`for`** , **`switch`** или **`while`** , который непосредственно его окружает. Передать управление за пределы вложенной структуры можно при помощи оператора **`return`** или **`goto`** .

В следующем примере показано использование оператора **`break`** :

```C
#include <stdio.h>
int main() {
   char c;
   for(;;) {
      printf_s( "\nPress any key, Q to quit: " );

      // Convert to character value
      scanf_s("%c", &c);
      if (c == 'Q')
          break;
   }
} // Loop exits only when 'Q' is pressed
```

## <a name="see-also"></a>См. также

[Оператор break](../cpp/break-statement-cpp.md)
