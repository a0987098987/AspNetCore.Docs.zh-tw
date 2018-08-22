---
uid: web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
title: ASP.NET Web API 中驗證和授權 |Microsoft Docs
author: MikeWasson
description: ASP.NET Web API 中提供驗證和授權的一般概觀。
ms.author: riande
ms.date: 11/27/2012
ms.assetid: 6dfb51ea-9f4d-4e70-916c-8ef8344a88d6
msc.legacyurl: /web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: a78606a74b2149e68e3b01f4fe204f4a13edf4b5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834200"
---
<a name="authentication-and-authorization-in-aspnet-web-api"></a>ASP.NET Web API 中驗證和授權
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

您已建立的 web API，但現在您想要控制其存取權。 在本系列的文章中，我們將探討一些選項來從未經授權的使用者保護的 web API。 此系列將會涵蓋驗證和授權。

- *驗證*了解使用者的身分識別。 例如，Alice 登入時她的使用者名稱和密碼，而伺服器會使用密碼驗證 Alice。
- *授權*決定是否允許使用者執行的動作。 例如，Alice 已取得資源，但不是會建立資源的權限。

數列中的第一篇文章提供 ASP.NET Web API 中的驗證和授權的一般概觀。 其他主題會說明常見驗證案例，Web api。

> [!NOTE]
> 檢閱這一系列，並提供寶貴意見的人感謝： Rick Anderson、 於 Levi Broderick、 Barry Dorrans、 Tom Dykstra、 Hongmei Ge、 David Matson、 Daniel Roth、 Tim Teebken。


## <a name="authentication"></a>驗證

Web API 會假設該驗證會在主機中。 適用於虛擬主機，主機會為 IIS 中，使用 HTTP 模組進行驗證。 您可以將專案設定為使用任何內建在 IIS 或 ASP.NET 的驗證模組，或撰寫您自己的 HTTP 模組，來執行自訂驗證。

當主應用程式驗證使用者時，它會建立*主體*，即[IPrincipal](https://msdn.microsoft.com/library/System.Security.Principal.IPrincipal.aspx)物件，表示執行程式碼的安全性內容。 主機設定將主體附加至目前的執行緒**Thread.CurrentPrincipal**。 主體包含相關聯**識別**物件，其中包含使用者的相關資訊。 如果使用者經過驗證， **Identity.IsAuthenticated**屬性會傳回 **，則為 true**。 匿名要求，如**IsAuthenticated**會傳回**false**。 如需有關主體的詳細資訊，請參閱[角色為基礎的安全性](https://msdn.microsoft.com/library/shz8h065.aspx)。

### <a name="http-message-handlers-for-authentication"></a>驗證的 HTTP 訊息處理常式

而不是使用主應用程式進行驗證，您可以將驗證邏輯至放[HTTP 訊息處理常式](../advanced/http-message-handlers.md)。 在此情況下，訊息處理常式會檢查 HTTP 要求，並設定主體。

何時應該使用訊息處理常式進行驗證？ 以下是一些利弊：

- HTTP 模組就會看到瀏覽 ASP.NET 管線的所有要求。 訊息處理常式只會看到會路由傳送至 Web API 的要求。
- 您可以設定每個路由訊息處理常式，讓您將驗證配置套用至特定的路由。
- HTTP 模組是 IIS 專用的。 訊息處理常式會是主機無從驗證，因此它們可以搭配 web 裝載及自我裝載。
- HTTP 模組是由 IIS 記錄、 稽核等等所參與。
- HTTP 模組執行稍早在管線中。 如果您處理驗證的訊息處理常式中，主體不會取得設定的處理常式執行之前。 此外，主體會還原回先前的主體時在回應傳出後的訊息處理常式。

一般而言，如果您不需要支援自我裝載，HTTP 模組就會是較好的選擇。 如果您需要支援自我裝載，請考慮訊息處理常式。

### <a name="setting-the-principal"></a>設定主體

如果您的應用程式會執行任何自訂驗證邏輯，您必須設定主體的兩個地方：

- **Thread.CurrentPrincipal**。 這個屬性是設定執行緒主體，在.NET 中的標準方式。
- **HttpContext.Current.User**。 這個屬性是 ASP.NET 特定的項目。

下列程式碼示範如何設定主體：

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample1.cs)]

