---
title: "在 ASP.NET Core 組態"
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
ms.openlocfilehash: dae7ac6e377d2c17bc8f86e5b6da98107366cc73
ms.sourcegitcommit: 418e6aa4ab79474ecc4d0a6af573a3759b113fe4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/05/2017
---
<a name=fundamentals-configuration></a>

  # <a name="configuration-in-aspnet-core"></a><span data-ttu-id="9c121-104">在 ASP.NET Core 組態</span><span class="sxs-lookup"><span data-stu-id="9c121-104">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="9c121-105">[Rick Anderson](https://twitter.com/RickAndMSFT)，[標記 Michaelis](http://intellitect.com/author/mark-michaelis/)， [Steve Smith](http://ardalis.com)，和[奧 Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="9c121-105">[Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](http://ardalis.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="9c121-106">組態 API 提供一種設定的名稱 / 值組清單為基礎的應用程式。</span><span class="sxs-lookup"><span data-stu-id="9c121-106">The Configuration API provides a way of configuring an app based on a list of name-value pairs.</span></span> <span data-ttu-id="9c121-107">在執行階段從多個來源讀取組態。</span><span class="sxs-lookup"><span data-stu-id="9c121-107">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="9c121-108">名稱 / 值組可以分為多層級的階層。</span><span class="sxs-lookup"><span data-stu-id="9c121-108">The name-value pairs can be grouped into a multi-level hierarchy.</span></span> <span data-ttu-id="9c121-109">有的組態提供者：</span><span class="sxs-lookup"><span data-stu-id="9c121-109">There are configuration providers for:</span></span>

