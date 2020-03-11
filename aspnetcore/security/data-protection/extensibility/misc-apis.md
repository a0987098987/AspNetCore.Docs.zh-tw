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
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a><span data-ttu-id="34c9b-103">其他 ASP.NET Core 資料保護 Api</span><span class="sxs-lookup"><span data-stu-id="34c9b-103">Miscellaneous ASP.NET Core Data Protection APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="34c9b-104">實作為下列任何介面的型別應該是多個呼叫端的安全線程。</span><span class="sxs-lookup"><span data-stu-id="34c9b-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="34c9b-105">ISecret</span><span class="sxs-lookup"><span data-stu-id="34c9b-105">ISecret</span></span>

<span data-ttu-id="34c9b-106">`ISecret` 介面代表秘密值，例如密碼編譯金鑰內容。</span><span class="sxs-lookup"><span data-stu-id="34c9b-106">The `ISecret` interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="34c9b-107">其中包含下列 API 介面：</span><span class="sxs-lookup"><span data-stu-id="34c9b-107">It contains the following API surface:</span></span>

* <span data-ttu-id="34c9b-108">`Length`: `int`</span><span class="sxs-lookup"><span data-stu-id="34c9b-108">`Length`: `int`</span></span>

* <span data-ttu-id="34c9b-109">`Dispose()`: `void`</span><span class="sxs-lookup"><span data-stu-id="34c9b-109">`Dispose()`: `void`</span></span>

* <span data-ttu-id="34c9b-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span><span class="sxs-lookup"><span data-stu-id="34c9b-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span></span>

<span data-ttu-id="34c9b-111">`WriteSecretIntoBuffer` 方法會將原始密碼值填入提供的緩衝區。</span><span class="sxs-lookup"><span data-stu-id="34c9b-111">The `WriteSecretIntoBuffer` method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="34c9b-112">此 API 會將緩衝區當做參數，而不是直接傳回 `byte[]`，這可讓呼叫者有機會釘選緩衝區物件，以限制受管理垃圾收集行程的秘密暴露。</span><span class="sxs-lookup"><span data-stu-id="34c9b-112">The reason this API takes the buffer as a parameter rather than returning a `byte[]` directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="34c9b-113">`Secret` 類型是 `ISecret` 的具體執行，其中的秘密值會儲存在同進程記憶體中。</span><span class="sxs-lookup"><span data-stu-id="34c9b-113">The `Secret` type is a concrete implementation of `ISecret` where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="34c9b-114">在 Windows 平臺上，密碼值是透過[CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx)加密。</span><span class="sxs-lookup"><span data-stu-id="34c9b-114">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
