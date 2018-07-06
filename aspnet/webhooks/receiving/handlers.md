---
uid: webhooks/receiving/handlers
title: ASP.NET Webhook 處理常式 |Microsoft Docs
author: rick-anderson
description: 如何處理 ASP.NET Webhook 的要求。
ms.author: aspnetcontent
ms.date: 01/17/2012
ms.assetid: a55b0d20-9c90-4bd3-a471-20da6f569f0c
ms.openlocfilehash: bc8f4ef3f4ade775b395d73dfa8d73fec92fba3f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841418"
---
# <a name="aspnet-webhooks-handlers"></a><span data-ttu-id="80297-103">ASP.NET Webhook 處理常式</span><span class="sxs-lookup"><span data-stu-id="80297-103">ASP.NET WebHooks handlers</span></span>

<span data-ttu-id="80297-104">一旦 Webhook 要求由 WebHook 接收器已驗證，就準備好處理使用者程式碼。</span><span class="sxs-lookup"><span data-stu-id="80297-104">Once WebHooks requests has been validated by a WebHook receiver, it is ready to be processed by user code.</span></span> <span data-ttu-id="80297-105">這正是*處理常式*進來。</span><span class="sxs-lookup"><span data-stu-id="80297-105">This is where *handlers* come in.</span></span> <span data-ttu-id="80297-106">處理常式衍生自[IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs)介面，但通常會使用[WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs)而不是直接從介面衍生的類別。</span><span class="sxs-lookup"><span data-stu-id="80297-106">Handlers derive from the [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) interface but typically uses the [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) class instead of deriving directly from the interface.</span></span>

<span data-ttu-id="80297-107">WebHook 要求可以由一或多個處理常式處理。</span><span class="sxs-lookup"><span data-stu-id="80297-107">A WebHook request can be processed by one or more handlers.</span></span> <span data-ttu-id="80297-108">根據其各自的順序呼叫處理常式*順序*屬性會從最低到最高順序為簡單整數 （建議為 1 到 100 之間） 的位置：</span><span class="sxs-lookup"><span data-stu-id="80297-108">Handlers are called in order based on their respective *Order* property going from lowest to highest where Order is a simple integer (suggested to be between 1 and 100):</span></span>

![WebHook 處理常式的順序屬性圖表](_static/Handlers.png)

<span data-ttu-id="80297-110">可選擇性地設定處理常式*回應*WebHookHandlerContext 停止並回復為 WebHook 的 HTTP 回應傳送回應導致處理上的屬性。</span><span class="sxs-lookup"><span data-stu-id="80297-110">A handler can optionally set the *Response* property on the WebHookHandlerContext which will lead the processing to stop and the response to be sent back as the HTTP response to the WebHook.</span></span> <span data-ttu-id="80297-111">在上述案例中，因為它具有較高的順序，比 B 且 B 設定的回應，將不會取得呼叫處理常式 C。</span><span class="sxs-lookup"><span data-stu-id="80297-111">In the case above, Handler C won't get called because it has a higher order than B and B sets the response.</span></span>

<span data-ttu-id="80297-112">設定回應通常是只與相關的 Webhook 回應可以在其中執行資訊傳回到原始的 API。</span><span class="sxs-lookup"><span data-stu-id="80297-112">Setting the response is typically only relevant for WebHooks where the response can carry information back to the originating API.</span></span> <span data-ttu-id="80297-113">比方說，這是傳回給 WebHook 的來源的通道公佈回應 Slack Webhook 的情況。</span><span class="sxs-lookup"><span data-stu-id="80297-113">This is for example the case with Slack WebHooks where the response is posted back to the channel where the WebHook came from.</span></span> <span data-ttu-id="80297-114">處理常式可以設定接收者屬性，如果他們只想要接收來自該特定接收者的 Webhook。</span><span class="sxs-lookup"><span data-stu-id="80297-114">Handlers can set the Receiver property if they only want to receive WebHooks from that particular receiver.</span></span> <span data-ttu-id="80297-115">如果它們不需要設定的接收者針對它們全部呼叫它們。</span><span class="sxs-lookup"><span data-stu-id="80297-115">If they don't set the receiver they are called for all of them.</span></span>

