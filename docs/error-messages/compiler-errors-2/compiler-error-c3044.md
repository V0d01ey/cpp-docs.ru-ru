---
title: Ошибка компилятора C3044
ms.date: 11/04/2016
f1_keywords:
- C3044
helpviewer_keywords:
- C3044
ms.assetid: 9f3e25b2-4676-49ab-97bf-6c88cd0fa377
ms.openlocfilehash: 0ac2a6a86ffd719dbb5f39fc085d3f5cbca0567d
ms.sourcegitcommit: a1676bf6caae05ecd698f26ed80c08828722b237
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91506361"
---
# <a name="compiler-error-c3044"></a>Ошибка компилятора C3044

section: допускается только непосредственное вложение в директиву OpenMP "sections"

Компилятор обнаружил неправильное использование директивы `section` . Дополнительные сведения см. в разделе [разделы](../../parallel/openmp/reference/openmp-directives.md#sections-openmp).

Следующий пример приводит к возникновению ошибки C3044:

```cpp
// C3044.cpp
// compile with: /openmp /c
#include "omp.h"
int main() {
   int n2 = 2, n3 = 3;

   #pragma omp parallel
   {
      ++n2;

      #pragma omp sections
      {
         ++n2;
      }

      #pragma omp section   // C3044
      {
         ++n3;
      }
   }

   #pragma omp parallel
   {
      ++n2;

      #pragma omp sections
      {
         #pragma omp section   // OK
         {
            ++n3;
         }
      }
   }
}
```
