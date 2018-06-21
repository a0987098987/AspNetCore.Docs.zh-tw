---
title: ASP.NET Core 的設定
author: rick-anderson
description: 了解如何使用組態 API 設定 ASP.NET Core 應用程式。
ms.author: riande
ms.custom: mvc
ms.date: 01/11/2018
uid: fundamentals/configuration/index
ms.openlocfilehash: 70e9e73eeb5d08baf9ef190ebfbda998ace60d77
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278319"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="3edad-103">ASP.NET Core 的設定</span><span class="sxs-lookup"><span data-stu-id="3edad-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="3edad-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)、[Mark Michaelis](http://intellitect.com/author/mark-michaelis/)、[Steve Smith](https://ardalis.com/)、[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="3edad-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="3edad-105">組態 API 可讓您根據成對的名稱和數值清單來設定 ASP.NET Core Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="3edad-105">The Configuration API provides a way to configure an ASP.NET Core web app based on a list of name-value pairs.</span></span> <span data-ttu-id="3edad-106">組態是在執行階段讀取自多個來源。</span><span class="sxs-lookup"><span data-stu-id="3edad-106">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="3edad-107">成對的名稱和數值可以分組成多重層級階層。</span><span class="sxs-lookup"><span data-stu-id="3edad-107">Name-value pairs can be grouped into a multi-level hierarchy.</span></span>

<span data-ttu-id="3edad-108">下列項目有組態提供者：</span><span class="sxs-lookup"><span data-stu-id="3edad-108">There are configuration providers for:</span></span>

* <span data-ttu-id="3edad-109">檔案格式 (INI、JSON 及 XML)。</span><span class="sxs-lookup"><span data-stu-id="3edad-109">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="3edad-110">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="3edad-110">Command-line arguments.</span></span>
* <span data-ttu-id="3edad-111">環境變數。</span><span class="sxs-lookup"><span data-stu-id="3edad-111">Environment variables.</span></span>
* <span data-ttu-id="3edad-112">記憶體內部 .NET 物件。</span><span class="sxs-lookup"><span data-stu-id="3edad-112">In-memory .NET objects.</span></span>
* <span data-ttu-id="3edad-113">未加密的[祕密管理員](xref:security/app-secrets)儲存體。</span><span class="sxs-lookup"><span data-stu-id="3edad-113">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="3edad-114">類似 [Azure Key Vault](xref:security/key-vault-configuration)的加密使用者存放區。</span><span class="sxs-lookup"><span data-stu-id="3edad-114">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="3edad-115">自訂提供者 (已安裝或已建立)。</span><span class="sxs-lookup"><span data-stu-id="3edad-115">Custom providers (installed or created).</span></span>

<span data-ttu-id="3edad-116">每個組態值會對應至一個字串索引鍵。</span><span class="sxs-lookup"><span data-stu-id="3edad-116">Each configuration value maps to a string key.</span></span> <span data-ttu-id="3edad-117">還有內建繫結支援可將設定還原序列化為自訂 [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) 物件 (具有屬性的簡單 .NET 類別)。</span><span class="sxs-lookup"><span data-stu-id="3edad-117">There's built-in binding support to deserialize settings into a custom [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

<span data-ttu-id="3edad-118">選項模式使用選項類別來代表一組相關的設定。</span><span class="sxs-lookup"><span data-stu-id="3edad-118">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="3edad-119">如需使用選項模式的詳細資訊，請參閱[選項](xref:fundamentals/configuration/options)主題。</span><span class="sxs-lookup"><span data-stu-id="3edad-119">For more information on using the options pattern, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

<span data-ttu-id="3edad-120">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3edad-120">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="json-configuration"></a><span data-ttu-id="3edad-121">JSON 組態</span><span class="sxs-lookup"><span data-stu-id="3edad-121">JSON configuration</span></span>

<span data-ttu-id="3edad-122">下列主控台應用程式使用 JSON 組態提供者：</span><span class="sxs-lookup"><span data-stu-id="3edad-122">The following console app uses the JSON configuration provider:</span></span>

[!code-csharp[](index/sample/ConfigJson/Program.cs)]

<span data-ttu-id="3edad-123">此應用程式會讀取並顯示下列組態設定：</span><span class="sxs-lookup"><span data-stu-id="3edad-123">The app reads and displays the following configuration settings:</span></span>

[!code-json[](index/sample/ConfigJson/appsettings.json)]

<span data-ttu-id="3edad-124">組態是由成對的名稱和數值階層式清單所組成，其中節點是以冒號分隔 (`:`)。</span><span class="sxs-lookup"><span data-stu-id="3edad-124">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon (`:`).</span></span> <span data-ttu-id="3edad-125">若要擷取值，請使用對應項目的索引鍵來存取 `Configuration` 索引子：</span><span class="sxs-lookup"><span data-stu-id="3edad-125">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

[!code-csharp[](index/sample/ConfigJson/Program.cs?range=21-22)]

