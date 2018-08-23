---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
title: SignalR 1.x 中樞 API 指南-JavaScript 用戶端 |Microsoft Docs
author: pfletcher
description: 本文件提供使用 signalr JavaScript 用戶端，例如瀏覽器和 Windows 市集 (WinJS) 應用程式中的 版本 1.1 的中樞 API 的簡介...
ms.author: riande
ms.date: 04/17/2013
ms.assetid: dcd4593b-1118-418a-af71-d12ff33fb36d
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 993ad7924d8335f79aa2c3e41c00ddfa8bc26874
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834672"
---
<a name="signalr-1x-hubs-api-guide---javascript-client"></a><span data-ttu-id="954e8-103">SignalR 1.x 中樞 API 指南-JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="954e8-103">SignalR 1.x Hubs API Guide - JavaScript Client</span></span>
====================
<span data-ttu-id="954e8-104">藉由[Patrick Fletcher](https://github.com/pfletcher)， [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="954e8-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="954e8-105">本文件提供使用 signalr JavaScript 用戶端，例如瀏覽器和 Windows 市集 (WinJS) 應用程式中的 版本 1.1 的中樞 API 的簡介。</span><span class="sxs-lookup"><span data-stu-id="954e8-105">This document provides an introduction to using the Hubs API for SignalR version 1.1 in JavaScript clients, such as browsers and Windows Store (WinJS) applications.</span></span>
> 
> <span data-ttu-id="954e8-106">SignalR 中樞 API 可讓您從伺服器連線的用戶端和伺服器的用戶端進行遠端程序呼叫 (Rpc)。</span><span class="sxs-lookup"><span data-stu-id="954e8-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="954e8-107">在伺服器程式碼中，您定義可由用戶端，呼叫的方法，呼叫用戶端執行的方法。</span><span class="sxs-lookup"><span data-stu-id="954e8-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="954e8-108">在用戶端程式碼中，您定義可以在伺服器上，從呼叫的方法，您呼叫在伺服器執行的方法。</span><span class="sxs-lookup"><span data-stu-id="954e8-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="954e8-109">SignalR 會處理所有為您的用戶端-伺服器配管。</span><span class="sxs-lookup"><span data-stu-id="954e8-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="954e8-110">SignalR 也提供一個名為持續連線的較低層級 API。</span><span class="sxs-lookup"><span data-stu-id="954e8-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="954e8-111">如需 SignalR、 中樞和持續連線，或該教學課程說明如何建置完整的 SignalR 應用程式，請參閱[SignalR-開始使用](../getting-started/index.md)。</span><span class="sxs-lookup"><span data-stu-id="954e8-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>


## <a name="overview"></a><span data-ttu-id="954e8-112">總覽</span><span class="sxs-lookup"><span data-stu-id="954e8-112">Overview</span></span>

<span data-ttu-id="954e8-113">本文件包含下列章節：</span><span class="sxs-lookup"><span data-stu-id="954e8-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="954e8-114">產生的 proxy，它做什麼，</span><span class="sxs-lookup"><span data-stu-id="954e8-114">The generated proxy and what it does for you</span></span>](#genproxy)

    - [<span data-ttu-id="954e8-115">使用產生的 proxy 的時機</span><span class="sxs-lookup"><span data-stu-id="954e8-115">When to use the generated proxy</span></span>](#cantusegenproxy)
- [<span data-ttu-id="954e8-116">用戶端安裝程式</span><span class="sxs-lookup"><span data-stu-id="954e8-116">Client Setup</span></span>](#clientsetup)

    - [<span data-ttu-id="954e8-117">如何參考動態產生的 proxy</span><span class="sxs-lookup"><span data-stu-id="954e8-117">How to reference the dynamically generated proxy</span></span>](#dynamicproxy)
    - [<span data-ttu-id="954e8-118">如何建立 signalr 的實體檔案產生 proxy</span><span class="sxs-lookup"><span data-stu-id="954e8-118">How to create a physical file for the SignalR generated proxy</span></span>](#manualproxy)
- [<span data-ttu-id="954e8-119">如何建立連線</span><span class="sxs-lookup"><span data-stu-id="954e8-119">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="954e8-120">$。 connection.hub 是相同物件會建立該 $.hubConnection()</span><span class="sxs-lookup"><span data-stu-id="954e8-120">$.connection.hub is the same object that $.hubConnection() creates</span></span>](#connequivalence)
    - [<span data-ttu-id="954e8-121">非同步執行的 start 方法</span><span class="sxs-lookup"><span data-stu-id="954e8-121">Asynchronous execution of the start method</span></span>](#asyncstart)
- [<span data-ttu-id="954e8-122">如何建立跨網域的連線</span><span class="sxs-lookup"><span data-stu-id="954e8-122">How to establish a cross-domain connection</span></span>](#crossdomain)
- [<span data-ttu-id="954e8-123">如何設定連線</span><span class="sxs-lookup"><span data-stu-id="954e8-123">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="954e8-124">如何指定查詢字串參數</span><span class="sxs-lookup"><span data-stu-id="954e8-124">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="954e8-125">如何指定傳輸方法</span><span class="sxs-lookup"><span data-stu-id="954e8-125">How to specify the transport method</span></span>](#transport)
- [<span data-ttu-id="954e8-126">如何取得 Hub 類別的 proxy</span><span class="sxs-lookup"><span data-stu-id="954e8-126">How to get a proxy for a Hub class</span></span>](#getproxy)
- [<span data-ttu-id="954e8-127">如何定義伺服器可以呼叫用戶端上的方法</span><span class="sxs-lookup"><span data-stu-id="954e8-127">How to define methods on the client that the server can call</span></span>](#callclient)
- [<span data-ttu-id="954e8-128">如何從用戶端呼叫伺服器方法</span><span class="sxs-lookup"><span data-stu-id="954e8-128">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="954e8-129">如何處理連接的存留期事件</span><span class="sxs-lookup"><span data-stu-id="954e8-129">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="954e8-130">如何處理錯誤</span><span class="sxs-lookup"><span data-stu-id="954e8-130">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="954e8-131">如何啟用用戶端記錄</span><span class="sxs-lookup"><span data-stu-id="954e8-131">How to enable client-side logging</span></span>](#logging)

<span data-ttu-id="954e8-132">如需如何撰寫程式的伺服器或.NET 用戶端的文件，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="954e8-132">For documentation on how to program the server or .NET clients, see the following resources:</span></span>

- [<span data-ttu-id="954e8-133">SignalR 中樞 API 指南-伺服器</span><span class="sxs-lookup"><span data-stu-id="954e8-133">SignalR Hubs API Guide - Server</span></span>](../guide-to-the-api/hubs-api-guide-server.md)
- [<span data-ttu-id="954e8-134">SignalR 中樞 API 指南-.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="954e8-134">SignalR Hubs API Guide - .NET Client</span></span>](../guide-to-the-api/hubs-api-guide-net-client.md)

<span data-ttu-id="954e8-135">API 參考主題的連結是 API 的.NET 4.5 版本。</span><span class="sxs-lookup"><span data-stu-id="954e8-135">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="954e8-136">如果您使用.NET 4，請參閱[API 主題的.NET 4 版本](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx)。</span><span class="sxs-lookup"><span data-stu-id="954e8-136">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a><span data-ttu-id="954e8-137">產生的 proxy，它做什麼，</span><span class="sxs-lookup"><span data-stu-id="954e8-137">The generated proxy and what it does for you</span></span>

<span data-ttu-id="954e8-138">您可以編寫 JavaScript 用戶端通訊的 SignalR 服務，或不使用 SignalR 會為您產生的 proxy。</span><span class="sxs-lookup"><span data-stu-id="954e8-138">You can program a JavaScript client to communicate with a SignalR service with or without a proxy that SignalR generates for you.</span></span> <span data-ttu-id="954e8-139">Proxy 會為您是簡化您的程式碼使用連線，寫入方法之伺服器呼叫，在伺服器上呼叫方法的語法。</span><span class="sxs-lookup"><span data-stu-id="954e8-139">What the proxy does for you is simplify the syntax of the code you use to connect, write methods that the server calls, and call methods on the server.</span></span>

<span data-ttu-id="954e8-140">當您撰寫程式碼呼叫伺服器方法時，產生的 proxy 可讓您使用看起來就好像您正在執行的區域函式的語法： 您可以撰寫`serverMethod(arg1, arg2)`而不是`invoke('serverMethod', arg1, arg2)`。</span><span class="sxs-lookup"><span data-stu-id="954e8-140">When you write code to call server methods, the generated proxy enables you to use syntax that looks as though you were executing a local function: you can write `serverMethod(arg1, arg2)` instead of `invoke('serverMethod', arg1, arg2)`.</span></span> <span data-ttu-id="954e8-141">如果您拼錯伺服器方法名稱，產生的 proxy 語法也可讓立即且容易理解的用戶端錯誤。</span><span class="sxs-lookup"><span data-stu-id="954e8-141">The generated proxy syntax also enables an immediate and intelligible client-side error if you mistype a server method name.</span></span> <span data-ttu-id="954e8-142">如果您以手動方式建立可定義 proxy 的檔案，則您也可以撰寫會呼叫伺服器方法的程式碼取得 IntelliSense 支援。</span><span class="sxs-lookup"><span data-stu-id="954e8-142">And if you manually create the file that defines the proxies, you can also get IntelliSense support for writing code that calls server methods.</span></span>

<span data-ttu-id="954e8-143">例如，假設您在伺服器上有下列的中樞類別：</span><span class="sxs-lookup"><span data-stu-id="954e8-143">For example, suppose you have the following Hub class on the server:</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

<span data-ttu-id="954e8-144">下列程式碼範例顯示的 JavaScript 程式碼看起來像什麼叫用`NewContosoChatMessage`方法，在伺服器和收到的引動過程上`addContosoChatMessageToPage`來自伺服器的方法。</span><span class="sxs-lookup"><span data-stu-id="954e8-144">The following code examples show what JavaScript code looks like for invoking the `NewContosoChatMessage` method on the server and receiving invocations of the `addContosoChatMessageToPage` method from the server.</span></span>

<span data-ttu-id="954e8-145">**使用產生的 proxy**</span><span class="sxs-lookup"><span data-stu-id="954e8-145">**With the generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

<span data-ttu-id="954e8-146">**不產生的 proxy**</span><span class="sxs-lookup"><span data-stu-id="954e8-146">**Without the generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a><span data-ttu-id="954e8-147">使用產生的 proxy 的時機</span><span class="sxs-lookup"><span data-stu-id="954e8-147">When to use the generated proxy</span></span>

<span data-ttu-id="954e8-148">如果您想要註冊的伺服器呼叫的用戶端方法的多個事件處理常式，您無法使用產生的 proxy。</span><span class="sxs-lookup"><span data-stu-id="954e8-148">If you want to register multiple event handlers for a client method that the server calls, you can't use the generated proxy.</span></span> <span data-ttu-id="954e8-149">否則，您可以選擇使用產生的 proxy，或不會根據您的程式碼撰寫喜好設定。</span><span class="sxs-lookup"><span data-stu-id="954e8-149">Otherwise, you can choose to use the generated proxy or not based on your coding preference.</span></span> <span data-ttu-id="954e8-150">如果您選擇不使用它，您不需要參考中的 「 signalr/中樞 」 URL`script`用戶端程式碼中的項目。</span><span class="sxs-lookup"><span data-stu-id="954e8-150">If you choose not to use it, you don't have to reference the "signalr/hubs" URL in a `script` element in your client code.</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="954e8-151">用戶端安裝程式</span><span class="sxs-lookup"><span data-stu-id="954e8-151">Client setup</span></span>

<span data-ttu-id="954e8-152">JavaScript 用戶端必須參考 jQuery 和 SignalR core JavaScript 檔案。</span><span class="sxs-lookup"><span data-stu-id="954e8-152">A JavaScript client requires references to jQuery and the SignalR core JavaScript file.</span></span> <span data-ttu-id="954e8-153">JQuery 版本必須是 1.6.4 或主要更新版本的詳細資訊，例如 1.7.2、 1.8.2 或 1.9.1。</span><span class="sxs-lookup"><span data-stu-id="954e8-153">The jQuery version must be 1.6.4 or major later versions, such as 1.7.2, 1.8.2, or 1.9.1.</span></span> <span data-ttu-id="954e8-154">如果您決定要使用產生的 proxy，您也需要將產生的 SignalR proxy JavaScript 檔案的參考。</span><span class="sxs-lookup"><span data-stu-id="954e8-154">If you decide to use the generated proxy, you also need a reference to the SignalR generated proxy JavaScript file.</span></span> <span data-ttu-id="954e8-155">下列範例顯示參考可能會如下所示在 HTML 頁面，會使用產生的 proxy。</span><span class="sxs-lookup"><span data-stu-id="954e8-155">The following example shows what the references might look like in an HTML page that uses the generated proxy.</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample4.html)]

<span data-ttu-id="954e8-156">這些參考必須包含在此順序： jQuery 在那之後，SignalR core SignalR proxy 名字、 姓氏。</span><span class="sxs-lookup"><span data-stu-id="954e8-156">These references must be included in this order: jQuery first, SignalR core after that, and SignalR proxies last.</span></span>

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a><span data-ttu-id="954e8-157">如何參考動態產生的 proxy</span><span class="sxs-lookup"><span data-stu-id="954e8-157">How to reference the dynamically generated proxy</span></span>

<span data-ttu-id="954e8-158">在上述範例中，產生的 SignalR proxy 的參考是動態產生的 JavaScript 程式碼，不以實體檔案。</span><span class="sxs-lookup"><span data-stu-id="954e8-158">In the preceding example, the reference to the SignalR generated proxy is to dynamically generated JavaScript code, not to a physical file.</span></span> <span data-ttu-id="954e8-159">SignalR 即時 proxy 建立的 JavaScript 程式碼，並提供給用戶端以回應至 「 signalr/中樞 」 的 URL。</span><span class="sxs-lookup"><span data-stu-id="954e8-159">SignalR creates the JavaScript code for the proxy on the fly and serves it to the client in response to the "/signalr/hubs" URL.</span></span> <span data-ttu-id="954e8-160">如果您在伺服器上指定不同的基底 URL，SignalR 連線的您`MapHubs`方法，以動態方式產生之 proxy 檔案的 URL 是您自訂的 URL，使用"/ 中樞"附加到它。</span><span class="sxs-lookup"><span data-stu-id="954e8-160">If you specified a different base URL for SignalR connections on the server in your `MapHubs` method, the URL for the dynamically generated proxy file is your custom URL with "/hubs" appended to it.</span></span>

> [!NOTE]
> <span data-ttu-id="954e8-161">Windows 8 （Windows 市集） JavaScript 用戶端，請使用實體的 proxy 檔案，而不是動態產生。</span><span class="sxs-lookup"><span data-stu-id="954e8-161">For Windows 8 (Windows Store) JavaScript clients, use the physical proxy file instead of the dynamically generated one.</span></span> <span data-ttu-id="954e8-162">如需詳細資訊，請參閱 <<c0> [ 如何建立 signalr 的實體檔案產生 proxy](#manualproxy)本主題稍後的。</span><span class="sxs-lookup"><span data-stu-id="954e8-162">For more information, see [How to create a physical file for the SignalR generated proxy](#manualproxy) later in this topic.</span></span>


<span data-ttu-id="954e8-163">在 ASP.NET MVC 4 Razor 檢視中，請參考您 proxy 檔案參考中的應用程式根目錄使用波狀符號：</span><span class="sxs-lookup"><span data-stu-id="954e8-163">In an ASP.NET MVC 4 Razor view, use the tilde to refer to the application root in your proxy file reference:</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample5.html)]

<span data-ttu-id="954e8-164">如需使用 MVC 4 的 SignalR 的詳細資訊，請參閱[開始使用 SignalR 和 MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md)。</span><span class="sxs-lookup"><span data-stu-id="954e8-164">For more information about using SignalR in MVC 4, see [Getting Started with SignalR and MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span></span>

<span data-ttu-id="954e8-165">在 ASP.NET MVC 3 Razor 檢視中，使用`Url.Content`供您的 proxy 檔案參考：</span><span class="sxs-lookup"><span data-stu-id="954e8-165">In an ASP.NET MVC 3 Razor view, use `Url.Content` for your proxy file reference:</span></span>

[!code-cshtml[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample6.cshtml)]

<span data-ttu-id="954e8-166">在 ASP.NET Web Forms 應用程式中，使用`ResolveClientUrl`您的 proxy 檔案的參考，或透過使用應用程式的根相對路徑 （開頭為波狀符號） ScriptManager 來註冊它：</span><span class="sxs-lookup"><span data-stu-id="954e8-166">In an ASP.NET Web Forms application, use `ResolveClientUrl` for your proxies file reference or register it via the ScriptManager using an app root relative path (starting with a tilde):</span></span>

[!code-aspx[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample7.aspx)]

<span data-ttu-id="954e8-167">一般而言，使用相同的方法來指定您用於 CSS 或 JavaScript 檔案的 「 signalr/中樞 」 URL。</span><span class="sxs-lookup"><span data-stu-id="954e8-167">As a general rule, use the same method for specifying the "/signalr/hubs" URL that you use for CSS or JavaScript files.</span></span> <span data-ttu-id="954e8-168">如果您指定的 URL，而不需使用波狀符號，在某些情況下您的應用程式會正常運作時您在 Visual Studio 中使用 IIS Express 測試，但部署至完整 IIS 時，將會失敗並傳回 404 錯誤。</span><span class="sxs-lookup"><span data-stu-id="954e8-168">If you specify a URL without using a tilde, in some scenarios your application will work correctly when you test in Visual Studio using IIS Express but will fail with a 404 error when you deploy to full IIS.</span></span> <span data-ttu-id="954e8-169">如需詳細資訊，請參閱 <<c0>  **解析根層級資源的參考**中[ASP.NET Web 專案的 Visual Studio 中的 Web 伺服器](https://msdn.microsoft.com/library/58wxa9w5.aspx)MSDN 網站上。</span><span class="sxs-lookup"><span data-stu-id="954e8-169">For more information, see **Resolving References to Root-Level Resources** in [Web Servers in Visual Studio for ASP.NET Web Projects](https://msdn.microsoft.com/library/58wxa9w5.aspx) on the MSDN site.</span></span>

<span data-ttu-id="954e8-170">當您 Visual Studio 2012 中執行 web 專案在偵錯模式中，如果您使用 Internet Explorer 作為您的瀏覽器時，您可以看到中的 proxy 檔案**方案總管**下方**指令碼文件**，如下所示下圖。</span><span class="sxs-lookup"><span data-stu-id="954e8-170">When you run a web project in Visual Studio 2012 in debug mode, and if you use Internet Explorer as your browser, you can see the proxy file in **Solution Explorer** under **Script Documents**, as shown in the following illustration.</span></span>

![在 [方案總管] 中的 JavaScript 產生之 proxy 檔案](signalr-1x-hubs-api-guide-javascript-client/_static/image1.png)

<span data-ttu-id="954e8-172">若要查看檔案的內容，請按兩下**中樞**。</span><span class="sxs-lookup"><span data-stu-id="954e8-172">To see the contents of the file, double-click **hubs**.</span></span> <span data-ttu-id="954e8-173">如果您不使用 Visual Studio 2012 和 Internet Explorer 中，或如果您不在偵錯模式中，您也可以取得檔案的內容瀏覽至 「 signalR/中樞 」 URL。</span><span class="sxs-lookup"><span data-stu-id="954e8-173">If you are not using Visual Studio 2012 and Internet Explorer, or if you are not in debug mode, you can also get the contents of the file by browsing to the "/signalR/hubs" URL.</span></span> <span data-ttu-id="954e8-174">例如，如果您的站台還在執行`http://localhost:56699`，請移至`http://localhost:56699/SignalR/hubs`瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="954e8-174">For example, if your site is running at `http://localhost:56699`, go to `http://localhost:56699/SignalR/hubs` in your browser.</span></span>

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a><span data-ttu-id="954e8-175">如何建立 signalr 的實體檔案產生 proxy</span><span class="sxs-lookup"><span data-stu-id="954e8-175">How to create a physical file for the SignalR generated proxy</span></span>

<span data-ttu-id="954e8-176">為動態產生之 proxy 的替代方案，您可以建立 proxy 程式碼的實體檔案，並參考該檔案。</span><span class="sxs-lookup"><span data-stu-id="954e8-176">As an alternative to the dynamically generated proxy, you can create a physical file that has the proxy code and reference that file.</span></span> <span data-ttu-id="954e8-177">您可以控制快取或統合的行為，如，或當您撰寫的程式碼對伺服器方法的呼叫時，取得 IntelliSense。</span><span class="sxs-lookup"><span data-stu-id="954e8-177">You might want to do that for control over caching or bundling behavior, or to get IntelliSense when you are coding calls to server methods.</span></span>

<span data-ttu-id="954e8-178">若要建立 proxy 檔案，請執行下列步驟：</span><span class="sxs-lookup"><span data-stu-id="954e8-178">To create a proxy file, perform the following steps:</span></span>

1. <span data-ttu-id="954e8-179">安裝[Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="954e8-179">Install the [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet package.</span></span>
2. <span data-ttu-id="954e8-180">開啟命令提示字元並瀏覽至*工具*SignalR.exe 檔案所在的資料夾。</span><span class="sxs-lookup"><span data-stu-id="954e8-180">Open a command prompt and browse to the *tools* folder that contains the SignalR.exe file.</span></span> <span data-ttu-id="954e8-181">[工具] 資料夾位於下列位置：</span><span class="sxs-lookup"><span data-stu-id="954e8-181">The tools folder is at the following location:</span></span>

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.1.0.1\tools`
3. <span data-ttu-id="954e8-182">輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="954e8-182">Enter the following command:</span></span>

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    <span data-ttu-id="954e8-183">通往您 *.dll*通常*bin*專案資料夾中的資料夾。</span><span class="sxs-lookup"><span data-stu-id="954e8-183">The path to your *.dll* is typically the *bin* folder in your project folder.</span></span>

    <span data-ttu-id="954e8-184">此命令會建立名為的檔案*server.js*相同的資料夾中*signalr.exe*。</span><span class="sxs-lookup"><span data-stu-id="954e8-184">This command creates a file named *server.js* in the same folder as *signalr.exe*.</span></span>
4. <span data-ttu-id="954e8-185">放*server.js*檔案中適當的資料夾，在您的專案、 將它重新命名為適用於您的應用程式，並新增對它的參考，來取代 「 signalr/中樞 」 參考。</span><span class="sxs-lookup"><span data-stu-id="954e8-185">Put the *server.js* file in an appropriate folder in your project, rename it as appropriate for your application, and add a reference to it in place of the "signalr/hubs" reference.</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="954e8-186">如何建立連線</span><span class="sxs-lookup"><span data-stu-id="954e8-186">How to establish a connection</span></span>

<span data-ttu-id="954e8-187">您可以建立連線之前，您必須建立連線物件、 建立 proxy，並註冊事件處理常式，您可以從伺服器呼叫的方法。</span><span class="sxs-lookup"><span data-stu-id="954e8-187">Before you can establish a connection, you have to create a connection object, create a proxy, and register event handlers for methods that can be called from the server.</span></span> <span data-ttu-id="954e8-188">設定 proxy 和事件處理常式，來建立此連接呼叫`start`方法。</span><span class="sxs-lookup"><span data-stu-id="954e8-188">When the proxy and event handlers are set up, establish the connection by calling the `start` method.</span></span>

<span data-ttu-id="954e8-189">如果您使用產生的 proxy，您不需要在自己的程式碼中建立的連接物件，因為產生的 proxy 程式碼會為您。</span><span class="sxs-lookup"><span data-stu-id="954e8-189">If you are using the generated proxy, you don't have to create the connection object in your own code because the generated proxy code does it for you.</span></span>

<a id="nogenconnection"></a>

<span data-ttu-id="954e8-190">**（與產生的 proxy) 建立連線**</span><span class="sxs-lookup"><span data-stu-id="954e8-190">**Establish a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

<span data-ttu-id="954e8-191">**建立連線 （不含產生的 proxy)**</span><span class="sxs-lookup"><span data-stu-id="954e8-191">**Establish a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

<span data-ttu-id="954e8-192">範例程式碼會使用預設值"/ signalr 」 來連線到您的 SignalR 服務的 URL。</span><span class="sxs-lookup"><span data-stu-id="954e8-192">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="954e8-193">如需有關如何指定不同的基底 URL 的資訊，請參閱 < [ASP.NET SignalR 中樞 API 指南-Server-/signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl)。</span><span class="sxs-lookup"><span data-stu-id="954e8-193">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>

> [!NOTE]
> <span data-ttu-id="954e8-194">通常您會註冊事件處理常式，然後再呼叫`start`方法來建立連線。</span><span class="sxs-lookup"><span data-stu-id="954e8-194">Normally you register event handlers before calling the `start` method to establish the connection.</span></span> <span data-ttu-id="954e8-195">如果您想要註冊一些事件處理常式建立連接後，您可以這麼做，但您必須註冊您的事件處理常式，然後再呼叫其中`start`方法。</span><span class="sxs-lookup"><span data-stu-id="954e8-195">If you want to register some event handlers after establishing the connection, you can do that, but you must register at least one of your event handler(s) before calling the `start` method.</span></span> <span data-ttu-id="954e8-196">這個的其中一個原因是應用程式，可以有許多的中樞，但您不想要觸發`OnConnected`上每個中樞，如果您打算只使用其中一個事件。</span><span class="sxs-lookup"><span data-stu-id="954e8-196">One reason for this is that there can be many Hubs in an application, but you wouldn't want to trigger the `OnConnected` event on every Hub if you are only going to use to one of them.</span></span> <span data-ttu-id="954e8-197">中樞 proxy 上的用戶端方法的存在時建立連線時，是什麼告訴 SignalR 來觸發`OnConnected`事件。</span><span class="sxs-lookup"><span data-stu-id="954e8-197">When the connection is established, the presence of a client method on a Hub's proxy is what tells SignalR to trigger the `OnConnected` event.</span></span> <span data-ttu-id="954e8-198">如果您未註冊任何事件處理常式，然後再呼叫`start`方法中，您將能夠叫用方法，在中樞中，但是中樞的`OnConnected`方法不會被呼叫，而且沒有用戶端會叫用方法從伺服器。</span><span class="sxs-lookup"><span data-stu-id="954e8-198">If you don't register any event handlers before calling the `start` method, you will be able to invoke methods on the Hub, but the Hub's `OnConnected` method won't be called and no client methods will be invoked from the server.</span></span>


<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a><span data-ttu-id="954e8-199">$。 connection.hub 是相同物件會建立該 $.hubConnection()</span><span class="sxs-lookup"><span data-stu-id="954e8-199">$.connection.hub is the same object that $.hubConnection() creates</span></span>

<span data-ttu-id="954e8-200">當您使用產生的 proxy 時，您可以看到範例中，從`$.connection.hub`參考的連接物件。</span><span class="sxs-lookup"><span data-stu-id="954e8-200">As you can see from the examples, when you use the generated proxy, `$.connection.hub` refers to the connection object.</span></span> <span data-ttu-id="954e8-201">這是您藉由呼叫取得的相同物件`$.hubConnection()`時您不使用產生的 proxy。</span><span class="sxs-lookup"><span data-stu-id="954e8-201">This is the same object that you get by calling `$.hubConnection()` when you aren't using the generated proxy.</span></span> <span data-ttu-id="954e8-202">產生的 proxy 程式碼會執行下列陳述式來建立您的連接：</span><span class="sxs-lookup"><span data-stu-id="954e8-202">The generated proxy code creates the connection for you by executing the following statement:</span></span>

![在產生之 proxy 檔案中建立的連線](signalr-1x-hubs-api-guide-javascript-client/_static/image3.png)

<span data-ttu-id="954e8-204">當您使用產生的 proxy 時，您可以執行任何動作`$.connection.hub`，您可以使用連接物件時您不使用產生的 proxy。</span><span class="sxs-lookup"><span data-stu-id="954e8-204">When you're using the generated proxy, you can do anything with `$.connection.hub` that you can do with a connection object when you're not using the generated proxy.</span></span>

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a><span data-ttu-id="954e8-205">非同步執行的 start 方法</span><span class="sxs-lookup"><span data-stu-id="954e8-205">Asynchronous execution of the start method</span></span>

<span data-ttu-id="954e8-206">`start`方法以非同步方式執行。</span><span class="sxs-lookup"><span data-stu-id="954e8-206">The `start` method executes asynchronously.</span></span> <span data-ttu-id="954e8-207">它會傳回[jQuery 延後物件](http://api.jquery.com/category/deferred-object/)，這表示您可以藉由呼叫方法，像是新增回呼函式`pipe`， `done`，和`fail`。</span><span class="sxs-lookup"><span data-stu-id="954e8-207">It returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/), which means that you can add callback functions by calling methods such as `pipe`, `done`, and `fail`.</span></span> <span data-ttu-id="954e8-208">如果您有您想要在連線建立之後執行的程式碼，例如伺服器方法呼叫，該程式碼放入回呼函式，或從回呼函式呼叫它。</span><span class="sxs-lookup"><span data-stu-id="954e8-208">If you have code that you want to execute after the connection is established, such as a call to a server method, put that code in a callback function or call it from a callback function.</span></span> <span data-ttu-id="954e8-209">`.done`回呼方法之後所建立的連線，以及任何程式碼之後，您必須執行您`OnConnected`事件處理常式方法，在伺服器上的完成執行。</span><span class="sxs-lookup"><span data-stu-id="954e8-209">The `.done` callback method is executed after the connection has been established, and after any code that you have in your `OnConnected` event handler method on the server finishes executing.</span></span>

<span data-ttu-id="954e8-210">如果您將 「 現在已連線 」 陳述式將前述範例中為程式碼之後的下一行`start`方法呼叫 (不是在`.done`回呼)，則`console.log`列將之前，先執行連線，如下列所示範例：</span><span class="sxs-lookup"><span data-stu-id="954e8-210">If you put the "Now connected" statement from the preceding example as the next line of code after the `start` method call (not in a `.done` callback), the `console.log` line will execute before the connection is established, as shown in the following example:</span></span>

![錯誤的方式，撰寫程式碼執行連線之後，](signalr-1x-hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a><span data-ttu-id="954e8-212">如何建立跨網域的連線</span><span class="sxs-lookup"><span data-stu-id="954e8-212">How to establish a cross-domain connection</span></span>

<span data-ttu-id="954e8-213">通常如果瀏覽器載入頁面上，從`http://contoso.com`，SignalR 連線位於相同的網域， `http://contoso.com/signalr`。</span><span class="sxs-lookup"><span data-stu-id="954e8-213">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="954e8-214">如果頁面上，從`http://contoso.com`會連接到`http://fabrikam.com/signalr`，也就是跨網域的連線。</span><span class="sxs-lookup"><span data-stu-id="954e8-214">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="954e8-215">基於安全性理由，預設會停用跨網域的連線。</span><span class="sxs-lookup"><span data-stu-id="954e8-215">For security reasons, cross-domain connections are disabled by default.</span></span> <span data-ttu-id="954e8-216">若要建立跨網域連線，請確定已啟用跨網域連接時，會在伺服器上，並建立連接物件時，指定在連接 URL。</span><span class="sxs-lookup"><span data-stu-id="954e8-216">To establish a cross-domain connection, make sure that cross-domain connections are enabled on the server, and specify the connection URL when you create the connection object.</span></span> <span data-ttu-id="954e8-217">SignalR 會使用適當的技術對於跨網域連線，例如[JSONP](http://en.wikipedia.org/wiki/JSONP)或是[CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing)。</span><span class="sxs-lookup"><span data-stu-id="954e8-217">SignalR will use the appropriate technology for cross-domain connections, such as [JSONP](http://en.wikipedia.org/wiki/JSONP) or [CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing).</span></span>

<span data-ttu-id="954e8-218">在伺服器上，請選取該選項，當您呼叫中啟用跨網域連接`MapHubs`方法。</span><span class="sxs-lookup"><span data-stu-id="954e8-218">On the server, enable cross-domain connections by selecting that option when you call the `MapHubs` method.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample10.cs?highlight=2)]

<span data-ttu-id="954e8-219">在用戶端，請指定 URL，或當您建立連接物件 （不含產生的 proxy) 時才能呼叫 start 方法 （使用產生的 proxy)。</span><span class="sxs-lookup"><span data-stu-id="954e8-219">On the client, specify the URL when you create the connection object (without the generated proxy) or before you call the start method (with the generated proxy).</span></span>

<span data-ttu-id="954e8-220">**指定 （使用產生的 proxy) 的跨網域連接的用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="954e8-220">**Client code that specifies a cross-domain connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample11.js?highlight=1)]

<span data-ttu-id="954e8-221">**指定跨網域連接 （不含產生的 proxy) 的用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="954e8-221">**Client code that specifies a cross-domain connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

<span data-ttu-id="954e8-222">當您使用`$.hubConnection`建構函式，您不需要包含`signalr`在 URL 中因為它會自動新增 (除非您指定`useDefaultUrl`做為`false`)。</span><span class="sxs-lookup"><span data-stu-id="954e8-222">When you use the `$.hubConnection` constructor, you don't have to include `signalr` in the URL because it is added automatically (unless you specify `useDefaultUrl` as `false`).</span></span>

<span data-ttu-id="954e8-223">您可以建立多個連接至不同的端點。</span><span class="sxs-lookup"><span data-stu-id="954e8-223">You can create multiple connections to different endpoints.</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample13.js)]

> [!NOTE] 
> 
> - <span data-ttu-id="954e8-224">不需要設定`jQuery.support.cors`設為 true，在您的程式碼。</span><span class="sxs-lookup"><span data-stu-id="954e8-224">Don't set `jQuery.support.cors` to true in your code.</span></span>
> 
>     ![未將 jQuery.support.cors 設定為 true](signalr-1x-hubs-api-guide-javascript-client/_static/image7.png)
> 
>     <span data-ttu-id="954e8-226">SignalR 處理使用 JSONP 或 CORS。</span><span class="sxs-lookup"><span data-stu-id="954e8-226">SignalR handles the use of JSONP or CORS.</span></span> <span data-ttu-id="954e8-227">設定`jQuery.support.cors`來為 true，則停用 JSONP，因為這會導致 SignalR 假設瀏覽器支援 CORS。</span><span class="sxs-lookup"><span data-stu-id="954e8-227">Setting `jQuery.support.cors` to true disables JSONP because it causes SignalR to assume the browser supports CORS.</span></span>
> - <span data-ttu-id="954e8-228">當您要連線至 localhost URL 時，Internet Explorer 10 不會將它視為跨網域連接，好讓應用程式能夠在本機搭配 IE 10 即使您尚未啟用伺服器上的跨網域連線。</span><span class="sxs-lookup"><span data-stu-id="954e8-228">When you're connecting to a localhost URL, Internet Explorer 10 won't consider it a cross-domain connection, so the application will work locally with IE 10 even if you haven't enabled cross-domain connections on the server.</span></span>
> - <span data-ttu-id="954e8-229">如需使用 Internet Explorer 9 中的跨網域的連線資訊，請參閱[這個 StackOverflow 執行緒](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work)。</span><span class="sxs-lookup"><span data-stu-id="954e8-229">For information about using cross-domain connections with Internet Explorer 9, see [this StackOverflow thread](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span></span>
> - <span data-ttu-id="954e8-230">如需使用 Chrome 中的跨網域的連線資訊，請參閱[這個 StackOverflow 執行緒](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome)。</span><span class="sxs-lookup"><span data-stu-id="954e8-230">For information about using cross-domain connections with Chrome, see [this StackOverflow thread](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span></span>
> - <span data-ttu-id="954e8-231">範例程式碼會使用預設值"/ signalr 」 來連線到您的 SignalR 服務的 URL。</span><span class="sxs-lookup"><span data-stu-id="954e8-231">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="954e8-232">如需有關如何指定不同的基底 URL 的資訊，請參閱 < [ASP.NET SignalR 中樞 API 指南-Server-/signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl)。</span><span class="sxs-lookup"><span data-stu-id="954e8-232">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>


<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="954e8-233">如何設定連線</span><span class="sxs-lookup"><span data-stu-id="954e8-233">How to configure the connection</span></span>

<span data-ttu-id="954e8-234">建立連線之前，您可以指定查詢字串參數，或指定的傳輸方法。</span><span class="sxs-lookup"><span data-stu-id="954e8-234">Before you establish a connection, you can specify query string parameters or specify the transport method.</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="954e8-235">如何指定查詢字串參數</span><span class="sxs-lookup"><span data-stu-id="954e8-235">How to specify query string parameters</span></span>

<span data-ttu-id="954e8-236">如果您想要將資料傳送至伺服器，用戶端連線時，您可以加入連接物件來查詢字串參數。</span><span class="sxs-lookup"><span data-stu-id="954e8-236">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="954e8-237">下列範例示範如何在用戶端程式碼中設定查詢字串參數。</span><span class="sxs-lookup"><span data-stu-id="954e8-237">The following examples show how to set a query string parameter in client code.</span></span>

<span data-ttu-id="954e8-238">**在呼叫 start 方法 （使用產生的 proxy) 之前設定的查詢字串值**</span><span class="sxs-lookup"><span data-stu-id="954e8-238">**Set a query string value before calling the start method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample14.js?highlight=1)]

<span data-ttu-id="954e8-239">**設定查詢字串值，然後再呼叫 start 方法 （不含產生的 proxy)**</span><span class="sxs-lookup"><span data-stu-id="954e8-239">**Set a query string value before calling the start method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample15.js?highlight=2)]

