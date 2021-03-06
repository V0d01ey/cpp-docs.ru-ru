---
title: Классы архитектуры приложения MFC
ms.date: 11/04/2016
helpviewer_keywords:
- MFC, classes
- MFC, application development
- classes [MFC], MFC
- application architecture classes [MFC]
ms.assetid: 71b2de54-b44d-407e-9c71-9baf954e18d9
ms.openlocfilehash: 455a49a4f93f2fb2590447071edca76a32cbc5dd
ms.sourcegitcommit: c21b05042debc97d14875e019ee9d698691ffc0b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84618019"
---
# <a name="mfc-application-architecture-classes"></a>Классы архитектуры приложения MFC

Классы в этой категории вносят вклад в архитектуру приложения платформы. Они предоставляют функциональные возможности, общие для большинства приложений. Вы заполните платформу, чтобы добавить функции для конкретных приложений. Как правило, это делается путем наследования новых классов из классов архитектуры, а затем добавления новых элементов или переопределения существующих функций элементов.

[Мастера приложений](reference/mfc-application-wizard.md) создают несколько типов приложений, все из которых используют платформу приложений различными способами. Приложения SDI (однодокументный интерфейс) и MDI (многодокументный интерфейс) полностью используют часть платформы, называемую архитектурой "документ-представление". Другие типы приложений, такие как приложения, основанные на диалоговых окнах, приложения на основе форм и библиотеки DLL, используют только некоторые функции архитектуры "документ-представление".

Приложения для работы с документами и представлениями содержат один или несколько наборов документов, представлений и окон фрейма. Объект шаблона документа связывает классы для каждого набора документов, представлений и кадров.

Несмотря на то, что в приложении MFC не нужно использовать архитектуру документов и представлений, существует ряд преимуществ. Поддержка контейнеров и сервера OLE в MFC основана на архитектуре "документ-представление", что поддерживается для печати и предварительного просмотра печати.

Все приложения MFC имеют по крайней мере два объекта: объект приложения, производный от [CWinApp](reference/cwinapp-class.md), и некоторый основной объект окна, производный (часто косвенно) от [CWnd](reference/cwnd-class.md). (Чаще всего главное окно является производным от класса [CFrameWnd](reference/cframewnd-class.md), [CMDIFrameWnd](reference/cmdiframewnd-class.md)или [CDialog](reference/cdialog-class.md), который является производным от `CWnd` .)

Приложения, использующие архитектуру "документ-представление", содержат дополнительные объекты. Основные объекты:

- Объект приложения, производный от класса [CWinApp](reference/cwinapp-class.md), как упоминалось ранее.

- Один или несколько объектов классов документов, производных от класса [CDocument](reference/cdocument-class.md). Объекты класса Document отвечают за внутреннее представление данных, которыми манипулирует в представлении. Они могут быть связаны с файлом данных.

- Один или несколько объектов представления, производных от класса [CView](reference/cview-class.md). Каждое представление — это окно, присоединенное к документу и связанное с окном фрейма. Представления отображают данные, содержащиеся в объекте класса документа, и управляют ими.

Приложения для документов и представлений также содержат окна фрейма (производные от [CFrameWnd](reference/cframewnd-class.md)) и шаблоны документов (производные от [CDocTemplate](reference/cdoctemplate-class.md)).

## <a name="see-also"></a>См. также раздел

[Общие сведения о классах](class-library-overview.md)
