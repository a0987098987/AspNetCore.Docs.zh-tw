---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-server
title: ASP.NET SignalR 中樞 API 指南-Server (SignalR 1.x) |Microsoft Docs
author: pfletcher
description: 本文件提供 SignalR 使用的程式碼範例 demonstratin 1.1 版的 ASP.NET SignalR 中樞 API 的伺服器端程式設計的簡介...
ms.author: riande
ms.date: 04/17/2013
ms.assetid: 03e4b9f5-0fea-4d94-959f-014b2762a301
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: f21f458e790b0103beb5c315bd7c1192e8866da3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824305"
---
<a name="aspnet-signalr-hubs-api-guide---server-signalr-1x"></a><span data-ttu-id="f371b-103">ASP.NET SignalR 中樞 API 指南-Server (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="f371b-103">ASP.NET SignalR Hubs API Guide - Server (SignalR 1.x)</span></span>
====================
<span data-ttu-id="f371b-104">藉由[Patrick Fletcher](https://github.com/pfletcher)， [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="f371b-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="f371b-105">本文件提供使用程式碼範例示範常見的選項進行 ASP.NET SignalR 中樞 API 的伺服器端程式設計 signalr 1.1 版的簡介。</span><span class="sxs-lookup"><span data-stu-id="f371b-105">This document provides an introduction to programming the server side of the ASP.NET SignalR Hubs API for SignalR version 1.1, with code samples demonstrating common options.</span></span>
> 
> <span data-ttu-id="f371b-106">SignalR 中樞 API 可讓您從伺服器連線的用戶端和伺服器的用戶端進行遠端程序呼叫 (Rpc)。</span><span class="sxs-lookup"><span data-stu-id="f371b-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="f371b-107">在伺服器程式碼中，您定義可由用戶端，呼叫的方法，呼叫用戶端執行的方法。</span><span class="sxs-lookup"><span data-stu-id="f371b-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="f371b-108">在用戶端程式碼中，您定義可以在伺服器上，從呼叫的方法，您呼叫在伺服器執行的方法。</span><span class="sxs-lookup"><span data-stu-id="f371b-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="f371b-109">SignalR 會處理所有為您的用戶端-伺服器配管。</span><span class="sxs-lookup"><span data-stu-id="f371b-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="f371b-110">SignalR 也提供一個名為持續連線的較低層級 API。</span><span class="sxs-lookup"><span data-stu-id="f371b-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="f371b-111">如需 SignalR、 中樞和持續連線，或該教學課程說明如何建置完整的 SignalR 應用程式，請參閱[SignalR-開始使用](index.md)。</span><span class="sxs-lookup"><span data-stu-id="f371b-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](index.md).</span></span>


## <a name="overview"></a><span data-ttu-id="f371b-112">總覽</span><span class="sxs-lookup"><span data-stu-id="f371b-112">Overview</span></span>

<span data-ttu-id="f371b-113">本文件包含下列章節：</span><span class="sxs-lookup"><span data-stu-id="f371b-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="f371b-114">如何註冊 SignalR 的路由，並設定 SignalR 選項</span><span class="sxs-lookup"><span data-stu-id="f371b-114">How to register the SignalR route and configure SignalR options</span></span>](#route)

    - [<span data-ttu-id="f371b-115">/Signalr URL</span><span class="sxs-lookup"><span data-stu-id="f371b-115">The /signalr URL</span></span>](#signalrurl)
    - [<span data-ttu-id="f371b-116">設定 SignalR 選項</span><span class="sxs-lookup"><span data-stu-id="f371b-116">Configuring SignalR options</span></span>](#options)
- [<span data-ttu-id="f371b-117">如何建立和使用中樞類別</span><span class="sxs-lookup"><span data-stu-id="f371b-117">How to create and use Hub classes</span></span>](#hubclass)

    - [<span data-ttu-id="f371b-118">中樞物件存留期</span><span class="sxs-lookup"><span data-stu-id="f371b-118">Hub object lifetime</span></span>](#transience)
    - [<span data-ttu-id="f371b-119">依照 camel 命名法大小寫的 JavaScript 用戶端中的中樞名稱</span><span class="sxs-lookup"><span data-stu-id="f371b-119">Camel-casing of Hub names in JavaScript clients</span></span>](#hubnames)
    - [<span data-ttu-id="f371b-120">多個中樞</span><span class="sxs-lookup"><span data-stu-id="f371b-120">Multiple Hubs</span></span>](#multiplehubs)
- [<span data-ttu-id="f371b-121">如何在用戶端可以呼叫 Hub 類別中定義方法</span><span class="sxs-lookup"><span data-stu-id="f371b-121">How to define methods in the Hub class that clients can call</span></span>](#hubmethods)

    - [<span data-ttu-id="f371b-122">依照 camel 命名法大小寫的 JavaScript 用戶端中的方法名稱</span><span class="sxs-lookup"><span data-stu-id="f371b-122">Camel-casing of method names in JavaScript clients</span></span>](#methodnames)
    - [<span data-ttu-id="f371b-123">以非同步方式執行的時機</span><span class="sxs-lookup"><span data-stu-id="f371b-123">When to execute asynchronously</span></span>](#asyncmethods)
    - [<span data-ttu-id="f371b-124">定義多載</span><span class="sxs-lookup"><span data-stu-id="f371b-124">Defining overloads</span></span>](#overloads)
- [<span data-ttu-id="f371b-125">如何從中樞類別呼叫用戶端方法</span><span class="sxs-lookup"><span data-stu-id="f371b-125">How to call client methods from the Hub class</span></span>](#callfromhub)

    - [<span data-ttu-id="f371b-126">選取哪些用戶端會收到 RPC</span><span class="sxs-lookup"><span data-stu-id="f371b-126">Selecting which clients will receive the RPC</span></span>](#selectingclients)
    - [<span data-ttu-id="f371b-127">沒有編譯時期的驗證方法的名稱</span><span class="sxs-lookup"><span data-stu-id="f371b-127">No compile-time validation for method names</span></span>](#dynamicmethodnames)
    - [<span data-ttu-id="f371b-128">不區分大小寫的方法名稱比對</span><span class="sxs-lookup"><span data-stu-id="f371b-128">Case-insensitive method name matching</span></span>](#caseinsensitive)
    - [<span data-ttu-id="f371b-129">非同步執行</span><span class="sxs-lookup"><span data-stu-id="f371b-129">Asynchronous execution</span></span>](#asyncclient)
- [<span data-ttu-id="f371b-130">如何從中樞類別管理群組成員資格</span><span class="sxs-lookup"><span data-stu-id="f371b-130">How to manage group membership from the Hub class</span></span>](#groupsfromhub)

    - [<span data-ttu-id="f371b-131">非同步執行的 Add 和 Remove 方法</span><span class="sxs-lookup"><span data-stu-id="f371b-131">Asynchronous execution of Add and Remove methods</span></span>](#asyncgroupmethods)
    - [<span data-ttu-id="f371b-132">群組成員資格持續性</span><span class="sxs-lookup"><span data-stu-id="f371b-132">Group membership persistence</span></span>](#grouppersistence)
    - [<span data-ttu-id="f371b-133">單一使用者群組</span><span class="sxs-lookup"><span data-stu-id="f371b-133">Single-user groups</span></span>](#singleusergroups)
- [<span data-ttu-id="f371b-134">如何處理中樞類別中的連線存留期事件</span><span class="sxs-lookup"><span data-stu-id="f371b-134">How to handle connection lifetime events in the Hub class</span></span>](#connectionlifetime)

    - [<span data-ttu-id="f371b-135">呼叫 OnConnected、 OnDisconnected 和 OnReconnected 的時機</span><span class="sxs-lookup"><span data-stu-id="f371b-135">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>](#onreconnected)
    - [<span data-ttu-id="f371b-136">不會填入呼叫狀態</span><span class="sxs-lookup"><span data-stu-id="f371b-136">Caller state not populated</span></span>](#nocallerstate)
- [<span data-ttu-id="f371b-137">如何取得用戶端的資訊，從內容屬性</span><span class="sxs-lookup"><span data-stu-id="f371b-137">How to get information about the client from the Context property</span></span>](#contextproperty)
- [<span data-ttu-id="f371b-138">如何將用戶端與中樞類別之間傳遞狀態</span><span class="sxs-lookup"><span data-stu-id="f371b-138">How to pass state between clients and the Hub class</span></span>](#passstate)
- [<span data-ttu-id="f371b-139">如何在中樞類別中處理錯誤</span><span class="sxs-lookup"><span data-stu-id="f371b-139">How to handle errors in the Hub class</span></span>](#handleErrors)
- [<span data-ttu-id="f371b-140">如何呼叫方法的用戶端和管理中樞類別外的群組</span><span class="sxs-lookup"><span data-stu-id="f371b-140">How to call client methods and manage groups from outside the Hub class</span></span>](#callfromoutsidehub)

    - [<span data-ttu-id="f371b-141">呼叫用戶端方法</span><span class="sxs-lookup"><span data-stu-id="f371b-141">Calling client methods</span></span>](#callingclientsoutsidehub)
    - [<span data-ttu-id="f371b-142">管理群組成員資格</span><span class="sxs-lookup"><span data-stu-id="f371b-142">Managing group membership</span></span>](#managinggroupsoutsidehub)
- [<span data-ttu-id="f371b-143">如何啟用追蹤</span><span class="sxs-lookup"><span data-stu-id="f371b-143">How to enable tracing</span></span>](#tracing)
- [<span data-ttu-id="f371b-144">如何自訂中樞管線</span><span class="sxs-lookup"><span data-stu-id="f371b-144">How to customize the Hubs pipeline</span></span>](#hubpipeline)

<span data-ttu-id="f371b-145">如需如何程式用戶端的文件，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="f371b-145">For documentation on how to program clients, see the following resources:</span></span>

- [<span data-ttu-id="f371b-146">SignalR 中樞 API 指南-JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="f371b-146">SignalR Hubs API Guide - JavaScript Client</span></span>](index.md)
- [<span data-ttu-id="f371b-147">SignalR 中樞 API 指南-.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="f371b-147">SignalR Hubs API Guide - .NET Client</span></span>](index.md)

<span data-ttu-id="f371b-148">API 參考主題的連結是 API 的.NET 4.5 版本。</span><span class="sxs-lookup"><span data-stu-id="f371b-148">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="f371b-149">如果您使用.NET 4，請參閱[API 主題的.NET 4 版本](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx)。</span><span class="sxs-lookup"><span data-stu-id="f371b-149">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="route"></a>

## <a name="how-to-register-the-signalr-route-and-configure-signalr-options"></a><span data-ttu-id="f371b-150">如何註冊 SignalR 的路由，並設定 SignalR 選項</span><span class="sxs-lookup"><span data-stu-id="f371b-150">How to register the SignalR route and configure SignalR options</span></span>

<span data-ttu-id="f371b-151">若要定義用戶端將用來連接到您的中樞的路由，請呼叫[MapHubs](https://msdn.microsoft.com/library/system.web.routing.signalrrouteextensions.maphubs(v=vs.111).aspx)應用程式啟動時的方法。</span><span class="sxs-lookup"><span data-stu-id="f371b-151">To define the route that clients will use to connect to your Hub, call the [MapHubs](https://msdn.microsoft.com/library/system.web.routing.signalrrouteextensions.maphubs(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="f371b-152">`MapHubs` 已[擴充方法](https://msdn.microsoft.com/library/vstudio/bb383977.aspx)如`System.Web.Routing.RouteCollection`類別。</span><span class="sxs-lookup"><span data-stu-id="f371b-152">`MapHubs` is an [extension method](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) for the `System.Web.Routing.RouteCollection` class.</span></span> <span data-ttu-id="f371b-153">下列範例示範如何定義中的 SignalR 中樞路由*Global.asax*檔案。</span><span class="sxs-lookup"><span data-stu-id="f371b-153">The following example shows how to define the SignalR Hubs route in the *Global.asax* file.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample1.cs)]

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample2.cs?highlight=5)]

<span data-ttu-id="f371b-154">如果您要新增 SignalR 功能至 ASP.NET MVC 應用程式，請確定 SignalR 的路由，會加入其他路由之前。</span><span class="sxs-lookup"><span data-stu-id="f371b-154">If you are adding SignalR functionality to an ASP.NET MVC application, make sure that the SignalR route is added before the other routes.</span></span> <span data-ttu-id="f371b-155">如需詳細資訊，請參閱 <<c0> [ 教學課程： 開始使用 SignalR 和 MVC 4](index.md)。</span><span class="sxs-lookup"><span data-stu-id="f371b-155">For more information, see [Tutorial: Getting Started with SignalR and MVC 4](index.md).</span></span>

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a><span data-ttu-id="f371b-156">/Signalr URL</span><span class="sxs-lookup"><span data-stu-id="f371b-156">The /signalr URL</span></span>

<span data-ttu-id="f371b-157">根據預設，用戶端將用來連線至中樞的路由 URL 是"/ signalr"。</span><span class="sxs-lookup"><span data-stu-id="f371b-157">By default, the route URL which clients will use to connect to your Hub is "/signalr".</span></span> <span data-ttu-id="f371b-158">（請勿混淆這個 URL，使用 「 signalr/中樞 」 的 URL，也就是自動產生的 JavaScript 檔案。</span><span class="sxs-lookup"><span data-stu-id="f371b-158">(Don't confuse this URL with the "/signalr/hubs" URL, which is for the automatically generated JavaScript file.</span></span> <span data-ttu-id="f371b-159">如需產生之 proxy 的詳細資訊，請參閱[SignalR 中樞 API 指南-JavaScript 用戶端-產生的 proxy，它會為您](index.md)。)</span><span class="sxs-lookup"><span data-stu-id="f371b-159">For more information about the generated proxy, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](index.md).)</span></span>

<span data-ttu-id="f371b-160">可能有異常的情況下，對 SignalR; 無法使用這個基底 URL比方說，您有一個資料夾中名為您的專案*signalr*而且您不想要變更名稱。</span><span class="sxs-lookup"><span data-stu-id="f371b-160">There might be extraordinary circumstances that make this base URL not usable for SignalR; for example, you have a folder in your project named *signalr* and you don't want to change the name.</span></span> <span data-ttu-id="f371b-161">在此情況下，您可以變更的基底 URL，如下列範例所示 (取代"/ signalr 」 中的範例程式碼，使用您所需的 URL)。</span><span class="sxs-lookup"><span data-stu-id="f371b-161">In that case, you can change the base URL, as shown in the following examples (replace "/signalr" in the sample code with your desired URL).</span></span>

<span data-ttu-id="f371b-162">**指定 URL 的伺服器程式碼**</span><span class="sxs-lookup"><span data-stu-id="f371b-162">**Server code that specifies the URL**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample3.cs?highlight=1)]

<span data-ttu-id="f371b-163">**指定的 URL （以產生的 proxy) 的 JavaScript 用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="f371b-163">**JavaScript client code that specifies the URL (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample4.js?highlight=1)]

<span data-ttu-id="f371b-164">**指定的 URL （不含產生的 proxy) 的 JavaScript 用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="f371b-164">**JavaScript client code that specifies the URL (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample5.js?highlight=1)]

<span data-ttu-id="f371b-165">**指定 URL 的.NET 用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="f371b-165">**.NET client code that specifies the URL**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample6.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a><span data-ttu-id="f371b-166">設定 SignalR 選項</span><span class="sxs-lookup"><span data-stu-id="f371b-166">Configuring SignalR Options</span></span>

<span data-ttu-id="f371b-167">多載`MapHubs`方法可讓您指定自訂 URL、 自訂相依性解析程式，以及下列選項：</span><span class="sxs-lookup"><span data-stu-id="f371b-167">Overloads of the `MapHubs` method enable you to specify a custom URL, a custom dependency resolver, and the following options:</span></span>

- <span data-ttu-id="f371b-168">啟用從瀏覽器用戶端的跨網域呼叫。</span><span class="sxs-lookup"><span data-stu-id="f371b-168">Enable cross-domain calls from browser clients.</span></span>

    <span data-ttu-id="f371b-169">通常如果瀏覽器載入頁面上，從`http://contoso.com`，SignalR 連線位於相同的網域， `http://contoso.com/signalr`。</span><span class="sxs-lookup"><span data-stu-id="f371b-169">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="f371b-170">如果頁面上，從`http://contoso.com`會連接到`http://fabrikam.com/signalr`，也就是跨網域的連線。</span><span class="sxs-lookup"><span data-stu-id="f371b-170">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="f371b-171">基於安全性理由，預設會停用跨網域的連線。</span><span class="sxs-lookup"><span data-stu-id="f371b-171">For security reasons, cross-domain connections are disabled by default.</span></span> <span data-ttu-id="f371b-172">如需詳細資訊，請參閱 < [ASP.NET SignalR 中樞 API 指南-JavaScript 用戶端-如何建立跨網域連接](index.md)。</span><span class="sxs-lookup"><span data-stu-id="f371b-172">For more information, see [ASP.NET SignalR Hubs API Guide - JavaScript Client - How to establish a cross-domain connection](index.md).</span></span>
- <span data-ttu-id="f371b-173">啟用詳細的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="f371b-173">Enable detailed error messages.</span></span>

    <span data-ttu-id="f371b-174">發生錯誤時，SignalR 的預設行為是傳送給用戶端通知訊息，而不發生了什麼事的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="f371b-174">When errors occur, the default behavior of SignalR is to send to clients a notification message without details about what happened.</span></span> <span data-ttu-id="f371b-175">詳細的錯誤資訊傳送至用戶端不建議在生產環境，因為惡意使用者可能可以使用您的應用程式的攻擊中的資訊。</span><span class="sxs-lookup"><span data-stu-id="f371b-175">Sending detailed error information to clients is not recommended in production, because malicious users might be able to use the information in attacks against your application.</span></span> <span data-ttu-id="f371b-176">如需疑難排解，您可以使用此選項，暫時啟用 更具參考性的錯誤報告。</span><span class="sxs-lookup"><span data-stu-id="f371b-176">For troubleshooting, you can use this option to temporarily enable more informative error reporting.</span></span>
- <span data-ttu-id="f371b-177">停用自動產生的 JavaScript proxy 檔案。</span><span class="sxs-lookup"><span data-stu-id="f371b-177">Disable automatically generated JavaScript proxy files.</span></span>

    <span data-ttu-id="f371b-178">根據預設，具有 proxy 的 JavaScript 檔案，為您的中樞類別會產生以回應 URL 「 signalr/中樞 」。</span><span class="sxs-lookup"><span data-stu-id="f371b-178">By default, a JavaScript file with proxies for your Hub classes is generated in response to the URL "/signalr/hubs".</span></span> <span data-ttu-id="f371b-179">如果您不想要使用的 JavaScript proxy，或如果您想要以手動方式產生這個檔案，而且參考的實體檔案中您的用戶端，您可以使用此選項以停用 proxy 的產生。</span><span class="sxs-lookup"><span data-stu-id="f371b-179">If you don't want to use the JavaScript proxies, or if you want to generate this file manually and refer to a physical file in your clients, you can use this option to disable proxy generation.</span></span> <span data-ttu-id="f371b-180">如需詳細資訊，請參閱 < [SignalR 中樞 API 指南-JavaScript 用戶端-如何建立 signalr 的實體檔案產生 proxy](index.md)。</span><span class="sxs-lookup"><span data-stu-id="f371b-180">For more information, see [SignalR Hubs API Guide - JavaScript Client - How to create a physical file for the SignalR generated proxy](index.md).</span></span>

<span data-ttu-id="f371b-181">下列範例示範如何呼叫中指定的 SignalR 連線 URL 和這些選項`MapHubs`方法。</span><span class="sxs-lookup"><span data-stu-id="f371b-181">The following example shows how to specify the SignalR connection URL and these options in a call to the `MapHubs` method.</span></span> <span data-ttu-id="f371b-182">若要指定自訂 URL，將"/ signalr 」 在範例中，您想要使用的 url。</span><span class="sxs-lookup"><span data-stu-id="f371b-182">To specify a custom URL, replace "/signalr" in the example with the URL that you want to use.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample7.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a><span data-ttu-id="f371b-183">如何建立和使用中樞類別</span><span class="sxs-lookup"><span data-stu-id="f371b-183">How to create and use Hub classes</span></span>

<span data-ttu-id="f371b-184">若要建立中樞時，建立衍生自類別[Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx)。</span><span class="sxs-lookup"><span data-stu-id="f371b-184">To create a Hub, create a class that derives from [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span></span> <span data-ttu-id="f371b-185">下列範例顯示的交談應用程式的簡單中樞類別。</span><span class="sxs-lookup"><span data-stu-id="f371b-185">The following example shows a simple Hub class for a chat application.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample8.cs)]

<span data-ttu-id="f371b-186">在此範例中，連線的用戶端可以呼叫`NewContosoChatMessage`方法，並收到的資料載入時，會傳播到所有連線的用戶端。</span><span class="sxs-lookup"><span data-stu-id="f371b-186">In this example, a connected client can call the `NewContosoChatMessage` method, and when it does, the data received is broadcasted to all connected clients.</span></span>

<a id="transience"></a>

### <a name="hub-object-lifetime"></a><span data-ttu-id="f371b-187">中樞物件存留期</span><span class="sxs-lookup"><span data-stu-id="f371b-187">Hub object lifetime</span></span>

<span data-ttu-id="f371b-188">您不具現化中樞類別，或從您的伺服器上的程式碼呼叫其方法只是為了由 SignalR 中樞管線。</span><span class="sxs-lookup"><span data-stu-id="f371b-188">You don't instantiate the Hub class or call its methods from your own code on the server; all that is done for you by the SignalR Hubs pipeline.</span></span> <span data-ttu-id="f371b-189">SignalR 建立您的中樞類別的新執行個體每次需要處理的中樞作業，例如用戶端連線、 中斷連線，或對伺服器提出的方法呼叫的時。</span><span class="sxs-lookup"><span data-stu-id="f371b-189">SignalR creates a new instance of your Hub class each time it needs to handle a Hub operation such as when a client connects, disconnects, or makes a method call to the server.</span></span>

<span data-ttu-id="f371b-190">中樞類別的執行個體是暫時性的因為您無法使用它們來維護從到下一個方法呼叫的狀態。</span><span class="sxs-lookup"><span data-stu-id="f371b-190">Because instances of the Hub class are transient, you can't use them to maintain state from one method call to the next.</span></span> <span data-ttu-id="f371b-191">每次伺服器接收的方法呼叫從用戶端 」，這是您的中樞類別程序的新執行個體的訊息。</span><span class="sxs-lookup"><span data-stu-id="f371b-191">Each time the server receives a method call from a client, a new instance of your Hub class processes the message.</span></span> <span data-ttu-id="f371b-192">若要維護透過多條連線和方法呼叫的狀態，使用一些其他方法，例如資料庫或靜態變數 Hub 類別，或不同的類別不是衍生自`Hub`。</span><span class="sxs-lookup"><span data-stu-id="f371b-192">To maintain state through multiple connections and method calls, use some other method such as a database, or a static variable on the Hub class, or a different class that does not derive from `Hub`.</span></span> <span data-ttu-id="f371b-193">如果您將保存在記憶體中的資料時，中樞類別上使用靜態變數之類的方法，資料會遺失應用程式定義域回收時。</span><span class="sxs-lookup"><span data-stu-id="f371b-193">If you persist data in memory, using a method such as a static variable on the Hub class, the data will be lost when the app domain recycles.</span></span>

<span data-ttu-id="f371b-194">如果您想要將訊息傳送至用戶端中，從自己的中樞類別外執行的程式碼，您就無法藉由執行個體化中樞類別的執行個體，但您可以為您的中樞類別取得 SignalR 內容物件的參考來執行。</span><span class="sxs-lookup"><span data-stu-id="f371b-194">If you want to send messages to clients from your own code that runs outside the Hub class, you can't do it by instantiating a Hub class instance, but you can do it by getting a reference to the SignalR context object for your Hub class.</span></span> <span data-ttu-id="f371b-195">如需詳細資訊，請參閱 <<c0> [ 如何呼叫方法的用戶端和管理中樞類別外的群組](#callfromoutsidehub)本主題稍後的。</span><span class="sxs-lookup"><span data-stu-id="f371b-195">For more information, see [How to call client methods and manage groups from outside the Hub class](#callfromoutsidehub) later in this topic.</span></span>

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a><span data-ttu-id="f371b-196">依照 camel 命名法大小寫的 JavaScript 用戶端中的中樞名稱</span><span class="sxs-lookup"><span data-stu-id="f371b-196">Camel-casing of Hub names in JavaScript clients</span></span>

<span data-ttu-id="f371b-197">根據預設，JavaScript 用戶端中樞使用參考的類別名稱的 camel 案例版本。</span><span class="sxs-lookup"><span data-stu-id="f371b-197">By default, JavaScript clients refer to Hubs by using a camel-cased version of the class name.</span></span> <span data-ttu-id="f371b-198">SignalR 會自動將這項變更，讓 JavaScript 程式碼可以符合 JavaScript 的慣例。</span><span class="sxs-lookup"><span data-stu-id="f371b-198">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span> <span data-ttu-id="f371b-199">前一個範例就稱為`contosoChatHub`JavaScript 程式碼。</span><span class="sxs-lookup"><span data-stu-id="f371b-199">The previous example would be referred to as `contosoChatHub` in JavaScript code.</span></span>

<span data-ttu-id="f371b-200">**伺服器**</span><span class="sxs-lookup"><span data-stu-id="f371b-200">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample9.cs?highlight=1)]

<span data-ttu-id="f371b-201">**使用產生之 proxy 的 JavaScript 用戶端**</span><span class="sxs-lookup"><span data-stu-id="f371b-201">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample10.js?highlight=1)]

<span data-ttu-id="f371b-202">如果您想要指定不同的名稱，讓用戶端使用，請將`HubName`屬性。</span><span class="sxs-lookup"><span data-stu-id="f371b-202">If you want to specify a different name for clients to use, add the `HubName` attribute.</span></span> <span data-ttu-id="f371b-203">當您使用`HubName`屬性中，沒有名稱變更為 JavaScript 用戶端上的 camel 案例。</span><span class="sxs-lookup"><span data-stu-id="f371b-203">When you use a `HubName` attribute, there is no name change to camel case on JavaScript clients.</span></span>

<span data-ttu-id="f371b-204">**伺服器**</span><span class="sxs-lookup"><span data-stu-id="f371b-204">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample11.cs?highlight=1)]

<span data-ttu-id="f371b-205">**使用產生之 proxy 的 JavaScript 用戶端**</span><span class="sxs-lookup"><span data-stu-id="f371b-205">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample12.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a><span data-ttu-id="f371b-206">多個中樞</span><span class="sxs-lookup"><span data-stu-id="f371b-206">Multiple Hubs</span></span>

<span data-ttu-id="f371b-207">您可以定義多個中樞類別的應用程式中。</span><span class="sxs-lookup"><span data-stu-id="f371b-207">You can define multiple Hub classes in an application.</span></span> <span data-ttu-id="f371b-208">當您這樣做，時請連線共用，但是群組是分開：</span><span class="sxs-lookup"><span data-stu-id="f371b-208">When you do that, the connection is shared but groups are separate:</span></span>

- <span data-ttu-id="f371b-209">所有用戶端會使用相同的 URL 來建立您的服務與 SignalR 連線 ("/ signalr"或您自訂的 URL，如果您指定一個)，連接適用於所有中樞和服務所定義。</span><span class="sxs-lookup"><span data-stu-id="f371b-209">All clients will use the same URL to establish a SignalR connection with your service ("/signalr" or your custom URL if you specified one), and that connection is used for all Hubs defined by the service.</span></span>

    <span data-ttu-id="f371b-210">沒有任何效能差異的多個中樞，相較於單一類別中定義中樞的所有功能。</span><span class="sxs-lookup"><span data-stu-id="f371b-210">There is no performance difference for multiple Hubs compared to defining all Hub functionality in a single class.</span></span>
- <span data-ttu-id="f371b-211">所有中樞都取得相同 HTTP 要求的資訊。</span><span class="sxs-lookup"><span data-stu-id="f371b-211">All Hubs get the same HTTP request information.</span></span>

    <span data-ttu-id="f371b-212">因為所有中樞都共用相同的連接，伺服器取得的唯一 HTTP 要求資訊是什麼是建立與 SignalR 連線的原始 HTTP 要求中。</span><span class="sxs-lookup"><span data-stu-id="f371b-212">Since all Hubs share the same connection, the only HTTP request information that the server gets is what comes in the original HTTP request that establishes the SignalR connection.</span></span> <span data-ttu-id="f371b-213">如果您使用連線要求到指定的查詢字串從用戶端傳遞資訊給伺服器時，您無法提供不同的查詢字串至不同的中樞。</span><span class="sxs-lookup"><span data-stu-id="f371b-213">If you use the connection request to pass information from the client to the server by specifying a query string, you can't provide different query strings to different Hubs.</span></span> <span data-ttu-id="f371b-214">所有中樞將會都收到相同的資訊。</span><span class="sxs-lookup"><span data-stu-id="f371b-214">All Hubs will receive the same information.</span></span>
- <span data-ttu-id="f371b-215">產生的 JavaScript proxy 檔案會包含一個檔案中的所有主機的 proxy。</span><span class="sxs-lookup"><span data-stu-id="f371b-215">The generated JavaScript proxies file will contain proxies for all Hubs in one file.</span></span>

    <span data-ttu-id="f371b-216">JavaScript proxy 的相關資訊，請參閱[SignalR 中樞 API 指南-JavaScript 用戶端-產生的 proxy，它會為您](index.md)。</span><span class="sxs-lookup"><span data-stu-id="f371b-216">For information about JavaScript proxies, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](index.md).</span></span>
- <span data-ttu-id="f371b-217">在中樞內定義的群組。</span><span class="sxs-lookup"><span data-stu-id="f371b-217">Groups are defined within Hubs.</span></span>

    <span data-ttu-id="f371b-218">Signalr 可以定義名為廣播已連線的用戶端子集的群組。</span><span class="sxs-lookup"><span data-stu-id="f371b-218">In SignalR you can define named groups to broadcast to subsets of connected clients.</span></span> <span data-ttu-id="f371b-219">群組會分別維護每個中樞。</span><span class="sxs-lookup"><span data-stu-id="f371b-219">Groups are maintained separately for each Hub.</span></span> <span data-ttu-id="f371b-220">例如，一個名為"Administrators"群組會包含用戶端的一組您`ContosoChatHub`類別和相同的群組名稱會參考一組不同的用戶端程式`StockTickerHub`類別。</span><span class="sxs-lookup"><span data-stu-id="f371b-220">For example, a group named "Administrators" would include one set of clients for your `ContosoChatHub` class, and the same group name would refer to a different set of clients for your `StockTickerHub` class.</span></span>

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a><span data-ttu-id="f371b-221">如何在用戶端可以呼叫 Hub 類別中定義方法</span><span class="sxs-lookup"><span data-stu-id="f371b-221">How to define methods in the Hub class that clients can call</span></span>

<span data-ttu-id="f371b-222">若要公開您想要從用戶端可呼叫的中樞上的方法，宣告公用的方法，如下列範例所示。</span><span class="sxs-lookup"><span data-stu-id="f371b-222">To expose a method on the Hub that you want to be callable from the client, declare a public method, as shown in the following examples.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample14.cs?highlight=3)]

<span data-ttu-id="f371b-223">您可以指定傳回型別和參數，包括複雜型別和陣列，如同在任何 C# 方法。</span><span class="sxs-lookup"><span data-stu-id="f371b-223">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="f371b-224">接收中的參數或傳回給呼叫者的任何資料之間進行通信時用戶端和伺服器使用 JSON 與 SignalR 複雜物件的繫結和物件的陣列會自動處理。</span><span class="sxs-lookup"><span data-stu-id="f371b-224">Any data that you receive in parameters or return to the caller is communicated between the client and the server by using JSON, and SignalR handles the binding of complex objects and arrays of objects automatically.</span></span>

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a><span data-ttu-id="f371b-225">依照 camel 命名法大小寫的 JavaScript 用戶端中的方法名稱</span><span class="sxs-lookup"><span data-stu-id="f371b-225">Camel-casing of method names in JavaScript clients</span></span>

<span data-ttu-id="f371b-226">根據預設，JavaScript 用戶端中樞方法使用參考的方法名稱的 camel 案例版本。</span><span class="sxs-lookup"><span data-stu-id="f371b-226">By default, JavaScript clients refer to Hub methods by using a camel-cased version of the method name.</span></span> <span data-ttu-id="f371b-227">SignalR 會自動將這項變更，讓 JavaScript 程式碼可以符合 JavaScript 的慣例。</span><span class="sxs-lookup"><span data-stu-id="f371b-227">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="f371b-228">**伺服器**</span><span class="sxs-lookup"><span data-stu-id="f371b-228">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample15.cs?highlight=1)]

<span data-ttu-id="f371b-229">**使用產生之 proxy 的 JavaScript 用戶端**</span><span class="sxs-lookup"><span data-stu-id="f371b-229">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample16.js?highlight=1)]

<span data-ttu-id="f371b-230">如果您想要指定不同的名稱，讓用戶端使用，請將`HubMethodName`屬性。</span><span class="sxs-lookup"><span data-stu-id="f371b-230">If you want to specify a different name for clients to use, add the `HubMethodName` attribute.</span></span>

<span data-ttu-id="f371b-231">**伺服器**</span><span class="sxs-lookup"><span data-stu-id="f371b-231">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample17.cs?highlight=1)]

<span data-ttu-id="f371b-232">**使用產生之 proxy 的 JavaScript 用戶端**</span><span class="sxs-lookup"><span data-stu-id="f371b-232">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a><span data-ttu-id="f371b-233">以非同步方式執行的時機</span><span class="sxs-lookup"><span data-stu-id="f371b-233">When to execute asynchronously</span></span>

<span data-ttu-id="f371b-234">如果方法將會是長時間執行，或者要執行的作業會涉及插撥功能，例如資料庫尋查 」 或 「 web 服務呼叫，請讓中樞方法所傳回的非同步[任務](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)(取代`void`傳回) 或[任務&lt;T&gt; ](https://msdn.microsoft.com/library/dd321424.aspx)物件 (取代`T`傳回型別)。</span><span class="sxs-lookup"><span data-stu-id="f371b-234">If the method will be long-running or has to do work that would involve waiting, such as a database lookup or a web service call, make the Hub method asynchronous by returning a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (in place of `void` return) or [Task&lt;T&gt;](https://msdn.microsoft.com/library/dd321424.aspx) object (in place of `T` return type).</span></span> <span data-ttu-id="f371b-235">當您恢復`Task`從 SignalR 方法的物件會等待`Task`，才能完成，然後它會傳送未包裝的結果傳回給用戶端，因此沒有任何差異，在您程式碼中用戶端的方法呼叫的方式。</span><span class="sxs-lookup"><span data-stu-id="f371b-235">When you return a `Task` object from the method, SignalR waits for the `Task` to complete, and then it sends the unwrapped result back to the client, so there is no difference in how you code the method call in the client.</span></span>

<span data-ttu-id="f371b-236">讓中樞方法非同步可避免使用 WebSocket 傳輸時，封鎖連接。</span><span class="sxs-lookup"><span data-stu-id="f371b-236">Making a Hub method asynchronous avoids blocking the connection when it uses the WebSocket transport.</span></span> <span data-ttu-id="f371b-237">當中樞方法以同步方式執行，而且傳輸 WebSocket 時上從相同的用戶端中樞, 方法的後續引動過程會遭到封鎖，直到完成中樞方法。</span><span class="sxs-lookup"><span data-stu-id="f371b-237">When a Hub method executes synchronously and the transport is WebSocket, subsequent invocations of methods on the Hub from the same client are blocked until the Hub method completes.</span></span>

<span data-ttu-id="f371b-238">下列範例顯示相同的方法以同步方式執行自動程式化，或以非同步的方式，後面接著呼叫其中一個版本的運作方式的 JavaScript 用戶端程式碼。</span><span class="sxs-lookup"><span data-stu-id="f371b-238">The following example shows the same method coded to run synchronously or asynchronously, followed by JavaScript client code that works for calling either version.</span></span>

<span data-ttu-id="f371b-239">**同步**</span><span class="sxs-lookup"><span data-stu-id="f371b-239">**Synchronous**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample19.cs)]

<span data-ttu-id="f371b-240">**非同步-ASP.NET 4.5**</span><span class="sxs-lookup"><span data-stu-id="f371b-240">**Asynchronous - ASP.NET 4.5**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

<span data-ttu-id="f371b-241">**使用產生之 proxy 的 JavaScript 用戶端**</span><span class="sxs-lookup"><span data-stu-id="f371b-241">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample21.js)]

<span data-ttu-id="f371b-242">如需如何使用 ASP.NET 4.5 中的非同步方法的詳細資訊，請參閱[ASP.NET MVC 4 中使用非同步方法](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md)。</span><span class="sxs-lookup"><span data-stu-id="f371b-242">For more information about how to use asynchronous methods in ASP.NET 4.5, see [Using Asynchronous Methods in ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span></span>

<a id="overloads"></a>

### <a name="defining-overloads"></a><span data-ttu-id="f371b-243">定義多載</span><span class="sxs-lookup"><span data-stu-id="f371b-243">Defining Overloads</span></span>

<span data-ttu-id="f371b-244">如果您想要定義多載方法，在每個多載的參數數目必須不同。</span><span class="sxs-lookup"><span data-stu-id="f371b-244">If you want to define overloads for a method, the number of parameters in each overload must be different.</span></span> <span data-ttu-id="f371b-245">如果您只要指定不同的參數類型區分的多載，將會編譯您的中樞類別，但 SignalR 服務將會擲回的例外狀況，在執行階段，當用戶端嘗試呼叫其中一個多載。</span><span class="sxs-lookup"><span data-stu-id="f371b-245">If you differentiate an overload just by specifying different parameter types, your Hub class will compile but the SignalR service will throw an exception at run time when clients try to call one of the overloads.</span></span>

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a><span data-ttu-id="f371b-246">如何從中樞類別呼叫用戶端方法</span><span class="sxs-lookup"><span data-stu-id="f371b-246">How to call client methods from the Hub class</span></span>

<span data-ttu-id="f371b-247">若要從伺服器呼叫用戶端方法，請使用`Clients`中樞類別中的方法中的屬性。</span><span class="sxs-lookup"><span data-stu-id="f371b-247">To call client methods from the server, use the `Clients` property in a method in your Hub class.</span></span> <span data-ttu-id="f371b-248">下列範例示範伺服端程式碼呼叫`addNewMessageToPage`上所有已連線的用戶端，並定義的方法在 JavaScript 用戶端的用戶端程式碼。</span><span class="sxs-lookup"><span data-stu-id="f371b-248">The following example shows server code that calls `addNewMessageToPage` on all connected clients, and client code that defines the method in a JavaScript client.</span></span>

<span data-ttu-id="f371b-249">**伺服器**</span><span class="sxs-lookup"><span data-stu-id="f371b-249">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample22.cs?highlight=5)]

<span data-ttu-id="f371b-250">**使用產生之 proxy 的 JavaScript 用戶端**</span><span class="sxs-lookup"><span data-stu-id="f371b-250">**JavaScript client using generated proxy**</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-server/samples/sample23.html?highlight=1)]

<span data-ttu-id="f371b-251">您無法從用戶端方法，取得傳回值這類的語法`int x = Clients.All.add(1,1)`無法運作。</span><span class="sxs-lookup"><span data-stu-id="f371b-251">You can't get a return value from a client method; syntax such as `int x = Clients.All.add(1,1)` does not work.</span></span>

<span data-ttu-id="f371b-252">您可以指定複雜型別和參數的陣列。</span><span class="sxs-lookup"><span data-stu-id="f371b-252">You can specify complex types and arrays for the parameters.</span></span> <span data-ttu-id="f371b-253">下列範例會將複雜型別傳遞至方法參數的用戶端。</span><span class="sxs-lookup"><span data-stu-id="f371b-253">The following example passes a complex type to the client in a method parameter.</span></span>

<span data-ttu-id="f371b-254">**呼叫用戶端方法，使用複雜物件的伺服器程式碼**</span><span class="sxs-lookup"><span data-stu-id="f371b-254">**Server code that calls a client method using a complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample24.cs?highlight=3)]

<span data-ttu-id="f371b-255">**定義的複雜物件的伺服器程式碼**</span><span class="sxs-lookup"><span data-stu-id="f371b-255">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample25.cs?highlight=1)]

