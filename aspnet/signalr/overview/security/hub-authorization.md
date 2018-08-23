---
uid: signalr/overview/security/hub-authorization
title: SignalR 中樞的驗證和授權 |Microsoft Docs
author: pfletcher
description: 本主題描述如何限制哪些使用者或角色可以存取中樞方法。 本主題中使用的軟體版本 Visual Studio 2013.NET 4.5 SignalR ve...
ms.author: riande
ms.date: 01/05/2015
ms.assetid: a610c796-c131-473c-baef-2e6c568cb2a2
msc.legacyurl: /signalr/overview/security/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: 21946b9fa0406de79af7b83809869d90e6e8234e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832636"
---
<a name="authentication-and-authorization-for-signalr-hubs"></a>SignalR 中樞的驗證和授權
====================
藉由[Patrick Fletcher](https://github.com/pfletcher)， [Tom FitzMacken](https://github.com/tfitzmac)

> 本主題描述如何限制哪些使用者或角色可以存取中樞方法。 
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

此主題包括下列章節：

- [授權屬性](#authorizeattribute)
- [驗證所需的所有中樞](#requireauth)
- [自訂的授權](#custom)
- [將驗證資訊傳遞給用戶端](#passauth)
- [.NET 用戶端的驗證選項](#authoptions)

    - [使用表單驗證 cookie](#cookie)
    - [Windows 驗證](#windows)
    - [連接標頭](#header)
    - [憑證](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a>授權屬性

提供 SignalR[授權](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx)屬性來指定哪些使用者或角色有中樞或方法的存取權。 此屬性位於`Microsoft.AspNet.SignalR`命名空間。 您套用`Authorize`中樞或中樞內的特定方法的屬性。 當您將套用`Authorize`hub 類別，指定的授權需求的屬性會套用到所有中樞中的方法。 本主題提供的授權需求，您可以套用不同類型的範例。 不含`Authorize`屬性，已連接用戶端可以存取任何中樞上的公用方法。

如果您已定義名為"Admin"，web 應用程式中的角色，您可以指定只有該角色中的使用者可以存取為下列程式碼中樞。

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

或者，您可以指定中樞都包含一個方法，可供所有使用者，並只會提供給已驗證的使用者，如下所示的第二個方法。

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

下列範例會處理不同的授權案例：

- `[Authorize]` – 只有經過驗證的使用者
- `[Authorize(Roles = "Admin,Manager")]` – 只有經過驗證的指定角色的使用者
- `[Authorize(Users = "user1,user2")]` – 只有經過驗證的使用者指定的使用者名稱
- `[Authorize(RequireOutgoing=false)]` – 只有已驗證的使用者可以叫用中樞，但從伺服器傳回給用戶端的呼叫不會受到授權，例如，只有特定使用者可以傳送訊息，但所有其他人可以接收訊息時。 RequireOutgoing 屬性只能套用至整個中樞，不需在中樞內的個人方法。 當 RequireOutgoing 未設定為 false 時，只有符合授權需求的使用者會呼叫從伺服器中。

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a>驗證所需的所有中樞

您可以對所有主機和主機方法要求驗證，應用程式中，藉由呼叫[RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx)應用程式啟動時的方法。 當您有多個中樞，想要針對所有人都強制執行的驗證需求，您可以使用這個方法。 使用此方法，您無法指定角色、 使用者或外寄的授權的需求。 您只能指定中樞方法的存取權僅限於已驗證的使用者。 不過，您仍然可以套用 Authorize 屬性中心或方法，來指定額外的需求。 您指定在屬性中的任何需求會新增至驗證的基本需求。

下列範例顯示已驗證的使用者限制所有中樞方法的啟動檔案。

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

如果您呼叫`RequireAuthentication()`SignalR 要求處理完之後的方法，SignalR 會擲回`InvalidOperationException`例外狀況。 SignalR 擲回這個例外狀況，因為您無法將模組新增至 HubPipeline，在叫用管線之後。 先前的範例示範呼叫`RequireAuthentication`方法中的`Configuration`方法會執行一次，再處理第一個要求。

<a id="custom"></a>

## <a name="customized-authorization"></a>自訂的授權

如果您需要自訂如何決定授權，您可以建立衍生自類別`AuthorizeAttribute`，並覆寫[UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx)方法。 針對每個要求，SignalR 叫用這個方法，以判斷是否授權使用者完成要求。 在覆寫方法中，您提供您授權的案例所需的邏輯。 下列範例示範如何強制執行授權透過宣告式身分識別。

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a>將驗證資訊傳遞給用戶端

您可能需要在用戶端執行的程式碼中使用的驗證資訊。 在用戶端上呼叫方法時，您會傳遞所需的資訊。 比方說，聊天應用程式方法可以當做參數傳遞張貼訊息，該人員的使用者名稱，如下所示。

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

或者，您可以建立代表驗證資訊，並將該物件傳遞做為參數的物件，如下所示。

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

您永遠不應該傳遞一個用戶端連接識別碼其他用戶端，因為惡意使用者無法使用它來模擬該用戶端的要求。

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a>.NET 用戶端的驗證選項

當您有.NET 用戶端，例如主控台應用程式，其與限制為已驗證的使用者中樞互動時，您可以傳遞 cookie、 連接標頭，或憑證的驗證認證。 在本節中的範例示範如何使用這些不同的方法來驗證使用者。 它們不是功能完整的 SignalR 應用程式。 如需有關使用 SignalR 的.NET 用戶端的詳細資訊，請參閱[中樞 API 指南-.NET 用戶端](../guide-to-the-api/hubs-api-guide-net-client.md)。

<a id="cookie"></a>

### <a name="cookie"></a>Cookie

當您的.NET 用戶端與 ASP.NET 表單驗證會使用中樞互動時，您必須手動設定連接上的 驗證 cookie。 您加入的 cookie`CookieContainer`上的屬性[HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx)物件。 下列範例示範從網頁擷取驗證 cookie，並將該 cookie 加入至連接的主控台應用程式。

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

主控台應用程式會張貼到認證<strong>www.contoso.com/RemoteLogin</strong>它可以參考包含下列程式碼後置檔案的空白頁面。

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a>Windows 驗證

使用 Windows 驗證時，您可以藉由傳遞目前使用者的認證[DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx)屬性。 您可以設定連線認證 DefaultCredentials 的值。

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a>連接標頭

如果您的應用程式不使用 cookie，您可以在連接標頭中傳遞使用者資訊。 比方說，您也可以在連接標頭中傳遞權杖。

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

然後，在中樞中，您會確認使用者的權杖。

<a id="certificate"></a>

### <a name="certificate"></a>憑證

您可以傳遞用戶端憑證來驗證使用者。 建立連線時，您可以加入憑證。 下列範例示範如何只將用戶端憑證新增至連線;它不會顯示完整的主控台應用程式。 它會使用[X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx)類別可提供數個不同的方式，來建立憑證。

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
