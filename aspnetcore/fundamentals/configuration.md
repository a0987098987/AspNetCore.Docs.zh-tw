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
ms.openlocfilehash: 7d591259587766a932a14bb030c76274101d16ac
ms.sourcegitcommit: f8f6b5934bd071a349f5bc1e389365c52b1c00fa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/14/2017
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="aa88f-104">ASP.NET Core 的設定</span><span class="sxs-lookup"><span data-stu-id="aa88f-104">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="aa88f-105">[Rick Anderson](https://twitter.com/RickAndMSFT)，[標記 Michaelis](http://intellitect.com/author/mark-michaelis/)， [Steve Smith](https://ardalis.com/)，和[奧 Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="aa88f-105">[Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="aa88f-106">組態 API 提供一種設定的名稱 / 值組清單為基礎的應用程式。</span><span class="sxs-lookup"><span data-stu-id="aa88f-106">The Configuration API provides a way of configuring an app based on a list of name-value pairs.</span></span> <span data-ttu-id="aa88f-107">在執行階段從多個來源讀取組態。</span><span class="sxs-lookup"><span data-stu-id="aa88f-107">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="aa88f-108">名稱 / 值組可以分為多層級的階層。</span><span class="sxs-lookup"><span data-stu-id="aa88f-108">The name-value pairs can be grouped into a multi-level hierarchy.</span></span> <span data-ttu-id="aa88f-109">有的組態提供者：</span><span class="sxs-lookup"><span data-stu-id="aa88f-109">There are configuration providers for:</span></span>

* <span data-ttu-id="aa88f-110">檔案格式 （INI、 JSON 和 XML）</span><span class="sxs-lookup"><span data-stu-id="aa88f-110">File formats (INI, JSON, and XML)</span></span>
* <span data-ttu-id="aa88f-111">命令列引數</span><span class="sxs-lookup"><span data-stu-id="aa88f-111">Command-line arguments</span></span>
* <span data-ttu-id="aa88f-112">環境變數</span><span class="sxs-lookup"><span data-stu-id="aa88f-112">Environment variables</span></span>
* <span data-ttu-id="aa88f-113">記憶體中的.NET 物件</span><span class="sxs-lookup"><span data-stu-id="aa88f-113">In-memory .NET objects</span></span>
* <span data-ttu-id="aa88f-114">加密的使用者存放區</span><span class="sxs-lookup"><span data-stu-id="aa88f-114">An encrypted user store</span></span>
* [<span data-ttu-id="aa88f-115">Azure 金鑰保存庫</span><span class="sxs-lookup"><span data-stu-id="aa88f-115">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
* <span data-ttu-id="aa88f-116">自訂提供者，供您安裝或建立</span><span class="sxs-lookup"><span data-stu-id="aa88f-116">Custom providers, which you install or create</span></span>

<span data-ttu-id="aa88f-117">每個組態值將對應至字串索引鍵。</span><span class="sxs-lookup"><span data-stu-id="aa88f-117">Each configuration value maps to a string key.</span></span> <span data-ttu-id="aa88f-118">沒有要還原序列化到自訂設定的內建繫結支援[POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object)物件 （具有屬性的簡單.NET 類別）。</span><span class="sxs-lookup"><span data-stu-id="aa88f-118">There's built-in binding support to deserialize settings into a custom [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

[<span data-ttu-id="aa88f-119">檢視或下載範例程式碼</span><span class="sxs-lookup"><span data-stu-id="aa88f-119">View or download sample code</span></span>](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/sample)

## <a name="simple-configuration"></a><span data-ttu-id="aa88f-120">簡單的組態</span><span class="sxs-lookup"><span data-stu-id="aa88f-120">Simple configuration</span></span>

<span data-ttu-id="aa88f-121">下列主控台應用程式會使用 JSON 組態提供者：</span><span class="sxs-lookup"><span data-stu-id="aa88f-121">The following console app uses the JSON configuration provider:</span></span>

[!code-csharp[Main](configuration/sample/ConfigJson/Program.cs)]

<span data-ttu-id="aa88f-122">應用程式讀取，並顯示下列組態設定：</span><span class="sxs-lookup"><span data-stu-id="aa88f-122">The app reads and displays the following configuration settings:</span></span>

[!code-json[Main](configuration/sample/ConfigJson/appsettings.json)]

<span data-ttu-id="aa88f-123">設定包含在其中的節點以冒號分隔的名稱 / 值組的階層式清單。</span><span class="sxs-lookup"><span data-stu-id="aa88f-123">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon.</span></span> <span data-ttu-id="aa88f-124">若要擷取值，存取`Configuration`與對應的項目索引鍵的索引子：</span><span class="sxs-lookup"><span data-stu-id="aa88f-124">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

```csharp
Console.WriteLine($"option1 = {Configuration["subsection:suboption1"]}");
```

<span data-ttu-id="aa88f-125">若要使用 JSON 格式設定來源中的陣列，使用陣列索引一部分的冒號分隔的字串。</span><span class="sxs-lookup"><span data-stu-id="aa88f-125">To work with arrays in JSON-formatted configuration sources, use a array index as part of the colon-separated string.</span></span> <span data-ttu-id="aa88f-126">下列範例會取得在上述第一個項目的名稱`wizards`陣列：</span><span class="sxs-lookup"><span data-stu-id="aa88f-126">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}, ");
```

<span data-ttu-id="aa88f-127">寫入至內建的名稱 / 值組中`Configuration`提供者是**不**保存，不過，您可以建立將值儲存的自訂提供者。</span><span class="sxs-lookup"><span data-stu-id="aa88f-127">Name-value pairs written to the built in `Configuration` providers are **not** persisted, however, you can create a custom provider that saves values.</span></span> <span data-ttu-id="aa88f-128">請參閱[自訂組態提供者](xref:fundamentals/configuration#custom-config-providers)。</span><span class="sxs-lookup"><span data-stu-id="aa88f-128">See [custom configuration provider](xref:fundamentals/configuration#custom-config-providers).</span></span>

<span data-ttu-id="aa88f-129">上述範例會使用設定索引子讀取的值。</span><span class="sxs-lookup"><span data-stu-id="aa88f-129">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="aa88f-130">外部存取組態`Startup`，使用[選項模式](xref:fundamentals/configuration#options-config-objects)。</span><span class="sxs-lookup"><span data-stu-id="aa88f-130">To access configuration outside of `Startup`, use the [options pattern](xref:fundamentals/configuration#options-config-objects).</span></span> <span data-ttu-id="aa88f-131">*選項模式*本文稍後所示。</span><span class="sxs-lookup"><span data-stu-id="aa88f-131">The *options pattern* is shown later in this article.</span></span>

<span data-ttu-id="aa88f-132">這是通常會有不同的環境，例如開發、 測試和實際執行不同的組態設定。</span><span class="sxs-lookup"><span data-stu-id="aa88f-132">It's typical to have different configuration settings for different environments, for example, development, test, and production.</span></span> <span data-ttu-id="aa88f-133">`CreateDefaultBuilder` ASP.NET Core 2.x 應用程式中的擴充方法 (或使用`AddJsonFile`和`AddEnvironmentVariables`直接在 ASP.NET Core 1.x 應用程式中) 將讀取 JSON 檔案和系統設定來源的組態提供者：</span><span class="sxs-lookup"><span data-stu-id="aa88f-133">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="aa88f-134">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="aa88f-134">*appsettings.json*</span></span>
* <span data-ttu-id="aa88f-135">*appsettings。\<EnvironmentName >.json*</span><span class="sxs-lookup"><span data-stu-id="aa88f-135">*appsettings.\<EnvironmentName>.json*</span></span>
* <span data-ttu-id="aa88f-136">環境變數</span><span class="sxs-lookup"><span data-stu-id="aa88f-136">environment variables</span></span>

<span data-ttu-id="aa88f-137">請參閱[AddJsonFile](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.jsonconfigurationextensions)取得參數的說明。</span><span class="sxs-lookup"><span data-stu-id="aa88f-137">See [AddJsonFile](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="aa88f-138">`reloadOnChange`僅適用於 ASP.NET Core 1.1 （含） 以上。</span><span class="sxs-lookup"><span data-stu-id="aa88f-138">`reloadOnChange` is only supported in ASP.NET Core 1.1 and higher.</span></span> 

<span data-ttu-id="aa88f-139">設定來源會讀取這些指定的順序。</span><span class="sxs-lookup"><span data-stu-id="aa88f-139">Configuration sources are read in the order they are specified.</span></span> <span data-ttu-id="aa88f-140">上述程式碼，是上次讀取環境變數。</span><span class="sxs-lookup"><span data-stu-id="aa88f-140">In the code above, the environment variables are read last.</span></span> <span data-ttu-id="aa88f-141">設定透過環境的所有組態值會都取代中的兩個先前提供者設定。</span><span class="sxs-lookup"><span data-stu-id="aa88f-141">Any configuration values set through the environment would replace those set in the two previous providers.</span></span>

<span data-ttu-id="aa88f-142">環境通常設定為其中一個`Development`， `Staging`，或`Production`。</span><span class="sxs-lookup"><span data-stu-id="aa88f-142">The environment is typically set to one of `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="aa88f-143">請參閱[使用多個環境](environments.md)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="aa88f-143">See [Working with Multiple Environments](environments.md) for more information.</span></span>

<span data-ttu-id="aa88f-144">組態的考量：</span><span class="sxs-lookup"><span data-stu-id="aa88f-144">Configuration considerations:</span></span>

* <span data-ttu-id="aa88f-145">`IOptionsSnapshot`當監視變更時，就可以載入組態資料。</span><span class="sxs-lookup"><span data-stu-id="aa88f-145">`IOptionsSnapshot` can reload configuration data when it changes.</span></span> <span data-ttu-id="aa88f-146">使用`IOptionsSnapshot`如果您要重新載入組態資料。</span><span class="sxs-lookup"><span data-stu-id="aa88f-146">Use `IOptionsSnapshot` if you need to reload configuration data.</span></span>  <span data-ttu-id="aa88f-147">請參閱[IOptionsSnapshot](#ioptionssnapshot)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="aa88f-147">See [IOptionsSnapshot](#ioptionssnapshot) for more information.</span></span>
* <span data-ttu-id="aa88f-148">設定索引鍵是不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="aa88f-148">Configuration keys are case insensitive.</span></span>
* <span data-ttu-id="aa88f-149">最佳作法是最後指定環境變數，讓本機的環境可以覆寫在已部署的組態檔中設定的任何項目。</span><span class="sxs-lookup"><span data-stu-id="aa88f-149">A best practice is to specify environment variables last, so that the local environment can override anything set in deployed configuration files.</span></span>
* <span data-ttu-id="aa88f-150">**永遠不會**將密碼或其他敏感性資料儲存在組態提供者程式碼或純文字設定檔案中。</span><span class="sxs-lookup"><span data-stu-id="aa88f-150">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="aa88f-151">不在您開發使用生產機密資料或測試環境。</span><span class="sxs-lookup"><span data-stu-id="aa88f-151">Don't use production secrets in your development or test environments.</span></span> <span data-ttu-id="aa88f-152">相反地，指定外部專案樹狀結構的機密資料，讓它們無法意外認可到您的儲存機制。</span><span class="sxs-lookup"><span data-stu-id="aa88f-152">Instead, specify secrets outside the project tree, so they cannot be accidentally committed into your repository.</span></span> <span data-ttu-id="aa88f-153">深入了解[使用多個環境](environments.md)和管理[安全存放應用程式密碼，在開發期間](../security/app-secrets.md)。</span><span class="sxs-lookup"><span data-stu-id="aa88f-153">Learn more about [Working with Multiple Environments](environments.md) and managing [safe storage of app secrets during development](../security/app-secrets.md).</span></span>
* <span data-ttu-id="aa88f-154">如果`:`不能在您的系統中，以使用環境變數中，取代`:`與`__`（雙底線）。</span><span class="sxs-lookup"><span data-stu-id="aa88f-154">If `:` cannot be used in environment variables in your system,  replace `:`  with `__` (double underscore).</span></span>

<a name=options-config-objects></a>

## <a name="using-options-and-configuration-objects"></a><span data-ttu-id="aa88f-155">使用選項和設定物件</span><span class="sxs-lookup"><span data-stu-id="aa88f-155">Using Options and configuration objects</span></span>

<span data-ttu-id="aa88f-156">選項模式會使用自訂的選項類別來代表一組相關的設定。</span><span class="sxs-lookup"><span data-stu-id="aa88f-156">The options pattern uses custom options classes to represent a group of related settings.</span></span> <span data-ttu-id="aa88f-157">我們建議您在應用程式中建立低耦合的每個功能類別。</span><span class="sxs-lookup"><span data-stu-id="aa88f-157">We recommended that you create decoupled classes for each feature within your app.</span></span> <span data-ttu-id="aa88f-158">請依照下列低耦合的類別：</span><span class="sxs-lookup"><span data-stu-id="aa88f-158">Decoupled classes follow:</span></span>

* <span data-ttu-id="aa88f-159">[介面隔離原則 」 (ISP)](http://deviq.com/interface-segregation-principle/) ： 類別只取決於它們所使用的組態設定。</span><span class="sxs-lookup"><span data-stu-id="aa88f-159">The [Interface Segregation Principle (ISP)](http://deviq.com/interface-segregation-principle/) : Classes depend only on the configuration settings they use.</span></span>
* <span data-ttu-id="aa88f-160">[重要性分離](http://deviq.com/separation-of-concerns/)： 設定您的應用程式的不同部分會變成相依或彼此結合。</span><span class="sxs-lookup"><span data-stu-id="aa88f-160">[Separation of Concerns](http://deviq.com/separation-of-concerns/) : Settings for different parts of your app are not dependent or coupled with one another.</span></span>

<span data-ttu-id="aa88f-161">選項類別必須是抽象的公用的無參數建構函式。</span><span class="sxs-lookup"><span data-stu-id="aa88f-161">The options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="aa88f-162">例如: </span><span class="sxs-lookup"><span data-stu-id="aa88f-162">For example:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Models/MyOptions.cs)]

<a name=options-example></a>

<span data-ttu-id="aa88f-163">在下列程式碼，被啟用的 JSON 組態提供者。</span><span class="sxs-lookup"><span data-stu-id="aa88f-163">In the following code, the JSON configuration provider is enabled.</span></span> <span data-ttu-id="aa88f-164">`MyOptions`類別加入至服務容器，並繫結至組態。</span><span class="sxs-lookup"><span data-stu-id="aa88f-164">The `MyOptions` class is added to the service container and bound to configuration.</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Startup.cs?name=snippet1&highlight=8,20-21)]

