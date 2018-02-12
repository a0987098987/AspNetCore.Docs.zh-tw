---
uid: web-api/overview/security/authentication-filters
title: "ASP.NET Web API 2 中的驗證篩選條件 |Microsoft 文件"
author: MikeWasson
description: "驗證篩選條件是可驗證的 HTTP 要求的元件。 Web API 2 和 MVC 5 都支援的驗證篩選條件，但他們稍有不同..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/25/2014
ms.topic: article
ms.assetid: b9882e53-b3ca-4def-89b0-322846973ccb
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/authentication-filters
msc.type: authoredcontent
ms.openlocfilehash: 16e451f52799625983368bc938091eff47019b52
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/12/2018
---
<a name="authentication-filters-in-aspnet-web-api-2"></a><span data-ttu-id="49bf4-104">ASP.NET Web API 2 中的驗證篩選條件</span><span class="sxs-lookup"><span data-stu-id="49bf4-104">Authentication Filters in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="49bf4-105">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="49bf4-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="49bf4-106">驗證篩選條件是可驗證的 HTTP 要求的元件。</span><span class="sxs-lookup"><span data-stu-id="49bf4-106">An authentication filter is a component that authenticates an HTTP request.</span></span> <span data-ttu-id="49bf4-107">Web API 2 和 MVC 5 都支援的驗證篩選條件，但大部分的篩選器介面的命名慣例而稍有不同。</span><span class="sxs-lookup"><span data-stu-id="49bf4-107">Web API 2 and MVC 5 both support authentication filters, but they differ slightly, mostly in the naming conventions for the filter interface.</span></span> <span data-ttu-id="49bf4-108">本主題描述 Web API 驗證篩選條件。</span><span class="sxs-lookup"><span data-stu-id="49bf4-108">This topic describes Web API authentication filters.</span></span>


<span data-ttu-id="49bf4-109">驗證篩選條件可讓您針對個別的控制器或動作中設定驗證配置。</span><span class="sxs-lookup"><span data-stu-id="49bf4-109">Authentication filters let you set an authentication scheme for individual controllers or actions.</span></span> <span data-ttu-id="49bf4-110">這樣一來，您的應用程式可支援不同的驗證機制的不同的 HTTP 資源。</span><span class="sxs-lookup"><span data-stu-id="49bf4-110">That way, your app can support different authentication mechanisms for different HTTP resources.</span></span>

