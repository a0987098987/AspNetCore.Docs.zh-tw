---
uid: signalr/overview/testing-and-debugging/troubleshooting
title: SignalR 疑難排解 |Microsoft Docs
author: pfletcher
description: 本文說明開發 SignalR 應用程式的常見問題。
ms.author: aspnetcontent
ms.date: 06/10/2014
ms.assetid: 4b559e6c-4fb0-4a04-9812-45cf08ae5779
msc.legacyurl: /signalr/overview/testing-and-debugging/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: fbe1ffe7dc816f6470f8bf93e592f0ff4b299145
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803943"
---
<a name="signalr-troubleshooting"></a>SignalR 疑難排解
====================
藉由[Patrick Fletcher](https://github.com/pfletcher)

> 本文件說明使用 SignalR 的常見疑難排解問題。
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


本文件包含下列各節。

- [以無訊息方式失敗呼叫用戶端與伺服器之間的方法](#connection)
- [設定 IIS websockets，ping/pong 來偵測無作用的用戶端](#pong)
- [其他連線問題](#other)
- [編譯和伺服器端錯誤](#server)
- [Visual Studio 問題](#vs)
- [Internet Information Services 的問題](#iis)
- [Microsoft Azure 問題](#azure)

<a id="connection"></a>

## <a name="calling-methods-between-the-client-and-server-silently-fails"></a>以無訊息方式失敗呼叫用戶端與伺服器之間的方法

本章節描述針對方法呼叫失敗不會有意義的錯誤訊息的用戶端與伺服器之間的可能原因。 SignalR 應用程式中，伺服器會有任何用戶端會實作; 方法的相關資訊當伺服器叫用的用戶端方法，方法名稱和參數資料傳送到用戶端，和方法，就會執行只存在於伺服器指定的格式。 如果沒有符合的方法會找到在用戶端，會發生任何事，而且不會引發錯誤訊息的伺服器上。

若要進一步調查用戶端未呼叫的方法，您可以開啟之前呼叫 start 方法上的中樞，以查看哪些呼叫來自伺服器的記錄。 若要啟用記錄中的 JavaScript 應用程式，請參閱[如何啟用用戶端記錄 （JavaScript 用戶端版本）](../guide-to-the-api/hubs-api-guide-javascript-client.md#logging)。 若要啟用記錄功能在.NET 用戶端應用程式，請參閱[如何啟用用戶端記錄 （.NET 用戶端版本）](../guide-to-the-api/hubs-api-guide-net-client.md#logging)。

### <a name="misspelled-method-incorrect-method-signature-or-incorrect-hub-name"></a>拼錯的方法、 不正確的方法簽章或不正確的中樞名稱

如果名稱或被呼叫的方法簽章不完全符合適當的方法，用戶端上，呼叫會失敗。 請確認伺服器所呼叫的方法名稱符合用戶端上方法的名稱。 此外，SignalR 建立中樞 proxy 使用依照 camel 命名法大小寫的方法，為適用於 JavaScript，因此呼叫的方法`SendMessage`會呼叫伺服器上`sendMessage`中用戶端 proxy。 如果您使用`HubName`您伺服器端程式碼中的屬性，請確認所使用的名稱，符合用來在用戶端建立中樞的名稱。 如果您不要使用`HubName`屬性，則驗證中的 JavaScript 用戶端中樞的名稱是依照 camel 命名法大小寫慣例，而不是 ChatHub chatHub 等。

### <a name="duplicate-method-name-on-client"></a>重複的用戶端上的方法名稱

請確認，您不需要重複的方法只有大小寫不同的用戶端上。 如果用戶端應用程式有一個方法，叫做`sendMessage`，確認沒有也呼叫的方法`SendMessage`以及。

### <a name="missing-json-parser-on-the-client"></a>在用戶端上遺漏的 JSON 剖析器

SignalR 需要 JSON 剖析器要出現序列化伺服器與用戶端之間的呼叫。 如果您的用戶端沒有內建的 JSON 剖析器 （例如 Internet Explorer 7)，您必須包含在應用程式中。 您可以下載 JSON 剖析器[此處](http://nuget.org/packages/json2)。

### <a name="mixing-hub-and-persistentconnection-syntax"></a>混用中樞 PersistentConnection 語法

SignalR 使用兩種通訊模式： 中樞和 PersistentConnections。 呼叫這兩個通訊模型的語法是不同的用戶端程式碼。 如果您已新增中樞伺服器程式碼中，確認所有用戶端程式碼會使用適當的中樞語法。

**建立 PersistentConnection JavaScript 用戶端中的 JavaScript 用戶端程式碼**

[!code-javascript[Main](troubleshooting/samples/sample1.js)]

**建立 Javascript 用戶端中的中樞 Proxy 的 JavaScript 用戶端程式碼**

[!code-javascript[Main](troubleshooting/samples/sample2.js)]

**C# 伺服端程式碼會將路由對應到 PersistentConnection**

[!code-csharp[Main](troubleshooting/samples/sample3.cs)]

**C# 伺服端程式碼，路由至中樞，或對應至多個中樞有多個應用程式**

[!code-css[Main](troubleshooting/samples/sample4.css)]

### <a name="connection-started-before-subscriptions-are-added"></a>開始之前會加入訂用帳戶的連線

如果可以從伺服器呼叫的方法加入至 proxy 之前啟動中樞的連接，就不會收到訊息。 下列 JavaScript 程式碼將不會啟動中樞正確：

**不正確 JavaScript 用戶端程式碼，不允許中樞接收訊息**

[!code-javascript[Main](troubleshooting/samples/sample5.js)]

相反地，加入方法的訂用帳戶呼叫開始之前：

**正確加入至中樞的 訂用帳戶的 JavaScript 用戶端程式碼**

[!code-javascript[Main](troubleshooting/samples/sample6.js)]

### <a name="missing-method-name-on-the-hub-proxy"></a>中樞 proxy 上遺漏的方法名稱

請確認訂閱用戶端上，在伺服器上定義的方法。 即使伺服器定義的方法，它必須仍會加入至用戶端 proxy。 方法可以透過下列方式新增至用戶端 proxy (請注意，此方法會加入至`client`中樞，而不是中樞直接的成員):

**將方法新增至中樞 proxy 的 JavaScript 用戶端程式碼**

[!code-javascript[Main](troubleshooting/samples/sample7.js)]

### <a name="hub-or-hub-methods-not-declared-as-public"></a>中樞或中樞方法未宣告為公用

若要顯示在用戶端上，在中樞實作和方法必須宣告為`public`。

### <a name="accessing-hub-from-a-different-application"></a>從不同的應用程式存取中樞

SignalR 中樞只能透過實作 SignalR 用戶端的應用程式存取。 SignalR 無法交互操作，與其他的通訊程式庫 （例如 SOAP 或 WCF web 服務。）如果沒有可供您的目標平台的 SignalR 用戶端，您無法直接存取伺服器的端點。

### <a name="manually-serializing-data"></a>以手動方式將資料序列化

SignalR 會自動使用 JSON 序列化程式的方法參數那里的不需要自行進行此作業。

### <a name="remote-hub-method-not-executed-on-client-in-ondisconnected-function"></a>遠端未在 OnDisconnected 函式中的用戶端上執行的中樞方法

這種行為是設計上的預期行為。 當`OnDisconnected`是呼叫，中樞已進入`Disconnected`不允許進一步的中樞方法呼叫的狀態。

**C# 伺服端程式碼正確執行 OnDisconnected 事件中的 程式碼**

[!code-csharp[Main](troubleshooting/samples/sample8.cs)]

### <a name="ondisconnect-not-firing-at-consistent-times"></a>OnDisconnect 未引發一致的時間

這種行為是設計上的預期行為。 當使用者嘗試離開頁面有使用中的 SignalR 連線時，則 SignalR 用戶端接著便可通知伺服器將會停止用戶端連線的最佳嘗試。 如果 SignalR 用戶端的最佳嘗試無法連線到伺服器，伺服器將會處置之後可設定的連接`DisconnectTimeout`更新版本中，在這段`OnDisconnected`事件就會引發。 如果 SignalR 用戶端的最佳嘗試會成功，`OnDisconnected`事件就會立即引發。

如需設定資訊`DisconnectTimeout`設定，請參閱[處理連線存留期事件： DisconnectTimeout](../guide-to-the-api/handling-connection-lifetime-events.md#disconnecttimeout)。

### <a name="connection-limit-reached"></a>已達到連線限制

當用戶端作業系統，例如 Windows 7 上使用完整版的 IIS，10 連接是加諸的限制。 當使用用戶端作業系統，請改用 IIS Express 若要避免這項限制。

### <a name="cross-domain-connection-not-set-up-properly"></a>跨網域連線設定不正確

如果跨網域連接未正確設定 （的 SignalR URL 不在與裝載網頁位於相同網域的連線），沒有錯誤訊息，則連線可能失敗。 如需如何啟用跨網域通訊的詳細資訊，請參閱[如何建立跨網域連接](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)。

### <a name="connection-using-ntlm-active-directory-not-working-in-net-client"></a>使用 NTLM (Active Directory) 不在.NET 用戶端的連線

如果連接未正確設定，使用網域安全性的.NET 用戶端應用程式中的連接可能會失敗。 若要使用 SignalR 網域環境中，將必要的連接屬性，如下所示：

**C# 用戶端程式碼實作連線認證**

[!code-csharp[Main](troubleshooting/samples/sample9.cs)]

<a id="pong"></a>

## <a name="configuring-iis-websockets-to-pingpong-to-detect-a-dead-client"></a>設定 IIS websockets，ping/pong 來偵測無作用的用戶端

如果用戶端是廢與否，而且它們都依賴來自基礎 websocket 連線失敗的通知，也就是 OnClose 回呼，不知道 SignalR 伺服器。 此問題的解決方案之一是設定為您執行 ping/pong IIS websockets。 這可確保如果意外中斷，將會關閉您的連線。 如需詳細資訊，請參閱[這篇 stackoverflow 文章](http://stackoverflow.com/questions/19502755/websocket-clients-state-not-changing-on-network-loss)。

<a id="other"></a>

## <a name="other-connection-issues"></a>其他連線問題

本章節描述的原因和解決方案的特定徵狀或在連接期間發生的錯誤訊息。

### <a name="start-must-be-called-before-data-can-be-sent-error"></a>「 在傳送資料前，必須呼叫啟動 」 錯誤

如果程式碼會在開始連接之前，先將 SignalR 物件參考，通常可以看到此錯誤。 處理常式和類似裝設，會呼叫方法，在伺服器上定義必須新增連線完成後。 請注意，呼叫`Start`是非同步的因此程式碼之後呼叫可能會執行它之前完成。 若要連接完全啟動之後，加入處理常式，最好是將它們放到做為參數傳遞至 start 方法的回呼函式：

**正確地將參考 SignalR 物件的事件處理常式的 JavaScript 用戶端程式碼**

[!code-javascript[Main](troubleshooting/samples/sample10.js?highlight=1)]

如果仍然正在參考 SignalR 物件時，就會停止連線，也會出現此錯誤。

### <a name="301-moved-permanently-or-302-moved-temporarily-error"></a>「 永久 301 已移動 」 或者 「 暫時 302 已移動 」 錯誤

如果專案包含名為 SignalR 會干擾自動建立 proxy 的資料夾，可能會出現此錯誤。 若要避免這個錯誤，不使用名為的資料夾`SignalR`在您的應用程式或關閉自動 proxy 產生關閉。 請參閱[產生的 Proxy，它會為您](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy)如需詳細資訊。

### <a name="403-forbidden-error-in-net-or-silverlight-client"></a>在.NET 或 Silverlight 用戶端的 「 403 禁止 」 錯誤

在其中跨網域通訊未正確啟用的跨網域環境中可能會發生這個錯誤。 如需如何啟用跨網域通訊的詳細資訊，請參閱[如何建立跨網域連接](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)。 若要建立跨網域連接 Silverlight 用戶端中的，請參閱[來自 Silverlight 用戶端的跨網域連線](../guide-to-the-api/hubs-api-guide-net-client.md#slcrossdomain)。

### <a name="404-not-found-error"></a>「 找不到 404 」 錯誤

有多種原因會導致此問題。 請確認下列各項：

- **中樞 proxy 位址參考的格式不正確：** 參考產生的中樞 proxy 位址的格式不正確，如果經常看到此錯誤。 確認會正確地設定中樞位址的參考。 請參閱[如何參考動態產生的 proxy](../guide-to-the-api/hubs-api-guide-javascript-client.md#dynamicproxy)如需詳細資訊。
- **將路由新增至應用程式，然後再加入中樞路由：** 如果您的應用程式使用其他路由，請確認新增的第一個路由會呼叫`MapSignalR`。
- **使用 IIS 7 或沒有更新 7.5 無副檔名的 url:** 使用 IIS 7 或 7.5 需要更新無副檔名的 url，讓伺服器能夠提供存取中樞定義`/signalr/hubs`。 您可以找到更新[此處](https://support.microsoft.com/kb/980368)。
- **IIS 快取過期或已損毀：** 若要確認快取內容不是已過期，請清除快取的 PowerShell 視窗中輸入下列命令：

    [!code-powershell[Main](troubleshooting/samples/sample11.ps1)]

### <a name="500-internal-server-error"></a>「 500 內部伺服器錯誤 」

這是種泛泛的錯誤，可能有各種不同的原因。 錯誤的詳細資料應該會出現在伺服器的事件記錄檔，或可透過偵錯伺服器。 藉由開啟詳細的錯誤，伺服器上可取得更詳細的錯誤資訊。 如需詳細資訊，請參閱 <<c0> [ 如何處理中樞類別中的錯誤](../guide-to-the-api/hubs-api-guide-server.md#handleErrors)。

如果防火牆或 proxy 設定不正確，導致重寫要求標頭，通常也會出現此錯誤。 解決方法是請確定連接埠 80 已啟用防火牆或 proxy 上。

### <a name="unexpected-response-code-500"></a>「 非預期的回應碼： 500"

如果應用程式中使用的.NET framework 版本不符合在 Web.Config 中指定的版本，可能會發生此錯誤。解決方法是確認.NET 4.5 用於應用程式設定和 Web.Config 檔案。

### <a name="typeerror-lthubtypegt-is-undefined-error"></a>「 TypeError: &lt;hubType&gt;是未定義 」 錯誤

如果將產生此錯誤的呼叫`MapSignalR`，不會正確。 請參閱[如何註冊 SignalR 中介軟體，並設定 SignalR 選項](../guide-to-the-api/hubs-api-guide-server.md#route)如需詳細資訊。

### <a name="jsonserializationexception-was-unhandled-by-user-code"></a>JsonSerializationException 未處理使用者程式碼

請確認您傳送給您的方法的參數不包含非可序列化的型別 （例如檔案控制代碼或資料庫連接）。 如果您需要在您不想要傳送至用戶端 （無論是安全性或序列化的原因），使用伺服器端物件上使用成員`JSONIgnore`屬性。

### <a name="protocol-error-unknown-transport-error"></a>「 通訊協定錯誤： 未知的傳輸 」 錯誤

如果用戶端不支援 SignalR 使用的傳輸，可能會發生此錯誤。 請參閱[傳輸和後援](../getting-started/introduction-to-signalr.md#transports)所在的瀏覽器可以使用與 SignalR 的資訊。

### <a name="javascript-hub-proxy-generation-has-been-disabled"></a>「 JavaScript 中樞 proxy 的產生具有已停用。 」

如果會發生此錯誤`DisableJavaScriptProxies`時也包括在動態產生之 proxy 的參考設定`signalr/hubs`。 如需有關如何手動建立 proxy 的詳細資訊，請參閱 <<c0> [ 產生的 proxy，它會為您](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy)。

### <a name="the-connection-id-is-in-the-incorrect-format-or-the-user-identity-cannot-change-during-an-active-signalr-connection-error"></a>"的格式不正確的連接識別碼 」 或 「 使用者身分識別不能變更期間作用中的 SignalR 連線 」 錯誤

如果使用驗證時，並停止連線之前，登出用戶端，可能會出現此錯誤。 解決方法是停止前用戶端登出 SignalR 連線。

### <a name="uncaught-error-signalr-jquery-not-found-please-ensure-jquery-is-referenced-before-the-signalrjs-file-error"></a>「 無法攔截錯誤： SignalR： 找不到 jQuery。 請確定 jQuery 參考 SignalR.js 檔案前 」 錯誤

SignalR JavaScript 用戶端需要執行的 jQuery。 請確認您參考 jQuery 是正確的所使用的路徑有效，且參考至 jQuery 是 signalr 參考之前。

### <a name="uncaught-typeerror-cannot-read-property-ltpropertygt-of-undefined-error"></a>「 無法攔截 TypeError： 無法讀取屬性 '&lt;屬性&gt;' 未定義 」 錯誤

從沒有 jQuery 或參考正確的中樞 proxy 會產生此錯誤。 請確認您到 jQuery 和中樞 proxy 的參考正確，所使用的路徑有效，且參考 jQuery 是之前的中樞 proxy 的參考。 中樞 proxy 的預設參考看起來應該如下所示：

**正確地參考中樞 proxy 的 HTML 用戶端程式碼**

[!code-html[Main](troubleshooting/samples/sample12.html)]

### <a name="runtimebinderexception-was-unhandled-by-user-code-error"></a>「 使用者程式碼未處理 RuntimeBinderException 」 錯誤

可能會發生此錯誤時的不正確的多載`Hub.On`用。 如果方法具有傳回值，傳回的型別必須指定為泛型類型參數：

**（沒有產生的 proxy) 的用戶端上定義的方法**

[!code-html[Main](troubleshooting/samples/sample13.html?highlight=1)]

### <a name="connection-id-is-inconsistent-or-connection-breaks-between-page-loads"></a>連線識別碼不一致或頁面載入之間的連線中斷

這種行為是設計上的預期行為。 因為中樞物件裝載在的頁面物件中，當頁面重新整理時，就會終結中樞。 多頁應用程式需要維護使用者與連線識別碼的關聯，以便將其載入頁面之間保持一致。 可以在伺服器上儲存的連線識別碼`ConcurrentDictionary`物件或資料庫。

### <a name="value-cannot-be-null-error"></a>"Value cannot be null 」 錯誤

目前不支援伺服器端方法，以選擇性的參數;如果省略選擇性參數，則此方法將會失敗。 如需詳細資訊，請參閱 <<c0> [ 選擇性參數](https://github.com/SignalR/SignalR/issues/324)。

### <a name="firefox-cant-establish-a-connection-to-the-server-at-ltaddressgt-error-in-firebug"></a>「 Firefox 無法連接到伺服器&lt;地址&gt;」 在 Firebug 中的錯誤

如果 WebSocket 傳輸的交涉失敗，而另一種傳輸會改為使用相同，就可以在 Firebug 中看到此錯誤訊息。 這種行為是設計上的預期行為。

### <a name="the-remote-certificate-is-invalid-according-to-the-validation-procedure-error-in-net-client-application"></a>在.NET 用戶端應用程式中的 「 遠端憑證無效根據驗證程序 」 錯誤

如果您的伺服器需要自訂用戶端憑證，然後您可以加入 x509certificate 連接之前提出要求。 將憑證新增至連線使用`Connection.AddClientCertificate`。

### <a name="connection-drops-after-authentication-times-out"></a>驗證逾時之後，會卸除連接

這種行為是設計上的預期行為。 連線處於作用中; 時，就無法修改驗證認證若要重新整理認證，連接必須停止並重新啟動。

### <a name="onconnected-gets-called-twice-when-using-jquery-mobile"></a>使用 jQuery Mobile 時，兩次呼叫 OnConnected

jQuery Mobile 的`initializePage`函式會強制重新執行，每個頁面中的指令碼以建立第二個連接。 此問題的解決方案包括：

- 包含您的 JavaScript 檔案之前的 jQuery Mobile 的參考。
- 停用`initializePage`函式，藉由設定`$.mobile.autoInitializePage = false`。
- 等候完成之前啟動連線初始化頁面。

### <a name="messages-are-delayed-in-silverlight-applications-using-server-sent-events"></a>訊息會顯示在 Silverlight 應用程式使用伺服器傳送事件

使用伺服器在 Silverlight 上傳送事件時，會延遲訊息。 啟動連線時，若要強制長時間輪詢要改為使用中，使用下列：

[!code-css[Main](troubleshooting/samples/sample14.css)]

### <a name="permission-denied-using-forever-frame-protocol"></a>不限次數 」 權限遭拒 」 使用框架通訊協定

這是已知的問題，並說明[此處](https://github.com/SignalR/SignalR/issues/1963)。 這個徵兆可能會看到使用最新的 JQuery 程式庫;因應措施是降級至 JQuery 1.8.2 應用程式。

### <a name="invalidoperationexception-not-a-valid-web-socket-request"></a>「 InvalidOperationException： 不是有效的 web 通訊端要求。

如果使用 WebSocket 通訊協定，但網路 proxy 正在修改的要求標頭，則可能會發生此錯誤。 解決方法是設定 proxy 以允許連接埠 80 上的使用 WebSocket。

### <a name="exception-ltmethod-namegt-method-could-not-be-resolved-when-client-calls-method-on-server"></a>「 例外狀況：&lt;方法名稱&gt;方法無法解決 「 當用戶端呼叫伺服器上的方法

此錯誤可能起因於使用找不到 JSON 承載，例如陣列中的資料類型。 因應措施是使用 JSON，例如 IList 可探索的資料類型。 如需詳細資訊，請參閱 <<c0> [ 無法呼叫具有陣列參數的中樞方法的.NET 用戶端](https://github.com/SignalR/SignalR/issues/2672)。

<a id="server"></a>

## <a name="compilation-and-server-side-errors"></a>編譯和伺服器端錯誤

 下節包含可能的解決方案，編譯器和伺服器端執行階段錯誤。 

### <a name="reference-to-hub-instance-is-null"></a>中樞執行個體的參考為 null

針對每個連接所建立的中樞執行個體，因為您無法中樞執行的個體在程式碼中自行建立。 若要呼叫方法，從中樞本身之外，請參閱[如何呼叫方法的用戶端和管理中樞類別外的群組](../guide-to-the-api/hubs-api-guide-server.md#callfromoutsidehub)為濆爧髍孮中樞內容的參考。

### <a name="httpcontextcurrentsession-is-null"></a>HTTPContext.Current.Session 為 null

這種行為是設計上的預期行為。 SignalR 不支援 ASP.NET 工作階段狀態，因為啟用的工作階段狀態，就會破壞雙工通訊。

### <a name="no-suitable-method-to-override"></a>若要覆寫任何合適的方法

如果您使用程式碼，從較舊的文件或部落格，您會看到此錯誤。 請確認您所未參考的已變更或已被取代的方法名稱 (例如`OnConnectedAsync`)。

### <a name="hostcontextextensionswebsocketserverurl-is-null"></a>HostContextExtensions.WebSocketServerUrl 為 null

這種行為是設計上的預期行為。 這個成員已被取代，而且不應該使用。

### <a name="a-route-named-signalrhubs-is-already-in-the-route-collection-error"></a>「 已將名為 'signalr.hubs' 已在路由集合中 」 錯誤

將會看到此錯誤，如果`MapSignalR`兩次呼叫您的應用程式。 某些範例應用程式會呼叫`MapSignalR`直接在啟動類別中其他人進行的呼叫包裝函式類別中。 請確定您的應用程式不會兩者。

### <a name="websocket-is-not-used"></a>不會使用 WebSocket

如果您已確認您的伺服器和用戶端符合 WebSocket 的需求 (列入[支援的平台](../getting-started/supported-platforms.md)文件)，您必須在您的伺服器上啟用 WebSocket。 執行此動作的指示，請參閱[此處](https://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-websocket-protocol-support)。

### <a name="connection-is-undefined"></a>$.connection 是未定義

此錯誤表示，在頁面上的指令碼不正確，載入或無法連線到中樞 proxy，或不正確地存取。 確認頁面上的指令碼參考對應到在您專案中，載入的指令碼，且在瀏覽器可存取，/signalr/hubs，當伺服器執行。

### <a name="one-or-more-types-required-to-compile-a-dynamic-expression-cannot-be-found"></a>找不到編譯動態運算式所需的一或多個型別

這個錯誤表示`Microsoft.CSharp`遺漏的文件庫。 將它加入**組件-&gt;Framework**  索引標籤。

### <a name="caller-state-cannot-be-accessed-from-clientscaller-in-visual-basic-or-in-a-strongly-typed-hub-conversion-from-type-taskof-object-to-type-string-is-not-valid-error"></a>無法從 Clients.Caller 強型別的中樞; 或 Visual Basic 中存取呼叫端狀態"從類型 'Task (Of Object)' 為類型 'String' 的轉換無效 」 錯誤

若要存取強型別中樞或 Visual Basic 中的呼叫端狀態，請使用`Clients.CallerState`屬性 （於 SignalR 2.1 引進），而不是`Clients.Caller`。

<a id="vs"></a>

## <a name="visual-studio-issues"></a>Visual Studio 問題

本節說明在 Visual Studio 中遇到的問題。

### <a name="script-documents-node-does-not-appear-in-solution-explorer"></a>指令碼文件] 節點不會出現在 [方案總管

某些我們的教學課程將您導向至 [指令碼文件] 節點，在 [方案總管] 中偵錯時。 這個節點由 JavaScript 偵錯工具所產生，並只會出現在偵錯在 Internet Explorer; 中的瀏覽器用戶端時如果使用 Chrome 或 Firefox，不會出現的節點。 如果正在執行的另一個用戶端偵錯工具，例如 Silverlight 偵錯工具，也將無法執行 JavaScript 偵錯工具。

### <a name="signalr-does-not-work-on-visual-studio-2008-or-earlier"></a>SignalR 不適 Visual Studio 2008 或更早版本

這種行為是設計上的預期行為。 SignalR 需要.NET Framework 4 或更新版本;這需要在 Visual Studio 2010 或更新版本開發 SignalR 應用程式。 SignalR 的伺服器元件需要.NET Framework 4.5。

<a id="iis"></a>

## <a name="iis-issues"></a>IIS 問題

本章節包含使用 Internet Information Services 的問題。

### <a name="signalr-works-on-visual-studio-development-server-but-not-in-iis"></a>在 Visual Studio 程式開發伺服器，但未在 IIS 中的 SignalR 運作方式

SignalR 支援 IIS 7.0 和 7.5，但必須新增無副檔名 Url 的支援。 若要加入支援無副檔名的 Url，請參閱 [https://support.microsoft.com/kb/980368](https://support.microsoft.com/kb/980368)

SignalR 要求 （未安裝 ASP.NET 在 IIS 預設情況下） 的伺服器上安裝 ASP.NET。 若要安裝 ASP.NET，請參閱[ASP.NET 下載](https://www.asp.net/downloads)。

<a id="azure"></a>

## <a name="microsoft-azure-issues"></a>Microsoft Azure 問題

本章節包含使用 Microsoft Azure 的問題。

### <a name="fileloadexception-when-hosting-signalr-in-an-azure-worker-role"></a>FileLoadException 時裝載 Azure 背景工作角色中的 SignalR

裝載 Azure 背景工作角色中的 SignalR 可能會導致例外狀況 「 無法載入檔案或組件 ' Microsoft.Owin，版本 = 2.0.0.0"。 這是 NuGet; 的已知的問題在 Azure 背景工作角色專案中，則不會自動新增繫結重新導向。 若要修正此問題，您可以手動新增繫結重新導向。 新增下列行以`app.config`背景工作角色專案的檔案。

[!code-xml[Main](troubleshooting/samples/sample15.xml)]

### <a name="messages-are-not-received-through-the-azure-backplane-after-altering-topic-names"></a>未接收訊息透過 Azure 後擋板之後變更的主題名稱

使用 Azure 的後擋板的主題會在內部，維護它們不被要作為使用者可設定。
