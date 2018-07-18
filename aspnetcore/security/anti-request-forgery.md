---
title: ASP.NET Core 中的防止跨網站要求偽造 (XSRF/CSRF) 攻擊
author: steve-smith
description: 了解如何防止攻擊，惡意網站可能會影響用戶端瀏覽器與應用程式之間的互動的 web 應用程式。
ms.author: riande
ms.custom: mvc
ms.date: 03/19/2018
uid: security/anti-request-forgery
ms.openlocfilehash: 6a30e1e2321ca3a81d6e1a320d1d87dddb3033c7
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095784"
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a>ASP.NET Core 中的防止跨網站要求偽造 (XSRF/CSRF) 攻擊

藉由[Steve Smith](https://ardalis.com/)， [Fiyaz Hasan](https://twitter.com/FiyazBinHasan)，和[Rick Anderson](https://twitter.com/RickAndMSFT)

跨網站偽造要求 (也稱為 XSRF 或 CSRF，唸成 *，請參閱上網*) 會讓惡意的 web 應用程式可能會影響用戶端瀏覽器和信任的 web 應用程式之間的互動的 web 應用程式對抗攻擊瀏覽器。 這些攻擊可能會因為網頁瀏覽器會將某些類型的驗證權杖會自動隨著每個要求傳送至網站。 這種形式的攻擊，也就是*單鍵攻擊*或是*工作階段乘載*因為攻擊會利用使用者先前的驗證工作階段。

CSRF 攻擊的範例：

1. 使用者登入`www.good-banking-site.com`使用表單驗證。 伺服器會驗證使用者，並會發出包含驗證 cookie 的回應。 站台是很容易遭受攻擊，因為它所信任的任何要求，它會收到包含有效的驗證 cookie。
1. 使用者瀏覽惡意網站， `www.bad-crook-site.com`。

   惡意的網站， `www.bad-crook-site.com`，包含 HTML 表單，如下所示：

   ```html
   <h1>Congratulations! You're a Winner!</h1>
   <form action="http://good-banking-site.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw">
       <input type="hidden" name="Amount" value="1000000">
       <input type="submit" value="Click to collect your prize!">
   </form>
   ```

   請注意，表單的`action`貼文到有弱點網站，而非惡意的網站。 這是 CSRF 的 「 跨站台 」 部分。

1. 使用者選取 [提交] 按鈕。 瀏覽器提出要求，並會自動包含所要求的網域，驗證 cookie `www.good-banking-site.com`。
1. 執行要求`www.good-banking-site.com`與使用者的驗證內容的伺服器，而且可以執行已驗證的使用者可以執行任何動作。

除了指定案例中，使用者選取按鈕以送出表單時，惡意網站可能：

* 執行自動送出表單的指令碼。
* 傳送的 AJAX 要求送出表單。
* 隱藏使用 CSS 的形式。

這些替代案例不需要任何動作或一開始瀏覽惡意的網站以外使用者的輸入。

使用 HTTPS 不會防止 CSRF 攻擊。 惡意網站可以傳送 `https://www.good-banking-site.com/` 要求只一樣，它可以傳送不安全的要求。

某些攻擊會鎖定在此情況下之影像標記可用來執行動作的回應 GET 要求的端點。 這種形式的攻擊會讓映像，但封鎖 JavaScript 的論壇網站上。 變更狀態的 GET 要求，其中會改變變數或資源，應用程式容易受到惡意攻擊的影響。 **變更狀態的 GET 要求是不安全。最佳做法是永遠不會變更在 GET 要求的狀態。**

CSRF 攻擊是可能對 web 應用程式使用 cookie 進行驗證，因為：

* 瀏覽器儲存 web 應用程式所發出的 cookie。
* 預存的 cookie 會包含已驗證的使用者工作階段 cookie。
* 瀏覽器傳送的所有 cookie 與網域相關聯 web 應用程式無論在瀏覽器內產生應用程式的要求方式的每個要求。

不過，CSRF 攻擊不會限制為 cookie 的全部功能。 例如，基本和摘要式驗證也是易受攻擊。 瀏覽器使用基本或摘要式驗證的使用者登入之後，自動傳送到工作階段認證&dagger;結束。

&dagger;在此情況下，*工作階段*指的用戶端工作階段期間會驗證使用者。 它是不相關的伺服器端工作階段或[ASP.NET Core 工作階段中介軟體](xref:fundamentals/app-state)。

使用者可以防範 CSRF 弱點採取預防措施：

* 從 web 應用程式時使用它們完成登入。
* 定期清除瀏覽器 cookie。

不過，CSRF 弱點基本上都是 web 應用程式，而非由終端使用者的問題。

## <a name="authentication-fundamentals"></a>驗證基本概念

熱門的形式的驗證 cookie 為基礎的驗證。 權杖型驗證系統越來越大受歡迎，特別是針對單一頁面應用程式 (Spa)。

### <a name="cookie-based-authentication"></a>以 cookie 為基礎的驗證

當使用者驗證使用使用者名稱和密碼時，會核發給這些包含可以用於驗證和授權的驗證票證的權杖。 權杖會儲存為隨附於每個要求的用戶端的 cookie 可讓。 產生和驗證此 cookie 是由 Cookie 驗證中介軟體執行的。 [中介軟體](xref:fundamentals/middleware/index)序列化的加密 cookie 中的使用者主體。 在後續的要求中, 介軟體驗證 cookie、 重新建立主體，並將指派主體[使用者](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user)屬性[HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext)。

### <a name="token-based-authentication"></a>權杖型驗證

當驗證使用者時，會核發給這些語彙基元 （不 antiforgery 權杖）。 權杖包含使用者資訊的形式[宣告](/dotnet/framework/security/claims-based-identity-model)或指向維護應用程式中的使用者狀態的應用程式的參考語彙基元。 當使用者嘗試存取需要驗證的資源時，就會將權杖傳送至應用程式與其他授權標頭的持有人權杖的格式。 這可讓應用程式的無狀態。 在每個後續的要求中，權杖會傳遞要求中的伺服器端驗證。 這個語彙基元未*加密*; 它具有*編碼*。 在伺服器上，此語彙基元解碼成存取其資訊。 在後續要求中傳送的權杖，會在瀏覽器的本機儲存體中儲存權杖。 如果瀏覽器的本機儲存體中儲存的語彙基元，則不會擔心 CSRF 的弱點可能會。 CSRF 權杖儲存在 cookie 中時的考量。

### <a name="multiple-apps-hosted-at-one-domain"></a>裝載在一個網域的多個應用程式

共用的裝載環境包括工作階段攔截、 登入 CSRF 和其他攻擊變得更加容易。

雖然`example1.contoso.net`並`example2.contoso.net`是不同的主控件，在主機之間沒有隱含的信任關係`*.contoso.net`網域。 這個隱含的信任關係可讓可能不受信任的主機會影響彼此的 cookie （控管 AJAX 要求的相同原始原則不一定會套用至 HTTP cookie）。

不會共用網域可防止惡意探索應用程式裝載於相同的網域之間的信任的 cookie 的攻擊。 當每個應用程式裝載在自己的網域中時，會利用沒有隱含的 cookie 信任關係。

## <a name="aspnet-core-antiforgery-configuration"></a>ASP.NET Core antiforgery 組態

> [!WARNING]
> ASP.NET Core 會實作使用 antiforgery [ASP.NET Core 資料保護](xref:security/data-protection/introduction)。 資料保護堆疊必須設定為伺服器陣列中。 請參閱[設定資料保護](xref:security/data-protection/configuration/overview)如需詳細資訊。

在 ASP.NET Core 2.0 或更新版本中， [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) antiforgery 權杖插入 HTML 表單項目。 Razor 檔案中的以下標記會自動產生的防偽語彙基元：

```cshtml
<form method="post">
    ...
</form>
```

同樣地， [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform)依預設會產生 antiforgery 權杖，如果表單的方法不是 GET。

自動產生的防偽語彙基元的 HTML 表單項目發生時`<form>`標記包含`method="post"`屬性和下列其中一項條件成立：

  * 動作屬性是空的 (`action=""`)。
  * 未提供的 action 屬性 (`<form method="post">`)。

您可以停用自動產生的防偽語彙基元，為 HTML 表單項目：

* 明確停用使用防偽權杖`asp-antiforgery`屬性：

  ```cshtml
  <form method="post" asp-antiforgery="false">
      ...
  </form>
  ```

* 表單項目是選擇外的標籤協助程式使用標籤協助程式[！ 退出符號](xref:mvc/views/tag-helpers/intro#opt-out):

  ```cshtml
  <!form method="post">
      ...
  </!form>
  ```

* 移除`FormTagHelper`從檢視。 `FormTagHelper`可以從檢視移除，藉由將下列指示詞新增至 Razor 檢視：

  ```cshtml
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> [Razor 頁面](xref:razor-pages/index)從 XSRF/CSRF 會自動施以保護。 如需詳細資訊，請參閱 < [XSRF/CSRF 和 Razor Pages](xref:razor-pages/index#xsrf)。

對抗 CSRF 攻擊的最常見方法是使用*同步器 Token 模式*(spanning tree Protocol)。 當使用者要求包含表單資料的頁面時，會使用 spanning tree Protocol:

1. 伺服器會傳送至用戶端的目前使用者的身分識別相關聯的語彙基元。
1. 用戶端上一步將權杖傳送至伺服器進行驗證。
1. 如果伺服器收到不符合已驗證的使用者的身分識別的權杖，則會拒絕要求。

權杖的唯一且無法預測。 語彙基元也可用來確保適當排序的一系列的要求 (例如，確保要求序列的： 第 1 頁&ndash;第 2 頁&ndash;頁面 3)。 所有的 ASP.NET Core MVC 和 Razor 頁面範本中的表單產生防偽語彙基元。 下列配對檢視範例產生的防偽語彙基元：

```cshtml
<form asp-controller="Manage" asp-action="ChangePassword" method="post">
    ...
</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    ...
}
```

明確地將加入的 antiforgery 權杖`<form>`而不使用標籤協助程式與 HTML 協助程式元素[ @Html.AntiForgeryToken ](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):

```cshtml
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

在每個上述所有情況下，ASP.NET Core 會將隱藏的表單欄位，如下所示：

```cshtml
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkS ... s2-m9Yw">
```

ASP.NET Core 包含三個[篩選器](xref:mvc/controllers/filters)使用防偽權杖：

* [ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)
* [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)
* [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)

## <a name="antiforgery-options"></a>Antiforgery 選項

來自訂[antiforgery 選項](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions)在`Startup.ConfigureServices`:

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
| [Cookie](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | 決定用來建立防偽 cookie 的設定。 |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiedomain) | Cookie 的網域。 預設值為 `null`。 這個屬性已經過時，將在未來版本中移除。 建議的替代做法是 Cookie.Domain。 |
| [CookieName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiename) | Cookie 的名稱。 如果未設定，系統會產生唯一的名稱開頭[DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) ("。AspNetCore.Antiforgery。")。 這個屬性已經過時，將在未來版本中移除。 建議的替代做法是 Cookie.Name。 |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiepath) | 在 cookie 上設定的路徑。 這個屬性已經過時，將在未來版本中移除。 建議的替代做法是 Cookie.Path。 |
| [FormFieldName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | 防偽系統用來呈現檢視中的防偽語彙基元的隱藏的表單欄位名稱。 |
| [HeaderName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | 防偽系統所使用的標頭名稱。 如果`null`，系統會考慮只表單資料。 |
| [RequireSsl](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.requiressl) | 指定是否需要 SSL，則防偽系統。 如果`true`，非 SSL 要求會失敗。 預設值為 `false`。 這個屬性已經過時，將在未來版本中移除。 建議的替代做法是將 Cookie.SecurePolicy。 |
| [SuppressXFrameOptionsHeader](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | 指定是否要隱藏產生`X-Frame-Options`標頭。 根據預設，標頭會產生含有"Sameorigin 所"的值。 預設值為 `false`。 |

如需詳細資訊，請參閱 < [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions)。

## <a name="configure-antiforgery-features-with-iantiforgery"></a>使用 IAntiforgery 設定 antiforgery 功能

[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery)提供的 API 來設定 antiforgery 的功能。 `IAntiforgery` 可在要求`Configure`方法的`Startup`類別。 下列範例會使用從應用程式的首頁上的中介軟體，以產生 antiforgery 權杖，並在回應中傳送 cookie （使用預設 Angular 命名慣例本主題稍後所述）：

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

### <a name="require-antiforgery-validation"></a>需要防偽驗證

[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)是動作篩選條件可以套用至個別動作、 控制器或全域。 除非要求包含有效的防偽語彙基元，則會封鎖對 套用此篩選條件的動作的要求。

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

`ValidateAntiForgeryToken`屬性裝飾，包括 HTTP GET 要求至動作方法要求需要的語彙基元。 如果`ValidateAntiForgeryToken`屬性會套用跨應用程式的控制站，可以使用覆寫`IgnoreAntiforgeryToken`屬性。

> [!NOTE]
> ASP.NET Core 不支援自動將 antiforgery 權杖新增至 GET 要求。

### <a name="automatically-validate-antiforgery-tokens-for-unsafe-http-methods-only"></a>會自動進行驗證不安全的 HTTP 方法只 antiforgery 的權杖

ASP.NET Core 應用程式不會產生安全的 HTTP 方法 （GET、 HEAD、 選項和追蹤） 的防偽語彙基元。 而不是廣泛套用`ValidateAntiForgeryToken`屬性，然後將它與覆寫`IgnoreAntiforgeryToken`屬性， [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)屬性可用。 此屬性運作方式完全相同`ValidateAntiForgeryToken`屬性，不同之處在於它不需要使用下列的 HTTP 方法所提出之要求的權杖：

* GET
* HEAD
* 選項
* TRACE

我們建議使用`AutoValidateAntiforgeryToken`廣泛用於非 API 案例。 這可確保預設受到保護後動作。 替代方法是忽略 antiforgery 權杖依預設，除非`ValidateAntiForgeryToken`套用至個別動作方法。 它比較可能在此案例中的 POST 動作方法來保留受保護的誤離開應用程式容易遭受 CSRF 攻擊。 所有貼文應該傳送的防偽語彙基元。

Api 不需要傳送非 cookie 權杖的一部分的自動機制。 實作可能取決於用戶端程式碼實作。 一些範例如下所示：

類別層級的範例：

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

全域的範例：

```csharp
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

### <a name="override-global-or-controller-antiforgery-attributes"></a>覆寫全域或控制器 antiforgery 屬性

[IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)篩選器可用來消除對指定的 「 動作 」 （或稱 「 控制器 」） 的 antiforgery 權杖。 此篩選器套用時，覆寫`ValidateAntiForgeryToken`和`AutoValidateAntiforgeryToken`（全域或控制站上），指定在較高層級的篩選條件。

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

使用者驗證的使用者重新導向至檢視表或 Razor 頁面 頁面之後，就應該重新整理權杖。

## <a name="javascript-ajax-and-spas"></a>JavaScript、 AJAX 和 Spa

在傳統的 HTML 型應用程式，antiforgery 權杖會傳遞至使用隱藏的表單欄位的伺服器。 在現代化的 JavaScript 應用程式和 Spa，會以程式設計方式提出許多要求。 這些 AJAX 要求可能會使用其他技術 （例如要求標頭或 cookie） 傳送的權杖。

如果 cookie 用來儲存驗證權杖，並驗證伺服器上的 API 要求，CSRF 是潛在的問題。 如果本機儲存體來儲存權杖，可能會降低 CSRF 的弱點可能會因為從本機儲存體的值不自動傳送至每個要求的伺服器。 若要將 antiforgery 權杖儲存在用戶端和傳送權杖做為要求標頭是建議的方法，因此，使用本機儲存體。

### <a name="javascript"></a>JavaScript

使用 JavaScript 與檢視，可以建立權杖使用檢視內的服務。 插入[Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery)服務到檢視，然後呼叫[GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):

[!code-csharp[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,35-36)]

這種方法就不需要直接處理伺服器上設定 cookie，或從用戶端讀取。

上述範例中使用 JavaScript 針對 POST 的 AJAX 標頭讀取隱藏的欄位值。

JavaScript 也可以存取 cookie 中的權杖，並使用 cookie 的內容權杖的值建立的標頭。

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
    new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

假設指令碼呼叫的標頭中傳送的權杖要求`X-CSRF-TOKEN`，將 antiforgery 服務設定為尋找`X-CSRF-TOKEN`標頭：

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

下列範例會使用 JavaScript 來提出 AJAX 要求使用適當的標頭：

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

AngularJS 使用位址 CSRF 慣例。 如果伺服器會傳送具有名稱的 cookie `XSRF-TOKEN`，AngularJS`$http`服務將 cookie 的值加入標頭將要求傳送到伺服器時。 此程序是自動的。 標頭不需要明確設定。 標頭名稱`X-XSRF-TOKEN`。 伺服器應該偵測此標頭，並驗證其內容。

適用於 ASP.NET Core API 會使用此慣例︰

* 設定您的應用程式提供的權杖在 cookie 中稱為`XSRF-TOKEN`。
* 將 antiforgery 服務設定為尋找標頭`X-XSRF-TOKEN`。

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="extend-antiforgery"></a>擴充 antiforgery

[IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider)類型可讓開發人員在每個權杖中的反覆存取其他資料來擴充防 CSRF 系統的行為。 [GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata)每次呼叫方法會產生欄位的語彙基元，並傳回值內嵌在產生的權杖。 實作者可能傳回的時間戳記、 nonce 或任何其他值，然後呼叫[ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata)來驗證權杖時驗證這項資料。 用戶端的使用者名稱已內嵌在產生的權杖中，這樣就不需要加入這項資訊。 如果語彙基元包含補充資料，但不是`IAntiForgeryAdditionalDataProvider`是設定，未驗證的補充資料。

## <a name="additional-resources"></a>其他資源

* [CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))上[開啟 Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP)。
* <xref:host-and-deploy/web-farm>
