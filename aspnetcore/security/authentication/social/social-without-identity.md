---
title: Facebook、 Google 及外部提供者驗證沒有 ASP.NET Core 身分識別
author: rick-anderson
description: 使用 Facebook、 Google、 Twitter 等帳戶使用者驗證，而不需要 ASP.NET Core 身分識別的說明。
ms.author: riande
ms.date: 07/04/2019
uid: security/authentication/social/social-without-identity
ms.openlocfilehash: e67da513fef1ce453110c465b08e9c7965e71df5
ms.sourcegitcommit: d6e51c60439f03a8992bda70cc982ddb15d3f100
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2019
ms.locfileid: "67557647"
---
# <a name="use-social-sign-in-provider-authentication-without-aspnet-core-identity"></a><span data-ttu-id="4be30-103">使用沒有 ASP.NET Core Identity 的社交登入提供者驗證</span><span class="sxs-lookup"><span data-stu-id="4be30-103">Use social sign-in provider authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="4be30-104"><xref:security/authentication/social/index> 描述如何讓使用者能夠登入使用 OAuth 2.0 搭配來自外部驗證提供者的認證。</span><span class="sxs-lookup"><span data-stu-id="4be30-104"><xref:security/authentication/social/index> describes how to enable users to sign in using OAuth 2.0 with credentials from external authentication providers.</span></span> <span data-ttu-id="4be30-105">該主題中所述的方法包括 ASP.NET Core 身分識別做為驗證提供者。</span><span class="sxs-lookup"><span data-stu-id="4be30-105">The approach described in that topic includes ASP.NET Core Identity as an authentication provider.</span></span>

<span data-ttu-id="4be30-106">這個範例會示範如何使用外部驗證提供者**而不需要**ASP.NET Core 身分識別。</span><span class="sxs-lookup"><span data-stu-id="4be30-106">This sample demonstrates how to use an external authentication provider **without** ASP.NET Core Identity.</span></span> <span data-ttu-id="4be30-107">這是適用於不需要的所有功能的 ASP.NET Core 身分識別，但仍需要與受信任的外部驗證提供者整合的應用程式。</span><span class="sxs-lookup"><span data-stu-id="4be30-107">This is useful for apps that don't require all of the features of ASP.NET Core Identity, but still require integration with a trusted external authentication provider.</span></span>

<span data-ttu-id="4be30-108">這個範例會使用[Google 驗證](xref:security/authentication/google-logins)來驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="4be30-108">This sample uses [Google authentication](xref:security/authentication/google-logins) for authenticating users.</span></span> <span data-ttu-id="4be30-109">使用 Google 驗證會轉移許多管理 Google 的登入程序的複雜性。</span><span class="sxs-lookup"><span data-stu-id="4be30-109">Using Google authentication shifts many of the complexities of managing the sign-in process to Google.</span></span> <span data-ttu-id="4be30-110">若要使用不同的外部驗證提供者整合，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="4be30-110">To integrate with a different external authentication provider, see the following topics:</span></span>

* [<span data-ttu-id="4be30-111">Facebook 驗證</span><span class="sxs-lookup"><span data-stu-id="4be30-111">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="4be30-112">Microsoft 驗證</span><span class="sxs-lookup"><span data-stu-id="4be30-112">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="4be30-113">Twitter 驗證</span><span class="sxs-lookup"><span data-stu-id="4be30-113">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="4be30-114">其他提供者</span><span class="sxs-lookup"><span data-stu-id="4be30-114">Other providers</span></span>](xref:security/authentication/otherlogins)

## <a name="configuration"></a><span data-ttu-id="4be30-115">組態</span><span class="sxs-lookup"><span data-stu-id="4be30-115">Configuration</span></span>

<span data-ttu-id="4be30-116">在 `ConfigureServices`方法，設定使用的應用程式的驗證配置`AddAuthentication`，`AddCookie`和`AddGoogle`方法：</span><span class="sxs-lookup"><span data-stu-id="4be30-116">In the `ConfigureServices` method, configure the app's authentication schemes with the `AddAuthentication`, `AddCookie` and `AddGoogle` methods:</span></span>

