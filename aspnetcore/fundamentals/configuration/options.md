---
title: "在 ASP.NET Core 選項模式"
author: guardrex
description: "了解如何使用選項模式來代表一組相關的設定，ASP.NET Core 應用程式中。"
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 11/28/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration/options
ms.openlocfilehash: 7d89416626433bf737b63eda4b17e65b089ae142
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/29/2017
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="e108e-103">在 ASP.NET Core 選項模式</span><span class="sxs-lookup"><span data-stu-id="e108e-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="e108e-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e108e-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="e108e-105">選項模式使用的選項類別來代表一組相關的設定。</span><span class="sxs-lookup"><span data-stu-id="e108e-105">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="e108e-106">當組態設定會隔離到不同的選項類別功能時，應用程式就會遵守兩個重要的軟體工程原則：</span><span class="sxs-lookup"><span data-stu-id="e108e-106">When configuration settings are isolated by feature into separate options classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="e108e-107">[介面隔離原則 」 (ISP)](http://deviq.com/interface-segregation-principle/)： 取決於組態設定的功能 （類別） 取決於它們所使用的組態設定。</span><span class="sxs-lookup"><span data-stu-id="e108e-107">The [Interface Segregation Principle (ISP)](http://deviq.com/interface-segregation-principle/): Features (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="e108e-108">[重要性分離](http://deviq.com/separation-of-concerns/)： 應用程式的不同部分的設定不相依或彼此結合。</span><span class="sxs-lookup"><span data-stu-id="e108e-108">[Separation of Concerns](http://deviq.com/separation-of-concerns/): Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="e108e-109">[檢視或下載範例程式碼](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)([如何下載](xref:tutorials/index#how-to-download-a-sample)) 這篇文章會更輕鬆地遵循範例應用程式使用。</span><span class="sxs-lookup"><span data-stu-id="e108e-109">[View or download sample code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)) This article is easier to follow with the sample app.</span></span>

## <a name="basic-options-configuration"></a><span data-ttu-id="e108e-110">基本選項設定</span><span class="sxs-lookup"><span data-stu-id="e108e-110">Basic options configuration</span></span>

<span data-ttu-id="e108e-111">範例示範基本的選項組態&num;1[範例應用程式](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)。</span><span class="sxs-lookup"><span data-stu-id="e108e-111">Basic options configuration is demonstrated as Example &num;1 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="e108e-112">必須非抽象的選項類別。 具有公用的無參數建構函式。</span><span class="sxs-lookup"><span data-stu-id="e108e-112">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="e108e-113">下列類別`MyOptions`，有兩個屬性，`Option1`和`Option2`。</span><span class="sxs-lookup"><span data-stu-id="e108e-113">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="e108e-114">設定預設值是選擇性的但在下列範例中的類別建構函式設定的預設值`Option1`。</span><span class="sxs-lookup"><span data-stu-id="e108e-114">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="e108e-115">`Option2`直接初始化屬性所設定的預設值 (*Models/MyOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="e108e-115">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[Main](options/sample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="e108e-116">`MyOptions`類別加入至服務容器具有[IConfigureOptions&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1)和繫結至設定：</span><span class="sxs-lookup"><span data-stu-id="e108e-116">The `MyOptions` class is added to the service container with [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) and bound to configuration:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="e108e-117">下列頁面上的模型使用[建構函式相依性插入](xref:fundamentals/dependency-injection#what-is-dependency-injection)與[IOptions&lt;TOptions&gt; ](/dotnet/api/Microsoft.Extensions.Options.IOptions-1)存取設定 (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="e108e-117">The following page model uses [constructor dependency injection](xref:fundamentals/dependency-injection#what-is-dependency-injection) with [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="e108e-118">此範例*appsettings.json*檔案指定的值`option1`和`option2`:</span><span class="sxs-lookup"><span data-stu-id="e108e-118">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[Main](options/sample/appsettings.json)]

<span data-ttu-id="e108e-119">當應用程式執行時，頁面模型`OnGet`方法會傳回字串，顯示的選項類別值：</span><span class="sxs-lookup"><span data-stu-id="e108e-119">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="e108e-120">將委派設定簡單的選項</span><span class="sxs-lookup"><span data-stu-id="e108e-120">Configure simple options with a delegate</span></span>

<span data-ttu-id="e108e-121">設定簡單的選項，將委派與範例會示範&num;2[範例應用程式](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)。</span><span class="sxs-lookup"><span data-stu-id="e108e-121">Configuring simple options with a delegate is demonstrated as Example &num;2 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="e108e-122">您可以使用委派來設定選項值。</span><span class="sxs-lookup"><span data-stu-id="e108e-122">Use a delegate to set options values.</span></span> <span data-ttu-id="e108e-123">範例應用程式會使用`MyOptionsWithDelegateConfig`類別 (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="e108e-123">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[Main](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="e108e-124">下列程式碼中，第二個`IConfigureOptions<TOptions>`服務新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="e108e-124">In the following code, a second `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="e108e-125">它使用委派來設定的繫結`MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="e108e-125">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="e108e-126">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="e108e-126">*Index.cshtml.cs*:</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="e108e-127">您可以加入多個組態提供者。</span><span class="sxs-lookup"><span data-stu-id="e108e-127">You can add multiple configuration providers.</span></span> <span data-ttu-id="e108e-128">使用 NuGet 封裝中的組態提供者。</span><span class="sxs-lookup"><span data-stu-id="e108e-128">Configuration providers are available in NuGet packages.</span></span> <span data-ttu-id="e108e-129">它們被套用順序來進行登錄。</span><span class="sxs-lookup"><span data-stu-id="e108e-129">They're applied in order that they're registered.</span></span>

<span data-ttu-id="e108e-130">每次呼叫[設定&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure)新增`IConfigureOptions<TOptions>`服務的服務容器。</span><span class="sxs-lookup"><span data-stu-id="e108e-130">Each call to [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) adds an `IConfigureOptions<TOptions>` service to the service container.</span></span> <span data-ttu-id="e108e-131">在上述範例中，值`Option1`和`Option2`中已指定*appsettings.json*，但值`Option1`和`Option2`會覆寫設定的委派。</span><span class="sxs-lookup"><span data-stu-id="e108e-131">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="e108e-132">啟用多個設定服務時，要指定最後一個組態來源*wins*並設定組態值。</span><span class="sxs-lookup"><span data-stu-id="e108e-132">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="e108e-133">當應用程式執行時，頁面模型`OnGet`方法會傳回字串，顯示的選項類別值：</span><span class="sxs-lookup"><span data-stu-id="e108e-133">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="e108e-134">Suboptions 組態</span><span class="sxs-lookup"><span data-stu-id="e108e-134">Suboptions configuration</span></span>

<span data-ttu-id="e108e-135">範例將示範 suboptions 組態&num;3[範例應用程式](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)。</span><span class="sxs-lookup"><span data-stu-id="e108e-135">Suboptions configuration is demonstrated as Example &num;3 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="e108e-136">應用程式應該建立屬於特定的功能群組 （類別） 的應用程式中的選項類別。</span><span class="sxs-lookup"><span data-stu-id="e108e-136">Apps should create options classes that pertain to specific feature groups (classes) in the app.</span></span> <span data-ttu-id="e108e-137">應用程式需要的組態值的組件應該只能存取他們使用的組態值。</span><span class="sxs-lookup"><span data-stu-id="e108e-137">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="e108e-138">當繫結組態選項，選項類型中的每一個屬性繫結至表單的組態機碼`property[:sub-property:]`。</span><span class="sxs-lookup"><span data-stu-id="e108e-138">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="e108e-139">比方說，`MyOptions.Option1`屬性繫結索引鍵`Option1`，這會從讀取`option1`屬性*appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="e108e-139">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="e108e-140">下列程式碼，而第三個`IConfigureOptions<TOptions>`服務新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="e108e-140">In the following code, a third `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="e108e-141">它會繫結`MySubOptions`區段`subsection`的*appsettings.json*檔案：</span><span class="sxs-lookup"><span data-stu-id="e108e-141">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="e108e-142">`GetSection`擴充方法需要[Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="e108e-142">The `GetSection` extension method requires the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet package.</span></span> <span data-ttu-id="e108e-143">如果應用程式使用[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage，封裝會自動包含。</span><span class="sxs-lookup"><span data-stu-id="e108e-143">If the app uses the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, the package is automatically included.</span></span>

<span data-ttu-id="e108e-144">此範例*appsettings.json*檔案會定義`subsection`成員索引鍵與`suboption1`和`suboption2`:</span><span class="sxs-lookup"><span data-stu-id="e108e-144">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[Main](options/sample/appsettings.json?highlight=4-7)]

<span data-ttu-id="e108e-145">`MySubOptions`類別定義的屬性，`SubOption1`和`SubOption2`，來保存的子選項值 (*Models/MySubOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="e108e-145">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the sub-option values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[Main](options/sample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="e108e-146">頁面模型`OnGet`方法以傳回字串的子選項值 (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="e108e-146">The page model's `OnGet` method returns a string with the sub-option values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="e108e-147">當應用程式執行時，`OnGet`方法會傳回字串，顯示類別值的子選項：</span><span class="sxs-lookup"><span data-stu-id="e108e-147">When the app is run, the `OnGet` method returns a string showing the sub-option class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="e108e-148">提供的檢視模型或直接檢視資料隱碼的選項</span><span class="sxs-lookup"><span data-stu-id="e108e-148">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="e108e-149">提供的檢視模型或直接檢視資料隱碼的選項也示範了範例&num;4[範例應用程式](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)。</span><span class="sxs-lookup"><span data-stu-id="e108e-149">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="e108e-150">您可以提供選項，檢視模型中，或是將，以便`IOptions<TOptions>`直接插入檢視 (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="e108e-150">Options can be supplied in a view model or by injecting `IOptions<TOptions>` directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="e108e-151">用於直接插入插入`IOptions<MyOptions>`與`@inject`指示詞：</span><span class="sxs-lookup"><span data-stu-id="e108e-151">For direct injection, inject `IOptions<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[Main](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

<span data-ttu-id="e108e-152">執行應用程式時，轉譯的頁面顯示的選項值：</span><span class="sxs-lookup"><span data-stu-id="e108e-152">When the app is run, the option values are shown in the rendered page:</span></span>

![選項值選項 1: value1_from_json 和選項 2： 從模型和檢視插入載入-1。](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="e108e-154">重新載入 IOptionsSnapshot 組態資料</span><span class="sxs-lookup"><span data-stu-id="e108e-154">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="e108e-155">重新載入組態資料與`IOptionsSnapshot`範例所示&num;5[範例應用程式](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)。</span><span class="sxs-lookup"><span data-stu-id="e108e-155">Reloading configuration data with `IOptionsSnapshot` is demonstrated in Example &num;5 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="e108e-156">*需要 ASP.NET Core 1.1 或更新版本。*</span><span class="sxs-lookup"><span data-stu-id="e108e-156">*Requires ASP.NET Core 1.1 or later.*</span></span>

<span data-ttu-id="e108e-157">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1)支援重新載入選項的最小處理負擔。</span><span class="sxs-lookup"><span data-stu-id="e108e-157">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) supports reloading options with minimal processing overhead.</span></span> <span data-ttu-id="e108e-158">在 ASP.NET Core 1.1 中，`IOptionsSnapshot`是快照集的[IOptionsMonitor&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1)和更新自動每當監視就會觸發根據的資料來源變更的變更。</span><span class="sxs-lookup"><span data-stu-id="e108e-158">In ASP.NET Core 1.1, `IOptionsSnapshot` is a snapshot of [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) and updates automatically whenever the monitor triggers changes based on the data source changing.</span></span> <span data-ttu-id="e108e-159">ASP.NET Core 2.0 版及更新版本中，選項會針對每個要求時存取並快取要求的存留期計算一次。</span><span class="sxs-lookup"><span data-stu-id="e108e-159">In ASP.NET Core 2.0 and later, options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="e108e-160">下列範例會示範如何為新`IOptionsSnapshot`之後，會建立*appsettings.json*變更 (*Pages/Index.cshtml.cs*)。</span><span class="sxs-lookup"><span data-stu-id="e108e-160">The following example demonstrates how a new `IOptionsSnapshot` is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="e108e-161">伺服器的多個要求會傳回所提供的常數值*appsettings.json*直到檔案變更，並設定重新載入檔案。</span><span class="sxs-lookup"><span data-stu-id="e108e-161">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="e108e-162">下圖顯示初始`option1`和`option2`值載入從*appsettings.json*檔案：</span><span class="sxs-lookup"><span data-stu-id="e108e-162">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="e108e-163">變更中的值*appsettings.json*檔案`value1_from_json UPDATED`和`200`。</span><span class="sxs-lookup"><span data-stu-id="e108e-163">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="e108e-164">儲存*appsettings.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="e108e-164">Save the *appsettings.json* file.</span></span> <span data-ttu-id="e108e-165">重新整理瀏覽器，以查看選項值，會更新：</span><span class="sxs-lookup"><span data-stu-id="e108e-165">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="e108e-166">名為 IConfigureNamedOptions 選項支援</span><span class="sxs-lookup"><span data-stu-id="e108e-166">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="e108e-167">名為選項支援[IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1)範例會示範&num;6[範例應用程式](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)。</span><span class="sxs-lookup"><span data-stu-id="e108e-167">Named options support with [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) is demonstrated as Example &num;6 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="e108e-168">*需要 ASP.NET Core 2.0 或更新版本。*</span><span class="sxs-lookup"><span data-stu-id="e108e-168">*Requires ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="e108e-169">*名為選項*支援可讓應用程式，以區別命名的選項設定。</span><span class="sxs-lookup"><span data-stu-id="e108e-169">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="e108e-170">範例應用程式，以宣告具名的選項[ConfigureNamedOptions&lt;TOptions&gt;。設定](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure)方法：</span><span class="sxs-lookup"><span data-stu-id="e108e-170">In the sample app, named options are declared with the [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) method:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="e108e-171">範例應用程式存取具名的選項與[IOptionsSnapshot&lt;TOptions&gt;。取得](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get)(*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="e108e-171">The sample app accesses the named options with [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="e108e-172">執行範例應用程式，會傳回具名的選項：</span><span class="sxs-lookup"><span data-stu-id="e108e-172">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="e108e-173">`named_options_1`值會提供從組態中，這會從載入*appsettings.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="e108e-173">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="e108e-174">`named_options_2`所提供的值：</span><span class="sxs-lookup"><span data-stu-id="e108e-174">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="e108e-175">`named_options_2`委派中`ConfigureServices`如`Option1`。</span><span class="sxs-lookup"><span data-stu-id="e108e-175">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="e108e-176">預設值為`Option2`提供`MyOptions`類別。</span><span class="sxs-lookup"><span data-stu-id="e108e-176">The default value for `Option2` provided by the `MyOptions` class.</span></span>

<span data-ttu-id="e108e-177">設定所有具有選項的具名執行個體[OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall)方法。</span><span class="sxs-lookup"><span data-stu-id="e108e-177">Configure all named options instances with the [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) method.</span></span> <span data-ttu-id="e108e-178">下列程式碼會設定`Option1`所有名為具有共通的值的組態執行個體。</span><span class="sxs-lookup"><span data-stu-id="e108e-178">The following code configures `Option1` for all named configuration instances with a common value.</span></span> <span data-ttu-id="e108e-179">手動加入下列程式碼`Configure`方法：</span><span class="sxs-lookup"><span data-stu-id="e108e-179">Add the following code manually to the `Configure` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="e108e-180">執行範例應用程式加入程式碼之後，就會產生下列結果：</span><span class="sxs-lookup"><span data-stu-id="e108e-180">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="e108e-181">ASP.NET Core 2.0 版及更新版本中，所有選項都被都具名執行個體。</span><span class="sxs-lookup"><span data-stu-id="e108e-181">In ASP.NET Core 2.0 and later, all options are named instances.</span></span> <span data-ttu-id="e108e-182">現有`IConfigureOption`執行個體視為目標`Options.DefaultName`執行個體，也就是`string.Empty`。</span><span class="sxs-lookup"><span data-stu-id="e108e-182">Existing `IConfigureOption` instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="e108e-183">`IConfigureNamedOptions`也會實作`IConfigureOptions`。</span><span class="sxs-lookup"><span data-stu-id="e108e-183">`IConfigureNamedOptions` also implements `IConfigureOptions`.</span></span> <span data-ttu-id="e108e-184">預設實作[IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([參考來源](https://github.com/aspnet/Options/blob/release/2.0.0/src/Microsoft.Extensions.Options/OptionsFactory.cs)) 已適當地使用每個邏輯。</span><span class="sxs-lookup"><span data-stu-id="e108e-184">The default implementation of the [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([reference source](https://github.com/aspnet/Options/blob/release/2.0.0/src/Microsoft.Extensions.Options/OptionsFactory.cs)) has logic to use each appropriately.</span></span> <span data-ttu-id="e108e-185">`null`具名的選項用來為目標的所有具名的執行個體，而不是特定的具名執行個體 ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall)和[PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall)使用此慣例時)。</span><span class="sxs-lookup"><span data-stu-id="e108e-185">The `null` named option is used to target all of the named instances instead of a specific named instance ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) and [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) use this convention).</span></span>

## <a name="ipostconfigureoptions"></a><span data-ttu-id="e108e-186">IPostConfigureOptions</span><span class="sxs-lookup"><span data-stu-id="e108e-186">IPostConfigureOptions</span></span>

<span data-ttu-id="e108e-187">*需要 ASP.NET Core 2.0 或更新版本。*</span><span class="sxs-lookup"><span data-stu-id="e108e-187">*Requires ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="e108e-188">設定與 postconfiguration [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1)。</span><span class="sxs-lookup"><span data-stu-id="e108e-188">Set postconfiguration with [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span></span> <span data-ttu-id="e108e-189">在所有執行 postconfiguration [IConfigureOptions&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1)便會進行設定：</span><span class="sxs-lookup"><span data-stu-id="e108e-189">Postconfiguration runs after all [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="e108e-190">[PostConfigure&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure)是可供後續設定具名的選項：</span><span class="sxs-lookup"><span data-stu-id="e108e-190">[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="e108e-191">使用[PostConfigureAll&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall)後設定所有名為組態執行個體：</span><span class="sxs-lookup"><span data-stu-id="e108e-191">Use [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) to post-configure all named configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="options-factory-monitoring-and-cache"></a><span data-ttu-id="e108e-192">選項 factory、 監視和快取</span><span class="sxs-lookup"><span data-stu-id="e108e-192">Options factory, monitoring, and cache</span></span>

<span data-ttu-id="e108e-193">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1)用於通知時`TOptions`變更執行個體。</span><span class="sxs-lookup"><span data-stu-id="e108e-193">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) is used for notifications when `TOptions` instances change.</span></span> <span data-ttu-id="e108e-194">`IOptionsMonitor`支援 reloadable 選項，變更通知，以及`IPostConfigureOptions`。</span><span class="sxs-lookup"><span data-stu-id="e108e-194">`IOptionsMonitor` supports reloadable options, change notifications, and `IPostConfigureOptions`.</span></span>

<span data-ttu-id="e108e-195">[IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) (ASP.NET Core 2.0 或更新版本) 負責建立新的選項執行個體。</span><span class="sxs-lookup"><span data-stu-id="e108e-195">[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) (ASP.NET Core 2.0 or later) is responsible for creating new options instances.</span></span> <span data-ttu-id="e108e-196">它有單一[建立](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create)方法。</span><span class="sxs-lookup"><span data-stu-id="e108e-196">It has a single [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) method.</span></span> <span data-ttu-id="e108e-197">預設實作會採用所有已註冊`IConfigureOptions`和`IPostConfigureOptions`並執行所有可設定第一次，後面接著後設定。</span><span class="sxs-lookup"><span data-stu-id="e108e-197">The default implementation takes all registered `IConfigureOptions` and `IPostConfigureOptions` and runs all the configures first, followed by the post-configures.</span></span> <span data-ttu-id="e108e-198">它會區別`IConfigureNamedOptions`和`IConfigureOptions`，只會呼叫適當的介面。</span><span class="sxs-lookup"><span data-stu-id="e108e-198">It distinguishes between `IConfigureNamedOptions` and `IConfigureOptions` and only calls the appropriate interface.</span></span>

<span data-ttu-id="e108e-199">[IOptionsMonitorCache&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (ASP.NET Core 2.0 或更新版本) 由`IOptionsMonitor`至快取`TOptions`執行個體。</span><span class="sxs-lookup"><span data-stu-id="e108e-199">[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (ASP.NET Core 2.0 or later) is used by `IOptionsMonitor` to cache `TOptions` instances.</span></span> <span data-ttu-id="e108e-200">`IOptionsMonitorCache`失效選項在監視器中的執行個體，以便重新計算值 ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove))。</span><span class="sxs-lookup"><span data-stu-id="e108e-200">The `IOptionsMonitorCache` invalidates options instances in the monitor so that the value is recomputed ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span></span> <span data-ttu-id="e108e-201">值可以手動導入以及使用[TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd)。</span><span class="sxs-lookup"><span data-stu-id="e108e-201">Values can be manually introduced as well with [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span></span> <span data-ttu-id="e108e-202">[清除](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear)方法應在需要重新建立所有具名執行個體時使用。</span><span class="sxs-lookup"><span data-stu-id="e108e-202">The [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) method is used when all named instances should be recreated on demand.</span></span>

## <a name="see-also"></a><span data-ttu-id="e108e-203">請參閱</span><span class="sxs-lookup"><span data-stu-id="e108e-203">See also</span></span>

* [<span data-ttu-id="e108e-204">組態</span><span class="sxs-lookup"><span data-stu-id="e108e-204">Configuration</span></span>](xref:fundamentals/configuration/index)
