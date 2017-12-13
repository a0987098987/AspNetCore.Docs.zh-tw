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
<a name="mapping-signalr-users-to-connections-in-signalr-1x"></a>將 SignalR 使用者對應至在 SignalR 連線 1.x
====================
由[Patrick Fletcher](https://github.com/pfletcher)， [Tom FitzMacken](https://github.com/tfitzmac)

> 本主題說明如何保留使用者和其連線的相關資訊。


## <a name="introduction"></a>簡介

每個連接至中樞的用戶端會傳遞的唯一連接識別碼。您可以擷取這個值`Context.ConnectionId`中樞內容屬性。 如果您的應用程式需要使用者對應至連接識別碼，並將保存該對應，您可以使用下列其中一項：

- [記憶體中儲存體](#inmemory)，例如字典
- [SignalR 的每個使用者的群組](#groups)
- [永久、 外部儲存體](#database)，例如資料庫資料表或 Azure 資料表儲存體

本主題顯示這些實作。 您使用`OnConnected`， `OnDisconnected`，和`OnReconnected`方法`Hub`類別來追蹤使用者連線狀態。

您的應用程式的最佳方法取決於：

- 裝載您的應用程式的 web 伺服器數目。
- 是否需要取得的目前連線的使用者清單。
- 是否需要應用程式或伺服器重新啟動時，保存群組和使用者資訊。
- 呼叫外部伺服器的延遲是否發生問題。

下表顯示這些考量適用於哪種方法。

|  | 多部伺服器 | 取得目前已連線的使用者清單 | 將資訊重新啟動後保存 | 最佳效能 |
| --- | --- | --- | --- | --- |
| 記憶體中 |  | ![](mapping-users-to-connections/_static/image1.png) |  | ![](mapping-users-to-connections/_static/image2.png) |
| 單一使用者群組 | ![](mapping-users-to-connections/_static/image3.png) |  |  | ![](mapping-users-to-connections/_static/image4.png) |
| 永久的外部 | ![](mapping-users-to-connections/_static/image5.png) | ![](mapping-users-to-connections/_static/image6.png) | ![](mapping-users-to-connections/_static/image7.png) |  |

<a id="inmemory"></a>

## <a name="in-memory-storage"></a>記憶體中儲存體

下列範例顯示如何保留字典，其中會儲存在記憶體中的連接和使用者資訊。 字典使用`HashSet`來儲存連接識別碼。在任何時間使用者可能會有多個連接至 SignalR 應用程式。 比方說，透過多個裝置或多個瀏覽器索引標籤連線的使用者會有一個以上的連線識別碼。

如果應用程式關閉時，所有資訊都會遺失，但它儵蜪填時使用者重新建立其連線。 記憶體中存放裝置無法運作如果您的環境包含一部以上的 web 伺服器，因為每一部伺服器會有不同的連接集合。

第一個範例會示範管理要連線的使用者對應的類別。 HashSet 的索引鍵會是使用者的名稱。

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

下一個範例示範如何使用與中樞連接對應類別。 類別的執行個體儲存在變數名稱`_connections`。

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a>單一使用者群組

您可以建立每個使用者群組，並再傳送訊息至該群組，當您想要連線到只有該使用者。 每個群組的名稱是使用者的名稱。 如果使用者有多個連線，每個連接識別碼加入至使用者的群組。

您不應手動移除使用者從群組時使用者中斷連接。 由 SignalR 架構會自動執行此動作。

下列範例會示範如何實作單一使用者群組。

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a>永久、 外部儲存體

本主題說明如何使用資料庫或 Azure 資料表儲存體來儲存連接資訊。 這個方法適用於當您有多個 web 伺服器，因為每個 web 伺服器可以與相同的資料儲存機制互動。 如果您的 web 伺服器停止運作或重新啟動應用程式，`OnDisconnected`不會呼叫方法。 因此，很可能您的資料儲存機制會有不再有效的連線識別碼的記錄。 若要清除這些孤立的記錄，您可能想要使建立於在與您的應用程式有關的時間範圍之外的任何連線。 本節中的範例包含的值追蹤時已建立連接，但不是會顯示如何清除舊的記錄，因為您可能想要這樣做，以背景處理程序。

### <a name="database"></a>資料庫

下列範例顯示如何保留在資料庫中的連接和使用者資訊。 您可以使用任何資料存取技術。不過，下列範例會示範如何定義使用 Entity Framework 模型。 這些實體模型對應至資料庫資料表和欄位。 您的資料結構無法變化視您的應用程式的需求而定。

第一個範例示範如何定義可以與多個連接實體相關聯的使用者實體。

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

然後，從中樞，您可以追蹤每個連接的狀態如下所示的程式碼。

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

### <a name="azure-table-storage"></a>Azure 資料表儲存體

下列的 Azure 資料表儲存體範例是類似於資料庫的範例。 它不包含所有您需要開始使用 Azure 資料表儲存體服務的資訊。 如需資訊，請參閱[如何使用資料表儲存體從.NET](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-how-to-use-tables/)。

下列範例示範儲存連接資訊的資料表實體。 它會分割資料依使用者名稱，並識別每個實體的連線識別碼，讓使用者可以隨時都有多個連線。

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

在中樞，您可以追蹤每個使用者連線的狀態。

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]
