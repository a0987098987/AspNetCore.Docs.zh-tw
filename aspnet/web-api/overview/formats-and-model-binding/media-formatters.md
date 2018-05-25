---
uid: web-api/overview/formats-and-model-binding/media-formatters
title: ASP.NET Web API 2 中的媒體格式器 |Microsoft 文件
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: 4c56f64a-086a-44ce-99c2-4c69604cd7fd
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/media-formatters
msc.type: authoredcontent
ms.openlocfilehash: 1cb1c7e0f832a0a0160276fbd41facc017e2ae3e
ms.sourcegitcommit: 50d40c83fa641d283c097f986dde5341ebe1b44c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/22/2018
---
<a name="media-formatters-in-aspnet-web-api-2"></a><span data-ttu-id="bf7fe-102">ASP.NET Web API 2 中的媒體格式器</span><span class="sxs-lookup"><span data-stu-id="bf7fe-102">Media Formatters in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="bf7fe-103">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="bf7fe-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="bf7fe-104">本教學課程會示範如何在 ASP.NET Web API 中支援的其他媒體格式。</span><span class="sxs-lookup"><span data-stu-id="bf7fe-104">This tutorial shows how to support additional media formats in ASP.NET Web API.</span></span>

## <a name="internet-media-types"></a><span data-ttu-id="bf7fe-105">網際網路媒體類型</span><span class="sxs-lookup"><span data-stu-id="bf7fe-105">Internet Media Types</span></span>

<span data-ttu-id="bf7fe-106">媒體類型，也稱為 MIME 類型，識別一段資料的格式。</span><span class="sxs-lookup"><span data-stu-id="bf7fe-106">A media type, also called a MIME type, identifies the format of a piece of data.</span></span> <span data-ttu-id="bf7fe-107">在 HTTP、 媒體類型會描述訊息本文的格式。</span><span class="sxs-lookup"><span data-stu-id="bf7fe-107">In HTTP, media types describe the format of the message body.</span></span> <span data-ttu-id="bf7fe-108">媒體類型是由兩個字串、 類型和子類型所組成。</span><span class="sxs-lookup"><span data-stu-id="bf7fe-108">A media type consists of two strings, a type and a subtype.</span></span> <span data-ttu-id="bf7fe-109">例如: </span><span class="sxs-lookup"><span data-stu-id="bf7fe-109">For example:</span></span>

- <span data-ttu-id="bf7fe-110">text/html</span><span class="sxs-lookup"><span data-stu-id="bf7fe-110">text/html</span></span>
- <span data-ttu-id="bf7fe-111">image/png</span><span class="sxs-lookup"><span data-stu-id="bf7fe-111">image/png</span></span>
- <span data-ttu-id="bf7fe-112">application/json</span><span class="sxs-lookup"><span data-stu-id="bf7fe-112">application/json</span></span>

<span data-ttu-id="bf7fe-113">當 HTTP 訊息包含實體內容時，內容類型標頭會指定訊息主體的格式。</span><span class="sxs-lookup"><span data-stu-id="bf7fe-113">When an HTTP message contains an entity-body, the Content-Type header specifies the format of the message body.</span></span> <span data-ttu-id="bf7fe-114">這會告知收件者如何剖析訊息本文的內容。</span><span class="sxs-lookup"><span data-stu-id="bf7fe-114">This tells the receiver how to parse the contents of the message body.</span></span>

<span data-ttu-id="bf7fe-115">例如，如果 HTTP 回應將包含為 PNG 影像，回應可能會有下列標頭。</span><span class="sxs-lookup"><span data-stu-id="bf7fe-115">For example, if an HTTP response contains a PNG image, the response might have the following headers.</span></span>

[!code-console[Main](media-formatters/samples/sample1.cmd)]

<span data-ttu-id="bf7fe-116">當用戶端傳送要求訊息時，它可以包含 Accept 標頭。</span><span class="sxs-lookup"><span data-stu-id="bf7fe-116">When the client sends a request message, it can include an Accept header.</span></span> <span data-ttu-id="bf7fe-117">Accept 標頭會告訴您的伺服器的媒體類型的用戶端想從伺服器。</span><span class="sxs-lookup"><span data-stu-id="bf7fe-117">The Accept header tells the server which media type(s) the client wants from the server.</span></span> <span data-ttu-id="bf7fe-118">例如: </span><span class="sxs-lookup"><span data-stu-id="bf7fe-118">For example:</span></span>

[!code-console[Main](media-formatters/samples/sample2.cmd)]

