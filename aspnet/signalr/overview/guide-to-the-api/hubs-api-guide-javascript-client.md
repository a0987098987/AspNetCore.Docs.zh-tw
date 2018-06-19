---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
title: ASP.NET SignalR 中樞 API 指南 JavaScript 用戶端 |Microsoft 文件
author: pfletcher
description: 本文件提供適用於第 2 版中 JavaScript 用戶端，例如瀏覽器和 Windows 市集 (WinJS) applicat SignalR 使用集線器 API 的簡介...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/28/2015
ms.topic: article
ms.assetid: a9fd4dc0-1b96-4443-82ca-932a5b4a8ea4
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 794ab576d3c6600911f331bab7c335476e45a0c9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
ms.locfileid: "28035331"
---
<a name="aspnet-signalr-hubs-api-guide---javascript-client"></a>ASP.NET SignalR 中樞 API 指南 JavaScript 用戶端
====================
由[Patrick Fletcher](https://github.com/pfletcher)， [Tom Dykstra](https://github.com/tdykstra)

> 本文件提供適用於 SignalR 第 2 版中 JavaScript 用戶端，例如瀏覽器和 Windows 市集 (WinJS) 應用程式使用中樞 API 的簡介。
> 
> SignalR 中樞應用程式開發介面可讓您從伺服器連線的用戶端和伺服器的用戶端進行遠端程序呼叫 (Rpc)。 伺服端程式碼，並定義用戶端，可以呼叫的方法，呼叫用戶端執行的方法。 用戶端程式碼，並定義可在伺服器上，從呼叫的方法，您呼叫在伺服器執行的方法。 SignalR 會負責為您的用戶端-伺服器一切細節。
> 
> SignalR 也能提供較低層級應用程式開發介面呼叫持續連線。 如需 SignalR、 集線器及持續連線的簡介，請參閱[簡介 SignalR](../getting-started/introduction-to-signalr.md)。
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

本文件包含下列章節：

- [產生的 proxy，它會為您](#genproxy)

    - [使用產生的 proxy 的時機](#cantusegenproxy)
- [用戶端安裝](#clientsetup)

    - [如何參考動態產生的 proxy](#dynamicproxy)
    - [如何建立 SignalR 的實體檔案產生 proxy](#manualproxy)
- [如何建立連接](#establishconnection)

    - [$。 connection.hub 是相同的物件會建立該 $.hubConnection()](#connequivalence)
    - [非同步執行的 start 方法](#asyncstart)
- [如何建立跨定義域連線](#crossdomain)
- [如何設定連接](#configureconnection)

    - [如何指定查詢字串參數](#querystring)
    - [如何指定傳輸方法](#transport)
- [如何取得 Hub 類別的 proxy](#getproxy)
- [如何定義伺服器可以呼叫的用戶端上的方法](#callclient)
- [如何從用戶端呼叫伺服器方法](#callserver)
- [如何處理連接的存留期事件](#connectionlifetime)
- [如何處理錯誤](#handleerrors)
- [如何啟用用戶端記錄](#logging)

如需如何將伺服器或.NET 用戶端進行程式設計的文件，請參閱下列資源：

- [SignalR 中樞 API 指南-伺服器](hubs-api-guide-server.md)
- [SignalR 中樞 API 指南-.NET 用戶端](hubs-api-guide-net-client.md)

（但沒有.NET 用戶端的 SignalR 2.NET 4.0），才可用在.NET 4.5 SignalR 2 伺服器元件。

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a>產生的 proxy，它會為您

您可以程式進行通訊與 SignalR 服務或不使用 proxy 為您產生 SignalR JavaScript 用戶端。 Proxy 會為您是簡化您用來連接，伺服器會呼叫寫入方法，並在伺服器上呼叫方法的程式碼的語法。

當您撰寫程式碼呼叫伺服器方法時，產生的 proxy 可讓您使用看起來像是您執行了區域函式的語法： 您可以撰寫`serverMethod(arg1, arg2)`而不是`invoke('serverMethod', arg1, arg2)`。 如果您輸入錯誤的伺服器方法名稱，產生的 proxy 語法也可讓立即和可理解的用戶端時發生錯誤。 如果您手動建立定義 proxy 的檔案，則您也可以撰寫程式碼呼叫伺服器方法取得 IntelliSense 支援。

例如，假設您有下列的中樞類別在伺服器上：

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

下列程式碼範例顯示的 JavaScript 程式碼看起來像叫用`NewContosoChatMessage`方法在伺服器和收到的引動過程上`addContosoChatMessageToPage`來自伺服器的方法。

**使用產生的 proxy**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

**不產生的 proxy**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a>使用產生的 proxy 的時機

如果您想要註冊的伺服器呼叫的用戶端方法的多個事件處理常式，您無法使用產生的 proxy。 否則，您可以選擇使用產生的 proxy，或未根據您的程式碼撰寫喜好設定。 如果您選擇不使用它，您不需要參考中的"signalr/中樞 」 URL`script`用戶端程式碼中的項目。

<a id="clientsetup"></a>

## <a name="client-setup"></a>用戶端安裝

JavaScript 用戶端需要 jQuery 和 SignalR core JavaScript 檔案的參考。 1.6.4 或主要的更新版本，例如 1.7.2、 1.8.2 或 1.9.1，必須是 jQuery 版本。 如果您決定使用產生的 proxy，您也需要 SignalR 產生 proxy JavaScript 檔案的參考。 下列範例顯示參考在 HTML 頁面會使用產生的 proxy，可能會尋找。

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample4.html)]

這些參考必須包含在此順序： jQuery SignalR core 之後，與 SignalR proxy first、 last。

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a>如何參考動態產生的 proxy

在上述範例中，產生的 SignalR proxy 的參考是動態產生的 JavaScript 程式碼，不以實體檔案。 SignalR 的 JavaScript 程式碼建立即時 proxy 和做"signalr/中樞 」 URL 的回應中的用戶端。 如果您指定不同的基底 URL 的 SignalR 連線中伺服器上您`MapSignalR`方法，以動態方式產生的 proxy 檔案的 URL 是自訂的 URL 與"/ 集線器 」 附加。

> [!NOTE]
> Windows 8 （Windows 市集） JavaScript 用戶端，請使用實體的 proxy 檔案而不是動態產生。 如需詳細資訊，請參閱[如何建立 SignalR 的實體檔案產生 proxy](#manualproxy)本主題稍後。


在 ASP.NET MVC 4 或 5 Razor 檢視，請參考將 proxy 檔案參考中的應用程式根目錄使用波狀符號：

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample5.html)]

如需使用 MVC 5 中的 SignalR 的詳細資訊，請參閱[SignalR 和 MVC 5 入門](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)。

在 ASP.NET MVC 3 Razor 檢視中，使用`Url.Content`供您的 proxy 檔案參考：

[!code-cshtml[Main](hubs-api-guide-javascript-client/samples/sample6.cshtml)]

ASP.NET Web Form 應用程式中，使用`ResolveClientUrl`檔案參考您的 proxy，或透過使用應用程式根目錄相對路徑 （開頭波狀符號） ScriptManager 註冊它：

[!code-aspx[Main](hubs-api-guide-javascript-client/samples/sample7.aspx)]

一般而言，使用相同的方法來指定您用於 CSS 或 JavaScript 檔案的 「 signalr/中樞 」 URL。 如果您指定的 URL，而不使用波狀符號，在某些情況下您的應用程式正常運作時，您在 Visual Studio 中使用 IIS Express 測試，但部署至完整 IIS 時，會因 404 錯誤。 如需詳細資訊，請參閱**根層級資源的解析參考**中[ASP.NET Web 專案的 Visual Studio 中的 Web 伺服器](https://msdn.microsoft.com/library/58wxa9w5.aspx)MSDN 網站上的。

當您執行 Visual Studio 2013 中 web 專案在偵錯模式中，如果您使用 Internet Explorer 瀏覽器，您可以看到在 proxy 檔**方案總管 中**下**指令碼文件**，如下所示下圖。

![在 [方案總管] 的 JavaScript 產生之 proxy 檔案](hubs-api-guide-javascript-client/_static/image1.png)

若要查看檔案的內容，請按兩下**集線器**。 如果您不使用 Visual Studio 2012 或 2013年和 Internet Explorer 中，或您不在偵錯模式，您也可以瀏覽至 「 signalR/中樞 」 URL 取得檔案的內容。 例如，如果您的網站執行在`http://localhost:56699`，請移至`http://localhost:56699/SignalR/hubs`瀏覽器中。

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a>如何建立 SignalR 的實體檔案產生 proxy

為動態產生之 proxy 的替代方案，您可以建立 proxy 程式碼的實體檔案，並參照該檔案。 您可以這樣做，以便控制快取，或結合在一起的行為，或當您在撰寫程式碼對伺服器方法呼叫時使用 IntelliSense。

若要建立 proxy 檔案時，執行下列步驟：

1. 安裝[Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet 封裝。
2. 開啟命令提示字元並瀏覽至*工具*SignalR.exe 檔案所在的資料夾。 [工具] 資料夾位於下列位置：

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.2.1.0\tools`
3. 輸入下列命令：

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    路徑您 *.dll*通常是*bin*專案資料夾中的資料夾。

    此命令會建立名為*立即轉譯 server.js*相同資料夾中*signalr.exe*。
4. Put*立即轉譯 server.js*檔中適當的資料夾，在您的專案中，視您的應用程式，將它重新命名並加入它的參考取代"signalr/中樞"參考。

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>如何建立連接

您可以建立連線之前，您必須建立連線物件、 建立 proxy 和註冊事件處理常式可以從伺服器呼叫的方法。 設定 proxy 和事件處理常式，來建立連接呼叫`start`方法。

如果您使用產生的 proxy，您不需要在自己的程式碼中建立的連接物件，因為產生的 proxy 程式碼會為您。

<a id="nogenconnection"></a>

**（與產生的 proxy) 建立連線**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

**建立連線 （不含產生的 proxy)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

範例程式碼會使用預設 「 / signalr 」 來連線到 SignalR 服務的 URL。 如需如何指定不同的基底 URL，請參閱[ASP.NET SignalR 中樞 API 指南-伺服器-/signalr URL](hubs-api-guide-server.md#signalrurl)。

根據預設，中樞位置是目前的伺服器。如果您要連接到不同的伺服器，指定的 URL，然後再呼叫`start`方法，如下列範例所示：

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample10.js)]

> [!NOTE]
> 通常您會註冊事件處理常式，然後再呼叫`start`方法，以建立連接。 如果您想要註冊一些事件處理常式建立連接後，您可以這樣做，但您必須註冊至少其中一個會在呼叫之前您事件而`start`方法。 其中一個原因是應用程式中，可以有許多集線器，但不會想要觸發`OnConnected`上每個中樞，如果您打算只使用其中一個事件。 建立連接時，中樞 proxy 上的用戶端方法的存在是什麼告知觸發 SignalR`OnConnected`事件。 如果您未註冊任何事件處理常式，然後再呼叫`start`方法，您將能夠叫用中樞，但是中樞的方法`OnConnected`方法不會被呼叫，而且沒有用戶端將會叫用方法從伺服器。


<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a>$。 connection.hub 是相同的物件會建立該 $.hubConnection()

當您使用產生的 proxy 時，您可以看到範例中，從`$.connection.hub`指的是連接物件。 這是您藉由呼叫取得的相同物件`$.hubConnection()`時您不使用產生的 proxy。 產生的 proxy 程式碼會執行下列陳述式來建立您的連接：

![產生的 proxy 檔案中建立的連接](hubs-api-guide-javascript-client/_static/image3.png)

當您使用產生的 proxy 時，您可以執行任何動作`$.connection.hub`，您可以使用連接物件時您不使用產生的 proxy。

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a>非同步執行的 start 方法

`start`方法以非同步方式執行。 它會傳回[jQuery 延期物件](http://api.jquery.com/category/deferred-object/)，這表示，您可以加入回呼函式所呼叫的方法，例如`pipe`， `done`，和`fail`。 如果您想要執行連線建立之後的程式碼，例如伺服器方法呼叫，將該程式碼中的回呼函式，或從回呼函式中呼叫它。 `.done`回呼方法執行之後已建立的連線，以及任何程式碼之後，您必須您`OnConnected`執行完成在伺服器上的事件處理常式方法。

如果您將 「 立即連線 」 陳述式前面範例中的為下一行程式碼之後`start`方法呼叫 (不在`.done`回呼)、`console.log`會執行列之前已建立的連線，如下列所示範例：

![錯誤的方式來撰寫執行連線建立之後的程式碼](hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a>如何建立跨定義域連線

通常如果瀏覽器中載入的頁面上，從`http://contoso.com`，SignalR 連線是在相同網域中，在`http://contoso.com/signalr`。 如果將頁面從`http://contoso.com`會連接到`http://fabrikam.com/signalr`，也就是跨網域的連接。 基於安全性理由，預設會停用跨網域連線。

SignalR 1.x，跨網域要求由單一 EnableCrossDomain 中的旗標。 這個旗標控制 JSONP 及 CORS 要求。 所有的 CORS 支援較大的彈性，已經移除了 SignalR 的伺服器元件 （JavaScript 用戶端仍然使用 CORS 通常如果它偵測到瀏覽器支援它），而且新的 OWIN 中介軟體進行可用來支援這些案例。

如果 JSONP 需要在用戶端 （以支援跨網域要求中舊的瀏覽器），它必須明確啟用藉由設定`EnableJSONP`上`HubConfiguration`物件`true`，如下所示。 JSONP 已停用根據預設，因為它比 CORS 較不安全。

**專案中加入 Microsoft.Owin.Cors:** 若要安裝此程式庫，請在 Package Manager Console 中執行下列命令：

`Install-Package Microsoft.Owin.Cors`

此命令將新增 2.1.0 封裝至您的專案版本。

### <a name="calling-usecors"></a>呼叫 UseCors

 下列程式碼片段示範如何實作 SignalR 2 中的跨網域連接。 

**實作 SignalR 2 中的跨網域要求**

下列程式碼會示範如何啟用 CORS 或 JSONP SignalR 2 專案中。 這個程式碼範例會使用`Map`和`RunSignalR`而不是`MapSignalR`，如此一來，CORS 中介軟體執行只會針對需要 CORS 支援 SignalR 要求 (而不會針對在指定的路徑上的所有流量`MapSignalR`。)地圖也可以用於任何其他中介軟體，必須執行特定的 URL 前置詞，而不是整個應用程式。

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample11.cs)]

> [!NOTE] 
> 
> - 不需要設定`jQuery.support.cors`設為 true，在程式碼中。
> 
>     ![不會設定為 true jQuery.support.cors](hubs-api-guide-javascript-client/_static/image7.png)
> 
>     SignalR 處理 CORS 的使用。 設定`jQuery.support.cors`來為 true，則停用 JSONP，因為這會導致 SignalR 假設瀏覽器支援 CORS。
> - 當您要連線到 localhost URL 時，Internet Explorer 10 將不會考慮它跨網域連線，以便應用程式會在本機使用 IE 10 即使您沒有啟用伺服器上的跨網域連線。
> - 如需使用 Internet Explorer 9 中的跨網域的連接資訊，請參閱[這個 StackOverflow 執行緒](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work)。
> - 如需使用 Chrome 的跨網域的連接資訊，請參閱[這個 StackOverflow 執行緒](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome)。
> - 範例程式碼會使用預設 「 / signalr 」 來連線到 SignalR 服務的 URL。 如需如何指定不同的基底 URL，請參閱[ASP.NET SignalR 中樞 API 指南-伺服器-/signalr URL](hubs-api-guide-server.md#signalrurl)。


<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>如何設定連接

建立連線之前，您可以指定查詢字串參數，或指定的傳輸方法。

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>如何指定查詢字串參數

如果您想要在用戶端連線時，將資料傳送到伺服器，您可以查詢字串參數加入連接物件。 下列範例顯示如何在用戶端程式碼中設定的查詢字串參數。

**（與產生的 proxy) 呼叫 start 方法之前設定的查詢字串值**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

**設定查詢字串值，然後再呼叫 start 方法 （不含產生的 proxy)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample13.js?highlight=2)]

下列範例會示範如何讀取伺服器程式碼中的查詢字串參數。

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample14.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>如何指定傳輸方法

連接的程序的一部分，SignalR 用戶端通常會與交涉的伺服器和用戶端到伺服器來判斷最佳支援的傳輸。 如果您已經知道您想要使用的傳輸，您可以略過此交涉程序指定傳輸方法，當您呼叫`start`方法。

**指定傳輸方法 （具有產生的 proxy) 的用戶端程式碼**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample15.js?highlight=1)]

**指定傳輸方法 （不含產生的 proxy) 的用戶端程式碼**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample16.js?highlight=2)]

或者，您可以指定多個傳輸方法，您想要再試一次他們的 SignalR 的順序：

**指定自訂傳輸後援配置 （與產生的 proxy) 的用戶端程式碼**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

**指定自訂傳輸後援配置 （不含產生的 proxy) 的用戶端程式碼**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

您可以使用下列值來指定傳輸方法：

- "webSockets"
- "foreverFrame"
- "serverSentEvents"
- "longPolling"

下列範例顯示如何找出連接正在使用哪一種傳輸方法。

**顯示連接 （使用產生的 proxy) 所使用的傳輸方法的用戶端程式碼**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample19.js?highlight=2)]

**顯示連線 （不含產生的 proxy) 所使用的傳輸方法的用戶端程式碼**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample20.js?highlight=3)]

