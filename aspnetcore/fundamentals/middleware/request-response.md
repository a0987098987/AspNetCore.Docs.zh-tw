---
title: ASP.NET Core 中的要求和回應作業
author: jkotalik
description: 了解如何在 ASP.NET Core 中讀取要求本文及寫入回應本文。
monikerRange: '>= aspnetcore-3.0'
ms.author: jkotalik
ms.custom: mvc
ms.date: 02/26/2019
uid: fundamentals/middleware/request-response
ms.openlocfilehash: f29c0aff95887d639cf3d1a8c8fe9f9228159f7a
ms.sourcegitcommit: 6ddd8a7675c1c1d997c8ab2d4498538e44954cac
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/05/2019
ms.locfileid: "57400723"
---
# <a name="request-and-response-operations-in-aspnet-core"></a><span data-ttu-id="d9841-103">ASP.NET Core 中的要求和回應作業</span><span class="sxs-lookup"><span data-stu-id="d9841-103">Request and response operations in ASP.NET Core</span></span>

<span data-ttu-id="d9841-104">作者：[Justin Kotalik](https://github.com/jkotalik)</span><span class="sxs-lookup"><span data-stu-id="d9841-104">By [Justin Kotalik](https://github.com/jkotalik)</span></span>

<span data-ttu-id="d9841-105">此文章說明如何讀取要求本文及寫入回應本文。</span><span class="sxs-lookup"><span data-stu-id="d9841-105">This article explains how to read from the request body and write to the response body.</span></span> <span data-ttu-id="d9841-106">當您撰寫中介軟體時，可能需要撰寫這些作業的程式碼。</span><span class="sxs-lookup"><span data-stu-id="d9841-106">You might need to write code for these operations when you're writing middleware.</span></span> <span data-ttu-id="d9841-107">否則，通常不需要撰寫此程式碼，因為作業會由 MVC 和 Razor Pages 處理。</span><span class="sxs-lookup"><span data-stu-id="d9841-107">Otherwise, you typically don't have to write this code because the operations are handled by MVC and Razor Pages.</span></span>

<span data-ttu-id="d9841-108">在 ASP.NET Core 3.0 中，有要求和回應主體的兩個抽象概念：<xref:System.IO.Stream> 和 <xref:System.IO.Pipelines.Pipe>。</span><span class="sxs-lookup"><span data-stu-id="d9841-108">In ASP.NET Core 3.0, there are two abstractions for the request and response bodies: <xref:System.IO.Stream> and <xref:System.IO.Pipelines.Pipe>.</span></span> <span data-ttu-id="d9841-109">對於要求讀取，[HttpRequest.Body](xref:Microsoft.AspNetCore.Http.HttpRequest.Body) 是 <xref:System.IO.Stream>，而 `HttpRequest.BodyPipe` 是 <xref:System.IO.Pipelines.PipeReader>。</span><span class="sxs-lookup"><span data-stu-id="d9841-109">For request reading, [HttpRequest.Body](xref:Microsoft.AspNetCore.Http.HttpRequest.Body) is a <xref:System.IO.Stream>, and `HttpRequest.BodyPipe` is a <xref:System.IO.Pipelines.PipeReader>.</span></span> <span data-ttu-id="d9841-110">對於要求讀取，[HttpResponse.Body](xref:Microsoft.AspNetCore.Http.HttpResponse.Body) 是 `HttpResponse.BodyPipe` 是 <xref:System.IO.Pipelines.PipeWriter>。</span><span class="sxs-lookup"><span data-stu-id="d9841-110">For response writing, [HttpResponse.Body](xref:Microsoft.AspNetCore.Http.HttpResponse.Body) is a `HttpResponse.BodyPipe` is a <xref:System.IO.Pipelines.PipeWriter>.</span></span>

<span data-ttu-id="d9841-111">我們建議在資料流上使用管線。</span><span class="sxs-lookup"><span data-stu-id="d9841-111">We recommend pipelines over streams.</span></span> <span data-ttu-id="d9841-112">資料流可以更容易地用於一些簡單的作業，但管線具有效能優勢，並且更容易在大部分情況下使用。</span><span class="sxs-lookup"><span data-stu-id="d9841-112">Streams can be easier to use for some simple operations, but pipelines have a performance advantage and are easier to use in most scenarios.</span></span> <span data-ttu-id="d9841-113">在 3.0 中，ASP.NET Core 正在啟動，以在內部使用管線，而非使用資料流。</span><span class="sxs-lookup"><span data-stu-id="d9841-113">In 3.0, ASP.NET Core is starting to use pipelines instead of streams internally.</span></span> <span data-ttu-id="d9841-114">範例包括：</span><span class="sxs-lookup"><span data-stu-id="d9841-114">Examples include:</span></span>

- `FormReader`
- `TextReader`
- `TexWriter`
- `HttpResponse.WriteAsync`

<span data-ttu-id="d9841-115">資料流並不會消失。</span><span class="sxs-lookup"><span data-stu-id="d9841-115">Streams aren't going away.</span></span> <span data-ttu-id="d9841-116">它們會繼續在整個 .NET 中使用，且許多資料流類型沒有管道對等項目，例如 `FileStreams` 和 `ResponseCompression`。</span><span class="sxs-lookup"><span data-stu-id="d9841-116">They continue to be used throughout .NET, and many stream types don't have pipe equivalents, like `FileStreams` and `ResponseCompression`.</span></span>

## <a name="stream-examples"></a><span data-ttu-id="d9841-117">資料流範例</span><span class="sxs-lookup"><span data-stu-id="d9841-117">Stream examples</span></span>

<span data-ttu-id="d9841-118">假設您想要建立一個以字串清單 (以新行區隔) 方式，讀取整個要求本文的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="d9841-118">Suppose you want to create a middleware that reads the entire request body as a list of strings, splitting on new lines.</span></span> <span data-ttu-id="d9841-119">簡單的資料流實作看起來可能像下列範例這樣：</span><span class="sxs-lookup"><span data-stu-id="d9841-119">A simple stream implementation might look like the following example:</span></span>

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStream)]

