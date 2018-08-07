---
title: ASP.NET Core 中的選項模式
author: guardrex
description: 了解如何使用選項模式來代表 ASP.NET Core 應用程式中的一組相關設定。
ms.author: riande
ms.custom: mvc
ms.date: 11/28/2017
uid: fundamentals/configuration/options
ms.openlocfilehash: fd3e55ec821be336501f523550f547f6049c9937
ms.sourcegitcommit: 4e34ce61e1e7f1317102b16012ce0742abf2cca6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/04/2018
ms.locfileid: "39514748"
---
# <a name="options-pattern-in-aspnet-core"></a>ASP.NET Core 中的選項模式

作者：[Luke Latham](https://github.com/guardrex)

選項模式使用類別來代表一組相關的設定。 當組態設定依功能隔離到不同的類別時，應用程式會遵守兩個重要的軟體工程準則：

* [介面隔離準則 (ISP)](http://deviq.com/interface-segregation-principle/)：相依於組態設定的功能 (類別) 只會相依於它們使用的組態設定。
* [關注點分離](http://deviq.com/separation-of-concerns/) (不同考量)：應用程式不同部分的設定不會彼此相依或結合。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([如何下載](xref:tutorials/index#how-to-download-a-sample))本文比較能輕鬆地遵循範例應用程式。

## <a name="basic-options-configuration"></a>基本選項組態

基本選項範例組態是以[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)中的範例 &num;1 來示範。

選項類別必須為非抽象，且具有公用的無參數建構函式。 下列 `MyOptions` 類別有兩個屬性，`Option1` 和 `Option2`。 設定預設值為選擇性，但在下列範例中，類別建構函式會設定 `Option1` 的預設值。 `Option2` 的預設值直接藉由初始化屬性來設定 (*Models/MyOptions.cs*)：

[!code-csharp[](options/sample/Models/MyOptions.cs?name=snippet1)]

`MyOptions` 類別使用 [Configure&lt;TOptions&gt;(IServiceCollection, IConfiguration)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsconfigurationservicecollectionextensions.configure#Microsoft_Extensions_DependencyInjection_OptionsConfigurationServiceCollectionExtensions_Configure__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_Microsoft_Extensions_Configuration_IConfiguration_) 新增至服務容器，並繫結至組態：

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example1)]

下列頁面模型使用[建構函式相依性插入](xref:mvc/controllers/dependency-injection)搭配 [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) 來存取設定 (*Pages/Index.cshtml.cs*)：

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

範例的 *appsettings.json* 檔案指定 `option1` 和 `option2` 的值：

[!code-json[](options/sample/appsettings.json?highlight=2-3)]

當應用程式執行時，頁面模型的 `OnGet` 方法會傳回字串，顯示選項類別值：

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> 使用自訂 [ConfigurationBuilder](/dotnet/api/system.configuration.configurationbuilder) 從設定檔載入選項組態時，請確認已正確設定基底路徑：
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
> 透過 [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) 從設定檔載入選項組態時，不需要明確設定基底路徑。

## <a name="configure-simple-options-with-a-delegate"></a>使用委派設定簡單的選項

使用委派設定簡單的選項是以[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)中的範例 &num;2 來示範。

使用委派來設定選項值。 範例應用程式使用 `MyOptionsWithDelegateConfig` 類別 (*Models/MyOptionsWithDelegateConfig.cs*)：

[!code-csharp[](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

在下列程式碼中，第二個 `IConfigureOptions<TOptions>` 服務新增至服務容器。 它使用委派，以 `MyOptionsWithDelegateConfig` 設定繫結：

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example2)]

*Index.cshtml.cs*：

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

您可以新增多個設定提供者。 設定提供者提供於 NuGet 套件中。 它們會以註冊的順序套用。

每次呼叫 [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) 會新增 `IConfigureOptions<TOptions>` 服務至服務容器。 在上述範例中，`Option1` 和 `Option2` 的值都指定在 *appsettings.json* 中，但 `Option1` 和 `Option2` 的值會被設定的委派所覆寫。

啟用多個設定服務時，最後一個指定的設定來源會「勝出」並設定此組態值。 當應用程式執行時，頁面模型的 `OnGet` 方法會傳回字串，顯示選項類別值：

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a>子選項組態

子選項組態是以[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)中的範例 &num;3 來示範。

應用程式應該建立屬於應用程式中特定功能群組 (類別) 的選項類別。 需要組態值的應用程式組件應該只能存取它們使用的設定值。

將選項繫結至組態時，選項類型中的每一個屬性都會繫結至 `property[:sub-property:]` 格式的組態索引鍵。 例如，`MyOptions.Option1` 屬性繫結至索引鍵 `Option1`，其是從 *appsettings.json* 中的 `option1` 屬性讀取。

在下列程式碼中，第三個 `IConfigureOptions<TOptions>` 服務新增至服務容器。 它將 `MySubOptions` 繫結至 *appSettings.json* 檔案的 `subsection` 區段：

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example3)]

`GetSection` 擴充方法需要 [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet 套件。 如果應用程式使用 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 或更新版本)，則會自動包含套件。

範例的 *appsettings.json* 檔案會定義 `subsection` 成員，並具有 `suboption1` 和 `suboption2` 的索引鍵：

[!code-json[](options/sample/appsettings.json?highlight=4-7)]

`MySubOptions` 類別會定義屬性 `SubOption1` 和 `SubOption2`，來保存子選項值 (*Models/MySubOptions.cs*)：

[!code-csharp[](options/sample/Models/MySubOptions.cs?name=snippet1)]

頁面模型的 `OnGet` 方法會傳回字串與子選項值 (*Pages/Index.cshtml.cs*)：

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