<span data-ttu-id="f371b-256">**使用產生之 proxy 的 JavaScript 用戶端**</span><span class="sxs-lookup"><span data-stu-id="f371b-256">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample26.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a><span data-ttu-id="f371b-257">選取哪些用戶端會收到 RPC</span><span class="sxs-lookup"><span data-stu-id="f371b-257">Selecting which clients will receive the RPC</span></span>

<span data-ttu-id="f371b-258">用戶端屬性會傳回[HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx)提供數個選項，來指定哪些用戶端會收到 RPC 的物件：</span><span class="sxs-lookup"><span data-stu-id="f371b-258">The Clients property returns a [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) object that provides several options for specifying which clients will receive the RPC:</span></span>

- <span data-ttu-id="f371b-259">所有已連線的用戶端。</span><span class="sxs-lookup"><span data-stu-id="f371b-259">All connected clients.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample27.cs)]
- <span data-ttu-id="f371b-260">只有呼叫的用戶端。</span><span class="sxs-lookup"><span data-stu-id="f371b-260">Only the calling client.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample28.cs)]
- <span data-ttu-id="f371b-261">除了呼叫用戶端的所有用戶端。</span><span class="sxs-lookup"><span data-stu-id="f371b-261">All clients except the calling client.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample29.cs)]
- <span data-ttu-id="f371b-262">連接識別碼所識別的特定用戶端</span><span class="sxs-lookup"><span data-stu-id="f371b-262">A specific client identified by connection ID.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample30.css)]

    <span data-ttu-id="f371b-263">這個範例會呼叫`addContosoChatMessageToPage`呼叫的用戶端上且具有相同的效果與使用`Clients.Caller`。</span><span class="sxs-lookup"><span data-stu-id="f371b-263">This example calls `addContosoChatMessageToPage` on the calling client and has the same effect as using `Clients.Caller`.</span></span>