<span data-ttu-id="3edad-126">若要使用 JSON 格式組態來源中的陣列，請使用陣列索引作為冒號分隔字串的一部分。</span><span class="sxs-lookup"><span data-stu-id="3edad-126">To work with arrays in JSON-formatted configuration sources, use an array index as part of the colon-separated string.</span></span> <span data-ttu-id="3edad-127">下列範例會取得上述 `wizards` 陣列中第一個項目的名稱：</span><span class="sxs-lookup"><span data-stu-id="3edad-127">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}");
// Output: Gandalf
```

<span data-ttu-id="3edad-128">寫入內建[組態](/dotnet/api/microsoft.extensions.configuration)提供者的成對名稱和數值**不會**保存。</span><span class="sxs-lookup"><span data-stu-id="3edad-128">Name-value pairs written to the built-in [Configuration](/dotnet/api/microsoft.extensions.configuration) providers are **not** persisted.</span></span> <span data-ttu-id="3edad-129">不過，可以建立自訂提供者來儲存值。</span><span class="sxs-lookup"><span data-stu-id="3edad-129">However, a custom provider that saves values can be created.</span></span> <span data-ttu-id="3edad-130">請參閱[自訂組態提供者](xref:fundamentals/configuration/index#custom-config-providers)。</span><span class="sxs-lookup"><span data-stu-id="3edad-130">See [custom configuration provider](xref:fundamentals/configuration/index#custom-config-providers).</span></span>

<span data-ttu-id="3edad-131">上述範例使用 Configuration 索引子來讀取值。</span><span class="sxs-lookup"><span data-stu-id="3edad-131">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="3edad-132">若要存取 `Startup` 外部組態，請使用「選項模式」。</span><span class="sxs-lookup"><span data-stu-id="3edad-132">To access configuration outside of `Startup`, use the *options pattern*.</span></span> <span data-ttu-id="3edad-133">如需詳細資訊，請參閱[選項](xref:fundamentals/configuration/options)主題。</span><span class="sxs-lookup"><span data-stu-id="3edad-133">For more information, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

## <a name="xml-configuration"></a><span data-ttu-id="3edad-134">XML 組態</span><span class="sxs-lookup"><span data-stu-id="3edad-134">XML configuration</span></span>

<span data-ttu-id="3edad-135">若要在 XML 格式的組態來源中使用陣列，請為各元素提供 `name` 索引。</span><span class="sxs-lookup"><span data-stu-id="3edad-135">To work with arrays in XML-formatted configuration sources, provide a `name` index to each element.</span></span> <span data-ttu-id="3edad-136">使用索引存取值：</span><span class="sxs-lookup"><span data-stu-id="3edad-136">Use the index to access the values:</span></span>

```xml
<wizards>
  <wizard name="Gandalf">
    <age>1000</age>
  </wizard>
  <wizard name="Harry">
    <age>17</age>
  </wizard>
</wizards>
```

```csharp
Console.Write($"{Configuration["wizard:Harry:age"]}");
// Output: 17
```

## <a name="configuration-by-environment"></a><span data-ttu-id="3edad-137">取決於環境的組態</span><span class="sxs-lookup"><span data-stu-id="3edad-137">Configuration by environment</span></span>

<span data-ttu-id="3edad-138">不同的環境 (例如開發、測試和生產) 通常會有不同的組態設定。</span><span class="sxs-lookup"><span data-stu-id="3edad-138">It's typical to have different configuration settings for different environments, for example, development, testing, and production.</span></span> <span data-ttu-id="3edad-139">ASP.NET Core 2.x 應用程式中的 `CreateDefaultBuilder` 擴充方法 (或在 ASP.NET Core 1.x 應用程式中直接使用 `AddJsonFile` 和 `AddEnvironmentVariables`) 會新增組態提供者以讀取 JSON 檔案和系統組態來源：</span><span class="sxs-lookup"><span data-stu-id="3edad-139">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="3edad-140">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="3edad-140">*appsettings.json*</span></span>
* <span data-ttu-id="3edad-141">*appsettings.\<環境名稱>.json*</span><span class="sxs-lookup"><span data-stu-id="3edad-141">*appsettings.\<EnvironmentName>.json*</span></span>
* <span data-ttu-id="3edad-142">環境變數</span><span class="sxs-lookup"><span data-stu-id="3edad-142">Environment variables</span></span>

<span data-ttu-id="3edad-143">ASP.NET Core 1.x 應用程式需要呼叫 `AddJsonFile` 與 [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_)。</span><span class="sxs-lookup"><span data-stu-id="3edad-143">ASP.NET Core 1.x apps need to call `AddJsonFile` and [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span></span>

<span data-ttu-id="3edad-144">如需參數的說明，請參閱 [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions)。</span><span class="sxs-lookup"><span data-stu-id="3edad-144">See [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="3edad-145">`reloadOnChange` 只有在 ASP.NET Core 1.1 和更新版本中才支援。</span><span class="sxs-lookup"><span data-stu-id="3edad-145">`reloadOnChange` is only supported in ASP.NET Core 1.1 and later.</span></span>

<span data-ttu-id="3edad-146">組態來源是依其指定順序讀取。</span><span class="sxs-lookup"><span data-stu-id="3edad-146">Configuration sources are read in the order that they're specified.</span></span> <span data-ttu-id="3edad-147">在上述程式碼中，環境變數最後才會讀取。</span><span class="sxs-lookup"><span data-stu-id="3edad-147">In the preceding code, the environment variables are read last.</span></span> <span data-ttu-id="3edad-148">在環境中設定的任何組態值會取代在上述兩個提供者中設定的值。</span><span class="sxs-lookup"><span data-stu-id="3edad-148">Any configuration values set through the environment replace those set in the two previous providers.</span></span>

<span data-ttu-id="3edad-149">請考慮使用下列 *appsettings.Staging.json* 檔案：</span><span class="sxs-lookup"><span data-stu-id="3edad-149">Consider the following *appsettings.Staging.json* file:</span></span>

[!code-json[](index/sample/appsettings.Staging.json)]

<span data-ttu-id="3edad-150">在下列程式碼中，`Configure` 會讀取 `MyConfig` 的值：</span><span class="sxs-lookup"><span data-stu-id="3edad-150">In the following code, `Configure` reads the value of `MyConfig`:</span></span>

[!code-csharp[](index/sample/StartupConfig.cs?name=snippet&highlight=3,4)]

<span data-ttu-id="3edad-151">環境通常會設定為 `Development`、`Staging` 或 `Production`。</span><span class="sxs-lookup"><span data-stu-id="3edad-151">The environment is typically set to `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="3edad-152">如需詳細資訊，請參閱[使用多重環境](xref:fundamentals/environments)。</span><span class="sxs-lookup"><span data-stu-id="3edad-152">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="3edad-153">組態考量：</span><span class="sxs-lookup"><span data-stu-id="3edad-153">Configuration considerations:</span></span>

