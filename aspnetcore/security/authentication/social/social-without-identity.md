---
title: Facebook、 Google 及外部提供者驗證沒有 ASP.NET Core 身分識別
author: rick-anderson
description: 使用 Facebook、 Google、 Twitter 等帳戶使用者驗證，而不需要 ASP.NET Core 身分識別的說明。
ms.author: riande
ms.date: 07/04/2019
uid: security/authentication/social/social-without-identity
ms.openlocfilehash: 1e7124e8b07c0faf2d005ec3ef55c0414a697d64
ms.sourcegitcommit: f6e6730872a7d6f039f97d1df762f0d0bd5e34cf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/04/2019
ms.locfileid: "67561573"
---
# <a name="use-social-sign-in-provider-authentication-without-aspnet-core-identity"></a>使用沒有 ASP.NET Core Identity 的社交登入提供者驗證

<xref:security/authentication/social/index> 描述如何讓使用者能夠登入使用 OAuth 2.0 搭配來自外部驗證提供者的認證。 該主題中所述的方法包括 ASP.NET Core 身分識別做為驗證提供者。

這個範例會示範如何使用外部驗證提供者**而不需要**ASP.NET Core 身分識別。 這是適用於不需要的所有功能的 ASP.NET Core 身分識別，但仍需要與受信任的外部驗證提供者整合的應用程式。

這個範例會使用[Google 驗證](xref:security/authentication/google-logins)來驗證使用者。 使用 Google 驗證會轉移許多管理 Google 的登入程序的複雜性。 若要使用不同的外部驗證提供者整合，請參閱下列主題：

* [Facebook 驗證](xref:security/authentication/facebook-logins)
* [Microsoft 驗證](xref:security/authentication/microsoft-logins)
* [Twitter 驗證](xref:security/authentication/twitter-logins)
* [其他提供者](xref:security/authentication/otherlogins)

## <a name="configuration"></a>組態

在 `ConfigureServices`方法，設定使用的應用程式的驗證配置`AddAuthentication`，`AddCookie`和`AddGoogle`方法：

[!code-csharp[](social-without-identity/sample/Startup.cs?name=snippet1)]

若要在呼叫[AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__)設定應用程式的[DefaultScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme)。 `DefaultScheme`是由下列的預設配置`HttpContext`驗證的擴充方法：

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

設定應用程式的`DefaultScheme`要[CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookie") 會使用 Cookie 做為預設配置，這些擴充方法的應用程式設定。 設定應用程式的<xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme>要[GoogleDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) (「 Google 」) 會設定應用程式使用 Google 做為預設配置，針對呼叫`ChallengeAsync`。 `DefaultChallengeScheme` 覆寫`DefaultScheme`。 請參閱<xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions>覆寫的其他屬性`DefaultScheme`時設定。

在 `Configure`方法中，呼叫`UseAuthentication`方法來叫用設定驗證中介軟體`HttpContext.User`屬性。 呼叫`UseAuthentication`方法之前呼叫`UseMvcWithDefaultRoute`或`UseMvc`:

[!code-csharp[](social-without-identity/sample/Startup.cs?name=snippet2)]

若要深入了解驗證配置和 cookie 驗證，請參閱<xref:security/authentication/cookie>。

## <a name="applying-authorization"></a>套用授權

測試應用程式的驗證設定，藉由套用`AuthorizeAttribute`屬性加入控制器、 動作或頁面。 下列程式碼會限制存取權*隱私權*給已驗證的使用者 頁面：

[!code-csharp[](social-without-identity/sample/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a>登出

若要將目前的使用者登出並刪除其 cookie，呼叫[SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0)。 下列程式碼加入`Logout`頁面處理常式*Index*頁面：

[!code-csharp[](social-without-identity/sample/Pages/Index.cshtml.cs?name=snippet&highlight=7-11)]

請注意，呼叫`SignOutAsync`未指定驗證配置。 應用程式的`DefaultScheme`的`CookieAuthenticationDefaults.AuthenticationScheme`改為使用。

## <a name="additional-resources"></a>其他資源

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>
