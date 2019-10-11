---
title: ASP.NET Core 中的要求和回應作業
author: jkotalik
description: 了解如何在 ASP.NET Core 中讀取要求本文及寫入回應本文。
monikerRange: '>= aspnetcore-3.0'
ms.author: jukotali
ms.custom: mvc
ms.date: 08/29/2019
uid: fundamentals/middleware/request-response
ms.openlocfilehash: 5e531c0ce0ed48097054fd81ddc3655a66cc7c5f
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2019
ms.locfileid: "71081677"
---
# <a name="request-and-response-operations-in-aspnet-core"></a><span data-ttu-id="392ae-103">ASP.NET Core 中的要求和回應作業</span><span class="sxs-lookup"><span data-stu-id="392ae-103">Request and response operations in ASP.NET Core</span></span>

<span data-ttu-id="392ae-104">作者：[Justin Kotalik](https://github.com/jkotalik)</span><span class="sxs-lookup"><span data-stu-id="392ae-104">By [Justin Kotalik](https://github.com/jkotalik)</span></span>

<span data-ttu-id="392ae-105">此文章說明如何讀取要求本文及寫入回應本文。</span><span class="sxs-lookup"><span data-stu-id="392ae-105">This article explains how to read from the request body and write to the response body.</span></span> <span data-ttu-id="392ae-106">撰寫中介軟體時，可能需要這些作業的程式碼。</span><span class="sxs-lookup"><span data-stu-id="392ae-106">Code for these operations might be required when writing middleware.</span></span> <span data-ttu-id="392ae-107">在撰寫中介軟體之外，自訂程式碼通常不是必要的，因為作業是由 MVC 和 Razor Pages 處理。</span><span class="sxs-lookup"><span data-stu-id="392ae-107">Outside of writing middleware, custom code isn't generally required because the operations are handled by MVC and Razor Pages.</span></span>

<span data-ttu-id="392ae-108">要求和回應主體有兩個抽象概念： <xref:System.IO.Stream>和。 <xref:System.IO.Pipelines.Pipe></span><span class="sxs-lookup"><span data-stu-id="392ae-108">There are two abstractions for the request and response bodies: <xref:System.IO.Stream> and <xref:System.IO.Pipelines.Pipe>.</span></span> <span data-ttu-id="392ae-109">對於要求讀取，[HttpRequest.Body](xref:Microsoft.AspNetCore.Http.HttpRequest.Body) 是 <xref:System.IO.Stream>，而 `HttpRequest.BodyReader` 是 <xref:System.IO.Pipelines.PipeReader>。</span><span class="sxs-lookup"><span data-stu-id="392ae-109">For request reading, [HttpRequest.Body](xref:Microsoft.AspNetCore.Http.HttpRequest.Body) is a <xref:System.IO.Stream>, and `HttpRequest.BodyReader` is a <xref:System.IO.Pipelines.PipeReader>.</span></span> <span data-ttu-id="392ae-110">針對回應寫入， [HttpResponse](xref:Microsoft.AspNetCore.Http.HttpResponse.Body)是<xref:System.IO.Stream> <xref:System.IO.Pipelines.PipeWriter>，而`HttpResponse.BodyWriter`是。</span><span class="sxs-lookup"><span data-stu-id="392ae-110">For response writing, [HttpResponse.Body](xref:Microsoft.AspNetCore.Http.HttpResponse.Body) is a <xref:System.IO.Stream>, and `HttpResponse.BodyWriter` is a <xref:System.IO.Pipelines.PipeWriter>.</span></span>

<span data-ttu-id="392ae-111">建議在資料流程上使用管線。</span><span class="sxs-lookup"><span data-stu-id="392ae-111">Pipelines are recommended over streams.</span></span> <span data-ttu-id="392ae-112">資料流可以更容易地用於一些簡單的作業，但管線具有效能優勢，並且更容易在大部分情況下使用。</span><span class="sxs-lookup"><span data-stu-id="392ae-112">Streams can be easier to use for some simple operations, but pipelines have a performance advantage and are easier to use in most scenarios.</span></span> <span data-ttu-id="392ae-113">ASP.NET Core 開始使用管線，而不是內部的資料流程。</span><span class="sxs-lookup"><span data-stu-id="392ae-113">ASP.NET Core is starting to use pipelines instead of streams internally.</span></span> <span data-ttu-id="392ae-114">範例包括：</span><span class="sxs-lookup"><span data-stu-id="392ae-114">Examples include:</span></span>

* `FormReader`
* `TextReader`
* `TextWriter`
* `HttpResponse.WriteAsync`

<span data-ttu-id="392ae-115">未從架構中移除資料流程。</span><span class="sxs-lookup"><span data-stu-id="392ae-115">Streams aren't being removed from the framework.</span></span> <span data-ttu-id="392ae-116">資料流程會繼續在整個 .net 中使用，而且許多資料流程類型沒有對等的管道， `FileStreams`例如`ResponseCompression`和。</span><span class="sxs-lookup"><span data-stu-id="392ae-116">Streams continue to be used throughout .NET, and many stream types don't have pipe equivalents, such as `FileStreams` and `ResponseCompression`.</span></span>

## <a name="stream-examples"></a><span data-ttu-id="392ae-117">資料流範例</span><span class="sxs-lookup"><span data-stu-id="392ae-117">Stream examples</span></span>

<span data-ttu-id="392ae-118">假設目標是要建立一個中介軟體，將整個要求本文當做字串清單來讀取，並在新行上分割。</span><span class="sxs-lookup"><span data-stu-id="392ae-118">Suppose the goal is to create a middleware that reads the entire request body as a list of strings, splitting on new lines.</span></span> <span data-ttu-id="392ae-119">簡單的資料流實作看起來可能像下列範例這樣：</span><span class="sxs-lookup"><span data-stu-id="392ae-119">A simple stream implementation might look like the following example:</span></span>

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStream)]

