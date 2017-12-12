---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-server
title: "ASP.NET SignalR 中樞 API 指南-伺服器 (SignalR 1.x) |Microsoft 文件"
author: pfletcher
description: "本文件提供 SignalR 使用的程式碼範例 demonstratin 1.1 版的 ASP.NET SignalR 中樞 API 的伺服器端程式設計的簡介..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/17/2013
ms.topic: article
ms.assetid: 03e4b9f5-0fea-4d94-959f-014b2762a301
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: e594dd1ea4ae027cf0b82574fc5a3eb061b1f2e1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-signalr-hubs-api-guide---server-signalr-1x"></a>ASP.NET SignalR 中樞 API 指南-伺服器 (SignalR 1.x)
====================
由[Patrick Fletcher](https://github.com/pfletcher)， [Tom Dykstra](https://github.com/tdykstra)

> 本文件提供簡介 SignalR 1.1 版的 ASP.NET SignalR 中樞 API 的伺服器端程式設計的程式碼範例示範常用的選項。
> 
> SignalR 中樞應用程式開發介面可讓您從伺服器連線的用戶端和伺服器的用戶端進行遠端程序呼叫 (Rpc)。 伺服端程式碼，並定義用戶端，可以呼叫的方法，呼叫用戶端執行的方法。 用戶端程式碼，並定義可在伺服器上，從呼叫的方法，您呼叫在伺服器執行的方法。 SignalR 會負責為您的用戶端-伺服器一切細節。
> 
> SignalR 也能提供較低層級應用程式開發介面呼叫持續連線。 如需簡介 SignalR、 集線器及持續連線，或示範如何建置完整的 SignalR 應用程式的教學課程，請參閱[SignalR-快速入門](index.md)。


## <a name="overview"></a>概觀

本文件包含下列章節：

- [如何登錄至 SignalR 路由並設定 SignalR 選項](#route)

    - [/Signalr URL](#signalrurl)
    - [設定 SignalR 選項](#options)
- [如何建立及使用 Hub 類別](#hubclass)

    - [中樞物件存留期](#transience)
    - [依照 camel 命名法的大小寫的 JavaScript 用戶端中樞名稱](#hubnames)
    - [多個集線器](#multiplehubs)
- [如何在用戶端可以呼叫的中樞類別中定義的方法](#hubmethods)

    - [依照 camel 命名法的大小寫的 JavaScript 用戶端中的方法名稱](#methodnames)
    - [以非同步方式執行的時機](#asyncmethods)
    - [定義多載](#overloads)
- [如何從 Hub 類別的方法呼叫用戶端](#callfromhub)

    - [選取哪些用戶端會收到 RPC](#selectingclients)
    - [方法名稱無法編譯時間驗證](#dynamicmethodnames)
    - [不區分大小寫的方法名稱比對](#caseinsensitive)
    - [非同步執行](#asyncclient)
- [如何從中樞類別管理群組成員資格](#groupsfromhub)

    - [加入和移除方法的非同步執行](#asyncgroupmethods)
    - [群組成員資格持續性](#grouppersistence)
    - [單一使用者群組](#singleusergroups)
- [如何處理連接的存留期事件中樞類別中](#connectionlifetime)

    - [呼叫 OnConnected、 OnDisconnected 和 OnReconnected 的時機](#onreconnected)
    - [不會填入呼叫狀態](#nocallerstate)
- [如何取得用戶端的相關資訊，從內容屬性](#contextproperty)
- [如何將用戶端與中樞類別之間傳遞狀態](#passstate)
- [如何在集線器類別中處理錯誤](#handleErrors)
- [呼叫方法的用戶端和管理從中樞類別以外的群組](#callfromoutsidehub)

    - [呼叫用戶端方法](#callingclientsoutsidehub)
    - [管理群組成員資格](#managinggroupsoutsidehub)
- [如何啟用追蹤](#tracing)
- [如何以自訂中樞管線](#hubpipeline)

如需如何程式用戶端文件，請參閱下列資源：

- [SignalR 中樞 API 指南 JavaScript 用戶端](index.md)
- [SignalR 中樞 API 指南-.NET 用戶端](index.md)

應用程式開發介面參考主題的連結是.NET 4.5 版的 API。 如果您使用.NET 4，請參閱[API 主題.NET 4 版本](https://msdn.microsoft.com/en-us/library/jj891075(v=vs.100).aspx)。

<a id="route"></a>

## <a name="how-to-register-the-signalr-route-and-configure-signalr-options"></a>如何登錄至 SignalR 路由並設定 SignalR 選項

若要定義用戶端將用來連接到您的中樞的路由，請呼叫[MapHubs](https://msdn.microsoft.com/en-us/library/system.web.routing.signalrrouteextensions.maphubs(v=vs.111).aspx)應用程式啟動時的方法。 `MapHubs`是[擴充方法](https://msdn.microsoft.com/en-us/library/vstudio/bb383977.aspx)如`System.Web.Routing.RouteCollection`類別。 下列範例示範如何定義中的 SignalR 中樞路由*Global.asax*檔案。

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample1.cs)]

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample2.cs?highlight=5)]

如果您要將 SignalR 功能加入 ASP.NET MVC 應用程式，請確定 SignalR 路由會加入其他路由之前。 如需詳細資訊，請參閱[教學課程： 開始使用 SignalR 和 MVC 4](index.md)。

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a>/Signalr URL

根據預設，用戶端將用來連接到您的中樞的路由 URL 是:"/ signalr"。 （請勿混淆這個 URL，使用"signalr/中樞 」 的 URL，這是自動產生的 JavaScript 檔案。 如需產生之 proxy 的詳細資訊，請參閱[SignalR 中樞 API 指南-JavaScript 用戶端-產生的 proxy，並會為您](index.md)。)

可能有異常的情況下，讓此基底 URL 無法使用適用於 SignalR;例如，名為專案中有一個資料夾*signalr*而且您不想要變更名稱。 在此情況下，您可以變更基底 URL，如下列範例所示 (取代 「 / signalr 」 範例程式碼以您所需的 URL)。

**指定的 URL 的伺服器程式碼**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample3.cs?highlight=1)]

**指定的 URL （與產生的 proxy) 的 JavaScript 用戶端程式碼**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample4.js?highlight=1)]

**指定的 URL （不含產生的 proxy) 的 JavaScript 用戶端程式碼**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample5.js?highlight=1)]

**.NET 用戶端程式碼來指定的 URL**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample6.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a>設定 SignalR 選項

多載`MapHubs`方法可讓您指定自訂 URL、 自訂相依性解析程式，以及下列選項：

- 啟用從瀏覽器用戶端的跨網域呼叫。

    通常如果瀏覽器中載入的頁面上，從`http://contoso.com`，SignalR 連線是在相同網域中，在`http://contoso.com/signalr`。 如果將頁面從`http://contoso.com`會連接到`http://fabrikam.com/signalr`，也就是跨網域的連接。 基於安全性理由，預設會停用跨網域連線。 如需詳細資訊，請參閱[ASP.NET SignalR 中樞 API 指南-JavaScript 用戶端-如何建立跨定義域連線](index.md)。
- 啟用詳細的錯誤訊息。

    發生錯誤時，SignalR 的預設行為是傳送給用戶端通知訊息，但發生了什麼事的詳細資料。 詳細的錯誤資訊傳送給用戶端不建議在實際執行環境，因為惡意使用者可能可以使用攻擊您的應用程式中的資訊。 如需疑難排解，您可以使用此選項以暫時啟用更具資訊性的錯誤報告。
- 停用自動產生的 JavaScript proxy 檔案。

    根據預設，使用 proxy 的 JavaScript 檔案，為您的中樞類別會產生以回應 URL 」 signalr/中樞 」。 如果您不想要使用的 JavaScript proxy，或如果您想要以手動方式產生這個檔案，而且參考的實體檔案中您的用戶端，您可以使用此選項以停用 proxy 產生。 如需詳細資訊，請參閱[SignalR 中樞 API 指南-JavaScript 用戶端-如何建立 SignalR 的實體檔案產生 proxy](index.md)。

下列範例示範如何呼叫中指定的 SignalR 連線 URL 和這些選項`MapHubs`方法。 若要指定自訂 URL，取代 「 / signalr 」 在您想要使用的 url 範例。

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample7.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a>如何建立及使用 Hub 類別

若要建立的中樞時，建立衍生自類別[Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx)。 下列範例會示範一個簡單的中樞類別交談應用程式。

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample8.cs)]

在此範例中，連線的用戶端可以呼叫`NewContosoChatMessage`方法，並收到的資料時，會傳播到所有連線的用戶端。

<a id="transience"></a>

### <a name="hub-object-lifetime"></a>中樞物件存留期

您不具現化中樞類別，或從伺服器上您的程式碼呼叫其方法所有的方式是為您 SignalR 中樞管線。 SignalR 建立 Hub 類別的新執行的個體每次需要處理的中樞 」 作業，例如當用戶端連接、 中斷連接，或對伺服器提出呼叫的方法。

因為 Hub 類別的執行個體是暫時性的所以您無法使用它們來維護狀態從一種方法呼叫下一步。 每次在伺服器接收方法呼叫從用戶端中樞的類別處理序的新執行個體的訊息。 為了維護透過多個連線和方法呼叫的狀態，使用透過其他方法，例如資料庫或靜態變數 Hub 類別，或不同類別且不是衍生自`Hub`。 如果您將保存在記憶體中的資料，中樞類別上使用的方法，例如靜態變數的資料將會遺失應用程式定義域回收時。

如果您想要從自己的中樞類別外執行的程式碼將訊息傳送給用戶端，您無法這樣藉由執行個體化中樞類別的執行個體，但這麼做為中樞類別取得 SignalR 內容物件的參考。 如需詳細資訊，請參閱[如何呼叫方法的用戶端和管理群組從中樞類別之外](#callfromoutsidehub)本主題稍後。

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a>依照 camel 命名法的大小寫的 JavaScript 用戶端中樞名稱

根據預設，JavaScript 用戶端是指中心所使用的類別名稱的 camel 案例版本。 SignalR 會自動將這項變更，讓 JavaScript 程式碼可以符合 JavaScript 慣例。 前一個範例會稱為`contosoChatHub`JavaScript 程式碼中。

**伺服器**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample9.cs?highlight=1)]

**JavaScript 用戶端會使用產生的 proxy**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample10.js?highlight=1)]

如果您想要指定不同的名稱，讓用戶端使用，所以將`HubName`屬性。 當您使用`HubName`屬性，請為 camel 命名法的大小寫，JavaScript 用戶端上沒有名稱變更。

**伺服器**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample11.cs?highlight=1)]

**JavaScript 用戶端會使用產生的 proxy**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample12.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a>多個集線器

您可以定義多個 Hub 類別的應用程式中。 當您這樣做，時請的連接共用，但群組都不同：

- 所有用戶端會使用相同的 URL，建立您的服務與 SignalR 連線 ("/ signalr"或自訂的 URL 是否指定)，而且連接會用於所有中樞的服務來定義。

    沒有任何效能差異相較於單一類別中定義中樞的所有功能的多個中樞。
- 所有中樞都取得相同的 HTTP 要求資訊。

    因為所有中樞會都共用相同的連接，唯一伺服器取得的 HTTP 要求資訊是就會建立 SignalR 連線的原始 HTTP 要求中。 如果您使用連線要求從用戶端傳遞資訊給伺服器藉由指定的查詢字串時，您無法提供不同的查詢字串至不同的中樞。 所有中樞將會都收到相同的資訊。
- 產生的 JavaScript proxy 檔案會包含在一個檔案中的所有主機的 proxy。

    如需 JavaScript proxy 資訊，請參閱[SignalR 中樞 API 指南-JavaScript 用戶端-產生的 proxy，並會為您](index.md)。
- 中樞內，會定義群組。

    您可以定義的 SignalR 中名為廣播已連線的用戶端子集的群組。 群組會分別維護每個中樞。 例如，名為 「 系統管理員 」 群組可包含的用戶端的一組您`ContosoChatHub`類別和相同的群組名稱會參考一組不同的用戶端程式`StockTickerHub`類別。

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a>如何在用戶端可以呼叫的中樞類別中定義的方法

若要公開您想要從用戶端可呼叫的中樞上的方法，宣告的公用方法，如下列範例所示。

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample14.cs?highlight=3)]

您可以指定傳回型別和參數，包括複雜型別及陣列，您可以按照任何 C# 方法。 您收到中參數或傳回給呼叫者的任何資料是用戶端與伺服器之間使用 JSON，即予 SignalR 複雜物件的繫結和物件的陣列會自動處理。

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a>依照 camel 命名法的大小寫的 JavaScript 用戶端中的方法名稱

根據預設，JavaScript 用戶端中樞方法使用參照的方法名稱的 camel 案例版本。 SignalR 會自動將這項變更，讓 JavaScript 程式碼可以符合 JavaScript 慣例。

**伺服器**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample15.cs?highlight=1)]

**JavaScript 用戶端會使用產生的 proxy**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample16.js?highlight=1)]

如果您想要指定不同的名稱，讓用戶端使用，所以將`HubMethodName`屬性。

**伺服器**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample17.cs?highlight=1)]

**JavaScript 用戶端會使用產生的 proxy**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a>以非同步方式執行的時機

如果方法將會是長時間執行，或者要執行的作業，會牽涉到插撥功能，例如資料庫尋查 」 或 「 web 服務呼叫，讓中樞方法以傳回非同步[工作](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task.aspx)(取代`void`傳回) 或[工作&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/dd321424.aspx)物件 (取代`T`傳回型別)。 當您傳回`Task`從 SignalR 方法的物件會等待`Task`若要完成，然後將進行傳送未包裝的結果傳回用戶端，因此沒有任何差異，在程式碼方法呼叫中用戶端如何。

讓中樞方法非同步可避免使用 WebSocket 傳輸時，封鎖連接。 當中樞方法以同步方式執行，而且傳輸 WebSocket 時，後續來自相同用戶端中樞方法叫用會遭到封鎖，直到中樞方法完成為止。

下列範例示範相同的方法以同步方式執行自動程式碼，或以非同步的方式，後面接著函式呼叫其中一個版本的運作方式的 JavaScript 用戶端程式碼。

**同步**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample19.cs)]

**非同步-ASP.NET 4.5**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

**JavaScript 用戶端會使用產生的 proxy**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample21.js)]

如需如何使用 ASP.NET 4.5 中的非同步方法的詳細資訊，請參閱[使用 ASP.NET MVC 4 中的非同步方法](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md)。

<a id="overloads"></a>

### <a name="defining-overloads"></a>定義多載

如果您想要定義方法的多載，必須是不同中每個多載的參數數目。 如果您只是藉由指定不同的參數類型區分多載，中樞類別將會編譯，但 SignalR 服務將會擲回例外狀況在執行階段，當用戶端會嘗試呼叫其中一個多載。

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a>如何從 Hub 類別的方法呼叫用戶端

若要從伺服器呼叫用戶端的方法，請使用`Clients`中樞類別中的方法中的屬性。 下列範例示範呼叫的伺服器程式碼`addNewMessageToPage`所有已連線的用戶端及 JavaScript 用戶端中定義之方法的用戶端程式碼。

**伺服器**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample22.cs?highlight=5)]

**JavaScript 用戶端會使用產生的 proxy**

[!code-html[Main](signalr-1x-hubs-api-guide-server/samples/sample23.html?highlight=1)]

您無法取得傳回值，從用戶端方法;例如語法`int x = Clients.All.add(1,1)`無法運作。

您可以指定複雜型別和參數的陣列。 下列範例會將複雜型別傳遞至方法參數中的用戶端。

**呼叫用戶端使用的複雜物件的伺服器程式碼**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample24.cs?highlight=3)]