<span data-ttu-id="aa88f-165">下列[控制器](../mvc/controllers/index.md)使用[建構函式相依性插入](xref:fundamentals/dependency-injection#what-is-dependency-injection)上[ `IOptions<TOptions>` ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1)以存取設定：</span><span class="sxs-lookup"><span data-stu-id="aa88f-165">The following [controller](../mvc/controllers/index.md)  uses [constructor Dependency Injection](xref:fundamentals/dependency-injection#what-is-dependency-injection) on [`IOptions<TOptions>`](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) to access settings:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="aa88f-166">以下列*appsettings.json*檔案：</span><span class="sxs-lookup"><span data-stu-id="aa88f-166">With the following *appsettings.json* file:</span></span>

[!code-json[Main](configuration/sample/UsingOptions/appsettings1.json)]

<span data-ttu-id="aa88f-167">`HomeController.Index`方法會傳回`option1 = value1_from_json, option2 = 2`。</span><span class="sxs-lookup"><span data-stu-id="aa88f-167">The `HomeController.Index` method returns `option1 = value1_from_json, option2 = 2`.</span></span>

<span data-ttu-id="aa88f-168">一般應用程式將不會將整個組態繫結至單一選項檔案。</span><span class="sxs-lookup"><span data-stu-id="aa88f-168">Typical apps won't bind the entire configuration to a single options file.</span></span> <span data-ttu-id="aa88f-169">稍後，我將示範如何使用`GetSection`繫結至一個區段。</span><span class="sxs-lookup"><span data-stu-id="aa88f-169">Later on I'll show how to use `GetSection` to bind to a section.</span></span>

<span data-ttu-id="aa88f-170">下列程式碼中，第二個`IConfigureOptions<TOptions>`服務新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="aa88f-170">In the following code, a second `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="aa88f-171">它使用委派來設定的繫結`MyOptions`。</span><span class="sxs-lookup"><span data-stu-id="aa88f-171">It uses a delegate to configure the binding with `MyOptions`.</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Startup2.cs?name=snippet1&highlight=9-13)]

<span data-ttu-id="aa88f-172">您可以加入多個組態提供者。</span><span class="sxs-lookup"><span data-stu-id="aa88f-172">You can add multiple configuration providers.</span></span> <span data-ttu-id="aa88f-173">使用 NuGet 封裝中的組態提供者。</span><span class="sxs-lookup"><span data-stu-id="aa88f-173">Configuration providers are available in NuGet packages.</span></span> <span data-ttu-id="aa88f-174">它們會套用順序註冊它們。</span><span class="sxs-lookup"><span data-stu-id="aa88f-174">They are applied in order they are registered.</span></span>

<span data-ttu-id="aa88f-175">每次呼叫`Configure<TOptions>`新增`IConfigureOptions<TOptions>`服務的服務容器。</span><span class="sxs-lookup"><span data-stu-id="aa88f-175">Each call to `Configure<TOptions>` adds an `IConfigureOptions<TOptions>` service to the service container.</span></span> <span data-ttu-id="aa88f-176">在上述範例中，值`Option1`和`Option2`中已指定*appsettings.json* -但的值`Option1`會覆寫設定的委派。</span><span class="sxs-lookup"><span data-stu-id="aa88f-176">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json* -- but the value of `Option1` is overridden by the configured delegate.</span></span> 

<span data-ttu-id="aa88f-177">啟用多個設定服務時，指定 「 獲勝 」，最後一個組態來源 （設定組態值）。</span><span class="sxs-lookup"><span data-stu-id="aa88f-177">When more than one configuration service is enabled, the last configuration source specified "wins" (sets the configuration value).</span></span> <span data-ttu-id="aa88f-178">在上述程式碼，`HomeController.Index`方法會傳回`option1 = value1_from_action, option2 = 2`。</span><span class="sxs-lookup"><span data-stu-id="aa88f-178">In the preceding code, the `HomeController.Index` method returns `option1 = value1_from_action, option2 = 2`.</span></span>

<span data-ttu-id="aa88f-179">當您將選項設定繫結時，繫結至表單的組態機碼的選項類型中的每一個屬性`property[:sub-property:]`。</span><span class="sxs-lookup"><span data-stu-id="aa88f-179">When you bind options to configuration, each property in your options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="aa88f-180">比方說，`MyOptions.Option1`屬性繫結索引鍵`Option1`，這會從讀取`option1`屬性*appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="aa88f-180">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span> <span data-ttu-id="aa88f-181">子屬性範例會顯示在本文稍後。</span><span class="sxs-lookup"><span data-stu-id="aa88f-181">A sub-property sample is shown later in this article.</span></span>

<span data-ttu-id="aa88f-182">下列程式碼，而第三個`IConfigureOptions<TOptions>`服務新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="aa88f-182">In the following code, a third `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="aa88f-183">它會繫結`MySubOptions`區段`subsection`的*appsettings.json*檔案：</span><span class="sxs-lookup"><span data-stu-id="aa88f-183">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Startup3.cs?name=snippet1&highlight=16-17)]

