---
uid: signalr/overview/guide-to-the-api/working-with-groups
title: "使用 SignalR 中群組 |Microsoft 文件"
author: pfletcher
description: "本主題描述如何保存與中樞應用程式開發介面的群組成員資格資訊。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: cd378ecd-3e9e-4236-b902-65916d85a048
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: 11f5be1ac4e74b692f0db3daac971a2c9d74a64c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="working-with-groups-in-signalr"></a><span data-ttu-id="4929e-103">使用 SignalR 中的群組</span><span class="sxs-lookup"><span data-stu-id="4929e-103">Working with Groups in SignalR</span></span>
====================
<span data-ttu-id="4929e-104">由[Patrick Fletcher](https://github.com/pfletcher)， [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="4929e-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="4929e-105">本主題描述如何將使用者新增至群組，並將保存群組成員資格資訊。</span><span class="sxs-lookup"><span data-stu-id="4929e-105">This topic describes how to add users to groups and persist group membership information.</span></span> 
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="4929e-106">本主題中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="4929e-106">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="4929e-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="4929e-107">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="4929e-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="4929e-108">.NET 4.5</span></span>
> - <span data-ttu-id="4929e-109">SignalR 第 2 版</span><span class="sxs-lookup"><span data-stu-id="4929e-109">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="4929e-110">本主題的先前版本</span><span class="sxs-lookup"><span data-stu-id="4929e-110">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="4929e-111">如需舊版 SignalR 的資訊，請參閱[SignalR 舊版](../older-versions/index.md)。</span><span class="sxs-lookup"><span data-stu-id="4929e-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="4929e-112">問題和註解</span><span class="sxs-lookup"><span data-stu-id="4929e-112">Questions and comments</span></span>
> 
> <span data-ttu-id="4929e-113">請留下上如何您所喜歡的本教學課程，我們可以改進中將註解放在頁面底部的意見反應。</span><span class="sxs-lookup"><span data-stu-id="4929e-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="4929e-114">如果您有與本教學課程不直接相關的問題，您可以將它們來公佈[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或[StackOverflow.com](http://stackoverflow.com/)。</span><span class="sxs-lookup"><span data-stu-id="4929e-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="4929e-115">總覽</span><span class="sxs-lookup"><span data-stu-id="4929e-115">Overview</span></span>

<span data-ttu-id="4929e-116">SignalR 中的群組提供方法，將訊息廣播至連線的用戶端指定的子集。</span><span class="sxs-lookup"><span data-stu-id="4929e-116">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="4929e-117">群組可以有任意數目的用戶端，並在用戶端可以是任意數目的群組成員。</span><span class="sxs-lookup"><span data-stu-id="4929e-117">A group can have any number of clients, and a client can be a member of any number of groups.</span></span> <span data-ttu-id="4929e-118">您不需要明確地建立群組。</span><span class="sxs-lookup"><span data-stu-id="4929e-118">You don't have to explicitly create groups.</span></span> <span data-ttu-id="4929e-119">作用中，群組會自動建立第一次您 Groups.Add，呼叫中指定其名稱，並從它的成員資格中移除最後一次連接時，就會刪除。</span><span class="sxs-lookup"><span data-stu-id="4929e-119">In effect, a group is automatically created the first time you specify its name in a call to Groups.Add, and it is deleted when you remove the last connection from membership in it.</span></span> <span data-ttu-id="4929e-120">如需使用群組的簡介，請參閱[如何 Hub 類別從管理群組成員資格](hubs-api-guide-server.md#groupsfromhub)集線器 API-伺服器指南中。</span><span class="sxs-lookup"><span data-stu-id="4929e-120">For an introduction to using groups, see [How to manage group membership from the Hub class](hubs-api-guide-server.md#groupsfromhub) in the Hubs API - Server Guide.</span></span>

<span data-ttu-id="4929e-121">沒有任何 API 來取得群組成員資格清單或群組的清單。</span><span class="sxs-lookup"><span data-stu-id="4929e-121">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="4929e-122">SignalR 將訊息傳送至用戶端和 pub/sub 模型，為基礎的群組，伺服器無法維護的群組或群組成員資格清單。</span><span class="sxs-lookup"><span data-stu-id="4929e-122">SignalR sends messages to clients and groups based on a pub/sub model, and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="4929e-123">這有助於發揮最大延展性，因為每當您將節點加入至 web 伺服陣列時，必須傳播至新的節點 SignalR 維護任何狀態。</span><span class="sxs-lookup"><span data-stu-id="4929e-123">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<span data-ttu-id="4929e-124">當您將使用者加入群組，使用`Groups.Add`方法，使用者會收到訊息導向到該群組的目前連接的持續時間，但該群組中的使用者的成員並不會超過目前的連接。</span><span class="sxs-lookup"><span data-stu-id="4929e-124">When you add a user to a group using the `Groups.Add` method, the user receives messages directed to that group for the duration of the current connection, but the user's membership in that group is not persisted beyond the current connection.</span></span> <span data-ttu-id="4929e-125">如果您想要永久保留群組和群組成員資格的相關資訊，您必須將該資料儲存在儲存機制，例如資料庫或 Azure 資料表儲存體。</span><span class="sxs-lookup"><span data-stu-id="4929e-125">If you want to permanently retain information about groups and group membership, you must store that data in a repository such as a database or Azure table storage.</span></span> <span data-ttu-id="4929e-126">然後，使用者連線到您的應用程式中，每次您擷取從儲存機制，使用者所隸屬的群組並手動將該使用者新增至這些群組。</span><span class="sxs-lookup"><span data-stu-id="4929e-126">Then, each time a user connects to your application, you retrieve from the repository which groups the user belongs to, and manually add that user to those groups.</span></span>

<span data-ttu-id="4929e-127">當重新連線暫時中斷後，使用者會自動重新連接先前指派的群組。</span><span class="sxs-lookup"><span data-stu-id="4929e-127">When reconnecting after a temporary disruption, the user automatically re-joins the previously-assigned groups.</span></span> <span data-ttu-id="4929e-128">自動重新加入群組時，才重新連接後，不會在建立新的連接。</span><span class="sxs-lookup"><span data-stu-id="4929e-128">Automatically rejoining a group only applies when reconnecting, not when establishing a new connection.</span></span> <span data-ttu-id="4929e-129">數位簽章的權杖會傳遞從用戶端，包含先前指派的群組清單。</span><span class="sxs-lookup"><span data-stu-id="4929e-129">A digitally-signed token is passed from the client that contains the list of previously-assigned groups.</span></span> <span data-ttu-id="4929e-130">如果您想要確認使用者是否屬於要求的群組，您可以覆寫預設行為。</span><span class="sxs-lookup"><span data-stu-id="4929e-130">If you want to verify whether the user belongs to the requested groups, you can override the default behavior.</span></span>

<span data-ttu-id="4929e-131">本主題包含下列章節：</span><span class="sxs-lookup"><span data-stu-id="4929e-131">This topic includes the following sections:</span></span>

- [<span data-ttu-id="4929e-132">加入和移除使用者</span><span class="sxs-lookup"><span data-stu-id="4929e-132">Adding and removing users</span></span>](#add)
- [<span data-ttu-id="4929e-133">呼叫群組成員</span><span class="sxs-lookup"><span data-stu-id="4929e-133">Calling members of a group</span></span>](#call)
- [<span data-ttu-id="4929e-134">儲存在資料庫中的群組成員資格</span><span class="sxs-lookup"><span data-stu-id="4929e-134">Storing group membership in a database</span></span>](#storedatabase)
- [<span data-ttu-id="4929e-135">Azure 資料表儲存體中儲存的群組成員資格</span><span class="sxs-lookup"><span data-stu-id="4929e-135">Storing group membership in Azure table storage</span></span>](#storeazuretable)
- [<span data-ttu-id="4929e-136">當重新連線時驗證群組成員資格</span><span class="sxs-lookup"><span data-stu-id="4929e-136">Verifying group membership when reconnecting</span></span>](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a><span data-ttu-id="4929e-137">加入和移除使用者</span><span class="sxs-lookup"><span data-stu-id="4929e-137">Adding and removing users</span></span>

<span data-ttu-id="4929e-138">若要新增或從群組移除使用者，請呼叫[新增](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx)或[移除](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx)方法，並傳入使用者的連線識別碼和群組的名稱做為參數。</span><span class="sxs-lookup"><span data-stu-id="4929e-138">To add or remove users from a group, you call the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) or [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods, and pass in the user's connection id and group's name as parameters.</span></span> <span data-ttu-id="4929e-139">您不需要以手動方式於連線結束時，從群組移除使用者。</span><span class="sxs-lookup"><span data-stu-id="4929e-139">You do not need to manually remove a user from a group when the connection ends.</span></span>

<span data-ttu-id="4929e-140">下列範例所示`Groups.Add`和`Groups.Remove`Hub 方法中使用的方法。</span><span class="sxs-lookup"><span data-stu-id="4929e-140">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

<span data-ttu-id="4929e-141">`Groups.Add`和`Groups.Remove`方法以非同步方式執行。</span><span class="sxs-lookup"><span data-stu-id="4929e-141">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span>

<span data-ttu-id="4929e-142">如果您想要新增到群組的用戶端，並立即傳送訊息至用戶端使用群組，您必須確定 Groups.Add 方法完成第一次。</span><span class="sxs-lookup"><span data-stu-id="4929e-142">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the Groups.Add method finishes first.</span></span> <span data-ttu-id="4929e-143">下列程式碼範例示範如何執行此作業。</span><span class="sxs-lookup"><span data-stu-id="4929e-143">The following code examples show how to do that.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

<span data-ttu-id="4929e-144">一般情況下，您不應該包含`await`呼叫時`Groups.Remove`方法因為您嘗試移除的連線識別碼可能不再可用。</span><span class="sxs-lookup"><span data-stu-id="4929e-144">In general, you should not include `await` when calling the `Groups.Remove` method because the connection id that you are trying to remove might no longer be available.</span></span> <span data-ttu-id="4929e-145">在此情況下，`TaskCanceledException`要求逾時之後，會擲回。如果您的應用程式必須確定使用者已傳送訊息至群組之前，先移除群組中，您可以加入`await`Groups.Remove，並再 catch 之前`TaskCanceledException`可能會擲回的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="4929e-145">In that case, `TaskCanceledException` is thrown after the request times out. If your application must ensure that the user has been removed from the group before sending a message to the group, you can add `await` before Groups.Remove, and then catch the `TaskCanceledException` exception that might be thrown.</span></span>

<a id="call"></a>

## <a name="calling-members-of-a-group"></a><span data-ttu-id="4929e-146">呼叫群組成員</span><span class="sxs-lookup"><span data-stu-id="4929e-146">Calling members of a group</span></span>

<span data-ttu-id="4929e-147">下列範例所示，您可以傳送訊息到所有群組的成員或只有指定的群組成員。</span><span class="sxs-lookup"><span data-stu-id="4929e-147">You can send messages to all of the members of a group or only specified members of the group, as shown in the following examples.</span></span>

- <span data-ttu-id="4929e-148">**所有**連接指定的群組中的用戶端。</span><span class="sxs-lookup"><span data-stu-id="4929e-148">**All** connected clients in a specified group.</span></span> 

    [!code-css[Main](working-with-groups/samples/sample3.css)]
- <span data-ttu-id="4929e-149">所有連接指定的群組中的用戶端**除了指定的用戶端**所識別的連接識別碼。</span><span class="sxs-lookup"><span data-stu-id="4929e-149">All connected clients in a specified group **except the specified clients**, identified by connection ID.</span></span> 

    [!code-csharp[Main](working-with-groups/samples/sample4.cs)]
- <span data-ttu-id="4929e-150">所有連接指定的群組中的用戶端**除非呼叫用戶端**。</span><span class="sxs-lookup"><span data-stu-id="4929e-150">All connected clients in a specified group **except the calling client**.</span></span> 

    [!code-css[Main](working-with-groups/samples/sample5.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a><span data-ttu-id="4929e-151">儲存在資料庫中的群組成員資格</span><span class="sxs-lookup"><span data-stu-id="4929e-151">Storing group membership in a database</span></span>

<span data-ttu-id="4929e-152">下列範例顯示如何保留在資料庫中的群組和使用者資訊。</span><span class="sxs-lookup"><span data-stu-id="4929e-152">The following examples show how to retain group and user information in a database.</span></span> <span data-ttu-id="4929e-153">您可以使用任何資料存取技術。不過，下列範例會示範如何定義使用 Entity Framework 模型。</span><span class="sxs-lookup"><span data-stu-id="4929e-153">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="4929e-154">這些實體模型對應至資料庫資料表和欄位。</span><span class="sxs-lookup"><span data-stu-id="4929e-154">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="4929e-155">您的資料結構無法變化視您的應用程式的需求而定。</span><span class="sxs-lookup"><span data-stu-id="4929e-155">Your data structure could vary considerably depending on the requirements of your application.</span></span> <span data-ttu-id="4929e-156">這個範例包含名為類別`ConversationRoom`這會是唯一的應用程式可讓使用者加入有關不同的主體，例如運動或處理的交談。</span><span class="sxs-lookup"><span data-stu-id="4929e-156">This example includes a class named `ConversationRoom` which would be unique to an application that enables users to join conversations about different subjects, such as sports or gardening.</span></span> <span data-ttu-id="4929e-157">此範例也包含連接的類別。</span><span class="sxs-lookup"><span data-stu-id="4929e-157">This example also includes a class for the connections.</span></span> <span data-ttu-id="4929e-158">連接類別並非絕對需要追蹤群組成員資格，但通常是強固的解決方案，來追蹤使用者的一部分。</span><span class="sxs-lookup"><span data-stu-id="4929e-158">The connection class is not absolutely required for tracking group membership but is frequently part of robust solution to tracking users.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample6.cs)]

<span data-ttu-id="4929e-159">然後，在中樞，您可以從資料庫擷取的群組和使用者資訊，並以手動方式將使用者新增至適當的群組。</span><span class="sxs-lookup"><span data-stu-id="4929e-159">Then, in the hub, you can retrieve the group and user information from the database and manually add the user to the appropriate groups.</span></span> <span data-ttu-id="4929e-160">此範例不包含用於追蹤使用者連接的程式碼。</span><span class="sxs-lookup"><span data-stu-id="4929e-160">The example does not include code for tracking the user connections.</span></span> <span data-ttu-id="4929e-161">在此範例中，`await`關鍵字不會套用之前`Groups.Add`因為訊息不會立即傳送給群組的成員。</span><span class="sxs-lookup"><span data-stu-id="4929e-161">In this example, the `await` keyword is not applied before `Groups.Add` because a message is not immediately sent to members of the group.</span></span> <span data-ttu-id="4929e-162">如果您想要將訊息傳送至群組的所有成員，後面加入新的成員，您會想要套用`await`關鍵字來確定非同步作業已完成。</span><span class="sxs-lookup"><span data-stu-id="4929e-162">If you want to send a message to all members of the group immediately after adding the new member, you would want to apply the `await` keyword to make sure the asynchronous operation has completed.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a><span data-ttu-id="4929e-163">Azure 資料表儲存體中儲存的群組成員資格</span><span class="sxs-lookup"><span data-stu-id="4929e-163">Storing group membership in Azure table storage</span></span>

<span data-ttu-id="4929e-164">使用 Azure 資料表儲存體來儲存群組和使用者資訊是類似於使用資料庫。</span><span class="sxs-lookup"><span data-stu-id="4929e-164">Using Azure table storage to store group and user information is similar to using a database.</span></span> <span data-ttu-id="4929e-165">下列範例會顯示儲存的使用者名稱和群組名稱在資料表實體。</span><span class="sxs-lookup"><span data-stu-id="4929e-165">The following example shows a table entity that stores the user name and group name.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

<span data-ttu-id="4929e-166">在中樞，您會擷取指派的群組，當使用者連線。</span><span class="sxs-lookup"><span data-stu-id="4929e-166">In the hub, you retrieve the assigned groups when the user connects.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a><span data-ttu-id="4929e-167">當重新連線時驗證群組成員資格</span><span class="sxs-lookup"><span data-stu-id="4929e-167">Verifying group membership when reconnecting</span></span>

<span data-ttu-id="4929e-168">根據預設，SignalR 會自動重新指派使用者給適當的群組從暫時中斷，例如當卸除並重新建立連接逾時之前連線重新連線時。使用者的群組資訊會傳入語彙基元時重新連線，而且該權杖會驗證伺服器上。</span><span class="sxs-lookup"><span data-stu-id="4929e-168">By default, SignalR automatically re-assigns a user to the appropriate groups when reconnecting from a temporary disruption, such as when a connection is dropped and re-established before the connection times out. The user's group information is passed in a token when reconnecting, and that token is verified on the server.</span></span> <span data-ttu-id="4929e-169">如需重新加入使用者至群組的驗證程序資訊，請參閱[重新連線時重新加入群組](../security/introduction-to-security.md#rejoingroup)。</span><span class="sxs-lookup"><span data-stu-id="4929e-169">For information about the verification process for rejoining users to groups, see [Rejoining groups when reconnecting](../security/introduction-to-security.md#rejoingroup).</span></span>

<span data-ttu-id="4929e-170">一般情況下，您應該使用自動重新加入群組 重新連線的預設行為。</span><span class="sxs-lookup"><span data-stu-id="4929e-170">In general, you should use the default behavior of automatically rejoining groups on reconnect.</span></span> <span data-ttu-id="4929e-171">SignalR 群組不適合做為限制存取機密資料的安全性機制。</span><span class="sxs-lookup"><span data-stu-id="4929e-171">SignalR groups are not intended as a security mechanism for restricting access to sensitive data.</span></span> <span data-ttu-id="4929e-172">不過，如果您的應用程式必須仔細檢查使用者的群組成員資格，重新連接時，您可以覆寫預設行為。</span><span class="sxs-lookup"><span data-stu-id="4929e-172">However, if your application must double-check a user's group membership when reconnecting, you can override the default behavior.</span></span> <span data-ttu-id="4929e-173">變更預設行為可以將負擔加入您的資料庫因為每個重新連線，而不是只在使用者連線時，必須先擷取使用者的群組成員資格。</span><span class="sxs-lookup"><span data-stu-id="4929e-173">Changing the default behavior can add a burden to your database because a user's group membership must be retrieved for each reconnection rather than just when the user connects.</span></span>

<span data-ttu-id="4929e-174">如果您必須確認群組成員資格，在重新連線，請建立新的中樞管線模組會傳回一份指派的群組，如下所示。</span><span class="sxs-lookup"><span data-stu-id="4929e-174">If you must verify group membership on reconnect, create a new hub pipeline module that returns a list of assigned groups, as shown below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

<span data-ttu-id="4929e-175">然後，將該模組加入至中樞管線，以反白顯示下面。</span><span class="sxs-lookup"><span data-stu-id="4929e-175">Then, add that module to the hub pipeline, as highlighted below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample11.cs?highlight=4)]
