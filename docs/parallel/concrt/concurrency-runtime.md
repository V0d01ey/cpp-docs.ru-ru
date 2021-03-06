---
title: Среда выполнения с параллелизмом
ms.date: 07/20/2018
helpviewer_keywords:
- Concurrency Runtime, getting started
- ConcRT (see Concurrency Runtime)
- Concurrency Runtime
ms.assetid: 874bc58f-8dce-483e-a3a1-4dcc9e52ed2c
ms.openlocfilehash: ce75d7a78fec69922c08479f6c130c6b6ccec566
ms.sourcegitcommit: ec6dd97ef3d10b44e0fedaa8e53f41696f49ac7b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/25/2020
ms.locfileid: "88845514"
---
# <a name="concurrency-runtime"></a>Среда выполнения с параллелизмом

Исполняющая среда с параллелизмом для C++ помогает создавать надежные, масштабируемые, быстро реагирующие параллельные приложения. Она повышает уровень абстракции, чтобы пользователю не приходилось управлять подробностями инфраструктуры, связанными с параллелизмом. Ее также можно использовать для указания политик планирования, соответствующих требованиям качества обслуживания приложений. Эти ресурсы помогут вам начать работу с исполняющей средой с параллелизмом.

Справочную документацию см. в [справочнике](../../parallel/concrt/reference/reference-concurrency-runtime.md).

> [!TIP]
> Исполняющая среда с параллелизмом интенсивно использует возможности C++11 и соответствует более современному стилю программирования на C++. Дополнительные сведения см. в статье  [Добро пожаловать в C++](../../cpp/welcome-back-to-cpp-modern-cpp.md).

## <a name="choosing-concurrency-runtime-features"></a>Выбор возможности среды выполнения с параллелизмом

|Статья|Описание|
|-|-|
|[Обзор](../../parallel/concrt/overview-of-the-concurrency-runtime.md)|Объясняет, почему важна среда выполнения с параллелизмом и описывает ее основные компоненты.|
|[Сравнение с другими моделями параллелизма](../../parallel/concrt/comparing-the-concurrency-runtime-to-other-concurrency-models.md)|Показывает характеристики среды выполнения с параллелизмом в сравнении с другими моделями параллелизма, например пулом потоков Windows и OpenMP, чтобы использовать ту модель параллелизма, которая лучше всего соответствует требованиям приложения.|
|[Переход от OpenMP к среде выполнения с параллелизмом](../../parallel/concrt/migrating-from-openmp-to-the-concurrency-runtime.md)|Здесь OpenMP сравнивается с исполняющей средой с параллелизмом, и предоставляются примеры способов миграции существующего кода OpenMP для использования исполняющей среды с параллелизмом.|
|[Библиотека параллельных шаблонов](../../parallel/concrt/parallel-patterns-library-ppl.md)|Здесь представлены общие сведения о работе с PPL, предоставляющей параллельные циклы, задачи и контейнеры.|
|[библиотеку асинхронных агентов](../../parallel/concrt/asynchronous-agents-library.md)|Здесь приводятся способы использования асинхронных агентов и функций передачи сообщений, позволяющих встроить в приложения потоки данных и обеспечить конвейерную обработку задач.|
|[Планировщик заданий](../../parallel/concrt/task-scheduler-concurrency-runtime.md)|Содержит описание планировщика задач, который позволяет точно настроить производительность ваших приложений для настольных систем, использующих исполняющую среду с параллелизмом.|

## <a name="task-parallelism-in-the-ppl"></a>Параллелизм задач в PPL