<span data-ttu-id="bf7fe-119">此標頭會要求伺服器在用戶端想 HTML、 XHTML 或 XML。</span><span class="sxs-lookup"><span data-stu-id="bf7fe-119">This header tells the server that the client wants either HTML, XHTML, or XML.</span></span>

<span data-ttu-id="bf7fe-120">媒體類型會決定如何序列化 Web API 和 HTTP 訊息本文還原序列化。</span><span class="sxs-lookup"><span data-stu-id="bf7fe-120">The media type determines how Web API serializes and deserializes the HTTP message body.</span></span> <span data-ttu-id="bf7fe-121">Web API XML、 JSON、 BSON 和表單 urlencoded 資料的內建支援，與您可以透過撰寫支援其他媒體類型*媒體格式器*。</span><span class="sxs-lookup"><span data-stu-id="bf7fe-121">Web API has built-in support for XML, JSON, BSON, and form-urlencoded data, and you can support additional media types by writing a *media formatter*.</span></span>

<span data-ttu-id="bf7fe-122">若要建立的媒體格式器，衍生自這些類別的其中一個：</span><span class="sxs-lookup"><span data-stu-id="bf7fe-122">To create a media formatter, derive from one of these classes:</span></span>

- <span data-ttu-id="bf7fe-123">[MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx)。</span><span class="sxs-lookup"><span data-stu-id="bf7fe-123">[MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx).</span></span> <span data-ttu-id="bf7fe-124">此類別會使用非同步讀取及寫入方法。</span><span class="sxs-lookup"><span data-stu-id="bf7fe-124">This class uses asynchronous read and write methods.</span></span>
- <span data-ttu-id="bf7fe-125">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx)。</span><span class="sxs-lookup"><span data-stu-id="bf7fe-125">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span></span> <span data-ttu-id="bf7fe-126">此類別衍生自**MediaTypeFormatter**但使用 sychronous 讀/寫方法。</span><span class="sxs-lookup"><span data-stu-id="bf7fe-126">This class derives from **MediaTypeFormatter** but uses sychronous read/write methods.</span></span>

<span data-ttu-id="bf7fe-127">衍生自**BufferedMediaTypeFormatter**較為簡單，因為沒有非同步程式碼，但這也表示 i/o 可能會封鎖呼叫執行緒。</span><span class="sxs-lookup"><span data-stu-id="bf7fe-127">Deriving from **BufferedMediaTypeFormatter** is simpler, because there is no asynchronous code, but it also means the calling thread can block during I/O.</span></span>

## <a name="example-creating-a-csv-media-formatter"></a><span data-ttu-id="bf7fe-128">範例： 建立 CSV 的媒體格式器</span><span class="sxs-lookup"><span data-stu-id="bf7fe-128">Example: Creating a CSV Media Formatter</span></span>

