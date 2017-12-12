---
uid: signalr/overview/testing-and-debugging/troubleshooting
title: "SignalR 疑難排解 |Microsoft 文件"
author: pfletcher
description: "本文說明開發 SignalR 應用程式的一般問題。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 4b559e6c-4fb0-4a04-9812-45cf08ae5779
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/testing-and-debugging/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: d7a1dcc04baaa5ab27aecf95936d943f5a9b3f0c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="signalr-troubleshooting"></a>SignalR 疑難排解
====================
由[Patrick Fletcher](https://github.com/pfletcher)

> 本文件說明與 SignalR 的常見疑難排解問題。
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


本文件包含下列各節。

- [呼叫以無訊息模式失敗的用戶端和伺服器之間的方法](#connection)
- [設定 IIS websockets，ping/pong 來偵測無作用的用戶端](#pong)
- [其他連線問題](#other)
- [編譯和伺服器端錯誤](#server)
- [Visual Studio 問題](#vs)
- [網際網路資訊服務的問題](#iis)
- [Microsoft Azure 問題](#azure)

<a id="connection"></a>

## <a name="calling-methods-between-the-client-and-server-silently-fails"></a>呼叫以無訊息模式失敗的用戶端和伺服器之間的方法

本章節描述針對方法呼叫失敗而無意義的錯誤訊息的用戶端與伺服器之間的可能原因。 中的 SignalR 應用程式中，伺服器會有任何資訊的方法，用戶端會實作;伺服器叫用時用戶端方法，方法名稱和參數資料傳送至用戶端，以及它存在於伺服器指定的格式時，才執行此方法。 如果沒有符合的方法上找到用戶端，會發生任何事，且沒有錯誤訊息會在伺服器上引發。

若要進一步調查的用戶端未呼叫的方法，您可以開啟之前呼叫，以查看哪些呼叫集線器上的 start 方法都來自伺服器的記錄。 若要啟用記錄至 JavaScript 應用程式中，請參閱[如何啟用用戶端記錄 （JavaScript 用戶端版本）](../guide-to-the-api/hubs-api-guide-javascript-client.md#logging)。 若要啟用.NET 用戶端應用程式中的記錄，請參閱[如何啟用用戶端記錄 （.NET 用戶端版本）](../guide-to-the-api/hubs-api-guide-net-client.md#logging)。

### <a name="misspelled-method-incorrect-method-signature-or-incorrect-hub-name"></a>拼錯的方法、 不正確的方法簽章或不正確的中樞名稱

如果名稱或呼叫方法的簽章不完全符合適當的方法，用戶端上，呼叫會失敗。 請確認伺服器所呼叫的方法名稱符合用戶端上方法的名稱。 此外，SignalR 建立中樞 proxy 使用 camel 命名法的大小寫慣例的方法，為適合在 JavaScript 中，因此呼叫的方法，`SendMessage`會呼叫在伺服器上`sendMessage`中用戶端 proxy。 如果您使用`HubName`屬性在伺服器端程式碼中，請確認所使用的名稱符合用來在用戶端建立中樞的名稱。 如果您未使用`HubName`屬性，請確認在 JavaScript 用戶端中樞的名稱是依照 camel 命名法的大小寫慣例，而不是 ChatHub chatHub 等。

### <a name="duplicate-method-name-on-client"></a>重複的用戶端上的方法名稱

請確認，您不需要重複的方法只有大小寫不同的用戶端上。 如果用戶端應用程式呼叫的方法`sendMessage`，確認沒有也稱為方法`SendMessage`以及。

### <a name="missing-json-parser-on-the-client"></a>在用戶端上遺漏的 JSON 剖析器

SignalR 要求必須有序列化呼叫伺服器與用戶端之間的 JSON 剖析器。 如果您的用戶端並沒有內建的 JSON 剖析器 （例如 Internet Explorer 7)，您必須包含在應用程式中。 您可以下載 JSON 剖析器[這裡](http://nuget.org/packages/json2)。

### <a name="mixing-hub-and-persistentconnection-syntax"></a>混用中樞 PersistentConnection 語法

SignalR 使用兩種通訊模式： 集線器和 PersistentConnections。 呼叫這兩個通訊模型的語法是在用戶端程式碼中的不同。 如果您在伺服器上的程式碼中加入集線器，請確認所有用戶端程式碼會使用適當的中樞語法。

**建立 PersistentConnection JavaScript 用戶端中的 JavaScript 用戶端程式碼**

[!code-javascript[Main](troubleshooting/samples/sample1.js)]

**建立 Javascript 用戶端中的中樞 Proxy 的 JavaScript 用戶端程式碼**

[!code-javascript[Main](troubleshooting/samples/sample2.js)]

**C# 伺服器程式碼會對應至 PersistentConnection 路由**

[!code-csharp[Main](troubleshooting/samples/sample3.cs)]

**C# 伺服器程式碼對應路由至中樞，或多個集線器如果您有多個應用程式**

[!code-css[Main](troubleshooting/samples/sample4.css)]

### <a name="connection-started-before-subscriptions-are-added"></a>連接啟動之後才會加入訂用帳戶

如果中樞的連線已啟動，可以從伺服器呼叫的方法加入至 proxy 之前，不會收到訊息。 下列 JavaScript 程式碼將不會啟動中樞正確：

**不正確用戶端的 JavaScript 程式碼將不允許接收的訊息中心**

[!code-javascript[Main](troubleshooting/samples/sample5.js)]

請改為加入的方法訂用帳戶呼叫開始之前：

**正確地將訂閱加入至中樞的 JavaScript 用戶端程式碼**

[!code-javascript[Main](troubleshooting/samples/sample6.js)]

### <a name="missing-method-name-on-the-hub-proxy"></a>中樞 proxy 上遺漏的方法名稱

請確認訂閱在用戶端上，在伺服器上定義的方法。 即使伺服器定義的方法，它必須仍加入至用戶端 proxy。 方法可以下列方式加入至用戶端 proxy (請注意，方法會加入至`client`成員的中樞，不中樞直接):

**將方法加入至中樞 proxy 的 JavaScript 用戶端程式碼**

[!code-javascript[Main](troubleshooting/samples/sample7.js)]

### <a name="hub-or-hub-methods-not-declared-as-public"></a>集線器或未宣告為公用的中樞方法

若要顯示在用戶端上，實作中樞和方法必須宣告為`public`。

### <a name="accessing-hub-from-a-different-application"></a>從不同的應用程式存取中樞

SignalR 中樞只在透過實作 SignalR 用戶端應用程式中存取。 SignalR 無法交互操作與其他的通訊程式庫 （例如 SOAP 或 WCF web 服務。）如果沒有可用的目標平台的 SignalR 用戶端，您無法直接存取伺服器的端點。

### <a name="manually-serializing-data"></a>以手動方式序列化的資料

SignalR 會自動使用 JSON 序列化程式方法的參數那里的不需要自行進行。

### <a name="remote-hub-method-not-executed-on-client-in-ondisconnected-function"></a>並未在 OnDisconnected 函式中的用戶端上執行的遠端中樞方法

這種行為是設計上的預期行為。 當`OnDisconnected`是呼叫，中樞已經輸入`Disconnected`不允許進一步的中樞方法呼叫的狀態。

**C# 伺服器正確執行的程式碼的程式碼 OnDisconnected 事件**

[!code-csharp[Main](troubleshooting/samples/sample8.cs)]

### <a name="ondisconnect-not-firing-at-consistent-times"></a>不一致的時間在引發 OnDisconnect

這種行為是設計上的預期行為。 當使用者嘗試巡覽離具有使用中的 SignalR 連線的網頁時，SignalR 用戶端接著便可通知用戶端連接將會停止伺服器的最佳嘗試。 如果 SignalR 用戶端的最大速率來連線伺服器的嘗試失敗，伺服器將處置之後可設定的連接`DisconnectTimeout`以後，在這段`OnDisconnected`事件就會引發。 如果 SignalR 用戶端的最佳嘗試會成功，`OnDisconnected`會立即引發事件。

如需設定資訊`DisconnectTimeout`設定，請參閱[處理連接的存留期事件： DisconnectTimeout](../guide-to-the-api/handling-connection-lifetime-events.md#disconnecttimeout)。

### <a name="connection-limit-reached"></a>已達到連線限制

當用戶端作業系統，例如 Windows 7 上使用 IIS 的完整版本，則會加上 10 連線限制。 當使用用戶端作業系統，請改用 IIS Express 若要避免這項限制。

### <a name="cross-domain-connection-not-set-up-properly"></a>適當地設定跨網域連線

如果跨網域連線 （連線的 SignalR URL 不是在與裝載網頁位於相同網域中） 未正確設定，連線可能會失敗，沒有錯誤訊息。 如需如何啟用跨網域通訊的資訊，請參閱[如何建立跨定義域連線](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)。

### <a name="connection-using-ntlm-active-directory-not-working-in-net-client"></a>使用 NTLM (Active Directory) 無法在.NET 用戶端中運作的連線

如果連接未正確設定，在.NET 用戶端應用程式中使用網域安全性的連接可能會失敗。 若要在網域環境中使用 SignalR，設定必要的連接屬性如下所示：

**C# 用戶端程式碼實作連線認證**

[!code-csharp[Main](troubleshooting/samples/sample9.cs)]

<a id="pong"></a>

## <a name="configuring-iis-websockets-to-pingpong-to-detect-a-dead-client"></a>設定 IIS websockets，ping/pong 來偵測無作用的用戶端

SignalR 的伺服器不知道，是否用戶端是無作用或不，而它們是依賴來自基礎 websocket 連線失敗的通知，也就是 OnClose 回呼。 這個問題的解決方案之一是設定為您執行 ping/pong IIS websocket。 這可確保如果意外地中斷，將會關閉您的連線。 如需詳細資訊，請參閱[這 stackoverflow 文章](http://stackoverflow.com/questions/19502755/websocket-clients-state-not-changing-on-network-loss)。

<a id="other"></a>

## <a name="other-connection-issues"></a>其他連線問題

本章節描述的原因和解決方案的特定徵狀或在連接期間發生的錯誤訊息。

### <a name="start-must-be-called-before-data-can-be-sent-error"></a>「 傳送資料之前，必須呼叫啟動 」 錯誤

如果程式碼會參考 SignalR 物件啟動連接之前，通常會出現此錯誤。 處理常式和之類裝設，呼叫在伺服器上定義的方法必須將連接完成後。 請注意，呼叫`Start`是非同步的因此程式碼之後呼叫可能會執行之前完成。 若要加入處理常式完全啟動連線之後，最好是將它們放到做為參數傳遞給 start 方法的回呼函式：

**正確加入參考 SignalR 物件的事件處理常式的 JavaScript 用戶端程式碼**

[!code-javascript[Main](troubleshooting/samples/sample10.js?highlight=1)]

如果連線停止 SignalR 物件仍然會被參考時，也會出現此錯誤。

### <a name="301-moved-permanently-or-302-moved-temporarily-error"></a>"301 已永久移動 」 或者 「 暫時移動 302 」 錯誤

如果專案包含名為 SignalR，自動建立的 proxy 會干擾的資料夾，可能會出現此錯誤。 若要避免這個錯誤，不使用資料夾，稱為`SignalR`在您的應用程式或關閉自動 proxy 產生關閉中。 請參閱[產生 Proxy 和它為您做](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy)如需詳細資訊。

### <a name="403-forbidden-error-in-net-or-silverlight-client"></a>在.NET 或 Silverlight 用戶端的 「 403 禁止 」 錯誤

在跨網域通訊未正確啟用跨網域環境中可能會發生這個錯誤。 如需如何啟用跨網域通訊的資訊，請參閱[如何建立跨定義域連線](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)。 若要建立跨定義域中的連接 Silverlight 用戶端，請參閱[Silverlight 用戶端進行跨網域連線](../guide-to-the-api/hubs-api-guide-net-client.md#slcrossdomain)。

### <a name="404-not-found-error"></a>「 404 找不到 」 錯誤

有多種原因會導致此問題。 請確認下列各項：

- **中樞 proxy 位址參照的格式不正確：**如果產生的中樞 proxy 位址的參考的格式不正確，通常會出現此錯誤。 請確認中樞位址的參考都正確。 請參閱[如何參考動態產生的 proxy](../guide-to-the-api/hubs-api-guide-javascript-client.md#dynamicproxy)如需詳細資訊。
- **將路由加入至應用程式，然後再加入中樞路由：**如果應用程式使用其他路由，請確認新增的第一個路由是呼叫`MapSignalR`。
- **無副檔名的 url 使用 IIS 7 或 7.5，而不進行更新：**使用 IIS 7 或 7.5 需要更新無副檔名的 url，讓伺服器可提供存取中樞定義`/signalr/hubs`。 您可以找到更新[這裡](https://support.microsoft.com/kb/980368/en-us)。
- **IIS 快取已過期或已損毀：**若要確認快取內容不是已過期，請在 PowerShell 視窗中，清除快取中輸入下列命令：

    [!code-powershell[Main](troubleshooting/samples/sample11.ps1)]

### <a name="500-internal-server-error"></a>「 500 內部伺服器錯誤 」

這是非常一般錯誤，可能會有各種不同的原因。 錯誤詳細資料應該會出現在伺服器的事件記錄檔，或可以找到偵錯伺服器。 藉由開啟詳細的錯誤，伺服器上可能會取得更詳細的錯誤資訊。 如需詳細資訊，請參閱[如何處理中樞類別中的錯誤](../guide-to-the-api/hubs-api-guide-server.md#handleErrors)。

如果防火牆或 proxy 設定不正確，造成重寫的要求標頭，也普遍出現此錯誤。 方案是確定防火牆或 proxy 上啟用了連接埠 80。

### <a name="unexpected-response-code-500"></a>「 非預期的回應碼： 500 」

如果應用程式中使用的.NET framework 版本不符合 Web.Config 中指定的版本，可能會發生這個錯誤。解決方法是確認.NET 4.5 用於應用程式設定和 Web.Config 檔。

### <a name="typeerror-lthubtypegt-is-undefined-error"></a>「 TypeError: &lt;hubType&gt;是未定義 」 錯誤

如果將產生此錯誤的呼叫`MapSignalR`進行不正確。 請參閱[如何註冊 SignalR 中介軟體和設定 SignalR 選項](../guide-to-the-api/hubs-api-guide-server.md#route)如需詳細資訊。

### <a name="jsonserializationexception-was-unhandled-by-user-code"></a>JsonSerializationException 未處理的使用者程式碼

請確認您傳送至您的方法的參數不包含非可序列化的型別 （例如檔案控制代碼或資料庫連接）。 如果您需要使用的成員，您不想要傳送至用戶端 （或是安全性序列化的原因），使用伺服器端物件上`JSONIgnore`屬性。

### <a name="protocol-error-unknown-transport-error"></a>"通訊協定錯誤： 未知的傳輸 」 錯誤

如果用戶端不支援 SignalR 使用的傳輸，可能會發生這個錯誤。 請參閱[傳輸和後援](../getting-started/introduction-to-signalr.md#transports)所在瀏覽器可以搭配 SignalR 的資訊。

### <a name="javascript-hub-proxy-generation-has-been-disabled"></a>「 JavaScript Hub proxy 產生具有已停用 」。

如果會發生此錯誤`DisableJavaScriptProxies`時也包括在動態產生之 proxy 的參考設定`signalr/hubs`。 如需有關如何手動建立 proxy 的詳細資訊，請參閱[產生的 proxy，它會為您](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy)。

### <a name="the-connection-id-is-in-the-incorrect-format-or-the-user-identity-cannot-change-during-an-active-signalr-connection-error"></a>"的格式不正確的連接識別碼 」 或 「 使用者識別無法在使用中的 SignalR 連線期間變更 」 錯誤

如果使用驗證時，並停止連線之前，登出用戶端，可能會出現此錯誤。 解決方法是停止前用戶端登出 SignalR 連線。

### <a name="uncaught-error-signalr-jquery-not-found-please-ensure-jquery-is-referenced-before-the-signalrjs-file-error"></a>「 未能攔截的錯誤： SignalR： 找不到的 jQuery。 請確定 jQuery 參考 SignalR.js 檔案前 」 錯誤

SignalR JavaScript 用戶端所需執行的 jQuery。 請確認您對 jQuery 參考是正確、 所使用的路徑有效，以及 signalr 參考之前，會參考 jQuery。

### <a name="uncaught-typeerror-cannot-read-property-ltpropertygt-of-undefined-error"></a>「 未能攔截 TypeError： 無法讀取屬性 '&lt;屬性&gt;' 的未定義 」 錯誤

產生此錯誤沒有 jQuery 或參考正確的中樞 proxy。 請確認您對 jQuery 和中樞 proxy 的參考正確，所使用的路徑有效，且參考 jQuery 是之前的中樞 proxy 的參考。 中樞 proxy 的預設參考看起來應該如下所示：

**正確參考中樞 proxy 的 HTML 用戶端程式碼**

[!code-html[Main](troubleshooting/samples/sample12.html)]

### <a name="runtimebinderexception-was-unhandled-by-user-code-error"></a>「 使用者程式碼未處理 RuntimeBinderException 」 錯誤

可能會發生此錯誤時的不正確的多載`Hub.On`用。 如果方法具有傳回值，必須指定的傳回型別為泛型型別參數：

**（不含產生的 proxy) 用戶端上定義的方法**

[!code-html[Main](troubleshooting/samples/sample13.html?highlight=1)]

### <a name="connection-id-is-inconsistent-or-connection-breaks-between-page-loads"></a>連接識別碼不一致或載入頁面之間的連線中斷

這種行為是設計上的預期行為。 中樞物件裝載在的頁面物件，因為當頁面重新整理時，就會終結中樞。 多頁的應用程式需要維護使用者與連線識別碼的關聯，如此它們才會載入頁面之間的一致性。 可以在伺服器上儲存的連線識別碼`ConcurrentDictionary`物件或資料庫。

### <a name="value-cannot-be-null-error"></a>"值不能是 null 」 錯誤

目前不支援伺服器端方法，以選擇性參數。如果省略選擇性參數，則該方法會失敗。 如需詳細資訊，請參閱[選擇性參數](https://github.com/SignalR/SignalR/issues/324)。

### <a name="firefox-cant-establish-a-connection-to-the-server-at-ltaddressgt-error-in-firebug"></a>「 Firefox 無法連接到伺服器&lt;位址&gt;"firebug 這類錯誤

這則錯誤訊息可以示 firebug 這類如果交涉的 WebSocket 傳輸失敗，而改為使用另一個的傳輸。 這種行為是設計上的預期行為。

### <a name="the-remote-certificate-is-invalid-according-to-the-validation-procedure-error-in-net-client-application"></a>.NET 用戶端應用程式中的 「 遠端憑證不正確的驗證程序根據 」 錯誤

如果您的伺服器需要自訂用戶端憑證，則您可以加入 x509certificate 連線進行要求的作業之前。 將憑證新增至連線使用`Connection.AddClientCertificate`。

### <a name="connection-drops-after-authentication-times-out"></a>驗證逾時之後，會卸除連接

這種行為是設計上的預期行為。 無法修改驗證認證，當連線為作用中;若要重新整理的認證，連接必須停止並重新啟動。

### <a name="onconnected-gets-called-twice-when-using-jquery-mobile"></a>使用 jQuery Mobile 時，兩次呼叫 OnConnected

jQuery Mobile 的`initializePage`函式會強制重新執行，每一頁中的指令碼以便建立第二個連接。 此問題的解決方案包括：

- 包含在 JavaScript 檔案之前的 jQuery Mobile 的參考。
- 停用`initializePage`函式，藉由設定`$.mobile.autoInitializePage = false`。
- 等候頁面，即可完成初始化之前啟動連線。

### <a name="messages-are-delayed-in-silverlight-applications-using-server-sent-events"></a>使用伺服器傳送事件的 Silverlight 應用程式中發生延遲訊息

使用伺服器上 Silverlight 傳送事件時，就會延遲的訊息。 啟動連線時，若要強制長輪詢改為使用中，使用下列：

[!code-css[Main](troubleshooting/samples/sample14.css)]

### <a name="permission-denied-using-forever-frame-protocol"></a>使用 「 權限遭拒 」 永遠框架通訊協定

這是已知的問題，並描述[這裡](https://github.com/SignalR/SignalR/issues/1963)。 這個徵兆可能會看到使用最新的 JQuery 程式庫。因應措施是降級 JQuery 1.8.2 您的應用程式。

### <a name="invalidoperationexception-not-a-valid-web-socket-request"></a>「 InvalidOperationException： 不是有效的 web socket 要求。

如果使用 WebSocket 通訊協定，但是網路 proxy 正在修改的要求標頭，則可能會發生這個錯誤。 解決方法是設定為允許連接埠 80 上的 WebSocket proxy。

### <a name="exception-ltmethod-namegt-method-could-not-be-resolved-when-client-calls-method-on-server"></a>「 例外狀況：&lt;方法名稱&gt;方法無法解析"當用戶端呼叫在伺服器上的方法

這個錯誤可能起因於使用，則無法探索 JSON 承載，例如陣列中的資料類型。 因應措施是使用資料型別，便可探索的 JSON，例如 IList。 如需詳細資訊，請參閱[.NET 用戶端無法呼叫具有陣列參數的中樞方法](https://github.com/SignalR/SignalR/issues/2672)。

<a id="server"></a>

## <a name="compilation-and-server-side-errors"></a>編譯和伺服器端錯誤

 下節包含可能的解決方案，編譯器和伺服器端執行階段錯誤。 

### <a name="reference-to-hub-instance-is-null"></a>中樞執行個體的參考為 null

針對每個連接所建立的 hub 執行個體，因為您無法中樞執行的個體在程式碼中自行建立。 若要從中樞之外本身的中樞上呼叫方法，請參閱[如何呼叫方法的用戶端和管理群組從中樞類別之外](../guide-to-the-api/hubs-api-guide-server.md#callfromoutsidehub)如何取得的中樞內容的參考。

### <a name="httpcontextcurrentsession-is-null"></a>HTTPContext.Current.Session 為 null

這種行為是設計上的預期行為。 SignalR 不支援 ASP.NET 工作階段狀態，因為啟用工作階段狀態雙工訊息會中斷。

### <a name="no-suitable-method-to-override"></a>若要覆寫任何合適的方法

如果您使用程式碼從舊版的文件或部落格，可能會看到這個錯誤。 請確認未參考的已變更或已被取代的方法名稱 (例如`OnConnectedAsync`)。

### <a name="hostcontextextensionswebsocketserverurl-is-null"></a>HostContextExtensions.WebSocketServerUrl 為 null

這種行為是設計上的預期行為。 這個成員已被取代，不應使用。

### <a name="a-route-named-signalrhubs-is-already-in-the-route-collection-error"></a>「 名稱為 'signalr.hubs' 的路由已經在路由集合中 」 錯誤

如果將會出現此錯誤`MapSignalR`兩次呼叫您的應用程式。 某些範例應用程式呼叫`MapSignalR`直接啟動類別; 在其他人進行的呼叫包裝函式類別中。 請確定您的應用程式不會兩者。

### <a name="websocket-is-not-used"></a>不是 WebSocket

如果您已確認您的伺服器和用戶端符合 WebSocket 的需求 (列在[支援的平台](../getting-started/supported-platforms.md)文件)，您必須在伺服器上啟用 WebSocket。 執行此作業可以指示[這裡](https://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-websocket-protocol-support)。

### <a name="connection-is-undefined"></a>$.connection 是未定義

此錯誤表示，在頁面上的指令碼未正常運作，載入或無法連線到中樞 proxy，或不正確地存取。 請確認您的頁面上的指令碼參考對應指令碼載入您的專案，且，存取在瀏覽器的 /signalr/hubs 當伺服器執行。

### <a name="one-or-more-types-required-to-compile-a-dynamic-expression-cannot-be-found"></a>找不到編譯動態運算式所需的一個或多個型別

此錯誤表示`Microsoft.CSharp`遺漏程式庫。 將它加入**組件-&gt;Framework**  索引標籤。

### <a name="caller-state-cannot-be-accessed-from-clientscaller-in-visual-basic-or-in-a-strongly-typed-hub-conversion-from-type-taskof-object-to-type-string-is-not-valid-error"></a>無法從 Clients.Caller 強型別集線器; 或 Visual Basic 中存取呼叫者狀態"從類型 'String' 類型 'Task (Of Object)' 轉換無效 」 錯誤

若要存取 Visual Basic 或強型別中樞中的呼叫端狀態，請使用`Clients.CallerState`屬性 （SignalR 2.1 中引進） 而非`Clients.Caller`。

<a id="vs"></a>

## <a name="visual-studio-issues"></a>Visual Studio 問題

本章節描述在 Visual Studio 中遇到的問題。

### <a name="script-documents-node-does-not-appear-in-solution-explorer"></a>指令碼文件] 節點沒有出現在 [方案總管

有些教學課程將您導向到 「 指令碼文件 」 節點，在 [方案總管] 中偵錯時。 此節點由 JavaScript 偵錯工具所產生，才會出現在 Internet Explorer 中，瀏覽器用戶端偵錯時如果使用 Chrome 或 Firefox，不會出現在節點。 如果正在執行另一個用戶端偵錯工具，例如 Silverlight 偵錯工具，也將無法執行 JavaScript 偵錯工具。

### <a name="signalr-does-not-work-on-visual-studio-2008-or-earlier"></a>SignalR 無法運作 Visual Studio 2008 或更早版本

這種行為是設計上的預期行為。 SignalR 需要.NET Framework 4 或更新版本。這需要 SignalR 應用程式來開發在 Visual Studio 2010 或更新版本。 SignalR 的伺服器元件需要.NET Framework 4.5。

<a id="iis"></a>

## <a name="iis-issues"></a>IIS 問題

本章節包含 Internet Information Services 的問題。

### <a name="signalr-works-on-visual-studio-development-server-but-not-in-iis"></a>在 Visual Studio 程式開發伺服器上，但未在 IIS 中的 SignalR 運作方式

SignalR 也支援 IIS 7.0 和 7.5，但必須加入無副檔名 Url 的支援。 若要加入支援無副檔名 Url，請參閱[https://support.microsoft.com/kb/980368](https://support.microsoft.com/kb/980368)

SignalR 要求 （未安裝 ASP.NET 在 IIS 預設情況下） 的伺服器上安裝 ASP.NET。 若要安裝 ASP.NET，請參閱[ASP.NET 下載](https://www.asp.net/downloads)。

<a id="azure"></a>

## <a name="microsoft-azure-issues"></a>Microsoft Azure 問題

本章節包含使用 Microsoft Azure 的問題。

### <a name="fileloadexception-when-hosting-signalr-in-an-azure-worker-role"></a>FileLoadException 時裝載 Azure 背景工作角色中的 SignalR

裝載 Azure 背景工作角色中的 SignalR 例外狀況可能會造成 「 無法載入檔案或組件 ' Microsoft.Owin，Version = 2.0.0.0"。 這是 NuGet; 的已知的問題Azure 背景工作角色專案中，則不會自動新增繫結重新導向。 若要修正此問題，您可以手動新增繫結重新導向。 加入下列行以`app.config`背景工作角色專案檔。

[!code-xml[Main](troubleshooting/samples/sample15.xml)]

### <a name="messages-are-not-received-through-the-azure-backplane-after-altering-topic-names"></a>會在收到訊息透過 Azure 後擋板之後變更的主題名稱

使用 Azure 的後擋板的主題會在內部，維護它們不是使用者可進行設定。
