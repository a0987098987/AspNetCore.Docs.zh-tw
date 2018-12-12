---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
title: ASP.NET SignalR 中樞 API 指南-JavaScript 用戶端 |Microsoft Docs
author: pfletcher
description: 本文件提供使用 signalr 第 2 版中 JavaScript 用戶端，例如瀏覽器和 Windows 市集 (WinJS) applicat 的中樞 API 的簡介...
ms.author: riande
ms.date: 09/28/2015
ms.assetid: a9fd4dc0-1b96-4443-82ca-932a5b4a8ea4
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 8e493eda256351904da49e1222773f188e6a2058
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2018
ms.locfileid: "53288062"
---
<a name="aspnet-signalr-hubs-api-guide---javascript-client"></a>ASP.NET SignalR 中樞 API 指南-JavaScript 用戶端
====================
藉由[Patrick Fletcher](https://github.com/pfletcher)， [Tom Dykstra](https://github.com/tdykstra)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> 本文件提供使用 signalr 第 2 版中 JavaScript 用戶端，例如瀏覽器和 Windows 市集 (WinJS) 應用程式的中樞 API 的簡介。
>
> SignalR 中樞 API 可讓您從伺服器連線的用戶端和伺服器的用戶端進行遠端程序呼叫 (Rpc)。 在伺服器程式碼中，您定義可由用戶端，呼叫的方法，呼叫用戶端執行的方法。 在用戶端程式碼中，您定義可以在伺服器上，從呼叫的方法，您呼叫在伺服器執行的方法。 SignalR 會處理所有為您的用戶端-伺服器配管。
>
> SignalR 也提供一個名為持續連線的較低層級 API。 如需 SignalR、 中樞和持續連線，請參閱 < [SignalR 簡介](../getting-started/introduction-to-signalr.md)。
>
> ## <a name="software-versions-used-in-this-topic"></a>本主題中使用的軟體版本
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
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

本文件包含下列章節：

- [產生的 proxy，它做什麼，](#genproxy)

    - [使用產生的 proxy 的時機](#cantusegenproxy)
- [用戶端安裝程式](#clientsetup)

    - [如何參考動態產生的 proxy](#dynamicproxy)
    - [如何建立 signalr 的實體檔案產生 proxy](#manualproxy)
- [如何建立連線](#establishconnection)

    - [$。 connection.hub 是相同物件會建立該 $.hubConnection()](#connequivalence)
    - [非同步執行的 start 方法](#asyncstart)
- [如何建立跨網域的連線](#crossdomain)
- [如何設定連線](#configureconnection)

    - [如何指定查詢字串參數](#querystring)
    - [如何指定傳輸方法](#transport)
- [如何取得 Hub 類別的 proxy](#getproxy)
- [如何定義伺服器可以呼叫用戶端上的方法](#callclient)
- [如何從用戶端呼叫伺服器方法](#callserver)
- [如何處理連接的存留期事件](#connectionlifetime)
- [如何處理錯誤](#handleerrors)
- [如何啟用用戶端記錄](#logging)

如需如何撰寫程式的伺服器或.NET 用戶端的文件，請參閱下列資源：

- [SignalR 中樞 API 指南-伺服器](hubs-api-guide-server.md)
- [SignalR 中樞 API 指南-.NET 用戶端](hubs-api-guide-net-client.md)

（雖然沒有.NET 用戶端在.NET 4.0 的 SignalR 2），並僅用於.NET 4.5 SignalR 2 伺服器元件。

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a>產生的 proxy，它做什麼，

您可以編寫 JavaScript 用戶端通訊的 SignalR 服務，或不使用 SignalR 會為您產生的 proxy。 Proxy 會為您是簡化您的程式碼使用連線，寫入方法之伺服器呼叫，在伺服器上呼叫方法的語法。

當您撰寫程式碼呼叫伺服器方法時，產生的 proxy 可讓您使用看起來就好像您正在執行的區域函式的語法： 您可以撰寫`serverMethod(arg1, arg2)`而不是`invoke('serverMethod', arg1, arg2)`。 如果您拼錯伺服器方法名稱，產生的 proxy 語法也可讓立即且容易理解的用戶端錯誤。 如果您以手動方式建立可定義 proxy 的檔案，則您也可以撰寫會呼叫伺服器方法的程式碼取得 IntelliSense 支援。

例如，假設您在伺服器上有下列的中樞類別：

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

下列程式碼範例顯示的 JavaScript 程式碼看起來像什麼叫用`NewContosoChatMessage`方法，在伺服器和收到的引動過程上`addContosoChatMessageToPage`來自伺服器的方法。

**使用產生的 proxy**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

**不產生的 proxy**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a>使用產生的 proxy 的時機

如果您想要註冊的伺服器呼叫的用戶端方法的多個事件處理常式，您無法使用產生的 proxy。 否則，您可以選擇使用產生的 proxy，或不會根據您的程式碼撰寫喜好設定。 如果您選擇不使用它，您不需要參考中的 「 signalr/中樞 」 URL`script`用戶端程式碼中的項目。

<a id="clientsetup"></a>

## <a name="client-setup"></a>用戶端安裝程式

JavaScript 用戶端必須參考 jQuery 和 SignalR core JavaScript 檔案。 JQuery 版本必須是 1.6.4 或主要更新版本的詳細資訊，例如 1.7.2、 1.8.2 或 1.9.1。 如果您決定要使用產生的 proxy，您也需要將產生的 SignalR proxy JavaScript 檔案的參考。 下列範例顯示參考可能會如下所示在 HTML 頁面，會使用產生的 proxy。

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample4.html)]

這些參考必須包含在此順序： jQuery 在那之後，SignalR core SignalR proxy 名字、 姓氏。

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a>如何參考動態產生的 proxy

在上述範例中，產生的 SignalR proxy 的參考是動態產生的 JavaScript 程式碼，不以實體檔案。 SignalR 即時 proxy 建立的 JavaScript 程式碼，並提供給用戶端以回應至 「 signalr/中樞 」 的 URL。 如果您在伺服器上指定不同的基底 URL，SignalR 連線的您`MapSignalR`方法，以動態方式產生之 proxy 檔案的 URL 是您自訂的 URL，使用"/ 中樞"附加到它。

> [!NOTE]
> Windows 8 （Windows 市集） JavaScript 用戶端，請使用實體的 proxy 檔案，而不是動態產生。 如需詳細資訊，請參閱 <<c0> [ 如何建立 signalr 的實體檔案產生 proxy](#manualproxy)本主題稍後的。


在 ASP.NET MVC 4 或 5 的 Razor 檢視中，請參考您 proxy 檔案參考中的應用程式根目錄使用波狀符號：

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample5.html)]

如需使用 MVC 5 中的 SignalR 的詳細資訊，請參閱[開始使用 SignalR 和 MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)。

在 ASP.NET MVC 3 Razor 檢視中，使用`Url.Content`供您的 proxy 檔案參考：

[!code-cshtml[Main](hubs-api-guide-javascript-client/samples/sample6.cshtml)]

在 ASP.NET Web Forms 應用程式中，使用`ResolveClientUrl`您的 proxy 檔案的參考，或透過使用應用程式的根相對路徑 （開頭為波狀符號） ScriptManager 來註冊它：

[!code-aspx[Main](hubs-api-guide-javascript-client/samples/sample7.aspx)]

一般而言，使用相同的方法來指定您用於 CSS 或 JavaScript 檔案的 「 signalr/中樞 」 URL。 如果您指定的 URL，而不需使用波狀符號，在某些情況下您的應用程式會正常運作時您在 Visual Studio 中使用 IIS Express 測試，但部署至完整 IIS 時，將會失敗並傳回 404 錯誤。 如需詳細資訊，請參閱 <<c0>  **解析根層級資源的參考**中[ASP.NET Web 專案的 Visual Studio 中的 Web 伺服器](https://msdn.microsoft.com/library/58wxa9w5.aspx)MSDN 網站上。

當您執行 Visual Studio 2013 中 web 專案在偵錯模式中，如果您使用 Internet Explorer 作為您的瀏覽器時，您可以看到中的 proxy 檔案**方案總管**下方**指令碼文件**，如下所示下圖。

![在 [方案總管] 中的 JavaScript 產生之 proxy 檔案](hubs-api-guide-javascript-client/_static/image1.png)

若要查看檔案的內容，請按兩下**中樞**。 如果您不使用 Visual Studio 2012 或 2013年和 Internet Explorer 中，或如果您不在偵錯模式中，您也可以瀏覽至 「 signalR/中樞 」 的 URL 來取得檔案的內容。 例如，如果您的站台還在執行`http://localhost:56699`，請移至`http://localhost:56699/SignalR/hubs`瀏覽器中。

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a>如何建立 signalr 的實體檔案產生 proxy

為動態產生之 proxy 的替代方案，您可以建立 proxy 程式碼的實體檔案，並參考該檔案。 您可以控制快取或統合的行為，如，或當您撰寫的程式碼對伺服器方法的呼叫時，取得 IntelliSense。

若要建立 proxy 檔案，請執行下列步驟：

1. 安裝[Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet 套件。
2. 開啟命令提示字元並瀏覽至*工具*SignalR.exe 檔案所在的資料夾。 [工具] 資料夾位於下列位置：

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.2.1.0\tools`
3. 輸入下列命令：

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    通往您 *.dll*通常*bin*專案資料夾中的資料夾。

    此命令會建立名為的檔案*server.js*相同的資料夾中*signalr.exe*。
4. 放*server.js*檔案中適當的資料夾，在您的專案、 將它重新命名為適用於您的應用程式，並新增對它的參考，來取代 「 signalr/中樞 」 參考。

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>如何建立連線

您可以建立連線之前，您必須建立連線物件、 建立 proxy，並註冊事件處理常式，您可以從伺服器呼叫的方法。 設定 proxy 和事件處理常式，來建立此連接呼叫`start`方法。

如果您使用產生的 proxy，您不需要在自己的程式碼中建立的連接物件，因為產生的 proxy 程式碼會為您。

<a id="nogenconnection"></a>

**（與產生的 proxy) 建立連線**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

**建立連線 （不含產生的 proxy)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

範例程式碼會使用預設值"/ signalr 」 來連線到您的 SignalR 服務的 URL。 如需有關如何指定不同的基底 URL 的資訊，請參閱 < [ASP.NET SignalR 中樞 API 指南-Server-/signalr URL](hubs-api-guide-server.md#signalrurl)。

根據預設，中樞位置是目前的伺服器;如果您要連接到不同的伺服器，指定的 URL，然後再呼叫`start`方法，如下列範例所示：

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample10.js)]

> [!NOTE]
> 通常您會註冊事件處理常式，然後再呼叫`start`方法來建立連線。 如果您想要註冊一些事件處理常式建立連接後，您可以這麼做，但您必須註冊您的事件處理常式，然後再呼叫其中`start`方法。 這個的其中一個原因是應用程式，可以有許多的中樞，但您不想要觸發`OnConnected`上每個中樞，如果您打算只使用其中一個事件。 中樞 proxy 上的用戶端方法的存在時建立連線時，是什麼告訴 SignalR 來觸發`OnConnected`事件。 如果您未註冊任何事件處理常式，然後再呼叫`start`方法中，您將能夠叫用方法，在中樞中，但是中樞的`OnConnected`方法不會被呼叫，而且沒有用戶端會叫用方法從伺服器。


<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a>$。 connection.hub 是相同物件會建立該 $.hubConnection()

當您使用產生的 proxy 時，您可以看到範例中，從`$.connection.hub`參考的連接物件。 這是您藉由呼叫取得的相同物件`$.hubConnection()`時您不使用產生的 proxy。 產生的 proxy 程式碼會執行下列陳述式來建立您的連接：

![在產生之 proxy 檔案中建立的連線](hubs-api-guide-javascript-client/_static/image3.png)

當您使用產生的 proxy 時，您可以執行任何動作`$.connection.hub`，您可以使用連接物件時您不使用產生的 proxy。

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a>非同步執行的 start 方法

`start`方法以非同步方式執行。 它會傳回[jQuery 延後物件](http://api.jquery.com/category/deferred-object/)，這表示您可以藉由呼叫方法，像是新增回呼函式`pipe`， `done`，和`fail`。 如果您有您想要在連線建立之後執行的程式碼，例如伺服器方法呼叫，該程式碼放入回呼函式，或從回呼函式呼叫它。 `.done`回呼方法之後所建立的連線，以及任何程式碼之後，您必須執行您`OnConnected`事件處理常式方法，在伺服器上的完成執行。

如果您將 「 現在已連線 」 陳述式將前述範例中為程式碼之後的下一行`start`方法呼叫 (不是在`.done`回呼)，則`console.log`列將之前，先執行連線，如下列所示範例：

![錯誤的方式，撰寫程式碼執行連線之後，](hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a>如何建立跨網域的連線

通常如果瀏覽器載入頁面上，從`http://contoso.com`，SignalR 連線位於相同的網域， `http://contoso.com/signalr`。 如果頁面上，從`http://contoso.com`會連接到`http://fabrikam.com/signalr`，也就是跨網域的連線。 基於安全性理由，預設會停用跨網域的連線。

Signalr 1.x 中的，跨網域要求是由單一 EnableCrossDomain 旗標控制。 這個旗標控制 JSONP 及 CORS 要求。 所有的 CORS 支援較大的彈性，已經移除了 SignalR 的伺服器元件 （JavaScript 用戶端仍然使用 CORS 通常如果它偵測到瀏覽器支援它），以及新的 OWIN 中介軟體已可支援這些案例。

如果 JSONP 需要在用戶端 （以支援跨網域要求在較舊的瀏覽器中），它必須明確啟用 splittunneling`EnableJSONP`上`HubConfiguration`物件到`true`，如下所示。 JSONP 是預設為停用，其會比 CORS 較不安全。

**Microsoft.Owin.Cors 加入您的專案：** 若要安裝此程式庫，請在 Package Manager Console 執行下列命令：

`Install-Package Microsoft.Owin.Cors`

此命令會新增 2.1.0 版本的套件至您的專案。

### <a name="calling-usecors"></a>呼叫 UseCors

 下列程式碼片段示範如何實作 SignalR 2 中的跨網域連接。

**實作 SignalR 2 中的跨網域要求**

下列程式碼示範如何啟用 CORS 或 JSONP SignalR 2 專案中。 此程式碼範例會使用`Map`並`RunSignalR`而不是`MapSignalR`，如此一來，只會針對需要 CORS 支援 SignalR 要求的 CORS 中介軟體會執行 (而不是針對在指定的路徑上的所有流量`MapSignalR`。)對應也可以用於任何其他中介軟體需要執行特定的 URL 前置詞，而不是整個應用程式。

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample11.cs)]

> [!NOTE]
>
> - 不需要設定`jQuery.support.cors`設為 true，在您的程式碼。
>
>     ![未將 jQuery.support.cors 設定為 true](hubs-api-guide-javascript-client/_static/image7.png)
>
>     SignalR 處理 CORS 的使用。 設定`jQuery.support.cors`來為 true，則停用 JSONP，因為這會導致 SignalR 假設瀏覽器支援 CORS。
> - 當您要連線至 localhost URL 時，Internet Explorer 10 不會將它視為跨網域連接，好讓應用程式能夠在本機搭配 IE 10 即使您尚未啟用伺服器上的跨網域連線。
> - 如需使用 Internet Explorer 9 中的跨網域的連線資訊，請參閱[這個 StackOverflow 執行緒](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work)。
> - 如需使用 Chrome 中的跨網域的連線資訊，請參閱[這個 StackOverflow 執行緒](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome)。
> - 範例程式碼會使用預設值"/ signalr 」 來連線到您的 SignalR 服務的 URL。 如需有關如何指定不同的基底 URL 的資訊，請參閱 < [ASP.NET SignalR 中樞 API 指南-Server-/signalr URL](hubs-api-guide-server.md#signalrurl)。


<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>如何設定連線

建立連線之前，您可以指定查詢字串參數，或指定的傳輸方法。

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>如何指定查詢字串參數

如果您想要將資料傳送至伺服器，用戶端連線時，您可以加入連接物件來查詢字串參數。 下列範例示範如何在用戶端程式碼中設定查詢字串參數。

**在呼叫 start 方法 （使用產生的 proxy) 之前設定的查詢字串值**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

**設定查詢字串值，然後再呼叫 start 方法 （不含產生的 proxy)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample13.js?highlight=2)]

下列範例示範如何讀取伺服器程式碼中的查詢字串參數。

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample14.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>如何指定傳輸方法

連接的程序的一部分，通常與伺服器，以判斷最佳傳輸所支援的伺服器和用戶端交涉，SignalR 用戶端。 如果您已經知道您想要使用哪一個的傳輸，您可以略過此交涉程序指定的傳輸方法，當您呼叫`start`方法。

**指定的傳輸方法 （產生的 proxy) 的用戶端程式碼**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample15.js?highlight=1)]

**指定傳輸方法 （不含產生的 proxy) 的用戶端程式碼**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample16.js?highlight=2)]

或者，您可以指定多個傳輸方法，您想要嘗試的 SignalR 的順序：

**指定自訂傳輸後援配置 （使用產生的 proxy) 的用戶端程式碼**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

**指定自訂傳輸後援配置 （不含產生的 proxy) 的用戶端程式碼**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

您可以使用下列值來指定傳輸方法：

- 「 webSockets"
- 「 foreverFrame"
- "serverSentEvents"
- 「 longPolling"

下列範例顯示如何找出連接正在使用哪一種傳輸方法。

**顯示連接 （使用產生的 proxy) 所使用的傳輸方法的用戶端程式碼**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample19.js?highlight=2)]

**顯示連接 （不含產生的 proxy) 所使用的傳輸方法的用戶端程式碼**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample20.js?highlight=3)]

