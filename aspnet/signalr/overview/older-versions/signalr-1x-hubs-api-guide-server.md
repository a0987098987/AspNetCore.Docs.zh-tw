---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-server
title: ASP.NET SignalR 中樞 API 指南-伺服器 (SignalR 1.x) |Microsoft 文件
author: pfletcher
description: 本文件提供 SignalR 使用的程式碼範例 demonstratin 1.1 版的 ASP.NET SignalR 中樞 API 的伺服器端程式設計的簡介...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/17/2013
ms.topic: article
ms.assetid: 03e4b9f5-0fea-4d94-959f-014b2762a301
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: 96155b1c648e5f6092b3ba67a560197f86a593b9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
ms.locfileid: "28044178"
---
<a name="aspnet-signalr-hubs-api-guide---server-signalr-1x"></a><span data-ttu-id="99d2d-103">ASP.NET SignalR 中樞 API 指南-伺服器 (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="99d2d-103">ASP.NET SignalR Hubs API Guide - Server (SignalR 1.x)</span></span>
====================
<span data-ttu-id="99d2d-104">由[Patrick Fletcher](https://github.com/pfletcher)， [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="99d2d-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="99d2d-105">本文件提供簡介 SignalR 1.1 版的 ASP.NET SignalR 中樞 API 的伺服器端程式設計的程式碼範例示範常用的選項。</span><span class="sxs-lookup"><span data-stu-id="99d2d-105">This document provides an introduction to programming the server side of the ASP.NET SignalR Hubs API for SignalR version 1.1, with code samples demonstrating common options.</span></span>
> 
> <span data-ttu-id="99d2d-106">SignalR 中樞應用程式開發介面可讓您從伺服器連線的用戶端和伺服器的用戶端進行遠端程序呼叫 (Rpc)。</span><span class="sxs-lookup"><span data-stu-id="99d2d-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="99d2d-107">伺服端程式碼，並定義用戶端，可以呼叫的方法，呼叫用戶端執行的方法。</span><span class="sxs-lookup"><span data-stu-id="99d2d-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="99d2d-108">用戶端程式碼，並定義可在伺服器上，從呼叫的方法，您呼叫在伺服器執行的方法。</span><span class="sxs-lookup"><span data-stu-id="99d2d-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="99d2d-109">SignalR 會負責為您的用戶端-伺服器一切細節。</span><span class="sxs-lookup"><span data-stu-id="99d2d-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="99d2d-110">SignalR 也能提供較低層級應用程式開發介面呼叫持續連線。</span><span class="sxs-lookup"><span data-stu-id="99d2d-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="99d2d-111">如需簡介 SignalR、 集線器及持續連線，或示範如何建置完整的 SignalR 應用程式的教學課程，請參閱[SignalR-快速入門](index.md)。</span><span class="sxs-lookup"><span data-stu-id="99d2d-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](index.md).</span></span>


## <a name="overview"></a><span data-ttu-id="99d2d-112">總覽</span><span class="sxs-lookup"><span data-stu-id="99d2d-112">Overview</span></span>

<span data-ttu-id="99d2d-113">本文件包含下列章節：</span><span class="sxs-lookup"><span data-stu-id="99d2d-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="99d2d-114">如何登錄至 SignalR 路由並設定 SignalR 選項</span><span class="sxs-lookup"><span data-stu-id="99d2d-114">How to register the SignalR route and configure SignalR options</span></span>](#route)

    - [<span data-ttu-id="99d2d-115">/Signalr URL</span><span class="sxs-lookup"><span data-stu-id="99d2d-115">The /signalr URL</span></span>](#signalrurl)
    - [<span data-ttu-id="99d2d-116">設定 SignalR 選項</span><span class="sxs-lookup"><span data-stu-id="99d2d-116">Configuring SignalR options</span></span>](#options)
- [<span data-ttu-id="99d2d-117">如何建立及使用 Hub 類別</span><span class="sxs-lookup"><span data-stu-id="99d2d-117">How to create and use Hub classes</span></span>](#hubclass)

    - [<span data-ttu-id="99d2d-118">中樞物件存留期</span><span class="sxs-lookup"><span data-stu-id="99d2d-118">Hub object lifetime</span></span>](#transience)
    - [<span data-ttu-id="99d2d-119">依照 camel 命名法的大小寫的 JavaScript 用戶端中樞名稱</span><span class="sxs-lookup"><span data-stu-id="99d2d-119">Camel-casing of Hub names in JavaScript clients</span></span>](#hubnames)
    - [<span data-ttu-id="99d2d-120">多個集線器</span><span class="sxs-lookup"><span data-stu-id="99d2d-120">Multiple Hubs</span></span>](#multiplehubs)
- [<span data-ttu-id="99d2d-121">如何在用戶端可以呼叫的中樞類別中定義的方法</span><span class="sxs-lookup"><span data-stu-id="99d2d-121">How to define methods in the Hub class that clients can call</span></span>](#hubmethods)

    - [<span data-ttu-id="99d2d-122">依照 camel 命名法的大小寫的 JavaScript 用戶端中的方法名稱</span><span class="sxs-lookup"><span data-stu-id="99d2d-122">Camel-casing of method names in JavaScript clients</span></span>](#methodnames)
    - [<span data-ttu-id="99d2d-123">以非同步方式執行的時機</span><span class="sxs-lookup"><span data-stu-id="99d2d-123">When to execute asynchronously</span></span>](#asyncmethods)
    - [<span data-ttu-id="99d2d-124">定義多載</span><span class="sxs-lookup"><span data-stu-id="99d2d-124">Defining overloads</span></span>](#overloads)
- [<span data-ttu-id="99d2d-125">如何從 Hub 類別的方法呼叫用戶端</span><span class="sxs-lookup"><span data-stu-id="99d2d-125">How to call client methods from the Hub class</span></span>](#callfromhub)

    - [<span data-ttu-id="99d2d-126">選取哪些用戶端會收到 RPC</span><span class="sxs-lookup"><span data-stu-id="99d2d-126">Selecting which clients will receive the RPC</span></span>](#selectingclients)
    - [<span data-ttu-id="99d2d-127">方法名稱無法編譯時間驗證</span><span class="sxs-lookup"><span data-stu-id="99d2d-127">No compile-time validation for method names</span></span>](#dynamicmethodnames)
    - [<span data-ttu-id="99d2d-128">不區分大小寫的方法名稱比對</span><span class="sxs-lookup"><span data-stu-id="99d2d-128">Case-insensitive method name matching</span></span>](#caseinsensitive)
    - [<span data-ttu-id="99d2d-129">非同步執行</span><span class="sxs-lookup"><span data-stu-id="99d2d-129">Asynchronous execution</span></span>](#asyncclient)
- [<span data-ttu-id="99d2d-130">如何從中樞類別管理群組成員資格</span><span class="sxs-lookup"><span data-stu-id="99d2d-130">How to manage group membership from the Hub class</span></span>](#groupsfromhub)

    - [<span data-ttu-id="99d2d-131">加入和移除方法的非同步執行</span><span class="sxs-lookup"><span data-stu-id="99d2d-131">Asynchronous execution of Add and Remove methods</span></span>](#asyncgroupmethods)
    - [<span data-ttu-id="99d2d-132">群組成員資格持續性</span><span class="sxs-lookup"><span data-stu-id="99d2d-132">Group membership persistence</span></span>](#grouppersistence)
    - [<span data-ttu-id="99d2d-133">單一使用者群組</span><span class="sxs-lookup"><span data-stu-id="99d2d-133">Single-user groups</span></span>](#singleusergroups)
- [<span data-ttu-id="99d2d-134">如何處理連接的存留期事件中樞類別中</span><span class="sxs-lookup"><span data-stu-id="99d2d-134">How to handle connection lifetime events in the Hub class</span></span>](#connectionlifetime)

    - [<span data-ttu-id="99d2d-135">呼叫 OnConnected、 OnDisconnected 和 OnReconnected 的時機</span><span class="sxs-lookup"><span data-stu-id="99d2d-135">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>](#onreconnected)
    - [<span data-ttu-id="99d2d-136">不會填入呼叫狀態</span><span class="sxs-lookup"><span data-stu-id="99d2d-136">Caller state not populated</span></span>](#nocallerstate)
- [<span data-ttu-id="99d2d-137">如何取得用戶端的相關資訊，從內容屬性</span><span class="sxs-lookup"><span data-stu-id="99d2d-137">How to get information about the client from the Context property</span></span>](#contextproperty)
- [<span data-ttu-id="99d2d-138">如何將用戶端與中樞類別之間傳遞狀態</span><span class="sxs-lookup"><span data-stu-id="99d2d-138">How to pass state between clients and the Hub class</span></span>](#passstate)
- [<span data-ttu-id="99d2d-139">如何在集線器類別中處理錯誤</span><span class="sxs-lookup"><span data-stu-id="99d2d-139">How to handle errors in the Hub class</span></span>](#handleErrors)
- [<span data-ttu-id="99d2d-140">呼叫方法的用戶端和管理從中樞類別以外的群組</span><span class="sxs-lookup"><span data-stu-id="99d2d-140">How to call client methods and manage groups from outside the Hub class</span></span>](#callfromoutsidehub)

    - [<span data-ttu-id="99d2d-141">呼叫用戶端方法</span><span class="sxs-lookup"><span data-stu-id="99d2d-141">Calling client methods</span></span>](#callingclientsoutsidehub)
    - [<span data-ttu-id="99d2d-142">管理群組成員資格</span><span class="sxs-lookup"><span data-stu-id="99d2d-142">Managing group membership</span></span>](#managinggroupsoutsidehub)
- [<span data-ttu-id="99d2d-143">如何啟用追蹤</span><span class="sxs-lookup"><span data-stu-id="99d2d-143">How to enable tracing</span></span>](#tracing)
- [<span data-ttu-id="99d2d-144">如何以自訂中樞管線</span><span class="sxs-lookup"><span data-stu-id="99d2d-144">How to customize the Hubs pipeline</span></span>](#hubpipeline)

<span data-ttu-id="99d2d-145">如需如何程式用戶端文件，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="99d2d-145">For documentation on how to program clients, see the following resources:</span></span>

- [<span data-ttu-id="99d2d-146">SignalR 中樞 API 指南 JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="99d2d-146">SignalR Hubs API Guide - JavaScript Client</span></span>](index.md)
- [<span data-ttu-id="99d2d-147">SignalR 中樞 API 指南-.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="99d2d-147">SignalR Hubs API Guide - .NET Client</span></span>](index.md)

<span data-ttu-id="99d2d-148">應用程式開發介面參考主題的連結是.NET 4.5 版的 API。</span><span class="sxs-lookup"><span data-stu-id="99d2d-148">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="99d2d-149">如果您使用.NET 4，請參閱[API 主題.NET 4 版本](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx)。</span><span class="sxs-lookup"><span data-stu-id="99d2d-149">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="route"></a>

## <a name="how-to-register-the-signalr-route-and-configure-signalr-options"></a><span data-ttu-id="99d2d-150">如何登錄至 SignalR 路由並設定 SignalR 選項</span><span class="sxs-lookup"><span data-stu-id="99d2d-150">How to register the SignalR route and configure SignalR options</span></span>

<span data-ttu-id="99d2d-151">若要定義用戶端將用來連接到您的中樞的路由，請呼叫[MapHubs](https://msdn.microsoft.com/library/system.web.routing.signalrrouteextensions.maphubs(v=vs.111).aspx)應用程式啟動時的方法。</span><span class="sxs-lookup"><span data-stu-id="99d2d-151">To define the route that clients will use to connect to your Hub, call the [MapHubs](https://msdn.microsoft.com/library/system.web.routing.signalrrouteextensions.maphubs(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="99d2d-152">`MapHubs`是[擴充方法](https://msdn.microsoft.com/library/vstudio/bb383977.aspx)如`System.Web.Routing.RouteCollection`類別。</span><span class="sxs-lookup"><span data-stu-id="99d2d-152">`MapHubs` is an [extension method](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) for the `System.Web.Routing.RouteCollection` class.</span></span> <span data-ttu-id="99d2d-153">下列範例示範如何定義中的 SignalR 中樞路由*Global.asax*檔案。</span><span class="sxs-lookup"><span data-stu-id="99d2d-153">The following example shows how to define the SignalR Hubs route in the *Global.asax* file.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample1.cs)]

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample2.cs?highlight=5)]

<span data-ttu-id="99d2d-154">如果您要將 SignalR 功能加入 ASP.NET MVC 應用程式，請確定 SignalR 路由會加入其他路由之前。</span><span class="sxs-lookup"><span data-stu-id="99d2d-154">If you are adding SignalR functionality to an ASP.NET MVC application, make sure that the SignalR route is added before the other routes.</span></span> <span data-ttu-id="99d2d-155">如需詳細資訊，請參閱[教學課程： 開始使用 SignalR 和 MVC 4](index.md)。</span><span class="sxs-lookup"><span data-stu-id="99d2d-155">For more information, see [Tutorial: Getting Started with SignalR and MVC 4](index.md).</span></span>

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a><span data-ttu-id="99d2d-156">/Signalr URL</span><span class="sxs-lookup"><span data-stu-id="99d2d-156">The /signalr URL</span></span>

<span data-ttu-id="99d2d-157">根據預設，用戶端將用來連接到您的中樞的路由 URL 是:"/ signalr"。</span><span class="sxs-lookup"><span data-stu-id="99d2d-157">By default, the route URL which clients will use to connect to your Hub is "/signalr".</span></span> <span data-ttu-id="99d2d-158">（請勿混淆這個 URL，使用"signalr/中樞 」 的 URL，這是自動產生的 JavaScript 檔案。</span><span class="sxs-lookup"><span data-stu-id="99d2d-158">(Don't confuse this URL with the "/signalr/hubs" URL, which is for the automatically generated JavaScript file.</span></span> <span data-ttu-id="99d2d-159">如需產生之 proxy 的詳細資訊，請參閱[SignalR 中樞 API 指南-JavaScript 用戶端-產生的 proxy，並會為您](index.md)。)</span><span class="sxs-lookup"><span data-stu-id="99d2d-159">For more information about the generated proxy, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](index.md).)</span></span>

<span data-ttu-id="99d2d-160">可能有異常的情況下，讓此基底 URL 無法使用適用於 SignalR;例如，名為專案中有一個資料夾*signalr*而且您不想要變更名稱。</span><span class="sxs-lookup"><span data-stu-id="99d2d-160">There might be extraordinary circumstances that make this base URL not usable for SignalR; for example, you have a folder in your project named *signalr* and you don't want to change the name.</span></span> <span data-ttu-id="99d2d-161">在此情況下，您可以變更基底 URL，如下列範例所示 (取代 「 / signalr 」 範例程式碼以您所需的 URL)。</span><span class="sxs-lookup"><span data-stu-id="99d2d-161">In that case, you can change the base URL, as shown in the following examples (replace "/signalr" in the sample code with your desired URL).</span></span>

<span data-ttu-id="99d2d-162">**指定的 URL 的伺服器程式碼**</span><span class="sxs-lookup"><span data-stu-id="99d2d-162">**Server code that specifies the URL**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample3.cs?highlight=1)]

<span data-ttu-id="99d2d-163">**指定的 URL （與產生的 proxy) 的 JavaScript 用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="99d2d-163">**JavaScript client code that specifies the URL (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample4.js?highlight=1)]

<span data-ttu-id="99d2d-164">**指定的 URL （不含產生的 proxy) 的 JavaScript 用戶端程式碼**</span><span class="sxs-lookup"><span data-stu-id="99d2d-164">**JavaScript client code that specifies the URL (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample5.js?highlight=1)]

<span data-ttu-id="99d2d-165">**.NET 用戶端程式碼來指定的 URL**</span><span class="sxs-lookup"><span data-stu-id="99d2d-165">**.NET client code that specifies the URL**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample6.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a><span data-ttu-id="99d2d-166">設定 SignalR 選項</span><span class="sxs-lookup"><span data-stu-id="99d2d-166">Configuring SignalR Options</span></span>

<span data-ttu-id="99d2d-167">多載`MapHubs`方法可讓您指定自訂 URL、 自訂相依性解析程式，以及下列選項：</span><span class="sxs-lookup"><span data-stu-id="99d2d-167">Overloads of the `MapHubs` method enable you to specify a custom URL, a custom dependency resolver, and the following options:</span></span>

- <span data-ttu-id="99d2d-168">啟用從瀏覽器用戶端的跨網域呼叫。</span><span class="sxs-lookup"><span data-stu-id="99d2d-168">Enable cross-domain calls from browser clients.</span></span>

    <span data-ttu-id="99d2d-169">通常如果瀏覽器中載入的頁面上，從`http://contoso.com`，SignalR 連線是在相同網域中，在`http://contoso.com/signalr`。</span><span class="sxs-lookup"><span data-stu-id="99d2d-169">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="99d2d-170">如果將頁面從`http://contoso.com`會連接到`http://fabrikam.com/signalr`，也就是跨網域的連接。</span><span class="sxs-lookup"><span data-stu-id="99d2d-170">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="99d2d-171">基於安全性理由，預設會停用跨網域連線。</span><span class="sxs-lookup"><span data-stu-id="99d2d-171">For security reasons, cross-domain connections are disabled by default.</span></span> <span data-ttu-id="99d2d-172">如需詳細資訊，請參閱[ASP.NET SignalR 中樞 API 指南-JavaScript 用戶端-如何建立跨定義域連線](index.md)。</span><span class="sxs-lookup"><span data-stu-id="99d2d-172">For more information, see [ASP.NET SignalR Hubs API Guide - JavaScript Client - How to establish a cross-domain connection](index.md).</span></span>
- <span data-ttu-id="99d2d-173">啟用詳細的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="99d2d-173">Enable detailed error messages.</span></span>

    <span data-ttu-id="99d2d-174">發生錯誤時，SignalR 的預設行為是傳送給用戶端通知訊息，但發生了什麼事的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="99d2d-174">When errors occur, the default behavior of SignalR is to send to clients a notification message without details about what happened.</span></span> <span data-ttu-id="99d2d-175">詳細的錯誤資訊傳送給用戶端不建議在實際執行環境，因為惡意使用者可能可以使用攻擊您的應用程式中的資訊。</span><span class="sxs-lookup"><span data-stu-id="99d2d-175">Sending detailed error information to clients is not recommended in production, because malicious users might be able to use the information in attacks against your application.</span></span> <span data-ttu-id="99d2d-176">如需疑難排解，您可以使用此選項以暫時啟用更具資訊性的錯誤報告。</span><span class="sxs-lookup"><span data-stu-id="99d2d-176">For troubleshooting, you can use this option to temporarily enable more informative error reporting.</span></span>
- <span data-ttu-id="99d2d-177">停用自動產生的 JavaScript proxy 檔案。</span><span class="sxs-lookup"><span data-stu-id="99d2d-177">Disable automatically generated JavaScript proxy files.</span></span>

    <span data-ttu-id="99d2d-178">根據預設，使用 proxy 的 JavaScript 檔案，為您的中樞類別會產生以回應 URL 」 signalr/中樞 」。</span><span class="sxs-lookup"><span data-stu-id="99d2d-178">By default, a JavaScript file with proxies for your Hub classes is generated in response to the URL "/signalr/hubs".</span></span> <span data-ttu-id="99d2d-179">如果您不想要使用的 JavaScript proxy，或如果您想要以手動方式產生這個檔案，而且參考的實體檔案中您的用戶端，您可以使用此選項以停用 proxy 產生。</span><span class="sxs-lookup"><span data-stu-id="99d2d-179">If you don't want to use the JavaScript proxies, or if you want to generate this file manually and refer to a physical file in your clients, you can use this option to disable proxy generation.</span></span> <span data-ttu-id="99d2d-180">如需詳細資訊，請參閱[SignalR 中樞 API 指南-JavaScript 用戶端-如何建立 SignalR 的實體檔案產生 proxy](index.md)。</span><span class="sxs-lookup"><span data-stu-id="99d2d-180">For more information, see [SignalR Hubs API Guide - JavaScript Client - How to create a physical file for the SignalR generated proxy](index.md).</span></span>

<span data-ttu-id="99d2d-181">下列範例示範如何呼叫中指定的 SignalR 連線 URL 和這些選項`MapHubs`方法。</span><span class="sxs-lookup"><span data-stu-id="99d2d-181">The following example shows how to specify the SignalR connection URL and these options in a call to the `MapHubs` method.</span></span> <span data-ttu-id="99d2d-182">若要指定自訂 URL，取代 「 / signalr 」 在您想要使用的 url 範例。</span><span class="sxs-lookup"><span data-stu-id="99d2d-182">To specify a custom URL, replace "/signalr" in the example with the URL that you want to use.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample7.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a><span data-ttu-id="99d2d-183">如何建立及使用 Hub 類別</span><span class="sxs-lookup"><span data-stu-id="99d2d-183">How to create and use Hub classes</span></span>

<span data-ttu-id="99d2d-184">若要建立的中樞時，建立衍生自類別[Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx)。</span><span class="sxs-lookup"><span data-stu-id="99d2d-184">To create a Hub, create a class that derives from [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span></span> <span data-ttu-id="99d2d-185">下列範例會示範一個簡單的中樞類別交談應用程式。</span><span class="sxs-lookup"><span data-stu-id="99d2d-185">The following example shows a simple Hub class for a chat application.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample8.cs)]

<span data-ttu-id="99d2d-186">在此範例中，連線的用戶端可以呼叫`NewContosoChatMessage`方法，並收到的資料時，會傳播到所有連線的用戶端。</span><span class="sxs-lookup"><span data-stu-id="99d2d-186">In this example, a connected client can call the `NewContosoChatMessage` method, and when it does, the data received is broadcasted to all connected clients.</span></span>

<a id="transience"></a>

### <a name="hub-object-lifetime"></a><span data-ttu-id="99d2d-187">中樞物件存留期</span><span class="sxs-lookup"><span data-stu-id="99d2d-187">Hub object lifetime</span></span>

<span data-ttu-id="99d2d-188">您不具現化中樞類別，或從伺服器上您的程式碼呼叫其方法所有的方式是為您 SignalR 中樞管線。</span><span class="sxs-lookup"><span data-stu-id="99d2d-188">You don't instantiate the Hub class or call its methods from your own code on the server; all that is done for you by the SignalR Hubs pipeline.</span></span> <span data-ttu-id="99d2d-189">SignalR 建立 Hub 類別的新執行的個體每次需要處理的中樞 」 作業，例如當用戶端連接、 中斷連接，或對伺服器提出呼叫的方法。</span><span class="sxs-lookup"><span data-stu-id="99d2d-189">SignalR creates a new instance of your Hub class each time it needs to handle a Hub operation such as when a client connects, disconnects, or makes a method call to the server.</span></span>

<span data-ttu-id="99d2d-190">因為 Hub 類別的執行個體是暫時性的所以您無法使用它們來維護狀態從一種方法呼叫下一步。</span><span class="sxs-lookup"><span data-stu-id="99d2d-190">Because instances of the Hub class are transient, you can't use them to maintain state from one method call to the next.</span></span> <span data-ttu-id="99d2d-191">每次在伺服器接收方法呼叫從用戶端中樞的類別處理序的新執行個體的訊息。</span><span class="sxs-lookup"><span data-stu-id="99d2d-191">Each time the server receives a method call from a client, a new instance of your Hub class processes the message.</span></span> <span data-ttu-id="99d2d-192">為了維護透過多個連線和方法呼叫的狀態，使用透過其他方法，例如資料庫或靜態變數 Hub 類別，或不同類別且不是衍生自`Hub`。</span><span class="sxs-lookup"><span data-stu-id="99d2d-192">To maintain state through multiple connections and method calls, use some other method such as a database, or a static variable on the Hub class, or a different class that does not derive from `Hub`.</span></span> <span data-ttu-id="99d2d-193">如果您將保存在記憶體中的資料，中樞類別上使用的方法，例如靜態變數的資料將會遺失應用程式定義域回收時。</span><span class="sxs-lookup"><span data-stu-id="99d2d-193">If you persist data in memory, using a method such as a static variable on the Hub class, the data will be lost when the app domain recycles.</span></span>

<span data-ttu-id="99d2d-194">如果您想要從自己的中樞類別外執行的程式碼將訊息傳送給用戶端，您無法這樣藉由執行個體化中樞類別的執行個體，但這麼做為中樞類別取得 SignalR 內容物件的參考。</span><span class="sxs-lookup"><span data-stu-id="99d2d-194">If you want to send messages to clients from your own code that runs outside the Hub class, you can't do it by instantiating a Hub class instance, but you can do it by getting a reference to the SignalR context object for your Hub class.</span></span> <span data-ttu-id="99d2d-195">如需詳細資訊，請參閱[如何呼叫方法的用戶端和管理群組從中樞類別之外](#callfromoutsidehub)本主題稍後。</span><span class="sxs-lookup"><span data-stu-id="99d2d-195">For more information, see [How to call client methods and manage groups from outside the Hub class](#callfromoutsidehub) later in this topic.</span></span>

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a><span data-ttu-id="99d2d-196">依照 camel 命名法的大小寫的 JavaScript 用戶端中樞名稱</span><span class="sxs-lookup"><span data-stu-id="99d2d-196">Camel-casing of Hub names in JavaScript clients</span></span>

<span data-ttu-id="99d2d-197">根據預設，JavaScript 用戶端是指中心所使用的類別名稱的 camel 案例版本。</span><span class="sxs-lookup"><span data-stu-id="99d2d-197">By default, JavaScript clients refer to Hubs by using a camel-cased version of the class name.</span></span> <span data-ttu-id="99d2d-198">SignalR 會自動將這項變更，讓 JavaScript 程式碼可以符合 JavaScript 慣例。</span><span class="sxs-lookup"><span data-stu-id="99d2d-198">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span> <span data-ttu-id="99d2d-199">前一個範例會稱為`contosoChatHub`JavaScript 程式碼中。</span><span class="sxs-lookup"><span data-stu-id="99d2d-199">The previous example would be referred to as `contosoChatHub` in JavaScript code.</span></span>

<span data-ttu-id="99d2d-200">**Server**</span><span class="sxs-lookup"><span data-stu-id="99d2d-200">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample9.cs?highlight=1)]

<span data-ttu-id="99d2d-201">**JavaScript 用戶端會使用產生的 proxy**</span><span class="sxs-lookup"><span data-stu-id="99d2d-201">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample10.js?highlight=1)]

<span data-ttu-id="99d2d-202">如果您想要指定不同的名稱，讓用戶端使用，所以將`HubName`屬性。</span><span class="sxs-lookup"><span data-stu-id="99d2d-202">If you want to specify a different name for clients to use, add the `HubName` attribute.</span></span> <span data-ttu-id="99d2d-203">當您使用`HubName`屬性，請為 camel 命名法的大小寫，JavaScript 用戶端上沒有名稱變更。</span><span class="sxs-lookup"><span data-stu-id="99d2d-203">When you use a `HubName` attribute, there is no name change to camel case on JavaScript clients.</span></span>

<span data-ttu-id="99d2d-204">**Server**</span><span class="sxs-lookup"><span data-stu-id="99d2d-204">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample11.cs?highlight=1)]

<span data-ttu-id="99d2d-205">**JavaScript 用戶端會使用產生的 proxy**</span><span class="sxs-lookup"><span data-stu-id="99d2d-205">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample12.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a><span data-ttu-id="99d2d-206">多個集線器</span><span class="sxs-lookup"><span data-stu-id="99d2d-206">Multiple Hubs</span></span>

<span data-ttu-id="99d2d-207">您可以定義多個 Hub 類別的應用程式中。</span><span class="sxs-lookup"><span data-stu-id="99d2d-207">You can define multiple Hub classes in an application.</span></span> <span data-ttu-id="99d2d-208">當您這樣做，時請的連接共用，但群組都不同：</span><span class="sxs-lookup"><span data-stu-id="99d2d-208">When you do that, the connection is shared but groups are separate:</span></span>

- <span data-ttu-id="99d2d-209">所有用戶端會使用相同的 URL，建立您的服務與 SignalR 連線 ("/ signalr"或自訂的 URL 是否指定)，而且連接會用於所有中樞的服務來定義。</span><span class="sxs-lookup"><span data-stu-id="99d2d-209">All clients will use the same URL to establish a SignalR connection with your service ("/signalr" or your custom URL if you specified one), and that connection is used for all Hubs defined by the service.</span></span>

    <span data-ttu-id="99d2d-210">沒有任何效能差異相較於單一類別中定義中樞的所有功能的多個中樞。</span><span class="sxs-lookup"><span data-stu-id="99d2d-210">There is no performance difference for multiple Hubs compared to defining all Hub functionality in a single class.</span></span>
- <span data-ttu-id="99d2d-211">所有中樞都取得相同的 HTTP 要求資訊。</span><span class="sxs-lookup"><span data-stu-id="99d2d-211">All Hubs get the same HTTP request information.</span></span>

    <span data-ttu-id="99d2d-212">因為所有中樞會都共用相同的連接，唯一伺服器取得的 HTTP 要求資訊是就會建立 SignalR 連線的原始 HTTP 要求中。</span><span class="sxs-lookup"><span data-stu-id="99d2d-212">Since all Hubs share the same connection, the only HTTP request information that the server gets is what comes in the original HTTP request that establishes the SignalR connection.</span></span> <span data-ttu-id="99d2d-213">如果您使用連線要求從用戶端傳遞資訊給伺服器藉由指定的查詢字串時，您無法提供不同的查詢字串至不同的中樞。</span><span class="sxs-lookup"><span data-stu-id="99d2d-213">If you use the connection request to pass information from the client to the server by specifying a query string, you can't provide different query strings to different Hubs.</span></span> <span data-ttu-id="99d2d-214">所有中樞將會都收到相同的資訊。</span><span class="sxs-lookup"><span data-stu-id="99d2d-214">All Hubs will receive the same information.</span></span>
- <span data-ttu-id="99d2d-215">產生的 JavaScript proxy 檔案會包含在一個檔案中的所有主機的 proxy。</span><span class="sxs-lookup"><span data-stu-id="99d2d-215">The generated JavaScript proxies file will contain proxies for all Hubs in one file.</span></span>

    <span data-ttu-id="99d2d-216">如需 JavaScript proxy 資訊，請參閱[SignalR 中樞 API 指南-JavaScript 用戶端-產生的 proxy，並會為您](index.md)。</span><span class="sxs-lookup"><span data-stu-id="99d2d-216">For information about JavaScript proxies, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](index.md).</span></span>
- <span data-ttu-id="99d2d-217">中樞內，會定義群組。</span><span class="sxs-lookup"><span data-stu-id="99d2d-217">Groups are defined within Hubs.</span></span>

    <span data-ttu-id="99d2d-218">您可以定義的 SignalR 中名為廣播已連線的用戶端子集的群組。</span><span class="sxs-lookup"><span data-stu-id="99d2d-218">In SignalR you can define named groups to broadcast to subsets of connected clients.</span></span> <span data-ttu-id="99d2d-219">群組會分別維護每個中樞。</span><span class="sxs-lookup"><span data-stu-id="99d2d-219">Groups are maintained separately for each Hub.</span></span> <span data-ttu-id="99d2d-220">例如，名為 「 系統管理員 」 群組可包含的用戶端的一組您`ContosoChatHub`類別和相同的群組名稱會參考一組不同的用戶端程式`StockTickerHub`類別。</span><span class="sxs-lookup"><span data-stu-id="99d2d-220">For example, a group named "Administrators" would include one set of clients for your `ContosoChatHub` class, and the same group name would refer to a different set of clients for your `StockTickerHub` class.</span></span>

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a><span data-ttu-id="99d2d-221">如何在用戶端可以呼叫的中樞類別中定義的方法</span><span class="sxs-lookup"><span data-stu-id="99d2d-221">How to define methods in the Hub class that clients can call</span></span>

<span data-ttu-id="99d2d-222">若要公開您想要從用戶端可呼叫的中樞上的方法，宣告的公用方法，如下列範例所示。</span><span class="sxs-lookup"><span data-stu-id="99d2d-222">To expose a method on the Hub that you want to be callable from the client, declare a public method, as shown in the following examples.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample14.cs?highlight=3)]

<span data-ttu-id="99d2d-223">您可以指定傳回型別和參數，包括複雜型別及陣列，您可以按照任何 C# 方法。</span><span class="sxs-lookup"><span data-stu-id="99d2d-223">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="99d2d-224">您收到中參數或傳回給呼叫者的任何資料是用戶端與伺服器之間使用 JSON，即予 SignalR 複雜物件的繫結和物件的陣列會自動處理。</span><span class="sxs-lookup"><span data-stu-id="99d2d-224">Any data that you receive in parameters or return to the caller is communicated between the client and the server by using JSON, and SignalR handles the binding of complex objects and arrays of objects automatically.</span></span>

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a><span data-ttu-id="99d2d-225">依照 camel 命名法的大小寫的 JavaScript 用戶端中的方法名稱</span><span class="sxs-lookup"><span data-stu-id="99d2d-225">Camel-casing of method names in JavaScript clients</span></span>

<span data-ttu-id="99d2d-226">根據預設，JavaScript 用戶端中樞方法使用參照的方法名稱的 camel 案例版本。</span><span class="sxs-lookup"><span data-stu-id="99d2d-226">By default, JavaScript clients refer to Hub methods by using a camel-cased version of the method name.</span></span> <span data-ttu-id="99d2d-227">SignalR 會自動將這項變更，讓 JavaScript 程式碼可以符合 JavaScript 慣例。</span><span class="sxs-lookup"><span data-stu-id="99d2d-227">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="99d2d-228">**Server**</span><span class="sxs-lookup"><span data-stu-id="99d2d-228">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample15.cs?highlight=1)]

<span data-ttu-id="99d2d-229">**JavaScript 用戶端會使用產生的 proxy**</span><span class="sxs-lookup"><span data-stu-id="99d2d-229">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample16.js?highlight=1)]

<span data-ttu-id="99d2d-230">如果您想要指定不同的名稱，讓用戶端使用，所以將`HubMethodName`屬性。</span><span class="sxs-lookup"><span data-stu-id="99d2d-230">If you want to specify a different name for clients to use, add the `HubMethodName` attribute.</span></span>

<span data-ttu-id="99d2d-231">**Server**</span><span class="sxs-lookup"><span data-stu-id="99d2d-231">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample17.cs?highlight=1)]

<span data-ttu-id="99d2d-232">**JavaScript 用戶端會使用產生的 proxy**</span><span class="sxs-lookup"><span data-stu-id="99d2d-232">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a><span data-ttu-id="99d2d-233">以非同步方式執行的時機</span><span class="sxs-lookup"><span data-stu-id="99d2d-233">When to execute asynchronously</span></span>

<span data-ttu-id="99d2d-234">如果方法將會是長時間執行，或者要執行的作業，會牽涉到插撥功能，例如資料庫尋查 」 或 「 web 服務呼叫，讓中樞方法以傳回非同步[工作](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)(取代`void`傳回) 或[工作&lt;T&gt; ](https://msdn.microsoft.com/library/dd321424.aspx)物件 (取代`T`傳回型別)。</span><span class="sxs-lookup"><span data-stu-id="99d2d-234">If the method will be long-running or has to do work that would involve waiting, such as a database lookup or a web service call, make the Hub method asynchronous by returning a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (in place of `void` return) or [Task&lt;T&gt;](https://msdn.microsoft.com/library/dd321424.aspx) object (in place of `T` return type).</span></span> <span data-ttu-id="99d2d-235">當您傳回`Task`從 SignalR 方法的物件會等待`Task`若要完成，然後將進行傳送未包裝的結果傳回用戶端，因此沒有任何差異，在程式碼方法呼叫中用戶端如何。</span><span class="sxs-lookup"><span data-stu-id="99d2d-235">When you return a `Task` object from the method, SignalR waits for the `Task` to complete, and then it sends the unwrapped result back to the client, so there is no difference in how you code the method call in the client.</span></span>

<span data-ttu-id="99d2d-236">讓中樞方法非同步可避免使用 WebSocket 傳輸時，封鎖連接。</span><span class="sxs-lookup"><span data-stu-id="99d2d-236">Making a Hub method asynchronous avoids blocking the connection when it uses the WebSocket transport.</span></span> <span data-ttu-id="99d2d-237">當中樞方法以同步方式執行，而且傳輸 WebSocket 時，後續來自相同用戶端中樞方法叫用會遭到封鎖，直到中樞方法完成為止。</span><span class="sxs-lookup"><span data-stu-id="99d2d-237">When a Hub method executes synchronously and the transport is WebSocket, subsequent invocations of methods on the Hub from the same client are blocked until the Hub method completes.</span></span>

<span data-ttu-id="99d2d-238">下列範例示範相同的方法以同步方式執行自動程式碼，或以非同步的方式，後面接著函式呼叫其中一個版本的運作方式的 JavaScript 用戶端程式碼。</span><span class="sxs-lookup"><span data-stu-id="99d2d-238">The following example shows the same method coded to run synchronously or asynchronously, followed by JavaScript client code that works for calling either version.</span></span>

<span data-ttu-id="99d2d-239">**同步**</span><span class="sxs-lookup"><span data-stu-id="99d2d-239">**Synchronous**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample19.cs)]

<span data-ttu-id="99d2d-240">**非同步-ASP.NET 4.5**</span><span class="sxs-lookup"><span data-stu-id="99d2d-240">**Asynchronous - ASP.NET 4.5**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

<span data-ttu-id="99d2d-241">**JavaScript 用戶端會使用產生的 proxy**</span><span class="sxs-lookup"><span data-stu-id="99d2d-241">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample21.js)]

<span data-ttu-id="99d2d-242">如需如何使用 ASP.NET 4.5 中的非同步方法的詳細資訊，請參閱[使用 ASP.NET MVC 4 中的非同步方法](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md)。</span><span class="sxs-lookup"><span data-stu-id="99d2d-242">For more information about how to use asynchronous methods in ASP.NET 4.5, see [Using Asynchronous Methods in ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span></span>

<a id="overloads"></a>

### <a name="defining-overloads"></a><span data-ttu-id="99d2d-243">定義多載</span><span class="sxs-lookup"><span data-stu-id="99d2d-243">Defining Overloads</span></span>

<span data-ttu-id="99d2d-244">如果您想要定義方法的多載，必須是不同中每個多載的參數數目。</span><span class="sxs-lookup"><span data-stu-id="99d2d-244">If you want to define overloads for a method, the number of parameters in each overload must be different.</span></span> <span data-ttu-id="99d2d-245">如果您只是藉由指定不同的參數類型區分多載，中樞類別將會編譯，但 SignalR 服務將會擲回例外狀況在執行階段，當用戶端會嘗試呼叫其中一個多載。</span><span class="sxs-lookup"><span data-stu-id="99d2d-245">If you differentiate an overload just by specifying different parameter types, your Hub class will compile but the SignalR service will throw an exception at run time when clients try to call one of the overloads.</span></span>

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a><span data-ttu-id="99d2d-246">如何從 Hub 類別的方法呼叫用戶端</span><span class="sxs-lookup"><span data-stu-id="99d2d-246">How to call client methods from the Hub class</span></span>

<span data-ttu-id="99d2d-247">若要從伺服器呼叫用戶端的方法，請使用`Clients`中樞類別中的方法中的屬性。</span><span class="sxs-lookup"><span data-stu-id="99d2d-247">To call client methods from the server, use the `Clients` property in a method in your Hub class.</span></span> <span data-ttu-id="99d2d-248">下列範例示範呼叫的伺服器程式碼`addNewMessageToPage`所有已連線的用戶端及 JavaScript 用戶端中定義之方法的用戶端程式碼。</span><span class="sxs-lookup"><span data-stu-id="99d2d-248">The following example shows server code that calls `addNewMessageToPage` on all connected clients, and client code that defines the method in a JavaScript client.</span></span>

<span data-ttu-id="99d2d-249">**Server**</span><span class="sxs-lookup"><span data-stu-id="99d2d-249">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample22.cs?highlight=5)]

<span data-ttu-id="99d2d-250">**JavaScript 用戶端會使用產生的 proxy**</span><span class="sxs-lookup"><span data-stu-id="99d2d-250">**JavaScript client using generated proxy**</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-server/samples/sample23.html?highlight=1)]

<span data-ttu-id="99d2d-251">您無法取得傳回值，從用戶端方法;例如語法`int x = Clients.All.add(1,1)`無法運作。</span><span class="sxs-lookup"><span data-stu-id="99d2d-251">You can't get a return value from a client method; syntax such as `int x = Clients.All.add(1,1)` does not work.</span></span>

<span data-ttu-id="99d2d-252">您可以指定複雜型別和參數的陣列。</span><span class="sxs-lookup"><span data-stu-id="99d2d-252">You can specify complex types and arrays for the parameters.</span></span> <span data-ttu-id="99d2d-253">下列範例會將複雜型別傳遞至方法參數中的用戶端。</span><span class="sxs-lookup"><span data-stu-id="99d2d-253">The following example passes a complex type to the client in a method parameter.</span></span>

<span data-ttu-id="99d2d-254">**呼叫用戶端使用的複雜物件的伺服器程式碼**</span><span class="sxs-lookup"><span data-stu-id="99d2d-254">**Server code that calls a client method using a complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample24.cs?highlight=3)]

