---
title: "ASP.NET Core 的設定"
author: rick-anderson
description: "透過多種方法來使用組態 API 設定 ASP.NET Core 應用程式。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 11/01/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration/index
ms.openlocfilehash: 6281d6ba254670b111964715410fc0694ae4d149
ms.sourcegitcommit: 216dfac27542f10a79274a9ce60dc449e888ed20
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/29/2017
---
# <a name="configure-an-aspnet-core-app"></a>設定 ASP.NET Core 應用程式

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)、[Mark Michaelis](http://intellitect.com/author/mark-michaelis/)、[Steve Smith](https://ardalis.com/)、[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)

組態 API 可讓您根據成對的名稱和數值清單來設定 ASP.NET Core Web 應用程式。 組態是在執行階段讀取自多個來源。 您可以將這些成對的名稱和數值分組為多層階層。 

下列項目有組態提供者：

* 檔案格式 (INI、JSON 和 XML)
* 命令列引數
* 環境變數
* 記憶體內部 .NET 物件
* 加密的使用者存放區
* [Azure Key Vault](xref:security/key-vault-configuration)
* 自訂提供者 (已安裝或已建立)

每個組態值會對應至一個字串索引鍵。 還有內建繫結支援可將設定還原序列化為自訂 [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) 物件 (具有屬性的簡單 .NET 類別)。

選項模式使用選項類別來代表一組相關的設定。 如需使用選項模式的詳細資訊，請參閱[選項](xref:fundamentals/configuration/options)主題。

[檢視或下載範例程式碼](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="json-configuration"></a>JSON 組態

下列主控台應用程式使用 JSON 組態提供者：

[!code-csharp[Main](index/sample/ConfigJson/Program.cs)]

此應用程式會讀取並顯示下列組態設定：

[!code-json[Main](index/sample/ConfigJson/appsettings.json)]

組態是由成對的名稱和數值階層式清單所組成，其中節點是以冒號分隔。 若要擷取值，請使用對應項目的索引鍵來存取 `Configuration` 索引子：

```csharp
Console.WriteLine($"option1 = {Configuration["subsection:suboption1"]}");
```

若要使用 JSON 格式組態來源中的陣列，請使用陣列索引作為冒號分隔字串的一部分。 下列範例會取得上述 `wizards` 陣列中第一個項目的名稱：

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}, ");
```

寫入至內建 `Configuration` 提供者之成對的名稱和數值**不會**保存。 不過，您可以建立自訂提供者來儲存值。 請參閱[自訂組態提供者](xref:fundamentals/configuration/index#custom-config-providers)。

上述範例使用 Configuration 索引子來讀取值。 若要存取 `Startup` 外部組態，請使用「選項模式」。 如需詳細資訊，請參閱[選項](xref:fundamentals/configuration/options)主題。

不同的環境 (例如開發、測試和生產) 通常會有不同的組態設定。 ASP.NET Core 2.x 應用程式中的 `CreateDefaultBuilder` 擴充方法 (或在 ASP.NET Core 1.x 應用程式中直接使用 `AddJsonFile` 和 `AddEnvironmentVariables`) 會新增組態提供者以讀取 JSON 檔案和系統組態來源：

* *appsettings.json*
* *appsettings.\<環境名稱>.json*
* 環境變數

如需參數的說明，請參閱 [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions)。 `reloadOnChange` 只有在 ASP.NET Core 1.1 和更新版本中才支援。 

組態來源是依其指定順序讀取。 在上述程式碼中，環境變數最後才會讀取。 在環境中設定的任何組態值會取代在上述兩個提供者中設定的值。

環境通常會設定為 `Development`、`Staging` 或 `Production`。 如需詳細資訊，請參閱[使用多個環境](xref:fundamentals/environments)。

組態考量：

* `IOptionsSnapshot` 可在組態資料變更時重新載入資料。 如需詳細資訊，請參閱 [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot)。
* 組態索引鍵不區分大小寫。
* 最後才指定環境變數，以便本機環境可覆寫已部署組態檔中的設定。
* **永遠不要**將密碼或其他敏感性資料儲存在組態提供者程式碼或純文字組態檔中。 不要在開發或測試環境中使用生產環境祕密。 相反地，請在專案外部指定祕密，以防止意外認可至您的存放庫。 深入了解如何[使用多個環境](xref:fundamentals/environments)及管理[在開發期間安全儲存應用程式祕密](xref:security/app-secrets)。
* 如果無法在您系統上的環境變數中使用冒號 (`:`)，請以雙底線 (`__`) 取代冒號 (`:`)。

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a>記憶體內部提供者和 POCO 類別的繫結

下列範例示範如何使用記憶體內部提供者，並繫結至一個類別：

[!code-csharp[Main](index/sample/InMemory/Program.cs)]

組態值是以字串傳回，但繫結會啟用物件的建構。 繫結可讓您擷取 POCO 物件，甚至是整個物件圖形。

### <a name="getvalue"></a>GetValue

下列範例示範 [GetValue&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) 擴充方法：

[!code-csharp[Main](index/sample/InMemoryGetValue/Program.cs?highlight=27-29)]

ConfigurationBinder 的 `GetValue<T>` 方法可讓您指定預設值 (在此範例中為 80)。 `GetValue<T>` 適用於簡單的案例，並不會繫結至整個區段。 `GetValue<T>` 會從轉換成特定類型的 `GetSection(key).Value` 取得純量值。

## <a name="bind-to-an-object-graph"></a>繫結至物件圖形

您可以遞迴繫結至類別中的每個物件。 請考慮下列 `AppSettings` 類別：

[!code-csharp[Main](index/sample/ObjectGraph/AppSettings.cs)]

下列範例會繫結至 `AppSettings` 類別：

[!code-csharp[Main](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

**ASP.NET Core 1.1** 和更新版本可以使用 `Get<T>`，這適用於整個區段。 `Get<T>` 可能比使用 `Bind` 更方便。 下列程式碼示範如何在上述範例中使用 `Get<T>`：

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

使用下列 *appsettings.json* 檔案：

[!code-json[Main](index/sample/ObjectGraph/appsettings.json)]

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

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

新增 `ConfigurationContext` 來儲存及存取所設定的值：

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

建立實作 [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource) 的類別：

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

透過繼承自 [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider) 來建立自訂組態提供者。  組態提供者會將空白資料庫初始化：

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

執行範例時，會顯示資料庫中醒目提示的值 ("value_from_ef_1" 和 "value_from_ef_2") 。

您可以新增 `EFConfigSource` 擴充方法來新增組態來源：

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

下列程式碼示範如何使用自訂 `EFConfigProvider`：

[!code-csharp[Main](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

請注意，此範例會在 JSON 提供者後面新增自訂 `EFConfigProvider`，如此一來資料庫中的任何設定就會覆寫 *appsettings.json* 檔案中的設定。

使用下列 *appsettings.json* 檔案：

[!code-json[Main](index/sample/CustomConfigurationProvider/appsettings.json)]

此時會顯示下列對話方塊：

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a>命令列組態提供者

[命令列組態提供者](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider)會在執行階段接收組態的命令列引數索引鍵/值組。

[檢視或下載命令列組態範例](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/index/sample/CommandLine)

### <a name="setup-and-use-the-commandline-configuration-provider"></a>設定及使用命令列組態提供者

# <a name="basic-configurationtabbasicconfiguration"></a>[基本組態](#tab/basicconfiguration)

若要啟用命令列組態，請在 [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) 的執行個體上呼叫 `AddCommandLine` 擴充方法：

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

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

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

一般 ASP.NET Core 2.x 應用程式會使用靜態簡便方法 `CreateDefaultBuilder` 來建置主應用程式：

[!code-csharp[Main](index/sample_snapshot//Program.cs?highlight=12)]

`CreateDefaultBuilder` 會從 *appsettings.json*、*appsettings.{Environment}.json*、[使用者祕密](xref:security/app-secrets) (在 `Development` 環境中)、環境變數和命令列引數載入選擇性組態。 最後才呼叫命令列組態提供者。 最後呼叫提供者可讓在執行階段傳遞的命令列引數，覆寫之前呼叫之其他組態提供者所設定的組態。

請注意，已啟用 *appsettings* 檔案的 `reloadOnChange`。 如果 *appsettings* 檔案中的相符組態值在應用程式啟動後已變更，則會覆寫命令列引數。

> [!NOTE]
> ASP.NET Core 2.x 支援使用 [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) 建立主應用程式並使用 [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) 手動建置組態，來替代使用 `CreateDefaultBuilder` 方法。 如需詳細資訊，請參閱 ASP.NET Core 1.x 索引標籤。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

建立 [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) 並呼叫 `AddCommandLine` 方法來使用命令列組態提供者。 最後呼叫提供者可讓在執行階段傳遞的命令列引數，覆寫之前呼叫之其他組態提供者所設定的組態。 使用 `UseConfiguration` 方法將組態套用至 [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)：

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a>引數

在命令列上傳遞的引數必須符合下表中所示的兩種格式之一。

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

注意：如果提供給組態提供者的[切換對應](#switch-mappings)中沒有 `-key1`，則會擲回 `FormatException`。

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

使用 `ConfigurationBuilder` 手動建置組態時，您可以選擇性地提供切換對應字典給 `AddCommandLine` 方法。 切換對應可讓您提供索引鍵名稱取代邏輯。

使用切換對應字典時，會檢查字典中是否有任何索引鍵符合命令列引數所提供的索引鍵。 如果在字典中找到命令列索引鍵，則會傳回字典值 (索引鍵取代) 以設定組態。 所有前面加上單虛線 (`-`) 的命令列索引鍵都需要切換對應。

切換對應字典索引鍵規則：

* 切換必須以單虛線 (`-`) 或雙虛線 (`--`) 開頭。
* 切換對應字典不能包含重複索引鍵。

在下列範例中，`GetSwitchMappings` 方法可讓您的命令列引數使用單虛線 (`-`) 索引鍵前置字元，並避免以子索引鍵前置字元開頭。

[!code-csharp[Main](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

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

建立切換對應字典之後，它會包含下表中所示的資料。

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

## <a name="the-webconfig-file"></a>web.config 檔案

當您在 IIS 或 IIS Express 中裝載應用程式時，需要 *web.config* 檔案。 *web.config* 會開啟 IIS 中的 AspNetCoreModule 以啟動您的應用程式。 *web.config* 中的設定可讓 IIS 中的 AspNetCoreModule 啟動您的應用程式，並設定其他 IIS 設定和模組。 如果您使用 Visual Studio 並刪除 *web.config*，Visual Studio 會建立新的 web.config 檔案。

## <a name="additional-notes"></a>其他備註

* 相依性插入 (DI) 會在叫用 `ConfigureServices` 之後才設定。
* 組態系統非 DI 感知。
* `IConfiguration` 有兩個特製化：
  * `IConfigurationRoot`：用於根節點。 可觸發重新載入。
  * `IConfigurationSection`：代表組態值區段。 `GetSection` 和 `GetChildren` 方法會傳回 `IConfigurationSection`。

## <a name="additional-resources"></a>其他資源

* [選項](xref:fundamentals/configuration/options)
* [使用多個環境](xref:fundamentals/environments)
* [在開發期間安全儲存應用程式密碼](xref:security/app-secrets)
* [在 ASP.NET Core 中裝載](xref:fundamentals/hosting)
* [相依性插入](xref:fundamentals/dependency-injection)
* [Azure Key Vault 組態提供者](xref:security/key-vault-configuration)
