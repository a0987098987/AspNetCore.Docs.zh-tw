---
uid: signalr/overview/security/introduction-to-security
title: SignalR 安全性簡介 |Microsoft Docs
author: bradygaster
description: 描述開發 SignalR 應用程式時，您必須考慮的安全性問題。
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: ed562717-8591-4936-8e10-c7e63dcb570a
msc.legacyurl: /signalr/overview/security/introduction-to-security
msc.type: authoredcontent
ms.openlocfilehash: 311bfb4279b1c919fddcef2c0aed657083f9c34f
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837321"
---
<a name="introduction-to-signalr-security"></a><span data-ttu-id="6b3f6-103">SignalR 安全性簡介</span><span class="sxs-lookup"><span data-stu-id="6b3f6-103">Introduction to SignalR Security</span></span>
====================
<span data-ttu-id="6b3f6-104">藉由[Patrick Fletcher](https://github.com/pfletcher)， [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="6b3f6-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="6b3f6-105">本文說明開發 SignalR 應用程式時，您必須考慮的安全性問題。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-105">This article describes the security issues you must consider when developing a SignalR application.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="6b3f6-106">本主題中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="6b3f6-106">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="6b3f6-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="6b3f6-107">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="6b3f6-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="6b3f6-108">.NET 4.5</span></span>
> - <span data-ttu-id="6b3f6-109">SignalR 第 2 版</span><span class="sxs-lookup"><span data-stu-id="6b3f6-109">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="6b3f6-110">本主題的上一個版本</span><span class="sxs-lookup"><span data-stu-id="6b3f6-110">Previous versions of this topic</span></span>
>
> <span data-ttu-id="6b3f6-111">如需舊版 SignalR 的資訊，請參閱[SignalR 舊版](../older-versions/index.md)。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="6b3f6-112">提出問題或意見</span><span class="sxs-lookup"><span data-stu-id="6b3f6-112">Questions and comments</span></span>
>
> <span data-ttu-id="6b3f6-113">您喜歡本教學課程中的方式，和我們可以改善在頁面底部的註解中，歡迎留下意見反應。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="6b3f6-114">如果您有不直接相關的教學課程中的問題，您可以張貼他們[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或是[StackOverflow.com](http://stackoverflow.com/)。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="6b3f6-115">總覽</span><span class="sxs-lookup"><span data-stu-id="6b3f6-115">Overview</span></span>

<span data-ttu-id="6b3f6-116">本文件包含下列章節：</span><span class="sxs-lookup"><span data-stu-id="6b3f6-116">This document contains the following sections:</span></span>

- [<span data-ttu-id="6b3f6-117">SignalR 安全性概念</span><span class="sxs-lookup"><span data-stu-id="6b3f6-117">SignalR Security Concepts</span></span>](#concepts)

    - [<span data-ttu-id="6b3f6-118">驗證與授權</span><span class="sxs-lookup"><span data-stu-id="6b3f6-118">Authentication and authorization</span></span>](#authentication)
    - [<span data-ttu-id="6b3f6-119">連線語彙基元</span><span class="sxs-lookup"><span data-stu-id="6b3f6-119">Connection token</span></span>](#connectiontoken)
    - [<span data-ttu-id="6b3f6-120">重新連線時，重新加入群組</span><span class="sxs-lookup"><span data-stu-id="6b3f6-120">Rejoining groups when reconnecting</span></span>](#rejoingroup)
- [<span data-ttu-id="6b3f6-121">SignalR 如何防止跨網站偽造要求</span><span class="sxs-lookup"><span data-stu-id="6b3f6-121">How SignalR prevents Cross-Site Request Forgery</span></span>](#csrf)
- [<span data-ttu-id="6b3f6-122">SignalR 安全性建議</span><span class="sxs-lookup"><span data-stu-id="6b3f6-122">SignalR Security Recommendations</span></span>](#recommendations)

    - [<span data-ttu-id="6b3f6-123">安全通訊端層 (SSL) 通訊協定</span><span class="sxs-lookup"><span data-stu-id="6b3f6-123">Secure Socket Layers (SSL) protocol</span></span>](#ssl)
    - [<span data-ttu-id="6b3f6-124">做為安全性機制，不使用群組</span><span class="sxs-lookup"><span data-stu-id="6b3f6-124">Do not use groups as a security mechanism</span></span>](#groupsecurity)
    - [<span data-ttu-id="6b3f6-125">安全地處理來自用戶端的輸入</span><span class="sxs-lookup"><span data-stu-id="6b3f6-125">Safely handling input from clients</span></span>](#input)
    - [<span data-ttu-id="6b3f6-126">使用中連線的使用者狀態的變更，重新調整</span><span class="sxs-lookup"><span data-stu-id="6b3f6-126">Reconciling a change in user status with an active connection</span></span>](#reconcile)
    - [<span data-ttu-id="6b3f6-127">自動產生的 JavaScript proxy 檔案</span><span class="sxs-lookup"><span data-stu-id="6b3f6-127">Automatically generated JavaScript proxy files</span></span>](#autogen)
    - [<span data-ttu-id="6b3f6-128">例外狀況</span><span class="sxs-lookup"><span data-stu-id="6b3f6-128">Exceptions</span></span>](#exceptions)

<a id="concepts"></a>

## <a name="signalr-security-concepts"></a><span data-ttu-id="6b3f6-129">SignalR 安全性概念</span><span class="sxs-lookup"><span data-stu-id="6b3f6-129">SignalR Security Concepts</span></span>

<a id="authentication"></a>

### <a name="authentication-and-authorization"></a><span data-ttu-id="6b3f6-130">驗證與授權</span><span class="sxs-lookup"><span data-stu-id="6b3f6-130">Authentication and authorization</span></span>

<span data-ttu-id="6b3f6-131">SignalR 不提供任何功能來驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-131">SignalR does not provide any features for authenticating users.</span></span> <span data-ttu-id="6b3f6-132">相反地，您將 SignalR 功能整合到現有的應用程式的驗證結構。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-132">Instead, you integrate the SignalR features into the existing authentication structure for an application.</span></span> <span data-ttu-id="6b3f6-133">因為您會通常在您的應用程式，並使用您 SignalR 中驗證結果程式碼中，您就會驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-133">You authenticate users as you would normally in your application, and work with the results of the authentication in your SignalR code.</span></span> <span data-ttu-id="6b3f6-134">例如，您可以驗證您的使用者，使用 ASP.NET 表單驗證，並接著在您的中樞，強制執行哪些使用者或角色有權呼叫的方法。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-134">For example, you might authenticate your users with ASP.NET forms authentication, and then in your hub, enforce which users or roles are authorized to call a method.</span></span> <span data-ttu-id="6b3f6-135">在您的中樞，您也可以傳遞驗證資訊，例如使用者名稱或使用者是否屬於的角色，用戶端。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-135">In your hub, you can also pass authentication information, such as user name or whether a user belongs to a role, to the client.</span></span>

<span data-ttu-id="6b3f6-136">提供 SignalR[授權](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx)屬性來指定哪些使用者可以存取的集線器或方法。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-136">SignalR provides the [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attribute to specify which users have access to a hub or method.</span></span> <span data-ttu-id="6b3f6-137">您將 Authorize 屬性套用至中樞或中樞內的特定方法時。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-137">You apply the Authorize attribute to either a hub or particular methods in a hub.</span></span> <span data-ttu-id="6b3f6-138">沒有授權屬性，該中樞上的所有公用方法可連線至中樞的用戶端。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-138">Without the Authorize attribute, all public methods on the hub are available to a client that is connected to the hub.</span></span> <span data-ttu-id="6b3f6-139">如需中樞的詳細資訊，請參閱[SignalR 中樞的驗證和授權](hub-authorization.md)。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-139">For more information about hubs, see [Authentication and Authorization for SignalR Hubs](hub-authorization.md).</span></span>

<span data-ttu-id="6b3f6-140">您套用`Authorize`屬性至中樞，但不是持續連線。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-140">You apply the `Authorize` attribute to hubs, but not persistent connections.</span></span> <span data-ttu-id="6b3f6-141">若要強制執行時使用授權規則`PersistentConnection`您必須覆寫`AuthorizeRequest`方法。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-141">To enforce authorization rules when using a `PersistentConnection` you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="6b3f6-142">如需持續連線的詳細資訊，請參閱[SignalR 持續連線的驗證和授權](persistent-connection-authorization.md)。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-142">For more information about persistent connections, see [Authentication and Authorization for SignalR Persistent Connections](persistent-connection-authorization.md).</span></span>

<a id="connectiontoken"></a>

### <a name="connection-token"></a><span data-ttu-id="6b3f6-143">連線語彙基元</span><span class="sxs-lookup"><span data-stu-id="6b3f6-143">Connection token</span></span>

<span data-ttu-id="6b3f6-144">SignalR 降低執行惡意的命令來驗證寄件者的身分識別的風險。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-144">SignalR mitigates the risk of executing malicious commands by validating the identity of the sender.</span></span> <span data-ttu-id="6b3f6-145">針對每個要求中，用戶端和伺服器傳遞連線權杖，其中包含已驗證使用者的使用者名稱與連接識別碼。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-145">For each request, the client and server pass a connection token which contains the connection id and username for authenticated users.</span></span> <span data-ttu-id="6b3f6-146">連線識別碼可唯一識別每個連線的用戶端。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-146">The connection id uniquely identifies each connected client.</span></span> <span data-ttu-id="6b3f6-147">新的連接建立，而且在連接期間持續該識別碼時，伺服器會隨機產生的連線識別碼。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-147">The server randomly generates the connection id when a new connection is created, and persists that id for the duration of the connection.</span></span> <span data-ttu-id="6b3f6-148">Web 應用程式的驗證機制提供使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-148">The authentication mechanism for the web application provides the username.</span></span> <span data-ttu-id="6b3f6-149">SignalR 使用加密和數位簽章來保護的連線語彙基元。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-149">SignalR uses encryption and a digital signature to protect the connection token.</span></span>

![](introduction-to-security/_static/image2.png)

<span data-ttu-id="6b3f6-150">針對每個要求，伺服器會驗證權杖，以確保要求來自指定之使用者的內容。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-150">For each request, the server validates the contents of the token to ensure that the request is coming from the specified user.</span></span> <span data-ttu-id="6b3f6-151">使用者名稱必須對應到的連線識別碼。藉由驗證的連線識別碼，而使用者名稱，SignalR 可以防止惡意使用者在輕鬆模擬另一位使用者。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-151">The username must correspond to the connection id. By validating both the connection id and the username, SignalR prevents a malicious user from easily impersonating another user.</span></span> <span data-ttu-id="6b3f6-152">如果伺服器無法驗證的連線語彙基元，要求將會失敗。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-152">If the server cannot validate the connection token, the request fails.</span></span>

![](introduction-to-security/_static/image4.png)

<span data-ttu-id="6b3f6-153">因為連線識別碼是在驗證程序的一部分，不應該顯示給其他使用者的一位使用者的連線識別碼，或這類的用戶端上的值儲存在 cookie 中。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-153">Because the connection id is part of the verification process, you should not reveal one user's connection id to other users or store the value on the client, such as in a cookie.</span></span>

#### <a name="connection-tokens-vs-other-token-types"></a><span data-ttu-id="6b3f6-154">與其他語彙基元類型的連線語彙基元</span><span class="sxs-lookup"><span data-stu-id="6b3f6-154">Connection tokens vs. other token types</span></span>

<span data-ttu-id="6b3f6-155">因為它們是工作階段權杖或驗證語彙基元，這會帶來風險，如果公開會出現安全性工具偶爾會標示連線權杖。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-155">Connection tokens are occasionally flagged by security tools because they appear to be session tokens or authentication tokens, which poses a risk if exposed.</span></span>

<span data-ttu-id="6b3f6-156">SignalR 的連線語彙基元不是驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-156">SignalR's connection token isn't an authentication token.</span></span> <span data-ttu-id="6b3f6-157">它用來確認提出此要求的使用者是同一個建立連接。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-157">It is used to confirm that the user making this request is the same one that created the connection.</span></span> <span data-ttu-id="6b3f6-158">連線語彙基元是必要的因為 ASP.NET SignalR 允許連線到伺服器之間移動。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-158">The connection token is necessary because ASP.NET SignalR allows connections to move between servers.</span></span> <span data-ttu-id="6b3f6-159">權杖關聯的特定使用者的連線，但不會判斷提示發出要求之使用者的身分識別。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-159">The token associates the connection with a particular user but doesn't assert the identity of the user making the request.</span></span> <span data-ttu-id="6b3f6-160">SignalR 要求適當的驗證，它必須有一些其他判斷提示使用者，例如 cookie 的身分識別的權杖或持有人權杖。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-160">For a SignalR request to be properly authenticated, it must have some other token that asserts the identity of the user, such as a cookie or bearer token.</span></span> <span data-ttu-id="6b3f6-161">不過，連線權杖本身會由該使用者，只有該權杖內含的連線識別碼提出要求的任何宣告是與該使用者相關聯。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-161">However, the connection token itself makes no claim that the request was made by that user, only that the connection ID contained within the token is associated with that user.</span></span>

<span data-ttu-id="6b3f6-162">連線語彙基元提供自己的未驗證宣告，因為它不被視為 「 工作階段 」 或 「 驗證 」 權杖。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-162">Since the connection token provides no authentication claim of its own, it isn't considered a "session" or "authentication" token.</span></span> <span data-ttu-id="6b3f6-163">取得指定的使用者的連線語彙基元，然後重新它執行要求驗證為不同的使用者 （或未驗證的要求） 中將會失敗，因為不符合要求的使用者身分識別並儲存在權杖中的身分識別。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-163">Taking a given user's connection token and replaying it in a request authenticated as a different user (or an unauthenticated request) will fail, because the user identity of the request and the identity stored in the token won't match.</span></span>

<a id="rejoingroup"></a>

### <a name="rejoining-groups-when-reconnecting"></a><span data-ttu-id="6b3f6-164">重新連線時，重新加入群組</span><span class="sxs-lookup"><span data-stu-id="6b3f6-164">Rejoining groups when reconnecting</span></span>

<span data-ttu-id="6b3f6-165">根據預設，SignalR 應用程式會自動重新將使用者指派給適當的群組從暫時中斷，例如當卸除並重新建立連線逾時之前連線重新連線時。當重新連接後，用戶端會傳遞包含連線識別碼和指派的群組的群組語彙基元。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-165">By default, the SignalR application will automatically re-assign a user to the appropriate groups when reconnecting from a temporary disruption, such as when a connection is dropped and re-established before the connection times out. When reconnecting, the client passes a group token that includes the connection id and the assigned groups.</span></span> <span data-ttu-id="6b3f6-166">以數位方式簽署及加密的群組語彙基元。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-166">The group token is digitally signed and encrypted.</span></span> <span data-ttu-id="6b3f6-167">用戶端重新連線; 之後，保留相同的連線識別碼因此，從重新連線的用戶端傳遞的連線識別碼必須符合用戶端使用先前的連線識別碼。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-167">The client retains the same connection id after a reconnection; therefore, the connection id passed from the reconnected client must match the previous connection id used by the client.</span></span> <span data-ttu-id="6b3f6-168">此驗證可防止惡意使用者傳遞要求加入未經授權的群組，當重新連線。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-168">This verification prevents a malicious user from passing requests to join unauthorized groups when reconnecting.</span></span>

<span data-ttu-id="6b3f6-169">不過，它是特別注意，群組語彙基元不會過期。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-169">However, it's important to note, that the group token does not expire.</span></span> <span data-ttu-id="6b3f6-170">如果使用者屬於群組，在過去，但是已被禁用，從該群組，則該使用者，將可以模擬包含禁止的群組的群組語彙基元。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-170">If a user belonged to a group in the past, but was banned from that group, that user may be able to mimic a group token that includes the prohibited group.</span></span> <span data-ttu-id="6b3f6-171">如果您需要安全地管理哪些使用者屬於哪些群組，您需要在資料庫中，例如儲存在伺服器上，該資料。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-171">If you need to securely manage which users belong to which groups, you need to store that data on the server, such as in a database.</span></span> <span data-ttu-id="6b3f6-172">然後，將邏輯加入至您在伺服器驗證使用者是否屬於群組的應用程式中。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-172">Then, add logic to your application that verifies on the server whether a user belongs to a group.</span></span> <span data-ttu-id="6b3f6-173">如需的驗證群組成員資格的範例，請參閱[使用群組](../guide-to-the-api/working-with-groups.md)。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-173">For an example of verifying group membership, see [Working with groups](../guide-to-the-api/working-with-groups.md).</span></span>

<span data-ttu-id="6b3f6-174">自動重新加入群組時，才適用連接會暫時中斷之後重新連線。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-174">Automatically rejoining groups only applies when a connection is reconnected after a temporary disruption.</span></span> <span data-ttu-id="6b3f6-175">如果使用者中斷連線的巡覽離開應用程式或應用程式重新啟動，您的應用程式必須處理如何將該使用者新增至正確的群組。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-175">If a user disconnects by navigating away from the application or the application restarts, your application must handle how to add that user to the correct groups.</span></span> <span data-ttu-id="6b3f6-176">如需詳細資訊，請參閱 <<c0> [ 使用群組](../guide-to-the-api/working-with-groups.md)。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-176">For more information, see [Working with groups](../guide-to-the-api/working-with-groups.md).</span></span>

<a id="csrf"></a>

## <a name="how-signalr-prevents-cross-site-request-forgery"></a><span data-ttu-id="6b3f6-177">SignalR 如何防止跨網站偽造要求</span><span class="sxs-lookup"><span data-stu-id="6b3f6-177">How SignalR prevents Cross-Site Request Forgery</span></span>

<span data-ttu-id="6b3f6-178">跨站台要求偽造 (CSRF) 攻擊會將惡意網站傳送要求給使用者目前登入的有弱點網站的位置。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-178">Cross-Site Request Forgery (CSRF) is an attack where a malicious site sends a request to a vulnerable site where the user is currently logged in.</span></span> <span data-ttu-id="6b3f6-179">SignalR 以防止 CSRF 進行惡意的網站，才能建立有效的要求，您的 SignalR 應用程式非常不可能。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-179">SignalR prevents CSRF by making it extremely unlikely for a malicious site to create a valid request for your SignalR application.</span></span>

### <a name="description-of-csrf-attack"></a><span data-ttu-id="6b3f6-180">CSRF 攻擊的描述</span><span class="sxs-lookup"><span data-stu-id="6b3f6-180">Description of CSRF attack</span></span>

<span data-ttu-id="6b3f6-181">CSRF 攻擊的範例如下：</span><span class="sxs-lookup"><span data-stu-id="6b3f6-181">Here is an example of a CSRF attack:</span></span>

1. <span data-ttu-id="6b3f6-182">使用者登入 www.example.com 中的，使用表單驗證。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-182">A user logs into www.example.com, using forms authentication.</span></span>
2. <span data-ttu-id="6b3f6-183">伺服器會驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-183">The server authenticates the user.</span></span> <span data-ttu-id="6b3f6-184">來自伺服器的回應包含驗證 cookie。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-184">The response from the server includes an authentication cookie.</span></span>
3. <span data-ttu-id="6b3f6-185">而不需要登出，使用者會造訪惡意的網站。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-185">Without logging out, the user visits a malicious web site.</span></span> <span data-ttu-id="6b3f6-186">此惡意的網站包含 HTML 格式如下：</span><span class="sxs-lookup"><span data-stu-id="6b3f6-186">This malicious site contains the following HTML form:</span></span>

    [!code-html[Main](introduction-to-security/samples/sample1.html)]

   <span data-ttu-id="6b3f6-187">請注意，張貼至易受攻擊的站台，不必惡意網站的表單動作。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-187">Notice that the form action posts to the vulnerable site, not to the malicious site.</span></span> <span data-ttu-id="6b3f6-188">這是 CSRF 的 「 跨站台 」 部分。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-188">This is the "cross-site" part of CSRF.</span></span>
4. <span data-ttu-id="6b3f6-189">使用者按一下 [提交] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-189">The user clicks the submit button.</span></span> <span data-ttu-id="6b3f6-190">瀏覽器會包含與要求的驗證 cookie。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-190">The browser includes the authentication cookie with the request.</span></span>
5. <span data-ttu-id="6b3f6-191">要求使用者的驗證內容，example.com 伺服器上執行，而且可以執行的任何已驗證的使用者能夠執行的動作項目。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-191">The request runs on the example.com server with the user's authentication context, and can do anything that an authenticated user is allowed to do.</span></span>

<span data-ttu-id="6b3f6-192">雖然此範例中所需的使用者可以按一下 [表單] 按鈕，惡意的頁面無法的身分執行的指令碼，將 AJAX 要求傳送至您的 SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-192">Although this example requires the user to click the form button, the malicious page could just as easily run a script that sends an AJAX request to your SignalR application.</span></span> <span data-ttu-id="6b3f6-193">此外，使用 SSL 不會防止 CSRF 攻擊中，因為惡意的網站可傳送"https://"要求。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-193">Moreover, using SSL does not prevent a CSRF attack, because the malicious site can send an "https://" request.</span></span>

<span data-ttu-id="6b3f6-194">一般而言，CSRF 攻擊是可能對網站使用 cookie 進行驗證，因為瀏覽器會將所有相關的 cookie 傳送至目的地網站。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-194">Typically, CSRF attacks are possible against web sites that use cookies for authentication, because browsers send all relevant cookies to the destination web site.</span></span> <span data-ttu-id="6b3f6-195">不過，不一定要利用 cookie CSRF 攻擊。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-195">However, CSRF attacks are not limited to exploiting cookies.</span></span> <span data-ttu-id="6b3f6-196">例如，基本和摘要式驗證也是易受攻擊。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-196">For example, Basic and Digest authentication are also vulnerable.</span></span> <span data-ttu-id="6b3f6-197">使用者登入時基本或摘要式驗證之後，瀏覽器將會自動傳送認證到工作階段結束為止。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-197">After a user logs in with Basic or Digest authentication, the browser automatically sends the credentials until the session ends.</span></span>

### <a name="csrf-mitigations-taken-by-signalr"></a><span data-ttu-id="6b3f6-198">SignalR 所採取的 CSRF 防護功能</span><span class="sxs-lookup"><span data-stu-id="6b3f6-198">CSRF mitigations taken by SignalR</span></span>

<span data-ttu-id="6b3f6-199">SignalR 採取下列步驟以建立您的應用程式的有效要求時，防止惡意網站。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-199">SignalR takes the following steps to prevent a malicious site from creating valid requests to your application.</span></span> <span data-ttu-id="6b3f6-200">SignalR 採取這些步驟，根據預設，您不需要採取任何動作，在您的程式碼。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-200">SignalR takes these steps by default, you do not need to take any action in your code.</span></span>

- <span data-ttu-id="6b3f6-201">**停用跨網域要求**SignalR 停用跨網域要求，以防止使用者呼叫 SignalR 端點來自外部網域。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-201">**Disable cross domain requests** SignalR disables cross domain requests to prevent users from calling a SignalR endpoint from an external domain.</span></span> <span data-ttu-id="6b3f6-202">SignalR 會考慮任何來自外部網域為無效的要求，並封鎖該要求。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-202">SignalR considers any request from an external domain to be invalid and blocks the request.</span></span> <span data-ttu-id="6b3f6-203">我們建議您保留這個預設行為。否則，惡意網站可能誘騙使用者將命令傳送給您的網站。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-203">We recommend that you keep this default behavior; otherwise, a malicious site could trick users into sending commands to your site.</span></span> <span data-ttu-id="6b3f6-204">如果您需要使用跨網域要求，請參閱[如何建立跨網域連接](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-204">If you need to use cross domain requests, see     [How to establish a cross-domain connection](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain) .</span></span>
- <span data-ttu-id="6b3f6-205">**傳入查詢字串，不 cookie 中的連線語彙基元**SignalR 的連線語彙基元，為查詢字串值，而不是傳遞為 cookie。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-205">**Pass connection token in query string, not cookie** SignalR passes the connection token as a query string value, instead of as a cookie.</span></span> <span data-ttu-id="6b3f6-206">在 cookie 中儲存的連線語彙基元是不安全的因為瀏覽器也可以在發生惡意程式碼時不小心轉送的連線語彙基元。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-206">Storing the connection token in a cookie is unsafe because the browser can inadvertently forward the connection token when malicious code is encountered.</span></span> <span data-ttu-id="6b3f6-207">此外，查詢字串中傳遞的連線語彙基元可以避免的連線語彙基元保存超過目前的連接。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-207">Also, passing the connection token in the query string prevents the connection token from persisting beyond the current connection.</span></span> <span data-ttu-id="6b3f6-208">因此，惡意使用者不能以其他使用者的驗證認證的要求。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-208">Therefore, a malicious user cannot make a request under another user's authentication credentials.</span></span>
- <span data-ttu-id="6b3f6-209">**確認連線語彙基元**中所述[連線語彙基元](#connectiontoken) 區段中，伺服器可讓您知道哪一個連接識別碼是與每個已驗證的使用者相關聯。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-209">**Verify connection token** As described in the     [Connection token](#connectiontoken) section, the server knows which connection id is associated with each authenticated user.</span></span> <span data-ttu-id="6b3f6-210">伺服器不會處理來自使用者名稱不相符之連線識別碼的任何要求。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-210">The server does not process any request from a connection id that does not match the user name.</span></span> <span data-ttu-id="6b3f6-211">不可能的惡意使用者猜到正確的要求因為惡意使用者必須知道的使用者名稱和目前的隨機產生的連線識別碼。只要結束連接時，該連線識別碼就會變成無效。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-211">It is unlikely a malicious user could guess a valid request because the malicious user would have to know the user name and the current randomly-generated connection id. That connection id becomes invalid as soon as the connection is ended.</span></span> <span data-ttu-id="6b3f6-212">匿名使用者不應該將任何機密資訊的存取。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-212">Anonymous users should not have access to any sensitive information.</span></span>

<a id="recommendations"></a>

## <a name="signalr-security-recommendations"></a><span data-ttu-id="6b3f6-213">SignalR 安全性建議</span><span class="sxs-lookup"><span data-stu-id="6b3f6-213">SignalR Security Recommendations</span></span>

<a id="ssl"></a>

### <a name="secure-socket-layers-ssl-protocol"></a><span data-ttu-id="6b3f6-214">安全通訊端層 (SSL) 通訊協定</span><span class="sxs-lookup"><span data-stu-id="6b3f6-214">Secure Socket Layers (SSL) protocol</span></span>

<span data-ttu-id="6b3f6-215">SSL 通訊協定會使用加密來保護用戶端與伺服器之間的資料傳輸。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-215">The SSL protocol uses encryption to secure the transport of data between a client and server.</span></span> <span data-ttu-id="6b3f6-216">如果您的 SignalR 應用程式傳輸用戶端與伺服器之間的機密資訊，請使用 SSL 傳輸。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-216">If your SignalR application transmits sensitive information between the client and server, use SSL for the transport.</span></span> <span data-ttu-id="6b3f6-217">如需有關設定 SSL 的詳細資訊，請參閱 <<c0> [ 如何設定 IIS 7 上的 SSL](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis)。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-217">For more information about setting up SSL, see [How to set up SSL on IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).</span></span>

<a id="groupsecurity"></a>

### <a name="do-not-use-groups-as-a-security-mechanism"></a><span data-ttu-id="6b3f6-218">做為安全性機制，不使用群組</span><span class="sxs-lookup"><span data-stu-id="6b3f6-218">Do not use groups as a security mechanism</span></span>

<span data-ttu-id="6b3f6-219">群組是便利的方式收集相關的使用者，但它們不安全的機制，以便限制存取機密資訊。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-219">Groups are a convenient way of collecting related users, but they are not a secure mechanism for limiting access to sensitive information.</span></span> <span data-ttu-id="6b3f6-220">當使用者可以自動重新加入群組的重新連線期間，這是特別有用。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-220">This is especially true when users can automatically rejoin groups during a reconnect.</span></span> <span data-ttu-id="6b3f6-221">相反地，請考慮新增至角色的權限的使用者，並限制只有該角色的成員到中樞方法的存取。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-221">Instead, consider adding privileged users to a role and limiting access to a hub method to only members of that role.</span></span> <span data-ttu-id="6b3f6-222">如需根據角色限制存取的範例，請參閱 < [SignalR 中樞的驗證和授權](hub-authorization.md)。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-222">For an example of restricting access based on a role, see [Authentication and Authorization for SignalR Hubs](hub-authorization.md).</span></span> <span data-ttu-id="6b3f6-223">檢查使用者存取群組，重新連接時的範例，請參閱 <<c0> [ 使用群組](../guide-to-the-api/working-with-groups.md)。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-223">For an example of checking user access to groups when reconnecting, see [Working with groups](../guide-to-the-api/working-with-groups.md).</span></span>

<a id="input"></a>

### <a name="safely-handling-input-from-clients"></a><span data-ttu-id="6b3f6-224">安全地處理來自用戶端的輸入</span><span class="sxs-lookup"><span data-stu-id="6b3f6-224">Safely handling input from clients</span></span>

<span data-ttu-id="6b3f6-225">若要確保惡意使用者不會對其他使用者傳送指令碼，您必須編碼所有來自用戶端的輸入，以供其他用戶端的廣播。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-225">To ensure that a malicious user does not send script to other users, you must encode all input from clients that is intended for broadcast to other clients.</span></span> <span data-ttu-id="6b3f6-226">因為您的 SignalR 應用程式可能有許多不同類型的用戶端，您應該編碼接收的用戶端，而不是在伺服器上的訊息。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-226">You should encode messages on the receiving clients rather than the server, because your SignalR application may have many different types of clients.</span></span> <span data-ttu-id="6b3f6-227">因此，HTML 編碼適用於 web 用戶端，但不適用於其他類型的用戶端。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-227">Therefore, HTML-encoding works for a web client, but not for other types of clients.</span></span> <span data-ttu-id="6b3f6-228">例如，web 用戶端方法，以顯示在聊天訊息會安全地處理的使用者名稱和訊息藉由呼叫`html()`函式。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-228">For example, a web client method to display a chat message would safely handle the user name and message by calling the `html()` function.</span></span>

[!code-html[Main](introduction-to-security/samples/sample2.html?highlight=3-4)]

<a id="reconcile"></a>

### <a name="reconciling-a-change-in-user-status-with-an-active-connection"></a><span data-ttu-id="6b3f6-229">使用中連線的使用者狀態的變更，重新調整</span><span class="sxs-lookup"><span data-stu-id="6b3f6-229">Reconciling a change in user status with an active connection</span></span>

<span data-ttu-id="6b3f6-230">如果使用者的驗證狀態會變更作用中連線存在時，使用者會收到錯誤訊息、 「 作用中的 SignalR 連線期間無法變更使用者的身分識別 」。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-230">If a user's authentication status changes while an active connection exists, the user will receive an error that states, "The user identity cannot change during an active SignalR connection."</span></span> <span data-ttu-id="6b3f6-231">在此情況下，您的應用程式應重新連接到伺服器，以確定使用者名稱與連接識別碼進行協調。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-231">In that case, your application should re-connect to the server to make sure the connection id and username are coordinated.</span></span> <span data-ttu-id="6b3f6-232">例如，如果您的應用程式可讓使用者登出的作用中連接存在時，連接的使用者名稱將不再符合下一個要求傳入的名稱。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-232">For example, if your application allows the user to log out while an active connection exists, the username for the connection will no longer match the name that is passed in for the next request.</span></span> <span data-ttu-id="6b3f6-233">您想要停止連線，才能在使用者登出時，以及再重新啟動它。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-233">You will want to stop the connection before the user logs out, and then restart it.</span></span>

<span data-ttu-id="6b3f6-234">不過，務必請注意，大部分的應用程式不需要手動停止和啟動的連線。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-234">However, it is important to note that most applications will not need to manually stop and start the connection.</span></span> <span data-ttu-id="6b3f6-235">如果您的應用程式會將使用者重新導向至另一個頁面記錄處理程序，例如 Web Form 應用程式或 MVC 應用程式的預設行為，或登出之後會重新整理目前的頁面，作用中連線會自動中斷連線，而不需要採取任何其他動作。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-235">If your application redirects users to a separate page after logging out, such as the default behavior in a Web Forms application or MVC application, or refreshes the current page after logging out, the active connection is automatically disconnected and does not require any additional action.</span></span>

<span data-ttu-id="6b3f6-236">下列範例示範如何停止和啟動的連線，使用者狀態變更時。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-236">The following example shows how to stop and start a connection when the user status has changed.</span></span>

[!code-html[Main](introduction-to-security/samples/sample3.html)]

<span data-ttu-id="6b3f6-237">或者，如果您的網站使用表單驗證，使用滑動期限，而且沒有任何活動，將有效的驗證 cookie，可能會變更使用者的驗證狀態。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-237">Or, the user's authentication status may change if your site uses sliding expiration with Forms Authentication, and there is no activity to keep the authentication cookie valid.</span></span> <span data-ttu-id="6b3f6-238">在此情況下，會將使用者登出，使用者名稱將不再符合的連線語彙基元中的使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-238">In that case, the user will be logged out and the user name will no longer match the user name in the connection token.</span></span> <span data-ttu-id="6b3f6-239">您可以藉由新增一些指令碼定期要求要保持有效的驗證 cookie 的 web 伺服器上的資源來修正這個問題。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-239">You can fix this problem by adding some script that periodically requests a resource on the web server to keep the authentication cookie valid.</span></span> <span data-ttu-id="6b3f6-240">下列範例示範如何要求資源每隔 30 分鐘。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-240">The following example shows how to request a resource every 30 minutes.</span></span>

[!code-javascript[Main](introduction-to-security/samples/sample4.js)]

<a id="autogen"></a>

### <a name="automatically-generated-javascript-proxy-files"></a><span data-ttu-id="6b3f6-241">自動產生的 JavaScript proxy 檔案</span><span class="sxs-lookup"><span data-stu-id="6b3f6-241">Automatically generated JavaScript proxy files</span></span>

<span data-ttu-id="6b3f6-242">如果您不想在每個使用者的 JavaScript proxy 檔案中包含所有中樞和方法，您可以停用自動產生的檔案。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-242">If you do not want to include all of the hubs and methods in the JavaScript proxy file for each user, you can disable the automatic generation of the file.</span></span> <span data-ttu-id="6b3f6-243">如果您有多個中樞和方法，但不是想要注意的所有方法的每位使用者，您可能會選擇此選項。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-243">You might choose this option if you have multiple hubs and methods, but do not want every user to be aware of all of the methods.</span></span> <span data-ttu-id="6b3f6-244">您設定停用自動產生**EnableJavaScriptProxies**要**false**。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-244">You disable automatic generation by setting **EnableJavaScriptProxies** to **false**.</span></span>

[!code-csharp[Main](introduction-to-security/samples/sample5.cs)]

<span data-ttu-id="6b3f6-245">如需 JavaScript proxy 檔案的詳細資訊，請參閱[產生的 proxy，它會為您](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy)。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-245">For more information about the JavaScript proxy files, see [The generated proxy and what it does for you](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy).</span></span> <a id="exceptions"></a>

### <a name="exceptions"></a><span data-ttu-id="6b3f6-246">例外狀況</span><span class="sxs-lookup"><span data-stu-id="6b3f6-246">Exceptions</span></span>

<span data-ttu-id="6b3f6-247">您應該避免將例外狀況物件傳遞至用戶端，因為物件可能會公開機密資訊的用戶端。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-247">You should avoid passing exception objects to clients because the objects may expose sensitive information to the clients.</span></span> <span data-ttu-id="6b3f6-248">相反地，會顯示相關的錯誤訊息的用戶端上呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="6b3f6-248">Instead, call a method on the client that displays the relevant error message.</span></span>

[!code-csharp[Main](introduction-to-security/samples/sample6.cs)]
