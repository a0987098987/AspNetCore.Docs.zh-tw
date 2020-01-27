---
title: ASP.NET Core 的設定
author: guardrex
description: 了解如何使用組態 API 設定 ASP.NET Core 應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/23/2020
uid: fundamentals/configuration/index
ms.openlocfilehash: 141ae5cda7672159032013cbda1ef4bfa7c142dd
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2020
ms.locfileid: "76726977"
---
# <a name="configuration-in-aspnet-core"></a>ASP.NET Core 的設定

作者：[Luke Latham](https://github.com/guardrex)

ASP.NET Core 中的應用程式設定是以由*設定提供者*所建立的機碼值組為基礎。 設定提供者會從各種設定來源將設定資料讀取到機碼值組中：

* Azure Key Vault
* Azure 應用程式組態
* 命令列引數
* 自訂提供者 (已安裝或已建立)
* 目錄檔案
* 環境變數
* 記憶體內部 .NET 物件
* 設定檔

::: moniker range=">= aspnetcore-3.0"

一般組態提供者案例 ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) 的組態套件會以隱含方式包含在架構中。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

一般組態提供者案例 ([microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) 的組態套件包含在 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)中。

::: moniker-end

下列程式碼範例與範例應用程式中的程式碼範例使用 <xref:Microsoft.Extensions.Configuration> 命名空間：

```csharp
using Microsoft.Extensions.Configuration;
```

*選項模式*是此主題中所述之設定概念的延伸。 選項使用類別來代表一組相關的設定。 如需詳細資訊，請參閱<xref:fundamentals/configuration/options>。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="host-versus-app-configuration"></a>主機與應用程式組態的比較

設定及啟動應用程式之前，會先設定及啟動「主機」。 主機負責應用程式啟動和存留期管理。 應用程式與主機都是使用此主題中所述的設定提供者來設定的。 主機組態機碼/值組也會包含在應用程式的組態中。 如需有關當建置主機時如何使用設定提供者的詳細資訊，以及設定來源如何影響主機設定的詳細資訊，請參閱 <xref:fundamentals/index#host>。

## <a name="default-configuration"></a>預設的組態

::: moniker range=">= aspnetcore-3.0"

以 ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) 範本為基礎的 Web 應用程式，會在建置主機時呼叫 <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>。 `CreateDefaultBuilder` 會以下列順序提供應用程式的預設組態：

下列項目適用於使用[一般主機](xref:fundamentals/host/generic-host)的應用程式。 如需使用 [Web 主機](xref:fundamentals/host/web-host)時預設組態的詳細資料，請參閱[本主題的 ASP.NET Core 2.2 版本](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2)。

