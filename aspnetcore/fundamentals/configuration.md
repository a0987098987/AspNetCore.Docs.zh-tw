---
title: "ASP.NET Core 的設定"
author: rick-anderson
description: "了解如何設定 ASP.NET Core 應用程式使用多個來源使用的組態 API。"
keywords: "ASP.NET Core，組態中，JSON 中設定"
ms.author: riande
manager: wpickett
ms.date: 6/24/2017
ms.topic: article
ms.assetid: b3a5984d-e172-42eb-8a48-547e4acb6806
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration
ms.openlocfilehash: ca6b62dd4699536b24c3422a2a51fc3fe1744f0a
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/01/2017
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="e893b-104">ASP.NET Core 的設定</span><span class="sxs-lookup"><span data-stu-id="e893b-104">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="e893b-105">[Rick Anderson](https://twitter.com/RickAndMSFT)，[標記 Michaelis](http://intellitect.com/author/mark-michaelis/)， [Steve Smith](https://ardalis.com/)，[奧 Roth](https://github.com/danroth27)，和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e893b-105">[Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="e893b-106">組態 API 提供一種設定的名稱 / 值組清單為基礎的應用程式。</span><span class="sxs-lookup"><span data-stu-id="e893b-106">The Configuration API provides a way of configuring an app based on a list of name-value pairs.</span></span> <span data-ttu-id="e893b-107">在執行階段從多個來源讀取組態。</span><span class="sxs-lookup"><span data-stu-id="e893b-107">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="e893b-108">名稱 / 值組可以分為多層級的階層。</span><span class="sxs-lookup"><span data-stu-id="e893b-108">The name-value pairs can be grouped into a multi-level hierarchy.</span></span> <span data-ttu-id="e893b-109">有的組態提供者：</span><span class="sxs-lookup"><span data-stu-id="e893b-109">There are configuration providers for:</span></span>

* <span data-ttu-id="e893b-110">檔案格式 （INI、 JSON 和 XML）</span><span class="sxs-lookup"><span data-stu-id="e893b-110">File formats (INI, JSON, and XML)</span></span>
* <span data-ttu-id="e893b-111">命令列引數</span><span class="sxs-lookup"><span data-stu-id="e893b-111">Command-line arguments</span></span>
* <span data-ttu-id="e893b-112">環境變數</span><span class="sxs-lookup"><span data-stu-id="e893b-112">Environment variables</span></span>
* <span data-ttu-id="e893b-113">記憶體中的.NET 物件</span><span class="sxs-lookup"><span data-stu-id="e893b-113">In-memory .NET objects</span></span>
* <span data-ttu-id="e893b-114">加密的使用者存放區</span><span class="sxs-lookup"><span data-stu-id="e893b-114">An encrypted user store</span></span>
* [<span data-ttu-id="e893b-115">Azure 金鑰保存庫</span><span class="sxs-lookup"><span data-stu-id="e893b-115">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
* <span data-ttu-id="e893b-116">自訂提供者，供您安裝或建立</span><span class="sxs-lookup"><span data-stu-id="e893b-116">Custom providers, which you install or create</span></span>

<span data-ttu-id="e893b-117">每個組態值將對應至字串索引鍵。</span><span class="sxs-lookup"><span data-stu-id="e893b-117">Each configuration value maps to a string key.</span></span> <span data-ttu-id="e893b-118">沒有要還原序列化到自訂設定的內建繫結支援[POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object)物件 （具有屬性的簡單.NET 類別）。</span><span class="sxs-lookup"><span data-stu-id="e893b-118">There's built-in binding support to deserialize settings into a custom [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

<span data-ttu-id="e893b-119">[檢視或下載範例程式碼](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/sample)([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e893b-119">[View or download sample code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="simple-configuration"></a><span data-ttu-id="e893b-120">簡單的組態</span><span class="sxs-lookup"><span data-stu-id="e893b-120">Simple configuration</span></span>

<span data-ttu-id="e893b-121">下列主控台應用程式會使用 JSON 組態提供者：</span><span class="sxs-lookup"><span data-stu-id="e893b-121">The following console app uses the JSON configuration provider:</span></span>

[!code-csharp[Main](configuration/sample/ConfigJson/Program.cs)]

<span data-ttu-id="e893b-122">應用程式讀取，並顯示下列組態設定：</span><span class="sxs-lookup"><span data-stu-id="e893b-122">The app reads and displays the following configuration settings:</span></span>

[!code-json[Main](configuration/sample/ConfigJson/appsettings.json)]

<span data-ttu-id="e893b-123">設定包含在其中的節點以冒號分隔的名稱 / 值組的階層式清單。</span><span class="sxs-lookup"><span data-stu-id="e893b-123">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon.</span></span> <span data-ttu-id="e893b-124">若要擷取值，存取`Configuration`與對應的項目索引鍵的索引子：</span><span class="sxs-lookup"><span data-stu-id="e893b-124">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

```csharp
Console.WriteLine($"option1 = {Configuration["subsection:suboption1"]}");
```

<span data-ttu-id="e893b-125">若要使用 JSON 格式設定來源中的陣列，使用陣列索引一部分的冒號分隔的字串。</span><span class="sxs-lookup"><span data-stu-id="e893b-125">To work with arrays in JSON-formatted configuration sources, use a array index as part of the colon-separated string.</span></span> <span data-ttu-id="e893b-126">下列範例會取得在上述第一個項目的名稱`wizards`陣列：</span><span class="sxs-lookup"><span data-stu-id="e893b-126">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}, ");
```

<span data-ttu-id="e893b-127">寫入至內建的名稱 / 值組中`Configuration`提供者是**不**保存，不過，您可以建立將值儲存的自訂提供者。</span><span class="sxs-lookup"><span data-stu-id="e893b-127">Name-value pairs written to the built in `Configuration` providers are **not** persisted, however, you can create a custom provider that saves values.</span></span> <span data-ttu-id="e893b-128">請參閱[自訂組態提供者](xref:fundamentals/configuration#custom-config-providers)。</span><span class="sxs-lookup"><span data-stu-id="e893b-128">See [custom configuration provider](xref:fundamentals/configuration#custom-config-providers).</span></span>

<span data-ttu-id="e893b-129">上述範例會使用設定索引子讀取的值。</span><span class="sxs-lookup"><span data-stu-id="e893b-129">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="e893b-130">外部存取組態`Startup`，使用[選項模式](xref:fundamentals/configuration#options-config-objects)。</span><span class="sxs-lookup"><span data-stu-id="e893b-130">To access configuration outside of `Startup`, use the [options pattern](xref:fundamentals/configuration#options-config-objects).</span></span> <span data-ttu-id="e893b-131">*選項模式*本文稍後所示。</span><span class="sxs-lookup"><span data-stu-id="e893b-131">The *options pattern* is shown later in this article.</span></span>

<span data-ttu-id="e893b-132">這是通常會有不同的環境，例如開發、 測試和實際執行不同的組態設定。</span><span class="sxs-lookup"><span data-stu-id="e893b-132">It's typical to have different configuration settings for different environments, for example, development, test, and production.</span></span> <span data-ttu-id="e893b-133">`CreateDefaultBuilder` ASP.NET Core 2.x 應用程式中的擴充方法 (或使用`AddJsonFile`和`AddEnvironmentVariables`直接在 ASP.NET Core 1.x 應用程式中) 將讀取 JSON 檔案和系統設定來源的組態提供者：</span><span class="sxs-lookup"><span data-stu-id="e893b-133">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="e893b-134">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="e893b-134">*appsettings.json*</span></span>
* <span data-ttu-id="e893b-135">*appsettings。\<EnvironmentName >.json*</span><span class="sxs-lookup"><span data-stu-id="e893b-135">*appsettings.\<EnvironmentName>.json*</span></span>
* <span data-ttu-id="e893b-136">環境變數</span><span class="sxs-lookup"><span data-stu-id="e893b-136">environment variables</span></span>

<span data-ttu-id="e893b-137">請參閱[AddJsonFile](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.jsonconfigurationextensions)取得參數的說明。</span><span class="sxs-lookup"><span data-stu-id="e893b-137">See [AddJsonFile](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="e893b-138">`reloadOnChange`僅適用於 ASP.NET Core 1.1 （含） 以上。</span><span class="sxs-lookup"><span data-stu-id="e893b-138">`reloadOnChange` is only supported in ASP.NET Core 1.1 and higher.</span></span> 

<span data-ttu-id="e893b-139">設定來源會讀取這些指定的順序。</span><span class="sxs-lookup"><span data-stu-id="e893b-139">Configuration sources are read in the order they are specified.</span></span> <span data-ttu-id="e893b-140">上述程式碼，是上次讀取環境變數。</span><span class="sxs-lookup"><span data-stu-id="e893b-140">In the code above, the environment variables are read last.</span></span> <span data-ttu-id="e893b-141">設定透過環境的所有組態值會都取代中的兩個先前提供者設定。</span><span class="sxs-lookup"><span data-stu-id="e893b-141">Any configuration values set through the environment would replace those set in the two previous providers.</span></span>

<span data-ttu-id="e893b-142">環境通常設定為其中一個`Development`， `Staging`，或`Production`。</span><span class="sxs-lookup"><span data-stu-id="e893b-142">The environment is typically set to one of `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="e893b-143">請參閱[使用多個環境](environments.md)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="e893b-143">See [Working with Multiple Environments](environments.md) for more information.</span></span>

<span data-ttu-id="e893b-144">組態的考量：</span><span class="sxs-lookup"><span data-stu-id="e893b-144">Configuration considerations:</span></span>

* <span data-ttu-id="e893b-145">`IOptionsSnapshot`當監視變更時，就可以載入組態資料。</span><span class="sxs-lookup"><span data-stu-id="e893b-145">`IOptionsSnapshot` can reload configuration data when it changes.</span></span> <span data-ttu-id="e893b-146">使用`IOptionsSnapshot`如果您要重新載入組態資料。</span><span class="sxs-lookup"><span data-stu-id="e893b-146">Use `IOptionsSnapshot` if you need to reload configuration data.</span></span>  <span data-ttu-id="e893b-147">請參閱[IOptionsSnapshot](#ioptionssnapshot)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="e893b-147">See [IOptionsSnapshot](#ioptionssnapshot) for more information.</span></span>
* <span data-ttu-id="e893b-148">設定索引鍵是不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="e893b-148">Configuration keys are case insensitive.</span></span>
* <span data-ttu-id="e893b-149">最佳作法是最後指定環境變數，讓本機的環境可以覆寫在已部署的組態檔中設定的任何項目。</span><span class="sxs-lookup"><span data-stu-id="e893b-149">A best practice is to specify environment variables last, so that the local environment can override anything set in deployed configuration files.</span></span>
* <span data-ttu-id="e893b-150">**永遠不會**將密碼或其他敏感性資料儲存在組態提供者程式碼或純文字設定檔案中。</span><span class="sxs-lookup"><span data-stu-id="e893b-150">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="e893b-151">不在您開發使用生產機密資料或測試環境。</span><span class="sxs-lookup"><span data-stu-id="e893b-151">Don't use production secrets in your development or test environments.</span></span> <span data-ttu-id="e893b-152">相反地，指定外部專案樹狀結構的機密資料，讓它們無法意外認可到您的儲存機制。</span><span class="sxs-lookup"><span data-stu-id="e893b-152">Instead, specify secrets outside the project tree, so they cannot be accidentally committed into your repository.</span></span> <span data-ttu-id="e893b-153">深入了解[使用多個環境](environments.md)和管理[安全存放應用程式密碼，在開發期間](../security/app-secrets.md)。</span><span class="sxs-lookup"><span data-stu-id="e893b-153">Learn more about [Working with Multiple Environments](environments.md) and managing [safe storage of app secrets during development](../security/app-secrets.md).</span></span>
* <span data-ttu-id="e893b-154">如果`:`不能在您的系統中，以使用環境變數中，取代`:`與`__`（雙底線）。</span><span class="sxs-lookup"><span data-stu-id="e893b-154">If `:` cannot be used in environment variables in your system,  replace `:`  with `__` (double underscore).</span></span>

<a name=options-config-objects></a>

## <a name="using-options-and-configuration-objects"></a><span data-ttu-id="e893b-155">使用選項和設定物件</span><span class="sxs-lookup"><span data-stu-id="e893b-155">Using Options and configuration objects</span></span>

<span data-ttu-id="e893b-156">選項模式會使用自訂的選項類別來代表一組相關的設定。</span><span class="sxs-lookup"><span data-stu-id="e893b-156">The options pattern uses custom options classes to represent a group of related settings.</span></span> <span data-ttu-id="e893b-157">我們建議您在應用程式中建立低耦合的每個功能類別。</span><span class="sxs-lookup"><span data-stu-id="e893b-157">We recommended that you create decoupled classes for each feature within your app.</span></span> <span data-ttu-id="e893b-158">請依照下列低耦合的類別：</span><span class="sxs-lookup"><span data-stu-id="e893b-158">Decoupled classes follow:</span></span>

* <span data-ttu-id="e893b-159">[介面隔離原則 」 (ISP)](http://deviq.com/interface-segregation-principle/) ： 類別只取決於它們所使用的組態設定。</span><span class="sxs-lookup"><span data-stu-id="e893b-159">The [Interface Segregation Principle (ISP)](http://deviq.com/interface-segregation-principle/) : Classes depend only on the configuration settings they use.</span></span>
* <span data-ttu-id="e893b-160">[重要性分離](http://deviq.com/separation-of-concerns/)： 設定您的應用程式的不同部分會變成相依或彼此結合。</span><span class="sxs-lookup"><span data-stu-id="e893b-160">[Separation of Concerns](http://deviq.com/separation-of-concerns/) : Settings for different parts of your app are not dependent or coupled with one another.</span></span>

<span data-ttu-id="e893b-161">選項類別必須是抽象的公用的無參數建構函式。</span><span class="sxs-lookup"><span data-stu-id="e893b-161">The options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="e893b-162">例如: </span><span class="sxs-lookup"><span data-stu-id="e893b-162">For example:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Models/MyOptions.cs)]

<a name=options-example></a>

<span data-ttu-id="e893b-163">在下列程式碼，被啟用的 JSON 組態提供者。</span><span class="sxs-lookup"><span data-stu-id="e893b-163">In the following code, the JSON configuration provider is enabled.</span></span> <span data-ttu-id="e893b-164">`MyOptions`類別加入至服務容器，並繫結至組態。</span><span class="sxs-lookup"><span data-stu-id="e893b-164">The `MyOptions` class is added to the service container and bound to configuration.</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Startup.cs?name=snippet1&highlight=8,20-21)]

<span data-ttu-id="e893b-165">下列[控制器](../mvc/controllers/index.md)使用[建構函式相依性插入](xref:fundamentals/dependency-injection#what-is-dependency-injection)上[ `IOptions<TOptions>` ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1)以存取設定：</span><span class="sxs-lookup"><span data-stu-id="e893b-165">The following [controller](../mvc/controllers/index.md)  uses [constructor Dependency Injection](xref:fundamentals/dependency-injection#what-is-dependency-injection) on [`IOptions<TOptions>`](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) to access settings:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="e893b-166">以下列*appsettings.json*檔案：</span><span class="sxs-lookup"><span data-stu-id="e893b-166">With the following *appsettings.json* file:</span></span>

[!code-json[Main](configuration/sample/UsingOptions/appsettings1.json)]

<span data-ttu-id="e893b-167">`HomeController.Index`方法會傳回`option1 = value1_from_json, option2 = 2`。</span><span class="sxs-lookup"><span data-stu-id="e893b-167">The `HomeController.Index` method returns `option1 = value1_from_json, option2 = 2`.</span></span>

<span data-ttu-id="e893b-168">一般應用程式將不會將整個組態繫結至單一選項檔案。</span><span class="sxs-lookup"><span data-stu-id="e893b-168">Typical apps won't bind the entire configuration to a single options file.</span></span> <span data-ttu-id="e893b-169">稍後，我將示範如何使用`GetSection`繫結至一個區段。</span><span class="sxs-lookup"><span data-stu-id="e893b-169">Later on I'll show how to use `GetSection` to bind to a section.</span></span>

<span data-ttu-id="e893b-170">下列程式碼中，第二個`IConfigureOptions<TOptions>`服務新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="e893b-170">In the following code, a second `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="e893b-171">它使用委派來設定的繫結`MyOptions`。</span><span class="sxs-lookup"><span data-stu-id="e893b-171">It uses a delegate to configure the binding with `MyOptions`.</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Startup2.cs?name=snippet1&highlight=9-13)]

<span data-ttu-id="e893b-172">您可以加入多個組態提供者。</span><span class="sxs-lookup"><span data-stu-id="e893b-172">You can add multiple configuration providers.</span></span> <span data-ttu-id="e893b-173">使用 NuGet 封裝中的組態提供者。</span><span class="sxs-lookup"><span data-stu-id="e893b-173">Configuration providers are available in NuGet packages.</span></span> <span data-ttu-id="e893b-174">它們會套用順序註冊它們。</span><span class="sxs-lookup"><span data-stu-id="e893b-174">They are applied in order they are registered.</span></span>

<span data-ttu-id="e893b-175">每次呼叫`Configure<TOptions>`新增`IConfigureOptions<TOptions>`服務的服務容器。</span><span class="sxs-lookup"><span data-stu-id="e893b-175">Each call to `Configure<TOptions>` adds an `IConfigureOptions<TOptions>` service to the service container.</span></span> <span data-ttu-id="e893b-176">在上述範例中，值`Option1`和`Option2`中已指定*appsettings.json* -但的值`Option1`會覆寫設定的委派。</span><span class="sxs-lookup"><span data-stu-id="e893b-176">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json* -- but the value of `Option1` is overridden by the configured delegate.</span></span> 

<span data-ttu-id="e893b-177">啟用多個設定服務時，指定 「 獲勝 」，最後一個組態來源 （設定組態值）。</span><span class="sxs-lookup"><span data-stu-id="e893b-177">When more than one configuration service is enabled, the last configuration source specified "wins" (sets the configuration value).</span></span> <span data-ttu-id="e893b-178">在上述程式碼，`HomeController.Index`方法會傳回`option1 = value1_from_action, option2 = 2`。</span><span class="sxs-lookup"><span data-stu-id="e893b-178">In the preceding code, the `HomeController.Index` method returns `option1 = value1_from_action, option2 = 2`.</span></span>

<span data-ttu-id="e893b-179">當您將選項設定繫結時，繫結至表單的組態機碼的選項類型中的每一個屬性`property[:sub-property:]`。</span><span class="sxs-lookup"><span data-stu-id="e893b-179">When you bind options to configuration, each property in your options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="e893b-180">比方說，`MyOptions.Option1`屬性繫結索引鍵`Option1`，這會從讀取`option1`屬性*appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="e893b-180">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span> <span data-ttu-id="e893b-181">子屬性範例會顯示在本文稍後。</span><span class="sxs-lookup"><span data-stu-id="e893b-181">A sub-property sample is shown later in this article.</span></span>

<span data-ttu-id="e893b-182">下列程式碼，而第三個`IConfigureOptions<TOptions>`服務新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="e893b-182">In the following code, a third `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="e893b-183">它會繫結`MySubOptions`區段`subsection`的*appsettings.json*檔案：</span><span class="sxs-lookup"><span data-stu-id="e893b-183">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Startup3.cs?name=snippet1&highlight=16-17)]

<span data-ttu-id="e893b-184">注意： 這個擴充方法需要`Microsoft.Extensions.Options.ConfigurationExtensions`NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="e893b-184">Note: This extension method requires the `Microsoft.Extensions.Options.ConfigurationExtensions` NuGet package.</span></span>

<span data-ttu-id="e893b-185">使用下列*appsettings.json*檔案：</span><span class="sxs-lookup"><span data-stu-id="e893b-185">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](configuration/sample/UsingOptions/appsettings.json)]

