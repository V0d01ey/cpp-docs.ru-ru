---
title: Привязка Imports
ms.date: 11/04/2016
helpviewer_keywords:
- /DELAY:NOBIND linker option
- DELAY:NOBIND linker option
ms.assetid: bb766038-deb1-41b1-bcbc-29a30e8c1e2a
ms.openlocfilehash: 4058d738b87b69a73e8f18d977be8435a7d96a14
ms.sourcegitcommit: 0ab61bc3d2b6cfbd52a16c6ab2b97a8ea1864f12
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "62272927"
---
# <a name="binding-imports"></a>Привязка Imports

Поведение компоновщика по умолчанию является создание связанной таблицы адресов импорта для библиотеки DLL, загружаемых с задержкой. Если библиотека DLL привязана, вспомогательная функция будет пытаться использовать данные привязки вместо вызова метода **GetProcAddress** на каждого из импортов, на который указывает ссылка. Если метка времени или предпочтительный адрес не совпадают с загружаемой библиотеки DLL, вспомогательная функция предполагает связанная таблица адресов импорта устарела и будет продолжена, как если бы он не существует.

Если не требуется привязать импорты, загружаемые с задержкой DLL, указание [/delay](delay-delay-load-import-settings.md): nobind в командной строке компоновщика предотвратит связанная таблица адресов импорта созданный и занимать много места в файле образа.

## <a name="see-also"></a>См. также

[Поддержка компоновщика для библиотек DLL с отложенной загрузкой](linker-support-for-delay-loaded-dlls.md)