<span data-ttu-id="d9841-120">此程式碼確實有用，但有一些問題：</span><span class="sxs-lookup"><span data-stu-id="d9841-120">This code works, but there are some issues:</span></span>

- <span data-ttu-id="d9841-121">附加到 `StringBuilder` 之前，此範例會建立另一個會立即棄置的字串 (`encodedString`)。</span><span class="sxs-lookup"><span data-stu-id="d9841-121">Before appending to the `StringBuilder`, the example creates another string (`encodedString`) that is thrown away immediately.</span></span> <span data-ttu-id="d9841-122">系統會對資料流中的所有位元組執行此處理序，因此整個要求本文會額外耗用記憶體。</span><span class="sxs-lookup"><span data-stu-id="d9841-122">This process occurs for all bytes in the stream, so the result is extra memory allocation the size of the entire request body.</span></span>
- <span data-ttu-id="d9841-123">此範例會在於新行上進行分割之前讀取整個字串。</span><span class="sxs-lookup"><span data-stu-id="d9841-123">The example reads the entire string before splitting on new lines.</span></span> <span data-ttu-id="d9841-124">檢查位元組陣列中的新行會是比較有效率的方式。</span><span class="sxs-lookup"><span data-stu-id="d9841-124">It would be more efficient to check for new lines in the byte array.</span></span>

<span data-ttu-id="d9841-125">以下是這些問題其中一部分的修正範例：</span><span class="sxs-lookup"><span data-stu-id="d9841-125">Here's an example that fixes some of these issues:</span></span>

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStreamMoreEfficient)]

<span data-ttu-id="d9841-126">此範例：</span><span class="sxs-lookup"><span data-stu-id="d9841-126">This example:</span></span>

