---
uid: web-api/overview/advanced/httpclient-message-handlers
title: ASP.NET Web API 的 HttpClient 訊息處理常式 |Microsoft 文件
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2012
ms.topic: article
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 805741b0ac682b7479ce82127df48b1b9a49a427
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
ms.locfileid: "26506787"
---
<a name="httpclient-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="e5c3d-102">ASP.NET Web API 的 HttpClient 訊息處理常式</span><span class="sxs-lookup"><span data-stu-id="e5c3d-102">HttpClient Message Handlers in ASP.NET Web API</span></span>
====================
<span data-ttu-id="e5c3d-103">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e5c3d-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="e5c3d-104">A*訊息處理常式*是接收 HTTP 要求，並傳回 HTTP 回應的類別。</span><span class="sxs-lookup"><span data-stu-id="e5c3d-104">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span>

<span data-ttu-id="e5c3d-105">一般而言，一系列的訊息處理常式會鏈結在一起。</span><span class="sxs-lookup"><span data-stu-id="e5c3d-105">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="e5c3d-106">第一個處理常式會接收 HTTP 要求，執行一些處理，並讓下一個處理常式要求。</span><span class="sxs-lookup"><span data-stu-id="e5c3d-106">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="e5c3d-107">在某些時候，回應會建立，並會回到鏈結。</span><span class="sxs-lookup"><span data-stu-id="e5c3d-107">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="e5c3d-108">此模式呼叫*委派*處理常式。</span><span class="sxs-lookup"><span data-stu-id="e5c3d-108">This pattern is called a *delegating* handler.</span></span>

![](httpclient-message-handlers/_static/image1.png)

<span data-ttu-id="e5c3d-109">在用戶端， **HttpClient**類別使用的訊息處理常式來處理要求。</span><span class="sxs-lookup"><span data-stu-id="e5c3d-109">On the client side, the **HttpClient** class uses a message handler to process requests.</span></span> <span data-ttu-id="e5c3d-110">預設處理常式是**HttpClientHandler**，其透過網路傳送要求，並從伺服器取得回應。</span><span class="sxs-lookup"><span data-stu-id="e5c3d-110">The default handler is **HttpClientHandler**, which sends the request over the network and gets the response from the server.</span></span> <span data-ttu-id="e5c3d-111">您可以插入自訂訊息處理常式到管線的用戶端：</span><span class="sxs-lookup"><span data-stu-id="e5c3d-111">You can insert custom message handlers into the client pipeline:</span></span>

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="e5c3d-112">ASP.NET Web API 也會使用伺服器端的訊息處理常式。</span><span class="sxs-lookup"><span data-stu-id="e5c3d-112">ASP.NET Web API also uses message handlers on the server side.</span></span> <span data-ttu-id="e5c3d-113">如需詳細資訊，請參閱[HTTP 訊息處理常式](http-message-handlers.md)。</span><span class="sxs-lookup"><span data-stu-id="e5c3d-113">For more information, see [HTTP Message Handlers](http-message-handlers.md).</span></span>


## <a name="custom-message-handlers"></a><span data-ttu-id="e5c3d-114">自訂訊息處理常式</span><span class="sxs-lookup"><span data-stu-id="e5c3d-114">Custom Message Handlers</span></span>

<span data-ttu-id="e5c3d-115">若要撰寫的自訂訊息處理常式，衍生自**System.Net.Http.DelegatingHandler**並覆寫**SendAsync**方法。</span><span class="sxs-lookup"><span data-stu-id="e5c3d-115">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="e5c3d-116">以下是方法簽章：</span><span class="sxs-lookup"><span data-stu-id="e5c3d-116">Here is the method signature:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

<span data-ttu-id="e5c3d-117">這個方法會接受**HttpRequestMessage**做為輸入，並以非同步方式傳回**HttpResponseMessage**。</span><span class="sxs-lookup"><span data-stu-id="e5c3d-117">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="e5c3d-118">一般實作會執行下列動作：</span><span class="sxs-lookup"><span data-stu-id="e5c3d-118">A typical implementation does the following:</span></span>

1. <span data-ttu-id="e5c3d-119">處理要求訊息。</span><span class="sxs-lookup"><span data-stu-id="e5c3d-119">Process the request message.</span></span>
2. <span data-ttu-id="e5c3d-120">呼叫`base.SendAsync`將要求傳送至內部處理常式。</span><span class="sxs-lookup"><span data-stu-id="e5c3d-120">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="e5c3d-121">內部處理常式會傳回回應訊息。</span><span class="sxs-lookup"><span data-stu-id="e5c3d-121">The inner handler returns a response message.</span></span> <span data-ttu-id="e5c3d-122">（此步驟是非同步的）。</span><span class="sxs-lookup"><span data-stu-id="e5c3d-122">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="e5c3d-123">處理回應，並將它傳回給呼叫者。</span><span class="sxs-lookup"><span data-stu-id="e5c3d-123">Process the response and return it to the caller.</span></span>

<span data-ttu-id="e5c3d-124">下列範例會示範將自訂標頭加入至傳出要求的訊息處理常式：</span><span class="sxs-lookup"><span data-stu-id="e5c3d-124">The following example shows a message handler that adds a custom header to the outgoing request:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

<span data-ttu-id="e5c3d-125">若要呼叫`base.SendAsync`為非同步。</span><span class="sxs-lookup"><span data-stu-id="e5c3d-125">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="e5c3d-126">如果此處理常式沒有任何工作在這個呼叫之後，使用**await**方法完成之後繼續執行至關鍵字。</span><span class="sxs-lookup"><span data-stu-id="e5c3d-126">If the handler does any work after this call, use the **await** keyword to resume execution after the method completes.</span></span> <span data-ttu-id="e5c3d-127">下列範例會記錄錯誤代碼的處理常式。</span><span class="sxs-lookup"><span data-stu-id="e5c3d-127">The following example shows a handler that logs error codes.</span></span> <span data-ttu-id="e5c3d-128">記錄本身不是非常有趣，但此範例示範如何取得在處理常式內部的回應。</span><span class="sxs-lookup"><span data-stu-id="e5c3d-128">The logging itself is not very interesting, but the example shows how to get at the response inside the handler.</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a><span data-ttu-id="e5c3d-129">加入至用戶端管線的訊息處理常式</span><span class="sxs-lookup"><span data-stu-id="e5c3d-129">Adding Message Handlers to the Client Pipeline</span></span>

<span data-ttu-id="e5c3d-130">若要加入自訂的處理常式來**HttpClient**，使用**HttpClientFactory.Create**方法：</span><span class="sxs-lookup"><span data-stu-id="e5c3d-130">To add custom handlers to **HttpClient**, use the **HttpClientFactory.Create** method:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

<span data-ttu-id="e5c3d-131">訊息處理常式會呼叫您傳遞到順序**建立**方法。</span><span class="sxs-lookup"><span data-stu-id="e5c3d-131">Message handlers are called in the order that you pass them into the **Create** method.</span></span> <span data-ttu-id="e5c3d-132">因為處理常式為巢狀，回應訊息會傳送另一方向。</span><span class="sxs-lookup"><span data-stu-id="e5c3d-132">Because handlers are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="e5c3d-133">也就是最後一個處理常式是最先收到回應訊息。</span><span class="sxs-lookup"><span data-stu-id="e5c3d-133">That is, the last handler is the first to get the response message.</span></span>
