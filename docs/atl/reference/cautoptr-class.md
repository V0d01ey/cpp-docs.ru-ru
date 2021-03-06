---
title: Класс Каутоптр
ms.date: 11/04/2016
f1_keywords:
- CAutoPtr
- ATLBASE/ATL::CAutoPtr
- ATLBASE/ATL::CAutoPtr::CAutoPtr
- ATLBASE/ATL::CAutoPtr::Attach
- ATLBASE/ATL::CAutoPtr::Detach
- ATLBASE/ATL::CAutoPtr::Free
- ATLBASE/ATL::CAutoPtr::m_p
helpviewer_keywords:
- CAutoPtr class
ms.assetid: 08988d53-4fb0-4711-bdfc-8ac29c63f410
ms.openlocfilehash: 699e62362bc74009e3faed3b4fd66b579c9c4cd3
ms.sourcegitcommit: 1f009ab0f2cc4a177f2d1353d5a38f164612bdb1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/27/2020
ms.locfileid: "87226663"
---
# <a name="cautoptr-class"></a>Класс Каутоптр

Этот класс представляет объект интеллектуального указателя.

> [!IMPORTANT]
> Этот класс и его члены не могут использоваться в приложениях, выполняемых в среда выполнения Windows.

## <a name="syntax"></a>Синтаксис

```cpp
template <typename T>
class CAutoPtr
```

### <a name="parameters"></a>Параметры

*T*<br/>
Тип указателя.

## <a name="members"></a>Элементы

### <a name="public-constructors"></a>Открытые конструкторы

