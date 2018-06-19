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
<a name="basic-authentication-in-aspnet-web-api"></a><span data-ttu-id="3344f-103">ASP.NET Web API 中的基本驗證</span><span class="sxs-lookup"><span data-stu-id="3344f-103">Basic Authentication in ASP.NET Web API</span></span>
====================
<span data-ttu-id="3344f-104">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3344f-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="3344f-105">基本驗證定義在[RFC 2617，HTTP 驗證： 基本與摘要式存取驗證](http://www.ietf.org/rfc/rfc2617.txt)。</span><span class="sxs-lookup"><span data-stu-id="3344f-105">Basic authentication is defined in [RFC 2617, HTTP Authentication: Basic and Digest Access Authentication](http://www.ietf.org/rfc/rfc2617.txt).</span></span>

<span data-ttu-id="3344f-106">缺點</span><span class="sxs-lookup"><span data-stu-id="3344f-106">Disadvantages</span></span>

- <span data-ttu-id="3344f-107">在要求中傳送使用者認證。</span><span class="sxs-lookup"><span data-stu-id="3344f-107">User credentials are sent in the request.</span></span>
- <span data-ttu-id="3344f-108">認證會以純文字傳送。</span><span class="sxs-lookup"><span data-stu-id="3344f-108">Credentials are sent as plaintext.</span></span>
- <span data-ttu-id="3344f-109">每個要求傳送認證。</span><span class="sxs-lookup"><span data-stu-id="3344f-109">Credentials are sent with every request.</span></span>
- <span data-ttu-id="3344f-110">沒有方法可以登出，除了藉由結束瀏覽器工作階段。</span><span class="sxs-lookup"><span data-stu-id="3344f-110">No way to log out, except by ending the browser session.</span></span>
- <span data-ttu-id="3344f-111">容易受到跨網站要求偽造 (CSRF);需要反 CSRF 量值。</span><span class="sxs-lookup"><span data-stu-id="3344f-111">Vulnerable to cross-site request forgery (CSRF); requires anti-CSRF measures.</span></span>

<span data-ttu-id="3344f-112">優點</span><span class="sxs-lookup"><span data-stu-id="3344f-112">Advantages</span></span>

- <span data-ttu-id="3344f-113">網際網路標準。</span><span class="sxs-lookup"><span data-stu-id="3344f-113">Internet standard.</span></span>
- <span data-ttu-id="3344f-114">支援所有主要瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="3344f-114">Supported by all major browsers.</span></span>
- <span data-ttu-id="3344f-115">相當簡單的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="3344f-115">Relatively simple protocol.</span></span>

<span data-ttu-id="3344f-116">基本驗證的運作方式，如下所示：</span><span class="sxs-lookup"><span data-stu-id="3344f-116">Basic authentication works as follows:</span></span>

1. <span data-ttu-id="3344f-117">如果要求需要驗證，伺服器會傳回 401 （未經授權）。</span><span class="sxs-lookup"><span data-stu-id="3344f-117">If a request requires authentication, the server returns 401 (Unauthorized).</span></span> <span data-ttu-id="3344f-118">回應包括 Www-authenticate 標頭，指出伺服器支援基本驗證。</span><span class="sxs-lookup"><span data-stu-id="3344f-118">The response includes a WWW-Authenticate header, indicating the server supports Basic authentication.</span></span>
2. <span data-ttu-id="3344f-119">用戶端會傳送另一個要求，Authorization 標頭中的用戶端認證。</span><span class="sxs-lookup"><span data-stu-id="3344f-119">The client sends another request, with the client credentials in the Authorization header.</span></span> <span data-ttu-id="3344f-120">認證會格式化為 「 名稱： 密碼 」，以 base64 編碼的字串。</span><span class="sxs-lookup"><span data-stu-id="3344f-120">The credentials are formatted as the string "name:password", base64-encoded.</span></span> <span data-ttu-id="3344f-121">認證不會加密。</span><span class="sxs-lookup"><span data-stu-id="3344f-121">The credentials are not encrypted.</span></span>

<span data-ttu-id="3344f-122">基本驗證會執行內容中的"realm"。</span><span class="sxs-lookup"><span data-stu-id="3344f-122">Basic authentication is performed within the context of a "realm."</span></span> <span data-ttu-id="3344f-123">伺服器的 Www-authenticate 標頭中包含領域名稱。</span><span class="sxs-lookup"><span data-stu-id="3344f-123">The server includes the name of the realm in the WWW-Authenticate header.</span></span> <span data-ttu-id="3344f-124">使用者的認證會在該範圍內有效。</span><span class="sxs-lookup"><span data-stu-id="3344f-124">The user's credentials are valid within that realm.</span></span> <span data-ttu-id="3344f-125">領域的確切的範圍是伺服器所定義。</span><span class="sxs-lookup"><span data-stu-id="3344f-125">The exact scope of a realm is defined by the server.</span></span> <span data-ttu-id="3344f-126">例如，您可以定義數個領域資料分割資源的順序。</span><span class="sxs-lookup"><span data-stu-id="3344f-126">For example, you might define several realms in order to partition resources.</span></span>

![](basic-authentication/_static/image1.png)

<span data-ttu-id="3344f-127">認證會傳送未加密，因為基本驗證僅保護透過 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="3344f-127">Because the credentials are sent unencrypted, Basic authentication is only secure over HTTPS.</span></span> <span data-ttu-id="3344f-128">請參閱[使用 Web 應用程式開發介面中的 SSL](working-with-ssl-in-web-api.md)。</span><span class="sxs-lookup"><span data-stu-id="3344f-128">See [Working with SSL in Web API](working-with-ssl-in-web-api.md).</span></span>

<span data-ttu-id="3344f-129">基本驗證也受到 CSRF 攻擊。</span><span class="sxs-lookup"><span data-stu-id="3344f-129">Basic authentication is also vulnerable to CSRF attacks.</span></span> <span data-ttu-id="3344f-130">使用者輸入認證之後，瀏覽器自動傳送它們在後續的要求給相同的網域中，工作階段的持續時間。</span><span class="sxs-lookup"><span data-stu-id="3344f-130">After the user enters credentials, the browser automatically sends them on subsequent requests to the same domain, for the duration of the session.</span></span> <span data-ttu-id="3344f-131">這包括 AJAX 要求。</span><span class="sxs-lookup"><span data-stu-id="3344f-131">This includes AJAX requests.</span></span> <span data-ttu-id="3344f-132">請參閱[防止跨網站要求偽造 (CSRF) 攻擊](preventing-cross-site-request-forgery-csrf-attacks.md)。</span><span class="sxs-lookup"><span data-stu-id="3344f-132">See [Preventing Cross-Site Request Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>

## <a name="basic-authentication-with-iis"></a><span data-ttu-id="3344f-133">Iis 基本驗證</span><span class="sxs-lookup"><span data-stu-id="3344f-133">Basic Authentication with IIS</span></span>

<span data-ttu-id="3344f-134">IIS 支援基本驗證，但沒有注意： 針對其 Windows 認證驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="3344f-134">IIS supports Basic authentication, but there is a caveat: The user is authenticated against their Windows credentials.</span></span> <span data-ttu-id="3344f-135">這表示使用者必須擁有伺服器的網域上的帳戶。</span><span class="sxs-lookup"><span data-stu-id="3344f-135">That means the user must have an account on the server's domain.</span></span> <span data-ttu-id="3344f-136">公開網站，您通常會想要對 ASP.NET 成員資格提供者進行驗證。</span><span class="sxs-lookup"><span data-stu-id="3344f-136">For a public-facing web site, you typically want to authenticate against an ASP.NET membership provider.</span></span>

<span data-ttu-id="3344f-137">若要啟用基本驗證使用 IIS，請為"Windows"ASP.NET 專案的 Web.config 中設定驗證模式：</span><span class="sxs-lookup"><span data-stu-id="3344f-137">To enable Basic authentication using IIS, set the authentication mode to "Windows" in the Web.config of your ASP.NET project:</span></span>

[!code-xml[Main](basic-authentication/samples/sample1.xml)]

<span data-ttu-id="3344f-138">在此模式中，IIS 會使用 Windows 認證來進行驗證。</span><span class="sxs-lookup"><span data-stu-id="3344f-138">In this mode, IIS uses Windows credentials to authenticate.</span></span> <span data-ttu-id="3344f-139">此外，您必須啟用 IIS 中的基本驗證。</span><span class="sxs-lookup"><span data-stu-id="3344f-139">In addition, you must enable Basic authentication in IIS.</span></span> <span data-ttu-id="3344f-140">在 [IIS 管理員] 中，移至 [功能檢視]、 選取 [驗證] 並啟用基本驗證。</span><span class="sxs-lookup"><span data-stu-id="3344f-140">In IIS Manager, go to Features View, select Authentication, and enable Basic authentication.</span></span>

![](basic-authentication/_static/image2.png)

<span data-ttu-id="3344f-141">在 Web API 專案中，加入`[Authorize]`適用於任何需要驗證的控制器動作的屬性。</span><span class="sxs-lookup"><span data-stu-id="3344f-141">In your Web API project, add the `[Authorize]` attribute for any controller actions that need authentication.</span></span>

<span data-ttu-id="3344f-142">當用戶端會驗證本身在要求中設定授權標頭。</span><span class="sxs-lookup"><span data-stu-id="3344f-142">A client authenticates itself by setting the Authorization header in the request.</span></span> <span data-ttu-id="3344f-143">瀏覽器用戶端會自動執行此步驟。</span><span class="sxs-lookup"><span data-stu-id="3344f-143">Browser clients perform this step automatically.</span></span> <span data-ttu-id="3344f-144">Nonbrowser 用戶端必須設定的標頭。</span><span class="sxs-lookup"><span data-stu-id="3344f-144">Nonbrowser clients will need to set the header.</span></span>

## <a name="basic-authentication-with-custom-membership"></a><span data-ttu-id="3344f-145">使用自訂成員資格的基本驗證</span><span class="sxs-lookup"><span data-stu-id="3344f-145">Basic Authentication with Custom Membership</span></span>

<span data-ttu-id="3344f-146">如前所述，內建於 IIS 基本驗證會使用 Windows 認證。</span><span class="sxs-lookup"><span data-stu-id="3344f-146">As mentioned, the Basic Authentication built into IIS uses Windows credentials.</span></span> <span data-ttu-id="3344f-147">這表示您需要建立使用者帳戶的主控伺服器上。</span><span class="sxs-lookup"><span data-stu-id="3344f-147">That means you need to create accounts for your users on the hosting server.</span></span> <span data-ttu-id="3344f-148">但在網際網路應用程式使用者帳戶通常儲存在外部資料庫。</span><span class="sxs-lookup"><span data-stu-id="3344f-148">But for an internet application, user accounts are typically stored in an external database.</span></span>

<span data-ttu-id="3344f-149">下列程式碼如何執行基本驗證的 HTTP 模組。</span><span class="sxs-lookup"><span data-stu-id="3344f-149">The following code how an HTTP module that performs Basic Authentication.</span></span> <span data-ttu-id="3344f-150">您可以輕鬆地插入 ASP.NET 成員資格提供者來取代`CheckPassword`方法，這是在此範例中的虛擬方法。</span><span class="sxs-lookup"><span data-stu-id="3344f-150">You can easily plug in an ASP.NET membership provider by replacing the `CheckPassword` method, which is a dummy method in this example.</span></span>

<span data-ttu-id="3344f-151">在 Web API 2 中，您應該考慮撰寫[驗證篩選條件](authentication-filters.md)或[OWIN 中介軟體](../../../aspnet/overview/owin-and-katana/index.md)，而不是 HTTP 模組。</span><span class="sxs-lookup"><span data-stu-id="3344f-151">In Web API 2, you should consider writing an [authentication filter](authentication-filters.md) or [OWIN middleware](../../../aspnet/overview/owin-and-katana/index.md), instead of an HTTP module.</span></span>

[!code-csharp[Main](basic-authentication/samples/sample2.cs)]

<span data-ttu-id="3344f-152">若要啟用 HTTP 模組，將下列加入至您的 web.config 檔案中**system.webServer** > 一節：</span><span class="sxs-lookup"><span data-stu-id="3344f-152">To enable the HTTP module, add the following to your web.config file in the **system.webServer** section:</span></span>

[!code-xml[Main](basic-authentication/samples/sample3.xml?highlight=4)]

<span data-ttu-id="3344f-153">組件 （不包括 「 dll 」 延伸模組） 的名稱取代"YourAssemblyName"。</span><span class="sxs-lookup"><span data-stu-id="3344f-153">Replace "YourAssemblyName" with the name of the assembly (not including the "dll" extension).</span></span>

<span data-ttu-id="3344f-154">您應該停用其他的驗證配置，例如表單或 Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="3344f-154">You should disable other authentication schemes, such as Forms or Windows auth.</span></span>
