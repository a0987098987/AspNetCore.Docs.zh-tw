---
title: "ASP.NET Core 中的工作階段與應用程式狀態"
author: rick-anderson
description: "在要求之間保留應用程式和使用者 (工作階段) 狀態的方法。"
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 11/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/app-state
ms.openlocfilehash: f4ed38f7395e3f4fe939584c1f3f5b0dba93724c
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/01/2018
---
# <a name="introduction-to-session-and-application-state-in-aspnet-core"></a><span data-ttu-id="0c634-103">ASP.NET Core 中的工作階段與應用程式狀態簡介</span><span class="sxs-lookup"><span data-stu-id="0c634-103">Introduction to session and application state in ASP.NET Core</span></span>

<span data-ttu-id="0c634-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)、[Steve Smith](https://ardalis.com/) 和 [Diana LaRose](https://github.com/DianaLaRose)</span><span class="sxs-lookup"><span data-stu-id="0c634-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), and [Diana LaRose](https://github.com/DianaLaRose)</span></span>

<span data-ttu-id="0c634-105">HTTP 是無狀態的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="0c634-105">HTTP is a stateless protocol.</span></span> <span data-ttu-id="0c634-106">Web 伺服器將每個 HTTP 要求視為獨立要求，而不會保留上一個要求中的使用者值。</span><span class="sxs-lookup"><span data-stu-id="0c634-106">A web server treats each HTTP request as an independent request and doesn't retain user values from previous requests.</span></span> <span data-ttu-id="0c634-107">本文將討論在要求之間保留應用程式和工作階段狀態的不同方式。</span><span class="sxs-lookup"><span data-stu-id="0c634-107">This article discusses different ways to preserve application and session state between requests.</span></span> 

## <a name="session-state"></a><span data-ttu-id="0c634-108">工作階段狀態</span><span class="sxs-lookup"><span data-stu-id="0c634-108">Session state</span></span>

<span data-ttu-id="0c634-109">在 ASP.NET Core 中的工作階段狀態功能，可用於在使用者瀏覽您的 Web 應用程式時，儲存及存放使用者資料。</span><span class="sxs-lookup"><span data-stu-id="0c634-109">Session state is a feature in ASP.NET Core that you can use to save and store user data while the user browses your web app.</span></span> <span data-ttu-id="0c634-110">工作階段狀態由伺服器上的字典或雜湊資料表所組成，可從瀏覽器跨要求保存資料。</span><span class="sxs-lookup"><span data-stu-id="0c634-110">Consisting of a dictionary or hash table on the server, session state persists data across requests from a browser.</span></span> <span data-ttu-id="0c634-111">快取支援工作階段資料。</span><span class="sxs-lookup"><span data-stu-id="0c634-111">The session data is backed by a cache.</span></span>

<span data-ttu-id="0c634-112">ASP.NET Core 可維護工作階段狀態，方法是提供包含工作階段識別碼的 Cookie 給用戶端，以便將其隨著每個要求傳送至伺服器。</span><span class="sxs-lookup"><span data-stu-id="0c634-112">ASP.NET Core maintains session state by giving the client a cookie that contains the session ID, which is sent to the server with each request.</span></span> <span data-ttu-id="0c634-113">伺服器則使用工作階段識別碼來擷取工作階段資料。</span><span class="sxs-lookup"><span data-stu-id="0c634-113">The server uses the session ID to fetch the session data.</span></span> <span data-ttu-id="0c634-114">因為工作階段 Cookie 是瀏覽器所特有，所以您無法跨瀏覽器共用工作階段。</span><span class="sxs-lookup"><span data-stu-id="0c634-114">Because the session cookie is specific to the browser, you cannot share sessions across browsers.</span></span> <span data-ttu-id="0c634-115">只有在瀏覽器工作階段結束時，才會刪除工作階段 Cookie。</span><span class="sxs-lookup"><span data-stu-id="0c634-115">Session cookies are deleted only when the browser session ends.</span></span> <span data-ttu-id="0c634-116">如果收到過期工作階段的 Cookie 時，則會建立使用相同工作階段 Cookie 的新工作階段。</span><span class="sxs-lookup"><span data-stu-id="0c634-116">If a cookie is received for an expired session, a new session that uses the same session cookie  is created.</span></span> 

<span data-ttu-id="0c634-117">伺服器會在最後一個要求之後，保留工作階段一段有限的時間。</span><span class="sxs-lookup"><span data-stu-id="0c634-117">The server retains a session for a limited time after the last request.</span></span> <span data-ttu-id="0c634-118">請設定工作階段逾時或使用預設值 20 分鐘。</span><span class="sxs-lookup"><span data-stu-id="0c634-118">Either set the session timeout or use the default value of 20 minutes.</span></span> <span data-ttu-id="0c634-119">工作階段狀態適合用來儲存特定工作階段特有的使用者資料，但不需要永久保存。</span><span class="sxs-lookup"><span data-stu-id="0c634-119">Session state is ideal for storing user data that's specific to a particular session but doesn't need to be persisted permanently.</span></span> <span data-ttu-id="0c634-120">呼叫 `Session.Clear` 時或資料存放區中的工作階段到期時，就會從支援存放區中刪除資料。</span><span class="sxs-lookup"><span data-stu-id="0c634-120">Data is deleted from the backing store either when calling `Session.Clear` or when the session expires in the data store.</span></span> <span data-ttu-id="0c634-121">伺服器並不知道何時會關閉瀏覽器或刪除工作階段 Cookie。</span><span class="sxs-lookup"><span data-stu-id="0c634-121">The server doesn't know when the browser is closed or when the session cookie is deleted.</span></span>

> [!WARNING]
> <span data-ttu-id="0c634-122">請勿將機密資料存放在工作階段。</span><span class="sxs-lookup"><span data-stu-id="0c634-122">Don't store sensitive data in session.</span></span> <span data-ttu-id="0c634-123">用戶端可能不會關閉瀏覽器並清除工作階段 Cookie (而某些瀏覽器會將工作階段 Cookie 跨 Windows 保持運作)。</span><span class="sxs-lookup"><span data-stu-id="0c634-123">The client might not close the browser and clear the session cookie (and some browsers keep session cookies alive across windows).</span></span> <span data-ttu-id="0c634-124">此外，工作階段可能無法限制為單一使用者；下一位使用者可能會繼續使用相同的工作階段。</span><span class="sxs-lookup"><span data-stu-id="0c634-124">Also, a session might not be restricted to a single user; the next user might continue with the same session.</span></span>

<span data-ttu-id="0c634-125">記憶體內部工作階段提供者會在本機伺服器上儲存工作階段資料。</span><span class="sxs-lookup"><span data-stu-id="0c634-125">The in-memory session provider stores session data on the local server.</span></span> <span data-ttu-id="0c634-126">如果您打算在伺服器陣列上執行 Web 應用程式，則必須使用黏性工作階段將每個工作階段繫結到特定伺服器。</span><span class="sxs-lookup"><span data-stu-id="0c634-126">If you plan to run your web app on a server farm, you must use sticky sessions to tie each session to a specific server.</span></span> <span data-ttu-id="0c634-127">Windows Azure 網站平台預設為黏性工作階段 (應用程式要求路由或 ARR)。</span><span class="sxs-lookup"><span data-stu-id="0c634-127">The Windows Azure Web Sites platform defaults to sticky sessions (Application Request Routing or ARR).</span></span> <span data-ttu-id="0c634-128">不過，黏性工作階段可能會影響延展性，並使 Web 應用程式更新複雜化。</span><span class="sxs-lookup"><span data-stu-id="0c634-128">However, sticky sessions can affect scalability and complicate web app updates.</span></span> <span data-ttu-id="0c634-129">較好的選項是使用 Redis 或 SQL Server 分散式快取，這不需要黏性工作階段。</span><span class="sxs-lookup"><span data-stu-id="0c634-129">A better option is to use the Redis or SQL Server distributed caches, which don't require sticky sessions.</span></span> <span data-ttu-id="0c634-130">如需詳細資訊，請參閱[使用分散式快取](xref:performance/caching/distributed)。</span><span class="sxs-lookup"><span data-stu-id="0c634-130">For more information, see [Working with a Distributed Cache](xref:performance/caching/distributed).</span></span> <span data-ttu-id="0c634-131">如需設定服務提供者的詳細資料，請參閱本文稍後的[設定工作階段](#configuring-session)。</span><span class="sxs-lookup"><span data-stu-id="0c634-131">For details on setting up service providers, see [Configuring Session](#configuring-session) later in this article.</span></span>

<a name="temp"></a>
## <a name="tempdata"></a><span data-ttu-id="0c634-132">TempData</span><span class="sxs-lookup"><span data-stu-id="0c634-132">TempData</span></span>

<span data-ttu-id="0c634-133">ASP.NET Core MVC 公開[控制器](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller?view=aspnetcore-2.0)上的 [TempData](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) 屬性。</span><span class="sxs-lookup"><span data-stu-id="0c634-133">ASP.NET Core MVC exposes the [TempData](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller?view=aspnetcore-2.0).</span></span> <span data-ttu-id="0c634-134">這個屬性會儲存資料，直到讀取為止。</span><span class="sxs-lookup"><span data-stu-id="0c634-134">This property stores data until it's read.</span></span> <span data-ttu-id="0c634-135">`Keep` 和 `Peek` 方法可以用來檢查資料，不用刪除。</span><span class="sxs-lookup"><span data-stu-id="0c634-135">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="0c634-136">當有多個要求需要資料時，`TempData` 對重新導向很有幫助。</span><span class="sxs-lookup"><span data-stu-id="0c634-136">`TempData` is particularly useful for redirection, when data is needed for more than a single request.</span></span> <span data-ttu-id="0c634-137">`TempData` 是 TempData 提供者的實作；例如，使用 Cookie 或工作階段狀態。</span><span class="sxs-lookup"><span data-stu-id="0c634-137">`TempData` is implemented by TempData providers, for example, using either cookies or session state.</span></span>

<a name="tempdata-providers"></a>
### <a name="tempdata-providers"></a><span data-ttu-id="0c634-138">TempData 提供者</span><span class="sxs-lookup"><span data-stu-id="0c634-138">TempData providers</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0c634-139">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0c634-139">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="0c634-140">在 ASP.NET Core 2.0 和更新版本中，預設會使用 Cookie 架構 TempData 提供者，將 TempData 儲存在 Cookie 中。</span><span class="sxs-lookup"><span data-stu-id="0c634-140">In ASP.NET Core 2.0 and later, the cookie-based TempData provider is used by default to store TempData in cookies.</span></span>

<span data-ttu-id="0c634-141">Cookie 資料使用 [Base64UrlTextEncoder](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder?view=aspnetcore-2.0) 加以編碼。</span><span class="sxs-lookup"><span data-stu-id="0c634-141">The cookie data is encoded with the [Base64UrlTextEncoder](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder?view=aspnetcore-2.0).</span></span> <span data-ttu-id="0c634-142">因為 Cookie 會進行加密及區塊處理，所以 ASP.NET Core 1.x 中的單一 Cookie 大小限制並不適用。</span><span class="sxs-lookup"><span data-stu-id="0c634-142">Because the cookie is encrypted and chunked, the single cookie size limit found in ASP.NET Core 1.x does not apply.</span></span> <span data-ttu-id="0c634-143">Cookie 資料不會壓縮，因為壓縮加密資料可能會導致安全性問題，例如 [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) 和 [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) 攻擊。</span><span class="sxs-lookup"><span data-stu-id="0c634-143">The cookie data is not compressed because compressing encrypted data can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span> <span data-ttu-id="0c634-144">如需 Cookie 架構 TempData 提供者的詳細資訊，請參閱 [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs)。</span><span class="sxs-lookup"><span data-stu-id="0c634-144">For more information on the cookie-based TempData provider, see [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0c634-145">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0c634-145">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="0c634-146">在 ASP.NET Core 1.0 和 1.1 中，工作階段狀態 TempData 提供者是預設值。</span><span class="sxs-lookup"><span data-stu-id="0c634-146">In ASP.NET Core 1.0 and 1.1, the session state TempData provider is the default.</span></span>

--------------

<a name="choose-temp"></a>
### <a name="choosing-a-tempdata-provider"></a><span data-ttu-id="0c634-147">選擇 TempData 提供者</span><span class="sxs-lookup"><span data-stu-id="0c634-147">Choosing a TempData provider</span></span>

<span data-ttu-id="0c634-148">選擇 TempData 提供者涉及數項考量，例如：</span><span class="sxs-lookup"><span data-stu-id="0c634-148">Choosing a TempData provider involves several considerations, such as:</span></span>

1. <span data-ttu-id="0c634-149">應用程式是否已將工作階段狀態用於其他用途？</span><span class="sxs-lookup"><span data-stu-id="0c634-149">Does the application already use session state for other purposes?</span></span> <span data-ttu-id="0c634-150">如果是的話，使用工作階段狀態 TempData 提供者沒有額外的應用程式成本 (除了資料的大小之外)。</span><span class="sxs-lookup"><span data-stu-id="0c634-150">If so, using the session state TempData provider has no additional cost to the application (aside from the size of the data).</span></span>
2. <span data-ttu-id="0c634-151">應用程式是否盡量只將 TempData 用於相對少量的資料 (最多 500 個位元組)？</span><span class="sxs-lookup"><span data-stu-id="0c634-151">Does the application use TempData only sparingly, for relatively small amounts of data (up to 500 bytes)?</span></span> <span data-ttu-id="0c634-152">如果是的話，Cookie TempData 提供者將對包含 TempData 的每個要求新增少量成本。</span><span class="sxs-lookup"><span data-stu-id="0c634-152">If so, the cookie TempData provider will add a small cost to each request that carries TempData.</span></span> <span data-ttu-id="0c634-153">如果不是的話，則工作階段狀態 TempData 提供者可能有助於避免在每個要求中來回傳送大量資料，直到取用 TempData 為止。</span><span class="sxs-lookup"><span data-stu-id="0c634-153">If not, the session state TempData provider can be beneficial to avoid round-tripping a large amount of data in each request until the TempData is consumed.</span></span>
3. <span data-ttu-id="0c634-154">應用程式是否在 Web 伺服陣列 (多部伺服器) 中執行？</span><span class="sxs-lookup"><span data-stu-id="0c634-154">Does the application run in a web farm (multiple servers)?</span></span> <span data-ttu-id="0c634-155">如果是的話，使用 Cookie TempData 提供者不需要其他組態。</span><span class="sxs-lookup"><span data-stu-id="0c634-155">If so, there's no additional configuration needed to use the cookie TempData provider.</span></span>

> [!NOTE]
> <span data-ttu-id="0c634-156">大部分的 Web 用戶端 (例如網頁瀏覽器) 會強制執行每個 Cookie 的大小上限、Cookie 總數或這兩者的限制。</span><span class="sxs-lookup"><span data-stu-id="0c634-156">Most web clients (such as web browsers) enforce limits on the maximum size of each cookie, the total number of cookies, or both.</span></span> <span data-ttu-id="0c634-157">因此，使用 Cookie TempData 提供者時，請確認應用程式不會超過這些限制。</span><span class="sxs-lookup"><span data-stu-id="0c634-157">Therefore, when using the cookie TempData provider, verify the app won't exceed these limits.</span></span> <span data-ttu-id="0c634-158">請考慮資料大小總計，並考量加密和區塊處理的額外負荷。</span><span class="sxs-lookup"><span data-stu-id="0c634-158">Consider the total size of the data, accounting for the overheads of encryption and chunking.</span></span>

<a name="config-temp"></a>
### <a name="configure-the-tempdata-provider"></a><span data-ttu-id="0c634-159">設定 TempData 提供者</span><span class="sxs-lookup"><span data-stu-id="0c634-159">Configure the TempData provider</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0c634-160">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0c634-160">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="0c634-161">預設會啟用 Cookie 架構 TempData 提供者。</span><span class="sxs-lookup"><span data-stu-id="0c634-161">The cookie-based TempData provider is enabled by default.</span></span> <span data-ttu-id="0c634-162">下列 `Startup` 類別程式碼會設定工作階段架構 TempData 提供者：</span><span class="sxs-lookup"><span data-stu-id="0c634-162">The following `Startup` class code configures the session-based TempData provider:</span></span>

[!code-csharp[](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,6,11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0c634-163">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0c634-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="0c634-164">下列 `Startup` 類別程式碼會設定工作階段架構 TempData 提供者：</span><span class="sxs-lookup"><span data-stu-id="0c634-164">The following `Startup` class code configures the session-based TempData provider:</span></span>

[!code-csharp[](app-state/sample/src/WebAppSession/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,9)]

---

<span data-ttu-id="0c634-165">順序對中介軟體元件來說很重要。</span><span class="sxs-lookup"><span data-stu-id="0c634-165">Ordering is critical for middleware components.</span></span> <span data-ttu-id="0c634-166">在上述範例中，如果在 `UseMvcWithDefaultRoute` 之後叫用 `UseSession`，則會發生 `InvalidOperationException`　類型的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="0c634-166">In the preceding example, an exception of type `InvalidOperationException` occurs when `UseSession` is invoked after `UseMvcWithDefaultRoute`.</span></span> <span data-ttu-id="0c634-167">如需詳細資料，請參閱[中介軟體順序](xref:fundamentals/middleware/index#ordering)。</span><span class="sxs-lookup"><span data-stu-id="0c634-167">See [Middleware Ordering](xref:fundamentals/middleware/index#ordering) for more detail.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0c634-168">如果目標為 .NET Framework 且使用工作階段架構提供者，請將 [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session) NuGet 套件新增至您的專案。</span><span class="sxs-lookup"><span data-stu-id="0c634-168">If targeting .NET Framework and using the session-based provider, add the [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session) NuGet package to your project.</span></span>

## <a name="query-strings"></a><span data-ttu-id="0c634-169">查詢字串</span><span class="sxs-lookup"><span data-stu-id="0c634-169">Query strings</span></span>

<span data-ttu-id="0c634-170">您可以將數量有限的資料從某個要求傳遞到另一個要求，方法是將其新增至新要求的查詢字串。</span><span class="sxs-lookup"><span data-stu-id="0c634-170">You can pass a limited amount of data from one request to another by adding it to the new request's query string.</span></span> <span data-ttu-id="0c634-171">這對於以持續方式擷取狀態很有用，可讓內嵌狀態的連結透過電子郵件或社交網路共用。</span><span class="sxs-lookup"><span data-stu-id="0c634-171">This is useful for capturing state in a persistent manner that allows links with embedded state to be shared through email or social networks.</span></span> <span data-ttu-id="0c634-172">不過，基於這個原因，您永遠不應該針對敏感性資料使用查詢字串。</span><span class="sxs-lookup"><span data-stu-id="0c634-172">However, for this reason, you should never use query strings for sensitive data.</span></span> <span data-ttu-id="0c634-173">除了輕鬆共用之外，在查詢字串中包括資料還可能會為[跨網站偽造要求 (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) 攻擊創造機會，這些攻擊可能會誘騙使用者在驗證時瀏覽惡意網站。</span><span class="sxs-lookup"><span data-stu-id="0c634-173">In addition to being easily shared, including data in query strings can create opportunities for [Cross-Site Request Forgery (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) attacks, which can trick users into visiting malicious sites while authenticated.</span></span> <span data-ttu-id="0c634-174">然後，攻擊者可能會竊取應用程式中的使用者資料，或代表使用者採取惡意動作。</span><span class="sxs-lookup"><span data-stu-id="0c634-174">Attackers can then steal user data from your app or take malicious actions on behalf of the user.</span></span> <span data-ttu-id="0c634-175">任何保留的應用程式或工作階段狀態必須防範 CSRF 攻擊。</span><span class="sxs-lookup"><span data-stu-id="0c634-175">Any preserved application or session state must protect against CSRF attacks.</span></span> <span data-ttu-id="0c634-176">如需詳細資訊，請參閱[防止 ASP.NET Core 中的跨網站偽造要求 (XSRF/CSRF) 攻擊](../security/anti-request-forgery.md)。</span><span class="sxs-lookup"><span data-stu-id="0c634-176">For more information on CSRF attacks, see [Preventing Cross-Site Request Forgery (XSRF/CSRF) Attacks in ASP.NET Core](../security/anti-request-forgery.md).</span></span>

## <a name="post-data-and-hidden-fields"></a><span data-ttu-id="0c634-177">張貼資料和隱藏欄位</span><span class="sxs-lookup"><span data-stu-id="0c634-177">Post data and hidden fields</span></span>

<span data-ttu-id="0c634-178">資料可以儲存在隱藏的表單欄位，並貼回下一個要求。</span><span class="sxs-lookup"><span data-stu-id="0c634-178">Data can be saved in hidden form fields and posted back on the next request.</span></span> <span data-ttu-id="0c634-179">這在多頁表單中很常見。</span><span class="sxs-lookup"><span data-stu-id="0c634-179">This is common in multi-page forms.</span></span> <span data-ttu-id="0c634-180">不過，因為用戶端可能會竄改資料，所以伺服器必須一律重新驗證它。</span><span class="sxs-lookup"><span data-stu-id="0c634-180">However, because the client can potentially tamper with the data, the server must always revalidate it.</span></span> 

## <a name="cookies"></a><span data-ttu-id="0c634-181">Cookie</span><span class="sxs-lookup"><span data-stu-id="0c634-181">Cookies</span></span>

<span data-ttu-id="0c634-182">Cookie 提供在 Web 應用程式中儲存使用者特定資料的方法。</span><span class="sxs-lookup"><span data-stu-id="0c634-182">Cookies provide a way to store user-specific data in web applications.</span></span> <span data-ttu-id="0c634-183">因為 Cookie 會隨著每個要求傳送，所以其大小應該保持最小。</span><span class="sxs-lookup"><span data-stu-id="0c634-183">Because cookies are sent with every request, their size should be kept to a minimum.</span></span> <span data-ttu-id="0c634-184">在理想情況下，應該只有識別碼儲存在 Cookie 中，而實際資料儲存在伺服器上。</span><span class="sxs-lookup"><span data-stu-id="0c634-184">Ideally, only an identifier should be stored in a cookie with the actual data stored on the server.</span></span> <span data-ttu-id="0c634-185">大部分的瀏覽器將 Cookie 限制為 4096 個位元組。</span><span class="sxs-lookup"><span data-stu-id="0c634-185">Most browsers restrict cookies to 4096 bytes.</span></span> <span data-ttu-id="0c634-186">此外，每個網域只有數量有限的 Cookie 可供使用。</span><span class="sxs-lookup"><span data-stu-id="0c634-186">In addition, only a limited number of cookies are available for each domain.</span></span>  

<span data-ttu-id="0c634-187">由於 Cookie 可能會遭到竄改，因此必須在伺服器上加以驗證。</span><span class="sxs-lookup"><span data-stu-id="0c634-187">Because cookies are subject to tampering, they must be validated on the server.</span></span> <span data-ttu-id="0c634-188">雖然用戶端上的 Cookie 持久性受限於使用者介入與到期日，但它們通常是用戶端上最持久的資料持續性形式。</span><span class="sxs-lookup"><span data-stu-id="0c634-188">Although the durability of the cookie on a client is subject to user intervention and expiration, they're generally the most durable form of data persistence on the client.</span></span>

<span data-ttu-id="0c634-189">Cookie 通常可用於個人化，其中內容會針對已知的使用者自訂。</span><span class="sxs-lookup"><span data-stu-id="0c634-189">Cookies are often used for personalization, where content is customized for a known user.</span></span> <span data-ttu-id="0c634-190">因為在大部分情況下，只會識別使用者而不會驗證，所以您通常可以藉由在 Cookie 中儲存使用者名稱、帳戶名稱或唯一使用者識別碼 (例如 GUID) 來保護 Cookie。</span><span class="sxs-lookup"><span data-stu-id="0c634-190">Because the user is only identified and not authenticated in most cases, you can typically secure a cookie by storing the user name, account name, or a unique user ID (such as a GUID) in the cookie.</span></span> <span data-ttu-id="0c634-191">然後，該Cookie 可用來存取網站的使用者個人化基礎結構。</span><span class="sxs-lookup"><span data-stu-id="0c634-191">You can then use the cookie to access the user personalization infrastructure of a site.</span></span>

## <a name="httpcontextitems"></a><span data-ttu-id="0c634-192">HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="0c634-192">HttpContext.Items</span></span>

<span data-ttu-id="0c634-193">要儲存只有處理某個特定要求時所需的資料，`Items` 集合是個不錯的位置。</span><span class="sxs-lookup"><span data-stu-id="0c634-193">The `Items` collection is a good location to store data that's needed only while processing one particular request.</span></span> <span data-ttu-id="0c634-194">集合的內容會在每個要求之後捨棄。</span><span class="sxs-lookup"><span data-stu-id="0c634-194">The collection's contents are discarded after each request.</span></span> <span data-ttu-id="0c634-195">當元件或中介軟體在要求期間的不同時間點運作，而且沒有可傳遞參數的直接方式時，`Items` 集合是元件或中介軟體用來通訊的最佳方式。</span><span class="sxs-lookup"><span data-stu-id="0c634-195">The `Items` collection is best used as a way for components or middleware to communicate when they operate at different points in time during a request and have no direct way to pass parameters.</span></span> <span data-ttu-id="0c634-196">如需詳細資訊，請參閱本文稍後的[使用 HttpContext.Items](#working-with-httpcontextitems)。</span><span class="sxs-lookup"><span data-stu-id="0c634-196">For more information, see [Working with HttpContext.Items](#working-with-httpcontextitems), later in this article.</span></span>

## <a name="cache"></a><span data-ttu-id="0c634-197">快取</span><span class="sxs-lookup"><span data-stu-id="0c634-197">Cache</span></span>

<span data-ttu-id="0c634-198">快取是儲存和擷取資料的有效方式。</span><span class="sxs-lookup"><span data-stu-id="0c634-198">Caching is an efficient way to store and retrieve data.</span></span> <span data-ttu-id="0c634-199">您可以依據時間和其他考量控制快取項目的存留期。</span><span class="sxs-lookup"><span data-stu-id="0c634-199">You can control the lifetime of cached items based on time and other considerations.</span></span> <span data-ttu-id="0c634-200">請深入了解[快取](../performance/caching/index.md)。</span><span class="sxs-lookup"><span data-stu-id="0c634-200">Learn more about [Caching](../performance/caching/index.md).</span></span>

<a name="session"></a>
## <a name="working-with-session-state"></a><span data-ttu-id="0c634-201">使用工作階段狀態</span><span class="sxs-lookup"><span data-stu-id="0c634-201">Working with Session State</span></span>

### <a name="configuring-session"></a><span data-ttu-id="0c634-202">設定工作階段</span><span class="sxs-lookup"><span data-stu-id="0c634-202">Configuring Session</span></span>

<span data-ttu-id="0c634-203">`Microsoft.AspNetCore.Session` 套件提供用來管理工作階段狀態的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="0c634-203">The `Microsoft.AspNetCore.Session` package provides middleware for managing session state.</span></span> <span data-ttu-id="0c634-204">若要啟用工作階段中介軟體，`Startup` 必須包含：</span><span class="sxs-lookup"><span data-stu-id="0c634-204">To enable the session middleware, `Startup` must contain:</span></span>

- <span data-ttu-id="0c634-205">任一 [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) 記憶體快取。</span><span class="sxs-lookup"><span data-stu-id="0c634-205">Any of the [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) memory caches.</span></span> <span data-ttu-id="0c634-206">`IDistributedCache` 實作會作為工作階段的支援存放區。</span><span class="sxs-lookup"><span data-stu-id="0c634-206">The `IDistributedCache` implementation is used as a backing store for session.</span></span>
- <span data-ttu-id="0c634-207">[AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_) 呼叫，這需要 NuGet 套件 "Microsoft.AspNetCore.Session"。</span><span class="sxs-lookup"><span data-stu-id="0c634-207">[AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_) call, which requires NuGet package "Microsoft.AspNetCore.Session".</span></span>
- <span data-ttu-id="0c634-208">[UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) 呼叫。</span><span class="sxs-lookup"><span data-stu-id="0c634-208">[UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) call.</span></span>

<span data-ttu-id="0c634-209">下列程式碼示範如何設定記憶體內部工作階段提供者。</span><span class="sxs-lookup"><span data-stu-id="0c634-209">The following code shows how to set up the in-memory session provider.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0c634-210">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0c634-210">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](app-state/sample/src/WebAppSessionDotNetCore2.0App/Startup.cs?highlight=11-19,24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0c634-211">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0c634-211">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](app-state/sample/src/WebAppSession/Startup.cs?highlight=11-19,24)]

---

<span data-ttu-id="0c634-212">在其安裝並設定之後，您就可以從 `HttpContext` 參考工作階段。</span><span class="sxs-lookup"><span data-stu-id="0c634-212">You can reference Session from `HttpContext` once it's installed and configured.</span></span>

<span data-ttu-id="0c634-213">如果在已呼叫 `UseSession` 之前嘗試存取 `Session`，就會擲回 `InvalidOperationException: Session has not been configured for this application or request` 例外狀況。</span><span class="sxs-lookup"><span data-stu-id="0c634-213">If you try to access `Session` before `UseSession` has been called, the exception `InvalidOperationException: Session has not been configured for this application or request` is thrown.</span></span>

<span data-ttu-id="0c634-214">如果在已經開始寫入 `Response` 資料流之後，嘗試建立新的 `Session` (也就是未建立任何工作階段 Cookie)，則會擲回 `InvalidOperationException: The session cannot be established after the response has started` 例外狀況。</span><span class="sxs-lookup"><span data-stu-id="0c634-214">If you try to create a new `Session` (that is, no session cookie has been created) after you have already begun writing to the `Response` stream, the exception `InvalidOperationException: The session cannot be established after the response has started` is thrown.</span></span> <span data-ttu-id="0c634-215">此例外狀況可以在網頁伺服器記錄檔中找到；它不會顯示在瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="0c634-215">The exception can be found in the web server log; it will not be displayed in the browser.</span></span>

### <a name="loading-session-asynchronously"></a><span data-ttu-id="0c634-216">非同步載入工作階段</span><span class="sxs-lookup"><span data-stu-id="0c634-216">Loading Session asynchronously</span></span> 

<span data-ttu-id="0c634-217">只有在 `TryGetValue`、`Set` 或 `Remove` 方法之前明確呼叫 [ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) 方法時，ASP.NET Core 中的預設工作階段提供者才會從基礎 [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) 存放區以非同步方式載入工作階段記錄。</span><span class="sxs-lookup"><span data-stu-id="0c634-217">The default session provider in ASP.NET Core loads the session record from the underlying [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) store asynchronously only if the [ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) method is explicitly called before  the `TryGetValue`, `Set`, or `Remove` methods.</span></span> <span data-ttu-id="0c634-218">如果並未先呼叫 `LoadAsync`，則基礎工作階段記錄會同步載入，這可能會影響應用程式的擴展能力。</span><span class="sxs-lookup"><span data-stu-id="0c634-218">If `LoadAsync` isn't called first, the underlying session record is loaded synchronously, which could potentially impact the ability of the app to scale.</span></span>

<span data-ttu-id="0c634-219">若要讓應用程式強制執行此模式，請使用未在 `TryGetValue`、`Set` 或 `Remove` 之前呼叫 `LoadAsync` 方法時擲回例外狀況的版本來包裝 [DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore) 和 [DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession) 實作。</span><span class="sxs-lookup"><span data-stu-id="0c634-219">To have applications enforce this pattern, wrap the [DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore) and [DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession) implementations with versions that throw an exception if the `LoadAsync` method isn't called before `TryGetValue`, `Set`, or `Remove`.</span></span> <span data-ttu-id="0c634-220">請在服務容器中註冊已包裝的版本。</span><span class="sxs-lookup"><span data-stu-id="0c634-220">Register the wrapped versions in the services container.</span></span>

### <a name="implementation-details"></a><span data-ttu-id="0c634-221">實作詳細資料</span><span class="sxs-lookup"><span data-stu-id="0c634-221">Implementation details</span></span>

<span data-ttu-id="0c634-222">工作階段使用 Cookie 來追蹤和識別來自單一瀏覽器的要求。</span><span class="sxs-lookup"><span data-stu-id="0c634-222">Session uses a cookie to track and identify requests from a single browser.</span></span> <span data-ttu-id="0c634-223">此 Cookie 預設名為 ".AspNet.Session"，並使用路徑 "/"。</span><span class="sxs-lookup"><span data-stu-id="0c634-223">By default, this cookie is named ".AspNet.Session", and it uses a path of "/".</span></span> <span data-ttu-id="0c634-224">由於 Cookie 預設值未指定網域，因此它不會提供給頁面上的用戶端指令碼 (因為 `CookieHttpOnly` 預設為 `true`)。</span><span class="sxs-lookup"><span data-stu-id="0c634-224">Because the cookie default doesn't specify a domain, it's not made available to the client-side script on the page (because `CookieHttpOnly` defaults to `true`).</span></span>

<span data-ttu-id="0c634-225">若要覆寫工作階段的預設值，請使用 `SessionOptions`：</span><span class="sxs-lookup"><span data-stu-id="0c634-225">To override session defaults, use `SessionOptions`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0c634-226">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0c634-226">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupCopy.cs?name=snippet1&highlight=8-12)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0c634-227">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0c634-227">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](app-state/sample/src/WebAppSession/StartupCopy.cs?name=snippet1&highlight=8-12)]

---

<span data-ttu-id="0c634-228">伺服器會使用 `IdleTimeout` 屬性，判斷工作階段可以在放棄其內容之前閒置多長時間。</span><span class="sxs-lookup"><span data-stu-id="0c634-228">The server uses the `IdleTimeout` property to determine how long a session can be idle before its contents are abandoned.</span></span> <span data-ttu-id="0c634-229">這個屬性與 Cookie 到期日無關。</span><span class="sxs-lookup"><span data-stu-id="0c634-229">This property is independent of the cookie expiration.</span></span> <span data-ttu-id="0c634-230">透過工作階段中介軟體傳遞 (讀取或寫入) 的每個要求會重設逾時。</span><span class="sxs-lookup"><span data-stu-id="0c634-230">Each request that passes through the Session middleware (read from or written to) resets the timeout.</span></span>

<span data-ttu-id="0c634-231">因為 `Session` 為「非鎖定」，如果兩個要求同時嘗試修改工作階段的內容，則最後一個內容會覆寫第一個內容。</span><span class="sxs-lookup"><span data-stu-id="0c634-231">Because `Session` is *non-locking*, if two requests both attempt to modify the contents of session, the last one overrides the first.</span></span> <span data-ttu-id="0c634-232">`Session` 會實作為「一致性工作階段」，這表示所有內容會都儲存在一起。</span><span class="sxs-lookup"><span data-stu-id="0c634-232">`Session` is implemented as a *coherent session*, which means that all the contents are stored together.</span></span> <span data-ttu-id="0c634-233">要修改工作階段不同部分 (不同的索引鍵) 的兩個要求仍可能會彼此影響。</span><span class="sxs-lookup"><span data-stu-id="0c634-233">Two requests that are modifying different parts of the session (different keys) might still impact each other.</span></span>

### <a name="setting-and-getting-session-values"></a><span data-ttu-id="0c634-234">設定和取得工作階段值</span><span class="sxs-lookup"><span data-stu-id="0c634-234">Setting and getting Session values</span></span>

<span data-ttu-id="0c634-235">工作階段可透過 `HttpContext` 上的 `Session` 屬性來存取。</span><span class="sxs-lookup"><span data-stu-id="0c634-235">Session is accessed through the `Session` property on `HttpContext`.</span></span> <span data-ttu-id="0c634-236">這個屬性是 [ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession) 實作。</span><span class="sxs-lookup"><span data-stu-id="0c634-236">This property is an [ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession) implementation.</span></span>

<span data-ttu-id="0c634-237">下列範例示範如何設定和取得整數及字串：</span><span class="sxs-lookup"><span data-stu-id="0c634-237">The following example shows setting and getting an int and a string:</span></span>

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?range=8-27,49)]

