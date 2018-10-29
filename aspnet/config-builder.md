---
uid: config-builder
title: 適用於 ASP.NET 的組態產生器
author: rick-anderson
description: 了解如何從外部來源中的 web.config 值以外的來源取得設定資料。
ms.author: riande
ms.date: 10/29/2018
ms.technology: aspnet
msc.type: content
ms.openlocfilehash: 4dcc62573fad13ec8b37b2c59e884eec7ca80b92
ms.sourcegitcommit: 54655f1e1abf0b64d19506334d94cfdb0caf55f6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/26/2018
ms.locfileid: "50148833"
---
# <a name="configuration-builders-for-aspnet"></a>適用於 ASP.NET 的組態產生器

藉由[Stephen Molloy](https://github.com/StephenMolloy)和[Rick Anderson](https://twitter.com/RickAndMSFT)

組態產生器提供現代化和敏捷式軟體開發的機制，以從外部來源取得設定值的 ASP.NET 應用程式。

組態產生器：

* 會提供於.NET Framework 4.7.1 和更新版本。
* 提供彈性的機制，用於讀取組態值。
* 解決一些應用程式的基本需求它們將移至容器及雲端具有焦點的環境。
* 可以用來提升保護組態資料所繪製，從先前無法使用的來源 （例如，Azure 金鑰保存庫和環境變數） 的.NET 組態系統中。

## <a name="keyvalue-configuration-builders"></a>索引鍵/值設定產生器

組態產生器可以處理的常見案例是提供基本的索引鍵/值的取代機制，遵循下列索引鍵/值模式的組態區段。 .NET Framework 的概念 ConfigurationBuilders 不限於特定的組態區段或模式。 不過，許多中的組態產生器`Microsoft.Configuration.ConfigurationBuilders`([github](https://github.com/aspnet/MicrosoftConfigurationBuilders)， [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders)) 工作內的索引鍵/值模式。

## <a name="keyvalue-configuration-builders-settings"></a>索引鍵/值設定產生器

下列設定會套用到所有的索引鍵/值設定產生器中`Microsoft.Configuration.ConfigurationBuilders`。

### <a name="mode"></a>模式

組態產生器會使用外部索引鍵/值資訊的來源來填入選取的索引鍵/值項目，組態系統。 具體而言，`<appSettings/>`和`<connectionStrings/>`各節會接收組態產生器中的特殊處理。 在三個模式下運作的產生器：

* `Strict` -預設的模式。 在此模式中，設定產生器只能在運作上的已知索引鍵/值為主的組態區段。 `Strict` 模式會列舉一節中的每個索引鍵。 如果外部來源中找到相符的索引鍵：

   * 組態產生器會將產生的組態區段中的值取代從外部來源的值。
* `Greedy` -這種模式與密切相關`Strict`模式。 而不是則限制為原始設定中存在的索引鍵：

  * 組態產生器會將所有的索引鍵/值組從外部來源新增到產生的組態區段。

* `Expand` -上運作的原始 XML 之前將會剖析為組態區段物件。 它可以視為語彙基元字串中的擴充功能。 未經處理的 XML 字串符合模式的任何部分`${token}`是語彙基元展開的候選項目。 如果外部來源中發現沒有對應的值，則權杖會不會變更。 在此模式中的產生器並不限於`<appSettings/>`和`<connectionStrings/>`區段。

從下列標記*web.config*可讓[EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/)在`Strict`模式︰

[!code-xml[Main](config-builder/MyConfigBuilders/WebDefault.config?name=snippet)]

下列程式碼的讀取`<appSettings/>`並`<connectionStrings/>`先前所示*web.config*檔案：

[!code-csharp[Main](config-builder/MyConfigBuilders/About.aspx.cs)]

上述程式碼會將屬性值：

* 中的值*web.config*檔案如果金鑰未設定環境變數中。
* 值的環境變數，如果設定。

比方說，`ServiceID`將包含：

* 「 從 web.config ServiceID 值，「 如果環境變數`ServiceID`未設定。
* 值`ServiceID`環境變數，如果設定。

下圖顯示`<appSettings/>`索引鍵/值來自上述*web.config*環境編輯器中的檔案集：

![環境編輯器](config-builder/static/env.png)

注意： 您可能需要結束並重新啟動 Visual Studio 以查看變更環境變數中。

### <a name="prefix-handling"></a>前置處理

索引鍵的前置詞可以簡化設定索引鍵，因為：

* .NET Framework 組態是複雜和巢狀。
* 基本和一般本質上，通常是外部索引鍵/值來源。 例如，未巢狀環境變數。

使用任何下列其中一個方法來插入兩`<appSettings/>`和`<connectionStrings/>`透過環境變數組態：

* 具有`EnvironmentConfigBuilder`在預設`Strict`模式和適當的索引鍵名稱在組態檔中。 上述程式碼和標記會使用此方法。 使用這種方法，您可以**未**有相同的已命名兩者中的索引鍵`<appSettings/>`和`<connectionStrings/>`。
* 使用兩個`EnvironmentConfigBuilder`中的 s`Greedy`具有不同的前置詞模式和`stripPrefix`。 使用此方法時，應用程式可以讀取`<appSettings/>`和`<connectionStrings/>`而不需要更新組態檔。 下一步 區段中， [stripPrefix](#stripprefix)，示範如何執行這項操作。
* 使用兩個`EnvironmentConfigBuilder`中的 s`Greedy`具有不同的前置詞的模式。 使用這種方法，因為索引鍵的名稱必須不同前置詞，不能有重複的索引鍵名稱。  例如: 

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefix.config?name=snippet&highlight=11-99)]

上述標記中，使用相同的一般的索引鍵/值來源可用來填入兩個不同區段的組態。

下圖顯示`<appSettings/>`並`<connectionStrings/>`索引鍵/值來自上述*web.config*環境編輯器中的檔案集：

![環境編輯器](config-builder/static/prefix.png)

下列程式碼的讀取`<appSettings/>`並`<connectionStrings/>`包含在先前的索引鍵/值*web.config*檔案：

[!code-csharp[Main](config-builder/MyConfigBuilders/Contact.aspx.cs?name=snippet)]

上述程式碼會將屬性值：

* 中的值*web.config*檔案如果金鑰未設定環境變數中。
* 值的環境變數，如果設定。

例如，使用 上一個*web.config*檔案，會設定索引鍵/值在前一個環境編輯器映像，和先前的程式碼中，下列值：

|  Key              | 值 |
| ----------------- | ------------ |
|     AppSetting_ServiceID           | 從環境變數的 AppSetting_ServiceID|
|    AppSetting_default            | Env AppSetting_default 值 |
|       ConnStr_default         | 從 env ConnStr_default val|

### <a name="stripprefix"></a>stripPrefix

`stripPrefix`： 布林值，預設為`false`。 

上述的 XML 標記分隔的連接字串中的應用程式設定，但要求中的所有索引鍵*web.config*檔案，以使用指定的前置詞。 例如，前置詞`AppSetting`必須新增至`ServiceID`金鑰 (「 AppSetting_ServiceID")。 具有`stripPrefix`，在不使用前置詞*web.config*檔案。 前置詞都需要組態產生器來源 （例如，在環境中。）我們預期大部分的開發人員會使用`stripPrefix`。

應用程式通常移除前置詞。 下列*web.config*去除前置詞：

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefixStrip.config?name=snippet&highlight=14,19)]

在上述*web.config*檔案，`default`索引鍵存在於同時`<appSettings/>`和`<connectionStrings/>`。

下圖顯示`<appSettings/>`並`<connectionStrings/>`索引鍵/值來自上述*web.config*環境編輯器中的檔案集：

![環境編輯器](config-builder/static/prefix.png)

下列程式碼的讀取`<appSettings/>`並`<connectionStrings/>`包含在先前的索引鍵/值*web.config*檔案：

[!code-csharp[Main](config-builder/MyConfigBuilders/About2.aspx.cs?name=snippet)]

上述程式碼會將屬性值：

* 中的值*web.config*檔案如果金鑰未設定環境變數中。
* 值的環境變數，如果設定。

例如，使用 上一個*web.config*檔案，會設定索引鍵/值在前一個環境編輯器映像，和先前的程式碼中，下列值：

|  Key              | 值 |
| ----------------- | ------------ |
|     ServiceID           | 從環境變數的 AppSetting_ServiceID|
|    default            | Env AppSetting_default 值 |
|    default         | 從 env ConnStr_default val|

### <a name="tokenpattern"></a>tokenPattern

`tokenPattern`： 字串，預設值為 `@"\$\{(\w+)\}"`

`Expand`行為的產生器搜尋的原始 XML 的權杖看起來像`${token}`。 搜尋是使用預設的規則運算式`@"\$\{(\w+)\}"`。 符合的字元組`\w`非 XML 與許多組態來源允許更為嚴格。 使用`tokenPattern`當多個字元比`@"\$\{(\w+)\}"`所需的語彙基元的名稱。

`tokenPattern`： 字串：

* 可讓開發人員若要變更用於語彙基元的比對的 regex。
* 若要確定它是語式正確，非危險的 regex 會不進行任何驗證。
* 它必須包含擷取群組。 整個 regex 必須符合將整個語彙基元。 第一個擷取必須是要查閱組態來源中的語彙基元名稱。

## <a name="configuration-builders-in-microsoftconfigurationconfigurationbuilders"></a>在 Microsoft.Configuration.ConfigurationBuilders 組態產生器

### <a name="environmentconfigbuilder"></a>EnvironmentConfigBuilder

```xml
<add name="Environment"
    [mode|prefix|stripPrefix|tokenPattern] 
    type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Environment" />
```

[EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):

* 是最簡單的組態產生器。
* 從環境中讀取的值。
* 沒有任何其他組態選項。
* `name`屬性值為任意值。

**注意：** 在 Windows 容器環境中，在執行階段設定的變數只會插入至進入點程序的環境。 應用程式，以服務方式執行，或非進入點程序無法挑選這些變數除非否則插入透過容器中的機制。 針對[IIS](https://github.com/Microsoft/iis-docker/pull/41)/[ASP.NET](https://github.com/Microsoft/aspnet-docker)-基礎容器的目前版本[ServiceMonitor.exe](https://github.com/Microsoft/iis-docker/pull/41)會處理在*DefaultAppPool*只。 開發自己的資料隱碼攻擊機制，不進入點程序，可能需要其他的 Windows 型容器變體。

### <a name="usersecretsconfigbuilder"></a>UserSecretsConfigBuilder

> [!WARNING]
> 永遠不會儲存密碼、 機密的連接字串或其他來源的程式碼中的機密資料。 不應使用生產環境祕密進行開發或測試。

```xml
<add name="UserSecrets"
    [mode|prefix|stripPrefix|tokenPattern]
    (userSecretsId="{secret string, typically a GUID}" | userSecretsFile="~\secrets.file")
    [optional="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.UserSecretsConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.UserSecrets" />
```

此組態產生器會提供類似的功能[ASP.NET Core Secret Manager](/aspnet/core/security/app-secrets)。

[UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/)可用在.NET Framework 專案中，但必須指定密碼檔案。 或者，您可以定義`UserSecretsId`專案中的屬性檔案並建立正確的位置進行讀取的未經處理的密碼檔案。 若要保留外部的相依性，從您的專案，祕密的檔案是 XML 格式。 XML 格式化是實作詳細資料，格式應該不依賴。 如果您要共用*secrets.json*檔案中使用.NET Core 專案，請考慮使用[SimpleJsonConfigBuilder](#simplejsonconfig)。 `SimpleJsonConfigBuilder`格式化的.NET Core 也應該考慮的實作詳細資料，有可能變更。

組態屬性`UserSecretsConfigBuilder`:

* `userSecretsId` -這是用來識別 XML 祕密檔案的慣用的方法。 它的運作方式類似於.NET Core，使用`UserSecretsId`專案來儲存這個識別項的屬性。 必須是唯一的字串，它不需要是 GUID。 利用此屬性，`UserSecretsConfigBuilder`查看已知的本機位置 (`%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml`) 屬於此識別碼的密碼檔案。
* `userSecretsFile` -此選擇性屬性，指定包含機密資料的檔案。 `~`字元會在開始時用來參考應用程式根目錄。 任一此屬性或`userSecretsId`是必要的屬性。 如果同時指定這兩者，`userSecretsFile`會優先使用。
* `optional`： 布林值，預設值`true`-如果找不到祕密檔案，可防止發生例外狀況。 
* `name`屬性值為任意值。

祕密檔案具有下列格式：

```xml
<?xml version="1.0" encoding="utf-8" ?>
<root>
  <secrets ver="1.0">
    <secret name="secret key name" value="secret value" />
  </secrets>
</root>
```

### <a name="azurekeyvaultconfigbuilder"></a>AzureKeyVaultConfigBuilder

```xml
<add name="AzureKeyVault"
    [mode|prefix|stripPrefix|tokenPattern]
    (vaultName="MyVaultName" |
     uri="https:/MyVaultName.vault.azure.net")
    [connectionString="connection string"]
    [version="secrets version"]
    [preloadSecretNames="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.AzureKeyVaultConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Azure" />
```

[AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/)讀取中儲存值[Azure Key Vault](/azure/key-vault/key-vault-whatis)。

`vaultName` 是所需 （保存庫的名稱），或是向保存庫 URI。 其他屬性允許連接至哪一個保存庫相關的控制項，但未搭配環境中執行應用程式時才需要`Microsoft.Azure.Services.AppAuthentication`。 Azure 服務驗證程式庫用來自動盡可能選取從執行環境的連接資訊。 您可以會覆寫自動挑選的連接資訊所提供的連接字串。

* `vaultName` -需要`uri`中未提供。 您要讀取索引鍵/值組的 Azure 訂用帳戶中指定的保存庫名稱。
* `connectionString` -連接字串供[azureservicetokenprovider 會](https://docs.microsoft.com/en-us/azure/key-vault/service-to-service-authentication#connection-string-support)
* `uri` -連接到具有指定之其他 Key Vault 提供者`uri`值。 如果未指定，Azure (`vaultName`) 是保存庫提供者。
* `version` -Azure Key Vault 祕密提供版本控制功能。 如果`version`指定時，產生器只會擷取相符的這個版本的祕密。
* `preloadSecretNames` -根據預設，此產生器從其中**所有**金鑰在金鑰保存庫的名稱，初始化時。 若要避免讀取所有的索引鍵值，請將此屬性設定為`false`。 這個設定設為`false`一次讀取一個密碼。 讀取一次一個非常實用的如果保存庫可讓 「 取得 」 存取權，但不是 「 清單 」 存取的密碼。 **注意︰** 使用時`Greedy`模式中，`preloadSecretNames`必須是`true`（預設值。）

### <a name="keyperfileconfigbuilder"></a>KeyPerFileConfigBuilder

```xml
<add name="KeyPerFile"
    [mode|prefix|stripPrefix|tokenPattern]
    (directoryPath="PathToSourceDirectory")
    [ignorePrefix="ignore."]
    [keyDelimiter=":"]
    [optional="false"]
    type="Microsoft.Configuration.ConfigurationBuilders.KeyPerFileConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.KeyPerFile" />
```

[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/)是基本組態產生器會使用目錄的檔案，做為來源的值。 檔案的名稱為索引鍵中，而且內容值。 協調的容器環境中執行時，此組態產生器可以是很有用。 像是 Docker Swarm 和 Kubernetes 所提供的系統`secrets`至其協調的 windows 容器，如此每個檔案的索引鍵。

屬性詳細資料：

* `directoryPath` - 必要項。 指定的路徑尋找的值。 密碼會儲存在 Windows 的 docker *C:\ProgramData\Docker\secrets*預設目錄。
* `ignorePrefix` 會排除在檔案開頭為此前置詞。 預設值為"ignore"。
* `keyDelimiter` 預設值是`null`。 如果指定，組態產生器會周遊多個層級的目錄中，建立使用此分隔符號的索引鍵名稱。 如果此值為`null`，組態產生器只會尋找最上層的目錄。
* `optional` 預設值是`false`。 指定是否來源目錄不存在，組態產生器是否應該導致錯誤。

### <a name="simplejsonconfigbuilder"></a>SimpleJsonConfigBuilder

> [!WARNING]
> 永遠不會儲存密碼、 機密的連接字串或其他來源的程式碼中的機密資料。 不應使用生產環境祕密進行開發或測試。

```xml
<add name="SimpleJson"
    [mode|prefix|stripPrefix|tokenPattern]
    jsonFile="~\config.json"
    [optional="true"]
    [jsonMode="(Flat|Sectional)"]
    type="Microsoft.Configuration.ConfigurationBuilders.SimpleJsonConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Json" />
```

.NET core 專案通常會使用組態的 JSON 檔案。 [SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/)產生器可讓.NET Framework 中使用的.NET Core 的 JSON 檔案。 此組態產生器會提供從一般的索引鍵/值來源的.NET Framework 組態的特定索引鍵/值區域到基本的對應。 此組態產生器 」**不**提供階層式組態。 類似於字典，而不是複雜的階層式物件的 JSON 支援檔案。 可以使用多層級的階層式檔案。 此提供者`flatten`s 附加在每個層級使用的屬性名稱的深度`:`做為分隔符號。

屬性詳細資料：

* `jsonFile` - 必要項。 指定要讀取從 JSON 檔案。 `~`字元會在開始時用來參考應用程式根目錄。
* `optional` -布林值，預設值是`true`。 可避免擲回例外狀況，如果找不到 JSON 檔案。
* `jsonMode` - `[Flat|Sectional]`. `Flat` 是預設值。 當`jsonMode`是`Flat`，JSON 檔案是單一的一般索引鍵/值來源。 `EnvironmentConfigBuilder`和`AzureKeyVaultConfigBuilder`也是單一的一般索引鍵/值來源。 當`SimpleJsonConfigBuilder`中設定`Sectional`模式︰

  * JSON 檔案會在概念上分成只在最高層級多個字典。
  * 每個字典只會套用至附加到它們的最上層的屬性名稱相符的組態區段。 例如: 

```json
    {
        "appSettings" : {
            "setting1" : "value1",
            "setting2" : "value2",
            "complex" : {
                "setting1" : "complex:value1",
                "setting2" : "complex:value2",
            }
        }
    }
```

## <a name="implementing-a-custom-keyvalue-configuration-builder"></a>實作自訂的索引鍵/值的組態產生器

如果組態產生器不符合您的需求，您可以撰寫的自訂承載。 `KeyValueConfigBuilder`替代模式和大部分的前置詞考量，會處理基底類別。 只需要實作的專案：

* 繼承自基底類別，並實作基本的來源索引鍵/值組，透過`GetValue`和`GetAllValues`:
* 新增[Microsoft.Configuration.ConfigurationBuilders.Base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/)至專案。

[!code-csharp[Main](config-builder/MyConfigBuilders/MyCustomConfigBuilder.cs)]

`KeyValueConfigBuilder`基底類別提供許多的工作且一致的行為索引鍵/值跨組態產生器。

## <a name="additional-resources"></a>其他資源

* [設定產生器的 GitHub 存放庫](https://github.com/aspnet/MicrosoftConfigurationBuilders)
* [若要使用.NET 的 Azure 金鑰保存庫的服務對服務驗證](/azure/key-vault/service-to-service-authentication#connection-string-support)
