---
title: ASP.NET Core 的設定
author: rick-anderson
description: 透過多種方法來使用組態 API 設定 ASP.NET Core 應用程式。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/configuration/index
ms.openlocfilehash: b1c2b734a2e9b274792b597bfd222c31e661b0d7
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/03/2018
---
# <a name="configuration-in-aspnet-core"></a>ASP.NET Core 的設定

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)、[Mark Michaelis](http://intellitect.com/author/mark-michaelis/)、[Steve Smith](https://ardalis.com/)、[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)

組態 API 可讓您根據成對的名稱和數值清單來設定 ASP.NET Core Web 應用程式。 組態是在執行階段讀取自多個來源。 成對的名稱和數值可以分組成多重層級階層。

下列項目有組態提供者：

* 檔案格式 (INI、JSON 及 XML)。
* 命令列引數。
* 環境變數。
* 記憶體內部 .NET 物件。
* 未加密的[祕密管理員](xref:security/app-secrets)儲存體。
* 類似 [Azure Key Vault](xref:security/key-vault-configuration)的加密使用者存放區。
* 自訂提供者 (已安裝或已建立)。

每個組態值會對應至一個字串索引鍵。 還有內建繫結支援可將設定還原序列化為自訂 [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) 物件 (具有屬性的簡單 .NET 類別)。

選項模式使用選項類別來代表一組相關的設定。 如需使用選項模式的詳細資訊，請參閱[選項](xref:fundamentals/configuration/options)主題。

[檢視或下載範例程式碼](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="json-configuration"></a>JSON 組態

下列主控台應用程式使用 JSON 組態提供者：

[!code-csharp[](index/sample/ConfigJson/Program.cs)]

此應用程式會讀取並顯示下列組態設定：

[!code-json[](index/sample/ConfigJson/appsettings.json)]

組態是由成對的名稱和數值階層式清單所組成，其中節點是以冒號分隔 (`:`)。 若要擷取值，請使用對應項目的索引鍵來存取 `Configuration` 索引子：

[!code-csharp[](index/sample/ConfigJson/Program.cs?range=21-22)]

若要使用 JSON 格式組態來源中的陣列，請使用陣列索引作為冒號分隔字串的一部分。 下列範例會取得上述 `wizards` 陣列中第一個項目的名稱：

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}");
// Output: Gandalf
```

寫入內建[組態](/dotnet/api/microsoft.extensions.configuration)提供者的成對名稱和數值**不會**保存。 不過，可以建立自訂提供者來儲存值。 請參閱[自訂組態提供者](xref:fundamentals/configuration/index#custom-config-providers)。

上述範例使用 Configuration 索引子來讀取值。 若要存取 `Startup` 外部組態，請使用「選項模式」。 如需詳細資訊，請參閱[選項](xref:fundamentals/configuration/options)主題。

## <a name="xml-configuration"></a>XML 組態

若要在 XML 格式的組態來源中使用陣列，請為各元素提供 `name` 索引。 使用索引存取值：

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

## <a name="configuration-by-environment"></a>取決於環境的組態

不同的環境 (例如開發、測試和生產) 通常會有不同的組態設定。 ASP.NET Core 2.x 應用程式中的 `CreateDefaultBuilder` 擴充方法 (或在 ASP.NET Core 1.x 應用程式中直接使用 `AddJsonFile` 和 `AddEnvironmentVariables`) 會新增組態提供者以讀取 JSON 檔案和系統組態來源：

* *appsettings.json*
* *appsettings.\<環境名稱>.json*
* 環境變數

ASP.NET Core 1.x 應用程式需要呼叫 `AddJsonFile` 與 [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_)。

如需參數的說明，請參閱 [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions)。 `reloadOnChange` 只有在 ASP.NET Core 1.1 和更新版本中才支援。

組態來源是依其指定順序讀取。 在上述程式碼中，環境變數最後才會讀取。 在環境中設定的任何組態值會取代在上述兩個提供者中設定的值。

請考慮使用下列 *appsettings.Staging.json* 檔案：

[!code-json[](index/sample/appsettings.Staging.json)]

當環境設定為 `Staging` 時，下列 `Configure` 方法會讀取 `MyConfig` 的值：

[!code-csharp[](index/sample/StartupConfig.cs?name=snippet&highlight=3,4)]

環境通常會設定為 `Development`、`Staging` 或 `Production`。 如需詳細資訊，請參閱[使用多個環境](xref:fundamentals/environments)。

組態考量：

* [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot) 可在組態資料變更時重新載入資料。
* 組態金鑰**不**區分大小寫。
* **永遠不要**將密碼或其他敏感性資料儲存在組態提供者程式碼或純文字組態檔中。 不要在開發或測試環境中使用生產環境祕密。 請在專案外部指定祕密，以防止其意外認可至開放原始碼存放庫。 深入了解[如何使用多個環境](xref:fundamentals/environments)及管理[在開發期間安全儲存應用程式祕密](xref:security/app-secrets)。
* 若為環境變數中指定的階層式組態值，冒號 (`:`) 可能無法在所有平台上運作。 所有平台皆支援雙底線 (`__`)。
* 與組態 API 互動時，冒號 (`:`) 可在所有平台上運作。

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a>記憶體內部提供者和 POCO 類別的繫結

下列範例示範如何使用記憶體內部提供者，並繫結至一個類別：

[!code-csharp[](index/sample/InMemory/Program.cs)]

組態值是以字串傳回，但繫結會啟用物件的建構。 使用繫結即可擷取 POCO 物件，甚至是整個物件圖形。

### <a name="getvalue"></a>GetValue

下列範例示範 [GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) 擴充方法：

[!code-csharp[](index/sample/InMemoryGetValue/Program.cs?highlight=31)]

使用 ConfigurationBinder 的 `GetValue<T>` 方法可指定預設值 (在範例中為 80)。 `GetValue<T>` 適用於簡單的案例，並不會繫結至整個區段。 `GetValue<T>` 會從轉換成特定類型的 `GetSection(key).Value` 取得純量值。

## <a name="bind-to-an-object-graph"></a>繫結至物件圖形

類別中的每個物件都可以遞迴繫結。 請考慮下列 `AppSettings` 類別：

[!code-csharp[](index/sample/ObjectGraph/AppSettings.cs)]

下列範例會繫結至 `AppSettings` 類別：

[!code-csharp[](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

**ASP.NET Core 1.1** 和更新版本可以使用 `Get<T>`，這適用於整個區段。 `Get<T>` 可能比使用 `Bind` 更方便。 下列程式碼示範如何在上述範例中使用 `Get<T>`：

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

使用下列 *appsettings.json* 檔案：

[!code-json[](index/sample/ObjectGraph/appsettings.json)]

此程式會顯示 `Height 11`。

下列程式碼可用於組態的單元測試：

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

## <a name="create-an-entity-framework-custom-provider"></a>建立 Entity Framework 自訂提供者

在本節中，會建立從使用 EF 的資料庫讀取成對的名稱和數值的基本組態提供者。

定義 `ConfigurationValue` 實體來將組態值儲存在資料庫中：

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

新增 `ConfigurationContext` 來儲存及存取所設定的值：

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

建立實作 [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource) 的類別：

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

透過繼承自 [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider) 來建立自訂組態提供者。 組態提供者會將空白資料庫初始化：

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

執行範例時，會顯示資料庫中醒目提示的值 ("value_from_ef_1" 和 "value_from_ef_2") 。

`EFConfigSource` 擴充方法可以用來新增組態來源：

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

下列程式碼示範如何使用自訂 `EFConfigProvider`：

[!code-csharp[](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

請注意，此範例會在 JSON 提供者後面新增自訂 `EFConfigProvider`，如此一來資料庫中的任何設定就會覆寫 *appsettings.json* 檔案中的設定。

使用下列 *appsettings.json* 檔案：

[!code-json[](index/sample/CustomConfigurationProvider/appsettings.json)]

下列輸出隨即顯示：

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a>命令列組態提供者

[命令列組態提供者](/dotnet/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider)會在執行階段接收組態的命令列引數索引鍵/值組。

[檢視或下載命令列組態範例](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/index/sample/CommandLine)

### <a name="setup-and-use-the-commandline-configuration-provider"></a>設定及使用命令列組態提供者

#### <a name="basic-configurationtabbasicconfiguration"></a>[基本組態](#tab/basicconfiguration/)
若要啟用命令列組態，請在 [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder) 的執行個體上呼叫 `AddCommandLine` 擴充方法：

[!code-csharp[](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

執行程式碼，隨即顯示下列輸出：

```console
MachineName: MairaPC
Left: 1980
```

在命令列上傳遞引數索引鍵/值組會變更 `Profile:MachineName` 和 `App:MainWindow:Left` 的值：

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

主控台視窗會顯示：

```console
MachineName: BartPC
Left: 1979
```

若要使用命令列組態來覆寫其他組態提供者所提供的組態，請在 `ConfigurationBuilder` 上最後才呼叫 `AddCommandLine`：

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
一般 ASP.NET Core 2.x 應用程式會使用靜態簡便方法 `CreateDefaultBuilder` 來建置主應用程式：

[!code-csharp[](index/sample_snapshot//Program.cs?highlight=12)]

`CreateDefaultBuilder` 會從 *appsettings.json*、*appsettings.{Environment}.json*、[使用者祕密](xref:security/app-secrets) (在 `Development` 環境中)、環境變數和命令列引數載入選擇性組態。 最後才呼叫命令列組態提供者。 最後呼叫提供者可讓在執行階段傳遞的命令列引數，覆寫之前呼叫之其他組態提供者所設定的組態。

若是符合下列條件的 *appsettings* 檔案：

* `reloadOnChange` 已啟用。
* 在命令列引數與 *appsettings* 檔案中包含相同設定。
* 包含相符命令列引數的 *appsettings* 檔案在應用程式啟動後變更。

若上述條件皆成立，即覆寫所有命令列引數。

ASP.NET Core 2.x 可以使用 [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) 來代替 `CreateDefaultBuilder`。 使用 `WebHostBuilder` 時，請以 [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) 手動進行設定。 如需詳細資訊，請參閱 ASP.NET Core 1.x 索引標籤。

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
建立 [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) 並呼叫 `AddCommandLine` 方法來使用命令列組態提供者。 最後呼叫提供者可讓在執行階段傳遞的命令列引數，覆寫之前呼叫之其他組態提供者所設定的組態。 使用 `UseConfiguration` 方法將組態套用至 [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)：

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

* * *
### <a name="arguments"></a>引數

在命令列上傳遞的引數必須符合下表所示的兩種格式之一：

| 引數格式                                                     | 範例        |
| ------------------------------------------------------------------- | :------------: |
| 單一引數：以等號 (`=`) 分隔的索引鍵/值組 | `key1=value`   |
| 連續兩個引數：以空格分隔的索引鍵/值組    | `/key1 value1` |

**單一引數**

值必須在等號 (`=`) 後面。 這個值可以是 Null (例如 `mykey=`)。

索引鍵可能會有前置字元。

| 索引鍵前置字元               | 範例         |
| ------------------------ | :-------------: |
| 沒有前置字元                | `key1=value1`   |
| 單虛線 (`-`)&#8224; | `-key2=value2`  |
| 雙虛線 (`--`)        | `--key3=value3` |
| 正斜線 (`/`)      | `/key4=value4`  |

&#8224;您必須在[切換對應](#switch-mappings)中提供具有單虛線前置字元 (`-`) 的索引鍵，如下所述。

範例命令：

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

注意：如果提供給組態提供者的[切換對應](#switch-mappings)中沒有 `-key2`，則會擲回 `FormatException`。

**連續兩個引數**

值不可以是 Null，而且必須在以空格分隔的索引鍵後面。

索引鍵必須有前置字元。

| 索引鍵前置字元               | 範例         |
| ------------------------ | :-------------: |
| 單虛線 (`-`)&#8224; | `-key1 value1`  |
| 雙虛線 (`--`)        | `--key2 value2` |
| 正斜線 (`/`)      | `/key3 value3`  |

&#8224;您必須在[切換對應](#switch-mappings)中提供具有單虛線前置字元 (`-`) 的索引鍵，如下所述。

範例命令：

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

注意：如果提供給組態提供者的[切換對應](#switch-mappings)中沒有 `-key1`，則會擲回 `FormatException`。

### <a name="duplicate-keys"></a>重複索引鍵

如果提供了重複索引鍵，則會使用最後一個索引鍵/值組。

### <a name="switch-mappings"></a>切換對應

使用 `ConfigurationBuilder` 手動建置組態時，可以將參數對應字典到 `AddCommandLine` 方法。 參數對應允許索引鍵名稱取代邏輯。

使用切換對應字典時，會檢查字典中是否有任何索引鍵符合命令列引數所提供的索引鍵。 如果在字典中找到命令列索引鍵，則會傳回字典值 (索引鍵取代) 以設定組態。 所有前面加上單虛線 (`-`) 的命令列索引鍵都需要切換對應。

切換對應字典索引鍵規則：

* 切換必須以單虛線 (`-`) 或雙虛線 (`--`) 開頭。
* 切換對應字典不能包含重複索引鍵。

在下列範例中，`GetSwitchMappings` 方法可讓命令列引數使用單虛線 (`-`) 索引鍵前置字元，並避免以子索引鍵前置字元開頭。

[!code-csharp[](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

若未提供命令列引數，就會由提供給 `AddInMemoryCollection` 的字典來設定組態值。 使用下列命令來執行應用程式：

```console
dotnet run
```

主控台視窗會顯示：

```console
MachineName: RickPC
Left: 1980
```

使用下列命令在組態設定中傳遞：

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

主控台視窗會顯示：

```console
MachineName: DahliaPC
Left: 1984
```

建立參數對應字典之後，其中會包含下表所示的資料：

| Key            | 值                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

若要示範如何使用字典切換索引鍵，請執行下列命令：

```console
dotnet run -MachineName=ChadPC -Left=1988
```

命令列索引鍵會互換。 主控台視窗會顯示 `Profile:MachineName` 和 `App:MainWindow:Left` 的組態值：

```console
MachineName: ChadPC
Left: 1988
```

## <a name="webconfig-file"></a>web.config 檔案

當您在 IIS 或 IIS Express 中裝載應用程式時，需要 *web.config* 檔案。 *web.config* 中的設定可讓 [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)啟動應用程式，並設定其他 IIS 設定和模組。 如果沒有 *web.config* 檔案，而專案檔包含 `<Project Sdk="Microsoft.NET.Sdk.Web">`，發行專案會在已發行的輸出 (*publish* 資料夾) 中建立 *web.config* 檔案。 如需詳細資訊，請參閱[在使用 IIS 的 Windows 上裝載 ASP.NET Core](xref:host-and-deploy/iis/index#webconfig-file)。

## <a name="access-configuration-during-startup"></a>在啟動期間存取組態

若要在啟動期間存取 `ConfigureServices` 或 `Configure` 內的組態，請參閱[應用程式啟動](xref:fundamentals/startup)主題中的範例。

## <a name="access-configuration-in-a-razor-page-or-mvc-view"></a>Razor 頁面或 MVC 檢視中的存取設定

若要在 [Razor 頁面] 頁面或 MVC 檢視中存取組態集，請針對 [Microsoft.Extensions.Configuration 命名空間新增[使用指示詞](xref:mvc/views/razor#using) ([C# 參考：使用指示詞](/dotnet/csharp/language-reference/keywords/using-directive)) ](/dotnet/api/microsoft.extensions.configuration)，並將 [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) 插入頁面或檢視中。

在 [Razor 頁面] 頁面中：

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

在 MVC 檢視中：

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

## <a name="additional-notes"></a>其他備註

* 相依性插入 (DI) 會在叫用 `ConfigureServices` 之後才設定。
* 組態系統無法感知 DI。
* `IConfiguration` 有兩個特製化：
  * `IConfigurationRoot` 用於根節點。 可觸發重新載入。
  * `IConfigurationSection` 代表組態值區段。 `GetSection` 和 `GetChildren` 方法會傳回 `IConfigurationSection`。
  * 在重新載入組態或存取各個提供者時使用 [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot)。 以上皆為罕見案例。

## <a name="additional-resources"></a>其他資源

* [選項](xref:fundamentals/configuration/options)
* [使用多個環境](xref:fundamentals/environments)
* [在開發期間安全儲存應用程式祕密](xref:security/app-secrets)
* [在 ASP.NET Core 中裝載](xref:fundamentals/hosting)
* [相依性插入](xref:fundamentals/dependency-injection)
* [Azure Key Vault 組態提供者](xref:security/key-vault-configuration)