<span data-ttu-id="99d2d-255">**定義的複雜物件的伺服器程式碼**</span><span class="sxs-lookup"><span data-stu-id="99d2d-255">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample25.cs?highlight=1)]

<span data-ttu-id="99d2d-256">**JavaScript 用戶端會使用產生的 proxy**</span><span class="sxs-lookup"><span data-stu-id="99d2d-256">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample26.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a><span data-ttu-id="99d2d-257">選取哪些用戶端會收到 RPC</span><span class="sxs-lookup"><span data-stu-id="99d2d-257">Selecting which clients will receive the RPC</span></span>

<span data-ttu-id="99d2d-258">用戶端屬性會傳回[HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx)提供數個選項來指定哪些用戶端會收到 RPC 的物件：</span><span class="sxs-lookup"><span data-stu-id="99d2d-258">The Clients property returns a [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) object that provides several options for specifying which clients will receive the RPC:</span></span>

- <span data-ttu-id="99d2d-259">所有已連線的用戶端。</span><span class="sxs-lookup"><span data-stu-id="99d2d-259">All connected clients.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample27.cs)]
- <span data-ttu-id="99d2d-260">只能呼叫用戶端。</span><span class="sxs-lookup"><span data-stu-id="99d2d-260">Only the calling client.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample28.cs)]
- <span data-ttu-id="99d2d-261">所有用戶端 （呼叫用戶端除外）。</span><span class="sxs-lookup"><span data-stu-id="99d2d-261">All clients except the calling client.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample29.cs)]
- <span data-ttu-id="99d2d-262">連接識別碼所識別的特定用戶端</span><span class="sxs-lookup"><span data-stu-id="99d2d-262">A specific client identified by connection ID.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample30.css)]

    <span data-ttu-id="99d2d-263">這個範例會呼叫`addContosoChatMessageToPage`呼叫用戶端上且使用相同的效果`Clients.Caller`。</span><span class="sxs-lookup"><span data-stu-id="99d2d-263">This example calls `addContosoChatMessageToPage` on the calling client and has the same effect as using `Clients.Caller`.</span></span>
