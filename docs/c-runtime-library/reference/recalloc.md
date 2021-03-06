---
title: _recalloc
ms.date: 4/2/2020
api_name:
- _recalloc
- _o__recalloc
api_location:
- msvcrt.dll
- msvcr80.dll
- msvcr90.dll
- msvcr100.dll
- msvcr100_clr0400.dll
- msvcr110.dll
- msvcr110_clr0400.dll
- msvcr120.dll
- msvcr120_clr0400.dll
- ucrtbase.dll
- api-ms-win-crt-heap-l1-1-0.dll
- api-ms-win-crt-private-l1-1-0.dll
api_type:
- DLLExport
topic_type:
- apiref
f1_keywords:
- _recalloc
- recalloc
helpviewer_keywords:
- _recalloc function
- recalloc function
ms.assetid: 1db8305a-3f03-418c-8844-bf9149f63046
ms.openlocfilehash: 45f483bcaa397969a81097768ebbd1ed4cda288b
ms.sourcegitcommit: 1f009ab0f2cc4a177f2d1353d5a38f164612bdb1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/27/2020
ms.locfileid: "87226194"
---
# <a name="_recalloc"></a>_recalloc

Сочетание **перераспределения** и **calloc**. Перераспределяет массив в памяти и инициализирует его элементы значением 0.

## <a name="syntax"></a>Синтаксис

```C
void *_recalloc(
   void *memblock
   size_t num,
   size_t size
);
```

### <a name="parameters"></a>Параметры

*memblock*<br/>
Указатель на ранее выделенный блок памяти.

*number*<br/>
Число элементов.

*size*<br/>
Длина каждого элемента в байтах.

## <a name="return-value"></a>Возвращаемое значение

**_recalloc** возвращает **`void`** указатель на перераспределенный (и, возможно, перемещенный) блок памяти.

Если недостаточно памяти для расширения блока до заданного размера, исходный блок остается без изменений и возвращается **значение NULL** .

Если запрошенный размер равен нулю, то блок, на который указывает *мемблокк* , освобождается; Возвращаемое значение равно **null**, и *мемблокк* остается указывающим на освобожденный блок.

Возвращаемое значение указывает на пространство хранилища, которое гарантированно будет соответственно выровнено для хранения объектов любого типа. Чтобы получить указатель на тип, отличный от **`void`** , используйте приведение типа к возвращаемому значению.

## <a name="remarks"></a>Remarks

Функция **_recalloc** изменяет размер выделенного блока памяти. Аргумент *мемблокк* указывает на начало блока памяти. Если *мемблокк* имеет **значение NULL**, **_recalloc** ведет себя так же, как [calloc](calloc.md) , и выделяет новый блок из байт *number*  *  *размера* числа. Каждый элемент инициализируется значением 0. Если *мемблокк* не равно **null**, это должен быть указатель, возвращенный предыдущим вызовом метода **calloc**, [malloc](malloc.md)или [realloc](realloc.md).

Так как новый блок может находиться в новом месте в памяти, указатель, возвращаемый **_recalloc** , не гарантирует, что указатель передается через аргумент *мемблокк* .

**_recalloc** устанавливает **значение** **еномем** , если выделение памяти завершается ошибкой, или если объем запрашиваемой памяти превышает **_HEAP_MAXREQ**. Дополнительные сведения об этих и других кодах ошибок см. в разделе [errno, _doserrno, _sys_errlist и _sys_nerr](../../c-runtime-library/errno-doserrno-sys-errlist-and-sys-nerr.md).

**рекаллок** вызывает **realloc** переопределение, чтобы использовать функцию C++ [_set_new_mode](set-new-mode.md) для установки нового режима обработчика. Новый режим обработчика указывает, следует ли при сбое **перераспределить** вызывать новую подпрограммы обработчика, заданную [_set_new_handler](set-new-handler.md). По умолчанию **перераспределение не вызывает** новую подпрограммы обработчика при сбое выделения памяти. Это поведение по умолчанию можно переопределить таким **образом, чтобы** , когда **_recalloc** не удается выделить память, перераспределение вызывает новую подпрограммы обработчика таким же образом, как и **`new`** оператор, когда он завершается неудачей по той же причине. Чтобы переопределить значение по умолчанию, вызовите

```C
_set_new_mode(1);
```

на ранних этапах программы или компонуйте с использованием NEWMODE.OBJ.

Если приложение связано с отладочной версией библиотек времени выполнения C, **_recalloc** разрешается в [_recalloc_dbg](recalloc-dbg.md). Дополнительные сведения об управлении кучей в процессе отладки см. в разделе [Куча отладки CRT](/visualstudio/debugger/crt-debug-heap-details).

**_recalloc** помечается `__declspec(noalias)` и `__declspec(restrict)` , что означает, что функция гарантированно не изменяет глобальные переменные и что возвращаемый указатель не имеет псевдонима. Дополнительные сведения см. в разделах [noalias](../../cpp/noalias.md) и [restrict](../../cpp/restrict.md).

По умолчанию глобальное состояние этой функции ограничивается приложением. Чтобы изменить это, см. раздел [глобальное состояние в CRT](../global-state.md).

## <a name="requirements"></a>Требования

|Подпрограмма|Обязательный заголовок|
|-------------|---------------------|
|**_recalloc**|\<stdlib.h> и \<malloc.h>|

Дополнительные сведения о совместимости см. в статье [Compatibility](../../c-runtime-library/compatibility.md).

## <a name="see-also"></a>См. также статью

[Выделение памяти](../../c-runtime-library/memory-allocation.md)<br/>
[_recalloc_dbg](recalloc-dbg.md)<br/>
[_aligned_recalloc](aligned-recalloc.md)<br/>
[_aligned_offset_recalloc](aligned-offset-recalloc.md)<br/>
[свободный](free.md)<br/>
[Параметры ссылок](../../c-runtime-library/link-options.md)<br/>