[!code-csharp[](social-without-identity/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="4be30-117">若要在呼叫[AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__)設定應用程式的[DefaultScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme)。</span><span class="sxs-lookup"><span data-stu-id="4be30-117">The call to [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) sets the app's [DefaultScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme).</span></span> <span data-ttu-id="4be30-118">`DefaultScheme`是由下列的預設配置`HttpContext`驗證的擴充方法：</span><span class="sxs-lookup"><span data-stu-id="4be30-118">The `DefaultScheme` is the default scheme used by the following `HttpContext` authentication extension methods:</span></span>

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

<span data-ttu-id="4be30-119">設定應用程式的`DefaultScheme`要[CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookie") 會使用 Cookie 做為預設配置，這些擴充方法的應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="4be30-119">Setting the app's `DefaultScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookies") configures the app to use Cookies as the default scheme for these extension methods.</span></span> <span data-ttu-id="4be30-120">設定應用程式的<xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme>要[GoogleDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) (「 Google 」) 會設定應用程式使用 Google 做為預設配置，針對呼叫`ChallengeAsync`。</span><span class="sxs-lookup"><span data-stu-id="4be30-120">Setting the app's <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> to [GoogleDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google") configures the app to use Google as the default scheme for calls to `ChallengeAsync`.</span></span> <span data-ttu-id="4be30-121">`DefaultChallengeScheme` 覆寫`DefaultScheme`。</span><span class="sxs-lookup"><span data-stu-id="4be30-121">`DefaultChallengeScheme` overrides `DefaultScheme`.</span></span> <span data-ttu-id="4be30-122">請參閱<xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions>覆寫的其他屬性`DefaultScheme`時設定。</span><span class="sxs-lookup"><span data-stu-id="4be30-122">See <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> for additional properties that override `DefaultScheme` when set.</span></span>

<span data-ttu-id="4be30-123">在 `Configure`方法中，呼叫`UseAuthentication`方法來叫用設定驗證中介軟體`HttpContext.User`屬性。</span><span class="sxs-lookup"><span data-stu-id="4be30-123">In the `Configure` method, call the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="4be30-124">呼叫`UseAuthentication`方法之前呼叫`UseMvcWithDefaultRoute`或`UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="4be30-124">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](social-without-identity/sample/Startup.cs?name=snippet2)]

<span data-ttu-id="4be30-125">若要深入了解驗證配置和 cookie 驗證，請參閱<xref:security/authentication/cookie>。</span><span class="sxs-lookup"><span data-stu-id="4be30-125">To learn more about authentication schemes and cookie authentication, see <xref:security/authentication/cookie>.</span></span>

## <a name="applying-basic-authorization"></a><span data-ttu-id="4be30-126">套用基本授權</span><span class="sxs-lookup"><span data-stu-id="4be30-126">Applying basic authorization</span></span>

<span data-ttu-id="4be30-127">測試應用程式的驗證設定，藉由套用`AuthorizeAttribute`屬性加入控制器、 動作或頁面。</span><span class="sxs-lookup"><span data-stu-id="4be30-127">Test the app's authentication configuration by applying the `AuthorizeAttribute` attribute to a controller, action, or page.</span></span> <span data-ttu-id="4be30-128">下列程式碼會限制存取權*隱私權*給已驗證的使用者 頁面：</span><span class="sxs-lookup"><span data-stu-id="4be30-128">The following code limits access to the *Privacy* page to users that have been authenticated:</span></span>

[!code-csharp[](social-without-identity/sample/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a><span data-ttu-id="4be30-129">登出</span><span class="sxs-lookup"><span data-stu-id="4be30-129">Sign out</span></span>

<span data-ttu-id="4be30-130">若要將目前的使用者登出並刪除其 cookie，呼叫[SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0)。</span><span class="sxs-lookup"><span data-stu-id="4be30-130">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0).</span></span> <span data-ttu-id="4be30-131">下列程式碼加入`Logout`頁面處理常式*Index*頁面：</span><span class="sxs-lookup"><span data-stu-id="4be30-131">The following code adds a `Logout` page handler to the *Index* page:</span></span>

[!code-csharp[](social-without-identity/sample/Pages/Index.cshtml.cs?name=snippet&highlight=7-11)]

<span data-ttu-id="4be30-132">請注意，呼叫`SignOutAsync`未指定驗證配置。</span><span class="sxs-lookup"><span data-stu-id="4be30-132">Notice that the call to `SignOutAsync` does not specify an authentication scheme.</span></span> <span data-ttu-id="4be30-133">應用程式的`DefaultScheme`的`CookieAuthenticationDefaults.AuthenticationScheme`改為使用。</span><span class="sxs-lookup"><span data-stu-id="4be30-133">The app's `DefaultScheme` of `CookieAuthenticationDefaults.AuthenticationScheme` is used as a fall back.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4be30-134">其他資源</span><span class="sxs-lookup"><span data-stu-id="4be30-134">Additional resources</span></span>

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>
