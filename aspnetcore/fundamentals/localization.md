---
<span data-ttu-id="2c773-101">標題： author： description： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2c773-101">title: author: description: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="2c773-102">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2c773-102">'Blazor'</span></span>
- <span data-ttu-id="2c773-103">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2c773-103">'Identity'</span></span>
- <span data-ttu-id="2c773-104">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2c773-104">'Let's Encrypt'</span></span>
- <span data-ttu-id="2c773-105">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2c773-105">'Razor'</span></span>
- <span data-ttu-id="2c773-106">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2c773-106">'SignalR' uid:</span></span> 

---
# <a name="globalization-and-localization-in-aspnet-core"></a><span data-ttu-id="2c773-107">ASP.NET Core 全球化和當地語系化</span><span class="sxs-lookup"><span data-stu-id="2c773-107">Globalization and localization in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0 < aspnetcore-5.0"

<span data-ttu-id="2c773-108">由 [Rick Anderson](https://twitter.com/RickAndMSFT)、[Damien Bowden](https://twitter.com/damien_bod)、[Bart Calixto](https://twitter.com/bartmax)、[Nadeem Afana](https://afana.me/) 和 [Hisham Bin Ateya](https://twitter.com/hishambinateya) 提供</span><span class="sxs-lookup"><span data-stu-id="2c773-108">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Damien Bowden](https://twitter.com/damien_bod), [Bart Calixto](https://twitter.com/bartmax), [Nadeem Afana](https://afana.me/), and [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span></span>

<span data-ttu-id="2c773-109">多語系網站可讓網站觸及更多物件。</span><span class="sxs-lookup"><span data-stu-id="2c773-109">A multilingual website allows the site to reach a wider audience.</span></span> <span data-ttu-id="2c773-110">ASP.NET Core 提供服務與中介軟體，可將網站當地語系化成不同的語言與文化特性。</span><span class="sxs-lookup"><span data-stu-id="2c773-110">ASP.NET Core provides services and middleware for localizing into different languages and cultures.</span></span>

<span data-ttu-id="2c773-111">國際化包含[全球化](/dotnet/api/system.globalization)和[當地語系化](/dotnet/standard/globalization-localization/localization)這兩部分。</span><span class="sxs-lookup"><span data-stu-id="2c773-111">Internationalization involves [Globalization](/dotnet/api/system.globalization) and [Localization](/dotnet/standard/globalization-localization/localization).</span></span> <span data-ttu-id="2c773-112">全球化是指設計出可支援不同文化特性之應用程式的程序。</span><span class="sxs-lookup"><span data-stu-id="2c773-112">Globalization is the process of designing apps that support different cultures.</span></span> <span data-ttu-id="2c773-113">透過全球化，您可新增支援與特定地區相關之已定義語言指令碼的輸入、顯示及輸出作業。</span><span class="sxs-lookup"><span data-stu-id="2c773-113">Globalization adds support for input, display, and output of a defined set of language scripts that relate to specific geographic areas.</span></span>

<span data-ttu-id="2c773-114">當地語系化是指針對全球化應用程式進行調整的程序，且您已順應特定文化特性/地區設定對這些全球化應用程式進行可當地語系化處理。</span><span class="sxs-lookup"><span data-stu-id="2c773-114">Localization is the process of adapting a globalized app, which you have already processed for localizability, to a particular culture/locale.</span></span> <span data-ttu-id="2c773-115">如需詳細資訊，請參閱本文件結尾處的**全球化和當地語系化詞彙**。</span><span class="sxs-lookup"><span data-stu-id="2c773-115">For more information see **Globalization and localization terms** near the end of this document.</span></span>

<span data-ttu-id="2c773-116">應用程式當地語系化包含下列作業：</span><span class="sxs-lookup"><span data-stu-id="2c773-116">App localization involves the following:</span></span>

1. <span data-ttu-id="2c773-117">讓應用程式的內容可當地語系化</span><span class="sxs-lookup"><span data-stu-id="2c773-117">Make the app's content localizable</span></span>
1. <span data-ttu-id="2c773-118">針對您支援的語言和文化特性提供當地語系化資源</span><span class="sxs-lookup"><span data-stu-id="2c773-118">Provide localized resources for the languages and cultures you support</span></span>
1. <span data-ttu-id="2c773-119">實作可依據每項要求選取語言/文化特性的策略</span><span class="sxs-lookup"><span data-stu-id="2c773-119">Implement a strategy to select the language/culture for each request</span></span>

<span data-ttu-id="2c773-120">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/localization/sample/Localization)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="2c773-120">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/localization/sample/Localization) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="make-the-apps-content-localizable"></a><span data-ttu-id="2c773-121">讓應用程式的內容可當地語系化</span><span class="sxs-lookup"><span data-stu-id="2c773-121">Make the app's content localizable</span></span>

<span data-ttu-id="2c773-122"><xref:Microsoft.Extensions.Localization.IStringLocalizer>` and <xref:Microsoft.Extensions.Localization.IStringLocalizer%601> were architected to improve productivity when developing localized apps. `IStringLocalizer ` uses the [ResourceManager](/dotnet/api/system.resources.resourcemanager) and [ResourceReader](/dotnet/api/system.resources.resourcereader) to provide culture-specific resources at run time. The interface has an indexer and an ` IEnumerable ` for returning localized strings. ` IStringLocalizer ' 不需要將預設語言字串儲存在資源檔中。</span><span class="sxs-lookup"><span data-stu-id="2c773-122"><xref:Microsoft.Extensions.Localization.IStringLocalizer>` and <xref:Microsoft.Extensions.Localization.IStringLocalizer%601> were architected to improve productivity when developing localized apps. `IStringLocalizer` uses the [ResourceManager](/dotnet/api/system.resources.resourcemanager) and [ResourceReader](/dotnet/api/system.resources.resourcereader) to provide culture-specific resources at run time. The interface has an indexer and an `IEnumerable` for returning localized strings. `IStringLocalizer\` doesn't require storing the default language strings in a resource file.</span></span> <span data-ttu-id="2c773-123">您不必在開發初期建立資源檔，即可開發以當地語系化為目標的應用程式。</span><span class="sxs-lookup"><span data-stu-id="2c773-123">You can develop an app targeted for localization and not need to create resource files early in development.</span></span> <span data-ttu-id="2c773-124">下列程式碼會示範如何包裝 "About Title" 字串以進行當地語系化。</span><span class="sxs-lookup"><span data-stu-id="2c773-124">The code below shows how to wrap the string "About Title" for localization.</span></span>

[!code-csharp[](localization/sample/Localization/Controllers/AboutController.cs)]

<span data-ttu-id="2c773-125">在上述程式碼中， `IStringLocalizer<T>` 執行是來自相依性[插入](dependency-injection.md)。</span><span class="sxs-lookup"><span data-stu-id="2c773-125">In the preceding code, the `IStringLocalizer<T>` implementation comes from [Dependency Injection](dependency-injection.md).</span></span> <span data-ttu-id="2c773-126">如果找不到 "About Title" 的當地語系化值，即會傳回索引子的索引鍵，也就是 "About Title" 字串。</span><span class="sxs-lookup"><span data-stu-id="2c773-126">If the localized value of "About Title" isn't found, then the indexer key is returned, that is, the string "About Title".</span></span> <span data-ttu-id="2c773-127">您可以保留應用程式中的預設語言常值字串，並將其包裝在當地語系化工具中，以便專注於開發應用程式。</span><span class="sxs-lookup"><span data-stu-id="2c773-127">You can leave the default language literal strings in the app and wrap them in the localizer, so that you can focus on developing the app.</span></span> <span data-ttu-id="2c773-128">您不用先建立預設資源檔，即可使用預設語言來開發應用程式，並針對當地語系化步驟進行應用程式的準備。</span><span class="sxs-lookup"><span data-stu-id="2c773-128">You develop your app with your default language and prepare it for the localization step without first creating a default resource file.</span></span> <span data-ttu-id="2c773-129">或者，您可以使用傳統方法，並提供索引鍵以擷取預設語言字串。</span><span class="sxs-lookup"><span data-stu-id="2c773-129">Alternatively, you can use the traditional approach and provide a key to retrieve the default language string.</span></span> <span data-ttu-id="2c773-130">對許多開發人員來說，新的工作流程 (單純包裝字串常值而不使用預設語言的 *.resx* 檔案) 可以降低當地語系化應用程式的額外負荷。</span><span class="sxs-lookup"><span data-stu-id="2c773-130">For many developers the new workflow of not having a default language *.resx* file and simply wrapping the string literals can reduce the overhead of localizing an app.</span></span> <span data-ttu-id="2c773-131">其他開發人員則偏好傳統的工作流程，因為這種方法更便於使用較長的字串常值，且更易於更新當地語系化的字串。</span><span class="sxs-lookup"><span data-stu-id="2c773-131">Other developers will prefer the traditional work flow as it can make it easier to work with longer string literals and make it easier to update localized strings.</span></span>

<span data-ttu-id="2c773-132">若是包含 HTML 的資源，請使用 `IHtmlLocalizer<T>` 實作。</span><span class="sxs-lookup"><span data-stu-id="2c773-132">Use the `IHtmlLocalizer<T>` implementation for resources that contain HTML.</span></span> <span data-ttu-id="2c773-133">`IHtmlLocalizer` 會對資源字串中經過格式化的引數進行 HTML 編碼，但不會對資源字串本身進行 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="2c773-133">`IHtmlLocalizer` HTML encodes arguments that are formatted in the resource string, but doesn't HTML encode the resource string itself.</span></span> <span data-ttu-id="2c773-134">在下列醒目提示的範例中，只有 `name` 參數的值經過 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="2c773-134">In the sample highlighted below, only the value of `name` parameter is HTML encoded.</span></span>

[!code-csharp[](../fundamentals/localization/sample/Localization/Controllers/BookController.cs?highlight=3,5,20&start=1&end=24)]

<span data-ttu-id="2c773-135">**注意：** 一般來說，您只想將文字當地語系化，而不是 HTML。</span><span class="sxs-lookup"><span data-stu-id="2c773-135">**Note:** You generally want to only localize text and not HTML.</span></span>

<span data-ttu-id="2c773-136">您可以在最底層的[相依性插入](dependency-injection.md)中，將 `IStringLocalizerFactory` 移出：</span><span class="sxs-lookup"><span data-stu-id="2c773-136">At the lowest level, you can get `IStringLocalizerFactory` out of [Dependency Injection](dependency-injection.md):</span></span>

[!code-csharp[](localization/sample/Localization/Controllers/TestController.cs?start=9&end=26&highlight=7-13)]

<span data-ttu-id="2c773-137">上述程式碼會示範這兩個 Factory Create 方法。</span><span class="sxs-lookup"><span data-stu-id="2c773-137">The code above demonstrates each of the two factory create methods.</span></span>

<span data-ttu-id="2c773-138">您可以依據控制器或區域來分割當地語系化的字串，也可以只用一個容器。</span><span class="sxs-lookup"><span data-stu-id="2c773-138">You can partition your localized strings by controller, area, or have just one container.</span></span> <span data-ttu-id="2c773-139">在範例應用程式中，會針對共用資源使用名為 `SharedResource` 的虛擬類別。</span><span class="sxs-lookup"><span data-stu-id="2c773-139">In the sample app, a dummy class named `SharedResource` is used for shared resources.</span></span>

[!code-csharp[](localization/sample/Localization/Resources/SharedResource.cs)]

<span data-ttu-id="2c773-140">有些開發人員會使用 `Startup` 類別來包含全域或共用字串。</span><span class="sxs-lookup"><span data-stu-id="2c773-140">Some developers use the `Startup` class to contain global or shared strings.</span></span> <span data-ttu-id="2c773-141">下列範例會使用 `InfoController` 和 `SharedResource` 當地語系化工具：</span><span class="sxs-lookup"><span data-stu-id="2c773-141">In the sample below, the `InfoController` and the `SharedResource` localizers are used:</span></span>

[!code-csharp[](localization/sample/Localization/Controllers/InfoController.cs?range=9-26)]

## <a name="view-localization"></a><span data-ttu-id="2c773-142">檢視當地語系化</span><span class="sxs-lookup"><span data-stu-id="2c773-142">View localization</span></span>

<span data-ttu-id="2c773-143">`IViewLocalizer` 服務可提供[檢視](xref:mvc/views/overview)的當地語系化字串。</span><span class="sxs-lookup"><span data-stu-id="2c773-143">The `IViewLocalizer` service provides localized strings for a [view](xref:mvc/views/overview).</span></span> <span data-ttu-id="2c773-144">`ViewLocalizer` 類別會實作這個介面，並透過檢視檔案路徑來找出資源的位置。</span><span class="sxs-lookup"><span data-stu-id="2c773-144">The `ViewLocalizer` class implements this interface and finds the resource location from the view file path.</span></span> <span data-ttu-id="2c773-145">下列程式碼示範如何使用 `IViewLocalizer` 的預設實作：</span><span class="sxs-lookup"><span data-stu-id="2c773-145">The following code shows how to use the default implementation of `IViewLocalizer`:</span></span>

[!code-cshtml[](localization/sample/Localization/Views/Home/About.cshtml)]

<span data-ttu-id="2c773-146">`IViewLocalizer` 的預設實作會依據檢視的檔案名稱來找出資源檔。</span><span class="sxs-lookup"><span data-stu-id="2c773-146">The default implementation of `IViewLocalizer` finds the resource file based on the view's file name.</span></span> <span data-ttu-id="2c773-147">其中並沒有任何選項可以使用全域共用的資源檔。</span><span class="sxs-lookup"><span data-stu-id="2c773-147">There's no option to use a global shared resource file.</span></span> <span data-ttu-id="2c773-148">`ViewLocalizer`使用來執行當地語系化工具 `IHtmlLocalizer` ，因此 Razor 不會對當地語系化字串進行 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="2c773-148">`ViewLocalizer` implements the localizer using `IHtmlLocalizer`, so Razor doesn't HTML encode the localized string.</span></span> <span data-ttu-id="2c773-149">您可以參數化資源字串，`IViewLocalizer` 即會對參數 (而不是資源字串) 進行 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="2c773-149">You can parameterize resource strings and `IViewLocalizer` will HTML encode the parameters, but not the resource string.</span></span> <span data-ttu-id="2c773-150">請考慮下列 Razor 標記：</span><span class="sxs-lookup"><span data-stu-id="2c773-150">Consider the following Razor markup:</span></span>

```cshtml
@Localizer["<i>Hello</i> <b>{0}!</b>", UserManager.GetUserName(User)]
```

<span data-ttu-id="2c773-151">法文資源檔可能包含下列內容：</span><span class="sxs-lookup"><span data-stu-id="2c773-151">A French resource file could contain the following:</span></span>

| <span data-ttu-id="2c773-152">Key</span><span class="sxs-lookup"><span data-stu-id="2c773-152">Key</span></span> | <span data-ttu-id="2c773-153">值</span><span class="sxs-lookup"><span data-stu-id="2c773-153">Value</span></span> |
| ----- | ---
<span data-ttu-id="2c773-154">標題： author： description： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2c773-154">title: author: description: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="2c773-155">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2c773-155">'Blazor'</span></span>
- <span data-ttu-id="2c773-156">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2c773-156">'Identity'</span></span>
- <span data-ttu-id="2c773-157">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2c773-157">'Let's Encrypt'</span></span>
- <span data-ttu-id="2c773-158">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2c773-158">'Razor'</span></span>
- <span data-ttu-id="2c773-159">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2c773-159">'SignalR' uid:</span></span> 

<span data-ttu-id="2c773-160">--- | | `<i>Hello</i> <b>{0}!</b>` | `<i>Bonjour</i> <b>{0} !</b>` |</span><span class="sxs-lookup"><span data-stu-id="2c773-160">--- | | `<i>Hello</i> <b>{0}!</b>` | `<i>Bonjour</i> <b>{0} !</b>` |</span></span>

<span data-ttu-id="2c773-161">轉譯的檢視內容可能包含來自資源檔的 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="2c773-161">The rendered view would contain the HTML markup from the resource file.</span></span>

<span data-ttu-id="2c773-162">**注意：** 一般來說，您只想將文字當地語系化，而不是 HTML。</span><span class="sxs-lookup"><span data-stu-id="2c773-162">**Note:** You generally want to only localize text and not HTML.</span></span>

<span data-ttu-id="2c773-163">若要在檢視中使用共用的資源檔，請插入 `IHtmlLocalizer<T>`：</span><span class="sxs-lookup"><span data-stu-id="2c773-163">To use a shared resource file in a view, inject `IHtmlLocalizer<T>`:</span></span>

[!code-cshtml[](../fundamentals/localization/sample/Localization/Views/Test/About.cshtml?highlight=5,12)]

## <a name="dataannotations-localization"></a><span data-ttu-id="2c773-164">DataAnnotations 當地語系化</span><span class="sxs-lookup"><span data-stu-id="2c773-164">DataAnnotations localization</span></span>

<span data-ttu-id="2c773-165">DataAnnotations 錯誤訊息會使用 `IStringLocalizer<T>` 來當地語系化。</span><span class="sxs-lookup"><span data-stu-id="2c773-165">DataAnnotations error messages are localized with `IStringLocalizer<T>`.</span></span> <span data-ttu-id="2c773-166">使用 `ResourcesPath = "Resources"` 選項時，`RegisterViewModel` 中的錯誤訊息會儲存在下列路徑之一：</span><span class="sxs-lookup"><span data-stu-id="2c773-166">Using the option `ResourcesPath = "Resources"`, the error messages in `RegisterViewModel` can be stored in either of the following paths:</span></span>

* <span data-ttu-id="2c773-167">*Resources/Viewmodel. RegisterViewModel. fr .resx*</span><span class="sxs-lookup"><span data-stu-id="2c773-167">*Resources/ViewModels.Account.RegisterViewModel.fr.resx*</span></span>
* <span data-ttu-id="2c773-168">*Resources/ViewModels/Account/RegisterViewModel.fr.resx*</span><span class="sxs-lookup"><span data-stu-id="2c773-168">*Resources/ViewModels/Account/RegisterViewModel.fr.resx*</span></span>

[!code-csharp[](localization/sample/Localization/ViewModels/Account/RegisterViewModel.cs?start=9&end=26)]

<span data-ttu-id="2c773-169">在 ASP.NET Core MVC 1.1.0 和更高版本中，系統會將非驗證屬性當地語系化。</span><span class="sxs-lookup"><span data-stu-id="2c773-169">In ASP.NET Core MVC 1.1.0 and higher, non-validation attributes are localized.</span></span> <span data-ttu-id="2c773-170">ASP.NET Core MVC 1.0 **不會**查閱非驗證屬性的當地語系化字串。</span><span class="sxs-lookup"><span data-stu-id="2c773-170">ASP.NET Core MVC 1.0 does **not** look up localized strings for non-validation attributes.</span></span>

<a name="one-resource-string-multiple-classes"></a>

### <a name="using-one-resource-string-for-multiple-classes"></a><span data-ttu-id="2c773-171">針對多個類別使用同一個資源字串</span><span class="sxs-lookup"><span data-stu-id="2c773-171">Using one resource string for multiple classes</span></span>

