---
uid: web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
title: "驗證和授權的 ASP.NET Web API |Microsoft 文件"
author: MikeWasson
description: "ASP.NET Web API 中提供驗證和授權的一般概觀。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/27/2012
ms.topic: article
ms.assetid: 6dfb51ea-9f4d-4e70-916c-8ef8344a88d6
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 2a4b5ed8a712b061b4afdf5a3adc9378dd72b37f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="authentication-and-authorization-in-aspnet-web-api"></a>驗證和授權的 ASP.NET Web API
====================
由[Mike Wasson](https://github.com/MikeWasson)

您已建立 web API，但現在您想要控制其存取權。 在這一系列的文章中，我們將查看來自未經授權的使用者保護的 web API 的一些選項。 驗證和授權，將涵蓋這一系列。

- *驗證*了解使用者的身分識別。 例如，Alice 登入時她的使用者名稱和密碼，而伺服器會使用密碼驗證 Alice。
- *授權*會決定是否允許使用者執行的動作。 例如，Alice 擁有取得資源，但不是建立資源的權限。

數列中的第一篇文章提供 ASP.NET Web API 驗證和授權的一般概觀。 其他主題將描述 Web API 的一般驗證案例。

> [!NOTE]
> 這點受惠人檢閱此數列，並提供珍貴的回函： Rick Anderson、 Levi Broderick、 Barry Dorrans、 Tom Dykstra、 Hongmei Ge、 David Matson、 奧 Roth、 Tim Teebken。


## <a name="authentication"></a>驗證

Web API 會假設該驗證發生在主機中。 適用於虛擬主機，主機會為 HTTP 模組用於驗證的 IIS。 您可以將專案設定為使用任何驗證模組內建於 IIS 或 ASP.NET，或撰寫您自己的 HTTP 模組來執行自訂驗證。

當主應用程式會驗證使用者時，它會建立*主體*，也就是[IPrincipal](https://msdn.microsoft.com/library/System.Security.Principal.IPrincipal.aspx)物件，表示執行程式碼的安全性內容。 主機將主體附加至目前的執行緒藉由設定**Thread.CurrentPrincipal**。 主體包含相關聯**識別**物件，其中包含使用者的相關資訊。 如果使用者已經過驗證， **Identity.IsAuthenticated**屬性會傳回**true**。 針對匿名要求， **IsAuthenticated**傳回**false**。 如需有關主體的詳細資訊，請參閱[以角色為基礎的安全性](https://msdn.microsoft.com/library/shz8h065.aspx)。

### <a name="http-message-handlers-for-authentication"></a>HTTP 訊息處理常式進行驗證

而不是使用主應用程式進行驗證，您可以將驗證邏輯[HTTP 訊息處理常式](../advanced/http-message-handlers.md)。 在此情況下，訊息處理常式會檢查 HTTP 要求，並設定主體。

何時應該使用訊息處理常式進行驗證？ 以下是一些權衡取捨：

- HTTP 模組會看到瀏覽 ASP.NET 管線的所有要求。 訊息處理常式只會看到會路由傳送至 Web API 的要求。
- 您可以設定每個路由訊息處理常式，它可讓您將驗證配置套用至特定的路徑。
- HTTP 模組是 IIS 專用的。 訊息處理常式是主機無從驗證，因此它們可以用於 web 裝載與自我裝載。
- HTTP 模組參與 IIS 記錄、 稽核和等等。
- 稍早在管線中，執行 HTTP 模組。 如果您處理的訊息處理常式中的驗證時，主體不會取得設定此處理常式執行之前。 此外，主體會還原回先前的主體時在回應傳出後的訊息處理常式。

一般而言，如果您不需要支援自我裝載，HTTP 模組是更好的選項。 如果您需要支援自我裝載，請考慮訊息處理常式。

### <a name="setting-the-principal"></a>設定主體

如果您的應用程式會執行任何自訂驗證邏輯，您必須設定兩個位置上的主體：

- **Thread.CurrentPrincipal**。 這個屬性是在.NET 中設定執行緒的主體的標準方式。
- **HttpContext.Current.User**. 這個屬性是 ASP.NET 特定的項目。

下列程式碼會示範如何設定主體：

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample1.cs)]

