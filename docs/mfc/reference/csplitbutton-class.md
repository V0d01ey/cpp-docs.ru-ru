---
title: Класс CSplitButton
ms.date: 11/19/2018
f1_keywords:
- CSplitButton
- AFXCMN/CSplitButton
- AFXCMN/CSplitButton::CSplitButton
- AFXCMN/CSplitButton::Create
- AFXCMN/CSplitButton::SetDropDownMenu
- AFXCMN/CSplitButton::OnDropDown
helpviewer_keywords:
- CSplitButton [MFC], CSplitButton
- CSplitButton [MFC], Create
- CSplitButton [MFC], SetDropDownMenu
- CSplitButton [MFC], OnDropDown
ms.assetid: 6844d0a9-6408-4e44-9b5f-57628ed8bad6
ms.openlocfilehash: 484cef2787c9e5c166a7b20b017251b559d7221c
ms.sourcegitcommit: 1839405b97036891b6e4d37c99def044d6f37eff
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/18/2020
ms.locfileid: "88562549"
---
# <a name="csplitbutton-class"></a>Класс CSplitButton

`CSplitButton`Класс представляет элемент управления "кнопка разворачивающейся кнопки". Элемент управления "кнопка разделения" реализует поведение по умолчанию, когда пользователь щелкает основную часть кнопки, и отображает раскрывающееся меню, когда пользователь щелкает раскрывающуюся стрелку кнопки.

## <a name="syntax"></a>Синтаксис

```
class CSplitButton : public CButton
```

## <a name="members"></a>Участники

### <a name="public-constructors"></a>Открытые конструкторы

