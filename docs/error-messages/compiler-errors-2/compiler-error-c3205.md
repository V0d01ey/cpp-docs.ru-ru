---
title: Ошибка компилятора C3205
ms.date: 11/04/2016
f1_keywords:
- C3205
helpviewer_keywords:
- C3205
ms.assetid: 802d306e-5ff3-4491-8a22-c5f1c072d005
ms.openlocfilehash: e7cb38a687daed93f72655337cd7cd3f25aaa08d
ms.sourcegitcommit: 857fa6b530224fa6c18675138043aba9aa0619fb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2020
ms.locfileid: "80176433"
---
# <a name="compiler-error-c3205"></a>Ошибка компилятора C3205

отсутствует список аргументов для параметра шаблона "параметр"

Отсутствует параметр [шаблона](../../cpp/templates-cpp.md) .

## <a name="example"></a>Пример

Следующий пример приводит к возникновению ошибки C3205:

```cpp
// C3205.cpp
template<template<class> class T> struct A {
   typedef T unparameterized_type;   // C3205
   // try the following line instead
   // typedef T<int> unparameterized_type;
};

template <class T>
struct B {
   typedef int value_type;
};

int main() {
   A<B> x;
}
```
