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
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78667213"
---
# <a name="request-and-response-operations-in-aspnet-core"></a><span data-ttu-id="1963c-103">ASP.NET Core 中的要求和回應作業</span><span class="sxs-lookup"><span data-stu-id="1963c-103">Request and response operations in ASP.NET Core</span></span>

<span data-ttu-id="1963c-104">作者：[Justin Kotalik](https://github.com/jkotalik)</span><span class="sxs-lookup"><span data-stu-id="1963c-104">By [Justin Kotalik](https://github.com/jkotalik)</span></span>

<span data-ttu-id="1963c-105">此文章說明如何讀取要求本文及寫入回應本文。</span><span class="sxs-lookup"><span data-stu-id="1963c-105">This article explains how to read from the request body and write to the response body.</span></span> <span data-ttu-id="1963c-106">編寫中間件時可能需要這些操作的代碼。</span><span class="sxs-lookup"><span data-stu-id="1963c-106">Code for these operations might be required when writing middleware.</span></span> <span data-ttu-id="1963c-107">除了編寫中間件之外,通常不需要自定義代碼,因為操作由 MVC 和 Razor Pages 處理。</span><span class="sxs-lookup"><span data-stu-id="1963c-107">Outside of writing middleware, custom code isn't generally required because the operations are handled by MVC and Razor Pages.</span></span>

<span data-ttu-id="1963c-108">要求與回應正文有兩個抽象:<xref:System.IO.Stream><xref:System.IO.Pipelines.Pipe>與 。</span><span class="sxs-lookup"><span data-stu-id="1963c-108">There are two abstractions for the request and response bodies: <xref:System.IO.Stream> and <xref:System.IO.Pipelines.Pipe>.</span></span> <span data-ttu-id="1963c-109">對於要求讀取，[HttpRequest.Body](xref:Microsoft.AspNetCore.Http.HttpRequest.Body) 是 <xref:System.IO.Stream>，而 `HttpRequest.BodyReader` 是 <xref:System.IO.Pipelines.PipeReader>。</span><span class="sxs-lookup"><span data-stu-id="1963c-109">For request reading, [HttpRequest.Body](xref:Microsoft.AspNetCore.Http.HttpRequest.Body) is a <xref:System.IO.Stream>, and `HttpRequest.BodyReader` is a <xref:System.IO.Pipelines.PipeReader>.</span></span> <span data-ttu-id="1963c-110">對於回應編寫[,HTTPResponse.Body](xref:Microsoft.AspNetCore.Http.HttpResponse.Body)`HttpResponse.BodyWriter`是<xref:System.IO.Pipelines.PipeWriter><xref:System.IO.Stream>, 是 。</span><span class="sxs-lookup"><span data-stu-id="1963c-110">For response writing, [HttpResponse.Body](xref:Microsoft.AspNetCore.Http.HttpResponse.Body) is a <xref:System.IO.Stream>, and `HttpResponse.BodyWriter` is a <xref:System.IO.Pipelines.PipeWriter>.</span></span>

<span data-ttu-id="1963c-111">建議透過串流[進行導管](/dotnet/standard/io/pipelines)。</span><span class="sxs-lookup"><span data-stu-id="1963c-111">[Pipelines](/dotnet/standard/io/pipelines) are recommended over streams.</span></span> <span data-ttu-id="1963c-112">資料流可以更容易地用於一些簡單的作業，但管線具有效能優勢，並且更容易在大部分情況下使用。</span><span class="sxs-lookup"><span data-stu-id="1963c-112">Streams can be easier to use for some simple operations, but pipelines have a performance advantage and are easier to use in most scenarios.</span></span> <span data-ttu-id="1963c-113">ASP.NET核心開始使用管道而不是內部流。</span><span class="sxs-lookup"><span data-stu-id="1963c-113">ASP.NET Core is starting to use pipelines instead of streams internally.</span></span> <span data-ttu-id="1963c-114">範例包括：</span><span class="sxs-lookup"><span data-stu-id="1963c-114">Examples include:</span></span>

* `FormReader`
* `TextReader`
* `TextWriter`
* `HttpResponse.WriteAsync`

<span data-ttu-id="1963c-115">不會從框架中刪除流。</span><span class="sxs-lookup"><span data-stu-id="1963c-115">Streams aren't being removed from the framework.</span></span> <span data-ttu-id="1963c-116">串流繼續在整個 .NET 中使用,許多流類型沒有管道等效`FileStreams`項`ResponseCompression`,如與 。</span><span class="sxs-lookup"><span data-stu-id="1963c-116">Streams continue to be used throughout .NET, and many stream types don't have pipe equivalents, such as `FileStreams` and `ResponseCompression`.</span></span>

## <a name="stream-examples"></a><span data-ttu-id="1963c-117">資料流範例</span><span class="sxs-lookup"><span data-stu-id="1963c-117">Stream examples</span></span>

<span data-ttu-id="1963c-118">假設目標是創建一個中間件,將整個請求正文讀取為字串清單,在新行上拆分。</span><span class="sxs-lookup"><span data-stu-id="1963c-118">Suppose the goal is to create a middleware that reads the entire request body as a list of strings, splitting on new lines.</span></span> <span data-ttu-id="1963c-119">簡單的資料流實作看起來可能像下列範例這樣：</span><span class="sxs-lookup"><span data-stu-id="1963c-119">A simple stream implementation might look like the following example:</span></span>

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStream)]
[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

<span data-ttu-id="1963c-120">此程式碼確實有用，但有一些問題：</span><span class="sxs-lookup"><span data-stu-id="1963c-120">This code works, but there are some issues:</span></span>

* <span data-ttu-id="1963c-121">附加到 `StringBuilder` 之前，此範例會建立另一個會立即棄置的字串 (`encodedString`)。</span><span class="sxs-lookup"><span data-stu-id="1963c-121">Before appending to the `StringBuilder`, the example creates another string (`encodedString`) that is thrown away immediately.</span></span> <span data-ttu-id="1963c-122">系統會對資料流中的所有位元組執行此處理序，因此整個要求本文會額外耗用記憶體。</span><span class="sxs-lookup"><span data-stu-id="1963c-122">This process occurs for all bytes in the stream, so the result is extra memory allocation the size of the entire request body.</span></span>
* <span data-ttu-id="1963c-123">此範例會在於新行上進行分割之前讀取整個字串。</span><span class="sxs-lookup"><span data-stu-id="1963c-123">The example reads the entire string before splitting on new lines.</span></span> <span data-ttu-id="1963c-124">在位元組中檢查新行更高效。</span><span class="sxs-lookup"><span data-stu-id="1963c-124">It's more efficient to check for new lines in the byte array.</span></span>

<span data-ttu-id="1963c-125">下面是一個修復上述問題的範例:</span><span class="sxs-lookup"><span data-stu-id="1963c-125">Here's an example that fixes some of the preceding issues:</span></span>

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStreamMoreEfficient)]