<span data-ttu-id="392ae-120">此程式碼確實有用，但有一些問題：</span><span class="sxs-lookup"><span data-stu-id="392ae-120">This code works, but there are some issues:</span></span>

* <span data-ttu-id="392ae-121">附加到 `StringBuilder` 之前，此範例會建立另一個會立即棄置的字串 (`encodedString`)。</span><span class="sxs-lookup"><span data-stu-id="392ae-121">Before appending to the `StringBuilder`, the example creates another string (`encodedString`) that is thrown away immediately.</span></span> <span data-ttu-id="392ae-122">系統會對資料流中的所有位元組執行此處理序，因此整個要求本文會額外耗用記憶體。</span><span class="sxs-lookup"><span data-stu-id="392ae-122">This process occurs for all bytes in the stream, so the result is extra memory allocation the size of the entire request body.</span></span>
* <span data-ttu-id="392ae-123">此範例會在於新行上進行分割之前讀取整個字串。</span><span class="sxs-lookup"><span data-stu-id="392ae-123">The example reads the entire string before splitting on new lines.</span></span> <span data-ttu-id="392ae-124">檢查位元組陣列中的新行會更有效率。</span><span class="sxs-lookup"><span data-stu-id="392ae-124">It's more efficient to check for new lines in the byte array.</span></span>

<span data-ttu-id="392ae-125">以下是修正上述幾個問題的範例：</span><span class="sxs-lookup"><span data-stu-id="392ae-125">Here's an example that fixes some of the preceding issues:</span></span>

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStreamMoreEfficient)]

<span data-ttu-id="392ae-126">前述範例：</span><span class="sxs-lookup"><span data-stu-id="392ae-126">This preceding example:</span></span>

* <span data-ttu-id="392ae-127">不會針對 `StringBuilder` 中的整個要求本文進行緩衝處理，除非沒有任何新行字元。</span><span class="sxs-lookup"><span data-stu-id="392ae-127">Doesn't buffer the entire request body in a `StringBuilder` unless there aren't any newline characters.</span></span>
* <span data-ttu-id="392ae-128">不會在字串上呼叫 `Split`。</span><span class="sxs-lookup"><span data-stu-id="392ae-128">Doesn't call `Split` on the string.</span></span>

<span data-ttu-id="392ae-129">但是仍有幾個問題：</span><span class="sxs-lookup"><span data-stu-id="392ae-129">However, there are still are a few issues:</span></span>

* <span data-ttu-id="392ae-130">如果換行字元是稀疏的，則會在字串中緩衝處理大部分的要求主體。</span><span class="sxs-lookup"><span data-stu-id="392ae-130">If newline characters are sparse, much of the request body is buffered in the string.</span></span>
* <span data-ttu-id="392ae-131">程式碼會繼續建立字串（`remainingString`），並將它們新增至字串緩衝區，這會導致額外的配置。</span><span class="sxs-lookup"><span data-stu-id="392ae-131">The code continues to create strings (`remainingString`) and adds them to the string buffer, which results in an extra allocation.</span></span>

<span data-ttu-id="392ae-132">這些問題都是可修復的，但程式碼變得越來越複雜，而且沒有任何改善。</span><span class="sxs-lookup"><span data-stu-id="392ae-132">These issues are fixable, but the code is becoming progressively more complicated with little improvement.</span></span> <span data-ttu-id="392ae-133">管線提供解決這些問題的方法，且具有最低的程式碼複雜度。</span><span class="sxs-lookup"><span data-stu-id="392ae-133">Pipelines provide a way to solve these problems with minimal code complexity.</span></span>

