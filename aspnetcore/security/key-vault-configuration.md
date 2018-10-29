---
title: ASP.NET Core 中的 azure Key Vault 組態提供者
author: guardrex
description: 了解如何使用 Azure 金鑰保存庫的組態提供者設定應用程式使用在執行階段載入的名稱 / 值組。
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: security/key-vault-configuration
ms.openlocfilehash: c6dc8b9c462841351b3ada72deeae727da356a6c
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207884"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a>ASP.NET Core 中的 azure Key Vault 組態提供者

藉由[Luke Latham](https://github.com/guardrex)和[Andrew Stanton-nurse](https://github.com/anurse)

本文件說明如何使用[Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/)組態提供者，從 Azure Key Vault 祕密載入應用程式組態值。 Azure Key Vault 是雲端式服務，可協助您保護密碼編譯金鑰和應用程式和服務所使用的密碼。 常見的案例包括控制存取敏感的組態資料，並符合需求的 FIPS 140-2 Level 2 驗證的硬體安全性模組 (HSM) 儲存組態資料時。 這項功能是適用於 ASP.NET Core 1.1 為目標的應用程式或更高版本。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="package"></a>Package

若要使用的提供者，將參考加入[Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/)封裝。

## <a name="app-configuration"></a>應用程式設定

您可以瀏覽提供者[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples)。 一旦您建立金鑰保存庫，並建立祕密，保存庫中，範例應用程式安全地載入其組態中的祕密的值，並在網頁中顯示它們。

提供者新增至應用程式的組態`AddAzureKeyVault`延伸模組。 在範例應用程式擴充功能會使用從載入的三個組態值*appsettings.json*檔案。

| 應用程式設定    | 描述                    | 範例                                      |
| -------------- | ------------------------------ | -------------------------------------------- |
| `Vault`        | Azure 金鑰保存庫名稱           | contosovault                                 |
| `ClientId`     | Azure Active Directory 應用程式識別碼  | 627e911e-43cc-61d4-992e-12db9c81b413         |
| `ClientSecret` | Azure Active Directory 應用程式金鑰 | g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di= |

[!code-csharp[Program](key-vault-configuration/samples/basic-sample/2.x/Program.cs?name=snippet1)]

## <a name="create-key-vault-secrets-and-load-configuration-values-basic-sample"></a>建立金鑰保存庫祕密，並載入組態值 （基本範例）

1. 建立金鑰保存庫，並設定 Azure Active Directory (Azure AD) 應用程式中的指導方針[開始使用 Azure Key Vault](https://azure.microsoft.com/documentation/articles/key-vault-get-started/)。
   * 將密碼新增至金鑰保存庫使用[AzureRM 金鑰保存庫 PowerShell 模組](/powershell/module/azurerm.keyvault)苀[PowerShell 資源庫](https://www.powershellgallery.com/packages/AzureRM.KeyVault)，則[Azure Key Vault REST API](/rest/api/keyvault/)，或[Azure 入口網站](https://portal.azure.com/)。 為建立祕密*手動*或是*憑證*祕密。 *憑證*機密資料是應用程式和服務所使用的憑證，但不是支援的組態提供者。 您應該使用*手動*選項建立的組態提供者使用的名稱 / 值組祕密。
     * 簡單的密碼會建立為名稱 / 值組。 Azure Key Vault 祕密名稱必須是限制為英數字元及虛線。
     * 階層式的值 （組態區段） 使用`--`（兩個連字號） 做為分隔符號，在此範例中。 通常用來分隔的區段中的子機碼的冒號[ASP.NET Core 組態](xref:fundamentals/configuration/index)，祕密名稱中不允許。 因此，兩個連字號是用，而且密碼會載入應用程式的設定時，已還原為冒號。
     * 建立兩個*手動*具有下列名稱 / 值組的密碼。 第一個密碼是簡單名稱和值，和第二個祕密建立祕密的值與區段和祕密名稱中的子機碼：
       * `SecretName`: `secret_value_1`
       * `Section--SecretName`: `secret_value_2`
   * 向 Azure Active Directory 中註冊範例應用程式。
   * 授權應用程式存取金鑰保存庫。 當您使用`Set-AzureRmKeyVaultAccessPolicy`PowerShell cmdlet，授權應用程式存取金鑰保存庫中，提供`List`並`Get`祕密存取`-PermissionsToSecrets list,get`。

2. 應用程式的更新*appsettings.json*的值的檔案`Vault`， `ClientId`，和`ClientSecret`。
3. 執行範例應用程式，它會取得從其組態值`IConfigurationRoot`祕密名稱同名。
   * 非階層式的值： 值`SecretName`取得`config["SecretName"]`。
   * （區段） 的階層式值： 使用`:`（冒號） 標記法或`GetSection`擴充方法。 您可以使用任一種方法來取得組態值：
     * `config["Section:SecretName"]`
     * `config.GetSection("Section")["SecretName"]`

當您執行應用程式時，網頁會顯示載入祕密的值：

![透過 「 Azure 金鑰保存庫組態提供者的瀏覽器視窗中顯示祕密值載入](key-vault-configuration/_static/sample1.png)

## <a name="bind-an-array-to-a-class"></a>將陣列繫結到類別

提供者可以讀入繫結到 POCO 陣列的陣列中的組態值。

從允許包含冒號的索引鍵的組態來源讀取時 (`:`) 的數值索引鍵的區段分隔符號用來區別構成陣列的索引鍵 (`:0:`， `:1:`，... `:{n}:`)。 如需詳細資訊，請參閱 <<c0> [ 組態： 將陣列繫結至類別](xref:fundamentals/configuration/index#bind-an-array-to-a-class)。

Azure Key Vault 金鑰無法使用冒號做為分隔符號。 本主題中所述的方法會使用雙連字號 (`--`) 當作分隔符號 （區段） 的階層式值。 陣列索引鍵時，會儲存在 Azure 金鑰保存庫上，使用雙連字號和數值的重要片段 (`--0--`， `--1--`，... `--{n}--`)。

檢查下列[Serilog](https://serilog.net/)記錄的 JSON 檔案所提供的提供者組態。 有兩個物件中定義的常值`WriteTo`陣列，其中會反映兩個 Serilog*接收*，可描述記錄輸出的目的地：

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

在上述的 JSON 檔案中所示的組態會儲存在 Azure 金鑰保存庫使用雙破折號 (`--`) 標記法和數值的區段：

| Key | 值 |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="create-prefixed-key-vault-secrets-and-load-configuration-values-key-name-prefix-sample"></a>建立帶有前置詞的金鑰保存庫祕密，並載入組態值 （索引鍵-名稱-前置詞的範例）

`AddAzureKeyVault` 也會提供多載可接受的實作`IKeyVaultSecretManager`，這可讓您控制如何金鑰保存庫祕密會轉換成設定的索引鍵。 比方說，您可以實作的介面，將根據您在應用程式啟動時所提供的前置詞值的祕密值。 這可讓您，比方說，若要載入的應用程式的版本為基礎的祕密。

> [!WARNING]
> 若要將多個應用程式的祕密放入相同的金鑰保存庫，或將環境的密碼，請勿使用前置詞上的金鑰保存庫祕密 (例如*開發*與*生產*祕密) 置於同一個保存庫。 我們建議，不同的應用程式和開發/生產環境使用不同的金鑰保存庫來隔離應用程式環境，以最高層級的安全性。

您使用第二個範例應用程式中的金鑰保存庫來建立祕密`5000-AppSecret`（期間不允許在金鑰保存庫祕密名稱） 代表您的應用程式的 version=5.0.0.0 新版應用程式祕密。 如需另一個版本，5.1.0.0，您建立的密碼`5100-AppSecret`。 每個應用程式版本會將它自己的祕密值載入其設定為`AppSecret`、 清除關閉版本載入祕密。 範例的實作如下所示：

[!code-csharp[Configuration builder](key-vault-configuration/samples/key-name-prefix-sample/2.x/Program.cs?name=snippet1&highlight=18)]

[!code-csharp[PrefixKeyVaultSecretManager](key-vault-configuration/samples/key-name-prefix-sample/2.x/Startup.cs?name=snippet1)]

`Load`逐一查看，找出有版本前置詞的保存庫祕密提供者演算法所呼叫方法。 當版本的前置詞上有`Load`，此演算法會使用`GetKey`方法傳回的密碼名稱的組態名稱。 它會除去版本前置詞，從 祕密的名稱，並傳回載入的祕密名稱的其餘部分，將應用程式設定成名稱 / 值組。

當您實作這種方法：

1. 載入的金鑰保存庫祕密。
2. 字串密碼`5000-AppSecret`會比對。
3. 版本中， `5000` （使用 dash)，移除離開的索引鍵名稱從`AppSecret`祕密值載入應用程式的設定。

> [!NOTE]
> 您也可以提供您自己`KeyVaultClient`實作`AddAzureKeyVault`。 提供自訂的用戶端，可讓您共用的用戶端的組態提供者與您的應用程式的其他部分之間的單一執行個體。

1. 建立金鑰保存庫，並設定 Azure Active Directory (Azure AD) 應用程式中的指導方針[開始使用 Azure Key Vault](https://azure.microsoft.com/documentation/articles/key-vault-get-started/)。
   * 將密碼新增至金鑰保存庫使用[AzureRM 金鑰保存庫 PowerShell 模組](/powershell/module/azurerm.keyvault)苀[PowerShell 資源庫](https://www.powershellgallery.com/packages/AzureRM.KeyVault)，則[Azure Key Vault REST API](/rest/api/keyvault/)，或[Azure 入口網站](https://portal.azure.com/)。 為建立祕密*手動*或是*憑證*祕密。 *憑證*機密資料是應用程式和服務所使用的憑證，但不是支援的組態提供者。 您應該使用*手動*選項建立的組態提供者使用的名稱 / 值組祕密。
     * 階層式的值 （組態區段） 使用`--`（兩個連字號） 做為分隔符號。
     * 建立兩個*手動*具有下列名稱 / 值組的祕密：
       * `5000-AppSecret`: `5.0.0.0_secret_value`
       * `5100-AppSecret`: `5.1.0.0_secret_value`
   * 向 Azure Active Directory 中註冊範例應用程式。
   * 授權應用程式存取金鑰保存庫。 當您使用`Set-AzureRmKeyVaultAccessPolicy`PowerShell cmdlet，授權應用程式存取金鑰保存庫中，提供`List`並`Get`祕密存取`-PermissionsToSecrets list,get`。

2. 應用程式的更新*appsettings.json*的值的檔案`Vault`， `ClientId`，和`ClientSecret`。
3. 執行範例應用程式，它會取得從其組態值`IConfigurationRoot`前置詞的祕密名稱同名。 在此範例中，前置詞是應用程式的版本，您提供給`PrefixKeyVaultSecretManager`當您加入的 Azure Key Vault 組態提供者。 值`AppSecret`取得`config["AppSecret"]`。 應用程式所產生的網頁顯示載入的值：

   ![瀏覽器視窗中顯示應用程式的版本時 version=5.0.0.0，透過 「 Azure 金鑰保存庫組態提供者載入祕密值](key-vault-configuration/_static/sample2-1.png)

4. 變更應用程式中的組件的專案檔版本`5.0.0.0`至`5.1.0.0`並再次執行應用程式。 這次，則傳回的祕密值是`5.1.0.0_secret_value`。 應用程式所產生的網頁顯示載入的值：

   ![瀏覽器視窗中顯示 5.1.0.0 應用程式的版本時，透過 「 Azure 金鑰保存庫組態提供者載入祕密值](key-vault-configuration/_static/sample2-2.png)

## <a name="control-access-to-the-clientsecret"></a>ClientSecret 控制存取

使用[Secret Manager 工具](xref:security/app-secrets)維護`ClientSecret`外部專案來源樹狀結構。 使用 Secret Manager 中，您將應用程式祕密與特定的專案產生關聯並加以共用跨多個專案。

在開發環境可支援憑證中的.NET Framework 應用程式時，您可以使用 X.509 憑證來驗證 Azure Key Vault。 X.509 憑證的私密金鑰是由 OS 管理。 如需詳細資訊，請參閱 <<c0> [ 使用而不是用戶端祕密憑證進行驗證](/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret)。 使用`AddAzureKeyVault`多載，接受`X509Certificate2`(`_env`在下列範例中：

```csharp
var builtConfig = config.Build();

var store = new X509Store(StoreLocation.CurrentUser);
store.Open(OpenFlags.ReadOnly);
var cert = store.Certificates
    .Find(X509FindType.FindByThumbprint, 
        config["CertificateThumbprint"], false);

config.AddAzureKeyVault(
    builtConfig["Vault"],
    builtConfig["ClientId"],
    cert.OfType<X509Certificate2>().Single(),
    new EnvironmentSecretManager(context.HostingEnvironment.ApplicationName));

store.Close();
```

## <a name="reload-secrets"></a>重新載入祕密

密碼會快取直到`IConfigurationRoot.Reload()`呼叫。 過期，停用，以及更新金鑰保存庫中的祕密未遵守應用程式，直到`Reload`執行。

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a>已停用和到期的祕密

已停用及過期的密碼會擲回`KeyVaultClientException`。 若要防止您的應用程式擲回，取代您的應用程式或更新已停用/過期的祕密。

## <a name="troubleshoot"></a>疑難排解

當應用程式無法載入組態使用的提供者時，錯誤訊息會寫入[ASP.NET Core 記錄基礎結構](xref:fundamentals/logging/index)。 在下列情況會造成無法載入組態：

* 應用程式未正確設定 Azure Active Directory 中。
* 金鑰保存庫不存在於 Azure 金鑰保存庫。
* 應用程式未獲授權存取金鑰保存庫。
* 存取原則不包含`Get`和`List`權限。
* 在金鑰保存庫中，設定資料 （名稱 / 值組） 錯誤命名為，遺漏，停用，或已過期。
* 應用程式有錯誤的金鑰保存庫名稱 (`Vault`)，Azure AD 應用程式識別碼 (`ClientId`)，或 Azure AD 金鑰 (`ClientSecret`)。
* Azure AD 金鑰 (`ClientSecret`) 已過期。
* 不正確的值，您想要載入的應用程式中設定金鑰 （名稱）。

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/configuration/index>
* [Microsoft Azure： 金鑰保存庫](https://azure.microsoft.com/services/key-vault/)
* [Microsoft Azure: Key Vault 文件](/azure/key-vault/)
* [如何產生並傳輸受 HSM 保護金鑰的 Azure 金鑰保存庫](/azure/key-vault/key-vault-hsm-protected-keys)
* [KeyVaultClient 類別](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
