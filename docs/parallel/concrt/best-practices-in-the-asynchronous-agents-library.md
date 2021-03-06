---
title: Рекомендации по работе с библиотекой асинхронных агентов
ms.date: 11/04/2016
helpviewer_keywords:
- best practices, Asynchronous Agents Library
- Asynchronous Agents Library, best practices
- Asynchronous Agents Library, practices to avoid
- practices to avoid, Asynchronous Agents Library
ms.assetid: 85f52354-41eb-4b0d-98c5-f7344ee8a8cf
ms.openlocfilehash: 99780de11d85831a6901f370d2491f15ef88c0b1
ms.sourcegitcommit: 1f009ab0f2cc4a177f2d1353d5a38f164612bdb1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/27/2020
ms.locfileid: "87231745"
---
# <a name="best-practices-in-the-asynchronous-agents-library"></a>Рекомендации по работе с библиотекой асинхронных агентов

В этом документе описывается, как эффективно использовать библиотеку асинхронных агентов. Библиотека агентов поддерживает модель программирования на основе субъектов и передачу сообщений внутри процесса для недетализированного потока данных и задач конвейеризации.

Дополнительные сведения о библиотеке агентов см. в разделе [Библиотека асинхронных агентов](../../parallel/concrt/asynchronous-agents-library.md).

## <a name="sections"></a><a name="top"></a>Священ

Этот документ содержит следующие разделы.

