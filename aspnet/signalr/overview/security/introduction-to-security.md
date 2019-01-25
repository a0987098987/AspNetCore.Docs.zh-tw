---
uid: signalr/overview/security/introduction-to-security
title: SignalR 安全性簡介 |Microsoft Docs
author: bradygaster
description: 描述開發 SignalR 應用程式時，您必須考慮的安全性問題。
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: ed562717-8591-4936-8e10-c7e63dcb570a
msc.legacyurl: /signalr/overview/security/introduction-to-security
msc.type: authoredcontent
ms.openlocfilehash: 311bfb4279b1c919fddcef2c0aed657083f9c34f
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837321"
---
<a name="introduction-to-signalr-security"></a>SignalR 安全性簡介
====================
藉由[Patrick Fletcher](https://github.com/pfletcher)， [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> 本文說明開發 SignalR 應用程式時，您必須考慮的安全性問題。
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

- [SignalR 安全性概念](#concepts)

    - [驗證與授權](#authentication)
    - [連線語彙基元](#connectiontoken)
    - [重新連線時，重新加入群組](#rejoingroup)
- [SignalR 如何防止跨網站偽造要求](#csrf)
- [SignalR 安全性建議](#recommendations)

    - [安全通訊端層 (SSL) 通訊協定](#ssl)
    - [做為安全性機制，不使用群組](#groupsecurity)
    - [安全地處理來自用戶端的輸入](#input)
    - [使用中連線的使用者狀態的變更，重新調整](#reconcile)
    - [自動產生的 JavaScript proxy 檔案](#autogen)
    - [例外狀況](#exceptions)

<a id="concepts"></a>

## <a name="signalr-security-concepts"></a>SignalR 安全性概念

<a id="authentication"></a>

### <a name="authentication-and-authorization"></a>驗證與授權

SignalR 不提供任何功能來驗證使用者。 相反地，您將 SignalR 功能整合到現有的應用程式的驗證結構。 因為您會通常在您的應用程式，並使用您 SignalR 中驗證結果程式碼中，您就會驗證使用者。 例如，您可以驗證您的使用者，使用 ASP.NET 表單驗證，並接著在您的中樞，強制執行哪些使用者或角色有權呼叫的方法。 在您的中樞，您也可以傳遞驗證資訊，例如使用者名稱或使用者是否屬於的角色，用戶端。

提供 SignalR[授權](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx)屬性來指定哪些使用者可以存取的集線器或方法。 您將 Authorize 屬性套用至中樞或中樞內的特定方法時。 沒有授權屬性，該中樞上的所有公用方法可連線至中樞的用戶端。 如需中樞的詳細資訊，請參閱[SignalR 中樞的驗證和授權](hub-authorization.md)。

您套用`Authorize`屬性至中樞，但不是持續連線。 若要強制執行時使用授權規則`PersistentConnection`您必須覆寫`AuthorizeRequest`方法。 如需持續連線的詳細資訊，請參閱[SignalR 持續連線的驗證和授權](persistent-connection-authorization.md)。

<a id="connectiontoken"></a>

### <a name="connection-token"></a>連線語彙基元

SignalR 降低執行惡意的命令來驗證寄件者的身分識別的風險。 針對每個要求中，用戶端和伺服器傳遞連線權杖，其中包含已驗證使用者的使用者名稱與連接識別碼。 連線識別碼可唯一識別每個連線的用戶端。 新的連接建立，而且在連接期間持續該識別碼時，伺服器會隨機產生的連線識別碼。 Web 應用程式的驗證機制提供使用者名稱。 SignalR 使用加密和數位簽章來保護的連線語彙基元。

![](introduction-to-security/_static/image2.png)

針對每個要求，伺服器會驗證權杖，以確保要求來自指定之使用者的內容。 使用者名稱必須對應到的連線識別碼。藉由驗證的連線識別碼，而使用者名稱，SignalR 可以防止惡意使用者在輕鬆模擬另一位使用者。 如果伺服器無法驗證的連線語彙基元，要求將會失敗。

![](introduction-to-security/_static/image4.png)

因為連線識別碼是在驗證程序的一部分，不應該顯示給其他使用者的一位使用者的連線識別碼，或這類的用戶端上的值儲存在 cookie 中。

#### <a name="connection-tokens-vs-other-token-types"></a>與其他語彙基元類型的連線語彙基元

因為它們是工作階段權杖或驗證語彙基元，這會帶來風險，如果公開會出現安全性工具偶爾會標示連線權杖。

SignalR 的連線語彙基元不是驗證權杖。 它用來確認提出此要求的使用者是同一個建立連接。 連線語彙基元是必要的因為 ASP.NET SignalR 允許連線到伺服器之間移動。 權杖關聯的特定使用者的連線，但不會判斷提示發出要求之使用者的身分識別。 SignalR 要求適當的驗證，它必須有一些其他判斷提示使用者，例如 cookie 的身分識別的權杖或持有人權杖。 不過，連線權杖本身會由該使用者，只有該權杖內含的連線識別碼提出要求的任何宣告是與該使用者相關聯。

連線語彙基元提供自己的未驗證宣告，因為它不被視為 「 工作階段 」 或 「 驗證 」 權杖。 取得指定的使用者的連線語彙基元，然後重新它執行要求驗證為不同的使用者 （或未驗證的要求） 中將會失敗，因為不符合要求的使用者身分識別並儲存在權杖中的身分識別。

<a id="rejoingroup"></a>

### <a name="rejoining-groups-when-reconnecting"></a>重新連線時，重新加入群組

根據預設，SignalR 應用程式會自動重新將使用者指派給適當的群組從暫時中斷，例如當卸除並重新建立連線逾時之前連線重新連線時。當重新連接後，用戶端會傳遞包含連線識別碼和指派的群組的群組語彙基元。 以數位方式簽署及加密的群組語彙基元。 用戶端重新連線; 之後，保留相同的連線識別碼因此，從重新連線的用戶端傳遞的連線識別碼必須符合用戶端使用先前的連線識別碼。 此驗證可防止惡意使用者傳遞要求加入未經授權的群組，當重新連線。

不過，它是特別注意，群組語彙基元不會過期。 如果使用者屬於群組，在過去，但是已被禁用，從該群組，則該使用者，將可以模擬包含禁止的群組的群組語彙基元。 如果您需要安全地管理哪些使用者屬於哪些群組，您需要在資料庫中，例如儲存在伺服器上，該資料。 然後，將邏輯加入至您在伺服器驗證使用者是否屬於群組的應用程式中。 如需的驗證群組成員資格的範例，請參閱[使用群組](../guide-to-the-api/working-with-groups.md)。

自動重新加入群組時，才適用連接會暫時中斷之後重新連線。 如果使用者中斷連線的巡覽離開應用程式或應用程式重新啟動，您的應用程式必須處理如何將該使用者新增至正確的群組。 如需詳細資訊，請參閱 <<c0> [ 使用群組](../guide-to-the-api/working-with-groups.md)。

<a id="csrf"></a>

## <a name="how-signalr-prevents-cross-site-request-forgery"></a>SignalR 如何防止跨網站偽造要求

跨站台要求偽造 (CSRF) 攻擊會將惡意網站傳送要求給使用者目前登入的有弱點網站的位置。 SignalR 以防止 CSRF 進行惡意的網站，才能建立有效的要求，您的 SignalR 應用程式非常不可能。

### <a name="description-of-csrf-attack"></a>CSRF 攻擊的描述

CSRF 攻擊的範例如下：

1. 使用者登入 www.example.com 中的，使用表單驗證。
2. 伺服器會驗證使用者。 來自伺服器的回應包含驗證 cookie。
3. 而不需要登出，使用者會造訪惡意的網站。 此惡意的網站包含 HTML 格式如下：

    [!code-html[Main](introduction-to-security/samples/sample1.html)]

   請注意，張貼至易受攻擊的站台，不必惡意網站的表單動作。 這是 CSRF 的 「 跨站台 」 部分。
4. 使用者按一下 [提交] 按鈕。 瀏覽器會包含與要求的驗證 cookie。
5. 要求使用者的驗證內容，example.com 伺服器上執行，而且可以執行的任何已驗證的使用者能夠執行的動作項目。

雖然此範例中所需的使用者可以按一下 [表單] 按鈕，惡意的頁面無法的身分執行的指令碼，將 AJAX 要求傳送至您的 SignalR 應用程式。 此外，使用 SSL 不會防止 CSRF 攻擊中，因為惡意的網站可傳送"https://"要求。

一般而言，CSRF 攻擊是可能對網站使用 cookie 進行驗證，因為瀏覽器會將所有相關的 cookie 傳送至目的地網站。 不過，不一定要利用 cookie CSRF 攻擊。 例如，基本和摘要式驗證也是易受攻擊。 使用者登入時基本或摘要式驗證之後，瀏覽器將會自動傳送認證到工作階段結束為止。

### <a name="csrf-mitigations-taken-by-signalr"></a>SignalR 所採取的 CSRF 防護功能

SignalR 採取下列步驟以建立您的應用程式的有效要求時，防止惡意網站。 SignalR 採取這些步驟，根據預設，您不需要採取任何動作，在您的程式碼。

- **停用跨網域要求**SignalR 停用跨網域要求，以防止使用者呼叫 SignalR 端點來自外部網域。 SignalR 會考慮任何來自外部網域為無效的要求，並封鎖該要求。 我們建議您保留這個預設行為。否則，惡意網站可能誘騙使用者將命令傳送給您的網站。 如果您需要使用跨網域要求，請參閱[如何建立跨網域連接](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)。
- **傳入查詢字串，不 cookie 中的連線語彙基元**SignalR 的連線語彙基元，為查詢字串值，而不是傳遞為 cookie。 在 cookie 中儲存的連線語彙基元是不安全的因為瀏覽器也可以在發生惡意程式碼時不小心轉送的連線語彙基元。 此外，查詢字串中傳遞的連線語彙基元可以避免的連線語彙基元保存超過目前的連接。 因此，惡意使用者不能以其他使用者的驗證認證的要求。
- **確認連線語彙基元**中所述[連線語彙基元](#connectiontoken) 區段中，伺服器可讓您知道哪一個連接識別碼是與每個已驗證的使用者相關聯。 伺服器不會處理來自使用者名稱不相符之連線識別碼的任何要求。 不可能的惡意使用者猜到正確的要求因為惡意使用者必須知道的使用者名稱和目前的隨機產生的連線識別碼。只要結束連接時，該連線識別碼就會變成無效。 匿名使用者不應該將任何機密資訊的存取。

<a id="recommendations"></a>

## <a name="signalr-security-recommendations"></a>SignalR 安全性建議

<a id="ssl"></a>

### <a name="secure-socket-layers-ssl-protocol"></a>安全通訊端層 (SSL) 通訊協定

SSL 通訊協定會使用加密來保護用戶端與伺服器之間的資料傳輸。 如果您的 SignalR 應用程式傳輸用戶端與伺服器之間的機密資訊，請使用 SSL 傳輸。 如需有關設定 SSL 的詳細資訊，請參閱 <<c0> [ 如何設定 IIS 7 上的 SSL](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis)。

<a id="groupsecurity"></a>

### <a name="do-not-use-groups-as-a-security-mechanism"></a>做為安全性機制，不使用群組

群組是便利的方式收集相關的使用者，但它們不安全的機制，以便限制存取機密資訊。 當使用者可以自動重新加入群組的重新連線期間，這是特別有用。 相反地，請考慮新增至角色的權限的使用者，並限制只有該角色的成員到中樞方法的存取。 如需根據角色限制存取的範例，請參閱 < [SignalR 中樞的驗證和授權](hub-authorization.md)。 檢查使用者存取群組，重新連接時的範例，請參閱 <<c0> [ 使用群組](../guide-to-the-api/working-with-groups.md)。

<a id="input"></a>

### <a name="safely-handling-input-from-clients"></a>安全地處理來自用戶端的輸入

若要確保惡意使用者不會對其他使用者傳送指令碼，您必須編碼所有來自用戶端的輸入，以供其他用戶端的廣播。 因為您的 SignalR 應用程式可能有許多不同類型的用戶端，您應該編碼接收的用戶端，而不是在伺服器上的訊息。 因此，HTML 編碼適用於 web 用戶端，但不適用於其他類型的用戶端。 例如，web 用戶端方法，以顯示在聊天訊息會安全地處理的使用者名稱和訊息藉由呼叫`html()`函式。

[!code-html[Main](introduction-to-security/samples/sample2.html?highlight=3-4)]

<a id="reconcile"></a>

### <a name="reconciling-a-change-in-user-status-with-an-active-connection"></a>使用中連線的使用者狀態的變更，重新調整

如果使用者的驗證狀態會變更作用中連線存在時，使用者會收到錯誤訊息、 「 作用中的 SignalR 連線期間無法變更使用者的身分識別 」。 在此情況下，您的應用程式應重新連接到伺服器，以確定使用者名稱與連接識別碼進行協調。 例如，如果您的應用程式可讓使用者登出的作用中連接存在時，連接的使用者名稱將不再符合下一個要求傳入的名稱。 您想要停止連線，才能在使用者登出時，以及再重新啟動它。

不過，務必請注意，大部分的應用程式不需要手動停止和啟動的連線。 如果您的應用程式會將使用者重新導向至另一個頁面記錄處理程序，例如 Web Form 應用程式或 MVC 應用程式的預設行為，或登出之後會重新整理目前的頁面，作用中連線會自動中斷連線，而不需要採取任何其他動作。

下列範例示範如何停止和啟動的連線，使用者狀態變更時。

[!code-html[Main](introduction-to-security/samples/sample3.html)]

或者，如果您的網站使用表單驗證，使用滑動期限，而且沒有任何活動，將有效的驗證 cookie，可能會變更使用者的驗證狀態。 在此情況下，會將使用者登出，使用者名稱將不再符合的連線語彙基元中的使用者名稱。 您可以藉由新增一些指令碼定期要求要保持有效的驗證 cookie 的 web 伺服器上的資源來修正這個問題。 下列範例示範如何要求資源每隔 30 分鐘。

[!code-javascript[Main](introduction-to-security/samples/sample4.js)]

<a id="autogen"></a>

### <a name="automatically-generated-javascript-proxy-files"></a>自動產生的 JavaScript proxy 檔案

如果您不想在每個使用者的 JavaScript proxy 檔案中包含所有中樞和方法，您可以停用自動產生的檔案。 如果您有多個中樞和方法，但不是想要注意的所有方法的每位使用者，您可能會選擇此選項。 您設定停用自動產生**EnableJavaScriptProxies**要**false**。

[!code-csharp[Main](introduction-to-security/samples/sample5.cs)]

如需 JavaScript proxy 檔案的詳細資訊，請參閱[產生的 proxy，它會為您](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy)。 <a id="exceptions"></a>

### <a name="exceptions"></a>例外狀況

您應該避免將例外狀況物件傳遞至用戶端，因為物件可能會公開機密資訊的用戶端。 相反地，會顯示相關的錯誤訊息的用戶端上呼叫方法。

[!code-csharp[Main](introduction-to-security/samples/sample6.cs)]