|name|Описание|
|----------|-----------------|
|[CSplitButton:: CSplitButton](#csplitbutton)|Формирует объект `CSplitButton`.|

### <a name="public-methods"></a>Открытые методы

|name|Описание|
|----------|-----------------|
|[CSplitButton:: Create](#create)|Создает элемент управления "разворачивающаяся кнопка" с указанными стилями и прикрепляет его к текущему `CSplitButton` объекту.|
|[CSplitButton:: Сетдропдовнмену](#setdropdownmenu)|Задает раскрывающееся меню, отображаемое, когда пользователь щелкает стрелку раскрывающегося списка текущего элемента управления "кнопка разделения".|

### <a name="protected-methods"></a>Защищенные методы

|Имя|Описание|
|----------|-----------------|
|[CSplitButton:: OnDropDown](#ondropdown)|Обрабатывает BCN_DROPDOWN уведомление о том, что система отправляет сообщение, когда пользователь щелкает стрелку раскрывающегося списка текущего элемента управления "кнопка разворачивающейся кнопки".|

## <a name="remarks"></a>Remarks

`CSplitButton`Класс является производным от класса [кбуттон](../../mfc/reference/cbutton-class.md) . Элемент управления "разворачивающаяся кнопка" — это элемент управления "Кнопка", стиль которого BS_SPLITBUTTON. Он отображает пользовательское меню, когда пользователь щелкает стрелку раскрывающегося списка. Дополнительные сведения см. в статьях стили BS_SPLITBUTTON и BS_DEFSPLITBUTTON в разделе [стили кнопок](/windows/win32/Controls/button-styles).

На следующем рисунке показано диалоговое окно, содержащее элемент управления страничный навигатор и элемент управления (1) разворачивающаяся кнопка. Стрелка раскрывающегося списка (2) уже была нажата, и отображается подменю (3).

![Диалоговое окно с кнопкой-разделителем и элементом управления страничного навигатора.](../../mfc/reference/media/splitbutton_pager.png "Диалоговое окно с кнопкой-разделителем и элементом управления страничного навигатора.")

## <a name="inheritance-hierarchy"></a>Иерархия наследования

[CObject](../../mfc/reference/cobject-class.md)

[CCmdTarget](../../mfc/reference/ccmdtarget-class.md)

[CWnd](../../mfc/reference/cwnd-class.md)

[CButton](../../mfc/reference/cbutton-class.md)

`CSplitButton`

## <a name="requirements"></a>Требования

**Заголовок:** afxcmn.h

Этот класс поддерживается в Windows Vista и более поздних версиях.

Дополнительные требования для этого класса описаны в статье [требования к сборке для стандартных элементов управления Windows Vista](../../mfc/build-requirements-for-windows-vista-common-controls.md).

## <a name="csplitbuttoncreate"></a><a name="create"></a> CSplitButton:: Create

Создает элемент управления "разворачивающаяся кнопка" с указанными стилями и прикрепляет его к текущему `CSplitButton` объекту.

```
virtual BOOL Create(
    DWORD dwStyle,
    const RECT& rect,
    CWnd* pParentWnd,
    UINT nID);
```

### <a name="parameters"></a>Параметры

*двстиле*\
окне Побитовое сочетание (или) стилей, применяемых к элементу управления. Дополнительные сведения см. в разделе [стили кнопок](../../mfc/reference/styles-used-by-mfc.md#button-styles).

*перетаскиваемые*\
окне Ссылка на структуру [Rect](/windows/win32/api/windef/ns-windef-rect) , которая содержит расположение и размер элемента управления.

*ппарентвнд*\
окне Указатель, отличный от NULL, на объект [CWnd](../../mfc/reference/cwnd-class.md) , который является родительским окном элемента управления.

*nID*\
окне Идентификатор элемента управления.

### <a name="return-value"></a>Возвращаемое значение

Значение TRUE, если этот метод успешно выполнен; в противном случае — значение FALSE.

## <a name="csplitbuttoncsplitbutton"></a><a name="csplitbutton"></a> CSplitButton:: CSplitButton

Формирует объект `CSplitButton`. Параметры конструктора указывают подменю, которое отображается, когда пользователь щелкает стрелку раскрывающегося списка элемента управления "разворачивающаяся кнопка".

```
CSplitButton();

CSplitButton(
    UINT nMenuId,
    UINT nSubMenuId)
CSplitButton(CMenu* pMenu)
```

### <a name="parameters"></a>Параметры

*нменуид*\
окне Идентификатор ресурса строки меню.

*нсубменуид*\
окне Идентификатор ресурса подменю.

*пмену*\
окне Указатель на объект [кмену](../../mfc/reference/cmenu-class.md) , указывающий подменю. `CSplitButton`Объект удаляет `CMenu` объект и связанный с ним HMENU, когда `CSplitButton` объект выходит из области действия.

### <a name="remarks"></a>Remarks

Используйте метод [CSplitButton:: Create](#create) , чтобы создать элемент управления "разворачивающаяся кнопка" и присоединить его к `CSplitButton` объекту.

## <a name="csplitbuttonondropdown"></a><a name="ondropdown"></a> CSplitButton:: OnDropDown

Обрабатывает BCN_DROPDOWN уведомление о том, что система отправляет сообщение, когда пользователь щелкает стрелку раскрывающегося списка текущего элемента управления "кнопка разворачивающейся кнопки".

```
afx_msg void OnDropDown(
    NMHDR* pNMHDR,
    LRESULT* pResult);
```

### <a name="parameters"></a>Параметры

*пнмхдр*\
окне Указатель на структуру [NMHDR](/windows/win32/api/richedit/ns-richedit-nmhdr) , содержащую сведения о [BCN_DROPDOWN](/windows/win32/Controls/bcn-dropdown) уведомлении.

*пресулт*\
заполняет (Не используется; значение не возвращается.) Возвращаемое значение уведомления [BCN_DROPDOWN](/windows/win32/Controls/bcn-dropdown) .

### <a name="remarks"></a>Remarks

Когда пользователь щелкает стрелку раскрывающегося списка на элементе управления "разворачивающаяся кнопка", система отправляет BCN_DROPDOWN сообщение уведомления, которое `OnDropDown` обрабатывает метод. Однако объект не `CSplitButton` пересылает BCN_DROPDOWN уведомление элементу управления, содержащему элемент управления "разворачивающаяся кнопка". Следовательно, вмещающий элемент управления не может поддерживать пользовательское действие в ответ на уведомление.

Чтобы реализовать пользовательское действие, поддерживаемое содержащим элементом управления, используйте объект [кбуттон](../../mfc/reference/cbutton-class.md) со стилем BS_SPLITBUTTON вместо `CSplitButton` объекта. Затем реализуйте обработчик для уведомления BCN_DROPDOWN в `CButton` объекте. Дополнительные сведения см. в разделе [стили кнопок](../../mfc/reference/styles-used-by-mfc.md#button-styles).

Для реализации настраиваемого действия, которое поддерживает элемент управления "разворачивающаяся кнопка", используйте [отражение сообщений](../../mfc/tn062-message-reflection-for-windows-controls.md). Создайте класс, производный от `CSplitButton` класса, и присвойте ему имя, например кмисплитбуттон. Затем добавьте следующую схему сообщения в приложение для работы с уведомлением BCN_DROPDOWN:

```
BEGIN_MESSAGE_MAP(CMySplitButton,
    CSplitButton)
    ON_NOTIFY_REFLECT(BCN_DROPDOWN, &CMySplitButton::OnDropDown)
END_MESSAGE_MAP()
```

## <a name="csplitbuttonsetdropdownmenu"></a><a name="setdropdownmenu"></a> CSplitButton:: Сетдропдовнмену

Задает раскрывающееся меню, отображаемое, когда пользователь щелкает стрелку раскрывающегося списка текущего элемента управления "кнопка разделения".

```cpp
void SetDropDownMenu(
    UINT nMenuId,
    UINT nSubMenuId);

void SetDropDownMenu(CMenu* pMenu);
```

### <a name="parameters"></a>Параметры

*нменуид*\
окне Идентификатор ресурса строки меню.

*нсубменуид*\
окне Идентификатор ресурса подменю.

*пмену*\
окне Указатель на объект [кмену](../../mfc/reference/cmenu-class.md) , указывающий подменю. `CSplitButton`Объект удаляет `CMenu` объект и связанный с ним HMENU, когда `CSplitButton` объект выходит из области действия.

### <a name="remarks"></a>Remarks

Параметр *нменуид* определяет строку меню, которая представляет собой горизонтальный список элементов строки меню. Параметр *нсубменуид* — это номер индекса, начинающийся с нуля, который определяет подменю, которое представляет собой раскрывающийся список пунктов меню, связанных с каждым элементом строки меню. Например, типичное приложение содержит меню, содержащее элементы строки меню "файл", "Изменить" и "Справка". Элемент строки меню "файл" содержит подменю, содержащее пункты меню "Открыть", "Закрыть" и "выход". При щелчке стрелки раскрывающегося списка элемента управления "разворачивающаяся кнопка" элемент управления отображает указанное подменю, а не строку меню.

На следующем рисунке показано диалоговое окно, содержащее элемент управления страничный навигатор и элемент управления (1) разворачивающаяся кнопка. Стрелка раскрывающегося списка (2) уже была нажата, и отображается подменю (3).

![Диалоговое окно с кнопкой-разделителем и элементом управления страничного навигатора.](../../mfc/reference/media/splitbutton_pager.png "Диалоговое окно с кнопкой-разделителем и элементом управления страничного навигатора.")

### <a name="example"></a>Пример

Первый оператор в следующем примере кода демонстрирует метод [CSplitButton:: сетдропдовнмену](#setdropdownmenu) . Мы создали меню с помощью редактора ресурсов Visual Studio, который автоматически назывался ИДЕНТИФИКАТОРом строки меню IDR_MENU1. Параметр *нсубменуид* , который равен нулю, ссылается на единственное подменю строки меню.

[!code-cpp[NVC_MFC_CSplitButton_s2#1](../../mfc/reference/codesnippet/cpp/csplitbutton-class_1.cpp)]

## <a name="see-also"></a>См. также

[Класс CSplitButton](../../mfc/reference/csplitbutton-class.md)<br/>
[Иерархическая диаграмма](../../mfc/hierarchy-chart.md)<br/>
[Класс Кбуттон](../../mfc/reference/cbutton-class.md)