- <span data-ttu-id="99d2d-264">除了指定用戶端連接識別碼所識別的所有已連線用戶端</span><span class="sxs-lookup"><span data-stu-id="99d2d-264">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample31.cs)]
- <span data-ttu-id="99d2d-265">指定的群組中所有已連線的用戶端。</span><span class="sxs-lookup"><span data-stu-id="99d2d-265">All connected clients in a specified group.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample32.css)]
- <span data-ttu-id="99d2d-266">除了指定用戶端連接識別碼所識別的指定群組中所有已連線的用戶端</span><span class="sxs-lookup"><span data-stu-id="99d2d-266">All connected clients in a specified group except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample33.cs)]
- <span data-ttu-id="99d2d-267">指定的群組中所有已連線的用戶端 （呼叫用戶端除外）。</span><span class="sxs-lookup"><span data-stu-id="99d2d-267">All connected clients in a specified group except the calling client.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample34.css)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a><span data-ttu-id="99d2d-268">方法名稱無法編譯時間驗證</span><span class="sxs-lookup"><span data-stu-id="99d2d-268">No compile-time validation for method names</span></span>

<span data-ttu-id="99d2d-269">您指定的方法名稱會解譯為動態的物件，表示沒有任何 IntelliSense 或它的編譯時間驗證。</span><span class="sxs-lookup"><span data-stu-id="99d2d-269">The method name that you specify is interpreted as a dynamic object, which means there is no IntelliSense or compile-time validation for it.</span></span> <span data-ttu-id="99d2d-270">在執行階段會評估運算式。</span><span class="sxs-lookup"><span data-stu-id="99d2d-270">The expression is evaluated at run time.</span></span> <span data-ttu-id="99d2d-271">當執行方法呼叫時，SignalR 的方法名稱和參數值傳送至用戶端，且如果用戶端有方法符合名稱、 呼叫的方法和參數值會傳遞給它。</span><span class="sxs-lookup"><span data-stu-id="99d2d-271">When the method call executes, SignalR sends the method name and the parameter values to the client, and if the client has a method that matches the name, that method is called and the parameter values are passed to it.</span></span> <span data-ttu-id="99d2d-272">如果用戶端上找到沒有對應的方法，就不會引發錯誤。</span><span class="sxs-lookup"><span data-stu-id="99d2d-272">If no matching method is found on the client, no error is raised.</span></span> <span data-ttu-id="99d2d-273">SignalR 傳輸至用戶端在幕後，當您呼叫用戶端方法的資料格式的相關資訊，請參閱[簡介 SignalR](index.md)。</span><span class="sxs-lookup"><span data-stu-id="99d2d-273">For information about the format of the data that SignalR transmits to the client behind the scenes when you call a client method, see [Introduction to SignalR](index.md).</span></span>

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a><span data-ttu-id="99d2d-274">不區分大小寫的方法名稱比對</span><span class="sxs-lookup"><span data-stu-id="99d2d-274">Case-insensitive method name matching</span></span>