**定義的複雜物件的伺服器程式碼**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample25.cs?highlight=1)]

**JavaScript 用戶端會使用產生的 proxy**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample26.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a>選取哪些用戶端會收到 RPC

用戶端屬性會傳回[HubConnectionContext](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx)提供數個選項來指定哪些用戶端會收到 RPC 的物件：

- 所有已連線的用戶端。

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample27.cs)]
- 只能呼叫用戶端。

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample28.cs)]
- 所有用戶端 （呼叫用戶端除外）。

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample29.cs)]
- 連接識別碼所識別的特定用戶端

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample30.css)]

    這個範例會呼叫`addContosoChatMessageToPage`呼叫用戶端上且使用相同的效果`Clients.Caller`。
- 除了指定用戶端連接識別碼所識別的所有已連線用戶端

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample31.cs)]
- 指定的群組中所有已連線的用戶端。

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample32.css)]
- 除了指定用戶端連接識別碼所識別的指定群組中所有已連線的用戶端

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample33.cs)]
- 指定的群組中所有已連線的用戶端 （呼叫用戶端除外）。

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample34.css)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a>方法名稱無法編譯時間驗證

您指定的方法名稱會解譯為動態的物件，表示沒有任何 IntelliSense 或它的編譯時間驗證。 在執行階段會評估運算式。 當執行方法呼叫時，SignalR 的方法名稱和參數值傳送至用戶端，且如果用戶端有方法符合名稱、 呼叫的方法和參數值會傳遞給它。 如果用戶端上找到沒有對應的方法，就不會引發錯誤。 SignalR 傳輸至用戶端在幕後，當您呼叫用戶端方法的資料格式的相關資訊，請參閱[簡介 SignalR](index.md)。

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a>不區分大小寫的方法名稱比對

