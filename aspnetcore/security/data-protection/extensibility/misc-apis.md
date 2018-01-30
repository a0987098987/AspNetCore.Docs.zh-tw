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
# <a name="miscellaneous-apis"></a><span data-ttu-id="04663-103">其他 API</span><span class="sxs-lookup"><span data-stu-id="04663-103">Miscellaneous APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="04663-104">實作下列介面的型別應該是安全執行緒的多個呼叫端。</span><span class="sxs-lookup"><span data-stu-id="04663-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="04663-105">ISecret</span><span class="sxs-lookup"><span data-stu-id="04663-105">ISecret</span></span>

<span data-ttu-id="04663-106">`ISecret`介面表示密碼的值，例如密碼編譯金鑰內容。</span><span class="sxs-lookup"><span data-stu-id="04663-106">The `ISecret` interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="04663-107">它包含下列 API 介面：</span><span class="sxs-lookup"><span data-stu-id="04663-107">It contains the following API surface:</span></span>

* <span data-ttu-id="04663-108">`Length`: `int`</span><span class="sxs-lookup"><span data-stu-id="04663-108">`Length`: `int`</span></span>

* <span data-ttu-id="04663-109">`Dispose()`: `void`</span><span class="sxs-lookup"><span data-stu-id="04663-109">`Dispose()`: `void`</span></span>

* <span data-ttu-id="04663-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span><span class="sxs-lookup"><span data-stu-id="04663-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span></span>

<span data-ttu-id="04663-111">`WriteSecretIntoBuffer`方法會填入與原始的祕密值提供的緩衝區。</span><span class="sxs-lookup"><span data-stu-id="04663-111">The `WriteSecretIntoBuffer` method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="04663-112">這個 API 會做為參數緩衝區的原因而不是傳回`byte[]`直接是這讓呼叫者有機會釘選的緩衝區物件，以限制受管理的記憶體回收行程密碼曝光。</span><span class="sxs-lookup"><span data-stu-id="04663-112">The reason this API takes the buffer as a parameter rather than returning a `byte[]` directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="04663-113">`Secret`類型是具象實作`ISecret`祕密值儲存在同處理序記憶體中的位置。</span><span class="sxs-lookup"><span data-stu-id="04663-113">The `Secret` type is a concrete implementation of `ISecret` where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="04663-114">Windows 平台上，此密碼的值會加密透過[CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx)。</span><span class="sxs-lookup"><span data-stu-id="04663-114">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
