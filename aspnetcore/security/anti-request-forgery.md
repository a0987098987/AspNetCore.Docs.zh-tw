---
title: 防止 ASP.NET Core 中的跨網站要求偽造（XSRF/CSRF）攻擊
author: steve-smith
description: 探索如何防範惡意網站可能會影響用戶端瀏覽器與應用程式之間互動的 web 應用程式攻擊。
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
uid: security/anti-request-forgery
ms.openlocfilehash: 54e153af55f28d9a89bbf16bce1c17f876567b59
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/06/2019
ms.locfileid: "74880799"
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a>防止 ASP.NET Core 中的跨網站要求偽造（XSRF/CSRF）攻擊

作者： [Rick Anderson](https://twitter.com/RickAndMSFT)、 [Fiyaz Hasan](https://twitter.com/FiyazBinHasan)和[Steve Smith](https://ardalis.com/)

跨網站偽造要求（也稱為 XSRF 或 CSRF）是對 web 裝載應用程式的攻擊，因此惡意的 web 應用程式可能會影響用戶端瀏覽器與信任該瀏覽器之 web 應用程式之間的互動。 這些攻擊是可行的，因為網頁瀏覽器會在每次要求網站時，自動傳送一些類型的驗證權杖。 這種形式的惡意探索也稱為單鍵*攻擊*或*會話騎*，因為攻擊會利用使用者先前驗證的會話。

CSRF 攻擊的範例：

1. 使用者使用表單驗證登入 `www.good-banking-site.com`。 伺服器會驗證使用者，併發出包含驗證 cookie 的回應。 網站很容易遭受攻擊，因為它會信任它以有效的驗證 cookie 接收的任何要求。
1. 使用者造訪惡意網站，`www.bad-crook-site.com`。

   惡意網站 `www.bad-crook-site.com`包含類似下面的 HTML 表單：

   ```html
   <h1>Congratulations! You're a Winner!</h1>
   <form action="http://good-banking-site.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw">
       <input type="hidden" name="Amount" value="1000000">
       <input type="submit" value="Click to collect your prize!">
   </form>
   ```

   請注意，表單的 `action` 張貼到易受攻擊的網站，而不是惡意網站。 這是 CSRF 的「跨網站」部分。

1. 使用者選取 [提交] 按鈕。 瀏覽器會提出要求，並自動包含要求之網域的驗證 cookie，`www.good-banking-site.com`。
1. 此要求會在 `www.good-banking-site.com` 伺服器上以使用者的驗證內容執行，並可執行已驗證使用者允許執行的任何動作。

除了使用者選取按鈕以提交表單的案例以外，惡意網站可能會：

* 執行自動提交表單的腳本。
* 傳送表單提交做為 AJAX 要求。
* 使用 CSS 隱藏表單。

這些替代案例不需要使用者一開始造訪惡意網站的任何動作或輸入。

使用 HTTPS 並不會防止 CSRF 攻擊。 惡意網站可以傳送 `https://www.good-banking-site.com/` 要求，就像傳送不安全的要求一樣簡單。

某些攻擊會以回應 GET 要求的端點為目標，在此情況下，可以使用影像標記來執行動作。 這種形式的攻擊在允許影像但封鎖 JavaScript 的論壇網站上很常見。 變更變數或資源的 GET 要求狀態的應用程式很容易遭受惡意攻擊。 **變更狀態的 GET 要求不安全。最佳做法是永遠不要變更 GET 要求的狀態。**

針對使用 cookie 進行驗證的 web 應用程式，可能會受到 CSRF 攻擊，因為：

* 瀏覽器會儲存 web 應用程式所發出的 cookie。
* 儲存的 cookie 包含已驗證使用者的會話 cookie。
* 無論應用程式在瀏覽器內產生的要求如何，瀏覽器都會將所有與網域相關聯的 cookie 傳送至 web 應用程式。

不過，CSRF 攻擊並不限於利用 cookie。 例如，基本和摘要式驗證也很容易受到攻擊。 使用者使用基本或摘要式驗證登入之後，瀏覽器會自動傳送認證，直到會話&dagger; 結束為止。

&dagger;在此內容中，*會話*是指驗證使用者的用戶端會話。 它與伺服器端會話或[ASP.NET Core 會話中介軟體](xref:fundamentals/app-state)無關。

使用者可以採取預防措施來防止 CSRF 的弱點：

* 完成使用 web 應用程式後，請將其登出。
* 定期清除瀏覽器 cookie。

不過，CSRF 弱點基本上是 web 應用程式的問題，而不是終端使用者。

## <a name="authentication-fundamentals"></a>驗證基本概念

以 Cookie 為基礎的驗證是一種常用的驗證形式。 以權杖為基礎的驗證系統日益普及，特別是針對單一頁面應用程式（Spa）。

### <a name="cookie-based-authentication"></a>以 Cookie 為基礎的驗證

當使用者使用其使用者名稱和密碼進行驗證時，就會發出權杖，其中包含可用於驗證和授權的驗證票證。 權杖會儲存為每個用戶端提出的要求所隨附的 cookie。 Cookie 驗證中介軟體會執行此 cookie 的產生和驗證。 [中介軟體](xref:fundamentals/middleware/index)會將使用者主體序列化為加密的 cookie。 在後續要求中，中介軟體會驗證 cookie、重新建立主體，並將主體指派給[HttpCoNtext](/dotnet/api/microsoft.aspnetcore.http.httpcontext)的[User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user)屬性。

### <a name="token-based-authentication"></a>以權杖為基礎的驗證

當使用者通過驗證時，就會發出權杖（不是 antiforgery token）。 權杖包含[宣告](/dotnet/framework/security/claims-based-identity-model)形式的使用者資訊或參考權杖，可將應用程式指向應用程式中維護的使用者狀態。 當使用者嘗試存取需要驗證的資源時，權杖會以持有人權杖的形式，以額外的授權標頭傳送至應用程式。 這會讓應用程式無狀態。 在每個後續要求中，會在要求中傳遞權杖以進行伺服器端驗證。 此權杖未*加密*;它會進行*編碼*。 在伺服器上，權杖會解碼以存取其資訊。 若要在後續要求中傳送權杖，請將權杖儲存在瀏覽器的本機儲存體中。 如果權杖儲存在瀏覽器的本機儲存體中，請不要擔心 CSRF 弱點。 當令牌儲存在 cookie 中時，CSRF 是一項考慮。 如需詳細資訊，請參閱 GitHub 問題[SPA 程式碼範例會新增兩個 cookie](https://github.com/aspnet/AspNetCore.Docs/issues/13369)。

### <a name="multiple-apps-hosted-at-one-domain"></a>裝載于一個網域的多個應用程式

共用的裝載環境容易遭受會話劫持、登入 CSRF 和其他攻擊。

雖然 `example1.contoso.net` 和 `example2.contoso.net` 是不同的主機，但 `*.contoso.net` 網域下的主機之間有隱含的信任關係。 此隱含信任關係允許可能不受信任的主機影響彼此的 cookie （控制 AJAX 要求的相同來源原則不一定會套用至 HTTP cookie）。

在相同網域上裝載的應用程式之間，惡意探索受信任 cookie 的攻擊，可以防止共用網域。 當每個應用程式裝載于它自己的網域時，就不會有隱含的 cookie 信任關係可以利用。

## <a name="aspnet-core-antiforgery-configuration"></a>ASP.NET Core antiforgery 設定

> [!WARNING]
> ASP.NET Core 使用 ASP.NET Core 的[資料保護](xref:security/data-protection/introduction)來執行 antiforgery。 資料保護堆疊必須設定為在伺服器陣列中工作。 如需詳細資訊，請參閱設定[資料保護](xref:security/data-protection/configuration/overview)。

::: moniker range=">= aspnetcore-3.0"

當 `Startup.ConfigureServices`中呼叫下列其中一個 Api 時，會將 Antiforgery 中介軟體新增至相依性[插入](xref:fundamentals/dependency-injection)容器：

* <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*>
* <xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapRazorPages*>
* <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*>
* <xref:Microsoft.AspNetCore.Builder.ComponentEndpointRouteBuilderExtensions.MapBlazorHub*>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

在中呼叫 <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> 時，Antiforgery 中介軟體會新增至相依性[插入](xref:fundamentals/dependency-injection)容器 `Startup.ConfigureServices`

::: moniker-end

在 ASP.NET Core 2.0 或更新版本中， [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper)會將 antiforgery TOKEN 插入 HTML 表單元素中。 Razor 檔案中的下列標記會自動產生 antiforgery 權杖：

```cshtml
<form method="post">
    ...
</form>
```

同樣地，如果無法取得表單的方法，則[IHtmlHelper](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform)會依預設產生 antiforgery token。

當 `<form>` 標籤包含 `method="post"` 屬性，而且下列其中一項為 true 時，就會自動產生 HTML 表單元素的 antiforgery token：

* Action 屬性是空的（`action=""`）。
* 未提供動作屬性（`<form method="post">`）。

可以停用 HTML 表單元素的自動產生 antiforgery token：

* 明確停用具有 `asp-antiforgery` 屬性的 antiforgery token：

  ```cshtml
  <form method="post" asp-antiforgery="false">
      ...
  </form>
  ```

* Form 元素會使用標籤協助程式[！退出符號](xref:mvc/views/tag-helpers/intro#opt-out)來退出宣告標記協助程式：

  ```cshtml
  <!form method="post">
      ...
  </!form>
  ```

* 從視圖中移除 `FormTagHelper`。 將下列指示詞新增至 Razor 視圖，即可從視圖中移除 `FormTagHelper`：

  ```cshtml
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> [Razor Pages](xref:razor-pages/index)會自動受到 XSRF/CSRF 的保護。 如需詳細資訊，請參閱[XSRF/CSRF 和 Razor Pages](xref:razor-pages/index#xsrf)。

防禦 CSRF 攻擊最常見的方法是使用*同步器權杖模式*（STP）。 當使用者要求具有表單資料的頁面時，會使用 STP：

1. 伺服器會將與目前使用者的身分識別相關聯的權杖傳送給用戶端。
1. 用戶端會將權杖傳回給伺服器以進行驗證。
1. 如果伺服器收到的權杖不符合已驗證使用者的身分識別，則會拒絕該要求。

Token 是唯一且無法預測的。 權杖也可以用來確保一系列要求的適當排序（例如，確保要求的順序：第1頁 &ndash; 第2頁 &ndash; 第3頁）。 ASP.NET Core MVC 和 Razor Pages 範本中的所有表單都會產生 antiforgery token。 下列對視圖範例會產生 antiforgery token：

```cshtml
<form asp-controller="Manage" asp-action="ChangePassword" method="post">
    ...
</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    ...
}
```

將 antiforgery token 明確新增至 `<form>` 專案，而不使用標記協助程式搭配 HTML helper [`@Html.AntiForgeryToken`](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken)：

```cshtml
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

在上述每個案例中，ASP.NET Core 新增如下所示的隱藏表單欄位：

```cshtml
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkS ... s2-m9Yw">
```

ASP.NET Core 包含三個用於處理 antiforgery token 的[篩選](xref:mvc/controllers/filters)條件：

* [ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)
* [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)
* [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)

## <a name="antiforgery-options"></a>Antiforgery 選項

在 `Startup.ConfigureServices`中自訂[antiforgery 選項](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions)：

::: moniker range=">= aspnetcore-2.0"

```csharp
services.AddAntiforgery(options => 
{
    // Set Cookie properties using CookieBuilder properties†.
    options.FormFieldName = "AntiforgeryFieldname";
    options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
    options.SuppressXFrameOptionsHeader = false;
});
```

&dagger;使用[CookieBuilder](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder)類別的屬性來設定 antiforgery `Cookie` 屬性。

| 選項 | 描述 |
| ------ | ----------- |
| [Cookie](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | 決定用來建立 antiforgery cookie 的設定。 |
| [FormFieldName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | Antiforgery 系統用來轉譯 views 中 antiforgery 標記的隱藏表單欄位名稱。 |
| [HeaderName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | Antiforgery 系統使用的標頭名稱。 如果 `null`，系統只會考慮表單資料。 |
| [SuppressXFrameOptionsHeader](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | 指定是否要隱藏 `X-Frame-Options` 標頭的產生。 根據預設，會產生值為 "SAMEORIGIN" 的標頭。 預設值為 `false`。 |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
services.AddAntiforgery(options => 
{
    options.CookieDomain = "contoso.com";
    options.CookieName = "X-CSRF-TOKEN-COOKIENAME";
    options.CookiePath = "Path";
    options.FormFieldName = "AntiforgeryFieldname";
    options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
    options.RequireSsl = false;
    options.SuppressXFrameOptionsHeader = false;
});
```

| 選項 | 描述 |
| ------ | ----------- |
| [Cookie](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | 決定用來建立 antiforgery cookie 的設定。 |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiedomain) | Cookie 的網域值。 預設值為 `null`。 這個屬性已經過時，將在未來的版本中移除。 建議的替代做法是 [Cookie. 網域]。 |
| [CookieName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiename) | Cookie 的名稱。 如果未設定，系統會產生以[DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) （"開頭的唯一名稱。AspNetCore. Antiforgery. "）。 這個屬性已經過時，將在未來的版本中移除。 建議的替代做法是 Cookie.Name。 |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiepath) | Cookie 上設定的路徑。 這個屬性已經過時，將在未來的版本中移除。 建議的替代做法是 [Cookie. 路徑]。 |
| [FormFieldName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | Antiforgery 系統用來轉譯 views 中 antiforgery 標記的隱藏表單欄位名稱。 |
| [HeaderName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | Antiforgery 系統使用的標頭名稱。 如果 `null`，系統只會考慮表單資料。 |
| [RequireSsl](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.requiressl) | 指定 antiforgery 系統是否需要 HTTPS。 如果 `true`，則非 HTTPS 要求會失敗。 預設值為 `false`。 這個屬性已經過時，將在未來的版本中移除。 建議的替代做法是設定 Cookie. SecurePolicy。 |
| [SuppressXFrameOptionsHeader](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | 指定是否要隱藏 `X-Frame-Options` 標頭的產生。 根據預設，會產生值為 "SAMEORIGIN" 的標頭。 預設值為 `false`。 |

::: moniker-end

如需詳細資訊，請參閱[cookieauthenticationoptions.authenticationtype](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions)。

## <a name="configure-antiforgery-features-with-iantiforgery"></a>使用 IAntiforgery 設定 antiforgery 功能

[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery)提供 API 來設定 antiforgery 功能。 `IAntiforgery` 可以在 `Startup` 類別的 `Configure` 方法中要求。 下列範例會使用來自應用程式首頁的中介軟體來產生 antiforgery token，並在回應中將它當做 cookie 傳送（使用本主題稍後所述的預設角度命名慣例）：

```csharp
public void Configure(IApplicationBuilder app, IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;

        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // The request token can be sent as a JavaScript-readable cookie, 
            // and Angular uses it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
}
```

### <a name="require-antiforgery-validation"></a>需要 antiforgery 驗證

[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)是可套用至個別動作、控制器或全域的動作篩選準則。 除非要求包含有效的 antiforgery token，否則會封鎖已套用此篩選之動作的要求。

```csharp
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> RemoveLogin(RemoveLoginViewModel account)
{
    ManageMessageId? message = ManageMessageId.Error;
    var user = await GetCurrentUserAsync();

    if (user != null)
    {
        var result = 
            await _userManager.RemoveLoginAsync(
                user, account.LoginProvider, account.ProviderKey);

        if (result.Succeeded)
        {
            await _signInManager.SignInAsync(user, isPersistent: false);
            message = ManageMessageId.RemoveLoginSuccess;
        }
    }

    return RedirectToAction(nameof(ManageLogins), new { Message = message });
}
```

`ValidateAntiForgeryToken` 屬性需要對其所標記之動作方法的要求使用權杖，包括 HTTP GET 要求。 如果 `ValidateAntiForgeryToken` 屬性會套用到應用程式的控制器，則可以使用 `IgnoreAntiforgeryToken` 屬性加以覆寫。

> [!NOTE]
> ASP.NET Core 不支援自動新增 antiforgery token 來取得要求。

### <a name="automatically-validate-antiforgery-tokens-for-unsafe-http-methods-only"></a>僅針對 unsafe HTTP 方法自動驗證 antiforgery 權杖

ASP.NET Core 應用程式不會為安全的 HTTP 方法（GET、HEAD、OPTIONS 和 TRACE）產生 antiforgery token。 您可以使用[AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)屬性，而不是廣泛套用 `ValidateAntiForgeryToken` 屬性，然後使用 `IgnoreAntiforgeryToken` 屬性來覆寫它。 這個屬性的運作方式與 `ValidateAntiForgeryToken` 屬性相同，不同之處在于它不需要使用下列 HTTP 方法所提出之要求的權杖：

* GET
* HEAD
* 選項
* TRACE

我們建議您針對非 API 案例廣泛使用 `AutoValidateAntiforgeryToken`。 這可確保預設會保護 POST 動作。 替代方式是預設忽略 antiforgery token，除非將 `ValidateAntiForgeryToken` 套用至個別的動作方法。 在此案例中，較有可能不小心將 POST 動作方法保留為未受保護，讓應用程式容易遭受 CSRF 攻擊。 所有貼文都應該傳送 antiforgery token。

Api 沒有自動機制來傳送權杖的非 cookie 部分。 執行可能取決於用戶端程式代碼的執行。 以下顯示一些範例：

類別層級範例：

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

全域範例：

```csharp
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

### <a name="override-global-or-controller-antiforgery-attributes"></a>覆寫全域或控制器 antiforgery 屬性

[IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)篩選器是用來消除指定動作（或控制器）的 antiforgery token 需求。 套用時，此篩選會覆寫在較高層級指定的 `ValidateAntiForgeryToken` 和 `AutoValidateAntiforgeryToken` 篩選（全域或在控制器上）。

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
    [HttpPost]
    [IgnoreAntiforgeryToken]
    public async Task<IActionResult> DoSomethingSafe(SomeViewModel model)
    {
        // no antiforgery token required
    }
}
```

## <a name="refresh-tokens-after-authentication"></a>驗證後重新整理權杖

使用者通過驗證之後，應該重新整理權杖，方法是將使用者重新導向至 view 或 Razor Pages 頁面。

## <a name="javascript-ajax-and-spas"></a>JavaScript、AJAX 和 Spa

在傳統的 HTML 架構應用程式中，antiforgery 權杖會使用隱藏的表單欄位來傳遞至伺服器。 在以 JavaScript 為基礎的新式應用程式和 Spa 中，許多要求都是以程式設計方式進行。 這些 AJAX 要求可能會使用其他技術（例如要求標頭或 cookie）來傳送權杖。

如果使用 cookie 來儲存驗證權杖，並在伺服器上驗證 API 要求，則 CSRF 可能會發生問題。 如果使用本機儲存體來儲存權杖，可能會降低 CSRF 弱點，因為本機儲存體中的值不會隨每個要求自動傳送到伺服器。 因此，使用本機儲存體將 antiforgery token 儲存在用戶端上，並以要求標頭的形式傳送權杖是建議的方法。

### <a name="javascript"></a>JavaScript

使用 JavaScript 搭配 views，可以從視圖內使用服務來建立權杖。 將[AspNetCore Antiforgery. IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery)服務插入至視圖，並呼叫[GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens)：

[!code-csharp[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,35-36)]

這種方法不需要直接處理從伺服器設定 cookie，或從用戶端讀取 cookie。

上述範例會使用 JavaScript 來讀取 AJAX POST 標頭的隱藏欄位值。

JavaScript 也可以存取 cookie 中的權杖，並使用 cookie 的內容來建立具有權杖值的標頭。

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
    new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

假設腳本要求在名為 `X-CSRF-TOKEN`的標頭中傳送權杖，請將 antiforgery 服務設定為尋找 `X-CSRF-TOKEN` 標頭：

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

下列範例會使用 JavaScript，以適當的標頭建立 AJAX 要求：

```javascript
function getCookie(cname) {
    var name = cname + "=";
    var decodedCookie = decodeURIComponent(document.cookie);
    var ca = decodedCookie.split(';');
    for(var i = 0; i <ca.length; i++) {
        var c = ca[i];
        while (c.charAt(0) == ' ') {
            c = c.substring(1);
        }
        if (c.indexOf(name) == 0) {
            return c.substring(name.length, c.length);
        }
    }
    return "";
}

var csrfToken = getCookie("CSRF-TOKEN");

var xhttp = new XMLHttpRequest();
xhttp.onreadystatechange = function() {
    if (xhttp.readyState == XMLHttpRequest.DONE) {
        if (xhttp.status == 200) {
            alert(xhttp.responseText);
        } else {
            alert('There was an error processing the AJAX request.');
        }
    }
};
xhttp.open('POST', '/api/password/changepassword', true);
xhttp.setRequestHeader("Content-type", "application/json");
xhttp.setRequestHeader("X-CSRF-TOKEN", csrfToken);
xhttp.send(JSON.stringify({ "newPassword": "ReallySecurePassword999$$$" }));
```

### <a name="angularjs"></a>AngularJS

AngularJS 會使用慣例來定址 CSRF。 如果伺服器傳送名稱為 `XSRF-TOKEN`的 cookie，則 AngularJS `$http` 服務會在將要求傳送至伺服器時，將 cookie 值新增至標頭。 此程式是自動的。 不需要明確地在用戶端中設定標頭。 標頭名稱為 `X-XSRF-TOKEN`。 伺服器應該會偵測到此標頭並驗證其內容。

若要讓 ASP.NET Core API 在應用程式啟動時使用此慣例：

* 設定您的應用程式，以在稱為 `XSRF-TOKEN`的 cookie 中提供權杖。
* 設定 antiforgery 服務以尋找名為 `X-XSRF-TOKEN`的標頭。

```csharp
public void Configure(IApplicationBuilder app, IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;

        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // The request token can be sent as a JavaScript-readable cookie, 
            // and Angular uses it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
}

public void ConfigureServices(IServiceCollection services)
{
    // Angular's default header name for sending the XSRF token.
    services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
}
```

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="extend-antiforgery"></a>擴充 antiforgery

[IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider)類型可讓開發人員在每個權杖中來回往返額外的資料，以擴充反 CSRF 系統的行為。 每次產生欄位標記時，會呼叫[GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata)方法，而傳回值會內嵌在產生的權杖中。 實施者可以傳回時間戳記、nonce 或任何其他值，然後在驗證權杖時呼叫[ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata)來驗證此資料。 用戶端的使用者名稱已內嵌在產生的權杖中，因此不需要包含這項資訊。 如果權杖包含補充資料，但未設定 `IAntiForgeryAdditionalDataProvider`，則不會驗證補充資料。

## <a name="additional-resources"></a>其他資源

* [開啟 Web 應用程式安全性專案](https://www.owasp.org/index.php/Main_Page)的[CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) （OWASP）。
* <xref:host-and-deploy/web-farm>