* <span data-ttu-id="9c121-110">檔案格式 （INI、 JSON 和 XML）</span><span class="sxs-lookup"><span data-stu-id="9c121-110">File formats (INI, JSON, and XML)</span></span>
* <span data-ttu-id="9c121-111">命令列引數</span><span class="sxs-lookup"><span data-stu-id="9c121-111">Command-line arguments</span></span>
* <span data-ttu-id="9c121-112">環境變數</span><span class="sxs-lookup"><span data-stu-id="9c121-112">Environment variables</span></span>
* <span data-ttu-id="9c121-113">記憶體中的.NET 物件</span><span class="sxs-lookup"><span data-stu-id="9c121-113">In-memory .NET objects</span></span>
* <span data-ttu-id="9c121-114">加密的使用者存放區</span><span class="sxs-lookup"><span data-stu-id="9c121-114">An encrypted user store</span></span>
* [<span data-ttu-id="9c121-115">Azure 金鑰保存庫</span><span class="sxs-lookup"><span data-stu-id="9c121-115">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
* <span data-ttu-id="9c121-116">自訂提供者，供您安裝或建立</span><span class="sxs-lookup"><span data-stu-id="9c121-116">Custom providers, which you install or create</span></span>

<span data-ttu-id="9c121-117">每個組態值將對應至字串索引鍵。</span><span class="sxs-lookup"><span data-stu-id="9c121-117">Each configuration value maps to a string key.</span></span> <span data-ttu-id="9c121-118">沒有要還原序列化到自訂設定的內建繫結支援[POCO](https://en.wikipedia.org/wiki/Plain_Old_CLR_Object)物件 （具有屬性的簡單.NET 類別）。</span><span class="sxs-lookup"><span data-stu-id="9c121-118">There's built-in binding support to deserialize settings into a custom [POCO](https://en.wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

[<span data-ttu-id="9c121-119">檢視或下載範例程式碼</span><span class="sxs-lookup"><span data-stu-id="9c121-119">View or download sample code</span></span>](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/sample)

## <a name="simple-configuration"></a><span data-ttu-id="9c121-120">簡單的組態</span><span class="sxs-lookup"><span data-stu-id="9c121-120">Simple configuration</span></span>

<span data-ttu-id="9c121-121">下列主控台應用程式會使用 JSON 組態提供者：</span><span class="sxs-lookup"><span data-stu-id="9c121-121">The following console app uses the JSON configuration provider:</span></span>

<span data-ttu-id="9c121-122">[!code-csharp[Main](configuration/sample/src/ConfigJson/Program.cs)]</span><span class="sxs-lookup"><span data-stu-id="9c121-122">[!code-csharp[Main](configuration/sample/src/ConfigJson/Program.cs)]</span></span>

<span data-ttu-id="9c121-123">應用程式讀取，並顯示下列組態設定：</span><span class="sxs-lookup"><span data-stu-id="9c121-123">The app reads and displays the following configuration settings:</span></span>

<span data-ttu-id="9c121-124">[!code-json[Main](configuration/sample/src/ConfigJson/appsettings.json)]</span><span class="sxs-lookup"><span data-stu-id="9c121-124">[!code-json[Main](configuration/sample/src/ConfigJson/appsettings.json)]</span></span>

<span data-ttu-id="9c121-125">設定包含在其中的節點以冒號分隔的名稱 / 值組的階層式清單。</span><span class="sxs-lookup"><span data-stu-id="9c121-125">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon.</span></span> <span data-ttu-id="9c121-126">若要擷取值，存取`Configuration`與對應的項目索引鍵的索引子：</span><span class="sxs-lookup"><span data-stu-id="9c121-126">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

```csharp
Console.WriteLine($"option1 = {Configuration["subsection:suboption1"]}");
```

<span data-ttu-id="9c121-127">若要使用 JSON 格式設定來源中的陣列，使用陣列索引一部分的冒號分隔的字串。</span><span class="sxs-lookup"><span data-stu-id="9c121-127">To work with arrays in JSON-formatted configuration sources, use a array index as part of the colon-separated string.</span></span> <span data-ttu-id="9c121-128">下列範例會取得在上述第一個項目的名稱`wizards`陣列：</span><span class="sxs-lookup"><span data-stu-id="9c121-128">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}, ");
```

<span data-ttu-id="9c121-129">寫入至內建的名稱 / 值組中`Configuration`提供者是**不**保存，不過，您可以建立將值儲存的自訂提供者。</span><span class="sxs-lookup"><span data-stu-id="9c121-129">Name-value pairs written to the built in `Configuration` providers are **not** persisted, however, you can create a custom provider that saves values.</span></span> <span data-ttu-id="9c121-130">請參閱[自訂組態提供者](xref:fundamentals/configuration#custom-config-providers)。</span><span class="sxs-lookup"><span data-stu-id="9c121-130">See [custom configuration provider](xref:fundamentals/configuration#custom-config-providers).</span></span>

<span data-ttu-id="9c121-131">上述範例會使用設定索引子讀取的值。</span><span class="sxs-lookup"><span data-stu-id="9c121-131">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="9c121-132">外部存取組態`Startup`，使用[選項模式](xref:fundamentals/configuration#options-config-objects)。</span><span class="sxs-lookup"><span data-stu-id="9c121-132">To access configuration outside of `Startup`, use the [options pattern](xref:fundamentals/configuration#options-config-objects).</span></span> <span data-ttu-id="9c121-133">*選項模式*本文稍後所示。</span><span class="sxs-lookup"><span data-stu-id="9c121-133">The *options pattern* is shown later in this article.</span></span>

<span data-ttu-id="9c121-134">這是通常會有不同的環境，例如開發、 測試和實際執行不同的組態設定。</span><span class="sxs-lookup"><span data-stu-id="9c121-134">It's typical to have different configuration settings for different environments, for example, development, test and production.</span></span> <span data-ttu-id="9c121-135">下列反白顯示的程式碼會將兩個組態提供者加入至三個來源：</span><span class="sxs-lookup"><span data-stu-id="9c121-135">The following highlighted code adds two configuration providers to three sources:</span></span>

1. <span data-ttu-id="9c121-136">JSON 提供者，讀取*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="9c121-136">JSON provider, reading *appsettings.json*</span></span>
2. <span data-ttu-id="9c121-137">JSON 提供者，讀取*appsettings。\<EnvironmentName >.json*</span><span class="sxs-lookup"><span data-stu-id="9c121-137">JSON provider, reading *appsettings.\<EnvironmentName>.json*</span></span>
3. <span data-ttu-id="9c121-138">環境變數提供者</span><span class="sxs-lookup"><span data-stu-id="9c121-138">Environment variables provider</span></span>

<span data-ttu-id="9c121-139">[!code-csharp[Main](configuration/sample/src/WebConfigBind/Startup.cs?name=snippet2&highlight=7-9)]</span><span class="sxs-lookup"><span data-stu-id="9c121-139">[!code-csharp[Main](configuration/sample/src/WebConfigBind/Startup.cs?name=snippet2&highlight=7-9)]</span></span>

<span data-ttu-id="9c121-140">請參閱[AddJsonFile](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.jsonconfigurationextensions)取得參數的說明。</span><span class="sxs-lookup"><span data-stu-id="9c121-140">See [AddJsonFile](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="9c121-141">`reloadOnChange`僅適用於 ASP.NET Core 1.1 （含） 以上。</span><span class="sxs-lookup"><span data-stu-id="9c121-141">`reloadOnChange` is only supported in ASP.NET Core 1.1 and higher.</span></span> 

<span data-ttu-id="9c121-142">設定來源會讀取這些指定的順序。</span><span class="sxs-lookup"><span data-stu-id="9c121-142">Configuration sources are read in the order they are specified.</span></span> <span data-ttu-id="9c121-143">上述程式碼，是上次讀取環境變數。</span><span class="sxs-lookup"><span data-stu-id="9c121-143">In the code above, the environment variables are read last.</span></span> <span data-ttu-id="9c121-144">設定透過環境的所有組態值會都取代中的兩個先前提供者設定。</span><span class="sxs-lookup"><span data-stu-id="9c121-144">Any configuration values set through the environment would replace those set in the two previous providers.</span></span>

<span data-ttu-id="9c121-145">環境通常設定為其中一個`Development`， `Staging`，或`Production`。</span><span class="sxs-lookup"><span data-stu-id="9c121-145">The environment is typically set to one of `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="9c121-146">請參閱[使用多個環境](environments.md)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="9c121-146">See [Working with Multiple Environments](environments.md) for more information.</span></span>

<span data-ttu-id="9c121-147">組態的考量：</span><span class="sxs-lookup"><span data-stu-id="9c121-147">Configuration considerations:</span></span>

* <span data-ttu-id="9c121-148">`IOptionsSnapshot`當監視變更時，就可以載入組態資料。</span><span class="sxs-lookup"><span data-stu-id="9c121-148">`IOptionsSnapshot` can reload configuration data when it changes.</span></span> <span data-ttu-id="9c121-149">使用`IOptionsSnapshot`如果您要重新載入組態資料。</span><span class="sxs-lookup"><span data-stu-id="9c121-149">Use `IOptionsSnapshot` if you need to reload configuration data.</span></span>  <span data-ttu-id="9c121-150">請參閱[IOptionsSnapshot](#ioptionssnapshot)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="9c121-150">See [IOptionsSnapshot](#ioptionssnapshot) for more information.</span></span>
* <span data-ttu-id="9c121-151">設定索引鍵是不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="9c121-151">Configuration keys are case insensitive.</span></span>
* <span data-ttu-id="9c121-152">最佳作法是最後指定環境變數，讓本機的環境可以覆寫在已部署的組態檔中設定的任何項目。</span><span class="sxs-lookup"><span data-stu-id="9c121-152">A best practice is to specify environment variables last, so that the local environment can override anything set in deployed configuration files.</span></span>
* <span data-ttu-id="9c121-153">**永遠不會**將密碼或其他敏感性資料儲存在組態提供者程式碼或純文字設定檔案中。</span><span class="sxs-lookup"><span data-stu-id="9c121-153">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="9c121-154">不在您開發使用生產機密資料或測試環境。</span><span class="sxs-lookup"><span data-stu-id="9c121-154">Don't use production secrets in your development or test environments.</span></span> <span data-ttu-id="9c121-155">相反地，指定外部專案樹狀結構的機密資料，讓它們無法意外認可到您的儲存機制。</span><span class="sxs-lookup"><span data-stu-id="9c121-155">Instead, specify secrets outside the project tree, so they cannot be accidentally committed into your repository.</span></span> <span data-ttu-id="9c121-156">深入了解[使用多個環境](environments.md)和管理[安全存放應用程式密碼，在開發期間](../security/app-secrets.md)。</span><span class="sxs-lookup"><span data-stu-id="9c121-156">Learn more about [Working with Multiple Environments](environments.md) and managing [safe storage of app secrets during development](../security/app-secrets.md).</span></span>
* <span data-ttu-id="9c121-157">如果`:`不能在您的系統中，以使用環境變數中，取代`:`與`__`（雙底線）。</span><span class="sxs-lookup"><span data-stu-id="9c121-157">If `:` cannot be used in environment variables in your system,  replace `:`  with `__` (double underscore).</span></span>

<a name=options-config-objects></a>

## <a name="using-options-and-configuration-objects"></a><span data-ttu-id="9c121-158">使用選項和設定物件</span><span class="sxs-lookup"><span data-stu-id="9c121-158">Using Options and configuration objects</span></span>

<span data-ttu-id="9c121-159">選項模式會使用自訂的選項類別來代表一組相關的設定。</span><span class="sxs-lookup"><span data-stu-id="9c121-159">The options pattern uses custom options classes to represent a group of related settings.</span></span> <span data-ttu-id="9c121-160">我們建議您在應用程式中建立低耦合的每個功能類別。</span><span class="sxs-lookup"><span data-stu-id="9c121-160">We recommended that you create decoupled classes for each feature within your app.</span></span> <span data-ttu-id="9c121-161">請依照下列低耦合的類別：</span><span class="sxs-lookup"><span data-stu-id="9c121-161">Decoupled classes follow:</span></span>

* <span data-ttu-id="9c121-162">[介面隔離原則 」 (ISP)](http://deviq.com/interface-segregation-principle/) ： 類別只取決於它們所使用的組態設定。</span><span class="sxs-lookup"><span data-stu-id="9c121-162">The [Interface Segregation Principle (ISP)](http://deviq.com/interface-segregation-principle/) : Classes depend only on the configuration settings they use.</span></span>
* <span data-ttu-id="9c121-163">[重要性分離](http://deviq.com/separation-of-concerns/)： 設定您的應用程式的不同部分會變成相依或彼此結合。</span><span class="sxs-lookup"><span data-stu-id="9c121-163">[Separation of Concerns](http://deviq.com/separation-of-concerns/) : Settings for different parts of your app are not dependent or coupled with one another.</span></span>

<span data-ttu-id="9c121-164">選項類別必須是抽象的公用的無參數建構函式。</span><span class="sxs-lookup"><span data-stu-id="9c121-164">The options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="9c121-165">例如: </span><span class="sxs-lookup"><span data-stu-id="9c121-165">For example:</span></span>

<span data-ttu-id="9c121-166">[!code-csharp[Main](configuration/sample/src/UsingOptions/Models/MyOptions.cs)]</span><span class="sxs-lookup"><span data-stu-id="9c121-166">[!code-csharp[Main](configuration/sample/src/UsingOptions/Models/MyOptions.cs)]</span></span>

<a name=options-example></a>

<span data-ttu-id="9c121-167">在下列程式碼，被啟用的 JSON 組態提供者。</span><span class="sxs-lookup"><span data-stu-id="9c121-167">In the following code, the JSON configuration provider is enabled.</span></span> <span data-ttu-id="9c121-168">`MyOptions`類別加入至服務容器，並繫結至組態。</span><span class="sxs-lookup"><span data-stu-id="9c121-168">The `MyOptions` class is added to the service container and bound to configuration.</span></span>

<span data-ttu-id="9c121-169">[!code-csharp[Main](configuration/sample/src/UsingOptions/Startup.cs?name=snippet1&highlight=8,20-22)]</span><span class="sxs-lookup"><span data-stu-id="9c121-169">[!code-csharp[Main](configuration/sample/src/UsingOptions/Startup.cs?name=snippet1&highlight=8,20-22)]</span></span>

<span data-ttu-id="9c121-170">下列[控制器](../mvc/controllers/index.md)使用[建構函式相依性插入](xref:fundamentals/dependency-injection#what-is-dependency-injection)上[ `IOptions<TOptions>` ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1)以存取設定：</span><span class="sxs-lookup"><span data-stu-id="9c121-170">The following [controller](../mvc/controllers/index.md)  uses [constructor Dependency Injection](xref:fundamentals/dependency-injection#what-is-dependency-injection) on [`IOptions<TOptions>`](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) to access settings:</span></span>

<span data-ttu-id="9c121-171">[!code-csharp[Main](configuration/sample/src/UsingOptions/Controllers/HomeController.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="9c121-171">[!code-csharp[Main](configuration/sample/src/UsingOptions/Controllers/HomeController.cs?name=snippet1)]</span></span>

<span data-ttu-id="9c121-172">以下列*appsettings.json*檔案：</span><span class="sxs-lookup"><span data-stu-id="9c121-172">With the following *appsettings.json* file:</span></span>

<span data-ttu-id="9c121-173">[!code-json[Main](configuration/sample/src/UsingOptions/appsettings1.json)]</span><span class="sxs-lookup"><span data-stu-id="9c121-173">[!code-json[Main](configuration/sample/src/UsingOptions/appsettings1.json)]</span></span>

<span data-ttu-id="9c121-174">`HomeController.Index`方法會傳回`option1 = value1_from_json, option2 = 2`。</span><span class="sxs-lookup"><span data-stu-id="9c121-174">The `HomeController.Index` method returns `option1 = value1_from_json, option2 = 2`.</span></span>

<span data-ttu-id="9c121-175">一般應用程式將不會將整個組態繫結至單一選項檔案。</span><span class="sxs-lookup"><span data-stu-id="9c121-175">Typical apps won't bind the entire configuration to a single options file.</span></span> <span data-ttu-id="9c121-176">稍後，我將示範如何使用`GetSection`繫結至一個區段。</span><span class="sxs-lookup"><span data-stu-id="9c121-176">Later on I'll show how to use `GetSection` to bind to a section.</span></span>

<span data-ttu-id="9c121-177">下列程式碼中，第二個`IConfigureOptions<TOptions>`服務新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="9c121-177">In the following code, a second `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="9c121-178">它使用委派來設定的繫結`MyOptions`。</span><span class="sxs-lookup"><span data-stu-id="9c121-178">It uses a delegate to configure the binding with `MyOptions`.</span></span>

<span data-ttu-id="9c121-179">[!code-csharp[Main](configuration/sample/src/UsingOptions/Startup2.cs?name=snippet1&highlight=9-13)]</span><span class="sxs-lookup"><span data-stu-id="9c121-179">[!code-csharp[Main](configuration/sample/src/UsingOptions/Startup2.cs?name=snippet1&highlight=9-13)]</span></span>

<span data-ttu-id="9c121-180">您可以加入多個組態提供者。</span><span class="sxs-lookup"><span data-stu-id="9c121-180">You can add multiple configuration providers.</span></span> <span data-ttu-id="9c121-181">使用 NuGet 封裝中的組態提供者。</span><span class="sxs-lookup"><span data-stu-id="9c121-181">Configuration providers are available in NuGet packages.</span></span> <span data-ttu-id="9c121-182">它們會套用順序註冊它們。</span><span class="sxs-lookup"><span data-stu-id="9c121-182">They are applied in order they are registered.</span></span>

<span data-ttu-id="9c121-183">每次呼叫`Configure<TOptions>`新增`IConfigureOptions<TOptions>`服務的服務容器。</span><span class="sxs-lookup"><span data-stu-id="9c121-183">Each call to `Configure<TOptions>` adds an `IConfigureOptions<TOptions>` service to the service container.</span></span> <span data-ttu-id="9c121-184">在上述範例中，值`Option1`和`Option2`中已指定*appsettings.json* -但的值`Option1`會覆寫設定的委派。</span><span class="sxs-lookup"><span data-stu-id="9c121-184">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json* -- but the value of `Option1` is overridden by the configured delegate.</span></span> 

<span data-ttu-id="9c121-185">啟用多個設定服務時，指定 「 獲勝 」，最後一個組態來源 （設定組態值）。</span><span class="sxs-lookup"><span data-stu-id="9c121-185">When more than one configuration service is enabled, the last configuration source specified "wins" (sets the configuration value).</span></span> <span data-ttu-id="9c121-186">在上述程式碼，`HomeController.Index`方法會傳回`option1 = value1_from_action, option2 = 2`。</span><span class="sxs-lookup"><span data-stu-id="9c121-186">In the preceding code, the `HomeController.Index` method returns `option1 = value1_from_action, option2 = 2`.</span></span>

<span data-ttu-id="9c121-187">當您將選項設定繫結時，繫結至表單的組態機碼的選項類型中的每一個屬性`property[:sub-property:]`。</span><span class="sxs-lookup"><span data-stu-id="9c121-187">When you bind options to configuration, each property in your options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="9c121-188">比方說，`MyOptions.Option1`屬性繫結索引鍵`Option1`，這會從讀取`option1`屬性*appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="9c121-188">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span> <span data-ttu-id="9c121-189">子屬性範例會顯示在本文稍後。</span><span class="sxs-lookup"><span data-stu-id="9c121-189">A sub-property sample is shown later in this article.</span></span>

<span data-ttu-id="9c121-190">下列程式碼，而第三個`IConfigureOptions<TOptions>`服務新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="9c121-190">In the following code, a third `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="9c121-191">它會繫結`MySubOptions`區段`subsection`的*appsettings.json*檔案：</span><span class="sxs-lookup"><span data-stu-id="9c121-191">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

<span data-ttu-id="9c121-192">[!code-csharp[Main](configuration/sample/src/UsingOptions/Startup3.cs?name=snippet1&highlight=16-17)]</span><span class="sxs-lookup"><span data-stu-id="9c121-192">[!code-csharp[Main](configuration/sample/src/UsingOptions/Startup3.cs?name=snippet1&highlight=16-17)]</span></span>

<span data-ttu-id="9c121-193">注意： 這個擴充方法需要`Microsoft.Extensions.Options.ConfigurationExtensions`NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="9c121-193">Note: This extension method requires the `Microsoft.Extensions.Options.ConfigurationExtensions` NuGet package.</span></span>

<span data-ttu-id="9c121-194">使用下列*appsettings.json*檔案：</span><span class="sxs-lookup"><span data-stu-id="9c121-194">Using the following *appsettings.json* file:</span></span>

<span data-ttu-id="9c121-195">[!code-json[Main](configuration/sample/src/UsingOptions/appsettings.json)]</span><span class="sxs-lookup"><span data-stu-id="9c121-195">[!code-json[Main](configuration/sample/src/UsingOptions/appsettings.json)]</span></span>

<span data-ttu-id="9c121-196">`MySubOptions`類別：</span><span class="sxs-lookup"><span data-stu-id="9c121-196">The `MySubOptions` class:</span></span>

<span data-ttu-id="9c121-197">[!code-csharp[Main](configuration/sample/src/UsingOptions/Models/MySubOptions.cs)]</span><span class="sxs-lookup"><span data-stu-id="9c121-197">[!code-csharp[Main](configuration/sample/src/UsingOptions/Models/MySubOptions.cs)]</span></span>

<span data-ttu-id="9c121-198">以下列`Controller`:</span><span class="sxs-lookup"><span data-stu-id="9c121-198">With the following `Controller`:</span></span>

<span data-ttu-id="9c121-199">[!code-csharp[Main](configuration/sample/src/UsingOptions/Controllers/HomeController2.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="9c121-199">[!code-csharp[Main](configuration/sample/src/UsingOptions/Controllers/HomeController2.cs?name=snippet1)]</span></span>

<span data-ttu-id="9c121-200">`subOption1 = subvalue1_from_json, subOption2 = 200`會傳回。</span><span class="sxs-lookup"><span data-stu-id="9c121-200">`subOption1 = subvalue1_from_json, subOption2 = 200` is returned.</span></span>

<a name=in-memory-provider></a>

## <a name="ioptionssnapshot"></a><span data-ttu-id="9c121-201">IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="9c121-201">IOptionsSnapshot</span></span>

<span data-ttu-id="9c121-202">*需要 ASP.NET Core 1.1 或更高版本。*</span><span class="sxs-lookup"><span data-stu-id="9c121-202">*Requires ASP.NET Core 1.1 or higher.*</span></span>

<span data-ttu-id="9c121-203">`IOptionsSnapshot`支援的組態檔變更時重新載入組態資料。</span><span class="sxs-lookup"><span data-stu-id="9c121-203">`IOptionsSnapshot` supports reloading configuration data when the configuration file has changed.</span></span> <span data-ttu-id="9c121-204">它的最少的額外負荷。</span><span class="sxs-lookup"><span data-stu-id="9c121-204">It has minimal overhead.</span></span> <span data-ttu-id="9c121-205">使用`IOptionsSnapshot`與`reloadOnChange: true`，選項會繫結至`IConfiguration`，變更時重新載入。</span><span class="sxs-lookup"><span data-stu-id="9c121-205">Using `IOptionsSnapshot` with `reloadOnChange: true`, the options are bound to `IConfiguration` and reloaded when changed.</span></span>

<span data-ttu-id="9c121-206">下列範例將示範如何為新`IOptionsSnapshot`之後，會建立*config.json*變更。</span><span class="sxs-lookup"><span data-stu-id="9c121-206">The following sample demonstrates how a new `IOptionsSnapshot` is created after *config.json* changes.</span></span> <span data-ttu-id="9c121-207">伺服器的要求會傳回相同時間*config.json*具有**不**變更。</span><span class="sxs-lookup"><span data-stu-id="9c121-207">Requests to the server will return the same time when *config.json* has **not** changed.</span></span> <span data-ttu-id="9c121-208">第一個要求之後*config.json*變更將會顯示新的時間。</span><span class="sxs-lookup"><span data-stu-id="9c121-208">The first request after *config.json* changes will show a new time.</span></span>

<span data-ttu-id="9c121-209">[!code-csharp[Main](configuration/sample/IOptionsSnapshot2/Startup.cs?name=snippet1&highlight=1-9,13-18,32,33,52,53)]</span><span class="sxs-lookup"><span data-stu-id="9c121-209">[!code-csharp[Main](configuration/sample/IOptionsSnapshot2/Startup.cs?name=snippet1&highlight=1-9,13-18,32,33,52,53)]</span></span>

<span data-ttu-id="9c121-210">下圖顯示伺服器輸出：</span><span class="sxs-lookup"><span data-stu-id="9c121-210">The following image shows the server output:</span></span>

![瀏覽器影像顯示 「 上次更新日期： 2016 年 11 月 22 下午 4:43"](configuration/_static/first.png)

<span data-ttu-id="9c121-212">重新整理瀏覽器不會變更訊息值或顯示的時間 (當*config.json*未變更)。</span><span class="sxs-lookup"><span data-stu-id="9c121-212">Refreshing the browser doesn't change the message value or time displayed (when *config.json* has not changed).</span></span>

<span data-ttu-id="9c121-213">變更並儲存*config.json*然後重新整理瀏覽器：</span><span class="sxs-lookup"><span data-stu-id="9c121-213">Change and save the  *config.json* and then refresh the browser:</span></span>

![瀏覽器影像顯示 「 上次更新為 e: 2016 年 11 月 22 4:53 PM"](configuration/_static/change.png)

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="9c121-215">記憶體中的提供者和繫結至 POCO 類別</span><span class="sxs-lookup"><span data-stu-id="9c121-215">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="9c121-216">下列範例會示範如何使用記憶體中的提供者，並繫結至類別：</span><span class="sxs-lookup"><span data-stu-id="9c121-216">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

<span data-ttu-id="9c121-217">[!code-csharp[Main](configuration/sample/src/InMemory/Program.cs)]</span><span class="sxs-lookup"><span data-stu-id="9c121-217">[!code-csharp[Main](configuration/sample/src/InMemory/Program.cs)]</span></span>

<span data-ttu-id="9c121-218">組態值會以字串傳回，但繫結啟用了物件的建構。</span><span class="sxs-lookup"><span data-stu-id="9c121-218">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="9c121-219">繫結可讓您擷取 POCO 物件或甚至整個物件圖形。</span><span class="sxs-lookup"><span data-stu-id="9c121-219">Binding allows you to retrieve POCO objects or even entire object graphs.</span></span> <span data-ttu-id="9c121-220">下列範例示範如何將繫結至`MyWindow`，ASP.NET Core MVC 應用程式會使用選項模式：</span><span class="sxs-lookup"><span data-stu-id="9c121-220">The following sample shows how to bind to `MyWindow` and use the options pattern with a ASP.NET Core MVC app:</span></span>

<span data-ttu-id="9c121-221">[!code-csharp[Main](configuration/sample/src/WebConfigBind/MyWindow.cs)]</span><span class="sxs-lookup"><span data-stu-id="9c121-221">[!code-csharp[Main](configuration/sample/src/WebConfigBind/MyWindow.cs)]</span></span>

<span data-ttu-id="9c121-222">[!code-json[Main](configuration/sample/src/WebConfigBind/appsettings.json)]</span><span class="sxs-lookup"><span data-stu-id="9c121-222">[!code-json[Main](configuration/sample/src/WebConfigBind/appsettings.json)]</span></span>

<span data-ttu-id="9c121-223">繫結中的自訂類別`ConfigureServices`中`Startup`類別：</span><span class="sxs-lookup"><span data-stu-id="9c121-223">Bind the custom class in `ConfigureServices` in the `Startup` class:</span></span>

<span data-ttu-id="9c121-224">[!code-csharp[Main](configuration/sample/src/WebConfigBind/Startup.cs?name=snippet1&highlight=3,4)]</span><span class="sxs-lookup"><span data-stu-id="9c121-224">[!code-csharp[Main](configuration/sample/src/WebConfigBind/Startup.cs?name=snippet1&highlight=3,4)]</span></span>

<span data-ttu-id="9c121-225">顯示從設定`HomeController`:</span><span class="sxs-lookup"><span data-stu-id="9c121-225">Display the settings from the `HomeController`:</span></span>

<span data-ttu-id="9c121-226">[!code-csharp[Main](configuration/sample/src/WebConfigBind/Controllers/HomeController.cs)]</span><span class="sxs-lookup"><span data-stu-id="9c121-226">[!code-csharp[Main](configuration/sample/src/WebConfigBind/Controllers/HomeController.cs)]</span></span>

### <a name="getvalue"></a><span data-ttu-id="9c121-227">GetValue</span><span class="sxs-lookup"><span data-stu-id="9c121-227">GetValue</span></span>

<span data-ttu-id="9c121-228">下列範例會示範[GetValue<T> ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_)擴充方法：</span><span class="sxs-lookup"><span data-stu-id="9c121-228">The following sample demonstrates the [GetValue<T>](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) extension method:</span></span>

<span data-ttu-id="9c121-229">[!code-csharp[Main](configuration/sample/src/InMemoryGetValue/Program.cs?highlight=27-29)]</span><span class="sxs-lookup"><span data-stu-id="9c121-229">[!code-csharp[Main](configuration/sample/src/InMemoryGetValue/Program.cs?highlight=27-29)]</span></span>

<span data-ttu-id="9c121-230">ConfigurationBinder`GetValue<T>`方法可讓您指定的預設值 (在此範例中為 80)。</span><span class="sxs-lookup"><span data-stu-id="9c121-230">The ConfigurationBinder's `GetValue<T>` method allows you to specify a default value (80 in the sample).</span></span> <span data-ttu-id="9c121-231">`GetValue<T>`適用於簡單的案例並不會連結到整個區段。</span><span class="sxs-lookup"><span data-stu-id="9c121-231">`GetValue<T>` is for simple scenarios and does not bind to entire sections.</span></span> <span data-ttu-id="9c121-232">`GetValue<T>`取得純量值從`GetSection(key).Value`轉換為特定的型別。</span><span class="sxs-lookup"><span data-stu-id="9c121-232">`GetValue<T>` gets scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="binding-to-an-object-graph"></a><span data-ttu-id="9c121-233">繫結至物件圖形</span><span class="sxs-lookup"><span data-stu-id="9c121-233">Binding to an object graph</span></span>

<span data-ttu-id="9c121-234">您可以對每個物件類別中的遞迴繫結。</span><span class="sxs-lookup"><span data-stu-id="9c121-234">You can recursively bind to each object in a class.</span></span> <span data-ttu-id="9c121-235">請考慮下列`AppOptions`類別：</span><span class="sxs-lookup"><span data-stu-id="9c121-235">Consider the following `AppOptions` class:</span></span>

<span data-ttu-id="9c121-236">[!code-csharp[Main](configuration/sample/src/ObjectGraph/AppOptions.cs)]</span><span class="sxs-lookup"><span data-stu-id="9c121-236">[!code-csharp[Main](configuration/sample/src/ObjectGraph/AppOptions.cs)]</span></span>

<span data-ttu-id="9c121-237">下列範例會繫結至`AppOptions`類別：</span><span class="sxs-lookup"><span data-stu-id="9c121-237">The following sample binds to the `AppOptions` class:</span></span>

<span data-ttu-id="9c121-238">[!code-csharp[Main](configuration/sample/src/ObjectGraph/Program.cs?highlight=15-16)]</span><span class="sxs-lookup"><span data-stu-id="9c121-238">[!code-csharp[Main](configuration/sample/src/ObjectGraph/Program.cs?highlight=15-16)]</span></span>

<span data-ttu-id="9c121-239">**ASP.NET Core 1.1**和更新版本可以使用`Get<T>`，這適用於整個區段。</span><span class="sxs-lookup"><span data-stu-id="9c121-239">**ASP.NET Core 1.1** and higher can use  `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="9c121-240">`Get<T>`可以是比使用更多便利`Bind`。</span><span class="sxs-lookup"><span data-stu-id="9c121-240">`Get<T>` can be more convienent than using `Bind`.</span></span> <span data-ttu-id="9c121-241">下列程式碼示範如何使用`Get<T>`與上面的範例：</span><span class="sxs-lookup"><span data-stu-id="9c121-241">The following code shows how to use `Get<T>` with the sample above:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppOptions>();
```

<span data-ttu-id="9c121-242">使用下列*appsettings.json*檔案：</span><span class="sxs-lookup"><span data-stu-id="9c121-242">Using the following *appsettings.json* file:</span></span>

<span data-ttu-id="9c121-243">[!code-json[Main](configuration/sample/src/ObjectGraph/appsettings.json)]</span><span class="sxs-lookup"><span data-stu-id="9c121-243">[!code-json[Main](configuration/sample/src/ObjectGraph/appsettings.json)]</span></span>

<span data-ttu-id="9c121-244">程式會顯示`Height 11`。</span><span class="sxs-lookup"><span data-stu-id="9c121-244">The program displays `Height 11`.</span></span>

<span data-ttu-id="9c121-245">下列程式碼可用於單元測試組態：</span><span class="sxs-lookup"><span data-stu-id="9c121-245">The following code can be used to unit test the configuration:</span></span>

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

## <a name="basic-sample-of-entity-framework-custom-provider"></a><span data-ttu-id="9c121-246">Entity Framework 自訂提供者的基本範例</span><span class="sxs-lookup"><span data-stu-id="9c121-246">Basic sample of Entity Framework custom provider</span></span>

<span data-ttu-id="9c121-247">在本節中，會建立使用 EF，從資料庫讀取名稱 / 值組的基本組態提供者。</span><span class="sxs-lookup"><span data-stu-id="9c121-247">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span> 

<span data-ttu-id="9c121-248">定義`ConfigurationValue`將組態值儲存在資料庫中的實體：</span><span class="sxs-lookup"><span data-stu-id="9c121-248">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

<span data-ttu-id="9c121-249">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/ConfigurationValue.cs)]</span><span class="sxs-lookup"><span data-stu-id="9c121-249">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/ConfigurationValue.cs)]</span></span>

<span data-ttu-id="9c121-250">新增`ConfigurationContext`來儲存及存取設定的值：</span><span class="sxs-lookup"><span data-stu-id="9c121-250">Add a `ConfigurationContext` to store and access the configured values:</span></span>

<span data-ttu-id="9c121-251">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="9c121-251">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]</span></span>

<span data-ttu-id="9c121-252">建立類別，實作[IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span><span class="sxs-lookup"><span data-stu-id="9c121-252">Create an class that implements [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span></span>

<span data-ttu-id="9c121-253">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]</span><span class="sxs-lookup"><span data-stu-id="9c121-253">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]</span></span>

<span data-ttu-id="9c121-254">建立自訂組態提供者透過繼承自[ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider)。</span><span class="sxs-lookup"><span data-stu-id="9c121-254">Create the custom configuration provider by inheriting from [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).</span></span>  <span data-ttu-id="9c121-255">當它是空的組態提供者會初始化資料庫：</span><span class="sxs-lookup"><span data-stu-id="9c121-255">The configuration provider initializes the database when it's empty:</span></span>

<span data-ttu-id="9c121-256">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]</span><span class="sxs-lookup"><span data-stu-id="9c121-256">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]</span></span>

<span data-ttu-id="9c121-257">當執行範例時，會顯示資料庫 （「 value_from_ef_1"和"value_from_ef_2"） 中反白顯示的值。</span><span class="sxs-lookup"><span data-stu-id="9c121-257">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="9c121-258">您可以加入`EFConfigSource`擴充方法新增組態來源：</span><span class="sxs-lookup"><span data-stu-id="9c121-258">You can add an `EFConfigSource` extension method for adding the configuration source:</span></span>

<span data-ttu-id="9c121-259">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]</span><span class="sxs-lookup"><span data-stu-id="9c121-259">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]</span></span>

<span data-ttu-id="9c121-260">下列程式碼示範如何使用自訂`EFConfigProvider`:</span><span class="sxs-lookup"><span data-stu-id="9c121-260">The following code shows how to use the custom `EFConfigProvider`:</span></span>

<span data-ttu-id="9c121-261">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/Program.cs?highlight=20-25)]</span><span class="sxs-lookup"><span data-stu-id="9c121-261">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/Program.cs?highlight=20-25)]</span></span>