<span data-ttu-id="954e8-240">下列範例示範如何讀取伺服器程式碼中的查詢字串參數。</span><span class="sxs-lookup"><span data-stu-id="954e8-240">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample16.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="954e8-241">如何指定傳輸方法</span><span class="sxs-lookup"><span data-stu-id="954e8-241">How to specify the transport method</span></span>

<span data-ttu-id="954e8-242">連接的程序的一部分，通常與伺服器，以判斷最佳傳輸所支援的伺服器和用戶端交涉，SignalR 用戶端。</span><span class="sxs-lookup"><span data-stu-id="954e8-242">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="954e8-243">如果您已經知道您想要使用哪一個的傳輸，您可以略過此交涉程序指定的傳輸方法，當您呼叫`start`方法。</span><span class="sxs-lookup"><span data-stu-id="954e8-243">If you already know which transport you want to use, you can bypass this negotiation process by specifying the transport method when you call the `start` method.</span></span>

<span data-ttu-id="954e8-244">**指定的傳輸方法 （產生的 proxy) 的用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="954e8-244">**Client code that specifies the transport method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

<span data-ttu-id="954e8-245">**指定傳輸方法 （不含產生的 proxy) 的用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="954e8-245">**Client code that specifies the transport method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

<span data-ttu-id="954e8-246">或者，您可以指定多個傳輸方法，您想要嘗試的 SignalR 的順序：</span><span class="sxs-lookup"><span data-stu-id="954e8-246">As an alternative, you can specify multiple transport methods in the order in which you want SignalR to try them:</span></span>

