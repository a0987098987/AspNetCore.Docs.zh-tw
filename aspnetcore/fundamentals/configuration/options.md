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
ms.openlocfilehash: aab96b5313a8632950e51f5586612c1d0d3d176e
ms.sourcegitcommit: 83b5a4715fd25e4eb6f7c8427c0ef03850a7fa07
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/25/2018
---
# <a name="options-pattern-in-aspnet-core"></a>在 ASP.NET Core 選項模式

作者：[Luke Latham](https://github.com/guardrex)

選項模式使用選項類別來代表一組相關的設定。 當組態設定會隔離到不同的選項類別功能時，應用程式就會遵守兩個重要的軟體工程原則：

* [介面隔離原則 」 (ISP)](http://deviq.com/interface-segregation-principle/)： 取決於組態設定的功能 （類別） 取決於它們所使用的組態設定。
* [重要性分離](http://deviq.com/separation-of-concerns/)： 應用程式的不同部分的設定不相依或彼此結合。

[檢視或下載範例程式碼](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)([如何下載](xref:tutorials/index#how-to-download-a-sample)) 這篇文章會更輕鬆地遵循範例應用程式使用。

## <a name="basic-options-configuration"></a>基本選項設定

範例示範基本的選項組態&num;1[範例應用程式](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)。

必須非抽象的選項類別。 具有公用的無參數建構函式。 下列類別`MyOptions`，有兩個屬性，`Option1`和`Option2`。 設定預設值是選擇性的但在下列範例中的類別建構函式設定的預設值`Option1`。 `Option2`直接初始化屬性所設定的預設值 (*Models/MyOptions.cs*):

[!code-csharp[Main](options/sample/Models/MyOptions.cs?name=snippet1)]

`MyOptions`類別加入至服務容器具有[IConfigureOptions&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1)和繫結至設定：

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example1)]

下列頁面上的模型使用[建構函式相依性插入](xref:fundamentals/dependency-injection#what-is-dependency-injection)與[IOptions&lt;TOptions&gt; ](/dotnet/api/Microsoft.Extensions.Options.IOptions-1)存取設定 (*Pages/Index.cshtml.cs*):

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

此範例*appsettings.json*檔案指定的值`option1`和`option2`:

[!code-json[Main](options/sample/appsettings.json)]

當應用程式執行時，頁面模型`OnGet`方法會傳回字串，顯示的選項類別值：

```html
option1 = value1_from_json, option2 = -1
```

## <a name="configure-simple-options-with-a-delegate"></a>將委派設定簡單的選項

設定簡單的選項，將委派與範例會示範&num;2[範例應用程式](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)。

您可以使用委派來設定選項值。 範例應用程式會使用`MyOptionsWithDelegateConfig`類別 (*Models/MyOptionsWithDelegateConfig.cs*):

[!code-csharp[Main](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

下列程式碼中，第二個`IConfigureOptions<TOptions>`服務新增至服務容器。 它使用委派來設定的繫結`MyOptionsWithDelegateConfig`:

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example2)]

*Index.cshtml.cs*:

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

您可以加入多個組態提供者。 使用 NuGet 封裝中的組態提供者。 它們被套用順序來進行登錄。

每次呼叫[設定&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure)新增`IConfigureOptions<TOptions>`服務的服務容器。 在上述範例中，值`Option1`和`Option2`中已指定*appsettings.json*，但值`Option1`和`Option2`會覆寫設定的委派。

啟用多個設定服務時，要指定最後一個組態來源*wins*並設定組態值。 當應用程式執行時，頁面模型`OnGet`方法會傳回字串，顯示的選項類別值：

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a>Suboptions 組態

範例將示範 suboptions 組態&num;3[範例應用程式](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)。

應用程式應該建立屬於特定的功能群組 （類別） 的應用程式中的選項類別。 應用程式需要的組態值的組件應該只能存取他們使用的組態值。

當繫結組態選項，選項類型中的每一個屬性繫結至表單的組態機碼`property[:sub-property:]`。 比方說，`MyOptions.Option1`屬性繫結索引鍵`Option1`，這會從讀取`option1`屬性*appsettings.json*。

下列程式碼，而第三個`IConfigureOptions<TOptions>`服務新增至服務容器。 它會繫結`MySubOptions`區段`subsection`的*appsettings.json*檔案：

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example3)]

`GetSection`擴充方法需要[Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet 封裝。 如果應用程式使用[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage，封裝會自動包含。

此範例*appsettings.json*檔案會定義`subsection`成員索引鍵與`suboption1`和`suboption2`:

[!code-json[Main](options/sample/appsettings.json?highlight=4-7)]

`MySubOptions`類別定義的屬性，`SubOption1`和`SubOption2`，來保存的子選項值 (*Models/MySubOptions.cs*):

[!code-csharp[Main](options/sample/Models/MySubOptions.cs?name=snippet1)]

頁面模型`OnGet`方法以傳回字串的子選項值 (*Pages/Index.cshtml.cs*):

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

當應用程式執行時，`OnGet`方法會傳回字串，顯示類別值的子選項：

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a>提供的檢視模型或直接檢視資料隱碼的選項

提供的檢視模型或直接檢視資料隱碼的選項也示範了範例&num;4[範例應用程式](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)。

您可以提供選項，檢視模型中，或是將，以便`IOptions<TOptions>`直接插入檢視 (*Pages/Index.cshtml.cs*):

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

用於直接插入插入`IOptions<MyOptions>`與`@inject`指示詞：

[!code-cshtml[Main](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

執行應用程式時，轉譯的頁面顯示的選項值：

![選項值選項 1: value1_from_json 和選項 2： 從模型和檢視插入載入-1。](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a>重新載入 IOptionsSnapshot 組態資料

重新載入組態資料與`IOptionsSnapshot`範例所示&num;5[範例應用程式](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)。

*需要 ASP.NET Core 1.1 或更新版本。*

[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1)支援重新載入選項的最小處理負擔。 在 ASP.NET Core 1.1 中，`IOptionsSnapshot`是快照集的[IOptionsMonitor&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1)和更新自動每當監視就會觸發根據的資料來源變更的變更。 ASP.NET Core 2.0 版及更新版本中，選項會針對每個要求時存取並快取要求的存留期計算一次。

下列範例會示範如何為新`IOptionsSnapshot`之後，會建立*appsettings.json*變更 (*Pages/Index.cshtml.cs*)。 伺服器的多個要求會傳回所提供的常數值*appsettings.json*直到檔案變更，並設定重新載入檔案。

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

下圖顯示初始`option1`和`option2`值載入從*appsettings.json*檔案：

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

變更中的值*appsettings.json*檔案`value1_from_json UPDATED`和`200`。 儲存*appsettings.json*檔案。 重新整理瀏覽器，以查看選項值，會更新：

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a>名為 IConfigureNamedOptions 選項支援

名為選項支援[IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1)範例會示範&num;6[範例應用程式](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)。

*需要 ASP.NET Core 2.0 或更新版本。*

*名為選項*支援可讓應用程式，以區別命名的選項設定。 範例應用程式，以宣告具名的選項[ConfigureNamedOptions&lt;TOptions&gt;。設定](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure)方法：

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example6)]

範例應用程式存取具名的選項與[IOptionsSnapshot&lt;TOptions&gt;。取得](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get)(*Pages/Index.cshtml.cs*):

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

執行範例應用程式，會傳回具名的選項：

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

`named_options_1`值會提供從組態中，這會從載入*appsettings.json*檔案。 `named_options_2`所提供的值：

* `named_options_2`委派中`ConfigureServices`如`Option1`。
* 預設值為`Option2`提供`MyOptions`類別。

設定所有具有選項的具名執行個體[OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall)方法。 下列程式碼會設定`Option1`所有名為具有共通的值的組態執行個體。 手動加入下列程式碼`Configure`方法：

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

執行範例應用程式加入程式碼之後，就會產生下列結果：

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> ASP.NET Core 2.0 版及更新版本中，所有選項都被都具名執行個體。 現有`IConfigureOption`執行個體視為目標`Options.DefaultName`執行個體，也就是`string.Empty`。 `IConfigureNamedOptions`也會實作`IConfigureOptions`。 預設實作[IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([參考來源](https://github.com/aspnet/Options/blob/release/2.0.0/src/Microsoft.Extensions.Options/OptionsFactory.cs)) 已適當地使用每個邏輯。 `null`具名的選項用來為目標的所有具名的執行個體，而不是特定的具名執行個體 ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall)和[PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall)使用此慣例時)。

## <a name="ipostconfigureoptions"></a>IPostConfigureOptions

*需要 ASP.NET Core 2.0 或更新版本。*

設定與 postconfiguration [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1)。 在所有執行 postconfiguration [IConfigureOptions&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1)便會進行設定：

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

[PostConfigure&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure)是可供後續設定具名的選項：

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

使用[PostConfigureAll&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall)後設定所有名為組態執行個體：

```csharp
services.PostConfigureAll<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="options-factory-monitoring-and-cache"></a>選項 factory、 監視和快取

[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1)用於通知時`TOptions`變更執行個體。 `IOptionsMonitor`支援 reloadable 選項，變更通知，以及`IPostConfigureOptions`。

[IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) (ASP.NET Core 2.0 或更新版本) 負責建立新的選項執行個體。 它有單一[建立](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create)方法。 預設實作會採用所有已註冊`IConfigureOptions`和`IPostConfigureOptions`並執行所有可設定第一次，後面接著後設定。 它會區別`IConfigureNamedOptions`和`IConfigureOptions`，只會呼叫適當的介面。

[IOptionsMonitorCache&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (ASP.NET Core 2.0 或更新版本) 由`IOptionsMonitor`至快取`TOptions`執行個體。 `IOptionsMonitorCache`失效選項在監視器中的執行個體，以便重新計算值 ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove))。 值可以手動導入以及使用[TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd)。 [清除](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear)方法應在需要重新建立所有具名執行個體時使用。

## <a name="accessing-options-during-startup"></a>在啟動期間存取選項

`IOptions`可用於`Configure`，因為服務建立之前`Configure`方法執行。 如果內建的服務提供者`ConfigureServices`若要存取選項，它不包含任何選項之後，內建的服務提供者提供的組態。 因此，可能存在選項不一致狀態，因為服務登錄裝置排序。

組態選項通常會從組態載入，因為可以用於啟動於兩者`Configure`和`ConfigureServices`。 如需使用在啟動期間設定的範例，請參閱[應用程式啟動](xref:fundamentals/startup)主題。

## <a name="see-also"></a>另請參閱

* [組態](xref:fundamentals/configuration/index)
