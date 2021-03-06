---
title: Ошибка компилятора ресурсов RC2101
ms.date: 11/04/2016
f1_keywords:
- RC2101
helpviewer_keywords:
- RC2101
ms.assetid: 580f9d74-162f-41e9-9438-ddbe3457c359
ms.openlocfilehash: 3fb576758e447c54e4ddfe7ddb024a1fd35a65f2
ms.sourcegitcommit: 857fa6b530224fa6c18675138043aba9aa0619fb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2020
ms.locfileid: "80191656"
---
# <a name="resource-compiler-error-rc2101"></a>Ошибка компилятора ресурсов RC2101

Недопустимая директива в предварительно обработанном файле RC

Файл компилятора ресурсов содержит директиву **#pragma** .

Используйте директиву препроцессора **#ifndef** с константой RC_INVOKED, которую компилятор ресурсов определяет при обработке включаемого файла. Поместите директиву **#pragma** внутрь блока кода, который не обрабатывается при определении константы RC_INVOKED. Код в блоке обрабатывается только C/C++ компилятором, а не компилятором ресурсов. Этот метод демонстрируется в следующем примере кода:

```
#ifndef RC_INVOKED
#pragma pack(2)  // C/C++ only, ignored by Resource Compiler
#endif
```

Директива препроцессора **#pragma** не имеет смысла в. RC-файл. Директива препроцессора **#include** часто используется в. RC-файл для включения файла заголовка (настраиваемого файла заголовка на основе проекта или стандартного файла заголовка, предоставленного корпорацией Майкрософт с одним из его продуктов). Некоторые из этих включаемых файлов содержат директиву **#pragma** . Поскольку файл заголовка может включать один или несколько других файлов заголовков, файл, содержащий директиву **#pragma** , вызвавшую ошибку, может быть не сразу очевидным.

Метод **#ifndef** RC_INVOKED может управлять включением файлов заголовков в файлы заголовков на основе проектов.
