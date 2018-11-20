---
title: ASP.NET Core 的設定
author: guardrex
description: 了解如何使用組態 API 設定 ASP.NET Core 應用程式。
ms.author: riande
ms.custom: mvc
ms.date: 11/15/2018
uid: fundamentals/configuration/index
ms.openlocfilehash: 766ac77a2af01509f8e4bc646a18f7dfbc923511
ms.sourcegitcommit: d3392f688cfebc1f25616da7489664d69c6ee330
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/16/2018
ms.locfileid: "51818391"
---
# <a name="configuration-in-aspnet-core"></a>ASP.NET Core 的設定

作者：[Luke Latham](https://github.com/guardrex)

ASP.NET Core 中的應用程式設定是以由*設定提供者*所建立的機碼值組為基礎。 設定提供者會從各種設定來源將設定資料讀取到機碼值組中：

::: moniker range=">= aspnetcore-2.1"

* Azure Key Vault
* 命令列引數
* 自訂提供者 (已安裝或已建立)
* 目錄檔案
* 環境變數
* 記憶體內部 .NET 物件
* 設定檔

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

* Azure Key Vault
* 命令列引數
* 自訂提供者 (已安裝或已建立)
* 環境變數
* 記憶體內部 .NET 物件
* 設定檔

::: moniker-end

::: moniker range="= aspnetcore-1.0"

* 命令列引數
* 自訂提供者 (已安裝或已建立)
* 環境變數
* 記憶體內部 .NET 物件
* 設定檔

::: moniker-end

*選項模式*是此主題中所述之設定概念的延伸。 選項使用類別來代表一組相關的設定。 如需使用選項模式的詳細資訊，請參閱 <xref:fundamentals/configuration/options>。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

此主題中提供的範例需要：

* 使用 <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> 設定應用程式的基底路徑。 參考 [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) 套件即可讓應用程式使用 `SetBasePath`。
* 使用 <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> 解析設定檔的區段。 參考 [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) 套件即可讓應用程式使用 `GetSection`。
* 使用 <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> 與 [Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) 將設定繫結到 .NET 類別。 參考 [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) 套件即可讓應用程式使用 `Bind` 與 `Get<T>`。 ASP.NET Core 1.1 或更新版本中提供了 `Get<T>`。

::: moniker range=">= aspnetcore-2.1"

這些套件均包含在 [Microsoft.AspNetCore.App j5/ 中繼套件](xref:fundamentals/metapackage-app)中。

::: moniker-end

::: moniker range="= aspnetcore-2.0"

這三個套件都包含在 [Microsoft.AspNetCore.All 中繼套件](xref:fundamentals/metapackage)中。

::: moniker-end

## <a name="host-vs-app-configuration"></a>主機與應用程式設定的比較

設定及啟動應用程式之前，會先設定及啟動「主機」。 主機負責應用程式啟動和存留期管理。 應用程式與主機都是使用此主題中所述的設定提供者來設定的。 主機設定機碼值組會成為應用程式全域設定的一部分。 如需有關當建置主機時如何使用設定提供者的詳細資訊，以及設定來源如何影響主機設定的詳細資訊，請參閱 <xref:fundamentals/host/index>。

## <a name="security"></a>安全性

採用下列最佳做法：

* 永遠不要將密碼或其他敏感性資料儲存在設定提供者程式碼或純文字設定檔中。
* 不要在開發或測試環境中使用生產環境祕密。
* 請在專案外部指定祕密，以防止其意外認可至開放原始碼存放庫。

深入了解[如何使用多個環境](xref:fundamentals/environments)及[使用祕密管理員管理開發中的應用程式祕密安全儲存體](xref:security/app-secrets) (包括有關使用環境變數來存放敏感性資料的建議)。 「祕密管理員」使用「檔案設定提供者」以 JSON 檔案在本機系統上存放使用者祕密。 此主題稍後將說明「檔案設定提供者」。

[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) 是安全儲存應用程式祕密的一個選項。 如需詳細資訊，請參閱<xref:security/key-vault-configuration>。

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

在應用程式啟動時，會依照設定來源的設定提供者的指定順序讀入設定來源。

「檔案設定提供者」可以在應用程式啟動之後且底層設定檔案變更時重新載入設定。 此主題稍後將說明「檔案設定提供者」。

您可以在應用程式的[相依性插入 (DI)](xref:fundamentals/dependency-injection) 容器中找到 <xref:Microsoft.Extensions.Configuration.IConfiguration>。 設定提供者無法使用 DI，因為當它們由主機設定時，它無法使用。

設定機碼會採用下列慣例：

* 機碼區分大小寫。 例如，`ConnectionString` 與 `connectionstring` 會被視為相等的機碼。
* 若相同機碼的值是由相同或不同設定提供者設定，在機碼上設定的最後一個值是所使用的值。
* 階層式機碼
  * 在設定 API 內，冒號分隔字元 (`:`) 可在所有平台上運作。
  * 在環境變數中，冒號分隔字元可能無法在所有平台上運作。 所有平台都支援雙底線 (`__`)，而且它會被轉換為冒號。
  * 在 Azure Key Vault 中，階層式機碼使用 `--` (兩個破折號) 來做為分隔符號。 您必須提供程式碼，在祕密載入到應用程式的設定時將破折號取代為冒號。
* <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> 支援在設定機碼中使用陣列索引將陣列繫結到物件。 [將陣列繫結到類別](#bind-an-array-to-a-class)一節說明陣列繫結。

設定值會採用下列慣例：

* 值是字串。
* Null 值無法存放在設定中或繫結到物件。

## <a name="providers"></a>提供者

下表顯示可供 ASP.NET Core 應用程式使用的設定提供者。

::: moniker range=">= aspnetcore-2.1"

| 提供者 | 從&hellip;提供設定 |
| -------- | ----------------------------------- |
| [Azure Key Vault 設定提供者](xref:security/key-vault-configuration) (*安全性*主題) | Azure Key Vault |
| [命令列設定提供者](#command-line-configuration-provider) | 命令列參數 |
| [自訂設定提供者](#custom-configuration-provider) | 自訂來源 |
| [環境變數設定提供者](#environment-variables-configuration-provider) | 環境變數 |
| [檔案設定提供者](#file-configuration-provider) | 檔案 (INI、JSON、XML) |
| [每個檔案機碼的設定提供者](#key-per-file-configuration-provider) | 目錄檔案 |
| [記憶體設定提供者](#memory-configuration-provider) | 記憶體內集合 |
| [使用者祕密 (祕密管理員)](xref:security/app-secrets) (*安全性*主題) | 使用者設定檔目錄中的檔案 |

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

| 提供者 | 從&hellip;提供設定 |
| -------- | ----------------------------------- |
| [Azure Key Vault 設定提供者](xref:security/key-vault-configuration) (*安全性*主題) | Azure Key Vault |
| [命令列設定提供者](#command-line-configuration-provider) | 命令列參數 |
| [自訂設定提供者](#custom-configuration-provider) | 自訂來源 |
| [環境變數設定提供者](#environment-variables-configuration-provider) | 環境變數 |
| [檔案設定提供者](#file-configuration-provider) | 檔案 (INI、JSON、XML) |
| [記憶體設定提供者](#memory-configuration-provider) | 記憶體內集合 |
| [使用者祕密 (祕密管理員)](xref:security/app-secrets) (*安全性*主題) | 使用者設定檔目錄中的檔案 |

::: moniker-end

::: moniker range="= aspnetcore-1.0"

| 提供者 | 從&hellip;提供設定 |
| -------- | ----------------------------------- |
| [命令列設定提供者](#command-line-configuration-provider) | 命令列參數 |
| [自訂設定提供者](#custom-configuration-provider) | 自訂來源 |
| [環境變數設定提供者](#environment-variables-configuration-provider) | 環境變數 |
| [檔案設定提供者](#file-configuration-provider) | 檔案 (INI、JSON、XML) |
| [記憶體設定提供者](#memory-configuration-provider) | 記憶體內集合 |
| [使用者祕密 (祕密管理員)](xref:security/app-secrets) (*安全性*主題) | 使用者設定檔目錄中的檔案 |

::: moniker-end

在啟動時，會依照設定來源的設定提供者的指定順序讀入設定來源。 此主題中所述的設定提供者是以字母順序描述，而非以您的程式碼安排它們的順序。 在您的程式碼中針對底層設定來源的優先順序，為設定提供者排序。

典型的設定提供者順序是：

1. 檔案 (*appsettings.json*、*appsettings.{Environment}.json*，其中 `{Environment}` 是應用程式的目前裝載環境)
1. [Azure Key Vault](xref:security/key-vault-configuration)
1. [使用者祕密 (祕密管理員)](xref:security/app-secrets) (僅限開發環境)
1. 環境變數
1. 命令列引數

將命令列設定提供者放在提供者序列結尾是常見作法，因為這樣可以讓命令列引數覆寫由其它提供者所設定的設定。

::: moniker range=">= aspnetcore-2.0"

當您使用 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>初始化新的 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 時，就會有這個提供者順序。 如需詳細資訊，請參閱 [Web主機：設定主機](xref:fundamentals/host/web-host#set-up-a-host)。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

您可以使用 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 並在 `Startup` <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> 方法，以為應用程式 (而非主機) 建立提供者順序：

```csharp
public Startup(IHostingEnvironment env)
{
    var builder = new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
        .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, 
            reloadOnChange: true);

    var appAssembly = Assembly.Load(new AssemblyName(env.ApplicationName));

    if (appAssembly != null)
    {
        builder.AddUserSecrets(appAssembly, optional: true);
    }

    builder.AddEnvironmentVariables();

    Configuration = builder.Build();
}

public IConfiguration Configuration { get; }

public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IConfiguration>(Configuration);
}
```

在上面的範例中，環境名稱 (`env.EnvironmentName`) 與應用程式組件名稱 (`env.ApplicationName`) 是由 <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> 提供。 如需詳細資訊，請參閱<xref:fundamentals/environments>。

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="configureappconfiguration"></a>ConfigureAppConfiguration

建置 Web 主機時呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 以指定應用程式的設定提供者，以及由 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 自動新增的設定提供者：

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=19)]

::: moniker-end

## <a name="command-line-configuration-provider"></a>命令列設定提供者

<xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> 會在執行階段從命令列引數機碼值組載入設定。

為了啟用命令列設定，<xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 延伸模組方法會在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫。

::: moniker range=">= aspnetcore-2.0"

當您使用 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 初始化新的 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 時，會自動呼叫 `AddCommandLine`。 如需詳細資訊，請參閱 [Web主機：設定主機](xref:fundamentals/host/web-host#set-up-a-host)。

`CreateDefaultBuilder` 也會載入：

* 從 *appsettings.json* 與 *appsettings.{Environment}.json* 載入其他選擇性設定。
* [使用者祕密 (祕密管理員)](xref:security/app-secrets) (在開發環境中)。
* 環境變數。

`CreateDefaultBuilder` 會最後才新增命令列設定提供者。 在執行階段傳遞的命令列引數會覆寫由其它提供者所設定的設定。

`CreateDefaultBuilder` 會在建構主機時執行作業。 因此，由`CreateDefaultBuilder` 啟用的命令列設定可以影響主機的設定方式。

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

建置主機時呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 以指定應用程式的設定。

`CreateDefaultBuilder` 已經呼叫 `AddCommandLine`。 如果您需要提供應用程式設定並仍要能夠使用命令列引數覆寫該設定，請在 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 中呼叫應用程式的其他提供者，最後呼叫 `AddCommandLine`。

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                // Call other providers here and call AddCommandLine last.
                config.AddCommandLine(args);
            })
            .UseStartup<Startup>();
}
```

建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：

::: moniker-end

::: moniker range="= aspnetcore-2.0"

使用 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> 方法將設定套用到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>。

呼叫 `UseConfiguration` 時，`CreateDefaultBuilder` 已經呼叫 `AddCommandLine`。 如果您需要提供應用程式設定並仍要能夠使用命令列引數覆寫該設定，請在 `ConfigurationBuilder` 上呼叫應用程式的其他提供者，最後呼叫 `AddCommandLine`。

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            // Call other providers here and call AddCommandLine last.
            .AddCommandLine(args)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：

::: moniker-end

::: moniker range="< aspnetcore-2.0"

若要啟用命令列設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 延伸模組方法。

最後才呼叫提供者，以允許在執行階段傳遞的命令列引數覆寫由其他設定提供者所設定的設定。

使用 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> 方法將設定套用到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>：

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    // Call additional providers here as needed.
    // Call AddCommandLine last to allow arguments to override other configuration.
    .AddCommandLine(args)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

**範例**

::: moniker range=">= aspnetcore-2.0"

2.x 範例應用程式可發揮靜態方便方法 `CreateDefaultBuilder` 的優勢以建置主機，這包括對 <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 的呼叫。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

1.x 範例應用程式會呼叫 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 上的 <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>。

::: moniker-end

1. 從專案目錄中開啟命令提示字元。
1. 提供命令列引數給 `dotnet run` 命令 `dotnet run CommandLineKey=CommandLineValue`。
1. 在應用程式執行之後，開啟瀏覽器以瀏覽位於 `http://localhost:5000` 的應用程式。
1. 觀察輸出是否包含提供給 `dotnet run` 之設定命令列引數的機碼值組。

### <a name="arguments"></a>引數

當值後面接著空格時，值必須接著等號 (`=`)，或機碼必須有前置詞 (`--` 或 `/`)。 若使用等號 (例如 `CommandLineKey=`)，值可以是 Null。

| 索引鍵前置字元               | 範例                                                |
| ------------------------ | ------------------------------------------------------ |
| 沒有前置字元                | `CommandLineKey1=value1`                               |
| 雙虛線 (`--`)        | `--CommandLineKey2=value2`、 `--CommandLineKey2 value2` |
| 正斜線 (`/`)      | `/CommandLineKey3=value3`、 `/CommandLineKey3 value3`   |

在相同的命令中，請不要混合使用等號搭配使用空格之機碼值組的命令列引數。

範例命令：

```console
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a>切換對應

參數對應允許索引鍵名稱取代邏輯。 當您使用 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 手動建置設定時，可以提供切換取代的字典給 <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 方法。

使用切換對應字典時，會檢查字典中是否有任何索引鍵符合命令列引數所提供的索引鍵。 如果在字典中找到命令列索引鍵，則會傳回字典值 (索引鍵取代) 以在應用程式的設定中設定機碼值組。 所有前面加上單虛線 (`-`) 的命令列索引鍵都需要切換對應。

切換對應字典索引鍵規則：

* 切換必須以單虛線 (`-`) 或雙虛線 (`--`) 開頭。
* 切換對應字典不能包含重複索引鍵。

::: moniker range=">= aspnetcore-2.1"

建置主機時呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 以指定應用程式的設定：

```csharp
public class Program
{
    public static readonly Dictionary<string, string> _switchMappings = 
        new Dictionary<string, string>
        {
            { "-CLKey1", "CommandLineKey1" },
            { "-CLKey2", "CommandLineKey2" }
        };

    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    // Do not pass the args to CreateDefaultBuilder
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder()
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.AddCommandLine(args, _switchMappings);
            })
            .UseStartup<Startup>();
}
```

如上面的範例所示，當使用切換對應時，對 `CreateDefaultBuilder` 的呼叫不應該傳遞引數。 `CreateDefaultBuilder` 方法的 `AddCommandLine` 呼叫不包括對應的切換，而且沒有任何方式可以將切換對應字典傳遞給 `CreateDefaultBuilder`。 若對應切換中包含引數且已傳遞給 `CreateDefaultBuilder`，其 `AddCommandLine` 提供者會無法使用 <xref:System.FormatException> 來初始化。 解決方案並非將引數傳遞給 `CreateDefaultBuilder`，而是允許 `ConfigurationBuilder` 方法的 `AddCommandLine` 方法同時處理引數與切換對應字典。

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var switchMappings = new Dictionary<string, string>
            {
                { "-CLKey1", "CommandLineKey1" },
                { "-CLKey2", "CommandLineKey2" }
            };

        var config = new ConfigurationBuilder()
            .AddCommandLine(args, switchMappings)
            .Build();

        // Do not pass the args to CreateDefaultBuilder
        return WebHost.CreateDefaultBuilder()
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

如上面的範例所示，當使用切換對應時，對 `CreateDefaultBuilder` 的呼叫不應該傳遞引數。 `CreateDefaultBuilder` 方法的 `AddCommandLine` 呼叫不包括對應的切換，而且沒有任何方式可以將切換對應字典傳遞給 `CreateDefaultBuilder`。 若對應切換中包含引數且已傳遞給 `CreateDefaultBuilder`，其 `AddCommandLine` 提供者會無法使用 <xref:System.FormatException> 來初始化。 解決方案並非將引數傳遞給 `CreateDefaultBuilder`，而是允許 `ConfigurationBuilder` 方法的 `AddCommandLine` 方法同時處理引數與切換對應字典。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public static void Main(string[] args)
{
    var switchMappings = new Dictionary<string, string>
        {
            { "-CLKey1", "CommandLineKey1" },
            { "-CLKey2", "CommandLineKey2" }
        };

    var config = new ConfigurationBuilder()
        .AddCommandLine(args, switchMappings)
        .Build();

    var host = new WebHostBuilder()
        .UseConfiguration(config)
        .UseKestrel()
        .UseStartup<Startup>()
        .Start();

    using (host)
    {
        Console.ReadLine();
    }
}
```

::: moniker-end

建立切換對應字典之後，它會包含下表中所示的資料。

| Key       | 值             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

若啟動應用程式時使用切換對應機碼，設定會接收由字典提供之機碼上的設定值：

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

執行上述命令之後，設定包含下表中顯示的值。

| Key               | 值    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a>環境變數設定提供者

<xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> 會在執行階段從環境變數機碼值組載入設定。

若要啟用環境變數設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> 延伸模組方法。

在環境變數中搭配階層式機碼使用時，冒號分隔字元 (`:`) 可能無法在所有平台上運作。 所有平台都支援雙底線 (`__`)，而且它會被冒號取代。

[Azure App Service](https://azure.microsoft.com/services/app-service/) 允許您在 Azure 入口網站中設定環境變數，以使用「環境變數設定提供者」覆寫應用程式設定。 如需詳細資訊，請參閱 [Azure App：使用 Azure 入口網站覆寫應用程式設定](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal)。

::: moniker range=">= aspnetcore-2.0"

當您初始新的 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 時，會為首碼為 `ASPNETCORE_` 的環境變數自動呼叫 `AddEnvironmentVariables`。 如需詳細資訊，請參閱 [Web主機：設定主機](xref:fundamentals/host/web-host#set-up-a-host)。

`CreateDefaultBuilder` 也會載入：

* 來自無首碼之環境變數的應用程式組態 (在未提供首碼的情況下呼叫 `AddEnvironmentVariables`)。
* 從 *appsettings.json* 與 *appsettings.{Environment}.json* 載入其他選擇性設定。
* [使用者祕密 (祕密管理員)](xref:security/app-secrets) (在開發環境中)。
* 命令列引數。

從使用者祕密與 *appsettings* 檔案建立設定之後，會呼叫「環境變數設定提供者」。 在此位置呼叫提供者可讓系統在執行階段讀取環境變數，以覆寫由使用者祕密與 *appsettings* 檔案所設定的設定。

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

建置主機時呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 以指定應用程式的設定。

如果您需要從其他環境變數提供應用程式設定，請呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 中的應用程式其他提供者，並呼叫具有該前置詞的 `AddEnvironmentVariables`。

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                // Call additional providers here as needed.
                // Call AddEnvironmentVariables last if you need to allow environment
                // variables to override values from other providers.
                config.AddEnvironmentVariables(prefix: "PREFIX_");
            })
            .UseStartup<Startup>();
}
```

建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：

::: moniker-end

::: moniker range="= aspnetcore-2.0"

呼叫 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 執行個體上的 `AddEnvironmentVariables` 延伸模組方法。 使用 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> 方法將設定套用到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>。

如果您需要從其他環境變數提供應用程式設定，請呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 中的應用程式其他提供者，並呼叫具有該前置詞的 `AddEnvironmentVariables`。

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            // Call additional providers here as needed.
            // Call AddEnvironmentVariables last if you need to allow environment
            // variables to override values from other providers.
            .AddEnvironmentVariables(prefix: "PREFIX_")
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：

::: moniker-end

::: moniker range="< aspnetcore-2.0"

使用 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> 方法將設定套用到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>：

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables()
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

**範例**

::: moniker range=">= aspnetcore-2.0"

2.x 範例應用程式可發揮靜態方便方法 `CreateDefaultBuilder` 的優勢以建置主機，這包括對 `AddEnvironmentVariables` 的呼叫。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

1.x 範例應用程式會呼叫 `ConfigurationBuilder` 上的 `AddEnvironmentVariables`。

::: moniker-end

1. 執行範例應用程式。 開啟瀏覽器以瀏覽位於 `http://localhost:5000` 的應用程式。
1. 觀察輸出是否包含環境變數 `ENVIRONMENT` 的機碼值組。 值反映應用程式執行所在環境，在本機執行時，通常是 `Development`。

為縮短由應用程式轉譯的環境變數清單，應用程式會篩選開頭如下的環境變數：

* ASPNETCORE_
* urls
* 記錄
* 環境
* contentRoot
* AllowedHosts
* applicationName
* CommandLine

::: moniker range=">= aspnetcore-2.0"

若想要將所有環境變數公開給應用程式使用，請將 *Pages/Index.cshtml.cs* 中的 `FilteredConfiguration` 變更為下面這樣：

::: moniker-end

::: moniker range="< aspnetcore-2.0"

若想要將所有環境變數公開給應用程式使用，請將 *Controllers/HomeController.cs* 中的 `FilteredConfiguration` 變更為下面這樣：

::: moniker-end

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a>首碼

當您將前置詞套用到 `AddEnvironmentVariables` 方法時，會篩選載入到應用程式設定中的環境變數。 例如，若要篩選前置詞為 `CUSTOM_` 的環境變數，請提供前置詞給設定提供者：

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

建立設定機碼值組時，會移除前置詞。

::: moniker range=">= aspnetcore-2.0"

靜態方便方法 `CreateDefaultBuilder` 會建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 以建立應用程式的主機。 當 `WebHostBuilder` 建立時，它會在前置詞為 `ASPNETCORE_` 的環境變數中尋找其主機設定。

::: moniker-end

**連接字串前置詞**

設定 API 四個連接字串環境變數的特殊處理規則，這些這些環境變數牽涉到針對應用程式環境設定 Azure 連接字串。 若將前置詞提供給 `AddEnvironmentVariables`具有下表顯示之前置詞的環境變數。

| 連接字串前置詞 | 提供者 |
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

::: moniker range=">= aspnetcore-2.1"

建置主機時呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 以指定應用程式的設定：

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddIniFile("config.ini", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：

::: moniker-end

::: moniker range="= aspnetcore-2.0"

呼叫 `CreateDefaultBuilder` 時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddIniFile("config.ini", optional: true, reloadOnChange: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：

::: moniker-end

::: moniker range="< aspnetcore-2.0"

使用 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> 方法將設定套用到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>：

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddIniFile("config.ini", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
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

::: moniker range=">= aspnetcore-2.0"

當您使用 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 初始化新的 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 時，會自動呼叫 `AddJsonFile` 兩次。 會呼叫此方法以從下列位置載入設定：

* *appsettings.json* &ndash; 會先讀取此檔案。 檔案的環境版本可以覆寫由 *appsettings.json* 檔案提供的值。
* *appsettings.{Environment}.json* &ndash; 檔案的環境版本是根據 [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*) 來載入的。

如需詳細資訊，請參閱 [Web主機：設定主機](xref:fundamentals/host/web-host#set-up-a-host)。

`CreateDefaultBuilder` 也會載入：

* 環境變數。
* [使用者祕密 (祕密管理員)](xref:security/app-secrets) (在開發環境中)。
* 命令列引數。

會先建立 JSON 設定提供者。 因此，使用者祕密、環境變數與命令列引數會覆寫由 *appsettings* 檔案設定的設定。

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

在建置主機時，呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 以指定檔案 (除了 *appsettings.json* 和  *appsettings.{Environment}.json* 以外) 的應用程式設定：

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddJsonFile("config.json", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：

::: moniker-end

::: moniker range="= aspnetcore-2.0"

您也可以直接呼叫 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 執行個體上的 `AddJsonFile` 延伸模組方法。

呼叫 `CreateDefaultBuilder` 時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("config.json", optional: true, reloadOnChange: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：

::: moniker-end

::: moniker range="< aspnetcore-2.0"

使用 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> 方法將設定套用到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>：

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddJsonFile("config.json", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

**範例**

::: moniker range=">= aspnetcore-2.0"

2.x 範例應用程式可發揮靜態方便方法 `CreateDefaultBuilder` 的優勢以建置主機，這包括對 `AddJsonFile` 的兩次呼叫。 從 *appsettings.json* 與 *appsettings.{Environment}.json* 載入設定。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

1.x 範例應用程式會呼叫 `ConfigurationBuilder` 上的 `AddJsonFile` 兩次。 從 *appsettings.json* 與 *appsettings.{Environment}.json* 載入設定。

::: moniker-end

1. 執行範例應用程式。 開啟瀏覽器以瀏覽位於 `http://localhost:5000` 的應用程式。
1. 觀察輸出是否包含表格中所顯示之設定的機碼值組 (視環境而定)。 記錄設定機碼會使用冒號 (`:`) 做為階層式分隔符號。

| Key                        | 開發值 | 生產值 |
| -------------------------- | :---------------: | :--------------: |
| Logging:LogLevel:System    | 資訊       | 資訊      |
| Logging:LogLevel:Microsoft | 資訊       | 資訊      |
| Logging:LogLevel:Default   | 偵錯             | 錯誤            |
| AllowedHosts               | *                 | *                |

### <a name="xml-configuration-provider"></a>XML 設定提供者

<xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> 會在執行階段從 XML 檔案機碼值組載入設定。

若要啟用 XML 檔案設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> 延伸模組方法。

多載允許指定：

* 檔案是否為選擇性。
* 檔案變更時是否要重新載入設定。
* <xref:Microsoft.Extensions.FileProviders.IFileProvider> 是用於存取該檔案。

建立設定機碼值組時，會忽略設定檔案的根節點。 請勿在檔案中指定文件類型定義 (DTD) 或命名空間。

::: moniker range=">= aspnetcore-2.1"

建置主機時呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 以指定應用程式的設定：

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddXmlFile("config.xml", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：

::: moniker-end

::: moniker range="= aspnetcore-2.0"

呼叫 `CreateDefaultBuilder` 時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddXmlFile("config.xml", optional: true, reloadOnChange: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：

::: moniker-end

::: moniker range="< aspnetcore-2.0"

使用 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> 方法將設定套用到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>：

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddXmlFile("config.xml", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
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

::: moniker range=">= aspnetcore-2.1"

## <a name="key-per-file-configuration-provider"></a>每個檔案機碼設定提供者

<xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> 使用目錄的檔案做為設定機碼值組。 機碼是檔案名稱。 值包含檔案的內容。 每個檔案機碼設定提供者是在 Docker 主機案例中使用。

若要啟用每個檔案機碼設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> 延伸模組方法。 檔案的 `directoryPath` 必須是絕對路徑。

多載允許指定：

* 設定來源的 `Action<KeyPerFileConfigurationSource>` 委派。
* 目錄是否為選擇性與目錄的路徑。

建置主機時呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 以指定應用程式的設定：

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                var path = Path.Combine(Directory.GetCurrentDirectory(), "path/to/files");
                config.AddKeyPerFile(directoryPath: path, optional: true);
            })
            .UseStartup<Startup>();
}
```

建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：

```csharp
var path = Path.Combine(Directory.GetCurrentDirectory(), "path/to/files");
var config = new ConfigurationBuilder()
    .AddKeyPerFile(directoryPath: path, optional: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

::: moniker-end

## <a name="memory-configuration-provider"></a>記憶體設定提供者

<xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> 使用記憶體內集合做為設定機碼值組。

若要啟用記憶體內集合設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> 延伸模組方法。

設定提供者可以使用 `IEnumerable<KeyValuePair<String,String>>` 來初始化。

::: moniker range=">= aspnetcore-2.1"

建置主機時呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> 以指定應用程式的設定：

```csharp
public class Program
{
    public static readonly Dictionary<string, string> _dict = 
        new Dictionary<string, string>
        {
            {"MemoryCollectionKey1", "value1"},
            {"MemoryCollectionKey2", "value2"}
        };

    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.AddInMemoryCollection(_dict);
            })
            .UseStartup<Startup>();
}
```

建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：

::: moniker-end

::: moniker range="= aspnetcore-2.0"

呼叫 `CreateDefaultBuilder` 時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

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
}
```

建立 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 目錄時，請使用下列設定呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>：

::: moniker-end

::: moniker range="< aspnetcore-2.0"

使用 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> 方法將設定套用到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>：

::: moniker-end

```csharp
var dict = new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };

var config = new ConfigurationBuilder()
    .AddInMemoryCollection(dict)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

## <a name="getvalue"></a>GetValue

[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) 會從具有所指定機碼的設定擷取值，並將它轉換為指定的型別。 若找不到機碼，多載允許您提供預設值。

下列範例會從具有機碼 `NumberKey` 的設定擷取字串值、將值的型別設定為 `int`，並將值存放在變數 `intValue` 中。 若在設定機碼中找不到 `NumberKey`，`intValue` 會接收 `99` 的預設值：

```csharp
var intValue = config.GetValue<int>("NumberKey", 99);
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

同樣地，若要取得 `section2:subsection0` 中之機碼的值，請呼叫 `GetSection` 並提供區段路徑：

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

`GetSection` 絕不會傳回 `null`。 若找不到相符的區段，會傳回空的 `IConfigurationSection`。

### <a name="getchildren"></a>GetChildren

對 `section2` 上之 [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) 的呼叫會取得包括下列項目的 `IEnumerable<IConfigurationSection>`：

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

::: moniker range=">= aspnetcore-2.0"

### <a name="exists"></a>存在

使用 [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) 來判斷設定區段是否存在：

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

以範例資料為例， `sectionExists` 是 `false`，因未設定資料中沒有 `section2:subsection2` 區段。

::: moniker-end

## <a name="bind-to-a-class"></a>繫結到類別

設定可以繫結到類別，以使用*選項模式*代表相關設定的群組。 如需詳細資訊，請參閱<xref:fundamentals/configuration/options>。

設定值是以字串傳回，但是呼叫 <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> 會啟用 [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) 物件的建構。

範例應用程式包含 `Starship` 模型 (*Models/Starship.cs*)：

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

當範例應用程式使用 JSON 設定提供者來載入設定時，*starship.json* 檔案的 `starship` 區段會建立設定：

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/starship.json)]

::: moniker-end

會建立下列設定機碼值組：

| Key                   | 值                                             |
| --------------------- | ------------------------------------------------- |
| starship:name         | USS Enterprise                                    |
| starship:registry     | NCC-1701                                          |
| starship:class        | Constitution                                      |
| starship:length       | 304.8                                             |
| starship:commissioned | False                                             |
| trademark             | Paramount Pictures Corp. http://www.paramount.com |

範例應用程式使用 `starship` 機碼呼叫 `GetSection`。 `starship` 機碼值組會被隔離。 會在子區段上呼叫 `Bind` 方法，並傳入 `Starship` 類別的執行個體。 繫結執行個體值之後，執行個體會被指派給屬性以用於轉譯：

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_starship)]

::: moniker-end

## <a name="bind-to-an-object-graph"></a>繫結至物件圖形

<xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> 可以繫結整個 POCO 物件圖形。

範例包含 `TvShow` 模型，其物件圖形包括 `Metadata` 與 `Actors` 類別 (*Models/TvShow.cs*)：

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

範例應用程式有 *tvshow.xml* 檔案，其中包含設定資料：

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-xml[](index/samples/1.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

設定會被繫結到整個 `TvShow` 物件 (使用 `Bind` 方法)。 已繫結的執行個體會被指派給屬性以用於轉譯：

::: moniker range=">= aspnetcore-2.0"

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
viewModel.TvShow = tvShow;
```

::: moniker-end

::: moniker range=">= aspnetcore-1.1"

[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) 會繫結並傳回所指定型別。 `Get<T>` 比使用 `Bind` 更方便。 下列程式碼顯示如何根據上面的範例使用 `Get<T>`，這可讓您直接將已繫結的執行個體指派給屬性以用於轉譯：

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_tvshow)]

::: moniker-end

## <a name="bind-an-array-to-a-class"></a>將陣列繫結到類別

範例應用程式示範此節中解釋的概念。

<xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> 支援在設定機碼中使用陣列索引將陣列繫結到物件。 任何公開數值機碼區段 (`:0:`、`:1:`、&hellip; `:{n}:`) 的陣列格式都能繫結到POCO 類別陣列。

> [!NOTE]
> 繫結是由慣例提供。 自訂設定提供者不需要實作陣列繫結。

**記憶體內陣列處理**

考慮下表中顯示的設定機碼與值。

| Key             | 值  |
| :-------------: | :----: |
| array:entries:0 | value0 |
| array:entries:1 | value1 |
| array:entries:2 | value2 |
| array:entries:4 | value4 |
| array:entries:5 | value5 |

這些機碼與值是使用「記憶體設定提供者」在範例應用程式中載入：

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=3-10,22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=5-12,16)]

::: moniker-end

陣列會跳過索引 &num;3 的值。 設定繫結程式無法繫結 Null 值或在已繫結的物件中建立 Null 項目，這在示範繫結此陣列的結果到某物件的時候已經很清楚。

在範例應用程式中，POCO 類別可用來存放繫結設定資料：

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

設定資料已繫結到物件：

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

::: moniker range=">= aspnetcore-1.1"

[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) 語法也可以使用，這會產生更精簡的程式碼：

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_array)]

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

::: moniker range=">= aspnetcore-2.0"

在 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>中：

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

在 `Startup` 建構函式中：

```csharp
.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

表格中顯示的機碼值組會載入到設定中。

| Key             | 值  |
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

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/json_array.json)]

::: moniker-end

「JSON 設定提供者」會將設定資料讀入到下列機碼值組：

| Key                     | 值  |
| ----------------------- | :----: |
| json_array:key          | valueA |
| json_array:subsection:0 | valueB |
| json_array:subsection:1 | valueC |
| json_array:subsection:2 | valueD |

在範例應用程式中，下列 POCO 類別可用來繫結設定機碼值組：

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

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

*Models/EFConfigurationValue.cs*：

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

新增 `EFConfigurationContext` 以存放及存取已設定的值。

*EFConfigurationProvider/EFConfigurationContext.cs*：

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

建立會實作 <xref:Microsoft.Extensions.Configuration.IConfigurationSource> 的類別。

*EFConfigurationProvider/EFConfigurationSource.cs*：

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

透過繼承自 <xref:Microsoft.Extensions.Configuration.ConfigurationProvider> 來建立自訂設定提供者。 若資料庫是空的，設定提供者會初始化資料庫。

*EFConfigurationProvider/EFConfigurationProvider.cs*：

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

`AddEFConfiguration` 擴充方法允許新增設定來源到 `ConfigurationBuilder`。

*Extensions/EntityFrameworkExtensions.cs*:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

下列程式碼顯示如何在 *Program.cs* 中使用自訂 `EFConfigurationProvider`：

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=26)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=24)]

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

若要在 [Razor Pages 頁面] 頁面或 MVC 檢視中存取組態設定，請針對 [Microsoft.Extensions.Configuration 命名空間](xref:Microsoft.Extensions.Configuration) [using 指示詞](xref:mvc/views/razor#using) ([C# 參考：using 指示詞](/dotnet/csharp/language-reference/keywords/using-directive))，並將 <xref:Microsoft.Extensions.Configuration.IConfiguration> 插入到頁面或檢視中。

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
* [深入了解 Microsoft 設定](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/)