<span data-ttu-id="e893b-186">`MySubOptions`類別：</span><span class="sxs-lookup"><span data-stu-id="e893b-186">The `MySubOptions` class:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="e893b-187">以下列`Controller`:</span><span class="sxs-lookup"><span data-stu-id="e893b-187">With the following `Controller`:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController2.cs?name=snippet1)]

<span data-ttu-id="e893b-188">`subOption1 = subvalue1_from_json, subOption2 = 200`會傳回。</span><span class="sxs-lookup"><span data-stu-id="e893b-188">`subOption1 = subvalue1_from_json, subOption2 = 200` is returned.</span></span>

<span data-ttu-id="e893b-189">您也可以提供選項，檢視模型中的，或插入`IOptions<TOptions>`直接插入檢視：</span><span class="sxs-lookup"><span data-stu-id="e893b-189">You can also supply options in a view model or inject `IOptions<TOptions>` directly into a view:</span></span>

[!code-html[Main](configuration/sample/UsingOptions/Views/Home/Index.cshtml?highlight=3-4,16-17,20-21)]

<a name=in-memory-provider></a>

## <a name="ioptionssnapshot"></a><span data-ttu-id="e893b-190">IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="e893b-190">IOptionsSnapshot</span></span>

<span data-ttu-id="e893b-191">*需要 ASP.NET Core 1.1 或更高版本。*</span><span class="sxs-lookup"><span data-stu-id="e893b-191">*Requires ASP.NET Core 1.1 or higher.*</span></span>