如需如何檢查伺服端程式碼中的傳輸方法的資訊，請參閱[ASP.NET SignalR 中樞 API 指南-Server-如何取得用戶端的資訊，從內容屬性](hubs-api-guide-server.md#contextproperty)。 如需有關傳輸與後援的詳細資訊，請參閱[SignalR-傳輸和後援簡介](../getting-started/introduction-to-signalr.md#transports)。

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a>如何取得 Hub 類別的 proxy

每個您所建立的連線物件會封裝包含一或多個中樞類別 SignalR 服務的連接相關資訊。 與 Hub 類別進行通訊，您會使用 proxy 物件，您會建立您自己 （如果您不使用產生的 proxy） 或其為您產生。

在用戶端 proxy 的名稱會是中樞類別名稱的 camel 案例版本。 SignalR 會自動將這項變更，讓 JavaScript 程式碼可以符合 JavaScript 的慣例。

**在伺服器上的中樞類別**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample21.cs?highlight=1)]

**取得產生的用戶端 proxy 的參考中樞**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample22.js?highlight=1)]

**建立中樞類別 （不含產生的 proxy) 的用戶端 proxy**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

如果裝飾您的中樞類別與`HubName`屬性，請使用完整名稱，而不變更大小寫。

**HubName 屬性的伺服器上的中樞類別**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample24.cs?highlight=1)]

