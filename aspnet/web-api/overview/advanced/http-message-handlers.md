---
uid: web-api/overview/advanced/http-message-handlers
title: 在 ASP.NET Web API HTTP 訊息處理常式 |Microsoft 文件
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2012
ms.topic: article
ms.assetid: 9002018b-3aa3-4358-bb1c-fbb5bc751d01
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/http-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 931ad3330d5e4dc2424ab37b8a6e2a3d123a8684
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
ms.locfileid: "26506947"
---
<a name="http-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="06b6b-102">在 ASP.NET Web API HTTP 訊息處理常式</span><span class="sxs-lookup"><span data-stu-id="06b6b-102">HTTP Message Handlers in ASP.NET Web API</span></span>
====================
<span data-ttu-id="06b6b-103">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="06b6b-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="06b6b-104">A*訊息處理常式*是接收 HTTP 要求，並傳回 HTTP 回應的類別。</span><span class="sxs-lookup"><span data-stu-id="06b6b-104">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span> <span data-ttu-id="06b6b-105">訊息處理常式衍生自抽象**HttpMessageHandler**類別。</span><span class="sxs-lookup"><span data-stu-id="06b6b-105">Message handlers derive from the abstract **HttpMessageHandler** class.</span></span>

<span data-ttu-id="06b6b-106">一般而言，一系列的訊息處理常式會鏈結在一起。</span><span class="sxs-lookup"><span data-stu-id="06b6b-106">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="06b6b-107">第一個處理常式會接收 HTTP 要求，執行一些處理，並讓下一個處理常式要求。</span><span class="sxs-lookup"><span data-stu-id="06b6b-107">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="06b6b-108">在某些時候，回應會建立，並會回到鏈結。</span><span class="sxs-lookup"><span data-stu-id="06b6b-108">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="06b6b-109">此模式呼叫*委派*處理常式。</span><span class="sxs-lookup"><span data-stu-id="06b6b-109">This pattern is called a *delegating* handler.</span></span>

![](http-message-handlers/_static/image1.png)

## <a name="server-side-message-handlers"></a><span data-ttu-id="06b6b-110">伺服器端訊息處理常式</span><span class="sxs-lookup"><span data-stu-id="06b6b-110">Server-Side Message Handlers</span></span>

<span data-ttu-id="06b6b-111">在伺服器端，Web API 管線會使用某些內建的訊息處理常式：</span><span class="sxs-lookup"><span data-stu-id="06b6b-111">On the server side, the Web API pipeline uses some built-in message handlers:</span></span>

- <span data-ttu-id="06b6b-112">**HttpServer**從主機取得要求。</span><span class="sxs-lookup"><span data-stu-id="06b6b-112">**HttpServer** gets the request from the host.</span></span>
- <span data-ttu-id="06b6b-113">**HttpRoutingDispatcher**分派根據路由的要求。</span><span class="sxs-lookup"><span data-stu-id="06b6b-113">**HttpRoutingDispatcher** dispatches the request based on the route.</span></span>
- <span data-ttu-id="06b6b-114">**HttpControllerDispatcher**將要求傳送至 Web API 控制器。</span><span class="sxs-lookup"><span data-stu-id="06b6b-114">**HttpControllerDispatcher** sends the request to a Web API controller.</span></span>

<span data-ttu-id="06b6b-115">您可以新增自訂處理常式管線。</span><span class="sxs-lookup"><span data-stu-id="06b6b-115">You can add custom handlers to the pipeline.</span></span> <span data-ttu-id="06b6b-116">訊息處理常式的有效期限為跨領域考量在 HTTP 訊息 （而非控制器動作） 的層級運作。</span><span class="sxs-lookup"><span data-stu-id="06b6b-116">Message handlers are good for cross-cutting concerns that operate at the level of HTTP messages (rather than controller actions).</span></span> <span data-ttu-id="06b6b-117">例如，可能會將訊息處理常式：</span><span class="sxs-lookup"><span data-stu-id="06b6b-117">For example, a message handler might:</span></span>

- <span data-ttu-id="06b6b-118">讀取或修改要求標頭。</span><span class="sxs-lookup"><span data-stu-id="06b6b-118">Read or modify request headers.</span></span>
- <span data-ttu-id="06b6b-119">回應標頭加入回應。</span><span class="sxs-lookup"><span data-stu-id="06b6b-119">Add a response header to responses.</span></span>
- <span data-ttu-id="06b6b-120">到達之前加以控制站驗證的要求。</span><span class="sxs-lookup"><span data-stu-id="06b6b-120">Validate requests before they reach the controller.</span></span>

