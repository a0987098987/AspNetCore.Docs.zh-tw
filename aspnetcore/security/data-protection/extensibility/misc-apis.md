---
title: "其他 API"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 512c6ba7-88ec-47e4-a656-6b30350b34e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 72274a0da94a14bcd5af11d245ba626214fa2607
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/13/2017
---
# <a name="miscellaneous-apis"></a>其他 API

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> 實作下列介面的型別應該是安全執行緒的多個呼叫端。

## <a name="isecret"></a>ISecret

ISecret 介面代表密碼的值，例如密碼編譯金鑰內容。 它包含下列 API 介面。

* 長度： int

* Dispose （): void

* WriteSecretIntoBuffer (ArraySegment<byte>緩衝區): void

WriteSecretIntoBuffer 方法會填入與原始的祕密值提供的緩衝區。 這個 API 接受緩衝區做為參數，而不是傳回 byte []，直接這讓呼叫者有機會釘選的緩衝區物件，以限制受管理的記憶體回收行程密碼曝光原因。

此密碼的類型為的 ISecret 祕密值儲存在同處理序記憶體中的具象實作。 Windows 平台上，此密碼的值會加密透過[CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx)。
