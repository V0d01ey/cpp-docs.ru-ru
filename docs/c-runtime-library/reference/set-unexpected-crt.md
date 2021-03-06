---
title: set_unexpected (CRT)
ms.date: 11/04/2016
api_name:
- set_unexpected
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
api_type:
- DLLExport
topic_type:
- apiref
f1_keywords:
- set_unexpected
helpviewer_keywords:
- set_unexpected function
- unexpected function
- exception handling, termination
ms.assetid: ebcef032-4771-48e5-88aa-2a1ab8750aa6
ms.openlocfilehash: f05eab14a53c8abc119a8014d5ac99dc076a9c25
ms.sourcegitcommit: 1f009ab0f2cc4a177f2d1353d5a38f164612bdb1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/27/2020
ms.locfileid: "87226168"
---
# <a name="set_unexpected-crt"></a>set_unexpected (CRT)

Устанавливает собственную функцию завершения, которая будет вызываться **unexpected**.

## <a name="syntax"></a>Синтаксис

```cpp
unexpected_function set_unexpected( unexpected_function unexpFunction );
```

### <a name="parameters"></a>Параметры

*унекспфунктион*<br/>
Указатель на функцию, которую вы пишете для замены **непредвиденной** функции.

## <a name="return-value"></a>Возвращаемое значение

Возвращает указатель на предыдущую функцию завершения, зарегистрированную **_set_unexpected** , чтобы предыдущую функцию можно было восстановить позже. Если Предыдущая функция не задана, то возвращаемое значение может использоваться для восстановления поведения по умолчанию; Это значение может быть **равно NULL**.

## <a name="remarks"></a>Remarks

Функция **set_unexpected** устанавливает *унекспфунктион* как **непредвиденно**вызванная функция. в текущей реализации обработки исключений C++ **не используется.** Тип **unexpected_function** ОПРЕДЕЛЕН в EH. H в качестве указателя на определяемую пользователем непредвиденную функцию, *унекспфунктион* , возвращающую **`void`** . Пользовательская функция *унекспфунктион* не должна возвращаться к ее вызывающему объекту.

```cpp
typedef void ( *unexpected_function )( );
```

По умолчанию **непредвиденные** вызовы **завершаются**. Это поведение по умолчанию можно изменить, написав собственную функцию завершения и вызвав **set_unexpected** с именем функции в качестве аргумента. **непредвиденный** вызов последней функции, заданной в качестве аргумента для **set_unexpected**.

В отличие от пользовательской функции завершения, установленной путем вызова **set_terminate**, исключение может быть вызвано из *унекспфунктион*.

В многопоточной среде непредвиденные функции поддерживаются отдельно для каждого потока. Каждый новый поток требует установки собственной непредвиденной функции. Таким образом, каждый поток отвечает за собственную обработку непредвиденных функций.

В текущей реализации Майкрософт обработки исключений C++ **непредвиденные** вызовы **завершаются** по умолчанию и никогда не вызываются библиотекой времени выполнения, обрабатывающей исключения. Нет каких-либо преимуществ для вызова **непредвиденных** вызовов, а не для **завершения**.

Для всех динамически связанных библиотек DLL или exe существует один обработчик **set_unexpected** ; даже при вызове **set_unexpected** обработчик можно заменить другим или заменить обработчик, заданный другим DLL или exe.

## <a name="requirements"></a>Требования

|Подпрограмма|Обязательный заголовок|
|-------------|---------------------|
|**set_unexpected**|\<eh.h>|

Дополнительные сведения о совместимости см. в статье [Compatibility](../../c-runtime-library/compatibility.md).

## <a name="see-also"></a>См. также статью

[Подпрограммы обработки исключений](../../c-runtime-library/exception-handling-routines.md)<br/>
[abort](abort.md)<br/>
[_get_unexpected](get-unexpected.md)<br/>
[set_terminate](set-terminate-crt.md)<br/>
[заканчива](terminate-crt.md)<br/>
[известно](unexpected-crt.md)<br/>
