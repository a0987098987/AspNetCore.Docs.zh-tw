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
<a name="authentication-and-authorization-in-aspnet-web-api"></a><span data-ttu-id="36308-103">ASP.NET Web API 中驗證和授權</span><span class="sxs-lookup"><span data-stu-id="36308-103">Authentication and Authorization in ASP.NET Web API</span></span>
====================
<span data-ttu-id="36308-104">藉由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="36308-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="36308-105">您已建立的 web API，但現在您想要控制其存取權。</span><span class="sxs-lookup"><span data-stu-id="36308-105">You've created a web API, but now you want to control access to it.</span></span> <span data-ttu-id="36308-106">在本系列的文章中，我們將探討一些選項來從未經授權的使用者保護的 web API。</span><span class="sxs-lookup"><span data-stu-id="36308-106">In this series of articles, we'll look at some options for securing a web API from unauthorized users.</span></span> <span data-ttu-id="36308-107">此系列將會涵蓋驗證和授權。</span><span class="sxs-lookup"><span data-stu-id="36308-107">This series will cover both authentication and authorization.</span></span>

- <span data-ttu-id="36308-108">*驗證*了解使用者的身分識別。</span><span class="sxs-lookup"><span data-stu-id="36308-108">*Authentication* is knowing the identity of the user.</span></span> <span data-ttu-id="36308-109">例如，Alice 登入時她的使用者名稱和密碼，而伺服器會使用密碼驗證 Alice。</span><span class="sxs-lookup"><span data-stu-id="36308-109">For example, Alice logs in with her username and password, and the server uses the password to authenticate Alice.</span></span>
- <span data-ttu-id="36308-110">*授權*決定是否允許使用者執行的動作。</span><span class="sxs-lookup"><span data-stu-id="36308-110">*Authorization* is deciding whether a user is allowed to perform an action.</span></span> <span data-ttu-id="36308-111">例如，Alice 已取得資源，但不是會建立資源的權限。</span><span class="sxs-lookup"><span data-stu-id="36308-111">For example, Alice has permission to get a resource but not create a resource.</span></span>

<span data-ttu-id="36308-112">數列中的第一篇文章提供 ASP.NET Web API 中的驗證和授權的一般概觀。</span><span class="sxs-lookup"><span data-stu-id="36308-112">The first article in the series gives a general overview of authentication and authorization in ASP.NET Web API.</span></span> <span data-ttu-id="36308-113">其他主題會說明常見驗證案例，Web api。</span><span class="sxs-lookup"><span data-stu-id="36308-113">Other topics describe common authentication scenarios for Web API.</span></span>

> [!NOTE]
> <span data-ttu-id="36308-114">檢閱這一系列，並提供寶貴意見的人感謝： Rick Anderson、 於 Levi Broderick、 Barry Dorrans、 Tom Dykstra、 Hongmei Ge、 David Matson、 Daniel Roth、 Tim Teebken。</span><span class="sxs-lookup"><span data-stu-id="36308-114">Thanks to the people who reviewed this series and provided valuable feedback: Rick Anderson, Levi Broderick, Barry Dorrans, Tom Dykstra, Hongmei Ge, David Matson, Daniel Roth, Tim Teebken.</span></span>


## <a name="authentication"></a><span data-ttu-id="36308-115">驗證</span><span class="sxs-lookup"><span data-stu-id="36308-115">Authentication</span></span>

<span data-ttu-id="36308-116">Web API 會假設該驗證會在主機中。</span><span class="sxs-lookup"><span data-stu-id="36308-116">Web API assumes that authentication happens in the host.</span></span> <span data-ttu-id="36308-117">適用於虛擬主機，主機會為 IIS 中，使用 HTTP 模組進行驗證。</span><span class="sxs-lookup"><span data-stu-id="36308-117">For web-hosting, the host is IIS, which uses HTTP modules for authentication.</span></span> <span data-ttu-id="36308-118">您可以將專案設定為使用任何內建在 IIS 或 ASP.NET 的驗證模組，或撰寫您自己的 HTTP 模組，來執行自訂驗證。</span><span class="sxs-lookup"><span data-stu-id="36308-118">You can configure your project to use any of the authentication modules built in to IIS or ASP.NET, or write your own HTTP module to perform custom authentication.</span></span>