<span data-ttu-id="0c634-238">如果新增下列擴充方法，您可以設定和取得工作階段的可序列化物件：</span><span class="sxs-lookup"><span data-stu-id="0c634-238">If you add the following extension methods, you can set and get serializable objects to Session:</span></span>

[!code-csharp[Main](app-state/sample/src/WebAppSession/Extensions/SessionExtensions.cs)]

<span data-ttu-id="0c634-239">下列範例示範如何設定和取得可序列化物件：</span><span class="sxs-lookup"><span data-stu-id="0c634-239">The following sample shows how to set and get a serializable object:</span></span>

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet2)]


## <a name="working-with-httpcontextitems"></a><span data-ttu-id="0c634-240">使用 HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="0c634-240">Working with HttpContext.Items</span></span>

<span data-ttu-id="0c634-241">`HttpContext` 抽象支援 `IDictionary<object, object>` 類型的字典集合，稱為 `Items`。</span><span class="sxs-lookup"><span data-stu-id="0c634-241">The `HttpContext` abstraction provides support for a dictionary collection of type `IDictionary<object, object>`, called `Items`.</span></span> <span data-ttu-id="0c634-242">此集合可在 *HttpRequest* 的開頭取得，並於每個要求的結尾處捨棄。</span><span class="sxs-lookup"><span data-stu-id="0c634-242">This collection is available from the start of an *HttpRequest* and is discarded at the end of each request.</span></span> <span data-ttu-id="0c634-243">透過將值指派給索引鍵項目，或要求特定索引鍵的值，即可存取它。</span><span class="sxs-lookup"><span data-stu-id="0c634-243">You can access it by  assigning a value to a keyed entry, or by requesting the value for a particular key.</span></span>

