---
uid: signalr/overview/performance/signalr-performance
title: SignalR 效能 |Microsoft 文件
author: pfletcher
description: SignalR 效能
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 3751f5e7-59db-4be0-a290-50abc24e5c84
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: 4468ee8031afccca847db67bd4b5b263f0a2c5ac
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
ms.locfileid: "28041861"
---
<a name="signalr-performance"></a><span data-ttu-id="d483d-103">SignalR 效能</span><span class="sxs-lookup"><span data-stu-id="d483d-103">SignalR Performance</span></span>
====================
<span data-ttu-id="d483d-104">由[Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="d483d-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="d483d-105">本主題描述如何設計、 量值，以及改善的 SignalR 應用程式的效能。</span><span class="sxs-lookup"><span data-stu-id="d483d-105">This topic describes how to design for, measure, and improve performance in a SignalR application.</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="d483d-106">本主題中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="d483d-106">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="d483d-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="d483d-107">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="d483d-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="d483d-108">.NET 4.5</span></span>
> - <span data-ttu-id="d483d-109">SignalR 第 2 版</span><span class="sxs-lookup"><span data-stu-id="d483d-109">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="d483d-110">本主題的先前版本</span><span class="sxs-lookup"><span data-stu-id="d483d-110">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="d483d-111">如需舊版 SignalR 的資訊，請參閱[SignalR 舊版](../older-versions/index.md)。</span><span class="sxs-lookup"><span data-stu-id="d483d-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="d483d-112">問題和註解</span><span class="sxs-lookup"><span data-stu-id="d483d-112">Questions and comments</span></span>
> 
> <span data-ttu-id="d483d-113">請留下上如何您所喜歡的本教學課程，我們可以改進中將註解放在頁面底部的意見反應。</span><span class="sxs-lookup"><span data-stu-id="d483d-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="d483d-114">如果您有與本教學課程不直接相關的問題，您可以將它們來公佈[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或[StackOverflow.com](http://stackoverflow.com/)。</span><span class="sxs-lookup"><span data-stu-id="d483d-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="d483d-115">SignalR 效能和調整的最新簡報，請參閱[調整透過 ASP.NET SignalR 即時Web</](https://channel9.msdn.com/Events/Build/2013/3-502)。</span><span class="sxs-lookup"><span data-stu-id="d483d-115">For a recent presentation on SignalR performance and scaling, see [Scaling the Real-time Web with ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span></span>

<span data-ttu-id="d483d-116">此主題包括下列章節：</span><span class="sxs-lookup"><span data-stu-id="d483d-116">This topic contains the following sections:</span></span>

- [<span data-ttu-id="d483d-117">設計考量</span><span class="sxs-lookup"><span data-stu-id="d483d-117">Design considerations</span></span>](#design)
- [<span data-ttu-id="d483d-118">微調您 SignalR 的伺服器效能</span><span class="sxs-lookup"><span data-stu-id="d483d-118">Tuning your SignalR server for performance</span></span>](#tuning)
- [<span data-ttu-id="d483d-119">疑難排解效能問題</span><span class="sxs-lookup"><span data-stu-id="d483d-119">Troubleshooting performance issues</span></span>](#troubleshooting)
- [<span data-ttu-id="d483d-120">使用 SignalR 效能計數器</span><span class="sxs-lookup"><span data-stu-id="d483d-120">Using SignalR performance counters</span></span>](#perfcounters)
- [<span data-ttu-id="d483d-121">使用其他效能計數器</span><span class="sxs-lookup"><span data-stu-id="d483d-121">Using other performance counters</span></span>](#othercounters)
- [<span data-ttu-id="d483d-122">其他資源</span><span class="sxs-lookup"><span data-stu-id="d483d-122">Other resources</span></span>](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a><span data-ttu-id="d483d-123">設計考量</span><span class="sxs-lookup"><span data-stu-id="d483d-123">Design considerations</span></span>

<span data-ttu-id="d483d-124">本節說明可以實作在 SignalR 應用程式，以確保不會被效能妨礙藉由產生不必要的網路流量的設計模式。</span><span class="sxs-lookup"><span data-stu-id="d483d-124">This section describes patterns that can be implemented during the design of a SignalR application, to ensure that performance is not being hindered by generating unnecessary network traffic.</span></span>

### <a name="throttling-message-frequency"></a><span data-ttu-id="d483d-125">節流的訊息頻率</span><span class="sxs-lookup"><span data-stu-id="d483d-125">Throttling message frequency</span></span>

<span data-ttu-id="d483d-126">即使在送出訊息 （例如即時遊戲應用程式） 的高頻率的應用程式，大部分的應用程式不需要傳送第二個的多個訊息。</span><span class="sxs-lookup"><span data-stu-id="d483d-126">Even in an application that sends out messages at a high frequency (such as a realtime gaming application), most applications don't need to send more than a few messages a second.</span></span> <span data-ttu-id="d483d-127">若要減少每個用戶端會產生的流量，訊息迴圈可以實作佇列，並送出訊息不超過經常固定 （也就是最高達訊息數目不會傳送每秒中，如果沒有訊息中該時間terval 傳送)。</span><span class="sxs-lookup"><span data-stu-id="d483d-127">To reduce the amount of traffic that each client generates, a message loop can be implemented that queues and sends out messages no more frequently than a fixed rate (that is, up to a certain number of messages will be sent every second, if there are messages in that time interval to be sent).</span></span> <span data-ttu-id="d483d-128">在特定速率會調整訊息 （來自用戶端和伺服器） 的範例應用程式，請參閱[與 SignalR 的頻率高的即時](../getting-started/tutorial-high-frequency-realtime-with-signalr.md)。</span><span class="sxs-lookup"><span data-stu-id="d483d-128">For a sample application that throttles messages to a certain rate (from both client and server), see [High-Frequency Realtime with SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span></span>

### <a name="reducing-message-size"></a><span data-ttu-id="d483d-129">降低訊息大小</span><span class="sxs-lookup"><span data-stu-id="d483d-129">Reducing message size</span></span>

<span data-ttu-id="d483d-130">您可以藉由減少序列化物件的大小減少 SignalR 訊息的大小。</span><span class="sxs-lookup"><span data-stu-id="d483d-130">You can reduce the size of a SignalR message by reducing the size of your serialized objects.</span></span> <span data-ttu-id="d483d-131">在伺服端程式碼，如果您要傳送的物件，其中包含要傳送不需要的屬性，使這些屬性無法序列化使用的`JsonIgnore`屬性。</span><span class="sxs-lookup"><span data-stu-id="d483d-131">In server code, if you're sending an object that contains properties that don't need to be transmitted, prevent those properties from being serialized by using the `JsonIgnore` attribute.</span></span> <span data-ttu-id="d483d-132">屬性的名稱也會儲存在訊息。屬性名稱可以使用縮短`JsonProperty`屬性。</span><span class="sxs-lookup"><span data-stu-id="d483d-132">The names of properties are also stored in the message; the names of properties can be shortened using the `JsonProperty` attribute.</span></span> <span data-ttu-id="d483d-133">下列程式碼範例示範如何排除屬性傳送到用戶端，以及如何縮短屬性名稱：</span><span class="sxs-lookup"><span data-stu-id="d483d-133">The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:</span></span>

<span data-ttu-id="d483d-134">**.NET 伺服器程式碼，示範 JsonIgnore 屬性，若要排除的資料傳送到用戶端，以及要降低訊息大小的 JsonProperty 屬性**</span><span class="sxs-lookup"><span data-stu-id="d483d-134">**.NET server code that demonstrates the JsonIgnore attribute to exclude data from being sent to the client, and the JsonProperty attribute to reduce message size**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

<span data-ttu-id="d483d-135">若要保留可讀性 / 用戶端程式碼中的可維護性、 收到訊息之後的縮寫的屬性名稱可以是重新對應至人們易記的名稱。</span><span class="sxs-lookup"><span data-stu-id="d483d-135">In order to retain readability/ maintainability in the client code, the abbreviated property names can be remapped to human-friendly names after the message is received.</span></span> <span data-ttu-id="d483d-136">下列程式碼範例示範一個可能方式重新對應縮短的名稱長度的項目，藉由定義訊息合約 （對應），並使用`reMap`来套用的最佳化的訊息類別合約函式：</span><span class="sxs-lookup"><span data-stu-id="d483d-136">The following code sample demonstrates one possible way of remapping shortened names to longer ones, by defining a message contract (mapping), and using the `reMap` function to apply the contract to the optimized message class:</span></span>

<span data-ttu-id="d483d-137">**重新對應的用戶端 JavaScript 程式碼縮短人類看得懂的名稱屬性名稱**</span><span class="sxs-lookup"><span data-stu-id="d483d-137">**Client-side JavaScript code that remaps shortened property names to human-readable names**</span></span>

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

<span data-ttu-id="d483d-138">名稱可以縮短到伺服器，用戶端的訊息中使用相同的方法。</span><span class="sxs-lookup"><span data-stu-id="d483d-138">Names can be shortened in messages from the client to the server as well, using the same method.</span></span>

<span data-ttu-id="d483d-139">降低記憶體耗用量 （也就是訊息所使用的記憶體數量） 訊息的物件也可以改善效能。</span><span class="sxs-lookup"><span data-stu-id="d483d-139">Reducing the memory footprint (that is, the amount of memory used for the message) of the message object can also improve performance.</span></span> <span data-ttu-id="d483d-140">例如，如果完整的`int`不需要`short`或`byte`可改為使用。</span><span class="sxs-lookup"><span data-stu-id="d483d-140">For example, if the full range of an `int` is not needed, a `short` or `byte` can be used instead.</span></span>

<span data-ttu-id="d483d-141">由於訊息會儲存在伺服器記憶體中，以減少的訊息大小的訊息匯流排中也可以處理伺服器的記憶體問題。</span><span class="sxs-lookup"><span data-stu-id="d483d-141">Since messages are stored in the message bus in server memory, reducing the size of messages can also address server memory issues.</span></span>

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a><span data-ttu-id="d483d-142">微調您 SignalR 的伺服器效能</span><span class="sxs-lookup"><span data-stu-id="d483d-142">Tuning your SignalR server for performance</span></span>

<span data-ttu-id="d483d-143">下列組態設定可以用來微調您的伺服器，以提升效能的 SignalR 應用程式中。</span><span class="sxs-lookup"><span data-stu-id="d483d-143">The following configuration settings can be used to tune your server for better performance in a SignalR application.</span></span> <span data-ttu-id="d483d-144">如需如何改善 ASP.NET 應用程式效能的一般資訊，請參閱[改善 ASP.NET 效能](https://msdn.microsoft.com/library/ff647787.aspx)。</span><span class="sxs-lookup"><span data-stu-id="d483d-144">For general information on how to improve performance in an ASP.NET application, see [Improving ASP.NET Performance](https://msdn.microsoft.com/library/ff647787.aspx).</span></span>

<span data-ttu-id="d483d-145">**SignalR 的組態設定**</span><span class="sxs-lookup"><span data-stu-id="d483d-145">**SignalR configuration settings**</span></span>

- <span data-ttu-id="d483d-146">**DefaultMessageBufferSize**： 根據預設，SignalR 會保留在記憶體中每個連線的中樞每 1000年的訊息。</span><span class="sxs-lookup"><span data-stu-id="d483d-146">**DefaultMessageBufferSize**: By default, SignalR retains 1000 messages in memory per hub per connection.</span></span> <span data-ttu-id="d483d-147">如果使用大型訊息，這可能會產生可以減輕藉由減少此值的記憶體問題。</span><span class="sxs-lookup"><span data-stu-id="d483d-147">If large messages are being used, this may create memory issues which can be alleviated by reducing this value.</span></span> <span data-ttu-id="d483d-148">這項設定可以在中設定`Application_Start`或 ASP.NET 應用程式中的事件處理常式`Configuration`OWIN 啟動類別自我裝載的應用程式中的方法。</span><span class="sxs-lookup"><span data-stu-id="d483d-148">This setting can be set in the `Application_Start` event handler in an ASP.NET application, or in the `Configuration` method of an OWIN startup class in a self-hosted application.</span></span> <span data-ttu-id="d483d-149">下列範例會示範如何以降低您的應用程式，以降低伺服器使用的記憶體數量的記憶體使用量降低此值：</span><span class="sxs-lookup"><span data-stu-id="d483d-149">The following sample demonstrates how to reduce this value in order to reduce the memory footprint of your application in order to reduce the amount of server memory used:</span></span>

    <span data-ttu-id="d483d-150">**伺服器的.NET 程式碼 Startup.cs 中減少預設的訊息緩衝區大小**</span><span class="sxs-lookup"><span data-stu-id="d483d-150">**.NET server code in Startup.cs for decreasing default message buffer size**</span></span>

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

<span data-ttu-id="d483d-151">**IIS 組態設定**</span><span class="sxs-lookup"><span data-stu-id="d483d-151">**IIS configuration settings**</span></span>

- <span data-ttu-id="d483d-152">**每個應用程式的最大並行要求**： 增加並行的 IIS 要求將會增加伺服器資源可用來服務要求。</span><span class="sxs-lookup"><span data-stu-id="d483d-152">**Max concurrent requests per application**: Increasing the number of concurrent IIS requests will increase server resources available for serving requests.</span></span> <span data-ttu-id="d483d-153">預設值為 5000。若要增加此設定，請在提升權限的命令提示字元中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="d483d-153">The default value is 5000; to increase this setting, execute the following commands in an elevated command prompt:</span></span>

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]
- <span data-ttu-id="d483d-154">**應用程式集區 QueueLength**： 這是 Http.sys 應用程式集區排入佇列的要求數目上限。</span><span class="sxs-lookup"><span data-stu-id="d483d-154">**ApplicationPool QueueLength**: This is the maximum number of requests that Http.sys queues for the application pool.</span></span> <span data-ttu-id="d483d-155">佇列已滿時，新的要求就會收到 503 「 服務無法使用 」 回應。</span><span class="sxs-lookup"><span data-stu-id="d483d-155">When the queue is full, new requests receive a 503 "Service Unavailable" response.</span></span> <span data-ttu-id="d483d-156">預設值為 1000。</span><span class="sxs-lookup"><span data-stu-id="d483d-156">The default value is 1000.</span></span>

    <span data-ttu-id="d483d-157">縮短中裝載您的應用程式的應用程式集區的工作者處理序的佇列長度，將會節省記憶體資源。</span><span class="sxs-lookup"><span data-stu-id="d483d-157">Shortening the queue length for the worker process in the application pool hosting your application will conserve memory resources.</span></span> <span data-ttu-id="d483d-158">如需詳細資訊，請參閱[管理、 調整及設定應用程式集區](https://technet.microsoft.com/library/cc745955.aspx)。</span><span class="sxs-lookup"><span data-stu-id="d483d-158">For more information, see [Managing, Tuning, and Configuring Application Pools](https://technet.microsoft.com/library/cc745955.aspx).</span></span>

<span data-ttu-id="d483d-159">**ASP.NET 組態設定**</span><span class="sxs-lookup"><span data-stu-id="d483d-159">**ASP.NET configuration settings**</span></span>

<span data-ttu-id="d483d-160">本節包含可以在中設定的組態設定`aspnet.config`檔案。</span><span class="sxs-lookup"><span data-stu-id="d483d-160">This section includes configuration settings that can be set in the `aspnet.config` file.</span></span> <span data-ttu-id="d483d-161">找到這個檔案中的兩個位置，根據平台：</span><span class="sxs-lookup"><span data-stu-id="d483d-161">This file is found in one of two locations, depending on platform:</span></span>

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

<span data-ttu-id="d483d-162">可能會改善效能 SignalR 的 ASP.NET 設定包括下列各項：</span><span class="sxs-lookup"><span data-stu-id="d483d-162">ASP.NET settings that may improve SignalR performance include the following:</span></span>

- <span data-ttu-id="d483d-163">**每一 CPU 的最大並行要求**： 增加此設定可能會解決效能瓶頸。</span><span class="sxs-lookup"><span data-stu-id="d483d-163">**Maximum concurrent requests per CPU**: Increasing this setting may alleviate performance bottlenecks.</span></span> <span data-ttu-id="d483d-164">若要增加此設定，加入下列組態設定加入`aspnet.config`檔案：</span><span class="sxs-lookup"><span data-stu-id="d483d-164">To increase this setting, add the following configuration setting to the `aspnet.config` file:</span></span>

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- <span data-ttu-id="d483d-165">**要求佇列限制**： 當連線總數超過`maxConcurrentRequestsPerCPU`設定，ASP.NET 將開始節流使用佇列的要求。</span><span class="sxs-lookup"><span data-stu-id="d483d-165">**Request Queue Limit**: When the total number of connections exceeds the `maxConcurrentRequestsPerCPU` setting, ASP.NET will start throttling requests using a queue.</span></span> <span data-ttu-id="d483d-166">若要增加佇列的大小，您可以增加`requestQueueLimit`設定。</span><span class="sxs-lookup"><span data-stu-id="d483d-166">To increase the size of the queue, you can increase the `requestQueueLimit` setting.</span></span> <span data-ttu-id="d483d-167">若要這樣做，請將加入下列組態設定加入`processModel`節點`config/machine.config`(而非`aspnet.config`):</span><span class="sxs-lookup"><span data-stu-id="d483d-167">To do this, add the following configuration setting to the `processModel` node in `config/machine.config` (rather than `aspnet.config`):</span></span>

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a><span data-ttu-id="d483d-168">疑難排解效能問題</span><span class="sxs-lookup"><span data-stu-id="d483d-168">Troubleshooting performance issues</span></span>

<span data-ttu-id="d483d-169">本章節描述如何在應用程式中，找出效能瓶頸。</span><span class="sxs-lookup"><span data-stu-id="d483d-169">This section describes ways to find performance bottlenecks in your application.</span></span>

### <a name="verifying-that-websocket-is-being-used"></a><span data-ttu-id="d483d-170">確認正在使用 WebSocket</span><span class="sxs-lookup"><span data-stu-id="d483d-170">Verifying that WebSocket is being used</span></span>

<span data-ttu-id="d483d-171">雖然 SignalR 可以使用各種傳輸用戶端和伺服器之間的通訊，WebSocket 提供顯著的效能優勢，，而且如果用戶端和伺服器支援它，則應該使用。</span><span class="sxs-lookup"><span data-stu-id="d483d-171">While SignalR can use a variety of transports for communication between client and server, WebSocket offers a significant performance advantage, and should be used if the client and server support it.</span></span> <span data-ttu-id="d483d-172">若要判斷您的用戶端和伺服器是否符合 WebSocket 的需求，請參閱[傳輸和後援](../getting-started/introduction-to-signalr.md#transports)。</span><span class="sxs-lookup"><span data-stu-id="d483d-172">To determine if your client and server meet the requirements for WebSocket, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="d483d-173">若要判斷您的應用程式中使用何種傳輸，您可以使用瀏覽器開發人員工具，並檢查記錄檔，以查看所使用的傳輸正在使用的連接。</span><span class="sxs-lookup"><span data-stu-id="d483d-173">To determine what transport is being used in your application, you can use the browser developer tools, and examine the logs to see what transport is being used for the connection.</span></span> <span data-ttu-id="d483d-174">如需使用 Internet Explorer 和 Chrome 瀏覽器開發工具的資訊，請參閱[傳輸和後援](../getting-started/introduction-to-signalr.md#transports)。</span><span class="sxs-lookup"><span data-stu-id="d483d-174">For information on using the browser development tools in Internet Explorer and Chrome, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a><span data-ttu-id="d483d-175">使用 SignalR 效能計數器</span><span class="sxs-lookup"><span data-stu-id="d483d-175">Using SignalR performance counters</span></span>

<span data-ttu-id="d483d-176">本章節描述如何啟用及使用 SignalR 效能計數器、 位於`Microsoft.AspNet.SignalR.Utils`封裝。</span><span class="sxs-lookup"><span data-stu-id="d483d-176">This section describes how to enable and use SignalR performance counters, found in the `Microsoft.AspNet.SignalR.Utils` package.</span></span>

### <a name="installing-signalrexe"></a><span data-ttu-id="d483d-177">安裝 signalr.exe</span><span class="sxs-lookup"><span data-stu-id="d483d-177">Installing signalr.exe</span></span>

<span data-ttu-id="d483d-178">效能計數器可以加入至使用稱為 SignalR.exe 的公用程式的伺服器。</span><span class="sxs-lookup"><span data-stu-id="d483d-178">Performance counters can be added to the server using a utility called SignalR.exe.</span></span> <span data-ttu-id="d483d-179">若要安裝此公用程式，請遵循下列步驟：</span><span class="sxs-lookup"><span data-stu-id="d483d-179">To install this utility, follow these steps:</span></span>

1. <span data-ttu-id="d483d-180">在 Visual Studio 應用程式中，選取**工具**，**程式庫套件管理員**，**管理方案的 NuGet 套件...**</span><span class="sxs-lookup"><span data-stu-id="d483d-180">In your Visual Studio application, select **Tools**, **Library Package Manager**, **Manage NuGet Packages for Solution...**</span></span>
2. <span data-ttu-id="d483d-181">搜尋**signalr.utils**，並選取 安裝。</span><span class="sxs-lookup"><span data-stu-id="d483d-181">Search for **signalr.utils**, and select Install.</span></span>

    ![](signalr-performance/_static/image1.png)
3. <span data-ttu-id="d483d-182">接受授權合約，若要安裝封裝。</span><span class="sxs-lookup"><span data-stu-id="d483d-182">Accept the license agreement to install the package.</span></span>
4. <span data-ttu-id="d483d-183">SignalR.exe 將會安裝成`<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`。</span><span class="sxs-lookup"><span data-stu-id="d483d-183">SignalR.exe will be installed to `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span></span>

### <a name="installing-performance-counters-with-signalrexe"></a><span data-ttu-id="d483d-184">使用 SignalR.exe 安裝效能計數器</span><span class="sxs-lookup"><span data-stu-id="d483d-184">Installing performance counters with SignalR.exe</span></span>

<span data-ttu-id="d483d-185">若要安裝 SignalR 效能計數器，請在提升權限的命令提示字元使用下列參數執行 SignalR.exe:</span><span class="sxs-lookup"><span data-stu-id="d483d-185">To install SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

<span data-ttu-id="d483d-186">若要移除 SignalR 效能計數器，請在提升權限的命令提示字元使用下列參數執行 SignalR.exe:</span><span class="sxs-lookup"><span data-stu-id="d483d-186">To remove SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a><span data-ttu-id="d483d-187">SignalR 效能計數器</span><span class="sxs-lookup"><span data-stu-id="d483d-187">SignalR Performance counters</span></span>

<span data-ttu-id="d483d-188">公用程式封裝會安裝下列效能計數器。</span><span class="sxs-lookup"><span data-stu-id="d483d-188">The utilities package installs the following performance counters.</span></span> <span data-ttu-id="d483d-189">自從最後一個應用程式集區或伺服器重新啟動之後，「 總計 」 計數器會測量事件的數目。</span><span class="sxs-lookup"><span data-stu-id="d483d-189">The "Total" counters measure the number of events since the last application pool or server restart.</span></span>

<span data-ttu-id="d483d-190">**連接度量**</span><span class="sxs-lookup"><span data-stu-id="d483d-190">**Connection metrics**</span></span>

<span data-ttu-id="d483d-191">下列度量會測量所發生的連線存留期事件。</span><span class="sxs-lookup"><span data-stu-id="d483d-191">The following metrics measure the connection lifetime events that occur.</span></span> <span data-ttu-id="d483d-192">如需詳細資訊，請參閱[了解和處理連線存留期事件](../guide-to-the-api/handling-connection-lifetime-events.md)。</span><span class="sxs-lookup"><span data-stu-id="d483d-192">For more information, see [Understanding and Handling Connection Lifetime Events](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

- <span data-ttu-id="d483d-193">**連線的連線**</span><span class="sxs-lookup"><span data-stu-id="d483d-193">**Connections Connected**</span></span>
- <span data-ttu-id="d483d-194">**重新連線的連線**</span><span class="sxs-lookup"><span data-stu-id="d483d-194">**Connections Reconnected**</span></span>
- <span data-ttu-id="d483d-195">**連線已中斷連線**</span><span class="sxs-lookup"><span data-stu-id="d483d-195">**Connections Disconnected**</span></span>
- <span data-ttu-id="d483d-196">**目前的連線**</span><span class="sxs-lookup"><span data-stu-id="d483d-196">**Connections Current**</span></span>

<span data-ttu-id="d483d-197">**訊息計量**</span><span class="sxs-lookup"><span data-stu-id="d483d-197">**Message metrics**</span></span>

<span data-ttu-id="d483d-198">下列度量會測量 SignalR 所產生的訊息流量。</span><span class="sxs-lookup"><span data-stu-id="d483d-198">The following metrics measure the message traffic generated by SignalR.</span></span>

- <span data-ttu-id="d483d-199">**已接收連線郵件總數**</span><span class="sxs-lookup"><span data-stu-id="d483d-199">**Connection Messages Received Total**</span></span>
- <span data-ttu-id="d483d-200">**連接訊息已傳送總數**</span><span class="sxs-lookup"><span data-stu-id="d483d-200">**Connection Messages Sent Total**</span></span>
- <span data-ttu-id="d483d-201">**連接訊息 Received/Sec**</span><span class="sxs-lookup"><span data-stu-id="d483d-201">**Connection Messages Received/Sec**</span></span>
- <span data-ttu-id="d483d-202">**連接訊息傳送數/秒**</span><span class="sxs-lookup"><span data-stu-id="d483d-202">**Connection Messages Sent/Sec**</span></span>

<span data-ttu-id="d483d-203">**訊息匯流排計量**</span><span class="sxs-lookup"><span data-stu-id="d483d-203">**Message bus metrics**</span></span>

<span data-ttu-id="d483d-204">下列度量會測量透過內部 SignalR 訊息匯流排中所有的傳入和傳出 SignalR 訊息都會放到的佇列的流量。</span><span class="sxs-lookup"><span data-stu-id="d483d-204">The following metrics measure traffic through the internal SignalR message bus, the queue in which all incoming and outgoing SignalR messages are placed.</span></span> <span data-ttu-id="d483d-205">訊息是**發佈**何時傳送或廣播。</span><span class="sxs-lookup"><span data-stu-id="d483d-205">A message is **Published** when it is sent or broadcast.</span></span> <span data-ttu-id="d483d-206">A**訂閱者**在此內容是在訊息匯流排的訂閱，則這應該會等於用戶端加上伺服器本身的數目。</span><span class="sxs-lookup"><span data-stu-id="d483d-206">A **Subscriber** in this context is a subscription on the message bus; this should equal the number of clients plus the server itself.</span></span> <span data-ttu-id="d483d-207">**配置工作者**是一種元件傳送資料至作用中連線;**忙碌的背景工作**一個主動傳送一則訊息。</span><span class="sxs-lookup"><span data-stu-id="d483d-207">An **Allocated Worker** is a component that sends data to active connections; a **Busy Worker** is one that is actively sending a message.</span></span>

- <span data-ttu-id="d483d-208">**訊息匯流排的訊息已接收的總計**</span><span class="sxs-lookup"><span data-stu-id="d483d-208">**Message Bus Messages Received Total**</span></span>
- <span data-ttu-id="d483d-209">**訊息匯流排的訊息數/秒**</span><span class="sxs-lookup"><span data-stu-id="d483d-209">**Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="d483d-210">**已發佈的訊息匯流排訊息總數**</span><span class="sxs-lookup"><span data-stu-id="d483d-210">**Message Bus Messages Published Total**</span></span>
- <span data-ttu-id="d483d-211">**訊息匯流排的訊息發行/Sec**</span><span class="sxs-lookup"><span data-stu-id="d483d-211">**Message Bus Messages Published/Sec**</span></span>
- <span data-ttu-id="d483d-212">**目前的訊息匯流排的訂閱者**</span><span class="sxs-lookup"><span data-stu-id="d483d-212">**Message Bus Subscribers Current**</span></span>
- <span data-ttu-id="d483d-213">**訊息匯流排的訂閱者總數**</span><span class="sxs-lookup"><span data-stu-id="d483d-213">**Message Bus Subscribers Total**</span></span>
- <span data-ttu-id="d483d-214">**訊息匯流排的訂閱者/Sec**</span><span class="sxs-lookup"><span data-stu-id="d483d-214">**Message Bus Subscribers/Sec**</span></span>
- <span data-ttu-id="d483d-215">**配置背景工作的訊息匯流排**</span><span class="sxs-lookup"><span data-stu-id="d483d-215">**Message Bus Allocated Workers**</span></span>
- <span data-ttu-id="d483d-216">**訊息匯流排忙碌員工**</span><span class="sxs-lookup"><span data-stu-id="d483d-216">**Message Bus Busy Workers**</span></span>
- <span data-ttu-id="d483d-217">**目前的訊息匯流排主題**</span><span class="sxs-lookup"><span data-stu-id="d483d-217">**Message Bus Topics Current**</span></span>

<span data-ttu-id="d483d-218">**誤差度量**</span><span class="sxs-lookup"><span data-stu-id="d483d-218">**Error metrics**</span></span>

<span data-ttu-id="d483d-219">下列度量會測量 SignalR 訊息傳輸所產生的錯誤。</span><span class="sxs-lookup"><span data-stu-id="d483d-219">The following metrics measure errors generated by SignalR message traffic.</span></span> <span data-ttu-id="d483d-220">**中樞解析**集線器或中樞方法無法解析時，會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="d483d-220">**Hub Resolution** errors occur when a hub or hub method cannot be resolved.</span></span> <span data-ttu-id="d483d-221">**中樞叫用**錯誤會叫用中樞方法時擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="d483d-221">**Hub Invocation** errors are exceptions thrown while invoking a hub method.</span></span> <span data-ttu-id="d483d-222">**傳輸**錯誤是在 HTTP 要求或回應期間擲回的連接錯誤。</span><span class="sxs-lookup"><span data-stu-id="d483d-222">**Transport** errors are connection errors thrown during an HTTP request or response.</span></span>

- <span data-ttu-id="d483d-223">**錯誤： 所有總計**</span><span class="sxs-lookup"><span data-stu-id="d483d-223">**Errors: All Total**</span></span>
- <span data-ttu-id="d483d-224">**錯誤： All/Sec**</span><span class="sxs-lookup"><span data-stu-id="d483d-224">**Errors: All/Sec**</span></span>
- <span data-ttu-id="d483d-225">**錯誤： 中樞解析總計**</span><span class="sxs-lookup"><span data-stu-id="d483d-225">**Errors: Hub Resolution Total**</span></span>
- <span data-ttu-id="d483d-226">**錯誤： 中樞解析數/秒**</span><span class="sxs-lookup"><span data-stu-id="d483d-226">**Errors: Hub Resolution/Sec**</span></span>
- <span data-ttu-id="d483d-227">**錯誤： 中樞叫用總數**</span><span class="sxs-lookup"><span data-stu-id="d483d-227">**Errors: Hub Invocation Total**</span></span>
- <span data-ttu-id="d483d-228">**錯誤： 中樞叫用數/秒**</span><span class="sxs-lookup"><span data-stu-id="d483d-228">**Errors: Hub Invocation/Sec**</span></span>
- <span data-ttu-id="d483d-229">**錯誤： 傳輸總計**</span><span class="sxs-lookup"><span data-stu-id="d483d-229">**Errors: Transport Total**</span></span>
- <span data-ttu-id="d483d-230">**錯誤： 傳輸數/秒**</span><span class="sxs-lookup"><span data-stu-id="d483d-230">**Errors: Transport/Sec**</span></span>

<a id="scaleout_metrics"></a>

<span data-ttu-id="d483d-231">**範圍外的度量**</span><span class="sxs-lookup"><span data-stu-id="d483d-231">**Scaleout metrics**</span></span>

<span data-ttu-id="d483d-232">下列度量會測量流量和向外延展提供者所產生的錯誤。</span><span class="sxs-lookup"><span data-stu-id="d483d-232">The following metrics measure traffic and errors generated by the scaleout provider.</span></span> <span data-ttu-id="d483d-233">A**資料流**在此內容是縮放單位向外延展提供者使用; 如果這是資料表，如果使用 SQL Server、 使用服務匯流排時，如果某個主題和訂用帳戶使用 Redis。</span><span class="sxs-lookup"><span data-stu-id="d483d-233">A **Stream** in this context is a scale unit used by the scaleout provider; this is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used.</span></span> <span data-ttu-id="d483d-234">每個資料流可確保已排序的讀取和寫入作業。單一資料流是潛在的小數位數瓶頸，因此可以增加的資料流數目，有助於減少該瓶頸。</span><span class="sxs-lookup"><span data-stu-id="d483d-234">Each stream ensures ordered read and write operations; a single stream is a potential scale bottleneck, so the number of streams can be increased to help reduce that bottleneck.</span></span> <span data-ttu-id="d483d-235">如果使用多個資料流，SignalR 會自動分配這些資料流的方式，可確保從任何給定的連線傳送訊息的順序 （分區） 訊息。</span><span class="sxs-lookup"><span data-stu-id="d483d-235">If multiple streams are used, SignalR will automatically distribute (shard) messages across these streams in a way that ensures messages sent from any given connection are in order.</span></span>

<span data-ttu-id="d483d-236">[MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx)設定控制的維護的 SignalR 範圍外傳送佇列長度。</span><span class="sxs-lookup"><span data-stu-id="d483d-236">The [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) setting controls the length of the scaleout send queue maintained by SignalR.</span></span> <span data-ttu-id="d483d-237">將它的值大於 0 會在傳送佇列以傳送一次設定的訊息後擋板以將所有訊息。</span><span class="sxs-lookup"><span data-stu-id="d483d-237">Setting it to a value greater than 0 will place all messages in a send queue to be sent one at a time to the configured messaging backplane.</span></span> <span data-ttu-id="d483d-238">如果佇列的大小超出設定的長度，後續的呼叫將會立即失敗並[InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx)直到佇列中的訊息數小於設定一次。</span><span class="sxs-lookup"><span data-stu-id="d483d-238">If the size of the queue goes above the configured length, subsequent calls to send will fail immediately with an [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) until the number of messages in the queue is less than the setting again.</span></span> <span data-ttu-id="d483d-239">因為已實作的背板通常已有自己的佇列或流程控制預設會停用佇列。</span><span class="sxs-lookup"><span data-stu-id="d483d-239">Queueing is disabled by default because the implemented backplanes generally have their own queuing or flow-control in place.</span></span> <span data-ttu-id="d483d-240">在 SQL Server 連接共用實際上會限制傳送一次進行的作業數目。</span><span class="sxs-lookup"><span data-stu-id="d483d-240">In the case of SQL Server, connection pooling effectively limits the number of sends going on at any one time.</span></span>

<span data-ttu-id="d483d-241">根據預設，只有一個資料流使用的 SQL Server 和 Redis，五個資料流用於服務匯流排和已停用佇列，但可以變更這些設定，透過 SQL Server 和服務匯流排上的設定：</span><span class="sxs-lookup"><span data-stu-id="d483d-241">By default, only one stream is used for SQL Server and Redis, five streams are used for Service Bus, and queueing is disabled, but these settings can be changed through configuration on SQL Server and Service Bus:</span></span>

<span data-ttu-id="d483d-242">**.NET 伺服端程式碼來設定資料表的 SQL Server 後擋板計數和佇列長度**</span><span class="sxs-lookup"><span data-stu-id="d483d-242">**.NET Server Code for configuring table count and queue length for SQL Server backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample9.cs)]

<span data-ttu-id="d483d-243">**.NET 伺服端程式碼來設定服務匯流排後擋板的主題計數和佇列長度**</span><span class="sxs-lookup"><span data-stu-id="d483d-243">**.NET Server Code for configuring topic count and queue length for Service Bus backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample10.cs)]

<span data-ttu-id="d483d-244">A**緩衝處理**資料流是已進入錯誤的狀態，則當資料流處於錯誤狀態時，所有傳送至後擋板的訊息將會失敗，資料流不會再判定為失敗之前，立即。</span><span class="sxs-lookup"><span data-stu-id="d483d-244">A **Buffering** stream is one that has entered a faulted state; when the stream is in the faulted state, all messages sent to the backplane will fail immediately until the stream is no longer faulting.</span></span> <span data-ttu-id="d483d-245">**傳送佇列長度**是張貼但尚未傳送的訊息數目。</span><span class="sxs-lookup"><span data-stu-id="d483d-245">The **Send Queue Length** is the number of messages that have been posted but not yet sent.</span></span>

- <span data-ttu-id="d483d-246">**範圍外訊息匯流排的訊息數/秒**</span><span class="sxs-lookup"><span data-stu-id="d483d-246">**Scaleout Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="d483d-247">**向外延展資料流總數**</span><span class="sxs-lookup"><span data-stu-id="d483d-247">**Scaleout Streams Total**</span></span>
- <span data-ttu-id="d483d-248">**向外延展資料流開啟**</span><span class="sxs-lookup"><span data-stu-id="d483d-248">**Scaleout Streams Open**</span></span>
- <span data-ttu-id="d483d-249">**向外延展資料流緩衝**</span><span class="sxs-lookup"><span data-stu-id="d483d-249">**Scaleout Streams Buffering**</span></span>
- <span data-ttu-id="d483d-250">**範圍外錯誤總數**</span><span class="sxs-lookup"><span data-stu-id="d483d-250">**Scaleout Errors Total**</span></span>
- <span data-ttu-id="d483d-251">**Scaleout Errors/Sec**</span><span class="sxs-lookup"><span data-stu-id="d483d-251">**Scaleout Errors/Sec**</span></span>
- <span data-ttu-id="d483d-252">**範圍外傳送佇列長度**</span><span class="sxs-lookup"><span data-stu-id="d483d-252">**Scaleout Send Queue Length**</span></span>

<span data-ttu-id="d483d-253">如需有關這些計數器會測量的詳細資訊，請參閱[SignalR 範圍外使用 Azure 服務匯流排](scaleout-with-windows-azure-service-bus.md)。</span><span class="sxs-lookup"><span data-stu-id="d483d-253">For more information on what these counters are measuring, see [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span></span>

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a><span data-ttu-id="d483d-254">使用其他效能計數器</span><span class="sxs-lookup"><span data-stu-id="d483d-254">Using other performance counters</span></span>

<span data-ttu-id="d483d-255">下列效能計數器也可能在監視應用程式的效能很有用。</span><span class="sxs-lookup"><span data-stu-id="d483d-255">The following performance counters may also be useful in monitoring your application's performance.</span></span>

<span data-ttu-id="d483d-256">**Memory**</span><span class="sxs-lookup"><span data-stu-id="d483d-256">**Memory**</span></span>

- <span data-ttu-id="d483d-257">.NET CLR 記憶體\\全部堆積 （w3wp) 中的 # 位元組</span><span class="sxs-lookup"><span data-stu-id="d483d-257">.NET CLR Memory\\# bytes in all Heaps (for w3wp)</span></span>

<span data-ttu-id="d483d-258">**ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="d483d-258">**ASP.NET**</span></span>

- <span data-ttu-id="d483d-259">ASP.NET\Requests Current</span><span class="sxs-lookup"><span data-stu-id="d483d-259">ASP.NET\Requests Current</span></span>
- <span data-ttu-id="d483d-260">ASP.NET\Queued</span><span class="sxs-lookup"><span data-stu-id="d483d-260">ASP.NET\Queued</span></span>
- <span data-ttu-id="d483d-261">ASP.NET\Rejected</span><span class="sxs-lookup"><span data-stu-id="d483d-261">ASP.NET\Rejected</span></span>

<span data-ttu-id="d483d-262">**CPU**</span><span class="sxs-lookup"><span data-stu-id="d483d-262">**CPU**</span></span>

- <span data-ttu-id="d483d-263">處理器 Information\Processor 時間</span><span class="sxs-lookup"><span data-stu-id="d483d-263">Processor Information\Processor Time</span></span>

<span data-ttu-id="d483d-264">**TCP/IP**</span><span class="sxs-lookup"><span data-stu-id="d483d-264">**TCP/IP**</span></span>

- <span data-ttu-id="d483d-265">TCPv6/建立的連接</span><span class="sxs-lookup"><span data-stu-id="d483d-265">TCPv6/Connections Established</span></span>
- <span data-ttu-id="d483d-266">TCPv4/建立的連接</span><span class="sxs-lookup"><span data-stu-id="d483d-266">TCPv4/Connections Established</span></span>

<span data-ttu-id="d483d-267">**Web 服務**</span><span class="sxs-lookup"><span data-stu-id="d483d-267">**Web Service**</span></span>

- <span data-ttu-id="d483d-268">Web Service\Current Connections</span><span class="sxs-lookup"><span data-stu-id="d483d-268">Web Service\Current Connections</span></span>
- <span data-ttu-id="d483d-269">Web Service\Maximum 連線</span><span class="sxs-lookup"><span data-stu-id="d483d-269">Web Service\Maximum Connections</span></span>

<span data-ttu-id="d483d-270">**執行緒處理**</span><span class="sxs-lookup"><span data-stu-id="d483d-270">**Threading**</span></span>

- <span data-ttu-id="d483d-271">.NET CLR 鎖定和執行緒\\# 個目前的邏輯執行緒</span><span class="sxs-lookup"><span data-stu-id="d483d-271">.NET CLR Locks And Threads\\# of current logical Threads</span></span>
- <span data-ttu-id="d483d-272">.NET CLR 鎖定和執行緒\\# 個目前的實體執行緒</span><span class="sxs-lookup"><span data-stu-id="d483d-272">.NET CLR Locks And Threads\\# of current physical Threads</span></span>

<a id="otherresources"></a>

## <a name="other-resources"></a><span data-ttu-id="d483d-273">其他資源</span><span class="sxs-lookup"><span data-stu-id="d483d-273">Other Resources</span></span>

<span data-ttu-id="d483d-274">如需 ASP.NET 效能監視與微調的詳細資訊，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="d483d-274">For more information on ASP.NET performance monitoring and tuning, see the following topics:</span></span>

- <span data-ttu-id="d483d-275">[ASP.NET 效能概觀](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="d483d-275">[ASP.NET Performance Overview](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span></span>
- [<span data-ttu-id="d483d-276">IIS 7.5、 IIS 7.0 和 IIS 6.0 的 ASP.NET 執行緒用法</span><span class="sxs-lookup"><span data-stu-id="d483d-276">ASP.NET Thread Usage on IIS 7.5, IIS 7.0, and IIS 6.0</span></span>](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [<span data-ttu-id="d483d-277">&lt;應用程式集區&gt;項目 （Web 設定）</span><span class="sxs-lookup"><span data-stu-id="d483d-277">&lt;applicationPool&gt; Element (Web Settings)</span></span>](https://msdn.microsoft.com/library/dd560842.aspx)