<span data-ttu-id="9c121-262">請注意此範例會將自訂`EFConfigProvider`後的 JSON 提供者，因此資料庫的任何設定會覆寫設定從*appsettings.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="9c121-262">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="9c121-263">使用下列*appsettings.json*檔案：</span><span class="sxs-lookup"><span data-stu-id="9c121-263">Using the following *appsettings.json* file:</span></span>

<span data-ttu-id="9c121-264">[!code-json[Main](configuration/sample/src/CustomConfigurationProvider/appsettings.json)]</span><span class="sxs-lookup"><span data-stu-id="9c121-264">[!code-json[Main](configuration/sample/src/CustomConfigurationProvider/appsettings.json)]</span></span>

<span data-ttu-id="9c121-265">顯示下列項目：</span><span class="sxs-lookup"><span data-stu-id="9c121-265">The following is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="9c121-266">命令列組態提供者</span><span class="sxs-lookup"><span data-stu-id="9c121-266">CommandLine configuration provider</span></span>

<span data-ttu-id="9c121-267">下列範例會啟用最後 CommandLine 組態提供者：</span><span class="sxs-lookup"><span data-stu-id="9c121-267">The following sample enables the CommandLine configuration provider last:</span></span>

<span data-ttu-id="9c121-268">[!code-csharp[Main](configuration/sample/src/CommandLine/Program.cs)]</span><span class="sxs-lookup"><span data-stu-id="9c121-268">[!code-csharp[Main](configuration/sample/src/CommandLine/Program.cs)]</span></span>

