---
title: ASP.NET Core 中的要求和回應作業
author: jkotalik
description: 了解如何在 ASP.NET Core 中讀取要求本文及寫入回應本文。
monikerRange: '>= aspnetcore-3.0'
ms.author: jukotali
ms.custom: mvc
ms.date: 08/29/2019
uid: fundamentals/middleware/request-response
ms.openlocfilehash: b473fa02e1d23f02bc5d2e15fa54ab7b1dbbb17c
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667213"
---
# <a name="request-and-response-operations-in-aspnet-core"></a><span data-ttu-id="b0c0f-103">ASP.NET Core 中的要求和回應作業</span><span class="sxs-lookup"><span data-stu-id="b0c0f-103">Request and response operations in ASP.NET Core</span></span>

<span data-ttu-id="b0c0f-104">作者：[Justin Kotalik](https://github.com/jkotalik)</span><span class="sxs-lookup"><span data-stu-id="b0c0f-104">By [Justin Kotalik](https://github.com/jkotalik)</span></span>

<span data-ttu-id="b0c0f-105">此文章說明如何讀取要求本文及寫入回應本文。</span><span class="sxs-lookup"><span data-stu-id="b0c0f-105">This article explains how to read from the request body and write to the response body.</span></span> <span data-ttu-id="b0c0f-106">撰寫中介軟體時，可能需要這些作業的程式碼。</span><span class="sxs-lookup"><span data-stu-id="b0c0f-106">Code for these operations might be required when writing middleware.</span></span> <span data-ttu-id="b0c0f-107">在撰寫中介軟體之外，自訂程式碼通常不是必要的，因為作業是由 MVC 和 Razor Pages 處理。</span><span class="sxs-lookup"><span data-stu-id="b0c0f-107">Outside of writing middleware, custom code isn't generally required because the operations are handled by MVC and Razor Pages.</span></span>

<span data-ttu-id="b0c0f-108">要求和回應主體有兩個抽象概念： <xref:System.IO.Stream> 和 <xref:System.IO.Pipelines.Pipe>。</span><span class="sxs-lookup"><span data-stu-id="b0c0f-108">There are two abstractions for the request and response bodies: <xref:System.IO.Stream> and <xref:System.IO.Pipelines.Pipe>.</span></span> <span data-ttu-id="b0c0f-109">對於要求讀取，[HttpRequest.Body](xref:Microsoft.AspNetCore.Http.HttpRequest.Body) 是 <xref:System.IO.Stream>，而 `HttpRequest.BodyReader` 是 <xref:System.IO.Pipelines.PipeReader>。</span><span class="sxs-lookup"><span data-stu-id="b0c0f-109">For request reading, [HttpRequest.Body](xref:Microsoft.AspNetCore.Http.HttpRequest.Body) is a <xref:System.IO.Stream>, and `HttpRequest.BodyReader` is a <xref:System.IO.Pipelines.PipeReader>.</span></span> <span data-ttu-id="b0c0f-110">針對回應寫入， [HttpResponse](xref:Microsoft.AspNetCore.Http.HttpResponse.Body)是一種 <xref:System.IO.Stream>，`HttpResponse.BodyWriter` 是 <xref:System.IO.Pipelines.PipeWriter>。</span><span class="sxs-lookup"><span data-stu-id="b0c0f-110">For response writing, [HttpResponse.Body](xref:Microsoft.AspNetCore.Http.HttpResponse.Body) is a <xref:System.IO.Stream>, and `HttpResponse.BodyWriter` is a <xref:System.IO.Pipelines.PipeWriter>.</span></span>

<span data-ttu-id="b0c0f-111">建議在資料流程上使用[管線](/dotnet/standard/io/pipelines)。</span><span class="sxs-lookup"><span data-stu-id="b0c0f-111">[Pipelines](/dotnet/standard/io/pipelines) are recommended over streams.</span></span> <span data-ttu-id="b0c0f-112">資料流可以更容易地用於一些簡單的作業，但管線具有效能優勢，並且更容易在大部分情況下使用。</span><span class="sxs-lookup"><span data-stu-id="b0c0f-112">Streams can be easier to use for some simple operations, but pipelines have a performance advantage and are easier to use in most scenarios.</span></span> <span data-ttu-id="b0c0f-113">ASP.NET Core 開始使用管線，而不是內部的資料流程。</span><span class="sxs-lookup"><span data-stu-id="b0c0f-113">ASP.NET Core is starting to use pipelines instead of streams internally.</span></span> <span data-ttu-id="b0c0f-114">範例包括：</span><span class="sxs-lookup"><span data-stu-id="b0c0f-114">Examples include:</span></span>

* `FormReader`
* `TextReader`
* `TextWriter`
* `HttpResponse.WriteAsync`

<span data-ttu-id="b0c0f-115">未從架構中移除資料流程。</span><span class="sxs-lookup"><span data-stu-id="b0c0f-115">Streams aren't being removed from the framework.</span></span> <span data-ttu-id="b0c0f-116">資料流程會繼續在整個 .NET 中使用，而且許多資料流程類型沒有對等的管道，例如 `FileStreams` 和 `ResponseCompression`。</span><span class="sxs-lookup"><span data-stu-id="b0c0f-116">Streams continue to be used throughout .NET, and many stream types don't have pipe equivalents, such as `FileStreams` and `ResponseCompression`.</span></span>

## <a name="stream-examples"></a><span data-ttu-id="b0c0f-117">資料流範例</span><span class="sxs-lookup"><span data-stu-id="b0c0f-117">Stream examples</span></span>

<span data-ttu-id="b0c0f-118">假設目標是要建立一個中介軟體，將整個要求本文當做字串清單來讀取，並在新行上分割。</span><span class="sxs-lookup"><span data-stu-id="b0c0f-118">Suppose the goal is to create a middleware that reads the entire request body as a list of strings, splitting on new lines.</span></span> <span data-ttu-id="b0c0f-119">簡單的資料流實作看起來可能像下列範例這樣：</span><span class="sxs-lookup"><span data-stu-id="b0c0f-119">A simple stream implementation might look like the following example:</span></span>

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStream)]
[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

<span data-ttu-id="b0c0f-120">此程式碼確實有用，但有一些問題：</span><span class="sxs-lookup"><span data-stu-id="b0c0f-120">This code works, but there are some issues:</span></span>

* <span data-ttu-id="b0c0f-121">附加到 `StringBuilder` 之前，此範例會建立另一個會立即棄置的字串 (`encodedString`)。</span><span class="sxs-lookup"><span data-stu-id="b0c0f-121">Before appending to the `StringBuilder`, the example creates another string (`encodedString`) that is thrown away immediately.</span></span> <span data-ttu-id="b0c0f-122">系統會對資料流中的所有位元組執行此處理序，因此整個要求本文會額外耗用記憶體。</span><span class="sxs-lookup"><span data-stu-id="b0c0f-122">This process occurs for all bytes in the stream, so the result is extra memory allocation the size of the entire request body.</span></span>
* <span data-ttu-id="b0c0f-123">此範例會在於新行上進行分割之前讀取整個字串。</span><span class="sxs-lookup"><span data-stu-id="b0c0f-123">The example reads the entire string before splitting on new lines.</span></span> <span data-ttu-id="b0c0f-124">檢查位元組陣列中的新行會更有效率。</span><span class="sxs-lookup"><span data-stu-id="b0c0f-124">It's more efficient to check for new lines in the byte array.</span></span>

<span data-ttu-id="b0c0f-125">以下是修正上述幾個問題的範例：</span><span class="sxs-lookup"><span data-stu-id="b0c0f-125">Here's an example that fixes some of the preceding issues:</span></span>

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStreamMoreEfficient)]

