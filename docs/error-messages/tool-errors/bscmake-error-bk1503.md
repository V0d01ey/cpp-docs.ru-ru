---
title: Ошибка BSCMAKE BK1503
ms.date: 11/04/2016
f1_keywords:
- BK1503
helpviewer_keywords:
- BK1503
ms.assetid: e6582344-b91e-486f-baf3-4f9028d83c3b
ms.openlocfilehash: e0f05b3979024cb053394c51fa9337197b5de7bf
ms.sourcegitcommit: 857fa6b530224fa6c18675138043aba9aa0619fb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2020
ms.locfileid: "80197864"
---
# <a name="bscmake-error-bk1503"></a>Ошибка BSCMAKE BK1503

не удается выполнить запись в файл "имя файла" [: причина]

BSCMAKE объединяет SBR файлы, созданные во время компиляции, в одну базу данных браузера. Если итоговая база данных браузера превышает 64 МБ, или если количество входных файлов (SBR) превышает 4092, то эта ошибка будет выдаваться.

Если проблема вызвана более чем 4092 SBR-файлами, необходимо уменьшить количество входных файлов. В среде Visual Studio это можно сделать с помощью [/fr](../../build/reference/fr-fr-create-dot-sbr-file.md) всего проекта и повторной проверки файла по отдельности по файлам.

Если проблема вызвана расширением BSC-файла, превышающим 64 МБ, уменьшение числа SBR-файлов в качестве входных данных приведет к уменьшению размера полученного BSC-файла. Кроме того, объем информации для просмотра может быть уменьшен с помощью/ем (исключение символов, развернутых в макросах),/ел (исключение локальных переменных) и/ES (исключение системных файлов).

## <a name="see-also"></a>См. также раздел

[Параметры BSCMAKE](../../build/reference/bscmake-options.md)