<span data-ttu-id="99d2d-275">方法名稱比對不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="99d2d-275">Method name matching is case-insensitive.</span></span> <span data-ttu-id="99d2d-276">例如，`Clients.All.addContosoChatMessageToPage`在伺服器上將會執行`AddContosoChatMessageToPage`， `addcontosochatmessagetopage`，或`addContosoChatMessageToPage`用戶端上。</span><span class="sxs-lookup"><span data-stu-id="99d2d-276">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, or `addContosoChatMessageToPage` on the client.</span></span>

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a><span data-ttu-id="99d2d-277">非同步執行</span><span class="sxs-lookup"><span data-stu-id="99d2d-277">Asynchronous execution</span></span>

<span data-ttu-id="99d2d-278">以非同步方式執行，讓您呼叫這個方法。</span><span class="sxs-lookup"><span data-stu-id="99d2d-278">The method that you call executes asynchronously.</span></span> <span data-ttu-id="99d2d-279">來自用戶端的方法呼叫會立即執行，而不需等待 SignalR 完成傳輸資料至用戶端，除非您指定的後續的行程式碼應該等候方法完成之後的任何程式碼。</span><span class="sxs-lookup"><span data-stu-id="99d2d-279">Any code that comes after a method call to a client will execute immediately without waiting for SignalR to finish transmitting data to clients unless you specify that the subsequent lines of code should wait for method completion.</span></span> <span data-ttu-id="99d2d-280">下列程式碼範例示範如何依序執行兩個用戶端方法，一個是使用程式碼，使其能夠在.NET 4.5，另一個使用程式碼，使其能夠在.NET 4 中。</span><span class="sxs-lookup"><span data-stu-id="99d2d-280">The following code examples show how to execute two client methods sequentially, one using code that works in .NET 4.5, and one using code that works in .NET 4.</span></span>

