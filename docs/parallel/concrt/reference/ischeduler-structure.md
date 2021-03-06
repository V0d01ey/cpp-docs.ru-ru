---
title: Структура IScheduler
ms.date: 11/04/2016
f1_keywords:
- IScheduler
- CONCRTRM/concurrency::IScheduler
- CONCRTRM/concurrency::IScheduler::IScheduler::AddVirtualProcessors
- CONCRTRM/concurrency::IScheduler::IScheduler::GetId
- CONCRTRM/concurrency::IScheduler::IScheduler::GetPolicy
- CONCRTRM/concurrency::IScheduler::IScheduler::NotifyResourcesExternallyBusy
- CONCRTRM/concurrency::IScheduler::IScheduler::NotifyResourcesExternallyIdle
- CONCRTRM/concurrency::IScheduler::IScheduler::RemoveVirtualProcessors
- CONCRTRM/concurrency::IScheduler::IScheduler::Statistics
helpviewer_keywords:
- IScheduler structure
ms.assetid: 471de85a-2b1a-4b6d-ab81-2eff2737161e
ms.openlocfilehash: ccd82b5c5112bc322717f2b58d79d4c8f34f5bbd
ms.sourcegitcommit: c123cc76bb2b6c5cde6f4c425ece420ac733bf70
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2020
ms.locfileid: "81368176"
---
# <a name="ischeduler-structure"></a>Структура IScheduler

Интерфейс для абстракции планировщика работы. Диспетчер ресурсов среды выполнения с параллелизмом использует этот интерфейс для взаимодействия с планировщиками работы.

## <a name="syntax"></a>Синтаксис

```cpp
struct IScheduler;
```

## <a name="members"></a>Участники

### <a name="public-methods"></a>Открытые методы