<span data-ttu-id="e893b-192">`IOptionsSnapshot`支援的組態檔變更時重新載入組態資料。</span><span class="sxs-lookup"><span data-stu-id="e893b-192">`IOptionsSnapshot` supports reloading configuration data when the configuration file has changed.</span></span> <span data-ttu-id="e893b-193">它的最少的額外負荷。</span><span class="sxs-lookup"><span data-stu-id="e893b-193">It has minimal overhead.</span></span> <span data-ttu-id="e893b-194">使用`IOptionsSnapshot`與`reloadOnChange: true`，選項會繫結至`IConfiguration`，變更時重新載入。</span><span class="sxs-lookup"><span data-stu-id="e893b-194">Using `IOptionsSnapshot` with `reloadOnChange: true`, the options are bound to `IConfiguration` and reloaded when changed.</span></span>

<span data-ttu-id="e893b-195">下列範例將示範如何為新`IOptionsSnapshot`之後，會建立*config.json*變更。</span><span class="sxs-lookup"><span data-stu-id="e893b-195">The following sample demonstrates how a new `IOptionsSnapshot` is created after *config.json* changes.</span></span> <span data-ttu-id="e893b-196">伺服器的要求會傳回相同時間*config.json*具有**不**變更。</span><span class="sxs-lookup"><span data-stu-id="e893b-196">Requests to the server will return the same time when *config.json* has **not** changed.</span></span> <span data-ttu-id="e893b-197">第一個要求之後*config.json*變更將會顯示新的時間。</span><span class="sxs-lookup"><span data-stu-id="e893b-197">The first request after *config.json* changes will show a new time.</span></span>

