---
title: "防止跨網站要求偽造 (XSRF/CSRF) 攻擊中 ASP.NET Core"
author: steve-smith
ms.author: riande
description: "防止跨網站要求偽造 (XSRF/CSRF) 攻擊中 ASP.NET Core"
manager: wpickett
ms.date: 7/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/anti-request-forgery
ms.openlocfilehash: 3831bf737186d10eb1b298f5ec2da1fd33ebedd9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
# <a name="preventing-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a>防止跨網站要求偽造 (XSRF/CSRF) 攻擊中 ASP.NET Core

[Steve Smith](https://ardalis.com/)， [Fiyaz Hasan](https://twitter.com/FiyazBinHasan)，和[Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="what-attack-does-anti-forgery-prevent"></a>防偽防止何種攻擊？

跨站台要求偽造 (也稱為 XSRF 或 CSRF，唸成*，請參閱衝浪*) 是 web 裝載的應用程式讓惡意網站可能會影響用戶端瀏覽器和信任的網站之間的互動攻擊該瀏覽器。 這些攻擊都可能因為 web 瀏覽器傳送到網站的某些類型的驗證權杖會自動與每個要求。 這種形式的攻擊也稱為的*單鍵攻擊*或*工作階段乘載*，因為使用者的攻擊會利用先前的驗證工作階段。

CSRF 攻擊的範例：

1. 使用者登入`www.example.com`，使用表單驗證。
2. 伺服器會驗證使用者，並會發出包含驗證 cookie 的回應。
3. 使用者造訪惡意網站。

   惡意的站台包含 HTML 表單如下所示：

```html
   <h1>You Are a Winner!</h1>
     <form action="http://example.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw" />
       <input type="hidden" name="Amount" value="1000000" />
     <input type="submit" value="Click Me"/>
   </form>
```

請注意，張貼至容易遭受站台，惡意網站的表單動作。 這是 CSRF 的 「 跨網站 」 部分。

4. 使用者按一下 [提交] 按鈕。 瀏覽器會自動包含所要求的網域 （在此情況下很容易遭受站台） 要求的驗證 cookie。
5. 要求使用者驗證內容與在伺服器上執行，並可以執行已驗證的使用者允許進行的任何動作。

這個範例會要求使用者可以按一下 [表單] 按鈕。 惡意的頁面可以：

* 執行自動送出表單的指令碼。
* 以 AJAX 要求中傳送的表單提交。 
* Css 使用隱藏的表單。 

使用 SSL 不會防止 CSRF 攻擊，惡意的站台可以傳送`https://`要求。 

部分攻擊目標站台端點回應`GET`要求，在其中案例之影像標記可以用來執行 （這種攻擊是常見的允許映像，但會封鎖 JavaScript 論壇網站） 的動作。 變更狀態時，使用的應用程式`GET`要求而受到損害受到惡意的攻擊。

CSRF 攻擊可能會進行對網站使用 cookie 進行驗證，因為瀏覽器傳送到目標網站的所有相關的 cookie。 不過，不限於利用 cookie CSRF 攻擊。 比方說，也是很容易遭受基本和摘要式驗證。 在使用者登入基本或摘要式驗證後，瀏覽器將會自動傳送認證到工作階段結束為止。

注意： 在此情況下，*工作階段*指的是用戶端工作階段期間會驗證使用者。 它是不相關的伺服器端工作階段或[工作階段中介軟體](xref:fundamentals/app-state)。

使用者可以防範 CSRF 缺失：
* 記錄遠離網站時使用它們完成。
* 定期清除其瀏覽器的 cookie。

不過，CSRF 弱點基本上都是使用 web 應用程式，終端使用者的問題。

## <a name="how-does-aspnet-core-mvc-address-csrf"></a>ASP.NET Core MVC 如何解決 CSRF？

> [!WARNING]
> ASP.NET Core 實作反 request 偽造使用[ASP.NET Core 資料保護堆疊](xref:security/data-protection/introduction)。 ASP.NET Core 資料保護必須設定為在伺服器陣列中運作。 請參閱[設定資料保護](xref:security/data-protection/configuration/overview)如需詳細資訊。

ASP.NET Core 反 request 偽造預設資料保護設定 

在 ASP.NET Core MVC 2.0 [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper)插入防偽語彙基元的 HTML 表單元素。 例如，下列標記 Razor 檔案中的將會自動產生防偽語彙基元：

```html
<form method="post">
  <!-- form markup -->
</form>
```

自動產生的防偽語彙基元的 HTML 表單元素，會發生情況時：

* `form`標記包含`method="post"`屬性 AND

  * 動作屬性是空的。 ( `action=""`) 或
  * 未提供的 action 屬性。 (`<form method="post">`)

您可以停用自動產生的防偽語彙基元的 HTML 表單項目：

* 明確停用`asp-antiforgery`。 例如

 ```html
  <form method="post" asp-antiforgery="false">
  </form>
  ```

* 使用標記協助程式選擇超出標記協助程式的表單項目[！ 退出符號](xref:mvc/views/tag-helpers/intro#opt-out)。

 ```html
  <!form method="post">
  </!form>
  ```

* 移除`FormTagHelper`從檢視。 您可以移除`FormTagHelper`從下列指示詞加入 Razor 檢視的檢視：

 ```html
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> [Razor 頁面](xref:mvc/razor-pages/index)XSRF/CSRF 會自動受到保護。 您不需要撰寫任何額外的程式碼。 請參閱[XSRF/CSRF 和 Razor 頁面](xref:mvc/razor-pages/index#xsrf)如需詳細資訊。

防禦 CSRF 攻擊的常見方法是同步器 token 模式 (STP)。 STP 是使用者要求表單資料的頁面時使用的技術。 伺服器傳送至用戶端的目前使用者的身分識別相關聯的語彙基元。 用戶端上一步將權杖傳送至伺服器以供驗證。 如果伺服器收到不符合已驗證的使用者的身分識別的語彙基元，則會拒絕要求。 權杖的唯一和無法預期。 語彙基元也可用以確保適當排序的一系列的要求 （確保第 1 頁位於第 2 頁位於第 3 頁）。 ASP.NET Core MVC 範本中的所有表單都產生 antiforgery 語彙基元。 檢視邏輯下列兩個範例會產生 antiforgery 語彙基元：

```html
<form asp-controller="Manage" asp-action="ChangePassword" method="post">

</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    
}
```

您可以明確加入 antiforgery 語彙基元``<form>``項目，而不使用標記協助程式的 HTML helper ``@Html.AntiForgeryToken``:


```html
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

在每一個上述的情況下，ASP.NET Core 將會加入隱藏的表單欄位，如下所示：
```html
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkSldwD9CpLRyOtm6FiJB1Jr_F3FQJQDvhlHoLNJJrLA6zaMUmhjMsisu2D2tFkAiYgyWQawJk9vNm36sYP1esHOtamBEPvSk1_x--Sg8Ey2a-d9CV2zHVWIN9MVhvKHOSyKqdZFlYDVd69XYx-rOWPw3ilHGLN6K0Km-1p83jZzF0E4WU5OGg5ns2-m9Yw" />
```

ASP.NET Core 包含三個[篩選](xref:mvc/controllers/filters)使用 antiforgery 語彙基元： ``ValidateAntiForgeryToken``， ``AutoValidateAntiforgeryToken``，和``IgnoreAntiforgeryToken``。

<a name="vaft"></a>

### <a name="validateantiforgerytoken"></a>ValidateAntiForgeryToken

``ValidateAntiForgeryToken``是動作篩選條件可以套用到個別的動作控制站，或全域。 除非要求包含有效的 antiforgery 語彙基元，將會封鎖套用此篩選條件的動作提出的要求。

```c#
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> RemoveLogin(RemoveLoginViewModel account)
{
    ManageMessageId? message = ManageMessageId.Error;
    var user = await GetCurrentUserAsync();
    if (user != null)
    {
        var result = await _userManager.RemoveLoginAsync(user, account.LoginProvider, account.ProviderKey);
        if (result.Succeeded)
        {
            await _signInManager.SignInAsync(user, isPersistent: false);
            message = ManageMessageId.RemoveLoginSuccess;
        }
    }
    return RedirectToAction(nameof(ManageLogins), new { Message = message });
}
```

``ValidateAntiForgeryToken``屬性需要裝飾，包括要求至動作方法的語彙基元`HTTP GET`要求。 如果您將它套用廣泛，您可以覆寫它與``IgnoreAntiforgeryToken``屬性。

### <a name="autovalidateantiforgerytoken"></a>AutoValidateAntiforgeryToken

ASP.NET Core 應用程式通常不產生 antiforgery 的語彙基元的安全 HTTP 方法 （GET、 HEAD、 選項和追蹤）。 而不是廣泛套用``ValidateAntiForgeryToken``屬性，然後將它與覆寫``IgnoreAntiforgeryToken``屬性，您可以使用``AutoValidateAntiforgeryToken``屬性。 這個屬性的運作方式和``ValidateAntiForgeryToken``屬性，不同之處在於它不需要使用下列的 HTTP 方法所提出之要求的語彙基元：

* GET
* HEAD
* 選項
* TRACE

我們建議您改用``AutoValidateAntiforgeryToken``廣泛的非 API 」 案例。 這可確保您張貼的動作依預設受到保護。 替代方案是根據預設，忽略 antiforgery 語彙基元，除非``ValidateAntiForgeryToken``套用至個別動作方法。 很可能在此案例中將 POST 動作方法的左未受保護，讓您的應用程式受到 CSRF 攻擊。 即使匿名文章應該傳送 antiforgery 語彙基元。

注意： 應用程式開發介面沒有自動化的機制來傳送非 cookie 權杖的一部分。您的實作可能取決於您的用戶端程式碼實作。 某些範例如下所示。


範例 （類別層級）：

```c#
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

範例 （全域）：

```c#
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

<a name="iaft"></a>

### <a name="ignoreantiforgerytoken"></a>IgnoreAntiforgeryToken

``IgnoreAntiforgeryToken``篩選器可用來消除 antiforgery 的權杖，才能在指定的 「 動作 」 （或稱 「 控制器 」） 的需要。 此篩選器套用時，將會覆寫``ValidateAntiForgeryToken``及/或``AutoValidateAntiforgeryToken``（全域或控制站上），在較高層級指定的篩選條件。

```c#
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

## <a name="javascript-ajax-and-spas"></a>JavaScript、 AJAX 和 SPAs

傳統的 HTML 型應用程式，antiforgery 權杖會傳遞至使用隱藏的表單欄位的伺服器。 在現代 JavaScript 為基礎的應用程式和單一頁面應用程式 (SPAs) 中，許多要求以程式設計的方式。 這些 AJAX 要求可以使用其他技術 （例如要求標頭或 cookie） 傳送語彙基元。 如果，使用 cookie 來儲存驗證權杖，並驗證伺服器上的應用程式開發介面要求 CSRF 將會有潛在的問題。 不過，如果本機儲存體用來儲存的語彙基元，CSRF 的弱點可能會可能會降低，因為從本機儲存體的值不會自動傳送到每個新要求的伺服器。 因此，使用本機儲存體來儲存 antiforgery 語彙基元，在用戶端傳送權杖，因為要求標頭是建議的方法。

### <a name="angularjs"></a>AngularJS

AngularJS 會到位址 CSRF 慣例。 如果伺服器傳送的 cookie 名稱``XSRF-TOKEN``，Angular``$http``服務會將加入的值從這個 cookie 標頭將要求傳送到此伺服器時。 此程序是自動的。您不需要明確設定的標頭。 標頭名稱``X-XSRF-TOKEN``。 伺服器應該偵測此標頭，並驗證其內容。

適用於 ASP.NET Core 應用程式開發介面使用這個慣例：

* 設定您的應用程式提供語彙基元在呼叫 cookie``XSRF-TOKEN``
* 設定 antiforgery 服務，以尋找名為標頭``X-XSRF-TOKEN``

```c#
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

[檢視範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample)。

### <a name="javascript"></a>JavaScript

使用 JavaScript 與檢視，您可以建立使用從您的檢視中的服務權杖。 若要這樣做，您將插入`Microsoft.AspNetCore.Antiforgery.IAntiforgery`到檢視並呼叫服務`GetAndStoreTokens`，如下所示：

[!code-csharp[Main](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,24)]

這個方法不需要直接處理從伺服器中設定 cookie，或從用戶端讀取。

JavaScript 可以也存取 cookie 中所提供的權杖，然後再使用 cookie 的內容來建立標頭的語彙基元值，如下所示。

```c#
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
  new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

接著，假設您建構指令碼就會要求呼叫標頭中傳送的語彙基元``X-CSRF-TOKEN``，antiforgery 服務設定為尋找``X-CSRF-TOKEN``標頭：

```c#
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

下列範例會使用 jQuery 將 AJAX 要求具有適當的標頭：

```javascript
var csrfToken = $.cookie("CSRF-TOKEN");

$.ajax({
    url: "/api/password/changepassword",
    contentType: "application/json",
    data: JSON.stringify({ "newPassword": "ReallySecurePassword999$$$" }),
    type: "POST",
    headers: {
        "X-CSRF-TOKEN": csrfToken
    }
});
```

## <a name="configuring-antiforgery"></a>設定 Antiforgery

`IAntiforgery`提供 API 來設定 antiforgery 系統。 可以要求在`Configure`方法`Startup`類別。 下列範例會使用產生 antiforgery 的語彙基元，並在回應中傳送為 （使用預設角度命名慣例上面所述） cookie 中介軟體應用程式的首頁上：


```c#
public void Configure(IApplicationBuilder app, 
    IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;
        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // We can send the request token as a JavaScript-readable cookie, 
            // and Angular will use it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
    //
}
```

### <a name="options"></a>選項

您可以自訂[antiforgery 選項](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions#fields_summary)中`ConfigureServices`:

```c#
services.AddAntiforgery(options => 
{
  options.CookieDomain = "mydomain.com";
  options.CookieName = "X-CSRF-TOKEN-COOKIENAME";
  options.CookiePath = "Path";
  options.FormFieldName = "AntiforgeryFieldname";
  options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
  options.RequireSsl = false;
  options.SuppressXFrameOptionsHeader = false;
});
```

<!-- QAfix fix table -->

|選項        | 描述 |
|------------- | ----------- |
|CookieDomain  | Cookie 的網域。 預設值為 `null`。 |
|CookieName    | Cookie 的名稱。 如果未設定，系統會產生唯一的名稱開頭`DefaultCookiePrefix`(」。AspNetCore.Antiforgery。")。 |
|CookiePath    | 在 cookie 上設定的路徑。 |
|FormFieldName | Antiforgery 系統用來呈現 antiforgery 語彙基元，在檢視中的隱藏的表單欄位的名稱。 |
|HeaderName    | Antiforgery 系統所使用的標頭名稱。 如果`null`，系統會考慮只表單資料。 |
|RequireSsl    | 指定 antiforgery 系統是否需要 SSL。 預設值為 `false`。 如果`true`，非 SSL 要求將會失敗。 |
|SuppressXFrameOptionsHeader  | 指定是否要隱藏產生`X-Frame-Options`標頭。 根據預設，會產生標頭，其值為"Sameorigin 所"。 預設值為 `false`。 |

Https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions 如需詳細資訊，請參閱。

### <a name="extending-antiforgery"></a>擴充 Antiforgery

[IAntiForgeryAdditionalDataProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider)類型可讓開發人員擴充以便在往返過程中每個語彙基元的其他資料的防 XSRF 系統行為。 [GetAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_GetAdditionalData_Microsoft_AspNetCore_Http_HttpContext_)每次呼叫方法會產生欄位語彙基元，並傳回值內嵌在產生的語彙基元。 實作者可能傳回的時間戳記、 nonce 或任何其他值，然後呼叫[ValidateAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_ValidateAdditionalData_Microsoft_AspNetCore_Http_HttpContext_System_String_)驗證語彙基元時驗證這項資料。 用戶端的使用者名稱已內嵌在產生的語彙基元，因此不需要加入這項資訊。 如果語彙基元包含補充資料但不是`IAntiForgeryAdditionalDataProvider`已經過設定，未驗證的補充資料。

## <a name="fundamentals"></a>Fundamentals

CSRF 攻擊依賴傳送每個要求對該網域與定義域相關聯的 cookie 的預設瀏覽器的行為。 這些 cookie 會儲存在瀏覽器中。 它們通常會包含已驗證的使用者工作階段 cookie。 受歡迎的形式的驗證 cookie 為基礎的驗證。 權杖型驗證系統已以受歡迎情況看出，成長特別是針對 SPAs 和其他 「 智慧型用戶端 」 案例。

### <a name="cookie-based-authentication"></a>Cookie 為基礎的驗證

一旦使用者已使用使用者名稱和密碼驗證時，它們被發行可用來識別及驗證他們已經過驗證的語彙基元。 權杖是以伴隨著每個要求的用戶端的 cookie 會儲存。 產生和驗證此 cookie 是由 cookie 驗證中介軟體。 ASP.NET Core 提供 cookie[中介軟體](../fundamentals/middleware.md)的序列化經過加密的 cookie 的使用者主體，然後在後續要求中，驗證 cookie，會重新建立主體，並將其指派給`User`屬性`HttpContext`.

使用 cookie 時，驗證 cookie 只是一個容器的表單驗證票證。 票證傳遞做為表單驗證 cookie，隨著每項要求的值，並為表單驗證，在伺服器上，用來識別已驗證的使用者。

當使用者登入系統時，使用者工作階段會在伺服器端上建立並儲存在資料庫或其他持續性存放區。 系統會產生指向實際的工作階段資料存放區中的工作階段金鑰，並為用戶端端 cookie 傳送出去。 使用者要求的資源需要授權的任何時間時，網頁伺服器會檢查此工作階段金鑰。 系統會檢查相關聯的使用者工作階段是否擁有存取所要求的資源的權限。 若是如此，要求將會繼續。 否則，要求會傳回為未獲授權。 這種方法，使用 cookie，以讓應用程式看起來似乎可設定狀態，因為它是可以 「 記住 」 的使用者之前驗證與伺服器。

### <a name="user-tokens"></a>使用者語彙基元

權杖型驗證不會儲存在伺服器上的工作階段。 相反地，當使用者登入它們正在發行的權杖 （不 antiforgery 語彙基元）。 這個語彙基元包含所有已驗證權杖所需的資料。 它也包含使用者資訊，請在表單的[宣告](https://docs.microsoft.com/dotnet/framework/security/claims-based-identity-model)。 當使用者想要存取需要驗證的伺服器資源時，權杖會傳送到其他授權標頭形式 Bearer {t} 的伺服器。 這可讓應用程式無狀態因為在每個後續的要求語彙基元，在要求中傳遞伺服器端驗證。 這個語彙基元不*加密*; 而是*編碼*。 在伺服器端權杖可以解碼存取權杖中的原始資訊。 若要在後續要求中傳送的語彙基元，您可以儲存它在瀏覽器的本機儲存體，或在 cookie 中。 您不必擔心 XSRF 弱點，如果您的權杖會儲存在本機儲存體中，但它會是問題，如果語彙基元，儲存在 cookie 中。

### <a name="multiple-applications-are-hosted-in-one-domain"></a>在一個網域中裝載多個應用程式

即使`example1.cloudapp.net`和`example2.cloudapp.net`是不同的主控件下的所有主機之間沒有隱含的信任關係`*.cloudapp.net`網域。 這個隱含的信任關係可讓可能不受信任的主機會影響彼此的 cookie （控管 AJAX 要求的相同原始原則不一定會套用至 HTTP cookie）。 ASP.NET Core 執行階段提供某些風險降低，因為即使惡意的子網域可將覆寫工作階段語彙基元會無法產生使用者的有效的欄位語彙基元欄位語彙基元中，內嵌使用者名稱。 不過，這類環境中裝載時的內建的防 XSRF 常式仍無法防止工作階段攔截或登入 CSRF 攻擊。 共用的裝載環境包括 vunerable 劫持、 登入 CSRF，和其他攻擊。


### <a name="additional-resources"></a>其他資源

* [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))上[開啟 Web 應用程式安全性專案](https://www.owasp.org/index.php/Main_Page)(OWASP)。
