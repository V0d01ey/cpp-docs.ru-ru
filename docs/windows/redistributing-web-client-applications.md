---
title: Распространение клиентских веб-приложений
ms.date: 11/04/2016
helpviewer_keywords:
- Web applications [C++], redistributing
- deploying applications [C++], Web applications
- Internet applications [C++], redistributing
- application deployment [C++], Web applications
ms.assetid: fe05988b-dee8-4a46-b381-016b5103a6bf
ms.openlocfilehash: f821541efd8705e61b3292cc63713d280fd17a68
ms.sourcegitcommit: 0ab61bc3d2b6cfbd52a16c6ab2b97a8ea1864f12
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "62388131"
---
# <a name="redistributing-web-client-applications"></a>Распространение клиентских веб-приложений

Если приложение использует классы MFC, реализуя элемент управления WebBrowser (например, `CHtmlView` или `CHtmlEditView`), на целевом компьютере нужно установить Microsoft Internet Explorer 4.0 или более поздней версии.

Установка последней версии Internet Explorer также гарантирует, что целевой компьютер располагает новейшими файлами общих элементов управления. Дополнительные сведения см. в статье [Установка и развертывание Internet Explorer 11](/internet-explorer/ie11-deploy-guide/install-and-deploy-ie11).

## <a name="see-also"></a>См. также

[Развертывание классических приложений](deploying-native-desktop-applications-visual-cpp.md)