<span data-ttu-id="954e8-247">**指定自訂傳輸後援配置 （使用產生的 proxy) 的用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="954e8-247">**Client code that specifies a custom transport fallback scheme (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample19.js?highlight=1)]

<span data-ttu-id="954e8-248">**指定自訂傳輸後援配置 （不含產生的 proxy) 的用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="954e8-248">**Client code that specifies a custom transport fallback scheme (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample20.js?highlight=2)]

<span data-ttu-id="954e8-249">您可以使用下列值來指定傳輸方法：</span><span class="sxs-lookup"><span data-stu-id="954e8-249">You can use the following values for specifying the transport method:</span></span>

- <span data-ttu-id="954e8-250">「 webSockets"</span><span class="sxs-lookup"><span data-stu-id="954e8-250">"webSockets"</span></span>
- <span data-ttu-id="954e8-251">「 foreverFrame"</span><span class="sxs-lookup"><span data-stu-id="954e8-251">"foreverFrame"</span></span>
- <span data-ttu-id="954e8-252">"serverSentEvents"</span><span class="sxs-lookup"><span data-stu-id="954e8-252">"serverSentEvents"</span></span>
- <span data-ttu-id="954e8-253">「 longPolling"</span><span class="sxs-lookup"><span data-stu-id="954e8-253">"longPolling"</span></span>