**取得產生的用戶端 proxy 的參考中樞**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample25.js?highlight=1)]

**建立中樞類別 （不含產生的 proxy) 的用戶端 proxy**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>如何定義伺服器可以呼叫用戶端上的方法

若要定義伺服器可以從中樞呼叫的方法，加入事件處理常式的中樞 proxy 使用`client`屬性產生的 proxy，或是呼叫`on`方法，如果您未使用產生的 proxy。 參數可以是複雜物件。

新增事件處理常式，然後再呼叫`start`方法來建立連線。 (如果您想要新增事件處理常式之後呼叫`start`方法，請參閱中的附註[如何建立連接](#establishconnection)稍早在此文件，並使用定義方法，而不需使用產生的 proxy 所顯示的語法。)

方法名稱比對不區分大小寫。 例如，`Clients.All.addContosoChatMessageToPage`伺服器上將會執行`AddContosoChatMessageToPage`， `addContosoChatMessageToPage`，或`addcontosochatmessagetopage`用戶端上。

**定義用戶端 （使用產生的 proxy) 上的方法**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample27.js?highlight=2)]

**若要定義方法 （具有產生的 proxy) 用戶端上的替代方式**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample28.js?highlight=1-2)]

**定義方法，在用戶端 （而不需要產生的 proxy，或新增之後呼叫 start 方法時）**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample29.js?highlight=3)]