如需如何檢查伺服端程式碼中的傳輸方法的資訊，請參閱[ASP.NET SignalR 中樞 API 指南-伺服器-如何取得用戶端的相關資訊的內容屬性從](hubs-api-guide-server.md#contextproperty)。 如需傳輸與後援的詳細資訊，請參閱[簡介 SignalR 的傳輸與後援](../getting-started/introduction-to-signalr.md#transports)。

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a>如何取得 Hub 類別的 proxy

您建立每個連接物件會封裝包含一或多個 Hub 類別的 SignalR 服務的連接相關資訊。 中樞類別與通訊，您會使用 proxy 物件，您自行建立 （如果您不使用產生的 proxy） 或為您所產生。

在用戶端 proxy 名稱會是中樞類別名稱的 camel 案例版本。 SignalR 會自動將這項變更，讓 JavaScript 程式碼可以符合 JavaScript 慣例。

**在伺服器上的中樞類別**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample21.cs?highlight=1)]

**取得中樞的產生的用戶端 proxy 的參考**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample22.js?highlight=1)]

**建立 Hub 類別 （而不產生的 proxy) 的用戶端的 proxy**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

如果裝飾中樞類別`HubName`屬性，請使用完整名稱，而不會變更大小寫。

**具有 HubName 屬性的伺服器上的中樞類別**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample24.cs?highlight=1)]

