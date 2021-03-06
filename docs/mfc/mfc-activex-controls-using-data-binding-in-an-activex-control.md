---
title: Элементы управления ActiveX в MFC. Использование привязки данных в элементе управления ActiveX
ms.date: 11/19/2018
f1_keywords:
- bindable
- requestedit
- defaultbind
- displaybind
- dispid
helpviewer_keywords:
- MFC ActiveX controls [MFC], data binding
- data binding [MFC], MFC ActiveX controls
- data-bound controls [MFC], MFC ActiveX controls
- controls [MFC], data binding
- bound controls [MFC], MFC ActiveX
ms.assetid: 476b590a-bf2a-498a-81b7-dd476bd346f1
ms.openlocfilehash: b32dbd8e1777f11998085a90e8851b25e4298e1a
ms.sourcegitcommit: 1f009ab0f2cc4a177f2d1353d5a38f164612bdb1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/27/2020
ms.locfileid: "87224998"
---
# <a name="mfc-activex-controls-using-data-binding-in-an-activex-control"></a>Элементы управления ActiveX в MFC. Использование привязки данных в элементе управления ActiveX

Одним из более эффективных способов использования элементов управления ActiveX является привязка данных, позволяющая привязывать свойство элемента управления к определенному полю в базе данных. Когда пользователь изменяет данные в этом привязанном свойстве, элемент управления уведомляет базу данных и запрашивает обновление поля записи. Затем база данных уведомляет элемент управления об успешном или неуспешном выполнении запроса.

>[!IMPORTANT]
> ActiveX — это устаревшая технология, которую не следует использовать для новой разработки. Дополнительные сведения о современных технологиях, которые заменяют ActiveX, см. в разделе [элементы управления ActiveX](activex-controls.md).

В этой статье описывается сторона управления задачи. Реализовать взаимодействие привязки данных с базой данных является обязанностью контейнера элемента управления. Управление взаимодействием с базой данных в контейнере выходит за рамки данной документации. Процедура подготовки элемента управления к привязке данных объясняется в оставшейся части этой статьи.