**伺服端程式碼，以呼叫用戶端方法**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample30.cs?highlight=5)]

下列範例中包含複雜物件作為方法參數。

**定義方法，採用複雜的物件 （與產生的 proxy) 的用戶端上**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample31.js?highlight=2-3)]

**定義可接受 （不含產生的 proxy) 的複雜物件的用戶端上的方法**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample32.js?highlight=3-4)]

**定義的複雜物件的伺服器程式碼**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample33.cs?highlight=1)]

**伺服端程式碼會呼叫使用複雜物件的用戶端方法**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample34.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>如何從用戶端呼叫伺服器方法

若要從用戶端呼叫伺服器方法，使用`server`屬性所產生的 proxy 或`invoke`中樞 proxy，如果您未使用產生的 proxy 上的方法。 傳回值或參數可以是複雜的物件。

在中樞傳入的方法名稱的 camel 命名法大小寫版本。 SignalR 會自動將這項變更，讓 JavaScript 程式碼可以符合 JavaScript 的慣例。

下列範例會示範如何呼叫伺服器方法沒有傳回值，以及如何呼叫沒有傳回值為伺服器方法。

**伺服器具有屬性的方法沒有 HubMethodName**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample35.cs?highlight=3)]

**伺服端程式碼定義複雜的物件傳入的參數**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample36.cs)]