<span data-ttu-id="49bf4-111">在本文中，我們將示範從程式碼[基本驗證](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt)範例上[http://aspnet.codeplex.com](http://aspnet.codeplex.com)。此範例會示範實作 HTTP 基本存取驗證配置 (RFC 2617) 的驗證篩選條件。</span><span class="sxs-lookup"><span data-stu-id="49bf4-111">In this article, I'll show code from the [Basic Authentication](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt) sample on [http://aspnet.codeplex.com](http://aspnet.codeplex.com). The sample shows an authentication filter that implements the HTTP Basic Access Authentication scheme (RFC 2617).</span></span> <span data-ttu-id="49bf4-112">篩選名為類別中實作`IdentityBasicAuthenticationAttribute`。</span><span class="sxs-lookup"><span data-stu-id="49bf4-112">The filter is implemented in a class named `IdentityBasicAuthenticationAttribute`.</span></span> <span data-ttu-id="49bf4-113">我不會顯示所有的程式碼範例中，只說明如何撰寫的驗證篩選條件的組件。</span><span class="sxs-lookup"><span data-stu-id="49bf4-113">I won't show all of the code from the sample, just the parts that illustrate how to write an authentication filter.</span></span>

## <a name="setting-an-authentication-filter"></a><span data-ttu-id="49bf4-114">設定的驗證篩選條件</span><span class="sxs-lookup"><span data-stu-id="49bf4-114">Setting an Authentication Filter</span></span>

<span data-ttu-id="49bf4-115">如同其他篩選器，驗證篩選條件可以是套用每個控制站，每個動作，或是全域所有的 Web API 控制器。</span><span class="sxs-lookup"><span data-stu-id="49bf4-115">Like other filters, authentication filters can be applied per-controller, per-action, or globally to all Web API controllers.</span></span>

<span data-ttu-id="49bf4-116">若要套用的驗證篩選條件的控制站，來裝飾控制器類別，以篩選條件屬性。</span><span class="sxs-lookup"><span data-stu-id="49bf4-116">To apply an authentication filter to a controller, decorate the controller class with the filter attribute.</span></span> <span data-ttu-id="49bf4-117">下列程式碼設定`[IdentityBasicAuthentication]`控制器類別，可讓基本驗證的所有控制器的動作篩選。</span><span class="sxs-lookup"><span data-stu-id="49bf4-117">The following code sets the `[IdentityBasicAuthentication]` filter on a controller class, which enables Basic Authentication for all of the controller's actions.</span></span>

[!code-csharp[Main](authentication-filters/samples/sample1.cs)]

<span data-ttu-id="49bf4-118">若要將篩選套用至一個動作，請使用篩選器裝飾的動作。</span><span class="sxs-lookup"><span data-stu-id="49bf4-118">To apply the filter to one action, decorate the action with the filter.</span></span> <span data-ttu-id="49bf4-119">下列程式碼設定`[IdentityBasicAuthentication]`在控制器上的篩選器`Post`方法。</span><span class="sxs-lookup"><span data-stu-id="49bf4-119">The following code sets the `[IdentityBasicAuthentication]` filter on the controller's `Post` method.</span></span>

[!code-csharp[Main](authentication-filters/samples/sample2.cs)]

<span data-ttu-id="49bf4-120">將篩選套用到所有的 Web API 控制器，將它加入**GlobalConfiguration.Filters**。</span><span class="sxs-lookup"><span data-stu-id="49bf4-120">To apply the filter to all Web API controllers, add it to **GlobalConfiguration.Filters**.</span></span>

[!code-csharp[Main](authentication-filters/samples/sample3.cs)]

## <a name="implementing-a-web-api-authentication-filter"></a><span data-ttu-id="49bf4-121">實作 Web API 驗證篩選條件</span><span class="sxs-lookup"><span data-stu-id="49bf4-121">Implementing a Web API Authentication Filter</span></span>

<span data-ttu-id="49bf4-122">在 Web API 驗證篩選條件會實作[System.Web.Http.Filters.IAuthenticationFilter](https://msdn.microsoft.com/library/system.web.http.filters.iauthenticationfilter.aspx)介面。</span><span class="sxs-lookup"><span data-stu-id="49bf4-122">In Web API, authentication filters implement the [System.Web.Http.Filters.IAuthenticationFilter](https://msdn.microsoft.com/library/system.web.http.filters.iauthenticationfilter.aspx) interface.</span></span> <span data-ttu-id="49bf4-123">它們也應該繼承自**System.Attribute**，以便套用做為屬性。</span><span class="sxs-lookup"><span data-stu-id="49bf4-123">They should also inherit from **System.Attribute**, in order to be applied as attributes.</span></span>

<span data-ttu-id="49bf4-124">**IAuthenticationFilter**介面有兩種方法：</span><span class="sxs-lookup"><span data-stu-id="49bf4-124">The **IAuthenticationFilter** interface has two methods:</span></span>

- <span data-ttu-id="49bf4-125">**AuthenticateAsync**藉由驗證認證，在要求中，驗證要求，如果有的話。</span><span class="sxs-lookup"><span data-stu-id="49bf4-125">**AuthenticateAsync** authenticates the request by validating credentials in the request, if present.</span></span>
- <span data-ttu-id="49bf4-126">**ChallengeAsync**將驗證挑戰新增至 HTTP 回應，如有需要。</span><span class="sxs-lookup"><span data-stu-id="49bf4-126">**ChallengeAsync** adds an authentication challenge to the HTTP response, if needed.</span></span>

<span data-ttu-id="49bf4-127">這些方法會對應至中定義的驗證流程[RFC 2612](http://tools.ietf.org/html/rfc2616)和[RFC 2617](http://tools.ietf.org/html/rfc2617):</span><span class="sxs-lookup"><span data-stu-id="49bf4-127">These methods correspond to the authentication flow defined in [RFC 2612](http://tools.ietf.org/html/rfc2616) and [RFC 2617](http://tools.ietf.org/html/rfc2617):</span></span>

1. <span data-ttu-id="49bf4-128">用戶端傳送授權標頭中的認證。</span><span class="sxs-lookup"><span data-stu-id="49bf4-128">The client sends credentials in the Authorization header.</span></span> <span data-ttu-id="49bf4-129">這通常發生在用戶端從伺服器接收 401 （未經授權） 回應之後。</span><span class="sxs-lookup"><span data-stu-id="49bf4-129">This typically happens after the client receives a 401 (Unauthorized) response from the server.</span></span> <span data-ttu-id="49bf4-130">不過，用戶端可以傳送認證與任何要求中，不只是取得 401 之後。</span><span class="sxs-lookup"><span data-stu-id="49bf4-130">However, a client can send credentials with any request, not just after getting a 401.</span></span>
2. <span data-ttu-id="49bf4-131">如果伺服器不接受認證，它會傳回 401 （未經授權） 的回應。</span><span class="sxs-lookup"><span data-stu-id="49bf4-131">If the server does not accept the credentials, it returns a 401 (Unauthorized) response.</span></span> <span data-ttu-id="49bf4-132">回應包括 Www-authenticate 標頭，其中包含一或多個挑戰。</span><span class="sxs-lookup"><span data-stu-id="49bf4-132">The response includes a Www-Authenticate header that contains one or more challenges.</span></span> <span data-ttu-id="49bf4-133">每個挑戰指定伺服器辨識驗證配置。</span><span class="sxs-lookup"><span data-stu-id="49bf4-133">Each challenge specifies an authentication scheme recognized by the server.</span></span>

<span data-ttu-id="49bf4-134">伺服器也可以從匿名要求傳回 401。</span><span class="sxs-lookup"><span data-stu-id="49bf4-134">The server can also return 401 from an anonymous request.</span></span> <span data-ttu-id="49bf4-135">事實上，也就是通常如何起始驗證程序：</span><span class="sxs-lookup"><span data-stu-id="49bf4-135">In fact, that's typically how the authentication process is initiated:</span></span>

1. <span data-ttu-id="49bf4-136">用戶端傳送的匿名要求。</span><span class="sxs-lookup"><span data-stu-id="49bf4-136">The client sends an anonymous request.</span></span>
2. <span data-ttu-id="49bf4-137">伺服器會傳回 401。</span><span class="sxs-lookup"><span data-stu-id="49bf4-137">The server returns 401.</span></span>
3. <span data-ttu-id="49bf4-138">用戶端重新傳送認證的要求。</span><span class="sxs-lookup"><span data-stu-id="49bf4-138">The clients resends the request with credentials.</span></span>

<span data-ttu-id="49bf4-139">此流程同時包含*驗證*和*授權*步驟。</span><span class="sxs-lookup"><span data-stu-id="49bf4-139">This flow includes both *authentication* and *authorization* steps.</span></span>

- <span data-ttu-id="49bf4-140">驗證證明用戶端的身分識別。</span><span class="sxs-lookup"><span data-stu-id="49bf4-140">Authentication proves the identity of the client.</span></span>
- <span data-ttu-id="49bf4-141">授權就會決定用戶端是否可以存取特定資源。</span><span class="sxs-lookup"><span data-stu-id="49bf4-141">Authorization determines whether the client can access a particular resource.</span></span>

<span data-ttu-id="49bf4-142">在 Web API 驗證篩選條件會處理驗證，但沒有授權。</span><span class="sxs-lookup"><span data-stu-id="49bf4-142">In Web API, authentication filters handle authentication, but not authorization.</span></span> <span data-ttu-id="49bf4-143">授權篩選條件或內部控制器動作，應該執行授權。</span><span class="sxs-lookup"><span data-stu-id="49bf4-143">Authorization should be done by an authorization filter or inside the controller action.</span></span>

<span data-ttu-id="49bf4-144">以下是 Web API 2 管線中的流程：</span><span class="sxs-lookup"><span data-stu-id="49bf4-144">Here is the flow in the Web API 2 pipeline:</span></span>

1. <span data-ttu-id="49bf4-145">在之前叫用動作，Web 應用程式開發介面會建立一份驗證篩選條件，該動作。</span><span class="sxs-lookup"><span data-stu-id="49bf4-145">Before invoking an action, Web API creates a list of the authentication filters for that action.</span></span> <span data-ttu-id="49bf4-146">這包括篩選器動作範圍、 控制站的範圍，與全域範圍。</span><span class="sxs-lookup"><span data-stu-id="49bf4-146">This includes filters with action scope, controller scope, and global scope.</span></span>
2. <span data-ttu-id="49bf4-147">Web 應用程式開發介面呼叫**AuthenticateAsync**清單中的每個篩選器。</span><span class="sxs-lookup"><span data-stu-id="49bf4-147">Web API calls **AuthenticateAsync** on every filter in the list.</span></span> <span data-ttu-id="49bf4-148">每個篩選條件可以驗證要求中的認證。</span><span class="sxs-lookup"><span data-stu-id="49bf4-148">Each filter can validate credentials in the request.</span></span> <span data-ttu-id="49bf4-149">如果任何篩選器已成功驗證認證，篩選器會建立**IPrincipal**並將其附加至要求。</span><span class="sxs-lookup"><span data-stu-id="49bf4-149">If any filter successfully validates credentials, the filter creates an **IPrincipal** and attaches it to the request.</span></span> <span data-ttu-id="49bf4-150">篩選也可以在此時觸發錯誤。</span><span class="sxs-lookup"><span data-stu-id="49bf4-150">A filter can also trigger an error at this point.</span></span> <span data-ttu-id="49bf4-151">如果是的話，就不會執行管線的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="49bf4-151">If so, the rest of the pipeline does not run.</span></span>
3. <span data-ttu-id="49bf4-152">假設沒有發生錯誤，要求會流過管線的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="49bf4-152">Assuming there is no error, the request flows through the rest of the pipeline.</span></span>
4. <span data-ttu-id="49bf4-153">最後，Web 應用程式開發介面呼叫每個驗證篩選條件**ChallengeAsync**方法。</span><span class="sxs-lookup"><span data-stu-id="49bf4-153">Finally, Web API calls every authentication filter's **ChallengeAsync** method.</span></span> <span data-ttu-id="49bf4-154">篩選器會使用這個方法將挑戰新增至回應，如有需要。</span><span class="sxs-lookup"><span data-stu-id="49bf4-154">Filters use this method to add a challenge to the response, if needed.</span></span> <span data-ttu-id="49bf4-155">通常 （但並非一定），就是發生在 401 錯誤回應。</span><span class="sxs-lookup"><span data-stu-id="49bf4-155">Typically (but not always) that would happen in response to a 401 error.</span></span>

<span data-ttu-id="49bf4-156">下圖顯示兩個可能的情況。</span><span class="sxs-lookup"><span data-stu-id="49bf4-156">The following diagrams show two possible cases.</span></span> <span data-ttu-id="49bf4-157">首先，驗證篩選條件已成功驗證要求，授權篩選條件授權要求，並控制器動作傳回 200 （確定）。</span><span class="sxs-lookup"><span data-stu-id="49bf4-157">In the first, the authentication filter successfully authenticates the request, an authorization filter authorizes the request, and the controller action returns 200 (OK).</span></span>

![](authentication-filters/_static/image1.png)

<span data-ttu-id="49bf4-158">在第二個範例中，驗證篩選條件來驗證要求，但授權篩選條件會傳回 401 （未經授權）。</span><span class="sxs-lookup"><span data-stu-id="49bf4-158">In the second example, the authentication filter authenticates the request, but the authorization filter returns 401 (Unauthorized).</span></span> <span data-ttu-id="49bf4-159">在此情況下，控制器動作不會叫用。</span><span class="sxs-lookup"><span data-stu-id="49bf4-159">In this case, the controller action is not invoked.</span></span> <span data-ttu-id="49bf4-160">驗證篩選條件新增至回應的 Www-authenticate 標頭。</span><span class="sxs-lookup"><span data-stu-id="49bf4-160">The authentication filter adds a Www-Authenticate header to the response.</span></span>

![](authentication-filters/_static/image2.png)

<span data-ttu-id="49bf4-161">其他組合都有可能&mdash;比方說，如果控制器動作可讓匿名要求，您可能會有的驗證篩選條件，但沒有授權。</span><span class="sxs-lookup"><span data-stu-id="49bf4-161">Other combinations are possible&mdash;for example, if the controller action allows anonymous requests, you might have an authentication filter but no authorization.</span></span>

## <a name="implementing-the-authenticateasync-method"></a><span data-ttu-id="49bf4-162">實作 AuthenticateAsync 方法</span><span class="sxs-lookup"><span data-stu-id="49bf4-162">Implementing the AuthenticateAsync Method</span></span>

<span data-ttu-id="49bf4-163">**AuthenticateAsync**方法會嘗試驗證要求。</span><span class="sxs-lookup"><span data-stu-id="49bf4-163">The **AuthenticateAsync** method tries to authenticate the request.</span></span> <span data-ttu-id="49bf4-164">以下是方法簽章：</span><span class="sxs-lookup"><span data-stu-id="49bf4-164">Here is the method signature:</span></span>

[!code-csharp[Main](authentication-filters/samples/sample4.cs)]

<span data-ttu-id="49bf4-165">**AuthenticateAsync**方法必須執行下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="49bf4-165">The **AuthenticateAsync** method must do one of the following:</span></span>

1. <span data-ttu-id="49bf4-166">沒有項目 （無作業）。</span><span class="sxs-lookup"><span data-stu-id="49bf4-166">Nothing (no-op).</span></span>
2. <span data-ttu-id="49bf4-167">建立**IPrincipal**並將它設定在要求上。</span><span class="sxs-lookup"><span data-stu-id="49bf4-167">Create an **IPrincipal** and set it on the request.</span></span>
3. <span data-ttu-id="49bf4-168">設定錯誤結果。</span><span class="sxs-lookup"><span data-stu-id="49bf4-168">Set an error result.</span></span>

<span data-ttu-id="49bf4-169">選項 (1) 表示要求沒有篩選了解的任何認證。</span><span class="sxs-lookup"><span data-stu-id="49bf4-169">Option (1) means the request did not have any credentials that the filter understands.</span></span> <span data-ttu-id="49bf4-170">選項 (2) 表示篩選條件已成功驗證要求。</span><span class="sxs-lookup"><span data-stu-id="49bf4-170">Option (2) means the filter successfully authenticated the request.</span></span> <span data-ttu-id="49bf4-171">選項 (3) 表示要求具有無效的認證 （例如錯誤的密碼），進而觸發錯誤回應。</span><span class="sxs-lookup"><span data-stu-id="49bf4-171">Option (3) means the request had invalid credentials (like the wrong password), which triggers an error response.</span></span>

<span data-ttu-id="49bf4-172">以下是實作的一般大綱**AuthenticateAsync**。</span><span class="sxs-lookup"><span data-stu-id="49bf4-172">Here is a general outline for implementing **AuthenticateAsync**.</span></span>

1. <span data-ttu-id="49bf4-173">尋找在要求中的認證。</span><span class="sxs-lookup"><span data-stu-id="49bf4-173">Look for credentials in the request.</span></span>
2. <span data-ttu-id="49bf4-174">如果沒有認證，不執行任何動作，並傳回 （無作業）。</span><span class="sxs-lookup"><span data-stu-id="49bf4-174">If there are no credentials, do nothing and return (no-op).</span></span>
3. <span data-ttu-id="49bf4-175">如果沒有認證，但是篩選無法辨識驗證配置，不執行任何動作，並傳回 （無作業）。</span><span class="sxs-lookup"><span data-stu-id="49bf4-175">If there are credentials but the filter does not recognize the authentication scheme, do nothing and return (no-op).</span></span> <span data-ttu-id="49bf4-176">在管線中的另一個篩選可能會了解配置。</span><span class="sxs-lookup"><span data-stu-id="49bf4-176">Another filter in the pipeline might understand the scheme.</span></span>
4. <span data-ttu-id="49bf4-177">如果篩選了解的認證，再試一次來驗證它們。</span><span class="sxs-lookup"><span data-stu-id="49bf4-177">If there are credentials that the filter understands, try to authenticate them.</span></span>
5. <span data-ttu-id="49bf4-178">如果認證不正確，會傳回 401 藉由設定`context.ErrorResult`。</span><span class="sxs-lookup"><span data-stu-id="49bf4-178">If the credentials are bad, return 401 by setting `context.ErrorResult`.</span></span>
6. <span data-ttu-id="49bf4-179">如果認證有效，請建立**IPrincipal**並設定`context.Principal`。</span><span class="sxs-lookup"><span data-stu-id="49bf4-179">If the credentials are valid, create an **IPrincipal** and set `context.Principal`.</span></span>

<span data-ttu-id="49bf4-180">下列程式碼所示**AuthenticateAsync**方法從[基本驗證](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt)範例。</span><span class="sxs-lookup"><span data-stu-id="49bf4-180">The follow code shows the **AuthenticateAsync** method from the [Basic Authentication](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt) sample.</span></span> <span data-ttu-id="49bf4-181">註解所指出的每個步驟。</span><span class="sxs-lookup"><span data-stu-id="49bf4-181">The comments indicate each step.</span></span> <span data-ttu-id="49bf4-182">程式碼會示範幾種類型的錯誤： 沒有認證，格式不正確的認證，而且不正確的使用者名稱/密碼的授權標頭。</span><span class="sxs-lookup"><span data-stu-id="49bf4-182">The code shows several types of error: An Authorization header with no credentials, malformed credentials, and bad username/password.</span></span>

[!code-csharp[Main](authentication-filters/samples/sample5.cs)]

## <a name="setting-an-error-result"></a><span data-ttu-id="49bf4-183">設定錯誤結果</span><span class="sxs-lookup"><span data-stu-id="49bf4-183">Setting an Error Result</span></span>

<span data-ttu-id="49bf4-184">如果認證無效，必須設定篩選`context.ErrorResult`至**IHttpActionResult**會建立錯誤回應。</span><span class="sxs-lookup"><span data-stu-id="49bf4-184">If the credentials are invalid, the filter must set `context.ErrorResult` to an **IHttpActionResult** that creates an error response.</span></span> <span data-ttu-id="49bf4-185">如需有關**IHttpActionResult**，請參閱[Web API 2 中的動作結果](../getting-started-with-aspnet-web-api/action-results.md)。</span><span class="sxs-lookup"><span data-stu-id="49bf4-185">For more information about **IHttpActionResult**, see [Action Results in Web API 2](../getting-started-with-aspnet-web-api/action-results.md).</span></span>

<span data-ttu-id="49bf4-186">基本驗證的範例包括`AuthenticationFailureResult`適用於此用途的類別。</span><span class="sxs-lookup"><span data-stu-id="49bf4-186">The Basic Authentication sample includes an `AuthenticationFailureResult` class that is suitable for this purpose.</span></span>

[!code-csharp[Main](authentication-filters/samples/sample6.cs)]

## <a name="implementing-challengeasync"></a><span data-ttu-id="49bf4-187">實作 ChallengeAsync</span><span class="sxs-lookup"><span data-stu-id="49bf4-187">Implementing ChallengeAsync</span></span>

<span data-ttu-id="49bf4-188">目的**ChallengeAsync**如有需要方法是將驗證挑戰新增至回應。</span><span class="sxs-lookup"><span data-stu-id="49bf4-188">The purpose of the **ChallengeAsync** method is to add authentication challenges to the response, if needed.</span></span> <span data-ttu-id="49bf4-189">以下是方法簽章：</span><span class="sxs-lookup"><span data-stu-id="49bf4-189">Here is the method signature:</span></span>

[!code-csharp[Main](authentication-filters/samples/sample7.cs)]

<span data-ttu-id="49bf4-190">每個要求管線中的驗證篩選器上呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="49bf4-190">The method is called on every authentication filter in the request pipeline.</span></span>

<span data-ttu-id="49bf4-191">請務必了解**ChallengeAsync**稱為*之前*HTTP 回應已建立，且可能甚至控制器動作執行之前。</span><span class="sxs-lookup"><span data-stu-id="49bf4-191">It's important to understand that **ChallengeAsync** is called *before* the HTTP response is created, and possibly even before the controller action runs.</span></span> <span data-ttu-id="49bf4-192">當**ChallengeAsync**呼叫時，`context.Result`包含**IHttpActionResult**，其稍後用來建立 HTTP 回應。</span><span class="sxs-lookup"><span data-stu-id="49bf4-192">When **ChallengeAsync** is called, `context.Result` contains an **IHttpActionResult**, which is used later to create the HTTP response.</span></span> <span data-ttu-id="49bf4-193">因此**ChallengeAsync**是呼叫，您還不知道有關 HTTP 回應的任何項目。</span><span class="sxs-lookup"><span data-stu-id="49bf4-193">So when **ChallengeAsync** is called, you don't know anything about the HTTP response yet.</span></span> <span data-ttu-id="49bf4-194">**ChallengeAsync**方法應該取代的原始值`context.Result`與新**IHttpActionResult**。</span><span class="sxs-lookup"><span data-stu-id="49bf4-194">The **ChallengeAsync** method should replace the original value of `context.Result` with a new **IHttpActionResult**.</span></span> <span data-ttu-id="49bf4-195">這**IHttpActionResult**必須包裝原始`context.Result`。</span><span class="sxs-lookup"><span data-stu-id="49bf4-195">This **IHttpActionResult** must wrap the original `context.Result`.</span></span>

![](authentication-filters/_static/image3.png)

<span data-ttu-id="49bf4-196">我稱原始**IHttpActionResult** *的內部結果*，與新**IHttpActionResult** *外部結果*。</span><span class="sxs-lookup"><span data-stu-id="49bf4-196">I'll call the original **IHttpActionResult** the *inner result*, and the new **IHttpActionResult** the *outer result*.</span></span> <span data-ttu-id="49bf4-197">外部的結果必須執行下列作業：</span><span class="sxs-lookup"><span data-stu-id="49bf4-197">The outer result must do the following:</span></span>

1. <span data-ttu-id="49bf4-198">叫用來建立 HTTP 回應的內部結果。</span><span class="sxs-lookup"><span data-stu-id="49bf4-198">Invoke the inner result to create the HTTP response.</span></span>
2. <span data-ttu-id="49bf4-199">檢查回應。</span><span class="sxs-lookup"><span data-stu-id="49bf4-199">Examine the response.</span></span>
3. <span data-ttu-id="49bf4-200">視需要新增至回應，驗證挑戰。</span><span class="sxs-lookup"><span data-stu-id="49bf4-200">Add an authentication challenge to the response, if needed.</span></span>

<span data-ttu-id="49bf4-201">下列範例取自 「 基本驗證的範例。</span><span class="sxs-lookup"><span data-stu-id="49bf4-201">The following example is taken from the Basic Authentication sample.</span></span> <span data-ttu-id="49bf4-202">它會定義**IHttpActionResult**外部的結果。</span><span class="sxs-lookup"><span data-stu-id="49bf4-202">It defines an **IHttpActionResult** for the outer result.</span></span>

[!code-csharp[Main](authentication-filters/samples/sample8.cs)]

<span data-ttu-id="49bf4-203">`InnerResult`屬性會保留內部**IHttpActionResult**。</span><span class="sxs-lookup"><span data-stu-id="49bf4-203">The `InnerResult` property holds the inner **IHttpActionResult**.</span></span> <span data-ttu-id="49bf4-204">`Challenge`屬性表示 Www 驗證標頭。</span><span class="sxs-lookup"><span data-stu-id="49bf4-204">The `Challenge` property represents a Www-Authentication header.</span></span> <span data-ttu-id="49bf4-205">請注意， **ExecuteAsync**會先呼叫`InnerResult.ExecuteAsync`建立 HTTP 回應，如有需要然後將所面臨的挑戰。</span><span class="sxs-lookup"><span data-stu-id="49bf4-205">Notice that **ExecuteAsync** first calls `InnerResult.ExecuteAsync` to create the HTTP response, and then adds the challenge if needed.</span></span>

<span data-ttu-id="49bf4-206">檢查然後再加入挑戰的回應碼。</span><span class="sxs-lookup"><span data-stu-id="49bf4-206">Check the response code before adding the challenge.</span></span> <span data-ttu-id="49bf4-207">大部分的驗證配置只能新增一項挑戰是否回應 401，如下所示。</span><span class="sxs-lookup"><span data-stu-id="49bf4-207">Most authentication schemes only add a challenge if the response is 401, as shown here.</span></span> <span data-ttu-id="49bf4-208">不過，某些驗證配置不要新增至成功回應的一項挑戰。</span><span class="sxs-lookup"><span data-stu-id="49bf4-208">However, some authentication schemes do add a challenge to a success response.</span></span> <span data-ttu-id="49bf4-209">例如，請參閱[交涉](http://tools.ietf.org/html/rfc4559#section-5)(RFC 4559)。</span><span class="sxs-lookup"><span data-stu-id="49bf4-209">For example, see [Negotiate](http://tools.ietf.org/html/rfc4559#section-5) (RFC 4559).</span></span>

<span data-ttu-id="49bf4-210">指定`AddChallengeOnUnauthorizedResult`類別中的實際程式碼**ChallengeAsync**很簡單。</span><span class="sxs-lookup"><span data-stu-id="49bf4-210">Given the `AddChallengeOnUnauthorizedResult` class, the actual code in **ChallengeAsync** is simple.</span></span> <span data-ttu-id="49bf4-211">您剛才建立的結果，並將其附加至`context.Result`。</span><span class="sxs-lookup"><span data-stu-id="49bf4-211">You just create the result and attach it to `context.Result`.</span></span>

[!code-csharp[Main](authentication-filters/samples/sample9.cs)]

<span data-ttu-id="49bf4-212">注意： 基本驗證範例擷取此邏輯位元，將它放在擴充方法。</span><span class="sxs-lookup"><span data-stu-id="49bf4-212">Note: The Basic Authentication sample abstracts this logic a bit, by placing it in an extension method.</span></span>

## <a name="combining-authentication-filters-with-host-level-authentication"></a><span data-ttu-id="49bf4-213">結合使用主機層級驗證的驗證篩選條件</span><span class="sxs-lookup"><span data-stu-id="49bf4-213">Combining Authentication Filters with Host-Level Authentication</span></span>

<span data-ttu-id="49bf4-214">「 主機層級驗證 」 是由主應用程式 （例如 IIS) 執行的驗證要求到達 Web API framework 之前。</span><span class="sxs-lookup"><span data-stu-id="49bf4-214">"Host-level authentication" is authentication performed by the host (such as IIS), before the request reaches the Web API framework.</span></span>

<span data-ttu-id="49bf4-215">通常，您可以啟用您的應用程式的其餘部分的主機層級驗證，但停用 Web API 控制器。</span><span class="sxs-lookup"><span data-stu-id="49bf4-215">Often, you may want to enable host-level authentication for the rest of your application, but disable it for your Web API controllers.</span></span> <span data-ttu-id="49bf4-216">例如，典型的案例是要啟用表單驗證，在主機層級，但用於 Web API 的權杖型驗證。</span><span class="sxs-lookup"><span data-stu-id="49bf4-216">For example, a typical scenario is to enable Forms Authentication at the host level, but use token-based authentication for Web API.</span></span>

<span data-ttu-id="49bf4-217">若要停用 Web API 管線內的主機層級驗證，請呼叫`config.SuppressHostPrincipal()`組態中。</span><span class="sxs-lookup"><span data-stu-id="49bf4-217">To disable host-level authentication inside the Web API pipeline, call `config.SuppressHostPrincipal()` in your configuration.</span></span> <span data-ttu-id="49bf4-218">這會導致 Web API 來移除**IPrincipal**從任何輸入 Web API 管線的要求。</span><span class="sxs-lookup"><span data-stu-id="49bf4-218">This causes Web API to remove the **IPrincipal** from any request that enters the Web API pipeline.</span></span> <span data-ttu-id="49bf4-219">實際上，它&quot;取消-驗證&quot;要求。</span><span class="sxs-lookup"><span data-stu-id="49bf4-219">Effectively, it &quot;un-authenticates&quot; the request.</span></span>

[!code-csharp[Main](authentication-filters/samples/sample10.cs)]

## <a name="additional-resources"></a><span data-ttu-id="49bf4-220">其他資源</span><span class="sxs-lookup"><span data-stu-id="49bf4-220">Additional Resources</span></span>

<span data-ttu-id="49bf4-221">[ASP.NET Web API 的安全性篩選](https://msdn.microsoft.com/magazine/dn781361.aspx)(MSDN Magazine)</span><span class="sxs-lookup"><span data-stu-id="49bf4-221">[ASP.NET Web API Security Filters](https://msdn.microsoft.com/magazine/dn781361.aspx) (MSDN Magazine)</span></span>