<span data-ttu-id="954e8-254">下列範例顯示如何找出連接正在使用哪一種傳輸方法。</span><span class="sxs-lookup"><span data-stu-id="954e8-254">The following examples show how to find out which transport method is being used by a connection.</span></span>

<span data-ttu-id="954e8-255">**顯示連接 （使用產生的 proxy) 所使用的傳輸方法的用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="954e8-255">**Client code that displays the transport method used by a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample21.js?highlight=2)]

<span data-ttu-id="954e8-256">**顯示連接 （不含產生的 proxy) 所使用的傳輸方法的用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="954e8-256">**Client code that displays the transport method used by a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample22.js?highlight=3)]

<span data-ttu-id="954e8-257">如需如何檢查伺服端程式碼中的傳輸方法的資訊，請參閱[ASP.NET SignalR 中樞 API 指南-Server-如何取得用戶端的資訊，從內容屬性](../guide-to-the-api/hubs-api-guide-server.md#contextproperty)。</span><span class="sxs-lookup"><span data-stu-id="954e8-257">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="954e8-258">如需有關傳輸與後援的詳細資訊，請參閱[SignalR-傳輸和後援簡介](../getting-started/introduction-to-signalr.md#transports)。</span><span class="sxs-lookup"><span data-stu-id="954e8-258">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a><span data-ttu-id="954e8-259">如何取得 Hub 類別的 proxy</span><span class="sxs-lookup"><span data-stu-id="954e8-259">How to get a proxy for a Hub class</span></span>

<span data-ttu-id="954e8-260">每個您所建立的連線物件會封裝包含一或多個中樞類別 SignalR 服務的連接相關資訊。</span><span class="sxs-lookup"><span data-stu-id="954e8-260">Each connection object that you create encapsulates information about a connection to a SignalR service that contains one or more Hub classes.</span></span> <span data-ttu-id="954e8-261">與 Hub 類別進行通訊，您會使用 proxy 物件，您會建立您自己 （如果您不使用產生的 proxy） 或其為您產生。</span><span class="sxs-lookup"><span data-stu-id="954e8-261">To communicate with a Hub class, you use a proxy object which you create yourself (if you're not using the generated proxy) or which is generated for you.</span></span>

<span data-ttu-id="954e8-262">在用戶端 proxy 的名稱會是中樞類別名稱的 camel 案例版本。</span><span class="sxs-lookup"><span data-stu-id="954e8-262">On the client the proxy name is a camel-cased version of the Hub class name.</span></span> <span data-ttu-id="954e8-263">SignalR 會自動將這項變更，讓 JavaScript 程式碼可以符合 JavaScript 的慣例。</span><span class="sxs-lookup"><span data-stu-id="954e8-263">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="954e8-264">**在伺服器上的中樞類別**</span><span class="sxs-lookup"><span data-stu-id="954e8-264">**Hub class on server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

<span data-ttu-id="954e8-265">**取得產生的用戶端 proxy 的參考中樞**</span><span class="sxs-lookup"><span data-stu-id="954e8-265">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample24.js?highlight=1)]

