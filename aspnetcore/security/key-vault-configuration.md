---
title: "Azure 金鑰保存庫的組態提供者"
author: guardrex
description: "了解如何使用 Azure 金鑰保存庫的組態提供者設定應用程式使用在執行階段載入的名稱 / 值組。"
ms.author: riande
manager: wpickett
ms.date: 08/09/2017
ms.topic: article
ms.prod: asp.net-core
uid: security/key-vault-configuration
ms.openlocfilehash: 25c7d38a27741c9877538673425c5a9dceccac93
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="azure-key-vault-configuration-provider"></a>Azure 金鑰保存庫的組態提供者

由[Luke Latham](https://github.com/guardrex)和[Andrew Stanton 護士](https://github.com/anurse)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

檢視或下載 2.x 的範例程式碼：

* [基本範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/basic-sample/2.x)([如何下載](xref:tutorials/index#how-to-download-a-sample))-讀取到應用程式的密碼值。
* [索引鍵名稱前置詞範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/key-name-prefix-sample/2.x)([如何下載](xref:tutorials/index#how-to-download-a-sample))-讀取密碼的值，以代表版本的應用程式，這可讓您載入一組不同的每個應用程式版本的密碼值的索引鍵名稱前置詞。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

檢視或下載 1.x 的範例程式碼：

* [基本範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/basic-sample/1.x)([如何下載](xref:tutorials/index#how-to-download-a-sample))-讀取到應用程式的密碼值。
* [索引鍵名稱前置詞範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/key-name-prefix-sample/1.x)([如何下載](xref:tutorials/index#how-to-download-a-sample))-讀取密碼的值，以代表版本的應用程式，這可讓您載入一組不同的每個應用程式版本的密碼值的索引鍵名稱前置詞。 

---

本文件說明如何使用[Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/)從 Azure 金鑰保存庫密碼載入應用程式組態值的組態提供者。 Azure 金鑰保存庫是以雲端為基礎的服務，可協助您保護密碼編譯金鑰和應用程式和服務所使用的密碼。 常見的案例包括敏感的組態資料控制存取，但符合 FIPS 140-2 的需求層級 2 硬體安全性模組 (HSM) 時驗證儲存設定資料。 這項功能是適用於 ASP.NET Core 1.1 版為目標的應用程式或更高版本。

## <a name="package"></a>Package
若要使用的提供者，將參考加入[Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/)封裝。

## <a name="application-configuration"></a>應用程式組態
您可以瀏覽的提供者[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples)。 一旦您建立金鑰保存庫，並在保存庫中建立密碼，範例應用程式安全地載入其組態中密碼的值，並在網頁中顯示。

提供者加入至`ConfigurationBuilder`與`AddAzureKeyVault`延伸模組。 範例應用程式中延伸模組會使用三個從載入的組態值*appsettings.json*檔案。

| 應用程式設定    | 描述                    | 範例                                      |
| -------------- | ------------------------------ | -------------------------------------------- |
| `Vault`        | Azure 金鑰保存庫名稱           | contosovault                                 |
| `ClientId`     | Azure Active Directory App Id  | 627e911e-43cc-61d4-992e-12db9c81b413         |
| `ClientSecret` | Azure Active Directory App Key | g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di= |

[!code-csharp[Program](key-vault-configuration/samples/basic-sample/2.x/Program.cs?name=snippet1&highlight=2,7-10)]

## <a name="creating-key-vault-secrets-and-loading-configuration-values-basic-sample"></a>建立金鑰保存庫密碼並載入組態值 （basic 範例）
1. 建立金鑰保存庫，並設定下列中的指導方針的應用程式的 Azure Active Directory (Azure AD)[開始使用 Azure 金鑰保存庫](https://azure.microsoft.com/documentation/articles/key-vault-get-started/)。
  * 將密碼加入金鑰保存庫使用[AzureRM 金鑰保存庫 PowerShell 模組](/powershell/module/azurerm.keyvault)可從[PowerShell 資源庫](https://www.powershellgallery.com/packages/AzureRM.KeyVault)、 [Azure 金鑰保存庫 REST API](/rest/api/keyvault/)，或[Azure 入口網站](https://portal.azure.com/)。 密碼會建立為*手動*或*憑證*機密資料。 *憑證*密碼使用的應用程式和服務憑證，但不是支援的組態提供者。 您應該使用*手動*選項建立的組態提供者使用的名稱 / 值組密碼。
    * 簡單密碼，會建立為名稱 / 值組。 Azure 金鑰保存庫密碼名稱會限制為英數字元及虛線。
    * 使用階層式的值 （組態區段） `--` （兩個破折號） 做為範例中的分隔符號。 通常用來分隔區段中的，從子機碼中的冒號[ASP.NET Core 組態](xref:fundamentals/configuration/index)，秘密名稱中不允許。 因此，使用兩個破折號和機密資料載入至應用程式的組態時，已還原為冒號。
    * 建立兩個*手動*具有下列名稱 / 值組的密碼。 第一個密碼是簡單名稱和值，並以第二個密碼建立祕密值區段和秘密名稱中的子機碼：
      * `SecretName`: `secret_value_1`
      * `Section--SecretName`: `secret_value_2`
  * 向 Azure Active Directory 中註冊範例應用程式。
  * 授權應用程式存取金鑰保存庫。 當您使用`Set-AzureRmKeyVaultAccessPolicy`PowerShell cmdlet，以授權應用程式存取金鑰保存庫中，提供`List`和`Get`密碼與存取`-PermissionsToSecrets list,get`。
2. 更新應用程式的*appsettings.json*檔案使用的數值`Vault`， `ClientId`，和`ClientSecret`。
3. 執行範例應用程式，會取得從其組態值`IConfigurationRoot`具有相同名稱與密碼的名稱。
  * 非階層式的值： 的值`SecretName`取得`config["SecretName"]`。
  * 階層值 （區段）： 使用`:`（冒號） 標記法或`GetSection`擴充方法。 您可以使用任一種方法來取得組態值：
    * `config["Section:SecretName"]`
    * `config.GetSection("Section")["SecretName"]`

當您執行應用程式時，網頁就會顯示載入的祕密值：

![透過 Azure 金鑰保存庫的組態提供者載入瀏覽器視窗中顯示密碼的值](key-vault-configuration/_static/sample1.png)

## <a name="creating-prefixed-key-vault-secrets-and-loading-configuration-values-key-name-prefix-sample"></a>建立帶有前置詞的金鑰保存庫密碼並載入組態值 （索引鍵-名稱-前置詞的範例）
`AddAzureKeyVault`也會提供多載，可接受的實作`IKeyVaultSecretManager`，可讓您控制如何金鑰保存庫密碼會轉換成組態機碼。 例如，您可以實作的介面，根據您在應用程式啟動時提供的前置詞值的密碼值載入。 這可讓您，例如，若要載入的應用程式版本為基礎的密碼。

> [!WARNING]
> 將多個應用程式的秘密資訊放入相同的金鑰保存庫，或將環境的機密資料，不使用前置詞在金鑰保存庫密碼 (例如，*開發*verus*生產*密碼) 放在同一個保存庫。 我們建議不同的應用程式和開發/生產環境使用不同的金鑰保存庫來隔離應用程式環境，最高的層級的安全性。

使用第二個範例應用程式，您會建立金鑰保存庫，以在執行密碼`5000-AppSecret`（期間不允許在金鑰保存庫密碼名稱） 代表應用程式的版本 5.0.0.0 應用程式密碼。 如需其他版本，5.1.0.0，您建立的密碼`5100-AppSecret`。 每個應用程式版本會將其設定為載入它自己的密碼值`AppSecret`、 清除關閉版本載入密碼。 範例的實作如下所示：

[!code-csharp[Configuration builder](key-vault-configuration/samples/key-name-prefix-sample/2.x/Program.cs?name=snippet1&highlight=12)]

[!code-csharp[PrefixKeyVaultSecretManager](key-vault-configuration/samples/key-name-prefix-sample/2.x/Startup.cs?name=snippet1)]

`Load`逐一，找出有版本前置詞的保存庫密碼提供者演算法所呼叫方法。 在找到版本前置詞與`Load`，此演算法會使用`GetKey`方法傳回之組態名稱的密碼的名稱。 它會剝版本前置詞之密碼的名稱，並傳回至應用程式的組態名稱 / 值組的載入的秘密名稱的其餘部分。

當您實作這個方法：

1. 會載入金鑰保存庫密碼。
2. 字串密碼`5000-AppSecret`時要比對。
3. 版本`5000`（與虛線），移除不保留索引鍵名稱`AppSecret`祕密值與載入應用程式的組態。

> [!NOTE]
> 您也可以提供您自己`KeyVaultClient`實作`AddAzureKeyVault`。 提供自訂用戶端，可讓您共用的組態提供者與您的應用程式的其他部分之間的用戶端的單一執行個體。

1. 建立金鑰保存庫，並設定下列中的指導方針的應用程式的 Azure Active Directory (Azure AD)[開始使用 Azure 金鑰保存庫](https://azure.microsoft.com/documentation/articles/key-vault-get-started/)。
  * 將密碼加入金鑰保存庫使用[AzureRM 金鑰保存庫 PowerShell 模組](/powershell/module/azurerm.keyvault)可從[PowerShell 資源庫](https://www.powershellgallery.com/packages/AzureRM.KeyVault)、 [Azure 金鑰保存庫 REST API](/rest/api/keyvault/)，或[Azure 入口網站](https://portal.azure.com/)。 密碼會建立為*手動*或*憑證*機密資料。 *憑證*密碼使用的應用程式和服務憑證，但不是支援的組態提供者。 您應該使用*手動*選項建立的組態提供者使用的名稱 / 值組密碼。
    * 使用階層式的值 （組態區段） `--` （兩個破折號） 做為分隔符號。
    * 建立兩個*手動*具有下列名稱 / 值組的密碼：
      * `5000-AppSecret`: `5.0.0.0_secret_value`
      * `5100-AppSecret`: `5.1.0.0_secret_value`
  * 向 Azure Active Directory 中註冊範例應用程式。
  * 授權應用程式存取金鑰保存庫。 當您使用`Set-AzureRmKeyVaultAccessPolicy`PowerShell cmdlet，以授權應用程式存取金鑰保存庫中，提供`List`和`Get`密碼與存取`-PermissionsToSecrets list,get`。
2. 更新應用程式的*appsettings.json*檔案使用的數值`Vault`， `ClientId`，和`ClientSecret`。
3. 執行範例應用程式，會取得從其組態值`IConfigurationRoot`具有相同名稱做為帶有前置詞的秘密名稱。 在此範例中，前置詞是應用程式的版本，您提供給`PrefixKeyVaultSecretManager`當您加入的 Azure 金鑰保存庫的組態提供者。 值`AppSecret`取得`config["AppSecret"]`。 應用程式所產生的網頁顯示載入的值：

   ![透過 Azure 金鑰保存庫的組態提供者應用程式的版本時 5.0.0.0 載入的瀏覽器視窗中顯示的密碼值](key-vault-configuration/_static/sample2-1.png)

4. 變更應用程式中的組件的專案檔版本`5.0.0.0`至`5.1.0.0`，然後再次執行應用程式。 此時，傳回的密碼值是`5.1.0.0_secret_value`。 應用程式所產生的網頁顯示載入的值：

   ![透過 Azure 金鑰保存庫的組態提供者應用程式的版本時 5.1.0.0 載入的瀏覽器視窗中顯示的密碼值](key-vault-configuration/_static/sample2-2.png)

## <a name="controlling-access-to-the-clientsecret"></a>ClientSecret 控制的存取
使用[密碼管理員工具](xref:security/app-secrets)維護`ClientSecret`專案原始檔樹狀之外。 使用密碼管理員，您將應用程式密碼與特定的專案產生關聯並共用跨多個專案。

當開發.NET Framework 應用程式支援憑證的環境中，您可以使用 X.509 憑證來驗證 Azure 金鑰保存庫。 X.509 憑證的私密金鑰是由作業系統負責管理。 如需詳細資訊，請參閱[驗證的憑證，而不是用戶端密碼](https://docs.microsoft.com/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret)。 使用`AddAzureKeyVault`多載會接受`X509Certificate2`。

```csharp
var store = new X509Store(StoreLocation.CurrentUser);
store.Open(OpenFlags.ReadOnly);
var cert = store.Certificates.Find(X509FindType.FindByThumbprint, config["CertificateThumbprint"], false);

builder.AddAzureKeyVault(
    config["Vault"],
    config["ClientId"],
    cert.OfType<X509Certificate2>().Single(),
    new EnvironmentSecretManager(env.ApplicationName));
store.Close();

Configuration = builder.Build();
```

## <a name="reloading-secrets"></a>重新載入密碼
密碼會快取直到`IConfigurationRoot.Reload()`呼叫。 過期時，停用，而金鑰保存庫中的更新的密碼不會代表應用程式，直到`Reload`執行。

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a>已停用及過期的密碼
已停用及過期的密碼會擲回`KeyVaultClientException`。 若要防止您的應用程式擲回，取代您的應用程式或更新已停用/過期的密碼。

## <a name="troubleshooting"></a>疑難排解
當應用程式無法載入組態使用的提供者時，錯誤訊息會寫入[ASP.NET 記錄基礎結構](xref:fundamentals/logging/index)。 在下列情況會造成無法載入組態：
* 應用程式未正確設定 Azure Active Directory 中。
* 金鑰保存庫不存在於 Azure 金鑰保存庫。
* 應用程式未獲授權存取金鑰保存庫。
* 存取原則不包括`Get`和`List`權限。
* 在金鑰保存庫中，設定資料 （名稱 / 值組） 是正確命名，遺失，停用，或已過期。
* 應用程式有錯誤的金鑰保存庫名稱 (`Vault`)，Azure AD 應用程式識別碼 (`ClientId`)，或 Azure AD 的索引鍵 (`ClientSecret`)。
* Azure AD 金鑰 (`ClientSecret`) 已過期。
* 設定索引鍵 （名稱） 不正確的值，您想要載入的應用程式中。

## <a name="additional-resources"></a>其他資源
* [組態](xref:fundamentals/configuration/index)
* [Microsoft Azure： 金鑰保存庫](https://azure.microsoft.com/services/key-vault/)
* [Microsoft Azure： 金鑰保存庫文件](https://docs.microsoft.com/azure/key-vault/)
* [如何產生並傳輸受 HSM 保護金鑰的 Azure 金鑰保存庫](https://docs.microsoft.com/azure/key-vault/key-vault-hsm-protected-keys)
* [KeyVaultClient 類別](https://docs.microsoft.com/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