|Статья|Описание|
|-|-|
|[Параллельное выполнение задач](../../parallel/concrt/task-parallelism-concurrency-runtime.md)<br /><br /> [Как использовать parallel_invoke для написания параллельной подпрограммы сортировки](../../parallel/concrt/how-to-use-parallel-invoke-to-write-a-parallel-sort-routine.md)<br /><br /> [Инструкции. Использование parallel_invoke для выполнения параллельных операций](../../parallel/concrt/how-to-use-parallel-invoke-to-execute-parallel-operations.md)<br /><br /> [Как создать задачу, которая завершается после задержки](../../parallel/concrt/how-to-create-a-task-that-completes-after-a-delay.md)|Содержит описание задач и групп задач, которые могут помочь в создании асинхронного кода и декомпозиции параллельной работы на меньшие части.|
|[Пошаговое руководство. Реализация фьючерсов](../../parallel/concrt/walkthrough-implementing-futures.md)|Здесь показано, как объединить возможности исполняющей среды с параллелизмом для выполнения более объемной задачи.|
|[Пошаговое руководство. Удаление работы из потока пользовательского интерфейса](../../parallel/concrt/walkthrough-removing-work-from-a-user-interface-thread.md)|Здесь показано, как переместить работу, выполняемую потоком ИП, в рабочий поток в приложении MFC.|
|[Рекомендации в библиотеке параллельных шаблонов](../../parallel/concrt/best-practices-in-the-parallel-patterns-library.md)<br /><br /> [Общие рекомендации в среда выполнения с параллелизмом](../../parallel/concrt/general-best-practices-in-the-concurrency-runtime.md)|Рекомендации по работе с PPL.|

## <a name="data-parallelism-in-the-ppl"></a>Параллелизм данных в PPL

|Статья|Описание|
|-|-|
|[Параллельные алгоритмы](../../parallel/concrt/parallel-algorithms.md)<br /><br /> [Руководство. Написание цикла parallel_for](../../parallel/concrt/how-to-write-a-parallel-for-loop.md)<br /><br /> [Руководство. Написание цикла parallel_for_each](../../parallel/concrt/how-to-write-a-parallel-for-each-loop.md)<br /><br /> [Как выполнять операции Map и reduce в параллельном режиме](../../parallel/concrt/how-to-perform-map-and-reduce-operations-in-parallel.md)|Здесь приводится описание `parallel_for`, `parallel_for_each`, `parallel_invoke`и других параллельных алгоритмов. Используйте параллельные алгоритмы для решения задач *параллельных данных* , включающих коллекции данных.|
|[Параллельные контейнеры и объекты](../../parallel/concrt/parallel-containers-and-objects.md)<br /><br /> [Как использовать параллельные контейнеры для повышения эффективности](../../parallel/concrt/how-to-use-parallel-containers-to-increase-efficiency.md)<br /><br /> [Как использовать комбинирование для повышения производительности](../../parallel/concrt/how-to-use-combinable-to-improve-performance.md)<br /><br /> [Как использовать комбинирование для объединения наборов](../../parallel/concrt/how-to-use-combinable-to-combine-sets.md)|Здесь приводится описание классов `combinable` , `concurrent_vector`, `concurrent_queue`, `concurrent_unordered_map`и других параллельных контейнеров. Используйте параллельные контейнеры и объекты, когда требуются контейнеры, предоставляющие потокобезопасный доступ к своим элементам.|
|[Рекомендации в библиотеке параллельных шаблонов](../../parallel/concrt/best-practices-in-the-parallel-patterns-library.md)<br /><br /> [Общие рекомендации в среда выполнения с параллелизмом](../../parallel/concrt/general-best-practices-in-the-concurrency-runtime.md)|Рекомендации по работе с PPL.|

## <a name="canceling-tasks-and-parallel-algorithms"></a>Отмена задач и параллельных алгоритмов

|Статья|Описание|
|-|-|
|[Отмена в библиотеке параллельных шаблонов](cancellation-in-the-ppl.md)|Здесь описывается роль отмены в PPL, включая способы создания запросов отмены и реагирования на них.|
|[Как прервать выполнение из параллельного цикла с помощью отмены](../../parallel/concrt/how-to-use-cancellation-to-break-from-a-parallel-loop.md)<br /><br /> [Как использовать обработку исключений для разрыва из параллельного цикла](../../parallel/concrt/how-to-use-exception-handling-to-break-from-a-parallel-loop.md)|Здесь демонстрируются два способа отмены работы с параллельными данными.|

## <a name="universal-windows-platform-apps"></a>Приложения универсальной платформы Windows