<span data-ttu-id="99d2d-281">**.NET 4.5 範例**</span><span class="sxs-lookup"><span data-stu-id="99d2d-281">**.NET 4.5 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample35.cs?highlight=1,3)]

<span data-ttu-id="99d2d-282">**.NET 4 範例**</span><span class="sxs-lookup"><span data-stu-id="99d2d-282">**.NET 4 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample36.cs?highlight=3-4)]

<span data-ttu-id="99d2d-283">如果您使用`await`或`ContinueWith`等候，直到下的一行程式碼執行之前，用戶端方法完成，這不表示用戶端會實際收到訊息之前執行的下的一行程式碼。</span><span class="sxs-lookup"><span data-stu-id="99d2d-283">If you use `await` or `ContinueWith` to wait until a client method finishes before the next line of code executes, that does not mean that clients will actually receive the message before the next line of code executes.</span></span> <span data-ttu-id="99d2d-284">用戶端方法呼叫的 「 完成 」 只表示 SignalR 已經完成傳送訊息所需的所有項目。</span><span class="sxs-lookup"><span data-stu-id="99d2d-284">"Completion" of a client method call only means that SignalR has done everything necessary to send the message.</span></span> <span data-ttu-id="99d2d-285">如果您需要驗證用戶端收到訊息時，您必須自行撰寫此機制。</span><span class="sxs-lookup"><span data-stu-id="99d2d-285">If you need verification that clients received the message, you have to program that mechanism yourself.</span></span> <span data-ttu-id="99d2d-286">例如，您可以撰寫程式碼`MessageReceived`方法，在中樞內，並在`addContosoChatMessageToPage`方法，您可以呼叫用戶端上的`MessageReceived`這麼做之後，任何正常運作，您需要在用戶端上。</span><span class="sxs-lookup"><span data-stu-id="99d2d-286">For example, you could code a `MessageReceived` method on the Hub, and in the `addContosoChatMessageToPage` method on the client you could call `MessageReceived` after you do whatever work you need to do on the client.</span></span> <span data-ttu-id="99d2d-287">在`MessageReceived`中樞中您可以執行任何工作取決於實際的用戶端接收和處理的原始方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="99d2d-287">In `MessageReceived` in the Hub you can do whatever work depends on actual client reception and processing of the original method call.</span></span>

### <a name="how-to-use-a-string-variable-as-the-method-name"></a><span data-ttu-id="99d2d-288">如何使用字串變數，做為方法名稱</span><span class="sxs-lookup"><span data-stu-id="99d2d-288">How to use a string variable as the method name</span></span>

<span data-ttu-id="99d2d-289">如果您想要使用的字串變數的方法名稱，轉換為叫用用戶端方法`Clients.All`(或`Clients.Others`，`Clients.Caller`等等) 要`IClientProxy`，然後呼叫[叫用 （方法名稱、 引數...）](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="99d2d-289">If you want to invoke a client method by using a string variable as the method name, cast `Clients.All` (or `Clients.Others`, `Clients.Caller`, etc.) to `IClientProxy` and then call [Invoke(methodName, args...)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample37.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a><span data-ttu-id="99d2d-290">如何從中樞類別管理群組成員資格</span><span class="sxs-lookup"><span data-stu-id="99d2d-290">How to manage group membership from the Hub class</span></span>

<span data-ttu-id="99d2d-291">SignalR 中的群組提供方法，將訊息廣播至連線的用戶端指定的子集。</span><span class="sxs-lookup"><span data-stu-id="99d2d-291">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="99d2d-292">群組可以有任意數目的用戶端，並在用戶端可以是任意數目的群組成員。</span><span class="sxs-lookup"><span data-stu-id="99d2d-292">A group can have any number of clients, and a client can be a member of any number of groups.</span></span>

<span data-ttu-id="99d2d-293">為管理群組成員資格，使用[新增](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx)和[移除](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx)所提供的方法`Groups`中樞類別的屬性。</span><span class="sxs-lookup"><span data-stu-id="99d2d-293">To manage group membership, use the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) and [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods provided by the `Groups` property of the Hub class.</span></span> <span data-ttu-id="99d2d-294">下列範例所示`Groups.Add`和`Groups.Remove`方法用於 Hub 方法呼叫的用戶端程式碼，後面接著 JavaScript 用戶端程式碼呼叫它們。</span><span class="sxs-lookup"><span data-stu-id="99d2d-294">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods that are called by client code, followed by JavaScript client code that calls them.</span></span>

<span data-ttu-id="99d2d-295">**Server**</span><span class="sxs-lookup"><span data-stu-id="99d2d-295">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample38.cs?highlight=5,10)]

<span data-ttu-id="99d2d-296">**JavaScript 用戶端會使用產生的 proxy**</span><span class="sxs-lookup"><span data-stu-id="99d2d-296">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample39.js)]

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample40.js)]

<span data-ttu-id="99d2d-297">您不需要明確地建立群組。</span><span class="sxs-lookup"><span data-stu-id="99d2d-297">You don't have to explicitly create groups.</span></span> <span data-ttu-id="99d2d-298">作用中的群組會自動建立第一次呼叫中指定其名稱`Groups.Add`，並從它的成員資格中移除最後一次連接時，就會刪除。</span><span class="sxs-lookup"><span data-stu-id="99d2d-298">In effect a group is automatically created the first time you specify its name in a call to `Groups.Add`, and it is deleted when you remove the last connection from membership in it.</span></span>

<span data-ttu-id="99d2d-299">沒有任何 API 來取得群組成員資格清單或群組的清單。</span><span class="sxs-lookup"><span data-stu-id="99d2d-299">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="99d2d-300">SignalR 將訊息傳送至用戶端與群組根據[pub/sub 模型](http://en.wikipedia.org/wiki/Publish/subscribe)，而且伺服器不會保留群組或群組成員資格的清單。</span><span class="sxs-lookup"><span data-stu-id="99d2d-300">SignalR sends messages to clients and groups based on a [pub/sub model](http://en.wikipedia.org/wiki/Publish/subscribe), and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="99d2d-301">這有助於發揮最大延展性，因為每當您將節點加入至 web 伺服陣列時，必須傳播至新的節點 SignalR 維護任何狀態。</span><span class="sxs-lookup"><span data-stu-id="99d2d-301">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a><span data-ttu-id="99d2d-302">加入和移除方法的非同步執行</span><span class="sxs-lookup"><span data-stu-id="99d2d-302">Asynchronous execution of Add and Remove methods</span></span>

<span data-ttu-id="99d2d-303">`Groups.Add`和`Groups.Remove`方法以非同步方式執行。</span><span class="sxs-lookup"><span data-stu-id="99d2d-303">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span> <span data-ttu-id="99d2d-304">如果您想要新增到群組的用戶端，並立即傳送訊息至用戶端使用群組，您必須確定`Groups.Add`方法完成第一次。</span><span class="sxs-lookup"><span data-stu-id="99d2d-304">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the `Groups.Add` method finishes first.</span></span> <span data-ttu-id="99d2d-305">下列程式碼範例示範如何這樣做，請使用適用於.NET 4.5，以及使用在.NET 4 中運作的程式碼的程式碼</span><span class="sxs-lookup"><span data-stu-id="99d2d-305">The following code examples show how to do that, one by using code that works in .NET 4.5, and one by using code that works in .NET 4</span></span>

<span data-ttu-id="99d2d-306">**.NET 4.5 範例**</span><span class="sxs-lookup"><span data-stu-id="99d2d-306">**.NET 4.5 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

<span data-ttu-id="99d2d-307">**.NET 4 範例**</span><span class="sxs-lookup"><span data-stu-id="99d2d-307">**.NET 4 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample42.cs?highlight=3-4)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a><span data-ttu-id="99d2d-308">群組成員資格持續性</span><span class="sxs-lookup"><span data-stu-id="99d2d-308">Group membership persistence</span></span>

