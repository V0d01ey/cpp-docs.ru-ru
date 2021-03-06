---
title: Многостраничные документы
ms.date: 11/19/2018
helpviewer_keywords:
- pagination [MFC]
- overriding [MFC], View class functions for printing
- OnPrepareDC method [MFC]
- CPrintInfo structure [MFC], multipage documents
- OnEndPrinting method [MFC]
- protocols [MFC], printing protocol
- document pages vs. printer pages [MFC]
- printer mode [MFC]
- printing [MFC], multipage documents
- printers [MFC], printer mode
- documents [MFC], printing
- OnPreparePrinting method [MFC]
- OnPrint method [MFC]
- DoPreparePrinting method and pagination [MFC]
- OnDraw method [MFC], printing
- pagination [MFC], printing multipage documents
- printing [MFC], protocol
- pages [MFC], printing
- OnBeginPrinting method [MFC]
- multipage documents [MFC]
- printing [MFC], pagination
- documents [MFC], paginating
ms.assetid: 69626b86-73ac-4b74-b126-9955034835ef
ms.openlocfilehash: c73692c315b07d6b690180886d494ee12f85f52d
ms.sourcegitcommit: c21b05042debc97d14875e019ee9d698691ffc0b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84621059"
---
# <a name="multipage-documents"></a>Многостраничные документы

В этой статье описывается протокол печати Windows и объясняется, как печатать документы, содержащие более одной страницы. В статье рассматриваются следующие темы:

