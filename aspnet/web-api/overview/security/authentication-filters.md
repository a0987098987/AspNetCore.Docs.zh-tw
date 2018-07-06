---
uid: web-api/overview/security/authentication-filters
title: ASP.NET Web API 2 中的驗證篩選條件 |Microsoft Docs
author: MikeWasson
description: 驗證篩選條件是一種元件，會驗證 HTTP 要求。 Web API 2 和 MVC 5 都支援的驗證篩選條件，但他們稍有不同...
ms.author: aspnetcontent
ms.date: 09/25/2014
ms.assetid: b9882e53-b3ca-4def-89b0-322846973ccb
msc.legacyurl: /web-api/overview/security/authentication-filters
msc.type: authoredcontent
ms.openlocfilehash: 6cad52e0454d685c6e96746524fbbad21e1c274d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839813"
---
<a name="authentication-filters-in-aspnet-web-api-2"></a><span data-ttu-id="48725-104">ASP.NET Web API 2 中的驗證篩選條件</span><span class="sxs-lookup"><span data-stu-id="48725-104">Authentication Filters in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="48725-105">藉由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="48725-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="48725-106">驗證篩選條件是一種元件，會驗證 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="48725-106">An authentication filter is a component that authenticates an HTTP request.</span></span> <span data-ttu-id="48725-107">Web API 2 和 MVC 5 都支援的驗證篩選條件，但他們稍有不同，大部分是在篩選條件介面的命名慣例。</span><span class="sxs-lookup"><span data-stu-id="48725-107">Web API 2 and MVC 5 both support authentication filters, but they differ slightly, mostly in the naming conventions for the filter interface.</span></span> <span data-ttu-id="48725-108">本主題描述 Web API 驗證篩選條件。</span><span class="sxs-lookup"><span data-stu-id="48725-108">This topic describes Web API authentication filters.</span></span>


<span data-ttu-id="48725-109">驗證篩選條件可讓您針對個別的控制器或動作中設定驗證配置。</span><span class="sxs-lookup"><span data-stu-id="48725-109">Authentication filters let you set an authentication scheme for individual controllers or actions.</span></span> <span data-ttu-id="48725-110">這樣一來，您的應用程式可以對不同的 HTTP 資源支援不同的驗證機制。</span><span class="sxs-lookup"><span data-stu-id="48725-110">That way, your app can support different authentication mechanisms for different HTTP resources.</span></span>

