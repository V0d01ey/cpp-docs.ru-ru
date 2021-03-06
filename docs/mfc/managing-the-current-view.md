---
title: Управление текущим представлением
ms.date: 11/04/2016
helpviewer_keywords:
- views [MFC], and OnActivateView method [MFC]
- views [MFC], deactivating
- views [MFC], activating
- frame windows [MFC], current view
- OnActivateView method [MFC]
- views [MFC], current
- deactivating views [MFC]
- current view in frame window [MFC]
ms.assetid: 0a1cc22d-d646-4536-9ad2-3cb6d7092e4a
ms.openlocfilehash: d2ce9d77234260ebcb1946dd381264fb6654a91c
ms.sourcegitcommit: c21b05042debc97d14875e019ee9d698691ffc0b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84621314"
---
# <a name="managing-the-current-view"></a>Управление текущим представлением

В рамках реализации окна фрейма по умолчанию окно фрейма отслеживает текущее активное представление. Если окно фрейма содержит более одного представления, например в окне разделителя, текущее представление является самым недавним. Активное представление не зависит от активного окна в Windows или от текущего фокуса ввода.

При изменении активного представления платформа уведомляет текущее представление, вызывая его функцию члена [онактиватевиев](reference/cview-class.md#onactivateview) . Вы можете определить, активируется ли представление или деактивируется, изучив `OnActivateView` параметр *бактивате* . По умолчанию `OnActivateView` устанавливает фокус на текущее представление при активации. Можно переопределить `OnActivateView` для выполнения любой особой обработки при деактивации или повторной активации представления. Например, может потребоваться предоставить специальные визуальные подсказки для различения активного представления от других неактивных представлений.

Окно фрейма пересылает команды в текущее (активное) представление, как описано в разделе [Маршрутизация команд](command-routing.md)в рамках стандартной маршрутизации команд.

## <a name="see-also"></a>См. также раздел

[Использование окон фрейма](using-frame-windows.md)
