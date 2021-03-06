---
title: Обновление проектов C++ с более ранних версий Visual Studio
description: Обновление проектов Microsoft C++ с предыдущих версий Visual Studio.
ms.date: 01/21/2020
helpviewer_keywords:
- 32-bit code porting
- upgrading Visual C++ applications, 32-bit code
ms.assetid: 18cdacaa-4742-43db-9e4c-2d9e73d8cc84
ms.openlocfilehash: 89c1df88aa8e533dd7d6e5312b1150468c905c18
ms.sourcegitcommit: 12eb6a824dd7187a065d44fceca4c410f58e121e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2020
ms.locfileid: "94334238"
---
# <a name="upgrade-c-projects-from-earlier-versions-of-visual-studio"></a>Обновление проектов C++ с более ранних версий Visual Studio

Чтобы обновить проект, созданный в более ранней версии Visual Studio, просто откройте проект в последней версии Visual Studio. Visual Studio предлагает обновить проект до текущей схемы.

Если вы выберете " **нет** ", проект не будет обновлен. Для проектов, созданных в Visual Studio 2010 и более поздних версиях, можно по-прежнему использовать проект в более новой версии Visual Studio. Просто задайте свойства проекта, чтобы они продолжали работать с более старым набором инструментов. Если вы оставляете старую версию Visual Studio на компьютере, его набор инструментов доступен в более поздних версиях. Например, если проект должен продолжать работать в Windows XP, можно выполнить обновление до Visual Studio 2019. Затем вы указываете набор инструментов в качестве v141_xp или более ранней версии в свойствах проекта. Дополнительные сведения см. в разделе [Использование собственного многоплатформенного нацеливания в Visual Studio для сборки старых проектов](use-native-multi-targeting.md).

Если выбрать **Да** , проект будет обновлен на месте. Его нельзя преобразовать обратно в предыдущую версию. В сценариях обновления рекомендуется создать резервную копию существующих файлов проекта и решения.

## <a name="upgrade-reports"></a>Обновление отчетов

При обновлении проекта появляется отчет об обновлении. Отчет также сохраняется в папке проекта как UpgradeLog.htm. В отчете об обновлении отображается сводка проблем, обнаруженных во время преобразования. В нем перечислены некоторые сведения о внесенных изменениях, включая:

- Свойства проекта.

- Включаемые файлы.

- Код, который больше не компилируется четко из-за улучшения соответствия компилятора или изменений в стандарте.

- Код, основанный на функциях Visual Studio или Windows, которые больше не доступны. Или файлы заголовков, которые не включаются в установку Visual Studio по умолчанию или были удалены из продукта.

- Код, который больше не компилируется из-за изменений в API, таких как переименованные API, измененные сигнатуры функций или устаревшие функции.

- Код, который больше не компилируется из-за изменений в диагностике, например предупреждения об ошибке

- Ошибки компоновщика из-за измененных библиотек, особенно при использовании параметра/NODEFAULTLIB.

- Ошибки времени выполнения или непредвиденные результаты из-за изменений в поведении.

- Ошибки, появившиеся в средствах. Если вы нашли вопрос, сообщите о ней группе Visual C++ по стандартным каналам поддержки или с помощью страницы [сообщества разработчиков C++ для Visual Studio](https://aka.ms/feedback/report?space=62) .

Некоторые обновленные проекты и решения могут быть успешно собраны без изменений. Однако большинству проектов, скорее всего, потребуется внести изменения в параметры проекта и исходный код. Не существует единого правильного способа устранения этих проблем, но мы рекомендуем использовать поэтапный подход. Прежде чем начать, ознакомьтесь с [обзором возможных проблем с обновлением](../porting/overview-of-potential-upgrade-issues-visual-cpp.md) , чтобы получить дополнительные сведения о многих типичных ошибках.

1. Задайте предпочтительные версии набора инструментов платформы, стандарта языка C++ и Windows SDK версии (если применимо). ( **Проект**  >  **Свойства**  >  **Свойства конфигурации**  >  **Общие** )

1. При наличии большого количества ошибок можно временно отключить некоторые параметры, пока вы их исправите. Чтобы отключить [`/permissive-`](../build/reference/permissive-standards-conformance.md) параметр, используйте свойства **проекта** свойства  >  **Properties**  >  **конфигурации**  >  **C/C++**  >  **Language**. Чтобы отключить параметр [анализа кода](../code-quality/code-analysis-for-c-cpp-overview.md) , используйте свойства **проекта** свойства  >  **Properties**  >  **конфигурации**  >  **анализ кода**.

1. Убедитесь, что все зависимости существуют и что пути к включаемым файлам или папкам библиотеки указаны правильно. ( **Проект**  >  **Свойства**  >  **Свойства конфигурации**  >  **Каталоги VC + +** )

1. Выявление и устранение ошибок, вызванных ссылками на интерфейсы API, которые больше не существуют.

1. Устраните все оставшиеся ошибки, препятствующие компиляции. Сведения об устранении распространенных ошибок см. в статье [Обзор возможных проблем с обновлением](../porting/overview-of-potential-upgrade-issues-visual-cpp.md) .

1. Снова включите **`/permissive-`** и исправьте все новые ошибки, вызванные несоответствием коду, который ранее был скомпилирован в компилятором MSVC.

1. Включите анализ кода, чтобы выявление потенциальных проблем или устаревших шаблонов кода, которые больше не считаются приемлемыми. Если анализ кода помечает много ошибок, можно отключить некоторые предупреждения, чтобы сосредоточиться на наиболее важных. Интегрированная среда разработки может помочь быстро устранить проблемы некоторых типов.

1. Рассмотрим другие возможности для модернизации кода. Например, замените пользовательские структуры данных и алгоритмы на шаблоны из стандартной библиотеки C++ или в библиотеку с открытым исходным кодом. Используя стандартные функции, можно упростить поддержку кода другими пользователями. Можно быть уверенным, что этот код тщательно проверен и проверен многими экспертами по стандартам и широкому сообществу C++.

Для исправления ошибок можно выполнить поиск решений или отправить вопрос по [документация Майкрософт Q&a](/answers/topics/c%2B%2B.html). Для проблем компилятора и средств C++ посетите веб-сайт [сообщества разработчиков c++](https://aka.ms/vsfeedback/browsecpp) .

## <a name="in-this-section"></a>В этом разделе

[Обзор возможных проблем с обновлением](overview-of-potential-upgrade-issues-visual-cpp.md)\
[Обновление кода до универсальной CRT](upgrade-your-code-to-the-universal-crt.md)\
[Обновление WINVER и _WIN32_WINNT](modifying-winver-and-win32-winnt.md)\
[Исправление зависимостей от внутренних компонентов библиотеки](fix-your-dependencies-on-library-internals.md)\
[Проблемы с миграцией с плавающей точкой](floating-point-migration-issues.md)\
[Функции C++, устаревшие в Visual Studio 2019](features-deprecated-in-visual-studio.md)\
[VCBuild и MSBuild](build-system-changes.md)\
[Перенос сторонних библиотек](porting-third-party-libraries.md)

## <a name="see-also"></a>См. также

[Новые возможности C++ в Visual Studio](../overview/what-s-new-for-visual-cpp-in-visual-studio.md)\
[Журнал изменений Visual C++ 2003-2015](../porting/visual-cpp-change-history-2003-2015.md)\
[Нестандартное поведение](../cpp/nonstandard-behavior.md)\
[Перенос приложений для обработки данных](../data/data-access-programming-mfc-atl.md)
