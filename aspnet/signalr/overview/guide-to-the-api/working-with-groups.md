---
uid: signalr/overview/guide-to-the-api/working-with-groups
title: 使用 signalr 的群組 |Microsoft Docs
author: pfletcher
description: 本主題描述如何保存中樞 API 使用的群組成員資格資訊。
ms.author: riande
ms.date: 06/10/2014
ms.assetid: cd378ecd-3e9e-4236-b902-65916d85a048
msc.legacyurl: /signalr/overview/guide-to-the-api/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: bf4b8597fe9c0bff06b95cfb9970211106191fd4
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287633"
---
<a name="working-with-groups-in-signalr"></a><span data-ttu-id="24b06-103">使用 signalr 的群組</span><span class="sxs-lookup"><span data-stu-id="24b06-103">Working with Groups in SignalR</span></span>
====================
<span data-ttu-id="24b06-104">藉由[Patrick Fletcher](https://github.com/pfletcher)， [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="24b06-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="24b06-105">本主題描述如何將使用者新增至群組，並將保存群組成員資格資訊。</span><span class="sxs-lookup"><span data-stu-id="24b06-105">This topic describes how to add users to groups and persist group membership information.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="24b06-106">本主題中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="24b06-106">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="24b06-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="24b06-107">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="24b06-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="24b06-108">.NET 4.5</span></span>
> - <span data-ttu-id="24b06-109">SignalR 第 2 版</span><span class="sxs-lookup"><span data-stu-id="24b06-109">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="24b06-110">本主題的上一個版本</span><span class="sxs-lookup"><span data-stu-id="24b06-110">Previous versions of this topic</span></span>
>
> <span data-ttu-id="24b06-111">如需舊版 SignalR 的資訊，請參閱[SignalR 舊版](../older-versions/index.md)。</span><span class="sxs-lookup"><span data-stu-id="24b06-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="24b06-112">提出問題或意見</span><span class="sxs-lookup"><span data-stu-id="24b06-112">Questions and comments</span></span>
>
> <span data-ttu-id="24b06-113">您喜歡本教學課程中的方式，和我們可以改善在頁面底部的註解中，歡迎留下意見反應。</span><span class="sxs-lookup"><span data-stu-id="24b06-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="24b06-114">如果您有不直接相關的教學課程中的問題，您可以張貼他們[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或是[StackOverflow.com](http://stackoverflow.com/)。</span><span class="sxs-lookup"><span data-stu-id="24b06-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="24b06-115">總覽</span><span class="sxs-lookup"><span data-stu-id="24b06-115">Overview</span></span>

<span data-ttu-id="24b06-116">Signalr 的群組提供一種方法將訊息廣播至連線的用戶端的指定子集。</span><span class="sxs-lookup"><span data-stu-id="24b06-116">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="24b06-117">群組可以有任意數目的用戶端，並在用戶端可以是任意數目的群組成員。</span><span class="sxs-lookup"><span data-stu-id="24b06-117">A group can have any number of clients, and a client can be a member of any number of groups.</span></span> <span data-ttu-id="24b06-118">您不需要明確建立群組。</span><span class="sxs-lookup"><span data-stu-id="24b06-118">You don't have to explicitly create groups.</span></span> <span data-ttu-id="24b06-119">作用中，群組會自動建立第一次您 Groups.Add，呼叫中指定其名稱，並從它的成員資格中移除最後一次連接時，就會刪除。</span><span class="sxs-lookup"><span data-stu-id="24b06-119">In effect, a group is automatically created the first time you specify its name in a call to Groups.Add, and it is deleted when you remove the last connection from membership in it.</span></span> <span data-ttu-id="24b06-120">如需使用群組，請參閱 <<c0> [ 如何管理群組成員資格從中樞類別](hubs-api-guide-server.md#groupsfromhub)中樞 API-Server 指南中。</span><span class="sxs-lookup"><span data-stu-id="24b06-120">For an introduction to using groups, see [How to manage group membership from the Hub class](hubs-api-guide-server.md#groupsfromhub) in the Hubs API - Server Guide.</span></span>

<span data-ttu-id="24b06-121">取得群組成員資格清單或群組的清單中沒有任何 API。</span><span class="sxs-lookup"><span data-stu-id="24b06-121">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="24b06-122">SignalR 將訊息傳送至用戶端和 pub/sub 模型，為基礎的群組，伺服器無法維護的群組或群組成員資格清單。</span><span class="sxs-lookup"><span data-stu-id="24b06-122">SignalR sends messages to clients and groups based on a pub/sub model, and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="24b06-123">這可協助最大化擴充性，因為每當您將節點加入至 web 伺服陣列時，SignalR 維護任何狀態傳播到新的節點。</span><span class="sxs-lookup"><span data-stu-id="24b06-123">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<span data-ttu-id="24b06-124">當您將使用者加入群組，使用`Groups.Add`方法，使用者會收到訊息導向至該群組中，目前的連接，持續時間，但該群組中的使用者成員資格不會保存超過目前的連接。</span><span class="sxs-lookup"><span data-stu-id="24b06-124">When you add a user to a group using the `Groups.Add` method, the user receives messages directed to that group for the duration of the current connection, but the user's membership in that group is not persisted beyond the current connection.</span></span> <span data-ttu-id="24b06-125">如果您想要永久保留群組和群組成員資格的相關資訊，您必須在存放庫，例如資料庫或 Azure 資料表儲存體中儲存該資料。</span><span class="sxs-lookup"><span data-stu-id="24b06-125">If you want to permanently retain information about groups and group membership, you must store that data in a repository such as a database or Azure table storage.</span></span> <span data-ttu-id="24b06-126">然後，使用者連線到您的應用程式中，每次您儲存機制中擷取使用者所屬的群組並手動將該使用者新增至這些群組。</span><span class="sxs-lookup"><span data-stu-id="24b06-126">Then, each time a user connects to your application, you retrieve from the repository which groups the user belongs to, and manually add that user to those groups.</span></span>

<span data-ttu-id="24b06-127">當重新連線暫時中斷之後，使用者會自動重新加入先前指派的群組。</span><span class="sxs-lookup"><span data-stu-id="24b06-127">When reconnecting after a temporary disruption, the user automatically re-joins the previously-assigned groups.</span></span> <span data-ttu-id="24b06-128">自動重新加入群組時，才適用重新連接後，不會在建立新的連接。</span><span class="sxs-lookup"><span data-stu-id="24b06-128">Automatically rejoining a group only applies when reconnecting, not when establishing a new connection.</span></span> <span data-ttu-id="24b06-129">數位簽章的權杖會傳遞從用戶端，其中包含先前指派的群組清單。</span><span class="sxs-lookup"><span data-stu-id="24b06-129">A digitally-signed token is passed from the client that contains the list of previously-assigned groups.</span></span> <span data-ttu-id="24b06-130">如果您想要確認使用者是否屬於所要求的群組，您可以覆寫預設行為。</span><span class="sxs-lookup"><span data-stu-id="24b06-130">If you want to verify whether the user belongs to the requested groups, you can override the default behavior.</span></span>

<span data-ttu-id="24b06-131">本主題包含下列章節：</span><span class="sxs-lookup"><span data-stu-id="24b06-131">This topic includes the following sections:</span></span>

- [<span data-ttu-id="24b06-132">新增和移除使用者</span><span class="sxs-lookup"><span data-stu-id="24b06-132">Adding and removing users</span></span>](#add)
- [<span data-ttu-id="24b06-133">呼叫群組的成員</span><span class="sxs-lookup"><span data-stu-id="24b06-133">Calling members of a group</span></span>](#call)
- [<span data-ttu-id="24b06-134">儲存在資料庫中的群組成員資格</span><span class="sxs-lookup"><span data-stu-id="24b06-134">Storing group membership in a database</span></span>](#storedatabase)
- [<span data-ttu-id="24b06-135">在 Azure 資料表儲存體中儲存的群組成員資格</span><span class="sxs-lookup"><span data-stu-id="24b06-135">Storing group membership in Azure table storage</span></span>](#storeazuretable)
- [<span data-ttu-id="24b06-136">重新連接時驗證群組成員資格</span><span class="sxs-lookup"><span data-stu-id="24b06-136">Verifying group membership when reconnecting</span></span>](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a><span data-ttu-id="24b06-137">新增和移除使用者</span><span class="sxs-lookup"><span data-stu-id="24b06-137">Adding and removing users</span></span>

<span data-ttu-id="24b06-138">若要新增或移除群組中的使用者，請呼叫[新增](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx)或是[移除](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx)方法，並傳入使用者的連線識別碼和群組的名稱，做為參數。</span><span class="sxs-lookup"><span data-stu-id="24b06-138">To add or remove users from a group, you call the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) or [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods, and pass in the user's connection id and group's name as parameters.</span></span> <span data-ttu-id="24b06-139">您不需要以手動方式於連線結束時，從群組移除使用者。</span><span class="sxs-lookup"><span data-stu-id="24b06-139">You do not need to manually remove a user from a group when the connection ends.</span></span>

<span data-ttu-id="24b06-140">下列範例所示`Groups.Add`和`Groups.Remove`用 Hub 方法中的方法。</span><span class="sxs-lookup"><span data-stu-id="24b06-140">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

<span data-ttu-id="24b06-141">`Groups.Add`和`Groups.Remove`方法以非同步方式執行。</span><span class="sxs-lookup"><span data-stu-id="24b06-141">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span>

<span data-ttu-id="24b06-142">如果您想要加入群組中的用戶端，並立即將訊息傳送至用戶端使用群組，您必須確定 Groups.Add 方法完成第一次。</span><span class="sxs-lookup"><span data-stu-id="24b06-142">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the Groups.Add method finishes first.</span></span> <span data-ttu-id="24b06-143">下列程式碼範例顯示如何執行該動作。</span><span class="sxs-lookup"><span data-stu-id="24b06-143">The following code examples show how to do that.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

<span data-ttu-id="24b06-144">一般情況下，您不應該包含`await`呼叫時`Groups.Remove`方法因為您嘗試移除的連線識別碼可能會無法再使用。</span><span class="sxs-lookup"><span data-stu-id="24b06-144">In general, you should not include `await` when calling the `Groups.Remove` method because the connection id that you are trying to remove might no longer be available.</span></span> <span data-ttu-id="24b06-145">在此情況下，`TaskCanceledException`要求逾時之後，就會擲回。如果您的應用程式必須確定使用者已傳送訊息至群組之前移除群組中，您可以加入`await`之前`Groups.Remove`，並接著攔截`TaskCanceledException`可能會擲回的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="24b06-145">In that case, `TaskCanceledException` is thrown after the request times out. If your application must ensure that the user has been removed from the group before sending a message to the group, you can add `await` before `Groups.Remove`, and then catch the `TaskCanceledException` exception that might be thrown.</span></span>

<a id="call"></a>

## <a name="calling-members-of-a-group"></a><span data-ttu-id="24b06-146">呼叫群組的成員</span><span class="sxs-lookup"><span data-stu-id="24b06-146">Calling members of a group</span></span>

<span data-ttu-id="24b06-147">下列範例所示，您可以傳送訊息至所有群組的成員或群組的唯一指定的成員。</span><span class="sxs-lookup"><span data-stu-id="24b06-147">You can send messages to all of the members of a group or only specified members of the group, as shown in the following examples.</span></span>

- <span data-ttu-id="24b06-148">**所有**連接指定的群組中的用戶端。</span><span class="sxs-lookup"><span data-stu-id="24b06-148">**All** connected clients in a specified group.</span></span>

    [!code-css[Main](working-with-groups/samples/sample3.css)]
- <span data-ttu-id="24b06-149">所有已連線的指定群組中的用戶端**除了指定的用戶端**所識別的連接識別碼。</span><span class="sxs-lookup"><span data-stu-id="24b06-149">All connected clients in a specified group **except the specified clients**, identified by connection ID.</span></span>

    [!code-csharp[Main](working-with-groups/samples/sample4.cs)]
- <span data-ttu-id="24b06-150">所有已連線的指定群組中的用戶端**除了呼叫用戶端**。</span><span class="sxs-lookup"><span data-stu-id="24b06-150">All connected clients in a specified group **except the calling client**.</span></span>

    [!code-css[Main](working-with-groups/samples/sample5.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a><span data-ttu-id="24b06-151">儲存在資料庫中的群組成員資格</span><span class="sxs-lookup"><span data-stu-id="24b06-151">Storing group membership in a database</span></span>

<span data-ttu-id="24b06-152">下列範例示範如何保留在資料庫中的群組和使用者資訊。</span><span class="sxs-lookup"><span data-stu-id="24b06-152">The following examples show how to retain group and user information in a database.</span></span> <span data-ttu-id="24b06-153">您可以使用任何的資料存取技術;不過，下列範例會示範如何定義使用 Entity Framework 模型。</span><span class="sxs-lookup"><span data-stu-id="24b06-153">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="24b06-154">這些實體模型會對應至資料庫資料表和欄位。</span><span class="sxs-lookup"><span data-stu-id="24b06-154">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="24b06-155">視您的應用程式需求而定，您的資料結構可以變化相當大。</span><span class="sxs-lookup"><span data-stu-id="24b06-155">Your data structure could vary considerably depending on the requirements of your application.</span></span> <span data-ttu-id="24b06-156">此範例包含名為類別`ConversationRoom`這絕對是唯一的應用程式，可讓使用者加入有關不同的主體，例如運動或處理的交談。</span><span class="sxs-lookup"><span data-stu-id="24b06-156">This example includes a class named `ConversationRoom` which would be unique to an application that enables users to join conversations about different subjects, such as sports or gardening.</span></span> <span data-ttu-id="24b06-157">此範例也包含連接的類別。</span><span class="sxs-lookup"><span data-stu-id="24b06-157">This example also includes a class for the connections.</span></span> <span data-ttu-id="24b06-158">Connection 類別不是絕對必要的追蹤群組成員資格，但通常是強固的解決方案，來追蹤使用者的一部分。</span><span class="sxs-lookup"><span data-stu-id="24b06-158">The connection class is not absolutely required for tracking group membership but is frequently part of robust solution to tracking users.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample6.cs)]

<span data-ttu-id="24b06-159">然後，在中樞中，您可以從資料庫擷取的群組和使用者資訊，而以手動方式將使用者新增至適當的群組。</span><span class="sxs-lookup"><span data-stu-id="24b06-159">Then, in the hub, you can retrieve the group and user information from the database and manually add the user to the appropriate groups.</span></span> <span data-ttu-id="24b06-160">此範例不包含用於追蹤使用者連接的程式碼。</span><span class="sxs-lookup"><span data-stu-id="24b06-160">The example does not include code for tracking the user connections.</span></span> <span data-ttu-id="24b06-161">在此範例中，`await`關鍵字不會套用之前`Groups.Add`因為不會立即傳送訊息給群組的成員。</span><span class="sxs-lookup"><span data-stu-id="24b06-161">In this example, the `await` keyword is not applied before `Groups.Add` because a message is not immediately sent to members of the group.</span></span> <span data-ttu-id="24b06-162">如果您想要傳送訊息至群組的所有成員，立即新增新的成員之後，您會想要套用`await`關鍵字來確定非同步作業已完成。</span><span class="sxs-lookup"><span data-stu-id="24b06-162">If you want to send a message to all members of the group immediately after adding the new member, you would want to apply the `await` keyword to make sure the asynchronous operation has completed.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a><span data-ttu-id="24b06-163">在 Azure 資料表儲存體中儲存的群組成員資格</span><span class="sxs-lookup"><span data-stu-id="24b06-163">Storing group membership in Azure table storage</span></span>

<span data-ttu-id="24b06-164">使用 Azure 資料表儲存體來儲存群組和使用者的資訊是類似於使用的資料庫。</span><span class="sxs-lookup"><span data-stu-id="24b06-164">Using Azure table storage to store group and user information is similar to using a database.</span></span> <span data-ttu-id="24b06-165">下列範例會顯示儲存的使用者名稱和群組名稱的資料表實體。</span><span class="sxs-lookup"><span data-stu-id="24b06-165">The following example shows a table entity that stores the user name and group name.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

<span data-ttu-id="24b06-166">在中樞中，您會擷取指派的群組，當使用者連線。</span><span class="sxs-lookup"><span data-stu-id="24b06-166">In the hub, you retrieve the assigned groups when the user connects.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a><span data-ttu-id="24b06-167">重新連接時驗證群組成員資格</span><span class="sxs-lookup"><span data-stu-id="24b06-167">Verifying group membership when reconnecting</span></span>

<span data-ttu-id="24b06-168">根據預設，SignalR 會自動重新指派使用者給適當的群組從暫時中斷，例如當卸除並重新建立連線逾時之前連線重新連線時。使用者的群組資訊傳入語彙基元，當重新連線，以及該語彙基元經過驗證的伺服器上。</span><span class="sxs-lookup"><span data-stu-id="24b06-168">By default, SignalR automatically re-assigns a user to the appropriate groups when reconnecting from a temporary disruption, such as when a connection is dropped and re-established before the connection times out. The user's group information is passed in a token when reconnecting, and that token is verified on the server.</span></span> <span data-ttu-id="24b06-169">如需重新加入至群組的使用者的驗證程序資訊，請參閱[重新連接時，重新加入群組](../security/introduction-to-security.md#rejoingroup)。</span><span class="sxs-lookup"><span data-stu-id="24b06-169">For information about the verification process for rejoining users to groups, see [Rejoining groups when reconnecting](../security/introduction-to-security.md#rejoingroup).</span></span>

<span data-ttu-id="24b06-170">一般情況下，您應該使用自動重新加入群組重新連接的預設行為。</span><span class="sxs-lookup"><span data-stu-id="24b06-170">In general, you should use the default behavior of automatically rejoining groups on reconnect.</span></span> <span data-ttu-id="24b06-171">SignalR 的群組不適合做為安全性機制，來限制機密資料的存取。</span><span class="sxs-lookup"><span data-stu-id="24b06-171">SignalR groups are not intended as a security mechanism for restricting access to sensitive data.</span></span> <span data-ttu-id="24b06-172">不過，如果您的應用程式必須再三檢查使用者的群組成員資格，重新連接時，您可以覆寫預設行為。</span><span class="sxs-lookup"><span data-stu-id="24b06-172">However, if your application must double-check a user's group membership when reconnecting, you can override the default behavior.</span></span> <span data-ttu-id="24b06-173">變更預設行為可以將負擔加入您的資料庫因為必須擷取使用者的群組成員資格，針對每個重新連線而不只是當使用者連線。</span><span class="sxs-lookup"><span data-stu-id="24b06-173">Changing the default behavior can add a burden to your database because a user's group membership must be retrieved for each reconnection rather than just when the user connects.</span></span>

<span data-ttu-id="24b06-174">如果您必須驗證群組成員資格，在重新連線，請建立新的中樞管線模組會傳回一份指派的群組，如下所示。</span><span class="sxs-lookup"><span data-stu-id="24b06-174">If you must verify group membership on reconnect, create a new hub pipeline module that returns a list of assigned groups, as shown below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

<span data-ttu-id="24b06-175">然後，新增至中樞管線，以反白顯示如下的模組。</span><span class="sxs-lookup"><span data-stu-id="24b06-175">Then, add that module to the hub pipeline, as highlighted below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample11.cs?highlight=4)]
