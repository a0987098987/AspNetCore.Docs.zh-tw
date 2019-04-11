---
title: ASP.NET Core 中的 azure Key Vault 組態提供者
author: guardrex
description: 了解如何使用 Azure 金鑰保存庫的組態提供者設定應用程式使用在執行階段載入的名稱 / 值組。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/25/2019
uid: security/key-vault-configuration
ms.openlocfilehash: 8fd1cca1803d3f1d44d80ec63c5cfc259cbdaf55
ms.sourcegitcommit: 1a7000630e55da90da19b284e1b2f2f13a393d74
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/04/2019
ms.locfileid: "59012691"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a>ASP.NET Core 中的 azure Key Vault 組態提供者

藉由[Luke Latham](https://github.com/guardrex)和[Andrew Stanton-nurse](https://github.com/anurse)

本文件說明如何使用[Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/)組態提供者，從 Azure Key Vault 祕密載入應用程式組態值。 Azure Key Vault 是雲端式服務，可以協助使用者保護密碼編譯金鑰和應用程式和服務所使用的密碼。 ASP.NET Core 應用程式使用 Azure 金鑰保存庫的常見案例包括：

* 控制對敏感的組態資料的存取。
* 儲存組態資料時，符合 fips 需求 140-2 Level 2 驗證的硬體安全性模組 (HSM)。

此案例是適用於應用程式為目標的 ASP.NET Core 2.1 或更新版本。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="packages"></a>封裝

若要使用 Azure 金鑰保存庫的組態提供者，將新增的套件參考[Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/)封裝。

採用[管理 Azure 資源的身分識別](/azure/active-directory/managed-identities-azure-resources/overview)案例中，新增的套件參考[Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/)封裝。

> [!NOTE]
> 在本文撰寫之際，最新穩定版本`Microsoft.Azure.Services.AppAuthentication`，版本`1.0.3`，可讓您[系統指派給受控身分識別](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-worka-namehow-does-it-worka)。 支援*指派使用者給受控身分識別*位於`1.2.0-preview2`封裝。 本主題示範如何使用系統管理的身分識別，並提供的範例應用程式使用的版本`1.0.3`的`Microsoft.Azure.Services.AppAuthentication`封裝。

## <a name="sample-app"></a>範例應用程式

範例應用程式執行所決定的兩種模式之一`#define`陳述式，在頂端*Program.cs*檔案：

* `Certificate` &ndash; 示範如何使用 Azure Key Vault 中儲存的存取祕密的 Azure 金鑰保存庫用戶端識別碼和 X.509 憑證。 從任何位置，部署至 Azure App Service 或任何能夠為 ASP.NET Core 應用程式的主機，可以執行此版本的範例。
* `Managed` &ndash; 示範如何使用[管理 Azure 資源的身分識別](/azure/active-directory/managed-identities-azure-resources/overview)驗證的應用程式至 Azure Key Vault 與 Azure AD 驗證不含認證儲存在應用程式的程式碼或組態。 當使用受管理的身分識別進行驗證，不需要的 Azure AD 應用程式識別碼和密碼 （用戶端祕密）。 `Managed`範例版本必須部署至 Azure。 請依照下列中的指導方針[適用於 Azure 資源使用受控身分識別](#use-managed-identities-for-azure-resources)一節。

如需有關如何設定範例應用程式使用前置處理器指示詞 (`#define`)，請參閱<xref:index#preprocessor-directives-in-sample-code>。

## <a name="secret-storage-in-the-development-environment"></a>在開發環境中的祕密儲存體

設定在本機使用的祕密[Secret Manager 工具](xref:security/app-secrets)。 當在開發環境中本機電腦上，執行範例應用程式時，會載入密碼從本機的祕密管理員存放區。

Secret Manager 工具需要`<UserSecretsId>`應用程式的專案檔中的屬性。 設定屬性值 (`{GUID}`) 任何唯一的 guid:

```xml
<PropertyGroup>
  <UserSecretsId>{GUID}</UserSecretsId>
</PropertyGroup>
```

密碼會建立為名稱 / 值組。 階層式的值 （組態區段） 會使用`:`（冒號） 為分隔符號[ASP.NET Core 組態](xref:fundamentals/configuration/index)索引鍵名稱。

Secret Manager 可從命令殼層，開啟專案的內容根目錄，其中`{SECRET NAME}`名稱和`{SECRET VALUE}`的值：

```console
dotnet user-secrets set "{SECRET NAME}" "{SECRET VALUE}"
```

從專案的內容根目錄設定範例應用程式祕密命令殼層中執行下列命令：

```console
dotnet user-secrets set "SecretName" "secret_value_1_dev"
dotnet user-secrets set "Section:SecretName" "secret_value_2_dev"
```

當這些祕密儲存在 Azure Key Vault[生產環境使用 Azure Key Vault 中密碼的儲存體](#secret-storage-in-the-production-environment-with-azure-key-vault)區段中，`_dev`後置詞變更為`_prod`。 後置詞提供視覺提示，指出組態值的來源的應用程式的輸出中。

## <a name="secret-storage-in-the-production-environment-with-azure-key-vault"></a>在生產環境使用 Azure Key Vault 的祕密儲存體

所提供的指示[快速入門：設定並從使用 Azure CLI 的 Azure 金鑰保存庫擷取祕密](/azure/key-vault/quick-create-cli)主題會彙總以供建立 Azure Key Vault 及儲存範例應用程式所使用的密碼。 請參閱本主題以取得詳細資料。

1. 開啟 Azure Cloud shell 中使用中的下列方法的任一[Azure 入口網站](https://portal.azure.com/):

   * 選取 **試試**的程式碼區塊右上角。 使用搜尋字串"Azure CLI 」，在文字方塊中。
   * 在您的瀏覽器中開啟 Cloud Shell**啟動 Cloud Shell**  按鈕。
   * 選取  **Cloud Shell**在 Azure 入口網站右上角的功能表上的按鈕。

   如需詳細資訊，請參閱 < [Azure 命令列介面 (CLI)](/cli/azure/)並[Azure Cloud Shell 概觀](/azure/cloud-shell/overview)。

1. 如果您不已通過驗證，登入的`az login`命令。

1. 建立資源群組，使用下列命令，其中`{RESOURCE GROUP NAME}`是新的資源群組的資源群組名稱和`{LOCATION}`是 Azure 區域 （資料中心）：

   ```console
   az group create --name "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. 在資源群組中，使用下列命令，建立金鑰保存庫所在`{KEY VAULT NAME}`是新的金鑰保存庫的名稱和`{LOCATION}`是 Azure 區域 （資料中心）：

   ```console
   az keyvault create --name "{KEY VAULT NAME}" --resource-group "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. 建立金鑰保存庫中的密碼，做為名稱 / 值組。

   Azure Key Vault 祕密名稱必須是限制為英數字元及虛線。 階層式的值 （組態區段） 使用`--`（兩個連字號） 做為分隔符號。 通常用來分隔的區段中的子機碼的冒號[ASP.NET Core 組態](xref:fundamentals/configuration/index)，不允許在金鑰保存庫祕密名稱。 因此，兩個連字號是用，而且密碼會載入應用程式的設定時，已還原為冒號。

   下列的機密資料是範例應用程式搭配使用。 值包括`_prod`後置詞來區別從`_dev`後置詞從使用者的機密資訊的開發環境中載入的值。 取代`{KEY VAULT NAME}`與您在先前步驟中建立的金鑰保存庫名稱：

   ```console
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "SecretName" --value "secret_value_1_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "Section--SecretName" --value "secret_value_2_prod"
   ```

## <a name="use-application-id-and-client-secret-for-non-azure-hosted-apps"></a>使用非 Azure 代管應用程式的應用程式識別碼和用戶端祕密

設定 Azure AD、 Azure 金鑰保存庫和應用程式使用 Azure Active Directory 應用程式識別碼和 X.509 憑證來向 key vault**當應用程式裝載在 Azure 之外**。 如需詳細資訊，請參閱 <<c0> [ 關於金鑰、 祕密和憑證](/azure/key-vault/about-keys-secrets-and-certificates)。

> [!NOTE]
> 雖然在 Azure 中託管的應用程式支援使用應用程式識別碼和 X.509 憑證，但我們建議您使用[管理 Azure 資源的身分識別](#use-managed-identities-for-azure-resources)裝載在 Azure 中的應用程式時。 受管理的身分識別不需要將憑證儲存在應用程式或開發環境。

範例應用程式會使用的應用程式識別碼和 X.509 憑證的時機`#define`陳述式，在頂端*Program.cs*檔案設定為`Certificate`。

1. 使用 Azure AD 註冊應用程式 (**應用程式註冊**)。
1. 上傳的公開金鑰：
   1. 在 Azure AD 中選取的應用程式。
   1. 瀏覽至**設定** > **金鑰**。
   1. 選取 **上傳公開金鑰**來上傳包含公開金鑰的憑證。 除了使用 *.cer*， *.pem*，或 *.crt*憑證 *.pfx*可以上傳憑證。
1. 應用程式中儲存的金鑰保存庫名稱和應用程式識別碼*appsettings.json*檔案。 將憑證放置在應用程式或應用程式的憑證存放區中根&dagger;。
1. 瀏覽至**金鑰保存庫**在 Azure 入口網站中。
1. 選取您在建立金鑰保存庫[生產環境使用 Azure Key Vault 中密碼的儲存體](#secret-storage-in-the-production-environment-with-azure-key-vault)一節。
1. 選取 **存取原則**。
1. 選取 **新增**。
1. 選取 **選取主體**依名稱選取的已註冊的應用程式。 選取 [**選取**] 按鈕。
1. 開啟**祕密權限**，並提供應用程式與**取得**並**清單**權限。
1. 選取 [確定]。
1. 選取 [儲存]。
1. 部署應用程式。

&dagger;在範例應用程式中，憑證由直接從應用程式的根目錄中的實體的憑證檔案建立新`X509Certificate2`呼叫時`AddAzureKeyVault`。 另一個方法是允許 OS 管理的憑證。 如需詳細資訊，請參閱 <<c0> [ 允許 OS 管理的 X.509 憑證](#allow-the-os-to-manage-the-x509-certificate)一節。

`Certificate`範例應用程式取得其組態值從`IConfigurationRoot`具有相同名稱與祕密名稱：

* 非階層式的值：值`SecretName`取得`config["SecretName"]`。
* （區段） 的階層式值：使用`:`（冒號） 標記法或`GetSection`擴充方法。 您可以使用任一種方法來取得組態值：
  * `config["Section:SecretName"]`
  * `config.GetSection("Section")["SecretName"]`

應用程式呼叫`AddAzureKeyVault`所提供的值*appsettings.json*檔案：

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet1&highlight=12-15)]

範例值：

* 金鑰保存庫名稱： `contosovault`
* 應用程式識別碼： `627e911e-43cc-61d4-992e-12db9c81b413`

*appsettings.json*：

[!code-json[](key-vault-configuration/sample/appsettings.json)]

當您執行應用程式時，網頁就會顯示載入的祕密值。 使用，在開發環境中，載入祕密值`_dev`後置詞。 在生產環境中，值則是使用載入`_prod`後置詞。

## <a name="use-managed-identities-for-azure-resources"></a>適用於 Azure 資源的使用受管理身分識別

**應用程式部署至 Azure**可以充分善用[管理適用於 Azure 資源的身分識別](/azure/active-directory/managed-identities-azure-resources/overview)，可讓應用程式，以向 Azure Key Vault 使用不含認證的 Azure AD 驗證 (應用程式識別碼和Password/Client 密碼) 儲存在應用程式。

範例應用程式會使用適用於 Azure 資源的受管理身分識別時`#define`陳述式，在頂端*Program.cs*檔案設定為`Managed`。

輸入應用程式的保存庫名稱*appsettings.json*檔案。 範例應用程式不需要應用程式識別碼和密碼 （用戶端秘密），當設定為`Managed`版本，因此您可以忽略這些組態項目。 應用程式部署至 Azure，以及 Azure 驗證應用程式存取只使用 保存庫名稱儲存在 Azure Key Vault *appsettings.json*檔案。

將範例應用程式部署至 Azure App Service。

應用程式部署至 Azure App Service 與 Azure AD 建立服務時，會自動註冊。 從下列命令中使用的部署中取得的物件識別碼。 物件識別碼時，會顯示在 Azure 入口網站上，在**識別**App Service 的面板。

使用 Azure CLI 和應用程式的物件識別碼，提供應用程式與`list`和`get`存取金鑰保存庫的權限：

```console
az keyvault set-policy --name '{KEY VAULT NAME}' --object-id {OBJECT ID} --secret-permissions get list
```

**重新啟動應用程式**使用 Azure CLI、 PowerShell 或 Azure 入口網站。

範例應用程式：

* 建立的執行個體`AzureServiceTokenProvider`類別，而不需要的連接字串。 當未提供的連接字串時，提供者會嘗試從適用於 Azure 資源的受管理身分識別取得存取權杖。
* 新`KeyVaultClient`會透過`AzureServiceTokenProvider`執行個體權杖回呼。
* `KeyVaultClient`的預設實作會使用執行個體`IKeyVaultSecretManager`載入所有祕密的值，會取代雙連字號 (`--`) 加上冒號 (`:`) 中索引鍵名稱。

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet2&highlight=13-21)]

當您執行應用程式時，網頁就會顯示載入的祕密值。 在開發環境中，有祕密值`_dev`後置詞，因為它們由使用者祕密。 在生產環境中，值則是使用載入`_prod`後置詞，因為它們由 Azure 金鑰保存庫。

如果您收到`Access denied`錯誤，確認已向 Azure AD 註冊應用程式，且提供存取金鑰保存庫。 請確認您已重新開啟的服務在 Azure 中。

## <a name="use-a-key-name-prefix"></a>使用索引鍵名稱前置詞

`AddAzureKeyVault` 提供的多載，接受的實作`IKeyVaultSecretManager`，這可讓您控制如何金鑰保存庫祕密會轉換成設定的索引鍵。 比方說，您可以實作的介面，將根據您在應用程式啟動時所提供的前置詞值的祕密值。 這可讓您，比方說，若要載入的應用程式的版本為基礎的祕密。

> [!WARNING]
> 若要將多個應用程式的祕密放入相同的金鑰保存庫，或將環境的密碼，請勿使用前置詞上的金鑰保存庫祕密 (例如*開發*與*生產*祕密) 置於同一個保存庫。 我們建議，不同的應用程式和開發/生產環境使用不同的金鑰保存庫來隔離應用程式環境，以最高層級的安全性。

在下列範例中，祕密建立金鑰保存庫 」 （並使用 Secret Manager 工具開發環境） 的`5000-AppSecret`（期間不允許在金鑰保存庫祕密名稱）。 此祕密表示 version=5.0.0.0 新版應用程式的應用程式祕密。 應用程式，5.1.0.0，另一個版本的祕密加入至金鑰保存庫 （並使用 Secret Manager 工具） 的`5100-AppSecret`。 每個應用程式版本載入其組態設為其建立版本的祕密值`AppSecret`、 清除關閉版本載入祕密。

`AddAzureKeyVault` 自訂呼叫`IKeyVaultSecretManager`:

[!code-csharp[](key-vault-configuration/sample_snapshot/Program.cs?name=snippet1&highlight=22)]

金鑰保存庫名稱、 應用程式識別碼和密碼 （用戶端祕密） 的值由*appsettings.json*檔案：

[!code-json[](key-vault-configuration/sample/appsettings.json)]

範例值：

* 金鑰保存庫名稱： `contosovault`
* 應用程式識別碼： `627e911e-43cc-61d4-992e-12db9c81b413`
* 密碼: `g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di=`

`IKeyVaultSecretManager`實作回應組態中載入適當的祕密的祕密版本前置詞：

[!code-csharp[](key-vault-configuration/sample_snapshot/Startup.cs?name=snippet1)]

`Load`逐一查看，找出有版本前置詞的保存庫祕密提供者演算法所呼叫方法。 當版本的前置詞上有`Load`，此演算法會使用`GetKey`方法傳回的密碼名稱的組態名稱。 它會除去版本前置詞，從 祕密的名稱，並傳回載入的祕密名稱的其餘部分，將應用程式設定成名稱 / 值組。

當這個方法的實作方式：

1. 應用程式的專案檔中指定的應用程式的版本。 在下列範例中，應用程式的版本設為`5.0.0.0`:

   ```xml
   <PropertyGroup>
     <Version>5.0.0.0</Version>
   </PropertyGroup>
   ```

1. 確認`<UserSecretsId>`屬性會出現在應用程式的專案檔案，其中`{GUID}`是使用者所提供的 GUID:

   ```xml
   <PropertyGroup>
     <UserSecretsId>{GUID}</UserSecretsId>
   </PropertyGroup>
   ```

   儲存在本機使用下列的祕密[Secret Manager 工具](xref:security/app-secrets):

   ```console
   dotnet user-secrets set "5000-AppSecret" "5.0.0.0_secret_value_dev"
   dotnet user-secrets set "5100-AppSecret" "5.1.0.0_secret_value_dev"
   ```

1. 密碼會儲存在 Azure Key Vault 中使用下列 Azure CLI 命令：

   ```console
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5000-AppSecret" --value "5.0.0.0_secret_value_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5100-AppSecret" --value "5.1.0.0_secret_value_prod"
   ```

1. 執行應用程式時，會載入的金鑰保存庫祕密。 字串密碼`5000-AppSecret`會對應到應用程式的專案檔中指定的應用程式的版本 (`5.0.0.0`)。

1. 版本， `5000` （與 dash)，移除的索引鍵名稱。 在整個應用程式，讀取具有索引鍵的組態`AppSecret`載入祕密的值。

1. 如果應用程式的版本會變更專案檔案中`5.1.0.0`再次執行應用程式，則傳回的祕密值是`5.1.0.0_secret_value_dev`在開發環境和`5.1.0.0_secret_value_prod`在生產環境中。

> [!NOTE]
> 您也可以提供您自己`KeyVaultClient`實作`AddAzureKeyVault`。 自訂用戶端會允許跨應用程式共用用戶端的單一執行個體。

## <a name="allow-the-os-to-manage-the-x509-certificate"></a>允許 OS 管理的 X.509 憑證

X.509 憑證可以由 OS 管理。 下列範例會使用`AddAzureKeyVault`多載，接受`X509Certificate2`從電腦的目前使用者憑證存放區和組態所提供的憑證指紋：

```csharp
// using System.Linq;
// using System.Security.Cryptography.X509Certificates;
// using Microsoft.Extensions.Configuration;

WebHost.CreateDefaultBuilder(args)
    .ConfigureAppConfiguration((context, config) =>
    {
        if (context.HostingEnvironment.IsProduction())
        {
            var builtConfig = config.Build();

            using (var store = new X509Store(StoreName.My, 
                StoreLocation.CurrentUser))
            {
                store.Open(OpenFlags.ReadOnly);
                var certs = store.Certificates
                    .Find(X509FindType.FindByThumbprint, 
                        builtConfig["CertificateThumbprint"], false);

                config.AddAzureKeyVault(
                    builtConfig["KeyVaultName"], 
                    builtConfig["AzureADApplicationId"], 
                    certs.OfType<X509Certificate2>().Single());

                store.Close();
            }
        }
    })
    .UseStartup<Startup>();
```

如需詳細資訊，請參閱 <<c0> [ 使用而不是用戶端祕密憑證進行驗證](/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret)。

## <a name="bind-an-array-to-a-class"></a>將陣列繫結到類別

提供者可以讀入繫結到 POCO 陣列的陣列中的組態值。

從允許包含冒號的索引鍵的組態來源讀取時 (`:`) 的數值索引鍵的區段分隔符號用來區別構成陣列的索引鍵 (`:0:`， `:1:`，... `:{n}:`) 來存取它所儲存的值。 如需詳細資訊，請參閱[組態：將陣列繫結至類別](xref:fundamentals/configuration/index#bind-an-array-to-a-class)。

Azure Key Vault 金鑰無法使用冒號做為分隔符號。 本主題中所述的方法會使用雙連字號 (`--`) 當作分隔符號 （區段） 的階層式值。 陣列索引鍵時，會儲存在 Azure 金鑰保存庫上，使用雙連字號和數值的重要片段 (`--0--`， `--1--`， &hellip; `--{n}--`)。

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
* 應用程式有錯誤的金鑰保存庫名稱 (`KeyVaultName`)，Azure AD 應用程式識別碼 (`AzureADApplicationId`)，或 Azure AD 密碼 （用戶端祕密） (`AzureADPassword`)。
* Azure AD 密碼 （用戶端祕密） (`AzureADPassword`) 已過期。
* 不正確的值，您想要載入的應用程式中設定金鑰 （名稱）。

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/configuration/index>
* [Microsoft Azure：金鑰保存庫](https://azure.microsoft.com/services/key-vault/)
* [Microsoft Azure：Key Vault 文件](/azure/key-vault/)
* [如何產生並傳輸受 HSM 保護金鑰的 Azure 金鑰保存庫](/azure/key-vault/key-vault-hsm-protected-keys)
* [KeyVaultClient 類別](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
* [快速入門：設定並從 Azure Key Vault 擷取密碼，利用.NET web 應用程式](/azure/key-vault/quick-create-net)
* [教學課程：如何搭配 Azure Windows 虛擬機器在.NET 中使用 Azure 金鑰保存庫](/azure/key-vault/tutorial-net-windows-virtual-machine)
