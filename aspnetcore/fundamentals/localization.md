---
<span data-ttu-id="3b0c0-101">標題： ASP.NET Core author 中的全球化和當地語系化： rick-anderson 描述：瞭解 ASP.NET Core 如何提供服務和中介軟體，將內容當地語系化成不同的語言和文化特性。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-101">title: Globalization and localization in ASP.NET Core author: rick-anderson description: Learn how ASP.NET Core provides services and middleware for localizing content into different languages and cultures.</span></span>
<span data-ttu-id="3b0c0-102">riande ms. date：11/30/2019 否-loc：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-102">ms.author: riande ms.date: 11/30/2019 no-loc:</span></span>
- <span data-ttu-id="3b0c0-103">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="3b0c0-103">'Blazor'</span></span>
- <span data-ttu-id="3b0c0-104">'Identity'</span><span class="sxs-lookup"><span data-stu-id="3b0c0-104">'Identity'</span></span>
- <span data-ttu-id="3b0c0-105">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="3b0c0-105">'Let's Encrypt'</span></span>
- <span data-ttu-id="3b0c0-106">'Razor'</span><span class="sxs-lookup"><span data-stu-id="3b0c0-106">'Razor'</span></span>
- <span data-ttu-id="3b0c0-107">' SignalR ' uid：基本概念/當地語系化</span><span class="sxs-lookup"><span data-stu-id="3b0c0-107">'SignalR' uid: fundamentals/localization</span></span>

---
# <a name="globalization-and-localization-in-aspnet-core"></a><span data-ttu-id="3b0c0-108">ASP.NET Core 全球化和當地語系化</span><span class="sxs-lookup"><span data-stu-id="3b0c0-108">Globalization and localization in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0 < aspnetcore-5.0"

<span data-ttu-id="3b0c0-109">由 [Rick Anderson](https://twitter.com/RickAndMSFT)、[Damien Bowden](https://twitter.com/damien_bod)、[Bart Calixto](https://twitter.com/bartmax)、[Nadeem Afana](https://afana.me/) 和 [Hisham Bin Ateya](https://twitter.com/hishambinateya) 提供</span><span class="sxs-lookup"><span data-stu-id="3b0c0-109">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Damien Bowden](https://twitter.com/damien_bod), [Bart Calixto](https://twitter.com/bartmax), [Nadeem Afana](https://afana.me/), and [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span></span>

<span data-ttu-id="3b0c0-110">多語系網站可讓網站觸及更多物件。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-110">A multilingual website allows the site to reach a wider audience.</span></span> <span data-ttu-id="3b0c0-111">ASP.NET Core 提供服務與中介軟體，可將網站當地語系化成不同的語言與文化特性。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-111">ASP.NET Core provides services and middleware for localizing into different languages and cultures.</span></span>

<span data-ttu-id="3b0c0-112">國際化包含[全球化](/dotnet/api/system.globalization)和[當地語系化](/dotnet/standard/globalization-localization/localization)這兩部分。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-112">Internationalization involves [Globalization](/dotnet/api/system.globalization) and [Localization](/dotnet/standard/globalization-localization/localization).</span></span> <span data-ttu-id="3b0c0-113">全球化是指設計出可支援不同文化特性之應用程式的程序。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-113">Globalization is the process of designing apps that support different cultures.</span></span> <span data-ttu-id="3b0c0-114">透過全球化，您可新增支援與特定地區相關之已定義語言指令碼的輸入、顯示及輸出作業。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-114">Globalization adds support for input, display, and output of a defined set of language scripts that relate to specific geographic areas.</span></span>

<span data-ttu-id="3b0c0-115">當地語系化是指針對全球化應用程式進行調整的程序，且您已順應特定文化特性/地區設定對這些全球化應用程式進行可當地語系化處理。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-115">Localization is the process of adapting a globalized app, which you have already processed for localizability, to a particular culture/locale.</span></span> <span data-ttu-id="3b0c0-116">如需詳細資訊，請參閱本文件結尾處的**全球化和當地語系化詞彙**。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-116">For more information see **Globalization and localization terms** near the end of this document.</span></span>

<span data-ttu-id="3b0c0-117">應用程式當地語系化包含下列作業：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-117">App localization involves the following:</span></span>

1. <span data-ttu-id="3b0c0-118">讓應用程式的內容可當地語系化</span><span class="sxs-lookup"><span data-stu-id="3b0c0-118">Make the app's content localizable</span></span>
1. <span data-ttu-id="3b0c0-119">針對您支援的語言和文化特性提供當地語系化資源</span><span class="sxs-lookup"><span data-stu-id="3b0c0-119">Provide localized resources for the languages and cultures you support</span></span>
1. <span data-ttu-id="3b0c0-120">實作可依據每項要求選取語言/文化特性的策略</span><span class="sxs-lookup"><span data-stu-id="3b0c0-120">Implement a strategy to select the language/culture for each request</span></span>

<span data-ttu-id="3b0c0-121">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/localization/sample/Localization)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="3b0c0-121">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/localization/sample/Localization) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="make-the-apps-content-localizable"></a><span data-ttu-id="3b0c0-122">讓應用程式的內容可當地語系化</span><span class="sxs-lookup"><span data-stu-id="3b0c0-122">Make the app's content localizable</span></span>

<span data-ttu-id="3b0c0-123"><xref:Microsoft.Extensions.Localization.IStringLocalizer>和 <xref:Microsoft.Extensions.Localization.IStringLocalizer%601> 已架構，可在開發當地語系化應用程式時改善生產力。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-123"><xref:Microsoft.Extensions.Localization.IStringLocalizer> and <xref:Microsoft.Extensions.Localization.IStringLocalizer%601> were architected to improve productivity when developing localized apps.</span></span> <span data-ttu-id="3b0c0-124">`IStringLocalizer`會使用 <xref:System.Resources.ResourceManager> 和， <xref:System.Resources.ResourceReader> 在執行時間提供特定文化特性的資源。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-124">`IStringLocalizer` uses the <xref:System.Resources.ResourceManager> and <xref:System.Resources.ResourceReader> to provide culture-specific resources at run time.</span></span> <span data-ttu-id="3b0c0-125">介面具有索引子和， `IEnumerable` 用於傳回當地語系化的字串。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-125">The interface has an indexer and an `IEnumerable` for returning localized strings.</span></span> <span data-ttu-id="3b0c0-126">`IStringLocalizer`不需要將預設語言字串儲存在資源檔中。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-126">`IStringLocalizer` doesn't require storing the default language strings in a resource file.</span></span> <span data-ttu-id="3b0c0-127">您不必在開發初期建立資源檔，即可開發以當地語系化為目標的應用程式。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-127">You can develop an app targeted for localization and not need to create resource files early in development.</span></span> <span data-ttu-id="3b0c0-128">下列程式碼會示範如何包裝 "About Title" 字串以進行當地語系化。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-128">The code below shows how to wrap the string "About Title" for localization.</span></span>

[!code-csharp[](localization/sample/Localization/Controllers/AboutController.cs)]

<span data-ttu-id="3b0c0-129">在上述程式碼中， `IStringLocalizer<T>` 執行是來自相依性[插入](dependency-injection.md)。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-129">In the preceding code, the `IStringLocalizer<T>` implementation comes from [Dependency Injection](dependency-injection.md).</span></span> <span data-ttu-id="3b0c0-130">如果找不到 "About Title" 的當地語系化值，即會傳回索引子的索引鍵，也就是 "About Title" 字串。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-130">If the localized value of "About Title" isn't found, then the indexer key is returned, that is, the string "About Title".</span></span> <span data-ttu-id="3b0c0-131">您可以保留應用程式中的預設語言常值字串，並將其包裝在當地語系化工具中，以便專注於開發應用程式。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-131">You can leave the default language literal strings in the app and wrap them in the localizer, so that you can focus on developing the app.</span></span> <span data-ttu-id="3b0c0-132">您不用先建立預設資源檔，即可使用預設語言來開發應用程式，並針對當地語系化步驟進行應用程式的準備。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-132">You develop your app with your default language and prepare it for the localization step without first creating a default resource file.</span></span> <span data-ttu-id="3b0c0-133">或者，您可以使用傳統方法，並提供索引鍵以擷取預設語言字串。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-133">Alternatively, you can use the traditional approach and provide a key to retrieve the default language string.</span></span> <span data-ttu-id="3b0c0-134">對許多開發人員來說，新的工作流程 (單純包裝字串常值而不使用預設語言的 *.resx* 檔案) 可以降低當地語系化應用程式的額外負荷。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-134">For many developers the new workflow of not having a default language *.resx* file and simply wrapping the string literals can reduce the overhead of localizing an app.</span></span> <span data-ttu-id="3b0c0-135">其他開發人員則偏好傳統的工作流程，因為這種方法更便於使用較長的字串常值，且更易於更新當地語系化的字串。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-135">Other developers will prefer the traditional work flow as it can make it easier to work with longer string literals and make it easier to update localized strings.</span></span>

<span data-ttu-id="3b0c0-136">若是包含 HTML 的資源，請使用 `IHtmlLocalizer<T>` 實作。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-136">Use the `IHtmlLocalizer<T>` implementation for resources that contain HTML.</span></span> <span data-ttu-id="3b0c0-137">`IHtmlLocalizer` 會對資源字串中經過格式化的引數進行 HTML 編碼，但不會對資源字串本身進行 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-137">`IHtmlLocalizer` HTML encodes arguments that are formatted in the resource string, but doesn't HTML encode the resource string itself.</span></span> <span data-ttu-id="3b0c0-138">在下列醒目提示的範例中，只有 `name` 參數的值經過 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-138">In the sample highlighted below, only the value of `name` parameter is HTML encoded.</span></span>

[!code-csharp[](../fundamentals/localization/sample/Localization/Controllers/BookController.cs?highlight=3,5,20&start=1&end=24)]

<span data-ttu-id="3b0c0-139">**注意：** 一般來說，您只想將文字當地語系化，而不是 HTML。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-139">**Note:** You generally want to only localize text and not HTML.</span></span>

<span data-ttu-id="3b0c0-140">您可以在最底層的[相依性插入](dependency-injection.md)中，將 `IStringLocalizerFactory` 移出：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-140">At the lowest level, you can get `IStringLocalizerFactory` out of [Dependency Injection](dependency-injection.md):</span></span>

[!code-csharp[](localization/sample/Localization/Controllers/TestController.cs?start=9&end=26&highlight=7-13)]

<span data-ttu-id="3b0c0-141">上述程式碼會示範這兩個 Factory Create 方法。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-141">The code above demonstrates each of the two factory create methods.</span></span>

<span data-ttu-id="3b0c0-142">您可以依據控制器或區域來分割當地語系化的字串，也可以只用一個容器。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-142">You can partition your localized strings by controller, area, or have just one container.</span></span> <span data-ttu-id="3b0c0-143">在範例應用程式中，會針對共用資源使用名為 `SharedResource` 的虛擬類別。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-143">In the sample app, a dummy class named `SharedResource` is used for shared resources.</span></span>

[!code-csharp[](localization/sample/Localization/Resources/SharedResource.cs)]

<span data-ttu-id="3b0c0-144">有些開發人員會使用 `Startup` 類別來包含全域或共用字串。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-144">Some developers use the `Startup` class to contain global or shared strings.</span></span> <span data-ttu-id="3b0c0-145">下列範例會使用 `InfoController` 和 `SharedResource` 當地語系化工具：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-145">In the sample below, the `InfoController` and the `SharedResource` localizers are used:</span></span>

[!code-csharp[](localization/sample/Localization/Controllers/InfoController.cs?range=9-26)]

## <a name="view-localization"></a><span data-ttu-id="3b0c0-146">檢視當地語系化</span><span class="sxs-lookup"><span data-stu-id="3b0c0-146">View localization</span></span>

<span data-ttu-id="3b0c0-147">`IViewLocalizer` 服務可提供[檢視](xref:mvc/views/overview)的當地語系化字串。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-147">The `IViewLocalizer` service provides localized strings for a [view](xref:mvc/views/overview).</span></span> <span data-ttu-id="3b0c0-148">`ViewLocalizer` 類別會實作這個介面，並透過檢視檔案路徑來找出資源的位置。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-148">The `ViewLocalizer` class implements this interface and finds the resource location from the view file path.</span></span> <span data-ttu-id="3b0c0-149">下列程式碼示範如何使用 `IViewLocalizer` 的預設實作：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-149">The following code shows how to use the default implementation of `IViewLocalizer`:</span></span>

[!code-cshtml[](localization/sample/Localization/Views/Home/About.cshtml)]

<span data-ttu-id="3b0c0-150">`IViewLocalizer` 的預設實作會依據檢視的檔案名稱來找出資源檔。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-150">The default implementation of `IViewLocalizer` finds the resource file based on the view's file name.</span></span> <span data-ttu-id="3b0c0-151">其中並沒有任何選項可以使用全域共用的資源檔。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-151">There's no option to use a global shared resource file.</span></span> <span data-ttu-id="3b0c0-152">`ViewLocalizer`使用來執行當地語系化工具 `IHtmlLocalizer` ，因此 Razor 不會對當地語系化字串進行 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-152">`ViewLocalizer` implements the localizer using `IHtmlLocalizer`, so Razor doesn't HTML encode the localized string.</span></span> <span data-ttu-id="3b0c0-153">您可以參數化資源字串，`IViewLocalizer` 即會對參數 (而不是資源字串) 進行 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-153">You can parameterize resource strings and `IViewLocalizer` will HTML encode the parameters, but not the resource string.</span></span> <span data-ttu-id="3b0c0-154">請考慮下列 Razor 標記：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-154">Consider the following Razor markup:</span></span>

```cshtml
@Localizer["<i>Hello</i> <b>{0}!</b>", UserManager.GetUserName(User)]
```

<span data-ttu-id="3b0c0-155">法文資源檔可能包含下列內容：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-155">A French resource file could contain the following:</span></span>

| <span data-ttu-id="3b0c0-156">Key</span><span class="sxs-lookup"><span data-stu-id="3b0c0-156">Key</span></span> | <span data-ttu-id="3b0c0-157">值</span><span class="sxs-lookup"><span data-stu-id="3b0c0-157">Value</span></span> |
| ----- | ------ |
| `<i>Hello</i> <b>{0}!</b>` | `<i>Bonjour</i> <b>{0} !</b>` |

<span data-ttu-id="3b0c0-158">轉譯的檢視內容可能包含來自資源檔的 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-158">The rendered view would contain the HTML markup from the resource file.</span></span>

<span data-ttu-id="3b0c0-159">**注意：** 一般來說，您只想將文字當地語系化，而不是 HTML。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-159">**Note:** You generally want to only localize text and not HTML.</span></span>

<span data-ttu-id="3b0c0-160">若要在檢視中使用共用的資源檔，請插入 `IHtmlLocalizer<T>`：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-160">To use a shared resource file in a view, inject `IHtmlLocalizer<T>`:</span></span>

[!code-cshtml[](../fundamentals/localization/sample/Localization/Views/Test/About.cshtml?highlight=5,12)]

## <a name="dataannotations-localization"></a><span data-ttu-id="3b0c0-161">DataAnnotations 當地語系化</span><span class="sxs-lookup"><span data-stu-id="3b0c0-161">DataAnnotations localization</span></span>

<span data-ttu-id="3b0c0-162">DataAnnotations 錯誤訊息會使用 `IStringLocalizer<T>` 來當地語系化。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-162">DataAnnotations error messages are localized with `IStringLocalizer<T>`.</span></span> <span data-ttu-id="3b0c0-163">使用 `ResourcesPath = "Resources"` 選項時，`RegisterViewModel` 中的錯誤訊息會儲存在下列路徑之一：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-163">Using the option `ResourcesPath = "Resources"`, the error messages in `RegisterViewModel` can be stored in either of the following paths:</span></span>

* <span data-ttu-id="3b0c0-164">*Resources/Viewmodel. RegisterViewModel. fr .resx*</span><span class="sxs-lookup"><span data-stu-id="3b0c0-164">*Resources/ViewModels.Account.RegisterViewModel.fr.resx*</span></span>
* <span data-ttu-id="3b0c0-165">*Resources/ViewModels/Account/RegisterViewModel.fr.resx*</span><span class="sxs-lookup"><span data-stu-id="3b0c0-165">*Resources/ViewModels/Account/RegisterViewModel.fr.resx*</span></span>

[!code-csharp[](localization/sample/Localization/ViewModels/Account/RegisterViewModel.cs?start=9&end=26)]

<span data-ttu-id="3b0c0-166">在 ASP.NET Core MVC 1.1.0 和更高版本中，系統會將非驗證屬性當地語系化。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-166">In ASP.NET Core MVC 1.1.0 and higher, non-validation attributes are localized.</span></span> <span data-ttu-id="3b0c0-167">ASP.NET Core MVC 1.0 **不會**查閱非驗證屬性的當地語系化字串。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-167">ASP.NET Core MVC 1.0 does **not** look up localized strings for non-validation attributes.</span></span>

<a name="one-resource-string-multiple-classes"></a>

### <a name="using-one-resource-string-for-multiple-classes"></a><span data-ttu-id="3b0c0-168">針對多個類別使用同一個資源字串</span><span class="sxs-lookup"><span data-stu-id="3b0c0-168">Using one resource string for multiple classes</span></span>

<span data-ttu-id="3b0c0-169">下列程式碼會示範如何針對含有多個類別的驗證屬性使用同一個資源字串：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-169">The following code shows how to use one resource string for validation attributes with multiple classes:</span></span>

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

<span data-ttu-id="3b0c0-170">在先前的程式碼中，`SharedResource` 是與儲存驗證訊息的 resx 對應的類別。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-170">In the preceding code, `SharedResource` is the class corresponding to the resx where your validation messages are stored.</span></span> <span data-ttu-id="3b0c0-171">使用這個方法時，DataAnnotations 只會使用 `SharedResource`，而不是每個類別的資源。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-171">With this approach, DataAnnotations will only use `SharedResource`, rather than the resource for each class.</span></span>

## <a name="provide-localized-resources-for-the-languages-and-cultures-you-support"></a><span data-ttu-id="3b0c0-172">針對您支援的語言和文化特性提供當地語系化資源</span><span class="sxs-lookup"><span data-stu-id="3b0c0-172">Provide localized resources for the languages and cultures you support</span></span>

### <a name="supportedcultures-and-supporteduicultures"></a><span data-ttu-id="3b0c0-173">SupportedCultures 和 SupportedUICultures</span><span class="sxs-lookup"><span data-stu-id="3b0c0-173">SupportedCultures and SupportedUICultures</span></span>

