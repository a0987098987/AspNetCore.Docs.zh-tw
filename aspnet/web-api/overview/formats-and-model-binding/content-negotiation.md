---
uid: web-api/overview/formats-and-model-binding/content-negotiation
title: 內容交涉，ASP.NET Web API 中的 |Microsoft Docs
author: MikeWasson
description: 說明 ASP.NET Web API 實作 HTTP 內容交涉的方式。
ms.author: riande
ms.date: 05/20/2012
ms.assetid: 0dd51b30-bf5a-419f-a1b7-2817ccca3c7d
msc.legacyurl: /web-api/overview/formats-and-model-binding/content-negotiation
msc.type: authoredcontent
ms.openlocfilehash: e936bdfa52f786ec86d3e84eac3cd644225b6f92
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835028"
---
<a name="content-negotiation-in-aspnet-web-api"></a><span data-ttu-id="fcf4b-103">ASP.NET Web API 中的內容交涉</span><span class="sxs-lookup"><span data-stu-id="fcf4b-103">Content Negotiation in ASP.NET Web API</span></span>
====================
<span data-ttu-id="fcf4b-104">藉由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="fcf4b-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="fcf4b-105">這篇文章說明 ASP.NET Web API 實作內容交涉的方式。</span><span class="sxs-lookup"><span data-stu-id="fcf4b-105">This article describes how ASP.NET Web API implements content negotiation.</span></span>

<span data-ttu-id="fcf4b-106">HTTP 規格 (RFC 2616) 定義為 「 程序時有多種表示選取最佳的表示法，針對給定的回應。 」 的內容交涉</span><span class="sxs-lookup"><span data-stu-id="fcf4b-106">The HTTP specification (RFC 2616) defines content negotiation as "the process of selecting the best representation for a given response when there are multiple representations available."</span></span> <span data-ttu-id="fcf4b-107">在 HTTP 內容交涉的主要機制都是這些要求標頭：</span><span class="sxs-lookup"><span data-stu-id="fcf4b-107">The primary mechanism for content negotiation in HTTP are these request headers:</span></span>

- <span data-ttu-id="fcf4b-108">**接受：** 哪些媒體類型是可接受的回應，例如"application/json"，"application/xml"或自訂的媒體類型，例如&quot;application/vnd.example+xml&quot;</span><span class="sxs-lookup"><span data-stu-id="fcf4b-108">**Accept:** Which media types are acceptable for the response, such as "application/json," "application/xml," or a custom media type such as &quot;application/vnd.example+xml&quot;</span></span>
- <span data-ttu-id="fcf4b-109">**Accept-charset:** 字元集是可接受的例如 utf-8 或 ISO 8859-1。</span><span class="sxs-lookup"><span data-stu-id="fcf4b-109">**Accept-Charset:** Which character sets are acceptable, such as UTF-8 or ISO 8859-1.</span></span>
- <span data-ttu-id="fcf4b-110">**編碼接受：** 內容編碼可接受的例如 gzip。</span><span class="sxs-lookup"><span data-stu-id="fcf4b-110">**Accept-Encoding:** Which content encodings are acceptable, such as gzip.</span></span>
- <span data-ttu-id="fcf4b-111">**接受語言：** 慣用的自然語言，例如 「 en-us-我們"。</span><span class="sxs-lookup"><span data-stu-id="fcf4b-111">**Accept-Language:** The preferred natural language, such as "en-us".</span></span>

<span data-ttu-id="fcf4b-112">伺服器也可以查看 HTTP 要求的其他部分。</span><span class="sxs-lookup"><span data-stu-id="fcf4b-112">The server can also look at other portions of the HTTP request.</span></span> <span data-ttu-id="fcf4b-113">例如，如果要求包含 X 要求與標頭，指出 AJAX 要求，伺服器可能會預設為 JSON 如果沒有 Accept 標頭。</span><span class="sxs-lookup"><span data-stu-id="fcf4b-113">For example, if the request contains an X-Requested-With header, indicating an AJAX request, the server might default to JSON if there is no Accept header.</span></span>

<span data-ttu-id="fcf4b-114">在本文中，我們將探討 Web API 使用的接受和 Accept-charset 標頭的方式。</span><span class="sxs-lookup"><span data-stu-id="fcf4b-114">In this article, we'll look at how Web API uses the Accept and Accept-Charset headers.</span></span> <span data-ttu-id="fcf4b-115">（在此階段中，接受編碼 」 或 「 接受語言沒有內建支援。)</span><span class="sxs-lookup"><span data-stu-id="fcf4b-115">(At this time, there is no built-in support for Accept-Encoding or Accept-Language.)</span></span>