<span data-ttu-id="1963c-126">前面的範例:</span><span class="sxs-lookup"><span data-stu-id="1963c-126">This preceding example:</span></span>

* <span data-ttu-id="1963c-127">不會針對 `StringBuilder` 中的整個要求本文進行緩衝處理，除非沒有任何新行字元。</span><span class="sxs-lookup"><span data-stu-id="1963c-127">Doesn't buffer the entire request body in a `StringBuilder` unless there aren't any newline characters.</span></span>
* <span data-ttu-id="1963c-128">不會在字串上呼叫 `Split`。</span><span class="sxs-lookup"><span data-stu-id="1963c-128">Doesn't call `Split` on the string.</span></span>

<span data-ttu-id="1963c-129">但是仍有幾個問題：</span><span class="sxs-lookup"><span data-stu-id="1963c-129">However, there are still are a few issues:</span></span>

* <span data-ttu-id="1963c-130">如果換行字元是稀疏的,則大部分請求正文將緩衝在字串中。</span><span class="sxs-lookup"><span data-stu-id="1963c-130">If newline characters are sparse, much of the request body is buffered in the string.</span></span>
* <span data-ttu-id="1963c-131">代碼繼續建立字串 (`remainingString`, 並將它們添加到字串緩衝區, 這將導致額外的分配。</span><span class="sxs-lookup"><span data-stu-id="1963c-131">The code continues to create strings (`remainingString`) and adds them to the string buffer, which results in an extra allocation.</span></span>

<span data-ttu-id="1963c-132">這些問題是可以解決的,但代碼正變得越來越複雜,幾乎沒有改進。</span><span class="sxs-lookup"><span data-stu-id="1963c-132">These issues are fixable, but the code is becoming progressively more complicated with little improvement.</span></span> <span data-ttu-id="1963c-133">管線提供解決這些問題的方法，且具有最低的程式碼複雜度。</span><span class="sxs-lookup"><span data-stu-id="1963c-133">Pipelines provide a way to solve these problems with minimal code complexity.</span></span>

## <a name="pipelines"></a><span data-ttu-id="1963c-134">管線</span><span class="sxs-lookup"><span data-stu-id="1963c-134">Pipelines</span></span>

<span data-ttu-id="1963c-135">下列範例示範如何使用 `PipeReader` 處理相同的狀況：</span><span class="sxs-lookup"><span data-stu-id="1963c-135">The following example shows how the same scenario can be handled using a `PipeReader`:</span></span>

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringFromPipe)]