<span data-ttu-id="2c773-172">下列程式碼會示範如何針對含有多個類別的驗證屬性使用同一個資源字串：</span><span class="sxs-lookup"><span data-stu-id="2c773-172">The following code shows how to use one resource string for validation attributes with multiple classes:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddDataAnnotationsLocalization(options => {
            options.DataAnnotationLocalizerProvider = (type, factory) =>
                factory.Create(typeof(SharedResource));
        });
}
```

<span data-ttu-id="2c773-173">在先前的程式碼中，`SharedResource` 是與儲存驗證訊息的 resx 對應的類別。</span><span class="sxs-lookup"><span data-stu-id="2c773-173">In the preceding code, `SharedResource` is the class corresponding to the resx where your validation messages are stored.</span></span> <span data-ttu-id="2c773-174">使用這個方法時，DataAnnotations 只會使用 `SharedResource`，而不是每個類別的資源。</span><span class="sxs-lookup"><span data-stu-id="2c773-174">With this approach, DataAnnotations will only use `SharedResource`, rather than the resource for each class.</span></span>

## <a name="provide-localized-resources-for-the-languages-and-cultures-you-support"></a><span data-ttu-id="2c773-175">針對您支援的語言和文化特性提供當地語系化資源</span><span class="sxs-lookup"><span data-stu-id="2c773-175">Provide localized resources for the languages and cultures you support</span></span>

### <a name="supportedcultures-and-supporteduicultures"></a><span data-ttu-id="2c773-176">SupportedCultures 和 SupportedUICultures</span><span class="sxs-lookup"><span data-stu-id="2c773-176">SupportedCultures and SupportedUICultures</span></span>

<span data-ttu-id="2c773-177">ASP.NET Core 可讓您指定 `SupportedCultures` 和 `SupportedUICultures` 這兩個文化特性值。</span><span class="sxs-lookup"><span data-stu-id="2c773-177">ASP.NET Core allows you to specify two culture values, `SupportedCultures` and `SupportedUICultures`.</span></span> <span data-ttu-id="2c773-178">`SupportedCultures` 的 [CultureInfo](/dotnet/api/system.globalization.cultureinfo) 物件可決定文化特性相依函式的結果，例如日期、時間、數字及貨幣格式。</span><span class="sxs-lookup"><span data-stu-id="2c773-178">The [CultureInfo](/dotnet/api/system.globalization.cultureinfo) object for `SupportedCultures` determines the results of culture-dependent functions, such as date, time, number, and currency formatting.</span></span> <span data-ttu-id="2c773-179">`SupportedCultures` 也可決定文字排列順序、大小寫慣例和字串比較。</span><span class="sxs-lookup"><span data-stu-id="2c773-179">`SupportedCultures` also determines the sorting order of text, casing conventions, and string comparisons.</span></span> <span data-ttu-id="2c773-180">如需伺服器如何取得文化特性的詳細資訊，請參閱 [CultureInfo.CurrentCulture](/dotnet/api/system.stringcomparer.currentculture#System_StringComparer_CurrentCulture)。</span><span class="sxs-lookup"><span data-stu-id="2c773-180">See [CultureInfo.CurrentCulture](/dotnet/api/system.stringcomparer.currentculture#System_StringComparer_CurrentCulture) for more info on how the server gets the Culture.</span></span> <span data-ttu-id="2c773-181">`SupportedUICultures`會決定[ResourceManager](/dotnet/api/system.resources.resourcemanager)會查閱哪些翻譯的字串（來自 *.resx*檔案）。</span><span class="sxs-lookup"><span data-stu-id="2c773-181">The `SupportedUICultures` determines which translated strings (from *.resx* files) are looked up by the [ResourceManager](/dotnet/api/system.resources.resourcemanager).</span></span> <span data-ttu-id="2c773-182">`ResourceManager` 僅會查閱 `CurrentUICulture` 所決定之文化特性特有的字串。</span><span class="sxs-lookup"><span data-stu-id="2c773-182">The `ResourceManager` simply looks up culture-specific strings that's determined by `CurrentUICulture`.</span></span> <span data-ttu-id="2c773-183">.NET 中的每個執行緒都有 `CurrentCulture` 和 `CurrentUICulture` 物件。</span><span class="sxs-lookup"><span data-stu-id="2c773-183">Every thread in .NET has `CurrentCulture` and `CurrentUICulture` objects.</span></span> <span data-ttu-id="2c773-184">ASP.NET Core 會在轉譯文化特性相依函式時檢查這些值。</span><span class="sxs-lookup"><span data-stu-id="2c773-184">ASP.NET Core inspects these values when rendering culture-dependent functions.</span></span> <span data-ttu-id="2c773-185">比方說，如果目前執行緒的文化特性設定為 "en-US" (英文 - 美國)，`DateTime.Now.ToLongDateString()` 會顯示 "Thursday, February 18, 2016"，但如果 `CurrentCulture` 設定為 "es-ES" (西班牙文 - 西班牙)，則輸出會是 "jueves, 18 de febrero de 2016"。</span><span class="sxs-lookup"><span data-stu-id="2c773-185">For example, if the current thread's culture is set to "en-US" (English, United States), `DateTime.Now.ToLongDateString()` displays "Thursday, February 18, 2016", but if `CurrentCulture` is set to "es-ES" (Spanish, Spain) the output will be "jueves, 18 de febrero de 2016".</span></span>

## <a name="resource-files"></a><span data-ttu-id="2c773-186">資源檔</span><span class="sxs-lookup"><span data-stu-id="2c773-186">Resource files</span></span>

<span data-ttu-id="2c773-187">資源檔是一種實用的機制，可讓您將可當地語系化的字串與代碼區隔開來。</span><span class="sxs-lookup"><span data-stu-id="2c773-187">A resource file is a useful mechanism for separating localizable strings from code.</span></span> <span data-ttu-id="2c773-188">非預設語言的翻譯字串會在 *.resx*資源檔中隔離。</span><span class="sxs-lookup"><span data-stu-id="2c773-188">Translated strings for the non-default language are isolated in *.resx* resource files.</span></span> <span data-ttu-id="2c773-189">例如，您可以建立名為 *Welcome.es.resx* 的西班牙文資源檔，以包含翻譯的字串。</span><span class="sxs-lookup"><span data-stu-id="2c773-189">For example, you might want to create Spanish resource file named *Welcome.es.resx* containing translated strings.</span></span> <span data-ttu-id="2c773-190">"es" 是西班牙文的語言代碼。</span><span class="sxs-lookup"><span data-stu-id="2c773-190">"es" is the language code for Spanish.</span></span> <span data-ttu-id="2c773-191">若要在 Visual Studio 中建立這個資源檔：</span><span class="sxs-lookup"><span data-stu-id="2c773-191">To create this resource file in Visual Studio:</span></span>

1. <span data-ttu-id="2c773-192">在方案總管\*\*\*\* 中，以滑鼠右鍵按一下要放置資源檔的資料夾 > [新增]**[新增項目]** > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="2c773-192">In **Solution Explorer**, right click on the folder which will contain the resource file > **Add** > **New Item**.</span></span>

    ![巢狀特色選單：方案總管會開啟 [資源] 的特色選單，](localization/_static/newi.png)

2. <span data-ttu-id="2c773-195">在 [Search installed templates] (搜尋已安裝的範本)\*\*\*\* 方塊中，輸入「資源」，並命名檔案。</span><span class="sxs-lookup"><span data-stu-id="2c773-195">In the **Search installed templates** box, enter "resource" and name the file.</span></span>

    ![[新增項目] 對話方塊](localization/_static/res.png)

3. <span data-ttu-id="2c773-197">在 [名稱]\*\*\*\* 資料行中輸入索引鍵值 (原生字串)，並在 [值]\*\*\*\* 資料行中輸入已翻譯的字串。</span><span class="sxs-lookup"><span data-stu-id="2c773-197">Enter the key value (native string) in the **Name** column and the translated string in the **Value** column.</span></span>

    ![Welcome.es.resx 檔案 (西班牙文的「歡迎使用」資源檔)，其中 [名稱] 資料行的文字為 Hello，而 [值] 資料行的文字為 Hola (Hello 的西班牙文)](localization/_static/hola.png)

    <span data-ttu-id="2c773-199">Visual Studio 會顯示 *Welcome.es.resx* 檔案。</span><span class="sxs-lookup"><span data-stu-id="2c773-199">Visual Studio shows the *Welcome.es.resx* file.</span></span>

    ![方案總管，其中顯示「歡迎使用」的西班牙文 (es) 資源檔](localization/_static/se.png)

## <a name="resource-file-naming"></a><span data-ttu-id="2c773-201">資源檔命名</span><span class="sxs-lookup"><span data-stu-id="2c773-201">Resource file naming</span></span>

<span data-ttu-id="2c773-202">資源的命名方式是以其類別的完整類型名稱去掉組件名稱而得。</span><span class="sxs-lookup"><span data-stu-id="2c773-202">Resources are named for the full type name of their class minus the assembly name.</span></span> <span data-ttu-id="2c773-203">例如，假設專案中的法文資源是 `LocalizationWebsite.Web.Startup` 類別、主要組件為 `LocalizationWebsite.Web.dll`，就會命名為 *Startup.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="2c773-203">For example, a French resource in a project whose main assembly is `LocalizationWebsite.Web.dll` for the class `LocalizationWebsite.Web.Startup` would be named *Startup.fr.resx*.</span></span> <span data-ttu-id="2c773-204">若是 `LocalizationWebsite.Web.Controllers.HomeController` 類別的資源，則應命名為 *Controllers.HomeController.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="2c773-204">A resource for the class `LocalizationWebsite.Web.Controllers.HomeController` would be named *Controllers.HomeController.fr.resx*.</span></span> <span data-ttu-id="2c773-205">如果目標類別的命名空間和組件名稱不相同，則需要使用完整類型名稱。</span><span class="sxs-lookup"><span data-stu-id="2c773-205">If your targeted class's namespace isn't the same as the assembly name you will need the full type name.</span></span> <span data-ttu-id="2c773-206">比方說，範例專案中 `ExtraNamespace.Tools` 類型的資源會命名為 *ExtraNamespace.Tools.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="2c773-206">For example, in the sample project a resource for the type `ExtraNamespace.Tools` would be named *ExtraNamespace.Tools.fr.resx*.</span></span>

<span data-ttu-id="2c773-207">在範例專案中，`ConfigureServices` 方法會將 `ResourcesPath` 設為 "Resources"，因此首頁控制器的法文資源檔專案相對路徑即為 *Resources/Controllers.HomeController.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="2c773-207">In the sample project, the `ConfigureServices` method sets the `ResourcesPath` to "Resources", so the project relative path for the home controller's French resource file is *Resources/Controllers.HomeController.fr.resx*.</span></span> <span data-ttu-id="2c773-208">或者，您可以使用資料夾來收集資源檔。</span><span class="sxs-lookup"><span data-stu-id="2c773-208">Alternatively, you can use folders to organize resource files.</span></span> <span data-ttu-id="2c773-209">若是首頁控制器，路徑就是 *Resources/Controllers/HomeController.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="2c773-209">For the home controller, the path would be *Resources/Controllers/HomeController.fr.resx*.</span></span> <span data-ttu-id="2c773-210">如果您不使用 `ResourcesPath` 選項，*.resx* 檔案即會放置在專案的基底目錄中。</span><span class="sxs-lookup"><span data-stu-id="2c773-210">If you don't use the `ResourcesPath` option, the *.resx* file would go in the project base directory.</span></span> <span data-ttu-id="2c773-211">`HomeController` 的資源檔會命名為 *Controllers.HomeController.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="2c773-211">The resource file for `HomeController` would be named *Controllers.HomeController.fr.resx*.</span></span> <span data-ttu-id="2c773-212">您可依據自己的資源檔收集方式，來選擇要使用點或路徑的命名慣例。</span><span class="sxs-lookup"><span data-stu-id="2c773-212">The choice of using the dot or path naming convention depends on how you want to organize your resource files.</span></span>

| <span data-ttu-id="2c773-213">資源名稱</span><span class="sxs-lookup"><span data-stu-id="2c773-213">Resource name</span></span> | <span data-ttu-id="2c773-214">點或路徑命名</span><span class="sxs-lookup"><span data-stu-id="2c773-214">Dot or path naming</span></span> |
| ---
<span data-ttu-id="2c773-215">標題： author： description： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2c773-215">title: author: description: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="2c773-216">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2c773-216">'Blazor'</span></span>
- <span data-ttu-id="2c773-217">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2c773-217">'Identity'</span></span>
- <span data-ttu-id="2c773-218">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2c773-218">'Let's Encrypt'</span></span>
- <span data-ttu-id="2c773-219">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2c773-219">'Razor'</span></span>
- <span data-ttu-id="2c773-220">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2c773-220">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2c773-221">標題： author： description： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2c773-221">title: author: description: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="2c773-222">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2c773-222">'Blazor'</span></span>
- <span data-ttu-id="2c773-223">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2c773-223">'Identity'</span></span>
- <span data-ttu-id="2c773-224">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2c773-224">'Let's Encrypt'</span></span>
- <span data-ttu-id="2c773-225">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2c773-225">'Razor'</span></span>
- <span data-ttu-id="2c773-226">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2c773-226">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2c773-227">標題： author： description： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2c773-227">title: author: description: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="2c773-228">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2c773-228">'Blazor'</span></span>
- <span data-ttu-id="2c773-229">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2c773-229">'Identity'</span></span>
- <span data-ttu-id="2c773-230">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2c773-230">'Let's Encrypt'</span></span>
- <span data-ttu-id="2c773-231">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2c773-231">'Razor'</span></span>
- <span data-ttu-id="2c773-232">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2c773-232">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2c773-233">標題： author： description： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2c773-233">title: author: description: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="2c773-234">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2c773-234">'Blazor'</span></span>
- <span data-ttu-id="2c773-235">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2c773-235">'Identity'</span></span>
- <span data-ttu-id="2c773-236">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2c773-236">'Let's Encrypt'</span></span>
- <span data-ttu-id="2c773-237">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2c773-237">'Razor'</span></span>
- <span data-ttu-id="2c773-238">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2c773-238">'SignalR' uid:</span></span> 

<span data-ttu-id="2c773-239">------   |---標題： author： description： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2c773-239">------   | --- title: author: description: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="2c773-240">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2c773-240">'Blazor'</span></span>
- <span data-ttu-id="2c773-241">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2c773-241">'Identity'</span></span>
- <span data-ttu-id="2c773-242">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2c773-242">'Let's Encrypt'</span></span>
- <span data-ttu-id="2c773-243">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2c773-243">'Razor'</span></span>
- <span data-ttu-id="2c773-244">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2c773-244">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2c773-245">標題： author： description： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2c773-245">title: author: description: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="2c773-246">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2c773-246">'Blazor'</span></span>
- <span data-ttu-id="2c773-247">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2c773-247">'Identity'</span></span>
- <span data-ttu-id="2c773-248">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2c773-248">'Let's Encrypt'</span></span>
- <span data-ttu-id="2c773-249">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2c773-249">'Razor'</span></span>
- <span data-ttu-id="2c773-250">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2c773-250">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2c773-251">標題： author： description： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2c773-251">title: author: description: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="2c773-252">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2c773-252">'Blazor'</span></span>
- <span data-ttu-id="2c773-253">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2c773-253">'Identity'</span></span>
- <span data-ttu-id="2c773-254">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2c773-254">'Let's Encrypt'</span></span>
- <span data-ttu-id="2c773-255">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2c773-255">'Razor'</span></span>
- <span data-ttu-id="2c773-256">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2c773-256">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2c773-257">標題： author： description： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2c773-257">title: author: description: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="2c773-258">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2c773-258">'Blazor'</span></span>
- <span data-ttu-id="2c773-259">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2c773-259">'Identity'</span></span>
- <span data-ttu-id="2c773-260">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2c773-260">'Let's Encrypt'</span></span>
- <span data-ttu-id="2c773-261">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2c773-261">'Razor'</span></span>
- <span data-ttu-id="2c773-262">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2c773-262">'SignalR' uid:</span></span> 

<span data-ttu-id="2c773-263">------- | |Resources/controller. HomeController. .resx |點 | |Resources/controller/HomeController. .resx |路徑 | |   |    |</span><span class="sxs-lookup"><span data-stu-id="2c773-263">------- | | Resources/Controllers.HomeController.fr.resx | Dot  | | Resources/Controllers/HomeController.fr.resx  | Path | |    |     |</span></span>

<span data-ttu-id="2c773-264">在 views 中使用的資源檔會 `@inject IViewLocalizer` Razor 遵循類似的模式。</span><span class="sxs-lookup"><span data-stu-id="2c773-264">Resource files using `@inject IViewLocalizer` in Razor views follow a similar pattern.</span></span> <span data-ttu-id="2c773-265">您可以使用點命名或路徑命名方式，來命名檢視的資源檔。</span><span class="sxs-lookup"><span data-stu-id="2c773-265">The resource file for a view can be named using either dot naming or path naming.</span></span> Razor<span data-ttu-id="2c773-266">查看資源檔模擬其相關聯之視圖檔案的路徑。</span><span class="sxs-lookup"><span data-stu-id="2c773-266"> view resource files mimic the path of their associated view file.</span></span> <span data-ttu-id="2c773-267">假設我們將 `ResourcesPath` 設為 "Resources"，與 *Views/Home/About.cshtml* 檢視建立關聯的法文資源檔可為下列其一：</span><span class="sxs-lookup"><span data-stu-id="2c773-267">Assuming we set the `ResourcesPath` to "Resources", the French resource file associated with the *Views/Home/About.cshtml* view could be either of the following:</span></span>

* <span data-ttu-id="2c773-268">Resources/Views/Home/About.fr.resx</span><span class="sxs-lookup"><span data-stu-id="2c773-268">Resources/Views/Home/About.fr.resx</span></span>

* <span data-ttu-id="2c773-269">Resources/Views.Home.About.fr.resx</span><span class="sxs-lookup"><span data-stu-id="2c773-269">Resources/Views.Home.About.fr.resx</span></span>

<span data-ttu-id="2c773-270">如果您不使用 `ResourcesPath` 選項，則檢視的 *.resx* 檔案會與檢視位於相同資料夾中。</span><span class="sxs-lookup"><span data-stu-id="2c773-270">If you don't use the `ResourcesPath` option, the *.resx* file for a view would be located in the same folder as the view.</span></span>

### <a name="rootnamespaceattribute"></a><span data-ttu-id="2c773-271">RootNamespaceAttribute</span><span class="sxs-lookup"><span data-stu-id="2c773-271">RootNamespaceAttribute</span></span> 

<span data-ttu-id="2c773-272">[RootNamespace](/dotnet/api/microsoft.extensions.localization.rootnamespaceattribute?view=aspnetcore-2.1) 屬性會在組件的根命名空間與組件名稱不同時，提供組件的根命名空間。</span><span class="sxs-lookup"><span data-stu-id="2c773-272">The [RootNamespace](/dotnet/api/microsoft.extensions.localization.rootnamespaceattribute?view=aspnetcore-2.1) attribute provides the root namespace of an assembly when the root namespace of an assembly is different than the assembly name.</span></span> 

> [!WARNING]
> <span data-ttu-id="2c773-273">當專案的名稱不是有效的 .NET 識別碼時，就可能發生這種情況。</span><span class="sxs-lookup"><span data-stu-id="2c773-273">This can occur when a project's name is not a valid .NET identifier.</span></span> <span data-ttu-id="2c773-274">例如， `my-project-name.csproj` 會使用根命名空間 `my_project_name` ，以及 `my-project-name` 導致此錯誤的元件名稱。</span><span class="sxs-lookup"><span data-stu-id="2c773-274">For instance `my-project-name.csproj` will use the root namespace `my_project_name` and the assembly name `my-project-name` leading to this error.</span></span> 

<span data-ttu-id="2c773-275">如果組件的根命名空間與組件名稱不同：</span><span class="sxs-lookup"><span data-stu-id="2c773-275">If the root namespace of an assembly is different than the assembly name:</span></span>

* <span data-ttu-id="2c773-276">根據預設，當地語系化無法運作。</span><span class="sxs-lookup"><span data-stu-id="2c773-276">Localization does not work by default.</span></span>
* <span data-ttu-id="2c773-277">在組件中搜尋資源的方式造成當地語系化失敗。</span><span class="sxs-lookup"><span data-stu-id="2c773-277">Localization fails due to the way resources are searched for within the assembly.</span></span> <span data-ttu-id="2c773-278">`RootNamespace` 是建置時間值，無法用於執行處理序。</span><span class="sxs-lookup"><span data-stu-id="2c773-278">`RootNamespace` is a build-time value which is not available to the executing process.</span></span> 

<span data-ttu-id="2c773-279">如果 `RootNamespace` 與 `AssemblyName` 不同，請將下列內容納入 *AssemblyInfo.cs* (參數值取代為實際值)：</span><span class="sxs-lookup"><span data-stu-id="2c773-279">If the `RootNamespace` is different from the `AssemblyName`, include the following in *AssemblyInfo.cs* (with parameter values replaced with the actual values):</span></span>

```csharp
using System.Reflection;
using Microsoft.Extensions.Localization;

[assembly: ResourceLocation("Resource Folder Name")]
[assembly: RootNamespace("App Root Namespace")]
```

<span data-ttu-id="2c773-280">上述程式碼可成功解析 resx 檔案。</span><span class="sxs-lookup"><span data-stu-id="2c773-280">The preceding code enables the successful resolution of resx files.</span></span>

## <a name="culture-fallback-behavior"></a><span data-ttu-id="2c773-281">文化特性後援行為</span><span class="sxs-lookup"><span data-stu-id="2c773-281">Culture fallback behavior</span></span>

<span data-ttu-id="2c773-282">搜尋資源時，當地語系化會使用「文化特性後援」。</span><span class="sxs-lookup"><span data-stu-id="2c773-282">When searching for a resource, localization engages in "culture fallback".</span></span> <span data-ttu-id="2c773-283">從所要求的文化特性開始，如果找不到，就會還原成該文化特性的父文化特性。</span><span class="sxs-lookup"><span data-stu-id="2c773-283">Starting from the requested culture, if not found, it reverts to the parent culture of that culture.</span></span> <span data-ttu-id="2c773-284">另外，[CultureInfo.Parent](/dotnet/api/system.globalization.cultureinfo.parent) 屬性代表父文化特性。</span><span class="sxs-lookup"><span data-stu-id="2c773-284">As an aside, the [CultureInfo.Parent](/dotnet/api/system.globalization.cultureinfo.parent) property represents the parent culture.</span></span> <span data-ttu-id="2c773-285">這通常 (但並非一定) 表示從 ISO 中移除國家意符 (Signifier)。</span><span class="sxs-lookup"><span data-stu-id="2c773-285">This usually (but not always) means removing the national signifier from the ISO.</span></span> <span data-ttu-id="2c773-286">例如，墨西哥的西班牙文方言是 "es-MX"。</span><span class="sxs-lookup"><span data-stu-id="2c773-286">For example, the dialect of Spanish spoken in Mexico is "es-MX".</span></span> <span data-ttu-id="2c773-287">它具有父系 "es" &mdash; 西班牙文不是任何國家/地區的特定項目。</span><span class="sxs-lookup"><span data-stu-id="2c773-287">It has the parent "es"&mdash;Spanish non-specific to any country.</span></span>

<span data-ttu-id="2c773-288">假設您的網站收到使用文化特性 "fr-CA" 之 "Welcome" 資源的要求。</span><span class="sxs-lookup"><span data-stu-id="2c773-288">Imagine your site receives a request for a "Welcome" resource using culture "fr-CA".</span></span> <span data-ttu-id="2c773-289">當地語系化系統會依照順序尋找下列資源，並選取第一個相符項目：</span><span class="sxs-lookup"><span data-stu-id="2c773-289">The localization system looks for the following resources, in order, and selects the first match:</span></span>

* <span data-ttu-id="2c773-290">*Welcome.fr-CA.resx*</span><span class="sxs-lookup"><span data-stu-id="2c773-290">*Welcome.fr-CA.resx*</span></span>
* <span data-ttu-id="2c773-291">*Welcome.fr.resx*</span><span class="sxs-lookup"><span data-stu-id="2c773-291">*Welcome.fr.resx*</span></span>
* <span data-ttu-id="2c773-292">*Welcome.resx* (如果 `NeutralResourcesLanguage` 是 "fr-CA")</span><span class="sxs-lookup"><span data-stu-id="2c773-292">*Welcome.resx* (if the `NeutralResourcesLanguage` is "fr-CA")</span></span>

<span data-ttu-id="2c773-293">舉例來說，如果您移除 ".fr" 文化特性指示項，並將文化特性設定為法文，則系統會讀取預設資源檔，並將字串當地語系化。</span><span class="sxs-lookup"><span data-stu-id="2c773-293">As an example, if you remove the ".fr" culture designator and you have the culture set to French, the default resource file is read and strings are localized.</span></span> <span data-ttu-id="2c773-294">當沒有任何項目符合您要求的文化特性時，資源管理員即會指定預設資源或後援資源。</span><span class="sxs-lookup"><span data-stu-id="2c773-294">The Resource manager designates a default or fallback resource for when nothing meets your requested culture.</span></span> <span data-ttu-id="2c773-295">如果您只想在要求的文化特性缺少資源時傳回索引鍵，就不能使用預設資源檔。</span><span class="sxs-lookup"><span data-stu-id="2c773-295">If you want to just return the key when missing a resource for the requested culture you must not have a default resource file.</span></span>

### <a name="generate-resource-files-with-visual-studio"></a><span data-ttu-id="2c773-296">使用 Visual Studio 產生資源檔</span><span class="sxs-lookup"><span data-stu-id="2c773-296">Generate resource files with Visual Studio</span></span>

<span data-ttu-id="2c773-297">如果您在 Visual Studio 中建立資源檔，但檔案名稱中不含文化特性 (例如 *Welcome.resx*)，則 Visual Studio 會針對每個字串的屬性建立 C# 類別。</span><span class="sxs-lookup"><span data-stu-id="2c773-297">If you create a resource file in Visual Studio without a culture in the file name (for example, *Welcome.resx*), Visual Studio will create a C# class with a property for each string.</span></span> <span data-ttu-id="2c773-298">但這通常不是您使用 ASP.NET Core 的初衷。</span><span class="sxs-lookup"><span data-stu-id="2c773-298">That's usually not what you want with ASP.NET Core.</span></span> <span data-ttu-id="2c773-299">一般來說，您不會有預設的 *.resx* 資源檔 (不含文化特性名稱的 *.resx* 檔案)。</span><span class="sxs-lookup"><span data-stu-id="2c773-299">You typically don't have a default *.resx* resource file (a *.resx* file without the culture name).</span></span> <span data-ttu-id="2c773-300">因此，建議您建立含有文化特性名稱的 *.resx* 檔案 (例如 *Welcome.fr.resx*)。</span><span class="sxs-lookup"><span data-stu-id="2c773-300">We suggest you create the *.resx* file with a culture name (for example *Welcome.fr.resx*).</span></span> <span data-ttu-id="2c773-301">當您建立含有文化特性名稱的 *.resx* 檔案時，Visual Studio 就不會產生類別檔案。</span><span class="sxs-lookup"><span data-stu-id="2c773-301">When you create a *.resx* file with a culture name, Visual Studio won't generate the class file.</span></span>

### <a name="add-other-cultures"></a><span data-ttu-id="2c773-302">新增其他文化特性</span><span class="sxs-lookup"><span data-stu-id="2c773-302">Add other cultures</span></span>

<span data-ttu-id="2c773-303">每種語言和文化特性的組合 (非預設語言) 都需要唯一的資源檔。</span><span class="sxs-lookup"><span data-stu-id="2c773-303">Each language and culture combination (other than the default language) requires a unique resource file.</span></span> <span data-ttu-id="2c773-304">若要建立不同文化特性和地區設定的資源檔，您可以建立新的資源檔並將 ISO 語言代碼作為檔名的一部分 (例如 **en-us**、**fr-ca** 和 **en-gb**)。</span><span class="sxs-lookup"><span data-stu-id="2c773-304">You create resource files for different cultures and locales by creating new resource files in which the ISO language codes are part of the file name (for example, **en-us**, **fr-ca**, and **en-gb**).</span></span> <span data-ttu-id="2c773-305">您應將 ISO 代碼置於檔案名稱和 *.resx* 副檔名之間，例如 *Welcome.es-MX.resx* (西班牙文/墨西哥)。</span><span class="sxs-lookup"><span data-stu-id="2c773-305">These ISO codes are placed between the file name and the *.resx* file extension, as in *Welcome.es-MX.resx* (Spanish/Mexico).</span></span>

## <a name="implement-a-strategy-to-select-the-languageculture-for-each-request"></a><span data-ttu-id="2c773-306">實作可依據每項要求選取語言/文化特性的策略</span><span class="sxs-lookup"><span data-stu-id="2c773-306">Implement a strategy to select the language/culture for each request</span></span>

### <a name="configure-localization"></a><span data-ttu-id="2c773-307">設定當地語系化</span><span class="sxs-lookup"><span data-stu-id="2c773-307">Configure localization</span></span>

<span data-ttu-id="2c773-308">您可以在 `Startup.ConfigureServices` 方法中設定當地語系化：</span><span class="sxs-lookup"><span data-stu-id="2c773-308">Localization is configured in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](localization/sample/Localization/Startup.cs?name=snippet1)]

* <span data-ttu-id="2c773-309">`AddLocalization` 可將當地語系化服務新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="2c773-309">`AddLocalization` Adds the localization services to the services container.</span></span> <span data-ttu-id="2c773-310">上方的程式碼也會將資源路徑設為 "Resources"。</span><span class="sxs-lookup"><span data-stu-id="2c773-310">The code above also sets the resources path to "Resources".</span></span>

* <span data-ttu-id="2c773-311">`AddViewLocalization` 可支援當地語系化的檢視檔案。</span><span class="sxs-lookup"><span data-stu-id="2c773-311">`AddViewLocalization` Adds support for localized view files.</span></span> <span data-ttu-id="2c773-312">在此範例中，檢視的當地語系化會以檢視檔案的後置詞為依據。</span><span class="sxs-lookup"><span data-stu-id="2c773-312">In this sample view localization is based on the view file suffix.</span></span> <span data-ttu-id="2c773-313">例如 *Index.fr.cshtml* 檔案中的 "fr"。</span><span class="sxs-lookup"><span data-stu-id="2c773-313">For example "fr" in the *Index.fr.cshtml* file.</span></span>

* <span data-ttu-id="2c773-314">`AddDataAnnotationsLocalization` 可支援透過 `IStringLocalizer` 抽象概念而來的當地語系化 `DataAnnotations` 驗證訊息。</span><span class="sxs-lookup"><span data-stu-id="2c773-314">`AddDataAnnotationsLocalization` Adds support for localized `DataAnnotations` validation messages through `IStringLocalizer` abstractions.</span></span>

### <a name="localization-middleware"></a><span data-ttu-id="2c773-315">當地語系化中介軟體</span><span class="sxs-lookup"><span data-stu-id="2c773-315">Localization middleware</span></span>

<span data-ttu-id="2c773-316">您可以在當地語系化[中介軟體](xref:fundamentals/middleware/index)中，設定要求目前的文化特性。</span><span class="sxs-lookup"><span data-stu-id="2c773-316">The current culture on a request is set in the localization [Middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="2c773-317">已在 `Startup.Configure` 方法中啟用當地語系化中介軟體。</span><span class="sxs-lookup"><span data-stu-id="2c773-317">The localization middleware is enabled in the `Startup.Configure` method.</span></span> <span data-ttu-id="2c773-318">您必須在可能檢查要求文化特性的任何中介軟體之前，設定當地語系化中介軟體 (例如 `app.UseMvcWithDefaultRoute()`)。</span><span class="sxs-lookup"><span data-stu-id="2c773-318">The localization middleware must be configured before any middleware which might check the request culture (for example, `app.UseMvcWithDefaultRoute()`).</span></span>

[!code-csharp[](localization/sample/Localization/Startup.cs?name=snippet2)]
[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

<span data-ttu-id="2c773-319">`UseRequestLocalization` 會初始化 `RequestLocalizationOptions` 物件。</span><span class="sxs-lookup"><span data-stu-id="2c773-319">`UseRequestLocalization` initializes a `RequestLocalizationOptions` object.</span></span> <span data-ttu-id="2c773-320">在每次要求時，系統會列舉 `RequestLocalizationOptions` 中的 `RequestCultureProvider` 清單，並使用能成功判斷要求的文化特性的第一個提供者。</span><span class="sxs-lookup"><span data-stu-id="2c773-320">On every request the list of `RequestCultureProvider` in the `RequestLocalizationOptions` is enumerated and the first provider that can successfully determine the request culture is used.</span></span> <span data-ttu-id="2c773-321">預設的提供者是來自 `RequestLocalizationOptions` 類別：</span><span class="sxs-lookup"><span data-stu-id="2c773-321">The default providers come from the `RequestLocalizationOptions` class:</span></span>

1. `QueryStringRequestCultureProvider`
2. `CookieRequestCultureProvider`
3. `AcceptLanguageHeaderRequestCultureProvider`

<span data-ttu-id="2c773-322">預設清單會由針對性高到低來排列。</span><span class="sxs-lookup"><span data-stu-id="2c773-322">The default list goes from most specific to least specific.</span></span> <span data-ttu-id="2c773-323">在本文稍後，我們會說明如何變更順序，甚至新增自訂的文化特性提供者。</span><span class="sxs-lookup"><span data-stu-id="2c773-323">Later in the article we'll see how you can change the order and even add a custom culture provider.</span></span> <span data-ttu-id="2c773-324">如果沒有任何提供者可以判斷要求的文化特性，即會使用 `DefaultRequestCulture`。</span><span class="sxs-lookup"><span data-stu-id="2c773-324">If none of the providers can determine the request culture, the `DefaultRequestCulture` is used.</span></span>

### <a name="querystringrequestcultureprovider"></a><span data-ttu-id="2c773-325">QueryStringRequestCultureProvider</span><span class="sxs-lookup"><span data-stu-id="2c773-325">QueryStringRequestCultureProvider</span></span>

<span data-ttu-id="2c773-326">有些應用程式會使用查詢字串來設定[文化特性和 UI 文化特性](https://msdn.microsoft.com/library/system.globalization.cultureinfo.aspx)。</span><span class="sxs-lookup"><span data-stu-id="2c773-326">Some apps will use a query string to set the [culture and UI culture](https://msdn.microsoft.com/library/system.globalization.cultureinfo.aspx).</span></span> <span data-ttu-id="2c773-327">若是使用 Cookie 或 Accept-Language 標頭方法的應用程式，您可以將查詢字串新增至 URL 以偵錯和測試程式碼。</span><span class="sxs-lookup"><span data-stu-id="2c773-327">For apps that use the cookie or Accept-Language header approach, adding a query string to the URL is useful for debugging and testing code.</span></span> <span data-ttu-id="2c773-328">系統預設會將 `QueryStringRequestCultureProvider` 登錄為 `RequestCultureProvider` 清單中的第一個當地語系化提供者。</span><span class="sxs-lookup"><span data-stu-id="2c773-328">By default, the `QueryStringRequestCultureProvider` is registered as the first localization provider in the `RequestCultureProvider` list.</span></span> <span data-ttu-id="2c773-329">您應傳遞查詢字串參數 `culture` 和 `ui-culture`。</span><span class="sxs-lookup"><span data-stu-id="2c773-329">You pass the query string parameters `culture` and `ui-culture`.</span></span> <span data-ttu-id="2c773-330">下列範例會設定西班牙文/墨西哥的特定文化特性 (語言和地區)：</span><span class="sxs-lookup"><span data-stu-id="2c773-330">The following example sets the specific culture (language and region) to Spanish/Mexico:</span></span>

   `http://localhost:5000/?culture=es-MX&ui-culture=es-MX`

<span data-ttu-id="2c773-331">如果您只傳入兩者其一 (`culture` 或 `ui-culture`)，查詢字串提供者就會使用您傳入的項目來設定這兩個值。</span><span class="sxs-lookup"><span data-stu-id="2c773-331">If you only pass in one of the two (`culture` or `ui-culture`), the query string provider will set both values using the one you passed in.</span></span> <span data-ttu-id="2c773-332">例如，若只設定文化特性，即會同時設定 `Culture` 和 `UICulture`：</span><span class="sxs-lookup"><span data-stu-id="2c773-332">For example, setting just the culture will set both the `Culture` and the `UICulture`:</span></span>

   `http://localhost:5000/?culture=es-MX`

### <a name="cookierequestcultureprovider"></a><span data-ttu-id="2c773-333">CookieRequestCultureProvider</span><span class="sxs-lookup"><span data-stu-id="2c773-333">CookieRequestCultureProvider</span></span>

<span data-ttu-id="2c773-334">生產環境應用程式通常會提供一個機制，來設定 ASP.NET Core 文化特性 Cookie 的文化特性。</span><span class="sxs-lookup"><span data-stu-id="2c773-334">Production apps will often provide a mechanism to set the culture with the ASP.NET Core culture cookie.</span></span> <span data-ttu-id="2c773-335">若要建立 Cookie，請使用 `MakeCookieValue` 方法。</span><span class="sxs-lookup"><span data-stu-id="2c773-335">Use the `MakeCookieValue` method to create a cookie.</span></span>