<span data-ttu-id="36308-119">當主應用程式驗證使用者時，它會建立*主體*，即[IPrincipal](https://msdn.microsoft.com/library/System.Security.Principal.IPrincipal.aspx)物件，表示執行程式碼的安全性內容。</span><span class="sxs-lookup"><span data-stu-id="36308-119">When the host authenticates the user, it creates a *principal*, which is an [IPrincipal](https://msdn.microsoft.com/library/System.Security.Principal.IPrincipal.aspx) object that represents the security context under which code is running.</span></span> <span data-ttu-id="36308-120">主機設定將主體附加至目前的執行緒**Thread.CurrentPrincipal**。</span><span class="sxs-lookup"><span data-stu-id="36308-120">The host attaches the principal to the current thread by setting **Thread.CurrentPrincipal**.</span></span> <span data-ttu-id="36308-121">主體包含相關聯**識別**物件，其中包含使用者的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="36308-121">The principal contains an associated **Identity** object that contains information about the user.</span></span> <span data-ttu-id="36308-122">如果使用者經過驗證， **Identity.IsAuthenticated**屬性會傳回 **，則為 true**。</span><span class="sxs-lookup"><span data-stu-id="36308-122">If the user is authenticated, the **Identity.IsAuthenticated** property returns **true**.</span></span> <span data-ttu-id="36308-123">匿名要求，如**IsAuthenticated**會傳回**false**。</span><span class="sxs-lookup"><span data-stu-id="36308-123">For anonymous requests, **IsAuthenticated** returns **false**.</span></span> <span data-ttu-id="36308-124">如需有關主體的詳細資訊，請參閱[角色為基礎的安全性](https://msdn.microsoft.com/library/shz8h065.aspx)。</span><span class="sxs-lookup"><span data-stu-id="36308-124">For more information about principals, see [Role-Based Security](https://msdn.microsoft.com/library/shz8h065.aspx).</span></span>

### <a name="http-message-handlers-for-authentication"></a><span data-ttu-id="36308-125">驗證的 HTTP 訊息處理常式</span><span class="sxs-lookup"><span data-stu-id="36308-125">HTTP Message Handlers for Authentication</span></span>

<span data-ttu-id="36308-126">而不是使用主應用程式進行驗證，您可以將驗證邏輯至放[HTTP 訊息處理常式](../advanced/http-message-handlers.md)。</span><span class="sxs-lookup"><span data-stu-id="36308-126">Instead of using the host for authentication, you can put authentication logic into an [HTTP message handler](../advanced/http-message-handlers.md).</span></span> <span data-ttu-id="36308-127">在此情況下，訊息處理常式會檢查 HTTP 要求，並設定主體。</span><span class="sxs-lookup"><span data-stu-id="36308-127">In that case, the message handler examines the HTTP request and sets the principal.</span></span>

<span data-ttu-id="36308-128">何時應該使用訊息處理常式進行驗證？</span><span class="sxs-lookup"><span data-stu-id="36308-128">When should you use message handlers for authentication?</span></span> <span data-ttu-id="36308-129">以下是一些利弊：</span><span class="sxs-lookup"><span data-stu-id="36308-129">Here are some tradeoffs:</span></span>

- <span data-ttu-id="36308-130">HTTP 模組就會看到瀏覽 ASP.NET 管線的所有要求。</span><span class="sxs-lookup"><span data-stu-id="36308-130">An HTTP module sees all requests that go through the ASP.NET pipeline.</span></span> <span data-ttu-id="36308-131">訊息處理常式只會看到會路由傳送至 Web API 的要求。</span><span class="sxs-lookup"><span data-stu-id="36308-131">A message handler only sees requests that are routed to Web API.</span></span>
- <span data-ttu-id="36308-132">您可以設定每個路由訊息處理常式，讓您將驗證配置套用至特定的路由。</span><span class="sxs-lookup"><span data-stu-id="36308-132">You can set per-route message handlers, which lets you apply an authentication scheme to a specific route.</span></span>
- <span data-ttu-id="36308-133">HTTP 模組是 IIS 專用的。</span><span class="sxs-lookup"><span data-stu-id="36308-133">HTTP modules are specific to IIS.</span></span> <span data-ttu-id="36308-134">訊息處理常式會是主機無從驗證，因此它們可以搭配 web 裝載及自我裝載。</span><span class="sxs-lookup"><span data-stu-id="36308-134">Message handlers are host-agnostic, so they can be used with both web-hosting and self-hosting.</span></span>
- <span data-ttu-id="36308-135">HTTP 模組是由 IIS 記錄、 稽核等等所參與。</span><span class="sxs-lookup"><span data-stu-id="36308-135">HTTP modules participate in IIS logging, auditing, and so on.</span></span>
- <span data-ttu-id="36308-136">HTTP 模組執行稍早在管線中。</span><span class="sxs-lookup"><span data-stu-id="36308-136">HTTP modules run earlier in the pipeline.</span></span> <span data-ttu-id="36308-137">如果您處理驗證的訊息處理常式中，主體不會取得設定的處理常式執行之前。</span><span class="sxs-lookup"><span data-stu-id="36308-137">If you handle authentication in a message handler, the principal does not get set until the handler runs.</span></span> <span data-ttu-id="36308-138">此外，主體會還原回先前的主體時在回應傳出後的訊息處理常式。</span><span class="sxs-lookup"><span data-stu-id="36308-138">Moreover, the principal reverts back to the previous principal when the response leaves the message handler.</span></span>

<span data-ttu-id="36308-139">一般而言，如果您不需要支援自我裝載，HTTP 模組就會是較好的選擇。</span><span class="sxs-lookup"><span data-stu-id="36308-139">Generally, if you don't need to support self-hosting, an HTTP module is a better option.</span></span> <span data-ttu-id="36308-140">如果您需要支援自我裝載，請考慮訊息處理常式。</span><span class="sxs-lookup"><span data-stu-id="36308-140">If you need to support self-hosting, consider a message handler.</span></span>

### <a name="setting-the-principal"></a><span data-ttu-id="36308-141">設定主體</span><span class="sxs-lookup"><span data-stu-id="36308-141">Setting the Principal</span></span>

<span data-ttu-id="36308-142">如果您的應用程式會執行任何自訂驗證邏輯，您必須設定主體的兩個地方：</span><span class="sxs-lookup"><span data-stu-id="36308-142">If your application performs any custom authentication logic, you must set the principal on two places:</span></span>

- <span data-ttu-id="36308-143">**Thread.CurrentPrincipal**。</span><span class="sxs-lookup"><span data-stu-id="36308-143">**Thread.CurrentPrincipal**.</span></span> <span data-ttu-id="36308-144">這個屬性是設定執行緒主體，在.NET 中的標準方式。</span><span class="sxs-lookup"><span data-stu-id="36308-144">This property is the standard way to set the thread's principal in .NET.</span></span>
- <span data-ttu-id="36308-145">**HttpContext.Current.User**。</span><span class="sxs-lookup"><span data-stu-id="36308-145">**HttpContext.Current.User**.</span></span> <span data-ttu-id="36308-146">這個屬性是 ASP.NET 特定的項目。</span><span class="sxs-lookup"><span data-stu-id="36308-146">This property is specific to ASP.NET.</span></span>

<span data-ttu-id="36308-147">下列程式碼示範如何設定主體：</span><span class="sxs-lookup"><span data-stu-id="36308-147">The following code shows how to set the principal:</span></span>

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="36308-148">適用於虛擬主機，您必須設定主體在兩個位置中;否則您可能會不一致的安全性內容。</span><span class="sxs-lookup"><span data-stu-id="36308-148">For web-hosting, you must set the principal in both places; otherwise the security context may become inconsistent.</span></span> <span data-ttu-id="36308-149">自我裝載，但是**HttpContext.Current**為 null。</span><span class="sxs-lookup"><span data-stu-id="36308-149">For self-hosting, however, **HttpContext.Current** is null.</span></span> <span data-ttu-id="36308-150">若要確保您的程式碼與主機無關，因此，檢查是否有 null 指派之前**HttpContext.Current**，如所示。</span><span class="sxs-lookup"><span data-stu-id="36308-150">To ensure your code is host-agnostic, therefore, check for null before assigning to **HttpContext.Current**, as shown.</span></span>

## <a name="authorization"></a><span data-ttu-id="36308-151">授權</span><span class="sxs-lookup"><span data-stu-id="36308-151">Authorization</span></span>

<span data-ttu-id="36308-152">授權會在管線中，稍後更靠近控制站。</span><span class="sxs-lookup"><span data-stu-id="36308-152">Authorization happens later in the pipeline, closer to the controller.</span></span> <span data-ttu-id="36308-153">可讓您進行更細微的選項，當您授與資源的存取權。</span><span class="sxs-lookup"><span data-stu-id="36308-153">That lets you make more granular choices when you grant access to resources.</span></span>

- <span data-ttu-id="36308-154">*授權篩選條件*控制器動作之前執行。</span><span class="sxs-lookup"><span data-stu-id="36308-154">*Authorization filters* run before the controller action.</span></span> <span data-ttu-id="36308-155">如果要求未獲授權，篩選條件會傳回錯誤回應，並不會叫用動作。</span><span class="sxs-lookup"><span data-stu-id="36308-155">If the request is not authorized, the filter returns an error response, and the action is not invoked.</span></span>
- <span data-ttu-id="36308-156">控制器動作，在中，您可以取得從目前的主體**ApiController.User**屬性。</span><span class="sxs-lookup"><span data-stu-id="36308-156">Within a controller action, you can get the current principal from the **ApiController.User** property.</span></span> <span data-ttu-id="36308-157">例如，您可能會篩選資源清單，根據使用者名稱，傳回只有該使用者所屬的資源。</span><span class="sxs-lookup"><span data-stu-id="36308-157">For example, you might filter a list of resources based on the user name, returning only those resources that belong to that user.</span></span>

![](authentication-and-authorization-in-aspnet-web-api/_static/image1.png)

<a id="auth3"></a>
### <a name="using-the-authorize-attribute"></a><span data-ttu-id="36308-158">使用 [授權] 屬性</span><span class="sxs-lookup"><span data-stu-id="36308-158">Using the [Authorize] Attribute</span></span>

<span data-ttu-id="36308-159">Web API 提供的內建的授權篩選條件， [AuthorizeAttribute](https://msdn.microsoft.com/library/system.web.http.authorizeattribute.aspx)。</span><span class="sxs-lookup"><span data-stu-id="36308-159">Web API provides a built-in authorization filter, [AuthorizeAttribute](https://msdn.microsoft.com/library/system.web.http.authorizeattribute.aspx).</span></span> <span data-ttu-id="36308-160">此篩選條件會檢查使用者是否經過驗證。</span><span class="sxs-lookup"><span data-stu-id="36308-160">This filter checks whether the user is authenticated.</span></span> <span data-ttu-id="36308-161">如果沒有，它會傳回 HTTP 狀態碼 401 （未經授權），而不叫用動作。</span><span class="sxs-lookup"><span data-stu-id="36308-161">If not, it returns HTTP status code 401 (Unauthorized), without invoking the action.</span></span>

<span data-ttu-id="36308-162">您可以套用全域，在控制器層級，或個別動作的層級的篩選器。</span><span class="sxs-lookup"><span data-stu-id="36308-162">You can apply the filter globally, at the controller level, or at the level of individual actions.</span></span>

<span data-ttu-id="36308-163">**全域**： 若要限制每個 Web API 控制器的存取權，新增**AuthorizeAttribute**全域篩選器清單的篩選：</span><span class="sxs-lookup"><span data-stu-id="36308-163">**Globally**: To restrict access for every Web API controller, add the **AuthorizeAttribute** filter to the global filter list:</span></span>

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="36308-164">**控制器**： 若要限制特定控制器的存取，將篩選做為屬性新增至控制器：</span><span class="sxs-lookup"><span data-stu-id="36308-164">**Controller**: To restrict access for a specific controller, add the filter as an attribute to the controller:</span></span>

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="36308-165">**動作**： 若要限制特定動作的存取，請將屬性新增至動作方法：</span><span class="sxs-lookup"><span data-stu-id="36308-165">**Action**: To restrict access for specific actions, add the attribute to the action method:</span></span>

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample4.cs)]

