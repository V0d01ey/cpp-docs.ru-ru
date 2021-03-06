---
title: Моментальный снимок
ms.date: 11/04/2016
helpviewer_keywords:
- ODBC cursor library [ODBC], snapshots
- cursors [ODBC], static
- recordsets, snapshots
- snapshots, support in ODBC
- static cursors
- ODBC recordsets, snapshots
- cursor library [ODBC], snapshots
- snapshots
ms.assetid: b5293a52-0657-43e9-bd71-fe3785b21c7e
ms.openlocfilehash: e487b5abcc5eee4e3f4b1941100980eac4a040c8
ms.sourcegitcommit: c123cc76bb2b6c5cde6f4c425ece420ac733bf70
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2020
ms.locfileid: "81366907"
---
# <a name="snapshot"></a>Моментальный снимок

Снимок — это набор записей, отражающий статический вид данных в том виде, в каком он существовал на момент создания снимка. При открытии снимка и перемещении на все записи набор записей, которые он содержит, и их значения не меняются до тех пор, пока вы не восстановите снимок, позвонив `Requery`в .

> [!NOTE]
> Этот раздел относится к классам ODBC библиотеки MFC. Если вы используете классы MFC DAO вместо классов MFC ODBC, [см. CDaoRecordset::Open](../../mfc/reference/cdaorecordset-class.md#open) для описания записей типа моментального снимка.

Можно создавать обновляемые или только для чтения снимки с классами баз данных. В отличие от dynaset, обновляемый снимок не отражает изменения в значениях записи, сделанные другими пользователями, но он отражает обновления и удаления, сделанные вашей программой. Записи, добавленные к моментальнократу, `Requery`не становятся видимыми для снимка до тех пор, пока вы не позвоните.

> [!TIP]
> Снимок — это статический курсор ODBC. Статические курсоры на самом деле не получают ряд данных, пока вы не прокрутите к этой записи. Чтобы убедиться, что все записи немедленно извлечены, можно прокрутить до конца записи, а затем прокрутить первую запись, которую вы хотите увидеть. Обратите внимание, однако, что прокрутка до конца влечет за собой дополнительные накладные расходы и может снизить производительность.

Снимки наиболее ценны, когда требуется, чтобы данные оставались исправленными во время операций, напротив, когда вы создаете отчет или выполняете расчеты. Несмотря на это, источник данных может значительно отличаться от вашего снимка, так что вы можете восстановить его время от времени.

Поддержка моментальных снимков основана на библиотеке ODBC Cursor Library, которая предоставляет статические курсоры и позиционированные обновления (необходимые для обновления) для любого драйвера уровня 1. Библиотека курсора DLL должна быть загружена в память для этой поддержки. При `CDatabase` построении объекта `OpenEx` и вызове его `CDatabase::useCursorLib` функции-члена необходимо указать опцию параметра *dwOptions.* Если вы `Open` называете функцию участника, библиотека курсора загружается по умолчанию. Если вы используете dynasets вместо снимков, вы не хотите, чтобы вызвать библиотеку курсора, чтобы быть загружены.

Снимки доступны только в том случае, если библиотека `CDatabase` ODBC Cursor была загружена при построении объекта или драйвер ODBC, который вы используете, поддерживает статические курсоры.

> [!NOTE]
> Для некоторых драйверов ODBC снимки (статические курсоры) могут быть необновляемыми. Проверьте документацию драйвера на наличие типов курсоров и поддерживаемых ими типов параллелизма. Чтобы гарантировать обновляемые снимки, убедитесь, что вы загружаете библиотеку курсора в память при создании `CDatabase` объекта. Для получения дополнительной информации, [см. ODBC: ODBC Курсор библиотека](../../data/odbc/odbc-the-odbc-cursor-library.md).

> [!NOTE]
> Если вы хотите использовать как снимки, так и динасеты, вы должны основывать их на двух разных `CDatabase` объектах (два разных соединения).

Для получения дополнительной информации о свойствах снимки поделиться со всеми рекордами, см [Recordset (ODBC)](../../data/odbc/recordset-odbc.md). Для получения дополнительной информации о ODBC и снимки, в том числе ODBC Курсор библиотека, [см.](../../data/odbc/odbc-basics.md)

## <a name="see-also"></a>См. также раздел

[Открытая связь с базами данных (ODBC)](../../data/odbc/open-database-connectivity-odbc.md)