|Имя|Описание|
|----------|-----------------|
|[I'Scheduler::AddVirtualProcessors](#addvirtualprocessors)|Предоставляет планировщик с набором виртуальных корней процессора для его использования. Каждый `IVirtualProcessorRoot` интерфейс представляет собой право на выполнение одного потока, который может выполнять работу от имени планировщика.|
|[IScheduler::GetId](#getid)|Возвращает уникальный идентификатор для планировщика.|
|[IScheduler::GetPolicy](#getpolicy)|Возвращает копию политики планировщика. Для получения дополнительной информации о политиках планировщика, [см.](schedulerpolicy-class.md)|
|[IScheduler::NotifyResourcesExternallyBusy](#notifyresourcesexternallybusy)|Уведомляет этот планировщик о том, что аппаратные потоки, `ppVirtualProcessorRoots` представленные набором виртуальных корней процессора в массиве, теперь используются другими планировщиками.|
|[IScheduler::NotifyResourcesExternallyIdle](#notifyresourcesexternallyidle)|Уведомляет этот планировщик о том, что аппаратные потоки, `ppVirtualProcessorRoots` представленные набором корней виртуального процессора в массиве, не используются другими планировщиками.|
|[I'Scheduler::RemoveVirtualProcessors](#removevirtualprocessors)|Инициирует удаление корней виртуальных процессоров, которые ранее были выделены этому планировщику.|
|[IScheduler::Статистика](#statistics)|Предоставляет информацию, связанную с показателями прибытия и завершения задач, а также изменение длины очереди для планировщика.|

## <a name="remarks"></a>Remarks

Если вы реализуете пользовательский планировщик, который общается с менеджером ресурсов, вы должны предоставить реализацию `IScheduler` интерфейса. Этот интерфейс является одним из конца двустороннего канала связи между планировщиком и менеджером ресурсов. Другой конец представлен интерфейсами `IResourceManager` `ISchedulerProxy` и интерфейсами, которые реализуются менеджером ресурсов.

## <a name="inheritance-hierarchy"></a>Иерархия наследования

`IScheduler`

## <a name="requirements"></a>Требования

**Заголовок:** concrtrm.h

**Название:** параллелизм

## <a name="ischeduleraddvirtualprocessors-method"></a><a name="addvirtualprocessors"></a>I'Scheduler::AddVirtualProcessors Метод

Предоставляет планировщик с набором виртуальных корней процессора для его использования. Каждый `IVirtualProcessorRoot` интерфейс представляет собой право на выполнение одного потока, который может выполнять работу от имени планировщика.

```cpp
virtual void AddVirtualProcessors(
    _In_reads_(count) IVirtualProcessorRoot** ppVirtualProcessorRoots,
    unsigned int count) = 0;
```

### <a name="parameters"></a>Параметры

*ppVirtualProcessorRoots*<br/>
Массив интерфейсов, представляющих `IVirtualProcessorRoot` корни виртуального процессора, добавляемых в планировщик.

*count*<br/>
Количество `IVirtualProcessorRoot` интерфейсов в массиве.

### <a name="remarks"></a>Remarks

Менеджер ресурсов ссылается `AddVirtualProcessor` на метод предоставления первоначального набора виртуальных корней процессора планировщику. Он также может ссылаться на метод добавления виртуальных корней процессора в планировщик, когда он перебалансирует ресурсы между планировщиками.

## <a name="ischedulergetid-method"></a><a name="getid"></a>IScheduler::GetId Метод

Возвращает уникальный идентификатор для планировщика.

```cpp
virtual unsigned int GetId() const = 0;
```

### <a name="return-value"></a>Возвращаемое значение

Уникальный идентификатор несбыванного.

### <a name="remarks"></a>Remarks

Функция [GetSchedulerId](concurrency-namespace-functions.md) должна использоваться для получения уникального идентификатора для объекта, реализующего `IScheduler` интерфейс, прежде чем использовать интерфейс в качестве параметра для методов, поставляемых диспетчером ресурсов. Ожидается, что при вызове `GetId` функции необходимо вернуть тот же идентификатор.

Идентификатор, полученный из другого источника, может привести к неопределенному поведению.

## <a name="ischedulergetpolicy-method"></a><a name="getpolicy"></a>IScheduler::Метод Гетполитики

Возвращает копию политики планировщика. Для получения дополнительной информации о политиках планировщика, [см.](schedulerpolicy-class.md)

```cpp
virtual SchedulerPolicy GetPolicy() const = 0;
```

### <a name="return-value"></a>Возвращаемое значение

Копия политики планировщика.

## <a name="ischedulernotifyresourcesexternallybusy-method"></a><a name="notifyresourcesexternallybusy"></a>IScheduler::NotifyResourcesExternallyBusy Метод

Уведомляет этот планировщик о том, что аппаратные потоки, `ppVirtualProcessorRoots` представленные набором виртуальных корней процессора в массиве, теперь используются другими планировщиками.

```cpp
virtual void NotifyResourcesExternallyBusy(
    _In_reads_(count) IVirtualProcessorRoot** ppVirtualProcessorRoots,
    unsigned int count) = 0;
```

### <a name="parameters"></a>Параметры

*ppVirtualProcessorRoots*<br/>
Массив `IVirtualProcessorRoot` интерфейсов, связанных с аппаратными потоками, на которых другие планировщики стали заняты.

*count*<br/>
Количество `IVirtualProcessorRoot` интерфейсов в массиве.

### <a name="remarks"></a>Remarks

Определенный аппаратный поток может быть назначен нескольким планировщикам одновременно. Одной из причин этого может быть то, что в системе недостаточно аппаратных потоков, чтобы удовлетворить минимальную параллель для всех планировщиков, без совместного использования ресурсов. Другая возможность заключается в том, что ресурсы временно назначаются другим планировщикам, когда планировщик-владелец не использует их, в зависимости от всех своих фактических корней процессора на этом аппаратном потоке, деактивируемом.

Уровень подписки аппаратного потока обозначается количеством подписанных потоков и активированными корнями виртуального процессора, связанными с этим аппаратным потоком. С точки зрения конкретного планировщика, внешний уровень подписки аппаратного потока — это часть подписки, в которая вносят свой вклад другие планировщики. Уведомления о том, что ресурсы внешне заняты, отправляются планировщику, когда внешний уровень подписки для аппаратного потока перемещается с нуля на положительную территорию.

Уведомления с помощью этого метода отправляются только планировщикам, у которых есть политика, в которой значение для ключа `MinConcurrency` политики равно значению для ключа `MaxConcurrency` политики. Для получения дополнительной информации о политиках планировщика, [см.](schedulerpolicy-class.md)

Планировщик, квалифицирующийся для уведомлений, получает набор первоначальных уведомлений при его создании, информируя его о том, заняты ли только что назначенные ресурсы или простоя.

## <a name="ischedulernotifyresourcesexternallyidle-method"></a><a name="notifyresourcesexternallyidle"></a>IScheduler::NotifyResourcesExternallyIdle Метод

Уведомляет этот планировщик о том, что аппаратные потоки, `ppVirtualProcessorRoots` представленные набором корней виртуального процессора в массиве, не используются другими планировщиками.

```cpp
virtual void NotifyResourcesExternallyIdle(
    _In_reads_(count) IVirtualProcessorRoot** ppVirtualProcessorRoots,
    unsigned int count) = 0;
```

### <a name="parameters"></a>Параметры

*ppVirtualProcessorRoots*<br/>
Массив `IVirtualProcessorRoot` интерфейсов, связанных с аппаратными потоками, на которых другие планировщики простаивали.

*count*<br/>
Количество `IVirtualProcessorRoot` интерфейсов в массиве.

### <a name="remarks"></a>Remarks

Определенный аппаратный поток может быть назначен нескольким планировщикам одновременно. Одной из причин этого может быть то, что в системе недостаточно аппаратных потоков, чтобы удовлетворить минимальную параллель для всех планировщиков, без совместного использования ресурсов. Другая возможность заключается в том, что ресурсы временно назначаются другим планировщикам, когда планировщик-владелец не использует их, в зависимости от всех своих фактических корней процессора на этом аппаратном потоке, деактивируемом.

Уровень подписки аппаратного потока обозначается количеством подписанных потоков и активированными корнями виртуального процессора, связанными с этим аппаратным потоком. С точки зрения конкретного планировщика, внешний уровень подписки аппаратного потока — это часть подписки, в которая вносят свой вклад другие планировщики. Уведомления о том, что ресурсы внешне заняты, отправляются планировщику, когда внешний уровень подписки для аппаратного потока падает до нуля от предыдущего положительного значения.

Уведомления с помощью этого метода отправляются только планировщикам, у которых есть политика, в которой значение для ключа `MinConcurrency` политики равно значению для ключа `MaxConcurrency` политики. Для получения дополнительной информации о политиках планировщика, [см.](schedulerpolicy-class.md)

Планировщик, квалифицирующийся для уведомлений, получает набор первоначальных уведомлений при его создании, информируя его о том, заняты ли только что назначенные ресурсы или простоя.

## <a name="ischedulerremovevirtualprocessors-method"></a><a name="removevirtualprocessors"></a>I'Scheduler::УдалитьМетодвиртуальныхпроцессоров

Инициирует удаление корней виртуальных процессоров, которые ранее были выделены этому планировщику.

```cpp
virtual void RemoveVirtualProcessors(
    _In_reads_(count) IVirtualProcessorRoot** ppVirtualProcessorRoots,
    unsigned int count) = 0;
```

### <a name="parameters"></a>Параметры

*ppVirtualProcessorRoots*<br/>
Массив интерфейсов, представляющих `IVirtualProcessorRoot` корни виртуального процессора, которые должны быть удалены.

*count*<br/>
Количество `IVirtualProcessorRoot` интерфейсов в массиве.

### <a name="remarks"></a>Remarks

Менеджер ресурсов вызывает `RemoveVirtualProcessors` метод, чтобы забрать набор виртуальных корней процессора из планировщика. Ожидается, что планировщик будет вызывать метод [Удаления](iexecutionresource-structure.md#remove) на каждом интерфейсе, когда это делается с корнями виртуального процессора. Не используйте `IVirtualProcessorRoot` интерфейс, как только `Remove` вы вызвали метод на нем.

Параметр `ppVirtualProcessorRoots` указывает на массив интерфейсов. Среди набора виртуальных корней процессора, которые должны быть удалены, `Remove` корни никогда не были активированы могут быть немедленно возвращены с помощью метода. Корни, которые были активированы и либо выполнения работы, или были отключены и ждут работы прибыть, должны быть возвращены асинхронно. Планировщик должен сделать все попытки удалить корень виртуального процессора как можно быстрее. Задержка удаления корней виртуального процессора может привести к непреднамеренной переподписке в планировщике.

## <a name="ischedulerstatistics-method"></a><a name="statistics"></a>IScheduler::Метод статистики

Предоставляет информацию, связанную с показателями прибытия и завершения задач, а также изменение длины очереди для планировщика.

```cpp
virtual void Statistics(
    _Out_ unsigned int* pTaskCompletionRate,
    _Out_ unsigned int* pTaskArrivalRate,
    _Out_ unsigned int* pNumberOfTasksEnqueued) = 0;
```

### <a name="parameters"></a>Параметры

*pTaskCompletionRate*<br/>
Количество задач, выполненных планировщиком с момента последнего вызова к этому методу.

*pTaskПрибытиеПрибытие*<br/>
Количество задач, поступивших в планировщик с момента последнего вызова к этому методу.

*pNumberOfTasksОбейки*<br/>
Общее количество задач во всех очередях планировщиков.

### <a name="remarks"></a>Remarks

Этот метод вызывается менеджером ресурсов для сбора статистических данных для планировщика. Собранная здесь статистика будет использоваться для определения алгоритмов динамической обратной связи для определения того, когда целесообразно назначать больше ресурсов планировщику и когда отнимаемым ресурсам. Значения, предоставляемые планировщиком, могут быть оптимистичными и не обязательно должны точно отражать текущий подсчет.

Необходимо реализовать этот метод, если требуется, чтобы диспетчер ресурсов использовал отзывы о таких действиях, как прибытие задачи, чтобы определить способ распределения ресурсов между данным планировщиком и другими планировщиками, зарегистрированными диспетчером ресурсов. Если вы решите не собирать статистику, `DynamicProgressFeedback` вы `DynamicProgressFeedbackDisabled` можете установить ключ политики к значению в политике планировщика, и менеджер ресурсов не будет ссылаться на этот метод в планировщике.

В отсутствие статистической информации диспетчер ресурсов будет использовать уровни подписки аппаратных потоков для принятия решений о распределении ресурсов и миграции. Для получения дополнительной информации об уровнях подписки см. [IExecutionResource::CurrentSubscriptionLevel](iexecutionresource-structure.md#currentsubscriptionlevel).

## <a name="see-also"></a>См. также раздел

[Пространство имен concurrency](concurrency-namespace.md)<br/>
[PolicyElementKey](concurrency-namespace-enums.md)<br/>
[Класс SchedulerPolicy](schedulerpolicy-class.md)<br/>
[Структура IExecutionContext](iexecutioncontext-structure.md)<br/>
[Структура IThreadProxy](ithreadproxy-structure.md)<br/>
[Структура IVirtualProcessorRoot](ivirtualprocessorroot-structure.md)<br/>
[Структура IResourceManager](iresourcemanager-structure.md)