<span data-ttu-id="aa88f-184">注意： 這個擴充方法需要`Microsoft.Extensions.Options.ConfigurationExtensions`NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="aa88f-184">Note: This extension method requires the `Microsoft.Extensions.Options.ConfigurationExtensions` NuGet package.</span></span>

<span data-ttu-id="aa88f-185">使用下列*appsettings.json*檔案：</span><span class="sxs-lookup"><span data-stu-id="aa88f-185">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](configuration/sample/UsingOptions/appsettings.json)]

<span data-ttu-id="aa88f-186">`MySubOptions`類別：</span><span class="sxs-lookup"><span data-stu-id="aa88f-186">The `MySubOptions` class:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="aa88f-187">以下列`Controller`:</span><span class="sxs-lookup"><span data-stu-id="aa88f-187">With the following `Controller`:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController2.cs?name=snippet1)]

<span data-ttu-id="aa88f-188">`subOption1 = subvalue1_from_json, subOption2 = 200`會傳回。</span><span class="sxs-lookup"><span data-stu-id="aa88f-188">`subOption1 = subvalue1_from_json, subOption2 = 200` is returned.</span></span>

<span data-ttu-id="aa88f-189">您也可以提供選項，檢視模型中的，或插入`IOptions<TOptions>`直接插入檢視：</span><span class="sxs-lookup"><span data-stu-id="aa88f-189">You can also supply options in a view model or inject `IOptions<TOptions>` directly into a view:</span></span>