- <span data-ttu-id="f371b-264">所有已連線的用戶端，除了指定的用戶端，識別連接識別碼。</span><span class="sxs-lookup"><span data-stu-id="f371b-264">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample31.cs)]
- <span data-ttu-id="f371b-265">指定群組中所有已連線的用戶端。</span><span class="sxs-lookup"><span data-stu-id="f371b-265">All connected clients in a specified group.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample32.css)]
- <span data-ttu-id="f371b-266">指定群組中所有連線的用戶端除了指定的用戶端，識別連接識別碼。</span><span class="sxs-lookup"><span data-stu-id="f371b-266">All connected clients in a specified group except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample33.cs)]
- <span data-ttu-id="f371b-267">指定群組中所有連線的用戶端除了呼叫用戶端。</span><span class="sxs-lookup"><span data-stu-id="f371b-267">All connected clients in a specified group except the calling client.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample34.css)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a><span data-ttu-id="f371b-268">沒有編譯時期的驗證方法的名稱</span><span class="sxs-lookup"><span data-stu-id="f371b-268">No compile-time validation for method names</span></span>

<span data-ttu-id="f371b-269">您指定的方法名稱會解譯為動態物件，這表示沒有任何 IntelliSense 或它的編譯時間驗證。</span><span class="sxs-lookup"><span data-stu-id="f371b-269">The method name that you specify is interpreted as a dynamic object, which means there is no IntelliSense or compile-time validation for it.</span></span> <span data-ttu-id="f371b-270">在執行階段會評估運算式。</span><span class="sxs-lookup"><span data-stu-id="f371b-270">The expression is evaluated at run time.</span></span> <span data-ttu-id="f371b-271">當方法呼叫執行時，SignalR 將方法名稱和參數值傳送至用戶端，且如果用戶端有一個方法符合名稱、 呼叫的方法和參數值傳遞給它。</span><span class="sxs-lookup"><span data-stu-id="f371b-271">When the method call executes, SignalR sends the method name and the parameter values to the client, and if the client has a method that matches the name, that method is called and the parameter values are passed to it.</span></span> <span data-ttu-id="f371b-272">如果用戶端上找不到任何相符的方法，不會引發錯誤。</span><span class="sxs-lookup"><span data-stu-id="f371b-272">If no matching method is found on the client, no error is raised.</span></span> <span data-ttu-id="f371b-273">SignalR 傳輸至用戶端在幕後，當您呼叫的用戶端方法的資料格式的相關資訊，請參閱[SignalR 簡介](index.md)。</span><span class="sxs-lookup"><span data-stu-id="f371b-273">For information about the format of the data that SignalR transmits to the client behind the scenes when you call a client method, see [Introduction to SignalR](index.md).</span></span>

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a><span data-ttu-id="f371b-274">不區分大小寫的方法名稱比對</span><span class="sxs-lookup"><span data-stu-id="f371b-274">Case-insensitive method name matching</span></span>

