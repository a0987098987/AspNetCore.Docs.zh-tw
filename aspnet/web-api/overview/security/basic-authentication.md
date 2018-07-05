---
uid: web-api/overview/security/basic-authentication
title: ASP.NET Web API 中的基本驗證 |Microsoft Docs
author: MikeWasson
description: 描述如何在 ASP.NET Web API 中使用基本驗證。
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/02/2014
ms.topic: article
ms.assetid: 41423767-0021-47c3-9e53-0021b457c39f
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/security/basic-authentication
msc.type: authoredcontent
ms.openlocfilehash: 9d5610eb61088a8e7573ba6399c771f0957ff437
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395327"
---
<a name="basic-authentication-in-aspnet-web-api"></a>ASP.NET Web API 中的基本驗證
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

中所定義的基本驗證[RFC 2617、 HTTP 驗證： 基本與摘要式存取驗證](http://www.ietf.org/rfc/rfc2617.txt)。

缺點

- 在要求中傳送使用者認證。
- 認證會以純文字傳送。
- 認證會隨著每個要求傳送。
- 無法登出，除非結束瀏覽器工作階段。
- 容易遭受跨網站要求偽造 (CSRF);需要防 CSRF 的量值。

優點

- 網際網路標準。
- 支援所有主要瀏覽器。
- 相當簡單的通訊協定。

基本驗證的運作方式，如下所示：

1. 如果要求必須經過驗證，伺服器就會傳回 401 （未經授權）。 此回應包含 Www-authenticate 標頭，指出伺服器支援基本驗證。
2. 用戶端會傳送另一個要求，使用授權標頭中的用戶端認證。 認證會格式化為 「 名稱： 密碼 」，以 base64 編碼的字串。 不會加密認證。

基本驗證會執行內容中的"realm"。 伺服器的 WWW 驗證標頭中包含的領域名稱。 使用者的認證的有效期內該領域。 領域的確切的範圍是伺服器所定義。 比方說，您可以定義數個領域中資料分割資源的順序。

![](basic-authentication/_static/image1.png)

認證會傳送未加密的因為基本驗證是只保護透過 HTTPS。 請參閱[使用 Web API 中的 SSL](working-with-ssl-in-web-api.md)。

基本驗證也是很容易遭受 CSRF 攻擊。 使用者輸入認證之後，瀏覽器會自動傳送它們在後續要求相同的網域中，工作階段的持續時間。 這包括 AJAX 要求。 請參閱[防止跨網站要求偽造 (CSRF) 攻擊](preventing-cross-site-request-forgery-csrf-attacks.md)。

## <a name="basic-authentication-with-iis"></a>Iis 基本驗證

IIS 支援基本驗證，但有一點要注意： 使用者會針對其 Windows 認證來驗證。 這表示使用者必須擁有伺服器的網域帳戶。 公開網站，您通常會想要對 ASP.NET 成員資格提供者進行驗證。

若要啟用使用 IIS 的基本驗證，將驗證模式設定為您的 ASP.NET 專案的 Web.config 中的"Windows":

[!code-xml[Main](basic-authentication/samples/sample1.xml)]

在此模式中，IIS 會使用 Windows 認證來驗證。 此外，您必須啟用 IIS 中的基本驗證。 在 [IIS 管理員] 中，移至 [功能檢視] 中選取 [驗證]，啟用基本驗證。

![](basic-authentication/_static/image2.png)

在您的 Web API 專案中加入`[Authorize]`適用於任何需要驗證的控制器動作的屬性。

用戶端會進行自我驗證要求中設定授權標頭。 瀏覽器用戶端會自動執行此步驟。 Nonbrowser 用戶端必須設定的標頭。

## <a name="basic-authentication-with-custom-membership"></a>具有自訂成員資格的基本驗證

如所述，內建在 IIS 基本驗證會使用 Windows 認證。 這表示您需要建立使用者帳戶對您的主控伺服器上。 但是，網際網路應用程式，使用者帳戶通常會儲存在外部資料庫。

下列程式碼如何執行基本驗證的 HTTP 模組。 您可以輕鬆地外掛的 ASP.NET 成員資格提供者，藉由取代`CheckPassword`方法，這是在此範例中的虛擬方法。

在 Web API 2 中，您應該考慮撰寫[驗證篩選條件](authentication-filters.md)或是[OWIN 中介軟體](../../../aspnet/overview/owin-and-katana/index.md)，而不是 HTTP 模組。

[!code-csharp[Main](basic-authentication/samples/sample2.cs)]

若要啟用 HTTP 模組，將下列內容新增到您的 web.config 檔案中**system.webServer**區段：

[!code-xml[Main](basic-authentication/samples/sample3.xml?highlight=4)]

（不包括 「 dll 」 擴充功能） 的組件的名稱取代"YourAssemblyName 」。

您應該停用其他的驗證配置，例如 Forms 或 Windows 驗證