* <span data-ttu-id="3edad-154">[IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot) 可在組態資料變更時重新載入資料。</span><span class="sxs-lookup"><span data-stu-id="3edad-154">[IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot) can reload configuration data when it changes.</span></span>
* <span data-ttu-id="3edad-155">組態金鑰**不**區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="3edad-155">Configuration keys are **not** case-sensitive.</span></span>
* <span data-ttu-id="3edad-156">**永遠不要**將密碼或其他敏感性資料儲存在組態提供者程式碼或純文字組態檔中。</span><span class="sxs-lookup"><span data-stu-id="3edad-156">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="3edad-157">不要在開發或測試環境中使用生產環境祕密。</span><span class="sxs-lookup"><span data-stu-id="3edad-157">Don't use production secrets in development or test environments.</span></span> <span data-ttu-id="3edad-158">請在專案外部指定祕密，以防止其意外認可至開放原始碼存放庫。</span><span class="sxs-lookup"><span data-stu-id="3edad-158">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span> <span data-ttu-id="3edad-159">深入了解[如何使用多重環境](xref:fundamentals/environments)以及管理[開發期間安全儲存應用程式祕密](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="3edad-159">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing [safe storage of app secrets in development](xref:security/app-secrets).</span></span>
* <span data-ttu-id="3edad-160">若為環境變數中指定的階層式組態值，冒號 (`:`) 可能無法在所有平台上運作。</span><span class="sxs-lookup"><span data-stu-id="3edad-160">For hierarchical config values specified in environment variables, a colon (`:`) may not work on all platforms.</span></span> <span data-ttu-id="3edad-161">所有平台皆支援雙底線 (`__`)。</span><span class="sxs-lookup"><span data-stu-id="3edad-161">Double underscore (`__`) is supported by all platforms.</span></span>
* <span data-ttu-id="3edad-162">與組態 API 互動時，冒號 (`:`) 可在所有平台上運作。</span><span class="sxs-lookup"><span data-stu-id="3edad-162">When interacting with the configuration API, a colon (`:`) works on all platforms.</span></span>

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="3edad-163">記憶體內部提供者和 POCO 類別的繫結</span><span class="sxs-lookup"><span data-stu-id="3edad-163">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="3edad-164">下列範例示範如何使用記憶體內部提供者，並繫結至一個類別：</span><span class="sxs-lookup"><span data-stu-id="3edad-164">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

[!code-csharp[](index/sample/InMemory/Program.cs)]

<span data-ttu-id="3edad-165">組態值是以字串傳回，但繫結會啟用物件的建構。</span><span class="sxs-lookup"><span data-stu-id="3edad-165">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="3edad-166">使用繫結即可擷取 POCO 物件，甚至是整個物件圖形。</span><span class="sxs-lookup"><span data-stu-id="3edad-166">Binding allows the retrieval of POCO objects or even entire object graphs.</span></span>

### <a name="getvalue"></a><span data-ttu-id="3edad-167">GetValue</span><span class="sxs-lookup"><span data-stu-id="3edad-167">GetValue</span></span>

<span data-ttu-id="3edad-168">下列範例示範 [GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) 擴充方法：</span><span class="sxs-lookup"><span data-stu-id="3edad-168">The following sample demonstrates the [GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) extension method:</span></span>

[!code-csharp[](index/sample/InMemoryGetValue/Program.cs?highlight=31)]

<span data-ttu-id="3edad-169">使用 ConfigurationBinder 的 `GetValue<T>` 方法可指定預設值 (在範例中為 80)。</span><span class="sxs-lookup"><span data-stu-id="3edad-169">The ConfigurationBinder's `GetValue<T>` method allows the specification of a default value (80 in the sample).</span></span> <span data-ttu-id="3edad-170">`GetValue<T>` 適用於簡單的案例，並不會繫結至整個區段。</span><span class="sxs-lookup"><span data-stu-id="3edad-170">`GetValue<T>` is for simple scenarios and doesn't bind to entire sections.</span></span> <span data-ttu-id="3edad-171">`GetValue<T>` 會從轉換成特定類型的 `GetSection(key).Value` 取得純量值。</span><span class="sxs-lookup"><span data-stu-id="3edad-171">`GetValue<T>` obtains scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="3edad-172">繫結至物件圖形</span><span class="sxs-lookup"><span data-stu-id="3edad-172">Bind to an object graph</span></span>

<span data-ttu-id="3edad-173">類別中的每個物件都可以遞迴繫結。</span><span class="sxs-lookup"><span data-stu-id="3edad-173">Each object in a class can be recursively bound.</span></span> <span data-ttu-id="3edad-174">請考慮下列 `AppSettings` 類別：</span><span class="sxs-lookup"><span data-stu-id="3edad-174">Consider the following `AppSettings` class:</span></span>

[!code-csharp[](index/sample/ObjectGraph/AppSettings.cs)]

<span data-ttu-id="3edad-175">下列範例會繫結至 `AppSettings` 類別：</span><span class="sxs-lookup"><span data-stu-id="3edad-175">The following sample binds to the `AppSettings` class:</span></span>