- <span data-ttu-id="d9841-127">不會針對 `StringBuilder` 中的整個要求本文進行緩衝處理，除非沒有任何新行字元。</span><span class="sxs-lookup"><span data-stu-id="d9841-127">Doesn't buffer the entire request body in a `StringBuilder` unless there aren't any newline characters.</span></span>
- <span data-ttu-id="d9841-128">不會在字串上呼叫 `Split`。</span><span class="sxs-lookup"><span data-stu-id="d9841-128">Doesn't call `Split` on the string.</span></span>

<span data-ttu-id="d9841-129">但是仍有幾個問題：</span><span class="sxs-lookup"><span data-stu-id="d9841-129">However, there are still are a few issues:</span></span>

- <span data-ttu-id="d9841-130">如果新行字元是疏鬆的，大部分要求本文都會在字串中進行緩衝處理。</span><span class="sxs-lookup"><span data-stu-id="d9841-130">If newline characters are sparse, much of the request body is buffered in the string .</span></span>
- <span data-ttu-id="d9841-131">它仍然會建立字串 (`remainingString`) 並將它們新增至字串緩衝區，這會額外耗用資源。</span><span class="sxs-lookup"><span data-stu-id="d9841-131">It still creates strings (`remainingString`) and adds them to the string buffer, which results in an extra allocation.</span></span>

<span data-ttu-id="d9841-132">這些問題是可以修正的，但程式碼變得越來越複雜，而且改善幅度很有限。</span><span class="sxs-lookup"><span data-stu-id="d9841-132">These issues are fixable, but the code is becoming more and more complicated with little improvement.</span></span> <span data-ttu-id="d9841-133">管線提供解決這些問題的方法，且具有最低的程式碼複雜度。</span><span class="sxs-lookup"><span data-stu-id="d9841-133">Pipelines provide a way to solve these problems with minimal code complexity.</span></span>

## <a name="pipelines"></a><span data-ttu-id="d9841-134">管線</span><span class="sxs-lookup"><span data-stu-id="d9841-134">Pipelines</span></span>

<span data-ttu-id="d9841-135">下列範例示範如何使用 `PipeReader` 處理相同的狀況：</span><span class="sxs-lookup"><span data-stu-id="d9841-135">The following example shows how the same scenario can be handled using a `PipeReader`:</span></span>

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringFromPipe)]

<span data-ttu-id="d9841-136">此範例可修正資料流實作的許多問題：</span><span class="sxs-lookup"><span data-stu-id="d9841-136">This example fixes many issues that the streams implementations had:</span></span>

- <span data-ttu-id="d9841-137">這並不需要字串緩衝區，因為 `PipeReader` 會處理未使用的位元組。</span><span class="sxs-lookup"><span data-stu-id="d9841-137">There is no need for a string buffer because the `PipeReader` handles bytes that haven't been used.</span></span>
- <span data-ttu-id="d9841-138">編碼的字串會直接新增至所傳回字串的清單。</span><span class="sxs-lookup"><span data-stu-id="d9841-138">Encoded strings are directly added to the list of returned strings.</span></span>
- <span data-ttu-id="d9841-139">除了字串 (除了 `ToArray()` 呼叫之外) 所使用的記憶體，系統不會配置資源給字串建立作業。</span><span class="sxs-lookup"><span data-stu-id="d9841-139">String creation is allocation-free besides the memory used by the string (except the `ToArray()` call).</span></span>

## <a name="adapters"></a><span data-ttu-id="d9841-140">配接器</span><span class="sxs-lookup"><span data-stu-id="d9841-140">Adapters</span></span>

