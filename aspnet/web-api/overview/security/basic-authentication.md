---
uid: web-api/overview/security/basic-authentication
title: ASP.NET Web API 中的基本驗證 |Microsoft 文件
author: MikeWasson
description: 描述如何使用 ASP.NET Web API 中的基本驗證。
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/02/2014
ms.topic: article
ms.assetid: 41423767-0021-47c3-9e53-0021b457c39f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/basic-authentication
msc.type: authoredcontent
ms.openlocfilehash: 4b8e6410668b2db289488bb4b6cd26d881e70f4c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508127"
---
<a name="basic-authentication-in-aspnet-web-api"></a>ASP.NET Web API 中的基本驗證
====================
由[Mike Wasson](https://github.com/MikeWasson)

基本驗證定義在[RFC 2617，HTTP 驗證： 基本與摘要式存取驗證](http://www.ietf.org/rfc/rfc2617.txt)。

缺點

- 在要求中傳送使用者認證。
- 認證會以純文字傳送。
- 每個要求傳送認證。
- 沒有方法可以登出，除了藉由結束瀏覽器工作階段。
- 容易受到跨網站要求偽造 (CSRF);需要反 CSRF 量值。

優點

- 網際網路標準。
- 支援所有主要瀏覽器。
- 相當簡單的通訊協定。

基本驗證的運作方式，如下所示：

1. 如果要求需要驗證，伺服器會傳回 401 （未經授權）。 回應包括 Www-authenticate 標頭，指出伺服器支援基本驗證。
2. 用戶端會傳送另一個要求，Authorization 標頭中的用戶端認證。 認證會格式化為 「 名稱： 密碼 」，以 base64 編碼的字串。 認證不會加密。

基本驗證會執行內容中的"realm"。 伺服器的 Www-authenticate 標頭中包含領域名稱。 使用者的認證會在該範圍內有效。 領域的確切的範圍是伺服器所定義。 例如，您可以定義數個領域資料分割資源的順序。

![](basic-authentication/_static/image1.png)

認證會傳送未加密，因為基本驗證僅保護透過 HTTPS。 請參閱[使用 Web 應用程式開發介面中的 SSL](working-with-ssl-in-web-api.md)。

基本驗證也受到 CSRF 攻擊。 使用者輸入認證之後，瀏覽器自動傳送它們在後續的要求給相同的網域中，工作階段的持續時間。 這包括 AJAX 要求。 請參閱[防止跨網站要求偽造 (CSRF) 攻擊](preventing-cross-site-request-forgery-csrf-attacks.md)。

## <a name="basic-authentication-with-iis"></a>Iis 基本驗證

IIS 支援基本驗證，但沒有注意： 針對其 Windows 認證驗證使用者。 這表示使用者必須擁有伺服器的網域上的帳戶。 公開網站，您通常會想要對 ASP.NET 成員資格提供者進行驗證。

若要啟用基本驗證使用 IIS，請為"Windows"ASP.NET 專案的 Web.config 中設定驗證模式：

[!code-xml[Main](basic-authentication/samples/sample1.xml)]

在此模式中，IIS 會使用 Windows 認證來進行驗證。 此外，您必須啟用 IIS 中的基本驗證。 在 [IIS 管理員] 中，移至 [功能檢視]、 選取 [驗證] 並啟用基本驗證。

![](basic-authentication/_static/image2.png)

在 Web API 專案中，加入`[Authorize]`適用於任何需要驗證的控制器動作的屬性。

當用戶端會驗證本身在要求中設定授權標頭。 瀏覽器用戶端會自動執行此步驟。 Nonbrowser 用戶端必須設定的標頭。

## <a name="basic-authentication-with-custom-membership"></a>使用自訂成員資格的基本驗證

如前所述，內建於 IIS 基本驗證會使用 Windows 認證。 這表示您需要建立使用者帳戶的主控伺服器上。 但在網際網路應用程式使用者帳戶通常儲存在外部資料庫。

下列程式碼如何執行基本驗證的 HTTP 模組。 您可以輕鬆地插入 ASP.NET 成員資格提供者來取代`CheckPassword`方法，這是在此範例中的虛擬方法。

在 Web API 2 中，您應該考慮撰寫[驗證篩選條件](authentication-filters.md)或[OWIN 中介軟體](../../../aspnet/overview/owin-and-katana/index.md)，而不是 HTTP 模組。

[!code-csharp[Main](basic-authentication/samples/sample2.cs)]

若要啟用 HTTP 模組，將下列加入至您的 web.config 檔案中**system.webServer** > 一節：

[!code-xml[Main](basic-authentication/samples/sample3.xml?highlight=4)]

組件 （不包括 「 dll 」 延伸模組） 的名稱取代"YourAssemblyName"。

您應該停用其他的驗證配置，例如表單或 Windows 驗證