[!code-html[Main](configuration/sample/UsingOptions/Views/Home/Index.cshtml?highlight=3-4,16-17,20-21)]

<a name=in-memory-provider></a>

## <a name="ioptionssnapshot"></a><span data-ttu-id="aa88f-190">IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="aa88f-190">IOptionsSnapshot</span></span>

<span data-ttu-id="aa88f-191">*需要 ASP.NET Core 1.1 或更高版本。*</span><span class="sxs-lookup"><span data-stu-id="aa88f-191">*Requires ASP.NET Core 1.1 or higher.*</span></span>

<span data-ttu-id="aa88f-192">`IOptionsSnapshot`支援的組態檔變更時重新載入組態資料。</span><span class="sxs-lookup"><span data-stu-id="aa88f-192">`IOptionsSnapshot` supports reloading configuration data when the configuration file has changed.</span></span> <span data-ttu-id="aa88f-193">它的最少的額外負荷。</span><span class="sxs-lookup"><span data-stu-id="aa88f-193">It has minimal overhead.</span></span> <span data-ttu-id="aa88f-194">使用`IOptionsSnapshot`與`reloadOnChange: true`，選項會繫結至`IConfiguration`，變更時重新載入。</span><span class="sxs-lookup"><span data-stu-id="aa88f-194">Using `IOptionsSnapshot` with `reloadOnChange: true`, the options are bound to `IConfiguration` and reloaded when changed.</span></span>