<span data-ttu-id="9c121-269">使用下列組態設定中傳遞：</span><span class="sxs-lookup"><span data-stu-id="9c121-269">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=Bob /App:MainWindow:Left=1234
```

<span data-ttu-id="9c121-270">這會顯示：</span><span class="sxs-lookup"><span data-stu-id="9c121-270">Which displays:</span></span>

```console
Hello Bob
Left 1234
```

<span data-ttu-id="9c121-271">`GetSwitchMappings`方法可讓您使用`-`而不是`/`和它去除開頭的子機碼前置詞。</span><span class="sxs-lookup"><span data-stu-id="9c121-271">The `GetSwitchMappings` method allows you to use `-` rather than `/` and it strips the leading subkey prefixes.</span></span> <span data-ttu-id="9c121-272">例如: </span><span class="sxs-lookup"><span data-stu-id="9c121-272">For example:</span></span>

```console
dotnet run -MachineName=Bob -Left=7734
```

<span data-ttu-id="9c121-273">顯示：</span><span class="sxs-lookup"><span data-stu-id="9c121-273">Displays:</span></span>

```console
Hello Bob
Left 7734
```

<span data-ttu-id="9c121-274">命令列引數必須包含 （它可以是 null） 值。</span><span class="sxs-lookup"><span data-stu-id="9c121-274">Command-line arguments must include a value (it can be null).</span></span> <span data-ttu-id="9c121-275">例如: </span><span class="sxs-lookup"><span data-stu-id="9c121-275">For example:</span></span>

```console
dotnet run /Profile:MachineName=
```

<span data-ttu-id="9c121-276">為 [確定]，但</span><span class="sxs-lookup"><span data-stu-id="9c121-276">Is OK, but</span></span>

```console
dotnet run /Profile:MachineName
```

<span data-ttu-id="9c121-277">導致例外狀況。</span><span class="sxs-lookup"><span data-stu-id="9c121-277">results in an exception.</span></span> <span data-ttu-id="9c121-278">如果您指定的-或--有是沒有對應的參數對應的命令列參數前置詞，將會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="9c121-278">An exception will be thrown if you specify a command-line switch prefix of - or -- for which there's no corresponding switch mapping.</span></span>

## <a name="the-webconfig-file"></a><span data-ttu-id="9c121-279">Web.config 檔案</span><span class="sxs-lookup"><span data-stu-id="9c121-279">The web.config file</span></span>

<span data-ttu-id="9c121-280">A *web.config*檔案時，需要您裝載於 IIS 或 IIS Express 應用程式。</span><span class="sxs-lookup"><span data-stu-id="9c121-280">A *web.config* file is required when you host the app in IIS or IIS-Express.</span></span> <span data-ttu-id="9c121-281">*web.config* AspNetCoreModule IIS 啟動您的應用程式中開啟。</span><span class="sxs-lookup"><span data-stu-id="9c121-281">*web.config* turns on the AspNetCoreModule in IIS to launch your app.</span></span> <span data-ttu-id="9c121-282">中的設定*web.config*啟用 AspNetCoreModule IIS 啟動應用程式，並設定其他 IIS 設定和模組中的。</span><span class="sxs-lookup"><span data-stu-id="9c121-282">Settings in *web.config* enable the AspNetCoreModule in IIS to launch your app and configure other IIS settings and modules.</span></span> <span data-ttu-id="9c121-283">如果您使用 Visual Studio，刪除*web.config*，Visual Studio 會建立一個新。</span><span class="sxs-lookup"><span data-stu-id="9c121-283">If you are using Visual Studio and delete *web.config*, Visual Studio will create a new one.</span></span>

### <a name="additional-notes"></a><span data-ttu-id="9c121-284">其他備註</span><span class="sxs-lookup"><span data-stu-id="9c121-284">Additional notes</span></span>

* <span data-ttu-id="9c121-285">相依性插入 (DI) 未設定為止之後`ConfigureServices`叫用。</span><span class="sxs-lookup"><span data-stu-id="9c121-285">Dependency Injection (DI) is not set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="9c121-286">組態系統不 DI 注意。</span><span class="sxs-lookup"><span data-stu-id="9c121-286">The configuration system is not DI aware.</span></span>
* <span data-ttu-id="9c121-287">`IConfiguration`有兩個特製化：</span><span class="sxs-lookup"><span data-stu-id="9c121-287">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="9c121-288">`IConfigurationRoot`用於根節點。</span><span class="sxs-lookup"><span data-stu-id="9c121-288">`IConfigurationRoot`  Used for the root node.</span></span> <span data-ttu-id="9c121-289">可以觸發重新載入。</span><span class="sxs-lookup"><span data-stu-id="9c121-289">Can trigger a reload.</span></span>
  * <span data-ttu-id="9c121-290">`IConfigurationSection`代表組態值的區段。</span><span class="sxs-lookup"><span data-stu-id="9c121-290">`IConfigurationSection`  Represents a section of configuration values.</span></span> <span data-ttu-id="9c121-291">`GetSection`和`GetChildren`方法會傳回`IConfigurationSection`。</span><span class="sxs-lookup"><span data-stu-id="9c121-291">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="9c121-292">其他資源</span><span class="sxs-lookup"><span data-stu-id="9c121-292">Additional resources</span></span>

* [<span data-ttu-id="9c121-293">使用多個環境</span><span class="sxs-lookup"><span data-stu-id="9c121-293">Working with Multiple Environments</span></span>](environments.md)
* [<span data-ttu-id="9c121-294">在開發期間的應用程式密碼的安全存放</span><span class="sxs-lookup"><span data-stu-id="9c121-294">Safe storage of app secrets during development</span></span>](../security/app-secrets.md)
* [<span data-ttu-id="9c121-295">相依性插入</span><span class="sxs-lookup"><span data-stu-id="9c121-295">Dependency Injection</span></span>](dependency-injection.md)
* [<span data-ttu-id="9c121-296">Azure 金鑰保存庫的組態提供者</span><span class="sxs-lookup"><span data-stu-id="9c121-296">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