<span data-ttu-id="48725-111">在本文中，我將示範從程式碼[基本驗證](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt)上的範例[ http://aspnet.codeplex.com ](http://aspnet.codeplex.com)。</span><span class="sxs-lookup"><span data-stu-id="48725-111">In this article, I'll show code from the [Basic Authentication](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt) sample on [http://aspnet.codeplex.com](http://aspnet.codeplex.com).</span></span> <span data-ttu-id="48725-112">此範例會示範實作 HTTP 基本存取驗證配置 (RFC 2617) 的驗證篩選條件。</span><span class="sxs-lookup"><span data-stu-id="48725-112">The sample shows an authentication filter that implements the HTTP Basic Access Authentication scheme (RFC 2617).</span></span> <span data-ttu-id="48725-113">名為類別中實作篩選條件`IdentityBasicAuthenticationAttribute`。</span><span class="sxs-lookup"><span data-stu-id="48725-113">The filter is implemented in a class named `IdentityBasicAuthenticationAttribute`.</span></span> <span data-ttu-id="48725-114">我不會顯示所有的程式碼範例中，從只說明如何撰寫的驗證篩選條件的組件。</span><span class="sxs-lookup"><span data-stu-id="48725-114">I won't show all of the code from the sample, just the parts that illustrate how to write an authentication filter.</span></span>

## <a name="setting-an-authentication-filter"></a><span data-ttu-id="48725-115">設定的驗證篩選條件</span><span class="sxs-lookup"><span data-stu-id="48725-115">Setting an Authentication Filter</span></span>

<span data-ttu-id="48725-116">其他篩選器，例如驗證篩選條件可以套用每個控制站、 每個動作或全域所有的 Web API 控制器。</span><span class="sxs-lookup"><span data-stu-id="48725-116">Like other filters, authentication filters can be applied per-controller, per-action, or globally to all Web API controllers.</span></span>

<span data-ttu-id="48725-117">若要套用的驗證篩選條件，控制站，來裝飾控制器類別，以篩選條件屬性。</span><span class="sxs-lookup"><span data-stu-id="48725-117">To apply an authentication filter to a controller, decorate the controller class with the filter attribute.</span></span> <span data-ttu-id="48725-118">下列程式碼設定`[IdentityBasicAuthentication]`篩選控制器類別，可讓所有控制器動作的基本驗證。</span><span class="sxs-lookup"><span data-stu-id="48725-118">The following code sets the `[IdentityBasicAuthentication]` filter on a controller class, which enables Basic Authentication for all of the controller's actions.</span></span>

[!code-csharp[Main](authentication-filters/samples/sample1.cs)]

<span data-ttu-id="48725-119">若要將篩選套用至一個動作，請使用篩選器裝飾的動作。</span><span class="sxs-lookup"><span data-stu-id="48725-119">To apply the filter to one action, decorate the action with the filter.</span></span> <span data-ttu-id="48725-120">下列程式碼設定`[IdentityBasicAuthentication]`控制器的篩選`Post`方法。</span><span class="sxs-lookup"><span data-stu-id="48725-120">The following code sets the `[IdentityBasicAuthentication]` filter on the controller's `Post` method.</span></span>

[!code-csharp[Main](authentication-filters/samples/sample2.cs)]

<span data-ttu-id="48725-121">若要將篩選套用到所有的 Web API 控制器，將它加入**GlobalConfiguration.Filters**。</span><span class="sxs-lookup"><span data-stu-id="48725-121">To apply the filter to all Web API controllers, add it to **GlobalConfiguration.Filters**.</span></span>

[!code-csharp[Main](authentication-filters/samples/sample3.cs)]

## <a name="implementing-a-web-api-authentication-filter"></a><span data-ttu-id="48725-122">實作 Web API 驗證篩選條件</span><span class="sxs-lookup"><span data-stu-id="48725-122">Implementing a Web API Authentication Filter</span></span>

<span data-ttu-id="48725-123">在 Web API 驗證篩選條件會實作[System.Web.Http.Filters.IAuthenticationFilter](https://msdn.microsoft.com/library/system.web.http.filters.iauthenticationfilter.aspx)介面。</span><span class="sxs-lookup"><span data-stu-id="48725-123">In Web API, authentication filters implement the [System.Web.Http.Filters.IAuthenticationFilter](https://msdn.microsoft.com/library/system.web.http.filters.iauthenticationfilter.aspx) interface.</span></span> <span data-ttu-id="48725-124">它們也應該繼承自**System.Attribute**，才能套用為屬性。</span><span class="sxs-lookup"><span data-stu-id="48725-124">They should also inherit from **System.Attribute**, in order to be applied as attributes.</span></span>

<span data-ttu-id="48725-125">**IAuthenticationFilter**介面有兩種方法：</span><span class="sxs-lookup"><span data-stu-id="48725-125">The **IAuthenticationFilter** interface has two methods:</span></span>

- <span data-ttu-id="48725-126">**AuthenticateAsync**藉由驗證要求中的認證來驗證要求，如果有的話。</span><span class="sxs-lookup"><span data-stu-id="48725-126">**AuthenticateAsync** authenticates the request by validating credentials in the request, if present.</span></span>
- <span data-ttu-id="48725-127">**ChallengeAsync**將驗證挑戰新增至 HTTP 回應，如有需要。</span><span class="sxs-lookup"><span data-stu-id="48725-127">**ChallengeAsync** adds an authentication challenge to the HTTP response, if needed.</span></span>

<span data-ttu-id="48725-128">這些方法會對應至驗證流程中定義[RFC 2612](http://tools.ietf.org/html/rfc2616)並[RFC 2617](http://tools.ietf.org/html/rfc2617):</span><span class="sxs-lookup"><span data-stu-id="48725-128">These methods correspond to the authentication flow defined in [RFC 2612](http://tools.ietf.org/html/rfc2616) and [RFC 2617](http://tools.ietf.org/html/rfc2617):</span></span>

1. <span data-ttu-id="48725-129">用戶端會將認證傳送授權標頭中。</span><span class="sxs-lookup"><span data-stu-id="48725-129">The client sends credentials in the Authorization header.</span></span> <span data-ttu-id="48725-130">此外，這通常會發生之後用戶端從伺服器收到 401 （未經授權） 回應。</span><span class="sxs-lookup"><span data-stu-id="48725-130">This typically happens after the client receives a 401 (Unauthorized) response from the server.</span></span> <span data-ttu-id="48725-131">不過，用戶端可以取得 401 之後，不只是傳送任何要求中，使用的認證。</span><span class="sxs-lookup"><span data-stu-id="48725-131">However, a client can send credentials with any request, not just after getting a 401.</span></span>
2. <span data-ttu-id="48725-132">如果伺服器未接受認證，則會傳回 401 （未經授權） 回應。</span><span class="sxs-lookup"><span data-stu-id="48725-132">If the server does not accept the credentials, it returns a 401 (Unauthorized) response.</span></span> <span data-ttu-id="48725-133">此回應包含 Www-authenticate 標頭，其中包含一或多個挑戰。</span><span class="sxs-lookup"><span data-stu-id="48725-133">The response includes a Www-Authenticate header that contains one or more challenges.</span></span> <span data-ttu-id="48725-134">每一項挑戰會指定伺服器辨識驗證配置。</span><span class="sxs-lookup"><span data-stu-id="48725-134">Each challenge specifies an authentication scheme recognized by the server.</span></span>

<span data-ttu-id="48725-135">伺服器也可以從匿名要求傳回 401。</span><span class="sxs-lookup"><span data-stu-id="48725-135">The server can also return 401 from an anonymous request.</span></span> <span data-ttu-id="48725-136">事實上，這通常是起始驗證程序的方式：</span><span class="sxs-lookup"><span data-stu-id="48725-136">In fact, that's typically how the authentication process is initiated:</span></span>

1. <span data-ttu-id="48725-137">用戶端傳送匿名要求。</span><span class="sxs-lookup"><span data-stu-id="48725-137">The client sends an anonymous request.</span></span>
2. <span data-ttu-id="48725-138">伺服器會傳回 401。</span><span class="sxs-lookup"><span data-stu-id="48725-138">The server returns 401.</span></span>
3. <span data-ttu-id="48725-139">用戶端重新傳送認證的要求。</span><span class="sxs-lookup"><span data-stu-id="48725-139">The clients resends the request with credentials.</span></span>

<span data-ttu-id="48725-140">此流程同時包含*驗證*並*授權*步驟。</span><span class="sxs-lookup"><span data-stu-id="48725-140">This flow includes both *authentication* and *authorization* steps.</span></span>

- <span data-ttu-id="48725-141">驗證可證明用戶端的身分識別。</span><span class="sxs-lookup"><span data-stu-id="48725-141">Authentication proves the identity of the client.</span></span>
- <span data-ttu-id="48725-142">授權會決定用戶端是否可以存取特定資源。</span><span class="sxs-lookup"><span data-stu-id="48725-142">Authorization determines whether the client can access a particular resource.</span></span>

<span data-ttu-id="48725-143">在 Web API 驗證篩選條件會處理驗證，但沒有授權。</span><span class="sxs-lookup"><span data-stu-id="48725-143">In Web API, authentication filters handle authentication, but not authorization.</span></span> <span data-ttu-id="48725-144">授權篩選條件或控制器動作內，應該授權。</span><span class="sxs-lookup"><span data-stu-id="48725-144">Authorization should be done by an authorization filter or inside the controller action.</span></span>

<span data-ttu-id="48725-145">以下是 Web API 2 管線中的流程：</span><span class="sxs-lookup"><span data-stu-id="48725-145">Here is the flow in the Web API 2 pipeline:</span></span>

1. <span data-ttu-id="48725-146">之前叫用動作，Web API 會建立一份驗證篩選條件，該動作。</span><span class="sxs-lookup"><span data-stu-id="48725-146">Before invoking an action, Web API creates a list of the authentication filters for that action.</span></span> <span data-ttu-id="48725-147">這包括篩選器動作範圍、 控制器範圍與全域範圍。</span><span class="sxs-lookup"><span data-stu-id="48725-147">This includes filters with action scope, controller scope, and global scope.</span></span>
2. <span data-ttu-id="48725-148">Web API 呼叫**AuthenticateAsync**清單中的每個篩選器。</span><span class="sxs-lookup"><span data-stu-id="48725-148">Web API calls **AuthenticateAsync** on every filter in the list.</span></span> <span data-ttu-id="48725-149">每個篩選條件可以驗證要求中的認證。</span><span class="sxs-lookup"><span data-stu-id="48725-149">Each filter can validate credentials in the request.</span></span> <span data-ttu-id="48725-150">如果任何篩選器已成功驗證認證，就會建立篩選條件**IPrincipal**並將它附加至要求。</span><span class="sxs-lookup"><span data-stu-id="48725-150">If any filter successfully validates credentials, the filter creates an **IPrincipal** and attaches it to the request.</span></span> <span data-ttu-id="48725-151">篩選也可以在此時觸發錯誤。</span><span class="sxs-lookup"><span data-stu-id="48725-151">A filter can also trigger an error at this point.</span></span> <span data-ttu-id="48725-152">如果是的話，就不會執行管線的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="48725-152">If so, the rest of the pipeline does not run.</span></span>
3. <span data-ttu-id="48725-153">假設沒有任何錯誤，要求會流經管線的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="48725-153">Assuming there is no error, the request flows through the rest of the pipeline.</span></span>
4. <span data-ttu-id="48725-154">最後，Web API 會呼叫每個驗證篩選條件**ChallengeAsync**方法。</span><span class="sxs-lookup"><span data-stu-id="48725-154">Finally, Web API calls every authentication filter's **ChallengeAsync** method.</span></span> <span data-ttu-id="48725-155">如有需要篩選會使用這個方法所做出的回應中加入一項挑戰。</span><span class="sxs-lookup"><span data-stu-id="48725-155">Filters use this method to add a challenge to the response, if needed.</span></span> <span data-ttu-id="48725-156">一般而言 （但並非絕對），會發生 401 錯誤回應。</span><span class="sxs-lookup"><span data-stu-id="48725-156">Typically (but not always) that would happen in response to a 401 error.</span></span>

<span data-ttu-id="48725-157">下圖顯示兩個可能的情況。</span><span class="sxs-lookup"><span data-stu-id="48725-157">The following diagrams show two possible cases.</span></span> <span data-ttu-id="48725-158">第一次，驗證篩選已成功驗證要求，授權篩選授權要求，並控制器動作傳回 200 （確定）。</span><span class="sxs-lookup"><span data-stu-id="48725-158">In the first, the authentication filter successfully authenticates the request, an authorization filter authorizes the request, and the controller action returns 200 (OK).</span></span>

![](authentication-filters/_static/image1.png)

<span data-ttu-id="48725-159">在第二個範例中，驗證篩選條件來驗證要求，但授權篩選條件會傳回 401 （未經授權）。</span><span class="sxs-lookup"><span data-stu-id="48725-159">In the second example, the authentication filter authenticates the request, but the authorization filter returns 401 (Unauthorized).</span></span> <span data-ttu-id="48725-160">在此情況下，不會叫用控制器動作。</span><span class="sxs-lookup"><span data-stu-id="48725-160">In this case, the controller action is not invoked.</span></span> <span data-ttu-id="48725-161">驗證篩選條件會將 Www-authenticate 標頭加入回應。</span><span class="sxs-lookup"><span data-stu-id="48725-161">The authentication filter adds a Www-Authenticate header to the response.</span></span>

![](authentication-filters/_static/image2.png)

<span data-ttu-id="48725-162">其他組合都有可能&mdash;比方說，如果控制器動作允許匿名要求，您可能會有的驗證篩選條件但沒有授權。</span><span class="sxs-lookup"><span data-stu-id="48725-162">Other combinations are possible&mdash;for example, if the controller action allows anonymous requests, you might have an authentication filter but no authorization.</span></span>

## <a name="implementing-the-authenticateasync-method"></a><span data-ttu-id="48725-163">實作 AuthenticateAsync 方法</span><span class="sxs-lookup"><span data-stu-id="48725-163">Implementing the AuthenticateAsync Method</span></span>

<span data-ttu-id="48725-164">**AuthenticateAsync**方法會嘗試驗證要求。</span><span class="sxs-lookup"><span data-stu-id="48725-164">The **AuthenticateAsync** method tries to authenticate the request.</span></span> <span data-ttu-id="48725-165">以下是方法簽章：</span><span class="sxs-lookup"><span data-stu-id="48725-165">Here is the method signature:</span></span>

[!code-csharp[Main](authentication-filters/samples/sample4.cs)]

<span data-ttu-id="48725-166">**AuthenticateAsync**方法必須執行下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="48725-166">The **AuthenticateAsync** method must do one of the following:</span></span>

1. <span data-ttu-id="48725-167">沒有項目 （無作業）。</span><span class="sxs-lookup"><span data-stu-id="48725-167">Nothing (no-op).</span></span>
2. <span data-ttu-id="48725-168">建立**IPrincipal**並將它設定在要求上。</span><span class="sxs-lookup"><span data-stu-id="48725-168">Create an **IPrincipal** and set it on the request.</span></span>
3. <span data-ttu-id="48725-169">設定錯誤結果。</span><span class="sxs-lookup"><span data-stu-id="48725-169">Set an error result.</span></span>

<span data-ttu-id="48725-170">選項 (1) 表示要求沒有任何篩選了解的認證。</span><span class="sxs-lookup"><span data-stu-id="48725-170">Option (1) means the request did not have any credentials that the filter understands.</span></span> <span data-ttu-id="48725-171">選項 (2) 表示篩選條件成功驗證要求。</span><span class="sxs-lookup"><span data-stu-id="48725-171">Option (2) means the filter successfully authenticated the request.</span></span> <span data-ttu-id="48725-172">選項 (3) 表示要求有無效的認證 （例如錯誤的密碼），這會觸發錯誤回應。</span><span class="sxs-lookup"><span data-stu-id="48725-172">Option (3) means the request had invalid credentials (like the wrong password), which triggers an error response.</span></span>

<span data-ttu-id="48725-173">以下是實作的一般概述**AuthenticateAsync**。</span><span class="sxs-lookup"><span data-stu-id="48725-173">Here is a general outline for implementing **AuthenticateAsync**.</span></span>

1. <span data-ttu-id="48725-174">尋找在要求中的認證。</span><span class="sxs-lookup"><span data-stu-id="48725-174">Look for credentials in the request.</span></span>
2. <span data-ttu-id="48725-175">如果沒有認證，不執行任何動作，並傳回 （無作業）。</span><span class="sxs-lookup"><span data-stu-id="48725-175">If there are no credentials, do nothing and return (no-op).</span></span>
3. <span data-ttu-id="48725-176">如果沒有認證，但篩選條件無法辨識的驗證配置，不執行任何動作，並傳回 （無作業）。</span><span class="sxs-lookup"><span data-stu-id="48725-176">If there are credentials but the filter does not recognize the authentication scheme, do nothing and return (no-op).</span></span> <span data-ttu-id="48725-177">在管線中的另一個篩選條件可能會了解配置。</span><span class="sxs-lookup"><span data-stu-id="48725-177">Another filter in the pipeline might understand the scheme.</span></span>
4. <span data-ttu-id="48725-178">如果篩選了解的認證，請嘗試以進行驗證。</span><span class="sxs-lookup"><span data-stu-id="48725-178">If there are credentials that the filter understands, try to authenticate them.</span></span>
5. <span data-ttu-id="48725-179">如果認證不正確，請藉由設定傳回 401 `context.ErrorResult`。</span><span class="sxs-lookup"><span data-stu-id="48725-179">If the credentials are bad, return 401 by setting `context.ErrorResult`.</span></span>
6. <span data-ttu-id="48725-180">如果是有效的認證，建立**IPrincipal**並設定`context.Principal`。</span><span class="sxs-lookup"><span data-stu-id="48725-180">If the credentials are valid, create an **IPrincipal** and set `context.Principal`.</span></span>

<span data-ttu-id="48725-181">下列程式碼所示**AuthenticateAsync**方法，從[基本驗證](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt)範例。</span><span class="sxs-lookup"><span data-stu-id="48725-181">The follow code shows the **AuthenticateAsync** method from the [Basic Authentication](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt) sample.</span></span> <span data-ttu-id="48725-182">註解每個步驟。</span><span class="sxs-lookup"><span data-stu-id="48725-182">The comments indicate each step.</span></span> <span data-ttu-id="48725-183">程式碼會示範數種類型的錯誤： 沒有認證，格式不正確的認證，而且使用者名稱/密碼不正確的授權標頭。</span><span class="sxs-lookup"><span data-stu-id="48725-183">The code shows several types of error: An Authorization header with no credentials, malformed credentials, and bad username/password.</span></span>

[!code-csharp[Main](authentication-filters/samples/sample5.cs)]

## <a name="setting-an-error-result"></a><span data-ttu-id="48725-184">設定錯誤結果</span><span class="sxs-lookup"><span data-stu-id="48725-184">Setting an Error Result</span></span>

<span data-ttu-id="48725-185">如果認證無效，必須設定篩選`context.ErrorResult`要**IHttpActionResult**這樣將會產生錯誤回應。</span><span class="sxs-lookup"><span data-stu-id="48725-185">If the credentials are invalid, the filter must set `context.ErrorResult` to an **IHttpActionResult** that creates an error response.</span></span> <span data-ttu-id="48725-186">如需詳細資訊**IHttpActionResult**，請參閱[Web API 2 中的動作結果](../getting-started-with-aspnet-web-api/action-results.md)。</span><span class="sxs-lookup"><span data-stu-id="48725-186">For more information about **IHttpActionResult**, see [Action Results in Web API 2](../getting-started-with-aspnet-web-api/action-results.md).</span></span>

<span data-ttu-id="48725-187">基本驗證的範例包括`AuthenticationFailureResult`適用於此用途的類別。</span><span class="sxs-lookup"><span data-stu-id="48725-187">The Basic Authentication sample includes an `AuthenticationFailureResult` class that is suitable for this purpose.</span></span>

[!code-csharp[Main](authentication-filters/samples/sample6.cs)]

## <a name="implementing-challengeasync"></a><span data-ttu-id="48725-188">實作 ChallengeAsync</span><span class="sxs-lookup"><span data-stu-id="48725-188">Implementing ChallengeAsync</span></span>

<span data-ttu-id="48725-189">目的**ChallengeAsync**如有需要方法是將驗證挑戰新增至回應。</span><span class="sxs-lookup"><span data-stu-id="48725-189">The purpose of the **ChallengeAsync** method is to add authentication challenges to the response, if needed.</span></span> <span data-ttu-id="48725-190">以下是方法簽章：</span><span class="sxs-lookup"><span data-stu-id="48725-190">Here is the method signature:</span></span>

[!code-csharp[Main](authentication-filters/samples/sample7.cs)]

<span data-ttu-id="48725-191">每個要求管線中的驗證篩選器上呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="48725-191">The method is called on every authentication filter in the request pipeline.</span></span>

<span data-ttu-id="48725-192">請務必了解**ChallengeAsync**稱為*之前*HTTP 回應便會建立，並甚至還可以在控制器動作執行之前。</span><span class="sxs-lookup"><span data-stu-id="48725-192">It's important to understand that **ChallengeAsync** is called *before* the HTTP response is created, and possibly even before the controller action runs.</span></span> <span data-ttu-id="48725-193">當**ChallengeAsync**呼叫時，`context.Result`包含**IHttpActionResult**，稍後用來建立 HTTP 回應。</span><span class="sxs-lookup"><span data-stu-id="48725-193">When **ChallengeAsync** is called, `context.Result` contains an **IHttpActionResult**, which is used later to create the HTTP response.</span></span> <span data-ttu-id="48725-194">因此當**ChallengeAsync**是呼叫，您還不知道有關 HTTP 回應的任何項目。</span><span class="sxs-lookup"><span data-stu-id="48725-194">So when **ChallengeAsync** is called, you don't know anything about the HTTP response yet.</span></span> <span data-ttu-id="48725-195">**ChallengeAsync**方法應該取代的原始值`context.Result`的新**IHttpActionResult**。</span><span class="sxs-lookup"><span data-stu-id="48725-195">The **ChallengeAsync** method should replace the original value of `context.Result` with a new **IHttpActionResult**.</span></span> <span data-ttu-id="48725-196">這**IHttpActionResult**必須包裝原始`context.Result`。</span><span class="sxs-lookup"><span data-stu-id="48725-196">This **IHttpActionResult** must wrap the original `context.Result`.</span></span>

![](authentication-filters/_static/image3.png)

<span data-ttu-id="48725-197">我稱原始**IHttpActionResult** *內部結果*，和新**IHttpActionResult** *外部結果*。</span><span class="sxs-lookup"><span data-stu-id="48725-197">I'll call the original **IHttpActionResult** the *inner result*, and the new **IHttpActionResult** the *outer result*.</span></span> <span data-ttu-id="48725-198">外部結果必須執行下列作業：</span><span class="sxs-lookup"><span data-stu-id="48725-198">The outer result must do the following:</span></span>

1. <span data-ttu-id="48725-199">叫用內部的結果，以建立 HTTP 回應。</span><span class="sxs-lookup"><span data-stu-id="48725-199">Invoke the inner result to create the HTTP response.</span></span>
2. <span data-ttu-id="48725-200">檢查回應。</span><span class="sxs-lookup"><span data-stu-id="48725-200">Examine the response.</span></span>
3. <span data-ttu-id="48725-201">如有需要則您可以加入回應驗證挑戰。</span><span class="sxs-lookup"><span data-stu-id="48725-201">Add an authentication challenge to the response, if needed.</span></span>

<span data-ttu-id="48725-202">下列範例是取自基本驗證的範例。</span><span class="sxs-lookup"><span data-stu-id="48725-202">The following example is taken from the Basic Authentication sample.</span></span> <span data-ttu-id="48725-203">它會定義**IHttpActionResult**外部的結果。</span><span class="sxs-lookup"><span data-stu-id="48725-203">It defines an **IHttpActionResult** for the outer result.</span></span>

[!code-csharp[Main](authentication-filters/samples/sample8.cs)]

<span data-ttu-id="48725-204">`InnerResult`屬性會保留內部**IHttpActionResult**。</span><span class="sxs-lookup"><span data-stu-id="48725-204">The `InnerResult` property holds the inner **IHttpActionResult**.</span></span> <span data-ttu-id="48725-205">`Challenge`屬性表示 Www 驗證標頭。</span><span class="sxs-lookup"><span data-stu-id="48725-205">The `Challenge` property represents a Www-Authentication header.</span></span> <span data-ttu-id="48725-206">請注意， **ExecuteAsync**會先呼叫`InnerResult.ExecuteAsync`建立 HTTP 回應，如有需要然後加入所面臨的挑戰。</span><span class="sxs-lookup"><span data-stu-id="48725-206">Notice that **ExecuteAsync** first calls `InnerResult.ExecuteAsync` to create the HTTP response, and then adds the challenge if needed.</span></span>

<span data-ttu-id="48725-207">檢查回應程式碼之前加入所面臨的挑戰。</span><span class="sxs-lookup"><span data-stu-id="48725-207">Check the response code before adding the challenge.</span></span> <span data-ttu-id="48725-208">大部分的驗證配置只能新增一項挑戰是否回應 401，如下所示。</span><span class="sxs-lookup"><span data-stu-id="48725-208">Most authentication schemes only add a challenge if the response is 401, as shown here.</span></span> <span data-ttu-id="48725-209">不過，某些驗證配置進行加入一項挑戰的成功回應。</span><span class="sxs-lookup"><span data-stu-id="48725-209">However, some authentication schemes do add a challenge to a success response.</span></span> <span data-ttu-id="48725-210">例如，請參閱[交涉](http://tools.ietf.org/html/rfc4559#section-5)(RFC 4559)。</span><span class="sxs-lookup"><span data-stu-id="48725-210">For example, see [Negotiate](http://tools.ietf.org/html/rfc4559#section-5) (RFC 4559).</span></span>

<span data-ttu-id="48725-211">給定`AddChallengeOnUnauthorizedResult`類別中的實際程式碼**ChallengeAsync**很簡單。</span><span class="sxs-lookup"><span data-stu-id="48725-211">Given the `AddChallengeOnUnauthorizedResult` class, the actual code in **ChallengeAsync** is simple.</span></span> <span data-ttu-id="48725-212">您剛建立的結果，並將其附加至`context.Result`。</span><span class="sxs-lookup"><span data-stu-id="48725-212">You just create the result and attach it to `context.Result`.</span></span>

[!code-csharp[Main](authentication-filters/samples/sample9.cs)]

<span data-ttu-id="48725-213">注意： 基本驗證範例抽象化此邏輯位元，將其放在擴充方法。</span><span class="sxs-lookup"><span data-stu-id="48725-213">Note: The Basic Authentication sample abstracts this logic a bit, by placing it in an extension method.</span></span>

## <a name="combining-authentication-filters-with-host-level-authentication"></a><span data-ttu-id="48725-214">結合使用主機層級驗證的驗證篩選條件</span><span class="sxs-lookup"><span data-stu-id="48725-214">Combining Authentication Filters with Host-Level Authentication</span></span>

<span data-ttu-id="48725-215">[主控件層級驗證] 是由主應用程式 （例如 IIS) 中，執行驗證之前要求到達 Web API 架構。</span><span class="sxs-lookup"><span data-stu-id="48725-215">"Host-level authentication" is authentication performed by the host (such as IIS), before the request reaches the Web API framework.</span></span>

<span data-ttu-id="48725-216">通常，您可能要啟用您的應用程式的其餘部分的主機層級驗證，但停用您的 Web API 控制器。</span><span class="sxs-lookup"><span data-stu-id="48725-216">Often, you may want to enable host-level authentication for the rest of your application, but disable it for your Web API controllers.</span></span> <span data-ttu-id="48725-217">例如，典型的案例是啟用表單驗證，在主機層級，但使用 Web api 的權杖型驗證。</span><span class="sxs-lookup"><span data-stu-id="48725-217">For example, a typical scenario is to enable Forms Authentication at the host level, but use token-based authentication for Web API.</span></span>

<span data-ttu-id="48725-218">若要停用 Web API 管線內的主機層級驗證，請呼叫`config.SuppressHostPrincipal()`組態中。</span><span class="sxs-lookup"><span data-stu-id="48725-218">To disable host-level authentication inside the Web API pipeline, call `config.SuppressHostPrincipal()` in your configuration.</span></span> <span data-ttu-id="48725-219">這會導致 Web API，可移除**IPrincipal**從輸入 Web API 管線的任何要求。</span><span class="sxs-lookup"><span data-stu-id="48725-219">This causes Web API to remove the **IPrincipal** from any request that enters the Web API pipeline.</span></span> <span data-ttu-id="48725-220">實際上，它&quot;取消-驗證&quot;要求。</span><span class="sxs-lookup"><span data-stu-id="48725-220">Effectively, it &quot;un-authenticates&quot; the request.</span></span>

[!code-csharp[Main](authentication-filters/samples/sample10.cs)]

## <a name="additional-resources"></a><span data-ttu-id="48725-221">其他資源</span><span class="sxs-lookup"><span data-stu-id="48725-221">Additional Resources</span></span>

<span data-ttu-id="48725-222">[ASP.NET Web API 安全性篩選器](https://msdn.microsoft.com/magazine/dn781361.aspx)(MSDN Magazine)</span><span class="sxs-lookup"><span data-stu-id="48725-222">[ASP.NET Web API Security Filters](https://msdn.microsoft.com/magazine/dn781361.aspx) (MSDN Magazine)</span></span>
