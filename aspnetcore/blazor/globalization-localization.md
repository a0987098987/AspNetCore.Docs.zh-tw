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
ms.openlocfilehash: 1b0db66b23c0caffc6b7c4e4af723c020609612a
ms.sourcegitcommit: d5d45d84fe488427d418de770000f7df44a08370
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2020
ms.locfileid: "81539657"
---
# <a name="aspnet-core-opno-locblazor-globalization-and-localization"></a><span data-ttu-id="82676-103">ASP.NET核心Blazor全球化和當地語系化</span><span class="sxs-lookup"><span data-stu-id="82676-103">ASP.NET Core Blazor globalization and localization</span></span>

<span data-ttu-id="82676-104">由[盧克·萊瑟姆](https://github.com/guardrex)和[丹尼爾·羅斯](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="82676-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="82676-105">Razor 元件可供多種語言和語言中的用戶訪問。</span><span class="sxs-lookup"><span data-stu-id="82676-105">Razor components can be made accessible to users in multiple cultures and languages.</span></span> <span data-ttu-id="82676-106">以下 .NET 全球化與當地語系化機制</span><span class="sxs-lookup"><span data-stu-id="82676-106">The following .NET globalization and localization scenarios are available:</span></span>

* <span data-ttu-id="82676-107">.NET 的資源系統</span><span class="sxs-lookup"><span data-stu-id="82676-107">.NET's resources system</span></span>
* <span data-ttu-id="82676-108">特定於區域性的數字與日期格式</span><span class="sxs-lookup"><span data-stu-id="82676-108">Culture-specific number and date formatting</span></span>

<span data-ttu-id="82676-109">目前支援一組有限的ASP.NET Core 的當地語系化方案:</span><span class="sxs-lookup"><span data-stu-id="82676-109">A limited set of ASP.NET Core's localization scenarios are currently supported:</span></span>

* <span data-ttu-id="82676-110">`IStringLocalizer<>`Blazor在應用程式中*受支援*。</span><span class="sxs-lookup"><span data-stu-id="82676-110">`IStringLocalizer<>` *is supported* in Blazor apps.</span></span>
* <span data-ttu-id="82676-111">`IHtmlLocalizer<>`和數據註釋當地語系化ASP.NET核心 MVCBlazor方案,並且在應用中**不受支援。** `IViewLocalizer<>`</span><span class="sxs-lookup"><span data-stu-id="82676-111">`IHtmlLocalizer<>`, `IViewLocalizer<>`, and Data Annotations localization are ASP.NET Core MVC scenarios and **not supported** in Blazor apps.</span></span>

<span data-ttu-id="82676-112">如需詳細資訊，請參閱 <xref:fundamentals/localization>。</span><span class="sxs-lookup"><span data-stu-id="82676-112">For more information, see <xref:fundamentals/localization>.</span></span>

## <a name="globalization"></a><span data-ttu-id="82676-113">全球化</span><span class="sxs-lookup"><span data-stu-id="82676-113">Globalization</span></span>

Blazor<span data-ttu-id="82676-114">功能`@bind`根據使用者當前區域性執行格式並分析顯示值。</span><span class="sxs-lookup"><span data-stu-id="82676-114">'s `@bind` functionality performs formats and parses values for display based on the user's current culture.</span></span>

<span data-ttu-id="82676-115">可以從<xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName>屬性訪問當前區域性。</span><span class="sxs-lookup"><span data-stu-id="82676-115">The current culture can be accessed from the <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> property.</span></span>

<span data-ttu-id="82676-116">[區域性資訊.不變區域性](xref:System.Globalization.CultureInfo.InvariantCulture)用於以下欄位型`<input type="{TYPE}" />`態 ( ):</span><span class="sxs-lookup"><span data-stu-id="82676-116">[CultureInfo.InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) is used for the following field types (`<input type="{TYPE}" />`):</span></span>

* `date`
* `number`

<span data-ttu-id="82676-117">前面的欄位型態:</span><span class="sxs-lookup"><span data-stu-id="82676-117">The preceding field types:</span></span>

* <span data-ttu-id="82676-118">使用其適當的基於瀏覽器的格式規則顯示。</span><span class="sxs-lookup"><span data-stu-id="82676-118">Are displayed using their appropriate browser-based formatting rules.</span></span>
* <span data-ttu-id="82676-119">不能包含自由格式的文本。</span><span class="sxs-lookup"><span data-stu-id="82676-119">Can't contain free-form text.</span></span>
* <span data-ttu-id="82676-120">根據瀏覽器的實現提供使用者交互特徵。</span><span class="sxs-lookup"><span data-stu-id="82676-120">Provide user interaction characteristics based on the browser's implementation.</span></span>

<span data-ttu-id="82676-121">以下欄位類型具有特定的格式設定要求,並且目前不受支援,Blazor因為它們不受所有主要瀏覽器的支援:</span><span class="sxs-lookup"><span data-stu-id="82676-121">The following field types have specific formatting requirements and aren't currently supported by Blazor because they aren't supported by all major browsers:</span></span>

* `datetime-local`
* `month`
* `week`

<span data-ttu-id="82676-122">`@bind`支援`@bind:culture`參數<xref:System.Globalization.CultureInfo?displayProperty=fullName>以 提供 用於解析和格式化值的 參數。</span><span class="sxs-lookup"><span data-stu-id="82676-122">`@bind` supports the `@bind:culture` parameter to provide a <xref:System.Globalization.CultureInfo?displayProperty=fullName> for parsing and formatting a value.</span></span> <span data-ttu-id="82676-123">使用`date`和`number`欄位類型時,不建議指定區域性。</span><span class="sxs-lookup"><span data-stu-id="82676-123">Specifying a culture isn't recommended when using the `date` and `number` field types.</span></span> <span data-ttu-id="82676-124">`date`並`number`內置Blazor支援,提供所需的區域性。</span><span class="sxs-lookup"><span data-stu-id="82676-124">`date` and `number` have built-in Blazor support that provides the required culture.</span></span>

## <a name="localization"></a><span data-ttu-id="82676-125">當地語系化</span><span class="sxs-lookup"><span data-stu-id="82676-125">Localization</span></span>

### <a name="opno-locblazor-webassembly"></a>Blazor<span data-ttu-id="82676-126">網路組裝</span><span class="sxs-lookup"><span data-stu-id="82676-126"> WebAssembly</span></span>

Blazor<span data-ttu-id="82676-127">WebAssembly 應用使用使用者[的語言首選項](https://developer.mozilla.org/docs/Web/API/NavigatorLanguage/languages)設置區域性。</span><span class="sxs-lookup"><span data-stu-id="82676-127"> WebAssembly apps set the culture using the user's [language preference](https://developer.mozilla.org/docs/Web/API/NavigatorLanguage/languages).</span></span>

<span data-ttu-id="82676-128">要顯示式設定區域性,請設定`CultureInfo.DefaultThreadCurrentCulture`與`CultureInfo.DefaultThreadCurrentUICulture``Program.Main`。</span><span class="sxs-lookup"><span data-stu-id="82676-128">To explicitly configure the culture, set `CultureInfo.DefaultThreadCurrentCulture` and `CultureInfo.DefaultThreadCurrentUICulture` in `Program.Main`.</span></span>

<span data-ttu-id="82676-129">默認情況下BlazorBlazor,WebAssembly 應用的連結器配置會剝離國際化資訊,但顯式請求區域設置除外。</span><span class="sxs-lookup"><span data-stu-id="82676-129">By default, Blazor's linker configuration for Blazor WebAssembly apps strips out internationalization information except for locales explicitly requested.</span></span> <span data-ttu-id="82676-130">有關控制連結器行為的詳細資訊和指導,請參閱<xref:host-and-deploy/blazor/configure-linker#configure-the-linker-for-internationalization>。</span><span class="sxs-lookup"><span data-stu-id="82676-130">For more information and guidance on controlling the linker's behavior, see <xref:host-and-deploy/blazor/configure-linker#configure-the-linker-for-internationalization>.</span></span>

<span data-ttu-id="82676-131">雖然默認情況下Blazor選擇的區域性對於大多數用戶來說可能就足夠了,但請考慮為使用者提供一種指定其首選區域設置的方法。</span><span class="sxs-lookup"><span data-stu-id="82676-131">While the culture that Blazor selects by default might be sufficient for most users, consider offering a way for users to specify their preferred locale.</span></span> <span data-ttu-id="82676-132">有關具有Blazor區域性選取器的 WebAssembly 範例應用程式,請參閱[LocSample](https://github.com/pranavkm/LocSample)當地語系化範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="82676-132">For a Blazor WebAssembly sample app with a culture picker, see the [LocSample](https://github.com/pranavkm/LocSample) localization sample app.</span></span>

### <a name="opno-locblazor-server"></a>Blazor<span data-ttu-id="82676-133">伺服器</span><span class="sxs-lookup"><span data-stu-id="82676-133"> Server</span></span>

Blazor<span data-ttu-id="82676-134">伺服器應用程式是本地化使用[本地化中間件](xref:fundamentals/localization#localization-middleware)。</span><span class="sxs-lookup"><span data-stu-id="82676-134"> Server apps are localized using [Localization Middleware](xref:fundamentals/localization#localization-middleware).</span></span> <span data-ttu-id="82676-135">中間件為從應用請求資源的用戶選擇適當的區域性。</span><span class="sxs-lookup"><span data-stu-id="82676-135">The middleware selects the appropriate culture for users requesting resources from the app.</span></span>

<span data-ttu-id="82676-136">可以使用以下方法之一設定區域性:</span><span class="sxs-lookup"><span data-stu-id="82676-136">The culture can be set using one of the following approaches:</span></span>

* [<span data-ttu-id="82676-137">Cookie</span><span class="sxs-lookup"><span data-stu-id="82676-137">Cookies</span></span>](#cookies)
* [<span data-ttu-id="82676-138">提供 UI 以選擇區域性</span><span class="sxs-lookup"><span data-stu-id="82676-138">Provide UI to choose the culture</span></span>](#provide-ui-to-choose-the-culture)

<span data-ttu-id="82676-139">如需詳細資訊和範例，請參閱 <xref:fundamentals/localization>。</span><span class="sxs-lookup"><span data-stu-id="82676-139">For more information and examples, see <xref:fundamentals/localization>.</span></span>

#### <a name="cookies"></a><span data-ttu-id="82676-140">Cookie</span><span class="sxs-lookup"><span data-stu-id="82676-140">Cookies</span></span>

<span data-ttu-id="82676-141">當地語系化區域性 Cookie 可以持久化使用者的文化。</span><span class="sxs-lookup"><span data-stu-id="82676-141">A localization culture cookie can persist the user's culture.</span></span> <span data-ttu-id="82676-142">Cookie 由應用程式的主`OnGet`機頁 (*頁面/ Host.cshtml.cs)* 的方法建立。</span><span class="sxs-lookup"><span data-stu-id="82676-142">The cookie is created by the `OnGet` method of the app's host page (*Pages/Host.cshtml.cs*).</span></span> <span data-ttu-id="82676-143">當地語系化中間件讀取後續設置使用者區域性的請求上的 Cookie。</span><span class="sxs-lookup"><span data-stu-id="82676-143">The Localization Middleware reads the cookie on subsequent requests to set the user's culture.</span></span> 

<span data-ttu-id="82676-144">使用 Cookie 可確保 WebSocket 連接可以正確傳播區域性。</span><span class="sxs-lookup"><span data-stu-id="82676-144">Use of a cookie ensures that the WebSocket connection can correctly propagate the culture.</span></span> <span data-ttu-id="82676-145">如果當地語系化方案基於 URL 路徑或查詢字串,則該方案可能無法使用 WebSocket,從而無法持久化區域性。</span><span class="sxs-lookup"><span data-stu-id="82676-145">If localization schemes are based on the URL path or query string, the scheme might not be able to work with WebSockets, thus fail to persist the culture.</span></span> <span data-ttu-id="82676-146">因此,建議使用本地化區域性 Cookie。</span><span class="sxs-lookup"><span data-stu-id="82676-146">Therefore, use of a localization culture cookie is the recommended approach.</span></span>

<span data-ttu-id="82676-147">如果區域性保留在當地語系化 Cookie 中,則任何技術都可用於分配區域性。</span><span class="sxs-lookup"><span data-stu-id="82676-147">Any technique can be used to assign a culture if the culture is persisted in a localization cookie.</span></span> <span data-ttu-id="82676-148">如果應用已為伺服器端ASP.NET Core 建立了當地語系化方案,請繼續使用應用的現有當地語系化基礎結構,並在應用方案中設置當地語系化區域性 Cookie。</span><span class="sxs-lookup"><span data-stu-id="82676-148">If the app already has an established localization scheme for server-side ASP.NET Core, continue to use the app's existing localization infrastructure and set the localization culture cookie within the app's scheme.</span></span>

<span data-ttu-id="82676-149">下面的示例演示如何在 Cookie 中設置當前區域性,該 Cookie 可以由本地化中間件讀取。</span><span class="sxs-lookup"><span data-stu-id="82676-149">The following example shows how to set the current culture in a cookie that can be read by the Localization Middleware.</span></span> <span data-ttu-id="82676-150">在Blazor'伺服器'應用中建立包含以下內容的*頁面/_Host.cs*檔:</span><span class="sxs-lookup"><span data-stu-id="82676-150">Create a *Pages/_Host.cshtml.cs* file with the following contents in the Blazor Server app:</span></span>

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

<span data-ttu-id="82676-151">本地化由套用的事件排序處理:</span><span class="sxs-lookup"><span data-stu-id="82676-151">Localization is handled by the app in the following sequence of events:</span></span>

1. <span data-ttu-id="82676-152">瀏覽器向應用發送初始 HTTP 請求。</span><span class="sxs-lookup"><span data-stu-id="82676-152">The browser sends an initial HTTP request to the app.</span></span>
1. <span data-ttu-id="82676-153">區域性由本地化中間件分配。</span><span class="sxs-lookup"><span data-stu-id="82676-153">The culture is assigned by the Localization Middleware.</span></span>
1. <span data-ttu-id="82676-154">`OnGet` *_Host.cshtml.cs*中的方法將區域性保留在 Cookie 中作為回應的一部分。</span><span class="sxs-lookup"><span data-stu-id="82676-154">The `OnGet` method in *_Host.cshtml.cs* persists the culture in a cookie as part of the response.</span></span>
1. <span data-ttu-id="82676-155">瀏覽器打開 WebSocket 連接以建立Blazor互動式 伺服器作業階段。</span><span class="sxs-lookup"><span data-stu-id="82676-155">The browser opens a WebSocket connection to create an interactive Blazor Server session.</span></span>
1. <span data-ttu-id="82676-156">當地語系化中間件讀取 Cookie 並分配區域性。</span><span class="sxs-lookup"><span data-stu-id="82676-156">The Localization Middleware reads the cookie and assigns the culture.</span></span>
1. <span data-ttu-id="82676-157">伺服器Blazor會話以正確的區域性開始。</span><span class="sxs-lookup"><span data-stu-id="82676-157">The Blazor Server session begins with the correct culture.</span></span>

#### <a name="provide-ui-to-choose-the-culture"></a><span data-ttu-id="82676-158">提供 UI 以選擇區域性</span><span class="sxs-lookup"><span data-stu-id="82676-158">Provide UI to choose the culture</span></span>

<span data-ttu-id="82676-159">要提供 UI 以允許使用者選擇區域性,建議採用*基於重定向的方法*。</span><span class="sxs-lookup"><span data-stu-id="82676-159">To provide UI to allow a user to select a culture, a *redirect-based approach* is recommended.</span></span> <span data-ttu-id="82676-160">此過程類似於用戶嘗試存取安全資源時 Web 應用中發生的情況。</span><span class="sxs-lookup"><span data-stu-id="82676-160">The process is similar to what happens in a web app when a user attempts to access a secure resource.</span></span> <span data-ttu-id="82676-161">使用者被重定向到登錄頁,然後重定向回原始資源。</span><span class="sxs-lookup"><span data-stu-id="82676-161">The user is redirected to a sign-in page and then redirected back to the original resource.</span></span> 

<span data-ttu-id="82676-162">應用通過重定向到控制器來保留使用者的選定區域性。</span><span class="sxs-lookup"><span data-stu-id="82676-162">The app persists the user's selected culture via a redirect to a controller.</span></span> <span data-ttu-id="82676-163">控制器將使用者所選區域性設置到 Cookie 中,並將使用者重定向回原始 URI。</span><span class="sxs-lookup"><span data-stu-id="82676-163">The controller sets the user's selected culture into a cookie and redirects the user back to the original URI.</span></span>

<span data-ttu-id="82676-164">在伺服器上建立 HTTP 終結點,以在 Cookie 中設定使用者所選區域性,並執行重定向回原始 URI:</span><span class="sxs-lookup"><span data-stu-id="82676-164">Establish an HTTP endpoint on the server to set the user's selected culture in a cookie and perform the redirect back to the original URI:</span></span>

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
> <span data-ttu-id="82676-165">使用`LocalRedirect`操作結果可防止打開重定向攻擊。</span><span class="sxs-lookup"><span data-stu-id="82676-165">Use the `LocalRedirect` action result to prevent open redirect attacks.</span></span> <span data-ttu-id="82676-166">如需詳細資訊，請參閱 <xref:security/preventing-open-redirects>。</span><span class="sxs-lookup"><span data-stu-id="82676-166">For more information, see <xref:security/preventing-open-redirects>.</span></span>

<span data-ttu-id="82676-167">以下元件顯示了使用者選擇區域性時如何執行初始重定向的範例:</span><span class="sxs-lookup"><span data-stu-id="82676-167">The following component shows an example of how to perform the initial redirection when the user selects a culture:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="82676-168">其他資源</span><span class="sxs-lookup"><span data-stu-id="82676-168">Additional resources</span></span>

* <xref:fundamentals/localization>
