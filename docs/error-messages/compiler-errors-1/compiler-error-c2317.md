---
title: Ошибка компилятора C2317
ms.date: 11/04/2016
f1_keywords:
- C2317
helpviewer_keywords:
- C2317
ms.assetid: e44d129b-8d3e-4ce9-9d79-6791ee77f25e
ms.openlocfilehash: 95444951106d74d01efe84f829b606eb2c547c99
ms.sourcegitcommit: 1f009ab0f2cc4a177f2d1353d5a38f164612bdb1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/27/2020
ms.locfileid: "87221267"
---
# <a name="compiler-error-c2317"></a>Ошибка компилятора C2317

блок try, начинающийся в строке "номер", не имеет соответствующих блоков catch

**`try`** Блок должен иметь по крайней мере один обработчик catch.

Следующий пример приводит к возникновению ошибки C2317:

```cpp
// C2317.cpp
// compile with: /EHsc
#include <eh.h>
int main() {
   try {
      throw "throw an exception";
   }
   // C2317, no catch handler
}
```

Возможное решение:

```cpp
// C2317b.cpp
// compile with: /EHsc
#include <eh.h>
int main() {
   try {
      throw "throw an exception";
   }
   catch(char*) {}
}
```
