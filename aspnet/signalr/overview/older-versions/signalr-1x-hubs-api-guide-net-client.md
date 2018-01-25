---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
title: "ASP.NET SignalR 中樞 API 指南-.NET 用戶端 (SignalR 1.x) |Microsoft 文件"
author: pfletcher
description: "本文件提供適用於 SignalR.NET 用戶端，例如 Windows 市集 (WinRT)、 WPF、 Silverlight 和優缺點比較的第 2 版使用集線器 API 的簡介..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/17/2013
ms.topic: article
ms.assetid: c334adc3-d6dc-44f3-9f06-f7634475aad3
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: 9a61bd255a217876aa2fdbeb6389539483b9f013
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-signalr-hubs-api-guide---net-client-signalr-1x"></a><span data-ttu-id="eccac-103">ASP.NET SignalR 中樞 API 指南-.NET 用戶端 (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="eccac-103">ASP.NET SignalR Hubs API Guide - .NET Client (SignalR 1.x)</span></span>
====================
<span data-ttu-id="eccac-104">由[Patrick Fletcher](https://github.com/pfletcher)， [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="eccac-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="eccac-105">本文件提供適用於 SignalR.NET 用戶端，例如 Windows 市集 (WinRT)、 WPF、 Silverlight 和主控台應用程式中的第 2 版使用集線器 API 的簡介。</span><span class="sxs-lookup"><span data-stu-id="eccac-105">This document provides an introduction to using the Hubs API for SignalR version 2 in .NET clients, such as Windows Store (WinRT), WPF, Silverlight, and console applications.</span></span>
> 
> <span data-ttu-id="eccac-106">SignalR 中樞應用程式開發介面可讓您從伺服器連線的用戶端和伺服器的用戶端進行遠端程序呼叫 (Rpc)。</span><span class="sxs-lookup"><span data-stu-id="eccac-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="eccac-107">伺服端程式碼，並定義用戶端，可以呼叫的方法，呼叫用戶端執行的方法。</span><span class="sxs-lookup"><span data-stu-id="eccac-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="eccac-108">用戶端程式碼，並定義可在伺服器上，從呼叫的方法，您呼叫在伺服器執行的方法。</span><span class="sxs-lookup"><span data-stu-id="eccac-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="eccac-109">SignalR 會負責為您的用戶端-伺服器一切細節。</span><span class="sxs-lookup"><span data-stu-id="eccac-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="eccac-110">SignalR 也能提供較低層級應用程式開發介面呼叫持續連線。</span><span class="sxs-lookup"><span data-stu-id="eccac-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="eccac-111">如需簡介 SignalR、 集線器及持續連線，或示範如何建置完整的 SignalR 應用程式的教學課程，請參閱[SignalR-快速入門](../getting-started/index.md)。</span><span class="sxs-lookup"><span data-stu-id="eccac-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>


## <a name="overview"></a><span data-ttu-id="eccac-112">總覽</span><span class="sxs-lookup"><span data-stu-id="eccac-112">Overview</span></span>

<span data-ttu-id="eccac-113">本文件包含下列章節：</span><span class="sxs-lookup"><span data-stu-id="eccac-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="eccac-114">用戶端安裝</span><span class="sxs-lookup"><span data-stu-id="eccac-114">Client Setup</span></span>](#clientsetup)
- [<span data-ttu-id="eccac-115">如何建立連接</span><span class="sxs-lookup"><span data-stu-id="eccac-115">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="eccac-116">Silverlight 用戶端進行跨網域連線</span><span class="sxs-lookup"><span data-stu-id="eccac-116">Cross-domain connections from Silverlight clients</span></span>](#slcrossdomain)
- [<span data-ttu-id="eccac-117">如何設定連接</span><span class="sxs-lookup"><span data-stu-id="eccac-117">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="eccac-118">如何在 WPF 用戶端中設定的同時連線數目上限</span><span class="sxs-lookup"><span data-stu-id="eccac-118">How to set the maximum number of concurrent connections in WPF clients</span></span>](#maxconnections)
    - [<span data-ttu-id="eccac-119">如何指定查詢字串參數</span><span class="sxs-lookup"><span data-stu-id="eccac-119">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="eccac-120">如何指定傳輸方法</span><span class="sxs-lookup"><span data-stu-id="eccac-120">How to specify the transport method</span></span>](#transport)
    - [<span data-ttu-id="eccac-121">如何指定 HTTP 標頭</span><span class="sxs-lookup"><span data-stu-id="eccac-121">How to specify HTTP headers</span></span>](#httpheaders)
    - [<span data-ttu-id="eccac-122">如何指定用戶端憑證</span><span class="sxs-lookup"><span data-stu-id="eccac-122">How to specify client certificates</span></span>](#clientcertificate)
- [<span data-ttu-id="eccac-123">如何建立中樞 proxy</span><span class="sxs-lookup"><span data-stu-id="eccac-123">How to create the Hub proxy</span></span>](#proxy)
- [<span data-ttu-id="eccac-124">如何定義伺服器可以呼叫的用戶端上的方法</span><span class="sxs-lookup"><span data-stu-id="eccac-124">How to define methods on the client that the server can call</span></span>](#callclient)

    - [<span data-ttu-id="eccac-125">不含參數的方法</span><span class="sxs-lookup"><span data-stu-id="eccac-125">Methods without parameters</span></span>](#clientmethodswithoutparms)
    - [<span data-ttu-id="eccac-126">具有指定參數型別參數的方法</span><span class="sxs-lookup"><span data-stu-id="eccac-126">Methods with parameters, specifying parameter types</span></span>](#clientmethodswithparmtypes)
    - [<span data-ttu-id="eccac-127">具有指定之參數的動態物件參數的方法</span><span class="sxs-lookup"><span data-stu-id="eccac-127">Methods with parameters, specifying dynamic objects for the parameters</span></span>](#clientmethodswithdynamparms)
    - [<span data-ttu-id="eccac-128">如何移除處理常式</span><span class="sxs-lookup"><span data-stu-id="eccac-128">How to remove a handler</span></span>](#removehandler)
- [<span data-ttu-id="eccac-129">如何從用戶端呼叫伺服器方法</span><span class="sxs-lookup"><span data-stu-id="eccac-129">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="eccac-130">如何處理連接的存留期事件</span><span class="sxs-lookup"><span data-stu-id="eccac-130">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="eccac-131">如何處理錯誤</span><span class="sxs-lookup"><span data-stu-id="eccac-131">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="eccac-132">如何啟用用戶端記錄</span><span class="sxs-lookup"><span data-stu-id="eccac-132">How to enable client-side logging</span></span>](#logging)
- [<span data-ttu-id="eccac-133">WPF、 Silverlight 和主控台應用程式程式碼可以呼叫伺服器的用戶端方法的範例</span><span class="sxs-lookup"><span data-stu-id="eccac-133">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>](#wpfsl)

<span data-ttu-id="eccac-134">如範例.NET 用戶端專案中，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="eccac-134">For a sample .NET client projects, see the following resources:</span></span>

- <span data-ttu-id="eccac-135">[羅 armenta / SignalR 範例](https://github.com/gustavo-armenta/SignalR-Samples)上 GitHub.com （WinRT，Silverlight，主控台應用程式範例）。</span><span class="sxs-lookup"><span data-stu-id="eccac-135">[gustavo-armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) on GitHub.com (WinRT, Silverlight, console app examples).</span></span>
- <span data-ttu-id="eccac-136">[DamianEdwards / SignalR MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) GitHub.com （例如，WPF） 上。</span><span class="sxs-lookup"><span data-stu-id="eccac-136">[DamianEdwards / SignalR-MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) on GitHub.com (WPF example).</span></span>
- <span data-ttu-id="eccac-137">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples)上 GitHub.com （主控台應用程式範例）。</span><span class="sxs-lookup"><span data-stu-id="eccac-137">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) on GitHub.com (Console app example).</span></span>

<span data-ttu-id="eccac-138">如需如何將伺服器或 JavaScript 用戶端進行程式設計的文件，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="eccac-138">For documentation on how to program the server or JavaScript clients, see the following resources:</span></span>

- [<span data-ttu-id="eccac-139">SignalR 中樞 API 指南-伺服器</span><span class="sxs-lookup"><span data-stu-id="eccac-139">SignalR Hubs API Guide - Server</span></span>](../guide-to-the-api/hubs-api-guide-server.md)
- [<span data-ttu-id="eccac-140">SignalR 中樞 API 指南 JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="eccac-140">SignalR Hubs API Guide - JavaScript Client</span></span>](../guide-to-the-api/hubs-api-guide-javascript-client.md)

<span data-ttu-id="eccac-141">應用程式開發介面參考主題的連結是.NET 4.5 版的 API。</span><span class="sxs-lookup"><span data-stu-id="eccac-141">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="eccac-142">如果您使用.NET 4，請參閱[API 主題.NET 4 版本](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx)。</span><span class="sxs-lookup"><span data-stu-id="eccac-142">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="eccac-143">用戶端安裝</span><span class="sxs-lookup"><span data-stu-id="eccac-143">Client setup</span></span>

<span data-ttu-id="eccac-144">安裝[Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet 封裝 (不[Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr)封裝)。</span><span class="sxs-lookup"><span data-stu-id="eccac-144">Install the [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet package (not the [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) package).</span></span> <span data-ttu-id="eccac-145">此套件支援 WinRT、 Silverlight、 WPF、 主控台應用程式和 Windows Phone 用戶端，.NET 4 和.NET 4.5。</span><span class="sxs-lookup"><span data-stu-id="eccac-145">This package supports WinRT, Silverlight, WPF, console application, and Windows Phone clients, for both .NET 4 and .NET 4.5.</span></span>

<span data-ttu-id="eccac-146">如果您有在用戶端的 SignalR 的版本是與您在伺服器擁有的版本不同，SignalR 是通常可以調整的差異。</span><span class="sxs-lookup"><span data-stu-id="eccac-146">If the version of SignalR that you have on the client is different from the version that you have on the server, SignalR is often able to adapt to the difference.</span></span> <span data-ttu-id="eccac-147">比方說，釋放 SignalR 2.0 版時，在伺服器上安裝，伺服器將會支援具有 1.1.x 以及具有 2.0 安裝的用戶端安裝的用戶端。</span><span class="sxs-lookup"><span data-stu-id="eccac-147">For example, when SignalR version 2.0 is released and you install that on the server, the server will support clients that have 1.1.x installed as well as clients that have 2.0 installed.</span></span> <span data-ttu-id="eccac-148">如果伺服器上的版本與用戶端上的版本之間的差異太大，會擲回 SignalR`InvalidOperationException`用戶端會嘗試建立連接時的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="eccac-148">If the difference between the version on the server and the version on the client is too great, SignalR throws an `InvalidOperationException` exception when the client tries to establish a connection.</span></span> <span data-ttu-id="eccac-149">錯誤訊息是 「`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`"。</span><span class="sxs-lookup"><span data-stu-id="eccac-149">The error message is "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="eccac-150">如何建立連接</span><span class="sxs-lookup"><span data-stu-id="eccac-150">How to establish a connection</span></span>

<span data-ttu-id="eccac-151">您可以建立連線之前，您必須建立`HubConnection`物件，並建立 proxy。</span><span class="sxs-lookup"><span data-stu-id="eccac-151">Before you can establish a connection, you have to create a `HubConnection` object and create a proxy.</span></span> <span data-ttu-id="eccac-152">若要建立連接，呼叫`Start`方法`HubConnection`物件。</span><span class="sxs-lookup"><span data-stu-id="eccac-152">To establish the connection, call the `Start` method on the `HubConnection` object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> <span data-ttu-id="eccac-153">您必須註冊至少一個事件處理常式，然後再呼叫 JavaScript 用戶端`Start`方法，以建立連接。</span><span class="sxs-lookup"><span data-stu-id="eccac-153">For JavaScript clients you have to register at least one event handler before calling the `Start` method to establish the connection.</span></span> <span data-ttu-id="eccac-154">這是不必要的.NET 用戶端。</span><span class="sxs-lookup"><span data-stu-id="eccac-154">This is not necessary for .NET clients.</span></span> <span data-ttu-id="eccac-155">JavaScript 用戶端，產生的 proxy 程式碼會自動建立存在的所有主機的 proxy 的伺服器上，以及註冊處理常式是哪一個中心指示您的用戶端所要使用。</span><span class="sxs-lookup"><span data-stu-id="eccac-155">For JavaScript clients, the generated proxy code automatically creates proxies for all Hubs that exist on the server, and registering a handler is how you indicate which Hubs your client intends to use.</span></span> <span data-ttu-id="eccac-156">但.NET 用戶端您中樞 proxy 手動建立，因此 SignalR 假設您將會使用任何您建立的 proxy 的中樞。</span><span class="sxs-lookup"><span data-stu-id="eccac-156">But for a .NET client you create Hub proxies manually, so SignalR assumes that you will be using any Hub that you create a proxy for.</span></span>


<span data-ttu-id="eccac-157">範例程式碼會使用預設 「 / signalr 」 來連線到 SignalR 服務的 URL。</span><span class="sxs-lookup"><span data-stu-id="eccac-157">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="eccac-158">如需如何指定不同的基底 URL，請參閱[ASP.NET SignalR 中樞 API 指南-伺服器-/signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl)。</span><span class="sxs-lookup"><span data-stu-id="eccac-158">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="eccac-159">`Start`方法以非同步方式執行。</span><span class="sxs-lookup"><span data-stu-id="eccac-159">The `Start` method executes asynchronously.</span></span> <span data-ttu-id="eccac-160">若要確定連線建立之後，後續幾行程式碼不執行之前，請使用`await`ASP.NET 4.5 非同步方法中或`.Wait()`同步方法中。</span><span class="sxs-lookup"><span data-stu-id="eccac-160">To make sure that subsequent lines of code don't execute until after the connection is established, use `await` in an ASP.NET 4.5 asynchronous method or `.Wait()` in a synchronous method.</span></span> <span data-ttu-id="eccac-161">請勿使用`.Wait()`WinRT 用戶端中。</span><span class="sxs-lookup"><span data-stu-id="eccac-161">Don't use `.Wait()` in a WinRT client.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<span data-ttu-id="eccac-162">`HubConnection` 類別是安全執行緒。</span><span class="sxs-lookup"><span data-stu-id="eccac-162">The `HubConnection` class is thread-safe.</span></span>

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a><span data-ttu-id="eccac-163">Silverlight 用戶端進行跨網域連線</span><span class="sxs-lookup"><span data-stu-id="eccac-163">Cross-domain connections from Silverlight clients</span></span>

<span data-ttu-id="eccac-164">如需如何啟用跨網域 Silverlight 用戶端的連接資訊，請參閱[讓服務提供跨網域界限](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx)。</span><span class="sxs-lookup"><span data-stu-id="eccac-164">For information about how to enable cross-domain connections from Silverlight clients, see [Making a Service Available Across Domain Boundaries](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span></span>

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="eccac-165">如何設定連接</span><span class="sxs-lookup"><span data-stu-id="eccac-165">How to configure the connection</span></span>

<span data-ttu-id="eccac-166">建立連線之前，您可以指定下列選項：</span><span class="sxs-lookup"><span data-stu-id="eccac-166">Before you establish a connection, you can specify any of the following options:</span></span>

- <span data-ttu-id="eccac-167">並行連線限制。</span><span class="sxs-lookup"><span data-stu-id="eccac-167">Concurrent connections limit.</span></span>
- <span data-ttu-id="eccac-168">查詢字串參數。</span><span class="sxs-lookup"><span data-stu-id="eccac-168">Query string parameters.</span></span>
- <span data-ttu-id="eccac-169">傳輸方法。</span><span class="sxs-lookup"><span data-stu-id="eccac-169">The transport method.</span></span>
- <span data-ttu-id="eccac-170">HTTP 標頭。</span><span class="sxs-lookup"><span data-stu-id="eccac-170">HTTP headers.</span></span>
- <span data-ttu-id="eccac-171">用戶端憑證。</span><span class="sxs-lookup"><span data-stu-id="eccac-171">Client certificates.</span></span>

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a><span data-ttu-id="eccac-172">如何在 WPF 用戶端中設定的同時連線數目上限</span><span class="sxs-lookup"><span data-stu-id="eccac-172">How to set the maximum number of concurrent connections in WPF clients</span></span>

<span data-ttu-id="eccac-173">在 WPF 用戶端，您可能必須增加的非預設值為 2 的並行連線數目上限。</span><span class="sxs-lookup"><span data-stu-id="eccac-173">In WPF clients, you might have to increase the maximum number of concurrent connections from its default value of 2.</span></span> <span data-ttu-id="eccac-174">建議的值為 10。</span><span class="sxs-lookup"><span data-stu-id="eccac-174">The recommended value is 10.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

<span data-ttu-id="eccac-175">如需詳細資訊，請參閱[ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx)。</span><span class="sxs-lookup"><span data-stu-id="eccac-175">For more information, see [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="eccac-176">如何指定查詢字串參數</span><span class="sxs-lookup"><span data-stu-id="eccac-176">How to specify query string parameters</span></span>

<span data-ttu-id="eccac-177">如果您想要在用戶端連線時，將資料傳送到伺服器，您可以查詢字串參數加入連接物件。</span><span class="sxs-lookup"><span data-stu-id="eccac-177">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="eccac-178">下列範例會示範如何在用戶端程式碼中設定的查詢字串參數。</span><span class="sxs-lookup"><span data-stu-id="eccac-178">The following example shows how to set a query string parameter in client code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample5.cs)]

<span data-ttu-id="eccac-179">下列範例會示範如何讀取伺服器程式碼中的查詢字串參數。</span><span class="sxs-lookup"><span data-stu-id="eccac-179">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="eccac-180">如何指定傳輸方法</span><span class="sxs-lookup"><span data-stu-id="eccac-180">How to specify the transport method</span></span>

<span data-ttu-id="eccac-181">連接的程序的一部分，SignalR 用戶端通常會與交涉的伺服器和用戶端到伺服器來判斷最佳支援的傳輸。</span><span class="sxs-lookup"><span data-stu-id="eccac-181">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="eccac-182">如果您已經知道您想要使用的傳輸，您可以略過此交涉程序。</span><span class="sxs-lookup"><span data-stu-id="eccac-182">If you already know which transport you want to use, you can bypass this negotiation process.</span></span> <span data-ttu-id="eccac-183">若要指定傳輸方法，傳入傳輸物件的 Start 方法。</span><span class="sxs-lookup"><span data-stu-id="eccac-183">To specify the transport method, pass in a transport object to the Start method.</span></span> <span data-ttu-id="eccac-184">下列範例顯示如何在用戶端程式碼中指定的傳輸方法。</span><span class="sxs-lookup"><span data-stu-id="eccac-184">The following example shows how to specify the transport method in client code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

<span data-ttu-id="eccac-185">[Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx)命名空間包含下列類別可讓您指定的傳輸。</span><span class="sxs-lookup"><span data-stu-id="eccac-185">The [Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) namespace includes the following classes that you can use to specify the transport.</span></span>

- <span data-ttu-id="eccac-186">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="eccac-186">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="eccac-187">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="eccac-187">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="eccac-188">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) （使用伺服器和用戶端使用.NET 4.5 時，才）。</span><span class="sxs-lookup"><span data-stu-id="eccac-188">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (Available only when both server and client use .NET 4.5.)</span></span>
- <span data-ttu-id="eccac-189">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) （會自動選擇最佳傳輸支援的用戶端和伺服器。</span><span class="sxs-lookup"><span data-stu-id="eccac-189">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (Automatically chooses the best transport that is supported by both the client and the server.</span></span> <span data-ttu-id="eccac-190">這是預設的傳輸。</span><span class="sxs-lookup"><span data-stu-id="eccac-190">This is the default transport.</span></span> <span data-ttu-id="eccac-191">傳遞此中`Start`方法具有相同的效果不會傳遞任何項目。)</span><span class="sxs-lookup"><span data-stu-id="eccac-191">Passing this in to the `Start` method has the same effect as not passing in anything.)</span></span>

<span data-ttu-id="eccac-192">ForeverFrame 傳輸不會包含這份清單中，因為它僅由瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="eccac-192">The ForeverFrame transport is not included in this list because it is used only by browsers.</span></span>

<span data-ttu-id="eccac-193">如需如何檢查伺服端程式碼中的傳輸方法的資訊，請參閱[ASP.NET SignalR 中樞 API 指南-伺服器-如何取得用戶端的相關資訊的內容屬性從](../guide-to-the-api/hubs-api-guide-server.md#contextproperty)。</span><span class="sxs-lookup"><span data-stu-id="eccac-193">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="eccac-194">如需傳輸與後援的詳細資訊，請參閱[簡介 SignalR 的傳輸與後援](../getting-started/introduction-to-signalr.md#transports)。</span><span class="sxs-lookup"><span data-stu-id="eccac-194">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a><span data-ttu-id="eccac-195">如何指定 HTTP 標頭</span><span class="sxs-lookup"><span data-stu-id="eccac-195">How to specify HTTP headers</span></span>

<span data-ttu-id="eccac-196">若要設定 HTTP 標頭，使用`Headers`連線物件上的屬性。</span><span class="sxs-lookup"><span data-stu-id="eccac-196">To set HTTP headers, use the `Headers` property on the connection object.</span></span> <span data-ttu-id="eccac-197">下列範例會示範如何新增 HTTP 標頭。</span><span class="sxs-lookup"><span data-stu-id="eccac-197">The following example shows how to add an HTTP header.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a><span data-ttu-id="eccac-198">如何指定用戶端憑證</span><span class="sxs-lookup"><span data-stu-id="eccac-198">How to specify client certificates</span></span>

<span data-ttu-id="eccac-199">若要新增用戶端憑證，請使用`AddClientCertificate`連線物件上的方法。</span><span class="sxs-lookup"><span data-stu-id="eccac-199">To add client certificates, use the `AddClientCertificate` method on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a><span data-ttu-id="eccac-200">如何建立中樞 proxy</span><span class="sxs-lookup"><span data-stu-id="eccac-200">How to create the Hub proxy</span></span>

<span data-ttu-id="eccac-201">為了在集線器可以呼叫在伺服器上，從用戶端上定義方法，並叫用中樞，以在伺服器上的方法，來建立中樞的 proxy 呼叫`CreateHubProxy`連線物件上。</span><span class="sxs-lookup"><span data-stu-id="eccac-201">In order to define methods on the client that a Hub can call from the server, and to invoke methods on a Hub at the server, create a proxy for the Hub by calling `CreateHubProxy` on the connection object.</span></span> <span data-ttu-id="eccac-202">字串您傳遞給`CreateHubProxy`是您的中樞類別名稱或所指定的名稱`HubName`屬性如果在伺服器上已使用的其中一種。</span><span class="sxs-lookup"><span data-stu-id="eccac-202">The string you pass in to `CreateHubProxy` is the name of your Hub class, or the name specified by the `HubName` attribute if one was used on the server.</span></span> <span data-ttu-id="eccac-203">名稱比對不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="eccac-203">Name matching is case-insensitive.</span></span>

<span data-ttu-id="eccac-204">**在伺服器上的中樞類別**</span><span class="sxs-lookup"><span data-stu-id="eccac-204">**Hub class on server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

<span data-ttu-id="eccac-205">**建立 Hub 類別的用戶端的 proxy**</span><span class="sxs-lookup"><span data-stu-id="eccac-205">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

<span data-ttu-id="eccac-206">如果裝飾中樞類別`HubName`屬性，請使用該名稱。</span><span class="sxs-lookup"><span data-stu-id="eccac-206">If you decorate your Hub class with a `HubName` attribute, use that name.</span></span>

<span data-ttu-id="eccac-207">**在伺服器上的中樞類別**</span><span class="sxs-lookup"><span data-stu-id="eccac-207">**Hub class on server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample12.cs)]

<span data-ttu-id="eccac-208">**建立 Hub 類別的用戶端的 proxy**</span><span class="sxs-lookup"><span data-stu-id="eccac-208">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

<span data-ttu-id="eccac-209">Proxy 物件是安全執行緒。</span><span class="sxs-lookup"><span data-stu-id="eccac-209">The proxy object is thread-safe.</span></span> <span data-ttu-id="eccac-210">事實上，如果您呼叫`HubConnection.CreateHubProxy`多次具有相同`hubName`，您可以取得相同的快取`IHubProxy`物件。</span><span class="sxs-lookup"><span data-stu-id="eccac-210">In fact, if you call `HubConnection.CreateHubProxy` multiple times with the same `hubName`, you get the same cached `IHubProxy` object.</span></span>

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="eccac-211">如何定義伺服器可以呼叫的用戶端上的方法</span><span class="sxs-lookup"><span data-stu-id="eccac-211">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="eccac-212">若要定義伺服器可以呼叫的方法，使用 proxy 的`On`方法，以註冊事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="eccac-212">To define a method that the server can call, use the proxy's `On` method to register an event handler.</span></span>

<span data-ttu-id="eccac-213">方法名稱比對不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="eccac-213">Method name matching is case-insensitive.</span></span> <span data-ttu-id="eccac-214">例如，`Clients.All.UpdateStockPrice`在伺服器上將會執行`updateStockPrice`， `updatestockprice`，或`UpdateStockPrice`用戶端上。</span><span class="sxs-lookup"><span data-stu-id="eccac-214">For example, `Clients.All.UpdateStockPrice` on the server will execute `updateStockPrice`, `updatestockprice`, or `UpdateStockPrice` on the client.</span></span>

<span data-ttu-id="eccac-215">不同的用戶端的平台有不同需求，撰寫方法的程式碼的方式更新 UI。</span><span class="sxs-lookup"><span data-stu-id="eccac-215">Different client platforms have different requirements for how you write method code to update the UI.</span></span> <span data-ttu-id="eccac-216">所顯示的範例是針對 WinRT (Windows 市集.NET) 用戶端。</span><span class="sxs-lookup"><span data-stu-id="eccac-216">The examples shown are for WinRT (Windows Store .NET) clients.</span></span> <span data-ttu-id="eccac-217">WPF、 Silverlight 和主控台應用程式範例中提供[稍後在本主題中的個別區段](#wpfsl)。</span><span class="sxs-lookup"><span data-stu-id="eccac-217">WPF, Silverlight, and console application examples are provided in [a separate section later in this topic](#wpfsl).</span></span>

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a><span data-ttu-id="eccac-218">不含參數的方法</span><span class="sxs-lookup"><span data-stu-id="eccac-218">Methods without parameters</span></span>

<span data-ttu-id="eccac-219">如果您正在處理的方法沒有參數，使用的非泛型多載`On`方法：</span><span class="sxs-lookup"><span data-stu-id="eccac-219">If the method you're handling does not have parameters, use the non-generic overload of the `On` method:</span></span>

<span data-ttu-id="eccac-220">**伺服端程式碼呼叫不含參數的用戶端方法**</span><span class="sxs-lookup"><span data-stu-id="eccac-220">**Server code calling client method without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

<span data-ttu-id="eccac-221">**從伺服器不使用參數呼叫方法的 WinRT 用戶端程式碼 ([請參閱本主題稍後的 WPF 和 Silverlight 範例](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="eccac-221">**WinRT Client code for method called from server without parameters ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="eccac-222">具有指定的參數型別參數的方法</span><span class="sxs-lookup"><span data-stu-id="eccac-222">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="eccac-223">如果您正在處理的方法有參數，指定的參數類型的泛型型別之`On`方法。</span><span class="sxs-lookup"><span data-stu-id="eccac-223">If the method you're handling has parameters, specify the types of the parameters as the generic types of the `On` method.</span></span> <span data-ttu-id="eccac-224">有泛型多載的`On`方法可讓您指定最多 8 個參數 (在 Windows Phone 7 上為 4)。</span><span class="sxs-lookup"><span data-stu-id="eccac-224">There are generic overloads of the `On` method to enable you to specify up to 8 parameters (4 on Windows Phone 7).</span></span> <span data-ttu-id="eccac-225">在下列範例中，一個參數會傳送至`UpdateStockPrice`方法。</span><span class="sxs-lookup"><span data-stu-id="eccac-225">In the following example, one parameter is sent to the `UpdateStockPrice` method.</span></span>

<span data-ttu-id="eccac-226">**伺服端程式碼呼叫具有參數的用戶端方法**</span><span class="sxs-lookup"><span data-stu-id="eccac-226">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

<span data-ttu-id="eccac-227">**用於參數的庫存類別**</span><span class="sxs-lookup"><span data-stu-id="eccac-227">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample17.cs)]

<span data-ttu-id="eccac-228">**從伺服器以參數呼叫方法的 WinRT 用戶端程式碼 ([請參閱本主題稍後的 WPF 和 Silverlight 範例](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="eccac-228">**WinRT Client code for a method called from server with a parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="eccac-229">具有指定之參數的動態物件參數的方法</span><span class="sxs-lookup"><span data-stu-id="eccac-229">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="eccac-230">做為參數指定做為泛型型別替代`On`方法，您可以指定參數為動態物件：</span><span class="sxs-lookup"><span data-stu-id="eccac-230">As an alternative to specifying parameters as generic types of the `On` method, you can specify parameters as dynamic objects:</span></span>

<span data-ttu-id="eccac-231">**伺服端程式碼呼叫具有參數的用戶端方法**</span><span class="sxs-lookup"><span data-stu-id="eccac-231">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

<span data-ttu-id="eccac-232">**用於參數的庫存類別**</span><span class="sxs-lookup"><span data-stu-id="eccac-232">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample20.cs)]

<span data-ttu-id="eccac-233">**從伺服器具有參數，使用參數的動態物件呼叫方法的 WinRT 用戶端程式碼 ([請參閱本主題稍後的 WPF 和 Silverlight 範例](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="eccac-233">**WinRT Client code for a method called from server with a parameter, using a dynamic object for the parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a><span data-ttu-id="eccac-234">如何移除處理常式</span><span class="sxs-lookup"><span data-stu-id="eccac-234">How to remove a handler</span></span>

<span data-ttu-id="eccac-235">若要移除處理常式，呼叫其`Dispose`方法。</span><span class="sxs-lookup"><span data-stu-id="eccac-235">To remove a handler, call its `Dispose` method.</span></span>

<span data-ttu-id="eccac-236">**從伺服器呼叫的方法的用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="eccac-236">**Client code for a method called from server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

<span data-ttu-id="eccac-237">**用戶端程式碼中移除處理常式**</span><span class="sxs-lookup"><span data-stu-id="eccac-237">**Client code to remove the handler**</span></span>

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="eccac-238">如何從用戶端呼叫伺服器方法</span><span class="sxs-lookup"><span data-stu-id="eccac-238">How to call server methods from the client</span></span>

<span data-ttu-id="eccac-239">若要在伺服器上呼叫方法，使用`Invoke`中樞 proxy 上的方法。</span><span class="sxs-lookup"><span data-stu-id="eccac-239">To call a method on the server, use the `Invoke` method on the Hub proxy.</span></span>

<span data-ttu-id="eccac-240">如果伺服器方法沒有傳回值，請使用非泛型多載`Invoke`方法。</span><span class="sxs-lookup"><span data-stu-id="eccac-240">If the server method has no return value, use the non-generic overload of the `Invoke` method.</span></span>

<span data-ttu-id="eccac-241">**伺服端程式碼沒有傳回值的方法**</span><span class="sxs-lookup"><span data-stu-id="eccac-241">**Server code for a method that has no return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

<span data-ttu-id="eccac-242">**用戶端程式碼呼叫的方法沒有傳回值，**</span><span class="sxs-lookup"><span data-stu-id="eccac-242">**Client code calling a method that has no return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="eccac-243">如果伺服器方法具有傳回值，指定的傳回型別為泛型型別`Invoke`方法。</span><span class="sxs-lookup"><span data-stu-id="eccac-243">If the server method has a return value, specify the return type as the generic type of the `Invoke` method.</span></span>

<span data-ttu-id="eccac-244">**伺服端程式碼的方法具有傳回值，使用複雜型別參數**</span><span class="sxs-lookup"><span data-stu-id="eccac-244">**Server code for a method that has a return value and takes a complex type parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="eccac-245">**用於參數和傳回值的庫存類別**</span><span class="sxs-lookup"><span data-stu-id="eccac-245">**The Stock class used for the parameter and return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample27.cs)]

<span data-ttu-id="eccac-246">**用戶端程式碼呼叫的方法具有傳回值，並使用 ASP.NET 4.5 的非同步方法中的複雜型別參數**</span><span class="sxs-lookup"><span data-stu-id="eccac-246">**Client code calling a method that has a return value and takes a complex type parameter, in an ASP.NET 4.5 async method**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

<span data-ttu-id="eccac-247">**用戶端程式碼呼叫的方法具有傳回值，同步方法中使用複雜型別參數**</span><span class="sxs-lookup"><span data-stu-id="eccac-247">**Client code calling a method that has a return value and takes a complex type parameter, in a synchronous method**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

<span data-ttu-id="eccac-248">`Invoke`方法以非同步方式執行，並傳回`Task`物件。</span><span class="sxs-lookup"><span data-stu-id="eccac-248">The `Invoke` method executes asynchronously and returns a `Task` object.</span></span> <span data-ttu-id="eccac-249">如果您未指定`await`或`.Wait()`之前您叫用的方法已完成執行時,，會執行下的一行程式碼。</span><span class="sxs-lookup"><span data-stu-id="eccac-249">If you don't specify `await` or `.Wait()`, the next line of code will execute before the method that you invoke has finished executing.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="eccac-250">如何處理連接的存留期事件</span><span class="sxs-lookup"><span data-stu-id="eccac-250">How to handle connection lifetime events</span></span>

<span data-ttu-id="eccac-251">SignalR 提供下列的連線，您可以控制它們的存留期事件：</span><span class="sxs-lookup"><span data-stu-id="eccac-251">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="eccac-252">`Received`： 在此連接上收到任何資料時引發。</span><span class="sxs-lookup"><span data-stu-id="eccac-252">`Received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="eccac-253">提供已接收的資料。</span><span class="sxs-lookup"><span data-stu-id="eccac-253">Provides the received data.</span></span>
- <span data-ttu-id="eccac-254">`ConnectionSlow`： 用戶端偵測到低速或經常卸除連接時引發。</span><span class="sxs-lookup"><span data-stu-id="eccac-254">`ConnectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="eccac-255">`Reconnecting`： 基礎傳輸可讓您開始重新連線時引發。</span><span class="sxs-lookup"><span data-stu-id="eccac-255">`Reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="eccac-256">`Reconnected`： 基礎傳輸已重新連接時引發。</span><span class="sxs-lookup"><span data-stu-id="eccac-256">`Reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="eccac-257">`StateChanged`： 連線狀態變更時引發。</span><span class="sxs-lookup"><span data-stu-id="eccac-257">`StateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="eccac-258">提供舊的狀態和新的狀態。</span><span class="sxs-lookup"><span data-stu-id="eccac-258">Provides the old state and the new state.</span></span> <span data-ttu-id="eccac-259">如需連接狀態值請參閱[ConnectionState 列舉，](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx)。</span><span class="sxs-lookup"><span data-stu-id="eccac-259">For information about connection state values see [ConnectionState Enumeration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span></span>
- <span data-ttu-id="eccac-260">`Closed`： 當連接已中斷連接時引發。</span><span class="sxs-lookup"><span data-stu-id="eccac-260">`Closed`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="eccac-261">例如，如果您想要顯示的不嚴重，但間歇性連線問題會造成錯誤的警告訊息，例如為緩慢或太頻繁地卸除的連接，處理`ConnectionSlow`事件。</span><span class="sxs-lookup"><span data-stu-id="eccac-261">For example, if you want to display warning messages for errors that are not fatal but cause intermittent connection problems, such as slowness or frequent dropping of the connection, handle the `ConnectionSlow` event.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample30.cs)]

<span data-ttu-id="eccac-262">如需詳細資訊，請參閱[了解與 SignalR 中處理連接存留期事件](../guide-to-the-api/handling-connection-lifetime-events.md)。</span><span class="sxs-lookup"><span data-stu-id="eccac-262">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="eccac-263">如何處理錯誤</span><span class="sxs-lookup"><span data-stu-id="eccac-263">How to handle errors</span></span>

<span data-ttu-id="eccac-264">如果您未明確啟用在伺服器上的詳細的錯誤訊息，在發生錯誤之後，傳回 SignalR 的例外狀況物件包含錯誤的相關基本資訊。</span><span class="sxs-lookup"><span data-stu-id="eccac-264">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="eccac-265">例如，如果呼叫`newContosoChatMessage`失敗，錯誤物件中的錯誤訊息包含 「`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`」 傳送詳細的錯誤訊息，以在生產環境中的用戶端不會建議基於安全性理由，但如果您想要啟用的詳細的錯誤訊息疑難排解用途，在伺服器上使用下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="eccac-265">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

<span data-ttu-id="eccac-266">若要處理 SignalR 引發的錯誤，您可以加入的處理常式`Error`連線物件上的事件。</span><span class="sxs-lookup"><span data-stu-id="eccac-266">To handle errors that SignalR raises, you can add a handler for the `Error` event on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample32.cs)]

<span data-ttu-id="eccac-267">若要處理來自方法引動過程的錯誤，程式碼包裝在 try catch 區塊中。</span><span class="sxs-lookup"><span data-stu-id="eccac-267">To handle errors from method invocations, wrap the code in a try-catch block.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="eccac-268">如何啟用用戶端記錄</span><span class="sxs-lookup"><span data-stu-id="eccac-268">How to enable client-side logging</span></span>

<span data-ttu-id="eccac-269">若要啟用用戶端記錄，`TraceLevel`和`TraceWriter`連線物件上的屬性。</span><span class="sxs-lookup"><span data-stu-id="eccac-269">To enable client-side logging, set the `TraceLevel` and `TraceWriter` properties on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a><span data-ttu-id="eccac-270">WPF、 Silverlight 和主控台應用程式程式碼可以呼叫伺服器的用戶端方法的範例</span><span class="sxs-lookup"><span data-stu-id="eccac-270">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>

<span data-ttu-id="eccac-271">稍早所示，定義伺服器可以呼叫的用戶端方法的程式碼範例會套用至 WinRT 用戶端。</span><span class="sxs-lookup"><span data-stu-id="eccac-271">The code samples shown earlier for defining client methods that the server can call apply to WinRT clients.</span></span> <span data-ttu-id="eccac-272">下列範例顯示 WPF、 Silverlight 和主控台應用程式用戶端的對等的程式碼。</span><span class="sxs-lookup"><span data-stu-id="eccac-272">The following samples show the equivalent code for WPF, Silverlight, and console application clients.</span></span>

### <a name="methods-without-parameters"></a><span data-ttu-id="eccac-273">不含參數的方法</span><span class="sxs-lookup"><span data-stu-id="eccac-273">Methods without parameters</span></span>

<span data-ttu-id="eccac-274">**從伺服器不使用參數呼叫方法的 WPF 用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="eccac-274">**WPF client code for method called from server without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="eccac-275">**從伺服器不使用參數呼叫方法的 Silverlight 用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="eccac-275">**Silverlight client code for method called from server without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

<span data-ttu-id="eccac-276">**從伺服器不使用參數呼叫方法的主控台應用程式用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="eccac-276">**Console application client code for method called from server without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="eccac-277">具有指定的參數型別參數的方法</span><span class="sxs-lookup"><span data-stu-id="eccac-277">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="eccac-278">**從伺服器以參數呼叫方法的 WPF 用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="eccac-278">**WPF client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

<span data-ttu-id="eccac-279">**Silverlight 用戶端程式碼從具有參數的伺服器呼叫的方法**</span><span class="sxs-lookup"><span data-stu-id="eccac-279">**Silverlight client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

<span data-ttu-id="eccac-280">**從伺服器以參數呼叫方法的主控台應用程式用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="eccac-280">**Console application client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="eccac-281">具有指定之參數的動態物件參數的方法</span><span class="sxs-lookup"><span data-stu-id="eccac-281">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="eccac-282">**從伺服器具有參數，使用參數的動態物件呼叫方法的 WPF 用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="eccac-282">**WPF client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

<span data-ttu-id="eccac-283">**從伺服器具有參數，使用參數的動態物件呼叫方法的 Silverlight 用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="eccac-283">**Silverlight client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

<span data-ttu-id="eccac-284">**從伺服器具有參數，使用參數的動態物件呼叫方法的主控台應用程式用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="eccac-284">**Console application client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
