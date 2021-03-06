---
title: Структура SYMBOL_NAME_DATA
description: Справочник по структуре SYMBOL_NAME_DATA из пакета SDK для Аналитики сборок C++.
ms.date: 02/12/2020
helpviewer_keywords:
- C++ Build Insights
- C++ Build Insights SDK
- SYMBOL_NAME_DATA
- throughput analysis
- build time analysis
- vcperf.exe
ms.openlocfilehash: 08c09f304f8cc90bd48a2cecc6771d90c997a7f4
ms.sourcegitcommit: 9c2b3df9b837879cd17932ae9f61cdd142078260
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2020
ms.locfileid: "92920935"
---
# <a name="symbol_name_data-structure"></a>Структура SYMBOL_NAME_DATA

::: moniker range="<=msvc-140"

Пакет SDK Аналитики сборок С++ совместим с Visual Studio 2017 и более поздних версий. Чтобы увидеть документацию для этих версий, установите в данной статье селектор **Версия** Visual Studio в Visual Studio 2017 или Visual Studio 2019. Он находится в верхней части оглавления на этой странице.

::: moniker-end
::: moniker range=">=msvc-150"

В структуре `SYMBOL_NAME_DATA` описывается символ внешнего интерфейса компилятора.

## <a name="syntax"></a>Синтаксис

```cpp
typedef struct SYMBOL_NAME_DATA_TAG
{
    unsigned long long  Key;
    const char*         Name;

} SYMBOL_NAME_DATA;
```

## <a name="members"></a>Участники

| Имя | Описание |
|--|--|
| `Key` | Ключ символа. Это значение уникально в пределах анализируемой трассировки. |
| `Name` | Имя символа. |

## <a name="remarks"></a>Remarks

Символы, созданные на двух разных этапах внешнего интерфейса компилятора, могут уметь одинаковое имя но разные ключи. В этом случае используйте имена символов, чтобы определить, являются ли два типа идентичными.

::: moniker-end