<span data-ttu-id="aa88f-195">下列範例將示範如何為新`IOptionsSnapshot`之後，會建立*config.json*變更。</span><span class="sxs-lookup"><span data-stu-id="aa88f-195">The following sample demonstrates how a new `IOptionsSnapshot` is created after *config.json* changes.</span></span> <span data-ttu-id="aa88f-196">伺服器的要求會傳回相同時間*config.json*具有**不**變更。</span><span class="sxs-lookup"><span data-stu-id="aa88f-196">Requests to the server will return the same time when *config.json* has **not** changed.</span></span> <span data-ttu-id="aa88f-197">第一個要求之後*config.json*變更將會顯示新的時間。</span><span class="sxs-lookup"><span data-stu-id="aa88f-197">The first request after *config.json* changes will show a new time.</span></span>

[!code-csharp[Main](configuration/sample/IOptionsSnapshot2/Startup.cs?name=snippet1&highlight=1-9,13-18,32,33,52,53)]

<span data-ttu-id="aa88f-198">下圖顯示伺服器輸出：</span><span class="sxs-lookup"><span data-stu-id="aa88f-198">The following image shows the server output:</span></span>

![瀏覽器影像顯示 「 上次更新日期： 2016 年 11 月 22 下午 4:43"](configuration/_static/first.png)

<span data-ttu-id="aa88f-200">重新整理瀏覽器不會變更訊息值或顯示的時間 (當*config.json*未變更)。</span><span class="sxs-lookup"><span data-stu-id="aa88f-200">Refreshing the browser doesn't change the message value or time displayed (when *config.json* has not changed).</span></span>

<span data-ttu-id="aa88f-201">變更並儲存*config.json*然後重新整理瀏覽器：</span><span class="sxs-lookup"><span data-stu-id="aa88f-201">Change and save the  *config.json* and then refresh the browser:</span></span>

![瀏覽器影像顯示 「 上次更新為 e: 2016 年 11 月 22 4:53 PM"](configuration/_static/change.png)

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="aa88f-203">記憶體中的提供者和繫結至 POCO 類別</span><span class="sxs-lookup"><span data-stu-id="aa88f-203">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="aa88f-204">下列範例會示範如何使用記憶體中的提供者，並繫結至類別：</span><span class="sxs-lookup"><span data-stu-id="aa88f-204">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

[!code-csharp[Main](configuration/sample/InMemory/Program.cs)]

<span data-ttu-id="aa88f-205">組態值會以字串傳回，但繫結啟用了物件的建構。</span><span class="sxs-lookup"><span data-stu-id="aa88f-205">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="aa88f-206">繫結可讓您擷取 POCO 物件或甚至整個物件圖形。</span><span class="sxs-lookup"><span data-stu-id="aa88f-206">Binding allows you to retrieve POCO objects or even entire object graphs.</span></span> <span data-ttu-id="aa88f-207">下列範例示範如何將繫結至`MyWindow`，ASP.NET Core MVC 應用程式會使用選項模式：</span><span class="sxs-lookup"><span data-stu-id="aa88f-207">The following sample shows how to bind to `MyWindow` and use the options pattern with a ASP.NET Core MVC app:</span></span>

[!code-csharp[Main](configuration/sample/WebConfigBind/MyWindow.cs)]

[!code-json[Main](configuration/sample/WebConfigBind/appsettings.json)]

<span data-ttu-id="aa88f-208">繫結中的自訂類別`ConfigureServices`建置主應用程式時：</span><span class="sxs-lookup"><span data-stu-id="aa88f-208">Bind the custom class in `ConfigureServices` when building the host:</span></span>

[!code-csharp[Main](configuration/sample/WebConfigBind/Program.cs?name=snippet1&highlight=3-4)]

<span data-ttu-id="aa88f-209">顯示從設定`HomeController`:</span><span class="sxs-lookup"><span data-stu-id="aa88f-209">Display the settings from the `HomeController`:</span></span>

[!code-csharp[Main](configuration/sample/WebConfigBind/Controllers/HomeController.cs)]

### <a name="getvalue"></a><span data-ttu-id="aa88f-210">GetValue</span><span class="sxs-lookup"><span data-stu-id="aa88f-210">GetValue</span></span>

<span data-ttu-id="aa88f-211">下列範例會示範[GetValue<T> ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_)擴充方法：</span><span class="sxs-lookup"><span data-stu-id="aa88f-211">The following sample demonstrates the [GetValue<T>](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) extension method:</span></span>

[!code-csharp[Main](configuration/sample/InMemoryGetValue/Program.cs?highlight=27-29)]

<span data-ttu-id="aa88f-212">ConfigurationBinder`GetValue<T>`方法可讓您指定的預設值 (在此範例中為 80)。</span><span class="sxs-lookup"><span data-stu-id="aa88f-212">The ConfigurationBinder's `GetValue<T>` method allows you to specify a default value (80 in the sample).</span></span> <span data-ttu-id="aa88f-213">`GetValue<T>`適用於簡單的案例並不會連結到整個區段。</span><span class="sxs-lookup"><span data-stu-id="aa88f-213">`GetValue<T>` is for simple scenarios and does not bind to entire sections.</span></span> <span data-ttu-id="aa88f-214">`GetValue<T>`取得純量值從`GetSection(key).Value`轉換為特定的型別。</span><span class="sxs-lookup"><span data-stu-id="aa88f-214">`GetValue<T>` gets scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="binding-to-an-object-graph"></a><span data-ttu-id="aa88f-215">繫結至物件圖形</span><span class="sxs-lookup"><span data-stu-id="aa88f-215">Binding to an object graph</span></span>

<span data-ttu-id="aa88f-216">您可以對每個物件類別中的遞迴繫結。</span><span class="sxs-lookup"><span data-stu-id="aa88f-216">You can recursively bind to each object in a class.</span></span> <span data-ttu-id="aa88f-217">請考慮下列`AppOptions`類別：</span><span class="sxs-lookup"><span data-stu-id="aa88f-217">Consider the following `AppOptions` class:</span></span>

[!code-csharp[Main](configuration/sample/ObjectGraph/AppOptions.cs)]

<span data-ttu-id="aa88f-218">下列範例會繫結至`AppOptions`類別：</span><span class="sxs-lookup"><span data-stu-id="aa88f-218">The following sample binds to the `AppOptions` class:</span></span>

[!code-csharp[Main](configuration/sample/ObjectGraph/Program.cs?highlight=15-16)]

<span data-ttu-id="aa88f-219">**ASP.NET Core 1.1**和更新版本可以使用`Get<T>`，這適用於整個區段。</span><span class="sxs-lookup"><span data-stu-id="aa88f-219">**ASP.NET Core 1.1** and higher can use  `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="aa88f-220">`Get<T>`可以是比使用更多便利`Bind`。</span><span class="sxs-lookup"><span data-stu-id="aa88f-220">`Get<T>` can be more convienent than using `Bind`.</span></span> <span data-ttu-id="aa88f-221">下列程式碼示範如何使用`Get<T>`與上面的範例：</span><span class="sxs-lookup"><span data-stu-id="aa88f-221">The following code shows how to use `Get<T>` with the sample above:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppOptions>();
```

<span data-ttu-id="aa88f-222">使用下列*appsettings.json*檔案：</span><span class="sxs-lookup"><span data-stu-id="aa88f-222">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](configuration/sample/ObjectGraph/appsettings.json)]

