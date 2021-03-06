---
title: Демонстрация маршрутизации команд
ms.date: 11/04/2016
helpviewer_keywords:
- MFC, command routing
- command handling [MFC], routing commands
- command routing [MFC], OnCmdMsg handler
ms.assetid: 4b7b4741-565f-4878-b076-fd85c670f87f
ms.openlocfilehash: d362cfe54a9b5a562237c7bb9632edae6e58228b
ms.sourcegitcommit: c21b05042debc97d14875e019ee9d698691ffc0b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84622914"
---
# <a name="command-routing-illustration"></a>Демонстрация маршрутизации команд

Чтобы проиллюстрировать это, рассмотрим командное сообщение из пункта меню «Очистить все» в меню «Правка» приложения MDI. Предположим, что функция обработчика для этой команды является функцией-членом класса документа приложения. Вот как эта команда достигает своего обработчика после того, как пользователь выберет пункт меню:

1. Основное окно фрейма сначала получает командное сообщение.

1. Основное окно фрейма MDI предоставляет в настоящее время активное дочернее окно MDI шанс обработки команды.

1. Стандартная маршрутизация окна дочернего фрейма MDI дает ему возможность увидеть шанс выполнения команды перед проверкой собственной схемы сообщений.

1. Представление сначала проверяет собственную схему сообщений и не обнаруживает обработчик, а затем отправляет команду связанному с ним документу.

1. Документ проверяет свою карту сообщений и находит обработчик. Эта функция-член документа вызывается, и маршрутизация останавливается.

Если у документа нет обработчика, он будет пересылать команду в шаблон документа. Затем команда вернется в представление, а затем в окно фрейма. Наконец, окно фрейма будет проверять свою карту сообщений. Если эта проверка не удалась, команда будет направляться обратно в основное окно фрейма MDI, а затем в объект приложения — конечное назначение необработанных команд.

## <a name="see-also"></a>См. также раздел

[Вызовы обработчика со стороны платформы](how-the-framework-calls-a-handler.md)
