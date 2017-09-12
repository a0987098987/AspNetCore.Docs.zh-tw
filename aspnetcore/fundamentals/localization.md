---
title: "全球化和當地語系化的 ASP.NET Core"
author: rick-anderson
description: "了解如何 ASP.NET Core 提供服務和中介軟體將內容當地語系化成不同的語言和文化特性。"
keywords: "ASP.NET Core 當地語系化，文化特性、 語言、 資源檔、 全球化、 國際化、 地區設定"
ms.author: riande
manager: wpickett
ms.date: 01/14/2017
ms.topic: article
ms.assetid: 7f275a09-f118-41c9-88d1-8de52d6a5aa1
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/localization
ms.openlocfilehash: 2a760343566d2c2be591983e20830b5207a2199b
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/12/2017
---
# <a name="globalization-and-localization-in-aspnet-core"></a><span data-ttu-id="e0142-104">全球化和當地語系化的 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e0142-104">Globalization and localization in ASP.NET Core</span></span>

<span data-ttu-id="e0142-105">由[Rick Anderson](https://twitter.com/RickAndMSFT)， [Damien Bowden](https://twitter.com/damien_bod)， [Bart Calixto](https://twitter.com/bartmax)， [Nadeem Afana](https://twitter.com/NadeemAfana)，和[Hisham Bin Ateya](https://twitter.com/hishambinateya)</span><span class="sxs-lookup"><span data-stu-id="e0142-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Damien Bowden](https://twitter.com/damien_bod), [Bart Calixto](https://twitter.com/bartmax), [Nadeem Afana](https://twitter.com/NadeemAfana), and [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span></span>

<span data-ttu-id="e0142-106">與 ASP.NET Core 建立多國語言的網站，可讓您的網站才能達到更多觀眾。</span><span class="sxs-lookup"><span data-stu-id="e0142-106">Creating a multilingual website with ASP.NET Core will allow your site to reach a wider audience.</span></span> <span data-ttu-id="e0142-107">ASP.NET Core 提供服務和中介軟體將當地語系化成不同的語言和文化特性。</span><span class="sxs-lookup"><span data-stu-id="e0142-107">ASP.NET Core provides services and middleware for localizing into different languages and cultures.</span></span>

<span data-ttu-id="e0142-108">國際化牽涉到[全球化](https://docs.microsoft.com/dotnet/api/system.globalization)和[當地語系化](https://docs.microsoft.com/dotnet/standard/globalization-localization/localization)。</span><span class="sxs-lookup"><span data-stu-id="e0142-108">Internationalization involves [Globalization](https://docs.microsoft.com/dotnet/api/system.globalization) and [Localization](https://docs.microsoft.com/dotnet/standard/globalization-localization/localization).</span></span> <span data-ttu-id="e0142-109">全球化是設計支援不同的文化特性的應用程式的程序。</span><span class="sxs-lookup"><span data-stu-id="e0142-109">Globalization is the process of designing apps that support different cultures.</span></span> <span data-ttu-id="e0142-110">全球化加入支援輸入、 顯示和一組定義的特定地理區域與相關聯的語言指令碼的輸出。</span><span class="sxs-lookup"><span data-stu-id="e0142-110">Globalization adds support for input, display, and output of a defined set of language scripts that relate to specific geographic areas.</span></span>

<span data-ttu-id="e0142-111">當地語系化是調整您已針對特定文化特性/地區設定可當地語系化的全球化應用程式的程序。</span><span class="sxs-lookup"><span data-stu-id="e0142-111">Localization is the process of adapting a globalized app, which you have already processed for localizability, to a particular culture/locale.</span></span>  <span data-ttu-id="e0142-112">如需詳細資訊，請參閱**全球化和當地語系化條款**這份文件結尾附近。</span><span class="sxs-lookup"><span data-stu-id="e0142-112">For more information see **Globalization and localization terms** near the end of this document.</span></span>

<span data-ttu-id="e0142-113">應用程式當地語系化涉及下列作業：</span><span class="sxs-lookup"><span data-stu-id="e0142-113">App localization involves the following:</span></span>

1. <span data-ttu-id="e0142-114">讓應用程式的內容可當地語系化</span><span class="sxs-lookup"><span data-stu-id="e0142-114">Make the app's content localizable</span></span>

2. <span data-ttu-id="e0142-115">提供的語言和您所支援的文化特性的當地語系化的資源</span><span class="sxs-lookup"><span data-stu-id="e0142-115">Provide localized resources for the languages and cultures you support</span></span>

3. <span data-ttu-id="e0142-116">選取每個要求的語言/文化特性的策略實作</span><span class="sxs-lookup"><span data-stu-id="e0142-116">Implement a strategy to select the language/culture for each request</span></span>

## <a name="make-the-apps-content-localizable"></a><span data-ttu-id="e0142-117">讓應用程式的內容可當地語系化</span><span class="sxs-lookup"><span data-stu-id="e0142-117">Make the app's content localizable</span></span>

<span data-ttu-id="e0142-118">ASP.NET Core 中導入`IStringLocalizer`和`IStringLocalizer<T>`已設計成開發當地語系化應用程式時提高生產力。</span><span class="sxs-lookup"><span data-stu-id="e0142-118">Introduced in ASP.NET Core, `IStringLocalizer` and `IStringLocalizer<T>` were architected to improve productivity when developing localized apps.</span></span> <span data-ttu-id="e0142-119">`IStringLocalizer`使用[ResourceManager](https://docs.microsoft.com/dotnet/api/system.resources.resourcemanager)和[ResourceReader](https://docs.microsoft.com/dotnet/api/system.resources.resourcereader)在執行階段提供特定文化特性的資源。</span><span class="sxs-lookup"><span data-stu-id="e0142-119">`IStringLocalizer` uses the [ResourceManager](https://docs.microsoft.com/dotnet/api/system.resources.resourcemanager) and [ResourceReader](https://docs.microsoft.com/dotnet/api/system.resources.resourcereader) to provide culture-specific resources at run time.</span></span> <span data-ttu-id="e0142-120">簡單的介面有索引子和`IEnumerable`傳回當地語系化的字串。</span><span class="sxs-lookup"><span data-stu-id="e0142-120">The simple interface has an indexer and an `IEnumerable` for returning localized strings.</span></span> <span data-ttu-id="e0142-121">`IStringLocalizer`不需要您將預設語言字串儲存在資源檔。</span><span class="sxs-lookup"><span data-stu-id="e0142-121">`IStringLocalizer` doesn't require you to store the default language strings in a resource file.</span></span> <span data-ttu-id="e0142-122">您可以開發應用程式當地語系化為目標，並不需要在開發的早期建立資源檔。</span><span class="sxs-lookup"><span data-stu-id="e0142-122">You can develop an app targeted for localization and not need to create resource files early in development.</span></span> <span data-ttu-id="e0142-123">下列程式碼會示範如何包裝當地語系化的字串"有關 Title"。</span><span class="sxs-lookup"><span data-stu-id="e0142-123">The code below shows how to wrap the string "About Title" for localization.</span></span>

<span data-ttu-id="e0142-124">[!code-csharp[Main](localization/sample/Controllers/AboutController.cs)]</span><span class="sxs-lookup"><span data-stu-id="e0142-124">[!code-csharp[Main](localization/sample/Controllers/AboutController.cs)]</span></span>

<span data-ttu-id="e0142-125">在上述程式碼中`IStringLocalizer<T>`實作來自[相依性插入](dependency-injection.md)。</span><span class="sxs-lookup"><span data-stu-id="e0142-125">In the code above, the `IStringLocalizer<T>` implementation comes from [Dependency Injection](dependency-injection.md).</span></span> <span data-ttu-id="e0142-126">如果找不到當地語系化"有關 Title"的值，則索引子機碼會傳回，也就是"有關 Title"的字串。</span><span class="sxs-lookup"><span data-stu-id="e0142-126">If the localized value of "About Title" is not found, then the indexer key is returned, that is, the string "About Title".</span></span> <span data-ttu-id="e0142-127">您可以保留預設的應用程式中的語言常值字串，並將其包裝在定位器，您可以專注於開發應用程式。</span><span class="sxs-lookup"><span data-stu-id="e0142-127">You can leave the default language literal strings in the app and wrap them in the localizer, so that you can focus on developing the app.</span></span> <span data-ttu-id="e0142-128">使用您的預設語言進行開發您的應用程式，並做好當地語系化步驟中沒有先建立預設資源檔。</span><span class="sxs-lookup"><span data-stu-id="e0142-128">You develop your app with your default language and prepare it for the localization step without first creating a default resource file.</span></span> <span data-ttu-id="e0142-129">或者，您可以使用的傳統方法，並提供要擷取的預設語言字串索引鍵。</span><span class="sxs-lookup"><span data-stu-id="e0142-129">Alternatively, you can use the traditional approach and provide a key to retrieve the default language string.</span></span> <span data-ttu-id="e0142-130">許多開發人員將新的工作流程不會有預設語言的*.resx*檔案和簡單地包裝字串常值可以降低當地語系化應用程式的額外負荷。</span><span class="sxs-lookup"><span data-stu-id="e0142-130">For many developers the new workflow of not having a default language *.resx* file and simply wrapping the string literals can reduce the overhead of localizing an app.</span></span> <span data-ttu-id="e0142-131">其他開發人員會偏好傳統的工作流程，因為它可以讓您更輕鬆地使用較長的字串常值和更輕鬆地更新的當地語系化的字串。</span><span class="sxs-lookup"><span data-stu-id="e0142-131">Other developers will prefer the traditional work flow as it can make it easier to work with longer string literals and make it easier to update localized strings.</span></span>

<span data-ttu-id="e0142-132">使用`IHtmlLocalizer<T>`包含 HTML 資源的實作。</span><span class="sxs-lookup"><span data-stu-id="e0142-132">Use the `IHtmlLocalizer<T>` implementation for resources that contain HTML.</span></span> <span data-ttu-id="e0142-133">`IHtmlLocalizer`HTML 編碼格式的資源字串，但不是資源字串的引數。</span><span class="sxs-lookup"><span data-stu-id="e0142-133">`IHtmlLocalizer` HTML encodes arguments that are formatted in the resource string, but not the resource string.</span></span> <span data-ttu-id="e0142-134">在此範例中反白顯示的值以下`name`參數是 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="e0142-134">In the sample highlighted below, only the value of `name` parameter is HTML encoded.</span></span>

<span data-ttu-id="e0142-135">[!code-csharp[Main](../fundamentals/localization/sample/Controllers/BookController.cs?highlight=3,5,20&start=1&end=24)]</span><span class="sxs-lookup"><span data-stu-id="e0142-135">[!code-csharp[Main](../fundamentals/localization/sample/Controllers/BookController.cs?highlight=3,5,20&start=1&end=24)]</span></span>

<span data-ttu-id="e0142-136">注意： 您通常想要只當地語系化文字和沒有 HTML。</span><span class="sxs-lookup"><span data-stu-id="e0142-136">Note: You generally want to only localize text and not HTML.</span></span>

<span data-ttu-id="e0142-137">最低層級，您可以取得`IStringLocalizerFactory`超出[相依性插入](dependency-injection.md):</span><span class="sxs-lookup"><span data-stu-id="e0142-137">At the lowest level, you can get `IStringLocalizerFactory` out of [Dependency Injection](dependency-injection.md):</span></span>

<span data-ttu-id="e0142-138">[!code-csharp[Main](localization/sample/Controllers/TestController.cs?start=9&end=26&highlight=7-13)]</span><span class="sxs-lookup"><span data-stu-id="e0142-138">[!code-csharp[Main](localization/sample/Controllers/TestController.cs?start=9&end=26&highlight=7-13)]</span></span>

<span data-ttu-id="e0142-139">上述程式碼示範兩個處理站的每個建立方法。</span><span class="sxs-lookup"><span data-stu-id="e0142-139">The code above demonstrates each of the two factory create methods.</span></span>

<span data-ttu-id="e0142-140">您可以控制站，區域中，依分割當地語系化的字串，或具有一個容器。</span><span class="sxs-lookup"><span data-stu-id="e0142-140">You can partition your localized strings by controller, area, or have just one container.</span></span> <span data-ttu-id="e0142-141">範例應用程式中的虛擬類別名稱為`SharedResource`用於共用資源。</span><span class="sxs-lookup"><span data-stu-id="e0142-141">In the sample app, a dummy class named `SharedResource` is used for shared resources.</span></span>

<span data-ttu-id="e0142-142">[!code-csharp[Main](localization/sample/Resources/SharedResource.cs)]</span><span class="sxs-lookup"><span data-stu-id="e0142-142">[!code-csharp[Main](localization/sample/Resources/SharedResource.cs)]</span></span>

<span data-ttu-id="e0142-143">有些開發人員使用`Startup`類別可以包含全域或共用的字串。</span><span class="sxs-lookup"><span data-stu-id="e0142-143">Some developers use the `Startup` class to contain global or shared strings.</span></span>  <span data-ttu-id="e0142-144">在下列範例，`InfoController`和`SharedResource`當地語系化人員使用：</span><span class="sxs-lookup"><span data-stu-id="e0142-144">In the sample below, the `InfoController` and the `SharedResource` localizers are used:</span></span>

<span data-ttu-id="e0142-145">[!code-csharp[Main](localization/sample/Controllers/InfoController.cs?range=9-26)]</span><span class="sxs-lookup"><span data-stu-id="e0142-145">[!code-csharp[Main](localization/sample/Controllers/InfoController.cs?range=9-26)]</span></span>

## <a name="view-localization"></a><span data-ttu-id="e0142-146">檢視當地語系化</span><span class="sxs-lookup"><span data-stu-id="e0142-146">View localization</span></span>

<span data-ttu-id="e0142-147">`IViewLocalizer`服務提供的當地語系化的字串[檢視](https://docs.microsoft.com/aspnet/core)。</span><span class="sxs-lookup"><span data-stu-id="e0142-147">The `IViewLocalizer` service provides localized strings for a [view](https://docs.microsoft.com/aspnet/core).</span></span> <span data-ttu-id="e0142-148">`ViewLocalizer`類別會實作這個介面，並尋找檢視的檔案路徑的資源位置。</span><span class="sxs-lookup"><span data-stu-id="e0142-148">The `ViewLocalizer` class implements this interface and finds the resource location from the view file path.</span></span> <span data-ttu-id="e0142-149">下列程式碼示範如何使用的預設實作`IViewLocalizer`:</span><span class="sxs-lookup"><span data-stu-id="e0142-149">The following code shows how to use the default implementation of `IViewLocalizer`:</span></span>

<span data-ttu-id="e0142-150">[!code-HTML[Main](localization/sample/Views/Home/About.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="e0142-150">[!code-HTML[Main](localization/sample/Views/Home/About.cshtml)]</span></span>

<span data-ttu-id="e0142-151">預設實作`IViewLocalizer`尋找檢視的檔案名稱為基礎的資源檔。</span><span class="sxs-lookup"><span data-stu-id="e0142-151">The default implementation of `IViewLocalizer` finds the resource file based on the view's file name.</span></span> <span data-ttu-id="e0142-152">沒有任何使用共用的全域資源檔案的選項。</span><span class="sxs-lookup"><span data-stu-id="e0142-152">There is no option to use a global shared resource file.</span></span> <span data-ttu-id="e0142-153">`ViewLocalizer`實作使用當地語系化`IHtmlLocalizer`，因此 Razor 不 HTML 編碼的當地語系化的字串。</span><span class="sxs-lookup"><span data-stu-id="e0142-153">`ViewLocalizer` implements the localizer using `IHtmlLocalizer`, so Razor doesn't HTML encode the localized string.</span></span> <span data-ttu-id="e0142-154">您可以參數化資源字串和`IViewLocalizer`將 HTML 編碼的參數，但不是資源字串。</span><span class="sxs-lookup"><span data-stu-id="e0142-154">You can parameterize resource strings and `IViewLocalizer` will HTML encode the parameters, but not the resource string.</span></span> <span data-ttu-id="e0142-155">請考慮下列 Razor 標記：</span><span class="sxs-lookup"><span data-stu-id="e0142-155">Consider the following Razor markup:</span></span>

```HTML
@Localizer["<i>Hello</i> <b>{0}!</b>", UserManager.GetUserName(User)]
   ```

<span data-ttu-id="e0142-156">法文資源檔可以包含下列內容：</span><span class="sxs-lookup"><span data-stu-id="e0142-156">A French resource file could contain the following:</span></span>

| <span data-ttu-id="e0142-157">Key</span><span class="sxs-lookup"><span data-stu-id="e0142-157">Key</span></span> | <span data-ttu-id="e0142-158">值</span><span class="sxs-lookup"><span data-stu-id="e0142-158">Value</span></span> |
| ----- | ------ |
| `<i>Hello</i> <b>{0}!</b>` | `<i>Bonjour</i> <b>{0} !</b> ` |

<span data-ttu-id="e0142-159">呈現的檢視可能包含從資源檔的 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="e0142-159">The rendered view would contain the HTML markup from the resource file.</span></span>

<span data-ttu-id="e0142-160">附註：</span><span class="sxs-lookup"><span data-stu-id="e0142-160">Notes:</span></span>
- <span data-ttu-id="e0142-161">檢視當地語系化需要 「 Localization.AspNetCore.TagHelpers"NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="e0142-161">View localization requires the "Localization.AspNetCore.TagHelpers" NuGet package.</span></span>
- <span data-ttu-id="e0142-162">您通常想要只當地語系化文字和沒有 HTML。</span><span class="sxs-lookup"><span data-stu-id="e0142-162">You generally want to only localize text and not HTML.</span></span>

<span data-ttu-id="e0142-163">若要使用的共用的資源檔案，在檢視中，插入`IHtmlLocalizer<T>`:</span><span class="sxs-lookup"><span data-stu-id="e0142-163">To use a shared resource file in a view, inject `IHtmlLocalizer<T>`:</span></span>

<span data-ttu-id="e0142-164">[!code-HTML[Main](../fundamentals/localization/sample/Views/Test/About.cshtml?highlight=5,12)]</span><span class="sxs-lookup"><span data-stu-id="e0142-164">[!code-HTML[Main](../fundamentals/localization/sample/Views/Test/About.cshtml?highlight=5,12)]</span></span>

## <a name="dataannotations-localization"></a><span data-ttu-id="e0142-165">DataAnnotations 當地語系化</span><span class="sxs-lookup"><span data-stu-id="e0142-165">DataAnnotations localization</span></span>

<span data-ttu-id="e0142-166">DataAnnotations 錯誤訊息會翻與`IStringLocalizer<T>`。</span><span class="sxs-lookup"><span data-stu-id="e0142-166">DataAnnotations error messages are localized with `IStringLocalizer<T>`.</span></span> <span data-ttu-id="e0142-167">使用選項`ResourcesPath = "Resources"`，錯誤訊息中`RegisterViewModel`可以儲存在下列路徑：</span><span class="sxs-lookup"><span data-stu-id="e0142-167">Using the option `ResourcesPath = "Resources"`, the error messages in `RegisterViewModel` can be stored in either of the following paths:</span></span>

* <span data-ttu-id="e0142-168">Resources/ViewModels.Account.RegisterViewModel.fr.resx</span><span class="sxs-lookup"><span data-stu-id="e0142-168">Resources/ViewModels.Account.RegisterViewModel.fr.resx</span></span>
* <span data-ttu-id="e0142-169">Resources/ViewModels/Account/RegisterViewModel.fr.resx</span><span class="sxs-lookup"><span data-stu-id="e0142-169">Resources/ViewModels/Account/RegisterViewModel.fr.resx</span></span>

<span data-ttu-id="e0142-170">[!code-csharp[Main](localization/sample/ViewModels/Account/RegisterViewModel.cs?start=9&end=26)]</span><span class="sxs-lookup"><span data-stu-id="e0142-170">[!code-csharp[Main](localization/sample/ViewModels/Account/RegisterViewModel.cs?start=9&end=26)]</span></span>

<span data-ttu-id="e0142-171">在 ASP.NET Core MVC 1.1.0 和更高版本、 非驗證屬性會當地語系化。</span><span class="sxs-lookup"><span data-stu-id="e0142-171">In ASP.NET Core MVC 1.1.0 and higher, non-validation attributes are localized.</span></span> <span data-ttu-id="e0142-172">ASP.NET Core MVC 1.0 未**不**查閱 非驗證屬性的當地語系化字串。</span><span class="sxs-lookup"><span data-stu-id="e0142-172">ASP.NET Core MVC 1.0 does **not** look up localized strings for non-validation attributes.</span></span>

<a name=one-resource-string-multiple-classes></a>
### <a name="using-one-resource-string-for-multiple-classes"></a><span data-ttu-id="e0142-173">多個類別使用一個資源字串</span><span class="sxs-lookup"><span data-stu-id="e0142-173">Using one resource string for multiple classes</span></span>

<span data-ttu-id="e0142-174">下列程式碼會示範如何使用資源字串，有多個類別的驗證屬性：</span><span class="sxs-lookup"><span data-stu-id="e0142-174">The following code shows how to use one resource string for validation attributes with multiple classes:</span></span>

```
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddDataAnnotationsLocalization(options => {
            options.DataAnnotationLocalizerProvider = (type, factory) =>
                factory.Create(typeof(SharedResource));
        });
}
```

<span data-ttu-id="e0142-175">在先前程式碼中，`SharedResource`是對應到 resx 類別儲存驗證訊息。</span><span class="sxs-lookup"><span data-stu-id="e0142-175">In the preceeding code, `SharedResource` is the class corresponding to the resx where your validation messages are stored.</span></span> <span data-ttu-id="e0142-176">使用這個方法，DataAnnotations 只會使用`SharedResource`，而不是每個類別的資源。</span><span class="sxs-lookup"><span data-stu-id="e0142-176">With this approach,  DataAnnotations will only use `SharedResource`, rather than the resource for each class.</span></span> 

## <a name="provide-localized-resources-for-the-languages-and-cultures-you-support"></a><span data-ttu-id="e0142-177">提供的語言和您所支援的文化特性的當地語系化的資源</span><span class="sxs-lookup"><span data-stu-id="e0142-177">Provide localized resources for the languages and cultures you support</span></span>  

### <a name="supportedcultures-and-supporteduicultures"></a><span data-ttu-id="e0142-178">SupportedCultures 和 SupportedUICultures</span><span class="sxs-lookup"><span data-stu-id="e0142-178">SupportedCultures and SupportedUICultures</span></span>

<span data-ttu-id="e0142-179">ASP.NET Core 可讓您指定兩個文化特性值，`SupportedCultures`和`SupportedUICultures`。</span><span class="sxs-lookup"><span data-stu-id="e0142-179">ASP.NET Core allows you to specify two culture values, `SupportedCultures` and `SupportedUICultures`.</span></span> <span data-ttu-id="e0142-180">[CultureInfo](https://docs.microsoft.com/dotnet/api/system.globalization.cultureinfo)物件`SupportedCultures`判斷文化特性相關的函數，例如日期、 時間、 數字和貨幣格式的結果。</span><span class="sxs-lookup"><span data-stu-id="e0142-180">The [CultureInfo](https://docs.microsoft.com/dotnet/api/system.globalization.cultureinfo) object for `SupportedCultures` determines the results of culture-dependent functions, such as date, time, number, and currency formatting.</span></span> <span data-ttu-id="e0142-181">`SupportedCultures`也會決定文字、 大小寫慣例和字串比較的排序順序。</span><span class="sxs-lookup"><span data-stu-id="e0142-181">`SupportedCultures` also determines the sorting order of text, casing conventions, and string comparisons.</span></span> <span data-ttu-id="e0142-182">請參閱[CultureInfo.CurrentCulture](https://docs.microsoft.com/dotnet/api/system.stringcomparer.currentculture#System_StringComparer_CurrentCulture)如需詳細資訊，在伺服器取得文化特性的方式。</span><span class="sxs-lookup"><span data-stu-id="e0142-182">See [CultureInfo.CurrentCulture](https://docs.microsoft.com/dotnet/api/system.stringcomparer.currentculture#System_StringComparer_CurrentCulture) for more info on how the server gets the Culture.</span></span> <span data-ttu-id="e0142-183">`SupportedUICultures`決定會轉譯為字串 (從*.resx*檔案) 依查閱[ResourceManager](https://docs.microsoft.com/dotnet/api/system.resources.resourcemanager)。</span><span class="sxs-lookup"><span data-stu-id="e0142-183">The `SupportedUICultures` determines which translates strings (from *.resx* files) are looked up by the [ResourceManager](https://docs.microsoft.com/dotnet/api/system.resources.resourcemanager).</span></span> <span data-ttu-id="e0142-184">`ResourceManager`查閱所決定的特定文化特性的字串，只需`CurrentUICulture`。</span><span class="sxs-lookup"><span data-stu-id="e0142-184">The `ResourceManager` simply looks up culture-specific strings that is determined by `CurrentUICulture`.</span></span> <span data-ttu-id="e0142-185">在.NET 中的每個執行緒都`CurrentCulture`和`CurrentUICulture`物件。</span><span class="sxs-lookup"><span data-stu-id="e0142-185">Every thread in .NET has `CurrentCulture` and `CurrentUICulture` objects.</span></span> <span data-ttu-id="e0142-186">ASP.NET Core 呈現文化特性相關函式時，會檢查這些值。</span><span class="sxs-lookup"><span data-stu-id="e0142-186">ASP.NET Core inspects these values when rendering culture-dependent functions.</span></span> <span data-ttu-id="e0142-187">比方說，如果目前執行緒的文化特性設定為"EN-US"英文 (美國），`DateTime.Now.ToLongDateString()`顯示 「 星期四年 2 月 18，2016 年 」，但是如果`CurrentCulture`設定"ES-ES"（西班牙文，西班牙） 的輸出會是"jueves，18 febrero de-de 2016"。</span><span class="sxs-lookup"><span data-stu-id="e0142-187">For example, if the current thread's culture is set to "en-US" (English, United States), `DateTime.Now.ToLongDateString()` displays "Thursday, February 18, 2016", but if `CurrentCulture` is set to "es-ES" (Spanish, Spain) the output will be "jueves, 18 de febrero de 2016".</span></span>

## <a name="working-with-resource-files"></a><span data-ttu-id="e0142-188">使用資源檔</span><span class="sxs-lookup"><span data-stu-id="e0142-188">Working with resource files</span></span>

<span data-ttu-id="e0142-189">資源檔是有用的機制，來從程式碼分隔可當地語系化的字串。</span><span class="sxs-lookup"><span data-stu-id="e0142-189">A resource file is a useful mechanism for separating localizable strings from code.</span></span> <span data-ttu-id="e0142-190">非預設語言的翻譯的字串會隔離*.resx*資源檔。</span><span class="sxs-lookup"><span data-stu-id="e0142-190">Translated strings for the non-default language are isolated *.resx* resource files.</span></span> <span data-ttu-id="e0142-191">例如，您可能想要建立名為西班牙文的資源檔*Welcome.es.resx*包含翻譯的字串。</span><span class="sxs-lookup"><span data-stu-id="e0142-191">For example, you might want to create Spanish resource file named *Welcome.es.resx* containing translated strings.</span></span> <span data-ttu-id="e0142-192">"es"是西班牙文語言程式碼。</span><span class="sxs-lookup"><span data-stu-id="e0142-192">"es" is the language code for Spanish.</span></span> <span data-ttu-id="e0142-193">若要在 Visual Studio 中建立此資源檔：</span><span class="sxs-lookup"><span data-stu-id="e0142-193">To create this resource file in Visual Studio:</span></span>

1. <span data-ttu-id="e0142-194">在**方案總管 中**，其中將包含資源檔的資料夾上按一下滑鼠右鍵 >**新增** > **新項目**。</span><span class="sxs-lookup"><span data-stu-id="e0142-194">In **Solution Explorer**, right click on the folder which will contain the resource file > **Add** > **New Item**.</span></span>

    ![巢狀的內容功能表： 在方案總管中，內容功能表已開啟的資源。](localization/_static/newi.png)

2. <span data-ttu-id="e0142-197">在**搜尋已安裝的範本**，輸入 「 資源 」，並將檔案命名。</span><span class="sxs-lookup"><span data-stu-id="e0142-197">In the **Search installed templates** box, enter "resource" and name the file.</span></span>

    ![加入新項目 對話方塊](localization/_static/res.png)

3. <span data-ttu-id="e0142-199">輸入索引鍵的值 （原生字串）**名稱**資料行和中的已翻譯的字串**值**資料行。</span><span class="sxs-lookup"><span data-stu-id="e0142-199">Enter the key value (native string) in the **Name** column and the translated string in the **Value** column.</span></span>

    ![名稱資料行中的 Hello 這個字與 值 欄位中的單字 Hola (Hello 西班牙文) Welcome.es.resx 檔案 （西班牙文的 歡迎使用資源檔）](localization/_static/hola.png)

    <span data-ttu-id="e0142-201">Visual Studio 會顯示*Welcome.es.resx*檔案。</span><span class="sxs-lookup"><span data-stu-id="e0142-201">Visual Studio shows the *Welcome.es.resx* file.</span></span>

    ![方案總管] 中顯示 [歡迎使用西班牙文 (es) 資源檔](localization/_static/se.png)

<a name="error"></a>

<span data-ttu-id="e0142-203">如果您使用 Visual Studio 2017 Preview 版本 15.3，您會取得資源編輯器中的錯誤指標。</span><span class="sxs-lookup"><span data-stu-id="e0142-203">If you are using Visual Studio 2017 Preview version 15.3, you'll get an error indicator in the resource editor.</span></span> <span data-ttu-id="e0142-204">移除*ResXFileCodeGenerator*值*自訂工具*屬性方格裡，若要避免這個錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="e0142-204">Remove the *ResXFileCodeGenerator*  value from the *Custom Tool*  properties grid to prevent this error message:</span></span>

![Resx 編輯器](localization/_static/err.png)

<span data-ttu-id="e0142-206">或者，您可以忽略此錯誤。</span><span class="sxs-lookup"><span data-stu-id="e0142-206">Alternatively, you can ignore this error.</span></span> <span data-ttu-id="e0142-207">我們希望的下一個版本中修正這個問題。</span><span class="sxs-lookup"><span data-stu-id="e0142-207">We hope to fix this in the next release.</span></span>

## <a name="resource-file-naming"></a><span data-ttu-id="e0142-208">資源檔命名</span><span class="sxs-lookup"><span data-stu-id="e0142-208">Resource file naming</span></span>

<span data-ttu-id="e0142-209">資源是減去的組件名稱與其類別的完整類型名稱命名。</span><span class="sxs-lookup"><span data-stu-id="e0142-209">Resources are named for the full type name of their class minus the assembly name.</span></span> <span data-ttu-id="e0142-210">例如，法文資源在其主要組件是的專案中`LocalizationWebsite.Web.dll`類別`LocalizationWebsite.Web.Startup`就會命名為*Startup.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="e0142-210">For example, a French resource in a project whose main assembly is `LocalizationWebsite.Web.dll` for the class `LocalizationWebsite.Web.Startup` would be named *Startup.fr.resx*.</span></span> <span data-ttu-id="e0142-211">類別資源`LocalizationWebsite.Web.Controllers.HomeController`就會命名為*Controllers.HomeController.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="e0142-211">A resource for the class `LocalizationWebsite.Web.Controllers.HomeController` would be named *Controllers.HomeController.fr.resx*.</span></span> <span data-ttu-id="e0142-212">如果您的目標的類別的命名空間不是相同的組件名稱必須完整類型名稱。</span><span class="sxs-lookup"><span data-stu-id="e0142-212">If your targeted class's namespace is not the same as the assembly name you will need the full type name.</span></span> <span data-ttu-id="e0142-213">比方說，在範例專案的資源類型`ExtraNamespace.Tools`就會命名為*ExtraNamespace.Tools.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="e0142-213">For example, in the sample project a resource for the type `ExtraNamespace.Tools` would be named *ExtraNamespace.Tools.fr.resx*.</span></span>

<span data-ttu-id="e0142-214">在範例專案中，`ConfigureServices`方法會設定`ResourcesPath`至 [資源]，主控制器的法文資源檔之專案相對路徑，所以*Resources/Controllers.HomeController.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="e0142-214">In the sample project, the `ConfigureServices` method sets the `ResourcesPath` to "Resources", so the project relative path for the home controller's French resource file is *Resources/Controllers.HomeController.fr.resx*.</span></span> <span data-ttu-id="e0142-215">或者，您可以使用資料夾來組織資源檔案。</span><span class="sxs-lookup"><span data-stu-id="e0142-215">Alternatively, you can use folders to organize resource files.</span></span> <span data-ttu-id="e0142-216">主控制器，路徑就是*Resources/Controllers/HomeController.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="e0142-216">For the home controller, the path would be *Resources/Controllers/HomeController.fr.resx*.</span></span> <span data-ttu-id="e0142-217">如果您不使用`ResourcesPath`選項， *.resx*檔案會放在專案的基底目錄。</span><span class="sxs-lookup"><span data-stu-id="e0142-217">If you don't use the `ResourcesPath` option, the *.resx* file would go in the project base directory.</span></span> <span data-ttu-id="e0142-218">資源檔`HomeController`就會命名為*Controllers.HomeController.fr.resx*。</span><span class="sxs-lookup"><span data-stu-id="e0142-218">The resource file for `HomeController` would be named *Controllers.HomeController.fr.resx*.</span></span> <span data-ttu-id="e0142-219">使用點或路徑的命名慣例的選擇取決於您想要組織您的資源檔的方式。</span><span class="sxs-lookup"><span data-stu-id="e0142-219">The choice of using the dot or path naming convention depends on how you want to organize your resource files.</span></span>

| <span data-ttu-id="e0142-220">資源名稱</span><span class="sxs-lookup"><span data-stu-id="e0142-220">Resource name</span></span> | <span data-ttu-id="e0142-221">點或路徑命名</span><span class="sxs-lookup"><span data-stu-id="e0142-221">Dot or path naming</span></span> |
| ------------   | ------------- |
| <span data-ttu-id="e0142-222">Resources/Controllers.HomeController.fr.resx</span><span class="sxs-lookup"><span data-stu-id="e0142-222">Resources/Controllers.HomeController.fr.resx</span></span> | <span data-ttu-id="e0142-223">點</span><span class="sxs-lookup"><span data-stu-id="e0142-223">Dot</span></span>  |
| <span data-ttu-id="e0142-224">Resources/Controllers/HomeController.fr.resx</span><span class="sxs-lookup"><span data-stu-id="e0142-224">Resources/Controllers/HomeController.fr.resx</span></span>  | <span data-ttu-id="e0142-225">路徑</span><span class="sxs-lookup"><span data-stu-id="e0142-225">Path</span></span> |
|    |     |

<span data-ttu-id="e0142-226">使用的資源檔`@inject IViewLocalizer`Razor 檢視中，請遵循類似的模式。</span><span class="sxs-lookup"><span data-stu-id="e0142-226">Resource files using `@inject IViewLocalizer` in Razor views follow a similar pattern.</span></span> <span data-ttu-id="e0142-227">如需檢視的資源檔可以使用點命名或路徑命名命名。</span><span class="sxs-lookup"><span data-stu-id="e0142-227">The resource file for a view can be named using either dot naming or path naming.</span></span> <span data-ttu-id="e0142-228">Razor 檢視資源檔模仿其相關聯的檢視檔案的路徑。</span><span class="sxs-lookup"><span data-stu-id="e0142-228">Razor view resource files mimic the path of their associated view file.</span></span> <span data-ttu-id="e0142-229">假設我們設定`ResourcesPath`「 資源 」，法文資源檔相關聯*Views/Home/About.cshtml*檢視可能是下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="e0142-229">Assuming we set the `ResourcesPath` to "Resources", the French resource file associated with the *Views/Home/About.cshtml* view could be either of the following:</span></span>

* <span data-ttu-id="e0142-230">Resources/Views/Home/About.fr.resx</span><span class="sxs-lookup"><span data-stu-id="e0142-230">Resources/Views/Home/About.fr.resx</span></span>

* <span data-ttu-id="e0142-231">Resources/Views.Home.About.fr.resx</span><span class="sxs-lookup"><span data-stu-id="e0142-231">Resources/Views.Home.About.fr.resx</span></span>

<span data-ttu-id="e0142-232">如果您不使用`ResourcesPath`選項， *.resx*檔案檢視會位於相同的資料夾檢視。</span><span class="sxs-lookup"><span data-stu-id="e0142-232">If you don't use the `ResourcesPath` option, the *.resx* file for a view would be located in the same folder as the view.</span></span>

<span data-ttu-id="e0142-233">如果您移除 「.fr 」 文化特性指示項，且您必須設定為 「 法文 （透過 cookie 或其他機制） 的文化特性，在讀取預設資源檔，以及字串已當地語系化。</span><span class="sxs-lookup"><span data-stu-id="e0142-233">If you remove the ".fr" culture designator AND you have the culture set to French (via cookie or other mechanism), the default resource file is read and strings are localized.</span></span> <span data-ttu-id="e0142-234">資源管理員會指定預設值或後援資源，不符合您要求的文化特性，您要提供文化特性指示項不 *.resx 檔案時。</span><span class="sxs-lookup"><span data-stu-id="e0142-234">The Resource manager designates a default or fallback resource, when nothing meets your requested culture you're served the *.resx file without a culture designator.</span></span> <span data-ttu-id="e0142-235">如果您想要針對您的要求文化特性缺少資源，必須沒有預設資源檔時，只傳回索引鍵。</span><span class="sxs-lookup"><span data-stu-id="e0142-235">If you want to just return the key when missing a resource for the requested culture you must not have a default resource file.</span></span>

### <a name="generating-resource-files-with-visual-studio"></a><span data-ttu-id="e0142-236">使用 Visual Studio 產生的資源檔</span><span class="sxs-lookup"><span data-stu-id="e0142-236">Generating resource files with Visual Studio</span></span>

<span data-ttu-id="e0142-237">如果在 Visual Studio 中建立資源檔，而不是檔案名稱中的文化特性 (例如， *Welcome.resx*)，Visual Studio 會針對每個字串的屬性建立 C# 類別。</span><span class="sxs-lookup"><span data-stu-id="e0142-237">If you create a resource file in Visual Studio without a culture in the file name (for example, *Welcome.resx*), Visual Studio will create a C# class with a property for each string.</span></span> <span data-ttu-id="e0142-238">這通常不是您想要使用 ASP.NET Core;您通常不會有預設值*.resx*資源檔 (A *.resx*沒有的文化特性名稱的檔案)。</span><span class="sxs-lookup"><span data-stu-id="e0142-238">That's usually not what you want with ASP.NET Core; you typically don't have a default *.resx* resource file (A *.resx* file without the culture name).</span></span> <span data-ttu-id="e0142-239">我們建議您建立*.resx*文化特性名稱的檔案 (例如*Welcome.fr.resx*)。</span><span class="sxs-lookup"><span data-stu-id="e0142-239">We suggest you create the *.resx* file with a culture name (for example *Welcome.fr.resx*).</span></span> <span data-ttu-id="e0142-240">當您建立*.resx*文化特性名稱，Visual Studio 檔案將不會產生類別檔案。</span><span class="sxs-lookup"><span data-stu-id="e0142-240">When you create a *.resx* file with a culture name, Visual Studio will not generate the class file.</span></span> <span data-ttu-id="e0142-241">我們預期的許多開發人員**不**建立預設語言資源檔案。</span><span class="sxs-lookup"><span data-stu-id="e0142-241">We anticipate that many developers will **not** create a default language resource file.</span></span>

### <a name="adding-other-cultures"></a><span data-ttu-id="e0142-242">加入其他文化特性</span><span class="sxs-lookup"><span data-stu-id="e0142-242">Adding Other Cultures</span></span>

<span data-ttu-id="e0142-243">每個語言和文化特性的組合 （非預設的語言） 需要唯一的資源檔。</span><span class="sxs-lookup"><span data-stu-id="e0142-243">Each language and culture combination (other than the default language) requires a unique resource file.</span></span> <span data-ttu-id="e0142-244">您藉由建立新的資源檔中的 ISO 語言代碼是檔案名稱的一部分建立不同的文化特性與地區設定的資源檔 (例如， **en-我們**， **fr ca**，和**en gb**)。</span><span class="sxs-lookup"><span data-stu-id="e0142-244">You create resource files for different cultures and locales by creating new resource files in which the ISO language codes are part of the file name (for example, **en-us**, **fr-ca**, and **en-gb**).</span></span> <span data-ttu-id="e0142-245">這些 ISO 碼放置於之間的檔案名稱和*.resx*副檔名，像是*Welcome.es MX.resx* （墨西哥西班牙文/）。</span><span class="sxs-lookup"><span data-stu-id="e0142-245">These ISO codes are placed between the file name and the *.resx* file name extension, as in *Welcome.es-MX.resx* (Spanish/Mexico).</span></span> <span data-ttu-id="e0142-246">若要指定的文化特性中性語言，移除 國家/地區的程式碼 (`MX`在上述範例中)。</span><span class="sxs-lookup"><span data-stu-id="e0142-246">To specify a culturally neutral language, remove the country code (`MX` in the preceding example).</span></span> <span data-ttu-id="e0142-247">西班牙文的文化特性中性資源檔案名稱是*Welcome.es.resx*。</span><span class="sxs-lookup"><span data-stu-id="e0142-247">The culturally neutral Spanish resource file name is *Welcome.es.resx*.</span></span>

## <a name="implement-a-strategy-to-select-the-languageculture-for-each-request"></a><span data-ttu-id="e0142-248">選取每個要求的語言/文化特性的策略實作</span><span class="sxs-lookup"><span data-stu-id="e0142-248">Implement a strategy to select the language/culture for each request</span></span>  

### <a name="configuring-localization"></a><span data-ttu-id="e0142-249">設定當地語系化</span><span class="sxs-lookup"><span data-stu-id="e0142-249">Configuring localization</span></span>

<span data-ttu-id="e0142-250">設定當地語系化`ConfigureServices`方法：</span><span class="sxs-lookup"><span data-stu-id="e0142-250">Localization is configured in the `ConfigureServices` method:</span></span>

<span data-ttu-id="e0142-251">[!code-csharp[Main](localization/sample/Program.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="e0142-251">[!code-csharp[Main](localization/sample/Program.cs?name=snippet1)]</span></span>

* <span data-ttu-id="e0142-252">`AddLocalization`將當地語系化服務加入至服務容器。</span><span class="sxs-lookup"><span data-stu-id="e0142-252">`AddLocalization` Adds the localization services to the services container.</span></span> <span data-ttu-id="e0142-253">上方的程式碼也會設定 「 資源 」 的資源路徑。</span><span class="sxs-lookup"><span data-stu-id="e0142-253">The code above also sets the resources path to "Resources".</span></span>

* <span data-ttu-id="e0142-254">`AddViewLocalization`新增支援當地語系化的檢視檔案。</span><span class="sxs-lookup"><span data-stu-id="e0142-254">`AddViewLocalization` Adds support for localized view files.</span></span> <span data-ttu-id="e0142-255">在此範例檢視當地語系化為基礎的檢視檔案後置詞。</span><span class="sxs-lookup"><span data-stu-id="e0142-255">In this sample view localization is based on the view file suffix.</span></span> <span data-ttu-id="e0142-256">例如"fr"中的*Index.fr.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="e0142-256">For example "fr" in the *Index.fr.cshtml* file.</span></span>

* <span data-ttu-id="e0142-257">`AddDataAnnotationsLocalization`新增支援當地語系化`DataAnnotations`驗證訊息透過`IStringLocalizer`抽象概念。</span><span class="sxs-lookup"><span data-stu-id="e0142-257">`AddDataAnnotationsLocalization` Adds support for localized `DataAnnotations` validation messages through `IStringLocalizer` abstractions.</span></span>

### <a name="localization-middleware"></a><span data-ttu-id="e0142-258">當地語系化的中介軟體</span><span class="sxs-lookup"><span data-stu-id="e0142-258">Localization middleware</span></span>

<span data-ttu-id="e0142-259">在要求上目前的文化特性設定當地語系化的[中介軟體](middleware.md)。</span><span class="sxs-lookup"><span data-stu-id="e0142-259">The current culture on a request is set in the localization [Middleware](middleware.md).</span></span> <span data-ttu-id="e0142-260">當地語系化的中介軟體中已啟用`Configure`方法*Program.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="e0142-260">The localization middleware is enabled in the `Configure` method of *Program.cs* file.</span></span> <span data-ttu-id="e0142-261">請注意，必須設定當地語系化的中介軟體，可能會檢查要求文化特性任何中介軟體之前 (例如， `app.UseMvcWithDefaultRoute()`)。</span><span class="sxs-lookup"><span data-stu-id="e0142-261">Note, the localization middleware must be configured before any middleware which might check the request culture (for example, `app.UseMvcWithDefaultRoute()`).</span></span>

<span data-ttu-id="e0142-262">[!code-csharp[Main](localization/sample/Program.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="e0142-262">[!code-csharp[Main](localization/sample/Program.cs?name=snippet2)]</span></span>

<span data-ttu-id="e0142-263">`UseRequestLocalization`初始化`RequestLocalizationOptions`物件。</span><span class="sxs-lookup"><span data-stu-id="e0142-263">`UseRequestLocalization` initializes a `RequestLocalizationOptions` object.</span></span> <span data-ttu-id="e0142-264">在每次要求清單的`RequestCultureProvider`中`RequestLocalizationOptions`列舉並用可以成功地決定要求文化特性的第一個提供者。</span><span class="sxs-lookup"><span data-stu-id="e0142-264">On every request the list of `RequestCultureProvider` in the `RequestLocalizationOptions` is enumerated and the first provider that can successfully determine the request culture is used.</span></span> <span data-ttu-id="e0142-265">預設的提供者來自`RequestLocalizationOptions`類別：</span><span class="sxs-lookup"><span data-stu-id="e0142-265">The default providers come from the `RequestLocalizationOptions` class:</span></span>

1. `QueryStringRequestCultureProvider`
2. `CookieRequestCultureProvider`
3. `AcceptLanguageHeaderRequestCultureProvider`

<span data-ttu-id="e0142-266">預設清單會從最特定至最不特定。</span><span class="sxs-lookup"><span data-stu-id="e0142-266">The default list goes from most specific to least specific.</span></span> <span data-ttu-id="e0142-267">在本文稍後我們會看到如何變更順序和甚至加入自訂文化特性的提供者。</span><span class="sxs-lookup"><span data-stu-id="e0142-267">Later in the article we'll see how you can change the order and even add a custom culture provider.</span></span> <span data-ttu-id="e0142-268">如果沒有提供者可以判斷要求的文化特性，`DefaultRequestCulture`用。</span><span class="sxs-lookup"><span data-stu-id="e0142-268">If none of the providers can determine the request culture, the `DefaultRequestCulture` is used.</span></span>

### <a name="querystringrequestcultureprovider"></a><span data-ttu-id="e0142-269">QueryStringRequestCultureProvider</span><span class="sxs-lookup"><span data-stu-id="e0142-269">QueryStringRequestCultureProvider</span></span>

<span data-ttu-id="e0142-270">某些應用程式會使用查詢字串來設定[文化特性和 UI 文化特性](https://msdn.microsoft.com/library/system.globalization.cultureinfo.aspx)。</span><span class="sxs-lookup"><span data-stu-id="e0142-270">Some apps will use a query string to set the [culture and UI culture](https://msdn.microsoft.com/library/system.globalization.cultureinfo.aspx).</span></span> <span data-ttu-id="e0142-271">針對使用 cookie 或 Accept-encoding 標頭方法的應用程式，將查詢字串新增至 URL 是用於偵錯和測試程式碼。</span><span class="sxs-lookup"><span data-stu-id="e0142-271">For apps that use the cookie or Accept-Language header approach, adding a query string to the URL is useful for debugging and testing code.</span></span> <span data-ttu-id="e0142-272">根據預設，`QueryStringRequestCultureProvider`登錄中的第一個當地語系化提供者為`RequestCultureProvider`清單。</span><span class="sxs-lookup"><span data-stu-id="e0142-272">By default, the `QueryStringRequestCultureProvider` is registered as the first localization provider in the `RequestCultureProvider` list.</span></span> <span data-ttu-id="e0142-273">您將查詢字串參數`culture`和`ui-culture`。</span><span class="sxs-lookup"><span data-stu-id="e0142-273">You pass the query string parameters `culture` and `ui-culture`.</span></span> <span data-ttu-id="e0142-274">下列範例會設定要每次墨西哥的西班牙文 （語言和地區） 的特定文化特性：</span><span class="sxs-lookup"><span data-stu-id="e0142-274">The following example sets the specific culture (language and region) to Spanish/Mexico:</span></span>

   `http://localhost:5000/?culture=es-MX&ui-culture=es-MX`

<span data-ttu-id="e0142-275">如果您只傳入以其中兩個 (`culture`或`ui-culture`)，查詢字串提供者會設定使用了傳入的兩個值。</span><span class="sxs-lookup"><span data-stu-id="e0142-275">If you only pass in one of the two (`culture` or `ui-culture`), the query string provider will set both values using the one you passed in.</span></span> <span data-ttu-id="e0142-276">例如，設定的文化特性會同時設定`Culture`和`UICulture`:</span><span class="sxs-lookup"><span data-stu-id="e0142-276">For example, setting just the culture will set both the `Culture` and the `UICulture`:</span></span>

   `http://localhost:5000/?culture=es-MX`

### <a name="cookierequestcultureprovider"></a><span data-ttu-id="e0142-277">CookieRequestCultureProvider</span><span class="sxs-lookup"><span data-stu-id="e0142-277">CookieRequestCultureProvider</span></span>

<span data-ttu-id="e0142-278">實際執行應用程式通常會提供一個機制來設定與 ASP.NET Core 文化特性 cookie 文化特性。</span><span class="sxs-lookup"><span data-stu-id="e0142-278">Production apps will often provide a mechanism to set the culture with the ASP.NET Core culture cookie.</span></span> <span data-ttu-id="e0142-279">使用`MakeCookieValue`方法來建立 cookie。</span><span class="sxs-lookup"><span data-stu-id="e0142-279">Use the `MakeCookieValue`  method to create a cookie.</span></span>

<span data-ttu-id="e0142-280">`CookieRequestCultureProvider` `DefaultCookieName`傳回用來追蹤使用者的預設 cookie 名稱的慣用文化特性資訊。</span><span class="sxs-lookup"><span data-stu-id="e0142-280">The `CookieRequestCultureProvider` `DefaultCookieName`  returns the default cookie name used to track the user’s preferred culture information.</span></span> <span data-ttu-id="e0142-281">預設的 cookie 名稱是"。AspNetCore.Culture"。</span><span class="sxs-lookup"><span data-stu-id="e0142-281">The default cookie  name is ".AspNetCore.Culture".</span></span>

<span data-ttu-id="e0142-282">Cookie 格式是`c=%LANGCODE%|uic=%LANGCODE%`，其中`c`是`Culture`和`uic`是`UICulture`，例如：</span><span class="sxs-lookup"><span data-stu-id="e0142-282">The cookie format is `c=%LANGCODE%|uic=%LANGCODE%`, where `c` is `Culture` and `uic` is `UICulture`, for example:</span></span>

    c=en-UK|uic=en-US

<span data-ttu-id="e0142-283">如果您只指定文化特性資訊和 UI 文化特性的其中一個，指定的文化特性將使用的文化特性資訊和 UI 文化特性。</span><span class="sxs-lookup"><span data-stu-id="e0142-283">If you only specify one of culture info and UI culture, the specified culture will be used for both culture info and UI culture.</span></span>

### <a name="the-accept-language-http-header"></a><span data-ttu-id="e0142-284">Accept-encoding HTTP 標頭</span><span class="sxs-lookup"><span data-stu-id="e0142-284">The Accept-Language HTTP header</span></span>

<span data-ttu-id="e0142-285">[Accept-encoding 標頭](https://www.w3.org/International/questions/qa-accept-lang-locales)是可在大部分的瀏覽器設定和原本要用來指定使用者的語言。</span><span class="sxs-lookup"><span data-stu-id="e0142-285">The [Accept-Language header](https://www.w3.org/International/questions/qa-accept-lang-locales) is settable in most browsers and was originally intended to specify the user's language.</span></span> <span data-ttu-id="e0142-286">這個設定會指出瀏覽器功能已設定為傳送或已繼承自基礎作業系統。</span><span class="sxs-lookup"><span data-stu-id="e0142-286">This setting indicates what the browser has been set to send or has inherited from the underlying operating system.</span></span> <span data-ttu-id="e0142-287">從瀏覽器要求的接受語言 HTTP 標頭不是百分之百的方法，可偵測使用者的慣用的語言 (請參閱[瀏覽器中設定語言喜好設定](https://www.w3.org/International/questions/qa-lang-priorities.en.php))。</span><span class="sxs-lookup"><span data-stu-id="e0142-287">The Accept-Language HTTP header from a browser request is not an infallible way to detect the user's preferred language (see [Setting language preferences in a browser](https://www.w3.org/International/questions/qa-lang-priorities.en.php)).</span></span> <span data-ttu-id="e0142-288">在生產應用程式應該包含方法，讓使用者可以自訂自己選擇的文化特性。</span><span class="sxs-lookup"><span data-stu-id="e0142-288">A production app should include a way for a user to customize their choice of culture.</span></span>

### <a name="setting-the-accept-language-http-header-in-ie"></a><span data-ttu-id="e0142-289">在 IE 中設定 Accept-encoding HTTP 標頭</span><span class="sxs-lookup"><span data-stu-id="e0142-289">Setting the Accept-Language HTTP header in IE</span></span>

1. <span data-ttu-id="e0142-290">從齒輪圖示，點選**網際網路選項**。</span><span class="sxs-lookup"><span data-stu-id="e0142-290">From the gear icon, tap **Internet Options**.</span></span>

2. <span data-ttu-id="e0142-291">點選**語言**。</span><span class="sxs-lookup"><span data-stu-id="e0142-291">Tap **Languages**.</span></span>

    ![網際網路選項](localization/_static/lang.png)

3. <span data-ttu-id="e0142-293">點選**設定語言喜好設定**。</span><span class="sxs-lookup"><span data-stu-id="e0142-293">Tap **Set Language Preferences**.</span></span>

4. <span data-ttu-id="e0142-294">點選**新增語言**。</span><span class="sxs-lookup"><span data-stu-id="e0142-294">Tap **Add a language**.</span></span>

5. <span data-ttu-id="e0142-295">新增的語言。</span><span class="sxs-lookup"><span data-stu-id="e0142-295">Add the language.</span></span>

6. <span data-ttu-id="e0142-296">點選語言，然後點選 **移**。</span><span class="sxs-lookup"><span data-stu-id="e0142-296">Tap the language, then tap **Move Up**.</span></span>

### <a name="using-a-custom-provider"></a><span data-ttu-id="e0142-297">使用自訂提供者</span><span class="sxs-lookup"><span data-stu-id="e0142-297">Using a custom provider</span></span>

<span data-ttu-id="e0142-298">假設您想要讓客戶將他們的語言和文化特性儲存在您的資料庫。</span><span class="sxs-lookup"><span data-stu-id="e0142-298">Suppose you want to let your customers store their language and culture in your databases.</span></span> <span data-ttu-id="e0142-299">您可以撰寫查詢使用者，這些值提供者。</span><span class="sxs-lookup"><span data-stu-id="e0142-299">You could write a provider to look up these values for the user.</span></span> <span data-ttu-id="e0142-300">下列程式碼會示範如何加入自訂提供者：</span><span class="sxs-lookup"><span data-stu-id="e0142-300">The following code shows how to add a custom provider:</span></span>

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

<span data-ttu-id="e0142-301">使用`RequestLocalizationOptions`新增或移除當地語系化提供者。</span><span class="sxs-lookup"><span data-stu-id="e0142-301">Use `RequestLocalizationOptions` to add or remove localization providers.</span></span>

### <a name="setting-the-culture-programmatically"></a><span data-ttu-id="e0142-302">以程式設計方式設定文化特性</span><span class="sxs-lookup"><span data-stu-id="e0142-302">Setting the culture programmatically</span></span>

<span data-ttu-id="e0142-303">這個範例**Localization.StarterWeb**投影上[GitHub](https://github.com/aspnet/entropy)包含設定的 UI `Culture`。</span><span class="sxs-lookup"><span data-stu-id="e0142-303">This sample **Localization.StarterWeb** project on [GitHub](https://github.com/aspnet/entropy) contains UI to set the `Culture`.</span></span> <span data-ttu-id="e0142-304">*Views/Shared/_SelectLanguagePartial.cshtml*檔可讓您從支援的文化特性的清單中選取的文化特性：</span><span class="sxs-lookup"><span data-stu-id="e0142-304">The *Views/Shared/_SelectLanguagePartial.cshtml* file allows you to select the culture from the list of supported cultures:</span></span>

<span data-ttu-id="e0142-305">[!code-HTML[Main](localization/sample/Views/Shared/_SelectLanguagePartial.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="e0142-305">[!code-HTML[Main](localization/sample/Views/Shared/_SelectLanguagePartial.cshtml)]</span></span>

<span data-ttu-id="e0142-306">*Views/Shared/_SelectLanguagePartial.cshtml*檔案加入至`footer`區段的配置檔案，因此將予以提供至所有的檢視：</span><span class="sxs-lookup"><span data-stu-id="e0142-306">The *Views/Shared/_SelectLanguagePartial.cshtml* file is added to the `footer` section of the layout file so it will be available to all views:</span></span>

<span data-ttu-id="e0142-307">[!code-HTML[Main](localization/sample/Views/Shared/_Layout.cshtml?range=43-56&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="e0142-307">[!code-HTML[Main](localization/sample/Views/Shared/_Layout.cshtml?range=43-56&highlight=10)]</span></span>

<span data-ttu-id="e0142-308">`SetLanguage`方法設定的文化特性的 cookie。</span><span class="sxs-lookup"><span data-stu-id="e0142-308">The `SetLanguage` method sets the culture cookie.</span></span>

<span data-ttu-id="e0142-309">[!code-csharp[Main](localization/sample/Controllers/HomeController.cs?range=57-67)]</span><span class="sxs-lookup"><span data-stu-id="e0142-309">[!code-csharp[Main](localization/sample/Controllers/HomeController.cs?range=57-67)]</span></span>

<span data-ttu-id="e0142-310">您無法插入*_SelectLanguagePartial.cshtml*這個專案的範例程式碼。</span><span class="sxs-lookup"><span data-stu-id="e0142-310">You can't plug in the *_SelectLanguagePartial.cshtml* to sample code for this project.</span></span> <span data-ttu-id="e0142-311">**Localization.StarterWeb**投影上[GitHub](https://github.com/aspnet/entropy)有程式碼流向`RequestLocalizationOptions`來透過部分 Razor[相依性插入](dependency-injection.md)容器。</span><span class="sxs-lookup"><span data-stu-id="e0142-311">The **Localization.StarterWeb** project on [GitHub](https://github.com/aspnet/entropy) has code to flow the `RequestLocalizationOptions` to a Razor partial through the [Dependency Injection](dependency-injection.md) container.</span></span>

## <a name="globalization-and-localization-terms"></a><span data-ttu-id="e0142-312">全球化和當地語系化之條款</span><span class="sxs-lookup"><span data-stu-id="e0142-312">Globalization and localization terms</span></span>

<span data-ttu-id="e0142-313">當地語系化您的應用程式的程序也需要相關的字元集中的現代軟體開發中常用的基本了解並了解與它們相關聯的問題。</span><span class="sxs-lookup"><span data-stu-id="e0142-313">The process of localizing your app also requires a basic understanding of relevant character sets commonly used in modern software development and an understanding of the issues associated with them.</span></span> <span data-ttu-id="e0142-314">雖然所有的電腦會將文字儲存為數字 （程式碼），不同的系統存放區使用不同的數字的相同文字。</span><span class="sxs-lookup"><span data-stu-id="e0142-314">Although all computers store text as numbers (codes), different systems store the same text using different numbers.</span></span> <span data-ttu-id="e0142-315">轉譯特定的文化特性/地區設定的應用程式使用者介面 (UI) 是指在當地語系化過程。</span><span class="sxs-lookup"><span data-stu-id="e0142-315">The localization process refers to translating the app user interface (UI) for a specific culture/locale.</span></span>

<span data-ttu-id="e0142-316">[當地語系化能力](https://docs.microsoft.com/dotnet/standard/globalization-localization/localizability-review)會確認全球化應用程式已準備好進行當地語系化的中繼程序。</span><span class="sxs-lookup"><span data-stu-id="e0142-316">[Localizability](https://docs.microsoft.com/dotnet/standard/globalization-localization/localizability-review) is an intermediate process for verifying that a globalized app is ready for localization.</span></span>

<span data-ttu-id="e0142-317">[RFC 4646](https://www.ietf.org/rfc/rfc4646.txt)格式的文化特性名稱是"<languagecode2>-< country/regioncode2 >"，其中<languagecode2>的語言代碼，而 < country/regioncode2 > 子文化特性代碼。</span><span class="sxs-lookup"><span data-stu-id="e0142-317">The [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) format for the culture name is "<languagecode2>-<country/regioncode2>", where <languagecode2> is the language code and <country/regioncode2> is the subculture code.</span></span> <span data-ttu-id="e0142-318">例如，`es-CL`西班牙文 （智利），`en-US`英文 （美國） 和`en-AU`英文 （澳大利亞）。</span><span class="sxs-lookup"><span data-stu-id="e0142-318">For example, `es-CL` for Spanish (Chile), `en-US` for English (United States), and `en-AU` for English (Australia).</span></span> <span data-ttu-id="e0142-319">[RFC 4646](https://www.ietf.org/rfc/rfc4646.txt)是兩個字母小寫文化特性代碼相關聯的語言的 ISO 639 和兩個字母的大寫子程式碼相關聯的國家或地區的 ISO 3166 的組合。</span><span class="sxs-lookup"><span data-stu-id="e0142-319">[RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) is a combination of an ISO 639 two-letter lowercase culture code associated with a language and an ISO 3166 two-letter uppercase subculture code associated with a country or region.</span></span>  <span data-ttu-id="e0142-320">請參閱[語言文化特性名稱](https://msdn.microsoft.com/library/ee825488(v=cs.20).aspx)。</span><span class="sxs-lookup"><span data-stu-id="e0142-320">See [Language Culture Name](https://msdn.microsoft.com/library/ee825488(v=cs.20).aspx).</span></span>

<span data-ttu-id="e0142-321">國際化通常縮寫為"I18N"。</span><span class="sxs-lookup"><span data-stu-id="e0142-321">Internationalization is often abbreviated to "I18N".</span></span> <span data-ttu-id="e0142-322">縮寫採用第一個和最後一個字母和字母它們，因此 18 之間的數字代表數目之間的第一個字母"I"及最後"N"。</span><span class="sxs-lookup"><span data-stu-id="e0142-322">The abbreviation takes the first and last letters and the number of letters between them, so 18 stands for the number of letters between the first "I" and the last "N".</span></span> <span data-ttu-id="e0142-323">同樣適用於全球化 (G11N)，與當地語系化 (L10N)。</span><span class="sxs-lookup"><span data-stu-id="e0142-323">The same applies to Globalization (G11N), and Localization (L10N).</span></span>

<span data-ttu-id="e0142-324">詞彙：</span><span class="sxs-lookup"><span data-stu-id="e0142-324">Terms:</span></span>

* <span data-ttu-id="e0142-325">全球化 (G11N): 程序可確保應用程式支援不同語言和區域。</span><span class="sxs-lookup"><span data-stu-id="e0142-325">Globalization (G11N): The process of making an app support different languages and regions.</span></span>
* <span data-ttu-id="e0142-326">當地語系化 (L10N): 自訂特定的語言和地區的應用程式的過程。</span><span class="sxs-lookup"><span data-stu-id="e0142-326">Localization (L10N): The process of customizing an app for a given language and region.</span></span>
* <span data-ttu-id="e0142-327">國際化 (I18N): 描述全球化和當地語系化。</span><span class="sxs-lookup"><span data-stu-id="e0142-327">Internationalization (I18N): Describes both globalization and localization.</span></span>
* <span data-ttu-id="e0142-328">文化特性： 它是一種語言和 （選擇性） 地區。</span><span class="sxs-lookup"><span data-stu-id="e0142-328">Culture: It is a language and, optionally, a region.</span></span>
* <span data-ttu-id="e0142-329">中性文化特性： 具有指定的語言，而不是地區的文化特性。</span><span class="sxs-lookup"><span data-stu-id="e0142-329">Neutral culture: A culture that has a specified language, but not a region.</span></span> <span data-ttu-id="e0142-330">(例如"en"，"es")</span><span class="sxs-lookup"><span data-stu-id="e0142-330">(for example "en", "es")</span></span>
* <span data-ttu-id="e0142-331">特定文化特性： 具有指定的語言和地區的文化特性。</span><span class="sxs-lookup"><span data-stu-id="e0142-331">Specific culture: A culture that has a specified language and region.</span></span> <span data-ttu-id="e0142-332">（適用於範例"EN-US"、"EN-GB"、"es CL"）</span><span class="sxs-lookup"><span data-stu-id="e0142-332">(for example "en-US", "en-GB", "es-CL")</span></span>
* <span data-ttu-id="e0142-333">地區設定： 地區設定是文化特性相同。</span><span class="sxs-lookup"><span data-stu-id="e0142-333">Locale: A locale is the same as a culture.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e0142-334">其他資源</span><span class="sxs-lookup"><span data-stu-id="e0142-334">Additional resources</span></span>

* <span data-ttu-id="e0142-335">[Localization.StarterWeb 專案](https://github.com/aspnet/entropy)用於發行項。</span><span class="sxs-lookup"><span data-stu-id="e0142-335">[Localization.StarterWeb project](https://github.com/aspnet/entropy) used in the article.</span></span>
* [<span data-ttu-id="e0142-336">Visual Studio 中的資源檔</span><span class="sxs-lookup"><span data-stu-id="e0142-336">Resource Files in Visual Studio</span></span>](https://docs.microsoft.com/cpp/windows/resource-files-visual-studio)
* [<span data-ttu-id="e0142-337">.Resx 檔中的資源</span><span class="sxs-lookup"><span data-stu-id="e0142-337">Resources in .resx Files</span></span>](https://docs.microsoft.com/dotnet/framework/resources/working-with-resx-files-programmatically)
