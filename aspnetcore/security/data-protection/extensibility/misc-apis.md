---
title: 其他 ASP.NET Core 資料保護 Api
author: rick-anderson
description: 瞭解 ASP.NET Core 資料保護 ISecret 介面。
ms.author: riande
ms.date: 10/14/2016
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: a07ccc3645a9a8132fd5290e7c43f353f74aca05
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82776977"
---
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a>其他 ASP.NET Core 資料保護 Api

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> 實作為下列任何介面的型別應該是多個呼叫端的安全線程。

## <a name="isecret"></a>ISecret

`ISecret`介面代表秘密值，例如密碼編譯金鑰內容。 其中包含下列 API 介面：

* `Length`: `int`

* `Dispose()`: `void`

* `WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`

`WriteSecretIntoBuffer`方法會將原始秘密值填入提供的緩衝區。 此 API 將緩衝區當做參數使用，而不是`byte[]`直接傳回的原因是，這讓呼叫者有機會釘選緩衝區物件，以限制受管理垃圾收集行程的秘密暴露。

`Secret`型別是的具體執行， `ISecret`其中的秘密值會儲存在同進程記憶體中。 在 Windows 平臺上，密碼值是透過[CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx)加密。