<span data-ttu-id="36308-166">或者，您可以限制控制器，然後允許匿名存取特定的動作，使用`[AllowAnonymous]`屬性。</span><span class="sxs-lookup"><span data-stu-id="36308-166">Alternatively, you can restrict the controller and then allow anonymous access to specific actions, by using the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="36308-167">在下列範例中，`Post`方法是限制，但`Get`方法允許匿名存取。</span><span class="sxs-lookup"><span data-stu-id="36308-167">In the following example, the `Post` method is restricted, but the `Get` method allows anonymous access.</span></span>

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="36308-168">在上一個範例中，篩選條件允許任何通過驗證的使用者存取受限制的方法;只有匿名使用者會保留。您也可以限制特定使用者或特定角色中使用者的存取：</span><span class="sxs-lookup"><span data-stu-id="36308-168">In the previous examples, the filter allows any authenticated user to access the restricted methods; only anonymous users are kept out. You can also limit access to specific users or to users in specific roles:</span></span>

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample6.cs)]

> [!NOTE]
> <span data-ttu-id="36308-169">**AuthorizeAttribute** Web API 控制器的篩選器位於**System.Web.Http**命名空間。</span><span class="sxs-lookup"><span data-stu-id="36308-169">The **AuthorizeAttribute** filter for Web API controllers is located in the **System.Web.Http** namespace.</span></span> <span data-ttu-id="36308-170">沒有類似的篩選條件中的 MVC 控制器**System.Web.Mvc**命名空間，與 Web API 控制器不相容。</span><span class="sxs-lookup"><span data-stu-id="36308-170">There is a similar filter for MVC controllers in the **System.Web.Mvc** namespace, which is not compatible with Web API controllers.</span></span>


