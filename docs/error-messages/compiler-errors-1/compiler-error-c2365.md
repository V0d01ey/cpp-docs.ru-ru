---
title: Ошибка компилятора C2365
ms.date: 11/04/2016
f1_keywords:
- C2365
helpviewer_keywords:
- C2365
ms.assetid: 35839b0b-4055-4b79-8957-b3a0871bdd02
ms.openlocfilehash: a25277f631ba51c43cde55510bf21be5b9720cc4
ms.sourcegitcommit: 16fa847794b60bf40c67d20f74751a67fccb602e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/03/2019
ms.locfileid: "74759871"
---
# <a name="compiler-error-c2365"></a>Ошибка компилятора C2365

"член класса": переопределение; предыдущим определением было "член класса"

Вы попытались переопределить член класса.

Следующий пример приводит к возникновению ошибки C2365:

```cpp
// C2365.cpp
// compile with: /c
class C1 {
   int CFunc();
   char *CFunc;   // C2365, already exists as a member function

   int CMem;
   char *CMem();   // C2365, already exists as a data member
};
```
