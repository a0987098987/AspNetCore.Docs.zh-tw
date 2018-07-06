---
uid: signalr/overview/guide-to-the-api/mapping-users-to-connections
title: 將 SignalR 使用者對應至連線 |Microsoft Docs
author: tfitzmac
description: 本主題說明如何保留使用者和其連線的相關資訊。 Patrick Fletcher 協助撰寫本主題。 本主題中使用的軟體版本...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/30/2014
ms.topic: article
ms.assetid: f80c08b1-3f1f-432c-980c-c7b6edeb31b1
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/guide-to-the-api/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: bd7c0cd9a645ab5b65c5c1446b51ea1646e43799
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391528"
---
<a name="mapping-signalr-users-to-connections"></a><span data-ttu-id="72cbb-105">將 SignalR 使用者對應至連線</span><span class="sxs-lookup"><span data-stu-id="72cbb-105">Mapping SignalR Users to Connections</span></span>
====================
<span data-ttu-id="72cbb-106">藉由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="72cbb-106">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="72cbb-107">本主題說明如何保留使用者和其連線的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="72cbb-107">This topic shows how to retain information about users and their connections.</span></span>
> 
> <span data-ttu-id="72cbb-108">Patrick Fletcher 協助撰寫本主題。</span><span class="sxs-lookup"><span data-stu-id="72cbb-108">Patrick Fletcher helped write this topic.</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="72cbb-109">本主題中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="72cbb-109">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="72cbb-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="72cbb-110">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="72cbb-111">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="72cbb-111">.NET 4.5</span></span>
> - <span data-ttu-id="72cbb-112">SignalR 第 2 版</span><span class="sxs-lookup"><span data-stu-id="72cbb-112">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="72cbb-113">本主題的上一個版本</span><span class="sxs-lookup"><span data-stu-id="72cbb-113">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="72cbb-114">如需舊版 SignalR 的資訊，請參閱[SignalR 舊版](../older-versions/index.md)。</span><span class="sxs-lookup"><span data-stu-id="72cbb-114">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="72cbb-115">提出問題或意見</span><span class="sxs-lookup"><span data-stu-id="72cbb-115">Questions and comments</span></span>
> 
> <span data-ttu-id="72cbb-116">您喜歡本教學課程中的方式，和我們可以改善在頁面底部的註解中，歡迎留下意見反應。</span><span class="sxs-lookup"><span data-stu-id="72cbb-116">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="72cbb-117">如果您有不直接相關的教學課程中的問題，您可以張貼他們[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或是[StackOverflow.com](http://stackoverflow.com/)。</span><span class="sxs-lookup"><span data-stu-id="72cbb-117">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="introduction"></a><span data-ttu-id="72cbb-118">簡介</span><span class="sxs-lookup"><span data-stu-id="72cbb-118">Introduction</span></span>

<span data-ttu-id="72cbb-119">連線到中樞的每個用戶端傳遞唯一連接識別碼。您可以擷取此值`Context.ConnectionId`中樞內容的屬性。</span><span class="sxs-lookup"><span data-stu-id="72cbb-119">Each client connecting to a hub passes a unique connection id. You can retrieve this value in the `Context.ConnectionId` property of the hub context.</span></span> <span data-ttu-id="72cbb-120">如果您的應用程式需要將使用者對應至連接識別碼，並將保存該對應，您可以使用下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="72cbb-120">If your application needs to map a user to the connection id and persist that mapping, you can use one of the following:</span></span>

- [<span data-ttu-id="72cbb-121">使用者 ID 提供者 (SignalR 2)</span><span class="sxs-lookup"><span data-stu-id="72cbb-121">The User ID Provider (SignalR 2)</span></span>](#IUserIdProvider)
- <span data-ttu-id="72cbb-122">[記憶體中儲存](#inmemory)，例如字典</span><span class="sxs-lookup"><span data-stu-id="72cbb-122">[In-memory storage](#inmemory), such as a dictionary</span></span>
- [<span data-ttu-id="72cbb-123">SignalR 的每個使用者的群組</span><span class="sxs-lookup"><span data-stu-id="72cbb-123">SignalR group for each user</span></span>](#groups)
- <span data-ttu-id="72cbb-124">[永久、 外部儲存體](#database)，例如資料庫資料表或 Azure 資料表儲存體</span><span class="sxs-lookup"><span data-stu-id="72cbb-124">[Permanent, external storage](#database), such as a database table or Azure table storage</span></span>

<span data-ttu-id="72cbb-125">這些實作都是以本主題所示。</span><span class="sxs-lookup"><span data-stu-id="72cbb-125">Each of these implementations is shown in this topic.</span></span> <span data-ttu-id="72cbb-126">您使用`OnConnected`， `OnDisconnected`，並`OnReconnected`方法`Hub`類別來追蹤使用者連線狀態。</span><span class="sxs-lookup"><span data-stu-id="72cbb-126">You use the `OnConnected`, `OnDisconnected`, and `OnReconnected` methods of the `Hub` class to track the user connection status.</span></span>

<span data-ttu-id="72cbb-127">最適合您的應用程式的方法取決於：</span><span class="sxs-lookup"><span data-stu-id="72cbb-127">The best approach for your application depends on:</span></span>

- <span data-ttu-id="72cbb-128">裝載您的應用程式的 web 伺服器數目。</span><span class="sxs-lookup"><span data-stu-id="72cbb-128">The number of web servers hosting your application.</span></span>
- <span data-ttu-id="72cbb-129">您是否需要取得一份目前連線的使用者。</span><span class="sxs-lookup"><span data-stu-id="72cbb-129">Whether you need to get a list of the currently connected users.</span></span>
- <span data-ttu-id="72cbb-130">是否要保存應用程式或伺服器重新啟動時的群組和使用者的資訊。</span><span class="sxs-lookup"><span data-stu-id="72cbb-130">Whether you need to persist group and user information when the application or server restarts.</span></span>
- <span data-ttu-id="72cbb-131">呼叫外部伺服器的延遲是否發生問題。</span><span class="sxs-lookup"><span data-stu-id="72cbb-131">Whether the latency of calling an external server is an issue.</span></span>

<span data-ttu-id="72cbb-132">下表說明這些考量適用於哪種方法。</span><span class="sxs-lookup"><span data-stu-id="72cbb-132">The following table shows which approach works for these considerations.</span></span>

|  | <span data-ttu-id="72cbb-133">多部伺服器</span><span class="sxs-lookup"><span data-stu-id="72cbb-133">More than one server</span></span> | <span data-ttu-id="72cbb-134">取得目前連接的使用者清單</span><span class="sxs-lookup"><span data-stu-id="72cbb-134">Get list of currently connected users</span></span> | <span data-ttu-id="72cbb-135">保存資訊重新啟動之後</span><span class="sxs-lookup"><span data-stu-id="72cbb-135">Persist information after restarts</span></span> | <span data-ttu-id="72cbb-136">最佳的效能</span><span class="sxs-lookup"><span data-stu-id="72cbb-136">Optimal performance</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="72cbb-137">使用者識別碼提供者</span><span class="sxs-lookup"><span data-stu-id="72cbb-137">UserID Provider</span></span> | ![](mapping-users-to-connections/_static/image1.png) |  |  | ![](mapping-users-to-connections/_static/image2.png) |
| <span data-ttu-id="72cbb-138">記憶體中</span><span class="sxs-lookup"><span data-stu-id="72cbb-138">In-memory</span></span> |  | ![](mapping-users-to-connections/_static/image3.png) |  | ![](mapping-users-to-connections/_static/image4.png) |
| <span data-ttu-id="72cbb-139">單一使用者群組</span><span class="sxs-lookup"><span data-stu-id="72cbb-139">Single-user groups</span></span> | ![](mapping-users-to-connections/_static/image5.png) |  |  | ![](mapping-users-to-connections/_static/image6.png) |
| <span data-ttu-id="72cbb-140">永久外部</span><span class="sxs-lookup"><span data-stu-id="72cbb-140">Permanent, external</span></span> | ![](mapping-users-to-connections/_static/image7.png) | ![](mapping-users-to-connections/_static/image8.png) | ![](mapping-users-to-connections/_static/image9.png) |  |

<a id="IUserIdProvider"></a>

## <a name="iuserid-provider"></a><span data-ttu-id="72cbb-141">IUserID 提供者</span><span class="sxs-lookup"><span data-stu-id="72cbb-141">IUserID provider</span></span>

<span data-ttu-id="72cbb-142">這項功能可讓使用者指定的使用者識別碼的是根據透過新的介面 IUserIdProvider IRequest。</span><span class="sxs-lookup"><span data-stu-id="72cbb-142">This feature allows users to specify what the userId is based on an IRequest via a new interface IUserIdProvider.</span></span>

<span data-ttu-id="72cbb-143">**IUserIdProvider**</span><span class="sxs-lookup"><span data-stu-id="72cbb-143">**The IUserIdProvider**</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

<span data-ttu-id="72cbb-144">根據預設，會實作，它會使用使用者的`IPrincipal.Identity.Name`作為使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="72cbb-144">By default, there will be an implementation that uses the user's `IPrincipal.Identity.Name` as the user name.</span></span> <span data-ttu-id="72cbb-145">若要變更這種情況，註冊您的實作`IUserIdProvider`與全域主機應用程式啟動時：</span><span class="sxs-lookup"><span data-stu-id="72cbb-145">To change this, register your implementation of `IUserIdProvider` with the global host when your application starts:</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<span data-ttu-id="72cbb-146">從內的中樞，您將能夠將訊息傳送給這些使用者，透過下列 API:</span><span class="sxs-lookup"><span data-stu-id="72cbb-146">From within a hub, you'll be able to send messages to these users via the following API:</span></span>

<span data-ttu-id="72cbb-147">**傳送訊息給特定使用者**</span><span class="sxs-lookup"><span data-stu-id="72cbb-147">**Sending a message to a specific user**</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs?highlight=5)]

<a id="inmemory"></a>

## <a name="in-memory-storage"></a><span data-ttu-id="72cbb-148">記憶體中儲存體</span><span class="sxs-lookup"><span data-stu-id="72cbb-148">In-memory storage</span></span>

<span data-ttu-id="72cbb-149">下列範例示範如何保留儲存在記憶體中的字典中的連線與使用者資訊。</span><span class="sxs-lookup"><span data-stu-id="72cbb-149">The following examples show how to retain connection and user information in a dictionary that is stored in memory.</span></span> <span data-ttu-id="72cbb-150">字典會使用`HashSet`儲存的連接識別碼。在任何時間使用者可能有多個連接至 SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="72cbb-150">The dictionary uses a `HashSet` to store the connection id. At any time a user could have more than one connection to the SignalR application.</span></span> <span data-ttu-id="72cbb-151">比方說，透過多個裝置或多個瀏覽器索引標籤來連線的使用者會有一個以上的連線識別碼。</span><span class="sxs-lookup"><span data-stu-id="72cbb-151">For example, a user who is connected through multiple devices or more than one browser tab would have more than one connection id.</span></span>

<span data-ttu-id="72cbb-152">如果應用程式關閉時，所有資訊都會遺失，但它  儵蜪填，使用者會重新建立其連線。</span><span class="sxs-lookup"><span data-stu-id="72cbb-152">If the application shuts down, all of the information is lost, but it will be re-populated as the users re-establish their connections.</span></span> <span data-ttu-id="72cbb-153">如果您的環境包含多個 web 伺服器，因為每一部伺服器會有不同的連接集合，則記憶體中儲存體即無法運作。</span><span class="sxs-lookup"><span data-stu-id="72cbb-153">In-memory storage does not work if your environment includes more than one web server because each server would have a separate collection of connections.</span></span>

<span data-ttu-id="72cbb-154">第一個範例會示範管理使用者對應至連接的類別。</span><span class="sxs-lookup"><span data-stu-id="72cbb-154">The first example shows a class that manages the mapping of users to connections.</span></span> <span data-ttu-id="72cbb-155">HashSet 的索引鍵會是使用者的名稱。</span><span class="sxs-lookup"><span data-stu-id="72cbb-155">The key for the HashSet will be the user's name.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

<span data-ttu-id="72cbb-156">下一個範例示範如何使用 從中樞的連線對應類別。</span><span class="sxs-lookup"><span data-stu-id="72cbb-156">The next example shows how to use the connection mapping class from a hub.</span></span> <span data-ttu-id="72cbb-157">類別的執行個體儲存在變數名稱`_connections`。</span><span class="sxs-lookup"><span data-stu-id="72cbb-157">The instance of the class is stored in a variable name `_connections`.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a><span data-ttu-id="72cbb-158">單一使用者群組</span><span class="sxs-lookup"><span data-stu-id="72cbb-158">Single-user groups</span></span>

<span data-ttu-id="72cbb-159">您可以建立每個使用者群組，並再傳送訊息至該群組，當您想要連線到只有該使用者。</span><span class="sxs-lookup"><span data-stu-id="72cbb-159">You can create a group for each user, and then send a message to that group when you want to reach only that user.</span></span> <span data-ttu-id="72cbb-160">每個群組的名稱是使用者的名稱。</span><span class="sxs-lookup"><span data-stu-id="72cbb-160">The name of each group is the name of the user.</span></span> <span data-ttu-id="72cbb-161">如果使用者有多個連接，每個連線識別碼會加入至使用者的群組。</span><span class="sxs-lookup"><span data-stu-id="72cbb-161">If a user has more than one connection, each connection id is added to the user's group.</span></span>

<span data-ttu-id="72cbb-162">您不應該手動移除使用者從群組使用者中斷連線時。</span><span class="sxs-lookup"><span data-stu-id="72cbb-162">You should not manually remove the user from the group when the user disconnects.</span></span> <span data-ttu-id="72cbb-163">由 SignalR 架構會自動執行此動作。</span><span class="sxs-lookup"><span data-stu-id="72cbb-163">This action is automatically performed by the SignalR framework.</span></span>

<span data-ttu-id="72cbb-164">下列範例示範如何實作單一使用者群組。</span><span class="sxs-lookup"><span data-stu-id="72cbb-164">The following example shows how to implement single-user groups.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a><span data-ttu-id="72cbb-165">永久、 外部儲存體</span><span class="sxs-lookup"><span data-stu-id="72cbb-165">Permanent, external storage</span></span>

<span data-ttu-id="72cbb-166">本主題說明如何使用資料庫或 Azure 資料表儲存體來儲存連接資訊。</span><span class="sxs-lookup"><span data-stu-id="72cbb-166">This topic shows how to use either a database or Azure table storage for storing connection information.</span></span> <span data-ttu-id="72cbb-167">此方法適用於當您有多部網頁伺服器，因為每個 web 伺服器可以與相同的資料存放庫互動。</span><span class="sxs-lookup"><span data-stu-id="72cbb-167">This approach works when you have multiple web servers because each web server can interact with the same data repository.</span></span> <span data-ttu-id="72cbb-168">如果您的 web 伺服器停止運作或應用程式重新啟動`OnDisconnected`不會呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="72cbb-168">If your web servers stop working or the application restarts, the `OnDisconnected` method is not called.</span></span> <span data-ttu-id="72cbb-169">因此，很可能您的資料儲存機制將會有不再有效的連線識別碼的記錄。</span><span class="sxs-lookup"><span data-stu-id="72cbb-169">Therefore, it is possible that your data repository will have records for connection ids that are no longer valid.</span></span> <span data-ttu-id="72cbb-170">若要清除這些孤立的記錄，您可能想要使其失效建立在與您的應用程式的時間範圍之外的任何連線。</span><span class="sxs-lookup"><span data-stu-id="72cbb-170">To clean up these orphaned records, you may wish to invalidate any connection that was created outside of a timeframe that is relevant to your application.</span></span> <span data-ttu-id="72cbb-171">在本節中的範例包括建立連接時，追蹤的值，但不是會顯示如何清除舊記錄，因為您可能想要這麼做為背景處理程序。</span><span class="sxs-lookup"><span data-stu-id="72cbb-171">The examples in this section include a value for tracking when the connection was created, but do not show how to clean up old records because you may want to do that as background process.</span></span>

### <a name="database"></a><span data-ttu-id="72cbb-172">資料庫</span><span class="sxs-lookup"><span data-stu-id="72cbb-172">Database</span></span>

<span data-ttu-id="72cbb-173">下列範例示範如何保留在資料庫中的連線與使用者資訊。</span><span class="sxs-lookup"><span data-stu-id="72cbb-173">The following examples show how to retain connection and user information in a database.</span></span> <span data-ttu-id="72cbb-174">您可以使用任何的資料存取技術;不過，下列範例會示範如何定義使用 Entity Framework 模型。</span><span class="sxs-lookup"><span data-stu-id="72cbb-174">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="72cbb-175">這些實體模型會對應至資料庫資料表和欄位。</span><span class="sxs-lookup"><span data-stu-id="72cbb-175">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="72cbb-176">視您的應用程式需求而定，您的資料結構可以變化相當大。</span><span class="sxs-lookup"><span data-stu-id="72cbb-176">Your data structure could vary considerably depending on the requirements of your application.</span></span>

<span data-ttu-id="72cbb-177">第一個範例示範如何定義可以與許多連線實體相關聯的使用者實體。</span><span class="sxs-lookup"><span data-stu-id="72cbb-177">The first example shows how to define a user entity that can be associated with many connection entities.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]

<span data-ttu-id="72cbb-178">然後，從中樞 中，您可以追蹤每個連接的狀態如下所示的程式碼。</span><span class="sxs-lookup"><span data-stu-id="72cbb-178">Then, from the hub, you can track the state of each connection with the code shown below.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample8.cs)]

<a id="azure"></a>
### <a name="azure-table-storage"></a><span data-ttu-id="72cbb-179">Azure 資料表儲存體</span><span class="sxs-lookup"><span data-stu-id="72cbb-179">Azure table storage</span></span>

<span data-ttu-id="72cbb-180">下列的 Azure 資料表儲存體範例是類似資料庫範例。</span><span class="sxs-lookup"><span data-stu-id="72cbb-180">The following Azure table storage example is similar to the database example.</span></span> <span data-ttu-id="72cbb-181">它不包含所有您想要開始使用 Azure 資料表儲存體服務的資訊。</span><span class="sxs-lookup"><span data-stu-id="72cbb-181">It does not include all of the information that you would need to get started with Azure Table Storage Service.</span></span> <span data-ttu-id="72cbb-182">如需資訊，請參閱[如何使用.NET 的資料表儲存體](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/)。</span><span class="sxs-lookup"><span data-stu-id="72cbb-182">For information, see [How to use Table storage from .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span></span>

<span data-ttu-id="72cbb-183">下列範例示範儲存連接資訊的資料表實體。</span><span class="sxs-lookup"><span data-stu-id="72cbb-183">The following example shows a table entity for storing connection information.</span></span> <span data-ttu-id="72cbb-184">它的使用者名稱，依分割資料，並識別每個實體的連線識別碼，讓使用者可以在任何時間有多個連線。</span><span class="sxs-lookup"><span data-stu-id="72cbb-184">It partitions the data by user name, and identifies each entity by the connection id, so a user can have multiple connections at any time.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample9.cs)]

<span data-ttu-id="72cbb-185">在中樞中，您可以追蹤每個使用者的連線狀態。</span><span class="sxs-lookup"><span data-stu-id="72cbb-185">In the hub, you track the status of each user's connection.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample10.cs)]
