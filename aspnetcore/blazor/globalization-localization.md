---
title: ASP.NET Core Blazor 全球化和當地語系化
author: guardrex
description: 瞭解如何讓使用者在多種文化特性和語言中存取 Razor 元件。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/globalization-localization
ms.openlocfilehash: aba62fa7b6285c8ba884652694f1ea3e3a66ed18
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/19/2020
ms.locfileid: "77453265"
---
# <a name="aspnet-core-opno-locblazor-globalization-and-localization"></a><span data-ttu-id="7b6c8-103">ASP.NET Core Blazor 全球化和當地語系化</span><span class="sxs-lookup"><span data-stu-id="7b6c8-103">ASP.NET Core Blazor globalization and localization</span></span>

<span data-ttu-id="7b6c8-104">By [Luke Latham](https://github.com/guardrex)和[Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="7b6c8-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="7b6c8-105">Razor 元件可讓使用者以多種文化特性和語言存取。</span><span class="sxs-lookup"><span data-stu-id="7b6c8-105">Razor components can be made accessible to users in multiple cultures and languages.</span></span> <span data-ttu-id="7b6c8-106">以下是可用的 .NET 全球化和當地語系化案例：</span><span class="sxs-lookup"><span data-stu-id="7b6c8-106">The following .NET globalization and localization scenarios are available:</span></span>

* <span data-ttu-id="7b6c8-107">.NET 的資源系統</span><span class="sxs-lookup"><span data-stu-id="7b6c8-107">.NET's resources system</span></span>
* <span data-ttu-id="7b6c8-108">特定文化特性的數位和日期格式</span><span class="sxs-lookup"><span data-stu-id="7b6c8-108">Culture-specific number and date formatting</span></span>

<span data-ttu-id="7b6c8-109">目前支援一組有限的 ASP.NET Core 當地語系化案例：</span><span class="sxs-lookup"><span data-stu-id="7b6c8-109">A limited set of ASP.NET Core's localization scenarios are currently supported:</span></span>

* <span data-ttu-id="7b6c8-110">Blazor 應用程式*支援*`IStringLocalizer<>`。</span><span class="sxs-lookup"><span data-stu-id="7b6c8-110">`IStringLocalizer<>` *is supported* in Blazor apps.</span></span>
* <span data-ttu-id="7b6c8-111">`IHtmlLocalizer<>`、`IViewLocalizer<>`和資料批註的當地語系化是 ASP.NET Core MVC 案例，而且 Blazor 應用程式中**不支援**。</span><span class="sxs-lookup"><span data-stu-id="7b6c8-111">`IHtmlLocalizer<>`, `IViewLocalizer<>`, and Data Annotations localization are ASP.NET Core MVC scenarios and **not supported** in Blazor apps.</span></span>

<span data-ttu-id="7b6c8-112">如需詳細資訊，請參閱 <xref:fundamentals/localization>。</span><span class="sxs-lookup"><span data-stu-id="7b6c8-112">For more information, see <xref:fundamentals/localization>.</span></span>

## <a name="globalization"></a><span data-ttu-id="7b6c8-113">全球化</span><span class="sxs-lookup"><span data-stu-id="7b6c8-113">Globalization</span></span>

Blazor<span data-ttu-id="7b6c8-114">的 `@bind` 功能會執行格式，並根據使用者目前的文化特性來剖析要顯示的值。</span><span class="sxs-lookup"><span data-stu-id="7b6c8-114">'s `@bind` functionality performs formats and parses values for display based on the user's current culture.</span></span>

<span data-ttu-id="7b6c8-115">您可以從 <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> 屬性存取目前的文化特性。</span><span class="sxs-lookup"><span data-stu-id="7b6c8-115">The current culture can be accessed from the <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> property.</span></span>

<span data-ttu-id="7b6c8-116">[InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture)用於下欄欄位類型（`<input type="{TYPE}" />`）：</span><span class="sxs-lookup"><span data-stu-id="7b6c8-116">[CultureInfo.InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) is used for the following field types (`<input type="{TYPE}" />`):</span></span>

* `date`
* `number`

<span data-ttu-id="7b6c8-117">前面的欄位類型：</span><span class="sxs-lookup"><span data-stu-id="7b6c8-117">The preceding field types:</span></span>

* <span data-ttu-id="7b6c8-118">會使用其適當的瀏覽器格式規則來顯示。</span><span class="sxs-lookup"><span data-stu-id="7b6c8-118">Are displayed using their appropriate browser-based formatting rules.</span></span>
* <span data-ttu-id="7b6c8-119">不能包含自由格式的文字。</span><span class="sxs-lookup"><span data-stu-id="7b6c8-119">Can't contain free-form text.</span></span>
* <span data-ttu-id="7b6c8-120">根據瀏覽器的執行方式提供使用者互動特性。</span><span class="sxs-lookup"><span data-stu-id="7b6c8-120">Provide user interaction characteristics based on the browser's implementation.</span></span>

<span data-ttu-id="7b6c8-121">下欄欄位類型具有特定的格式需求，而且目前不支援 Blazor，因為所有主要瀏覽器都不支援：</span><span class="sxs-lookup"><span data-stu-id="7b6c8-121">The following field types have specific formatting requirements and aren't currently supported by Blazor because they aren't supported by all major browsers:</span></span>

* `datetime-local`
* `month`
* `week`

<span data-ttu-id="7b6c8-122">`@bind` 支援 `@bind:culture` 參數，以提供用來剖析和格式化值的 <xref:System.Globalization.CultureInfo?displayProperty=fullName>。</span><span class="sxs-lookup"><span data-stu-id="7b6c8-122">`@bind` supports the `@bind:culture` parameter to provide a <xref:System.Globalization.CultureInfo?displayProperty=fullName> for parsing and formatting a value.</span></span> <span data-ttu-id="7b6c8-123">使用 `date` 和 `number` 欄位類型時，不建議指定文化特性。</span><span class="sxs-lookup"><span data-stu-id="7b6c8-123">Specifying a culture isn't recommended when using the `date` and `number` field types.</span></span> <span data-ttu-id="7b6c8-124">`date` 和 `number` 具有內建的 Blazor 支援，可提供所需的文化特性。</span><span class="sxs-lookup"><span data-stu-id="7b6c8-124">`date` and `number` have built-in Blazor support that provides the required culture.</span></span>

## <a name="localization"></a><span data-ttu-id="7b6c8-125">當地語系化</span><span class="sxs-lookup"><span data-stu-id="7b6c8-125">Localization</span></span>

Blazor<span data-ttu-id="7b6c8-126"> 伺服器應用程式是使用[當地語系化中介軟體](xref:fundamentals/localization#localization-middleware)來當地語系化。</span><span class="sxs-lookup"><span data-stu-id="7b6c8-126"> Server apps are localized using [Localization Middleware](xref:fundamentals/localization#localization-middleware).</span></span> <span data-ttu-id="7b6c8-127">中介軟體會為從應用程式要求資源的使用者選取適當的文化特性。</span><span class="sxs-lookup"><span data-stu-id="7b6c8-127">The middleware selects the appropriate culture for users requesting resources from the app.</span></span>

<span data-ttu-id="7b6c8-128">您可以使用下列其中一種方法來設定文化特性：</span><span class="sxs-lookup"><span data-stu-id="7b6c8-128">The culture can be set using one of the following approaches:</span></span>

* [<span data-ttu-id="7b6c8-129">Cookie</span><span class="sxs-lookup"><span data-stu-id="7b6c8-129">Cookies</span></span>](#cookies)
* [<span data-ttu-id="7b6c8-130">提供 UI 以選擇文化特性</span><span class="sxs-lookup"><span data-stu-id="7b6c8-130">Provide UI to choose the culture</span></span>](#provide-ui-to-choose-the-culture)

<span data-ttu-id="7b6c8-131">如需詳細資訊和範例，請參閱 <xref:fundamentals/localization>。</span><span class="sxs-lookup"><span data-stu-id="7b6c8-131">For more information and examples, see <xref:fundamentals/localization>.</span></span>

### <a name="configure-the-linker-for-internationalization-opno-locblazor-webassembly"></a><span data-ttu-id="7b6c8-132">設定國際化的連結器（Blazor WebAssembly）</span><span class="sxs-lookup"><span data-stu-id="7b6c8-132">Configure the linker for internationalization (Blazor WebAssembly)</span></span>

<span data-ttu-id="7b6c8-133">根據預設，Blazor WebAssembly 應用程式的 Blazor連結器設定會去除國際化資訊，但不包括明確要求的地區設定。</span><span class="sxs-lookup"><span data-stu-id="7b6c8-133">By default, Blazor's linker configuration for Blazor WebAssembly apps strips out internationalization information except for locales explicitly requested.</span></span> <span data-ttu-id="7b6c8-134">如需控制連結器行為的詳細資訊和指引，請參閱 <xref:host-and-deploy/blazor/configure-linker#configure-the-linker-for-internationalization>。</span><span class="sxs-lookup"><span data-stu-id="7b6c8-134">For more information and guidance on controlling the linker's behavior, see <xref:host-and-deploy/blazor/configure-linker#configure-the-linker-for-internationalization>.</span></span>

### <a name="cookies"></a><span data-ttu-id="7b6c8-135">Cookie</span><span class="sxs-lookup"><span data-stu-id="7b6c8-135">Cookies</span></span>

<span data-ttu-id="7b6c8-136">當地語系化文化特性 cookie 可以保存使用者的文化特性。</span><span class="sxs-lookup"><span data-stu-id="7b6c8-136">A localization culture cookie can persist the user's culture.</span></span> <span data-ttu-id="7b6c8-137">Cookie 是由應用程式的主機頁面（*Pages/主機. cshtml .cs*）的 `OnGet` 方法所建立。</span><span class="sxs-lookup"><span data-stu-id="7b6c8-137">The cookie is created by the `OnGet` method of the app's host page (*Pages/Host.cshtml.cs*).</span></span> <span data-ttu-id="7b6c8-138">當地語系化中介軟體會在後續要求中讀取 cookie，以設定使用者的文化特性。</span><span class="sxs-lookup"><span data-stu-id="7b6c8-138">The Localization Middleware reads the cookie on subsequent requests to set the user's culture.</span></span> 

<span data-ttu-id="7b6c8-139">使用 cookie 可確保 WebSocket 連接可以正確地傳播文化特性。</span><span class="sxs-lookup"><span data-stu-id="7b6c8-139">Use of a cookie ensures that the WebSocket connection can correctly propagate the culture.</span></span> <span data-ttu-id="7b6c8-140">如果當地語系化架構是以 URL 路徑或查詢字串為基礎，配置可能無法使用 Websocket，因而無法保存文化特性。</span><span class="sxs-lookup"><span data-stu-id="7b6c8-140">If localization schemes are based on the URL path or query string, the scheme might not be able to work with WebSockets, thus fail to persist the culture.</span></span> <span data-ttu-id="7b6c8-141">因此，建議的方法是使用當地語系化文化特性 cookie。</span><span class="sxs-lookup"><span data-stu-id="7b6c8-141">Therefore, use of a localization culture cookie is the recommended approach.</span></span>

<span data-ttu-id="7b6c8-142">如果文化特性保存在當地語系化 cookie 中，任何技術都可以用來指派文化特性。</span><span class="sxs-lookup"><span data-stu-id="7b6c8-142">Any technique can be used to assign a culture if the culture is persisted in a localization cookie.</span></span> <span data-ttu-id="7b6c8-143">如果應用程式已建立伺服器端 ASP.NET Core 的當地語系化配置，請繼續使用應用程式的現有當地語系化基礎結構，並在應用程式的配置內設定當地語系化文化特性 cookie。</span><span class="sxs-lookup"><span data-stu-id="7b6c8-143">If the app already has an established localization scheme for server-side ASP.NET Core, continue to use the app's existing localization infrastructure and set the localization culture cookie within the app's scheme.</span></span>

<span data-ttu-id="7b6c8-144">下列範例顯示如何在可由當地語系化中介軟體讀取的 cookie 中設定目前的文化特性。</span><span class="sxs-lookup"><span data-stu-id="7b6c8-144">The following example shows how to set the current culture in a cookie that can be read by the Localization Middleware.</span></span> <span data-ttu-id="7b6c8-145">在 Blazor 伺服器應用程式中，建立具有下列內容的*Pages/Host. cshtml. .cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="7b6c8-145">Create a *Pages/Host.cshtml.cs* file with the following contents in the Blazor Server app:</span></span>

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

<span data-ttu-id="7b6c8-146">應用程式會依照下列事件順序來處理當地語系化：</span><span class="sxs-lookup"><span data-stu-id="7b6c8-146">Localization is handled by the app in the following sequence of events:</span></span>

1. <span data-ttu-id="7b6c8-147">瀏覽器會將初始 HTTP 要求傳送至應用程式。</span><span class="sxs-lookup"><span data-stu-id="7b6c8-147">The browser sends an initial HTTP request to the app.</span></span>
1. <span data-ttu-id="7b6c8-148">文化特性是由當地語系化中介軟體所指派。</span><span class="sxs-lookup"><span data-stu-id="7b6c8-148">The culture is assigned by the Localization Middleware.</span></span>
1. <span data-ttu-id="7b6c8-149">*_Host*中的 `OnGet` 方法會在 cookie 中保存文化特性，做為回應的一部分。</span><span class="sxs-lookup"><span data-stu-id="7b6c8-149">The `OnGet` method in *_Host.cshtml.cs* persists the culture in a cookie as part of the response.</span></span>
1. <span data-ttu-id="7b6c8-150">瀏覽器會開啟 WebSocket 連線，以建立互動式 Blazor 伺服器會話。</span><span class="sxs-lookup"><span data-stu-id="7b6c8-150">The browser opens a WebSocket connection to create an interactive Blazor Server session.</span></span>
1. <span data-ttu-id="7b6c8-151">當地語系化中介軟體會讀取 cookie 並指派文化特性。</span><span class="sxs-lookup"><span data-stu-id="7b6c8-151">The Localization Middleware reads the cookie and assigns the culture.</span></span>
1. <span data-ttu-id="7b6c8-152">Blazor 伺服器會話的開頭是正確的文化特性。</span><span class="sxs-lookup"><span data-stu-id="7b6c8-152">The Blazor Server session begins with the correct culture.</span></span>

### <a name="provide-ui-to-choose-the-culture"></a><span data-ttu-id="7b6c8-153">提供 UI 以選擇文化特性</span><span class="sxs-lookup"><span data-stu-id="7b6c8-153">Provide UI to choose the culture</span></span>

<span data-ttu-id="7b6c8-154">為了提供允許使用者選取文化特性的 UI，建議使用以重新*導向為基礎的方法*。</span><span class="sxs-lookup"><span data-stu-id="7b6c8-154">To provide UI to allow a user to select a culture, a *redirect-based approach* is recommended.</span></span> <span data-ttu-id="7b6c8-155">此程式類似于在使用者嘗試存取安全資源時，在 web 應用程式中發生的情況，&mdash;使用者重新導向至登入頁面，然後重新導向至原始資源。</span><span class="sxs-lookup"><span data-stu-id="7b6c8-155">The process is similar to what happens in a web app when a user attempts to access a secure resource&mdash;the user is redirected to a sign-in page and then redirected back to the original resource.</span></span> 

<span data-ttu-id="7b6c8-156">應用程式會透過重新導向至控制器的方式，來保存使用者選取的文化特性。</span><span class="sxs-lookup"><span data-stu-id="7b6c8-156">The app persists the user's selected culture via a redirect to a controller.</span></span> <span data-ttu-id="7b6c8-157">控制器會將使用者選取的文化特性設定為 cookie，並將使用者重新導向回到原始 URI。</span><span class="sxs-lookup"><span data-stu-id="7b6c8-157">The controller sets the user's selected culture into a cookie and redirects the user back to the original URI.</span></span>

<span data-ttu-id="7b6c8-158">在伺服器上建立 HTTP 端點，以在 cookie 中設定使用者選取的文化特性，並執行重新導向回到原始 URI：</span><span class="sxs-lookup"><span data-stu-id="7b6c8-158">Establish an HTTP endpoint on the server to set the user's selected culture in a cookie and perform the redirect back to the original URI:</span></span>

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
> <span data-ttu-id="7b6c8-159">使用 `LocalRedirect` 動作結果來防止開啟重新導向攻擊。</span><span class="sxs-lookup"><span data-stu-id="7b6c8-159">Use the `LocalRedirect` action result to prevent open redirect attacks.</span></span> <span data-ttu-id="7b6c8-160">如需詳細資訊，請參閱 <xref:security/preventing-open-redirects>。</span><span class="sxs-lookup"><span data-stu-id="7b6c8-160">For more information, see <xref:security/preventing-open-redirects>.</span></span>

<span data-ttu-id="7b6c8-161">下列元件顯示當使用者選取文化特性時，如何執行初始重新導向的範例：</span><span class="sxs-lookup"><span data-stu-id="7b6c8-161">The following component shows an example of how to perform the initial redirection when the user selects a culture:</span></span>

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
        var uri = new Uri(NavigationManager.Uri())
            .GetComponents(UriComponents.PathAndQuery, UriFormat.Unescaped);
        var query = $"?culture={Uri.EscapeDataString(culture)}&" +
            $"redirectUri={Uri.EscapeDataString(uri)}";

        NavigationManager.NavigateTo("/Culture/SetCulture" + query, forceLoad: true);
    }
}
```

## <a name="additional-resources"></a><span data-ttu-id="7b6c8-162">其他資源</span><span class="sxs-lookup"><span data-stu-id="7b6c8-162">Additional resources</span></span>

* <xref:fundamentals/localization>
