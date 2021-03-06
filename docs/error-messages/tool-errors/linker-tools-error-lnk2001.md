---
title: Ошибка средств компоновщика LNK2001
ms.date: 12/19/2019
f1_keywords:
- LNK2001
helpviewer_keywords:
- LNK2001
ms.assetid: dc1cf267-c984-486c-abd2-fd07c799f7ef
ms.openlocfilehash: 59915b3aa0ad25b5638a43a6d09dccc2b42825ab
ms.sourcegitcommit: 1f009ab0f2cc4a177f2d1353d5a38f164612bdb1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/27/2020
ms.locfileid: "87230575"
---
# <a name="linker-tools-error-lnk2001"></a>Ошибка средств компоновщика LNK2001

> неразрешенный внешний символ "*символ*"

Скомпилированный код создает ссылку или вызов *символа*. Символ не определен ни в одной из библиотек или объектных файлов, поиск которого осуществляется компоновщиком.

Это сообщение об ошибке после неустранимой ошибки [LNK1120](../../error-messages/tool-errors/linker-tools-error-lnk1120.md). Чтобы устранить ошибку LNK1120, сначала исправьте все ошибки LNK2001 и LNK2019.

Существует множество способов получения ошибок LNK2001. Все они используют *ссылку* на функцию или переменную, которую компоновщик не может *Разрешить*, или найти определение для. Компилятор может определить, когда код не *объявляет* символ, но не в том случае, если он не *определен* . Это связано с тем, что определение может находиться в другом исходном файле или библиотеке. Если код ссылается на символ, но он никогда не определен, компоновщик создает ошибку.

## <a name="what-is-an-unresolved-external-symbol"></a>Что такое неразрешенный внешний символ?

*Символ* — это внутреннее имя функции или глобальной переменной. Это форма имени, используемая или определенная в скомпилированном объектном файле или библиотеке. Глобальная переменная определяется в объектном файле, где для него выделяется хранилище. Функция определена в объектном файле, где размещается скомпилированный код для тела функции. *Внешний символ* является ссылкой в одном файле объекта, но определен в другой библиотеке или объектном файле. *Экспортированный символ* — это открытый объект, который становится общедоступным для файлового файла или библиотеки, определяющей его.

Для создания приложения или библиотеки DLL в каждом используемом символе должно быть определено определение. Компоновщик должен *Разрешить*или найти определение сопоставления для каждого внешнего символа, на который ссылается каждый файл объекта. Компоновщик создает ошибку, если не удается разрешить внешний символ. Это означает, что компоновщику не удалось найти соответствующее определение экспортированного символа в любом из связанных файлов.

## <a name="compilation-and-link-issues"></a>Проблемы компиляции и компоновки

Эта ошибка может возникать:

- Если в проекте отсутствует ссылка на библиотеку (. LIB) или Object (. OBJ-файл). Чтобы устранить эту проблему, добавьте в проект ссылку на требуемую библиотеку или файл объекта. Дополнительные сведения см. [в разделе lib files as input компоновщика](../../build/reference/dot-lib-files-as-linker-input.md).