<span data-ttu-id="bf7fe-129">下列範例示範可以產品的物件序列化為逗號分隔值 (CSV) 格式的媒體類型格式器。</span><span class="sxs-lookup"><span data-stu-id="bf7fe-129">The following example shows a media type formatter that can serialize a Product object to a comma-separated values (CSV) format.</span></span> <span data-ttu-id="bf7fe-130">這個範例會使用在本教學課程中定義的產品類型[建立 Web API 的支援 CRUD 操作](../older-versions/creating-a-web-api-that-supports-crud-operations.md)。</span><span class="sxs-lookup"><span data-stu-id="bf7fe-130">This example uses the Product type defined in the tutorial [Creating a Web API that Supports CRUD Operations](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span></span> <span data-ttu-id="bf7fe-131">以下是產品物件的定義：</span><span class="sxs-lookup"><span data-stu-id="bf7fe-131">Here is the definition of the Product object:</span></span>

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

<span data-ttu-id="bf7fe-132">若要實作 CSV 格式器，請定義衍生自類別**BufferedMediaTypeFormater**:</span><span class="sxs-lookup"><span data-stu-id="bf7fe-132">To implement a CSV formatter, define a class that derives from **BufferedMediaTypeFormater**:</span></span>

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

<span data-ttu-id="bf7fe-133">在建構函式，新增的媒體類型格式器支援。</span><span class="sxs-lookup"><span data-stu-id="bf7fe-133">In the constructor, add the media types that the formatter supports.</span></span> <span data-ttu-id="bf7fe-134">在此範例中，格式器支援單一媒體類型， &quot;text/csv&quot;:</span><span class="sxs-lookup"><span data-stu-id="bf7fe-134">In this example, the formatter supports a single media type, &quot;text/csv&quot;:</span></span>

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

<span data-ttu-id="bf7fe-135">覆寫**CanWriteType**方法，以表示的類型格式器可以序列化：</span><span class="sxs-lookup"><span data-stu-id="bf7fe-135">Override the **CanWriteType** method to indicate which types the formatter can serialize:</span></span>

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

<span data-ttu-id="bf7fe-136">在此範例中，格式器可以序列化單一`Product`物件的集合以及`Product`物件。</span><span class="sxs-lookup"><span data-stu-id="bf7fe-136">In this example, the formatter can serialize single `Product` objects as well as collections of `Product` objects.</span></span>

<span data-ttu-id="bf7fe-137">同樣地，覆寫**CanReadType**方法，以表示的類型格式器可以還原序列化。</span><span class="sxs-lookup"><span data-stu-id="bf7fe-137">Similarly, override the **CanReadType** method to indicate which types the formatter can deserialize.</span></span> <span data-ttu-id="bf7fe-138">在此範例中，格式器不支援還原序列化，因此方法只會傳回**false**。</span><span class="sxs-lookup"><span data-stu-id="bf7fe-138">In this example, the formatter does not support deserialization, so the method simply returns **false**.</span></span>

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

<span data-ttu-id="bf7fe-139">最後，覆寫**WriteToStream**方法。</span><span class="sxs-lookup"><span data-stu-id="bf7fe-139">Finally, override the **WriteToStream** method.</span></span> <span data-ttu-id="bf7fe-140">這個方法會序列化型別的所寫入資料流。</span><span class="sxs-lookup"><span data-stu-id="bf7fe-140">This method serializes a type by writing it to a stream.</span></span> <span data-ttu-id="bf7fe-141">如果您的格式器支援還原序列化，也會覆寫**ReadFromStream**方法。</span><span class="sxs-lookup"><span data-stu-id="bf7fe-141">If your formatter supports deserialization, also override the **ReadFromStream** method.</span></span>

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a><span data-ttu-id="bf7fe-142">將媒體格式器加入至 Web API 管線</span><span class="sxs-lookup"><span data-stu-id="bf7fe-142">Adding a Media Formatter to the Web API Pipeline</span></span>

<span data-ttu-id="bf7fe-143">若要新增的媒體類型格式器至 Web API 管線，使用**格式器**屬性**HttpConfiguration**物件。</span><span class="sxs-lookup"><span data-stu-id="bf7fe-143">To add a media type formatter to the Web API pipeline, use the **Formatters** property on the **HttpConfiguration** object.</span></span>

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a><span data-ttu-id="bf7fe-144">字元編碼方式</span><span class="sxs-lookup"><span data-stu-id="bf7fe-144">Character Encodings</span></span>

<span data-ttu-id="bf7fe-145">（選擇性） 媒體格式器可以支援多個的字元編碼，例如 utf-8 或 ISO 8859-1。</span><span class="sxs-lookup"><span data-stu-id="bf7fe-145">Optionally, a media formatter can support multiple character encodings, such as UTF-8 or ISO 8859-1.</span></span>

<span data-ttu-id="bf7fe-146">在建構函式，加入一個或多個[System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx)類型**SupportedEncodings**集合。</span><span class="sxs-lookup"><span data-stu-id="bf7fe-146">In the constructor, add one or more [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) types to the **SupportedEncodings** collection.</span></span> <span data-ttu-id="bf7fe-147">將使用預設編碼方式的第一個。</span><span class="sxs-lookup"><span data-stu-id="bf7fe-147">Put the default encoding first.</span></span>

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

<span data-ttu-id="bf7fe-148">在**WriteToStream**和**ReadFromStream**方法呼叫[MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx)選取的慣用的字元編碼。</span><span class="sxs-lookup"><span data-stu-id="bf7fe-148">In the **WriteToStream** and **ReadFromStream** methods, call [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) to select the preferred character encoding.</span></span> <span data-ttu-id="bf7fe-149">這個方法比對支援的編碼方式清單進行比對的要求標頭。</span><span class="sxs-lookup"><span data-stu-id="bf7fe-149">This method matches the request headers against the list of supported encodings.</span></span> <span data-ttu-id="bf7fe-150">使用傳回**編碼**當讀取或寫入資料流：</span><span class="sxs-lookup"><span data-stu-id="bf7fe-150">Use the returned **Encoding** when you read or write from the stream:</span></span>

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