<span data-ttu-id="954e8-266">**建立中樞類別 （不含產生的 proxy) 的用戶端 proxy**</span><span class="sxs-lookup"><span data-stu-id="954e8-266">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="954e8-267">如果裝飾您的中樞類別與`HubName`屬性，請使用完整名稱，而不變更大小寫。</span><span class="sxs-lookup"><span data-stu-id="954e8-267">If you decorate your Hub class with a `HubName` attribute, use the exact name without changing case.</span></span>

<span data-ttu-id="954e8-268">**HubName 屬性的伺服器上的中樞類別**</span><span class="sxs-lookup"><span data-stu-id="954e8-268">**Hub class on server with HubName attribute**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="954e8-269">**取得產生的用戶端 proxy 的參考中樞**</span><span class="sxs-lookup"><span data-stu-id="954e8-269">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample27.js?highlight=1)]

<span data-ttu-id="954e8-270">**建立中樞類別 （不含產生的 proxy) 的用戶端 proxy**</span><span class="sxs-lookup"><span data-stu-id="954e8-270">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample28.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="954e8-271">如何定義伺服器可以呼叫用戶端上的方法</span><span class="sxs-lookup"><span data-stu-id="954e8-271">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="954e8-272">若要定義伺服器可以從中樞呼叫的方法，加入事件處理常式的中樞 proxy 使用`client`屬性產生的 proxy，或是呼叫`on`方法，如果您未使用產生的 proxy。</span><span class="sxs-lookup"><span data-stu-id="954e8-272">To define a method that the server can call from a Hub, add an event handler to the Hub proxy by using the `client` property of the generated proxy, or call the `on` method if you aren't using the generated proxy.</span></span> <span data-ttu-id="954e8-273">參數可以是複雜物件。</span><span class="sxs-lookup"><span data-stu-id="954e8-273">The parameters can be complex objects.</span></span>