<span data-ttu-id="f371b-275">方法名稱比對不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="f371b-275">Method name matching is case-insensitive.</span></span> <span data-ttu-id="f371b-276">例如，`Clients.All.addContosoChatMessageToPage`伺服器上將會執行`AddContosoChatMessageToPage`， `addcontosochatmessagetopage`，或`addContosoChatMessageToPage`用戶端上。</span><span class="sxs-lookup"><span data-stu-id="f371b-276">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, or `addContosoChatMessageToPage` on the client.</span></span>

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a><span data-ttu-id="f371b-277">非同步執行</span><span class="sxs-lookup"><span data-stu-id="f371b-277">Asynchronous execution</span></span>

<span data-ttu-id="f371b-278">您呼叫的方法以非同步方式執行。</span><span class="sxs-lookup"><span data-stu-id="f371b-278">The method that you call executes asynchronously.</span></span> <span data-ttu-id="f371b-279">來自用戶端的方法呼叫會立即執行，而不需等待 SignalR 完成傳輸資料到用戶端，除非您另外指定，接下來的幾行程式碼之後的任何程式碼應該等候方法完成。</span><span class="sxs-lookup"><span data-stu-id="f371b-279">Any code that comes after a method call to a client will execute immediately without waiting for SignalR to finish transmitting data to clients unless you specify that the subsequent lines of code should wait for method completion.</span></span> <span data-ttu-id="f371b-280">下列的程式碼範例示範如何以循序方式執行兩個用戶端方法，一個使用程式碼，使其能夠在.NET 4.5 中的，一個使用程式碼，使其能夠在.NET 4。</span><span class="sxs-lookup"><span data-stu-id="f371b-280">The following code examples show how to execute two client methods sequentially, one using code that works in .NET 4.5, and one using code that works in .NET 4.</span></span>