適用於虛擬主機，您必須設定主體在兩個位置中;否則您可能會不一致的安全性內容。 自我裝載，但是**HttpContext.Current**為 null。 若要確保您的程式碼與主機無關，因此，檢查是否有 null 指派之前**HttpContext.Current**，如所示。

## <a name="authorization"></a>授權

授權會在管線中，稍後更靠近控制站。 可讓您進行更細微的選項，當您授與資源的存取權。

- *授權篩選條件*控制器動作之前執行。 如果要求未獲授權，篩選條件會傳回錯誤回應，並不會叫用動作。
- 控制器動作，在中，您可以取得從目前的主體**ApiController.User**屬性。 例如，您可能會篩選資源清單，根據使用者名稱，傳回只有該使用者所屬的資源。

![](authentication-and-authorization-in-aspnet-web-api/_static/image1.png)

<a id="auth3"></a>
### <a name="using-the-authorize-attribute"></a>使用 [授權] 屬性

Web API 提供的內建的授權篩選條件， [AuthorizeAttribute](https://msdn.microsoft.com/library/system.web.http.authorizeattribute.aspx)。 此篩選條件會檢查使用者是否經過驗證。 如果沒有，它會傳回 HTTP 狀態碼 401 （未經授權），而不叫用動作。

您可以套用全域，在控制器層級，或個別動作的層級的篩選器。

**全域**： 若要限制每個 Web API 控制器的存取權，新增**AuthorizeAttribute**全域篩選器清單的篩選：

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample2.cs)]

**控制器**： 若要限制特定控制器的存取，將篩選做為屬性新增至控制器：

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample3.cs)]

**動作**： 若要限制特定動作的存取，請將屬性新增至動作方法：

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample4.cs)]

或者，您可以限制控制器，然後允許匿名存取特定的動作，使用`[AllowAnonymous]`屬性。 在下列範例中，`Post`方法是限制，但`Get`方法允許匿名存取。

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample5.cs)]

在上一個範例中，篩選條件允許任何通過驗證的使用者存取受限制的方法;只有匿名使用者會保留。您也可以限制特定使用者或特定角色中使用者的存取：

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample6.cs)]

> [!NOTE]
> **AuthorizeAttribute** Web API 控制器的篩選器位於**System.Web.Http**命名空間。 沒有類似的篩選條件中的 MVC 控制器**System.Web.Mvc**命名空間，與 Web API 控制器不相容。


### <a name="custom-authorization-filters"></a>自訂授權篩選條件

若要撰寫自訂授權篩選條件，衍生自其中一種類型：

- **AuthorizeAttribute**。 擴充此類別來執行目前的使用者和使用者的角色為基礎的授權邏輯。
- **AuthorizationFilterAttribute**。 擴充此類別來執行目前的使用者或角色不一定是以同步的授權邏輯。
- **IAuthorizationFilter**。 實作這個介面來執行非同步的授權邏輯;例如，如果您的授權邏輯會非同步的 I/O 或網路呼叫。 (如果您授權邏輯 CPU 繫結，則是衍生自的工作變得更容易**AuthorizationFilterAttribute**，因為您不需要寫入為非同步方法。)

下圖顯示的類別階層**AuthorizeAttribute**類別。

![](authentication-and-authorization-in-aspnet-web-api/_static/image2.png)

### <a name="authorization-inside-a-controller-action"></a>在控制器動作授權

在某些情況下，您可能會允許繼續進行，但變更行為的主體為基礎的要求。 比方說，您所傳回的資訊可能會變更，視使用者的角色而定。 在控制器方法中，您可以取得從目前的主體**ApiController.User**屬性。

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample7.cs)]