<span data-ttu-id="954e8-274">新增事件處理常式，然後再呼叫`start`方法來建立連線。</span><span class="sxs-lookup"><span data-stu-id="954e8-274">Add the event handler before you call the `start` method to establish the connection.</span></span> <span data-ttu-id="954e8-275">(如果您想要新增事件處理常式之後呼叫`start`方法，請參閱中的附註[如何建立連接](#establishconnection)稍早在此文件，並使用定義方法，而不需使用產生的 proxy 所顯示的語法。)</span><span class="sxs-lookup"><span data-stu-id="954e8-275">(If you want to add event handlers after calling the `start` method, see the note in [How to establish a connection](#establishconnection) earlier in this document, and use the syntax shown for defining a method without using the generated proxy.)</span></span>

<span data-ttu-id="954e8-276">方法名稱比對不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="954e8-276">Method name matching is case-insensitive.</span></span> <span data-ttu-id="954e8-277">例如，`Clients.All.addContosoChatMessageToPage`伺服器上將會執行`AddContosoChatMessageToPage`， `addContosoChatMessageToPage`，或`addcontosochatmessagetopage`用戶端上。</span><span class="sxs-lookup"><span data-stu-id="954e8-277">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, or `addcontosochatmessagetopage` on the client.</span></span>

<span data-ttu-id="954e8-278">**定義用戶端 （使用產生的 proxy) 上的方法**</span><span class="sxs-lookup"><span data-stu-id="954e8-278">**Define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample29.js?highlight=2)]

