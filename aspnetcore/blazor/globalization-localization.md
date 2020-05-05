---
title: ASP.NET Core Blazor全球化和當地語系化
author: guardrex
description: 瞭解如何讓Razor具有多種文化特性和語言的使用者能夠存取元件。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/14/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/globalization-localization
ms.openlocfilehash: b39695f8b506744b4af27a1d7e09bfac9594d7ca
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82772487"
---
# <a name="aspnet-core-blazor-globalization-and-localization"></a>ASP.NET Core Blazor全球化和當地語系化

By [Luke Latham](https://github.com/guardrex)和[Daniel Roth](https://github.com/danroth27)

Razor元件可以讓使用者以多種文化特性和語言來存取。 以下是可用的 .NET 全球化和當地語系化案例：

* .NET 的資源系統
* 特定文化特性的數位和日期格式

目前支援一組有限的 ASP.NET Core 當地語系化案例：

* `IStringLocalizer<>`應用程式*支援。* Blazor
* `IHtmlLocalizer<>`、 `IViewLocalizer<>`和資料批註當地語系化 ASP.NET Core MVC 案例中Blazor ，而且應用程式**不支援**。

如需詳細資訊，請參閱<xref:fundamentals/localization>。

## <a name="globalization"></a>全球化

Blazor的`@bind`功能會執行格式，並根據使用者目前的文化特性來剖析要顯示的值。

您可以從<xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName>屬性存取目前的文化特性。

[InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture)用於下欄欄位類型（`<input type="{TYPE}" />`）：

* `date`
* `number`

前面的欄位類型：

* 會使用其適當的瀏覽器格式規則來顯示。
* 不能包含自由格式的文字。
* 根據瀏覽器的執行方式提供使用者互動特性。

下欄欄位類型具有特定的格式需求，目前不支援， Blazor因為所有主要瀏覽器都不支援它們：

* `datetime-local`
* `month`
* `week`

`@bind`支援`@bind:culture`參數，以提供<xref:System.Globalization.CultureInfo?displayProperty=fullName>用於剖析和格式化值的。 使用`date`和欄位類型時， `number`不建議指定文化特性。 `date`和`number`具有提供必要文化Blazor特性的內建支援。

## <a name="localization"></a>當地語系化

### <a name="blazor-webassembly"></a>BlazorWebAssembly

BlazorWebAssembly apps 會使用使用者的[語言喜好](https://developer.mozilla.org/docs/Web/API/NavigatorLanguage/languages)設定來設定文化特性。

若要明確設定文化特性， `CultureInfo.DefaultThreadCurrentCulture`請`CultureInfo.DefaultThreadCurrentUICulture`在`Program.Main`中設定和。

根據預設， Blazor Blazor WebAssembly 應用程式的連結器設定會去除國際化資訊，但不包括明確要求的地區設定。 如需控制連結器行為的詳細資訊和指引，請<xref:host-and-deploy/blazor/configure-linker#configure-the-linker-for-internationalization>參閱。

雖然預設Blazor選取的文化特性可能足以滿足大部分使用者的需求，但請考慮提供一種方法讓使用者指定其慣用的地區設定。 如需Blazor具有文化特性選擇器的 WebAssembly 範例應用程式，請參閱[LocSample](https://github.com/pranavkm/LocSample)當地語系化範例應用程式。

### <a name="blazor-server"></a>Blazor伺服器

Blazor伺服器應用程式是使用[當地語系化中介軟體](xref:fundamentals/localization#localization-middleware)來當地語系化。 中介軟體會為從應用程式要求資源的使用者選取適當的文化特性。

您可以使用下列其中一種方法來設定文化特性：

* [Cookie](#cookies)
* [提供 UI 以選擇文化特性](#provide-ui-to-choose-the-culture)

如需詳細資訊和範例，請參閱 <xref:fundamentals/localization>。

#### <a name="cookies"></a>Cookie

當地語系化文化特性 cookie 可以保存使用者的文化特性。 Cookie 是由應用程式的`OnGet`主機頁面（*Pages/主機. cshtml .cs*）的方法所建立。 當地語系化中介軟體會在後續要求中讀取 cookie，以設定使用者的文化特性。 

使用 cookie 可確保 WebSocket 連接可以正確地傳播文化特性。 如果當地語系化架構是以 URL 路徑或查詢字串為基礎，配置可能無法使用 Websocket，因而無法保存文化特性。 因此，建議的方法是使用當地語系化文化特性 cookie。

如果文化特性保存在當地語系化 cookie 中，任何技術都可以用來指派文化特性。 如果應用程式已建立伺服器端 ASP.NET Core 的當地語系化配置，請繼續使用應用程式的現有當地語系化基礎結構，並在應用程式的配置內設定當地語系化文化特性 cookie。

下列範例顯示如何在可由當地語系化中介軟體讀取的 cookie 中設定目前的文化特性。 使用Blazor伺服器應用程式中的下列內容，建立*Pages/_Host. cshtml*檔案：

```csharp
public class HostModel : PageModel
{
    public void OnGet()
    {
        HttpContext.Response.Cookies.Append(
            CookieRequestCultureProvider.DefaultCookieName,
            CookieRequestCultureProvider.MakeCookieValue(
                new RequestCulture(
                    CultureInfo.CurrentCulture,
                    CultureInfo.CurrentUICulture)));
    }
}
```

應用程式會依照下列事件順序來處理當地語系化：

1. 瀏覽器會將初始 HTTP 要求傳送至應用程式。
1. 文化特性是由當地語系化中介軟體所指派。
1. _Host `OnGet`中的 *_Host.cshtml.cs*方法會在 cookie 中保存文化特性，做為回應的一部分。
1. 瀏覽器會開啟 WebSocket 連接，以建立互動式Blazor伺服器會話。
1. 當地語系化中介軟體會讀取 cookie 並指派文化特性。
1. Blazor伺服器會話的開頭是正確的文化特性。

#### <a name="provide-ui-to-choose-the-culture"></a>提供 UI 以選擇文化特性

為了提供允許使用者選取文化特性的 UI，建議使用以重新*導向為基礎的方法*。 此程式類似于使用者嘗試存取安全資源時，在 web 應用程式中發生的情況。 使用者重新導向至登入頁面，然後重新導向回原始資源。 

應用程式會透過重新導向至控制器的方式，來保存使用者選取的文化特性。 控制器會將使用者選取的文化特性設定為 cookie，並將使用者重新導向回到原始 URI。

在伺服器上建立 HTTP 端點，以在 cookie 中設定使用者選取的文化特性，並執行重新導向回到原始 URI：

```csharp
[Route("[controller]/[action]")]
public class CultureController : Controller
{
    public IActionResult SetCulture(string culture, string redirectUri)
    {
        if (culture != null)
        {
            HttpContext.Response.Cookies.Append(
                CookieRequestCultureProvider.DefaultCookieName,
                CookieRequestCultureProvider.MakeCookieValue(
                    new RequestCulture(culture)));
        }

        return LocalRedirect(redirectUri);
    }
}
```

> [!WARNING]
> 使用`LocalRedirect`動作結果來防止開啟重新導向攻擊。 如需詳細資訊，請參閱<xref:security/preventing-open-redirects>。

下列元件顯示當使用者選取文化特性時，如何執行初始重新導向的範例：

```razor
@inject NavigationManager NavigationManager

<h3>Select your language</h3>

<select @onchange="OnSelected">
    <option>Select...</option>
    <option value="en-US">English</option>
    <option value="fr-FR">Français</option>
</select>

@code {
    private void OnSelected(ChangeEventArgs e)
    {
        var culture = (string)e.Value;
        var uri = new Uri(NavigationManager.Uri)
            .GetComponents(UriComponents.PathAndQuery, UriFormat.Unescaped);
        var query = $"?culture={Uri.EscapeDataString(culture)}&" +
            $"redirectUri={Uri.EscapeDataString(uri)}";

        NavigationManager.NavigateTo("/Culture/SetCulture" + query, forceLoad: true);
    }
}
```

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/localization>