<span data-ttu-id="f371b-281">**.NET 4.5 範例**</span><span class="sxs-lookup"><span data-stu-id="f371b-281">**.NET 4.5 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample35.cs?highlight=1,3)]

<span data-ttu-id="f371b-282">**.NET 4 的範例**</span><span class="sxs-lookup"><span data-stu-id="f371b-282">**.NET 4 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample36.cs?highlight=3-4)]

<span data-ttu-id="f371b-283">如果您使用`await`或`ContinueWith`等候，直到用戶端方法完成執行下的一行程式碼之前，這不表示用戶端會實際收到的訊息下的一行程式碼執行之前。</span><span class="sxs-lookup"><span data-stu-id="f371b-283">If you use `await` or `ContinueWith` to wait until a client method finishes before the next line of code executes, that does not mean that clients will actually receive the message before the next line of code executes.</span></span> <span data-ttu-id="f371b-284">用戶端方法呼叫的 「 完成 」 只表示 SignalR 已完成傳送訊息所需的所有項目。</span><span class="sxs-lookup"><span data-stu-id="f371b-284">"Completion" of a client method call only means that SignalR has done everything necessary to send the message.</span></span> <span data-ttu-id="f371b-285">如果您需要驗證用戶端收到訊息時，您必須自行進行程式設計的機制。</span><span class="sxs-lookup"><span data-stu-id="f371b-285">If you need verification that clients received the message, you have to program that mechanism yourself.</span></span> <span data-ttu-id="f371b-286">例如，您可以撰寫`MessageReceived`方法，在中樞，並在`addContosoChatMessageToPage`方法，您可以呼叫用戶端上的`MessageReceived`這麼做之後，任何正常運作，您需要在用戶端上。</span><span class="sxs-lookup"><span data-stu-id="f371b-286">For example, you could code a `MessageReceived` method on the Hub, and in the `addContosoChatMessageToPage` method on the client you could call `MessageReceived` after you do whatever work you need to do on the client.</span></span> <span data-ttu-id="f371b-287">在 `MessageReceived`中樞中您可以執行任何工作取決於實際的用戶端接收和處理的原始方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="f371b-287">In `MessageReceived` in the Hub you can do whatever work depends on actual client reception and processing of the original method call.</span></span>

### <a name="how-to-use-a-string-variable-as-the-method-name"></a><span data-ttu-id="f371b-288">如何使用做為方法名稱的字串變數</span><span class="sxs-lookup"><span data-stu-id="f371b-288">How to use a string variable as the method name</span></span>

<span data-ttu-id="f371b-289">如果您想要使用的字串變數的方法名稱，轉換為叫用用戶端方法`Clients.All`(或`Clients.Others`，`Clients.Caller`等等) 要`IClientProxy`，然後呼叫[叫用 （methodName、 args...）](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="f371b-289">If you want to invoke a client method by using a string variable as the method name, cast `Clients.All` (or `Clients.Others`, `Clients.Caller`, etc.) to `IClientProxy` and then call [Invoke(methodName, args...)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample37.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a><span data-ttu-id="f371b-290">如何從中樞類別管理群組成員資格</span><span class="sxs-lookup"><span data-stu-id="f371b-290">How to manage group membership from the Hub class</span></span>

<span data-ttu-id="f371b-291">Signalr 的群組提供一種方法將訊息廣播至連線的用戶端的指定子集。</span><span class="sxs-lookup"><span data-stu-id="f371b-291">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="f371b-292">群組可以有任意數目的用戶端，並在用戶端可以是任意數目的群組成員。</span><span class="sxs-lookup"><span data-stu-id="f371b-292">A group can have any number of clients, and a client can be a member of any number of groups.</span></span>

<span data-ttu-id="f371b-293">若要管理群組成員資格，請使用[新增](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx)並[移除](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx)所提供的方法`Groups`中樞類別的屬性。</span><span class="sxs-lookup"><span data-stu-id="f371b-293">To manage group membership, use the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) and [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods provided by the `Groups` property of the Hub class.</span></span> <span data-ttu-id="f371b-294">下列範例所示`Groups.Add`和`Groups.Remove`用中樞方法呼叫的用戶端程式碼，在方法後面 JavaScript 用戶端程式碼呼叫它們。</span><span class="sxs-lookup"><span data-stu-id="f371b-294">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods that are called by client code, followed by JavaScript client code that calls them.</span></span>

<span data-ttu-id="f371b-295">**伺服器**</span><span class="sxs-lookup"><span data-stu-id="f371b-295">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample38.cs?highlight=5,10)]

<span data-ttu-id="f371b-296">**使用產生之 proxy 的 JavaScript 用戶端**</span><span class="sxs-lookup"><span data-stu-id="f371b-296">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample39.js)]

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample40.js)]

<span data-ttu-id="f371b-297">您不需要明確建立群組。</span><span class="sxs-lookup"><span data-stu-id="f371b-297">You don't have to explicitly create groups.</span></span> <span data-ttu-id="f371b-298">作用中的群組會自動建立第一次呼叫中指定其名稱`Groups.Add`，和它的成員資格中移除最後一次連接時，它會刪除。</span><span class="sxs-lookup"><span data-stu-id="f371b-298">In effect a group is automatically created the first time you specify its name in a call to `Groups.Add`, and it is deleted when you remove the last connection from membership in it.</span></span>

<span data-ttu-id="f371b-299">取得群組成員資格清單或群組的清單中沒有任何 API。</span><span class="sxs-lookup"><span data-stu-id="f371b-299">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="f371b-300">SignalR 將訊息傳送至用戶端與群組依據[pub/sub 模型](http://en.wikipedia.org/wiki/Publish/subscribe)，和伺服器不會維護的群組或群組成員資格清單。</span><span class="sxs-lookup"><span data-stu-id="f371b-300">SignalR sends messages to clients and groups based on a [pub/sub model](http://en.wikipedia.org/wiki/Publish/subscribe), and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="f371b-301">這可協助最大化擴充性，因為每當您將節點加入至 web 伺服陣列時，SignalR 維護任何狀態傳播到新的節點。</span><span class="sxs-lookup"><span data-stu-id="f371b-301">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a><span data-ttu-id="f371b-302">非同步執行的 Add 和 Remove 方法</span><span class="sxs-lookup"><span data-stu-id="f371b-302">Asynchronous execution of Add and Remove methods</span></span>

<span data-ttu-id="f371b-303">`Groups.Add`和`Groups.Remove`方法以非同步方式執行。</span><span class="sxs-lookup"><span data-stu-id="f371b-303">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span> <span data-ttu-id="f371b-304">如果您想要加入群組中的用戶端，並立即將訊息傳送至用戶端使用群組，您必須確定`Groups.Add`方法完成第一次。</span><span class="sxs-lookup"><span data-stu-id="f371b-304">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the `Groups.Add` method finishes first.</span></span> <span data-ttu-id="f371b-305">下列程式碼範例示範如何這樣做，請使用適用於.NET 4.5 中，並使用.NET 4 中的運作方式的程式碼的程式碼</span><span class="sxs-lookup"><span data-stu-id="f371b-305">The following code examples show how to do that, one by using code that works in .NET 4.5, and one by using code that works in .NET 4</span></span>

<span data-ttu-id="f371b-306">**.NET 4.5 範例**</span><span class="sxs-lookup"><span data-stu-id="f371b-306">**.NET 4.5 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

<span data-ttu-id="f371b-307">**.NET 4 的範例**</span><span class="sxs-lookup"><span data-stu-id="f371b-307">**.NET 4 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample42.cs?highlight=3-4)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a><span data-ttu-id="f371b-308">群組成員資格持續性</span><span class="sxs-lookup"><span data-stu-id="f371b-308">Group membership persistence</span></span>