<span data-ttu-id="954e8-279">**若要定義方法 （具有產生的 proxy) 用戶端上的替代方式**</span><span class="sxs-lookup"><span data-stu-id="954e8-279">**Alternate way to define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample30.js?highlight=1-2)]

<span data-ttu-id="954e8-280">**定義方法，在用戶端 （而不需要產生的 proxy，或新增之後呼叫 start 方法時）**</span><span class="sxs-lookup"><span data-stu-id="954e8-280">**Define method on client (without the generated proxy, or when adding after calling the start method)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample31.js?highlight=3)]

<span data-ttu-id="954e8-281">**伺服端程式碼，以呼叫用戶端方法**</span><span class="sxs-lookup"><span data-stu-id="954e8-281">**Server code that calls the client method**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample32.cs?highlight=5)]

<span data-ttu-id="954e8-282">下列範例中包含複雜物件作為方法參數。</span><span class="sxs-lookup"><span data-stu-id="954e8-282">The following examples include a complex object as a method parameter.</span></span>

<span data-ttu-id="954e8-283">**定義方法，採用複雜的物件 （與產生的 proxy) 的用戶端上**</span><span class="sxs-lookup"><span data-stu-id="954e8-283">**Define method on client that takes a complex object (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample33.js?highlight=2-3)]

<span data-ttu-id="954e8-284">**定義可接受 （不含產生的 proxy) 的複雜物件的用戶端上的方法**</span><span class="sxs-lookup"><span data-stu-id="954e8-284">**Define method on client that takes a complex object (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample34.js?highlight=3-4)]

<span data-ttu-id="954e8-285">**定義的複雜物件的伺服器程式碼**</span><span class="sxs-lookup"><span data-stu-id="954e8-285">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="954e8-286">**伺服端程式碼會呼叫使用複雜物件的用戶端方法**</span><span class="sxs-lookup"><span data-stu-id="954e8-286">**Server code that calls the client method using a complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample36.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="954e8-287">如何從用戶端呼叫伺服器方法</span><span class="sxs-lookup"><span data-stu-id="954e8-287">How to call server methods from the client</span></span>

<span data-ttu-id="954e8-288">若要從用戶端呼叫伺服器方法，使用`server`屬性所產生的 proxy 或`invoke`中樞 proxy，如果您未使用產生的 proxy 上的方法。</span><span class="sxs-lookup"><span data-stu-id="954e8-288">To call a server method from the client, use the `server` property of the generated proxy or the `invoke` method on the Hub proxy if you aren't using the generated proxy.</span></span> <span data-ttu-id="954e8-289">傳回值或參數可以是複雜的物件。</span><span class="sxs-lookup"><span data-stu-id="954e8-289">The return value or parameters can be complex objects.</span></span>

<span data-ttu-id="954e8-290">在中樞傳入的方法名稱的 camel 命名法大小寫版本。</span><span class="sxs-lookup"><span data-stu-id="954e8-290">Pass in a camel-case version of the method name on the Hub.</span></span> <span data-ttu-id="954e8-291">SignalR 會自動將這項變更，讓 JavaScript 程式碼可以符合 JavaScript 的慣例。</span><span class="sxs-lookup"><span data-stu-id="954e8-291">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="954e8-292">下列範例會示範如何呼叫伺服器方法沒有傳回值，以及如何呼叫沒有傳回值為伺服器方法。</span><span class="sxs-lookup"><span data-stu-id="954e8-292">The following examples show how to call a server method that doesn't have a return value and how to call a server method that does have a return value.</span></span>

<span data-ttu-id="954e8-293">**伺服器具有屬性的方法沒有 HubMethodName**</span><span class="sxs-lookup"><span data-stu-id="954e8-293">**Server method with no HubMethodName attribute**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample37.cs?highlight=3)]

<span data-ttu-id="954e8-294">**伺服端程式碼定義複雜的物件傳入的參數**</span><span class="sxs-lookup"><span data-stu-id="954e8-294">**Server code that defines the complex object passed in a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample38.cs)]

<span data-ttu-id="954e8-295">**用戶端程式碼叫用伺服器方法 （使用產生的 proxy)**</span><span class="sxs-lookup"><span data-stu-id="954e8-295">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample39.js?highlight=1)]

<span data-ttu-id="954e8-296">**用戶端程式碼叫用伺服器方法 （不含產生的 proxy)**</span><span class="sxs-lookup"><span data-stu-id="954e8-296">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

<span data-ttu-id="954e8-297">如果裝飾適用的中樞方法的`HubMethodName`屬性，請使用該名稱，而不變更大小寫。</span><span class="sxs-lookup"><span data-stu-id="954e8-297">If you decorated the Hub method with a `HubMethodName` attribute, use that name without changing case.</span></span>

<span data-ttu-id="954e8-298">**伺服器方法**HubMethodName 屬性</span><span class="sxs-lookup"><span data-stu-id="954e8-298">**Server method** with a HubMethodName attribute</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample41.cs?highlight=3)]

<span data-ttu-id="954e8-299">**用戶端程式碼叫用伺服器方法 （使用產生的 proxy)**</span><span class="sxs-lookup"><span data-stu-id="954e8-299">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample42.js?highlight=1)]

<span data-ttu-id="954e8-300">**用戶端程式碼叫用伺服器方法 （不含產生的 proxy)**</span><span class="sxs-lookup"><span data-stu-id="954e8-300">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample43.js?highlight=1)]

<span data-ttu-id="954e8-301">上述範例示範如何呼叫沒有傳回值為伺服器方法。</span><span class="sxs-lookup"><span data-stu-id="954e8-301">The preceding examples show how to call a server method that has no return value.</span></span> <span data-ttu-id="954e8-302">下列範例示範如何呼叫伺服器方法，其傳回值。</span><span class="sxs-lookup"><span data-stu-id="954e8-302">The following examples show how to call a server method that has a return value.</span></span>

<span data-ttu-id="954e8-303">**伺服端程式碼之方法的傳回值**</span><span class="sxs-lookup"><span data-stu-id="954e8-303">**Server code for a method that has a return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample44.cs?highlight=3)]

<span data-ttu-id="954e8-304">**用於的庫存類別**傳回值</span><span class="sxs-lookup"><span data-stu-id="954e8-304">**The Stock class used for the** return value</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample45.cs?highlight=1)]

<span data-ttu-id="954e8-305">**用戶端程式碼叫用伺服器方法 （使用產生的 proxy)**</span><span class="sxs-lookup"><span data-stu-id="954e8-305">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample46.js?highlight=2,4-5)]

