---
title: ASP.NET Core 中的選項模式
author: guardrex
description: 了解如何使用選項模式來代表 ASP.NET Core 應用程式中的一組相關設定。
ms.author: riande
ms.custom: mvc
ms.date: 11/09/2018
uid: fundamentals/configuration/options
ms.openlocfilehash: 99aa5028a8704c7e9e3010415137e2560213a2ad
ms.sourcegitcommit: edb9d2d78c9a4d68b397e74ae2aff088b325a143
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/09/2018
ms.locfileid: "51505788"
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="19387-103">ASP.NET Core 中的選項模式</span><span class="sxs-lookup"><span data-stu-id="19387-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="19387-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="19387-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="19387-105">選項模式使用類別來代表一組相關的設定。</span><span class="sxs-lookup"><span data-stu-id="19387-105">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="19387-106">當[組態設定](xref:fundamentals/configuration/index)依案例隔離到不同的類別時，應用程式會遵守兩個重要的軟體工程準則：</span><span class="sxs-lookup"><span data-stu-id="19387-106">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="19387-107">[介面隔離準則 (ISP)](http://deviq.com/interface-segregation-principle/)：相依於組態設定的案例 (類別) 只會相依於它們使用的組態設定。</span><span class="sxs-lookup"><span data-stu-id="19387-107">The [Interface Segregation Principle (ISP)](http://deviq.com/interface-segregation-principle/): Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="19387-108">[關注點分離](http://deviq.com/separation-of-concerns/) (不同考量)：應用程式不同部分的設定不會彼此相依或結合。</span><span class="sxs-lookup"><span data-stu-id="19387-108">[Separation of Concerns](http://deviq.com/separation-of-concerns/): Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="19387-109">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([如何下載](xref:index#how-to-download-a-sample))本文比較能輕鬆地遵循範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="19387-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([how to download](xref:index#how-to-download-a-sample)) This article is easier to follow with the sample app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="19387-110">必要條件</span><span class="sxs-lookup"><span data-stu-id="19387-110">Prerequisites</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="19387-111">參考 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)，或新增 [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) 套件的套件參考。</span><span class="sxs-lookup"><span data-stu-id="19387-111">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="19387-112">參考 [Microsoft.AspNetCore.All 中繼套件](xref:fundamentals/metapackage)，或新增 [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) 套件的套件參考。</span><span class="sxs-lookup"><span data-stu-id="19387-112">Reference the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="19387-113">新增 [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) 套件的套件參考。</span><span class="sxs-lookup"><span data-stu-id="19387-113">Add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

::: moniker-end

## <a name="basic-options-configuration"></a><span data-ttu-id="19387-114">基本選項組態</span><span class="sxs-lookup"><span data-stu-id="19387-114">Basic options configuration</span></span>

<span data-ttu-id="19387-115">基本選項範例組態是以[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)中的範例 &num;1 來示範。</span><span class="sxs-lookup"><span data-stu-id="19387-115">Basic options configuration is demonstrated as Example &num;1 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="19387-116">選項類別必須為非抽象，且具有公用的無參數建構函式。</span><span class="sxs-lookup"><span data-stu-id="19387-116">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="19387-117">下列 `MyOptions` 類別有兩個屬性，`Option1` 和 `Option2`。</span><span class="sxs-lookup"><span data-stu-id="19387-117">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="19387-118">設定預設值為選擇性，但在下列範例中，類別建構函式會設定 `Option1` 的預設值。</span><span class="sxs-lookup"><span data-stu-id="19387-118">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="19387-119">`Option2` 的預設值直接藉由初始化屬性來設定 (*Models/MyOptions.cs*)：</span><span class="sxs-lookup"><span data-stu-id="19387-119">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/sample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="19387-120">`MyOptions` 類別使用 [Configure&lt;TOptions&gt;(IServiceCollection, IConfiguration)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsconfigurationservicecollectionextensions.configure#Microsoft_Extensions_DependencyInjection_OptionsConfigurationServiceCollectionExtensions_Configure__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_Microsoft_Extensions_Configuration_IConfiguration_) 新增至服務容器，並繫結至組態：</span><span class="sxs-lookup"><span data-stu-id="19387-120">The `MyOptions` class is added to the service container with [Configure&lt;TOptions&gt;(IServiceCollection, IConfiguration)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsconfigurationservicecollectionextensions.configure#Microsoft_Extensions_DependencyInjection_OptionsConfigurationServiceCollectionExtensions_Configure__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_Microsoft_Extensions_Configuration_IConfiguration_) and bound to configuration:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="19387-121">下列頁面模型使用[建構函式相依性插入](xref:mvc/controllers/dependency-injection)搭配 [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) 來存取設定 (*Pages/Index.cshtml.cs*)：</span><span class="sxs-lookup"><span data-stu-id="19387-121">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="19387-122">範例的 *appsettings.json* 檔案指定 `option1` 和 `option2` 的值：</span><span class="sxs-lookup"><span data-stu-id="19387-122">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/sample/appsettings.json?highlight=2-3)]

<span data-ttu-id="19387-123">當應用程式執行時，頁面模型的 `OnGet` 方法會傳回字串，顯示選項類別值：</span><span class="sxs-lookup"><span data-stu-id="19387-123">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="19387-124">使用自訂 [ConfigurationBuilder](/dotnet/api/system.configuration.configurationbuilder) 從設定檔載入選項組態時，請確認已正確設定基底路徑：</span><span class="sxs-lookup"><span data-stu-id="19387-124">When using a custom [ConfigurationBuilder](/dotnet/api/system.configuration.configurationbuilder) to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
>
> ```csharp
> var configBuilder = new ConfigurationBuilder()
>    .SetBasePath(Directory.GetCurrentDirectory())
>    .AddJsonFile("appsettings.json", optional: true);
> var config = configBuilder.Build();
>
> services.Configure<MyOptions>(config);
> ```
>
> <span data-ttu-id="19387-125">透過 [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) 從設定檔載入選項組態時，不需要明確設定基底路徑。</span><span class="sxs-lookup"><span data-stu-id="19387-125">Explicitly setting the base path isn't required when loading options configuration from the settings file via [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="19387-126">使用委派設定簡單的選項</span><span class="sxs-lookup"><span data-stu-id="19387-126">Configure simple options with a delegate</span></span>

<span data-ttu-id="19387-127">使用委派設定簡單的選項是以[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)中的範例 &num;2 來示範。</span><span class="sxs-lookup"><span data-stu-id="19387-127">Configuring simple options with a delegate is demonstrated as Example &num;2 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="19387-128">使用委派來設定選項值。</span><span class="sxs-lookup"><span data-stu-id="19387-128">Use a delegate to set options values.</span></span> <span data-ttu-id="19387-129">範例應用程式使用 `MyOptionsWithDelegateConfig` 類別 (*Models/MyOptionsWithDelegateConfig.cs*)：</span><span class="sxs-lookup"><span data-stu-id="19387-129">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="19387-130">在下列程式碼中，第二個 `IConfigureOptions<TOptions>` 服務新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="19387-130">In the following code, a second `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="19387-131">它使用委派，以 `MyOptionsWithDelegateConfig` 設定繫結：</span><span class="sxs-lookup"><span data-stu-id="19387-131">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="19387-132">*Index.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="19387-132">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="19387-133">您可以新增多個設定提供者。</span><span class="sxs-lookup"><span data-stu-id="19387-133">You can add multiple configuration providers.</span></span> <span data-ttu-id="19387-134">設定提供者提供於 NuGet 套件中。</span><span class="sxs-lookup"><span data-stu-id="19387-134">Configuration providers are available in NuGet packages.</span></span> <span data-ttu-id="19387-135">它們會以註冊的順序套用。</span><span class="sxs-lookup"><span data-stu-id="19387-135">They're applied in the order that they're registered.</span></span>

<span data-ttu-id="19387-136">每次呼叫 [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) 會新增 `IConfigureOptions<TOptions>` 服務至服務容器。</span><span class="sxs-lookup"><span data-stu-id="19387-136">Each call to [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) adds an `IConfigureOptions<TOptions>` service to the service container.</span></span> <span data-ttu-id="19387-137">在上述範例中，`Option1` 和 `Option2` 的值都指定在 *appsettings.json* 中，但 `Option1` 和 `Option2` 的值會被設定的委派所覆寫。</span><span class="sxs-lookup"><span data-stu-id="19387-137">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="19387-138">啟用多個設定服務時，最後一個指定的設定來源會「勝出」並設定此組態值。</span><span class="sxs-lookup"><span data-stu-id="19387-138">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="19387-139">當應用程式執行時，頁面模型的 `OnGet` 方法會傳回字串，顯示選項類別值：</span><span class="sxs-lookup"><span data-stu-id="19387-139">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="19387-140">子選項組態</span><span class="sxs-lookup"><span data-stu-id="19387-140">Suboptions configuration</span></span>

<span data-ttu-id="19387-141">子選項組態是以[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)中的範例 &num;3 來示範。</span><span class="sxs-lookup"><span data-stu-id="19387-141">Suboptions configuration is demonstrated as Example &num;3 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="19387-142">應用程式應該建立屬於應用程式中特定案例群組 (類別) 的選項類別。</span><span class="sxs-lookup"><span data-stu-id="19387-142">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="19387-143">需要組態值的應用程式組件應該只能存取它們使用的設定值。</span><span class="sxs-lookup"><span data-stu-id="19387-143">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="19387-144">將選項繫結至組態時，選項類型中的每一個屬性都會繫結至 `property[:sub-property:]` 格式的組態索引鍵。</span><span class="sxs-lookup"><span data-stu-id="19387-144">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="19387-145">例如，`MyOptions.Option1` 屬性繫結至索引鍵 `Option1`，其是從 *appsettings.json* 中的 `option1` 屬性讀取。</span><span class="sxs-lookup"><span data-stu-id="19387-145">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="19387-146">在下列程式碼中，第三個 `IConfigureOptions<TOptions>` 服務新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="19387-146">In the following code, a third `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="19387-147">它將 `MySubOptions` 繫結至 *appSettings.json* 檔案的 `subsection` 區段：</span><span class="sxs-lookup"><span data-stu-id="19387-147">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="19387-148">`GetSection` 擴充方法需要 [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="19387-148">The `GetSection` extension method requires the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet package.</span></span> <span data-ttu-id="19387-149">如果應用程式使用 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 或更新版本)，則會自動包含套件。</span><span class="sxs-lookup"><span data-stu-id="19387-149">If the app uses the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later), the package is automatically included.</span></span>

<span data-ttu-id="19387-150">範例的 *appsettings.json* 檔案會定義 `subsection` 成員，並具有 `suboption1` 和 `suboption2` 的索引鍵：</span><span class="sxs-lookup"><span data-stu-id="19387-150">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/sample/appsettings.json?highlight=4-7)]