![Концептуальная схема элемента управления с привязкой данных&#45;](../mfc/media/vc374v1.gif "Концептуальная схема элемента управления с привязкой данных&#45;") <br/>
Концептуальная схема элемента управления с привязкой к данным

`COleControl`Класс предоставляет две функции элементов, которые делают привязку данных удобной для реализации простым процессом. Первая функция, [баундпропертирекуестедит](reference/colecontrol-class.md#boundpropertyrequestedit), используется для запроса разрешения на изменение значения свойства. [Баундпропертичанжед](reference/colecontrol-class.md#boundpropertychanged), вторая функция вызывается после успешного изменения значения свойства.

В этой статье рассматриваются следующие темы:

- [Создание свойства акции с возможностью привязки](#vchowcreatingbindablestockproperty)

- [Создание привязке метода Get/Set](#vchowcreatingbindablegetsetmethod)

## <a name="creating-a-bindable-stock-property"></a><a name="vchowcreatingbindablestockproperty"></a>Создание свойства акции с возможностью привязки

Можно создать свойство акции с привязкой к данным, хотя более вероятно, что потребуется [привязать метод Get/Set](#vchowcreatingbindablegetsetmethod).

> [!NOTE]
> Стандартные свойства по `bindable` `requestedit` умолчанию имеют атрибуты и.

#### <a name="to-add-a-bindable-stock-property-using-the-add-property-wizard"></a>Добавление свойства акции с возможностью привязки с помощью мастера добавления свойства

1. Начните проект с помощью [мастера элементов ActiveX MFC](reference/mfc-activex-control-wizard.md).

1. Щелкните правой кнопкой мыши узел интерфейса для своего элемента управления.

   Откроется контекстное меню.

1. В контекстном меню выберите **Добавить** , а затем — **Добавить свойство**.

1. Выберите одну из записей из раскрывающегося списка **имя свойства** . Например, можно выбрать **текст**.

   Поскольку **текст** является свойством акции, атрибуты с **возможностью привязки** и **рекуестедит** уже отмечены.

1. Установите следующие флажки на вкладке **атрибуты IDL** : **дисплайбинд** и **дефаултбинд** , чтобы добавить атрибуты в определение свойства в проекте. IDL-файл. Эти атрибуты делают элемент управления видимым для пользователя и делают свойство «акции» доступным для привязки по умолчанию.

На этом этапе элемент управления может отображать данные из источника данных, но пользователь не сможет обновлять поля данных. Если вы хотите, чтобы элемент управления также мог обновлять данные, измените `OnOcmCommand` функцию [OnOcmCommand](mfc-activex-controls-subclassing-a-windows-control.md) так, чтобы она выглядела следующим образом:

[!code-cpp[NVC_MFC_AxData#1](codesnippet/cpp/mfc-activex-controls-using-data-binding-in-an-activex-control_1.cpp)]

Теперь можно выполнить сборку проекта, который будет регистрировать элемент управления. При вставке элемента управления в диалоговое окно были добавлены **поля данных** и свойства **источника данных** , и теперь можно выбрать источник данных и поле, которые будут отображаться в элементе управления.

## <a name="creating-a-bindable-getset-method"></a><a name="vchowcreatingbindablegetsetmethod"></a>Создание привязке метода Get/Set

В дополнение к методу get/set с привязкой к данным также можно создать [свойство акции с возможностью привязки](#vchowcreatingbindablestockproperty).

> [!NOTE]
> В этой процедуре предполагается, что у вас есть проект элемента управления ActiveX, который подклассует элемент управления Windows.

#### <a name="to-add-a-bindable-getset-method-using-the-add-property-wizard"></a>Добавление привязываемого метода Get/Set с помощью мастера добавления свойства

1. Загрузите проект элемента управления.

1. На странице **Параметры управления** выберите класс окна для подклассного элемента управления. Например, может потребоваться подкласс элемента управления EDIT.

1. Загрузите проект элемента управления.

1. Щелкните правой кнопкой мыши узел интерфейса для своего элемента управления.

   Откроется контекстное меню.

1. В контекстном меню выберите **Добавить** , а затем — **Добавить свойство**.

1. Введите имя свойства в поле **имя свойства** . Используйте `MyProp` для этого примера.

1. Выберите тип данных из раскрывающегося списка **тип свойства** . Используйте **`short`** для этого примера.

1. В поле **Тип реализации**выберите **Методы Get/Set**.

1. Установите следующие флажки на вкладке атрибуты IDL: **привязываемые**, **рекуестедит**, **дисплайбинд**и **дефаултбинд** , чтобы добавить атрибуты в определение свойства в проекте. IDL-файл. Эти атрибуты делают элемент управления видимым для пользователя и делают свойство «акции» доступным для привязки по умолчанию.

1. Нажмите кнопку **Готово**.

1. Измените текст `SetMyProp` функции, чтобы он содержал следующий код:

   [!code-cpp[NVC_MFC_AxData#2](codesnippet/cpp/mfc-activex-controls-using-data-binding-in-an-activex-control_2.cpp)]

1. Параметр, передаваемый `BoundPropertyChanged` `BoundPropertyRequestEdit` функциям и, является идентификатором DISPID свойства, который является параметром, передаваемым атрибуту ID () для свойства в. IDL-файл.

1. Измените функцию [OnOcmCommand](mfc-activex-controls-subclassing-a-windows-control.md) , чтобы она содержала следующий код:

   [!code-cpp[NVC_MFC_AxData#1](codesnippet/cpp/mfc-activex-controls-using-data-binding-in-an-activex-control_1.cpp)]

1. Измените `OnDraw` функцию таким образом, чтобы она содержала следующий код:

   [!code-cpp[NVC_MFC_AxData#3](codesnippet/cpp/mfc-activex-controls-using-data-binding-in-an-activex-control_3.cpp)]

1. В общедоступном разделе файла заголовка файл заголовка для класса элемента управления добавьте следующие определения (конструкторы) для переменных-членов:

   [!code-cpp[NVC_MFC_AxData#4](codesnippet/cpp/mfc-activex-controls-using-data-binding-in-an-activex-control_4.h)]

1. В последней строке функции сделайте следующее `DoPropExchange` :

   [!code-cpp[NVC_MFC_AxData#5](codesnippet/cpp/mfc-activex-controls-using-data-binding-in-an-activex-control_5.cpp)]

1. Измените `OnResetState` функцию таким образом, чтобы она содержала следующий код:

   [!code-cpp[NVC_MFC_AxData#6](codesnippet/cpp/mfc-activex-controls-using-data-binding-in-an-activex-control_6.cpp)]

1. Измените `GetMyProp` функцию таким образом, чтобы она содержала следующий код:

   [!code-cpp[NVC_MFC_AxData#7](codesnippet/cpp/mfc-activex-controls-using-data-binding-in-an-activex-control_7.cpp)]

Теперь можно выполнить сборку проекта, который будет регистрировать элемент управления. При вставке элемента управления в диалоговое окно были добавлены **поля данных** и свойства **источника данных** , и теперь можно выбрать источник данных и поле, которые будут отображаться в элементе управления.

## <a name="see-also"></a>См. также статью

[Элементы управления ActiveX в MFC](mfc-activex-controls.md)