<span data-ttu-id="aa88f-223">程式會顯示`Height 11`。</span><span class="sxs-lookup"><span data-stu-id="aa88f-223">The program displays `Height 11`.</span></span>

<span data-ttu-id="aa88f-224">下列程式碼可用於單元測試組態：</span><span class="sxs-lookup"><span data-stu-id="aa88f-224">The following code can be used to unit test the configuration:</span></span>

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

## <a name="basic-sample-of-entity-framework-custom-provider"></a><span data-ttu-id="aa88f-225">Entity Framework 自訂提供者的基本範例</span><span class="sxs-lookup"><span data-stu-id="aa88f-225">Basic sample of Entity Framework custom provider</span></span>

<span data-ttu-id="aa88f-226">在本節中，會建立使用 EF，從資料庫讀取名稱 / 值組的基本組態提供者。</span><span class="sxs-lookup"><span data-stu-id="aa88f-226">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span> 

<span data-ttu-id="aa88f-227">定義`ConfigurationValue`將組態值儲存在資料庫中的實體：</span><span class="sxs-lookup"><span data-stu-id="aa88f-227">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

<span data-ttu-id="aa88f-228">新增`ConfigurationContext`來儲存及存取設定的值：</span><span class="sxs-lookup"><span data-stu-id="aa88f-228">Add a `ConfigurationContext` to store and access the configured values:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="aa88f-229">建立類別，實作[IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span><span class="sxs-lookup"><span data-stu-id="aa88f-229">Create an class that implements [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

<span data-ttu-id="aa88f-230">建立自訂組態提供者透過繼承自[ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider)。</span><span class="sxs-lookup"><span data-stu-id="aa88f-230">Create the custom configuration provider by inheriting from [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).</span></span>  <span data-ttu-id="aa88f-231">當它是空的組態提供者會初始化資料庫：</span><span class="sxs-lookup"><span data-stu-id="aa88f-231">The configuration provider initializes the database when it's empty:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

<span data-ttu-id="aa88f-232">當執行範例時，會顯示資料庫 （「 value_from_ef_1"和"value_from_ef_2"） 中反白顯示的值。</span><span class="sxs-lookup"><span data-stu-id="aa88f-232">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="aa88f-233">您可以加入`EFConfigSource`擴充方法新增組態來源：</span><span class="sxs-lookup"><span data-stu-id="aa88f-233">You can add an `EFConfigSource` extension method for adding the configuration source:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

<span data-ttu-id="aa88f-234">下列程式碼示範如何使用自訂`EFConfigProvider`:</span><span class="sxs-lookup"><span data-stu-id="aa88f-234">The following code shows how to use the custom `EFConfigProvider`:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

<span data-ttu-id="aa88f-235">請注意此範例會將自訂`EFConfigProvider`後的 JSON 提供者，因此資料庫的任何設定會覆寫設定從*appsettings.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="aa88f-235">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="aa88f-236">使用下列*appsettings.json*檔案：</span><span class="sxs-lookup"><span data-stu-id="aa88f-236">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](configuration/sample/CustomConfigurationProvider/appsettings.json)]