方法名稱比對不區分大小寫。 例如，`Clients.All.addContosoChatMessageToPage`在伺服器上將會執行`AddContosoChatMessageToPage`， `addcontosochatmessagetopage`，或`addContosoChatMessageToPage`用戶端上。

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a>非同步執行

以非同步方式執行，讓您呼叫這個方法。 來自用戶端的方法呼叫會立即執行，而不需等待 SignalR 完成傳輸資料至用戶端，除非您指定的後續的行程式碼應該等候方法完成之後的任何程式碼。 下列程式碼範例示範如何依序執行兩個用戶端方法，一個是使用程式碼，使其能夠在.NET 4.5，另一個使用程式碼，使其能夠在.NET 4 中。

**.NET 4.5 範例**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample35.cs?highlight=1,3)]

**.NET 4 範例**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample36.cs?highlight=3-4)]

如果您使用`await`或`ContinueWith`等候，直到下的一行程式碼執行之前，用戶端方法完成，這不表示用戶端會實際收到訊息之前執行的下的一行程式碼。 用戶端方法呼叫的 「 完成 」 只表示 SignalR 已經完成傳送訊息所需的所有項目。 如果您需要驗證用戶端收到訊息時，您必須自行撰寫此機制。 例如，您可以撰寫程式碼`MessageReceived`方法，在中樞內，並在`addContosoChatMessageToPage`方法，您可以呼叫用戶端上的`MessageReceived`這麼做之後，任何正常運作，您需要在用戶端上。 在`MessageReceived`中樞中您可以執行任何工作取決於實際的用戶端接收和處理的原始方法呼叫。

