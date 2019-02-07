---
title: ASP.NET Core 中的 azure Key Vault 組態提供者
author: guardrex
description: 了解如何使用 Azure 金鑰保存庫的組態提供者設定應用程式使用在執行階段載入的名稱 / 值組。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/28/2019
uid: security/key-vault-configuration
ms.openlocfilehash: d255321f6083747ce9b452e1efd4da5bc015bf64
ms.sourcegitcommit: 3c2ba9a0d833d2a096d9d800ba67a1a7f9491af0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/07/2019
ms.locfileid: "55854428"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a><span data-ttu-id="7b681-103">ASP.NET Core 中的 azure Key Vault 組態提供者</span><span class="sxs-lookup"><span data-stu-id="7b681-103">Azure Key Vault Configuration Provider in ASP.NET Core</span></span>

<span data-ttu-id="7b681-104">藉由[Luke Latham](https://github.com/guardrex)和[Andrew Stanton-nurse](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="7b681-104">By [Luke Latham](https://github.com/guardrex) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="7b681-105">本文件說明如何使用[Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/)組態提供者，從 Azure Key Vault 祕密載入應用程式組態值。</span><span class="sxs-lookup"><span data-stu-id="7b681-105">This document explains how to use the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) Configuration Provider to load app configuration values from Azure Key Vault secrets.</span></span> <span data-ttu-id="7b681-106">Azure Key Vault 是雲端式服務，可以協助使用者保護密碼編譯金鑰和應用程式和服務所使用的密碼。</span><span class="sxs-lookup"><span data-stu-id="7b681-106">Azure Key Vault is a cloud-based service that assists in safeguarding cryptographic keys and secrets used by apps and services.</span></span> <span data-ttu-id="7b681-107">ASP.NET Core 應用程式使用 Azure 金鑰保存庫的常見案例包括：</span><span class="sxs-lookup"><span data-stu-id="7b681-107">Common scenarios for using Azure Key Vault with ASP.NET Core apps include:</span></span>

* <span data-ttu-id="7b681-108">控制對敏感的組態資料的存取。</span><span class="sxs-lookup"><span data-stu-id="7b681-108">Controlling access to sensitive configuration data.</span></span>
* <span data-ttu-id="7b681-109">儲存組態資料時，符合 fips 需求 140-2 Level 2 驗證的硬體安全性模組 (HSM)。</span><span class="sxs-lookup"><span data-stu-id="7b681-109">Meeting the requirement for FIPS 140-2 Level 2 validated Hardware Security Modules (HSM's) when storing configuration data.</span></span>

<span data-ttu-id="7b681-110">此案例是適用於應用程式為目標的 ASP.NET Core 2.1 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="7b681-110">This scenario is available for apps that target ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="7b681-111">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7b681-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="packages"></a><span data-ttu-id="7b681-112">封裝</span><span class="sxs-lookup"><span data-stu-id="7b681-112">Packages</span></span>

<span data-ttu-id="7b681-113">若要使用 Azure 金鑰保存庫的組態提供者，將新增的套件參考[Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/)封裝。</span><span class="sxs-lookup"><span data-stu-id="7b681-113">To use the Azure Key Vault Configuration Provider, add a package reference to the [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) package.</span></span>

<span data-ttu-id="7b681-114">採用[管理 Azure 資源的身分識別](/azure/active-directory/managed-identities-azure-resources/overview)案例中，新增的套件參考[Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/)封裝。</span><span class="sxs-lookup"><span data-stu-id="7b681-114">To adopt the [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview) scenario, add a package reference to the [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) package.</span></span>

> [!NOTE]
> <span data-ttu-id="7b681-115">在本文撰寫之際，最新穩定版本`Microsoft.Azure.Services.AppAuthentication`，版本`1.0.3`，可讓您[系統指派給受控身分識別](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-worka-namehow-does-it-worka)。</span><span class="sxs-lookup"><span data-stu-id="7b681-115">At the time of writing, the latest stable release of `Microsoft.Azure.Services.AppAuthentication`, version `1.0.3`, provides support for [system-assigned managed identities](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-worka-namehow-does-it-worka).</span></span> <span data-ttu-id="7b681-116">支援*指派使用者給受控身分識別*位於`1.0.2-preview`封裝。</span><span class="sxs-lookup"><span data-stu-id="7b681-116">Support for *user-assigned managed identities* is available in the `1.0.2-preview` package.</span></span> <span data-ttu-id="7b681-117">本主題示範如何使用系統管理的身分識別，並提供的範例應用程式使用的版本`1.0.3`的`Microsoft.Azure.Services.AppAuthentication`封裝。</span><span class="sxs-lookup"><span data-stu-id="7b681-117">This topic demonstrates the use of system-managed identities, and the provided sample app uses version `1.0.3` of the `Microsoft.Azure.Services.AppAuthentication` package.</span></span>

## <a name="sample-app"></a><span data-ttu-id="7b681-118">範例應用程式</span><span class="sxs-lookup"><span data-stu-id="7b681-118">Sample app</span></span>

<span data-ttu-id="7b681-119">範例應用程式執行所決定的兩種模式之一`#define`陳述式，在頂端*Program.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="7b681-119">The sample app runs in either of two modes determined by the `#define` statement at the top of the *Program.cs* file:</span></span>

* <span data-ttu-id="7b681-120">`Basic` &ndash; 示範如何使用 Azure 金鑰保存庫的應用程式識別碼和密碼 （用戶端密碼） 來存取儲存在金鑰保存庫中的祕密。</span><span class="sxs-lookup"><span data-stu-id="7b681-120">`Basic` &ndash; Demonstrates the use of an Azure Key Vault Application ID and Password (Client Secret) to access secrets stored in the key vault.</span></span> <span data-ttu-id="7b681-121">部署`Basic`版本的任何主機能夠為 ASP.NET Core 應用程式範例。</span><span class="sxs-lookup"><span data-stu-id="7b681-121">Deploy the `Basic` version of the sample to any host capable of serving an ASP.NET Core app.</span></span> <span data-ttu-id="7b681-122">請依照下列中的指導方針[使用應用程式識別碼和非 Azure 代管應用程式的用戶端祕密](#use-application-id-and-client-secret-for-non-azure-hosted-apps)一節。</span><span class="sxs-lookup"><span data-stu-id="7b681-122">Follow the guidance in the [Use Application ID and Client Secret for non-Azure-hosted apps](#use-application-id-and-client-secret-for-non-azure-hosted-apps) section.</span></span>
* <span data-ttu-id="7b681-123">`Managed` &ndash; 示範如何使用[管理 Azure 資源的身分識別](/azure/active-directory/managed-identities-azure-resources/overview)驗證的應用程式至 Azure Key Vault 與 Azure AD 驗證不含認證儲存在應用程式的程式碼或組態。</span><span class="sxs-lookup"><span data-stu-id="7b681-123">`Managed` &ndash; Demonstrates how to use [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview) to authenticate the app to Azure Key Vault with Azure AD authentication without credentials stored in the app's code or configuration.</span></span> <span data-ttu-id="7b681-124">當使用受管理的身分識別進行驗證，不需要的 Azure AD 應用程式識別碼和密碼 （用戶端祕密）。</span><span class="sxs-lookup"><span data-stu-id="7b681-124">When using managed identities to authenticate, an Azure AD Application ID and Password (Client Secret) aren't required.</span></span> <span data-ttu-id="7b681-125">`Managed`範例版本必須部署至 Azure。</span><span class="sxs-lookup"><span data-stu-id="7b681-125">The `Managed` version of the sample must be deployed to Azure.</span></span> <span data-ttu-id="7b681-126">請依照下列中的指導方針[適用於 Azure 資源使用受控身分識別](#use-managed-identities-for-azure-resources)一節。</span><span class="sxs-lookup"><span data-stu-id="7b681-126">Follow the guidance in the [Use the Managed identities for Azure resources](#use-managed-identities-for-azure-resources) section.</span></span>

<span data-ttu-id="7b681-127">如需有關如何設定範例應用程式使用前置處理器指示詞 (`#define`)，請參閱<xref:index#preprocessor-directives-in-sample-code>。</span><span class="sxs-lookup"><span data-stu-id="7b681-127">For more information on how to configure a sample app using preprocessor directives (`#define`), see <xref:index#preprocessor-directives-in-sample-code>.</span></span>

## <a name="secret-storage-in-the-development-environment"></a><span data-ttu-id="7b681-128">在開發環境中的祕密儲存體</span><span class="sxs-lookup"><span data-stu-id="7b681-128">Secret storage in the Development environment</span></span>

<span data-ttu-id="7b681-129">設定在本機使用的祕密[Secret Manager 工具](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="7b681-129">Set secrets locally using the [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="7b681-130">當在開發環境中本機電腦上，執行範例應用程式時，會載入密碼從本機的祕密管理員存放區。</span><span class="sxs-lookup"><span data-stu-id="7b681-130">When the sample app runs on the local machine in the Development environment, secrets are loaded from the local Secret Manager store.</span></span>

<span data-ttu-id="7b681-131">Secret Manager 工具需要`<UserSecretsId>`應用程式的專案檔中的屬性。</span><span class="sxs-lookup"><span data-stu-id="7b681-131">The Secret Manager tool requires a `<UserSecretsId>` property in the app's project file.</span></span> <span data-ttu-id="7b681-132">設定屬性值 (`{GUID}`) 任何唯一的 guid:</span><span class="sxs-lookup"><span data-stu-id="7b681-132">Set the property value (`{GUID}`) to any unique GUID:</span></span>

```xml
<PropertyGroup>
  <UserSecretsId>{GUID}</UserSecretsId>
</PropertyGroup>
```

<span data-ttu-id="7b681-133">密碼會建立為名稱 / 值組。</span><span class="sxs-lookup"><span data-stu-id="7b681-133">Secrets are created as name-value pairs.</span></span> <span data-ttu-id="7b681-134">階層式的值 （組態區段） 會使用`:`（冒號） 為分隔符號[ASP.NET Core 組態](xref:fundamentals/configuration/index)索引鍵名稱。</span><span class="sxs-lookup"><span data-stu-id="7b681-134">Hierarchical values (configuration sections) use a `:` (colon) as a separator in [ASP.NET Core configuration](xref:fundamentals/configuration/index) key names.</span></span>

<span data-ttu-id="7b681-135">Secret Manager 可從命令殼層，開啟專案的內容根目錄，其中`{SECRET NAME}`名稱和`{SECRET VALUE}`的值：</span><span class="sxs-lookup"><span data-stu-id="7b681-135">The Secret Manager is used from a command shell opened to the project's content root, where `{SECRET NAME}` is the name and `{SECRET VALUE}` is the value:</span></span>

```console
dotnet user-secrets set "{SECRET NAME}" "{SECRET VALUE}"
```

<span data-ttu-id="7b681-136">從專案的內容根目錄設定範例應用程式祕密命令殼層中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="7b681-136">Execute the following commands in a command shell from the project's content root to set the secrets for the sample app:</span></span>

```console
dotnet user-secrets set "SecretName" "secret_value_1_dev"
dotnet user-secrets set "Section:SecretName" "secret_value_2_dev"
```

<span data-ttu-id="7b681-137">當這些祕密儲存在 Azure Key Vault[生產環境使用 Azure Key Vault 中密碼的儲存體](#secret-storage-in-the-production-environment-with-azure-key-vault)區段中，`_dev`後置詞變更為`_prod`。</span><span class="sxs-lookup"><span data-stu-id="7b681-137">When these secrets are stored in Azure Key Vault in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section, the `_dev` suffix is changed to `_prod`.</span></span> <span data-ttu-id="7b681-138">後置詞提供視覺提示，指出組態值的來源的應用程式的輸出中。</span><span class="sxs-lookup"><span data-stu-id="7b681-138">The suffix provides a visual cue in the app's output indicating the source of the configuration values.</span></span>

## <a name="secret-storage-in-the-production-environment-with-azure-key-vault"></a><span data-ttu-id="7b681-139">在生產環境使用 Azure Key Vault 的祕密儲存體</span><span class="sxs-lookup"><span data-stu-id="7b681-139">Secret storage in the Production environment with Azure Key Vault</span></span>

<span data-ttu-id="7b681-140">所提供的指示[快速入門：設定並從使用 Azure CLI 的 Azure 金鑰保存庫擷取祕密](/azure/key-vault/quick-create-cli)主題會彙總以供建立 Azure Key Vault 及儲存範例應用程式所使用的密碼。</span><span class="sxs-lookup"><span data-stu-id="7b681-140">The instructions provided by the [Quickstart: Set and retrieve a secret from Azure Key Vault using Azure CLI](/azure/key-vault/quick-create-cli) topic are summarized here for creating an Azure Key Vault and storing secrets used by the sample app.</span></span> <span data-ttu-id="7b681-141">請參閱本主題以取得詳細資料。</span><span class="sxs-lookup"><span data-stu-id="7b681-141">Refer to the topic for further details.</span></span>

1. <span data-ttu-id="7b681-142">開啟 Azure Cloud shell 中使用中的下列方法的任一[Azure 入口網站](https://portal.azure.com/):</span><span class="sxs-lookup"><span data-stu-id="7b681-142">Open Azure Cloud shell using any one of the following methods in the [Azure portal](https://portal.azure.com/):</span></span>

   * <span data-ttu-id="7b681-143">選取 **試試**的程式碼區塊右上角。</span><span class="sxs-lookup"><span data-stu-id="7b681-143">Select **Try It** in the upper-right corner of a code block.</span></span> <span data-ttu-id="7b681-144">使用搜尋字串"Azure CLI 」，在文字方塊中。</span><span class="sxs-lookup"><span data-stu-id="7b681-144">Use the search string "Azure CLI" in the text box.</span></span>
   * <span data-ttu-id="7b681-145">在您的瀏覽器中開啟 Cloud Shell**啟動 Cloud Shell**  按鈕。</span><span class="sxs-lookup"><span data-stu-id="7b681-145">Open Cloud Shell in your browser with the **Launch Cloud Shell** button.</span></span>
   * <span data-ttu-id="7b681-146">選取  **Cloud Shell**在 Azure 入口網站右上角的功能表上的按鈕。</span><span class="sxs-lookup"><span data-stu-id="7b681-146">Select the **Cloud Shell** button on the menu in the upper-right corner of the Azure portal.</span></span>

   <span data-ttu-id="7b681-147">如需詳細資訊，請參閱 < [Azure 命令列介面 (CLI)](/cli/azure/)並[Azure Cloud Shell 概觀](/azure/cloud-shell/overview)。</span><span class="sxs-lookup"><span data-stu-id="7b681-147">For more information, see [Azure Command-Line Interface (CLI)](/cli/azure/) and [Overview of Azure Cloud Shell](/azure/cloud-shell/overview).</span></span>

1. <span data-ttu-id="7b681-148">如果您不已通過驗證，登入的`az login`命令。</span><span class="sxs-lookup"><span data-stu-id="7b681-148">If you aren't already authenticated, sign in with the `az login` command.</span></span>

1. <span data-ttu-id="7b681-149">建立資源群組，使用下列命令，其中`{RESOURCE GROUP NAME}`是新的資源群組的資源群組名稱和`{LOCATION}`是 Azure 區域 （資料中心）：</span><span class="sxs-lookup"><span data-stu-id="7b681-149">Create a resource group with the following command, where `{RESOURCE GROUP NAME}` is the resource group name for the new resource group and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```console
   az group create --name "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="7b681-150">在資源群組中，使用下列命令，建立金鑰保存庫所在`{KEY VAULT NAME}`是新的金鑰保存庫的名稱和`{LOCATION}`是 Azure 區域 （資料中心）：</span><span class="sxs-lookup"><span data-stu-id="7b681-150">Create a key vault in the resource group with the following command, where `{KEY VAULT NAME}` is the name for the new key vault and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```console
   az keyvault create --name "{KEY VAULT NAME}" --resource-group "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="7b681-151">建立金鑰保存庫中的密碼，做為名稱 / 值組。</span><span class="sxs-lookup"><span data-stu-id="7b681-151">Create secrets in the key vault as name-value pairs.</span></span>

   <span data-ttu-id="7b681-152">Azure Key Vault 祕密名稱必須是限制為英數字元及虛線。</span><span class="sxs-lookup"><span data-stu-id="7b681-152">Azure Key Vault secret names are limited to alphanumeric characters and dashes.</span></span> <span data-ttu-id="7b681-153">階層式的值 （組態區段） 使用`--`（兩個連字號） 做為分隔符號。</span><span class="sxs-lookup"><span data-stu-id="7b681-153">Hierarchical values (configuration sections) use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="7b681-154">通常用來分隔的區段中的子機碼的冒號[ASP.NET Core 組態](xref:fundamentals/configuration/index)，不允許在金鑰保存庫祕密名稱。</span><span class="sxs-lookup"><span data-stu-id="7b681-154">Colons, which are normally used to delimit a section from a subkey in [ASP.NET Core configuration](xref:fundamentals/configuration/index), aren't allowed in key vault secret names.</span></span> <span data-ttu-id="7b681-155">因此，兩個連字號是用，而且密碼會載入應用程式的設定時，已還原為冒號。</span><span class="sxs-lookup"><span data-stu-id="7b681-155">Therefore, two dashes are used and swapped for a colon when the secrets are loaded into the app's configuration.</span></span>

   <span data-ttu-id="7b681-156">下列的機密資料是範例應用程式搭配使用。</span><span class="sxs-lookup"><span data-stu-id="7b681-156">The following secrets are for use with the sample app.</span></span> <span data-ttu-id="7b681-157">值包括`_prod`後置詞來區別從`_dev`後置詞從使用者的機密資訊的開發環境中載入的值。</span><span class="sxs-lookup"><span data-stu-id="7b681-157">The values include a `_prod` suffix to distinguish them from the `_dev` suffix values loaded in the Development environment from User Secrets.</span></span> <span data-ttu-id="7b681-158">取代`{KEY VAULT NAME}`與您在先前步驟中建立的金鑰保存庫名稱：</span><span class="sxs-lookup"><span data-stu-id="7b681-158">Replace `{KEY VAULT NAME}` with the name of the key vault that you created in the prior step:</span></span>

   ```console
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "SecretName" --value "secret_value_1_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "Section--SecretName" --value "secret_value_2_prod"
   ```

## <a name="use-application-id-and-client-secret-for-non-azure-hosted-apps"></a><span data-ttu-id="7b681-159">使用非 Azure 代管應用程式的應用程式識別碼和用戶端祕密</span><span class="sxs-lookup"><span data-stu-id="7b681-159">Use Application ID and Client Secret for non-Azure-hosted apps</span></span>

<span data-ttu-id="7b681-160">設定 Azure AD、 Azure 金鑰保存庫和應用程式使用的應用程式識別碼和密碼 （用戶端密碼） 來驗證 key vault**當應用程式裝載在 Azure 之外**。</span><span class="sxs-lookup"><span data-stu-id="7b681-160">Configure Azure AD, Azure Key Vault, and the app to use an Application ID and Password (Client Secret) to authenticate to a key vault **when the app is hosted outside of Azure**.</span></span>

> [!NOTE]
> <span data-ttu-id="7b681-161">雖然在 Azure 中託管的應用程式支援使用應用程式識別碼和密碼 （用戶端秘密），我們建議您使用[管理 Azure 資源的身分識別](#use-managed-identities-for-azure-resources)裝載在 Azure 中的應用程式時。</span><span class="sxs-lookup"><span data-stu-id="7b681-161">Although using an Application ID and Password (Client Secret) is supported for apps hosted in Azure, we recommend using [Managed identities for Azure resources](#use-managed-identities-for-azure-resources) when hosting an app in Azure.</span></span> <span data-ttu-id="7b681-162">受管理的身分識別會需要將認證儲存在應用程式或其組態，因此被視為通常更安全的方法。</span><span class="sxs-lookup"><span data-stu-id="7b681-162">Managed identities require storing credentials in the app or its configuration, so it's regarded as a generally safer approach.</span></span>

<span data-ttu-id="7b681-163">範例應用程式可使用的應用程式識別碼和密碼 （用戶端密碼） 時`#define`陳述式，在頂端*Program.cs*檔案設定為`Basic`。</span><span class="sxs-lookup"><span data-stu-id="7b681-163">The sample app uses an Application ID and Password (Client Secret) when the `#define` statement at the top of the *Program.cs* file is set to `Basic`.</span></span>

1. <span data-ttu-id="7b681-164">使用 Azure AD 註冊應用程式，並建立應用程式的身分識別的密碼 （用戶端祕密）。</span><span class="sxs-lookup"><span data-stu-id="7b681-164">Register the app with Azure AD and establish a Password (Client Secret) for the app's identity.</span></span>
1. <span data-ttu-id="7b681-165">在應用程式中儲存的金鑰保存庫名稱、 應用程式識別碼和密碼/用戶端祕密*appsettings.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="7b681-165">Store the key vault name, Application ID, and Password/Client Secret in the app's *appsettings.json* file.</span></span>
1. <span data-ttu-id="7b681-166">瀏覽至**金鑰保存庫**在 Azure 入口網站中。</span><span class="sxs-lookup"><span data-stu-id="7b681-166">Navigate to **Key vaults** in the Azure portal.</span></span>
1. <span data-ttu-id="7b681-167">選取您在建立金鑰保存庫[生產環境使用 Azure Key Vault 中密碼的儲存體](#secret-storage-in-the-production-environment-with-azure-key-vault)一節。</span><span class="sxs-lookup"><span data-stu-id="7b681-167">Select the key vault that you created in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section.</span></span>
1. <span data-ttu-id="7b681-168">選取 **存取原則**。</span><span class="sxs-lookup"><span data-stu-id="7b681-168">Select **Access policies**.</span></span>
1. <span data-ttu-id="7b681-169">選取 **新增**。</span><span class="sxs-lookup"><span data-stu-id="7b681-169">Select **Add new**.</span></span>
1. <span data-ttu-id="7b681-170">選取 **選取主體**依名稱選取的已註冊的應用程式。</span><span class="sxs-lookup"><span data-stu-id="7b681-170">Select **Select principal** and select the registered app by name.</span></span> <span data-ttu-id="7b681-171">選取 [**選取**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7b681-171">Select the **Select** button.</span></span>
1. <span data-ttu-id="7b681-172">開啟**祕密權限**，並提供應用程式與**取得**並**清單**權限。</span><span class="sxs-lookup"><span data-stu-id="7b681-172">Open **Secret permissions** and provide the app with **Get** and **List** permissions.</span></span>
1. <span data-ttu-id="7b681-173">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="7b681-173">Select **OK**.</span></span>
1. <span data-ttu-id="7b681-174">選取 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="7b681-174">Select **Save**.</span></span>
1. <span data-ttu-id="7b681-175">部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="7b681-175">Deploy the app.</span></span>

<span data-ttu-id="7b681-176">`Basic`範例應用程式取得其組態值從`IConfigurationRoot`具有相同名稱與祕密名稱：</span><span class="sxs-lookup"><span data-stu-id="7b681-176">The `Basic` sample app obtains its configuration values from `IConfigurationRoot` with the same name as the secret name:</span></span>

* <span data-ttu-id="7b681-177">非階層式的值：值`SecretName`取得`config["SecretName"]`。</span><span class="sxs-lookup"><span data-stu-id="7b681-177">Non-hierarchical values: The value for `SecretName` is obtained with `config["SecretName"]`.</span></span>
* <span data-ttu-id="7b681-178">（區段） 的階層式值：使用`:`（冒號） 標記法或`GetSection`擴充方法。</span><span class="sxs-lookup"><span data-stu-id="7b681-178">Hierarchical values (sections): Use `:` (colon) notation or the `GetSection` extension method.</span></span> <span data-ttu-id="7b681-179">您可以使用任一種方法來取得組態值：</span><span class="sxs-lookup"><span data-stu-id="7b681-179">Use either of these approaches to obtain the configuration value:</span></span>
  * `config["Section:SecretName"]`
  * `config.GetSection("Section")["SecretName"]`

<span data-ttu-id="7b681-180">應用程式呼叫`AddAzureKeyVault`所提供的值*appsettings.json*檔案：</span><span class="sxs-lookup"><span data-stu-id="7b681-180">The app calls `AddAzureKeyVault` with values supplied by the *appsettings.json* file:</span></span>

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet1&highlight=11-14)]

<span data-ttu-id="7b681-181">範例值：</span><span class="sxs-lookup"><span data-stu-id="7b681-181">Example values:</span></span>

* <span data-ttu-id="7b681-182">金鑰保存庫名稱： `contosovault`</span><span class="sxs-lookup"><span data-stu-id="7b681-182">Key vault name: `contosovault`</span></span>
* <span data-ttu-id="7b681-183">應用程式識別碼： `627e911e-43cc-61d4-992e-12db9c81b413`</span><span class="sxs-lookup"><span data-stu-id="7b681-183">Application ID: `627e911e-43cc-61d4-992e-12db9c81b413`</span></span>
* <span data-ttu-id="7b681-184">密碼： `g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di=`</span><span class="sxs-lookup"><span data-stu-id="7b681-184">Password: `g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di=`</span></span>

<span data-ttu-id="7b681-185">*appsettings.json*：</span><span class="sxs-lookup"><span data-stu-id="7b681-185">*appsettings.json*:</span></span>

[!code-json[](key-vault-configuration/sample/appsettings.json)]

<span data-ttu-id="7b681-186">當您執行應用程式時，網頁就會顯示載入的祕密值。</span><span class="sxs-lookup"><span data-stu-id="7b681-186">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="7b681-187">使用，在開發環境中，載入祕密值`_dev`後置詞。</span><span class="sxs-lookup"><span data-stu-id="7b681-187">In the Development environment, secret values load with the `_dev` suffix.</span></span> <span data-ttu-id="7b681-188">在生產環境中，值則是使用載入`_prod`後置詞。</span><span class="sxs-lookup"><span data-stu-id="7b681-188">In the Production environment, the values load with the `_prod` suffix.</span></span>

## <a name="use-managed-identities-for-azure-resources"></a><span data-ttu-id="7b681-189">適用於 Azure 資源的使用受管理身分識別</span><span class="sxs-lookup"><span data-stu-id="7b681-189">Use Managed identities for Azure resources</span></span>

<span data-ttu-id="7b681-190">**應用程式部署至 Azure**可以充分善用[管理適用於 Azure 資源的身分識別](/azure/active-directory/managed-identities-azure-resources/overview)，可讓應用程式，以向 Azure Key Vault 使用不含認證的 Azure AD 驗證 (應用程式識別碼和Password/Client 密碼) 儲存在應用程式。</span><span class="sxs-lookup"><span data-stu-id="7b681-190">**An app deployed to Azure** can take advantage of [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview), which allows the app to authenticate with Azure Key Vault using Azure AD authentication without credentials (Application ID and Password/Client Secret) stored in the app.</span></span>

<span data-ttu-id="7b681-191">範例應用程式會使用適用於 Azure 資源的受管理身分識別時`#define`陳述式，在頂端*Program.cs*檔案設定為`Managed`。</span><span class="sxs-lookup"><span data-stu-id="7b681-191">The sample app uses Managed identities for Azure resources when the `#define` statement at the top of the *Program.cs* file is set to `Managed`.</span></span>

<span data-ttu-id="7b681-192">輸入應用程式的保存庫名稱*appsettings.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="7b681-192">Enter the vault name into the app's *appsettings.json* file.</span></span> <span data-ttu-id="7b681-193">範例應用程式不需要應用程式識別碼和密碼 （用戶端秘密），當設定為`Managed`版本，因此您可以忽略這些組態項目。</span><span class="sxs-lookup"><span data-stu-id="7b681-193">The sample app doesn't require an Application ID and Password (Client Secret) when set to the `Managed` version, so you can ignore those configuration entries.</span></span> <span data-ttu-id="7b681-194">應用程式部署至 Azure，以及 Azure 驗證應用程式存取只使用 保存庫名稱儲存在 Azure Key Vault *appsettings.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="7b681-194">The app is deployed to Azure, and Azure authenticates the app to access Azure Key Vault only using the vault name stored in the *appsettings.json* file.</span></span>

<span data-ttu-id="7b681-195">將範例應用程式部署至 Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="7b681-195">Deploy the sample app to Azure App Service.</span></span>

<span data-ttu-id="7b681-196">應用程式部署至 Azure App Service 與 Azure AD 建立服務時，會自動註冊。</span><span class="sxs-lookup"><span data-stu-id="7b681-196">An app deployed to Azure App Service is automatically registered with Azure AD when the service is created.</span></span> <span data-ttu-id="7b681-197">從下列命令中使用的部署中取得的物件識別碼。</span><span class="sxs-lookup"><span data-stu-id="7b681-197">Obtain the Object ID from the deployment for use in the following command.</span></span> <span data-ttu-id="7b681-198">物件識別碼時，會顯示在 Azure 入口網站上，在**識別**App Service 的面板。</span><span class="sxs-lookup"><span data-stu-id="7b681-198">The Object ID is shown in the Azure portal on the **Identity** panel of the App Service.</span></span>

<span data-ttu-id="7b681-199">使用 Azure CLI 和應用程式的物件識別碼，提供應用程式與`list`和`get`存取金鑰保存庫的權限：</span><span class="sxs-lookup"><span data-stu-id="7b681-199">Using Azure CLI and the app's Object ID, provide the app with `list` and `get` permissions to access the key vault:</span></span>

```console
az keyvault set-policy --name '{KEY VAULT NAME}' --object-id {OBJECT ID} --secret-permissions get list
```

<span data-ttu-id="7b681-200">**重新啟動應用程式**使用 Azure CLI、 PowerShell 或 Azure 入口網站。</span><span class="sxs-lookup"><span data-stu-id="7b681-200">**Restart the app** using Azure CLI, PowerShell, or the Azure portal.</span></span>

<span data-ttu-id="7b681-201">範例應用程式：</span><span class="sxs-lookup"><span data-stu-id="7b681-201">The sample app:</span></span>

* <span data-ttu-id="7b681-202">建立的執行個體`AzureServiceTokenProvider`類別，而不需要的連接字串。</span><span class="sxs-lookup"><span data-stu-id="7b681-202">Creates an instance of the `AzureServiceTokenProvider` class without a connection string.</span></span> <span data-ttu-id="7b681-203">當未提供的連接字串時，提供者會嘗試從適用於 Azure 資源的受管理身分識別取得存取權杖。</span><span class="sxs-lookup"><span data-stu-id="7b681-203">When a connection string isn't provided, the provider attempts to obtain an access token from Managed identities for Azure resources.</span></span>
* <span data-ttu-id="7b681-204">新`KeyVaultClient`會透過`AzureServiceTokenProvider`執行個體權杖回呼。</span><span class="sxs-lookup"><span data-stu-id="7b681-204">A new `KeyVaultClient` is created with the `AzureServiceTokenProvider` instance token callback.</span></span>
* <span data-ttu-id="7b681-205">`KeyVaultClient`的預設實作會使用執行個體`IKeyVaultSecretManager`載入所有祕密的值，會取代雙連字號 (`--`) 加上冒號 (`:`) 中索引鍵名稱。</span><span class="sxs-lookup"><span data-stu-id="7b681-205">The `KeyVaultClient` instance is used with a default implementation of `IKeyVaultSecretManager` that loads all secret values and replaces double-dashes (`--`) with colons (`:`) in key names.</span></span>

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet2&highlight=13-21)]

<span data-ttu-id="7b681-206">當您執行應用程式時，網頁就會顯示載入的祕密值。</span><span class="sxs-lookup"><span data-stu-id="7b681-206">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="7b681-207">在開發環境中，有祕密值`_dev`後置詞，因為它們由使用者祕密。</span><span class="sxs-lookup"><span data-stu-id="7b681-207">In the Development environment, secret values have the `_dev` suffix because they're provided by User Secrets.</span></span> <span data-ttu-id="7b681-208">在生產環境中，值則是使用載入`_prod`後置詞，因為它們由 Azure 金鑰保存庫。</span><span class="sxs-lookup"><span data-stu-id="7b681-208">In the Production environment, the values load with the `_prod` suffix because they're provided by Azure Key Vault.</span></span>

<span data-ttu-id="7b681-209">如果您收到`Access denied`錯誤，確認已向 Azure AD 註冊應用程式，且提供存取金鑰保存庫。</span><span class="sxs-lookup"><span data-stu-id="7b681-209">If you receive an `Access denied` error, confirm that the app is registered with Azure AD and provided access to the key vault.</span></span> <span data-ttu-id="7b681-210">請確認您已重新開啟的服務在 Azure 中。</span><span class="sxs-lookup"><span data-stu-id="7b681-210">Confirm that you've restarted the service in Azure.</span></span>

## <a name="use-a-key-name-prefix"></a><span data-ttu-id="7b681-211">使用索引鍵名稱前置詞</span><span class="sxs-lookup"><span data-stu-id="7b681-211">Use a key name prefix</span></span>

<span data-ttu-id="7b681-212">`AddAzureKeyVault` 提供的多載，接受的實作`IKeyVaultSecretManager`，這可讓您控制如何金鑰保存庫祕密會轉換成設定的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="7b681-212">`AddAzureKeyVault` provides an overload that accepts an implementation of `IKeyVaultSecretManager`, which allows you to control how key vault secrets are converted into configuration keys.</span></span> <span data-ttu-id="7b681-213">比方說，您可以實作的介面，將根據您在應用程式啟動時所提供的前置詞值的祕密值。</span><span class="sxs-lookup"><span data-stu-id="7b681-213">For example, you can implement the interface to load secret values based on a prefix value you provide at app startup.</span></span> <span data-ttu-id="7b681-214">這可讓您，比方說，若要載入的應用程式的版本為基礎的祕密。</span><span class="sxs-lookup"><span data-stu-id="7b681-214">This allows you, for example, to load secrets based on the version of the app.</span></span>

> [!WARNING]
> <span data-ttu-id="7b681-215">若要將多個應用程式的祕密放入相同的金鑰保存庫，或將環境的密碼，請勿使用前置詞上的金鑰保存庫祕密 (例如*開發*與*生產*祕密) 置於同一個保存庫。</span><span class="sxs-lookup"><span data-stu-id="7b681-215">Don't use prefixes on key vault secrets to place secrets for multiple apps into the same key vault or to place environmental secrets (for example, *development* versus *production* secrets) into the same vault.</span></span> <span data-ttu-id="7b681-216">我們建議，不同的應用程式和開發/生產環境使用不同的金鑰保存庫來隔離應用程式環境，以最高層級的安全性。</span><span class="sxs-lookup"><span data-stu-id="7b681-216">We recommend that different apps and development/production environments use separate key vaults to isolate app environments for the highest level of security.</span></span>

<span data-ttu-id="7b681-217">在下列範例中，祕密建立金鑰保存庫 」 （並使用 Secret Manager 工具開發環境） 的`5000-AppSecret`（期間不允許在金鑰保存庫祕密名稱）。</span><span class="sxs-lookup"><span data-stu-id="7b681-217">In the following example, a secret is established in the key vault (and using the Secret Manager tool for the Development environment) for `5000-AppSecret` (periods aren't allowed in key vault secret names).</span></span> <span data-ttu-id="7b681-218">此祕密表示 version=5.0.0.0 新版應用程式的應用程式祕密。</span><span class="sxs-lookup"><span data-stu-id="7b681-218">This secret represents an app secret for version 5.0.0.0 of the app.</span></span> <span data-ttu-id="7b681-219">應用程式，5.1.0.0，另一個版本的祕密加入至金鑰保存庫 （並使用 Secret Manager 工具） 的`5100-AppSecret`。</span><span class="sxs-lookup"><span data-stu-id="7b681-219">For another version of the app, 5.1.0.0, a secret is added to the key vault (and using the Secret Manager tool) for `5100-AppSecret`.</span></span> <span data-ttu-id="7b681-220">每個應用程式版本載入其組態設為其建立版本的祕密值`AppSecret`、 清除關閉版本載入祕密。</span><span class="sxs-lookup"><span data-stu-id="7b681-220">Each app version loads its versioned secret value into its configuration as `AppSecret`, stripping off the version as it loads the secret.</span></span>

<span data-ttu-id="7b681-221">`AddAzureKeyVault` 自訂呼叫`IKeyVaultSecretManager`:</span><span class="sxs-lookup"><span data-stu-id="7b681-221">`AddAzureKeyVault` is called with a custom `IKeyVaultSecretManager`:</span></span>

[!code-csharp[](key-vault-configuration/sample_snapshot/Program.cs?name=snippet1&highlight=22)]

<span data-ttu-id="7b681-222">金鑰保存庫名稱、 應用程式識別碼和密碼 （用戶端祕密） 的值由*appsettings.json*檔案：</span><span class="sxs-lookup"><span data-stu-id="7b681-222">The values for key vault name, Application ID, and Password (Client Secret) are provided by the *appsettings.json* file:</span></span>

[!code-json[](key-vault-configuration/sample/appsettings.json)]

<span data-ttu-id="7b681-223">範例值：</span><span class="sxs-lookup"><span data-stu-id="7b681-223">Example values:</span></span>

* <span data-ttu-id="7b681-224">金鑰保存庫名稱： `contosovault`</span><span class="sxs-lookup"><span data-stu-id="7b681-224">Key vault name: `contosovault`</span></span>
* <span data-ttu-id="7b681-225">應用程式識別碼： `627e911e-43cc-61d4-992e-12db9c81b413`</span><span class="sxs-lookup"><span data-stu-id="7b681-225">Application ID: `627e911e-43cc-61d4-992e-12db9c81b413`</span></span>
* <span data-ttu-id="7b681-226">密碼： `g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di=`</span><span class="sxs-lookup"><span data-stu-id="7b681-226">Password: `g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di=`</span></span>

<span data-ttu-id="7b681-227">`IKeyVaultSecretManager`實作回應組態中載入適當的祕密的祕密版本前置詞：</span><span class="sxs-lookup"><span data-stu-id="7b681-227">The `IKeyVaultSecretManager` implementation reacts to the version prefixes of secrets to load the proper secret into configuration:</span></span>

[!code-csharp[](key-vault-configuration/sample_snapshot/Startup.cs?name=snippet1)]

<span data-ttu-id="7b681-228">`Load`逐一查看，找出有版本前置詞的保存庫祕密提供者演算法所呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="7b681-228">The `Load` method is called by a provider algorithm that iterates through the vault secrets to find the ones that have the version prefix.</span></span> <span data-ttu-id="7b681-229">當版本的前置詞上有`Load`，此演算法會使用`GetKey`方法傳回的密碼名稱的組態名稱。</span><span class="sxs-lookup"><span data-stu-id="7b681-229">When a version prefix is found with `Load`, the algorithm uses the `GetKey` method to return the configuration name of the secret name.</span></span> <span data-ttu-id="7b681-230">它會除去版本前置詞，從 祕密的名稱，並傳回載入的祕密名稱的其餘部分，將應用程式設定成名稱 / 值組。</span><span class="sxs-lookup"><span data-stu-id="7b681-230">It strips off the version prefix from the secret's name and returns the rest of the secret name for loading into the app's configuration name-value pairs.</span></span>

<span data-ttu-id="7b681-231">當這個方法的實作方式：</span><span class="sxs-lookup"><span data-stu-id="7b681-231">When this approach is implemented:</span></span>

1. <span data-ttu-id="7b681-232">應用程式的專案檔中指定的應用程式的版本。</span><span class="sxs-lookup"><span data-stu-id="7b681-232">The app's version specified in the app's project file.</span></span> <span data-ttu-id="7b681-233">在下列範例中，應用程式的版本設為`5.0.0.0`:</span><span class="sxs-lookup"><span data-stu-id="7b681-233">In the following example, the app's version is set to `5.0.0.0`:</span></span>

   ```xml
   <PropertyGroup>
     <Version>5.0.0.0</Version>
   </PropertyGroup>
   ```

1. <span data-ttu-id="7b681-234">確認`<UserSecretsId>`屬性會出現在應用程式的專案檔案，其中`{GUID}`是使用者所提供的 GUID:</span><span class="sxs-lookup"><span data-stu-id="7b681-234">Confirm that a `<UserSecretsId>` property is present in the app's project file, where `{GUID}` is a user-supplied GUID:</span></span>

   ```xml
   <PropertyGroup>
     <UserSecretsId>{GUID}</UserSecretsId>
   </PropertyGroup>
   ```

   <span data-ttu-id="7b681-235">儲存在本機使用下列的祕密[Secret Manager 工具](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="7b681-235">Save the following secrets locally with the [Secret Manager tool](xref:security/app-secrets):</span></span>

   ```console
   dotnet user-secrets set "5000-AppSecret" "5.0.0.0_secret_value_dev"
   dotnet user-secrets set "5100-AppSecret" "5.1.0.0_secret_value_dev"
   ```

1. <span data-ttu-id="7b681-236">密碼會儲存在 Azure Key Vault 中使用下列 Azure CLI 命令：</span><span class="sxs-lookup"><span data-stu-id="7b681-236">Secrets are saved in Azure Key Vault using the following Azure CLI commands:</span></span>

   ```console
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5000-AppSecret" --value "5.0.0.0_secret_value_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5100-AppSecret" --value "5.1.0.0_secret_value_prod"
   ```

1. <span data-ttu-id="7b681-237">執行應用程式時，會載入的金鑰保存庫祕密。</span><span class="sxs-lookup"><span data-stu-id="7b681-237">When the app is run, the key vault secrets are loaded.</span></span> <span data-ttu-id="7b681-238">字串密碼`5000-AppSecret`會對應到應用程式的專案檔中指定的應用程式的版本 (`5.0.0.0`)。</span><span class="sxs-lookup"><span data-stu-id="7b681-238">The string secret for `5000-AppSecret` is matched to the app's version specified in the app's project file (`5.0.0.0`).</span></span>

1. <span data-ttu-id="7b681-239">版本， `5000` （與 dash)，移除的索引鍵名稱。</span><span class="sxs-lookup"><span data-stu-id="7b681-239">The version, `5000` (with the dash), is stripped from the key name.</span></span> <span data-ttu-id="7b681-240">在整個應用程式，讀取具有索引鍵的組態`AppSecret`載入祕密的值。</span><span class="sxs-lookup"><span data-stu-id="7b681-240">Throughout the app, reading configuration with the key `AppSecret` loads the secret value.</span></span>

1. <span data-ttu-id="7b681-241">如果應用程式的版本會變更專案檔案中`5.1.0.0`再次執行應用程式，則傳回的祕密值是`5.1.0.0_secret_value_dev`在開發環境和`5.1.0.0_secret_value_prod`在生產環境中。</span><span class="sxs-lookup"><span data-stu-id="7b681-241">If the app's version is changed in the project file to `5.1.0.0` and the app is run again, the secret value returned is `5.1.0.0_secret_value_dev` in the Development environment and `5.1.0.0_secret_value_prod` in Production.</span></span>

> [!NOTE]
> <span data-ttu-id="7b681-242">您也可以提供您自己`KeyVaultClient`實作`AddAzureKeyVault`。</span><span class="sxs-lookup"><span data-stu-id="7b681-242">You can also provide your own `KeyVaultClient` implementation to `AddAzureKeyVault`.</span></span> <span data-ttu-id="7b681-243">自訂用戶端會允許跨應用程式共用用戶端的單一執行個體。</span><span class="sxs-lookup"><span data-stu-id="7b681-243">A custom client permits sharing a single instance of the client across the app.</span></span>

## <a name="authenticate-to-azure-key-vault-with-an-x509-certificate"></a><span data-ttu-id="7b681-244">Azure 金鑰保存庫中的 X.509 憑證進行驗證</span><span class="sxs-lookup"><span data-stu-id="7b681-244">Authenticate to Azure Key Vault with an X.509 certificate</span></span>

<span data-ttu-id="7b681-245">在開發環境可支援憑證中的.NET Framework 應用程式時，您可以使用 X.509 憑證來驗證 Azure Key Vault。</span><span class="sxs-lookup"><span data-stu-id="7b681-245">When developing a .NET Framework app in an environment that supports certificates, you can authenticate to Azure Key Vault with an X.509 certificate.</span></span> <span data-ttu-id="7b681-246">X.509 憑證的私密金鑰是由 OS 管理。</span><span class="sxs-lookup"><span data-stu-id="7b681-246">The X.509 certificate's private key is managed by the OS.</span></span> <span data-ttu-id="7b681-247">如需詳細資訊，請參閱 <<c0> [ 使用而不是用戶端祕密憑證進行驗證](/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret)。</span><span class="sxs-lookup"><span data-stu-id="7b681-247">For more information, see [Authenticate with a Certificate instead of a Client Secret](/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret).</span></span> <span data-ttu-id="7b681-248">使用`AddAzureKeyVault`多載，接受`X509Certificate2`(`_env`在下列範例中：</span><span class="sxs-lookup"><span data-stu-id="7b681-248">Use the `AddAzureKeyVault` overload that accepts an `X509Certificate2` (`_env` in the following example :</span></span>

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

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="7b681-249">將陣列繫結到類別</span><span class="sxs-lookup"><span data-stu-id="7b681-249">Bind an array to a class</span></span>

<span data-ttu-id="7b681-250">提供者可以讀入繫結到 POCO 陣列的陣列中的組態值。</span><span class="sxs-lookup"><span data-stu-id="7b681-250">The provider is capable of reading configuration values into an array for binding to a POCO array.</span></span>

<span data-ttu-id="7b681-251">從允許包含冒號的索引鍵的組態來源讀取時 (`:`) 的數值索引鍵的區段分隔符號用來區別構成陣列的索引鍵 (`:0:`， `:1:`，...</span><span class="sxs-lookup"><span data-stu-id="7b681-251">When reading from a configuration source that allows keys to contain colon (`:`) separators, a numeric key segment is used to distinguish the keys that make up an array (`:0:`, `:1:`, …</span></span> <span data-ttu-id="7b681-252">`:{n}:`)。</span><span class="sxs-lookup"><span data-stu-id="7b681-252">`:{n}:`).</span></span> <span data-ttu-id="7b681-253">如需詳細資訊，請參閱[組態：將陣列繫結至類別](xref:fundamentals/configuration/index#bind-an-array-to-a-class)。</span><span class="sxs-lookup"><span data-stu-id="7b681-253">For more information, see [Configuration: Bind an array to a class](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span></span>

<span data-ttu-id="7b681-254">Azure Key Vault 金鑰無法使用冒號做為分隔符號。</span><span class="sxs-lookup"><span data-stu-id="7b681-254">Azure Key Vault keys can't use a colon as a separator.</span></span> <span data-ttu-id="7b681-255">本主題中所述的方法會使用雙連字號 (`--`) 當作分隔符號 （區段） 的階層式值。</span><span class="sxs-lookup"><span data-stu-id="7b681-255">The approach described in this topic uses double dashes (`--`) as a separator for hierarchical values (sections).</span></span> <span data-ttu-id="7b681-256">陣列索引鍵時，會儲存在 Azure 金鑰保存庫上，使用雙連字號和數值的重要片段 (`--0--`， `--1--`，...</span><span class="sxs-lookup"><span data-stu-id="7b681-256">Array keys are stored in Azure Key Vault with double dashes and numeric key segments (`--0--`, `--1--`, …</span></span> <span data-ttu-id="7b681-257">`--{n}--`)。</span><span class="sxs-lookup"><span data-stu-id="7b681-257">`--{n}--`).</span></span>

<span data-ttu-id="7b681-258">檢查下列[Serilog](https://serilog.net/)記錄的 JSON 檔案所提供的提供者組態。</span><span class="sxs-lookup"><span data-stu-id="7b681-258">Examine the following [Serilog](https://serilog.net/) logging provider configuration provided by a JSON file.</span></span> <span data-ttu-id="7b681-259">有兩個物件中定義的常值`WriteTo`陣列，其中會反映兩個 Serilog*接收*，可描述記錄輸出的目的地：</span><span class="sxs-lookup"><span data-stu-id="7b681-259">There are two object literals defined in the `WriteTo` array that reflect two Serilog *sinks*, which describe destinations for logging output:</span></span>

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

<span data-ttu-id="7b681-260">在上述的 JSON 檔案中所示的組態會儲存在 Azure 金鑰保存庫使用雙破折號 (`--`) 標記法和數值的區段：</span><span class="sxs-lookup"><span data-stu-id="7b681-260">The configuration shown in the preceding JSON file is stored in Azure Key Vault using double dash (`--`) notation and numeric segments:</span></span>

| <span data-ttu-id="7b681-261">Key</span><span class="sxs-lookup"><span data-stu-id="7b681-261">Key</span></span> | <span data-ttu-id="7b681-262">值</span><span class="sxs-lookup"><span data-stu-id="7b681-262">Value</span></span> |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="reload-secrets"></a><span data-ttu-id="7b681-263">重新載入祕密</span><span class="sxs-lookup"><span data-stu-id="7b681-263">Reload secrets</span></span>

<span data-ttu-id="7b681-264">密碼會快取直到`IConfigurationRoot.Reload()`呼叫。</span><span class="sxs-lookup"><span data-stu-id="7b681-264">Secrets are cached until `IConfigurationRoot.Reload()` is called.</span></span> <span data-ttu-id="7b681-265">過期，停用，以及更新金鑰保存庫中的祕密未遵守應用程式，直到`Reload`執行。</span><span class="sxs-lookup"><span data-stu-id="7b681-265">Expired, disabled, and updated secrets in the key vault are not respected by the app until `Reload` is executed.</span></span>

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a><span data-ttu-id="7b681-266">已停用和到期的祕密</span><span class="sxs-lookup"><span data-stu-id="7b681-266">Disabled and expired secrets</span></span>

<span data-ttu-id="7b681-267">已停用及過期的密碼會擲回`KeyVaultClientException`。</span><span class="sxs-lookup"><span data-stu-id="7b681-267">Disabled and expired secrets throw a `KeyVaultClientException`.</span></span> <span data-ttu-id="7b681-268">若要防止您的應用程式擲回，取代您的應用程式或更新已停用/過期的祕密。</span><span class="sxs-lookup"><span data-stu-id="7b681-268">To prevent your app from throwing, replace your app or update the disabled/expired secret.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="7b681-269">疑難排解</span><span class="sxs-lookup"><span data-stu-id="7b681-269">Troubleshoot</span></span>

<span data-ttu-id="7b681-270">當應用程式無法載入組態使用的提供者時，錯誤訊息會寫入[ASP.NET Core 記錄基礎結構](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="7b681-270">When the app fails to load configuration using the provider, an error message is written to the [ASP.NET Core Logging infrastructure](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="7b681-271">在下列情況會造成無法載入組態：</span><span class="sxs-lookup"><span data-stu-id="7b681-271">The following conditions will prevent configuration from loading:</span></span>

* <span data-ttu-id="7b681-272">應用程式未正確設定 Azure Active Directory 中。</span><span class="sxs-lookup"><span data-stu-id="7b681-272">The app isn't configured correctly in Azure Active Directory.</span></span>
* <span data-ttu-id="7b681-273">金鑰保存庫不存在於 Azure 金鑰保存庫。</span><span class="sxs-lookup"><span data-stu-id="7b681-273">The key vault doesn't exist in Azure Key Vault.</span></span>
* <span data-ttu-id="7b681-274">應用程式未獲授權存取金鑰保存庫。</span><span class="sxs-lookup"><span data-stu-id="7b681-274">The app isn't authorized to access the key vault.</span></span>
* <span data-ttu-id="7b681-275">存取原則不包含`Get`和`List`權限。</span><span class="sxs-lookup"><span data-stu-id="7b681-275">The access policy doesn't include `Get` and `List` permissions.</span></span>
* <span data-ttu-id="7b681-276">在金鑰保存庫中，設定資料 （名稱 / 值組） 錯誤命名為，遺漏，停用，或已過期。</span><span class="sxs-lookup"><span data-stu-id="7b681-276">In the key vault, the configuration data (name-value pair) is incorrectly named, missing, disabled, or expired.</span></span>
* <span data-ttu-id="7b681-277">應用程式有錯誤的金鑰保存庫名稱 (`Vault`)，Azure AD 應用程式識別碼 (`ClientId`)，或 Azure AD 金鑰 (`ClientSecret`)。</span><span class="sxs-lookup"><span data-stu-id="7b681-277">The app has the wrong key vault name (`Vault`), Azure AD App Id (`ClientId`), or Azure AD Key (`ClientSecret`).</span></span>
* <span data-ttu-id="7b681-278">Azure AD 金鑰 (`ClientSecret`) 已過期。</span><span class="sxs-lookup"><span data-stu-id="7b681-278">The Azure AD Key (`ClientSecret`) is expired.</span></span>
* <span data-ttu-id="7b681-279">不正確的值，您想要載入的應用程式中設定金鑰 （名稱）。</span><span class="sxs-lookup"><span data-stu-id="7b681-279">The configuration key (name) is incorrect in the app for the value you're trying to load.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7b681-280">其他資源</span><span class="sxs-lookup"><span data-stu-id="7b681-280">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* [<span data-ttu-id="7b681-281">Microsoft Azure:金鑰保存庫</span><span class="sxs-lookup"><span data-stu-id="7b681-281">Microsoft Azure: Key Vault</span></span>](https://azure.microsoft.com/services/key-vault/)
* [<span data-ttu-id="7b681-282">Microsoft Azure:Key Vault 文件</span><span class="sxs-lookup"><span data-stu-id="7b681-282">Microsoft Azure: Key Vault Documentation</span></span>](/azure/key-vault/)
* [<span data-ttu-id="7b681-283">如何產生並傳輸受 HSM 保護金鑰的 Azure 金鑰保存庫</span><span class="sxs-lookup"><span data-stu-id="7b681-283">How to generate and transfer HSM-protected keys for Azure Key Vault</span></span>](/azure/key-vault/key-vault-hsm-protected-keys)
* [<span data-ttu-id="7b681-284">KeyVaultClient 類別</span><span class="sxs-lookup"><span data-stu-id="7b681-284">KeyVaultClient Class</span></span>](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