[!code-csharp[](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

<span data-ttu-id="3edad-176">**ASP.NET Core 1.1** 和更新版本可以使用 `Get<T>`，這適用於整個區段。</span><span class="sxs-lookup"><span data-stu-id="3edad-176">**ASP.NET Core 1.1** and higher can use `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="3edad-177">`Get<T>` 可能比使用 `Bind` 更方便。</span><span class="sxs-lookup"><span data-stu-id="3edad-177">`Get<T>` can be more convenient than using `Bind`.</span></span> <span data-ttu-id="3edad-178">下列程式碼示範如何在上述範例中使用 `Get<T>`：</span><span class="sxs-lookup"><span data-stu-id="3edad-178">The following code shows how to use `Get<T>` with the preceding sample:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

<span data-ttu-id="3edad-179">使用下列 *appsettings.json* 檔案：</span><span class="sxs-lookup"><span data-stu-id="3edad-179">Using the following *appsettings.json* file:</span></span>

[!code-json[](index/sample/ObjectGraph/appsettings.json)]

<span data-ttu-id="3edad-180">此程式會顯示 `Height 11`。</span><span class="sxs-lookup"><span data-stu-id="3edad-180">The program displays `Height 11`.</span></span>

<span data-ttu-id="3edad-181">下列程式碼可用於組態的單元測試：</span><span class="sxs-lookup"><span data-stu-id="3edad-181">The following code can be used to unit test the configuration:</span></span>

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

    var settings = new AppSettings();
    config.GetSection("App").Bind(settings);

    Assert.Equal("Rick", settings.Profile.Machine);
    Assert.Equal(11, settings.Window.Height);
    Assert.Equal(11, settings.Window.Width);
    Assert.Equal("connectionstring", settings.Connection.Value);
}
```

<a name="custom-config-providers"></a>

## <a name="create-an-entity-framework-custom-provider"></a><span data-ttu-id="3edad-182">建立 Entity Framework 自訂提供者</span><span class="sxs-lookup"><span data-stu-id="3edad-182">Create an Entity Framework custom provider</span></span>

<span data-ttu-id="3edad-183">在本節中，會建立從使用 EF 的資料庫讀取成對的名稱和數值的基本組態提供者。</span><span class="sxs-lookup"><span data-stu-id="3edad-183">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span>

<span data-ttu-id="3edad-184">定義 `ConfigurationValue` 實體來將組態值儲存在資料庫中：</span><span class="sxs-lookup"><span data-stu-id="3edad-184">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

<span data-ttu-id="3edad-185">新增 `ConfigurationContext` 來儲存及存取所設定的值：</span><span class="sxs-lookup"><span data-stu-id="3edad-185">Add a `ConfigurationContext` to store and access the configured values:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="3edad-186">建立實作 [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource) 的類別：</span><span class="sxs-lookup"><span data-stu-id="3edad-186">Create a class that implements [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource):</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

<span data-ttu-id="3edad-187">透過繼承自 [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider) 來建立自訂組態提供者。</span><span class="sxs-lookup"><span data-stu-id="3edad-187">Create the custom configuration provider by inheriting from [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider).</span></span> <span data-ttu-id="3edad-188">組態提供者會將空白資料庫初始化：</span><span class="sxs-lookup"><span data-stu-id="3edad-188">The configuration provider initializes the database when it's empty:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

<span data-ttu-id="3edad-189">執行範例時，會顯示資料庫中醒目提示的值 ("value_from_ef_1" 和 "value_from_ef_2") 。</span><span class="sxs-lookup"><span data-stu-id="3edad-189">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="3edad-190">`EFConfigSource` 擴充方法可以用來新增組態來源：</span><span class="sxs-lookup"><span data-stu-id="3edad-190">An `EFConfigSource` extension method for adding the configuration source can be used:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

<span data-ttu-id="3edad-191">下列程式碼示範如何使用自訂 `EFConfigProvider`：</span><span class="sxs-lookup"><span data-stu-id="3edad-191">The following code shows how to use the custom `EFConfigProvider`:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

<span data-ttu-id="3edad-192">請注意，此範例會在 JSON 提供者後面新增自訂 `EFConfigProvider`，如此一來資料庫中的任何設定就會覆寫 *appsettings.json* 檔案中的設定。</span><span class="sxs-lookup"><span data-stu-id="3edad-192">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="3edad-193">使用下列 *appsettings.json* 檔案：</span><span class="sxs-lookup"><span data-stu-id="3edad-193">Using the following *appsettings.json* file:</span></span>

[!code-json[](index/sample/CustomConfigurationProvider/appsettings.json)]

<span data-ttu-id="3edad-194">下列輸出隨即顯示：</span><span class="sxs-lookup"><span data-stu-id="3edad-194">The following output is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="3edad-195">命令列組態提供者</span><span class="sxs-lookup"><span data-stu-id="3edad-195">CommandLine configuration provider</span></span>

<span data-ttu-id="3edad-196">[命令列組態提供者](/dotnet/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider)會在執行階段接收組態的命令列引數索引鍵/值組。</span><span class="sxs-lookup"><span data-stu-id="3edad-196">The [CommandLine configuration provider](/dotnet/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) receives command-line argument key-value pairs for configuration at runtime.</span></span>

[<span data-ttu-id="3edad-197">檢視或下載命令列組態範例</span><span class="sxs-lookup"><span data-stu-id="3edad-197">View or download the CommandLine configuration sample</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/sample/CommandLine)

### <a name="setup-and-use-the-commandline-configuration-provider"></a><span data-ttu-id="3edad-198">設定及使用命令列組態提供者</span><span class="sxs-lookup"><span data-stu-id="3edad-198">Setup and use the CommandLine configuration provider</span></span>

# <a name="basic-configurationtabbasicconfiguration"></a>[<span data-ttu-id="3edad-199">基本組態</span><span class="sxs-lookup"><span data-stu-id="3edad-199">Basic Configuration</span></span>](#tab/basicconfiguration/)

<span data-ttu-id="3edad-200">若要啟用命令列組態，請在 [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder) 的執行個體上呼叫 `AddCommandLine` 擴充方法：</span><span class="sxs-lookup"><span data-stu-id="3edad-200">To activate command-line configuration, call the `AddCommandLine` extension method on an instance of [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder):</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

<span data-ttu-id="3edad-201">執行程式碼，隨即顯示下列輸出：</span><span class="sxs-lookup"><span data-stu-id="3edad-201">Running the code, the following output is displayed:</span></span>

```console
MachineName: MairaPC
Left: 1980
```

<span data-ttu-id="3edad-202">在命令列上傳遞引數索引鍵/值組會變更 `Profile:MachineName` 和 `App:MainWindow:Left` 的值：</span><span class="sxs-lookup"><span data-stu-id="3edad-202">Passing argument key-value pairs on the command line changes the values of `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

<span data-ttu-id="3edad-203">主控台視窗會顯示：</span><span class="sxs-lookup"><span data-stu-id="3edad-203">The console window displays:</span></span>

```console
MachineName: BartPC
Left: 1979
```

<span data-ttu-id="3edad-204">若要使用命令列組態來覆寫其他組態提供者所提供的組態，請在 `ConfigurationBuilder` 上最後才呼叫 `AddCommandLine`：</span><span class="sxs-lookup"><span data-stu-id="3edad-204">To override configuration provided by other configuration providers with command-line configuration, call `AddCommandLine` last on `ConfigurationBuilder`:</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3edad-205">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3edad-205">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="3edad-206">一般 ASP.NET Core 2.x 應用程式會使用靜態簡便方法 `CreateDefaultBuilder` 來建置主應用程式：</span><span class="sxs-lookup"><span data-stu-id="3edad-206">Typical ASP.NET Core 2.x apps use the static convenience method `CreateDefaultBuilder` to build the host:</span></span>

[!code-csharp[](index/sample_snapshot//Program.cs?highlight=12)]

<span data-ttu-id="3edad-207">`CreateDefaultBuilder` 會從 *appsettings.json*、*appsettings.{Environment}.json*、[使用者祕密](xref:security/app-secrets) (在 `Development` 環境中)、環境變數和命令列引數載入選擇性組態。</span><span class="sxs-lookup"><span data-stu-id="3edad-207">`CreateDefaultBuilder` loads optional configuration from *appsettings.json*, *appsettings.{Environment}.json*, [user secrets](xref:security/app-secrets) (in the `Development` environment), environment variables, and command-line arguments.</span></span> <span data-ttu-id="3edad-208">最後才呼叫命令列組態提供者。</span><span class="sxs-lookup"><span data-stu-id="3edad-208">The CommandLine configuration provider is called last.</span></span> <span data-ttu-id="3edad-209">最後呼叫提供者可讓在執行階段傳遞的命令列引數，覆寫之前呼叫之其他組態提供者所設定的組態。</span><span class="sxs-lookup"><span data-stu-id="3edad-209">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span>

<span data-ttu-id="3edad-210">若是符合下列條件的 *appsettings* 檔案：</span><span class="sxs-lookup"><span data-stu-id="3edad-210">For *appsettings* files where:</span></span>

* <span data-ttu-id="3edad-211">`reloadOnChange` 已啟用。</span><span class="sxs-lookup"><span data-stu-id="3edad-211">`reloadOnChange` is enabled.</span></span>
* <span data-ttu-id="3edad-212">在命令列引數與 *appsettings* 檔案中包含相同設定。</span><span class="sxs-lookup"><span data-stu-id="3edad-212">Contain the same setting in the command-line arguments and an *appsettings* file.</span></span>
* <span data-ttu-id="3edad-213">包含相符命令列引數的 *appsettings* 檔案在應用程式啟動後變更。</span><span class="sxs-lookup"><span data-stu-id="3edad-213">The *appsettings* file containing the matching command-line argument is changed after the app starts.</span></span>

<span data-ttu-id="3edad-214">若上述條件皆成立，即覆寫所有命令列引數。</span><span class="sxs-lookup"><span data-stu-id="3edad-214">If all the preceding conditions are true, the command-line arguments are overridden.</span></span>

<span data-ttu-id="3edad-215">ASP.NET Core 2.x 可以使用 [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) 來代替 `CreateDefaultBuilder`。</span><span class="sxs-lookup"><span data-stu-id="3edad-215">ASP.NET Core 2.x app can use [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) instead of `CreateDefaultBuilder`.</span></span> <span data-ttu-id="3edad-216">使用 `WebHostBuilder` 時，請以 [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) 手動進行設定。</span><span class="sxs-lookup"><span data-stu-id="3edad-216">When using `WebHostBuilder`, manually set configuration with [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span></span> <span data-ttu-id="3edad-217">如需詳細資訊，請參閱 ASP.NET Core 1.x 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="3edad-217">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3edad-218">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3edad-218">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="3edad-219">建立 [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) 並呼叫 `AddCommandLine` 方法來使用命令列組態提供者。</span><span class="sxs-lookup"><span data-stu-id="3edad-219">Create a [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) and call the `AddCommandLine` method to use the CommandLine configuration provider.</span></span> <span data-ttu-id="3edad-220">最後呼叫提供者可讓在執行階段傳遞的命令列引數，覆寫之前呼叫之其他組態提供者所設定的組態。</span><span class="sxs-lookup"><span data-stu-id="3edad-220">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span> <span data-ttu-id="3edad-221">使用 `UseConfiguration` 方法將組態套用至 [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)：</span><span class="sxs-lookup"><span data-stu-id="3edad-221">Apply the configuration to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) with the `UseConfiguration` method:</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a><span data-ttu-id="3edad-222">引數</span><span class="sxs-lookup"><span data-stu-id="3edad-222">Arguments</span></span>

<span data-ttu-id="3edad-223">在命令列上傳遞的引數必須符合下表所示的兩種格式之一：</span><span class="sxs-lookup"><span data-stu-id="3edad-223">Arguments passed on the command line must conform to one of two formats shown in the following table:</span></span>

| <span data-ttu-id="3edad-224">引數格式</span><span class="sxs-lookup"><span data-stu-id="3edad-224">Argument format</span></span>                                                     | <span data-ttu-id="3edad-225">範例</span><span class="sxs-lookup"><span data-stu-id="3edad-225">Example</span></span>        |
| ------------------------------------------------------------------- | :------------: |
| <span data-ttu-id="3edad-226">單一引數：以等號 (`=`) 分隔的索引鍵/值組</span><span class="sxs-lookup"><span data-stu-id="3edad-226">Single argument: a key-value pair separated by an equals sign (`=`)</span></span> | `key1=value`   |
| <span data-ttu-id="3edad-227">連續兩個引數：以空格分隔的索引鍵/值組</span><span class="sxs-lookup"><span data-stu-id="3edad-227">Sequence of two arguments: a key-value pair separated by a space</span></span>    | `/key1 value1` |

<span data-ttu-id="3edad-228">**單一引數**</span><span class="sxs-lookup"><span data-stu-id="3edad-228">**Single argument**</span></span>

<span data-ttu-id="3edad-229">值必須在等號 (`=`) 後面。</span><span class="sxs-lookup"><span data-stu-id="3edad-229">The value must follow an equals sign (`=`).</span></span> <span data-ttu-id="3edad-230">這個值可以是 Null (例如 `mykey=`)。</span><span class="sxs-lookup"><span data-stu-id="3edad-230">The value can be null (for example, `mykey=`).</span></span>

<span data-ttu-id="3edad-231">索引鍵可能會有前置字元。</span><span class="sxs-lookup"><span data-stu-id="3edad-231">The key may have a prefix.</span></span>

| <span data-ttu-id="3edad-232">索引鍵前置字元</span><span class="sxs-lookup"><span data-stu-id="3edad-232">Key prefix</span></span>               | <span data-ttu-id="3edad-233">範例</span><span class="sxs-lookup"><span data-stu-id="3edad-233">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="3edad-234">沒有前置字元</span><span class="sxs-lookup"><span data-stu-id="3edad-234">No prefix</span></span>                | `key1=value1`   |
| <span data-ttu-id="3edad-235">單虛線 (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="3edad-235">Single dash (`-`)&#8224;</span></span> | `-key2=value2`  |
| <span data-ttu-id="3edad-236">雙虛線 (`--`)</span><span class="sxs-lookup"><span data-stu-id="3edad-236">Two dashes (`--`)</span></span>        | `--key3=value3` |
| <span data-ttu-id="3edad-237">正斜線 (`/`)</span><span class="sxs-lookup"><span data-stu-id="3edad-237">Forward slash (`/`)</span></span>      | `/key4=value4`  |

<span data-ttu-id="3edad-238">&#8224;您必須在[切換對應](#switch-mappings)中提供具有單虛線前置字元 (`-`) 的索引鍵，如下所述。</span><span class="sxs-lookup"><span data-stu-id="3edad-238">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="3edad-239">範例命令：</span><span class="sxs-lookup"><span data-stu-id="3edad-239">Example command:</span></span>

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

<span data-ttu-id="3edad-240">注意：如果提供給組態提供者的[切換對應](#switch-mappings)中沒有 `-key2`，則會擲回 `FormatException`。</span><span class="sxs-lookup"><span data-stu-id="3edad-240">Note: If `-key2` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

<span data-ttu-id="3edad-241">**連續兩個引數**</span><span class="sxs-lookup"><span data-stu-id="3edad-241">**Sequence of two arguments**</span></span>

<span data-ttu-id="3edad-242">值不可以是 Null，而且必須在以空格分隔的索引鍵後面。</span><span class="sxs-lookup"><span data-stu-id="3edad-242">The value can't be null and must follow the key separated by a space.</span></span>

<span data-ttu-id="3edad-243">索引鍵必須有前置字元。</span><span class="sxs-lookup"><span data-stu-id="3edad-243">The key must have a prefix.</span></span>

| <span data-ttu-id="3edad-244">索引鍵前置字元</span><span class="sxs-lookup"><span data-stu-id="3edad-244">Key prefix</span></span>               | <span data-ttu-id="3edad-245">範例</span><span class="sxs-lookup"><span data-stu-id="3edad-245">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="3edad-246">單虛線 (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="3edad-246">Single dash (`-`)&#8224;</span></span> | `-key1 value1`  |
| <span data-ttu-id="3edad-247">雙虛線 (`--`)</span><span class="sxs-lookup"><span data-stu-id="3edad-247">Two dashes (`--`)</span></span>        | `--key2 value2` |
| <span data-ttu-id="3edad-248">正斜線 (`/`)</span><span class="sxs-lookup"><span data-stu-id="3edad-248">Forward slash (`/`)</span></span>      | `/key3 value3`  |

<span data-ttu-id="3edad-249">&#8224;您必須在[切換對應](#switch-mappings)中提供具有單虛線前置字元 (`-`) 的索引鍵，如下所述。</span><span class="sxs-lookup"><span data-stu-id="3edad-249">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="3edad-250">範例命令：</span><span class="sxs-lookup"><span data-stu-id="3edad-250">Example command:</span></span>

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

<span data-ttu-id="3edad-251">注意：如果提供給組態提供者的[切換對應](#switch-mappings)中沒有 `-key1`，則會擲回 `FormatException`。</span><span class="sxs-lookup"><span data-stu-id="3edad-251">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

### <a name="duplicate-keys"></a><span data-ttu-id="3edad-252">重複索引鍵</span><span class="sxs-lookup"><span data-stu-id="3edad-252">Duplicate keys</span></span>

<span data-ttu-id="3edad-253">如果提供了重複索引鍵，則會使用最後一個索引鍵/值組。</span><span class="sxs-lookup"><span data-stu-id="3edad-253">If duplicate keys are provided, the last key-value pair is used.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="3edad-254">切換對應</span><span class="sxs-lookup"><span data-stu-id="3edad-254">Switch mappings</span></span>

<span data-ttu-id="3edad-255">使用 `ConfigurationBuilder` 手動建置組態時，可以將參數對應字典到 `AddCommandLine` 方法。</span><span class="sxs-lookup"><span data-stu-id="3edad-255">When manually building configuration with `ConfigurationBuilder`, a switch mappings dictionary can be added to the `AddCommandLine` method.</span></span> <span data-ttu-id="3edad-256">參數對應允許索引鍵名稱取代邏輯。</span><span class="sxs-lookup"><span data-stu-id="3edad-256">Switch mappings allow key name replacement logic.</span></span>

<span data-ttu-id="3edad-257">使用切換對應字典時，會檢查字典中是否有任何索引鍵符合命令列引數所提供的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="3edad-257">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="3edad-258">如果在字典中找到命令列索引鍵，則會傳回字典值 (索引鍵取代) 以設定組態。</span><span class="sxs-lookup"><span data-stu-id="3edad-258">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the configuration.</span></span> <span data-ttu-id="3edad-259">所有前面加上單虛線 (`-`) 的命令列索引鍵都需要切換對應。</span><span class="sxs-lookup"><span data-stu-id="3edad-259">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="3edad-260">切換對應字典索引鍵規則：</span><span class="sxs-lookup"><span data-stu-id="3edad-260">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="3edad-261">切換必須以單虛線 (`-`) 或雙虛線 (`--`) 開頭。</span><span class="sxs-lookup"><span data-stu-id="3edad-261">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="3edad-262">切換對應字典不能包含重複索引鍵。</span><span class="sxs-lookup"><span data-stu-id="3edad-262">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="3edad-263">在下列範例中，`GetSwitchMappings` 方法可讓命令列引數使用單虛線 (`-`) 索引鍵前置字元，並避免以子索引鍵前置字元開頭。</span><span class="sxs-lookup"><span data-stu-id="3edad-263">In the following example, the `GetSwitchMappings` method allows command-line arguments to use a single dash (`-`) key prefix and avoid leading subkey prefixes.</span></span>

[!code-csharp[](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

<span data-ttu-id="3edad-264">若未提供命令列引數，就會由提供給 `AddInMemoryCollection` 的字典來設定組態值。</span><span class="sxs-lookup"><span data-stu-id="3edad-264">Without providing command-line arguments, the dictionary provided to `AddInMemoryCollection` sets the configuration values.</span></span> <span data-ttu-id="3edad-265">使用下列命令來執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="3edad-265">Run the app with the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="3edad-266">主控台視窗會顯示：</span><span class="sxs-lookup"><span data-stu-id="3edad-266">The console window displays:</span></span>

```console
MachineName: RickPC
Left: 1980
```

<span data-ttu-id="3edad-267">使用下列命令在組態設定中傳遞：</span><span class="sxs-lookup"><span data-stu-id="3edad-267">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

<span data-ttu-id="3edad-268">主控台視窗會顯示：</span><span class="sxs-lookup"><span data-stu-id="3edad-268">The console window displays:</span></span>

```console
MachineName: DahliaPC
Left: 1984
```

<span data-ttu-id="3edad-269">建立參數對應字典之後，其中會包含下表所示的資料：</span><span class="sxs-lookup"><span data-stu-id="3edad-269">After the switch mappings dictionary is created, it contains the data shown in the following table:</span></span>

| <span data-ttu-id="3edad-270">Key</span><span class="sxs-lookup"><span data-stu-id="3edad-270">Key</span></span>            | <span data-ttu-id="3edad-271">值</span><span class="sxs-lookup"><span data-stu-id="3edad-271">Value</span></span>                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

<span data-ttu-id="3edad-272">若要示範如何使用字典切換索引鍵，請執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="3edad-272">To demonstrate key switching using the dictionary, run the following command:</span></span>

```console
dotnet run -MachineName=ChadPC -Left=1988
```

<span data-ttu-id="3edad-273">命令列索引鍵會互換。</span><span class="sxs-lookup"><span data-stu-id="3edad-273">The command-line keys are swapped.</span></span> <span data-ttu-id="3edad-274">主控台視窗會顯示 `Profile:MachineName` 和 `App:MainWindow:Left` 的組態值：</span><span class="sxs-lookup"><span data-stu-id="3edad-274">The console window displays the configuration values for `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
MachineName: ChadPC
Left: 1988
```

## <a name="webconfig-file"></a><span data-ttu-id="3edad-275">web.config 檔案</span><span class="sxs-lookup"><span data-stu-id="3edad-275">web.config file</span></span>

<span data-ttu-id="3edad-276">當您在 IIS 或 IIS Express 中裝載應用程式時，需要 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="3edad-276">A *web.config* file is required when hosting the app in IIS or IIS Express.</span></span> <span data-ttu-id="3edad-277">*web.config* 中的設定可讓 [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)啟動應用程式，並設定其他 IIS 設定和模組。</span><span class="sxs-lookup"><span data-stu-id="3edad-277">Settings in *web.config* enable the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to launch the app and configure other IIS settings and modules.</span></span> <span data-ttu-id="3edad-278">如果沒有 *web.config* 檔案，而專案檔包含 `<Project Sdk="Microsoft.NET.Sdk.Web">`，發行專案會在已發行的輸出 (*publish* 資料夾) 中建立 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="3edad-278">If the *web.config* file isn't present and the project file includes `<Project Sdk="Microsoft.NET.Sdk.Web">`, publishing the project creates a *web.config* file in the published output (the *publish* folder).</span></span> <span data-ttu-id="3edad-279">如需詳細資訊，請參閱[在使用 IIS 的 Windows 上裝載 ASP.NET Core](xref:host-and-deploy/iis/index#webconfig-file)。</span><span class="sxs-lookup"><span data-stu-id="3edad-279">For more information, see [Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#webconfig-file).</span></span>

## <a name="access-configuration-during-startup"></a><span data-ttu-id="3edad-280">在啟動期間存取組態</span><span class="sxs-lookup"><span data-stu-id="3edad-280">Access configuration during startup</span></span>

<span data-ttu-id="3edad-281">若要在啟動期間存取 `ConfigureServices` 或 `Configure` 內的組態，請參閱[應用程式啟動](xref:fundamentals/startup)主題中的範例。</span><span class="sxs-lookup"><span data-stu-id="3edad-281">To access configuration within `ConfigureServices` or `Configure` during startup, see the examples in the [Application startup](xref:fundamentals/startup) topic.</span></span>

## <a name="adding-configuration-from-an-external-assembly"></a><span data-ttu-id="3edad-282">從外部組件新增組態</span><span class="sxs-lookup"><span data-stu-id="3edad-282">Adding configuration from an external assembly</span></span>

<span data-ttu-id="3edad-283">實作 [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) 即可在啟動時，從應用程式非 `Startup` 類別的外部組件，對應用程式新增強功能。</span><span class="sxs-lookup"><span data-stu-id="3edad-283">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="3edad-284">如需詳細資訊，請參閱[從外部組件增強應用程式](xref:fundamentals/configuration/platform-specific-configuration)。</span><span class="sxs-lookup"><span data-stu-id="3edad-284">For more information, see [Enhance an app from an external assembly](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="access-configuration-in-a-razor-page-or-mvc-view"></a><span data-ttu-id="3edad-285">Razor 頁面或 MVC 檢視中的存取設定</span><span class="sxs-lookup"><span data-stu-id="3edad-285">Access configuration in a Razor Page or MVC view</span></span>

<span data-ttu-id="3edad-286">若要在 [Razor 頁面] 頁面或 MVC 檢視中存取組態集，請針對 [Microsoft.Extensions.Configuration 命名空間新增[使用指示詞](xref:mvc/views/razor#using) ([C# 參考：使用指示詞](/dotnet/csharp/language-reference/keywords/using-directive)) ](/dotnet/api/microsoft.extensions.configuration)，並將 [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) 插入頁面或檢視中。</span><span class="sxs-lookup"><span data-stu-id="3edad-286">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](/dotnet/api/microsoft.extensions.configuration) and inject [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) into the page or view.</span></span>

<span data-ttu-id="3edad-287">在 [Razor 頁面] 頁面中：</span><span class="sxs-lookup"><span data-stu-id="3edad-287">In a Razor Pages page:</span></span>

```cshtml
@page
@model IndexModel

@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index Page</title>
</head>
<body>
    <h1>Access configuration in a Razor Pages page</h1>
    <p>Configuration[&quot;key&quot;]: @Configuration["key"]</p>
</body>
</html>
```

<span data-ttu-id="3edad-288">在 MVC 檢視中：</span><span class="sxs-lookup"><span data-stu-id="3edad-288">In an MVC view:</span></span>

```cshtml
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index View</title>
</head>
<body>
    <h1>Access configuration in an MVC view</h1>
    <p>Configuration[&quot;key&quot;]: @Configuration["key"]</p>
</body>
</html>
```

## <a name="additional-notes"></a><span data-ttu-id="3edad-289">其他備註</span><span class="sxs-lookup"><span data-stu-id="3edad-289">Additional notes</span></span>

* <span data-ttu-id="3edad-290">相依性插入 (DI) 會在叫用 `ConfigureServices` 之後才設定。</span><span class="sxs-lookup"><span data-stu-id="3edad-290">Dependency Injection (DI) isn't set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="3edad-291">組態系統無法感知 DI。</span><span class="sxs-lookup"><span data-stu-id="3edad-291">The configuration system isn't DI aware.</span></span>
* <span data-ttu-id="3edad-292">`IConfiguration` 有兩個特製化：</span><span class="sxs-lookup"><span data-stu-id="3edad-292">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="3edad-293">`IConfigurationRoot` 用於根節點。</span><span class="sxs-lookup"><span data-stu-id="3edad-293">`IConfigurationRoot` Used for the root node.</span></span> <span data-ttu-id="3edad-294">可觸發重新載入。</span><span class="sxs-lookup"><span data-stu-id="3edad-294">Can trigger a reload.</span></span>
  * <span data-ttu-id="3edad-295">`IConfigurationSection` 代表組態值區段。</span><span class="sxs-lookup"><span data-stu-id="3edad-295">`IConfigurationSection` Represents a section of configuration values.</span></span> <span data-ttu-id="3edad-296">`GetSection` 和 `GetChildren` 方法會傳回 `IConfigurationSection`。</span><span class="sxs-lookup"><span data-stu-id="3edad-296">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>
  * <span data-ttu-id="3edad-297">在重新載入組態或存取各個提供者時使用 [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot)。</span><span class="sxs-lookup"><span data-stu-id="3edad-297">Use [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) when reloading configuration or for access to each provider.</span></span> <span data-ttu-id="3edad-298">以上皆為罕見案例。</span><span class="sxs-lookup"><span data-stu-id="3edad-298">Neither of these situations are common.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3edad-299">其他資源</span><span class="sxs-lookup"><span data-stu-id="3edad-299">Additional resources</span></span>

* [<span data-ttu-id="3edad-300">選項</span><span class="sxs-lookup"><span data-stu-id="3edad-300">Options</span></span>](xref:fundamentals/configuration/options)
* [<span data-ttu-id="3edad-301">使用多重環境</span><span class="sxs-lookup"><span data-stu-id="3edad-301">Use multiple environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="3edad-302">在開發期間安全儲存應用程式祕密</span><span class="sxs-lookup"><span data-stu-id="3edad-302">Safe storage of app secrets in development</span></span>](xref:security/app-secrets)
* [<span data-ttu-id="3edad-303">在 ASP.NET Core 中代管</span><span class="sxs-lookup"><span data-stu-id="3edad-303">Host in ASP.NET Core</span></span>](xref:fundamentals/host/index)
* [<span data-ttu-id="3edad-304">相依性插入</span><span class="sxs-lookup"><span data-stu-id="3edad-304">Dependency Injection</span></span>](xref:fundamentals/dependency-injection)
* [<span data-ttu-id="3edad-305">Azure Key Vault 組態提供者</span><span class="sxs-lookup"><span data-stu-id="3edad-305">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
