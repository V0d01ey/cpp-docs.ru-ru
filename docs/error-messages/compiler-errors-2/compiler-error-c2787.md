---
title: Ошибка компилятора C2787
ms.date: 11/04/2016
f1_keywords:
- C2787
helpviewer_keywords:
- C2787
ms.assetid: 34cb57e6-cafe-4ce7-bcc6-53d194629bd0
ms.openlocfilehash: 00f2097dc556055f0becf1d81d784c9126c66f63
ms.sourcegitcommit: 16fa847794b60bf40c67d20f74751a67fccb602e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/03/2019
ms.locfileid: "74739601"
---
# <a name="compiler-error-c2787"></a>Ошибка компилятора C2787

"идентификатор": с этим объектом не связан ни один GUID

Оператор [__uuidof](../../cpp/uuidof-operator.md) принимает определяемый пользователем тип с ПРИСОЕДИНЕНным идентификатором GUID или объектом такого определяемого пользователем типа. Эта ошибка возникает, когда аргумент является определяемым пользователем типом без GUID.

Следующий пример приводит к возникновению ошибки C2787:

```cpp
// C2787.cpp
#include <windows.h>
struct F {};

struct __declspec(uuid("00000000-0000-0000-c000-000000000046")) F2;

int main() {
   __uuidof(F);   // C2787
   __uuidof(F2);   // OK
}
```