<span data-ttu-id="99d2d-309">SignalR 追蹤連線，而不是使用者，因此如果您希望使用者是相同群組中每次使用者建立的連接，您必須呼叫`Groups.Add`每次使用者建立新的連接。</span><span class="sxs-lookup"><span data-stu-id="99d2d-309">SignalR tracks connections, not users, so if you want a user to be in the same group every time the user establishes a connection, you have to call `Groups.Add` every time the user establishes a new connection.</span></span>

<span data-ttu-id="99d2d-310">之後連線暫時中斷，有時候 SignalR 可以連線自動還原。</span><span class="sxs-lookup"><span data-stu-id="99d2d-310">After a temporary loss of connectivity, sometimes SignalR can restore the connection automatically.</span></span> <span data-ttu-id="99d2d-311">在此情況下，SignalR 還原相同的連接，不會建立新的連接，並因此就會自動還原用戶端的群組成員資格。</span><span class="sxs-lookup"><span data-stu-id="99d2d-311">In that case, SignalR is restoring the same connection, not establishing a new connection, and so the client's group membership is automatically restored.</span></span> <span data-ttu-id="99d2d-312">這是可能甚至暫時中斷時將伺服器重新啟動或失敗，因為每個用戶端，包括群組的成員資格的連接狀態往返到用戶端。</span><span class="sxs-lookup"><span data-stu-id="99d2d-312">This is possible even when the temporary break is the result of a server reboot or failure, because connection state for each client, including group memberships, is round-tripped to the client.</span></span> <span data-ttu-id="99d2d-313">如果伺服器關閉，且會取代新的伺服器連接逾時之前，用戶端可以自動重新連線至新的伺服器，然後重新註冊它所屬的群組中。</span><span class="sxs-lookup"><span data-stu-id="99d2d-313">If a server goes down and is replaced by a new server before the connection times out, a client can reconnect automatically to the new server and re-enroll in groups it is a member of.</span></span>

<span data-ttu-id="99d2d-314">當無法自動恢復連線之後連線、 中斷, 或時連接逾時，或用戶端中斷連接 （例如，當瀏覽器瀏覽至新頁面），群組成員資格將會遺失。</span><span class="sxs-lookup"><span data-stu-id="99d2d-314">When a connection can't be restored automatically after a loss of connectivity, or when the connection times out, or when the client disconnects (for example, when a browser navigates to a new page), group memberships are lost.</span></span> <span data-ttu-id="99d2d-315">在使用者連接在下一次將新的連接。</span><span class="sxs-lookup"><span data-stu-id="99d2d-315">The next time the user connects will be a new connection.</span></span> <span data-ttu-id="99d2d-316">若要維護相同的使用者建立新的連接群組成員資格，您的應用程式必須追蹤、 使用者和群組之間的關聯，然後還原群組成員資格，每次使用者建立新的連接。</span><span class="sxs-lookup"><span data-stu-id="99d2d-316">To maintain group memberships when the same user establishes a new connection, your application has to track the associations between users and groups, and restore group memberships each time a user establishes a new connection.</span></span>

<span data-ttu-id="99d2d-317">如需連接和重新連線的詳細資訊，請參閱[如何處理連接的存留期事件中樞類別中](#connectionlifetime)本主題稍後。</span><span class="sxs-lookup"><span data-stu-id="99d2d-317">For more information about connections and reconnections, see [How to handle connection lifetime events in the Hub class](#connectionlifetime) later in this topic.</span></span>

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a><span data-ttu-id="99d2d-318">單一使用者群組</span><span class="sxs-lookup"><span data-stu-id="99d2d-318">Single-user groups</span></span>

<span data-ttu-id="99d2d-319">通常使用 SignalR 的應用程式需要追蹤的使用者與連線之間的關聯，才能知道哪些使用者已經傳送的訊息，以及哪些使用者應該能夠接收訊息。</span><span class="sxs-lookup"><span data-stu-id="99d2d-319">Applications that use SignalR typically have to keep track of the associations between users and connections in order to know which user has sent a message and which user(s) should be receiving a message.</span></span> <span data-ttu-id="99d2d-320">群組會這麼做可在兩個常用模式的其中一個用於。</span><span class="sxs-lookup"><span data-stu-id="99d2d-320">Groups are used in one of the two commonly used patterns for doing that.</span></span>

- <span data-ttu-id="99d2d-321">單一使用者群組。</span><span class="sxs-lookup"><span data-stu-id="99d2d-321">Single-user groups.</span></span>

    <span data-ttu-id="99d2d-322">您可以為群組名稱時，指定的使用者名稱及目前的連接識別碼新增至群組，每次使用者連線或重新連線。</span><span class="sxs-lookup"><span data-stu-id="99d2d-322">You can specify the user name as the group name, and add the current connection ID to the group every time the user connects or reconnects.</span></span> <span data-ttu-id="99d2d-323">若要傳送訊息給使用者，傳送至群組。</span><span class="sxs-lookup"><span data-stu-id="99d2d-323">To send messages to the user you send to the group.</span></span> <span data-ttu-id="99d2d-324">這個方法的缺點是該群組不會提供您找出使用者是否為線上或離線的方式。</span><span class="sxs-lookup"><span data-stu-id="99d2d-324">A disadvantage of this method is that the group doesn't provide you with a way to find out if the user is online or offline.</span></span>
- <span data-ttu-id="99d2d-325">追蹤使用者名稱和連接識別碼之間的關聯。</span><span class="sxs-lookup"><span data-stu-id="99d2d-325">Track associations between user names and connection IDs.</span></span>

    <span data-ttu-id="99d2d-326">您可以儲存在字典或資料庫，每個使用者名稱和一或多個連線識別碼之間的關聯，並更新儲存的資料每次使用者連線或中斷連線。</span><span class="sxs-lookup"><span data-stu-id="99d2d-326">You can store an association between each user name and one or more connection IDs in a dictionary or database, and update the stored data each time the user connects or disconnects.</span></span> <span data-ttu-id="99d2d-327">若要傳送訊息給使用者，您可以指定連線識別碼。</span><span class="sxs-lookup"><span data-stu-id="99d2d-327">To send messages to the user you specify the connection IDs.</span></span> <span data-ttu-id="99d2d-328">這個方法的缺點是，花費更多記憶體。</span><span class="sxs-lookup"><span data-stu-id="99d2d-328">A disadvantage of this method is that it takes more memory.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a><span data-ttu-id="99d2d-329">如何處理連接的存留期事件中樞類別中</span><span class="sxs-lookup"><span data-stu-id="99d2d-329">How to handle connection lifetime events in the Hub class</span></span>

<span data-ttu-id="99d2d-330">處理連接的存留期事件的常見原因是來追蹤是否已連線，以及追蹤的使用者名稱和連線識別碼之間的關聯。</span><span class="sxs-lookup"><span data-stu-id="99d2d-330">Typical reasons for handling connection lifetime events are to keep track of whether a user is connected or not, and to keep track of the association between user names and connection IDs.</span></span> <span data-ttu-id="99d2d-331">若要執行自己的程式碼，用戶端連接或中斷連線時，覆寫`OnConnected`， `OnDisconnected`，和`OnReconnected`類別虛擬方法的中樞，如下列範例所示。</span><span class="sxs-lookup"><span data-stu-id="99d2d-331">To run your own code when clients connect or disconnect, override the `OnConnected`, `OnDisconnected`, and `OnReconnected` virtual methods of the Hub class, as shown in the following example.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample43.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a><span data-ttu-id="99d2d-332">呼叫 OnConnected、 OnDisconnected 和 OnReconnected 的時機</span><span class="sxs-lookup"><span data-stu-id="99d2d-332">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>

<span data-ttu-id="99d2d-333">瀏覽器瀏覽至新頁面，每次新的連接必須建立，這表示將會執行 SignalR`OnDisconnected`後面接著方法`OnConnected`方法。</span><span class="sxs-lookup"><span data-stu-id="99d2d-333">Each time a browser navigates to a new page, a new connection has to be established, which means SignalR will execute the `OnDisconnected` method followed by the `OnConnected` method.</span></span> <span data-ttu-id="99d2d-334">建立新連線時讓 SignalR 一律會建立新的連接識別碼。</span><span class="sxs-lookup"><span data-stu-id="99d2d-334">SignalR always creates a new connection ID when a new connection is established.</span></span>

<span data-ttu-id="99d2d-335">`OnReconnected`呼叫方法時，發生暫時性中斷 SignalR 可以自動復原，例如當暫時中斷連線並重新連接的連接逾時之前的纜線的連線。`OnDisconnected`方法時呼叫，用戶端中斷連線並 SignalR 無法自動重新連線，例如當瀏覽器瀏覽至新的頁面。</span><span class="sxs-lookup"><span data-stu-id="99d2d-335">The `OnReconnected` method is called when there has been a temporary break in connectivity that SignalR can automatically recover from, such as when a cable is temporarily disconnected and reconnected before the connection times out. The `OnDisconnected` method is called when the client is disconnected and SignalR can't automatically reconnect, such as when a browser navigates to a new page.</span></span> <span data-ttu-id="99d2d-336">因此，可能針對給定的用戶端的事件序列是`OnConnected`， `OnReconnected`， `OnDisconnected`; 或`OnConnected`， `OnDisconnected`。</span><span class="sxs-lookup"><span data-stu-id="99d2d-336">Therefore, a possible sequence of events for a given client is `OnConnected`, `OnReconnected`, `OnDisconnected`; or `OnConnected`, `OnDisconnected`.</span></span> <span data-ttu-id="99d2d-337">不會看到序列`OnConnected`， `OnDisconnected`，`OnReconnected`給定連接。</span><span class="sxs-lookup"><span data-stu-id="99d2d-337">You won't see the sequence `OnConnected`, `OnDisconnected`, `OnReconnected` for a given connection.</span></span>

<span data-ttu-id="99d2d-338">`OnDisconnected`方法不會被呼叫，請在某些情況下，例如當伺服器關閉或回收應用程式定義域，取得。</span><span class="sxs-lookup"><span data-stu-id="99d2d-338">The `OnDisconnected` method doesn't get called in some scenarios, such as when a server goes down or the App Domain gets recycled.</span></span> <span data-ttu-id="99d2d-339">當另一部伺服器上線時或在應用程式網域完成其回收時，某些用戶端能夠重新連線，並引發`OnReconnected`事件。</span><span class="sxs-lookup"><span data-stu-id="99d2d-339">When another server comes on line or the App Domain completes its recycle, some clients may be able to reconnect and fire the `OnReconnected` event.</span></span>

<span data-ttu-id="99d2d-340">如需詳細資訊，請參閱[了解與 SignalR 中處理連接存留期事件](index.md)。</span><span class="sxs-lookup"><span data-stu-id="99d2d-340">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](index.md).</span></span>

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a><span data-ttu-id="99d2d-341">不會填入呼叫狀態</span><span class="sxs-lookup"><span data-stu-id="99d2d-341">Caller state not populated</span></span>

