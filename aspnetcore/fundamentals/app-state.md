---
title: ASP.NET Core 中的工作階段與應用程式狀態
author: rick-anderson
description: 探索方法以在要求之間保留工作階段和應用程式狀態。
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2018
uid: fundamentals/app-state
ms.openlocfilehash: 072699113a45056ec3ea79436ad56896ba0a4197
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095810"
---
# <a name="session-and-app-state-in-aspnet-core"></a><span data-ttu-id="31ff6-103">ASP.NET Core 中的工作階段與應用程式狀態</span><span class="sxs-lookup"><span data-stu-id="31ff6-103">Session and app state in ASP.NET Core</span></span>

<span data-ttu-id="31ff6-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)、[Steve Smith](https://ardalis.com/)、[Diana LaRose](https://github.com/DianaLaRose) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="31ff6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), [Diana LaRose](https://github.com/DianaLaRose), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="31ff6-105">HTTP 是無狀態的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="31ff6-105">HTTP is a stateless protocol.</span></span> <span data-ttu-id="31ff6-106">若不採取其他步驟，HTTP 要求是獨立的訊息，不會保留使用者的值或應用程式狀態。</span><span class="sxs-lookup"><span data-stu-id="31ff6-106">Without taking additional steps, HTTP requests are independent messages that don't retain user values or app state.</span></span> <span data-ttu-id="31ff6-107">本文描述數種方法來在要求之間保留使用者資料和應用程式狀態。</span><span class="sxs-lookup"><span data-stu-id="31ff6-107">This article describes several approaches to preserve user data and app state between requests.</span></span>

<span data-ttu-id="31ff6-108">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/samples) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="31ff6-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="state-management"></a><span data-ttu-id="31ff6-109">狀態管理</span><span class="sxs-lookup"><span data-stu-id="31ff6-109">State management</span></span>

<span data-ttu-id="31ff6-110">可以使用數種方法來儲存狀態。</span><span class="sxs-lookup"><span data-stu-id="31ff6-110">State can be stored using several approaches.</span></span> <span data-ttu-id="31ff6-111">本主題稍後將提供每種方法的描述。</span><span class="sxs-lookup"><span data-stu-id="31ff6-111">Each approach is described later in this topic.</span></span>