當應用程式執行時，`OnGet` 方法會傳回字串，顯示子選項類別值：

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a>檢視模型提供的選項或使用直接檢視插入提供的選項

檢視模型提供的選項或使用直接檢視插入提供的選項是以[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)中的範例 &num;4 來示範。

可以在檢視模型中提供選項，或藉由將 `IOptions<TOptions>` 直接插入至檢視來提供選項 (*Pages/Index.cshtml.cs*)：

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

對於直接插入，請使用 `@inject` 指示詞插入 `IOptions<MyOptions>`：

[!code-cshtml[](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

執行應用程式時，轉譯的頁面中會顯示選項值：

![選項值 Option1:：value1_from_json 和 Option2: -1 是從模型藉由插入至檢視來載入。](options/_static/view.png)

::: moniker range=">= aspnetcore-1.1"

## <a name="reload-configuration-data-with-ioptionssnapshot"></a>使用 IOptionsSnapshot 重新載入設定資料

使用 `IOptionsSnapshot` 重新載入組態資料是以[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)中的範例 &num;5 來示範。

[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) 支援以最小的處理負擔來重新載入選項。

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

在要求的存留期內存取及快取選項時，會針對每個要求計算一次選項。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

`IOptionsSnapshot` 是 [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) 的快照集，每當監視器根據資料來源變更觸發變更時，會自動更新。

::: moniker-end

::: moniker range=">= aspnetcore-1.1"

下列範例示範在 *appsettings.json* 變更之後如何建立新的 `IOptionsSnapshot` (*Pages/Index.cshtml.cs*)。 對伺服器的多個要求會傳回 *appsettings.json* 檔案所提供的常數值，直到檔案變更並重新載入組態為止。

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

下圖顯示從 *appsettings.json* 檔案載入的初始 `option1` 和 `option2` 值：

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

將 *appsettings.json* 檔案中的值變更為 `value1_from_json UPDATED` 和 `200`。 儲存 *appsettings.json* 檔案。 重新整理瀏覽器，以查看選項值更新：

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

## <a name="named-options-support-with-iconfigurenamedoptions"></a>IConfigureNamedOptions 的具名選項支援

[IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) 的具名選項支援是以[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)中的範例 &num;6 來示範。

「具名選項」支援可讓應用程式區別具名選項組態。 在範例應用程式中，具名選項會使用 [OptionsServiceCollectionExtensions.Configure&lt;TOptions&gt;(IServiceCollection, String, Action&lt;TOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configure) 進行宣告，而後者又會呼叫擴充方法 [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) 方法：

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example6)]

範例應用程式會使用[IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) 來存取具名選項 (*Pages/Index.cshtml.cs*)：

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

執行範例應用程式後，會傳回具名選項：

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

`named_options_1` 值會從設定提供，這會從 *appsettings.json* 檔案載入。 `named_options_2` 值是由以下提供：

* `ConfigureServices` 中 `Option1` 的 `named_options_2` 委派。
* `MyOptions` 類別所提供的 `Option2` 預設值。

使用 [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) 方法設定所有具名選項執行個體。 下列程式碼會為具有共通值的所有具名組態執行個體設定 `Option1`。 將下列程式碼手動新增至 `Configure` 方法：

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

新增程式碼之後執行範例應用程式，就會產生下列結果：

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> 所有選項都是具名執行個體。 現有的 `IConfigureOption` 執行個體會視為以 `Options.DefaultName` 執行個體為目標，也就是 `string.Empty`。 `IConfigureNamedOptions` 也會實作 `IConfigureOptions`。 [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) 的預設實作 ([參考來源](https://github.com/aspnet/Options/blob/release/2.0/src/Microsoft.Extensions.Options/IOptionsFactory.cs)) 有邏輯可適當地使用每一個。 `null` 具名選項用來以所有具名執行個體為目標，而不是特定的具名執行個體 ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) 和 [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) 使用此慣例)。

## <a name="ipostconfigureoptions"></a>IPostConfigureOptions

使用 [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1) 設定後置組態。 後置組態會在所有 [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) 組態都發生之後執行：

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) 可以用來後置設定具名選項：

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

使用 [PostConfigureAll&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) 後置設定所有具名組態執行個體：

```csharp
services.PostConfigureAll<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

::: moniker-end

## <a name="options-factory-monitoring-and-cache"></a>選項 Factory、監視和快取

[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) 用於 `TOptions` 執行個體變更時的通知。 `IOptionsMonitor` 支援可重新載入的選項、變更通知，以及 `IPostConfigureOptions`。

::: moniker range=">= aspnetcore-2.0"

[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) 負責建立新的選項執行個體。 它有一個 [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) 方法。 預設實作會採用所有已註冊的 `IConfigureOptions` 和 `IPostConfigureOptions`，並先執行所有設定，接著執行後置設定。 它會區別 `IConfigureNamedOptions` 和 `IConfigureOptions`，且只會呼叫適當的介面。

[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) 由 `IOptionsMonitor` 用來快取 `TOptions` 執行個體。 `IOptionsMonitorCache` 會使監視器中的選項執行個體失效，以便重新計算值 ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove))。 值可以手動導入，也能使用 [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd)。 [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) 方法用於應該視需要重新建立所有具名執行個體時。

::: moniker-end

## <a name="accessing-options-during-startup"></a>在啟動期間存取選項

`IOptions` 可用於 `Startup.Configure`，因為服務是在 `Configure` 方法執行之前建置。

```csharp
public void Configure(IApplicationBuilder app, IOptions<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.Value.Option1;
}
```

`IOptions` 不應該在 `Startup.ConfigureServices` 中使用。 可能會因為服務註冊的順序而有不一致的選項狀態存在。

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/configuration/index>