<span data-ttu-id="aa88f-237">此時會顯示下列對話方塊：</span><span class="sxs-lookup"><span data-stu-id="aa88f-237">The following is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="aa88f-238">命令列組態提供者</span><span class="sxs-lookup"><span data-stu-id="aa88f-238">CommandLine configuration provider</span></span>

<span data-ttu-id="aa88f-239">下列範例會啟用最後 CommandLine 組態提供者：</span><span class="sxs-lookup"><span data-stu-id="aa88f-239">The following sample enables the CommandLine configuration provider last:</span></span>

[!code-csharp[Main](configuration/sample/CommandLine/Program.cs)]

<span data-ttu-id="aa88f-240">使用下列組態設定中傳遞：</span><span class="sxs-lookup"><span data-stu-id="aa88f-240">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=Bob /App:MainWindow:Left=1234
```

<span data-ttu-id="aa88f-241">這會顯示：</span><span class="sxs-lookup"><span data-stu-id="aa88f-241">Which displays:</span></span>

```console
Hello Bob
Left 1234
```

<span data-ttu-id="aa88f-242">`GetSwitchMappings`方法可讓您使用`-`而不是`/`和它去除開頭的子機碼前置詞。</span><span class="sxs-lookup"><span data-stu-id="aa88f-242">The `GetSwitchMappings` method allows you to use `-` rather than `/` and it strips the leading subkey prefixes.</span></span> <span data-ttu-id="aa88f-243">例如: </span><span class="sxs-lookup"><span data-stu-id="aa88f-243">For example:</span></span>

```console
dotnet run -MachineName=Bob -Left=7734
```

<span data-ttu-id="aa88f-244">顯示：</span><span class="sxs-lookup"><span data-stu-id="aa88f-244">Displays:</span></span>

```console
Hello Bob
Left 7734
```

<span data-ttu-id="aa88f-245">命令列引數必須包含 （它可以是 null） 值。</span><span class="sxs-lookup"><span data-stu-id="aa88f-245">Command-line arguments must include a value (it can be null).</span></span> <span data-ttu-id="aa88f-246">例如: </span><span class="sxs-lookup"><span data-stu-id="aa88f-246">For example:</span></span>

```console
dotnet run /Profile:MachineName=
```

<span data-ttu-id="aa88f-247">為 [確定]，但</span><span class="sxs-lookup"><span data-stu-id="aa88f-247">Is OK, but</span></span>

```console
dotnet run /Profile:MachineName
```

<span data-ttu-id="aa88f-248">導致例外狀況。</span><span class="sxs-lookup"><span data-stu-id="aa88f-248">results in an exception.</span></span> <span data-ttu-id="aa88f-249">如果您指定的-或--有是沒有對應的參數對應的命令列參數前置詞，將會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="aa88f-249">An exception will be thrown if you specify a command-line switch prefix of - or -- for which there's no corresponding switch mapping.</span></span>

## <a name="the-webconfig-file"></a><span data-ttu-id="aa88f-250">Web.config 檔案</span><span class="sxs-lookup"><span data-stu-id="aa88f-250">The web.config file</span></span>

<span data-ttu-id="aa88f-251">A *web.config*檔案時，需要您裝載於 IIS 或 IIS Express 應用程式。</span><span class="sxs-lookup"><span data-stu-id="aa88f-251">A *web.config* file is required when you host the app in IIS or IIS-Express.</span></span> <span data-ttu-id="aa88f-252">*web.config* AspNetCoreModule IIS 啟動您的應用程式中開啟。</span><span class="sxs-lookup"><span data-stu-id="aa88f-252">*web.config* turns on the AspNetCoreModule in IIS to launch your app.</span></span> <span data-ttu-id="aa88f-253">中的設定*web.config*啟用 AspNetCoreModule IIS 啟動應用程式，並設定其他 IIS 設定和模組中的。</span><span class="sxs-lookup"><span data-stu-id="aa88f-253">Settings in *web.config* enable the AspNetCoreModule in IIS to launch your app and configure other IIS settings and modules.</span></span> <span data-ttu-id="aa88f-254">如果您使用 Visual Studio，刪除*web.config*，Visual Studio 會建立一個新。</span><span class="sxs-lookup"><span data-stu-id="aa88f-254">If you are using Visual Studio and delete *web.config*, Visual Studio will create a new one.</span></span>

### <a name="additional-notes"></a><span data-ttu-id="aa88f-255">其他備註</span><span class="sxs-lookup"><span data-stu-id="aa88f-255">Additional notes</span></span>

* <span data-ttu-id="aa88f-256">相依性插入 (DI) 未設定為止之後`ConfigureServices`叫用。</span><span class="sxs-lookup"><span data-stu-id="aa88f-256">Dependency Injection (DI) is not set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="aa88f-257">組態系統不 DI 注意。</span><span class="sxs-lookup"><span data-stu-id="aa88f-257">The configuration system is not DI aware.</span></span>
* <span data-ttu-id="aa88f-258">`IConfiguration`有兩個特製化：</span><span class="sxs-lookup"><span data-stu-id="aa88f-258">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="aa88f-259">`IConfigurationRoot`用於根節點。</span><span class="sxs-lookup"><span data-stu-id="aa88f-259">`IConfigurationRoot`  Used for the root node.</span></span> <span data-ttu-id="aa88f-260">可以觸發重新載入。</span><span class="sxs-lookup"><span data-stu-id="aa88f-260">Can trigger a reload.</span></span>
  * <span data-ttu-id="aa88f-261">`IConfigurationSection`代表組態值的區段。</span><span class="sxs-lookup"><span data-stu-id="aa88f-261">`IConfigurationSection`  Represents a section of configuration values.</span></span> <span data-ttu-id="aa88f-262">`GetSection`和`GetChildren`方法會傳回`IConfigurationSection`。</span><span class="sxs-lookup"><span data-stu-id="aa88f-262">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="aa88f-263">其他資源</span><span class="sxs-lookup"><span data-stu-id="aa88f-263">Additional resources</span></span>

* [<span data-ttu-id="aa88f-264">使用多個環境</span><span class="sxs-lookup"><span data-stu-id="aa88f-264">Working with Multiple Environments</span></span>](environments.md)
* [<span data-ttu-id="aa88f-265">在開發期間安全儲存應用程式密碼</span><span class="sxs-lookup"><span data-stu-id="aa88f-265">Safe storage of app secrets during development</span></span>](../security/app-secrets.md)
* [<span data-ttu-id="aa88f-266">相依性插入</span><span class="sxs-lookup"><span data-stu-id="aa88f-266">Dependency Injection</span></span>](dependency-injection.md)
* [<span data-ttu-id="aa88f-267">Azure Key Vault 組態提供者</span><span class="sxs-lookup"><span data-stu-id="aa88f-267">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
