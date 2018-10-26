---
title: ASP.NET Core 中的 azure Key Vault 組態提供者
author: guardrex
description: 了解如何使用 Azure 金鑰保存庫的組態提供者設定應用程式使用在執行階段載入的名稱 / 值組。
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: security/key-vault-configuration
ms.openlocfilehash: 06445eb2ecec4cf101b23a4bfe131b2c56a18f62
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090302"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a><span data-ttu-id="ee410-103">ASP.NET Core 中的 azure Key Vault 組態提供者</span><span class="sxs-lookup"><span data-stu-id="ee410-103">Azure Key Vault configuration provider in ASP.NET Core</span></span>

<span data-ttu-id="ee410-104">藉由[Luke Latham](https://github.com/guardrex)和[Andrew Stanton-nurse](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="ee410-104">By [Luke Latham](https://github.com/guardrex) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="ee410-105">本文件說明如何使用[Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/)組態提供者，從 Azure Key Vault 祕密載入應用程式組態值。</span><span class="sxs-lookup"><span data-stu-id="ee410-105">This document explains how to use the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) configuration provider to load app configuration values from Azure Key Vault secrets.</span></span> <span data-ttu-id="ee410-106">Azure Key Vault 是雲端式服務，可協助您保護密碼編譯金鑰和應用程式和服務所使用的密碼。</span><span class="sxs-lookup"><span data-stu-id="ee410-106">Azure Key Vault is a cloud-based service that helps you safeguard cryptographic keys and secrets used by apps and services.</span></span> <span data-ttu-id="ee410-107">常見的案例包括控制存取敏感的組態資料，並符合需求的 FIPS 140-2 Level 2 驗證的硬體安全性模組 (HSM) 儲存組態資料時。</span><span class="sxs-lookup"><span data-stu-id="ee410-107">Common scenarios include controlling access to sensitive configuration data and meeting the requirement for FIPS 140-2 Level 2 validated Hardware Security Modules (HSM's) when storing configuration data.</span></span> <span data-ttu-id="ee410-108">這項功能是適用於 ASP.NET Core 1.1 為目標的應用程式或更高版本。</span><span class="sxs-lookup"><span data-stu-id="ee410-108">This feature is available for apps that target ASP.NET Core 1.1 or higher.</span></span>

<span data-ttu-id="ee410-109">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ee410-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="package"></a><span data-ttu-id="ee410-110">Package</span><span class="sxs-lookup"><span data-stu-id="ee410-110">Package</span></span>

<span data-ttu-id="ee410-111">若要使用的提供者，將參考加入[Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/)封裝。</span><span class="sxs-lookup"><span data-stu-id="ee410-111">To use the provider, add a reference to the [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) package.</span></span>

## <a name="app-configuration"></a><span data-ttu-id="ee410-112">應用程式設定</span><span class="sxs-lookup"><span data-stu-id="ee410-112">App configuration</span></span>

<span data-ttu-id="ee410-113">您可以瀏覽提供者[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples)。</span><span class="sxs-lookup"><span data-stu-id="ee410-113">You can explore the provider with the [sample apps](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples).</span></span> <span data-ttu-id="ee410-114">一旦您建立金鑰保存庫，並建立祕密，保存庫中，範例應用程式安全地載入其組態中的祕密的值，並在網頁中顯示它們。</span><span class="sxs-lookup"><span data-stu-id="ee410-114">Once you establish a key vault and create secrets in the vault, the sample apps securely load the secret values into their configurations and display them in webpages.</span></span>

<span data-ttu-id="ee410-115">提供者新增至應用程式的組態`AddAzureKeyVault`延伸模組。</span><span class="sxs-lookup"><span data-stu-id="ee410-115">The provider is added to the app's configuration with the `AddAzureKeyVault` extension.</span></span> <span data-ttu-id="ee410-116">在範例應用程式擴充功能會使用從載入的三個組態值*appsettings.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="ee410-116">In the sample apps, the extension uses three configuration values loaded from the *appsettings.json* file.</span></span>

| <span data-ttu-id="ee410-117">應用程式設定</span><span class="sxs-lookup"><span data-stu-id="ee410-117">App Setting</span></span>    | <span data-ttu-id="ee410-118">描述</span><span class="sxs-lookup"><span data-stu-id="ee410-118">Description</span></span>                    | <span data-ttu-id="ee410-119">範例</span><span class="sxs-lookup"><span data-stu-id="ee410-119">Example</span></span>                                      |
| -------------- | ------------------------------ | -------------------------------------------- |
| `Vault`        | <span data-ttu-id="ee410-120">Azure 金鑰保存庫名稱</span><span class="sxs-lookup"><span data-stu-id="ee410-120">Azure Key Vault name</span></span>           | <span data-ttu-id="ee410-121">contosovault</span><span class="sxs-lookup"><span data-stu-id="ee410-121">contosovault</span></span>                                 |
| `ClientId`     | <span data-ttu-id="ee410-122">Azure Active Directory 應用程式識別碼</span><span class="sxs-lookup"><span data-stu-id="ee410-122">Azure Active Directory App Id</span></span>  | <span data-ttu-id="ee410-123">627e911e-43cc-61d4-992e-12db9c81b413</span><span class="sxs-lookup"><span data-stu-id="ee410-123">627e911e-43cc-61d4-992e-12db9c81b413</span></span>         |
| `ClientSecret` | <span data-ttu-id="ee410-124">Azure Active Directory 應用程式金鑰</span><span class="sxs-lookup"><span data-stu-id="ee410-124">Azure Active Directory App Key</span></span> | <span data-ttu-id="ee410-125">g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di=</span><span class="sxs-lookup"><span data-stu-id="ee410-125">g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di=</span></span> |

[!code-csharp[Program](key-vault-configuration/samples/basic-sample/2.x/Program.cs?name=snippet1)]

## <a name="create-key-vault-secrets-and-load-configuration-values-basic-sample"></a><span data-ttu-id="ee410-126">建立金鑰保存庫祕密，並載入組態值 （基本範例）</span><span class="sxs-lookup"><span data-stu-id="ee410-126">Create key vault secrets and load configuration values (basic-sample)</span></span>

1. <span data-ttu-id="ee410-127">建立金鑰保存庫，並設定 Azure Active Directory (Azure AD) 應用程式中的指導方針[開始使用 Azure Key Vault](https://azure.microsoft.com/documentation/articles/key-vault-get-started/)。</span><span class="sxs-lookup"><span data-stu-id="ee410-127">Create a key vault and set up Azure Active Directory (Azure AD) for the app following the guidance in [Get started with Azure Key Vault](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).</span></span>
   * <span data-ttu-id="ee410-128">將密碼新增至金鑰保存庫使用[AzureRM 金鑰保存庫 PowerShell 模組](/powershell/module/azurerm.keyvault)苀[PowerShell 資源庫](https://www.powershellgallery.com/packages/AzureRM.KeyVault)，則[Azure Key Vault REST API](/rest/api/keyvault/)，或[Azure 入口網站](https://portal.azure.com/)。</span><span class="sxs-lookup"><span data-stu-id="ee410-128">Add secrets to the key vault using the [AzureRM Key Vault PowerShell Module](/powershell/module/azurerm.keyvault) available from the [PowerShell Gallery](https://www.powershellgallery.com/packages/AzureRM.KeyVault), the [Azure Key Vault REST API](/rest/api/keyvault/), or the [Azure Portal](https://portal.azure.com/).</span></span> <span data-ttu-id="ee410-129">為建立祕密*手動*或是*憑證*祕密。</span><span class="sxs-lookup"><span data-stu-id="ee410-129">Secrets are created as either *Manual* or *Certificate* secrets.</span></span> <span data-ttu-id="ee410-130">*憑證*機密資料是應用程式和服務所使用的憑證，但不是支援的組態提供者。</span><span class="sxs-lookup"><span data-stu-id="ee410-130">*Certificate* secrets are certificates for use by apps and services but are not supported by the configuration provider.</span></span> <span data-ttu-id="ee410-131">您應該使用*手動*選項建立的組態提供者使用的名稱 / 值組祕密。</span><span class="sxs-lookup"><span data-stu-id="ee410-131">You should use the *Manual* option to create name-value pair secrets for use with the configuration provider.</span></span>
     * <span data-ttu-id="ee410-132">簡單的密碼會建立為名稱 / 值組。</span><span class="sxs-lookup"><span data-stu-id="ee410-132">Simple secrets are created as name-value pairs.</span></span> <span data-ttu-id="ee410-133">Azure Key Vault 祕密名稱必須是限制為英數字元及虛線。</span><span class="sxs-lookup"><span data-stu-id="ee410-133">Azure Key Vault secret names are limited to alphanumeric characters and dashes.</span></span>
     * <span data-ttu-id="ee410-134">階層式的值 （組態區段） 使用`--`（兩個連字號） 做為分隔符號，在此範例中。</span><span class="sxs-lookup"><span data-stu-id="ee410-134">Hierarchical values (configuration sections) use `--` (two dashes) as a separator in the sample.</span></span> <span data-ttu-id="ee410-135">通常用來分隔的區段中的子機碼的冒號[ASP.NET Core 組態](xref:fundamentals/configuration/index)，祕密名稱中不允許。</span><span class="sxs-lookup"><span data-stu-id="ee410-135">Colons, which are normally used to delimit a section from a subkey in [ASP.NET Core configuration](xref:fundamentals/configuration/index), aren't allowed in secret names.</span></span> <span data-ttu-id="ee410-136">因此，兩個連字號是用，而且密碼會載入應用程式的設定時，已還原為冒號。</span><span class="sxs-lookup"><span data-stu-id="ee410-136">Therefore, two dashes are used and swapped for a colon when the secrets are loaded into the app's configuration.</span></span>
     * <span data-ttu-id="ee410-137">建立兩個*手動*具有下列名稱 / 值組的密碼。</span><span class="sxs-lookup"><span data-stu-id="ee410-137">Create two *Manual* secrets with the following name-value pairs.</span></span> <span data-ttu-id="ee410-138">第一個密碼是簡單名稱和值，和第二個祕密建立祕密的值與區段和祕密名稱中的子機碼：</span><span class="sxs-lookup"><span data-stu-id="ee410-138">The first secret is a simple name and value, and the second secret creates a secret value with a section and subkey in the secret name:</span></span>
       * <span data-ttu-id="ee410-139">`SecretName`: `secret_value_1`</span><span class="sxs-lookup"><span data-stu-id="ee410-139">`SecretName`: `secret_value_1`</span></span>
       * <span data-ttu-id="ee410-140">`Section--SecretName`: `secret_value_2`</span><span class="sxs-lookup"><span data-stu-id="ee410-140">`Section--SecretName`: `secret_value_2`</span></span>
   * <span data-ttu-id="ee410-141">向 Azure Active Directory 中註冊範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="ee410-141">Register the sample app with Azure Active Directory.</span></span>
   * <span data-ttu-id="ee410-142">授權應用程式存取金鑰保存庫。</span><span class="sxs-lookup"><span data-stu-id="ee410-142">Authorize the app to access the key vault.</span></span> <span data-ttu-id="ee410-143">當您使用`Set-AzureRmKeyVaultAccessPolicy`PowerShell cmdlet，授權應用程式存取金鑰保存庫中，提供`List`並`Get`祕密存取`-PermissionsToSecrets list,get`。</span><span class="sxs-lookup"><span data-stu-id="ee410-143">When you use the `Set-AzureRmKeyVaultAccessPolicy` PowerShell cmdlet to authorize the app to access the key vault, provide `List` and `Get` access to secrets with `-PermissionsToSecrets list,get`.</span></span>

2. <span data-ttu-id="ee410-144">應用程式的更新*appsettings.json*的值的檔案`Vault`， `ClientId`，和`ClientSecret`。</span><span class="sxs-lookup"><span data-stu-id="ee410-144">Update the app's *appsettings.json* file with the values of `Vault`, `ClientId`, and `ClientSecret`.</span></span>
3. <span data-ttu-id="ee410-145">執行範例應用程式，它會取得從其組態值`IConfigurationRoot`祕密名稱同名。</span><span class="sxs-lookup"><span data-stu-id="ee410-145">Run the sample app, which obtains its configuration values from `IConfigurationRoot` with the same name as the secret name.</span></span>
   * <span data-ttu-id="ee410-146">非階層式的值： 值`SecretName`取得`config["SecretName"]`。</span><span class="sxs-lookup"><span data-stu-id="ee410-146">Non-hierarchical values: The value for `SecretName` is obtained with `config["SecretName"]`.</span></span>
   * <span data-ttu-id="ee410-147">（區段） 的階層式值： 使用`:`（冒號） 標記法或`GetSection`擴充方法。</span><span class="sxs-lookup"><span data-stu-id="ee410-147">Hierarchical values (sections): Use `:` (colon) notation or the `GetSection` extension method.</span></span> <span data-ttu-id="ee410-148">您可以使用任一種方法來取得組態值：</span><span class="sxs-lookup"><span data-stu-id="ee410-148">Use either of these approaches to obtain the configuration value:</span></span>
     * `config["Section:SecretName"]`
     * `config.GetSection("Section")["SecretName"]`

<span data-ttu-id="ee410-149">當您執行應用程式時，網頁會顯示載入祕密的值：</span><span class="sxs-lookup"><span data-stu-id="ee410-149">When you run the app, a webpage shows the loaded secret values:</span></span>

![透過 「 Azure 金鑰保存庫組態提供者的瀏覽器視窗中顯示祕密值載入](key-vault-configuration/_static/sample1.png)

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="ee410-151">將陣列繫結到類別</span><span class="sxs-lookup"><span data-stu-id="ee410-151">Bind an array to a class</span></span>

<span data-ttu-id="ee410-152">提供者可以讀入繫結到 POCO 陣列的陣列中的組態值。</span><span class="sxs-lookup"><span data-stu-id="ee410-152">The provider is capable of reading configuration values into an array for binding to a POCO array.</span></span>

<span data-ttu-id="ee410-153">從允許包含冒號的索引鍵的組態來源讀取時 (`:`) 的數值索引鍵的區段分隔符號用來區別構成陣列的索引鍵 (`:0:`， `:1:`，...</span><span class="sxs-lookup"><span data-stu-id="ee410-153">When reading from a configuration source that allows keys to contain colon (`:`) separators, a numeric key segment is used to distinguish the keys that make up an array (`:0:`, `:1:`, …</span></span> <span data-ttu-id="ee410-154">`:{n}:`)。</span><span class="sxs-lookup"><span data-stu-id="ee410-154">`:{n}:`).</span></span> <span data-ttu-id="ee410-155">如需詳細資訊，請參閱 <<c0> [ 組態： 將陣列繫結至類別](xref:fundamentals/configuration/index#bind-an-array-to-a-class)。</span><span class="sxs-lookup"><span data-stu-id="ee410-155">For more information, see [Configuration: Bind an array to a class](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span></span>

<span data-ttu-id="ee410-156">Azure Key Vault 金鑰無法使用冒號做為分隔符號。</span><span class="sxs-lookup"><span data-stu-id="ee410-156">Azure Key Vault keys can't use a colon as a separator.</span></span> <span data-ttu-id="ee410-157">本主題中所述的方法會使用雙連字號 (`--`) 當作分隔符號 （區段） 的階層式值。</span><span class="sxs-lookup"><span data-stu-id="ee410-157">The approach described in this topic uses double dashes (`--`) as a separator for hierarchical values (sections).</span></span> <span data-ttu-id="ee410-158">陣列索引鍵時，會儲存在 Azure 金鑰保存庫上，使用雙連字號和數值的重要片段 (`--0--`， `--1--`，...</span><span class="sxs-lookup"><span data-stu-id="ee410-158">Array keys are stored in Azure Key Vault with double dashes and numeric key segments (`--0--`, `--1--`, …</span></span> <span data-ttu-id="ee410-159">`--{n}--`)。</span><span class="sxs-lookup"><span data-stu-id="ee410-159">`--{n}--`).</span></span>

<span data-ttu-id="ee410-160">檢查下列[Serilog](https://serilog.net/)記錄的 JSON 檔案所提供的提供者組態。</span><span class="sxs-lookup"><span data-stu-id="ee410-160">Examine the following [Serilog](https://serilog.net/) logging provider configuration provided by a JSON file.</span></span> <span data-ttu-id="ee410-161">有兩個物件中定義的常值`WriteTo`陣列，其中會反映兩個 Serilog*接收*，可描述記錄輸出的目的地：</span><span class="sxs-lookup"><span data-stu-id="ee410-161">There are two object literals defined in the `WriteTo` array that reflect two Serilog *sinks*, which describe destinations for logging output:</span></span>

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

<span data-ttu-id="ee410-162">在上述的 JSON 檔案中所示的組態會儲存在 Azure 金鑰保存庫使用雙破折號 (`--`) 標記法和數值的區段：</span><span class="sxs-lookup"><span data-stu-id="ee410-162">The configuration shown in the preceding JSON file is stored in Azure Key Vault using double dash (`--`) notation and numeric segments:</span></span>

| <span data-ttu-id="ee410-163">Key</span><span class="sxs-lookup"><span data-stu-id="ee410-163">Key</span></span> | <span data-ttu-id="ee410-164">值</span><span class="sxs-lookup"><span data-stu-id="ee410-164">Value</span></span> |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="create-prefixed-key-vault-secrets-and-load-configuration-values-key-name-prefix-sample"></a><span data-ttu-id="ee410-165">建立帶有前置詞的金鑰保存庫祕密，並載入組態值 （索引鍵-名稱-前置詞的範例）</span><span class="sxs-lookup"><span data-stu-id="ee410-165">Create prefixed key vault secrets and load configuration values (key-name-prefix-sample)</span></span>

<span data-ttu-id="ee410-166">`AddAzureKeyVault` 也會提供多載可接受的實作`IKeyVaultSecretManager`，這可讓您控制如何金鑰保存庫祕密會轉換成設定的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="ee410-166">`AddAzureKeyVault` also provides an overload that accepts an implementation of `IKeyVaultSecretManager`, which allows you to control how key vault secrets are converted into configuration keys.</span></span> <span data-ttu-id="ee410-167">比方說，您可以實作的介面，將根據您在應用程式啟動時所提供的前置詞值的祕密值。</span><span class="sxs-lookup"><span data-stu-id="ee410-167">For example, you can implement the interface to load secret values based on a prefix value you provide at app startup.</span></span> <span data-ttu-id="ee410-168">這可讓您，比方說，若要載入的應用程式的版本為基礎的祕密。</span><span class="sxs-lookup"><span data-stu-id="ee410-168">This allows you, for example, to load secrets based on the version of the app.</span></span>

> [!WARNING]
> <span data-ttu-id="ee410-169">若要將多個應用程式的祕密放入相同的金鑰保存庫，或將環境的密碼，請勿使用前置詞上的金鑰保存庫祕密 (例如*開發*與*生產*祕密) 置於同一個保存庫。</span><span class="sxs-lookup"><span data-stu-id="ee410-169">Don't use prefixes on key vault secrets to place secrets for multiple apps into the same key vault or to place environmental secrets (for example, *development* versus *production* secrets) into the same vault.</span></span> <span data-ttu-id="ee410-170">我們建議，不同的應用程式和開發/生產環境使用不同的金鑰保存庫來隔離應用程式環境，以最高層級的安全性。</span><span class="sxs-lookup"><span data-stu-id="ee410-170">We recommend that different apps and development/production environments use separate key vaults to isolate app environments for the highest level of security.</span></span>

<span data-ttu-id="ee410-171">您使用第二個範例應用程式中的金鑰保存庫來建立祕密`5000-AppSecret`（期間不允許在金鑰保存庫祕密名稱） 代表您的應用程式的 version=5.0.0.0 新版應用程式祕密。</span><span class="sxs-lookup"><span data-stu-id="ee410-171">Using the second sample app, you create a secret in the key vault for `5000-AppSecret` (periods aren't allowed in key vault secret names) representing an app secret for version 5.0.0.0 of your app.</span></span> <span data-ttu-id="ee410-172">如需另一個版本，5.1.0.0，您建立的密碼`5100-AppSecret`。</span><span class="sxs-lookup"><span data-stu-id="ee410-172">For another version, 5.1.0.0, you create a secret for `5100-AppSecret`.</span></span> <span data-ttu-id="ee410-173">每個應用程式版本會將它自己的祕密值載入其設定為`AppSecret`、 清除關閉版本載入祕密。</span><span class="sxs-lookup"><span data-stu-id="ee410-173">Each app version loads its own secret value into its configuration as `AppSecret`, stripping off the version as it loads the secret.</span></span> <span data-ttu-id="ee410-174">範例的實作如下所示：</span><span class="sxs-lookup"><span data-stu-id="ee410-174">The sample's implementation is shown below:</span></span>

[!code-csharp[Configuration builder](key-vault-configuration/samples/key-name-prefix-sample/2.x/Program.cs?name=snippet1&highlight=18)]

[!code-csharp[PrefixKeyVaultSecretManager](key-vault-configuration/samples/key-name-prefix-sample/2.x/Startup.cs?name=snippet1)]

<span data-ttu-id="ee410-175">`Load`逐一查看，找出有版本前置詞的保存庫祕密提供者演算法所呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="ee410-175">The `Load` method is called by a provider algorithm that iterates through the vault secrets to find the ones that have the version prefix.</span></span> <span data-ttu-id="ee410-176">當版本的前置詞上有`Load`，此演算法會使用`GetKey`方法傳回的密碼名稱的組態名稱。</span><span class="sxs-lookup"><span data-stu-id="ee410-176">When a version prefix is found with `Load`, the algorithm uses the `GetKey` method to return the configuration name of the secret name.</span></span> <span data-ttu-id="ee410-177">它會除去版本前置詞，從 祕密的名稱，並傳回載入的祕密名稱的其餘部分，將應用程式設定成名稱 / 值組。</span><span class="sxs-lookup"><span data-stu-id="ee410-177">It strips off the version prefix from the secret's name and returns the rest of the secret name for loading into the app's configuration name-value pairs.</span></span>

<span data-ttu-id="ee410-178">當您實作這種方法：</span><span class="sxs-lookup"><span data-stu-id="ee410-178">When you implement this approach:</span></span>

1. <span data-ttu-id="ee410-179">載入的金鑰保存庫祕密。</span><span class="sxs-lookup"><span data-stu-id="ee410-179">The key vault secrets are loaded.</span></span>
2. <span data-ttu-id="ee410-180">字串密碼`5000-AppSecret`會比對。</span><span class="sxs-lookup"><span data-stu-id="ee410-180">The string secret for `5000-AppSecret` is matched.</span></span>
3. <span data-ttu-id="ee410-181">版本中， `5000` （使用 dash)，移除離開的索引鍵名稱從`AppSecret`祕密值載入應用程式的設定。</span><span class="sxs-lookup"><span data-stu-id="ee410-181">The version, `5000` (with the dash), is stripped off of the key name leaving `AppSecret` to load with the secret value into the app's configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="ee410-182">您也可以提供您自己`KeyVaultClient`實作`AddAzureKeyVault`。</span><span class="sxs-lookup"><span data-stu-id="ee410-182">You can also provide your own `KeyVaultClient` implementation to `AddAzureKeyVault`.</span></span> <span data-ttu-id="ee410-183">提供自訂的用戶端，可讓您共用的用戶端的組態提供者與您的應用程式的其他部分之間的單一執行個體。</span><span class="sxs-lookup"><span data-stu-id="ee410-183">Supplying a custom client allows you to share a single instance of the client between the configuration provider and other parts of your app.</span></span>

1. <span data-ttu-id="ee410-184">建立金鑰保存庫，並設定 Azure Active Directory (Azure AD) 應用程式中的指導方針[開始使用 Azure Key Vault](https://azure.microsoft.com/documentation/articles/key-vault-get-started/)。</span><span class="sxs-lookup"><span data-stu-id="ee410-184">Create a key vault and set up Azure Active Directory (Azure AD) for the app following the guidance in [Get started with Azure Key Vault](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).</span></span>
   * <span data-ttu-id="ee410-185">將密碼新增至金鑰保存庫使用[AzureRM 金鑰保存庫 PowerShell 模組](/powershell/module/azurerm.keyvault)苀[PowerShell 資源庫](https://www.powershellgallery.com/packages/AzureRM.KeyVault)，則[Azure Key Vault REST API](/rest/api/keyvault/)，或[Azure 入口網站](https://portal.azure.com/)。</span><span class="sxs-lookup"><span data-stu-id="ee410-185">Add secrets to the key vault using the [AzureRM Key Vault PowerShell Module](/powershell/module/azurerm.keyvault) available from the [PowerShell Gallery](https://www.powershellgallery.com/packages/AzureRM.KeyVault), the [Azure Key Vault REST API](/rest/api/keyvault/), or the [Azure Portal](https://portal.azure.com/).</span></span> <span data-ttu-id="ee410-186">為建立祕密*手動*或是*憑證*祕密。</span><span class="sxs-lookup"><span data-stu-id="ee410-186">Secrets are created as either *Manual* or *Certificate* secrets.</span></span> <span data-ttu-id="ee410-187">*憑證*機密資料是應用程式和服務所使用的憑證，但不是支援的組態提供者。</span><span class="sxs-lookup"><span data-stu-id="ee410-187">*Certificate* secrets are certificates for use by apps and services but are not supported by the configuration provider.</span></span> <span data-ttu-id="ee410-188">您應該使用*手動*選項建立的組態提供者使用的名稱 / 值組祕密。</span><span class="sxs-lookup"><span data-stu-id="ee410-188">You should use the *Manual* option to create name-value pair secrets for use with the configuration provider.</span></span>
     * <span data-ttu-id="ee410-189">階層式的值 （組態區段） 使用`--`（兩個連字號） 做為分隔符號。</span><span class="sxs-lookup"><span data-stu-id="ee410-189">Hierarchical values (configuration sections) use `--` (two dashes) as a separator.</span></span>
     * <span data-ttu-id="ee410-190">建立兩個*手動*具有下列名稱 / 值組的祕密：</span><span class="sxs-lookup"><span data-stu-id="ee410-190">Create two *Manual* secrets with the following name-value pairs:</span></span>
       * <span data-ttu-id="ee410-191">`5000-AppSecret`: `5.0.0.0_secret_value`</span><span class="sxs-lookup"><span data-stu-id="ee410-191">`5000-AppSecret`: `5.0.0.0_secret_value`</span></span>
       * <span data-ttu-id="ee410-192">`5100-AppSecret`: `5.1.0.0_secret_value`</span><span class="sxs-lookup"><span data-stu-id="ee410-192">`5100-AppSecret`: `5.1.0.0_secret_value`</span></span>
   * <span data-ttu-id="ee410-193">向 Azure Active Directory 中註冊範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="ee410-193">Register the sample app with Azure Active Directory.</span></span>
   * <span data-ttu-id="ee410-194">授權應用程式存取金鑰保存庫。</span><span class="sxs-lookup"><span data-stu-id="ee410-194">Authorize the app to access the key vault.</span></span> <span data-ttu-id="ee410-195">當您使用`Set-AzureRmKeyVaultAccessPolicy`PowerShell cmdlet，授權應用程式存取金鑰保存庫中，提供`List`並`Get`祕密存取`-PermissionsToSecrets list,get`。</span><span class="sxs-lookup"><span data-stu-id="ee410-195">When you use the `Set-AzureRmKeyVaultAccessPolicy` PowerShell cmdlet to authorize the app to access the key vault, provide `List` and `Get` access to secrets with `-PermissionsToSecrets list,get`.</span></span>

2. <span data-ttu-id="ee410-196">應用程式的更新*appsettings.json*的值的檔案`Vault`， `ClientId`，和`ClientSecret`。</span><span class="sxs-lookup"><span data-stu-id="ee410-196">Update the app's *appsettings.json* file with the values of `Vault`, `ClientId`, and `ClientSecret`.</span></span>
3. <span data-ttu-id="ee410-197">執行範例應用程式，它會取得從其組態值`IConfigurationRoot`前置詞的祕密名稱同名。</span><span class="sxs-lookup"><span data-stu-id="ee410-197">Run the sample app, which obtains its configuration values from `IConfigurationRoot` with the same name as the prefixed secret name.</span></span> <span data-ttu-id="ee410-198">在此範例中，前置詞是應用程式的版本，您提供給`PrefixKeyVaultSecretManager`當您加入的 Azure Key Vault 組態提供者。</span><span class="sxs-lookup"><span data-stu-id="ee410-198">In this sample, the prefix is the app's version, which you provided to the `PrefixKeyVaultSecretManager` when you added the Azure Key Vault configuration provider.</span></span> <span data-ttu-id="ee410-199">值`AppSecret`取得`config["AppSecret"]`。</span><span class="sxs-lookup"><span data-stu-id="ee410-199">The value for `AppSecret` is obtained with `config["AppSecret"]`.</span></span> <span data-ttu-id="ee410-200">應用程式所產生的網頁顯示載入的值：</span><span class="sxs-lookup"><span data-stu-id="ee410-200">The webpage generated by the app shows the loaded value:</span></span>

   ![瀏覽器視窗中顯示應用程式的版本時 version=5.0.0.0，透過 「 Azure 金鑰保存庫組態提供者載入祕密值](key-vault-configuration/_static/sample2-1.png)

4. <span data-ttu-id="ee410-202">變更應用程式中的組件的專案檔版本`5.0.0.0`至`5.1.0.0`並再次執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="ee410-202">Change the version of the app assembly in the project file from `5.0.0.0` to `5.1.0.0` and run the app again.</span></span> <span data-ttu-id="ee410-203">這次，則傳回的祕密值是`5.1.0.0_secret_value`。</span><span class="sxs-lookup"><span data-stu-id="ee410-203">This time, the secret value returned is `5.1.0.0_secret_value`.</span></span> <span data-ttu-id="ee410-204">應用程式所產生的網頁顯示載入的值：</span><span class="sxs-lookup"><span data-stu-id="ee410-204">The webpage generated by the app shows the loaded value:</span></span>

   ![瀏覽器視窗中顯示 5.1.0.0 應用程式的版本時，透過 「 Azure 金鑰保存庫組態提供者載入祕密值](key-vault-configuration/_static/sample2-2.png)

## <a name="control-access-to-the-clientsecret"></a><span data-ttu-id="ee410-206">ClientSecret 控制存取</span><span class="sxs-lookup"><span data-stu-id="ee410-206">Control access to the ClientSecret</span></span>

<span data-ttu-id="ee410-207">使用[Secret Manager 工具](xref:security/app-secrets)維護`ClientSecret`外部專案來源樹狀結構。</span><span class="sxs-lookup"><span data-stu-id="ee410-207">Use the [Secret Manager tool](xref:security/app-secrets) to maintain the `ClientSecret` outside of your project source tree.</span></span> <span data-ttu-id="ee410-208">使用 Secret Manager 中，您將應用程式祕密與特定的專案產生關聯並加以共用跨多個專案。</span><span class="sxs-lookup"><span data-stu-id="ee410-208">With Secret Manager, you associate app secrets with a specific project and share them across multiple projects.</span></span>

<span data-ttu-id="ee410-209">在開發環境可支援憑證中的.NET Framework 應用程式時，您可以使用 X.509 憑證來驗證 Azure Key Vault。</span><span class="sxs-lookup"><span data-stu-id="ee410-209">When developing a .NET Framework app in an environment that supports certificates, you can authenticate to Azure Key Vault with an X.509 certificate.</span></span> <span data-ttu-id="ee410-210">X.509 憑證的私密金鑰是由 OS 管理。</span><span class="sxs-lookup"><span data-stu-id="ee410-210">The X.509 certificate's private key is managed by the OS.</span></span> <span data-ttu-id="ee410-211">如需詳細資訊，請參閱 <<c0> [ 使用而不是用戶端祕密憑證進行驗證](/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret)。</span><span class="sxs-lookup"><span data-stu-id="ee410-211">For more information, see [Authenticate with a Certificate instead of a Client Secret](/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret).</span></span> <span data-ttu-id="ee410-212">使用`AddAzureKeyVault`多載，接受`X509Certificate2`(`_env`在下列範例中：</span><span class="sxs-lookup"><span data-stu-id="ee410-212">Use the `AddAzureKeyVault` overload that accepts an `X509Certificate2` (`_env` in the following example :</span></span>

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

## <a name="reload-secrets"></a><span data-ttu-id="ee410-213">重新載入祕密</span><span class="sxs-lookup"><span data-stu-id="ee410-213">Reload secrets</span></span>

<span data-ttu-id="ee410-214">密碼會快取直到`IConfigurationRoot.Reload()`呼叫。</span><span class="sxs-lookup"><span data-stu-id="ee410-214">Secrets are cached until `IConfigurationRoot.Reload()` is called.</span></span> <span data-ttu-id="ee410-215">過期，停用，以及更新金鑰保存庫中的祕密未遵守應用程式，直到`Reload`執行。</span><span class="sxs-lookup"><span data-stu-id="ee410-215">Expired, disabled, and updated secrets in the key vault are not respected by the app until `Reload` is executed.</span></span>

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a><span data-ttu-id="ee410-216">已停用和到期的祕密</span><span class="sxs-lookup"><span data-stu-id="ee410-216">Disabled and expired secrets</span></span>

<span data-ttu-id="ee410-217">已停用及過期的密碼會擲回`KeyVaultClientException`。</span><span class="sxs-lookup"><span data-stu-id="ee410-217">Disabled and expired secrets throw a `KeyVaultClientException`.</span></span> <span data-ttu-id="ee410-218">若要防止您的應用程式擲回，取代您的應用程式或更新已停用/過期的祕密。</span><span class="sxs-lookup"><span data-stu-id="ee410-218">To prevent your app from throwing, replace your app or update the disabled/expired secret.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="ee410-219">疑難排解</span><span class="sxs-lookup"><span data-stu-id="ee410-219">Troubleshoot</span></span>

<span data-ttu-id="ee410-220">當應用程式無法載入組態使用的提供者時，錯誤訊息會寫入[ASP.NET Core 記錄基礎結構](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="ee410-220">When the app fails to load configuration using the provider, an error message is written to the [ASP.NET Core Logging infrastructure](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="ee410-221">在下列情況會造成無法載入組態：</span><span class="sxs-lookup"><span data-stu-id="ee410-221">The following conditions will prevent configuration from loading:</span></span>

* <span data-ttu-id="ee410-222">應用程式未正確設定 Azure Active Directory 中。</span><span class="sxs-lookup"><span data-stu-id="ee410-222">The app isn't configured correctly in Azure Active Directory.</span></span>
* <span data-ttu-id="ee410-223">金鑰保存庫不存在於 Azure 金鑰保存庫。</span><span class="sxs-lookup"><span data-stu-id="ee410-223">The key vault doesn't exist in Azure Key Vault.</span></span>
* <span data-ttu-id="ee410-224">應用程式未獲授權存取金鑰保存庫。</span><span class="sxs-lookup"><span data-stu-id="ee410-224">The app isn't authorized to access the key vault.</span></span>
* <span data-ttu-id="ee410-225">存取原則不包含`Get`和`List`權限。</span><span class="sxs-lookup"><span data-stu-id="ee410-225">The access policy doesn't include `Get` and `List` permissions.</span></span>
* <span data-ttu-id="ee410-226">在金鑰保存庫中，設定資料 （名稱 / 值組） 錯誤命名為，遺漏，停用，或已過期。</span><span class="sxs-lookup"><span data-stu-id="ee410-226">In the key vault, the configuration data (name-value pair) is incorrectly named, missing, disabled, or expired.</span></span>
* <span data-ttu-id="ee410-227">應用程式有錯誤的金鑰保存庫名稱 (`Vault`)，Azure AD 應用程式識別碼 (`ClientId`)，或 Azure AD 金鑰 (`ClientSecret`)。</span><span class="sxs-lookup"><span data-stu-id="ee410-227">The app has the wrong key vault name (`Vault`), Azure AD App Id (`ClientId`), or Azure AD Key (`ClientSecret`).</span></span>
* <span data-ttu-id="ee410-228">Azure AD 金鑰 (`ClientSecret`) 已過期。</span><span class="sxs-lookup"><span data-stu-id="ee410-228">The Azure AD Key (`ClientSecret`) is expired.</span></span>
* <span data-ttu-id="ee410-229">不正確的值，您想要載入的應用程式中設定金鑰 （名稱）。</span><span class="sxs-lookup"><span data-stu-id="ee410-229">The configuration key (name) is incorrect in the app for the value you're trying to load.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ee410-230">其他資源</span><span class="sxs-lookup"><span data-stu-id="ee410-230">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* [<span data-ttu-id="ee410-231">Microsoft Azure： 金鑰保存庫</span><span class="sxs-lookup"><span data-stu-id="ee410-231">Microsoft Azure: Key Vault</span></span>](https://azure.microsoft.com/services/key-vault/)
* [<span data-ttu-id="ee410-232">Microsoft Azure: Key Vault 文件</span><span class="sxs-lookup"><span data-stu-id="ee410-232">Microsoft Azure: Key Vault Documentation</span></span>](/azure/key-vault/)
* [<span data-ttu-id="ee410-233">如何產生並傳輸受 HSM 保護金鑰的 Azure 金鑰保存庫</span><span class="sxs-lookup"><span data-stu-id="ee410-233">How to generate and transfer HSM-protected keys for Azure Key Vault</span></span>](/azure/key-vault/key-vault-hsm-protected-keys)
* [<span data-ttu-id="ee410-234">KeyVaultClient 類別</span><span class="sxs-lookup"><span data-stu-id="ee410-234">KeyVaultClient Class</span></span>](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
