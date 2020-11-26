---
title: Настройка обработки командной строки C
description: Сведения о том, как подавить обработку параметра среды и аргумента функции `main` в коде запуска среды выполнения.
ms.date: 11/19/2020
helpviewer_keywords:
- _spawn functions
- command line, processing
- command-line processing
- startup code, customizing command-line processing
- environment, environment-processing routine
- _setargv function
- command line, processing arguments
- suppressing environment processing
- _exec function
ms.openlocfilehash: fc306172491cd401caeecb3c87c0711f8b4ef911
ms.sourcegitcommit: b02c61667ff7f38e7add266d0aabd8463f2dbfa1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/23/2020
ms.locfileid: "95483299"
---
# <a name="customizing-c-command-line-processing"></a>Настройка обработки командной строки C

Если программа не принимает аргументы командной строки, можно сохранить небольшой объем пространства, подавив подпрограмму обработки командной строки. Для этого включите файл *`noarg.obj`* (для `main` и `wmain`) в параметры компилятора **`/link`** или командную строку **`LINK`** .

Аналогичным образом, если вы никогда не использовали таблицу среды для доступа к аргументу *`envp`* , можно подавить внутреннюю подпрограмму обработки среды. Для этого включите файл *`noenv.obj`* (для `main` и `wmain`) в параметры компилятора **`/link`** или командную строку **`LINK`** .

Дополнительные сведения о параметрах компоновщика для запуска среды выполнения см. в статье [Параметры ссылок](../c-runtime-library/link-options.md).

Программа может вызывать семейство подпрограмм `spawn` или `exec` в библиотеке среды выполнения C. В этом случае не следует подавлять подпрограмму обработки среды, так как она используется для передачи данных о среде из родительского процесса в дочерний.

## <a name="see-also"></a>См. также раздел

[Функция `main` и выполнение программ](../c-language/main-function-and-program-execution.md)\
[Параметры ссылок](../c-runtime-library/link-options.md)