* 主機組態的提供來源：
  * 使用[環境變數組態提供者](#environment-variables-configuration-provider)且以 `DOTNET_` 為前置詞 (例如 `DOTNET_ENVIRONMENT`) 的環境變數。 載入設定機碼值組時，會移除前置詞 (`DOTNET_`)。
  * 使用[命令列組態提供者](#command-line-configuration-provider)的命令列引數。
* 已建立 Web 主機預設組態 (`ConfigureWebHostDefaults`)：
  * Kestrel 會用作為網頁伺服器，並使用應用程式的組態提供者來設定。
  * 新增主機篩選中介軟體。
  * 如果 `ASPNETCORE_FORWARDEDHEADERS_ENABLED` 環境變數設定為 `true`，則會新增轉接的標頭中介軟體。
  * 啟用 IIS 整合。
* 應用程式設定的提供來源：
  * 使用[檔案組態提供者](#file-configuration-provider)的 *appsettings.json*。
  * 使用[檔案組態提供者](#file-configuration-provider)的 *appsettings.{Environment}.json*。
  * 應用程式在使用項目組件之 `Development` 環境中執行時的[秘密管理員](xref:security/app-secrets)。
  * 使用[環境變數組態提供者](#environment-variables-configuration-provider)的環境變數。
  * 使用[命令列組態提供者](#command-line-configuration-provider)的命令列引數。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

以 ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) 範本為基礎的 Web 應用程式，會在建置主機時呼叫 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>。 `CreateDefaultBuilder` 會以下列順序提供應用程式的預設組態：

下列項目適用於使用 [Web 主機](xref:fundamentals/host/web-host)的應用程式。 如需使用[一般主機](xref:fundamentals/host/generic-host)時預設組態的詳細資料，請參閱[本主題的最新版本](xref:fundamentals/configuration/index)。

* 主機組態的提供來源：
  * 使用[環境變數組態提供者](#environment-variables-configuration-provider)且以 `ASPNETCORE_` 為前置詞 (例如 `ASPNETCORE_ENVIRONMENT`) 的環境變數。 載入設定機碼值組時，會移除前置詞 (`ASPNETCORE_`)。
  * 使用[命令列組態提供者](#command-line-configuration-provider)的命令列引數。
* 應用程式設定的提供來源：
  * 使用[檔案組態提供者](#file-configuration-provider)的 *appsettings.json*。
  * 使用[檔案組態提供者](#file-configuration-provider)的 *appsettings.{Environment}.json*。
  * 應用程式在使用項目組件之 `Development` 環境中執行時的[秘密管理員](xref:security/app-secrets)。
  * 使用[環境變數組態提供者](#environment-variables-configuration-provider)的環境變數。
  * 使用[命令列組態提供者](#command-line-configuration-provider)的命令列引數。

::: moniker-end

## <a name="security"></a>安全性

採用下列做法來保護敏感性組態資料：

* 永遠不要將密碼或其他敏感性資料儲存在設定提供者程式碼或純文字設定檔中。
* 不要在開發或測試環境中使用生產環境祕密。
* 請在專案外部指定祕密，以防止其意外認可至開放原始碼存放庫。

如需詳細資訊，請參閱下列主題：

* <xref:fundamentals/environments>
* <xref:security/app-secrets> &ndash; 包含有關使用環境變數來儲存敏感性資料的建議。 「祕密管理員」使用「檔案設定提供者」以 JSON 檔案在本機系統上存放使用者祕密。 此主題稍後將說明「檔案設定提供者」。

[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) 可安全地儲存 ASP.NET Core 應用程式的應用程式祕密。 如需詳細資訊，請參閱<xref:security/key-vault-configuration>。

## <a name="hierarchical-configuration-data"></a>階層式設定資料

設定 API 可在設定機碼中使用分隔符號來壓平合併階層式資料，以管理階層式設定資料。

在下列 JSON 檔案中，兩個區段的結構式階層中有四個機碼存在：

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  }
}
```

當該檔案被讀入設定之後，會建立唯一機碼來維護設定來源的原始階層式資料結構。 系統會使用冒號 (`:`) 將區段與機碼壓平合併以維護原始結構：

* section0:key0
* section0:key1
* section1:key0
* section1:key1

<xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> 與 <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> 方法可用來在設定資料中隔離區段與區段的子系。 [GetSection,、 GetChildren 與 Exists](#getsection-getchildren-and-exists) 中說明這些方法。

## <a name="conventions"></a>慣例

### <a name="sources-and-providers"></a>來源和提供者

在應用程式啟動時，會依照設定來源的設定提供者的指定順序讀入設定來源。

實作變更偵測的組態提供者能夠在基礎設定變更時重新載入組態。 例如，檔案組態提供者 (將於本主題稍後討論) 和 [Azure Key Vault 組態提供者](xref:security/key-vault-configuration)均會實作變更偵測。

您可以在應用程式的[相依性插入 (DI)](xref:fundamentals/dependency-injection) 容器中找到 <xref:Microsoft.Extensions.Configuration.IConfiguration>。 <xref:Microsoft.Extensions.Configuration.IConfiguration> 可以插入至 Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> 或 MVC <xref:Microsoft.AspNetCore.Mvc.Controller>，以取得類別的設定。

在下列範例中，`_config` 欄位是用來存取設定值：

```csharp
public class IndexModel : PageModel
{
    private readonly IConfiguration _config;

    public IndexModel(IConfiguration config)
    {
        _config = config;
    }
}
```

```csharp
public class HomeController : Controller
{
    private readonly IConfiguration _config;

    public HomeController(IConfiguration config)
    {
        _config = config;
    }
}
```

設定提供者無法使用 DI，因為當它們由主機設定時，它無法使用。

### <a name="keys"></a>按鍵

設定機碼會採用下列慣例：

* 機碼區分大小寫。 例如，`ConnectionString` 與 `connectionstring` 會被視為相等的機碼。
* 若相同機碼的值是由相同或不同設定提供者設定，在機碼上設定的最後一個值是所使用的值。
* 階層式機碼
  * 在設定 API 內，冒號分隔字元 (`:`) 可在所有平台上運作。
  * 在環境變數中，冒號分隔字元可能無法在所有平台上運作。 所有平台都支援雙底線 (`__`)，且會自動轉換為冒號。
  * 在 Azure Key Vault 中，階層式機碼使用 `--` (兩個破折號) 來做為分隔符號。 撰寫程式碼，以在將密碼載入應用程式的設定時，以冒號取代破折號。
* <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> 支援在設定機碼中使用陣列索引將陣列繫結到物件。 [將陣列繫結到類別](#bind-an-array-to-a-class)一節說明陣列繫結。

### <a name="values"></a>值

設定值會採用下列慣例：

* 值是字串。
* Null 值無法存放在設定中或繫結到物件。

## <a name="providers"></a>提供者

下表顯示可供 ASP.NET Core 應用程式使用的設定提供者。

| Provider | 從&hellip;提供設定 |
| -------- | ----------------------------------- |
| [Azure Key Vault 設定提供者](xref:security/key-vault-configuration) (*安全性*主題) | Azure Key Vault |
| [Azure 應用程式組態提供者](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure 文件) | Azure 應用程式組態 |
| [命令列設定提供者](#command-line-configuration-provider) | 命令列參數 |
| [自訂設定提供者](#custom-configuration-provider) | 自訂來源 |
| [環境變數設定提供者](#environment-variables-configuration-provider) | 環境變數 |
| [檔案設定提供者](#file-configuration-provider) | 檔案 (INI、JSON、XML) |
| [每個檔案機碼的設定提供者](#key-per-file-configuration-provider) | 目錄檔案 |
| [記憶體設定提供者](#memory-configuration-provider) | 記憶體內集合 |
| [使用者祕密 (祕密管理員)](xref:security/app-secrets) (*安全性*主題) | 使用者設定檔目錄中的檔案 |

在啟動時，會依照設定來源的設定提供者的指定順序讀入設定來源。 本主題所描述的設定提供者會依字母順序描述，而不是程式碼排列它們的順序。 請在程式碼中訂購設定提供者，以符合應用程式所需之基礎設定來源的優先順序。

典型的設定提供者順序是：

1. 檔案 (*appsettings.json*、*appsettings.{Environment}.json*，其中 `{Environment}` 是應用程式的目前裝載環境)
1. [Azure Key Vault](xref:security/key-vault-configuration)
1. [使用者祕密 (祕密管理員)](xref:security/app-secrets) (僅限開發環境)
1. 環境變數
1. 命令列引數

將命令列組態提供者放在提供者序列結尾是常見做法，因為這樣可以讓命令列引數覆寫由其他提供者所設定的組態。

當使用 `CreateDefaultBuilder`初始化新的主機產生器時，會使用上述的提供者序列。 如需詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。

::: moniker range=">= aspnetcore-3.0"

## <a name="configure-the-host-builder-with-configurehostconfiguration"></a>使用 ConfigureHostConfiguration 設定主機建立器

若要設定主機建立器，請呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> 並提供組態。 `ConfigureHostConfiguration` 用於初始化 <xref:Microsoft.Extensions.Hosting.IHostEnvironment>，以便稍後在建置程序中使用。 `ConfigureHostConfiguration` 可以多次呼叫，其結果是累加的。

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureHostConfiguration(config =>
        {
            var dict = new Dictionary<string, string>
            {
                {"MemoryCollectionKey1", "value1"},
                {"MemoryCollectionKey2", "value2"}
            };

            config.AddInMemoryCollection(dict);
        })
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

## <a name="configure-the-host-builder-with-useconfiguration"></a>使用 UseConfiguration 設定主機建立器

若要設定主機建立器，請使用該組態在主機建立器上呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>。

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args)
{
    var dict = new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };

    var config = new ConfigurationBuilder()
        .AddInMemoryCollection(dict)
        .Build();

    return WebHost.CreateDefaultBuilder(args)
        .UseConfiguration(config)
        .UseStartup<Startup>();
}
```

::: moniker-end

## <a name="configureappconfiguration"></a>ConfigureAppConfiguration

建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定提供者，以及由 `CreateDefaultBuilder` 自動新增的設定提供者：

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

::: moniker-end

### <a name="override-previous-configuration-with-command-line-arguments"></a>使用命令列引數覆寫先前的組態

若要提供可使用命令列引數覆寫的應用程式組態，請最後呼叫 `AddCommandLine`：

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="remove-providers-added-by-createdefaultbuilder"></a>移除 CreateDefaultBuilder 新增的提供者

若要移除 `CreateDefaultBuilder`新增的提供者，請先在[IConfigurationBuilder](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources)上呼叫[Clear](/dotnet/api/system.collections.generic.icollection-1.clear) ：

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.Sources.Clear();
    // Add providers here
})
```

### <a name="consume-configuration-during-app-startup"></a>在應用程式啟動期間使用組態

應用程式啟動期間，可以使用 `ConfigureAppConfiguration` 中為應用程式提供的組態，包括 `Startup.ConfigureServices`。 如需詳細資訊，請參閱[在啟動期間存取組態](#access-configuration-during-startup)一節。

## <a name="command-line-configuration-provider"></a>命令列設定提供者

<xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> 會在執行階段從命令列引數機碼值組載入設定。

為了啟用命令列設定，<xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 延伸模組方法會在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫。

呼叫 `CreateDefaultBuilder(string [])` 時，會自動呼叫 `AddCommandLine`。 如需詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。

`CreateDefaultBuilder` 也會載入：

* 從 *appsettings.json* 與 *appsettings.{Environment}.json* 檔案載入其他選擇性組態。
* 開發環境中的[使用者祕密 (祕密管理員)](xref:security/app-secrets)。
* 環境變數。

`CreateDefaultBuilder` 會最後才新增命令列設定提供者。 在執行階段傳遞的命令列引數會覆寫由其它提供者所設定的設定。

`CreateDefaultBuilder` 會在建構主機時執行作業。 因此，由`CreateDefaultBuilder` 啟用的命令列設定可以影響主機的設定方式。

針對以 ASP.NET Core 範本為基礎的應用程式，`AddCommandLine` 已由 `CreateDefaultBuilder` 呼叫。 若要新增其他組態提供者並維持能夠使用命令列引數覆寫這些提供者組態的能力，請在 `ConfigureAppConfiguration` 中呼叫應用程式的其他提供者，且最後呼叫 `AddCommandLine`。

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

**範例**

範例應用程式利用靜態方便方法 `CreateDefaultBuilder` 的優勢來建置主機，這包括對 <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 的呼叫。

1. 從專案目錄中開啟命令提示字元。
1. 提供命令列引數給 `dotnet run` 命令 `dotnet run CommandLineKey=CommandLineValue`。
1. 在應用程式執行之後，開啟瀏覽器以瀏覽位於 `http://localhost:5000` 的應用程式。
1. 觀察輸出是否包含提供給 `dotnet run` 之設定命令列引數的機碼值組。

### <a name="arguments"></a>Arguments

當值後面接著空格時，值必須接著等號 (`=`)，或機碼必須有前置詞 (`--` 或 `/`)。 如果使用等號 (例如 `CommandLineKey=`)，則不需要此值。

| 索引鍵前置字元               | 範例                                                |
| ------------------------ | ------------------------------------------------------ |
| 沒有前置字元                | `CommandLineKey1=value1`                               |
| 雙虛線 (`--`)        | `--CommandLineKey2=value2`、 `--CommandLineKey2 value2` |
| 正斜線 (`/`)      | `/CommandLineKey3=value3`、 `/CommandLineKey3 value3`   |

在相同的命令中，請不要混合使用等號搭配使用空格之機碼值組的命令列引數。

範例命令：

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a>切換對應

參數對應允許索引鍵名稱取代邏輯。 以 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>手動建立設定時，請將參數取代的字典提供給 <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 方法。

使用切換對應字典時，會檢查字典中是否有任何索引鍵符合命令列引數所提供的索引鍵。 如果在字典中找到命令列索引鍵，則會傳回字典值 (索引鍵取代) 以在應用程式的設定中設定機碼值組。 所有前面加上單虛線 (`-`) 的命令列索引鍵都需要切換對應。

切換對應字典索引鍵規則：

* 切換必須以單虛線 (`-`) 或雙虛線 (`--`) 開頭。
* 切換對應字典不能包含重複索引鍵。

建立切換對應字典。 下列範例會建立兩個切換對應：

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

建立主機時，請使用切換對應字典呼叫 `AddCommandLine`：

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

針對使用切換對應的應用程式，呼叫 `CreateDefaultBuilder` 不應傳遞引數。 `CreateDefaultBuilder` 方法的 `AddCommandLine` 呼叫不包括對應的切換，且沒有任何方式可以將切換對應字典傳遞給 `CreateDefaultBuilder`。 解決方案並非將引數傳遞給 `CreateDefaultBuilder`，而是允許 `ConfigurationBuilder` 方法的 `AddCommandLine` 方法同時處理引數與切換對應字典。

建立切換對應字典之後，它會包含下表中所示的資料。

| 索引鍵       | {2&gt;值&lt;2}             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

若啟動應用程式時使用切換對應機碼，設定會接收由字典提供之機碼上的設定值：

```dotnetcli
dotnet run -CLKey1=value1 -CLKey2=value2
```

執行上述命令之後，設定包含下表中顯示的值。

| 索引鍵               | {2&gt;值&lt;2}    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a>環境變數設定提供者

<xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> 會在執行階段從環境變數機碼值組載入設定。

若要啟用環境變數設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> 延伸模組方法。

[!INCLUDE[](~/includes/environmentVarableColon.md)]

[Azure App Service](https://azure.microsoft.com/services/app-service/)允許在 Azure 入口網站中設定環境變數，以使用環境變數設定提供者來覆寫應用程式設定。 如需詳細資訊，請參閱 [Azure App：使用 Azure 入口網站覆寫應用程式設定](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal)。

::: moniker range=">= aspnetcore-3.0"

使用[一般主機](xref:fundamentals/host/generic-host)初始化新的主機建立器並呼叫 `CreateDefaultBuilder` 時，可使用 `AddEnvironmentVariables` 為[主機組態](#host-versus-app-configuration)載入字首為 `DOTNET_` 的環境變數。 如需詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

使用 [Web 主機](xref:fundamentals/host/web-host)初始化新的主機建立器並呼叫 `CreateDefaultBuilder` 時，可使用 `AddEnvironmentVariables` 為[主機組態](#host-versus-app-configuration)載入字首為 `ASPNETCORE_` 的環境變數。 如需詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。

::: moniker-end

`CreateDefaultBuilder` 也會載入：

* 來自無首碼之環境變數的應用程式組態 (在未提供首碼的情況下呼叫 `AddEnvironmentVariables`)。
* 從 *appsettings.json* 與 *appsettings.{Environment}.json* 檔案載入其他選擇性組態。
* 開發環境中的[使用者祕密 (祕密管理員)](xref:security/app-secrets)。
* 命令列引數。

從使用者祕密與 *appsettings* 檔案建立設定之後，會呼叫「環境變數設定提供者」。 在此位置呼叫提供者可讓系統在執行階段讀取環境變數，以覆寫由使用者祕密與 *appsettings* 檔案所設定的設定。

若要從其他環境變數提供應用程式設定，請在 `ConfigureAppConfiguration` 中呼叫應用程式的其他提供者，並使用前置詞呼叫 `AddEnvironmentVariables`：

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddEnvironmentVariables(prefix: "PREFIX_");
})
```

呼叫 last `AddEnvironmentVariables`，以允許具有指定前置詞的環境變數覆寫其他提供者的值。

**範例**

範例應用程式利用靜態方便方法 `CreateDefaultBuilder` 的優勢來建置主機，這包括對 `AddEnvironmentVariables` 的呼叫。

1. 執行範例應用程式。 開啟瀏覽器以瀏覽位於 `http://localhost:5000` 的應用程式。
1. 觀察輸出是否包含環境變數 `ENVIRONMENT` 的機碼值組。 值反映應用程式執行所在環境，在本機執行時，通常是 `Development`。

為縮短由應用程式轉譯的環境變數清單，應用程式會篩選環境變數。 請參閱範例應用程式的 *Pages/Index.cshtml.cs* 檔案。

若要公開應用程式可用的所有環境變數，請將*Pages/Index. cshtml*中的 `FilteredConfiguration` 變更為下列內容：

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a>首碼

在 `AddEnvironmentVariables` 方法中提供前置詞時，會篩選載入應用程式設定中的環境變數。 例如，若要篩選前置詞為 `CUSTOM_` 的環境變數，請提供前置詞給設定提供者：

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

建立設定機碼值組時，會移除前置詞。

建立主機建立器時，主機組態由環境變數提供。 如需這些環境變數所用前置詞的詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。

**連接字串前置詞**

設定 API 四個連接字串環境變數的特殊處理規則，這些這些環境變數牽涉到針對應用程式環境設定 Azure 連接字串。 若將前置詞提供給 `AddEnvironmentVariables`具有下表顯示之前置詞的環境變數。

| 連接字串前置詞 | Provider |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | 自訂提供者 |
| `MYSQLCONNSTR_` | [MySQL](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [SQL Server](https://www.microsoft.com/sql-server/) |

當探索到具有下表所顯示之任何四個前置詞的環境變數並將其載入到設定中時：

* 會透過移除環境變數前置詞並新增設定機碼區段 (`ConnectionStrings`) 來移除具有下表顯示之前置詞的環境變數。
* 會建立新的設定機碼值組以代表資料庫連線提供者 (`CUSTOMCONNSTR_` 除外，這沒有所述提供者)。

| 環境變數機碼 | 已轉換的設定機碼 | 提供者設定項目                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | 設定項目未建立。                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | 機碼：`ConnectionStrings:<KEY>_ProviderName`：<br>值：`MySql.Data.MySqlClient` |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | 機碼：`ConnectionStrings:<KEY>_ProviderName`：<br>值：`System.Data.SqlClient`  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | 機碼：`ConnectionStrings:<KEY>_ProviderName`：<br>值：`System.Data.SqlClient`  |

## <a name="file-configuration-provider"></a>檔案設定提供者

<xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> 是用於從檔案系統載入設定的基底類別。 下列設定提供者專用於特定檔案類型：

* [INI 設定提供者](#ini-configuration-provider)
* [JSON 設定提供者](#json-configuration-provider)
* [XML 設定提供者](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a>INI 設定提供者

<xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> 會在執行階段從 INI 檔案機碼值組載入設定。

若要啟用 INI 檔案設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> 延伸模組方法。

冒號可用來做為 INI 檔案設定中的區段分隔符號。

多載允許指定：

* 檔案是否為選擇性。
* 檔案變更時是否要重新載入設定。
* <xref:Microsoft.Extensions.FileProviders.IFileProvider> 是用於存取該檔案。

建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定：

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

INI 設定檔的一般範例：

```ini
[section0]
key0=value
key1=value

[section1]
subsection:key=value

[section2:subsection0]
key=value

[section2:subsection1]
key=value
```

先前的設定檔會使用 `value` 載入下列機碼：

* section0:key0
* section0:key1
* section1:subsection:key
* section2:subsection0:key
* section2:subsection1:key

### <a name="json-configuration-provider"></a>JSON 設定提供者

<xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> 會在執行階段從 JSON 檔案機碼值組載入設定。

若要啟用 JSON 檔案設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> 延伸模組方法。

多載允許指定：

* 檔案是否為選擇性。
* 檔案變更時是否要重新載入設定。
* <xref:Microsoft.Extensions.FileProviders.IFileProvider> 是用於存取該檔案。

當使用 `CreateDefaultBuilder`初始化新的主機產生器時，會自動呼叫 `AddJsonFile` 兩次。 會呼叫此方法以從下列位置載入設定：

* *appsettings*會先讀取此檔案 &ndash;。 檔案的環境版本可以覆寫由 *appsettings.json* 檔案提供的值。
* *appsettings。{環境}. json* &ndash; 檔案的環境版本是根據[IHostingEnvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*)載入。

如需詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。

`CreateDefaultBuilder` 也會載入：

* 環境變數。
* 開發環境中的[使用者祕密 (祕密管理員)](xref:security/app-secrets)。
* 命令列引數。

會先建立 JSON 設定提供者。 因此，使用者祕密、環境變數與命令列引數會覆寫由 *appsettings* 檔案設定的設定。

在建置主機時，呼叫 `ConfigureAppConfiguration` 以指定檔案 (除了 *appsettings.json* 和  *appsettings.{Environment}.json* 以外) 的應用程式設定：

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

**範例**

範例應用程式會利用靜態便利方法 `CreateDefaultBuilder` 來建立主機，其中包括兩個 `AddJsonFile`呼叫：

::: moniker range=">= aspnetcore-3.0"

* 第一次呼叫 `AddJsonFile` 會從*appsettings*載入設定：

  [!code-json[](index/samples/3.x/ConfigurationSample/appsettings.json)]

* 第二次呼叫 `AddJsonFile` 會從 appsettings 載入設定 *。 {環境}. json*。 適用于*appsettings。* 範例應用程式中的開發 json 會載入下列檔案：

  [!code-json[](index/samples/3.x/ConfigurationSample/appsettings.Development.json)]

1. 執行範例應用程式。 開啟瀏覽器以瀏覽位於 `http://localhost:5000` 的應用程式。
1. 輸出包含以應用程式環境為基礎之設定的索引鍵/值組。 在開發環境中執行應用程式時，會 `Debug` 金鑰 `Logging:LogLevel:Default` 的記錄層級。
1. 在生產環境中再次執行範例應用程式：
   1. 開啟*Properties/launchsettings.json*檔案。
   1. 在 `ConfigurationSample` 設定檔中，將 `ASPNETCORE_ENVIRONMENT` 環境變數的值變更為 [`Production`]。
   1. 儲存檔案，並使用命令 shell 中的 `dotnet run` 來執行應用程式。
1. Appsettings 中的設定 *。* 在*appsettings*中，不會再覆寫 json 中的設定。 `Information`金鑰 `Logging:LogLevel:Default` 的記錄層級。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* 第一次呼叫 `AddJsonFile` 會從*appsettings*載入設定：

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.json)]

* 第二次呼叫 `AddJsonFile` 會從 appsettings 載入設定 *。 {環境}. json*。 適用于*appsettings。* 範例應用程式中的開發 json 會載入下列檔案：

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.Development.json)]

1. 執行範例應用程式。 開啟瀏覽器以瀏覽位於 `http://localhost:5000` 的應用程式。
1. 輸出包含以應用程式環境為基礎之設定的索引鍵/值組。 在開發環境中執行應用程式時，會 `Debug` 金鑰 `Logging:LogLevel:Default` 的記錄層級。
1. 在生產環境中再次執行範例應用程式：
   1. 開啟*Properties/launchsettings.json*檔案。
   1. 在 `ConfigurationSample` 設定檔中，將 `ASPNETCORE_ENVIRONMENT` 環境變數的值變更為 [`Production`]。
   1. 儲存檔案，並使用命令 shell 中的 `dotnet run` 來執行應用程式。
1. Appsettings 中的設定 *。* 在*appsettings*中，不會再覆寫 json 中的設定。 `Warning`金鑰 `Logging:LogLevel:Default` 的記錄層級。

::: moniker-end

### <a name="xml-configuration-provider"></a>XML 設定提供者

<xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> 會在執行階段從 XML 檔案機碼值組載入設定。

若要啟用 XML 檔案設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> 延伸模組方法。

多載允許指定：

* 檔案是否為選擇性。
* 檔案變更時是否要重新載入設定。
* <xref:Microsoft.Extensions.FileProviders.IFileProvider> 是用於存取該檔案。

建立設定機碼值組時，會忽略設定檔案的根節點。 請勿在檔案中指定文件類型定義 (DTD) 或命名空間。

建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定：

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

XML 設定檔可以針對重複的區段使用相異元素名稱：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <section0>
    <key0>value</key0>
    <key1>value</key1>
  </section0>
  <section1>
    <key0>value</key0>
    <key1>value</key1>
  </section1>
</configuration>
```

先前的設定檔會使用 `value` 載入下列機碼：

* section0:key0
* section0:key1
* section1:key0
* section1:key1

若 `name` 屬性是用來區別元素，則可以重複那些使用相同元素名稱的元素：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <section name="section0">
    <key name="key0">value</key>
    <key name="key1">value</key>
  </section>
  <section name="section1">
    <key name="key0">value</key>
    <key name="key1">value</key>
  </section>
</configuration>
```

先前的設定檔會使用 `value` 載入下列機碼：

* section:section0:key:key0
* section:section0:key:key1
* section:section1:key:key0
* section:section1:key:key1

屬性可用來提供值：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

先前的設定檔會使用 `value` 載入下列機碼：

* key:attribute
* section:key:attribute

## <a name="key-per-file-configuration-provider"></a>每個檔案機碼設定提供者

<xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> 使用目錄的檔案做為設定機碼值組。 機碼是檔案名稱。 值包含檔案的內容。 每個檔案機碼設定提供者是在 Docker 主機案例中使用。

若要啟用每個檔案機碼設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> 延伸模組方法。 檔案的 `directoryPath` 必須是絕對路徑。

多載允許指定：

* 設定來源的 `Action<KeyPerFileConfigurationSource>` 委派。
* 目錄是否為選擇性與目錄的路徑。

雙底線 (`__`) 是做為檔案名稱中的設定金鑰分隔符號使用。 例如，檔案名稱 `Logging__LogLevel__System` 會產生設定金鑰 `Logging:LogLevel:System`。

建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定：

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

## <a name="memory-configuration-provider"></a>記憶體設定提供者

<xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> 使用記憶體內集合做為設定機碼值組。

若要啟用記憶體內集合設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> 延伸模組方法。

設定提供者可以使用 `IEnumerable<KeyValuePair<String,String>>` 來初始化。

建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定。

下列範例會建立組態字典：

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

字典會與 `AddInMemoryCollection` 的呼叫搭配使用以提供組態：

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a>GetValue

[ConfigurationBinder\<t >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*)會使用指定的索引鍵從設定中解壓縮單一值，並將它轉換成指定的 noncollection 類型。 多載會接受預設值。

下列範例︰

* 從具有機碼 `NumberKey` 的組態擷取字串值。 若在組態機碼中找不到 `NumberKey`，則會使用預設值 `99`。
* 鍵入值為 `int`。
* 在 `NumberConfig` 屬性中儲存值供頁面使用。

```csharp
public class IndexModel : PageModel
{
    public IndexModel(IConfiguration config)
    {
        _config = config;
    }

    public int NumberConfig { get; private set; }

    public void OnGet()
    {
        NumberConfig = _config.GetValue<int>("NumberKey", 99);
    }
}
```

## <a name="getsection-getchildren-and-exists"></a>GetSection、GetChildren 與 Exists

針對下面的範例，請考慮下列 JSON 檔案。 在兩個區段中找到四個機碼，其中一個包括子區段組：

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  },
  "section2": {
    "subsection0" : {
      "key0": "value",
      "key1": "value"
    },
    "subsection1" : {
      "key0": "value",
      "key1": "value"
    }
  }
}
```

當檔案被讀入到設定之後，會建立下列唯一階層式機碼以存放設定值：

* section0:key0
* section0:key1
* section1:key0
* section1:key1
* section2:subsection0:key0
* section2:subsection0:key1
* section2:subsection1:key0
* section2:subsection1:key1

### <a name="getsection"></a>GetSection

[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) 會擷取具有所指定子區段機碼的設定子區段。

若要傳回 `section1` 中只包含一個機碼值組的 <xref:Microsoft.Extensions.Configuration.IConfigurationSection>，請呼叫 `GetSection` 並提供區段名稱：

```csharp
var configSection = _config.GetSection("section1");
```

`configSection` 沒有值，只有索引鍵和路徑。

同樣地，若要取得 `section2:subsection0` 中之機碼的值，請呼叫 `GetSection` 並提供區段路徑：

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

`GetSection` 絕不會傳回 `null`。 若找不到相符的區段，會傳回空的 `IConfigurationSection`。

當 `GetSection` 傳回相符區段時，未填入 <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value>。 當區段存在時，會傳回 <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> 與 <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path>。

### <a name="getchildren"></a>GetChildren

對 `section2` 上之 [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) 的呼叫會取得包括下列項目的 `IEnumerable<IConfigurationSection>`：

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a>存在

使用 [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) 來判斷設定區段是否存在：

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

以範例資料為例， `sectionExists` 是 `false`，因未設定資料中沒有 `section2:subsection2` 區段。

## <a name="bind-to-a-class"></a>繫結到類別

設定可以繫結到類別，以使用*選項模式*代表相關設定的群組。 如需詳細資訊，請參閱<xref:fundamentals/configuration/options>。

設定值是以字串傳回，但是呼叫 <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> 會啟用 [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) 物件的建構。 系結器會將值系結至提供之類型的所有公用讀取/寫入屬性。 欄位**未**系結。

範例應用程式包含 `Starship` 模型 (*Models/Starship.cs*)：

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

當範例應用程式使用 JSON 設定提供者來載入設定時，*starship.json* 檔案的 `starship` 區段會建立設定：

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

會建立下列設定機碼值組：

| 索引鍵                   | {2&gt;值&lt;2}                                             |
| --------------------- | ------------------------------------------------- |
| starship:name         | USS Enterprise                                    |
| starship:registry     | NCC-1701                                          |
| starship:class        | Constitution                                      |
| starship:length       | 304.8                                             |
| starship:commissioned | False                                             |
| trademark             | Paramount Pictures Corp. https://www.paramount.com |

範例應用程式使用 `starship` 機碼呼叫 `GetSection`。 `starship` 機碼值組會被隔離。 會在子區段上呼叫 `Bind` 方法，並傳入 `Starship` 類別的執行個體。 繫結執行個體值之後，執行個體會被指派給屬性以用於轉譯：

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

## <a name="bind-to-an-object-graph"></a>繫結至物件圖形

<xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> 可以繫結整個 POCO 物件圖形。 如同系結簡單的物件，只會系結公用讀取/寫入屬性。

範例包含 `TvShow` 模型，其物件圖形包括 `Metadata` 與 `Actors` 類別 (*Models/TvShow.cs*)：

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

範例應用程式有 *tvshow.xml* 檔案，其中包含設定資料：

::: moniker range=">= aspnetcore-3.0"

[!code-xml[](index/samples/3.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

設定會被繫結到整個 `TvShow` 物件 (使用 `Bind` 方法)。 已繫結的執行個體會被指派給屬性以用於轉譯：

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) 會繫結並傳回所指定型別。 `Get<T>` 比使用 `Bind` 更方便。 下列程式碼顯示如何根據上面的範例使用 `Get<T>`，這可讓您直接將已繫結的執行個體指派給屬性以用於轉譯：

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

## <a name="bind-an-array-to-a-class"></a>將陣列繫結到類別

範例應用程式示範此節中解釋的概念。

<xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> 支援在設定機碼中使用陣列索引將陣列繫結到物件。 公開數值索引鍵區段（`:0:`、`:1:`&hellip; `:{n}:`）的任何陣列格式，都能夠系結至 POCO 類別陣列。

> [!NOTE]
> 繫結是由慣例提供。 自訂設定提供者不需要實作陣列繫結。

**記憶體內陣列處理**

考慮下表中顯示的設定機碼與值。

| 索引鍵             | {2&gt;值&lt;2}  |
| :-------------: | :----: |
| array:entries:0 | value0 |
| array:entries:1 | value1 |
| array:entries:2 | value2 |
| array:entries:4 | value4 |
| array:entries:5 | value5 |

這些機碼與值是使用「記憶體設定提供者」在範例應用程式中載入：

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

::: moniker-end

陣列會跳過索引 &num;3 的值。 設定繫結程式無法繫結 Null 值或在已繫結的物件中建立 Null 項目，這在示範繫結此陣列的結果到某物件的時候已經很清楚。

在範例應用程式中，POCO 類別可用來存放繫結設定資料：

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

設定資料已繫結到物件：

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

也可以使用 [ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) 語法，其會產生更精簡的程式碼：

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

繫結物件 (`ArrayExample`的執行個體) 會從設定接收陣列資料。

| `ArrayExample.Entries` 索引 | `ArrayExample.Entries` 值 |
| :--------------------------: | :--------------------------: |
| 0                            | value0                       |
| 1                            | value1                       |
| 2                            | value2                       |
| 3                            | value4                       |
| 4                            | value5                       |

繫結物件中的索引 &num;3 存放 `array:4` 設定機碼與其 `value4` 的設定資料。 當繫結包含陣列的設定資料時，設定機碼中的陣列索引只會用來列舉設定資料 (當建立物件時)。 設定資料中不能保留 Null 值，當設定機碼中的陣列略過一或多個索引時，不會在繫結物件中建立 Null 值項目。

由會在設定中產生正確機碼值組的任何設定提供者繫結到 `ArrayExample` 執行個體之前，可以提供索引 &num;3 缺少的設定項目。 若範例包含具有缺少之機碼值組的額外 JSON 設定提供者，`ArrayExample.Entries` 會符合完整的設定陣列：

*missing_value.json*：

```json
{
  "array:entries:3": "value3"
}
```

在 `ConfigureAppConfiguration`中：

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

表格中顯示的機碼值組會載入到設定中。

| 索引鍵             | {2&gt;值&lt;2}  |
| :-------------: | :----: |
| array:entries:3 | value3 |

在「JSON 設定提供者」包含索引 &num;3 的項目之後，若繫結 `ArrayExample` 類別執行個體，`ArrayExample.Entries` 陣列會包括這些值：

| `ArrayExample.Entries` 索引 | `ArrayExample.Entries` 值 |
| :--------------------------: | :--------------------------: |
| 0                            | value0                       |
| 1                            | value1                       |
| 2                            | value2                       |
| 3                            | value3                       |
| 4                            | value4                       |
| 5                            | value5                       |

**JSON 陣列處理**

若 JSON 檔案包含陣列，會為具有以零為基礎之區段索引的陣列元素建立設定機碼。 在下列設定檔中，`subsection` 是陣列：

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

「JSON 設定提供者」會將設定資料讀入到下列機碼值組：

| 索引鍵                     | {2&gt;值&lt;2}  |
| ----------------------- | :----: |
| json_array:key          | valueA |
| json_array:subsection:0 | valueB |
| json_array:subsection:1 | valueC |
| json_array:subsection:2 | valueD |

在範例應用程式中，下列 POCO 類別可用來繫結設定機碼值組：

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

在繫結之後，`JsonArrayExample.Key` 會存放值 `valueA`。 子區段值會存放在 POCO 陣列屬性 `Subsection` 中。

| `JsonArrayExample.Subsection` 索引 | `JsonArrayExample.Subsection` 值 |
| :---------------------------------: | :---------------------------------: |
| 0                                   | valueB                              |
| 1                                   | valueC                              |
| 2                                   | valueD                              |

## <a name="custom-configuration-provider"></a>自訂設定提供者

範例應用程式示範如何建立會使用 [Entity Framework (EF)](/ef/core/) 從資料庫讀取設定機碼值組的基本設定提供者。

提供者具有下列特性：

* EF 記憶體內資料庫會用於示範用途。 若要使用需要連接字串的資料庫，請實作第二個 `ConfigurationBuilder` 以從另一個設定提供者提供連接字串。
* 啟動時，該提供者會將資料庫資料表讀入到設定中。 該提供者不會以個別機碼為基礎查詢資料庫。
* 未實作變更時重新載入，因此在應用程式啟動後更新資料庫對應用程式設定沒有影響。

定義 `EFConfigurationValue` 實體來在資料庫中存放設定值。

::: moniker range=">= aspnetcore-3.0"

*Models/EFConfigurationValue.cs*：

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

新增 `EFConfigurationContext` 以存放及存取已設定的值。

*EFConfigurationProvider/EFConfigurationContext.cs*：

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

建立會實作 <xref:Microsoft.Extensions.Configuration.IConfigurationSource> 的類別。

*EFConfigurationProvider/EFConfigurationSource.cs*：

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

透過繼承自 <xref:Microsoft.Extensions.Configuration.ConfigurationProvider> 來建立自訂設定提供者。 若資料庫是空的，設定提供者會初始化資料庫。 由於設定索引[鍵不區分大小寫](#keys)，用來初始化資料庫的字典會以不區分大小寫的比較子（[StringComparer. OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)）來建立。

*EFConfigurationProvider/EFConfigurationProvider.cs*：

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

`AddEFConfiguration` 擴充方法允許新增設定來源到 `ConfigurationBuilder`。

*Extensions/EntityFrameworkExtensions.cs*:

[!code-csharp[](index/samples/3.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

下列程式碼顯示如何在 *Program.cs* 中使用自訂 `EFConfigurationProvider`：

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

*Models/EFConfigurationValue.cs*：

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

新增 `EFConfigurationContext` 以存放及存取已設定的值。

*EFConfigurationProvider/EFConfigurationContext.cs*：

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

建立會實作 <xref:Microsoft.Extensions.Configuration.IConfigurationSource> 的類別。

*EFConfigurationProvider/EFConfigurationSource.cs*：

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

透過繼承自 <xref:Microsoft.Extensions.Configuration.ConfigurationProvider> 來建立自訂設定提供者。 若資料庫是空的，設定提供者會初始化資料庫。

*EFConfigurationProvider/EFConfigurationProvider.cs*：

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

`AddEFConfiguration` 擴充方法允許新增設定來源到 `ConfigurationBuilder`。

*Extensions/EntityFrameworkExtensions.cs*:

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

下列程式碼顯示如何在 *Program.cs* 中使用自訂 `EFConfigurationProvider`：

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

::: moniker-end

## <a name="access-configuration-during-startup"></a>在啟動期間存取組態

將 `IConfiguration` 插入到 `Startup` 建構函式，以存取 `Startup.ConfigureServices` 中的設定值。 若要存取 `Startup.Configure` 中的設定，請直接將 `IConfiguration` 插入到方法或從建構函式使用執行個體：

```csharp
public class Startup
{
    private readonly IConfiguration _config;

    public Startup(IConfiguration config)
    {
        _config = config;
    }

    public void ConfigureServices(IServiceCollection services)
    {
        var value = _config["key"];
    }

    public void Configure(IApplicationBuilder app, IConfiguration config)
    {
        var value = config["key"];
    }
}
```

如需使用啟動方便方法來存取設定的範例，請參閱[應用程式啟動：方便方法](xref:fundamentals/startup#convenience-methods)。

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a>存取 Razor Pages 頁面或 MVC 檢視中的設定

若要在 [Razor Pages 頁面] 頁面或 MVC 檢視中存取組態設定，請針對 [Microsoft.Extensions.Configuration 命名空間](xref:Microsoft.Extensions.Configuration)[using 指示詞](xref:mvc/views/razor#using) ([C# 參考：using 指示詞](/dotnet/csharp/language-reference/keywords/using-directive))，並將 <xref:Microsoft.Extensions.Configuration.IConfiguration> 插入到頁面或檢視中。

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
    <p>Configuration value for 'key': @Configuration["key"]</p>
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
    <p>Configuration value for 'key': @Configuration["key"]</p>
</body>
</html>
```

## <a name="add-configuration-from-an-external-assembly"></a>從外部組件新增設定

<xref:Microsoft.AspNetCore.Hosting.IHostingStartup> 實作允許在啟動時從應用程式 `Startup` 類別外部的外部組件，針對應用程式新增增強功能。 如需詳細資訊，請參閱<xref:fundamentals/configuration/platform-specific-configuration>。

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/configuration/options>
