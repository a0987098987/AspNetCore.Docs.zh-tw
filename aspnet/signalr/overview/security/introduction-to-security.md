---
uid: signalr/overview/security/introduction-to-security
title: SignalR 安全性簡介 |Microsoft 文件
author: pfletcher
description: 描述開發 SignalR 應用程式時，您必須考量的安全性問題。
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: ed562717-8591-4936-8e10-c7e63dcb570a
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/security/introduction-to-security
msc.type: authoredcontent
ms.openlocfilehash: 9a4f09c8036d6d662dfdc44d7c7feaba0101e0c6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873897"
---
<a name="introduction-to-signalr-security"></a>SignalR 安全性簡介
====================
由[Patrick Fletcher](https://github.com/pfletcher)， [Tom FitzMacken](https://github.com/tfitzmac)

> 本文將告訴您開發的 SignalR 應用程式時必須考量的安全性問題。 
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

- [SignalR 的安全性概念](#concepts)

    - [驗證和授權](#authentication)
    - [連線語彙基元](#connectiontoken)
    - [當重新連線時重新加入群組](#rejoingroup)
- [SignalR 防止跨站台要求偽造的方式](#csrf)
- [SignalR 的安全性建議](#recommendations)

    - [安全通訊端層 (SSL) 通訊協定](#ssl)
    - [請勿使用群組做為安全性機制](#groupsecurity)
    - [安全地處理來自用戶端的輸入](#input)
    - [使用中連線的使用者狀態的變更，重新調整](#reconcile)
    - [自動產生的 JavaScript proxy 檔案](#autogen)
    - [例外狀況](#exceptions)

<a id="concepts"></a>

## <a name="signalr-security-concepts"></a>SignalR 的安全性概念

<a id="authentication"></a>

### <a name="authentication-and-authorization"></a>驗證與授權

SignalR 不提供任何功能來驗證使用者。 相反地，您將 SignalR 功能整合到現有的驗證結構，應用程式。 因為您會通常在您的應用程式，並使用您 SignalR 中驗證結果程式碼中，您可以驗證使用者。 例如，您可能會使用 ASP.NET 表單驗證，讓使用者通過驗證，並接著在您的中樞，強制執行哪些使用者或角色有權呼叫的方法。 在您的中樞，您也可以傳遞驗證資訊，例如使用者名稱或使用者是否屬於角色，用戶端。

提供 SignalR[授權](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx)屬性來指定哪些使用者可以存取的集線器或方法。 您可以套用 Authorize 屬性集線器或中樞中的特定方法。 若沒有授權屬性集線器上的所有公用方法可用來連線至中樞的用戶端。 如需集線器的詳細資訊，請參閱[SignalR 中樞的驗證和授權](hub-authorization.md)。

您套用`Authorize`屬性中心，但不持續連線。 若要強制執行時使用授權規則`PersistentConnection`必須覆寫`AuthorizeRequest`方法。 如需持續連線的詳細資訊，請參閱[SignalR 持續連線的驗證和授權](persistent-connection-authorization.md)。

<a id="connectiontoken"></a>

### <a name="connection-token"></a>連線語彙基元

SignalR 可減少執行惡意的命令來驗證寄件者的身分識別的風險。 針對每個要求中，用戶端和伺服器傳遞的連線權杖，其中包含連接識別碼和已驗證使用者的使用者名稱。 連接識別碼可唯一識別每個連線的用戶端。 新的連接建立，而且在連接期間保存該識別碼時，伺服器會隨機產生的連線識別碼。 Web 應用程式的驗證機制提供使用者名稱。 SignalR 使用加密和數位簽章來保護的連線語彙基元。

![](introduction-to-security/_static/image2.png)

針對每個要求，伺服器會驗證權杖，以確定要求來自指定之使用者的內容。 使用者名稱必須對應至連接識別碼。驗證的連接識別碼和使用者名稱，以 SignalR 可防止惡意使用者輕鬆地模擬另一位使用者。 如果伺服器無法驗證的連線語彙基元，要求將會失敗。

![](introduction-to-security/_static/image4.png)

因為連接識別碼是在驗證程序的一部分，不應該顯示給其他使用者的一名使用者的連線識別碼，或在 cookie 中儲存的值，在用戶端，例如。

<a id="rejoingroup"></a>

### <a name="rejoining-groups-when-reconnecting"></a>當重新連線時重新加入群組

根據預設，SignalR 應用程式將自動重新指派使用者給適當的群組從暫時中斷，例如當卸除並重新建立連接逾時之前連線重新連線時。當重新連接後，用戶端會傳遞包含連線識別碼並指派的群組的群組語彙基元。 以數位方式簽署及加密的群組語彙基元。 用戶端重新連線; 之後，保留相同的連線識別碼因此，從重新連線的用戶端傳送的連線識別碼必須符合用戶端使用先前的連線識別碼。 此驗證可防止惡意使用者傳遞加入未經授權的群組，重新連接時的要求。

不過，它是很重要的一點的群組語彙基元不會過期。 如果使用者屬於群組，在過去，但是該群組已禁止，該使用者可以模擬包含禁止的群組的群組語彙基元。 如果您需要安全的方式管理哪些使用者所屬的群組，您需要在資料庫中，例如儲存在伺服器上，該資料。 然後，將邏輯加入至您的應用程式，用來在伺服器上驗證使用者是否屬於其他群組。 如需的驗證群組成員資格的範例，請參閱[使用群組](../guide-to-the-api/working-with-groups.md)。

自動重新加入群組時才適用連接暫時中斷之後重新連線。 如果使用者中斷連線的不用離開應用程式或重新啟動應用程式，您的應用程式必須處理如何將該使用者新增至正確的群組。 如需詳細資訊，請參閱[使用群組](../guide-to-the-api/working-with-groups.md)。

<a id="csrf"></a>

## <a name="how-signalr-prevents-cross-site-request-forgery"></a>SignalR 防止跨站台要求偽造的方式

跨站台要求偽造 (CSRF) 攻擊是惡意網站傳送要求到易受攻擊的站台，使用者目前登入的位置。 SignalR 以防止 CSRF 進行極不像是會用來建立您的 SignalR 應用程式的有效要求惡意網站。

### <a name="description-of-csrf-attack"></a>CSRF 攻擊的描述

CSRF 攻擊的範例如下：

1. 使用者登入 www.example.com 時，使用表單驗證。
2. 伺服器會驗證使用者。 伺服器的回應包含驗證 cookie。
3. 沒有登出，使用者會造訪惡意網站。 這個惡意網站包含 HTML 格式如下： 

    [!code-html[Main](introduction-to-security/samples/sample1.html)]

   請注意，張貼至容易遭受站台，惡意網站的表單動作。 這是 CSRF 的 「 跨網站 」 部分。
4. 使用者按一下 [提交] 按鈕。 瀏覽器包含與要求的驗證 cookie。
5. 要求 example.com 與使用者的驗證內容，在伺服器上執行，並可以執行已驗證的使用者允許進行的任何動作。

雖然這個範例需要使用者按一下 [表單] 按鈕，惡意的頁面無法輕鬆地執行指令碼將 AJAX 要求傳送至您的 SignalR 應用程式一樣。 此外，使用 SSL 無法防止 CSRF 攻擊中，因為惡意網站可以傳送"https://"要求。

一般而言，CSRF 攻擊可能會對網站使用 cookie 進行驗證，因為瀏覽器傳送到目標網站的所有相關的 cookie。 不過，不限於利用 cookie CSRF 攻擊。 比方說，也是很容易遭受基本和摘要式驗證。 在使用者登入基本或摘要式驗證後，瀏覽器將會自動傳送認證到工作階段結束為止。

### <a name="csrf-mitigations-taken-by-signalr"></a>由 SignalR CSRF 防護

SignalR 採取下列步驟可防止惡意網站建立您的應用程式的有效要求。 SignalR 會依照依預設，您不需要採取任何動作，在程式碼中。

- **停用跨網域要求**  
 SignalR 停用跨網域要求，以防止使用者從外部網域呼叫 SignalR 端點。 SignalR 會考慮任何來自外部網域無效的要求，並封鎖該要求。 我們建議您保留這個預設行為。否則，惡意網站無法誘騙使用者將命令傳送給您的網站。 如果您需要使用跨網域要求，請參閱[如何建立跨定義域連線](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)。
- **傳入查詢字串，而不是 cookie 的連線語彙基元**  
 SignalR 會當做 cookie，而不是傳遞做為查詢字串值的連線語彙基元。 在 cookie 中儲存的連線語彙基元是不安全，因為瀏覽器也可以在發生惡意程式碼時不小心轉送的連線語彙基元。 此外，查詢字串中傳遞的連線語彙基元會防止的連線語彙基元保存超出目前的連接。 因此，惡意的使用者不能在另一位使用者的驗證認證的要求。
- **確認連線語彙基元**  
 中所述[連線語彙基元](#connectiontoken) 區段中，伺服器會知道哪些連接識別碼是與每個已驗證的使用者相關聯。 伺服器不會處理來自不符合使用者名稱之連線識別碼的任何要求。 不太惡意的使用者無法猜到正確的要求，因為惡意的使用者必須知道的使用者名稱和目前的隨機產生的連線識別碼。結束連接時，該連接識別碼會變成無效。 匿名使用者不應有任何機密資訊的存取權。

<a id="recommendations"></a>

## <a name="signalr-security-recommendations"></a>SignalR 的安全性建議

<a id="ssl"></a>

### <a name="secure-socket-layers-ssl-protocol"></a>安全通訊端層 (SSL) 通訊協定

SSL 通訊協定會使用加密來保護用戶端與伺服器之間的資料傳輸。 如果您的 SignalR 應用程式傳輸用戶端與伺服器之間的機密資訊，請使用 SSL 傳輸。 如需有關設定 SSL 的詳細資訊，請參閱[如何設定 IIS 7 上 SSL](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis)。

<a id="groupsecurity"></a>

### <a name="do-not-use-groups-as-a-security-mechanism"></a>請勿使用群組做為安全性機制

群組是便利的方式收集相關的使用者，但它們不是安全的機制，來限制存取機密資訊。 使用者可以自動重新加入群組期間重新連線時，這是特別有用。 相反地，請考慮將特殊權限的使用者加入至角色，並限制為只有該角色的成員的中樞方法的存取。 如需限制存取權限角色的範例，請參閱[SignalR 中樞的驗證和授權](hub-authorization.md)。 重新連接時，請檢查使用者群組的存取權的範例，請參閱[使用群組](../guide-to-the-api/working-with-groups.md)。

<a id="input"></a>

### <a name="safely-handling-input-from-clients"></a>安全地處理來自用戶端的輸入

若要確保惡意的使用者不會與其他使用者傳送指令碼，您必須編碼所有來自用戶端的輸入適用於其他用戶端的廣播。 因為您的 SignalR 應用程式可能有許多不同類型的用戶端，您應該編碼接收用戶端，而不是在伺服器上的訊息。 因此，HTML 編碼方式可執行 web 用戶端，但不適用於其他類型的用戶端。 例如，web 用戶端方法，以顯示在聊天訊息會安全地處理的使用者名稱和訊息藉由呼叫`html()`函式。

[!code-html[Main](introduction-to-security/samples/sample2.html?highlight=3-4)]

<a id="reconcile"></a>

### <a name="reconciling-a-change-in-user-status-with-an-active-connection"></a>使用中連線的使用者狀態的變更，重新調整

如果使用者的驗證狀態變更時的作用中連接存在時，使用者會收到錯誤指出，「 在使用中的 SignalR 連線期間無法變更使用者的身分識別 」。 在此情況下，您的應用程式應重新連接到伺服器，以確定連接識別碼和使用者名稱會經過協調。 比方說，如果您的應用程式可讓使用者登出的使用中連接存在時，連接的使用者名稱將不再符合下一個要求傳入的名稱。 您會想要使用者登出時，先停止連線，然後重新啟動。

不過，務必注意，大部分的應用程式將不需要以手動方式停止及啟動連線。 如果您的應用程式將使用者重新導向至另一個頁面記錄處理程序，例如 Web Form 應用程式或 MVC 應用程式中的預設行為，或登出之後重新整理目前頁面上，使用中連接自動中斷連線，而不需要任何額外的動作。

下列範例會示範如何停止和啟動連線時使用者狀態已變更。

[!code-html[Main](introduction-to-security/samples/sample3.html)]

或者，如果您的網站使用滑動期限使用表單驗證，而沒有任何活動，以保持有效的驗證 cookie，可能會變更使用者的驗證狀態。 在此情況下，使用者將登出，使用者名稱將不再相符的連線語彙基元中的使用者名稱。 您可以加入一些定期要求要保持有效的驗證 cookie 的 web 伺服器上的資源的指令碼來修正此問題。 下列範例會示範如何要求資源每隔 30 分鐘。

[!code-javascript[Main](introduction-to-security/samples/sample4.js)]

<a id="autogen"></a>

### <a name="automatically-generated-javascript-proxy-files"></a>自動產生的 JavaScript proxy 檔案

如果您不希望在每個使用者的 JavaScript proxy 檔案中包含所有的中樞和方法，您可以停用自動產生的檔案。 如果您有多個中樞和方法，但不是想要注意的所有方法的每個使用者，您可能會選擇此選項。 設定以停用自動產生**EnableJavaScriptProxies**至**false**。

[!code-csharp[Main](introduction-to-security/samples/sample5.cs)]

如需 JavaScript proxy 檔案的詳細資訊，請參閱[產生的 proxy，它會為您](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy)。 <a id="exceptions"></a>

### <a name="exceptions"></a>例外狀況

您應該避免將例外狀況物件傳遞至用戶端，因為物件可能會公開給用戶端機密資訊。 相反地，會顯示相關的錯誤訊息的用戶端上呼叫方法。

[!code-csharp[Main](introduction-to-security/samples/sample6.cs)]
