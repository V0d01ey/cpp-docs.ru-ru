---
title: /SUBSYSTEM
ms.date: 11/04/2016
f1_keywords:
- /subsystem_editbin
helpviewer_keywords:
- /SUBSYSTEM editbin option
- -SUBSYSTEM editbin option
- SUBSYSTEM editbin option
ms.assetid: 515e4cdf-3cc4-4659-8764-1f2757b49215
ms.openlocfilehash: 708bfcce3e6d6616116bcc08441f374b46914c82
ms.sourcegitcommit: 63784729604aaf526de21f6c6b62813882af930a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/17/2020
ms.locfileid: "79438860"
---
# <a name="subsystem"></a>/SUBSYSTEM

Указывает среду выполнения, которая необходима для исполняемого образа.

```
/SUBSYSTEM:{BOOT_APPLICATION|CONSOLE|EFI_APPLICATION|
        EFI_BOOT_SERVICE_DRIVER|EFI_ROM|EFI_RUNTIME_DRIVER|
        NATIVE|POSIX|WINDOWS|WINDOWSCE}[,major[.minor]]
```

## <a name="remarks"></a>Remarks

Этот параметр изменяет образ, чтобы указать, какую подсистему необходимо использовать операционной системе для исполнения.

Можно указать одну из следующих подсистем:

**BOOT_APPLICATION**<br/>
Приложение, которое выполняется в среде загрузки Windows. Дополнительные сведения о приложениях загрузки см. [в разделе о поставщике данных BCD WMI](/previous-versions/windows/desktop/bcd/about-bcd).

**КОНСОЛ**<br/>
Приложения символьного режима Windows. Операционная система предоставляет консоль для консольных приложений.

**EFI_APPLICATION**<br/>
**EFI_BOOT_SERVICE_DRIVER**<br/>
**EFI_ROM**<br/>
**EFI_RUNTIME_DRIVER**<br/>
Образ EFI

Параметры подсистемы EFI описывают исполняемые образы, которые выполняются в среде Extensible Firmware Interface. Она обычно предоставляются с оборудованием и запускается перед загрузкой операционной системы. Основные различия между типами образов EFI — это область памяти, в которую загружается образ, и действие, выполняемое при возвращении вызова к образу. Образ EFI_APPLICATION выгружается, когда управление возвращается. EFI_BOOT_SERVICE_DRIVER и EFI_RUNTIME_DRIVER выгружаются, только если управление возвращается с кодом ошибки. Образ EFI_ROM выполняется из ПЗУ. Дополнительные сведения см. в спецификациях на унифицированном веб-сайте [форума EFI](https://www.uefi.org/) .

**СОБСТВЕННОЙ**<br/>
Код, который выполняется без среды подсистемы — например, работающие в режиме ядра драйверы устройств и собственные системные процессы. Этот параметр обычно зарезервирован для компонентов системы Windows.

**POSIX**<br/>
Приложение, выполняемое в подсистеме POSIX в Windows.

**WINDOWS**<br/>
Приложение, выполняемое в графической среде Windows. Сюда входят классические приложения и приложения универсальная платформа Windows (UWP).

**WINDOWSCE**<br/>
Подсистема WINDOWSCE указывает, что приложение предназначено для устройств с ядром Windows CE. Доступные версии ядра: PocketPC, Windows Mobile, Windows Phone 7, Windows CE V1.0-6.0R3 и Windows Embedded Compact 7.

Необязательные значения `major` и `minor` указывают минимальную требуемую версию заданной подсистемы:

- Целая часть номера версии (находится слева от десятичного разделителя) представлена `major`.

- Дробная часть номера версии (находится справа от десятичного разделителя) представлена `minor`.

- Значения `major` и `minor` должно иметь значение от 0 до 65 535.

Выбор подсистемы влияет на начальный адрес программы по умолчанию. Дополнительные сведения см. в разделе [/Entry (символ точки входа)](entry-entry-point-symbol.md), параметр компоновщика/ENTRY:*Function* .

Дополнительные сведения, включая минимальное значение и значения по умолчанию для основного и дополнительного номеров версий каждой подсистемы, см. в разделе параметр компоновщика [/SUBSYSTEM](subsystem-specify-subsystem.md) .

## <a name="see-also"></a>См. также раздел

[Параметры EDITBIN](editbin-options.md)
