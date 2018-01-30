---
title: "其他 API"
author: rick-anderson
description: "本文概述 ASP.NET Core 資料保護 ISecret 介面。"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 26474de3a1c681a8c7f8b7812e9ef859f5d448bb
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="miscellaneous-apis"></a>其他 API

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> 實作下列介面的型別應該是安全執行緒的多個呼叫端。

## <a name="isecret"></a>ISecret

`ISecret`介面表示密碼的值，例如密碼編譯金鑰內容。 它包含下列 API 介面：

* `Length`: `int`

* `Dispose()`: `void`

* `WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`

`WriteSecretIntoBuffer`方法會填入與原始的祕密值提供的緩衝區。 這個 API 會做為參數緩衝區的原因而不是傳回`byte[]`直接是這讓呼叫者有機會釘選的緩衝區物件，以限制受管理的記憶體回收行程密碼曝光。

`Secret`類型是具象實作`ISecret`祕密值儲存在同處理序記憶體中的位置。 Windows 平台上，此密碼的值會加密透過[CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx)。
