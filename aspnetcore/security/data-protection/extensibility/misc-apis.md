---
title: 其他 ASP.NET Core 資料保護 Api
author: rick-anderson
description: 深入了解 ASP.NET Core 資料保護 ISecret 介面。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 114cdd6209970e46b827e403fbe79b95692d0242
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64896615"
---
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a>其他 ASP.NET Core 資料保護 Api

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> 會實作下列介面的任何的類型應該是安全執行緒的多個呼叫端。

## <a name="isecret"></a>ISecret

`ISecret`介面代表祕密的值，例如密碼編譯金鑰的內容。 它包含下列的 API 介面：

* `Length`: `int`

* `Dispose()`: `void`

* `WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`

`WriteSecretIntoBuffer`方法會填入與原始的祕密值提供的緩衝區。 此 API 會做為參數緩衝區的原因而不會傳回`byte[]`直接是這讓呼叫端若要釘選緩衝區物件，限制受管理的記憶體回收行程的祕密曝光機會。

`Secret`類型是具象實作`ISecret`同處理序記憶體中儲存的祕密的值。 在 Windows 平台，透過加密祕密值[CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx)。