## <a name="serialization"></a><span data-ttu-id="fcf4b-116">序列化</span><span class="sxs-lookup"><span data-stu-id="fcf4b-116">Serialization</span></span>

<span data-ttu-id="fcf4b-117">如果 Web API 控制器會傳回做為 CLR 類型的資源，管線就會序列化傳回的值，並將它寫入至 HTTP 回應主體。</span><span class="sxs-lookup"><span data-stu-id="fcf4b-117">If a Web API controller returns a resource as CLR type, the pipeline serializes the return value and writes it into the HTTP response body.</span></span>

<span data-ttu-id="fcf4b-118">例如，請考慮下列控制器動作：</span><span class="sxs-lookup"><span data-stu-id="fcf4b-118">For example, consider the following controller action:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample1.cs)]

<span data-ttu-id="fcf4b-119">用戶端可能會傳送這個 HTTP 要求：</span><span class="sxs-lookup"><span data-stu-id="fcf4b-119">A client might send this HTTP request:</span></span>

[!code-console[Main](content-negotiation/samples/sample2.cmd)]

<span data-ttu-id="fcf4b-120">在回應中，可能會傳送伺服器：</span><span class="sxs-lookup"><span data-stu-id="fcf4b-120">In response, the server might send:</span></span>

[!code-console[Main](content-negotiation/samples/sample3.cmd)]

<span data-ttu-id="fcf4b-121">在此範例中，用戶端要求 JSON、 Javascript 或 「 任何 」 (\*/\*)。</span><span class="sxs-lookup"><span data-stu-id="fcf4b-121">In this example, the client requested either JSON, Javascript, or "anything" (\*/\*).</span></span> <span data-ttu-id="fcf4b-122">伺服器回應的 JSON 表示法的`Product`物件。</span><span class="sxs-lookup"><span data-stu-id="fcf4b-122">The server responsed with a JSON representation of the `Product` object.</span></span> <span data-ttu-id="fcf4b-123">請注意，在回應中的 Content-type 標頭設定為&quot;application/json&quot;。</span><span class="sxs-lookup"><span data-stu-id="fcf4b-123">Notice that the Content-Type header in the response is set to &quot;application/json&quot;.</span></span>

<span data-ttu-id="fcf4b-124">也可以傳回一個控制站**HttpResponseMessage**物件。</span><span class="sxs-lookup"><span data-stu-id="fcf4b-124">A controller can also return an **HttpResponseMessage** object.</span></span> <span data-ttu-id="fcf4b-125">若要指定，回應主體的 CLR 物件，呼叫**CreateResponse**擴充方法：</span><span class="sxs-lookup"><span data-stu-id="fcf4b-125">To specify a CLR object for the response body, call the **CreateResponse** extension method:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample4.cs)]

<span data-ttu-id="fcf4b-126">此選項可讓您更充分掌控回應詳細資料。</span><span class="sxs-lookup"><span data-stu-id="fcf4b-126">This option gives you more control over the details of the response.</span></span> <span data-ttu-id="fcf4b-127">您可以設定狀態的程式碼中，新增 HTTP 標頭，等等。</span><span class="sxs-lookup"><span data-stu-id="fcf4b-127">You can set the status code, add HTTP headers, and so forth.</span></span>

<span data-ttu-id="fcf4b-128">將序列化資源物件稱為*媒體格式器*。</span><span class="sxs-lookup"><span data-stu-id="fcf4b-128">The object that serializes the resource is called a *media formatter*.</span></span> <span data-ttu-id="fcf4b-129">媒體格式器衍生自**MediaTypeFormatter**類別。</span><span class="sxs-lookup"><span data-stu-id="fcf4b-129">Media formatters derive from the **MediaTypeFormatter** class.</span></span> <span data-ttu-id="fcf4b-130">Web API for XML 和 JSON，提供媒體格式器，您可以建立自訂的格式器，以支援其他類型的媒體。</span><span class="sxs-lookup"><span data-stu-id="fcf4b-130">Web API provides media formatters for XML and JSON, and you can create custom formatters to support other media types.</span></span> <span data-ttu-id="fcf4b-131">如需撰寫自訂的格式器的詳細資訊，請參閱 <<c0> [ 媒體格式器](media-formatters.md)。</span><span class="sxs-lookup"><span data-stu-id="fcf4b-131">For information about writing a custom formatter, see [Media Formatters](media-formatters.md).</span></span>

## <a name="how-content-negotiation-works"></a><span data-ttu-id="fcf4b-132">如何內容交涉運作方式</span><span class="sxs-lookup"><span data-stu-id="fcf4b-132">How Content Negotiation Works</span></span>

