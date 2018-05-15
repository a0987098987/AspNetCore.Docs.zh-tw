---
title: ASP.NET Core 中的選項模式
author: guardrex
description: 了解如何使用選項模式來代表 ASP.NET Core 應用程式中的一組相關設定。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 11/28/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/configuration/options
ms.openlocfilehash: 660ee2365e2e186dd93d57ec79628e0bd7d24d52
ms.sourcegitcommit: d45d766504c2c5aad2453f01f089bc6b696b5576
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/30/2018
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="6cc14-103">ASP.NET Core 中的選項模式</span><span class="sxs-lookup"><span data-stu-id="6cc14-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="6cc14-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="6cc14-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="6cc14-105">選項模式使用類別來代表一組相關的設定。</span><span class="sxs-lookup"><span data-stu-id="6cc14-105">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="6cc14-106">當組態設定依功能隔離到不同的類別時，應用程式會遵守兩個重要的軟體工程準則：</span><span class="sxs-lookup"><span data-stu-id="6cc14-106">When configuration settings are isolated by feature into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="6cc14-107">[介面隔離準則 (ISP)](http://deviq.com/interface-segregation-principle/)：相依於組態設定的功能 (類別) 只會相依於它們使用的組態設定。</span><span class="sxs-lookup"><span data-stu-id="6cc14-107">The [Interface Segregation Principle (ISP)](http://deviq.com/interface-segregation-principle/): Features (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="6cc14-108">[關注點分離](http://deviq.com/separation-of-concerns/) (不同考量)：應用程式不同部分的設定不會彼此相依或結合。</span><span class="sxs-lookup"><span data-stu-id="6cc14-108">[Separation of Concerns](http://deviq.com/separation-of-concerns/): Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="6cc14-109">[檢視或下載範例程式碼](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([如何下載](xref:tutorials/index#how-to-download-a-sample))本文比較能輕鬆地遵循範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="6cc14-109">[View or download sample code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)) This article is easier to follow with the sample app.</span></span>

## <a name="basic-options-configuration"></a><span data-ttu-id="6cc14-110">基本選項組態</span><span class="sxs-lookup"><span data-stu-id="6cc14-110">Basic options configuration</span></span>

