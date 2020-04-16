---
title: ASP.NET核心Blazor全球化和當地語系化
author: guardrex
description: 瞭解如何使 Razor 元件可供多種語言和語言中的用戶訪問。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/14/2020
no-loc:
- Blazor
- SignalR
uid: blazor/globalization-localization
ms.openlocfilehash: 0883a67e0129590f7a3fb68689eaba8d85e5523f
ms.sourcegitcommit: 6c8cff2d6753415c4f5d2ffda88159a7f6f7431a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/16/2020
ms.locfileid: "81440710"
---
# <a name="aspnet-core-opno-locblazor-globalization-and-localization"></a>ASP.NET核心Blazor全球化和當地語系化

由[盧克·萊瑟姆](https://github.com/guardrex)和[丹尼爾·羅斯](https://github.com/danroth27)

Razor 元件可供多種語言和語言中的用戶訪問。 以下 .NET 全球化與當地語系化機制

* .NET 的資源系統
* 特定於區域性的數字與日期格式

目前支援一組有限的ASP.NET Core 的當地語系化方案:

* `IStringLocalizer<>`Blazor在應用程式中*受支援*。
* `IHtmlLocalizer<>`和數據註釋當地語系化ASP.NET核心 MVCBlazor方案,並且在應用中**不受支援。** `IViewLocalizer<>`

如需詳細資訊，請參閱 <xref:fundamentals/localization>。

## <a name="globalization"></a>全球化

Blazor功能`@bind`根據使用者當前區域性執行格式並分析顯示值。

可以從<xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName>屬性訪問當前區域性。

[區域性資訊.不變區域性](xref:System.Globalization.CultureInfo.InvariantCulture)用於以下欄位型`<input type="{TYPE}" />`態 ( ):

* `date`
* `number`

前面的欄位型態:

* 使用其適當的基於瀏覽器的格式規則顯示。
* 不能包含自由格式的文本。
* 根據瀏覽器的實現提供使用者交互特徵。

以下欄位類型具有特定的格式設定要求,並且目前不受支援,Blazor因為它們不受所有主要瀏覽器的支援:

* `datetime-local`
* `month`
* `week`

`@bind`支援`@bind:culture`參數<xref:System.Globalization.CultureInfo?displayProperty=fullName>以 提供 用於解析和格式化值的 參數。 使用`date`和`number`欄位類型時,不建議指定區域性。 `date`並`number`內置Blazor支援,提供所需的區域性。

## <a name="localization"></a>當地語系化

### <a name="opno-locblazor-webassembly"></a>Blazor網路組裝

默認情況下BlazorBlazor,WebAssembly 應用的連結器配置會剝離國際化資訊,但顯式請求區域設置除外。 有關控制連結器行為的詳細資訊和指導,請參閱<xref:host-and-deploy/blazor/configure-linker#configure-the-linker-for-internationalization>。

<!-- HOLD FOR 3.2 PREVIEW 4: Replace prior paragraph with ...

Blazor WebAssembly apps set the culture using the user's [language preference](https://developer.mozilla.org/docs/Web/API/NavigatorLanguage/languages).

To explicitly configure the culture, set `CultureInfo.DefaultThreadCurrentCulture` and `CultureInfo.DefaultThreadCurrentUICulture` in `Program.Main`.

By default, Blazor's linker configuration for Blazor WebAssembly apps strips out internationalization information except for locales explicitly requested. For more information and guidance on controlling the linker's behavior, see <xref:host-and-deploy/blazor/configure-linker#configure-the-linker-for-internationalization>.

While the culture that Blazor selects by default might be sufficient for most users, consider offering a way for users to specify their preferred locale. For a Blazor WebAssembly sample app with a culture picker, see the [LocSample](https://github.com/pranavkm/LocSample) localization sample app.

-->

### <a name="opno-locblazor-server"></a>Blazor伺服器

Blazor伺服器應用程式是本地化使用[本地化中間件](xref:fundamentals/localization#localization-middleware)。 中間件為從應用請求資源的用戶選擇適當的區域性。

可以使用以下方法之一設定區域性:

* [Cookie](#cookies)
* [提供 UI 以選擇區域性](#provide-ui-to-choose-the-culture)

如需詳細資訊和範例，請參閱 <xref:fundamentals/localization>。

#### <a name="cookies"></a>Cookie

當地語系化區域性 Cookie 可以持久化使用者的文化。 Cookie 由應用程式的主`OnGet`機頁 (*頁面/ Host.cshtml.cs)* 的方法建立。 當地語系化中間件讀取後續設置使用者區域性的請求上的 Cookie。 

使用 Cookie 可確保 WebSocket 連接可以正確傳播區域性。 如果當地語系化方案基於 URL 路徑或查詢字串,則該方案可能無法使用 WebSocket,從而無法持久化區域性。 因此,建議使用本地化區域性 Cookie。

如果區域性保留在當地語系化 Cookie 中,則任何技術都可用於分配區域性。 如果應用已為伺服器端ASP.NET Core 建立了當地語系化方案,請繼續使用應用的現有當地語系化基礎結構,並在應用方案中設置當地語系化區域性 Cookie。

下面的示例演示如何在 Cookie 中設置當前區域性,該 Cookie 可以由本地化中間件讀取。 在Blazor'伺服器'應用中建立包含以下內容的*頁面/_Host.cs*檔:

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

本地化由套用的事件排序處理:

1. 瀏覽器向應用發送初始 HTTP 請求。
1. 區域性由本地化中間件分配。
1. `OnGet` *_Host.cshtml.cs*中的方法將區域性保留在 Cookie 中作為回應的一部分。
1. 瀏覽器打開 WebSocket 連接以建立Blazor互動式 伺服器作業階段。
1. 當地語系化中間件讀取 Cookie 並分配區域性。
1. 伺服器Blazor會話以正確的區域性開始。

#### <a name="provide-ui-to-choose-the-culture"></a>提供 UI 以選擇區域性

要提供 UI 以允許使用者選擇區域性,建議採用*基於重定向的方法*。 此過程類似於用戶嘗試存取安全資源時 Web 應用中發生的情況。 使用者被重定向到登錄頁,然後重定向回原始資源。 

應用通過重定向到控制器來保留使用者的選定區域性。 控制器將使用者所選區域性設置到 Cookie 中,並將使用者重定向回原始 URI。

在伺服器上建立 HTTP 終結點,以在 Cookie 中設定使用者所選區域性,並執行重定向回原始 URI:

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
> 使用`LocalRedirect`操作結果可防止打開重定向攻擊。 如需詳細資訊，請參閱 <xref:security/preventing-open-redirects>。

以下元件顯示了使用者選擇區域性時如何執行初始重定向的範例:

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