### <a name="how-to-use-a-string-variable-as-the-method-name"></a>如何使用字串變數，做為方法名稱

如果您想要使用的字串變數的方法名稱，轉換為叫用用戶端方法`Clients.All`(或`Clients.Others`，`Clients.Caller`等等) 要`IClientProxy`，然後呼叫[叫用 （方法名稱、 引數...）](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample37.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a>如何從中樞類別管理群組成員資格

SignalR 中的群組提供方法，將訊息廣播至連線的用戶端指定的子集。 群組可以有任意數目的用戶端，並在用戶端可以是任意數目的群組成員。

為管理群組成員資格，使用[新增](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx)和[移除](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx)所提供的方法`Groups`中樞類別的屬性。 下列範例所示`Groups.Add`和`Groups.Remove`方法用於 Hub 方法呼叫的用戶端程式碼，後面接著 JavaScript 用戶端程式碼呼叫它們。

**伺服器**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample38.cs?highlight=5,10)]

**JavaScript 用戶端會使用產生的 proxy**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample39.js)]

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample40.js)]

您不需要明確地建立群組。 作用中的群組會自動建立第一次呼叫中指定其名稱`Groups.Add`，並從它的成員資格中移除最後一次連接時，就會刪除。

沒有任何 API 來取得群組成員資格清單或群組的清單。 SignalR 將訊息傳送至用戶端與群組根據[pub/sub 模型](http://en.wikipedia.org/wiki/Publish/subscribe)，而且伺服器不會保留群組或群組成員資格的清單。 這有助於發揮最大延展性，因為每當您將節點加入至 web 伺服陣列時，必須傳播至新的節點 SignalR 維護任何狀態。

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a>加入和移除方法的非同步執行

`Groups.Add`和`Groups.Remove`方法以非同步方式執行。 如果您想要新增到群組的用戶端，並立即傳送訊息至用戶端使用群組，您必須確定`Groups.Add`方法完成第一次。 下列程式碼範例示範如何這樣做，請使用適用於.NET 4.5，以及使用在.NET 4 中運作的程式碼的程式碼

**.NET 4.5 範例**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

**.NET 4 範例**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample42.cs?highlight=3-4)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a>群組成員資格持續性