| <span data-ttu-id="31ff6-112">儲存方法</span><span class="sxs-lookup"><span data-stu-id="31ff6-112">Storage approach</span></span> | <span data-ttu-id="31ff6-113">儲存機制</span><span class="sxs-lookup"><span data-stu-id="31ff6-113">Storage mechanism</span></span> |
| ---------------- | ----------------- |
| [<span data-ttu-id="31ff6-114">Cookie</span><span class="sxs-lookup"><span data-stu-id="31ff6-114">Cookies</span></span>](#cookies) | <span data-ttu-id="31ff6-115">HTTP cookie (可能包含使用伺服器端應用程式程式碼所儲存的資料)</span><span class="sxs-lookup"><span data-stu-id="31ff6-115">HTTP cookies (may include data stored using server-side app code)</span></span> |
| [<span data-ttu-id="31ff6-116">工作階段狀態</span><span class="sxs-lookup"><span data-stu-id="31ff6-116">Session state</span></span>](#session-state) | <span data-ttu-id="31ff6-117">HTTP Cookie 和伺服器端應用程式程式碼</span><span class="sxs-lookup"><span data-stu-id="31ff6-117">HTTP cookies and server-side app code</span></span> |
| [<span data-ttu-id="31ff6-118">TempData</span><span class="sxs-lookup"><span data-stu-id="31ff6-118">TempData</span></span>](#tempdata) | <span data-ttu-id="31ff6-119">HTTP Cookie 或工作階段狀態</span><span class="sxs-lookup"><span data-stu-id="31ff6-119">HTTP cookies or session state</span></span> |
| [<span data-ttu-id="31ff6-120">查詢字串</span><span class="sxs-lookup"><span data-stu-id="31ff6-120">Query strings</span></span>](#query-strings) | <span data-ttu-id="31ff6-121">HTTP 查詢字串</span><span class="sxs-lookup"><span data-stu-id="31ff6-121">HTTP query strings</span></span> |
| [<span data-ttu-id="31ff6-122">隱藏欄位</span><span class="sxs-lookup"><span data-stu-id="31ff6-122">Hidden fields</span></span>](#hidden-fields) | <span data-ttu-id="31ff6-123">HTTP 表單欄位</span><span class="sxs-lookup"><span data-stu-id="31ff6-123">HTTP form fields</span></span> |
| [<span data-ttu-id="31ff6-124">HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="31ff6-124">HttpContext.Items</span></span>](#httpcontextitems) | <span data-ttu-id="31ff6-125">伺服器端應用程式程式碼</span><span class="sxs-lookup"><span data-stu-id="31ff6-125">Server-side app code</span></span> |
| [<span data-ttu-id="31ff6-126">快取</span><span class="sxs-lookup"><span data-stu-id="31ff6-126">Cache</span></span>](#cache) | <span data-ttu-id="31ff6-127">伺服器端應用程式程式碼</span><span class="sxs-lookup"><span data-stu-id="31ff6-127">Server-side app code</span></span> |
| [<span data-ttu-id="31ff6-128">相依性插入</span><span class="sxs-lookup"><span data-stu-id="31ff6-128">Dependency Injection</span></span>](#dependency-injection) | <span data-ttu-id="31ff6-129">伺服器端應用程式程式碼</span><span class="sxs-lookup"><span data-stu-id="31ff6-129">Server-side app code</span></span> |

## <a name="cookies"></a><span data-ttu-id="31ff6-130">Cookie</span><span class="sxs-lookup"><span data-stu-id="31ff6-130">Cookies</span></span>

<span data-ttu-id="31ff6-131">Cookie 會在要求之間儲存資料。</span><span class="sxs-lookup"><span data-stu-id="31ff6-131">Cookies store data across requests.</span></span> <span data-ttu-id="31ff6-132">因為 Cookie 會隨著每個要求傳送，所以其大小應該保持最小。</span><span class="sxs-lookup"><span data-stu-id="31ff6-132">Because cookies are sent with every request, their size should be kept to a minimum.</span></span> <span data-ttu-id="31ff6-133">在理想情況下，應該只有識別碼儲存在 Cookie 中，而資料由應用程式儲存。</span><span class="sxs-lookup"><span data-stu-id="31ff6-133">Ideally, only an identifier should be stored in a cookie with the data stored by the app.</span></span> <span data-ttu-id="31ff6-134">大部分的瀏覽器將 Cookie 大小限制為 4096 個位元組。</span><span class="sxs-lookup"><span data-stu-id="31ff6-134">Most browsers restrict cookie size to 4096 bytes.</span></span> <span data-ttu-id="31ff6-135">每個網域只有數量有限的 Cookie 可供使用。</span><span class="sxs-lookup"><span data-stu-id="31ff6-135">Only a limited number of cookies are available for each domain.</span></span>

<span data-ttu-id="31ff6-136">由於 Cookie 可能會遭到竄改，因此必須由應用程式加以驗證。</span><span class="sxs-lookup"><span data-stu-id="31ff6-136">Because cookies are subject to tampering, they must be validated by the app.</span></span> <span data-ttu-id="31ff6-137">使用者可以刪除 Cookie，而且 Cookie 會在用戶端上過期。</span><span class="sxs-lookup"><span data-stu-id="31ff6-137">Cookies can be deleted by users and expire on clients.</span></span> <span data-ttu-id="31ff6-138">不過，Cookie 通常是在用戶端上資料持續性最持久的形式。</span><span class="sxs-lookup"><span data-stu-id="31ff6-138">However, cookies are generally the most durable form of data persistence on the client.</span></span>

<span data-ttu-id="31ff6-139">Cookie 通常可用於個人化，其中內容會針對已知的使用者自訂。</span><span class="sxs-lookup"><span data-stu-id="31ff6-139">Cookies are often used for personalization, where content is customized for a known user.</span></span> <span data-ttu-id="31ff6-140">在大部分情況下，只會識別使用者，而未加以驗證。</span><span class="sxs-lookup"><span data-stu-id="31ff6-140">The user is only identified and not authenticated in most cases.</span></span> <span data-ttu-id="31ff6-141">Cookie 可以儲存使用者的名稱、帳戶名稱或唯一使用者識別碼 (例如 GUID)。</span><span class="sxs-lookup"><span data-stu-id="31ff6-141">The cookie can store the user's name, account name, or unique user ID (such as a GUID).</span></span> <span data-ttu-id="31ff6-142">您接著可以使用 Cookie 來存取使用者的個人化設定，例如其慣用網站的背景色彩。</span><span class="sxs-lookup"><span data-stu-id="31ff6-142">You can then use the cookie to access the user's personalized settings, such as their preferred website background color.</span></span>

<span data-ttu-id="31ff6-143">在發出 Cookie 及處理隱私權顧慮時，請留意[歐盟一般資料保護規定 (GDPR)](https://ec.europa.eu/info/law/law-topic/data-protection)。</span><span class="sxs-lookup"><span data-stu-id="31ff6-143">Be mindful of the [European Union General Data Protection Regulations (GDPR)](https://ec.europa.eu/info/law/law-topic/data-protection) when issuing cookies and dealing with privacy concerns.</span></span> <span data-ttu-id="31ff6-144">如需詳細資訊，請參閱 [ASP.NET Core 中的一般資料保護規定 (GDPR) 支援](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="31ff6-144">For more information, see [General Data Protection Regulation (GDPR) support in ASP.NET Core](xref:security/gdpr).</span></span>

## <a name="session-state"></a><span data-ttu-id="31ff6-145">工作階段狀態</span><span class="sxs-lookup"><span data-stu-id="31ff6-145">Session state</span></span>

<span data-ttu-id="31ff6-146">工作階段狀態是用來在使用者瀏覽 Web 應用程式時存放使用者資料的 ASP.NET Core 情節。</span><span class="sxs-lookup"><span data-stu-id="31ff6-146">Session state is an ASP.NET Core scenario for storage of user data while the user browses a web app.</span></span> <span data-ttu-id="31ff6-147">工作階段狀態使用應用程式所維護的存放區，在用戶端的要求之間保存資料。</span><span class="sxs-lookup"><span data-stu-id="31ff6-147">Session state uses a store maintained by the app to persist data across requests from a client.</span></span> <span data-ttu-id="31ff6-148">工作階段資料受到快取的支援，並被視為暫時資料&mdash;網站沒有工作階段資料應該也會繼續運作。</span><span class="sxs-lookup"><span data-stu-id="31ff6-148">The session data is backed by a cache and considered ephemeral data&mdash;the site should continue to function without the session data.</span></span>

> [!NOTE]
> <span data-ttu-id="31ff6-149">[SignalR](xref:signalr/index) 應用程式中不支援工作階段，因為 [SignalR 中樞](xref:signalr/hubs)可獨立於 HTTP 內容之外而執行。</span><span class="sxs-lookup"><span data-stu-id="31ff6-149">Session isn't supported in [SignalR](xref:signalr/index) apps because a [SignalR Hub](xref:signalr/hubs) may execute independent of an HTTP context.</span></span> <span data-ttu-id="31ff6-150">例如，當長時間輪詢要求由中樞維持開啟，超過要求的 HTTP 內容存留期時，便可能發生此情況。</span><span class="sxs-lookup"><span data-stu-id="31ff6-150">For example, this can occur when a long polling request is held open by a hub beyond the lifetime of the request's HTTP context.</span></span>

<span data-ttu-id="31ff6-151">ASP.NET Core 可維護工作階段狀態，方法是提供包含工作階段識別碼的 Cookie 給用戶端，以便將其隨著每個要求傳送至應用程式。</span><span class="sxs-lookup"><span data-stu-id="31ff6-151">ASP.NET Core maintains session state by providing a cookie to the client that contains a session ID, which is sent to the app with each request.</span></span> <span data-ttu-id="31ff6-152">應用程式則使用工作階段識別碼來擷取工作階段資料。</span><span class="sxs-lookup"><span data-stu-id="31ff6-152">The app uses the session ID to fetch the session data.</span></span>

<span data-ttu-id="31ff6-153">工作階段狀態表現下列行為：</span><span class="sxs-lookup"><span data-stu-id="31ff6-153">Session state exhibits the following behaviors:</span></span>

* <span data-ttu-id="31ff6-154">因為工作階段 Cookie 是瀏覽器所特有，所以不會跨瀏覽器共用工作階段。</span><span class="sxs-lookup"><span data-stu-id="31ff6-154">Because the session cookie is specific to the browser, sessions aren't shared across browsers.</span></span>
* <span data-ttu-id="31ff6-155">在瀏覽器工作階段結束時，會刪除工作階段 Cookie。</span><span class="sxs-lookup"><span data-stu-id="31ff6-155">Session cookies are deleted when the browser session ends.</span></span>
* <span data-ttu-id="31ff6-156">如果收到過期工作階段的 Cookie，則會建立使用相同工作階段 Cookie 的新工作階段。</span><span class="sxs-lookup"><span data-stu-id="31ff6-156">If a cookie is received for an expired session, a new session is created that uses the same session cookie.</span></span>
* <span data-ttu-id="31ff6-157">並不會保留空白工作階段&mdash;必須在工作階段中設定至少一個值，才能在要求之間保存工作階段。</span><span class="sxs-lookup"><span data-stu-id="31ff6-157">Empty sessions aren't retained&mdash;the session must have at least one value set into it to persist the session across requests.</span></span> <span data-ttu-id="31ff6-158">不保留工作階段時，會為每個新的要求產生新的工作階段識別碼。</span><span class="sxs-lookup"><span data-stu-id="31ff6-158">When a session isn't retained, a new session ID is generated for each new request.</span></span>
* <span data-ttu-id="31ff6-159">應用程式會在最後一個要求之後，保留工作階段一段有限的時間。</span><span class="sxs-lookup"><span data-stu-id="31ff6-159">The app retains a session for a limited time after the last request.</span></span> <span data-ttu-id="31ff6-160">應用程式會設定工作階段逾時或使用預設值 20 分鐘。</span><span class="sxs-lookup"><span data-stu-id="31ff6-160">The app either sets the session timeout or uses the default value of 20 minutes.</span></span> <span data-ttu-id="31ff6-161">工作階段狀態適合用來儲存特定工作階段特定的使用者資料，但資料不需要在工作階段之間永久儲存的情況。</span><span class="sxs-lookup"><span data-stu-id="31ff6-161">Session state is ideal for storing user data that's specific to a particular session but where the data doesn't require permanent storage across sessions.</span></span>
* <span data-ttu-id="31ff6-162">工作階段資料會在呼叫 [ISession.Clear](/dotnet/api/microsoft.aspnetcore.http.isession.clear) 實作或工作階段過期時刪除。</span><span class="sxs-lookup"><span data-stu-id="31ff6-162">Session data is deleted either when the [ISession.Clear](/dotnet/api/microsoft.aspnetcore.http.isession.clear) implementation is called or when the session expires.</span></span>
* <span data-ttu-id="31ff6-163">沒有任何預設機制可通知應用程式程式碼，用戶端瀏覽器已關閉，或工作階段 Cookie 遭到刪除或在用戶端上已過期。</span><span class="sxs-lookup"><span data-stu-id="31ff6-163">There's no default mechanism to inform app code that a client browser has been closed or when the session cookie is deleted or expired on the client.</span></span>

> [!WARNING]
> <span data-ttu-id="31ff6-164">請勿將敏感性資料存放在工作階段狀態。</span><span class="sxs-lookup"><span data-stu-id="31ff6-164">Don't store sensitive data in session state.</span></span> <span data-ttu-id="31ff6-165">使用者可能不會關閉瀏覽器，並清除工作階段 Cookie。</span><span class="sxs-lookup"><span data-stu-id="31ff6-165">The user might not close the browser and clear the session cookie.</span></span> <span data-ttu-id="31ff6-166">某些瀏覽器會在瀏覽器視窗之間維護有效的工作階段 Cookie。</span><span class="sxs-lookup"><span data-stu-id="31ff6-166">Some browsers maintain valid session cookies across browser windows.</span></span> <span data-ttu-id="31ff6-167">工作階段可能無法限制為單一使用者&mdash;下一位使用者可能會繼續使用相同的工作階段 Cookie 瀏覽應用程式。</span><span class="sxs-lookup"><span data-stu-id="31ff6-167">A session might not be restricted to a single user&mdash;the next user might continue to browse the app with the same session cookie.</span></span>

<span data-ttu-id="31ff6-168">記憶體中快取提供者會將工作階段資料存放在應用程式所在伺服器的記憶體中。</span><span class="sxs-lookup"><span data-stu-id="31ff6-168">The in-memory cache provider stores session data in the memory of the server where the app resides.</span></span> <span data-ttu-id="31ff6-169">在伺服器陣列案例中：</span><span class="sxs-lookup"><span data-stu-id="31ff6-169">In a server farm scenario:</span></span>

* <span data-ttu-id="31ff6-170">使用「黏性工作階段」將每個工作階段繫結至個別伺服器上的特定應用程式執行個體。</span><span class="sxs-lookup"><span data-stu-id="31ff6-170">Use *sticky sessions* to tie each session to a specific app instance on an individual server.</span></span> <span data-ttu-id="31ff6-171">[Azure App Service](https://azure.microsoft.com/services/app-service/) 預設會使用[應用程式要求路由 (ARR)](/iis/extensions/planning-for-arr/using-the-application-request-routing-module) 來強制執行自黏工作階段。</span><span class="sxs-lookup"><span data-stu-id="31ff6-171">[Azure App Service](https://azure.microsoft.com/services/app-service/) uses [Application Request Routing (ARR)](/iis/extensions/planning-for-arr/using-the-application-request-routing-module) to enforce sticky sessions by default.</span></span> <span data-ttu-id="31ff6-172">不過，黏性工作階段可能會影響延展性，並使 Web 應用程式更新複雜化。</span><span class="sxs-lookup"><span data-stu-id="31ff6-172">However, sticky sessions can affect scalability and complicate web app updates.</span></span> <span data-ttu-id="31ff6-173">較好的方法是使用 Redis 或 SQL Server 分散式快取，這不需要黏性工作階段。</span><span class="sxs-lookup"><span data-stu-id="31ff6-173">A better approach is to use a Redis or SQL Server distributed cache, which doesn't require sticky sessions.</span></span> <span data-ttu-id="31ff6-174">如需詳細資訊，請參閱[使用分散式快取](xref:performance/caching/distributed)。</span><span class="sxs-lookup"><span data-stu-id="31ff6-174">For more information, see [Work with a distributed cache](xref:performance/caching/distributed).</span></span>
* <span data-ttu-id="31ff6-175">工作階段 Cookie 是透過 [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) 加密。</span><span class="sxs-lookup"><span data-stu-id="31ff6-175">The session cookie is encrypted via [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector).</span></span> <span data-ttu-id="31ff6-176">必須正確設定資料保護，以閱讀每一部機器上的工作階段 Cookie。</span><span class="sxs-lookup"><span data-stu-id="31ff6-176">Data Protection must be properly configured to read session cookies on each machine.</span></span> <span data-ttu-id="31ff6-177">如需詳細資訊，請參閱 [ASP.NET Core 的資料保護](xref:security/data-protection/index)和[金鑰儲存提供者](xref:security/data-protection/implementation/key-storage-providers)。</span><span class="sxs-lookup"><span data-stu-id="31ff6-177">For more information, see [Data Protection in ASP.NET Core](xref:security/data-protection/index) and [Key storage providers](xref:security/data-protection/implementation/key-storage-providers).</span></span>

### <a name="configure-session-state"></a><span data-ttu-id="31ff6-178">設定工作階段狀態</span><span class="sxs-lookup"><span data-stu-id="31ff6-178">Configure session state</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="31ff6-179">[Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) 套件隨附於 [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app)，提供用來管理工作階段狀態的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="31ff6-179">The [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), provides middleware for managing session state.</span></span> <span data-ttu-id="31ff6-180">若要啟用工作階段中介軟體，`Startup` 必須包含：</span><span class="sxs-lookup"><span data-stu-id="31ff6-180">To enable the session middleware, `Startup` must contain:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="31ff6-181">[Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) 套件提供用來管理工作階段狀態的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="31ff6-181">The [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) package provides middleware for managing session state.</span></span> <span data-ttu-id="31ff6-182">若要啟用工作階段中介軟體，`Startup` 必須包含：</span><span class="sxs-lookup"><span data-stu-id="31ff6-182">To enable the session middleware, `Startup` must contain:</span></span>

::: moniker-end

* <span data-ttu-id="31ff6-183">任一 [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) 記憶體快取。</span><span class="sxs-lookup"><span data-stu-id="31ff6-183">Any of the [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) memory caches.</span></span> <span data-ttu-id="31ff6-184">`IDistributedCache` 實作會作為工作階段的支援存放區。</span><span class="sxs-lookup"><span data-stu-id="31ff6-184">The `IDistributedCache` implementation is used as a backing store for session.</span></span> <span data-ttu-id="31ff6-185">如需詳細資訊，請參閱[使用分散式快取](xref:performance/caching/distributed)。</span><span class="sxs-lookup"><span data-stu-id="31ff6-185">For more information, see [Work with a distributed cache](xref:performance/caching/distributed).</span></span>
* <span data-ttu-id="31ff6-186">在 `ConfigureServices` 中呼叫 [AddSession](/dotnet/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions.addsession)。</span><span class="sxs-lookup"><span data-stu-id="31ff6-186">A call to [AddSession](/dotnet/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions.addsession) in `ConfigureServices`.</span></span>
* <span data-ttu-id="31ff6-187">在 `Configure` 中呼叫 [UseSession](/dotnet/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_)。</span><span class="sxs-lookup"><span data-stu-id="31ff6-187">A call to [UseSession](/dotnet/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) in `Configure`.</span></span>

<span data-ttu-id="31ff6-188">下列程式碼示範如何設定記憶體內部工作階段提供者，以及 `IDistributedCache` 的預設記憶體中實作。</span><span class="sxs-lookup"><span data-stu-id="31ff6-188">The following code shows how to set up the in-memory session provider with a default in-memory implementation of `IDistributedCache`:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13-18,39)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Startup.cs?name=snippet1&highlight=5,7-12,19)]

::: moniker-end

<span data-ttu-id="31ff6-189">中介軟體的順序很重要。</span><span class="sxs-lookup"><span data-stu-id="31ff6-189">The order of middleware is important.</span></span> <span data-ttu-id="31ff6-190">在上述範例中，如果在 `UseMvc` 之後叫用 `UseSession`，則會發生 `InvalidOperationException`　例外狀況。</span><span class="sxs-lookup"><span data-stu-id="31ff6-190">In the preceding example, an `InvalidOperationException` exception occurs when `UseSession` is invoked after `UseMvc`.</span></span> <span data-ttu-id="31ff6-191">如需詳細資訊，請參閱[中介軟體順序](xref:fundamentals/middleware/index#ordering)。</span><span class="sxs-lookup"><span data-stu-id="31ff6-191">For more information, see [Middleware Ordering](xref:fundamentals/middleware/index#ordering).</span></span>

<span data-ttu-id="31ff6-192">設定工作階段狀態之後便可以使用 [HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session)。</span><span class="sxs-lookup"><span data-stu-id="31ff6-192">[HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session) is available after session state is configured.</span></span>

<span data-ttu-id="31ff6-193">呼叫 `UseSession` 之前無法存取 `HttpContext.Session`。</span><span class="sxs-lookup"><span data-stu-id="31ff6-193">`HttpContext.Session` can't be accessed before `UseSession` has been called.</span></span>

<span data-ttu-id="31ff6-194">應用程式開始寫入回應資料流之後，無法建立具有新工作階段 Cookie 的新工作階段。</span><span class="sxs-lookup"><span data-stu-id="31ff6-194">A new session with a new session cookie can't be created after the app has begun writing to the response stream.</span></span> <span data-ttu-id="31ff6-195">例外狀況會記錄在 Web 伺服器記錄檔中，而不會顯示在瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="31ff6-195">The exception is recorded in the web server log and not displayed in the browser.</span></span>

### <a name="load-session-state-asynchronously"></a><span data-ttu-id="31ff6-196">非同步載入工作階段狀態</span><span class="sxs-lookup"><span data-stu-id="31ff6-196">Load session state asynchronously</span></span>

<span data-ttu-id="31ff6-197">只有在 [TryGetValue](/dotnet/api/microsoft.aspnetcore.http.isession.trygetvalue)、[Set](/dotnet/api/microsoft.aspnetcore.http.isession.set) 或 [Remove](/dotnet/api/microsoft.aspnetcore.http.isession.remove) 方法之前明確呼叫 [ISession.LoadAsync](/dotnet/api/microsoft.aspnetcore.http.isession.loadasync) 方法時，ASP.NET Core 中的預設工作階段提供者才會從基礎 [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) 備份存放區以非同步方式載入工作階段記錄。</span><span class="sxs-lookup"><span data-stu-id="31ff6-197">The default session provider in ASP.NET Core loads session records from the underlying [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) backing store asynchronously only if the [ISession.LoadAsync](/dotnet/api/microsoft.aspnetcore.http.isession.loadasync) method is explicitly called before the [TryGetValue](/dotnet/api/microsoft.aspnetcore.http.isession.trygetvalue), [Set](/dotnet/api/microsoft.aspnetcore.http.isession.set), or [Remove](/dotnet/api/microsoft.aspnetcore.http.isession.remove) methods.</span></span> <span data-ttu-id="31ff6-198">如果並未先呼叫 `LoadAsync`，則基礎工作階段記錄會同步載入，這可能大規模地為效能帶來負面影響。</span><span class="sxs-lookup"><span data-stu-id="31ff6-198">If `LoadAsync` isn't called first, the underlying session record is loaded synchronously, which can incur a performance penalty at scale.</span></span>

<span data-ttu-id="31ff6-199">若要讓應用程式強制執行此模式，請使用未在 `TryGetValue`、`Set` 或 `Remove` 之前呼叫 `LoadAsync` 方法時擲回例外狀況的版本來包裝 [DistributedSessionStore](/dotnet/api/microsoft.aspnetcore.session.distributedsessionstore) 和 [DistributedSession](/dotnet/api/microsoft.aspnetcore.session.distributedsession) 實作。</span><span class="sxs-lookup"><span data-stu-id="31ff6-199">To have apps enforce this pattern, wrap the [DistributedSessionStore](/dotnet/api/microsoft.aspnetcore.session.distributedsessionstore) and [DistributedSession](/dotnet/api/microsoft.aspnetcore.session.distributedsession) implementations with versions that throw an exception if the `LoadAsync` method isn't called before `TryGetValue`, `Set`, or `Remove`.</span></span> <span data-ttu-id="31ff6-200">請在服務容器中註冊已包裝的版本。</span><span class="sxs-lookup"><span data-stu-id="31ff6-200">Register the wrapped versions in the services container.</span></span>

### <a name="session-options"></a><span data-ttu-id="31ff6-201">工作階段選項</span><span class="sxs-lookup"><span data-stu-id="31ff6-201">Session options</span></span>

<span data-ttu-id="31ff6-202">若要覆寫工作階段的預設值，請使用 [SessionOptions](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions)。</span><span class="sxs-lookup"><span data-stu-id="31ff6-202">To override session defaults, use [SessionOptions](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions).</span></span>

::: moniker range=">= aspnetcore-2.0"

| <span data-ttu-id="31ff6-203">選項</span><span class="sxs-lookup"><span data-stu-id="31ff6-203">Option</span></span> | <span data-ttu-id="31ff6-204">描述</span><span class="sxs-lookup"><span data-stu-id="31ff6-204">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="31ff6-205">Cookie</span><span class="sxs-lookup"><span data-stu-id="31ff6-205">Cookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookie) | <span data-ttu-id="31ff6-206">決定用來建立 Cookie 的設定。</span><span class="sxs-lookup"><span data-stu-id="31ff6-206">Determines the settings used to create the cookie.</span></span> <span data-ttu-id="31ff6-207">[Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name) 預設為 [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`)。</span><span class="sxs-lookup"><span data-stu-id="31ff6-207">[Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name) defaults to [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`).</span></span> <span data-ttu-id="31ff6-208">[Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path) 預設為 [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`)。</span><span class="sxs-lookup"><span data-stu-id="31ff6-208">[Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path) defaults to [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`).</span></span> <span data-ttu-id="31ff6-209">[SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite) 預設為 [SameSiteMode.Lax](/dotnet/api/microsoft.aspnetcore.http.samesitemode) (`1`)。</span><span class="sxs-lookup"><span data-stu-id="31ff6-209">[SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite) defaults to [SameSiteMode.Lax](/dotnet/api/microsoft.aspnetcore.http.samesitemode) (`1`).</span></span> <span data-ttu-id="31ff6-210">[HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) 預設為 `true`。</span><span class="sxs-lookup"><span data-stu-id="31ff6-210">[HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) defaults to `true`.</span></span> <span data-ttu-id="31ff6-211">[IsEssential](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.isessential) 預設為 `false`。</span><span class="sxs-lookup"><span data-stu-id="31ff6-211">[IsEssential](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.isessential) defaults to `false`.</span></span> |
| [<span data-ttu-id="31ff6-212">IdleTimeout</span><span class="sxs-lookup"><span data-stu-id="31ff6-212">IdleTimeout</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) | <span data-ttu-id="31ff6-213">`IdleTimeout` 指出工作階段可以閒置多久，之後才會放棄它的內容。</span><span class="sxs-lookup"><span data-stu-id="31ff6-213">The `IdleTimeout` indicates how long the session can be idle before its contents are abandoned.</span></span> <span data-ttu-id="31ff6-214">每個工作階段存取都會重設逾時。</span><span class="sxs-lookup"><span data-stu-id="31ff6-214">Each session access resets the timeout.</span></span> <span data-ttu-id="31ff6-215">請注意，這只適用於工作階段的內容，而不適用於 Cookie。</span><span class="sxs-lookup"><span data-stu-id="31ff6-215">Note this only applies to the content of the session, not the cookie.</span></span> <span data-ttu-id="31ff6-216">預設值是 20 分鐘。</span><span class="sxs-lookup"><span data-stu-id="31ff6-216">The default is 20 minutes.</span></span> |
| [<span data-ttu-id="31ff6-217">IOTimeout</span><span class="sxs-lookup"><span data-stu-id="31ff6-217">IOTimeout</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.iotimeout) | <span data-ttu-id="31ff6-218">從存放區載入工作階段，或將它認可回到存放區時，所允許的最大時間量。</span><span class="sxs-lookup"><span data-stu-id="31ff6-218">The maximim amount of time allowed to load a session from the store or to commit it back to the store.</span></span> <span data-ttu-id="31ff6-219">請注意，這可能只適用於非同步作業。</span><span class="sxs-lookup"><span data-stu-id="31ff6-219">Note this may only apply to asynchronous operations.</span></span> <span data-ttu-id="31ff6-220">此逾時可以使用 [InfiniteTimeSpan](/dotnet/api/system.threading.timeout.infinitetimespan) 來停用。</span><span class="sxs-lookup"><span data-stu-id="31ff6-220">This timeout can be disabled using [InfiniteTimeSpan](/dotnet/api/system.threading.timeout.infinitetimespan).</span></span> <span data-ttu-id="31ff6-221">預設為 1 分鐘。</span><span class="sxs-lookup"><span data-stu-id="31ff6-221">The default is 1 minute.</span></span> |

<span data-ttu-id="31ff6-222">工作階段使用 Cookie 來追蹤和識別來自單一瀏覽器的要求。</span><span class="sxs-lookup"><span data-stu-id="31ff6-222">Session uses a cookie to track and identify requests from a single browser.</span></span> <span data-ttu-id="31ff6-223">此 Cookie 預設名為 `.AspNetCore.Session`，並使用路徑 `/`。</span><span class="sxs-lookup"><span data-stu-id="31ff6-223">By default, this cookie is named `.AspNetCore.Session`, and it uses a path of `/`.</span></span> <span data-ttu-id="31ff6-224">由於 Cookie 預設值未指定網域，因此它不會提供給頁面上的用戶端指令碼 (因為 [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) 預設為 `true`)。</span><span class="sxs-lookup"><span data-stu-id="31ff6-224">Because the cookie default doesn't specify a domain, it isn't made available to the client-side script on the page (because [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) defaults to `true`).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| <span data-ttu-id="31ff6-225">選項</span><span class="sxs-lookup"><span data-stu-id="31ff6-225">Option</span></span> | <span data-ttu-id="31ff6-226">描述</span><span class="sxs-lookup"><span data-stu-id="31ff6-226">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="31ff6-227">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="31ff6-227">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiedomain) | <span data-ttu-id="31ff6-228">決定用來建立 Cookie 的網域。</span><span class="sxs-lookup"><span data-stu-id="31ff6-228">Determines the domain used to create the cookie.</span></span> <span data-ttu-id="31ff6-229">預設不會設定 `CookieDomain`。</span><span class="sxs-lookup"><span data-stu-id="31ff6-229">`CookieDomain` isn't set by default.</span></span> |
| [<span data-ttu-id="31ff6-230">CookieHttpOnly</span><span class="sxs-lookup"><span data-stu-id="31ff6-230">CookieHttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiehttponly) | <span data-ttu-id="31ff6-231">決定瀏覽器是否應該允許 Cookie 由用戶端 JavaScript 存取。</span><span class="sxs-lookup"><span data-stu-id="31ff6-231">Determines if the browser should allow the cookie to be accessed by client-side JavaScript.</span></span> <span data-ttu-id="31ff6-232">預設值是 `true`，這表示 Cookie 僅會傳遞給 HTTP 要求，並未開放給頁面上的指令碼使用。</span><span class="sxs-lookup"><span data-stu-id="31ff6-232">The default is `true`, which means the cookie is only passed to HTTP requests and isn't made available to script on the page.</span></span> |
| [<span data-ttu-id="31ff6-233">CookieName</span><span class="sxs-lookup"><span data-stu-id="31ff6-233">CookieName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiename) | <span data-ttu-id="31ff6-234">決定用來保存工作階段識別碼的 Cookie 名稱。</span><span class="sxs-lookup"><span data-stu-id="31ff6-234">Determines the cookie name used to persist the session ID.</span></span> <span data-ttu-id="31ff6-235">預設值是 [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`)。</span><span class="sxs-lookup"><span data-stu-id="31ff6-235">The default is [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`).</span></span> |
| [<span data-ttu-id="31ff6-236">CookiePath</span><span class="sxs-lookup"><span data-stu-id="31ff6-236">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiepath) | <span data-ttu-id="31ff6-237">決定用來建立 Cookie 的路徑。</span><span class="sxs-lookup"><span data-stu-id="31ff6-237">Determines the path used to create the cookie.</span></span> <span data-ttu-id="31ff6-238">預設為 [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`)。</span><span class="sxs-lookup"><span data-stu-id="31ff6-238">Defaults to [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`).</span></span> |
| [<span data-ttu-id="31ff6-239">CookieSecure</span><span class="sxs-lookup"><span data-stu-id="31ff6-239">CookieSecure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiesecure) | <span data-ttu-id="31ff6-240">決定是否應該只在 HTTPS 要求傳送 Cookie。</span><span class="sxs-lookup"><span data-stu-id="31ff6-240">Determines if the cookie should only be transmitted on HTTPS requests.</span></span> <span data-ttu-id="31ff6-241">預設值是 [CookieSecurePolicy.None](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy) (`2`)。</span><span class="sxs-lookup"><span data-stu-id="31ff6-241">The default is [CookieSecurePolicy.None](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy) (`2`).</span></span> |
| [<span data-ttu-id="31ff6-242">IdleTimeout</span><span class="sxs-lookup"><span data-stu-id="31ff6-242">IdleTimeout</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) | <span data-ttu-id="31ff6-243">`IdleTimeout` 指出工作階段可以閒置多久，之後才會放棄它的內容。</span><span class="sxs-lookup"><span data-stu-id="31ff6-243">The `IdleTimeout` indicates how long the session can be idle before its contents are abandoned.</span></span> <span data-ttu-id="31ff6-244">每個工作階段存取都會重設逾時。</span><span class="sxs-lookup"><span data-stu-id="31ff6-244">Each session access resets the timeout.</span></span> <span data-ttu-id="31ff6-245">請注意，這只適用於工作階段的內容，而不適用於 Cookie。</span><span class="sxs-lookup"><span data-stu-id="31ff6-245">Note this only applies to the content of the session, not the cookie.</span></span> <span data-ttu-id="31ff6-246">預設值是 20 分鐘。</span><span class="sxs-lookup"><span data-stu-id="31ff6-246">The default is 20 minutes.</span></span> |

<span data-ttu-id="31ff6-247">工作階段使用 Cookie 來追蹤和識別來自單一瀏覽器的要求。</span><span class="sxs-lookup"><span data-stu-id="31ff6-247">Session uses a cookie to track and identify requests from a single browser.</span></span> <span data-ttu-id="31ff6-248">此 Cookie 預設名為 `.AspNet.Session`，並使用路徑 `/`。</span><span class="sxs-lookup"><span data-stu-id="31ff6-248">By default, this cookie is named `.AspNet.Session`, and it uses a path of `/`.</span></span>

::: moniker-end

<span data-ttu-id="31ff6-249">若要覆寫 Cookie 工作階段的預設值，請使用 `SessionOptions`：</span><span class="sxs-lookup"><span data-stu-id="31ff6-249">To override cookie session defaults, use `SessionOptions`:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples_snapshot/2.x/SessionSample/Startup.cs?name=snippet1&highlight=13-18)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples_snapshot/1.x/SessionSample/Startup.cs?name=snippet1&highlight=5-9)]

::: moniker-end

<span data-ttu-id="31ff6-250">應用程式會使用 [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) 屬性，判斷工作階段可以在放棄伺服器快取中的內容之前閒置多長時間。</span><span class="sxs-lookup"><span data-stu-id="31ff6-250">The app uses the [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) property to determine how long a session can be idle before its contents in the server's cache are abandoned.</span></span> <span data-ttu-id="31ff6-251">這個屬性與 Cookie 到期日無關。</span><span class="sxs-lookup"><span data-stu-id="31ff6-251">This property is independent of the cookie expiration.</span></span> <span data-ttu-id="31ff6-252">透過[工作階段中介軟體](/dotnet/api/microsoft.aspnetcore.session.sessionmiddleware)傳遞的每個要求都會重設逾時。</span><span class="sxs-lookup"><span data-stu-id="31ff6-252">Each request that passes through the [Session Middleware](/dotnet/api/microsoft.aspnetcore.session.sessionmiddleware) resets the timeout.</span></span>

<span data-ttu-id="31ff6-253">工作階段狀態為「非鎖定」。</span><span class="sxs-lookup"><span data-stu-id="31ff6-253">Session state is *non-locking*.</span></span> <span data-ttu-id="31ff6-254">如果兩個要求同時嘗試修改工作階段的內容，則最後一個要求會覆寫第一個要求。</span><span class="sxs-lookup"><span data-stu-id="31ff6-254">If two requests simultaneously attempt to modify the contents of a session, the last request overrides the first.</span></span> <span data-ttu-id="31ff6-255">`Session` 會實作為「一致性工作階段」，這表示所有內容會都儲存在一起。</span><span class="sxs-lookup"><span data-stu-id="31ff6-255">`Session` is implemented as a *coherent session*, which means that all the contents are stored together.</span></span> <span data-ttu-id="31ff6-256">當兩個要求試圖修改不同的工作階段值時，最後一個要求可能會覆寫第一個要求所做的工作階段變更。</span><span class="sxs-lookup"><span data-stu-id="31ff6-256">When two requests seek to modify different session values, the last request may override session changes made by the first.</span></span>

### <a name="set-and-get-session-values"></a><span data-ttu-id="31ff6-257">設定和取得工作階段值</span><span class="sxs-lookup"><span data-stu-id="31ff6-257">Set and get Session values</span></span>

<span data-ttu-id="31ff6-258">工作階段狀態的存取，是從 Razor Pages [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) 類別，或 MVC [Controller](/dotnet/api/microsoft.aspnetcore.mvc.controller) 類別搭配 [HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session)。</span><span class="sxs-lookup"><span data-stu-id="31ff6-258">Session state is accessed from a Razor Pages [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) class or MVC [Controller](/dotnet/api/microsoft.aspnetcore.mvc.controller) class with [HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session).</span></span> <span data-ttu-id="31ff6-259">這個屬性是 [ISession](/dotnet/api/microsoft.aspnetcore.http.isession) 實作。</span><span class="sxs-lookup"><span data-stu-id="31ff6-259">This property is an [ISession](/dotnet/api/microsoft.aspnetcore.http.isession) implementation.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="31ff6-260">`ISession` 實作提供數個擴充方法來設定和擷取整數和字串值。</span><span class="sxs-lookup"><span data-stu-id="31ff6-260">The `ISession` implementation provides several extension methods to set and retreive integer and string values.</span></span> <span data-ttu-id="31ff6-261">當專案參考 [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) 套件時，擴充方法位於 [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) 命名空間 (新增 `using Microsoft.AspNetCore.Http;` 陳述式即可取得擴充方法的存取權)。</span><span class="sxs-lookup"><span data-stu-id="31ff6-261">The extension methods are in the [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) namespace (add a `using Microsoft.AspNetCore.Http;` statement to gain access to the extension methods) when the [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) package is referenced by the project.</span></span> <span data-ttu-id="31ff6-262">[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)包含兩個套件。</span><span class="sxs-lookup"><span data-stu-id="31ff6-262">Both packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="31ff6-263">`ISession` 實作提供數個擴充方法來設定和擷取整數和字串值。</span><span class="sxs-lookup"><span data-stu-id="31ff6-263">The `ISession` implementation provides several extension methods to set and retreive integer and string values.</span></span> <span data-ttu-id="31ff6-264">當專案參考 [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) 套件時，擴充方法位於 [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) 命名空間 (新增 `using Microsoft.AspNetCore.Http;` 陳述式即可取得擴充方法的存取權)。</span><span class="sxs-lookup"><span data-stu-id="31ff6-264">The extension methods are in the [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) namespace (add a `using Microsoft.AspNetCore.Http;` statement to gain access to the extension methods) when the [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) package is referenced by the project.</span></span>

::: moniker-end

<span data-ttu-id="31ff6-265">`ISession` 擴充方法：</span><span class="sxs-lookup"><span data-stu-id="31ff6-265">`ISession` extension methods:</span></span>

* [<span data-ttu-id="31ff6-266">Get(ISession, String)</span><span class="sxs-lookup"><span data-stu-id="31ff6-266">Get(ISession, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.get)
* [<span data-ttu-id="31ff6-267">GetInt32(ISession, String)</span><span class="sxs-lookup"><span data-stu-id="31ff6-267">GetInt32(ISession, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.getint32)
* [<span data-ttu-id="31ff6-268">GetString(ISession, String)</span><span class="sxs-lookup"><span data-stu-id="31ff6-268">GetString(ISession, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.getstring)
* [<span data-ttu-id="31ff6-269">SetInt32(ISession, String, Int32)</span><span class="sxs-lookup"><span data-stu-id="31ff6-269">SetInt32(ISession, String, Int32)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.setint32)
* [<span data-ttu-id="31ff6-270">SetString(ISession, String, String)</span><span class="sxs-lookup"><span data-stu-id="31ff6-270">SetString(ISession, String, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.setstring)

<span data-ttu-id="31ff6-271">下列範例會在 Razor Pages 頁面中，擷取 `IndexModel.SessionKeyName` 索引鍵的工作階段值 (範例應用程式中的 `_Name`)：</span><span class="sxs-lookup"><span data-stu-id="31ff6-271">The following example retrieves the session value for the `IndexModel.SessionKeyName` key (`_Name` in the sample app) in a Razor Pages page:</span></span>

```csharp
@page
@using Microsoft.AspNetCore.Http
@model IndexModel

...

Name: @HttpContext.Session.GetString(IndexModel.SessionKeyName)
```

<span data-ttu-id="31ff6-272">下列範例示範如何設定及取得整數和字串：</span><span class="sxs-lookup"><span data-stu-id="31ff6-272">The following example shows how to set and get an integer and a string:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=18-19,22-23)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet1&highlight=10-11,18-19)]

::: moniker-end

<span data-ttu-id="31ff6-273">若要啟用分散式快取案例，即使是使用記憶體中快取時，都必須序列化所有工作階段資料。</span><span class="sxs-lookup"><span data-stu-id="31ff6-273">All session data must be serialized to enable a distributed cache scenario, even when using the in-memory cache.</span></span> <span data-ttu-id="31ff6-274">已提供最小字串及數字的序列化程式 (請參閱 [ISession](/dotnet/api/microsoft.aspnetcore.http.isession) 的方法和擴充方法)。</span><span class="sxs-lookup"><span data-stu-id="31ff6-274">Minimal string and number serializers are provided (see the methods and extension methods of [ISession](/dotnet/api/microsoft.aspnetcore.http.isession)).</span></span> <span data-ttu-id="31ff6-275">複雜類型必須由使用者使用另一個機制加以序列化，例如 JSON。</span><span class="sxs-lookup"><span data-stu-id="31ff6-275">Complex types must be serialized by the user using another mechanism, such as JSON.</span></span>

<span data-ttu-id="31ff6-276">新增下列擴充方法，即可設定和取得可序列化物件：</span><span class="sxs-lookup"><span data-stu-id="31ff6-276">Add the following extension methods to set and get serializable objects:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="31ff6-277">下列範例示範如何使用擴充方法來設定和取得可序列化物件：</span><span class="sxs-lookup"><span data-stu-id="31ff6-277">The following example shows how to set and get a serializable object with the extension methods:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet2&highlight=4,12)]

::: moniker-end

## <a name="tempdata"></a><span data-ttu-id="31ff6-278">TempData</span><span class="sxs-lookup"><span data-stu-id="31ff6-278">TempData</span></span>

<span data-ttu-id="31ff6-279">ASP.NET Core 會公開 [Razor Pages 頁面模型的 TempData 屬性](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.tempdata)或 [MVC 控制器的 TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata)。</span><span class="sxs-lookup"><span data-stu-id="31ff6-279">ASP.NET Core exposes the [TempData property of a Razor Pages page model](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.tempdata) or [TempData of an MVC controller](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata).</span></span> <span data-ttu-id="31ff6-280">這個屬性會儲存資料，直到讀取為止。</span><span class="sxs-lookup"><span data-stu-id="31ff6-280">This property stores data until it's read.</span></span> <span data-ttu-id="31ff6-281">[Keep](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.keep) 和 [Peek](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.peek) 方法可以用來檢查資料，不用刪除。</span><span class="sxs-lookup"><span data-stu-id="31ff6-281">The [Keep](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.keep) and [Peek](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.peek) methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="31ff6-282">當有多個要求需要資料時，TempData 對重新導向很有幫助。</span><span class="sxs-lookup"><span data-stu-id="31ff6-282">TempData is particularly useful for redirection when data is required for more than a single request.</span></span> <span data-ttu-id="31ff6-283">TempData 是由 TempData 提供者使用 Cookie 或工作階段狀態來實作。</span><span class="sxs-lookup"><span data-stu-id="31ff6-283">TempData is implemented by TempData providers using either cookies or session state.</span></span>

### <a name="tempdata-providers"></a><span data-ttu-id="31ff6-284">TempData 提供者</span><span class="sxs-lookup"><span data-stu-id="31ff6-284">TempData providers</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="31ff6-285">在 ASP.NET Core 2.0 或更新版本中，預設會使用 Cookie 架構 TempData 提供者，將 TempData 儲存在 Cookie 中。</span><span class="sxs-lookup"><span data-stu-id="31ff6-285">In ASP.NET Core 2.0 or later, the cookie-based TempData provider is used by default to store TempData in cookies.</span></span>

<span data-ttu-id="31ff6-286">Cookie 資料是使用 [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) 加密，並使用 [Base64UrlTextEncoder](/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder) 編碼，然後分成區塊。</span><span class="sxs-lookup"><span data-stu-id="31ff6-286">The cookie data is encrypted using [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector), encoded with [Base64UrlTextEncoder](/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder), then chunked.</span></span> <span data-ttu-id="31ff6-287">因為 Cookie 分成區塊，所以不適用 ASP.NET Core 1.x 中找到的 Cookie 大小限制。</span><span class="sxs-lookup"><span data-stu-id="31ff6-287">Because the cookie is chunked, the single cookie size limit found in ASP.NET Core 1.x doesn't apply.</span></span> <span data-ttu-id="31ff6-288">Cookie 資料不會壓縮，因為壓縮加密資料可能會導致安全性問題，例如 [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) 和 [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) 攻擊。</span><span class="sxs-lookup"><span data-stu-id="31ff6-288">The cookie data isn't compressed because compressing encrypted data can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span> <span data-ttu-id="31ff6-289">如需 Cookie 架構 TempData 提供者的詳細資訊，請參閱 [CookieTempDataProvider](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.cookietempdataprovider)。</span><span class="sxs-lookup"><span data-stu-id="31ff6-289">For more information on the cookie-based TempData provider, see [CookieTempDataProvider](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.cookietempdataprovider).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="31ff6-290">在 ASP.NET Core 1.0 和 1.1 中，工作階段狀態 TempData 提供者是預設提供者。</span><span class="sxs-lookup"><span data-stu-id="31ff6-290">In ASP.NET Core 1.0 and 1.1, the session state TempData provider is the default provider.</span></span>

::: moniker-end

### <a name="choose-a-tempdata-provider"></a><span data-ttu-id="31ff6-291">選擇 TempData 提供者</span><span class="sxs-lookup"><span data-stu-id="31ff6-291">Choose a TempData provider</span></span>

<span data-ttu-id="31ff6-292">選擇 TempData 提供者涉及數項考量，例如：</span><span class="sxs-lookup"><span data-stu-id="31ff6-292">Choosing a TempData provider involves several considerations, such as:</span></span>

1. <span data-ttu-id="31ff6-293">應用程式已經使用工作階段狀態了嗎？</span><span class="sxs-lookup"><span data-stu-id="31ff6-293">Does the app already use session state?</span></span> <span data-ttu-id="31ff6-294">如果是的話，使用工作階段狀態 TempData 提供者沒有額外的應用程式成本 (除了資料的大小之外)。</span><span class="sxs-lookup"><span data-stu-id="31ff6-294">If so, using the session state TempData provider has no additional cost to the app (aside from the size of the data).</span></span>
2. <span data-ttu-id="31ff6-295">應用程式是否盡量只將 TempData 用於相對少量的資料 (最多 500 個位元組)？</span><span class="sxs-lookup"><span data-stu-id="31ff6-295">Does the app use TempData only sparingly for relatively small amounts of data (up to 500 bytes)?</span></span> <span data-ttu-id="31ff6-296">如果是的話，Cookie TempData 提供者將對包含 TempData 的每個要求新增少量成本。</span><span class="sxs-lookup"><span data-stu-id="31ff6-296">If so, the cookie TempData provider adds a small cost to each request that carries TempData.</span></span> <span data-ttu-id="31ff6-297">如果不是的話，則工作階段狀態 TempData 提供者可能有助於避免在每個要求中來回傳送大量資料，直到取用 TempData 為止。</span><span class="sxs-lookup"><span data-stu-id="31ff6-297">If not, the session state TempData provider can be beneficial to avoid round-tripping a large amount of data in each request until the TempData is consumed.</span></span>
3. <span data-ttu-id="31ff6-298">應用程式在伺服器陣列中的多部伺服器上執行？</span><span class="sxs-lookup"><span data-stu-id="31ff6-298">Does the app run in a server farm on multiple servers?</span></span> <span data-ttu-id="31ff6-299">如果是的話，不需要額外組態，即可在資料保護之外使用 Cookie TempData 提供者 (請參閱[資料保護](xref:security/data-protection/index)和[金鑰儲存提供者](xref:security/data-protection/implementation/key-storage-providers))。</span><span class="sxs-lookup"><span data-stu-id="31ff6-299">If so, there's no additional configuration required to use the cookie TempData provider outside of Data Protection (see [Data Protection](xref:security/data-protection/index) and [Key storage providers](xref:security/data-protection/implementation/key-storage-providers)).</span></span>

> [!NOTE]
> <span data-ttu-id="31ff6-300">大部分的 Web 用戶端 (例如網頁瀏覽器) 會強制執行每個 Cookie 的大小上限、Cookie 總數或這兩者的限制。</span><span class="sxs-lookup"><span data-stu-id="31ff6-300">Most web clients (such as web browsers) enforce limits on the maximum size of each cookie, the total number of cookies, or both.</span></span> <span data-ttu-id="31ff6-301">使用 Cookie TempData 提供者時，請確認應用程式不會超過這些限制。</span><span class="sxs-lookup"><span data-stu-id="31ff6-301">When using the cookie TempData provider, verify the app won't exceed these limits.</span></span> <span data-ttu-id="31ff6-302">請考慮資料的大小總計。</span><span class="sxs-lookup"><span data-stu-id="31ff6-302">Consider the total size of the data.</span></span> <span data-ttu-id="31ff6-303">請考慮因為加密和區塊處理而增加的 Cookie 大小。</span><span class="sxs-lookup"><span data-stu-id="31ff6-303">Account for increases in cookie size due to encryption and chunking.</span></span>

### <a name="configure-the-tempdata-provider"></a><span data-ttu-id="31ff6-304">設定 TempData 提供者</span><span class="sxs-lookup"><span data-stu-id="31ff6-304">Configure the TempData provider</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="31ff6-305">預設會啟用 Cookie 架構 TempData 提供者。</span><span class="sxs-lookup"><span data-stu-id="31ff6-305">The cookie-based TempData provider is enabled by default.</span></span>

<span data-ttu-id="31ff6-306">若要啟用以工作階段為基礎的 TempData 提供者，請使用 [AddSessionStateTempDataProvider](/dotnet/api/microsoft.extensions.dependencyinjection.mvcviewfeaturesmvcbuilderextensions.addsessionstatetempdataprovider) 擴充方法：</span><span class="sxs-lookup"><span data-stu-id="31ff6-306">To enable the session-based TempData provider, use the [AddSessionStateTempDataProvider](/dotnet/api/microsoft.extensions.dependencyinjection.mvcviewfeaturesmvcbuilderextensions.addsessionstatetempdataprovider) extension method:</span></span>

[!code-csharp[](app-state/samples_snapshot_2/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13,32)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="31ff6-307">下列 `Startup` 類別程式碼會設定工作階段架構 TempData 提供者：</span><span class="sxs-lookup"><span data-stu-id="31ff6-307">The following `Startup` class code configures the session-based TempData provider:</span></span>

[!code-csharp[](app-state/samples_snapshot_2/1.x/SessionSample/Startup.cs?name=snippet1&highlight=4,9)]

::: moniker-end

<span data-ttu-id="31ff6-308">中介軟體的順序很重要。</span><span class="sxs-lookup"><span data-stu-id="31ff6-308">The order of middleware is important.</span></span> <span data-ttu-id="31ff6-309">在上述範例中，如果在 `UseMvc` 之後叫用 `UseSession`，則會發生 `InvalidOperationException`　例外狀況。</span><span class="sxs-lookup"><span data-stu-id="31ff6-309">In the preceding example, an `InvalidOperationException` exception occurs when `UseSession` is invoked after `UseMvc`.</span></span> <span data-ttu-id="31ff6-310">如需詳細資訊，請參閱[中介軟體順序](xref:fundamentals/middleware/index#ordering)。</span><span class="sxs-lookup"><span data-stu-id="31ff6-310">For more information, see [Middleware Ordering](xref:fundamentals/middleware/index#ordering).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="31ff6-311">如果目標為 .NET Framework 且使用以工作階段為基礎的 TempData 提供者，請將 [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) 套件新增至專案。</span><span class="sxs-lookup"><span data-stu-id="31ff6-311">If targeting .NET Framework and using the session-based TempData provider, add the [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) package to the project.</span></span>

## <a name="query-strings"></a><span data-ttu-id="31ff6-312">查詢字串</span><span class="sxs-lookup"><span data-stu-id="31ff6-312">Query strings</span></span>

<span data-ttu-id="31ff6-313">可以將數量有限的資料從某個要求傳遞到另一個要求，方法是將其新增至新要求的查詢字串。</span><span class="sxs-lookup"><span data-stu-id="31ff6-313">A limited amount of data can be passed from one request to another by adding it to the new request's query string.</span></span> <span data-ttu-id="31ff6-314">這對於以持續方式擷取狀態很有用，可讓內嵌狀態的連結透過電子郵件或社交網路共用。</span><span class="sxs-lookup"><span data-stu-id="31ff6-314">This is useful for capturing state in a persistent manner that allows links with embedded state to be shared through email or social networks.</span></span> <span data-ttu-id="31ff6-315">因為 URL 查詢字串為公用，所以請絕對不要使用查詢字串來處理敏感性資料。</span><span class="sxs-lookup"><span data-stu-id="31ff6-315">Because URL query strings are public, never use query strings for sensitive data.</span></span>

<span data-ttu-id="31ff6-316">除了非預期的共用之外，在查詢字串中包括資料還可能會為[跨網站偽造要求 (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) 攻擊創造機會，這些攻擊可能會誘騙使用者在驗證時瀏覽惡意網站。</span><span class="sxs-lookup"><span data-stu-id="31ff6-316">In addition to unintended sharing, including data in query strings can create opportunities for [Cross-Site Request Forgery (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) attacks, which can trick users into visiting malicious sites while authenticated.</span></span> <span data-ttu-id="31ff6-317">然後，攻擊者可能會竊取應用程式中的使用者資料，或代表使用者採取惡意動作。</span><span class="sxs-lookup"><span data-stu-id="31ff6-317">Attackers can then steal user data from the app or take malicious actions on behalf of the user.</span></span> <span data-ttu-id="31ff6-318">任何保留的應用程式或工作階段狀態必須防範 CSRF 攻擊。</span><span class="sxs-lookup"><span data-stu-id="31ff6-318">Any preserved app or session state must protect against CSRF attacks.</span></span> <span data-ttu-id="31ff6-319">如需詳細資訊，請參閱[防止跨站台要求偽造 (XSRF/CSRF) 攻擊](xref:security/anti-request-forgery)。</span><span class="sxs-lookup"><span data-stu-id="31ff6-319">For more information, see [Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks](xref:security/anti-request-forgery).</span></span>

## <a name="hidden-fields"></a><span data-ttu-id="31ff6-320">隱藏欄位</span><span class="sxs-lookup"><span data-stu-id="31ff6-320">Hidden fields</span></span>

<span data-ttu-id="31ff6-321">資料可以儲存在隱藏的表單欄位，並貼回下一個要求。</span><span class="sxs-lookup"><span data-stu-id="31ff6-321">Data can be saved in hidden form fields and posted back on the next request.</span></span> <span data-ttu-id="31ff6-322">這在多頁表單中很常見。</span><span class="sxs-lookup"><span data-stu-id="31ff6-322">This is common in multi-page forms.</span></span> <span data-ttu-id="31ff6-323">因為用戶端可能會竄改資料，所以應用程式必須一律重新驗證存放在隱藏欄位中的資料。</span><span class="sxs-lookup"><span data-stu-id="31ff6-323">Because the client can potentially tamper with the data, the app must always revalidate the data stored in hidden fields.</span></span>

## <a name="httpcontextitems"></a><span data-ttu-id="31ff6-324">HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="31ff6-324">HttpContext.Items</span></span>

<span data-ttu-id="31ff6-325">[HttpContext.Items](/dotnet/api/microsoft.aspnetcore.http.httpcontext.items) 集合用來在處理單一要求時存放資料。</span><span class="sxs-lookup"><span data-stu-id="31ff6-325">The [HttpContext.Items](/dotnet/api/microsoft.aspnetcore.http.httpcontext.items) collection is used to store data while processing a single request.</span></span> <span data-ttu-id="31ff6-326">集合的內容會在每個要求處理之後捨棄。</span><span class="sxs-lookup"><span data-stu-id="31ff6-326">The collection's contents are discarded after a request is processed.</span></span> <span data-ttu-id="31ff6-327">當元件或中介軟體在要求期間的不同時間點運作，而且沒有可傳遞參數的直接方式時，`Items` 集合經常用來允許元件或中介軟體進行通訊。</span><span class="sxs-lookup"><span data-stu-id="31ff6-327">The `Items` collection is often used to allow components or middleware to communicate when they operate at different points in time during a request and have no direct way to pass parameters.</span></span>

<span data-ttu-id="31ff6-328">在下列範例中，[中介軟體](xref:fundamentals/middleware/index)會將 `isVerified` 新增至 `Items` 集合。</span><span class="sxs-lookup"><span data-stu-id="31ff6-328">In the following example, [middleware](xref:fundamentals/middleware/index) adds `isVerified` to the `Items` collection.</span></span>

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

<span data-ttu-id="31ff6-329">稍後在管線中，另一個中介軟體可以存取 `isVerified` 的值：</span><span class="sxs-lookup"><span data-stu-id="31ff6-329">Later in the pipeline, another middleware can access the value of `isVerified`:</span></span>

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync($"Verified: {context.Items["isVerified"]}");
});
```

<span data-ttu-id="31ff6-330">如果是只由單一應用程式使用的中介軟體，可接受 `string` 索引鍵。</span><span class="sxs-lookup"><span data-stu-id="31ff6-330">For middleware that's only used by a single app, `string` keys are acceptable.</span></span> <span data-ttu-id="31ff6-331">應用程式執行個體之間共用的中介軟體，應該使用唯一的物件索引鍵，來避免索引鍵衝突。</span><span class="sxs-lookup"><span data-stu-id="31ff6-331">Middleware shared between app instances should use unique object keys to avoid key collisions.</span></span> <span data-ttu-id="31ff6-332">下列範例示範如何使用中介軟體類別中定義的唯一物件索引鍵：</span><span class="sxs-lookup"><span data-stu-id="31ff6-332">The following example shows how to use a unique object key defined in a middleware class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=4,13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=5,14)]

::: moniker-end

<span data-ttu-id="31ff6-333">其他程式碼可以使用中介軟體類別所公開的索引鍵，來存取 `HttpContext.Items` 中儲存的值：</span><span class="sxs-lookup"><span data-stu-id="31ff6-333">Other code can access the value stored in `HttpContext.Items` using the key exposed by the middleware class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet3)]

::: moniker-end

<span data-ttu-id="31ff6-334">此方法也有在程式碼中消除使用索引鍵字串的優點。</span><span class="sxs-lookup"><span data-stu-id="31ff6-334">This approach also has the advantage of eliminating the use of key strings in the code.</span></span>

## <a name="cache"></a><span data-ttu-id="31ff6-335">快取</span><span class="sxs-lookup"><span data-stu-id="31ff6-335">Cache</span></span>

<span data-ttu-id="31ff6-336">快取是儲存和擷取資料的有效方式。</span><span class="sxs-lookup"><span data-stu-id="31ff6-336">Caching is an efficient way to store and retrieve data.</span></span> <span data-ttu-id="31ff6-337">應用程式可以控制快取項目的存留期。</span><span class="sxs-lookup"><span data-stu-id="31ff6-337">The app can control the lifetime of cached items.</span></span>

<span data-ttu-id="31ff6-338">快取的資料未與特定要求、使用者或工作階段建立關聯。</span><span class="sxs-lookup"><span data-stu-id="31ff6-338">Cached data isn't associated with a specific request, user, or session.</span></span> <span data-ttu-id="31ff6-339">**請小心不要快取可能由其他使用者要求所擷取的特定使用者資料。**</span><span class="sxs-lookup"><span data-stu-id="31ff6-339">**Be careful not to cache user-specific data that may be retrieved by other users' requests.**</span></span>

<span data-ttu-id="31ff6-340">如需詳細資訊，請參閱[快取回應](xref:performance/caching/index)主題。</span><span class="sxs-lookup"><span data-stu-id="31ff6-340">For more information, see the [Cache responses](xref:performance/caching/index) topic.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="31ff6-341">相依性插入</span><span class="sxs-lookup"><span data-stu-id="31ff6-341">Dependency Injection</span></span>

<span data-ttu-id="31ff6-342">請使用[相依性插入](xref:fundamentals/dependency-injection)將資料提供給所有使用者：</span><span class="sxs-lookup"><span data-stu-id="31ff6-342">Use [Dependency Injection](xref:fundamentals/dependency-injection) to make data available to all users:</span></span>

1. <span data-ttu-id="31ff6-343">定義包含資料的服務。</span><span class="sxs-lookup"><span data-stu-id="31ff6-343">Define a service containing the data.</span></span> <span data-ttu-id="31ff6-344">例如，已定義名為 `MyAppData` 的類別：</span><span class="sxs-lookup"><span data-stu-id="31ff6-344">For example, a class named `MyAppData` is defined:</span></span>

    ```csharp
    public class MyAppData
    {
        // Declare properties and methods
    }
    ```

2. <span data-ttu-id="31ff6-345">將服務類別新增至 `Startup.ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="31ff6-345">Add the service class to `Startup.ConfigureServices`:</span></span>

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddSingleton<MyAppData>();
    }
    ```

3. <span data-ttu-id="31ff6-346">取用資料服務類別：</span><span class="sxs-lookup"><span data-stu-id="31ff6-346">Consume the data service class:</span></span>

    ::: moniker range=">= aspnetcore-2.0"

    ```csharp
    public class IndexModel : PageModel
    {
        public IndexModel(MyAppData myService)
        {
            // Do something with the service
            //    Examples: Read data, store in a field or property
        }
    }
    ```

    ::: moniker-end

    ::: moniker range="< aspnetcore-2.0"

    ```csharp
    public class HomeController : Controller
    {
        public HomeController(MyAppData myService)
        {
            // Do something with the service
            //    Examples: Read data, store in a field or property
        }
    }
    ```

    ::: moniker-end

## <a name="common-errors"></a><span data-ttu-id="31ff6-347">常見的錯誤</span><span class="sxs-lookup"><span data-stu-id="31ff6-347">Common errors</span></span>

* <span data-ttu-id="31ff6-348">「嘗試啟動 'Microsoft.AspNetCore.Session.DistributedSessionStore' 時，無法解析類型 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' 。」</span><span class="sxs-lookup"><span data-stu-id="31ff6-348">"Unable to resolve service for type 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' while attempting to activate 'Microsoft.AspNetCore.Session.DistributedSessionStore'."</span></span>

  <span data-ttu-id="31ff6-349">這種情形通常是因為無法設定至少一個 `IDistributedCache` 實作。</span><span class="sxs-lookup"><span data-stu-id="31ff6-349">This is usually caused by failing to configure at least one `IDistributedCache` implementation.</span></span> <span data-ttu-id="31ff6-350">如需詳細資訊，請參閱[使用分散式快取](xref:performance/caching/distributed)和[在記憶體中快取](xref:performance/caching/memory)。</span><span class="sxs-lookup"><span data-stu-id="31ff6-350">For more information, see [Work with a distributed cache](xref:performance/caching/distributed) and [Cache in-memory](xref:performance/caching/memory).</span></span>

* <span data-ttu-id="31ff6-351">如果工作階段中介軟體無法持續存在於工作階段 (例如，備份存放區無法使用)，中介軟體會記錄例外狀況，而要求會照常繼續進行。</span><span class="sxs-lookup"><span data-stu-id="31ff6-351">In the event that the session middleware fails to persist a session (for example, if the backing store isn't available), the middleware logs the exception and the request continues normally.</span></span> <span data-ttu-id="31ff6-352">這會導致無法預期的行為。</span><span class="sxs-lookup"><span data-stu-id="31ff6-352">This leads to unpredictable behavior.</span></span>

  <span data-ttu-id="31ff6-353">例如，使用者在工作階段中存放購物車。</span><span class="sxs-lookup"><span data-stu-id="31ff6-353">For example, a user stores a shopping cart in session.</span></span> <span data-ttu-id="31ff6-354">使用者在購物車新增一個項目，但認可失敗。</span><span class="sxs-lookup"><span data-stu-id="31ff6-354">The user adds an item to the cart but the commit fails.</span></span> <span data-ttu-id="31ff6-355">應用程式未察覺到失敗，因此它向使用者報告項目已新增至購物車，但這並不正確。</span><span class="sxs-lookup"><span data-stu-id="31ff6-355">The app doesn't know about the failure so it reports to the user that the item was added to their cart, which isn't true.</span></span>

  <span data-ttu-id="31ff6-356">檢查是否有錯誤的建議方法是，當應用程式完成寫入至工作階段後，從應用程式程式碼呼叫 `await feature.Session.CommitAsync();`。</span><span class="sxs-lookup"><span data-stu-id="31ff6-356">The recommended approach to check for errors is to call `await feature.Session.CommitAsync();` from app code when the app is done writing to the session.</span></span> <span data-ttu-id="31ff6-357">備份存放區無法使用時，`CommitAsync` 會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="31ff6-357">`CommitAsync` throws an exception if the backing store is unavailable.</span></span> <span data-ttu-id="31ff6-358">如果 `CommitAsync` 失敗，應用程式可以處理例外狀況。</span><span class="sxs-lookup"><span data-stu-id="31ff6-358">If `CommitAsync` fails, the app can process the exception.</span></span> <span data-ttu-id="31ff6-359">`LoadAsync` 在無法使用資料存放區的相同情況下會擲回。</span><span class="sxs-lookup"><span data-stu-id="31ff6-359">`LoadAsync` throws under the same conditions where the data store is unavailable.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="31ff6-360">其他資源</span><span class="sxs-lookup"><span data-stu-id="31ff6-360">Additional resources</span></span>

<xref:host-and-deploy/web-farm>
