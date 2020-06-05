---
<span data-ttu-id="0b4f6-101">標題： ' ASP.NET Core Blazor 全球化和當地語系化的作者： guardrex 描述： ' 瞭解如何讓 Razor 使用者以多種文化特性和語言存取元件。 '</span><span class="sxs-lookup"><span data-stu-id="0b4f6-101">title: 'ASP.NET Core Blazor globalization and localization' author: guardrex description: 'Learn how to make Razor components accessible to users in multiple cultures and languages.'</span></span>
<span data-ttu-id="0b4f6-102">monikerRange： ' >= aspnetcore-3.1 ' ms-chap： riande ms. custom： mvc ms. date： 06/04/2020 no-loc：</span><span class="sxs-lookup"><span data-stu-id="0b4f6-102">monikerRange: '>= aspnetcore-3.1' ms.author: riande ms.custom: mvc ms.date: 06/04/2020 no-loc:</span></span>
- <span data-ttu-id="0b4f6-103">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="0b4f6-103">'Blazor'</span></span>
- <span data-ttu-id="0b4f6-104">'Identity'</span><span class="sxs-lookup"><span data-stu-id="0b4f6-104">'Identity'</span></span>
- <span data-ttu-id="0b4f6-105">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="0b4f6-105">'Let's Encrypt'</span></span>
- <span data-ttu-id="0b4f6-106">'Razor'</span><span class="sxs-lookup"><span data-stu-id="0b4f6-106">'Razor'</span></span>
- <span data-ttu-id="0b4f6-107">' SignalR ' uid： blazor/全球化-當地語系化</span><span class="sxs-lookup"><span data-stu-id="0b4f6-107">'SignalR' uid: blazor/globalization-localization</span></span>

---
# <a name="aspnet-core-blazor-globalization-and-localization"></a><span data-ttu-id="0b4f6-108">ASP.NET Core Blazor 全球化和當地語系化</span><span class="sxs-lookup"><span data-stu-id="0b4f6-108">ASP.NET Core Blazor globalization and localization</span></span>