SignalR 追蹤連線，而不是使用者，因此如果您希望使用者是相同群組中每次使用者建立的連接，您必須呼叫`Groups.Add`每次使用者建立新的連接。

之後連線暫時中斷，有時候 SignalR 可以連線自動還原。 在此情況下，SignalR 還原相同的連接，不會建立新的連接，並因此就會自動還原用戶端的群組成員資格。 這是可能甚至暫時中斷時將伺服器重新啟動或失敗，因為每個用戶端，包括群組的成員資格的連接狀態往返到用戶端。 如果伺服器關閉，且會取代新的伺服器連接逾時之前，用戶端可以自動重新連線至新的伺服器，然後重新註冊它所屬的群組中。

當無法自動恢復連線之後連線、 中斷, 或時連接逾時，或用戶端中斷連接 （例如，當瀏覽器瀏覽至新頁面），群組成員資格將會遺失。 在使用者連接在下一次將新的連接。 若要維護相同的使用者建立新的連接群組成員資格，您的應用程式必須追蹤、 使用者和群組之間的關聯，然後還原群組成員資格，每次使用者建立新的連接。

如需連接和重新連線的詳細資訊，請參閱[如何處理連接的存留期事件中樞類別中](#connectionlifetime)本主題稍後。

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a>單一使用者群組

通常使用 SignalR 的應用程式需要追蹤的使用者與連線之間的關聯，才能知道哪些使用者已經傳送的訊息，以及哪些使用者應該能夠接收訊息。 群組會這麼做可在兩個常用模式的其中一個用於。

- 單一使用者群組。

    您可以為群組名稱時，指定的使用者名稱及目前的連接識別碼新增至群組，每次使用者連線或重新連線。 若要傳送訊息給使用者，傳送至群組。 這個方法的缺點是該群組不會提供您找出使用者是否為線上或離線的方式。
- 追蹤使用者名稱和連接識別碼之間的關聯。

    您可以儲存在字典或資料庫，每個使用者名稱和一或多個連線識別碼之間的關聯，並更新儲存的資料每次使用者連線或中斷連線。 若要傳送訊息給使用者，您可以指定連線識別碼。 這個方法的缺點是，花費更多記憶體。

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a>如何處理連接的存留期事件中樞類別中

處理連接的存留期事件的常見原因是來追蹤是否已連線，以及追蹤的使用者名稱和連線識別碼之間的關聯。 若要執行自己的程式碼，用戶端連接或中斷連線時，覆寫`OnConnected`， `OnDisconnected`，和`OnReconnected`類別虛擬方法的中樞，如下列範例所示。

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample43.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a>呼叫 OnConnected、 OnDisconnected 和 OnReconnected 的時機

瀏覽器瀏覽至新頁面，每次新的連接必須建立，這表示將會執行 SignalR`OnDisconnected`後面接著方法`OnConnected`方法。 建立新連線時讓 SignalR 一律會建立新的連接識別碼。

`OnReconnected`呼叫方法時，發生暫時性中斷 SignalR 可以自動復原，例如當暫時中斷連線並重新連接的連接逾時之前的纜線的連線。`OnDisconnected`方法時呼叫，用戶端中斷連線並 SignalR 無法自動重新連線，例如當瀏覽器瀏覽至新的頁面。 因此，可能針對給定的用戶端的事件序列是`OnConnected`， `OnReconnected`， `OnDisconnected`; 或`OnConnected`， `OnDisconnected`。 不會看到序列`OnConnected`， `OnDisconnected`，`OnReconnected`給定連接。

`OnDisconnected`方法不會被呼叫，請在某些情況下，例如當伺服器關閉或回收應用程式定義域，取得。 當另一部伺服器上線時或在應用程式網域完成其回收時，某些用戶端能夠重新連線，並引發`OnReconnected`事件。

如需詳細資訊，請參閱[了解與 SignalR 中處理連接存留期事件](index.md)。

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a>不會填入呼叫狀態

連接生命週期事件處理常式方法會呼叫從伺服器上，這表示您將放在任何狀態`state`用戶端上的物件將不會填入`Caller`在伺服器上的屬性。 如需有關資訊`state`物件和`Caller`屬性，請參閱[如何將用戶端與中樞類別之間傳遞狀態](#passstate)本主題稍後。

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a>如何取得用戶端的相關資訊，從內容屬性

若要取得用戶端的相關資訊，請使用`Context`中樞類別的屬性。 `Context`屬性會傳回[HubCallerContext](https://msdn.microsoft.com/en-us/library/jj890883(v=vs.111).aspx)物件，其提供下列資訊的存取權：

- 呼叫用戶端連接識別碼。

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample44.cs?highlight=1)]

    連接識別碼是的 SignalR （您無法在自己的程式碼中指定的值） 所指派的 GUID。 還有一個針對每個連接和相同的連接識別碼由所有中樞，如果您的應用程式中有多個中樞的連線識別碼。
- HTTP 標頭的資料。

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample45.cs?highlight=1)]

    您也可以取得 HTTP 標頭從`Context.Headers`。 多個參考相同的原因在於`Context.Headers`首先，建立`Context.Request`屬性之後，加入和`Context.Headers`保留回溯相容性。
