---
title: Ошибка компилятора ресурсов RC2001
ms.date: 11/04/2016
f1_keywords:
- RC2001
helpviewer_keywords:
- RC2001
ms.assetid: 92bfb4c0-1879-4606-bb9f-ef7368707b4a
ms.openlocfilehash: 35042687b798b53857becdedba57861bd4f41a05
ms.sourcegitcommit: 857fa6b530224fa6c18675138043aba9aa0619fb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2020
ms.locfileid: "80191731"
---
# <a name="resource-compiler-error-rc2001"></a>Ошибка компилятора ресурсов RC2001

Новая строка в константе

Строковая константа была продолжена на второй строке без обратной косой черты ( **\\** ) или закрывающей и открывающей двойные кавычки ( **"** ).

Чтобы разорвать строковую константу, которая находится в двух строках исходного файла, выполните одно из следующих действий.

- Завершите первую строку символом продолжения строки, обратной косой чертой.

- Закройте строку в первой строке с двойной кавычкой и откройте строку в следующей строке с помощью другой кавычки.

Не нужно завершать первую строку символом \n, escape-последовательностью для встраивания символа новой строки в строковую константу.