<span data-ttu-id="d9841-141">現在 `Body` 和 `BodyPipe` 屬性都可供 `HttpRequest` 和 `HttpResponse` 使用，那麼將 `Body` 設至其他資料流時，會發生什麼情況？</span><span class="sxs-lookup"><span data-stu-id="d9841-141">Now that both `Body` and `BodyPipe` properties are available for `HttpRequest` and `HttpResponse`, what happens when you set `Body` to a different stream?</span></span> <span data-ttu-id="d9841-142">在 3.0 中有一組新的配接器可自動將每種類型與其他類型配接。</span><span class="sxs-lookup"><span data-stu-id="d9841-142">In 3.0, a new set of adapters automatically adapt each type to the other.</span></span> <span data-ttu-id="d9841-143">例如，如果將 `HttpRequest.Body` 設定至新的資料流，`HttpRequest.BodyPipe` 會自動設為包裝 `HttpRequest.Body` 的新 `PipeReader`。</span><span class="sxs-lookup"><span data-stu-id="d9841-143">For example, if you set `HttpRequest.Body` to a new stream, `HttpRequest.BodyPipe` is automatically set to a new `PipeReader` that wraps `HttpRequest.Body`.</span></span> <span data-ttu-id="d9841-144">相同的行為會套用至設定 `BodyPipe` 屬性。</span><span class="sxs-lookup"><span data-stu-id="d9841-144">The same behavior applies to setting the `BodyPipe` property.</span></span> <span data-ttu-id="d9841-145">如果將 `HttpResponse.BodyPipe` 設定至新的 `PipeWriter`，`HttpResponse.Body` 會自動設為包裝 `HttpResponse.BodyPipe` 的新資料流。</span><span class="sxs-lookup"><span data-stu-id="d9841-145">If `HttpResponse.BodyPipe` is set to a new `PipeWriter`, the `HttpResponse.Body` is automatically set to a new stream that wraps `HttpResponse.BodyPipe`.</span></span>

## <a name="startasync"></a><span data-ttu-id="d9841-146">StartAsync</span><span class="sxs-lookup"><span data-stu-id="d9841-146">StartAsync</span></span>

<span data-ttu-id="d9841-147">3.0 中 `HttpResponse.StartAsync` 是新的。</span><span class="sxs-lookup"><span data-stu-id="d9841-147">`HttpResponse.StartAsync` is new in 3.0.</span></span> <span data-ttu-id="d9841-148">它可以用來指示標題是不可修改的，以及執行 `OnStarting` 回撥。</span><span class="sxs-lookup"><span data-stu-id="d9841-148">It is used to indicate that headers are unmodifiable and to run `OnStarting` callbacks.</span></span> <span data-ttu-id="d9841-149">在 3.0-preview3 中，您必須先呼叫 `StartAsync` 再使用 `HttpRequest.BodyPipe`，而且在未來版本中，它將會是一個建議。</span><span class="sxs-lookup"><span data-stu-id="d9841-149">In 3.0-preview3, you have to call `StartAsync` before using `HttpRequest.BodyPipe`, and in future releases, it will be a recommendation.</span></span> <span data-ttu-id="d9841-150">使用 Kestrel 作為伺服器時，請先呼叫 StartAsync 再使用 `PipeReader`，以保證 `GetMemory` 傳回的記憶體將屬於 Kestrel 的內部 <xref:System.IO.Pipelines.Pipe>，而不是外部緩衝區。</span><span class="sxs-lookup"><span data-stu-id="d9841-150">When using Kestrel as a server, calling StartAsync before using the `PipeReader` guarantees that memory returned by `GetMemory` will belong to Kestrel's internal <xref:System.IO.Pipelines.Pipe> rather than an external buffer.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d9841-151">其他資源</span><span class="sxs-lookup"><span data-stu-id="d9841-151">Additional resources</span></span>

* <span data-ttu-id="d9841-152">[System.IO.Pipelines 簡介](https://devblogs.microsoft.com/dotnet/system-io-pipelines-high-performance-io-in-net/) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="d9841-152">[Introducing System.IO.Pipelines](https://devblogs.microsoft.com/dotnet/system-io-pipelines-high-performance-io-in-net/)</span></span>
* <xref:fundamentals/middleware/write>