---
uid: signalr/overview/guide-to-the-api/mapping-users-to-connections
title: 將 SignalR 使用者對應至連接 |Microsoft 文件
author: tfitzmac
description: 本主題說明如何保留使用者和其連線的相關資訊。 Patrick Fletcher 協助撰寫這個主題。 本主題中使用的軟體版本...
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
ms.locfileid: "28036410"
---
<a name="mapping-signalr-users-to-connections"></a>將 SignalR 使用者對應至連接
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 本主題說明如何保留使用者和其連線的相關資訊。
> 
> Patrick Fletcher 協助撰寫這個主題。
> 
> ## <a name="software-versions-used-in-this-topic"></a>本主題中使用的軟體版本
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR 第 2 版
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>本主題的先前版本
> 
> 如需舊版 SignalR 的資訊，請參閱[SignalR 舊版](../older-versions/index.md)。
> 
> ## <a name="questions-and-comments"></a>問題和註解
> 
> 請留下上如何您所喜歡的本教學課程，我們可以改進中將註解放在頁面底部的意見反應。 如果您有與本教學課程不直接相關的問題，您可以將它們來公佈[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或[StackOverflow.com](http://stackoverflow.com/)。


## <a name="introduction"></a>簡介

每個連接至中樞的用戶端會傳遞的唯一連接識別碼。您可以擷取這個值`Context.ConnectionId`中樞內容屬性。 如果您的應用程式需要使用者對應至連接識別碼，並將保存該對應，您可以使用下列其中一項：

- [使用者識別碼提供者 (SignalR 2)](#IUserIdProvider)
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
| 使用者識別碼提供者 | ![](mapping-users-to-connections/_static/image1.png) |  |  | ![](mapping-users-to-connections/_static/image2.png) |
| 記憶體中 |  | ![](mapping-users-to-connections/_static/image3.png) |  | ![](mapping-users-to-connections/_static/image4.png) |
| 單一使用者群組 | ![](mapping-users-to-connections/_static/image5.png) |  |  | ![](mapping-users-to-connections/_static/image6.png) |
| 永久的外部 | ![](mapping-users-to-connections/_static/image7.png) | ![](mapping-users-to-connections/_static/image8.png) | ![](mapping-users-to-connections/_static/image9.png) |  |

<a id="IUserIdProvider"></a>

## <a name="iuserid-provider"></a>IUserID 提供者

這項功能可讓使用者指定的使用者識別碼的是根據透過新的介面 IUserIdProvider IRequest。

**IUserIdProvider**

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

根據預設，會有的實作會透過使用者的`IPrincipal.Identity.Name`的使用者名稱。 若要變更這種情況，註冊您的實作`IUserIdProvider`與全域主機應用程式啟動時：

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

從內中樞，您可以將訊息傳送至這些使用者透過下列 API:

**傳送訊息給特定使用者**

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs?highlight=5)]

<a id="inmemory"></a>

## <a name="in-memory-storage"></a>記憶體中儲存體

下列範例顯示如何保留字典，其中會儲存在記憶體中的連接和使用者資訊。 字典使用`HashSet`來儲存連接識別碼。在任何時間使用者可能會有多個連接至 SignalR 應用程式。 比方說，透過多個裝置或多個瀏覽器索引標籤連線的使用者會有一個以上的連線識別碼。

如果應用程式關閉時，所有資訊都會遺失，但它  儵蜪填時使用者重新建立其連線。 記憶體中存放裝置無法運作如果您的環境包含一部以上的 web 伺服器，因為每一部伺服器會有不同的連接集合。

第一個範例會示範管理要連線的使用者對應的類別。 HashSet 的索引鍵會是使用者的名稱。

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

下一個範例示範如何使用與中樞連接對應類別。 類別的執行個體儲存在變數名稱`_connections`。

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a>單一使用者群組

您可以建立每個使用者群組，並再傳送訊息至該群組，當您想要連線到只有該使用者。 每個群組的名稱是使用者的名稱。 如果使用者有多個連線，每個連接識別碼加入至使用者的群組。

您不應手動移除使用者從群組時使用者中斷連接。 由 SignalR 架構會自動執行此動作。

下列範例會示範如何實作單一使用者群組。

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a>永久、 外部儲存體

本主題說明如何使用資料庫或 Azure 資料表儲存體來儲存連接資訊。 這個方法適用於當您有多個 web 伺服器，因為每個 web 伺服器可以與相同的資料儲存機制互動。 如果您的 web 伺服器停止運作或重新啟動應用程式，`OnDisconnected`不會呼叫方法。 因此，很可能您的資料儲存機制會有不再有效的連線識別碼的記錄。 若要清除這些孤立的記錄，您可能想要使建立於在與您的應用程式有關的時間範圍之外的任何連線。 本節中的範例包含的值追蹤時已建立連接，但不是會顯示如何清除舊的記錄，因為您可能想要這樣做，以背景處理程序。

### <a name="database"></a>資料庫

下列範例顯示如何保留在資料庫中的連接和使用者資訊。 您可以使用任何資料存取技術。不過，下列範例會示範如何定義使用 Entity Framework 模型。 這些實體模型對應至資料庫資料表和欄位。 您的資料結構無法變化視您的應用程式的需求而定。

第一個範例示範如何定義可以與多個連接實體相關聯的使用者實體。

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]

然後，從中樞，您可以追蹤每個連接的狀態如下所示的程式碼。

[!code-csharp[Main](mapping-users-to-connections/samples/sample8.cs)]

<a id="azure"></a>
### <a name="azure-table-storage"></a>Azure 資料表儲存體

下列的 Azure 資料表儲存體範例是類似於資料庫的範例。 它不包含所有您需要開始使用 Azure 資料表儲存體服務的資訊。 如需資訊，請參閱[如何使用資料表儲存體從.NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/)。

下列範例示範儲存連接資訊的資料表實體。 它會分割資料依使用者名稱，並識別每個實體的連線識別碼，讓使用者可以隨時都有多個連線。

[!code-csharp[Main](mapping-users-to-connections/samples/sample9.cs)]

在中樞，您可以追蹤每個使用者連線的狀態。

[!code-csharp[Main](mapping-users-to-connections/samples/sample10.cs)]