**取得中樞的產生的用戶端 proxy 的參考**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample25.js?highlight=1)]

**建立 Hub 類別 （而不產生的 proxy) 的用戶端的 proxy**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>如何定義伺服器可以呼叫的用戶端上的方法

若要定義伺服器可以呼叫與中樞的方法，加入事件處理常式的中樞 proxy 使用`client`屬性產生的 proxy，或呼叫`on`方法，如果您不使用產生的 proxy。 參數可以是複雜的物件。

加入事件處理常式，才能呼叫`start`方法，以建立連接。 (如果您想要加入事件處理常式後呼叫`start`方法，請參閱中的附註[如何建立連接](#establishconnection)稍早在此文件，並使用定義方法，而不使用產生的 proxy 所顯示的語法。)

方法名稱比對不區分大小寫。 例如，`Clients.All.addContosoChatMessageToPage`在伺服器上將會執行`AddContosoChatMessageToPage`， `addContosoChatMessageToPage`，或`addcontosochatmessagetopage`用戶端上。

**（與產生的 proxy) 的用戶端上定義的方法**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample27.js?highlight=2)]

**定義方法 （具有產生的 proxy) 用戶端上的替代方式**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample28.js?highlight=1-2)]

**（不產生的 proxy，或加入之後呼叫 start 方法時） 的用戶端上定義的方法**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample29.js?highlight=3)]

