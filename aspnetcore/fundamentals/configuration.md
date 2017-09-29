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
ms.openlocfilehash: 379030df4ca91a38fce251aeaab9c5dfaf11e915
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/28/2017
---
# <a name="configuration-in-aspnet-core"></a>ASP.NET Core 的設定

[Rick Anderson](https://twitter.com/RickAndMSFT)，[標記 Michaelis](http://intellitect.com/author/mark-michaelis/)， [Steve Smith](https://ardalis.com/)，[奧 Roth](https://github.com/danroth27)，和[Luke Latham](https://github.com/guardrex)

組態 API 提供一種設定的名稱 / 值組清單為基礎的應用程式。 在執行階段從多個來源讀取組態。 名稱 / 值組可以分為多層級的階層。 有的組態提供者：

* 檔案格式 （INI、 JSON 和 XML）
* 命令列引數
* 環境變數
* 記憶體中的.NET 物件
* 加密的使用者存放區
* [Azure 金鑰保存庫](xref:security/key-vault-configuration)
* 自訂提供者，供您安裝或建立

每個組態值將對應至字串索引鍵。 沒有要還原序列化到自訂設定的內建繫結支援[POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object)物件 （具有屬性的簡單.NET 類別）。

[檢視或下載範例程式碼](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/sample)

## <a name="simple-configuration"></a>簡單的組態

下列主控台應用程式會使用 JSON 組態提供者：

[!code-csharp[Main](configuration/sample/ConfigJson/Program.cs)]

應用程式讀取，並顯示下列組態設定：

[!code-json[Main](configuration/sample/ConfigJson/appsettings.json)]

設定包含在其中的節點以冒號分隔的名稱 / 值組的階層式清單。 若要擷取值，存取`Configuration`與對應的項目索引鍵的索引子：

```csharp
Console.WriteLine($"option1 = {Configuration["subsection:suboption1"]}");
```

若要使用 JSON 格式設定來源中的陣列，使用陣列索引一部分的冒號分隔的字串。 下列範例會取得在上述第一個項目的名稱`wizards`陣列：

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}, ");
```

寫入至內建的名稱 / 值組中`Configuration`提供者是**不**保存，不過，您可以建立將值儲存的自訂提供者。 請參閱[自訂組態提供者](xref:fundamentals/configuration#custom-config-providers)。

上述範例會使用設定索引子讀取的值。 外部存取組態`Startup`，使用[選項模式](xref:fundamentals/configuration#options-config-objects)。 *選項模式*本文稍後所示。

這是通常會有不同的環境，例如開發、 測試和實際執行不同的組態設定。 `CreateDefaultBuilder` ASP.NET Core 2.x 應用程式中的擴充方法 (或使用`AddJsonFile`和`AddEnvironmentVariables`直接在 ASP.NET Core 1.x 應用程式中) 將讀取 JSON 檔案和系統設定來源的組態提供者：

* *appsettings.json*
* *appsettings。\<EnvironmentName >.json*
* 環境變數

請參閱[AddJsonFile](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.jsonconfigurationextensions)取得參數的說明。 `reloadOnChange`僅適用於 ASP.NET Core 1.1 （含） 以上。 

設定來源會讀取這些指定的順序。 上述程式碼，是上次讀取環境變數。 設定透過環境的所有組態值會都取代中的兩個先前提供者設定。

環境通常設定為其中一個`Development`， `Staging`，或`Production`。 請參閱[使用多個環境](environments.md)如需詳細資訊。

組態的考量：

* `IOptionsSnapshot`當監視變更時，就可以載入組態資料。 使用`IOptionsSnapshot`如果您要重新載入組態資料。  請參閱[IOptionsSnapshot](#ioptionssnapshot)如需詳細資訊。
* 設定索引鍵是不區分大小寫。
* 最佳作法是最後指定環境變數，讓本機的環境可以覆寫在已部署的組態檔中設定的任何項目。
* **永遠不會**將密碼或其他敏感性資料儲存在組態提供者程式碼或純文字設定檔案中。 不在您開發使用生產機密資料或測試環境。 相反地，指定外部專案樹狀結構的機密資料，讓它們無法意外認可到您的儲存機制。 深入了解[使用多個環境](environments.md)和管理[安全存放應用程式密碼，在開發期間](../security/app-secrets.md)。
* 如果`:`不能在您的系統中，以使用環境變數中，取代`:`與`__`（雙底線）。

<a name=options-config-objects></a>

## <a name="using-options-and-configuration-objects"></a>使用選項和設定物件

選項模式會使用自訂的選項類別來代表一組相關的設定。 我們建議您在應用程式中建立低耦合的每個功能類別。 請依照下列低耦合的類別：

* [介面隔離原則 」 (ISP)](http://deviq.com/interface-segregation-principle/) ： 類別只取決於它們所使用的組態設定。
* [重要性分離](http://deviq.com/separation-of-concerns/)： 設定您的應用程式的不同部分會變成相依或彼此結合。

選項類別必須是抽象的公用的無參數建構函式。 例如: 

[!code-csharp[Main](configuration/sample/UsingOptions/Models/MyOptions.cs)]

<a name=options-example></a>

在下列程式碼，被啟用的 JSON 組態提供者。 `MyOptions`類別加入至服務容器，並繫結至組態。

[!code-csharp[Main](configuration/sample/UsingOptions/Startup.cs?name=snippet1&highlight=8,20-21)]

下列[控制器](../mvc/controllers/index.md)使用[建構函式相依性插入](xref:fundamentals/dependency-injection#what-is-dependency-injection)上[ `IOptions<TOptions>` ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1)以存取設定：

[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController.cs?name=snippet1)]

以下列*appsettings.json*檔案：

[!code-json[Main](configuration/sample/UsingOptions/appsettings1.json)]

`HomeController.Index`方法會傳回`option1 = value1_from_json, option2 = 2`。

一般應用程式將不會將整個組態繫結至單一選項檔案。 稍後，我將示範如何使用`GetSection`繫結至一個區段。

下列程式碼中，第二個`IConfigureOptions<TOptions>`服務新增至服務容器。 它使用委派來設定的繫結`MyOptions`。

[!code-csharp[Main](configuration/sample/UsingOptions/Startup2.cs?name=snippet1&highlight=9-13)]

您可以加入多個組態提供者。 使用 NuGet 封裝中的組態提供者。 它們會套用順序註冊它們。

每次呼叫`Configure<TOptions>`新增`IConfigureOptions<TOptions>`服務的服務容器。 在上述範例中，值`Option1`和`Option2`中已指定*appsettings.json* -但的值`Option1`會覆寫設定的委派。 

啟用多個設定服務時，指定 「 獲勝 」，最後一個組態來源 （設定組態值）。 在上述程式碼，`HomeController.Index`方法會傳回`option1 = value1_from_action, option2 = 2`。

當您將選項設定繫結時，繫結至表單的組態機碼的選項類型中的每一個屬性`property[:sub-property:]`。 比方說，`MyOptions.Option1`屬性繫結索引鍵`Option1`，這會從讀取`option1`屬性*appsettings.json*。 子屬性範例會顯示在本文稍後。

下列程式碼，而第三個`IConfigureOptions<TOptions>`服務新增至服務容器。 它會繫結`MySubOptions`區段`subsection`的*appsettings.json*檔案：

[!code-csharp[Main](configuration/sample/UsingOptions/Startup3.cs?name=snippet1&highlight=16-17)]

注意： 這個擴充方法需要`Microsoft.Extensions.Options.ConfigurationExtensions`NuGet 封裝。

使用下列*appsettings.json*檔案：

[!code-json[Main](configuration/sample/UsingOptions/appsettings.json)]

`MySubOptions`類別：

[!code-csharp[Main](configuration/sample/UsingOptions/Models/MySubOptions.cs?name=snippet1)]

以下列`Controller`:

[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController2.cs?name=snippet1)]

`subOption1 = subvalue1_from_json, subOption2 = 200`會傳回。

您也可以提供選項，檢視模型中的，或插入`IOptions<TOptions>`直接插入檢視：

[!code-html[Main](configuration/sample/UsingOptions/Views/Home/Index.cshtml?highlight=3-4,16-17,20-21)]

<a name=in-memory-provider></a>

## <a name="ioptionssnapshot"></a>IOptionsSnapshot

*需要 ASP.NET Core 1.1 或更高版本。*

`IOptionsSnapshot`支援的組態檔變更時重新載入組態資料。 它的最少的額外負荷。 使用`IOptionsSnapshot`與`reloadOnChange: true`，選項會繫結至`IConfiguration`，變更時重新載入。

下列範例將示範如何為新`IOptionsSnapshot`之後，會建立*config.json*變更。 伺服器的要求會傳回相同時間*config.json*具有**不**變更。 第一個要求之後*config.json*變更將會顯示新的時間。

[!code-csharp[Main](configuration/sample/IOptionsSnapshot2/Startup.cs?name=snippet1&highlight=1-9,13-18,32,33,52,53)]

下圖顯示伺服器輸出：

![瀏覽器影像顯示 「 上次更新日期： 2016 年 11 月 22 下午 4:43"](configuration/_static/first.png)

重新整理瀏覽器不會變更訊息值或顯示的時間 (當*config.json*未變更)。

變更並儲存*config.json*然後重新整理瀏覽器：

![瀏覽器影像顯示 「 上次更新為 e: 2016 年 11 月 22 4:53 PM"](configuration/_static/change.png)

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a>記憶體中的提供者和繫結至 POCO 類別

下列範例會示範如何使用記憶體中的提供者，並繫結至類別：

[!code-csharp[Main](configuration/sample/InMemory/Program.cs)]

組態值會以字串傳回，但繫結啟用了物件的建構。 繫結可讓您擷取 POCO 物件或甚至整個物件圖形。 下列範例示範如何將繫結至`MyWindow`，ASP.NET Core MVC 應用程式會使用選項模式：

[!code-csharp[Main](configuration/sample/WebConfigBind/MyWindow.cs)]

[!code-json[Main](configuration/sample/WebConfigBind/appsettings.json)]

繫結中的自訂類別`ConfigureServices`建置主應用程式時：

[!code-csharp[Main](configuration/sample/WebConfigBind/Program.cs?name=snippet1&highlight=3-4)]

顯示從設定`HomeController`:

[!code-csharp[Main](configuration/sample/WebConfigBind/Controllers/HomeController.cs)]

### <a name="getvalue"></a>GetValue

下列範例會示範[GetValue<T> ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_)擴充方法：

[!code-csharp[Main](configuration/sample/InMemoryGetValue/Program.cs?highlight=27-29)]

ConfigurationBinder`GetValue<T>`方法可讓您指定的預設值 (在此範例中為 80)。 `GetValue<T>`適用於簡單的案例並不會連結到整個區段。 `GetValue<T>`取得純量值從`GetSection(key).Value`轉換為特定的型別。

## <a name="binding-to-an-object-graph"></a>繫結至物件圖形

您可以對每個物件類別中的遞迴繫結。 請考慮下列`AppOptions`類別：

[!code-csharp[Main](configuration/sample/ObjectGraph/AppOptions.cs)]

下列範例會繫結至`AppOptions`類別：

[!code-csharp[Main](configuration/sample/ObjectGraph/Program.cs?highlight=15-16)]

**ASP.NET Core 1.1**和更新版本可以使用`Get<T>`，這適用於整個區段。 `Get<T>`可以是比使用更多便利`Bind`。 下列程式碼示範如何使用`Get<T>`與上面的範例：

```csharp
var appConfig = config.GetSection("App").Get<AppOptions>();
```

使用下列*appsettings.json*檔案：

[!code-json[Main](configuration/sample/ObjectGraph/appsettings.json)]

程式會顯示`Height 11`。

下列程式碼可用於單元測試組態：

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

## <a name="basic-sample-of-entity-framework-custom-provider"></a>Entity Framework 自訂提供者的基本範例

在本節中，會建立使用 EF，從資料庫讀取名稱 / 值組的基本組態提供者。 

定義`ConfigurationValue`將組態值儲存在資料庫中的實體：

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

新增`ConfigurationContext`來儲存及存取設定的值：

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

建立類別，實作[IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

建立自訂組態提供者透過繼承自[ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider)。  當它是空的組態提供者會初始化資料庫：

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

當執行範例時，會顯示資料庫 （「 value_from_ef_1"和"value_from_ef_2"） 中反白顯示的值。

您可以加入`EFConfigSource`擴充方法新增組態來源：

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

下列程式碼示範如何使用自訂`EFConfigProvider`:

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

請注意此範例會將自訂`EFConfigProvider`後的 JSON 提供者，因此資料庫的任何設定會覆寫設定從*appsettings.json*檔案。

使用下列*appsettings.json*檔案：

[!code-json[Main](configuration/sample/CustomConfigurationProvider/appsettings.json)]

此時會顯示下列對話方塊：

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a>命令列組態提供者

[CommandLine 組態提供者](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider)接收組態在執行階段的命令列引數索引鍵-值組。

[檢視或下載的命令列組態範例](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/sample/CommandLine)

### <a name="setting-up-the-provider"></a>設定提供者

# <a name="basic-configurationtabbasicconfiguration"></a>[基本組態](#tab/basicconfiguration)

若要啟動 命令列組態，請呼叫`AddCommandLine`擴充方法的執行個體上[ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder):

[!code-csharp[Main](configuration/sample_snapshot/CommandLine/Program.cs?highlight=18,21)]

執行程式碼，會顯示下列輸出：

```console
MachineName: MairaPC
Left: 1980
```

在命令列上傳遞引數索引鍵-值組會變更值`Profile:MachineName`和`App:MainWindow:Left`:

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

在主控台視窗會顯示：

```console
MachineName: BartPC
Left: 1979
```

若要覆寫其他組態提供者所提供的命令列組態與設定，請呼叫`AddCommandLine`上最後一個`ConfigurationBuilder`:

[!code-csharp[Main](configuration/sample_snapshot/CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

典型的 ASP.NET Core 2.x 應用程式使用靜態簡便方法`CreateDefaultBuilder`建置主應用程式：

[!code-csharp[Main](configuration/sample_snapshot/Program.cs?highlight=12)]

`CreateDefaultBuilder`載入選擇性組態從*appsettings.json*， *appsettings。 {環境}.json*，[使用者密碼](xref:security/app-secrets)(在`Development`環境)，環境變數和命令列引數。 最後會呼叫的命令列組態提供者。 上次呼叫提供者允許在執行階段來覆寫組態集中的其他組態提供者所傳遞的命令列引數之前呼叫。

請注意，針對*appsettings*檔案`reloadOnChange`已啟用。 如果相符的組態值中，命令列引數會覆寫*appsettings*應用程式啟動之後變更檔案。

> [!NOTE]
> 做為使用替代`CreateDefaultBuilder`方法，建立主控件使用[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)及手動建置組態[ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder)適用於 ASP.NET Core 2.x。 請參閱 ASP.NET Core 1.x 索引標籤的詳細資訊。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

建立[ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder)呼叫`AddCommandLine`方法，以使用命令列組態提供者。 上次呼叫提供者允許在執行階段來覆寫組態集中的其他組態提供者所傳遞的命令列引數之前呼叫。 將組態套用到[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)與`UseConfiguration`方法：

[!code-csharp[Main](configuration/sample_snapshot/CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a>引數

在命令列上傳遞引數必須符合下表顯示兩種格式之一。

| 引數格式                                                     | 範例        |
| ------------------------------------------------------------------- | :------------: |
| 單一引數： 等號分隔的索引鍵-值組 (`=`) | `key1=value`   |
| 兩個引數的順序： 以空格分隔的索引鍵-值配對    | `/key1 value1` |

**單一引數**

值必須遵照等號 (`=`)。 這個值可以是 null (例如， `mykey=`)。

索引鍵可能會有前置詞。

| 索引鍵前置詞               | 範例         |
| ------------------------ | :-------------: |
| 沒有前置詞                | `key1=value1`   |
| 單一虛線 (`-`) &#8224; | `-key2=value2`  |
| 兩個破折號 (`--`)        | `--key3=value3` |
| 正斜線 (`/`)      | `/key4=value4`  |

&#8224;具有單一虛線前置詞的索引鍵 (`-`) 中必須提供[切換對應](#switch-mappings)，如下所述。

範例命令：

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

注意： 如果`-key1`不存在於[切換對應](#switch-mappings)提供給組態提供者`FormatException`就會擲回。

**兩個引數的順序**

值不能是 null，而且必須遵循以空格分隔的索引鍵。

機碼必須具有前置詞。

| 索引鍵前置詞               | 範例         |
| ------------------------ | :-------------: |
| 單一虛線 (`-`) &#8224; | `-key1 value1`  |
| 兩個破折號 (`--`)        | `--key2 value2` |
| 正斜線 (`/`)      | `/key3 value3`  |

&#8224;具有單一虛線前置詞的索引鍵 (`-`) 中必須提供[切換對應](#switch-mappings)，如下所述。

範例命令：

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

注意： 如果`-key1`不存在於[切換對應](#switch-mappings)提供給組態提供者`FormatException`就會擲回。

### <a name="duplicate-keys"></a>重複的索引鍵

如果未提供重複的索引鍵時，會使用最後一個索引鍵-值配對。

### <a name="switch-mappings"></a>參數對應

當手動建置組態`ConfigurationBuilder`，您可以選擇性地提供參數對應字典`AddCommandLine`方法。 參數對應可讓您提供的索引鍵名稱更換邏輯。

使用交換器對應字典時，會檢查字典索引鍵符合命令列引數所提供的金鑰。 如果在字典中找到的命令列的索引鍵，該字典值 （索引鍵取代） 會傳遞回來進行設定。 參數對應無須任何命令列的索引鍵，加上單一虛線 (`-`)。

切換對應字典索引鍵規則：

* 參數必須以破折號開頭 (`-`) 或雙虛線 (`--`)。
* 參數對應字典不能包含重複的索引鍵。

在下列範例中，`GetSwitchMappings`方法可讓您使用單一的虛線的命令列引數 (`-`) 索引鍵前置詞，並避免開頭的子機碼前置詞。

[!code-csharp[Main](configuration/sample/CommandLine/Program.cs?highlight=10-19,32)]

不需要提供命令列引數，提供給字典`AddInMemoryCollection`設定組態值。 執行應用程式使用下列命令：

```console
dotnet run
```

在主控台視窗會顯示：

```console
MachineName: RickPC
Left: 1980
```

使用下列組態設定中傳遞：

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

在主控台視窗會顯示：

```console
MachineName: DahliaPC
Left: 1984
```

建立參數對應字典之後，它會包含下表中所顯示的資料。

| Key            | 值                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

若要示範使用字典的索引鍵切換，請執行下列命令：

```console
dotnet run -MachineName=ChadPC -Left=1988
```

交換的命令列的索引鍵。 主控台視窗會顯示組態值`Profile:MachineName`和`App:MainWindow:Left`:

```console
MachineName: ChadPC
Left: 1988
```

## <a name="the-webconfig-file"></a>Web.config 檔案

A *web.config*檔案時，需要您裝載於 IIS 或 IIS Express 應用程式。 *web.config* AspNetCoreModule IIS 啟動您的應用程式中開啟。 中的設定*web.config*啟用 AspNetCoreModule IIS 啟動應用程式，並設定其他 IIS 設定和模組中的。 如果您使用 Visual Studio，刪除*web.config*，Visual Studio 會建立一個新。

## <a name="additional-notes"></a>其他備註

* 相依性插入 (DI) 未設定為止之後`ConfigureServices`叫用。
* 組態系統不 DI 注意。
* `IConfiguration`有兩個特製化：
  * `IConfigurationRoot`用於根節點。 可以觸發重新載入。
  * `IConfigurationSection`代表組態值的區段。 `GetSection`和`GetChildren`方法會傳回`IConfigurationSection`。

## <a name="additional-resources"></a>其他資源

* [使用多個環境](environments.md)
* [在開發期間安全儲存應用程式密碼](../security/app-secrets.md)
* [在 ASP.NET Core 中裝載](xref:fundamentals/hosting)
* [相依性插入](dependency-injection.md)
* [Azure Key Vault 組態提供者](xref:security/key-vault-configuration)
