---
title: ASP.NET核心中的 Azure 金鑰保管庫配置提供者
author: rick-anderson
description: 瞭解如何使用 Azure 密鑰保管庫配置提供程式使用運行時載入的名稱值對配置應用。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2020
uid: security/key-vault-configuration
ms.openlocfilehash: d78584eaa3fcd50425698e4db581060b6ca845f8
ms.sourcegitcommit: fbdb8b9ab5a52656384b117ff6e7c92ae070813c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/13/2020
ms.locfileid: "81228162"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a>ASP.NET核心中的 Azure 金鑰保管庫配置提供者

由[安德魯斯坦頓護士](https://github.com/anurse)

::: moniker range=">= aspnetcore-3.0"

本文件介紹如何使用[Microsoft Azure 密鑰保管庫](https://azure.microsoft.com/services/key-vault/)配置提供程式從 Azure 密鑰保管庫機密載入應用配置值。 Azure 密鑰保管庫是一種基於雲的服務,可幫助保護應用和服務使用的加密密鑰和機密。 將 Azure 金鑰保管庫用於 ASP.NET核心應用的常見方案包括:

* 控制對敏感配置數據的訪問。
* 在儲存配置資料時滿足 FIPS 140-2 級驗證的硬體安全模組 (HSM) 的要求。

[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/samples)([如何下載](xref:index#how-to-download-a-sample))

## <a name="packages"></a>Packages

向[Microsoft.擴展.配置.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/)包添加包引用。

## <a name="sample-app"></a>範例應用程式

範例應用以`#define`*Program.cs*檔案頂部的語句確定的兩種模式之一運行:

* `Certificate`&ndash;演示使用 Azure 金鑰保管庫客戶端 ID 和 X.509 憑證存取儲存在 Azure 密鑰保管庫中的秘密。 此版本的示例可以從任何位置運行,部署到 Azure 應用服務或任何能夠為 ASP.NET 酷應用提供服務的主機。
* `Managed`&ndash;演示如何[使用 Azure 資源的託管識別](/azure/active-directory/managed-identities-azure-resources/overview)使用 Azure AD 身份驗證將應用驗證為 Azure 密鑰保管庫,而無需在應用的代碼或配置中儲存認證。 使用託管標識進行身份驗證時,不需要 Azure AD 應用程式 ID 和密碼(用戶端密鑰)。 必須`Managed`將示例的版本部署到 Azure。 按照"[為 Azure 資源使用託管標識](#use-managed-identities-for-azure-resources)「部分中的指南進行操作。

有關如何使用預處理器指令 ()`#define`設定範例應用程式的詳細資訊,請<xref:index#preprocessor-directives-in-sample-code>參閱 。

## <a name="secret-storage-in-the-development-environment"></a>開發環境中的秘密儲存

使用[機密管理器工具](xref:security/app-secrets)在本地設置機密。 當示例應用在開發環境中的本地電腦上運行時,將從本地機密管理器儲存載入機密。

「機密管理員」工具需要`<UserSecretsId>`應用的專案檔中的屬性。 將屬性值`{GUID}`( ) 設定為任何唯一的 GUID:

```xml
<PropertyGroup>
  <UserSecretsId>{GUID}</UserSecretsId>
</PropertyGroup>
```

機密創建為名稱值對。 層次結構值(配置部分)在`:`[ASP.NET核心配置](xref:fundamentals/configuration/index)密鑰名稱中使用(冒號)作為分隔符。

機密管理員從開啟到項目[內容根](xref:fundamentals/index#content-root)的命令外殼使用`{SECRET NAME}`,其中的`{SECRET VALUE}`名稱和 值是:

```dotnetcli
dotnet user-secrets set "{SECRET NAME}" "{SECRET VALUE}"
```

從項目[的內容根](xref:fundamentals/index#content-root)在命令 shell 中執行以下指令,以設定範例的祕密:

```dotnetcli
dotnet user-secrets set "SecretName" "secret_value_1_dev"
dotnet user-secrets set "Section:SecretName" "secret_value_2_dev"
```

當這些機密儲存在[Azure 金鑰保管庫部分的生產環境中的「機密記憶體」中的](#secret-storage-in-the-production-environment-with-azure-key-vault)Azure`_dev`金鑰保管 庫中`_prod`時,後綴將更改為 。 後綴在應用的輸出中提供指示配置值來源的可視提示。

## <a name="secret-storage-in-the-production-environment-with-azure-key-vault"></a>使用 Azure 金鑰保存式庫的生產環境

此處匯總了[「快速入門:使用 Azure CLI」主題從 Azure 密鑰保管庫設置和檢索機密](/azure/key-vault/quick-create-cli)的說明,用於創建 Azure 密鑰保管庫並儲存範例應用使用的秘密。 有關詳細資訊,請參閱主題。

1. 使用[Azure 門戶](https://portal.azure.com/)中的以下任一方法打開 Azure 雲外殼:

   * 選取程式碼區塊右上角的 [試試看]  。 在文字框中使用搜索字串「Azure CLI」。。
   * 使用**啟動雲外殼**按鈕在瀏覽器中打開雲外殼。
   * 選取 Azure 入口網站右上角功能表上的 **[Cloud Shell]** 按鈕。

   有關詳細資訊,請參閱[Azure CLI](/cli/azure/)和[Azure 雲外殼概述](/azure/cloud-shell/overview)。

1. 如果您尚未通過身份驗證,請使用命令`az login`登錄。

1. 使用以下指令建立資源群組,其中`{RESOURCE GROUP NAME}`為新資源群組的資源組名`{LOCATION}`稱為 Azure 區域(資料中心):

   ```azurecli
   az group create --name "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. 使用以下指令在資源群組中建立金鑰保管庫,其中`{KEY VAULT NAME}`為新密鑰保管庫的名稱`{LOCATION}`, 是 Azure 區域(資料中心):

   ```azurecli
   az keyvault create --name {KEY VAULT NAME} --resource-group "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. 在密鑰保管庫中創建機密作為名稱值對。

   Azure 密鑰保管庫金鑰庫密鑰名稱僅限於字母數位字元和破折號。 階層值(配置部分)使用`--`(兩個破折號)作為分隔符。 在密鑰保管庫密鑰名稱中,通常不允許從[ASP.NET Core 配置](xref:fundamentals/configuration/index)中從子鍵中分隔節的冒號。 因此,當機密載入應用的配置中時,將使用兩個破折號並將其交換為冒號。

   以下機密用於示例應用。 這些值包括後`_prod`綴,用於將它們與在開發環境`_dev`中 載入的後綴值與使用者機密區分開來。 取代為`{KEY VAULT NAME}`在上一步驟中建立的金鑰保管庫的名稱:

   ```azurecli
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "SecretName" --value "secret_value_1_prod"
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "Section--SecretName" --value "secret_value_2_prod"
   ```

## <a name="use-application-id-and-x509-certificate-for-non-azure-hosted-apps"></a>對非 Azure 託管應用使用應用程式 ID 和 X.509 憑證

將 Azure AD、Azure 密鑰保管庫和應用配置為使用 Azure 活動目錄應用程式 ID 和 X.509 證書在**應用託管在 Azure 之外時**對金鑰保管庫進行身份驗證。 如需詳細資訊，請參閱[關於金鑰、祕密和憑證](/azure/key-vault/about-keys-secrets-and-certificates)。

> [!NOTE]
> 儘管 Azure 中託管的應用支援使用應用程式 ID 和 X.509 憑證,但我們建議在 Azure 中託管應用時[對 Azure 資源使用託管標識](#use-managed-identities-for-azure-resources)。 託管標識不需要在應用或開發環境中存儲證書。

當*Program.cs*檔案頂部的語句`Certificate`設定為`#define`時, 範例應用程式使用應用程式 ID 和 X.509 憑證。

1. 創建 PKCS_12 存檔 (*.pfx*) 證書。 建立憑證的選項包括[Windows 上的 MakeCert](/windows/desktop/seccrypto/makecert)與[OpenSSL](https://www.openssl.org/)。
1. 將憑證安裝到當前使用者的個人憑證儲存中。 將密鑰標記為可匯出是可選的。 請注意證書的指紋,此過程稍後將使用。
1. 將 PKCS_12 存檔 *(.pfx*) 證書匯出為 DER 編碼證書 *(.cer*)。
1. 使用 Azure AD(**應用註冊)註冊**應用。
1. 將 DER 編碼的憑證 *(.cer*) 上傳到 Azure AD:
   1. 在 Azure AD 中選擇應用。
   1. 導覽到**憑證&機密**。
   1. 選擇 **「上載證書**」以上載包含公開金鑰的憑證。 *.cer* *、.pem*或 *.crt*證書是可以接受的。
1. 將金鑰保管庫名稱、應用程式 ID 和證書指紋存儲在應用*的應用設置.json*檔中。
1. 瀏覽到 Azure 門戶中的**金鑰保管庫**。
1. [使用 Azure 金鑰保管庫部分在「生產環境」 中的秘密儲存中選擇在「機密存儲」 中](#secret-storage-in-the-production-environment-with-azure-key-vault)創建的金鑰保管庫。
1. 選擇**存取原則**。
1. 選擇 **「添加存取策略**」。
1. 打開 **「機密」許可權**,並提供具有**獲取**和**列表**許可權的應用。
1. 選擇 **「選擇主體**」並按名稱選擇已註冊的應用。 選取 [選取]  按鈕。
1. 選取 [確定]  。
1. 選取 [儲存]  。
1. 部署應用程式。

範例`Certificate`套`IConfigurationRoot`用與機密名稱同名取得其設定值:

* 非階層值:使用 取得`SecretName`的值`config["SecretName"]`。
* 分層值(節):使用`:`(冒號)表示法`GetSection`或 擴展方法。 使用以下任一方法取得設定值值:
  * `config["Section:SecretName"]`
  * `config.GetSection("Section")["SecretName"]`

X.509 證書由操作系統管理。 套用套<xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>用*應用程式設定.json*檔提供的值:

[!code-csharp[](key-vault-configuration/samples/3.x/SampleApp/Program.cs?name=snippet1&highlight=20-23)]

範例值：

* 金鑰保存庫名稱:`contosovault`
* 應用程式識別碼:`627e911e-43cc-61d4-992e-12db9c81b413`
* 憑證指紋:`fe14593dd66b2406c5269d742d04b6e1ab03adb1`

*應用程式設定.json*:

[!code-json[](key-vault-configuration/samples/3.x/SampleApp/appsettings.json?highlight=10-12)]

運行應用時,網頁將顯示載入的機密值。 在開發環境中,機密值隨後綴一`_dev`起載入。 在生產環境中,值使用`_prod`後綴載入。

## <a name="use-managed-identities-for-azure-resources"></a>對 Azure 資源使用託管識別

**部署到 Azure 的應用**可以利用 Azure[資源的託管標識](/azure/active-directory/managed-identities-azure-resources/overview),這允許應用使用 Azure 密鑰保管庫進行身份驗證,而無需在應用中存儲認證(應用程式 ID 和密碼/用戶端金鑰)。

當*Program.cs*檔案頂部的語句`Managed`設定為`#define`時, 範例應用對 Azure 資源使用託管標識。

在應用*的應用設置.json*檔中輸入保管庫名稱。 當範例應用設定為`Managed`版本時,不需要應用程式 ID 和密碼(用戶端密鑰),因此您可以忽略這些配置條目。 應用將部署到 Azure,Azure 會驗證應用僅使用儲存在*appsettings.json*檔中的保管庫名稱訪問 Azure 密鑰保管庫。

將示例應用部署到 Azure 應用服務。

創建服務時,部署到 Azure 應用服務的應用將自動註冊到 Azure AD。 從部署中獲取對象 ID,以便在以下命令中使用。 物件識別碼顯示在應用服務的 **「標識」** 面板上的 Azure 入口中。

使用 Azure CLI 和應用程式 ID,向`list`應用程式`get`提供 存取 金鑰保管庫的許可權:

```azurecli
az keyvault set-policy --name {KEY VAULT NAME} --object-id {OBJECT ID} --secret-permissions get list
```

使用 Azure CLI、PowerShell 或 Azure 門戶**重新啟動應用**。

範例應用程式:

* 建立沒有連接字串的`AzureServiceTokenProvider`類實例。 未提供連接字串時,提供程式將嘗試從 Azure 資源的託管標識獲取訪問權杖。
* <xref:Microsoft.Azure.KeyVault.KeyVaultClient>使用`AzureServiceTokenProvider`實例令牌回調創建新。
* 實例<xref:Microsoft.Azure.KeyVault.KeyVaultClient>與載入所有機密值的預設<xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>實現 一起使用,並將雙分線`--`() 替換為鍵`:`名中的冒號 ( )。

[!code-csharp[](key-vault-configuration/samples/3.x/SampleApp/Program.cs?name=snippet2&highlight=13-21)]

金鑰保存庫名稱範例值:`contosovault`
    
*應用程式設定.json*:

```json
{
  "KeyVaultName": "Key Vault Name"
}
```

運行應用時,網頁將顯示載入的機密值。 在開發環境中,機密值具有後綴`_dev`,因為它們由使用者機密提供。 在生產環境中,值使用後綴載入,`_prod`因為它們由 Azure 密鑰保管庫提供。

如果收到錯誤`Access denied`,請確認應用已註冊 Azure AD 並提供有關密鑰保管庫的訪問許可權。 確認已重新啟動 Azure 中的服務。

有關將提供程式與託管標識和 Azure DevOps 管道一起使用的資訊,請參閱創建 Azure[資源管理器服務連接到具有託管服務標識的 VM。](/azure/devops/pipelines/library/connect-to-azure#create-an-azure-resource-manager-service-connection-to-a-vm-with-a-managed-service-identity)

## <a name="configuration-options"></a>設定選項

<xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>可以接受<xref:Microsoft.Extensions.Configuration.AzureKeyVault.AzureKeyVaultConfigurationOptions>:

```csharp
config.AddAzureKeyVault(
    new AzureKeyVaultConfigurationOptions()
    {
        ...
    });
```

| 屬性         | 描述 |
| ---------------- | ----------- |
| `Client`         | <xref:Microsoft.Azure.KeyVault.KeyVaultClient>用於檢索值。 |
| `Manager`        | <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>用於控制機密載入的實例。 |
| `ReloadInterval` | `Timespan`在輪詢金鑰保管庫以進行更改的嘗試之間等待。 默認值為`null`(未重新載入配置)。 |
| `Vault`          | 密鑰保管庫URI。 |

## <a name="use-a-key-name-prefix"></a>使用鍵名稱前置文字

<xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>提供接受 的實現<xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>的重載,它允許您控制如何將密鑰保管庫機密轉換為設定金鑰。 例如,您可以實現介面,以基於您在應用啟動時提供的首碼值載入機密值。 例如,這允許您根據應用版本載入機密。

> [!WARNING]
> 不要使用密鑰保管庫機密的首碼將多個應用的機密放入同一密鑰保管庫或將環境機密(例如 *,開發*與*生產*機密)放入同一保管庫。 我們建議不同的應用和開發/生產環境使用單獨的密鑰保管庫來隔離應用環境,以確保最高級別的安全性。

在下面的範例中,在密鑰保管庫中建立了機密(並使用開發環境的密鑰管理員工具)(`5000-AppSecret`密鑰保管庫機密名稱中不允許週期)。 此機密表示應用版本 5.0.0.0 的應用機密。 套用用的另一個版本 5.1.0.0,將機密新增到的金鑰保管庫(並使用機密管理員工具`5100-AppSecret`)中 。 每個應用版本將其版本控制的秘密值載入到其配置中`AppSecret`,作為,在載入金鑰時剝離版本。

<xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>使用自訂<xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>呼叫 :

[!code-csharp[](key-vault-configuration/samples_snapshot/Program.cs)]

實現<xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>對機密的版本首碼做出反應,以將正確的機密載入到配置中:

* `Load`當機密的名稱以前綴開頭時載入它。 其他機密未載入。
* `GetKey`:
  * 從機密名稱中刪除前置碼。
  * 將任何名稱中的兩個破折號取代為`KeyDelimiter`, 是 設定中使用的分隔符(通常是冒號)。 Azure 密鑰保管庫不允許在機密名稱中冒號。

[!code-csharp[](key-vault-configuration/samples_snapshot/Startup.cs)]

該方法`Load`由提供程式演演演算法調用,該演演演算法通過保管庫機密進行遍舍以查找具有版本首碼的提供程式演演演算法。 使用`Load`找到 版本前置碼時,演`GetKey`演演算法使用方法傳回機密名稱的配置名稱。 它將版本首碼從機密的名稱中剝離,並返回用於載入到應用的配置名稱值對中的其他機密名稱。

實作此方法時:

1. 應用在應用的專案檔中指定的版本。 在下面的範例中,套用的版本設定`5.0.0.0`為 :

   ```xml
   <PropertyGroup>
     <Version>5.0.0.0</Version>
   </PropertyGroup>
   ```

1. 確認應用的項目`<UserSecretsId>`檔案中存在屬性,`{GUID}`其中 是使用者提供的 GUID:

   ```xml
   <PropertyGroup>
     <UserSecretsId>{GUID}</UserSecretsId>
   </PropertyGroup>
   ```

   [使用秘密管理員工具](xref:security/app-secrets)在本地儲存以下機密:

   ```dotnetcli
   dotnet user-secrets set "5000-AppSecret" "5.0.0.0_secret_value_dev"
   dotnet user-secrets set "5100-AppSecret" "5.1.0.0_secret_value_dev"
   ```

1. 機密使用以下 Azure CLI 命令儲存在 Azure 金鑰保管庫中:

   ```azurecli
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "5000-AppSecret" --value "5.0.0.0_secret_value_prod"
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "5100-AppSecret" --value "5.1.0.0_secret_value_prod"
   ```

1. 運行應用時,將載入金鑰保管庫機密。 的`5000-AppSecret`字串機密與應用的專案檔 ()`5.0.0.0`中指定的應用版本匹配。

1. 版本`5000`(使用破折號)從密鑰名稱中剝離。 在整個應用中,使用密鑰`AppSecret`讀取配置將載入機密值。

1. 如果在專案檔中將應用的版本更改為,`5.1.0.0`並且應用再次運行,則傳回的機密`5.1.0.0_secret_value_dev`值位於 「開發`5.1.0.0_secret_value_prod`」環境和 「生產」 中。

> [!NOTE]
> 您還可以向<xref:Microsoft.Azure.KeyVault.KeyVaultClient><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>提供您自己的實現。 自定義客戶端允許跨應用共用用戶端的單個實例。

## <a name="bind-an-array-to-a-class"></a>將陣列繫結到類別

提供程式能夠將配置值讀取到陣列中以綁定到 POCO 陣列。

從允許鍵包含冒號`:`() 分隔符的設定源讀取時,使用數字鍵段來區分組成陣`:0:`列`:1:`&hellip;`:{n}:`( 的鍵 ) 。 有關詳細資訊,請參閱[設定:將陣列綁定到類](xref:fundamentals/configuration/index#bind-an-array-to-a-class)。

Azure 密鑰保管庫密鑰不能將冒號用作分隔符。 本主題中描述的方法使用雙破折號 ()`--`作為分層值(節)的分隔符。 陣列鍵儲存在 Azure 金鑰保管庫中,具有雙破折號和數位`--0--``--1--`鍵段&hellip;`--{n}--`(, 。

檢查 JSON 檔提供的以下[Serilog](https://serilog.net/)記錄提供程式設定。 `WriteTo`陣列中定義了兩個物件文本,反映了兩個 Serilog*接收器*,它們描述日誌記錄輸出的目標:

```json
"Serilog": {
  "WriteTo": [
    {
      "Name": "AzureTableStorage",
      "Args": {
        "storageTableName": "logs",
        "connectionString": "DefaultEnd...ountKey=Eby8...GMGw=="
      }
    },
    {
      "Name": "AzureDocumentDB",
      "Args": {
        "endpointUrl": "https://contoso.documents.azure.com:443",
        "authorizationKey": "Eby8...GMGw=="
      }
    }
  ]
}
```

前面 JSON 檔中顯示的設定使用雙破折號`--`() 表示法和數位段儲存在 Azure 金鑰保管庫中:

| Key | 值 |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="reload-secrets"></a>重新載入機密

機密被緩存,直到`IConfigurationRoot.Reload()`被調用。 在執行密鑰保管庫之前`Reload`,應用不會尊重金鑰保管庫中過期、禁用和更新的機密。

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a>已關閉和過期的機密

關閉與過期的機密引發<xref:Microsoft.Azure.KeyVault.Models.KeyVaultErrorException>。 要防止應用引發,請使用其他配置提供程式提供配置或更新禁用或過期的機密。

## <a name="troubleshoot"></a>疑難排解

當應用無法使用提供程式載入配置時,將寫入[ASP.NET 核心紀錄記錄基礎結構](xref:fundamentals/logging/index)中的錯誤訊息。 以下條件將阻止載入設定:

* 應用或證書未在 Azure 活動目錄中正確配置。
* 密鑰保管庫在 Azure 密鑰保管庫中不存在。
* 應用程式無權訪問密鑰保管庫。
* 訪問策略不包括`Get`和`List`許可權。
* 在金鑰保管庫中,配置數據(名稱值對)被錯誤地命名、丟失、禁用或過期。
* 應用具有錯誤的金鑰保管庫名稱`KeyVaultName`( )、Azure AD`AzureADApplicationId`應用程式`AzureADCertThumbprint`ID () 或 Azure AD 證書指紋 ()。
* 對於要載入的值,配置鍵(名稱)在應用中不正確。
* 將應用的存取策略添加到密鑰保管庫時,將創建該策略,但在**Access 策略**UI 中未選擇 **「儲存**」按鈕。

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/configuration/index>
* [微軟 Azure:金鑰保管庫](https://azure.microsoft.com/services/key-vault/)
* [微軟 Azure:金鑰保管庫文件](/azure/key-vault/)
* [如何為 Azure 金鑰保存庫產生並傳輸受 HSM 保護的金鑰](/azure/key-vault/key-vault-hsm-protected-keys)
* [金鑰庫用戶端類別](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
* [快速入門：使用 .NET Web 應用程式從 Azure Key Vault 設定及擷取祕密](/azure/key-vault/quick-create-net)
* [教學課程：如何在 .NET 中搭配使用 Azure Key Vault 與 Azure Windows 虛擬機器](/azure/key-vault/tutorial-net-windows-virtual-machine)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

本文件介紹如何使用[Microsoft Azure 密鑰保管庫](https://azure.microsoft.com/services/key-vault/)配置提供程式從 Azure 密鑰保管庫機密載入應用配置值。 Azure 密鑰保管庫是一種基於雲的服務,可幫助保護應用和服務使用的加密密鑰和機密。 將 Azure 金鑰保管庫用於 ASP.NET核心應用的常見方案包括:

* 控制對敏感配置數據的訪問。
* 在儲存配置資料時滿足 FIPS 140-2 級驗證的硬體安全模組 (HSM) 的要求。

[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/samples)([如何下載](xref:index#how-to-download-a-sample))

## <a name="packages"></a>Packages

向[Microsoft.擴展.配置.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/)包添加包引用。

## <a name="sample-app"></a>範例應用程式

範例應用以`#define`*Program.cs*檔案頂部的語句確定的兩種模式之一運行:

* `Certificate`&ndash;演示使用 Azure 金鑰保管庫客戶端 ID 和 X.509 憑證存取儲存在 Azure 密鑰保管庫中的秘密。 此版本的示例可以從任何位置運行,部署到 Azure 應用服務或任何能夠為 ASP.NET 酷應用提供服務的主機。
* `Managed`&ndash;演示如何[使用 Azure 資源的託管識別](/azure/active-directory/managed-identities-azure-resources/overview)使用 Azure AD 身份驗證將應用驗證為 Azure 密鑰保管庫,而無需在應用的代碼或配置中儲存認證。 使用託管標識進行身份驗證時,不需要 Azure AD 應用程式 ID 和密碼(用戶端密鑰)。 必須`Managed`將示例的版本部署到 Azure。 按照"[為 Azure 資源使用託管標識](#use-managed-identities-for-azure-resources)「部分中的指南進行操作。

有關如何使用預處理器指令 ()`#define`設定範例應用程式的詳細資訊,請<xref:index#preprocessor-directives-in-sample-code>參閱 。

## <a name="secret-storage-in-the-development-environment"></a>開發環境中的秘密儲存

使用[機密管理器工具](xref:security/app-secrets)在本地設置機密。 當示例應用在開發環境中的本地電腦上運行時,將從本地機密管理器儲存載入機密。

「機密管理員」工具需要`<UserSecretsId>`應用的專案檔中的屬性。 將屬性值`{GUID}`( ) 設定為任何唯一的 GUID:

```xml
<PropertyGroup>
  <UserSecretsId>{GUID}</UserSecretsId>
</PropertyGroup>
```

機密創建為名稱值對。 層次結構值(配置部分)在`:`[ASP.NET核心配置](xref:fundamentals/configuration/index)密鑰名稱中使用(冒號)作為分隔符。

機密管理員從開啟到項目[內容根](xref:fundamentals/index#content-root)的命令外殼使用`{SECRET NAME}`,其中的`{SECRET VALUE}`名稱和 值是:

```dotnetcli
dotnet user-secrets set "{SECRET NAME}" "{SECRET VALUE}"
```

從項目[的內容根](xref:fundamentals/index#content-root)在命令 shell 中執行以下指令,以設定範例的祕密:

```dotnetcli
dotnet user-secrets set "SecretName" "secret_value_1_dev"
dotnet user-secrets set "Section:SecretName" "secret_value_2_dev"
```

當這些機密儲存在[Azure 金鑰保管庫部分的生產環境中的「機密記憶體」中的](#secret-storage-in-the-production-environment-with-azure-key-vault)Azure`_dev`金鑰保管 庫中`_prod`時,後綴將更改為 。 後綴在應用的輸出中提供指示配置值來源的可視提示。

## <a name="secret-storage-in-the-production-environment-with-azure-key-vault"></a>使用 Azure 金鑰保存式庫的生產環境

此處匯總了[「快速入門:使用 Azure CLI」主題從 Azure 密鑰保管庫設置和檢索機密](/azure/key-vault/quick-create-cli)的說明,用於創建 Azure 密鑰保管庫並儲存範例應用使用的秘密。 有關詳細資訊,請參閱主題。

1. 使用[Azure 門戶](https://portal.azure.com/)中的以下任一方法打開 Azure 雲外殼:

   * 選取程式碼區塊右上角的 [試試看]  。 在文字框中使用搜索字串「Azure CLI」。。
   * 使用**啟動雲外殼**按鈕在瀏覽器中打開雲外殼。
   * 選取 Azure 入口網站右上角功能表上的 **[Cloud Shell]** 按鈕。

   有關詳細資訊,請參閱[Azure CLI](/cli/azure/)和[Azure 雲外殼概述](/azure/cloud-shell/overview)。

1. 如果您尚未通過身份驗證,請使用命令`az login`登錄。

1. 使用以下指令建立資源群組,其中`{RESOURCE GROUP NAME}`為新資源群組的資源組名`{LOCATION}`稱為 Azure 區域(資料中心):

   ```azurecli
   az group create --name "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. 使用以下指令在資源群組中建立金鑰保管庫,其中`{KEY VAULT NAME}`為新密鑰保管庫的名稱`{LOCATION}`, 是 Azure 區域(資料中心):

   ```azurecli
   az keyvault create --name {KEY VAULT NAME} --resource-group "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. 在密鑰保管庫中創建機密作為名稱值對。

   Azure 密鑰保管庫金鑰庫密鑰名稱僅限於字母數位字元和破折號。 階層值(配置部分)使用`--`(兩個破折號)作為分隔符。 在密鑰保管庫密鑰名稱中,通常不允許從[ASP.NET Core 配置](xref:fundamentals/configuration/index)中從子鍵中分隔節的冒號。 因此,當機密載入應用的配置中時,將使用兩個破折號並將其交換為冒號。

   以下機密用於示例應用。 這些值包括後`_prod`綴,用於將它們與在開發環境`_dev`中 載入的後綴值與使用者機密區分開來。 取代為`{KEY VAULT NAME}`在上一步驟中建立的金鑰保管庫的名稱:

   ```azurecli
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "SecretName" --value "secret_value_1_prod"
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "Section--SecretName" --value "secret_value_2_prod"
   ```

## <a name="use-application-id-and-x509-certificate-for-non-azure-hosted-apps"></a>對非 Azure 託管應用使用應用程式 ID 和 X.509 憑證

將 Azure AD、Azure 密鑰保管庫和應用配置為使用 Azure 活動目錄應用程式 ID 和 X.509 證書在**應用託管在 Azure 之外時**對金鑰保管庫進行身份驗證。 如需詳細資訊，請參閱[關於金鑰、祕密和憑證](/azure/key-vault/about-keys-secrets-and-certificates)。

> [!NOTE]
> 儘管 Azure 中託管的應用支援使用應用程式 ID 和 X.509 憑證,但我們建議在 Azure 中託管應用時[對 Azure 資源使用託管標識](#use-managed-identities-for-azure-resources)。 託管標識不需要在應用或開發環境中存儲證書。

當*Program.cs*檔案頂部的語句`Certificate`設定為`#define`時, 範例應用程式使用應用程式 ID 和 X.509 憑證。

1. 創建 PKCS_12 存檔 (*.pfx*) 證書。 建立憑證的選項包括[Windows 上的 MakeCert](/windows/desktop/seccrypto/makecert)與[OpenSSL](https://www.openssl.org/)。
1. 將憑證安裝到當前使用者的個人憑證儲存中。 將密鑰標記為可匯出是可選的。 請注意證書的指紋,此過程稍後將使用。
1. 將 PKCS_12 存檔 *(.pfx*) 證書匯出為 DER 編碼證書 *(.cer*)。
1. 使用 Azure AD(**應用註冊)註冊**應用。
1. 將 DER 編碼的憑證 *(.cer*) 上傳到 Azure AD:
   1. 在 Azure AD 中選擇應用。
   1. 導覽到**憑證&機密**。
   1. 選擇 **「上載證書**」以上載包含公開金鑰的憑證。 *.cer* *、.pem*或 *.crt*證書是可以接受的。
1. 將金鑰保管庫名稱、應用程式 ID 和證書指紋存儲在應用*的應用設置.json*檔中。
1. 瀏覽到 Azure 門戶中的**金鑰保管庫**。
1. [使用 Azure 金鑰保管庫部分在「生產環境」 中的秘密儲存中選擇在「機密存儲」 中](#secret-storage-in-the-production-environment-with-azure-key-vault)創建的金鑰保管庫。
1. 選擇**存取原則**。
1. 選擇 **「添加存取策略**」。
1. 打開 **「機密」許可權**,並提供具有**獲取**和**列表**許可權的應用。
1. 選擇 **「選擇主體**」並按名稱選擇已註冊的應用。 選取 [選取]  按鈕。
1. 選取 [確定]  。
1. 選取 [儲存]  。
1. 部署應用程式。

範例`Certificate`套`IConfigurationRoot`用與機密名稱同名取得其設定值:

* 非階層值:使用 取得`SecretName`的值`config["SecretName"]`。
* 分層值(節):使用`:`(冒號)表示法`GetSection`或 擴展方法。 使用以下任一方法取得設定值值:
  * `config["Section:SecretName"]`
  * `config.GetSection("Section")["SecretName"]`

X.509 證書由操作系統管理。 套用套<xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>用*應用程式設定.json*檔提供的值:

[!code-csharp[](key-vault-configuration/samples/2.x/SampleApp/Program.cs?name=snippet1&highlight=20-23)]

範例值：

* 金鑰保存庫名稱:`contosovault`
* 應用程式識別碼:`627e911e-43cc-61d4-992e-12db9c81b413`
* 憑證指紋:`fe14593dd66b2406c5269d742d04b6e1ab03adb1`

*應用程式設定.json*:

[!code-json[](key-vault-configuration/samples/2.x/SampleApp/appsettings.json?highlight=10-12)]

運行應用時,網頁將顯示載入的機密值。 在開發環境中,機密值隨後綴一`_dev`起載入。 在生產環境中,值使用`_prod`後綴載入。

## <a name="use-managed-identities-for-azure-resources"></a>對 Azure 資源使用託管識別

**部署到 Azure 的應用**可以利用 Azure[資源的託管標識](/azure/active-directory/managed-identities-azure-resources/overview),這允許應用使用 Azure 密鑰保管庫進行身份驗證,而無需在應用中存儲認證(應用程式 ID 和密碼/用戶端金鑰)。

當*Program.cs*檔案頂部的語句`Managed`設定為`#define`時, 範例應用對 Azure 資源使用託管標識。

在應用*的應用設置.json*檔中輸入保管庫名稱。 當範例應用設定為`Managed`版本時,不需要應用程式 ID 和密碼(用戶端密鑰),因此您可以忽略這些配置條目。 應用將部署到 Azure,Azure 會驗證應用僅使用儲存在*appsettings.json*檔中的保管庫名稱訪問 Azure 密鑰保管庫。

將示例應用部署到 Azure 應用服務。

創建服務時,部署到 Azure 應用服務的應用將自動註冊到 Azure AD。 從部署中獲取對象 ID,以便在以下命令中使用。 物件識別碼顯示在應用服務的 **「標識」** 面板上的 Azure 入口中。

使用 Azure CLI 和應用程式 ID,向`list`應用程式`get`提供 存取 金鑰保管庫的許可權:

```azurecli
az keyvault set-policy --name {KEY VAULT NAME} --object-id {OBJECT ID} --secret-permissions get list
```

使用 Azure CLI、PowerShell 或 Azure 門戶**重新啟動應用**。

範例應用程式:

* 建立沒有連接字串的`AzureServiceTokenProvider`類實例。 未提供連接字串時,提供程式將嘗試從 Azure 資源的託管標識獲取訪問權杖。
* <xref:Microsoft.Azure.KeyVault.KeyVaultClient>使用`AzureServiceTokenProvider`實例令牌回調創建新。
* 實例<xref:Microsoft.Azure.KeyVault.KeyVaultClient>與載入所有機密值的預設<xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>實現 一起使用,並將雙分線`--`() 替換為鍵`:`名中的冒號 ( )。

[!code-csharp[](key-vault-configuration/samples/2.x/SampleApp/Program.cs?name=snippet2&highlight=13-21)]

金鑰保存庫名稱範例值:`contosovault`
    
*應用程式設定.json*:

```json
{
  "KeyVaultName": "Key Vault Name"
}
```

運行應用時,網頁將顯示載入的機密值。 在開發環境中,機密值具有後綴`_dev`,因為它們由使用者機密提供。 在生產環境中,值使用後綴載入,`_prod`因為它們由 Azure 密鑰保管庫提供。

如果收到錯誤`Access denied`,請確認應用已註冊 Azure AD 並提供有關密鑰保管庫的訪問許可權。 確認已重新啟動 Azure 中的服務。

有關將提供程式與託管標識和 Azure DevOps 管道一起使用的資訊,請參閱創建 Azure[資源管理器服務連接到具有託管服務標識的 VM。](/azure/devops/pipelines/library/connect-to-azure#create-an-azure-resource-manager-service-connection-to-a-vm-with-a-managed-service-identity)

## <a name="use-a-key-name-prefix"></a>使用鍵名稱前置文字

<xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>提供接受 的實現<xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>的重載,它允許您控制如何將密鑰保管庫機密轉換為設定金鑰。 例如,您可以實現介面,以基於您在應用啟動時提供的首碼值載入機密值。 例如,這允許您根據應用版本載入機密。

> [!WARNING]
> 不要使用密鑰保管庫機密的首碼將多個應用的機密放入同一密鑰保管庫或將環境機密(例如 *,開發*與*生產*機密)放入同一保管庫。 我們建議不同的應用和開發/生產環境使用單獨的密鑰保管庫來隔離應用環境,以確保最高級別的安全性。

在下面的範例中,在密鑰保管庫中建立了機密(並使用開發環境的密鑰管理員工具)(`5000-AppSecret`密鑰保管庫機密名稱中不允許週期)。 此機密表示應用版本 5.0.0.0 的應用機密。 套用用的另一個版本 5.1.0.0,將機密新增到的金鑰保管庫(並使用機密管理員工具`5100-AppSecret`)中 。 每個應用版本將其版本控制的秘密值載入到其配置中`AppSecret`,作為,在載入金鑰時剝離版本。

<xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>使用自訂<xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>呼叫 :

[!code-csharp[](key-vault-configuration/samples_snapshot/Program.cs)]

實現<xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>對機密的版本首碼做出反應,以將正確的機密載入到配置中:

* `Load`當機密的名稱以前綴開頭時載入它。 其他機密未載入。
* `GetKey`:
  * 從機密名稱中刪除前置碼。
  * 將任何名稱中的兩個破折號取代為`KeyDelimiter`, 是 設定中使用的分隔符(通常是冒號)。 Azure 密鑰保管庫不允許在機密名稱中冒號。

[!code-csharp[](key-vault-configuration/samples_snapshot/Startup.cs)]

該方法`Load`由提供程式演演演算法調用,該演演演算法通過保管庫機密進行遍舍以查找具有版本首碼的提供程式演演演算法。 使用`Load`找到 版本前置碼時,演`GetKey`演演算法使用方法傳回機密名稱的配置名稱。 它將版本首碼從機密的名稱中剝離,並返回用於載入到應用的配置名稱值對中的其他機密名稱。

實作此方法時:

1. 應用在應用的專案檔中指定的版本。 在下面的範例中,套用的版本設定`5.0.0.0`為 :

   ```xml
   <PropertyGroup>
     <Version>5.0.0.0</Version>
   </PropertyGroup>
   ```

1. 確認應用的項目`<UserSecretsId>`檔案中存在屬性,`{GUID}`其中 是使用者提供的 GUID:

   ```xml
   <PropertyGroup>
     <UserSecretsId>{GUID}</UserSecretsId>
   </PropertyGroup>
   ```

   [使用秘密管理員工具](xref:security/app-secrets)在本地儲存以下機密:

   ```dotnetcli
   dotnet user-secrets set "5000-AppSecret" "5.0.0.0_secret_value_dev"
   dotnet user-secrets set "5100-AppSecret" "5.1.0.0_secret_value_dev"
   ```

1. 機密使用以下 Azure CLI 命令儲存在 Azure 金鑰保管庫中:

   ```azurecli
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "5000-AppSecret" --value "5.0.0.0_secret_value_prod"
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "5100-AppSecret" --value "5.1.0.0_secret_value_prod"
   ```

1. 運行應用時,將載入金鑰保管庫機密。 的`5000-AppSecret`字串機密與應用的專案檔 ()`5.0.0.0`中指定的應用版本匹配。

1. 版本`5000`(使用破折號)從密鑰名稱中剝離。 在整個應用中,使用密鑰`AppSecret`讀取配置將載入機密值。

1. 如果在專案檔中將應用的版本更改為,`5.1.0.0`並且應用再次運行,則傳回的機密`5.1.0.0_secret_value_dev`值位於 「開發`5.1.0.0_secret_value_prod`」環境和 「生產」 中。

> [!NOTE]
> 您還可以向<xref:Microsoft.Azure.KeyVault.KeyVaultClient><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>提供您自己的實現。 自定義客戶端允許跨應用共用用戶端的單個實例。

## <a name="bind-an-array-to-a-class"></a>將陣列繫結到類別

提供程式能夠將配置值讀取到陣列中以綁定到 POCO 陣列。

從允許鍵包含冒號`:`() 分隔符的設定源讀取時,使用數字鍵段來區分組成陣`:0:`列`:1:`&hellip;`:{n}:`( 的鍵 ) 。 有關詳細資訊,請參閱[設定:將陣列綁定到類](xref:fundamentals/configuration/index#bind-an-array-to-a-class)。

Azure 密鑰保管庫密鑰不能將冒號用作分隔符。 本主題中描述的方法使用雙破折號 ()`--`作為分層值(節)的分隔符。 陣列鍵儲存在 Azure 金鑰保管庫中,具有雙破折號和數位`--0--``--1--`鍵段&hellip;`--{n}--`(, 。

檢查 JSON 檔提供的以下[Serilog](https://serilog.net/)記錄提供程式設定。 `WriteTo`陣列中定義了兩個物件文本,反映了兩個 Serilog*接收器*,它們描述日誌記錄輸出的目標:

```json
"Serilog": {
  "WriteTo": [
    {
      "Name": "AzureTableStorage",
      "Args": {
        "storageTableName": "logs",
        "connectionString": "DefaultEnd...ountKey=Eby8...GMGw=="
      }
    },
    {
      "Name": "AzureDocumentDB",
      "Args": {
        "endpointUrl": "https://contoso.documents.azure.com:443",
        "authorizationKey": "Eby8...GMGw=="
      }
    }
  ]
}
```

前面 JSON 檔中顯示的設定使用雙破折號`--`() 表示法和數位段儲存在 Azure 金鑰保管庫中:

| Key | 值 |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="reload-secrets"></a>重新載入機密

機密被緩存,直到`IConfigurationRoot.Reload()`被調用。 在執行密鑰保管庫之前`Reload`,應用不會尊重金鑰保管庫中過期、禁用和更新的機密。

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a>已關閉和過期的機密

關閉與過期的機密引發<xref:Microsoft.Azure.KeyVault.Models.KeyVaultErrorException>。 要防止應用引發,請使用其他配置提供程式提供配置或更新禁用或過期的機密。

## <a name="troubleshoot"></a>疑難排解

當應用無法使用提供程式載入配置時,將寫入[ASP.NET 核心紀錄記錄基礎結構](xref:fundamentals/logging/index)中的錯誤訊息。 以下條件將阻止載入設定:

* 應用或證書未在 Azure 活動目錄中正確配置。
* 密鑰保管庫在 Azure 密鑰保管庫中不存在。
* 應用程式無權訪問密鑰保管庫。
* 訪問策略不包括`Get`和`List`許可權。
* 在金鑰保管庫中,配置數據(名稱值對)被錯誤地命名、丟失、禁用或過期。
* 應用具有錯誤的金鑰保管庫名稱`KeyVaultName`( )、Azure AD`AzureADApplicationId`應用程式`AzureADCertThumbprint`ID () 或 Azure AD 證書指紋 ()。
* 對於要載入的值,配置鍵(名稱)在應用中不正確。
* 將應用的存取策略添加到密鑰保管庫時,將創建該策略,但在**Access 策略**UI 中未選擇 **「儲存**」按鈕。

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/configuration/index>
* [微軟 Azure:金鑰保管庫](https://azure.microsoft.com/services/key-vault/)
* [微軟 Azure:金鑰保管庫文件](/azure/key-vault/)
* [如何為 Azure 金鑰保存庫產生並傳輸受 HSM 保護的金鑰](/azure/key-vault/key-vault-hsm-protected-keys)
* [金鑰庫用戶端類別](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
* [快速入門：使用 .NET Web 應用程式從 Azure Key Vault 設定及擷取祕密](/azure/key-vault/quick-create-net)
* [教學課程：如何在 .NET 中搭配使用 Azure Key Vault 與 Azure Windows 虛擬機器](/azure/key-vault/tutorial-net-windows-virtual-machine)

::: moniker-end