**用戶端程式碼叫用伺服器方法 （使用產生的 proxy)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample37.js?highlight=1)]

**用戶端程式碼叫用伺服器方法 （不含產生的 proxy)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample38.js?highlight=1)]

如果裝飾適用的中樞方法的`HubMethodName`屬性，請使用該名稱，而不變更大小寫。

**伺服器方法**HubMethodName 屬性

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample39.cs?highlight=3)]

**用戶端程式碼叫用伺服器方法 （使用產生的 proxy)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

**用戶端程式碼叫用伺服器方法 （不含產生的 proxy)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample41.js?highlight=1)]

上述範例示範如何呼叫沒有傳回值為伺服器方法。 下列範例示範如何呼叫伺服器方法，其傳回值。

**伺服端程式碼之方法的傳回值**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample42.cs?highlight=3)]

**用於的庫存類別**傳回值

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample43.cs?highlight=1)]

**用戶端程式碼叫用伺服器方法 （使用產生的 proxy)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample44.js?highlight=2,4-5)]

**用戶端程式碼叫用伺服器方法 （不含產生的 proxy)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample45.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>如何處理連接的存留期事件

SignalR 提供以下連線，您可以處理存留期事件：

- `starting`：透過連線傳送任何資料之前引發。
- `received`：在此連接上收到任何資料時，就會引發。 提供已接收的資料。
- `connectionSlow`：當用戶端偵測到較慢或經常卸除連接時引發。
- `reconnecting`：基礎傳輸可讓您開始重新連線時引發。
- `reconnected`：當基礎傳輸已重新連接時引發。
- `stateChanged`：連線狀態變更時引發。 提供的舊狀態和新的狀態 （連接、 已連線、 正在重新連線或已中斷連線）。
- `disconnected`：當連接已中斷連線時，就會引發。