<span data-ttu-id="06b6b-121">下圖顯示兩個自訂處理常式插入管線：</span><span class="sxs-lookup"><span data-stu-id="06b6b-121">This diagram shows two custom handlers inserted into the pipeline:</span></span>

![](http-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="06b6b-122">在用戶端，HttpClient 也會使用訊息處理常式。</span><span class="sxs-lookup"><span data-stu-id="06b6b-122">On the client side, HttpClient also uses message handlers.</span></span> <span data-ttu-id="06b6b-123">如需詳細資訊，請參閱[HttpClient 訊息處理常式](httpclient-message-handlers.md)。</span><span class="sxs-lookup"><span data-stu-id="06b6b-123">For more information, see [HttpClient Message Handlers](httpclient-message-handlers.md).</span></span>


## <a name="custom-message-handlers"></a><span data-ttu-id="06b6b-124">自訂訊息處理常式</span><span class="sxs-lookup"><span data-stu-id="06b6b-124">Custom Message Handlers</span></span>

<span data-ttu-id="06b6b-125">若要撰寫的自訂訊息處理常式，衍生自**System.Net.Http.DelegatingHandler**並覆寫**SendAsync**方法。</span><span class="sxs-lookup"><span data-stu-id="06b6b-125">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="06b6b-126">此方法具有下列簽章：</span><span class="sxs-lookup"><span data-stu-id="06b6b-126">This method has the following signature:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample1.cs)]

<span data-ttu-id="06b6b-127">這個方法會接受**HttpRequestMessage**做為輸入，並以非同步方式傳回**HttpResponseMessage**。</span><span class="sxs-lookup"><span data-stu-id="06b6b-127">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="06b6b-128">一般實作會執行下列動作：</span><span class="sxs-lookup"><span data-stu-id="06b6b-128">A typical implementation does the following:</span></span>

1. <span data-ttu-id="06b6b-129">處理要求訊息。</span><span class="sxs-lookup"><span data-stu-id="06b6b-129">Process the request message.</span></span>
2. <span data-ttu-id="06b6b-130">呼叫`base.SendAsync`將要求傳送至內部處理常式。</span><span class="sxs-lookup"><span data-stu-id="06b6b-130">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="06b6b-131">內部處理常式會傳回回應訊息。</span><span class="sxs-lookup"><span data-stu-id="06b6b-131">The inner handler returns a response message.</span></span> <span data-ttu-id="06b6b-132">（此步驟是非同步的）。</span><span class="sxs-lookup"><span data-stu-id="06b6b-132">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="06b6b-133">處理回應，並將它傳回給呼叫者。</span><span class="sxs-lookup"><span data-stu-id="06b6b-133">Process the response and return it to the caller.</span></span>

<span data-ttu-id="06b6b-134">以下是簡單的範例：</span><span class="sxs-lookup"><span data-stu-id="06b6b-134">Here is a trivial example:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="06b6b-135">若要呼叫`base.SendAsync`為非同步。</span><span class="sxs-lookup"><span data-stu-id="06b6b-135">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="06b6b-136">如果此處理常式沒有任何工作在這個呼叫之後，使用**await**關鍵字，如下所示。</span><span class="sxs-lookup"><span data-stu-id="06b6b-136">If the handler does any work after this call, use the **await** keyword, as shown.</span></span>


<span data-ttu-id="06b6b-137">委派處理常式可以也略過內部處理常式，並直接建立回應：</span><span class="sxs-lookup"><span data-stu-id="06b6b-137">A delegating handler can also skip the inner handler and directly create the response:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample3.cs)]

<span data-ttu-id="06b6b-138">如果委派處理常式會建立的回應，而不需呼叫`base.SendAsync`，要求會略過其餘的管線。</span><span class="sxs-lookup"><span data-stu-id="06b6b-138">If a delegating handler creates the response without calling `base.SendAsync`, the request skips the rest of the pipeline.</span></span> <span data-ttu-id="06b6b-139">這可用於驗證要求 （建立錯誤回應） 的處理常式。</span><span class="sxs-lookup"><span data-stu-id="06b6b-139">This can be useful for a handler that validates the request (creating an error response).</span></span>

![](http-message-handlers/_static/image3.png)

## <a name="adding-a-handler-to-the-pipeline"></a><span data-ttu-id="06b6b-140">將處理常式新增至管線</span><span class="sxs-lookup"><span data-stu-id="06b6b-140">Adding a Handler to the Pipeline</span></span>

