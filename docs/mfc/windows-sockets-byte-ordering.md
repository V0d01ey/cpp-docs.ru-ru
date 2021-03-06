---
title: Сокеты Windows. Порядок байтов
ms.date: 11/04/2016
helpviewer_keywords:
- byte order issues in sockets programming
- sockets [MFC], byte order issues
- Windows Sockets [MFC], byte order issues
ms.assetid: 8a787a65-f9f4-4002-a02f-ac25a5dace5d
ms.openlocfilehash: f00936f3de07df8c1e4d9df1c678b2cfd5f3e3ad
ms.sourcegitcommit: 1f009ab0f2cc4a177f2d1353d5a38f164612bdb1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/27/2020
ms.locfileid: "87226806"
---
# <a name="windows-sockets-byte-ordering"></a>Сокеты Windows. Порядок байтов

В этой статье и двух сопутствующих статьях объясняются некоторые проблемы, связанные с программированием Windows Sockets. В этой статье описывается порядок байтов. Другие проблемы описаны в статьях: [сокеты Windows: Блокировка](../mfc/windows-sockets-blocking.md) и [сокеты Windows: преобразование строк](../mfc/windows-sockets-converting-strings.md).

Если вы используете или наследуете от класса [CAsyncSocket](../mfc/reference/casyncsocket-class.md), вам придется самостоятельно управлять этими проблемами. Если вы используете или наследуете от класса [CSocket](../mfc/reference/csocket-class.md), MFC управляет ими.

## <a name="byte-ordering"></a>Порядок байтов

Различные архитектуры компьютеров иногда хранят данные, используя различные порядки байтов. Например, компьютеры на базе технологии Intel хранят данные в противоположном порядке компьютеров Macintosh (Motorola). Порядок байтов Intel, называемый прямым порядком байтов, также является обратным по отношению к сетевому стандарту с обратным порядковым порядком байтов. Эти термины объясняются в следующей таблице.

### <a name="big--and-little-endian-byte-ordering"></a>Big-и с прямым порядком байтов

|Порядок байтов|Значение|
|-------------------|-------------|
|С обратным порядком байтов|Наиболее значимый байт находится в левом конце слова.|
|С прямым порядком байтов|Наиболее значимый байт находится в правом конце слова.|

Как правило, не нужно беспокоиться о преобразовании порядка байтов для данных, отправляемых и получаемых по сети, но существуют ситуации, в которых необходимо преобразовать заказы байтов.

## <a name="when-you-must-convert-byte-orders"></a>Когда необходимо преобразовать порядок байтов

Порядок байтов необходимо преобразовать в следующих ситуациях:

- Вы передаете сведения, которые должны интерпретироваться сетью, а не данные, отправляемые на другой компьютер. Например, можно передать порты и адреса, которые должны быть понятны сети.

- Серверное приложение, с которым вы обмениваетесь данными, не является приложением MFC (и у вас нет исходного кода). Это вызывает преобразование порядка байтов, если два компьютера не имеют одинаковый порядок байтов.

## <a name="when-you-do-not-have-to-convert-byte-orders"></a>Если не нужно преобразовывать порядок байтов

Вы можете избежать операций преобразования порядка байтов в следующих ситуациях:

- Компьютеры на обоих концах могут согласиться не менять байты, и оба компьютера используют одинаковый порядок байтов.

- Сервер, с которым вы обмениваетесь данными, является приложением MFC.

- У вас есть исходный код для сервера, с которым вы обмениваетесь данными, поэтому вы можете явно определить, нужно ли преобразовывать побайтовые заказы.

- Сервер можно перенести в MFC. Это довольно просто, и результат обычно меньше, чем быстрее.

