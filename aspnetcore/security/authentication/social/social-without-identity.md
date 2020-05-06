---
title: 不 ASP.NET Core 的 Facebook、Google 及外部提供者驗證Identity
author: rick-anderson
description: 使用 Facebook、Google、Twitter 等的說明，而不 ASP.NET Core Identity的帳戶使用者驗證。
ms.author: riande
ms.date: 12/10/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authentication/social/social-without-identity
ms.openlocfilehash: cc44eb83947540ca9a5a04ffad4fdb8522fab26a
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82775735"
---
# <a name="use-social-sign-in-provider-authentication-without-aspnet-core-identity"></a>使用不 ASP.NET Core 的社交登入提供者驗證Identity

By [Kirk Larkin](https://twitter.com/serpent5)和[Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

<xref:security/authentication/social/index>說明如何讓使用者使用 OAuth 2.0 搭配外部驗證提供者的認證來登入。 該主題中所述的方法包含Identity ASP.NET Core 做為驗證提供者。

這個範例會示範如何在不 ASP.NET Core Identity的**情況下**使用外部驗證提供者。 這適用于不需要所有 ASP.NET Core Identity功能的應用程式，但仍需要與信任的外部驗證提供者整合。

這個範例會使用[Google 驗證](xref:security/authentication/google-logins)來驗證使用者。 使用 Google authentication 會將管理登入程式的許多複雜性轉移到 Google。 若要與不同的外部驗證提供者整合，請參閱下列主題：

* [Facebook 驗證](xref:security/authentication/facebook-logins)
* [Microsoft 驗證](xref:security/authentication/microsoft-logins)
* [Twitter 驗證](xref:security/authentication/twitter-logins)
* [其他提供者](xref:security/authentication/otherlogins)

## <a name="configuration"></a>設定

在`ConfigureServices`方法中，使用<xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*>、 <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>和<xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*>方法來設定應用程式的驗證配置：

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Startup.cs?name=snippet1)]

的呼叫會<xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*>設定應用程式的<xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme>。 `DefaultScheme`是下列`HttpContext`驗證擴充方法所使用的預設配置：

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

將應用程式的`DefaultScheme`設定為[CookieAuthenticationDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) （"Cookies"），會將應用程式設為使用 cookie 做為這些擴充方法的預設配置。 將應用程式的<xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme>設為[GoogleDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) （"Google"），會將應用程式設定為使用 Google 做為呼叫`ChallengeAsync`的預設配置。 `DefaultChallengeScheme`覆`DefaultScheme`寫。 如<xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions>需在設定時覆`DefaultScheme`寫的其他屬性，請參閱。

在`Startup.Configure`中， `UseAuthentication`在`UseAuthorization`呼叫`UseRouting`和`UseEndpoints`之間呼叫和。 這會設定`HttpContext.User`屬性，並執行要求的授權中介軟體：

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Startup.cs?name=snippet2&highlight=3-4)]

若要深入瞭解驗證配置，請參閱[驗證概念](xref:security/authentication/index#authentication-concepts)。 若要深入瞭解 cookie 驗證，請<xref:security/authentication/cookie>參閱。

## <a name="apply-authorization"></a>套用授權

藉由將`AuthorizeAttribute`屬性套用至控制器、動作或頁面，測試應用程式的驗證設定。 下列程式碼會將*隱私權*頁面的存取限制為已驗證的使用者：

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a>登出

若要登出目前的使用者並刪除其 cookie，請呼叫[SignOutAsync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*)。 下列程式碼會將`Logout`頁面處理常式新增至 [*索引*] 頁面：

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Pages/Index.cshtml.cs?name=snippet&highlight=3-7)]

請注意，對的`SignOutAsync`呼叫不會指定驗證配置。 的應用程式`DefaultScheme` `CookieAuthenticationDefaults.AuthenticationScheme`會用來做為回復。

## <a name="additional-resources"></a>其他資源

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>

::: moniker-end
::: moniker range="< aspnetcore-3.0"

<xref:security/authentication/social/index>說明如何讓使用者使用 OAuth 2.0 搭配外部驗證提供者的認證來登入。 該主題中所述的方法包含Identity ASP.NET Core 做為驗證提供者。

這個範例會示範如何在不 ASP.NET Core Identity的**情況下**使用外部驗證提供者。 這適用于不需要所有 ASP.NET Core Identity功能的應用程式，但仍需要與信任的外部驗證提供者整合。

這個範例會使用[Google 驗證](xref:security/authentication/google-logins)來驗證使用者。 使用 Google authentication 會將管理登入程式的許多複雜性轉移到 Google。 若要與不同的外部驗證提供者整合，請參閱下列主題：

* [Facebook 驗證](xref:security/authentication/facebook-logins)
* [Microsoft 驗證](xref:security/authentication/microsoft-logins)
* [Twitter 驗證](xref:security/authentication/twitter-logins)
* [其他提供者](xref:security/authentication/otherlogins)

## <a name="configuration"></a>設定

在`ConfigureServices`方法中，使用`AddAuthentication`、 `AddCookie`和`AddGoogle`方法來設定應用程式的驗證配置：

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Startup.cs?name=snippet1)]

對[AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__)的呼叫會設定應用程式的[DefaultScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme)。 `DefaultScheme`是下列`HttpContext`驗證擴充方法所使用的預設配置：

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

將應用程式的`DefaultScheme`設定為[CookieAuthenticationDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) （"Cookies"），會將應用程式設為使用 cookie 做為這些擴充方法的預設配置。 將應用程式的<xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme>設為[GoogleDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) （"Google"），會將應用程式設定為使用 Google 做為呼叫`ChallengeAsync`的預設配置。 `DefaultChallengeScheme`覆`DefaultScheme`寫。 如<xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions>需在設定時覆`DefaultScheme`寫的其他屬性，請參閱。

在`Configure`方法中，呼叫`UseAuthentication`方法來叫用設定`HttpContext.User`屬性的驗證中介軟體。 呼叫`UseAuthentication` `UseMvcWithDefaultRoute`或之前，請`UseMvc`先呼叫方法：

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Startup.cs?name=snippet2)]

若要深入瞭解驗證配置，請參閱[驗證概念](xref:security/authentication/index#authentication-concepts)。 若要深入瞭解 cookie 驗證，請<xref:security/authentication/cookie>參閱。

## <a name="apply-authorization"></a>套用授權

藉由將`AuthorizeAttribute`屬性套用至控制器、動作或頁面，測試應用程式的驗證設定。 下列程式碼會將*隱私權*頁面的存取限制為已驗證的使用者：

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a>登出

若要登出目前的使用者並刪除其 cookie，請呼叫[SignOutAsync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*)。 下列程式碼會將`Logout`頁面處理常式新增至 [*索引*] 頁面：

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Pages/Index.cshtml.cs?name=snippet&highlight=3-7)]

請注意，對的`SignOutAsync`呼叫不會指定驗證配置。 的應用程式`DefaultScheme` `CookieAuthenticationDefaults.AuthenticationScheme`會用來做為回復。

## <a name="additional-resources"></a>其他資源

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>

::: moniker-end