<span data-ttu-id="80297-116">回應的其他常見用法是使用*410 不存在*回應指出 WebHook 不再為作用中，且應該提交任何進一步的要求。</span><span class="sxs-lookup"><span data-stu-id="80297-116">One other common use of a response is to use a *410 Gone* response to indicate that the WebHook no longer is active and no further requests should be submitted.</span></span>

<span data-ttu-id="80297-117">根據預設處理常式會呼叫所有的 WebHook 接收器。</span><span class="sxs-lookup"><span data-stu-id="80297-117">By default a handler will be called by all WebHook receivers.</span></span> <span data-ttu-id="80297-118">不過，如果*接收者*屬性設定為處理常式的名稱，則該處理常式只會從該接收者收到 WebHook 要求。</span><span class="sxs-lookup"><span data-stu-id="80297-118">However, if the *Receiver* property is set to the name of a handler then that handler will only receive WebHook requests from that receiver.</span></span>

## <a name="processing-a-webhook"></a><span data-ttu-id="80297-119">處理 WebHook</span><span class="sxs-lookup"><span data-stu-id="80297-119">Processing a WebHook</span></span>

<span data-ttu-id="80297-120">呼叫處理常式時，它會取得[WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs)包含 WebHook 要求的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="80297-120">When a handler is called, it gets a [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) containing information about the WebHook request.</span></span> <span data-ttu-id="80297-121">的資料，通常是 HTTP 要求本文中，可從*資料*屬性。</span><span class="sxs-lookup"><span data-stu-id="80297-121">The data, typically the HTTP request body, is available from the *Data* property.</span></span>

<span data-ttu-id="80297-122">資料類型通常是 JSON 或 HTML 表單資料，但可以轉換成更明確的類型，如有需要。</span><span class="sxs-lookup"><span data-stu-id="80297-122">The type of the data is typically JSON or HTML form data, but it is possible to cast to a more specific type if desired.</span></span> <span data-ttu-id="80297-123">例如，ASP.NET Webhook 所產生的自訂 Webhook 可以轉換成類型[CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) ，如下所示：</span><span class="sxs-lookup"><span data-stu-id="80297-123">For example, the custom WebHooks generated by ASP.NET WebHooks can be cast to the type [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) as follows:</span></span>

```csharp
public class MyWebHookHandler : WebHookHandler
{
    public MyWebHookHandler()
    {
        this.Receiver = "custom";
    }

    public override Task ExecuteAsync(string generator, WebHookHandlerContext context)
    {
        CustomNotifications notifications = context.GetDataOrDefault<CustomNotifications>();
        foreach (var notification in notifications.Notifications)
        {
            ...
        }
        return Task.FromResult(true);
    }
}
```

  ## <a name="queued-processing"></a><span data-ttu-id="80297-124">已排入佇列的處理</span><span class="sxs-lookup"><span data-stu-id="80297-124">Queued Processing</span></span>

<span data-ttu-id="80297-125">如果回應不產生在數秒內，大部分的 WebHook 寄件者會重新傳送 WebHook。</span><span class="sxs-lookup"><span data-stu-id="80297-125">Most WebHook senders will resend a WebHook if a response is not generated within a handful of seconds.</span></span> <span data-ttu-id="80297-126">這表示您的處理常式必須完成處理，為了不讓它再次呼叫該時間範圍內。</span><span class="sxs-lookup"><span data-stu-id="80297-126">This means that your handler must complete the processing within that time frame in order not for it to be called again.</span></span>

<span data-ttu-id="80297-127">如果處理所花費的時間，或最好在個別處理然後[WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs)可用來將 WebHook 要求提交至佇列，例如[Azure 儲存體佇列](https://msdn.microsoft.com/library/azure/dd179353.aspx)。</span><span class="sxs-lookup"><span data-stu-id="80297-127">If the processing takes longer, or is better handled separately then the [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) can be used to submit the WebHook request to a queue, for example [Azure Storage Queue](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span></span>

<span data-ttu-id="80297-128">概述[WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs)這裡提供實作：</span><span class="sxs-lookup"><span data-stu-id="80297-128">An outline of a [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) implementation is provided here:</span></span>

```csharp
public class QueueHandler : WebHookQueueHandler
{
    public override Task EnqueueAsync(WebHookQueueContext context)
    {

        // Enqueue WebHookQueueContext to your queuing system of choice

        return Task.FromResult(true);
    }
}
```
