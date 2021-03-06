---
title: Проверка параметров
description: Описание проверки параметров в библиотеке времени выполнения Microsoft C.
ms.date: 11/04/2016
ms.topic: conceptual
helpviewer_keywords:
- parameters, validation
ms.assetid: 019dd5f0-dc61-4d2e-b4e9-b66409ddf1f2
ms.openlocfilehash: 8378e4bf9bdfc950002c3ed8c3ef50c27a3c162d
ms.sourcegitcommit: 30792632548d1c71894f9fecbe2f554294b86020
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/06/2020
ms.locfileid: "91765255"
---
# <a name="parameter-validation"></a>Проверка параметров

Большинство функций CRT с повышенной безопасностью и многие из них не проверяют их параметры, например проверяя указатели на **null**, целые числа попадают в допустимый диапазон или значения перечисления являются допустимыми. Если обнаружен недопустимый параметр, вызывается обработчик недопустимых параметров.

## <a name="invalid-parameter-handler-routine"></a>Подпрограмма обработчика недопустимых параметров

Если функция библиотеки времени выполнения C обнаруживает недопустимый параметр, она фиксирует некоторые сведения об ошибке, а затем вызывает макрос, который создает оболочку для функции диспетчеризации обработчика недопустимых параметров. Это может быть один из [_invalid_parameter](../c-runtime-library/reference/invalid-parameter-functions.md), [_invalid_parameter_noinfo](../c-runtime-library/reference/invalid-parameter-functions.md)или [_invalid_parameter_noinfo_noreturn](../c-runtime-library/reference/invalid-parameter-functions.md). Вызов функции диспетчеризации зависит от того, является ли ваш код соответственно, отладочная сборка, розничная сборка или ошибка не считаются восстанавливаемыми.

В отладочных сборках макрос недопустимых параметров обычно вызывает неудачное утверждение и точку останова отладчика перед вызовом функции диспетчеризации. При выполнении кода утверждение может быть сообщено пользователю в диалоговом окне с параметрами "прервать", "повторить" и "продолжить" или аналогичным образом в зависимости от версии операционной системы и библиотеки среды выполнения. Эти параметры позволяют пользователю немедленно завершить программу, подключить отладчик или продолжить выполнение существующего кода, который вызывает функцию диспетчеризации.

Функция диспетчеризации обработчика недопустимого параметра вызывает текущий назначенный обработчик недопустимых параметров. По умолчанию вызовы недопустимых параметров `_invoke_watson` , что приводит к закрытию и созданию мини-дампа приложения. Если параметр включен операционной системой, диалоговое окно запрашивает у пользователя, хочет ли он отправить аварийный дамп в корпорацию Майкрософт для анализа.

Это поведение можно изменить с помощью функций [_set_invalid_parameter_handler](../c-runtime-library/reference/set-invalid-parameter-handler-set-thread-local-invalid-parameter-handler.md) или [_set_thread_local_invalid_parameter_handler](../c-runtime-library/reference/set-invalid-parameter-handler-set-thread-local-invalid-parameter-handler.md) , чтобы задать для обработчика недопустимых параметров собственную функцию. Если указанная вами функция не завершает работу приложения, управление возвращается к функции, которая получила недопустимые параметры. В CRT эти функции обычно останавливают выполнение функций, применяют `errno` код ошибки и возвращают код ошибки. Во многих случаях `errno` значение и возвращаемое значение `EINVAL` указываются и для указания недопустимого параметра. В некоторых случаях возвращается более конкретный код ошибки, например `EBADF`, соответствующий указателю на неправильный файл, переданному как параметр.

Дополнительные сведения о параметрах `errno` см. в разделе [errno, _doserrno, _sys_errlist и _sys_nerr](../c-runtime-library/errno-doserrno-sys-errlist-and-sys-nerr.md).

## <a name="see-also"></a>См. также раздел

[Функции безопасности в CRT](../c-runtime-library/security-features-in-the-crt.md)\
[Возможности библиотеки CRT](../c-runtime-library/crt-library-features.md)