|Статья|Описание|
|-|-|
|[Создание асинхронных операций в C++ для приложений UWP](../../parallel/concrt/creating-asynchronous-operations-in-cpp-for-windows-store-apps.md)|Описывает некоторые ключевые моменты, которые следует учитывать при использовании среда выполнения с параллелизмом для создания асинхронных операций в приложении UWP.|
|[Пошаговое руководство. подключение с помощью задач и HTTP-запросов XML](../../parallel/concrt/walkthrough-connecting-using-tasks-and-xml-http-requests.md)|Показывает, как объединять задачи PPL с `IXMLHTTPRequest2` `IXMLHTTPRequest2Callback` интерфейсами и для отправки запросов HTTP GET и POST к веб-службе в приложении UWP.|
|[Примеры приложений среда выполнения Windows](/samples/browse/?languages=cpp&expanded=windows&products=windows-uwp)|Содержит загружаемые примеры кода и демонстрационные приложения для среда выполнения Windows.|

## <a name="dataflow-programming-in-the-asynchronous-agents-library"></a>Программирование потоков данных в библиотеке асинхронных агентов

|Статья|Описание|
|-|-|
|[Асинхронные агенты](../../parallel/concrt/asynchronous-agents.md)<br /><br /> [Асинхронные блоки сообщений](../../parallel/concrt/asynchronous-message-blocks.md)<br /><br /> [Функции передачи сообщений](../../parallel/concrt/message-passing-functions.md)<br /><br /> [Как реализовать различные шаблоны "производитель-получатель"](../../parallel/concrt/how-to-implement-various-producer-consumer-patterns.md)<br /><br /> [Руководство. предоставление рабочих функций классам Call и transformer](../../parallel/concrt/how-to-provide-work-functions-to-the-call-and-transformer-classes.md)<br /><br /> [Как использовать преобразователь в конвейере данных](../../parallel/concrt/how-to-use-transformer-in-a-data-pipeline.md)<br /><br /> [Практические руководства. Выбор между завершенными задачами](../../parallel/concrt/how-to-select-among-completed-tasks.md)<br /><br /> [Как отправить сообщение с постоянным интервалом](../../parallel/concrt/how-to-send-a-message-at-a-regular-interval.md)<br /><br /> [Как использовать фильтр блоков сообщений](../../parallel/concrt/how-to-use-a-message-block-filter.md)|Здесь описываются асинхронные агенты, блоки сообщений и функции передачи сообщений, которые являются стандартными сборочными блоками для выполнения операций с потоками данных в исполняющей среде с параллелизмом.|
|[Пошаговое руководство. Создание приложения на основе агента](../../parallel/concrt/walkthrough-creating-an-agent-based-application.md)<br /><br /> [Пошаговое руководство. Создание агента потока данных](../../parallel/concrt/walkthrough-creating-a-dataflow-agent.md)|Здесь демонстрируется создание простого приложения на основе агентов.|
|[Пошаговое руководство. Создание сети обработки изображений](../../parallel/concrt/walkthrough-creating-an-image-processing-network.md)|Здесь показано, как создавать сеть асинхронных блоков сообщений, выполняющих обработку изображений.|
|[Пошаговое руководство. использование Join для предотвращения взаимоблокировок](../../parallel/concrt/walkthrough-using-join-to-prevent-deadlock.md)|На примере проблемы обедающих философов показано использование исполняющей среды с параллелизмом для недопущения взаимоблокировок в приложении.|
|[Пошаговое руководство. Создание пользовательского блока сообщений](../../parallel/concrt/walkthrough-creating-a-custom-message-block.md)|Здесь описано, как создать пользовательский тип блока сообщений, сортирующий входящие сообщения по приоритету.|
|[Рекомендации в библиотеке асинхронных агентов](../../parallel/concrt/best-practices-in-the-asynchronous-agents-library.md)<br /><br /> [Общие рекомендации в среда выполнения с параллелизмом](../../parallel/concrt/general-best-practices-in-the-concurrency-runtime.md)|Рекомендации по работе с агентами.|

## <a name="exception-handling-and-debugging"></a>Обработка и отладка исключений

|Статья|Описание|
|-|-|
|[Обработка исключений](../../parallel/concrt/exception-handling-in-the-concurrency-runtime.md)|Здесь описана работа с исключениями в среде выполнения с параллелизмом.|
|[Средства диагностики параллельного выполнения](../../parallel/concrt/parallel-diagnostic-tools-concurrency-runtime.md)|Описывает способы оптимизации приложений и наиболее эффективного использования среды выполнения с параллелизмом.|