- 查詢字串的資料。

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample46.cs?highlight=1)]

    您也可以取得查詢字串資料從`Context.QueryString`。

    您在這個屬性中取得查詢字串，則建立 SignalR 連線的 HTTP 要求搭配使用的一個。 您可以加入用戶端中的查詢字串參數，藉由設定連接，這是方便的方式從用戶端的用戶端的相關資料傳送至伺服器。 下列範例會示範其中一種方式新增的查詢字串中的 JavaScript 用戶端，當您使用產生的 proxy。

    [!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample47.js?highlight=1)]

    如需有關設定查詢字串參數的詳細資訊，請參閱應用程式開發介面指南[JavaScript](index.md)和[.NET](index.md)用戶端。

    您可以找到連線上的查詢字串的資料，供內部使用 SignalR 的其他值一起使用的傳輸方法：

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample48.cs)]

    值`transportMethod`會 「 webSockets"、"serverSentEvents"、"foreverFrame"或"longPolling"。 請注意，如果您確認此值`OnConnected`事件處理常式方法，在某些情況下您可能一開始就會傳輸的值不是連線的最後一個交涉的傳輸方法。 在此情況下方法將會擲回例外狀況，並建立最後傳輸方法時將稍後呼叫。
- Cookie。

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    您也可以取得 cookie `Context.RequestCookies`。
- 使用者資訊。

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample50.cs?highlight=1)]
- 要求的 HttpContext 物件：

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample51.cs?highlight=1)]

    使用這個方法，而非`HttpContext.Current`取得`HttpContext`SignalR 連線的物件。

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a>如何將用戶端與中樞類別之間傳遞狀態

