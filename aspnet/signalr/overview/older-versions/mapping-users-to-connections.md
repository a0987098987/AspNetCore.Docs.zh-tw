---
uid: signalr/overview/older-versions/mapping-users-to-connections
title: "將 SignalR 使用者對應至在 SignalR 連線 1.x |Microsoft 文件"
author: pfletcher
description: "本主題說明如何保留使用者和其連線的相關資訊。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: ebbc93a8-e6c4-4122-8e0d-3aa42293c747
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: 561c5739c4e8465efeb4b5d1eaf8a196dab8673f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="mapping-signalr-users-to-connections-in-signalr-1x"></a><span data-ttu-id="1c8b0-103">將 SignalR 使用者對應至在 SignalR 連線 1.x</span><span class="sxs-lookup"><span data-stu-id="1c8b0-103">Mapping SignalR Users to Connections in SignalR 1.x</span></span>
====================
<span data-ttu-id="1c8b0-104">由[Patrick Fletcher](https://github.com/pfletcher)， [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="1c8b0-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="1c8b0-105">本主題說明如何保留使用者和其連線的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="1c8b0-105">This topic shows how to retain information about users and their connections.</span></span>


## <a name="introduction"></a><span data-ttu-id="1c8b0-106">簡介</span><span class="sxs-lookup"><span data-stu-id="1c8b0-106">Introduction</span></span>

<span data-ttu-id="1c8b0-107">每個連接至中樞的用戶端會傳遞的唯一連接識別碼。您可以擷取這個值`Context.ConnectionId`中樞內容屬性。</span><span class="sxs-lookup"><span data-stu-id="1c8b0-107">Each client connecting to a hub passes a unique connection id. You can retrieve this value in the `Context.ConnectionId` property of the hub context.</span></span> <span data-ttu-id="1c8b0-108">如果您的應用程式需要使用者對應至連接識別碼，並將保存該對應，您可以使用下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="1c8b0-108">If your application needs to map a user to the connection id and persist that mapping, you can use one of the following:</span></span>

- <span data-ttu-id="1c8b0-109">[記憶體中儲存體](#inmemory)，例如字典</span><span class="sxs-lookup"><span data-stu-id="1c8b0-109">[In-memory storage](#inmemory), such as a dictionary</span></span>
- [<span data-ttu-id="1c8b0-110">SignalR 的每個使用者的群組</span><span class="sxs-lookup"><span data-stu-id="1c8b0-110">SignalR group for each user</span></span>](#groups)
- <span data-ttu-id="1c8b0-111">[永久、 外部儲存體](#database)，例如資料庫資料表或 Azure 資料表儲存體</span><span class="sxs-lookup"><span data-stu-id="1c8b0-111">[Permanent, external storage](#database), such as a database table or Azure table storage</span></span>

<span data-ttu-id="1c8b0-112">本主題顯示這些實作。</span><span class="sxs-lookup"><span data-stu-id="1c8b0-112">Each of these implementations is shown in this topic.</span></span> <span data-ttu-id="1c8b0-113">您使用`OnConnected`， `OnDisconnected`，和`OnReconnected`方法`Hub`類別來追蹤使用者連線狀態。</span><span class="sxs-lookup"><span data-stu-id="1c8b0-113">You use the `OnConnected`, `OnDisconnected`, and `OnReconnected` methods of the `Hub` class to track the user connection status.</span></span>

<span data-ttu-id="1c8b0-114">您的應用程式的最佳方法取決於：</span><span class="sxs-lookup"><span data-stu-id="1c8b0-114">The best approach for your application depends on:</span></span>

- <span data-ttu-id="1c8b0-115">裝載您的應用程式的 web 伺服器數目。</span><span class="sxs-lookup"><span data-stu-id="1c8b0-115">The number of web servers hosting your application.</span></span>
- <span data-ttu-id="1c8b0-116">是否需要取得的目前連線的使用者清單。</span><span class="sxs-lookup"><span data-stu-id="1c8b0-116">Whether you need to get a list of the currently connected users.</span></span>
- <span data-ttu-id="1c8b0-117">是否需要應用程式或伺服器重新啟動時，保存群組和使用者資訊。</span><span class="sxs-lookup"><span data-stu-id="1c8b0-117">Whether you need to persist group and user information when the application or server restarts.</span></span>
- <span data-ttu-id="1c8b0-118">呼叫外部伺服器的延遲是否發生問題。</span><span class="sxs-lookup"><span data-stu-id="1c8b0-118">Whether the latency of calling an external server is an issue.</span></span>

<span data-ttu-id="1c8b0-119">下表顯示這些考量適用於哪種方法。</span><span class="sxs-lookup"><span data-stu-id="1c8b0-119">The following table shows which approach works for these considerations.</span></span>

|  | <span data-ttu-id="1c8b0-120">多部伺服器</span><span class="sxs-lookup"><span data-stu-id="1c8b0-120">More than one server</span></span> | <span data-ttu-id="1c8b0-121">取得目前已連線的使用者清單</span><span class="sxs-lookup"><span data-stu-id="1c8b0-121">Get list of currently connected users</span></span> | <span data-ttu-id="1c8b0-122">將資訊重新啟動後保存</span><span class="sxs-lookup"><span data-stu-id="1c8b0-122">Persist information after restarts</span></span> | <span data-ttu-id="1c8b0-123">最佳效能</span><span class="sxs-lookup"><span data-stu-id="1c8b0-123">Optimal performance</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="1c8b0-124">記憶體中</span><span class="sxs-lookup"><span data-stu-id="1c8b0-124">In-memory</span></span> |  | ![](mapping-users-to-connections/_static/image1.png) |  | ![](mapping-users-to-connections/_static/image2.png) |
| <span data-ttu-id="1c8b0-125">單一使用者群組</span><span class="sxs-lookup"><span data-stu-id="1c8b0-125">Single-user groups</span></span> | ![](mapping-users-to-connections/_static/image3.png) |  |  | ![](mapping-users-to-connections/_static/image4.png) |
| <span data-ttu-id="1c8b0-126">永久的外部</span><span class="sxs-lookup"><span data-stu-id="1c8b0-126">Permanent, external</span></span> | ![](mapping-users-to-connections/_static/image5.png) | ![](mapping-users-to-connections/_static/image6.png) | ![](mapping-users-to-connections/_static/image7.png) |  |

<a id="inmemory"></a>

## <a name="in-memory-storage"></a><span data-ttu-id="1c8b0-127">記憶體中儲存體</span><span class="sxs-lookup"><span data-stu-id="1c8b0-127">In-memory storage</span></span>

<span data-ttu-id="1c8b0-128">下列範例顯示如何保留字典，其中會儲存在記憶體中的連接和使用者資訊。</span><span class="sxs-lookup"><span data-stu-id="1c8b0-128">The following examples show how to retain connection and user information in a dictionary that is stored in memory.</span></span> <span data-ttu-id="1c8b0-129">字典使用`HashSet`來儲存連接識別碼。在任何時間使用者可能會有多個連接至 SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="1c8b0-129">The dictionary uses a `HashSet` to store the connection id. At any time a user could have more than one connection to the SignalR application.</span></span> <span data-ttu-id="1c8b0-130">比方說，透過多個裝置或多個瀏覽器索引標籤連線的使用者會有一個以上的連線識別碼。</span><span class="sxs-lookup"><span data-stu-id="1c8b0-130">For example, a user who is connected through multiple devices or more than one browser tab would have more than one connection id.</span></span>

<span data-ttu-id="1c8b0-131">如果應用程式關閉時，所有資訊都會遺失，但它儵蜪填時使用者重新建立其連線。</span><span class="sxs-lookup"><span data-stu-id="1c8b0-131">If the application shuts down, all of the information is lost, but it will be re-populated as the users re-establish their connections.</span></span> <span data-ttu-id="1c8b0-132">記憶體中存放裝置無法運作如果您的環境包含一部以上的 web 伺服器，因為每一部伺服器會有不同的連接集合。</span><span class="sxs-lookup"><span data-stu-id="1c8b0-132">In-memory storage does not work if your environment includes more than one web server because each server would have a separate collection of connections.</span></span>

<span data-ttu-id="1c8b0-133">第一個範例會示範管理要連線的使用者對應的類別。</span><span class="sxs-lookup"><span data-stu-id="1c8b0-133">The first example shows a class that manages the mapping of users to connections.</span></span> <span data-ttu-id="1c8b0-134">HashSet 的索引鍵會是使用者的名稱。</span><span class="sxs-lookup"><span data-stu-id="1c8b0-134">The key for the HashSet will be the user's name.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

<span data-ttu-id="1c8b0-135">下一個範例示範如何使用與中樞連接對應類別。</span><span class="sxs-lookup"><span data-stu-id="1c8b0-135">The next example shows how to use the connection mapping class from a hub.</span></span> <span data-ttu-id="1c8b0-136">類別的執行個體儲存在變數名稱`_connections`。</span><span class="sxs-lookup"><span data-stu-id="1c8b0-136">The instance of the class is stored in a variable name `_connections`.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a><span data-ttu-id="1c8b0-137">單一使用者群組</span><span class="sxs-lookup"><span data-stu-id="1c8b0-137">Single-user groups</span></span>

<span data-ttu-id="1c8b0-138">您可以建立每個使用者群組，並再傳送訊息至該群組，當您想要連線到只有該使用者。</span><span class="sxs-lookup"><span data-stu-id="1c8b0-138">You can create a group for each user, and then send a message to that group when you want to reach only that user.</span></span> <span data-ttu-id="1c8b0-139">每個群組的名稱是使用者的名稱。</span><span class="sxs-lookup"><span data-stu-id="1c8b0-139">The name of each group is the name of the user.</span></span> <span data-ttu-id="1c8b0-140">如果使用者有多個連線，每個連接識別碼加入至使用者的群組。</span><span class="sxs-lookup"><span data-stu-id="1c8b0-140">If a user has more than one connection, each connection id is added to the user's group.</span></span>

<span data-ttu-id="1c8b0-141">您不應手動移除使用者從群組時使用者中斷連接。</span><span class="sxs-lookup"><span data-stu-id="1c8b0-141">You should not manually remove the user from the group when the user disconnects.</span></span> <span data-ttu-id="1c8b0-142">由 SignalR 架構會自動執行此動作。</span><span class="sxs-lookup"><span data-stu-id="1c8b0-142">This action is automatically performed by the SignalR framework.</span></span>

<span data-ttu-id="1c8b0-143">下列範例會示範如何實作單一使用者群組。</span><span class="sxs-lookup"><span data-stu-id="1c8b0-143">The following example shows how to implement single-user groups.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a><span data-ttu-id="1c8b0-144">永久、 外部儲存體</span><span class="sxs-lookup"><span data-stu-id="1c8b0-144">Permanent, external storage</span></span>

<span data-ttu-id="1c8b0-145">本主題說明如何使用資料庫或 Azure 資料表儲存體來儲存連接資訊。</span><span class="sxs-lookup"><span data-stu-id="1c8b0-145">This topic shows how to use either a database or Azure table storage for storing connection information.</span></span> <span data-ttu-id="1c8b0-146">這個方法適用於當您有多個 web 伺服器，因為每個 web 伺服器可以與相同的資料儲存機制互動。</span><span class="sxs-lookup"><span data-stu-id="1c8b0-146">This approach works when you have multiple web servers because each web server can interact with the same data repository.</span></span> <span data-ttu-id="1c8b0-147">如果您的 web 伺服器停止運作或重新啟動應用程式，`OnDisconnected`不會呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="1c8b0-147">If your web servers stop working or the application restarts, the `OnDisconnected` method is not called.</span></span> <span data-ttu-id="1c8b0-148">因此，很可能您的資料儲存機制會有不再有效的連線識別碼的記錄。</span><span class="sxs-lookup"><span data-stu-id="1c8b0-148">Therefore, it is possible that your data repository will have records for connection ids that are no longer valid.</span></span> <span data-ttu-id="1c8b0-149">若要清除這些孤立的記錄，您可能想要使建立於在與您的應用程式有關的時間範圍之外的任何連線。</span><span class="sxs-lookup"><span data-stu-id="1c8b0-149">To clean up these orphaned records, you may wish to invalidate any connection that was created outside of a timeframe that is relevant to your application.</span></span> <span data-ttu-id="1c8b0-150">本節中的範例包含的值追蹤時已建立連接，但不是會顯示如何清除舊的記錄，因為您可能想要這樣做，以背景處理程序。</span><span class="sxs-lookup"><span data-stu-id="1c8b0-150">The examples in this section include a value for tracking when the connection was created, but do not show how to clean up old records because you may want to do that as background process.</span></span>

### <a name="database"></a><span data-ttu-id="1c8b0-151">資料庫</span><span class="sxs-lookup"><span data-stu-id="1c8b0-151">Database</span></span>

<span data-ttu-id="1c8b0-152">下列範例顯示如何保留在資料庫中的連接和使用者資訊。</span><span class="sxs-lookup"><span data-stu-id="1c8b0-152">The following examples show how to retain connection and user information in a database.</span></span> <span data-ttu-id="1c8b0-153">您可以使用任何資料存取技術。不過，下列範例會示範如何定義使用 Entity Framework 模型。</span><span class="sxs-lookup"><span data-stu-id="1c8b0-153">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="1c8b0-154">這些實體模型對應至資料庫資料表和欄位。</span><span class="sxs-lookup"><span data-stu-id="1c8b0-154">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="1c8b0-155">您的資料結構無法變化視您的應用程式的需求而定。</span><span class="sxs-lookup"><span data-stu-id="1c8b0-155">Your data structure could vary considerably depending on the requirements of your application.</span></span>

<span data-ttu-id="1c8b0-156">第一個範例示範如何定義可以與多個連接實體相關聯的使用者實體。</span><span class="sxs-lookup"><span data-stu-id="1c8b0-156">The first example shows how to define a user entity that can be associated with many connection entities.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

<span data-ttu-id="1c8b0-157">然後，從中樞，您可以追蹤每個連接的狀態如下所示的程式碼。</span><span class="sxs-lookup"><span data-stu-id="1c8b0-157">Then, from the hub, you can track the state of each connection with the code shown below.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

### <a name="azure-table-storage"></a><span data-ttu-id="1c8b0-158">Azure 資料表儲存體</span><span class="sxs-lookup"><span data-stu-id="1c8b0-158">Azure table storage</span></span>

<span data-ttu-id="1c8b0-159">下列的 Azure 資料表儲存體範例是類似於資料庫的範例。</span><span class="sxs-lookup"><span data-stu-id="1c8b0-159">The following Azure table storage example is similar to the database example.</span></span> <span data-ttu-id="1c8b0-160">它不包含所有您需要開始使用 Azure 資料表儲存體服務的資訊。</span><span class="sxs-lookup"><span data-stu-id="1c8b0-160">It does not include all of the information that you would need to get started with Azure Table Storage Service.</span></span> <span data-ttu-id="1c8b0-161">如需資訊，請參閱[如何使用資料表儲存體從.NET](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-how-to-use-tables/)。</span><span class="sxs-lookup"><span data-stu-id="1c8b0-161">For information, see [How to use Table storage from .NET](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-how-to-use-tables/).</span></span>

<span data-ttu-id="1c8b0-162">下列範例示範儲存連接資訊的資料表實體。</span><span class="sxs-lookup"><span data-stu-id="1c8b0-162">The following example shows a table entity for storing connection information.</span></span> <span data-ttu-id="1c8b0-163">它會分割資料依使用者名稱，並識別每個實體的連線識別碼，讓使用者可以隨時都有多個連線。</span><span class="sxs-lookup"><span data-stu-id="1c8b0-163">It partitions the data by user name, and identifies each entity by the connection id, so a user can have multiple connections at any time.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<span data-ttu-id="1c8b0-164">在中樞，您可以追蹤每個使用者連線的狀態。</span><span class="sxs-lookup"><span data-stu-id="1c8b0-164">In the hub, you track the status of each user's connection.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]