**伺服端程式碼呼叫的方法，用戶端**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample30.cs?highlight=5)]

下列範例中包含複雜的物件做為方法參數。

**採用的複雜物件 （產生的 proxy) 的用戶端上定義的方法**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample31.js?highlight=2-3)]

**會採用複雜物件 （不含產生的 proxy) 的用戶端上定義的方法**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample32.js?highlight=3-4)]

**定義的複雜物件的伺服器程式碼**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample33.cs?highlight=1)]

**伺服端程式碼呼叫的用戶端方法，使用複雜的物件**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample34.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>如何從用戶端呼叫伺服器方法

若要從用戶端呼叫伺服器方法，使用`server`屬性產生的 proxy 或`invoke`方法上的中樞 proxy，如果您不使用產生的 proxy。 傳回值或參數可以是複雜的物件。

方法名稱的 camel 案例版本通過集線器上。 SignalR 會自動將這項變更，讓 JavaScript 程式碼可以符合 JavaScript 慣例。

下列範例顯示如何呼叫沒有傳回值為伺服器方法，以及如何呼叫沒有傳回值為伺服器方法。

**伺服器具有屬性的方法沒有 HubMethodName**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample35.cs?highlight=3)]

**伺服端程式碼定義的複雜物件傳入參數**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample36.cs)]