[!code-csharp[Main](configuration/sample/IOptionsSnapshot2/Startup.cs?name=snippet1&highlight=1-9,13-18,32,33,52,53)]

<span data-ttu-id="e893b-198">下圖顯示伺服器輸出：</span><span class="sxs-lookup"><span data-stu-id="e893b-198">The following image shows the server output:</span></span>

![瀏覽器影像顯示 「 上次更新日期： 2016 年 11 月 22 下午 4:43"](configuration/_static/first.png)

<span data-ttu-id="e893b-200">重新整理瀏覽器不會變更訊息值或顯示的時間 (當*config.json*未變更)。</span><span class="sxs-lookup"><span data-stu-id="e893b-200">Refreshing the browser doesn't change the message value or time displayed (when *config.json* has not changed).</span></span>

<span data-ttu-id="e893b-201">變更並儲存*config.json*然後重新整理瀏覽器：</span><span class="sxs-lookup"><span data-stu-id="e893b-201">Change and save the  *config.json* and then refresh the browser:</span></span>

![瀏覽器影像顯示 「 上次更新為 e: 2016 年 11 月 22 4:53 PM"](configuration/_static/change.png)

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="e893b-203">記憶體中的提供者和繫結至 POCO 類別</span><span class="sxs-lookup"><span data-stu-id="e893b-203">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="e893b-204">下列範例會示範如何使用記憶體中的提供者，並繫結至類別：</span><span class="sxs-lookup"><span data-stu-id="e893b-204">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

[!code-csharp[Main](configuration/sample/InMemory/Program.cs)]

<span data-ttu-id="e893b-205">組態值會以字串傳回，但繫結啟用了物件的建構。</span><span class="sxs-lookup"><span data-stu-id="e893b-205">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="e893b-206">繫結可讓您擷取 POCO 物件或甚至整個物件圖形。</span><span class="sxs-lookup"><span data-stu-id="e893b-206">Binding allows you to retrieve POCO objects or even entire object graphs.</span></span> <span data-ttu-id="e893b-207">下列範例示範如何將繫結至`MyWindow`，ASP.NET Core MVC 應用程式會使用選項模式：</span><span class="sxs-lookup"><span data-stu-id="e893b-207">The following sample shows how to bind to `MyWindow` and use the options pattern with a ASP.NET Core MVC app:</span></span>

[!code-csharp[Main](configuration/sample/WebConfigBind/MyWindow.cs)]

[!code-json[Main](configuration/sample/WebConfigBind/appsettings.json)]

<span data-ttu-id="e893b-208">繫結中的自訂類別`ConfigureServices`建置主應用程式時：</span><span class="sxs-lookup"><span data-stu-id="e893b-208">Bind the custom class in `ConfigureServices` when building the host:</span></span>

[!code-csharp[Main](configuration/sample/WebConfigBind/Program.cs?name=snippet1&highlight=3-4)]

<span data-ttu-id="e893b-209">顯示從設定`HomeController`:</span><span class="sxs-lookup"><span data-stu-id="e893b-209">Display the settings from the `HomeController`:</span></span>

[!code-csharp[Main](configuration/sample/WebConfigBind/Controllers/HomeController.cs)]

### <a name="getvalue"></a><span data-ttu-id="e893b-210">GetValue</span><span class="sxs-lookup"><span data-stu-id="e893b-210">GetValue</span></span>

<span data-ttu-id="e893b-211">下列範例會示範[GetValue<T> ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_)擴充方法：</span><span class="sxs-lookup"><span data-stu-id="e893b-211">The following sample demonstrates the [GetValue<T>](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) extension method:</span></span>

[!code-csharp[Main](configuration/sample/InMemoryGetValue/Program.cs?highlight=27-29)]

<span data-ttu-id="e893b-212">ConfigurationBinder`GetValue<T>`方法可讓您指定的預設值 (在此範例中為 80)。</span><span class="sxs-lookup"><span data-stu-id="e893b-212">The ConfigurationBinder's `GetValue<T>` method allows you to specify a default value (80 in the sample).</span></span> <span data-ttu-id="e893b-213">`GetValue<T>`適用於簡單的案例並不會連結到整個區段。</span><span class="sxs-lookup"><span data-stu-id="e893b-213">`GetValue<T>` is for simple scenarios and does not bind to entire sections.</span></span> <span data-ttu-id="e893b-214">`GetValue<T>`取得純量值從`GetSection(key).Value`轉換為特定的型別。</span><span class="sxs-lookup"><span data-stu-id="e893b-214">`GetValue<T>` gets scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="binding-to-an-object-graph"></a><span data-ttu-id="e893b-215">繫結至物件圖形</span><span class="sxs-lookup"><span data-stu-id="e893b-215">Binding to an object graph</span></span>

<span data-ttu-id="e893b-216">您可以對每個物件類別中的遞迴繫結。</span><span class="sxs-lookup"><span data-stu-id="e893b-216">You can recursively bind to each object in a class.</span></span> <span data-ttu-id="e893b-217">請考慮下列`AppOptions`類別：</span><span class="sxs-lookup"><span data-stu-id="e893b-217">Consider the following `AppOptions` class:</span></span>

[!code-csharp[Main](configuration/sample/ObjectGraph/AppOptions.cs)]

<span data-ttu-id="e893b-218">下列範例會繫結至`AppOptions`類別：</span><span class="sxs-lookup"><span data-stu-id="e893b-218">The following sample binds to the `AppOptions` class:</span></span>

[!code-csharp[Main](configuration/sample/ObjectGraph/Program.cs?highlight=15-16)]

<span data-ttu-id="e893b-219">**ASP.NET Core 1.1**和更新版本可以使用`Get<T>`，這適用於整個區段。</span><span class="sxs-lookup"><span data-stu-id="e893b-219">**ASP.NET Core 1.1** and higher can use  `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="e893b-220">`Get<T>`可以是比使用更多便利`Bind`。</span><span class="sxs-lookup"><span data-stu-id="e893b-220">`Get<T>` can be more convienent than using `Bind`.</span></span> <span data-ttu-id="e893b-221">下列程式碼示範如何使用`Get<T>`與上面的範例：</span><span class="sxs-lookup"><span data-stu-id="e893b-221">The following code shows how to use `Get<T>` with the sample above:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppOptions>();
```

<span data-ttu-id="e893b-222">使用下列*appsettings.json*檔案：</span><span class="sxs-lookup"><span data-stu-id="e893b-222">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](configuration/sample/ObjectGraph/appsettings.json)]