<span data-ttu-id="06b6b-141">若要加入伺服器端的訊息處理常式，加入處理常式**HttpConfiguration.MessageHandlers**集合。</span><span class="sxs-lookup"><span data-stu-id="06b6b-141">To add a message handler on the server side, add the handler to the **HttpConfiguration.MessageHandlers** collection.</span></span> <span data-ttu-id="06b6b-142">如果您使用 「 ASP.NET MVC 4 Web 應用程式 」 範本建立專案時，您可以執行此內部**到 WebApiConfig**類別：</span><span class="sxs-lookup"><span data-stu-id="06b6b-142">If you used the "ASP.NET MVC 4 Web Application" template to create the project, you can do this inside the **WebApiConfig** class:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample4.cs)]

<span data-ttu-id="06b6b-143">訊息處理常式會依照它們出現在相同順序呼叫**MessageHandlers**集合。</span><span class="sxs-lookup"><span data-stu-id="06b6b-143">Message handlers are called in the same order that they appear in **MessageHandlers** collection.</span></span> <span data-ttu-id="06b6b-144">因為它們內嵌，回應訊息會傳送另一方向。</span><span class="sxs-lookup"><span data-stu-id="06b6b-144">Because they are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="06b6b-145">也就是最後一個處理常式是最先收到回應訊息。</span><span class="sxs-lookup"><span data-stu-id="06b6b-145">That is, the last handler is the first to get the response message.</span></span>

<span data-ttu-id="06b6b-146">請注意，您不需要設定內部的處理常式。Web API 架構會自動連結訊息處理常式。</span><span class="sxs-lookup"><span data-stu-id="06b6b-146">Notice that you don't need to set the inner handlers; the Web API framework automatically connects the message handlers.</span></span>

<span data-ttu-id="06b6b-147">如果您是[自我裝載](../older-versions/self-host-a-web-api.md)，建立的執行個體**HttpSelfHostConfiguration**類別，然後將處理常式， **MessageHandlers**集合。</span><span class="sxs-lookup"><span data-stu-id="06b6b-147">If you are [self-hosting](../older-versions/self-host-a-web-api.md), create an instance of the **HttpSelfHostConfiguration** class and add the handlers to the **MessageHandlers** collection.</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample5.cs)]

<span data-ttu-id="06b6b-148">現在讓我們看看自訂訊息處理常式的一些範例。</span><span class="sxs-lookup"><span data-stu-id="06b6b-148">Now let's look at some examples of custom message handlers.</span></span>

## <a name="example-x-http-method-override"></a><span data-ttu-id="06b6b-149">範例： X-HTTP-方法-覆寫</span><span class="sxs-lookup"><span data-stu-id="06b6b-149">Example: X-HTTP-Method-Override</span></span>

<span data-ttu-id="06b6b-150">X-HTTP-方法-覆寫為非標準 HTTP 標頭。</span><span class="sxs-lookup"><span data-stu-id="06b6b-150">X-HTTP-Method-Override is a non-standard HTTP header.</span></span> <span data-ttu-id="06b6b-151">它可供用戶端無法傳送特定 HTTP 要求類型，例如 PUT 或 DELETE。</span><span class="sxs-lookup"><span data-stu-id="06b6b-151">It is designed for clients that cannot send certain HTTP request types, such as PUT or DELETE.</span></span> <span data-ttu-id="06b6b-152">相反地，用戶端傳送 POST 要求，並將 X-HTTP-方法-覆寫標頭設定為所需的方法。</span><span class="sxs-lookup"><span data-stu-id="06b6b-152">Instead, the client sends a POST request and sets the X-HTTP-Method-Override header to the desired method.</span></span> <span data-ttu-id="06b6b-153">例如: </span><span class="sxs-lookup"><span data-stu-id="06b6b-153">For example:</span></span>

[!code-console[Main](http-message-handlers/samples/sample6.cmd)]

<span data-ttu-id="06b6b-154">以下是新增支援 X-HTTP-方法-覆寫的訊息處理常式：</span><span class="sxs-lookup"><span data-stu-id="06b6b-154">Here is a message handler that adds support for X-HTTP-Method-Override:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample7.cs)]