## <a name="pipelines"></a><span data-ttu-id="392ae-134">管線</span><span class="sxs-lookup"><span data-stu-id="392ae-134">Pipelines</span></span>

<span data-ttu-id="392ae-135">下列範例示範如何使用 `PipeReader` 處理相同的狀況：</span><span class="sxs-lookup"><span data-stu-id="392ae-135">The following example shows how the same scenario can be handled using a `PipeReader`:</span></span>

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringFromPipe)]

<span data-ttu-id="392ae-136">此範例可修正資料流實作的許多問題：</span><span class="sxs-lookup"><span data-stu-id="392ae-136">This example fixes many issues that the streams implementations had:</span></span>

* <span data-ttu-id="392ae-137">不需要字串緩衝區，因為 `PipeReader` 會處理尚未使用位元組。</span><span class="sxs-lookup"><span data-stu-id="392ae-137">There's no need for a string buffer because the `PipeReader` handles bytes that haven't been used.</span></span>
* <span data-ttu-id="392ae-138">編碼的字串會直接新增至所傳回字串的清單。</span><span class="sxs-lookup"><span data-stu-id="392ae-138">Encoded strings are directly added to the list of returned strings.</span></span>
* <span data-ttu-id="392ae-139">除了字串 (除了 `ToArray()` 呼叫之外) 所使用的記憶體，系統不會配置資源給字串建立作業。</span><span class="sxs-lookup"><span data-stu-id="392ae-139">String creation is allocation-free besides the memory used by the string (except the `ToArray()` call).</span></span>

## <a name="adapters"></a><span data-ttu-id="392ae-140">配接器</span><span class="sxs-lookup"><span data-stu-id="392ae-140">Adapters</span></span>

<span data-ttu-id="392ae-141">和屬性都適用于`HttpRequest`和。 `HttpResponse` `BodyReader/BodyWriter` `Body`</span><span class="sxs-lookup"><span data-stu-id="392ae-141">Both `Body` and `BodyReader/BodyWriter` properties are available for `HttpRequest` and `HttpResponse`.</span></span> <span data-ttu-id="392ae-142">當您將`Body`設定為不同的資料流程時，一組新的介面卡會自動將每個類型調整成另一種。</span><span class="sxs-lookup"><span data-stu-id="392ae-142">When you set `Body` to a different stream, a new set of adapters automatically adapt each type to the other.</span></span> <span data-ttu-id="392ae-143">如果您將`HttpRequest.Body`設定為新的資料流程`HttpRequest.BodyReader` ，會自動設為包裝`PipeReader` `HttpRequest.Body`的新。</span><span class="sxs-lookup"><span data-stu-id="392ae-143">If you set `HttpRequest.Body` to a new stream, `HttpRequest.BodyReader` is automatically set to a new `PipeReader` that wraps `HttpRequest.Body`.</span></span>

## <a name="startasync"></a><span data-ttu-id="392ae-144">StartAsync</span><span class="sxs-lookup"><span data-stu-id="392ae-144">StartAsync</span></span>

<span data-ttu-id="392ae-145">`HttpResponse.StartAsync` 是用來指出標頭是不可修改的，且指出要執行 `OnStarting` 回呼。</span><span class="sxs-lookup"><span data-stu-id="392ae-145">`HttpResponse.StartAsync` is used to indicate that headers are unmodifiable and to run `OnStarting` callbacks.</span></span> <span data-ttu-id="392ae-146">使用 Kestrel 做為伺服器時，在 `PipeReader` 之前呼叫 `StartAsync`  可確保 `GetMemory` 所傳回的記憶體屬於 Kestrel 的內部 <xref:System.IO.Pipelines.Pipe>，而不是屬於 外部緩衝區。</span><span class="sxs-lookup"><span data-stu-id="392ae-146">When using Kestrel as a server, calling `StartAsync` before using the `PipeReader` guarantees that memory returned by `GetMemory` belongs to Kestrel's internal <xref:System.IO.Pipelines.Pipe> rather than an external buffer.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="392ae-147">其他資源</span><span class="sxs-lookup"><span data-stu-id="392ae-147">Additional resources</span></span>

* <span data-ttu-id="392ae-148">[System.IO.Pipelines 簡介](https://devblogs.microsoft.com/dotnet/system-io-pipelines-high-performance-io-in-net/) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="392ae-148">[Introducing System.IO.Pipelines](https://devblogs.microsoft.com/dotnet/system-io-pipelines-high-performance-io-in-net/)</span></span>
* <xref:fundamentals/middleware/write>