<span data-ttu-id="99d2d-342">連接生命週期事件處理常式方法會呼叫從伺服器上，這表示您將放在任何狀態`state`用戶端上的物件將不會填入`Caller`在伺服器上的屬性。</span><span class="sxs-lookup"><span data-stu-id="99d2d-342">The connection lifetime event handler methods are called from the server, which means that any state that you put in the `state` object on the client will not be populated in the `Caller` property on the server.</span></span> <span data-ttu-id="99d2d-343">如需有關資訊`state`物件和`Caller`屬性，請參閱[如何將用戶端與中樞類別之間傳遞狀態](#passstate)本主題稍後。</span><span class="sxs-lookup"><span data-stu-id="99d2d-343">For information about the `state` object and the `Caller` property, see [How to pass state between clients and the Hub class](#passstate) later in this topic.</span></span>

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a><span data-ttu-id="99d2d-344">如何取得用戶端的相關資訊，從內容屬性</span><span class="sxs-lookup"><span data-stu-id="99d2d-344">How to get information about the client from the Context property</span></span>

<span data-ttu-id="99d2d-345">若要取得用戶端的相關資訊，請使用`Context`中樞類別的屬性。</span><span class="sxs-lookup"><span data-stu-id="99d2d-345">To get information about the client, use the `Context` property of the Hub class.</span></span> <span data-ttu-id="99d2d-346">`Context`屬性會傳回[HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx)物件，其提供下列資訊的存取權：</span><span class="sxs-lookup"><span data-stu-id="99d2d-346">The `Context` property returns a [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) object which provides access to the following information:</span></span>

- <span data-ttu-id="99d2d-347">呼叫用戶端連接識別碼。</span><span class="sxs-lookup"><span data-stu-id="99d2d-347">The connection ID of the calling client.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample44.cs?highlight=1)]

    <span data-ttu-id="99d2d-348">連接識別碼是的 SignalR （您無法在自己的程式碼中指定的值） 所指派的 GUID。</span><span class="sxs-lookup"><span data-stu-id="99d2d-348">The connection ID is a GUID that is assigned by SignalR (you can't specify the value in your own code).</span></span> <span data-ttu-id="99d2d-349">還有一個針對每個連接和相同的連接識別碼由所有中樞，如果您的應用程式中有多個中樞的連線識別碼。</span><span class="sxs-lookup"><span data-stu-id="99d2d-349">There is one connection ID for each connection, and the same connection ID is used by all Hubs if you have multiple Hubs in your application.</span></span>
- <span data-ttu-id="99d2d-350">HTTP 標頭的資料。</span><span class="sxs-lookup"><span data-stu-id="99d2d-350">HTTP header data.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample45.cs?highlight=1)]

    <span data-ttu-id="99d2d-351">您也可以取得 HTTP 標頭從`Context.Headers`。</span><span class="sxs-lookup"><span data-stu-id="99d2d-351">You can also get HTTP headers from `Context.Headers`.</span></span> <span data-ttu-id="99d2d-352">多個參考相同的原因在於`Context.Headers`首先，建立`Context.Request`屬性之後，加入和`Context.Headers`保留回溯相容性。</span><span class="sxs-lookup"><span data-stu-id="99d2d-352">The reason for multiple references to the same thing is that `Context.Headers` was created first, the `Context.Request` property was added later, and `Context.Headers` was retained for backward compatibility.</span></span>
- <span data-ttu-id="99d2d-353">查詢字串的資料。</span><span class="sxs-lookup"><span data-stu-id="99d2d-353">Query string data.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample46.cs?highlight=1)]

    <span data-ttu-id="99d2d-354">您也可以取得查詢字串資料從`Context.QueryString`。</span><span class="sxs-lookup"><span data-stu-id="99d2d-354">You can also get query string data from `Context.QueryString`.</span></span>

    <span data-ttu-id="99d2d-355">您在這個屬性中取得查詢字串，則建立 SignalR 連線的 HTTP 要求搭配使用的一個。</span><span class="sxs-lookup"><span data-stu-id="99d2d-355">The query string that you get in this property is the one that was used with the HTTP request that established the SignalR connection.</span></span> <span data-ttu-id="99d2d-356">您可以加入用戶端中的查詢字串參數，藉由設定連接，這是方便的方式從用戶端的用戶端的相關資料傳送至伺服器。</span><span class="sxs-lookup"><span data-stu-id="99d2d-356">You can add query string parameters in the client by configuring the connection, which is a convenient way to pass data about the client from the client to the server.</span></span> <span data-ttu-id="99d2d-357">下列範例會示範其中一種方式新增的查詢字串中的 JavaScript 用戶端，當您使用產生的 proxy。</span><span class="sxs-lookup"><span data-stu-id="99d2d-357">The following example shows one way to add a query string in a JavaScript client when you use the generated proxy.</span></span>

    [!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample47.js?highlight=1)]

    <span data-ttu-id="99d2d-358">如需有關設定查詢字串參數的詳細資訊，請參閱應用程式開發介面指南[JavaScript](index.md)和[.NET](index.md)用戶端。</span><span class="sxs-lookup"><span data-stu-id="99d2d-358">For more information about setting query string parameters, see the API guides for the [JavaScript](index.md) and [.NET](index.md) clients.</span></span>

    <span data-ttu-id="99d2d-359">您可以找到連線上的查詢字串的資料，供內部使用 SignalR 的其他值一起使用的傳輸方法：</span><span class="sxs-lookup"><span data-stu-id="99d2d-359">You can find the transport method used for the connection in the query string data, along with some other values used internally by SignalR:</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample48.cs)]

    <span data-ttu-id="99d2d-360">值`transportMethod`會 「 webSockets"、"serverSentEvents"、"foreverFrame"或"longPolling"。</span><span class="sxs-lookup"><span data-stu-id="99d2d-360">The value of `transportMethod` will be "webSockets", "serverSentEvents", "foreverFrame" or "longPolling".</span></span> <span data-ttu-id="99d2d-361">請注意，如果您確認此值`OnConnected`事件處理常式方法，在某些情況下您可能一開始就會傳輸的值不是連線的最後一個交涉的傳輸方法。</span><span class="sxs-lookup"><span data-stu-id="99d2d-361">Note that if you check this value in the `OnConnected` event handler method, in some scenarios you might initially get a transport value that is not the final negotiated transport method for the connection.</span></span> <span data-ttu-id="99d2d-362">在此情況下方法將會擲回例外狀況，並建立最後傳輸方法時將稍後呼叫。</span><span class="sxs-lookup"><span data-stu-id="99d2d-362">In that case the method will throw an exception and will be called again later when the final transport method is established.</span></span>
- <span data-ttu-id="99d2d-363">Cookie。</span><span class="sxs-lookup"><span data-stu-id="99d2d-363">Cookies.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    <span data-ttu-id="99d2d-364">您也可以取得 cookie `Context.RequestCookies`。</span><span class="sxs-lookup"><span data-stu-id="99d2d-364">You can also get cookies from `Context.RequestCookies`.</span></span>
- <span data-ttu-id="99d2d-365">使用者資訊。</span><span class="sxs-lookup"><span data-stu-id="99d2d-365">User information.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample50.cs?highlight=1)]
- <span data-ttu-id="99d2d-366">要求的 HttpContext 物件：</span><span class="sxs-lookup"><span data-stu-id="99d2d-366">The HttpContext object for the request :</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample51.cs?highlight=1)]

    <span data-ttu-id="99d2d-367">使用這個方法，而非`HttpContext.Current`取得`HttpContext`SignalR 連線的物件。</span><span class="sxs-lookup"><span data-stu-id="99d2d-367">Use this method instead of getting `HttpContext.Current` to get the `HttpContext` object for the SignalR connection.</span></span>

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a><span data-ttu-id="99d2d-368">如何將用戶端與中樞類別之間傳遞狀態</span><span class="sxs-lookup"><span data-stu-id="99d2d-368">How to pass state between clients and the Hub class</span></span>

<span data-ttu-id="99d2d-369">用戶端 proxy 提供`state`您可以在您想要傳送到每個方法呼叫的伺服器的資料儲存的物件。</span><span class="sxs-lookup"><span data-stu-id="99d2d-369">The client proxy provides a `state` object in which you can store data that you want to be transmitted to the server with each method call.</span></span> <span data-ttu-id="99d2d-370">在伺服器上，您可以存取此資料在`Clients.Caller`中呼叫的用戶端中樞方法的屬性。</span><span class="sxs-lookup"><span data-stu-id="99d2d-370">On the server you can access this data in the `Clients.Caller` property in Hub methods that are called by clients.</span></span> <span data-ttu-id="99d2d-371">`Clients.Caller`連接生命週期事件處理常式方法不會擴展屬性`OnConnected`， `OnDisconnected`，和`OnReconnected`。</span><span class="sxs-lookup"><span data-stu-id="99d2d-371">The `Clients.Caller` property is not populated for the connection lifetime event handler methods `OnConnected`, `OnDisconnected`, and `OnReconnected`.</span></span>

<span data-ttu-id="99d2d-372">建立或更新資料集中的`state`物件和`Clients.Caller`屬性的運作方式在兩個方向。</span><span class="sxs-lookup"><span data-stu-id="99d2d-372">Creating or updating data in the `state` object and the `Clients.Caller` property works in both directions.</span></span> <span data-ttu-id="99d2d-373">您可以更新伺服器中的值，而且它們會傳回給用戶端。</span><span class="sxs-lookup"><span data-stu-id="99d2d-373">You can update values in the server and they are passed back to the client.</span></span>

<span data-ttu-id="99d2d-374">下列範例會顯示 JavaScript 用戶端程式碼，儲存到每個方法呼叫的伺服器傳輸的狀態。</span><span class="sxs-lookup"><span data-stu-id="99d2d-374">The following example shows JavaScript client code that stores state for transmission to the server with every method call.</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample52.js?highlight=1-2)]

<span data-ttu-id="99d2d-375">下列範例會顯示在.NET 用戶端對等的程式碼。</span><span class="sxs-lookup"><span data-stu-id="99d2d-375">The following example shows the equivalent code in a .NET client.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample53.cs?highlight=1-2)]

<span data-ttu-id="99d2d-376">在中樞類別中，您可以存取此資料在`Clients.Caller`屬性。</span><span class="sxs-lookup"><span data-stu-id="99d2d-376">In your Hub class, you can access this data in the `Clients.Caller` property.</span></span> <span data-ttu-id="99d2d-377">下列範例會顯示程式碼會擷取前一個範例中所參考的狀態。</span><span class="sxs-lookup"><span data-stu-id="99d2d-377">The following example shows code that retrieves the state referred to in the previous example.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample54.cs?highlight=3-4)]

> [!NOTE]
> <span data-ttu-id="99d2d-378">這項機制來保存狀態，並不適用於大量的資料，因為所有項目置於`state`或`Clients.Caller`屬性是往返與每個方法引動過程。</span><span class="sxs-lookup"><span data-stu-id="99d2d-378">This mechanism for persisting state is not intended for large amounts of data, since everything you put in the `state` or `Clients.Caller` property is round-tripped with every method invocation.</span></span> <span data-ttu-id="99d2d-379">它可用於較小的項目，例如使用者名稱或計數器。</span><span class="sxs-lookup"><span data-stu-id="99d2d-379">It's useful for smaller items such as user names or counters.</span></span>


