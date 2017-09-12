---
title: "在 ASP.NET Core 的工作階段和應用程式狀態"
author: rick-anderson
description: "在要求之間的方法來保留應用程式和使用者 （工作階段） 狀態。"
keywords: "ASP.NET Core 應用程式狀態、 工作階段狀態、 查詢字串，張貼"
ms.author: riande
manager: wpickett
ms.date: 06/08/2017
ms.topic: article
ms.assetid: 18cda488-0769-4cb9-82f6-4c6685f2045d
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/app-state
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8b451bde1e3180d12781d55113638cc1a99182c8
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/12/2017
---
# <a name="introduction-to-session-and-application-state-in-aspnet-core"></a><span data-ttu-id="346f9-104">工作階段和應用程式的狀態，在 ASP.NET Core 簡介</span><span class="sxs-lookup"><span data-stu-id="346f9-104">Introduction to session and application state in ASP.NET Core</span></span>

<span data-ttu-id="346f9-105">由[Rick Anderson](https://twitter.com/RickAndMSFT)， [Steve Smith](https://ardalis.com/)，和[Diana LaRose](https://github.com/DianaLaRose)</span><span class="sxs-lookup"><span data-stu-id="346f9-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), and [Diana LaRose](https://github.com/DianaLaRose)</span></span>

<span data-ttu-id="346f9-106">HTTP 是無狀態的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="346f9-106">HTTP is a stateless protocol.</span></span> <span data-ttu-id="346f9-107">Web 伺服器視為獨立的要求中的每個 HTTP 要求，並不會保留來自前一個要求的使用者值。</span><span class="sxs-lookup"><span data-stu-id="346f9-107">A  web server treats each HTTP request as an independent request and does not retain user values from previous requests.</span></span> <span data-ttu-id="346f9-108">本文將討論不同的方式來保留應用程式和要求之間的工作階段狀態。</span><span class="sxs-lookup"><span data-stu-id="346f9-108">This article discusses different ways to preserve application and session state between requests.</span></span> 

## <a name="session-state"></a><span data-ttu-id="346f9-109">工作階段狀態</span><span class="sxs-lookup"><span data-stu-id="346f9-109">Session state</span></span>

<span data-ttu-id="346f9-110">工作階段狀態會是可用來儲存和還原使用者資料，而使用者瀏覽您的 web 應用程式的 ASP.NET 核心功能。</span><span class="sxs-lookup"><span data-stu-id="346f9-110">Session state is a feature in ASP.NET Core that you can use to save and store user data while the user browses your web app.</span></span> <span data-ttu-id="346f9-111">伺服器上的字典或雜湊資料表所組成，工作階段狀態資料保存跨要求從瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="346f9-111">Consisting of a dictionary or hash table on the server, session state persists data across requests from a browser.</span></span> <span data-ttu-id="346f9-112">快取所支援的工作階段資料。</span><span class="sxs-lookup"><span data-stu-id="346f9-112">The session data is backed by a cache.</span></span>

<span data-ttu-id="346f9-113">ASP.NET Core 會讓用戶端的 cookie，包含工作階段識別碼，以便傳送至每個要求的伺服器維護工作階段狀態。</span><span class="sxs-lookup"><span data-stu-id="346f9-113">ASP.NET Core maintains session state by giving the client a cookie that contains the session ID, which is sent to the server with each request.</span></span> <span data-ttu-id="346f9-114">伺服器會使用工作階段識別碼來擷取工作階段資料。</span><span class="sxs-lookup"><span data-stu-id="346f9-114">The server uses the session ID to fetch the session data.</span></span> <span data-ttu-id="346f9-115">因為工作階段 cookie 是瀏覽器專用的所以您無法跨瀏覽器共用工作階段。</span><span class="sxs-lookup"><span data-stu-id="346f9-115">Because the session cookie is specific to the browser, you cannot share sessions across browsers.</span></span> <span data-ttu-id="346f9-116">只有在瀏覽器工作階段結束時，會刪除工作階段 cookie。</span><span class="sxs-lookup"><span data-stu-id="346f9-116">Session cookies are deleted only when the browser session ends.</span></span> <span data-ttu-id="346f9-117">如果 cookie 已過期的工作階段收到時，會建立新的工作階段使用相同的工作階段 cookie。</span><span class="sxs-lookup"><span data-stu-id="346f9-117">If a cookie is received for an expired session, a new session that uses the same session cookie  is created.</span></span> 

<span data-ttu-id="346f9-118">伺服器會保留有限的時間，最後一個要求之後的工作階段。</span><span class="sxs-lookup"><span data-stu-id="346f9-118">The server retains a session for a limited time after the last request.</span></span> <span data-ttu-id="346f9-119">您可以設定工作階段逾時或使用預設值為 20 分鐘。</span><span class="sxs-lookup"><span data-stu-id="346f9-119">You can either set the session timeout or use the default value of 20 minutes.</span></span> <span data-ttu-id="346f9-120">工作階段狀態是適合用來儲存使用者資料屬於特定的工作階段，但不需要永久保存。</span><span class="sxs-lookup"><span data-stu-id="346f9-120">Session state is ideal for storing user data that is specific to a particular session but doesn’t need to be persisted permanently.</span></span> <span data-ttu-id="346f9-121">當您呼叫，會有資料從備份存放區是刪除`Session.Clear`或資料存放區中的工作階段到期時。</span><span class="sxs-lookup"><span data-stu-id="346f9-121">Data is deleted from the backing store either when you call `Session.Clear` or when the session expires in the data store.</span></span> <span data-ttu-id="346f9-122">關閉瀏覽器時，或刪除工作階段 cookie 時，伺服器不知道。</span><span class="sxs-lookup"><span data-stu-id="346f9-122">The server does not know when the browser is closed or when the session cookie is deleted.</span></span>

> [!WARNING]
> <span data-ttu-id="346f9-123">不要儲存工作階段中的機密資料。</span><span class="sxs-lookup"><span data-stu-id="346f9-123">Do not store sensitive data in session.</span></span> <span data-ttu-id="346f9-124">用戶端不可能在關閉瀏覽器，並清除工作階段 cookie （和某些瀏覽器工作階段 cookie 存留在整個保留 windows）。</span><span class="sxs-lookup"><span data-stu-id="346f9-124">The client might not close the browser and clear the session cookie (and some browsers keep session cookies alive across windows).</span></span> <span data-ttu-id="346f9-125">此外，工作階段可能無法限制為單一使用者。下一位使用者可能會繼續使用相同的工作階段。</span><span class="sxs-lookup"><span data-stu-id="346f9-125">Also, a session might not be restricted to a single user; the next user might continue with the same session.</span></span>

<span data-ttu-id="346f9-126">記憶體中工作階段提供者會在本機伺服器儲存工作階段資料。</span><span class="sxs-lookup"><span data-stu-id="346f9-126">The in-memory session provider stores session data on the local server.</span></span> <span data-ttu-id="346f9-127">如果您打算在伺服器陣列上執行您 web 應用程式，您必須將繫結到特定伺服器的每個工作階段使用自黏工作階段。</span><span class="sxs-lookup"><span data-stu-id="346f9-127">If you plan to run your web app on a server farm, you must use sticky sessions to tie each session to a specific server.</span></span> <span data-ttu-id="346f9-128">Windows Azure Web Sites 平台預設值是自黏工作階段 （應用程式要求路由或 ARR）。</span><span class="sxs-lookup"><span data-stu-id="346f9-128">The Windows Azure Web Sites platform defaults to sticky sessions (Application Request Routing or ARR).</span></span> <span data-ttu-id="346f9-129">不過，自黏工作階段可能會影響延展性，而且使得 web 應用程式更新。</span><span class="sxs-lookup"><span data-stu-id="346f9-129">However, sticky sessions can affect scalability and complicate web app updates.</span></span> <span data-ttu-id="346f9-130">更好的選項是使用 Redis 或 SQL Server 分散式快取，而這不需要黏性工作階段。</span><span class="sxs-lookup"><span data-stu-id="346f9-130">A better option is to use the Redis or SQL Server distributed caches, which don't require sticky sessions.</span></span> <span data-ttu-id="346f9-131">如需詳細資訊，請參閱[使用分散式快取](xref:performance/caching/distributed)。</span><span class="sxs-lookup"><span data-stu-id="346f9-131">For more information, see [Working with a Distributed Cache](xref:performance/caching/distributed).</span></span> <span data-ttu-id="346f9-132">如需設定服務提供者的詳細資訊，請參閱[設定工作階段](#configuring-session)本文稍後。</span><span class="sxs-lookup"><span data-stu-id="346f9-132">For details on setting up service providers, see [Configuring Session](#configuring-session) later in this article.</span></span>

<span data-ttu-id="346f9-133">本章節的其餘部分描述來儲存使用者資料的選項。</span><span class="sxs-lookup"><span data-stu-id="346f9-133">The remainder of this section describes the options for storing user data.</span></span>

<a name="temp"></a>
### <a name="tempdata"></a><span data-ttu-id="346f9-134">TempData</span><span class="sxs-lookup"><span data-stu-id="346f9-134">TempData</span></span>

<span data-ttu-id="346f9-135">ASP.NET Core MVC 公開[TempData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller#Microsoft_AspNetCore_Mvc_Controller_TempData)屬性[控制器](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller)。</span><span class="sxs-lookup"><span data-stu-id="346f9-135">ASP.NET Core MVC exposes the [TempData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="346f9-136">這個屬性會儲存資料，直到讀取為止。</span><span class="sxs-lookup"><span data-stu-id="346f9-136">This property stores data until it is read.</span></span> <span data-ttu-id="346f9-137">`Keep` 和 `Peek` 方法可以用來檢查資料，不用刪除。</span><span class="sxs-lookup"><span data-stu-id="346f9-137">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="346f9-138">`TempData`就特別有用的重新導向，當超過單一要求所需的資料。</span><span class="sxs-lookup"><span data-stu-id="346f9-138">`TempData` is particularly useful for redirection, when data is needed for more than a single request.</span></span> <span data-ttu-id="346f9-139">`TempData`建置工作階段狀態的頂端。</span><span class="sxs-lookup"><span data-stu-id="346f9-139">`TempData` is built on top of session state.</span></span> 

## <a name="cookie-based-tempdata-provider"></a><span data-ttu-id="346f9-140">Cookie 架構 TempData 提供者</span><span class="sxs-lookup"><span data-stu-id="346f9-140">Cookie-based TempData provider</span></span> 

<span data-ttu-id="346f9-141">在 ASP.NET Core 1.1 和更新版本，您可以使用 cookie 架構 TempData 提供者來儲存使用者的 TempData 在 cookie 中。</span><span class="sxs-lookup"><span data-stu-id="346f9-141">In ASP.NET Core 1.1 and higher, you can use the cookie-based TempData provider to store a user's TempData in a cookie.</span></span> <span data-ttu-id="346f9-142">若要啟用的 cookie 架構 TempData 提供者，註冊`CookieTempDataProvider`服務`ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="346f9-142">To enable the  cookie-based TempData provider, register the `CookieTempDataProvider` service in `ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();
    // Add CookieTempDataProvider after AddMvc and include ViewFeatures.
    // using Microsoft.AspNetCore.Mvc.ViewFeatures;
    services.AddSingleton<ITempDataProvider, CookieTempDataProvider>();
}
```

<span data-ttu-id="346f9-143">Cookie 資料以編碼[Base64UrlTextEncoder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.authentication.base64urltextencoder)。</span><span class="sxs-lookup"><span data-stu-id="346f9-143">The cookie data is encoded with the [Base64UrlTextEncoder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.authentication.base64urltextencoder).</span></span> <span data-ttu-id="346f9-144">Cookie 會進行加密及區塊處理，因為單一 cookie 大小限制不適用。</span><span class="sxs-lookup"><span data-stu-id="346f9-144">Because the cookie is encrypted and chunked, the single cookie size limit does not apply.</span></span> <span data-ttu-id="346f9-145">Cookie 資料不會壓縮，因為壓縮 encryped 資料可能會導致安全性問題例如[CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit))和[破壞](https://wikipedia.org/wiki/BREACH_(security_exploit))攻擊。</span><span class="sxs-lookup"><span data-stu-id="346f9-145">The cookie data is not compressed, because compressing encryped data can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span> <span data-ttu-id="346f9-146">如需有關 cookie 架構 TempData 提供者的詳細資訊，請參閱[CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs)。</span><span class="sxs-lookup"><span data-stu-id="346f9-146">For more information on the cookie-based TempData provider, see [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs).</span></span>

### <a name="query-strings"></a><span data-ttu-id="346f9-147">查詢字串</span><span class="sxs-lookup"><span data-stu-id="346f9-147">Query strings</span></span>

<span data-ttu-id="346f9-148">您可以傳遞有限的數量的資料從一個要求到另一個方法是加入新的要求查詢字串。</span><span class="sxs-lookup"><span data-stu-id="346f9-148">You can pass a limited amount of data from one request to another by adding it to the new request’s query string.</span></span> <span data-ttu-id="346f9-149">這可用於擷取狀態，以持續方式，可讓連結與內嵌的狀態，要透過電子郵件或社交網路共用。</span><span class="sxs-lookup"><span data-stu-id="346f9-149">This is useful for capturing state in a persistent manner that allows links with embedded state to be shared through email or social networks.</span></span> <span data-ttu-id="346f9-150">不過，基於這個理由，您應該永遠不會使用查詢字串之機密資料。</span><span class="sxs-lookup"><span data-stu-id="346f9-150">However, for this reason,  you should never use query strings for sensitive data.</span></span> <span data-ttu-id="346f9-151">除了輕鬆地共用，在查詢字串中包括的資料可以建立的機會[跨網站要求偽造 (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))可以誘騙瀏覽惡意網站時驗證使用者的攻擊。</span><span class="sxs-lookup"><span data-stu-id="346f9-151">In addition to being easily shared, including data in query strings can create opportunities for [Cross-Site Request Forgery (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) attacks, which can trick users into visiting malicious sites while authenticated.</span></span> <span data-ttu-id="346f9-152">攻擊者可能會再竊取您的應用程式中的使用者資料，或者要代表使用者惡意動作。</span><span class="sxs-lookup"><span data-stu-id="346f9-152">Attackers can then steal user data from your app or take malicious actions on behalf of the user.</span></span> <span data-ttu-id="346f9-153">任何保留的應用程式或工作階段狀態必須防範 CSRF 攻擊。</span><span class="sxs-lookup"><span data-stu-id="346f9-153">Any preserved application or session state must protect against CSRF attacks.</span></span> <span data-ttu-id="346f9-154">如需有關 CSRF 攻擊的詳細資訊，請參閱[防止跨站台要求偽造 (XSRF/CSRF) 攻擊，在 ASP.NET Core](../security/anti-request-forgery.md)。</span><span class="sxs-lookup"><span data-stu-id="346f9-154">For more information on CSRF attacks, see [Preventing Cross-Site Request Forgery (XSRF/CSRF) Attacks in ASP.NET Core](../security/anti-request-forgery.md).</span></span>

### <a name="post-data-and-hidden-fields"></a><span data-ttu-id="346f9-155">張貼資料和隱藏的欄位</span><span class="sxs-lookup"><span data-stu-id="346f9-155">Post data and hidden fields</span></span>

<span data-ttu-id="346f9-156">資料可以儲存在隱藏的表單欄位和公佈回於下一個要求。</span><span class="sxs-lookup"><span data-stu-id="346f9-156">Data can be saved in hidden form fields and posted back on the next request.</span></span> <span data-ttu-id="346f9-157">這是一般多頁的表單中。</span><span class="sxs-lookup"><span data-stu-id="346f9-157">This is common in multipage forms.</span></span> <span data-ttu-id="346f9-158">不過，用戶端可能可以修改資料，因為伺服器必須一律重新驗證它。</span><span class="sxs-lookup"><span data-stu-id="346f9-158">However, because the  client can potentially tamper with the data, the server must always revalidate it.</span></span> 

### <a name="cookies"></a><span data-ttu-id="346f9-159">Cookie</span><span class="sxs-lookup"><span data-stu-id="346f9-159">Cookies</span></span>

<span data-ttu-id="346f9-160">Cookie 會提供在 web 應用程式儲存使用者專屬資料的方法。</span><span class="sxs-lookup"><span data-stu-id="346f9-160">Cookies provide a way to store user-specific data in web applications.</span></span> <span data-ttu-id="346f9-161">每個要求傳送 cookie，因為其大小應該降到最低。</span><span class="sxs-lookup"><span data-stu-id="346f9-161">Because cookies are sent with every request, their size should be kept to a minimum.</span></span> <span data-ttu-id="346f9-162">在理想情況下，只有識別碼應該與實際伺服器上儲存的資料儲存在 cookie 中。</span><span class="sxs-lookup"><span data-stu-id="346f9-162">Ideally, only an identifier should be stored in a cookie with the actual data stored on the server.</span></span> <span data-ttu-id="346f9-163">大部分的瀏覽器限制為 4096 個位元組的 cookie。</span><span class="sxs-lookup"><span data-stu-id="346f9-163">Most browsers restrict cookies to 4096 bytes.</span></span> <span data-ttu-id="346f9-164">此外，只有少數的 cookie 可供每個網域。</span><span class="sxs-lookup"><span data-stu-id="346f9-164">In addition, only a limited number of cookies are available for each domain.</span></span>  

<span data-ttu-id="346f9-165">Cookie 是有可能遭到竄改，因為它們必須在伺服器上驗證。</span><span class="sxs-lookup"><span data-stu-id="346f9-165">Because cookies are subject to tampering, they must be validated on the server.</span></span> <span data-ttu-id="346f9-166">雖然用戶端上的 cookie 持久性受限於使用者介入的情況下與到期日，通常都在用戶端上的資料持續性最持久表單。</span><span class="sxs-lookup"><span data-stu-id="346f9-166">Although the durability of the cookie on a client is subject to user intervention and expiration, they are generally the most durable form of data persistence on the client.</span></span>

<span data-ttu-id="346f9-167">Cookie 通常可用來個人化，其中內容自訂為已知的使用者。</span><span class="sxs-lookup"><span data-stu-id="346f9-167">Cookies are often used for personalization, where content is customized for a known user.</span></span> <span data-ttu-id="346f9-168">因為使用者只能識別，並且尚未驗證在大部分情況下，您通常可以藉由在 cookie 中儲存的使用者名稱、 帳戶名稱或唯一的使用者識別碼 （例如 GUID) 來保護 cookie。</span><span class="sxs-lookup"><span data-stu-id="346f9-168">Because the user is only identified and not authenticated in most cases, you can typically secure a cookie by storing the user name, account name, or a unique user ID (such as a GUID) in the cookie.</span></span> <span data-ttu-id="346f9-169">Cookie 然後可用來存取站台的使用者個人化基礎結構。</span><span class="sxs-lookup"><span data-stu-id="346f9-169">You can then use the cookie to access the user personalization infrastructure of a site.</span></span>

### <a name="httpcontextitems"></a><span data-ttu-id="346f9-170">HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="346f9-170">HttpContext.Items</span></span>

<span data-ttu-id="346f9-171">`Items`集合是個不錯的位置來儲存資料所需時，才處理一個特定的要求。</span><span class="sxs-lookup"><span data-stu-id="346f9-171">The `Items` collection is a good location to store data that is needed only while processing one particular request.</span></span> <span data-ttu-id="346f9-172">每個要求之後，會捨棄集合的內容。</span><span class="sxs-lookup"><span data-stu-id="346f9-172">The collection's contents are discarded after each request.</span></span> <span data-ttu-id="346f9-173">`Items`集合最適合用做為元件或中介軟體的方式來通訊時可操作不同時間點期間要求的時間，並且具有沒有直接的方式，將參數傳遞。</span><span class="sxs-lookup"><span data-stu-id="346f9-173">The `Items` collection is best used as a way for components or middleware to communicate when they operate at different points in time during a request and have no direct way to pass parameters.</span></span> <span data-ttu-id="346f9-174">如需詳細資訊，請參閱[使用 HttpContext.Items](#working-with-httpcontextitems)在本文稍後。</span><span class="sxs-lookup"><span data-stu-id="346f9-174">For more information, see [Working with HttpContext.Items](#working-with-httpcontextitems), later in this article.</span></span>

### <a name="cache"></a><span data-ttu-id="346f9-175">快取</span><span class="sxs-lookup"><span data-stu-id="346f9-175">Cache</span></span>

<span data-ttu-id="346f9-176">快取是有效的方式儲存和擷取資料。</span><span class="sxs-lookup"><span data-stu-id="346f9-176">Caching is an efficient way to store and retrieve data.</span></span> <span data-ttu-id="346f9-177">您可以控制時間和其他考量為基礎的快取項目存留的期。</span><span class="sxs-lookup"><span data-stu-id="346f9-177">You can control the lifetime of cached items based on time and other considerations.</span></span> <span data-ttu-id="346f9-178">深入了解[快取](../performance/caching/index.md)。</span><span class="sxs-lookup"><span data-stu-id="346f9-178">Learn more about [Caching](../performance/caching/index.md).</span></span>

<a name=session></a>

## <a name="configuring-session"></a><span data-ttu-id="346f9-179">設定工作階段</span><span class="sxs-lookup"><span data-stu-id="346f9-179">Configuring Session</span></span>

<span data-ttu-id="346f9-180">`Microsoft.AspNetCore.Session`套件提供中介軟體管理的工作階段狀態。</span><span class="sxs-lookup"><span data-stu-id="346f9-180">The `Microsoft.AspNetCore.Session` package provides middleware for managing session state.</span></span> <span data-ttu-id="346f9-181">若要啟用工作階段中介軟體`Startup`必須包含：</span><span class="sxs-lookup"><span data-stu-id="346f9-181">To enable the session middleware, `Startup`must contain:</span></span>

- <span data-ttu-id="346f9-182">任一[IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache)記憶體快取。</span><span class="sxs-lookup"><span data-stu-id="346f9-182">Any of the [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) memory caches.</span></span> <span data-ttu-id="346f9-183">`IDistributedCache`做為備份存放區使用實作工作階段。</span><span class="sxs-lookup"><span data-stu-id="346f9-183">The `IDistributedCache` implementation is used as a backing store for session.</span></span>
- <span data-ttu-id="346f9-184">[AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_)呼叫，這需要 NuGet 套件 」 Microsoft.AspNetCore.Session"。</span><span class="sxs-lookup"><span data-stu-id="346f9-184">[AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_) call, which requires NuGet package "Microsoft.AspNetCore.Session".</span></span>
- <span data-ttu-id="346f9-185">[UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_)呼叫。</span><span class="sxs-lookup"><span data-stu-id="346f9-185">[UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) call.</span></span>

<span data-ttu-id="346f9-186">下列程式碼會示範如何設定記憶體中工作階段提供者。</span><span class="sxs-lookup"><span data-stu-id="346f9-186">The following code shows how to set up the in-memory session provider.</span></span>

<span data-ttu-id="346f9-187">[!code-csharp[Main](app-state/sample/src/WebAppSession/Startup.cs?highlight=11-19,24)]</span><span class="sxs-lookup"><span data-stu-id="346f9-187">[!code-csharp[Main](app-state/sample/src/WebAppSession/Startup.cs?highlight=11-19,24)]</span></span>

<span data-ttu-id="346f9-188">您可以參考從工作階段`HttpContext`之後就會安裝並設定。</span><span class="sxs-lookup"><span data-stu-id="346f9-188">You can reference Session from `HttpContext` once it is installed and configured.</span></span>

<span data-ttu-id="346f9-189">如果您嘗試存取`Session`之前`UseSession`已經呼叫，例外狀況`InvalidOperationException: Session has not been configured for this application or request`就會擲回。</span><span class="sxs-lookup"><span data-stu-id="346f9-189">If you try to access `Session` before `UseSession` has been called, the exception `InvalidOperationException: Session has not been configured for this application or request` is thrown.</span></span>

<span data-ttu-id="346f9-190">如果您嘗試建立新`Session`（也就是任何工作階段 cookie 具有已建立） 已經開始寫入之後`Response`串流處理的例外狀況`InvalidOperationException: The session cannot be established after the response has started`就會擲回。</span><span class="sxs-lookup"><span data-stu-id="346f9-190">If you try to create a new `Session` (that is, no session cookie has been created) after you have already begun writing to the `Response` stream, the exception `InvalidOperationException: The session cannot be established after the response has started` is thrown.</span></span> <span data-ttu-id="346f9-191">例外狀況可以在 web 伺服器記錄檔。它不會顯示在瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="346f9-191">The exception can be found in the web server log; it will not be displayed in the browser.</span></span>

### <a name="loading-session-asynchronously"></a><span data-ttu-id="346f9-192">非同步載入工作階段</span><span class="sxs-lookup"><span data-stu-id="346f9-192">Loading Session asynchronously</span></span> 

<span data-ttu-id="346f9-193">在 ASP.NET Core 的預設工作階段提供者會從基礎載入工作階段記錄[IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache)存放區以非同步方式只有當[ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync)之前明確地呼叫方法 `TryGetValue`， `Set`，或`Remove`方法。</span><span class="sxs-lookup"><span data-stu-id="346f9-193">The default session provider in ASP.NET Core loads the session record from the underlying [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) store asynchronously only if the [ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) method is explicitly called before  the `TryGetValue`, `Set`, or `Remove` methods.</span></span> <span data-ttu-id="346f9-194">如果`LoadAsync`則不會呼叫第一次，基礎工作階段記錄同步載入，這可能會影響應用程式能夠調整。</span><span class="sxs-lookup"><span data-stu-id="346f9-194">If `LoadAsync` is not called first, the underlying session record is loaded synchronously, which could potentially impact the ability of the app to scale.</span></span>

<span data-ttu-id="346f9-195">若要強制執行此模式的應用程式，請包裝[DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore)和[DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession)實作時擲回例外狀況的版本與`LoadAsync`方法不是之前呼叫`TryGetValue`， `Set`，或`Remove`。</span><span class="sxs-lookup"><span data-stu-id="346f9-195">To have applications enforce this pattern, wrap the [DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore) and [DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession) implementations with versions that throw an exception if the `LoadAsync` method is not called before `TryGetValue`, `Set`, or `Remove`.</span></span> <span data-ttu-id="346f9-196">註冊服務容器中的已包裝的版本。</span><span class="sxs-lookup"><span data-stu-id="346f9-196">Register the wrapped versions in the services container.</span></span>

### <a name="implementation-details"></a><span data-ttu-id="346f9-197">實作詳細資料</span><span class="sxs-lookup"><span data-stu-id="346f9-197">Implementation Details</span></span>

<span data-ttu-id="346f9-198">工作階段會追蹤並找出要求從單一瀏覽器使用 cookie。</span><span class="sxs-lookup"><span data-stu-id="346f9-198">Session uses a cookie to track and identify requests from a single browser.</span></span> <span data-ttu-id="346f9-199">根據預設，此 cookie 名為"。AspNet.Session 」，並使用的路徑"/"。</span><span class="sxs-lookup"><span data-stu-id="346f9-199">By default, this cookie is named ".AspNet.Session", and it uses a path of "/".</span></span> <span data-ttu-id="346f9-200">Cookie 預設未指定網域，因為它不開放給用戶端指令碼頁面上 (因為`CookieHttpOnly`預設為`true`)。</span><span class="sxs-lookup"><span data-stu-id="346f9-200">Because the cookie default does not specify a domain, it is not made available to the client-side script on the page (because `CookieHttpOnly` defaults to `true`).</span></span>

<span data-ttu-id="346f9-201">若要覆寫工作階段的預設值，請使用`SessionOptions`:</span><span class="sxs-lookup"><span data-stu-id="346f9-201">To override session defaults, use `SessionOptions`:</span></span>

<span data-ttu-id="346f9-202">[!code-csharp[Main](app-state/sample/src/WebAppSession/StartupCopy.cs?name=snippet1&highlight=8-12)]</span><span class="sxs-lookup"><span data-stu-id="346f9-202">[!code-csharp[Main](app-state/sample/src/WebAppSession/StartupCopy.cs?name=snippet1&highlight=8-12)]</span></span>

<span data-ttu-id="346f9-203">伺服器會使用`IdleTimeout`屬性來判斷如何在工作階段前可保留長度閒置其內容會被放棄。</span><span class="sxs-lookup"><span data-stu-id="346f9-203">The server uses the `IdleTimeout` property to determine how long a session can be idle before its contents are abandoned.</span></span> <span data-ttu-id="346f9-204">這個屬性是獨立的 cookie 到期日。</span><span class="sxs-lookup"><span data-stu-id="346f9-204">This property is independent of the cookie expiration.</span></span> <span data-ttu-id="346f9-205">通過的工作階段中介軟體 （讀取或寫入） 的每個要求會重設時的逾時。</span><span class="sxs-lookup"><span data-stu-id="346f9-205">Each request that passes through the Session middleware (read from or written to) resets the timeout.</span></span>

<span data-ttu-id="346f9-206">因為`Session`是*非鎖定*，如果兩個要求同時嘗試修改的工作階段中，最後一個內容，會覆寫第一個。</span><span class="sxs-lookup"><span data-stu-id="346f9-206">Because `Session` is *non-locking*, if two requests both attempt to modify the contents of session, the last one overrides the first.</span></span> <span data-ttu-id="346f9-207">`Session`會實作為*一致的工作階段*，這表示所有內容會都儲存在一起。</span><span class="sxs-lookup"><span data-stu-id="346f9-207">`Session` is implemented as a *coherent session*, which means that all the contents are stored together.</span></span> <span data-ttu-id="346f9-208">要修改的工作階段 （不同的索引鍵） 的不同部分的兩個要求仍可能會影響彼此。</span><span class="sxs-lookup"><span data-stu-id="346f9-208">Two requests that are modifying different parts of the session (different keys) might still impact each other.</span></span>

## <a name="setting-and-getting-session-values"></a><span data-ttu-id="346f9-209">設定和取得工作階段值</span><span class="sxs-lookup"><span data-stu-id="346f9-209">Setting and getting Session values</span></span>

<span data-ttu-id="346f9-210">工作階段透過存取`Session`屬性`HttpContext`。</span><span class="sxs-lookup"><span data-stu-id="346f9-210">Session is accessed through the `Session` property on `HttpContext`.</span></span> <span data-ttu-id="346f9-211">這個屬性是[ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession)實作。</span><span class="sxs-lookup"><span data-stu-id="346f9-211">This property is an [ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession) implementation.</span></span>

<span data-ttu-id="346f9-212">下列範例示範設定和取得整數和字串：</span><span class="sxs-lookup"><span data-stu-id="346f9-212">The following example shows setting and getting an int and a string:</span></span>

<span data-ttu-id="346f9-213">[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="346f9-213">[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet1)]</span></span>

<span data-ttu-id="346f9-214">如果您加入下列的擴充方法，您可以設定並取得工作階段可序列化的物件：</span><span class="sxs-lookup"><span data-stu-id="346f9-214">If you add the following extension methods, you can set and get serializable objects to Session:</span></span>

<span data-ttu-id="346f9-215">[!code-csharp[Main](app-state/sample/src/WebAppSession/Extensions/SessionExtensions.cs)]</span><span class="sxs-lookup"><span data-stu-id="346f9-215">[!code-csharp[Main](app-state/sample/src/WebAppSession/Extensions/SessionExtensions.cs)]</span></span>

<span data-ttu-id="346f9-216">下列範例會示範如何設定和取得可序列化的物件：</span><span class="sxs-lookup"><span data-stu-id="346f9-216">The following sample shows how to set and get a serializable object:</span></span>

<span data-ttu-id="346f9-217">[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="346f9-217">[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet2)]</span></span>


## <a name="working-with-httpcontextitems"></a><span data-ttu-id="346f9-218">使用 HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="346f9-218">Working with HttpContext.Items</span></span>

<span data-ttu-id="346f9-219">`HttpContext`字典集合類型的抽象提供支援`IDictionary<object, object>`，稱為`Items`。</span><span class="sxs-lookup"><span data-stu-id="346f9-219">The `HttpContext` abstraction provides support for a dictionary collection of type `IDictionary<object, object>`, called `Items`.</span></span> <span data-ttu-id="346f9-220">此集合可從開頭*HttpRequest*並捨棄每個要求的結尾。</span><span class="sxs-lookup"><span data-stu-id="346f9-220">This collection is available from the start of an *HttpRequest* and is discarded at the end of each request.</span></span> <span data-ttu-id="346f9-221">將值指派至一個索引鍵的項目，或要求特定的索引鍵的值，您可以存取它。</span><span class="sxs-lookup"><span data-stu-id="346f9-221">You can access it by  assigning a value to a keyed entry, or by requesting the value for a particular key.</span></span>

<span data-ttu-id="346f9-222">在下列範例，[中介軟體](middleware.md)新增`isVerified`至`Items`集合。</span><span class="sxs-lookup"><span data-stu-id="346f9-222">In the sample below, [Middleware](middleware.md) adds `isVerified` to the `Items` collection.</span></span>

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

<span data-ttu-id="346f9-223">稍後在管線中，另一個中介軟體，也無法存取它：</span><span class="sxs-lookup"><span data-stu-id="346f9-223">Later in the pipeline, another middleware could access it:</span></span>

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync("Verified request? " + 
        context.Items["isVerified"]);
});
```

<span data-ttu-id="346f9-224">只會由一個單一的應用程式的中介軟體`string`索引鍵是可接受。</span><span class="sxs-lookup"><span data-stu-id="346f9-224">For middleware that will only be used by a single app, `string` keys are acceptable.</span></span> <span data-ttu-id="346f9-225">不過，將應用程式之間共用的中介軟體應該使用唯一物件索引鍵，以避免任何可能發生的索引鍵衝突。</span><span class="sxs-lookup"><span data-stu-id="346f9-225">However, middleware that will be shared between applications should use unique object keys to avoid any chance of key collisions.</span></span> <span data-ttu-id="346f9-226">如果您正在開發必須跨多個應用程式的中介軟體，請使用定義在您的中介軟體類別如下所示的唯一物件索引鍵：</span><span class="sxs-lookup"><span data-stu-id="346f9-226">If you are developing middleware that must work across multiple applications, use a unique object key defined in your middleware class as shown below:</span></span>

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

<span data-ttu-id="346f9-227">其他程式碼可以存取儲存的值`HttpContext.Items`使用由中介軟體類別公開的索引鍵：</span><span class="sxs-lookup"><span data-stu-id="346f9-227">Other code can access the value stored in `HttpContext.Items` using the key exposed by the middleware class:</span></span>

```csharp
public class HomeController : Controller
{
    public IActionResult Index()
    {
        string value = HttpContext.Items[SampleMiddleware.SampleKey];
    }
}
```

<span data-ttu-id="346f9-228">此方法也有消除重複的程式碼中的多個位置中的"magic 字串 」 的優點。</span><span class="sxs-lookup"><span data-stu-id="346f9-228">This approach also has the advantage of eliminating repetition of "magic strings" in multiple places in the code.</span></span>

<a name=appstate-errors></a>

## <a name="application-state-data"></a><span data-ttu-id="346f9-229">應用程式狀態資料</span><span class="sxs-lookup"><span data-stu-id="346f9-229">Application state data</span></span>

<span data-ttu-id="346f9-230">使用[相依性插入](xref:fundamentals/dependency-injection)可讓所有使用者使用資料：</span><span class="sxs-lookup"><span data-stu-id="346f9-230">Use [Dependency Injection](xref:fundamentals/dependency-injection) to make data available to all users:</span></span>

1. <span data-ttu-id="346f9-231">定義包含資料服務 (例如，名為類別`MyAppData`)。</span><span class="sxs-lookup"><span data-stu-id="346f9-231">Define a service containing the data (for example, a class named `MyAppData`).</span></span>

```csharp
public class MyAppData
{
    // Declare properties/methods/etc.
} 
```
2. <span data-ttu-id="346f9-232">將服務類別，加入`ConfigureServices`(例如`services.AddSingleton<MyAppData>();`)。</span><span class="sxs-lookup"><span data-stu-id="346f9-232">Add the service class to `ConfigureServices` (for example `services.AddSingleton<MyAppData>();`).</span></span>
3. <span data-ttu-id="346f9-233">使用每個控制站與資料服務類別：</span><span class="sxs-lookup"><span data-stu-id="346f9-233">Consume the data service class in each controller:</span></span>

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

### <a name="common-errors-when-working-with-session"></a><span data-ttu-id="346f9-234">使用工作階段時的常見錯誤</span><span class="sxs-lookup"><span data-stu-id="346f9-234">Common errors when working with session</span></span>

* <span data-ttu-id="346f9-235">「 無法解析類型 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' 服務嘗試啟動 'Microsoft.AspNetCore.Session.DistributedSessionStore'。 」</span><span class="sxs-lookup"><span data-stu-id="346f9-235">"Unable to resolve service for type 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' while attempting to activate 'Microsoft.AspNetCore.Session.DistributedSessionStore'."</span></span>

  <span data-ttu-id="346f9-236">這種情形通常因未能設定至少一個`IDistributedCache`實作。</span><span class="sxs-lookup"><span data-stu-id="346f9-236">This is usually caused by failing to configure at least one `IDistributedCache` implementation.</span></span> <span data-ttu-id="346f9-237">如需詳細資訊，請參閱[使用分散式快取](xref:performance/caching/distributed)和[中記憶體內部快取](xref:performance/caching/memory)。</span><span class="sxs-lookup"><span data-stu-id="346f9-237">For more information, see [Working with a Distributed Cache](xref:performance/caching/distributed) and [In memory caching](xref:performance/caching/memory).</span></span>

### <a name="additional-resources"></a><span data-ttu-id="346f9-238">其他資源</span><span class="sxs-lookup"><span data-stu-id="346f9-238">Additional Resources</span></span>


* [<span data-ttu-id="346f9-239">本文件中使用的範例程式碼</span><span class="sxs-lookup"><span data-stu-id="346f9-239">Sample code used in this document</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSession)