<span data-ttu-id="e893b-223">程式會顯示`Height 11`。</span><span class="sxs-lookup"><span data-stu-id="e893b-223">The program displays `Height 11`.</span></span>

<span data-ttu-id="e893b-224">下列程式碼可用於單元測試組態：</span><span class="sxs-lookup"><span data-stu-id="e893b-224">The following code can be used to unit test the configuration:</span></span>

```csharp
[Fact]
public void CanBindObjectTree()
{
    var dict = new Dictionary<string, string>
        {
            {"App:Profile:Machine", "Rick"},
            {"App:Connection:Value", "connectionstring"},
            {"App:Window:Height", "11"},
            {"App:Window:Width", "11"}
        };
    var builder = new ConfigurationBuilder();
    builder.AddInMemoryCollection(dict);
    var config = builder.Build();

    var options = new AppOptions();
    config.GetSection("App").Bind(options);

    Assert.Equal("Rick", options.Profile.Machine);
    Assert.Equal(11, options.Window.Height);
    Assert.Equal(11, options.Window.Width);
    Assert.Equal("connectionstring", options.Connection.Value);
}
```

<a name=custom-config-providers></a>

## <a name="basic-sample-of-entity-framework-custom-provider"></a><span data-ttu-id="e893b-225">Entity Framework 自訂提供者的基本範例</span><span class="sxs-lookup"><span data-stu-id="e893b-225">Basic sample of Entity Framework custom provider</span></span>

<span data-ttu-id="e893b-226">在本節中，會建立使用 EF，從資料庫讀取名稱 / 值組的基本組態提供者。</span><span class="sxs-lookup"><span data-stu-id="e893b-226">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span> 

<span data-ttu-id="e893b-227">定義`ConfigurationValue`將組態值儲存在資料庫中的實體：</span><span class="sxs-lookup"><span data-stu-id="e893b-227">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

<span data-ttu-id="e893b-228">新增`ConfigurationContext`來儲存及存取設定的值：</span><span class="sxs-lookup"><span data-stu-id="e893b-228">Add a `ConfigurationContext` to store and access the configured values:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="e893b-229">建立類別，實作[IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span><span class="sxs-lookup"><span data-stu-id="e893b-229">Create an class that implements [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

<span data-ttu-id="e893b-230">建立自訂組態提供者透過繼承自[ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider)。</span><span class="sxs-lookup"><span data-stu-id="e893b-230">Create the custom configuration provider by inheriting from [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).</span></span>  <span data-ttu-id="e893b-231">當它是空的組態提供者會初始化資料庫：</span><span class="sxs-lookup"><span data-stu-id="e893b-231">The configuration provider initializes the database when it's empty:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

<span data-ttu-id="e893b-232">當執行範例時，會顯示資料庫 （「 value_from_ef_1"和"value_from_ef_2"） 中反白顯示的值。</span><span class="sxs-lookup"><span data-stu-id="e893b-232">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="e893b-233">您可以加入`EFConfigSource`擴充方法新增組態來源：</span><span class="sxs-lookup"><span data-stu-id="e893b-233">You can add an `EFConfigSource` extension method for adding the configuration source:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

<span data-ttu-id="e893b-234">下列程式碼示範如何使用自訂`EFConfigProvider`:</span><span class="sxs-lookup"><span data-stu-id="e893b-234">The following code shows how to use the custom `EFConfigProvider`:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

<span data-ttu-id="e893b-235">請注意此範例會將自訂`EFConfigProvider`後的 JSON 提供者，因此資料庫的任何設定會覆寫設定從*appsettings.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="e893b-235">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="e893b-236">使用下列*appsettings.json*檔案：</span><span class="sxs-lookup"><span data-stu-id="e893b-236">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](configuration/sample/CustomConfigurationProvider/appsettings.json)]

<span data-ttu-id="e893b-237">此時會顯示下列對話方塊：</span><span class="sxs-lookup"><span data-stu-id="e893b-237">The following is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="e893b-238">命令列組態提供者</span><span class="sxs-lookup"><span data-stu-id="e893b-238">CommandLine configuration provider</span></span>