<span data-ttu-id="06b6b-155">在**SendAsync**方法，此處理常式會檢查要求訊息是否為 POST 要求，以及是否包含 X-HTTP-方法-覆寫標頭。</span><span class="sxs-lookup"><span data-stu-id="06b6b-155">In the **SendAsync** method, the handler checks whether the request message is a POST request, and whether it contains the X-HTTP-Method-Override header.</span></span> <span data-ttu-id="06b6b-156">如果是的話，它會驗證標頭值，，，然後修改 要求方法。</span><span class="sxs-lookup"><span data-stu-id="06b6b-156">If so, it validates the header value, and then modifies the request method.</span></span> <span data-ttu-id="06b6b-157">最後，此處理常式會呼叫`base.SendAsync`來傳遞訊息至下一個處理常式。</span><span class="sxs-lookup"><span data-stu-id="06b6b-157">Finally, the handler calls `base.SendAsync` to pass the message to the next handler.</span></span>

<span data-ttu-id="06b6b-158">當要求到達**HttpControllerDispatcher**類別**HttpControllerDispatcher**會路由傳送更新的要求方法為基礎的要求。</span><span class="sxs-lookup"><span data-stu-id="06b6b-158">When the request reaches the **HttpControllerDispatcher** class, **HttpControllerDispatcher** will route the request based on the updated request method.</span></span>

## <a name="example-adding-a-custom-response-header"></a><span data-ttu-id="06b6b-159">範例： 加入自訂的回應標頭</span><span class="sxs-lookup"><span data-stu-id="06b6b-159">Example: Adding a Custom Response Header</span></span>

<span data-ttu-id="06b6b-160">以下是將自訂標頭加入至每個回應訊息的訊息處理常式：</span><span class="sxs-lookup"><span data-stu-id="06b6b-160">Here is a message handler that adds a custom header to every response message:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample8.cs)]

<span data-ttu-id="06b6b-161">首先，處理常式會呼叫`base.SendAsync`將要求傳遞至內部訊息處理常式。</span><span class="sxs-lookup"><span data-stu-id="06b6b-161">First, the handler calls `base.SendAsync` to pass the request to the inner message handler.</span></span> <span data-ttu-id="06b6b-162">內部處理常式會傳回回應訊息，但也不會以非同步方式使用**工作&lt;T&gt;** 物件。</span><span class="sxs-lookup"><span data-stu-id="06b6b-162">The inner handler returns a response message, but it does so asynchronously using a **Task&lt;T&gt;** object.</span></span> <span data-ttu-id="06b6b-163">回應訊息不是後才可以使用`base.SendAsync`以非同步方式完成。</span><span class="sxs-lookup"><span data-stu-id="06b6b-163">The response message is not available until `base.SendAsync` completes asynchronously.</span></span>

<span data-ttu-id="06b6b-164">這個範例會使用**await**關鍵字來執行工作以非同步方式之後`SendAsync`完成。</span><span class="sxs-lookup"><span data-stu-id="06b6b-164">This example uses the **await** keyword to perform work asynchronously after `SendAsync` completes.</span></span> <span data-ttu-id="06b6b-165">如果您的目標.NET Framework 4.0，使用**工作**&lt;T&gt;**。ContinueWith**方法：</span><span class="sxs-lookup"><span data-stu-id="06b6b-165">If you are targeting .NET Framework 4.0, use the **Task**&lt;T&gt;**.ContinueWith** method:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample9.cs)]

## <a name="example-checking-for-an-api-key"></a><span data-ttu-id="06b6b-166">範例： 檢查 API 金鑰</span><span class="sxs-lookup"><span data-stu-id="06b6b-166">Example: Checking for an API Key</span></span>

<span data-ttu-id="06b6b-167">某些 web 服務要求用戶端在要求中包含的 API 金鑰。</span><span class="sxs-lookup"><span data-stu-id="06b6b-167">Some web services require clients to include an API key in their request.</span></span> <span data-ttu-id="06b6b-168">下列範例會示範如何將訊息處理常式可以檢查有效的 API 金鑰的要求：</span><span class="sxs-lookup"><span data-stu-id="06b6b-168">The following example shows how a message handler can check requests for a valid API key:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample10.cs)]

<span data-ttu-id="06b6b-169">這個處理常式會尋找 URI 查詢字串中的 API 金鑰。</span><span class="sxs-lookup"><span data-stu-id="06b6b-169">This handler looks for the API key in the URI query string.</span></span> <span data-ttu-id="06b6b-170">（此範例中，我們假設索引鍵是靜態字串。</span><span class="sxs-lookup"><span data-stu-id="06b6b-170">(For this example, we assume that the key is a static string.</span></span> <span data-ttu-id="06b6b-171">實際實作可能會使用更複雜的驗證。）如果查詢字串包含索引鍵，此處理常式會將要求傳遞至內部處理常式。</span><span class="sxs-lookup"><span data-stu-id="06b6b-171">A real implementation would probably use more complex validation.) If the query string contains the key, the handler passes the request to the inner handler.</span></span>

