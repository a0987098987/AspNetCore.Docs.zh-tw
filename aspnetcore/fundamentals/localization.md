---
<span data-ttu-id="f6f5f-101">標題： ASP.NET Core author 中的全球化和當地語系化： rick-anderson 描述：瞭解 ASP.NET Core 如何提供服務和中介軟體，將內容當地語系化成不同的語言和文化特性。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-101">title: Globalization and localization in ASP.NET Core author: rick-anderson description: Learn how ASP.NET Core provides services and middleware for localizing content into different languages and cultures.</span></span>
<span data-ttu-id="f6f5f-102">riande ms. date：11/30/2019 否-loc：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-102">ms.author: riande ms.date: 11/30/2019 no-loc:</span></span>
- <span data-ttu-id="f6f5f-103">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f6f5f-103">'Blazor'</span></span>
- <span data-ttu-id="f6f5f-104">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f6f5f-104">'Identity'</span></span>
- <span data-ttu-id="f6f5f-105">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f6f5f-105">'Let's Encrypt'</span></span>
- <span data-ttu-id="f6f5f-106">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f6f5f-106">'Razor'</span></span>
- <span data-ttu-id="f6f5f-107">' SignalR ' uid：基本概念/當地語系化</span><span class="sxs-lookup"><span data-stu-id="f6f5f-107">'SignalR' uid: fundamentals/localization</span></span>

---
# <a name="globalization-and-localization-in-aspnet-core"></a><span data-ttu-id="f6f5f-108">ASP.NET Core 全球化和當地語系化</span><span class="sxs-lookup"><span data-stu-id="f6f5f-108">Globalization and localization in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0 < aspnetcore-5.0"

<span data-ttu-id="f6f5f-109">由 [Rick Anderson](https://twitter.com/RickAndMSFT)、[Damien Bowden](https://twitter.com/damien_bod)、[Bart Calixto](https://twitter.com/bartmax)、[Nadeem Afana](https://afana.me/) 和 [Hisham Bin Ateya](https://twitter.com/hishambinateya) 提供</span><span class="sxs-lookup"><span data-stu-id="f6f5f-109">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Damien Bowden](https://twitter.com/damien_bod), [Bart Calixto](https://twitter.com/bartmax), [Nadeem Afana](https://afana.me/), and [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span></span>

<span data-ttu-id="f6f5f-110">多語系網站可讓網站觸及更多物件。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-110">A multilingual website allows the site to reach a wider audience.</span></span> <span data-ttu-id="f6f5f-111">ASP.NET Core 提供服務與中介軟體，可將網站當地語系化成不同的語言與文化特性。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-111">ASP.NET Core provides services and middleware for localizing into different languages and cultures.</span></span>

<span data-ttu-id="f6f5f-112">國際化包含[全球化](/dotnet/api/system.globalization)和[當地語系化](/dotnet/standard/globalization-localization/localization)這兩部分。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-112">Internationalization involves [Globalization](/dotnet/api/system.globalization) and [Localization](/dotnet/standard/globalization-localization/localization).</span></span> <span data-ttu-id="f6f5f-113">全球化是指設計出可支援不同文化特性之應用程式的程序。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-113">Globalization is the process of designing apps that support different cultures.</span></span> <span data-ttu-id="f6f5f-114">透過全球化，您可新增支援與特定地區相關之已定義語言指令碼的輸入、顯示及輸出作業。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-114">Globalization adds support for input, display, and output of a defined set of language scripts that relate to specific geographic areas.</span></span>

<span data-ttu-id="f6f5f-115">當地語系化是指針對全球化應用程式進行調整的程序，且您已順應特定文化特性/地區設定對這些全球化應用程式進行可當地語系化處理。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-115">Localization is the process of adapting a globalized app, which you have already processed for localizability, to a particular culture/locale.</span></span> <span data-ttu-id="f6f5f-116">如需詳細資訊，請參閱本文件結尾處的**全球化和當地語系化詞彙**。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-116">For more information see **Globalization and localization terms** near the end of this document.</span></span>

<span data-ttu-id="f6f5f-117">應用程式當地語系化包含下列作業：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-117">App localization involves the following:</span></span>

1. <span data-ttu-id="f6f5f-118">讓應用程式的內容可當地語系化</span><span class="sxs-lookup"><span data-stu-id="f6f5f-118">Make the app's content localizable</span></span>
1. <span data-ttu-id="f6f5f-119">針對您支援的語言和文化特性提供當地語系化資源</span><span class="sxs-lookup"><span data-stu-id="f6f5f-119">Provide localized resources for the languages and cultures you support</span></span>
1. <span data-ttu-id="f6f5f-120">實作可依據每項要求選取語言/文化特性的策略</span><span class="sxs-lookup"><span data-stu-id="f6f5f-120">Implement a strategy to select the language/culture for each request</span></span>

<span data-ttu-id="f6f5f-121">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/localization/sample/3.x/Localization)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="f6f5f-121">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/localization/sample/3.x/Localization) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="make-the-apps-content-localizable"></a><span data-ttu-id="f6f5f-122">讓應用程式的內容可當地語系化</span><span class="sxs-lookup"><span data-stu-id="f6f5f-122">Make the app's content localizable</span></span>

<span data-ttu-id="f6f5f-123"><xref:Microsoft.Extensions.Localization.IStringLocalizer>和 <xref:Microsoft.Extensions.Localization.IStringLocalizer%601> 已架構，可在開發當地語系化應用程式時改善生產力。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-123"><xref:Microsoft.Extensions.Localization.IStringLocalizer> and <xref:Microsoft.Extensions.Localization.IStringLocalizer%601> were architected to improve productivity when developing localized apps.</span></span> <span data-ttu-id="f6f5f-124">`IStringLocalizer`會使用 <xref:System.Resources.ResourceManager> 和， <xref:System.Resources.ResourceReader> 在執行時間提供特定文化特性的資源。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-124">`IStringLocalizer` uses the <xref:System.Resources.ResourceManager> and <xref:System.Resources.ResourceReader> to provide culture-specific resources at run time.</span></span> <span data-ttu-id="f6f5f-125">介面具有索引子和， `IEnumerable` 用於傳回當地語系化的字串。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-125">The interface has an indexer and an `IEnumerable` for returning localized strings.</span></span> <span data-ttu-id="f6f5f-126">`IStringLocalizer`不需要將預設語言字串儲存在資源檔中。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-126">`IStringLocalizer` doesn't require storing the default language strings in a resource file.</span></span> <span data-ttu-id="f6f5f-127">您不必在開發初期建立資源檔，即可開發以當地語系化為目標的應用程式。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-127">You can develop an app targeted for localization and not need to create resource files early in development.</span></span> <span data-ttu-id="f6f5f-128">下列程式碼會示範如何包裝 "About Title" 字串以進行當地語系化。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-128">The code below shows how to wrap the string "About Title" for localization.</span></span>

[!code-csharp[](localization/sample/3.x/Localization/Controllers/AboutController.cs)]

<span data-ttu-id="f6f5f-129">在上述程式碼中， `IStringLocalizer<T>` 執行是來自相依性[插入](dependency-injection.md)。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-129">In the preceding code, the `IStringLocalizer<T>` implementation comes from [Dependency Injection](dependency-injection.md).</span></span> <span data-ttu-id="f6f5f-130">如果找不到 "About Title" 的當地語系化值，即會傳回索引子的索引鍵，也就是 "About Title" 字串。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-130">If the localized value of "About Title" isn't found, then the indexer key is returned, that is, the string "About Title".</span></span> <span data-ttu-id="f6f5f-131">您可以保留應用程式中的預設語言常值字串，並將其包裝在當地語系化工具中，以便專注於開發應用程式。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-131">You can leave the default language literal strings in the app and wrap them in the localizer, so that you can focus on developing the app.</span></span> <span data-ttu-id="f6f5f-132">您不用先建立預設資源檔，即可使用預設語言來開發應用程式，並針對當地語系化步驟進行應用程式的準備。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-132">You develop your app with your default language and prepare it for the localization step without first creating a default resource file.</span></span> <span data-ttu-id="f6f5f-133">或者，您可以使用傳統方法，並提供索引鍵以擷取預設語言字串。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-133">Alternatively, you can use the traditional approach and provide a key to retrieve the default language string.</span></span> <span data-ttu-id="f6f5f-134">對許多開發人員來說，新的工作流程 (單純包裝字串常值而不使用預設語言的 *.resx* 檔案) 可以降低當地語系化應用程式的額外負荷。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-134">For many developers the new workflow of not having a default language *.resx* file and simply wrapping the string literals can reduce the overhead of localizing an app.</span></span> <span data-ttu-id="f6f5f-135">其他開發人員則偏好傳統的工作流程，因為這種方法更便於使用較長的字串常值，且更易於更新當地語系化的字串。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-135">Other developers will prefer the traditional work flow as it can make it easier to work with longer string literals and make it easier to update localized strings.</span></span>

<span data-ttu-id="f6f5f-136">若是包含 HTML 的資源，請使用 `IHtmlLocalizer<T>` 實作。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-136">Use the `IHtmlLocalizer<T>` implementation for resources that contain HTML.</span></span> <span data-ttu-id="f6f5f-137">`IHtmlLocalizer` 會對資源字串中經過格式化的引數進行 HTML 編碼，但不會對資源字串本身進行 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-137">`IHtmlLocalizer` HTML encodes arguments that are formatted in the resource string, but doesn't HTML encode the resource string itself.</span></span> <span data-ttu-id="f6f5f-138">在下列醒目提示的範例中，只有 `name` 參數的值經過 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-138">In the sample highlighted below, only the value of `name` parameter is HTML encoded.</span></span>

[!code-csharp[](~/fundamentals/localization/sample/3.x/Localization/Controllers/BookController.cs?highlight=3,5,20&start=1&end=24)]

<span data-ttu-id="f6f5f-139">**注意：** 一般來說，您只想將文字當地語系化，而不是 HTML。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-139">**Note:** You generally want to only localize text and not HTML.</span></span>

<span data-ttu-id="f6f5f-140">您可以在最底層的[相依性插入](dependency-injection.md)中，將 `IStringLocalizerFactory` 移出：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-140">At the lowest level, you can get `IStringLocalizerFactory` out of [Dependency Injection](dependency-injection.md):</span></span>

[!code-csharp[](localization/sample/3.x/Localization/Controllers/TestController.cs?start=9&end=26&highlight=7-13)]

<span data-ttu-id="f6f5f-141">上述程式碼會示範這兩個 Factory Create 方法。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-141">The code above demonstrates each of the two factory create methods.</span></span>

<span data-ttu-id="f6f5f-142">您可以依據控制器或區域來分割當地語系化的字串，也可以只用一個容器。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-142">You can partition your localized strings by controller, area, or have just one container.</span></span> <span data-ttu-id="f6f5f-143">在範例應用程式中，會針對共用資源使用名為 `SharedResource` 的虛擬類別。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-143">In the sample app, a dummy class named `SharedResource` is used for shared resources.</span></span>

[!code-csharp[](localization/sample/3.x/Localization/Resources/SharedResource.cs)]

<span data-ttu-id="f6f5f-144">有些開發人員會使用 `Startup` 類別來包含全域或共用字串。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-144">Some developers use the `Startup` class to contain global or shared strings.</span></span> <span data-ttu-id="f6f5f-145">下列範例會使用 `InfoController` 和 `SharedResource` 當地語系化工具：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-145">In the sample below, the `InfoController` and the `SharedResource` localizers are used:</span></span>

[!code-csharp[](localization/sample/3.x/Localization/Controllers/InfoController.cs?range=9-26)]

## <a name="view-localization"></a><span data-ttu-id="f6f5f-146">檢視當地語系化</span><span class="sxs-lookup"><span data-stu-id="f6f5f-146">View localization</span></span>

<span data-ttu-id="f6f5f-147">`IViewLocalizer` 服務可提供[檢視](xref:mvc/views/overview)的當地語系化字串。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-147">The `IViewLocalizer` service provides localized strings for a [view](xref:mvc/views/overview).</span></span> <span data-ttu-id="f6f5f-148">`ViewLocalizer` 類別會實作這個介面，並透過檢視檔案路徑來找出資源的位置。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-148">The `ViewLocalizer` class implements this interface and finds the resource location from the view file path.</span></span> <span data-ttu-id="f6f5f-149">下列程式碼示範如何使用 `IViewLocalizer` 的預設實作：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-149">The following code shows how to use the default implementation of `IViewLocalizer`:</span></span>

[!code-cshtml[](localization/sample/3.x/Localization/Views/Home/About.cshtml)]

<span data-ttu-id="f6f5f-150">`IViewLocalizer` 的預設實作會依據檢視的檔案名稱來找出資源檔。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-150">The default implementation of `IViewLocalizer` finds the resource file based on the view's file name.</span></span> <span data-ttu-id="f6f5f-151">其中並沒有任何選項可以使用全域共用的資源檔。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-151">There's no option to use a global shared resource file.</span></span> <span data-ttu-id="f6f5f-152">`ViewLocalizer`使用來執行當地語系化工具 `IHtmlLocalizer` ，因此 Razor 不會對當地語系化字串進行 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-152">`ViewLocalizer` implements the localizer using `IHtmlLocalizer`, so Razor doesn't HTML encode the localized string.</span></span> <span data-ttu-id="f6f5f-153">您可以參數化資源字串，`IViewLocalizer` 即會對參數 (而不是資源字串) 進行 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-153">You can parameterize resource strings and `IViewLocalizer` will HTML encode the parameters, but not the resource string.</span></span> <span data-ttu-id="f6f5f-154">請考慮下列 Razor 標記：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-154">Consider the following Razor markup:</span></span>

```cshtml
@Localizer["<i>Hello</i> <b>{0}!</b>", UserManager.GetUserName(User)]
```

<span data-ttu-id="f6f5f-155">法文資源檔可能包含下列內容：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-155">A French resource file could contain the following:</span></span>

| <span data-ttu-id="f6f5f-156">機碼</span><span class="sxs-lookup"><span data-stu-id="f6f5f-156">Key</span></span> | <span data-ttu-id="f6f5f-157">值</span><span class="sxs-lookup"><span data-stu-id="f6f5f-157">Value</span></span> |
| ----- | ------ |
| `<i>Hello</i> <b>{0}!</b>` | `<i>Bonjour</i> <b>{0} !</b>` |

<span data-ttu-id="f6f5f-158">轉譯的檢視內容可能包含來自資源檔的 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-158">The rendered view would contain the HTML markup from the resource file.</span></span>

<span data-ttu-id="f6f5f-159">**注意：** 一般來說，您只想將文字當地語系化，而不是 HTML。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-159">**Note:** You generally want to only localize text and not HTML.</span></span>

<span data-ttu-id="f6f5f-160">若要在檢視中使用共用的資源檔，請插入 `IHtmlLocalizer<T>`：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-160">To use a shared resource file in a view, inject `IHtmlLocalizer<T>`:</span></span>

[!code-cshtml[](~/fundamentals/localization/sample/3.x/Localization/Views/Test/About.cshtml?highlight=5,12)]

## <a name="dataannotations-localization"></a><span data-ttu-id="f6f5f-161">DataAnnotations 當地語系化</span><span class="sxs-lookup"><span data-stu-id="f6f5f-161">DataAnnotations localization</span></span>

<span data-ttu-id="f6f5f-162">DataAnnotations 錯誤訊息會使用 `IStringLocalizer<T>` 來當地語系化。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-162">DataAnnotations error messages are localized with `IStringLocalizer<T>`.</span></span> <span data-ttu-id="f6f5f-163">使用 `ResourcesPath = "Resources"` 選項時，`RegisterViewModel` 中的錯誤訊息會儲存在下列路徑之一：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-163">Using the option `ResourcesPath = "Resources"`, the error messages in `RegisterViewModel` can be stored in either of the following paths:</span></span>

* <span data-ttu-id="f6f5f-164">*Resources/Viewmodel. RegisterViewModel. fr .resx*</span><span class="sxs-lookup"><span data-stu-id="f6f5f-164">*Resources/ViewModels.Account.RegisterViewModel.fr.resx*</span></span>
* <span data-ttu-id="f6f5f-165">*Resources/ViewModels/Account/RegisterViewModel.fr.resx*</span><span class="sxs-lookup"><span data-stu-id="f6f5f-165">*Resources/ViewModels/Account/RegisterViewModel.fr.resx*</span></span>

[!code-csharp[](localization/sample/3.x/Localization/ViewModels/Account/RegisterViewModel.cs?start=9&end=26)]

<span data-ttu-id="f6f5f-166">在 ASP.NET Core MVC 1.1.0 和更高版本中，系統會將非驗證屬性當地語系化。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-166">In ASP.NET Core MVC 1.1.0 and higher, non-validation attributes are localized.</span></span> <span data-ttu-id="f6f5f-167">ASP.NET Core MVC 1.0 **不會**查閱非驗證屬性的當地語系化字串。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-167">ASP.NET Core MVC 1.0 does **not** look up localized strings for non-validation attributes.</span></span>

<a name="one-resource-string-multiple-classes"></a>

### <a name="using-one-resource-string-for-multiple-classes"></a><span data-ttu-id="f6f5f-168">針對多個類別使用同一個資源字串</span><span class="sxs-lookup"><span data-stu-id="f6f5f-168">Using one resource string for multiple classes</span></span>

<span data-ttu-id="f6f5f-169">下列程式碼會示範如何針對含有多個類別的驗證屬性使用同一個資源字串：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-169">The following code shows how to use one resource string for validation attributes with multiple classes:</span></span>

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

<span data-ttu-id="f6f5f-170">在先前的程式碼中，`SharedResource` 是與儲存驗證訊息的 resx 對應的類別。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-170">In the preceding code, `SharedResource` is the class corresponding to the resx where your validation messages are stored.</span></span> <span data-ttu-id="f6f5f-171">使用這個方法時，DataAnnotations 只會使用 `SharedResource`，而不是每個類別的資源。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-171">With this approach, DataAnnotations will only use `SharedResource`, rather than the resource for each class.</span></span>

## <a name="provide-localized-resources-for-the-languages-and-cultures-you-support"></a><span data-ttu-id="f6f5f-172">針對您支援的語言和文化特性提供當地語系化資源</span><span class="sxs-lookup"><span data-stu-id="f6f5f-172">Provide localized resources for the languages and cultures you support</span></span>

### <a name="supportedcultures-and-supporteduicultures"></a><span data-ttu-id="f6f5f-173">SupportedCultures 和 SupportedUICultures</span><span class="sxs-lookup"><span data-stu-id="f6f5f-173">SupportedCultures and SupportedUICultures</span></span>

