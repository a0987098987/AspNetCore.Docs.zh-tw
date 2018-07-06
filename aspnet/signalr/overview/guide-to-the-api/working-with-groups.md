---
uid: signalr/overview/guide-to-the-api/working-with-groups
title: 使用 signalr 的群組 |Microsoft Docs
author: pfletcher
description: 本主題描述如何保存中樞 API 使用的群組成員資格資訊。
ms.author: aspnetcontent
ms.date: 06/10/2014
ms.assetid: cd378ecd-3e9e-4236-b902-65916d85a048
msc.legacyurl: /signalr/overview/guide-to-the-api/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: c1df772c19bfa89c1d780d09d56c6bc4a79967c6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806189"
---
<a name="working-with-groups-in-signalr"></a>使用 signalr 的群組
====================
藉由[Patrick Fletcher](https://github.com/pfletcher)， [Tom FitzMacken](https://github.com/tfitzmac)

> 本主題描述如何將使用者新增至群組，並將保存群組成員資格資訊。 
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
> ## <a name="previous-versions-of-this-topic"></a>本主題的上一個版本
> 
> 如需舊版 SignalR 的資訊，請參閱[SignalR 舊版](../older-versions/index.md)。
> 
> ## <a name="questions-and-comments"></a>提出問題或意見
> 
> 您喜歡本教學課程中的方式，和我們可以改善在頁面底部的註解中，歡迎留下意見反應。 如果您有不直接相關的教學課程中的問題，您可以張貼他們[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或是[StackOverflow.com](http://stackoverflow.com/)。


## <a name="overview"></a>總覽

Signalr 的群組提供一種方法將訊息廣播至連線的用戶端的指定子集。 群組可以有任意數目的用戶端，並在用戶端可以是任意數目的群組成員。 您不需要明確建立群組。 作用中，群組會自動建立第一次您 Groups.Add，呼叫中指定其名稱，並從它的成員資格中移除最後一次連接時，就會刪除。 如需使用群組，請參閱 <<c0> [ 如何管理群組成員資格從中樞類別](hubs-api-guide-server.md#groupsfromhub)中樞 API-Server 指南中。

取得群組成員資格清單或群組的清單中沒有任何 API。 SignalR 將訊息傳送至用戶端和 pub/sub 模型，為基礎的群組，伺服器無法維護的群組或群組成員資格清單。 這可協助最大化擴充性，因為每當您將節點加入至 web 伺服陣列時，SignalR 維護任何狀態傳播到新的節點。

當您將使用者加入群組，使用`Groups.Add`方法，使用者會收到訊息導向至該群組中，目前的連接，持續時間，但該群組中的使用者成員資格不會保存超過目前的連接。 如果您想要永久保留群組和群組成員資格的相關資訊，您必須在存放庫，例如資料庫或 Azure 資料表儲存體中儲存該資料。 然後，使用者連線到您的應用程式中，每次您儲存機制中擷取使用者所屬的群組並手動將該使用者新增至這些群組。

當重新連線暫時中斷之後，使用者會自動重新加入先前指派的群組。 自動重新加入群組時，才適用重新連接後，不會在建立新的連接。 數位簽章的權杖會傳遞從用戶端，其中包含先前指派的群組清單。 如果您想要確認使用者是否屬於所要求的群組，您可以覆寫預設行為。

本主題包含下列章節：

- [新增和移除使用者](#add)
- [呼叫群組的成員](#call)
- [儲存在資料庫中的群組成員資格](#storedatabase)
- [在 Azure 資料表儲存體中儲存的群組成員資格](#storeazuretable)
- [重新連接時驗證群組成員資格](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a>新增和移除使用者

若要新增或移除群組中的使用者，請呼叫[新增](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx)或是[移除](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx)方法，並傳入使用者的連線識別碼和群組的名稱，做為參數。 您不需要以手動方式於連線結束時，從群組移除使用者。

下列範例所示`Groups.Add`和`Groups.Remove`用 Hub 方法中的方法。

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

`Groups.Add`和`Groups.Remove`方法以非同步方式執行。

如果您想要加入群組中的用戶端，並立即將訊息傳送至用戶端使用群組，您必須確定 Groups.Add 方法完成第一次。 下列程式碼範例顯示如何執行該動作。

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

一般情況下，您不應該包含`await`呼叫時`Groups.Remove`方法因為您嘗試移除的連線識別碼可能會無法再使用。 在此情況下，`TaskCanceledException`要求逾時之後，就會擲回。如果您的應用程式必須確定使用者已傳送訊息至群組之前移除群組中，您可以加入`await`Groups.Remove，然後 catch 前`TaskCanceledException`可能會擲回的例外狀況。

<a id="call"></a>

## <a name="calling-members-of-a-group"></a>呼叫群組的成員

下列範例所示，您可以傳送訊息至所有群組的成員或群組的唯一指定的成員。

- **所有**連接指定的群組中的用戶端。 

    [!code-css[Main](working-with-groups/samples/sample3.css)]
- 所有已連線的指定群組中的用戶端**除了指定的用戶端**所識別的連接識別碼。 

    [!code-csharp[Main](working-with-groups/samples/sample4.cs)]
- 所有已連線的指定群組中的用戶端**除了呼叫用戶端**。 

    [!code-css[Main](working-with-groups/samples/sample5.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a>儲存在資料庫中的群組成員資格

下列範例示範如何保留在資料庫中的群組和使用者資訊。 您可以使用任何的資料存取技術;不過，下列範例會示範如何定義使用 Entity Framework 模型。 這些實體模型會對應至資料庫資料表和欄位。 視您的應用程式需求而定，您的資料結構可以變化相當大。 此範例包含名為類別`ConversationRoom`這絕對是唯一的應用程式，可讓使用者加入有關不同的主體，例如運動或處理的交談。 此範例也包含連接的類別。 Connection 類別不是絕對必要的追蹤群組成員資格，但通常是強固的解決方案，來追蹤使用者的一部分。

[!code-csharp[Main](working-with-groups/samples/sample6.cs)]

然後，在中樞中，您可以從資料庫擷取的群組和使用者資訊，而以手動方式將使用者新增至適當的群組。 此範例不包含用於追蹤使用者連接的程式碼。 在此範例中，`await`關鍵字不會套用之前`Groups.Add`因為不會立即傳送訊息給群組的成員。 如果您想要傳送訊息至群組的所有成員，立即新增新的成員之後，您會想要套用`await`關鍵字來確定非同步作業已完成。

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a>在 Azure 資料表儲存體中儲存的群組成員資格

使用 Azure 資料表儲存體來儲存群組和使用者的資訊是類似於使用的資料庫。 下列範例會顯示儲存的使用者名稱和群組名稱的資料表實體。

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

在中樞中，您會擷取指派的群組，當使用者連線。

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a>重新連接時驗證群組成員資格

根據預設，SignalR 會自動重新指派使用者給適當的群組從暫時中斷，例如當卸除並重新建立連線逾時之前連線重新連線時。使用者的群組資訊傳入語彙基元，當重新連線，以及該語彙基元經過驗證的伺服器上。 如需重新加入至群組的使用者的驗證程序資訊，請參閱[重新連接時，重新加入群組](../security/introduction-to-security.md#rejoingroup)。

一般情況下，您應該使用自動重新加入群組重新連接的預設行為。 SignalR 的群組不適合做為安全性機制，來限制機密資料的存取。 不過，如果您的應用程式必須再三檢查使用者的群組成員資格，重新連接時，您可以覆寫預設行為。 變更預設行為可以將負擔加入您的資料庫因為必須擷取使用者的群組成員資格，針對每個重新連線而不只是當使用者連線。

如果您必須驗證群組成員資格，在重新連線，請建立新的中樞管線模組會傳回一份指派的群組，如下所示。

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

然後，新增至中樞管線，以反白顯示如下的模組。

[!code-csharp[Main](working-with-groups/samples/sample11.cs?highlight=4)]
