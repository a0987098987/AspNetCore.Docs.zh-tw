---
uid: signalr/overview/older-versions/signalr-performance
title: SignalR 效能 (SignalR 1.x) |Microsoft Docs
author: pfletcher
description: SignalR 效能
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2013
ms.topic: article
ms.assetid: 9594d644-66b6-4223-acdd-23e29a6e4c46
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/older-versions/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: bb5bc29306ad94597239fa6d366be231ab1e86e1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362472"
---
<a name="signalr-performance-signalr-1x"></a><span data-ttu-id="6aaba-103">SignalR 效能 (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="6aaba-103">SignalR Performance (SignalR 1.x)</span></span>
====================
<span data-ttu-id="6aaba-104">藉由[Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="6aaba-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="6aaba-105">本主題描述如何設計、 測量和改善的 SignalR 應用程式中的效能。</span><span class="sxs-lookup"><span data-stu-id="6aaba-105">This topic describes how to design for, measure, and improve performance in a SignalR application.</span></span>


<span data-ttu-id="6aaba-106">SignalR 效能和調整的近期簡報，請參閱 <<c0> [ 調整與 ASP.NET SignalR 即時 Web](https://channel9.msdn.com/Events/Build/2013/3-502)。</span><span class="sxs-lookup"><span data-stu-id="6aaba-106">For a recent presentation on SignalR performance and scaling, see [Scaling the Real-time Web with ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span></span>

<span data-ttu-id="6aaba-107">此主題包括下列章節：</span><span class="sxs-lookup"><span data-stu-id="6aaba-107">This topic contains the following sections:</span></span>

- [<span data-ttu-id="6aaba-108">設計考量</span><span class="sxs-lookup"><span data-stu-id="6aaba-108">Design considerations</span></span>](#design)
- [<span data-ttu-id="6aaba-109">微調 SignalR 伺服器效能</span><span class="sxs-lookup"><span data-stu-id="6aaba-109">Tuning your SignalR server for performance</span></span>](#tuning)
- [<span data-ttu-id="6aaba-110">針對效能問題進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="6aaba-110">Troubleshooting performance issues</span></span>](#troubleshooting)
- [<span data-ttu-id="6aaba-111">使用 SignalR 效能計數器</span><span class="sxs-lookup"><span data-stu-id="6aaba-111">Using SignalR performance counters</span></span>](#perfcounters)
- [<span data-ttu-id="6aaba-112">使用其他的效能計數器</span><span class="sxs-lookup"><span data-stu-id="6aaba-112">Using other performance counters</span></span>](#othercounters)
- [<span data-ttu-id="6aaba-113">其他資源</span><span class="sxs-lookup"><span data-stu-id="6aaba-113">Other resources</span></span>](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a><span data-ttu-id="6aaba-114">設計考量</span><span class="sxs-lookup"><span data-stu-id="6aaba-114">Design considerations</span></span>

<span data-ttu-id="6aaba-115">本章節描述可實作 SignalR 應用程式，以確保效能，正在降低藉由產生不必要的網路流量的設計期間的模式。</span><span class="sxs-lookup"><span data-stu-id="6aaba-115">This section describes patterns that can be implemented during the design of a SignalR application, to ensure that performance is not being hindered by generating unnecessary network traffic.</span></span>

### <a name="throttling-message-frequency"></a><span data-ttu-id="6aaba-116">節流的訊息頻率</span><span class="sxs-lookup"><span data-stu-id="6aaba-116">Throttling message frequency</span></span>

<span data-ttu-id="6aaba-117">即使在送出訊息 （例如即時遊戲應用程式） 的高頻率的應用程式，大部分的應用程式不需要傳送第二個的多個訊息。</span><span class="sxs-lookup"><span data-stu-id="6aaba-117">Even in an application that sends out messages at a high frequency (such as a realtime gaming application), most applications don't need to send more than a few messages a second.</span></span> <span data-ttu-id="6aaba-118">若要減少每個用戶端產生的流量，訊息迴圈可以實作佇列，並送出訊息不超過經常以固定費率 （也就是特定數目的訊息將會傳送到每秒中，是否在該時間中有訊息terval 傳送)。</span><span class="sxs-lookup"><span data-stu-id="6aaba-118">To reduce the amount of traffic that each client generates, a message loop can be implemented that queues and sends out messages no more frequently than a fixed rate (that is, up to a certain number of messages will be sent every second, if there are messages in that time interval to be sent).</span></span> <span data-ttu-id="6aaba-119">節流的訊息，以特定的費率 （從用戶端和伺服器） 的範例應用程式，請參閱[高頻率即時與 SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md)。</span><span class="sxs-lookup"><span data-stu-id="6aaba-119">For a sample application that throttles messages to a certain rate (from both client and server), see [High-Frequency Realtime with SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span></span>

### <a name="reducing-message-size"></a><span data-ttu-id="6aaba-120">減少訊息大小</span><span class="sxs-lookup"><span data-stu-id="6aaba-120">Reducing message size</span></span>

<span data-ttu-id="6aaba-121">您可以藉由減少您序列化物件的大小減少 SignalR 訊息的大小。</span><span class="sxs-lookup"><span data-stu-id="6aaba-121">You can reduce the size of a SignalR message by reducing the size of your serialized objects.</span></span> <span data-ttu-id="6aaba-122">在伺服器程式碼，如果您要傳送的物件，其中包含要傳送不需要的屬性，會阻止這些屬性序列化使用`JsonIgnore`屬性。</span><span class="sxs-lookup"><span data-stu-id="6aaba-122">In server code, if you're sending an object that contains properties that don't need to be transmitted, prevent those properties from being serialized by using the `JsonIgnore` attribute.</span></span> <span data-ttu-id="6aaba-123">屬性名稱也會儲存在訊息;屬性名稱可以使用縮短`JsonProperty`屬性。</span><span class="sxs-lookup"><span data-stu-id="6aaba-123">The names of properties are also stored in the message; the names of properties can be shortened using the `JsonProperty` attribute.</span></span> <span data-ttu-id="6aaba-124">下列程式碼範例示範如何排除屬性傳送至用戶端，以及如何縮短屬性名稱：</span><span class="sxs-lookup"><span data-stu-id="6aaba-124">The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:</span></span>

<span data-ttu-id="6aaba-125">**.NET 伺服器程式碼，示範 JsonIgnore 屬性，若要排除的資料傳送至用戶端，以及減少訊息大小，JsonProperty 屬性**</span><span class="sxs-lookup"><span data-stu-id="6aaba-125">**.NET server code that demonstrates the JsonIgnore attribute to exclude data from being sent to the client, and the JsonProperty attribute to reduce message size**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

<span data-ttu-id="6aaba-126">若要保留可讀性 / 用戶端程式碼中的可維護性，在收到訊息之後的縮寫的屬性名稱可能會以人類易重新對應的名稱。</span><span class="sxs-lookup"><span data-stu-id="6aaba-126">In order to retain readability/ maintainability in the client code, the abbreviated property names can be remapped to human-friendly names after the message is received.</span></span> <span data-ttu-id="6aaba-127">下列程式碼範例示範一個可能方式重新對應的簡短的名稱，以較長的藉由定義訊息合約 （對應），並使用`reMap`来套用的最佳化的訊息類別合約函式：</span><span class="sxs-lookup"><span data-stu-id="6aaba-127">The following code sample demonstrates one possible way of remapping shortened names to longer ones, by defining a message contract (mapping), and using the `reMap` function to apply the contract to the optimized message class:</span></span>

<span data-ttu-id="6aaba-128">**重新對應的用戶端 JavaScript 程式碼已縮短人類看得懂的名稱的屬性名稱**</span><span class="sxs-lookup"><span data-stu-id="6aaba-128">**Client-side JavaScript code that remaps shortened property names to human-readable names**</span></span>

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

<span data-ttu-id="6aaba-129">名稱可以縮短在用戶端的訊息至伺服器，使用相同的方法。</span><span class="sxs-lookup"><span data-stu-id="6aaba-129">Names can be shortened in messages from the client to the server as well, using the same method.</span></span>

<span data-ttu-id="6aaba-130">減少記憶體使用量 （也就是訊息所使用的記憶體數量） 訊息的物件也可以改善效能。</span><span class="sxs-lookup"><span data-stu-id="6aaba-130">Reducing the memory footprint (that is, the amount of memory used for the message) of the message object can also improve performance.</span></span> <span data-ttu-id="6aaba-131">例如，如果的全套`int`不需要`short`或`byte`可以改為使用。</span><span class="sxs-lookup"><span data-stu-id="6aaba-131">For example, if the full range of an `int` is not needed, a `short` or `byte` can be used instead.</span></span>

<span data-ttu-id="6aaba-132">由於訊息會儲存在伺服器記憶體中，進而減少訊息大小的訊息匯流排也可以處理伺服器的記憶體問題。</span><span class="sxs-lookup"><span data-stu-id="6aaba-132">Since messages are stored in the message bus in server memory, reducing the size of messages can also address server memory issues.</span></span>

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a><span data-ttu-id="6aaba-133">微調 SignalR 伺服器效能</span><span class="sxs-lookup"><span data-stu-id="6aaba-133">Tuning your SignalR server for performance</span></span>

<span data-ttu-id="6aaba-134">下列組態設定可用來微調您的伺服器中的 SignalR 應用程式更佳的效能。</span><span class="sxs-lookup"><span data-stu-id="6aaba-134">The following configuration settings can be used to tune your server for better performance in a SignalR application.</span></span> <span data-ttu-id="6aaba-135">如需如何在 ASP.NET 應用程式中改善效能的一般資訊，請參閱[提升 ASP.NET 效能](https://msdn.microsoft.com/library/ff647787.aspx)。</span><span class="sxs-lookup"><span data-stu-id="6aaba-135">For general information on how to improve performance in an ASP.NET application, see [Improving ASP.NET Performance](https://msdn.microsoft.com/library/ff647787.aspx).</span></span>

<span data-ttu-id="6aaba-136">**SignalR 組態設定**</span><span class="sxs-lookup"><span data-stu-id="6aaba-136">**SignalR configuration settings**</span></span>

- <span data-ttu-id="6aaba-137">**DefaultMessageBufferSize**： 依預設，SignalR 會保留在記憶體中每個連線的中樞每 1000 則訊息。</span><span class="sxs-lookup"><span data-stu-id="6aaba-137">**DefaultMessageBufferSize**: By default, SignalR retains 1000 messages in memory per hub per connection.</span></span> <span data-ttu-id="6aaba-138">如果使用大型訊息時，這可能會產生記憶體問題可以降低此值可以緩解這些問題。</span><span class="sxs-lookup"><span data-stu-id="6aaba-138">If large messages are being used, this may create memory issues which can be alleviated by reducing this value.</span></span> <span data-ttu-id="6aaba-139">這項設定可以在中設定`Application_Start`事件處理常式在 ASP.NET 應用程式，或`Configuration`的 OWIN 啟動類別中的自我裝載的應用程式的方法。</span><span class="sxs-lookup"><span data-stu-id="6aaba-139">This setting can be set in the `Application_Start` event handler in an ASP.NET application, or in the `Configuration` method of an OWIN startup class in a self-hosted application.</span></span> <span data-ttu-id="6aaba-140">下列範例示範如何減少此值，以降低您的應用程式，以降低伺服器使用的記憶體數量的記憶體使用量：</span><span class="sxs-lookup"><span data-stu-id="6aaba-140">The following sample demonstrates how to reduce this value in order to reduce the memory footprint of your application in order to reduce the amount of server memory used:</span></span>

    <span data-ttu-id="6aaba-141">**.NET 伺服器程式碼在 Global.asax 中的減少預設的訊息緩衝區大小**</span><span class="sxs-lookup"><span data-stu-id="6aaba-141">**.NET server code in Global.asax for decreasing default message buffer size**</span></span>

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

<span data-ttu-id="6aaba-142">**IIS 組態設定**</span><span class="sxs-lookup"><span data-stu-id="6aaba-142">**IIS configuration settings**</span></span>

- <span data-ttu-id="6aaba-143">**每個應用程式的最大並行要求**： 增加並行的 IIS 要求將會增加可用的服務要求的伺服器資源。</span><span class="sxs-lookup"><span data-stu-id="6aaba-143">**Max concurrent requests per application**: Increasing the number of concurrent IIS requests will increase server resources available for serving requests.</span></span> <span data-ttu-id="6aaba-144">預設值為 5000。若要增加此設定，請在提升權限的命令提示字元執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="6aaba-144">The default value is 5000; to increase this setting, execute the following commands in an elevated command prompt:</span></span>

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]

<span data-ttu-id="6aaba-145">**ASP.NET 組態設定**</span><span class="sxs-lookup"><span data-stu-id="6aaba-145">**ASP.NET configuration settings**</span></span>

<span data-ttu-id="6aaba-146">本節包含可在設定的組態設定`aspnet.config`檔案。</span><span class="sxs-lookup"><span data-stu-id="6aaba-146">This section includes configuration settings that can be set in the `aspnet.config` file.</span></span> <span data-ttu-id="6aaba-147">這個檔案位於其中一個位置，根據平台：</span><span class="sxs-lookup"><span data-stu-id="6aaba-147">This file is found in one of two locations, depending on platform:</span></span>

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

<span data-ttu-id="6aaba-148">可能會改善 SignalR 效能的 ASP.NET 設定包括下列各項：</span><span class="sxs-lookup"><span data-stu-id="6aaba-148">ASP.NET settings that may improve SignalR performance include the following:</span></span>

- <span data-ttu-id="6aaba-149">**每一 CPU 的最大並行要求**： 提高此設定可能會解決效能瓶頸。</span><span class="sxs-lookup"><span data-stu-id="6aaba-149">**Maximum concurrent requests per CPU**: Increasing this setting may alleviate performance bottlenecks.</span></span> <span data-ttu-id="6aaba-150">若要增加此設定，新增下列組態設定加入`aspnet.config`檔案：</span><span class="sxs-lookup"><span data-stu-id="6aaba-150">To increase this setting, add the following configuration setting to the `aspnet.config` file:</span></span>

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- <span data-ttu-id="6aaba-151">**要求佇列限制**： 當連線總數超過`maxConcurrentRequestsPerCPU`設定時，ASP.NET 會開始節流使用佇列的要求。</span><span class="sxs-lookup"><span data-stu-id="6aaba-151">**Request Queue Limit**: When the total number of connections exceeds the `maxConcurrentRequestsPerCPU` setting, ASP.NET will start throttling requests using a queue.</span></span> <span data-ttu-id="6aaba-152">若要增加佇列的大小，您可以增加`requestQueueLimit`設定。</span><span class="sxs-lookup"><span data-stu-id="6aaba-152">To increase the size of the queue, you can increase the `requestQueueLimit` setting.</span></span> <span data-ttu-id="6aaba-153">若要這樣做，請新增下列組態設定加入`processModel`中的節點`config/machine.config`(而非`aspnet.config`):</span><span class="sxs-lookup"><span data-stu-id="6aaba-153">To do this, add the following configuration setting to the `processModel` node in `config/machine.config` (rather than `aspnet.config`):</span></span>

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a><span data-ttu-id="6aaba-154">針對效能問題進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="6aaba-154">Troubleshooting performance issues</span></span>

<span data-ttu-id="6aaba-155">本章節描述如何在您的應用程式中，找出效能瓶頸。</span><span class="sxs-lookup"><span data-stu-id="6aaba-155">This section describes ways to find performance bottlenecks in your application.</span></span>

### <a name="verifying-that-websocket-is-being-used"></a><span data-ttu-id="6aaba-156">驗證正在使用 WebSocket</span><span class="sxs-lookup"><span data-stu-id="6aaba-156">Verifying that WebSocket is being used</span></span>

<span data-ttu-id="6aaba-157">雖然 SignalR 可以使用各種傳輸用戶端與伺服器之間的通訊，WebSocket 提供顯著的優勢，並應在用戶端和伺服器支援它。</span><span class="sxs-lookup"><span data-stu-id="6aaba-157">While SignalR can use a variety of transports for communication between client and server, WebSocket offers a significant performance advantage, and should be used if the client and server support it.</span></span> <span data-ttu-id="6aaba-158">若要判斷您的用戶端和伺服器是否符合 WebSocket 的需求，請參閱[傳輸和後援](../getting-started/introduction-to-signalr.md#transports)。</span><span class="sxs-lookup"><span data-stu-id="6aaba-158">To determine if your client and server meet the requirements for WebSocket, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="6aaba-159">若要判斷何種傳輸正用於您的應用程式，您可以使用瀏覽器的開發人員工具，，並檢查記錄，以查看所使用的傳輸用於連接。</span><span class="sxs-lookup"><span data-stu-id="6aaba-159">To determine what transport is being used in your application, you can use the browser developer tools, and examine the logs to see what transport is being used for the connection.</span></span> <span data-ttu-id="6aaba-160">如需使用瀏覽器的開發工具，在 Internet Explorer 和 Chrome 中的資訊，請參閱[傳輸和後援](../getting-started/introduction-to-signalr.md#transports)。</span><span class="sxs-lookup"><span data-stu-id="6aaba-160">For information on using the browser development tools in Internet Explorer and Chrome, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a><span data-ttu-id="6aaba-161">使用 SignalR 效能計數器</span><span class="sxs-lookup"><span data-stu-id="6aaba-161">Using SignalR performance counters</span></span>

<span data-ttu-id="6aaba-162">本節說明如何啟用和使用 SignalR 效能計數器、 位於`Microsoft.AspNet.SignalR.Utils`封裝。</span><span class="sxs-lookup"><span data-stu-id="6aaba-162">This section describes how to enable and use SignalR performance counters, found in the `Microsoft.AspNet.SignalR.Utils` package.</span></span>

### <a name="installing-signalrexe"></a><span data-ttu-id="6aaba-163">安裝 signalr.exe</span><span class="sxs-lookup"><span data-stu-id="6aaba-163">Installing signalr.exe</span></span>

<span data-ttu-id="6aaba-164">效能計數器可以加入至使用稱為 SignalR.exe 的公用程式的伺服器。</span><span class="sxs-lookup"><span data-stu-id="6aaba-164">Peformance counters can be added to the server using a utility called SignalR.exe.</span></span> <span data-ttu-id="6aaba-165">若要安裝此公用程式，請遵循下列步驟：</span><span class="sxs-lookup"><span data-stu-id="6aaba-165">To install this utility, follow these steps:</span></span>

1. <span data-ttu-id="6aaba-166">在 Visual Studio 應用程式中，選取**工具**，**程式庫套件管理員**，**管理方案的 NuGet 套件...**</span><span class="sxs-lookup"><span data-stu-id="6aaba-166">In your Visual Studio application, select **Tools**, **Library Package Manager**, **Manage NuGet Packages for Solution...**</span></span>
2. <span data-ttu-id="6aaba-167">搜尋**signalr.utils**，並選取 [安裝]。</span><span class="sxs-lookup"><span data-stu-id="6aaba-167">Search for **signalr.utils**, and select Install.</span></span>

    ![](signalr-performance/_static/image1.png)
3. <span data-ttu-id="6aaba-168">接受授權合約，來安裝封裝。</span><span class="sxs-lookup"><span data-stu-id="6aaba-168">Accept the license agreement to install the package.</span></span>
4. <span data-ttu-id="6aaba-169">SignalR.exe 將會安裝成`<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`。</span><span class="sxs-lookup"><span data-stu-id="6aaba-169">SignalR.exe will be installed to `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span></span>

### <a name="installing-performance-counters-with-signalrexe"></a><span data-ttu-id="6aaba-170">使用 SignalR.exe 安裝效能計數器</span><span class="sxs-lookup"><span data-stu-id="6aaba-170">Installing performance counters with SignalR.exe</span></span>

<span data-ttu-id="6aaba-171">若要安裝 SignalR 效能計數器，請在提升權限的命令提示字元使用下列參數執行 SignalR.exe:</span><span class="sxs-lookup"><span data-stu-id="6aaba-171">To install SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

<span data-ttu-id="6aaba-172">若要移除 SignalR 效能計數器，請在提升權限的命令提示字元使用下列參數執行 SignalR.exe:</span><span class="sxs-lookup"><span data-stu-id="6aaba-172">To remove SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a><span data-ttu-id="6aaba-173">SignalR 效能計數器</span><span class="sxs-lookup"><span data-stu-id="6aaba-173">SignalR Performance counters</span></span>

<span data-ttu-id="6aaba-174">公用程式封裝會安裝下列效能計數器。</span><span class="sxs-lookup"><span data-stu-id="6aaba-174">The utilites package installs the following performance counters.</span></span> <span data-ttu-id="6aaba-175">自從最後一個應用程式集區或伺服器重新啟動之後，「 總計 」 計數器會測量事件的數目。</span><span class="sxs-lookup"><span data-stu-id="6aaba-175">The "Total" counters measure the number of events since the last application pool or server restart.</span></span>

<span data-ttu-id="6aaba-176">**連線計量**</span><span class="sxs-lookup"><span data-stu-id="6aaba-176">**Connection metrics**</span></span>

<span data-ttu-id="6aaba-177">下列度量會測量所發生的連線存留期事件。</span><span class="sxs-lookup"><span data-stu-id="6aaba-177">The following metrics measure the connection lifetime events that occur.</span></span> <span data-ttu-id="6aaba-178">如需詳細資訊，請參閱 <<c0> [ 了解和處理連線存留期事件](../guide-to-the-api/handling-connection-lifetime-events.md)。</span><span class="sxs-lookup"><span data-stu-id="6aaba-178">For more information, see [Understanding and Handling Connection Lifetime Events](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

- <span data-ttu-id="6aaba-179">**連線的連線**</span><span class="sxs-lookup"><span data-stu-id="6aaba-179">**Connections Connected**</span></span>
- <span data-ttu-id="6aaba-180">**重新連線的連線**</span><span class="sxs-lookup"><span data-stu-id="6aaba-180">**Connections Reconnected**</span></span>
- <span data-ttu-id="6aaba-181">**連線 Disonnected**</span><span class="sxs-lookup"><span data-stu-id="6aaba-181">**Connections Disonnected**</span></span>
- <span data-ttu-id="6aaba-182">**目前的連線**</span><span class="sxs-lookup"><span data-stu-id="6aaba-182">**Connections Current**</span></span>

<span data-ttu-id="6aaba-183">**訊息計量**</span><span class="sxs-lookup"><span data-stu-id="6aaba-183">**Message metrics**</span></span>

<span data-ttu-id="6aaba-184">下列度量會測量產生 SignalR 的訊息流量。</span><span class="sxs-lookup"><span data-stu-id="6aaba-184">The following metrics measure the message traffic generated by SignalR.</span></span>

- <span data-ttu-id="6aaba-185">**已接收連線的郵件總數**</span><span class="sxs-lookup"><span data-stu-id="6aaba-185">**Connection Messages Received Total**</span></span>
- <span data-ttu-id="6aaba-186">**連接訊息已傳送總數**</span><span class="sxs-lookup"><span data-stu-id="6aaba-186">**Connection Messages Sent Total**</span></span>
- <span data-ttu-id="6aaba-187">**連接訊息 Received/Sec**</span><span class="sxs-lookup"><span data-stu-id="6aaba-187">**Connection Messages Received/Sec**</span></span>
- <span data-ttu-id="6aaba-188">**連線的訊息傳送/秒**</span><span class="sxs-lookup"><span data-stu-id="6aaba-188">**Connection Messages Sent/Sec**</span></span>

<span data-ttu-id="6aaba-189">**訊息匯流排計量**</span><span class="sxs-lookup"><span data-stu-id="6aaba-189">**Message bus metrics**</span></span>

<span data-ttu-id="6aaba-190">下列度量會測量流量通過內部 SignalR 訊息匯流排，位於所有傳入和傳出的 SignalR 訊息的佇列。</span><span class="sxs-lookup"><span data-stu-id="6aaba-190">The following metrics measure traffic through the internal SignalR message bus, the queue in which all incoming and outgoing SignalR messages are placed.</span></span> <span data-ttu-id="6aaba-191">訊息**已發佈**當它是傳送或廣播。</span><span class="sxs-lookup"><span data-stu-id="6aaba-191">A message is **Published** when it is sent or broadcast.</span></span> <span data-ttu-id="6aaba-192">A**訂閱者**在此內容是在訊息匯流排上的訂用帳戶，這應該符合用戶端，再加上伺服器本身的數目。</span><span class="sxs-lookup"><span data-stu-id="6aaba-192">A **Subscriber** in this context is a subscription on the message bus; this should equal the number of clients plus the server itself.</span></span> <span data-ttu-id="6aaba-193">**配置的背景工作角色**是一種元件，將資料傳送至作用中連線;**忙碌的背景工作角色**是指是否主動傳送一則訊息。</span><span class="sxs-lookup"><span data-stu-id="6aaba-193">An **Allocated Worker** is a component that sends data to active connections; a **Busy Worker** is one that is actively sending a message.</span></span>

- <span data-ttu-id="6aaba-194">**訊息匯流排的訊息接收的總計**</span><span class="sxs-lookup"><span data-stu-id="6aaba-194">**Message Bus Messages Received Total**</span></span>
- <span data-ttu-id="6aaba-195">**訊息匯流排的訊息數/秒**</span><span class="sxs-lookup"><span data-stu-id="6aaba-195">**Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="6aaba-196">**已發佈的訊息匯流排訊息總計**</span><span class="sxs-lookup"><span data-stu-id="6aaba-196">**Message Bus Messages Published Total**</span></span>
- <span data-ttu-id="6aaba-197">**訊息匯流排郵件發佈數/秒**</span><span class="sxs-lookup"><span data-stu-id="6aaba-197">**Message Bus Messages Published/Sec**</span></span>
- <span data-ttu-id="6aaba-198">**目前的訊息匯流排的訂閱者**</span><span class="sxs-lookup"><span data-stu-id="6aaba-198">**Message Bus Subscribers Current**</span></span>
- <span data-ttu-id="6aaba-199">**訊息匯流排的訂閱者總數**</span><span class="sxs-lookup"><span data-stu-id="6aaba-199">**Message Bus Subscribers Total**</span></span>
- <span data-ttu-id="6aaba-200">**訊息匯流排的訂閱者/Sec**</span><span class="sxs-lookup"><span data-stu-id="6aaba-200">**Message Bus Subscribers/Sec**</span></span>
- <span data-ttu-id="6aaba-201">**訊息匯流排配置的背景工作角色**</span><span class="sxs-lookup"><span data-stu-id="6aaba-201">**Message Bus Allocated Workers**</span></span>
- <span data-ttu-id="6aaba-202">**訊息匯流排忙碌的背景工作角色**</span><span class="sxs-lookup"><span data-stu-id="6aaba-202">**Message Bus Busy Workers**</span></span>
- <span data-ttu-id="6aaba-203">**目前的訊息匯流排的主題**</span><span class="sxs-lookup"><span data-stu-id="6aaba-203">**Message Bus Topics Current**</span></span>

<span data-ttu-id="6aaba-204">**誤差度量**</span><span class="sxs-lookup"><span data-stu-id="6aaba-204">**Error metrics**</span></span>

<span data-ttu-id="6aaba-205">下列度量會測量 SignalR 訊息傳輸所產生的錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aaba-205">The following metrics measure errors generated by SignalR message traffic.</span></span> <span data-ttu-id="6aaba-206">**中樞解析**中樞或中樞方法無法解析時，會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aaba-206">**Hub Resolution** errors occur when a hub or hub method cannot be resolved.</span></span> <span data-ttu-id="6aaba-207">**中樞叫用**錯誤是在叫用中樞方法時擲回的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="6aaba-207">**Hub Invocation** errors are exceptions thrown while invoking a hub method.</span></span> <span data-ttu-id="6aaba-208">**傳輸**錯誤是在 HTTP 要求或回應期間擲回的連線錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aaba-208">**Transport** errors are connection errors thrown during an HTTP request or response.</span></span>

- <span data-ttu-id="6aaba-209">**錯誤： 所有的總數**</span><span class="sxs-lookup"><span data-stu-id="6aaba-209">**Errors: All Total**</span></span>
- <span data-ttu-id="6aaba-210">**錯誤： All/Sec**</span><span class="sxs-lookup"><span data-stu-id="6aaba-210">**Errors: All/Sec**</span></span>
- <span data-ttu-id="6aaba-211">**錯誤： 中樞解析總數**</span><span class="sxs-lookup"><span data-stu-id="6aaba-211">**Errors: Hub Resolution Total**</span></span>
- <span data-ttu-id="6aaba-212">**錯誤： 中樞解析數/秒**</span><span class="sxs-lookup"><span data-stu-id="6aaba-212">**Errors: Hub Resolution/Sec**</span></span>
- <span data-ttu-id="6aaba-213">**錯誤： 中樞叫用總數**</span><span class="sxs-lookup"><span data-stu-id="6aaba-213">**Errors: Hub Invocation Total**</span></span>
- <span data-ttu-id="6aaba-214">**錯誤： 中樞叫用數/秒**</span><span class="sxs-lookup"><span data-stu-id="6aaba-214">**Errors: Hub Invocation/Sec**</span></span>
- <span data-ttu-id="6aaba-215">**錯誤： 傳輸總數**</span><span class="sxs-lookup"><span data-stu-id="6aaba-215">**Errors: Transport Total**</span></span>
- <span data-ttu-id="6aaba-216">**錯誤： 傳輸數/秒**</span><span class="sxs-lookup"><span data-stu-id="6aaba-216">**Errors: Transport/Sec**</span></span>

<span data-ttu-id="6aaba-217">**向外延展計量**</span><span class="sxs-lookup"><span data-stu-id="6aaba-217">**Scaleout metrics**</span></span>

<span data-ttu-id="6aaba-218">下列度量會測量流量和向外延展提供者所產生的錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aaba-218">The following metrics measure traffic and errors generated by the scaleout provider.</span></span> <span data-ttu-id="6aaba-219">A **Stream**在此內容中向外延展提供者所使用的縮放單位; 這是資料表，如果使用 SQL Server、 使用服務匯流排時，如果某個主題和訂用帳戶如果使用 Redis。</span><span class="sxs-lookup"><span data-stu-id="6aaba-219">A **Stream** in this context is a scale unit used by the scaleout provider; this is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used.</span></span> <span data-ttu-id="6aaba-220">根據預設，只有一個資料流使用，但可增加透過 SQL Server 和服務匯流排的組態。</span><span class="sxs-lookup"><span data-stu-id="6aaba-220">By default, only one stream is used, but this can be increased through configuration on SQL Server and Service Bus.</span></span> <span data-ttu-id="6aaba-221">A**緩衝處理**資料流是已進入錯誤的狀態; 當資料流處於錯誤狀態，所有傳送至後擋板的訊息將會失敗，資料流不會再判定為失敗之前，立即。</span><span class="sxs-lookup"><span data-stu-id="6aaba-221">A **Buffering** stream is one that has entered a faulted state; when the stream is in the faulted state, all messages sent to the backplane will fail immediately until the stream is no longer faulting.</span></span> <span data-ttu-id="6aaba-222">**傳送佇列長度**是張貼但尚未傳送的訊息數目。</span><span class="sxs-lookup"><span data-stu-id="6aaba-222">The **Send Queue Length** is the number of messages that have been posted but not yet sent.</span></span>

- <span data-ttu-id="6aaba-223">**範圍外訊息匯流排的訊息數/秒**</span><span class="sxs-lookup"><span data-stu-id="6aaba-223">**Scaleout Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="6aaba-224">**向外延展資料流總數**</span><span class="sxs-lookup"><span data-stu-id="6aaba-224">**Scaleout Streams Total**</span></span>
- <span data-ttu-id="6aaba-225">**向外延展資料流開啟**</span><span class="sxs-lookup"><span data-stu-id="6aaba-225">**Scaleout Streams Open**</span></span>
- <span data-ttu-id="6aaba-226">**向外延展資料流緩衝**</span><span class="sxs-lookup"><span data-stu-id="6aaba-226">**Scaleout Streams Buffering**</span></span>
- <span data-ttu-id="6aaba-227">**範圍外錯誤總數**</span><span class="sxs-lookup"><span data-stu-id="6aaba-227">**Scaleout Errors Total**</span></span>
- <span data-ttu-id="6aaba-228">**範圍外錯誤數/秒**</span><span class="sxs-lookup"><span data-stu-id="6aaba-228">**Scaleout Errors/Sec**</span></span>
- <span data-ttu-id="6aaba-229">**範圍外傳送佇列長度**</span><span class="sxs-lookup"><span data-stu-id="6aaba-229">**Scaleout Send Queue Length**</span></span>

<span data-ttu-id="6aaba-230">如需有關這些計數器會測量的詳細資訊，請參閱[SignalR 向外延展與 Azure 服務匯流排](scaleout-with-windows-azure-service-bus.md)。</span><span class="sxs-lookup"><span data-stu-id="6aaba-230">For more information on what these counters are measuring, see [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span></span>

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a><span data-ttu-id="6aaba-231">使用其他的效能計數器</span><span class="sxs-lookup"><span data-stu-id="6aaba-231">Using other performance counters</span></span>

<span data-ttu-id="6aaba-232">下列效能計數器也可能在監視應用程式的效能很有用。</span><span class="sxs-lookup"><span data-stu-id="6aaba-232">The following performance counters may also be useful in monitoring your application's performance.</span></span>

<span data-ttu-id="6aaba-233">**記憶體**</span><span class="sxs-lookup"><span data-stu-id="6aaba-233">**Memory**</span></span>

- <span data-ttu-id="6aaba-234">.NET CLR 記憶體中的位元組數全部堆積 （w3wp)</span><span class="sxs-lookup"><span data-stu-id="6aaba-234">.NET CLR Memory# bytes in all Heaps (for w3wp)</span></span>

<span data-ttu-id="6aaba-235">**ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="6aaba-235">**ASP.NET**</span></span>

- <span data-ttu-id="6aaba-236">Asp.net\requests Current</span><span class="sxs-lookup"><span data-stu-id="6aaba-236">ASP.NET\Requests Current</span></span>
- <span data-ttu-id="6aaba-237">ASP.NET\Queued</span><span class="sxs-lookup"><span data-stu-id="6aaba-237">ASP.NET\Queued</span></span>
- <span data-ttu-id="6aaba-238">ASP.NET\Rejected</span><span class="sxs-lookup"><span data-stu-id="6aaba-238">ASP.NET\Rejected</span></span>

<span data-ttu-id="6aaba-239">**CPU**</span><span class="sxs-lookup"><span data-stu-id="6aaba-239">**CPU**</span></span>

- <span data-ttu-id="6aaba-240">處理器 Information\Processor 時間</span><span class="sxs-lookup"><span data-stu-id="6aaba-240">Processor Information\Processor Time</span></span>

<span data-ttu-id="6aaba-241">**TCP/IP**</span><span class="sxs-lookup"><span data-stu-id="6aaba-241">**TCP/IP**</span></span>

- <span data-ttu-id="6aaba-242">Tcpv6-已/建立的連接</span><span class="sxs-lookup"><span data-stu-id="6aaba-242">TCPv6/Connections Established</span></span>
- <span data-ttu-id="6aaba-243">TCPv4/建立的連接</span><span class="sxs-lookup"><span data-stu-id="6aaba-243">TCPv4/Connections Established</span></span>

<span data-ttu-id="6aaba-244">**Web 服務**</span><span class="sxs-lookup"><span data-stu-id="6aaba-244">**Web Service**</span></span>

- <span data-ttu-id="6aaba-245">Web Service\Current Connections</span><span class="sxs-lookup"><span data-stu-id="6aaba-245">Web Service\Current Connections</span></span>
- <span data-ttu-id="6aaba-246">Web Service\Maximum 連線</span><span class="sxs-lookup"><span data-stu-id="6aaba-246">Web Service\Maximum Connections</span></span>

<span data-ttu-id="6aaba-247">**執行緒處理**</span><span class="sxs-lookup"><span data-stu-id="6aaba-247">**Threading**</span></span>

- <span data-ttu-id="6aaba-248">.NET CLR LocksAndThreads\#個目前的邏輯執行緒</span><span class="sxs-lookup"><span data-stu-id="6aaba-248">.NET CLR LocksAndThreads\# of current logical Threads</span></span>
- <span data-ttu-id="6aaba-249">.NET CLR LocksAnd 執行緒\#的目前實體的執行緒</span><span class="sxs-lookup"><span data-stu-id="6aaba-249">.NET CLR LocksAnd Threads\# of current physical Threads</span></span>

<a id="otherresources"></a>

## <a name="other-resources"></a><span data-ttu-id="6aaba-250">其他資源</span><span class="sxs-lookup"><span data-stu-id="6aaba-250">Other Resources</span></span>

<span data-ttu-id="6aaba-251">如需有關 ASP.NET 的效能監視和調整的詳細資訊，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="6aaba-251">For more information on ASP.NET performance monitoring and tuning, see the following topics:</span></span>

- <span data-ttu-id="6aaba-252">[ASP.NET 效能概觀](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="6aaba-252">[ASP.NET Performance Overview](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span></span>
- [<span data-ttu-id="6aaba-253">IIS 7.5，IIS 7.0 和 IIS 6.0 的 ASP.NET 執行緒用法</span><span class="sxs-lookup"><span data-stu-id="6aaba-253">ASP.NET Thread Usage on IIS 7.5, IIS 7.0, and IIS 6.0</span></span>](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [<span data-ttu-id="6aaba-254">&lt;applicationPool&gt;項目 （Web 設定）</span><span class="sxs-lookup"><span data-stu-id="6aaba-254">&lt;applicationPool&gt; Element (Web Settings)</span></span>](https://msdn.microsoft.com/library/dd560842.aspx)