<span data-ttu-id="0c634-244">在下列範例中，[中介軟體](xref:fundamentals/middleware/index)會將 `isVerified` 新增至 `Items` 集合。</span><span class="sxs-lookup"><span data-stu-id="0c634-244">In the following sample, [Middleware](xref:fundamentals/middleware/index) adds `isVerified` to the `Items` collection.</span></span>

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

<span data-ttu-id="0c634-245">稍後在管線中，另一個中介軟體可以存取它：</span><span class="sxs-lookup"><span data-stu-id="0c634-245">Later in the pipeline, another middleware could access it:</span></span>

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync("Verified request? " + 
        context.Items["isVerified"]);
});
```

<span data-ttu-id="0c634-246">如果是只由單一應用程式使用的中介軟體，可接受 `string` 索引鍵。</span><span class="sxs-lookup"><span data-stu-id="0c634-246">For middleware that will only be used by a single app, `string` keys are acceptable.</span></span> <span data-ttu-id="0c634-247">不過，將在應用程式之間共用的中介軟體應該使用唯一物件索引鍵，以避免任何可能發生的索引鍵衝突。</span><span class="sxs-lookup"><span data-stu-id="0c634-247">However, middleware that will be shared between applications should use unique object keys to avoid any chance of key collisions.</span></span> <span data-ttu-id="0c634-248">如果您要開發必須跨多個應用程式運作的中介軟體，請使用中介軟體類別中定義的唯一物件索引鍵，如下所示：</span><span class="sxs-lookup"><span data-stu-id="0c634-248">If you are developing middleware that must work across multiple applications, use a unique object key defined in your middleware class as shown below:</span></span>

```csharp
public class SampleMiddleware
{
    public static readonly object SampleKey = new Object();