<span data-ttu-id="b0c0f-126">前述範例：</span><span class="sxs-lookup"><span data-stu-id="b0c0f-126">This preceding example:</span></span>

* <span data-ttu-id="b0c0f-127">不會針對 `StringBuilder` 中的整個要求本文進行緩衝處理，除非沒有任何新行字元。</span><span class="sxs-lookup"><span data-stu-id="b0c0f-127">Doesn't buffer the entire request body in a `StringBuilder` unless there aren't any newline characters.</span></span>
* <span data-ttu-id="b0c0f-128">不會在字串上呼叫 `Split`。</span><span class="sxs-lookup"><span data-stu-id="b0c0f-128">Doesn't call `Split` on the string.</span></span>

<span data-ttu-id="b0c0f-129">但是仍有幾個問題：</span><span class="sxs-lookup"><span data-stu-id="b0c0f-129">However, there are still are a few issues:</span></span>

* <span data-ttu-id="b0c0f-130">如果換行字元是稀疏的，則會在字串中緩衝處理大部分的要求主體。</span><span class="sxs-lookup"><span data-stu-id="b0c0f-130">If newline characters are sparse, much of the request body is buffered in the string.</span></span>
* <span data-ttu-id="b0c0f-131">程式碼會繼續建立字串（`remainingString`），並將它們新增至字串緩衝區，這會導致額外的配置。</span><span class="sxs-lookup"><span data-stu-id="b0c0f-131">The code continues to create strings (`remainingString`) and adds them to the string buffer, which results in an extra allocation.</span></span>

<span data-ttu-id="b0c0f-132">這些問題都是可修復的，但程式碼變得越來越複雜，而且沒有任何改善。</span><span class="sxs-lookup"><span data-stu-id="b0c0f-132">These issues are fixable, but the code is becoming progressively more complicated with little improvement.</span></span> <span data-ttu-id="b0c0f-133">管線提供解決這些問題的方法，且具有最低的程式碼複雜度。</span><span class="sxs-lookup"><span data-stu-id="b0c0f-133">Pipelines provide a way to solve these problems with minimal code complexity.</span></span>