<span data-ttu-id="1963c-136">此範例可修正資料流實作的許多問題：</span><span class="sxs-lookup"><span data-stu-id="1963c-136">This example fixes many issues that the streams implementations had:</span></span>

* <span data-ttu-id="1963c-137">不需要字串緩衝區,`PipeReader`因為句柄位元組尚未使用。</span><span class="sxs-lookup"><span data-stu-id="1963c-137">There's no need for a string buffer because the `PipeReader` handles bytes that haven't been used.</span></span>
* <span data-ttu-id="1963c-138">編碼的字串會直接新增至所傳回字串的清單。</span><span class="sxs-lookup"><span data-stu-id="1963c-138">Encoded strings are directly added to the list of returned strings.</span></span>
* <span data-ttu-id="1963c-139">除了字串 (除了 `ToArray()` 呼叫之外) 所使用的記憶體，系統不會配置資源給字串建立作業。</span><span class="sxs-lookup"><span data-stu-id="1963c-139">String creation is allocation-free besides the memory used by the string (except the `ToArray()` call).</span></span>

## <a name="adapters"></a><span data-ttu-id="1963c-140">配接器</span><span class="sxs-lookup"><span data-stu-id="1963c-140">Adapters</span></span>

<span data-ttu-id="1963c-141">屬性`Body``BodyReader/BodyWriter`都可用於`HttpRequest`與`HttpResponse`。</span><span class="sxs-lookup"><span data-stu-id="1963c-141">Both `Body` and `BodyReader/BodyWriter` properties are available for `HttpRequest` and `HttpResponse`.</span></span> <span data-ttu-id="1963c-142">當您設置為`Body`其他流時,一組新的適配器會自動將每種類型調整到另一種。</span><span class="sxs-lookup"><span data-stu-id="1963c-142">When you set `Body` to a different stream, a new set of adapters automatically adapt each type to the other.</span></span> <span data-ttu-id="1963c-143">如果設置為`HttpRequest.Body`新流`HttpRequest.BodyReader`, 則會自動設置為換`PipeReader``HttpRequest.Body`行的新流。</span><span class="sxs-lookup"><span data-stu-id="1963c-143">If you set `HttpRequest.Body` to a new stream, `HttpRequest.BodyReader` is automatically set to a new `PipeReader` that wraps `HttpRequest.Body`.</span></span>

## <a name="startasync"></a><span data-ttu-id="1963c-144">StartAsync</span><span class="sxs-lookup"><span data-stu-id="1963c-144">StartAsync</span></span>

<span data-ttu-id="1963c-145">`HttpResponse.StartAsync`用於指示標頭不可修改並運行`OnStarting`回調。</span><span class="sxs-lookup"><span data-stu-id="1963c-145">`HttpResponse.StartAsync` is used to indicate that headers are unmodifiable and to run `OnStarting` callbacks.</span></span> <span data-ttu-id="1963c-146">使用 Kestrel 作為伺服器`StartAsync`時,`PipeReader``GetMemory`在使用傳回 的記憶體的保證之前調用屬於 Kestrel 的內部<xref:System.IO.Pipelines.Pipe>緩衝區,而不是外部緩衝區。</span><span class="sxs-lookup"><span data-stu-id="1963c-146">When using Kestrel as a server, calling `StartAsync` before using the `PipeReader` guarantees that memory returned by `GetMemory` belongs to Kestrel's internal <xref:System.IO.Pipelines.Pipe> rather than an external buffer.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1963c-147">其他資源</span><span class="sxs-lookup"><span data-stu-id="1963c-147">Additional resources</span></span>

* <span data-ttu-id="1963c-148">[System.IO.Pipelines 簡介](https://devblogs.microsoft.com/dotnet/system-io-pipelines-high-performance-io-in-net/) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="1963c-148">[Introducing System.IO.Pipelines](https://devblogs.microsoft.com/dotnet/system-io-pipelines-high-performance-io-in-net/)</span></span>
* <xref:fundamentals/middleware/write>
