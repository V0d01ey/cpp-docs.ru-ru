---
title: Структура ATL_DRAWINFO
ms.date: 11/04/2016
f1_keywords:
- ATL::ATL_DRAWINFO
- ATL_DRAWINFO
- ATL.ATL_DRAWINFO
helpviewer_keywords:
- ATL_DRAWINFO structure
ms.assetid: dd2e2aa8-e8c5-403b-b4df-35c0f6f57fb7
ms.openlocfilehash: 00d93b3dd8b060a21b6ff4083bb9880d8d836a19
ms.sourcegitcommit: 2bc15c5b36372ab01fa21e9bcf718fa22705814f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2020
ms.locfileid: "82168622"
---
# <a name="atl_drawinfo-structure"></a>Структура ATL_DRAWINFO

Содержит сведения, используемые для подготовки к просмотру различных целевых объектов, таких как принтер, метафайл или элемент управления ActiveX.

## <a name="syntax"></a>Синтаксис

```cpp
struct ATL_DRAWINFO {
    UINT cbSize;
    DWORD dwDrawAspect;
    LONG lindex;
    DVTARGETDEVICE* ptd;
    HDC hicTargetDev;
    HDC hdcDraw;
    LPCRECTL prcBounds;
    LPCRECTL prcWBounds;
    BOOL bOptimize;
    BOOL bZoomed;
    BOOL bRectInHimetric;
    SIZEL ZoomNum;
    SIZEL ZoomDen;
};
```

## <a name="members"></a>Участники

`cbSize`<br/>
Размер структуры в байтах.

`dwDrawAspect`<br/>
Указывает способ представления целевого объекта. Представления могут включать содержимое, значок, эскиз или печатный документ. Список возможных значений см. в разделе [дваспект](/windows/win32/api/wtypes/ne-wtypes-dvaspect) and [DVASPECT2](/windows/win32/api/ocidl/ne-ocidl-dvaspect2).

`lindex`<br/>
Часть целевого объекта, представляющая интерес для операции рисования. Его интерпретация зависит от значения в `dwDrawAspect` элементе.

`ptd`<br/>
Указатель на структуру [двтаржетдевице](/windows/win32/api/objidl/ns-objidl-dvtargetdevice) , которая включает оптимизацию отрисовки в зависимости от указанного аспекта. Обратите внимание, что новые объекты и контейнеры, поддерживающие оптимизированные интерфейсы рисования, также поддерживают этот элемент. Более старые объекты и контейнеры, которые не поддерживают оптимизированные интерфейсы рисования, всегда указывают значение NULL для этого элемента.

`hicTargetDev`<br/>
Информационный контекст для целевого устройства, на который указывает `ptd` объект, который может извлекать метрики устройства и тестировать возможности устройства. Если `ptd` параметр имеет значение null, объект должен игнорировать значение в `hicTargetDev` элементе.

`hdcDraw`<br/>
Контекст устройства для рисования. Для объекта без окон `hdcDraw` элемент находится в режиме `MM_TEXT` сопоставления с логическими координатами, соответствующими клиентским координатам содержащего окна. Кроме того, контекст устройства должен находиться в том же состоянии, что и то, что обычно передается `WM_PAINT` сообщением.

`prcBounds`<br/>
Указатель на структуру [Rect](/windows/win32/api/windef/ns-windef-rectl) , указывающую прямоугольник на `hdcDraw` и, в котором должен быть нарисован объект. Этот элемент управляет положением и растяжением объекта. Этот элемент должен иметь значение NULL, чтобы нарисовать неоконный активный объект на месте. В любой другой ситуации значение NULL не является допустимым значением и должно приводить к `E_INVALIDARG` созданию кода ошибки. Если контейнер передает значение, отличное от NULL, в безоконный объект, объект должен отобразить запрошенный аспект в указанном контексте устройства и прямоугольнике. Контейнер может запрашивать это из безоконного объекта для отображения второго, неактивного представления объекта или для печати объекта.

`prcWBounds`<br/>
Если `hdcDraw` является контекстом устройства-метафайла (см. раздел [жетдевицекапс](/windows/win32/api/wingdi/nf-wingdi-getdevicecaps) в Windows SDK), это указатель на `RECTL` структуру, указывающую ограничивающий прямоугольник в базовом метафайле. Структура прямоугольника содержит размеры окна и исходное положение окна. Эти значения полезны при рисовании метафайлов. Прямоугольник, обозначенный `prcBounds` , вложен в этот `prcWBounds` прямоугольник; они находятся в одном пространстве координат.

`bOptimize`<br/>
Ненулевое значение, если рисунок элемента управления должен быть оптимизирован; в противном случае — значение 0. Если рисунок оптимизирован, состояние контекста устройства автоматически восстанавливается после завершения подготовки к просмотру.

`bZoomed`<br/>
Ненулевое значение, если целевой объект имеет коэффициент масштабирования, в противном случае — 0. Коэффициент масштабирования хранится в `ZoomNum`.

`bRectInHimetric`<br/>
Ненулевое значение, `prcBounds` если размеры находятся в HIMETRIC, в противном случае — 0.

`ZoomNum`<br/>
Ширина и высота прямоугольника, в котором отображается объект. Коэффициент масштабирования по оси x (доля естественного размера объекта до его текущего экстента) является значением, `ZoomNum.cx` деленным на значение. `ZoomDen.cx` Коэффициент масштабирования вдоль оси y достигается аналогичным образом.

`ZoomDen`<br/>
Фактическая ширина и высота целевого объекта.

## <a name="remarks"></a>Remarks

Типичное использование этой структуры — получение информации во время отрисовки целевого объекта. Например, можно извлечь значения из ATL_DRAWINFO в перегрузке метода [ккомконтролбасе:: ондравадванцед](ccomcontrolbase-class.md#ondrawadvanced).

В этой структуре хранятся соответствующие сведения, используемые для визуализации внешнего вида объекта для целевого устройства. Предоставленные сведения можно использовать для рисования на экране, на принтере или даже в метафайле.

## <a name="requirements"></a>Требования

**Заголовок:** атлктл. h

## <a name="see-also"></a>См. также

[Классы и структуры](../../atl/reference/atl-classes.md)<br/>
[Ивиевобжект::D RAW](/windows/win32/api/oleidl/nf-oleidl-iviewobject-draw)<br/>
[CComControlBase::OnDrawAdvanced](../../atl/reference/ccomcontrolbase-class.md#ondrawadvanced)
