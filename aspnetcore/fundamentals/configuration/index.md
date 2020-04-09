---
title: ASP.NET Core 的設定
author: rick-anderson
description: 了解如何使用組態 API 設定 ASP.NET Core 應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 3/29/2020
uid: fundamentals/configuration/index
ms.openlocfilehash: d76ca78bc988f859b4e99752a0e88735e1df1d82
ms.sourcegitcommit: 72792e349458190b4158fcbacb87caf3fc605268
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "80501333"
---
# <a name="configuration-in-aspnet-core"></a>ASP.NET Core 的設定

由[里克·安德森](https://twitter.com/RickAndMSFT)和[柯克·拉金](https://twitter.com/serpent5)

::: moniker range=">= aspnetcore-3.0"

ASP.NET核心中的配置使用一個或多個[配置提供程式](#cp)執行。 設定提供者使用各種設定來源從鍵值對讀取設定資料:

* 設定檔,如*應用程式設定.json*
* 環境變數
* Azure 金鑰保存庫
* Azure 應用程式組態
* 命令列引數
* 已安裝或建立的自訂提供者
* 目錄檔案
* 記憶體內部 .NET 物件

[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples)([如何下載](xref:index#how-to-download-a-sample))

<a name="default"></a>

## <a name="default-configuration"></a>預設組態

ASP.NET使用[dotnet 新](/dotnet/core/tools/dotnet-new)工作室或 Visual Studio 建立的核心 Web 應用程式產生以下代碼:

[!code-csharp[](index/samples/3.x/ConfigSample/Program.cs?name=snippet&highlight=9)]

 <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> 會以下列順序提供應用程式的預設組態：

1. [鏈式配置提供程式](xref:Microsoft.Extensions.Configuration.ChainedConfigurationSource):添加`IConfiguration`現有來源。 在預設配置情況下,添加[主機](#hvac)配置並將其設置為_應用_配置的第一個源。
1. [應用程式設定.json](#appsettingsjson)使用[JSON 設定提供者](#file-configuration-provider)。
1. *應用設置。*`Environment` *.json*使用[JSON 設定提供者](#file-configuration-provider)。 例如,*應用程式設定*。***生產***。*json*與*應用程式設定*。***發展***.*json*.
1. 套用`Development`在環境中執行時[的應用程式 。](xref:security/app-secrets)
1. 使用[環境變數配置提供程式](#evcp)的環境變數。
1. 使用[指令列設定提供者](#command-line)的指令列參數 。

稍後添加的配置提供程式將覆蓋以前的鍵設置。 例如,如果在`MyKey` *appset.json*和環境中設置,則使用環境值。 使用預設設定提供者,[命令列設定提供程式](#command-line-configuration-provider)將覆蓋所有其他提供程式。

有關 的詳細`CreateDefaultBuilder`資訊 ,請參閱[預設產生器設定](xref:fundamentals/host/generic-host#default-builder-settings)。

以下代碼按已新增的順序顯示已啟用的設定提供者:

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Index2.cshtml.cs?name=snippet)]

### <a name="appsettingsjson"></a>appsettings.json

請考慮以下*應用程式設定.json*檔:

[!code-json[](index/samples/3.x/ConfigSample/appsettings.json)]

[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)的以下代碼顯示以下幾個設定設定:

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

預設<xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider>依以下順序載入設定:

1. *appsettings.json*
1. *應用設置。*`Environment` *.json* : 例如,*應用程式設定*。***生產***。*json*與*應用程式設定*。***發展***.*json*檔。 檔的環境版本基於[IHosting 環境.環境名稱](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*)載入。 如需詳細資訊，請參閱 <xref:fundamentals/environments>。

*應用程式設定*。`Environment`.*json*值覆蓋*應用設定中的*鍵. 例如，根據預設：

* 在開發中,*應用程式設定 。****發展***.*json*配置覆蓋*應用設置.json*中找到的值。
* 在生產中,*應用程式設定 。****生產***。*json*配置覆蓋*應用設置.json*中找到的值。 例如,將應用部署到 Azure 時。

<a name="optpat"></a>

#### <a name="bind-hierarchical-configuration-data-using-the-options-pattern"></a>使用選項模式繫結分層設定資料

讀取相關設定值的偏好方法是使用[選項模式](xref:fundamentals/configuration/options)。 例如,要讀取以下設定值:

```json
  "Position": {
    "Title": "Editor",
    "Name": "Joe Smith"
  }
```

建立以下`PositionOptions`類別:

[!code-csharp[](index/samples/3.x/ConfigSample/Options/PositionOptions.cs?name=snippet)]

該類型的所有公共讀寫屬性都綁定。 字段***未***綁定。

下列程式碼：

* 調用[配置綁定](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*)`PositionOptions`以將 類綁`Position`定到該 部分。
* 顯示`Position`配置數據。

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test22.cshtml.cs?name=snippet)]

[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*)綁定並返回指定的類型。 `ConfigurationBinder.Get<T>`可能比使用`ConfigurationBinder.Bind`更方便。 以下程式碼展示如何`ConfigurationBinder.Get<T>``PositionOptions`與 類別使用:

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test21.cshtml.cs?name=snippet)]

使用***選項模式***時的另一種方法是繫`Position`結節並將其新增到[相依項的服務容器](xref:fundamentals/dependency-injection)。 在以下代碼中,`PositionOptions`將新增到<xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*>具有與繫結到設定的服務容器中:

[!code-csharp[](index/samples/3.x/ConfigSample/Startup.cs?name=snippet)]

使用此代碼,以下代碼讀取位置選項:

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test2.cshtml.cs?name=snippet)]

使用[預設](#default)設定,*應用設定.json*和*應用程式設定。*`Environment` *.json*檔案啟用與[重新載入OnChange: true](https://github.com/dotnet/extensions/blob/release/3.1/src/Hosting/Hosting/src/Host.cs#L74-L75)。 對*應用設置.json*和*應用程式設置所做的更改。*`Environment` *.json*檔***後***,應用程式啟動由[JSON 配置提供程式](#jcp)讀取。

有關新增其他 JSON 設定檔的資訊,請參考此文件中的[JSON 設定提供者](#jcp)。

<a name="security"></a>

## <a name="security-and-secret-manager"></a>安全和秘密經理

設定資料指南:

* 永遠不要將密碼或其他敏感性資料儲存在設定提供者程式碼或純文字設定檔中。 [機密管理器](xref:security/app-secrets)可用於在開發中儲存機密。
* 不要在開發或測試環境中使用生產環境祕密。
* 請在專案外部指定祕密，以防止其意外認可至開放原始碼存放庫。

[默認情況下](#default),[秘密管理員](xref:security/app-secrets)在*應用設置.json*和應用程式設置後讀取配置*設置。*`Environment` *.json*.

關於儲存密碼或其他敏感資料的詳細資訊:

* <xref:fundamentals/environments>
* <xref:security/app-secrets>:包括有關使用環境變數存儲敏感數據的建議。 機密管理員使用[檔案配置提供程式](#fcp)將使用者機密存儲在本地系統上的 JSON 檔中。

[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) 可安全地儲存 ASP.NET Core 應用程式的應用程式祕密。 如需詳細資訊，請參閱 <xref:security/key-vault-configuration>。

<a name="evcp"></a>

## <a name="environment-variables"></a>環境變數

使用[預設](#default)設定,<xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider>在讀取*appsettings.json*應用設定後,從環境變數鍵值對載入設定 *。*`Environment` *.json*, 和[秘密經理](xref:security/app-secrets). 因此,從環境中讀取的鍵值將覆蓋從*appsettings.json*、*應用設置*讀取的值。`Environment` *.json*和秘密經理。

[!INCLUDE[](~/includes/environmentVarableColon.md)]

以下`set`指令:

* 在 Windows 上設置[上述範例](#appsettingsjson)的環境鍵和值。
* 使用[示例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)時測試設置。 該`dotnet run`命令必須在專案目錄中運行。

```dotnetcli
set MyKey="My key from Environment"
set Position__Title=Environment_Editor
set Position__Name=Environment_Rick
dotnet run
```

前面的環境設定:

* 僅在從它們設置的命令視窗中啟動的進程中設置。
* 使用 Visual Studio 啟動的瀏覽器不會讀取。

以下[setx](/windows-server/administration/windows-commands/setx)命令可用於在 Windows 上設定環境鍵和值。 與`set``setx`不同,設置是保留的。 `/M`在系統環境中設置變數。 如果未使用`/M`交換機,則設置使用者環境變數。

```cmd
setx MyKey "My key from setx Environment" /M
setx Position__Title Setx_Environment_Editor /M
setx Position__Name Environment_Rick /M
```

要測試前面的命令覆蓋*應用設置.json*和應用*設置。*`Environment` *.json*:

* 使用可視化工作室:退出並重新啟動視覺工作室。
* 使用 CLI:啟動新的指令視窗並`dotnet run`輸入 。

使用<xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*>字串呼叫以指定環境變數的前置字串:

[!code-csharp[](index/samples/3.x/ConfigSample/Program.cs?name=snippet4&highlight=12)]

在上述程式碼中：

* `config.AddEnvironmentVariables(prefix: "MyCustomPrefix_")`預設[設定提供者](#default)之後新增 。 有關排序設定提供者的範例,請參考[JSON 設定提供者](#jcp)。
* 使用`MyCustomPrefix_`前置文字設定的環境變數會覆蓋[預設設定提供者](#default)。 這包括沒有首碼的環境變數。

讀取配置鍵值對時,前置碼將被刪除。

以下命令測試自訂前置字串:

```dotnetcli
set MyCustomPrefix_MyKey="My key with MyCustomPrefix_ Environment"
set MyCustomPrefix_Position__Title=Editor_with_customPrefix
set MyCustomPrefix_Position__Name=Environment_Rick_cp
dotnet run
```

[預設設定](#default)增入環境變數和指令列參數,預先設定`DOTNET_`。`ASPNETCORE_` 和`DOTNET_``ASPNETCORE_`首碼由ASP.NET核心用於[主機和應用配置](xref:fundamentals/host/generic-host#host-configuration),但不用於使用者配置。 有關主機和應用設定的詳細資訊,請參閱[.NET 通用主機](xref:fundamentals/host/generic-host)。

在[Azure 應用服務](https://azure.microsoft.com/services/app-service/)上,在 **「設定>設定」** 頁上選擇 **「新應用程式」設定**。 Azure 應用程式服務應用程式設定包括:

* 靜態加密,通過加密通道傳輸。
* 作為環境變數公開。

如需詳細資訊，請參閱 [Azure App：使用 Azure 入口網站覆寫應用程式設定](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal)。

關於 Azure 資料庫連接字串的資訊[,請參考連接字串前置字串 。](#constr)

<a name="clcp"></a>

## <a name="command-line"></a>命令列

使用[預設](#default)設定<xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider>, 在以下設定源之後,從命令列參數鍵值對載入設定:

* *應用程式設定.json*與*應用程式設定*。`Environment`.*json*檔。
* 開發環境中[的應用機密(秘密管理器)。](xref:security/app-secrets)
* 環境變數。

[默認情況下](#default),在命令列重寫配置值上設置的配置值與所有其他配置提供程式一起設置。

### <a name="command-line-arguments"></a>命令列引數

以下指令使用`=`設定鍵與值:

```dotnetcli
dotnet run MyKey="My key from command line" Position:Title=Cmd Position:Name=Cmd_Rick
```

以下指令使用`/`設定鍵與值:

```dotnetcli
dotnet run /MyKey "Using /" /Position:Title=Cmd_ /Position:Name=Cmd_Rick
```

以下指令使用`--`設定鍵與值:

```dotnetcli
dotnet run --MyKey "Using --" --Position:Title=Cmd-- --Position:Name=Cmd--Rick
```

關鍵值:

* 必須遵循`=`,或者鍵必須具有`--``/`首碼或當值跟隨空格時。
* 如果使用`=`,則不是必需的。 例如： `MySetting=` 。

在同一命令中,不要將使用`=`鍵值對的命令列參數鍵值對與使用空格的鍵值對混合。

### <a name="switch-mappings"></a>切換對應

交換機對應**金鑰**名稱替換邏輯。 提供<xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>方法的交換機替換字典。

使用切換對應字典時，會檢查字典中是否有任何索引鍵符合命令列引數所提供的索引鍵。 如果在字典中找到命令行鍵,則將回傳遞字典值,將鍵值對設置為應用的配置中。 所有前面加上單虛線 (`-`) 的命令列索引鍵都需要切換對應。

切換對應字典索引鍵規則：

* 開關必須從`-``--`或 開始。
* 切換對應字典不能包含重複索引鍵。

要使用交換機映射字典,請將其傳遞到呼叫`AddCommandLine`:

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramSwitch.cs?name=snippet&highlight=10-18,23)]

以下代碼顯示取代金鑰的鍵值:

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test3.cshtml.cs?name=snippet)]

執行以下指令以測試金鑰取代:

```dotnetcli
dotnet run -k1=value1 -k2 value2 --alt3=value2 /alt4=value3 --alt5 value5 /alt6 value6
```

注意:目前,`=`不能使用單個破折`-`號 設置密鑰替換值。 請參閱[這個 GitHub 問題](https://github.com/dotnet/extensions/issues/3059)。

以下命令用於測試金鑰取代:

```dotnetcli
dotnet run -k1 value1 -k2 value2 --alt3=value2 /alt4=value3 --alt5 value5 /alt6 value6
```

針對使用切換對應的應用程式，呼叫 `CreateDefaultBuilder` 不應傳遞引數。 該方法`CreateDefaultBuilder``AddCommandLine`的 呼叫不包括對應的交換機,並且無法將交換機映射字典傳遞`CreateDefaultBuilder`給 。 解不是將參數傳遞給`CreateDefaultBuilder`,而是`ConfigurationBuilder`允許`AddCommandLine`該方法 的方法同時處理參數和開關映射字典。

## <a name="hierarchical-configuration-data"></a>階層式設定資料

設定 API 透過在設定鍵中使用分隔符來拼平分層資料來讀取分層設定資料。

[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)包含以下*應用程式設定.json*檔:

[!code-json[](index/samples/3.x/ConfigSample/appsettings.json)]

[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)的以下代碼顯示幾個設定設定:

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

讀取分層配置數據的首選方法是使用選項模式。 關於詳細資訊,請參考文件中[連結的分層設定資料](#optpat)。

<xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> 與 <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> 方法可用來在設定資料中隔離區段與區段的子系。 [GetSection,、 GetChildren 與 Exists](#getsection) 中說明這些方法。

<!--
[Azure Key Vault configuration provider](xref:security/key-vault-configuration) implement change detection.
-->

## <a name="configuration-keys-and-values"></a>設定鍵與值

設定金鑰:

* 不區分大小寫。 例如，`ConnectionString` 與 `connectionstring` 會被視為相等的機碼。
* 如果在多個配置提供程式中設置了密鑰和值,則使用上次添加提供程式的值。 有關詳細資訊,請參閱[預設設定](#default)。
* 階層式機碼
  * 在設定 API 內，冒號分隔字元 (`:`) 可在所有平台上運作。
  * 在環境變數中，冒號分隔字元可能無法在所有平台上運作。 雙下劃線`__`由所有平台支援,並自動轉換為冒`:`號 。
  * 在 Azure 金鑰保管庫`--`中, 分層鍵用作分隔符。 編寫程式以在將`--`機密載`:`入到應用設定中時取代為 。
* <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> 支援在設定機碼中使用陣列索引將陣列繫結到物件。 [將陣列繫結到類別](#boa)一節說明陣列繫結。

設定值:

* 是字串。
* Null 值無法存放在設定中或繫結到物件。

<a name="cp"></a>

## <a name="configuration-providers"></a>設定提供者

下表顯示可供 ASP.NET Core 應用程式使用的設定提供者。

| 提供者 | 從提供設定 |
| -------- | ----------------------------------- |
| [Azure Key Vault 組態提供者](xref:security/key-vault-configuration) | Azure 金鑰保存庫 |
| [Azure 應用程式設定提供者](/azure/azure-app-configuration/quickstart-aspnet-core-app) | Azure 應用程式組態 |
| [命令列設定提供者](#clcp) | 命令列參數 |
| [自訂設定提供者](#custom-configuration-provider) | 自訂來源 |
| [環境變數設定提供者](#evcp) | 環境變數 |
| [檔案設定提供者](#file-configuration-provider) | INI、JSON 和 XML 檔 |
| [按檔案鍵設定提供者](#key-per-file-configuration-provider) | 目錄檔案 |
| [記憶體設定提供者](#memory-configuration-provider) | 記憶體內集合 |
| [秘密經理](xref:security/app-secrets)  | 使用者設定檔目錄中的檔案 |

按指定其配置提供程式的順序讀取配置源。 在代碼中訂購配置提供程式,以滿足應用所需的基礎配置源的優先順序。

典型的設定提供者順序是：

1. *appsettings.json*
1. *應用程式設定*。`Environment`.*傑森*
1. [秘密經理](xref:security/app-secrets)
1. 使用[環境變數配置提供程式](#evcp)的環境變數。
1. 使用[指令列設定提供者](#command-line-configuration-provider)的指令列參數 。

常見做法是添加命令列配置提供程式在一系列提供程式中的最後一個提供程式,以允許命令列參數覆蓋其他提供程式設置的配置。

前面的提供程式序列用於[預設設定](#default)。

<a name="constr"></a>

### <a name="connection-string-prefixes"></a>連接字串前置詞

設定 API 具有針對四個連接字串環境變數的特殊處理規則。 這些連接字串涉及為應用環境配置 Azure 連接字串。 具有表中顯示的前置碼的環境變數將載入到具有[預設配置](#default)的應用中,或者沒有向`AddEnvironmentVariables`提供前置碼時。

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
| `CUSTOMCONNSTR_{KEY} `   | `ConnectionStrings:{KEY}`   | 設定項目未建立。                                                |
| `MYSQLCONNSTR_{KEY}`     | `ConnectionStrings:{KEY}`   | 機碼：`ConnectionStrings:{KEY}_ProviderName`：<br>值: `MySql.Data.MySqlClient` |
| `SQLAZURECONNSTR_{KEY}`  | `ConnectionStrings:{KEY}`   | 機碼：`ConnectionStrings:{KEY}_ProviderName`：<br>值: `System.Data.SqlClient`  |
| `SQLCONNSTR_{KEY}`       | `ConnectionStrings:{KEY}`   | 機碼：`ConnectionStrings:{KEY}_ProviderName`：<br>值: `System.Data.SqlClient`  |

<a name="jcp"></a>

### <a name="json-configuration-provider"></a>JSON 設定提供者

從<xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider>JSON 檔鍵值對載入配置。

重新載入指定:

* 檔案是否為選擇性。
* 檔案變更時是否要重新載入設定。

請考慮下列程式碼：

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramJSON.cs?name=snippet&highlight=12-14)]

上述程式碼：

* 設定 JSON 設定提供者以載入*MyConfig.json*檔,並包含以下選項:
  * `optional: true`:該檔是可選的。
  * `reloadOnChange: true`:保存更改時將重新載入該檔。
* 在*MyConfig.json*檔之前讀取[預設設定提供者](#default)。 *MyConfig.json*檔中的設定預設設定提供程式中的設定,包括[環境變數設定提供者](#evcp)與[命令列設定提供者](#clcp)。

通常***不希望***[在環境變數配置提供程式](#evcp)和[命令列配置提供程式](#clcp)中設置自定義 JSON 檔重寫值。

以下代碼清除所有設定提供者並新增多個設定提供者:

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramJSON2.cs?name=snippet)]

在前面的代碼中 *,MyConfig.json*和*MyConfig*中的設置。`Environment`.*json*檔案:

* 覆蓋*應用設置.json*和*應用程式設置*中的設置。`Environment`.*json*檔。
* 被[環境變數配置提供程式](#evcp)和[命令列配置提供程式](#clcp)中的設置覆蓋。

[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)包含以下*MyConfig.json*檔:

[!code-json[](index/samples/3.x/ConfigSample/MyConfig.json)]

[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)的以下代碼顯示以下幾個設定設定:

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

<a name="fcp"></a>

## <a name="file-configuration-provider"></a>檔案設定提供者

<xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> 是用於從檔案系統載入設定的基底類別。 以下設定提供者集集`FileConfigurationProvider`:

* [INI 設定提供者](#ini-configuration-provider)
* [JSON 設定提供者](#jcp)
* [XML 設定提供者](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a>INI 設定提供者

<xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> 會在執行階段從 INI 檔案機碼值組載入設定。

以下代碼清除所有設定提供者並新增多個設定提供者:

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramINI.cs?name=snippet&highlight=10-30)]

在前面的代碼中 *,MyIniConfig.ini*和*MyIniConfig*中的設置。`Environment`.*ini*檔案被 以下中的設定覆寫:

* [環境變數設定提供者](#evcp)
* [命令列設定提供者](#clcp)。

[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)包含以下*MyIniConfig.ini*檔案:

[!code-ini[](index/samples/3.x/ConfigSample/MyIniConfig.ini)]

[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)的以下代碼顯示以下幾個設定設定:

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

### <a name="xml-configuration-provider"></a>XML 設定提供者

<xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> 會在執行階段從 XML 檔案機碼值組載入設定。

以下代碼清除所有設定提供者並新增多個設定提供者:

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramXML.cs?name=snippet)]

在前面的代碼中 *,MyXMLFile.xml*和*MyXMLFile*中的設置。`Environment`.*xml*檔案被 以下設定覆寫:

* [環境變數設定提供者](#evcp)
* [命令列設定提供者](#clcp)。

[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)包含以下*MyXMLFile.xml*檔:

[!code-xml[](index/samples/3.x/ConfigSample/MyXMLFile.xml)]

[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)的以下代碼顯示以下幾個設定設定:

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

若 `name` 屬性是用來區別元素，則可以重複那些使用相同元素名稱的元素：

[!code-xml[](index/samples/3.x/ConfigSample/MyXMLFile3.xml)]

以下代碼讀取以前的設定檔並顯示鍵與值:

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/XML/Index.cshtml.cs?name=snippet)]

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

## <a name="key-per-file-configuration-provider"></a>按檔案鍵設定提供者

<xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> 使用目錄的檔案做為設定機碼值組。 機碼是檔案名稱。 值包含檔案的內容。 每個檔金鑰設定提供者用於 Docker 託管方案。

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

<a name="mcp"></a>

## <a name="memory-configuration-provider"></a>記憶體設定提供者

<xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> 使用記憶體內集合做為設定機碼值組。

以下代碼向設定系統新增記憶體集合:

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramArray.cs?name=snippet6)]

[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)的以下代碼顯示前面的設定設定:

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

在前面的代碼中,`config.AddInMemoryCollection(Dict)`預設[設定提供程式](#default)之後新增 。 有關排序設定提供者的範例,請參考[JSON 設定提供者](#jcp)。

有關排序設定提供者的範例,請參考[JSON 設定提供者](#jcp)。

有關`MemoryConfigurationProvider`使用另一個範例,請參閱[連結的陣列](#boa)。

## <a name="getvalue"></a>GetValue

[`ConfigurationBinder.GetValue<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*)使用指定金鑰從設定中提取單個值並將其轉換為指定類型:

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/TestNum.cshtml.cs?name=snippet)]

在前面的代碼中,如果在`NumberKey`配置中找不到,則使用`99`的 預設值。

## <a name="getsection-getchildren-and-exists"></a>GetSection、GetChildren 與 Exists

有關以下範例,請考慮以下*MySub 節.json*檔:

[!code-json[](index/samples/3.x/ConfigSample/MySubsection.json)]

以下代碼將*MySub 節.json*新增到設定提供者:

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramJSONsection.cs?name=snippet)]

### <a name="getsection"></a>GetSection

[I配置.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*)返回具有指定子節鍵的配置子節。

以下代碼傳回的`section1`值 :

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/TestSection.cshtml.cs?name=snippet)]

以下代碼傳回的`section2:subsection0`值 :

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/TestSection2.cshtml.cs?name=snippet)]

`GetSection` 絕不會傳回 `null`。 若找不到相符的區段，會傳回空的 `IConfigurationSection`。

當 `GetSection` 傳回相符區段時，未填入 <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value>。 當區段存在時，會傳回 <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> 與 <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path>。

### <a name="getchildren-and-exists"></a>取得子級與存在

以下代碼呼叫[I 設定.Get 子級](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*)並傳回`section2:subsection0`的值:

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/TestSection4.cshtml.cs?name=snippet)]

前面的代碼呼叫[配置延伸.存在](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*)以驗證該部分是否存在:

 <a name="boa"></a>

## <a name="bind-an-array"></a>繫結陣列

[設定 Binder.Bind](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*)支援使用配置鍵中的陣列索引將數位列綁定到物件。 公開數位鍵段的任何陣列格式都能夠綁定到[POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object)類陣組。

從[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)中考慮*MyArray.json:*

[!code-json[](index/samples/3.x/ConfigSample/MyArray.json)]

以下代碼將*MyArray.json*新增到設定提供者:

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramJSONarray.cs?name=snippet)]

以下代碼讀取設定並顯示值:

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Array.cshtml.cs?name=snippet)]

前面的代碼傳回以下輸出:

```text
Index: 0  Value: value00
Index: 1  Value: value10
Index: 2  Value: value20
Index: 3  Value: value40
Index: 4  Value: value50
```

在前面的輸出中,索引`value40`3 具有`"4": "value40",`值, 對應於*MyArray.json*。 綁定陣列索引是連續的,不綁定到配置密鑰索引。 設定活頁夾無法結合空值或在繫結物件建立空條目

以下程式碼<xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*>使用`array:entries`擴充方法載入設定:

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramArray.cs?name=snippet)]

以下代碼讀取`arrayDict``Dictionary`的設定,並顯示值:

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Array.cshtml.cs?name=snippet)]

前面的代碼傳回以下輸出:

```text
Index: 0  Value: value0
Index: 1  Value: value1
Index: 2  Value: value2
Index: 3  Value: value4
Index: 4  Value: value5
```

繫結物件中的索引 &num;3 存放 `array:4` 設定機碼與其 `value4` 的設定資料。 綁定包含數位的配置資料時,配置鍵中的陣列索引用於在創建物件時反覆運算配置資料。 設定資料中不能保留 Null 值，當設定機碼中的陣列略過一或多個索引時，不會在繫結物件中建立 Null 值項目。

索引 3&num;的缺失 配置項可以在讀&num;取`ArrayExample`索引 3 鍵/值對的任何配置提供程式綁定到實例之前提供。 考慮以下從範例下載*的 Value3.json*檔:

[!code-json[](index/samples/3.x/ConfigSample/Value3.json)]

以下代碼包括*Value3.json*`arrayDict``Dictionary`和 的 設定:

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramArray.cs?name=snippet2)]

以下代碼讀取前面的設定並顯示值:

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Array.cshtml.cs?name=snippet)]

前面的代碼傳回以下輸出:

```text
Index: 0  Value: value0
Index: 1  Value: value1
Index: 2  Value: value2
Index: 3  Value: value3
Index: 4  Value: value4
Index: 5  Value: value5
```

自訂設定提供者不需要實作陣列繫結。

## <a name="custom-configuration-provider"></a>自訂設定提供者

範例應用程式示範如何建立會使用 [Entity Framework (EF)](/ef/core/) 從資料庫讀取設定機碼值組的基本設定提供者。

提供者具有下列特性：

* EF 記憶體內資料庫會用於示範用途。 若要使用需要連接字串的資料庫，請實作第二個 `ConfigurationBuilder` 以從另一個設定提供者提供連接字串。
* 啟動時，該提供者會將資料庫資料表讀入到設定中。 該提供者不會以個別機碼為基礎查詢資料庫。
* 未實作變更時重新載入，因此在應用程式啟動後更新資料庫對應用程式設定沒有影響。

定義 `EFConfigurationValue` 實體來在資料庫中存放設定值。

*Models/EFConfigurationValue.cs*：

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

新增 `EFConfigurationContext` 以存放及存取已設定的值。

*EFConfigurationProvider/EFConfigurationContext.cs*：

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

建立會實作 <xref:Microsoft.Extensions.Configuration.IConfigurationSource> 的類別。

*EFConfigurationProvider/EFConfigurationSource.cs*：

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

透過繼承自 <xref:Microsoft.Extensions.Configuration.ConfigurationProvider> 來建立自訂設定提供者。 若資料庫是空的，設定提供者會初始化資料庫。 由於[配置鍵不區分大小寫](#keys),因此使用不區分大小寫的比較器[(StringComparer.OrdinalIgnoreCase)](xref:System.StringComparer.OrdinalIgnoreCase)創建用於初始化資料庫的字典。

*EFConfigurationProvider/EFConfigurationProvider.cs*：

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

`AddEFConfiguration` 擴充方法允許新增設定來源到 `ConfigurationBuilder`。

*Extensions/EntityFrameworkExtensions.cs*:

[!code-csharp[](index/samples/3.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

下列程式碼顯示如何在 *Program.cs* 中使用自訂 `EFConfigurationProvider`：

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

<a name="acs"></a>

## <a name="access-configuration-in-startup"></a>啟動中的存取設定

以下代碼以`Startup`方法顯示設定資料:

[!code-csharp[](index/samples/3.x/ConfigSample/StartupKey.cs?name=snippet&highlight=13,18)]

如需使用啟動方便方法來存取設定的範例，請參閱[應用程式啟動：方便方法](xref:fundamentals/startup#convenience-methods)。

## <a name="access-configuration-in-razor-pages"></a>剃刀頁中的存取設定

以下代碼在 Razor 頁面中顯示設定資料:

[!code-cshtml[](index/samples/3.x/ConfigSample/Pages/Test5.cshtml)]

## <a name="access-configuration-in-a-mvc-view-file"></a>造訪 MVC 檢視檔中的設定

以下代碼在 MVC 檢視中顯示設定資料:

[!code-cshtml[](index/samples/3.x/ConfigSample/Views/Home2/Index.cshtml)]

<a name="hvac"></a>

## <a name="host-versus-app-configuration"></a>主機與應用程式組態的比較

設定及啟動應用程式之前，會先設定及啟動「主機」**。 主機負責應用程式啟動和存留期管理。 應用程式與主機都是使用此主題中所述的設定提供者來設定的。 主機組態機碼/值組也會包含在應用程式的組態中。 如需有關當建置主機時如何使用設定提供者的詳細資訊，以及設定來源如何影響主機設定的詳細資訊，請參閱 <xref:fundamentals/index#host>。

<a name="dhc"></a>

## <a name="default-host-configuration"></a>預設主機設定

如需使用 [Web 主機](xref:fundamentals/host/web-host)時預設組態的詳細資料，請參閱[本主題的 ASP.NET Core 2.2 版本](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2)。

* 主機組態的提供來源：
  * 使用[環境變數配置提供者](#environment-variables-configuration-provider)(`DOTNET_`例如),`DOTNET_ENVIRONMENT`環境變數已預先固定。 載入設定機碼值組時，會移除前置詞 (`DOTNET_`)。
  * 使用[指令列設定提供者](#command-line-configuration-provider)的指令列參數 。
* 已建立 Web 主機預設組態 (`ConfigureWebHostDefaults`)：
  * Kestrel 會用作為網頁伺服器，並使用應用程式的組態提供者來設定。
  * 新增主機篩選中介軟體。
  * 如果 `ASPNETCORE_FORWARDEDHEADERS_ENABLED` 環境變數設定為 `true`，則會新增轉接的標頭中介軟體。
  * 啟用 IIS 整合。

## <a name="other-configuration"></a>其他設定

這個主題僅涉及*應用設定*。 使用本主題未涵蓋的設定檔設定 ASP.NET 核心應用執行和託管的其他方面:

* *啟動.json*/*啟動設定.json*是開發環境的工具設定檔,描述:
  * 在<xref:fundamentals/environments#development>中。
  * 跨文檔集,其中使用檔配置ASP.NET開發方案的核心應用。
* *web.config*是伺服器設定檔,在以下主題中描述:
  * <xref:host-and-deploy/iis/index>
  * <xref:host-and-deploy/aspnet-core-module>

有關從早期版本的ASP.NET遷移應用配置的詳細資訊,請參閱<xref:migration/proper-to-2x/index#store-configurations>。

## <a name="add-configuration-from-an-external-assembly"></a>從外部組件新增設定

<xref:Microsoft.AspNetCore.Hosting.IHostingStartup> 實作允許在啟動時從應用程式 `Startup` 類別外部的外部組件，針對應用程式新增增強功能。 如需詳細資訊，請參閱 <xref:fundamentals/configuration/platform-specific-configuration>。

## <a name="additional-resources"></a>其他資源

* [設定原始碼](https://github.com/dotnet/extensions/tree/master/src/Configuration)
* <xref:fundamentals/configuration/options>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core 中的應用程式設定是以由*設定提供者*所建立的機碼值組為基礎。 設定提供者會從各種設定來源將設定資料讀取到機碼值組中：

* Azure 金鑰保存庫
* Azure 應用程式組態
* 命令列引數
* 自訂提供者 (已安裝或已建立)
* 目錄檔案
* 環境變數
* 記憶體內部 .NET 物件
* 設定檔

一般組態提供者案例 ([microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) 的組態套件包含在 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)中。

下列程式碼範例與範例應用程式中的程式碼範例使用 <xref:Microsoft.Extensions.Configuration> 命名空間：

```csharp
using Microsoft.Extensions.Configuration;
```

*選項模式*是此主題中所述之設定概念的延伸。 選項使用類別來代表一組相關的設定。 如需詳細資訊，請參閱 <xref:fundamentals/configuration/options>。

[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples)([如何下載](xref:index#how-to-download-a-sample))

## <a name="host-versus-app-configuration"></a>主機與應用程式組態的比較

設定及啟動應用程式之前，會先設定及啟動「主機」**。 主機負責應用程式啟動和存留期管理。 應用程式與主機都是使用此主題中所述的設定提供者來設定的。 主機組態機碼/值組也會包含在應用程式的組態中。 如需有關當建置主機時如何使用設定提供者的詳細資訊，以及設定來源如何影響主機設定的詳細資訊，請參閱 <xref:fundamentals/index#host>。

## <a name="other-configuration"></a>其他設定

這個主題僅涉及*應用設定*。 使用本主題未涵蓋的設定檔設定 ASP.NET 核心應用執行和託管的其他方面:

* *啟動.json*/*啟動設定.json*是開發環境的工具設定檔,描述:
  * 在<xref:fundamentals/environments#development>中。
  * 跨文檔集,其中使用檔配置ASP.NET開發方案的核心應用。
* *web.config*是伺服器設定檔,在以下主題中描述:
  * <xref:host-and-deploy/iis/index>
  * <xref:host-and-deploy/aspnet-core-module>

有關從早期版本的ASP.NET遷移應用配置的詳細資訊,請參閱<xref:migration/proper-to-2x/index#store-configurations>。

## <a name="default-configuration"></a>預設組態

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

## <a name="security"></a>安全性

採用下列做法來保護敏感性組態資料：

* 永遠不要將密碼或其他敏感性資料儲存在設定提供者程式碼或純文字設定檔中。
* 不要在開發或測試環境中使用生產環境祕密。
* 請在專案外部指定祕密，以防止其意外認可至開放原始碼存放庫。

如需詳細資訊，請參閱下列主題：

* <xref:fundamentals/environments>
* <xref:security/app-secrets>&ndash;包括有關使用環境變數存儲敏感數據的建議。 「祕密管理員」使用「檔案設定提供者」以 JSON 檔案在本機系統上存放使用者祕密。 此主題稍後將說明「檔案設定提供者」。

[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) 可安全地儲存 ASP.NET Core 應用程式的應用程式祕密。 如需詳細資訊，請參閱 <xref:security/key-vault-configuration>。

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

您可以在應用程式的[相依性插入 (DI)](xref:fundamentals/dependency-injection) 容器中找到 <xref:Microsoft.Extensions.Configuration.IConfiguration>。 <xref:Microsoft.Extensions.Configuration.IConfiguration>可以注入 Razor<xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel>頁面<xref:Microsoft.AspNetCore.Mvc.Controller>或 MVC 以獲取類的配置。

在以下範例中,該`_config`欄位用於存取設定值:

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

### <a name="keys"></a>索引鍵

設定機碼會採用下列慣例：

* 機碼區分大小寫。 例如，`ConnectionString` 與 `connectionstring` 會被視為相等的機碼。
* 若相同機碼的值是由相同或不同設定提供者設定，在機碼上設定的最後一個值是所使用的值。
* 階層式機碼
  * 在設定 API 內，冒號分隔字元 (`:`) 可在所有平台上運作。
  * 在環境變數中，冒號分隔字元可能無法在所有平台上運作。 所有平台都支援雙底線 (`__`)，且會自動轉換為冒號。
  * 在 Azure Key Vault 中，階層式機碼使用 `--` (兩個破折號) 來做為分隔符號。 編寫代碼,在機密載入到應用的配置中時,用冒號替換破折號。
* <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> 支援在設定機碼中使用陣列索引將陣列繫結到物件。 [將陣列繫結到類別](#bind-an-array-to-a-class)一節說明陣列繫結。

### <a name="values"></a>值

設定值會採用下列慣例：

* 值是字串。
* Null 值無法存放在設定中或繫結到物件。

## <a name="providers"></a>提供者

下表顯示可供 ASP.NET Core 應用程式使用的設定提供者。

| 提供者 | 從&hellip;提供設定 |
| -------- | ----------------------------------- |
| [Azure Key Vault 設定提供者](xref:security/key-vault-configuration) (*安全性*主題) | Azure 金鑰保存庫 |
| [Azure 應用程式組態提供者](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure 文件) | Azure 應用程式組態 |
| [命令列設定提供者](#command-line-configuration-provider) | 命令列參數 |
| [自訂設定提供者](#custom-configuration-provider) | 自訂來源 |
| [環境變數設定提供者](#environment-variables-configuration-provider) | 環境變數 |
| [檔案設定提供者](#file-configuration-provider) | 檔案 (INI、JSON、XML) |
| [每個檔案機碼的設定提供者](#key-per-file-configuration-provider) | 目錄檔案 |
| [記憶體設定提供者](#memory-configuration-provider) | 記憶體內集合 |
| [使用者祕密 (祕密管理員)](xref:security/app-secrets) (*安全性*主題) | 使用者設定檔目錄中的檔案 |

在啟動時，會依照設定來源的設定提供者的指定順序讀入設定來源。 本主題中描述的配置提供程式按字母順序描述,而不是按代碼排列順序描述。 在代碼中訂購配置提供程式,以滿足應用所需的基礎配置源的優先順序。

典型的設定提供者順序是：

1. 檔(*應用程式設定.json,**應用程式設置。環境\.json*`{Environment}`, 應用程式目前的託管環境在哪裡)
1. [Azure 金鑰保存庫](xref:security/key-vault-configuration)
1. [使用者祕密 (祕密管理員)](xref:security/app-secrets) (僅限開發環境)
1. 環境變數
1. 命令列引數

將命令列組態提供者放在提供者序列結尾是常見做法，因為這樣可以讓命令列引數覆寫由其他提供者所設定的組態。

使用 初始化新的主機產生器時,將使用前面的提供程式`CreateDefaultBuilder`序列 。 如需詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。

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

## <a name="configureappconfiguration"></a>ConfigureAppConfiguration

建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定提供者，以及由 `CreateDefaultBuilder` 自動新增的設定提供者：

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

### <a name="override-previous-configuration-with-command-line-arguments"></a>使用命令列引數覆寫先前的組態

若要提供可使用命令列引數覆寫的應用程式組態，請最後呼叫 `AddCommandLine`：

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="remove-providers-added-by-createdefaultbuilder"></a>移除由建立預設產生器新增的提供者

要刪除`CreateDefaultBuilder`新增的提供者,請先在[I 設定產生器](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources)上調用[Clear:](/dotnet/api/system.collections.generic.icollection-1.clear)

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

### <a name="arguments"></a>引數

當值後面接著空格時，值必須接著等號 (`=`)，或機碼必須有前置詞 (`--` 或 `/`)。 如果使用等號 (例如 `CommandLineKey=`)，則不需要此值。

| 索引鍵前置字元               | 範例                                                |
| ------------------------ | ------------------------------------------------------ |
| 沒有前置字元                | `CommandLineKey1=value1`                               |
| 雙虛線 (`--`)        | `--CommandLineKey2=value2`, `--CommandLineKey2 value2` |
| 正斜線 (`/`)      | `/CommandLineKey3=value3`, `/CommandLineKey3 value3`   |

在相同的命令中，請不要混合使用等號搭配使用空格之機碼值組的命令列引數。

範例命令：

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a>切換對應

參數對應允許索引鍵名稱取代邏輯。 使用 手動構建配置<xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>時<xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>,提供方法的交換機替換字典。

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

| Key       | 值             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

若啟動應用程式時使用切換對應機碼，設定會接收由字典提供之機碼上的設定值：

```dotnetcli
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

[!INCLUDE[](~/includes/environmentVarableColon.md)]

[Azure 應用服務](https://azure.microsoft.com/services/app-service/)允許在 Azure 門戶中設置環境變數,這些變數可以使用環境變數配置提供程式覆蓋應用配置。 如需詳細資訊，請參閱 [Azure App：使用 Azure 入口網站覆寫應用程式設定](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal)。

使用 [Web 主機](xref:fundamentals/host/web-host)初始化新的主機建立器並呼叫 `CreateDefaultBuilder` 時，可使用 `AddEnvironmentVariables` 為[主機組態](#host-versus-app-configuration)載入字首為 `ASPNETCORE_` 的環境變數。 如需詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。

`CreateDefaultBuilder` 也會載入：

* 來自無首碼之環境變數的應用程式組態 (在未提供首碼的情況下呼叫 `AddEnvironmentVariables`)。
* 從 *appsettings.json* 與 *appsettings.{Environment}.json* 檔案載入其他選擇性組態。
* 開發環境中的[使用者祕密 (祕密管理員)](xref:security/app-secrets)。
* 命令列引數。

從使用者祕密與 *appsettings* 檔案建立設定之後，會呼叫「環境變數設定提供者」。 在此位置呼叫提供者可讓系統在執行階段讀取環境變數，以覆寫由使用者祕密與 *appsettings* 檔案所設定的設定。

要從其他環境變數提供應用配置,請調用應用的其他提供程式`ConfigureAppConfiguration`,然後調用`AddEnvironmentVariables`首碼:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddEnvironmentVariables(prefix: "PREFIX_");
})
```

最後`AddEnvironmentVariables`調用以允許具有給定首碼的環境變數覆蓋來自其他提供程式的值。

**範例**

範例應用程式利用靜態方便方法 `CreateDefaultBuilder` 的優勢來建置主機，這包括對 `AddEnvironmentVariables` 的呼叫。

1. 執行範例應用程式。 開啟瀏覽器以瀏覽位於 `http://localhost:5000` 的應用程式。
1. 觀察輸出是否包含環境變數 `ENVIRONMENT` 的機碼值組。 值反映應用程式執行所在環境，在本機執行時，通常是 `Development`。

為縮短由應用程式轉譯的環境變數清單，應用程式會篩選環境變數。 請參閱範例應用程式的 *Pages/Index.cshtml.cs* 檔案。

要公開應用程式可用的所有環境變數,將`FilteredConfiguration`*頁/Index.cshtml.cs*中的內容更改為以下內容:

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a>首碼

向應用配置提供首碼`AddEnvironmentVariables`時,將篩選載入到應用配置中的環境變數。 例如，若要篩選前置詞為 `CUSTOM_` 的環境變數，請提供前置詞給設定提供者：

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

建立設定機碼值組時，會移除前置詞。

建立主機建立器時，主機組態由環境變數提供。 如需這些環境變數所用前置詞的詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。

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
| `CUSTOMCONNSTR_{KEY} `   | `ConnectionStrings:{KEY}`   | 設定項目未建立。                                                |
| `MYSQLCONNSTR_{KEY}`     | `ConnectionStrings:{KEY}`   | 機碼：`ConnectionStrings:{KEY}_ProviderName`：<br>值: `MySql.Data.MySqlClient` |
| `SQLAZURECONNSTR_{KEY}`  | `ConnectionStrings:{KEY}`   | 機碼：`ConnectionStrings:{KEY}_ProviderName`：<br>值: `System.Data.SqlClient`  |
| `SQLCONNSTR_{KEY}`       | `ConnectionStrings:{KEY}`   | 機碼：`ConnectionStrings:{KEY}_ProviderName`：<br>值: `System.Data.SqlClient`  |

**範例**

在伺服器上建立自訂連接字串環境變數:

* 名稱&ndash;`CUSTOMCONNSTR_ReleaseDB`
* 值&ndash;`Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`

如果`IConfiguration`注入並分配給名為的`_config`欄位,請閱讀該值:

```csharp
_config["ConnectionStrings:ReleaseDB"]
```

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

`AddJsonFile`使用 初始化新的主機產生器時,將自動呼叫兩`CreateDefaultBuilder`次 。 會呼叫此方法以從下列位置載入設定：

* *appsettings.json* &ndash; 會先讀取此檔案。 檔案的環境版本可以覆寫由 *appsettings.json* 檔案提供的值。
* *應用設置。{環境}.json*&ndash;檔的環境版本根據[IHosting 環境.環境名稱](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*)載入。

如需詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。

`CreateDefaultBuilder` 也會載入：

* 環境變數。
* 開發環境中的[使用者祕密 (祕密管理員)](xref:security/app-secrets)。
* 命令列引數。

會先建立 JSON 設定提供者。 因此，使用者祕密、環境變數與命令列引數會覆寫由 *appsettings* 檔案設定的設定。

在建置主機時，呼叫 `ConfigureAppConfiguration` 以指定檔案 (除了 *appsettings.json* 和 * appsettings.{Environment}.json* 以外) 的應用程式設定：

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

**範例**

範例應用利用靜態便利方法`CreateDefaultBuilder`構建主機,其中包括對`AddJsonFile`的 兩個調用:

* 第一個呼叫`AddJsonFile`從*應用程式設定載入設定. json*:

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.json)]

* 從`AddJsonFile`應用程式設定載入設定的第二個呼叫 *。環境\.json*. 對於*應用設置。在範例應用程式中的開發.json*中,將載入以下檔:

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.Development.json)]

1. 執行範例應用程式。 開啟瀏覽器以瀏覽位於 `http://localhost:5000` 的應用程式。
1. 輸出包含基於應用環境的配置的鍵值對。 金鑰`Logging:LogLevel:Default`的日誌級別是在`Debug`開發環境中運行應用時。
1. 在「生產」環境中再次運行範例應用:
   1. 打開*屬性/啟動設置.json*檔。
   1. 在設定檔`ConfigurationSample`中,將`ASPNETCORE_ENVIRONMENT`環境變數的值變更為`Production`。
   1. 保存檔案並在命令 shell`dotnet run`中運行應用。
1. *應用設置中的設置。開發.json*不再覆*寫 應用程式設定中的設定*。 鍵`Logging:LogLevel:Default`的紀錄等級為`Warning`。

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

[`ConfigurationBinder.GetValue<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*)使用指定金鑰從配置中提取單個值,並將其轉換為指定的非集合類型。 重載接受預設值。

下列範例將：

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

### <a name="exists"></a>Exists

使用 [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) 來判斷設定區段是否存在：

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

以範例資料為例， `sectionExists` 是 `false`，因未設定資料中沒有 `section2:subsection2` 區段。

## <a name="bind-to-an-object-graph"></a>繫結至物件圖形

<xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> 可以繫結整個 POCO 物件圖形。 與綁定簡單物件一樣,僅綁定公共讀/寫屬性。

範例包含 `TvShow` 模型，其物件圖形包括 `Metadata` 與 `Actors` 類別 (*Models/TvShow.cs*)：

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

範例應用程式有 *tvshow.xml* 檔案，其中包含設定資料：

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

設定會被繫結到整個 `TvShow` 物件 (使用 `Bind` 方法)。 已繫結的執行個體會被指派給屬性以用於轉譯：

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*)綁定並返回指定的類型。 `Get<T>` 比使用 `Bind` 更方便。 以下代碼展示如何與前面的範例一`Get<T>`起使用:

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

## <a name="bind-an-array-to-a-class"></a>將陣列繫結到類別

範例應用程式示範此節中解釋的概念。**

<xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> 支援在設定機碼中使用陣列索引將陣列繫結到物件。 公開數位`:0:`鍵段`:1:`&hellip;`:{n}:`(、 、 ) 的任何陣列格式都能夠綁定到 POCO 類陣列。

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

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

陣列會跳過索引 &num;3 的值。 設定繫結程式無法繫結 Null 值或在已繫結的物件中建立 Null 項目，這在示範繫結此陣列的結果到某物件的時候已經很清楚。

在範例應用程式中，POCO 類別可用來存放繫結設定資料：

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

設定資料已繫結到物件：

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*)也可以使用語法,這會產生更緊湊的代碼:

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

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

在 `ConfigureAppConfiguration` 中：

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

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

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

「JSON 設定提供者」會將設定資料讀入到下列機碼值組：

| Key                     | 值  |
| ----------------------- | :----: |
| json_array:key          | valueA |
| json_array:subsection:0 | valueB |
| json_array:subsection:1 | valueC |
| json_array:subsection:2 | valueD |

在範例應用程式中，下列 POCO 類別可用來繫結設定機碼值組：

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

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

<xref:Microsoft.AspNetCore.Hosting.IHostingStartup> 實作允許在啟動時從應用程式 `Startup` 類別外部的外部組件，針對應用程式新增增強功能。 如需詳細資訊，請參閱 <xref:fundamentals/configuration/platform-specific-configuration>。

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/configuration/options>

::: moniker-end