<span data-ttu-id="0b4f6-109">By [Luke Latham](https://github.com/guardrex)和[Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="0b4f6-109">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

Razor<span data-ttu-id="0b4f6-110">元件可以讓使用者以多種文化特性和語言來存取。</span><span class="sxs-lookup"><span data-stu-id="0b4f6-110"> components can be made accessible to users in multiple cultures and languages.</span></span> <span data-ttu-id="0b4f6-111">以下是可用的 .NET 全球化和當地語系化案例：</span><span class="sxs-lookup"><span data-stu-id="0b4f6-111">The following .NET globalization and localization scenarios are available:</span></span>

* <span data-ttu-id="0b4f6-112">.NET 的資源系統</span><span class="sxs-lookup"><span data-stu-id="0b4f6-112">.NET's resources system</span></span>
* <span data-ttu-id="0b4f6-113">特定文化特性的數位和日期格式</span><span class="sxs-lookup"><span data-stu-id="0b4f6-113">Culture-specific number and date formatting</span></span>

<span data-ttu-id="0b4f6-114">目前支援一組有限的 ASP.NET Core 當地語系化案例：</span><span class="sxs-lookup"><span data-stu-id="0b4f6-114">A limited set of ASP.NET Core's localization scenarios are currently supported:</span></span>

* <span data-ttu-id="0b4f6-115"><xref:Microsoft.Extensions.Localization.IStringLocalizer><xref:Microsoft.Extensions.Localization.IStringLocalizer%601>應用程式*支援*和 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="0b4f6-115"><xref:Microsoft.Extensions.Localization.IStringLocalizer> and <xref:Microsoft.Extensions.Localization.IStringLocalizer%601> *are supported* in Blazor apps.</span></span>
* <span data-ttu-id="0b4f6-116"><xref:Microsoft.AspNetCore.Mvc.Localization.IHtmlLocalizer>、 <xref:Microsoft.AspNetCore.Mvc.Localization.IViewLocalizer> 和資料批註當地語系化 ASP.NET CORE MVC 案例中，而且應用程式**不支援** Blazor 。</span><span class="sxs-lookup"><span data-stu-id="0b4f6-116"><xref:Microsoft.AspNetCore.Mvc.Localization.IHtmlLocalizer>, <xref:Microsoft.AspNetCore.Mvc.Localization.IViewLocalizer>, and Data Annotations localization are ASP.NET Core MVC scenarios and **not supported** in Blazor apps.</span></span>

<span data-ttu-id="0b4f6-117">如需詳細資訊，請參閱 <xref:fundamentals/localization> 。</span><span class="sxs-lookup"><span data-stu-id="0b4f6-117">For more information, see <xref:fundamentals/localization>.</span></span>

## <a name="globalization"></a><span data-ttu-id="0b4f6-118">全球化</span><span class="sxs-lookup"><span data-stu-id="0b4f6-118">Globalization</span></span>

Blazor<span data-ttu-id="0b4f6-119">的 [`@bind`](xref:mvc/views/razor#bind) 功能會執行格式，並根據使用者目前的文化特性來剖析要顯示的值。</span><span class="sxs-lookup"><span data-stu-id="0b4f6-119">'s [`@bind`](xref:mvc/views/razor#bind) functionality performs formats and parses values for display based on the user's current culture.</span></span>

<span data-ttu-id="0b4f6-120">您可以從屬性存取目前的文化 <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> 特性。</span><span class="sxs-lookup"><span data-stu-id="0b4f6-120">The current culture can be accessed from the <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> property.</span></span>

<span data-ttu-id="0b4f6-121"><xref:System.Globalization.CultureInfo.InvariantCulture?displayProperty=nameWithType>用於下欄欄位類型（ `<input type="{TYPE}" />` ）：</span><span class="sxs-lookup"><span data-stu-id="0b4f6-121"><xref:System.Globalization.CultureInfo.InvariantCulture?displayProperty=nameWithType> is used for the following field types (`<input type="{TYPE}" />`):</span></span>

* `date`
* `number`

<span data-ttu-id="0b4f6-122">前面的欄位類型：</span><span class="sxs-lookup"><span data-stu-id="0b4f6-122">The preceding field types:</span></span>

* <span data-ttu-id="0b4f6-123">會使用其適當的瀏覽器格式規則來顯示。</span><span class="sxs-lookup"><span data-stu-id="0b4f6-123">Are displayed using their appropriate browser-based formatting rules.</span></span>
* <span data-ttu-id="0b4f6-124">不能包含自由格式的文字。</span><span class="sxs-lookup"><span data-stu-id="0b4f6-124">Can't contain free-form text.</span></span>
* <span data-ttu-id="0b4f6-125">根據瀏覽器的執行方式提供使用者互動特性。</span><span class="sxs-lookup"><span data-stu-id="0b4f6-125">Provide user interaction characteristics based on the browser's implementation.</span></span>

<span data-ttu-id="0b4f6-126">下欄欄位類型具有特定的格式需求，目前不支援， Blazor 因為所有主要瀏覽器都不支援它們：</span><span class="sxs-lookup"><span data-stu-id="0b4f6-126">The following field types have specific formatting requirements and aren't currently supported by Blazor because they aren't supported by all major browsers:</span></span>

* `datetime-local`
* `month`
* `week`

<span data-ttu-id="0b4f6-127">[`@bind`](xref:mvc/views/razor#bind)支援 `@bind:culture` 參數，以提供 <xref:System.Globalization.CultureInfo?displayProperty=fullName> 用於剖析和格式化值的。</span><span class="sxs-lookup"><span data-stu-id="0b4f6-127">[`@bind`](xref:mvc/views/razor#bind) supports the `@bind:culture` parameter to provide a <xref:System.Globalization.CultureInfo?displayProperty=fullName> for parsing and formatting a value.</span></span> <span data-ttu-id="0b4f6-128">使用 `date` 和欄位類型時，不建議指定文化特性 `number` 。</span><span class="sxs-lookup"><span data-stu-id="0b4f6-128">Specifying a culture isn't recommended when using the `date` and `number` field types.</span></span> <span data-ttu-id="0b4f6-129">`date`和 `number` 具有提供必要文化特性的內建 Blazor 支援。</span><span class="sxs-lookup"><span data-stu-id="0b4f6-129">`date` and `number` have built-in Blazor support that provides the required culture.</span></span>

## <a name="localization"></a><span data-ttu-id="0b4f6-130">Localization</span><span class="sxs-lookup"><span data-stu-id="0b4f6-130">Localization</span></span>

### <a name="blazor-webassembly"></a>Blazor<span data-ttu-id="0b4f6-131">WebAssembly</span><span class="sxs-lookup"><span data-stu-id="0b4f6-131"> WebAssembly</span></span>

Blazor<span data-ttu-id="0b4f6-132">WebAssembly apps 會使用使用者的[語言喜好](https://developer.mozilla.org/docs/Web/API/NavigatorLanguage/languages)設定來設定文化特性。</span><span class="sxs-lookup"><span data-stu-id="0b4f6-132"> WebAssembly apps set the culture using the user's [language preference](https://developer.mozilla.org/docs/Web/API/NavigatorLanguage/languages).</span></span>

<span data-ttu-id="0b4f6-133">若要明確設定文化特性， <xref:System.Globalization.CultureInfo.DefaultThreadCurrentCulture?displayProperty=nameWithType> 請 <xref:System.Globalization.CultureInfo.DefaultThreadCurrentUICulture?displayProperty=nameWithType> 在中設定和 `Program.Main` 。</span><span class="sxs-lookup"><span data-stu-id="0b4f6-133">To explicitly configure the culture, set <xref:System.Globalization.CultureInfo.DefaultThreadCurrentCulture?displayProperty=nameWithType> and <xref:System.Globalization.CultureInfo.DefaultThreadCurrentUICulture?displayProperty=nameWithType> in `Program.Main`.</span></span>

<span data-ttu-id="0b4f6-134">根據預設， Blazor Blazor WebAssembly 應用程式的連結器設定會去除國際化資訊，但不包括明確要求的地區設定。</span><span class="sxs-lookup"><span data-stu-id="0b4f6-134">By default, Blazor's linker configuration for Blazor WebAssembly apps strips out internationalization information except for locales explicitly requested.</span></span> <span data-ttu-id="0b4f6-135">如需控制連結器行為的詳細資訊和指引，請參閱 <xref:host-and-deploy/blazor/configure-linker#configure-the-linker-for-internationalization> 。</span><span class="sxs-lookup"><span data-stu-id="0b4f6-135">For more information and guidance on controlling the linker's behavior, see <xref:host-and-deploy/blazor/configure-linker#configure-the-linker-for-internationalization>.</span></span>

<span data-ttu-id="0b4f6-136">雖然預設選取的文化特性 Blazor 可能足以滿足大部分使用者的需求，但請考慮提供一種方法讓使用者指定其慣用的地區設定。</span><span class="sxs-lookup"><span data-stu-id="0b4f6-136">While the culture that Blazor selects by default might be sufficient for most users, consider offering a way for users to specify their preferred locale.</span></span> <span data-ttu-id="0b4f6-137">如需 Blazor 具有文化特性選擇器的 WebAssembly 範例應用程式，請參閱[LocSample](https://github.com/pranavkm/LocSample)當地語系化範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="0b4f6-137">For a Blazor WebAssembly sample app with a culture picker, see the [LocSample](https://github.com/pranavkm/LocSample) localization sample app.</span></span>

### <a name="blazor-server"></a>Blazor<span data-ttu-id="0b4f6-138">伺服器</span><span class="sxs-lookup"><span data-stu-id="0b4f6-138"> Server</span></span>

Blazor<span data-ttu-id="0b4f6-139">伺服器應用程式是使用[當地語系化中介軟體](xref:fundamentals/localization#localization-middleware)來當地語系化。</span><span class="sxs-lookup"><span data-stu-id="0b4f6-139"> Server apps are localized using [Localization Middleware](xref:fundamentals/localization#localization-middleware).</span></span> <span data-ttu-id="0b4f6-140">中介軟體會為從應用程式要求資源的使用者選取適當的文化特性。</span><span class="sxs-lookup"><span data-stu-id="0b4f6-140">The middleware selects the appropriate culture for users requesting resources from the app.</span></span>

<span data-ttu-id="0b4f6-141">您可以使用下列其中一種方法來設定文化特性：</span><span class="sxs-lookup"><span data-stu-id="0b4f6-141">The culture can be set using one of the following approaches:</span></span>

* [<span data-ttu-id="0b4f6-142">Cookie</span><span class="sxs-lookup"><span data-stu-id="0b4f6-142">Cookies</span></span>](#cookies)
* [<span data-ttu-id="0b4f6-143">提供 UI 以選擇文化特性</span><span class="sxs-lookup"><span data-stu-id="0b4f6-143">Provide UI to choose the culture</span></span>](#provide-ui-to-choose-the-culture)

<span data-ttu-id="0b4f6-144">如需詳細資訊和範例，請參閱 <xref:fundamentals/localization>。</span><span class="sxs-lookup"><span data-stu-id="0b4f6-144">For more information and examples, see <xref:fundamentals/localization>.</span></span>

#### <a name="cookies"></a><span data-ttu-id="0b4f6-145">Cookie</span><span class="sxs-lookup"><span data-stu-id="0b4f6-145">Cookies</span></span>

<span data-ttu-id="0b4f6-146">當地語系化文化特性 cookie 可以保存使用者的文化特性。</span><span class="sxs-lookup"><span data-stu-id="0b4f6-146">A localization culture cookie can persist the user's culture.</span></span> <span data-ttu-id="0b4f6-147">當地語系化中介軟體會在後續要求中讀取 cookie，以設定使用者的文化特性。</span><span class="sxs-lookup"><span data-stu-id="0b4f6-147">The Localization Middleware reads the cookie on subsequent requests to set the user's culture.</span></span> 

<span data-ttu-id="0b4f6-148">使用 cookie 可確保 WebSocket 連接可以正確地傳播文化特性。</span><span class="sxs-lookup"><span data-stu-id="0b4f6-148">Use of a cookie ensures that the WebSocket connection can correctly propagate the culture.</span></span> <span data-ttu-id="0b4f6-149">如果當地語系化架構是以 URL 路徑或查詢字串為基礎，配置可能無法使用 Websocket，因而無法保存文化特性。</span><span class="sxs-lookup"><span data-stu-id="0b4f6-149">If localization schemes are based on the URL path or query string, the scheme might not be able to work with WebSockets, thus fail to persist the culture.</span></span> <span data-ttu-id="0b4f6-150">因此，建議的方法是使用當地語系化文化特性 cookie。</span><span class="sxs-lookup"><span data-stu-id="0b4f6-150">Therefore, use of a localization culture cookie is the recommended approach.</span></span>

<span data-ttu-id="0b4f6-151">如果文化特性保存在當地語系化 cookie 中，任何技術都可以用來指派文化特性。</span><span class="sxs-lookup"><span data-stu-id="0b4f6-151">Any technique can be used to assign a culture if the culture is persisted in a localization cookie.</span></span> <span data-ttu-id="0b4f6-152">如果應用程式已建立伺服器端 ASP.NET Core 的當地語系化配置，請繼續使用應用程式的現有當地語系化基礎結構，並在應用程式的配置內設定當地語系化文化特性 cookie。</span><span class="sxs-lookup"><span data-stu-id="0b4f6-152">If the app already has an established localization scheme for server-side ASP.NET Core, continue to use the app's existing localization infrastructure and set the localization culture cookie within the app's scheme.</span></span>

<span data-ttu-id="0b4f6-153">下列範例顯示如何在可由當地語系化中介軟體讀取的 cookie 中設定目前的文化特性。</span><span class="sxs-lookup"><span data-stu-id="0b4f6-153">The following example shows how to set the current culture in a cookie that can be read by the Localization Middleware.</span></span> <span data-ttu-id="0b4f6-154">Razor在*分頁/_Host*中，于開頭標記的正後方建立運算式 `<body>` ：</span><span class="sxs-lookup"><span data-stu-id="0b4f6-154">Create a Razor expression in the *Pages/_Host.cshtml* file immediately inside the opening `<body>` tag:</span></span>

```cshtml
@using System.Globalization
@using Microsoft.AspNetCore.Localization

...

<body>
    @{
        this.HttpContext.Response.Cookies.Append(
            CookieRequestCultureProvider.DefaultCookieName,
            CookieRequestCultureProvider.MakeCookieValue(
                new RequestCulture(
                    CultureInfo.CurrentCulture,
                    CultureInfo.CurrentUICulture)));
    }

    ...
</body>
```

<span data-ttu-id="0b4f6-155">應用程式會依照下列事件順序來處理當地語系化：</span><span class="sxs-lookup"><span data-stu-id="0b4f6-155">Localization is handled by the app in the following sequence of events:</span></span>

1. <span data-ttu-id="0b4f6-156">瀏覽器會將初始 HTTP 要求傳送至應用程式。</span><span class="sxs-lookup"><span data-stu-id="0b4f6-156">The browser sends an initial HTTP request to the app.</span></span>
1. <span data-ttu-id="0b4f6-157">文化特性是由當地語系化中介軟體所指派。</span><span class="sxs-lookup"><span data-stu-id="0b4f6-157">The culture is assigned by the Localization Middleware.</span></span>
1. <span data-ttu-id="0b4f6-158">Razor `_Host` 頁面（*_Host. cshtml*）中的運算式會在 cookie 中保存文化特性，做為回應的一部分。</span><span class="sxs-lookup"><span data-stu-id="0b4f6-158">The Razor expression in the `_Host` page (*_Host.cshtml*) persists the culture in a cookie as part of the response.</span></span>
1. <span data-ttu-id="0b4f6-159">瀏覽器會開啟 WebSocket 連接，以建立互動式 Blazor 伺服器會話。</span><span class="sxs-lookup"><span data-stu-id="0b4f6-159">The browser opens a WebSocket connection to create an interactive Blazor Server session.</span></span>
1. <span data-ttu-id="0b4f6-160">當地語系化中介軟體會讀取 cookie 並指派文化特性。</span><span class="sxs-lookup"><span data-stu-id="0b4f6-160">The Localization Middleware reads the cookie and assigns the culture.</span></span>
1. <span data-ttu-id="0b4f6-161">Blazor伺服器會話的開頭是正確的文化特性。</span><span class="sxs-lookup"><span data-stu-id="0b4f6-161">The Blazor Server session begins with the correct culture.</span></span>

#### <a name="provide-ui-to-choose-the-culture"></a><span data-ttu-id="0b4f6-162">提供 UI 以選擇文化特性</span><span class="sxs-lookup"><span data-stu-id="0b4f6-162">Provide UI to choose the culture</span></span>

<span data-ttu-id="0b4f6-163">為了提供允許使用者選取文化特性的 UI，建議使用以重新*導向為基礎的方法*。</span><span class="sxs-lookup"><span data-stu-id="0b4f6-163">To provide UI to allow a user to select a culture, a *redirect-based approach* is recommended.</span></span> <span data-ttu-id="0b4f6-164">此程式類似于使用者嘗試存取安全資源時，在 web 應用程式中發生的情況。</span><span class="sxs-lookup"><span data-stu-id="0b4f6-164">The process is similar to what happens in a web app when a user attempts to access a secure resource.</span></span> <span data-ttu-id="0b4f6-165">使用者重新導向至登入頁面，然後重新導向回原始資源。</span><span class="sxs-lookup"><span data-stu-id="0b4f6-165">The user is redirected to a sign-in page and then redirected back to the original resource.</span></span> 

<span data-ttu-id="0b4f6-166">應用程式會透過重新導向至控制器的方式，來保存使用者選取的文化特性。</span><span class="sxs-lookup"><span data-stu-id="0b4f6-166">The app persists the user's selected culture via a redirect to a controller.</span></span> <span data-ttu-id="0b4f6-167">控制器會將使用者選取的文化特性設定為 cookie，並將使用者重新導向回到原始 URI。</span><span class="sxs-lookup"><span data-stu-id="0b4f6-167">The controller sets the user's selected culture into a cookie and redirects the user back to the original URI.</span></span>

<span data-ttu-id="0b4f6-168">在伺服器上建立 HTTP 端點，以在 cookie 中設定使用者選取的文化特性，並執行重新導向回到原始 URI：</span><span class="sxs-lookup"><span data-stu-id="0b4f6-168">Establish an HTTP endpoint on the server to set the user's selected culture in a cookie and perform the redirect back to the original URI:</span></span>

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
> <span data-ttu-id="0b4f6-169">使用 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.LocalRedirect%2A> 動作結果來防止開啟重新導向攻擊。</span><span class="sxs-lookup"><span data-stu-id="0b4f6-169">Use the <xref:Microsoft.AspNetCore.Mvc.ControllerBase.LocalRedirect%2A> action result to prevent open redirect attacks.</span></span> <span data-ttu-id="0b4f6-170">如需詳細資訊，請參閱 <xref:security/preventing-open-redirects> 。</span><span class="sxs-lookup"><span data-stu-id="0b4f6-170">For more information, see <xref:security/preventing-open-redirects>.</span></span>

<span data-ttu-id="0b4f6-171">如果應用程式未設定為處理控制器動作：</span><span class="sxs-lookup"><span data-stu-id="0b4f6-171">If the app isn't configured to process controller actions:</span></span>

* <span data-ttu-id="0b4f6-172">將 MVC 服務新增至中的服務集合 `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="0b4f6-172">Add MVC services to the service collection in `Startup.ConfigureServices`:</span></span>

  ```csharp
  services.AddControllers();
  ```

* <span data-ttu-id="0b4f6-173">在中新增控制器端點路由 `Startup.Configure` ：</span><span class="sxs-lookup"><span data-stu-id="0b4f6-173">Add controller endpoint routing in `Startup.Configure`:</span></span>

  ```csharp
  app.UseEndpoints(endpoints =>
  {
      endpoints.MapControllers();
      endpoints.MapBlazorHub();
      endpoints.MapFallbackToPage("/_Host");
  });
  ```

<span data-ttu-id="0b4f6-174">下列元件顯示當使用者選取文化特性時，如何執行初始重新導向的範例：</span><span class="sxs-lookup"><span data-stu-id="0b4f6-174">The following component shows an example of how to perform the initial redirection when the user selects a culture:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="0b4f6-175">其他資源</span><span class="sxs-lookup"><span data-stu-id="0b4f6-175">Additional resources</span></span>

* <xref:fundamentals/localization>
