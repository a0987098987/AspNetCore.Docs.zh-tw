---
uid: web-api/overview/advanced/httpclient-message-handlers
title: ASP.NET Web API 中的 HttpClient 訊息處理常式 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/01/2012
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 764244d1299d8cfcb59c3f15d63b42ebff4f6ac0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831841"
---
<a name="httpclient-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="7eba4-102">ASP.NET Web API 中的 HttpClient 訊息處理常式</span><span class="sxs-lookup"><span data-stu-id="7eba4-102">HttpClient Message Handlers in ASP.NET Web API</span></span>
====================
<span data-ttu-id="7eba4-103">藉由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="7eba4-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="7eba4-104">A*訊息處理常式*是接收 HTTP 要求，並傳回 HTTP 回應的類別。</span><span class="sxs-lookup"><span data-stu-id="7eba4-104">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span>

<span data-ttu-id="7eba4-105">一般而言，一系列的訊息處理常式鏈結在一起。</span><span class="sxs-lookup"><span data-stu-id="7eba4-105">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="7eba4-106">第一個處理常式會接收 HTTP 要求、 執行一些處理、 和讓下一個處理常式的要求。</span><span class="sxs-lookup"><span data-stu-id="7eba4-106">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="7eba4-107">在某些時候，回應會建立，並會回到鏈結。</span><span class="sxs-lookup"><span data-stu-id="7eba4-107">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="7eba4-108">這個模式稱為*委派*處理常式。</span><span class="sxs-lookup"><span data-stu-id="7eba4-108">This pattern is called a *delegating* handler.</span></span>

![](httpclient-message-handlers/_static/image1.png)

<span data-ttu-id="7eba4-109">在用戶端**HttpClient**類別使用的訊息處理常式來處理要求。</span><span class="sxs-lookup"><span data-stu-id="7eba4-109">On the client side, the **HttpClient** class uses a message handler to process requests.</span></span> <span data-ttu-id="7eba4-110">預設處理常式**HttpClientHandler**，這會透過網路傳送要求，並從伺服器取得回應。</span><span class="sxs-lookup"><span data-stu-id="7eba4-110">The default handler is **HttpClientHandler**, which sends the request over the network and gets the response from the server.</span></span> <span data-ttu-id="7eba4-111">您可以將自訂訊息處理常式插入用戶端管線：</span><span class="sxs-lookup"><span data-stu-id="7eba4-111">You can insert custom message handlers into the client pipeline:</span></span>

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="7eba4-112">ASP.NET Web API 也會使用伺服器端的訊息處理常式。</span><span class="sxs-lookup"><span data-stu-id="7eba4-112">ASP.NET Web API also uses message handlers on the server side.</span></span> <span data-ttu-id="7eba4-113">如需詳細資訊，請參閱 < [HTTP 訊息處理常式](http-message-handlers.md)。</span><span class="sxs-lookup"><span data-stu-id="7eba4-113">For more information, see [HTTP Message Handlers](http-message-handlers.md).</span></span>


## <a name="custom-message-handlers"></a><span data-ttu-id="7eba4-114">自訂訊息處理常式</span><span class="sxs-lookup"><span data-stu-id="7eba4-114">Custom Message Handlers</span></span>

<span data-ttu-id="7eba4-115">若要撰寫的自訂訊息處理常式，衍生自**System.Net.Http.DelegatingHandler** ，並覆寫**SendAsync**方法。</span><span class="sxs-lookup"><span data-stu-id="7eba4-115">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="7eba4-116">以下是方法簽章：</span><span class="sxs-lookup"><span data-stu-id="7eba4-116">Here is the method signature:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

<span data-ttu-id="7eba4-117">此方法會採用**HttpRequestMessage**做為輸入，並以非同步方式傳回**HttpResponseMessage**。</span><span class="sxs-lookup"><span data-stu-id="7eba4-117">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="7eba4-118">一般實作會執行下列作業：</span><span class="sxs-lookup"><span data-stu-id="7eba4-118">A typical implementation does the following:</span></span>

1. <span data-ttu-id="7eba4-119">處理要求訊息。</span><span class="sxs-lookup"><span data-stu-id="7eba4-119">Process the request message.</span></span>
2. <span data-ttu-id="7eba4-120">呼叫`base.SendAsync`將要求傳送至內部處理常式。</span><span class="sxs-lookup"><span data-stu-id="7eba4-120">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="7eba4-121">內部處理常式會傳回回應訊息。</span><span class="sxs-lookup"><span data-stu-id="7eba4-121">The inner handler returns a response message.</span></span> <span data-ttu-id="7eba4-122">（此步驟是非同步的）。</span><span class="sxs-lookup"><span data-stu-id="7eba4-122">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="7eba4-123">處理回應，並將它傳回給呼叫者。</span><span class="sxs-lookup"><span data-stu-id="7eba4-123">Process the response and return it to the caller.</span></span>

<span data-ttu-id="7eba4-124">下列範例示範將自訂標頭新增至傳出要求的訊息處理常式：</span><span class="sxs-lookup"><span data-stu-id="7eba4-124">The following example shows a message handler that adds a custom header to the outgoing request:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

<span data-ttu-id="7eba4-125">若要呼叫`base.SendAsync`是非同步的。</span><span class="sxs-lookup"><span data-stu-id="7eba4-125">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="7eba4-126">如果處理常式會執行任何工作，此呼叫之後，使用**await**關鍵字，在方法完成之後繼續執行。</span><span class="sxs-lookup"><span data-stu-id="7eba4-126">If the handler does any work after this call, use the **await** keyword to resume execution after the method completes.</span></span> <span data-ttu-id="7eba4-127">下列範例示範會記錄錯誤代碼的處理常式。</span><span class="sxs-lookup"><span data-stu-id="7eba4-127">The following example shows a handler that logs error codes.</span></span> <span data-ttu-id="7eba4-128">記錄本身不太妙，但此範例示範如何取得在處理常式內回應。</span><span class="sxs-lookup"><span data-stu-id="7eba4-128">The logging itself is not very interesting, but the example shows how to get at the response inside the handler.</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a><span data-ttu-id="7eba4-129">將訊息處理常式新增至用戶端管線</span><span class="sxs-lookup"><span data-stu-id="7eba4-129">Adding Message Handlers to the Client Pipeline</span></span>

<span data-ttu-id="7eba4-130">若要加入自訂的處理常式來**HttpClient**，使用**HttpClientFactory.Create**方法：</span><span class="sxs-lookup"><span data-stu-id="7eba4-130">To add custom handlers to **HttpClient**, use the **HttpClientFactory.Create** method:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

<span data-ttu-id="7eba4-131">您傳遞它們的順序呼叫訊息處理常式**建立**方法。</span><span class="sxs-lookup"><span data-stu-id="7eba4-131">Message handlers are called in the order that you pass them into the **Create** method.</span></span> <span data-ttu-id="7eba4-132">因為處理常式為巢狀，回應訊息會傳送另一個方向。</span><span class="sxs-lookup"><span data-stu-id="7eba4-132">Because handlers are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="7eba4-133">也就是最後一個處理常式會是第一個取得回應訊息。</span><span class="sxs-lookup"><span data-stu-id="7eba4-133">That is, the last handler is the first to get the response message.</span></span>
