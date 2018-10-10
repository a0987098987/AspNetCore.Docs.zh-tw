---
uid: signalr/overview/performance/signalr-performance
title: SignalR 效能 |Microsoft Docs
author: pfletcher
description: SignalR 效能
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 3751f5e7-59db-4be0-a290-50abc24e5c84
msc.legacyurl: /signalr/overview/performance/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: 269c10d7a73f181eaceac1c43ad51f3933d6711c
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/10/2018
ms.locfileid: "48911854"
---
<a name="signalr-performance"></a><span data-ttu-id="e0acc-103">SignalR 效能</span><span class="sxs-lookup"><span data-stu-id="e0acc-103">SignalR Performance</span></span>
====================
<span data-ttu-id="e0acc-104">藉由[Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="e0acc-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="e0acc-105">本主題描述如何設計、 測量和改善的 SignalR 應用程式中的效能。</span><span class="sxs-lookup"><span data-stu-id="e0acc-105">This topic describes how to design for, measure, and improve performance in a SignalR application.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="e0acc-106">本主題中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="e0acc-106">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="e0acc-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="e0acc-107">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="e0acc-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="e0acc-108">.NET 4.5</span></span>
> - <span data-ttu-id="e0acc-109">SignalR 第 2 版</span><span class="sxs-lookup"><span data-stu-id="e0acc-109">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="e0acc-110">本主題的上一個版本</span><span class="sxs-lookup"><span data-stu-id="e0acc-110">Previous versions of this topic</span></span>
>
> <span data-ttu-id="e0acc-111">如需舊版 SignalR 的資訊，請參閱[SignalR 舊版](../older-versions/index.md)。</span><span class="sxs-lookup"><span data-stu-id="e0acc-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="e0acc-112">提出問題或意見</span><span class="sxs-lookup"><span data-stu-id="e0acc-112">Questions and comments</span></span>
>
> <span data-ttu-id="e0acc-113">您喜歡本教學課程中的方式，和我們可以改善在頁面底部的註解中，歡迎留下意見反應。</span><span class="sxs-lookup"><span data-stu-id="e0acc-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="e0acc-114">如果您有不直接相關的教學課程中的問題，您可以張貼他們[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或是[StackOverflow.com](http://stackoverflow.com/)。</span><span class="sxs-lookup"><span data-stu-id="e0acc-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="e0acc-115">SignalR 效能和調整的近期簡報，請參閱 <<c0> [ 調整與 ASP.NET SignalR 即時 Web](https://channel9.msdn.com/Events/Build/2013/3-502)。</span><span class="sxs-lookup"><span data-stu-id="e0acc-115">For a recent presentation on SignalR performance and scaling, see [Scaling the Real-time Web with ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span></span>

<span data-ttu-id="e0acc-116">此主題包括下列章節：</span><span class="sxs-lookup"><span data-stu-id="e0acc-116">This topic contains the following sections:</span></span>

- [<span data-ttu-id="e0acc-117">設計考量</span><span class="sxs-lookup"><span data-stu-id="e0acc-117">Design considerations</span></span>](#design)
- [<span data-ttu-id="e0acc-118">微調 SignalR 伺服器效能</span><span class="sxs-lookup"><span data-stu-id="e0acc-118">Tuning your SignalR server for performance</span></span>](#tuning)
- [<span data-ttu-id="e0acc-119">針對效能問題進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="e0acc-119">Troubleshooting performance issues</span></span>](#troubleshooting)
- [<span data-ttu-id="e0acc-120">使用 SignalR 效能計數器</span><span class="sxs-lookup"><span data-stu-id="e0acc-120">Using SignalR performance counters</span></span>](#perfcounters)
- [<span data-ttu-id="e0acc-121">使用其他的效能計數器</span><span class="sxs-lookup"><span data-stu-id="e0acc-121">Using other performance counters</span></span>](#othercounters)
- [<span data-ttu-id="e0acc-122">其他資源</span><span class="sxs-lookup"><span data-stu-id="e0acc-122">Other resources</span></span>](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a><span data-ttu-id="e0acc-123">設計考量</span><span class="sxs-lookup"><span data-stu-id="e0acc-123">Design considerations</span></span>

<span data-ttu-id="e0acc-124">本章節描述可實作 SignalR 應用程式，以確保效能，正在降低藉由產生不必要的網路流量的設計期間的模式。</span><span class="sxs-lookup"><span data-stu-id="e0acc-124">This section describes patterns that can be implemented during the design of a SignalR application, to ensure that performance is not being hindered by generating unnecessary network traffic.</span></span>

### <a name="throttling-message-frequency"></a><span data-ttu-id="e0acc-125">節流的訊息頻率</span><span class="sxs-lookup"><span data-stu-id="e0acc-125">Throttling message frequency</span></span>

<span data-ttu-id="e0acc-126">即使在送出訊息 （例如即時遊戲應用程式） 的高頻率的應用程式，大部分的應用程式不需要傳送第二個的多個訊息。</span><span class="sxs-lookup"><span data-stu-id="e0acc-126">Even in an application that sends out messages at a high frequency (such as a realtime gaming application), most applications don't need to send more than a few messages a second.</span></span> <span data-ttu-id="e0acc-127">若要減少每個用戶端產生的流量，訊息迴圈可以實作佇列，並送出訊息不超過經常以固定費率 （也就是特定數目的訊息將會傳送到每秒中，是否在該時間中有訊息terval 傳送)。</span><span class="sxs-lookup"><span data-stu-id="e0acc-127">To reduce the amount of traffic that each client generates, a message loop can be implemented that queues and sends out messages no more frequently than a fixed rate (that is, up to a certain number of messages will be sent every second, if there are messages in that time interval to be sent).</span></span> <span data-ttu-id="e0acc-128">節流的訊息，以特定的費率 （從用戶端和伺服器） 的範例應用程式，請參閱[高頻率即時與 SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md)。</span><span class="sxs-lookup"><span data-stu-id="e0acc-128">For a sample application that throttles messages to a certain rate (from both client and server), see [High-Frequency Realtime with SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span></span>

### <a name="reducing-message-size"></a><span data-ttu-id="e0acc-129">減少訊息大小</span><span class="sxs-lookup"><span data-stu-id="e0acc-129">Reducing message size</span></span>

<span data-ttu-id="e0acc-130">您可以藉由減少您序列化物件的大小減少 SignalR 訊息的大小。</span><span class="sxs-lookup"><span data-stu-id="e0acc-130">You can reduce the size of a SignalR message by reducing the size of your serialized objects.</span></span> <span data-ttu-id="e0acc-131">在伺服器程式碼，如果您要傳送的物件，其中包含要傳送不需要的屬性，會阻止這些屬性序列化使用`JsonIgnore`屬性。</span><span class="sxs-lookup"><span data-stu-id="e0acc-131">In server code, if you're sending an object that contains properties that don't need to be transmitted, prevent those properties from being serialized by using the `JsonIgnore` attribute.</span></span> <span data-ttu-id="e0acc-132">屬性名稱也會儲存在訊息;屬性名稱可以使用縮短`JsonProperty`屬性。</span><span class="sxs-lookup"><span data-stu-id="e0acc-132">The names of properties are also stored in the message; the names of properties can be shortened using the `JsonProperty` attribute.</span></span> <span data-ttu-id="e0acc-133">下列程式碼範例示範如何排除屬性傳送至用戶端，以及如何縮短屬性名稱：</span><span class="sxs-lookup"><span data-stu-id="e0acc-133">The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:</span></span>

<span data-ttu-id="e0acc-134">**.NET 伺服器程式碼，示範 JsonIgnore 屬性，若要排除的資料傳送至用戶端，以及減少訊息大小，JsonProperty 屬性**</span><span class="sxs-lookup"><span data-stu-id="e0acc-134">**.NET server code that demonstrates the JsonIgnore attribute to exclude data from being sent to the client, and the JsonProperty attribute to reduce message size**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

<span data-ttu-id="e0acc-135">若要保留可讀性 / 用戶端程式碼中的可維護性，在收到訊息之後的縮寫的屬性名稱可能會以人類易重新對應的名稱。</span><span class="sxs-lookup"><span data-stu-id="e0acc-135">In order to retain readability/ maintainability in the client code, the abbreviated property names can be remapped to human-friendly names after the message is received.</span></span> <span data-ttu-id="e0acc-136">下列程式碼範例示範一個可能方式重新對應的簡短的名稱，以較長的藉由定義訊息合約 （對應），並使用`reMap`来套用的最佳化的訊息類別合約函式：</span><span class="sxs-lookup"><span data-stu-id="e0acc-136">The following code sample demonstrates one possible way of remapping shortened names to longer ones, by defining a message contract (mapping), and using the `reMap` function to apply the contract to the optimized message class:</span></span>

<span data-ttu-id="e0acc-137">**重新對應的用戶端 JavaScript 程式碼已縮短人類看得懂的名稱的屬性名稱**</span><span class="sxs-lookup"><span data-stu-id="e0acc-137">**Client-side JavaScript code that remaps shortened property names to human-readable names**</span></span>

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

<span data-ttu-id="e0acc-138">名稱可以縮短在用戶端的訊息至伺服器，使用相同的方法。</span><span class="sxs-lookup"><span data-stu-id="e0acc-138">Names can be shortened in messages from the client to the server as well, using the same method.</span></span>

<span data-ttu-id="e0acc-139">減少記憶體使用量 （也就是訊息所使用的記憶體數量） 訊息的物件也可以改善效能。</span><span class="sxs-lookup"><span data-stu-id="e0acc-139">Reducing the memory footprint (that is, the amount of memory used for the message) of the message object can also improve performance.</span></span> <span data-ttu-id="e0acc-140">例如，如果的全套`int`不需要`short`或`byte`可以改為使用。</span><span class="sxs-lookup"><span data-stu-id="e0acc-140">For example, if the full range of an `int` is not needed, a `short` or `byte` can be used instead.</span></span>

<span data-ttu-id="e0acc-141">由於訊息會儲存在伺服器記憶體中，進而減少訊息大小的訊息匯流排也可以處理伺服器的記憶體問題。</span><span class="sxs-lookup"><span data-stu-id="e0acc-141">Since messages are stored in the message bus in server memory, reducing the size of messages can also address server memory issues.</span></span>

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a><span data-ttu-id="e0acc-142">微調 SignalR 伺服器效能</span><span class="sxs-lookup"><span data-stu-id="e0acc-142">Tuning your SignalR server for performance</span></span>

<span data-ttu-id="e0acc-143">下列組態設定可用來微調您的伺服器中的 SignalR 應用程式更佳的效能。</span><span class="sxs-lookup"><span data-stu-id="e0acc-143">The following configuration settings can be used to tune your server for better performance in a SignalR application.</span></span> <span data-ttu-id="e0acc-144">如需如何在 ASP.NET 應用程式中改善效能的一般資訊，請參閱[提升 ASP.NET 效能](https://msdn.microsoft.com/library/ff647787.aspx)。</span><span class="sxs-lookup"><span data-stu-id="e0acc-144">For general information on how to improve performance in an ASP.NET application, see [Improving ASP.NET Performance](https://msdn.microsoft.com/library/ff647787.aspx).</span></span>

<span data-ttu-id="e0acc-145">**SignalR 組態設定**</span><span class="sxs-lookup"><span data-stu-id="e0acc-145">**SignalR configuration settings**</span></span>

- <span data-ttu-id="e0acc-146">**DefaultMessageBufferSize**： 依預設，SignalR 會保留在記憶體中每個連線的中樞每 1000 則訊息。</span><span class="sxs-lookup"><span data-stu-id="e0acc-146">**DefaultMessageBufferSize**: By default, SignalR retains 1000 messages in memory per hub per connection.</span></span> <span data-ttu-id="e0acc-147">如果使用大型訊息時，這可能會產生記憶體問題可以降低此值可以緩解這些問題。</span><span class="sxs-lookup"><span data-stu-id="e0acc-147">If large messages are being used, this may create memory issues which can be alleviated by reducing this value.</span></span> <span data-ttu-id="e0acc-148">這項設定可以在中設定`Application_Start`事件處理常式在 ASP.NET 應用程式，或`Configuration`的 OWIN 啟動類別中的自我裝載的應用程式的方法。</span><span class="sxs-lookup"><span data-stu-id="e0acc-148">This setting can be set in the `Application_Start` event handler in an ASP.NET application, or in the `Configuration` method of an OWIN startup class in a self-hosted application.</span></span> <span data-ttu-id="e0acc-149">下列範例示範如何減少此值，以降低您的應用程式，以降低伺服器使用的記憶體數量的記憶體使用量：</span><span class="sxs-lookup"><span data-stu-id="e0acc-149">The following sample demonstrates how to reduce this value in order to reduce the memory footprint of your application in order to reduce the amount of server memory used:</span></span>

    <span data-ttu-id="e0acc-150">**.NET 伺服器程式碼在 Startup.cs 中的減少預設的訊息緩衝區大小**</span><span class="sxs-lookup"><span data-stu-id="e0acc-150">**.NET server code in Startup.cs for decreasing default message buffer size**</span></span>

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

<span data-ttu-id="e0acc-151">**IIS 組態設定**</span><span class="sxs-lookup"><span data-stu-id="e0acc-151">**IIS configuration settings**</span></span>

- <span data-ttu-id="e0acc-152">**每個應用程式的最大並行要求**： 增加並行的 IIS 要求將會增加可用的服務要求的伺服器資源。</span><span class="sxs-lookup"><span data-stu-id="e0acc-152">**Max concurrent requests per application**: Increasing the number of concurrent IIS requests will increase server resources available for serving requests.</span></span> <span data-ttu-id="e0acc-153">預設值為 5000。若要增加此設定，請在提升權限的命令提示字元執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="e0acc-153">The default value is 5000; to increase this setting, execute the following commands in an elevated command prompt:</span></span>

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]
- <span data-ttu-id="e0acc-154">**ApplicationPool QueueLength**： 這是 Http.sys 的應用程式集區排入佇列的要求數目上限。</span><span class="sxs-lookup"><span data-stu-id="e0acc-154">**ApplicationPool QueueLength**: This is the maximum number of requests that Http.sys queues for the application pool.</span></span> <span data-ttu-id="e0acc-155">當佇列已滿時，新的要求就會收到 503 「 服務無法使用 」 回應。</span><span class="sxs-lookup"><span data-stu-id="e0acc-155">When the queue is full, new requests receive a 503 "Service Unavailable" response.</span></span> <span data-ttu-id="e0acc-156">預設值為 1000。</span><span class="sxs-lookup"><span data-stu-id="e0acc-156">The default value is 1000.</span></span>

    <span data-ttu-id="e0acc-157">縮短工作者處理序中裝載您的應用程式的應用程式集區佇列長度，將會節省記憶體資源。</span><span class="sxs-lookup"><span data-stu-id="e0acc-157">Shortening the queue length for the worker process in the application pool hosting your application will conserve memory resources.</span></span> <span data-ttu-id="e0acc-158">如需詳細資訊，請參閱 <<c0> [ 管理、 調整，並設定應用程式集區](https://technet.microsoft.com/library/cc745955.aspx)。</span><span class="sxs-lookup"><span data-stu-id="e0acc-158">For more information, see [Managing, Tuning, and Configuring Application Pools](https://technet.microsoft.com/library/cc745955.aspx).</span></span>

<span data-ttu-id="e0acc-159">**ASP.NET 組態設定**</span><span class="sxs-lookup"><span data-stu-id="e0acc-159">**ASP.NET configuration settings**</span></span>

<span data-ttu-id="e0acc-160">本節包含可在設定的組態設定`aspnet.config`檔案。</span><span class="sxs-lookup"><span data-stu-id="e0acc-160">This section includes configuration settings that can be set in the `aspnet.config` file.</span></span> <span data-ttu-id="e0acc-161">這個檔案位於其中一個位置，根據平台：</span><span class="sxs-lookup"><span data-stu-id="e0acc-161">This file is found in one of two locations, depending on platform:</span></span>

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

<span data-ttu-id="e0acc-162">可能會改善 SignalR 效能的 ASP.NET 設定包括下列各項：</span><span class="sxs-lookup"><span data-stu-id="e0acc-162">ASP.NET settings that may improve SignalR performance include the following:</span></span>

- <span data-ttu-id="e0acc-163">**每一 CPU 的最大並行要求**： 提高此設定可能會解決效能瓶頸。</span><span class="sxs-lookup"><span data-stu-id="e0acc-163">**Maximum concurrent requests per CPU**: Increasing this setting may alleviate performance bottlenecks.</span></span> <span data-ttu-id="e0acc-164">若要增加此設定，新增下列組態設定加入`aspnet.config`檔案：</span><span class="sxs-lookup"><span data-stu-id="e0acc-164">To increase this setting, add the following configuration setting to the `aspnet.config` file:</span></span>

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- <span data-ttu-id="e0acc-165">**要求佇列限制**： 當連線總數超過`maxConcurrentRequestsPerCPU`設定時，ASP.NET 會開始節流使用佇列的要求。</span><span class="sxs-lookup"><span data-stu-id="e0acc-165">**Request Queue Limit**: When the total number of connections exceeds the `maxConcurrentRequestsPerCPU` setting, ASP.NET will start throttling requests using a queue.</span></span> <span data-ttu-id="e0acc-166">若要增加佇列的大小，您可以增加`requestQueueLimit`設定。</span><span class="sxs-lookup"><span data-stu-id="e0acc-166">To increase the size of the queue, you can increase the `requestQueueLimit` setting.</span></span> <span data-ttu-id="e0acc-167">若要這樣做，請新增下列組態設定加入`processModel`中的節點`config/machine.config`(而非`aspnet.config`):</span><span class="sxs-lookup"><span data-stu-id="e0acc-167">To do this, add the following configuration setting to the `processModel` node in `config/machine.config` (rather than `aspnet.config`):</span></span>

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a><span data-ttu-id="e0acc-168">針對效能問題進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="e0acc-168">Troubleshooting performance issues</span></span>

<span data-ttu-id="e0acc-169">本章節描述如何在您的應用程式中，找出效能瓶頸。</span><span class="sxs-lookup"><span data-stu-id="e0acc-169">This section describes ways to find performance bottlenecks in your application.</span></span>

### <a name="verifying-that-websocket-is-being-used"></a><span data-ttu-id="e0acc-170">驗證正在使用 WebSocket</span><span class="sxs-lookup"><span data-stu-id="e0acc-170">Verifying that WebSocket is being used</span></span>

<span data-ttu-id="e0acc-171">雖然 SignalR 可以使用各種傳輸用戶端與伺服器之間的通訊，WebSocket 提供顯著的優勢，並應在用戶端和伺服器支援它。</span><span class="sxs-lookup"><span data-stu-id="e0acc-171">While SignalR can use a variety of transports for communication between client and server, WebSocket offers a significant performance advantage, and should be used if the client and server support it.</span></span> <span data-ttu-id="e0acc-172">若要判斷您的用戶端和伺服器是否符合 WebSocket 的需求，請參閱[傳輸和後援](../getting-started/introduction-to-signalr.md#transports)。</span><span class="sxs-lookup"><span data-stu-id="e0acc-172">To determine if your client and server meet the requirements for WebSocket, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="e0acc-173">若要判斷何種傳輸正用於您的應用程式，您可以使用瀏覽器的開發人員工具，，並檢查記錄，以查看所使用的傳輸用於連接。</span><span class="sxs-lookup"><span data-stu-id="e0acc-173">To determine what transport is being used in your application, you can use the browser developer tools, and examine the logs to see what transport is being used for the connection.</span></span> <span data-ttu-id="e0acc-174">如需使用瀏覽器的開發工具，在 Internet Explorer 和 Chrome 中的資訊，請參閱[傳輸和後援](../getting-started/introduction-to-signalr.md#transports)。</span><span class="sxs-lookup"><span data-stu-id="e0acc-174">For information on using the browser development tools in Internet Explorer and Chrome, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a><span data-ttu-id="e0acc-175">使用 SignalR 效能計數器</span><span class="sxs-lookup"><span data-stu-id="e0acc-175">Using SignalR performance counters</span></span>

<span data-ttu-id="e0acc-176">本節說明如何啟用和使用 SignalR 效能計數器、 位於`Microsoft.AspNet.SignalR.Utils`封裝。</span><span class="sxs-lookup"><span data-stu-id="e0acc-176">This section describes how to enable and use SignalR performance counters, found in the `Microsoft.AspNet.SignalR.Utils` package.</span></span>

### <a name="installing-signalrexe"></a><span data-ttu-id="e0acc-177">安裝 signalr.exe</span><span class="sxs-lookup"><span data-stu-id="e0acc-177">Installing signalr.exe</span></span>

<span data-ttu-id="e0acc-178">效能計數器可以加入至使用稱為 SignalR.exe 的公用程式的伺服器。</span><span class="sxs-lookup"><span data-stu-id="e0acc-178">Performance counters can be added to the server using a utility called SignalR.exe.</span></span> <span data-ttu-id="e0acc-179">若要安裝此公用程式，請遵循下列步驟：</span><span class="sxs-lookup"><span data-stu-id="e0acc-179">To install this utility, follow these steps:</span></span>

1. <span data-ttu-id="e0acc-180">在 Visual Studio 中，選取**工具** > **NuGet 套件管理員** > **管理方案的 NuGet 套件**</span><span class="sxs-lookup"><span data-stu-id="e0acc-180">In Visual Studio, select **Tools** > **NuGet Package Manager** > **Manage NuGet Packages for Solution**</span></span>
2. <span data-ttu-id="e0acc-181">搜尋**signalr.utils**，並選取 [安裝]。</span><span class="sxs-lookup"><span data-stu-id="e0acc-181">Search for **signalr.utils**, and select Install.</span></span>

    ![](signalr-performance/_static/image1.png)
3. <span data-ttu-id="e0acc-182">接受授權合約，來安裝封裝。</span><span class="sxs-lookup"><span data-stu-id="e0acc-182">Accept the license agreement to install the package.</span></span>
4. <span data-ttu-id="e0acc-183">SignalR.exe 將會安裝成`<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`。</span><span class="sxs-lookup"><span data-stu-id="e0acc-183">SignalR.exe will be installed to `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span></span>

### <a name="installing-performance-counters-with-signalrexe"></a><span data-ttu-id="e0acc-184">使用 SignalR.exe 安裝效能計數器</span><span class="sxs-lookup"><span data-stu-id="e0acc-184">Installing performance counters with SignalR.exe</span></span>

<span data-ttu-id="e0acc-185">若要安裝 SignalR 效能計數器，請在提升權限的命令提示字元使用下列參數執行 SignalR.exe:</span><span class="sxs-lookup"><span data-stu-id="e0acc-185">To install SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

<span data-ttu-id="e0acc-186">若要移除 SignalR 效能計數器，請在提升權限的命令提示字元使用下列參數執行 SignalR.exe:</span><span class="sxs-lookup"><span data-stu-id="e0acc-186">To remove SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a><span data-ttu-id="e0acc-187">SignalR 效能計數器</span><span class="sxs-lookup"><span data-stu-id="e0acc-187">SignalR Performance counters</span></span>

<span data-ttu-id="e0acc-188">公用程式封裝會安裝下列效能計數器。</span><span class="sxs-lookup"><span data-stu-id="e0acc-188">The utilities package installs the following performance counters.</span></span> <span data-ttu-id="e0acc-189">自從最後一個應用程式集區或伺服器重新啟動之後，「 總計 」 計數器會測量事件的數目。</span><span class="sxs-lookup"><span data-stu-id="e0acc-189">The "Total" counters measure the number of events since the last application pool or server restart.</span></span>

<span data-ttu-id="e0acc-190">**連線計量**</span><span class="sxs-lookup"><span data-stu-id="e0acc-190">**Connection metrics**</span></span>

<span data-ttu-id="e0acc-191">下列度量會測量所發生的連線存留期事件。</span><span class="sxs-lookup"><span data-stu-id="e0acc-191">The following metrics measure the connection lifetime events that occur.</span></span> <span data-ttu-id="e0acc-192">如需詳細資訊，請參閱 <<c0> [ 了解和處理連線存留期事件](../guide-to-the-api/handling-connection-lifetime-events.md)。</span><span class="sxs-lookup"><span data-stu-id="e0acc-192">For more information, see [Understanding and Handling Connection Lifetime Events](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

- <span data-ttu-id="e0acc-193">**連線的連線**</span><span class="sxs-lookup"><span data-stu-id="e0acc-193">**Connections Connected**</span></span>
- <span data-ttu-id="e0acc-194">**重新連線的連線**</span><span class="sxs-lookup"><span data-stu-id="e0acc-194">**Connections Reconnected**</span></span>
- <span data-ttu-id="e0acc-195">**連線已中斷連線**</span><span class="sxs-lookup"><span data-stu-id="e0acc-195">**Connections Disconnected**</span></span>
- <span data-ttu-id="e0acc-196">**目前的連線**</span><span class="sxs-lookup"><span data-stu-id="e0acc-196">**Connections Current**</span></span>

<span data-ttu-id="e0acc-197">**訊息計量**</span><span class="sxs-lookup"><span data-stu-id="e0acc-197">**Message metrics**</span></span>

<span data-ttu-id="e0acc-198">下列度量會測量產生 SignalR 的訊息流量。</span><span class="sxs-lookup"><span data-stu-id="e0acc-198">The following metrics measure the message traffic generated by SignalR.</span></span>

- <span data-ttu-id="e0acc-199">**已接收連線的郵件總數**</span><span class="sxs-lookup"><span data-stu-id="e0acc-199">**Connection Messages Received Total**</span></span>
- <span data-ttu-id="e0acc-200">**連接訊息已傳送總數**</span><span class="sxs-lookup"><span data-stu-id="e0acc-200">**Connection Messages Sent Total**</span></span>
- <span data-ttu-id="e0acc-201">**連接訊息 Received/Sec**</span><span class="sxs-lookup"><span data-stu-id="e0acc-201">**Connection Messages Received/Sec**</span></span>
- <span data-ttu-id="e0acc-202">**連線的訊息傳送/秒**</span><span class="sxs-lookup"><span data-stu-id="e0acc-202">**Connection Messages Sent/Sec**</span></span>

<span data-ttu-id="e0acc-203">**訊息匯流排計量**</span><span class="sxs-lookup"><span data-stu-id="e0acc-203">**Message bus metrics**</span></span>

<span data-ttu-id="e0acc-204">下列度量會測量流量通過內部 SignalR 訊息匯流排，位於所有傳入和傳出的 SignalR 訊息的佇列。</span><span class="sxs-lookup"><span data-stu-id="e0acc-204">The following metrics measure traffic through the internal SignalR message bus, the queue in which all incoming and outgoing SignalR messages are placed.</span></span> <span data-ttu-id="e0acc-205">訊息**已發佈**當它是傳送或廣播。</span><span class="sxs-lookup"><span data-stu-id="e0acc-205">A message is **Published** when it is sent or broadcast.</span></span> <span data-ttu-id="e0acc-206">A**訂閱者**在此內容是在訊息匯流排上的訂用帳戶，這應該符合用戶端，再加上伺服器本身的數目。</span><span class="sxs-lookup"><span data-stu-id="e0acc-206">A **Subscriber** in this context is a subscription on the message bus; this should equal the number of clients plus the server itself.</span></span> <span data-ttu-id="e0acc-207">**配置的背景工作角色**是一種元件，將資料傳送至作用中連線;**忙碌的背景工作角色**是指是否主動傳送一則訊息。</span><span class="sxs-lookup"><span data-stu-id="e0acc-207">An **Allocated Worker** is a component that sends data to active connections; a **Busy Worker** is one that is actively sending a message.</span></span>

- <span data-ttu-id="e0acc-208">**訊息匯流排的訊息接收的總計**</span><span class="sxs-lookup"><span data-stu-id="e0acc-208">**Message Bus Messages Received Total**</span></span>
- <span data-ttu-id="e0acc-209">**訊息匯流排的訊息數/秒**</span><span class="sxs-lookup"><span data-stu-id="e0acc-209">**Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="e0acc-210">**已發佈的訊息匯流排訊息總計**</span><span class="sxs-lookup"><span data-stu-id="e0acc-210">**Message Bus Messages Published Total**</span></span>
- <span data-ttu-id="e0acc-211">**訊息匯流排郵件發佈數/秒**</span><span class="sxs-lookup"><span data-stu-id="e0acc-211">**Message Bus Messages Published/Sec**</span></span>
- <span data-ttu-id="e0acc-212">**目前的訊息匯流排的訂閱者**</span><span class="sxs-lookup"><span data-stu-id="e0acc-212">**Message Bus Subscribers Current**</span></span>
- <span data-ttu-id="e0acc-213">**訊息匯流排的訂閱者總數**</span><span class="sxs-lookup"><span data-stu-id="e0acc-213">**Message Bus Subscribers Total**</span></span>
- <span data-ttu-id="e0acc-214">**訊息匯流排的訂閱者/Sec**</span><span class="sxs-lookup"><span data-stu-id="e0acc-214">**Message Bus Subscribers/Sec**</span></span>
- <span data-ttu-id="e0acc-215">**訊息匯流排配置的背景工作角色**</span><span class="sxs-lookup"><span data-stu-id="e0acc-215">**Message Bus Allocated Workers**</span></span>
- <span data-ttu-id="e0acc-216">**訊息匯流排忙碌的背景工作角色**</span><span class="sxs-lookup"><span data-stu-id="e0acc-216">**Message Bus Busy Workers**</span></span>
- <span data-ttu-id="e0acc-217">**目前的訊息匯流排的主題**</span><span class="sxs-lookup"><span data-stu-id="e0acc-217">**Message Bus Topics Current**</span></span>

<span data-ttu-id="e0acc-218">**誤差度量**</span><span class="sxs-lookup"><span data-stu-id="e0acc-218">**Error metrics**</span></span>

<span data-ttu-id="e0acc-219">下列度量會測量 SignalR 訊息傳輸所產生的錯誤。</span><span class="sxs-lookup"><span data-stu-id="e0acc-219">The following metrics measure errors generated by SignalR message traffic.</span></span> <span data-ttu-id="e0acc-220">**中樞解析**中樞或中樞方法無法解析時，會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="e0acc-220">**Hub Resolution** errors occur when a hub or hub method cannot be resolved.</span></span> <span data-ttu-id="e0acc-221">**中樞叫用**錯誤是在叫用中樞方法時擲回的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="e0acc-221">**Hub Invocation** errors are exceptions thrown while invoking a hub method.</span></span> <span data-ttu-id="e0acc-222">**傳輸**錯誤是在 HTTP 要求或回應期間擲回的連線錯誤。</span><span class="sxs-lookup"><span data-stu-id="e0acc-222">**Transport** errors are connection errors thrown during an HTTP request or response.</span></span>

- <span data-ttu-id="e0acc-223">**錯誤： 所有的總數**</span><span class="sxs-lookup"><span data-stu-id="e0acc-223">**Errors: All Total**</span></span>
- <span data-ttu-id="e0acc-224">**錯誤： All/Sec**</span><span class="sxs-lookup"><span data-stu-id="e0acc-224">**Errors: All/Sec**</span></span>
- <span data-ttu-id="e0acc-225">**錯誤： 中樞解析總數**</span><span class="sxs-lookup"><span data-stu-id="e0acc-225">**Errors: Hub Resolution Total**</span></span>
- <span data-ttu-id="e0acc-226">**錯誤： 中樞解析數/秒**</span><span class="sxs-lookup"><span data-stu-id="e0acc-226">**Errors: Hub Resolution/Sec**</span></span>
- <span data-ttu-id="e0acc-227">**錯誤： 中樞叫用總數**</span><span class="sxs-lookup"><span data-stu-id="e0acc-227">**Errors: Hub Invocation Total**</span></span>
- <span data-ttu-id="e0acc-228">**錯誤： 中樞叫用數/秒**</span><span class="sxs-lookup"><span data-stu-id="e0acc-228">**Errors: Hub Invocation/Sec**</span></span>
- <span data-ttu-id="e0acc-229">**錯誤： 傳輸總數**</span><span class="sxs-lookup"><span data-stu-id="e0acc-229">**Errors: Transport Total**</span></span>
- <span data-ttu-id="e0acc-230">**錯誤： 傳輸數/秒**</span><span class="sxs-lookup"><span data-stu-id="e0acc-230">**Errors: Transport/Sec**</span></span>

<a id="scaleout_metrics"></a>

<span data-ttu-id="e0acc-231">**向外延展計量**</span><span class="sxs-lookup"><span data-stu-id="e0acc-231">**Scaleout metrics**</span></span>

<span data-ttu-id="e0acc-232">下列度量會測量流量和向外延展提供者所產生的錯誤。</span><span class="sxs-lookup"><span data-stu-id="e0acc-232">The following metrics measure traffic and errors generated by the scaleout provider.</span></span> <span data-ttu-id="e0acc-233">A **Stream**在此內容中向外延展提供者所使用的縮放單位; 這是資料表，如果使用 SQL Server、 使用服務匯流排時，如果某個主題和訂用帳戶如果使用 Redis。</span><span class="sxs-lookup"><span data-stu-id="e0acc-233">A **Stream** in this context is a scale unit used by the scaleout provider; this is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used.</span></span> <span data-ttu-id="e0acc-234">每個資料流可確保已排序的讀取和寫入作業;單一資料流是潛在擴展瓶頸，因此可以增加的資料流數目，以減少該瓶頸。</span><span class="sxs-lookup"><span data-stu-id="e0acc-234">Each stream ensures ordered read and write operations; a single stream is a potential scale bottleneck, so the number of streams can be increased to help reduce that bottleneck.</span></span> <span data-ttu-id="e0acc-235">如果使用多個資料流，SignalR 會自動分散方式，從指定的任何連線傳送訊息的順序可確保這些資料流 （分區） 訊息。</span><span class="sxs-lookup"><span data-stu-id="e0acc-235">If multiple streams are used, SignalR will automatically distribute (shard) messages across these streams in a way that ensures messages sent from any given connection are in order.</span></span>

<span data-ttu-id="e0acc-236">[MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx)設定可以控制維護由 SignalR 範圍外傳送佇列的長度。</span><span class="sxs-lookup"><span data-stu-id="e0acc-236">The [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) setting controls the length of the scaleout send queue maintained by SignalR.</span></span> <span data-ttu-id="e0acc-237">將它設定為值大於 0 會將傳送一次一個已設定的訊息後擋板以在傳送佇列中的所有訊息。</span><span class="sxs-lookup"><span data-stu-id="e0acc-237">Setting it to a value greater than 0 will place all messages in a send queue to be sent one at a time to the configured messaging backplane.</span></span> <span data-ttu-id="e0acc-238">如果佇列的大小超過設定的長度，後續的呼叫，將會失敗，立即[InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx)直到佇列中的訊息數目未超過設定一次。</span><span class="sxs-lookup"><span data-stu-id="e0acc-238">If the size of the queue goes above the configured length, subsequent calls to send will fail immediately with an [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) until the number of messages in the queue is less than the setting again.</span></span> <span data-ttu-id="e0acc-239">因為實作的背板通常採用自己的佇列或流量控制，所以預設會停用佇列。</span><span class="sxs-lookup"><span data-stu-id="e0acc-239">Queueing is disabled by default because the implemented backplanes generally have their own queuing or flow-control in place.</span></span> <span data-ttu-id="e0acc-240">在 SQL Server 的情況下連接共用實際上會限制將在上一次的傳送數目。</span><span class="sxs-lookup"><span data-stu-id="e0acc-240">In the case of SQL Server, connection pooling effectively limits the number of sends going on at any one time.</span></span>

<span data-ttu-id="e0acc-241">根據預設，只有一個資料流用於 SQL Server 和 Redis、 五個資料流用於服務匯流排佇列已停用，但可以變更這些設定，透過 SQL Server 和服務匯流排的組態：</span><span class="sxs-lookup"><span data-stu-id="e0acc-241">By default, only one stream is used for SQL Server and Redis, five streams are used for Service Bus, and queueing is disabled, but these settings can be changed through configuration on SQL Server and Service Bus:</span></span>

<span data-ttu-id="e0acc-242">**.NET 伺服器程式碼來設定資料表的 SQL Server 後擋板的計數和佇列長度**</span><span class="sxs-lookup"><span data-stu-id="e0acc-242">**.NET Server Code for configuring table count and queue length for SQL Server backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample9.cs)]

<span data-ttu-id="e0acc-243">**.NET 伺服器程式碼來設定服務匯流排後擋板的主題計數和佇列長度**</span><span class="sxs-lookup"><span data-stu-id="e0acc-243">**.NET Server Code for configuring topic count and queue length for Service Bus backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample10.cs)]

<span data-ttu-id="e0acc-244">A**緩衝處理**資料流是已進入錯誤的狀態; 當資料流處於錯誤狀態，所有傳送至後擋板的訊息將會失敗，資料流不會再判定為失敗之前，立即。</span><span class="sxs-lookup"><span data-stu-id="e0acc-244">A **Buffering** stream is one that has entered a faulted state; when the stream is in the faulted state, all messages sent to the backplane will fail immediately until the stream is no longer faulting.</span></span> <span data-ttu-id="e0acc-245">**傳送佇列長度**是張貼但尚未傳送的訊息數目。</span><span class="sxs-lookup"><span data-stu-id="e0acc-245">The **Send Queue Length** is the number of messages that have been posted but not yet sent.</span></span>

- <span data-ttu-id="e0acc-246">**範圍外訊息匯流排的訊息數/秒**</span><span class="sxs-lookup"><span data-stu-id="e0acc-246">**Scaleout Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="e0acc-247">**向外延展資料流總數**</span><span class="sxs-lookup"><span data-stu-id="e0acc-247">**Scaleout Streams Total**</span></span>
- <span data-ttu-id="e0acc-248">**向外延展資料流開啟**</span><span class="sxs-lookup"><span data-stu-id="e0acc-248">**Scaleout Streams Open**</span></span>
- <span data-ttu-id="e0acc-249">**向外延展資料流緩衝**</span><span class="sxs-lookup"><span data-stu-id="e0acc-249">**Scaleout Streams Buffering**</span></span>
- <span data-ttu-id="e0acc-250">**範圍外錯誤總數**</span><span class="sxs-lookup"><span data-stu-id="e0acc-250">**Scaleout Errors Total**</span></span>
- <span data-ttu-id="e0acc-251">**範圍外錯誤數/秒**</span><span class="sxs-lookup"><span data-stu-id="e0acc-251">**Scaleout Errors/Sec**</span></span>
- <span data-ttu-id="e0acc-252">**範圍外傳送佇列長度**</span><span class="sxs-lookup"><span data-stu-id="e0acc-252">**Scaleout Send Queue Length**</span></span>

<span data-ttu-id="e0acc-253">如需有關這些計數器會測量的詳細資訊，請參閱[SignalR 向外延展與 Azure 服務匯流排](scaleout-with-windows-azure-service-bus.md)。</span><span class="sxs-lookup"><span data-stu-id="e0acc-253">For more information on what these counters are measuring, see [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span></span>

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a><span data-ttu-id="e0acc-254">使用其他的效能計數器</span><span class="sxs-lookup"><span data-stu-id="e0acc-254">Using other performance counters</span></span>

<span data-ttu-id="e0acc-255">下列效能計數器也可能在監視應用程式的效能很有用。</span><span class="sxs-lookup"><span data-stu-id="e0acc-255">The following performance counters may also be useful in monitoring your application's performance.</span></span>

<span data-ttu-id="e0acc-256">**記憶體**</span><span class="sxs-lookup"><span data-stu-id="e0acc-256">**Memory**</span></span>

- <span data-ttu-id="e0acc-257">.NET CLR Memory\\中全部堆積 （w3wp) 的 # bytes</span><span class="sxs-lookup"><span data-stu-id="e0acc-257">.NET CLR Memory\\# bytes in all Heaps (for w3wp)</span></span>

<span data-ttu-id="e0acc-258">**ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="e0acc-258">**ASP.NET**</span></span>

- <span data-ttu-id="e0acc-259">Asp.net\requests Current</span><span class="sxs-lookup"><span data-stu-id="e0acc-259">ASP.NET\Requests Current</span></span>
- <span data-ttu-id="e0acc-260">ASP.NET\Queued</span><span class="sxs-lookup"><span data-stu-id="e0acc-260">ASP.NET\Queued</span></span>
- <span data-ttu-id="e0acc-261">ASP.NET\Rejected</span><span class="sxs-lookup"><span data-stu-id="e0acc-261">ASP.NET\Rejected</span></span>

<span data-ttu-id="e0acc-262">**CPU**</span><span class="sxs-lookup"><span data-stu-id="e0acc-262">**CPU**</span></span>

- <span data-ttu-id="e0acc-263">處理器 Information\Processor 時間</span><span class="sxs-lookup"><span data-stu-id="e0acc-263">Processor Information\Processor Time</span></span>

<span data-ttu-id="e0acc-264">**TCP/IP**</span><span class="sxs-lookup"><span data-stu-id="e0acc-264">**TCP/IP**</span></span>

- <span data-ttu-id="e0acc-265">Tcpv6-已/建立的連接</span><span class="sxs-lookup"><span data-stu-id="e0acc-265">TCPv6/Connections Established</span></span>
- <span data-ttu-id="e0acc-266">TCPv4/建立的連接</span><span class="sxs-lookup"><span data-stu-id="e0acc-266">TCPv4/Connections Established</span></span>

<span data-ttu-id="e0acc-267">**Web 服務**</span><span class="sxs-lookup"><span data-stu-id="e0acc-267">**Web Service**</span></span>

- <span data-ttu-id="e0acc-268">Web Service\Current Connections</span><span class="sxs-lookup"><span data-stu-id="e0acc-268">Web Service\Current Connections</span></span>
- <span data-ttu-id="e0acc-269">Web Service\Maximum 連線</span><span class="sxs-lookup"><span data-stu-id="e0acc-269">Web Service\Maximum Connections</span></span>

<span data-ttu-id="e0acc-270">**執行緒處理**</span><span class="sxs-lookup"><span data-stu-id="e0acc-270">**Threading**</span></span>

- <span data-ttu-id="e0acc-271">.NET CLR 鎖定和執行緒\\# 個目前的邏輯執行緒</span><span class="sxs-lookup"><span data-stu-id="e0acc-271">.NET CLR Locks And Threads\\# of current logical Threads</span></span>
- <span data-ttu-id="e0acc-272">.NET CLR 鎖定和執行緒\\# 個目前的實體執行緒</span><span class="sxs-lookup"><span data-stu-id="e0acc-272">.NET CLR Locks And Threads\\# of current physical Threads</span></span>

<a id="otherresources"></a>

## <a name="other-resources"></a><span data-ttu-id="e0acc-273">其他資源</span><span class="sxs-lookup"><span data-stu-id="e0acc-273">Other Resources</span></span>

<span data-ttu-id="e0acc-274">如需有關 ASP.NET 的效能監視和調整的詳細資訊，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="e0acc-274">For more information on ASP.NET performance monitoring and tuning, see the following topics:</span></span>

- <span data-ttu-id="e0acc-275">[ASP.NET 效能概觀](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="e0acc-275">[ASP.NET Performance Overview](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span></span>
- [<span data-ttu-id="e0acc-276">IIS 7.5，IIS 7.0 和 IIS 6.0 的 ASP.NET 執行緒用法</span><span class="sxs-lookup"><span data-stu-id="e0acc-276">ASP.NET Thread Usage on IIS 7.5, IIS 7.0, and IIS 6.0</span></span>](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [<span data-ttu-id="e0acc-277">&lt;applicationPool&gt;項目 （Web 設定）</span><span class="sxs-lookup"><span data-stu-id="e0acc-277">&lt;applicationPool&gt; Element (Web Settings)</span></span>](https://msdn.microsoft.com/library/dd560842.aspx)
