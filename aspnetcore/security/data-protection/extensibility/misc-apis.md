---
title: 其他 ASP.NET Core 資料保護 Api
author: rick-anderson
description: 瞭解 ASP.NET Core 資料保護 ISecret 介面。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 114cdd6209970e46b827e403fbe79b95692d0242
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78666079"
---
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a>其他 ASP.NET Core 資料保護 Api

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> 實作為下列任何介面的型別應該是多個呼叫端的安全線程。

## <a name="isecret"></a>ISecret

`ISecret` 介面代表秘密值，例如密碼編譯金鑰內容。 其中包含下列 API 介面：

* `Length`: `int`

* `Dispose()`: `void`

* `WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`

`WriteSecretIntoBuffer` 方法會將原始密碼值填入提供的緩衝區。 此 API 會將緩衝區當做參數，而不是直接傳回 `byte[]`，這可讓呼叫者有機會釘選緩衝區物件，以限制受管理垃圾收集行程的秘密暴露。

`Secret` 類型是 `ISecret` 的具體執行，其中的秘密值會儲存在同進程記憶體中。 在 Windows 平臺上，密碼值是透過[CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx)加密。