<span data-ttu-id="2c773-336">會傳回 `CookieRequestCultureProvider` `DefaultCookieName` 用來追蹤使用者慣用文化特性資訊的預設 cookie 名稱。</span><span class="sxs-lookup"><span data-stu-id="2c773-336">The `CookieRequestCultureProvider` `DefaultCookieName` returns the default cookie name used to track the user's preferred culture information.</span></span> <span data-ttu-id="2c773-337">預設 Cookie 名稱為 `.AspNetCore.Culture`。</span><span class="sxs-lookup"><span data-stu-id="2c773-337">The default cookie name is `.AspNetCore.Culture`.</span></span>

<span data-ttu-id="2c773-338">Cookie 格式為 `c=%LANGCODE%|uic=%LANGCODE%`，其中 `c` 是 `Culture` 而 `uic` 是 `UICulture`，例如：</span><span class="sxs-lookup"><span data-stu-id="2c773-338">The cookie format is `c=%LANGCODE%|uic=%LANGCODE%`, where `c` is `Culture` and `uic` is `UICulture`, for example:</span></span>

    c=en-UK|uic=en-US

<span data-ttu-id="2c773-339">如果您只指定文化特性資訊和 UI 文化特性其一，系統就會將您所指定的文化特性用於文化特性資訊和 UI 文化特性。</span><span class="sxs-lookup"><span data-stu-id="2c773-339">If you only specify one of culture info and UI culture, the specified culture will be used for both culture info and UI culture.</span></span>

### <a name="the-accept-language-http-header"></a><span data-ttu-id="2c773-340">Accept-Language HTTP 標頭</span><span class="sxs-lookup"><span data-stu-id="2c773-340">The Accept-Language HTTP header</span></span>