<span data-ttu-id="6cc14-111">基本選項範例組態是以[範例應用程式](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)中的範例 &num;1 來示範。</span><span class="sxs-lookup"><span data-stu-id="6cc14-111">Basic options configuration is demonstrated as Example &num;1 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="6cc14-112">選項類別必須為非抽象，且具有公用的無參數建構函式。</span><span class="sxs-lookup"><span data-stu-id="6cc14-112">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="6cc14-113">下列 `MyOptions` 類別有兩個屬性，`Option1` 和 `Option2`。</span><span class="sxs-lookup"><span data-stu-id="6cc14-113">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="6cc14-114">設定預設值為選擇性，但在下列範例中，類別建構函式會設定 `Option1` 的預設值。</span><span class="sxs-lookup"><span data-stu-id="6cc14-114">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="6cc14-115">`Option2` 的預設值直接藉由初始化屬性來設定 (*Models/MyOptions.cs*)：</span><span class="sxs-lookup"><span data-stu-id="6cc14-115">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/sample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="6cc14-116">`MyOptions` 類別使用 [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) 新增至服務容器，並繫結至組態：</span><span class="sxs-lookup"><span data-stu-id="6cc14-116">The `MyOptions` class is added to the service container with [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) and bound to configuration:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="6cc14-117">下列頁面模型使用[建構函式相依性插入](xref:fundamentals/dependency-injection#what-is-dependency-injection)搭配 [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) 來存取設定 (*Pages/Index.cshtml.cs*)：</span><span class="sxs-lookup"><span data-stu-id="6cc14-117">The following page model uses [constructor dependency injection](xref:fundamentals/dependency-injection#what-is-dependency-injection) with [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="6cc14-118">範例的 *appsettings.json* 檔案指定 `option1` 和 `option2` 的值：</span><span class="sxs-lookup"><span data-stu-id="6cc14-118">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/sample/appsettings.json)]

<span data-ttu-id="6cc14-119">當應用程式執行時，頁面模型的 `OnGet` 方法會傳回字串，顯示選項類別值：</span><span class="sxs-lookup"><span data-stu-id="6cc14-119">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="6cc14-120">使用委派設定簡單的選項</span><span class="sxs-lookup"><span data-stu-id="6cc14-120">Configure simple options with a delegate</span></span>

<span data-ttu-id="6cc14-121">使用委派設定簡單的選項是以[範例應用程式](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)中的範例 &num;2 來示範。</span><span class="sxs-lookup"><span data-stu-id="6cc14-121">Configuring simple options with a delegate is demonstrated as Example &num;2 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="6cc14-122">使用委派來設定選項值。</span><span class="sxs-lookup"><span data-stu-id="6cc14-122">Use a delegate to set options values.</span></span> <span data-ttu-id="6cc14-123">範例應用程式使用 `MyOptionsWithDelegateConfig` 類別 (*Models/MyOptionsWithDelegateConfig.cs*)：</span><span class="sxs-lookup"><span data-stu-id="6cc14-123">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="6cc14-124">在下列程式碼中，第二個 `IConfigureOptions<TOptions>` 服務新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="6cc14-124">In the following code, a second `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="6cc14-125">它使用委派，以 `MyOptionsWithDelegateConfig` 設定繫結：</span><span class="sxs-lookup"><span data-stu-id="6cc14-125">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="6cc14-126">*Index.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="6cc14-126">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="6cc14-127">您可以新增多個設定提供者。</span><span class="sxs-lookup"><span data-stu-id="6cc14-127">You can add multiple configuration providers.</span></span> <span data-ttu-id="6cc14-128">設定提供者提供於 NuGet 套件中。</span><span class="sxs-lookup"><span data-stu-id="6cc14-128">Configuration providers are available in NuGet packages.</span></span> <span data-ttu-id="6cc14-129">它們會以註冊的順序套用。</span><span class="sxs-lookup"><span data-stu-id="6cc14-129">They're applied in order that they're registered.</span></span>

<span data-ttu-id="6cc14-130">每次呼叫 [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) 會新增 `IConfigureOptions<TOptions>` 服務至服務容器。</span><span class="sxs-lookup"><span data-stu-id="6cc14-130">Each call to [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) adds an `IConfigureOptions<TOptions>` service to the service container.</span></span> <span data-ttu-id="6cc14-131">在上述範例中，`Option1` 和 `Option2` 的值都指定在 *appsettings.json* 中，但 `Option1` 和 `Option2` 的值會被設定的委派所覆寫。</span><span class="sxs-lookup"><span data-stu-id="6cc14-131">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="6cc14-132">啟用多個設定服務時，最後一個指定的設定來源會「勝出」並設定此組態值。</span><span class="sxs-lookup"><span data-stu-id="6cc14-132">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="6cc14-133">當應用程式執行時，頁面模型的 `OnGet` 方法會傳回字串，顯示選項類別值：</span><span class="sxs-lookup"><span data-stu-id="6cc14-133">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="6cc14-134">子選項組態</span><span class="sxs-lookup"><span data-stu-id="6cc14-134">Suboptions configuration</span></span>

<span data-ttu-id="6cc14-135">子選項組態是以[範例應用程式](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)中的範例 &num;3 來示範。</span><span class="sxs-lookup"><span data-stu-id="6cc14-135">Suboptions configuration is demonstrated as Example &num;3 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="6cc14-136">應用程式應該建立屬於應用程式中特定功能群組 (類別) 的選項類別。</span><span class="sxs-lookup"><span data-stu-id="6cc14-136">Apps should create options classes that pertain to specific feature groups (classes) in the app.</span></span> <span data-ttu-id="6cc14-137">需要組態值的應用程式組件應該只能存取它們使用的設定值。</span><span class="sxs-lookup"><span data-stu-id="6cc14-137">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="6cc14-138">將選項繫結至組態時，選項類型中的每一個屬性都會繫結至 `property[:sub-property:]` 格式的組態索引鍵。</span><span class="sxs-lookup"><span data-stu-id="6cc14-138">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="6cc14-139">例如，`MyOptions.Option1` 屬性繫結至索引鍵 `Option1`，其是從 *appsettings.json* 中的 `option1` 屬性讀取。</span><span class="sxs-lookup"><span data-stu-id="6cc14-139">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="6cc14-140">在下列程式碼中，第三個 `IConfigureOptions<TOptions>` 服務新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="6cc14-140">In the following code, a third `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="6cc14-141">它將 `MySubOptions` 繫結至 *appSettings.json* 檔案的 `subsection` 區段：</span><span class="sxs-lookup"><span data-stu-id="6cc14-141">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="6cc14-142">`GetSection` 擴充方法需要 [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="6cc14-142">The `GetSection` extension method requires the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet package.</span></span> <span data-ttu-id="6cc14-143">如果應用程式使用 [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) 中繼套件，則會自動包含套件。</span><span class="sxs-lookup"><span data-stu-id="6cc14-143">If the app uses the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, the package is automatically included.</span></span>

<span data-ttu-id="6cc14-144">範例的 *appsettings.json* 檔案會定義 `subsection` 成員，並具有 `suboption1` 和 `suboption2` 的索引鍵：</span><span class="sxs-lookup"><span data-stu-id="6cc14-144">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/sample/appsettings.json?highlight=4-7)]