<span data-ttu-id="954e8-306">**用戶端程式碼叫用伺服器方法 （不含產生的 proxy)**</span><span class="sxs-lookup"><span data-stu-id="954e8-306">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample47.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="954e8-307">如何處理連接的存留期事件</span><span class="sxs-lookup"><span data-stu-id="954e8-307">How to handle connection lifetime events</span></span>

<span data-ttu-id="954e8-308">SignalR 提供以下連線，您可以處理存留期事件：</span><span class="sxs-lookup"><span data-stu-id="954e8-308">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="954e8-309">`starting`： 透過連線傳送任何資料之前引發。</span><span class="sxs-lookup"><span data-stu-id="954e8-309">`starting`: Raised before any data is sent over the connection.</span></span>
- <span data-ttu-id="954e8-310">`received`： 在此連接上收到任何資料時引發。</span><span class="sxs-lookup"><span data-stu-id="954e8-310">`received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="954e8-311">提供已接收的資料。</span><span class="sxs-lookup"><span data-stu-id="954e8-311">Provides the received data.</span></span>
- <span data-ttu-id="954e8-312">`connectionSlow`： 當用戶端偵測到較慢或經常卸除連接時引發。</span><span class="sxs-lookup"><span data-stu-id="954e8-312">`connectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="954e8-313">`reconnecting`： 基礎傳輸可讓您開始重新連線時引發。</span><span class="sxs-lookup"><span data-stu-id="954e8-313">`reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="954e8-314">`reconnected`： 當基礎傳輸已重新連接時引發。</span><span class="sxs-lookup"><span data-stu-id="954e8-314">`reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="954e8-315">`stateChanged`： 連線狀態變更時引發。</span><span class="sxs-lookup"><span data-stu-id="954e8-315">`stateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="954e8-316">提供的舊狀態和新的狀態 （連接、 已連線、 正在重新連線或已中斷連線）。</span><span class="sxs-lookup"><span data-stu-id="954e8-316">Provides the old state and the new state (Connecting, Connected, Reconnecting, or Disconnected).</span></span>
- <span data-ttu-id="954e8-317">`disconnected`： 當連接已中斷連線時引發。</span><span class="sxs-lookup"><span data-stu-id="954e8-317">`disconnected`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="954e8-318">例如，如果您想要顯示警告訊息，可能會造成明顯延遲的連線問題時，處理`connectionSlow`事件。</span><span class="sxs-lookup"><span data-stu-id="954e8-318">For example, if you want to display warning messages when there are connection problems that might cause noticeable delays, handle the `connectionSlow` event.</span></span>

<span data-ttu-id="954e8-319">**（使用產生的 proxy) 來處理 connectionSlow 事件**</span><span class="sxs-lookup"><span data-stu-id="954e8-319">**Handle the connectionSlow event (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample48.js?highlight=1)]

<span data-ttu-id="954e8-320">**處理 connectionSlow 事件 （不含產生的 proxy)**</span><span class="sxs-lookup"><span data-stu-id="954e8-320">**Handle the connectionSlow event (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample49.js?highlight=2)]

<span data-ttu-id="954e8-321">如需詳細資訊，請參閱 <<c0> [ 了解和處理連線存留期事件 SignalR](index.md)。</span><span class="sxs-lookup"><span data-stu-id="954e8-321">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](index.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="954e8-322">如何處理錯誤</span><span class="sxs-lookup"><span data-stu-id="954e8-322">How to handle errors</span></span>

<span data-ttu-id="954e8-323">SignalR JavaScript 用戶端提供`error`可以加入的處理常式的事件。</span><span class="sxs-lookup"><span data-stu-id="954e8-323">The SignalR JavaScript client provides an `error` event that you can add a handler for.</span></span> <span data-ttu-id="954e8-324">您也可以使用 fail 方法將從伺服器方法引動過程所產生的錯誤處理常式。</span><span class="sxs-lookup"><span data-stu-id="954e8-324">You can also use the fail method to add a handler for errors that result from a server method invocation.</span></span>

<span data-ttu-id="954e8-325">如果您未明確啟用在伺服器上的詳細的錯誤訊息，SignalR 在發生錯誤之後所傳回的例外狀況物件包含有關錯誤的最少資訊。</span><span class="sxs-lookup"><span data-stu-id="954e8-325">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="954e8-326">例如，如果呼叫`newContosoChatMessage`失敗，錯誤物件中的錯誤訊息包含 「`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`」 傳送詳細的錯誤訊息，以在生產環境中的用戶端不會建議基於安全性理由，但如果您想要啟用的詳細的錯誤訊息疑難排解的目的，在伺服器上使用下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="954e8-326">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample50.cs?highlight=2)]

<span data-ttu-id="954e8-327">下列範例示範如何新增錯誤事件的處理常式。</span><span class="sxs-lookup"><span data-stu-id="954e8-327">The following example shows how to add a handler for the error event.</span></span>

<span data-ttu-id="954e8-328">**新增錯誤處理常式 （使用產生的 proxy)**</span><span class="sxs-lookup"><span data-stu-id="954e8-328">**Add an error handler (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample51.js?highlight=1)]

<span data-ttu-id="954e8-329">**新增錯誤處理常式 （而不需要產生的 proxy)**</span><span class="sxs-lookup"><span data-stu-id="954e8-329">**Add an error handler (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

<span data-ttu-id="954e8-330">下列範例示範如何處理來自方法引動過程的錯誤。</span><span class="sxs-lookup"><span data-stu-id="954e8-330">The following example shows how to handle an error from a method invocation.</span></span>

<span data-ttu-id="954e8-331">**控制代碼 （與產生的 proxy) 的方法引動過程發生錯誤**</span><span class="sxs-lookup"><span data-stu-id="954e8-331">**Handle an error from a method invocation (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample53.js?highlight=2)]

<span data-ttu-id="954e8-332">**控制代碼 （不含產生的 proxy) 的方法引動過程發生錯誤**</span><span class="sxs-lookup"><span data-stu-id="954e8-332">**Handle an error from a method invocation (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

<span data-ttu-id="954e8-333">如果在方法引動過程失敗，`error`也會引發事件，因此您的程式碼`error`方法處理常式和`.fail`方法回呼會執行。</span><span class="sxs-lookup"><span data-stu-id="954e8-333">If a method invocation fails, the `error` event is also raised, so your code in the `error` method handler and in the `.fail` method callback would execute.</span></span>

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="954e8-334">如何啟用用戶端記錄</span><span class="sxs-lookup"><span data-stu-id="954e8-334">How to enable client-side logging</span></span>

<span data-ttu-id="954e8-335">若要啟用用戶端登入連線，請設定`logging`屬性上的連接物件，然後再呼叫`start`方法來建立連線。</span><span class="sxs-lookup"><span data-stu-id="954e8-335">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="954e8-336">**啟用記錄 （與產生的 proxy)**</span><span class="sxs-lookup"><span data-stu-id="954e8-336">**Enable logging (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample55.js?highlight=1)]

<span data-ttu-id="954e8-337">**啟用記錄功能 （不含產生的 proxy)**</span><span class="sxs-lookup"><span data-stu-id="954e8-337">**Enable logging (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample56.js?highlight=2)]

<span data-ttu-id="954e8-338">若要查看記錄檔，開啟您的瀏覽器開發人員工具，並移至 [主控台] 索引標籤。顯示的逐步指示和螢幕擷取畫面示範如何執行這項操作的教學課程，請參閱[伺服器廣播與 ASP.NET Signalr-Enable Logging](index.md)。</span><span class="sxs-lookup"><span data-stu-id="954e8-338">To see the logs, open your browser's developer tools and go to the Console tab. For a tutorial that shows step-by-step instructions and screen shots that show how to do this, see [Server Broadcast with ASP.NET Signalr - Enable Logging](index.md).</span></span>
