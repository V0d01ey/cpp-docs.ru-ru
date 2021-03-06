---
title: Управление потоками
description: Общие сведения о работе с потоками в библиотеке времени выполнения Microsoft C.
ms.date: 11/04/2016
ms.topic: conceptual
f1_keywords:
- Controlling Streams
helpviewer_keywords:
- streams, controlling
- controlling streams
- streams
ms.assetid: 267e9013-9afc-45f6-91e3-ca093230d9d9
ms.openlocfilehash: 0caa9eca7c960acbb581358c1a92afcc6a8af066
ms.sourcegitcommit: 9451db8480992017c46f9d2df23fb17b503bbe74
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91589722"
---
# <a name="controlling-streams"></a>Управление потоками

Функция [fopen](../c-runtime-library/reference/fopen-wfopen.md) возвращает адрес объекта с типом `FILE`. Этот адрес используется в качестве аргумента `stream` в нескольких библиотечных функциях для выполнения различных операций с открытым файлом. Для байтового потока ввод выполняется так, как если бы каждый символ считывался с помощью функции [fgetc](../c-runtime-library/reference/fgetc-fgetwc.md), а вывод — как если бы каждый символ записывался с помощью функции [fputc](../c-runtime-library/reference/fputc-fputwc.md). Для потока расширенных символов ввод выполняется так, как если бы каждый символ считывался с помощью функции [fgetwc](../c-runtime-library/reference/fgetc-fgetwc.md), а вывод — как если бы каждый символ записывался с помощью функции [fputwc](../c-runtime-library/reference/fputc-fputwc.md).

Можно закрыть файл, вызвав функцию [fclose](../c-runtime-library/reference/fclose-fcloseall.md), после чего адрес объекта `FILE` становится недействительным.

Объект `FILE` сохраняет состояние потока, в том числе:

- Индикатор ошибки, установленный в ненулевое значение функцией, обнаружившей ошибку чтения или записи.

- Индикатор конца файла, установленный в ненулевое значение функцией, встретившей конец файла при чтении.

- Индикатор позиции файла определяет следующий байт в потоке для чтения или записи, если файл поддерживает запросы размещения.

- [Состояние потока](../c-runtime-library/stream-states.md) сообщает, доступен ли поток для чтения и (или) записи, а также является ли поток непривязанным, байтовым или расширенным символьным.

- Состояние преобразования запоминает состояние любого частично собранного или созданного обобщенного многобайтового символа, а также все состояния сдвига для последовательности байтов в файле.

- Файловый буфер указывает адрес и размер объекта массива, который может использоваться библиотечными функциями для повышения производительности операций чтения и записи потока.

Не изменяйте никакие значения, хранящиеся в объекте `FILE` или файловом буфере, указанном для использования с этим объектом. Невозможно скопировать объект `FILE` и переносимо использовать адрес копии в качестве аргумента `stream` библиотечной функции.

## <a name="see-also"></a>См. также

[Файлы и потоки](../c-runtime-library/files-and-streams.md)
