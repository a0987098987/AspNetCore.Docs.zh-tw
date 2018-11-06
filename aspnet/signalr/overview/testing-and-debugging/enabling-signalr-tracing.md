---
uid: signalr/overview/testing-and-debugging/enabling-signalr-tracing
title: 啟用 SignalR 追蹤 |Microsoft Docs
author: Rick-Anderson
description: 本文件說明如何啟用和設定追蹤 SignalR 伺服器和用戶端。 追蹤可讓您檢視事件的診斷資訊...
ms.author: riande
ms.date: 08/08/2014
ms.assetid: 30060acb-be3e-4347-996f-3870f0c37829
msc.legacyurl: /signalr/overview/testing-and-debugging/enabling-signalr-tracing
msc.type: authoredcontent
ms.openlocfilehash: 6ab9a5de16a1440d14f7526c0cd417592ba415db
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021322"
---
<a name="enabling-signalr-tracing"></a><span data-ttu-id="440ca-104">啟用 SignalR 追蹤</span><span class="sxs-lookup"><span data-stu-id="440ca-104">Enabling SignalR Tracing</span></span>
====================
<span data-ttu-id="440ca-105">藉由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="440ca-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="440ca-106">本文件說明如何啟用和設定追蹤 SignalR 伺服器和用戶端。</span><span class="sxs-lookup"><span data-stu-id="440ca-106">This document describes how to enable and configure tracing for SignalR servers and clients.</span></span> <span data-ttu-id="440ca-107">追蹤可讓您檢視 SignalR 應用程式中的 診斷事件的資訊。</span><span class="sxs-lookup"><span data-stu-id="440ca-107">Tracing enables you to view diagnostic information about events in your SignalR application.</span></span>
>
> <span data-ttu-id="440ca-108">本主題最初是由 Patrick Fletcher 寫入。</span><span class="sxs-lookup"><span data-stu-id="440ca-108">This topic was originally written by Patrick Fletcher.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="440ca-109">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="440ca-109">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="440ca-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="440ca-110">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="440ca-111">.NET Framework 4.5</span><span class="sxs-lookup"><span data-stu-id="440ca-111">.NET Framework 4.5</span></span>
> - <span data-ttu-id="440ca-112">SignalR 第 2 版</span><span class="sxs-lookup"><span data-stu-id="440ca-112">SignalR version 2</span></span>
>
>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="440ca-113">提出問題或意見</span><span class="sxs-lookup"><span data-stu-id="440ca-113">Questions and comments</span></span>
>
> <span data-ttu-id="440ca-114">您喜歡本教學課程中的方式，和我們可以改善在頁面底部的註解中，歡迎留下意見反應。</span><span class="sxs-lookup"><span data-stu-id="440ca-114">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="440ca-115">如果您有不直接相關的教學課程中的問題，您可以張貼他們[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或是[StackOverflow.com](http://stackoverflow.com/)。</span><span class="sxs-lookup"><span data-stu-id="440ca-115">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="440ca-116">當啟用追蹤時，SignalR 應用程式會建立事件的記錄項目。</span><span class="sxs-lookup"><span data-stu-id="440ca-116">When tracing is enabled, a SignalR application creates log entries for events.</span></span> <span data-ttu-id="440ca-117">您可以從用戶端和伺服器記錄事件。</span><span class="sxs-lookup"><span data-stu-id="440ca-117">You can log events from both the client and the server.</span></span> <span data-ttu-id="440ca-118">追蹤伺服器記錄檔的連接、 向外延展提供者，以及訊息匯流排事件。</span><span class="sxs-lookup"><span data-stu-id="440ca-118">Tracing on the server logs connection, scaleout provider, and message bus events.</span></span> <span data-ttu-id="440ca-119">追蹤用戶端記錄檔的連接事件。</span><span class="sxs-lookup"><span data-stu-id="440ca-119">Tracing on the client logs connection events.</span></span> <span data-ttu-id="440ca-120">SignalR 2.1 和更新版本用戶端上的追蹤記錄中樞叫用訊息的完整內容。</span><span class="sxs-lookup"><span data-stu-id="440ca-120">In SignalR 2.1 and later, tracing on the client logs the full content of hub invocation messages.</span></span>

## <a name="contents"></a><span data-ttu-id="440ca-121">內容</span><span class="sxs-lookup"><span data-stu-id="440ca-121">Contents</span></span>

- [<span data-ttu-id="440ca-122">啟用在伺服器上的追蹤</span><span class="sxs-lookup"><span data-stu-id="440ca-122">Enabling tracing on the server</span></span>](#server)

    - [<span data-ttu-id="440ca-123">伺服器事件記錄到文字檔</span><span class="sxs-lookup"><span data-stu-id="440ca-123">Logging server events to text files</span></span>](#server_text)
    - [<span data-ttu-id="440ca-124">伺服器事件記錄到事件記錄檔</span><span class="sxs-lookup"><span data-stu-id="440ca-124">Logging server events to the event log</span></span>](#server_eventlog)
- [<span data-ttu-id="440ca-125">在.NET 用戶端 （Windows 桌面應用程式） 中啟用追蹤</span><span class="sxs-lookup"><span data-stu-id="440ca-125">Enabling tracing in the .NET client (Windows Desktop apps)</span></span>](#net_client)

    - [<span data-ttu-id="440ca-126">桌面用戶端事件記錄到主控台</span><span class="sxs-lookup"><span data-stu-id="440ca-126">Logging Desktop client events to the console</span></span>](#desktop_console)
    - [<span data-ttu-id="440ca-127">桌面用戶端事件記錄到文字檔</span><span class="sxs-lookup"><span data-stu-id="440ca-127">Logging Desktop client events to a text file</span></span>](#desktop_text)
- [<span data-ttu-id="440ca-128">在 Windows Phone 8 的用戶端中啟用追蹤</span><span class="sxs-lookup"><span data-stu-id="440ca-128">Enabling tracing in Windows Phone 8 clients</span></span>](#phone)

    - [<span data-ttu-id="440ca-129">Windows Phone 用戶端事件記錄到 UI</span><span class="sxs-lookup"><span data-stu-id="440ca-129">Logging Windows Phone client events to the UI</span></span>](#phone_ui)
    - [<span data-ttu-id="440ca-130">Windows Phone 用戶端事件記錄到偵錯主控台</span><span class="sxs-lookup"><span data-stu-id="440ca-130">Logging Windows Phone client events to the debug console</span></span>](#phone_debug)
- [<span data-ttu-id="440ca-131">在 JavaScript 用戶端中啟用追蹤</span><span class="sxs-lookup"><span data-stu-id="440ca-131">Enabling tracing in the JavaScript client</span></span>](#javascript)

<a id="server"></a>
## <a name="enabling-tracing-on-the-server"></a><span data-ttu-id="440ca-132">啟用在伺服器上的追蹤</span><span class="sxs-lookup"><span data-stu-id="440ca-132">Enabling tracing on the server</span></span>

<span data-ttu-id="440ca-133">啟用應用程式的組態檔 （App.config 或 Web.config，視專案類型而定。） 內的伺服器上的追蹤您指定您想要記錄哪些事件的類別。</span><span class="sxs-lookup"><span data-stu-id="440ca-133">You enable tracing on the server within the application's configuration file (either App.config or Web.config depending on the type of project.) You specify which categories of events you want to log.</span></span> <span data-ttu-id="440ca-134">在組態檔中，您也指定是否將事件記錄到文字檔、 Windows 事件記錄檔或使用的實作自訂記錄檔[TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx)。</span><span class="sxs-lookup"><span data-stu-id="440ca-134">In the configuration file, you also specify whether to log the events to a text file, the Windows event log, or a custom log using an implementation of [TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx).</span></span>

<span data-ttu-id="440ca-135">伺服器事件類別目錄包含下列類型的訊息：</span><span class="sxs-lookup"><span data-stu-id="440ca-135">The server event categories include the following sorts of messages:</span></span>

| <span data-ttu-id="440ca-136">原始程式檔</span><span class="sxs-lookup"><span data-stu-id="440ca-136">Source</span></span> | <span data-ttu-id="440ca-137">訊息</span><span class="sxs-lookup"><span data-stu-id="440ca-137">Messages</span></span> |
| --- | --- |
| <span data-ttu-id="440ca-138">SignalR.SqlMessageBus</span><span class="sxs-lookup"><span data-stu-id="440ca-138">SignalR.SqlMessageBus</span></span> | <span data-ttu-id="440ca-139">SQL 訊息匯流排範圍外的提供者安裝程式、 資料庫作業、 錯誤和逾時事件</span><span class="sxs-lookup"><span data-stu-id="440ca-139">SQL Message Bus scaleout provider setup, database operation, error, and timeout events</span></span> |
| <span data-ttu-id="440ca-140">SignalR.ServiceBusMessageBus</span><span class="sxs-lookup"><span data-stu-id="440ca-140">SignalR.ServiceBusMessageBus</span></span> | <span data-ttu-id="440ca-141">服務匯流排範圍外提供者建立主題和訂用帳戶、 錯誤和訊息事件</span><span class="sxs-lookup"><span data-stu-id="440ca-141">Service bus scaleout provider topic creation and subscription, error, and messaging events</span></span> |
| <span data-ttu-id="440ca-142">SignalR.RedisMessageBus</span><span class="sxs-lookup"><span data-stu-id="440ca-142">SignalR.RedisMessageBus</span></span> | <span data-ttu-id="440ca-143">Redis 範圍外提供者的連接、 中斷連線和錯誤事件</span><span class="sxs-lookup"><span data-stu-id="440ca-143">Redis scaleout provider connection, disconnection, and error events</span></span> |
| <span data-ttu-id="440ca-144">SignalR.ScaleoutMessageBus</span><span class="sxs-lookup"><span data-stu-id="440ca-144">SignalR.ScaleoutMessageBus</span></span> | <span data-ttu-id="440ca-145">向外延展訊息事件</span><span class="sxs-lookup"><span data-stu-id="440ca-145">Scaleout messaging events</span></span> |
| <span data-ttu-id="440ca-146">SignalR.Transports.WebSocketTransport</span><span class="sxs-lookup"><span data-stu-id="440ca-146">SignalR.Transports.WebSocketTransport</span></span> | <span data-ttu-id="440ca-147">WebSocket 傳輸連線、 中斷連線、 訊息和錯誤事件</span><span class="sxs-lookup"><span data-stu-id="440ca-147">WebSocket transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="440ca-148">SignalR.Transports.ServerSentEventsTransport</span><span class="sxs-lookup"><span data-stu-id="440ca-148">SignalR.Transports.ServerSentEventsTransport</span></span> | <span data-ttu-id="440ca-149">ServerSentEvents 傳輸連線、 中斷連線、 訊息和錯誤事件</span><span class="sxs-lookup"><span data-stu-id="440ca-149">ServerSentEvents transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="440ca-150">SignalR.Transports.ForeverFrameTransport</span><span class="sxs-lookup"><span data-stu-id="440ca-150">SignalR.Transports.ForeverFrameTransport</span></span> | <span data-ttu-id="440ca-151">ForeverFrame 傳輸連線、 中斷連線、 訊息和錯誤事件</span><span class="sxs-lookup"><span data-stu-id="440ca-151">ForeverFrame transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="440ca-152">SignalR.Transports.LongPollingTransport</span><span class="sxs-lookup"><span data-stu-id="440ca-152">SignalR.Transports.LongPollingTransport</span></span> | <span data-ttu-id="440ca-153">LongPolling 傳輸連線、 中斷連線、 訊息和錯誤事件</span><span class="sxs-lookup"><span data-stu-id="440ca-153">LongPolling transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="440ca-154">SignalR.Transports.TransportHeartBeat</span><span class="sxs-lookup"><span data-stu-id="440ca-154">SignalR.Transports.TransportHeartBeat</span></span> | <span data-ttu-id="440ca-155">傳輸連線、 中斷連線和 keepalive 事件</span><span class="sxs-lookup"><span data-stu-id="440ca-155">Transport connection, disconnection, and keepalive events</span></span> |
| <span data-ttu-id="440ca-156">SignalR.ReflectedHubDescriptorProvider</span><span class="sxs-lookup"><span data-stu-id="440ca-156">SignalR.ReflectedHubDescriptorProvider</span></span> | <span data-ttu-id="440ca-157">中樞探索事件</span><span class="sxs-lookup"><span data-stu-id="440ca-157">Hub discovery events</span></span> |

<a id="server_text"></a>
### <a name="logging-server-events-to-text-files"></a><span data-ttu-id="440ca-158">伺服器事件記錄到文字檔</span><span class="sxs-lookup"><span data-stu-id="440ca-158">Logging server events to text files</span></span>

<span data-ttu-id="440ca-159">下列程式碼示範如何啟用追蹤每個事件類別。</span><span class="sxs-lookup"><span data-stu-id="440ca-159">The following code shows how to enable tracing for each category of event.</span></span> <span data-ttu-id="440ca-160">此範例會設定應用程式事件記錄到文字檔。</span><span class="sxs-lookup"><span data-stu-id="440ca-160">This sample configures the application to log events to text files.</span></span>

<span data-ttu-id="440ca-161">**XML 伺服器程式碼啟用追蹤**</span><span class="sxs-lookup"><span data-stu-id="440ca-161">**XML server code for enabling tracing**</span></span>

[!code-html[Main](enabling-signalr-tracing/samples/sample1.html)]

<span data-ttu-id="440ca-162">在上述程式碼`SignalRSwitch`項目會指定[TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx)用於傳送至指定的記錄檔的事件。</span><span class="sxs-lookup"><span data-stu-id="440ca-162">In the code above, the `SignalRSwitch` entry specifies the [TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx) used for events sent to the specified log.</span></span> <span data-ttu-id="440ca-163">在此情況下，它會設定為`Verbose`這表示所有的偵錯和追蹤訊息會記錄。</span><span class="sxs-lookup"><span data-stu-id="440ca-163">In this case, it is set to `Verbose` which means all debugging and tracing messages are logged.</span></span>

<span data-ttu-id="440ca-164">下列輸出顯示的項目`transports.log.txt`使用上述的組態檔的應用程式的檔案。</span><span class="sxs-lookup"><span data-stu-id="440ca-164">The following output shows entries from the `transports.log.txt` file for an application using the above configuration file.</span></span> <span data-ttu-id="440ca-165">它會顯示新的連線、 已移除的連線和傳輸活動訊號事件。</span><span class="sxs-lookup"><span data-stu-id="440ca-165">It shows a new connection, a removed connection, and transport heartbeat events.</span></span>

[!code-console[Main](enabling-signalr-tracing/samples/sample2.cmd)]

<a id="server_eventlog"></a>
### <a name="logging-server-events-to-the-event-log"></a><span data-ttu-id="440ca-166">伺服器事件記錄到事件記錄檔</span><span class="sxs-lookup"><span data-stu-id="440ca-166">Logging server events to the event log</span></span>

<span data-ttu-id="440ca-167">若要將事件記錄到事件記錄檔，而不是文字檔案，變更的值中的項目`sharedListeners`節點。</span><span class="sxs-lookup"><span data-stu-id="440ca-167">To log events to the event log rather than a text file, change the values for the entries in the `sharedListeners` node.</span></span> <span data-ttu-id="440ca-168">下列程式碼顯示如何將伺服器事件記錄到事件記錄檔：</span><span class="sxs-lookup"><span data-stu-id="440ca-168">The following code shows how to log server events to the event log:</span></span>

<span data-ttu-id="440ca-169">**事件記錄到事件記錄檔的 XML 伺服器程式碼**</span><span class="sxs-lookup"><span data-stu-id="440ca-169">**XML server code for logging events to the event log**</span></span>

[!code-xml[Main](enabling-signalr-tracing/samples/sample3.xml)]

<span data-ttu-id="440ca-170">事件會記錄在應用程式記錄檔中，而且都是透過事件檢視器中，如下所示：</span><span class="sxs-lookup"><span data-stu-id="440ca-170">The events are logged in the Application log, and are available through the Event Viewer, as shown below:</span></span>

![顯示 SignalR 記錄的事件檢視器](enabling-signalr-tracing/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="440ca-172">當使用事件記錄檔，設定**TraceLevel**來**錯誤**將可管理的訊息數目。</span><span class="sxs-lookup"><span data-stu-id="440ca-172">When using the event log, set the **TraceLevel** to **Error** to keep the number of messages manageable.</span></span>

<a id="net_client"></a>
## <a name="enabling-tracing-in-the-net-client-windows-desktop-apps"></a><span data-ttu-id="440ca-173">在.NET 用戶端 （Windows 桌面應用程式） 中啟用追蹤</span><span class="sxs-lookup"><span data-stu-id="440ca-173">Enabling tracing in the .NET client (Windows Desktop apps)</span></span>

<span data-ttu-id="440ca-174">.NET 用戶端可以記錄事件，主控台中，文字檔案，或使用的實作自訂記錄檔[TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx)。</span><span class="sxs-lookup"><span data-stu-id="440ca-174">The .NET client can log events to the console, a text file, or to a custom log using an implementation of [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).</span></span>

<span data-ttu-id="440ca-175">若要啟用登入的.NET 用戶端，設定連接的`TraceLevel`屬性，以[TraceLevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx)的值，而`TraceWriter`屬性設為有效[TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx)執行個體。</span><span class="sxs-lookup"><span data-stu-id="440ca-175">To enable logging in the .NET client, set the connection's `TraceLevel` property to a [TraceLevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) value, and the `TraceWriter` property to a valid [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) instance.</span></span>

<a id="desktop_console"></a>
### <a name="logging-desktop-client-events-to-the-console"></a><span data-ttu-id="440ca-176">桌面用戶端事件記錄到主控台</span><span class="sxs-lookup"><span data-stu-id="440ca-176">Logging Desktop client events to the console</span></span>

<span data-ttu-id="440ca-177">下列 C# 程式碼示範如何在.NET 用戶端主控台中記錄事件：</span><span class="sxs-lookup"><span data-stu-id="440ca-177">The following C# code shows how to log events in the .NET client to the console:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample4.cs?highlight=2-3)]

<a id="desktop_text"></a>
### <a name="logging-desktop-client-events-to-a-text-file"></a><span data-ttu-id="440ca-178">桌面用戶端事件記錄到文字檔</span><span class="sxs-lookup"><span data-stu-id="440ca-178">Logging Desktop client events to a text file</span></span>

<span data-ttu-id="440ca-179">下列 C# 程式碼示範如何在.NET 用戶端至文字檔案中記錄事件：</span><span class="sxs-lookup"><span data-stu-id="440ca-179">The following C# code shows how to log events in the .NET client to a text file:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample5.cs?highlight=4-5)]

<span data-ttu-id="440ca-180">下列輸出顯示的項目`ClientLog.txt`使用上述的組態檔的應用程式的檔案。</span><span class="sxs-lookup"><span data-stu-id="440ca-180">The following output shows entries from the `ClientLog.txt` file for an application using the above configuration file.</span></span> <span data-ttu-id="440ca-181">它會顯示用戶端連接到伺服器，而且中樞叫用用戶端方法呼叫`addMessage`:</span><span class="sxs-lookup"><span data-stu-id="440ca-181">It shows the client connecting to the server, and the hub invoking a client method called `addMessage`:</span></span>

[!code-console[Main](enabling-signalr-tracing/samples/sample6.cmd)]

<a id="phone"></a>
## <a name="enabling-tracing-in-windows-phone-8-clients"></a><span data-ttu-id="440ca-182">在 Windows Phone 8 的用戶端中啟用追蹤</span><span class="sxs-lookup"><span data-stu-id="440ca-182">Enabling tracing in Windows Phone 8 clients</span></span>

<span data-ttu-id="440ca-183">Windows Phone 應用程式的 SignalR 應用程式相同的.NET 用戶端做為桌面應用程式，但[Console.Out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx)和寫入的檔案[StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx)不提供。</span><span class="sxs-lookup"><span data-stu-id="440ca-183">SignalR applications for Windows Phone apps use the same .NET client as desktop apps, but [Console.Out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx) and writing to a file with [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) are not available.</span></span> <span data-ttu-id="440ca-184">相反地，您需要建立的自訂實作[TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx)進行追蹤。</span><span class="sxs-lookup"><span data-stu-id="440ca-184">Instead, you need to create a custom implementation of [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) for tracing.</span></span>

<a id="phone_ui"></a>
### <a name="logging-windows-phone-client-events-to-the-ui"></a><span data-ttu-id="440ca-185">Windows Phone 用戶端事件記錄到 UI</span><span class="sxs-lookup"><span data-stu-id="440ca-185">Logging Windows Phone client events to the UI</span></span>

<span data-ttu-id="440ca-186">[SignalR 程式碼基底](https://github.com/SignalR/SignalR/archive/master.zip)包含寫入追蹤輸出的 Windows Phone 範例[TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx)使用自訂[TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx)稱為實作`TextBlockWriter`。</span><span class="sxs-lookup"><span data-stu-id="440ca-186">The [SignalR codebase](https://github.com/SignalR/SignalR/archive/master.zip) includes a Windows Phone sample that writes trace output to a [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) using a custom [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) implementation called `TextBlockWriter`.</span></span> <span data-ttu-id="440ca-187">這個類別可在**samples/Microsoft.AspNet.SignalR.Client.WP8.Samples**專案。</span><span class="sxs-lookup"><span data-stu-id="440ca-187">This class can be found in the **samples/Microsoft.AspNet.SignalR.Client.WP8.Samples** project.</span></span> <span data-ttu-id="440ca-188">建立的執行個體時`TextBlockWriter`，傳入目前[SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx)，以及[StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx)它將在其中建立[TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx)来用於追蹤輸出：</span><span class="sxs-lookup"><span data-stu-id="440ca-188">When creating an instance of `TextBlockWriter`, pass in the current [SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx), and a [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) where it will create a [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) to use for trace output:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample7.cs)]

<span data-ttu-id="440ca-189">追蹤輸出則會寫入至新[TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx)中建立[StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx)您傳入：</span><span class="sxs-lookup"><span data-stu-id="440ca-189">The trace output will then be written to a new [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) created in the [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) you passed in:</span></span>

![](enabling-signalr-tracing/_static/image2.png)

<a id="phone_debug"></a>
### <a name="logging-windows-phone-client-events-to-the-debug-console"></a><span data-ttu-id="440ca-190">Windows Phone 用戶端事件記錄到偵錯主控台</span><span class="sxs-lookup"><span data-stu-id="440ca-190">Logging Windows Phone client events to the debug console</span></span>

<span data-ttu-id="440ca-191">若要將輸出傳送至偵錯主控台，而不是 UI 中，建立 [的實作[TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) ，將寫入至偵錯] 視窗中，並將它指派給您的連線[TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx)屬性：</span><span class="sxs-lookup"><span data-stu-id="440ca-191">To send output to the debug console rather than the UI, create an implementation of [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) that writes to the debug window, and assign it to your connection's [TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) property:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample8.cs)]

<span data-ttu-id="440ca-192">然後在 Visual Studio 中的 [偵錯] 視窗會寫入追蹤資訊：</span><span class="sxs-lookup"><span data-stu-id="440ca-192">Trace information will then be written to the debug window in Visual Studio:</span></span>

![](enabling-signalr-tracing/_static/image3.png)

<a id="javascript"></a>
## <a name="enabling-tracing-in-the-javascript-client"></a><span data-ttu-id="440ca-193">在 JavaScript 用戶端中啟用追蹤</span><span class="sxs-lookup"><span data-stu-id="440ca-193">Enabling tracing in the JavaScript client</span></span>

<span data-ttu-id="440ca-194">若要啟用用戶端登入連線，請設定`logging`屬性上的連接物件，然後再呼叫`start`方法來建立連線。</span><span class="sxs-lookup"><span data-stu-id="440ca-194">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="440ca-195">**用戶端 JavaScript 程式碼啟用追蹤，以瀏覽器主控台 （與產生的 proxy)**</span><span class="sxs-lookup"><span data-stu-id="440ca-195">**Client JavaScript code for enabling tracing to the browser console (with the generated proxy)**</span></span>

[!code-javascript[Main](enabling-signalr-tracing/samples/sample9.js?highlight=1)]

<span data-ttu-id="440ca-196">**用戶端 JavaScript 程式碼啟用追蹤，以瀏覽器主控台 （不含產生的 proxy)**</span><span class="sxs-lookup"><span data-stu-id="440ca-196">**Client JavaScript code for enabling tracing to the browser console (without the generated proxy)**</span></span>

[!code-javascript[Main](enabling-signalr-tracing/samples/sample10.js?highlight=2)]

<span data-ttu-id="440ca-197">當啟用追蹤時，JavaScript 用戶端會記錄事件到瀏覽器主控台。</span><span class="sxs-lookup"><span data-stu-id="440ca-197">When tracing is enabled, the JavaScript client logs events to the browser console.</span></span> <span data-ttu-id="440ca-198">若要存取瀏覽器主控台，請參閱[監視的傳輸](../getting-started/introduction-to-signalr.md#MonitoringTransports)。</span><span class="sxs-lookup"><span data-stu-id="440ca-198">To access the browser console, see [Monitoring Transports](../getting-started/introduction-to-signalr.md#MonitoringTransports).</span></span>

<span data-ttu-id="440ca-199">下列螢幕擷取畫面顯示 SignalR JavaScript 用戶端已啟用追蹤。</span><span class="sxs-lookup"><span data-stu-id="440ca-199">The following screenshot shows a SignalR JavaScript client with tracing enabled.</span></span> <span data-ttu-id="440ca-200">它在瀏覽器主控台中會顯示連線和中樞叫用事件：</span><span class="sxs-lookup"><span data-stu-id="440ca-200">It shows connection and hub invocation events in the browser console:</span></span>

![在瀏覽器主控台中的 SignalR 追蹤事件](enabling-signalr-tracing/_static/image4.png)
