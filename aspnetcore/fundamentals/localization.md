---
title: "ASP.NET Core 全球化和當地語系化"
author: rick-anderson
description: "了解 ASP.NET Core 如何提供服務與中介軟體，以將內容當地語系化成不同的語言與文化特性。"
manager: wpickett
ms.author: riande
ms.date: 01/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/localization
ms.openlocfilehash: 794abf628beff7e5c78f9ca04309694d46910373
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="globalization-and-localization-in-aspnet-core"></a><span data-ttu-id="f7785-103">ASP.NET Core 全球化和當地語系化</span><span class="sxs-lookup"><span data-stu-id="f7785-103">Globalization and localization in ASP.NET Core</span></span>

<span data-ttu-id="f7785-104">由 [Rick Anderson](https://twitter.com/RickAndMSFT)、[Damien Bowden](https://twitter.com/damien_bod)、[Bart Calixto](https://twitter.com/bartmax)、[Nadeem Afana](https://twitter.com/NadeemAfana) 和 [Hisham Bin Ateya](https://twitter.com/hishambinateya) 提供</span><span class="sxs-lookup"><span data-stu-id="f7785-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Damien Bowden](https://twitter.com/damien_bod), [Bart Calixto](https://twitter.com/bartmax), [Nadeem Afana](https://twitter.com/NadeemAfana), and [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span></span>

<span data-ttu-id="f7785-105">使用 ASP.NET Core 建立多語系網站時，可讓更廣大的群眾使用您的網站。</span><span class="sxs-lookup"><span data-stu-id="f7785-105">Creating a multilingual website with ASP.NET Core will allow your site to reach a wider audience.</span></span> <span data-ttu-id="f7785-106">ASP.NET Core 提供服務與中介軟體，可將網站當地語系化成不同的語言與文化特性。</span><span class="sxs-lookup"><span data-stu-id="f7785-106">ASP.NET Core provides services and middleware for localizing into different languages and cultures.</span></span>

<span data-ttu-id="f7785-107">國際化包含[全球化](https://docs.microsoft.com/dotnet/api/system.globalization)和[當地語系化](https://docs.microsoft.com/dotnet/standard/globalization-localization/localization)這兩部分。</span><span class="sxs-lookup"><span data-stu-id="f7785-107">Internationalization involves [Globalization](https://docs.microsoft.com/dotnet/api/system.globalization) and [Localization](https://docs.microsoft.com/dotnet/standard/globalization-localization/localization).</span></span> <span data-ttu-id="f7785-108">全球化是指設計出可支援不同文化特性之應用程式的程序。</span><span class="sxs-lookup"><span data-stu-id="f7785-108">Globalization is the process of designing apps that support different cultures.</span></span> <span data-ttu-id="f7785-109">透過全球化，您可新增支援與特定地區相關之已定義語言指令碼的輸入、顯示及輸出作業。</span><span class="sxs-lookup"><span data-stu-id="f7785-109">Globalization adds support for input, display, and output of a defined set of language scripts that relate to specific geographic areas.</span></span>

<span data-ttu-id="f7785-110">當地語系化是指針對全球化應用程式進行調整的程序，且您已順應特定文化特性/地區設定對這些全球化應用程式進行可當地語系化處理。</span><span class="sxs-lookup"><span data-stu-id="f7785-110">Localization is the process of adapting a globalized app, which you have already processed for localizability, to a particular culture/locale.</span></span> <span data-ttu-id="f7785-111">如需詳細資訊，請參閱本文件結尾處的**全球化和當地語系化詞彙**。</span><span class="sxs-lookup"><span data-stu-id="f7785-111">For more information see **Globalization and localization terms** near the end of this document.</span></span>

<span data-ttu-id="f7785-112">應用程式當地語系化包含下列作業：</span><span class="sxs-lookup"><span data-stu-id="f7785-112">App localization involves the following:</span></span>

1. <span data-ttu-id="f7785-113">讓應用程式的內容可當地語系化</span><span class="sxs-lookup"><span data-stu-id="f7785-113">Make the app's content localizable</span></span>

2. <span data-ttu-id="f7785-114">針對您支援的語言和文化特性提供當地語系化資源</span><span class="sxs-lookup"><span data-stu-id="f7785-114">Provide localized resources for the languages and cultures you support</span></span>

3. <span data-ttu-id="f7785-115">實作可依據每項要求選取語言/文化特性的策略</span><span class="sxs-lookup"><span data-stu-id="f7785-115">Implement a strategy to select the language/culture for each request</span></span>

## <a name="make-the-apps-content-localizable"></a><span data-ttu-id="f7785-116">讓應用程式的內容可當地語系化</span><span class="sxs-lookup"><span data-stu-id="f7785-116">Make the app's content localizable</span></span>

<span data-ttu-id="f7785-117">ASP.NET Core 中導入了 `IStringLocalizer` 和 `IStringLocalizer<T>`，其設計用意是提高開發當地語系化應用程式時的生產力。</span><span class="sxs-lookup"><span data-stu-id="f7785-117">Introduced in ASP.NET Core, `IStringLocalizer` and `IStringLocalizer<T>` were architected to improve productivity when developing localized apps.</span></span> <span data-ttu-id="f7785-118">`IStringLocalizer` 會使用 [ResourceManager](https://docs.microsoft.com/dotnet/api/system.resources.resourcemanager) 和 [ResourceReader](https://docs.microsoft.com/dotnet/api/system.resources.resourcereader)，於執行階段時提供文化特性特有的資源。</span><span class="sxs-lookup"><span data-stu-id="f7785-118">`IStringLocalizer` uses the [ResourceManager](https://docs.microsoft.com/dotnet/api/system.resources.resourcemanager) and [ResourceReader](https://docs.microsoft.com/dotnet/api/system.resources.resourcereader) to provide culture-specific resources at run time.</span></span> <span data-ttu-id="f7785-119">這個簡單介面具有的索引子和 `IEnumerable` 可傳回當地語系化字串。</span><span class="sxs-lookup"><span data-stu-id="f7785-119">The simple interface has an indexer and an `IEnumerable` for returning localized strings.</span></span> <span data-ttu-id="f7785-120">`IStringLocalizer` 不需要您將預設語言字串儲存在資源檔中。</span><span class="sxs-lookup"><span data-stu-id="f7785-120">`IStringLocalizer` doesn't require you to store the default language strings in a resource file.</span></span> <span data-ttu-id="f7785-121">您不必在開發初期建立資源檔，即可開發以當地語系化為目標的應用程式。</span><span class="sxs-lookup"><span data-stu-id="f7785-121">You can develop an app targeted for localization and not need to create resource files early in development.</span></span> <span data-ttu-id="f7785-122">下列程式碼會示範如何包裝 "About Title" 字串以進行當地語系化。</span><span class="sxs-lookup"><span data-stu-id="f7785-122">The code below shows how to wrap the string "About Title" for localization.</span></span>

[!code-csharp[Main](localization/sample/Localization/Controllers/AboutController.cs)]

<span data-ttu-id="f7785-123">在上述程式碼中，`IStringLocalizer<T>` 實作是來自[相依性插入](dependency-injection.md)。</span><span class="sxs-lookup"><span data-stu-id="f7785-123">In the code above, the `IStringLocalizer<T>` implementation comes from [Dependency Injection](dependency-injection.md).</span></span> <span data-ttu-id="f7785-124">如果找不到 "About Title" 的當地語系化值，即會傳回索引子的索引鍵，也就是 "About Title" 字串。</span><span class="sxs-lookup"><span data-stu-id="f7785-124">If the localized value of "About Title" isn't found, then the indexer key is returned, that is, the string "About Title".</span></span> <span data-ttu-id="f7785-125">您可以保留應用程式中的預設語言常值字串，並將其包裝在當地語系化工具中，以便專注於開發應用程式。</span><span class="sxs-lookup"><span data-stu-id="f7785-125">You can leave the default language literal strings in the app and wrap them in the localizer, so that you can focus on developing the app.</span></span> <span data-ttu-id="f7785-126">您不用先建立預設資源檔，即可使用預設語言來開發應用程式，並針對當地語系化步驟進行應用程式的準備。</span><span class="sxs-lookup"><span data-stu-id="f7785-126">You develop your app with your default language and prepare it for the localization step without first creating a default resource file.</span></span> <span data-ttu-id="f7785-127">或者，您可以使用傳統方法，並提供索引鍵以擷取預設語言字串。</span><span class="sxs-lookup"><span data-stu-id="f7785-127">Alternatively, you can use the traditional approach and provide a key to retrieve the default language string.</span></span> <span data-ttu-id="f7785-128">對許多開發人員來說，新的工作流程 (單純包裝字串常值而不使用預設語言的 *.resx* 檔案) 可以降低當地語系化應用程式的額外負荷。</span><span class="sxs-lookup"><span data-stu-id="f7785-128">For many developers the new workflow of not having a default language *.resx* file and simply wrapping the string literals can reduce the overhead of localizing an app.</span></span> <span data-ttu-id="f7785-129">其他開發人員則偏好傳統的工作流程，因為這種方法更便於使用較長的字串常值，且更易於更新當地語系化的字串。</span><span class="sxs-lookup"><span data-stu-id="f7785-129">Other developers will prefer the traditional work flow as it can make it easier to work with longer string literals and make it easier to update localized strings.</span></span>

<span data-ttu-id="f7785-130">若是包含 HTML 的資源，請使用 `IHtmlLocalizer<T>` 實作。</span><span class="sxs-lookup"><span data-stu-id="f7785-130">Use the `IHtmlLocalizer<T>` implementation for resources that contain HTML.</span></span> <span data-ttu-id="f7785-131">`IHtmlLocalizer` 會對資源字串中經過格式化的引數進行 HTML 編碼，但不會對資源字串本身進行 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="f7785-131">`IHtmlLocalizer` HTML encodes arguments that are formatted in the resource string, but doesn't HTML encode the resource string itself.</span></span> <span data-ttu-id="f7785-132">在下列醒目提示的範例中，只有 `name` 參數的值經過 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="f7785-132">In the sample highlighted below, only the value of `name` parameter is HTML encoded.</span></span>

[!code-csharp[Main](../fundamentals/localization/sample/Localization/Controllers/BookController.cs?highlight=3,5,20&start=1&end=24)]

<span data-ttu-id="f7785-133">**注意：**一般來說，您只想將文字當地語系化，而不是 HTML。</span><span class="sxs-lookup"><span data-stu-id="f7785-133">**Note:** You generally want to only localize text and not HTML.</span></span>

<span data-ttu-id="f7785-134">您可以在最底層的[相依性插入](dependency-injection.md)中，將 `IStringLocalizerFactory` 移出：</span><span class="sxs-lookup"><span data-stu-id="f7785-134">At the lowest level, you can get `IStringLocalizerFactory` out of [Dependency Injection](dependency-injection.md):</span></span>

[!code-csharp[Main](localization/sample/Localization/Controllers/TestController.cs?start=9&end=26&highlight=7-13)]

<span data-ttu-id="f7785-135">上述程式碼會示範這兩個 Factory Create 方法。</span><span class="sxs-lookup"><span data-stu-id="f7785-135">The code above demonstrates each of the two factory create methods.</span></span>

<span data-ttu-id="f7785-136">您可以依據控制器或區域來分割當地語系化的字串，也可以只用一個容器。</span><span class="sxs-lookup"><span data-stu-id="f7785-136">You can partition your localized strings by controller, area, or have just one container.</span></span> <span data-ttu-id="f7785-137">在範例應用程式中，會針對共用資源使用名為 `SharedResource` 的虛擬類別。</span><span class="sxs-lookup"><span data-stu-id="f7785-137">In the sample app, a dummy class named `SharedResource` is used for shared resources.</span></span>

[!code-csharp[Main](localization/sample/Localization/Resources/SharedResource.cs)]

<span data-ttu-id="f7785-138">有些開發人員會使用 `Startup` 類別來包含全域或共用字串。</span><span class="sxs-lookup"><span data-stu-id="f7785-138">Some developers use the `Startup` class to contain global or shared strings.</span></span> <span data-ttu-id="f7785-139">下列範例會使用 `InfoController` 和 `SharedResource` 當地語系化工具：</span><span class="sxs-lookup"><span data-stu-id="f7785-139">In the sample below, the `InfoController` and the `SharedResource` localizers are used:</span></span>

[!code-csharp[Main](localization/sample/Localization/Controllers/InfoController.cs?range=9-26)]

## <a name="view-localization"></a><span data-ttu-id="f7785-140">檢視當地語系化</span><span class="sxs-lookup"><span data-stu-id="f7785-140">View localization</span></span>

<span data-ttu-id="f7785-141">`IViewLocalizer` 服務可提供[檢視](https://docs.microsoft.com/aspnet/core)的當地語系化字串。</span><span class="sxs-lookup"><span data-stu-id="f7785-141">The `IViewLocalizer` service provides localized strings for a [view](https://docs.microsoft.com/aspnet/core).</span></span> <span data-ttu-id="f7785-142">`ViewLocalizer` 類別會實作這個介面，並透過檢視檔案路徑來找出資源的位置。</span><span class="sxs-lookup"><span data-stu-id="f7785-142">The `ViewLocalizer` class implements this interface and finds the resource location from the view file path.</span></span> <span data-ttu-id="f7785-143">下列程式碼示範如何使用 `IViewLocalizer` 的預設實作：</span><span class="sxs-lookup"><span data-stu-id="f7785-143">The following code shows how to use the default implementation of `IViewLocalizer`:</span></span>

[!code-cshtml[Main](localization/sample/Localization/Views/Home/About.cshtml)]

<span data-ttu-id="f7785-144">`IViewLocalizer` 的預設實作會依據檢視的檔案名稱來找出資源檔。</span><span class="sxs-lookup"><span data-stu-id="f7785-144">The default implementation of `IViewLocalizer` finds the resource file based on the view's file name.</span></span> <span data-ttu-id="f7785-145">其中並沒有任何選項可以使用全域共用的資源檔。</span><span class="sxs-lookup"><span data-stu-id="f7785-145">There's no option to use a global shared resource file.</span></span> <span data-ttu-id="f7785-146">`ViewLocalizer` 會使用 `IHtmlLocalizer` 來實作當地語系化工具，因此 Razor 不會對當地語系化的字串進行 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="f7785-146">`ViewLocalizer` implements the localizer using `IHtmlLocalizer`, so Razor doesn't HTML encode the localized string.</span></span> <span data-ttu-id="f7785-147">您可以參數化資源字串，`IViewLocalizer` 即會對參數 (而不是資源字串) 進行 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="f7785-147">You can parameterize resource strings and `IViewLocalizer` will HTML encode the parameters, but not the resource string.</span></span> <span data-ttu-id="f7785-148">請考慮下列 Razor 標記：</span><span class="sxs-lookup"><span data-stu-id="f7785-148">Consider the following Razor markup:</span></span>

```cshtml
@Localizer["<i>Hello</i> <b>{0}!</b>", UserManager.GetUserName(User)]
```

<span data-ttu-id="f7785-149">法文資源檔可能包含下列內容：</span><span class="sxs-lookup"><span data-stu-id="f7785-149">A French resource file could contain the following:</span></span>

| <span data-ttu-id="f7785-150">Key</span><span class="sxs-lookup"><span data-stu-id="f7785-150">Key</span></span> | <span data-ttu-id="f7785-151">值</span><span class="sxs-lookup"><span data-stu-id="f7785-151">Value</span></span> |
| ----- | ------ |
| `<i>Hello</i> <b>{0}!</b>` | `<i>Bonjour</i> <b>{0} !</b> ` |

<span data-ttu-id="f7785-152">轉譯的檢視內容可能包含來自資源檔的 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="f7785-152">The rendered view would contain the HTML markup from the resource file.</span></span>

<span data-ttu-id="f7785-153">**注意：**一般來說，您只想將文字當地語系化，而不是 HTML。</span><span class="sxs-lookup"><span data-stu-id="f7785-153">**Note:** You generally want to only localize text and not HTML.</span></span>

<span data-ttu-id="f7785-154">若要在檢視中使用共用的資源檔，請插入 `IHtmlLocalizer<T>`：</span><span class="sxs-lookup"><span data-stu-id="f7785-154">To use a shared resource file in a view, inject `IHtmlLocalizer<T>`:</span></span>

[!code-cshtml[Main](../fundamentals/localization/sample/Localization/Views/Test/About.cshtml?highlight=5,12)]

## <a name="dataannotations-localization"></a><span data-ttu-id="f7785-155">DataAnnotations 當地語系化</span><span class="sxs-lookup"><span data-stu-id="f7785-155">DataAnnotations localization</span></span>

<span data-ttu-id="f7785-156">DataAnnotations 錯誤訊息會使用 `IStringLocalizer<T>` 來當地語系化。</span><span class="sxs-lookup"><span data-stu-id="f7785-156">DataAnnotations error messages are localized with `IStringLocalizer<T>`.</span></span> <span data-ttu-id="f7785-157">使用 `ResourcesPath = "Resources"` 選項時，`RegisterViewModel` 中的錯誤訊息會儲存在下列路徑之一：</span><span class="sxs-lookup"><span data-stu-id="f7785-157">Using the option `ResourcesPath = "Resources"`, the error messages in `RegisterViewModel` can be stored in either of the following paths:</span></span>

* <span data-ttu-id="f7785-158">Resources/ViewModels.Account.RegisterViewModel.fr.resx</span><span class="sxs-lookup"><span data-stu-id="f7785-158">Resources/ViewModels.Account.RegisterViewModel.fr.resx</span></span>
* <span data-ttu-id="f7785-159">Resources/ViewModels/Account/RegisterViewModel.fr.resx</span><span class="sxs-lookup"><span data-stu-id="f7785-159">Resources/ViewModels/Account/RegisterViewModel.fr.resx</span></span>

[!code-csharp[Main](localization/sample/Localization/ViewModels/Account/RegisterViewModel.cs?start=9&end=26)]

<span data-ttu-id="f7785-160">在 ASP.NET Core MVC 1.1.0 和更高版本中，系統會將非驗證屬性當地語系化。</span><span class="sxs-lookup"><span data-stu-id="f7785-160">In ASP.NET Core MVC 1.1.0 and higher, non-validation attributes are localized.</span></span> <span data-ttu-id="f7785-161">ASP.NET Core MVC 1.0 **不會**查閱非驗證屬性的當地語系化字串。</span><span class="sxs-lookup"><span data-stu-id="f7785-161">ASP.NET Core MVC 1.0 does **not** look up localized strings for non-validation attributes.</span></span>

<a name="one-resource-string-multiple-classes"></a>
### <a name="using-one-resource-string-for-multiple-classes"></a><span data-ttu-id="f7785-162">針對多個類別使用同一個資源字串</span><span class="sxs-lookup"><span data-stu-id="f7785-162">Using one resource string for multiple classes</span></span>

<span data-ttu-id="f7785-163">下列程式碼會示範如何針對含有多個類別的驗證屬性使用同一個資源字串：</span><span class="sxs-lookup"><span data-stu-id="f7785-163">The following code shows how to use one resource string for validation attributes with multiple classes:</span></span>

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

<span data-ttu-id="f7785-164">在先前的程式碼中，`SharedResource` 是與儲存驗證訊息的 resx 對應的類別。</span><span class="sxs-lookup"><span data-stu-id="f7785-164">In the preceeding code, `SharedResource` is the class corresponding to the resx where your validation messages are stored.</span></span> <span data-ttu-id="f7785-165">使用這個方法時，DataAnnotations 只會使用 `SharedResource`，而不是每個類別的資源。</span><span class="sxs-lookup"><span data-stu-id="f7785-165">With this approach, DataAnnotations will only use `SharedResource`, rather than the resource for each class.</span></span> 

## <a name="provide-localized-resources-for-the-languages-and-cultures-you-support"></a><span data-ttu-id="f7785-166">針對您支援的語言和文化特性提供當地語系化資源</span><span class="sxs-lookup"><span data-stu-id="f7785-166">Provide localized resources for the languages and cultures you support</span></span>  

### <a name="supportedcultures-and-supporteduicultures"></a><span data-ttu-id="f7785-167">SupportedCultures 和 SupportedUICultures</span><span class="sxs-lookup"><span data-stu-id="f7785-167">SupportedCultures and SupportedUICultures</span></span>

<span data-ttu-id="f7785-168">ASP.NET Core 可讓您指定 `SupportedCultures` 和 `SupportedUICultures` 這兩個文化特性值。</span><span class="sxs-lookup"><span data-stu-id="f7785-168">ASP.NET Core allows you to specify two culture values, `SupportedCultures` and `SupportedUICultures`.</span></span> <span data-ttu-id="f7785-169">`SupportedCultures` 的 [CultureInfo](https://docs.microsoft.com/dotnet/api/system.globalization.cultureinfo) 物件可決定文化特性相依函式的結果，例如日期、時間、數字及貨幣格式。</span><span class="sxs-lookup"><span data-stu-id="f7785-169">The [CultureInfo](https://docs.microsoft.com/dotnet/api/system.globalization.cultureinfo) object for `SupportedCultures` determines the results of culture-dependent functions, such as date, time, number, and currency formatting.</span></span> <span data-ttu-id="f7785-170">`SupportedCultures` 也可決定文字排列順序、大小寫慣例和字串比較。</span><span class="sxs-lookup"><span data-stu-id="f7785-170">`SupportedCultures` also determines the sorting order of text, casing conventions, and string comparisons.</span></span> <span data-ttu-id="f7785-171">如需伺服器如何取得文化特性的詳細資訊，請參閱 [CultureInfo.CurrentCulture](https://docs.microsoft.com/dotnet/api/system.stringcomparer.currentculture#System_StringComparer_CurrentCulture)。</span><span class="sxs-lookup"><span data-stu-id="f7785-171">See [CultureInfo.CurrentCulture](https://docs.microsoft.com/dotnet/api/system.stringcomparer.currentculture#System_StringComparer_CurrentCulture) for more info on how the server gets the Culture.</span></span> <span data-ttu-id="f7785-172">`SupportedUICultures` 可決定 [ResourceManager](https://docs.microsoft.com/dotnet/api/system.resources.resourcemanager) 要查閱哪些翻譯的字串 (來自 *.resx* 檔案)。</span><span class="sxs-lookup"><span data-stu-id="f7785-172">The `SupportedUICultures` determines which translates strings (from *.resx* files) are looked up by the [ResourceManager](https://docs.microsoft.com/dotnet/api/system.resources.resourcemanager).</span></span> <span data-ttu-id="f7785-173">`ResourceManager` 僅會查閱 `CurrentUICulture` 所決定之文化特性特有的字串。</span><span class="sxs-lookup"><span data-stu-id="f7785-173">The `ResourceManager` simply looks up culture-specific strings that's determined by `CurrentUICulture`.</span></span> <span data-ttu-id="f7785-174">.NET 中的每個執行緒都有 `CurrentCulture` 和 `CurrentUICulture` 物件。</span><span class="sxs-lookup"><span data-stu-id="f7785-174">Every thread in .NET has `CurrentCulture` and `CurrentUICulture` objects.</span></span> <span data-ttu-id="f7785-175">ASP.NET Core 會在轉譯文化特性相依函式時檢查這些值。</span><span class="sxs-lookup"><span data-stu-id="f7785-175">ASP.NET Core inspects these values when rendering culture-dependent functions.</span></span> <span data-ttu-id="f7785-176">比方說，如果目前執行緒的文化特性設定為 "en-US" (英文 - 美國)，`DateTime.Now.ToLongDateString()` 會顯示 "Thursday, February 18, 2016"，但如果 `CurrentCulture` 設定為 "es-ES" (西班牙文 - 西班牙)，則輸出會是 "jueves, 18 de febrero de 2016"。</span><span class="sxs-lookup"><span data-stu-id="f7785-176">For example, if the current thread's culture is set to "en-US" (English, United States), `DateTime.Now.ToLongDateString()` displays "Thursday, February 18, 2016", but if `CurrentCulture` is set to "es-ES" (Spanish, Spain) the output will be "jueves, 18 de febrero de 2016".</span></span>

## <a name="resource-files"></a><span data-ttu-id="f7785-177">資源檔</span><span class="sxs-lookup"><span data-stu-id="f7785-177">Resource files</span></span>

<span data-ttu-id="f7785-178">資源檔是一種實用的機制，可讓您將可當地語系化的字串與代碼區隔開來。</span><span class="sxs-lookup"><span data-stu-id="f7785-178">A resource file is a useful mechanism for separating localizable strings from code.</span></span> <span data-ttu-id="f7785-179">您可以將非預設語言的翻譯字串作為隔離的 *.resx* 資源檔。</span><span class="sxs-lookup"><span data-stu-id="f7785-179">Translated strings for the non-default language are isolated *.resx* resource files.</span></span> <span data-ttu-id="f7785-180">例如，您可以建立名為 *Welcome.es.resx* 的西班牙文資源檔，以包含翻譯的字串。</span><span class="sxs-lookup"><span data-stu-id="f7785-180">For example, you might want to create Spanish resource file named *Welcome.es.resx* containing translated strings.</span></span> <span data-ttu-id="f7785-181">"es" 是西班牙文的語言代碼。</span><span class="sxs-lookup"><span data-stu-id="f7785-181">"es" is the language code for Spanish.</span></span> <span data-ttu-id="f7785-182">若要在 Visual Studio 中建立這個資源檔：</span><span class="sxs-lookup"><span data-stu-id="f7785-182">To create this resource file in Visual Studio:</span></span>

1. <span data-ttu-id="f7785-183">在方案總管中，以滑鼠右鍵按一下要放置資源檔的資料夾 > [新增] > [新增項目]。</span><span class="sxs-lookup"><span data-stu-id="f7785-183">In **Solution Explorer**, right click on the folder which will contain the resource file > **Add** > **New Item**.</span></span>

    ![巢狀特色選單：方案總管會開啟 [資源] 的特色選單，](localization/_static/newi.png)

2. <span data-ttu-id="f7785-186">在 [Search installed templates] (搜尋已安裝的範本) 方塊中，輸入「資源」，並命名檔案。</span><span class="sxs-lookup"><span data-stu-id="f7785-186">In the **Search installed templates** box, enter "resource" and name the file.</span></span>

    ![[新增項目] 對話方塊](localization/_static/res.png)

3. <span data-ttu-id="f7785-188">在 [名稱] 資料行中輸入索引鍵值 (原生字串)，並在 [值] 資料行中輸入已翻譯的字串。</span><span class="sxs-lookup"><span data-stu-id="f7785-188">Enter the key value (native string) in the **Name** column and the translated string in the **Value** column.</span></span>

    ![Welcome.es.resx 檔案 (西班牙文的「歡迎使用」資源檔)，其中 [名稱] 資料行的文字為 Hello，而 [值] 資料行的文字為 Hola (Hello 的西班牙文)](localization/_static/hola.png)

    <span data-ttu-id="f7785-190">Visual Studio 會顯示 *Welcome.es.resx* 檔案。</span><span class="sxs-lookup"><span data-stu-id="f7785-190">Visual Studio shows the *Welcome.es.resx* file.</span></span>

    ![方案總管，其中顯示「歡迎使用」的西班牙文 (es) 資源檔](localization/_static/se.png)

<a name="error"></a>

<span data-ttu-id="f7785-192">如果您是使用 Visual Studio 2017 Preview 15.3 版，即會在資源編輯器中收到錯誤指標。</span><span class="sxs-lookup"><span data-stu-id="f7785-192">If you are using Visual Studio 2017 Preview version 15.3, you'll get an error indicator in the resource editor.</span></span> <span data-ttu-id="f7785-193">若要避免這個錯誤訊息，請從「自訂工具」屬性方格裡，移除 *ResXFileCodeGenerator* 值：</span><span class="sxs-lookup"><span data-stu-id="f7785-193">Remove the *ResXFileCodeGenerator*  value from the *Custom Tool* properties grid to prevent this error message:</span></span>

![Resx 編輯器](localization/_static/err.png)

<span data-ttu-id="f7785-195">或者，您可以忽略這個錯誤。</span><span class="sxs-lookup"><span data-stu-id="f7785-195">Alternatively, you can ignore this error.</span></span> <span data-ttu-id="f7785-196">我們希望下一個版本能修正這個問題。</span><span class="sxs-lookup"><span data-stu-id="f7785-196">We hope to fix this in the next release.</span></span>

## <a name="resource-file-naming"></a><span data-ttu-id="f7785-197">資源檔命名</span><span class="sxs-lookup"><span data-stu-id="f7785-197">Resource file naming</span></span>

<span data-ttu-id="f7785-198">資源的命名方式是以其類別的完整類型名稱去掉組件名稱而得。</span><span class="sxs-lookup"><span data-stu-id="f7785-198">Resources are named for the full type name of their class minus the assembly name.</span></span> <span data-ttu-id="f7785-199">例如，假設專案中的法文資源是 `LocalizationWebsite.Web.Startup` 類別、主要組件為 `LocalizationWebsite.Web.dll`，就會命名為 *Startup.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="f7785-199">For example, a French resource in a project whose main assembly is `LocalizationWebsite.Web.dll` for the class `LocalizationWebsite.Web.Startup` would be named *Startup.fr.resx*.</span></span> <span data-ttu-id="f7785-200">若是 `LocalizationWebsite.Web.Controllers.HomeController` 類別的資源，則應命名為 *Controllers.HomeController.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="f7785-200">A resource for the class `LocalizationWebsite.Web.Controllers.HomeController` would be named *Controllers.HomeController.fr.resx*.</span></span> <span data-ttu-id="f7785-201">如果目標類別的命名空間和組件名稱不相同，則需要使用完整類型名稱。</span><span class="sxs-lookup"><span data-stu-id="f7785-201">If your targeted class's namespace isn't the same as the assembly name you will need the full type name.</span></span> <span data-ttu-id="f7785-202">比方說，範例專案中 `ExtraNamespace.Tools` 類型的資源會命名為 *ExtraNamespace.Tools.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="f7785-202">For example, in the sample project a resource for the type `ExtraNamespace.Tools` would be named *ExtraNamespace.Tools.fr.resx*.</span></span>

<span data-ttu-id="f7785-203">在範例專案中，`ConfigureServices` 方法會將 `ResourcesPath` 設為 "Resources"，因此首頁控制器的法文資源檔專案相對路徑即為 *Resources/Controllers.HomeController.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="f7785-203">In the sample project, the `ConfigureServices` method sets the `ResourcesPath` to "Resources", so the project relative path for the home controller's French resource file is *Resources/Controllers.HomeController.fr.resx*.</span></span> <span data-ttu-id="f7785-204">或者，您可以使用資料夾來收集資源檔。</span><span class="sxs-lookup"><span data-stu-id="f7785-204">Alternatively, you can use folders to organize resource files.</span></span> <span data-ttu-id="f7785-205">若是首頁控制器，路徑就是 *Resources/Controllers/HomeController.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="f7785-205">For the home controller, the path would be *Resources/Controllers/HomeController.fr.resx*.</span></span> <span data-ttu-id="f7785-206">如果您不使用 `ResourcesPath` 選項，*.resx* 檔案即會放置在專案的基底目錄中。</span><span class="sxs-lookup"><span data-stu-id="f7785-206">If you don't use the `ResourcesPath` option, the *.resx* file would go in the project base directory.</span></span> <span data-ttu-id="f7785-207">`HomeController` 的資源檔會命名為 *Controllers.HomeController.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="f7785-207">The resource file for `HomeController` would be named *Controllers.HomeController.fr.resx*.</span></span> <span data-ttu-id="f7785-208">您可依據自己的資源檔收集方式，來選擇要使用點或路徑的命名慣例。</span><span class="sxs-lookup"><span data-stu-id="f7785-208">The choice of using the dot or path naming convention depends on how you want to organize your resource files.</span></span>

| <span data-ttu-id="f7785-209">資源名稱</span><span class="sxs-lookup"><span data-stu-id="f7785-209">Resource name</span></span> | <span data-ttu-id="f7785-210">點或路徑命名</span><span class="sxs-lookup"><span data-stu-id="f7785-210">Dot or path naming</span></span> |
| ------------   | ------------- |
| <span data-ttu-id="f7785-211">Resources/Controllers.HomeController.fr.resx</span><span class="sxs-lookup"><span data-stu-id="f7785-211">Resources/Controllers.HomeController.fr.resx</span></span> | <span data-ttu-id="f7785-212">點</span><span class="sxs-lookup"><span data-stu-id="f7785-212">Dot</span></span>  |
| <span data-ttu-id="f7785-213">Resources/Controllers/HomeController.fr.resx</span><span class="sxs-lookup"><span data-stu-id="f7785-213">Resources/Controllers/HomeController.fr.resx</span></span>  | <span data-ttu-id="f7785-214">路徑</span><span class="sxs-lookup"><span data-stu-id="f7785-214">Path</span></span> |
|    |     |

<span data-ttu-id="f7785-215">如果資源檔是使用 Razor 檢視中的 `@inject IViewLocalizer`，亦遵循類似的模式。</span><span class="sxs-lookup"><span data-stu-id="f7785-215">Resource files using `@inject IViewLocalizer` in Razor views follow a similar pattern.</span></span> <span data-ttu-id="f7785-216">您可以使用點命名或路徑命名方式，來命名檢視的資源檔。</span><span class="sxs-lookup"><span data-stu-id="f7785-216">The resource file for a view can be named using either dot naming or path naming.</span></span> <span data-ttu-id="f7785-217">Razor 檢視的資源檔會模仿其相關聯檢視檔案的路徑。</span><span class="sxs-lookup"><span data-stu-id="f7785-217">Razor view resource files mimic the path of their associated view file.</span></span> <span data-ttu-id="f7785-218">假設我們將 `ResourcesPath` 設為 "Resources"，與 *Views/Home/About.cshtml* 檢視建立關聯的法文資源檔可為下列其一：</span><span class="sxs-lookup"><span data-stu-id="f7785-218">Assuming we set the `ResourcesPath` to "Resources", the French resource file associated with the *Views/Home/About.cshtml* view could be either of the following:</span></span>

* <span data-ttu-id="f7785-219">Resources/Views/Home/About.fr.resx</span><span class="sxs-lookup"><span data-stu-id="f7785-219">Resources/Views/Home/About.fr.resx</span></span>

* <span data-ttu-id="f7785-220">Resources/Views.Home.About.fr.resx</span><span class="sxs-lookup"><span data-stu-id="f7785-220">Resources/Views.Home.About.fr.resx</span></span>

<span data-ttu-id="f7785-221">如果您不使用 `ResourcesPath` 選項，則檢視的 *.resx* 檔案會與檢視位於相同資料夾中。</span><span class="sxs-lookup"><span data-stu-id="f7785-221">If you don't use the `ResourcesPath` option, the *.resx* file for a view would be located in the same folder as the view.</span></span>

## <a name="culture-fallback-behavior"></a><span data-ttu-id="f7785-222">文化特性後援行為</span><span class="sxs-lookup"><span data-stu-id="f7785-222">Culture fallback behavior</span></span>

<span data-ttu-id="f7785-223">舉例來說，如果您移除 ".fr" 文化特性指示項，並將文化特性設定為法文，則系統會讀取預設資源檔，並將字串當地語系化。</span><span class="sxs-lookup"><span data-stu-id="f7785-223">As an example, if you remove the ".fr" culture designator and you have the culture set to French, the default resource file is read and strings are localized.</span></span> <span data-ttu-id="f7785-224">當沒有任何項目符合您要求的文化特性時，資源管理員即會指定預設資源或後援資源。</span><span class="sxs-lookup"><span data-stu-id="f7785-224">The Resource manager designates a default or fallback resource for when nothing meets your requested culture.</span></span> <span data-ttu-id="f7785-225">如果您只想在要求的文化特性缺少資源時傳回索引鍵，就不能使用預設資源檔。</span><span class="sxs-lookup"><span data-stu-id="f7785-225">If you want to just return the key when missing a resource for the requested culture you must not have a default resource file.</span></span>

### <a name="generate-resource-files-with-visual-studio"></a><span data-ttu-id="f7785-226">使用 Visual Studio 產生資源檔</span><span class="sxs-lookup"><span data-stu-id="f7785-226">Generate resource files with Visual Studio</span></span>

<span data-ttu-id="f7785-227">如果您在 Visual Studio 中建立資源檔，但檔案名稱中不含文化特性 (例如 *Welcome.resx*)，則 Visual Studio 會針對每個字串的屬性建立 C# 類別。</span><span class="sxs-lookup"><span data-stu-id="f7785-227">If you create a resource file in Visual Studio without a culture in the file name (for example, *Welcome.resx*), Visual Studio will create a C# class with a property for each string.</span></span> <span data-ttu-id="f7785-228">但這通常不是您使用 ASP.NET Core 的初衷；一般來說，您不會有預設的 *.resx* 資源檔 (不含文化特性名稱的 *.resx* 檔案)。</span><span class="sxs-lookup"><span data-stu-id="f7785-228">That's usually not what you want with ASP.NET Core; you typically don't have a default *.resx* resource file (A *.resx* file without the culture name).</span></span> <span data-ttu-id="f7785-229">因此，建議您建立含有文化特性名稱的 *.resx* 檔案 (例如 *Welcome.fr.resx*)。</span><span class="sxs-lookup"><span data-stu-id="f7785-229">We suggest you create the *.resx* file with a culture name (for example *Welcome.fr.resx*).</span></span> <span data-ttu-id="f7785-230">當您建立含有文化特性名稱的 *.resx* 檔案時，Visual Studio 就不會產生類別檔案。</span><span class="sxs-lookup"><span data-stu-id="f7785-230">When you create a *.resx* file with a culture name, Visual Studio won't generate the class file.</span></span> <span data-ttu-id="f7785-231">我們認為大多數開發人員**不需建立**預設的語言資源檔案。</span><span class="sxs-lookup"><span data-stu-id="f7785-231">We anticipate that many developers will **not** create a default language resource file.</span></span>

### <a name="add-other-cultures"></a><span data-ttu-id="f7785-232">新增其他文化特性</span><span class="sxs-lookup"><span data-stu-id="f7785-232">Add Other Cultures</span></span>

<span data-ttu-id="f7785-233">每種語言和文化特性的組合 (非預設語言) 都需要唯一的資源檔。</span><span class="sxs-lookup"><span data-stu-id="f7785-233">Each language and culture combination (other than the default language) requires a unique resource file.</span></span> <span data-ttu-id="f7785-234">若要建立不同文化特性和地區設定的資源檔，您可以建立新的資源檔並將 ISO 語言代碼作為檔名的一部分 (例如 **en-us**、**fr-ca** 和 **en-gb**)。</span><span class="sxs-lookup"><span data-stu-id="f7785-234">You create resource files for different cultures and locales by creating new resource files in which the ISO language codes are part of the file name (for example, **en-us**, **fr-ca**, and **en-gb**).</span></span> <span data-ttu-id="f7785-235">您應將 ISO 代碼置於檔案名稱和 *.resx* 副檔名之間，例如 *Welcome.es MX.resx* (西班牙文/墨西哥)。</span><span class="sxs-lookup"><span data-stu-id="f7785-235">These ISO codes are placed between the file name and the *.resx* file name extension, as in *Welcome.es-MX.resx* (Spanish/Mexico).</span></span> <span data-ttu-id="f7785-236">若要指定與文化特性無關的語言，請移除國碼 (地區碼)，亦即上述範例中的 `MX`。</span><span class="sxs-lookup"><span data-stu-id="f7785-236">To specify a culturally neutral language, remove the country code (`MX` in the preceding example).</span></span> <span data-ttu-id="f7785-237">如果是與文化特性無關的西班牙文資源檔案，其名稱為 *Welcome.es.resx*。</span><span class="sxs-lookup"><span data-stu-id="f7785-237">The culturally neutral Spanish resource file name is *Welcome.es.resx*.</span></span>

## <a name="implement-a-strategy-to-select-the-languageculture-for-each-request"></a><span data-ttu-id="f7785-238">實作可依據每項要求選取語言/文化特性的策略</span><span class="sxs-lookup"><span data-stu-id="f7785-238">Implement a strategy to select the language/culture for each request</span></span>  

### <a name="configure-localization"></a><span data-ttu-id="f7785-239">設定當地語系化</span><span class="sxs-lookup"><span data-stu-id="f7785-239">Configure localization</span></span>

<span data-ttu-id="f7785-240">您可以在 `ConfigureServices` 方法中設定當地語系化：</span><span class="sxs-lookup"><span data-stu-id="f7785-240">Localization is configured in the `ConfigureServices` method:</span></span>

[!code-csharp[Main](localization/sample/Localization/Program.cs?name=snippet1)]

* <span data-ttu-id="f7785-241">`AddLocalization` 可將當地語系化服務新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="f7785-241">`AddLocalization` Adds the localization services to the services container.</span></span> <span data-ttu-id="f7785-242">上方的程式碼也會將資源路徑設為 "Resources"。</span><span class="sxs-lookup"><span data-stu-id="f7785-242">The code above also sets the resources path to "Resources".</span></span>

* <span data-ttu-id="f7785-243">`AddViewLocalization` 可支援當地語系化的檢視檔案。</span><span class="sxs-lookup"><span data-stu-id="f7785-243">`AddViewLocalization` Adds support for localized view files.</span></span> <span data-ttu-id="f7785-244">在此範例中，檢視的當地語系化會以檢視檔案的後置詞為依據。</span><span class="sxs-lookup"><span data-stu-id="f7785-244">In this sample view localization is based on the view file suffix.</span></span> <span data-ttu-id="f7785-245">例如 *Index.fr.cshtml* 檔案中的 "fr"。</span><span class="sxs-lookup"><span data-stu-id="f7785-245">For example "fr" in the *Index.fr.cshtml* file.</span></span>

* <span data-ttu-id="f7785-246">`AddDataAnnotationsLocalization` 可支援透過 `IStringLocalizer` 抽象概念而來的當地語系化 `DataAnnotations` 驗證訊息。</span><span class="sxs-lookup"><span data-stu-id="f7785-246">`AddDataAnnotationsLocalization` Adds support for localized `DataAnnotations` validation messages through `IStringLocalizer` abstractions.</span></span>

### <a name="localization-middleware"></a><span data-ttu-id="f7785-247">當地語系化中介軟體</span><span class="sxs-lookup"><span data-stu-id="f7785-247">Localization middleware</span></span>

<span data-ttu-id="f7785-248">您可以在當地語系化[中介軟體](middleware.md)中，設定要求目前的文化特性。</span><span class="sxs-lookup"><span data-stu-id="f7785-248">The current culture on a request is set in the localization [Middleware](middleware.md).</span></span> <span data-ttu-id="f7785-249">在 *Program.cs* 檔案的 `Configure` 方法中，已啟用當地語系化中介軟體。</span><span class="sxs-lookup"><span data-stu-id="f7785-249">The localization middleware is enabled in the `Configure` method of *Program.cs* file.</span></span> <span data-ttu-id="f7785-250">請注意，您必須在任何可能會檢查要求的文化特性的中介軟體之前，設定當地語系化中介軟體 (例如 `app.UseMvcWithDefaultRoute()`)。</span><span class="sxs-lookup"><span data-stu-id="f7785-250">Note, the localization middleware must be configured before any middleware which might check the request culture (for example, `app.UseMvcWithDefaultRoute()`).</span></span>

[!code-csharp[Main](localization/sample/Localization/Program.cs?name=snippet2)]

<span data-ttu-id="f7785-251">`UseRequestLocalization` 會初始化 `RequestLocalizationOptions` 物件。</span><span class="sxs-lookup"><span data-stu-id="f7785-251">`UseRequestLocalization` initializes a `RequestLocalizationOptions` object.</span></span> <span data-ttu-id="f7785-252">在每次要求時，系統會列舉 `RequestLocalizationOptions` 中的 `RequestCultureProvider` 清單，並使用能成功判斷要求的文化特性的第一個提供者。</span><span class="sxs-lookup"><span data-stu-id="f7785-252">On every request the list of `RequestCultureProvider` in the `RequestLocalizationOptions` is enumerated and the first provider that can successfully determine the request culture is used.</span></span> <span data-ttu-id="f7785-253">預設的提供者是來自 `RequestLocalizationOptions` 類別：</span><span class="sxs-lookup"><span data-stu-id="f7785-253">The default providers come from the `RequestLocalizationOptions` class:</span></span>

1. `QueryStringRequestCultureProvider`
2. `CookieRequestCultureProvider`
3. `AcceptLanguageHeaderRequestCultureProvider`

<span data-ttu-id="f7785-254">預設清單會由針對性高到低來排列。</span><span class="sxs-lookup"><span data-stu-id="f7785-254">The default list goes from most specific to least specific.</span></span> <span data-ttu-id="f7785-255">在本文稍後，我們會說明如何變更順序，甚至新增自訂的文化特性提供者。</span><span class="sxs-lookup"><span data-stu-id="f7785-255">Later in the article we'll see how you can change the order and even add a custom culture provider.</span></span> <span data-ttu-id="f7785-256">如果沒有任何提供者可以判斷要求的文化特性，即會使用 `DefaultRequestCulture`。</span><span class="sxs-lookup"><span data-stu-id="f7785-256">If none of the providers can determine the request culture, the `DefaultRequestCulture` is used.</span></span>

### <a name="querystringrequestcultureprovider"></a><span data-ttu-id="f7785-257">QueryStringRequestCultureProvider</span><span class="sxs-lookup"><span data-stu-id="f7785-257">QueryStringRequestCultureProvider</span></span>

<span data-ttu-id="f7785-258">有些應用程式會使用查詢字串來設定[文化特性和 UI 文化特性](https://msdn.microsoft.com/library/system.globalization.cultureinfo.aspx)。</span><span class="sxs-lookup"><span data-stu-id="f7785-258">Some apps will use a query string to set the [culture and UI culture](https://msdn.microsoft.com/library/system.globalization.cultureinfo.aspx).</span></span> <span data-ttu-id="f7785-259">若是使用 Cookie 或 Accept-Language 標頭方法的應用程式，您可以將查詢字串新增至 URL 以偵錯和測試程式碼。</span><span class="sxs-lookup"><span data-stu-id="f7785-259">For apps that use the cookie or Accept-Language header approach, adding a query string to the URL is useful for debugging and testing code.</span></span> <span data-ttu-id="f7785-260">系統預設會將 `QueryStringRequestCultureProvider` 登錄為 `RequestCultureProvider` 清單中的第一個當地語系化提供者。</span><span class="sxs-lookup"><span data-stu-id="f7785-260">By default, the `QueryStringRequestCultureProvider` is registered as the first localization provider in the `RequestCultureProvider` list.</span></span> <span data-ttu-id="f7785-261">您應傳遞查詢字串參數 `culture` 和 `ui-culture`。</span><span class="sxs-lookup"><span data-stu-id="f7785-261">You pass the query string parameters `culture` and `ui-culture`.</span></span> <span data-ttu-id="f7785-262">下列範例會設定西班牙文/墨西哥的特定文化特性 (語言和地區)：</span><span class="sxs-lookup"><span data-stu-id="f7785-262">The following example sets the specific culture (language and region) to Spanish/Mexico:</span></span>

   `http://localhost:5000/?culture=es-MX&ui-culture=es-MX`

<span data-ttu-id="f7785-263">如果您只傳入兩者其一 (`culture` 或 `ui-culture`)，查詢字串提供者就會使用您傳入的項目來設定這兩個值。</span><span class="sxs-lookup"><span data-stu-id="f7785-263">If you only pass in one of the two (`culture` or `ui-culture`), the query string provider will set both values using the one you passed in.</span></span> <span data-ttu-id="f7785-264">例如，若只設定文化特性，即會同時設定 `Culture` 和 `UICulture`：</span><span class="sxs-lookup"><span data-stu-id="f7785-264">For example, setting just the culture will set both the `Culture` and the `UICulture`:</span></span>

   `http://localhost:5000/?culture=es-MX`

### <a name="cookierequestcultureprovider"></a><span data-ttu-id="f7785-265">CookieRequestCultureProvider</span><span class="sxs-lookup"><span data-stu-id="f7785-265">CookieRequestCultureProvider</span></span>

<span data-ttu-id="f7785-266">生產環境應用程式通常會提供一個機制，來設定 ASP.NET Core 文化特性 Cookie 的文化特性。</span><span class="sxs-lookup"><span data-stu-id="f7785-266">Production apps will often provide a mechanism to set the culture with the ASP.NET Core culture cookie.</span></span> <span data-ttu-id="f7785-267">若要建立 Cookie，請使用 `MakeCookieValue` 方法。</span><span class="sxs-lookup"><span data-stu-id="f7785-267">Use the `MakeCookieValue` method to create a cookie.</span></span>

<span data-ttu-id="f7785-268">`CookieRequestCultureProvider` `DefaultCookieName` 會傳回預設 Cookie 名稱，以用來追蹤使用者的慣用文化特性資訊。</span><span class="sxs-lookup"><span data-stu-id="f7785-268">The `CookieRequestCultureProvider` `DefaultCookieName` returns the default cookie name used to track the user's preferred culture information.</span></span> <span data-ttu-id="f7785-269">預設 Cookie 名稱為 `.AspNetCore.Culture`。</span><span class="sxs-lookup"><span data-stu-id="f7785-269">The default cookie name is `.AspNetCore.Culture`.</span></span>

<span data-ttu-id="f7785-270">Cookie 格式為 `c=%LANGCODE%|uic=%LANGCODE%`，其中 `c` 是 `Culture` 而 `uic` 是 `UICulture`，例如：</span><span class="sxs-lookup"><span data-stu-id="f7785-270">The cookie format is `c=%LANGCODE%|uic=%LANGCODE%`, where `c` is `Culture` and `uic` is `UICulture`, for example:</span></span>

    c=en-UK|uic=en-US

<span data-ttu-id="f7785-271">如果您只指定文化特性資訊和 UI 文化特性其一，系統就會將您所指定的文化特性用於文化特性資訊和 UI 文化特性。</span><span class="sxs-lookup"><span data-stu-id="f7785-271">If you only specify one of culture info and UI culture, the specified culture will be used for both culture info and UI culture.</span></span>

### <a name="the-accept-language-http-header"></a><span data-ttu-id="f7785-272">Accept-Language HTTP 標頭</span><span class="sxs-lookup"><span data-stu-id="f7785-272">The Accept-Language HTTP header</span></span>

<span data-ttu-id="f7785-273">您可在大多數的瀏覽器中設定 [Accept-Language 標頭](https://www.w3.org/International/questions/qa-accept-lang-locales)，其最初的設計目的是用來指定使用者的語言。</span><span class="sxs-lookup"><span data-stu-id="f7785-273">The [Accept-Language header](https://www.w3.org/International/questions/qa-accept-lang-locales) is settable in most browsers and was originally intended to specify the user's language.</span></span> <span data-ttu-id="f7785-274">這項設定可指出瀏覽器已設好要傳送哪些項目，或已從基礎作業系統繼承哪些項目。</span><span class="sxs-lookup"><span data-stu-id="f7785-274">This setting indicates what the browser has been set to send or has inherited from the underlying operating system.</span></span> <span data-ttu-id="f7785-275">透過瀏覽器要求的 Accept-Language HTTP 標頭來偵測使用者的慣用語言，並非萬無一失 (請參閱 [Setting language preferences in a browser](https://www.w3.org/International/questions/qa-lang-priorities.en.php) (在瀏覽器中設定語言喜好設定)。</span><span class="sxs-lookup"><span data-stu-id="f7785-275">The Accept-Language HTTP header from a browser request isn't an infallible way to detect the user's preferred language (see [Setting language preferences in a browser](https://www.w3.org/International/questions/qa-lang-priorities.en.php)).</span></span> <span data-ttu-id="f7785-276">生產環境應用程式應該包含可讓使用者自訂文化特性的選擇方式。</span><span class="sxs-lookup"><span data-stu-id="f7785-276">A production app should include a way for a user to customize their choice of culture.</span></span>

### <a name="set-the-accept-language-http-header-in-ie"></a><span data-ttu-id="f7785-277">在 IE 中設定 Accept-Language HTTP 標頭</span><span class="sxs-lookup"><span data-stu-id="f7785-277">Set the Accept-Language HTTP header in IE</span></span>

1. <span data-ttu-id="f7785-278">從齒輪圖示，點選 [網際網路選項]。</span><span class="sxs-lookup"><span data-stu-id="f7785-278">From the gear icon, tap **Internet Options**.</span></span>

2. <span data-ttu-id="f7785-279">點選 [語言]。</span><span class="sxs-lookup"><span data-stu-id="f7785-279">Tap **Languages**.</span></span>

    ![網際網路選項](localization/_static/lang.png)

3. <span data-ttu-id="f7785-281">點選 [設定語言喜好設定]。</span><span class="sxs-lookup"><span data-stu-id="f7785-281">Tap **Set Language Preferences**.</span></span>

4. <span data-ttu-id="f7785-282">點選 [新增語言]。</span><span class="sxs-lookup"><span data-stu-id="f7785-282">Tap **Add a language**.</span></span>

5. <span data-ttu-id="f7785-283">新增語言。</span><span class="sxs-lookup"><span data-stu-id="f7785-283">Add the language.</span></span>

6. <span data-ttu-id="f7785-284">點選語言，然後點選 [上移]。</span><span class="sxs-lookup"><span data-stu-id="f7785-284">Tap the language, then tap **Move Up**.</span></span>

### <a name="use-a-custom-provider"></a><span data-ttu-id="f7785-285">使用自訂提供者</span><span class="sxs-lookup"><span data-stu-id="f7785-285">Use a custom provider</span></span>

<span data-ttu-id="f7785-286">假設您想要讓客戶將他們的語言和文化特性儲存在您的資料庫中。</span><span class="sxs-lookup"><span data-stu-id="f7785-286">Suppose you want to let your customers store their language and culture in your databases.</span></span> <span data-ttu-id="f7785-287">您可以撰寫提供者，以供使用者查詢這些值。</span><span class="sxs-lookup"><span data-stu-id="f7785-287">You could write a provider to look up these values for the user.</span></span> <span data-ttu-id="f7785-288">下列程式碼會示範如何新增自訂提供者：</span><span class="sxs-lookup"><span data-stu-id="f7785-288">The following code shows how to add a custom provider:</span></span>

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

<span data-ttu-id="f7785-289">使用 `RequestLocalizationOptions` 新增或移除當地語系化提供者。</span><span class="sxs-lookup"><span data-stu-id="f7785-289">Use `RequestLocalizationOptions` to add or remove localization providers.</span></span>

### <a name="set-the-culture-programmatically"></a><span data-ttu-id="f7785-290">以程式設計方式來設定文化特性</span><span class="sxs-lookup"><span data-stu-id="f7785-290">Set the culture programmatically</span></span>

<span data-ttu-id="f7785-291">[GitHub](https://github.com/aspnet/entropy) 上的這個範例 **Localization.StarterWeb** 專案包含可設定 `Culture` 的 UI。</span><span class="sxs-lookup"><span data-stu-id="f7785-291">This sample **Localization.StarterWeb** project on [GitHub](https://github.com/aspnet/entropy) contains UI to set the `Culture`.</span></span> <span data-ttu-id="f7785-292">*Views/Shared/_SelectLanguagePartial.cshtml* 檔可讓您從支援的文化特性清單中選取文化特性：</span><span class="sxs-lookup"><span data-stu-id="f7785-292">The *Views/Shared/_SelectLanguagePartial.cshtml* file allows you to select the culture from the list of supported cultures:</span></span>

[!code-cshtml[Main](localization/sample/Localization/Views/Shared/_SelectLanguagePartial.cshtml)]

<span data-ttu-id="f7785-293">系統會將 *Views/Shared/_SelectLanguagePartial.cshtml* 檔案新增至配置檔案的 `footer` 區段，以供所有檢視使用：</span><span class="sxs-lookup"><span data-stu-id="f7785-293">The *Views/Shared/_SelectLanguagePartial.cshtml* file is added to the `footer` section of the layout file so it will be available to all views:</span></span>

[!code-cshtml[Main](localization/sample/Localization/Views/Shared/_Layout.cshtml?range=43-56&highlight=10)]

<span data-ttu-id="f7785-294">`SetLanguage` 方法會設定文化特性的 Cookie。</span><span class="sxs-lookup"><span data-stu-id="f7785-294">The `SetLanguage` method sets the culture cookie.</span></span>

[!code-csharp[Main](localization/sample/Localization/Controllers/HomeController.cs?range=57-67)]

<span data-ttu-id="f7785-295">您無法將 *_SelectLanguagePartial.cshtml* 插入這個專案的範例程式碼。</span><span class="sxs-lookup"><span data-stu-id="f7785-295">You can't plug in the *_SelectLanguagePartial.cshtml* to sample code for this project.</span></span> <span data-ttu-id="f7785-296">[GitHub](https://github.com/aspnet/entropy) 上的範例 **Localization.StarterWeb** 專案，其中的程式碼會部分透過[相依性插入](dependency-injection.md)容器將 `RequestLocalizationOptions` 流向 Razor。</span><span class="sxs-lookup"><span data-stu-id="f7785-296">The **Localization.StarterWeb** project on [GitHub](https://github.com/aspnet/entropy) has code to flow the `RequestLocalizationOptions` to a Razor partial through the [Dependency Injection](dependency-injection.md) container.</span></span>

## <a name="globalization-and-localization-terms"></a><span data-ttu-id="f7785-297">全球化和當地語系化詞彙</span><span class="sxs-lookup"><span data-stu-id="f7785-297">Globalization and localization terms</span></span>

<span data-ttu-id="f7785-298">在進行應用程式的當地語系化程序時，您也需要具備現代軟體開發中常用的相關字元集基本知識，並了解與它們建立關聯的問題。</span><span class="sxs-lookup"><span data-stu-id="f7785-298">The process of localizing your app also requires a basic understanding of relevant character sets commonly used in modern software development and an understanding of the issues associated with them.</span></span> <span data-ttu-id="f7785-299">雖然所有電腦都會將文字儲存為數字 (程式碼)，但不同系統會使用不同數字來儲存相同的文字。</span><span class="sxs-lookup"><span data-stu-id="f7785-299">Although all computers store text as numbers (codes), different systems store the same text using different numbers.</span></span> <span data-ttu-id="f7785-300">當地語系化是指針對特定文化特性/地區設定，轉譯應用程式使用者介面 (UI) 的程序。</span><span class="sxs-lookup"><span data-stu-id="f7785-300">The localization process refers to translating the app user interface (UI) for a specific culture/locale.</span></span>

<span data-ttu-id="f7785-301">[可當地語系化](https://docs.microsoft.com/dotnet/standard/globalization-localization/localizability-review)是確認全球化應用程式已準備好進行當地語系化的中繼程序。</span><span class="sxs-lookup"><span data-stu-id="f7785-301">[Localizability](https://docs.microsoft.com/dotnet/standard/globalization-localization/localizability-review) is an intermediate process for verifying that a globalized app is ready for localization.</span></span>

<span data-ttu-id="f7785-302">文化特性名稱的 [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) 格式是 `<languagecode2>-<country/regioncode2>`，其中 `<languagecode2>` 是語言代碼，而 `<country/regioncode2>` 是子文化特性代碼。</span><span class="sxs-lookup"><span data-stu-id="f7785-302">The [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) format for the culture name is `<languagecode2>-<country/regioncode2>`, where `<languagecode2>` is the language code and `<country/regioncode2>` is the subculture code.</span></span> <span data-ttu-id="f7785-303">例如，`es-CL` 是指西班牙文 (智利)，`en-US` 是指英文 (美國)，而 `en-AU` 是指英文 (澳大利亞)。</span><span class="sxs-lookup"><span data-stu-id="f7785-303">For example, `es-CL` for Spanish (Chile), `en-US` for English (United States), and `en-AU` for English (Australia).</span></span> <span data-ttu-id="f7785-304">[RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) 是 ISO 639 (兩個小寫字母，為與某個語言建立關聯的文化特性代碼) 及 ISO 3166 (兩個大寫字母，為與某個國家或地區建立關聯的子文化特性代碼) 的組合。</span><span class="sxs-lookup"><span data-stu-id="f7785-304">[RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) is a combination of an ISO 639 two-letter lowercase culture code associated with a language and an ISO 3166 two-letter uppercase subculture code associated with a country or region.</span></span> <span data-ttu-id="f7785-305">請參閱 [Language Culture Name](https://msdn.microsoft.com/library/ee825488(v=cs.20).aspx) (語言的文化特性名稱)。</span><span class="sxs-lookup"><span data-stu-id="f7785-305">See [Language Culture Name](https://msdn.microsoft.com/library/ee825488(v=cs.20).aspx).</span></span>

<span data-ttu-id="f7785-306">國際化通常縮寫為 "I18N"。</span><span class="sxs-lookup"><span data-stu-id="f7785-306">Internationalization is often abbreviated to "I18N".</span></span> <span data-ttu-id="f7785-307">這個縮寫是採用該詞彙的第一個和最後一個字母，以及這兩個字母間的字母數組成，因此 18 代表第一個字母 "I" 及最後 "N" 中間的字母數。</span><span class="sxs-lookup"><span data-stu-id="f7785-307">The abbreviation takes the first and last letters and the number of letters between them, so 18 stands for the number of letters between the first "I" and the last "N".</span></span> <span data-ttu-id="f7785-308">同樣的原則也適用於全球化 (G11N) 與當地語系化 (L10N) 的縮寫。</span><span class="sxs-lookup"><span data-stu-id="f7785-308">The same applies to Globalization (G11N), and Localization (L10N).</span></span>

<span data-ttu-id="f7785-309">詞彙：</span><span class="sxs-lookup"><span data-stu-id="f7785-309">Terms:</span></span>

* <span data-ttu-id="f7785-310">全球化 (G11N)：讓應用程式支援不同語言和區域的程序。</span><span class="sxs-lookup"><span data-stu-id="f7785-310">Globalization (G11N): The process of making an app support different languages and regions.</span></span>
* <span data-ttu-id="f7785-311">當地語系化 (L10N)：針對特定語言和區域，自訂應用程式的程序。</span><span class="sxs-lookup"><span data-stu-id="f7785-311">Localization (L10N): The process of customizing an app for a given language and region.</span></span>
* <span data-ttu-id="f7785-312">國際化 (I18N)：包含全球化和當地語系化這兩部分的描述。</span><span class="sxs-lookup"><span data-stu-id="f7785-312">Internationalization (I18N): Describes both globalization and localization.</span></span>
* <span data-ttu-id="f7785-313">文化特性：它是一種語言和/或地區。</span><span class="sxs-lookup"><span data-stu-id="f7785-313">Culture: It's a language and, optionally, a region.</span></span>
* <span data-ttu-id="f7785-314">中性文化特性：具有指定的語言但不限地區的文化特性 </span><span class="sxs-lookup"><span data-stu-id="f7785-314">Neutral culture: A culture that has a specified language, but not a region.</span></span> <span data-ttu-id="f7785-315">(例如 "en"、"es")。</span><span class="sxs-lookup"><span data-stu-id="f7785-315">(for example "en", "es")</span></span>
* <span data-ttu-id="f7785-316">特定文化特性：具有指定的語言和地區的文化特性 </span><span class="sxs-lookup"><span data-stu-id="f7785-316">Specific culture: A culture that has a specified language and region.</span></span> <span data-ttu-id="f7785-317">(例如 "en-US"、"en-GB"、"es-CL")。</span><span class="sxs-lookup"><span data-stu-id="f7785-317">(for example "en-US", "en-GB", "es-CL")</span></span>
* <span data-ttu-id="f7785-318">父文化特性：包含特定文化特性的中性文化特性 </span><span class="sxs-lookup"><span data-stu-id="f7785-318">Parent culture: The neutral culture that contains a specific culture.</span></span> <span data-ttu-id="f7785-319">(例如，"en" 是 "en-US" 和 "en-GB" 的父文化特性)。</span><span class="sxs-lookup"><span data-stu-id="f7785-319">(for example, "en" is the parent culture of "en-US" and "en-GB")</span></span>
* <span data-ttu-id="f7785-320">地區設定：地區設定與文化特性相同。</span><span class="sxs-lookup"><span data-stu-id="f7785-320">Locale: A locale is the same as a culture.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f7785-321">其他資源</span><span class="sxs-lookup"><span data-stu-id="f7785-321">Additional resources</span></span>

* <span data-ttu-id="f7785-322">本文使用的 [Localization.StarterWeb 專案](https://github.com/aspnet/entropy)。</span><span class="sxs-lookup"><span data-stu-id="f7785-322">[Localization.StarterWeb project](https://github.com/aspnet/entropy) used in the article.</span></span>
* [<span data-ttu-id="f7785-323">Visual Studio 中的資源檔</span><span class="sxs-lookup"><span data-stu-id="f7785-323">Resource Files in Visual Studio</span></span>](https://docs.microsoft.com/cpp/windows/resource-files-visual-studio)
* [<span data-ttu-id="f7785-324">.resx 檔案中的資源</span><span class="sxs-lookup"><span data-stu-id="f7785-324">Resources in .resx Files</span></span>](https://docs.microsoft.com/dotnet/framework/resources/working-with-resx-files-programmatically)
