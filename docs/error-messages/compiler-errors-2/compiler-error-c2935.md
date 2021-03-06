---
title: Ошибка компилятора C2935
ms.date: 11/04/2016
f1_keywords:
- C2935
helpviewer_keywords:
- C2935
ms.assetid: e11ef90d-0756-4e43-8a09-4974c6aa72a3
ms.openlocfilehash: 676c238dfb0ae78dbe5b144b5242bfb4ccbda76c
ms.sourcegitcommit: 16fa847794b60bf40c67d20f74751a67fccb602e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/03/2019
ms.locfileid: "74756153"
---
# <a name="compiler-error-c2935"></a>Ошибка компилятора C2935

"класс": идентификатор класса типа переопределен как глобальная функция

Универсальный класс или класс-шаблон нельзя использовать в качестве глобальной функции.

Эта ошибка может возникнуть при неправильной расстановке скобок.

Следующий пример приводит к возникновению ошибки C2935:

```cpp
// C2935.cpp
// compile with: /c
template<class T>
struct TC {};
void TC<int>() {}   // C2935

// OK
struct TC2 {};
void TC2() {}
```

Ошибка C2935 также может возникнуть при использовании универсальных шаблонов:

```cpp
// C2935b.cpp
// compile with: /clr /c
generic<class T>
ref struct GC { };
void GC<int>() {}   // C2935
void GC() {}   // OK
```