<span data-ttu-id="19387-151">`MySubOptions` 類別會定義屬性 `SubOption1` 和 `SubOption2`，來保存選項值 (*Models/MySubOptions.cs*)：</span><span class="sxs-lookup"><span data-stu-id="19387-151">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/sample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="19387-152">頁面模型的 `OnGet` 方法會傳回具有選項值 (*Pages/Index.cshtml.cs*) 的字串：</span><span class="sxs-lookup"><span data-stu-id="19387-152">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="19387-153">當應用程式執行時，`OnGet` 方法會傳回字串，顯示子選項類別值：</span><span class="sxs-lookup"><span data-stu-id="19387-153">When the app is run, the `OnGet` method returns a string showing the sub-option class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="19387-154">檢視模型提供的選項或使用直接檢視插入提供的選項</span><span class="sxs-lookup"><span data-stu-id="19387-154">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="19387-155">檢視模型提供的選項或使用直接檢視插入提供的選項是以[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)中的範例 &num;4 來示範。</span><span class="sxs-lookup"><span data-stu-id="19387-155">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="19387-156">可以在檢視模型中提供選項，或藉由將 `IOptions<TOptions>` 直接插入至檢視來提供選項 (*Pages/Index.cshtml.cs*)：</span><span class="sxs-lookup"><span data-stu-id="19387-156">Options can be supplied in a view model or by injecting `IOptions<TOptions>` directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="19387-157">對於直接插入，請使用 `@inject` 指示詞插入 `IOptions<MyOptions>`：</span><span class="sxs-lookup"><span data-stu-id="19387-157">For direct injection, inject `IOptions<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

<span data-ttu-id="19387-158">執行應用程式時，轉譯的頁面中會顯示選項值：</span><span class="sxs-lookup"><span data-stu-id="19387-158">When the app is run, the options values are shown in the rendered page:</span></span>

![選項值 Option1:：value1_from_json 和 Option2: -1 是從模型藉由插入至檢視來載入。](options/_static/view.png)

::: moniker range=">= aspnetcore-1.1"

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="19387-160">使用 IOptionsSnapshot 重新載入設定資料</span><span class="sxs-lookup"><span data-stu-id="19387-160">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="19387-161">使用 `IOptionsSnapshot` 重新載入組態資料是以[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)中的範例 &num;5 來示範。</span><span class="sxs-lookup"><span data-stu-id="19387-161">Reloading configuration data with `IOptionsSnapshot` is demonstrated in Example &num;5 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="19387-162">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) 支援以最小的處理負擔來重新載入選項。</span><span class="sxs-lookup"><span data-stu-id="19387-162">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) supports reloading options with minimal processing overhead.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="19387-163">在要求的存留期內存取及快取選項時，會針對每個要求計算一次選項。</span><span class="sxs-lookup"><span data-stu-id="19387-163">Options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="19387-164">`IOptionsSnapshot` 是 [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) 的快照集，每當監視器根據資料來源變更觸發變更時，會自動更新。</span><span class="sxs-lookup"><span data-stu-id="19387-164">`IOptionsSnapshot` is a snapshot of [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) and updates automatically whenever the monitor triggers changes based on the data source changing.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="19387-165">下列範例示範在 *appsettings.json* 變更之後如何建立新的 `IOptionsSnapshot` (*Pages/Index.cshtml.cs*)。</span><span class="sxs-lookup"><span data-stu-id="19387-165">The following example demonstrates how a new `IOptionsSnapshot` is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="19387-166">對伺服器的多個要求會傳回 *appsettings.json* 檔案所提供的常數值，直到檔案變更並重新載入組態為止。</span><span class="sxs-lookup"><span data-stu-id="19387-166">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="19387-167">下圖顯示從 *appsettings.json* 檔案載入的初始 `option1` 和 `option2` 值：</span><span class="sxs-lookup"><span data-stu-id="19387-167">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="19387-168">將 *appsettings.json* 檔案中的值變更為 `value1_from_json UPDATED` 和 `200`。</span><span class="sxs-lookup"><span data-stu-id="19387-168">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="19387-169">儲存 *appsettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="19387-169">Save the *appsettings.json* file.</span></span> <span data-ttu-id="19387-170">重新整理瀏覽器，以查看選項值更新：</span><span class="sxs-lookup"><span data-stu-id="19387-170">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="19387-171">IConfigureNamedOptions 的具名選項支援</span><span class="sxs-lookup"><span data-stu-id="19387-171">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="19387-172">[IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) 的具名選項支援是以[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)中的範例 &num;6 來示範。</span><span class="sxs-lookup"><span data-stu-id="19387-172">Named options support with [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) is demonstrated as Example &num;6 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="19387-173">「具名選項」支援可讓應用程式區別具名選項組態。</span><span class="sxs-lookup"><span data-stu-id="19387-173">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="19387-174">在範例應用程式中，具名選項會使用 [OptionsServiceCollectionExtensions.Configure&lt;TOptions&gt;(IServiceCollection, String, Action&lt;TOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configure) 進行宣告，而後者又會呼叫擴充方法 [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) 方法：</span><span class="sxs-lookup"><span data-stu-id="19387-174">In the sample app, named options are declared with the [OptionsServiceCollectionExtensions.Configure&lt;TOptions&gt;(IServiceCollection, String, Action&lt;TOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configure) which in turn calls the extension method [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) method:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="19387-175">範例應用程式會使用[IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) 來存取具名選項 (*Pages/Index.cshtml.cs*)：</span><span class="sxs-lookup"><span data-stu-id="19387-175">The sample app accesses the named options with [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="19387-176">執行範例應用程式後，會傳回具名選項：</span><span class="sxs-lookup"><span data-stu-id="19387-176">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="19387-177">`named_options_1` 值會從設定提供，這會從 *appsettings.json* 檔案載入。</span><span class="sxs-lookup"><span data-stu-id="19387-177">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="19387-178">`named_options_2` 值是由以下提供：</span><span class="sxs-lookup"><span data-stu-id="19387-178">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="19387-179">`ConfigureServices` 中 `Option1` 的 `named_options_2` 委派。</span><span class="sxs-lookup"><span data-stu-id="19387-179">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="19387-180">`MyOptions` 類別所提供的 `Option2` 預設值。</span><span class="sxs-lookup"><span data-stu-id="19387-180">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="19387-181">使用 ConfigureAll 方法設定所有選項</span><span class="sxs-lookup"><span data-stu-id="19387-181">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="19387-182">使用 [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) 方法設定所有選項執行個體。</span><span class="sxs-lookup"><span data-stu-id="19387-182">Configure all options instances with the [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) method.</span></span> <span data-ttu-id="19387-183">下列程式碼會為具有共通值的所有設定執行個體設定 `Option1`。</span><span class="sxs-lookup"><span data-stu-id="19387-183">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="19387-184">將下列程式碼手動新增至 `ConfigureServices` 方法：</span><span class="sxs-lookup"><span data-stu-id="19387-184">Add the following code manually to the `ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="19387-185">新增程式碼之後執行範例應用程式，就會產生下列結果：</span><span class="sxs-lookup"><span data-stu-id="19387-185">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="19387-186">所有選項都是具名執行個體。</span><span class="sxs-lookup"><span data-stu-id="19387-186">All options are named instances.</span></span> <span data-ttu-id="19387-187">現有的 `IConfigureOption` 執行個體會視為以 `Options.DefaultName` 執行個體為目標，也就是 `string.Empty`。</span><span class="sxs-lookup"><span data-stu-id="19387-187">Existing `IConfigureOption` instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="19387-188">`IConfigureNamedOptions` 也會實作 `IConfigureOptions`。</span><span class="sxs-lookup"><span data-stu-id="19387-188">`IConfigureNamedOptions` also implements `IConfigureOptions`.</span></span> <span data-ttu-id="19387-189">[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) 的預設實作 ([參考來源](https://github.com/aspnet/Options/blob/release/2.0/src/Microsoft.Extensions.Options/IOptionsFactory.cs)) 有邏輯可適當地使用每一個。</span><span class="sxs-lookup"><span data-stu-id="19387-189">The default implementation of the [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([reference source](https://github.com/aspnet/Options/blob/release/2.0/src/Microsoft.Extensions.Options/IOptionsFactory.cs) has logic to use each appropriately.</span></span> <span data-ttu-id="19387-190">`null` 具名選項用來以所有具名執行個體為目標，而不是特定的具名執行個體 ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) 和 [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) 使用此慣例)。</span><span class="sxs-lookup"><span data-stu-id="19387-190">The `null` named option is used to target all of the named instances instead of a specific named instance ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) and [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) use this convention).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="optionsbuilder-api"></a><span data-ttu-id="19387-191">OptionsBuilder API</span><span class="sxs-lookup"><span data-stu-id="19387-191">OptionsBuilder API</span></span>

<span data-ttu-id="19387-192"><xref:Microsoft.Extensions.Options.OptionsBuilder`1> 會用於設定 `TOptions` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="19387-192"><xref:Microsoft.Extensions.Options.OptionsBuilder`1> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="19387-193">因為 `OptionsBuilder` 僅為初始 `AddOptions<TOptions>(string optionsName)` 呼叫的單一參數，而不是出現在所有後續呼叫的參數，所以其可簡化建立具名選項的程序。</span><span class="sxs-lookup"><span data-stu-id="19387-193">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="19387-194">選項驗證及接受服務依存性的 `ConfigureOptions` 多載，只可透過 `OptionsBuilder` 使用。</span><span class="sxs-lookup"><span data-stu-id="19387-194">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");
    
services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="configurelttoptions-tdep1--tdep4gt-method"></a><span data-ttu-id="19387-195">設定 &lt;TOptions, TDep1, ...TDep4&gt; 方法</span><span class="sxs-lookup"><span data-stu-id="19387-195">Configure&lt;TOptions, TDep1, ... TDep4&gt; method</span></span>

<span data-ttu-id="19387-196">透過以未定案的方式實作 `IConfigure[Named]Options`，來使用從 DI 到設定選項的服務是多餘的。</span><span class="sxs-lookup"><span data-stu-id="19387-196">Using services from DI to configure options by implementing `IConfigure[Named]Options` in a boilerplate manner is verbose.</span></span> <span data-ttu-id="19387-197">`OptionsBuilder<TOptions>`上 `ConfigureOptions` 的多載可讓您使用最多五個服務來設定選項：</span><span class="sxs-lookup"><span data-stu-id="19387-197">Overloads for `ConfigureOptions` on `OptionsBuilder<TOptions>` allow you to use up to five services to configure options:</span></span>

```csharp
services.AddOptions<MyOptions>("optionalName")
    .Configure<Service1, Service2, Service3, Service4, Service5>(
        (o, s, s2, s3, s4, s5) => 
            o.Property = DoSomethingWith(s, s2, s3, s4, s5));
```

<span data-ttu-id="19387-198">多載會註冊暫時性泛型 <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1>，其具有會接受所指定泛型服務類型的建構函式。</span><span class="sxs-lookup"><span data-stu-id="19387-198">The overload registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1>, which has a constructor that accepts the generic service types specified.</span></span> 

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

## <a name="options-validation"></a><span data-ttu-id="19387-199">選項驗證</span><span class="sxs-lookup"><span data-stu-id="19387-199">Options validation</span></span>

<span data-ttu-id="19387-200">選項驗證可讓您在設定選項之後驗證選項。</span><span class="sxs-lookup"><span data-stu-id="19387-200">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="19387-201">搭配驗證方法呼叫 `Validate`，若選項有效會傳回 `true`，若為無效則傳回 `false`：</span><span class="sxs-lookup"><span data-stu-id="19387-201">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

```csharp
// Registration
services.AddOptions<MyOptions>("optionalOptionsName")
    .Configure(o => { }) // Configure the options
    .Validate(o => YourValidationShouldReturnTrueIfValid(o), 
        "custom error");
        
// Consumption
var monitor = services.BuildServiceProvider()
    .GetService<IOptionsMonitor<MyOptions>>();
  
try
{
    var options = monitor.Get("optionalOptionsName");
} 
catch (OptionsValidationException e) 
{
   // e.OptionsName returns "optionalOptionsName"
   // e.OptionsType returns typeof(MyOptions)
   // e.Failures returns a list of errors, which would contain 
   //     "custom error"
}
```

<span data-ttu-id="19387-202">上述範例會將具名的選項執行個體設定為 `optionalOptionsName`。</span><span class="sxs-lookup"><span data-stu-id="19387-202">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="19387-203">預設選項執行個體為 `Options.DefaultName`。</span><span class="sxs-lookup"><span data-stu-id="19387-203">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="19387-204">當選項執行個體建立之後，便會執行驗證。</span><span class="sxs-lookup"><span data-stu-id="19387-204">Validation runs when the options instance is created.</span></span> <span data-ttu-id="19387-205">您的選項執行個體保證會在第一次存取時傳遞驗證。</span><span class="sxs-lookup"><span data-stu-id="19387-205">Your options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="19387-206">選項一開始設定並驗證之後，選項驗證不會防範選項的修改。</span><span class="sxs-lookup"><span data-stu-id="19387-206">Options validation doesn't guard against options modifications after the options are initially configured and validated.</span></span>

<span data-ttu-id="19387-207">`Validate` 方法可接受 `Func<TOptions, bool>`。</span><span class="sxs-lookup"><span data-stu-id="19387-207">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="19387-208">若要完整地自訂驗證，請實作 `IValidateOptions<TOptions>`，它允許：</span><span class="sxs-lookup"><span data-stu-id="19387-208">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="19387-209">多種選項類型的驗證：`class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span><span class="sxs-lookup"><span data-stu-id="19387-209">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="19387-210">取決於另一個選項類型的驗證：`public DependsOnAnotherOptionValidator(IOptions<AnotherOption> options)`</span><span class="sxs-lookup"><span data-stu-id="19387-210">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptions<AnotherOption> options)`</span></span>

<span data-ttu-id="19387-211">`IValidateOptions` 可驗證：</span><span class="sxs-lookup"><span data-stu-id="19387-211">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="19387-212">特定的具名選項執行個體。</span><span class="sxs-lookup"><span data-stu-id="19387-212">A specific named options instance.</span></span>
* <span data-ttu-id="19387-213">所有選項 (當 `name` 為 `null` 時)。</span><span class="sxs-lookup"><span data-stu-id="19387-213">All options when `name` is `null`.</span></span>

<span data-ttu-id="19387-214">從以下的介面實作傳回 `ValidateOptionsResult`：</span><span class="sxs-lookup"><span data-stu-id="19387-214">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="19387-215">透過呼叫 `OptionsBuilder<TOptions>` 上的 `ValidateDataAnnotations` 方法，就可從 [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) 套件使用資料註解型驗證：</span><span class="sxs-lookup"><span data-stu-id="19387-215">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the `ValidateDataAnnotations` method on `OptionsBuilder<TOptions>`:</span></span>

```csharp
private class AnnotatedOptions
{
    [Required]
    public string Required { get; set; }

    [StringLength(5, ErrorMessage = "Too long.")]
    public string StringLength { get; set; }

    [Range(-5, 5, ErrorMessage = "Out of range.")]
    public int IntRange { get; set; }
}
    
[Fact]
public void CanValidateDataAnnotations()
{
    var services = new ServiceCollection();
    services.AddOptions<AnnotatedOptions>()
        .Configure(o =>
        {
            o.StringLength = "111111";
            o.IntRange = 10;
            o.Custom = "nowhere";
        })
        .ValidateDataAnnotations();

    var sp = services.BuildServiceProvider();

    var error = Assert.Throws<OptionsValidationException>(() => 
        sp.GetRequiredService<IOptions<AnnotatedOptions>>().Value);
    ValidateFailure<AnnotatedOptions>(error, Options.DefaultName, 1,
        "DataAnnotation validation failed for members Required " +
            "with the error 'The Required field is required.'.",
        "DataAnnotation validation failed for members StringLength " +
            "with the error 'Too long.'.",
        "DataAnnotation validation failed for members IntRange " +
            "with the error 'Out of range.'.");
}    
```

<span data-ttu-id="19387-216">我們考慮在之後的版本中加入積極式驗證 (啟動時快速檢錯)。</span><span class="sxs-lookup"><span data-stu-id="19387-216">Eager validation (fail fast at startup) is under consideration for a future release.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

## <a name="ipostconfigureoptions"></a><span data-ttu-id="19387-217">IPostConfigureOptions</span><span class="sxs-lookup"><span data-stu-id="19387-217">IPostConfigureOptions</span></span>

<span data-ttu-id="19387-218">使用 [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1) 設定後置組態。</span><span class="sxs-lookup"><span data-stu-id="19387-218">Set postconfiguration with [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span></span> <span data-ttu-id="19387-219">後置組態會在所有 [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) 組態都發生之後執行：</span><span class="sxs-lookup"><span data-stu-id="19387-219">Postconfiguration runs after all [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="19387-220">[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) 可以用來後置設定具名選項：</span><span class="sxs-lookup"><span data-stu-id="19387-220">[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="19387-221">使用 [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) 後置設定所有設定執行個體：</span><span class="sxs-lookup"><span data-stu-id="19387-221">Use [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

::: moniker-end

## <a name="options-factory-monitoring-and-cache"></a><span data-ttu-id="19387-222">選項 Factory、監視和快取</span><span class="sxs-lookup"><span data-stu-id="19387-222">Options factory, monitoring, and cache</span></span>

<span data-ttu-id="19387-223">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) 用於 `TOptions` 執行個體變更時的通知。</span><span class="sxs-lookup"><span data-stu-id="19387-223">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) is used for notifications when `TOptions` instances change.</span></span> <span data-ttu-id="19387-224">`IOptionsMonitor` 支援可重新載入的選項、變更通知，以及 `IPostConfigureOptions`。</span><span class="sxs-lookup"><span data-stu-id="19387-224">`IOptionsMonitor` supports reloadable options, change notifications, and `IPostConfigureOptions`.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="19387-225">[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) 負責建立新的選項執行個體。</span><span class="sxs-lookup"><span data-stu-id="19387-225">[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) is responsible for creating new options instances.</span></span> <span data-ttu-id="19387-226">它有一個 [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) 方法。</span><span class="sxs-lookup"><span data-stu-id="19387-226">It has a single [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) method.</span></span> <span data-ttu-id="19387-227">預設實作會採用所有已註冊的 `IConfigureOptions` 和 `IPostConfigureOptions`，並先執行所有設定，接著執行後置設定。</span><span class="sxs-lookup"><span data-stu-id="19387-227">The default implementation takes all registered `IConfigureOptions` and `IPostConfigureOptions` and runs all the configures first, followed by the post-configures.</span></span> <span data-ttu-id="19387-228">它會區別 `IConfigureNamedOptions` 和 `IConfigureOptions`，且只會呼叫適當的介面。</span><span class="sxs-lookup"><span data-stu-id="19387-228">It distinguishes between `IConfigureNamedOptions` and `IConfigureOptions` and only calls the appropriate interface.</span></span>

<span data-ttu-id="19387-229">[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) 由 `IOptionsMonitor` 用來快取 `TOptions` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="19387-229">[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) is used by `IOptionsMonitor` to cache `TOptions` instances.</span></span> <span data-ttu-id="19387-230">`IOptionsMonitorCache` 會使監視器中的選項執行個體失效，以便重新計算值 ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove))。</span><span class="sxs-lookup"><span data-stu-id="19387-230">The `IOptionsMonitorCache` invalidates options instances in the monitor so that the value is recomputed ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span></span> <span data-ttu-id="19387-231">值可以手動導入，也能使用 [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd)。</span><span class="sxs-lookup"><span data-stu-id="19387-231">Values can be manually introduced as well with [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span></span> <span data-ttu-id="19387-232">[Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) 方法用於應該視需要重新建立所有具名執行個體時。</span><span class="sxs-lookup"><span data-stu-id="19387-232">The [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) method is used when all named instances should be recreated on demand.</span></span>

::: moniker-end

## <a name="accessing-options-during-startup"></a><span data-ttu-id="19387-233">在啟動期間存取選項</span><span class="sxs-lookup"><span data-stu-id="19387-233">Accessing options during startup</span></span>

<span data-ttu-id="19387-234">`IOptions` 可用於 `Startup.Configure`，因為服務是在 `Configure` 方法執行之前建置。</span><span class="sxs-lookup"><span data-stu-id="19387-234">`IOptions` can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptions<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.Value.Option1;
}
```

<span data-ttu-id="19387-235">`IOptions` 不應該在 `Startup.ConfigureServices` 中使用。</span><span class="sxs-lookup"><span data-stu-id="19387-235">`IOptions` shouldn't be used in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="19387-236">可能會因為服務註冊的順序而有不一致的選項狀態存在。</span><span class="sxs-lookup"><span data-stu-id="19387-236">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="19387-237">其他資源</span><span class="sxs-lookup"><span data-stu-id="19387-237">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
