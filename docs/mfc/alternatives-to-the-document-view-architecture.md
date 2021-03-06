---
title: Альтернативы архитектуре "документ-представление"
ms.date: 11/04/2016
helpviewer_keywords:
- documents [MFC], applications without
- CDocument class [MFC], space requirements
- views [MFC], applications without
ms.assetid: 2c22f352-a137-45ce-9971-c142173496fb
ms.openlocfilehash: 66325d1ae087b29f59f37197fb8695504bbddbc6
ms.sourcegitcommit: c21b05042debc97d14875e019ee9d698691ffc0b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84619754"
---
# <a name="alternatives-to-the-documentview-architecture"></a>Альтернативы для архитектуры "документ-представление"

Приложения MFC обычно используют архитектуру "документ-представление" для управления сведениями, форматами файлов и визуальным представлением данных для пользователей. Для большинства классических приложений архитектура документов и представлений представляет собой соответствующую и эффективную архитектуру приложения. Эта архитектура разделяет данные от просмотра и, в большинстве случаев, упрощает приложение и сокращает избыточный код.

Однако архитектура "документ-представление" не подходит для некоторых ситуаций. Рассмотрим следующие примеры.

- При переносе приложения, написанного на языке C для Windows, может потребоваться завершить работу порта перед добавлением в приложение поддержки документов и представлений.

- При написании упрощенной служебной программы может оказаться, что без архитектуры "документ-представление".

- Если исходный код уже смешивает управление данными с просмотром данных, то перемещение кода в модель «документ/представление» не стоит тратить усилий, поскольку необходимо разделить эти два. Вы можете оставить код без изменений.

Чтобы создать приложение, которое не использует архитектуру "документ/представление", снимите флажок **Поддержка архитектуры документа/представления** на шаге 1 мастера приложений MFC. Дополнительные сведения см. в разделе [Мастер приложений MFC](reference/mfc-application-wizard.md) .

> [!NOTE]
> Приложения на основе диалоговых окон, созданные мастером приложений MFC, не используют архитектуру "документ-представление", поэтому флажок " **Поддержка архитектуры документов/представлений** " отключается, если выбран тип приложения "диалог".

Мастера Visual C++, а также исходные и диалоговые редакторы работают с созданным приложением так же, как и с любым другим приложением, созданным мастером. Приложение может поддерживать панели инструментов, полосы прокрутки и строку состояния, а также имеет поле **About** . Приложение не будет регистрировать шаблоны документов и не будет содержать класс документа.

Обратите внимание, что созданное приложение имеет класс представления, `CChildView` производный от `CWnd` . MFC создает и помещает один экземпляр класса View в окна фрейма, созданные приложением. MFC по-прежнему обеспечивает использование окна представления, поскольку оно упрощает размещение и управление содержимым приложения. Вы можете добавить код рисования к `OnPaint` элементу этого класса. Код должен добавлять полосы прокрутки к представлению, а не к кадру.

Поскольку архитектура документов и представлений, предоставляемая MFC, отвечает за реализацию многих основных функций приложения, его отсутствие в проекте означает, что вы несете ответственность за реализацию многих важных функций приложения:

- Как указано в мастере приложений MFC, меню приложения содержит только команды **New** и **Exit** в меню **файл** . ( **Новая** команда поддерживается только для приложений MDI, а не для приложений SDI без поддержки документов и представлений.) Созданный ресурс меню не будет поддерживать список MRU (самый последний использованный).

- Необходимо добавить функции и реализации обработчика для всех команд, которые будет поддерживать ваше приложение, включая **Открытие** и **Сохранение** в меню **файл** . MFC обычно предоставляет код для поддержки этих функций, но эта поддержка тесно связана с архитектурой "документ-представление".

- Панель инструментов приложения, если вы запросили его, будет минимальной.

Настоятельно рекомендуется использовать мастер приложений MFC для создания приложений без архитектуры "документ-представление", так как мастер гарантирует правильную архитектуру MFC. Однако, если необходимо избегать использования мастера, вот несколько подходов к обходу архитектуры документа/представления в коде:

- Рассматривайте документ как неиспользуемый аппендаже и реализуйте свой код управления данными в классе View, как было предложено выше. Затраты на документ относительно низкие. В одном объекте [CDocument](reference/cdocument-class.md) возникает небольшой объем издержек, а также небольшие издержки `CDocument` базовых классов, [от CCmdTarget](reference/ccmdtarget-class.md) и [CObject](reference/cobject-class.md). Оба последних класса являются небольшими.

   Объявлено в `CDocument` :

  - Два `CString` объекта.

  - Три типа **bool**s.

  - Один `CDocTemplate` указатель.

  - Один `CPtrList` объект, содержащий список представлений документа.

  Кроме того, документ требует времени на создание объекта документа, его объектов представления, окна фрейма и объекта шаблона документа.

- Рассматривать как документ, так и представление как неиспользуемые дополнений. Размещение кода для управления данными и рисования в окне фрейма, а не в представлении. Этот подход ближе к модели программирования на языке C.

- Переопределите части платформы MFC, которые создают документ и представление, чтобы исключить их создание. Процесс создания документа начинается с вызова `CWinApp::AddDocTemplate` . Удалите этот вызов из функции-члена класса приложения `InitInstance` , а вместо этого создайте окно фрейма `InitInstance` самостоятельно. Добавьте код управления данными в класс окна фрейма. Процесс создания документа/представления показан в статье [Создание документа или представления](document-view-creation.md). Это больше работы и требует более глубокого понимания платформы, но она позволяет полностью разобраться в работе с документами и представлениями.

Статья [MFC: использование классов баз данных без документов и представлений](../data/mfc-using-database-classes-without-documents-and-views.md) предоставляет более конкретные примеры альтернативы документа или представления в контексте приложений базы данных.

## <a name="see-also"></a>См. также раздел

[Архитектура документа/представления](document-view-architecture.md)