<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a><span data-ttu-id="99d2d-380">如何在集線器類別中處理錯誤</span><span class="sxs-lookup"><span data-stu-id="99d2d-380">How to handle errors in the Hub class</span></span>

<span data-ttu-id="99d2d-381">若要處理中樞類別方法中發生的錯誤，使用其中一個或多個下列方法：</span><span class="sxs-lookup"><span data-stu-id="99d2d-381">To handle errors that occur in your Hub class methods, use one or both of the following methods:</span></span>

- <span data-ttu-id="99d2d-382">將方法的程式碼包裝在 try catch 區塊，並記錄例外狀況物件。</span><span class="sxs-lookup"><span data-stu-id="99d2d-382">Wrap your method code in try-catch blocks and log the exception object.</span></span> <span data-ttu-id="99d2d-383">以進行偵錯您可以將例外狀況傳送給用戶端，但不是建議原因的詳細的資訊傳送給用戶端在生產環境中的安全性。</span><span class="sxs-lookup"><span data-stu-id="99d2d-383">For debugging purposes you can send the exception to the client, but for security reasons sending detailed information to clients in production is not recommended.</span></span>
- <span data-ttu-id="99d2d-384">建立中樞管線模組來處理[OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx)方法。</span><span class="sxs-lookup"><span data-stu-id="99d2d-384">Create a Hubs pipeline module that handles the [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) method.</span></span> <span data-ttu-id="99d2d-385">下列範例會顯示記錄錯誤，後面接著程式碼中會插入至中樞管線的模組的 Global.asax 管線模組。</span><span class="sxs-lookup"><span data-stu-id="99d2d-385">The following example shows a pipeline module that logs errors, followed by code in Global.asax that injects the module into the Hubs pipeline.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample55.cs)]

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample56.cs?highlight=3)]

<span data-ttu-id="99d2d-386">如需中樞管線模組的詳細資訊，請參閱[如何以自訂中樞管線](#hubpipeline)本主題稍後。</span><span class="sxs-lookup"><span data-stu-id="99d2d-386">For more information about Hub pipeline modules, see [How to customize the Hubs pipeline](#hubpipeline) later in this topic.</span></span>

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a><span data-ttu-id="99d2d-387">如何啟用追蹤</span><span class="sxs-lookup"><span data-stu-id="99d2d-387">How to enable tracing</span></span>

<span data-ttu-id="99d2d-388">若要啟用伺服器端追蹤，system.diagnostics 項目加入您的 Web.config 檔案，在此範例所示：</span><span class="sxs-lookup"><span data-stu-id="99d2d-388">To enable server-side tracing, add a system.diagnostics element to your Web.config file, as shown in this example:</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-server/samples/sample57.html?highlight=17-72)]

<span data-ttu-id="99d2d-389">當您在 Visual Studio 中執行應用程式時，您可以檢視中的記錄檔**輸出**視窗。</span><span class="sxs-lookup"><span data-stu-id="99d2d-389">When you run the application in Visual Studio, you can view the logs in the **Output** window.</span></span>

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a><span data-ttu-id="99d2d-390">呼叫方法的用戶端和管理從中樞類別以外的群組</span><span class="sxs-lookup"><span data-stu-id="99d2d-390">How to call client methods and manage groups from outside the Hub class</span></span>

<span data-ttu-id="99d2d-391">若要從不同的類別，比集線器類別來呼叫用戶端方法，中樞取得 SignalR 內容物件的參考並使用該用戶端上呼叫方法，或管理群組。</span><span class="sxs-lookup"><span data-stu-id="99d2d-391">To call client methods from a different class than your Hub class, get a reference to the SignalR context object for the Hub and use that to call methods on the client or manage groups.</span></span>

<span data-ttu-id="99d2d-392">下列範例`StockTicker`類別取得內容物件，將它儲存在類別的執行個體、 將類別執行個體儲存在靜態屬性，並使用從單一類別執行個體的內容來呼叫`updateStockPrice`方法上的用戶端連接到集線器，名為`StockTickerHub`。</span><span class="sxs-lookup"><span data-stu-id="99d2d-392">The following sample `StockTicker` class gets the context object, stores it in an instance of the class, stores the class instance in a static property, and uses the context from the singleton class instance to call the `updateStockPrice` method on clients that are connected to a Hub named `StockTickerHub`.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample58.cs?highlight=8,24)]

<span data-ttu-id="99d2d-393">如果您需要長時間執行的物件中使用內容多時間，取得參考一次，並儲存它，而非每次進行取得。</span><span class="sxs-lookup"><span data-stu-id="99d2d-393">If you need to use the context multiple-times in a long-lived object, get the reference once and save it rather than getting it again each time.</span></span> <span data-ttu-id="99d2d-394">一次取得內容，可確保 SignalR 會將訊息傳送至用戶端所在 Hub 方法進行用戶端方法引動過程的順序相同。</span><span class="sxs-lookup"><span data-stu-id="99d2d-394">Getting the context once ensures that SignalR sends messages to clients in the same sequence in which your Hub methods make client method invocations.</span></span> <span data-ttu-id="99d2d-395">如需示範如何使用集線器的 SignalR 內容的教學課程，請參閱[伺服器廣播透過 ASP.NET SignalR](index.md)。</span><span class="sxs-lookup"><span data-stu-id="99d2d-395">For a tutorial that shows how to use the SignalR context for a Hub, see [Server Broadcast with ASP.NET SignalR](index.md).</span></span>

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a><span data-ttu-id="99d2d-396">呼叫用戶端方法</span><span class="sxs-lookup"><span data-stu-id="99d2d-396">Calling client methods</span></span>

<span data-ttu-id="99d2d-397">您可以指定哪些用戶端會收到 RPC，但是有比當您呼叫從 Hub 類別的選項較少。</span><span class="sxs-lookup"><span data-stu-id="99d2d-397">You can specify which clients will receive the RPC, but you have fewer options than when you call from a Hub class.</span></span> <span data-ttu-id="99d2d-398">這是因為內容未與特定的呼叫，從用戶端，相關聯，因此任何方法，需要了解目前的連線識別碼，例如`Clients.Others`，或`Clients.Caller`，或`Clients.OthersInGroup`，則無法使用。</span><span class="sxs-lookup"><span data-stu-id="99d2d-398">The reason for this is that the context is not associated with a particular call from a client, so any methods that require knowledge of the current connection ID, such as `Clients.Others`, or `Clients.Caller`, or `Clients.OthersInGroup`, are not available.</span></span> <span data-ttu-id="99d2d-399">有下列選項可供使用：</span><span class="sxs-lookup"><span data-stu-id="99d2d-399">The following options are available:</span></span>

- <span data-ttu-id="99d2d-400">所有已連線的用戶端。</span><span class="sxs-lookup"><span data-stu-id="99d2d-400">All connected clients.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample59.cs)]
- <span data-ttu-id="99d2d-401">連接識別碼所識別的特定用戶端</span><span class="sxs-lookup"><span data-stu-id="99d2d-401">A specific client identified by connection ID.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample60.css)]
- <span data-ttu-id="99d2d-402">除了指定用戶端連接識別碼所識別的所有已連線用戶端</span><span class="sxs-lookup"><span data-stu-id="99d2d-402">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample61.cs)]
- <span data-ttu-id="99d2d-403">指定的群組中所有已連線的用戶端。</span><span class="sxs-lookup"><span data-stu-id="99d2d-403">All connected clients in a specified group.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample62.css)]
- <span data-ttu-id="99d2d-404">除了指定的用戶端，連接識別碼所識別的指定群組中所有已連線的用戶端</span><span class="sxs-lookup"><span data-stu-id="99d2d-404">All connected clients in a specified group except specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample63.cs)]

<span data-ttu-id="99d2d-405">如果您呼叫非中樞類別方法從中樞類別中，您可以傳入目前的連接識別碼，並使用透過`Clients.Client`， `Clients.AllExcept`，或`Clients.Group`模擬`Clients.Caller`， `Clients.Others`，或`Clients.OthersInGroup`。</span><span class="sxs-lookup"><span data-stu-id="99d2d-405">If you are calling into your non-Hub class from methods in your Hub class, you can pass in the current connection ID and use that with `Clients.Client`, `Clients.AllExcept`, or `Clients.Group` to simulate `Clients.Caller`, `Clients.Others`, or `Clients.OthersInGroup`.</span></span> <span data-ttu-id="99d2d-406">在下列範例中，`MoveShapeHub`類別會傳遞至的連線識別碼`Broadcaster`類別以便`Broadcaster`類別可以模擬`Clients.Others`。</span><span class="sxs-lookup"><span data-stu-id="99d2d-406">In the following example, the `MoveShapeHub` class passes the connection ID to the `Broadcaster` class so that the `Broadcaster` class can simulate `Clients.Others`.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample64.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a><span data-ttu-id="99d2d-407">管理群組成員資格</span><span class="sxs-lookup"><span data-stu-id="99d2d-407">Managing group membership</span></span>

<span data-ttu-id="99d2d-408">用於管理群組您會有相同的選項，就像是在中樞類別。</span><span class="sxs-lookup"><span data-stu-id="99d2d-408">For managing groups you have the same options as you do in a Hub class.</span></span>

- <span data-ttu-id="99d2d-409">用戶端新增到群組</span><span class="sxs-lookup"><span data-stu-id="99d2d-409">Add a client to a group</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample65.cs)]
- <span data-ttu-id="99d2d-410">從群組移除用戶端</span><span class="sxs-lookup"><span data-stu-id="99d2d-410">Remove a client from a group</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample66.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a><span data-ttu-id="99d2d-411">如何以自訂中樞管線</span><span class="sxs-lookup"><span data-stu-id="99d2d-411">How to customize the Hubs pipeline</span></span>

<span data-ttu-id="99d2d-412">SignalR 可讓您將自己的程式碼插入中樞管線。</span><span class="sxs-lookup"><span data-stu-id="99d2d-412">SignalR enables you to inject your own code into the Hub pipeline.</span></span> <span data-ttu-id="99d2d-413">下列範例會顯示記錄來自用戶端和外寄的方法呼叫，用戶端上叫用每個傳入方法呼叫的自訂中樞管線模組：</span><span class="sxs-lookup"><span data-stu-id="99d2d-413">The following example shows a custom Hub pipeline module that logs each incoming method call received from the client and outgoing method call invoked on the client:</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample67.cs)]

<span data-ttu-id="99d2d-414">下列程式碼中*Global.asax*檔案會註冊在中樞管線中執行模組：</span><span class="sxs-lookup"><span data-stu-id="99d2d-414">The following code in the *Global.asax* file registers the module to run in the Hub pipeline:</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample68.cs?highlight=3)]

<span data-ttu-id="99d2d-415">有許多不同的方法，您可以覆寫。</span><span class="sxs-lookup"><span data-stu-id="99d2d-415">There are many different methods that you can override.</span></span> <span data-ttu-id="99d2d-416">如需完整清單，請參閱[HubPipelineModule 方法](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx)。</span><span class="sxs-lookup"><span data-stu-id="99d2d-416">For a complete list, see [HubPipelineModule Methods](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).</span></span>