Работая с [CAsyncSocket](../mfc/reference/casyncsocket-class.md), необходимо самостоятельно управлять любыми необходимыми преобразованиями порядка байтов. Сокеты Windows стандартизируют модель порядка байтов с обратным порядком следования и предоставляют функции для преобразования между этим заказом и другими. Однако [CArchive](../mfc/reference/carchive-class.md), который используется с [CSocket](../mfc/reference/csocket-class.md), использует противоположный порядок ("с прямым порядком следования байтов)", но `CArchive` позаботится о преобразованиях порядка байтов. Используя это стандартное упорядочение в приложениях или используя функции преобразования порядка следования байтов в Windows Sockets, можно сделать код более переносимым.

Идеальным вариантом использования сокетов MFC является ситуация, когда вы пишете оба конца связи: использование MFC на обоих концах. Если вы создаете приложение, которое будет взаимодействовать с приложениями, не использующими MFC, например с FTP-сервером, вам, вероятно, придется самостоятельно управлять обменом байтов перед передачей данных в архивный объект, используя подпрограммы преобразования сокетов Windows **нтохс**, **нтохл**, **хтонс**и **хтонл**. Ниже в этой статье приводятся примеры этих функций, используемых при взаимодействии с приложениями, не использующими MFC.

> [!NOTE]
> Если другой конец обмена данными не является приложением MFC, необходимо также избегать потоковой передачи объектов C++, производных от, `CObject` в архив, так как получатель не сможет их обрабатывать. См. Примечание в разделе [сокеты Windows: Использование сокетов с архивами](../mfc/windows-sockets-using-sockets-with-archives.md).

Дополнительные сведения о побайтовых заказах см. в разделе Спецификация сокетов Windows, доступная в Windows SDK.

## <a name="a-byte-order-conversion-example"></a>Пример преобразования порядка байтов

В следующем примере показана функция сериализации для `CSocket` объекта, который использует Архив. Он также иллюстрирует использование функций преобразования порядка байтов в API-интерфейсе Windows Sockets.

В этом примере представлен сценарий, в котором выполняется запись клиента, взаимодействующего с серверным приложением, не являющимся MFC, для которого отсутствует доступ к исходному коду. В этом сценарии необходимо предположить, что сервер, не являющийся сервером MFC, использует стандартный сетевой порядок байтов. В отличие от этого, клиентское приложение MFC использует `CArchive` объект с `CSocket` объектом и `CArchive` использует порядок байтов с прямым порядком следования, противоположный стандарту сети.

Предположим, что сервер, не относящийся к MFC, с которым планируется обмениваться данными, имеет установленный протокол для пакета сообщений, который выглядит следующим образом:

[!code-cpp[NVC_MFCSimpleSocket#5](../mfc/codesnippet/cpp/windows-sockets-byte-ordering_1.cpp)]

В терминах MFC это выражение выражается следующим образом:

[!code-cpp[NVC_MFCSimpleSocket#6](../mfc/codesnippet/cpp/windows-sockets-byte-ordering_2.cpp)]

В C++, по **`struct`** сути, это то же самое, что и класс. `Message`Структура может иметь функции элементов, например `Serialize` функцию члена, объявленную выше. `Serialize`Функция члена может выглядеть следующим образом:

[!code-cpp[NVC_MFCSimpleSocket#7](../mfc/codesnippet/cpp/windows-sockets-byte-ordering_3.cpp)]

В этом примере вызывается преобразование порядка байтов данных, так как существует четкое несоответствие порядка байтов серверного приложения, не относящегося к MFC, на одном конце и `CArchive` используемого в клиентском приложении MFC на другом конце. В примере показаны некоторые функции преобразования порядка байтов, предоставляемые сокетами Windows. Эти функции описаны в следующей таблице.

### <a name="windows-sockets-byte-order-conversion-functions"></a>Функции преобразования порядка байтов сокетов Windows

|Функция|Назначение|
|--------------|-------------|
|**нтохс**|Преобразование 16-разрядного количества из сетевого байтового порядка в порядок размещения байтов (с обратным порядком байтов (Big-endian).|
|**нтохл**|Преобразование 32-разрядного количества из сетевого байтового порядка в порядковый номер размещения в байтах (с обратным порядком байтов).|
|**хтонс**|Преобразует 16-разрядное количество из байтового порядка узла в сетевой байтовый порядок (с прямым порядком байтов с обратным порядком байтов).|
|**хтонл**|Преобразование 32-разрядного количества из байтового порядка узла в сетевой байтовый порядок (с прямым порядком байтов с обратным порядком байтов).|

Еще одной точкой этого примера является то, что если приложение сокета на другом конце связи является приложением, не являющимся MFC, необходимо избегать выполнения следующих действий:

`ar << pMsg;`

где `pMsg` — указатель на объект C++, производный от класса `CObject` . При этом будут отправлены дополнительные сведения MFC, связанные с объектами, и сервер не сможет понять его, как если бы он был приложением MFC.

Дополнительные сведения см. в разделе:

- [Сокеты Windows: использование класса CAsyncSocket](../mfc/windows-sockets-using-class-casyncsocket.md)

- [Сокеты Windows. Фон](../mfc/windows-sockets-background.md)

- [Сокеты Windows: сокеты потоков](../mfc/windows-sockets-stream-sockets.md)

- [Сокеты Windows: сокеты датаграммы](../mfc/windows-sockets-datagram-sockets.md)

## <a name="see-also"></a>См. также статью

[Сокеты Windows в MFC](../mfc/windows-sockets-in-mfc.md)