<span data-ttu-id="fcf4b-133">首先，管線會取得**IContentNegotiator**服務**HttpConfiguration**物件。</span><span class="sxs-lookup"><span data-stu-id="fcf4b-133">First, the pipeline gets the **IContentNegotiator** service from the **HttpConfiguration** object.</span></span> <span data-ttu-id="fcf4b-134">它也會取得中的媒體格式器的清單**HttpConfiguration.Formatters**集合。</span><span class="sxs-lookup"><span data-stu-id="fcf4b-134">It also gets the list of media formatters from the **HttpConfiguration.Formatters** collection.</span></span>

<span data-ttu-id="fcf4b-135">接下來，管線會呼叫**IContentNegotiatior.Negotiate**，並傳入：</span><span class="sxs-lookup"><span data-stu-id="fcf4b-135">Next, the pipeline calls **IContentNegotiatior.Negotiate**, passing in:</span></span>

- <span data-ttu-id="fcf4b-136">要序列化的物件類型</span><span class="sxs-lookup"><span data-stu-id="fcf4b-136">The type of object to serialize</span></span>
- <span data-ttu-id="fcf4b-137">媒體格式器的集合</span><span class="sxs-lookup"><span data-stu-id="fcf4b-137">The collection of media formatters</span></span>
- <span data-ttu-id="fcf4b-138">HTTP 要求</span><span class="sxs-lookup"><span data-stu-id="fcf4b-138">The HTTP request</span></span>

<span data-ttu-id="fcf4b-139">**交涉**方法會傳回兩項資訊：</span><span class="sxs-lookup"><span data-stu-id="fcf4b-139">The **Negotiate** method returns two pieces of information:</span></span>

- <span data-ttu-id="fcf4b-140">若要使用的格式器</span><span class="sxs-lookup"><span data-stu-id="fcf4b-140">Which formatter to use</span></span>
- <span data-ttu-id="fcf4b-141">回應的媒體類型</span><span class="sxs-lookup"><span data-stu-id="fcf4b-141">The media type for the response</span></span>

<span data-ttu-id="fcf4b-142">找不到，如果**交涉**方法會傳回**null**，和用戶端接收 HTTP 錯誤 406 （無法接受）。</span><span class="sxs-lookup"><span data-stu-id="fcf4b-142">If no formatter is found, the **Negotiate** method returns **null**, and the client recevies HTTP error 406 (Not Acceptable).</span></span>

<span data-ttu-id="fcf4b-143">下列程式碼顯示如何控制站可以直接叫用內容交涉：</span><span class="sxs-lookup"><span data-stu-id="fcf4b-143">The following code shows how a controller can directly invoke content negotiation:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample5.cs)]

<span data-ttu-id="fcf4b-144">此程式碼相當於管線會自動執行。</span><span class="sxs-lookup"><span data-stu-id="fcf4b-144">This code is equivalent to the what the pipeline does automatically.</span></span>

## <a name="default-content-negotiator"></a><span data-ttu-id="fcf4b-145">預設內容 Negotiator</span><span class="sxs-lookup"><span data-stu-id="fcf4b-145">Default Content Negotiator</span></span>

<span data-ttu-id="fcf4b-146">**DefaultContentNegotiator**類別提供的預設實作**IContentNegotiator**。</span><span class="sxs-lookup"><span data-stu-id="fcf4b-146">The **DefaultContentNegotiator** class provides the default implementation of **IContentNegotiator**.</span></span> <span data-ttu-id="fcf4b-147">它會使用數個條件選取格式器。</span><span class="sxs-lookup"><span data-stu-id="fcf4b-147">It uses several criteria to select a formatter.</span></span>

<span data-ttu-id="fcf4b-148">首先，必須能夠將類型序列化格式器。</span><span class="sxs-lookup"><span data-stu-id="fcf4b-148">First, the formatter must be able to serialize the type.</span></span> <span data-ttu-id="fcf4b-149">此乃藉由呼叫**MediaTypeFormatter.CanWriteType**。</span><span class="sxs-lookup"><span data-stu-id="fcf4b-149">This is verified by calling **MediaTypeFormatter.CanWriteType**.</span></span>

<span data-ttu-id="fcf4b-150">接下來，器會查看每個格式器，並評估其符合 HTTP 要求的程度。</span><span class="sxs-lookup"><span data-stu-id="fcf4b-150">Next, the content negotiator looks at each formatter and evaluates how well it matches the HTTP request.</span></span> <span data-ttu-id="fcf4b-151">若要評估相符項目，器會查看兩件事上格式器中：</span><span class="sxs-lookup"><span data-stu-id="fcf4b-151">To evaluate the match, the content negotiator looks at two things on the formatter:</span></span>