<span data-ttu-id="2c773-341">您可在大多數的瀏覽器中設定 [Accept-Language 標頭](https://www.w3.org/International/questions/qa-accept-lang-locales)，其最初的設計目的是用來指定使用者的語言。</span><span class="sxs-lookup"><span data-stu-id="2c773-341">The [Accept-Language header](https://www.w3.org/International/questions/qa-accept-lang-locales) is settable in most browsers and was originally intended to specify the user's language.</span></span> <span data-ttu-id="2c773-342">這項設定可指出瀏覽器已設好要傳送哪些項目，或已從基礎作業系統繼承哪些項目。</span><span class="sxs-lookup"><span data-stu-id="2c773-342">This setting indicates what the browser has been set to send or has inherited from the underlying operating system.</span></span> <span data-ttu-id="2c773-343">透過瀏覽器要求的 Accept-Language HTTP 標頭來偵測使用者的慣用語言，並非萬無一失 (請參閱 [Setting language preferences in a browser](https://www.w3.org/International/questions/qa-lang-priorities.en.php) (在瀏覽器中設定語言喜好設定)。</span><span class="sxs-lookup"><span data-stu-id="2c773-343">The Accept-Language HTTP header from a browser request isn't an infallible way to detect the user's preferred language (see [Setting language preferences in a browser](https://www.w3.org/International/questions/qa-lang-priorities.en.php)).</span></span> <span data-ttu-id="2c773-344">生產環境應用程式應該包含可讓使用者自訂文化特性的選擇方式。</span><span class="sxs-lookup"><span data-stu-id="2c773-344">A production app should include a way for a user to customize their choice of culture.</span></span>

### <a name="set-the-accept-language-http-header-in-ie"></a><span data-ttu-id="2c773-345">在 IE 中設定 Accept-Language HTTP 標頭</span><span class="sxs-lookup"><span data-stu-id="2c773-345">Set the Accept-Language HTTP header in IE</span></span>

1. <span data-ttu-id="2c773-346">從齒輪圖示，點選 [網際網路選項]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="2c773-346">From the gear icon, tap **Internet Options**.</span></span>

2. <span data-ttu-id="2c773-347">點選 [語言]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="2c773-347">Tap **Languages**.</span></span>

    ![網際網路選項](localization/_static/lang.png)

3. <span data-ttu-id="2c773-349">點選 [設定語言喜好設定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="2c773-349">Tap **Set Language Preferences**.</span></span>

4. <span data-ttu-id="2c773-350">點選 [新增語言]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="2c773-350">Tap **Add a language**.</span></span>

5. <span data-ttu-id="2c773-351">新增語言。</span><span class="sxs-lookup"><span data-stu-id="2c773-351">Add the language.</span></span>

6. <span data-ttu-id="2c773-352">點選語言，然後點選 [上移]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="2c773-352">Tap the language, then tap **Move Up**.</span></span>

### <a name="use-a-custom-provider"></a><span data-ttu-id="2c773-353">使用自訂提供者</span><span class="sxs-lookup"><span data-stu-id="2c773-353">Use a custom provider</span></span>

<span data-ttu-id="2c773-354">假設您想要讓客戶將他們的語言和文化特性儲存在您的資料庫中。</span><span class="sxs-lookup"><span data-stu-id="2c773-354">Suppose you want to let your customers store their language and culture in your databases.</span></span> <span data-ttu-id="2c773-355">您可以撰寫提供者，以供使用者查詢這些值。</span><span class="sxs-lookup"><span data-stu-id="2c773-355">You could write a provider to look up these values for the user.</span></span> <span data-ttu-id="2c773-356">下列程式碼會示範如何新增自訂提供者：</span><span class="sxs-lookup"><span data-stu-id="2c773-356">The following code shows how to add a custom provider:</span></span>

```csharp
private const string enUSCulture = "en-US";

services.Configure<RequestLocalizationOptions>(options =>
{
    var supportedCultures = new[]
    {
        new CultureInfo(enUSCulture),
        new CultureInfo("fr")
    };

    options.DefaultRequestCulture = new RequestCulture(culture: enUSCulture, uiCulture: enUSCulture);
    options.SupportedCultures = supportedCultures;
    options.SupportedUICultures = supportedCultures;

    options.AddInitialRequestCultureProvider(new CustomRequestCultureProvider(async context =>
    {
        // My custom request culture logic
        return new ProviderCultureResult("en");
    }));
});
```

<span data-ttu-id="2c773-357">使用 `RequestLocalizationOptions` 新增或移除當地語系化提供者。</span><span class="sxs-lookup"><span data-stu-id="2c773-357">Use `RequestLocalizationOptions` to add or remove localization providers.</span></span>

### <a name="set-the-culture-programmatically"></a><span data-ttu-id="2c773-358">以程式設計方式來設定文化特性</span><span class="sxs-lookup"><span data-stu-id="2c773-358">Set the culture programmatically</span></span>

<span data-ttu-id="2c773-359">[GitHub](https://github.com/aspnet/entropy) 上的這個範例 **Localization.StarterWeb** 專案包含可設定 `Culture` 的 UI。</span><span class="sxs-lookup"><span data-stu-id="2c773-359">This sample **Localization.StarterWeb** project on [GitHub](https://github.com/aspnet/entropy) contains UI to set the `Culture`.</span></span> <span data-ttu-id="2c773-360">*Views/Shared/_SelectLanguagePartial.cshtml* 檔可讓您從支援的文化特性清單中選取文化特性：</span><span class="sxs-lookup"><span data-stu-id="2c773-360">The *Views/Shared/_SelectLanguagePartial.cshtml* file allows you to select the culture from the list of supported cultures:</span></span>

[!code-cshtml[](localization/sample/Localization/Views/Shared/_SelectLanguagePartial.cshtml)]

<span data-ttu-id="2c773-361">系統會將 *Views/Shared/_SelectLanguagePartial.cshtml* 檔案新增至配置檔案的 `footer` 區段，以供所有檢視使用：</span><span class="sxs-lookup"><span data-stu-id="2c773-361">The *Views/Shared/_SelectLanguagePartial.cshtml* file is added to the `footer` section of the layout file so it will be available to all views:</span></span>

[!code-cshtml[](localization/sample/Localization/Views/Shared/_Layout.cshtml?range=43-56&highlight=10)]

<span data-ttu-id="2c773-362">`SetLanguage` 方法會設定文化特性的 Cookie。</span><span class="sxs-lookup"><span data-stu-id="2c773-362">The `SetLanguage` method sets the culture cookie.</span></span>

[!code-csharp[](localization/sample/Localization/Controllers/HomeController.cs?range=57-67)]

<span data-ttu-id="2c773-363">您無法將 *_SelectLanguagePartial.cshtml* 插入這個專案的範例程式碼。</span><span class="sxs-lookup"><span data-stu-id="2c773-363">You can't plug in the *_SelectLanguagePartial.cshtml* to sample code for this project.</span></span> <span data-ttu-id="2c773-364">[GitHub](https://github.com/aspnet/entropy)上的**localization.starterweb**專案具有程式碼，可透過相依性 `RequestLocalizationOptions` 插入容器將傳遞至 Razor 部分。 [Dependency Injection](dependency-injection.md)</span><span class="sxs-lookup"><span data-stu-id="2c773-364">The **Localization.StarterWeb** project on [GitHub](https://github.com/aspnet/entropy) has code to flow the `RequestLocalizationOptions` to a Razor partial through the [Dependency Injection](dependency-injection.md) container.</span></span>

## <a name="model-binding-route-data-and-query-strings"></a><span data-ttu-id="2c773-365">模型系結路由資料和查詢字串</span><span class="sxs-lookup"><span data-stu-id="2c773-365">Model binding route data and query strings</span></span>

<span data-ttu-id="2c773-366">請參閱模型系結[路由資料和查詢字串的全球化行為](xref:mvc/models/model-binding#glob)。</span><span class="sxs-lookup"><span data-stu-id="2c773-366">See [Globalization behavior of model binding route data and query strings](xref:mvc/models/model-binding#glob).</span></span>

## <a name="globalization-and-localization-terms"></a><span data-ttu-id="2c773-367">全球化和當地語系化詞彙</span><span class="sxs-lookup"><span data-stu-id="2c773-367">Globalization and localization terms</span></span>

<span data-ttu-id="2c773-368">在進行應用程式的當地語系化程序時，您也需要具備現代軟體開發中常用的相關字元集基本知識，並了解與它們建立關聯的問題。</span><span class="sxs-lookup"><span data-stu-id="2c773-368">The process of localizing your app also requires a basic understanding of relevant character sets commonly used in modern software development and an understanding of the issues associated with them.</span></span> <span data-ttu-id="2c773-369">雖然所有電腦都會將文字儲存為數字 (程式碼)，但不同系統會使用不同數字來儲存相同的文字。</span><span class="sxs-lookup"><span data-stu-id="2c773-369">Although all computers store text as numbers (codes), different systems store the same text using different numbers.</span></span> <span data-ttu-id="2c773-370">當地語系化是指針對特定文化特性/地區設定，轉譯應用程式使用者介面 (UI) 的程序。</span><span class="sxs-lookup"><span data-stu-id="2c773-370">The localization process refers to translating the app user interface (UI) for a specific culture/locale.</span></span>

<span data-ttu-id="2c773-371">[可當地語系化](/dotnet/standard/globalization-localization/localizability-review)是確認全球化應用程式已準備好進行當地語系化的中繼程序。</span><span class="sxs-lookup"><span data-stu-id="2c773-371">[Localizability](/dotnet/standard/globalization-localization/localizability-review) is an intermediate process for verifying that a globalized app is ready for localization.</span></span>

<span data-ttu-id="2c773-372">文化特性名稱的 [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) 格式是 `<languagecode2>-<country/regioncode2>`，其中 `<languagecode2>` 是語言代碼，而 `<country/regioncode2>` 是子文化特性代碼。</span><span class="sxs-lookup"><span data-stu-id="2c773-372">The [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) format for the culture name is `<languagecode2>-<country/regioncode2>`, where `<languagecode2>` is the language code and `<country/regioncode2>` is the subculture code.</span></span> <span data-ttu-id="2c773-373">例如，`es-CL` 是指西班牙文 (智利)，`en-US` 是指英文 (美國)，而 `en-AU` 是指英文 (澳大利亞)。</span><span class="sxs-lookup"><span data-stu-id="2c773-373">For example, `es-CL` for Spanish (Chile), `en-US` for English (United States), and `en-AU` for English (Australia).</span></span> <span data-ttu-id="2c773-374">[RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) 是 ISO 639 (兩個小寫字母，為與某個語言建立關聯的文化特性代碼) 及 ISO 3166 (兩個大寫字母，為與某個國家或地區建立關聯的子文化特性代碼) 的組合。</span><span class="sxs-lookup"><span data-stu-id="2c773-374">[RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) is a combination of an ISO 639 two-letter lowercase culture code associated with a language and an ISO 3166 two-letter uppercase subculture code associated with a country or region.</span></span> <span data-ttu-id="2c773-375">請參閱 [Language Culture Name](https://msdn.microsoft.com/library/ee825488(v=cs.20).aspx) (語言的文化特性名稱)。</span><span class="sxs-lookup"><span data-stu-id="2c773-375">See [Language Culture Name](https://msdn.microsoft.com/library/ee825488(v=cs.20).aspx).</span></span>

<span data-ttu-id="2c773-376">國際化通常縮寫為 "I18N"。</span><span class="sxs-lookup"><span data-stu-id="2c773-376">Internationalization is often abbreviated to "I18N".</span></span> <span data-ttu-id="2c773-377">這個縮寫是採用該詞彙的第一個和最後一個字母，以及這兩個字母間的字母數組成，因此 18 代表第一個字母 "I" 及最後 "N" 中間的字母數。</span><span class="sxs-lookup"><span data-stu-id="2c773-377">The abbreviation takes the first and last letters and the number of letters between them, so 18 stands for the number of letters between the first "I" and the last "N".</span></span> <span data-ttu-id="2c773-378">同樣的原則也適用於全球化 (G11N) 與當地語系化 (L10N) 的縮寫。</span><span class="sxs-lookup"><span data-stu-id="2c773-378">The same applies to Globalization (G11N), and Localization (L10N).</span></span>

<span data-ttu-id="2c773-379">詞彙：</span><span class="sxs-lookup"><span data-stu-id="2c773-379">Terms:</span></span>

* <span data-ttu-id="2c773-380">全球化 (G11N)：讓應用程式支援不同語言和區域的程序。</span><span class="sxs-lookup"><span data-stu-id="2c773-380">Globalization (G11N): The process of making an app support different languages and regions.</span></span>
* <span data-ttu-id="2c773-381">當地語系化 (L10N)：針對特定語言和區域，自訂應用程式的程序。</span><span class="sxs-lookup"><span data-stu-id="2c773-381">Localization (L10N): The process of customizing an app for a given language and region.</span></span>
* <span data-ttu-id="2c773-382">國際化 (I18N)：包含全球化和當地語系化這兩部分的描述。</span><span class="sxs-lookup"><span data-stu-id="2c773-382">Internationalization (I18N): Describes both globalization and localization.</span></span>
* <span data-ttu-id="2c773-383">文化特性：它是一種語言和/或地區。</span><span class="sxs-lookup"><span data-stu-id="2c773-383">Culture: It's a language and, optionally, a region.</span></span>
* <span data-ttu-id="2c773-384">中性文化特性：具有指定的語言但不限地區的文化特性 </span><span class="sxs-lookup"><span data-stu-id="2c773-384">Neutral culture: A culture that has a specified language, but not a region.</span></span> <span data-ttu-id="2c773-385">(例如 "en"、"es")。</span><span class="sxs-lookup"><span data-stu-id="2c773-385">(for example "en", "es")</span></span>
* <span data-ttu-id="2c773-386">特定文化特性：具有指定的語言和地區的文化特性 </span><span class="sxs-lookup"><span data-stu-id="2c773-386">Specific culture: A culture that has a specified language and region.</span></span> <span data-ttu-id="2c773-387">(例如 "en-US"、"en-GB"、"es-CL")。</span><span class="sxs-lookup"><span data-stu-id="2c773-387">(for example "en-US", "en-GB", "es-CL")</span></span>
* <span data-ttu-id="2c773-388">父文化特性：包含特定文化特性的中性文化特性 </span><span class="sxs-lookup"><span data-stu-id="2c773-388">Parent culture: The neutral culture that contains a specific culture.</span></span> <span data-ttu-id="2c773-389">(例如，"en" 是 "en-US" 和 "en-GB" 的父文化特性)。</span><span class="sxs-lookup"><span data-stu-id="2c773-389">(for example, "en" is the parent culture of "en-US" and "en-GB")</span></span>
* <span data-ttu-id="2c773-390">地區設定：地區設定與文化特性相同。</span><span class="sxs-lookup"><span data-stu-id="2c773-390">Locale: A locale is the same as a culture.</span></span>

[!INCLUDE[](~/includes/localization/currency.md)]

[!INCLUDE[](~/includes/localization/unsupported-culture-log-level.md)]

## <a name="additional-resources"></a><span data-ttu-id="2c773-391">其他資源</span><span class="sxs-lookup"><span data-stu-id="2c773-391">Additional resources</span></span>

* <xref:fundamentals/troubleshoot-aspnet-core-localization>
* <span data-ttu-id="2c773-392">本文使用的 [Localization.StarterWeb 專案](https://github.com/aspnet/Entropy/tree/master/samples/Localization.StarterWeb)。</span><span class="sxs-lookup"><span data-stu-id="2c773-392">[Localization.StarterWeb project](https://github.com/aspnet/Entropy/tree/master/samples/Localization.StarterWeb) used in the article.</span></span>
* [<span data-ttu-id="2c773-393">全球化與當地語系化 .NET 應用程式</span><span class="sxs-lookup"><span data-stu-id="2c773-393">Globalizing and localizing .NET applications</span></span>](/dotnet/standard/globalization-localization/index)
* [<span data-ttu-id="2c773-394">.Resx 檔案中的資源</span><span class="sxs-lookup"><span data-stu-id="2c773-394">Resources in .resx Files</span></span>](/dotnet/framework/resources/working-with-resx-files-programmatically)
* [<span data-ttu-id="2c773-395">Microsoft 多語應用程式工具組</span><span class="sxs-lookup"><span data-stu-id="2c773-395">Microsoft Multilingual App Toolkit</span></span>](https://marketplace.visualstudio.com/items?itemName=MultilingualAppToolkit.MultilingualAppToolkit-18308)
* [<span data-ttu-id="2c773-396">當地語系化和泛型</span><span class="sxs-lookup"><span data-stu-id="2c773-396">Localization & Generics</span></span>](http://hishambinateya.com/localization-and-generics)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="2c773-397">由 [Rick Anderson](https://twitter.com/RickAndMSFT)、[Damien Bowden](https://twitter.com/damien_bod)、[Bart Calixto](https://twitter.com/bartmax)、[Nadeem Afana](https://afana.me/) 和 [Hisham Bin Ateya](https://twitter.com/hishambinateya) 提供</span><span class="sxs-lookup"><span data-stu-id="2c773-397">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Damien Bowden](https://twitter.com/damien_bod), [Bart Calixto](https://twitter.com/bartmax), [Nadeem Afana](https://afana.me/), and [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span></span>

<span data-ttu-id="2c773-398">多語系網站可讓網站觸及更多物件。</span><span class="sxs-lookup"><span data-stu-id="2c773-398">A multilingual website allows the site to reach a wider audience.</span></span> <span data-ttu-id="2c773-399">ASP.NET Core 提供服務與中介軟體，可將網站當地語系化成不同的語言與文化特性。</span><span class="sxs-lookup"><span data-stu-id="2c773-399">ASP.NET Core provides services and middleware for localizing into different languages and cultures.</span></span>

<span data-ttu-id="2c773-400">國際化包含[全球化](/dotnet/api/system.globalization)和[當地語系化](/dotnet/standard/globalization-localization/localization)這兩部分。</span><span class="sxs-lookup"><span data-stu-id="2c773-400">Internationalization involves [Globalization](/dotnet/api/system.globalization) and [Localization](/dotnet/standard/globalization-localization/localization).</span></span> <span data-ttu-id="2c773-401">全球化是指設計出可支援不同文化特性之應用程式的程序。</span><span class="sxs-lookup"><span data-stu-id="2c773-401">Globalization is the process of designing apps that support different cultures.</span></span> <span data-ttu-id="2c773-402">透過全球化，您可新增支援與特定地區相關之已定義語言指令碼的輸入、顯示及輸出作業。</span><span class="sxs-lookup"><span data-stu-id="2c773-402">Globalization adds support for input, display, and output of a defined set of language scripts that relate to specific geographic areas.</span></span>

<span data-ttu-id="2c773-403">當地語系化是指針對全球化應用程式進行調整的程序，且您已順應特定文化特性/地區設定對這些全球化應用程式進行可當地語系化處理。</span><span class="sxs-lookup"><span data-stu-id="2c773-403">Localization is the process of adapting a globalized app, which you have already processed for localizability, to a particular culture/locale.</span></span> <span data-ttu-id="2c773-404">如需詳細資訊，請參閱本文件結尾處的**全球化和當地語系化詞彙**。</span><span class="sxs-lookup"><span data-stu-id="2c773-404">For more information see **Globalization and localization terms** near the end of this document.</span></span>

<span data-ttu-id="2c773-405">應用程式當地語系化包含下列作業：</span><span class="sxs-lookup"><span data-stu-id="2c773-405">App localization involves the following:</span></span>

1. <span data-ttu-id="2c773-406">讓應用程式的內容可當地語系化</span><span class="sxs-lookup"><span data-stu-id="2c773-406">Make the app's content localizable</span></span>
1. <span data-ttu-id="2c773-407">針對您支援的語言和文化特性提供當地語系化資源</span><span class="sxs-lookup"><span data-stu-id="2c773-407">Provide localized resources for the languages and cultures you support</span></span>
1. <span data-ttu-id="2c773-408">實作可依據每項要求選取語言/文化特性的策略</span><span class="sxs-lookup"><span data-stu-id="2c773-408">Implement a strategy to select the language/culture for each request</span></span>

<span data-ttu-id="2c773-409">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/localization/sample/Localization)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="2c773-409">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/localization/sample/Localization) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="make-the-apps-content-localizable"></a><span data-ttu-id="2c773-410">讓應用程式的內容可當地語系化</span><span class="sxs-lookup"><span data-stu-id="2c773-410">Make the app's content localizable</span></span>

<span data-ttu-id="2c773-411"><xref:Microsoft.Extensions.Localization.IStringLocalizer>` and <xref:Microsoft.Extensions.Localization.IStringLocalizer%601> were architected to improve productivity when developing localized apps. `IStringLocalizer ` uses the [ResourceManager](/dotnet/api/system.resources.resourcemanager) and [ResourceReader](/dotnet/api/system.resources.resourcereader) to provide culture-specific resources at run time. The interface has an indexer and an ` IEnumerable ` for returning localized strings. ` IStringLocalizer ' 不需要將預設語言字串儲存在資源檔中。</span><span class="sxs-lookup"><span data-stu-id="2c773-411"><xref:Microsoft.Extensions.Localization.IStringLocalizer>` and <xref:Microsoft.Extensions.Localization.IStringLocalizer%601> were architected to improve productivity when developing localized apps. `IStringLocalizer` uses the [ResourceManager](/dotnet/api/system.resources.resourcemanager) and [ResourceReader](/dotnet/api/system.resources.resourcereader) to provide culture-specific resources at run time. The interface has an indexer and an `IEnumerable` for returning localized strings. `IStringLocalizer\` doesn't require storing the default language strings in a resource file.</span></span> <span data-ttu-id="2c773-412">您不必在開發初期建立資源檔，即可開發以當地語系化為目標的應用程式。</span><span class="sxs-lookup"><span data-stu-id="2c773-412">You can develop an app targeted for localization and not need to create resource files early in development.</span></span> <span data-ttu-id="2c773-413">下列程式碼會示範如何包裝 "About Title" 字串以進行當地語系化。</span><span class="sxs-lookup"><span data-stu-id="2c773-413">The code below shows how to wrap the string "About Title" for localization.</span></span>

[!code-csharp[](localization/sample/Localization/Controllers/AboutController.cs)]

<span data-ttu-id="2c773-414">在上述程式碼中， `IStringLocalizer<T>` 執行是來自相依性[插入](dependency-injection.md)。</span><span class="sxs-lookup"><span data-stu-id="2c773-414">In the preceding code, the `IStringLocalizer<T>` implementation comes from [Dependency Injection](dependency-injection.md).</span></span> <span data-ttu-id="2c773-415">如果找不到 "About Title" 的當地語系化值，即會傳回索引子的索引鍵，也就是 "About Title" 字串。</span><span class="sxs-lookup"><span data-stu-id="2c773-415">If the localized value of "About Title" isn't found, then the indexer key is returned, that is, the string "About Title".</span></span> <span data-ttu-id="2c773-416">您可以保留應用程式中的預設語言常值字串，並將其包裝在當地語系化工具中，以便專注於開發應用程式。</span><span class="sxs-lookup"><span data-stu-id="2c773-416">You can leave the default language literal strings in the app and wrap them in the localizer, so that you can focus on developing the app.</span></span> <span data-ttu-id="2c773-417">您不用先建立預設資源檔，即可使用預設語言來開發應用程式，並針對當地語系化步驟進行應用程式的準備。</span><span class="sxs-lookup"><span data-stu-id="2c773-417">You develop your app with your default language and prepare it for the localization step without first creating a default resource file.</span></span> <span data-ttu-id="2c773-418">或者，您可以使用傳統方法，並提供索引鍵以擷取預設語言字串。</span><span class="sxs-lookup"><span data-stu-id="2c773-418">Alternatively, you can use the traditional approach and provide a key to retrieve the default language string.</span></span> <span data-ttu-id="2c773-419">對許多開發人員來說，新的工作流程 (單純包裝字串常值而不使用預設語言的 *.resx* 檔案) 可以降低當地語系化應用程式的額外負荷。</span><span class="sxs-lookup"><span data-stu-id="2c773-419">For many developers the new workflow of not having a default language *.resx* file and simply wrapping the string literals can reduce the overhead of localizing an app.</span></span> <span data-ttu-id="2c773-420">其他開發人員則偏好傳統的工作流程，因為這種方法更便於使用較長的字串常值，且更易於更新當地語系化的字串。</span><span class="sxs-lookup"><span data-stu-id="2c773-420">Other developers will prefer the traditional work flow as it can make it easier to work with longer string literals and make it easier to update localized strings.</span></span>

<span data-ttu-id="2c773-421">若是包含 HTML 的資源，請使用 `IHtmlLocalizer<T>` 實作。</span><span class="sxs-lookup"><span data-stu-id="2c773-421">Use the `IHtmlLocalizer<T>` implementation for resources that contain HTML.</span></span> <span data-ttu-id="2c773-422">`IHtmlLocalizer` 會對資源字串中經過格式化的引數進行 HTML 編碼，但不會對資源字串本身進行 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="2c773-422">`IHtmlLocalizer` HTML encodes arguments that are formatted in the resource string, but doesn't HTML encode the resource string itself.</span></span> <span data-ttu-id="2c773-423">在下列醒目提示的範例中，只有 `name` 參數的值經過 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="2c773-423">In the sample highlighted below, only the value of `name` parameter is HTML encoded.</span></span>

[!code-csharp[](../fundamentals/localization/sample/Localization/Controllers/BookController.cs?highlight=3,5,20&start=1&end=24)]

<span data-ttu-id="2c773-424">**注意：** 一般來說，您只想將文字當地語系化，而不是 HTML。</span><span class="sxs-lookup"><span data-stu-id="2c773-424">**Note:** You generally want to only localize text and not HTML.</span></span>

<span data-ttu-id="2c773-425">您可以在最底層的[相依性插入](dependency-injection.md)中，將 `IStringLocalizerFactory` 移出：</span><span class="sxs-lookup"><span data-stu-id="2c773-425">At the lowest level, you can get `IStringLocalizerFactory` out of [Dependency Injection](dependency-injection.md):</span></span>

[!code-csharp[](localization/sample/Localization/Controllers/TestController.cs?start=9&end=26&highlight=7-13)]

<span data-ttu-id="2c773-426">上述程式碼會示範這兩個 Factory Create 方法。</span><span class="sxs-lookup"><span data-stu-id="2c773-426">The code above demonstrates each of the two factory create methods.</span></span>

<span data-ttu-id="2c773-427">您可以依據控制器或區域來分割當地語系化的字串，也可以只用一個容器。</span><span class="sxs-lookup"><span data-stu-id="2c773-427">You can partition your localized strings by controller, area, or have just one container.</span></span> <span data-ttu-id="2c773-428">在範例應用程式中，會針對共用資源使用名為 `SharedResource` 的虛擬類別。</span><span class="sxs-lookup"><span data-stu-id="2c773-428">In the sample app, a dummy class named `SharedResource` is used for shared resources.</span></span>

[!code-csharp[](localization/sample/Localization/Resources/SharedResource.cs)]

<span data-ttu-id="2c773-429">有些開發人員會使用 `Startup` 類別來包含全域或共用字串。</span><span class="sxs-lookup"><span data-stu-id="2c773-429">Some developers use the `Startup` class to contain global or shared strings.</span></span> <span data-ttu-id="2c773-430">下列範例會使用 `InfoController` 和 `SharedResource` 當地語系化工具：</span><span class="sxs-lookup"><span data-stu-id="2c773-430">In the sample below, the `InfoController` and the `SharedResource` localizers are used:</span></span>

[!code-csharp[](localization/sample/Localization/Controllers/InfoController.cs?range=9-26)]

## <a name="view-localization"></a><span data-ttu-id="2c773-431">檢視當地語系化</span><span class="sxs-lookup"><span data-stu-id="2c773-431">View localization</span></span>

<span data-ttu-id="2c773-432">`IViewLocalizer` 服務可提供[檢視](xref:mvc/views/overview)的當地語系化字串。</span><span class="sxs-lookup"><span data-stu-id="2c773-432">The `IViewLocalizer` service provides localized strings for a [view](xref:mvc/views/overview).</span></span> <span data-ttu-id="2c773-433">`ViewLocalizer` 類別會實作這個介面，並透過檢視檔案路徑來找出資源的位置。</span><span class="sxs-lookup"><span data-stu-id="2c773-433">The `ViewLocalizer` class implements this interface and finds the resource location from the view file path.</span></span> <span data-ttu-id="2c773-434">下列程式碼示範如何使用 `IViewLocalizer` 的預設實作：</span><span class="sxs-lookup"><span data-stu-id="2c773-434">The following code shows how to use the default implementation of `IViewLocalizer`:</span></span>

[!code-cshtml[](localization/sample/Localization/Views/Home/About.cshtml)]

<span data-ttu-id="2c773-435">`IViewLocalizer` 的預設實作會依據檢視的檔案名稱來找出資源檔。</span><span class="sxs-lookup"><span data-stu-id="2c773-435">The default implementation of `IViewLocalizer` finds the resource file based on the view's file name.</span></span> <span data-ttu-id="2c773-436">其中並沒有任何選項可以使用全域共用的資源檔。</span><span class="sxs-lookup"><span data-stu-id="2c773-436">There's no option to use a global shared resource file.</span></span> <span data-ttu-id="2c773-437">`ViewLocalizer`使用來執行當地語系化工具 `IHtmlLocalizer` ，因此 Razor 不會對當地語系化字串進行 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="2c773-437">`ViewLocalizer` implements the localizer using `IHtmlLocalizer`, so Razor doesn't HTML encode the localized string.</span></span> <span data-ttu-id="2c773-438">您可以參數化資源字串，`IViewLocalizer` 即會對參數 (而不是資源字串) 進行 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="2c773-438">You can parameterize resource strings and `IViewLocalizer` will HTML encode the parameters, but not the resource string.</span></span> <span data-ttu-id="2c773-439">請考慮下列 Razor 標記：</span><span class="sxs-lookup"><span data-stu-id="2c773-439">Consider the following Razor markup:</span></span>

```cshtml
@Localizer["<i>Hello</i> <b>{0}!</b>", UserManager.GetUserName(User)]
```

<span data-ttu-id="2c773-440">法文資源檔可能包含下列內容：</span><span class="sxs-lookup"><span data-stu-id="2c773-440">A French resource file could contain the following:</span></span>

| <span data-ttu-id="2c773-441">Key</span><span class="sxs-lookup"><span data-stu-id="2c773-441">Key</span></span> | <span data-ttu-id="2c773-442">值</span><span class="sxs-lookup"><span data-stu-id="2c773-442">Value</span></span> |
| ----- | ---
<span data-ttu-id="2c773-443">標題： author： description： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2c773-443">title: author: description: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="2c773-444">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2c773-444">'Blazor'</span></span>
- <span data-ttu-id="2c773-445">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2c773-445">'Identity'</span></span>
- <span data-ttu-id="2c773-446">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2c773-446">'Let's Encrypt'</span></span>
- <span data-ttu-id="2c773-447">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2c773-447">'Razor'</span></span>
- <span data-ttu-id="2c773-448">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2c773-448">'SignalR' uid:</span></span> 

<span data-ttu-id="2c773-449">--- | | `<i>Hello</i> <b>{0}!</b>` | `<i>Bonjour</i> <b>{0} !</b>` |</span><span class="sxs-lookup"><span data-stu-id="2c773-449">--- | | `<i>Hello</i> <b>{0}!</b>` | `<i>Bonjour</i> <b>{0} !</b>` |</span></span>

<span data-ttu-id="2c773-450">轉譯的檢視內容可能包含來自資源檔的 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="2c773-450">The rendered view would contain the HTML markup from the resource file.</span></span>

<span data-ttu-id="2c773-451">**注意：** 一般來說，您只想將文字當地語系化，而不是 HTML。</span><span class="sxs-lookup"><span data-stu-id="2c773-451">**Note:** You generally want to only localize text and not HTML.</span></span>

<span data-ttu-id="2c773-452">若要在檢視中使用共用的資源檔，請插入 `IHtmlLocalizer<T>`：</span><span class="sxs-lookup"><span data-stu-id="2c773-452">To use a shared resource file in a view, inject `IHtmlLocalizer<T>`:</span></span>

[!code-cshtml[](../fundamentals/localization/sample/Localization/Views/Test/About.cshtml?highlight=5,12)]

## <a name="dataannotations-localization"></a><span data-ttu-id="2c773-453">DataAnnotations 當地語系化</span><span class="sxs-lookup"><span data-stu-id="2c773-453">DataAnnotations localization</span></span>

<span data-ttu-id="2c773-454">DataAnnotations 錯誤訊息會使用 `IStringLocalizer<T>` 來當地語系化。</span><span class="sxs-lookup"><span data-stu-id="2c773-454">DataAnnotations error messages are localized with `IStringLocalizer<T>`.</span></span> <span data-ttu-id="2c773-455">使用 `ResourcesPath = "Resources"` 選項時，`RegisterViewModel` 中的錯誤訊息會儲存在下列路徑之一：</span><span class="sxs-lookup"><span data-stu-id="2c773-455">Using the option `ResourcesPath = "Resources"`, the error messages in `RegisterViewModel` can be stored in either of the following paths:</span></span>

* <span data-ttu-id="2c773-456">*Resources/Viewmodel. RegisterViewModel. fr .resx*</span><span class="sxs-lookup"><span data-stu-id="2c773-456">*Resources/ViewModels.Account.RegisterViewModel.fr.resx*</span></span>
* <span data-ttu-id="2c773-457">*Resources/ViewModels/Account/RegisterViewModel.fr.resx*</span><span class="sxs-lookup"><span data-stu-id="2c773-457">*Resources/ViewModels/Account/RegisterViewModel.fr.resx*</span></span>

[!code-csharp[](localization/sample/Localization/ViewModels/Account/RegisterViewModel.cs?start=9&end=26)]

<span data-ttu-id="2c773-458">在 ASP.NET Core MVC 1.1.0 和更高版本中，系統會將非驗證屬性當地語系化。</span><span class="sxs-lookup"><span data-stu-id="2c773-458">In ASP.NET Core MVC 1.1.0 and higher, non-validation attributes are localized.</span></span> <span data-ttu-id="2c773-459">ASP.NET Core MVC 1.0 **不會**查閱非驗證屬性的當地語系化字串。</span><span class="sxs-lookup"><span data-stu-id="2c773-459">ASP.NET Core MVC 1.0 does **not** look up localized strings for non-validation attributes.</span></span>

<a name="one-resource-string-multiple-classes"></a>

### <a name="using-one-resource-string-for-multiple-classes"></a><span data-ttu-id="2c773-460">針對多個類別使用同一個資源字串</span><span class="sxs-lookup"><span data-stu-id="2c773-460">Using one resource string for multiple classes</span></span>

<span data-ttu-id="2c773-461">下列程式碼會示範如何針對含有多個類別的驗證屬性使用同一個資源字串：</span><span class="sxs-lookup"><span data-stu-id="2c773-461">The following code shows how to use one resource string for validation attributes with multiple classes:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddDataAnnotationsLocalization(options => {
            options.DataAnnotationLocalizerProvider = (type, factory) =>
                factory.Create(typeof(SharedResource));
        });
}
```

<span data-ttu-id="2c773-462">在先前的程式碼中，`SharedResource` 是與儲存驗證訊息的 resx 對應的類別。</span><span class="sxs-lookup"><span data-stu-id="2c773-462">In the preceding code, `SharedResource` is the class corresponding to the resx where your validation messages are stored.</span></span> <span data-ttu-id="2c773-463">使用這個方法時，DataAnnotations 只會使用 `SharedResource`，而不是每個類別的資源。</span><span class="sxs-lookup"><span data-stu-id="2c773-463">With this approach, DataAnnotations will only use `SharedResource`, rather than the resource for each class.</span></span>

## <a name="provide-localized-resources-for-the-languages-and-cultures-you-support"></a><span data-ttu-id="2c773-464">針對您支援的語言和文化特性提供當地語系化資源</span><span class="sxs-lookup"><span data-stu-id="2c773-464">Provide localized resources for the languages and cultures you support</span></span>

### <a name="supportedcultures-and-supporteduicultures"></a><span data-ttu-id="2c773-465">SupportedCultures 和 SupportedUICultures</span><span class="sxs-lookup"><span data-stu-id="2c773-465">SupportedCultures and SupportedUICultures</span></span>

<span data-ttu-id="2c773-466">ASP.NET Core 可讓您指定 `SupportedCultures` 和 `SupportedUICultures` 這兩個文化特性值。</span><span class="sxs-lookup"><span data-stu-id="2c773-466">ASP.NET Core allows you to specify two culture values, `SupportedCultures` and `SupportedUICultures`.</span></span> <span data-ttu-id="2c773-467">`SupportedCultures` 的 [CultureInfo](/dotnet/api/system.globalization.cultureinfo) 物件可決定文化特性相依函式的結果，例如日期、時間、數字及貨幣格式。</span><span class="sxs-lookup"><span data-stu-id="2c773-467">The [CultureInfo](/dotnet/api/system.globalization.cultureinfo) object for `SupportedCultures` determines the results of culture-dependent functions, such as date, time, number, and currency formatting.</span></span> <span data-ttu-id="2c773-468">`SupportedCultures` 也可決定文字排列順序、大小寫慣例和字串比較。</span><span class="sxs-lookup"><span data-stu-id="2c773-468">`SupportedCultures` also determines the sorting order of text, casing conventions, and string comparisons.</span></span> <span data-ttu-id="2c773-469">如需伺服器如何取得文化特性的詳細資訊，請參閱 [CultureInfo.CurrentCulture](/dotnet/api/system.stringcomparer.currentculture#System_StringComparer_CurrentCulture)。</span><span class="sxs-lookup"><span data-stu-id="2c773-469">See [CultureInfo.CurrentCulture](/dotnet/api/system.stringcomparer.currentculture#System_StringComparer_CurrentCulture) for more info on how the server gets the Culture.</span></span> <span data-ttu-id="2c773-470">`SupportedUICultures`會決定[ResourceManager](/dotnet/api/system.resources.resourcemanager)會查閱哪些翻譯的字串（來自 *.resx*檔案）。</span><span class="sxs-lookup"><span data-stu-id="2c773-470">The `SupportedUICultures` determines which translated strings (from *.resx* files) are looked up by the [ResourceManager](/dotnet/api/system.resources.resourcemanager).</span></span> <span data-ttu-id="2c773-471">`ResourceManager` 僅會查閱 `CurrentUICulture` 所決定之文化特性特有的字串。</span><span class="sxs-lookup"><span data-stu-id="2c773-471">The `ResourceManager` simply looks up culture-specific strings that's determined by `CurrentUICulture`.</span></span> <span data-ttu-id="2c773-472">.NET 中的每個執行緒都有 `CurrentCulture` 和 `CurrentUICulture` 物件。</span><span class="sxs-lookup"><span data-stu-id="2c773-472">Every thread in .NET has `CurrentCulture` and `CurrentUICulture` objects.</span></span> <span data-ttu-id="2c773-473">ASP.NET Core 會在轉譯文化特性相依函式時檢查這些值。</span><span class="sxs-lookup"><span data-stu-id="2c773-473">ASP.NET Core inspects these values when rendering culture-dependent functions.</span></span> <span data-ttu-id="2c773-474">比方說，如果目前執行緒的文化特性設定為 "en-US" (英文 - 美國)，`DateTime.Now.ToLongDateString()` 會顯示 "Thursday, February 18, 2016"，但如果 `CurrentCulture` 設定為 "es-ES" (西班牙文 - 西班牙)，則輸出會是 "jueves, 18 de febrero de 2016"。</span><span class="sxs-lookup"><span data-stu-id="2c773-474">For example, if the current thread's culture is set to "en-US" (English, United States), `DateTime.Now.ToLongDateString()` displays "Thursday, February 18, 2016", but if `CurrentCulture` is set to "es-ES" (Spanish, Spain) the output will be "jueves, 18 de febrero de 2016".</span></span>

## <a name="resource-files"></a><span data-ttu-id="2c773-475">資源檔</span><span class="sxs-lookup"><span data-stu-id="2c773-475">Resource files</span></span>

<span data-ttu-id="2c773-476">資源檔是一種實用的機制，可讓您將可當地語系化的字串與代碼區隔開來。</span><span class="sxs-lookup"><span data-stu-id="2c773-476">A resource file is a useful mechanism for separating localizable strings from code.</span></span> <span data-ttu-id="2c773-477">非預設語言的翻譯字串會在 *.resx*資源檔中隔離。</span><span class="sxs-lookup"><span data-stu-id="2c773-477">Translated strings for the non-default language are isolated in *.resx* resource files.</span></span> <span data-ttu-id="2c773-478">例如，您可以建立名為 *Welcome.es.resx* 的西班牙文資源檔，以包含翻譯的字串。</span><span class="sxs-lookup"><span data-stu-id="2c773-478">For example, you might want to create Spanish resource file named *Welcome.es.resx* containing translated strings.</span></span> <span data-ttu-id="2c773-479">"es" 是西班牙文的語言代碼。</span><span class="sxs-lookup"><span data-stu-id="2c773-479">"es" is the language code for Spanish.</span></span> <span data-ttu-id="2c773-480">若要在 Visual Studio 中建立這個資源檔：</span><span class="sxs-lookup"><span data-stu-id="2c773-480">To create this resource file in Visual Studio:</span></span>

1. <span data-ttu-id="2c773-481">在方案總管\*\*\*\* 中，以滑鼠右鍵按一下要放置資源檔的資料夾 > [新增]**[新增項目]** > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="2c773-481">In **Solution Explorer**, right click on the folder which will contain the resource file > **Add** > **New Item**.</span></span>

    ![巢狀特色選單：方案總管會開啟 [資源] 的特色選單，](localization/_static/newi.png)

2. <span data-ttu-id="2c773-484">在 [Search installed templates] (搜尋已安裝的範本)\*\*\*\* 方塊中，輸入「資源」，並命名檔案。</span><span class="sxs-lookup"><span data-stu-id="2c773-484">In the **Search installed templates** box, enter "resource" and name the file.</span></span>

    ![[新增項目] 對話方塊](localization/_static/res.png)

3. <span data-ttu-id="2c773-486">在 [名稱]\*\*\*\* 資料行中輸入索引鍵值 (原生字串)，並在 [值]\*\*\*\* 資料行中輸入已翻譯的字串。</span><span class="sxs-lookup"><span data-stu-id="2c773-486">Enter the key value (native string) in the **Name** column and the translated string in the **Value** column.</span></span>

    ![Welcome.es.resx 檔案 (西班牙文的「歡迎使用」資源檔)，其中 [名稱] 資料行的文字為 Hello，而 [值] 資料行的文字為 Hola (Hello 的西班牙文)](localization/_static/hola.png)

    <span data-ttu-id="2c773-488">Visual Studio 會顯示 *Welcome.es.resx* 檔案。</span><span class="sxs-lookup"><span data-stu-id="2c773-488">Visual Studio shows the *Welcome.es.resx* file.</span></span>

    ![方案總管，其中顯示「歡迎使用」的西班牙文 (es) 資源檔](localization/_static/se.png)

## <a name="resource-file-naming"></a><span data-ttu-id="2c773-490">資源檔命名</span><span class="sxs-lookup"><span data-stu-id="2c773-490">Resource file naming</span></span>

<span data-ttu-id="2c773-491">資源的命名方式是以其類別的完整類型名稱去掉組件名稱而得。</span><span class="sxs-lookup"><span data-stu-id="2c773-491">Resources are named for the full type name of their class minus the assembly name.</span></span> <span data-ttu-id="2c773-492">例如，假設專案中的法文資源是 `LocalizationWebsite.Web.Startup` 類別、主要組件為 `LocalizationWebsite.Web.dll`，就會命名為 *Startup.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="2c773-492">For example, a French resource in a project whose main assembly is `LocalizationWebsite.Web.dll` for the class `LocalizationWebsite.Web.Startup` would be named *Startup.fr.resx*.</span></span> <span data-ttu-id="2c773-493">若是 `LocalizationWebsite.Web.Controllers.HomeController` 類別的資源，則應命名為 *Controllers.HomeController.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="2c773-493">A resource for the class `LocalizationWebsite.Web.Controllers.HomeController` would be named *Controllers.HomeController.fr.resx*.</span></span> <span data-ttu-id="2c773-494">如果目標類別的命名空間和組件名稱不相同，則需要使用完整類型名稱。</span><span class="sxs-lookup"><span data-stu-id="2c773-494">If your targeted class's namespace isn't the same as the assembly name you will need the full type name.</span></span> <span data-ttu-id="2c773-495">比方說，範例專案中 `ExtraNamespace.Tools` 類型的資源會命名為 *ExtraNamespace.Tools.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="2c773-495">For example, in the sample project a resource for the type `ExtraNamespace.Tools` would be named *ExtraNamespace.Tools.fr.resx*.</span></span>

<span data-ttu-id="2c773-496">在範例專案中，`ConfigureServices` 方法會將 `ResourcesPath` 設為 "Resources"，因此首頁控制器的法文資源檔專案相對路徑即為 *Resources/Controllers.HomeController.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="2c773-496">In the sample project, the `ConfigureServices` method sets the `ResourcesPath` to "Resources", so the project relative path for the home controller's French resource file is *Resources/Controllers.HomeController.fr.resx*.</span></span> <span data-ttu-id="2c773-497">或者，您可以使用資料夾來收集資源檔。</span><span class="sxs-lookup"><span data-stu-id="2c773-497">Alternatively, you can use folders to organize resource files.</span></span> <span data-ttu-id="2c773-498">若是首頁控制器，路徑就是 *Resources/Controllers/HomeController.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="2c773-498">For the home controller, the path would be *Resources/Controllers/HomeController.fr.resx*.</span></span> <span data-ttu-id="2c773-499">如果您不使用 `ResourcesPath` 選項，*.resx* 檔案即會放置在專案的基底目錄中。</span><span class="sxs-lookup"><span data-stu-id="2c773-499">If you don't use the `ResourcesPath` option, the *.resx* file would go in the project base directory.</span></span> <span data-ttu-id="2c773-500">`HomeController` 的資源檔會命名為 *Controllers.HomeController.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="2c773-500">The resource file for `HomeController` would be named *Controllers.HomeController.fr.resx*.</span></span> <span data-ttu-id="2c773-501">您可依據自己的資源檔收集方式，來選擇要使用點或路徑的命名慣例。</span><span class="sxs-lookup"><span data-stu-id="2c773-501">The choice of using the dot or path naming convention depends on how you want to organize your resource files.</span></span>

| <span data-ttu-id="2c773-502">資源名稱</span><span class="sxs-lookup"><span data-stu-id="2c773-502">Resource name</span></span> | <span data-ttu-id="2c773-503">點或路徑命名</span><span class="sxs-lookup"><span data-stu-id="2c773-503">Dot or path naming</span></span> |
| ---
<span data-ttu-id="2c773-504">標題： author： description： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2c773-504">title: author: description: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="2c773-505">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2c773-505">'Blazor'</span></span>
- <span data-ttu-id="2c773-506">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2c773-506">'Identity'</span></span>
- <span data-ttu-id="2c773-507">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2c773-507">'Let's Encrypt'</span></span>
- <span data-ttu-id="2c773-508">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2c773-508">'Razor'</span></span>
- <span data-ttu-id="2c773-509">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2c773-509">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2c773-510">標題： author： description： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2c773-510">title: author: description: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="2c773-511">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2c773-511">'Blazor'</span></span>
- <span data-ttu-id="2c773-512">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2c773-512">'Identity'</span></span>
- <span data-ttu-id="2c773-513">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2c773-513">'Let's Encrypt'</span></span>
- <span data-ttu-id="2c773-514">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2c773-514">'Razor'</span></span>
- <span data-ttu-id="2c773-515">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2c773-515">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2c773-516">標題： author： description： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2c773-516">title: author: description: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="2c773-517">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2c773-517">'Blazor'</span></span>
- <span data-ttu-id="2c773-518">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2c773-518">'Identity'</span></span>
- <span data-ttu-id="2c773-519">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2c773-519">'Let's Encrypt'</span></span>
- <span data-ttu-id="2c773-520">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2c773-520">'Razor'</span></span>
- <span data-ttu-id="2c773-521">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2c773-521">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2c773-522">標題： author： description： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2c773-522">title: author: description: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="2c773-523">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2c773-523">'Blazor'</span></span>
- <span data-ttu-id="2c773-524">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2c773-524">'Identity'</span></span>
- <span data-ttu-id="2c773-525">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2c773-525">'Let's Encrypt'</span></span>
- <span data-ttu-id="2c773-526">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2c773-526">'Razor'</span></span>
- <span data-ttu-id="2c773-527">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2c773-527">'SignalR' uid:</span></span> 

<span data-ttu-id="2c773-528">------   |---標題： author： description： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2c773-528">------   | --- title: author: description: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="2c773-529">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2c773-529">'Blazor'</span></span>
- <span data-ttu-id="2c773-530">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2c773-530">'Identity'</span></span>
- <span data-ttu-id="2c773-531">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2c773-531">'Let's Encrypt'</span></span>
- <span data-ttu-id="2c773-532">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2c773-532">'Razor'</span></span>
- <span data-ttu-id="2c773-533">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2c773-533">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2c773-534">標題： author： description： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2c773-534">title: author: description: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="2c773-535">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2c773-535">'Blazor'</span></span>
- <span data-ttu-id="2c773-536">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2c773-536">'Identity'</span></span>
- <span data-ttu-id="2c773-537">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2c773-537">'Let's Encrypt'</span></span>
- <span data-ttu-id="2c773-538">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2c773-538">'Razor'</span></span>
- <span data-ttu-id="2c773-539">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2c773-539">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2c773-540">標題： author： description： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2c773-540">title: author: description: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="2c773-541">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2c773-541">'Blazor'</span></span>
- <span data-ttu-id="2c773-542">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2c773-542">'Identity'</span></span>
- <span data-ttu-id="2c773-543">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2c773-543">'Let's Encrypt'</span></span>
- <span data-ttu-id="2c773-544">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2c773-544">'Razor'</span></span>
- <span data-ttu-id="2c773-545">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2c773-545">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2c773-546">標題： author： description： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2c773-546">title: author: description: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="2c773-547">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2c773-547">'Blazor'</span></span>
- <span data-ttu-id="2c773-548">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2c773-548">'Identity'</span></span>
- <span data-ttu-id="2c773-549">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2c773-549">'Let's Encrypt'</span></span>
- <span data-ttu-id="2c773-550">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2c773-550">'Razor'</span></span>
- <span data-ttu-id="2c773-551">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2c773-551">'SignalR' uid:</span></span> 

<span data-ttu-id="2c773-552">------- | |Resources/controller. HomeController. .resx |點 | |Resources/controller/HomeController. .resx |路徑 | |   |    |</span><span class="sxs-lookup"><span data-stu-id="2c773-552">------- | | Resources/Controllers.HomeController.fr.resx | Dot  | | Resources/Controllers/HomeController.fr.resx  | Path | |    |     |</span></span>

<span data-ttu-id="2c773-553">在 views 中使用的資源檔會 `@inject IViewLocalizer` Razor 遵循類似的模式。</span><span class="sxs-lookup"><span data-stu-id="2c773-553">Resource files using `@inject IViewLocalizer` in Razor views follow a similar pattern.</span></span> <span data-ttu-id="2c773-554">您可以使用點命名或路徑命名方式，來命名檢視的資源檔。</span><span class="sxs-lookup"><span data-stu-id="2c773-554">The resource file for a view can be named using either dot naming or path naming.</span></span> Razor<span data-ttu-id="2c773-555">查看資源檔模擬其相關聯之視圖檔案的路徑。</span><span class="sxs-lookup"><span data-stu-id="2c773-555"> view resource files mimic the path of their associated view file.</span></span> <span data-ttu-id="2c773-556">假設我們將 `ResourcesPath` 設為 "Resources"，與 *Views/Home/About.cshtml* 檢視建立關聯的法文資源檔可為下列其一：</span><span class="sxs-lookup"><span data-stu-id="2c773-556">Assuming we set the `ResourcesPath` to "Resources", the French resource file associated with the *Views/Home/About.cshtml* view could be either of the following:</span></span>

* <span data-ttu-id="2c773-557">Resources/Views/Home/About.fr.resx</span><span class="sxs-lookup"><span data-stu-id="2c773-557">Resources/Views/Home/About.fr.resx</span></span>

* <span data-ttu-id="2c773-558">Resources/Views.Home.About.fr.resx</span><span class="sxs-lookup"><span data-stu-id="2c773-558">Resources/Views.Home.About.fr.resx</span></span>

<span data-ttu-id="2c773-559">如果您不使用 `ResourcesPath` 選項，則檢視的 *.resx* 檔案會與檢視位於相同資料夾中。</span><span class="sxs-lookup"><span data-stu-id="2c773-559">If you don't use the `ResourcesPath` option, the *.resx* file for a view would be located in the same folder as the view.</span></span>

### <a name="rootnamespaceattribute"></a><span data-ttu-id="2c773-560">RootNamespaceAttribute</span><span class="sxs-lookup"><span data-stu-id="2c773-560">RootNamespaceAttribute</span></span> 

<span data-ttu-id="2c773-561">[RootNamespace](/dotnet/api/microsoft.extensions.localization.rootnamespaceattribute?view=aspnetcore-2.1) 屬性會在組件的根命名空間與組件名稱不同時，提供組件的根命名空間。</span><span class="sxs-lookup"><span data-stu-id="2c773-561">The [RootNamespace](/dotnet/api/microsoft.extensions.localization.rootnamespaceattribute?view=aspnetcore-2.1) attribute provides the root namespace of an assembly when the root namespace of an assembly is different than the assembly name.</span></span> 

> [!WARNING]
> <span data-ttu-id="2c773-562">當專案的名稱不是有效的 .NET 識別碼時，就可能發生這種情況。</span><span class="sxs-lookup"><span data-stu-id="2c773-562">This can occur when a project's name is not a valid .NET identifier.</span></span> <span data-ttu-id="2c773-563">例如， `my-project-name.csproj` 會使用根命名空間 `my_project_name` ，以及 `my-project-name` 導致此錯誤的元件名稱。</span><span class="sxs-lookup"><span data-stu-id="2c773-563">For instance `my-project-name.csproj` will use the root namespace `my_project_name` and the assembly name `my-project-name` leading to this error.</span></span> 

<span data-ttu-id="2c773-564">如果組件的根命名空間與組件名稱不同：</span><span class="sxs-lookup"><span data-stu-id="2c773-564">If the root namespace of an assembly is different than the assembly name:</span></span>

* <span data-ttu-id="2c773-565">根據預設，當地語系化無法運作。</span><span class="sxs-lookup"><span data-stu-id="2c773-565">Localization does not work by default.</span></span>
* <span data-ttu-id="2c773-566">在組件中搜尋資源的方式造成當地語系化失敗。</span><span class="sxs-lookup"><span data-stu-id="2c773-566">Localization fails due to the way resources are searched for within the assembly.</span></span> <span data-ttu-id="2c773-567">`RootNamespace` 是建置時間值，無法用於執行處理序。</span><span class="sxs-lookup"><span data-stu-id="2c773-567">`RootNamespace` is a build-time value which is not available to the executing process.</span></span> 

<span data-ttu-id="2c773-568">如果 `RootNamespace` 與 `AssemblyName` 不同，請將下列內容納入 *AssemblyInfo.cs* (參數值取代為實際值)：</span><span class="sxs-lookup"><span data-stu-id="2c773-568">If the `RootNamespace` is different from the `AssemblyName`, include the following in *AssemblyInfo.cs* (with parameter values replaced with the actual values):</span></span>

```csharp
using System.Reflection;
using Microsoft.Extensions.Localization;

[assembly: ResourceLocation("Resource Folder Name")]
[assembly: RootNamespace("App Root Namespace")]
```

<span data-ttu-id="2c773-569">上述程式碼可成功解析 resx 檔案。</span><span class="sxs-lookup"><span data-stu-id="2c773-569">The preceding code enables the successful resolution of resx files.</span></span>

## <a name="culture-fallback-behavior"></a><span data-ttu-id="2c773-570">文化特性後援行為</span><span class="sxs-lookup"><span data-stu-id="2c773-570">Culture fallback behavior</span></span>

<span data-ttu-id="2c773-571">搜尋資源時，當地語系化會使用「文化特性後援」。</span><span class="sxs-lookup"><span data-stu-id="2c773-571">When searching for a resource, localization engages in "culture fallback".</span></span> <span data-ttu-id="2c773-572">從所要求的文化特性開始，如果找不到，就會還原成該文化特性的父文化特性。</span><span class="sxs-lookup"><span data-stu-id="2c773-572">Starting from the requested culture, if not found, it reverts to the parent culture of that culture.</span></span> <span data-ttu-id="2c773-573">另外，[CultureInfo.Parent](/dotnet/api/system.globalization.cultureinfo.parent) 屬性代表父文化特性。</span><span class="sxs-lookup"><span data-stu-id="2c773-573">As an aside, the [CultureInfo.Parent](/dotnet/api/system.globalization.cultureinfo.parent) property represents the parent culture.</span></span> <span data-ttu-id="2c773-574">這通常 (但並非一定) 表示從 ISO 中移除國家意符 (Signifier)。</span><span class="sxs-lookup"><span data-stu-id="2c773-574">This usually (but not always) means removing the national signifier from the ISO.</span></span> <span data-ttu-id="2c773-575">例如，墨西哥的西班牙文方言是 "es-MX"。</span><span class="sxs-lookup"><span data-stu-id="2c773-575">For example, the dialect of Spanish spoken in Mexico is "es-MX".</span></span> <span data-ttu-id="2c773-576">它具有父系 "es" &mdash; 西班牙文不是任何國家/地區的特定項目。</span><span class="sxs-lookup"><span data-stu-id="2c773-576">It has the parent "es"&mdash;Spanish non-specific to any country.</span></span>

<span data-ttu-id="2c773-577">假設您的網站收到使用文化特性 "fr-CA" 之 "Welcome" 資源的要求。</span><span class="sxs-lookup"><span data-stu-id="2c773-577">Imagine your site receives a request for a "Welcome" resource using culture "fr-CA".</span></span> <span data-ttu-id="2c773-578">當地語系化系統會依照順序尋找下列資源，並選取第一個相符項目：</span><span class="sxs-lookup"><span data-stu-id="2c773-578">The localization system looks for the following resources, in order, and selects the first match:</span></span>

* <span data-ttu-id="2c773-579">*Welcome.fr-CA.resx*</span><span class="sxs-lookup"><span data-stu-id="2c773-579">*Welcome.fr-CA.resx*</span></span>
* <span data-ttu-id="2c773-580">*Welcome.fr.resx*</span><span class="sxs-lookup"><span data-stu-id="2c773-580">*Welcome.fr.resx*</span></span>
* <span data-ttu-id="2c773-581">*Welcome.resx* (如果 `NeutralResourcesLanguage` 是 "fr-CA")</span><span class="sxs-lookup"><span data-stu-id="2c773-581">*Welcome.resx* (if the `NeutralResourcesLanguage` is "fr-CA")</span></span>

<span data-ttu-id="2c773-582">舉例來說，如果您移除 ".fr" 文化特性指示項，並將文化特性設定為法文，則系統會讀取預設資源檔，並將字串當地語系化。</span><span class="sxs-lookup"><span data-stu-id="2c773-582">As an example, if you remove the ".fr" culture designator and you have the culture set to French, the default resource file is read and strings are localized.</span></span> <span data-ttu-id="2c773-583">當沒有任何項目符合您要求的文化特性時，資源管理員即會指定預設資源或後援資源。</span><span class="sxs-lookup"><span data-stu-id="2c773-583">The Resource manager designates a default or fallback resource for when nothing meets your requested culture.</span></span> <span data-ttu-id="2c773-584">如果您只想在要求的文化特性缺少資源時傳回索引鍵，就不能使用預設資源檔。</span><span class="sxs-lookup"><span data-stu-id="2c773-584">If you want to just return the key when missing a resource for the requested culture you must not have a default resource file.</span></span>

### <a name="generate-resource-files-with-visual-studio"></a><span data-ttu-id="2c773-585">使用 Visual Studio 產生資源檔</span><span class="sxs-lookup"><span data-stu-id="2c773-585">Generate resource files with Visual Studio</span></span>

<span data-ttu-id="2c773-586">如果您在 Visual Studio 中建立資源檔，但檔案名稱中不含文化特性 (例如 *Welcome.resx*)，則 Visual Studio 會針對每個字串的屬性建立 C# 類別。</span><span class="sxs-lookup"><span data-stu-id="2c773-586">If you create a resource file in Visual Studio without a culture in the file name (for example, *Welcome.resx*), Visual Studio will create a C# class with a property for each string.</span></span> <span data-ttu-id="2c773-587">但這通常不是您使用 ASP.NET Core 的初衷。</span><span class="sxs-lookup"><span data-stu-id="2c773-587">That's usually not what you want with ASP.NET Core.</span></span> <span data-ttu-id="2c773-588">一般來說，您不會有預設的 *.resx* 資源檔 (不含文化特性名稱的 *.resx* 檔案)。</span><span class="sxs-lookup"><span data-stu-id="2c773-588">You typically don't have a default *.resx* resource file (a *.resx* file without the culture name).</span></span> <span data-ttu-id="2c773-589">因此，建議您建立含有文化特性名稱的 *.resx* 檔案 (例如 *Welcome.fr.resx*)。</span><span class="sxs-lookup"><span data-stu-id="2c773-589">We suggest you create the *.resx* file with a culture name (for example *Welcome.fr.resx*).</span></span> <span data-ttu-id="2c773-590">當您建立含有文化特性名稱的 *.resx* 檔案時，Visual Studio 就不會產生類別檔案。</span><span class="sxs-lookup"><span data-stu-id="2c773-590">When you create a *.resx* file with a culture name, Visual Studio won't generate the class file.</span></span>

### <a name="add-other-cultures"></a><span data-ttu-id="2c773-591">新增其他文化特性</span><span class="sxs-lookup"><span data-stu-id="2c773-591">Add other cultures</span></span>

<span data-ttu-id="2c773-592">每種語言和文化特性的組合 (非預設語言) 都需要唯一的資源檔。</span><span class="sxs-lookup"><span data-stu-id="2c773-592">Each language and culture combination (other than the default language) requires a unique resource file.</span></span> <span data-ttu-id="2c773-593">若要建立不同文化特性和地區設定的資源檔，您可以建立新的資源檔並將 ISO 語言代碼作為檔名的一部分 (例如 **en-us**、**fr-ca** 和 **en-gb**)。</span><span class="sxs-lookup"><span data-stu-id="2c773-593">You create resource files for different cultures and locales by creating new resource files in which the ISO language codes are part of the file name (for example, **en-us**, **fr-ca**, and **en-gb**).</span></span> <span data-ttu-id="2c773-594">您應將 ISO 代碼置於檔案名稱和 *.resx* 副檔名之間，例如 *Welcome.es-MX.resx* (西班牙文/墨西哥)。</span><span class="sxs-lookup"><span data-stu-id="2c773-594">These ISO codes are placed between the file name and the *.resx* file extension, as in *Welcome.es-MX.resx* (Spanish/Mexico).</span></span>

## <a name="implement-a-strategy-to-select-the-languageculture-for-each-request"></a><span data-ttu-id="2c773-595">實作可依據每項要求選取語言/文化特性的策略</span><span class="sxs-lookup"><span data-stu-id="2c773-595">Implement a strategy to select the language/culture for each request</span></span>

### <a name="configure-localization"></a><span data-ttu-id="2c773-596">設定當地語系化</span><span class="sxs-lookup"><span data-stu-id="2c773-596">Configure localization</span></span>

<span data-ttu-id="2c773-597">您可以在 `Startup.ConfigureServices` 方法中設定當地語系化：</span><span class="sxs-lookup"><span data-stu-id="2c773-597">Localization is configured in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](localization/sample/Localization/Startup.cs?name=snippet1)]

* <span data-ttu-id="2c773-598">`AddLocalization` 可將當地語系化服務新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="2c773-598">`AddLocalization` Adds the localization services to the services container.</span></span> <span data-ttu-id="2c773-599">上方的程式碼也會將資源路徑設為 "Resources"。</span><span class="sxs-lookup"><span data-stu-id="2c773-599">The code above also sets the resources path to "Resources".</span></span>

* <span data-ttu-id="2c773-600">`AddViewLocalization` 可支援當地語系化的檢視檔案。</span><span class="sxs-lookup"><span data-stu-id="2c773-600">`AddViewLocalization` Adds support for localized view files.</span></span> <span data-ttu-id="2c773-601">在此範例中，檢視的當地語系化會以檢視檔案的後置詞為依據。</span><span class="sxs-lookup"><span data-stu-id="2c773-601">In this sample view localization is based on the view file suffix.</span></span> <span data-ttu-id="2c773-602">例如 *Index.fr.cshtml* 檔案中的 "fr"。</span><span class="sxs-lookup"><span data-stu-id="2c773-602">For example "fr" in the *Index.fr.cshtml* file.</span></span>

* <span data-ttu-id="2c773-603">`AddDataAnnotationsLocalization` 可支援透過 `IStringLocalizer` 抽象概念而來的當地語系化 `DataAnnotations` 驗證訊息。</span><span class="sxs-lookup"><span data-stu-id="2c773-603">`AddDataAnnotationsLocalization` Adds support for localized `DataAnnotations` validation messages through `IStringLocalizer` abstractions.</span></span>

### <a name="localization-middleware"></a><span data-ttu-id="2c773-604">當地語系化中介軟體</span><span class="sxs-lookup"><span data-stu-id="2c773-604">Localization middleware</span></span>

<span data-ttu-id="2c773-605">您可以在當地語系化[中介軟體](xref:fundamentals/middleware/index)中，設定要求目前的文化特性。</span><span class="sxs-lookup"><span data-stu-id="2c773-605">The current culture on a request is set in the localization [Middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="2c773-606">已在 `Startup.Configure` 方法中啟用當地語系化中介軟體。</span><span class="sxs-lookup"><span data-stu-id="2c773-606">The localization middleware is enabled in the `Startup.Configure` method.</span></span> <span data-ttu-id="2c773-607">您必須在可能檢查要求文化特性的任何中介軟體之前，設定當地語系化中介軟體 (例如 `app.UseMvcWithDefaultRoute()`)。</span><span class="sxs-lookup"><span data-stu-id="2c773-607">The localization middleware must be configured before any middleware which might check the request culture (for example, `app.UseMvcWithDefaultRoute()`).</span></span>

[!code-csharp[](localization/sample/Localization/Startup.cs?name=snippet2)]
[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

<span data-ttu-id="2c773-608">`UseRequestLocalization` 會初始化 `RequestLocalizationOptions` 物件。</span><span class="sxs-lookup"><span data-stu-id="2c773-608">`UseRequestLocalization` initializes a `RequestLocalizationOptions` object.</span></span> <span data-ttu-id="2c773-609">在每次要求時，系統會列舉 `RequestLocalizationOptions` 中的 `RequestCultureProvider` 清單，並使用能成功判斷要求的文化特性的第一個提供者。</span><span class="sxs-lookup"><span data-stu-id="2c773-609">On every request the list of `RequestCultureProvider` in the `RequestLocalizationOptions` is enumerated and the first provider that can successfully determine the request culture is used.</span></span> <span data-ttu-id="2c773-610">預設的提供者是來自 `RequestLocalizationOptions` 類別：</span><span class="sxs-lookup"><span data-stu-id="2c773-610">The default providers come from the `RequestLocalizationOptions` class:</span></span>

1. `QueryStringRequestCultureProvider`
2. `CookieRequestCultureProvider`
3. `AcceptLanguageHeaderRequestCultureProvider`

<span data-ttu-id="2c773-611">預設清單會由針對性高到低來排列。</span><span class="sxs-lookup"><span data-stu-id="2c773-611">The default list goes from most specific to least specific.</span></span> <span data-ttu-id="2c773-612">在本文稍後，我們會說明如何變更順序，甚至新增自訂的文化特性提供者。</span><span class="sxs-lookup"><span data-stu-id="2c773-612">Later in the article we'll see how you can change the order and even add a custom culture provider.</span></span> <span data-ttu-id="2c773-613">如果沒有任何提供者可以判斷要求的文化特性，即會使用 `DefaultRequestCulture`。</span><span class="sxs-lookup"><span data-stu-id="2c773-613">If none of the providers can determine the request culture, the `DefaultRequestCulture` is used.</span></span>

### <a name="querystringrequestcultureprovider"></a><span data-ttu-id="2c773-614">QueryStringRequestCultureProvider</span><span class="sxs-lookup"><span data-stu-id="2c773-614">QueryStringRequestCultureProvider</span></span>

<span data-ttu-id="2c773-615">有些應用程式會使用查詢字串來設定[文化特性和 UI 文化特性](https://msdn.microsoft.com/library/system.globalization.cultureinfo.aspx)。</span><span class="sxs-lookup"><span data-stu-id="2c773-615">Some apps will use a query string to set the [culture and UI culture](https://msdn.microsoft.com/library/system.globalization.cultureinfo.aspx).</span></span> <span data-ttu-id="2c773-616">若是使用 Cookie 或 Accept-Language 標頭方法的應用程式，您可以將查詢字串新增至 URL 以偵錯和測試程式碼。</span><span class="sxs-lookup"><span data-stu-id="2c773-616">For apps that use the cookie or Accept-Language header approach, adding a query string to the URL is useful for debugging and testing code.</span></span> <span data-ttu-id="2c773-617">系統預設會將 `QueryStringRequestCultureProvider` 登錄為 `RequestCultureProvider` 清單中的第一個當地語系化提供者。</span><span class="sxs-lookup"><span data-stu-id="2c773-617">By default, the `QueryStringRequestCultureProvider` is registered as the first localization provider in the `RequestCultureProvider` list.</span></span> <span data-ttu-id="2c773-618">您應傳遞查詢字串參數 `culture` 和 `ui-culture`。</span><span class="sxs-lookup"><span data-stu-id="2c773-618">You pass the query string parameters `culture` and `ui-culture`.</span></span> <span data-ttu-id="2c773-619">下列範例會設定西班牙文/墨西哥的特定文化特性 (語言和地區)：</span><span class="sxs-lookup"><span data-stu-id="2c773-619">The following example sets the specific culture (language and region) to Spanish/Mexico:</span></span>

   `http://localhost:5000/?culture=es-MX&ui-culture=es-MX`

<span data-ttu-id="2c773-620">如果您只傳入兩者其一 (`culture` 或 `ui-culture`)，查詢字串提供者就會使用您傳入的項目來設定這兩個值。</span><span class="sxs-lookup"><span data-stu-id="2c773-620">If you only pass in one of the two (`culture` or `ui-culture`), the query string provider will set both values using the one you passed in.</span></span> <span data-ttu-id="2c773-621">例如，若只設定文化特性，即會同時設定 `Culture` 和 `UICulture`：</span><span class="sxs-lookup"><span data-stu-id="2c773-621">For example, setting just the culture will set both the `Culture` and the `UICulture`:</span></span>

   `http://localhost:5000/?culture=es-MX`

### <a name="cookierequestcultureprovider"></a><span data-ttu-id="2c773-622">CookieRequestCultureProvider</span><span class="sxs-lookup"><span data-stu-id="2c773-622">CookieRequestCultureProvider</span></span>

<span data-ttu-id="2c773-623">生產環境應用程式通常會提供一個機制，來設定 ASP.NET Core 文化特性 Cookie 的文化特性。</span><span class="sxs-lookup"><span data-stu-id="2c773-623">Production apps will often provide a mechanism to set the culture with the ASP.NET Core culture cookie.</span></span> <span data-ttu-id="2c773-624">若要建立 Cookie，請使用 `MakeCookieValue` 方法。</span><span class="sxs-lookup"><span data-stu-id="2c773-624">Use the `MakeCookieValue` method to create a cookie.</span></span>

<span data-ttu-id="2c773-625">會傳回 `CookieRequestCultureProvider` `DefaultCookieName` 用來追蹤使用者慣用文化特性資訊的預設 cookie 名稱。</span><span class="sxs-lookup"><span data-stu-id="2c773-625">The `CookieRequestCultureProvider` `DefaultCookieName` returns the default cookie name used to track the user's preferred culture information.</span></span> <span data-ttu-id="2c773-626">預設 Cookie 名稱為 `.AspNetCore.Culture`。</span><span class="sxs-lookup"><span data-stu-id="2c773-626">The default cookie name is `.AspNetCore.Culture`.</span></span>

<span data-ttu-id="2c773-627">Cookie 格式為 `c=%LANGCODE%|uic=%LANGCODE%`，其中 `c` 是 `Culture` 而 `uic` 是 `UICulture`，例如：</span><span class="sxs-lookup"><span data-stu-id="2c773-627">The cookie format is `c=%LANGCODE%|uic=%LANGCODE%`, where `c` is `Culture` and `uic` is `UICulture`, for example:</span></span>

    c=en-UK|uic=en-US

<span data-ttu-id="2c773-628">如果您只指定文化特性資訊和 UI 文化特性其一，系統就會將您所指定的文化特性用於文化特性資訊和 UI 文化特性。</span><span class="sxs-lookup"><span data-stu-id="2c773-628">If you only specify one of culture info and UI culture, the specified culture will be used for both culture info and UI culture.</span></span>

### <a name="the-accept-language-http-header"></a><span data-ttu-id="2c773-629">Accept-Language HTTP 標頭</span><span class="sxs-lookup"><span data-stu-id="2c773-629">The Accept-Language HTTP header</span></span>

<span data-ttu-id="2c773-630">您可在大多數的瀏覽器中設定 [Accept-Language 標頭](https://www.w3.org/International/questions/qa-accept-lang-locales)，其最初的設計目的是用來指定使用者的語言。</span><span class="sxs-lookup"><span data-stu-id="2c773-630">The [Accept-Language header](https://www.w3.org/International/questions/qa-accept-lang-locales) is settable in most browsers and was originally intended to specify the user's language.</span></span> <span data-ttu-id="2c773-631">這項設定可指出瀏覽器已設好要傳送哪些項目，或已從基礎作業系統繼承哪些項目。</span><span class="sxs-lookup"><span data-stu-id="2c773-631">This setting indicates what the browser has been set to send or has inherited from the underlying operating system.</span></span> <span data-ttu-id="2c773-632">透過瀏覽器要求的 Accept-Language HTTP 標頭來偵測使用者的慣用語言，並非萬無一失 (請參閱 [Setting language preferences in a browser](https://www.w3.org/International/questions/qa-lang-priorities.en.php) (在瀏覽器中設定語言喜好設定)。</span><span class="sxs-lookup"><span data-stu-id="2c773-632">The Accept-Language HTTP header from a browser request isn't an infallible way to detect the user's preferred language (see [Setting language preferences in a browser](https://www.w3.org/International/questions/qa-lang-priorities.en.php)).</span></span> <span data-ttu-id="2c773-633">生產環境應用程式應該包含可讓使用者自訂文化特性的選擇方式。</span><span class="sxs-lookup"><span data-stu-id="2c773-633">A production app should include a way for a user to customize their choice of culture.</span></span>

### <a name="set-the-accept-language-http-header-in-ie"></a><span data-ttu-id="2c773-634">在 IE 中設定 Accept-Language HTTP 標頭</span><span class="sxs-lookup"><span data-stu-id="2c773-634">Set the Accept-Language HTTP header in IE</span></span>

1. <span data-ttu-id="2c773-635">從齒輪圖示，點選 [網際網路選項]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="2c773-635">From the gear icon, tap **Internet Options**.</span></span>

2. <span data-ttu-id="2c773-636">點選 [語言]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="2c773-636">Tap **Languages**.</span></span>

    ![網際網路選項](localization/_static/lang.png)

3. <span data-ttu-id="2c773-638">點選 [設定語言喜好設定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="2c773-638">Tap **Set Language Preferences**.</span></span>

4. <span data-ttu-id="2c773-639">點選 [新增語言]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="2c773-639">Tap **Add a language**.</span></span>

5. <span data-ttu-id="2c773-640">新增語言。</span><span class="sxs-lookup"><span data-stu-id="2c773-640">Add the language.</span></span>

6. <span data-ttu-id="2c773-641">點選語言，然後點選 [上移]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="2c773-641">Tap the language, then tap **Move Up**.</span></span>

### <a name="use-a-custom-provider"></a><span data-ttu-id="2c773-642">使用自訂提供者</span><span class="sxs-lookup"><span data-stu-id="2c773-642">Use a custom provider</span></span>

<span data-ttu-id="2c773-643">假設您想要讓客戶將他們的語言和文化特性儲存在您的資料庫中。</span><span class="sxs-lookup"><span data-stu-id="2c773-643">Suppose you want to let your customers store their language and culture in your databases.</span></span> <span data-ttu-id="2c773-644">您可以撰寫提供者，以供使用者查詢這些值。</span><span class="sxs-lookup"><span data-stu-id="2c773-644">You could write a provider to look up these values for the user.</span></span> <span data-ttu-id="2c773-645">下列程式碼會示範如何新增自訂提供者：</span><span class="sxs-lookup"><span data-stu-id="2c773-645">The following code shows how to add a custom provider:</span></span>

```csharp
private const string enUSCulture = "en-US";

services.Configure<RequestLocalizationOptions>(options =>
{
    var supportedCultures = new[]
    {
        new CultureInfo(enUSCulture),
        new CultureInfo("fr")
    };

    options.DefaultRequestCulture = new RequestCulture(culture: enUSCulture, uiCulture: enUSCulture);
    options.SupportedCultures = supportedCultures;
    options.SupportedUICultures = supportedCultures;

    options.RequestCultureProviders.Insert(0, new CustomRequestCultureProvider(async context =>
    {
        // My custom request culture logic
        return new ProviderCultureResult("en");
    }));
});
```

<span data-ttu-id="2c773-646">使用 `RequestLocalizationOptions` 新增或移除當地語系化提供者。</span><span class="sxs-lookup"><span data-stu-id="2c773-646">Use `RequestLocalizationOptions` to add or remove localization providers.</span></span>

### <a name="set-the-culture-programmatically"></a><span data-ttu-id="2c773-647">以程式設計方式來設定文化特性</span><span class="sxs-lookup"><span data-stu-id="2c773-647">Set the culture programmatically</span></span>

<span data-ttu-id="2c773-648">[GitHub](https://github.com/aspnet/entropy) 上的這個範例 **Localization.StarterWeb** 專案包含可設定 `Culture` 的 UI。</span><span class="sxs-lookup"><span data-stu-id="2c773-648">This sample **Localization.StarterWeb** project on [GitHub](https://github.com/aspnet/entropy) contains UI to set the `Culture`.</span></span> <span data-ttu-id="2c773-649">*Views/Shared/_SelectLanguagePartial.cshtml* 檔可讓您從支援的文化特性清單中選取文化特性：</span><span class="sxs-lookup"><span data-stu-id="2c773-649">The *Views/Shared/_SelectLanguagePartial.cshtml* file allows you to select the culture from the list of supported cultures:</span></span>

[!code-cshtml[](localization/sample/Localization/Views/Shared/_SelectLanguagePartial.cshtml)]

<span data-ttu-id="2c773-650">系統會將 *Views/Shared/_SelectLanguagePartial.cshtml* 檔案新增至配置檔案的 `footer` 區段，以供所有檢視使用：</span><span class="sxs-lookup"><span data-stu-id="2c773-650">The *Views/Shared/_SelectLanguagePartial.cshtml* file is added to the `footer` section of the layout file so it will be available to all views:</span></span>

[!code-cshtml[](localization/sample/Localization/Views/Shared/_Layout.cshtml?range=43-56&highlight=10)]

<span data-ttu-id="2c773-651">`SetLanguage` 方法會設定文化特性的 Cookie。</span><span class="sxs-lookup"><span data-stu-id="2c773-651">The `SetLanguage` method sets the culture cookie.</span></span>

[!code-csharp[](localization/sample/Localization/Controllers/HomeController.cs?range=57-67)]

<span data-ttu-id="2c773-652">您無法將 *_SelectLanguagePartial.cshtml* 插入這個專案的範例程式碼。</span><span class="sxs-lookup"><span data-stu-id="2c773-652">You can't plug in the *_SelectLanguagePartial.cshtml* to sample code for this project.</span></span> <span data-ttu-id="2c773-653">[GitHub](https://github.com/aspnet/entropy)上的**localization.starterweb**專案具有程式碼，可透過相依性 `RequestLocalizationOptions` 插入容器將傳遞至 Razor 部分。 [Dependency Injection](dependency-injection.md)</span><span class="sxs-lookup"><span data-stu-id="2c773-653">The **Localization.StarterWeb** project on [GitHub](https://github.com/aspnet/entropy) has code to flow the `RequestLocalizationOptions` to a Razor partial through the [Dependency Injection](dependency-injection.md) container.</span></span>

## <a name="model-binding-route-data-and-query-strings"></a><span data-ttu-id="2c773-654">模型系結路由資料和查詢字串</span><span class="sxs-lookup"><span data-stu-id="2c773-654">Model binding route data and query strings</span></span>

<span data-ttu-id="2c773-655">請參閱模型系結[路由資料和查詢字串的全球化行為](xref:mvc/models/model-binding#glob)。</span><span class="sxs-lookup"><span data-stu-id="2c773-655">See [Globalization behavior of model binding route data and query strings](xref:mvc/models/model-binding#glob).</span></span>

## <a name="globalization-and-localization-terms"></a><span data-ttu-id="2c773-656">全球化和當地語系化詞彙</span><span class="sxs-lookup"><span data-stu-id="2c773-656">Globalization and localization terms</span></span>

<span data-ttu-id="2c773-657">在進行應用程式的當地語系化程序時，您也需要具備現代軟體開發中常用的相關字元集基本知識，並了解與它們建立關聯的問題。</span><span class="sxs-lookup"><span data-stu-id="2c773-657">The process of localizing your app also requires a basic understanding of relevant character sets commonly used in modern software development and an understanding of the issues associated with them.</span></span> <span data-ttu-id="2c773-658">雖然所有電腦都會將文字儲存為數字 (程式碼)，但不同系統會使用不同數字來儲存相同的文字。</span><span class="sxs-lookup"><span data-stu-id="2c773-658">Although all computers store text as numbers (codes), different systems store the same text using different numbers.</span></span> <span data-ttu-id="2c773-659">當地語系化是指針對特定文化特性/地區設定，轉譯應用程式使用者介面 (UI) 的程序。</span><span class="sxs-lookup"><span data-stu-id="2c773-659">The localization process refers to translating the app user interface (UI) for a specific culture/locale.</span></span>

<span data-ttu-id="2c773-660">[可當地語系化](/dotnet/standard/globalization-localization/localizability-review)是確認全球化應用程式已準備好進行當地語系化的中繼程序。</span><span class="sxs-lookup"><span data-stu-id="2c773-660">[Localizability](/dotnet/standard/globalization-localization/localizability-review) is an intermediate process for verifying that a globalized app is ready for localization.</span></span>

<span data-ttu-id="2c773-661">文化特性名稱的 [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) 格式是 `<languagecode2>-<country/regioncode2>`，其中 `<languagecode2>` 是語言代碼，而 `<country/regioncode2>` 是子文化特性代碼。</span><span class="sxs-lookup"><span data-stu-id="2c773-661">The [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) format for the culture name is `<languagecode2>-<country/regioncode2>`, where `<languagecode2>` is the language code and `<country/regioncode2>` is the subculture code.</span></span> <span data-ttu-id="2c773-662">例如，`es-CL` 是指西班牙文 (智利)，`en-US` 是指英文 (美國)，而 `en-AU` 是指英文 (澳大利亞)。</span><span class="sxs-lookup"><span data-stu-id="2c773-662">For example, `es-CL` for Spanish (Chile), `en-US` for English (United States), and `en-AU` for English (Australia).</span></span> <span data-ttu-id="2c773-663">[RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) 是 ISO 639 (兩個小寫字母，為與某個語言建立關聯的文化特性代碼) 及 ISO 3166 (兩個大寫字母，為與某個國家或地區建立關聯的子文化特性代碼) 的組合。</span><span class="sxs-lookup"><span data-stu-id="2c773-663">[RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) is a combination of an ISO 639 two-letter lowercase culture code associated with a language and an ISO 3166 two-letter uppercase subculture code associated with a country or region.</span></span> <span data-ttu-id="2c773-664">請參閱 [Language Culture Name](https://msdn.microsoft.com/library/ee825488(v=cs.20).aspx) (語言的文化特性名稱)。</span><span class="sxs-lookup"><span data-stu-id="2c773-664">See [Language Culture Name](https://msdn.microsoft.com/library/ee825488(v=cs.20).aspx).</span></span>

<span data-ttu-id="2c773-665">國際化通常縮寫為 "I18N"。</span><span class="sxs-lookup"><span data-stu-id="2c773-665">Internationalization is often abbreviated to "I18N".</span></span> <span data-ttu-id="2c773-666">這個縮寫是採用該詞彙的第一個和最後一個字母，以及這兩個字母間的字母數組成，因此 18 代表第一個字母 "I" 及最後 "N" 中間的字母數。</span><span class="sxs-lookup"><span data-stu-id="2c773-666">The abbreviation takes the first and last letters and the number of letters between them, so 18 stands for the number of letters between the first "I" and the last "N".</span></span> <span data-ttu-id="2c773-667">同樣的原則也適用於全球化 (G11N) 與當地語系化 (L10N) 的縮寫。</span><span class="sxs-lookup"><span data-stu-id="2c773-667">The same applies to Globalization (G11N), and Localization (L10N).</span></span>

<span data-ttu-id="2c773-668">詞彙：</span><span class="sxs-lookup"><span data-stu-id="2c773-668">Terms:</span></span>

* <span data-ttu-id="2c773-669">全球化 (G11N)：讓應用程式支援不同語言和區域的程序。</span><span class="sxs-lookup"><span data-stu-id="2c773-669">Globalization (G11N): The process of making an app support different languages and regions.</span></span>
* <span data-ttu-id="2c773-670">當地語系化 (L10N)：針對特定語言和區域，自訂應用程式的程序。</span><span class="sxs-lookup"><span data-stu-id="2c773-670">Localization (L10N): The process of customizing an app for a given language and region.</span></span>
* <span data-ttu-id="2c773-671">國際化 (I18N)：包含全球化和當地語系化這兩部分的描述。</span><span class="sxs-lookup"><span data-stu-id="2c773-671">Internationalization (I18N): Describes both globalization and localization.</span></span>
* <span data-ttu-id="2c773-672">文化特性：它是一種語言和/或地區。</span><span class="sxs-lookup"><span data-stu-id="2c773-672">Culture: It's a language and, optionally, a region.</span></span>
* <span data-ttu-id="2c773-673">中性文化特性：具有指定的語言但不限地區的文化特性 </span><span class="sxs-lookup"><span data-stu-id="2c773-673">Neutral culture: A culture that has a specified language, but not a region.</span></span> <span data-ttu-id="2c773-674">(例如 "en"、"es")。</span><span class="sxs-lookup"><span data-stu-id="2c773-674">(for example "en", "es")</span></span>
* <span data-ttu-id="2c773-675">特定文化特性：具有指定的語言和地區的文化特性 </span><span class="sxs-lookup"><span data-stu-id="2c773-675">Specific culture: A culture that has a specified language and region.</span></span> <span data-ttu-id="2c773-676">(例如 "en-US"、"en-GB"、"es-CL")。</span><span class="sxs-lookup"><span data-stu-id="2c773-676">(for example "en-US", "en-GB", "es-CL")</span></span>
* <span data-ttu-id="2c773-677">父文化特性：包含特定文化特性的中性文化特性 </span><span class="sxs-lookup"><span data-stu-id="2c773-677">Parent culture: The neutral culture that contains a specific culture.</span></span> <span data-ttu-id="2c773-678">(例如，"en" 是 "en-US" 和 "en-GB" 的父文化特性)。</span><span class="sxs-lookup"><span data-stu-id="2c773-678">(for example, "en" is the parent culture of "en-US" and "en-GB")</span></span>
* <span data-ttu-id="2c773-679">地區設定：地區設定與文化特性相同。</span><span class="sxs-lookup"><span data-stu-id="2c773-679">Locale: A locale is the same as a culture.</span></span>

[!INCLUDE[](~/includes/localization/currency.md)]

## <a name="additional-resources"></a><span data-ttu-id="2c773-680">其他資源</span><span class="sxs-lookup"><span data-stu-id="2c773-680">Additional resources</span></span>

* <xref:fundamentals/troubleshoot-aspnet-core-localization>
* <span data-ttu-id="2c773-681">本文使用的 [Localization.StarterWeb 專案](https://github.com/aspnet/Entropy/tree/master/samples/Localization.StarterWeb)。</span><span class="sxs-lookup"><span data-stu-id="2c773-681">[Localization.StarterWeb project](https://github.com/aspnet/Entropy/tree/master/samples/Localization.StarterWeb) used in the article.</span></span>
* [<span data-ttu-id="2c773-682">全球化與當地語系化 .NET 應用程式</span><span class="sxs-lookup"><span data-stu-id="2c773-682">Globalizing and localizing .NET applications</span></span>](/dotnet/standard/globalization-localization/index)
* [<span data-ttu-id="2c773-683">.Resx 檔案中的資源</span><span class="sxs-lookup"><span data-stu-id="2c773-683">Resources in .resx Files</span></span>](/dotnet/framework/resources/working-with-resx-files-programmatically)
* [<span data-ttu-id="2c773-684">Microsoft 多語應用程式工具組</span><span class="sxs-lookup"><span data-stu-id="2c773-684">Microsoft Multilingual App Toolkit</span></span>](https://marketplace.visualstudio.com/items?itemName=MultilingualAppToolkit.MultilingualAppToolkit-18308)
* [<span data-ttu-id="2c773-685">當地語系化和泛型</span><span class="sxs-lookup"><span data-stu-id="2c773-685">Localization & Generics</span></span>](http://hishambinateya.com/localization-and-generics)

::: moniker-end

<!-- ASP.NET Core 5.x starts here -->
::: moniker range="> aspnetcore-3.1"

<span data-ttu-id="2c773-686">由 [Rick Anderson](https://twitter.com/RickAndMSFT)、[Damien Bowden](https://twitter.com/damien_bod)、[Bart Calixto](https://twitter.com/bartmax)、[Nadeem Afana](https://afana.me/) 和 [Hisham Bin Ateya](https://twitter.com/hishambinateya) 提供</span><span class="sxs-lookup"><span data-stu-id="2c773-686">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Damien Bowden](https://twitter.com/damien_bod), [Bart Calixto](https://twitter.com/bartmax), [Nadeem Afana](https://afana.me/), and [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span></span>

<span data-ttu-id="2c773-687">多語系網站可讓網站觸及更多物件。</span><span class="sxs-lookup"><span data-stu-id="2c773-687">A multilingual website allows the site to reach a wider audience.</span></span> <span data-ttu-id="2c773-688">ASP.NET Core 提供服務與中介軟體，可將網站當地語系化成不同的語言與文化特性。</span><span class="sxs-lookup"><span data-stu-id="2c773-688">ASP.NET Core provides services and middleware for localizing into different languages and cultures.</span></span>

<span data-ttu-id="2c773-689">國際化包含[全球化](/dotnet/api/system.globalization)和[當地語系化](/dotnet/standard/globalization-localization/localization)這兩部分。</span><span class="sxs-lookup"><span data-stu-id="2c773-689">Internationalization involves [Globalization](/dotnet/api/system.globalization) and [Localization](/dotnet/standard/globalization-localization/localization).</span></span> <span data-ttu-id="2c773-690">全球化是指設計出可支援不同文化特性之應用程式的程序。</span><span class="sxs-lookup"><span data-stu-id="2c773-690">Globalization is the process of designing apps that support different cultures.</span></span> <span data-ttu-id="2c773-691">透過全球化，您可新增支援與特定地區相關之已定義語言指令碼的輸入、顯示及輸出作業。</span><span class="sxs-lookup"><span data-stu-id="2c773-691">Globalization adds support for input, display, and output of a defined set of language scripts that relate to specific geographic areas.</span></span>

<span data-ttu-id="2c773-692">當地語系化是指針對全球化應用程式進行調整的程序，且您已順應特定文化特性/地區設定對這些全球化應用程式進行可當地語系化處理。</span><span class="sxs-lookup"><span data-stu-id="2c773-692">Localization is the process of adapting a globalized app, which you have already processed for localizability, to a particular culture/locale.</span></span> <span data-ttu-id="2c773-693">如需詳細資訊，請參閱本文件結尾處的**全球化和當地語系化詞彙**。</span><span class="sxs-lookup"><span data-stu-id="2c773-693">For more information see **Globalization and localization terms** near the end of this document.</span></span>

<span data-ttu-id="2c773-694">應用程式當地語系化包含下列作業：</span><span class="sxs-lookup"><span data-stu-id="2c773-694">App localization involves the following:</span></span>

1. <span data-ttu-id="2c773-695">讓應用程式的內容可當地語系化</span><span class="sxs-lookup"><span data-stu-id="2c773-695">Make the app's content localizable</span></span>
1. <span data-ttu-id="2c773-696">針對您支援的語言和文化特性提供當地語系化資源</span><span class="sxs-lookup"><span data-stu-id="2c773-696">Provide localized resources for the languages and cultures you support</span></span>
1. <span data-ttu-id="2c773-697">實作可依據每項要求選取語言/文化特性的策略</span><span class="sxs-lookup"><span data-stu-id="2c773-697">Implement a strategy to select the language/culture for each request</span></span>

<span data-ttu-id="2c773-698">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/localization/sample/Localization)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="2c773-698">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/localization/sample/Localization) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="make-the-apps-content-localizable"></a><span data-ttu-id="2c773-699">讓應用程式的內容可當地語系化</span><span class="sxs-lookup"><span data-stu-id="2c773-699">Make the app's content localizable</span></span>

<span data-ttu-id="2c773-700"><xref:Microsoft.Extensions.Localization.IStringLocalizer>` and <xref:Microsoft.Extensions.Localization.IStringLocalizer%601> were architected to improve productivity when developing localized apps. `IStringLocalizer ` uses the [ResourceManager](/dotnet/api/system.resources.resourcemanager) and [ResourceReader](/dotnet/api/system.resources.resourcereader) to provide culture-specific resources at run time. The interface has an indexer and an ` IEnumerable ` for returning localized strings. ` IStringLocalizer ' 不需要將預設語言字串儲存在資源檔中。</span><span class="sxs-lookup"><span data-stu-id="2c773-700"><xref:Microsoft.Extensions.Localization.IStringLocalizer>` and <xref:Microsoft.Extensions.Localization.IStringLocalizer%601> were architected to improve productivity when developing localized apps. `IStringLocalizer` uses the [ResourceManager](/dotnet/api/system.resources.resourcemanager) and [ResourceReader](/dotnet/api/system.resources.resourcereader) to provide culture-specific resources at run time. The interface has an indexer and an `IEnumerable` for returning localized strings. `IStringLocalizer\` doesn't require storing the default language strings in a resource file.</span></span> <span data-ttu-id="2c773-701">您不必在開發初期建立資源檔，即可開發以當地語系化為目標的應用程式。</span><span class="sxs-lookup"><span data-stu-id="2c773-701">You can develop an app targeted for localization and not need to create resource files early in development.</span></span> <span data-ttu-id="2c773-702">下列程式碼會示範如何包裝 "About Title" 字串以進行當地語系化。</span><span class="sxs-lookup"><span data-stu-id="2c773-702">The code below shows how to wrap the string "About Title" for localization.</span></span>

[!code-csharp[](localization/sample/Localization/Controllers/AboutController.cs)]

<span data-ttu-id="2c773-703">在上述程式碼中， `IStringLocalizer<T>` 執行是來自相依性[插入](dependency-injection.md)。</span><span class="sxs-lookup"><span data-stu-id="2c773-703">In the preceding code, the `IStringLocalizer<T>` implementation comes from [Dependency Injection](dependency-injection.md).</span></span> <span data-ttu-id="2c773-704">如果找不到 "About Title" 的當地語系化值，即會傳回索引子的索引鍵，也就是 "About Title" 字串。</span><span class="sxs-lookup"><span data-stu-id="2c773-704">If the localized value of "About Title" isn't found, then the indexer key is returned, that is, the string "About Title".</span></span> <span data-ttu-id="2c773-705">您可以保留應用程式中的預設語言常值字串，並將其包裝在當地語系化工具中，以便專注於開發應用程式。</span><span class="sxs-lookup"><span data-stu-id="2c773-705">You can leave the default language literal strings in the app and wrap them in the localizer, so that you can focus on developing the app.</span></span> <span data-ttu-id="2c773-706">您不用先建立預設資源檔，即可使用預設語言來開發應用程式，並針對當地語系化步驟進行應用程式的準備。</span><span class="sxs-lookup"><span data-stu-id="2c773-706">You develop your app with your default language and prepare it for the localization step without first creating a default resource file.</span></span> <span data-ttu-id="2c773-707">或者，您可以使用傳統方法，並提供索引鍵以擷取預設語言字串。</span><span class="sxs-lookup"><span data-stu-id="2c773-707">Alternatively, you can use the traditional approach and provide a key to retrieve the default language string.</span></span> <span data-ttu-id="2c773-708">對許多開發人員來說，新的工作流程 (單純包裝字串常值而不使用預設語言的 *.resx* 檔案) 可以降低當地語系化應用程式的額外負荷。</span><span class="sxs-lookup"><span data-stu-id="2c773-708">For many developers the new workflow of not having a default language *.resx* file and simply wrapping the string literals can reduce the overhead of localizing an app.</span></span> <span data-ttu-id="2c773-709">其他開發人員則偏好傳統的工作流程，因為這種方法更便於使用較長的字串常值，且更易於更新當地語系化的字串。</span><span class="sxs-lookup"><span data-stu-id="2c773-709">Other developers will prefer the traditional work flow as it can make it easier to work with longer string literals and make it easier to update localized strings.</span></span>

<span data-ttu-id="2c773-710">若是包含 HTML 的資源，請使用 `IHtmlLocalizer<T>` 實作。</span><span class="sxs-lookup"><span data-stu-id="2c773-710">Use the `IHtmlLocalizer<T>` implementation for resources that contain HTML.</span></span> <span data-ttu-id="2c773-711">`IHtmlLocalizer` 會對資源字串中經過格式化的引數進行 HTML 編碼，但不會對資源字串本身進行 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="2c773-711">`IHtmlLocalizer` HTML encodes arguments that are formatted in the resource string, but doesn't HTML encode the resource string itself.</span></span> <span data-ttu-id="2c773-712">在下列醒目提示的範例中，只有 `name` 參數的值經過 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="2c773-712">In the sample highlighted below, only the value of `name` parameter is HTML encoded.</span></span>

[!code-csharp[](../fundamentals/localization/sample/Localization/Controllers/BookController.cs?highlight=3,5,20&start=1&end=24)]

<span data-ttu-id="2c773-713">**注意：** 一般來說，您只想將文字當地語系化，而不是 HTML。</span><span class="sxs-lookup"><span data-stu-id="2c773-713">**Note:** You generally want to only localize text and not HTML.</span></span>

<span data-ttu-id="2c773-714">您可以在最底層的[相依性插入](dependency-injection.md)中，將 `IStringLocalizerFactory` 移出：</span><span class="sxs-lookup"><span data-stu-id="2c773-714">At the lowest level, you can get `IStringLocalizerFactory` out of [Dependency Injection](dependency-injection.md):</span></span>

[!code-csharp[](localization/sample/Localization/Controllers/TestController.cs?start=9&end=26&highlight=7-13)]

<span data-ttu-id="2c773-715">上述程式碼會示範這兩個 Factory Create 方法。</span><span class="sxs-lookup"><span data-stu-id="2c773-715">The code above demonstrates each of the two factory create methods.</span></span>

<span data-ttu-id="2c773-716">您可以依據控制器或區域來分割當地語系化的字串，也可以只用一個容器。</span><span class="sxs-lookup"><span data-stu-id="2c773-716">You can partition your localized strings by controller, area, or have just one container.</span></span> <span data-ttu-id="2c773-717">在範例應用程式中，會針對共用資源使用名為 `SharedResource` 的虛擬類別。</span><span class="sxs-lookup"><span data-stu-id="2c773-717">In the sample app, a dummy class named `SharedResource` is used for shared resources.</span></span>

[!code-csharp[](localization/sample/Localization/Resources/SharedResource.cs)]

<span data-ttu-id="2c773-718">有些開發人員會使用 `Startup` 類別來包含全域或共用字串。</span><span class="sxs-lookup"><span data-stu-id="2c773-718">Some developers use the `Startup` class to contain global or shared strings.</span></span> <span data-ttu-id="2c773-719">下列範例會使用 `InfoController` 和 `SharedResource` 當地語系化工具：</span><span class="sxs-lookup"><span data-stu-id="2c773-719">In the sample below, the `InfoController` and the `SharedResource` localizers are used:</span></span>

[!code-csharp[](localization/sample/Localization/Controllers/InfoController.cs?range=9-26)]

## <a name="view-localization"></a><span data-ttu-id="2c773-720">檢視當地語系化</span><span class="sxs-lookup"><span data-stu-id="2c773-720">View localization</span></span>

<span data-ttu-id="2c773-721">`IViewLocalizer` 服務可提供[檢視](xref:mvc/views/overview)的當地語系化字串。</span><span class="sxs-lookup"><span data-stu-id="2c773-721">The `IViewLocalizer` service provides localized strings for a [view](xref:mvc/views/overview).</span></span> <span data-ttu-id="2c773-722">`ViewLocalizer` 類別會實作這個介面，並透過檢視檔案路徑來找出資源的位置。</span><span class="sxs-lookup"><span data-stu-id="2c773-722">The `ViewLocalizer` class implements this interface and finds the resource location from the view file path.</span></span> <span data-ttu-id="2c773-723">下列程式碼示範如何使用 `IViewLocalizer` 的預設實作：</span><span class="sxs-lookup"><span data-stu-id="2c773-723">The following code shows how to use the default implementation of `IViewLocalizer`:</span></span>

[!code-cshtml[](localization/sample/Localization/Views/Home/About.cshtml)]

<span data-ttu-id="2c773-724">`IViewLocalizer` 的預設實作會依據檢視的檔案名稱來找出資源檔。</span><span class="sxs-lookup"><span data-stu-id="2c773-724">The default implementation of `IViewLocalizer` finds the resource file based on the view's file name.</span></span> <span data-ttu-id="2c773-725">其中並沒有任何選項可以使用全域共用的資源檔。</span><span class="sxs-lookup"><span data-stu-id="2c773-725">There's no option to use a global shared resource file.</span></span> <span data-ttu-id="2c773-726">`ViewLocalizer`使用來執行當地語系化工具 `IHtmlLocalizer` ，因此 Razor 不會對當地語系化字串進行 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="2c773-726">`ViewLocalizer` implements the localizer using `IHtmlLocalizer`, so Razor doesn't HTML encode the localized string.</span></span> <span data-ttu-id="2c773-727">您可以參數化資源字串，`IViewLocalizer` 即會對參數 (而不是資源字串) 進行 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="2c773-727">You can parameterize resource strings and `IViewLocalizer` will HTML encode the parameters, but not the resource string.</span></span> <span data-ttu-id="2c773-728">請考慮下列 Razor 標記：</span><span class="sxs-lookup"><span data-stu-id="2c773-728">Consider the following Razor markup:</span></span>

```cshtml
@Localizer["<i>Hello</i> <b>{0}!</b>", UserManager.GetUserName(User)]
```

<span data-ttu-id="2c773-729">法文資源檔可能包含下列內容：</span><span class="sxs-lookup"><span data-stu-id="2c773-729">A French resource file could contain the following:</span></span>

| <span data-ttu-id="2c773-730">Key</span><span class="sxs-lookup"><span data-stu-id="2c773-730">Key</span></span> | <span data-ttu-id="2c773-731">值</span><span class="sxs-lookup"><span data-stu-id="2c773-731">Value</span></span> |
| ----- | ---
<span data-ttu-id="2c773-732">標題： author： description： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2c773-732">title: author: description: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="2c773-733">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2c773-733">'Blazor'</span></span>
- <span data-ttu-id="2c773-734">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2c773-734">'Identity'</span></span>
- <span data-ttu-id="2c773-735">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2c773-735">'Let's Encrypt'</span></span>
- <span data-ttu-id="2c773-736">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2c773-736">'Razor'</span></span>
- <span data-ttu-id="2c773-737">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2c773-737">'SignalR' uid:</span></span> 

<span data-ttu-id="2c773-738">--- | | `<i>Hello</i> <b>{0}!</b>` | `<i>Bonjour</i> <b>{0} !</b>` |</span><span class="sxs-lookup"><span data-stu-id="2c773-738">--- | | `<i>Hello</i> <b>{0}!</b>` | `<i>Bonjour</i> <b>{0} !</b>` |</span></span>

<span data-ttu-id="2c773-739">轉譯的檢視內容可能包含來自資源檔的 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="2c773-739">The rendered view would contain the HTML markup from the resource file.</span></span>

<span data-ttu-id="2c773-740">**注意：** 一般來說，您只想將文字當地語系化，而不是 HTML。</span><span class="sxs-lookup"><span data-stu-id="2c773-740">**Note:** You generally want to only localize text and not HTML.</span></span>

<span data-ttu-id="2c773-741">若要在檢視中使用共用的資源檔，請插入 `IHtmlLocalizer<T>`：</span><span class="sxs-lookup"><span data-stu-id="2c773-741">To use a shared resource file in a view, inject `IHtmlLocalizer<T>`:</span></span>

[!code-cshtml[](../fundamentals/localization/sample/Localization/Views/Test/About.cshtml?highlight=5,12)]

## <a name="dataannotations-localization"></a><span data-ttu-id="2c773-742">DataAnnotations 當地語系化</span><span class="sxs-lookup"><span data-stu-id="2c773-742">DataAnnotations localization</span></span>

<span data-ttu-id="2c773-743">DataAnnotations 錯誤訊息會使用 `IStringLocalizer<T>` 來當地語系化。</span><span class="sxs-lookup"><span data-stu-id="2c773-743">DataAnnotations error messages are localized with `IStringLocalizer<T>`.</span></span> <span data-ttu-id="2c773-744">使用 `ResourcesPath = "Resources"` 選項時，`RegisterViewModel` 中的錯誤訊息會儲存在下列路徑之一：</span><span class="sxs-lookup"><span data-stu-id="2c773-744">Using the option `ResourcesPath = "Resources"`, the error messages in `RegisterViewModel` can be stored in either of the following paths:</span></span>

* <span data-ttu-id="2c773-745">*Resources/Viewmodel. RegisterViewModel. fr .resx*</span><span class="sxs-lookup"><span data-stu-id="2c773-745">*Resources/ViewModels.Account.RegisterViewModel.fr.resx*</span></span>
* <span data-ttu-id="2c773-746">*Resources/ViewModels/Account/RegisterViewModel.fr.resx*</span><span class="sxs-lookup"><span data-stu-id="2c773-746">*Resources/ViewModels/Account/RegisterViewModel.fr.resx*</span></span>

[!code-csharp[](localization/sample/Localization/ViewModels/Account/RegisterViewModel.cs?start=9&end=26)]

<span data-ttu-id="2c773-747">在 ASP.NET Core MVC 1.1.0 和更高版本中，系統會將非驗證屬性當地語系化。</span><span class="sxs-lookup"><span data-stu-id="2c773-747">In ASP.NET Core MVC 1.1.0 and higher, non-validation attributes are localized.</span></span> <span data-ttu-id="2c773-748">ASP.NET Core MVC 1.0 **不會**查閱非驗證屬性的當地語系化字串。</span><span class="sxs-lookup"><span data-stu-id="2c773-748">ASP.NET Core MVC 1.0 does **not** look up localized strings for non-validation attributes.</span></span>

<a name="one-resource-string-multiple-classes"></a>

### <a name="using-one-resource-string-for-multiple-classes"></a><span data-ttu-id="2c773-749">針對多個類別使用同一個資源字串</span><span class="sxs-lookup"><span data-stu-id="2c773-749">Using one resource string for multiple classes</span></span>

<span data-ttu-id="2c773-750">下列程式碼會示範如何針對含有多個類別的驗證屬性使用同一個資源字串：</span><span class="sxs-lookup"><span data-stu-id="2c773-750">The following code shows how to use one resource string for validation attributes with multiple classes:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddDataAnnotationsLocalization(options => {
            options.DataAnnotationLocalizerProvider = (type, factory) =>
                factory.Create(typeof(SharedResource));
        });
}
```

<span data-ttu-id="2c773-751">在先前的程式碼中，`SharedResource` 是與儲存驗證訊息的 resx 對應的類別。</span><span class="sxs-lookup"><span data-stu-id="2c773-751">In the preceding code, `SharedResource` is the class corresponding to the resx where your validation messages are stored.</span></span> <span data-ttu-id="2c773-752">使用這個方法時，DataAnnotations 只會使用 `SharedResource`，而不是每個類別的資源。</span><span class="sxs-lookup"><span data-stu-id="2c773-752">With this approach, DataAnnotations will only use `SharedResource`, rather than the resource for each class.</span></span>

## <a name="provide-localized-resources-for-the-languages-and-cultures-you-support"></a><span data-ttu-id="2c773-753">針對您支援的語言和文化特性提供當地語系化資源</span><span class="sxs-lookup"><span data-stu-id="2c773-753">Provide localized resources for the languages and cultures you support</span></span>

### <a name="supportedcultures-and-supporteduicultures"></a><span data-ttu-id="2c773-754">SupportedCultures 和 SupportedUICultures</span><span class="sxs-lookup"><span data-stu-id="2c773-754">SupportedCultures and SupportedUICultures</span></span>

<span data-ttu-id="2c773-755">ASP.NET Core 可讓您指定 `SupportedCultures` 和 `SupportedUICultures` 這兩個文化特性值。</span><span class="sxs-lookup"><span data-stu-id="2c773-755">ASP.NET Core allows you to specify two culture values, `SupportedCultures` and `SupportedUICultures`.</span></span> <span data-ttu-id="2c773-756">`SupportedCultures` 的 [CultureInfo](/dotnet/api/system.globalization.cultureinfo) 物件可決定文化特性相依函式的結果，例如日期、時間、數字及貨幣格式。</span><span class="sxs-lookup"><span data-stu-id="2c773-756">The [CultureInfo](/dotnet/api/system.globalization.cultureinfo) object for `SupportedCultures` determines the results of culture-dependent functions, such as date, time, number, and currency formatting.</span></span> <span data-ttu-id="2c773-757">`SupportedCultures` 也可決定文字排列順序、大小寫慣例和字串比較。</span><span class="sxs-lookup"><span data-stu-id="2c773-757">`SupportedCultures` also determines the sorting order of text, casing conventions, and string comparisons.</span></span> <span data-ttu-id="2c773-758">如需伺服器如何取得文化特性的詳細資訊，請參閱 [CultureInfo.CurrentCulture](/dotnet/api/system.stringcomparer.currentculture#System_StringComparer_CurrentCulture)。</span><span class="sxs-lookup"><span data-stu-id="2c773-758">See [CultureInfo.CurrentCulture](/dotnet/api/system.stringcomparer.currentculture#System_StringComparer_CurrentCulture) for more info on how the server gets the Culture.</span></span> <span data-ttu-id="2c773-759">`SupportedUICultures`會決定[ResourceManager](/dotnet/api/system.resources.resourcemanager)會查閱哪些翻譯的字串（來自 *.resx*檔案）。</span><span class="sxs-lookup"><span data-stu-id="2c773-759">The `SupportedUICultures` determines which translated strings (from *.resx* files) are looked up by the [ResourceManager](/dotnet/api/system.resources.resourcemanager).</span></span> <span data-ttu-id="2c773-760">`ResourceManager` 僅會查閱 `CurrentUICulture` 所決定之文化特性特有的字串。</span><span class="sxs-lookup"><span data-stu-id="2c773-760">The `ResourceManager` simply looks up culture-specific strings that's determined by `CurrentUICulture`.</span></span> <span data-ttu-id="2c773-761">.NET 中的每個執行緒都有 `CurrentCulture` 和 `CurrentUICulture` 物件。</span><span class="sxs-lookup"><span data-stu-id="2c773-761">Every thread in .NET has `CurrentCulture` and `CurrentUICulture` objects.</span></span> <span data-ttu-id="2c773-762">ASP.NET Core 會在轉譯文化特性相依函式時檢查這些值。</span><span class="sxs-lookup"><span data-stu-id="2c773-762">ASP.NET Core inspects these values when rendering culture-dependent functions.</span></span> <span data-ttu-id="2c773-763">比方說，如果目前執行緒的文化特性設定為 "en-US" (英文 - 美國)，`DateTime.Now.ToLongDateString()` 會顯示 "Thursday, February 18, 2016"，但如果 `CurrentCulture` 設定為 "es-ES" (西班牙文 - 西班牙)，則輸出會是 "jueves, 18 de febrero de 2016"。</span><span class="sxs-lookup"><span data-stu-id="2c773-763">For example, if the current thread's culture is set to "en-US" (English, United States), `DateTime.Now.ToLongDateString()` displays "Thursday, February 18, 2016", but if `CurrentCulture` is set to "es-ES" (Spanish, Spain) the output will be "jueves, 18 de febrero de 2016".</span></span>

## <a name="resource-files"></a><span data-ttu-id="2c773-764">資源檔</span><span class="sxs-lookup"><span data-stu-id="2c773-764">Resource files</span></span>

<span data-ttu-id="2c773-765">資源檔是一種實用的機制，可讓您將可當地語系化的字串與代碼區隔開來。</span><span class="sxs-lookup"><span data-stu-id="2c773-765">A resource file is a useful mechanism for separating localizable strings from code.</span></span> <span data-ttu-id="2c773-766">非預設語言的翻譯字串會在 *.resx*資源檔中隔離。</span><span class="sxs-lookup"><span data-stu-id="2c773-766">Translated strings for the non-default language are isolated in *.resx* resource files.</span></span> <span data-ttu-id="2c773-767">例如，您可以建立名為 *Welcome.es.resx* 的西班牙文資源檔，以包含翻譯的字串。</span><span class="sxs-lookup"><span data-stu-id="2c773-767">For example, you might want to create Spanish resource file named *Welcome.es.resx* containing translated strings.</span></span> <span data-ttu-id="2c773-768">"es" 是西班牙文的語言代碼。</span><span class="sxs-lookup"><span data-stu-id="2c773-768">"es" is the language code for Spanish.</span></span> <span data-ttu-id="2c773-769">若要在 Visual Studio 中建立這個資源檔：</span><span class="sxs-lookup"><span data-stu-id="2c773-769">To create this resource file in Visual Studio:</span></span>

1. <span data-ttu-id="2c773-770">在方案總管\*\*\*\* 中，以滑鼠右鍵按一下要放置資源檔的資料夾 > [新增]**[新增項目]** > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="2c773-770">In **Solution Explorer**, right click on the folder which will contain the resource file > **Add** > **New Item**.</span></span>

    ![巢狀特色選單：方案總管會開啟 [資源] 的特色選單，](localization/_static/newi.png)

2. <span data-ttu-id="2c773-773">在 [Search installed templates] (搜尋已安裝的範本)\*\*\*\* 方塊中，輸入「資源」，並命名檔案。</span><span class="sxs-lookup"><span data-stu-id="2c773-773">In the **Search installed templates** box, enter "resource" and name the file.</span></span>

    ![[新增項目] 對話方塊](localization/_static/res.png)

3. <span data-ttu-id="2c773-775">在 [名稱]\*\*\*\* 資料行中輸入索引鍵值 (原生字串)，並在 [值]\*\*\*\* 資料行中輸入已翻譯的字串。</span><span class="sxs-lookup"><span data-stu-id="2c773-775">Enter the key value (native string) in the **Name** column and the translated string in the **Value** column.</span></span>

    ![Welcome.es.resx 檔案 (西班牙文的「歡迎使用」資源檔)，其中 [名稱] 資料行的文字為 Hello，而 [值] 資料行的文字為 Hola (Hello 的西班牙文)](localization/_static/hola.png)

    <span data-ttu-id="2c773-777">Visual Studio 會顯示 *Welcome.es.resx* 檔案。</span><span class="sxs-lookup"><span data-stu-id="2c773-777">Visual Studio shows the *Welcome.es.resx* file.</span></span>

    ![方案總管，其中顯示「歡迎使用」的西班牙文 (es) 資源檔](localization/_static/se.png)

## <a name="resource-file-naming"></a><span data-ttu-id="2c773-779">資源檔命名</span><span class="sxs-lookup"><span data-stu-id="2c773-779">Resource file naming</span></span>

<span data-ttu-id="2c773-780">資源的命名方式是以其類別的完整類型名稱去掉組件名稱而得。</span><span class="sxs-lookup"><span data-stu-id="2c773-780">Resources are named for the full type name of their class minus the assembly name.</span></span> <span data-ttu-id="2c773-781">例如，假設專案中的法文資源是 `LocalizationWebsite.Web.Startup` 類別、主要組件為 `LocalizationWebsite.Web.dll`，就會命名為 *Startup.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="2c773-781">For example, a French resource in a project whose main assembly is `LocalizationWebsite.Web.dll` for the class `LocalizationWebsite.Web.Startup` would be named *Startup.fr.resx*.</span></span> <span data-ttu-id="2c773-782">若是 `LocalizationWebsite.Web.Controllers.HomeController` 類別的資源，則應命名為 *Controllers.HomeController.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="2c773-782">A resource for the class `LocalizationWebsite.Web.Controllers.HomeController` would be named *Controllers.HomeController.fr.resx*.</span></span> <span data-ttu-id="2c773-783">如果目標類別的命名空間和組件名稱不相同，則需要使用完整類型名稱。</span><span class="sxs-lookup"><span data-stu-id="2c773-783">If your targeted class's namespace isn't the same as the assembly name you will need the full type name.</span></span> <span data-ttu-id="2c773-784">比方說，範例專案中 `ExtraNamespace.Tools` 類型的資源會命名為 *ExtraNamespace.Tools.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="2c773-784">For example, in the sample project a resource for the type `ExtraNamespace.Tools` would be named *ExtraNamespace.Tools.fr.resx*.</span></span>

<span data-ttu-id="2c773-785">在範例專案中，`ConfigureServices` 方法會將 `ResourcesPath` 設為 "Resources"，因此首頁控制器的法文資源檔專案相對路徑即為 *Resources/Controllers.HomeController.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="2c773-785">In the sample project, the `ConfigureServices` method sets the `ResourcesPath` to "Resources", so the project relative path for the home controller's French resource file is *Resources/Controllers.HomeController.fr.resx*.</span></span> <span data-ttu-id="2c773-786">或者，您可以使用資料夾來收集資源檔。</span><span class="sxs-lookup"><span data-stu-id="2c773-786">Alternatively, you can use folders to organize resource files.</span></span> <span data-ttu-id="2c773-787">若是首頁控制器，路徑就是 *Resources/Controllers/HomeController.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="2c773-787">For the home controller, the path would be *Resources/Controllers/HomeController.fr.resx*.</span></span> <span data-ttu-id="2c773-788">如果您不使用 `ResourcesPath` 選項，*.resx* 檔案即會放置在專案的基底目錄中。</span><span class="sxs-lookup"><span data-stu-id="2c773-788">If you don't use the `ResourcesPath` option, the *.resx* file would go in the project base directory.</span></span> <span data-ttu-id="2c773-789">`HomeController` 的資源檔會命名為 *Controllers.HomeController.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="2c773-789">The resource file for `HomeController` would be named *Controllers.HomeController.fr.resx*.</span></span> <span data-ttu-id="2c773-790">您可依據自己的資源檔收集方式，來選擇要使用點或路徑的命名慣例。</span><span class="sxs-lookup"><span data-stu-id="2c773-790">The choice of using the dot or path naming convention depends on how you want to organize your resource files.</span></span>

| <span data-ttu-id="2c773-791">資源名稱</span><span class="sxs-lookup"><span data-stu-id="2c773-791">Resource name</span></span> | <span data-ttu-id="2c773-792">點或路徑命名</span><span class="sxs-lookup"><span data-stu-id="2c773-792">Dot or path naming</span></span> |
| ---
<span data-ttu-id="2c773-793">標題： author： description： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2c773-793">title: author: description: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="2c773-794">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2c773-794">'Blazor'</span></span>
- <span data-ttu-id="2c773-795">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2c773-795">'Identity'</span></span>
- <span data-ttu-id="2c773-796">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2c773-796">'Let's Encrypt'</span></span>
- <span data-ttu-id="2c773-797">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2c773-797">'Razor'</span></span>
- <span data-ttu-id="2c773-798">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2c773-798">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2c773-799">標題： author： description： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2c773-799">title: author: description: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="2c773-800">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2c773-800">'Blazor'</span></span>
- <span data-ttu-id="2c773-801">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2c773-801">'Identity'</span></span>
- <span data-ttu-id="2c773-802">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2c773-802">'Let's Encrypt'</span></span>
- <span data-ttu-id="2c773-803">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2c773-803">'Razor'</span></span>
- <span data-ttu-id="2c773-804">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2c773-804">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2c773-805">標題： author： description： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2c773-805">title: author: description: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="2c773-806">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2c773-806">'Blazor'</span></span>
- <span data-ttu-id="2c773-807">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2c773-807">'Identity'</span></span>
- <span data-ttu-id="2c773-808">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2c773-808">'Let's Encrypt'</span></span>
- <span data-ttu-id="2c773-809">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2c773-809">'Razor'</span></span>
- <span data-ttu-id="2c773-810">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2c773-810">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2c773-811">標題： author： description： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2c773-811">title: author: description: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="2c773-812">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2c773-812">'Blazor'</span></span>
- <span data-ttu-id="2c773-813">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2c773-813">'Identity'</span></span>
- <span data-ttu-id="2c773-814">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2c773-814">'Let's Encrypt'</span></span>
- <span data-ttu-id="2c773-815">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2c773-815">'Razor'</span></span>
- <span data-ttu-id="2c773-816">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2c773-816">'SignalR' uid:</span></span> 

<span data-ttu-id="2c773-817">------   |---標題： author： description： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2c773-817">------   | --- title: author: description: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="2c773-818">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2c773-818">'Blazor'</span></span>
- <span data-ttu-id="2c773-819">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2c773-819">'Identity'</span></span>
- <span data-ttu-id="2c773-820">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2c773-820">'Let's Encrypt'</span></span>
- <span data-ttu-id="2c773-821">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2c773-821">'Razor'</span></span>
- <span data-ttu-id="2c773-822">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2c773-822">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2c773-823">標題： author： description： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2c773-823">title: author: description: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="2c773-824">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2c773-824">'Blazor'</span></span>
- <span data-ttu-id="2c773-825">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2c773-825">'Identity'</span></span>
- <span data-ttu-id="2c773-826">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2c773-826">'Let's Encrypt'</span></span>
- <span data-ttu-id="2c773-827">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2c773-827">'Razor'</span></span>
- <span data-ttu-id="2c773-828">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2c773-828">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2c773-829">標題： author： description： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2c773-829">title: author: description: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="2c773-830">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2c773-830">'Blazor'</span></span>
- <span data-ttu-id="2c773-831">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2c773-831">'Identity'</span></span>
- <span data-ttu-id="2c773-832">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2c773-832">'Let's Encrypt'</span></span>
- <span data-ttu-id="2c773-833">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2c773-833">'Razor'</span></span>
- <span data-ttu-id="2c773-834">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2c773-834">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2c773-835">標題： author： description： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2c773-835">title: author: description: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="2c773-836">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2c773-836">'Blazor'</span></span>
- <span data-ttu-id="2c773-837">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2c773-837">'Identity'</span></span>
- <span data-ttu-id="2c773-838">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2c773-838">'Let's Encrypt'</span></span>
- <span data-ttu-id="2c773-839">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2c773-839">'Razor'</span></span>
- <span data-ttu-id="2c773-840">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2c773-840">'SignalR' uid:</span></span> 

<span data-ttu-id="2c773-841">------- | |Resources/controller. HomeController. .resx |點 | |Resources/controller/HomeController. .resx |路徑 | |   |    |</span><span class="sxs-lookup"><span data-stu-id="2c773-841">------- | | Resources/Controllers.HomeController.fr.resx | Dot  | | Resources/Controllers/HomeController.fr.resx  | Path | |    |     |</span></span>

<span data-ttu-id="2c773-842">在 views 中使用的資源檔會 `@inject IViewLocalizer` Razor 遵循類似的模式。</span><span class="sxs-lookup"><span data-stu-id="2c773-842">Resource files using `@inject IViewLocalizer` in Razor views follow a similar pattern.</span></span> <span data-ttu-id="2c773-843">您可以使用點命名或路徑命名方式，來命名檢視的資源檔。</span><span class="sxs-lookup"><span data-stu-id="2c773-843">The resource file for a view can be named using either dot naming or path naming.</span></span> Razor<span data-ttu-id="2c773-844">查看資源檔模擬其相關聯之視圖檔案的路徑。</span><span class="sxs-lookup"><span data-stu-id="2c773-844"> view resource files mimic the path of their associated view file.</span></span> <span data-ttu-id="2c773-845">假設我們將 `ResourcesPath` 設為 "Resources"，與 *Views/Home/About.cshtml* 檢視建立關聯的法文資源檔可為下列其一：</span><span class="sxs-lookup"><span data-stu-id="2c773-845">Assuming we set the `ResourcesPath` to "Resources", the French resource file associated with the *Views/Home/About.cshtml* view could be either of the following:</span></span>

* <span data-ttu-id="2c773-846">Resources/Views/Home/About.fr.resx</span><span class="sxs-lookup"><span data-stu-id="2c773-846">Resources/Views/Home/About.fr.resx</span></span>

* <span data-ttu-id="2c773-847">Resources/Views.Home.About.fr.resx</span><span class="sxs-lookup"><span data-stu-id="2c773-847">Resources/Views.Home.About.fr.resx</span></span>

<span data-ttu-id="2c773-848">如果您不使用 `ResourcesPath` 選項，則檢視的 *.resx* 檔案會與檢視位於相同資料夾中。</span><span class="sxs-lookup"><span data-stu-id="2c773-848">If you don't use the `ResourcesPath` option, the *.resx* file for a view would be located in the same folder as the view.</span></span>

### <a name="rootnamespaceattribute"></a><span data-ttu-id="2c773-849">RootNamespaceAttribute</span><span class="sxs-lookup"><span data-stu-id="2c773-849">RootNamespaceAttribute</span></span> 

<span data-ttu-id="2c773-850">[RootNamespace](/dotnet/api/microsoft.extensions.localization.rootnamespaceattribute?view=aspnetcore-2.1) 屬性會在組件的根命名空間與組件名稱不同時，提供組件的根命名空間。</span><span class="sxs-lookup"><span data-stu-id="2c773-850">The [RootNamespace](/dotnet/api/microsoft.extensions.localization.rootnamespaceattribute?view=aspnetcore-2.1) attribute provides the root namespace of an assembly when the root namespace of an assembly is different than the assembly name.</span></span> 

> [!WARNING]
> <span data-ttu-id="2c773-851">當專案的名稱不是有效的 .NET 識別碼時，就可能發生這種情況。</span><span class="sxs-lookup"><span data-stu-id="2c773-851">This can occur when a project's name is not a valid .NET identifier.</span></span> <span data-ttu-id="2c773-852">例如， `my-project-name.csproj` 會使用根命名空間 `my_project_name` ，以及 `my-project-name` 導致此錯誤的元件名稱。</span><span class="sxs-lookup"><span data-stu-id="2c773-852">For instance `my-project-name.csproj` will use the root namespace `my_project_name` and the assembly name `my-project-name` leading to this error.</span></span> 

<span data-ttu-id="2c773-853">如果組件的根命名空間與組件名稱不同：</span><span class="sxs-lookup"><span data-stu-id="2c773-853">If the root namespace of an assembly is different than the assembly name:</span></span>

* <span data-ttu-id="2c773-854">根據預設，當地語系化無法運作。</span><span class="sxs-lookup"><span data-stu-id="2c773-854">Localization does not work by default.</span></span>
* <span data-ttu-id="2c773-855">在組件中搜尋資源的方式造成當地語系化失敗。</span><span class="sxs-lookup"><span data-stu-id="2c773-855">Localization fails due to the way resources are searched for within the assembly.</span></span> <span data-ttu-id="2c773-856">`RootNamespace` 是建置時間值，無法用於執行處理序。</span><span class="sxs-lookup"><span data-stu-id="2c773-856">`RootNamespace` is a build-time value which is not available to the executing process.</span></span> 

<span data-ttu-id="2c773-857">如果 `RootNamespace` 與 `AssemblyName` 不同，請將下列內容納入 *AssemblyInfo.cs* (參數值取代為實際值)：</span><span class="sxs-lookup"><span data-stu-id="2c773-857">If the `RootNamespace` is different from the `AssemblyName`, include the following in *AssemblyInfo.cs* (with parameter values replaced with the actual values):</span></span>

```csharp
using System.Reflection;
using Microsoft.Extensions.Localization;

[assembly: ResourceLocation("Resource Folder Name")]
[assembly: RootNamespace("App Root Namespace")]
```

<span data-ttu-id="2c773-858">上述程式碼可成功解析 resx 檔案。</span><span class="sxs-lookup"><span data-stu-id="2c773-858">The preceding code enables the successful resolution of resx files.</span></span>

## <a name="culture-fallback-behavior"></a><span data-ttu-id="2c773-859">文化特性後援行為</span><span class="sxs-lookup"><span data-stu-id="2c773-859">Culture fallback behavior</span></span>

<span data-ttu-id="2c773-860">搜尋資源時，當地語系化會使用「文化特性後援」。</span><span class="sxs-lookup"><span data-stu-id="2c773-860">When searching for a resource, localization engages in "culture fallback".</span></span> <span data-ttu-id="2c773-861">從所要求的文化特性開始，如果找不到，就會還原成該文化特性的父文化特性。</span><span class="sxs-lookup"><span data-stu-id="2c773-861">Starting from the requested culture, if not found, it reverts to the parent culture of that culture.</span></span> <span data-ttu-id="2c773-862">另外，[CultureInfo.Parent](/dotnet/api/system.globalization.cultureinfo.parent) 屬性代表父文化特性。</span><span class="sxs-lookup"><span data-stu-id="2c773-862">As an aside, the [CultureInfo.Parent](/dotnet/api/system.globalization.cultureinfo.parent) property represents the parent culture.</span></span> <span data-ttu-id="2c773-863">這通常 (但並非一定) 表示從 ISO 中移除國家意符 (Signifier)。</span><span class="sxs-lookup"><span data-stu-id="2c773-863">This usually (but not always) means removing the national signifier from the ISO.</span></span> <span data-ttu-id="2c773-864">例如，墨西哥的西班牙文方言是 "es-MX"。</span><span class="sxs-lookup"><span data-stu-id="2c773-864">For example, the dialect of Spanish spoken in Mexico is "es-MX".</span></span> <span data-ttu-id="2c773-865">它具有父系 "es" &mdash; 西班牙文不是任何國家/地區的特定項目。</span><span class="sxs-lookup"><span data-stu-id="2c773-865">It has the parent "es"&mdash;Spanish non-specific to any country.</span></span>

<span data-ttu-id="2c773-866">假設您的網站收到使用文化特性 "fr-CA" 之 "Welcome" 資源的要求。</span><span class="sxs-lookup"><span data-stu-id="2c773-866">Imagine your site receives a request for a "Welcome" resource using culture "fr-CA".</span></span> <span data-ttu-id="2c773-867">當地語系化系統會依照順序尋找下列資源，並選取第一個相符項目：</span><span class="sxs-lookup"><span data-stu-id="2c773-867">The localization system looks for the following resources, in order, and selects the first match:</span></span>

* <span data-ttu-id="2c773-868">*Welcome.fr-CA.resx*</span><span class="sxs-lookup"><span data-stu-id="2c773-868">*Welcome.fr-CA.resx*</span></span>
* <span data-ttu-id="2c773-869">*Welcome.fr.resx*</span><span class="sxs-lookup"><span data-stu-id="2c773-869">*Welcome.fr.resx*</span></span>
* <span data-ttu-id="2c773-870">*Welcome.resx* (如果 `NeutralResourcesLanguage` 是 "fr-CA")</span><span class="sxs-lookup"><span data-stu-id="2c773-870">*Welcome.resx* (if the `NeutralResourcesLanguage` is "fr-CA")</span></span>

<span data-ttu-id="2c773-871">舉例來說，如果您移除 ".fr" 文化特性指示項，並將文化特性設定為法文，則系統會讀取預設資源檔，並將字串當地語系化。</span><span class="sxs-lookup"><span data-stu-id="2c773-871">As an example, if you remove the ".fr" culture designator and you have the culture set to French, the default resource file is read and strings are localized.</span></span> <span data-ttu-id="2c773-872">當沒有任何項目符合您要求的文化特性時，資源管理員即會指定預設資源或後援資源。</span><span class="sxs-lookup"><span data-stu-id="2c773-872">The Resource manager designates a default or fallback resource for when nothing meets your requested culture.</span></span> <span data-ttu-id="2c773-873">如果您只想在要求的文化特性缺少資源時傳回索引鍵，就不能使用預設資源檔。</span><span class="sxs-lookup"><span data-stu-id="2c773-873">If you want to just return the key when missing a resource for the requested culture you must not have a default resource file.</span></span>

### <a name="generate-resource-files-with-visual-studio"></a><span data-ttu-id="2c773-874">使用 Visual Studio 產生資源檔</span><span class="sxs-lookup"><span data-stu-id="2c773-874">Generate resource files with Visual Studio</span></span>

<span data-ttu-id="2c773-875">如果您在 Visual Studio 中建立資源檔，但檔案名稱中不含文化特性 (例如 *Welcome.resx*)，則 Visual Studio 會針對每個字串的屬性建立 C# 類別。</span><span class="sxs-lookup"><span data-stu-id="2c773-875">If you create a resource file in Visual Studio without a culture in the file name (for example, *Welcome.resx*), Visual Studio will create a C# class with a property for each string.</span></span> <span data-ttu-id="2c773-876">但這通常不是您使用 ASP.NET Core 的初衷。</span><span class="sxs-lookup"><span data-stu-id="2c773-876">That's usually not what you want with ASP.NET Core.</span></span> <span data-ttu-id="2c773-877">一般來說，您不會有預設的 *.resx* 資源檔 (不含文化特性名稱的 *.resx* 檔案)。</span><span class="sxs-lookup"><span data-stu-id="2c773-877">You typically don't have a default *.resx* resource file (a *.resx* file without the culture name).</span></span> <span data-ttu-id="2c773-878">因此，建議您建立含有文化特性名稱的 *.resx* 檔案 (例如 *Welcome.fr.resx*)。</span><span class="sxs-lookup"><span data-stu-id="2c773-878">We suggest you create the *.resx* file with a culture name (for example *Welcome.fr.resx*).</span></span> <span data-ttu-id="2c773-879">當您建立含有文化特性名稱的 *.resx* 檔案時，Visual Studio 就不會產生類別檔案。</span><span class="sxs-lookup"><span data-stu-id="2c773-879">When you create a *.resx* file with a culture name, Visual Studio won't generate the class file.</span></span>

### <a name="add-other-cultures"></a><span data-ttu-id="2c773-880">新增其他文化特性</span><span class="sxs-lookup"><span data-stu-id="2c773-880">Add other cultures</span></span>

<span data-ttu-id="2c773-881">每種語言和文化特性的組合 (非預設語言) 都需要唯一的資源檔。</span><span class="sxs-lookup"><span data-stu-id="2c773-881">Each language and culture combination (other than the default language) requires a unique resource file.</span></span> <span data-ttu-id="2c773-882">若要建立不同文化特性和地區設定的資源檔，您可以建立新的資源檔並將 ISO 語言代碼作為檔名的一部分 (例如 **en-us**、**fr-ca** 和 **en-gb**)。</span><span class="sxs-lookup"><span data-stu-id="2c773-882">You create resource files for different cultures and locales by creating new resource files in which the ISO language codes are part of the file name (for example, **en-us**, **fr-ca**, and **en-gb**).</span></span> <span data-ttu-id="2c773-883">您應將 ISO 代碼置於檔案名稱和 *.resx* 副檔名之間，例如 *Welcome.es-MX.resx* (西班牙文/墨西哥)。</span><span class="sxs-lookup"><span data-stu-id="2c773-883">These ISO codes are placed between the file name and the *.resx* file extension, as in *Welcome.es-MX.resx* (Spanish/Mexico).</span></span>

## <a name="implement-a-strategy-to-select-the-languageculture-for-each-request"></a><span data-ttu-id="2c773-884">實作可依據每項要求選取語言/文化特性的策略</span><span class="sxs-lookup"><span data-stu-id="2c773-884">Implement a strategy to select the language/culture for each request</span></span>

### <a name="configure-localization"></a><span data-ttu-id="2c773-885">設定當地語系化</span><span class="sxs-lookup"><span data-stu-id="2c773-885">Configure localization</span></span>

<span data-ttu-id="2c773-886">您可以在 `Startup.ConfigureServices` 方法中設定當地語系化：</span><span class="sxs-lookup"><span data-stu-id="2c773-886">Localization is configured in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](localization/sample/Localization/Startup.cs?name=snippet1)]

* <span data-ttu-id="2c773-887">`AddLocalization` 可將當地語系化服務新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="2c773-887">`AddLocalization` Adds the localization services to the services container.</span></span> <span data-ttu-id="2c773-888">上方的程式碼也會將資源路徑設為 "Resources"。</span><span class="sxs-lookup"><span data-stu-id="2c773-888">The code above also sets the resources path to "Resources".</span></span>

* <span data-ttu-id="2c773-889">`AddViewLocalization` 可支援當地語系化的檢視檔案。</span><span class="sxs-lookup"><span data-stu-id="2c773-889">`AddViewLocalization` Adds support for localized view files.</span></span> <span data-ttu-id="2c773-890">在此範例中，檢視的當地語系化會以檢視檔案的後置詞為依據。</span><span class="sxs-lookup"><span data-stu-id="2c773-890">In this sample view localization is based on the view file suffix.</span></span> <span data-ttu-id="2c773-891">例如 *Index.fr.cshtml* 檔案中的 "fr"。</span><span class="sxs-lookup"><span data-stu-id="2c773-891">For example "fr" in the *Index.fr.cshtml* file.</span></span>

* <span data-ttu-id="2c773-892">`AddDataAnnotationsLocalization` 可支援透過 `IStringLocalizer` 抽象概念而來的當地語系化 `DataAnnotations` 驗證訊息。</span><span class="sxs-lookup"><span data-stu-id="2c773-892">`AddDataAnnotationsLocalization` Adds support for localized `DataAnnotations` validation messages through `IStringLocalizer` abstractions.</span></span>

### <a name="localization-middleware"></a><span data-ttu-id="2c773-893">當地語系化中介軟體</span><span class="sxs-lookup"><span data-stu-id="2c773-893">Localization middleware</span></span>

<span data-ttu-id="2c773-894">您可以在當地語系化[中介軟體](xref:fundamentals/middleware/index)中，設定要求目前的文化特性。</span><span class="sxs-lookup"><span data-stu-id="2c773-894">The current culture on a request is set in the localization [Middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="2c773-895">已在 `Startup.Configure` 方法中啟用當地語系化中介軟體。</span><span class="sxs-lookup"><span data-stu-id="2c773-895">The localization middleware is enabled in the `Startup.Configure` method.</span></span> <span data-ttu-id="2c773-896">您必須在可能檢查要求文化特性的任何中介軟體之前，設定當地語系化中介軟體 (例如 `app.UseMvcWithDefaultRoute()`)。</span><span class="sxs-lookup"><span data-stu-id="2c773-896">The localization middleware must be configured before any middleware which might check the request culture (for example, `app.UseMvcWithDefaultRoute()`).</span></span>

[!code-csharp[](localization/sample/Localization/Startup.cs?name=snippet2)]
[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

<span data-ttu-id="2c773-897">`UseRequestLocalization` 會初始化 `RequestLocalizationOptions` 物件。</span><span class="sxs-lookup"><span data-stu-id="2c773-897">`UseRequestLocalization` initializes a `RequestLocalizationOptions` object.</span></span> <span data-ttu-id="2c773-898">在每次要求時，系統會列舉 `RequestLocalizationOptions` 中的 `RequestCultureProvider` 清單，並使用能成功判斷要求的文化特性的第一個提供者。</span><span class="sxs-lookup"><span data-stu-id="2c773-898">On every request the list of `RequestCultureProvider` in the `RequestLocalizationOptions` is enumerated and the first provider that can successfully determine the request culture is used.</span></span> <span data-ttu-id="2c773-899">預設的提供者是來自 `RequestLocalizationOptions` 類別：</span><span class="sxs-lookup"><span data-stu-id="2c773-899">The default providers come from the `RequestLocalizationOptions` class:</span></span>

1. `QueryStringRequestCultureProvider`
2. `CookieRequestCultureProvider`
3. `AcceptLanguageHeaderRequestCultureProvider`

<span data-ttu-id="2c773-900">預設清單會由針對性高到低來排列。</span><span class="sxs-lookup"><span data-stu-id="2c773-900">The default list goes from most specific to least specific.</span></span> <span data-ttu-id="2c773-901">在本文稍後，我們會說明如何變更順序，甚至新增自訂的文化特性提供者。</span><span class="sxs-lookup"><span data-stu-id="2c773-901">Later in the article we'll see how you can change the order and even add a custom culture provider.</span></span> <span data-ttu-id="2c773-902">如果沒有任何提供者可以判斷要求的文化特性，即會使用 `DefaultRequestCulture`。</span><span class="sxs-lookup"><span data-stu-id="2c773-902">If none of the providers can determine the request culture, the `DefaultRequestCulture` is used.</span></span>

### <a name="querystringrequestcultureprovider"></a><span data-ttu-id="2c773-903">QueryStringRequestCultureProvider</span><span class="sxs-lookup"><span data-stu-id="2c773-903">QueryStringRequestCultureProvider</span></span>

<span data-ttu-id="2c773-904">有些應用程式會使用查詢字串來設定[文化特性和 UI 文化特性](https://msdn.microsoft.com/library/system.globalization.cultureinfo.aspx)。</span><span class="sxs-lookup"><span data-stu-id="2c773-904">Some apps will use a query string to set the [culture and UI culture](https://msdn.microsoft.com/library/system.globalization.cultureinfo.aspx).</span></span> <span data-ttu-id="2c773-905">若是使用 Cookie 或 Accept-Language 標頭方法的應用程式，您可以將查詢字串新增至 URL 以偵錯和測試程式碼。</span><span class="sxs-lookup"><span data-stu-id="2c773-905">For apps that use the cookie or Accept-Language header approach, adding a query string to the URL is useful for debugging and testing code.</span></span> <span data-ttu-id="2c773-906">系統預設會將 `QueryStringRequestCultureProvider` 登錄為 `RequestCultureProvider` 清單中的第一個當地語系化提供者。</span><span class="sxs-lookup"><span data-stu-id="2c773-906">By default, the `QueryStringRequestCultureProvider` is registered as the first localization provider in the `RequestCultureProvider` list.</span></span> <span data-ttu-id="2c773-907">您應傳遞查詢字串參數 `culture` 和 `ui-culture`。</span><span class="sxs-lookup"><span data-stu-id="2c773-907">You pass the query string parameters `culture` and `ui-culture`.</span></span> <span data-ttu-id="2c773-908">下列範例會設定西班牙文/墨西哥的特定文化特性 (語言和地區)：</span><span class="sxs-lookup"><span data-stu-id="2c773-908">The following example sets the specific culture (language and region) to Spanish/Mexico:</span></span>

   `http://localhost:5000/?culture=es-MX&ui-culture=es-MX`

<span data-ttu-id="2c773-909">如果您只傳入兩者其一 (`culture` 或 `ui-culture`)，查詢字串提供者就會使用您傳入的項目來設定這兩個值。</span><span class="sxs-lookup"><span data-stu-id="2c773-909">If you only pass in one of the two (`culture` or `ui-culture`), the query string provider will set both values using the one you passed in.</span></span> <span data-ttu-id="2c773-910">例如，若只設定文化特性，即會同時設定 `Culture` 和 `UICulture`：</span><span class="sxs-lookup"><span data-stu-id="2c773-910">For example, setting just the culture will set both the `Culture` and the `UICulture`:</span></span>

   `http://localhost:5000/?culture=es-MX`

### <a name="cookierequestcultureprovider"></a><span data-ttu-id="2c773-911">CookieRequestCultureProvider</span><span class="sxs-lookup"><span data-stu-id="2c773-911">CookieRequestCultureProvider</span></span>

<span data-ttu-id="2c773-912">生產環境應用程式通常會提供一個機制，來設定 ASP.NET Core 文化特性 Cookie 的文化特性。</span><span class="sxs-lookup"><span data-stu-id="2c773-912">Production apps will often provide a mechanism to set the culture with the ASP.NET Core culture cookie.</span></span> <span data-ttu-id="2c773-913">若要建立 Cookie，請使用 `MakeCookieValue` 方法。</span><span class="sxs-lookup"><span data-stu-id="2c773-913">Use the `MakeCookieValue` method to create a cookie.</span></span>

<span data-ttu-id="2c773-914">會傳回 `CookieRequestCultureProvider` `DefaultCookieName` 用來追蹤使用者慣用文化特性資訊的預設 cookie 名稱。</span><span class="sxs-lookup"><span data-stu-id="2c773-914">The `CookieRequestCultureProvider` `DefaultCookieName` returns the default cookie name used to track the user's preferred culture information.</span></span> <span data-ttu-id="2c773-915">預設 Cookie 名稱為 `.AspNetCore.Culture`。</span><span class="sxs-lookup"><span data-stu-id="2c773-915">The default cookie name is `.AspNetCore.Culture`.</span></span>

<span data-ttu-id="2c773-916">Cookie 格式為 `c=%LANGCODE%|uic=%LANGCODE%`，其中 `c` 是 `Culture` 而 `uic` 是 `UICulture`，例如：</span><span class="sxs-lookup"><span data-stu-id="2c773-916">The cookie format is `c=%LANGCODE%|uic=%LANGCODE%`, where `c` is `Culture` and `uic` is `UICulture`, for example:</span></span>

    c=en-UK|uic=en-US

<span data-ttu-id="2c773-917">如果您只指定文化特性資訊和 UI 文化特性其一，系統就會將您所指定的文化特性用於文化特性資訊和 UI 文化特性。</span><span class="sxs-lookup"><span data-stu-id="2c773-917">If you only specify one of culture info and UI culture, the specified culture will be used for both culture info and UI culture.</span></span>

### <a name="the-accept-language-http-header"></a><span data-ttu-id="2c773-918">Accept-Language HTTP 標頭</span><span class="sxs-lookup"><span data-stu-id="2c773-918">The Accept-Language HTTP header</span></span>

<span data-ttu-id="2c773-919">您可在大多數的瀏覽器中設定 [Accept-Language 標頭](https://www.w3.org/International/questions/qa-accept-lang-locales)，其最初的設計目的是用來指定使用者的語言。</span><span class="sxs-lookup"><span data-stu-id="2c773-919">The [Accept-Language header](https://www.w3.org/International/questions/qa-accept-lang-locales) is settable in most browsers and was originally intended to specify the user's language.</span></span> <span data-ttu-id="2c773-920">這項設定可指出瀏覽器已設好要傳送哪些項目，或已從基礎作業系統繼承哪些項目。</span><span class="sxs-lookup"><span data-stu-id="2c773-920">This setting indicates what the browser has been set to send or has inherited from the underlying operating system.</span></span> <span data-ttu-id="2c773-921">透過瀏覽器要求的 Accept-Language HTTP 標頭來偵測使用者的慣用語言，並非萬無一失 (請參閱 [Setting language preferences in a browser](https://www.w3.org/International/questions/qa-lang-priorities.en.php) (在瀏覽器中設定語言喜好設定)。</span><span class="sxs-lookup"><span data-stu-id="2c773-921">The Accept-Language HTTP header from a browser request isn't an infallible way to detect the user's preferred language (see [Setting language preferences in a browser](https://www.w3.org/International/questions/qa-lang-priorities.en.php)).</span></span> <span data-ttu-id="2c773-922">生產環境應用程式應該包含可讓使用者自訂文化特性的選擇方式。</span><span class="sxs-lookup"><span data-stu-id="2c773-922">A production app should include a way for a user to customize their choice of culture.</span></span>

### <a name="set-the-accept-language-http-header-in-ie"></a><span data-ttu-id="2c773-923">在 IE 中設定 Accept-Language HTTP 標頭</span><span class="sxs-lookup"><span data-stu-id="2c773-923">Set the Accept-Language HTTP header in IE</span></span>

1. <span data-ttu-id="2c773-924">從齒輪圖示，點選 [網際網路選項]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="2c773-924">From the gear icon, tap **Internet Options**.</span></span>

2. <span data-ttu-id="2c773-925">點選 [語言]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="2c773-925">Tap **Languages**.</span></span>

    ![網際網路選項](localization/_static/lang.png)

3. <span data-ttu-id="2c773-927">點選 [設定語言喜好設定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="2c773-927">Tap **Set Language Preferences**.</span></span>

4. <span data-ttu-id="2c773-928">點選 [新增語言]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="2c773-928">Tap **Add a language**.</span></span>

5. <span data-ttu-id="2c773-929">新增語言。</span><span class="sxs-lookup"><span data-stu-id="2c773-929">Add the language.</span></span>

6. <span data-ttu-id="2c773-930">點選語言，然後點選 [上移]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="2c773-930">Tap the language, then tap **Move Up**.</span></span>

### <a name="the-content-language-http-header"></a><span data-ttu-id="2c773-931">內容語言 HTTP 標頭</span><span class="sxs-lookup"><span data-stu-id="2c773-931">The Content-Language HTTP header</span></span>

<span data-ttu-id="2c773-932">[內容語言](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Language)實體標頭：</span><span class="sxs-lookup"><span data-stu-id="2c773-932">The [Content-Language](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Language) entity header:</span></span>

 - <span data-ttu-id="2c773-933">用來描述適用于物件的語言。</span><span class="sxs-lookup"><span data-stu-id="2c773-933">Is used to describe the language(s) intended for the audience.</span></span>
 - <span data-ttu-id="2c773-934">可讓使用者根據使用者的慣用語言來區分。</span><span class="sxs-lookup"><span data-stu-id="2c773-934">Allows a user to differentiate according to the users' own preferred language.</span></span>

<span data-ttu-id="2c773-935">實體標頭會同時用於 HTTP 要求和回應。</span><span class="sxs-lookup"><span data-stu-id="2c773-935">Entity headers are used in both HTTP requests and responses.</span></span>

<span data-ttu-id="2c773-936">您 `Content-Language` 可以藉由設定屬性來新增標頭 `ApplyCurrentCultureToResponseHeaders` 。</span><span class="sxs-lookup"><span data-stu-id="2c773-936">The `Content-Language` header can be added by setting the property `ApplyCurrentCultureToResponseHeaders`.</span></span>

<span data-ttu-id="2c773-937">新增 `Content-Language` 標頭：</span><span class="sxs-lookup"><span data-stu-id="2c773-937">Adding the `Content-Language` header:</span></span>

 - <span data-ttu-id="2c773-938">允許 RequestLocalizationMiddleware 使用來設定 `Content-Language` 標頭 `CurrentUICulture` 。</span><span class="sxs-lookup"><span data-stu-id="2c773-938">Allows the RequestLocalizationMiddleware to set the `Content-Language` header with the `CurrentUICulture`.</span></span>
 - <span data-ttu-id="2c773-939">不需要明確地設定回應標頭 `Content-Language` 。</span><span class="sxs-lookup"><span data-stu-id="2c773-939">Eliminates the need to set the response header `Content-Language` explicitly.</span></span>

```csharp
app.UseRequestLocalization(new RequestLocalizationOptions
{
    ApplyCurrentCultureToResponseHeaders = true
});
```

### <a name="use-a-custom-provider"></a><span data-ttu-id="2c773-940">使用自訂提供者</span><span class="sxs-lookup"><span data-stu-id="2c773-940">Use a custom provider</span></span>

<span data-ttu-id="2c773-941">假設您想要讓客戶將他們的語言和文化特性儲存在您的資料庫中。</span><span class="sxs-lookup"><span data-stu-id="2c773-941">Suppose you want to let your customers store their language and culture in your databases.</span></span> <span data-ttu-id="2c773-942">您可以撰寫提供者，以供使用者查詢這些值。</span><span class="sxs-lookup"><span data-stu-id="2c773-942">You could write a provider to look up these values for the user.</span></span> <span data-ttu-id="2c773-943">下列程式碼會示範如何新增自訂提供者：</span><span class="sxs-lookup"><span data-stu-id="2c773-943">The following code shows how to add a custom provider:</span></span>

```csharp
private const string enUSCulture = "en-US";

services.Configure<RequestLocalizationOptions>(options =>
{
    var supportedCultures = new[]
    {
        new CultureInfo(enUSCulture),
        new CultureInfo("fr")
    };

    options.DefaultRequestCulture = new RequestCulture(culture: enUSCulture, uiCulture: enUSCulture);
    options.SupportedCultures = supportedCultures;
    options.SupportedUICultures = supportedCultures;

    options.AddInitialRequestCultureProvider(new CustomRequestCultureProvider(async context =>
    {
        // My custom request culture logic
        return new ProviderCultureResult("en");
    }));
});
```

<span data-ttu-id="2c773-944">使用 `RequestLocalizationOptions` 新增或移除當地語系化提供者。</span><span class="sxs-lookup"><span data-stu-id="2c773-944">Use `RequestLocalizationOptions` to add or remove localization providers.</span></span>

### <a name="set-the-culture-programmatically"></a><span data-ttu-id="2c773-945">以程式設計方式來設定文化特性</span><span class="sxs-lookup"><span data-stu-id="2c773-945">Set the culture programmatically</span></span>

<span data-ttu-id="2c773-946">[GitHub](https://github.com/aspnet/entropy) 上的這個範例 **Localization.StarterWeb** 專案包含可設定 `Culture` 的 UI。</span><span class="sxs-lookup"><span data-stu-id="2c773-946">This sample **Localization.StarterWeb** project on [GitHub](https://github.com/aspnet/entropy) contains UI to set the `Culture`.</span></span> <span data-ttu-id="2c773-947">*Views/Shared/_SelectLanguagePartial.cshtml* 檔可讓您從支援的文化特性清單中選取文化特性：</span><span class="sxs-lookup"><span data-stu-id="2c773-947">The *Views/Shared/_SelectLanguagePartial.cshtml* file allows you to select the culture from the list of supported cultures:</span></span>

[!code-cshtml[](localization/sample/Localization/Views/Shared/_SelectLanguagePartial.cshtml)]

<span data-ttu-id="2c773-948">系統會將 *Views/Shared/_SelectLanguagePartial.cshtml* 檔案新增至配置檔案的 `footer` 區段，以供所有檢視使用：</span><span class="sxs-lookup"><span data-stu-id="2c773-948">The *Views/Shared/_SelectLanguagePartial.cshtml* file is added to the `footer` section of the layout file so it will be available to all views:</span></span>

[!code-cshtml[](localization/sample/Localization/Views/Shared/_Layout.cshtml?range=43-56&highlight=10)]

<span data-ttu-id="2c773-949">`SetLanguage` 方法會設定文化特性的 Cookie。</span><span class="sxs-lookup"><span data-stu-id="2c773-949">The `SetLanguage` method sets the culture cookie.</span></span>

[!code-csharp[](localization/sample/Localization/Controllers/HomeController.cs?range=57-67)]

<span data-ttu-id="2c773-950">您無法將 *_SelectLanguagePartial.cshtml* 插入這個專案的範例程式碼。</span><span class="sxs-lookup"><span data-stu-id="2c773-950">You can't plug in the *_SelectLanguagePartial.cshtml* to sample code for this project.</span></span> <span data-ttu-id="2c773-951">[GitHub](https://github.com/aspnet/entropy)上的**localization.starterweb**專案具有程式碼，可透過相依性 `RequestLocalizationOptions` 插入容器將傳遞至 Razor 部分。 [Dependency Injection](dependency-injection.md)</span><span class="sxs-lookup"><span data-stu-id="2c773-951">The **Localization.StarterWeb** project on [GitHub](https://github.com/aspnet/entropy) has code to flow the `RequestLocalizationOptions` to a Razor partial through the [Dependency Injection](dependency-injection.md) container.</span></span>

## <a name="model-binding-route-data-and-query-strings"></a><span data-ttu-id="2c773-952">模型系結路由資料和查詢字串</span><span class="sxs-lookup"><span data-stu-id="2c773-952">Model binding route data and query strings</span></span>

<span data-ttu-id="2c773-953">請參閱模型系結[路由資料和查詢字串的全球化行為](xref:mvc/models/model-binding#glob)。</span><span class="sxs-lookup"><span data-stu-id="2c773-953">See [Globalization behavior of model binding route data and query strings](xref:mvc/models/model-binding#glob).</span></span>

## <a name="globalization-and-localization-terms"></a><span data-ttu-id="2c773-954">全球化和當地語系化詞彙</span><span class="sxs-lookup"><span data-stu-id="2c773-954">Globalization and localization terms</span></span>

<span data-ttu-id="2c773-955">在進行應用程式的當地語系化程序時，您也需要具備現代軟體開發中常用的相關字元集基本知識，並了解與它們建立關聯的問題。</span><span class="sxs-lookup"><span data-stu-id="2c773-955">The process of localizing your app also requires a basic understanding of relevant character sets commonly used in modern software development and an understanding of the issues associated with them.</span></span> <span data-ttu-id="2c773-956">雖然所有電腦都會將文字儲存為數字 (程式碼)，但不同系統會使用不同數字來儲存相同的文字。</span><span class="sxs-lookup"><span data-stu-id="2c773-956">Although all computers store text as numbers (codes), different systems store the same text using different numbers.</span></span> <span data-ttu-id="2c773-957">當地語系化是指針對特定文化特性/地區設定，轉譯應用程式使用者介面 (UI) 的程序。</span><span class="sxs-lookup"><span data-stu-id="2c773-957">The localization process refers to translating the app user interface (UI) for a specific culture/locale.</span></span>

<span data-ttu-id="2c773-958">[可當地語系化](/dotnet/standard/globalization-localization/localizability-review)是確認全球化應用程式已準備好進行當地語系化的中繼程序。</span><span class="sxs-lookup"><span data-stu-id="2c773-958">[Localizability](/dotnet/standard/globalization-localization/localizability-review) is an intermediate process for verifying that a globalized app is ready for localization.</span></span>

<span data-ttu-id="2c773-959">文化特性名稱的 [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) 格式是 `<languagecode2>-<country/regioncode2>`，其中 `<languagecode2>` 是語言代碼，而 `<country/regioncode2>` 是子文化特性代碼。</span><span class="sxs-lookup"><span data-stu-id="2c773-959">The [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) format for the culture name is `<languagecode2>-<country/regioncode2>`, where `<languagecode2>` is the language code and `<country/regioncode2>` is the subculture code.</span></span> <span data-ttu-id="2c773-960">例如，`es-CL` 是指西班牙文 (智利)，`en-US` 是指英文 (美國)，而 `en-AU` 是指英文 (澳大利亞)。</span><span class="sxs-lookup"><span data-stu-id="2c773-960">For example, `es-CL` for Spanish (Chile), `en-US` for English (United States), and `en-AU` for English (Australia).</span></span> <span data-ttu-id="2c773-961">[RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) 是 ISO 639 (兩個小寫字母，為與某個語言建立關聯的文化特性代碼) 及 ISO 3166 (兩個大寫字母，為與某個國家或地區建立關聯的子文化特性代碼) 的組合。</span><span class="sxs-lookup"><span data-stu-id="2c773-961">[RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) is a combination of an ISO 639 two-letter lowercase culture code associated with a language and an ISO 3166 two-letter uppercase subculture code associated with a country or region.</span></span> <span data-ttu-id="2c773-962">請參閱 [Language Culture Name](https://msdn.microsoft.com/library/ee825488(v=cs.20).aspx) (語言的文化特性名稱)。</span><span class="sxs-lookup"><span data-stu-id="2c773-962">See [Language Culture Name](https://msdn.microsoft.com/library/ee825488(v=cs.20).aspx).</span></span>

<span data-ttu-id="2c773-963">國際化通常縮寫為 "I18N"。</span><span class="sxs-lookup"><span data-stu-id="2c773-963">Internationalization is often abbreviated to "I18N".</span></span> <span data-ttu-id="2c773-964">這個縮寫是採用該詞彙的第一個和最後一個字母，以及這兩個字母間的字母數組成，因此 18 代表第一個字母 "I" 及最後 "N" 中間的字母數。</span><span class="sxs-lookup"><span data-stu-id="2c773-964">The abbreviation takes the first and last letters and the number of letters between them, so 18 stands for the number of letters between the first "I" and the last "N".</span></span> <span data-ttu-id="2c773-965">同樣的原則也適用於全球化 (G11N) 與當地語系化 (L10N) 的縮寫。</span><span class="sxs-lookup"><span data-stu-id="2c773-965">The same applies to Globalization (G11N), and Localization (L10N).</span></span>

<span data-ttu-id="2c773-966">詞彙：</span><span class="sxs-lookup"><span data-stu-id="2c773-966">Terms:</span></span>

* <span data-ttu-id="2c773-967">全球化 (G11N)：讓應用程式支援不同語言和區域的程序。</span><span class="sxs-lookup"><span data-stu-id="2c773-967">Globalization (G11N): The process of making an app support different languages and regions.</span></span>
* <span data-ttu-id="2c773-968">當地語系化 (L10N)：針對特定語言和區域，自訂應用程式的程序。</span><span class="sxs-lookup"><span data-stu-id="2c773-968">Localization (L10N): The process of customizing an app for a given language and region.</span></span>
* <span data-ttu-id="2c773-969">國際化 (I18N)：包含全球化和當地語系化這兩部分的描述。</span><span class="sxs-lookup"><span data-stu-id="2c773-969">Internationalization (I18N): Describes both globalization and localization.</span></span>
* <span data-ttu-id="2c773-970">文化特性：它是一種語言和/或地區。</span><span class="sxs-lookup"><span data-stu-id="2c773-970">Culture: It's a language and, optionally, a region.</span></span>
* <span data-ttu-id="2c773-971">中性文化特性：具有指定的語言但不限地區的文化特性 </span><span class="sxs-lookup"><span data-stu-id="2c773-971">Neutral culture: A culture that has a specified language, but not a region.</span></span> <span data-ttu-id="2c773-972">(例如 "en"、"es")。</span><span class="sxs-lookup"><span data-stu-id="2c773-972">(for example "en", "es")</span></span>
* <span data-ttu-id="2c773-973">特定文化特性：具有指定的語言和地區的文化特性 </span><span class="sxs-lookup"><span data-stu-id="2c773-973">Specific culture: A culture that has a specified language and region.</span></span> <span data-ttu-id="2c773-974">(例如 "en-US"、"en-GB"、"es-CL")。</span><span class="sxs-lookup"><span data-stu-id="2c773-974">(for example "en-US", "en-GB", "es-CL")</span></span>
* <span data-ttu-id="2c773-975">父文化特性：包含特定文化特性的中性文化特性 </span><span class="sxs-lookup"><span data-stu-id="2c773-975">Parent culture: The neutral culture that contains a specific culture.</span></span> <span data-ttu-id="2c773-976">(例如，"en" 是 "en-US" 和 "en-GB" 的父文化特性)。</span><span class="sxs-lookup"><span data-stu-id="2c773-976">(for example, "en" is the parent culture of "en-US" and "en-GB")</span></span>
* <span data-ttu-id="2c773-977">地區設定：地區設定與文化特性相同。</span><span class="sxs-lookup"><span data-stu-id="2c773-977">Locale: A locale is the same as a culture.</span></span>

[!INCLUDE[](~/includes/localization/currency.md)]

[!INCLUDE[](~/includes/localization/unsupported-culture-log-level.md)]

## <a name="additional-resources"></a><span data-ttu-id="2c773-978">其他資源</span><span class="sxs-lookup"><span data-stu-id="2c773-978">Additional resources</span></span>

* <xref:fundamentals/troubleshoot-aspnet-core-localization>
* <span data-ttu-id="2c773-979">本文使用的 [Localization.StarterWeb 專案](https://github.com/aspnet/Entropy/tree/master/samples/Localization.StarterWeb)。</span><span class="sxs-lookup"><span data-stu-id="2c773-979">[Localization.StarterWeb project](https://github.com/aspnet/Entropy/tree/master/samples/Localization.StarterWeb) used in the article.</span></span>
* [<span data-ttu-id="2c773-980">全球化與當地語系化 .NET 應用程式</span><span class="sxs-lookup"><span data-stu-id="2c773-980">Globalizing and localizing .NET applications</span></span>](/dotnet/standard/globalization-localization/index)
* [<span data-ttu-id="2c773-981">.Resx 檔案中的資源</span><span class="sxs-lookup"><span data-stu-id="2c773-981">Resources in .resx Files</span></span>](/dotnet/framework/resources/working-with-resx-files-programmatically)
* [<span data-ttu-id="2c773-982">Microsoft 多語應用程式工具組</span><span class="sxs-lookup"><span data-stu-id="2c773-982">Microsoft Multilingual App Toolkit</span></span>](https://marketplace.visualstudio.com/items?itemName=MultilingualAppToolkit.MultilingualAppToolkit-18308)
* [<span data-ttu-id="2c773-983">當地語系化和泛型</span><span class="sxs-lookup"><span data-stu-id="2c773-983">Localization & Generics</span></span>](http://hishambinateya.com/localization-and-generics)

::: moniker-end
