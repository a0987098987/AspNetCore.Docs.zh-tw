---
title: ASP.NET Core 中的 Azure Key Vault 設定提供者
author: guardrex
description: 瞭解如何使用 Azure Key Vault 設定提供者, 使用在執行時間載入的名稱/值配對來設定應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/01/2019
uid: security/key-vault-configuration
ms.openlocfilehash: 0d0b6e20a1901d4a2630ce263b5fd0cd7bcca8fe
ms.sourcegitcommit: 4fe3ae892f54dc540859bff78741a28c2daa9a38
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/04/2019
ms.locfileid: "68776659"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a>ASP.NET Core 中的 Azure Key Vault 設定提供者

By [Luke Latham](https://github.com/guardrex)和[Andrew Stanton-護士](https://github.com/anurse)

本檔說明如何使用[Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/)設定提供者, 從 Azure Key Vault 秘密載入應用程式設定值。 Azure Key Vault 是一種雲端式服務, 可協助保護應用程式和服務所使用的密碼編譯金鑰和密碼。 搭配 ASP.NET Core 應用程式使用 Azure Key Vault 的常見案例包括:

* 控制敏感性設定資料的存取權。
* 當儲存設定資料時, 符合 FIPS 140-2 Level 2 驗證的硬體安全性模組 (HSM) 的需求。

此案例適用于以 ASP.NET Core 2.1 或更新版本為目標的應用程式。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="packages"></a>Packages

若要使用 Azure Key Vault 設定提供者, 請將套件參考新增至[AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/)套件。

若要採用[適用于 Azure 資源的受控](/azure/active-directory/managed-identities-azure-resources/overview)識別案例, 請將套件參考新增至[AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/)套件。

> [!NOTE]
> 在撰寫本文時, 最新的穩定版本`Microsoft.Azure.Services.AppAuthentication`(版本`1.0.3`) 可支援[系統指派的受控](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-work)識別。 `1.2.0-preview2`套件中提供*使用者指派受控*識別的支援。 本主題示範如何使用系統管理的身分識別, 而提供的範例應用程式會`1.0.3`使用版本`Microsoft.Azure.Services.AppAuthentication`的封裝。

## <a name="sample-app"></a>範例應用程式

範例應用程式會以`#define` *Program.cs*檔案頂端的語句所決定的兩種模式之一來執行:

* `Certificate`&ndash;示範如何使用 Azure Key Vault 的用戶端識別碼和 x.509 憑證, 來存取儲存在 Azure Key Vault 中的秘密。 這個版本的範例可以從任何位置執行, 部署至 Azure App Service 或任何能夠提供 ASP.NET Core 應用程式的主機。
* `Managed`示範如何使用[適用于 Azure 資源的受控](/azure/active-directory/managed-identities-azure-resources/overview)識別來驗證應用程式, 以在未儲存于應用程式代碼或設定中的認證 Azure AD 驗證 Azure Key Vault。 &ndash; 使用受控識別進行驗證時, 不需要 Azure AD 應用程式識別碼和密碼 (用戶端密碼)。 範例`Managed`的版本必須部署至 Azure。 請遵循[使用適用于 Azure 資源的受控](#use-managed-identities-for-azure-resources)識別一節中的指導方針。

如需有關如何使用預處理器指示詞 (`#define`) 來設定範例應用程式的詳細資訊, 請參閱。 <xref:index#preprocessor-directives-in-sample-code>

## <a name="secret-storage-in-the-development-environment"></a>開發環境中的秘密儲存

使用[秘密管理員工具](xref:security/app-secrets)在本機設定秘密。 當範例應用程式在開發環境中的本機電腦上執行時, 會從本機密碼管理員存放區載入秘密。

「密碼管理員」工具需要`<UserSecretsId>`應用程式專案檔中的屬性。 將屬性值 (`{GUID}`) 設定為任何唯一的 GUID:

```xml
<PropertyGroup>
  <UserSecretsId>{GUID}</UserSecretsId>
</PropertyGroup>
```

密碼會以名稱/值組的形式建立。 階層式值 (設定區段) 使用`:` (冒號) 做為[ASP.NET Core](xref:fundamentals/configuration/index)設定機碼名稱中的分隔符號。

密碼管理員會從開啟的命令 shell 使用到專案的內容根目錄, 其中`{SECRET NAME}`是名稱, 而`{SECRET VALUE}`是值:

```console
dotnet user-secrets set "{SECRET NAME}" "{SECRET VALUE}"
```

在命令 shell 中, 從專案的內容根目錄執行下列命令, 以設定範例應用程式的秘密:

```console
dotnet user-secrets set "SecretName" "secret_value_1_dev"
dotnet user-secrets set "Section:SecretName" "secret_value_2_dev"
```

當這些秘密儲存在具有 Azure Key Vault 區段之[生產環境中的秘密儲存](#secret-storage-in-the-production-environment-with-azure-key-vault)Azure Key Vault 中時, `_dev`尾碼會變更為`_prod`。 尾碼會在應用程式的輸出中提供視覺提示, 指出設定值的來源。

## <a name="secret-storage-in-the-production-environment-with-azure-key-vault"></a>在生產環境中使用 Azure Key Vault 的秘密儲存

[快速入門所提供的指示:若要建立 Azure Key Vault 並儲存範例應用程式](/azure/key-vault/quick-create-cli)所使用的秘密, 請從 Azure Key Vault 使用 Azure CLI 主題來設定和取出秘密。 如需進一步的詳細資料, 請參閱主題。

1. 使用下列其中一種方法, 在[Azure 入口網站](https://portal.azure.com/)中開啟 Azure Cloud shell:

   * 選取程式碼區塊右上角的 [**試試看**]。 在文字方塊中使用搜尋字串 "Azure CLI"。
   * 使用 [**啟動 Cloud Shell** ] 按鈕, 在瀏覽器中開啟 Cloud Shell。
   * 在 Azure 入口網站右上角的功能表上, 選取 [ **Cloud Shell** ] 按鈕。

   如需詳細資訊, 請參閱[Azure 命令列介面 (CLI)](/cli/azure/)和[Azure Cloud Shell 的總覽](/azure/cloud-shell/overview)。

1. 如果您尚未驗證, 請使用`az login`命令登入。

1. 使用下列命令建立資源群組, 其中`{RESOURCE GROUP NAME}`是新資源群組的資源組名, 而`{LOCATION}`是 Azure 區域 (datacenter):

   ```console
   az group create --name "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. 使用下列命令在資源群組中建立 key vault, 其中是新`{KEY VAULT NAME}`金鑰保存庫的名稱, 而`{LOCATION}`是 Azure 區域 (datacenter):

   ```console
   az keyvault create --name "{KEY VAULT NAME}" --resource-group "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. 在金鑰保存庫中建立秘密, 做為名稱/值配對。

   Azure Key Vault 秘密名稱僅限於英數位元和連字號。 階層式值 (設定區段) `--`使用 (兩個虛線) 做為分隔符號。 冒號, 通常用來從[ASP.NET Core](xref:fundamentals/configuration/index)設定中的子機碼分隔區段, 但不允許用在金鑰保存庫密碼名稱中。 因此, 當密碼載入應用程式的設定時, 會使用兩個破折號並交換冒號。

   下列秘密可用於範例應用程式。 這些值包含`_prod`後置詞, 以區別它們`_dev`與開發環境中從使用者秘密載入的尾碼值。 將`{KEY VAULT NAME}`取代為您在上一個步驟中建立的金鑰保存庫名稱:

   ```console
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "SecretName" --value "secret_value_1_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "Section--SecretName" --value "secret_value_2_prod"
   ```

## <a name="use-application-id-and-x509-certificate-for-non-azure-hosted-apps"></a>將應用程式識別碼和 x.509 憑證用於非 Azure 託管的應用程式

設定 Azure AD、Azure Key Vault 和應用程式, 以在應用程式裝載于**Azure 外部時**, 使用 Azure Active Directory 應用程式識別碼和 x.509 憑證來驗證金鑰保存庫。 如需詳細資訊, 請參閱[關於金鑰、秘密和憑證](/azure/key-vault/about-keys-secrets-and-certificates)。

> [!NOTE]
> 雖然 Azure 中裝載的應用程式支援使用應用程式識別碼和 x.509 憑證, 但建議您在 Azure 中裝載應用程式時, 使用[適用于 azure 資源的受控](#use-managed-identities-for-azure-resources)識別。 受控識別不需要在應用程式中或在開發環境中儲存憑證。

當`#define` *Program.cs*檔案頂端的語句設定為`Certificate`時, 範例應用程式會使用應用程式識別碼和 x.509 憑證。

1. 建立 PKCS # 12 封存檔案 ( *.pfx*) 憑證。 建立憑證的選項包括[Windows](/windows/desktop/seccrypto/makecert)和[OpenSSL](https://www.openssl.org/)上的 MakeCert。
1. 將憑證安裝到目前使用者的個人憑證存儲。 將金鑰標示為可匯出是選擇性的。 記下憑證的指紋, 這會在此程式稍後使用。
1. 將 PKCS # 12 封存 ( *.pfx*) 憑證匯出為 DER 編碼憑證 ( *.cer*)。
1. 使用 Azure AD (**應用程式註冊**) 註冊應用程式。
1. 將 DER 編碼的憑證 ( *.cer*) 上傳至 Azure AD:
   1. 在 Azure AD 中選取應用程式。
   1. 流覽至 [**憑證 & 密碼**]。
   1. 選取 [**上傳憑證**] 來上傳包含公開金鑰的憑證。 *.Cer*、 *pem*或 *.crt*憑證是可接受的。
1. 將金鑰保存庫名稱、應用程式識別碼和憑證指紋儲存在應用程式的*appsettings*中。
1. 流覽至 Azure 入口網站中的 [**金鑰保存庫**]。
1. 選取您在[生產環境中使用 Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault)一節所建立的金鑰保存庫。
1. 選取 [**存取原則**]。
1. 選取 [**加入新**的]。
1. 選取 [**選取主體**], 然後依名稱選取已註冊的應用程式。 選取 [**選取**] 按鈕。
1. 開啟 [**秘密許可權**], 並提供具有 [**取得**] 和 [**列出**] 許可權的應用程式。
1. 選取 [確定]。
1. 選取 [儲存]。
1. 部署應用程式。

範例應用程式會從`IConfigurationRoot`使用與秘密名稱相同的名稱取得其設定值: `Certificate`

* 非階層式值:的值`SecretName`是使用`config["SecretName"]`取得。
* 階層式值 (區段):請`:`使用 (冒號) 標記法`GetSection`或擴充方法。 使用下列其中一種方法來取得設定值:
  * `config["Section:SecretName"]`
  * `config.GetSection("Section")["SecretName"]`

X.509 憑證是由作業系統所管理。 應用程式會`AddAzureKeyVault`使用*appsettings*所提供的值來呼叫:

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet1&highlight=20-23)]

範例值:

* 金鑰保存庫名稱:`contosovault`
* 應用程式識別碼:`627e911e-43cc-61d4-992e-12db9c81b413`
* 憑證指紋:`fe14593dd66b2406c5269d742d04b6e1ab03adb1`

*appsettings.json*：

[!code-json[](key-vault-configuration/sample/appsettings.json)]

當您執行應用程式時, 網頁會顯示已載入的密碼值。 在開發環境中, 秘密值會以`_dev`尾碼載入。 在生產環境中, 值會以`_prod`尾碼載入。

## <a name="use-managed-identities-for-azure-resources"></a>使用適用于 Azure 資源的受控識別

**部署至 azure 的應用程式**可以利用[Azure 資源的受控](/azure/active-directory/managed-identities-azure-resources/overview)識別, 讓應用程式使用不需認證的 Azure AD 驗證 (應用程式識別碼和密碼/用戶端密碼) 驗證 Azure Key Vault儲存在應用程式中。

當`#define` *Program.cs*檔案頂端的語句設定為`Managed`時, 範例應用程式會使用適用于 Azure 資源的受控識別。

在應用程式的*appsettings*中輸入保存庫名稱。 在設定為`Managed`版本時, 範例應用程式不需要應用程式識別碼和密碼 (用戶端密碼), 因此您可以忽略這些設定專案。 應用程式會部署至 Azure, 而 Azure 只會使用儲存在*appsettings*中的保存庫名稱來驗證應用程式, 以存取 Azure Key Vault。

將範例應用程式部署至 Azure App Service。

部署到 Azure App Service 的應用程式會在建立服務時, 自動向 Azure AD 註冊。 從部署取得物件識別碼, 以便在下列命令中使用。 物件識別碼會顯示在 App Service 的 身分**識別** 面板上的 Azure 入口網站。

使用 Azure CLI 和應用程式的物件識別碼, 提供應用程式`list`和`get`存取金鑰保存庫的許可權:

```console
az keyvault set-policy --name '{KEY VAULT NAME}' --object-id {OBJECT ID} --secret-permissions get list
```

使用 Azure CLI、PowerShell 或 Azure 入口網站**重新開機應用程式**。

範例應用程式:

* 建立不含連接字串`AzureServiceTokenProvider`之類別的實例。 未提供連接字串時, 提供者會嘗試從 Azure 資源的受控識別取得存取權杖。
* 新`KeyVaultClient`的會`AzureServiceTokenProvider`使用實例 token 回呼來建立。
* 實例是與的預設`IKeyVaultSecretManager`執行搭配使用, 它會載入所有秘密值, 並以冒號 (`--` `:`) 取代索引鍵名稱中的雙破折號 ()。 `KeyVaultClient`

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet2&highlight=13-21)]

當您執行應用程式時, 網頁會顯示已載入的密碼值。 在開發環境中, 秘密值具有`_dev`尾碼, 因為它們是由使用者密碼提供。 在生產環境中, 值是以`_prod`尾碼載入, 因為它們是由 Azure Key Vault 所提供。

如果您收到`Access denied`錯誤, 請確認已向 Azure AD 註冊應用程式, 並提供金鑰保存庫的存取權。 確認您已在 Azure 中重新開機服務。

## <a name="use-a-key-name-prefix"></a>使用索引鍵名稱前置詞

`AddAzureKeyVault`提供接受的執行`IKeyVaultSecretManager`的多載, 可讓您控制如何將金鑰保存庫密碼轉換成設定金鑰。 例如, 您可以根據您在應用程式啟動時提供的首碼值, 執行介面來載入密碼值。 例如, 這可讓您根據應用程式的版本來載入密碼。

> [!WARNING]
> 請勿在金鑰保存庫秘密上使用前置詞, 將多個應用程式的秘密放入相同的金鑰保存庫, 或將環境秘密 (例如*開發*與*生產*密碼) 放入相同的保存庫中。 我們建議不同的應用程式和開發/生產環境使用不同的金鑰保存庫, 以隔離最高安全性層級的應用程式環境。

在下列範例中, 會在金鑰保存庫中建立秘密 (並使用適用于開發環境的秘密管理員工具) 來`5000-AppSecret`進行 (金鑰保存庫密碼名稱中不允許週期)。 此秘密代表應用程式版本5.0.0.0 的應用程式密碼。 針對其他版本的應用程式 (5.1.0.0), 會將密碼新增至金鑰保存庫 (並使用密碼管理員工具) 來`5100-AppSecret`進行。 每個應用程式版本都會將其版本設定的秘密`AppSecret`值載入至其設定中, 因為它會在載入秘密時去除版本。

`AddAzureKeyVault`使用自訂`IKeyVaultSecretManager`進行呼叫:

[!code-csharp[](key-vault-configuration/sample_snapshot/Program.cs?highlight=30-34)]

`IKeyVaultSecretManager`執行動作會回應秘密的版本前置詞, 以將適當的密碼載入設定中:

[!code-csharp[](key-vault-configuration/sample_snapshot/Startup.cs?name=snippet1)]

`Load`方法是由可逐一查看保存庫密碼的提供者演算法所呼叫, 以尋找具有版本前置詞的金鑰。 當找到`Load`版本前置詞時, 演算法會`GetKey`使用方法來傳回密碼名稱的設定名稱。 它會從密碼的名稱中去除版本前置詞, 並傳回其餘的秘密名稱, 以載入至應用程式的設定名稱/值配對。

當此方法執行時:

1. 應用程式的專案檔中指定的應用程式版本。 在下列範例中, 應用程式的版本會設定為`5.0.0.0`:

   ```xml
   <PropertyGroup>
     <Version>5.0.0.0</Version>
   </PropertyGroup>
   ```

1. 確認屬性存在於應用程式的專案檔中, 其中`{GUID}`是使用者提供的 GUID: `<UserSecretsId>`

   ```xml
   <PropertyGroup>
     <UserSecretsId>{GUID}</UserSecretsId>
   </PropertyGroup>
   ```

   使用[秘密管理員工具](xref:security/app-secrets)在本機儲存下列秘密:

   ```console
   dotnet user-secrets set "5000-AppSecret" "5.0.0.0_secret_value_dev"
   dotnet user-secrets set "5100-AppSecret" "5.1.0.0_secret_value_dev"
   ```

1. 使用下列 Azure CLI 命令, 將秘密儲存在 Azure Key Vault 中:

   ```console
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5000-AppSecret" --value "5.0.0.0_secret_value_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5100-AppSecret" --value "5.1.0.0_secret_value_prod"
   ```

1. 當應用程式執行時, 會載入金鑰保存庫密碼。 的字串秘密`5000-AppSecret`會符合應用程式的專案檔 (`5.0.0.0`) 中指定的應用程式版本。

1. 版本`5000` (含破折號) 會從索引鍵名稱中移除。 在整個應用程式中, 使用金鑰`AppSecret`來讀取設定會載入秘密值。

1. 如果應用程式的版本已在專案檔中變更為`5.1.0.0` , 且應用程式再次執行, 則傳回的秘密值`5.1.0.0_secret_value_dev`會在開發環境和`5.1.0.0_secret_value_prod`生產環境中。

> [!NOTE]
> 您也可以將自己`KeyVaultClient`的實作為提供給。 `AddAzureKeyVault` 自訂用戶端允許跨應用程式共用單一用戶端實例。

## <a name="bind-an-array-to-a-class"></a>將陣列繫結到類別

提供者能夠將設定值讀入陣列中, 以系結至 POCO 陣列。

從允許金鑰包含冒號 (`:`) 分隔符號的設定來源讀取時, 會使用數值索引鍵區段來區別組成陣列的索引鍵 (`:0:`、 `:1:`、。 `:{n}:`)。 如需詳細資訊, [請參閱 Configuration:將陣列系結至類別](xref:fundamentals/configuration/index#bind-an-array-to-a-class)。

Azure Key Vault 索引鍵不能使用冒號做為分隔符號。 本主題中所述的方法會使用雙`--`虛線 () 做為階層式值的分隔符號 (區段)。 陣列索引鍵會以雙虛線和數位索引鍵區段`--0--`(、 `--1--`、 &hellip; `--{n}--`) 儲存在 Azure Key Vault 中。

檢查 JSON 檔案所提供的下列[Serilog](https://serilog.net/)記錄提供者設定。 `WriteTo`陣列中定義了兩個物件常值, 以反映兩個 Serilog*接收*, 其中描述記錄輸出的目的地:

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

先前 JSON 檔案中顯示的設定會使用雙虛線 (`--`) 標記法和數值區段儲存在 Azure Key Vault 中:

| Key | 值 |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="reload-secrets"></a>重載秘密

會快取密碼`IConfigurationRoot.Reload()` , 直到呼叫為止。 在執行之前`Reload` , 應用程式不會遵守金鑰保存庫中已過期、已停用及更新的秘密。

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a>已停用和過期的秘密

已停用和過期的`KeyVaultClientException`秘密會在執行時間擲回。 若要防止應用程式擲回, 請使用不同的設定提供者來提供設定, 或更新已停用或已過期的密碼。

## <a name="troubleshoot"></a>疑難排解

當應用程式無法使用提供者載入設定時, 會將錯誤訊息寫入[ASP.NET Core 記錄基礎結構](xref:fundamentals/logging/index)。 下列條件將導致無法載入設定:

* 應用程式或憑證未在 Azure Active Directory 中正確設定。
* 金鑰保存庫不存在於 Azure Key Vault 中。
* 應用程式未獲授權, 無法存取金鑰保存庫。
* 存取原則不包含`Get`和`List`許可權。
* 在金鑰保存庫中, 設定資料 (名稱/值組) 未正確命名、遺失、停用或過期。
* 應用程式具有錯誤的金鑰保存庫名稱`KeyVaultName`()、Azure AD 應用程式`AzureADApplicationId`識別碼 (), 或 Azure AD 憑證`AzureADCertThumbprint`指紋 ()。
* 在應用程式中, 設定金鑰 (名稱) 不正確, 因為您嘗試載入的值。

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/configuration/index>
* [Microsoft Azure:Key Vault](https://azure.microsoft.com/services/key-vault/)
* [Microsoft Azure:Key Vault 檔](/azure/key-vault/)
* [如何為 Azure Key Vault 產生及傳輸受 HSM 保護的金鑰](/azure/key-vault/key-vault-hsm-protected-keys)
* [KeyVaultClient 類別](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
* [快速入門：使用 .NET web 應用程式從 Azure Key Vault 設定和取出秘密](/azure/key-vault/quick-create-net)
* [教學課程：如何在 .NET 中搭配使用 Azure Key Vault 與 Azure Windows 虛擬機器](/azure/key-vault/tutorial-net-windows-virtual-machine)