<span data-ttu-id="e893b-239">[CommandLine 組態提供者](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider)接收組態在執行階段的命令列引數索引鍵-值組。</span><span class="sxs-lookup"><span data-stu-id="e893b-239">The [CommandLine configuration provider](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) receives command-line argument key-value pairs for configuration at runtime.</span></span>

[<span data-ttu-id="e893b-240">檢視或下載的命令列組態範例</span><span class="sxs-lookup"><span data-stu-id="e893b-240">View or download the CommandLine configuration sample</span></span>](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/sample/CommandLine)

### <a name="setting-up-the-provider"></a><span data-ttu-id="e893b-241">設定提供者</span><span class="sxs-lookup"><span data-stu-id="e893b-241">Setting up the provider</span></span>

# <a name="basic-configurationtabbasicconfiguration"></a>[<span data-ttu-id="e893b-242">基本組態</span><span class="sxs-lookup"><span data-stu-id="e893b-242">Basic Configuration</span></span>](#tab/basicconfiguration)

<span data-ttu-id="e893b-243">若要啟動 命令列組態，請呼叫`AddCommandLine`擴充方法的執行個體上[ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder):</span><span class="sxs-lookup"><span data-stu-id="e893b-243">To activate command-line configuration, call the `AddCommandLine` extension method on an instance of [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder):</span></span>

[!code-csharp[Main](configuration/sample_snapshot/CommandLine/Program.cs?highlight=18,21)]

<span data-ttu-id="e893b-244">執行程式碼，會顯示下列輸出：</span><span class="sxs-lookup"><span data-stu-id="e893b-244">Running the code, the following output is displayed:</span></span>

```console
MachineName: MairaPC
Left: 1980
```

<span data-ttu-id="e893b-245">在命令列上傳遞引數索引鍵-值組會變更值`Profile:MachineName`和`App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="e893b-245">Passing argument key-value pairs on the command line changes the values of `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

<span data-ttu-id="e893b-246">在主控台視窗會顯示：</span><span class="sxs-lookup"><span data-stu-id="e893b-246">The console window displays:</span></span>

```console
MachineName: BartPC
Left: 1979
```

