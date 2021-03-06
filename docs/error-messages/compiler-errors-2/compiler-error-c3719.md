---
title: Ошибка компилятора C3719
ms.date: 11/04/2016
f1_keywords:
- C3719
helpviewer_keywords:
- C3719
ms.assetid: d0d59d4e-babb-4480-9ef7-70cf1a28165c
ms.openlocfilehash: 9dce5fad3b38b0b0b396ff036f437af90e3e6d38
ms.sourcegitcommit: a1676bf6caae05ecd698f26ed80c08828722b237
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91510081"
---
# <a name="compiler-error-c3719"></a>Ошибка компилятора C3719

"Interface": источник событий на основе интерфейса может использоваться только для событий COM

Вы объявили интерфейс в контексте, не являющемся контекстом COM.

Следующий пример приводит к возникновению ошибки C3719:

```cpp
// C3719a.cpp
#define _ATL_ATTRIBUTES 1
#include "atlbase.h"
#include "atlcom.h"

[module(name="MyLibrary", version="1.2", helpfile="MyHelpFile")];

[object]
__interface I {
   HRESULT func1();
};

[event_source(native), coclass]
struct A {
   __event __interface I;   // C3719

   // try the following line instead
   // __event func2();
};

int main() {
}
```

Чтобы устранить эту ошибку, примените атрибуты [Object](../../windows/attributes/object-cpp.md), [coclass](../../windows/attributes/coclass.md), [event_source](../../windows/attributes/event-source.md)и [event_receiver](../../windows/attributes/event-receiver.md) соответствующим образом, чтобы сделать классы, в которых используются классы COM интерфейса. Пример:

```cpp
// C3719b.cpp
#define _ATL_ATTRIBUTES 1
#include <atlbase.h>
#include <atlcom.h>

[module(name="xx")];
[object, uuid("00000000-0000-0000-0000-000000000001")]
__interface I {
   HRESULT f();
};

[coclass, event_source(com) , uuid("00000000-0000-0000-0000-000000000002")]
struct MyStruct {
   __event __interface I;
};

[event_receiver(com)]
struct MyStruct2 {
   void f() {
   }
   MyStruct2(I* pB) {
      __hook(&I::f, pB, &MyStruct2::f);
   }
};

int main()
{
}
```
