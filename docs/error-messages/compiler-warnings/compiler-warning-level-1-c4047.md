---
title: Предупреждение компилятора (уровень 1) C4047
ms.date: 11/04/2016
f1_keywords:
- C4047
helpviewer_keywords:
- C4047
ms.assetid: b75ad6fb-5c93-4434-a85f-c4083051a5de
ms.openlocfilehash: be2f755793de53aa8ba88ac0a77c5031c7112226
ms.sourcegitcommit: c1fd917a8c06c6504f66f66315ff352d0c046700
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/16/2020
ms.locfileid: "90686409"
---
# <a name="compiler-warning-level-1-c4047"></a>Предупреждение компилятора (уровень 1) C4047

operator: identifier1 отличается по уровням косвенного обращения от identifier2

Указатель может указывать на переменную (один уровень косвенного обращения) к другому указателю, который указывает на переменную (два уровня косвенного обращения) и т. д.

## <a name="examples"></a>Примеры

Следующий пример приводит к возникновению ошибки C4047:

```c
// C4047.c
// compile with: /W1

int main() {
   char **p = 0;   // two levels of indirection
   char *q = 0;   // one level of indirection

   char *p2 = 0;   // one level of indirection
   char *q2 = 0;   // one level of indirection

   p = q;   // C4047
   p2 = q2;
}
```

Следующий пример приводит к возникновению ошибки C4047:

```c
// C4047b.c
// compile with: /W1
#include <stdio.h>

int main() {
   int i;
   FILE *myFile = NULL;
   errno_t  err = 0;
   char file_name[256];
   char *cs = 0;

   err = fopen_s(&myFile, "C4047.txt", "r");
   if ((err != 0) || (myFile)) {
      printf_s("fopen_s failed!\n");
      exit(-1);
    }
   i = fgets(file_name, 256, myFile);   // C4047
   cs = fgets(file_name, 256, myFile);   // OK
}
```