## <a name="tuning-performance"></a>Настройка производительности

|Статья|Описание|
|-|-|
|[Средства диагностики параллельного выполнения](../../parallel/concrt/parallel-diagnostic-tools-concurrency-runtime.md)|Описывает способы оптимизации приложений и наиболее эффективного использования среды выполнения с параллелизмом.|
|[Экземпляры планировщика](../../parallel/concrt/scheduler-instances.md)<br /><br /> [Руководство. Управление экземпляром планировщика](../../parallel/concrt/how-to-manage-a-scheduler-instance.md)<br /><br /> [Политики планировщика](../../parallel/concrt/scheduler-policies.md)<br /><br /> [Как указать конкретные политики планировщика](../../parallel/concrt/how-to-specify-specific-scheduler-policies.md)<br /><br /> [Как создавать агенты, использующие определенные политики планировщика](../../parallel/concrt/how-to-create-agents-that-use-specific-scheduler-policies.md)|Здесь показаны способы управления экземплярами планировщиков и политиками планировщика. В классических приложениях политики планировщика позволяют связывать определенные правила с определенными типами рабочих нагрузок. Например, можно создать один экземпляр планировщика для выполнения некоторых задач с повышенным приоритетом потока и использовать планировщик по умолчанию для выполнения задач с обычным приоритетом потока.|
|[Планирование групп](../../parallel/concrt/schedule-groups.md)<br /><br /> [Как использовать группы расписаний для влияния на порядок выполнения](../../parallel/concrt/how-to-use-schedule-groups-to-influence-order-of-execution.md)|Демонстрирует использование групп планирования для группировки связанных задач. Например, вам может потребоваться высокая локальность связанных задач, если эти задачи лучше выполнять в одном узле процессора.|
|[Упрощенные задачи](../../parallel/concrt/lightweight-tasks.md)|Здесь описывается полезность легковесных задач при создании работы, которая не требует распределения нагрузки или отмены, и как они также могут использоваться для адаптации существующего кода для использования с исполняющей средой с параллелизмом.|
|[Контексты](../../parallel/concrt/contexts.md)<br /><br /> [Как использовать класс контекста для реализации параллельного семафора](../../parallel/concrt/how-to-use-the-context-class-to-implement-a-cooperative-semaphore.md)<br /><br /> [Как использовать превышение лимита подписки для смещения задержки](../../parallel/concrt/how-to-use-oversubscription-to-offset-latency.md)|Здесь приводятся способы управления поведением потоков, управляемых исполняющей средой с параллелизмом.|
|[Функции управления памятью](../../parallel/concrt/memory-management-functions.md)<br /><br /> [Как использовать Alloc и Free для повышения производительности памяти](../../parallel/concrt/how-to-use-alloc-and-free-to-improve-memory-performance.md)|Здесь описываются функции управления памятью, предоставляемые исполняющей средой с параллелизмом для параллельного выделения и высвобождения памяти.|

## <a name="additional-resources"></a>Дополнительные ресурсы

|Статья|Описание|
|-|-|
|[Шаблоны асинхронного программирования и советы по Hilo (приложения Магазина Windows на C++ и XAML)](/previous-versions/windows/apps/jj160321(v=win.10))|Узнайте, как мы использовали среда выполнения с параллелизмом для реализации асинхронных операций в Hilo, среда выполнения Windows приложении с использованием C++ и XAML.|
|[Блог Parallel Programming in Native Code](/archive/blogs/nativeconcurrency)|Содержит дополнительные подробные статьи о параллельном программировании в среде выполнения с параллелизмом.|
|[Форум Parallel Computing in C++ and Native Code](https://go.microsoft.com/fwlink/p/?linkid=183874)|Позволяет участвовать в обсуждениях сообщества о среде выполнения с параллелизмом.|
|[Параллельное программирование](/dotnet/standard/parallel-programming/index)|Учебник по модели параллельного программирования, доступной в .NET Framework.|

## <a name="see-also"></a>См. также раздел

[Ссылки](../../parallel/concrt/reference/reference-concurrency-runtime.md)