用戶端 proxy 提供`state`您可以在您想要傳送到每個方法呼叫的伺服器的資料儲存的物件。 在伺服器上，您可以存取此資料在`Clients.Caller`中呼叫的用戶端中樞方法的屬性。 `Clients.Caller`連接生命週期事件處理常式方法不會擴展屬性`OnConnected`， `OnDisconnected`，和`OnReconnected`。

建立或更新資料集中的`state`物件和`Clients.Caller`屬性的運作方式在兩個方向。 您可以更新伺服器中的值，而且它們會傳回給用戶端。

下列範例會顯示 JavaScript 用戶端程式碼，儲存到每個方法呼叫的伺服器傳輸的狀態。

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample52.js?highlight=1-2)]

下列範例會顯示在.NET 用戶端對等的程式碼。

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample53.cs?highlight=1-2)]

在中樞類別中，您可以存取此資料在`Clients.Caller`屬性。 下列範例會顯示程式碼會擷取前一個範例中所參考的狀態。

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample54.cs?highlight=3-4)]

> [!NOTE]
> 這項機制來保存狀態，並不適用於大量的資料，因為所有項目置於`state`或`Clients.Caller`屬性是往返與每個方法引動過程。 它可用於較小的項目，例如使用者名稱或計數器。


<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a>如何在集線器類別中處理錯誤