- [Изоляция состояния с помощью агентов](#isolation)

- [Использование механизма регулирования для ограничения количества сообщений в конвейере данных](#throttling)

- [Не выполнять детализированную работу в конвейере данных](#fine-grained)

- [Не передавайте большие полезные данные сообщений по значению](#large-payloads)

- [Использование shared_ptr в сети данных, если владение не определено](#ownership)

## <a name="use-agents-to-isolate-state"></a><a name="isolation"></a>Изоляция состояния с помощью агентов

Библиотека агентов предоставляет альтернативы общему состоянию, позволяя подключать изолированные компоненты через асинхронный механизм передачи сообщений. Асинхронные агенты наиболее эффективны, если они изолируют свое внутреннее состояние от других компонентов. При изоляции состояния несколько компонентов обычно не работают с общими данными. Изоляция состояния может позволить приложению масштабироваться, так как оно уменьшает состязание за общую память. Изоляция состояния также сокращает вероятность взаимоблокировок и состояний гонки, так как компоненты не должны синхронизировать доступ к общим данным.

Обычно состояние изолируется в агенте путем хранения элементов данных в **`private`** разделах или **`protected`** класса агента и с помощью буферов сообщений для передачи изменений состояния. В следующем примере показан `basic_agent` класс, производный от [Concurrency:: Agent](../../parallel/concrt/reference/agent-class.md). `basic_agent`Класс использует два буфера сообщений для взаимодействия с внешними компонентами. Один буфер сообщений содержит входящие сообщения; в другом буфере сообщений содержатся исходящие сообщения.

[!code-cpp[concrt-simple-agent#1](../../parallel/concrt/codesnippet/cpp/best-practices-in-the-asynchronous-agents-library_1.cpp)]

Полные примеры определения и использования агентов см. в разделе [Пошаговое руководство. Создание приложения на основе агента](../../parallel/concrt/walkthrough-creating-an-agent-based-application.md) и [Пошаговое руководство. Создание агента потоков](../../parallel/concrt/walkthrough-creating-a-dataflow-agent.md)данных.

[[Top](#top)]

## <a name="use-a-throttling-mechanism-to-limit-the-number-of-messages-in-a-data-pipeline"></a><a name="throttling"></a>Использование механизма регулирования для ограничения количества сообщений в конвейере данных

Многие типы буферов сообщений, например [Concurrency:: unbounded_buffer](reference/unbounded-buffer-class.md), могут содержать неограниченное количество сообщений. Когда производитель сообщений отправляет сообщения в конвейер данных быстрее, чем потребитель может обработать эти сообщения, приложение может ввести нехватка памяти или состояние нехватки памяти. Для ограничения количества сообщений, одновременно активных в конвейере данных, можно использовать механизм регулирования, например семафор.

В следующем примере показано использование семафора для ограничения количества сообщений в конвейере данных. Конвейер данных использует функцию [Concurrency:: wait](reference/concurrency-namespace-functions.md#wait) для имитации операции, которая занимает не менее 100 миллисекунд. Поскольку отправитель создает сообщения быстрее, чем потребитель может обработать эти сообщения, в этом примере определяется класс, позволяющий `semaphore` приложению ограничить количество активных сообщений.

[!code-cpp[concrt-message-throttling#1](../../parallel/concrt/codesnippet/cpp/best-practices-in-the-asynchronous-agents-library_2.cpp)]

`semaphore`Объект ограничивает обработку конвейера не более чем двумя сообщениями одновременно.

В этом примере производитель отправляет относительно недостаточное число сообщений потребителю. Поэтому в этом примере не демонстрируется потенциально нехватка памяти или нехватка памяти. Однако этот механизм полезен, если конвейер данных содержит относительно большое количество сообщений.

Дополнительные сведения о создании класса семафора, используемого в этом примере, см. в разделе [как использовать класс контекста для реализации параллельного семафора](../../parallel/concrt/how-to-use-the-context-class-to-implement-a-cooperative-semaphore.md).

[[Top](#top)]

## <a name="do-not-perform-fine-grained-work-in-a-data-pipeline"></a><a name="fine-grained"></a>Не выполнять детализированную работу в конвейере данных

Библиотека агентов наиболее полезна, если работа, выполняемая конвейером данных, достаточно детализирована. Например, один компонент приложения может считывать данные из файла или сетевого подключения и периодически передавать эти данные в другой компонент. Протокол, используемый библиотекой агентов для распространения сообщений, приводит к тому, что механизм передачи сообщений будет иметь больше издержек, чем конструкции параллельных задач, предоставляемые [библиотекой параллельных шаблонов](../../parallel/concrt/parallel-patterns-library-ppl.md) (PPL). Поэтому убедитесь, что работа, выполняемая конвейером данных, достаточно длинна, чтобы отменять эти затраты.

Хотя конвейер данных наиболее эффективен, когда его задачи детализированы, каждый этап конвейера данных может использовать конструкции PPL, такие как группы задач и параллельные алгоритмы, для выполнения более детализированной работы. Пример недетализированной сети данных, в которой используется детализированный параллелизм на каждом этапе обработки, см. в разделе [Пошаговое руководство. Создание сети обработки изображений](../../parallel/concrt/walkthrough-creating-an-image-processing-network.md).

[[Top](#top)]

## <a name="do-not-pass-large-message-payloads-by-value"></a><a name="large-payloads"></a>Не передавайте большие полезные данные сообщений по значению

В некоторых случаях среда выполнения создает копию каждого сообщения, которое передается из одного буфера сообщений в другой. Например, класс [Concurrency:: overwrite_buffer](../../parallel/concrt/reference/overwrite-buffer-class.md) предлагает копию каждого сообщения, которое оно получает для каждой цели. Среда выполнения также создает копию данных сообщения при использовании таких функций передачи сообщений, как [Concurrency:: send](reference/concurrency-namespace-functions.md#send) и [Concurrency:: Receive](reference/concurrency-namespace-functions.md#receive) для записи сообщений в буфер сообщений и чтения сообщений из него. Хотя этот механизм помогает устранить риск одновременной записи в общие данные, это может привести к ухудшению производительности памяти, когда полезная нагрузка сообщения относительно велика.

Можно использовать указатели или ссылки для повышения производительности памяти при передаче сообщений с большими объемами полезных данных. В следующем примере сравниваются передача больших сообщений по значению для передачи указателей на один и тот же тип сообщений. В примере определяются два типа агентов `producer` и `consumer` , которые работают с `message_data` объектами. В примере сравнивается время, требуемое производителю для отправки нескольких `message_data` объектов потребителю на время, необходимое агенту-производителю для отправки нескольких указателей на `message_data` объекты потребителю.

[!code-cpp[concrt-message-payloads#1](../../parallel/concrt/codesnippet/cpp/best-practices-in-the-asynchronous-agents-library_3.cpp)]

В этом примере выводится следующий пример выходных данных:

```Output
Using message_data...
took 437ms.
Using message_data*...
took 47ms.
```

Версия, использующая указатели, работает лучше, так как устраняет необходимость создания полной копии всех `message_data` объектов, переданных от производителя к потребителю, в среде выполнения.

[[Top](#top)]

## <a name="use-shared_ptr-in-a-data-network-when-ownership-is-undefined"></a><a name="ownership"></a>Использование shared_ptr в сети данных, если владение не определено

При отправке сообщений по указателю через конвейер или сеть, передающие сообщения, обычно выделяется память для каждого сообщения в начале сети и освобождается память в конце сети. Несмотря на то, что этот механизм часто работает хорошо, бывают случаи, в которых это сложно или невозможно использовать. Например, рассмотрим случай, в котором сеть данных содержит несколько конечных узлов. В этом случае нет четкого расположения для освобождения памяти для сообщений.

Чтобы решить эту проблему, можно использовать механизм, например [std:: shared_ptr](../../standard-library/shared-ptr-class.md), который обеспечивает принадлежность указателя нескольким компонентам. При удалении последнего `shared_ptr` объекта, владеющего ресурсом, также освобождается ресурс.

В следующем примере показано, как использовать `shared_ptr` для совместного использования значений указателей между несколькими буферами сообщений. В примере объект [Concurrency:: overwrite_buffer](../../parallel/concrt/reference/overwrite-buffer-class.md) соединяется с тремя объектами [Concurrency:: Call](../../parallel/concrt/reference/call-class.md) . `overwrite_buffer`Класс предлагает сообщения для каждой цели. Так как в конце сети данных имеется несколько владельцев данных, в этом примере используется, `shared_ptr` чтобы разрешить каждому `call` объекту совместное использование владельцев сообщений.

[!code-cpp[concrt-message-sharing#1](../../parallel/concrt/codesnippet/cpp/best-practices-in-the-asynchronous-agents-library_4.cpp)]

В этом примере выводится следующий пример выходных данных:

```Output
Creating resource 42...
receiver1: received resource 42
Creating resource 64...
receiver2: received resource 42
receiver1: received resource 64
Destroying resource 42...
receiver2: received resource 64
Destroying resource 64...
```

## <a name="see-also"></a>См. также раздел

[Рекомендации по среда выполнения с параллелизмом](../../parallel/concrt/concurrency-runtime-best-practices.md)<br/>
[библиотеку асинхронных агентов](../../parallel/concrt/asynchronous-agents-library.md)<br/>
[Пошаговое руководство. Создание приложения на основе агента](../../parallel/concrt/walkthrough-creating-an-agent-based-application.md)<br/>
[Пошаговое руководство. Создание агента потока данных](../../parallel/concrt/walkthrough-creating-a-dataflow-agent.md)<br/>
[Пошаговое руководство. Создание сети обработки изображений](../../parallel/concrt/walkthrough-creating-an-image-processing-network.md)<br/>
[Рекомендации в библиотеке параллельных шаблонов](../../parallel/concrt/best-practices-in-the-parallel-patterns-library.md)<br/>
[Общие рекомендации в среда выполнения с параллелизмом](../../parallel/concrt/general-best-practices-in-the-concurrency-runtime.md)
