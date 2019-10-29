---
title: ASP.NET Core 中的選項模式
author: guardrex
description: 了解如何使用選項模式來代表 ASP.NET Core 應用程式中的一組相關設定。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/28/2019
uid: fundamentals/configuration/options
ms.openlocfilehash: f9e94e8d1736b7ffaa2640aba03da6b239a34f0a
ms.sourcegitcommit: 16cf016035f0c9acf3ff0ad874c56f82e013d415
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/29/2019
ms.locfileid: "73034020"
---
# <a name="options-pattern-in-aspnet-core"></a>ASP.NET Core 中的選項模式

作者：[Luke Latham](https://github.com/guardrex)

::: moniker range=">= aspnetcore-3.0"

選項模式使用類別來代表一組相關的設定。 當[組態設定](xref:fundamentals/configuration/index)依案例隔離到不同的類別時，應用程式會遵守兩個重要的軟體工程準則：

* [介面隔離準則 (ISP) 或封裝](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; 相依於組態設定的案例 (類別) 只會相依於它們使用的組態設定。
* [關注點分離](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; (不同考量)：應用程式不同部分的設定不會彼此相依或結合。

選項也提供驗證設定資料的機制。 如需詳細資訊，請參閱[選項驗證](#options-validation)一節。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="package"></a>封裝

ASP.NET Core 應用程式中會隱含地參考[microsoft.extensions.options.configurationextensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/)套件。

## <a name="options-interfaces"></a>選項介面

<xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 是用來擷取選項並管理 `TOptions` 執行個體的選項通知。 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 支援下列案例：

* 變更通知
* [具名選項](#named-options-support-with-iconfigurenamedoptions)
* [可重新載入的設定](#reload-configuration-data-with-ioptionssnapshot)
* 選擇性選項無效判定 (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)

[設定後](#options-post-configuration)案例可讓您在所有 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 設定發生時設定或變更選項。

<xref:Microsoft.Extensions.Options.IOptionsFactory%601> 負責建立新的選項執行個體。 它有單一 <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> 方法。 預設實作會接受所有已註冊的 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 與 <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>，並先執行所有設定，接著執行設定後作業。 它會區別 <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> 和 <xref:Microsoft.Extensions.Options.IConfigureOptions%601>，且只會呼叫適當的介面。

<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> 會由 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 用來快取 `TOptions` 執行個體。 <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> 會使監視器中的選項執行個體失效，以便重新計算值 (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>)。 值可以使用 <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*> 手動導入。 <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> 方法用於應該視需要重新建立所有具名執行個體時。

<xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> 在應該於收到每個要求時重新計算選項的案例中很實用用。 如需詳細資訊，請參閱[使用 IOptionsSnapshot 重新載入設定資料](#reload-configuration-data-with-ioptionssnapshot)一節。

<xref:Microsoft.Extensions.Options.IOptions%601> 可用於支援選項。 不過，<xref:Microsoft.Extensions.Options.IOptions%601> 不支援前面的 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 案例。 您可以在現有架構與程式庫中繼續使用 <xref:Microsoft.Extensions.Options.IOptions%601>，此程式庫已使用 <xref:Microsoft.Extensions.Options.IOptions%601> 介面且不需要 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 提供的案例。

## <a name="general-options-configuration"></a>一般選項設定

一般選項設定是以範例應用程式中的範例 &num;1 來示範。

選項類別必須為非抽象，且具有公用的無參數建構函式。 下列 `MyOptions` 類別有兩個屬性，`Option1` 和 `Option2`。 設定預設值為選擇性，但在下列範例中，類別建構函式會設定 `Option1` 的預設值。 `Option2` 的預設值直接藉由初始化屬性來設定 (*Models/MyOptions.cs*)：

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

`MyOptions` 類別使用 <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> 新增到服務容器，並繫結到設定：

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example1)]

下列頁面模型使用[建構函式相依性插入](xref:mvc/controllers/dependency-injection)搭配 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 來存取設定 (*Pages/Index.cshtml.cs*)：

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

範例的 *appsettings.json* 檔案指定 `option1` 和 `option2` 的值：

[!code-json[](options/samples/3.x/OptionsSample/appsettings.json?highlight=2-3)]

當應用程式執行時，頁面模型的 `OnGet` 方法會傳回字串，顯示選項類別值：

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> 使用自訂 <xref:System.Configuration.ConfigurationBuilder> 從設定檔載入選項設定時，請確認已正確設定基底路徑：
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
> 透過 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 從設定檔載入選項設定時，不需要明確設定基底路徑。

## <a name="configure-simple-options-with-a-delegate"></a>使用委派設定簡單的選項

使用委派設定簡單的選項是以範例應用程式中的範例 &num;2 來示範。

使用委派來設定選項值。 範例應用程式使用 `MyOptionsWithDelegateConfig` 類別 (*Models/MyOptionsWithDelegateConfig.cs*)：

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

在下列程式碼中，第二個 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 服務新增至服務容器。 它使用委派，以 `MyOptionsWithDelegateConfig` 設定繫結：

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example2)]

*Index.cshtml.cs*：

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

您可以新增多個設定提供者。 設定提供者可在 NuGet 套件中找到，而且會依註冊順序套用。 如需詳細資訊，請參閱<xref:fundamentals/configuration/index>。

每次呼叫 <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> 時都會新增 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 服務到服務容器。 在上述範例中，`Option1` 和 `Option2` 的值都指定在 *appsettings.json* 中，但 `Option1` 和 `Option2` 的值會被設定的委派所覆寫。

啟用多個設定服務時，最後一個指定的設定來源會「勝出」並設定此組態值。 當應用程式執行時，頁面模型的 `OnGet` 方法會傳回字串，顯示選項類別值：

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a>子選項組態

子選項組態是以範例應用程式中的範例 &num;3 來示範。

應用程式應該建立屬於應用程式中特定案例群組 (類別) 的選項類別。 需要組態值的應用程式組件應該只能存取它們使用的設定值。

將選項繫結至組態時，選項類型中的每一個屬性都會繫結至 `property[:sub-property:]` 格式的組態索引鍵。 例如，`MyOptions.Option1` 屬性繫結至索引鍵 `Option1`，其是從 *appsettings.json* 中的 `option1` 屬性讀取。

在下列程式碼中，第三個 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 服務新增至服務容器。 它將 `MySubOptions` 繫結至 *appSettings.json* 檔案的 `subsection` 區段：

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example3)]

`GetSection` 方法需要 <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> 命名空間。

範例的 *appsettings.json* 檔案會定義 `subsection` 成員，並具有 `suboption1` 和 `suboption2` 的索引鍵：

[!code-json[](options/samples/3.x/OptionsSample/appsettings.json?highlight=4-7)]

`MySubOptions` 類別會定義屬性 `SubOption1` 和 `SubOption2`，來保存選項值 (*Models/MySubOptions.cs*)：

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

頁面模型的 `OnGet` 方法會傳回具有選項值 (*Pages/Index.cshtml.cs*) 的字串：

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

當應用程式執行時，`OnGet` 方法會傳回字串，顯示子選項類別值：

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a>檢視模型提供的選項或使用直接檢視插入提供的選項

檢視模型提供的選項或使用直接檢視插入提供的選項是以範例應用程式中的範例 &num;4 來示範。

可以在檢視模型中提供選項，或藉由將 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 直接插入至檢視來提供選項 (*Pages/Index.cshtml.cs*)：

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

範例應用程式會顯示如何使用 `@inject` 指示詞來插入 `IOptionsMonitor<MyOptions>`：

[!code-cshtml[](options/samples/3.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

執行應用程式時，轉譯的頁面中會顯示選項值：

![選項值 Option1:：value1_from_json 和 Option2: -1 是從模型藉由插入至檢視來載入。](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a>使用 IOptionsSnapshot 重新載入設定資料

使用 <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> 重新載入組態資料是以範例應用程式中的範例 &num;5 來示範。

<xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> 支援以最小的處理負擔來重新載入選項。

在要求的存留期內存取及快取選項時，會針對每個要求計算一次選項。

下列範例示範在 *appsettings.json* 變更之後如何建立新的 <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> (*Pages/Index.cshtml.cs*)。 對伺服器的多個要求會傳回 *appsettings.json* 檔案所提供的常數值，直到檔案變更並重新載入組態為止。

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

下圖顯示從 *appsettings.json* 檔案載入的初始 `option1` 和 `option2` 值：

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

將 *appsettings.json* 檔案中的值變更為 `value1_from_json UPDATED` 和 `200`。 儲存 *appsettings.json* 檔案。 重新整理瀏覽器，以查看選項值更新：

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a>IConfigureNamedOptions 的具名選項支援

<xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> 的具名選項支援是以範例應用程式中的範例 &num;6 來示範。

「具名選項」支援可讓應用程式區別具名選項組態。 在範例應用程式中，會使用 [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*) 來宣告具名選項，而前者又會呼叫 [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) 擴充方法：

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example6)]

範例應用程式會使用　<xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*) 來存取具名選項：

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

執行範例應用程式後，會傳回具名選項：

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

`named_options_1` 值會從設定提供，這會從 *appsettings.json* 檔案載入。 `named_options_2` 值是由以下提供：

* `ConfigureServices` 中 `Option1` 的 `named_options_2` 委派。
* `MyOptions` 類別所提供的 `Option2` 預設值。

## <a name="configure-all-options-with-the-configureall-method"></a>使用 ConfigureAll 方法設定所有選項

使用 <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> 方法設定所有選項執行個體。 下列程式碼會為具有共通值的所有設定執行個體設定 `Option1`。 將下列程式碼手動新增至 `Startup.ConfigureServices` 方法：

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
> 所有選項都是具名執行個體。 現有的 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 執行個體會視為以 `Options.DefaultName` 執行個體為目標，也就是 `string.Empty`。 <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> 也會實作 <xref:Microsoft.Extensions.Options.IConfigureOptions%601>。 <xref:Microsoft.Extensions.Options.IOptionsFactory%601> 的預設實作有邏輯可以適當地使用每個項目。 `null` 具名選項用來以所有具名執行個體為目標，而不是特定的具名執行個體 (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> 與 <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> 使用此慣例)。

## <a name="optionsbuilder-api"></a>OptionsBuilder API

<xref:Microsoft.Extensions.Options.OptionsBuilder%601> 會用於設定 `TOptions` 執行個體。 因為 `OptionsBuilder` 僅為初始 `AddOptions<TOptions>(string optionsName)` 呼叫的單一參數，而不是出現在所有後續呼叫的參數，所以其可簡化建立具名選項的程序。 選項驗證及接受服務依存性的 `ConfigureOptions` 多載，只可透過 `OptionsBuilder` 使用。

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a>使用 DI 服務來設定選項

在以下列兩種方式設定選項的同時，您可以從相依性插入中存取其他服務：

* 將設定委派傳遞到 [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) 上的 [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*)。 [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) 提供 [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) 的多載，可讓您最多使用五個服務來設定選項：

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* 建立您自己的類型來實作 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 或 <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>，並將該類型註冊為服務。

我們建議您將設定委派傳遞到 [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*)，因為建立服務更複雜。 建立您自己的類型相當於，當您使用 [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) 時此架構可為您執行的動作。 呼叫 [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) 會註冊暫時性泛型 <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>，其具有會接受所指定泛型服務類型的建構函式。 

## <a name="options-validation"></a>選項驗證

選項驗證可讓您在設定選項之後驗證選項。 搭配驗證方法呼叫 `Validate`，若選項有效會傳回 `true`，若為無效則傳回 `false`：

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

上述範例會將具名的選項執行個體設定為 `optionalOptionsName`。 預設選項執行個體為 `Options.DefaultName`。

當選項執行個體建立之後，便會執行驗證。 您的選項執行個體保證會在第一次存取時傳遞驗證。

> [!IMPORTANT]
> 選項一開始設定並驗證之後，選項驗證不會防範選項的修改。

`Validate` 方法可接受 `Func<TOptions, bool>`。 若要完整地自訂驗證，請實作 `IValidateOptions<TOptions>`，它允許：

* 多種選項類型的驗證：`class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`
* 取決於另一個選項類型的驗證：`public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`

`IValidateOptions` 可驗證：

* 特定的具名選項執行個體。
* 所有選項 (當 `name` 為 `null` 時)。

從以下的介面實作傳回 `ValidateOptionsResult`：

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

透過呼叫 `OptionsBuilder<TOptions>` 上的 <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> 方法，就可從 [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) 套件使用資料註解型驗證。 `Microsoft.Extensions.Options.DataAnnotations` 會在 ASP.NET Core apps 中以隱含方式參考。

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

我們考慮在之後的版本中加入積極式驗證 (啟動時快速檢錯)。

## <a name="options-post-configuration"></a>選項設定後作業

使用 <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> 來設定設定後作業。 設定後作業會在所有 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 設定發生後執行：

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> 可用來後置設定具名選項：

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

使用 <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> 後置設定所有設定執行個體：

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a>在啟動期間存取選項

<xref:Microsoft.Extensions.Options.IOptions%601> 與 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 可用於 `Startup.Configure`，因為服務是在 `Configure` 方法執行之前建置。

```csharp
public void Configure(IApplicationBuilder app, 
    IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

請勿在 `Startup.ConfigureServices` 中使用 <xref:Microsoft.Extensions.Options.IOptions%601> 或 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>。 可能會因為服務註冊的順序而有不一致的選項狀態存在。

::: moniker-end

::: moniker range="= aspnetcore-2.2"

選項模式使用類別來代表一組相關的設定。 當[組態設定](xref:fundamentals/configuration/index)依案例隔離到不同的類別時，應用程式會遵守兩個重要的軟體工程準則：

* [介面隔離準則 (ISP) 或封裝](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; 相依於組態設定的案例 (類別) 只會相依於它們使用的組態設定。
* [關注點分離](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; (不同考量)：應用程式不同部分的設定不會彼此相依或結合。

選項也提供驗證設定資料的機制。 如需詳細資訊，請參閱[選項驗證](#options-validation)一節。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Prerequisites

參考 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)，或新增 [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) 套件的套件參考。

## <a name="options-interfaces"></a>選項介面

<xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 是用來擷取選項並管理 `TOptions` 執行個體的選項通知。 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 支援下列案例：

* 變更通知
* [具名選項](#named-options-support-with-iconfigurenamedoptions)
* [可重新載入的設定](#reload-configuration-data-with-ioptionssnapshot)
* 選擇性選項無效判定 (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)

[設定後](#options-post-configuration)案例可讓您在所有 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 設定發生時設定或變更選項。

<xref:Microsoft.Extensions.Options.IOptionsFactory%601> 負責建立新的選項執行個體。 它有單一 <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> 方法。 預設實作會接受所有已註冊的 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 與 <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>，並先執行所有設定，接著執行設定後作業。 它會區別 <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> 和 <xref:Microsoft.Extensions.Options.IConfigureOptions%601>，且只會呼叫適當的介面。

<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> 會由 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 用來快取 `TOptions` 執行個體。 <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> 會使監視器中的選項執行個體失效，以便重新計算值 (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>)。 值可以使用 <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*> 手動導入。 <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> 方法用於應該視需要重新建立所有具名執行個體時。

<xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> 在應該於收到每個要求時重新計算選項的案例中很實用用。 如需詳細資訊，請參閱[使用 IOptionsSnapshot 重新載入設定資料](#reload-configuration-data-with-ioptionssnapshot)一節。

<xref:Microsoft.Extensions.Options.IOptions%601> 可用於支援選項。 不過，<xref:Microsoft.Extensions.Options.IOptions%601> 不支援前面的 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 案例。 您可以在現有架構與程式庫中繼續使用 <xref:Microsoft.Extensions.Options.IOptions%601>，此程式庫已使用 <xref:Microsoft.Extensions.Options.IOptions%601> 介面且不需要 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 提供的案例。

## <a name="general-options-configuration"></a>一般選項設定

一般選項設定是以範例應用程式中的範例 &num;1 來示範。

選項類別必須為非抽象，且具有公用的無參數建構函式。 下列 `MyOptions` 類別有兩個屬性，`Option1` 和 `Option2`。 設定預設值為選擇性，但在下列範例中，類別建構函式會設定 `Option1` 的預設值。 `Option2` 的預設值直接藉由初始化屬性來設定 (*Models/MyOptions.cs*)：

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

`MyOptions` 類別使用 <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> 新增到服務容器，並繫結到設定：

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example1)]

下列頁面模型使用[建構函式相依性插入](xref:mvc/controllers/dependency-injection)搭配 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 來存取設定 (*Pages/Index.cshtml.cs*)：

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

範例的 *appsettings.json* 檔案指定 `option1` 和 `option2` 的值：

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=2-3)]

當應用程式執行時，頁面模型的 `OnGet` 方法會傳回字串，顯示選項類別值：

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> 使用自訂 <xref:System.Configuration.ConfigurationBuilder> 從設定檔載入選項設定時，請確認已正確設定基底路徑：
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
> 透過 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 從設定檔載入選項設定時，不需要明確設定基底路徑。

## <a name="configure-simple-options-with-a-delegate"></a>使用委派設定簡單的選項

使用委派設定簡單的選項是以範例應用程式中的範例 &num;2 來示範。

使用委派來設定選項值。 範例應用程式使用 `MyOptionsWithDelegateConfig` 類別 (*Models/MyOptionsWithDelegateConfig.cs*)：

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

在下列程式碼中，第二個 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 服務新增至服務容器。 它使用委派，以 `MyOptionsWithDelegateConfig` 設定繫結：

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example2)]

*Index.cshtml.cs*：

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

您可以新增多個設定提供者。 設定提供者可在 NuGet 套件中找到，而且會依註冊順序套用。 如需詳細資訊，請參閱<xref:fundamentals/configuration/index>。

每次呼叫 <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> 時都會新增 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 服務到服務容器。 在上述範例中，`Option1` 和 `Option2` 的值都指定在 *appsettings.json* 中，但 `Option1` 和 `Option2` 的值會被設定的委派所覆寫。

啟用多個設定服務時，最後一個指定的設定來源會「勝出」並設定此組態值。 當應用程式執行時，頁面模型的 `OnGet` 方法會傳回字串，顯示選項類別值：

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a>子選項組態

子選項組態是以範例應用程式中的範例 &num;3 來示範。

應用程式應該建立屬於應用程式中特定案例群組 (類別) 的選項類別。 需要組態值的應用程式組件應該只能存取它們使用的設定值。

將選項繫結至組態時，選項類型中的每一個屬性都會繫結至 `property[:sub-property:]` 格式的組態索引鍵。 例如，`MyOptions.Option1` 屬性繫結至索引鍵 `Option1`，其是從 *appsettings.json* 中的 `option1` 屬性讀取。

在下列程式碼中，第三個 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 服務新增至服務容器。 它將 `MySubOptions` 繫結至 *appSettings.json* 檔案的 `subsection` 區段：

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example3)]

`GetSection` 方法需要 <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> 命名空間。

範例的 *appsettings.json* 檔案會定義 `subsection` 成員，並具有 `suboption1` 和 `suboption2` 的索引鍵：

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=4-7)]

`MySubOptions` 類別會定義屬性 `SubOption1` 和 `SubOption2`，來保存選項值 (*Models/MySubOptions.cs*)：

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

頁面模型的 `OnGet` 方法會傳回具有選項值 (*Pages/Index.cshtml.cs*) 的字串：

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

當應用程式執行時，`OnGet` 方法會傳回字串，顯示子選項類別值：

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a>檢視模型提供的選項或使用直接檢視插入提供的選項

檢視模型提供的選項或使用直接檢視插入提供的選項是以範例應用程式中的範例 &num;4 來示範。

可以在檢視模型中提供選項，或藉由將 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 直接插入至檢視來提供選項 (*Pages/Index.cshtml.cs*)：

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

範例應用程式會顯示如何使用 `@inject` 指示詞來插入 `IOptionsMonitor<MyOptions>`：

[!code-cshtml[](options/samples/2.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

執行應用程式時，轉譯的頁面中會顯示選項值：

![選項值 Option1:：value1_from_json 和 Option2: -1 是從模型藉由插入至檢視來載入。](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a>使用 IOptionsSnapshot 重新載入設定資料

使用 <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> 重新載入組態資料是以範例應用程式中的範例 &num;5 來示範。

<xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> 支援以最小的處理負擔來重新載入選項。

在要求的存留期內存取及快取選項時，會針對每個要求計算一次選項。

下列範例示範在 *appsettings.json* 變更之後如何建立新的 <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> (*Pages/Index.cshtml.cs*)。 對伺服器的多個要求會傳回 *appsettings.json* 檔案所提供的常數值，直到檔案變更並重新載入組態為止。

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

下圖顯示從 *appsettings.json* 檔案載入的初始 `option1` 和 `option2` 值：

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

將 *appsettings.json* 檔案中的值變更為 `value1_from_json UPDATED` 和 `200`。 儲存 *appsettings.json* 檔案。 重新整理瀏覽器，以查看選項值更新：

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a>IConfigureNamedOptions 的具名選項支援

<xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> 的具名選項支援是以範例應用程式中的範例 &num;6 來示範。

「具名選項」支援可讓應用程式區別具名選項組態。 在範例應用程式中，會使用 [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*) 來宣告具名選項，而前者又會呼叫 [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) 擴充方法：

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example6)]

範例應用程式會使用　<xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*) 來存取具名選項：

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

執行範例應用程式後，會傳回具名選項：

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

`named_options_1` 值會從設定提供，這會從 *appsettings.json* 檔案載入。 `named_options_2` 值是由以下提供：

* `ConfigureServices` 中 `Option1` 的 `named_options_2` 委派。
* `MyOptions` 類別所提供的 `Option2` 預設值。

## <a name="configure-all-options-with-the-configureall-method"></a>使用 ConfigureAll 方法設定所有選項

使用 <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> 方法設定所有選項執行個體。 下列程式碼會為具有共通值的所有設定執行個體設定 `Option1`。 將下列程式碼手動新增至 `Startup.ConfigureServices` 方法：

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
> 所有選項都是具名執行個體。 現有的 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 執行個體會視為以 `Options.DefaultName` 執行個體為目標，也就是 `string.Empty`。 <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> 也會實作 <xref:Microsoft.Extensions.Options.IConfigureOptions%601>。 <xref:Microsoft.Extensions.Options.IOptionsFactory%601> 的預設實作有邏輯可以適當地使用每個項目。 `null` 具名選項用來以所有具名執行個體為目標，而不是特定的具名執行個體 (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> 與 <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> 使用此慣例)。

## <a name="optionsbuilder-api"></a>OptionsBuilder API

<xref:Microsoft.Extensions.Options.OptionsBuilder%601> 會用於設定 `TOptions` 執行個體。 因為 `OptionsBuilder` 僅為初始 `AddOptions<TOptions>(string optionsName)` 呼叫的單一參數，而不是出現在所有後續呼叫的參數，所以其可簡化建立具名選項的程序。 選項驗證及接受服務依存性的 `ConfigureOptions` 多載，只可透過 `OptionsBuilder` 使用。

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a>使用 DI 服務來設定選項

在以下列兩種方式設定選項的同時，您可以從相依性插入中存取其他服務：

* 將設定委派傳遞到 [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) 上的 [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*)。 [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) 提供 [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) 的多載，可讓您最多使用五個服務來設定選項：

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* 建立您自己的類型來實作 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 或 <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>，並將該類型註冊為服務。

我們建議您將設定委派傳遞到 [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*)，因為建立服務更複雜。 建立您自己的類型相當於，當您使用 [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) 時此架構可為您執行的動作。 呼叫 [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) 會註冊暫時性泛型 <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>，其具有會接受所指定泛型服務類型的建構函式。 

## <a name="options-validation"></a>選項驗證

選項驗證可讓您在設定選項之後驗證選項。 搭配驗證方法呼叫 `Validate`，若選項有效會傳回 `true`，若為無效則傳回 `false`：

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

上述範例會將具名的選項執行個體設定為 `optionalOptionsName`。 預設選項執行個體為 `Options.DefaultName`。

當選項執行個體建立之後，便會執行驗證。 您的選項執行個體保證會在第一次存取時傳遞驗證。

> [!IMPORTANT]
> 選項一開始設定並驗證之後，選項驗證不會防範選項的修改。

`Validate` 方法可接受 `Func<TOptions, bool>`。 若要完整地自訂驗證，請實作 `IValidateOptions<TOptions>`，它允許：

* 多種選項類型的驗證：`class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`
* 取決於另一個選項類型的驗證：`public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`

`IValidateOptions` 可驗證：

* 特定的具名選項執行個體。
* 所有選項 (當 `name` 為 `null` 時)。

從以下的介面實作傳回 `ValidateOptionsResult`：

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

透過呼叫 `OptionsBuilder<TOptions>` 上的 <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> 方法，就可從 [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) 套件使用資料註解型驗證。 `Microsoft.Extensions.Options.DataAnnotations` 包含在[AspNetCore. App 中繼套件](xref:fundamentals/metapackage-app)中。

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

我們考慮在之後的版本中加入積極式驗證 (啟動時快速檢錯)。

## <a name="options-post-configuration"></a>選項設定後作業

使用 <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> 來設定設定後作業。 設定後作業會在所有 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 設定發生後執行：

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> 可用來後置設定具名選項：

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

使用 <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> 後置設定所有設定執行個體：

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a>在啟動期間存取選項

<xref:Microsoft.Extensions.Options.IOptions%601> 與 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 可用於 `Startup.Configure`，因為服務是在 `Configure` 方法執行之前建置。

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

請勿在 `Startup.ConfigureServices` 中使用 <xref:Microsoft.Extensions.Options.IOptions%601> 或 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>。 可能會因為服務註冊的順序而有不一致的選項狀態存在。

::: moniker-end

::: moniker range="= aspnetcore-2.1"

選項模式使用類別來代表一組相關的設定。 當[組態設定](xref:fundamentals/configuration/index)依案例隔離到不同的類別時，應用程式會遵守兩個重要的軟體工程準則：

* [介面隔離準則 (ISP) 或封裝](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; 相依於組態設定的案例 (類別) 只會相依於它們使用的組態設定。
* [關注點分離](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; (不同考量)：應用程式不同部分的設定不會彼此相依或結合。

選項也提供驗證設定資料的機制。 如需詳細資訊，請參閱[選項驗證](#options-validation)一節。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Prerequisites

參考 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)，或新增 [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) 套件的套件參考。

## <a name="options-interfaces"></a>選項介面

<xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 是用來擷取選項並管理 `TOptions` 執行個體的選項通知。 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 支援下列案例：

* 變更通知
* [具名選項](#named-options-support-with-iconfigurenamedoptions)
* [可重新載入的設定](#reload-configuration-data-with-ioptionssnapshot)
* 選擇性選項無效判定 (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)

[設定後](#options-post-configuration)案例可讓您在所有 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 設定發生時設定或變更選項。

<xref:Microsoft.Extensions.Options.IOptionsFactory%601> 負責建立新的選項執行個體。 它有單一 <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> 方法。 預設實作會接受所有已註冊的 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 與 <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>，並先執行所有設定，接著執行設定後作業。 它會區別 <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> 和 <xref:Microsoft.Extensions.Options.IConfigureOptions%601>，且只會呼叫適當的介面。

<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> 會由 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 用來快取 `TOptions` 執行個體。 <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> 會使監視器中的選項執行個體失效，以便重新計算值 (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>)。 值可以使用 <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*> 手動導入。 <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> 方法用於應該視需要重新建立所有具名執行個體時。

<xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> 在應該於收到每個要求時重新計算選項的案例中很實用用。 如需詳細資訊，請參閱[使用 IOptionsSnapshot 重新載入設定資料](#reload-configuration-data-with-ioptionssnapshot)一節。

<xref:Microsoft.Extensions.Options.IOptions%601> 可用於支援選項。 不過，<xref:Microsoft.Extensions.Options.IOptions%601> 不支援前面的 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 案例。 您可以在現有架構與程式庫中繼續使用 <xref:Microsoft.Extensions.Options.IOptions%601>，此程式庫已使用 <xref:Microsoft.Extensions.Options.IOptions%601> 介面且不需要 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 提供的案例。

## <a name="general-options-configuration"></a>一般選項設定

一般選項設定是以範例應用程式中的範例 &num;1 來示範。

選項類別必須為非抽象，且具有公用的無參數建構函式。 下列 `MyOptions` 類別有兩個屬性，`Option1` 和 `Option2`。 設定預設值為選擇性，但在下列範例中，類別建構函式會設定 `Option1` 的預設值。 `Option2` 的預設值直接藉由初始化屬性來設定 (*Models/MyOptions.cs*)：

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

`MyOptions` 類別使用 <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> 新增到服務容器，並繫結到設定：

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example1)]

下列頁面模型使用[建構函式相依性插入](xref:mvc/controllers/dependency-injection)搭配 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 來存取設定 (*Pages/Index.cshtml.cs*)：

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

範例的 *appsettings.json* 檔案指定 `option1` 和 `option2` 的值：

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=2-3)]

當應用程式執行時，頁面模型的 `OnGet` 方法會傳回字串，顯示選項類別值：

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> 使用自訂 <xref:System.Configuration.ConfigurationBuilder> 從設定檔載入選項設定時，請確認已正確設定基底路徑：
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
> 透過 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 從設定檔載入選項設定時，不需要明確設定基底路徑。

## <a name="configure-simple-options-with-a-delegate"></a>使用委派設定簡單的選項

使用委派設定簡單的選項是以範例應用程式中的範例 &num;2 來示範。

使用委派來設定選項值。 範例應用程式使用 `MyOptionsWithDelegateConfig` 類別 (*Models/MyOptionsWithDelegateConfig.cs*)：

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

在下列程式碼中，第二個 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 服務新增至服務容器。 它使用委派，以 `MyOptionsWithDelegateConfig` 設定繫結：

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example2)]

*Index.cshtml.cs*：

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

您可以新增多個設定提供者。 設定提供者可在 NuGet 套件中找到，而且會依註冊順序套用。 如需詳細資訊，請參閱<xref:fundamentals/configuration/index>。

每次呼叫 <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> 時都會新增 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 服務到服務容器。 在上述範例中，`Option1` 和 `Option2` 的值都指定在 *appsettings.json* 中，但 `Option1` 和 `Option2` 的值會被設定的委派所覆寫。

啟用多個設定服務時，最後一個指定的設定來源會「勝出」並設定此組態值。 當應用程式執行時，頁面模型的 `OnGet` 方法會傳回字串，顯示選項類別值：

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a>子選項組態

子選項組態是以範例應用程式中的範例 &num;3 來示範。

應用程式應該建立屬於應用程式中特定案例群組 (類別) 的選項類別。 需要組態值的應用程式組件應該只能存取它們使用的設定值。

將選項繫結至組態時，選項類型中的每一個屬性都會繫結至 `property[:sub-property:]` 格式的組態索引鍵。 例如，`MyOptions.Option1` 屬性繫結至索引鍵 `Option1`，其是從 *appsettings.json* 中的 `option1` 屬性讀取。

在下列程式碼中，第三個 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 服務新增至服務容器。 它將 `MySubOptions` 繫結至 *appSettings.json* 檔案的 `subsection` 區段：

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example3)]

`GetSection` 方法需要 <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> 命名空間。

範例的 *appsettings.json* 檔案會定義 `subsection` 成員，並具有 `suboption1` 和 `suboption2` 的索引鍵：

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=4-7)]

`MySubOptions` 類別會定義屬性 `SubOption1` 和 `SubOption2`，來保存選項值 (*Models/MySubOptions.cs*)：

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

頁面模型的 `OnGet` 方法會傳回具有選項值 (*Pages/Index.cshtml.cs*) 的字串：

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

當應用程式執行時，`OnGet` 方法會傳回字串，顯示子選項類別值：

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a>檢視模型提供的選項或使用直接檢視插入提供的選項

檢視模型提供的選項或使用直接檢視插入提供的選項是以範例應用程式中的範例 &num;4 來示範。

可以在檢視模型中提供選項，或藉由將 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 直接插入至檢視來提供選項 (*Pages/Index.cshtml.cs*)：

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

範例應用程式會顯示如何使用 `@inject` 指示詞來插入 `IOptionsMonitor<MyOptions>`：

[!code-cshtml[](options/samples/2.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

執行應用程式時，轉譯的頁面中會顯示選項值：

![選項值 Option1:：value1_from_json 和 Option2: -1 是從模型藉由插入至檢視來載入。](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a>使用 IOptionsSnapshot 重新載入設定資料

使用 <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> 重新載入組態資料是以範例應用程式中的範例 &num;5 來示範。

<xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> 支援以最小的處理負擔來重新載入選項。

在要求的存留期內存取及快取選項時，會針對每個要求計算一次選項。

下列範例示範在 *appsettings.json* 變更之後如何建立新的 <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> (*Pages/Index.cshtml.cs*)。 對伺服器的多個要求會傳回 *appsettings.json* 檔案所提供的常數值，直到檔案變更並重新載入組態為止。

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

下圖顯示從 *appsettings.json* 檔案載入的初始 `option1` 和 `option2` 值：

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

將 *appsettings.json* 檔案中的值變更為 `value1_from_json UPDATED` 和 `200`。 儲存 *appsettings.json* 檔案。 重新整理瀏覽器，以查看選項值更新：

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a>IConfigureNamedOptions 的具名選項支援

<xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> 的具名選項支援是以範例應用程式中的範例 &num;6 來示範。

「具名選項」支援可讓應用程式區別具名選項組態。 在範例應用程式中，會使用 [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*) 來宣告具名選項，而前者又會呼叫 [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) 擴充方法：

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example6)]

範例應用程式會使用　<xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*) 來存取具名選項：

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

執行範例應用程式後，會傳回具名選項：

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

`named_options_1` 值會從設定提供，這會從 *appsettings.json* 檔案載入。 `named_options_2` 值是由以下提供：

* `ConfigureServices` 中 `Option1` 的 `named_options_2` 委派。
* `MyOptions` 類別所提供的 `Option2` 預設值。

## <a name="configure-all-options-with-the-configureall-method"></a>使用 ConfigureAll 方法設定所有選項

使用 <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> 方法設定所有選項執行個體。 下列程式碼會為具有共通值的所有設定執行個體設定 `Option1`。 將下列程式碼手動新增至 `Startup.ConfigureServices` 方法：

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
> 所有選項都是具名執行個體。 現有的 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 執行個體會視為以 `Options.DefaultName` 執行個體為目標，也就是 `string.Empty`。 <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> 也會實作 <xref:Microsoft.Extensions.Options.IConfigureOptions%601>。 <xref:Microsoft.Extensions.Options.IOptionsFactory%601> 的預設實作有邏輯可以適當地使用每個項目。 `null` 具名選項用來以所有具名執行個體為目標，而不是特定的具名執行個體 (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> 與 <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> 使用此慣例)。

## <a name="optionsbuilder-api"></a>OptionsBuilder API

<xref:Microsoft.Extensions.Options.OptionsBuilder%601> 會用於設定 `TOptions` 執行個體。 因為 `OptionsBuilder` 僅為初始 `AddOptions<TOptions>(string optionsName)` 呼叫的單一參數，而不是出現在所有後續呼叫的參數，所以其可簡化建立具名選項的程序。 選項驗證及接受服務依存性的 `ConfigureOptions` 多載，只可透過 `OptionsBuilder` 使用。

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a>使用 DI 服務來設定選項

在以下列兩種方式設定選項的同時，您可以從相依性插入中存取其他服務：

* 將設定委派傳遞到 [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) 上的 [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*)。 [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) 提供 [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) 的多載，可讓您最多使用五個服務來設定選項：

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* 建立您自己的類型來實作 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 或 <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>，並將該類型註冊為服務。

我們建議您將設定委派傳遞到 [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*)，因為建立服務更複雜。 建立您自己的類型相當於，當您使用 [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) 時此架構可為您執行的動作。 呼叫 [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) 會註冊暫時性泛型 <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>，其具有會接受所指定泛型服務類型的建構函式。 

## <a name="options-post-configuration"></a>選項設定後作業

使用 <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> 來設定設定後作業。 設定後作業會在所有 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 設定發生後執行：

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> 可用來後置設定具名選項：

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

使用 <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> 後置設定所有設定執行個體：

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a>在啟動期間存取選項

<xref:Microsoft.Extensions.Options.IOptions%601> 與 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 可用於 `Startup.Configure`，因為服務是在 `Configure` 方法執行之前建置。

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

請勿在 `Startup.ConfigureServices` 中使用 <xref:Microsoft.Extensions.Options.IOptions%601> 或 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>。 可能會因為服務註冊的順序而有不一致的選項狀態存在。

::: moniker-end

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/configuration/index>