    public async Task Invoke(HttpContext httpContext)
    {
        httpContext.Items[SampleKey] = "some value";
        // additional code omitted
    }
}
```

<span data-ttu-id="0c634-249">其他程式碼可以使用中介軟體類別所公開的索引鍵，來存取 `HttpContext.Items` 中儲存的值：</span><span class="sxs-lookup"><span data-stu-id="0c634-249">Other code can access the value stored in `HttpContext.Items` using the key exposed by the middleware class:</span></span>

```csharp
public class HomeController : Controller
{
    public IActionResult Index()
    {
        string value = HttpContext.Items[SampleMiddleware.SampleKey];
    }
}
```

<span data-ttu-id="0c634-250">此方法也有在程式碼的多個位置中消除重複的魔術字串 (magic string) 的優點。</span><span class="sxs-lookup"><span data-stu-id="0c634-250">This approach also has the advantage of eliminating repetition of "magic strings" in multiple places in the code.</span></span>

<a name="appstate-errors"></a>

## <a name="application-state-data"></a><span data-ttu-id="0c634-251">應用程式狀態資料</span><span class="sxs-lookup"><span data-stu-id="0c634-251">Application state data</span></span>

<span data-ttu-id="0c634-252">請使用[相依性插入](xref:fundamentals/dependency-injection)將資料提供給所有使用者：</span><span class="sxs-lookup"><span data-stu-id="0c634-252">Use [Dependency Injection](xref:fundamentals/dependency-injection) to make data available to all users:</span></span>

1. <span data-ttu-id="0c634-253">定義包含資料的服務 (例如，名為 `MyAppData` 的類別)。</span><span class="sxs-lookup"><span data-stu-id="0c634-253">Define a service containing the data (for example, a class named `MyAppData`).</span></span>

```csharp
public class MyAppData
{
    // Declare properties/methods/etc.
} 
```
2. <span data-ttu-id="0c634-254">將服務類別新增至 `ConfigureServices` (例如 `services.AddSingleton<MyAppData>();`)。</span><span class="sxs-lookup"><span data-stu-id="0c634-254">Add the service class to `ConfigureServices` (for example `services.AddSingleton<MyAppData>();`).</span></span>
3. <span data-ttu-id="0c634-255">在每個控制器中取用資料服務類別：</span><span class="sxs-lookup"><span data-stu-id="0c634-255">Consume the data service class in each controller:</span></span>

```csharp
public class MyController : Controller
{
    public MyController(MyAppData myService)
    {
        // Do something with the service (read some data from it, 
        // store it in a private field/property, etc.)
    }
} 
```

## <a name="common-errors-when-working-with-session"></a><span data-ttu-id="0c634-256">使用工作階段時的常見錯誤</span><span class="sxs-lookup"><span data-stu-id="0c634-256">Common errors when working with session</span></span>

* <span data-ttu-id="0c634-257">「嘗試啟動 'Microsoft.AspNetCore.Session.DistributedSessionStore' 時，無法解析類型 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' 。」</span><span class="sxs-lookup"><span data-stu-id="0c634-257">"Unable to resolve service for type 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' while attempting to activate 'Microsoft.AspNetCore.Session.DistributedSessionStore'."</span></span>

  <span data-ttu-id="0c634-258">這種情形通常是因為無法設定至少一個 `IDistributedCache` 實作。</span><span class="sxs-lookup"><span data-stu-id="0c634-258">This is usually caused by failing to configure at least one `IDistributedCache` implementation.</span></span> <span data-ttu-id="0c634-259">如需詳細資訊，請參閱[使用分散式快取](xref:performance/caching/distributed)和[記憶體內部快取](xref:performance/caching/memory)。</span><span class="sxs-lookup"><span data-stu-id="0c634-259">For more information, see [Working with a Distributed Cache](xref:performance/caching/distributed) and [In memory caching](xref:performance/caching/memory).</span></span>

* <span data-ttu-id="0c634-260">如果工作階段中介軟體無法保存工作階段 (例如：如果資料庫無法使用)，它會記錄例外狀況並抑制它。</span><span class="sxs-lookup"><span data-stu-id="0c634-260">In the event that the session middleware fails to persist a session (for example: if the database isn't available), it logs the exception and swallows it.</span></span> <span data-ttu-id="0c634-261">然後要求將繼續正常運作，這會導致完全無法預期的行為。</span><span class="sxs-lookup"><span data-stu-id="0c634-261">The request will then continue normally, which leads to very unpredictable behavior.</span></span>

<span data-ttu-id="0c634-262">典型的範例：</span><span class="sxs-lookup"><span data-stu-id="0c634-262">A typical example:</span></span>

<span data-ttu-id="0c634-263">有人會在工作階段中儲存購物籃。</span><span class="sxs-lookup"><span data-stu-id="0c634-263">Someone stores a shopping basket in session.</span></span> <span data-ttu-id="0c634-264">使用者新增一個項目，但認可失敗。</span><span class="sxs-lookup"><span data-stu-id="0c634-264">The user adds an item but the commit fails.</span></span> <span data-ttu-id="0c634-265">應用程式未察覺到失敗，因此它會報告「已新增項目」訊息，但這並不正確。</span><span class="sxs-lookup"><span data-stu-id="0c634-265">The app doesn't know about the failure so it reports the message "The item has been added", which isn't true.</span></span>

<span data-ttu-id="0c634-266">檢查是否有這類錯誤的建議方式是，當您完成寫入工作階段後，從應用程式程式碼呼叫 `await feature.Session.CommitAsync();`。</span><span class="sxs-lookup"><span data-stu-id="0c634-266">The recommended way to check for such errors is to call `await feature.Session.CommitAsync();` from app code when you're done writing to the session.</span></span> <span data-ttu-id="0c634-267">然後，您就可以對錯誤執行想要的操作。</span><span class="sxs-lookup"><span data-stu-id="0c634-267">Then you can do what you like with the error.</span></span> <span data-ttu-id="0c634-268">這與呼叫 `LoadAsync` 時的運作方式相同。</span><span class="sxs-lookup"><span data-stu-id="0c634-268">It works the same way when calling `LoadAsync`.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="0c634-269">其他資源</span><span class="sxs-lookup"><span data-stu-id="0c634-269">Additional resources</span></span>

* [<span data-ttu-id="0c634-270">ASP.NET Core 1.x：本文件中使用的程式碼範例</span><span class="sxs-lookup"><span data-stu-id="0c634-270">ASP.NET Core 1.x: Sample code used in this document</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSession)
* [<span data-ttu-id="0c634-271">ASP.NET Core 2.x：本文件中使用的程式碼範例</span><span class="sxs-lookup"><span data-stu-id="0c634-271">ASP.NET Core 2.x: Sample code used in this document</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSessionDotNetCore2.0App)