適用於虛擬主機，您必須設定主體兩個地方。否則安全性內容可能會不一致。 自我裝載，不過， **HttpContext.Current**為 null。 若要確保您的程式碼是主機無從驗證，因此，檢查是否有 null 指派之前**HttpContext.Current**，如下所示。

## <a name="authorization"></a>授權

稍後在管線中發生授權更靠近控制站。 可讓您進行更細微的選擇，當您授與資源的存取權。

- *授權篩選條件*控制器動作之前執行。 如果要求未獲授權，篩選會傳回錯誤回應，並不會叫用動作。
- 在控制器動作，就可以從目前的主體**ApiController.User**屬性。 例如，您可以篩選資源清單，根據使用者名稱，傳回只能隸屬於該使用者的資源。

![](authentication-and-authorization-in-aspnet-web-api/_static/image1.png)

<a id="auth3"></a>
### <a name="using-the-authorize-attribute"></a>使用 [授權] 屬性

Web 應用程式開發介面提供的內建的授權篩選條件中， [AuthorizeAttribute](https://msdn.microsoft.com/library/system.web.http.authorizeattribute.aspx)。 此篩選條件會檢查是否已驗證使用者。 如果沒有，它會傳回 HTTP 狀態碼 401 （未經授權），而不叫用動作。

您可以套用全域，在控制器層級，或 inidivual 動作的層級的篩選條件。

**全域**： 若要限制每個 Web API 控制器的存取，請加入**AuthorizeAttribute**全域篩選清單中的篩選：

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample2.cs)]

**控制器**： 若要限制特定控制器的存取權，將篩選做為屬性加入至控制器：

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample3.cs)]

**動作**： 若要限制特定動作的存取，請將屬性加入至動作方法：

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample4.cs)]

或者，您可以限制控制器，然後允許匿名存取特定的動作，使用`[AllowAnonymous]`屬性。 在下列範例中，`Post`方法受到限制，但`Get`方法允許匿名存取。

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample5.cs)]

在上一個範例中，篩選條件允許任何通過驗證的使用者存取受限制的方法。只有匿名使用者會保留。您也可以限制特定使用者或特定角色中使用者的存取：

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample6.cs)]

> [!NOTE]
> **AuthorizeAttribute** Web API 控制器的篩選器位於**System.Web.Http**命名空間。 沒有類似的篩選條件的 MVC 控制器中**System.Web.Mvc**命名空間，與 Web API 控制器不相容。


### <a name="custom-authorization-filters"></a>自訂的授權篩選條件

若要撰寫自訂的授權篩選條件，衍生自這些類型之一：

- **AuthorizeAttribute**。 擴充此類別來執行目前的使用者和使用者的角色為基礎的授權邏輯。
- **AuthorizationFilterAttribute**。 擴充此類別來執行同步授權邏輯不一定是根據目前的使用者或角色。
- **IAuthorizationFilter**. 實作這個介面來執行非同步的授權邏輯。例如，如果您的授權邏輯呼叫非同步 I/O 或網路。 (如果受限於 CPU 授權邏輯，比較容易衍生自**AuthorizationFilterAttribute**，因為您不需要撰寫的非同步方法。)

下圖顯示的類別階層**AuthorizeAttribute**類別。

![](authentication-and-authorization-in-aspnet-web-api/_static/image2.png)

### <a name="authorization-inside-a-controller-action"></a>內部控制器動作的授權

在某些情況下，您可能會允許要求繼續進行，但變更主體為基礎的行為。 例如，您所傳回的資訊可能會變更視使用者的角色而定。 在控制器方法中，就可以從目前的原則**ApiController.User**屬性。

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample7.cs)]