例如，如果您想要顯示警告訊息，可能會造成明顯延遲的連線問題時，處理`connectionSlow`事件。

**（使用產生的 proxy) 來處理 connectionSlow 事件**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample46.js?highlight=1)]

**處理 connectionSlow 事件 （不含產生的 proxy)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample47.js?highlight=2)]

如需詳細資訊，請參閱 <<c0> [ 了解和處理連線存留期事件 SignalR](handling-connection-lifetime-events.md)。

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>如何處理錯誤

SignalR JavaScript 用戶端提供`error`可以加入的處理常式的事件。 您也可以使用 fail 方法將從伺服器方法引動過程所產生的錯誤處理常式。

如果您未明確啟用在伺服器上的詳細的錯誤訊息，SignalR 在發生錯誤之後所傳回的例外狀況物件包含有關錯誤的最少資訊。 例如，如果呼叫`newContosoChatMessage`失敗，錯誤物件中的錯誤訊息包含 「`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`」 傳送詳細的錯誤訊息，以在生產環境中的用戶端不會建議基於安全性理由，但如果您想要啟用的詳細的錯誤訊息疑難排解的目的，在伺服器上使用下列程式碼。

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample48.cs?highlight=2)]

下列範例示範如何新增錯誤事件的處理常式。

**新增錯誤處理常式 （使用產生的 proxy)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample49.js?highlight=1)]

**新增錯誤處理常式 （而不需要產生的 proxy)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample50.js?highlight=2)]

下列範例示範如何處理來自方法引動過程的錯誤。

**控制代碼 （與產生的 proxy) 的方法引動過程發生錯誤**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample51.js?highlight=2)]

**控制代碼 （不含產生的 proxy) 的方法引動過程發生錯誤**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

如果在方法引動過程失敗，`error`也會引發事件，因此您的程式碼`error`方法處理常式和`.fail`方法回呼會執行。

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>如何啟用用戶端記錄

若要啟用用戶端登入連線，請設定`logging`屬性上的連接物件，然後再呼叫`start`方法來建立連線。

**啟用記錄 （與產生的 proxy)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample53.js?highlight=1)]

**啟用記錄功能 （不含產生的 proxy)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

若要查看記錄檔，開啟您的瀏覽器開發人員工具，並移至 [主控台] 索引標籤。顯示的逐步指示和螢幕擷取畫面示範如何執行這項操作的教學課程，請參閱[伺服器廣播與 ASP.NET Signalr-Enable Logging](../getting-started/tutorial-server-broadcast-with-signalr.md#enablelogging)。
