---
uid: signalr/overview/security/introduction-to-security
title: "SignalR 安全性簡介 |Microsoft 文件"
author: pfletcher
description: "描述開發 SignalR 應用程式時，您必須考量的安全性問題。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: ed562717-8591-4936-8e10-c7e63dcb570a
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/security/introduction-to-security
msc.type: authoredcontent
ms.openlocfilehash: ffe71f8ea7105db4d5a0c156e2b4e76d6e40761d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="introduction-to-signalr-security"></a><span data-ttu-id="07135-103">SignalR 安全性簡介</span><span class="sxs-lookup"><span data-stu-id="07135-103">Introduction to SignalR Security</span></span>
====================
<span data-ttu-id="07135-104">由[Patrick Fletcher](https://github.com/pfletcher)， [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="07135-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="07135-105">本文將告訴您開發的 SignalR 應用程式時必須考量的安全性問題。</span><span class="sxs-lookup"><span data-stu-id="07135-105">This article describes the security issues you must consider when developing a SignalR application.</span></span> 
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="07135-106">本主題中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="07135-106">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="07135-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="07135-107">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="07135-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="07135-108">.NET 4.5</span></span>
> - <span data-ttu-id="07135-109">SignalR 第 2 版</span><span class="sxs-lookup"><span data-stu-id="07135-109">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="07135-110">本主題的先前版本</span><span class="sxs-lookup"><span data-stu-id="07135-110">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="07135-111">如需舊版 SignalR 的資訊，請參閱[SignalR 舊版](../older-versions/index.md)。</span><span class="sxs-lookup"><span data-stu-id="07135-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="07135-112">問題和註解</span><span class="sxs-lookup"><span data-stu-id="07135-112">Questions and comments</span></span>
> 
> <span data-ttu-id="07135-113">請留下上如何您所喜歡的本教學課程，我們可以改進中將註解放在頁面底部的意見反應。</span><span class="sxs-lookup"><span data-stu-id="07135-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="07135-114">如果您有與本教學課程不直接相關的問題，您可以將它們來公佈[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或[StackOverflow.com](http://stackoverflow.com/)。</span><span class="sxs-lookup"><span data-stu-id="07135-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="07135-115">概觀</span><span class="sxs-lookup"><span data-stu-id="07135-115">Overview</span></span>

<span data-ttu-id="07135-116">本文件包含下列章節：</span><span class="sxs-lookup"><span data-stu-id="07135-116">This document contains the following sections:</span></span>

- [<span data-ttu-id="07135-117">SignalR 的安全性概念</span><span class="sxs-lookup"><span data-stu-id="07135-117">SignalR Security Concepts</span></span>](#concepts)

    - [<span data-ttu-id="07135-118">驗證和授權</span><span class="sxs-lookup"><span data-stu-id="07135-118">Authentication and authorization</span></span>](#authentication)
    - [<span data-ttu-id="07135-119">連線語彙基元</span><span class="sxs-lookup"><span data-stu-id="07135-119">Connection token</span></span>](#connectiontoken)
    - [<span data-ttu-id="07135-120">當重新連線時重新加入群組</span><span class="sxs-lookup"><span data-stu-id="07135-120">Rejoining groups when reconnecting</span></span>](#rejoingroup)
- [<span data-ttu-id="07135-121">SignalR 防止跨站台要求偽造的方式</span><span class="sxs-lookup"><span data-stu-id="07135-121">How SignalR prevents Cross-Site Request Forgery</span></span>](#csrf)
- [<span data-ttu-id="07135-122">SignalR 的安全性建議</span><span class="sxs-lookup"><span data-stu-id="07135-122">SignalR Security Recommendations</span></span>](#recommendations)

    - [<span data-ttu-id="07135-123">安全通訊端層 (SSL) 通訊協定</span><span class="sxs-lookup"><span data-stu-id="07135-123">Secure Socket Layers (SSL) protocol</span></span>](#ssl)
    - [<span data-ttu-id="07135-124">請勿使用群組做為安全性機制</span><span class="sxs-lookup"><span data-stu-id="07135-124">Do not use groups as a security mechanism</span></span>](#groupsecurity)
    - [<span data-ttu-id="07135-125">安全地處理來自用戶端的輸入</span><span class="sxs-lookup"><span data-stu-id="07135-125">Safely handling input from clients</span></span>](#input)
    - [<span data-ttu-id="07135-126">使用中連線的使用者狀態的變更，重新調整</span><span class="sxs-lookup"><span data-stu-id="07135-126">Reconciling a change in user status with an active connection</span></span>](#reconcile)
    - [<span data-ttu-id="07135-127">自動產生的 JavaScript proxy 檔案</span><span class="sxs-lookup"><span data-stu-id="07135-127">Automatically generated JavaScript proxy files</span></span>](#autogen)
    - [<span data-ttu-id="07135-128">例外狀況</span><span class="sxs-lookup"><span data-stu-id="07135-128">Exceptions</span></span>](#exceptions)

<a id="concepts"></a>

## <a name="signalr-security-concepts"></a><span data-ttu-id="07135-129">SignalR 的安全性概念</span><span class="sxs-lookup"><span data-stu-id="07135-129">SignalR Security Concepts</span></span>

<a id="authentication"></a>

### <a name="authentication-and-authorization"></a><span data-ttu-id="07135-130">驗證與授權</span><span class="sxs-lookup"><span data-stu-id="07135-130">Authentication and authorization</span></span>

<span data-ttu-id="07135-131">SignalR 不提供任何功能來驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="07135-131">SignalR does not provide any features for authenticating users.</span></span> <span data-ttu-id="07135-132">相反地，您將 SignalR 功能整合到現有的驗證結構，應用程式。</span><span class="sxs-lookup"><span data-stu-id="07135-132">Instead, you integrate the SignalR features into the existing authentication structure for an application.</span></span> <span data-ttu-id="07135-133">因為您會通常在您的應用程式，並使用您 SignalR 中驗證結果程式碼中，您可以驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="07135-133">You authenticate users as you would normally in your application, and work with the results of the authentication in your SignalR code.</span></span> <span data-ttu-id="07135-134">例如，您可能會使用 ASP.NET 表單驗證，讓使用者通過驗證，並接著在您的中樞，強制執行哪些使用者或角色有權呼叫的方法。</span><span class="sxs-lookup"><span data-stu-id="07135-134">For example, you might authenticate your users with ASP.NET forms authentication, and then in your hub, enforce which users or roles are authorized to call a method.</span></span> <span data-ttu-id="07135-135">在您的中樞，您也可以傳遞驗證資訊，例如使用者名稱或使用者是否屬於角色，用戶端。</span><span class="sxs-lookup"><span data-stu-id="07135-135">In your hub, you can also pass authentication information, such as user name or whether a user belongs to a role, to the client.</span></span>

<span data-ttu-id="07135-136">提供 SignalR[授權](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx)屬性來指定哪些使用者可以存取的集線器或方法。</span><span class="sxs-lookup"><span data-stu-id="07135-136">SignalR provides the [Authorize](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attribute to specify which users have access to a hub or method.</span></span> <span data-ttu-id="07135-137">您可以套用 Authorize 屬性集線器或中樞中的特定方法。</span><span class="sxs-lookup"><span data-stu-id="07135-137">You apply the Authorize attribute to either a hub or particular methods in a hub.</span></span> <span data-ttu-id="07135-138">若沒有授權屬性集線器上的所有公用方法可用來連線至中樞的用戶端。</span><span class="sxs-lookup"><span data-stu-id="07135-138">Without the Authorize attribute, all public methods on the hub are available to a client that is connected to the hub.</span></span> <span data-ttu-id="07135-139">如需集線器的詳細資訊，請參閱[SignalR 中樞的驗證和授權](hub-authorization.md)。</span><span class="sxs-lookup"><span data-stu-id="07135-139">For more information about hubs, see [Authentication and Authorization for SignalR Hubs](hub-authorization.md).</span></span>

<span data-ttu-id="07135-140">您套用`Authorize`屬性中心，但不持續連線。</span><span class="sxs-lookup"><span data-stu-id="07135-140">You apply the `Authorize` attribute to hubs, but not persistent connections.</span></span> <span data-ttu-id="07135-141">若要強制執行時使用授權規則`PersistentConnection`必須覆寫`AuthorizeRequest`方法。</span><span class="sxs-lookup"><span data-stu-id="07135-141">To enforce authorization rules when using a `PersistentConnection` you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="07135-142">如需持續連線的詳細資訊，請參閱[SignalR 持續連線的驗證和授權](persistent-connection-authorization.md)。</span><span class="sxs-lookup"><span data-stu-id="07135-142">For more information about persistent connections, see [Authentication and Authorization for SignalR Persistent Connections](persistent-connection-authorization.md).</span></span>

<a id="connectiontoken"></a>

### <a name="connection-token"></a><span data-ttu-id="07135-143">連線語彙基元</span><span class="sxs-lookup"><span data-stu-id="07135-143">Connection token</span></span>

<span data-ttu-id="07135-144">SignalR 可減少執行惡意的命令來驗證寄件者的身分識別的風險。</span><span class="sxs-lookup"><span data-stu-id="07135-144">SignalR mitigates the risk of executing malicious commands by validating the identity of the sender.</span></span> <span data-ttu-id="07135-145">針對每個要求中，用戶端和伺服器傳遞的連線權杖，其中包含連接識別碼和已驗證使用者的使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="07135-145">For each request, the client and server pass a connection token which contains the connection id and username for authenticated users.</span></span> <span data-ttu-id="07135-146">連接識別碼可唯一識別每個連線的用戶端。</span><span class="sxs-lookup"><span data-stu-id="07135-146">The connection id uniquely identifies each connected client.</span></span> <span data-ttu-id="07135-147">新的連接建立，而且在連接期間保存該識別碼時，伺服器會隨機產生的連線識別碼。</span><span class="sxs-lookup"><span data-stu-id="07135-147">The server randomly generates the connection id when a new connection is created, and persists that id for the duration of the connection.</span></span> <span data-ttu-id="07135-148">Web 應用程式的驗證機制提供使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="07135-148">The authentication mechanism for the web application provides the username.</span></span> <span data-ttu-id="07135-149">SignalR 使用加密和數位簽章來保護的連線語彙基元。</span><span class="sxs-lookup"><span data-stu-id="07135-149">SignalR uses encryption and a digital signature to protect the connection token.</span></span>

![](introduction-to-security/_static/image2.png)

<span data-ttu-id="07135-150">針對每個要求，伺服器會驗證權杖，以確定要求來自指定之使用者的內容。</span><span class="sxs-lookup"><span data-stu-id="07135-150">For each request, the server validates the contents of the token to ensure that the request is coming from the specified user.</span></span> <span data-ttu-id="07135-151">使用者名稱必須對應至連接識別碼。驗證的連接識別碼和使用者名稱，以 SignalR 可防止惡意使用者輕鬆地模擬另一位使用者。</span><span class="sxs-lookup"><span data-stu-id="07135-151">The username must correspond to the connection id. By validating both the connection id and the username, SignalR prevents a malicious user from easily impersonating another user.</span></span> <span data-ttu-id="07135-152">如果伺服器無法驗證的連線語彙基元，要求將會失敗。</span><span class="sxs-lookup"><span data-stu-id="07135-152">If the server cannot validate the connection token, the request fails.</span></span>

![](introduction-to-security/_static/image4.png)

<span data-ttu-id="07135-153">因為連接識別碼是在驗證程序的一部分，不應該顯示給其他使用者的一名使用者的連線識別碼，或在 cookie 中儲存的值，在用戶端，例如。</span><span class="sxs-lookup"><span data-stu-id="07135-153">Because the connection id is part of the verification process, you should not reveal one user's connection id to other users or store the value on the client, such as in a cookie.</span></span>

<a id="rejoingroup"></a>

### <a name="rejoining-groups-when-reconnecting"></a><span data-ttu-id="07135-154">當重新連線時重新加入群組</span><span class="sxs-lookup"><span data-stu-id="07135-154">Rejoining groups when reconnecting</span></span>

<span data-ttu-id="07135-155">根據預設，SignalR 應用程式將自動重新指派使用者給適當的群組從暫時中斷，例如當卸除並重新建立連接逾時之前連線重新連線時。當重新連接後，用戶端會傳遞包含連線識別碼並指派的群組的群組語彙基元。</span><span class="sxs-lookup"><span data-stu-id="07135-155">By default, the SignalR application will automatically re-assign a user to the appropriate groups when reconnecting from a temporary disruption, such as when a connection is dropped and re-established before the connection times out. When reconnecting, the client passes a group token that includes the connection id and the assigned groups.</span></span> <span data-ttu-id="07135-156">以數位方式簽署及加密的群組語彙基元。</span><span class="sxs-lookup"><span data-stu-id="07135-156">The group token is digitally signed and encrypted.</span></span> <span data-ttu-id="07135-157">用戶端重新連線; 之後，保留相同的連線識別碼因此，從重新連線的用戶端傳送的連線識別碼必須符合用戶端使用先前的連線識別碼。</span><span class="sxs-lookup"><span data-stu-id="07135-157">The client retains the same connection id after a reconnection; therefore, the connection id passed from the reconnected client must match the previous connection id used by the client.</span></span> <span data-ttu-id="07135-158">此驗證可防止惡意使用者傳遞加入未經授權的群組，重新連接時的要求。</span><span class="sxs-lookup"><span data-stu-id="07135-158">This verification prevents a malicious user from passing requests to join unauthorized groups when reconnecting.</span></span>

<span data-ttu-id="07135-159">不過，它是很重要的一點的群組語彙基元不會過期。</span><span class="sxs-lookup"><span data-stu-id="07135-159">However, it is important to note, that the group token does not expire.</span></span> <span data-ttu-id="07135-160">如果使用者屬於群組，在過去，但是該群組已禁止，該使用者可以模擬包含禁止的群組的群組語彙基元。</span><span class="sxs-lookup"><span data-stu-id="07135-160">If a user belonged to a group in the past, but was banned from that group, that user may be able to mimic a group token that includes the prohibited group.</span></span> <span data-ttu-id="07135-161">如果您需要安全的方式管理哪些使用者所屬的群組，您需要在資料庫中，例如儲存在伺服器上，該資料。</span><span class="sxs-lookup"><span data-stu-id="07135-161">If you need to securely manage which users belong to which groups, you need to store that data on the server, such as in a database.</span></span> <span data-ttu-id="07135-162">然後，將邏輯加入至您的應用程式，用來在伺服器上驗證使用者是否屬於其他群組。</span><span class="sxs-lookup"><span data-stu-id="07135-162">Then, add logic to your application that verifies on the server whether a user belongs to a group.</span></span> <span data-ttu-id="07135-163">如需的驗證群組成員資格的範例，請參閱[使用群組](../guide-to-the-api/working-with-groups.md)。</span><span class="sxs-lookup"><span data-stu-id="07135-163">For an example of verifying group membership, see [Working with groups](../guide-to-the-api/working-with-groups.md).</span></span>

<span data-ttu-id="07135-164">自動重新加入群組時才適用連接暫時中斷之後重新連線。</span><span class="sxs-lookup"><span data-stu-id="07135-164">Automatically rejoining groups only applies when a connection is reconnected after a temporary disruption.</span></span> <span data-ttu-id="07135-165">如果使用者中斷連線的不用離開應用程式或重新啟動應用程式，您的應用程式必須處理如何將該使用者新增至正確的群組。</span><span class="sxs-lookup"><span data-stu-id="07135-165">If a user disconnects by navigating away from the application or the application restarts, your application must handle how to add that user to the correct groups.</span></span> <span data-ttu-id="07135-166">如需詳細資訊，請參閱[使用群組](../guide-to-the-api/working-with-groups.md)。</span><span class="sxs-lookup"><span data-stu-id="07135-166">For more information, see [Working with groups](../guide-to-the-api/working-with-groups.md).</span></span>

<a id="csrf"></a>

## <a name="how-signalr-prevents-cross-site-request-forgery"></a><span data-ttu-id="07135-167">SignalR 防止跨站台要求偽造的方式</span><span class="sxs-lookup"><span data-stu-id="07135-167">How SignalR prevents Cross-Site Request Forgery</span></span>

<span data-ttu-id="07135-168">跨站台要求偽造 (CSRF) 攻擊是惡意網站傳送要求到易受攻擊的站台，使用者目前登入的位置。</span><span class="sxs-lookup"><span data-stu-id="07135-168">Cross-Site Request Forgery (CSRF) is an attack where a malicious site sends a request to a vulnerable site where the user is currently logged in.</span></span> <span data-ttu-id="07135-169">SignalR 以防止 CSRF 進行極不像是會用來建立您的 SignalR 應用程式的有效要求惡意網站。</span><span class="sxs-lookup"><span data-stu-id="07135-169">SignalR prevents CSRF by making it extremely unlikely for a malicious site to create a valid request for your SignalR application.</span></span>

### <a name="description-of-csrf-attack"></a><span data-ttu-id="07135-170">CSRF 攻擊的描述</span><span class="sxs-lookup"><span data-stu-id="07135-170">Description of CSRF attack</span></span>

<span data-ttu-id="07135-171">CSRF 攻擊的範例如下：</span><span class="sxs-lookup"><span data-stu-id="07135-171">Here is an example of a CSRF attack:</span></span>

1. <span data-ttu-id="07135-172">使用者登入 www.example.com 時，使用表單驗證。</span><span class="sxs-lookup"><span data-stu-id="07135-172">A user logs into www.example.com, using forms authentication.</span></span>
2. <span data-ttu-id="07135-173">伺服器會驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="07135-173">The server authenticates the user.</span></span> <span data-ttu-id="07135-174">伺服器的回應包含驗證 cookie。</span><span class="sxs-lookup"><span data-stu-id="07135-174">The response from the server includes an authentication cookie.</span></span>
3. <span data-ttu-id="07135-175">沒有登出，使用者會造訪惡意網站。</span><span class="sxs-lookup"><span data-stu-id="07135-175">Without logging out, the user visits a malicious web site.</span></span> <span data-ttu-id="07135-176">這個惡意網站包含 HTML 格式如下：</span><span class="sxs-lookup"><span data-stu-id="07135-176">This malicious site contains the following HTML form:</span></span> 

    [!code-html[Main](introduction-to-security/samples/sample1.html)]

 <span data-ttu-id="07135-177">請注意，張貼至容易遭受站台，惡意網站的表單動作。</span><span class="sxs-lookup"><span data-stu-id="07135-177">Notice that the form action posts to the vulnerable site, not to the malicious site.</span></span> <span data-ttu-id="07135-178">這是 CSRF 的 「 跨網站 」 部分。</span><span class="sxs-lookup"><span data-stu-id="07135-178">This is the "cross-site" part of CSRF.</span></span>
4. <span data-ttu-id="07135-179">使用者按一下 [提交] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="07135-179">The user clicks the submit button.</span></span> <span data-ttu-id="07135-180">瀏覽器包含與要求的驗證 cookie。</span><span class="sxs-lookup"><span data-stu-id="07135-180">The browser includes the authentication cookie with the request.</span></span>
5. <span data-ttu-id="07135-181">要求 example.com 與使用者的驗證內容，在伺服器上執行，並可以執行已驗證的使用者允許進行的任何動作。</span><span class="sxs-lookup"><span data-stu-id="07135-181">The request runs on the example.com server with the user's authentication context, and can do anything that an authenticated user is allowed to do.</span></span>

<span data-ttu-id="07135-182">雖然這個範例需要使用者按一下 [表單] 按鈕，惡意的頁面無法輕鬆地執行指令碼將 AJAX 要求傳送至您的 SignalR 應用程式一樣。</span><span class="sxs-lookup"><span data-stu-id="07135-182">Although this example requires the user to click the form button, the malicious page could just as easily run a script that sends an AJAX request to your SignalR application.</span></span> <span data-ttu-id="07135-183">此外，使用 SSL 無法防止 CSRF 攻擊中，因為惡意網站可以傳送"https://"要求。</span><span class="sxs-lookup"><span data-stu-id="07135-183">Moreover, using SSL does not prevent a CSRF attack, because the malicious site can send an "https://" request.</span></span>

<span data-ttu-id="07135-184">一般而言，CSRF 攻擊可能會對網站使用 cookie 進行驗證，因為瀏覽器傳送到目標網站的所有相關的 cookie。</span><span class="sxs-lookup"><span data-stu-id="07135-184">Typically, CSRF attacks are possible against web sites that use cookies for authentication, because browsers send all relevant cookies to the destination web site.</span></span> <span data-ttu-id="07135-185">不過，不限於利用 cookie CSRF 攻擊。</span><span class="sxs-lookup"><span data-stu-id="07135-185">However, CSRF attacks are not limited to exploiting cookies.</span></span> <span data-ttu-id="07135-186">比方說，也是很容易遭受基本和摘要式驗證。</span><span class="sxs-lookup"><span data-stu-id="07135-186">For example, Basic and Digest authentication are also vulnerable.</span></span> <span data-ttu-id="07135-187">在使用者登入基本或摘要式驗證後，瀏覽器將會自動傳送認證到工作階段結束為止。</span><span class="sxs-lookup"><span data-stu-id="07135-187">After a user logs in with Basic or Digest authentication, the browser automatically sends the credentials until the session ends.</span></span>

### <a name="csrf-mitigations-taken-by-signalr"></a><span data-ttu-id="07135-188">由 SignalR CSRF 防護</span><span class="sxs-lookup"><span data-stu-id="07135-188">CSRF mitigations taken by SignalR</span></span>

<span data-ttu-id="07135-189">SignalR 採取下列步驟可防止惡意網站建立您的應用程式的有效要求。</span><span class="sxs-lookup"><span data-stu-id="07135-189">SignalR takes the following steps to prevent a malicious site from creating valid requests to your application.</span></span> <span data-ttu-id="07135-190">SignalR 會依照依預設，您不需要採取任何動作，在程式碼中。</span><span class="sxs-lookup"><span data-stu-id="07135-190">SignalR takes these steps by default, you do not need to take any action in your code.</span></span>

- <span data-ttu-id="07135-191">**停用跨網域要求**</span><span class="sxs-lookup"><span data-stu-id="07135-191">**Disable cross domain requests**</span></span>  
 <span data-ttu-id="07135-192">SignalR 停用跨網域要求，以防止使用者從外部網域呼叫 SignalR 端點。</span><span class="sxs-lookup"><span data-stu-id="07135-192">SignalR disables cross domain requests to prevent users from calling a SignalR endpoint from an external domain.</span></span> <span data-ttu-id="07135-193">SignalR 會考慮任何來自外部網域無效的要求，並封鎖該要求。</span><span class="sxs-lookup"><span data-stu-id="07135-193">SignalR considers any request from an external domain to be invalid and blocks the request.</span></span> <span data-ttu-id="07135-194">我們建議您保留這個預設行為。否則，惡意網站無法誘騙使用者將命令傳送給您的網站。</span><span class="sxs-lookup"><span data-stu-id="07135-194">We recommend that you keep this default behavior; otherwise, a malicious site could trick users into sending commands to your site.</span></span> <span data-ttu-id="07135-195">如果您需要使用跨網域要求，請參閱[如何建立跨定義域連線](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)。</span><span class="sxs-lookup"><span data-stu-id="07135-195">If you need to use cross domain requests, see     [How to establish a cross-domain connection](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain) .</span></span>
- <span data-ttu-id="07135-196">**傳入查詢字串，而不是 cookie 的連線語彙基元**</span><span class="sxs-lookup"><span data-stu-id="07135-196">**Pass connection token in query string, not cookie**</span></span>  
 <span data-ttu-id="07135-197">SignalR 會當做 cookie，而不是傳遞做為查詢字串值的連線語彙基元。</span><span class="sxs-lookup"><span data-stu-id="07135-197">SignalR passes the connection token as a query string value, instead of as a cookie.</span></span> <span data-ttu-id="07135-198">在 cookie 中儲存的連線語彙基元是不安全，因為瀏覽器也可以在發生惡意程式碼時不小心轉送的連線語彙基元。</span><span class="sxs-lookup"><span data-stu-id="07135-198">Storing the connection token in a cookie is unsafe because the browser can inadvertently forward the connection token when malicious code is encountered.</span></span> <span data-ttu-id="07135-199">此外，查詢字串中傳遞的連線語彙基元會防止的連線語彙基元保存超出目前的連接。</span><span class="sxs-lookup"><span data-stu-id="07135-199">Also, passing the connection token in the query string prevents the connection token from persisting beyond the current connection.</span></span> <span data-ttu-id="07135-200">因此，惡意的使用者不能在另一位使用者的驗證認證的要求。</span><span class="sxs-lookup"><span data-stu-id="07135-200">Therefore, a malicious user cannot make a request under another user's authentication credentials.</span></span>
- <span data-ttu-id="07135-201">**確認連線語彙基元**</span><span class="sxs-lookup"><span data-stu-id="07135-201">**Verify connection token**</span></span>  
 <span data-ttu-id="07135-202">中所述[連線語彙基元](#connectiontoken) 區段中，伺服器會知道哪些連接識別碼是與每個已驗證的使用者相關聯。</span><span class="sxs-lookup"><span data-stu-id="07135-202">As described in the     [Connection token](#connectiontoken) section, the server knows which connection id is associated with each authenticated user.</span></span> <span data-ttu-id="07135-203">伺服器不會處理來自不符合使用者名稱之連線識別碼的任何要求。</span><span class="sxs-lookup"><span data-stu-id="07135-203">The server does not process any request from a connection id that does not match the user name.</span></span> <span data-ttu-id="07135-204">不太惡意的使用者無法猜到正確的要求，因為惡意的使用者必須知道的使用者名稱和目前的隨機產生的連線識別碼。結束連接時，該連接識別碼會變成無效。</span><span class="sxs-lookup"><span data-stu-id="07135-204">It is unlikely a malicious user could guess a valid request because the malicious user would have to know the user name and the current randomly-generated connection id. That connection id becomes invalid as soon as the connection is ended.</span></span> <span data-ttu-id="07135-205">匿名使用者不應有任何機密資訊的存取權。</span><span class="sxs-lookup"><span data-stu-id="07135-205">Anonymous users should not have access to any sensitive information.</span></span>

<a id="recommendations"></a>

## <a name="signalr-security-recommendations"></a><span data-ttu-id="07135-206">SignalR 的安全性建議</span><span class="sxs-lookup"><span data-stu-id="07135-206">SignalR Security Recommendations</span></span>

<a id="ssl"></a>

### <a name="secure-socket-layers-ssl-protocol"></a><span data-ttu-id="07135-207">安全通訊端層 (SSL) 通訊協定</span><span class="sxs-lookup"><span data-stu-id="07135-207">Secure Socket Layers (SSL) protocol</span></span>

<span data-ttu-id="07135-208">SSL 通訊協定會使用加密來保護用戶端與伺服器之間的資料傳輸。</span><span class="sxs-lookup"><span data-stu-id="07135-208">The SSL protocol uses encryption to secure the transport of data between a client and server.</span></span> <span data-ttu-id="07135-209">如果您的 SignalR 應用程式傳輸用戶端與伺服器之間的機密資訊，請使用 SSL 傳輸。</span><span class="sxs-lookup"><span data-stu-id="07135-209">If your SignalR application transmits sensitive information between the client and server, use SSL for the transport.</span></span> <span data-ttu-id="07135-210">如需有關設定 SSL 的詳細資訊，請參閱[如何設定 IIS 7 上 SSL](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis)。</span><span class="sxs-lookup"><span data-stu-id="07135-210">For more information about setting up SSL, see [How to set up SSL on IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).</span></span>

<a id="groupsecurity"></a>

### <a name="do-not-use-groups-as-a-security-mechanism"></a><span data-ttu-id="07135-211">請勿使用群組做為安全性機制</span><span class="sxs-lookup"><span data-stu-id="07135-211">Do not use groups as a security mechanism</span></span>

<span data-ttu-id="07135-212">群組是便利的方式收集相關的使用者，但它們不是安全的機制，來限制存取機密資訊。</span><span class="sxs-lookup"><span data-stu-id="07135-212">Groups are a convenient way of collecting related users, but they are not a secure mechanism for limiting access to sensitive information.</span></span> <span data-ttu-id="07135-213">使用者可以自動重新加入群組期間重新連線時，這是特別有用。</span><span class="sxs-lookup"><span data-stu-id="07135-213">This is especially true when users can automatically rejoin groups during a reconnect.</span></span> <span data-ttu-id="07135-214">相反地，請考慮將特殊權限的使用者加入至角色，並限制為只有該角色的成員的中樞方法的存取。</span><span class="sxs-lookup"><span data-stu-id="07135-214">Instead, consider adding privileged users to a role and limiting access to a hub method to only members of that role.</span></span> <span data-ttu-id="07135-215">如需限制存取權限角色的範例，請參閱[SignalR 中樞的驗證和授權](hub-authorization.md)。</span><span class="sxs-lookup"><span data-stu-id="07135-215">For an example of restricting access based on a role, see [Authentication and Authorization for SignalR Hubs](hub-authorization.md).</span></span> <span data-ttu-id="07135-216">重新連接時，請檢查使用者群組的存取權的範例，請參閱[使用群組](../guide-to-the-api/working-with-groups.md)。</span><span class="sxs-lookup"><span data-stu-id="07135-216">For an example of checking user access to groups when reconnecting, see [Working with groups](../guide-to-the-api/working-with-groups.md).</span></span>

<a id="input"></a>

### <a name="safely-handling-input-from-clients"></a><span data-ttu-id="07135-217">安全地處理來自用戶端的輸入</span><span class="sxs-lookup"><span data-stu-id="07135-217">Safely handling input from clients</span></span>

<span data-ttu-id="07135-218">若要確保惡意的使用者不會與其他使用者傳送指令碼，您必須編碼所有來自用戶端的輸入適用於其他用戶端的廣播。</span><span class="sxs-lookup"><span data-stu-id="07135-218">To ensure that a malicious user does not send script to other users, you must encode all input from clients that is intended for broadcast to other clients.</span></span> <span data-ttu-id="07135-219">因為您的 SignalR 應用程式可能有許多不同類型的用戶端，您應該編碼接收用戶端，而不是在伺服器上的訊息。</span><span class="sxs-lookup"><span data-stu-id="07135-219">You should encode messages on the receiving clients rather than the server, because your SignalR application may have many different types of clients.</span></span> <span data-ttu-id="07135-220">因此，HTML 編碼方式可執行 web 用戶端，但不適用於其他類型的用戶端。</span><span class="sxs-lookup"><span data-stu-id="07135-220">Therefore, HTML-encoding works for a web client, but not for other types of clients.</span></span> <span data-ttu-id="07135-221">例如，web 用戶端方法，以顯示在聊天訊息會安全地處理的使用者名稱和訊息藉由呼叫`html()`函式。</span><span class="sxs-lookup"><span data-stu-id="07135-221">For example, a web client method to display a chat message would safely handle the user name and message by calling the `html()` function.</span></span>

[!code-html[Main](introduction-to-security/samples/sample2.html?highlight=3-4)]

<a id="reconcile"></a>

### <a name="reconciling-a-change-in-user-status-with-an-active-connection"></a><span data-ttu-id="07135-222">使用中連線的使用者狀態的變更，重新調整</span><span class="sxs-lookup"><span data-stu-id="07135-222">Reconciling a change in user status with an active connection</span></span>

<span data-ttu-id="07135-223">如果使用者的驗證狀態變更時的作用中連接存在時，使用者會收到錯誤指出，「 在使用中的 SignalR 連線期間無法變更使用者的身分識別 」。</span><span class="sxs-lookup"><span data-stu-id="07135-223">If a user's authentication status changes while an active connection exists, the user will receive an error that states, "The user identity cannot change during an active SignalR connection."</span></span> <span data-ttu-id="07135-224">在此情況下，您的應用程式應重新連接到伺服器，以確定連接識別碼和使用者名稱會經過協調。</span><span class="sxs-lookup"><span data-stu-id="07135-224">In that case, your application should re-connect to the server to make sure the connection id and username are coordinated.</span></span> <span data-ttu-id="07135-225">比方說，如果您的應用程式可讓使用者登出的使用中連接存在時，連接的使用者名稱將不再符合下一個要求傳入的名稱。</span><span class="sxs-lookup"><span data-stu-id="07135-225">For example, if your application allows the user to log out while an active connection exists, the username for the connection will no longer match the name that is passed in for the next request.</span></span> <span data-ttu-id="07135-226">您會想要使用者登出時，先停止連線，然後重新啟動。</span><span class="sxs-lookup"><span data-stu-id="07135-226">You will want to stop the connection before the user logs out, and then restart it.</span></span>

<span data-ttu-id="07135-227">不過，務必注意，大部分的應用程式將不需要以手動方式停止及啟動連線。</span><span class="sxs-lookup"><span data-stu-id="07135-227">However, it is important to note that most applications will not need to manually stop and start the connection.</span></span> <span data-ttu-id="07135-228">如果您的應用程式將使用者重新導向至另一個頁面記錄處理程序，例如 Web Form 應用程式或 MVC 應用程式中的預設行為，或登出之後重新整理目前頁面上，使用中連接自動中斷連線，而不需要任何額外的動作。</span><span class="sxs-lookup"><span data-stu-id="07135-228">If your application redirects users to a separate page after logging out, such as the default behavior in a Web Forms application or MVC application, or refreshes the current page after logging out, the active connection is automatically disconnected and does not require any additional action.</span></span>

<span data-ttu-id="07135-229">下列範例會示範如何停止和啟動連線時使用者狀態已變更。</span><span class="sxs-lookup"><span data-stu-id="07135-229">The following example shows how to stop and start a connection when the user status has changed.</span></span>

[!code-html[Main](introduction-to-security/samples/sample3.html)]

<span data-ttu-id="07135-230">或者，如果您的網站使用滑動期限使用表單驗證，而沒有任何活動，以保持有效的驗證 cookie，可能會變更使用者的驗證狀態。</span><span class="sxs-lookup"><span data-stu-id="07135-230">Or, the user's authentication status may change if your site uses sliding expiration with Forms Authentication, and there is no activity to keep the authentication cookie valid.</span></span> <span data-ttu-id="07135-231">在此情況下，使用者將登出，使用者名稱將不再相符的連線語彙基元中的使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="07135-231">In that case, the user will be logged out and the user name will no longer match the user name in the connection token.</span></span> <span data-ttu-id="07135-232">您可以加入一些定期要求要保持有效的驗證 cookie 的 web 伺服器上的資源的指令碼來修正此問題。</span><span class="sxs-lookup"><span data-stu-id="07135-232">You can fix this problem by adding some script that periodically requests a resource on the web server to keep the authentication cookie valid.</span></span> <span data-ttu-id="07135-233">下列範例會示範如何要求資源每隔 30 分鐘。</span><span class="sxs-lookup"><span data-stu-id="07135-233">The following example shows how to request a resource every 30 minutes.</span></span>

[!code-javascript[Main](introduction-to-security/samples/sample4.js)]

<a id="autogen"></a>

### <a name="automatically-generated-javascript-proxy-files"></a><span data-ttu-id="07135-234">自動產生的 JavaScript proxy 檔案</span><span class="sxs-lookup"><span data-stu-id="07135-234">Automatically generated JavaScript proxy files</span></span>

<span data-ttu-id="07135-235">如果您不希望在每個使用者的 JavaScript proxy 檔案中包含所有的中樞和方法，您可以停用自動產生的檔案。</span><span class="sxs-lookup"><span data-stu-id="07135-235">If you do not want to include all of the hubs and methods in the JavaScript proxy file for each user, you can disable the automatic generation of the file.</span></span> <span data-ttu-id="07135-236">如果您有多個中樞和方法，但不是想要注意的所有方法的每個使用者，您可能會選擇此選項。</span><span class="sxs-lookup"><span data-stu-id="07135-236">You might choose this option if you have multiple hubs and methods, but do not want every user to be aware of all of the methods.</span></span> <span data-ttu-id="07135-237">設定以停用自動產生**EnableJavaScriptProxies**至**false**。</span><span class="sxs-lookup"><span data-stu-id="07135-237">You disable automatic generation by setting **EnableJavaScriptProxies** to **false**.</span></span>

[!code-csharp[Main](introduction-to-security/samples/sample5.cs)]

<span data-ttu-id="07135-238">如需 JavaScript proxy 檔案的詳細資訊，請參閱[產生的 proxy，它會為您](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy)。</span><span class="sxs-lookup"><span data-stu-id="07135-238">For more information about the JavaScript proxy files, see [The generated proxy and what it does for you](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy).</span></span> <a id="exceptions"></a>

### <a name="exceptions"></a><span data-ttu-id="07135-239">例外狀況</span><span class="sxs-lookup"><span data-stu-id="07135-239">Exceptions</span></span>

<span data-ttu-id="07135-240">您應該避免將例外狀況物件傳遞至用戶端，因為物件可能會公開給用戶端機密資訊。</span><span class="sxs-lookup"><span data-stu-id="07135-240">You should avoid passing exception objects to clients because the objects may expose sensitive information to the clients.</span></span> <span data-ttu-id="07135-241">相反地，會顯示相關的錯誤訊息的用戶端上呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="07135-241">Instead, call a method on the client that displays the relevant error message.</span></span>

[!code-csharp[Main](introduction-to-security/samples/sample6.cs)]
