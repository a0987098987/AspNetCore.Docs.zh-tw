---
title: ASP.NET Core 中的選項模式
author: guardrex
description: 了解如何使用選項模式來代表 ASP.NET Core 應用程式中的一組相關設定。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
uid: fundamentals/configuration/options
ms.openlocfilehash: 1f3625380d816c7d4df5a7a24b0ac146500330de
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/19/2020
ms.locfileid: "77447201"
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="0bd65-103">ASP.NET Core 中的選項模式</span><span class="sxs-lookup"><span data-stu-id="0bd65-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="0bd65-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0bd65-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="0bd65-105">選項模式使用類別來代表一組相關的設定。</span><span class="sxs-lookup"><span data-stu-id="0bd65-105">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="0bd65-106">當[組態設定](xref:fundamentals/configuration/index)依案例隔離到不同的類別時，應用程式會遵守兩個重要的軟體工程準則：</span><span class="sxs-lookup"><span data-stu-id="0bd65-106">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="0bd65-107">依存于 configuration 設定的[介面隔離原則（ISP）或封裝](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation)&ndash; 案例（類別），只取決於它們所使用的設定。</span><span class="sxs-lookup"><span data-stu-id="0bd65-107">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="0bd65-108">在應用程式的不同部分 &ndash; 設定不會彼此相依或彼此結合[的問題分離](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns)。</span><span class="sxs-lookup"><span data-stu-id="0bd65-108">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="0bd65-109">選項也提供驗證設定資料的機制。</span><span class="sxs-lookup"><span data-stu-id="0bd65-109">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="0bd65-110">如需詳細資訊，請參閱[選項驗證](#options-validation)一節。</span><span class="sxs-lookup"><span data-stu-id="0bd65-110">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="0bd65-111">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0bd65-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="package"></a><span data-ttu-id="0bd65-112">套件</span><span class="sxs-lookup"><span data-stu-id="0bd65-112">Package</span></span>

<span data-ttu-id="0bd65-113">ASP.NET Core 應用程式中會隱含地參考[microsoft.extensions.options.configurationextensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/)套件。</span><span class="sxs-lookup"><span data-stu-id="0bd65-113">The [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package is implicitly referenced in ASP.NET Core apps.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="0bd65-114">選項介面</span><span class="sxs-lookup"><span data-stu-id="0bd65-114">Options interfaces</span></span>

<span data-ttu-id="0bd65-115"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 是用來擷取選項並管理 `TOptions` 執行個體的選項通知。</span><span class="sxs-lookup"><span data-stu-id="0bd65-115"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="0bd65-116"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 支援以下案例：</span><span class="sxs-lookup"><span data-stu-id="0bd65-116"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="0bd65-117">變更通知</span><span class="sxs-lookup"><span data-stu-id="0bd65-117">Change notifications</span></span>
* [<span data-ttu-id="0bd65-118">具名選項</span><span class="sxs-lookup"><span data-stu-id="0bd65-118">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="0bd65-119">可重新載入的設定</span><span class="sxs-lookup"><span data-stu-id="0bd65-119">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="0bd65-120">選擇性選項無效判定 (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="0bd65-120">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="0bd65-121">[設定後](#options-post-configuration)案例可讓您在所有 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 設定發生時設定或變更選項。</span><span class="sxs-lookup"><span data-stu-id="0bd65-121">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="0bd65-122"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> 負責建立新的選項執行個體。</span><span class="sxs-lookup"><span data-stu-id="0bd65-122"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="0bd65-123">它有單一 <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> 方法。</span><span class="sxs-lookup"><span data-stu-id="0bd65-123">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="0bd65-124">預設實作會接受所有已註冊的 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 與 <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>，並先執行所有設定，接著執行設定後作業。</span><span class="sxs-lookup"><span data-stu-id="0bd65-124">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="0bd65-125">它會區別 <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> 和 <xref:Microsoft.Extensions.Options.IConfigureOptions%601>，且只會呼叫適當的介面。</span><span class="sxs-lookup"><span data-stu-id="0bd65-125">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="0bd65-126"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> 會由 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 用來快取 `TOptions` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="0bd65-126"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="0bd65-127"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> 會使監視器中的選項執行個體失效，以便重新計算值 (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>)。</span><span class="sxs-lookup"><span data-stu-id="0bd65-127">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="0bd65-128">值可以使用 <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*> 手動導入。</span><span class="sxs-lookup"><span data-stu-id="0bd65-128">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="0bd65-129"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> 方法用於應該視需要重新建立所有具名執行個體時。</span><span class="sxs-lookup"><span data-stu-id="0bd65-129">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="0bd65-130"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> 在應該於收到每個要求時重新計算選項的案例中很實用用。</span><span class="sxs-lookup"><span data-stu-id="0bd65-130"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="0bd65-131">如需詳細資訊，請參閱[使用 IOptionsSnapshot 重新載入設定資料](#reload-configuration-data-with-ioptionssnapshot)一節。</span><span class="sxs-lookup"><span data-stu-id="0bd65-131">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="0bd65-132"><xref:Microsoft.Extensions.Options.IOptions%601> 可用於支援選項。</span><span class="sxs-lookup"><span data-stu-id="0bd65-132"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="0bd65-133">不過，<xref:Microsoft.Extensions.Options.IOptions%601> 不支援前面的 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 案例。</span><span class="sxs-lookup"><span data-stu-id="0bd65-133">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="0bd65-134">您可以在現有架構與程式庫中繼續使用 <xref:Microsoft.Extensions.Options.IOptions%601>，此程式庫已使用 <xref:Microsoft.Extensions.Options.IOptions%601> 介面且不需要 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 提供的案例。</span><span class="sxs-lookup"><span data-stu-id="0bd65-134">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="0bd65-135">一般選項設定</span><span class="sxs-lookup"><span data-stu-id="0bd65-135">General options configuration</span></span>

<span data-ttu-id="0bd65-136">一般選項設定會示範為範例應用程式中的範例1。</span><span class="sxs-lookup"><span data-stu-id="0bd65-136">General options configuration is demonstrated as Example 1 in the sample app.</span></span>

<span data-ttu-id="0bd65-137">選項類別必須為非抽象，且具有公用的無參數建構函式。</span><span class="sxs-lookup"><span data-stu-id="0bd65-137">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="0bd65-138">下列 `MyOptions` 類別有兩個屬性，`Option1` 和 `Option2`。</span><span class="sxs-lookup"><span data-stu-id="0bd65-138">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="0bd65-139">設定預設值為選擇性，但在下列範例中，類別建構函式會設定 `Option1` 的預設值。</span><span class="sxs-lookup"><span data-stu-id="0bd65-139">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="0bd65-140">`Option2` 的預設值直接藉由初始化屬性來設定 (*Models/MyOptions.cs*)：</span><span class="sxs-lookup"><span data-stu-id="0bd65-140">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="0bd65-141">`MyOptions` 類別使用 <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> 新增到服務容器，並繫結到設定：</span><span class="sxs-lookup"><span data-stu-id="0bd65-141">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="0bd65-142">下列頁面模型使用[建構函式相依性插入](xref:mvc/controllers/dependency-injection)搭配 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 來存取設定 (*Pages/Index.cshtml.cs*)：</span><span class="sxs-lookup"><span data-stu-id="0bd65-142">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="0bd65-143">範例的 *appsettings.json* 檔案指定 `option1` 和 `option2` 的值：</span><span class="sxs-lookup"><span data-stu-id="0bd65-143">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/3.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="0bd65-144">當應用程式執行時，頁面模型的 `OnGet` 方法會傳回字串，顯示選項類別值：</span><span class="sxs-lookup"><span data-stu-id="0bd65-144">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="0bd65-145">使用自訂 <xref:System.Configuration.ConfigurationBuilder> 從設定檔載入選項設定時，請確認已正確設定基底路徑：</span><span class="sxs-lookup"><span data-stu-id="0bd65-145">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="0bd65-146">透過 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 從設定檔載入選項設定時，不需要明確設定基底路徑。</span><span class="sxs-lookup"><span data-stu-id="0bd65-146">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="0bd65-147">使用委派設定簡單的選項</span><span class="sxs-lookup"><span data-stu-id="0bd65-147">Configure simple options with a delegate</span></span>

<span data-ttu-id="0bd65-148">使用委派來設定簡單的選項，會示範為範例應用程式中的範例2。</span><span class="sxs-lookup"><span data-stu-id="0bd65-148">Configuring simple options with a delegate is demonstrated as Example 2 in the sample app.</span></span>

<span data-ttu-id="0bd65-149">使用委派來設定選項值。</span><span class="sxs-lookup"><span data-stu-id="0bd65-149">Use a delegate to set options values.</span></span> <span data-ttu-id="0bd65-150">範例應用程式使用 `MyOptionsWithDelegateConfig` 類別 (*Models/MyOptionsWithDelegateConfig.cs*)：</span><span class="sxs-lookup"><span data-stu-id="0bd65-150">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="0bd65-151">在下列程式碼中，第二個 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 服務新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="0bd65-151">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="0bd65-152">它使用委派，以 `MyOptionsWithDelegateConfig` 設定繫結：</span><span class="sxs-lookup"><span data-stu-id="0bd65-152">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="0bd65-153">*Index.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="0bd65-153">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="0bd65-154">您可以新增多個設定提供者。</span><span class="sxs-lookup"><span data-stu-id="0bd65-154">You can add multiple configuration providers.</span></span> <span data-ttu-id="0bd65-155">設定提供者可在 NuGet 套件中找到，而且會依註冊順序套用。</span><span class="sxs-lookup"><span data-stu-id="0bd65-155">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="0bd65-156">如需詳細資訊，請參閱 <xref:fundamentals/configuration/index>。</span><span class="sxs-lookup"><span data-stu-id="0bd65-156">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="0bd65-157">每次呼叫 <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> 時都會新增 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 服務到服務容器。</span><span class="sxs-lookup"><span data-stu-id="0bd65-157">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="0bd65-158">在上述範例中，`Option1` 和 `Option2` 的值都指定在 *appsettings.json* 中，但 `Option1` 和 `Option2` 的值會被設定的委派所覆寫。</span><span class="sxs-lookup"><span data-stu-id="0bd65-158">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="0bd65-159">啟用多個設定服務時，最後一個指定的設定來源會「勝出」並設定此組態值。</span><span class="sxs-lookup"><span data-stu-id="0bd65-159">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="0bd65-160">當應用程式執行時，頁面模型的 `OnGet` 方法會傳回字串，顯示選項類別值：</span><span class="sxs-lookup"><span data-stu-id="0bd65-160">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="0bd65-161">子選項組態</span><span class="sxs-lookup"><span data-stu-id="0bd65-161">Suboptions configuration</span></span>

<span data-ttu-id="0bd65-162">子選項設定會示範為範例應用程式中的範例3。</span><span class="sxs-lookup"><span data-stu-id="0bd65-162">Suboptions configuration is demonstrated as Example 3 in the sample app.</span></span>

<span data-ttu-id="0bd65-163">應用程式應該建立屬於應用程式中特定案例群組 (類別) 的選項類別。</span><span class="sxs-lookup"><span data-stu-id="0bd65-163">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="0bd65-164">需要組態值的應用程式組件應該只能存取它們使用的設定值。</span><span class="sxs-lookup"><span data-stu-id="0bd65-164">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="0bd65-165">將選項繫結至組態時，選項類型中的每一個屬性都會繫結至 `property[:sub-property:]` 格式的組態索引鍵。</span><span class="sxs-lookup"><span data-stu-id="0bd65-165">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="0bd65-166">例如，`MyOptions.Option1` 屬性繫結至索引鍵 `Option1`，其是從 `option1`appsettings.json*中的* 屬性讀取。</span><span class="sxs-lookup"><span data-stu-id="0bd65-166">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="0bd65-167">在下列程式碼中，第三個 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 服務新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="0bd65-167">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="0bd65-168">它將 `MySubOptions` 繫結至 `subsection`appSettings.json*檔案的* 區段：</span><span class="sxs-lookup"><span data-stu-id="0bd65-168">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="0bd65-169">`GetSection` 方法需要 <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> 命名空間。</span><span class="sxs-lookup"><span data-stu-id="0bd65-169">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="0bd65-170">範例的 *appsettings.json* 檔案會定義 `subsection` 成員，並具有 `suboption1` 和 `suboption2` 的索引鍵：</span><span class="sxs-lookup"><span data-stu-id="0bd65-170">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/3.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="0bd65-171">`MySubOptions` 類別會定義屬性 `SubOption1` 和 `SubOption2`，來保存選項值 (*Models/MySubOptions.cs*)：</span><span class="sxs-lookup"><span data-stu-id="0bd65-171">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="0bd65-172">頁面模型的 `OnGet` 方法會傳回具有選項值 (*Pages/Index.cshtml.cs*) 的字串：</span><span class="sxs-lookup"><span data-stu-id="0bd65-172">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="0bd65-173">當應用程式執行時，`OnGet` 方法會傳回字串，顯示子選項類別值：</span><span class="sxs-lookup"><span data-stu-id="0bd65-173">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-injection"></a><span data-ttu-id="0bd65-174">選項插入</span><span class="sxs-lookup"><span data-stu-id="0bd65-174">Options injection</span></span>

<span data-ttu-id="0bd65-175">範例應用程式中的範例4示範了選項插入。</span><span class="sxs-lookup"><span data-stu-id="0bd65-175">Options injection is demonstrated as Example 4 in the sample app.</span></span>

<span data-ttu-id="0bd65-176">將 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 插入：</span><span class="sxs-lookup"><span data-stu-id="0bd65-176">Inject <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into:</span></span>

* <span data-ttu-id="0bd65-177">具有[`@inject`](xref:mvc/views/razor#inject) razor 指示詞的 Razor 頁面或 MVC 視圖。</span><span class="sxs-lookup"><span data-stu-id="0bd65-177">A Razor page or MVC view with the [`@inject`](xref:mvc/views/razor#inject) Razor directive.</span></span>
* <span data-ttu-id="0bd65-178">頁面或視圖模型。</span><span class="sxs-lookup"><span data-stu-id="0bd65-178">A page or view model.</span></span>

<span data-ttu-id="0bd65-179">下列來自範例應用程式的範例會將 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 插入頁面模型中（*Pages/Index. cshtml .cs*）：</span><span class="sxs-lookup"><span data-stu-id="0bd65-179">The following example from the sample app injects <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into a page model (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="0bd65-180">範例應用程式會顯示如何使用 `IOptionsMonitor<MyOptions>` 指示詞來插入 `@inject`：</span><span class="sxs-lookup"><span data-stu-id="0bd65-180">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/3.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="0bd65-181">執行應用程式時，轉譯的頁面中會顯示選項值：</span><span class="sxs-lookup"><span data-stu-id="0bd65-181">When the app is run, the options values are shown in the rendered page:</span></span>

![選項值 Option1:：value1_from_json 和 Option2: -1 是從模型藉由插入至檢視來載入。](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="0bd65-183">使用 IOptionsSnapshot 重新載入設定資料</span><span class="sxs-lookup"><span data-stu-id="0bd65-183">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="0bd65-184">範例應用程式中的範例5示範使用 <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> 重載設定資料。</span><span class="sxs-lookup"><span data-stu-id="0bd65-184">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example 5 in the sample app.</span></span>

<span data-ttu-id="0bd65-185">使用 <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>，在要求的存留期記憶體取和快取時，會針對每個要求計算一次的選項。</span><span class="sxs-lookup"><span data-stu-id="0bd65-185">Using <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="0bd65-186">`IOptionsMonitor` 和 `IOptionsSnapshot` 之間的差異在於：</span><span class="sxs-lookup"><span data-stu-id="0bd65-186">The difference between `IOptionsMonitor` and `IOptionsSnapshot` is that:</span></span>

* <span data-ttu-id="0bd65-187">`IOptionsMonitor` 是一項[單一服務](xref:fundamentals/dependency-injection#singleton)，可隨時抓取目前的選項值，這在單一相依性中特別有用。</span><span class="sxs-lookup"><span data-stu-id="0bd65-187">`IOptionsMonitor` is a [singleton service](xref:fundamentals/dependency-injection#singleton) that retrieves current option values at any time, which is especially useful in singleton dependencies.</span></span>
* <span data-ttu-id="0bd65-188">`IOptionsSnapshot` 是已設定[範圍的服務](xref:fundamentals/dependency-injection#scoped)，並會在建立 `IOptionsSnapshot<T>` 物件時提供選項的快照集。</span><span class="sxs-lookup"><span data-stu-id="0bd65-188">`IOptionsSnapshot` is a [scoped service](xref:fundamentals/dependency-injection#scoped) and provides a snapshot of the options at the time the `IOptionsSnapshot<T>` object is constructed.</span></span> <span data-ttu-id="0bd65-189">選項快照集的設計目的是要搭配暫時性和範圍相依性使用。</span><span class="sxs-lookup"><span data-stu-id="0bd65-189">Options snapshots are designed for use with transient and scoped dependencies.</span></span>

<span data-ttu-id="0bd65-190">下列範例示範在 <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>appsettings.json*變更之後如何建立新的* (*Pages/Index.cshtml.cs*)。</span><span class="sxs-lookup"><span data-stu-id="0bd65-190">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="0bd65-191">對伺服器的多個要求會傳回 *appsettings.json* 檔案所提供的常數值，直到檔案變更並重新載入組態為止。</span><span class="sxs-lookup"><span data-stu-id="0bd65-191">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="0bd65-192">下圖顯示從 `option1`appsettings.json`option2` 檔案載入的初始 *和* 值：</span><span class="sxs-lookup"><span data-stu-id="0bd65-192">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="0bd65-193">將 *appsettings.json* 檔案中的值變更為 `value1_from_json UPDATED` 和 `200`。</span><span class="sxs-lookup"><span data-stu-id="0bd65-193">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="0bd65-194">儲存 *appsettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="0bd65-194">Save the *appsettings.json* file.</span></span> <span data-ttu-id="0bd65-195">重新整理瀏覽器，以查看選項值更新：</span><span class="sxs-lookup"><span data-stu-id="0bd65-195">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="0bd65-196">IConfigureNamedOptions 的具名選項支援</span><span class="sxs-lookup"><span data-stu-id="0bd65-196">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="0bd65-197">範例應用程式中的範例6示範了 <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> 的命名選項支援。</span><span class="sxs-lookup"><span data-stu-id="0bd65-197">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example 6 in the sample app.</span></span>

<span data-ttu-id="0bd65-198">命名選項支援可讓應用程式區別已命名的選項設定。</span><span class="sxs-lookup"><span data-stu-id="0bd65-198">Named options support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="0bd65-199">在範例應用程式中，名稱為 options 的宣告為[optionsservicecollectionextensions.configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*)，它會呼叫[configurenamedoptions .configure\<TOptions >。設定](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*)擴充方法。</span><span class="sxs-lookup"><span data-stu-id="0bd65-199">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method.</span></span> <span data-ttu-id="0bd65-200">已命名的選項會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="0bd65-200">Named options are case sensitive.</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="0bd65-201">範例應用程式會使用　<xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*) 來存取具名選項：</span><span class="sxs-lookup"><span data-stu-id="0bd65-201">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="0bd65-202">執行範例應用程式後，會傳回具名選項：</span><span class="sxs-lookup"><span data-stu-id="0bd65-202">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="0bd65-203">`named_options_1` 值會從設定提供，這會從 *appsettings.json* 檔案載入。</span><span class="sxs-lookup"><span data-stu-id="0bd65-203">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="0bd65-204">`named_options_2` 值是由以下提供：</span><span class="sxs-lookup"><span data-stu-id="0bd65-204">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="0bd65-205">`named_options_2` 中 `ConfigureServices` 的 `Option1` 委派。</span><span class="sxs-lookup"><span data-stu-id="0bd65-205">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="0bd65-206">`Option2` 類別所提供的 `MyOptions` 預設值。</span><span class="sxs-lookup"><span data-stu-id="0bd65-206">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="0bd65-207">使用 ConfigureAll 方法設定所有選項</span><span class="sxs-lookup"><span data-stu-id="0bd65-207">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="0bd65-208">使用 <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> 方法設定所有選項執行個體。</span><span class="sxs-lookup"><span data-stu-id="0bd65-208">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="0bd65-209">下列程式碼會為具有共通值的所有設定執行個體設定 `Option1`。</span><span class="sxs-lookup"><span data-stu-id="0bd65-209">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="0bd65-210">將下列程式碼手動新增至 `Startup.ConfigureServices` 方法：</span><span class="sxs-lookup"><span data-stu-id="0bd65-210">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="0bd65-211">新增程式碼之後執行範例應用程式，就會產生下列結果：</span><span class="sxs-lookup"><span data-stu-id="0bd65-211">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="0bd65-212">所有選項都是具名執行個體。</span><span class="sxs-lookup"><span data-stu-id="0bd65-212">All options are named instances.</span></span> <span data-ttu-id="0bd65-213">現有的 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 執行個體會視為以 `Options.DefaultName` 執行個體為目標，也就是 `string.Empty`。</span><span class="sxs-lookup"><span data-stu-id="0bd65-213">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="0bd65-214"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> 也會實作 <xref:Microsoft.Extensions.Options.IConfigureOptions%601>。</span><span class="sxs-lookup"><span data-stu-id="0bd65-214"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="0bd65-215"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> 的預設實作有邏輯可以適當地使用每個項目。</span><span class="sxs-lookup"><span data-stu-id="0bd65-215">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="0bd65-216">`null` 具名選項用來以所有具名執行個體為目標，而不是特定的具名執行個體 (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> 與 <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> 使用此慣例)。</span><span class="sxs-lookup"><span data-stu-id="0bd65-216">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="0bd65-217">OptionsBuilder API</span><span class="sxs-lookup"><span data-stu-id="0bd65-217">OptionsBuilder API</span></span>

<span data-ttu-id="0bd65-218"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> 會用於設定 `TOptions` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="0bd65-218"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="0bd65-219">因為 `OptionsBuilder` 僅為初始 `AddOptions<TOptions>(string optionsName)` 呼叫的單一參數，而不是出現在所有後續呼叫的參數，所以其可簡化建立具名選項的程序。</span><span class="sxs-lookup"><span data-stu-id="0bd65-219">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="0bd65-220">選項驗證及接受服務依存性的 `ConfigureOptions` 多載，只可透過 `OptionsBuilder` 使用。</span><span class="sxs-lookup"><span data-stu-id="0bd65-220">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="0bd65-221">使用 DI 服務來設定選項</span><span class="sxs-lookup"><span data-stu-id="0bd65-221">Use DI services to configure options</span></span>

<span data-ttu-id="0bd65-222">在以下列兩種方式設定選項的同時，您可以從相依性插入中存取其他服務：</span><span class="sxs-lookup"><span data-stu-id="0bd65-222">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="0bd65-223">將設定委派傳遞到 [OptionsBuilder](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*)TOptions>[ 上的 \<Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1)。</span><span class="sxs-lookup"><span data-stu-id="0bd65-223">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="0bd65-224">`OptionsBuilder<TOptions>` 提供[了設定的多載，讓](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*)您可以使用最多五個服務來設定選項：</span><span class="sxs-lookup"><span data-stu-id="0bd65-224">`OptionsBuilder<TOptions>` provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="0bd65-225">建立您自己的類型來實作 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 或 <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>，並將該類型註冊為服務。</span><span class="sxs-lookup"><span data-stu-id="0bd65-225">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="0bd65-226">我們建議您將設定委派傳遞到 [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*)，因為建立服務更複雜。</span><span class="sxs-lookup"><span data-stu-id="0bd65-226">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="0bd65-227">建立您自己的類型相當於，當您使用 [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) 時此架構可為您執行的動作。</span><span class="sxs-lookup"><span data-stu-id="0bd65-227">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="0bd65-228">呼叫 [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) 會註冊暫時性泛型 <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>，其具有會接受所指定泛型服務類型的建構函式。</span><span class="sxs-lookup"><span data-stu-id="0bd65-228">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-validation"></a><span data-ttu-id="0bd65-229">選項驗證</span><span class="sxs-lookup"><span data-stu-id="0bd65-229">Options validation</span></span>

<span data-ttu-id="0bd65-230">選項驗證可讓您在設定選項之後驗證選項。</span><span class="sxs-lookup"><span data-stu-id="0bd65-230">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="0bd65-231">搭配驗證方法呼叫 `Validate`，若選項有效會傳回 `true`，若為無效則傳回 `false`：</span><span class="sxs-lookup"><span data-stu-id="0bd65-231">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

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

<span data-ttu-id="0bd65-232">上述範例會將具名的選項執行個體設定為 `optionalOptionsName`。</span><span class="sxs-lookup"><span data-stu-id="0bd65-232">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="0bd65-233">預設選項執行個體為 `Options.DefaultName`。</span><span class="sxs-lookup"><span data-stu-id="0bd65-233">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="0bd65-234">當選項執行個體建立之後，便會執行驗證。</span><span class="sxs-lookup"><span data-stu-id="0bd65-234">Validation runs when the options instance is created.</span></span> <span data-ttu-id="0bd65-235">選項實例保證會在第一次存取時通過驗證。</span><span class="sxs-lookup"><span data-stu-id="0bd65-235">An options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0bd65-236">選項驗證不會在建立 options 實例之後防護選項的修改。</span><span class="sxs-lookup"><span data-stu-id="0bd65-236">Options validation doesn't guard against options modifications after the options instance is created.</span></span> <span data-ttu-id="0bd65-237">例如，第一次存取選項時，會針對每個要求建立和驗證 `IOptionsSnapshot` 選項。</span><span class="sxs-lookup"><span data-stu-id="0bd65-237">For example, `IOptionsSnapshot` options are created and validated once per request when the options are first accessed.</span></span> <span data-ttu-id="0bd65-238">*針對相同要求*的後續存取嘗試時，不會再次驗證 `IOptionsSnapshot` 選項。</span><span class="sxs-lookup"><span data-stu-id="0bd65-238">The `IOptionsSnapshot` options aren't validated again on subsequent access attempts *for the same request*.</span></span>

<span data-ttu-id="0bd65-239">`Validate` 方法可接受 `Func<TOptions, bool>`。</span><span class="sxs-lookup"><span data-stu-id="0bd65-239">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="0bd65-240">若要完整地自訂驗證，請實作 `IValidateOptions<TOptions>`，它允許：</span><span class="sxs-lookup"><span data-stu-id="0bd65-240">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="0bd65-241">多種選項類型的驗證：`class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span><span class="sxs-lookup"><span data-stu-id="0bd65-241">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="0bd65-242">取決於另一個選項類型的驗證：`public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span><span class="sxs-lookup"><span data-stu-id="0bd65-242">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span></span>

<span data-ttu-id="0bd65-243">`IValidateOptions` 可驗證：</span><span class="sxs-lookup"><span data-stu-id="0bd65-243">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="0bd65-244">特定的具名選項執行個體。</span><span class="sxs-lookup"><span data-stu-id="0bd65-244">A specific named options instance.</span></span>
* <span data-ttu-id="0bd65-245">所有選項 (當 `name` 為 `null` 時)。</span><span class="sxs-lookup"><span data-stu-id="0bd65-245">All options when `name` is `null`.</span></span>

<span data-ttu-id="0bd65-246">從以下的介面實作傳回 `ValidateOptionsResult`：</span><span class="sxs-lookup"><span data-stu-id="0bd65-246">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="0bd65-247">透過呼叫 [ 上的 ](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) 方法，就可從 <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*>Microsoft.Extensions.Options.DataAnnotations`OptionsBuilder<TOptions>` 套件使用資料註解型驗證。</span><span class="sxs-lookup"><span data-stu-id="0bd65-247">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> method on `OptionsBuilder<TOptions>`.</span></span> <span data-ttu-id="0bd65-248">ASP.NET Core 應用程式中會隱含地參考 `Microsoft.Extensions.Options.DataAnnotations`。</span><span class="sxs-lookup"><span data-stu-id="0bd65-248">`Microsoft.Extensions.Options.DataAnnotations` is implicitly referenced in ASP.NET Core apps.</span></span>

```csharp
using System.ComponentModel.DataAnnotations;
using Microsoft.Extensions.DependencyInjection;

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
        sp.GetRequiredService<IOptionsMonitor<AnnotatedOptions>>().CurrentValue);
    ValidateFailure<AnnotatedOptions>(error, Options.DefaultName, 1,
        "DataAnnotation validation failed for members Required " +
            "with the error 'The Required field is required.'.",
        "DataAnnotation validation failed for members StringLength " +
            "with the error 'Too long.'.",
        "DataAnnotation validation failed for members IntRange " +
            "with the error 'Out of range.'.");
}
```

<span data-ttu-id="0bd65-249">我們考慮在之後的版本中加入積極式驗證 (啟動時快速檢錯)。</span><span class="sxs-lookup"><span data-stu-id="0bd65-249">Eager validation (fail fast at startup) is under consideration for a future release.</span></span>

## <a name="options-post-configuration"></a><span data-ttu-id="0bd65-250">選項設定後作業</span><span class="sxs-lookup"><span data-stu-id="0bd65-250">Options post-configuration</span></span>

<span data-ttu-id="0bd65-251">使用 <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> 來設定設定後作業。</span><span class="sxs-lookup"><span data-stu-id="0bd65-251">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="0bd65-252">設定後作業會在所有 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 設定發生後執行：</span><span class="sxs-lookup"><span data-stu-id="0bd65-252">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="0bd65-253"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> 可用來後置設定具名選項：</span><span class="sxs-lookup"><span data-stu-id="0bd65-253"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="0bd65-254">使用 <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> 後置設定所有設定執行個體：</span><span class="sxs-lookup"><span data-stu-id="0bd65-254">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="0bd65-255">在啟動期間存取選項</span><span class="sxs-lookup"><span data-stu-id="0bd65-255">Accessing options during startup</span></span>

<span data-ttu-id="0bd65-256"><xref:Microsoft.Extensions.Options.IOptions%601> 與 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 可用於 `Startup.Configure`，因為服務是在 `Configure` 方法執行之前建置。</span><span class="sxs-lookup"><span data-stu-id="0bd65-256"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, 
    IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="0bd65-257">請勿在 <xref:Microsoft.Extensions.Options.IOptions%601> 中使用 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 或 `Startup.ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="0bd65-257">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="0bd65-258">可能會因為服務註冊的順序而有不一致的選項狀態存在。</span><span class="sxs-lookup"><span data-stu-id="0bd65-258">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="0bd65-259">選項模式使用類別來代表一組相關的設定。</span><span class="sxs-lookup"><span data-stu-id="0bd65-259">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="0bd65-260">當[組態設定](xref:fundamentals/configuration/index)依案例隔離到不同的類別時，應用程式會遵守兩個重要的軟體工程準則：</span><span class="sxs-lookup"><span data-stu-id="0bd65-260">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="0bd65-261">依存于 configuration 設定的[介面隔離原則（ISP）或封裝](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation)&ndash; 案例（類別），只取決於它們所使用的設定。</span><span class="sxs-lookup"><span data-stu-id="0bd65-261">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="0bd65-262">在應用程式的不同部分 &ndash; 設定不會彼此相依或彼此結合[的問題分離](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns)。</span><span class="sxs-lookup"><span data-stu-id="0bd65-262">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="0bd65-263">選項也提供驗證設定資料的機制。</span><span class="sxs-lookup"><span data-stu-id="0bd65-263">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="0bd65-264">如需詳細資訊，請參閱[選項驗證](#options-validation)一節。</span><span class="sxs-lookup"><span data-stu-id="0bd65-264">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="0bd65-265">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0bd65-265">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0bd65-266">必要條件</span><span class="sxs-lookup"><span data-stu-id="0bd65-266">Prerequisites</span></span>

<span data-ttu-id="0bd65-267">參考 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)，或新增 [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) 套件的套件參考。</span><span class="sxs-lookup"><span data-stu-id="0bd65-267">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="0bd65-268">選項介面</span><span class="sxs-lookup"><span data-stu-id="0bd65-268">Options interfaces</span></span>

<span data-ttu-id="0bd65-269"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 是用來擷取選項並管理 `TOptions` 執行個體的選項通知。</span><span class="sxs-lookup"><span data-stu-id="0bd65-269"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="0bd65-270"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 支援以下案例：</span><span class="sxs-lookup"><span data-stu-id="0bd65-270"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="0bd65-271">變更通知</span><span class="sxs-lookup"><span data-stu-id="0bd65-271">Change notifications</span></span>
* [<span data-ttu-id="0bd65-272">具名選項</span><span class="sxs-lookup"><span data-stu-id="0bd65-272">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="0bd65-273">可重新載入的設定</span><span class="sxs-lookup"><span data-stu-id="0bd65-273">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="0bd65-274">選擇性選項無效判定 (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="0bd65-274">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="0bd65-275">[設定後](#options-post-configuration)案例可讓您在所有 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 設定發生時設定或變更選項。</span><span class="sxs-lookup"><span data-stu-id="0bd65-275">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="0bd65-276"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> 負責建立新的選項執行個體。</span><span class="sxs-lookup"><span data-stu-id="0bd65-276"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="0bd65-277">它有單一 <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> 方法。</span><span class="sxs-lookup"><span data-stu-id="0bd65-277">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="0bd65-278">預設實作會接受所有已註冊的 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 與 <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>，並先執行所有設定，接著執行設定後作業。</span><span class="sxs-lookup"><span data-stu-id="0bd65-278">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="0bd65-279">它會區別 <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> 和 <xref:Microsoft.Extensions.Options.IConfigureOptions%601>，且只會呼叫適當的介面。</span><span class="sxs-lookup"><span data-stu-id="0bd65-279">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="0bd65-280"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> 會由 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 用來快取 `TOptions` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="0bd65-280"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="0bd65-281"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> 會使監視器中的選項執行個體失效，以便重新計算值 (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>)。</span><span class="sxs-lookup"><span data-stu-id="0bd65-281">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="0bd65-282">值可以使用 <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*> 手動導入。</span><span class="sxs-lookup"><span data-stu-id="0bd65-282">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="0bd65-283"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> 方法用於應該視需要重新建立所有具名執行個體時。</span><span class="sxs-lookup"><span data-stu-id="0bd65-283">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="0bd65-284"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> 在應該於收到每個要求時重新計算選項的案例中很實用用。</span><span class="sxs-lookup"><span data-stu-id="0bd65-284"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="0bd65-285">如需詳細資訊，請參閱[使用 IOptionsSnapshot 重新載入設定資料](#reload-configuration-data-with-ioptionssnapshot)一節。</span><span class="sxs-lookup"><span data-stu-id="0bd65-285">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="0bd65-286"><xref:Microsoft.Extensions.Options.IOptions%601> 可用於支援選項。</span><span class="sxs-lookup"><span data-stu-id="0bd65-286"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="0bd65-287">不過，<xref:Microsoft.Extensions.Options.IOptions%601> 不支援前面的 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 案例。</span><span class="sxs-lookup"><span data-stu-id="0bd65-287">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="0bd65-288">您可以在現有架構與程式庫中繼續使用 <xref:Microsoft.Extensions.Options.IOptions%601>，此程式庫已使用 <xref:Microsoft.Extensions.Options.IOptions%601> 介面且不需要 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 提供的案例。</span><span class="sxs-lookup"><span data-stu-id="0bd65-288">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="0bd65-289">一般選項設定</span><span class="sxs-lookup"><span data-stu-id="0bd65-289">General options configuration</span></span>

<span data-ttu-id="0bd65-290">一般選項設定會示範為範例應用程式中的範例1。</span><span class="sxs-lookup"><span data-stu-id="0bd65-290">General options configuration is demonstrated as Example 1 in the sample app.</span></span>

<span data-ttu-id="0bd65-291">選項類別必須為非抽象，且具有公用的無參數建構函式。</span><span class="sxs-lookup"><span data-stu-id="0bd65-291">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="0bd65-292">下列 `MyOptions` 類別有兩個屬性，`Option1` 和 `Option2`。</span><span class="sxs-lookup"><span data-stu-id="0bd65-292">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="0bd65-293">設定預設值為選擇性，但在下列範例中，類別建構函式會設定 `Option1` 的預設值。</span><span class="sxs-lookup"><span data-stu-id="0bd65-293">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="0bd65-294">`Option2` 的預設值直接藉由初始化屬性來設定 (*Models/MyOptions.cs*)：</span><span class="sxs-lookup"><span data-stu-id="0bd65-294">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="0bd65-295">`MyOptions` 類別使用 <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> 新增到服務容器，並繫結到設定：</span><span class="sxs-lookup"><span data-stu-id="0bd65-295">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="0bd65-296">下列頁面模型使用[建構函式相依性插入](xref:mvc/controllers/dependency-injection)搭配 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 來存取設定 (*Pages/Index.cshtml.cs*)：</span><span class="sxs-lookup"><span data-stu-id="0bd65-296">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="0bd65-297">範例的 *appsettings.json* 檔案指定 `option1` 和 `option2` 的值：</span><span class="sxs-lookup"><span data-stu-id="0bd65-297">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="0bd65-298">當應用程式執行時，頁面模型的 `OnGet` 方法會傳回字串，顯示選項類別值：</span><span class="sxs-lookup"><span data-stu-id="0bd65-298">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="0bd65-299">使用自訂 <xref:System.Configuration.ConfigurationBuilder> 從設定檔載入選項設定時，請確認已正確設定基底路徑：</span><span class="sxs-lookup"><span data-stu-id="0bd65-299">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="0bd65-300">透過 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 從設定檔載入選項設定時，不需要明確設定基底路徑。</span><span class="sxs-lookup"><span data-stu-id="0bd65-300">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="0bd65-301">使用委派設定簡單的選項</span><span class="sxs-lookup"><span data-stu-id="0bd65-301">Configure simple options with a delegate</span></span>

<span data-ttu-id="0bd65-302">使用委派來設定簡單的選項，會示範為範例應用程式中的範例2。</span><span class="sxs-lookup"><span data-stu-id="0bd65-302">Configuring simple options with a delegate is demonstrated as Example 2 in the sample app.</span></span>

<span data-ttu-id="0bd65-303">使用委派來設定選項值。</span><span class="sxs-lookup"><span data-stu-id="0bd65-303">Use a delegate to set options values.</span></span> <span data-ttu-id="0bd65-304">範例應用程式使用 `MyOptionsWithDelegateConfig` 類別 (*Models/MyOptionsWithDelegateConfig.cs*)：</span><span class="sxs-lookup"><span data-stu-id="0bd65-304">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="0bd65-305">在下列程式碼中，第二個 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 服務新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="0bd65-305">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="0bd65-306">它使用委派，以 `MyOptionsWithDelegateConfig` 設定繫結：</span><span class="sxs-lookup"><span data-stu-id="0bd65-306">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="0bd65-307">*Index.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="0bd65-307">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="0bd65-308">您可以新增多個設定提供者。</span><span class="sxs-lookup"><span data-stu-id="0bd65-308">You can add multiple configuration providers.</span></span> <span data-ttu-id="0bd65-309">設定提供者可在 NuGet 套件中找到，而且會依註冊順序套用。</span><span class="sxs-lookup"><span data-stu-id="0bd65-309">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="0bd65-310">如需詳細資訊，請參閱 <xref:fundamentals/configuration/index>。</span><span class="sxs-lookup"><span data-stu-id="0bd65-310">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="0bd65-311">每次呼叫 <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> 時都會新增 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 服務到服務容器。</span><span class="sxs-lookup"><span data-stu-id="0bd65-311">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="0bd65-312">在上述範例中，`Option1` 和 `Option2` 的值都指定在 *appsettings.json* 中，但 `Option1` 和 `Option2` 的值會被設定的委派所覆寫。</span><span class="sxs-lookup"><span data-stu-id="0bd65-312">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="0bd65-313">啟用多個設定服務時，最後一個指定的設定來源會「勝出」並設定此組態值。</span><span class="sxs-lookup"><span data-stu-id="0bd65-313">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="0bd65-314">當應用程式執行時，頁面模型的 `OnGet` 方法會傳回字串，顯示選項類別值：</span><span class="sxs-lookup"><span data-stu-id="0bd65-314">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="0bd65-315">子選項組態</span><span class="sxs-lookup"><span data-stu-id="0bd65-315">Suboptions configuration</span></span>

<span data-ttu-id="0bd65-316">子選項設定會示範為範例應用程式中的範例3。</span><span class="sxs-lookup"><span data-stu-id="0bd65-316">Suboptions configuration is demonstrated as Example 3 in the sample app.</span></span>

<span data-ttu-id="0bd65-317">應用程式應該建立屬於應用程式中特定案例群組 (類別) 的選項類別。</span><span class="sxs-lookup"><span data-stu-id="0bd65-317">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="0bd65-318">需要組態值的應用程式組件應該只能存取它們使用的設定值。</span><span class="sxs-lookup"><span data-stu-id="0bd65-318">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="0bd65-319">將選項繫結至組態時，選項類型中的每一個屬性都會繫結至 `property[:sub-property:]` 格式的組態索引鍵。</span><span class="sxs-lookup"><span data-stu-id="0bd65-319">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="0bd65-320">例如，`MyOptions.Option1` 屬性繫結至索引鍵 `Option1`，其是從 `option1`appsettings.json*中的* 屬性讀取。</span><span class="sxs-lookup"><span data-stu-id="0bd65-320">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="0bd65-321">在下列程式碼中，第三個 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 服務新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="0bd65-321">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="0bd65-322">它將 `MySubOptions` 繫結至 `subsection`appSettings.json*檔案的* 區段：</span><span class="sxs-lookup"><span data-stu-id="0bd65-322">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="0bd65-323">`GetSection` 方法需要 <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> 命名空間。</span><span class="sxs-lookup"><span data-stu-id="0bd65-323">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="0bd65-324">範例的 *appsettings.json* 檔案會定義 `subsection` 成員，並具有 `suboption1` 和 `suboption2` 的索引鍵：</span><span class="sxs-lookup"><span data-stu-id="0bd65-324">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="0bd65-325">`MySubOptions` 類別會定義屬性 `SubOption1` 和 `SubOption2`，來保存選項值 (*Models/MySubOptions.cs*)：</span><span class="sxs-lookup"><span data-stu-id="0bd65-325">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="0bd65-326">頁面模型的 `OnGet` 方法會傳回具有選項值 (*Pages/Index.cshtml.cs*) 的字串：</span><span class="sxs-lookup"><span data-stu-id="0bd65-326">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="0bd65-327">當應用程式執行時，`OnGet` 方法會傳回字串，顯示子選項類別值：</span><span class="sxs-lookup"><span data-stu-id="0bd65-327">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-injection"></a><span data-ttu-id="0bd65-328">選項插入</span><span class="sxs-lookup"><span data-stu-id="0bd65-328">Options injection</span></span>

<span data-ttu-id="0bd65-329">範例應用程式中的範例4示範了選項插入。</span><span class="sxs-lookup"><span data-stu-id="0bd65-329">Options injection is demonstrated as Example 4 in the sample app.</span></span>

<span data-ttu-id="0bd65-330">將 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 插入：</span><span class="sxs-lookup"><span data-stu-id="0bd65-330">Inject <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into:</span></span>

* <span data-ttu-id="0bd65-331">具有[`@inject`](xref:mvc/views/razor#inject) razor 指示詞的 Razor 頁面或 MVC 視圖。</span><span class="sxs-lookup"><span data-stu-id="0bd65-331">A Razor page or MVC view with the [`@inject`](xref:mvc/views/razor#inject) Razor directive.</span></span>
* <span data-ttu-id="0bd65-332">頁面或視圖模型。</span><span class="sxs-lookup"><span data-stu-id="0bd65-332">A page or view model.</span></span>

<span data-ttu-id="0bd65-333">下列來自範例應用程式的範例會將 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 插入頁面模型中（*Pages/Index. cshtml .cs*）：</span><span class="sxs-lookup"><span data-stu-id="0bd65-333">The following example from the sample app injects <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into a page model (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="0bd65-334">範例應用程式會顯示如何使用 `IOptionsMonitor<MyOptions>` 指示詞來插入 `@inject`：</span><span class="sxs-lookup"><span data-stu-id="0bd65-334">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/2.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="0bd65-335">執行應用程式時，轉譯的頁面中會顯示選項值：</span><span class="sxs-lookup"><span data-stu-id="0bd65-335">When the app is run, the options values are shown in the rendered page:</span></span>

![選項值 Option1:：value1_from_json 和 Option2: -1 是從模型藉由插入至檢視來載入。](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="0bd65-337">使用 IOptionsSnapshot 重新載入設定資料</span><span class="sxs-lookup"><span data-stu-id="0bd65-337">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="0bd65-338">範例應用程式中的範例5示範使用 <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> 重載設定資料。</span><span class="sxs-lookup"><span data-stu-id="0bd65-338">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example 5 in the sample app.</span></span>

<span data-ttu-id="0bd65-339">使用 <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>，在要求的存留期記憶體取和快取時，會針對每個要求計算一次的選項。</span><span class="sxs-lookup"><span data-stu-id="0bd65-339">Using <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="0bd65-340">`IOptionsMonitor` 和 `IOptionsSnapshot` 之間的差異在於：</span><span class="sxs-lookup"><span data-stu-id="0bd65-340">The difference between `IOptionsMonitor` and `IOptionsSnapshot` is that:</span></span>

* <span data-ttu-id="0bd65-341">`IOptionsMonitor` 是一項[單一服務](xref:fundamentals/dependency-injection#singleton)，可隨時抓取目前的選項值，這在單一相依性中特別有用。</span><span class="sxs-lookup"><span data-stu-id="0bd65-341">`IOptionsMonitor` is a [singleton service](xref:fundamentals/dependency-injection#singleton) that retrieves current option values at any time, which is especially useful in singleton dependencies.</span></span>
* <span data-ttu-id="0bd65-342">`IOptionsSnapshot` 是已設定[範圍的服務](xref:fundamentals/dependency-injection#scoped)，並會在建立 `IOptionsSnapshot<T>` 物件時提供選項的快照集。</span><span class="sxs-lookup"><span data-stu-id="0bd65-342">`IOptionsSnapshot` is a [scoped service](xref:fundamentals/dependency-injection#scoped) and provides a snapshot of the options at the time the `IOptionsSnapshot<T>` object is constructed.</span></span> <span data-ttu-id="0bd65-343">選項快照集的設計目的是要搭配暫時性和範圍相依性使用。</span><span class="sxs-lookup"><span data-stu-id="0bd65-343">Options snapshots are designed for use with transient and scoped dependencies.</span></span>

<span data-ttu-id="0bd65-344">下列範例示範在 <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>appsettings.json*變更之後如何建立新的* (*Pages/Index.cshtml.cs*)。</span><span class="sxs-lookup"><span data-stu-id="0bd65-344">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="0bd65-345">對伺服器的多個要求會傳回 *appsettings.json* 檔案所提供的常數值，直到檔案變更並重新載入組態為止。</span><span class="sxs-lookup"><span data-stu-id="0bd65-345">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="0bd65-346">下圖顯示從 `option1`appsettings.json`option2` 檔案載入的初始 *和* 值：</span><span class="sxs-lookup"><span data-stu-id="0bd65-346">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="0bd65-347">將 *appsettings.json* 檔案中的值變更為 `value1_from_json UPDATED` 和 `200`。</span><span class="sxs-lookup"><span data-stu-id="0bd65-347">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="0bd65-348">儲存 *appsettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="0bd65-348">Save the *appsettings.json* file.</span></span> <span data-ttu-id="0bd65-349">重新整理瀏覽器，以查看選項值更新：</span><span class="sxs-lookup"><span data-stu-id="0bd65-349">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="0bd65-350">IConfigureNamedOptions 的具名選項支援</span><span class="sxs-lookup"><span data-stu-id="0bd65-350">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="0bd65-351">範例應用程式中的範例6示範了 <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> 的命名選項支援。</span><span class="sxs-lookup"><span data-stu-id="0bd65-351">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example 6 in the sample app.</span></span>

<span data-ttu-id="0bd65-352">命名選項支援可讓應用程式區別已命名的選項設定。</span><span class="sxs-lookup"><span data-stu-id="0bd65-352">Named options support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="0bd65-353">在範例應用程式中，名稱為 options 的宣告為[optionsservicecollectionextensions.configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*)，它會呼叫[configurenamedoptions .configure\<TOptions >。設定](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*)擴充方法。</span><span class="sxs-lookup"><span data-stu-id="0bd65-353">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method.</span></span> <span data-ttu-id="0bd65-354">已命名的選項會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="0bd65-354">Named options are case sensitive.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="0bd65-355">範例應用程式會使用　<xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*) 來存取具名選項：</span><span class="sxs-lookup"><span data-stu-id="0bd65-355">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="0bd65-356">執行範例應用程式後，會傳回具名選項：</span><span class="sxs-lookup"><span data-stu-id="0bd65-356">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="0bd65-357">`named_options_1` 值會從設定提供，這會從 *appsettings.json* 檔案載入。</span><span class="sxs-lookup"><span data-stu-id="0bd65-357">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="0bd65-358">`named_options_2` 值是由以下提供：</span><span class="sxs-lookup"><span data-stu-id="0bd65-358">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="0bd65-359">`named_options_2` 中 `ConfigureServices` 的 `Option1` 委派。</span><span class="sxs-lookup"><span data-stu-id="0bd65-359">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="0bd65-360">`Option2` 類別所提供的 `MyOptions` 預設值。</span><span class="sxs-lookup"><span data-stu-id="0bd65-360">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="0bd65-361">使用 ConfigureAll 方法設定所有選項</span><span class="sxs-lookup"><span data-stu-id="0bd65-361">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="0bd65-362">使用 <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> 方法設定所有選項執行個體。</span><span class="sxs-lookup"><span data-stu-id="0bd65-362">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="0bd65-363">下列程式碼會為具有共通值的所有設定執行個體設定 `Option1`。</span><span class="sxs-lookup"><span data-stu-id="0bd65-363">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="0bd65-364">將下列程式碼手動新增至 `Startup.ConfigureServices` 方法：</span><span class="sxs-lookup"><span data-stu-id="0bd65-364">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="0bd65-365">新增程式碼之後執行範例應用程式，就會產生下列結果：</span><span class="sxs-lookup"><span data-stu-id="0bd65-365">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="0bd65-366">所有選項都是具名執行個體。</span><span class="sxs-lookup"><span data-stu-id="0bd65-366">All options are named instances.</span></span> <span data-ttu-id="0bd65-367">現有的 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 執行個體會視為以 `Options.DefaultName` 執行個體為目標，也就是 `string.Empty`。</span><span class="sxs-lookup"><span data-stu-id="0bd65-367">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="0bd65-368"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> 也會實作 <xref:Microsoft.Extensions.Options.IConfigureOptions%601>。</span><span class="sxs-lookup"><span data-stu-id="0bd65-368"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="0bd65-369"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> 的預設實作有邏輯可以適當地使用每個項目。</span><span class="sxs-lookup"><span data-stu-id="0bd65-369">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="0bd65-370">`null` 具名選項用來以所有具名執行個體為目標，而不是特定的具名執行個體 (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> 與 <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> 使用此慣例)。</span><span class="sxs-lookup"><span data-stu-id="0bd65-370">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="0bd65-371">OptionsBuilder API</span><span class="sxs-lookup"><span data-stu-id="0bd65-371">OptionsBuilder API</span></span>

<span data-ttu-id="0bd65-372"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> 會用於設定 `TOptions` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="0bd65-372"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="0bd65-373">因為 `OptionsBuilder` 僅為初始 `AddOptions<TOptions>(string optionsName)` 呼叫的單一參數，而不是出現在所有後續呼叫的參數，所以其可簡化建立具名選項的程序。</span><span class="sxs-lookup"><span data-stu-id="0bd65-373">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="0bd65-374">選項驗證及接受服務依存性的 `ConfigureOptions` 多載，只可透過 `OptionsBuilder` 使用。</span><span class="sxs-lookup"><span data-stu-id="0bd65-374">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="0bd65-375">使用 DI 服務來設定選項</span><span class="sxs-lookup"><span data-stu-id="0bd65-375">Use DI services to configure options</span></span>

<span data-ttu-id="0bd65-376">在以下列兩種方式設定選項的同時，您可以從相依性插入中存取其他服務：</span><span class="sxs-lookup"><span data-stu-id="0bd65-376">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="0bd65-377">將設定委派傳遞到 [OptionsBuilder](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*)TOptions>[ 上的 \<Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1)。</span><span class="sxs-lookup"><span data-stu-id="0bd65-377">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="0bd65-378">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) 提供 [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) 的多載，可讓您最多使用五個服務來設定選項：</span><span class="sxs-lookup"><span data-stu-id="0bd65-378">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="0bd65-379">建立您自己的類型來實作 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 或 <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>，並將該類型註冊為服務。</span><span class="sxs-lookup"><span data-stu-id="0bd65-379">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="0bd65-380">我們建議您將設定委派傳遞到 [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*)，因為建立服務更複雜。</span><span class="sxs-lookup"><span data-stu-id="0bd65-380">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="0bd65-381">建立您自己的類型相當於，當您使用 [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) 時此架構可為您執行的動作。</span><span class="sxs-lookup"><span data-stu-id="0bd65-381">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="0bd65-382">呼叫 [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) 會註冊暫時性泛型 <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>，其具有會接受所指定泛型服務類型的建構函式。</span><span class="sxs-lookup"><span data-stu-id="0bd65-382">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-validation"></a><span data-ttu-id="0bd65-383">選項驗證</span><span class="sxs-lookup"><span data-stu-id="0bd65-383">Options validation</span></span>

<span data-ttu-id="0bd65-384">選項驗證可讓您在設定選項之後驗證選項。</span><span class="sxs-lookup"><span data-stu-id="0bd65-384">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="0bd65-385">搭配驗證方法呼叫 `Validate`，若選項有效會傳回 `true`，若為無效則傳回 `false`：</span><span class="sxs-lookup"><span data-stu-id="0bd65-385">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

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

<span data-ttu-id="0bd65-386">上述範例會將具名的選項執行個體設定為 `optionalOptionsName`。</span><span class="sxs-lookup"><span data-stu-id="0bd65-386">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="0bd65-387">預設選項執行個體為 `Options.DefaultName`。</span><span class="sxs-lookup"><span data-stu-id="0bd65-387">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="0bd65-388">當選項執行個體建立之後，便會執行驗證。</span><span class="sxs-lookup"><span data-stu-id="0bd65-388">Validation runs when the options instance is created.</span></span> <span data-ttu-id="0bd65-389">選項實例保證會在第一次存取時通過驗證。</span><span class="sxs-lookup"><span data-stu-id="0bd65-389">An options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0bd65-390">選項驗證不會在建立 options 實例之後防護選項的修改。</span><span class="sxs-lookup"><span data-stu-id="0bd65-390">Options validation doesn't guard against options modifications after the options instance is created.</span></span> <span data-ttu-id="0bd65-391">例如，第一次存取選項時，會針對每個要求建立和驗證 `IOptionsSnapshot` 選項。</span><span class="sxs-lookup"><span data-stu-id="0bd65-391">For example, `IOptionsSnapshot` options are created and validated once per request when the options are first accessed.</span></span> <span data-ttu-id="0bd65-392">*針對相同要求*的後續存取嘗試時，不會再次驗證 `IOptionsSnapshot` 選項。</span><span class="sxs-lookup"><span data-stu-id="0bd65-392">The `IOptionsSnapshot` options aren't validated again on subsequent access attempts *for the same request*.</span></span>

<span data-ttu-id="0bd65-393">`Validate` 方法可接受 `Func<TOptions, bool>`。</span><span class="sxs-lookup"><span data-stu-id="0bd65-393">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="0bd65-394">若要完整地自訂驗證，請實作 `IValidateOptions<TOptions>`，它允許：</span><span class="sxs-lookup"><span data-stu-id="0bd65-394">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="0bd65-395">多種選項類型的驗證：`class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span><span class="sxs-lookup"><span data-stu-id="0bd65-395">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="0bd65-396">取決於另一個選項類型的驗證：`public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span><span class="sxs-lookup"><span data-stu-id="0bd65-396">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span></span>

<span data-ttu-id="0bd65-397">`IValidateOptions` 可驗證：</span><span class="sxs-lookup"><span data-stu-id="0bd65-397">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="0bd65-398">特定的具名選項執行個體。</span><span class="sxs-lookup"><span data-stu-id="0bd65-398">A specific named options instance.</span></span>
* <span data-ttu-id="0bd65-399">所有選項 (當 `name` 為 `null` 時)。</span><span class="sxs-lookup"><span data-stu-id="0bd65-399">All options when `name` is `null`.</span></span>

<span data-ttu-id="0bd65-400">從以下的介面實作傳回 `ValidateOptionsResult`：</span><span class="sxs-lookup"><span data-stu-id="0bd65-400">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="0bd65-401">透過呼叫 [ 上的 ](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) 方法，就可從 <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*>Microsoft.Extensions.Options.DataAnnotations`OptionsBuilder<TOptions>` 套件使用資料註解型驗證。</span><span class="sxs-lookup"><span data-stu-id="0bd65-401">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> method on `OptionsBuilder<TOptions>`.</span></span> <span data-ttu-id="0bd65-402">`Microsoft.Extensions.Options.DataAnnotations` 包含在[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)中。</span><span class="sxs-lookup"><span data-stu-id="0bd65-402">`Microsoft.Extensions.Options.DataAnnotations` is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

```csharp
using Microsoft.Extensions.DependencyInjection;

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
        sp.GetRequiredService<IOptionsMonitor<AnnotatedOptions>>().CurrentValue);
    ValidateFailure<AnnotatedOptions>(error, Options.DefaultName, 1,
        "DataAnnotation validation failed for members Required " +
            "with the error 'The Required field is required.'.",
        "DataAnnotation validation failed for members StringLength " +
            "with the error 'Too long.'.",
        "DataAnnotation validation failed for members IntRange " +
            "with the error 'Out of range.'.");
}
```

<span data-ttu-id="0bd65-403">我們考慮在之後的版本中加入積極式驗證 (啟動時快速檢錯)。</span><span class="sxs-lookup"><span data-stu-id="0bd65-403">Eager validation (fail fast at startup) is under consideration for a future release.</span></span>

## <a name="options-post-configuration"></a><span data-ttu-id="0bd65-404">選項設定後作業</span><span class="sxs-lookup"><span data-stu-id="0bd65-404">Options post-configuration</span></span>

<span data-ttu-id="0bd65-405">使用 <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> 來設定設定後作業。</span><span class="sxs-lookup"><span data-stu-id="0bd65-405">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="0bd65-406">設定後作業會在所有 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 設定發生後執行：</span><span class="sxs-lookup"><span data-stu-id="0bd65-406">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="0bd65-407"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> 可用來後置設定具名選項：</span><span class="sxs-lookup"><span data-stu-id="0bd65-407"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="0bd65-408">使用 <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> 後置設定所有設定執行個體：</span><span class="sxs-lookup"><span data-stu-id="0bd65-408">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="0bd65-409">在啟動期間存取選項</span><span class="sxs-lookup"><span data-stu-id="0bd65-409">Accessing options during startup</span></span>

<span data-ttu-id="0bd65-410"><xref:Microsoft.Extensions.Options.IOptions%601> 與 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 可用於 `Startup.Configure`，因為服務是在 `Configure` 方法執行之前建置。</span><span class="sxs-lookup"><span data-stu-id="0bd65-410"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="0bd65-411">請勿在 <xref:Microsoft.Extensions.Options.IOptions%601> 中使用 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 或 `Startup.ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="0bd65-411">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="0bd65-412">可能會因為服務註冊的順序而有不一致的選項狀態存在。</span><span class="sxs-lookup"><span data-stu-id="0bd65-412">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="0bd65-413">選項模式使用類別來代表一組相關的設定。</span><span class="sxs-lookup"><span data-stu-id="0bd65-413">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="0bd65-414">當[組態設定](xref:fundamentals/configuration/index)依案例隔離到不同的類別時，應用程式會遵守兩個重要的軟體工程準則：</span><span class="sxs-lookup"><span data-stu-id="0bd65-414">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="0bd65-415">依存于 configuration 設定的[介面隔離原則（ISP）或封裝](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation)&ndash; 案例（類別），只取決於它們所使用的設定。</span><span class="sxs-lookup"><span data-stu-id="0bd65-415">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="0bd65-416">在應用程式的不同部分 &ndash; 設定不會彼此相依或彼此結合[的問題分離](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns)。</span><span class="sxs-lookup"><span data-stu-id="0bd65-416">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="0bd65-417">選項也提供驗證設定資料的機制。</span><span class="sxs-lookup"><span data-stu-id="0bd65-417">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="0bd65-418">如需詳細資訊，請參閱[選項驗證](#options-validation)一節。</span><span class="sxs-lookup"><span data-stu-id="0bd65-418">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="0bd65-419">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0bd65-419">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0bd65-420">必要條件</span><span class="sxs-lookup"><span data-stu-id="0bd65-420">Prerequisites</span></span>

<span data-ttu-id="0bd65-421">參考 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)，或新增 [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) 套件的套件參考。</span><span class="sxs-lookup"><span data-stu-id="0bd65-421">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="0bd65-422">選項介面</span><span class="sxs-lookup"><span data-stu-id="0bd65-422">Options interfaces</span></span>

<span data-ttu-id="0bd65-423"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 是用來擷取選項並管理 `TOptions` 執行個體的選項通知。</span><span class="sxs-lookup"><span data-stu-id="0bd65-423"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="0bd65-424"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 支援以下案例：</span><span class="sxs-lookup"><span data-stu-id="0bd65-424"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="0bd65-425">變更通知</span><span class="sxs-lookup"><span data-stu-id="0bd65-425">Change notifications</span></span>
* [<span data-ttu-id="0bd65-426">具名選項</span><span class="sxs-lookup"><span data-stu-id="0bd65-426">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="0bd65-427">可重新載入的設定</span><span class="sxs-lookup"><span data-stu-id="0bd65-427">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="0bd65-428">選擇性選項無效判定 (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="0bd65-428">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="0bd65-429">[設定後](#options-post-configuration)案例可讓您在所有 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 設定發生時設定或變更選項。</span><span class="sxs-lookup"><span data-stu-id="0bd65-429">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="0bd65-430"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> 負責建立新的選項執行個體。</span><span class="sxs-lookup"><span data-stu-id="0bd65-430"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="0bd65-431">它有單一 <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> 方法。</span><span class="sxs-lookup"><span data-stu-id="0bd65-431">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="0bd65-432">預設實作會接受所有已註冊的 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 與 <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>，並先執行所有設定，接著執行設定後作業。</span><span class="sxs-lookup"><span data-stu-id="0bd65-432">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="0bd65-433">它會區別 <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> 和 <xref:Microsoft.Extensions.Options.IConfigureOptions%601>，且只會呼叫適當的介面。</span><span class="sxs-lookup"><span data-stu-id="0bd65-433">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="0bd65-434"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> 會由 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 用來快取 `TOptions` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="0bd65-434"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="0bd65-435"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> 會使監視器中的選項執行個體失效，以便重新計算值 (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>)。</span><span class="sxs-lookup"><span data-stu-id="0bd65-435">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="0bd65-436">值可以使用 <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*> 手動導入。</span><span class="sxs-lookup"><span data-stu-id="0bd65-436">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="0bd65-437"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> 方法用於應該視需要重新建立所有具名執行個體時。</span><span class="sxs-lookup"><span data-stu-id="0bd65-437">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="0bd65-438"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> 在應該於收到每個要求時重新計算選項的案例中很實用用。</span><span class="sxs-lookup"><span data-stu-id="0bd65-438"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="0bd65-439">如需詳細資訊，請參閱[使用 IOptionsSnapshot 重新載入設定資料](#reload-configuration-data-with-ioptionssnapshot)一節。</span><span class="sxs-lookup"><span data-stu-id="0bd65-439">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="0bd65-440"><xref:Microsoft.Extensions.Options.IOptions%601> 可用於支援選項。</span><span class="sxs-lookup"><span data-stu-id="0bd65-440"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="0bd65-441">不過，<xref:Microsoft.Extensions.Options.IOptions%601> 不支援前面的 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 案例。</span><span class="sxs-lookup"><span data-stu-id="0bd65-441">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="0bd65-442">您可以在現有架構與程式庫中繼續使用 <xref:Microsoft.Extensions.Options.IOptions%601>，此程式庫已使用 <xref:Microsoft.Extensions.Options.IOptions%601> 介面且不需要 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 提供的案例。</span><span class="sxs-lookup"><span data-stu-id="0bd65-442">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="0bd65-443">一般選項設定</span><span class="sxs-lookup"><span data-stu-id="0bd65-443">General options configuration</span></span>

<span data-ttu-id="0bd65-444">一般選項設定會示範為範例應用程式中的範例1。</span><span class="sxs-lookup"><span data-stu-id="0bd65-444">General options configuration is demonstrated as Example 1 in the sample app.</span></span>

<span data-ttu-id="0bd65-445">選項類別必須為非抽象，且具有公用的無參數建構函式。</span><span class="sxs-lookup"><span data-stu-id="0bd65-445">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="0bd65-446">下列 `MyOptions` 類別有兩個屬性，`Option1` 和 `Option2`。</span><span class="sxs-lookup"><span data-stu-id="0bd65-446">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="0bd65-447">設定預設值為選擇性，但在下列範例中，類別建構函式會設定 `Option1` 的預設值。</span><span class="sxs-lookup"><span data-stu-id="0bd65-447">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="0bd65-448">`Option2` 的預設值直接藉由初始化屬性來設定 (*Models/MyOptions.cs*)：</span><span class="sxs-lookup"><span data-stu-id="0bd65-448">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="0bd65-449">`MyOptions` 類別使用 <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> 新增到服務容器，並繫結到設定：</span><span class="sxs-lookup"><span data-stu-id="0bd65-449">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="0bd65-450">下列頁面模型使用[建構函式相依性插入](xref:mvc/controllers/dependency-injection)搭配 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 來存取設定 (*Pages/Index.cshtml.cs*)：</span><span class="sxs-lookup"><span data-stu-id="0bd65-450">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="0bd65-451">範例的 *appsettings.json* 檔案指定 `option1` 和 `option2` 的值：</span><span class="sxs-lookup"><span data-stu-id="0bd65-451">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="0bd65-452">當應用程式執行時，頁面模型的 `OnGet` 方法會傳回字串，顯示選項類別值：</span><span class="sxs-lookup"><span data-stu-id="0bd65-452">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="0bd65-453">使用自訂 <xref:System.Configuration.ConfigurationBuilder> 從設定檔載入選項設定時，請確認已正確設定基底路徑：</span><span class="sxs-lookup"><span data-stu-id="0bd65-453">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="0bd65-454">透過 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 從設定檔載入選項設定時，不需要明確設定基底路徑。</span><span class="sxs-lookup"><span data-stu-id="0bd65-454">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="0bd65-455">使用委派設定簡單的選項</span><span class="sxs-lookup"><span data-stu-id="0bd65-455">Configure simple options with a delegate</span></span>

<span data-ttu-id="0bd65-456">使用委派來設定簡單的選項，會示範為範例應用程式中的範例2。</span><span class="sxs-lookup"><span data-stu-id="0bd65-456">Configuring simple options with a delegate is demonstrated as Example 2 in the sample app.</span></span>

<span data-ttu-id="0bd65-457">使用委派來設定選項值。</span><span class="sxs-lookup"><span data-stu-id="0bd65-457">Use a delegate to set options values.</span></span> <span data-ttu-id="0bd65-458">範例應用程式使用 `MyOptionsWithDelegateConfig` 類別 (*Models/MyOptionsWithDelegateConfig.cs*)：</span><span class="sxs-lookup"><span data-stu-id="0bd65-458">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="0bd65-459">在下列程式碼中，第二個 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 服務新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="0bd65-459">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="0bd65-460">它使用委派，以 `MyOptionsWithDelegateConfig` 設定繫結：</span><span class="sxs-lookup"><span data-stu-id="0bd65-460">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="0bd65-461">*Index.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="0bd65-461">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="0bd65-462">您可以新增多個設定提供者。</span><span class="sxs-lookup"><span data-stu-id="0bd65-462">You can add multiple configuration providers.</span></span> <span data-ttu-id="0bd65-463">設定提供者可在 NuGet 套件中找到，而且會依註冊順序套用。</span><span class="sxs-lookup"><span data-stu-id="0bd65-463">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="0bd65-464">如需詳細資訊，請參閱 <xref:fundamentals/configuration/index>。</span><span class="sxs-lookup"><span data-stu-id="0bd65-464">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="0bd65-465">每次呼叫 <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> 時都會新增 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 服務到服務容器。</span><span class="sxs-lookup"><span data-stu-id="0bd65-465">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="0bd65-466">在上述範例中，`Option1` 和 `Option2` 的值都指定在 *appsettings.json* 中，但 `Option1` 和 `Option2` 的值會被設定的委派所覆寫。</span><span class="sxs-lookup"><span data-stu-id="0bd65-466">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="0bd65-467">啟用多個設定服務時，最後一個指定的設定來源會「勝出」並設定此組態值。</span><span class="sxs-lookup"><span data-stu-id="0bd65-467">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="0bd65-468">當應用程式執行時，頁面模型的 `OnGet` 方法會傳回字串，顯示選項類別值：</span><span class="sxs-lookup"><span data-stu-id="0bd65-468">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="0bd65-469">子選項組態</span><span class="sxs-lookup"><span data-stu-id="0bd65-469">Suboptions configuration</span></span>

<span data-ttu-id="0bd65-470">子選項設定會示範為範例應用程式中的範例3。</span><span class="sxs-lookup"><span data-stu-id="0bd65-470">Suboptions configuration is demonstrated as Example 3 in the sample app.</span></span>

<span data-ttu-id="0bd65-471">應用程式應該建立屬於應用程式中特定案例群組 (類別) 的選項類別。</span><span class="sxs-lookup"><span data-stu-id="0bd65-471">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="0bd65-472">需要組態值的應用程式組件應該只能存取它們使用的設定值。</span><span class="sxs-lookup"><span data-stu-id="0bd65-472">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="0bd65-473">將選項繫結至組態時，選項類型中的每一個屬性都會繫結至 `property[:sub-property:]` 格式的組態索引鍵。</span><span class="sxs-lookup"><span data-stu-id="0bd65-473">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="0bd65-474">例如，`MyOptions.Option1` 屬性繫結至索引鍵 `Option1`，其是從 `option1`appsettings.json*中的* 屬性讀取。</span><span class="sxs-lookup"><span data-stu-id="0bd65-474">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="0bd65-475">在下列程式碼中，第三個 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 服務新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="0bd65-475">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="0bd65-476">它將 `MySubOptions` 繫結至 `subsection`appSettings.json*檔案的* 區段：</span><span class="sxs-lookup"><span data-stu-id="0bd65-476">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="0bd65-477">`GetSection` 方法需要 <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> 命名空間。</span><span class="sxs-lookup"><span data-stu-id="0bd65-477">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="0bd65-478">範例的 *appsettings.json* 檔案會定義 `subsection` 成員，並具有 `suboption1` 和 `suboption2` 的索引鍵：</span><span class="sxs-lookup"><span data-stu-id="0bd65-478">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="0bd65-479">`MySubOptions` 類別會定義屬性 `SubOption1` 和 `SubOption2`，來保存選項值 (*Models/MySubOptions.cs*)：</span><span class="sxs-lookup"><span data-stu-id="0bd65-479">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="0bd65-480">頁面模型的 `OnGet` 方法會傳回具有選項值 (*Pages/Index.cshtml.cs*) 的字串：</span><span class="sxs-lookup"><span data-stu-id="0bd65-480">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="0bd65-481">當應用程式執行時，`OnGet` 方法會傳回字串，顯示子選項類別值：</span><span class="sxs-lookup"><span data-stu-id="0bd65-481">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="0bd65-482">檢視模型提供的選項或使用直接檢視插入提供的選項</span><span class="sxs-lookup"><span data-stu-id="0bd65-482">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="0bd65-483">視圖模型或直接視圖插入所提供的選項，在範例應用程式中會以範例4來示範。</span><span class="sxs-lookup"><span data-stu-id="0bd65-483">Options provided by a view model or with direct view injection is demonstrated as Example 4 in the sample app.</span></span>

<span data-ttu-id="0bd65-484">可以在檢視模型中提供選項，或藉由將 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 直接插入至檢視來提供選項 (*Pages/Index.cshtml.cs*)：</span><span class="sxs-lookup"><span data-stu-id="0bd65-484">Options can be supplied in a view model or by injecting <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="0bd65-485">範例應用程式會顯示如何使用 `IOptionsMonitor<MyOptions>` 指示詞來插入 `@inject`：</span><span class="sxs-lookup"><span data-stu-id="0bd65-485">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/2.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="0bd65-486">執行應用程式時，轉譯的頁面中會顯示選項值：</span><span class="sxs-lookup"><span data-stu-id="0bd65-486">When the app is run, the options values are shown in the rendered page:</span></span>

![選項值 Option1:：value1_from_json 和 Option2: -1 是從模型藉由插入至檢視來載入。](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="0bd65-488">使用 IOptionsSnapshot 重新載入設定資料</span><span class="sxs-lookup"><span data-stu-id="0bd65-488">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="0bd65-489">範例應用程式中的範例5示範使用 <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> 重載設定資料。</span><span class="sxs-lookup"><span data-stu-id="0bd65-489">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example 5 in the sample app.</span></span>

<span data-ttu-id="0bd65-490"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> 支援以最小的處理負擔來重新載入選項。</span><span class="sxs-lookup"><span data-stu-id="0bd65-490"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> supports reloading options with minimal processing overhead.</span></span>

<span data-ttu-id="0bd65-491">在要求的存留期內存取及快取選項時，會針對每個要求計算一次選項。</span><span class="sxs-lookup"><span data-stu-id="0bd65-491">Options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="0bd65-492">下列範例示範在 <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>appsettings.json*變更之後如何建立新的* (*Pages/Index.cshtml.cs*)。</span><span class="sxs-lookup"><span data-stu-id="0bd65-492">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="0bd65-493">對伺服器的多個要求會傳回 *appsettings.json* 檔案所提供的常數值，直到檔案變更並重新載入組態為止。</span><span class="sxs-lookup"><span data-stu-id="0bd65-493">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="0bd65-494">下圖顯示從 `option1`appsettings.json`option2` 檔案載入的初始 *和* 值：</span><span class="sxs-lookup"><span data-stu-id="0bd65-494">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="0bd65-495">將 *appsettings.json* 檔案中的值變更為 `value1_from_json UPDATED` 和 `200`。</span><span class="sxs-lookup"><span data-stu-id="0bd65-495">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="0bd65-496">儲存 *appsettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="0bd65-496">Save the *appsettings.json* file.</span></span> <span data-ttu-id="0bd65-497">重新整理瀏覽器，以查看選項值更新：</span><span class="sxs-lookup"><span data-stu-id="0bd65-497">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="0bd65-498">IConfigureNamedOptions 的具名選項支援</span><span class="sxs-lookup"><span data-stu-id="0bd65-498">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="0bd65-499">範例應用程式中的範例6示範了 <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> 的命名選項支援。</span><span class="sxs-lookup"><span data-stu-id="0bd65-499">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example 6 in the sample app.</span></span>

<span data-ttu-id="0bd65-500">命名選項支援可讓應用程式區別已命名的選項設定。</span><span class="sxs-lookup"><span data-stu-id="0bd65-500">Named options support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="0bd65-501">在範例應用程式中，名稱為 options 的宣告為[optionsservicecollectionextensions.configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*)，它會呼叫[configurenamedoptions .configure\<TOptions >。設定](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*)擴充方法。</span><span class="sxs-lookup"><span data-stu-id="0bd65-501">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method.</span></span> <span data-ttu-id="0bd65-502">已命名的選項會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="0bd65-502">Named options are case sensitive.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="0bd65-503">範例應用程式會使用　<xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*) 來存取具名選項：</span><span class="sxs-lookup"><span data-stu-id="0bd65-503">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="0bd65-504">執行範例應用程式後，會傳回具名選項：</span><span class="sxs-lookup"><span data-stu-id="0bd65-504">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="0bd65-505">`named_options_1` 值會從設定提供，這會從 *appsettings.json* 檔案載入。</span><span class="sxs-lookup"><span data-stu-id="0bd65-505">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="0bd65-506">`named_options_2` 值是由以下提供：</span><span class="sxs-lookup"><span data-stu-id="0bd65-506">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="0bd65-507">`named_options_2` 中 `ConfigureServices` 的 `Option1` 委派。</span><span class="sxs-lookup"><span data-stu-id="0bd65-507">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="0bd65-508">`Option2` 類別所提供的 `MyOptions` 預設值。</span><span class="sxs-lookup"><span data-stu-id="0bd65-508">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="0bd65-509">使用 ConfigureAll 方法設定所有選項</span><span class="sxs-lookup"><span data-stu-id="0bd65-509">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="0bd65-510">使用 <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> 方法設定所有選項執行個體。</span><span class="sxs-lookup"><span data-stu-id="0bd65-510">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="0bd65-511">下列程式碼會為具有共通值的所有設定執行個體設定 `Option1`。</span><span class="sxs-lookup"><span data-stu-id="0bd65-511">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="0bd65-512">將下列程式碼手動新增至 `Startup.ConfigureServices` 方法：</span><span class="sxs-lookup"><span data-stu-id="0bd65-512">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="0bd65-513">新增程式碼之後執行範例應用程式，就會產生下列結果：</span><span class="sxs-lookup"><span data-stu-id="0bd65-513">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="0bd65-514">所有選項都是具名執行個體。</span><span class="sxs-lookup"><span data-stu-id="0bd65-514">All options are named instances.</span></span> <span data-ttu-id="0bd65-515">現有的 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 執行個體會視為以 `Options.DefaultName` 執行個體為目標，也就是 `string.Empty`。</span><span class="sxs-lookup"><span data-stu-id="0bd65-515">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="0bd65-516"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> 也會實作 <xref:Microsoft.Extensions.Options.IConfigureOptions%601>。</span><span class="sxs-lookup"><span data-stu-id="0bd65-516"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="0bd65-517"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> 的預設實作有邏輯可以適當地使用每個項目。</span><span class="sxs-lookup"><span data-stu-id="0bd65-517">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="0bd65-518">`null` 具名選項用來以所有具名執行個體為目標，而不是特定的具名執行個體 (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> 與 <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> 使用此慣例)。</span><span class="sxs-lookup"><span data-stu-id="0bd65-518">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="0bd65-519">OptionsBuilder API</span><span class="sxs-lookup"><span data-stu-id="0bd65-519">OptionsBuilder API</span></span>

<span data-ttu-id="0bd65-520"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> 會用於設定 `TOptions` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="0bd65-520"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="0bd65-521">因為 `OptionsBuilder` 僅為初始 `AddOptions<TOptions>(string optionsName)` 呼叫的單一參數，而不是出現在所有後續呼叫的參數，所以其可簡化建立具名選項的程序。</span><span class="sxs-lookup"><span data-stu-id="0bd65-521">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="0bd65-522">選項驗證及接受服務依存性的 `ConfigureOptions` 多載，只可透過 `OptionsBuilder` 使用。</span><span class="sxs-lookup"><span data-stu-id="0bd65-522">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="0bd65-523">使用 DI 服務來設定選項</span><span class="sxs-lookup"><span data-stu-id="0bd65-523">Use DI services to configure options</span></span>

<span data-ttu-id="0bd65-524">在以下列兩種方式設定選項的同時，您可以從相依性插入中存取其他服務：</span><span class="sxs-lookup"><span data-stu-id="0bd65-524">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="0bd65-525">將設定委派傳遞到 [OptionsBuilder](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*)TOptions>[ 上的 \<Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1)。</span><span class="sxs-lookup"><span data-stu-id="0bd65-525">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="0bd65-526">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) 提供 [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) 的多載，可讓您最多使用五個服務來設定選項：</span><span class="sxs-lookup"><span data-stu-id="0bd65-526">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="0bd65-527">建立您自己的類型來實作 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 或 <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>，並將該類型註冊為服務。</span><span class="sxs-lookup"><span data-stu-id="0bd65-527">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="0bd65-528">我們建議您將設定委派傳遞到 [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*)，因為建立服務更複雜。</span><span class="sxs-lookup"><span data-stu-id="0bd65-528">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="0bd65-529">建立您自己的類型相當於，當您使用 [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) 時此架構可為您執行的動作。</span><span class="sxs-lookup"><span data-stu-id="0bd65-529">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="0bd65-530">呼叫 [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) 會註冊暫時性泛型 <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>，其具有會接受所指定泛型服務類型的建構函式。</span><span class="sxs-lookup"><span data-stu-id="0bd65-530">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-post-configuration"></a><span data-ttu-id="0bd65-531">選項設定後作業</span><span class="sxs-lookup"><span data-stu-id="0bd65-531">Options post-configuration</span></span>

<span data-ttu-id="0bd65-532">使用 <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> 來設定設定後作業。</span><span class="sxs-lookup"><span data-stu-id="0bd65-532">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="0bd65-533">設定後作業會在所有 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 設定發生後執行：</span><span class="sxs-lookup"><span data-stu-id="0bd65-533">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="0bd65-534"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> 可用來後置設定具名選項：</span><span class="sxs-lookup"><span data-stu-id="0bd65-534"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="0bd65-535">使用 <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> 後置設定所有設定執行個體：</span><span class="sxs-lookup"><span data-stu-id="0bd65-535">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="0bd65-536">在啟動期間存取選項</span><span class="sxs-lookup"><span data-stu-id="0bd65-536">Accessing options during startup</span></span>

<span data-ttu-id="0bd65-537"><xref:Microsoft.Extensions.Options.IOptions%601> 與 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 可用於 `Startup.Configure`，因為服務是在 `Configure` 方法執行之前建置。</span><span class="sxs-lookup"><span data-stu-id="0bd65-537"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="0bd65-538">請勿在 <xref:Microsoft.Extensions.Options.IOptions%601> 中使用 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 或 `Startup.ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="0bd65-538">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="0bd65-539">可能會因為服務註冊的順序而有不一致的選項狀態存在。</span><span class="sxs-lookup"><span data-stu-id="0bd65-539">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="0bd65-540">其他資源</span><span class="sxs-lookup"><span data-stu-id="0bd65-540">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