- Если проект содержит ссылку на библиотеку (. LIB) или Object (. OBJ), который, в свою очередь, требуются символы из другой библиотеки. Это может произойти даже в том случае, если не вызываются функции, вызывающие зависимость. Чтобы устранить эту проблему, добавьте в проект ссылку на другую библиотеку. Дополнительные сведения см. в разделе [понимание классической модели для связывания: использование символов в качестве пути](https://devblogs.microsoft.com/oldnewthing/20130108-00/?p=5623).

- При использовании параметров [/NODEFAULTLIB](../../build/reference/nodefaultlib-ignore-libraries.md) или [/Zl](../../build/reference/zl-omit-default-library-name.md) . При указании этих параметров библиотеки, содержащие требуемый код, не будут связаны с проектом, если они не включены явным образом. Чтобы устранить эту проблему, явно включите все библиотеки, используемые в командной строке компоновки. Если при использовании этих параметров отображается множество отсутствующих имен функций CRT или стандартной библиотеки, явно включите библиотеки CRT и библиотеки стандартных библиотек или файлы библиотеки в ссылку.

- При компиляции с параметром **/CLR** . Возможно, отсутствует ссылка на `.cctor` . Дополнительные сведения об устранении этой проблемы см. в разделе [Инициализация смешанных сборок](../../dotnet/initialization-of-mixed-assemblies.md).

- Если при построении отладочной версии приложения вы связываетесь с библиотеками в режиме выпуска. Аналогично, если вы используете параметры **/MTD** или **/MDD** или определяете, `_DEBUG` а затем связываетесь с библиотеками выпусков, то во многих других случаях следует рассчитывать на множество потенциальных неразрешенных внешних значений. Связывание сборки в режиме выпуска с отладочными библиотеками также вызывает аналогичные проблемы. Чтобы устранить эту проблему, убедитесь, что вы используете отладочные библиотеки в отладочных сборках и розничных библиотеках в ваших розничных сборках.

- Если код ссылается на символ из одной версии библиотеки, но вы связываете другую версию библиотеки. Как правило, нельзя смешивать объектные файлы или библиотеки, созданные для разных версий компилятора. Библиотеки, поставляемые в одной версии, могут содержать символы, которые не могут быть найдены в библиотеках, включенных в другие версии. Чтобы устранить эту проблему, создайте все объектные файлы и библиотеки с одной и той же версией компилятора, прежде чем связывать их друг с другом. Дополнительные сведения см. в разделе [C++ двоичная совместимость 2015-2019](../../porting/binary-compat-2015-2017.md).

- Если пути к библиотекам устарели. **Средства > параметры > проектов > диалоговом окне Каталоги VC + +** в разделе **файлы библиотеки** позволяют изменить порядок поиска библиотеки. Папка Компоновщик в диалоговом окне страницы свойств проекта может также содержать неактуальные пути.

- При установке нового Windows SDK (возможно, в другое расположение). Необходимо обновить порядок поиска библиотеки, чтобы он указывал на новое расположение. Как правило, путь следует поместить в новый каталог include и lib для пакета SDK перед расположением Visual C++ по умолчанию. Кроме того, проект, содержащий внедренные пути, может по-прежнему указывать на старые пути, которые являются допустимыми, но устарели. Обновите пути для новых функций, добавленных новой версией, которая установлена в другое расположение.

- При построении в командной строке и создании собственных переменных среды. Убедитесь, что пути к инструментам, библиотекам и файлам заголовков имеют одинаковую версию. Дополнительные сведения см. [в разделе Задание переменных пути и среды для сборок из командной строки](../../build/setting-the-path-and-environment-variables-for-command-line-builds.md) .

## <a name="coding-issues"></a>Проблемы кодирования

Эта ошибка может быть вызвана следующими причинами.

- Несовпадение регистра в исходном коде или файле определения модуля (DEF). Например, если вы назначите переменную `var1` в одном исходном файле C++ и попытаетесь получить к ней доступ `VAR1` , как в другой, возникает эта ошибка. Чтобы устранить эту проблему, используйте согласованное написание имен и имена регистров.

- Проект, использующий [встраивание функций](../../error-messages/tool-errors/function-inlining-problems.md). Это может произойти при определении функций **`inline`** в исходном файле, а не в файле заголовка. Встроенные функции не отображаются за пределами исходного файла, который их определяет. Чтобы устранить эту проблему, определите встроенные функции в заголовках, где они объявляются.

- Вызов функции C из программы на языке C++ без использования `extern "C"` объявления для функции C. Компилятор использует разные внутренние соглашения об именовании символов для кода C и C++. Внутреннее имя символа — это то, что ищет компоновщик при разрешении символов. Чтобы устранить эту проблему, используйте `extern "C"` обертку для всех объявлений функций C, используемых в коде C++, в результате чего компилятор должен использовать внутреннее соглашение об именовании языка c для этих символов. Параметры компилятора [/TP](../../build/reference/tc-tp-tc-tp-specify-source-file-type.md) и [/TC](../../build/reference/tc-tp-tc-tp-specify-source-file-type.md) заставляют компилятор компилировать файлы как C++ или C соответственно, независимо от расширения имени файла. Эти параметры могут привести к тому, что имена внутренних функций отличаются от предполагаемых.

- Попытка сослаться на функции или данные, у которых нет внешней компоновки. В C++ встроенные функции и **`const`** данные имеют внутреннюю компоновку, если явно не указано в качестве **`extern`** . Чтобы устранить эту проблему, используйте явные **`extern`** объявления для символов, которые ссылаются вне определяющего исходного файла.

- [Отсутствует тело функции или определение переменной](../../error-messages/tool-errors/missing-function-body-or-variable.md) . Эта ошибка часто возникает при объявлении, но не определении, переменных, функций или классов в коде. Компилятору требуется только прототип функции или **`extern`** объявление переменной, чтобы создать объектный файл без ошибок, но компоновщик не может разрешить вызов функции или ссылку на переменную, так как код функции или переменное пространство не зарезервированы. Чтобы устранить эту проблему, обязательно Определите каждую указанную функцию и переменную в исходном файле или библиотеке, на которую вы связываетесь.

- Вызов функции, который использует типы возвращаемых значений и параметров или соглашения о вызовах, которые не соответствуют объектам в определении функции. В объектных файлах C++ [декорирование имен](../../error-messages/tool-errors/name-decoration.md) кодирует соглашение о вызовах, область класса или пространства имен, а также типы возвращаемых данных и параметров функции. Закодированная строка становится частью окончательного декорированного имени функции. Это имя используется компоновщиком для разрешения или сопоставления вызовов функции из других объектных файлов. Чтобы устранить эту проблему, убедитесь, что в объявлении функции, определении и вызовах используются одни и те же области, типы и соглашения о вызовах.

- Код C++, который вызывается при включении прототипа функции в определение класса, но не [включает реализацию](../../error-messages/tool-errors/missing-function-body-or-variable.md) функции. Чтобы устранить эту проблему, обязательно предоставьте определение для всех членов класса, которые вы вызываете.

- Попытка вызвать чисто виртуальную функцию из абстрактного базового класса. Чистая виртуальная функция не имеет реализации базового класса. Чтобы устранить эту проблему, убедитесь, что все вызванные виртуальные функции реализованы.

- Попытка использовать переменную, объявленную в функции ([Локальная переменная](../../error-messages/tool-errors/automatic-function-scope-variables.md)), за пределами области этой функции. Чтобы устранить эту проблему, удалите ссылку на переменную, которая не находится в области действия, или переместите переменную в область более высокого уровня.

- При построении окончательной версии проекта ATL создается сообщение о том, что код запуска CRT является обязательным. Чтобы устранить эту проблему, выполните одно из следующих действий.

  - Удалите `_ATL_MIN_CRT` из списка определений препроцессора, чтобы разрешить включение кода запуска CRT. Дополнительные сведения см. в разделе [Страница свойств "Общие" (проект)](../../build/reference/general-property-page-project.md).

  - Если это возможно, удалите вызовы функций CRT, требующих код запуска CRT. Вместо этого используйте эквиваленты Win32. Например, используйте `lstrcmp` вместо `strcmp`. Известными функциями, требующими код запуска CRT, являются некоторые функции строк и вычислений с плавающей запятой.

## <a name="consistency-issues"></a>Проблемы согласованности

В настоящее время нет стандарта для [декорирования имен C++](../../error-messages/tool-errors/name-decoration.md) между поставщиками компиляторов или даже между разными версиями одного и того же компилятора. Объектные файлы, скомпилированные с разными компиляторами, могут не использовать одинаковую схему именования. Связывание их может вызвать ошибку LNK2001.

[Смешивание встроенных и невстроенных параметров компиляции](../../error-messages/tool-errors/function-inlining-problems.md) в разных модулях может вызвать ошибку LNK2001. Если библиотека C++ создается с включенной функцией встраивания функций (**/Ob1** или **/Ob2**), но соответствующий заголовочный файл, описывающий функции, отключен (без **`inline`** ключевого слова), возникает эта ошибка. Чтобы устранить эту проблему, определите функции **`inline`** в файле заголовка, который включается в другие исходные файлы.

Если вы используете `#pragma inline_depth` директиву компилятора, убедитесь, что задано [значение 2 или более](../../error-messages/tool-errors/function-inlining-problems.md), и убедитесь, что вы также используете параметр компилятора [/Ob1](../../build/reference/ob-inline-function-expansion.md) или [/Ob2](../../build/reference/ob-inline-function-expansion.md) .

Эта ошибка может возникать, если опустить параметр LINK/NOENTRY при создании библиотеки DLL только для ресурсов. Чтобы устранить эту проблему, добавьте параметр/NOENTRY в команду Link.

Эта ошибка может возникать, если в проекте используются неверные параметры/SUBSYSTEM или/ENTRY. Например, при написании консольного приложения и задании/SUBSYSTEM: WINDOWS создается неразрешенная внешняя ошибка для `WinMain` . Чтобы устранить эту проблему, убедитесь, что вы соответствуете параметрам типа проекта. Дополнительные сведения об этих параметрах и точках входа см. в разделе Параметры компоновщика [/SUBSYSTEM](../../build/reference/subsystem-specify-subsystem.md) и [/entry](../../build/reference/entry-entry-point-symbol.md) .

## <a name="exported-def-file-symbol-issues"></a>Ошибки экспортированного файла DEF

Эта ошибка возникает, когда экспорт, указанный в DEF-файле, не найден. Это может быть вызвано тем, что экспорт не существует, написан неправильно или использует декорированные имена C++. DEF-файл не имеет декорированных имен. Чтобы устранить эту проблему, удалите ненужные экспорты и используйте `extern "C"` объявления для экспортированных символов.

## <a name="use-the-decorated-name-to-find-the-error"></a>Использовать декорированное имя для поиска ошибки

Компилятор и компоновщик C++ используют [декорирование имен](../../error-messages/tool-errors/name-decoration.md), также называемое *искажением имени*. Декорирование имен кодирует дополнительные сведения о типе переменной в ее имени символа. Имя символа для функции кодирует свой возвращаемый тип, типы параметров, область и соглашение о вызовах. Это декорированное имя — это имя символа, которое компоновщик ищет для разрешения внешних символов.

Ошибка связи может возникнуть, если объявление функции или переменной не *полностью* соответствует определению функции или переменной. Это связано с тем, что все различия становятся частью имени символа для сопоставления. Ошибка может возникать даже в том случае, если один и тот же файл заголовка используется как в вызывающем, так и в коде, определяющем код. Это может произойти, если вы компилируете исходные файлы с помощью различных флагов компилятора. Например, если код компилируется для использования **`__vectorcall`** соглашения о вызовах, но вы связываетесь с библиотекой, которая ожидает, что клиенты будут вызывать его с помощью соглашения по умолчанию **`__cdecl`** или **`__fastcall`** вызова. В этом случае символы не совпадают, поскольку соглашения о вызовах различаются.

Чтобы помочь вам найти причину, в сообщении об ошибке отображаются две версии имени. В нем отображается «понятное имя», имя, используемое в исходном коде, и декорированное имя (в круглых скобках). Вам не нужно знать, как интерпретировать декорированное имя. Вы по-прежнему можете выполнять поиск и сравнивать его с другими декорированными именами. Программы командной строки могут помочь найти и сравнить ожидаемое имя символа и фактическое имя символа:

- Параметры [/EXPORTS](../../build/reference/dash-exports.md) и [/SYMBOLS](../../build/reference/symbols.md) программы командной строки DUMPBIN полезны здесь. Они могут помочь вам определить, какие символы определены в DLL-файле и файлах объектов или библиотек. Можно использовать список символов, чтобы убедиться, что экспортированные декорированные имена соответствуют декорированным именам, которые ищет компоновщик.

- В некоторых случаях Компоновщик может сообщать только о декорированном имени символа. Для получения недекорированной формы декорированного имени можно использовать программу командной строки UNDNAME.

## <a name="additional-resources"></a>Дополнительные ресурсы

Дополнительные сведения см. в Stack Overflow вопросе ["что такое неопределенная ссылка или ошибка неразрешенного внешнего символа и как ее исправить?"](https://stackoverflow.com/q/12573816/2002113).