**叫用伺服器方法 （具有產生的 proxy) 的用戶端程式碼**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample37.js?highlight=1)]

**叫用伺服器 （不含方法產生的 proxy) 的用戶端程式碼**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample38.js?highlight=1)]

如果裝飾中樞方法與`HubMethodName`屬性，請使用該名稱而不會變更大小寫。

**伺服器方法**HubMethodName 屬性

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample39.cs?highlight=3)]

**叫用伺服器方法 （具有產生的 proxy) 的用戶端程式碼**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

**叫用伺服器 （不含方法產生的 proxy) 的用戶端程式碼**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample41.js?highlight=1)]

前面的範例示範如何呼叫沒有傳回值為伺服器方法。 下列範例會示範如何呼叫傳回的值為伺服器方法。

**伺服端程式碼具有傳回值的方法**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample42.cs?highlight=3)]

**用於的庫存類別**傳回值

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample43.cs?highlight=1)]

**叫用伺服器方法 （具有產生的 proxy) 的用戶端程式碼**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample44.js?highlight=2,4-5)]

**叫用伺服器 （不含方法產生的 proxy) 的用戶端程式碼**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample45.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>如何處理連接的存留期事件

SignalR 提供下列的連線，您可以控制它們的存留期事件：

- `starting`： 會透過連線傳送任何資料之前引發。
- `received`： 在此連接上收到任何資料時引發。 提供已接收的資料。
- `connectionSlow`： 用戶端偵測到低速或經常卸除連接時引發。
- `reconnecting`： 基礎傳輸可讓您開始重新連線時引發。
- `reconnected`： 基礎傳輸已重新連接時引發。
- `stateChanged`： 連線狀態變更時引發。 提供舊的狀態和新的狀態 （連線、 已連線、 Reconnecting 或已中斷連接）。
- `disconnected`： 當連接已中斷連接時引發。