若要處理中樞類別方法中發生的錯誤，使用其中一個或多個下列方法：

- 將方法的程式碼包裝在 try catch 區塊，並記錄例外狀況物件。 以進行偵錯您可以將例外狀況傳送給用戶端，但不是建議原因的詳細的資訊傳送給用戶端在生產環境中的安全性。
- 建立中樞管線模組來處理[OnIncomingError](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx)方法。 下列範例會顯示記錄錯誤，後面接著程式碼中會插入至中樞管線的模組的 Global.asax 管線模組。

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample55.cs)]

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample56.cs?highlight=3)]

如需中樞管線模組的詳細資訊，請參閱[如何以自訂中樞管線](#hubpipeline)本主題稍後。

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a>如何啟用追蹤

若要啟用伺服器端追蹤，system.diagnostics 項目加入您的 Web.config 檔案，在此範例所示：

[!code-html[Main](signalr-1x-hubs-api-guide-server/samples/sample57.html?highlight=17-72)]

當您在 Visual Studio 中執行應用程式時，您可以檢視中的記錄檔**輸出**視窗。

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a>呼叫方法的用戶端和管理從中樞類別以外的群組

若要從不同的類別，比集線器類別來呼叫用戶端方法，中樞取得 SignalR 內容物件的參考並使用該用戶端上呼叫方法，或管理群組。

下列範例`StockTicker`類別取得內容物件，將它儲存在類別的執行個體、 將類別執行個體儲存在靜態屬性，並使用從單一類別執行個體的內容來呼叫`updateStockPrice`方法上的用戶端連接到集線器，名為`StockTickerHub`。

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample58.cs?highlight=8,24)]

如果您需要長時間執行的物件中使用內容多時間，取得參考一次，並儲存它，而非每次進行取得。 一次取得內容，可確保 SignalR 會將訊息傳送至用戶端所在 Hub 方法進行用戶端方法引動過程的順序相同。 如需示範如何使用集線器的 SignalR 內容的教學課程，請參閱[伺服器廣播透過 ASP.NET SignalR](index.md)。

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a>呼叫用戶端方法

您可以指定哪些用戶端會收到 RPC，但是有比當您呼叫從 Hub 類別的選項較少。 這是因為內容未與特定的呼叫，從用戶端，相關聯，因此任何方法，需要了解目前的連線識別碼，例如`Clients.Others`，或`Clients.Caller`，或`Clients.OthersInGroup`，則無法使用。 有下列選項可供使用：

- 所有已連線的用戶端。

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample59.cs)]
- 連接識別碼所識別的特定用戶端

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample60.css)]
- 除了指定用戶端連接識別碼所識別的所有已連線用戶端

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample61.cs)]
- 指定的群組中所有已連線的用戶端。

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample62.css)]
- 除了指定的用戶端，連接識別碼所識別的指定群組中所有已連線的用戶端

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample63.cs)]

如果您呼叫非中樞類別方法從中樞類別中，您可以傳入目前的連接識別碼，並使用透過`Clients.Client`， `Clients.AllExcept`，或`Clients.Group`模擬`Clients.Caller`， `Clients.Others`，或`Clients.OthersInGroup`。 在下列範例中，`MoveShapeHub`類別會傳遞至的連線識別碼`Broadcaster`類別以便`Broadcaster`類別可以模擬`Clients.Others`。

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample64.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a>管理群組成員資格

用於管理群組您會有相同的選項，就像是在中樞類別。

- 用戶端新增到群組

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample65.cs)]
- 從群組移除用戶端

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample66.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a>如何以自訂中樞管線

SignalR 可讓您將自己的程式碼插入中樞管線。 下列範例會顯示記錄來自用戶端和外寄的方法呼叫，用戶端上叫用每個傳入方法呼叫的自訂中樞管線模組：

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample67.cs)]

下列程式碼中*Global.asax*檔案會註冊在中樞管線中執行模組：

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample68.cs?highlight=3)]

有許多不同的方法，您可以覆寫。 如需完整清單，請參閱[HubPipelineModule 方法](https://msdn.microsoft.com/en-us/library/jj918633(v=vs.111).aspx)。