<span data-ttu-id="e893b-247">若要覆寫其他組態提供者所提供的命令列組態與設定，請呼叫`AddCommandLine`上最後一個`ConfigurationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="e893b-247">To override configuration provided by other configuration providers with command-line configuration, call `AddCommandLine` last on `ConfigurationBuilder`:</span></span>

[!code-csharp[Main](configuration/sample_snapshot/CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e893b-248">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e893b-248">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="e893b-249">典型的 ASP.NET Core 2.x 應用程式使用靜態簡便方法`CreateDefaultBuilder`建置主應用程式：</span><span class="sxs-lookup"><span data-stu-id="e893b-249">Typical ASP.NET Core 2.x apps use the static convenience method `CreateDefaultBuilder` to build the host:</span></span>

[!code-csharp[Main](configuration/sample_snapshot/Program.cs?highlight=12)]

<span data-ttu-id="e893b-250">`CreateDefaultBuilder`載入選擇性組態從*appsettings.json*， *appsettings。 {環境}.json*，[使用者密碼](xref:security/app-secrets)(在`Development`環境)，環境變數和命令列引數。</span><span class="sxs-lookup"><span data-stu-id="e893b-250">`CreateDefaultBuilder` loads optional configuration from *appsettings.json*, *appsettings.{Environment}.json*, [user secrets](xref:security/app-secrets) (in the `Development` environment), environment variables, and command-line arguments.</span></span> <span data-ttu-id="e893b-251">最後會呼叫的命令列組態提供者。</span><span class="sxs-lookup"><span data-stu-id="e893b-251">The CommandLine configuration provider is called last.</span></span> <span data-ttu-id="e893b-252">上次呼叫提供者允許在執行階段來覆寫組態集中的其他組態提供者所傳遞的命令列引數之前呼叫。</span><span class="sxs-lookup"><span data-stu-id="e893b-252">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span>

<span data-ttu-id="e893b-253">請注意，針對*appsettings*檔案`reloadOnChange`已啟用。</span><span class="sxs-lookup"><span data-stu-id="e893b-253">Note that for *appsettings* files that `reloadOnChange` is enabled.</span></span> <span data-ttu-id="e893b-254">如果相符的組態值中，命令列引數會覆寫*appsettings*應用程式啟動之後變更檔案。</span><span class="sxs-lookup"><span data-stu-id="e893b-254">Command-line arguments are overridden if a matching configuration value in an *appsettings* file is changed after the app starts.</span></span>

> [!NOTE]
> <span data-ttu-id="e893b-255">做為使用替代`CreateDefaultBuilder`方法，建立主控件使用[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)及手動建置組態[ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder)適用於 ASP.NET Core 2.x。</span><span class="sxs-lookup"><span data-stu-id="e893b-255">As an alternative to using the `CreateDefaultBuilder` method, creating a host using [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) and manually building configuration with [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) is supported in ASP.NET Core 2.x.</span></span> <span data-ttu-id="e893b-256">請參閱 ASP.NET Core 1.x 索引標籤的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="e893b-256">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e893b-257">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e893b-257">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e893b-258">建立[ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder)呼叫`AddCommandLine`方法，以使用命令列組態提供者。</span><span class="sxs-lookup"><span data-stu-id="e893b-258">Create a [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) and call the `AddCommandLine` method to use the CommandLine configuration provider.</span></span> <span data-ttu-id="e893b-259">上次呼叫提供者允許在執行階段來覆寫組態集中的其他組態提供者所傳遞的命令列引數之前呼叫。</span><span class="sxs-lookup"><span data-stu-id="e893b-259">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span> <span data-ttu-id="e893b-260">將組態套用到[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)與`UseConfiguration`方法：</span><span class="sxs-lookup"><span data-stu-id="e893b-260">Apply the configuration to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) with the `UseConfiguration` method:</span></span>

[!code-csharp[Main](configuration/sample_snapshot/CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a><span data-ttu-id="e893b-261">引數</span><span class="sxs-lookup"><span data-stu-id="e893b-261">Arguments</span></span>

<span data-ttu-id="e893b-262">在命令列上傳遞引數必須符合下表顯示兩種格式之一。</span><span class="sxs-lookup"><span data-stu-id="e893b-262">Arguments passed on the command line must conform to one of two formats shown in the following table.</span></span>

| <span data-ttu-id="e893b-263">引數格式</span><span class="sxs-lookup"><span data-stu-id="e893b-263">Argument format</span></span>                                                     | <span data-ttu-id="e893b-264">範例</span><span class="sxs-lookup"><span data-stu-id="e893b-264">Example</span></span>        |
| ------------------------------------------------------------------- | :------------: |
| <span data-ttu-id="e893b-265">單一引數： 等號分隔的索引鍵-值組 (`=`)</span><span class="sxs-lookup"><span data-stu-id="e893b-265">Single argument: a key-value pair separated by an equals sign (`=`)</span></span> | `key1=value`   |
| <span data-ttu-id="e893b-266">兩個引數的順序： 以空格分隔的索引鍵-值配對</span><span class="sxs-lookup"><span data-stu-id="e893b-266">Sequence of two arguments: a key-value pair separated by a space</span></span>    | `/key1 value1` |

<span data-ttu-id="e893b-267">**單一引數**</span><span class="sxs-lookup"><span data-stu-id="e893b-267">**Single argument**</span></span>

<span data-ttu-id="e893b-268">值必須遵照等號 (`=`)。</span><span class="sxs-lookup"><span data-stu-id="e893b-268">The value must follow an equals sign (`=`).</span></span> <span data-ttu-id="e893b-269">這個值可以是 null (例如， `mykey=`)。</span><span class="sxs-lookup"><span data-stu-id="e893b-269">The value can be null (for example, `mykey=`).</span></span>

<span data-ttu-id="e893b-270">索引鍵可能會有前置詞。</span><span class="sxs-lookup"><span data-stu-id="e893b-270">The key may have a prefix.</span></span>

| <span data-ttu-id="e893b-271">索引鍵前置詞</span><span class="sxs-lookup"><span data-stu-id="e893b-271">Key prefix</span></span>               | <span data-ttu-id="e893b-272">範例</span><span class="sxs-lookup"><span data-stu-id="e893b-272">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="e893b-273">沒有前置詞</span><span class="sxs-lookup"><span data-stu-id="e893b-273">No prefix</span></span>                | `key1=value1`   |
| <span data-ttu-id="e893b-274">單一虛線 (`-`) &#8224;</span><span class="sxs-lookup"><span data-stu-id="e893b-274">Single dash (`-`)&#8224;</span></span> | `-key2=value2`  |
| <span data-ttu-id="e893b-275">兩個破折號 (`--`)</span><span class="sxs-lookup"><span data-stu-id="e893b-275">Two dashes (`--`)</span></span>        | `--key3=value3` |
| <span data-ttu-id="e893b-276">正斜線 (`/`)</span><span class="sxs-lookup"><span data-stu-id="e893b-276">Forward slash (`/`)</span></span>      | `/key4=value4`  |

<span data-ttu-id="e893b-277">&#8224;具有單一虛線前置詞的索引鍵 (`-`) 中必須提供[切換對應](#switch-mappings)，如下所述。</span><span class="sxs-lookup"><span data-stu-id="e893b-277">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="e893b-278">範例命令：</span><span class="sxs-lookup"><span data-stu-id="e893b-278">Example command:</span></span>

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

<span data-ttu-id="e893b-279">注意： 如果`-key1`不存在於[切換對應](#switch-mappings)提供給組態提供者`FormatException`就會擲回。</span><span class="sxs-lookup"><span data-stu-id="e893b-279">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

<span data-ttu-id="e893b-280">**兩個引數的順序**</span><span class="sxs-lookup"><span data-stu-id="e893b-280">**Sequence of two arguments**</span></span>

<span data-ttu-id="e893b-281">值不能是 null，而且必須遵循以空格分隔的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="e893b-281">The value can't be null and must follow the key separated by a space.</span></span>

<span data-ttu-id="e893b-282">機碼必須具有前置詞。</span><span class="sxs-lookup"><span data-stu-id="e893b-282">The key must have a prefix.</span></span>

| <span data-ttu-id="e893b-283">索引鍵前置詞</span><span class="sxs-lookup"><span data-stu-id="e893b-283">Key prefix</span></span>               | <span data-ttu-id="e893b-284">範例</span><span class="sxs-lookup"><span data-stu-id="e893b-284">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="e893b-285">單一虛線 (`-`) &#8224;</span><span class="sxs-lookup"><span data-stu-id="e893b-285">Single dash (`-`)&#8224;</span></span> | `-key1 value1`  |
| <span data-ttu-id="e893b-286">兩個破折號 (`--`)</span><span class="sxs-lookup"><span data-stu-id="e893b-286">Two dashes (`--`)</span></span>        | `--key2 value2` |
| <span data-ttu-id="e893b-287">正斜線 (`/`)</span><span class="sxs-lookup"><span data-stu-id="e893b-287">Forward slash (`/`)</span></span>      | `/key3 value3`  |

<span data-ttu-id="e893b-288">&#8224;具有單一虛線前置詞的索引鍵 (`-`) 中必須提供[切換對應](#switch-mappings)，如下所述。</span><span class="sxs-lookup"><span data-stu-id="e893b-288">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="e893b-289">範例命令：</span><span class="sxs-lookup"><span data-stu-id="e893b-289">Example command:</span></span>

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

<span data-ttu-id="e893b-290">注意： 如果`-key1`不存在於[切換對應](#switch-mappings)提供給組態提供者`FormatException`就會擲回。</span><span class="sxs-lookup"><span data-stu-id="e893b-290">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

### <a name="duplicate-keys"></a><span data-ttu-id="e893b-291">重複的索引鍵</span><span class="sxs-lookup"><span data-stu-id="e893b-291">Duplicate keys</span></span>

<span data-ttu-id="e893b-292">如果未提供重複的索引鍵時，會使用最後一個索引鍵-值配對。</span><span class="sxs-lookup"><span data-stu-id="e893b-292">If duplicate keys are provided, the last key-value pair is used.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="e893b-293">參數對應</span><span class="sxs-lookup"><span data-stu-id="e893b-293">Switch mappings</span></span>

<span data-ttu-id="e893b-294">當手動建置組態`ConfigurationBuilder`，您可以選擇性地提供參數對應字典`AddCommandLine`方法。</span><span class="sxs-lookup"><span data-stu-id="e893b-294">When manually building configuration with `ConfigurationBuilder`, you can optionally provide a switch mappings dictionary to the `AddCommandLine` method.</span></span> <span data-ttu-id="e893b-295">參數對應可讓您提供的索引鍵名稱更換邏輯。</span><span class="sxs-lookup"><span data-stu-id="e893b-295">Switch mappings allow you to provide key name replacement logic.</span></span>

<span data-ttu-id="e893b-296">使用交換器對應字典時，會檢查字典索引鍵符合命令列引數所提供的金鑰。</span><span class="sxs-lookup"><span data-stu-id="e893b-296">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="e893b-297">如果在字典中找到的命令列的索引鍵，該字典值 （索引鍵取代） 會傳遞回來進行設定。</span><span class="sxs-lookup"><span data-stu-id="e893b-297">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the configuration.</span></span> <span data-ttu-id="e893b-298">參數對應無須任何命令列的索引鍵，加上單一虛線 (`-`)。</span><span class="sxs-lookup"><span data-stu-id="e893b-298">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="e893b-299">切換對應字典索引鍵規則：</span><span class="sxs-lookup"><span data-stu-id="e893b-299">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="e893b-300">參數必須以破折號開頭 (`-`) 或雙虛線 (`--`)。</span><span class="sxs-lookup"><span data-stu-id="e893b-300">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="e893b-301">參數對應字典不能包含重複的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="e893b-301">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="e893b-302">在下列範例中，`GetSwitchMappings`方法可讓您使用單一的虛線的命令列引數 (`-`) 索引鍵前置詞，並避免開頭的子機碼前置詞。</span><span class="sxs-lookup"><span data-stu-id="e893b-302">In the following example, the `GetSwitchMappings` method allows your command-line arguments to use a single dash (`-`) key prefix and avoid leading subkey prefixes.</span></span>

[!code-csharp[Main](configuration/sample/CommandLine/Program.cs?highlight=10-19,32)]

<span data-ttu-id="e893b-303">不需要提供命令列引數，提供給字典`AddInMemoryCollection`設定組態值。</span><span class="sxs-lookup"><span data-stu-id="e893b-303">Without providing command-line arguments, the dictionary provided to `AddInMemoryCollection` sets the configuration values.</span></span> <span data-ttu-id="e893b-304">執行應用程式使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="e893b-304">Run the app with the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="e893b-305">在主控台視窗會顯示：</span><span class="sxs-lookup"><span data-stu-id="e893b-305">The console window displays:</span></span>

```console
MachineName: RickPC
Left: 1980
```

<span data-ttu-id="e893b-306">使用下列組態設定中傳遞：</span><span class="sxs-lookup"><span data-stu-id="e893b-306">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

<span data-ttu-id="e893b-307">在主控台視窗會顯示：</span><span class="sxs-lookup"><span data-stu-id="e893b-307">The console window displays:</span></span>

```console
MachineName: DahliaPC
Left: 1984
```

<span data-ttu-id="e893b-308">建立參數對應字典之後，它會包含下表中所顯示的資料。</span><span class="sxs-lookup"><span data-stu-id="e893b-308">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="e893b-309">Key</span><span class="sxs-lookup"><span data-stu-id="e893b-309">Key</span></span>            | <span data-ttu-id="e893b-310">值</span><span class="sxs-lookup"><span data-stu-id="e893b-310">Value</span></span>                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

<span data-ttu-id="e893b-311">若要示範使用字典的索引鍵切換，請執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="e893b-311">To demonstrate key switching using the dictionary, run the following command:</span></span>

```console
dotnet run -MachineName=ChadPC -Left=1988
```

<span data-ttu-id="e893b-312">交換的命令列的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="e893b-312">The command-line keys are swapped.</span></span> <span data-ttu-id="e893b-313">主控台視窗會顯示組態值`Profile:MachineName`和`App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="e893b-313">The console window displays the configuration values for `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
MachineName: ChadPC
Left: 1988
```