|Имя|Описание:|
|----------|-----------------|
|[Каутоптр:: Каутоптр](#cautoptr)|Конструктор.|
|[Каутоптр:: ~ Каутоптр](#dtor)|Деструктор|

### <a name="public-methods"></a>Открытые методы

|name|Описание:|
|----------|-----------------|
|[Каутоптр:: Attach](#attach)|Вызовите этот метод, чтобы стать владельцем существующего указателя.|
|[Каутоптр::D етач](#detach)|Вызовите этот метод, чтобы освободить владение указателем.|
|[Каутоптр:: Free](#free)|Вызовите этот метод, чтобы удалить объект, на который указывает `CAutoPtr` .|

### <a name="public-operators"></a>Открытые операторы

|Имя|Описание:|
|----------|-----------------|
|[Каутоптр:: operator T *](#operator_t_star)|Оператор CAST.|
|[Каутоптр:: operator =](#operator_eq)|Оператор присваивания.|
|[Каутоптр:: operator — >](#operator_ptr)|Оператор указателя на член.|

### <a name="public-data-members"></a>Открытые члены данных

|Имя|Описание:|
|----------|-----------------|
|[Каутоптр:: m_p](#m_p)|Переменная члена данных указателя.|

## <a name="remarks"></a>Remarks

Этот класс предоставляет методы для создания и управления интеллектуальным указателем, что поможет защититься от утечек памяти за счет автоматического освобождения ресурсов, если он выходит за пределы области.

Кроме того, `CAutoPtr` конструктор копий и оператор присваивания передают владение указателем, копируя исходный указатель на указатель назначения и устанавливая указатель источника на null. Поэтому невозможно иметь два `CAutoPtr` объекта, каждый из которых хранит один и тот же указатель, и это уменьшает вероятность того, что один и тот же указатель дважды удаляется.

`CAutoPtr`также упрощает создание коллекций указателей. Вместо того чтобы создавать производный класс коллекции и переопределять деструктор, проще сделать коллекцию `CAutoPtr` объектов. При удалении коллекции `CAutoPtr` объекты выходят за пределы области действия и автоматически удаляют себя.

[Чеапптр](../../atl/reference/cheapptr-class.md) и Variant работают так же `CAutoPtr` , как, за исключением того, что они выделяют и освобождают память с помощью различных функций кучи вместо C++ **`new`** и **`delete`** операторов. [Каутовекторптр](../../atl/reference/cautovectorptr-class.md) похож на `CAutoPtr` , единственная разница заключается в том, что для выделения и освобождения памяти используется **вектор New []** и **векторное удаление []** .

См. также [каутоптраррай](../../atl/reference/cautoptrarray-class.md) и [каутоптрлист](../../atl/reference/cautoptrlist-class.md) , если требуются массивы или списки смарт-указателей.

## <a name="requirements"></a>Требования

**Заголовок:** atlbase. h

## <a name="example"></a>Пример

[!code-cpp[NVC_ATL_Utilities#74](../../atl/codesnippet/cpp/cautoptr-class_1.cpp)]

## <a name="cautoptrattach"></a><a name="attach"></a>Каутоптр:: Attach

Вызовите этот метод, чтобы стать владельцем существующего указателя.

```cpp
void Attach(T* p) throw();
```

### <a name="parameters"></a>Параметры

*ш*<br/>
`CAutoPtr`Объект будет становиться владельцем этого указателя.

### <a name="remarks"></a>Remarks

Когда `CAutoPtr` объект получает владение указателем, он автоматически удаляет указатель и все выделенные данные, когда оно выходит за пределы области. Если вызывается [каутоптр::D етач](#detach) , программист снова получит ответственность за освобождение ресурсов, выделенных для памяти.

В отладочных сборках возникнет ошибка утверждения, если элемент данных [каутоптр:: m_p](#m_p) в данный момент указывает на существующее значение; то есть он не равен NULL.

### <a name="example"></a>Пример

См. пример в [обзоре каутоптр](../../atl/reference/cautoptr-class.md).

## <a name="cautoptrcautoptr"></a><a name="cautoptr"></a>Каутоптр:: Каутоптр

Конструктор.

```cpp
CAutoPtr() throw();
explicit CAutoPtr(T* p) throw();

template<typename TSrc>
CAutoPtr(CAutoPtr<TSrc>& p) throw();

template<>
CAutoPtr(CAutoPtr<T>& p) throw();
```

### <a name="parameters"></a>Параметры

*ш*<br/>
Существующий указатель.

*тсрк*<br/>
Тип, управляемый другим `CAutoPtr` объектом и используемый для инициализации текущего объекта.

### <a name="remarks"></a>Remarks

`CAutoPtr`Объект можно создать с помощью существующего указателя. в этом случае он передает владение указателем.

### <a name="example"></a>Пример

См. пример в [обзоре каутоптр](../../atl/reference/cautoptr-class.md).

## <a name="cautoptrcautoptr"></a><a name="dtor"></a>Каутоптр:: ~ Каутоптр

Деструктор

```cpp
~CAutoPtr() throw();
```

### <a name="remarks"></a>Remarks

Освобождает все выделенные ресурсы. Вызывает [каутоптр:: Free](#free).

## <a name="cautoptrdetach"></a><a name="detach"></a>Каутоптр::D етач

Вызовите этот метод, чтобы освободить владение указателем.

```cpp
T* Detach() throw();
```

### <a name="return-value"></a>Возвращаемое значение

Возвращает копию указателя.

### <a name="remarks"></a>Remarks

Освобождает владение указателем, задает для переменной-члена [каутоптр:: M_P](#m_p) значение NULL и возвращает копию указателя. После вызова `Detach` разработчик может освобождать все выделенные ресурсы, для которых `CAutoPtr` объект мог ранее предположить репонсибилити.

### <a name="example"></a>Пример

См. пример в [обзоре каутоптр](../../atl/reference/cautoptr-class.md).

## <a name="cautoptrfree"></a><a name="free"></a>Каутоптр:: Free

Вызовите этот метод, чтобы удалить объект, на который указывает `CAutoPtr` .

```cpp
void Free() throw();
```

### <a name="remarks"></a>Remarks

Объект, на который указывает, `CAutoPtr` освобождается, а переменная-член [каутоптр:: m_p](#m_p) имеет значение null.

## <a name="cautoptrm_p"></a><a name="m_p"></a>Каутоптр:: m_p

Переменная члена данных указателя.

```cpp
T* m_p;
```

### <a name="remarks"></a>Remarks

Эта переменная члена содержит сведения об указателе.

## <a name="cautoptroperator-"></a><a name="operator_eq"></a>Каутоптр:: operator =

Оператор присваивания.

```cpp
template<>
CAutoPtr<T>& operator= (CAutoPtr<T>& p);

template<typename TSrc>
CAutoPtr<T>& operator= (CAutoPtr<TSrc>& p);
```

### <a name="parameters"></a>Параметры

*ш*<br/>
Указатель.

*тсрк*<br/>
Тип класса.

### <a name="return-value"></a>Возвращаемое значение

Возвращает ссылку на **каутоптр \< T > **.

### <a name="remarks"></a>Remarks

Оператор присваивания отсоединяет `CAutoPtr` объект от любого текущего указателя и присоединяет новый указатель, *p*, на его месте.

### <a name="example"></a>Пример

См. пример в [обзоре каутоптр](../../atl/reference/cautoptr-class.md).

## <a name="cautoptroperator--gt"></a><a name="operator_ptr"></a>Каутоптр:: operator —&gt;

Оператор указателя на член.

```cpp
T* operator->() const throw();
```

### <a name="return-value"></a>Возвращаемое значение

Возвращает значение переменной элемента данных [каутоптр:: m_p](#m_p) .

### <a name="remarks"></a>Remarks

Этот оператор используется для вызова метода в классе, на который указывает `CAutoPtr` объект. В отладочных сборках произойдет сбой утверждения, если `CAutoPtr` указывает на значение null.

### <a name="example"></a>Пример

См. пример в [обзоре каутоптр](../../atl/reference/cautoptr-class.md).

## <a name="cautoptroperator-t"></a><a name="operator_t_star"></a>Каутоптр:: operator T *

Оператор CAST.

```cpp
operator T* () const throw();
```

### <a name="return-value"></a>Возвращаемое значение

Возвращает указатель на тип данных Object, определенный в шаблоне класса.

### <a name="example"></a>Пример

См. пример в [обзоре каутоптр](../../atl/reference/cautoptr-class.md).

## <a name="see-also"></a>См. также статью

[Класс Чеапптр](../../atl/reference/cheapptr-class.md)<br/>
[Класс Каутовекторптр](../../atl/reference/cautovectorptr-class.md)<br/>
[Общие сведения о классах](../../atl/atl-class-overview.md)