<span data-ttu-id="06b6b-172">如果要求沒有有效的金鑰，此處理常式建立回應訊息的狀態為 403，禁止。</span><span class="sxs-lookup"><span data-stu-id="06b6b-172">If the request does not have a valid key, the handler creates a response message with status 403, Forbidden.</span></span> <span data-ttu-id="06b6b-173">在此情況下，此處理常式不會呼叫`base.SendAsync`，因此內部處理常式永遠不會接收要求時，也不會控制站。</span><span class="sxs-lookup"><span data-stu-id="06b6b-173">In this case, the handler does not call `base.SendAsync`, so the inner handler never receives the request, nor does the controller.</span></span> <span data-ttu-id="06b6b-174">因此，控制站可以假設所有連入要求都有有效的 API 金鑰。</span><span class="sxs-lookup"><span data-stu-id="06b6b-174">Therefore, the controller can assume that all incoming requests have a valid API key.</span></span>

> [!NOTE]
> <span data-ttu-id="06b6b-175">如果 API 金鑰僅適用於特定控制器的動作，請考慮使用動作篩選條件，而不訊息處理常式。</span><span class="sxs-lookup"><span data-stu-id="06b6b-175">If the API key applies only to certain controller actions, consider using an action filter instead of a message handler.</span></span> <span data-ttu-id="06b6b-176">URI 的路由會執行之後，就會執行動作篩選條件。</span><span class="sxs-lookup"><span data-stu-id="06b6b-176">Action filters run after URI routing is performed.</span></span>


## <a name="per-route-message-handlers"></a><span data-ttu-id="06b6b-177">每個路由訊息處理常式</span><span class="sxs-lookup"><span data-stu-id="06b6b-177">Per-Route Message Handlers</span></span>

<span data-ttu-id="06b6b-178">中的處理常式**HttpConfiguration.MessageHandlers**集合全域套用。</span><span class="sxs-lookup"><span data-stu-id="06b6b-178">Handlers in the **HttpConfiguration.MessageHandlers** collection apply globally.</span></span>

<span data-ttu-id="06b6b-179">或者，您可以加入訊息處理常式至特定的路由，當您定義路由：</span><span class="sxs-lookup"><span data-stu-id="06b6b-179">Alternatively, you can add a message handler to a specific route when you define the route:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample11.cs?highlight=16)]

<span data-ttu-id="06b6b-180">在此範例中，如果要求 URI 符合"Route2 」，要求分派至`MessageHandler2`。</span><span class="sxs-lookup"><span data-stu-id="06b6b-180">In this example, if the request URI matches "Route2", the request is dispatched to `MessageHandler2`.</span></span> <span data-ttu-id="06b6b-181">下圖顯示這兩個路由的管線：</span><span class="sxs-lookup"><span data-stu-id="06b6b-181">The following diagram shows the pipeline for these two routes:</span></span>

![](http-message-handlers/_static/image4.png)

<span data-ttu-id="06b6b-182">請注意，`MessageHandler2`取代預設**HttpControllerDispatcher**。</span><span class="sxs-lookup"><span data-stu-id="06b6b-182">Notice that `MessageHandler2` replaces the default **HttpControllerDispatcher**.</span></span> <span data-ttu-id="06b6b-183">在此範例中，`MessageHandler2`建立回應，並要求的符合"Route2 」 永遠不會移至控制器。</span><span class="sxs-lookup"><span data-stu-id="06b6b-183">In this example, `MessageHandler2` creates the response, and requests that match "Route2" never go to a controller.</span></span> <span data-ttu-id="06b6b-184">這可讓您的整個 Web API 控制器機制取代您自己自訂的端點。</span><span class="sxs-lookup"><span data-stu-id="06b6b-184">This lets you replace the entire Web API controller mechanism with your own custom endpoint.</span></span>

<span data-ttu-id="06b6b-185">或者，每個路由訊息處理常式可以委派給**HttpControllerDispatcher**，然後分派到以控制站。</span><span class="sxs-lookup"><span data-stu-id="06b6b-185">Alternatively, a per-route message handler can delegate to **HttpControllerDispatcher**, which then dispatches to a controller.</span></span>

![](http-message-handlers/_static/image5.png)

<span data-ttu-id="06b6b-186">下列程式碼會示範如何設定此路由：</span><span class="sxs-lookup"><span data-stu-id="06b6b-186">The following code shows how to configure this route:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample12.cs)]
