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
<a name="working-with-groups-in-signalr"></a>使用 SignalR 中的群組
====================
由[Patrick Fletcher](https://github.com/pfletcher)， [Tom FitzMacken](https://github.com/tfitzmac)

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
> ## <a name="previous-versions-of-this-topic"></a>本主題的先前版本
> 
> 如需舊版 SignalR 的資訊，請參閱[SignalR 舊版](../older-versions/index.md)。
> 
> ## <a name="questions-and-comments"></a>問題和註解
> 
> 請留下上如何您所喜歡的本教學課程，我們可以改進中將註解放在頁面底部的意見反應。 如果您有與本教學課程不直接相關的問題，您可以將它們來公佈[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或[StackOverflow.com](http://stackoverflow.com/)。


## <a name="overview"></a>總覽

SignalR 中的群組提供方法，將訊息廣播至連線的用戶端指定的子集。 群組可以有任意數目的用戶端，並在用戶端可以是任意數目的群組成員。 您不需要明確地建立群組。 作用中，群組會自動建立第一次您 Groups.Add，呼叫中指定其名稱，並從它的成員資格中移除最後一次連接時，就會刪除。 如需使用群組的簡介，請參閱[如何 Hub 類別從管理群組成員資格](hubs-api-guide-server.md#groupsfromhub)集線器 API-伺服器指南中。

沒有任何 API 來取得群組成員資格清單或群組的清單。 SignalR 將訊息傳送至用戶端和 pub/sub 模型，為基礎的群組，伺服器無法維護的群組或群組成員資格清單。 這有助於發揮最大延展性，因為每當您將節點加入至 web 伺服陣列時，必須傳播至新的節點 SignalR 維護任何狀態。

當您將使用者加入群組，使用`Groups.Add`方法，使用者會收到訊息導向到該群組的目前連接的持續時間，但該群組中的使用者的成員並不會超過目前的連接。 如果您想要永久保留群組和群組成員資格的相關資訊，您必須將該資料儲存在儲存機制，例如資料庫或 Azure 資料表儲存體。 然後，使用者連線到您的應用程式中，每次您擷取從儲存機制，使用者所隸屬的群組並手動將該使用者新增至這些群組。

當重新連線暫時中斷後，使用者會自動重新連接先前指派的群組。 自動重新加入群組時，才重新連接後，不會在建立新的連接。 數位簽章的權杖會傳遞從用戶端，包含先前指派的群組清單。 如果您想要確認使用者是否屬於要求的群組，您可以覆寫預設行為。

本主題包含下列章節：

- [加入和移除使用者](#add)
- [呼叫群組成員](#call)
- [儲存在資料庫中的群組成員資格](#storedatabase)
- [Azure 資料表儲存體中儲存的群組成員資格](#storeazuretable)
- [當重新連線時驗證群組成員資格](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a>加入和移除使用者

若要新增或從群組移除使用者，請呼叫[新增](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx)或[移除](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx)方法，並傳入使用者的連線識別碼和群組的名稱做為參數。 您不需要以手動方式於連線結束時，從群組移除使用者。

下列範例所示`Groups.Add`和`Groups.Remove`Hub 方法中使用的方法。

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

`Groups.Add`和`Groups.Remove`方法以非同步方式執行。

如果您想要新增到群組的用戶端，並立即傳送訊息至用戶端使用群組，您必須確定 Groups.Add 方法完成第一次。 下列程式碼範例示範如何執行此作業。

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

一般情況下，您不應該包含`await`呼叫時`Groups.Remove`方法因為您嘗試移除的連線識別碼可能不再可用。 在此情況下，`TaskCanceledException`要求逾時之後，會擲回。如果您的應用程式必須確定使用者已傳送訊息至群組之前，先移除群組中，您可以加入`await`Groups.Remove，並再 catch 之前`TaskCanceledException`可能會擲回的例外狀況。

<a id="call"></a>

## <a name="calling-members-of-a-group"></a>呼叫群組成員

下列範例所示，您可以傳送訊息到所有群組的成員或只有指定的群組成員。

- **所有**連接指定的群組中的用戶端。 

    [!code-css[Main](working-with-groups/samples/sample3.css)]
- 所有連接指定的群組中的用戶端**除了指定的用戶端**所識別的連接識別碼。 

    [!code-csharp[Main](working-with-groups/samples/sample4.cs)]
- 所有連接指定的群組中的用戶端**除非呼叫用戶端**。 

    [!code-css[Main](working-with-groups/samples/sample5.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a>儲存在資料庫中的群組成員資格

下列範例顯示如何保留在資料庫中的群組和使用者資訊。 您可以使用任何資料存取技術。不過，下列範例會示範如何定義使用 Entity Framework 模型。 這些實體模型對應至資料庫資料表和欄位。 您的資料結構無法變化視您的應用程式的需求而定。 這個範例包含名為類別`ConversationRoom`這會是唯一的應用程式可讓使用者加入有關不同的主體，例如運動或處理的交談。 此範例也包含連接的類別。 連接類別並非絕對需要追蹤群組成員資格，但通常是強固的解決方案，來追蹤使用者的一部分。

[!code-csharp[Main](working-with-groups/samples/sample6.cs)]

然後，在中樞，您可以從資料庫擷取的群組和使用者資訊，並以手動方式將使用者新增至適當的群組。 此範例不包含用於追蹤使用者連接的程式碼。 在此範例中，`await`關鍵字不會套用之前`Groups.Add`因為訊息不會立即傳送給群組的成員。 如果您想要將訊息傳送至群組的所有成員，後面加入新的成員，您會想要套用`await`關鍵字來確定非同步作業已完成。

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a>Azure 資料表儲存體中儲存的群組成員資格

使用 Azure 資料表儲存體來儲存群組和使用者資訊是類似於使用資料庫。 下列範例會顯示儲存的使用者名稱和群組名稱在資料表實體。

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

在中樞，您會擷取指派的群組，當使用者連線。

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a>當重新連線時驗證群組成員資格

根據預設，SignalR 會自動重新指派使用者給適當的群組從暫時中斷，例如當卸除並重新建立連接逾時之前連線重新連線時。使用者的群組資訊會傳入語彙基元時重新連線，而且該權杖會驗證伺服器上。 如需重新加入使用者至群組的驗證程序資訊，請參閱[重新連線時重新加入群組](../security/introduction-to-security.md#rejoingroup)。

一般情況下，您應該使用自動重新加入群組 重新連線的預設行為。 SignalR 群組不適合做為限制存取機密資料的安全性機制。 不過，如果您的應用程式必須仔細檢查使用者的群組成員資格，重新連接時，您可以覆寫預設行為。 變更預設行為可以將負擔加入您的資料庫因為每個重新連線，而不是只在使用者連線時，必須先擷取使用者的群組成員資格。

如果您必須確認群組成員資格，在重新連線，請建立新的中樞管線模組會傳回一份指派的群組，如下所示。

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

然後，將該模組加入至中樞管線，以反白顯示下面。

[!code-csharp[Main](working-with-groups/samples/sample11.cs?highlight=4)]
