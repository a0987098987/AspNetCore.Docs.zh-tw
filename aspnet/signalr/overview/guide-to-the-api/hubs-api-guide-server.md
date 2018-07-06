---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-server
title: ASP.NET SignalR 中樞 API 指南-伺服器 (C#) |Microsoft Docs
author: pfletcher
description: 本文件介紹 SignalR 第 2 版的 ASP.NET SignalR 中樞 API 的伺服器端程式設計利用程式碼範例示範...
ms.author: aspnetcontent
ms.date: 06/10/2014
ms.assetid: b19913e5-cd8a-4e4b-a872-5ac7a858a934
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: f036d2bab466a02fdb566593aca8ec0b7d6aa897
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807501"
---
<a name="aspnet-signalr-hubs-api-guide---server-c"></a>ASP.NET SignalR 中樞 API 指南-伺服器 (C#)
====================
藉由[Patrick Fletcher](https://github.com/pfletcher)， [Tom Dykstra](https://github.com/tdykstra)

> 本文件提供簡介 SignalR 第 2 版的 ASP.NET SignalR 中樞 API 的伺服器端程式設計的程式碼範例示範常見的選項。
> 
> SignalR 中樞 API 可讓您從伺服器連線的用戶端和伺服器的用戶端進行遠端程序呼叫 (Rpc)。 在伺服器程式碼中，您定義可由用戶端，呼叫的方法，呼叫用戶端執行的方法。 在用戶端程式碼中，您定義可以在伺服器上，從呼叫的方法，您呼叫在伺服器執行的方法。 SignalR 會處理所有為您的用戶端-伺服器配管。
> 
> SignalR 也提供一個名為持續連線的較低層級 API。 如需 SignalR、 中樞和持續連線，請參閱 < [SignalR 2 簡介](../getting-started/introduction-to-signalr.md)。
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
> ## <a name="topic-versions"></a>主題版本
> 
> 如需舊版 SignalR 的資訊，請參閱[SignalR 舊版](../older-versions/index.md)。
> 
> ## <a name="questions-and-comments"></a>提出問題或意見
> 
> 您喜歡本教學課程中的方式，和我們可以改善在頁面底部的註解中，歡迎留下意見反應。 如果您有不直接相關的教學課程中的問題，您可以張貼他們[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或是[StackOverflow.com](http://stackoverflow.com/)。


## <a name="overview"></a>總覽

本文件包含下列章節：

- [如何註冊 SignalR 中介軟體](#route)

    - [/Signalr URL](#signalrurl)
    - [設定 SignalR 選項](#options)
- [如何建立和使用中樞類別](#hubclass)

    - [中樞物件存留期](#transience)
    - [依照 camel 命名法大小寫的 JavaScript 用戶端中的中樞名稱](#hubnames)
    - [多個中樞](#multiplehubs)
    - [強型別中樞](#stronglytypedhubs)
- [如何在用戶端可以呼叫 Hub 類別中定義方法](#hubmethods)

    - [依照 camel 命名法大小寫的 JavaScript 用戶端中的方法名稱](#methodnames)
    - [以非同步方式執行的時機](#asyncmethods)
    - [定義多載](#overloads)
    - [從中樞方法叫用的報告進度](#progress)
- [如何從中樞類別呼叫用戶端方法](#callfromhub)

    - [選取哪些用戶端會收到 RPC](#selectingclients)
    - [沒有編譯時期的驗證方法的名稱](#dynamicmethodnames)
    - [不區分大小寫的方法名稱比對](#caseinsensitive)
    - [非同步執行](#asyncclient)
- [如何從中樞類別管理群組成員資格](#groupsfromhub)

    - [非同步執行的 Add 和 Remove 方法](#asyncgroupmethods)
    - [群組成員資格持續性](#grouppersistence)
    - [單一使用者群組](#singleusergroups)
- [如何處理中樞類別中的連線存留期事件](#connectionlifetime)

    - [呼叫 OnConnected、 OnDisconnected 和 OnReconnected 的時機](#onreconnected)
    - [不會填入呼叫狀態](#nocallerstate)
- [如何取得用戶端的資訊，從內容屬性](#contextproperty)
- [如何將用戶端與中樞類別之間傳遞狀態](#passstate)
- [如何在中樞類別中處理錯誤](#handleErrors)
- [如何呼叫方法的用戶端和管理中樞類別外的群組](#callfromoutsidehub)

    - [呼叫用戶端方法](#callingclientsoutsidehub)
    - [管理群組成員資格](#managinggroupsoutsidehub)
- [如何啟用追蹤](#tracing)
- [如何自訂中樞管線](#hubpipeline)

如需如何程式用戶端的文件，請參閱下列資源：

- [SignalR 中樞 API 指南-JavaScript 用戶端](hubs-api-guide-javascript-client.md)
- [SignalR 中樞 API 指南-.NET 用戶端](hubs-api-guide-net-client.md)

SignalR 2 的伺服器元件只可在.NET 4.5 中。 執行.NET 4.0 的伺服器必須使用 SignalR v1.x。

<a id="route"></a>

## <a name="how-to-register-signalr-middleware"></a>如何註冊 SignalR 中介軟體

若要定義用戶端將用來連接到您的中樞的路由，請呼叫`MapSignalR`應用程式啟動時的方法。 `MapSignalR` 已[擴充方法](https://msdn.microsoft.com/library/vstudio/bb383977.aspx)如`OwinExtensions`類別。 下列範例示範如何定義使用 OWIN 啟動類別之 SignalR 中樞路由。

[!code-csharp[Main](hubs-api-guide-server/samples/sample1.cs)]

如果您要新增 SignalR 功能至 ASP.NET MVC 應用程式，請確定 SignalR 的路由，會加入其他路由之前。 如需詳細資訊，請參閱 <<c0> [ 教學課程： 開始使用 SignalR 2 和 MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)。

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a>/Signalr URL

根據預設，用戶端將用來連線至中樞的路由 URL 是"/ signalr"。 （請勿混淆這個 URL，使用 「 signalr/中樞 」 的 URL，也就是自動產生的 JavaScript 檔案。 如需產生之 proxy 的詳細資訊，請參閱[SignalR 中樞 API 指南-JavaScript 用戶端-產生的 proxy，它會為您](hubs-api-guide-javascript-client.md#genproxy)。)

可能有異常的情況下，對 SignalR; 無法使用這個基底 URL比方說，您有一個資料夾中名為您的專案*signalr*而且您不想要變更名稱。 在此情況下，您可以變更的基底 URL，如下列範例所示 (取代"/ signalr 」 中的範例程式碼，使用您所需的 URL)。

**指定 URL 的伺服器程式碼**

[!code-csharp[Main](hubs-api-guide-server/samples/sample2.cs?highlight=1)]

**指定的 URL （以產生的 proxy) 的 JavaScript 用戶端程式碼**

[!code-javascript[Main](hubs-api-guide-server/samples/sample3.js?highlight=1)]

**指定的 URL （不含產生的 proxy) 的 JavaScript 用戶端程式碼**

[!code-javascript[Main](hubs-api-guide-server/samples/sample4.js?highlight=1)]

**指定 URL 的.NET 用戶端程式碼**

[!code-csharp[Main](hubs-api-guide-server/samples/sample5.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a>設定 SignalR 選項

多載`MapSignalR`方法可讓您指定自訂 URL、 自訂相依性解析程式，以及下列選項：

- 啟用 CORS 或 JSONP 利用瀏覽器用戶端的跨網域呼叫。

    通常如果瀏覽器載入頁面上，從`http://contoso.com`，SignalR 連線位於相同的網域， `http://contoso.com/signalr`。 如果頁面上，從`http://contoso.com`會連接到`http://fabrikam.com/signalr`，也就是跨網域的連線。 基於安全性理由，預設會停用跨網域的連線。 如需詳細資訊，請參閱 < [ASP.NET SignalR 中樞 API 指南-JavaScript 用戶端-如何建立跨網域連接](hubs-api-guide-javascript-client.md#crossdomain)。
- 啟用詳細的錯誤訊息。

    發生錯誤時，SignalR 的預設行為是傳送給用戶端通知訊息，而不發生了什麼事的詳細資料。 詳細的錯誤資訊傳送至用戶端不建議在生產環境，因為惡意使用者可能可以使用您的應用程式的攻擊中的資訊。 如需疑難排解，您可以使用此選項，暫時啟用 更具參考性的錯誤報告。
- 停用自動產生的 JavaScript proxy 檔案。

    根據預設，具有 proxy 的 JavaScript 檔案，為您的中樞類別會產生以回應 URL 「 signalr/中樞 」。 如果您不想要使用的 JavaScript proxy，或如果您想要以手動方式產生這個檔案，而且參考的實體檔案中您的用戶端，您可以使用此選項以停用 proxy 的產生。 如需詳細資訊，請參閱 < [SignalR 中樞 API 指南-JavaScript 用戶端-如何建立 signalr 的實體檔案產生 proxy](hubs-api-guide-javascript-client.md#manualproxy)。

下列範例示範如何呼叫中指定的 SignalR 連線 URL 和這些選項`MapSignalR`方法。 若要指定自訂 URL，將"/ signalr 」 在範例中，您想要使用的 url。

[!code-csharp[Main](hubs-api-guide-server/samples/sample6.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a>如何建立和使用中樞類別

若要建立中樞時，建立衍生自類別[Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx)。 下列範例顯示的交談應用程式的簡單中樞類別。

[!code-csharp[Main](hubs-api-guide-server/samples/sample7.cs)]

在此範例中，連線的用戶端可以呼叫`NewContosoChatMessage`方法，並收到的資料載入時，會傳播到所有連線的用戶端。

<a id="transience"></a>

### <a name="hub-object-lifetime"></a>中樞物件存留期

您不具現化中樞類別，或從您的伺服器上的程式碼呼叫其方法只是為了由 SignalR 中樞管線。 SignalR 建立您的中樞類別的新執行個體每次需要處理的中樞作業，例如用戶端連線、 中斷連線，或對伺服器提出的方法呼叫的時。

中樞類別的執行個體是暫時性的因為您無法使用它們來維護從到下一個方法呼叫的狀態。 每次伺服器接收的方法呼叫從用戶端 」，這是您的中樞類別程序的新執行個體的訊息。 若要維護透過多條連線和方法呼叫的狀態，使用一些其他方法，例如資料庫或靜態變數 Hub 類別，或不同的類別不是衍生自`Hub`。 如果您將保存在記憶體中的資料時，中樞類別上使用靜態變數之類的方法，資料會遺失應用程式定義域回收時。

如果您想要將訊息傳送至用戶端中，從自己的中樞類別外執行的程式碼，您就無法藉由執行個體化中樞類別的執行個體，但您可以為您的中樞類別取得 SignalR 內容物件的參考來執行。 如需詳細資訊，請參閱 <<c0> [ 如何呼叫方法的用戶端和管理中樞類別外的群組](#callfromoutsidehub)本主題稍後的。

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a>依照 camel 命名法大小寫的 JavaScript 用戶端中的中樞名稱

根據預設，JavaScript 用戶端中樞使用參考的類別名稱的 camel 案例版本。 SignalR 會自動將這項變更，讓 JavaScript 程式碼可以符合 JavaScript 的慣例。 前一個範例就稱為`contosoChatHub`JavaScript 程式碼。

**伺服器**

[!code-csharp[Main](hubs-api-guide-server/samples/sample8.cs?highlight=1)]

**使用產生之 proxy 的 JavaScript 用戶端**

[!code-javascript[Main](hubs-api-guide-server/samples/sample9.js?highlight=1)]

如果您想要指定不同的名稱，讓用戶端使用，請將`HubName`屬性。 當您使用`HubName`屬性中，沒有名稱變更為 JavaScript 用戶端上的 camel 案例。

**伺服器**

[!code-csharp[Main](hubs-api-guide-server/samples/sample10.cs?highlight=1)]

**使用產生之 proxy 的 JavaScript 用戶端**

[!code-javascript[Main](hubs-api-guide-server/samples/sample11.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a>多個中樞

您可以定義多個中樞類別的應用程式中。 當您這樣做，時請連線共用，但是群組是分開：

- 所有用戶端會使用相同的 URL 來建立您的服務與 SignalR 連線 ("/ signalr"或您自訂的 URL，如果您指定一個)，連接適用於所有中樞和服務所定義。

    沒有任何效能差異的多個中樞，相較於單一類別中定義中樞的所有功能。
- 所有中樞都取得相同 HTTP 要求的資訊。

    因為所有中樞都共用相同的連接，伺服器取得的唯一 HTTP 要求資訊是什麼是建立與 SignalR 連線的原始 HTTP 要求中。 如果您使用連線要求到指定的查詢字串從用戶端傳遞資訊給伺服器時，您無法提供不同的查詢字串至不同的中樞。 所有中樞將會都收到相同的資訊。
- 產生的 JavaScript proxy 檔案會包含一個檔案中的所有主機的 proxy。

    JavaScript proxy 的相關資訊，請參閱[SignalR 中樞 API 指南-JavaScript 用戶端-產生的 proxy，它會為您](hubs-api-guide-javascript-client.md#genproxy)。
- 在中樞內定義的群組。

    Signalr 可以定義名為廣播已連線的用戶端子集的群組。 群組會分別維護每個中樞。 例如，一個名為"Administrators"群組會包含用戶端的一組您`ContosoChatHub`類別和相同的群組名稱會參考一組不同的用戶端程式`StockTickerHub`類別。

<a id="stronglytypedhubs"></a>
### <a name="strongly-typed-hubs"></a>強型別中樞

若要定義您的用戶端可以您中樞方法的介面參考 （並在您的中樞方法的啟用 Intellisense），衍生您的中樞，從`Hub<T>`（於 SignalR 2.1 引進） 而非`Hub`:

[!code-csharp[Main](hubs-api-guide-server/samples/sample12.cs)]

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a>如何在用戶端可以呼叫 Hub 類別中定義方法

若要公開您想要從用戶端可呼叫的中樞上的方法，宣告公用的方法，如下列範例所示。

[!code-csharp[Main](hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](hubs-api-guide-server/samples/sample14.cs?highlight=3)]

您可以指定傳回型別和參數，包括複雜型別和陣列，如同在任何 C# 方法。 接收中的參數或傳回給呼叫者的任何資料之間進行通信時用戶端和伺服器使用 JSON 與 SignalR 複雜物件的繫結和物件的陣列會自動處理。

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a>依照 camel 命名法大小寫的 JavaScript 用戶端中的方法名稱

根據預設，JavaScript 用戶端中樞方法使用參考的方法名稱的 camel 案例版本。 SignalR 會自動將這項變更，讓 JavaScript 程式碼可以符合 JavaScript 的慣例。

**伺服器**

[!code-csharp[Main](hubs-api-guide-server/samples/sample15.cs?highlight=1)]

**使用產生之 proxy 的 JavaScript 用戶端**

[!code-javascript[Main](hubs-api-guide-server/samples/sample16.js?highlight=1)]

如果您想要指定不同的名稱，讓用戶端使用，請將`HubMethodName`屬性。

**伺服器**

[!code-csharp[Main](hubs-api-guide-server/samples/sample17.cs?highlight=1)]

**使用產生之 proxy 的 JavaScript 用戶端**

[!code-javascript[Main](hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a>以非同步方式執行的時機

如果方法將會是長時間執行，或者要執行的作業會涉及插撥功能，例如資料庫尋查 」 或 「 web 服務呼叫，請讓中樞方法所傳回的非同步[任務](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)(取代`void`傳回) 或[任務&lt;T&gt; ](https://msdn.microsoft.com/library/dd321424.aspx)物件 (取代`T`傳回型別)。 當您恢復`Task`從 SignalR 方法的物件會等待`Task`，才能完成，然後它會傳送未包裝的結果傳回給用戶端，因此沒有任何差異，在您程式碼中用戶端的方法呼叫的方式。

讓中樞方法非同步可避免使用 WebSocket 傳輸時，封鎖連接。 當中樞方法以同步方式執行，而且傳輸 WebSocket 時上從相同的用戶端中樞, 方法的後續引動過程會遭到封鎖，直到完成中樞方法。

下列範例顯示相同的方法以同步方式執行自動程式化，或以非同步的方式，後面接著呼叫其中一個版本的運作方式的 JavaScript 用戶端程式碼。

**同步**

[!code-csharp[Main](hubs-api-guide-server/samples/sample19.cs)]

**非同步**

[!code-csharp[Main](hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

**使用產生之 proxy 的 JavaScript 用戶端**

[!code-javascript[Main](hubs-api-guide-server/samples/sample21.js)]

如需如何使用 ASP.NET 4.5 中的非同步方法的詳細資訊，請參閱[ASP.NET MVC 4 中使用非同步方法](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md)。

<a id="overloads"></a>

### <a name="defining-overloads"></a>定義多載

如果您想要定義多載方法，在每個多載的參數數目必須不同。 如果您只要指定不同的參數類型區分的多載，將會編譯您的中樞類別，但 SignalR 服務將會擲回的例外狀況，在執行階段，當用戶端嘗試呼叫其中一個多載。

<a id="progress"></a>
### <a name="reporting-progress-from-hub-method-invocations"></a>從中樞方法叫用的報告進度

SignalR 2.1 新增的支援[進度報告模式](https://blogs.msdn.com/b/dotnet/archive/2012/06/06/async-in-4-5-enabling-progress-and-cancellation-in-async-apis.aspx).NET 4.5 中引進。 若要實作進度報告，請定義`IProgress<T>`可存取您的用戶端中樞方法的參數：

[!code-csharp[Main](hubs-api-guide-server/samples/sample22.cs)]

在撰寫長時間執行的伺服器方法時，務必要使用非同步程式設計模式，例如非同步 / 等候而不是封鎖中樞執行緒。

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a>如何從中樞類別呼叫用戶端方法

若要從伺服器呼叫用戶端方法，請使用`Clients`中樞類別中的方法中的屬性。 下列範例示範伺服端程式碼呼叫`addNewMessageToPage`上所有已連線的用戶端，並定義的方法在 JavaScript 用戶端的用戶端程式碼。

**伺服器**

[!code-csharp[Main](hubs-api-guide-server/samples/sample23.cs?highlight=5)]

**使用產生之 proxy 的 JavaScript 用戶端**

[!code-html[Main](hubs-api-guide-server/samples/sample24.html?highlight=1)]

您無法從用戶端方法，取得傳回值這類的語法`int x = Clients.All.add(1,1)`無法運作。

您可以指定複雜型別和參數的陣列。 下列範例會將複雜型別傳遞至方法參數的用戶端。

**呼叫用戶端方法，使用複雜物件的伺服器程式碼**

[!code-csharp[Main](hubs-api-guide-server/samples/sample25.cs?highlight=3)]

**定義的複雜物件的伺服器程式碼**

[!code-csharp[Main](hubs-api-guide-server/samples/sample26.cs?highlight=1)]

**使用產生之 proxy 的 JavaScript 用戶端**

[!code-javascript[Main](hubs-api-guide-server/samples/sample27.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a>選取哪些用戶端會收到 RPC

用戶端屬性會傳回[HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx)提供數個選項，來指定哪些用戶端會收到 RPC 的物件：

- 所有已連線的用戶端。

    [!code-csharp[Main](hubs-api-guide-server/samples/sample28.cs)]
- 只有呼叫的用戶端。

    [!code-csharp[Main](hubs-api-guide-server/samples/sample29.cs)]
- 除了呼叫用戶端的所有用戶端。

    [!code-csharp[Main](hubs-api-guide-server/samples/sample30.cs)]
- 連接識別碼所識別的特定用戶端

    [!code-css[Main](hubs-api-guide-server/samples/sample31.css)]

    這個範例會呼叫`addContosoChatMessageToPage`呼叫的用戶端上且具有相同的效果與使用`Clients.Caller`。
- 所有已連線的用戶端，除了指定的用戶端，識別連接識別碼。

    [!code-csharp[Main](hubs-api-guide-server/samples/sample32.cs)]
- 指定群組中所有已連線的用戶端。

    [!code-css[Main](hubs-api-guide-server/samples/sample33.css)]
- 指定群組中所有連線的用戶端除了指定的用戶端，識別連接識別碼。

    [!code-csharp[Main](hubs-api-guide-server/samples/sample34.cs)]
- 指定群組中所有連線的用戶端除了呼叫用戶端。

    [!code-css[Main](hubs-api-guide-server/samples/sample35.css)]
- UserId 所識別特定使用者。

    [!code-csharp[Main](hubs-api-guide-server/samples/sample36.cs)]

    根據預設，這是`IPrincipal.Identity.Name`，但可以變更此[IUserIdProvider 實作向全域主機](mapping-users-to-connections.md#IUserIdProvider)。
- 所有的用戶端與之連線識別碼的清單中的群組。

    [!code-css[Main](hubs-api-guide-server/samples/sample37.css)]
- 群組的清單。

    [!code-css[Main](hubs-api-guide-server/samples/sample38.css)]
- 依名稱的使用者。

    [!code-csharp[Main](hubs-api-guide-server/samples/sample39.cs)]
- （於 SignalR 2.1 引進） 的使用者名稱的清單。

    [!code-csharp[Main](hubs-api-guide-server/samples/sample40.cs)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a>沒有編譯時期的驗證方法的名稱

您指定的方法名稱會解譯為動態物件，這表示沒有任何 IntelliSense 或它的編譯時間驗證。 在執行階段會評估運算式。 當方法呼叫執行時，SignalR 將方法名稱和參數值傳送至用戶端，且如果用戶端有一個方法符合名稱、 呼叫的方法和參數值傳遞給它。 如果用戶端上找不到任何相符的方法，不會引發錯誤。 SignalR 傳輸至用戶端在幕後，當您呼叫的用戶端方法的資料格式的相關資訊，請參閱[SignalR 簡介](../getting-started/introduction-to-signalr.md)。

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a>不區分大小寫的方法名稱比對

方法名稱比對不區分大小寫。 例如，`Clients.All.addContosoChatMessageToPage`伺服器上將會執行`AddContosoChatMessageToPage`， `addcontosochatmessagetopage`，或`addContosoChatMessageToPage`用戶端上。

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a>非同步執行

您呼叫的方法以非同步方式執行。 來自用戶端的方法呼叫會立即執行，而不需等待 SignalR 完成傳輸資料到用戶端，除非您另外指定，接下來的幾行程式碼之後的任何程式碼應該等候方法完成。 下列程式碼範例示範如何以循序方式執行兩個用戶端的方法。

**使用 Await (.NET 4.5)**

[!code-csharp[Main](hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

如果您使用`await`等候，直到用戶端方法完成執行下的一行程式碼之前，這不表示用戶端會實際收到的訊息下的一行程式碼執行之前。 用戶端方法呼叫的 「 完成 」 只表示 SignalR 已完成傳送訊息所需的所有項目。 如果您需要驗證用戶端收到訊息時，您必須自行進行程式設計的機制。 例如，您可以撰寫`MessageReceived`方法，在中樞，並在`addContosoChatMessageToPage`方法，您可以呼叫用戶端上的`MessageReceived`這麼做之後，任何正常運作，您需要在用戶端上。 在 `MessageReceived`中樞中您可以執行任何工作取決於實際的用戶端接收和處理的原始方法呼叫。

### <a name="how-to-use-a-string-variable-as-the-method-name"></a>如何使用做為方法名稱的字串變數

如果您想要使用的字串變數的方法名稱，轉換為叫用用戶端方法`Clients.All`(或`Clients.Others`，`Clients.Caller`等等) 要`IClientProxy`，然後呼叫[叫用 （methodName、 args...）](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).

[!code-csharp[Main](hubs-api-guide-server/samples/sample42.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a>如何從中樞類別管理群組成員資格

Signalr 的群組提供一種方法將訊息廣播至連線的用戶端的指定子集。 群組可以有任意數目的用戶端，並在用戶端可以是任意數目的群組成員。

若要管理群組成員資格，請使用[新增](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx)並[移除](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx)所提供的方法`Groups`中樞類別的屬性。 下列範例所示`Groups.Add`和`Groups.Remove`用中樞方法呼叫的用戶端程式碼，在方法後面 JavaScript 用戶端程式碼呼叫它們。

**伺服器**

[!code-csharp[Main](hubs-api-guide-server/samples/sample43.cs?highlight=5,10)]

**使用產生之 proxy 的 JavaScript 用戶端**

[!code-javascript[Main](hubs-api-guide-server/samples/sample44.js)]

[!code-javascript[Main](hubs-api-guide-server/samples/sample45.js)]

您不需要明確建立群組。 作用中的群組會自動建立第一次呼叫中指定其名稱`Groups.Add`，和它的成員資格中移除最後一次連接時，它會刪除。

取得群組成員資格清單或群組的清單中沒有任何 API。 SignalR 將訊息傳送至用戶端與群組依據[pub/sub 模型](http://en.wikipedia.org/wiki/Publish/subscribe)，和伺服器不會維護的群組或群組成員資格清單。 這可協助最大化擴充性，因為每當您將節點加入至 web 伺服陣列時，SignalR 維護任何狀態傳播到新的節點。

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a>非同步執行的 Add 和 Remove 方法

`Groups.Add`和`Groups.Remove`方法以非同步方式執行。 如果您想要加入群組中的用戶端，並立即將訊息傳送至用戶端使用群組，您必須確定`Groups.Add`方法完成第一次。 下列程式碼範例顯示如何執行該動作。

**加入群組中的用戶端，然後傳訊用戶端**

[!code-csharp[Main](hubs-api-guide-server/samples/sample46.cs?highlight=1,3)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a>群組成員資格持續性

SignalR 追蹤連線，而不是使用者，因此如果您希望使用者是相同群組中每次使用者建立的連接，您必須呼叫`Groups.Add`每次使用者建立新的連接。

之後的連線暫時中斷，有時候 SignalR 可以連線自動還原。 在此情況下，SignalR 還原相同的連線，不建立新的連接，並因此會自動還原用戶端的群組成員資格。 這可能是甚至暫時中斷時的結果伺服器重新啟動或失敗，因為每個用戶端，包括群組成員資格的連接狀態會是往返用戶端。 如果在伺服器故障，而被新的伺服器連線逾時之前，用戶端可以自動重新連線至新的伺服器，並重新註冊的群組的成員。

當連線無法自動還原後失去連線，或當連線逾時，或用戶端中斷連線 （例如，當瀏覽器瀏覽至新頁面），群組成員資格將會遺失。 下次使用者連線會是新的連接。 若要維護群組成員資格，相同的使用者建立新的連接時，您的應用程式必須進行追蹤、 使用者和群組之間的關聯和還原群組成員資格的每當使用者建立新的連接。

如需連線和重新連線的詳細資訊，請參閱[如何處理連線存留期事件中樞類別](#connectionlifetime)本主題稍後的。

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a>單一使用者群組

通常使用 SignalR 的應用程式需要追蹤的使用者與連線之間的關聯，才能知道哪位使用者已傳送訊息，以及哪些使用者應接收訊息。 其中兩個常用的模式中使用群組來達成目的。

- 單一使用者群組。

    您可以指定的使用者名稱與群組名稱，並將目前的連線識別碼新增至群組，每次使用者連線或重新連線。 若要將訊息傳送給您傳送給群組的使用者。 這個方法的缺點是群組不提供您了解使用者在線上或離線的方式。
- 追蹤使用者名稱和連線識別碼之間的關聯。

    您可以在字典或資料庫中儲存每個使用者名稱與一或多個連線識別碼之間的關聯，並更新儲存的資料每次使用者連線或中斷連線。 若要傳送訊息給使用者，您可以指定連線識別碼。 這個方法的缺點是它會耗用更多的記憶體。

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a>如何處理中樞類別中的連線存留期事件

處理連線存留期事件的常見原因是為了追蹤是否已連線，以及追蹤的使用者名稱和連線識別碼之間的關聯。 若要執行自己的程式碼，用戶端連線或中斷連線時，覆寫`OnConnected`， `OnDisconnected`，和`OnReconnected`類別虛擬方法的中樞，如下列範例所示。

[!code-csharp[Main](hubs-api-guide-server/samples/sample47.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a>呼叫 OnConnected、 OnDisconnected 和 OnReconnected 的時機

瀏覽器巡覽至新的頁面上，每次新的連線已建立，這表示將會執行 SignalR`OnDisconnected`方法，後面`OnConnected`方法。 建立新的連線時，SignalR 一律會建立新的連線識別碼。

`OnReconnected`發生時暫時中斷 SignalR 可以自動復原，例如當暫時中斷連線且連線逾時之前，重新連接纜線的連線，會呼叫方法。`OnDisconnected`用戶端中斷連線和 SignalR 無法自動重新連線，例如當瀏覽器導覽至新頁面時，會呼叫方法。 因此，可以指定用戶端的事件序列是`OnConnected`， `OnReconnected`， `OnDisconnected`; 或是`OnConnected`， `OnDisconnected`。 您不會看到序列`OnConnected`， `OnDisconnected`，`OnReconnected`指定的連接。

`OnDisconnected`方法不會呼叫在某些情況下，例如當伺服器關閉時或應用程式定義域取得回收。 當另一部伺服器上線時或在應用程式網域完成其回收時，某些用戶端可以重新連線，並引發`OnReconnected`事件。

如需詳細資訊，請參閱 <<c0> [ 了解和處理連線存留期事件 SignalR](handling-connection-lifetime-events.md)。

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a>不會填入呼叫狀態

連接的存留期事件處理常式方法會呼叫從伺服器上，這表示您將放在任何狀態`state`用戶端上的物件將不會填入`Caller`伺服器上的屬性。 如需`state`物件和`Caller`屬性，請參閱[如何在用戶端與中樞類別之間傳遞狀態](#passstate)本主題稍後的。

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a>如何取得用戶端的資訊，從內容屬性

若要取得用戶端的相關資訊，請使用`Context`中樞類別的屬性。 `Context`屬性會傳回[HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx)物件可讓您存取下列資訊：

- 呼叫用戶端連接識別碼。

    [!code-csharp[Main](hubs-api-guide-server/samples/sample48.cs?highlight=1)]

    連線識別碼是由 SignalR （您無法在自己的程式碼中指定的值） 指派的 GUID。 還有一個針對每個連線和相同的連線識別碼由所有中樞，如果您的應用程式中有多個中樞的連線識別碼。
- HTTP 標頭資料。

    [!code-csharp[Main](hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    您也可以取得 HTTP 標頭從`Context.Headers`。 多個參考相同的原因在於`Context.Headers`首先，建立`Context.Request`已更新版本中，加入屬性和`Context.Headers`保留回溯相容性。
- 查詢字串的資料。

    [!code-csharp[Main](hubs-api-guide-server/samples/sample50.cs?highlight=1)]

    您也可以取得查詢字串資料從`Context.QueryString`。

    您在這個屬性中取得查詢字串是所使用的 HTTP 要求建立 SignalR 連線。 您可以設定連線，也就是從用戶端的用戶端相關的資料傳遞至伺服器的便利方式，在用戶端新增查詢字串參數。 下列範例會示範一種方法在 JavaScript 用戶端新增的查詢字串，當您使用產生的 proxy。

    [!code-javascript[Main](hubs-api-guide-server/samples/sample51.js?highlight=1)]

    如需有關設定查詢字串參數的詳細資訊，請參閱 API 指南[JavaScript](hubs-api-guide-javascript-client.md)並[.NET](hubs-api-guide-net-client.md)用戶端。

    您可以找到查詢字串的資料，以及供內部使用 SignalR 的一些其他值中的連接所使用的傳輸方法：

    [!code-csharp[Main](hubs-api-guide-server/samples/sample52.cs)]

    值`transportMethod`會 「 webSockets"、"serverSentEvents"、"foreverFrame"或"longPolling 」。 請注意，如果您檢查此值`OnConnected`事件處理常式方法，在某些情況下您一開始可能會收到不是最終的交涉的傳輸連線的方法的傳輸值。 在此情況下方法會擲回例外狀況，並將會呼叫稍後再建立的最後一個傳輸方法時。
- Cookie。

    [!code-csharp[Main](hubs-api-guide-server/samples/sample53.cs?highlight=1)]

    您也可以取得 cookie `Context.RequestCookies`。
- 使用者資訊。

    [!code-csharp[Main](hubs-api-guide-server/samples/sample54.cs?highlight=1)]
- 要求的 HttpContext 物件：

    [!code-csharp[Main](hubs-api-guide-server/samples/sample55.cs?highlight=1)]

    使用這個方法，而非得到`HttpContext.Current`以取得`HttpContext`SignalR 連線的物件。

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a>如何將用戶端與中樞類別之間傳遞狀態

用戶端 proxy 提供`state`您可以在您想要傳送至每個方法呼叫與伺服器的資料儲存的物件。 在伺服器上，您可以存取此資料在`Clients.Caller`中呼叫的用戶端中樞方法的屬性。 `Clients.Caller`屬性不會擴展連線存留期事件處理常式方法`OnConnected`， `OnDisconnected`，和`OnReconnected`。

建立或更新資料集中`state`物件和`Clients.Caller`屬性可在兩個方向。 您可以更新伺服器中的值，而且它們會傳回給用戶端。

下列範例會儲存傳輸到每個方法呼叫的伺服器狀態的 JavaScript 用戶端程式碼。

[!code-javascript[Main](hubs-api-guide-server/samples/sample56.js?highlight=1-2)]

下列範例會示範在.NET 用戶端對等的程式碼。

[!code-csharp[Main](hubs-api-guide-server/samples/sample57.cs?highlight=1-2)]

在中樞類別中，您可以存取此資料在`Clients.Caller`屬性。 下列範例程式碼來擷取上一個範例中所指的狀態。

[!code-csharp[Main](hubs-api-guide-server/samples/sample58.cs?highlight=3-4)]

> [!NOTE]
> 這項機制保存的狀態並不適用於大量的資料，因為您放入的所有項目`state`或`Clients.Caller`屬性是每個方法引動過程來回時間。 它可用於較小的項目，例如使用者名稱或計數器。


在 VB.NET 或強型別中樞中，無法透過存取呼叫端的狀態物件`Clients.Caller`; 相反地，使用`Clients.CallerState`（SignalR 2.1 中引進）：

**在 C# 中使用 CallerState**

[!code-csharp[Main](hubs-api-guide-server/samples/sample59.cs?highlight=3-4)]

**Visual Basic 中使用 CallerState**

[!code-vb[Main](hubs-api-guide-server/samples/sample60.vb)]

<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a>如何在中樞類別中處理錯誤

若要處理您的中樞類別方法中發生的錯誤，請使用一或多個下列方法：

- 將您的方法程式碼包裝在 try / catch 區塊，並記錄例外狀況物件。 以進行偵錯您可以將例外狀況傳送至用戶端，但在生產環境中的用戶端傳送的詳細的資訊的原因不建議的安全性。
- 建立中樞的管線模組來處理[OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx)方法。 下列範例示範會記錄錯誤，後面接著程式碼，將模組插入至中樞管線的 Startup.cs 中的管線模組。

    [!code-csharp[Main](hubs-api-guide-server/samples/sample61.cs)]

    [!code-csharp[Main](hubs-api-guide-server/samples/sample62.cs?highlight=4)]
- 使用`HubException`（SignalR 2 中引進） 的類別。 從任何中樞引動過程，可以擲回這個錯誤。 `HubError`建構函式採用字串訊息，並儲存額外的錯誤資料的物件。 SignalR 將自動序列化例外狀況，並將它傳送到用戶端，它使用拒絕或失敗中樞方法叫用。

    下列程式碼範例會示範如何擲回`HubException`期間中樞叫用，以及如何處理在 JavaScript 和.NET 用戶端上的例外狀況。

    **伺服端程式碼示範 HubException 類別**

    [!code-csharp[Main](hubs-api-guide-server/samples/sample63.cs)]

    **JavaScript 用戶端程式碼示範在中樞中擲回 HubException 的回應**

    [!code-html[Main](hubs-api-guide-server/samples/sample64.html)]

    **.NET 用戶端程式碼示範在中樞中擲回 HubException 的回應**

    [!code-csharp[Main](hubs-api-guide-server/samples/sample65.cs)]

如需中樞管線模組的詳細資訊，請參閱[爧簏濻中樞管線](#hubpipeline)本主題稍後的。

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a>如何啟用追蹤

若要啟用伺服器端追蹤，system.diagnostics 項目加入 Web.config 檔案中，在此範例中所示：

[!code-html[Main](hubs-api-guide-server/samples/sample66.html?highlight=17-72)]

當您在 Visual Studio 中執行應用程式時，您可以檢視中的記錄檔**輸出**視窗。

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a>如何呼叫方法的用戶端和管理中樞類別外的群組

若要從不同的類別，比您中樞的類別呼叫用戶端方法，取得 SignalR 內容物件的參考中樞並使用的用戶端上呼叫方法，或管理群組。

下例會`StockTicker`類別取得內容物件，將它儲存在類別的執行個體、 將類別執行個體儲存在靜態屬性，並使用從單一類別執行個體的內容來呼叫`updateStockPrice`之用戶端上的方法連接到集線器，名為`StockTickerHub`。

[!code-csharp[Main](hubs-api-guide-server/samples/sample67.cs?highlight=8,24)]

如果您需要使用內容多個時間長時間執行的物件中，一次取得參考，並儲存它，而非每次進行取得。 一次取得內容，可確保 SignalR 會將訊息傳送至用戶端在 Hub 方法進行用戶端方法引動過程的順序相同。 示範如何使用 SignalR 內容中樞教學課程中，請參閱[伺服器廣播與 ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md)。

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a>呼叫用戶端方法

您可以指定哪些用戶端會收到 RPC，但是您有較少的選項，比從中樞類別您每次呼叫時。 這是內容未與特定的呼叫，從用戶端，相關聯，因此任何方法，需要了解目前的連線識別碼，例如`Clients.Others`，或`Clients.Caller`，或`Clients.OthersInGroup`，未提供。 有下列選項可供使用：

- 所有已連線的用戶端。

    [!code-csharp[Main](hubs-api-guide-server/samples/sample68.cs)]
- 連接識別碼所識別的特定用戶端

    [!code-css[Main](hubs-api-guide-server/samples/sample69.css)]
- 所有已連線的用戶端，除了指定的用戶端，識別連接識別碼。

    [!code-csharp[Main](hubs-api-guide-server/samples/sample70.cs)]
- 指定群組中所有已連線的用戶端。

    [!code-css[Main](hubs-api-guide-server/samples/sample71.css)]
- 所有已連線的用戶端在指定的群組，除了指定的用戶端，識別連接識別碼。

    [!code-csharp[Main](hubs-api-guide-server/samples/sample72.cs)]

如果您呼叫到非中樞類別方法從中樞類別中，您可以傳入目前的連線識別碼，並使用透過`Clients.Client`， `Clients.AllExcept`，或`Clients.Group`模擬`Clients.Caller`， `Clients.Others`，或`Clients.OthersInGroup`。 在下列範例中，`MoveShapeHub`類別會將傳遞至的連線識別碼`Broadcaster`類別，讓`Broadcaster`類別可以模擬`Clients.Others`。

[!code-csharp[Main](hubs-api-guide-server/samples/sample73.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a>管理群組成員資格

群組管理您會有相同的選項，就像是在 Hub 類別。

- 加入群組中的用戶端

    [!code-csharp[Main](hubs-api-guide-server/samples/sample74.cs)]
- 從群組移除用戶端

    [!code-css[Main](hubs-api-guide-server/samples/sample75.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a>如何自訂中樞管線

SignalR 可讓您將自己的程式碼插入中樞管線。 下列範例會記錄來自用戶端與傳出用戶端上叫用的方法呼叫每個傳入方法呼叫的自訂中樞管線模組：

[!code-csharp[Main](hubs-api-guide-server/samples/sample76.cs)]

下列程式碼中*Startup.cs*檔案可註冊在中樞管線中執行的模組：

[!code-csharp[Main](hubs-api-guide-server/samples/sample77.cs?highlight=3)]

有許多不同的方法，您可以覆寫。 如需完整清單，請參閱 < [HubPipelineModule 方法](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx)。