例如，如果您想要連線的問題，可能會導致明顯的延遲時顯示警告訊息，處理`connectionSlow`事件。

**處理 connectionSlow 事件 （與產生的 proxy)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample46.js?highlight=1)]

**處理 connectionSlow 事件 （不含產生的 proxy)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample47.js?highlight=2)]

如需詳細資訊，請參閱[了解與 SignalR 中處理連接存留期事件](handling-connection-lifetime-events.md)。

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>如何處理錯誤

提供 SignalR JavaScript 用戶端`error`可以加入的處理常式的事件。 您也可以使用 fail 方法加入處理常式所導致從伺服器方法引動過程的錯誤。

如果您未明確啟用在伺服器上的詳細的錯誤訊息，在發生錯誤之後，傳回 SignalR 的例外狀況物件包含錯誤的相關基本資訊。 例如，如果呼叫`newContosoChatMessage`失敗，錯誤物件中的錯誤訊息包含 「`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`」 傳送詳細的錯誤訊息，以在生產環境中的用戶端不會建議基於安全性理由，但如果您想要啟用的詳細的錯誤訊息疑難排解用途，在伺服器上使用下列程式碼。

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample48.cs?highlight=2)]

下列範例會示範如何加入錯誤事件的處理常式。

**加入錯誤處理常式 （與產生的 proxy)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample49.js?highlight=1)]

**加入錯誤處理常式 （而不產生的 proxy)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample50.js?highlight=2)]

下列範例顯示如何處理來自方法引動過程的錯誤。

**控制代碼 （與產生的 proxy) 的方法引動過程發生錯誤**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample51.js?highlight=2)]

**控制代碼 （不含產生的 proxy) 的方法引動過程發生錯誤**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

如果方法引動過程失敗，`error`也會引發事件，因此您的程式碼`error`方法處理常式並在`.fail`回呼方法會執行。

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>如何啟用用戶端記錄

若要啟用用戶端登入連線，`logging`屬性上的連接物件，才能呼叫`start`方法，以建立連接。

**啟用記錄 （具有產生的 proxy)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample53.js?highlight=1)]

**啟用記錄功能 （不含產生的 proxy)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

若要查看記錄檔，開啟瀏覽器的開發人員工具並移至 [主控台] 索引標籤。教學課程中，顯示逐步指示和螢幕擷取畫面，顯示如何執行這項操作，請參閱[伺服器廣播透過 ASP.NET Signalr-Enable Logging](../getting-started/tutorial-server-broadcast-with-signalr.md#enablelogging)。
