---
uid: signalr/overview/guide-to-the-api/mapping-users-to-connections
title: "將 SignalR 使用者對應至連接 |Microsoft 文件"
author: tfitzmac
description: "本主題說明如何保留使用者和其連線的相關資訊。 Patrick Fletcher 協助撰寫這個主題。 本主題中使用的軟體版本..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/30/2014
ms.topic: article
ms.assetid: f80c08b1-3f1f-432c-980c-c7b6edeb31b1
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: c4f95a3b65c57dd7cb7c5c7f1ee09daa17fa9616
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="mapping-signalr-users-to-connections"></a><span data-ttu-id="15bc4-105">將 SignalR 使用者對應至連接</span><span class="sxs-lookup"><span data-stu-id="15bc4-105">Mapping SignalR Users to Connections</span></span>
====================
<span data-ttu-id="15bc4-106">由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="15bc4-106">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="15bc4-107">本主題說明如何保留使用者和其連線的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="15bc4-107">This topic shows how to retain information about users and their connections.</span></span>
> 
> <span data-ttu-id="15bc4-108">Patrick Fletcher 協助撰寫這個主題。</span><span class="sxs-lookup"><span data-stu-id="15bc4-108">Patrick Fletcher helped write this topic.</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="15bc4-109">本主題中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="15bc4-109">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="15bc4-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="15bc4-110">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="15bc4-111">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="15bc4-111">.NET 4.5</span></span>
> - <span data-ttu-id="15bc4-112">SignalR 第 2 版</span><span class="sxs-lookup"><span data-stu-id="15bc4-112">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="15bc4-113">本主題的先前版本</span><span class="sxs-lookup"><span data-stu-id="15bc4-113">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="15bc4-114">如需舊版 SignalR 的資訊，請參閱[SignalR 舊版](../older-versions/index.md)。</span><span class="sxs-lookup"><span data-stu-id="15bc4-114">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="15bc4-115">問題和註解</span><span class="sxs-lookup"><span data-stu-id="15bc4-115">Questions and comments</span></span>
> 
> <span data-ttu-id="15bc4-116">請留下上如何您所喜歡的本教學課程，我們可以改進中將註解放在頁面底部的意見反應。</span><span class="sxs-lookup"><span data-stu-id="15bc4-116">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="15bc4-117">如果您有與本教學課程不直接相關的問題，您可以將它們來公佈[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或[StackOverflow.com](http://stackoverflow.com/)。</span><span class="sxs-lookup"><span data-stu-id="15bc4-117">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="introduction"></a><span data-ttu-id="15bc4-118">簡介</span><span class="sxs-lookup"><span data-stu-id="15bc4-118">Introduction</span></span>

<span data-ttu-id="15bc4-119">每個連接至中樞的用戶端會傳遞的唯一連接識別碼。您可以擷取這個值`Context.ConnectionId`中樞內容屬性。</span><span class="sxs-lookup"><span data-stu-id="15bc4-119">Each client connecting to a hub passes a unique connection id. You can retrieve this value in the `Context.ConnectionId` property of the hub context.</span></span> <span data-ttu-id="15bc4-120">如果您的應用程式需要使用者對應至連接識別碼，並將保存該對應，您可以使用下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="15bc4-120">If your application needs to map a user to the connection id and persist that mapping, you can use one of the following:</span></span>

- [<span data-ttu-id="15bc4-121">使用者識別碼提供者 (SignalR 2)</span><span class="sxs-lookup"><span data-stu-id="15bc4-121">The User ID Provider (SignalR 2)</span></span>](#IUserIdProvider)
- <span data-ttu-id="15bc4-122">[記憶體中儲存體](#inmemory)，例如字典</span><span class="sxs-lookup"><span data-stu-id="15bc4-122">[In-memory storage](#inmemory), such as a dictionary</span></span>
- [<span data-ttu-id="15bc4-123">SignalR 的每個使用者的群組</span><span class="sxs-lookup"><span data-stu-id="15bc4-123">SignalR group for each user</span></span>](#groups)
- <span data-ttu-id="15bc4-124">[永久、 外部儲存體](#database)，例如資料庫資料表或 Azure 資料表儲存體</span><span class="sxs-lookup"><span data-stu-id="15bc4-124">[Permanent, external storage](#database), such as a database table or Azure table storage</span></span>

<span data-ttu-id="15bc4-125">本主題顯示這些實作。</span><span class="sxs-lookup"><span data-stu-id="15bc4-125">Each of these implementations is shown in this topic.</span></span> <span data-ttu-id="15bc4-126">您使用`OnConnected`， `OnDisconnected`，和`OnReconnected`方法`Hub`類別來追蹤使用者連線狀態。</span><span class="sxs-lookup"><span data-stu-id="15bc4-126">You use the `OnConnected`, `OnDisconnected`, and `OnReconnected` methods of the `Hub` class to track the user connection status.</span></span>

<span data-ttu-id="15bc4-127">您的應用程式的最佳方法取決於：</span><span class="sxs-lookup"><span data-stu-id="15bc4-127">The best approach for your application depends on:</span></span>

- <span data-ttu-id="15bc4-128">裝載您的應用程式的 web 伺服器數目。</span><span class="sxs-lookup"><span data-stu-id="15bc4-128">The number of web servers hosting your application.</span></span>
- <span data-ttu-id="15bc4-129">是否需要取得的目前連線的使用者清單。</span><span class="sxs-lookup"><span data-stu-id="15bc4-129">Whether you need to get a list of the currently connected users.</span></span>
- <span data-ttu-id="15bc4-130">是否需要應用程式或伺服器重新啟動時，保存群組和使用者資訊。</span><span class="sxs-lookup"><span data-stu-id="15bc4-130">Whether you need to persist group and user information when the application or server restarts.</span></span>
- <span data-ttu-id="15bc4-131">呼叫外部伺服器的延遲是否發生問題。</span><span class="sxs-lookup"><span data-stu-id="15bc4-131">Whether the latency of calling an external server is an issue.</span></span>

<span data-ttu-id="15bc4-132">下表顯示這些考量適用於哪種方法。</span><span class="sxs-lookup"><span data-stu-id="15bc4-132">The following table shows which approach works for these considerations.</span></span>

|  | <span data-ttu-id="15bc4-133">多部伺服器</span><span class="sxs-lookup"><span data-stu-id="15bc4-133">More than one server</span></span> | <span data-ttu-id="15bc4-134">取得目前已連線的使用者清單</span><span class="sxs-lookup"><span data-stu-id="15bc4-134">Get list of currently connected users</span></span> | <span data-ttu-id="15bc4-135">將資訊重新啟動後保存</span><span class="sxs-lookup"><span data-stu-id="15bc4-135">Persist information after restarts</span></span> | <span data-ttu-id="15bc4-136">最佳效能</span><span class="sxs-lookup"><span data-stu-id="15bc4-136">Optimal performance</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="15bc4-137">使用者識別碼提供者</span><span class="sxs-lookup"><span data-stu-id="15bc4-137">UserID Provider</span></span> | ![](mapping-users-to-connections/_static/image1.png) |  |  | ![](mapping-users-to-connections/_static/image2.png) |
| <span data-ttu-id="15bc4-138">記憶體中</span><span class="sxs-lookup"><span data-stu-id="15bc4-138">In-memory</span></span> |  | ![](mapping-users-to-connections/_static/image3.png) |  | ![](mapping-users-to-connections/_static/image4.png) |
| <span data-ttu-id="15bc4-139">單一使用者群組</span><span class="sxs-lookup"><span data-stu-id="15bc4-139">Single-user groups</span></span> | ![](mapping-users-to-connections/_static/image5.png) |  |  | ![](mapping-users-to-connections/_static/image6.png) |
| <span data-ttu-id="15bc4-140">永久的外部</span><span class="sxs-lookup"><span data-stu-id="15bc4-140">Permanent, external</span></span> | ![](mapping-users-to-connections/_static/image7.png) | ![](mapping-users-to-connections/_static/image8.png) | ![](mapping-users-to-connections/_static/image9.png) |  |

<a id="IUserIdProvider"></a>

## <a name="iuserid-provider"></a><span data-ttu-id="15bc4-141">IUserID 提供者</span><span class="sxs-lookup"><span data-stu-id="15bc4-141">IUserID provider</span></span>

<span data-ttu-id="15bc4-142">這項功能可讓使用者指定的使用者識別碼的是根據透過新的介面 IUserIdProvider IRequest。</span><span class="sxs-lookup"><span data-stu-id="15bc4-142">This feature allows users to specify what the userId is based on an IRequest via a new interface IUserIdProvider.</span></span>

<span data-ttu-id="15bc4-143">**IUserIdProvider**</span><span class="sxs-lookup"><span data-stu-id="15bc4-143">**The IUserIdProvider**</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

<span data-ttu-id="15bc4-144">根據預設，會有的實作會透過使用者的`IPrincipal.Identity.Name`的使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="15bc4-144">By default, there will be an implementation that uses the user's `IPrincipal.Identity.Name` as the user name.</span></span> <span data-ttu-id="15bc4-145">若要變更這種情況，註冊您的實作`IUserIdProvider`與全域主機應用程式啟動時：</span><span class="sxs-lookup"><span data-stu-id="15bc4-145">To change this, register your implementation of `IUserIdProvider` with the global host when your application starts:</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<span data-ttu-id="15bc4-146">從內中樞，您可以將訊息傳送至這些使用者透過下列 API:</span><span class="sxs-lookup"><span data-stu-id="15bc4-146">From within a hub, you'll be able to send messages to these users via the following API:</span></span>

<span data-ttu-id="15bc4-147">**傳送訊息給特定使用者**</span><span class="sxs-lookup"><span data-stu-id="15bc4-147">**Sending a message to a specific user**</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs?highlight=5)]

<a id="inmemory"></a>

## <a name="in-memory-storage"></a><span data-ttu-id="15bc4-148">記憶體中儲存體</span><span class="sxs-lookup"><span data-stu-id="15bc4-148">In-memory storage</span></span>

<span data-ttu-id="15bc4-149">下列範例顯示如何保留字典，其中會儲存在記憶體中的連接和使用者資訊。</span><span class="sxs-lookup"><span data-stu-id="15bc4-149">The following examples show how to retain connection and user information in a dictionary that is stored in memory.</span></span> <span data-ttu-id="15bc4-150">字典使用`HashSet`來儲存連接識別碼。在任何時間使用者可能會有多個連接至 SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="15bc4-150">The dictionary uses a `HashSet` to store the connection id. At any time a user could have more than one connection to the SignalR application.</span></span> <span data-ttu-id="15bc4-151">比方說，透過多個裝置或多個瀏覽器索引標籤連線的使用者會有一個以上的連線識別碼。</span><span class="sxs-lookup"><span data-stu-id="15bc4-151">For example, a user who is connected through multiple devices or more than one browser tab would have more than one connection id.</span></span>

<span data-ttu-id="15bc4-152">如果應用程式關閉時，所有資訊都會遺失，但它  儵蜪填時使用者重新建立其連線。</span><span class="sxs-lookup"><span data-stu-id="15bc4-152">If the application shuts down, all of the information is lost, but it will be re-populated as the users re-establish their connections.</span></span> <span data-ttu-id="15bc4-153">記憶體中存放裝置無法運作如果您的環境包含一部以上的 web 伺服器，因為每一部伺服器會有不同的連接集合。</span><span class="sxs-lookup"><span data-stu-id="15bc4-153">In-memory storage does not work if your environment includes more than one web server because each server would have a separate collection of connections.</span></span>

<span data-ttu-id="15bc4-154">第一個範例會示範管理要連線的使用者對應的類別。</span><span class="sxs-lookup"><span data-stu-id="15bc4-154">The first example shows a class that manages the mapping of users to connections.</span></span> <span data-ttu-id="15bc4-155">HashSet 的索引鍵會是使用者的名稱。</span><span class="sxs-lookup"><span data-stu-id="15bc4-155">The key for the HashSet will be the user's name.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

<span data-ttu-id="15bc4-156">下一個範例示範如何使用與中樞連接對應類別。</span><span class="sxs-lookup"><span data-stu-id="15bc4-156">The next example shows how to use the connection mapping class from a hub.</span></span> <span data-ttu-id="15bc4-157">類別的執行個體儲存在變數名稱`_connections`。</span><span class="sxs-lookup"><span data-stu-id="15bc4-157">The instance of the class is stored in a variable name `_connections`.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a><span data-ttu-id="15bc4-158">單一使用者群組</span><span class="sxs-lookup"><span data-stu-id="15bc4-158">Single-user groups</span></span>

<span data-ttu-id="15bc4-159">您可以建立每個使用者群組，並再傳送訊息至該群組，當您想要連線到只有該使用者。</span><span class="sxs-lookup"><span data-stu-id="15bc4-159">You can create a group for each user, and then send a message to that group when you want to reach only that user.</span></span> <span data-ttu-id="15bc4-160">每個群組的名稱是使用者的名稱。</span><span class="sxs-lookup"><span data-stu-id="15bc4-160">The name of each group is the name of the user.</span></span> <span data-ttu-id="15bc4-161">如果使用者有多個連線，每個連接識別碼加入至使用者的群組。</span><span class="sxs-lookup"><span data-stu-id="15bc4-161">If a user has more than one connection, each connection id is added to the user's group.</span></span>

<span data-ttu-id="15bc4-162">您不應手動移除使用者從群組時使用者中斷連接。</span><span class="sxs-lookup"><span data-stu-id="15bc4-162">You should not manually remove the user from the group when the user disconnects.</span></span> <span data-ttu-id="15bc4-163">由 SignalR 架構會自動執行此動作。</span><span class="sxs-lookup"><span data-stu-id="15bc4-163">This action is automatically performed by the SignalR framework.</span></span>

<span data-ttu-id="15bc4-164">下列範例會示範如何實作單一使用者群組。</span><span class="sxs-lookup"><span data-stu-id="15bc4-164">The following example shows how to implement single-user groups.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a><span data-ttu-id="15bc4-165">永久、 外部儲存體</span><span class="sxs-lookup"><span data-stu-id="15bc4-165">Permanent, external storage</span></span>

<span data-ttu-id="15bc4-166">本主題說明如何使用資料庫或 Azure 資料表儲存體來儲存連接資訊。</span><span class="sxs-lookup"><span data-stu-id="15bc4-166">This topic shows how to use either a database or Azure table storage for storing connection information.</span></span> <span data-ttu-id="15bc4-167">這個方法適用於當您有多個 web 伺服器，因為每個 web 伺服器可以與相同的資料儲存機制互動。</span><span class="sxs-lookup"><span data-stu-id="15bc4-167">This approach works when you have multiple web servers because each web server can interact with the same data repository.</span></span> <span data-ttu-id="15bc4-168">如果您的 web 伺服器停止運作或重新啟動應用程式，`OnDisconnected`不會呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="15bc4-168">If your web servers stop working or the application restarts, the `OnDisconnected` method is not called.</span></span> <span data-ttu-id="15bc4-169">因此，很可能您的資料儲存機制會有不再有效的連線識別碼的記錄。</span><span class="sxs-lookup"><span data-stu-id="15bc4-169">Therefore, it is possible that your data repository will have records for connection ids that are no longer valid.</span></span> <span data-ttu-id="15bc4-170">若要清除這些孤立的記錄，您可能想要使建立於在與您的應用程式有關的時間範圍之外的任何連線。</span><span class="sxs-lookup"><span data-stu-id="15bc4-170">To clean up these orphaned records, you may wish to invalidate any connection that was created outside of a timeframe that is relevant to your application.</span></span> <span data-ttu-id="15bc4-171">本節中的範例包含的值追蹤時已建立連接，但不是會顯示如何清除舊的記錄，因為您可能想要這樣做，以背景處理程序。</span><span class="sxs-lookup"><span data-stu-id="15bc4-171">The examples in this section include a value for tracking when the connection was created, but do not show how to clean up old records because you may want to do that as background process.</span></span>

### <a name="database"></a><span data-ttu-id="15bc4-172">資料庫</span><span class="sxs-lookup"><span data-stu-id="15bc4-172">Database</span></span>

<span data-ttu-id="15bc4-173">下列範例顯示如何保留在資料庫中的連接和使用者資訊。</span><span class="sxs-lookup"><span data-stu-id="15bc4-173">The following examples show how to retain connection and user information in a database.</span></span> <span data-ttu-id="15bc4-174">您可以使用任何資料存取技術。不過，下列範例會示範如何定義使用 Entity Framework 模型。</span><span class="sxs-lookup"><span data-stu-id="15bc4-174">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="15bc4-175">這些實體模型對應至資料庫資料表和欄位。</span><span class="sxs-lookup"><span data-stu-id="15bc4-175">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="15bc4-176">您的資料結構無法變化視您的應用程式的需求而定。</span><span class="sxs-lookup"><span data-stu-id="15bc4-176">Your data structure could vary considerably depending on the requirements of your application.</span></span>

<span data-ttu-id="15bc4-177">第一個範例示範如何定義可以與多個連接實體相關聯的使用者實體。</span><span class="sxs-lookup"><span data-stu-id="15bc4-177">The first example shows how to define a user entity that can be associated with many connection entities.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]

<span data-ttu-id="15bc4-178">然後，從中樞，您可以追蹤每個連接的狀態如下所示的程式碼。</span><span class="sxs-lookup"><span data-stu-id="15bc4-178">Then, from the hub, you can track the state of each connection with the code shown below.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample8.cs)]

<a id="azure"></a>
### <a name="azure-table-storage"></a><span data-ttu-id="15bc4-179">Azure 資料表儲存體</span><span class="sxs-lookup"><span data-stu-id="15bc4-179">Azure table storage</span></span>

<span data-ttu-id="15bc4-180">下列的 Azure 資料表儲存體範例是類似於資料庫的範例。</span><span class="sxs-lookup"><span data-stu-id="15bc4-180">The following Azure table storage example is similar to the database example.</span></span> <span data-ttu-id="15bc4-181">它不包含所有您需要開始使用 Azure 資料表儲存體服務的資訊。</span><span class="sxs-lookup"><span data-stu-id="15bc4-181">It does not include all of the information that you would need to get started with Azure Table Storage Service.</span></span> <span data-ttu-id="15bc4-182">如需資訊，請參閱[如何使用資料表儲存體從.NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/)。</span><span class="sxs-lookup"><span data-stu-id="15bc4-182">For information, see [How to use Table storage from .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span></span>

<span data-ttu-id="15bc4-183">下列範例示範儲存連接資訊的資料表實體。</span><span class="sxs-lookup"><span data-stu-id="15bc4-183">The following example shows a table entity for storing connection information.</span></span> <span data-ttu-id="15bc4-184">它會分割資料依使用者名稱，並識別每個實體的連線識別碼，讓使用者可以隨時都有多個連線。</span><span class="sxs-lookup"><span data-stu-id="15bc4-184">It partitions the data by user name, and identifies each entity by the connection id, so a user can have multiple connections at any time.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample9.cs)]

<span data-ttu-id="15bc4-185">在中樞，您可以追蹤每個使用者連線的狀態。</span><span class="sxs-lookup"><span data-stu-id="15bc4-185">In the hub, you track the status of each user's connection.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample10.cs)]