<span data-ttu-id="f371b-309">SignalR 追蹤連線，而不是使用者，因此如果您希望使用者是相同群組中每次使用者建立的連接，您必須呼叫`Groups.Add`每次使用者建立新的連接。</span><span class="sxs-lookup"><span data-stu-id="f371b-309">SignalR tracks connections, not users, so if you want a user to be in the same group every time the user establishes a connection, you have to call `Groups.Add` every time the user establishes a new connection.</span></span>

<span data-ttu-id="f371b-310">之後的連線暫時中斷，有時候 SignalR 可以連線自動還原。</span><span class="sxs-lookup"><span data-stu-id="f371b-310">After a temporary loss of connectivity, sometimes SignalR can restore the connection automatically.</span></span> <span data-ttu-id="f371b-311">在此情況下，SignalR 還原相同的連線，不建立新的連接，並因此會自動還原用戶端的群組成員資格。</span><span class="sxs-lookup"><span data-stu-id="f371b-311">In that case, SignalR is restoring the same connection, not establishing a new connection, and so the client's group membership is automatically restored.</span></span> <span data-ttu-id="f371b-312">這可能是甚至暫時中斷時的結果伺服器重新啟動或失敗，因為每個用戶端，包括群組成員資格的連接狀態會是往返用戶端。</span><span class="sxs-lookup"><span data-stu-id="f371b-312">This is possible even when the temporary break is the result of a server reboot or failure, because connection state for each client, including group memberships, is round-tripped to the client.</span></span> <span data-ttu-id="f371b-313">如果在伺服器故障，而被新的伺服器連線逾時之前，用戶端可以自動重新連線至新的伺服器，並重新註冊的群組的成員。</span><span class="sxs-lookup"><span data-stu-id="f371b-313">If a server goes down and is replaced by a new server before the connection times out, a client can reconnect automatically to the new server and re-enroll in groups it is a member of.</span></span>

<span data-ttu-id="f371b-314">當連線無法自動還原後失去連線，或當連線逾時，或用戶端中斷連線 （例如，當瀏覽器瀏覽至新頁面），群組成員資格將會遺失。</span><span class="sxs-lookup"><span data-stu-id="f371b-314">When a connection can't be restored automatically after a loss of connectivity, or when the connection times out, or when the client disconnects (for example, when a browser navigates to a new page), group memberships are lost.</span></span> <span data-ttu-id="f371b-315">下次使用者連線會是新的連接。</span><span class="sxs-lookup"><span data-stu-id="f371b-315">The next time the user connects will be a new connection.</span></span> <span data-ttu-id="f371b-316">若要維護群組成員資格，相同的使用者建立新的連接時，您的應用程式必須進行追蹤、 使用者和群組之間的關聯和還原群組成員資格的每當使用者建立新的連接。</span><span class="sxs-lookup"><span data-stu-id="f371b-316">To maintain group memberships when the same user establishes a new connection, your application has to track the associations between users and groups, and restore group memberships each time a user establishes a new connection.</span></span>

<span data-ttu-id="f371b-317">如需連線和重新連線的詳細資訊，請參閱[如何處理連線存留期事件中樞類別](#connectionlifetime)本主題稍後的。</span><span class="sxs-lookup"><span data-stu-id="f371b-317">For more information about connections and reconnections, see [How to handle connection lifetime events in the Hub class](#connectionlifetime) later in this topic.</span></span>

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a><span data-ttu-id="f371b-318">單一使用者群組</span><span class="sxs-lookup"><span data-stu-id="f371b-318">Single-user groups</span></span>

<span data-ttu-id="f371b-319">通常使用 SignalR 的應用程式需要追蹤的使用者與連線之間的關聯，才能知道哪位使用者已傳送訊息，以及哪些使用者應接收訊息。</span><span class="sxs-lookup"><span data-stu-id="f371b-319">Applications that use SignalR typically have to keep track of the associations between users and connections in order to know which user has sent a message and which user(s) should be receiving a message.</span></span> <span data-ttu-id="f371b-320">其中兩個常用的模式中使用群組來達成目的。</span><span class="sxs-lookup"><span data-stu-id="f371b-320">Groups are used in one of the two commonly used patterns for doing that.</span></span>

- <span data-ttu-id="f371b-321">單一使用者群組。</span><span class="sxs-lookup"><span data-stu-id="f371b-321">Single-user groups.</span></span>

    <span data-ttu-id="f371b-322">您可以指定的使用者名稱與群組名稱，並將目前的連線識別碼新增至群組，每次使用者連線或重新連線。</span><span class="sxs-lookup"><span data-stu-id="f371b-322">You can specify the user name as the group name, and add the current connection ID to the group every time the user connects or reconnects.</span></span> <span data-ttu-id="f371b-323">若要將訊息傳送給您傳送給群組的使用者。</span><span class="sxs-lookup"><span data-stu-id="f371b-323">To send messages to the user you send to the group.</span></span> <span data-ttu-id="f371b-324">這個方法的缺點是群組不提供您了解使用者在線上或離線的方式。</span><span class="sxs-lookup"><span data-stu-id="f371b-324">A disadvantage of this method is that the group doesn't provide you with a way to find out if the user is online or offline.</span></span>
- <span data-ttu-id="f371b-325">追蹤使用者名稱和連線識別碼之間的關聯。</span><span class="sxs-lookup"><span data-stu-id="f371b-325">Track associations between user names and connection IDs.</span></span>

    <span data-ttu-id="f371b-326">您可以在字典或資料庫中儲存每個使用者名稱與一或多個連線識別碼之間的關聯，並更新儲存的資料每次使用者連線或中斷連線。</span><span class="sxs-lookup"><span data-stu-id="f371b-326">You can store an association between each user name and one or more connection IDs in a dictionary or database, and update the stored data each time the user connects or disconnects.</span></span> <span data-ttu-id="f371b-327">若要傳送訊息給使用者，您可以指定連線識別碼。</span><span class="sxs-lookup"><span data-stu-id="f371b-327">To send messages to the user you specify the connection IDs.</span></span> <span data-ttu-id="f371b-328">這個方法的缺點是它會耗用更多的記憶體。</span><span class="sxs-lookup"><span data-stu-id="f371b-328">A disadvantage of this method is that it takes more memory.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a><span data-ttu-id="f371b-329">如何處理中樞類別中的連線存留期事件</span><span class="sxs-lookup"><span data-stu-id="f371b-329">How to handle connection lifetime events in the Hub class</span></span>

<span data-ttu-id="f371b-330">處理連線存留期事件的常見原因是為了追蹤是否已連線，以及追蹤的使用者名稱和連線識別碼之間的關聯。</span><span class="sxs-lookup"><span data-stu-id="f371b-330">Typical reasons for handling connection lifetime events are to keep track of whether a user is connected or not, and to keep track of the association between user names and connection IDs.</span></span> <span data-ttu-id="f371b-331">若要執行自己的程式碼，用戶端連線或中斷連線時，覆寫`OnConnected`， `OnDisconnected`，和`OnReconnected`類別虛擬方法的中樞，如下列範例所示。</span><span class="sxs-lookup"><span data-stu-id="f371b-331">To run your own code when clients connect or disconnect, override the `OnConnected`, `OnDisconnected`, and `OnReconnected` virtual methods of the Hub class, as shown in the following example.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample43.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a><span data-ttu-id="f371b-332">呼叫 OnConnected、 OnDisconnected 和 OnReconnected 的時機</span><span class="sxs-lookup"><span data-stu-id="f371b-332">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>

<span data-ttu-id="f371b-333">瀏覽器巡覽至新的頁面上，每次新的連線已建立，這表示將會執行 SignalR`OnDisconnected`方法，後面`OnConnected`方法。</span><span class="sxs-lookup"><span data-stu-id="f371b-333">Each time a browser navigates to a new page, a new connection has to be established, which means SignalR will execute the `OnDisconnected` method followed by the `OnConnected` method.</span></span> <span data-ttu-id="f371b-334">建立新的連線時，SignalR 一律會建立新的連線識別碼。</span><span class="sxs-lookup"><span data-stu-id="f371b-334">SignalR always creates a new connection ID when a new connection is established.</span></span>

<span data-ttu-id="f371b-335">`OnReconnected`發生時暫時中斷 SignalR 可以自動復原，例如當暫時中斷連線且連線逾時之前，重新連接纜線的連線，會呼叫方法。`OnDisconnected`用戶端中斷連線和 SignalR 無法自動重新連線，例如當瀏覽器導覽至新頁面時，會呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="f371b-335">The `OnReconnected` method is called when there has been a temporary break in connectivity that SignalR can automatically recover from, such as when a cable is temporarily disconnected and reconnected before the connection times out. The `OnDisconnected` method is called when the client is disconnected and SignalR can't automatically reconnect, such as when a browser navigates to a new page.</span></span> <span data-ttu-id="f371b-336">因此，可以指定用戶端的事件序列是`OnConnected`， `OnReconnected`， `OnDisconnected`; 或是`OnConnected`， `OnDisconnected`。</span><span class="sxs-lookup"><span data-stu-id="f371b-336">Therefore, a possible sequence of events for a given client is `OnConnected`, `OnReconnected`, `OnDisconnected`; or `OnConnected`, `OnDisconnected`.</span></span> <span data-ttu-id="f371b-337">您不會看到序列`OnConnected`， `OnDisconnected`，`OnReconnected`指定的連接。</span><span class="sxs-lookup"><span data-stu-id="f371b-337">You won't see the sequence `OnConnected`, `OnDisconnected`, `OnReconnected` for a given connection.</span></span>

<span data-ttu-id="f371b-338">`OnDisconnected`方法不會呼叫在某些情況下，例如當伺服器關閉時或應用程式定義域取得回收。</span><span class="sxs-lookup"><span data-stu-id="f371b-338">The `OnDisconnected` method doesn't get called in some scenarios, such as when a server goes down or the App Domain gets recycled.</span></span> <span data-ttu-id="f371b-339">當另一部伺服器上線時或在應用程式網域完成其回收時，某些用戶端可以重新連線，並引發`OnReconnected`事件。</span><span class="sxs-lookup"><span data-stu-id="f371b-339">When another server comes on line or the App Domain completes its recycle, some clients may be able to reconnect and fire the `OnReconnected` event.</span></span>

<span data-ttu-id="f371b-340">如需詳細資訊，請參閱 <<c0> [ 了解和處理連線存留期事件 SignalR](index.md)。</span><span class="sxs-lookup"><span data-stu-id="f371b-340">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](index.md).</span></span>

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a><span data-ttu-id="f371b-341">不會填入呼叫狀態</span><span class="sxs-lookup"><span data-stu-id="f371b-341">Caller state not populated</span></span>