<span data-ttu-id="6cc14-145">`MySubOptions` 類別會定義屬性 `SubOption1` 和 `SubOption2`，來保存子選項值 (*Models/MySubOptions.cs*)：</span><span class="sxs-lookup"><span data-stu-id="6cc14-145">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the sub-option values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/sample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="6cc14-146">頁面模型的 `OnGet` 方法會傳回字串與子選項值 (*Pages/Index.cshtml.cs*)：</span><span class="sxs-lookup"><span data-stu-id="6cc14-146">The page model's `OnGet` method returns a string with the sub-option values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="6cc14-147">當應用程式執行時，`OnGet` 方法會傳回字串，顯示子選項類別值：</span><span class="sxs-lookup"><span data-stu-id="6cc14-147">When the app is run, the `OnGet` method returns a string showing the sub-option class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="6cc14-148">檢視模型提供的選項或使用直接檢視插入提供的選項</span><span class="sxs-lookup"><span data-stu-id="6cc14-148">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="6cc14-149">檢視模型提供的選項或使用直接檢視插入提供的選項是以[範例應用程式](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)中的範例 &num;4 來示範。</span><span class="sxs-lookup"><span data-stu-id="6cc14-149">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="6cc14-150">可以在檢視模型中提供選項，或藉由將 `IOptions<TOptions>` 直接插入至檢視來提供選項 (*Pages/Index.cshtml.cs*)：</span><span class="sxs-lookup"><span data-stu-id="6cc14-150">Options can be supplied in a view model or by injecting `IOptions<TOptions>` directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="6cc14-151">對於直接插入，請使用 `@inject` 指示詞插入 `IOptions<MyOptions>`：</span><span class="sxs-lookup"><span data-stu-id="6cc14-151">For direct injection, inject `IOptions<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

<span data-ttu-id="6cc14-152">執行應用程式時，轉譯的頁面中會顯示選項值：</span><span class="sxs-lookup"><span data-stu-id="6cc14-152">When the app is run, the option values are shown in the rendered page:</span></span>