- [Протокол печати](#_core_the_printing_protocol)

- [Переопределение функций класса представления](#_core_overriding_view_class_functions)

- [Разбивк](#_core_pagination)

- [Страницы принтеров и страницы документов](#_core_printer_pages_vs.._document_pages)

- [Разбивка на страницы во время печати](#_core_print.2d.time_pagination)

## <a name="the-printing-protocol"></a><a name="_core_the_printing_protocol"></a>Протокол печати

Чтобы напечатать многостраничный документ, платформа и представление взаимодействуют следующим образом. Сначала платформа отображает диалоговое окно **Печать** , создает контекст устройства для принтера и вызывает функцию-член [стартдок](reference/cdc-class.md#startdoc) объекта [CDC](reference/cdc-class.md) . Затем для каждой страницы документа платформа вызывает функцию-член [StartPage](reference/cdc-class.md#startpage) `CDC` объекта, предписывает объекту представления выводить страницу и вызывает функцию-член [EndPage](reference/cdc-class.md#endpage) . Если необходимо изменить режим печати перед запуском определенной страницы, представление вызывает [ресетдк](reference/cdc-class.md#resetdc), которое обновляет структуру [DEVMODE](/windows/win32/api/wingdi/ns-wingdi-devmodea) , содержащую новые сведения о режиме принтера. При печати всего документа платформа вызывает функцию члена [енддок](reference/cdc-class.md#enddoc) .

## <a name="overriding-view-class-functions"></a><a name="_core_overriding_view_class_functions"></a>Переопределение функций класса представления

Класс [CView](reference/cview-class.md) определяет несколько функций — членов, которые вызываются платформой во время печати. Переопределяя эти функции в классе представления, вы предоставляете соединения между логикой печати платформы и логикой печати класса представления. В следующей таблице перечислены эти функции элементов.

### <a name="cviews-overridable-functions-for-printing"></a>Переопределяемые функции CView для печати

|Имя|Причина переопределения|
|----------|---------------------------|
|[онпрепарепринтинг](reference/cview-class.md#onprepareprinting)|Вставка значений в диалоговое окно «Печать», особенно длина документа|
|[онбегинпринтинг](reference/cview-class.md#onbeginprinting)|Выделение шрифтов или других ресурсов GDI|
|[онпрепаредк](reference/cview-class.md#onpreparedc)|Настройка атрибутов контекста устройства для данной страницы или разбивка на страницы во время печати|
|[Печать](reference/cview-class.md#onprint)|Печать данной страницы|
|[онендпринтинг](reference/cview-class.md#onendprinting)|Освобождение ресурсов GDI|

Можно также выполнить обработку, связанную с печатью, в других функциях, но эти функции являются теми, которые выполняют процесс печати.

На следующем рисунке показаны шаги, связанные с процессом печати, и показано, где `CView` вызываются все функции элементов печати. В оставшейся части этой статьи подробно описаны эти действия. Дополнительные компоненты процесса печати описаны в статье [выделение ресурсов GDI](allocating-gdi-resources.md).

![Процесс цикла печати](../mfc/media/vc37c71.gif "Процесс цикла печати") <br/>
Цикл печати

## <a name="pagination"></a><a name="_core_pagination"></a>Разбивк

Платформа хранит большую часть информации о задании печати в структуре [CPrintInfo](reference/cprintinfo-structure.md) . Некоторые значения в параметре `CPrintInfo` относятся к разбиению на страницы; эти значения доступны, как показано в следующей таблице.

### <a name="page-number-information-stored-in-cprintinfo"></a>Сведения о номере страницы, хранящиеся в CPrintInfo

|Переменная члена или<br /><br /> имена функций|Номер страницы, на которую указывает ссылка|
|-----------------------------------------------|----------------------------|
|`GetMinPage`/`SetMinPage`|Первая страница документа|
|`GetMaxPage`/`SetMaxPage`|Последняя страница документа|
|`GetFromPage`|Первая страница для печати|
|`GetToPage`|Последняя страница для печати|
|`m_nCurPage`|Напечатанная на данный момент страница|

Номера страниц начинаются с 1, то есть первая страница пронумерована 1, а не 0. Дополнительные сведения об этих и других членах [CPrintInfo](reference/cprintinfo-structure.md)см. в *справочнике по MFC*.

В начале процесса печати платформа вызывает функцию-член [онпрепарепринтинг](reference/cview-class.md#onprepareprinting) представления, передавая указатель на `CPrintInfo` структуру. Мастер приложений предоставляет реализацию `OnPreparePrinting` , которая вызывает [метод DoPreparePrinting](reference/cview-class.md#doprepareprinting), другую функцию члена `CView` . `DoPreparePrinting`функция, которая отображает диалоговое окно Печать и создает контекст устройства печати.

На этом этапе приложение не знает, сколько страниц находится в документе. Он использует значения по умолчанию 1 и 0xFFFF для номеров первой и последней страницы документа. Если известно, сколько страниц содержит документ, переопределите `OnPreparePrinting` и вызовите [сетмакспаже]--brokenlink--(Reference/CPrintInfo-Class. md # сетмакспаже) для `CPrintInfo` структуры перед отправкой `DoPreparePrinting` . Это позволяет указать длину документа.

`DoPreparePrinting`затем выводит диалоговое окно Печать. При возвращении `CPrintInfo` структура содержит значения, указанные пользователем. Если пользователь хочет напечатать только выбранный диапазон страниц, он может указать начальные и конечные номера страниц в диалоговом окне Печать. Платформа извлекает эти значения с помощью `GetFromPage` функций и `GetToPage` объекта [CPrintInfo](reference/cprintinfo-structure.md). Если пользователь не указал диапазон страниц, платформа вызывает `GetMinPage` и `GetMaxPage` и использует значения, возвращаемые для печати всего документа.

Для каждой страницы печатаемого документа платформа вызывает две функции-члены в классе представления, [онпрепаредк](reference/cview-class.md#onpreparedc) и [OnPrint](reference/cview-class.md#onprint), и передает каждую функцию два параметра: указатель на объект [CDC](reference/cdc-class.md) и указатель на `CPrintInfo` структуру. Каждый раз, когда платформа вызывает `OnPrepareDC` и `OnPrint` , она передает другое значение в *m_nCurPage* члене `CPrintInfo` структуры. Таким образом платформа сообщает, какая страница должна быть распечатана.

Функция-член [онпрепаредк](reference/cview-class.md#onpreparedc) также используется для отображения экрана. Он вносит изменения в контекст устройства перед рисованием. `OnPrepareDC`играет аналогичную роль при печати, но существует несколько отличий: сначала `CDC` объект представляет контекст устройства печати вместо контекста устройства экрана, а во-вторых, `CPrintInfo` объект передается как второй параметр. (Этот параметр имеет **значение NULL** , если `OnPrepareDC` вызывается для отображения на экране.) Переопределите `OnPrepareDC` , чтобы внести изменения в контекст устройства в зависимости от того, какая страница печатается. Например, можно переместить источник окна просмотра и область обрезки, чтобы убедиться, что печатается соответствующая часть документа.

Функция-член [OnPrint](reference/cview-class.md#onprint) выполняет фактическую печать страницы. В этой [статье показано, как платформа](how-default-printing-is-done.md) вызывает [OnDraw](reference/cview-class.md#ondraw) с контекстом печатающего устройства для выполнения печати. Точнее, платформа вызывается `OnPrint` с помощью `CPrintInfo` структуры и контекста устройства и `OnPrint` передает контекст устройства в `OnDraw` . Переопределение `OnPrint` для выполнения любой отрисовки, которая должна выполняться только во время печати, а не для отображения на экране. Например, для печати верхних и нижних колонтитулов (Дополнительные сведения см. в статье [верхние и нижние](headers-and-footers.md) колонтитулы). Затем вызовите `OnDraw` из переопределения, `OnPrint` чтобы сделать отрисовку общей для отображения и печати на экране.

Тот факт, что `OnDraw` визуализация как для экрана, так и для печати, означает, что ваше приложение является WYSIWYG: «что вы видите, что вы получаете». Однако предположим, что вы не пишете приложение WYSIWYG. Например, рассмотрим текстовый редактор, использующий полужирный шрифт для печати, но отображающий управляющие коды для обозначения полужирного текста на экране. В такой ситуации используется `OnDraw` исключительно для отображения экрана. При переопределении `OnPrint` замените вызов на `OnDraw` вызов отдельной функции рисования. Эта функция рисует документ в том виде, в котором он отображается на бумаге, используя атрибуты, которые не отображаются на экране.

## <a name="printer-pages-vs-document-pages"></a><a name="_core_printer_pages_vs.._document_pages"></a>Страницы принтеров и страницы документов

При ссылке на номера страниц иногда бывает необходимо отличать концепцию страницы и концепцию документа к нему. С точки зрения принтера, страница является одним листом бумаги. Однако один лист бумаги не обязательно равен одной странице документа. Например, если печатается информационный бюллетень, в котором должны быть включены листы, один лист бумаги может содержать как первую, так и последнюю страницы документа, параллельно. Аналогично, при печати электронной таблицы документ вообще не состоит из страниц. Вместо этого один лист бумаги может содержать строки с 1 по 20, столбцы с 6 по 10.

Все номера страниц в структуре [CPrintInfo](reference/cprintinfo-structure.md) относятся к страницам принтера. Платформа вызывает `OnPrepareDC` и `OnPrint` один раз для каждого листа бумаги, который будет проходить через принтер. При переопределении функции [онпрепарепринтинг](reference/cview-class.md#onprepareprinting) для указания длины документа необходимо использовать страницы принтера. Если имеется корреспонденция "один к одному" (то есть одна страница принтера равна одной странице документа), это просто. Если, с другой стороны, страницы документа и страницы принтера не соответствуют непосредственно, необходимо выполнить преобразование между ними. Например, рассмотрите возможность печати электронной таблицы. При переопределении `OnPreparePrinting` необходимо вычислить, сколько листов бумаги потребуется для печати всей таблицы, а затем использовать это значение при вызове `SetMaxPage` функции члена `CPrintInfo` . Аналогичным образом при переопределении `OnPrepareDC` необходимо преобразовать *m_nCurPage* в диапазон строк и столбцов, которые будут отображаться на этом конкретном листе, а затем соответствующим образом настроить источник окна просмотра.

## <a name="print-time-pagination"></a><a name="_core_print.2d.time_pagination"></a>Разбивка на страницы во время печати

В некоторых ситуациях класс представления может не заранее узнать, как долго документ будет напечатан. Например, предположим, что приложение не поддерживает режим WYSIWYG, поэтому длина документа на экране не соответствует его длине.

Это вызывает проблему при переопределении [онпрепарепринтинг](reference/cview-class.md#onprepareprinting) для класса представления: вы не можете передать значение `SetMaxPage` функции структуры [CPrintInfo](reference/cprintinfo-structure.md) , так как вы не помните длину документа. Если пользователь не указал номер страницы для отмены в диалоговом окне «Печать», платформа не знает, когда следует прерывать цикл печати. Единственный способ определить время прекращения цикла печати — распечатать документ и увидеть, когда он заканчивается. Класс представления должен проверять конец документа во время его печати, а затем уведомлять платформу о достижении конца.

Платформа использует функцию [онпрепаредк](reference/cview-class.md#onpreparedc) класса представления, чтобы сообщить ей, когда следует ее останавливаться. После каждого вызова `OnPrepareDC` платформа проверяет член `CPrintInfo` структуры с именем *m_bContinuePrinting*. Его значение по умолчанию — **true.** Пока это остается, платформа продолжает цикл печати. Если задано **значение false**, инфраструктура останавливается. Для выполнения разбивки на страницы во время печати Переопределите, `OnPrepareDC` чтобы проверить, достигнут ли конец документа, и задайте *m_bContinuePrinting* для m_bContinuePrinting **значение false** , если оно есть.

Реализация по умолчанию `OnPrepareDC` устанавливает *m_bContinuePrinting* равным **false** , если текущая страница больше 1. Это означает, что если длина документа не была указана, платформа предполагает, что документ является одной страницей длинной страницы. Один из этих последствий заключается в том, что необходимо соблюдать осторожность при вызове версии базового класса `OnPrepareDC` . Не считайте, что *m_bContinuePrinting* будет иметь **значение true** после вызова версии базового класса.

### <a name="what-do-you-want-to-know-more-about"></a>Что вы хотите узнать подробнее

- [Верхние и нижние колонтитулы](headers-and-footers.md)

- [Выделение ресурсов GDI](allocating-gdi-resources.md)

## <a name="see-also"></a>См. также раздел

[Вывод на печать](printing.md)<br/>
[Класс CView](reference/cview-class.md)<br/>
[CDC, класс](reference/cdc-class.md)