- <span data-ttu-id="fcf4b-152">**SupportedMediaTypes**集合，其中包含一份支援的媒體類型。</span><span class="sxs-lookup"><span data-stu-id="fcf4b-152">The **SupportedMediaTypes** collection, which contains a list of supported media types.</span></span> <span data-ttu-id="fcf4b-153">器會嘗試比對這份清單，針對要求 Accept 標頭。</span><span class="sxs-lookup"><span data-stu-id="fcf4b-153">The content negotiator tries to match this list against the request Accept header.</span></span> <span data-ttu-id="fcf4b-154">請注意 Accept 標頭可以包含範圍。</span><span class="sxs-lookup"><span data-stu-id="fcf4b-154">Note that the Accept header can include ranges.</span></span> <span data-ttu-id="fcf4b-155">例如，"text/plain"是文字的相符項目 /\*或是\* / \*。</span><span class="sxs-lookup"><span data-stu-id="fcf4b-155">For example, "text/plain" is a match for text/\* or \*/\*.</span></span>
- <span data-ttu-id="fcf4b-156">**MediaTypeMappings**集合，其中包含一份**MediaTypeMapping**物件。</span><span class="sxs-lookup"><span data-stu-id="fcf4b-156">The **MediaTypeMappings** collection, which contains a list of **MediaTypeMapping** objects.</span></span> <span data-ttu-id="fcf4b-157">**MediaTypeMapping**類別提供的泛型的方法，以符合 HTTP 要求的媒體類型。</span><span class="sxs-lookup"><span data-stu-id="fcf4b-157">The **MediaTypeMapping** class provides a generic way to match HTTP requests with media types.</span></span> <span data-ttu-id="fcf4b-158">比方說，它可以將自訂的 HTTP 標頭對應至特定的媒體類型。</span><span class="sxs-lookup"><span data-stu-id="fcf4b-158">For example, it could map a custom HTTP header to a particular media type.</span></span>

<span data-ttu-id="fcf4b-159">如果有多個比對，最高的品質因素 wins 的相符結果。</span><span class="sxs-lookup"><span data-stu-id="fcf4b-159">If there are multiple matches, the match with the highest quality factor wins.</span></span> <span data-ttu-id="fcf4b-160">例如: </span><span class="sxs-lookup"><span data-stu-id="fcf4b-160">For example:</span></span>

[!code-console[Main](content-negotiation/samples/sample6.cmd)]

<span data-ttu-id="fcf4b-161">在此範例中，application/json 會有為 1.0，隱含的品質係數，因此最好是透過 application/xml。</span><span class="sxs-lookup"><span data-stu-id="fcf4b-161">In this example, application/json has an implied quality factor of 1.0, so it is preferred over application/xml.</span></span>

<span data-ttu-id="fcf4b-162">如果找不到任何相符項目，器會嘗試比對媒體類型的要求主體中，如果有的話。</span><span class="sxs-lookup"><span data-stu-id="fcf4b-162">If no matches are found, the content negotiator tries to match on the media type of the request body, if any.</span></span> <span data-ttu-id="fcf4b-163">例如，如果要求包含 JSON 資料，器會尋找 JSON 格式器。</span><span class="sxs-lookup"><span data-stu-id="fcf4b-163">For example, if the request contains JSON data, the content negotiator looks for a JSON formatter.</span></span>

<span data-ttu-id="fcf4b-164">如果仍不有任何相符項目，則器只會挑選可以將類型序列化的第一個格式器。</span><span class="sxs-lookup"><span data-stu-id="fcf4b-164">If there are still no matches, the content negotiator simply picks the first formatter that can serialize the type.</span></span>

## <a name="selecting-a-character-encoding"></a><span data-ttu-id="fcf4b-165">選取的字元編碼</span><span class="sxs-lookup"><span data-stu-id="fcf4b-165">Selecting a Character Encoding</span></span>

<span data-ttu-id="fcf4b-166">選取格式器之後，會選擇器的最佳字元編碼來看看**SupportedEncodings**屬性格式器，並比對要求 Accept-charset 標頭 （如果有的話）。</span><span class="sxs-lookup"><span data-stu-id="fcf4b-166">After a formatter is selected, the content negotiator chooses the best character encoding by looking at the **SupportedEncodings** property on the formatter, and matching it against the Accept-Charset header in the request (if any).</span></span>
