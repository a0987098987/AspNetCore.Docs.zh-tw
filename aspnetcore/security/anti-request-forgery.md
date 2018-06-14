---
title: 防止跨站台要求偽造 (XSRF/CSRF) 攻擊，在 ASP.NET Core
author: steve-smith
description: 了解如何防止攻擊，其中惡意網站可能會影響用戶端瀏覽器和應用程式之間的互動的 web 應用程式。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/19/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/anti-request-forgery
ms.openlocfilehash: 3bca96f4a2e247eeeb93140df93221371d88d4d3
ms.sourcegitcommit: 7e87671fea9a5f36ca516616fe3b40b537f428d2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/12/2018
ms.locfileid: "35341856"
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a>防止跨站台要求偽造 (XSRF/CSRF) 攻擊，在 ASP.NET Core

由[Steve Smith](https://ardalis.com/)， [Fiyaz Hasan](https://twitter.com/FiyazBinHasan)，和[Rick Anderson](https://twitter.com/RickAndMSFT)

跨站台要求偽造 (也稱為 XSRF 或 CSRF，唸成 *，請參閱衝浪*) 是 web 裝載的應用程式，惡意的 web 應用程式可能會影響用戶端瀏覽器和信任的 web 應用程式之間的互動攻擊瀏覽器。 這些攻擊可能會因為網頁瀏覽器傳送到網站的某些類型的驗證權杖會自動與每個要求。 這種攻擊形式就是所謂*單鍵攻擊*或*工作階段乘載*使用者因為攻擊利用先前的驗證工作階段。

CSRF 攻擊的範例：

1. 使用者登入`www.good-banking-site.com`使用表單驗證。 伺服器會驗證使用者，並會發出包含驗證 cookie 的回應。 站台是很容易遭受攻擊，因為它信任它會接收一個有效的驗證 cookie 的任何要求。
1. 使用者造訪惡意網站， `www.bad-crook-site.com`。

   惡意的站台， `www.bad-crook-site.com`，包含 HTML 表單與下列類似：

   ```html
   <h1>Congratulations! You're a Winner!</h1>
   <form action="http://good-banking-site.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw">
       <input type="hidden" name="Amount" value="1000000">
       <input type="submit" value="Click to collect your prize!">
   </form>
   ```

   請注意，表單的`action`文章易受攻擊的站台，不供惡意網站。 這是 CSRF 的 「 跨網站 」 部分。

1. 使用者會選取 [提交] 按鈕。 瀏覽器提出要求，並會自動包含所要求的網域，驗證 cookie `www.good-banking-site.com`。
1. 執行要求`www.good-banking-site.com`與使用者的驗證內容的伺服器，而且可以執行已驗證的使用者可以執行任何動作。

除了指定案例中，使用者在選取按鈕送出表單，惡意網站無法：

* 執行自動送出表單的指令碼。
* 傳送 AJAX 要求送出表單。
* 隱藏使用 CSS 的表單。

這些替代案例不需要任何動作或以外一開始瀏覽惡意網站使用者的輸入。

使用 HTTPS，不會防止 CSRF 攻擊。 惡意的站台可以傳送`https://www.good-banking-site.com/`要求很容易就可以傳送不安全的要求。

部分攻擊的目標回應至 GET 要求的端點在此情況下之影像標記可以用來執行此動作。 這種形式的攻擊常會論壇網站允許映像，但封鎖 JavaScript。 變更狀態的 GET 要求，其中會變更變數或資源，應用程式很容易受到惡意的攻擊。 **變更狀態的 GET 要求是不安全的。最佳做法是永遠不會變更的 GET 要求的狀態。**

針對 web 應用程式都使用 cookie 進行驗證，因為可能 CSRF 攻擊︰

* 瀏覽器儲存 web 應用程式所發出的 cookie。
* 預存的 cookie 會包含已驗證的使用者工作階段 cookie。
* 瀏覽器傳送的所有 cookie 相關聯 web 應用程式網域每個要求，不論在瀏覽器內已產生的應用程式的要求。

不過，不限制 CSRF 攻擊利用 cookie。 比方說，也是很容易遭受基本和摘要式驗證。 瀏覽器使用者登入基本或摘要式驗證之後，自動傳送直到工作階段的認證&dagger;結束。

&dagger;在此內容中*工作階段*指的是用戶端工作階段期間會驗證使用者。 它是不相關的伺服器端工作階段或[ASP.NET Core 工作階段中介軟體](xref:fundamentals/app-state)。

使用者可以防範 CSRF 弱點，採取預防措施：

* 登出 web 應用程式使用它們。
* 定期清除瀏覽器 cookie。

不過，CSRF 弱點基本上都是使用 web 應用程式，終端使用者的問題。

## <a name="authentication-fundamentals"></a>驗證基本概念

受歡迎的形式的驗證 cookie 為基礎的驗證。 權杖型驗證系統會受歡迎情況看出，在不斷增加，特別是針對單一頁面應用程式 (SPAs)。

### <a name="cookie-based-authentication"></a>Cookie 為基礎的驗證

當使用者使用其使用者名稱和密碼，它們被發行包含可以用於驗證和授權的驗證票證的權杖。 權杖是以伴隨著每個要求的用戶端的 cookie 會儲存。 產生和驗證此 cookie 是由 Cookie 驗證中介軟體執行。 [中介軟體](xref:fundamentals/middleware/index)序列化經過加密的 cookie 的使用者主體。 在後續要求中中, 介軟體驗證 cookie、 主體，會重新建立並指派主體[使用者](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user)屬性[HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext)。

### <a name="token-based-authentication"></a>權杖型驗證

當使用者進行驗證時，它們被發行的權杖 （不 antiforgery 語彙基元）。 權杖包含使用者資訊的形式[宣告](/dotnet/framework/security/claims-based-identity-model)或指向維護應用程式中的使用者狀態的應用程式參考語彙基元。 當使用者嘗試存取需要驗證的資源時，權杖會傳送至應用程式與其他授權標頭的持有人權杖的格式。 這可讓應用程式的無狀態。 在每個後續的要求語彙基元被傳入要求的伺服器端驗證。 這個語彙基元不*加密*; 它有*編碼*。 在伺服器上，語彙基元解碼成存取其資訊。 若要傳送的語彙基元在後續的要求，將儲存在瀏覽器的本機儲存體的語彙基元。 如果瀏覽器的本機儲存體中儲存的語彙基元，不會擔心 CSRF 弱點。 CSRF 是考量當權杖儲存在 cookie 中。

### <a name="multiple-apps-hosted-at-one-domain"></a>裝載在一個網域的多個應用程式

共用裝載環境中受到劫持、 登入 CSRF 和其他攻擊。

雖然`example1.contoso.net`和`example2.contoso.net`是不同的主機，在主機之間沒有隱含的信任關係`*.contoso.net`網域。 這個隱含的信任關係可讓可能不受信任的主機會影響彼此的 cookie （控管 AJAX 要求的相同原始原則不一定會套用至 HTTP cookie）。

不共用網域就可以阻止利用相同的網域上裝載的應用程式之間的信任的 cookie 的攻擊。 當每個應用程式裝載於自己的網域時，就會利用沒有隱含的 cookie 信任關係。

## <a name="aspnet-core-antiforgery-configuration"></a>ASP.NET Core antiforgery 組態

> [!WARNING]
> ASP.NET Core 實作 antiforgery 使用[ASP.NET Core 資料保護](xref:security/data-protection/introduction)。 資料保護堆疊必須設定為在伺服器陣列中運作。 請參閱[設定資料保護](xref:security/data-protection/configuration/overview)如需詳細資訊。

在 ASP.NET Core 2.0 或更新版本， [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper)會 antiforgery token 插入至 HTML 表單元素。 Razor 檔案中的下列標記會自動產生 antiforgery 語彙基元：

```cshtml
<form method="post">
    ...
</form>
```

同樣地， [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform)依預設會產生 antiforgery 語彙基元，如果表單的方法不是 GET。

自動產生 antiforgery 語彙基元的 HTML 表單元素，會發生情況時`<form>`標記包含`method="post"`屬性和下列任何一個條件成立：

  * 動作屬性是空的 (`action=""`)。
  * 動作屬性不提供 (`<form method="post">`)。

您可以停用自動產生 antiforgery 語彙基元的 HTML 表單項目：

* 明確停用 antiforgery token 搭配`asp-antiforgery`屬性：

  ```cshtml
  <form method="post" asp-antiforgery="false">
      ...
  </form>
  ```

* Form 項目是選擇向外標記協助程式使用標記協助程式[！ 退出符號](xref:mvc/views/tag-helpers/intro#opt-out):

  ```cshtml
  <!form method="post">
      ...
  </!form>
  ```

* 移除`FormTagHelper`從檢視。 `FormTagHelper`可以從檢視移除 Razor 檢視中加入下列指示詞：

  ```cshtml
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> [Razor 頁面](xref:mvc/razor-pages/index)XSRF/CSRF 會自動受到保護。 如需詳細資訊，請參閱[XSRF/CSRF 和 Razor 頁面](xref:mvc/razor-pages/index#xsrf)。

防禦 CSRF 攻擊的常見方法是使用*同步器 Token 模式*(STP)。 當使用者要求網頁，以與表單資料，請使用 STP:

1. 伺服器傳送至用戶端的目前使用者的身分識別相關聯的語彙基元。
1. 用戶端上一步將權杖傳送至伺服器以供驗證。
1. 如果伺服器收到不符合已驗證的使用者的身分識別的語彙基元，則會拒絕要求。

權杖的唯一和無法預期。 語彙基元也可用來確保適當排序的一系列的要求 (例如，確保要求序列的： 第 1 頁&ndash;第 2 頁&ndash;頁面 3)。 所有的表單中 ASP.NET Core MVC 和 Razor 頁面範本產生 antiforgery 語彙基元。 下列檢視範例組產生 antiforgery 語彙基元：

```cshtml
<form asp-controller="Manage" asp-action="ChangePassword" method="post">
    ...
</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    ...
}
```

明確地加入 antiforgery 語彙基元`<form>`項目，而不使用標記協助程式的 HTML helper [ @Html.AntiForgeryToken ](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):

```cshtml
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

在每一個上述的情況下，ASP.NET Core 加入隱藏的表單欄位，如下所示：

```cshtml
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkS ... s2-m9Yw">
```

ASP.NET Core 包含三個[篩選](xref:mvc/controllers/filters)使用 antiforgery 語彙基元：

* [ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)
* [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)
* [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)

## <a name="antiforgery-options"></a>Antiforgery 選項

自訂[antiforgery 選項](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions)中`Startup.ConfigureServices`:

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
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiedomain) | Cookie 的網域。 預設值為 `null`。 這個屬性已經過時，未來版本將移除。 建議的替代做法是 Cookie.Domain。 |
| [CookieName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiename) | Cookie 的名稱。 如果未設定，系統會產生唯一的名稱開頭[DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (」。AspNetCore.Antiforgery。")。 這個屬性已經過時，未來版本將移除。 建議的替代做法是 Cookie.Name。 |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiepath) | 在 cookie 上設定的路徑。 這個屬性已經過時，未來版本將移除。 建議的替代做法是 Cookie.Path。 |
| [FormFieldName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | Antiforgery 系統用來呈現 antiforgery 語彙基元，在檢視中的隱藏的表單欄位的名稱。 |
| [HeaderName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | Antiforgery 系統所使用的標頭名稱。 如果`null`，系統會考慮只表單資料。 |
| [RequireSsl](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.requiressl) | 指定 antiforgery 系統是否需要 SSL。 如果`true`，非 SSL 要求失敗。 預設值為 `false`。 這個屬性已經過時，未來版本將移除。 建議的替代做法是將設定 Cookie.SecurePolicy。 |
| [SuppressXFrameOptionsHeader](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | 指定是否要隱藏產生`X-Frame-Options`標頭。 根據預設，會產生標頭，其值為"Sameorigin 所"。 預設值為 `false`。 |

如需詳細資訊，請參閱[CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions)。

## <a name="configure-antiforgery-features-with-iantiforgery"></a>使用 IAntiforgery 設定 antiforgery 功能

[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery)提供 API 來設定 antiforgery 功能。 `IAntiforgery` 可以要求在`Configure`方法`Startup`類別。 下列範例會使用產生 antiforgery 的語彙基元，並在回應中傳送為 （使用預設角度命名慣例在本主題稍後所述） cookie 中介軟體應用程式的首頁上：

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

[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)是動作篩選條件可以套用到個別的動作控制站，或全域。 套用此篩選條件的動作提出的要求會遭到封鎖，除非要求包含有效的 antiforgery 語彙基元。

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

`ValidateAntiForgeryToken`屬性裝飾，包括 HTTP GET 要求的動作方法要求需要 token。 如果`ValidateAntiForgeryToken`屬性套用跨應用程式的控制站，可以使用覆寫`IgnoreAntiforgeryToken`屬性。

> [!NOTE]
> ASP.NET Core 不支援 GET 要求中自動加入 antiforgery 語彙基元。

### <a name="automatically-validate-antiforgery-tokens-for-unsafe-http-methods-only"></a>Antiforgery 的語彙基元的不安全的 HTTP 方法只會自動進行驗證

ASP.NET Core 應用程式不會產生 antiforgery 權杖之安全的 HTTP 方法 （GET、 HEAD、 選項和追蹤）。 而不是廣泛套用`ValidateAntiForgeryToken`屬性，然後將它與覆寫`IgnoreAntiforgeryToken`屬性[AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)屬性可以使用。 這個屬性的運作方式和`ValidateAntiForgeryToken`屬性，不同之處在於它不需要使用下列的 HTTP 方法所提出之要求的語彙基元：

* GET
* HEAD
* 選項
* TRACE

我們建議使用`AutoValidateAntiforgeryToken`廣泛的非 API 」 案例。 這可確保依預設受到保護後動作。 替代方案是根據預設，忽略 antiforgery 語彙基元，除非`ValidateAntiForgeryToken`套用至個別動作方法。 它比較可能在此案例中剩餘的 POST 動作方法的受保護的錯誤，讓應用程式受到 CSRF 攻擊。 所有文章應該都傳送 antiforgery 語彙基元。

應用程式開發介面不需要傳送非 cookie 權杖的一部分的自動機制。 實作可能取決於用戶端程式碼實作。 某些範例如下所示：

類別層級的範例：

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

[IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)篩選條件可用於不需要指定的 「 動作 」 （或稱 「 控制器 」） antiforgery 語彙基元。 套用時，此篩選條件會覆寫`ValidateAntiForgeryToken`和`AutoValidateAntiforgeryToken`（全域或控制站上），在較高層級指定的篩選條件。

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

## <a name="refresh-tokens-after-authentication"></a>在驗證之後，重新整理權杖

將使用者重新導向至檢視或 Razor 頁面驗證使用者之後，應該重新整理語彙基元。

## <a name="javascript-ajax-and-spas"></a>JavaScript、 AJAX 和 SPAs

在傳統的 HTML 型應用程式，antiforgery 權杖會傳遞至使用隱藏的表單欄位的伺服器。 在現代 JavaScript 為基礎的應用程式和 SPAs，許多要求以程式設計的方式。 這些 AJAX 要求可以使用其他技術 （例如要求標頭或 cookie） 傳送語彙基元。

Cookie 用來儲存驗證權杖，並驗證伺服器上的應用程式開發介面要求，CSRF 有潛在的問題。 如果本機儲存體用來儲存的語彙基元，CSRF 的弱點可能會可能會降低，因為從本機儲存體的值不會自動傳送到每個要求的伺服器。 因此，使用本機儲存體來儲存 antiforgery 語彙基元，在用戶端傳送權杖，因為要求標頭是建議的方法。

### <a name="javascript"></a>JavaScript

檢視與使用 JavaScript，權杖可以建立使用從檢視內的服務。 插入[Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery)到檢視並呼叫服務[GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):

[!code-csharp[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,35-36)]

這個方法不需要直接處理從伺服器中設定 cookie，或從用戶端讀取。

上述範例使用 JavaScript AJAX 張貼標頭讀取隱藏的欄位值。

JavaScript 也可以存取 cookie 中的語彙基元，並使用 cookie 的內容建立的語彙基元值的標頭。

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
    new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

假設指令碼中呼叫的標頭傳送權杖要求`X-CSRF-TOKEN`，antiforgery 服務設定為尋找`X-CSRF-TOKEN`標頭：

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

下列範例使用 JavaScript，請使用適當的標頭 AJAX 要求：

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

AngularJS 會到位址 CSRF 慣例。 如果伺服器傳送的 cookie 名稱`XSRF-TOKEN`，AngularJS`$http`服務將 cookie 的值加入標頭將要求傳送到伺服器時。 此程序是自動的。 標頭，不需要明確設定。 標頭名稱`X-XSRF-TOKEN`。 伺服器應該偵測此標頭，並驗證其內容。

適用於 ASP.NET Core 應用程式開發介面使用這個慣例：

* 設定您的應用程式提供語彙基元在呼叫 cookie `XSRF-TOKEN`。
* 設定 antiforgery 服務，以尋找名為標頭`X-XSRF-TOKEN`。

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="extend-antiforgery"></a>擴充 antiforgery

[IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider)類型可讓開發人員以便在往返過程中每個語彙基元的其他資料來擴充反 CSRF 系統的行為。 [GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata)每次呼叫方法會產生欄位語彙基元，並傳回值內嵌在產生的語彙基元。 實作者可能傳回的時間戳記、 nonce 或任何其他值，然後呼叫[ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata)驗證語彙基元時驗證這項資料。 用戶端的使用者名稱已內嵌在產生的語彙基元，因此不需要加入這項資訊。 如果語彙基元包含補充資料但不是`IAntiForgeryAdditionalDataProvider`是設定，未驗證的補充資料。

## <a name="additional-resources"></a>其他資源

* [CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))上[開啟 Web 應用程式安全性專案](https://www.owasp.org/index.php/Main_Page)(OWASP)。
