---
title: Практическое руководство. Компиляция кода MFC и ATL с помощью - clr
ms.custom: get-started-article
ms.date: 11/04/2016
helpviewer_keywords:
- MFC [C++], interoperability
- ATL [C++], interoperability
- mixed assemblies [C++], MFC code
- mixed assemblies [C++], ATL code
- /clr compiler option [C++], compiling ATL and MFC code
- interoperability [C++], /clr compiler option
- regular MFC DLLs [C++], /clr compiler option
- interop [C++], /clr compiler option
- extension DLLs [C++], /clr compiler option
ms.assetid: 12464bec-33a4-482c-880a-c078de7f6ea5
ms.openlocfilehash: 9a24e82787eb0fce8ff668843e73de9f2d05e1ad
ms.sourcegitcommit: 0ab61bc3d2b6cfbd52a16c6ab2b97a8ea1864f12
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "62379055"
---
# <a name="how-to-compile-mfc-and-atl-code-by-using-clr"></a>Практическое руководство. Скомпилируйте MFC и ATL кода с помощью/CLR

В этом разделе описывается компиляция программ MFC и ATL целевых среда CLR.

### <a name="to-compile-an-mfc-executable-or-regular-mfc-dll-by-using-clr"></a>Для компиляции исполняемого файла или обычной MFC библиотеке DLL MFC с помощью/CLR

1. Щелкните правой кнопкой мыши проект в **обозревателе решений** и нажмите кнопку **свойства**.

1. В **свойства проекта** диалоговом окне разверните узел, рядом с полем **свойства конфигурации** и выберите **Общие**. В области справа в разделе **проекта по умолчанию**, задайте **поддержки Common Language Runtime** для **поддержка среды CLR (/ clr)**.

   Убедитесь, что в этой же области **использование MFC** присваивается **использовать MFC в общей DLL**.

1. В разделе **свойства конфигурации**, разверните узел, рядом с полем **C/C++** и выберите **Общие**. Убедитесь, что **формат отладочной информации** присваивается **/ZI базы данных программы** (не **/ZI**).

1. Выберите **создание кода** узла. Задайте **включить минимальное перестроение** для **нет (/ Gm-)**. Также задайте **основные проверки времени выполнения** для **по умолчанию**.

1. В разделе **свойства конфигурации**выберите **C/C++** и затем **создание кода**. Убедитесь, что **библиотеки времени выполнения** может принимать значение **Многопоточная DLL с возможностью отладки (/ MDd)** или **Многопоточная DLL (/ MD)**.

1. В файле Stdafx.h добавьте следующую строку.

    ```
    #using <System.Windows.Forms.dll>
    ```

### <a name="to-compile-an-mfc-extension-dll-by-using-clr"></a>Для компиляции библиотеки DLL расширения MFC с помощью/CLR

1. Следуйте указаниям в разделе «Компиляция MFC исполняемого файла или обычной библиотеке DLL MFC с помощью/CLR».

1. В разделе **свойства конфигурации**, разверните узел, рядом с полем **C/C++** и выберите **предкомпилированные заголовки**. Задайте **создать/использовать предварительно скомпилированный заголовочный файл** для **не использовать предварительно скомпилированные заголовки**.

   Кроме того в **обозревателе решений**, щелкните правой кнопкой мыши файл Stdafx.cpp и нажмите кнопку **свойства**. В разделе **свойства конфигурации**, разверните узел, рядом с полем **C/C++** и выберите **Общие**. Задайте **компилировать с поддержкой среда CLR** для **Поддержка общеязыковой среды выполнения нет**.

1. Для файла, содержащего DllMain и все, что она вызывает, в **обозревателе решений**, щелкните правой кнопкой мыши файл и нажмите кнопку **свойства**. В разделе **свойства конфигурации**, разверните узел, рядом с полем **C/C++** и выберите **Общие**. В области справа в разделе **проекта по умолчанию**, задайте **компилировать с поддержкой среда CLR** для **Поддержка общеязыковой среды выполнения нет**.

### <a name="to-compile-an-atl-executable-by-using-clr"></a>Для компиляции исполняемого файла ATL с помощью/CLR

1. В **обозревателе решений**, щелкните правой кнопкой мыши проект и нажмите кнопку **свойства**.

1. В **свойства проекта** диалоговом окне разверните узел, рядом с полем **свойства конфигурации** и выберите **Общие**. В области справа в разделе **проекта по умолчанию**, задайте **поддержки Common Language Runtime** для **поддержка среды CLR (/ clr)**.

1. В разделе **свойства конфигурации**, разверните узел, рядом с полем **C/C++** и выберите **Общие**. Убедитесь, что **формат отладочной информации** присваивается **/ZI базы данных программы** (не **/ZI**).

1. Выберите **создание кода** узла. Задайте **включить минимальное перестроение** для **нет (/ Gm-)**. Также задайте **основные проверки времени выполнения** для **по умолчанию**.

1. В разделе **свойства конфигурации**выберите **C/C++** и затем **создание кода**. Убедитесь, что **библиотеки времени выполнения** может принимать значение **Многопоточная DLL с возможностью отладки (/ MDd)** или **Многопоточная DLL (/ MD)**.

1. Для каждого MIDL-файла (C-файлы), щелкните правой кнопкой мыши файл в **обозревателе решений** и нажмите кнопку **свойства**. В разделе **свойства конфигурации**, разверните узел, рядом с полем **C/C++** и выберите **Общие**. Задайте **компилировать с поддержкой среда CLR** для **Поддержка общеязыковой среды выполнения нет**.

### <a name="to-compile-an-atl-dll-by-using-clr"></a>Для компиляции библиотеки DLL ATL с помощью/CLR

1. Выполните действия, описанные в разделе «для компиляции исполняемого файла ATL с помощью/CLR».

1. В разделе **свойства конфигурации**, разверните узел, рядом с полем **C/C++** и выберите **предкомпилированные заголовки**. Задайте **создать/использовать предварительно скомпилированный заголовочный файл** для **не использовать предварительно скомпилированные заголовки**.

   Кроме того в **обозревателе решений**, щелкните правой кнопкой мыши файл Stdafx.cpp и нажмите кнопку **свойства**. В разделе **свойства конфигурации**, разверните узел, рядом с полем **C/C++** и выберите **Общие**. Задайте **компилировать с поддержкой среда CLR** для **Поддержка общеязыковой среды выполнения нет**.

1. Для файла, содержащего DllMain и все, что она вызывает, в **обозревателе решений**, щелкните правой кнопкой мыши файл и нажмите кнопку **свойства**. В разделе **свойства конфигурации**, разверните узел, рядом с полем **C/C++** и выберите **Общие**. В области справа в разделе **проекта по умолчанию**, задайте **компилировать с поддержкой среда CLR** для **Поддержка общеязыковой среды выполнения нет**.

## <a name="see-also"></a>См. также

[Смешанные (собственные и управляемые) сборки](../dotnet/mixed-native-and-managed-assemblies.md)