## <a name="the-webconfig-file"></a><span data-ttu-id="e893b-314">Web.config 檔案</span><span class="sxs-lookup"><span data-stu-id="e893b-314">The web.config file</span></span>

<span data-ttu-id="e893b-315">A *web.config*檔案時，需要您裝載於 IIS 或 IIS Express 應用程式。</span><span class="sxs-lookup"><span data-stu-id="e893b-315">A *web.config* file is required when you host the app in IIS or IIS-Express.</span></span> <span data-ttu-id="e893b-316">*web.config* AspNetCoreModule IIS 啟動您的應用程式中開啟。</span><span class="sxs-lookup"><span data-stu-id="e893b-316">*web.config* turns on the AspNetCoreModule in IIS to launch your app.</span></span> <span data-ttu-id="e893b-317">中的設定*web.config*啟用 AspNetCoreModule IIS 啟動應用程式，並設定其他 IIS 設定和模組中的。</span><span class="sxs-lookup"><span data-stu-id="e893b-317">Settings in *web.config* enable the AspNetCoreModule in IIS to launch your app and configure other IIS settings and modules.</span></span> <span data-ttu-id="e893b-318">如果您使用 Visual Studio，刪除*web.config*，Visual Studio 會建立一個新。</span><span class="sxs-lookup"><span data-stu-id="e893b-318">If you are using Visual Studio and delete *web.config*, Visual Studio will create a new one.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="e893b-319">其他備註</span><span class="sxs-lookup"><span data-stu-id="e893b-319">Additional notes</span></span>

* <span data-ttu-id="e893b-320">相依性插入 (DI) 未設定為止之後`ConfigureServices`叫用。</span><span class="sxs-lookup"><span data-stu-id="e893b-320">Dependency Injection (DI) is not set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="e893b-321">組態系統不 DI 注意。</span><span class="sxs-lookup"><span data-stu-id="e893b-321">The configuration system is not DI aware.</span></span>
* <span data-ttu-id="e893b-322">`IConfiguration`有兩個特製化：</span><span class="sxs-lookup"><span data-stu-id="e893b-322">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="e893b-323">`IConfigurationRoot`用於根節點。</span><span class="sxs-lookup"><span data-stu-id="e893b-323">`IConfigurationRoot`  Used for the root node.</span></span> <span data-ttu-id="e893b-324">可以觸發重新載入。</span><span class="sxs-lookup"><span data-stu-id="e893b-324">Can trigger a reload.</span></span>
  * <span data-ttu-id="e893b-325">`IConfigurationSection`代表組態值的區段。</span><span class="sxs-lookup"><span data-stu-id="e893b-325">`IConfigurationSection`  Represents a section of configuration values.</span></span> <span data-ttu-id="e893b-326">`GetSection`和`GetChildren`方法會傳回`IConfigurationSection`。</span><span class="sxs-lookup"><span data-stu-id="e893b-326">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e893b-327">其他資源</span><span class="sxs-lookup"><span data-stu-id="e893b-327">Additional resources</span></span>

* [<span data-ttu-id="e893b-328">使用多個環境</span><span class="sxs-lookup"><span data-stu-id="e893b-328">Working with Multiple Environments</span></span>](environments.md)
* [<span data-ttu-id="e893b-329">在開發期間安全儲存應用程式密碼</span><span class="sxs-lookup"><span data-stu-id="e893b-329">Safe storage of app secrets during development</span></span>](../security/app-secrets.md)
* [<span data-ttu-id="e893b-330">在 ASP.NET Core 中裝載</span><span class="sxs-lookup"><span data-stu-id="e893b-330">Hosting in ASP.NET Core</span></span>](xref:fundamentals/hosting)
* [<span data-ttu-id="e893b-331">相依性插入</span><span class="sxs-lookup"><span data-stu-id="e893b-331">Dependency Injection</span></span>](dependency-injection.md)
* [<span data-ttu-id="e893b-332">Azure Key Vault 組態提供者</span><span class="sxs-lookup"><span data-stu-id="e893b-332">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