<span data-ttu-id="f371b-342">連接的存留期事件處理常式方法會呼叫從伺服器上，這表示您將放在任何狀態`state`用戶端上的物件將不會填入`Caller`伺服器上的屬性。</span><span class="sxs-lookup"><span data-stu-id="f371b-342">The connection lifetime event handler methods are called from the server, which means that any state that you put in the `state` object on the client will not be populated in the `Caller` property on the server.</span></span> <span data-ttu-id="f371b-343">如需`state`物件和`Caller`屬性，請參閱[如何在用戶端與中樞類別之間傳遞狀態](#passstate)本主題稍後的。</span><span class="sxs-lookup"><span data-stu-id="f371b-343">For information about the `state` object and the `Caller` property, see [How to pass state between clients and the Hub class](#passstate) later in this topic.</span></span>

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a><span data-ttu-id="f371b-344">如何取得用戶端的資訊，從內容屬性</span><span class="sxs-lookup"><span data-stu-id="f371b-344">How to get information about the client from the Context property</span></span>

<span data-ttu-id="f371b-345">若要取得用戶端的相關資訊，請使用`Context`中樞類別的屬性。</span><span class="sxs-lookup"><span data-stu-id="f371b-345">To get information about the client, use the `Context` property of the Hub class.</span></span> <span data-ttu-id="f371b-346">`Context`屬性會傳回[HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx)物件可讓您存取下列資訊：</span><span class="sxs-lookup"><span data-stu-id="f371b-346">The `Context` property returns a [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) object which provides access to the following information:</span></span>

- <span data-ttu-id="f371b-347">呼叫用戶端連接識別碼。</span><span class="sxs-lookup"><span data-stu-id="f371b-347">The connection ID of the calling client.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample44.cs?highlight=1)]

    <span data-ttu-id="f371b-348">連線識別碼是由 SignalR （您無法在自己的程式碼中指定的值） 指派的 GUID。</span><span class="sxs-lookup"><span data-stu-id="f371b-348">The connection ID is a GUID that is assigned by SignalR (you can't specify the value in your own code).</span></span> <span data-ttu-id="f371b-349">還有一個針對每個連線和相同的連線識別碼由所有中樞，如果您的應用程式中有多個中樞的連線識別碼。</span><span class="sxs-lookup"><span data-stu-id="f371b-349">There is one connection ID for each connection, and the same connection ID is used by all Hubs if you have multiple Hubs in your application.</span></span>
- <span data-ttu-id="f371b-350">HTTP 標頭資料。</span><span class="sxs-lookup"><span data-stu-id="f371b-350">HTTP header data.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample45.cs?highlight=1)]

    <span data-ttu-id="f371b-351">您也可以取得 HTTP 標頭從`Context.Headers`。</span><span class="sxs-lookup"><span data-stu-id="f371b-351">You can also get HTTP headers from `Context.Headers`.</span></span> <span data-ttu-id="f371b-352">多個參考相同的原因在於`Context.Headers`首先，建立`Context.Request`已更新版本中，加入屬性和`Context.Headers`保留回溯相容性。</span><span class="sxs-lookup"><span data-stu-id="f371b-352">The reason for multiple references to the same thing is that `Context.Headers` was created first, the `Context.Request` property was added later, and `Context.Headers` was retained for backward compatibility.</span></span>
- <span data-ttu-id="f371b-353">查詢字串的資料。</span><span class="sxs-lookup"><span data-stu-id="f371b-353">Query string data.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample46.cs?highlight=1)]

    <span data-ttu-id="f371b-354">您也可以取得查詢字串資料從`Context.QueryString`。</span><span class="sxs-lookup"><span data-stu-id="f371b-354">You can also get query string data from `Context.QueryString`.</span></span>

    <span data-ttu-id="f371b-355">您在這個屬性中取得查詢字串是所使用的 HTTP 要求建立 SignalR 連線。</span><span class="sxs-lookup"><span data-stu-id="f371b-355">The query string that you get in this property is the one that was used with the HTTP request that established the SignalR connection.</span></span> <span data-ttu-id="f371b-356">您可以設定連線，也就是從用戶端的用戶端相關的資料傳遞至伺服器的便利方式，在用戶端新增查詢字串參數。</span><span class="sxs-lookup"><span data-stu-id="f371b-356">You can add query string parameters in the client by configuring the connection, which is a convenient way to pass data about the client from the client to the server.</span></span> <span data-ttu-id="f371b-357">下列範例會示範一種方法在 JavaScript 用戶端新增的查詢字串，當您使用產生的 proxy。</span><span class="sxs-lookup"><span data-stu-id="f371b-357">The following example shows one way to add a query string in a JavaScript client when you use the generated proxy.</span></span>

    [!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample47.js?highlight=1)]

    <span data-ttu-id="f371b-358">如需有關設定查詢字串參數的詳細資訊，請參閱 API 指南[JavaScript](index.md)並[.NET](index.md)用戶端。</span><span class="sxs-lookup"><span data-stu-id="f371b-358">For more information about setting query string parameters, see the API guides for the [JavaScript](index.md) and [.NET](index.md) clients.</span></span>

    <span data-ttu-id="f371b-359">您可以找到查詢字串的資料，以及供內部使用 SignalR 的一些其他值中的連接所使用的傳輸方法：</span><span class="sxs-lookup"><span data-stu-id="f371b-359">You can find the transport method used for the connection in the query string data, along with some other values used internally by SignalR:</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample48.cs)]

    <span data-ttu-id="f371b-360">值`transportMethod`會 「 webSockets"、"serverSentEvents"、"foreverFrame"或"longPolling 」。</span><span class="sxs-lookup"><span data-stu-id="f371b-360">The value of `transportMethod` will be "webSockets", "serverSentEvents", "foreverFrame" or "longPolling".</span></span> <span data-ttu-id="f371b-361">請注意，如果您檢查此值`OnConnected`事件處理常式方法，在某些情況下您一開始可能會收到不是最終的交涉的傳輸連線的方法的傳輸值。</span><span class="sxs-lookup"><span data-stu-id="f371b-361">Note that if you check this value in the `OnConnected` event handler method, in some scenarios you might initially get a transport value that is not the final negotiated transport method for the connection.</span></span> <span data-ttu-id="f371b-362">在此情況下方法會擲回例外狀況，並將會呼叫稍後再建立的最後一個傳輸方法時。</span><span class="sxs-lookup"><span data-stu-id="f371b-362">In that case the method will throw an exception and will be called again later when the final transport method is established.</span></span>
- <span data-ttu-id="f371b-363">Cookie。</span><span class="sxs-lookup"><span data-stu-id="f371b-363">Cookies.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    <span data-ttu-id="f371b-364">您也可以取得 cookie `Context.RequestCookies`。</span><span class="sxs-lookup"><span data-stu-id="f371b-364">You can also get cookies from `Context.RequestCookies`.</span></span>
- <span data-ttu-id="f371b-365">使用者資訊。</span><span class="sxs-lookup"><span data-stu-id="f371b-365">User information.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample50.cs?highlight=1)]
- <span data-ttu-id="f371b-366">要求的 HttpContext 物件：</span><span class="sxs-lookup"><span data-stu-id="f371b-366">The HttpContext object for the request :</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample51.cs?highlight=1)]

    <span data-ttu-id="f371b-367">使用這個方法，而非得到`HttpContext.Current`以取得`HttpContext`SignalR 連線的物件。</span><span class="sxs-lookup"><span data-stu-id="f371b-367">Use this method instead of getting `HttpContext.Current` to get the `HttpContext` object for the SignalR connection.</span></span>

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a><span data-ttu-id="f371b-368">如何將用戶端與中樞類別之間傳遞狀態</span><span class="sxs-lookup"><span data-stu-id="f371b-368">How to pass state between clients and the Hub class</span></span>

<span data-ttu-id="f371b-369">用戶端 proxy 提供`state`您可以在您想要傳送至每個方法呼叫與伺服器的資料儲存的物件。</span><span class="sxs-lookup"><span data-stu-id="f371b-369">The client proxy provides a `state` object in which you can store data that you want to be transmitted to the server with each method call.</span></span> <span data-ttu-id="f371b-370">在伺服器上，您可以存取此資料在`Clients.Caller`中呼叫的用戶端中樞方法的屬性。</span><span class="sxs-lookup"><span data-stu-id="f371b-370">On the server you can access this data in the `Clients.Caller` property in Hub methods that are called by clients.</span></span> <span data-ttu-id="f371b-371">`Clients.Caller`屬性不會擴展連線存留期事件處理常式方法`OnConnected`， `OnDisconnected`，和`OnReconnected`。</span><span class="sxs-lookup"><span data-stu-id="f371b-371">The `Clients.Caller` property is not populated for the connection lifetime event handler methods `OnConnected`, `OnDisconnected`, and `OnReconnected`.</span></span>

<span data-ttu-id="f371b-372">建立或更新資料集中`state`物件和`Clients.Caller`屬性可在兩個方向。</span><span class="sxs-lookup"><span data-stu-id="f371b-372">Creating or updating data in the `state` object and the `Clients.Caller` property works in both directions.</span></span> <span data-ttu-id="f371b-373">您可以更新伺服器中的值，而且它們會傳回給用戶端。</span><span class="sxs-lookup"><span data-stu-id="f371b-373">You can update values in the server and they are passed back to the client.</span></span>

<span data-ttu-id="f371b-374">下列範例會儲存傳輸到每個方法呼叫的伺服器狀態的 JavaScript 用戶端程式碼。</span><span class="sxs-lookup"><span data-stu-id="f371b-374">The following example shows JavaScript client code that stores state for transmission to the server with every method call.</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample52.js?highlight=1-2)]

<span data-ttu-id="f371b-375">下列範例會示範在.NET 用戶端對等的程式碼。</span><span class="sxs-lookup"><span data-stu-id="f371b-375">The following example shows the equivalent code in a .NET client.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample53.cs?highlight=1-2)]

<span data-ttu-id="f371b-376">在中樞類別中，您可以存取此資料在`Clients.Caller`屬性。</span><span class="sxs-lookup"><span data-stu-id="f371b-376">In your Hub class, you can access this data in the `Clients.Caller` property.</span></span> <span data-ttu-id="f371b-377">下列範例程式碼來擷取上一個範例中所指的狀態。</span><span class="sxs-lookup"><span data-stu-id="f371b-377">The following example shows code that retrieves the state referred to in the previous example.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample54.cs?highlight=3-4)]

> [!NOTE]
> <span data-ttu-id="f371b-378">這項機制保存的狀態並不適用於大量的資料，因為您放入的所有項目`state`或`Clients.Caller`屬性是每個方法引動過程來回時間。</span><span class="sxs-lookup"><span data-stu-id="f371b-378">This mechanism for persisting state is not intended for large amounts of data, since everything you put in the `state` or `Clients.Caller` property is round-tripped with every method invocation.</span></span> <span data-ttu-id="f371b-379">它可用於較小的項目，例如使用者名稱或計數器。</span><span class="sxs-lookup"><span data-stu-id="f371b-379">It's useful for smaller items such as user names or counters.</span></span>