### <a name="custom-authorization-filters"></a><span data-ttu-id="36308-171">自訂授權篩選條件</span><span class="sxs-lookup"><span data-stu-id="36308-171">Custom Authorization Filters</span></span>

<span data-ttu-id="36308-172">若要撰寫自訂授權篩選條件，衍生自其中一種類型：</span><span class="sxs-lookup"><span data-stu-id="36308-172">To write a custom authorization filter, derive from one of these types:</span></span>

- <span data-ttu-id="36308-173">**AuthorizeAttribute**。</span><span class="sxs-lookup"><span data-stu-id="36308-173">**AuthorizeAttribute**.</span></span> <span data-ttu-id="36308-174">擴充此類別來執行目前的使用者和使用者的角色為基礎的授權邏輯。</span><span class="sxs-lookup"><span data-stu-id="36308-174">Extend this class to perform authorization logic based on the current user and the user's roles.</span></span>
- <span data-ttu-id="36308-175">**AuthorizationFilterAttribute**。</span><span class="sxs-lookup"><span data-stu-id="36308-175">**AuthorizationFilterAttribute**.</span></span> <span data-ttu-id="36308-176">擴充此類別來執行目前的使用者或角色不一定是以同步的授權邏輯。</span><span class="sxs-lookup"><span data-stu-id="36308-176">Extend this class to perform synchronous authorization logic that is not necessarily based on the current user or role.</span></span>
- <span data-ttu-id="36308-177">**IAuthorizationFilter**。</span><span class="sxs-lookup"><span data-stu-id="36308-177">**IAuthorizationFilter**.</span></span> <span data-ttu-id="36308-178">實作這個介面來執行非同步的授權邏輯;例如，如果您的授權邏輯會非同步的 I/O 或網路呼叫。</span><span class="sxs-lookup"><span data-stu-id="36308-178">Implement this interface to perform asynchronous authorization logic; for example, if your authorization logic makes asynchronous I/O or network calls.</span></span> <span data-ttu-id="36308-179">(如果您授權邏輯 CPU 繫結，則是衍生自的工作變得更容易**AuthorizationFilterAttribute**，因為您不需要寫入為非同步方法。)</span><span class="sxs-lookup"><span data-stu-id="36308-179">(If your authorization logic is CPU-bound, it is simpler to derive from **AuthorizationFilterAttribute**, because then you don't need to write an asynchronous method.)</span></span>

<span data-ttu-id="36308-180">下圖顯示的類別階層**AuthorizeAttribute**類別。</span><span class="sxs-lookup"><span data-stu-id="36308-180">The following diagram shows the class hierarchy for the **AuthorizeAttribute** class.</span></span>

![](authentication-and-authorization-in-aspnet-web-api/_static/image2.png)

### <a name="authorization-inside-a-controller-action"></a><span data-ttu-id="36308-181">在控制器動作授權</span><span class="sxs-lookup"><span data-stu-id="36308-181">Authorization Inside a Controller Action</span></span>

<span data-ttu-id="36308-182">在某些情況下，您可能會允許繼續進行，但變更行為的主體為基礎的要求。</span><span class="sxs-lookup"><span data-stu-id="36308-182">In some cases, you might allow a request to proceed, but change the behavior based on the principal.</span></span> <span data-ttu-id="36308-183">比方說，您所傳回的資訊可能會變更，視使用者的角色而定。</span><span class="sxs-lookup"><span data-stu-id="36308-183">For example, the information that you return might change depending on the user's role.</span></span> <span data-ttu-id="36308-184">在控制器方法中，您可以取得從目前的主體**ApiController.User**屬性。</span><span class="sxs-lookup"><span data-stu-id="36308-184">Within a controller method, you can get the current principal from the **ApiController.User** property.</span></span>

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample7.cs)]