<span data-ttu-id="f6f5f-174">ASP.NET Core 可讓您指定 `SupportedCultures` 和 `SupportedUICultures` 這兩個文化特性值。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-174">ASP.NET Core allows you to specify two culture values, `SupportedCultures` and `SupportedUICultures`.</span></span> <span data-ttu-id="f6f5f-175">`SupportedCultures` 的 [CultureInfo](/dotnet/api/system.globalization.cultureinfo) 物件可決定文化特性相依函式的結果，例如日期、時間、數字及貨幣格式。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-175">The [CultureInfo](/dotnet/api/system.globalization.cultureinfo) object for `SupportedCultures` determines the results of culture-dependent functions, such as date, time, number, and currency formatting.</span></span> <span data-ttu-id="f6f5f-176">`SupportedCultures` 也可決定文字排列順序、大小寫慣例和字串比較。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-176">`SupportedCultures` also determines the sorting order of text, casing conventions, and string comparisons.</span></span> <span data-ttu-id="f6f5f-177">如需伺服器如何取得文化特性的詳細資訊，請參閱 [CultureInfo.CurrentCulture](/dotnet/api/system.stringcomparer.currentculture#System_StringComparer_CurrentCulture)。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-177">See [CultureInfo.CurrentCulture](/dotnet/api/system.stringcomparer.currentculture#System_StringComparer_CurrentCulture) for more info on how the server gets the Culture.</span></span> <span data-ttu-id="f6f5f-178">`SupportedUICultures`會決定[ResourceManager](/dotnet/api/system.resources.resourcemanager)會查閱哪些翻譯的字串（來自 *.resx*檔案）。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-178">The `SupportedUICultures` determines which translated strings (from *.resx* files) are looked up by the [ResourceManager](/dotnet/api/system.resources.resourcemanager).</span></span> <span data-ttu-id="f6f5f-179">`ResourceManager` 僅會查閱 `CurrentUICulture` 所決定之文化特性特有的字串。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-179">The `ResourceManager` simply looks up culture-specific strings that's determined by `CurrentUICulture`.</span></span> <span data-ttu-id="f6f5f-180">.NET 中的每個執行緒都有 `CurrentCulture` 和 `CurrentUICulture` 物件。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-180">Every thread in .NET has `CurrentCulture` and `CurrentUICulture` objects.</span></span> <span data-ttu-id="f6f5f-181">ASP.NET Core 會在轉譯文化特性相依函式時檢查這些值。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-181">ASP.NET Core inspects these values when rendering culture-dependent functions.</span></span> <span data-ttu-id="f6f5f-182">比方說，如果目前執行緒的文化特性設定為 "en-US" (英文 - 美國)，`DateTime.Now.ToLongDateString()` 會顯示 "Thursday, February 18, 2016"，但如果 `CurrentCulture` 設定為 "es-ES" (西班牙文 - 西班牙)，則輸出會是 "jueves, 18 de febrero de 2016"。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-182">For example, if the current thread's culture is set to "en-US" (English, United States), `DateTime.Now.ToLongDateString()` displays "Thursday, February 18, 2016", but if `CurrentCulture` is set to "es-ES" (Spanish, Spain) the output will be "jueves, 18 de febrero de 2016".</span></span>

## <a name="resource-files"></a><span data-ttu-id="f6f5f-183">資源檔</span><span class="sxs-lookup"><span data-stu-id="f6f5f-183">Resource files</span></span>

<span data-ttu-id="f6f5f-184">資源檔是一種實用的機制，可讓您將可當地語系化的字串與代碼區隔開來。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-184">A resource file is a useful mechanism for separating localizable strings from code.</span></span> <span data-ttu-id="f6f5f-185">非預設語言的翻譯字串會在 *.resx*資源檔中隔離。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-185">Translated strings for the non-default language are isolated in *.resx* resource files.</span></span> <span data-ttu-id="f6f5f-186">例如，您可以建立名為 *Welcome.es.resx* 的西班牙文資源檔，以包含翻譯的字串。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-186">For example, you might want to create Spanish resource file named *Welcome.es.resx* containing translated strings.</span></span> <span data-ttu-id="f6f5f-187">"es" 是西班牙文的語言代碼。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-187">"es" is the language code for Spanish.</span></span> <span data-ttu-id="f6f5f-188">若要在 Visual Studio 中建立這個資源檔：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-188">To create this resource file in Visual Studio:</span></span>

1. <span data-ttu-id="f6f5f-189">在方案總管\*\*\*\* 中，以滑鼠右鍵按一下要放置資源檔的資料夾 > [新增]**[新增項目]** > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-189">In **Solution Explorer**, right click on the folder which will contain the resource file > **Add** > **New Item**.</span></span>

    ![巢狀特色選單：方案總管會開啟 [資源] 的特色選單，](localization/_static/newi.png)

2. <span data-ttu-id="f6f5f-192">在 [Search installed templates] (搜尋已安裝的範本)\*\*\*\* 方塊中，輸入「資源」，並命名檔案。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-192">In the **Search installed templates** box, enter "resource" and name the file.</span></span>

    ![[新增項目] 對話方塊](localization/_static/res.png)

3. <span data-ttu-id="f6f5f-194">在 [名稱]\*\*\*\* 資料行中輸入索引鍵值 (原生字串)，並在 [值]\*\*\*\* 資料行中輸入已翻譯的字串。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-194">Enter the key value (native string) in the **Name** column and the translated string in the **Value** column.</span></span>

    ![Welcome.es.resx 檔案 (西班牙文的「歡迎使用」資源檔)，其中 [名稱] 資料行的文字為 Hello，而 [值] 資料行的文字為 Hola (Hello 的西班牙文)](localization/_static/hola.png)

    <span data-ttu-id="f6f5f-196">Visual Studio 會顯示 *Welcome.es.resx* 檔案。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-196">Visual Studio shows the *Welcome.es.resx* file.</span></span>

    ![方案總管，其中顯示「歡迎使用」的西班牙文 (es) 資源檔](localization/_static/se.png)

## <a name="resource-file-naming"></a><span data-ttu-id="f6f5f-198">資源檔命名</span><span class="sxs-lookup"><span data-stu-id="f6f5f-198">Resource file naming</span></span>

<span data-ttu-id="f6f5f-199">資源的命名方式是以其類別的完整類型名稱去掉組件名稱而得。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-199">Resources are named for the full type name of their class minus the assembly name.</span></span> <span data-ttu-id="f6f5f-200">例如，假設專案中的法文資源是 `LocalizationWebsite.Web.Startup` 類別、主要組件為 `LocalizationWebsite.Web.dll`，就會命名為 *Startup.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-200">For example, a French resource in a project whose main assembly is `LocalizationWebsite.Web.dll` for the class `LocalizationWebsite.Web.Startup` would be named *Startup.fr.resx*.</span></span> <span data-ttu-id="f6f5f-201">若是 `LocalizationWebsite.Web.Controllers.HomeController` 類別的資源，則應命名為 *Controllers.HomeController.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-201">A resource for the class `LocalizationWebsite.Web.Controllers.HomeController` would be named *Controllers.HomeController.fr.resx*.</span></span> <span data-ttu-id="f6f5f-202">如果目標類別的命名空間和組件名稱不相同，則需要使用完整類型名稱。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-202">If your targeted class's namespace isn't the same as the assembly name you will need the full type name.</span></span> <span data-ttu-id="f6f5f-203">比方說，範例專案中 `ExtraNamespace.Tools` 類型的資源會命名為 *ExtraNamespace.Tools.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-203">For example, in the sample project a resource for the type `ExtraNamespace.Tools` would be named *ExtraNamespace.Tools.fr.resx*.</span></span>

<span data-ttu-id="f6f5f-204">在範例專案中，`ConfigureServices` 方法會將 `ResourcesPath` 設為 "Resources"，因此首頁控制器的法文資源檔專案相對路徑即為 *Resources/Controllers.HomeController.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-204">In the sample project, the `ConfigureServices` method sets the `ResourcesPath` to "Resources", so the project relative path for the home controller's French resource file is *Resources/Controllers.HomeController.fr.resx*.</span></span> <span data-ttu-id="f6f5f-205">或者，您可以使用資料夾來收集資源檔。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-205">Alternatively, you can use folders to organize resource files.</span></span> <span data-ttu-id="f6f5f-206">若是首頁控制器，路徑就是 *Resources/Controllers/HomeController.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-206">For the home controller, the path would be *Resources/Controllers/HomeController.fr.resx*.</span></span> <span data-ttu-id="f6f5f-207">如果您不使用 `ResourcesPath` 選項，*.resx* 檔案即會放置在專案的基底目錄中。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-207">If you don't use the `ResourcesPath` option, the *.resx* file would go in the project base directory.</span></span> <span data-ttu-id="f6f5f-208">`HomeController` 的資源檔會命名為 *Controllers.HomeController.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-208">The resource file for `HomeController` would be named *Controllers.HomeController.fr.resx*.</span></span> <span data-ttu-id="f6f5f-209">您可依據自己的資源檔收集方式，來選擇要使用點或路徑的命名慣例。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-209">The choice of using the dot or path naming convention depends on how you want to organize your resource files.</span></span>

| <span data-ttu-id="f6f5f-210">資源名稱</span><span class="sxs-lookup"><span data-stu-id="f6f5f-210">Resource name</span></span> | <span data-ttu-id="f6f5f-211">點或路徑命名</span><span class="sxs-lookup"><span data-stu-id="f6f5f-211">Dot or path naming</span></span> |
| ------------   | ------------- |
| <span data-ttu-id="f6f5f-212">Resources/Controllers.HomeController.fr.resx</span><span class="sxs-lookup"><span data-stu-id="f6f5f-212">Resources/Controllers.HomeController.fr.resx</span></span> | <span data-ttu-id="f6f5f-213">點</span><span class="sxs-lookup"><span data-stu-id="f6f5f-213">Dot</span></span>  |
| <span data-ttu-id="f6f5f-214">Resources/Controllers/HomeController.fr.resx</span><span class="sxs-lookup"><span data-stu-id="f6f5f-214">Resources/Controllers/HomeController.fr.resx</span></span>  | <span data-ttu-id="f6f5f-215">路徑</span><span class="sxs-lookup"><span data-stu-id="f6f5f-215">Path</span></span> |
|    |     |

<span data-ttu-id="f6f5f-216">在 views 中使用的資源檔會 `@inject IViewLocalizer` Razor 遵循類似的模式。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-216">Resource files using `@inject IViewLocalizer` in Razor views follow a similar pattern.</span></span> <span data-ttu-id="f6f5f-217">您可以使用點命名或路徑命名方式，來命名檢視的資源檔。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-217">The resource file for a view can be named using either dot naming or path naming.</span></span> Razor<span data-ttu-id="f6f5f-218">查看資源檔模擬其相關聯之視圖檔案的路徑。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-218"> view resource files mimic the path of their associated view file.</span></span> <span data-ttu-id="f6f5f-219">假設我們將 `ResourcesPath` 設為 "Resources"，與 *Views/Home/About.cshtml* 檢視建立關聯的法文資源檔可為下列其一：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-219">Assuming we set the `ResourcesPath` to "Resources", the French resource file associated with the *Views/Home/About.cshtml* view could be either of the following:</span></span>

* <span data-ttu-id="f6f5f-220">Resources/Views/Home/About.fr.resx</span><span class="sxs-lookup"><span data-stu-id="f6f5f-220">Resources/Views/Home/About.fr.resx</span></span>

* <span data-ttu-id="f6f5f-221">Resources/Views.Home.About.fr.resx</span><span class="sxs-lookup"><span data-stu-id="f6f5f-221">Resources/Views.Home.About.fr.resx</span></span>

<span data-ttu-id="f6f5f-222">如果您不使用 `ResourcesPath` 選項，則檢視的 *.resx* 檔案會與檢視位於相同資料夾中。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-222">If you don't use the `ResourcesPath` option, the *.resx* file for a view would be located in the same folder as the view.</span></span>

### <a name="rootnamespaceattribute"></a><span data-ttu-id="f6f5f-223">RootNamespaceAttribute</span><span class="sxs-lookup"><span data-stu-id="f6f5f-223">RootNamespaceAttribute</span></span> 

<span data-ttu-id="f6f5f-224">[RootNamespace](/dotnet/api/microsoft.extensions.localization.rootnamespaceattribute?view=aspnetcore-2.1) 屬性會在組件的根命名空間與組件名稱不同時，提供組件的根命名空間。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-224">The [RootNamespace](/dotnet/api/microsoft.extensions.localization.rootnamespaceattribute?view=aspnetcore-2.1) attribute provides the root namespace of an assembly when the root namespace of an assembly is different than the assembly name.</span></span> 

> [!WARNING]
> <span data-ttu-id="f6f5f-225">當專案的名稱不是有效的 .NET 識別碼時，就可能發生這種情況。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-225">This can occur when a project's name is not a valid .NET identifier.</span></span> <span data-ttu-id="f6f5f-226">例如， `my-project-name.csproj` 會使用根命名空間 `my_project_name` ，以及 `my-project-name` 導致此錯誤的元件名稱。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-226">For instance `my-project-name.csproj` will use the root namespace `my_project_name` and the assembly name `my-project-name` leading to this error.</span></span> 

<span data-ttu-id="f6f5f-227">如果組件的根命名空間與組件名稱不同：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-227">If the root namespace of an assembly is different than the assembly name:</span></span>

* <span data-ttu-id="f6f5f-228">根據預設，當地語系化無法運作。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-228">Localization does not work by default.</span></span>
* <span data-ttu-id="f6f5f-229">在組件中搜尋資源的方式造成當地語系化失敗。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-229">Localization fails due to the way resources are searched for within the assembly.</span></span> <span data-ttu-id="f6f5f-230">`RootNamespace` 是建置時間值，無法用於執行處理序。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-230">`RootNamespace` is a build-time value which is not available to the executing process.</span></span> 

<span data-ttu-id="f6f5f-231">如果 `RootNamespace` 與 `AssemblyName` 不同，請將下列內容納入 *AssemblyInfo.cs* (參數值取代為實際值)：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-231">If the `RootNamespace` is different from the `AssemblyName`, include the following in *AssemblyInfo.cs* (with parameter values replaced with the actual values):</span></span>

```csharp
using System.Reflection;
using Microsoft.Extensions.Localization;

[assembly: ResourceLocation("Resource Folder Name")]
[assembly: RootNamespace("App Root Namespace")]
```

<span data-ttu-id="f6f5f-232">上述程式碼可成功解析 resx 檔案。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-232">The preceding code enables the successful resolution of resx files.</span></span>

## <a name="culture-fallback-behavior"></a><span data-ttu-id="f6f5f-233">文化特性後援行為</span><span class="sxs-lookup"><span data-stu-id="f6f5f-233">Culture fallback behavior</span></span>

<span data-ttu-id="f6f5f-234">搜尋資源時，當地語系化會使用「文化特性後援」。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-234">When searching for a resource, localization engages in "culture fallback".</span></span> <span data-ttu-id="f6f5f-235">從所要求的文化特性開始，如果找不到，就會還原成該文化特性的父文化特性。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-235">Starting from the requested culture, if not found, it reverts to the parent culture of that culture.</span></span> <span data-ttu-id="f6f5f-236">另外，[CultureInfo.Parent](/dotnet/api/system.globalization.cultureinfo.parent) 屬性代表父文化特性。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-236">As an aside, the [CultureInfo.Parent](/dotnet/api/system.globalization.cultureinfo.parent) property represents the parent culture.</span></span> <span data-ttu-id="f6f5f-237">這通常 (但並非一定) 表示從 ISO 中移除國家意符 (Signifier)。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-237">This usually (but not always) means removing the national signifier from the ISO.</span></span> <span data-ttu-id="f6f5f-238">例如，墨西哥的西班牙文方言是 "es-MX"。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-238">For example, the dialect of Spanish spoken in Mexico is "es-MX".</span></span> <span data-ttu-id="f6f5f-239">它具有父系 "es" &mdash; 西班牙文不是任何國家/地區的特定項目。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-239">It has the parent "es"&mdash;Spanish non-specific to any country.</span></span>

<span data-ttu-id="f6f5f-240">假設您的網站收到使用文化特性 "fr-CA" 之 "Welcome" 資源的要求。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-240">Imagine your site receives a request for a "Welcome" resource using culture "fr-CA".</span></span> <span data-ttu-id="f6f5f-241">當地語系化系統會依照順序尋找下列資源，並選取第一個相符項目：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-241">The localization system looks for the following resources, in order, and selects the first match:</span></span>

* <span data-ttu-id="f6f5f-242">*Welcome.fr-CA.resx*</span><span class="sxs-lookup"><span data-stu-id="f6f5f-242">*Welcome.fr-CA.resx*</span></span>
* <span data-ttu-id="f6f5f-243">*Welcome.fr.resx*</span><span class="sxs-lookup"><span data-stu-id="f6f5f-243">*Welcome.fr.resx*</span></span>
* <span data-ttu-id="f6f5f-244">*Welcome.resx* (如果 `NeutralResourcesLanguage` 是 "fr-CA")</span><span class="sxs-lookup"><span data-stu-id="f6f5f-244">*Welcome.resx* (if the `NeutralResourcesLanguage` is "fr-CA")</span></span>

<span data-ttu-id="f6f5f-245">舉例來說，如果您移除 ".fr" 文化特性指示項，並將文化特性設定為法文，則系統會讀取預設資源檔，並將字串當地語系化。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-245">As an example, if you remove the ".fr" culture designator and you have the culture set to French, the default resource file is read and strings are localized.</span></span> <span data-ttu-id="f6f5f-246">當沒有任何項目符合您要求的文化特性時，資源管理員即會指定預設資源或後援資源。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-246">The Resource manager designates a default or fallback resource for when nothing meets your requested culture.</span></span> <span data-ttu-id="f6f5f-247">如果您只想在要求的文化特性缺少資源時傳回索引鍵，就不能使用預設資源檔。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-247">If you want to just return the key when missing a resource for the requested culture you must not have a default resource file.</span></span>

### <a name="generate-resource-files-with-visual-studio"></a><span data-ttu-id="f6f5f-248">使用 Visual Studio 產生資源檔</span><span class="sxs-lookup"><span data-stu-id="f6f5f-248">Generate resource files with Visual Studio</span></span>

<span data-ttu-id="f6f5f-249">如果您在 Visual Studio 中建立資源檔，但檔案名稱中不含文化特性 (例如 *Welcome.resx*)，則 Visual Studio 會針對每個字串的屬性建立 C# 類別。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-249">If you create a resource file in Visual Studio without a culture in the file name (for example, *Welcome.resx*), Visual Studio will create a C# class with a property for each string.</span></span> <span data-ttu-id="f6f5f-250">但這通常不是您使用 ASP.NET Core 的初衷。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-250">That's usually not what you want with ASP.NET Core.</span></span> <span data-ttu-id="f6f5f-251">一般來說，您不會有預設的 *.resx* 資源檔 (不含文化特性名稱的 *.resx* 檔案)。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-251">You typically don't have a default *.resx* resource file (a *.resx* file without the culture name).</span></span> <span data-ttu-id="f6f5f-252">因此，建議您建立含有文化特性名稱的 *.resx* 檔案 (例如 *Welcome.fr.resx*)。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-252">We suggest you create the *.resx* file with a culture name (for example *Welcome.fr.resx*).</span></span> <span data-ttu-id="f6f5f-253">當您建立含有文化特性名稱的 *.resx* 檔案時，Visual Studio 就不會產生類別檔案。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-253">When you create a *.resx* file with a culture name, Visual Studio won't generate the class file.</span></span>

### <a name="add-other-cultures"></a><span data-ttu-id="f6f5f-254">新增其他文化特性</span><span class="sxs-lookup"><span data-stu-id="f6f5f-254">Add other cultures</span></span>

<span data-ttu-id="f6f5f-255">每種語言和文化特性的組合 (非預設語言) 都需要唯一的資源檔。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-255">Each language and culture combination (other than the default language) requires a unique resource file.</span></span> <span data-ttu-id="f6f5f-256">若要建立不同文化特性和地區設定的資源檔，您可以建立新的資源檔並將 ISO 語言代碼作為檔名的一部分 (例如 **en-us**、**fr-ca** 和 **en-gb**)。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-256">You create resource files for different cultures and locales by creating new resource files in which the ISO language codes are part of the file name (for example, **en-us**, **fr-ca**, and **en-gb**).</span></span> <span data-ttu-id="f6f5f-257">您應將 ISO 代碼置於檔案名稱和 *.resx* 副檔名之間，例如 *Welcome.es-MX.resx* (西班牙文/墨西哥)。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-257">These ISO codes are placed between the file name and the *.resx* file extension, as in *Welcome.es-MX.resx* (Spanish/Mexico).</span></span>

## <a name="implement-a-strategy-to-select-the-languageculture-for-each-request"></a><span data-ttu-id="f6f5f-258">實作可依據每項要求選取語言/文化特性的策略</span><span class="sxs-lookup"><span data-stu-id="f6f5f-258">Implement a strategy to select the language/culture for each request</span></span>

### <a name="configure-localization"></a><span data-ttu-id="f6f5f-259">設定當地語系化</span><span class="sxs-lookup"><span data-stu-id="f6f5f-259">Configure localization</span></span>

<span data-ttu-id="f6f5f-260">您可以在 `Startup.ConfigureServices` 方法中設定當地語系化：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-260">Localization is configured in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](localization/sample/3.x/Localization/Startup.cs?name=snippet1)]

* <span data-ttu-id="f6f5f-261">`AddLocalization` 可將當地語系化服務新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-261">`AddLocalization` Adds the localization services to the services container.</span></span> <span data-ttu-id="f6f5f-262">上方的程式碼也會將資源路徑設為 "Resources"。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-262">The code above also sets the resources path to "Resources".</span></span>

* <span data-ttu-id="f6f5f-263">`AddViewLocalization` 可支援當地語系化的檢視檔案。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-263">`AddViewLocalization` Adds support for localized view files.</span></span> <span data-ttu-id="f6f5f-264">在此範例中，檢視的當地語系化會以檢視檔案的後置詞為依據。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-264">In this sample view localization is based on the view file suffix.</span></span> <span data-ttu-id="f6f5f-265">例如 *Index.fr.cshtml* 檔案中的 "fr"。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-265">For example "fr" in the *Index.fr.cshtml* file.</span></span>

* <span data-ttu-id="f6f5f-266">`AddDataAnnotationsLocalization` 可支援透過 `IStringLocalizer` 抽象概念而來的當地語系化 `DataAnnotations` 驗證訊息。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-266">`AddDataAnnotationsLocalization` Adds support for localized `DataAnnotations` validation messages through `IStringLocalizer` abstractions.</span></span>

### <a name="localization-middleware"></a><span data-ttu-id="f6f5f-267">當地語系化中介軟體</span><span class="sxs-lookup"><span data-stu-id="f6f5f-267">Localization middleware</span></span>

<span data-ttu-id="f6f5f-268">您可以在當地語系化[中介軟體](xref:fundamentals/middleware/index)中，設定要求目前的文化特性。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-268">The current culture on a request is set in the localization [Middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="f6f5f-269">已在 `Startup.Configure` 方法中啟用當地語系化中介軟體。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-269">The localization middleware is enabled in the `Startup.Configure` method.</span></span> <span data-ttu-id="f6f5f-270">您必須在可能檢查要求文化特性的任何中介軟體之前，設定當地語系化中介軟體 (例如 `app.UseMvcWithDefaultRoute()`)。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-270">The localization middleware must be configured before any middleware which might check the request culture (for example, `app.UseMvcWithDefaultRoute()`).</span></span>

[!code-csharp[](localization/sample/3.x/Localization/Startup.cs?name=snippet2)]
[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

<span data-ttu-id="f6f5f-271">`UseRequestLocalization` 會初始化 `RequestLocalizationOptions` 物件。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-271">`UseRequestLocalization` initializes a `RequestLocalizationOptions` object.</span></span> <span data-ttu-id="f6f5f-272">在每次要求時，系統會列舉 `RequestLocalizationOptions` 中的 `RequestCultureProvider` 清單，並使用能成功判斷要求的文化特性的第一個提供者。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-272">On every request the list of `RequestCultureProvider` in the `RequestLocalizationOptions` is enumerated and the first provider that can successfully determine the request culture is used.</span></span> <span data-ttu-id="f6f5f-273">預設的提供者是來自 `RequestLocalizationOptions` 類別：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-273">The default providers come from the `RequestLocalizationOptions` class:</span></span>

1. `QueryStringRequestCultureProvider`
2. `CookieRequestCultureProvider`
3. `AcceptLanguageHeaderRequestCultureProvider`

<span data-ttu-id="f6f5f-274">預設清單會由針對性高到低來排列。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-274">The default list goes from most specific to least specific.</span></span> <span data-ttu-id="f6f5f-275">在本文稍後，我們會說明如何變更順序，甚至新增自訂的文化特性提供者。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-275">Later in the article we'll see how you can change the order and even add a custom culture provider.</span></span> <span data-ttu-id="f6f5f-276">如果沒有任何提供者可以判斷要求的文化特性，即會使用 `DefaultRequestCulture`。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-276">If none of the providers can determine the request culture, the `DefaultRequestCulture` is used.</span></span>

### <a name="querystringrequestcultureprovider"></a><span data-ttu-id="f6f5f-277">QueryStringRequestCultureProvider</span><span class="sxs-lookup"><span data-stu-id="f6f5f-277">QueryStringRequestCultureProvider</span></span>

<span data-ttu-id="f6f5f-278">有些應用程式會使用查詢字串來設定[文化特性和 UI 文化特性](https://msdn.microsoft.com/library/system.globalization.cultureinfo.aspx)。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-278">Some apps will use a query string to set the [culture and UI culture](https://msdn.microsoft.com/library/system.globalization.cultureinfo.aspx).</span></span> <span data-ttu-id="f6f5f-279">若是使用 Cookie 或 Accept-Language 標頭方法的應用程式，您可以將查詢字串新增至 URL 以偵錯和測試程式碼。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-279">For apps that use the cookie or Accept-Language header approach, adding a query string to the URL is useful for debugging and testing code.</span></span> <span data-ttu-id="f6f5f-280">系統預設會將 `QueryStringRequestCultureProvider` 登錄為 `RequestCultureProvider` 清單中的第一個當地語系化提供者。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-280">By default, the `QueryStringRequestCultureProvider` is registered as the first localization provider in the `RequestCultureProvider` list.</span></span> <span data-ttu-id="f6f5f-281">您應傳遞查詢字串參數 `culture` 和 `ui-culture`。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-281">You pass the query string parameters `culture` and `ui-culture`.</span></span> <span data-ttu-id="f6f5f-282">下列範例會設定西班牙文/墨西哥的特定文化特性 (語言和地區)：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-282">The following example sets the specific culture (language and region) to Spanish/Mexico:</span></span>

   `http://localhost:5000/?culture=es-MX&ui-culture=es-MX`

<span data-ttu-id="f6f5f-283">如果您只傳入兩者其一 (`culture` 或 `ui-culture`)，查詢字串提供者就會使用您傳入的項目來設定這兩個值。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-283">If you only pass in one of the two (`culture` or `ui-culture`), the query string provider will set both values using the one you passed in.</span></span> <span data-ttu-id="f6f5f-284">例如，若只設定文化特性，即會同時設定 `Culture` 和 `UICulture`：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-284">For example, setting just the culture will set both the `Culture` and the `UICulture`:</span></span>

   `http://localhost:5000/?culture=es-MX`

### <a name="cookierequestcultureprovider"></a><span data-ttu-id="f6f5f-285">CookieRequestCultureProvider</span><span class="sxs-lookup"><span data-stu-id="f6f5f-285">CookieRequestCultureProvider</span></span>

<span data-ttu-id="f6f5f-286">生產環境應用程式通常會提供一個機制，來設定 ASP.NET Core 文化特性 Cookie 的文化特性。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-286">Production apps will often provide a mechanism to set the culture with the ASP.NET Core culture cookie.</span></span> <span data-ttu-id="f6f5f-287">若要建立 Cookie，請使用 `MakeCookieValue` 方法。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-287">Use the `MakeCookieValue` method to create a cookie.</span></span>

<span data-ttu-id="f6f5f-288">會傳回 `CookieRequestCultureProvider` `DefaultCookieName` 用來追蹤使用者慣用文化特性資訊的預設 cookie 名稱。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-288">The `CookieRequestCultureProvider` `DefaultCookieName` returns the default cookie name used to track the user's preferred culture information.</span></span> <span data-ttu-id="f6f5f-289">預設 Cookie 名稱為 `.AspNetCore.Culture`。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-289">The default cookie name is `.AspNetCore.Culture`.</span></span>

<span data-ttu-id="f6f5f-290">Cookie 格式為 `c=%LANGCODE%|uic=%LANGCODE%`，其中 `c` 是 `Culture` 而 `uic` 是 `UICulture`，例如：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-290">The cookie format is `c=%LANGCODE%|uic=%LANGCODE%`, where `c` is `Culture` and `uic` is `UICulture`, for example:</span></span>

    c=en-UK|uic=en-US

<span data-ttu-id="f6f5f-291">如果您只指定文化特性資訊和 UI 文化特性其一，系統就會將您所指定的文化特性用於文化特性資訊和 UI 文化特性。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-291">If you only specify one of culture info and UI culture, the specified culture will be used for both culture info and UI culture.</span></span>

### <a name="the-accept-language-http-header"></a><span data-ttu-id="f6f5f-292">Accept-Language HTTP 標頭</span><span class="sxs-lookup"><span data-stu-id="f6f5f-292">The Accept-Language HTTP header</span></span>

<span data-ttu-id="f6f5f-293">您可在大多數的瀏覽器中設定 [Accept-Language 標頭](https://www.w3.org/International/questions/qa-accept-lang-locales)，其最初的設計目的是用來指定使用者的語言。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-293">The [Accept-Language header](https://www.w3.org/International/questions/qa-accept-lang-locales) is settable in most browsers and was originally intended to specify the user's language.</span></span> <span data-ttu-id="f6f5f-294">這項設定可指出瀏覽器已設好要傳送哪些項目，或已從基礎作業系統繼承哪些項目。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-294">This setting indicates what the browser has been set to send or has inherited from the underlying operating system.</span></span> <span data-ttu-id="f6f5f-295">透過瀏覽器要求的 Accept-Language HTTP 標頭來偵測使用者的慣用語言，並非萬無一失 (請參閱 [Setting language preferences in a browser](https://www.w3.org/International/questions/qa-lang-priorities.en.php) (在瀏覽器中設定語言喜好設定)。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-295">The Accept-Language HTTP header from a browser request isn't an infallible way to detect the user's preferred language (see [Setting language preferences in a browser](https://www.w3.org/International/questions/qa-lang-priorities.en.php)).</span></span> <span data-ttu-id="f6f5f-296">生產環境應用程式應該包含可讓使用者自訂文化特性的選擇方式。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-296">A production app should include a way for a user to customize their choice of culture.</span></span>

### <a name="set-the-accept-language-http-header-in-ie"></a><span data-ttu-id="f6f5f-297">在 IE 中設定 Accept-Language HTTP 標頭</span><span class="sxs-lookup"><span data-stu-id="f6f5f-297">Set the Accept-Language HTTP header in IE</span></span>

1. <span data-ttu-id="f6f5f-298">從齒輪圖示，點選 [網際網路選項]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-298">From the gear icon, tap **Internet Options**.</span></span>

2. <span data-ttu-id="f6f5f-299">點選 [語言]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-299">Tap **Languages**.</span></span>

    ![網際網路選項](localization/_static/lang.png)

3. <span data-ttu-id="f6f5f-301">點選 [設定語言喜好設定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-301">Tap **Set Language Preferences**.</span></span>

4. <span data-ttu-id="f6f5f-302">點選 [新增語言]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-302">Tap **Add a language**.</span></span>

5. <span data-ttu-id="f6f5f-303">新增語言。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-303">Add the language.</span></span>

6. <span data-ttu-id="f6f5f-304">點選語言，然後點選 [上移]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-304">Tap the language, then tap **Move Up**.</span></span>

### <a name="use-a-custom-provider"></a><span data-ttu-id="f6f5f-305">使用自訂提供者</span><span class="sxs-lookup"><span data-stu-id="f6f5f-305">Use a custom provider</span></span>

<span data-ttu-id="f6f5f-306">假設您想要讓客戶將他們的語言和文化特性儲存在您的資料庫中。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-306">Suppose you want to let your customers store their language and culture in your databases.</span></span> <span data-ttu-id="f6f5f-307">您可以撰寫提供者，以供使用者查詢這些值。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-307">You could write a provider to look up these values for the user.</span></span> <span data-ttu-id="f6f5f-308">下列程式碼會示範如何新增自訂提供者：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-308">The following code shows how to add a custom provider:</span></span>

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

<span data-ttu-id="f6f5f-309">使用 `RequestLocalizationOptions` 新增或移除當地語系化提供者。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-309">Use `RequestLocalizationOptions` to add or remove localization providers.</span></span>

### <a name="set-the-culture-programmatically"></a><span data-ttu-id="f6f5f-310">以程式設計方式來設定文化特性</span><span class="sxs-lookup"><span data-stu-id="f6f5f-310">Set the culture programmatically</span></span>

<span data-ttu-id="f6f5f-311">[GitHub](https://github.com/aspnet/entropy) 上的這個範例 **Localization.StarterWeb** 專案包含可設定 `Culture` 的 UI。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-311">This sample **Localization.StarterWeb** project on [GitHub](https://github.com/aspnet/entropy) contains UI to set the `Culture`.</span></span> <span data-ttu-id="f6f5f-312">*Views/Shared/_SelectLanguagePartial.cshtml* 檔可讓您從支援的文化特性清單中選取文化特性：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-312">The *Views/Shared/_SelectLanguagePartial.cshtml* file allows you to select the culture from the list of supported cultures:</span></span>

[!code-cshtml[](localization/sample/3.x/Localization/Views/Shared/_SelectLanguagePartial.cshtml)]

<span data-ttu-id="f6f5f-313">系統會將 *Views/Shared/_SelectLanguagePartial.cshtml* 檔案新增至配置檔案的 `footer` 區段，以供所有檢視使用：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-313">The *Views/Shared/_SelectLanguagePartial.cshtml* file is added to the `footer` section of the layout file so it will be available to all views:</span></span>

[!code-cshtml[](localization/sample/3.x/Localization/Views/Shared/_Layout.cshtml?range=43-56&highlight=10)]

<span data-ttu-id="f6f5f-314">`SetLanguage` 方法會設定文化特性的 Cookie。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-314">The `SetLanguage` method sets the culture cookie.</span></span>

[!code-csharp[](localization/sample/3.x/Localization/Controllers/HomeController.cs?range=57-67)]

<span data-ttu-id="f6f5f-315">您無法將 *_SelectLanguagePartial.cshtml* 插入這個專案的範例程式碼。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-315">You can't plug in the *_SelectLanguagePartial.cshtml* to sample code for this project.</span></span> <span data-ttu-id="f6f5f-316">[GitHub](https://github.com/aspnet/entropy)上的**localization.starterweb**專案具有程式碼，可透過相依性 `RequestLocalizationOptions` 插入容器將傳遞至 Razor 部分。 [Dependency Injection](dependency-injection.md)</span><span class="sxs-lookup"><span data-stu-id="f6f5f-316">The **Localization.StarterWeb** project on [GitHub](https://github.com/aspnet/entropy) has code to flow the `RequestLocalizationOptions` to a Razor partial through the [Dependency Injection](dependency-injection.md) container.</span></span>

## <a name="model-binding-route-data-and-query-strings"></a><span data-ttu-id="f6f5f-317">模型系結路由資料和查詢字串</span><span class="sxs-lookup"><span data-stu-id="f6f5f-317">Model binding route data and query strings</span></span>

<span data-ttu-id="f6f5f-318">請參閱模型系結[路由資料和查詢字串的全球化行為](xref:mvc/models/model-binding#glob)。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-318">See [Globalization behavior of model binding route data and query strings](xref:mvc/models/model-binding#glob).</span></span>

## <a name="globalization-and-localization-terms"></a><span data-ttu-id="f6f5f-319">全球化和當地語系化詞彙</span><span class="sxs-lookup"><span data-stu-id="f6f5f-319">Globalization and localization terms</span></span>

<span data-ttu-id="f6f5f-320">在進行應用程式的當地語系化程序時，您也需要具備現代軟體開發中常用的相關字元集基本知識，並了解與它們建立關聯的問題。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-320">The process of localizing your app also requires a basic understanding of relevant character sets commonly used in modern software development and an understanding of the issues associated with them.</span></span> <span data-ttu-id="f6f5f-321">雖然所有電腦都會將文字儲存為數字 (程式碼)，但不同系統會使用不同數字來儲存相同的文字。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-321">Although all computers store text as numbers (codes), different systems store the same text using different numbers.</span></span> <span data-ttu-id="f6f5f-322">當地語系化是指針對特定文化特性/地區設定，轉譯應用程式使用者介面 (UI) 的程序。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-322">The localization process refers to translating the app user interface (UI) for a specific culture/locale.</span></span>

<span data-ttu-id="f6f5f-323">[可當地語系化](/dotnet/standard/globalization-localization/localizability-review)是確認全球化應用程式已準備好進行當地語系化的中繼程序。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-323">[Localizability](/dotnet/standard/globalization-localization/localizability-review) is an intermediate process for verifying that a globalized app is ready for localization.</span></span>

<span data-ttu-id="f6f5f-324">文化特性名稱的 [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) 格式是 `<languagecode2>-<country/regioncode2>`，其中 `<languagecode2>` 是語言代碼，而 `<country/regioncode2>` 是子文化特性代碼。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-324">The [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) format for the culture name is `<languagecode2>-<country/regioncode2>`, where `<languagecode2>` is the language code and `<country/regioncode2>` is the subculture code.</span></span> <span data-ttu-id="f6f5f-325">例如，`es-CL` 是指西班牙文 (智利)，`en-US` 是指英文 (美國)，而 `en-AU` 是指英文 (澳大利亞)。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-325">For example, `es-CL` for Spanish (Chile), `en-US` for English (United States), and `en-AU` for English (Australia).</span></span> <span data-ttu-id="f6f5f-326">[RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) 是 ISO 639 (兩個小寫字母，為與某個語言建立關聯的文化特性代碼) 及 ISO 3166 (兩個大寫字母，為與某個國家或地區建立關聯的子文化特性代碼) 的組合。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-326">[RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) is a combination of an ISO 639 two-letter lowercase culture code associated with a language and an ISO 3166 two-letter uppercase subculture code associated with a country or region.</span></span> <span data-ttu-id="f6f5f-327">請參閱 [Language Culture Name](https://msdn.microsoft.com/library/ee825488(v=cs.20).aspx) (語言的文化特性名稱)。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-327">See [Language Culture Name](https://msdn.microsoft.com/library/ee825488(v=cs.20).aspx).</span></span>

<span data-ttu-id="f6f5f-328">國際化通常縮寫為 "I18N"。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-328">Internationalization is often abbreviated to "I18N".</span></span> <span data-ttu-id="f6f5f-329">這個縮寫是採用該詞彙的第一個和最後一個字母，以及這兩個字母間的字母數組成，因此 18 代表第一個字母 "I" 及最後 "N" 中間的字母數。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-329">The abbreviation takes the first and last letters and the number of letters between them, so 18 stands for the number of letters between the first "I" and the last "N".</span></span> <span data-ttu-id="f6f5f-330">同樣的原則也適用於全球化 (G11N) 與當地語系化 (L10N) 的縮寫。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-330">The same applies to Globalization (G11N), and Localization (L10N).</span></span>

<span data-ttu-id="f6f5f-331">詞彙：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-331">Terms:</span></span>

* <span data-ttu-id="f6f5f-332">全球化 (G11N)：讓應用程式支援不同語言和區域的程序。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-332">Globalization (G11N): The process of making an app support different languages and regions.</span></span>
* <span data-ttu-id="f6f5f-333">當地語系化 (L10N)：針對特定語言和區域，自訂應用程式的程序。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-333">Localization (L10N): The process of customizing an app for a given language and region.</span></span>
* <span data-ttu-id="f6f5f-334">國際化 (I18N)：包含全球化和當地語系化這兩部分的描述。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-334">Internationalization (I18N): Describes both globalization and localization.</span></span>
* <span data-ttu-id="f6f5f-335">文化特性：它是一種語言和/或地區。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-335">Culture: It's a language and, optionally, a region.</span></span>
* <span data-ttu-id="f6f5f-336">中性文化特性：具有指定的語言但不限地區的文化特性 </span><span class="sxs-lookup"><span data-stu-id="f6f5f-336">Neutral culture: A culture that has a specified language, but not a region.</span></span> <span data-ttu-id="f6f5f-337">(例如 "en"、"es")。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-337">(for example "en", "es")</span></span>
* <span data-ttu-id="f6f5f-338">特定文化特性：具有指定的語言和地區的文化特性 </span><span class="sxs-lookup"><span data-stu-id="f6f5f-338">Specific culture: A culture that has a specified language and region.</span></span> <span data-ttu-id="f6f5f-339">(例如 "en-US"、"en-GB"、"es-CL")。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-339">(for example "en-US", "en-GB", "es-CL")</span></span>
* <span data-ttu-id="f6f5f-340">父文化特性：包含特定文化特性的中性文化特性 </span><span class="sxs-lookup"><span data-stu-id="f6f5f-340">Parent culture: The neutral culture that contains a specific culture.</span></span> <span data-ttu-id="f6f5f-341">(例如，"en" 是 "en-US" 和 "en-GB" 的父文化特性)。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-341">(for example, "en" is the parent culture of "en-US" and "en-GB")</span></span>
* <span data-ttu-id="f6f5f-342">地區設定：地區設定與文化特性相同。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-342">Locale: A locale is the same as a culture.</span></span>

[!INCLUDE[](~/includes/localization/currency.md)]

[!INCLUDE[](~/includes/localization/unsupported-culture-log-level.md)]

## <a name="additional-resources"></a><span data-ttu-id="f6f5f-343">其他資源</span><span class="sxs-lookup"><span data-stu-id="f6f5f-343">Additional resources</span></span>

* <xref:fundamentals/troubleshoot-aspnet-core-localization>
* <span data-ttu-id="f6f5f-344">本文使用的 [Localization.StarterWeb 專案](https://github.com/aspnet/Entropy/tree/master/samples/Localization.StarterWeb)。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-344">[Localization.StarterWeb project](https://github.com/aspnet/Entropy/tree/master/samples/Localization.StarterWeb) used in the article.</span></span>
* [<span data-ttu-id="f6f5f-345">全球化與當地語系化 .NET 應用程式</span><span class="sxs-lookup"><span data-stu-id="f6f5f-345">Globalizing and localizing .NET applications</span></span>](/dotnet/standard/globalization-localization/index)
* [<span data-ttu-id="f6f5f-346">.Resx 檔案中的資源</span><span class="sxs-lookup"><span data-stu-id="f6f5f-346">Resources in .resx Files</span></span>](/dotnet/framework/resources/working-with-resx-files-programmatically)
* [<span data-ttu-id="f6f5f-347">Microsoft 多語應用程式工具組</span><span class="sxs-lookup"><span data-stu-id="f6f5f-347">Microsoft Multilingual App Toolkit</span></span>](https://marketplace.visualstudio.com/items?itemName=MultilingualAppToolkit.MultilingualAppToolkit-18308)
* [<span data-ttu-id="f6f5f-348">當地語系化和泛型</span><span class="sxs-lookup"><span data-stu-id="f6f5f-348">Localization & Generics</span></span>](http://hishambinateya.com/localization-and-generics)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="f6f5f-349">由 [Rick Anderson](https://twitter.com/RickAndMSFT)、[Damien Bowden](https://twitter.com/damien_bod)、[Bart Calixto](https://twitter.com/bartmax)、[Nadeem Afana](https://afana.me/) 和 [Hisham Bin Ateya](https://twitter.com/hishambinateya) 提供</span><span class="sxs-lookup"><span data-stu-id="f6f5f-349">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Damien Bowden](https://twitter.com/damien_bod), [Bart Calixto](https://twitter.com/bartmax), [Nadeem Afana](https://afana.me/), and [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span></span>

<span data-ttu-id="f6f5f-350">多語系網站可讓網站觸及更多物件。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-350">A multilingual website allows the site to reach a wider audience.</span></span> <span data-ttu-id="f6f5f-351">ASP.NET Core 提供服務與中介軟體，可將網站當地語系化成不同的語言與文化特性。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-351">ASP.NET Core provides services and middleware for localizing into different languages and cultures.</span></span>

<span data-ttu-id="f6f5f-352">國際化包含[全球化](/dotnet/api/system.globalization)和[當地語系化](/dotnet/standard/globalization-localization/localization)這兩部分。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-352">Internationalization involves [Globalization](/dotnet/api/system.globalization) and [Localization](/dotnet/standard/globalization-localization/localization).</span></span> <span data-ttu-id="f6f5f-353">全球化是指設計出可支援不同文化特性之應用程式的程序。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-353">Globalization is the process of designing apps that support different cultures.</span></span> <span data-ttu-id="f6f5f-354">透過全球化，您可新增支援與特定地區相關之已定義語言指令碼的輸入、顯示及輸出作業。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-354">Globalization adds support for input, display, and output of a defined set of language scripts that relate to specific geographic areas.</span></span>

<span data-ttu-id="f6f5f-355">當地語系化是指針對全球化應用程式進行調整的程序，且您已順應特定文化特性/地區設定對這些全球化應用程式進行可當地語系化處理。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-355">Localization is the process of adapting a globalized app, which you have already processed for localizability, to a particular culture/locale.</span></span> <span data-ttu-id="f6f5f-356">如需詳細資訊，請參閱本文件結尾處的**全球化和當地語系化詞彙**。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-356">For more information see **Globalization and localization terms** near the end of this document.</span></span>

<span data-ttu-id="f6f5f-357">應用程式當地語系化包含下列作業：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-357">App localization involves the following:</span></span>

1. <span data-ttu-id="f6f5f-358">讓應用程式的內容可當地語系化</span><span class="sxs-lookup"><span data-stu-id="f6f5f-358">Make the app's content localizable</span></span>
1. <span data-ttu-id="f6f5f-359">針對您支援的語言和文化特性提供當地語系化資源</span><span class="sxs-lookup"><span data-stu-id="f6f5f-359">Provide localized resources for the languages and cultures you support</span></span>
1. <span data-ttu-id="f6f5f-360">實作可依據每項要求選取語言/文化特性的策略</span><span class="sxs-lookup"><span data-stu-id="f6f5f-360">Implement a strategy to select the language/culture for each request</span></span>

<span data-ttu-id="f6f5f-361">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/localization/sample/Localization)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="f6f5f-361">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/localization/sample/Localization) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="make-the-apps-content-localizable"></a><span data-ttu-id="f6f5f-362">讓應用程式的內容可當地語系化</span><span class="sxs-lookup"><span data-stu-id="f6f5f-362">Make the app's content localizable</span></span>

<span data-ttu-id="f6f5f-363"><xref:Microsoft.Extensions.Localization.IStringLocalizer>和 <xref:Microsoft.Extensions.Localization.IStringLocalizer%601> 已架構，可在開發當地語系化應用程式時改善生產力。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-363"><xref:Microsoft.Extensions.Localization.IStringLocalizer> and <xref:Microsoft.Extensions.Localization.IStringLocalizer%601> were architected to improve productivity when developing localized apps.</span></span> <span data-ttu-id="f6f5f-364">`IStringLocalizer`會使用 <xref:System.Resources.ResourceManager> 和， <xref:System.Resources.ResourceReader> 在執行時間提供特定文化特性的資源。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-364">`IStringLocalizer` uses the <xref:System.Resources.ResourceManager> and <xref:System.Resources.ResourceReader> to provide culture-specific resources at run time.</span></span> <span data-ttu-id="f6f5f-365">介面具有索引子和， `IEnumerable` 用於傳回當地語系化的字串。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-365">The interface has an indexer and an `IEnumerable` for returning localized strings.</span></span> <span data-ttu-id="f6f5f-366">`IStringLocalizer`不需要將預設語言字串儲存在資源檔中。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-366">`IStringLocalizer` doesn't require storing the default language strings in a resource file.</span></span> <span data-ttu-id="f6f5f-367">您不必在開發初期建立資源檔，即可開發以當地語系化為目標的應用程式。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-367">You can develop an app targeted for localization and not need to create resource files early in development.</span></span> <span data-ttu-id="f6f5f-368">下列程式碼會示範如何包裝 "About Title" 字串以進行當地語系化。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-368">The code below shows how to wrap the string "About Title" for localization.</span></span>

[!code-csharp[](localization/sample/3.x/Localization/Controllers/AboutController.cs)]

<span data-ttu-id="f6f5f-369">在上述程式碼中， `IStringLocalizer<T>` 執行是來自相依性[插入](dependency-injection.md)。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-369">In the preceding code, the `IStringLocalizer<T>` implementation comes from [Dependency Injection](dependency-injection.md).</span></span> <span data-ttu-id="f6f5f-370">如果找不到 "About Title" 的當地語系化值，即會傳回索引子的索引鍵，也就是 "About Title" 字串。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-370">If the localized value of "About Title" isn't found, then the indexer key is returned, that is, the string "About Title".</span></span> <span data-ttu-id="f6f5f-371">您可以保留應用程式中的預設語言常值字串，並將其包裝在當地語系化工具中，以便專注於開發應用程式。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-371">You can leave the default language literal strings in the app and wrap them in the localizer, so that you can focus on developing the app.</span></span> <span data-ttu-id="f6f5f-372">您不用先建立預設資源檔，即可使用預設語言來開發應用程式，並針對當地語系化步驟進行應用程式的準備。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-372">You develop your app with your default language and prepare it for the localization step without first creating a default resource file.</span></span> <span data-ttu-id="f6f5f-373">或者，您可以使用傳統方法，並提供索引鍵以擷取預設語言字串。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-373">Alternatively, you can use the traditional approach and provide a key to retrieve the default language string.</span></span> <span data-ttu-id="f6f5f-374">對許多開發人員來說，新的工作流程 (單純包裝字串常值而不使用預設語言的 *.resx* 檔案) 可以降低當地語系化應用程式的額外負荷。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-374">For many developers the new workflow of not having a default language *.resx* file and simply wrapping the string literals can reduce the overhead of localizing an app.</span></span> <span data-ttu-id="f6f5f-375">其他開發人員則偏好傳統的工作流程，因為這種方法更便於使用較長的字串常值，且更易於更新當地語系化的字串。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-375">Other developers will prefer the traditional work flow as it can make it easier to work with longer string literals and make it easier to update localized strings.</span></span>

<span data-ttu-id="f6f5f-376">若是包含 HTML 的資源，請使用 `IHtmlLocalizer<T>` 實作。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-376">Use the `IHtmlLocalizer<T>` implementation for resources that contain HTML.</span></span> <span data-ttu-id="f6f5f-377">`IHtmlLocalizer` 會對資源字串中經過格式化的引數進行 HTML 編碼，但不會對資源字串本身進行 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-377">`IHtmlLocalizer` HTML encodes arguments that are formatted in the resource string, but doesn't HTML encode the resource string itself.</span></span> <span data-ttu-id="f6f5f-378">在下列醒目提示的範例中，只有 `name` 參數的值經過 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-378">In the sample highlighted below, only the value of `name` parameter is HTML encoded.</span></span>

[!code-csharp[](~/fundamentals/localization/sample/3.x/Localization/Controllers/BookController.cs?highlight=3,5,20&start=1&end=24)]

<span data-ttu-id="f6f5f-379">**注意：** 一般來說，您只想將文字當地語系化，而不是 HTML。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-379">**Note:** You generally want to only localize text and not HTML.</span></span>

<span data-ttu-id="f6f5f-380">您可以在最底層的[相依性插入](dependency-injection.md)中，將 `IStringLocalizerFactory` 移出：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-380">At the lowest level, you can get `IStringLocalizerFactory` out of [Dependency Injection](dependency-injection.md):</span></span>

[!code-csharp[](localization/sample/3.x/Localization/Controllers/TestController.cs?start=9&end=26&highlight=7-13)]

<span data-ttu-id="f6f5f-381">上述程式碼會示範這兩個 Factory Create 方法。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-381">The code above demonstrates each of the two factory create methods.</span></span>

<span data-ttu-id="f6f5f-382">您可以依據控制器或區域來分割當地語系化的字串，也可以只用一個容器。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-382">You can partition your localized strings by controller, area, or have just one container.</span></span> <span data-ttu-id="f6f5f-383">在範例應用程式中，會針對共用資源使用名為 `SharedResource` 的虛擬類別。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-383">In the sample app, a dummy class named `SharedResource` is used for shared resources.</span></span>

[!code-csharp[](localization/sample/3.x/Localization/Resources/SharedResource.cs)]

<span data-ttu-id="f6f5f-384">有些開發人員會使用 `Startup` 類別來包含全域或共用字串。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-384">Some developers use the `Startup` class to contain global or shared strings.</span></span> <span data-ttu-id="f6f5f-385">下列範例會使用 `InfoController` 和 `SharedResource` 當地語系化工具：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-385">In the sample below, the `InfoController` and the `SharedResource` localizers are used:</span></span>

[!code-csharp[](localization/sample/3.x/Localization/Controllers/InfoController.cs?range=9-26)]

## <a name="view-localization"></a><span data-ttu-id="f6f5f-386">檢視當地語系化</span><span class="sxs-lookup"><span data-stu-id="f6f5f-386">View localization</span></span>

<span data-ttu-id="f6f5f-387">`IViewLocalizer` 服務可提供[檢視](xref:mvc/views/overview)的當地語系化字串。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-387">The `IViewLocalizer` service provides localized strings for a [view](xref:mvc/views/overview).</span></span> <span data-ttu-id="f6f5f-388">`ViewLocalizer` 類別會實作這個介面，並透過檢視檔案路徑來找出資源的位置。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-388">The `ViewLocalizer` class implements this interface and finds the resource location from the view file path.</span></span> <span data-ttu-id="f6f5f-389">下列程式碼示範如何使用 `IViewLocalizer` 的預設實作：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-389">The following code shows how to use the default implementation of `IViewLocalizer`:</span></span>

[!code-cshtml[](localization/sample/3.x/Localization/Views/Home/About.cshtml)]

<span data-ttu-id="f6f5f-390">`IViewLocalizer` 的預設實作會依據檢視的檔案名稱來找出資源檔。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-390">The default implementation of `IViewLocalizer` finds the resource file based on the view's file name.</span></span> <span data-ttu-id="f6f5f-391">其中並沒有任何選項可以使用全域共用的資源檔。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-391">There's no option to use a global shared resource file.</span></span> <span data-ttu-id="f6f5f-392">`ViewLocalizer`使用來執行當地語系化工具 `IHtmlLocalizer` ，因此 Razor 不會對當地語系化字串進行 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-392">`ViewLocalizer` implements the localizer using `IHtmlLocalizer`, so Razor doesn't HTML encode the localized string.</span></span> <span data-ttu-id="f6f5f-393">您可以參數化資源字串，`IViewLocalizer` 即會對參數 (而不是資源字串) 進行 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-393">You can parameterize resource strings and `IViewLocalizer` will HTML encode the parameters, but not the resource string.</span></span> <span data-ttu-id="f6f5f-394">請考慮下列 Razor 標記：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-394">Consider the following Razor markup:</span></span>

```cshtml
@Localizer["<i>Hello</i> <b>{0}!</b>", UserManager.GetUserName(User)]
```

<span data-ttu-id="f6f5f-395">法文資源檔可能包含下列內容：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-395">A French resource file could contain the following:</span></span>

| <span data-ttu-id="f6f5f-396">機碼</span><span class="sxs-lookup"><span data-stu-id="f6f5f-396">Key</span></span> | <span data-ttu-id="f6f5f-397">值</span><span class="sxs-lookup"><span data-stu-id="f6f5f-397">Value</span></span> |
| ----- | ------ |
| `<i>Hello</i> <b>{0}!</b>` | `<i>Bonjour</i> <b>{0} !</b>` |

<span data-ttu-id="f6f5f-398">轉譯的檢視內容可能包含來自資源檔的 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-398">The rendered view would contain the HTML markup from the resource file.</span></span>

<span data-ttu-id="f6f5f-399">**注意：** 一般來說，您只想將文字當地語系化，而不是 HTML。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-399">**Note:** You generally want to only localize text and not HTML.</span></span>

<span data-ttu-id="f6f5f-400">若要在檢視中使用共用的資源檔，請插入 `IHtmlLocalizer<T>`：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-400">To use a shared resource file in a view, inject `IHtmlLocalizer<T>`:</span></span>

[!code-cshtml[](~/fundamentals/localization/sample/3.x/Localization/Views/Test/About.cshtml?highlight=5,12)]

## <a name="dataannotations-localization"></a><span data-ttu-id="f6f5f-401">DataAnnotations 當地語系化</span><span class="sxs-lookup"><span data-stu-id="f6f5f-401">DataAnnotations localization</span></span>

<span data-ttu-id="f6f5f-402">DataAnnotations 錯誤訊息會使用 `IStringLocalizer<T>` 來當地語系化。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-402">DataAnnotations error messages are localized with `IStringLocalizer<T>`.</span></span> <span data-ttu-id="f6f5f-403">使用 `ResourcesPath = "Resources"` 選項時，`RegisterViewModel` 中的錯誤訊息會儲存在下列路徑之一：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-403">Using the option `ResourcesPath = "Resources"`, the error messages in `RegisterViewModel` can be stored in either of the following paths:</span></span>

* <span data-ttu-id="f6f5f-404">*Resources/Viewmodel. RegisterViewModel. fr .resx*</span><span class="sxs-lookup"><span data-stu-id="f6f5f-404">*Resources/ViewModels.Account.RegisterViewModel.fr.resx*</span></span>
* <span data-ttu-id="f6f5f-405">*Resources/ViewModels/Account/RegisterViewModel.fr.resx*</span><span class="sxs-lookup"><span data-stu-id="f6f5f-405">*Resources/ViewModels/Account/RegisterViewModel.fr.resx*</span></span>

[!code-csharp[](localization/sample/3.x/Localization/ViewModels/Account/RegisterViewModel.cs?start=9&end=26)]

<span data-ttu-id="f6f5f-406">在 ASP.NET Core MVC 1.1.0 和更高版本中，系統會將非驗證屬性當地語系化。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-406">In ASP.NET Core MVC 1.1.0 and higher, non-validation attributes are localized.</span></span> <span data-ttu-id="f6f5f-407">ASP.NET Core MVC 1.0 **不會**查閱非驗證屬性的當地語系化字串。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-407">ASP.NET Core MVC 1.0 does **not** look up localized strings for non-validation attributes.</span></span>

<a name="one-resource-string-multiple-classes"></a>

### <a name="using-one-resource-string-for-multiple-classes"></a><span data-ttu-id="f6f5f-408">針對多個類別使用同一個資源字串</span><span class="sxs-lookup"><span data-stu-id="f6f5f-408">Using one resource string for multiple classes</span></span>

<span data-ttu-id="f6f5f-409">下列程式碼會示範如何針對含有多個類別的驗證屬性使用同一個資源字串：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-409">The following code shows how to use one resource string for validation attributes with multiple classes:</span></span>

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

<span data-ttu-id="f6f5f-410">在先前的程式碼中，`SharedResource` 是與儲存驗證訊息的 resx 對應的類別。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-410">In the preceding code, `SharedResource` is the class corresponding to the resx where your validation messages are stored.</span></span> <span data-ttu-id="f6f5f-411">使用這個方法時，DataAnnotations 只會使用 `SharedResource`，而不是每個類別的資源。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-411">With this approach, DataAnnotations will only use `SharedResource`, rather than the resource for each class.</span></span>

## <a name="provide-localized-resources-for-the-languages-and-cultures-you-support"></a><span data-ttu-id="f6f5f-412">針對您支援的語言和文化特性提供當地語系化資源</span><span class="sxs-lookup"><span data-stu-id="f6f5f-412">Provide localized resources for the languages and cultures you support</span></span>

### <a name="supportedcultures-and-supporteduicultures"></a><span data-ttu-id="f6f5f-413">SupportedCultures 和 SupportedUICultures</span><span class="sxs-lookup"><span data-stu-id="f6f5f-413">SupportedCultures and SupportedUICultures</span></span>

<span data-ttu-id="f6f5f-414">ASP.NET Core 可讓您指定 `SupportedCultures` 和 `SupportedUICultures` 這兩個文化特性值。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-414">ASP.NET Core allows you to specify two culture values, `SupportedCultures` and `SupportedUICultures`.</span></span> <span data-ttu-id="f6f5f-415">`SupportedCultures` 的 [CultureInfo](/dotnet/api/system.globalization.cultureinfo) 物件可決定文化特性相依函式的結果，例如日期、時間、數字及貨幣格式。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-415">The [CultureInfo](/dotnet/api/system.globalization.cultureinfo) object for `SupportedCultures` determines the results of culture-dependent functions, such as date, time, number, and currency formatting.</span></span> <span data-ttu-id="f6f5f-416">`SupportedCultures` 也可決定文字排列順序、大小寫慣例和字串比較。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-416">`SupportedCultures` also determines the sorting order of text, casing conventions, and string comparisons.</span></span> <span data-ttu-id="f6f5f-417">如需伺服器如何取得文化特性的詳細資訊，請參閱 [CultureInfo.CurrentCulture](/dotnet/api/system.stringcomparer.currentculture#System_StringComparer_CurrentCulture)。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-417">See [CultureInfo.CurrentCulture](/dotnet/api/system.stringcomparer.currentculture#System_StringComparer_CurrentCulture) for more info on how the server gets the Culture.</span></span> <span data-ttu-id="f6f5f-418">`SupportedUICultures`會決定[ResourceManager](/dotnet/api/system.resources.resourcemanager)會查閱哪些翻譯的字串（來自 *.resx*檔案）。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-418">The `SupportedUICultures` determines which translated strings (from *.resx* files) are looked up by the [ResourceManager](/dotnet/api/system.resources.resourcemanager).</span></span> <span data-ttu-id="f6f5f-419">`ResourceManager` 僅會查閱 `CurrentUICulture` 所決定之文化特性特有的字串。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-419">The `ResourceManager` simply looks up culture-specific strings that's determined by `CurrentUICulture`.</span></span> <span data-ttu-id="f6f5f-420">.NET 中的每個執行緒都有 `CurrentCulture` 和 `CurrentUICulture` 物件。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-420">Every thread in .NET has `CurrentCulture` and `CurrentUICulture` objects.</span></span> <span data-ttu-id="f6f5f-421">ASP.NET Core 會在轉譯文化特性相依函式時檢查這些值。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-421">ASP.NET Core inspects these values when rendering culture-dependent functions.</span></span> <span data-ttu-id="f6f5f-422">比方說，如果目前執行緒的文化特性設定為 "en-US" (英文 - 美國)，`DateTime.Now.ToLongDateString()` 會顯示 "Thursday, February 18, 2016"，但如果 `CurrentCulture` 設定為 "es-ES" (西班牙文 - 西班牙)，則輸出會是 "jueves, 18 de febrero de 2016"。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-422">For example, if the current thread's culture is set to "en-US" (English, United States), `DateTime.Now.ToLongDateString()` displays "Thursday, February 18, 2016", but if `CurrentCulture` is set to "es-ES" (Spanish, Spain) the output will be "jueves, 18 de febrero de 2016".</span></span>

## <a name="resource-files"></a><span data-ttu-id="f6f5f-423">資源檔</span><span class="sxs-lookup"><span data-stu-id="f6f5f-423">Resource files</span></span>

<span data-ttu-id="f6f5f-424">資源檔是一種實用的機制，可讓您將可當地語系化的字串與代碼區隔開來。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-424">A resource file is a useful mechanism for separating localizable strings from code.</span></span> <span data-ttu-id="f6f5f-425">非預設語言的翻譯字串會在 *.resx*資源檔中隔離。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-425">Translated strings for the non-default language are isolated in *.resx* resource files.</span></span> <span data-ttu-id="f6f5f-426">例如，您可以建立名為 *Welcome.es.resx* 的西班牙文資源檔，以包含翻譯的字串。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-426">For example, you might want to create Spanish resource file named *Welcome.es.resx* containing translated strings.</span></span> <span data-ttu-id="f6f5f-427">"es" 是西班牙文的語言代碼。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-427">"es" is the language code for Spanish.</span></span> <span data-ttu-id="f6f5f-428">若要在 Visual Studio 中建立這個資源檔：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-428">To create this resource file in Visual Studio:</span></span>

1. <span data-ttu-id="f6f5f-429">在方案總管\*\*\*\* 中，以滑鼠右鍵按一下要放置資源檔的資料夾 > [新增]**[新增項目]** > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-429">In **Solution Explorer**, right click on the folder which will contain the resource file > **Add** > **New Item**.</span></span>

    ![巢狀特色選單：方案總管會開啟 [資源] 的特色選單，](localization/_static/newi.png)

2. <span data-ttu-id="f6f5f-432">在 [Search installed templates] (搜尋已安裝的範本)\*\*\*\* 方塊中，輸入「資源」，並命名檔案。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-432">In the **Search installed templates** box, enter "resource" and name the file.</span></span>

    ![[新增項目] 對話方塊](localization/_static/res.png)

3. <span data-ttu-id="f6f5f-434">在 [名稱]\*\*\*\* 資料行中輸入索引鍵值 (原生字串)，並在 [值]\*\*\*\* 資料行中輸入已翻譯的字串。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-434">Enter the key value (native string) in the **Name** column and the translated string in the **Value** column.</span></span>

    ![Welcome.es.resx 檔案 (西班牙文的「歡迎使用」資源檔)，其中 [名稱] 資料行的文字為 Hello，而 [值] 資料行的文字為 Hola (Hello 的西班牙文)](localization/_static/hola.png)

    <span data-ttu-id="f6f5f-436">Visual Studio 會顯示 *Welcome.es.resx* 檔案。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-436">Visual Studio shows the *Welcome.es.resx* file.</span></span>

    ![方案總管，其中顯示「歡迎使用」的西班牙文 (es) 資源檔](localization/_static/se.png)

## <a name="resource-file-naming"></a><span data-ttu-id="f6f5f-438">資源檔命名</span><span class="sxs-lookup"><span data-stu-id="f6f5f-438">Resource file naming</span></span>

<span data-ttu-id="f6f5f-439">資源的命名方式是以其類別的完整類型名稱去掉組件名稱而得。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-439">Resources are named for the full type name of their class minus the assembly name.</span></span> <span data-ttu-id="f6f5f-440">例如，假設專案中的法文資源是 `LocalizationWebsite.Web.Startup` 類別、主要組件為 `LocalizationWebsite.Web.dll`，就會命名為 *Startup.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-440">For example, a French resource in a project whose main assembly is `LocalizationWebsite.Web.dll` for the class `LocalizationWebsite.Web.Startup` would be named *Startup.fr.resx*.</span></span> <span data-ttu-id="f6f5f-441">若是 `LocalizationWebsite.Web.Controllers.HomeController` 類別的資源，則應命名為 *Controllers.HomeController.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-441">A resource for the class `LocalizationWebsite.Web.Controllers.HomeController` would be named *Controllers.HomeController.fr.resx*.</span></span> <span data-ttu-id="f6f5f-442">如果目標類別的命名空間和組件名稱不相同，則需要使用完整類型名稱。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-442">If your targeted class's namespace isn't the same as the assembly name you will need the full type name.</span></span> <span data-ttu-id="f6f5f-443">比方說，範例專案中 `ExtraNamespace.Tools` 類型的資源會命名為 *ExtraNamespace.Tools.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-443">For example, in the sample project a resource for the type `ExtraNamespace.Tools` would be named *ExtraNamespace.Tools.fr.resx*.</span></span>

<span data-ttu-id="f6f5f-444">在範例專案中，`ConfigureServices` 方法會將 `ResourcesPath` 設為 "Resources"，因此首頁控制器的法文資源檔專案相對路徑即為 *Resources/Controllers.HomeController.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-444">In the sample project, the `ConfigureServices` method sets the `ResourcesPath` to "Resources", so the project relative path for the home controller's French resource file is *Resources/Controllers.HomeController.fr.resx*.</span></span> <span data-ttu-id="f6f5f-445">或者，您可以使用資料夾來收集資源檔。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-445">Alternatively, you can use folders to organize resource files.</span></span> <span data-ttu-id="f6f5f-446">若是首頁控制器，路徑就是 *Resources/Controllers/HomeController.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-446">For the home controller, the path would be *Resources/Controllers/HomeController.fr.resx*.</span></span> <span data-ttu-id="f6f5f-447">如果您不使用 `ResourcesPath` 選項，*.resx* 檔案即會放置在專案的基底目錄中。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-447">If you don't use the `ResourcesPath` option, the *.resx* file would go in the project base directory.</span></span> <span data-ttu-id="f6f5f-448">`HomeController` 的資源檔會命名為 *Controllers.HomeController.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-448">The resource file for `HomeController` would be named *Controllers.HomeController.fr.resx*.</span></span> <span data-ttu-id="f6f5f-449">您可依據自己的資源檔收集方式，來選擇要使用點或路徑的命名慣例。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-449">The choice of using the dot or path naming convention depends on how you want to organize your resource files.</span></span>

| <span data-ttu-id="f6f5f-450">資源名稱</span><span class="sxs-lookup"><span data-stu-id="f6f5f-450">Resource name</span></span> | <span data-ttu-id="f6f5f-451">點或路徑命名</span><span class="sxs-lookup"><span data-stu-id="f6f5f-451">Dot or path naming</span></span> |
| ------------   | ------------- |
| <span data-ttu-id="f6f5f-452">Resources/Controllers.HomeController.fr.resx</span><span class="sxs-lookup"><span data-stu-id="f6f5f-452">Resources/Controllers.HomeController.fr.resx</span></span> | <span data-ttu-id="f6f5f-453">點</span><span class="sxs-lookup"><span data-stu-id="f6f5f-453">Dot</span></span>  |
| <span data-ttu-id="f6f5f-454">Resources/Controllers/HomeController.fr.resx</span><span class="sxs-lookup"><span data-stu-id="f6f5f-454">Resources/Controllers/HomeController.fr.resx</span></span>  | <span data-ttu-id="f6f5f-455">路徑</span><span class="sxs-lookup"><span data-stu-id="f6f5f-455">Path</span></span> |
|    |     |

<span data-ttu-id="f6f5f-456">在 views 中使用的資源檔會 `@inject IViewLocalizer` Razor 遵循類似的模式。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-456">Resource files using `@inject IViewLocalizer` in Razor views follow a similar pattern.</span></span> <span data-ttu-id="f6f5f-457">您可以使用點命名或路徑命名方式，來命名檢視的資源檔。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-457">The resource file for a view can be named using either dot naming or path naming.</span></span> Razor<span data-ttu-id="f6f5f-458">查看資源檔模擬其相關聯之視圖檔案的路徑。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-458"> view resource files mimic the path of their associated view file.</span></span> <span data-ttu-id="f6f5f-459">假設我們將 `ResourcesPath` 設為 "Resources"，與 *Views/Home/About.cshtml* 檢視建立關聯的法文資源檔可為下列其一：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-459">Assuming we set the `ResourcesPath` to "Resources", the French resource file associated with the *Views/Home/About.cshtml* view could be either of the following:</span></span>

* <span data-ttu-id="f6f5f-460">Resources/Views/Home/About.fr.resx</span><span class="sxs-lookup"><span data-stu-id="f6f5f-460">Resources/Views/Home/About.fr.resx</span></span>

* <span data-ttu-id="f6f5f-461">Resources/Views.Home.About.fr.resx</span><span class="sxs-lookup"><span data-stu-id="f6f5f-461">Resources/Views.Home.About.fr.resx</span></span>

<span data-ttu-id="f6f5f-462">如果您不使用 `ResourcesPath` 選項，則檢視的 *.resx* 檔案會與檢視位於相同資料夾中。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-462">If you don't use the `ResourcesPath` option, the *.resx* file for a view would be located in the same folder as the view.</span></span>

### <a name="rootnamespaceattribute"></a><span data-ttu-id="f6f5f-463">RootNamespaceAttribute</span><span class="sxs-lookup"><span data-stu-id="f6f5f-463">RootNamespaceAttribute</span></span> 

<span data-ttu-id="f6f5f-464">[RootNamespace](/dotnet/api/microsoft.extensions.localization.rootnamespaceattribute?view=aspnetcore-2.1) 屬性會在組件的根命名空間與組件名稱不同時，提供組件的根命名空間。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-464">The [RootNamespace](/dotnet/api/microsoft.extensions.localization.rootnamespaceattribute?view=aspnetcore-2.1) attribute provides the root namespace of an assembly when the root namespace of an assembly is different than the assembly name.</span></span> 

> [!WARNING]
> <span data-ttu-id="f6f5f-465">當專案的名稱不是有效的 .NET 識別碼時，就可能發生這種情況。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-465">This can occur when a project's name is not a valid .NET identifier.</span></span> <span data-ttu-id="f6f5f-466">例如， `my-project-name.csproj` 會使用根命名空間 `my_project_name` ，以及 `my-project-name` 導致此錯誤的元件名稱。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-466">For instance `my-project-name.csproj` will use the root namespace `my_project_name` and the assembly name `my-project-name` leading to this error.</span></span> 

<span data-ttu-id="f6f5f-467">如果組件的根命名空間與組件名稱不同：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-467">If the root namespace of an assembly is different than the assembly name:</span></span>

* <span data-ttu-id="f6f5f-468">根據預設，當地語系化無法運作。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-468">Localization does not work by default.</span></span>
* <span data-ttu-id="f6f5f-469">在組件中搜尋資源的方式造成當地語系化失敗。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-469">Localization fails due to the way resources are searched for within the assembly.</span></span> <span data-ttu-id="f6f5f-470">`RootNamespace` 是建置時間值，無法用於執行處理序。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-470">`RootNamespace` is a build-time value which is not available to the executing process.</span></span> 

<span data-ttu-id="f6f5f-471">如果 `RootNamespace` 與 `AssemblyName` 不同，請將下列內容納入 *AssemblyInfo.cs* (參數值取代為實際值)：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-471">If the `RootNamespace` is different from the `AssemblyName`, include the following in *AssemblyInfo.cs* (with parameter values replaced with the actual values):</span></span>

```csharp
using System.Reflection;
using Microsoft.Extensions.Localization;

[assembly: ResourceLocation("Resource Folder Name")]
[assembly: RootNamespace("App Root Namespace")]
```

<span data-ttu-id="f6f5f-472">上述程式碼可成功解析 resx 檔案。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-472">The preceding code enables the successful resolution of resx files.</span></span>

## <a name="culture-fallback-behavior"></a><span data-ttu-id="f6f5f-473">文化特性後援行為</span><span class="sxs-lookup"><span data-stu-id="f6f5f-473">Culture fallback behavior</span></span>

<span data-ttu-id="f6f5f-474">搜尋資源時，當地語系化會使用「文化特性後援」。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-474">When searching for a resource, localization engages in "culture fallback".</span></span> <span data-ttu-id="f6f5f-475">從所要求的文化特性開始，如果找不到，就會還原成該文化特性的父文化特性。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-475">Starting from the requested culture, if not found, it reverts to the parent culture of that culture.</span></span> <span data-ttu-id="f6f5f-476">另外，[CultureInfo.Parent](/dotnet/api/system.globalization.cultureinfo.parent) 屬性代表父文化特性。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-476">As an aside, the [CultureInfo.Parent](/dotnet/api/system.globalization.cultureinfo.parent) property represents the parent culture.</span></span> <span data-ttu-id="f6f5f-477">這通常 (但並非一定) 表示從 ISO 中移除國家意符 (Signifier)。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-477">This usually (but not always) means removing the national signifier from the ISO.</span></span> <span data-ttu-id="f6f5f-478">例如，墨西哥的西班牙文方言是 "es-MX"。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-478">For example, the dialect of Spanish spoken in Mexico is "es-MX".</span></span> <span data-ttu-id="f6f5f-479">它具有父系 "es" &mdash; 西班牙文不是任何國家/地區的特定項目。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-479">It has the parent "es"&mdash;Spanish non-specific to any country.</span></span>

<span data-ttu-id="f6f5f-480">假設您的網站收到使用文化特性 "fr-CA" 之 "Welcome" 資源的要求。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-480">Imagine your site receives a request for a "Welcome" resource using culture "fr-CA".</span></span> <span data-ttu-id="f6f5f-481">當地語系化系統會依照順序尋找下列資源，並選取第一個相符項目：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-481">The localization system looks for the following resources, in order, and selects the first match:</span></span>

* <span data-ttu-id="f6f5f-482">*Welcome.fr-CA.resx*</span><span class="sxs-lookup"><span data-stu-id="f6f5f-482">*Welcome.fr-CA.resx*</span></span>
* <span data-ttu-id="f6f5f-483">*Welcome.fr.resx*</span><span class="sxs-lookup"><span data-stu-id="f6f5f-483">*Welcome.fr.resx*</span></span>
* <span data-ttu-id="f6f5f-484">*Welcome.resx* (如果 `NeutralResourcesLanguage` 是 "fr-CA")</span><span class="sxs-lookup"><span data-stu-id="f6f5f-484">*Welcome.resx* (if the `NeutralResourcesLanguage` is "fr-CA")</span></span>

<span data-ttu-id="f6f5f-485">舉例來說，如果您移除 ".fr" 文化特性指示項，並將文化特性設定為法文，則系統會讀取預設資源檔，並將字串當地語系化。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-485">As an example, if you remove the ".fr" culture designator and you have the culture set to French, the default resource file is read and strings are localized.</span></span> <span data-ttu-id="f6f5f-486">當沒有任何項目符合您要求的文化特性時，資源管理員即會指定預設資源或後援資源。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-486">The Resource manager designates a default or fallback resource for when nothing meets your requested culture.</span></span> <span data-ttu-id="f6f5f-487">如果您只想在要求的文化特性缺少資源時傳回索引鍵，就不能使用預設資源檔。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-487">If you want to just return the key when missing a resource for the requested culture you must not have a default resource file.</span></span>

### <a name="generate-resource-files-with-visual-studio"></a><span data-ttu-id="f6f5f-488">使用 Visual Studio 產生資源檔</span><span class="sxs-lookup"><span data-stu-id="f6f5f-488">Generate resource files with Visual Studio</span></span>

<span data-ttu-id="f6f5f-489">如果您在 Visual Studio 中建立資源檔，但檔案名稱中不含文化特性 (例如 *Welcome.resx*)，則 Visual Studio 會針對每個字串的屬性建立 C# 類別。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-489">If you create a resource file in Visual Studio without a culture in the file name (for example, *Welcome.resx*), Visual Studio will create a C# class with a property for each string.</span></span> <span data-ttu-id="f6f5f-490">但這通常不是您使用 ASP.NET Core 的初衷。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-490">That's usually not what you want with ASP.NET Core.</span></span> <span data-ttu-id="f6f5f-491">一般來說，您不會有預設的 *.resx* 資源檔 (不含文化特性名稱的 *.resx* 檔案)。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-491">You typically don't have a default *.resx* resource file (a *.resx* file without the culture name).</span></span> <span data-ttu-id="f6f5f-492">因此，建議您建立含有文化特性名稱的 *.resx* 檔案 (例如 *Welcome.fr.resx*)。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-492">We suggest you create the *.resx* file with a culture name (for example *Welcome.fr.resx*).</span></span> <span data-ttu-id="f6f5f-493">當您建立含有文化特性名稱的 *.resx* 檔案時，Visual Studio 就不會產生類別檔案。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-493">When you create a *.resx* file with a culture name, Visual Studio won't generate the class file.</span></span>

### <a name="add-other-cultures"></a><span data-ttu-id="f6f5f-494">新增其他文化特性</span><span class="sxs-lookup"><span data-stu-id="f6f5f-494">Add other cultures</span></span>

<span data-ttu-id="f6f5f-495">每種語言和文化特性的組合 (非預設語言) 都需要唯一的資源檔。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-495">Each language and culture combination (other than the default language) requires a unique resource file.</span></span> <span data-ttu-id="f6f5f-496">若要建立不同文化特性和地區設定的資源檔，您可以建立新的資源檔並將 ISO 語言代碼作為檔名的一部分 (例如 **en-us**、**fr-ca** 和 **en-gb**)。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-496">You create resource files for different cultures and locales by creating new resource files in which the ISO language codes are part of the file name (for example, **en-us**, **fr-ca**, and **en-gb**).</span></span> <span data-ttu-id="f6f5f-497">您應將 ISO 代碼置於檔案名稱和 *.resx* 副檔名之間，例如 *Welcome.es-MX.resx* (西班牙文/墨西哥)。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-497">These ISO codes are placed between the file name and the *.resx* file extension, as in *Welcome.es-MX.resx* (Spanish/Mexico).</span></span>

## <a name="implement-a-strategy-to-select-the-languageculture-for-each-request"></a><span data-ttu-id="f6f5f-498">實作可依據每項要求選取語言/文化特性的策略</span><span class="sxs-lookup"><span data-stu-id="f6f5f-498">Implement a strategy to select the language/culture for each request</span></span>

### <a name="configure-localization"></a><span data-ttu-id="f6f5f-499">設定當地語系化</span><span class="sxs-lookup"><span data-stu-id="f6f5f-499">Configure localization</span></span>

<span data-ttu-id="f6f5f-500">您可以在 `Startup.ConfigureServices` 方法中設定當地語系化：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-500">Localization is configured in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](localization/sample/3.x/Localization/Startup.cs?name=snippet1)]

* <span data-ttu-id="f6f5f-501">`AddLocalization` 可將當地語系化服務新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-501">`AddLocalization` Adds the localization services to the services container.</span></span> <span data-ttu-id="f6f5f-502">上方的程式碼也會將資源路徑設為 "Resources"。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-502">The code above also sets the resources path to "Resources".</span></span>

* <span data-ttu-id="f6f5f-503">`AddViewLocalization` 可支援當地語系化的檢視檔案。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-503">`AddViewLocalization` Adds support for localized view files.</span></span> <span data-ttu-id="f6f5f-504">在此範例中，檢視的當地語系化會以檢視檔案的後置詞為依據。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-504">In this sample view localization is based on the view file suffix.</span></span> <span data-ttu-id="f6f5f-505">例如 *Index.fr.cshtml* 檔案中的 "fr"。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-505">For example "fr" in the *Index.fr.cshtml* file.</span></span>

* <span data-ttu-id="f6f5f-506">`AddDataAnnotationsLocalization` 可支援透過 `IStringLocalizer` 抽象概念而來的當地語系化 `DataAnnotations` 驗證訊息。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-506">`AddDataAnnotationsLocalization` Adds support for localized `DataAnnotations` validation messages through `IStringLocalizer` abstractions.</span></span>

### <a name="localization-middleware"></a><span data-ttu-id="f6f5f-507">當地語系化中介軟體</span><span class="sxs-lookup"><span data-stu-id="f6f5f-507">Localization middleware</span></span>

<span data-ttu-id="f6f5f-508">您可以在當地語系化[中介軟體](xref:fundamentals/middleware/index)中，設定要求目前的文化特性。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-508">The current culture on a request is set in the localization [Middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="f6f5f-509">已在 `Startup.Configure` 方法中啟用當地語系化中介軟體。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-509">The localization middleware is enabled in the `Startup.Configure` method.</span></span> <span data-ttu-id="f6f5f-510">您必須在可能檢查要求文化特性的任何中介軟體之前，設定當地語系化中介軟體 (例如 `app.UseMvcWithDefaultRoute()`)。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-510">The localization middleware must be configured before any middleware which might check the request culture (for example, `app.UseMvcWithDefaultRoute()`).</span></span>

[!code-csharp[](localization/sample/3.x/Localization/Startup.cs?name=snippet2)]
[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

<span data-ttu-id="f6f5f-511">`UseRequestLocalization` 會初始化 `RequestLocalizationOptions` 物件。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-511">`UseRequestLocalization` initializes a `RequestLocalizationOptions` object.</span></span> <span data-ttu-id="f6f5f-512">在每次要求時，系統會列舉 `RequestLocalizationOptions` 中的 `RequestCultureProvider` 清單，並使用能成功判斷要求的文化特性的第一個提供者。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-512">On every request the list of `RequestCultureProvider` in the `RequestLocalizationOptions` is enumerated and the first provider that can successfully determine the request culture is used.</span></span> <span data-ttu-id="f6f5f-513">預設的提供者是來自 `RequestLocalizationOptions` 類別：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-513">The default providers come from the `RequestLocalizationOptions` class:</span></span>

1. `QueryStringRequestCultureProvider`
2. `CookieRequestCultureProvider`
3. `AcceptLanguageHeaderRequestCultureProvider`

<span data-ttu-id="f6f5f-514">預設清單會由針對性高到低來排列。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-514">The default list goes from most specific to least specific.</span></span> <span data-ttu-id="f6f5f-515">在本文稍後，我們會說明如何變更順序，甚至新增自訂的文化特性提供者。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-515">Later in the article we'll see how you can change the order and even add a custom culture provider.</span></span> <span data-ttu-id="f6f5f-516">如果沒有任何提供者可以判斷要求的文化特性，即會使用 `DefaultRequestCulture`。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-516">If none of the providers can determine the request culture, the `DefaultRequestCulture` is used.</span></span>

### <a name="querystringrequestcultureprovider"></a><span data-ttu-id="f6f5f-517">QueryStringRequestCultureProvider</span><span class="sxs-lookup"><span data-stu-id="f6f5f-517">QueryStringRequestCultureProvider</span></span>

<span data-ttu-id="f6f5f-518">有些應用程式會使用查詢字串來設定[文化特性和 UI 文化特性](https://msdn.microsoft.com/library/system.globalization.cultureinfo.aspx)。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-518">Some apps will use a query string to set the [culture and UI culture](https://msdn.microsoft.com/library/system.globalization.cultureinfo.aspx).</span></span> <span data-ttu-id="f6f5f-519">若是使用 Cookie 或 Accept-Language 標頭方法的應用程式，您可以將查詢字串新增至 URL 以偵錯和測試程式碼。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-519">For apps that use the cookie or Accept-Language header approach, adding a query string to the URL is useful for debugging and testing code.</span></span> <span data-ttu-id="f6f5f-520">系統預設會將 `QueryStringRequestCultureProvider` 登錄為 `RequestCultureProvider` 清單中的第一個當地語系化提供者。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-520">By default, the `QueryStringRequestCultureProvider` is registered as the first localization provider in the `RequestCultureProvider` list.</span></span> <span data-ttu-id="f6f5f-521">您應傳遞查詢字串參數 `culture` 和 `ui-culture`。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-521">You pass the query string parameters `culture` and `ui-culture`.</span></span> <span data-ttu-id="f6f5f-522">下列範例會設定西班牙文/墨西哥的特定文化特性 (語言和地區)：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-522">The following example sets the specific culture (language and region) to Spanish/Mexico:</span></span>

   `http://localhost:5000/?culture=es-MX&ui-culture=es-MX`

<span data-ttu-id="f6f5f-523">如果您只傳入兩者其一 (`culture` 或 `ui-culture`)，查詢字串提供者就會使用您傳入的項目來設定這兩個值。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-523">If you only pass in one of the two (`culture` or `ui-culture`), the query string provider will set both values using the one you passed in.</span></span> <span data-ttu-id="f6f5f-524">例如，若只設定文化特性，即會同時設定 `Culture` 和 `UICulture`：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-524">For example, setting just the culture will set both the `Culture` and the `UICulture`:</span></span>

   `http://localhost:5000/?culture=es-MX`

### <a name="cookierequestcultureprovider"></a><span data-ttu-id="f6f5f-525">CookieRequestCultureProvider</span><span class="sxs-lookup"><span data-stu-id="f6f5f-525">CookieRequestCultureProvider</span></span>

<span data-ttu-id="f6f5f-526">生產環境應用程式通常會提供一個機制，來設定 ASP.NET Core 文化特性 Cookie 的文化特性。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-526">Production apps will often provide a mechanism to set the culture with the ASP.NET Core culture cookie.</span></span> <span data-ttu-id="f6f5f-527">若要建立 Cookie，請使用 `MakeCookieValue` 方法。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-527">Use the `MakeCookieValue` method to create a cookie.</span></span>

<span data-ttu-id="f6f5f-528">會傳回 `CookieRequestCultureProvider` `DefaultCookieName` 用來追蹤使用者慣用文化特性資訊的預設 cookie 名稱。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-528">The `CookieRequestCultureProvider` `DefaultCookieName` returns the default cookie name used to track the user's preferred culture information.</span></span> <span data-ttu-id="f6f5f-529">預設 Cookie 名稱為 `.AspNetCore.Culture`。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-529">The default cookie name is `.AspNetCore.Culture`.</span></span>

<span data-ttu-id="f6f5f-530">Cookie 格式為 `c=%LANGCODE%|uic=%LANGCODE%`，其中 `c` 是 `Culture` 而 `uic` 是 `UICulture`，例如：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-530">The cookie format is `c=%LANGCODE%|uic=%LANGCODE%`, where `c` is `Culture` and `uic` is `UICulture`, for example:</span></span>

    c=en-UK|uic=en-US

<span data-ttu-id="f6f5f-531">如果您只指定文化特性資訊和 UI 文化特性其一，系統就會將您所指定的文化特性用於文化特性資訊和 UI 文化特性。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-531">If you only specify one of culture info and UI culture, the specified culture will be used for both culture info and UI culture.</span></span>

### <a name="the-accept-language-http-header"></a><span data-ttu-id="f6f5f-532">Accept-Language HTTP 標頭</span><span class="sxs-lookup"><span data-stu-id="f6f5f-532">The Accept-Language HTTP header</span></span>

<span data-ttu-id="f6f5f-533">您可在大多數的瀏覽器中設定 [Accept-Language 標頭](https://www.w3.org/International/questions/qa-accept-lang-locales)，其最初的設計目的是用來指定使用者的語言。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-533">The [Accept-Language header](https://www.w3.org/International/questions/qa-accept-lang-locales) is settable in most browsers and was originally intended to specify the user's language.</span></span> <span data-ttu-id="f6f5f-534">這項設定可指出瀏覽器已設好要傳送哪些項目，或已從基礎作業系統繼承哪些項目。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-534">This setting indicates what the browser has been set to send or has inherited from the underlying operating system.</span></span> <span data-ttu-id="f6f5f-535">透過瀏覽器要求的 Accept-Language HTTP 標頭來偵測使用者的慣用語言，並非萬無一失 (請參閱 [Setting language preferences in a browser](https://www.w3.org/International/questions/qa-lang-priorities.en.php) (在瀏覽器中設定語言喜好設定)。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-535">The Accept-Language HTTP header from a browser request isn't an infallible way to detect the user's preferred language (see [Setting language preferences in a browser](https://www.w3.org/International/questions/qa-lang-priorities.en.php)).</span></span> <span data-ttu-id="f6f5f-536">生產環境應用程式應該包含可讓使用者自訂文化特性的選擇方式。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-536">A production app should include a way for a user to customize their choice of culture.</span></span>

### <a name="set-the-accept-language-http-header-in-ie"></a><span data-ttu-id="f6f5f-537">在 IE 中設定 Accept-Language HTTP 標頭</span><span class="sxs-lookup"><span data-stu-id="f6f5f-537">Set the Accept-Language HTTP header in IE</span></span>

1. <span data-ttu-id="f6f5f-538">從齒輪圖示，點選 [網際網路選項]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-538">From the gear icon, tap **Internet Options**.</span></span>

2. <span data-ttu-id="f6f5f-539">點選 [語言]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-539">Tap **Languages**.</span></span>

    ![網際網路選項](localization/_static/lang.png)

3. <span data-ttu-id="f6f5f-541">點選 [設定語言喜好設定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-541">Tap **Set Language Preferences**.</span></span>

4. <span data-ttu-id="f6f5f-542">點選 [新增語言]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-542">Tap **Add a language**.</span></span>

5. <span data-ttu-id="f6f5f-543">新增語言。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-543">Add the language.</span></span>

6. <span data-ttu-id="f6f5f-544">點選語言，然後點選 [上移]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-544">Tap the language, then tap **Move Up**.</span></span>

### <a name="use-a-custom-provider"></a><span data-ttu-id="f6f5f-545">使用自訂提供者</span><span class="sxs-lookup"><span data-stu-id="f6f5f-545">Use a custom provider</span></span>

<span data-ttu-id="f6f5f-546">假設您想要讓客戶將他們的語言和文化特性儲存在您的資料庫中。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-546">Suppose you want to let your customers store their language and culture in your databases.</span></span> <span data-ttu-id="f6f5f-547">您可以撰寫提供者，以供使用者查詢這些值。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-547">You could write a provider to look up these values for the user.</span></span> <span data-ttu-id="f6f5f-548">下列程式碼會示範如何新增自訂提供者：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-548">The following code shows how to add a custom provider:</span></span>

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

<span data-ttu-id="f6f5f-549">使用 `RequestLocalizationOptions` 新增或移除當地語系化提供者。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-549">Use `RequestLocalizationOptions` to add or remove localization providers.</span></span>

### <a name="set-the-culture-programmatically"></a><span data-ttu-id="f6f5f-550">以程式設計方式來設定文化特性</span><span class="sxs-lookup"><span data-stu-id="f6f5f-550">Set the culture programmatically</span></span>

<span data-ttu-id="f6f5f-551">[GitHub](https://github.com/aspnet/entropy) 上的這個範例 **Localization.StarterWeb** 專案包含可設定 `Culture` 的 UI。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-551">This sample **Localization.StarterWeb** project on [GitHub](https://github.com/aspnet/entropy) contains UI to set the `Culture`.</span></span> <span data-ttu-id="f6f5f-552">*Views/Shared/_SelectLanguagePartial.cshtml* 檔可讓您從支援的文化特性清單中選取文化特性：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-552">The *Views/Shared/_SelectLanguagePartial.cshtml* file allows you to select the culture from the list of supported cultures:</span></span>

[!code-cshtml[](localization/sample/3.x/Localization/Views/Shared/_SelectLanguagePartial.cshtml)]

<span data-ttu-id="f6f5f-553">系統會將 *Views/Shared/_SelectLanguagePartial.cshtml* 檔案新增至配置檔案的 `footer` 區段，以供所有檢視使用：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-553">The *Views/Shared/_SelectLanguagePartial.cshtml* file is added to the `footer` section of the layout file so it will be available to all views:</span></span>

[!code-cshtml[](localization/sample/3.x/Localization/Views/Shared/_Layout.cshtml?range=43-56&highlight=10)]

<span data-ttu-id="f6f5f-554">`SetLanguage` 方法會設定文化特性的 Cookie。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-554">The `SetLanguage` method sets the culture cookie.</span></span>

[!code-csharp[](localization/sample/3.x/Localization/Controllers/HomeController.cs?range=57-67)]

<span data-ttu-id="f6f5f-555">您無法將 *_SelectLanguagePartial.cshtml* 插入這個專案的範例程式碼。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-555">You can't plug in the *_SelectLanguagePartial.cshtml* to sample code for this project.</span></span> <span data-ttu-id="f6f5f-556">[GitHub](https://github.com/aspnet/entropy)上的**localization.starterweb**專案具有程式碼，可透過相依性 `RequestLocalizationOptions` 插入容器將傳遞至 Razor 部分。 [Dependency Injection](dependency-injection.md)</span><span class="sxs-lookup"><span data-stu-id="f6f5f-556">The **Localization.StarterWeb** project on [GitHub](https://github.com/aspnet/entropy) has code to flow the `RequestLocalizationOptions` to a Razor partial through the [Dependency Injection](dependency-injection.md) container.</span></span>

## <a name="model-binding-route-data-and-query-strings"></a><span data-ttu-id="f6f5f-557">模型系結路由資料和查詢字串</span><span class="sxs-lookup"><span data-stu-id="f6f5f-557">Model binding route data and query strings</span></span>

<span data-ttu-id="f6f5f-558">請參閱模型系結[路由資料和查詢字串的全球化行為](xref:mvc/models/model-binding#glob)。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-558">See [Globalization behavior of model binding route data and query strings](xref:mvc/models/model-binding#glob).</span></span>

## <a name="globalization-and-localization-terms"></a><span data-ttu-id="f6f5f-559">全球化和當地語系化詞彙</span><span class="sxs-lookup"><span data-stu-id="f6f5f-559">Globalization and localization terms</span></span>

<span data-ttu-id="f6f5f-560">在進行應用程式的當地語系化程序時，您也需要具備現代軟體開發中常用的相關字元集基本知識，並了解與它們建立關聯的問題。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-560">The process of localizing your app also requires a basic understanding of relevant character sets commonly used in modern software development and an understanding of the issues associated with them.</span></span> <span data-ttu-id="f6f5f-561">雖然所有電腦都會將文字儲存為數字 (程式碼)，但不同系統會使用不同數字來儲存相同的文字。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-561">Although all computers store text as numbers (codes), different systems store the same text using different numbers.</span></span> <span data-ttu-id="f6f5f-562">當地語系化是指針對特定文化特性/地區設定，轉譯應用程式使用者介面 (UI) 的程序。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-562">The localization process refers to translating the app user interface (UI) for a specific culture/locale.</span></span>

<span data-ttu-id="f6f5f-563">[可當地語系化](/dotnet/standard/globalization-localization/localizability-review)是確認全球化應用程式已準備好進行當地語系化的中繼程序。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-563">[Localizability](/dotnet/standard/globalization-localization/localizability-review) is an intermediate process for verifying that a globalized app is ready for localization.</span></span>

<span data-ttu-id="f6f5f-564">文化特性名稱的 [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) 格式是 `<languagecode2>-<country/regioncode2>`，其中 `<languagecode2>` 是語言代碼，而 `<country/regioncode2>` 是子文化特性代碼。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-564">The [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) format for the culture name is `<languagecode2>-<country/regioncode2>`, where `<languagecode2>` is the language code and `<country/regioncode2>` is the subculture code.</span></span> <span data-ttu-id="f6f5f-565">例如，`es-CL` 是指西班牙文 (智利)，`en-US` 是指英文 (美國)，而 `en-AU` 是指英文 (澳大利亞)。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-565">For example, `es-CL` for Spanish (Chile), `en-US` for English (United States), and `en-AU` for English (Australia).</span></span> <span data-ttu-id="f6f5f-566">[RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) 是 ISO 639 (兩個小寫字母，為與某個語言建立關聯的文化特性代碼) 及 ISO 3166 (兩個大寫字母，為與某個國家或地區建立關聯的子文化特性代碼) 的組合。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-566">[RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) is a combination of an ISO 639 two-letter lowercase culture code associated with a language and an ISO 3166 two-letter uppercase subculture code associated with a country or region.</span></span> <span data-ttu-id="f6f5f-567">請參閱 [Language Culture Name](https://msdn.microsoft.com/library/ee825488(v=cs.20).aspx) (語言的文化特性名稱)。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-567">See [Language Culture Name](https://msdn.microsoft.com/library/ee825488(v=cs.20).aspx).</span></span>

<span data-ttu-id="f6f5f-568">國際化通常縮寫為 "I18N"。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-568">Internationalization is often abbreviated to "I18N".</span></span> <span data-ttu-id="f6f5f-569">這個縮寫是採用該詞彙的第一個和最後一個字母，以及這兩個字母間的字母數組成，因此 18 代表第一個字母 "I" 及最後 "N" 中間的字母數。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-569">The abbreviation takes the first and last letters and the number of letters between them, so 18 stands for the number of letters between the first "I" and the last "N".</span></span> <span data-ttu-id="f6f5f-570">同樣的原則也適用於全球化 (G11N) 與當地語系化 (L10N) 的縮寫。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-570">The same applies to Globalization (G11N), and Localization (L10N).</span></span>

<span data-ttu-id="f6f5f-571">詞彙：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-571">Terms:</span></span>

* <span data-ttu-id="f6f5f-572">全球化 (G11N)：讓應用程式支援不同語言和區域的程序。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-572">Globalization (G11N): The process of making an app support different languages and regions.</span></span>
* <span data-ttu-id="f6f5f-573">當地語系化 (L10N)：針對特定語言和區域，自訂應用程式的程序。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-573">Localization (L10N): The process of customizing an app for a given language and region.</span></span>
* <span data-ttu-id="f6f5f-574">國際化 (I18N)：包含全球化和當地語系化這兩部分的描述。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-574">Internationalization (I18N): Describes both globalization and localization.</span></span>
* <span data-ttu-id="f6f5f-575">文化特性：它是一種語言和/或地區。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-575">Culture: It's a language and, optionally, a region.</span></span>
* <span data-ttu-id="f6f5f-576">中性文化特性：具有指定的語言但不限地區的文化特性 </span><span class="sxs-lookup"><span data-stu-id="f6f5f-576">Neutral culture: A culture that has a specified language, but not a region.</span></span> <span data-ttu-id="f6f5f-577">(例如 "en"、"es")。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-577">(for example "en", "es")</span></span>
* <span data-ttu-id="f6f5f-578">特定文化特性：具有指定的語言和地區的文化特性 </span><span class="sxs-lookup"><span data-stu-id="f6f5f-578">Specific culture: A culture that has a specified language and region.</span></span> <span data-ttu-id="f6f5f-579">(例如 "en-US"、"en-GB"、"es-CL")。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-579">(for example "en-US", "en-GB", "es-CL")</span></span>
* <span data-ttu-id="f6f5f-580">父文化特性：包含特定文化特性的中性文化特性 </span><span class="sxs-lookup"><span data-stu-id="f6f5f-580">Parent culture: The neutral culture that contains a specific culture.</span></span> <span data-ttu-id="f6f5f-581">(例如，"en" 是 "en-US" 和 "en-GB" 的父文化特性)。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-581">(for example, "en" is the parent culture of "en-US" and "en-GB")</span></span>
* <span data-ttu-id="f6f5f-582">地區設定：地區設定與文化特性相同。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-582">Locale: A locale is the same as a culture.</span></span>

[!INCLUDE[](~/includes/localization/currency.md)]

## <a name="additional-resources"></a><span data-ttu-id="f6f5f-583">其他資源</span><span class="sxs-lookup"><span data-stu-id="f6f5f-583">Additional resources</span></span>

* <xref:fundamentals/troubleshoot-aspnet-core-localization>
* <span data-ttu-id="f6f5f-584">本文使用的 [Localization.StarterWeb 專案](https://github.com/aspnet/Entropy/tree/master/samples/Localization.StarterWeb)。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-584">[Localization.StarterWeb project](https://github.com/aspnet/Entropy/tree/master/samples/Localization.StarterWeb) used in the article.</span></span>
* [<span data-ttu-id="f6f5f-585">全球化與當地語系化 .NET 應用程式</span><span class="sxs-lookup"><span data-stu-id="f6f5f-585">Globalizing and localizing .NET applications</span></span>](/dotnet/standard/globalization-localization/index)
* [<span data-ttu-id="f6f5f-586">.Resx 檔案中的資源</span><span class="sxs-lookup"><span data-stu-id="f6f5f-586">Resources in .resx Files</span></span>](/dotnet/framework/resources/working-with-resx-files-programmatically)
* [<span data-ttu-id="f6f5f-587">Microsoft 多語應用程式工具組</span><span class="sxs-lookup"><span data-stu-id="f6f5f-587">Microsoft Multilingual App Toolkit</span></span>](https://marketplace.visualstudio.com/items?itemName=MultilingualAppToolkit.MultilingualAppToolkit-18308)
* [<span data-ttu-id="f6f5f-588">當地語系化和泛型</span><span class="sxs-lookup"><span data-stu-id="f6f5f-588">Localization & Generics</span></span>](http://hishambinateya.com/localization-and-generics)

::: moniker-end

<!-- ASP.NET Core 5.x starts here -->
::: moniker range="> aspnetcore-3.1"

<span data-ttu-id="f6f5f-589">由 [Rick Anderson](https://twitter.com/RickAndMSFT)、[Damien Bowden](https://twitter.com/damien_bod)、[Bart Calixto](https://twitter.com/bartmax)、[Nadeem Afana](https://afana.me/) 和 [Hisham Bin Ateya](https://twitter.com/hishambinateya) 提供</span><span class="sxs-lookup"><span data-stu-id="f6f5f-589">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Damien Bowden](https://twitter.com/damien_bod), [Bart Calixto](https://twitter.com/bartmax), [Nadeem Afana](https://afana.me/), and [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span></span>

<span data-ttu-id="f6f5f-590">多語系網站可讓網站觸及更多物件。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-590">A multilingual website allows the site to reach a wider audience.</span></span> <span data-ttu-id="f6f5f-591">ASP.NET Core 提供服務與中介軟體，可將網站當地語系化成不同的語言與文化特性。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-591">ASP.NET Core provides services and middleware for localizing into different languages and cultures.</span></span>

<span data-ttu-id="f6f5f-592">國際化包含[全球化](/dotnet/api/system.globalization)和[當地語系化](/dotnet/standard/globalization-localization/localization)這兩部分。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-592">Internationalization involves [Globalization](/dotnet/api/system.globalization) and [Localization](/dotnet/standard/globalization-localization/localization).</span></span> <span data-ttu-id="f6f5f-593">全球化是指設計出可支援不同文化特性之應用程式的程序。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-593">Globalization is the process of designing apps that support different cultures.</span></span> <span data-ttu-id="f6f5f-594">透過全球化，您可新增支援與特定地區相關之已定義語言指令碼的輸入、顯示及輸出作業。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-594">Globalization adds support for input, display, and output of a defined set of language scripts that relate to specific geographic areas.</span></span>

<span data-ttu-id="f6f5f-595">當地語系化是指針對全球化應用程式進行調整的程序，且您已順應特定文化特性/地區設定對這些全球化應用程式進行可當地語系化處理。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-595">Localization is the process of adapting a globalized app, which you have already processed for localizability, to a particular culture/locale.</span></span> <span data-ttu-id="f6f5f-596">如需詳細資訊，請參閱本文件結尾處的**全球化和當地語系化詞彙**。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-596">For more information see **Globalization and localization terms** near the end of this document.</span></span>

<span data-ttu-id="f6f5f-597">應用程式當地語系化包含下列作業：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-597">App localization involves the following:</span></span>

1. <span data-ttu-id="f6f5f-598">讓應用程式的內容可當地語系化</span><span class="sxs-lookup"><span data-stu-id="f6f5f-598">Make the app's content localizable</span></span>
1. <span data-ttu-id="f6f5f-599">針對您支援的語言和文化特性提供當地語系化資源</span><span class="sxs-lookup"><span data-stu-id="f6f5f-599">Provide localized resources for the languages and cultures you support</span></span>
1. <span data-ttu-id="f6f5f-600">實作可依據每項要求選取語言/文化特性的策略</span><span class="sxs-lookup"><span data-stu-id="f6f5f-600">Implement a strategy to select the language/culture for each request</span></span>

<span data-ttu-id="f6f5f-601">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/localization/sample/2.x/)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="f6f5f-601">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/localization/sample/2.x/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="make-the-apps-content-localizable"></a><span data-ttu-id="f6f5f-602">讓應用程式的內容可當地語系化</span><span class="sxs-lookup"><span data-stu-id="f6f5f-602">Make the app's content localizable</span></span>

<span data-ttu-id="f6f5f-603"><xref:Microsoft.Extensions.Localization.IStringLocalizer>和 <xref:Microsoft.Extensions.Localization.IStringLocalizer%601> 已架構，可在開發當地語系化應用程式時改善生產力。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-603"><xref:Microsoft.Extensions.Localization.IStringLocalizer> and <xref:Microsoft.Extensions.Localization.IStringLocalizer%601> were architected to improve productivity when developing localized apps.</span></span> <span data-ttu-id="f6f5f-604">`IStringLocalizer`會使用 <xref:System.Resources.ResourceManager> 和， <xref:System.Resources.ResourceReader> 在執行時間提供特定文化特性的資源。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-604">`IStringLocalizer` uses the <xref:System.Resources.ResourceManager> and <xref:System.Resources.ResourceReader> to provide culture-specific resources at run time.</span></span> <span data-ttu-id="f6f5f-605">介面具有索引子和， `IEnumerable` 用於傳回當地語系化的字串。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-605">The interface has an indexer and an `IEnumerable` for returning localized strings.</span></span> <span data-ttu-id="f6f5f-606">`IStringLocalizer`不需要將預設語言字串儲存在資源檔中。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-606">`IStringLocalizer` doesn't require storing the default language strings in a resource file.</span></span> <span data-ttu-id="f6f5f-607">您不必在開發初期建立資源檔，即可開發以當地語系化為目標的應用程式。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-607">You can develop an app targeted for localization and not need to create resource files early in development.</span></span> <span data-ttu-id="f6f5f-608">下列程式碼會示範如何包裝 "About Title" 字串以進行當地語系化。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-608">The code below shows how to wrap the string "About Title" for localization.</span></span>

[!code-csharp[](localization/sample/3.x/Localization/Controllers/AboutController.cs)]

<span data-ttu-id="f6f5f-609">在上述程式碼中， `IStringLocalizer<T>` 執行是來自相依性[插入](dependency-injection.md)。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-609">In the preceding code, the `IStringLocalizer<T>` implementation comes from [Dependency Injection](dependency-injection.md).</span></span> <span data-ttu-id="f6f5f-610">如果找不到 "About Title" 的當地語系化值，即會傳回索引子的索引鍵，也就是 "About Title" 字串。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-610">If the localized value of "About Title" isn't found, then the indexer key is returned, that is, the string "About Title".</span></span> <span data-ttu-id="f6f5f-611">您可以保留應用程式中的預設語言常值字串，並將其包裝在當地語系化工具中，以便專注於開發應用程式。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-611">You can leave the default language literal strings in the app and wrap them in the localizer, so that you can focus on developing the app.</span></span> <span data-ttu-id="f6f5f-612">您不用先建立預設資源檔，即可使用預設語言來開發應用程式，並針對當地語系化步驟進行應用程式的準備。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-612">You develop your app with your default language and prepare it for the localization step without first creating a default resource file.</span></span> <span data-ttu-id="f6f5f-613">或者，您可以使用傳統方法，並提供索引鍵以擷取預設語言字串。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-613">Alternatively, you can use the traditional approach and provide a key to retrieve the default language string.</span></span> <span data-ttu-id="f6f5f-614">對許多開發人員來說，新的工作流程 (單純包裝字串常值而不使用預設語言的 *.resx* 檔案) 可以降低當地語系化應用程式的額外負荷。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-614">For many developers the new workflow of not having a default language *.resx* file and simply wrapping the string literals can reduce the overhead of localizing an app.</span></span> <span data-ttu-id="f6f5f-615">其他開發人員則偏好傳統的工作流程，因為這種方法更便於使用較長的字串常值，且更易於更新當地語系化的字串。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-615">Other developers will prefer the traditional work flow as it can make it easier to work with longer string literals and make it easier to update localized strings.</span></span>

<span data-ttu-id="f6f5f-616">若是包含 HTML 的資源，請使用 `IHtmlLocalizer<T>` 實作。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-616">Use the `IHtmlLocalizer<T>` implementation for resources that contain HTML.</span></span> <span data-ttu-id="f6f5f-617">`IHtmlLocalizer` 會對資源字串中經過格式化的引數進行 HTML 編碼，但不會對資源字串本身進行 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-617">`IHtmlLocalizer` HTML encodes arguments that are formatted in the resource string, but doesn't HTML encode the resource string itself.</span></span> <span data-ttu-id="f6f5f-618">在下列醒目提示的範例中，只有 `name` 參數的值經過 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-618">In the sample highlighted below, only the value of `name` parameter is HTML encoded.</span></span>

[!code-csharp[](~/fundamentals/localization/sample/3.x/Localization/Controllers/BookController.cs?highlight=3,5,20&start=1&end=24)]

<span data-ttu-id="f6f5f-619">**注意：** 一般來說，您只想將文字當地語系化，而不是 HTML。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-619">**Note:** You generally want to only localize text and not HTML.</span></span>

<span data-ttu-id="f6f5f-620">您可以在最底層的[相依性插入](dependency-injection.md)中，將 `IStringLocalizerFactory` 移出：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-620">At the lowest level, you can get `IStringLocalizerFactory` out of [Dependency Injection](dependency-injection.md):</span></span>

[!code-csharp[](localization/sample/3.x/Localization/Controllers/TestController.cs?start=9&end=26&highlight=7-13)]

<span data-ttu-id="f6f5f-621">上述程式碼會示範這兩個 Factory Create 方法。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-621">The code above demonstrates each of the two factory create methods.</span></span>

<span data-ttu-id="f6f5f-622">您可以依據控制器或區域來分割當地語系化的字串，也可以只用一個容器。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-622">You can partition your localized strings by controller, area, or have just one container.</span></span> <span data-ttu-id="f6f5f-623">在範例應用程式中，會針對共用資源使用名為 `SharedResource` 的虛擬類別。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-623">In the sample app, a dummy class named `SharedResource` is used for shared resources.</span></span>

[!code-csharp[](localization/sample/3.x/Localization/Resources/SharedResource.cs)]

<span data-ttu-id="f6f5f-624">有些開發人員會使用 `Startup` 類別來包含全域或共用字串。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-624">Some developers use the `Startup` class to contain global or shared strings.</span></span> <span data-ttu-id="f6f5f-625">下列範例會使用 `InfoController` 和 `SharedResource` 當地語系化工具：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-625">In the sample below, the `InfoController` and the `SharedResource` localizers are used:</span></span>

[!code-csharp[](localization/sample/3.x/Localization/Controllers/InfoController.cs?range=9-26)]

## <a name="view-localization"></a><span data-ttu-id="f6f5f-626">檢視當地語系化</span><span class="sxs-lookup"><span data-stu-id="f6f5f-626">View localization</span></span>

<span data-ttu-id="f6f5f-627">`IViewLocalizer` 服務可提供[檢視](xref:mvc/views/overview)的當地語系化字串。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-627">The `IViewLocalizer` service provides localized strings for a [view](xref:mvc/views/overview).</span></span> <span data-ttu-id="f6f5f-628">`ViewLocalizer` 類別會實作這個介面，並透過檢視檔案路徑來找出資源的位置。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-628">The `ViewLocalizer` class implements this interface and finds the resource location from the view file path.</span></span> <span data-ttu-id="f6f5f-629">下列程式碼示範如何使用 `IViewLocalizer` 的預設實作：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-629">The following code shows how to use the default implementation of `IViewLocalizer`:</span></span>

[!code-cshtml[](localization/sample/3.x/Localization/Views/Home/About.cshtml)]

<span data-ttu-id="f6f5f-630">`IViewLocalizer` 的預設實作會依據檢視的檔案名稱來找出資源檔。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-630">The default implementation of `IViewLocalizer` finds the resource file based on the view's file name.</span></span> <span data-ttu-id="f6f5f-631">其中並沒有任何選項可以使用全域共用的資源檔。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-631">There's no option to use a global shared resource file.</span></span> <span data-ttu-id="f6f5f-632">`ViewLocalizer`使用來執行當地語系化工具 `IHtmlLocalizer` ，因此 Razor 不會對當地語系化字串進行 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-632">`ViewLocalizer` implements the localizer using `IHtmlLocalizer`, so Razor doesn't HTML encode the localized string.</span></span> <span data-ttu-id="f6f5f-633">您可以參數化資源字串，`IViewLocalizer` 即會對參數 (而不是資源字串) 進行 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-633">You can parameterize resource strings and `IViewLocalizer` will HTML encode the parameters, but not the resource string.</span></span> <span data-ttu-id="f6f5f-634">請考慮下列 Razor 標記：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-634">Consider the following Razor markup:</span></span>

```cshtml
@Localizer["<i>Hello</i> <b>{0}!</b>", UserManager.GetUserName(User)]
```

<span data-ttu-id="f6f5f-635">法文資源檔可能包含下列內容：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-635">A French resource file could contain the following:</span></span>

| <span data-ttu-id="f6f5f-636">機碼</span><span class="sxs-lookup"><span data-stu-id="f6f5f-636">Key</span></span> | <span data-ttu-id="f6f5f-637">值</span><span class="sxs-lookup"><span data-stu-id="f6f5f-637">Value</span></span> |
| ----- | ------ |
| `<i>Hello</i> <b>{0}!</b>` | `<i>Bonjour</i> <b>{0} !</b>` |

<span data-ttu-id="f6f5f-638">轉譯的檢視內容可能包含來自資源檔的 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-638">The rendered view would contain the HTML markup from the resource file.</span></span>

<span data-ttu-id="f6f5f-639">**注意：** 一般來說，您只想將文字當地語系化，而不是 HTML。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-639">**Note:** You generally want to only localize text and not HTML.</span></span>

<span data-ttu-id="f6f5f-640">若要在檢視中使用共用的資源檔，請插入 `IHtmlLocalizer<T>`：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-640">To use a shared resource file in a view, inject `IHtmlLocalizer<T>`:</span></span>

[!code-cshtml[](~/fundamentals/localization/sample/3.x/Localization/Views/Test/About.cshtml?highlight=5,12)]

## <a name="dataannotations-localization"></a><span data-ttu-id="f6f5f-641">DataAnnotations 當地語系化</span><span class="sxs-lookup"><span data-stu-id="f6f5f-641">DataAnnotations localization</span></span>

<span data-ttu-id="f6f5f-642">DataAnnotations 錯誤訊息會使用 `IStringLocalizer<T>` 來當地語系化。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-642">DataAnnotations error messages are localized with `IStringLocalizer<T>`.</span></span> <span data-ttu-id="f6f5f-643">使用 `ResourcesPath = "Resources"` 選項時，`RegisterViewModel` 中的錯誤訊息會儲存在下列路徑之一：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-643">Using the option `ResourcesPath = "Resources"`, the error messages in `RegisterViewModel` can be stored in either of the following paths:</span></span>

* <span data-ttu-id="f6f5f-644">*Resources/Viewmodel. RegisterViewModel. fr .resx*</span><span class="sxs-lookup"><span data-stu-id="f6f5f-644">*Resources/ViewModels.Account.RegisterViewModel.fr.resx*</span></span>
* <span data-ttu-id="f6f5f-645">*Resources/ViewModels/Account/RegisterViewModel.fr.resx*</span><span class="sxs-lookup"><span data-stu-id="f6f5f-645">*Resources/ViewModels/Account/RegisterViewModel.fr.resx*</span></span>

[!code-csharp[](localization/sample/3.x/Localization/ViewModels/Account/RegisterViewModel.cs?start=9&end=26)]

<span data-ttu-id="f6f5f-646">在 ASP.NET Core MVC 1.1.0 和更高版本中，系統會將非驗證屬性當地語系化。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-646">In ASP.NET Core MVC 1.1.0 and higher, non-validation attributes are localized.</span></span> <span data-ttu-id="f6f5f-647">ASP.NET Core MVC 1.0 **不會**查閱非驗證屬性的當地語系化字串。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-647">ASP.NET Core MVC 1.0 does **not** look up localized strings for non-validation attributes.</span></span>

<a name="one-resource-string-multiple-classes"></a>

### <a name="using-one-resource-string-for-multiple-classes"></a><span data-ttu-id="f6f5f-648">針對多個類別使用同一個資源字串</span><span class="sxs-lookup"><span data-stu-id="f6f5f-648">Using one resource string for multiple classes</span></span>

<span data-ttu-id="f6f5f-649">下列程式碼會示範如何針對含有多個類別的驗證屬性使用同一個資源字串：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-649">The following code shows how to use one resource string for validation attributes with multiple classes:</span></span>

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

<span data-ttu-id="f6f5f-650">在先前的程式碼中，`SharedResource` 是與儲存驗證訊息的 resx 對應的類別。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-650">In the preceding code, `SharedResource` is the class corresponding to the resx where your validation messages are stored.</span></span> <span data-ttu-id="f6f5f-651">使用這個方法時，DataAnnotations 只會使用 `SharedResource`，而不是每個類別的資源。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-651">With this approach, DataAnnotations will only use `SharedResource`, rather than the resource for each class.</span></span>

## <a name="provide-localized-resources-for-the-languages-and-cultures-you-support"></a><span data-ttu-id="f6f5f-652">針對您支援的語言和文化特性提供當地語系化資源</span><span class="sxs-lookup"><span data-stu-id="f6f5f-652">Provide localized resources for the languages and cultures you support</span></span>

### <a name="supportedcultures-and-supporteduicultures"></a><span data-ttu-id="f6f5f-653">SupportedCultures 和 SupportedUICultures</span><span class="sxs-lookup"><span data-stu-id="f6f5f-653">SupportedCultures and SupportedUICultures</span></span>

<span data-ttu-id="f6f5f-654">ASP.NET Core 可讓您指定 `SupportedCultures` 和 `SupportedUICultures` 這兩個文化特性值。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-654">ASP.NET Core allows you to specify two culture values, `SupportedCultures` and `SupportedUICultures`.</span></span> <span data-ttu-id="f6f5f-655">`SupportedCultures` 的 [CultureInfo](/dotnet/api/system.globalization.cultureinfo) 物件可決定文化特性相依函式的結果，例如日期、時間、數字及貨幣格式。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-655">The [CultureInfo](/dotnet/api/system.globalization.cultureinfo) object for `SupportedCultures` determines the results of culture-dependent functions, such as date, time, number, and currency formatting.</span></span> <span data-ttu-id="f6f5f-656">`SupportedCultures` 也可決定文字排列順序、大小寫慣例和字串比較。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-656">`SupportedCultures` also determines the sorting order of text, casing conventions, and string comparisons.</span></span> <span data-ttu-id="f6f5f-657">如需伺服器如何取得文化特性的詳細資訊，請參閱 [CultureInfo.CurrentCulture](/dotnet/api/system.stringcomparer.currentculture#System_StringComparer_CurrentCulture)。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-657">See [CultureInfo.CurrentCulture](/dotnet/api/system.stringcomparer.currentculture#System_StringComparer_CurrentCulture) for more info on how the server gets the Culture.</span></span> <span data-ttu-id="f6f5f-658">`SupportedUICultures`會決定[ResourceManager](/dotnet/api/system.resources.resourcemanager)會查閱哪些翻譯的字串（來自 *.resx*檔案）。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-658">The `SupportedUICultures` determines which translated strings (from *.resx* files) are looked up by the [ResourceManager](/dotnet/api/system.resources.resourcemanager).</span></span> <span data-ttu-id="f6f5f-659">`ResourceManager` 僅會查閱 `CurrentUICulture` 所決定之文化特性特有的字串。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-659">The `ResourceManager` simply looks up culture-specific strings that's determined by `CurrentUICulture`.</span></span> <span data-ttu-id="f6f5f-660">.NET 中的每個執行緒都有 `CurrentCulture` 和 `CurrentUICulture` 物件。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-660">Every thread in .NET has `CurrentCulture` and `CurrentUICulture` objects.</span></span> <span data-ttu-id="f6f5f-661">ASP.NET Core 會在轉譯文化特性相依函式時檢查這些值。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-661">ASP.NET Core inspects these values when rendering culture-dependent functions.</span></span> <span data-ttu-id="f6f5f-662">比方說，如果目前執行緒的文化特性設定為 "en-US" (英文 - 美國)，`DateTime.Now.ToLongDateString()` 會顯示 "Thursday, February 18, 2016"，但如果 `CurrentCulture` 設定為 "es-ES" (西班牙文 - 西班牙)，則輸出會是 "jueves, 18 de febrero de 2016"。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-662">For example, if the current thread's culture is set to "en-US" (English, United States), `DateTime.Now.ToLongDateString()` displays "Thursday, February 18, 2016", but if `CurrentCulture` is set to "es-ES" (Spanish, Spain) the output will be "jueves, 18 de febrero de 2016".</span></span>

## <a name="resource-files"></a><span data-ttu-id="f6f5f-663">資源檔</span><span class="sxs-lookup"><span data-stu-id="f6f5f-663">Resource files</span></span>

<span data-ttu-id="f6f5f-664">資源檔是一種實用的機制，可讓您將可當地語系化的字串與代碼區隔開來。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-664">A resource file is a useful mechanism for separating localizable strings from code.</span></span> <span data-ttu-id="f6f5f-665">非預設語言的翻譯字串會在 *.resx*資源檔中隔離。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-665">Translated strings for the non-default language are isolated in *.resx* resource files.</span></span> <span data-ttu-id="f6f5f-666">例如，您可以建立名為 *Welcome.es.resx* 的西班牙文資源檔，以包含翻譯的字串。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-666">For example, you might want to create Spanish resource file named *Welcome.es.resx* containing translated strings.</span></span> <span data-ttu-id="f6f5f-667">"es" 是西班牙文的語言代碼。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-667">"es" is the language code for Spanish.</span></span> <span data-ttu-id="f6f5f-668">若要在 Visual Studio 中建立這個資源檔：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-668">To create this resource file in Visual Studio:</span></span>

1. <span data-ttu-id="f6f5f-669">在方案總管\*\*\*\* 中，以滑鼠右鍵按一下要放置資源檔的資料夾 > [新增]**[新增項目]** > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-669">In **Solution Explorer**, right click on the folder which will contain the resource file > **Add** > **New Item**.</span></span>

    ![巢狀特色選單：方案總管會開啟 [資源] 的特色選單，](localization/_static/newi.png)

2. <span data-ttu-id="f6f5f-672">在 [Search installed templates] (搜尋已安裝的範本)\*\*\*\* 方塊中，輸入「資源」，並命名檔案。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-672">In the **Search installed templates** box, enter "resource" and name the file.</span></span>

    ![[新增項目] 對話方塊](localization/_static/res.png)

3. <span data-ttu-id="f6f5f-674">在 [名稱]\*\*\*\* 資料行中輸入索引鍵值 (原生字串)，並在 [值]\*\*\*\* 資料行中輸入已翻譯的字串。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-674">Enter the key value (native string) in the **Name** column and the translated string in the **Value** column.</span></span>

    ![Welcome.es.resx 檔案 (西班牙文的「歡迎使用」資源檔)，其中 [名稱] 資料行的文字為 Hello，而 [值] 資料行的文字為 Hola (Hello 的西班牙文)](localization/_static/hola.png)

    <span data-ttu-id="f6f5f-676">Visual Studio 會顯示 *Welcome.es.resx* 檔案。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-676">Visual Studio shows the *Welcome.es.resx* file.</span></span>

    ![方案總管，其中顯示「歡迎使用」的西班牙文 (es) 資源檔](localization/_static/se.png)

## <a name="resource-file-naming"></a><span data-ttu-id="f6f5f-678">資源檔命名</span><span class="sxs-lookup"><span data-stu-id="f6f5f-678">Resource file naming</span></span>

<span data-ttu-id="f6f5f-679">資源的命名方式是以其類別的完整類型名稱去掉組件名稱而得。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-679">Resources are named for the full type name of their class minus the assembly name.</span></span> <span data-ttu-id="f6f5f-680">例如，假設專案中的法文資源是 `LocalizationWebsite.Web.Startup` 類別、主要組件為 `LocalizationWebsite.Web.dll`，就會命名為 *Startup.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-680">For example, a French resource in a project whose main assembly is `LocalizationWebsite.Web.dll` for the class `LocalizationWebsite.Web.Startup` would be named *Startup.fr.resx*.</span></span> <span data-ttu-id="f6f5f-681">若是 `LocalizationWebsite.Web.Controllers.HomeController` 類別的資源，則應命名為 *Controllers.HomeController.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-681">A resource for the class `LocalizationWebsite.Web.Controllers.HomeController` would be named *Controllers.HomeController.fr.resx*.</span></span> <span data-ttu-id="f6f5f-682">如果目標類別的命名空間和組件名稱不相同，則需要使用完整類型名稱。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-682">If your targeted class's namespace isn't the same as the assembly name you will need the full type name.</span></span> <span data-ttu-id="f6f5f-683">比方說，範例專案中 `ExtraNamespace.Tools` 類型的資源會命名為 *ExtraNamespace.Tools.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-683">For example, in the sample project a resource for the type `ExtraNamespace.Tools` would be named *ExtraNamespace.Tools.fr.resx*.</span></span>

<span data-ttu-id="f6f5f-684">在範例專案中，`ConfigureServices` 方法會將 `ResourcesPath` 設為 "Resources"，因此首頁控制器的法文資源檔專案相對路徑即為 *Resources/Controllers.HomeController.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-684">In the sample project, the `ConfigureServices` method sets the `ResourcesPath` to "Resources", so the project relative path for the home controller's French resource file is *Resources/Controllers.HomeController.fr.resx*.</span></span> <span data-ttu-id="f6f5f-685">或者，您可以使用資料夾來收集資源檔。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-685">Alternatively, you can use folders to organize resource files.</span></span> <span data-ttu-id="f6f5f-686">若是首頁控制器，路徑就是 *Resources/Controllers/HomeController.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-686">For the home controller, the path would be *Resources/Controllers/HomeController.fr.resx*.</span></span> <span data-ttu-id="f6f5f-687">如果您不使用 `ResourcesPath` 選項，*.resx* 檔案即會放置在專案的基底目錄中。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-687">If you don't use the `ResourcesPath` option, the *.resx* file would go in the project base directory.</span></span> <span data-ttu-id="f6f5f-688">`HomeController` 的資源檔會命名為 *Controllers.HomeController.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-688">The resource file for `HomeController` would be named *Controllers.HomeController.fr.resx*.</span></span> <span data-ttu-id="f6f5f-689">您可依據自己的資源檔收集方式，來選擇要使用點或路徑的命名慣例。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-689">The choice of using the dot or path naming convention depends on how you want to organize your resource files.</span></span>

| <span data-ttu-id="f6f5f-690">資源名稱</span><span class="sxs-lookup"><span data-stu-id="f6f5f-690">Resource name</span></span> | <span data-ttu-id="f6f5f-691">點或路徑命名</span><span class="sxs-lookup"><span data-stu-id="f6f5f-691">Dot or path naming</span></span> |
| ------------   | ------------- |
| <span data-ttu-id="f6f5f-692">Resources/Controllers.HomeController.fr.resx</span><span class="sxs-lookup"><span data-stu-id="f6f5f-692">Resources/Controllers.HomeController.fr.resx</span></span> | <span data-ttu-id="f6f5f-693">點</span><span class="sxs-lookup"><span data-stu-id="f6f5f-693">Dot</span></span>  |
| <span data-ttu-id="f6f5f-694">Resources/Controllers/HomeController.fr.resx</span><span class="sxs-lookup"><span data-stu-id="f6f5f-694">Resources/Controllers/HomeController.fr.resx</span></span>  | <span data-ttu-id="f6f5f-695">路徑</span><span class="sxs-lookup"><span data-stu-id="f6f5f-695">Path</span></span> |
|    |     |

<span data-ttu-id="f6f5f-696">在 views 中使用的資源檔會 `@inject IViewLocalizer` Razor 遵循類似的模式。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-696">Resource files using `@inject IViewLocalizer` in Razor views follow a similar pattern.</span></span> <span data-ttu-id="f6f5f-697">您可以使用點命名或路徑命名方式，來命名檢視的資源檔。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-697">The resource file for a view can be named using either dot naming or path naming.</span></span> Razor<span data-ttu-id="f6f5f-698">查看資源檔模擬其相關聯之視圖檔案的路徑。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-698"> view resource files mimic the path of their associated view file.</span></span> <span data-ttu-id="f6f5f-699">假設我們將 `ResourcesPath` 設為 "Resources"，與 *Views/Home/About.cshtml* 檢視建立關聯的法文資源檔可為下列其一：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-699">Assuming we set the `ResourcesPath` to "Resources", the French resource file associated with the *Views/Home/About.cshtml* view could be either of the following:</span></span>

* <span data-ttu-id="f6f5f-700">Resources/Views/Home/About.fr.resx</span><span class="sxs-lookup"><span data-stu-id="f6f5f-700">Resources/Views/Home/About.fr.resx</span></span>

* <span data-ttu-id="f6f5f-701">Resources/Views.Home.About.fr.resx</span><span class="sxs-lookup"><span data-stu-id="f6f5f-701">Resources/Views.Home.About.fr.resx</span></span>

<span data-ttu-id="f6f5f-702">如果您不使用 `ResourcesPath` 選項，則檢視的 *.resx* 檔案會與檢視位於相同資料夾中。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-702">If you don't use the `ResourcesPath` option, the *.resx* file for a view would be located in the same folder as the view.</span></span>

### <a name="rootnamespaceattribute"></a><span data-ttu-id="f6f5f-703">RootNamespaceAttribute</span><span class="sxs-lookup"><span data-stu-id="f6f5f-703">RootNamespaceAttribute</span></span> 

<span data-ttu-id="f6f5f-704">[RootNamespace](/dotnet/api/microsoft.extensions.localization.rootnamespaceattribute?view=aspnetcore-2.1) 屬性會在組件的根命名空間與組件名稱不同時，提供組件的根命名空間。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-704">The [RootNamespace](/dotnet/api/microsoft.extensions.localization.rootnamespaceattribute?view=aspnetcore-2.1) attribute provides the root namespace of an assembly when the root namespace of an assembly is different than the assembly name.</span></span> 

> [!WARNING]
> <span data-ttu-id="f6f5f-705">當專案的名稱不是有效的 .NET 識別碼時，就可能發生這種情況。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-705">This can occur when a project's name is not a valid .NET identifier.</span></span> <span data-ttu-id="f6f5f-706">例如， `my-project-name.csproj` 會使用根命名空間 `my_project_name` ，以及 `my-project-name` 導致此錯誤的元件名稱。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-706">For instance `my-project-name.csproj` will use the root namespace `my_project_name` and the assembly name `my-project-name` leading to this error.</span></span> 

<span data-ttu-id="f6f5f-707">如果組件的根命名空間與組件名稱不同：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-707">If the root namespace of an assembly is different than the assembly name:</span></span>

* <span data-ttu-id="f6f5f-708">根據預設，當地語系化無法運作。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-708">Localization does not work by default.</span></span>
* <span data-ttu-id="f6f5f-709">在組件中搜尋資源的方式造成當地語系化失敗。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-709">Localization fails due to the way resources are searched for within the assembly.</span></span> <span data-ttu-id="f6f5f-710">`RootNamespace` 是建置時間值，無法用於執行處理序。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-710">`RootNamespace` is a build-time value which is not available to the executing process.</span></span> 

<span data-ttu-id="f6f5f-711">如果 `RootNamespace` 與 `AssemblyName` 不同，請將下列內容納入 *AssemblyInfo.cs* (參數值取代為實際值)：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-711">If the `RootNamespace` is different from the `AssemblyName`, include the following in *AssemblyInfo.cs* (with parameter values replaced with the actual values):</span></span>

```csharp
using System.Reflection;
using Microsoft.Extensions.Localization;

[assembly: ResourceLocation("Resource Folder Name")]
[assembly: RootNamespace("App Root Namespace")]
```

<span data-ttu-id="f6f5f-712">上述程式碼可成功解析 resx 檔案。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-712">The preceding code enables the successful resolution of resx files.</span></span>

## <a name="culture-fallback-behavior"></a><span data-ttu-id="f6f5f-713">文化特性後援行為</span><span class="sxs-lookup"><span data-stu-id="f6f5f-713">Culture fallback behavior</span></span>

<span data-ttu-id="f6f5f-714">搜尋資源時，當地語系化會使用「文化特性後援」。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-714">When searching for a resource, localization engages in "culture fallback".</span></span> <span data-ttu-id="f6f5f-715">從所要求的文化特性開始，如果找不到，就會還原成該文化特性的父文化特性。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-715">Starting from the requested culture, if not found, it reverts to the parent culture of that culture.</span></span> <span data-ttu-id="f6f5f-716">另外，[CultureInfo.Parent](/dotnet/api/system.globalization.cultureinfo.parent) 屬性代表父文化特性。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-716">As an aside, the [CultureInfo.Parent](/dotnet/api/system.globalization.cultureinfo.parent) property represents the parent culture.</span></span> <span data-ttu-id="f6f5f-717">這通常 (但並非一定) 表示從 ISO 中移除國家意符 (Signifier)。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-717">This usually (but not always) means removing the national signifier from the ISO.</span></span> <span data-ttu-id="f6f5f-718">例如，墨西哥的西班牙文方言是 "es-MX"。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-718">For example, the dialect of Spanish spoken in Mexico is "es-MX".</span></span> <span data-ttu-id="f6f5f-719">它具有父系 "es" &mdash; 西班牙文不是任何國家/地區的特定項目。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-719">It has the parent "es"&mdash;Spanish non-specific to any country.</span></span>

<span data-ttu-id="f6f5f-720">假設您的網站收到使用文化特性 "fr-CA" 之 "Welcome" 資源的要求。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-720">Imagine your site receives a request for a "Welcome" resource using culture "fr-CA".</span></span> <span data-ttu-id="f6f5f-721">當地語系化系統會依照順序尋找下列資源，並選取第一個相符項目：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-721">The localization system looks for the following resources, in order, and selects the first match:</span></span>

* <span data-ttu-id="f6f5f-722">*Welcome.fr-CA.resx*</span><span class="sxs-lookup"><span data-stu-id="f6f5f-722">*Welcome.fr-CA.resx*</span></span>
* <span data-ttu-id="f6f5f-723">*Welcome.fr.resx*</span><span class="sxs-lookup"><span data-stu-id="f6f5f-723">*Welcome.fr.resx*</span></span>
* <span data-ttu-id="f6f5f-724">*Welcome.resx* (如果 `NeutralResourcesLanguage` 是 "fr-CA")</span><span class="sxs-lookup"><span data-stu-id="f6f5f-724">*Welcome.resx* (if the `NeutralResourcesLanguage` is "fr-CA")</span></span>

<span data-ttu-id="f6f5f-725">舉例來說，如果您移除 ".fr" 文化特性指示項，並將文化特性設定為法文，則系統會讀取預設資源檔，並將字串當地語系化。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-725">As an example, if you remove the ".fr" culture designator and you have the culture set to French, the default resource file is read and strings are localized.</span></span> <span data-ttu-id="f6f5f-726">當沒有任何項目符合您要求的文化特性時，資源管理員即會指定預設資源或後援資源。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-726">The Resource manager designates a default or fallback resource for when nothing meets your requested culture.</span></span> <span data-ttu-id="f6f5f-727">如果您只想在要求的文化特性缺少資源時傳回索引鍵，就不能使用預設資源檔。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-727">If you want to just return the key when missing a resource for the requested culture you must not have a default resource file.</span></span>

### <a name="generate-resource-files-with-visual-studio"></a><span data-ttu-id="f6f5f-728">使用 Visual Studio 產生資源檔</span><span class="sxs-lookup"><span data-stu-id="f6f5f-728">Generate resource files with Visual Studio</span></span>

<span data-ttu-id="f6f5f-729">如果您在 Visual Studio 中建立資源檔，但檔案名稱中不含文化特性 (例如 *Welcome.resx*)，則 Visual Studio 會針對每個字串的屬性建立 C# 類別。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-729">If you create a resource file in Visual Studio without a culture in the file name (for example, *Welcome.resx*), Visual Studio will create a C# class with a property for each string.</span></span> <span data-ttu-id="f6f5f-730">但這通常不是您使用 ASP.NET Core 的初衷。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-730">That's usually not what you want with ASP.NET Core.</span></span> <span data-ttu-id="f6f5f-731">一般來說，您不會有預設的 *.resx* 資源檔 (不含文化特性名稱的 *.resx* 檔案)。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-731">You typically don't have a default *.resx* resource file (a *.resx* file without the culture name).</span></span> <span data-ttu-id="f6f5f-732">因此，建議您建立含有文化特性名稱的 *.resx* 檔案 (例如 *Welcome.fr.resx*)。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-732">We suggest you create the *.resx* file with a culture name (for example *Welcome.fr.resx*).</span></span> <span data-ttu-id="f6f5f-733">當您建立含有文化特性名稱的 *.resx* 檔案時，Visual Studio 就不會產生類別檔案。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-733">When you create a *.resx* file with a culture name, Visual Studio won't generate the class file.</span></span>

### <a name="add-other-cultures"></a><span data-ttu-id="f6f5f-734">新增其他文化特性</span><span class="sxs-lookup"><span data-stu-id="f6f5f-734">Add other cultures</span></span>

<span data-ttu-id="f6f5f-735">每種語言和文化特性的組合 (非預設語言) 都需要唯一的資源檔。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-735">Each language and culture combination (other than the default language) requires a unique resource file.</span></span> <span data-ttu-id="f6f5f-736">若要建立不同文化特性和地區設定的資源檔，您可以建立新的資源檔並將 ISO 語言代碼作為檔名的一部分 (例如 **en-us**、**fr-ca** 和 **en-gb**)。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-736">You create resource files for different cultures and locales by creating new resource files in which the ISO language codes are part of the file name (for example, **en-us**, **fr-ca**, and **en-gb**).</span></span> <span data-ttu-id="f6f5f-737">您應將 ISO 代碼置於檔案名稱和 *.resx* 副檔名之間，例如 *Welcome.es-MX.resx* (西班牙文/墨西哥)。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-737">These ISO codes are placed between the file name and the *.resx* file extension, as in *Welcome.es-MX.resx* (Spanish/Mexico).</span></span>

## <a name="implement-a-strategy-to-select-the-languageculture-for-each-request"></a><span data-ttu-id="f6f5f-738">實作可依據每項要求選取語言/文化特性的策略</span><span class="sxs-lookup"><span data-stu-id="f6f5f-738">Implement a strategy to select the language/culture for each request</span></span>

### <a name="configure-localization"></a><span data-ttu-id="f6f5f-739">設定當地語系化</span><span class="sxs-lookup"><span data-stu-id="f6f5f-739">Configure localization</span></span>

<span data-ttu-id="f6f5f-740">您可以在 `Startup.ConfigureServices` 方法中設定當地語系化：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-740">Localization is configured in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](localization/sample/3.x/Localization/Startup.cs?name=snippet1)]

* <span data-ttu-id="f6f5f-741">`AddLocalization` 可將當地語系化服務新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-741">`AddLocalization` Adds the localization services to the services container.</span></span> <span data-ttu-id="f6f5f-742">上方的程式碼也會將資源路徑設為 "Resources"。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-742">The code above also sets the resources path to "Resources".</span></span>

* <span data-ttu-id="f6f5f-743">`AddViewLocalization` 可支援當地語系化的檢視檔案。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-743">`AddViewLocalization` Adds support for localized view files.</span></span> <span data-ttu-id="f6f5f-744">在此範例中，檢視的當地語系化會以檢視檔案的後置詞為依據。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-744">In this sample view localization is based on the view file suffix.</span></span> <span data-ttu-id="f6f5f-745">例如 *Index.fr.cshtml* 檔案中的 "fr"。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-745">For example "fr" in the *Index.fr.cshtml* file.</span></span>

* <span data-ttu-id="f6f5f-746">`AddDataAnnotationsLocalization` 可支援透過 `IStringLocalizer` 抽象概念而來的當地語系化 `DataAnnotations` 驗證訊息。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-746">`AddDataAnnotationsLocalization` Adds support for localized `DataAnnotations` validation messages through `IStringLocalizer` abstractions.</span></span>

### <a name="localization-middleware"></a><span data-ttu-id="f6f5f-747">當地語系化中介軟體</span><span class="sxs-lookup"><span data-stu-id="f6f5f-747">Localization middleware</span></span>

<span data-ttu-id="f6f5f-748">您可以在當地語系化[中介軟體](xref:fundamentals/middleware/index)中，設定要求目前的文化特性。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-748">The current culture on a request is set in the localization [Middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="f6f5f-749">已在 `Startup.Configure` 方法中啟用當地語系化中介軟體。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-749">The localization middleware is enabled in the `Startup.Configure` method.</span></span> <span data-ttu-id="f6f5f-750">您必須在可能檢查要求文化特性的任何中介軟體之前，設定當地語系化中介軟體 (例如 `app.UseMvcWithDefaultRoute()`)。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-750">The localization middleware must be configured before any middleware which might check the request culture (for example, `app.UseMvcWithDefaultRoute()`).</span></span>

[!code-csharp[](localization/sample/3.x/Localization/Startup.cs?name=snippet2)]
[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

<span data-ttu-id="f6f5f-751">`UseRequestLocalization` 會初始化 `RequestLocalizationOptions` 物件。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-751">`UseRequestLocalization` initializes a `RequestLocalizationOptions` object.</span></span> <span data-ttu-id="f6f5f-752">在每次要求時，系統會列舉 `RequestLocalizationOptions` 中的 `RequestCultureProvider` 清單，並使用能成功判斷要求的文化特性的第一個提供者。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-752">On every request the list of `RequestCultureProvider` in the `RequestLocalizationOptions` is enumerated and the first provider that can successfully determine the request culture is used.</span></span> <span data-ttu-id="f6f5f-753">預設的提供者是來自 `RequestLocalizationOptions` 類別：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-753">The default providers come from the `RequestLocalizationOptions` class:</span></span>

1. `QueryStringRequestCultureProvider`
2. `CookieRequestCultureProvider`
3. `AcceptLanguageHeaderRequestCultureProvider`

<span data-ttu-id="f6f5f-754">預設清單會由針對性高到低來排列。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-754">The default list goes from most specific to least specific.</span></span> <span data-ttu-id="f6f5f-755">在本文稍後，我們會說明如何變更順序，甚至新增自訂的文化特性提供者。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-755">Later in the article we'll see how you can change the order and even add a custom culture provider.</span></span> <span data-ttu-id="f6f5f-756">如果沒有任何提供者可以判斷要求的文化特性，即會使用 `DefaultRequestCulture`。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-756">If none of the providers can determine the request culture, the `DefaultRequestCulture` is used.</span></span>

### <a name="querystringrequestcultureprovider"></a><span data-ttu-id="f6f5f-757">QueryStringRequestCultureProvider</span><span class="sxs-lookup"><span data-stu-id="f6f5f-757">QueryStringRequestCultureProvider</span></span>

<span data-ttu-id="f6f5f-758">有些應用程式會使用查詢字串來設定[文化特性和 UI 文化特性](https://msdn.microsoft.com/library/system.globalization.cultureinfo.aspx)。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-758">Some apps will use a query string to set the [culture and UI culture](https://msdn.microsoft.com/library/system.globalization.cultureinfo.aspx).</span></span> <span data-ttu-id="f6f5f-759">若是使用 Cookie 或 Accept-Language 標頭方法的應用程式，您可以將查詢字串新增至 URL 以偵錯和測試程式碼。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-759">For apps that use the cookie or Accept-Language header approach, adding a query string to the URL is useful for debugging and testing code.</span></span> <span data-ttu-id="f6f5f-760">系統預設會將 `QueryStringRequestCultureProvider` 登錄為 `RequestCultureProvider` 清單中的第一個當地語系化提供者。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-760">By default, the `QueryStringRequestCultureProvider` is registered as the first localization provider in the `RequestCultureProvider` list.</span></span> <span data-ttu-id="f6f5f-761">您應傳遞查詢字串參數 `culture` 和 `ui-culture`。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-761">You pass the query string parameters `culture` and `ui-culture`.</span></span> <span data-ttu-id="f6f5f-762">下列範例會設定西班牙文/墨西哥的特定文化特性 (語言和地區)：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-762">The following example sets the specific culture (language and region) to Spanish/Mexico:</span></span>

   `http://localhost:5000/?culture=es-MX&ui-culture=es-MX`

<span data-ttu-id="f6f5f-763">如果您只傳入兩者其一 (`culture` 或 `ui-culture`)，查詢字串提供者就會使用您傳入的項目來設定這兩個值。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-763">If you only pass in one of the two (`culture` or `ui-culture`), the query string provider will set both values using the one you passed in.</span></span> <span data-ttu-id="f6f5f-764">例如，若只設定文化特性，即會同時設定 `Culture` 和 `UICulture`：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-764">For example, setting just the culture will set both the `Culture` and the `UICulture`:</span></span>

   `http://localhost:5000/?culture=es-MX`

### <a name="cookierequestcultureprovider"></a><span data-ttu-id="f6f5f-765">CookieRequestCultureProvider</span><span class="sxs-lookup"><span data-stu-id="f6f5f-765">CookieRequestCultureProvider</span></span>

<span data-ttu-id="f6f5f-766">生產環境應用程式通常會提供一個機制，來設定 ASP.NET Core 文化特性 Cookie 的文化特性。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-766">Production apps will often provide a mechanism to set the culture with the ASP.NET Core culture cookie.</span></span> <span data-ttu-id="f6f5f-767">若要建立 Cookie，請使用 `MakeCookieValue` 方法。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-767">Use the `MakeCookieValue` method to create a cookie.</span></span>

<span data-ttu-id="f6f5f-768">會傳回 `CookieRequestCultureProvider` `DefaultCookieName` 用來追蹤使用者慣用文化特性資訊的預設 cookie 名稱。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-768">The `CookieRequestCultureProvider` `DefaultCookieName` returns the default cookie name used to track the user's preferred culture information.</span></span> <span data-ttu-id="f6f5f-769">預設 Cookie 名稱為 `.AspNetCore.Culture`。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-769">The default cookie name is `.AspNetCore.Culture`.</span></span>

<span data-ttu-id="f6f5f-770">Cookie 格式為 `c=%LANGCODE%|uic=%LANGCODE%`，其中 `c` 是 `Culture` 而 `uic` 是 `UICulture`，例如：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-770">The cookie format is `c=%LANGCODE%|uic=%LANGCODE%`, where `c` is `Culture` and `uic` is `UICulture`, for example:</span></span>

    c=en-UK|uic=en-US

<span data-ttu-id="f6f5f-771">如果您只指定文化特性資訊和 UI 文化特性其一，系統就會將您所指定的文化特性用於文化特性資訊和 UI 文化特性。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-771">If you only specify one of culture info and UI culture, the specified culture will be used for both culture info and UI culture.</span></span>

### <a name="the-accept-language-http-header"></a><span data-ttu-id="f6f5f-772">Accept-Language HTTP 標頭</span><span class="sxs-lookup"><span data-stu-id="f6f5f-772">The Accept-Language HTTP header</span></span>

<span data-ttu-id="f6f5f-773">您可在大多數的瀏覽器中設定 [Accept-Language 標頭](https://www.w3.org/International/questions/qa-accept-lang-locales)，其最初的設計目的是用來指定使用者的語言。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-773">The [Accept-Language header](https://www.w3.org/International/questions/qa-accept-lang-locales) is settable in most browsers and was originally intended to specify the user's language.</span></span> <span data-ttu-id="f6f5f-774">這項設定可指出瀏覽器已設好要傳送哪些項目，或已從基礎作業系統繼承哪些項目。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-774">This setting indicates what the browser has been set to send or has inherited from the underlying operating system.</span></span> <span data-ttu-id="f6f5f-775">透過瀏覽器要求的 Accept-Language HTTP 標頭來偵測使用者的慣用語言，並非萬無一失 (請參閱 [Setting language preferences in a browser](https://www.w3.org/International/questions/qa-lang-priorities.en.php) (在瀏覽器中設定語言喜好設定)。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-775">The Accept-Language HTTP header from a browser request isn't an infallible way to detect the user's preferred language (see [Setting language preferences in a browser](https://www.w3.org/International/questions/qa-lang-priorities.en.php)).</span></span> <span data-ttu-id="f6f5f-776">生產環境應用程式應該包含可讓使用者自訂文化特性的選擇方式。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-776">A production app should include a way for a user to customize their choice of culture.</span></span>

### <a name="set-the-accept-language-http-header-in-ie"></a><span data-ttu-id="f6f5f-777">在 IE 中設定 Accept-Language HTTP 標頭</span><span class="sxs-lookup"><span data-stu-id="f6f5f-777">Set the Accept-Language HTTP header in IE</span></span>

1. <span data-ttu-id="f6f5f-778">從齒輪圖示，點選 [網際網路選項]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-778">From the gear icon, tap **Internet Options**.</span></span>

2. <span data-ttu-id="f6f5f-779">點選 [語言]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-779">Tap **Languages**.</span></span>

    ![網際網路選項](localization/_static/lang.png)

3. <span data-ttu-id="f6f5f-781">點選 [設定語言喜好設定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-781">Tap **Set Language Preferences**.</span></span>

4. <span data-ttu-id="f6f5f-782">點選 [新增語言]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-782">Tap **Add a language**.</span></span>

5. <span data-ttu-id="f6f5f-783">新增語言。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-783">Add the language.</span></span>

6. <span data-ttu-id="f6f5f-784">點選語言，然後點選 [上移]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-784">Tap the language, then tap **Move Up**.</span></span>

### <a name="the-content-language-http-header"></a><span data-ttu-id="f6f5f-785">內容語言 HTTP 標頭</span><span class="sxs-lookup"><span data-stu-id="f6f5f-785">The Content-Language HTTP header</span></span>

<span data-ttu-id="f6f5f-786">[內容語言](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Language)實體標頭：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-786">The [Content-Language](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Language) entity header:</span></span>

 - <span data-ttu-id="f6f5f-787">用來描述適用于物件的語言。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-787">Is used to describe the language(s) intended for the audience.</span></span>
 - <span data-ttu-id="f6f5f-788">可讓使用者根據使用者的慣用語言來區分。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-788">Allows a user to differentiate according to the users' own preferred language.</span></span>

<span data-ttu-id="f6f5f-789">實體標頭會同時用於 HTTP 要求和回應。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-789">Entity headers are used in both HTTP requests and responses.</span></span>

<span data-ttu-id="f6f5f-790">您 `Content-Language` 可以藉由設定屬性來新增標頭 `ApplyCurrentCultureToResponseHeaders` 。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-790">The `Content-Language` header can be added by setting the property `ApplyCurrentCultureToResponseHeaders`.</span></span>

<span data-ttu-id="f6f5f-791">新增 `Content-Language` 標頭：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-791">Adding the `Content-Language` header:</span></span>

 - <span data-ttu-id="f6f5f-792">允許 RequestLocalizationMiddleware 使用來設定 `Content-Language` 標頭 `CurrentUICulture` 。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-792">Allows the RequestLocalizationMiddleware to set the `Content-Language` header with the `CurrentUICulture`.</span></span>
 - <span data-ttu-id="f6f5f-793">不需要明確地設定回應標頭 `Content-Language` 。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-793">Eliminates the need to set the response header `Content-Language` explicitly.</span></span>

```csharp
app.UseRequestLocalization(new RequestLocalizationOptions
{
    ApplyCurrentCultureToResponseHeaders = true
});
```

### <a name="use-a-custom-provider"></a><span data-ttu-id="f6f5f-794">使用自訂提供者</span><span class="sxs-lookup"><span data-stu-id="f6f5f-794">Use a custom provider</span></span>

<span data-ttu-id="f6f5f-795">假設您想要讓客戶將他們的語言和文化特性儲存在您的資料庫中。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-795">Suppose you want to let your customers store their language and culture in your databases.</span></span> <span data-ttu-id="f6f5f-796">您可以撰寫提供者，以供使用者查詢這些值。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-796">You could write a provider to look up these values for the user.</span></span> <span data-ttu-id="f6f5f-797">下列程式碼會示範如何新增自訂提供者：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-797">The following code shows how to add a custom provider:</span></span>

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

<span data-ttu-id="f6f5f-798">使用 `RequestLocalizationOptions` 新增或移除當地語系化提供者。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-798">Use `RequestLocalizationOptions` to add or remove localization providers.</span></span>

### <a name="set-the-culture-programmatically"></a><span data-ttu-id="f6f5f-799">以程式設計方式來設定文化特性</span><span class="sxs-lookup"><span data-stu-id="f6f5f-799">Set the culture programmatically</span></span>

<span data-ttu-id="f6f5f-800">[GitHub](https://github.com/aspnet/entropy) 上的這個範例 **Localization.StarterWeb** 專案包含可設定 `Culture` 的 UI。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-800">This sample **Localization.StarterWeb** project on [GitHub](https://github.com/aspnet/entropy) contains UI to set the `Culture`.</span></span> <span data-ttu-id="f6f5f-801">*Views/Shared/_SelectLanguagePartial.cshtml* 檔可讓您從支援的文化特性清單中選取文化特性：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-801">The *Views/Shared/_SelectLanguagePartial.cshtml* file allows you to select the culture from the list of supported cultures:</span></span>

[!code-cshtml[](localization/sample/3.x/Localization/Views/Shared/_SelectLanguagePartial.cshtml)]

<span data-ttu-id="f6f5f-802">系統會將 *Views/Shared/_SelectLanguagePartial.cshtml* 檔案新增至配置檔案的 `footer` 區段，以供所有檢視使用：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-802">The *Views/Shared/_SelectLanguagePartial.cshtml* file is added to the `footer` section of the layout file so it will be available to all views:</span></span>

[!code-cshtml[](localization/sample/3.x/Localization/Views/Shared/_Layout.cshtml?range=43-56&highlight=10)]

<span data-ttu-id="f6f5f-803">`SetLanguage` 方法會設定文化特性的 Cookie。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-803">The `SetLanguage` method sets the culture cookie.</span></span>

[!code-csharp[](localization/sample/3.x/Localization/Controllers/HomeController.cs?range=57-67)]

<span data-ttu-id="f6f5f-804">您無法將 *_SelectLanguagePartial.cshtml* 插入這個專案的範例程式碼。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-804">You can't plug in the *_SelectLanguagePartial.cshtml* to sample code for this project.</span></span> <span data-ttu-id="f6f5f-805">[GitHub](https://github.com/aspnet/entropy)上的**localization.starterweb**專案具有程式碼，可透過相依性 `RequestLocalizationOptions` 插入容器將傳遞至 Razor 部分。 [Dependency Injection](dependency-injection.md)</span><span class="sxs-lookup"><span data-stu-id="f6f5f-805">The **Localization.StarterWeb** project on [GitHub](https://github.com/aspnet/entropy) has code to flow the `RequestLocalizationOptions` to a Razor partial through the [Dependency Injection](dependency-injection.md) container.</span></span>

## <a name="model-binding-route-data-and-query-strings"></a><span data-ttu-id="f6f5f-806">模型系結路由資料和查詢字串</span><span class="sxs-lookup"><span data-stu-id="f6f5f-806">Model binding route data and query strings</span></span>

<span data-ttu-id="f6f5f-807">請參閱模型系結[路由資料和查詢字串的全球化行為](xref:mvc/models/model-binding#glob)。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-807">See [Globalization behavior of model binding route data and query strings](xref:mvc/models/model-binding#glob).</span></span>

## <a name="globalization-and-localization-terms"></a><span data-ttu-id="f6f5f-808">全球化和當地語系化詞彙</span><span class="sxs-lookup"><span data-stu-id="f6f5f-808">Globalization and localization terms</span></span>

<span data-ttu-id="f6f5f-809">在進行應用程式的當地語系化程序時，您也需要具備現代軟體開發中常用的相關字元集基本知識，並了解與它們建立關聯的問題。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-809">The process of localizing your app also requires a basic understanding of relevant character sets commonly used in modern software development and an understanding of the issues associated with them.</span></span> <span data-ttu-id="f6f5f-810">雖然所有電腦都會將文字儲存為數字 (程式碼)，但不同系統會使用不同數字來儲存相同的文字。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-810">Although all computers store text as numbers (codes), different systems store the same text using different numbers.</span></span> <span data-ttu-id="f6f5f-811">當地語系化是指針對特定文化特性/地區設定，轉譯應用程式使用者介面 (UI) 的程序。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-811">The localization process refers to translating the app user interface (UI) for a specific culture/locale.</span></span>

<span data-ttu-id="f6f5f-812">[可當地語系化](/dotnet/standard/globalization-localization/localizability-review)是確認全球化應用程式已準備好進行當地語系化的中繼程序。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-812">[Localizability](/dotnet/standard/globalization-localization/localizability-review) is an intermediate process for verifying that a globalized app is ready for localization.</span></span>

<span data-ttu-id="f6f5f-813">文化特性名稱的 [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) 格式是 `<languagecode2>-<country/regioncode2>`，其中 `<languagecode2>` 是語言代碼，而 `<country/regioncode2>` 是子文化特性代碼。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-813">The [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) format for the culture name is `<languagecode2>-<country/regioncode2>`, where `<languagecode2>` is the language code and `<country/regioncode2>` is the subculture code.</span></span> <span data-ttu-id="f6f5f-814">例如，`es-CL` 是指西班牙文 (智利)，`en-US` 是指英文 (美國)，而 `en-AU` 是指英文 (澳大利亞)。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-814">For example, `es-CL` for Spanish (Chile), `en-US` for English (United States), and `en-AU` for English (Australia).</span></span> <span data-ttu-id="f6f5f-815">[RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) 是 ISO 639 (兩個小寫字母，為與某個語言建立關聯的文化特性代碼) 及 ISO 3166 (兩個大寫字母，為與某個國家或地區建立關聯的子文化特性代碼) 的組合。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-815">[RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) is a combination of an ISO 639 two-letter lowercase culture code associated with a language and an ISO 3166 two-letter uppercase subculture code associated with a country or region.</span></span> <span data-ttu-id="f6f5f-816">請參閱 [Language Culture Name](https://msdn.microsoft.com/library/ee825488(v=cs.20).aspx) (語言的文化特性名稱)。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-816">See [Language Culture Name](https://msdn.microsoft.com/library/ee825488(v=cs.20).aspx).</span></span>

<span data-ttu-id="f6f5f-817">國際化通常縮寫為 "I18N"。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-817">Internationalization is often abbreviated to "I18N".</span></span> <span data-ttu-id="f6f5f-818">這個縮寫是採用該詞彙的第一個和最後一個字母，以及這兩個字母間的字母數組成，因此 18 代表第一個字母 "I" 及最後 "N" 中間的字母數。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-818">The abbreviation takes the first and last letters and the number of letters between them, so 18 stands for the number of letters between the first "I" and the last "N".</span></span> <span data-ttu-id="f6f5f-819">同樣的原則也適用於全球化 (G11N) 與當地語系化 (L10N) 的縮寫。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-819">The same applies to Globalization (G11N), and Localization (L10N).</span></span>

<span data-ttu-id="f6f5f-820">詞彙：</span><span class="sxs-lookup"><span data-stu-id="f6f5f-820">Terms:</span></span>

* <span data-ttu-id="f6f5f-821">全球化 (G11N)：讓應用程式支援不同語言和區域的程序。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-821">Globalization (G11N): The process of making an app support different languages and regions.</span></span>
* <span data-ttu-id="f6f5f-822">當地語系化 (L10N)：針對特定語言和區域，自訂應用程式的程序。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-822">Localization (L10N): The process of customizing an app for a given language and region.</span></span>
* <span data-ttu-id="f6f5f-823">國際化 (I18N)：包含全球化和當地語系化這兩部分的描述。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-823">Internationalization (I18N): Describes both globalization and localization.</span></span>
* <span data-ttu-id="f6f5f-824">文化特性：它是一種語言和/或地區。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-824">Culture: It's a language and, optionally, a region.</span></span>
* <span data-ttu-id="f6f5f-825">中性文化特性：具有指定的語言但不限地區的文化特性 </span><span class="sxs-lookup"><span data-stu-id="f6f5f-825">Neutral culture: A culture that has a specified language, but not a region.</span></span> <span data-ttu-id="f6f5f-826">(例如 "en"、"es")。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-826">(for example "en", "es")</span></span>
* <span data-ttu-id="f6f5f-827">特定文化特性：具有指定的語言和地區的文化特性 </span><span class="sxs-lookup"><span data-stu-id="f6f5f-827">Specific culture: A culture that has a specified language and region.</span></span> <span data-ttu-id="f6f5f-828">(例如 "en-US"、"en-GB"、"es-CL")。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-828">(for example "en-US", "en-GB", "es-CL")</span></span>
* <span data-ttu-id="f6f5f-829">父文化特性：包含特定文化特性的中性文化特性 </span><span class="sxs-lookup"><span data-stu-id="f6f5f-829">Parent culture: The neutral culture that contains a specific culture.</span></span> <span data-ttu-id="f6f5f-830">(例如，"en" 是 "en-US" 和 "en-GB" 的父文化特性)。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-830">(for example, "en" is the parent culture of "en-US" and "en-GB")</span></span>
* <span data-ttu-id="f6f5f-831">地區設定：地區設定與文化特性相同。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-831">Locale: A locale is the same as a culture.</span></span>

[!INCLUDE[](~/includes/localization/currency.md)]

[!INCLUDE[](~/includes/localization/unsupported-culture-log-level.md)]

## <a name="additional-resources"></a><span data-ttu-id="f6f5f-832">其他資源</span><span class="sxs-lookup"><span data-stu-id="f6f5f-832">Additional resources</span></span>

* <xref:fundamentals/troubleshoot-aspnet-core-localization>
* <span data-ttu-id="f6f5f-833">本文使用的 [Localization.StarterWeb 專案](https://github.com/aspnet/Entropy/tree/master/samples/Localization.StarterWeb)。</span><span class="sxs-lookup"><span data-stu-id="f6f5f-833">[Localization.StarterWeb project](https://github.com/aspnet/Entropy/tree/master/samples/Localization.StarterWeb) used in the article.</span></span>
* [<span data-ttu-id="f6f5f-834">全球化與當地語系化 .NET 應用程式</span><span class="sxs-lookup"><span data-stu-id="f6f5f-834">Globalizing and localizing .NET applications</span></span>](/dotnet/standard/globalization-localization/index)
* [<span data-ttu-id="f6f5f-835">.Resx 檔案中的資源</span><span class="sxs-lookup"><span data-stu-id="f6f5f-835">Resources in .resx Files</span></span>](/dotnet/framework/resources/working-with-resx-files-programmatically)
* [<span data-ttu-id="f6f5f-836">Microsoft 多語應用程式工具組</span><span class="sxs-lookup"><span data-stu-id="f6f5f-836">Microsoft Multilingual App Toolkit</span></span>](https://marketplace.visualstudio.com/items?itemName=MultilingualAppToolkit.MultilingualAppToolkit-18308)
* [<span data-ttu-id="f6f5f-837">當地語系化和泛型</span><span class="sxs-lookup"><span data-stu-id="f6f5f-837">Localization & Generics</span></span>](http://hishambinateya.com/localization-and-generics)

::: moniker-end