<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a><span data-ttu-id="f371b-380">如何在中樞類別中處理錯誤</span><span class="sxs-lookup"><span data-stu-id="f371b-380">How to handle errors in the Hub class</span></span>

<span data-ttu-id="f371b-381">若要處理您的中樞類別方法中發生的錯誤，請使用其中一個或多個下列方法：</span><span class="sxs-lookup"><span data-stu-id="f371b-381">To handle errors that occur in your Hub class methods, use one or both of the following methods:</span></span>

- <span data-ttu-id="f371b-382">將您的方法程式碼包裝在 try / catch 區塊，並記錄例外狀況物件。</span><span class="sxs-lookup"><span data-stu-id="f371b-382">Wrap your method code in try-catch blocks and log the exception object.</span></span> <span data-ttu-id="f371b-383">以進行偵錯您可以將例外狀況傳送至用戶端，但在生產環境中的用戶端傳送的詳細的資訊的原因不建議的安全性。</span><span class="sxs-lookup"><span data-stu-id="f371b-383">For debugging purposes you can send the exception to the client, but for security reasons sending detailed information to clients in production is not recommended.</span></span>
- <span data-ttu-id="f371b-384">建立中樞的管線模組來處理[OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx)方法。</span><span class="sxs-lookup"><span data-stu-id="f371b-384">Create a Hubs pipeline module that handles the [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) method.</span></span> <span data-ttu-id="f371b-385">下列範例示範會記錄錯誤，後面接著程式碼，將模組插入至中樞管線的 Global.asax 中的管線模組。</span><span class="sxs-lookup"><span data-stu-id="f371b-385">The following example shows a pipeline module that logs errors, followed by code in Global.asax that injects the module into the Hubs pipeline.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample55.cs)]

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample56.cs?highlight=3)]

<span data-ttu-id="f371b-386">如需中樞管線模組的詳細資訊，請參閱[爧簏濻中樞管線](#hubpipeline)本主題稍後的。</span><span class="sxs-lookup"><span data-stu-id="f371b-386">For more information about Hub pipeline modules, see [How to customize the Hubs pipeline](#hubpipeline) later in this topic.</span></span>

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a><span data-ttu-id="f371b-387">如何啟用追蹤</span><span class="sxs-lookup"><span data-stu-id="f371b-387">How to enable tracing</span></span>

<span data-ttu-id="f371b-388">若要啟用伺服器端追蹤，system.diagnostics 項目加入 Web.config 檔案中，在此範例中所示：</span><span class="sxs-lookup"><span data-stu-id="f371b-388">To enable server-side tracing, add a system.diagnostics element to your Web.config file, as shown in this example:</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-server/samples/sample57.html?highlight=17-72)]

<span data-ttu-id="f371b-389">當您在 Visual Studio 中執行應用程式時，您可以檢視中的記錄檔**輸出**視窗。</span><span class="sxs-lookup"><span data-stu-id="f371b-389">When you run the application in Visual Studio, you can view the logs in the **Output** window.</span></span>

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a><span data-ttu-id="f371b-390">如何呼叫方法的用戶端和管理中樞類別外的群組</span><span class="sxs-lookup"><span data-stu-id="f371b-390">How to call client methods and manage groups from outside the Hub class</span></span>

<span data-ttu-id="f371b-391">若要從不同的類別，比您中樞的類別呼叫用戶端方法，取得 SignalR 內容物件的參考中樞並使用的用戶端上呼叫方法，或管理群組。</span><span class="sxs-lookup"><span data-stu-id="f371b-391">To call client methods from a different class than your Hub class, get a reference to the SignalR context object for the Hub and use that to call methods on the client or manage groups.</span></span>

<span data-ttu-id="f371b-392">下例會`StockTicker`類別取得內容物件，將它儲存在類別的執行個體、 將類別執行個體儲存在靜態屬性，並使用從單一類別執行個體的內容來呼叫`updateStockPrice`之用戶端上的方法連接到集線器，名為`StockTickerHub`。</span><span class="sxs-lookup"><span data-stu-id="f371b-392">The following sample `StockTicker` class gets the context object, stores it in an instance of the class, stores the class instance in a static property, and uses the context from the singleton class instance to call the `updateStockPrice` method on clients that are connected to a Hub named `StockTickerHub`.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample58.cs?highlight=8,24)]

<span data-ttu-id="f371b-393">如果您需要使用內容多個時間長時間執行的物件中，一次取得參考，並儲存它，而非每次進行取得。</span><span class="sxs-lookup"><span data-stu-id="f371b-393">If you need to use the context multiple-times in a long-lived object, get the reference once and save it rather than getting it again each time.</span></span> <span data-ttu-id="f371b-394">一次取得內容，可確保 SignalR 會將訊息傳送至用戶端在 Hub 方法進行用戶端方法引動過程的順序相同。</span><span class="sxs-lookup"><span data-stu-id="f371b-394">Getting the context once ensures that SignalR sends messages to clients in the same sequence in which your Hub methods make client method invocations.</span></span> <span data-ttu-id="f371b-395">示範如何使用 SignalR 內容中樞教學課程中，請參閱[伺服器廣播與 ASP.NET SignalR](index.md)。</span><span class="sxs-lookup"><span data-stu-id="f371b-395">For a tutorial that shows how to use the SignalR context for a Hub, see [Server Broadcast with ASP.NET SignalR](index.md).</span></span>

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a><span data-ttu-id="f371b-396">呼叫用戶端方法</span><span class="sxs-lookup"><span data-stu-id="f371b-396">Calling client methods</span></span>

<span data-ttu-id="f371b-397">您可以指定哪些用戶端會收到 RPC，但是您有較少的選項，比從中樞類別您每次呼叫時。</span><span class="sxs-lookup"><span data-stu-id="f371b-397">You can specify which clients will receive the RPC, but you have fewer options than when you call from a Hub class.</span></span> <span data-ttu-id="f371b-398">這是內容未與特定的呼叫，從用戶端，相關聯，因此任何方法，需要了解目前的連線識別碼，例如`Clients.Others`，或`Clients.Caller`，或`Clients.OthersInGroup`，未提供。</span><span class="sxs-lookup"><span data-stu-id="f371b-398">The reason for this is that the context is not associated with a particular call from a client, so any methods that require knowledge of the current connection ID, such as `Clients.Others`, or `Clients.Caller`, or `Clients.OthersInGroup`, are not available.</span></span> <span data-ttu-id="f371b-399">有下列選項可供使用：</span><span class="sxs-lookup"><span data-stu-id="f371b-399">The following options are available:</span></span>

- <span data-ttu-id="f371b-400">所有已連線的用戶端。</span><span class="sxs-lookup"><span data-stu-id="f371b-400">All connected clients.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample59.cs)]
- <span data-ttu-id="f371b-401">連接識別碼所識別的特定用戶端</span><span class="sxs-lookup"><span data-stu-id="f371b-401">A specific client identified by connection ID.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample60.css)]
- <span data-ttu-id="f371b-402">所有已連線的用戶端，除了指定的用戶端，識別連接識別碼。</span><span class="sxs-lookup"><span data-stu-id="f371b-402">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample61.cs)]
- <span data-ttu-id="f371b-403">指定群組中所有已連線的用戶端。</span><span class="sxs-lookup"><span data-stu-id="f371b-403">All connected clients in a specified group.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample62.css)]
- <span data-ttu-id="f371b-404">所有已連線的用戶端在指定的群組，除了指定的用戶端，識別連接識別碼。</span><span class="sxs-lookup"><span data-stu-id="f371b-404">All connected clients in a specified group except specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample63.cs)]

<span data-ttu-id="f371b-405">如果您呼叫到非中樞類別方法從中樞類別中，您可以傳入目前的連線識別碼，並使用透過`Clients.Client`， `Clients.AllExcept`，或`Clients.Group`模擬`Clients.Caller`， `Clients.Others`，或`Clients.OthersInGroup`。</span><span class="sxs-lookup"><span data-stu-id="f371b-405">If you are calling into your non-Hub class from methods in your Hub class, you can pass in the current connection ID and use that with `Clients.Client`, `Clients.AllExcept`, or `Clients.Group` to simulate `Clients.Caller`, `Clients.Others`, or `Clients.OthersInGroup`.</span></span> <span data-ttu-id="f371b-406">在下列範例中，`MoveShapeHub`類別會將傳遞至的連線識別碼`Broadcaster`類別，讓`Broadcaster`類別可以模擬`Clients.Others`。</span><span class="sxs-lookup"><span data-stu-id="f371b-406">In the following example, the `MoveShapeHub` class passes the connection ID to the `Broadcaster` class so that the `Broadcaster` class can simulate `Clients.Others`.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample64.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a><span data-ttu-id="f371b-407">管理群組成員資格</span><span class="sxs-lookup"><span data-stu-id="f371b-407">Managing group membership</span></span>

<span data-ttu-id="f371b-408">群組管理您會有相同的選項，就像是在 Hub 類別。</span><span class="sxs-lookup"><span data-stu-id="f371b-408">For managing groups you have the same options as you do in a Hub class.</span></span>

- <span data-ttu-id="f371b-409">加入群組中的用戶端</span><span class="sxs-lookup"><span data-stu-id="f371b-409">Add a client to a group</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample65.cs)]
- <span data-ttu-id="f371b-410">從群組移除用戶端</span><span class="sxs-lookup"><span data-stu-id="f371b-410">Remove a client from a group</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample66.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a><span data-ttu-id="f371b-411">如何自訂中樞管線</span><span class="sxs-lookup"><span data-stu-id="f371b-411">How to customize the Hubs pipeline</span></span>

<span data-ttu-id="f371b-412">SignalR 可讓您將自己的程式碼插入中樞管線。</span><span class="sxs-lookup"><span data-stu-id="f371b-412">SignalR enables you to inject your own code into the Hub pipeline.</span></span> <span data-ttu-id="f371b-413">下列範例會記錄來自用戶端與傳出用戶端上叫用的方法呼叫每個傳入方法呼叫的自訂中樞管線模組：</span><span class="sxs-lookup"><span data-stu-id="f371b-413">The following example shows a custom Hub pipeline module that logs each incoming method call received from the client and outgoing method call invoked on the client:</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample67.cs)]

<span data-ttu-id="f371b-414">下列程式碼中*Global.asax*檔案可註冊在中樞管線中執行的模組：</span><span class="sxs-lookup"><span data-stu-id="f371b-414">The following code in the *Global.asax* file registers the module to run in the Hub pipeline:</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample68.cs?highlight=3)]

<span data-ttu-id="f371b-415">有許多不同的方法，您可以覆寫。</span><span class="sxs-lookup"><span data-stu-id="f371b-415">There are many different methods that you can override.</span></span> <span data-ttu-id="f371b-416">如需完整清單，請參閱 < [HubPipelineModule 方法](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx)。</span><span class="sxs-lookup"><span data-stu-id="f371b-416">For a complete list, see [HubPipelineModule Methods](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).</span></span>