<span data-ttu-id="3b0c0-174">ASP.NET Core 可讓您指定 `SupportedCultures` 和 `SupportedUICultures` 這兩個文化特性值。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-174">ASP.NET Core allows you to specify two culture values, `SupportedCultures` and `SupportedUICultures`.</span></span> <span data-ttu-id="3b0c0-175">`SupportedCultures` 的 [CultureInfo](/dotnet/api/system.globalization.cultureinfo) 物件可決定文化特性相依函式的結果，例如日期、時間、數字及貨幣格式。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-175">The [CultureInfo](/dotnet/api/system.globalization.cultureinfo) object for `SupportedCultures` determines the results of culture-dependent functions, such as date, time, number, and currency formatting.</span></span> <span data-ttu-id="3b0c0-176">`SupportedCultures` 也可決定文字排列順序、大小寫慣例和字串比較。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-176">`SupportedCultures` also determines the sorting order of text, casing conventions, and string comparisons.</span></span> <span data-ttu-id="3b0c0-177">如需伺服器如何取得文化特性的詳細資訊，請參閱 [CultureInfo.CurrentCulture](/dotnet/api/system.stringcomparer.currentculture#System_StringComparer_CurrentCulture)。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-177">See [CultureInfo.CurrentCulture](/dotnet/api/system.stringcomparer.currentculture#System_StringComparer_CurrentCulture) for more info on how the server gets the Culture.</span></span> <span data-ttu-id="3b0c0-178">`SupportedUICultures`會決定[ResourceManager](/dotnet/api/system.resources.resourcemanager)會查閱哪些翻譯的字串（來自 *.resx*檔案）。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-178">The `SupportedUICultures` determines which translated strings (from *.resx* files) are looked up by the [ResourceManager](/dotnet/api/system.resources.resourcemanager).</span></span> <span data-ttu-id="3b0c0-179">`ResourceManager` 僅會查閱 `CurrentUICulture` 所決定之文化特性特有的字串。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-179">The `ResourceManager` simply looks up culture-specific strings that's determined by `CurrentUICulture`.</span></span> <span data-ttu-id="3b0c0-180">.NET 中的每個執行緒都有 `CurrentCulture` 和 `CurrentUICulture` 物件。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-180">Every thread in .NET has `CurrentCulture` and `CurrentUICulture` objects.</span></span> <span data-ttu-id="3b0c0-181">ASP.NET Core 會在轉譯文化特性相依函式時檢查這些值。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-181">ASP.NET Core inspects these values when rendering culture-dependent functions.</span></span> <span data-ttu-id="3b0c0-182">比方說，如果目前執行緒的文化特性設定為 "en-US" (英文 - 美國)，`DateTime.Now.ToLongDateString()` 會顯示 "Thursday, February 18, 2016"，但如果 `CurrentCulture` 設定為 "es-ES" (西班牙文 - 西班牙)，則輸出會是 "jueves, 18 de febrero de 2016"。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-182">For example, if the current thread's culture is set to "en-US" (English, United States), `DateTime.Now.ToLongDateString()` displays "Thursday, February 18, 2016", but if `CurrentCulture` is set to "es-ES" (Spanish, Spain) the output will be "jueves, 18 de febrero de 2016".</span></span>

## <a name="resource-files"></a><span data-ttu-id="3b0c0-183">資源檔</span><span class="sxs-lookup"><span data-stu-id="3b0c0-183">Resource files</span></span>

<span data-ttu-id="3b0c0-184">資源檔是一種實用的機制，可讓您將可當地語系化的字串與代碼區隔開來。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-184">A resource file is a useful mechanism for separating localizable strings from code.</span></span> <span data-ttu-id="3b0c0-185">非預設語言的翻譯字串會在 *.resx*資源檔中隔離。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-185">Translated strings for the non-default language are isolated in *.resx* resource files.</span></span> <span data-ttu-id="3b0c0-186">例如，您可以建立名為 *Welcome.es.resx* 的西班牙文資源檔，以包含翻譯的字串。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-186">For example, you might want to create Spanish resource file named *Welcome.es.resx* containing translated strings.</span></span> <span data-ttu-id="3b0c0-187">"es" 是西班牙文的語言代碼。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-187">"es" is the language code for Spanish.</span></span> <span data-ttu-id="3b0c0-188">若要在 Visual Studio 中建立這個資源檔：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-188">To create this resource file in Visual Studio:</span></span>

1. <span data-ttu-id="3b0c0-189">在方案總管\*\*\*\* 中，以滑鼠右鍵按一下要放置資源檔的資料夾 > [新增]**[新增項目]** > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-189">In **Solution Explorer**, right click on the folder which will contain the resource file > **Add** > **New Item**.</span></span>

    ![巢狀特色選單：方案總管會開啟 [資源] 的特色選單，](localization/_static/newi.png)

2. <span data-ttu-id="3b0c0-192">在 [Search installed templates] (搜尋已安裝的範本)\*\*\*\* 方塊中，輸入「資源」，並命名檔案。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-192">In the **Search installed templates** box, enter "resource" and name the file.</span></span>

    ![[新增項目] 對話方塊](localization/_static/res.png)

3. <span data-ttu-id="3b0c0-194">在 [名稱]\*\*\*\* 資料行中輸入索引鍵值 (原生字串)，並在 [值]\*\*\*\* 資料行中輸入已翻譯的字串。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-194">Enter the key value (native string) in the **Name** column and the translated string in the **Value** column.</span></span>

    ![Welcome.es.resx 檔案 (西班牙文的「歡迎使用」資源檔)，其中 [名稱] 資料行的文字為 Hello，而 [值] 資料行的文字為 Hola (Hello 的西班牙文)](localization/_static/hola.png)

    <span data-ttu-id="3b0c0-196">Visual Studio 會顯示 *Welcome.es.resx* 檔案。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-196">Visual Studio shows the *Welcome.es.resx* file.</span></span>

    ![方案總管，其中顯示「歡迎使用」的西班牙文 (es) 資源檔](localization/_static/se.png)

## <a name="resource-file-naming"></a><span data-ttu-id="3b0c0-198">資源檔命名</span><span class="sxs-lookup"><span data-stu-id="3b0c0-198">Resource file naming</span></span>

<span data-ttu-id="3b0c0-199">資源的命名方式是以其類別的完整類型名稱去掉組件名稱而得。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-199">Resources are named for the full type name of their class minus the assembly name.</span></span> <span data-ttu-id="3b0c0-200">例如，假設專案中的法文資源是 `LocalizationWebsite.Web.Startup` 類別、主要組件為 `LocalizationWebsite.Web.dll`，就會命名為 *Startup.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-200">For example, a French resource in a project whose main assembly is `LocalizationWebsite.Web.dll` for the class `LocalizationWebsite.Web.Startup` would be named *Startup.fr.resx*.</span></span> <span data-ttu-id="3b0c0-201">若是 `LocalizationWebsite.Web.Controllers.HomeController` 類別的資源，則應命名為 *Controllers.HomeController.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-201">A resource for the class `LocalizationWebsite.Web.Controllers.HomeController` would be named *Controllers.HomeController.fr.resx*.</span></span> <span data-ttu-id="3b0c0-202">如果目標類別的命名空間和組件名稱不相同，則需要使用完整類型名稱。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-202">If your targeted class's namespace isn't the same as the assembly name you will need the full type name.</span></span> <span data-ttu-id="3b0c0-203">比方說，範例專案中 `ExtraNamespace.Tools` 類型的資源會命名為 *ExtraNamespace.Tools.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-203">For example, in the sample project a resource for the type `ExtraNamespace.Tools` would be named *ExtraNamespace.Tools.fr.resx*.</span></span>

<span data-ttu-id="3b0c0-204">在範例專案中，`ConfigureServices` 方法會將 `ResourcesPath` 設為 "Resources"，因此首頁控制器的法文資源檔專案相對路徑即為 *Resources/Controllers.HomeController.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-204">In the sample project, the `ConfigureServices` method sets the `ResourcesPath` to "Resources", so the project relative path for the home controller's French resource file is *Resources/Controllers.HomeController.fr.resx*.</span></span> <span data-ttu-id="3b0c0-205">或者，您可以使用資料夾來收集資源檔。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-205">Alternatively, you can use folders to organize resource files.</span></span> <span data-ttu-id="3b0c0-206">若是首頁控制器，路徑就是 *Resources/Controllers/HomeController.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-206">For the home controller, the path would be *Resources/Controllers/HomeController.fr.resx*.</span></span> <span data-ttu-id="3b0c0-207">如果您不使用 `ResourcesPath` 選項，*.resx* 檔案即會放置在專案的基底目錄中。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-207">If you don't use the `ResourcesPath` option, the *.resx* file would go in the project base directory.</span></span> <span data-ttu-id="3b0c0-208">`HomeController` 的資源檔會命名為 *Controllers.HomeController.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-208">The resource file for `HomeController` would be named *Controllers.HomeController.fr.resx*.</span></span> <span data-ttu-id="3b0c0-209">您可依據自己的資源檔收集方式，來選擇要使用點或路徑的命名慣例。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-209">The choice of using the dot or path naming convention depends on how you want to organize your resource files.</span></span>

| <span data-ttu-id="3b0c0-210">資源名稱</span><span class="sxs-lookup"><span data-stu-id="3b0c0-210">Resource name</span></span> | <span data-ttu-id="3b0c0-211">點或路徑命名</span><span class="sxs-lookup"><span data-stu-id="3b0c0-211">Dot or path naming</span></span> |
| ------------   | ------------- |
| <span data-ttu-id="3b0c0-212">Resources/Controllers.HomeController.fr.resx</span><span class="sxs-lookup"><span data-stu-id="3b0c0-212">Resources/Controllers.HomeController.fr.resx</span></span> | <span data-ttu-id="3b0c0-213">點</span><span class="sxs-lookup"><span data-stu-id="3b0c0-213">Dot</span></span>  |
| <span data-ttu-id="3b0c0-214">Resources/Controllers/HomeController.fr.resx</span><span class="sxs-lookup"><span data-stu-id="3b0c0-214">Resources/Controllers/HomeController.fr.resx</span></span>  | <span data-ttu-id="3b0c0-215">路徑</span><span class="sxs-lookup"><span data-stu-id="3b0c0-215">Path</span></span> |
|    |     |

<span data-ttu-id="3b0c0-216">在 views 中使用的資源檔會 `@inject IViewLocalizer` Razor 遵循類似的模式。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-216">Resource files using `@inject IViewLocalizer` in Razor views follow a similar pattern.</span></span> <span data-ttu-id="3b0c0-217">您可以使用點命名或路徑命名方式，來命名檢視的資源檔。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-217">The resource file for a view can be named using either dot naming or path naming.</span></span> Razor<span data-ttu-id="3b0c0-218">查看資源檔模擬其相關聯之視圖檔案的路徑。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-218"> view resource files mimic the path of their associated view file.</span></span> <span data-ttu-id="3b0c0-219">假設我們將 `ResourcesPath` 設為 "Resources"，與 *Views/Home/About.cshtml* 檢視建立關聯的法文資源檔可為下列其一：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-219">Assuming we set the `ResourcesPath` to "Resources", the French resource file associated with the *Views/Home/About.cshtml* view could be either of the following:</span></span>

* <span data-ttu-id="3b0c0-220">Resources/Views/Home/About.fr.resx</span><span class="sxs-lookup"><span data-stu-id="3b0c0-220">Resources/Views/Home/About.fr.resx</span></span>

* <span data-ttu-id="3b0c0-221">Resources/Views.Home.About.fr.resx</span><span class="sxs-lookup"><span data-stu-id="3b0c0-221">Resources/Views.Home.About.fr.resx</span></span>

<span data-ttu-id="3b0c0-222">如果您不使用 `ResourcesPath` 選項，則檢視的 *.resx* 檔案會與檢視位於相同資料夾中。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-222">If you don't use the `ResourcesPath` option, the *.resx* file for a view would be located in the same folder as the view.</span></span>

### <a name="rootnamespaceattribute"></a><span data-ttu-id="3b0c0-223">RootNamespaceAttribute</span><span class="sxs-lookup"><span data-stu-id="3b0c0-223">RootNamespaceAttribute</span></span> 

<span data-ttu-id="3b0c0-224">[RootNamespace](/dotnet/api/microsoft.extensions.localization.rootnamespaceattribute?view=aspnetcore-2.1) 屬性會在組件的根命名空間與組件名稱不同時，提供組件的根命名空間。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-224">The [RootNamespace](/dotnet/api/microsoft.extensions.localization.rootnamespaceattribute?view=aspnetcore-2.1) attribute provides the root namespace of an assembly when the root namespace of an assembly is different than the assembly name.</span></span> 

> [!WARNING]
> <span data-ttu-id="3b0c0-225">當專案的名稱不是有效的 .NET 識別碼時，就可能發生這種情況。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-225">This can occur when a project's name is not a valid .NET identifier.</span></span> <span data-ttu-id="3b0c0-226">例如， `my-project-name.csproj` 會使用根命名空間 `my_project_name` ，以及 `my-project-name` 導致此錯誤的元件名稱。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-226">For instance `my-project-name.csproj` will use the root namespace `my_project_name` and the assembly name `my-project-name` leading to this error.</span></span> 

<span data-ttu-id="3b0c0-227">如果組件的根命名空間與組件名稱不同：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-227">If the root namespace of an assembly is different than the assembly name:</span></span>

* <span data-ttu-id="3b0c0-228">根據預設，當地語系化無法運作。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-228">Localization does not work by default.</span></span>
* <span data-ttu-id="3b0c0-229">在組件中搜尋資源的方式造成當地語系化失敗。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-229">Localization fails due to the way resources are searched for within the assembly.</span></span> <span data-ttu-id="3b0c0-230">`RootNamespace` 是建置時間值，無法用於執行處理序。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-230">`RootNamespace` is a build-time value which is not available to the executing process.</span></span> 

<span data-ttu-id="3b0c0-231">如果 `RootNamespace` 與 `AssemblyName` 不同，請將下列內容納入 *AssemblyInfo.cs* (參數值取代為實際值)：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-231">If the `RootNamespace` is different from the `AssemblyName`, include the following in *AssemblyInfo.cs* (with parameter values replaced with the actual values):</span></span>

```csharp
using System.Reflection;
using Microsoft.Extensions.Localization;

[assembly: ResourceLocation("Resource Folder Name")]
[assembly: RootNamespace("App Root Namespace")]
```

<span data-ttu-id="3b0c0-232">上述程式碼可成功解析 resx 檔案。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-232">The preceding code enables the successful resolution of resx files.</span></span>

## <a name="culture-fallback-behavior"></a><span data-ttu-id="3b0c0-233">文化特性後援行為</span><span class="sxs-lookup"><span data-stu-id="3b0c0-233">Culture fallback behavior</span></span>

<span data-ttu-id="3b0c0-234">搜尋資源時，當地語系化會使用「文化特性後援」。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-234">When searching for a resource, localization engages in "culture fallback".</span></span> <span data-ttu-id="3b0c0-235">從所要求的文化特性開始，如果找不到，就會還原成該文化特性的父文化特性。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-235">Starting from the requested culture, if not found, it reverts to the parent culture of that culture.</span></span> <span data-ttu-id="3b0c0-236">另外，[CultureInfo.Parent](/dotnet/api/system.globalization.cultureinfo.parent) 屬性代表父文化特性。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-236">As an aside, the [CultureInfo.Parent](/dotnet/api/system.globalization.cultureinfo.parent) property represents the parent culture.</span></span> <span data-ttu-id="3b0c0-237">這通常 (但並非一定) 表示從 ISO 中移除國家意符 (Signifier)。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-237">This usually (but not always) means removing the national signifier from the ISO.</span></span> <span data-ttu-id="3b0c0-238">例如，墨西哥的西班牙文方言是 "es-MX"。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-238">For example, the dialect of Spanish spoken in Mexico is "es-MX".</span></span> <span data-ttu-id="3b0c0-239">它具有父系 "es" &mdash; 西班牙文不是任何國家/地區的特定項目。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-239">It has the parent "es"&mdash;Spanish non-specific to any country.</span></span>

<span data-ttu-id="3b0c0-240">假設您的網站收到使用文化特性 "fr-CA" 之 "Welcome" 資源的要求。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-240">Imagine your site receives a request for a "Welcome" resource using culture "fr-CA".</span></span> <span data-ttu-id="3b0c0-241">當地語系化系統會依照順序尋找下列資源，並選取第一個相符項目：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-241">The localization system looks for the following resources, in order, and selects the first match:</span></span>

* <span data-ttu-id="3b0c0-242">*Welcome.fr-CA.resx*</span><span class="sxs-lookup"><span data-stu-id="3b0c0-242">*Welcome.fr-CA.resx*</span></span>
* <span data-ttu-id="3b0c0-243">*Welcome.fr.resx*</span><span class="sxs-lookup"><span data-stu-id="3b0c0-243">*Welcome.fr.resx*</span></span>
* <span data-ttu-id="3b0c0-244">*Welcome.resx* (如果 `NeutralResourcesLanguage` 是 "fr-CA")</span><span class="sxs-lookup"><span data-stu-id="3b0c0-244">*Welcome.resx* (if the `NeutralResourcesLanguage` is "fr-CA")</span></span>

<span data-ttu-id="3b0c0-245">舉例來說，如果您移除 ".fr" 文化特性指示項，並將文化特性設定為法文，則系統會讀取預設資源檔，並將字串當地語系化。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-245">As an example, if you remove the ".fr" culture designator and you have the culture set to French, the default resource file is read and strings are localized.</span></span> <span data-ttu-id="3b0c0-246">當沒有任何項目符合您要求的文化特性時，資源管理員即會指定預設資源或後援資源。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-246">The Resource manager designates a default or fallback resource for when nothing meets your requested culture.</span></span> <span data-ttu-id="3b0c0-247">如果您只想在要求的文化特性缺少資源時傳回索引鍵，就不能使用預設資源檔。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-247">If you want to just return the key when missing a resource for the requested culture you must not have a default resource file.</span></span>

### <a name="generate-resource-files-with-visual-studio"></a><span data-ttu-id="3b0c0-248">使用 Visual Studio 產生資源檔</span><span class="sxs-lookup"><span data-stu-id="3b0c0-248">Generate resource files with Visual Studio</span></span>

<span data-ttu-id="3b0c0-249">如果您在 Visual Studio 中建立資源檔，但檔案名稱中不含文化特性 (例如 *Welcome.resx*)，則 Visual Studio 會針對每個字串的屬性建立 C# 類別。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-249">If you create a resource file in Visual Studio without a culture in the file name (for example, *Welcome.resx*), Visual Studio will create a C# class with a property for each string.</span></span> <span data-ttu-id="3b0c0-250">但這通常不是您使用 ASP.NET Core 的初衷。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-250">That's usually not what you want with ASP.NET Core.</span></span> <span data-ttu-id="3b0c0-251">一般來說，您不會有預設的 *.resx* 資源檔 (不含文化特性名稱的 *.resx* 檔案)。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-251">You typically don't have a default *.resx* resource file (a *.resx* file without the culture name).</span></span> <span data-ttu-id="3b0c0-252">因此，建議您建立含有文化特性名稱的 *.resx* 檔案 (例如 *Welcome.fr.resx*)。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-252">We suggest you create the *.resx* file with a culture name (for example *Welcome.fr.resx*).</span></span> <span data-ttu-id="3b0c0-253">當您建立含有文化特性名稱的 *.resx* 檔案時，Visual Studio 就不會產生類別檔案。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-253">When you create a *.resx* file with a culture name, Visual Studio won't generate the class file.</span></span>

### <a name="add-other-cultures"></a><span data-ttu-id="3b0c0-254">新增其他文化特性</span><span class="sxs-lookup"><span data-stu-id="3b0c0-254">Add other cultures</span></span>

<span data-ttu-id="3b0c0-255">每種語言和文化特性的組合 (非預設語言) 都需要唯一的資源檔。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-255">Each language and culture combination (other than the default language) requires a unique resource file.</span></span> <span data-ttu-id="3b0c0-256">若要建立不同文化特性和地區設定的資源檔，您可以建立新的資源檔並將 ISO 語言代碼作為檔名的一部分 (例如 **en-us**、**fr-ca** 和 **en-gb**)。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-256">You create resource files for different cultures and locales by creating new resource files in which the ISO language codes are part of the file name (for example, **en-us**, **fr-ca**, and **en-gb**).</span></span> <span data-ttu-id="3b0c0-257">您應將 ISO 代碼置於檔案名稱和 *.resx* 副檔名之間，例如 *Welcome.es-MX.resx* (西班牙文/墨西哥)。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-257">These ISO codes are placed between the file name and the *.resx* file extension, as in *Welcome.es-MX.resx* (Spanish/Mexico).</span></span>

## <a name="implement-a-strategy-to-select-the-languageculture-for-each-request"></a><span data-ttu-id="3b0c0-258">實作可依據每項要求選取語言/文化特性的策略</span><span class="sxs-lookup"><span data-stu-id="3b0c0-258">Implement a strategy to select the language/culture for each request</span></span>

### <a name="configure-localization"></a><span data-ttu-id="3b0c0-259">設定當地語系化</span><span class="sxs-lookup"><span data-stu-id="3b0c0-259">Configure localization</span></span>

<span data-ttu-id="3b0c0-260">您可以在 `Startup.ConfigureServices` 方法中設定當地語系化：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-260">Localization is configured in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](localization/sample/Localization/Startup.cs?name=snippet1)]

* <span data-ttu-id="3b0c0-261">`AddLocalization` 可將當地語系化服務新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-261">`AddLocalization` Adds the localization services to the services container.</span></span> <span data-ttu-id="3b0c0-262">上方的程式碼也會將資源路徑設為 "Resources"。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-262">The code above also sets the resources path to "Resources".</span></span>

* <span data-ttu-id="3b0c0-263">`AddViewLocalization` 可支援當地語系化的檢視檔案。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-263">`AddViewLocalization` Adds support for localized view files.</span></span> <span data-ttu-id="3b0c0-264">在此範例中，檢視的當地語系化會以檢視檔案的後置詞為依據。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-264">In this sample view localization is based on the view file suffix.</span></span> <span data-ttu-id="3b0c0-265">例如 *Index.fr.cshtml* 檔案中的 "fr"。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-265">For example "fr" in the *Index.fr.cshtml* file.</span></span>

* <span data-ttu-id="3b0c0-266">`AddDataAnnotationsLocalization` 可支援透過 `IStringLocalizer` 抽象概念而來的當地語系化 `DataAnnotations` 驗證訊息。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-266">`AddDataAnnotationsLocalization` Adds support for localized `DataAnnotations` validation messages through `IStringLocalizer` abstractions.</span></span>

### <a name="localization-middleware"></a><span data-ttu-id="3b0c0-267">當地語系化中介軟體</span><span class="sxs-lookup"><span data-stu-id="3b0c0-267">Localization middleware</span></span>

<span data-ttu-id="3b0c0-268">您可以在當地語系化[中介軟體](xref:fundamentals/middleware/index)中，設定要求目前的文化特性。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-268">The current culture on a request is set in the localization [Middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="3b0c0-269">已在 `Startup.Configure` 方法中啟用當地語系化中介軟體。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-269">The localization middleware is enabled in the `Startup.Configure` method.</span></span> <span data-ttu-id="3b0c0-270">您必須在可能檢查要求文化特性的任何中介軟體之前，設定當地語系化中介軟體 (例如 `app.UseMvcWithDefaultRoute()`)。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-270">The localization middleware must be configured before any middleware which might check the request culture (for example, `app.UseMvcWithDefaultRoute()`).</span></span>

[!code-csharp[](localization/sample/Localization/Startup.cs?name=snippet2)]
[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

<span data-ttu-id="3b0c0-271">`UseRequestLocalization` 會初始化 `RequestLocalizationOptions` 物件。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-271">`UseRequestLocalization` initializes a `RequestLocalizationOptions` object.</span></span> <span data-ttu-id="3b0c0-272">在每次要求時，系統會列舉 `RequestLocalizationOptions` 中的 `RequestCultureProvider` 清單，並使用能成功判斷要求的文化特性的第一個提供者。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-272">On every request the list of `RequestCultureProvider` in the `RequestLocalizationOptions` is enumerated and the first provider that can successfully determine the request culture is used.</span></span> <span data-ttu-id="3b0c0-273">預設的提供者是來自 `RequestLocalizationOptions` 類別：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-273">The default providers come from the `RequestLocalizationOptions` class:</span></span>

1. `QueryStringRequestCultureProvider`
2. `CookieRequestCultureProvider`
3. `AcceptLanguageHeaderRequestCultureProvider`

<span data-ttu-id="3b0c0-274">預設清單會由針對性高到低來排列。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-274">The default list goes from most specific to least specific.</span></span> <span data-ttu-id="3b0c0-275">在本文稍後，我們會說明如何變更順序，甚至新增自訂的文化特性提供者。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-275">Later in the article we'll see how you can change the order and even add a custom culture provider.</span></span> <span data-ttu-id="3b0c0-276">如果沒有任何提供者可以判斷要求的文化特性，即會使用 `DefaultRequestCulture`。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-276">If none of the providers can determine the request culture, the `DefaultRequestCulture` is used.</span></span>

### <a name="querystringrequestcultureprovider"></a><span data-ttu-id="3b0c0-277">QueryStringRequestCultureProvider</span><span class="sxs-lookup"><span data-stu-id="3b0c0-277">QueryStringRequestCultureProvider</span></span>

<span data-ttu-id="3b0c0-278">有些應用程式會使用查詢字串來設定[文化特性和 UI 文化特性](https://msdn.microsoft.com/library/system.globalization.cultureinfo.aspx)。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-278">Some apps will use a query string to set the [culture and UI culture](https://msdn.microsoft.com/library/system.globalization.cultureinfo.aspx).</span></span> <span data-ttu-id="3b0c0-279">若是使用 Cookie 或 Accept-Language 標頭方法的應用程式，您可以將查詢字串新增至 URL 以偵錯和測試程式碼。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-279">For apps that use the cookie or Accept-Language header approach, adding a query string to the URL is useful for debugging and testing code.</span></span> <span data-ttu-id="3b0c0-280">系統預設會將 `QueryStringRequestCultureProvider` 登錄為 `RequestCultureProvider` 清單中的第一個當地語系化提供者。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-280">By default, the `QueryStringRequestCultureProvider` is registered as the first localization provider in the `RequestCultureProvider` list.</span></span> <span data-ttu-id="3b0c0-281">您應傳遞查詢字串參數 `culture` 和 `ui-culture`。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-281">You pass the query string parameters `culture` and `ui-culture`.</span></span> <span data-ttu-id="3b0c0-282">下列範例會設定西班牙文/墨西哥的特定文化特性 (語言和地區)：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-282">The following example sets the specific culture (language and region) to Spanish/Mexico:</span></span>

   `http://localhost:5000/?culture=es-MX&ui-culture=es-MX`

<span data-ttu-id="3b0c0-283">如果您只傳入兩者其一 (`culture` 或 `ui-culture`)，查詢字串提供者就會使用您傳入的項目來設定這兩個值。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-283">If you only pass in one of the two (`culture` or `ui-culture`), the query string provider will set both values using the one you passed in.</span></span> <span data-ttu-id="3b0c0-284">例如，若只設定文化特性，即會同時設定 `Culture` 和 `UICulture`：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-284">For example, setting just the culture will set both the `Culture` and the `UICulture`:</span></span>

   `http://localhost:5000/?culture=es-MX`

### <a name="cookierequestcultureprovider"></a><span data-ttu-id="3b0c0-285">CookieRequestCultureProvider</span><span class="sxs-lookup"><span data-stu-id="3b0c0-285">CookieRequestCultureProvider</span></span>

<span data-ttu-id="3b0c0-286">生產環境應用程式通常會提供一個機制，來設定 ASP.NET Core 文化特性 Cookie 的文化特性。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-286">Production apps will often provide a mechanism to set the culture with the ASP.NET Core culture cookie.</span></span> <span data-ttu-id="3b0c0-287">若要建立 Cookie，請使用 `MakeCookieValue` 方法。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-287">Use the `MakeCookieValue` method to create a cookie.</span></span>

<span data-ttu-id="3b0c0-288">會傳回 `CookieRequestCultureProvider` `DefaultCookieName` 用來追蹤使用者慣用文化特性資訊的預設 cookie 名稱。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-288">The `CookieRequestCultureProvider` `DefaultCookieName` returns the default cookie name used to track the user's preferred culture information.</span></span> <span data-ttu-id="3b0c0-289">預設 Cookie 名稱為 `.AspNetCore.Culture`。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-289">The default cookie name is `.AspNetCore.Culture`.</span></span>

<span data-ttu-id="3b0c0-290">Cookie 格式為 `c=%LANGCODE%|uic=%LANGCODE%`，其中 `c` 是 `Culture` 而 `uic` 是 `UICulture`，例如：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-290">The cookie format is `c=%LANGCODE%|uic=%LANGCODE%`, where `c` is `Culture` and `uic` is `UICulture`, for example:</span></span>

    c=en-UK|uic=en-US

<span data-ttu-id="3b0c0-291">如果您只指定文化特性資訊和 UI 文化特性其一，系統就會將您所指定的文化特性用於文化特性資訊和 UI 文化特性。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-291">If you only specify one of culture info and UI culture, the specified culture will be used for both culture info and UI culture.</span></span>

### <a name="the-accept-language-http-header"></a><span data-ttu-id="3b0c0-292">Accept-Language HTTP 標頭</span><span class="sxs-lookup"><span data-stu-id="3b0c0-292">The Accept-Language HTTP header</span></span>

<span data-ttu-id="3b0c0-293">您可在大多數的瀏覽器中設定 [Accept-Language 標頭](https://www.w3.org/International/questions/qa-accept-lang-locales)，其最初的設計目的是用來指定使用者的語言。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-293">The [Accept-Language header](https://www.w3.org/International/questions/qa-accept-lang-locales) is settable in most browsers and was originally intended to specify the user's language.</span></span> <span data-ttu-id="3b0c0-294">這項設定可指出瀏覽器已設好要傳送哪些項目，或已從基礎作業系統繼承哪些項目。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-294">This setting indicates what the browser has been set to send or has inherited from the underlying operating system.</span></span> <span data-ttu-id="3b0c0-295">透過瀏覽器要求的 Accept-Language HTTP 標頭來偵測使用者的慣用語言，並非萬無一失 (請參閱 [Setting language preferences in a browser](https://www.w3.org/International/questions/qa-lang-priorities.en.php) (在瀏覽器中設定語言喜好設定)。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-295">The Accept-Language HTTP header from a browser request isn't an infallible way to detect the user's preferred language (see [Setting language preferences in a browser](https://www.w3.org/International/questions/qa-lang-priorities.en.php)).</span></span> <span data-ttu-id="3b0c0-296">生產環境應用程式應該包含可讓使用者自訂文化特性的選擇方式。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-296">A production app should include a way for a user to customize their choice of culture.</span></span>

### <a name="set-the-accept-language-http-header-in-ie"></a><span data-ttu-id="3b0c0-297">在 IE 中設定 Accept-Language HTTP 標頭</span><span class="sxs-lookup"><span data-stu-id="3b0c0-297">Set the Accept-Language HTTP header in IE</span></span>

1. <span data-ttu-id="3b0c0-298">從齒輪圖示，點選 [網際網路選項]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-298">From the gear icon, tap **Internet Options**.</span></span>

2. <span data-ttu-id="3b0c0-299">點選 [語言]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-299">Tap **Languages**.</span></span>

    ![網際網路選項](localization/_static/lang.png)

3. <span data-ttu-id="3b0c0-301">點選 [設定語言喜好設定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-301">Tap **Set Language Preferences**.</span></span>

4. <span data-ttu-id="3b0c0-302">點選 [新增語言]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-302">Tap **Add a language**.</span></span>

5. <span data-ttu-id="3b0c0-303">新增語言。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-303">Add the language.</span></span>

6. <span data-ttu-id="3b0c0-304">點選語言，然後點選 [上移]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-304">Tap the language, then tap **Move Up**.</span></span>

### <a name="use-a-custom-provider"></a><span data-ttu-id="3b0c0-305">使用自訂提供者</span><span class="sxs-lookup"><span data-stu-id="3b0c0-305">Use a custom provider</span></span>

<span data-ttu-id="3b0c0-306">假設您想要讓客戶將他們的語言和文化特性儲存在您的資料庫中。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-306">Suppose you want to let your customers store their language and culture in your databases.</span></span> <span data-ttu-id="3b0c0-307">您可以撰寫提供者，以供使用者查詢這些值。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-307">You could write a provider to look up these values for the user.</span></span> <span data-ttu-id="3b0c0-308">下列程式碼會示範如何新增自訂提供者：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-308">The following code shows how to add a custom provider:</span></span>

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

<span data-ttu-id="3b0c0-309">使用 `RequestLocalizationOptions` 新增或移除當地語系化提供者。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-309">Use `RequestLocalizationOptions` to add or remove localization providers.</span></span>

### <a name="set-the-culture-programmatically"></a><span data-ttu-id="3b0c0-310">以程式設計方式來設定文化特性</span><span class="sxs-lookup"><span data-stu-id="3b0c0-310">Set the culture programmatically</span></span>

<span data-ttu-id="3b0c0-311">[GitHub](https://github.com/aspnet/entropy) 上的這個範例 **Localization.StarterWeb** 專案包含可設定 `Culture` 的 UI。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-311">This sample **Localization.StarterWeb** project on [GitHub](https://github.com/aspnet/entropy) contains UI to set the `Culture`.</span></span> <span data-ttu-id="3b0c0-312">*Views/Shared/_SelectLanguagePartial.cshtml* 檔可讓您從支援的文化特性清單中選取文化特性：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-312">The *Views/Shared/_SelectLanguagePartial.cshtml* file allows you to select the culture from the list of supported cultures:</span></span>

[!code-cshtml[](localization/sample/Localization/Views/Shared/_SelectLanguagePartial.cshtml)]

<span data-ttu-id="3b0c0-313">系統會將 *Views/Shared/_SelectLanguagePartial.cshtml* 檔案新增至配置檔案的 `footer` 區段，以供所有檢視使用：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-313">The *Views/Shared/_SelectLanguagePartial.cshtml* file is added to the `footer` section of the layout file so it will be available to all views:</span></span>

[!code-cshtml[](localization/sample/Localization/Views/Shared/_Layout.cshtml?range=43-56&highlight=10)]

<span data-ttu-id="3b0c0-314">`SetLanguage` 方法會設定文化特性的 Cookie。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-314">The `SetLanguage` method sets the culture cookie.</span></span>

[!code-csharp[](localization/sample/Localization/Controllers/HomeController.cs?range=57-67)]

<span data-ttu-id="3b0c0-315">您無法將 *_SelectLanguagePartial.cshtml* 插入這個專案的範例程式碼。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-315">You can't plug in the *_SelectLanguagePartial.cshtml* to sample code for this project.</span></span> <span data-ttu-id="3b0c0-316">[GitHub](https://github.com/aspnet/entropy)上的**localization.starterweb**專案具有程式碼，可透過相依性 `RequestLocalizationOptions` 插入容器將傳遞至 Razor 部分。 [Dependency Injection](dependency-injection.md)</span><span class="sxs-lookup"><span data-stu-id="3b0c0-316">The **Localization.StarterWeb** project on [GitHub](https://github.com/aspnet/entropy) has code to flow the `RequestLocalizationOptions` to a Razor partial through the [Dependency Injection](dependency-injection.md) container.</span></span>

## <a name="model-binding-route-data-and-query-strings"></a><span data-ttu-id="3b0c0-317">模型系結路由資料和查詢字串</span><span class="sxs-lookup"><span data-stu-id="3b0c0-317">Model binding route data and query strings</span></span>

<span data-ttu-id="3b0c0-318">請參閱模型系結[路由資料和查詢字串的全球化行為](xref:mvc/models/model-binding#glob)。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-318">See [Globalization behavior of model binding route data and query strings](xref:mvc/models/model-binding#glob).</span></span>

## <a name="globalization-and-localization-terms"></a><span data-ttu-id="3b0c0-319">全球化和當地語系化詞彙</span><span class="sxs-lookup"><span data-stu-id="3b0c0-319">Globalization and localization terms</span></span>

<span data-ttu-id="3b0c0-320">在進行應用程式的當地語系化程序時，您也需要具備現代軟體開發中常用的相關字元集基本知識，並了解與它們建立關聯的問題。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-320">The process of localizing your app also requires a basic understanding of relevant character sets commonly used in modern software development and an understanding of the issues associated with them.</span></span> <span data-ttu-id="3b0c0-321">雖然所有電腦都會將文字儲存為數字 (程式碼)，但不同系統會使用不同數字來儲存相同的文字。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-321">Although all computers store text as numbers (codes), different systems store the same text using different numbers.</span></span> <span data-ttu-id="3b0c0-322">當地語系化是指針對特定文化特性/地區設定，轉譯應用程式使用者介面 (UI) 的程序。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-322">The localization process refers to translating the app user interface (UI) for a specific culture/locale.</span></span>

<span data-ttu-id="3b0c0-323">[可當地語系化](/dotnet/standard/globalization-localization/localizability-review)是確認全球化應用程式已準備好進行當地語系化的中繼程序。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-323">[Localizability](/dotnet/standard/globalization-localization/localizability-review) is an intermediate process for verifying that a globalized app is ready for localization.</span></span>

<span data-ttu-id="3b0c0-324">文化特性名稱的 [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) 格式是 `<languagecode2>-<country/regioncode2>`，其中 `<languagecode2>` 是語言代碼，而 `<country/regioncode2>` 是子文化特性代碼。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-324">The [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) format for the culture name is `<languagecode2>-<country/regioncode2>`, where `<languagecode2>` is the language code and `<country/regioncode2>` is the subculture code.</span></span> <span data-ttu-id="3b0c0-325">例如，`es-CL` 是指西班牙文 (智利)，`en-US` 是指英文 (美國)，而 `en-AU` 是指英文 (澳大利亞)。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-325">For example, `es-CL` for Spanish (Chile), `en-US` for English (United States), and `en-AU` for English (Australia).</span></span> <span data-ttu-id="3b0c0-326">[RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) 是 ISO 639 (兩個小寫字母，為與某個語言建立關聯的文化特性代碼) 及 ISO 3166 (兩個大寫字母，為與某個國家或地區建立關聯的子文化特性代碼) 的組合。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-326">[RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) is a combination of an ISO 639 two-letter lowercase culture code associated with a language and an ISO 3166 two-letter uppercase subculture code associated with a country or region.</span></span> <span data-ttu-id="3b0c0-327">請參閱 [Language Culture Name](https://msdn.microsoft.com/library/ee825488(v=cs.20).aspx) (語言的文化特性名稱)。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-327">See [Language Culture Name](https://msdn.microsoft.com/library/ee825488(v=cs.20).aspx).</span></span>

<span data-ttu-id="3b0c0-328">國際化通常縮寫為 "I18N"。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-328">Internationalization is often abbreviated to "I18N".</span></span> <span data-ttu-id="3b0c0-329">這個縮寫是採用該詞彙的第一個和最後一個字母，以及這兩個字母間的字母數組成，因此 18 代表第一個字母 "I" 及最後 "N" 中間的字母數。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-329">The abbreviation takes the first and last letters and the number of letters between them, so 18 stands for the number of letters between the first "I" and the last "N".</span></span> <span data-ttu-id="3b0c0-330">同樣的原則也適用於全球化 (G11N) 與當地語系化 (L10N) 的縮寫。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-330">The same applies to Globalization (G11N), and Localization (L10N).</span></span>

<span data-ttu-id="3b0c0-331">詞彙：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-331">Terms:</span></span>

* <span data-ttu-id="3b0c0-332">全球化 (G11N)：讓應用程式支援不同語言和區域的程序。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-332">Globalization (G11N): The process of making an app support different languages and regions.</span></span>
* <span data-ttu-id="3b0c0-333">當地語系化 (L10N)：針對特定語言和區域，自訂應用程式的程序。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-333">Localization (L10N): The process of customizing an app for a given language and region.</span></span>
* <span data-ttu-id="3b0c0-334">國際化 (I18N)：包含全球化和當地語系化這兩部分的描述。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-334">Internationalization (I18N): Describes both globalization and localization.</span></span>
* <span data-ttu-id="3b0c0-335">文化特性：它是一種語言和/或地區。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-335">Culture: It's a language and, optionally, a region.</span></span>
* <span data-ttu-id="3b0c0-336">中性文化特性：具有指定的語言但不限地區的文化特性 </span><span class="sxs-lookup"><span data-stu-id="3b0c0-336">Neutral culture: A culture that has a specified language, but not a region.</span></span> <span data-ttu-id="3b0c0-337">(例如 "en"、"es")。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-337">(for example "en", "es")</span></span>
* <span data-ttu-id="3b0c0-338">特定文化特性：具有指定的語言和地區的文化特性 </span><span class="sxs-lookup"><span data-stu-id="3b0c0-338">Specific culture: A culture that has a specified language and region.</span></span> <span data-ttu-id="3b0c0-339">(例如 "en-US"、"en-GB"、"es-CL")。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-339">(for example "en-US", "en-GB", "es-CL")</span></span>
* <span data-ttu-id="3b0c0-340">父文化特性：包含特定文化特性的中性文化特性 </span><span class="sxs-lookup"><span data-stu-id="3b0c0-340">Parent culture: The neutral culture that contains a specific culture.</span></span> <span data-ttu-id="3b0c0-341">(例如，"en" 是 "en-US" 和 "en-GB" 的父文化特性)。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-341">(for example, "en" is the parent culture of "en-US" and "en-GB")</span></span>
* <span data-ttu-id="3b0c0-342">地區設定：地區設定與文化特性相同。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-342">Locale: A locale is the same as a culture.</span></span>

[!INCLUDE[](~/includes/localization/currency.md)]

[!INCLUDE[](~/includes/localization/unsupported-culture-log-level.md)]

## <a name="additional-resources"></a><span data-ttu-id="3b0c0-343">其他資源</span><span class="sxs-lookup"><span data-stu-id="3b0c0-343">Additional resources</span></span>

* <xref:fundamentals/troubleshoot-aspnet-core-localization>
* <span data-ttu-id="3b0c0-344">本文使用的 [Localization.StarterWeb 專案](https://github.com/aspnet/Entropy/tree/master/samples/Localization.StarterWeb)。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-344">[Localization.StarterWeb project](https://github.com/aspnet/Entropy/tree/master/samples/Localization.StarterWeb) used in the article.</span></span>
* [<span data-ttu-id="3b0c0-345">全球化與當地語系化 .NET 應用程式</span><span class="sxs-lookup"><span data-stu-id="3b0c0-345">Globalizing and localizing .NET applications</span></span>](/dotnet/standard/globalization-localization/index)
* [<span data-ttu-id="3b0c0-346">.Resx 檔案中的資源</span><span class="sxs-lookup"><span data-stu-id="3b0c0-346">Resources in .resx Files</span></span>](/dotnet/framework/resources/working-with-resx-files-programmatically)
* [<span data-ttu-id="3b0c0-347">Microsoft 多語應用程式工具組</span><span class="sxs-lookup"><span data-stu-id="3b0c0-347">Microsoft Multilingual App Toolkit</span></span>](https://marketplace.visualstudio.com/items?itemName=MultilingualAppToolkit.MultilingualAppToolkit-18308)
* [<span data-ttu-id="3b0c0-348">當地語系化和泛型</span><span class="sxs-lookup"><span data-stu-id="3b0c0-348">Localization & Generics</span></span>](http://hishambinateya.com/localization-and-generics)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="3b0c0-349">由 [Rick Anderson](https://twitter.com/RickAndMSFT)、[Damien Bowden](https://twitter.com/damien_bod)、[Bart Calixto](https://twitter.com/bartmax)、[Nadeem Afana](https://afana.me/) 和 [Hisham Bin Ateya](https://twitter.com/hishambinateya) 提供</span><span class="sxs-lookup"><span data-stu-id="3b0c0-349">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Damien Bowden](https://twitter.com/damien_bod), [Bart Calixto](https://twitter.com/bartmax), [Nadeem Afana](https://afana.me/), and [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span></span>

<span data-ttu-id="3b0c0-350">多語系網站可讓網站觸及更多物件。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-350">A multilingual website allows the site to reach a wider audience.</span></span> <span data-ttu-id="3b0c0-351">ASP.NET Core 提供服務與中介軟體，可將網站當地語系化成不同的語言與文化特性。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-351">ASP.NET Core provides services and middleware for localizing into different languages and cultures.</span></span>

<span data-ttu-id="3b0c0-352">國際化包含[全球化](/dotnet/api/system.globalization)和[當地語系化](/dotnet/standard/globalization-localization/localization)這兩部分。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-352">Internationalization involves [Globalization](/dotnet/api/system.globalization) and [Localization](/dotnet/standard/globalization-localization/localization).</span></span> <span data-ttu-id="3b0c0-353">全球化是指設計出可支援不同文化特性之應用程式的程序。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-353">Globalization is the process of designing apps that support different cultures.</span></span> <span data-ttu-id="3b0c0-354">透過全球化，您可新增支援與特定地區相關之已定義語言指令碼的輸入、顯示及輸出作業。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-354">Globalization adds support for input, display, and output of a defined set of language scripts that relate to specific geographic areas.</span></span>

<span data-ttu-id="3b0c0-355">當地語系化是指針對全球化應用程式進行調整的程序，且您已順應特定文化特性/地區設定對這些全球化應用程式進行可當地語系化處理。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-355">Localization is the process of adapting a globalized app, which you have already processed for localizability, to a particular culture/locale.</span></span> <span data-ttu-id="3b0c0-356">如需詳細資訊，請參閱本文件結尾處的**全球化和當地語系化詞彙**。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-356">For more information see **Globalization and localization terms** near the end of this document.</span></span>

<span data-ttu-id="3b0c0-357">應用程式當地語系化包含下列作業：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-357">App localization involves the following:</span></span>

1. <span data-ttu-id="3b0c0-358">讓應用程式的內容可當地語系化</span><span class="sxs-lookup"><span data-stu-id="3b0c0-358">Make the app's content localizable</span></span>
1. <span data-ttu-id="3b0c0-359">針對您支援的語言和文化特性提供當地語系化資源</span><span class="sxs-lookup"><span data-stu-id="3b0c0-359">Provide localized resources for the languages and cultures you support</span></span>
1. <span data-ttu-id="3b0c0-360">實作可依據每項要求選取語言/文化特性的策略</span><span class="sxs-lookup"><span data-stu-id="3b0c0-360">Implement a strategy to select the language/culture for each request</span></span>

<span data-ttu-id="3b0c0-361">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/localization/sample/Localization)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="3b0c0-361">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/localization/sample/Localization) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="make-the-apps-content-localizable"></a><span data-ttu-id="3b0c0-362">讓應用程式的內容可當地語系化</span><span class="sxs-lookup"><span data-stu-id="3b0c0-362">Make the app's content localizable</span></span>

<span data-ttu-id="3b0c0-363"><xref:Microsoft.Extensions.Localization.IStringLocalizer>和 <xref:Microsoft.Extensions.Localization.IStringLocalizer%601> 已架構，可在開發當地語系化應用程式時改善生產力。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-363"><xref:Microsoft.Extensions.Localization.IStringLocalizer> and <xref:Microsoft.Extensions.Localization.IStringLocalizer%601> were architected to improve productivity when developing localized apps.</span></span> <span data-ttu-id="3b0c0-364">`IStringLocalizer`會使用 <xref:System.Resources.ResourceManager> 和， <xref:System.Resources.ResourceReader> 在執行時間提供特定文化特性的資源。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-364">`IStringLocalizer` uses the <xref:System.Resources.ResourceManager> and <xref:System.Resources.ResourceReader> to provide culture-specific resources at run time.</span></span> <span data-ttu-id="3b0c0-365">介面具有索引子和， `IEnumerable` 用於傳回當地語系化的字串。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-365">The interface has an indexer and an `IEnumerable` for returning localized strings.</span></span> <span data-ttu-id="3b0c0-366">`IStringLocalizer`不需要將預設語言字串儲存在資源檔中。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-366">`IStringLocalizer` doesn't require storing the default language strings in a resource file.</span></span> <span data-ttu-id="3b0c0-367">您不必在開發初期建立資源檔，即可開發以當地語系化為目標的應用程式。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-367">You can develop an app targeted for localization and not need to create resource files early in development.</span></span> <span data-ttu-id="3b0c0-368">下列程式碼會示範如何包裝 "About Title" 字串以進行當地語系化。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-368">The code below shows how to wrap the string "About Title" for localization.</span></span>

[!code-csharp[](localization/sample/Localization/Controllers/AboutController.cs)]

<span data-ttu-id="3b0c0-369">在上述程式碼中， `IStringLocalizer<T>` 執行是來自相依性[插入](dependency-injection.md)。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-369">In the preceding code, the `IStringLocalizer<T>` implementation comes from [Dependency Injection](dependency-injection.md).</span></span> <span data-ttu-id="3b0c0-370">如果找不到 "About Title" 的當地語系化值，即會傳回索引子的索引鍵，也就是 "About Title" 字串。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-370">If the localized value of "About Title" isn't found, then the indexer key is returned, that is, the string "About Title".</span></span> <span data-ttu-id="3b0c0-371">您可以保留應用程式中的預設語言常值字串，並將其包裝在當地語系化工具中，以便專注於開發應用程式。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-371">You can leave the default language literal strings in the app and wrap them in the localizer, so that you can focus on developing the app.</span></span> <span data-ttu-id="3b0c0-372">您不用先建立預設資源檔，即可使用預設語言來開發應用程式，並針對當地語系化步驟進行應用程式的準備。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-372">You develop your app with your default language and prepare it for the localization step without first creating a default resource file.</span></span> <span data-ttu-id="3b0c0-373">或者，您可以使用傳統方法，並提供索引鍵以擷取預設語言字串。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-373">Alternatively, you can use the traditional approach and provide a key to retrieve the default language string.</span></span> <span data-ttu-id="3b0c0-374">對許多開發人員來說，新的工作流程 (單純包裝字串常值而不使用預設語言的 *.resx* 檔案) 可以降低當地語系化應用程式的額外負荷。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-374">For many developers the new workflow of not having a default language *.resx* file and simply wrapping the string literals can reduce the overhead of localizing an app.</span></span> <span data-ttu-id="3b0c0-375">其他開發人員則偏好傳統的工作流程，因為這種方法更便於使用較長的字串常值，且更易於更新當地語系化的字串。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-375">Other developers will prefer the traditional work flow as it can make it easier to work with longer string literals and make it easier to update localized strings.</span></span>

<span data-ttu-id="3b0c0-376">若是包含 HTML 的資源，請使用 `IHtmlLocalizer<T>` 實作。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-376">Use the `IHtmlLocalizer<T>` implementation for resources that contain HTML.</span></span> <span data-ttu-id="3b0c0-377">`IHtmlLocalizer` 會對資源字串中經過格式化的引數進行 HTML 編碼，但不會對資源字串本身進行 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-377">`IHtmlLocalizer` HTML encodes arguments that are formatted in the resource string, but doesn't HTML encode the resource string itself.</span></span> <span data-ttu-id="3b0c0-378">在下列醒目提示的範例中，只有 `name` 參數的值經過 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-378">In the sample highlighted below, only the value of `name` parameter is HTML encoded.</span></span>

[!code-csharp[](../fundamentals/localization/sample/Localization/Controllers/BookController.cs?highlight=3,5,20&start=1&end=24)]

<span data-ttu-id="3b0c0-379">**注意：** 一般來說，您只想將文字當地語系化，而不是 HTML。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-379">**Note:** You generally want to only localize text and not HTML.</span></span>

<span data-ttu-id="3b0c0-380">您可以在最底層的[相依性插入](dependency-injection.md)中，將 `IStringLocalizerFactory` 移出：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-380">At the lowest level, you can get `IStringLocalizerFactory` out of [Dependency Injection](dependency-injection.md):</span></span>

[!code-csharp[](localization/sample/Localization/Controllers/TestController.cs?start=9&end=26&highlight=7-13)]

<span data-ttu-id="3b0c0-381">上述程式碼會示範這兩個 Factory Create 方法。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-381">The code above demonstrates each of the two factory create methods.</span></span>

<span data-ttu-id="3b0c0-382">您可以依據控制器或區域來分割當地語系化的字串，也可以只用一個容器。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-382">You can partition your localized strings by controller, area, or have just one container.</span></span> <span data-ttu-id="3b0c0-383">在範例應用程式中，會針對共用資源使用名為 `SharedResource` 的虛擬類別。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-383">In the sample app, a dummy class named `SharedResource` is used for shared resources.</span></span>

[!code-csharp[](localization/sample/Localization/Resources/SharedResource.cs)]

<span data-ttu-id="3b0c0-384">有些開發人員會使用 `Startup` 類別來包含全域或共用字串。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-384">Some developers use the `Startup` class to contain global or shared strings.</span></span> <span data-ttu-id="3b0c0-385">下列範例會使用 `InfoController` 和 `SharedResource` 當地語系化工具：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-385">In the sample below, the `InfoController` and the `SharedResource` localizers are used:</span></span>

[!code-csharp[](localization/sample/Localization/Controllers/InfoController.cs?range=9-26)]

## <a name="view-localization"></a><span data-ttu-id="3b0c0-386">檢視當地語系化</span><span class="sxs-lookup"><span data-stu-id="3b0c0-386">View localization</span></span>

<span data-ttu-id="3b0c0-387">`IViewLocalizer` 服務可提供[檢視](xref:mvc/views/overview)的當地語系化字串。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-387">The `IViewLocalizer` service provides localized strings for a [view](xref:mvc/views/overview).</span></span> <span data-ttu-id="3b0c0-388">`ViewLocalizer` 類別會實作這個介面，並透過檢視檔案路徑來找出資源的位置。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-388">The `ViewLocalizer` class implements this interface and finds the resource location from the view file path.</span></span> <span data-ttu-id="3b0c0-389">下列程式碼示範如何使用 `IViewLocalizer` 的預設實作：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-389">The following code shows how to use the default implementation of `IViewLocalizer`:</span></span>

[!code-cshtml[](localization/sample/Localization/Views/Home/About.cshtml)]

<span data-ttu-id="3b0c0-390">`IViewLocalizer` 的預設實作會依據檢視的檔案名稱來找出資源檔。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-390">The default implementation of `IViewLocalizer` finds the resource file based on the view's file name.</span></span> <span data-ttu-id="3b0c0-391">其中並沒有任何選項可以使用全域共用的資源檔。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-391">There's no option to use a global shared resource file.</span></span> <span data-ttu-id="3b0c0-392">`ViewLocalizer`使用來執行當地語系化工具 `IHtmlLocalizer` ，因此 Razor 不會對當地語系化字串進行 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-392">`ViewLocalizer` implements the localizer using `IHtmlLocalizer`, so Razor doesn't HTML encode the localized string.</span></span> <span data-ttu-id="3b0c0-393">您可以參數化資源字串，`IViewLocalizer` 即會對參數 (而不是資源字串) 進行 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-393">You can parameterize resource strings and `IViewLocalizer` will HTML encode the parameters, but not the resource string.</span></span> <span data-ttu-id="3b0c0-394">請考慮下列 Razor 標記：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-394">Consider the following Razor markup:</span></span>

```cshtml
@Localizer["<i>Hello</i> <b>{0}!</b>", UserManager.GetUserName(User)]
```

<span data-ttu-id="3b0c0-395">法文資源檔可能包含下列內容：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-395">A French resource file could contain the following:</span></span>

| <span data-ttu-id="3b0c0-396">Key</span><span class="sxs-lookup"><span data-stu-id="3b0c0-396">Key</span></span> | <span data-ttu-id="3b0c0-397">值</span><span class="sxs-lookup"><span data-stu-id="3b0c0-397">Value</span></span> |
| ----- | ------ |
| `<i>Hello</i> <b>{0}!</b>` | `<i>Bonjour</i> <b>{0} !</b>` |

<span data-ttu-id="3b0c0-398">轉譯的檢視內容可能包含來自資源檔的 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-398">The rendered view would contain the HTML markup from the resource file.</span></span>

<span data-ttu-id="3b0c0-399">**注意：** 一般來說，您只想將文字當地語系化，而不是 HTML。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-399">**Note:** You generally want to only localize text and not HTML.</span></span>

<span data-ttu-id="3b0c0-400">若要在檢視中使用共用的資源檔，請插入 `IHtmlLocalizer<T>`：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-400">To use a shared resource file in a view, inject `IHtmlLocalizer<T>`:</span></span>

[!code-cshtml[](../fundamentals/localization/sample/Localization/Views/Test/About.cshtml?highlight=5,12)]

## <a name="dataannotations-localization"></a><span data-ttu-id="3b0c0-401">DataAnnotations 當地語系化</span><span class="sxs-lookup"><span data-stu-id="3b0c0-401">DataAnnotations localization</span></span>

<span data-ttu-id="3b0c0-402">DataAnnotations 錯誤訊息會使用 `IStringLocalizer<T>` 來當地語系化。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-402">DataAnnotations error messages are localized with `IStringLocalizer<T>`.</span></span> <span data-ttu-id="3b0c0-403">使用 `ResourcesPath = "Resources"` 選項時，`RegisterViewModel` 中的錯誤訊息會儲存在下列路徑之一：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-403">Using the option `ResourcesPath = "Resources"`, the error messages in `RegisterViewModel` can be stored in either of the following paths:</span></span>

* <span data-ttu-id="3b0c0-404">*Resources/Viewmodel. RegisterViewModel. fr .resx*</span><span class="sxs-lookup"><span data-stu-id="3b0c0-404">*Resources/ViewModels.Account.RegisterViewModel.fr.resx*</span></span>
* <span data-ttu-id="3b0c0-405">*Resources/ViewModels/Account/RegisterViewModel.fr.resx*</span><span class="sxs-lookup"><span data-stu-id="3b0c0-405">*Resources/ViewModels/Account/RegisterViewModel.fr.resx*</span></span>

[!code-csharp[](localization/sample/Localization/ViewModels/Account/RegisterViewModel.cs?start=9&end=26)]

<span data-ttu-id="3b0c0-406">在 ASP.NET Core MVC 1.1.0 和更高版本中，系統會將非驗證屬性當地語系化。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-406">In ASP.NET Core MVC 1.1.0 and higher, non-validation attributes are localized.</span></span> <span data-ttu-id="3b0c0-407">ASP.NET Core MVC 1.0 **不會**查閱非驗證屬性的當地語系化字串。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-407">ASP.NET Core MVC 1.0 does **not** look up localized strings for non-validation attributes.</span></span>

<a name="one-resource-string-multiple-classes"></a>

### <a name="using-one-resource-string-for-multiple-classes"></a><span data-ttu-id="3b0c0-408">針對多個類別使用同一個資源字串</span><span class="sxs-lookup"><span data-stu-id="3b0c0-408">Using one resource string for multiple classes</span></span>

<span data-ttu-id="3b0c0-409">下列程式碼會示範如何針對含有多個類別的驗證屬性使用同一個資源字串：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-409">The following code shows how to use one resource string for validation attributes with multiple classes:</span></span>

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

<span data-ttu-id="3b0c0-410">在先前的程式碼中，`SharedResource` 是與儲存驗證訊息的 resx 對應的類別。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-410">In the preceding code, `SharedResource` is the class corresponding to the resx where your validation messages are stored.</span></span> <span data-ttu-id="3b0c0-411">使用這個方法時，DataAnnotations 只會使用 `SharedResource`，而不是每個類別的資源。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-411">With this approach, DataAnnotations will only use `SharedResource`, rather than the resource for each class.</span></span>

## <a name="provide-localized-resources-for-the-languages-and-cultures-you-support"></a><span data-ttu-id="3b0c0-412">針對您支援的語言和文化特性提供當地語系化資源</span><span class="sxs-lookup"><span data-stu-id="3b0c0-412">Provide localized resources for the languages and cultures you support</span></span>

### <a name="supportedcultures-and-supporteduicultures"></a><span data-ttu-id="3b0c0-413">SupportedCultures 和 SupportedUICultures</span><span class="sxs-lookup"><span data-stu-id="3b0c0-413">SupportedCultures and SupportedUICultures</span></span>

<span data-ttu-id="3b0c0-414">ASP.NET Core 可讓您指定 `SupportedCultures` 和 `SupportedUICultures` 這兩個文化特性值。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-414">ASP.NET Core allows you to specify two culture values, `SupportedCultures` and `SupportedUICultures`.</span></span> <span data-ttu-id="3b0c0-415">`SupportedCultures` 的 [CultureInfo](/dotnet/api/system.globalization.cultureinfo) 物件可決定文化特性相依函式的結果，例如日期、時間、數字及貨幣格式。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-415">The [CultureInfo](/dotnet/api/system.globalization.cultureinfo) object for `SupportedCultures` determines the results of culture-dependent functions, such as date, time, number, and currency formatting.</span></span> <span data-ttu-id="3b0c0-416">`SupportedCultures` 也可決定文字排列順序、大小寫慣例和字串比較。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-416">`SupportedCultures` also determines the sorting order of text, casing conventions, and string comparisons.</span></span> <span data-ttu-id="3b0c0-417">如需伺服器如何取得文化特性的詳細資訊，請參閱 [CultureInfo.CurrentCulture](/dotnet/api/system.stringcomparer.currentculture#System_StringComparer_CurrentCulture)。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-417">See [CultureInfo.CurrentCulture](/dotnet/api/system.stringcomparer.currentculture#System_StringComparer_CurrentCulture) for more info on how the server gets the Culture.</span></span> <span data-ttu-id="3b0c0-418">`SupportedUICultures`會決定[ResourceManager](/dotnet/api/system.resources.resourcemanager)會查閱哪些翻譯的字串（來自 *.resx*檔案）。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-418">The `SupportedUICultures` determines which translated strings (from *.resx* files) are looked up by the [ResourceManager](/dotnet/api/system.resources.resourcemanager).</span></span> <span data-ttu-id="3b0c0-419">`ResourceManager` 僅會查閱 `CurrentUICulture` 所決定之文化特性特有的字串。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-419">The `ResourceManager` simply looks up culture-specific strings that's determined by `CurrentUICulture`.</span></span> <span data-ttu-id="3b0c0-420">.NET 中的每個執行緒都有 `CurrentCulture` 和 `CurrentUICulture` 物件。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-420">Every thread in .NET has `CurrentCulture` and `CurrentUICulture` objects.</span></span> <span data-ttu-id="3b0c0-421">ASP.NET Core 會在轉譯文化特性相依函式時檢查這些值。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-421">ASP.NET Core inspects these values when rendering culture-dependent functions.</span></span> <span data-ttu-id="3b0c0-422">比方說，如果目前執行緒的文化特性設定為 "en-US" (英文 - 美國)，`DateTime.Now.ToLongDateString()` 會顯示 "Thursday, February 18, 2016"，但如果 `CurrentCulture` 設定為 "es-ES" (西班牙文 - 西班牙)，則輸出會是 "jueves, 18 de febrero de 2016"。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-422">For example, if the current thread's culture is set to "en-US" (English, United States), `DateTime.Now.ToLongDateString()` displays "Thursday, February 18, 2016", but if `CurrentCulture` is set to "es-ES" (Spanish, Spain) the output will be "jueves, 18 de febrero de 2016".</span></span>

## <a name="resource-files"></a><span data-ttu-id="3b0c0-423">資源檔</span><span class="sxs-lookup"><span data-stu-id="3b0c0-423">Resource files</span></span>

<span data-ttu-id="3b0c0-424">資源檔是一種實用的機制，可讓您將可當地語系化的字串與代碼區隔開來。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-424">A resource file is a useful mechanism for separating localizable strings from code.</span></span> <span data-ttu-id="3b0c0-425">非預設語言的翻譯字串會在 *.resx*資源檔中隔離。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-425">Translated strings for the non-default language are isolated in *.resx* resource files.</span></span> <span data-ttu-id="3b0c0-426">例如，您可以建立名為 *Welcome.es.resx* 的西班牙文資源檔，以包含翻譯的字串。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-426">For example, you might want to create Spanish resource file named *Welcome.es.resx* containing translated strings.</span></span> <span data-ttu-id="3b0c0-427">"es" 是西班牙文的語言代碼。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-427">"es" is the language code for Spanish.</span></span> <span data-ttu-id="3b0c0-428">若要在 Visual Studio 中建立這個資源檔：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-428">To create this resource file in Visual Studio:</span></span>

1. <span data-ttu-id="3b0c0-429">在方案總管\*\*\*\* 中，以滑鼠右鍵按一下要放置資源檔的資料夾 > [新增]**[新增項目]** > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-429">In **Solution Explorer**, right click on the folder which will contain the resource file > **Add** > **New Item**.</span></span>

    ![巢狀特色選單：方案總管會開啟 [資源] 的特色選單，](localization/_static/newi.png)

2. <span data-ttu-id="3b0c0-432">在 [Search installed templates] (搜尋已安裝的範本)\*\*\*\* 方塊中，輸入「資源」，並命名檔案。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-432">In the **Search installed templates** box, enter "resource" and name the file.</span></span>

    ![[新增項目] 對話方塊](localization/_static/res.png)

3. <span data-ttu-id="3b0c0-434">在 [名稱]\*\*\*\* 資料行中輸入索引鍵值 (原生字串)，並在 [值]\*\*\*\* 資料行中輸入已翻譯的字串。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-434">Enter the key value (native string) in the **Name** column and the translated string in the **Value** column.</span></span>

    ![Welcome.es.resx 檔案 (西班牙文的「歡迎使用」資源檔)，其中 [名稱] 資料行的文字為 Hello，而 [值] 資料行的文字為 Hola (Hello 的西班牙文)](localization/_static/hola.png)

    <span data-ttu-id="3b0c0-436">Visual Studio 會顯示 *Welcome.es.resx* 檔案。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-436">Visual Studio shows the *Welcome.es.resx* file.</span></span>

    ![方案總管，其中顯示「歡迎使用」的西班牙文 (es) 資源檔](localization/_static/se.png)

## <a name="resource-file-naming"></a><span data-ttu-id="3b0c0-438">資源檔命名</span><span class="sxs-lookup"><span data-stu-id="3b0c0-438">Resource file naming</span></span>

<span data-ttu-id="3b0c0-439">資源的命名方式是以其類別的完整類型名稱去掉組件名稱而得。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-439">Resources are named for the full type name of their class minus the assembly name.</span></span> <span data-ttu-id="3b0c0-440">例如，假設專案中的法文資源是 `LocalizationWebsite.Web.Startup` 類別、主要組件為 `LocalizationWebsite.Web.dll`，就會命名為 *Startup.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-440">For example, a French resource in a project whose main assembly is `LocalizationWebsite.Web.dll` for the class `LocalizationWebsite.Web.Startup` would be named *Startup.fr.resx*.</span></span> <span data-ttu-id="3b0c0-441">若是 `LocalizationWebsite.Web.Controllers.HomeController` 類別的資源，則應命名為 *Controllers.HomeController.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-441">A resource for the class `LocalizationWebsite.Web.Controllers.HomeController` would be named *Controllers.HomeController.fr.resx*.</span></span> <span data-ttu-id="3b0c0-442">如果目標類別的命名空間和組件名稱不相同，則需要使用完整類型名稱。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-442">If your targeted class's namespace isn't the same as the assembly name you will need the full type name.</span></span> <span data-ttu-id="3b0c0-443">比方說，範例專案中 `ExtraNamespace.Tools` 類型的資源會命名為 *ExtraNamespace.Tools.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-443">For example, in the sample project a resource for the type `ExtraNamespace.Tools` would be named *ExtraNamespace.Tools.fr.resx*.</span></span>

<span data-ttu-id="3b0c0-444">在範例專案中，`ConfigureServices` 方法會將 `ResourcesPath` 設為 "Resources"，因此首頁控制器的法文資源檔專案相對路徑即為 *Resources/Controllers.HomeController.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-444">In the sample project, the `ConfigureServices` method sets the `ResourcesPath` to "Resources", so the project relative path for the home controller's French resource file is *Resources/Controllers.HomeController.fr.resx*.</span></span> <span data-ttu-id="3b0c0-445">或者，您可以使用資料夾來收集資源檔。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-445">Alternatively, you can use folders to organize resource files.</span></span> <span data-ttu-id="3b0c0-446">若是首頁控制器，路徑就是 *Resources/Controllers/HomeController.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-446">For the home controller, the path would be *Resources/Controllers/HomeController.fr.resx*.</span></span> <span data-ttu-id="3b0c0-447">如果您不使用 `ResourcesPath` 選項，*.resx* 檔案即會放置在專案的基底目錄中。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-447">If you don't use the `ResourcesPath` option, the *.resx* file would go in the project base directory.</span></span> <span data-ttu-id="3b0c0-448">`HomeController` 的資源檔會命名為 *Controllers.HomeController.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-448">The resource file for `HomeController` would be named *Controllers.HomeController.fr.resx*.</span></span> <span data-ttu-id="3b0c0-449">您可依據自己的資源檔收集方式，來選擇要使用點或路徑的命名慣例。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-449">The choice of using the dot or path naming convention depends on how you want to organize your resource files.</span></span>

| <span data-ttu-id="3b0c0-450">資源名稱</span><span class="sxs-lookup"><span data-stu-id="3b0c0-450">Resource name</span></span> | <span data-ttu-id="3b0c0-451">點或路徑命名</span><span class="sxs-lookup"><span data-stu-id="3b0c0-451">Dot or path naming</span></span> |
| ------------   | ------------- |
| <span data-ttu-id="3b0c0-452">Resources/Controllers.HomeController.fr.resx</span><span class="sxs-lookup"><span data-stu-id="3b0c0-452">Resources/Controllers.HomeController.fr.resx</span></span> | <span data-ttu-id="3b0c0-453">點</span><span class="sxs-lookup"><span data-stu-id="3b0c0-453">Dot</span></span>  |
| <span data-ttu-id="3b0c0-454">Resources/Controllers/HomeController.fr.resx</span><span class="sxs-lookup"><span data-stu-id="3b0c0-454">Resources/Controllers/HomeController.fr.resx</span></span>  | <span data-ttu-id="3b0c0-455">路徑</span><span class="sxs-lookup"><span data-stu-id="3b0c0-455">Path</span></span> |
|    |     |

<span data-ttu-id="3b0c0-456">在 views 中使用的資源檔會 `@inject IViewLocalizer` Razor 遵循類似的模式。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-456">Resource files using `@inject IViewLocalizer` in Razor views follow a similar pattern.</span></span> <span data-ttu-id="3b0c0-457">您可以使用點命名或路徑命名方式，來命名檢視的資源檔。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-457">The resource file for a view can be named using either dot naming or path naming.</span></span> Razor<span data-ttu-id="3b0c0-458">查看資源檔模擬其相關聯之視圖檔案的路徑。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-458"> view resource files mimic the path of their associated view file.</span></span> <span data-ttu-id="3b0c0-459">假設我們將 `ResourcesPath` 設為 "Resources"，與 *Views/Home/About.cshtml* 檢視建立關聯的法文資源檔可為下列其一：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-459">Assuming we set the `ResourcesPath` to "Resources", the French resource file associated with the *Views/Home/About.cshtml* view could be either of the following:</span></span>

* <span data-ttu-id="3b0c0-460">Resources/Views/Home/About.fr.resx</span><span class="sxs-lookup"><span data-stu-id="3b0c0-460">Resources/Views/Home/About.fr.resx</span></span>

* <span data-ttu-id="3b0c0-461">Resources/Views.Home.About.fr.resx</span><span class="sxs-lookup"><span data-stu-id="3b0c0-461">Resources/Views.Home.About.fr.resx</span></span>

<span data-ttu-id="3b0c0-462">如果您不使用 `ResourcesPath` 選項，則檢視的 *.resx* 檔案會與檢視位於相同資料夾中。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-462">If you don't use the `ResourcesPath` option, the *.resx* file for a view would be located in the same folder as the view.</span></span>

### <a name="rootnamespaceattribute"></a><span data-ttu-id="3b0c0-463">RootNamespaceAttribute</span><span class="sxs-lookup"><span data-stu-id="3b0c0-463">RootNamespaceAttribute</span></span> 

<span data-ttu-id="3b0c0-464">[RootNamespace](/dotnet/api/microsoft.extensions.localization.rootnamespaceattribute?view=aspnetcore-2.1) 屬性會在組件的根命名空間與組件名稱不同時，提供組件的根命名空間。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-464">The [RootNamespace](/dotnet/api/microsoft.extensions.localization.rootnamespaceattribute?view=aspnetcore-2.1) attribute provides the root namespace of an assembly when the root namespace of an assembly is different than the assembly name.</span></span> 

> [!WARNING]
> <span data-ttu-id="3b0c0-465">當專案的名稱不是有效的 .NET 識別碼時，就可能發生這種情況。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-465">This can occur when a project's name is not a valid .NET identifier.</span></span> <span data-ttu-id="3b0c0-466">例如， `my-project-name.csproj` 會使用根命名空間 `my_project_name` ，以及 `my-project-name` 導致此錯誤的元件名稱。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-466">For instance `my-project-name.csproj` will use the root namespace `my_project_name` and the assembly name `my-project-name` leading to this error.</span></span> 

<span data-ttu-id="3b0c0-467">如果組件的根命名空間與組件名稱不同：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-467">If the root namespace of an assembly is different than the assembly name:</span></span>

* <span data-ttu-id="3b0c0-468">根據預設，當地語系化無法運作。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-468">Localization does not work by default.</span></span>
* <span data-ttu-id="3b0c0-469">在組件中搜尋資源的方式造成當地語系化失敗。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-469">Localization fails due to the way resources are searched for within the assembly.</span></span> <span data-ttu-id="3b0c0-470">`RootNamespace` 是建置時間值，無法用於執行處理序。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-470">`RootNamespace` is a build-time value which is not available to the executing process.</span></span> 

<span data-ttu-id="3b0c0-471">如果 `RootNamespace` 與 `AssemblyName` 不同，請將下列內容納入 *AssemblyInfo.cs* (參數值取代為實際值)：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-471">If the `RootNamespace` is different from the `AssemblyName`, include the following in *AssemblyInfo.cs* (with parameter values replaced with the actual values):</span></span>

```csharp
using System.Reflection;
using Microsoft.Extensions.Localization;

[assembly: ResourceLocation("Resource Folder Name")]
[assembly: RootNamespace("App Root Namespace")]
```

<span data-ttu-id="3b0c0-472">上述程式碼可成功解析 resx 檔案。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-472">The preceding code enables the successful resolution of resx files.</span></span>

## <a name="culture-fallback-behavior"></a><span data-ttu-id="3b0c0-473">文化特性後援行為</span><span class="sxs-lookup"><span data-stu-id="3b0c0-473">Culture fallback behavior</span></span>

<span data-ttu-id="3b0c0-474">搜尋資源時，當地語系化會使用「文化特性後援」。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-474">When searching for a resource, localization engages in "culture fallback".</span></span> <span data-ttu-id="3b0c0-475">從所要求的文化特性開始，如果找不到，就會還原成該文化特性的父文化特性。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-475">Starting from the requested culture, if not found, it reverts to the parent culture of that culture.</span></span> <span data-ttu-id="3b0c0-476">另外，[CultureInfo.Parent](/dotnet/api/system.globalization.cultureinfo.parent) 屬性代表父文化特性。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-476">As an aside, the [CultureInfo.Parent](/dotnet/api/system.globalization.cultureinfo.parent) property represents the parent culture.</span></span> <span data-ttu-id="3b0c0-477">這通常 (但並非一定) 表示從 ISO 中移除國家意符 (Signifier)。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-477">This usually (but not always) means removing the national signifier from the ISO.</span></span> <span data-ttu-id="3b0c0-478">例如，墨西哥的西班牙文方言是 "es-MX"。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-478">For example, the dialect of Spanish spoken in Mexico is "es-MX".</span></span> <span data-ttu-id="3b0c0-479">它具有父系 "es" &mdash; 西班牙文不是任何國家/地區的特定項目。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-479">It has the parent "es"&mdash;Spanish non-specific to any country.</span></span>

<span data-ttu-id="3b0c0-480">假設您的網站收到使用文化特性 "fr-CA" 之 "Welcome" 資源的要求。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-480">Imagine your site receives a request for a "Welcome" resource using culture "fr-CA".</span></span> <span data-ttu-id="3b0c0-481">當地語系化系統會依照順序尋找下列資源，並選取第一個相符項目：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-481">The localization system looks for the following resources, in order, and selects the first match:</span></span>

* <span data-ttu-id="3b0c0-482">*Welcome.fr-CA.resx*</span><span class="sxs-lookup"><span data-stu-id="3b0c0-482">*Welcome.fr-CA.resx*</span></span>
* <span data-ttu-id="3b0c0-483">*Welcome.fr.resx*</span><span class="sxs-lookup"><span data-stu-id="3b0c0-483">*Welcome.fr.resx*</span></span>
* <span data-ttu-id="3b0c0-484">*Welcome.resx* (如果 `NeutralResourcesLanguage` 是 "fr-CA")</span><span class="sxs-lookup"><span data-stu-id="3b0c0-484">*Welcome.resx* (if the `NeutralResourcesLanguage` is "fr-CA")</span></span>

<span data-ttu-id="3b0c0-485">舉例來說，如果您移除 ".fr" 文化特性指示項，並將文化特性設定為法文，則系統會讀取預設資源檔，並將字串當地語系化。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-485">As an example, if you remove the ".fr" culture designator and you have the culture set to French, the default resource file is read and strings are localized.</span></span> <span data-ttu-id="3b0c0-486">當沒有任何項目符合您要求的文化特性時，資源管理員即會指定預設資源或後援資源。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-486">The Resource manager designates a default or fallback resource for when nothing meets your requested culture.</span></span> <span data-ttu-id="3b0c0-487">如果您只想在要求的文化特性缺少資源時傳回索引鍵，就不能使用預設資源檔。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-487">If you want to just return the key when missing a resource for the requested culture you must not have a default resource file.</span></span>

### <a name="generate-resource-files-with-visual-studio"></a><span data-ttu-id="3b0c0-488">使用 Visual Studio 產生資源檔</span><span class="sxs-lookup"><span data-stu-id="3b0c0-488">Generate resource files with Visual Studio</span></span>

<span data-ttu-id="3b0c0-489">如果您在 Visual Studio 中建立資源檔，但檔案名稱中不含文化特性 (例如 *Welcome.resx*)，則 Visual Studio 會針對每個字串的屬性建立 C# 類別。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-489">If you create a resource file in Visual Studio without a culture in the file name (for example, *Welcome.resx*), Visual Studio will create a C# class with a property for each string.</span></span> <span data-ttu-id="3b0c0-490">但這通常不是您使用 ASP.NET Core 的初衷。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-490">That's usually not what you want with ASP.NET Core.</span></span> <span data-ttu-id="3b0c0-491">一般來說，您不會有預設的 *.resx* 資源檔 (不含文化特性名稱的 *.resx* 檔案)。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-491">You typically don't have a default *.resx* resource file (a *.resx* file without the culture name).</span></span> <span data-ttu-id="3b0c0-492">因此，建議您建立含有文化特性名稱的 *.resx* 檔案 (例如 *Welcome.fr.resx*)。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-492">We suggest you create the *.resx* file with a culture name (for example *Welcome.fr.resx*).</span></span> <span data-ttu-id="3b0c0-493">當您建立含有文化特性名稱的 *.resx* 檔案時，Visual Studio 就不會產生類別檔案。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-493">When you create a *.resx* file with a culture name, Visual Studio won't generate the class file.</span></span>

### <a name="add-other-cultures"></a><span data-ttu-id="3b0c0-494">新增其他文化特性</span><span class="sxs-lookup"><span data-stu-id="3b0c0-494">Add other cultures</span></span>

<span data-ttu-id="3b0c0-495">每種語言和文化特性的組合 (非預設語言) 都需要唯一的資源檔。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-495">Each language and culture combination (other than the default language) requires a unique resource file.</span></span> <span data-ttu-id="3b0c0-496">若要建立不同文化特性和地區設定的資源檔，您可以建立新的資源檔並將 ISO 語言代碼作為檔名的一部分 (例如 **en-us**、**fr-ca** 和 **en-gb**)。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-496">You create resource files for different cultures and locales by creating new resource files in which the ISO language codes are part of the file name (for example, **en-us**, **fr-ca**, and **en-gb**).</span></span> <span data-ttu-id="3b0c0-497">您應將 ISO 代碼置於檔案名稱和 *.resx* 副檔名之間，例如 *Welcome.es-MX.resx* (西班牙文/墨西哥)。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-497">These ISO codes are placed between the file name and the *.resx* file extension, as in *Welcome.es-MX.resx* (Spanish/Mexico).</span></span>

## <a name="implement-a-strategy-to-select-the-languageculture-for-each-request"></a><span data-ttu-id="3b0c0-498">實作可依據每項要求選取語言/文化特性的策略</span><span class="sxs-lookup"><span data-stu-id="3b0c0-498">Implement a strategy to select the language/culture for each request</span></span>

### <a name="configure-localization"></a><span data-ttu-id="3b0c0-499">設定當地語系化</span><span class="sxs-lookup"><span data-stu-id="3b0c0-499">Configure localization</span></span>

<span data-ttu-id="3b0c0-500">您可以在 `Startup.ConfigureServices` 方法中設定當地語系化：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-500">Localization is configured in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](localization/sample/Localization/Startup.cs?name=snippet1)]

* <span data-ttu-id="3b0c0-501">`AddLocalization` 可將當地語系化服務新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-501">`AddLocalization` Adds the localization services to the services container.</span></span> <span data-ttu-id="3b0c0-502">上方的程式碼也會將資源路徑設為 "Resources"。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-502">The code above also sets the resources path to "Resources".</span></span>

* <span data-ttu-id="3b0c0-503">`AddViewLocalization` 可支援當地語系化的檢視檔案。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-503">`AddViewLocalization` Adds support for localized view files.</span></span> <span data-ttu-id="3b0c0-504">在此範例中，檢視的當地語系化會以檢視檔案的後置詞為依據。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-504">In this sample view localization is based on the view file suffix.</span></span> <span data-ttu-id="3b0c0-505">例如 *Index.fr.cshtml* 檔案中的 "fr"。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-505">For example "fr" in the *Index.fr.cshtml* file.</span></span>

* <span data-ttu-id="3b0c0-506">`AddDataAnnotationsLocalization` 可支援透過 `IStringLocalizer` 抽象概念而來的當地語系化 `DataAnnotations` 驗證訊息。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-506">`AddDataAnnotationsLocalization` Adds support for localized `DataAnnotations` validation messages through `IStringLocalizer` abstractions.</span></span>

### <a name="localization-middleware"></a><span data-ttu-id="3b0c0-507">當地語系化中介軟體</span><span class="sxs-lookup"><span data-stu-id="3b0c0-507">Localization middleware</span></span>

<span data-ttu-id="3b0c0-508">您可以在當地語系化[中介軟體](xref:fundamentals/middleware/index)中，設定要求目前的文化特性。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-508">The current culture on a request is set in the localization [Middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="3b0c0-509">已在 `Startup.Configure` 方法中啟用當地語系化中介軟體。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-509">The localization middleware is enabled in the `Startup.Configure` method.</span></span> <span data-ttu-id="3b0c0-510">您必須在可能檢查要求文化特性的任何中介軟體之前，設定當地語系化中介軟體 (例如 `app.UseMvcWithDefaultRoute()`)。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-510">The localization middleware must be configured before any middleware which might check the request culture (for example, `app.UseMvcWithDefaultRoute()`).</span></span>

[!code-csharp[](localization/sample/Localization/Startup.cs?name=snippet2)]
[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

<span data-ttu-id="3b0c0-511">`UseRequestLocalization` 會初始化 `RequestLocalizationOptions` 物件。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-511">`UseRequestLocalization` initializes a `RequestLocalizationOptions` object.</span></span> <span data-ttu-id="3b0c0-512">在每次要求時，系統會列舉 `RequestLocalizationOptions` 中的 `RequestCultureProvider` 清單，並使用能成功判斷要求的文化特性的第一個提供者。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-512">On every request the list of `RequestCultureProvider` in the `RequestLocalizationOptions` is enumerated and the first provider that can successfully determine the request culture is used.</span></span> <span data-ttu-id="3b0c0-513">預設的提供者是來自 `RequestLocalizationOptions` 類別：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-513">The default providers come from the `RequestLocalizationOptions` class:</span></span>

1. `QueryStringRequestCultureProvider`
2. `CookieRequestCultureProvider`
3. `AcceptLanguageHeaderRequestCultureProvider`

<span data-ttu-id="3b0c0-514">預設清單會由針對性高到低來排列。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-514">The default list goes from most specific to least specific.</span></span> <span data-ttu-id="3b0c0-515">在本文稍後，我們會說明如何變更順序，甚至新增自訂的文化特性提供者。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-515">Later in the article we'll see how you can change the order and even add a custom culture provider.</span></span> <span data-ttu-id="3b0c0-516">如果沒有任何提供者可以判斷要求的文化特性，即會使用 `DefaultRequestCulture`。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-516">If none of the providers can determine the request culture, the `DefaultRequestCulture` is used.</span></span>

### <a name="querystringrequestcultureprovider"></a><span data-ttu-id="3b0c0-517">QueryStringRequestCultureProvider</span><span class="sxs-lookup"><span data-stu-id="3b0c0-517">QueryStringRequestCultureProvider</span></span>

<span data-ttu-id="3b0c0-518">有些應用程式會使用查詢字串來設定[文化特性和 UI 文化特性](https://msdn.microsoft.com/library/system.globalization.cultureinfo.aspx)。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-518">Some apps will use a query string to set the [culture and UI culture](https://msdn.microsoft.com/library/system.globalization.cultureinfo.aspx).</span></span> <span data-ttu-id="3b0c0-519">若是使用 Cookie 或 Accept-Language 標頭方法的應用程式，您可以將查詢字串新增至 URL 以偵錯和測試程式碼。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-519">For apps that use the cookie or Accept-Language header approach, adding a query string to the URL is useful for debugging and testing code.</span></span> <span data-ttu-id="3b0c0-520">系統預設會將 `QueryStringRequestCultureProvider` 登錄為 `RequestCultureProvider` 清單中的第一個當地語系化提供者。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-520">By default, the `QueryStringRequestCultureProvider` is registered as the first localization provider in the `RequestCultureProvider` list.</span></span> <span data-ttu-id="3b0c0-521">您應傳遞查詢字串參數 `culture` 和 `ui-culture`。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-521">You pass the query string parameters `culture` and `ui-culture`.</span></span> <span data-ttu-id="3b0c0-522">下列範例會設定西班牙文/墨西哥的特定文化特性 (語言和地區)：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-522">The following example sets the specific culture (language and region) to Spanish/Mexico:</span></span>

   `http://localhost:5000/?culture=es-MX&ui-culture=es-MX`

<span data-ttu-id="3b0c0-523">如果您只傳入兩者其一 (`culture` 或 `ui-culture`)，查詢字串提供者就會使用您傳入的項目來設定這兩個值。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-523">If you only pass in one of the two (`culture` or `ui-culture`), the query string provider will set both values using the one you passed in.</span></span> <span data-ttu-id="3b0c0-524">例如，若只設定文化特性，即會同時設定 `Culture` 和 `UICulture`：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-524">For example, setting just the culture will set both the `Culture` and the `UICulture`:</span></span>

   `http://localhost:5000/?culture=es-MX`

### <a name="cookierequestcultureprovider"></a><span data-ttu-id="3b0c0-525">CookieRequestCultureProvider</span><span class="sxs-lookup"><span data-stu-id="3b0c0-525">CookieRequestCultureProvider</span></span>

<span data-ttu-id="3b0c0-526">生產環境應用程式通常會提供一個機制，來設定 ASP.NET Core 文化特性 Cookie 的文化特性。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-526">Production apps will often provide a mechanism to set the culture with the ASP.NET Core culture cookie.</span></span> <span data-ttu-id="3b0c0-527">若要建立 Cookie，請使用 `MakeCookieValue` 方法。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-527">Use the `MakeCookieValue` method to create a cookie.</span></span>

<span data-ttu-id="3b0c0-528">會傳回 `CookieRequestCultureProvider` `DefaultCookieName` 用來追蹤使用者慣用文化特性資訊的預設 cookie 名稱。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-528">The `CookieRequestCultureProvider` `DefaultCookieName` returns the default cookie name used to track the user's preferred culture information.</span></span> <span data-ttu-id="3b0c0-529">預設 Cookie 名稱為 `.AspNetCore.Culture`。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-529">The default cookie name is `.AspNetCore.Culture`.</span></span>

<span data-ttu-id="3b0c0-530">Cookie 格式為 `c=%LANGCODE%|uic=%LANGCODE%`，其中 `c` 是 `Culture` 而 `uic` 是 `UICulture`，例如：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-530">The cookie format is `c=%LANGCODE%|uic=%LANGCODE%`, where `c` is `Culture` and `uic` is `UICulture`, for example:</span></span>

    c=en-UK|uic=en-US

<span data-ttu-id="3b0c0-531">如果您只指定文化特性資訊和 UI 文化特性其一，系統就會將您所指定的文化特性用於文化特性資訊和 UI 文化特性。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-531">If you only specify one of culture info and UI culture, the specified culture will be used for both culture info and UI culture.</span></span>

### <a name="the-accept-language-http-header"></a><span data-ttu-id="3b0c0-532">Accept-Language HTTP 標頭</span><span class="sxs-lookup"><span data-stu-id="3b0c0-532">The Accept-Language HTTP header</span></span>

<span data-ttu-id="3b0c0-533">您可在大多數的瀏覽器中設定 [Accept-Language 標頭](https://www.w3.org/International/questions/qa-accept-lang-locales)，其最初的設計目的是用來指定使用者的語言。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-533">The [Accept-Language header](https://www.w3.org/International/questions/qa-accept-lang-locales) is settable in most browsers and was originally intended to specify the user's language.</span></span> <span data-ttu-id="3b0c0-534">這項設定可指出瀏覽器已設好要傳送哪些項目，或已從基礎作業系統繼承哪些項目。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-534">This setting indicates what the browser has been set to send or has inherited from the underlying operating system.</span></span> <span data-ttu-id="3b0c0-535">透過瀏覽器要求的 Accept-Language HTTP 標頭來偵測使用者的慣用語言，並非萬無一失 (請參閱 [Setting language preferences in a browser](https://www.w3.org/International/questions/qa-lang-priorities.en.php) (在瀏覽器中設定語言喜好設定)。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-535">The Accept-Language HTTP header from a browser request isn't an infallible way to detect the user's preferred language (see [Setting language preferences in a browser](https://www.w3.org/International/questions/qa-lang-priorities.en.php)).</span></span> <span data-ttu-id="3b0c0-536">生產環境應用程式應該包含可讓使用者自訂文化特性的選擇方式。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-536">A production app should include a way for a user to customize their choice of culture.</span></span>

### <a name="set-the-accept-language-http-header-in-ie"></a><span data-ttu-id="3b0c0-537">在 IE 中設定 Accept-Language HTTP 標頭</span><span class="sxs-lookup"><span data-stu-id="3b0c0-537">Set the Accept-Language HTTP header in IE</span></span>

1. <span data-ttu-id="3b0c0-538">從齒輪圖示，點選 [網際網路選項]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-538">From the gear icon, tap **Internet Options**.</span></span>

2. <span data-ttu-id="3b0c0-539">點選 [語言]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-539">Tap **Languages**.</span></span>

    ![網際網路選項](localization/_static/lang.png)

3. <span data-ttu-id="3b0c0-541">點選 [設定語言喜好設定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-541">Tap **Set Language Preferences**.</span></span>

4. <span data-ttu-id="3b0c0-542">點選 [新增語言]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-542">Tap **Add a language**.</span></span>

5. <span data-ttu-id="3b0c0-543">新增語言。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-543">Add the language.</span></span>

6. <span data-ttu-id="3b0c0-544">點選語言，然後點選 [上移]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-544">Tap the language, then tap **Move Up**.</span></span>

### <a name="use-a-custom-provider"></a><span data-ttu-id="3b0c0-545">使用自訂提供者</span><span class="sxs-lookup"><span data-stu-id="3b0c0-545">Use a custom provider</span></span>

<span data-ttu-id="3b0c0-546">假設您想要讓客戶將他們的語言和文化特性儲存在您的資料庫中。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-546">Suppose you want to let your customers store their language and culture in your databases.</span></span> <span data-ttu-id="3b0c0-547">您可以撰寫提供者，以供使用者查詢這些值。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-547">You could write a provider to look up these values for the user.</span></span> <span data-ttu-id="3b0c0-548">下列程式碼會示範如何新增自訂提供者：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-548">The following code shows how to add a custom provider:</span></span>

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

<span data-ttu-id="3b0c0-549">使用 `RequestLocalizationOptions` 新增或移除當地語系化提供者。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-549">Use `RequestLocalizationOptions` to add or remove localization providers.</span></span>

### <a name="set-the-culture-programmatically"></a><span data-ttu-id="3b0c0-550">以程式設計方式來設定文化特性</span><span class="sxs-lookup"><span data-stu-id="3b0c0-550">Set the culture programmatically</span></span>

<span data-ttu-id="3b0c0-551">[GitHub](https://github.com/aspnet/entropy) 上的這個範例 **Localization.StarterWeb** 專案包含可設定 `Culture` 的 UI。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-551">This sample **Localization.StarterWeb** project on [GitHub](https://github.com/aspnet/entropy) contains UI to set the `Culture`.</span></span> <span data-ttu-id="3b0c0-552">*Views/Shared/_SelectLanguagePartial.cshtml* 檔可讓您從支援的文化特性清單中選取文化特性：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-552">The *Views/Shared/_SelectLanguagePartial.cshtml* file allows you to select the culture from the list of supported cultures:</span></span>

[!code-cshtml[](localization/sample/Localization/Views/Shared/_SelectLanguagePartial.cshtml)]

<span data-ttu-id="3b0c0-553">系統會將 *Views/Shared/_SelectLanguagePartial.cshtml* 檔案新增至配置檔案的 `footer` 區段，以供所有檢視使用：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-553">The *Views/Shared/_SelectLanguagePartial.cshtml* file is added to the `footer` section of the layout file so it will be available to all views:</span></span>

[!code-cshtml[](localization/sample/Localization/Views/Shared/_Layout.cshtml?range=43-56&highlight=10)]

<span data-ttu-id="3b0c0-554">`SetLanguage` 方法會設定文化特性的 Cookie。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-554">The `SetLanguage` method sets the culture cookie.</span></span>

[!code-csharp[](localization/sample/Localization/Controllers/HomeController.cs?range=57-67)]

<span data-ttu-id="3b0c0-555">您無法將 *_SelectLanguagePartial.cshtml* 插入這個專案的範例程式碼。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-555">You can't plug in the *_SelectLanguagePartial.cshtml* to sample code for this project.</span></span> <span data-ttu-id="3b0c0-556">[GitHub](https://github.com/aspnet/entropy)上的**localization.starterweb**專案具有程式碼，可透過相依性 `RequestLocalizationOptions` 插入容器將傳遞至 Razor 部分。 [Dependency Injection](dependency-injection.md)</span><span class="sxs-lookup"><span data-stu-id="3b0c0-556">The **Localization.StarterWeb** project on [GitHub](https://github.com/aspnet/entropy) has code to flow the `RequestLocalizationOptions` to a Razor partial through the [Dependency Injection](dependency-injection.md) container.</span></span>

## <a name="model-binding-route-data-and-query-strings"></a><span data-ttu-id="3b0c0-557">模型系結路由資料和查詢字串</span><span class="sxs-lookup"><span data-stu-id="3b0c0-557">Model binding route data and query strings</span></span>

<span data-ttu-id="3b0c0-558">請參閱模型系結[路由資料和查詢字串的全球化行為](xref:mvc/models/model-binding#glob)。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-558">See [Globalization behavior of model binding route data and query strings](xref:mvc/models/model-binding#glob).</span></span>

## <a name="globalization-and-localization-terms"></a><span data-ttu-id="3b0c0-559">全球化和當地語系化詞彙</span><span class="sxs-lookup"><span data-stu-id="3b0c0-559">Globalization and localization terms</span></span>

<span data-ttu-id="3b0c0-560">在進行應用程式的當地語系化程序時，您也需要具備現代軟體開發中常用的相關字元集基本知識，並了解與它們建立關聯的問題。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-560">The process of localizing your app also requires a basic understanding of relevant character sets commonly used in modern software development and an understanding of the issues associated with them.</span></span> <span data-ttu-id="3b0c0-561">雖然所有電腦都會將文字儲存為數字 (程式碼)，但不同系統會使用不同數字來儲存相同的文字。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-561">Although all computers store text as numbers (codes), different systems store the same text using different numbers.</span></span> <span data-ttu-id="3b0c0-562">當地語系化是指針對特定文化特性/地區設定，轉譯應用程式使用者介面 (UI) 的程序。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-562">The localization process refers to translating the app user interface (UI) for a specific culture/locale.</span></span>

<span data-ttu-id="3b0c0-563">[可當地語系化](/dotnet/standard/globalization-localization/localizability-review)是確認全球化應用程式已準備好進行當地語系化的中繼程序。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-563">[Localizability](/dotnet/standard/globalization-localization/localizability-review) is an intermediate process for verifying that a globalized app is ready for localization.</span></span>

<span data-ttu-id="3b0c0-564">文化特性名稱的 [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) 格式是 `<languagecode2>-<country/regioncode2>`，其中 `<languagecode2>` 是語言代碼，而 `<country/regioncode2>` 是子文化特性代碼。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-564">The [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) format for the culture name is `<languagecode2>-<country/regioncode2>`, where `<languagecode2>` is the language code and `<country/regioncode2>` is the subculture code.</span></span> <span data-ttu-id="3b0c0-565">例如，`es-CL` 是指西班牙文 (智利)，`en-US` 是指英文 (美國)，而 `en-AU` 是指英文 (澳大利亞)。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-565">For example, `es-CL` for Spanish (Chile), `en-US` for English (United States), and `en-AU` for English (Australia).</span></span> <span data-ttu-id="3b0c0-566">[RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) 是 ISO 639 (兩個小寫字母，為與某個語言建立關聯的文化特性代碼) 及 ISO 3166 (兩個大寫字母，為與某個國家或地區建立關聯的子文化特性代碼) 的組合。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-566">[RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) is a combination of an ISO 639 two-letter lowercase culture code associated with a language and an ISO 3166 two-letter uppercase subculture code associated with a country or region.</span></span> <span data-ttu-id="3b0c0-567">請參閱 [Language Culture Name](https://msdn.microsoft.com/library/ee825488(v=cs.20).aspx) (語言的文化特性名稱)。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-567">See [Language Culture Name](https://msdn.microsoft.com/library/ee825488(v=cs.20).aspx).</span></span>

<span data-ttu-id="3b0c0-568">國際化通常縮寫為 "I18N"。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-568">Internationalization is often abbreviated to "I18N".</span></span> <span data-ttu-id="3b0c0-569">這個縮寫是採用該詞彙的第一個和最後一個字母，以及這兩個字母間的字母數組成，因此 18 代表第一個字母 "I" 及最後 "N" 中間的字母數。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-569">The abbreviation takes the first and last letters and the number of letters between them, so 18 stands for the number of letters between the first "I" and the last "N".</span></span> <span data-ttu-id="3b0c0-570">同樣的原則也適用於全球化 (G11N) 與當地語系化 (L10N) 的縮寫。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-570">The same applies to Globalization (G11N), and Localization (L10N).</span></span>

<span data-ttu-id="3b0c0-571">詞彙：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-571">Terms:</span></span>

* <span data-ttu-id="3b0c0-572">全球化 (G11N)：讓應用程式支援不同語言和區域的程序。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-572">Globalization (G11N): The process of making an app support different languages and regions.</span></span>
* <span data-ttu-id="3b0c0-573">當地語系化 (L10N)：針對特定語言和區域，自訂應用程式的程序。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-573">Localization (L10N): The process of customizing an app for a given language and region.</span></span>
* <span data-ttu-id="3b0c0-574">國際化 (I18N)：包含全球化和當地語系化這兩部分的描述。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-574">Internationalization (I18N): Describes both globalization and localization.</span></span>
* <span data-ttu-id="3b0c0-575">文化特性：它是一種語言和/或地區。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-575">Culture: It's a language and, optionally, a region.</span></span>
* <span data-ttu-id="3b0c0-576">中性文化特性：具有指定的語言但不限地區的文化特性 </span><span class="sxs-lookup"><span data-stu-id="3b0c0-576">Neutral culture: A culture that has a specified language, but not a region.</span></span> <span data-ttu-id="3b0c0-577">(例如 "en"、"es")。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-577">(for example "en", "es")</span></span>
* <span data-ttu-id="3b0c0-578">特定文化特性：具有指定的語言和地區的文化特性 </span><span class="sxs-lookup"><span data-stu-id="3b0c0-578">Specific culture: A culture that has a specified language and region.</span></span> <span data-ttu-id="3b0c0-579">(例如 "en-US"、"en-GB"、"es-CL")。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-579">(for example "en-US", "en-GB", "es-CL")</span></span>
* <span data-ttu-id="3b0c0-580">父文化特性：包含特定文化特性的中性文化特性 </span><span class="sxs-lookup"><span data-stu-id="3b0c0-580">Parent culture: The neutral culture that contains a specific culture.</span></span> <span data-ttu-id="3b0c0-581">(例如，"en" 是 "en-US" 和 "en-GB" 的父文化特性)。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-581">(for example, "en" is the parent culture of "en-US" and "en-GB")</span></span>
* <span data-ttu-id="3b0c0-582">地區設定：地區設定與文化特性相同。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-582">Locale: A locale is the same as a culture.</span></span>

[!INCLUDE[](~/includes/localization/currency.md)]

## <a name="additional-resources"></a><span data-ttu-id="3b0c0-583">其他資源</span><span class="sxs-lookup"><span data-stu-id="3b0c0-583">Additional resources</span></span>

* <xref:fundamentals/troubleshoot-aspnet-core-localization>
* <span data-ttu-id="3b0c0-584">本文使用的 [Localization.StarterWeb 專案](https://github.com/aspnet/Entropy/tree/master/samples/Localization.StarterWeb)。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-584">[Localization.StarterWeb project](https://github.com/aspnet/Entropy/tree/master/samples/Localization.StarterWeb) used in the article.</span></span>
* [<span data-ttu-id="3b0c0-585">全球化與當地語系化 .NET 應用程式</span><span class="sxs-lookup"><span data-stu-id="3b0c0-585">Globalizing and localizing .NET applications</span></span>](/dotnet/standard/globalization-localization/index)
* [<span data-ttu-id="3b0c0-586">.Resx 檔案中的資源</span><span class="sxs-lookup"><span data-stu-id="3b0c0-586">Resources in .resx Files</span></span>](/dotnet/framework/resources/working-with-resx-files-programmatically)
* [<span data-ttu-id="3b0c0-587">Microsoft 多語應用程式工具組</span><span class="sxs-lookup"><span data-stu-id="3b0c0-587">Microsoft Multilingual App Toolkit</span></span>](https://marketplace.visualstudio.com/items?itemName=MultilingualAppToolkit.MultilingualAppToolkit-18308)
* [<span data-ttu-id="3b0c0-588">當地語系化和泛型</span><span class="sxs-lookup"><span data-stu-id="3b0c0-588">Localization & Generics</span></span>](http://hishambinateya.com/localization-and-generics)

::: moniker-end

<!-- ASP.NET Core 5.x starts here -->
::: moniker range="> aspnetcore-3.1"

<span data-ttu-id="3b0c0-589">由 [Rick Anderson](https://twitter.com/RickAndMSFT)、[Damien Bowden](https://twitter.com/damien_bod)、[Bart Calixto](https://twitter.com/bartmax)、[Nadeem Afana](https://afana.me/) 和 [Hisham Bin Ateya](https://twitter.com/hishambinateya) 提供</span><span class="sxs-lookup"><span data-stu-id="3b0c0-589">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Damien Bowden](https://twitter.com/damien_bod), [Bart Calixto](https://twitter.com/bartmax), [Nadeem Afana](https://afana.me/), and [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span></span>

<span data-ttu-id="3b0c0-590">多語系網站可讓網站觸及更多物件。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-590">A multilingual website allows the site to reach a wider audience.</span></span> <span data-ttu-id="3b0c0-591">ASP.NET Core 提供服務與中介軟體，可將網站當地語系化成不同的語言與文化特性。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-591">ASP.NET Core provides services and middleware for localizing into different languages and cultures.</span></span>

<span data-ttu-id="3b0c0-592">國際化包含[全球化](/dotnet/api/system.globalization)和[當地語系化](/dotnet/standard/globalization-localization/localization)這兩部分。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-592">Internationalization involves [Globalization](/dotnet/api/system.globalization) and [Localization](/dotnet/standard/globalization-localization/localization).</span></span> <span data-ttu-id="3b0c0-593">全球化是指設計出可支援不同文化特性之應用程式的程序。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-593">Globalization is the process of designing apps that support different cultures.</span></span> <span data-ttu-id="3b0c0-594">透過全球化，您可新增支援與特定地區相關之已定義語言指令碼的輸入、顯示及輸出作業。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-594">Globalization adds support for input, display, and output of a defined set of language scripts that relate to specific geographic areas.</span></span>

<span data-ttu-id="3b0c0-595">當地語系化是指針對全球化應用程式進行調整的程序，且您已順應特定文化特性/地區設定對這些全球化應用程式進行可當地語系化處理。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-595">Localization is the process of adapting a globalized app, which you have already processed for localizability, to a particular culture/locale.</span></span> <span data-ttu-id="3b0c0-596">如需詳細資訊，請參閱本文件結尾處的**全球化和當地語系化詞彙**。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-596">For more information see **Globalization and localization terms** near the end of this document.</span></span>

<span data-ttu-id="3b0c0-597">應用程式當地語系化包含下列作業：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-597">App localization involves the following:</span></span>

1. <span data-ttu-id="3b0c0-598">讓應用程式的內容可當地語系化</span><span class="sxs-lookup"><span data-stu-id="3b0c0-598">Make the app's content localizable</span></span>
1. <span data-ttu-id="3b0c0-599">針對您支援的語言和文化特性提供當地語系化資源</span><span class="sxs-lookup"><span data-stu-id="3b0c0-599">Provide localized resources for the languages and cultures you support</span></span>
1. <span data-ttu-id="3b0c0-600">實作可依據每項要求選取語言/文化特性的策略</span><span class="sxs-lookup"><span data-stu-id="3b0c0-600">Implement a strategy to select the language/culture for each request</span></span>

<span data-ttu-id="3b0c0-601">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/localization/sample/Localization)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="3b0c0-601">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/localization/sample/Localization) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="make-the-apps-content-localizable"></a><span data-ttu-id="3b0c0-602">讓應用程式的內容可當地語系化</span><span class="sxs-lookup"><span data-stu-id="3b0c0-602">Make the app's content localizable</span></span>

<span data-ttu-id="3b0c0-603"><xref:Microsoft.Extensions.Localization.IStringLocalizer>和 <xref:Microsoft.Extensions.Localization.IStringLocalizer%601> 已架構，可在開發當地語系化應用程式時改善生產力。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-603"><xref:Microsoft.Extensions.Localization.IStringLocalizer> and <xref:Microsoft.Extensions.Localization.IStringLocalizer%601> were architected to improve productivity when developing localized apps.</span></span> <span data-ttu-id="3b0c0-604">`IStringLocalizer`會使用 <xref:System.Resources.ResourceManager> 和， <xref:System.Resources.ResourceReader> 在執行時間提供特定文化特性的資源。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-604">`IStringLocalizer` uses the <xref:System.Resources.ResourceManager> and <xref:System.Resources.ResourceReader> to provide culture-specific resources at run time.</span></span> <span data-ttu-id="3b0c0-605">介面具有索引子和， `IEnumerable` 用於傳回當地語系化的字串。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-605">The interface has an indexer and an `IEnumerable` for returning localized strings.</span></span> <span data-ttu-id="3b0c0-606">`IStringLocalizer`不需要將預設語言字串儲存在資源檔中。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-606">`IStringLocalizer` doesn't require storing the default language strings in a resource file.</span></span> <span data-ttu-id="3b0c0-607">您不必在開發初期建立資源檔，即可開發以當地語系化為目標的應用程式。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-607">You can develop an app targeted for localization and not need to create resource files early in development.</span></span> <span data-ttu-id="3b0c0-608">下列程式碼會示範如何包裝 "About Title" 字串以進行當地語系化。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-608">The code below shows how to wrap the string "About Title" for localization.</span></span>

[!code-csharp[](localization/sample/Localization/Controllers/AboutController.cs)]

<span data-ttu-id="3b0c0-609">在上述程式碼中， `IStringLocalizer<T>` 執行是來自相依性[插入](dependency-injection.md)。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-609">In the preceding code, the `IStringLocalizer<T>` implementation comes from [Dependency Injection](dependency-injection.md).</span></span> <span data-ttu-id="3b0c0-610">如果找不到 "About Title" 的當地語系化值，即會傳回索引子的索引鍵，也就是 "About Title" 字串。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-610">If the localized value of "About Title" isn't found, then the indexer key is returned, that is, the string "About Title".</span></span> <span data-ttu-id="3b0c0-611">您可以保留應用程式中的預設語言常值字串，並將其包裝在當地語系化工具中，以便專注於開發應用程式。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-611">You can leave the default language literal strings in the app and wrap them in the localizer, so that you can focus on developing the app.</span></span> <span data-ttu-id="3b0c0-612">您不用先建立預設資源檔，即可使用預設語言來開發應用程式，並針對當地語系化步驟進行應用程式的準備。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-612">You develop your app with your default language and prepare it for the localization step without first creating a default resource file.</span></span> <span data-ttu-id="3b0c0-613">或者，您可以使用傳統方法，並提供索引鍵以擷取預設語言字串。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-613">Alternatively, you can use the traditional approach and provide a key to retrieve the default language string.</span></span> <span data-ttu-id="3b0c0-614">對許多開發人員來說，新的工作流程 (單純包裝字串常值而不使用預設語言的 *.resx* 檔案) 可以降低當地語系化應用程式的額外負荷。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-614">For many developers the new workflow of not having a default language *.resx* file and simply wrapping the string literals can reduce the overhead of localizing an app.</span></span> <span data-ttu-id="3b0c0-615">其他開發人員則偏好傳統的工作流程，因為這種方法更便於使用較長的字串常值，且更易於更新當地語系化的字串。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-615">Other developers will prefer the traditional work flow as it can make it easier to work with longer string literals and make it easier to update localized strings.</span></span>

<span data-ttu-id="3b0c0-616">若是包含 HTML 的資源，請使用 `IHtmlLocalizer<T>` 實作。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-616">Use the `IHtmlLocalizer<T>` implementation for resources that contain HTML.</span></span> <span data-ttu-id="3b0c0-617">`IHtmlLocalizer` 會對資源字串中經過格式化的引數進行 HTML 編碼，但不會對資源字串本身進行 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-617">`IHtmlLocalizer` HTML encodes arguments that are formatted in the resource string, but doesn't HTML encode the resource string itself.</span></span> <span data-ttu-id="3b0c0-618">在下列醒目提示的範例中，只有 `name` 參數的值經過 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-618">In the sample highlighted below, only the value of `name` parameter is HTML encoded.</span></span>

[!code-csharp[](../fundamentals/localization/sample/Localization/Controllers/BookController.cs?highlight=3,5,20&start=1&end=24)]

<span data-ttu-id="3b0c0-619">**注意：** 一般來說，您只想將文字當地語系化，而不是 HTML。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-619">**Note:** You generally want to only localize text and not HTML.</span></span>

<span data-ttu-id="3b0c0-620">您可以在最底層的[相依性插入](dependency-injection.md)中，將 `IStringLocalizerFactory` 移出：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-620">At the lowest level, you can get `IStringLocalizerFactory` out of [Dependency Injection](dependency-injection.md):</span></span>

[!code-csharp[](localization/sample/Localization/Controllers/TestController.cs?start=9&end=26&highlight=7-13)]

<span data-ttu-id="3b0c0-621">上述程式碼會示範這兩個 Factory Create 方法。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-621">The code above demonstrates each of the two factory create methods.</span></span>

<span data-ttu-id="3b0c0-622">您可以依據控制器或區域來分割當地語系化的字串，也可以只用一個容器。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-622">You can partition your localized strings by controller, area, or have just one container.</span></span> <span data-ttu-id="3b0c0-623">在範例應用程式中，會針對共用資源使用名為 `SharedResource` 的虛擬類別。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-623">In the sample app, a dummy class named `SharedResource` is used for shared resources.</span></span>

[!code-csharp[](localization/sample/Localization/Resources/SharedResource.cs)]

<span data-ttu-id="3b0c0-624">有些開發人員會使用 `Startup` 類別來包含全域或共用字串。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-624">Some developers use the `Startup` class to contain global or shared strings.</span></span> <span data-ttu-id="3b0c0-625">下列範例會使用 `InfoController` 和 `SharedResource` 當地語系化工具：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-625">In the sample below, the `InfoController` and the `SharedResource` localizers are used:</span></span>

[!code-csharp[](localization/sample/Localization/Controllers/InfoController.cs?range=9-26)]

## <a name="view-localization"></a><span data-ttu-id="3b0c0-626">檢視當地語系化</span><span class="sxs-lookup"><span data-stu-id="3b0c0-626">View localization</span></span>

<span data-ttu-id="3b0c0-627">`IViewLocalizer` 服務可提供[檢視](xref:mvc/views/overview)的當地語系化字串。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-627">The `IViewLocalizer` service provides localized strings for a [view](xref:mvc/views/overview).</span></span> <span data-ttu-id="3b0c0-628">`ViewLocalizer` 類別會實作這個介面，並透過檢視檔案路徑來找出資源的位置。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-628">The `ViewLocalizer` class implements this interface and finds the resource location from the view file path.</span></span> <span data-ttu-id="3b0c0-629">下列程式碼示範如何使用 `IViewLocalizer` 的預設實作：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-629">The following code shows how to use the default implementation of `IViewLocalizer`:</span></span>

[!code-cshtml[](localization/sample/Localization/Views/Home/About.cshtml)]

<span data-ttu-id="3b0c0-630">`IViewLocalizer` 的預設實作會依據檢視的檔案名稱來找出資源檔。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-630">The default implementation of `IViewLocalizer` finds the resource file based on the view's file name.</span></span> <span data-ttu-id="3b0c0-631">其中並沒有任何選項可以使用全域共用的資源檔。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-631">There's no option to use a global shared resource file.</span></span> <span data-ttu-id="3b0c0-632">`ViewLocalizer`使用來執行當地語系化工具 `IHtmlLocalizer` ，因此 Razor 不會對當地語系化字串進行 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-632">`ViewLocalizer` implements the localizer using `IHtmlLocalizer`, so Razor doesn't HTML encode the localized string.</span></span> <span data-ttu-id="3b0c0-633">您可以參數化資源字串，`IViewLocalizer` 即會對參數 (而不是資源字串) 進行 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-633">You can parameterize resource strings and `IViewLocalizer` will HTML encode the parameters, but not the resource string.</span></span> <span data-ttu-id="3b0c0-634">請考慮下列 Razor 標記：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-634">Consider the following Razor markup:</span></span>

```cshtml
@Localizer["<i>Hello</i> <b>{0}!</b>", UserManager.GetUserName(User)]
```

<span data-ttu-id="3b0c0-635">法文資源檔可能包含下列內容：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-635">A French resource file could contain the following:</span></span>

| <span data-ttu-id="3b0c0-636">Key</span><span class="sxs-lookup"><span data-stu-id="3b0c0-636">Key</span></span> | <span data-ttu-id="3b0c0-637">值</span><span class="sxs-lookup"><span data-stu-id="3b0c0-637">Value</span></span> |
| ----- | ------ |
| `<i>Hello</i> <b>{0}!</b>` | `<i>Bonjour</i> <b>{0} !</b>` |

<span data-ttu-id="3b0c0-638">轉譯的檢視內容可能包含來自資源檔的 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-638">The rendered view would contain the HTML markup from the resource file.</span></span>

<span data-ttu-id="3b0c0-639">**注意：** 一般來說，您只想將文字當地語系化，而不是 HTML。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-639">**Note:** You generally want to only localize text and not HTML.</span></span>

<span data-ttu-id="3b0c0-640">若要在檢視中使用共用的資源檔，請插入 `IHtmlLocalizer<T>`：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-640">To use a shared resource file in a view, inject `IHtmlLocalizer<T>`:</span></span>

[!code-cshtml[](../fundamentals/localization/sample/Localization/Views/Test/About.cshtml?highlight=5,12)]

## <a name="dataannotations-localization"></a><span data-ttu-id="3b0c0-641">DataAnnotations 當地語系化</span><span class="sxs-lookup"><span data-stu-id="3b0c0-641">DataAnnotations localization</span></span>

<span data-ttu-id="3b0c0-642">DataAnnotations 錯誤訊息會使用 `IStringLocalizer<T>` 來當地語系化。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-642">DataAnnotations error messages are localized with `IStringLocalizer<T>`.</span></span> <span data-ttu-id="3b0c0-643">使用 `ResourcesPath = "Resources"` 選項時，`RegisterViewModel` 中的錯誤訊息會儲存在下列路徑之一：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-643">Using the option `ResourcesPath = "Resources"`, the error messages in `RegisterViewModel` can be stored in either of the following paths:</span></span>

* <span data-ttu-id="3b0c0-644">*Resources/Viewmodel. RegisterViewModel. fr .resx*</span><span class="sxs-lookup"><span data-stu-id="3b0c0-644">*Resources/ViewModels.Account.RegisterViewModel.fr.resx*</span></span>
* <span data-ttu-id="3b0c0-645">*Resources/ViewModels/Account/RegisterViewModel.fr.resx*</span><span class="sxs-lookup"><span data-stu-id="3b0c0-645">*Resources/ViewModels/Account/RegisterViewModel.fr.resx*</span></span>

[!code-csharp[](localization/sample/Localization/ViewModels/Account/RegisterViewModel.cs?start=9&end=26)]

<span data-ttu-id="3b0c0-646">在 ASP.NET Core MVC 1.1.0 和更高版本中，系統會將非驗證屬性當地語系化。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-646">In ASP.NET Core MVC 1.1.0 and higher, non-validation attributes are localized.</span></span> <span data-ttu-id="3b0c0-647">ASP.NET Core MVC 1.0 **不會**查閱非驗證屬性的當地語系化字串。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-647">ASP.NET Core MVC 1.0 does **not** look up localized strings for non-validation attributes.</span></span>

<a name="one-resource-string-multiple-classes"></a>

### <a name="using-one-resource-string-for-multiple-classes"></a><span data-ttu-id="3b0c0-648">針對多個類別使用同一個資源字串</span><span class="sxs-lookup"><span data-stu-id="3b0c0-648">Using one resource string for multiple classes</span></span>

<span data-ttu-id="3b0c0-649">下列程式碼會示範如何針對含有多個類別的驗證屬性使用同一個資源字串：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-649">The following code shows how to use one resource string for validation attributes with multiple classes:</span></span>

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

<span data-ttu-id="3b0c0-650">在先前的程式碼中，`SharedResource` 是與儲存驗證訊息的 resx 對應的類別。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-650">In the preceding code, `SharedResource` is the class corresponding to the resx where your validation messages are stored.</span></span> <span data-ttu-id="3b0c0-651">使用這個方法時，DataAnnotations 只會使用 `SharedResource`，而不是每個類別的資源。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-651">With this approach, DataAnnotations will only use `SharedResource`, rather than the resource for each class.</span></span>

## <a name="provide-localized-resources-for-the-languages-and-cultures-you-support"></a><span data-ttu-id="3b0c0-652">針對您支援的語言和文化特性提供當地語系化資源</span><span class="sxs-lookup"><span data-stu-id="3b0c0-652">Provide localized resources for the languages and cultures you support</span></span>

### <a name="supportedcultures-and-supporteduicultures"></a><span data-ttu-id="3b0c0-653">SupportedCultures 和 SupportedUICultures</span><span class="sxs-lookup"><span data-stu-id="3b0c0-653">SupportedCultures and SupportedUICultures</span></span>

<span data-ttu-id="3b0c0-654">ASP.NET Core 可讓您指定 `SupportedCultures` 和 `SupportedUICultures` 這兩個文化特性值。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-654">ASP.NET Core allows you to specify two culture values, `SupportedCultures` and `SupportedUICultures`.</span></span> <span data-ttu-id="3b0c0-655">`SupportedCultures` 的 [CultureInfo](/dotnet/api/system.globalization.cultureinfo) 物件可決定文化特性相依函式的結果，例如日期、時間、數字及貨幣格式。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-655">The [CultureInfo](/dotnet/api/system.globalization.cultureinfo) object for `SupportedCultures` determines the results of culture-dependent functions, such as date, time, number, and currency formatting.</span></span> <span data-ttu-id="3b0c0-656">`SupportedCultures` 也可決定文字排列順序、大小寫慣例和字串比較。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-656">`SupportedCultures` also determines the sorting order of text, casing conventions, and string comparisons.</span></span> <span data-ttu-id="3b0c0-657">如需伺服器如何取得文化特性的詳細資訊，請參閱 [CultureInfo.CurrentCulture](/dotnet/api/system.stringcomparer.currentculture#System_StringComparer_CurrentCulture)。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-657">See [CultureInfo.CurrentCulture](/dotnet/api/system.stringcomparer.currentculture#System_StringComparer_CurrentCulture) for more info on how the server gets the Culture.</span></span> <span data-ttu-id="3b0c0-658">`SupportedUICultures`會決定[ResourceManager](/dotnet/api/system.resources.resourcemanager)會查閱哪些翻譯的字串（來自 *.resx*檔案）。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-658">The `SupportedUICultures` determines which translated strings (from *.resx* files) are looked up by the [ResourceManager](/dotnet/api/system.resources.resourcemanager).</span></span> <span data-ttu-id="3b0c0-659">`ResourceManager` 僅會查閱 `CurrentUICulture` 所決定之文化特性特有的字串。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-659">The `ResourceManager` simply looks up culture-specific strings that's determined by `CurrentUICulture`.</span></span> <span data-ttu-id="3b0c0-660">.NET 中的每個執行緒都有 `CurrentCulture` 和 `CurrentUICulture` 物件。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-660">Every thread in .NET has `CurrentCulture` and `CurrentUICulture` objects.</span></span> <span data-ttu-id="3b0c0-661">ASP.NET Core 會在轉譯文化特性相依函式時檢查這些值。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-661">ASP.NET Core inspects these values when rendering culture-dependent functions.</span></span> <span data-ttu-id="3b0c0-662">比方說，如果目前執行緒的文化特性設定為 "en-US" (英文 - 美國)，`DateTime.Now.ToLongDateString()` 會顯示 "Thursday, February 18, 2016"，但如果 `CurrentCulture` 設定為 "es-ES" (西班牙文 - 西班牙)，則輸出會是 "jueves, 18 de febrero de 2016"。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-662">For example, if the current thread's culture is set to "en-US" (English, United States), `DateTime.Now.ToLongDateString()` displays "Thursday, February 18, 2016", but if `CurrentCulture` is set to "es-ES" (Spanish, Spain) the output will be "jueves, 18 de febrero de 2016".</span></span>

## <a name="resource-files"></a><span data-ttu-id="3b0c0-663">資源檔</span><span class="sxs-lookup"><span data-stu-id="3b0c0-663">Resource files</span></span>

<span data-ttu-id="3b0c0-664">資源檔是一種實用的機制，可讓您將可當地語系化的字串與代碼區隔開來。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-664">A resource file is a useful mechanism for separating localizable strings from code.</span></span> <span data-ttu-id="3b0c0-665">非預設語言的翻譯字串會在 *.resx*資源檔中隔離。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-665">Translated strings for the non-default language are isolated in *.resx* resource files.</span></span> <span data-ttu-id="3b0c0-666">例如，您可以建立名為 *Welcome.es.resx* 的西班牙文資源檔，以包含翻譯的字串。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-666">For example, you might want to create Spanish resource file named *Welcome.es.resx* containing translated strings.</span></span> <span data-ttu-id="3b0c0-667">"es" 是西班牙文的語言代碼。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-667">"es" is the language code for Spanish.</span></span> <span data-ttu-id="3b0c0-668">若要在 Visual Studio 中建立這個資源檔：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-668">To create this resource file in Visual Studio:</span></span>

1. <span data-ttu-id="3b0c0-669">在方案總管\*\*\*\* 中，以滑鼠右鍵按一下要放置資源檔的資料夾 > [新增]**[新增項目]** > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-669">In **Solution Explorer**, right click on the folder which will contain the resource file > **Add** > **New Item**.</span></span>

    ![巢狀特色選單：方案總管會開啟 [資源] 的特色選單，](localization/_static/newi.png)

2. <span data-ttu-id="3b0c0-672">在 [Search installed templates] (搜尋已安裝的範本)\*\*\*\* 方塊中，輸入「資源」，並命名檔案。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-672">In the **Search installed templates** box, enter "resource" and name the file.</span></span>

    ![[新增項目] 對話方塊](localization/_static/res.png)

3. <span data-ttu-id="3b0c0-674">在 [名稱]\*\*\*\* 資料行中輸入索引鍵值 (原生字串)，並在 [值]\*\*\*\* 資料行中輸入已翻譯的字串。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-674">Enter the key value (native string) in the **Name** column and the translated string in the **Value** column.</span></span>

    ![Welcome.es.resx 檔案 (西班牙文的「歡迎使用」資源檔)，其中 [名稱] 資料行的文字為 Hello，而 [值] 資料行的文字為 Hola (Hello 的西班牙文)](localization/_static/hola.png)

    <span data-ttu-id="3b0c0-676">Visual Studio 會顯示 *Welcome.es.resx* 檔案。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-676">Visual Studio shows the *Welcome.es.resx* file.</span></span>

    ![方案總管，其中顯示「歡迎使用」的西班牙文 (es) 資源檔](localization/_static/se.png)

## <a name="resource-file-naming"></a><span data-ttu-id="3b0c0-678">資源檔命名</span><span class="sxs-lookup"><span data-stu-id="3b0c0-678">Resource file naming</span></span>

<span data-ttu-id="3b0c0-679">資源的命名方式是以其類別的完整類型名稱去掉組件名稱而得。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-679">Resources are named for the full type name of their class minus the assembly name.</span></span> <span data-ttu-id="3b0c0-680">例如，假設專案中的法文資源是 `LocalizationWebsite.Web.Startup` 類別、主要組件為 `LocalizationWebsite.Web.dll`，就會命名為 *Startup.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-680">For example, a French resource in a project whose main assembly is `LocalizationWebsite.Web.dll` for the class `LocalizationWebsite.Web.Startup` would be named *Startup.fr.resx*.</span></span> <span data-ttu-id="3b0c0-681">若是 `LocalizationWebsite.Web.Controllers.HomeController` 類別的資源，則應命名為 *Controllers.HomeController.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-681">A resource for the class `LocalizationWebsite.Web.Controllers.HomeController` would be named *Controllers.HomeController.fr.resx*.</span></span> <span data-ttu-id="3b0c0-682">如果目標類別的命名空間和組件名稱不相同，則需要使用完整類型名稱。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-682">If your targeted class's namespace isn't the same as the assembly name you will need the full type name.</span></span> <span data-ttu-id="3b0c0-683">比方說，範例專案中 `ExtraNamespace.Tools` 類型的資源會命名為 *ExtraNamespace.Tools.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-683">For example, in the sample project a resource for the type `ExtraNamespace.Tools` would be named *ExtraNamespace.Tools.fr.resx*.</span></span>

<span data-ttu-id="3b0c0-684">在範例專案中，`ConfigureServices` 方法會將 `ResourcesPath` 設為 "Resources"，因此首頁控制器的法文資源檔專案相對路徑即為 *Resources/Controllers.HomeController.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-684">In the sample project, the `ConfigureServices` method sets the `ResourcesPath` to "Resources", so the project relative path for the home controller's French resource file is *Resources/Controllers.HomeController.fr.resx*.</span></span> <span data-ttu-id="3b0c0-685">或者，您可以使用資料夾來收集資源檔。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-685">Alternatively, you can use folders to organize resource files.</span></span> <span data-ttu-id="3b0c0-686">若是首頁控制器，路徑就是 *Resources/Controllers/HomeController.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-686">For the home controller, the path would be *Resources/Controllers/HomeController.fr.resx*.</span></span> <span data-ttu-id="3b0c0-687">如果您不使用 `ResourcesPath` 選項，*.resx* 檔案即會放置在專案的基底目錄中。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-687">If you don't use the `ResourcesPath` option, the *.resx* file would go in the project base directory.</span></span> <span data-ttu-id="3b0c0-688">`HomeController` 的資源檔會命名為 *Controllers.HomeController.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-688">The resource file for `HomeController` would be named *Controllers.HomeController.fr.resx*.</span></span> <span data-ttu-id="3b0c0-689">您可依據自己的資源檔收集方式，來選擇要使用點或路徑的命名慣例。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-689">The choice of using the dot or path naming convention depends on how you want to organize your resource files.</span></span>

| <span data-ttu-id="3b0c0-690">資源名稱</span><span class="sxs-lookup"><span data-stu-id="3b0c0-690">Resource name</span></span> | <span data-ttu-id="3b0c0-691">點或路徑命名</span><span class="sxs-lookup"><span data-stu-id="3b0c0-691">Dot or path naming</span></span> |
| ------------   | ------------- |
| <span data-ttu-id="3b0c0-692">Resources/Controllers.HomeController.fr.resx</span><span class="sxs-lookup"><span data-stu-id="3b0c0-692">Resources/Controllers.HomeController.fr.resx</span></span> | <span data-ttu-id="3b0c0-693">點</span><span class="sxs-lookup"><span data-stu-id="3b0c0-693">Dot</span></span>  |
| <span data-ttu-id="3b0c0-694">Resources/Controllers/HomeController.fr.resx</span><span class="sxs-lookup"><span data-stu-id="3b0c0-694">Resources/Controllers/HomeController.fr.resx</span></span>  | <span data-ttu-id="3b0c0-695">路徑</span><span class="sxs-lookup"><span data-stu-id="3b0c0-695">Path</span></span> |
|    |     |

<span data-ttu-id="3b0c0-696">在 views 中使用的資源檔會 `@inject IViewLocalizer` Razor 遵循類似的模式。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-696">Resource files using `@inject IViewLocalizer` in Razor views follow a similar pattern.</span></span> <span data-ttu-id="3b0c0-697">您可以使用點命名或路徑命名方式，來命名檢視的資源檔。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-697">The resource file for a view can be named using either dot naming or path naming.</span></span> Razor<span data-ttu-id="3b0c0-698">查看資源檔模擬其相關聯之視圖檔案的路徑。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-698"> view resource files mimic the path of their associated view file.</span></span> <span data-ttu-id="3b0c0-699">假設我們將 `ResourcesPath` 設為 "Resources"，與 *Views/Home/About.cshtml* 檢視建立關聯的法文資源檔可為下列其一：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-699">Assuming we set the `ResourcesPath` to "Resources", the French resource file associated with the *Views/Home/About.cshtml* view could be either of the following:</span></span>

* <span data-ttu-id="3b0c0-700">Resources/Views/Home/About.fr.resx</span><span class="sxs-lookup"><span data-stu-id="3b0c0-700">Resources/Views/Home/About.fr.resx</span></span>

* <span data-ttu-id="3b0c0-701">Resources/Views.Home.About.fr.resx</span><span class="sxs-lookup"><span data-stu-id="3b0c0-701">Resources/Views.Home.About.fr.resx</span></span>

<span data-ttu-id="3b0c0-702">如果您不使用 `ResourcesPath` 選項，則檢視的 *.resx* 檔案會與檢視位於相同資料夾中。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-702">If you don't use the `ResourcesPath` option, the *.resx* file for a view would be located in the same folder as the view.</span></span>

### <a name="rootnamespaceattribute"></a><span data-ttu-id="3b0c0-703">RootNamespaceAttribute</span><span class="sxs-lookup"><span data-stu-id="3b0c0-703">RootNamespaceAttribute</span></span> 

<span data-ttu-id="3b0c0-704">[RootNamespace](/dotnet/api/microsoft.extensions.localization.rootnamespaceattribute?view=aspnetcore-2.1) 屬性會在組件的根命名空間與組件名稱不同時，提供組件的根命名空間。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-704">The [RootNamespace](/dotnet/api/microsoft.extensions.localization.rootnamespaceattribute?view=aspnetcore-2.1) attribute provides the root namespace of an assembly when the root namespace of an assembly is different than the assembly name.</span></span> 

> [!WARNING]
> <span data-ttu-id="3b0c0-705">當專案的名稱不是有效的 .NET 識別碼時，就可能發生這種情況。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-705">This can occur when a project's name is not a valid .NET identifier.</span></span> <span data-ttu-id="3b0c0-706">例如， `my-project-name.csproj` 會使用根命名空間 `my_project_name` ，以及 `my-project-name` 導致此錯誤的元件名稱。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-706">For instance `my-project-name.csproj` will use the root namespace `my_project_name` and the assembly name `my-project-name` leading to this error.</span></span> 

<span data-ttu-id="3b0c0-707">如果組件的根命名空間與組件名稱不同：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-707">If the root namespace of an assembly is different than the assembly name:</span></span>

* <span data-ttu-id="3b0c0-708">根據預設，當地語系化無法運作。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-708">Localization does not work by default.</span></span>
* <span data-ttu-id="3b0c0-709">在組件中搜尋資源的方式造成當地語系化失敗。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-709">Localization fails due to the way resources are searched for within the assembly.</span></span> <span data-ttu-id="3b0c0-710">`RootNamespace` 是建置時間值，無法用於執行處理序。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-710">`RootNamespace` is a build-time value which is not available to the executing process.</span></span> 

<span data-ttu-id="3b0c0-711">如果 `RootNamespace` 與 `AssemblyName` 不同，請將下列內容納入 *AssemblyInfo.cs* (參數值取代為實際值)：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-711">If the `RootNamespace` is different from the `AssemblyName`, include the following in *AssemblyInfo.cs* (with parameter values replaced with the actual values):</span></span>

```csharp
using System.Reflection;
using Microsoft.Extensions.Localization;

[assembly: ResourceLocation("Resource Folder Name")]
[assembly: RootNamespace("App Root Namespace")]
```

<span data-ttu-id="3b0c0-712">上述程式碼可成功解析 resx 檔案。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-712">The preceding code enables the successful resolution of resx files.</span></span>

## <a name="culture-fallback-behavior"></a><span data-ttu-id="3b0c0-713">文化特性後援行為</span><span class="sxs-lookup"><span data-stu-id="3b0c0-713">Culture fallback behavior</span></span>

<span data-ttu-id="3b0c0-714">搜尋資源時，當地語系化會使用「文化特性後援」。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-714">When searching for a resource, localization engages in "culture fallback".</span></span> <span data-ttu-id="3b0c0-715">從所要求的文化特性開始，如果找不到，就會還原成該文化特性的父文化特性。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-715">Starting from the requested culture, if not found, it reverts to the parent culture of that culture.</span></span> <span data-ttu-id="3b0c0-716">另外，[CultureInfo.Parent](/dotnet/api/system.globalization.cultureinfo.parent) 屬性代表父文化特性。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-716">As an aside, the [CultureInfo.Parent](/dotnet/api/system.globalization.cultureinfo.parent) property represents the parent culture.</span></span> <span data-ttu-id="3b0c0-717">這通常 (但並非一定) 表示從 ISO 中移除國家意符 (Signifier)。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-717">This usually (but not always) means removing the national signifier from the ISO.</span></span> <span data-ttu-id="3b0c0-718">例如，墨西哥的西班牙文方言是 "es-MX"。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-718">For example, the dialect of Spanish spoken in Mexico is "es-MX".</span></span> <span data-ttu-id="3b0c0-719">它具有父系 "es" &mdash; 西班牙文不是任何國家/地區的特定項目。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-719">It has the parent "es"&mdash;Spanish non-specific to any country.</span></span>

<span data-ttu-id="3b0c0-720">假設您的網站收到使用文化特性 "fr-CA" 之 "Welcome" 資源的要求。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-720">Imagine your site receives a request for a "Welcome" resource using culture "fr-CA".</span></span> <span data-ttu-id="3b0c0-721">當地語系化系統會依照順序尋找下列資源，並選取第一個相符項目：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-721">The localization system looks for the following resources, in order, and selects the first match:</span></span>

* <span data-ttu-id="3b0c0-722">*Welcome.fr-CA.resx*</span><span class="sxs-lookup"><span data-stu-id="3b0c0-722">*Welcome.fr-CA.resx*</span></span>
* <span data-ttu-id="3b0c0-723">*Welcome.fr.resx*</span><span class="sxs-lookup"><span data-stu-id="3b0c0-723">*Welcome.fr.resx*</span></span>
* <span data-ttu-id="3b0c0-724">*Welcome.resx* (如果 `NeutralResourcesLanguage` 是 "fr-CA")</span><span class="sxs-lookup"><span data-stu-id="3b0c0-724">*Welcome.resx* (if the `NeutralResourcesLanguage` is "fr-CA")</span></span>

<span data-ttu-id="3b0c0-725">舉例來說，如果您移除 ".fr" 文化特性指示項，並將文化特性設定為法文，則系統會讀取預設資源檔，並將字串當地語系化。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-725">As an example, if you remove the ".fr" culture designator and you have the culture set to French, the default resource file is read and strings are localized.</span></span> <span data-ttu-id="3b0c0-726">當沒有任何項目符合您要求的文化特性時，資源管理員即會指定預設資源或後援資源。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-726">The Resource manager designates a default or fallback resource for when nothing meets your requested culture.</span></span> <span data-ttu-id="3b0c0-727">如果您只想在要求的文化特性缺少資源時傳回索引鍵，就不能使用預設資源檔。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-727">If you want to just return the key when missing a resource for the requested culture you must not have a default resource file.</span></span>

### <a name="generate-resource-files-with-visual-studio"></a><span data-ttu-id="3b0c0-728">使用 Visual Studio 產生資源檔</span><span class="sxs-lookup"><span data-stu-id="3b0c0-728">Generate resource files with Visual Studio</span></span>

<span data-ttu-id="3b0c0-729">如果您在 Visual Studio 中建立資源檔，但檔案名稱中不含文化特性 (例如 *Welcome.resx*)，則 Visual Studio 會針對每個字串的屬性建立 C# 類別。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-729">If you create a resource file in Visual Studio without a culture in the file name (for example, *Welcome.resx*), Visual Studio will create a C# class with a property for each string.</span></span> <span data-ttu-id="3b0c0-730">但這通常不是您使用 ASP.NET Core 的初衷。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-730">That's usually not what you want with ASP.NET Core.</span></span> <span data-ttu-id="3b0c0-731">一般來說，您不會有預設的 *.resx* 資源檔 (不含文化特性名稱的 *.resx* 檔案)。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-731">You typically don't have a default *.resx* resource file (a *.resx* file without the culture name).</span></span> <span data-ttu-id="3b0c0-732">因此，建議您建立含有文化特性名稱的 *.resx* 檔案 (例如 *Welcome.fr.resx*)。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-732">We suggest you create the *.resx* file with a culture name (for example *Welcome.fr.resx*).</span></span> <span data-ttu-id="3b0c0-733">當您建立含有文化特性名稱的 *.resx* 檔案時，Visual Studio 就不會產生類別檔案。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-733">When you create a *.resx* file with a culture name, Visual Studio won't generate the class file.</span></span>

### <a name="add-other-cultures"></a><span data-ttu-id="3b0c0-734">新增其他文化特性</span><span class="sxs-lookup"><span data-stu-id="3b0c0-734">Add other cultures</span></span>

<span data-ttu-id="3b0c0-735">每種語言和文化特性的組合 (非預設語言) 都需要唯一的資源檔。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-735">Each language and culture combination (other than the default language) requires a unique resource file.</span></span> <span data-ttu-id="3b0c0-736">若要建立不同文化特性和地區設定的資源檔，您可以建立新的資源檔並將 ISO 語言代碼作為檔名的一部分 (例如 **en-us**、**fr-ca** 和 **en-gb**)。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-736">You create resource files for different cultures and locales by creating new resource files in which the ISO language codes are part of the file name (for example, **en-us**, **fr-ca**, and **en-gb**).</span></span> <span data-ttu-id="3b0c0-737">您應將 ISO 代碼置於檔案名稱和 *.resx* 副檔名之間，例如 *Welcome.es-MX.resx* (西班牙文/墨西哥)。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-737">These ISO codes are placed between the file name and the *.resx* file extension, as in *Welcome.es-MX.resx* (Spanish/Mexico).</span></span>

## <a name="implement-a-strategy-to-select-the-languageculture-for-each-request"></a><span data-ttu-id="3b0c0-738">實作可依據每項要求選取語言/文化特性的策略</span><span class="sxs-lookup"><span data-stu-id="3b0c0-738">Implement a strategy to select the language/culture for each request</span></span>

### <a name="configure-localization"></a><span data-ttu-id="3b0c0-739">設定當地語系化</span><span class="sxs-lookup"><span data-stu-id="3b0c0-739">Configure localization</span></span>

<span data-ttu-id="3b0c0-740">您可以在 `Startup.ConfigureServices` 方法中設定當地語系化：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-740">Localization is configured in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](localization/sample/Localization/Startup.cs?name=snippet1)]

* <span data-ttu-id="3b0c0-741">`AddLocalization` 可將當地語系化服務新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-741">`AddLocalization` Adds the localization services to the services container.</span></span> <span data-ttu-id="3b0c0-742">上方的程式碼也會將資源路徑設為 "Resources"。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-742">The code above also sets the resources path to "Resources".</span></span>

* <span data-ttu-id="3b0c0-743">`AddViewLocalization` 可支援當地語系化的檢視檔案。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-743">`AddViewLocalization` Adds support for localized view files.</span></span> <span data-ttu-id="3b0c0-744">在此範例中，檢視的當地語系化會以檢視檔案的後置詞為依據。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-744">In this sample view localization is based on the view file suffix.</span></span> <span data-ttu-id="3b0c0-745">例如 *Index.fr.cshtml* 檔案中的 "fr"。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-745">For example "fr" in the *Index.fr.cshtml* file.</span></span>

* <span data-ttu-id="3b0c0-746">`AddDataAnnotationsLocalization` 可支援透過 `IStringLocalizer` 抽象概念而來的當地語系化 `DataAnnotations` 驗證訊息。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-746">`AddDataAnnotationsLocalization` Adds support for localized `DataAnnotations` validation messages through `IStringLocalizer` abstractions.</span></span>

### <a name="localization-middleware"></a><span data-ttu-id="3b0c0-747">當地語系化中介軟體</span><span class="sxs-lookup"><span data-stu-id="3b0c0-747">Localization middleware</span></span>

<span data-ttu-id="3b0c0-748">您可以在當地語系化[中介軟體](xref:fundamentals/middleware/index)中，設定要求目前的文化特性。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-748">The current culture on a request is set in the localization [Middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="3b0c0-749">已在 `Startup.Configure` 方法中啟用當地語系化中介軟體。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-749">The localization middleware is enabled in the `Startup.Configure` method.</span></span> <span data-ttu-id="3b0c0-750">您必須在可能檢查要求文化特性的任何中介軟體之前，設定當地語系化中介軟體 (例如 `app.UseMvcWithDefaultRoute()`)。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-750">The localization middleware must be configured before any middleware which might check the request culture (for example, `app.UseMvcWithDefaultRoute()`).</span></span>

[!code-csharp[](localization/sample/Localization/Startup.cs?name=snippet2)]
[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

<span data-ttu-id="3b0c0-751">`UseRequestLocalization` 會初始化 `RequestLocalizationOptions` 物件。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-751">`UseRequestLocalization` initializes a `RequestLocalizationOptions` object.</span></span> <span data-ttu-id="3b0c0-752">在每次要求時，系統會列舉 `RequestLocalizationOptions` 中的 `RequestCultureProvider` 清單，並使用能成功判斷要求的文化特性的第一個提供者。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-752">On every request the list of `RequestCultureProvider` in the `RequestLocalizationOptions` is enumerated and the first provider that can successfully determine the request culture is used.</span></span> <span data-ttu-id="3b0c0-753">預設的提供者是來自 `RequestLocalizationOptions` 類別：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-753">The default providers come from the `RequestLocalizationOptions` class:</span></span>

1. `QueryStringRequestCultureProvider`
2. `CookieRequestCultureProvider`
3. `AcceptLanguageHeaderRequestCultureProvider`

<span data-ttu-id="3b0c0-754">預設清單會由針對性高到低來排列。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-754">The default list goes from most specific to least specific.</span></span> <span data-ttu-id="3b0c0-755">在本文稍後，我們會說明如何變更順序，甚至新增自訂的文化特性提供者。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-755">Later in the article we'll see how you can change the order and even add a custom culture provider.</span></span> <span data-ttu-id="3b0c0-756">如果沒有任何提供者可以判斷要求的文化特性，即會使用 `DefaultRequestCulture`。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-756">If none of the providers can determine the request culture, the `DefaultRequestCulture` is used.</span></span>

### <a name="querystringrequestcultureprovider"></a><span data-ttu-id="3b0c0-757">QueryStringRequestCultureProvider</span><span class="sxs-lookup"><span data-stu-id="3b0c0-757">QueryStringRequestCultureProvider</span></span>

<span data-ttu-id="3b0c0-758">有些應用程式會使用查詢字串來設定[文化特性和 UI 文化特性](https://msdn.microsoft.com/library/system.globalization.cultureinfo.aspx)。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-758">Some apps will use a query string to set the [culture and UI culture](https://msdn.microsoft.com/library/system.globalization.cultureinfo.aspx).</span></span> <span data-ttu-id="3b0c0-759">若是使用 Cookie 或 Accept-Language 標頭方法的應用程式，您可以將查詢字串新增至 URL 以偵錯和測試程式碼。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-759">For apps that use the cookie or Accept-Language header approach, adding a query string to the URL is useful for debugging and testing code.</span></span> <span data-ttu-id="3b0c0-760">系統預設會將 `QueryStringRequestCultureProvider` 登錄為 `RequestCultureProvider` 清單中的第一個當地語系化提供者。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-760">By default, the `QueryStringRequestCultureProvider` is registered as the first localization provider in the `RequestCultureProvider` list.</span></span> <span data-ttu-id="3b0c0-761">您應傳遞查詢字串參數 `culture` 和 `ui-culture`。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-761">You pass the query string parameters `culture` and `ui-culture`.</span></span> <span data-ttu-id="3b0c0-762">下列範例會設定西班牙文/墨西哥的特定文化特性 (語言和地區)：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-762">The following example sets the specific culture (language and region) to Spanish/Mexico:</span></span>

   `http://localhost:5000/?culture=es-MX&ui-culture=es-MX`

<span data-ttu-id="3b0c0-763">如果您只傳入兩者其一 (`culture` 或 `ui-culture`)，查詢字串提供者就會使用您傳入的項目來設定這兩個值。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-763">If you only pass in one of the two (`culture` or `ui-culture`), the query string provider will set both values using the one you passed in.</span></span> <span data-ttu-id="3b0c0-764">例如，若只設定文化特性，即會同時設定 `Culture` 和 `UICulture`：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-764">For example, setting just the culture will set both the `Culture` and the `UICulture`:</span></span>

   `http://localhost:5000/?culture=es-MX`

### <a name="cookierequestcultureprovider"></a><span data-ttu-id="3b0c0-765">CookieRequestCultureProvider</span><span class="sxs-lookup"><span data-stu-id="3b0c0-765">CookieRequestCultureProvider</span></span>

<span data-ttu-id="3b0c0-766">生產環境應用程式通常會提供一個機制，來設定 ASP.NET Core 文化特性 Cookie 的文化特性。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-766">Production apps will often provide a mechanism to set the culture with the ASP.NET Core culture cookie.</span></span> <span data-ttu-id="3b0c0-767">若要建立 Cookie，請使用 `MakeCookieValue` 方法。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-767">Use the `MakeCookieValue` method to create a cookie.</span></span>

<span data-ttu-id="3b0c0-768">會傳回 `CookieRequestCultureProvider` `DefaultCookieName` 用來追蹤使用者慣用文化特性資訊的預設 cookie 名稱。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-768">The `CookieRequestCultureProvider` `DefaultCookieName` returns the default cookie name used to track the user's preferred culture information.</span></span> <span data-ttu-id="3b0c0-769">預設 Cookie 名稱為 `.AspNetCore.Culture`。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-769">The default cookie name is `.AspNetCore.Culture`.</span></span>

<span data-ttu-id="3b0c0-770">Cookie 格式為 `c=%LANGCODE%|uic=%LANGCODE%`，其中 `c` 是 `Culture` 而 `uic` 是 `UICulture`，例如：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-770">The cookie format is `c=%LANGCODE%|uic=%LANGCODE%`, where `c` is `Culture` and `uic` is `UICulture`, for example:</span></span>

    c=en-UK|uic=en-US

<span data-ttu-id="3b0c0-771">如果您只指定文化特性資訊和 UI 文化特性其一，系統就會將您所指定的文化特性用於文化特性資訊和 UI 文化特性。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-771">If you only specify one of culture info and UI culture, the specified culture will be used for both culture info and UI culture.</span></span>

### <a name="the-accept-language-http-header"></a><span data-ttu-id="3b0c0-772">Accept-Language HTTP 標頭</span><span class="sxs-lookup"><span data-stu-id="3b0c0-772">The Accept-Language HTTP header</span></span>

<span data-ttu-id="3b0c0-773">您可在大多數的瀏覽器中設定 [Accept-Language 標頭](https://www.w3.org/International/questions/qa-accept-lang-locales)，其最初的設計目的是用來指定使用者的語言。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-773">The [Accept-Language header](https://www.w3.org/International/questions/qa-accept-lang-locales) is settable in most browsers and was originally intended to specify the user's language.</span></span> <span data-ttu-id="3b0c0-774">這項設定可指出瀏覽器已設好要傳送哪些項目，或已從基礎作業系統繼承哪些項目。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-774">This setting indicates what the browser has been set to send or has inherited from the underlying operating system.</span></span> <span data-ttu-id="3b0c0-775">透過瀏覽器要求的 Accept-Language HTTP 標頭來偵測使用者的慣用語言，並非萬無一失 (請參閱 [Setting language preferences in a browser](https://www.w3.org/International/questions/qa-lang-priorities.en.php) (在瀏覽器中設定語言喜好設定)。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-775">The Accept-Language HTTP header from a browser request isn't an infallible way to detect the user's preferred language (see [Setting language preferences in a browser](https://www.w3.org/International/questions/qa-lang-priorities.en.php)).</span></span> <span data-ttu-id="3b0c0-776">生產環境應用程式應該包含可讓使用者自訂文化特性的選擇方式。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-776">A production app should include a way for a user to customize their choice of culture.</span></span>

### <a name="set-the-accept-language-http-header-in-ie"></a><span data-ttu-id="3b0c0-777">在 IE 中設定 Accept-Language HTTP 標頭</span><span class="sxs-lookup"><span data-stu-id="3b0c0-777">Set the Accept-Language HTTP header in IE</span></span>

1. <span data-ttu-id="3b0c0-778">從齒輪圖示，點選 [網際網路選項]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-778">From the gear icon, tap **Internet Options**.</span></span>

2. <span data-ttu-id="3b0c0-779">點選 [語言]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-779">Tap **Languages**.</span></span>

    ![網際網路選項](localization/_static/lang.png)

3. <span data-ttu-id="3b0c0-781">點選 [設定語言喜好設定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-781">Tap **Set Language Preferences**.</span></span>

4. <span data-ttu-id="3b0c0-782">點選 [新增語言]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-782">Tap **Add a language**.</span></span>

5. <span data-ttu-id="3b0c0-783">新增語言。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-783">Add the language.</span></span>

6. <span data-ttu-id="3b0c0-784">點選語言，然後點選 [上移]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-784">Tap the language, then tap **Move Up**.</span></span>

### <a name="the-content-language-http-header"></a><span data-ttu-id="3b0c0-785">內容語言 HTTP 標頭</span><span class="sxs-lookup"><span data-stu-id="3b0c0-785">The Content-Language HTTP header</span></span>

<span data-ttu-id="3b0c0-786">[內容語言](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Language)實體標頭：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-786">The [Content-Language](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Language) entity header:</span></span>

 - <span data-ttu-id="3b0c0-787">用來描述適用于物件的語言。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-787">Is used to describe the language(s) intended for the audience.</span></span>
 - <span data-ttu-id="3b0c0-788">可讓使用者根據使用者的慣用語言來區分。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-788">Allows a user to differentiate according to the users' own preferred language.</span></span>

<span data-ttu-id="3b0c0-789">實體標頭會同時用於 HTTP 要求和回應。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-789">Entity headers are used in both HTTP requests and responses.</span></span>

<span data-ttu-id="3b0c0-790">您 `Content-Language` 可以藉由設定屬性來新增標頭 `ApplyCurrentCultureToResponseHeaders` 。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-790">The `Content-Language` header can be added by setting the property `ApplyCurrentCultureToResponseHeaders`.</span></span>

<span data-ttu-id="3b0c0-791">新增 `Content-Language` 標頭：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-791">Adding the `Content-Language` header:</span></span>

 - <span data-ttu-id="3b0c0-792">允許 RequestLocalizationMiddleware 使用來設定 `Content-Language` 標頭 `CurrentUICulture` 。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-792">Allows the RequestLocalizationMiddleware to set the `Content-Language` header with the `CurrentUICulture`.</span></span>
 - <span data-ttu-id="3b0c0-793">不需要明確地設定回應標頭 `Content-Language` 。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-793">Eliminates the need to set the response header `Content-Language` explicitly.</span></span>

```csharp
app.UseRequestLocalization(new RequestLocalizationOptions
{
    ApplyCurrentCultureToResponseHeaders = true
});
```

### <a name="use-a-custom-provider"></a><span data-ttu-id="3b0c0-794">使用自訂提供者</span><span class="sxs-lookup"><span data-stu-id="3b0c0-794">Use a custom provider</span></span>

<span data-ttu-id="3b0c0-795">假設您想要讓客戶將他們的語言和文化特性儲存在您的資料庫中。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-795">Suppose you want to let your customers store their language and culture in your databases.</span></span> <span data-ttu-id="3b0c0-796">您可以撰寫提供者，以供使用者查詢這些值。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-796">You could write a provider to look up these values for the user.</span></span> <span data-ttu-id="3b0c0-797">下列程式碼會示範如何新增自訂提供者：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-797">The following code shows how to add a custom provider:</span></span>

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

<span data-ttu-id="3b0c0-798">使用 `RequestLocalizationOptions` 新增或移除當地語系化提供者。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-798">Use `RequestLocalizationOptions` to add or remove localization providers.</span></span>

### <a name="set-the-culture-programmatically"></a><span data-ttu-id="3b0c0-799">以程式設計方式來設定文化特性</span><span class="sxs-lookup"><span data-stu-id="3b0c0-799">Set the culture programmatically</span></span>

<span data-ttu-id="3b0c0-800">[GitHub](https://github.com/aspnet/entropy) 上的這個範例 **Localization.StarterWeb** 專案包含可設定 `Culture` 的 UI。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-800">This sample **Localization.StarterWeb** project on [GitHub](https://github.com/aspnet/entropy) contains UI to set the `Culture`.</span></span> <span data-ttu-id="3b0c0-801">*Views/Shared/_SelectLanguagePartial.cshtml* 檔可讓您從支援的文化特性清單中選取文化特性：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-801">The *Views/Shared/_SelectLanguagePartial.cshtml* file allows you to select the culture from the list of supported cultures:</span></span>

[!code-cshtml[](localization/sample/Localization/Views/Shared/_SelectLanguagePartial.cshtml)]

<span data-ttu-id="3b0c0-802">系統會將 *Views/Shared/_SelectLanguagePartial.cshtml* 檔案新增至配置檔案的 `footer` 區段，以供所有檢視使用：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-802">The *Views/Shared/_SelectLanguagePartial.cshtml* file is added to the `footer` section of the layout file so it will be available to all views:</span></span>

[!code-cshtml[](localization/sample/Localization/Views/Shared/_Layout.cshtml?range=43-56&highlight=10)]

<span data-ttu-id="3b0c0-803">`SetLanguage` 方法會設定文化特性的 Cookie。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-803">The `SetLanguage` method sets the culture cookie.</span></span>

[!code-csharp[](localization/sample/Localization/Controllers/HomeController.cs?range=57-67)]

<span data-ttu-id="3b0c0-804">您無法將 *_SelectLanguagePartial.cshtml* 插入這個專案的範例程式碼。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-804">You can't plug in the *_SelectLanguagePartial.cshtml* to sample code for this project.</span></span> <span data-ttu-id="3b0c0-805">[GitHub](https://github.com/aspnet/entropy)上的**localization.starterweb**專案具有程式碼，可透過相依性 `RequestLocalizationOptions` 插入容器將傳遞至 Razor 部分。 [Dependency Injection](dependency-injection.md)</span><span class="sxs-lookup"><span data-stu-id="3b0c0-805">The **Localization.StarterWeb** project on [GitHub](https://github.com/aspnet/entropy) has code to flow the `RequestLocalizationOptions` to a Razor partial through the [Dependency Injection](dependency-injection.md) container.</span></span>

## <a name="model-binding-route-data-and-query-strings"></a><span data-ttu-id="3b0c0-806">模型系結路由資料和查詢字串</span><span class="sxs-lookup"><span data-stu-id="3b0c0-806">Model binding route data and query strings</span></span>

<span data-ttu-id="3b0c0-807">請參閱模型系結[路由資料和查詢字串的全球化行為](xref:mvc/models/model-binding#glob)。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-807">See [Globalization behavior of model binding route data and query strings](xref:mvc/models/model-binding#glob).</span></span>

## <a name="globalization-and-localization-terms"></a><span data-ttu-id="3b0c0-808">全球化和當地語系化詞彙</span><span class="sxs-lookup"><span data-stu-id="3b0c0-808">Globalization and localization terms</span></span>

<span data-ttu-id="3b0c0-809">在進行應用程式的當地語系化程序時，您也需要具備現代軟體開發中常用的相關字元集基本知識，並了解與它們建立關聯的問題。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-809">The process of localizing your app also requires a basic understanding of relevant character sets commonly used in modern software development and an understanding of the issues associated with them.</span></span> <span data-ttu-id="3b0c0-810">雖然所有電腦都會將文字儲存為數字 (程式碼)，但不同系統會使用不同數字來儲存相同的文字。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-810">Although all computers store text as numbers (codes), different systems store the same text using different numbers.</span></span> <span data-ttu-id="3b0c0-811">當地語系化是指針對特定文化特性/地區設定，轉譯應用程式使用者介面 (UI) 的程序。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-811">The localization process refers to translating the app user interface (UI) for a specific culture/locale.</span></span>

<span data-ttu-id="3b0c0-812">[可當地語系化](/dotnet/standard/globalization-localization/localizability-review)是確認全球化應用程式已準備好進行當地語系化的中繼程序。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-812">[Localizability](/dotnet/standard/globalization-localization/localizability-review) is an intermediate process for verifying that a globalized app is ready for localization.</span></span>

<span data-ttu-id="3b0c0-813">文化特性名稱的 [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) 格式是 `<languagecode2>-<country/regioncode2>`，其中 `<languagecode2>` 是語言代碼，而 `<country/regioncode2>` 是子文化特性代碼。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-813">The [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) format for the culture name is `<languagecode2>-<country/regioncode2>`, where `<languagecode2>` is the language code and `<country/regioncode2>` is the subculture code.</span></span> <span data-ttu-id="3b0c0-814">例如，`es-CL` 是指西班牙文 (智利)，`en-US` 是指英文 (美國)，而 `en-AU` 是指英文 (澳大利亞)。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-814">For example, `es-CL` for Spanish (Chile), `en-US` for English (United States), and `en-AU` for English (Australia).</span></span> <span data-ttu-id="3b0c0-815">[RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) 是 ISO 639 (兩個小寫字母，為與某個語言建立關聯的文化特性代碼) 及 ISO 3166 (兩個大寫字母，為與某個國家或地區建立關聯的子文化特性代碼) 的組合。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-815">[RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) is a combination of an ISO 639 two-letter lowercase culture code associated with a language and an ISO 3166 two-letter uppercase subculture code associated with a country or region.</span></span> <span data-ttu-id="3b0c0-816">請參閱 [Language Culture Name](https://msdn.microsoft.com/library/ee825488(v=cs.20).aspx) (語言的文化特性名稱)。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-816">See [Language Culture Name](https://msdn.microsoft.com/library/ee825488(v=cs.20).aspx).</span></span>

<span data-ttu-id="3b0c0-817">國際化通常縮寫為 "I18N"。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-817">Internationalization is often abbreviated to "I18N".</span></span> <span data-ttu-id="3b0c0-818">這個縮寫是採用該詞彙的第一個和最後一個字母，以及這兩個字母間的字母數組成，因此 18 代表第一個字母 "I" 及最後 "N" 中間的字母數。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-818">The abbreviation takes the first and last letters and the number of letters between them, so 18 stands for the number of letters between the first "I" and the last "N".</span></span> <span data-ttu-id="3b0c0-819">同樣的原則也適用於全球化 (G11N) 與當地語系化 (L10N) 的縮寫。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-819">The same applies to Globalization (G11N), and Localization (L10N).</span></span>

<span data-ttu-id="3b0c0-820">詞彙：</span><span class="sxs-lookup"><span data-stu-id="3b0c0-820">Terms:</span></span>

* <span data-ttu-id="3b0c0-821">全球化 (G11N)：讓應用程式支援不同語言和區域的程序。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-821">Globalization (G11N): The process of making an app support different languages and regions.</span></span>
* <span data-ttu-id="3b0c0-822">當地語系化 (L10N)：針對特定語言和區域，自訂應用程式的程序。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-822">Localization (L10N): The process of customizing an app for a given language and region.</span></span>
* <span data-ttu-id="3b0c0-823">國際化 (I18N)：包含全球化和當地語系化這兩部分的描述。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-823">Internationalization (I18N): Describes both globalization and localization.</span></span>
* <span data-ttu-id="3b0c0-824">文化特性：它是一種語言和/或地區。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-824">Culture: It's a language and, optionally, a region.</span></span>
* <span data-ttu-id="3b0c0-825">中性文化特性：具有指定的語言但不限地區的文化特性 </span><span class="sxs-lookup"><span data-stu-id="3b0c0-825">Neutral culture: A culture that has a specified language, but not a region.</span></span> <span data-ttu-id="3b0c0-826">(例如 "en"、"es")。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-826">(for example "en", "es")</span></span>
* <span data-ttu-id="3b0c0-827">特定文化特性：具有指定的語言和地區的文化特性 </span><span class="sxs-lookup"><span data-stu-id="3b0c0-827">Specific culture: A culture that has a specified language and region.</span></span> <span data-ttu-id="3b0c0-828">(例如 "en-US"、"en-GB"、"es-CL")。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-828">(for example "en-US", "en-GB", "es-CL")</span></span>
* <span data-ttu-id="3b0c0-829">父文化特性：包含特定文化特性的中性文化特性 </span><span class="sxs-lookup"><span data-stu-id="3b0c0-829">Parent culture: The neutral culture that contains a specific culture.</span></span> <span data-ttu-id="3b0c0-830">(例如，"en" 是 "en-US" 和 "en-GB" 的父文化特性)。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-830">(for example, "en" is the parent culture of "en-US" and "en-GB")</span></span>
* <span data-ttu-id="3b0c0-831">地區設定：地區設定與文化特性相同。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-831">Locale: A locale is the same as a culture.</span></span>

[!INCLUDE[](~/includes/localization/currency.md)]

[!INCLUDE[](~/includes/localization/unsupported-culture-log-level.md)]

## <a name="additional-resources"></a><span data-ttu-id="3b0c0-832">其他資源</span><span class="sxs-lookup"><span data-stu-id="3b0c0-832">Additional resources</span></span>

* <xref:fundamentals/troubleshoot-aspnet-core-localization>
* <span data-ttu-id="3b0c0-833">本文使用的 [Localization.StarterWeb 專案](https://github.com/aspnet/Entropy/tree/master/samples/Localization.StarterWeb)。</span><span class="sxs-lookup"><span data-stu-id="3b0c0-833">[Localization.StarterWeb project](https://github.com/aspnet/Entropy/tree/master/samples/Localization.StarterWeb) used in the article.</span></span>
* [<span data-ttu-id="3b0c0-834">全球化與當地語系化 .NET 應用程式</span><span class="sxs-lookup"><span data-stu-id="3b0c0-834">Globalizing and localizing .NET applications</span></span>](/dotnet/standard/globalization-localization/index)
* [<span data-ttu-id="3b0c0-835">.Resx 檔案中的資源</span><span class="sxs-lookup"><span data-stu-id="3b0c0-835">Resources in .resx Files</span></span>](/dotnet/framework/resources/working-with-resx-files-programmatically)
* [<span data-ttu-id="3b0c0-836">Microsoft 多語應用程式工具組</span><span class="sxs-lookup"><span data-stu-id="3b0c0-836">Microsoft Multilingual App Toolkit</span></span>](https://marketplace.visualstudio.com/items?itemName=MultilingualAppToolkit.MultilingualAppToolkit-18308)
* [<span data-ttu-id="3b0c0-837">當地語系化和泛型</span><span class="sxs-lookup"><span data-stu-id="3b0c0-837">Localization & Generics</span></span>](http://hishambinateya.com/localization-and-generics)

::: moniker-end