## <a name="pipelines"></a><span data-ttu-id="b0c0f-134">管線</span><span class="sxs-lookup"><span data-stu-id="b0c0f-134">Pipelines</span></span>

<span data-ttu-id="b0c0f-135">下列範例示範如何使用 `PipeReader` 處理相同的狀況：</span><span class="sxs-lookup"><span data-stu-id="b0c0f-135">The following example shows how the same scenario can be handled using a `PipeReader`:</span></span>

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringFromPipe)]

<span data-ttu-id="b0c0f-136">此範例可修正資料流實作的許多問題：</span><span class="sxs-lookup"><span data-stu-id="b0c0f-136">This example fixes many issues that the streams implementations had:</span></span>

* <span data-ttu-id="b0c0f-137">不需要字串緩衝區，因為 `PipeReader` 會處理尚未使用的位元組。</span><span class="sxs-lookup"><span data-stu-id="b0c0f-137">There's no need for a string buffer because the `PipeReader` handles bytes that haven't been used.</span></span>
* <span data-ttu-id="b0c0f-138">編碼的字串會直接新增至所傳回字串的清單。</span><span class="sxs-lookup"><span data-stu-id="b0c0f-138">Encoded strings are directly added to the list of returned strings.</span></span>
* <span data-ttu-id="b0c0f-139">除了字串 (除了 `ToArray()` 呼叫之外) 所使用的記憶體，系統不會配置資源給字串建立作業。</span><span class="sxs-lookup"><span data-stu-id="b0c0f-139">String creation is allocation-free besides the memory used by the string (except the `ToArray()` call).</span></span>

## <a name="adapters"></a><span data-ttu-id="b0c0f-140">配接器</span><span class="sxs-lookup"><span data-stu-id="b0c0f-140">Adapters</span></span>

<span data-ttu-id="b0c0f-141">`Body` 和 `BodyReader/BodyWriter` 屬性都適用于 `HttpRequest` 和 `HttpResponse`。</span><span class="sxs-lookup"><span data-stu-id="b0c0f-141">Both `Body` and `BodyReader/BodyWriter` properties are available for `HttpRequest` and `HttpResponse`.</span></span> <span data-ttu-id="b0c0f-142">當您將 `Body` 設定為不同的資料流程時，一組新的介面卡會自動將每個類型調整成另一種。</span><span class="sxs-lookup"><span data-stu-id="b0c0f-142">When you set `Body` to a different stream, a new set of adapters automatically adapt each type to the other.</span></span> <span data-ttu-id="b0c0f-143">如果您將 `HttpRequest.Body` 設定為新的資料流程，`HttpRequest.BodyReader` 會自動設定為包裝 `HttpRequest.Body`的新 `PipeReader`。</span><span class="sxs-lookup"><span data-stu-id="b0c0f-143">If you set `HttpRequest.Body` to a new stream, `HttpRequest.BodyReader` is automatically set to a new `PipeReader` that wraps `HttpRequest.Body`.</span></span>

## <a name="startasync"></a><span data-ttu-id="b0c0f-144">StartAsync</span><span class="sxs-lookup"><span data-stu-id="b0c0f-144">StartAsync</span></span>

<span data-ttu-id="b0c0f-145">`HttpResponse.StartAsync` 是用來指出標頭是不可修改的，而且會執行 `OnStarting` 回呼。</span><span class="sxs-lookup"><span data-stu-id="b0c0f-145">`HttpResponse.StartAsync` is used to indicate that headers are unmodifiable and to run `OnStarting` callbacks.</span></span> <span data-ttu-id="b0c0f-146">使用 Kestrel 做為伺服器時，在使用 `PipeReader` 之前呼叫 `StartAsync`，保證 `GetMemory` 所傳回的記憶體屬於 Kestrel 的內部 <xref:System.IO.Pipelines.Pipe>，而不是外部緩衝區。</span><span class="sxs-lookup"><span data-stu-id="b0c0f-146">When using Kestrel as a server, calling `StartAsync` before using the `PipeReader` guarantees that memory returned by `GetMemory` belongs to Kestrel's internal <xref:System.IO.Pipelines.Pipe> rather than an external buffer.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b0c0f-147">其他資源</span><span class="sxs-lookup"><span data-stu-id="b0c0f-147">Additional resources</span></span>

* <span data-ttu-id="b0c0f-148">[System.IO.Pipelines 簡介](https://devblogs.microsoft.com/dotnet/system-io-pipelines-high-performance-io-in-net/) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="b0c0f-148">[Introducing System.IO.Pipelines](https://devblogs.microsoft.com/dotnet/system-io-pipelines-high-performance-io-in-net/)</span></span>
* <xref:fundamentals/middleware/write>
