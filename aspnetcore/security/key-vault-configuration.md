---
title: ASP.NET Core 中的 azure Key Vault 組態提供者
author: guardrex
description: 了解如何使用 Azure 金鑰保存庫的組態提供者設定應用程式使用在執行階段載入的名稱 / 值組。
ms.author: riande
ms.date: 08/01/2018
uid: security/key-vault-configuration
ms.openlocfilehash: 6474b9f5cb9e441854565a7891c4aac7f781c810
ms.sourcegitcommit: 571d76fbbff05e84406b6d909c8fe9cbea2c8ff1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/01/2018
ms.locfileid: "39410126"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a><span data-ttu-id="22f01-103">ASP.NET Core 中的 azure Key Vault 組態提供者</span><span class="sxs-lookup"><span data-stu-id="22f01-103">Azure Key Vault configuration provider in ASP.NET Core</span></span>

<span data-ttu-id="22f01-104">藉由[Luke Latham](https://github.com/guardrex)和[Andrew Stanton-nurse](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="22f01-104">By [Luke Latham](https://github.com/guardrex) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="22f01-105">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="22f01-105">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="22f01-106">檢視或下載 2.x 的範例程式碼：</span><span class="sxs-lookup"><span data-stu-id="22f01-106">View or download sample code for 2.x:</span></span>

* <span data-ttu-id="22f01-107">[基本範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/basic-sample/2.x)([如何下載](xref:tutorials/index#how-to-download-a-sample))-讀取到應用程式的祕密的值。</span><span class="sxs-lookup"><span data-stu-id="22f01-107">[Basic sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/basic-sample/2.x) ([how to download](xref:tutorials/index#how-to-download-a-sample)) - Reads secret values into an app.</span></span>
* <span data-ttu-id="22f01-108">[機碼名稱前置詞的範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/key-name-prefix-sample/2.x)([如何下載](xref:tutorials/index#how-to-download-a-sample))-讀取祕密值使用的機碼名稱前置詞，表示應用程式，可讓您載入一組不同的密碼值，針對每個應用程式版本的版本。</span><span class="sxs-lookup"><span data-stu-id="22f01-108">[Key name prefix sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/key-name-prefix-sample/2.x) ([how to download](xref:tutorials/index#how-to-download-a-sample)) - Reads secret values using a key name prefix that represents the version of an app, which allows you to load a different set of secret values for each app version.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="22f01-109">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="22f01-109">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="22f01-110">檢視或下載 1.x 的範例程式碼：</span><span class="sxs-lookup"><span data-stu-id="22f01-110">View or download sample code for 1.x:</span></span>

* <span data-ttu-id="22f01-111">[基本範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/basic-sample/1.x)([如何下載](xref:tutorials/index#how-to-download-a-sample))-讀取到應用程式的祕密的值。</span><span class="sxs-lookup"><span data-stu-id="22f01-111">[Basic sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/basic-sample/1.x) ([how to download](xref:tutorials/index#how-to-download-a-sample)) - Reads secret values into an app.</span></span>
* <span data-ttu-id="22f01-112">[機碼名稱前置詞的範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/key-name-prefix-sample/1.x)([如何下載](xref:tutorials/index#how-to-download-a-sample))-讀取祕密值使用的機碼名稱前置詞，表示應用程式，可讓您載入一組不同的密碼值，針對每個應用程式版本的版本。</span><span class="sxs-lookup"><span data-stu-id="22f01-112">[Key name prefix sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/key-name-prefix-sample/1.x) ([how to download](xref:tutorials/index#how-to-download-a-sample)) - Reads secret values using a key name prefix that represents the version of an app, which allows you to load a different set of secret values for each app version.</span></span>

---

<span data-ttu-id="22f01-113">本文件說明如何使用[Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/)組態提供者，從 Azure Key Vault 祕密載入應用程式組態值。</span><span class="sxs-lookup"><span data-stu-id="22f01-113">This document explains how to use the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) configuration provider to load app configuration values from Azure Key Vault secrets.</span></span> <span data-ttu-id="22f01-114">Azure Key Vault 是雲端式服務，可協助您保護密碼編譯金鑰和應用程式和服務所使用的密碼。</span><span class="sxs-lookup"><span data-stu-id="22f01-114">Azure Key Vault is a cloud-based service that helps you safeguard cryptographic keys and secrets used by apps and services.</span></span> <span data-ttu-id="22f01-115">常見的案例包括控制存取敏感的組態資料，並符合需求的 FIPS 140-2 Level 2 驗證的硬體安全性模組 (HSM) 儲存組態資料時。</span><span class="sxs-lookup"><span data-stu-id="22f01-115">Common scenarios include controlling access to sensitive configuration data and meeting the requirement for FIPS 140-2 Level 2 validated Hardware Security Modules (HSM's) when storing configuration data.</span></span> <span data-ttu-id="22f01-116">這項功能是適用於 ASP.NET Core 1.1 為目標的應用程式或更高版本。</span><span class="sxs-lookup"><span data-stu-id="22f01-116">This feature is available for apps that target ASP.NET Core 1.1 or higher.</span></span>

## <a name="package"></a><span data-ttu-id="22f01-117">Package</span><span class="sxs-lookup"><span data-stu-id="22f01-117">Package</span></span>

<span data-ttu-id="22f01-118">若要使用的提供者，將參考加入[Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/)封裝。</span><span class="sxs-lookup"><span data-stu-id="22f01-118">To use the provider, add a reference to the [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) package.</span></span>

## <a name="app-configuration"></a><span data-ttu-id="22f01-119">應用程式設定</span><span class="sxs-lookup"><span data-stu-id="22f01-119">App configuration</span></span>

<span data-ttu-id="22f01-120">您可以瀏覽提供者[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples)。</span><span class="sxs-lookup"><span data-stu-id="22f01-120">You can explore the provider with the [sample apps](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples).</span></span> <span data-ttu-id="22f01-121">一旦您建立金鑰保存庫，並建立祕密，保存庫中，範例應用程式安全地載入其組態中的祕密的值，並在網頁中顯示它們。</span><span class="sxs-lookup"><span data-stu-id="22f01-121">Once you establish a key vault and create secrets in the vault, the sample apps securely load the secret values into their configurations and display them in webpages.</span></span>

<span data-ttu-id="22f01-122">提供者新增至應用程式的組態`AddAzureKeyVault`延伸模組。</span><span class="sxs-lookup"><span data-stu-id="22f01-122">The provider is added to the app's configuration with the `AddAzureKeyVault` extension.</span></span> <span data-ttu-id="22f01-123">在範例應用程式擴充功能會使用從載入的三個組態值*appsettings.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="22f01-123">In the sample apps, the extension uses three configuration values loaded from the *appsettings.json* file.</span></span>

| <span data-ttu-id="22f01-124">應用程式設定</span><span class="sxs-lookup"><span data-stu-id="22f01-124">App Setting</span></span>    | <span data-ttu-id="22f01-125">描述</span><span class="sxs-lookup"><span data-stu-id="22f01-125">Description</span></span>                    | <span data-ttu-id="22f01-126">範例</span><span class="sxs-lookup"><span data-stu-id="22f01-126">Example</span></span>                                      |
| -------------- | ------------------------------ | -------------------------------------------- |
| `Vault`        | <span data-ttu-id="22f01-127">Azure 金鑰保存庫名稱</span><span class="sxs-lookup"><span data-stu-id="22f01-127">Azure Key Vault name</span></span>           | <span data-ttu-id="22f01-128">contosovault</span><span class="sxs-lookup"><span data-stu-id="22f01-128">contosovault</span></span>                                 |
| `ClientId`     | <span data-ttu-id="22f01-129">Azure Active Directory 應用程式識別碼</span><span class="sxs-lookup"><span data-stu-id="22f01-129">Azure Active Directory App Id</span></span>  | <span data-ttu-id="22f01-130">627e911e-43cc-61d4-992e-12db9c81b413</span><span class="sxs-lookup"><span data-stu-id="22f01-130">627e911e-43cc-61d4-992e-12db9c81b413</span></span>         |
| `ClientSecret` | <span data-ttu-id="22f01-131">Azure Active Directory 應用程式金鑰</span><span class="sxs-lookup"><span data-stu-id="22f01-131">Azure Active Directory App Key</span></span> | <span data-ttu-id="22f01-132">g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di=</span><span class="sxs-lookup"><span data-stu-id="22f01-132">g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di=</span></span> |

[!code-csharp[Program](key-vault-configuration/samples/basic-sample/2.x/Program.cs?name=snippet1)]

## <a name="creating-key-vault-secrets-and-loading-configuration-values-basic-sample"></a><span data-ttu-id="22f01-133">建立金鑰保存庫密碼和載入組態值 （基本範例）</span><span class="sxs-lookup"><span data-stu-id="22f01-133">Creating key vault secrets and loading configuration values (basic-sample)</span></span>

1. <span data-ttu-id="22f01-134">建立金鑰保存庫，並設定 Azure Active Directory (Azure AD) 應用程式中的指導方針[開始使用 Azure Key Vault](https://azure.microsoft.com/documentation/articles/key-vault-get-started/)。</span><span class="sxs-lookup"><span data-stu-id="22f01-134">Create a key vault and set up Azure Active Directory (Azure AD) for the app following the guidance in [Get started with Azure Key Vault](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).</span></span>
   * <span data-ttu-id="22f01-135">將密碼新增至金鑰保存庫使用[AzureRM 金鑰保存庫 PowerShell 模組](/powershell/module/azurerm.keyvault)苀[PowerShell 資源庫](https://www.powershellgallery.com/packages/AzureRM.KeyVault)，則[Azure Key Vault REST API](/rest/api/keyvault/)，或[Azure 入口網站](https://portal.azure.com/)。</span><span class="sxs-lookup"><span data-stu-id="22f01-135">Add secrets to the key vault using the [AzureRM Key Vault PowerShell Module](/powershell/module/azurerm.keyvault) available from the [PowerShell Gallery](https://www.powershellgallery.com/packages/AzureRM.KeyVault), the [Azure Key Vault REST API](/rest/api/keyvault/), or the [Azure Portal](https://portal.azure.com/).</span></span> <span data-ttu-id="22f01-136">為建立祕密*手動*或是*憑證*祕密。</span><span class="sxs-lookup"><span data-stu-id="22f01-136">Secrets are created as either *Manual* or *Certificate* secrets.</span></span> <span data-ttu-id="22f01-137">*憑證*機密資料是應用程式和服務所使用的憑證，但不是支援的組態提供者。</span><span class="sxs-lookup"><span data-stu-id="22f01-137">*Certificate* secrets are certificates for use by apps and services but are not supported by the configuration provider.</span></span> <span data-ttu-id="22f01-138">您應該使用*手動*選項建立的組態提供者使用的名稱 / 值組祕密。</span><span class="sxs-lookup"><span data-stu-id="22f01-138">You should use the *Manual* option to create name-value pair secrets for use with the configuration provider.</span></span>
     * <span data-ttu-id="22f01-139">簡單的密碼會建立為名稱 / 值組。</span><span class="sxs-lookup"><span data-stu-id="22f01-139">Simple secrets are created as name-value pairs.</span></span> <span data-ttu-id="22f01-140">Azure Key Vault 祕密名稱必須是限制為英數字元及虛線。</span><span class="sxs-lookup"><span data-stu-id="22f01-140">Azure Key Vault secret names are limited to alphanumeric characters and dashes.</span></span>
     * <span data-ttu-id="22f01-141">階層式的值 （組態區段） 使用`--`（兩個連字號） 做為分隔符號，在此範例中。</span><span class="sxs-lookup"><span data-stu-id="22f01-141">Hierarchical values (configuration sections) use `--` (two dashes) as a separator in the sample.</span></span> <span data-ttu-id="22f01-142">通常用來分隔的區段中的子機碼的冒號[ASP.NET Core 組態](xref:fundamentals/configuration/index)，祕密名稱中不允許。</span><span class="sxs-lookup"><span data-stu-id="22f01-142">Colons, which are normally used to delimit a section from a subkey in [ASP.NET Core configuration](xref:fundamentals/configuration/index), aren't allowed in secret names.</span></span> <span data-ttu-id="22f01-143">因此，兩個連字號是用，而且密碼會載入應用程式的設定時，已還原為冒號。</span><span class="sxs-lookup"><span data-stu-id="22f01-143">Therefore, two dashes are used and swapped for a colon when the secrets are loaded into the app's configuration.</span></span>
     * <span data-ttu-id="22f01-144">建立兩個*手動*具有下列名稱 / 值組的密碼。</span><span class="sxs-lookup"><span data-stu-id="22f01-144">Create two *Manual* secrets with the following name-value pairs.</span></span> <span data-ttu-id="22f01-145">第一個密碼是簡單名稱和值，和第二個祕密建立祕密的值與區段和祕密名稱中的子機碼：</span><span class="sxs-lookup"><span data-stu-id="22f01-145">The first secret is a simple name and value, and the second secret creates a secret value with a section and subkey in the secret name:</span></span>
       * <span data-ttu-id="22f01-146">`SecretName`: `secret_value_1`</span><span class="sxs-lookup"><span data-stu-id="22f01-146">`SecretName`: `secret_value_1`</span></span>
       * <span data-ttu-id="22f01-147">`Section--SecretName`: `secret_value_2`</span><span class="sxs-lookup"><span data-stu-id="22f01-147">`Section--SecretName`: `secret_value_2`</span></span>
   * <span data-ttu-id="22f01-148">向 Azure Active Directory 中註冊範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="22f01-148">Register the sample app with Azure Active Directory.</span></span>
   * <span data-ttu-id="22f01-149">授權應用程式存取金鑰保存庫。</span><span class="sxs-lookup"><span data-stu-id="22f01-149">Authorize the app to access the key vault.</span></span> <span data-ttu-id="22f01-150">當您使用`Set-AzureRmKeyVaultAccessPolicy`PowerShell cmdlet，授權應用程式存取金鑰保存庫中，提供`List`並`Get`祕密存取`-PermissionsToSecrets list,get`。</span><span class="sxs-lookup"><span data-stu-id="22f01-150">When you use the `Set-AzureRmKeyVaultAccessPolicy` PowerShell cmdlet to authorize the app to access the key vault, provide `List` and `Get` access to secrets with `-PermissionsToSecrets list,get`.</span></span>

2. <span data-ttu-id="22f01-151">應用程式的更新*appsettings.json*的值的檔案`Vault`， `ClientId`，和`ClientSecret`。</span><span class="sxs-lookup"><span data-stu-id="22f01-151">Update the app's *appsettings.json* file with the values of `Vault`, `ClientId`, and `ClientSecret`.</span></span>
3. <span data-ttu-id="22f01-152">執行範例應用程式，它會取得從其組態值`IConfigurationRoot`祕密名稱同名。</span><span class="sxs-lookup"><span data-stu-id="22f01-152">Run the sample app, which obtains its configuration values from `IConfigurationRoot` with the same name as the secret name.</span></span>
   * <span data-ttu-id="22f01-153">非階層式的值： 值`SecretName`取得`config["SecretName"]`。</span><span class="sxs-lookup"><span data-stu-id="22f01-153">Non-hierarchical values: The value for `SecretName` is obtained with `config["SecretName"]`.</span></span>
   * <span data-ttu-id="22f01-154">（區段） 的階層式值： 使用`:`（冒號） 標記法或`GetSection`擴充方法。</span><span class="sxs-lookup"><span data-stu-id="22f01-154">Hierarchical values (sections): Use `:` (colon) notation or the `GetSection` extension method.</span></span> <span data-ttu-id="22f01-155">您可以使用任一種方法來取得組態值：</span><span class="sxs-lookup"><span data-stu-id="22f01-155">Use either of these approaches to obtain the configuration value:</span></span>
     * `config["Section:SecretName"]`
     * `config.GetSection("Section")["SecretName"]`

<span data-ttu-id="22f01-156">當您執行應用程式時，網頁會顯示載入祕密的值：</span><span class="sxs-lookup"><span data-stu-id="22f01-156">When you run the app, a webpage shows the loaded secret values:</span></span>

![透過 「 Azure 金鑰保存庫組態提供者的瀏覽器視窗中顯示祕密值載入](key-vault-configuration/_static/sample1.png)

## <a name="creating-prefixed-key-vault-secrets-and-loading-configuration-values-key-name-prefix-sample"></a><span data-ttu-id="22f01-158">建立帶有前置詞的金鑰保存庫密碼和載入組態值 （索引鍵-名稱-前置詞的範例）</span><span class="sxs-lookup"><span data-stu-id="22f01-158">Creating prefixed key vault secrets and loading configuration values (key-name-prefix-sample)</span></span>

<span data-ttu-id="22f01-159">`AddAzureKeyVault` 也會提供多載可接受的實作`IKeyVaultSecretManager`，這可讓您控制如何金鑰保存庫祕密會轉換成設定的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="22f01-159">`AddAzureKeyVault` also provides an overload that accepts an implementation of `IKeyVaultSecretManager`, which allows you to control how key vault secrets are converted into configuration keys.</span></span> <span data-ttu-id="22f01-160">比方說，您可以實作的介面，將根據您在應用程式啟動時所提供的前置詞值的祕密值。</span><span class="sxs-lookup"><span data-stu-id="22f01-160">For example, you can implement the interface to load secret values based on a prefix value you provide at app startup.</span></span> <span data-ttu-id="22f01-161">這可讓您，比方說，若要載入的應用程式的版本為基礎的祕密。</span><span class="sxs-lookup"><span data-stu-id="22f01-161">This allows you, for example, to load secrets based on the version of the app.</span></span>

> [!WARNING]
> <span data-ttu-id="22f01-162">若要將多個應用程式的祕密放入相同的金鑰保存庫，或將環境的密碼，請勿使用前置詞上的金鑰保存庫祕密 (例如*開發*與*生產*祕密) 置於同一個保存庫。</span><span class="sxs-lookup"><span data-stu-id="22f01-162">Don't use prefixes on key vault secrets to place secrets for multiple apps into the same key vault or to place environmental secrets (for example, *development* versus *production* secrets) into the same vault.</span></span> <span data-ttu-id="22f01-163">我們建議，不同的應用程式和開發/生產環境使用不同的金鑰保存庫來隔離應用程式環境，以最高層級的安全性。</span><span class="sxs-lookup"><span data-stu-id="22f01-163">We recommend that different apps and development/production environments use separate key vaults to isolate app environments for the highest level of security.</span></span>

<span data-ttu-id="22f01-164">您使用第二個範例應用程式中的金鑰保存庫來建立祕密`5000-AppSecret`（期間不允許在金鑰保存庫祕密名稱） 代表您的應用程式的 version=5.0.0.0 新版應用程式祕密。</span><span class="sxs-lookup"><span data-stu-id="22f01-164">Using the second sample app, you create a secret in the key vault for `5000-AppSecret` (periods aren't allowed in key vault secret names) representing an app secret for version 5.0.0.0 of your app.</span></span> <span data-ttu-id="22f01-165">如需另一個版本，5.1.0.0，您建立的密碼`5100-AppSecret`。</span><span class="sxs-lookup"><span data-stu-id="22f01-165">For another version, 5.1.0.0, you create a secret for `5100-AppSecret`.</span></span> <span data-ttu-id="22f01-166">每個應用程式版本會將它自己的祕密值載入其設定為`AppSecret`、 清除關閉版本載入祕密。</span><span class="sxs-lookup"><span data-stu-id="22f01-166">Each app version loads its own secret value into its configuration as `AppSecret`, stripping off the version as it loads the secret.</span></span> <span data-ttu-id="22f01-167">範例的實作如下所示：</span><span class="sxs-lookup"><span data-stu-id="22f01-167">The sample's implementation is shown below:</span></span>

[!code-csharp[Configuration builder](key-vault-configuration/samples/key-name-prefix-sample/2.x/Program.cs?name=snippet1&highlight=18)]

[!code-csharp[PrefixKeyVaultSecretManager](key-vault-configuration/samples/key-name-prefix-sample/2.x/Startup.cs?name=snippet1)]

<span data-ttu-id="22f01-168">`Load`逐一查看，找出有版本前置詞的保存庫祕密提供者演算法所呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="22f01-168">The `Load` method is called by a provider algorithm that iterates through the vault secrets to find the ones that have the version prefix.</span></span> <span data-ttu-id="22f01-169">當版本的前置詞上有`Load`，此演算法會使用`GetKey`方法傳回的密碼名稱的組態名稱。</span><span class="sxs-lookup"><span data-stu-id="22f01-169">When a version prefix is found with `Load`, the algorithm uses the `GetKey` method to return the configuration name of the secret name.</span></span> <span data-ttu-id="22f01-170">它會除去版本前置詞，從 祕密的名稱，並傳回載入的祕密名稱的其餘部分，將應用程式設定成名稱 / 值組。</span><span class="sxs-lookup"><span data-stu-id="22f01-170">It strips off the version prefix from the secret's name and returns the rest of the secret name for loading into the app's configuration name-value pairs.</span></span>

<span data-ttu-id="22f01-171">當您實作這種方法：</span><span class="sxs-lookup"><span data-stu-id="22f01-171">When you implement this approach:</span></span>

1. <span data-ttu-id="22f01-172">載入的金鑰保存庫祕密。</span><span class="sxs-lookup"><span data-stu-id="22f01-172">The key vault secrets are loaded.</span></span>
2. <span data-ttu-id="22f01-173">字串密碼`5000-AppSecret`會比對。</span><span class="sxs-lookup"><span data-stu-id="22f01-173">The string secret for `5000-AppSecret` is matched.</span></span>
3. <span data-ttu-id="22f01-174">版本中， `5000` （使用 dash)，移除離開的索引鍵名稱從`AppSecret`祕密值載入應用程式的設定。</span><span class="sxs-lookup"><span data-stu-id="22f01-174">The version, `5000` (with the dash), is stripped off of the key name leaving `AppSecret` to load with the secret value into the app's configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="22f01-175">您也可以提供您自己`KeyVaultClient`實作`AddAzureKeyVault`。</span><span class="sxs-lookup"><span data-stu-id="22f01-175">You can also provide your own `KeyVaultClient` implementation to `AddAzureKeyVault`.</span></span> <span data-ttu-id="22f01-176">提供自訂的用戶端，可讓您共用的用戶端的組態提供者與您的應用程式的其他部分之間的單一執行個體。</span><span class="sxs-lookup"><span data-stu-id="22f01-176">Supplying a custom client allows you to share a single instance of the client between the configuration provider and other parts of your app.</span></span>

1. <span data-ttu-id="22f01-177">建立金鑰保存庫，並設定 Azure Active Directory (Azure AD) 應用程式中的指導方針[開始使用 Azure Key Vault](https://azure.microsoft.com/documentation/articles/key-vault-get-started/)。</span><span class="sxs-lookup"><span data-stu-id="22f01-177">Create a key vault and set up Azure Active Directory (Azure AD) for the app following the guidance in [Get started with Azure Key Vault](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).</span></span>
   * <span data-ttu-id="22f01-178">將密碼新增至金鑰保存庫使用[AzureRM 金鑰保存庫 PowerShell 模組](/powershell/module/azurerm.keyvault)苀[PowerShell 資源庫](https://www.powershellgallery.com/packages/AzureRM.KeyVault)，則[Azure Key Vault REST API](/rest/api/keyvault/)，或[Azure 入口網站](https://portal.azure.com/)。</span><span class="sxs-lookup"><span data-stu-id="22f01-178">Add secrets to the key vault using the [AzureRM Key Vault PowerShell Module](/powershell/module/azurerm.keyvault) available from the [PowerShell Gallery](https://www.powershellgallery.com/packages/AzureRM.KeyVault), the [Azure Key Vault REST API](/rest/api/keyvault/), or the [Azure Portal](https://portal.azure.com/).</span></span> <span data-ttu-id="22f01-179">為建立祕密*手動*或是*憑證*祕密。</span><span class="sxs-lookup"><span data-stu-id="22f01-179">Secrets are created as either *Manual* or *Certificate* secrets.</span></span> <span data-ttu-id="22f01-180">*憑證*機密資料是應用程式和服務所使用的憑證，但不是支援的組態提供者。</span><span class="sxs-lookup"><span data-stu-id="22f01-180">*Certificate* secrets are certificates for use by apps and services but are not supported by the configuration provider.</span></span> <span data-ttu-id="22f01-181">您應該使用*手動*選項建立的組態提供者使用的名稱 / 值組祕密。</span><span class="sxs-lookup"><span data-stu-id="22f01-181">You should use the *Manual* option to create name-value pair secrets for use with the configuration provider.</span></span>
     * <span data-ttu-id="22f01-182">階層式的值 （組態區段） 使用`--`（兩個連字號） 做為分隔符號。</span><span class="sxs-lookup"><span data-stu-id="22f01-182">Hierarchical values (configuration sections) use `--` (two dashes) as a separator.</span></span>
     * <span data-ttu-id="22f01-183">建立兩個*手動*具有下列名稱 / 值組的祕密：</span><span class="sxs-lookup"><span data-stu-id="22f01-183">Create two *Manual* secrets with the following name-value pairs:</span></span>
       * <span data-ttu-id="22f01-184">`5000-AppSecret`: `5.0.0.0_secret_value`</span><span class="sxs-lookup"><span data-stu-id="22f01-184">`5000-AppSecret`: `5.0.0.0_secret_value`</span></span>
       * <span data-ttu-id="22f01-185">`5100-AppSecret`: `5.1.0.0_secret_value`</span><span class="sxs-lookup"><span data-stu-id="22f01-185">`5100-AppSecret`: `5.1.0.0_secret_value`</span></span>
   * <span data-ttu-id="22f01-186">向 Azure Active Directory 中註冊範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="22f01-186">Register the sample app with Azure Active Directory.</span></span>
   * <span data-ttu-id="22f01-187">授權應用程式存取金鑰保存庫。</span><span class="sxs-lookup"><span data-stu-id="22f01-187">Authorize the app to access the key vault.</span></span> <span data-ttu-id="22f01-188">當您使用`Set-AzureRmKeyVaultAccessPolicy`PowerShell cmdlet，授權應用程式存取金鑰保存庫中，提供`List`並`Get`祕密存取`-PermissionsToSecrets list,get`。</span><span class="sxs-lookup"><span data-stu-id="22f01-188">When you use the `Set-AzureRmKeyVaultAccessPolicy` PowerShell cmdlet to authorize the app to access the key vault, provide `List` and `Get` access to secrets with `-PermissionsToSecrets list,get`.</span></span>

2. <span data-ttu-id="22f01-189">應用程式的更新*appsettings.json*的值的檔案`Vault`， `ClientId`，和`ClientSecret`。</span><span class="sxs-lookup"><span data-stu-id="22f01-189">Update the app's *appsettings.json* file with the values of `Vault`, `ClientId`, and `ClientSecret`.</span></span>
3. <span data-ttu-id="22f01-190">執行範例應用程式，它會取得從其組態值`IConfigurationRoot`前置詞的祕密名稱同名。</span><span class="sxs-lookup"><span data-stu-id="22f01-190">Run the sample app, which obtains its configuration values from `IConfigurationRoot` with the same name as the prefixed secret name.</span></span> <span data-ttu-id="22f01-191">在此範例中，前置詞是應用程式的版本，您提供給`PrefixKeyVaultSecretManager`當您加入的 Azure Key Vault 組態提供者。</span><span class="sxs-lookup"><span data-stu-id="22f01-191">In this sample, the prefix is the app's version, which you provided to the `PrefixKeyVaultSecretManager` when you added the Azure Key Vault configuration provider.</span></span> <span data-ttu-id="22f01-192">值`AppSecret`取得`config["AppSecret"]`。</span><span class="sxs-lookup"><span data-stu-id="22f01-192">The value for `AppSecret` is obtained with `config["AppSecret"]`.</span></span> <span data-ttu-id="22f01-193">應用程式所產生的網頁顯示載入的值：</span><span class="sxs-lookup"><span data-stu-id="22f01-193">The webpage generated by the app shows the loaded value:</span></span>

   ![瀏覽器視窗中顯示應用程式的版本時 version=5.0.0.0，透過 「 Azure 金鑰保存庫組態提供者載入祕密值](key-vault-configuration/_static/sample2-1.png)

4. <span data-ttu-id="22f01-195">變更應用程式中的組件的專案檔版本`5.0.0.0`至`5.1.0.0`並再次執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="22f01-195">Change the version of the app assembly in the project file from `5.0.0.0` to `5.1.0.0` and run the app again.</span></span> <span data-ttu-id="22f01-196">這次，則傳回的祕密值是`5.1.0.0_secret_value`。</span><span class="sxs-lookup"><span data-stu-id="22f01-196">This time, the secret value returned is `5.1.0.0_secret_value`.</span></span> <span data-ttu-id="22f01-197">應用程式所產生的網頁顯示載入的值：</span><span class="sxs-lookup"><span data-stu-id="22f01-197">The webpage generated by the app shows the loaded value:</span></span>

   ![瀏覽器視窗中顯示 5.1.0.0 應用程式的版本時，透過 「 Azure 金鑰保存庫組態提供者載入祕密值](key-vault-configuration/_static/sample2-2.png)

## <a name="controlling-access-to-the-clientsecret"></a><span data-ttu-id="22f01-199">ClientSecret 控制存取</span><span class="sxs-lookup"><span data-stu-id="22f01-199">Controlling access to the ClientSecret</span></span>

<span data-ttu-id="22f01-200">使用[Secret Manager 工具](xref:security/app-secrets)維護`ClientSecret`外部專案來源樹狀結構。</span><span class="sxs-lookup"><span data-stu-id="22f01-200">Use the [Secret Manager tool](xref:security/app-secrets) to maintain the `ClientSecret` outside of your project source tree.</span></span> <span data-ttu-id="22f01-201">使用 Secret Manager 中，您將應用程式祕密與特定的專案產生關聯並加以共用跨多個專案。</span><span class="sxs-lookup"><span data-stu-id="22f01-201">With Secret Manager, you associate app secrets with a specific project and share them across multiple projects.</span></span>

<span data-ttu-id="22f01-202">在開發環境可支援憑證中的.NET Framework 應用程式時，您可以使用 X.509 憑證來驗證 Azure Key Vault。</span><span class="sxs-lookup"><span data-stu-id="22f01-202">When developing a .NET Framework app in an environment that supports certificates, you can authenticate to Azure Key Vault with an X.509 certificate.</span></span> <span data-ttu-id="22f01-203">X.509 憑證的私密金鑰是由 OS 管理。</span><span class="sxs-lookup"><span data-stu-id="22f01-203">The X.509 certificate's private key is managed by the OS.</span></span> <span data-ttu-id="22f01-204">如需詳細資訊，請參閱 <<c0> [ 使用而不是用戶端祕密憑證進行驗證](https://docs.microsoft.com/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret)。</span><span class="sxs-lookup"><span data-stu-id="22f01-204">For more information, see [Authenticate with a Certificate instead of a Client Secret](https://docs.microsoft.com/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret).</span></span> <span data-ttu-id="22f01-205">使用`AddAzureKeyVault`多載，接受`X509Certificate2`(`_env`在下列範例中：</span><span class="sxs-lookup"><span data-stu-id="22f01-205">Use the `AddAzureKeyVault` overload that accepts an `X509Certificate2` (`_env` in the following example :</span></span>

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

## <a name="reloading-secrets"></a><span data-ttu-id="22f01-206">重新載入祕密</span><span class="sxs-lookup"><span data-stu-id="22f01-206">Reloading secrets</span></span>

<span data-ttu-id="22f01-207">密碼會快取直到`IConfigurationRoot.Reload()`呼叫。</span><span class="sxs-lookup"><span data-stu-id="22f01-207">Secrets are cached until `IConfigurationRoot.Reload()` is called.</span></span> <span data-ttu-id="22f01-208">過期，停用，以及更新金鑰保存庫中的祕密未遵守應用程式，直到`Reload`執行。</span><span class="sxs-lookup"><span data-stu-id="22f01-208">Expired, disabled, and updated secrets in the key vault are not respected by the app until `Reload` is executed.</span></span>

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a><span data-ttu-id="22f01-209">已停用和到期的祕密</span><span class="sxs-lookup"><span data-stu-id="22f01-209">Disabled and expired secrets</span></span>

<span data-ttu-id="22f01-210">已停用及過期的密碼會擲回`KeyVaultClientException`。</span><span class="sxs-lookup"><span data-stu-id="22f01-210">Disabled and expired secrets throw a `KeyVaultClientException`.</span></span> <span data-ttu-id="22f01-211">若要防止您的應用程式擲回，取代您的應用程式或更新已停用/過期的祕密。</span><span class="sxs-lookup"><span data-stu-id="22f01-211">To prevent your app from throwing, replace your app or update the disabled/expired secret.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="22f01-212">疑難排解</span><span class="sxs-lookup"><span data-stu-id="22f01-212">Troubleshooting</span></span>

<span data-ttu-id="22f01-213">當應用程式無法載入組態使用的提供者時，錯誤訊息會寫入[ASP.NET 記錄基礎結構](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="22f01-213">When the app fails to load configuration using the provider, an error message is written to the [ASP.NET Logging infrastructure](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="22f01-214">在下列情況會造成無法載入組態：</span><span class="sxs-lookup"><span data-stu-id="22f01-214">The following conditions will prevent configuration from loading:</span></span>

* <span data-ttu-id="22f01-215">應用程式未正確設定 Azure Active Directory 中。</span><span class="sxs-lookup"><span data-stu-id="22f01-215">The app isn't configured correctly in Azure Active Directory.</span></span>
* <span data-ttu-id="22f01-216">金鑰保存庫不存在於 Azure 金鑰保存庫。</span><span class="sxs-lookup"><span data-stu-id="22f01-216">The key vault doesn't exist in Azure Key Vault.</span></span>
* <span data-ttu-id="22f01-217">應用程式未獲授權存取金鑰保存庫。</span><span class="sxs-lookup"><span data-stu-id="22f01-217">The app isn't authorized to access the key vault.</span></span>
* <span data-ttu-id="22f01-218">存取原則不包含`Get`和`List`權限。</span><span class="sxs-lookup"><span data-stu-id="22f01-218">The access policy doesn't include `Get` and `List` permissions.</span></span>
* <span data-ttu-id="22f01-219">在金鑰保存庫中，設定資料 （名稱 / 值組） 錯誤命名為，遺漏，停用，或已過期。</span><span class="sxs-lookup"><span data-stu-id="22f01-219">In the key vault, the configuration data (name-value pair) is incorrectly named, missing, disabled, or expired.</span></span>
* <span data-ttu-id="22f01-220">應用程式有錯誤的金鑰保存庫名稱 (`Vault`)，Azure AD 應用程式識別碼 (`ClientId`)，或 Azure AD 金鑰 (`ClientSecret`)。</span><span class="sxs-lookup"><span data-stu-id="22f01-220">The app has the wrong key vault name (`Vault`), Azure AD App Id (`ClientId`), or Azure AD Key (`ClientSecret`).</span></span>
* <span data-ttu-id="22f01-221">Azure AD 金鑰 (`ClientSecret`) 已過期。</span><span class="sxs-lookup"><span data-stu-id="22f01-221">The Azure AD Key (`ClientSecret`) is expired.</span></span>
* <span data-ttu-id="22f01-222">不正確的值，您想要載入的應用程式中設定金鑰 （名稱）。</span><span class="sxs-lookup"><span data-stu-id="22f01-222">The configuration key (name) is incorrect in the app for the value you're trying to load.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="22f01-223">其他資源</span><span class="sxs-lookup"><span data-stu-id="22f01-223">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* [<span data-ttu-id="22f01-224">Microsoft Azure： 金鑰保存庫</span><span class="sxs-lookup"><span data-stu-id="22f01-224">Microsoft Azure: Key Vault</span></span>](https://azure.microsoft.com/services/key-vault/)
* [<span data-ttu-id="22f01-225">Microsoft Azure: Key Vault 文件</span><span class="sxs-lookup"><span data-stu-id="22f01-225">Microsoft Azure: Key Vault Documentation</span></span>](/azure/key-vault/)
* [<span data-ttu-id="22f01-226">如何產生並傳輸受 HSM 保護金鑰的 Azure 金鑰保存庫</span><span class="sxs-lookup"><span data-stu-id="22f01-226">How to generate and transfer HSM-protected keys for Azure Key Vault</span></span>](/azure/key-vault/key-vault-hsm-protected-keys)
* [<span data-ttu-id="22f01-227">KeyVaultClient 類別</span><span class="sxs-lookup"><span data-stu-id="22f01-227">KeyVaultClient Class</span></span>](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
