---
title: "其他 API"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 512c6ba7-88ec-47e4-a656-6b30350b34e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 541dd721a00495632f0d633577b55933c9be03fa
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2017
---
# <a name="miscellaneous-apis"></a><span data-ttu-id="aaa58-103">其他 API</span><span class="sxs-lookup"><span data-stu-id="aaa58-103">Miscellaneous APIs</span></span>

<a name=data-protection-extensibility-mics-apis></a>

>[!WARNING]
> <span data-ttu-id="aaa58-104">實作下列介面的型別應該是安全執行緒的多個呼叫端。</span><span class="sxs-lookup"><span data-stu-id="aaa58-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="aaa58-105">ISecret</span><span class="sxs-lookup"><span data-stu-id="aaa58-105">ISecret</span></span>

<span data-ttu-id="aaa58-106">ISecret 介面代表密碼的值，例如密碼編譯金鑰內容。</span><span class="sxs-lookup"><span data-stu-id="aaa58-106">The ISecret interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="aaa58-107">它包含下列 API 介面。</span><span class="sxs-lookup"><span data-stu-id="aaa58-107">It contains the following API surface.</span></span>

* <span data-ttu-id="aaa58-108">長度： int</span><span class="sxs-lookup"><span data-stu-id="aaa58-108">Length : int</span></span>

* <span data-ttu-id="aaa58-109">Dispose （): void</span><span class="sxs-lookup"><span data-stu-id="aaa58-109">Dispose() : void</span></span>

* <span data-ttu-id="aaa58-110">WriteSecretIntoBuffer (ArraySegment<byte>緩衝區): void</span><span class="sxs-lookup"><span data-stu-id="aaa58-110">WriteSecretIntoBuffer(ArraySegment<byte> buffer) : void</span></span>

<span data-ttu-id="aaa58-111">WriteSecretIntoBuffer 方法會填入與原始的祕密值提供的緩衝區。</span><span class="sxs-lookup"><span data-stu-id="aaa58-111">The WriteSecretIntoBuffer method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="aaa58-112">這個 API 接受緩衝區做為參數，而不是傳回 byte []，直接這讓呼叫者有機會釘選的緩衝區物件，以限制受管理的記憶體回收行程密碼曝光原因。</span><span class="sxs-lookup"><span data-stu-id="aaa58-112">The reason this API takes the buffer as a parameter rather than returning a byte[] directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="aaa58-113">此密碼的類型為的 ISecret 祕密值儲存在同處理序記憶體中的具象實作。</span><span class="sxs-lookup"><span data-stu-id="aaa58-113">The Secret type is a concrete implementation of ISecret where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="aaa58-114">Windows 平台上，此密碼的值會加密透過[CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx)。</span><span class="sxs-lookup"><span data-stu-id="aaa58-114">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