![選項值 Option1:：value1_from_json 和 Option2: -1 是從模型藉由插入至檢視來載入。](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="6cc14-154">使用 IOptionsSnapshot 重新載入設定資料</span><span class="sxs-lookup"><span data-stu-id="6cc14-154">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="6cc14-155">使用 `IOptionsSnapshot` 重新載入組態資料是以[範例應用程式](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)中的範例 &num;5 來示範。</span><span class="sxs-lookup"><span data-stu-id="6cc14-155">Reloading configuration data with `IOptionsSnapshot` is demonstrated in Example &num;5 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="6cc14-156">*需要 ASP.NET Core 1.1 或更新版本。*</span><span class="sxs-lookup"><span data-stu-id="6cc14-156">*Requires ASP.NET Core 1.1 or later.*</span></span>

<span data-ttu-id="6cc14-157">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) 支援以最小的處理負擔來重新載入選項。</span><span class="sxs-lookup"><span data-stu-id="6cc14-157">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) supports reloading options with minimal processing overhead.</span></span> <span data-ttu-id="6cc14-158">在 ASP.NET Core 1.1 中，`IOptionsSnapshot` 是 [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) 的快照集，每當監視器根據資料來源變更觸發變更時便會自動更新。</span><span class="sxs-lookup"><span data-stu-id="6cc14-158">In ASP.NET Core 1.1, `IOptionsSnapshot` is a snapshot of [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) and updates automatically whenever the monitor triggers changes based on the data source changing.</span></span> <span data-ttu-id="6cc14-159">ASP.NET Core 2.0 版及更新版本中，選項會在要求的存留期內存取及快取時，針對每個要求計算一次。</span><span class="sxs-lookup"><span data-stu-id="6cc14-159">In ASP.NET Core 2.0 and later, options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="6cc14-160">下列範例示範在 *appsettings.json* 變更之後如何建立新的 `IOptionsSnapshot` (*Pages/Index.cshtml.cs*)。</span><span class="sxs-lookup"><span data-stu-id="6cc14-160">The following example demonstrates how a new `IOptionsSnapshot` is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="6cc14-161">對伺服器的多個要求會傳回 *appsettings.json* 檔案所提供的常數值，直到檔案變更並重新載入組態為止。</span><span class="sxs-lookup"><span data-stu-id="6cc14-161">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="6cc14-162">下圖顯示從 *appsettings.json* 檔案載入的初始 `option1` 和 `option2` 值：</span><span class="sxs-lookup"><span data-stu-id="6cc14-162">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="6cc14-163">將 *appsettings.json* 檔案中的值變更為 `value1_from_json UPDATED` 和 `200`。</span><span class="sxs-lookup"><span data-stu-id="6cc14-163">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="6cc14-164">儲存 *appsettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="6cc14-164">Save the *appsettings.json* file.</span></span> <span data-ttu-id="6cc14-165">重新整理瀏覽器，以查看選項值更新：</span><span class="sxs-lookup"><span data-stu-id="6cc14-165">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="6cc14-166">IConfigureNamedOptions 的具名選項支援</span><span class="sxs-lookup"><span data-stu-id="6cc14-166">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="6cc14-167">[IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) 的具名選項支援是以[範例應用程式](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)中的範例 &num;6 來示範。</span><span class="sxs-lookup"><span data-stu-id="6cc14-167">Named options support with [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) is demonstrated as Example &num;6 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="6cc14-168">*需要 ASP.NET Core 2.0 或更新版本。*</span><span class="sxs-lookup"><span data-stu-id="6cc14-168">*Requires ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="6cc14-169">「具名選項」支援可讓應用程式區別具名選項組態。</span><span class="sxs-lookup"><span data-stu-id="6cc14-169">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="6cc14-170">在範例應用程式中，具名選項使用 [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) 方法來宣告：</span><span class="sxs-lookup"><span data-stu-id="6cc14-170">In the sample app, named options are declared with the [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) method:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="6cc14-171">範例應用程式會使用[IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) 來存取具名選項 (*Pages/Index.cshtml.cs*)：</span><span class="sxs-lookup"><span data-stu-id="6cc14-171">The sample app accesses the named options with [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="6cc14-172">執行範例應用程式後，會傳回具名選項：</span><span class="sxs-lookup"><span data-stu-id="6cc14-172">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="6cc14-173">`named_options_1` 值會從設定提供，這會從 *appsettings.json* 檔案載入。</span><span class="sxs-lookup"><span data-stu-id="6cc14-173">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="6cc14-174">`named_options_2` 值是由以下提供：</span><span class="sxs-lookup"><span data-stu-id="6cc14-174">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="6cc14-175">`ConfigureServices` 中 `Option1` 的 `named_options_2` 委派。</span><span class="sxs-lookup"><span data-stu-id="6cc14-175">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="6cc14-176">`MyOptions` 類別所提供的 `Option2` 預設值。</span><span class="sxs-lookup"><span data-stu-id="6cc14-176">The default value for `Option2` provided by the `MyOptions` class.</span></span>

<span data-ttu-id="6cc14-177">使用 [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) 方法設定所有具名選項執行個體。</span><span class="sxs-lookup"><span data-stu-id="6cc14-177">Configure all named options instances with the [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) method.</span></span> <span data-ttu-id="6cc14-178">下列程式碼會為具有共通值的所有具名組態執行個體設定 `Option1`。</span><span class="sxs-lookup"><span data-stu-id="6cc14-178">The following code configures `Option1` for all named configuration instances with a common value.</span></span> <span data-ttu-id="6cc14-179">將下列程式碼手動新增至 `Configure` 方法：</span><span class="sxs-lookup"><span data-stu-id="6cc14-179">Add the following code manually to the `Configure` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="6cc14-180">新增程式碼之後執行範例應用程式，就會產生下列結果：</span><span class="sxs-lookup"><span data-stu-id="6cc14-180">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="6cc14-181">在 ASP.NET Core 2.0 版及更新版本中，所有選項都是具名執行個體。</span><span class="sxs-lookup"><span data-stu-id="6cc14-181">In ASP.NET Core 2.0 and later, all options are named instances.</span></span> <span data-ttu-id="6cc14-182">現有的 `IConfigureOption` 執行個體會視為以 `Options.DefaultName` 執行個體為目標，也就是 `string.Empty`。</span><span class="sxs-lookup"><span data-stu-id="6cc14-182">Existing `IConfigureOption` instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="6cc14-183">`IConfigureNamedOptions` 也會實作 `IConfigureOptions`。</span><span class="sxs-lookup"><span data-stu-id="6cc14-183">`IConfigureNamedOptions` also implements `IConfigureOptions`.</span></span> <span data-ttu-id="6cc14-184">[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) 的預設實作 ([參考來源](https://github.com/aspnet/Options/blob/release/2.0/src/Microsoft.Extensions.Options/IOptionsFactory.cs)) 有邏輯可適當地使用每一個。</span><span class="sxs-lookup"><span data-stu-id="6cc14-184">The default implementation of the [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([reference source](https://github.com/aspnet/Options/blob/release/2.0/src/Microsoft.Extensions.Options/IOptionsFactory.cs) has logic to use each appropriately.</span></span> <span data-ttu-id="6cc14-185">`null` 具名選項用來以所有具名執行個體為目標，而不是特定的具名執行個體 ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) 和 [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) 使用此慣例)。</span><span class="sxs-lookup"><span data-stu-id="6cc14-185">The `null` named option is used to target all of the named instances instead of a specific named instance ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) and [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) use this convention).</span></span>

## <a name="ipostconfigureoptions"></a><span data-ttu-id="6cc14-186">IPostConfigureOptions</span><span class="sxs-lookup"><span data-stu-id="6cc14-186">IPostConfigureOptions</span></span>

<span data-ttu-id="6cc14-187">*需要 ASP.NET Core 2.0 或更新版本。*</span><span class="sxs-lookup"><span data-stu-id="6cc14-187">*Requires ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="6cc14-188">使用 [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1) 設定後置組態。</span><span class="sxs-lookup"><span data-stu-id="6cc14-188">Set postconfiguration with [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span></span> <span data-ttu-id="6cc14-189">後置組態會在所有 [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) 組態都發生之後執行：</span><span class="sxs-lookup"><span data-stu-id="6cc14-189">Postconfiguration runs after all [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="6cc14-190">[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) 可以用來後置設定具名選項：</span><span class="sxs-lookup"><span data-stu-id="6cc14-190">[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="6cc14-191">使用 [PostConfigureAll&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) 後置設定所有具名組態執行個體：</span><span class="sxs-lookup"><span data-stu-id="6cc14-191">Use [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) to post-configure all named configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="options-factory-monitoring-and-cache"></a><span data-ttu-id="6cc14-192">選項 Factory、監視和快取</span><span class="sxs-lookup"><span data-stu-id="6cc14-192">Options factory, monitoring, and cache</span></span>

<span data-ttu-id="6cc14-193">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) 用於 `TOptions` 執行個體變更時的通知。</span><span class="sxs-lookup"><span data-stu-id="6cc14-193">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) is used for notifications when `TOptions` instances change.</span></span> <span data-ttu-id="6cc14-194">`IOptionsMonitor` 支援可重新載入的選項、變更通知，以及 `IPostConfigureOptions`。</span><span class="sxs-lookup"><span data-stu-id="6cc14-194">`IOptionsMonitor` supports reloadable options, change notifications, and `IPostConfigureOptions`.</span></span>

<span data-ttu-id="6cc14-195">[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) (ASP.NET Core 2.0 或更新版本) 負責建立新的選項執行個體。</span><span class="sxs-lookup"><span data-stu-id="6cc14-195">[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) (ASP.NET Core 2.0 or later) is responsible for creating new options instances.</span></span> <span data-ttu-id="6cc14-196">它有一個 [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) 方法。</span><span class="sxs-lookup"><span data-stu-id="6cc14-196">It has a single [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) method.</span></span> <span data-ttu-id="6cc14-197">預設實作會採用所有已註冊的 `IConfigureOptions` 和 `IPostConfigureOptions`，並先執行所有設定，接著執行後置設定。</span><span class="sxs-lookup"><span data-stu-id="6cc14-197">The default implementation takes all registered `IConfigureOptions` and `IPostConfigureOptions` and runs all the configures first, followed by the post-configures.</span></span> <span data-ttu-id="6cc14-198">它會區別 `IConfigureNamedOptions` 和 `IConfigureOptions`，且只會呼叫適當的介面。</span><span class="sxs-lookup"><span data-stu-id="6cc14-198">It distinguishes between `IConfigureNamedOptions` and `IConfigureOptions` and only calls the appropriate interface.</span></span>

<span data-ttu-id="6cc14-199">[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (ASP.NET Core 2.0 或更新版本) 由 `IOptionsMonitor` 用來快取 `TOptions` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="6cc14-199">[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (ASP.NET Core 2.0 or later) is used by `IOptionsMonitor` to cache `TOptions` instances.</span></span> <span data-ttu-id="6cc14-200">`IOptionsMonitorCache` 會使監視器中的選項執行個體失效，以便重新計算值 ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove))。</span><span class="sxs-lookup"><span data-stu-id="6cc14-200">The `IOptionsMonitorCache` invalidates options instances in the monitor so that the value is recomputed ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span></span> <span data-ttu-id="6cc14-201">值可以手動導入，也能使用 [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd)。</span><span class="sxs-lookup"><span data-stu-id="6cc14-201">Values can be manually introduced as well with [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span></span> <span data-ttu-id="6cc14-202">[Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) 方法用於應該視需要重新建立所有具名執行個體時。</span><span class="sxs-lookup"><span data-stu-id="6cc14-202">The [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) method is used when all named instances should be recreated on demand.</span></span>

## <a name="accessing-options-during-startup"></a><span data-ttu-id="6cc14-203">在啟動期間存取選項</span><span class="sxs-lookup"><span data-stu-id="6cc14-203">Accessing options during startup</span></span>

<span data-ttu-id="6cc14-204">`IOptions` 可用於 `Configure`，因為服務是在 `Configure` 方法執行之前建置。</span><span class="sxs-lookup"><span data-stu-id="6cc14-204">`IOptions` can be used in `Configure`, since services are built before the `Configure` method executes.</span></span> <span data-ttu-id="6cc14-205">如果在 `ConfigureServices` 建置服務提供者以存取選項，它將不會包含建置服務提供者之後提供的任何選項組態。</span><span class="sxs-lookup"><span data-stu-id="6cc14-205">If a service provider is built in `ConfigureServices` to access options, it wouldn't contain any options configurations provided after the service provider is built.</span></span> <span data-ttu-id="6cc14-206">因此，可能會因為服務註冊的順序而有不一致的選項狀態存在。</span><span class="sxs-lookup"><span data-stu-id="6cc14-206">Therefore, an inconsistent options state may exist due to the ordering of service registrations.</span></span>

<span data-ttu-id="6cc14-207">因為選項通常會從組態載入，所以組態可以在啟動時用於 `Configure` 和 `ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="6cc14-207">Since options are typically loaded from configuration, configuration can be used in startup in both `Configure` and `ConfigureServices`.</span></span> <span data-ttu-id="6cc14-208">如需在啟動期間使用組態的範例，請參閱[應用程式啟動](xref:fundamentals/startup)主題。</span><span class="sxs-lookup"><span data-stu-id="6cc14-208">For examples of using configuration during startup, see the [Application startup](xref:fundamentals/startup) topic.</span></span>

## <a name="see-also"></a><span data-ttu-id="6cc14-209">另請參閱</span><span class="sxs-lookup"><span data-stu-id="6cc14-209">See also</span></span>

* [<span data-ttu-id="6cc14-210">組態</span><span class="sxs-lookup"><span data-stu-id="6cc14-210">Configuration</span></span>](xref:fundamentals/configuration/index)
