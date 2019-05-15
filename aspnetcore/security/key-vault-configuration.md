---
title: ASP.NET Core 中的 azure Key Vault 組態提供者
author: guardrex
description: 了解如何使用 Azure 金鑰保存庫的組態提供者設定應用程式使用在執行階段載入的名稱 / 值組。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/13/2019
uid: security/key-vault-configuration
ms.openlocfilehash: 78c63cf135ca92f0b5f6c6828b2ae34a44a7b36c
ms.sourcegitcommit: 3ee6ee0051c3d2c8d47a58cb17eef1a84a4c46a0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/14/2019
ms.locfileid: "65621004"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a><span data-ttu-id="34e85-103">ASP.NET Core 中的 azure Key Vault 組態提供者</span><span class="sxs-lookup"><span data-stu-id="34e85-103">Azure Key Vault Configuration Provider in ASP.NET Core</span></span>

<span data-ttu-id="34e85-104">藉由[Luke Latham](https://github.com/guardrex)和[Andrew Stanton-nurse](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="34e85-104">By [Luke Latham](https://github.com/guardrex) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="34e85-105">本文件說明如何使用[Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/)組態提供者，從 Azure Key Vault 祕密載入應用程式組態值。</span><span class="sxs-lookup"><span data-stu-id="34e85-105">This document explains how to use the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) Configuration Provider to load app configuration values from Azure Key Vault secrets.</span></span> <span data-ttu-id="34e85-106">Azure Key Vault 是雲端式服務，可以協助使用者保護密碼編譯金鑰和應用程式和服務所使用的密碼。</span><span class="sxs-lookup"><span data-stu-id="34e85-106">Azure Key Vault is a cloud-based service that assists in safeguarding cryptographic keys and secrets used by apps and services.</span></span> <span data-ttu-id="34e85-107">ASP.NET Core 應用程式使用 Azure 金鑰保存庫的常見案例包括：</span><span class="sxs-lookup"><span data-stu-id="34e85-107">Common scenarios for using Azure Key Vault with ASP.NET Core apps include:</span></span>

* <span data-ttu-id="34e85-108">控制對敏感的組態資料的存取。</span><span class="sxs-lookup"><span data-stu-id="34e85-108">Controlling access to sensitive configuration data.</span></span>
* <span data-ttu-id="34e85-109">儲存組態資料時，符合 fips 需求 140-2 Level 2 驗證的硬體安全性模組 (HSM)。</span><span class="sxs-lookup"><span data-stu-id="34e85-109">Meeting the requirement for FIPS 140-2 Level 2 validated Hardware Security Modules (HSM's) when storing configuration data.</span></span>

<span data-ttu-id="34e85-110">此案例是適用於應用程式為目標的 ASP.NET Core 2.1 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="34e85-110">This scenario is available for apps that target ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="34e85-111">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="34e85-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="packages"></a><span data-ttu-id="34e85-112">封裝</span><span class="sxs-lookup"><span data-stu-id="34e85-112">Packages</span></span>

<span data-ttu-id="34e85-113">若要使用 Azure 金鑰保存庫的組態提供者，將新增的套件參考[Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/)封裝。</span><span class="sxs-lookup"><span data-stu-id="34e85-113">To use the Azure Key Vault Configuration Provider, add a package reference to the [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) package.</span></span>

<span data-ttu-id="34e85-114">採用[管理 Azure 資源的身分識別](/azure/active-directory/managed-identities-azure-resources/overview)案例中，新增的套件參考[Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/)封裝。</span><span class="sxs-lookup"><span data-stu-id="34e85-114">To adopt the [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview) scenario, add a package reference to the [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) package.</span></span>

> [!NOTE]
> <span data-ttu-id="34e85-115">在本文撰寫之際，最新穩定版本`Microsoft.Azure.Services.AppAuthentication`，版本`1.0.3`，可讓您[系統指派給受控身分識別](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-worka-namehow-does-it-worka)。</span><span class="sxs-lookup"><span data-stu-id="34e85-115">At the time of writing, the latest stable release of `Microsoft.Azure.Services.AppAuthentication`, version `1.0.3`, provides support for [system-assigned managed identities](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-worka-namehow-does-it-worka).</span></span> <span data-ttu-id="34e85-116">支援*指派使用者給受控身分識別*位於`1.2.0-preview2`封裝。</span><span class="sxs-lookup"><span data-stu-id="34e85-116">Support for *user-assigned managed identities* is available in the `1.2.0-preview2` package.</span></span> <span data-ttu-id="34e85-117">本主題示範如何使用系統管理的身分識別，並提供的範例應用程式使用的版本`1.0.3`的`Microsoft.Azure.Services.AppAuthentication`封裝。</span><span class="sxs-lookup"><span data-stu-id="34e85-117">This topic demonstrates the use of system-managed identities, and the provided sample app uses version `1.0.3` of the `Microsoft.Azure.Services.AppAuthentication` package.</span></span>

## <a name="sample-app"></a><span data-ttu-id="34e85-118">範例應用程式</span><span class="sxs-lookup"><span data-stu-id="34e85-118">Sample app</span></span>

<span data-ttu-id="34e85-119">範例應用程式執行所決定的兩種模式之一`#define`陳述式，在頂端*Program.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="34e85-119">The sample app runs in either of two modes determined by the `#define` statement at the top of the *Program.cs* file:</span></span>

* <span data-ttu-id="34e85-120">`Certificate` &ndash; 示範如何使用 Azure Key Vault 中儲存的存取祕密的 Azure 金鑰保存庫用戶端識別碼和 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="34e85-120">`Certificate` &ndash; Demonstrates the use of an Azure Key Vault Client ID and X.509 certificate to access secrets stored in Azure Key Vault.</span></span> <span data-ttu-id="34e85-121">從任何位置，部署至 Azure App Service 或任何能夠為 ASP.NET Core 應用程式的主機，可以執行此版本的範例。</span><span class="sxs-lookup"><span data-stu-id="34e85-121">This version of the sample can be run from any location, deployed to Azure App Service or any host capable of serving an ASP.NET Core app.</span></span>
* <span data-ttu-id="34e85-122">`Managed` &ndash; 示範如何使用[管理 Azure 資源的身分識別](/azure/active-directory/managed-identities-azure-resources/overview)驗證的應用程式至 Azure Key Vault 與 Azure AD 驗證不含認證儲存在應用程式的程式碼或組態。</span><span class="sxs-lookup"><span data-stu-id="34e85-122">`Managed` &ndash; Demonstrates how to use [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview) to authenticate the app to Azure Key Vault with Azure AD authentication without credentials stored in the app's code or configuration.</span></span> <span data-ttu-id="34e85-123">當使用受管理的身分識別進行驗證，不需要的 Azure AD 應用程式識別碼和密碼 （用戶端祕密）。</span><span class="sxs-lookup"><span data-stu-id="34e85-123">When using managed identities to authenticate, an Azure AD Application ID and Password (Client Secret) aren't required.</span></span> <span data-ttu-id="34e85-124">`Managed`範例版本必須部署至 Azure。</span><span class="sxs-lookup"><span data-stu-id="34e85-124">The `Managed` version of the sample must be deployed to Azure.</span></span> <span data-ttu-id="34e85-125">請依照下列中的指導方針[適用於 Azure 資源使用受控身分識別](#use-managed-identities-for-azure-resources)一節。</span><span class="sxs-lookup"><span data-stu-id="34e85-125">Follow the guidance in the [Use the Managed identities for Azure resources](#use-managed-identities-for-azure-resources) section.</span></span>

<span data-ttu-id="34e85-126">如需有關如何設定範例應用程式使用前置處理器指示詞 (`#define`)，請參閱<xref:index#preprocessor-directives-in-sample-code>。</span><span class="sxs-lookup"><span data-stu-id="34e85-126">For more information on how to configure a sample app using preprocessor directives (`#define`), see <xref:index#preprocessor-directives-in-sample-code>.</span></span>

## <a name="secret-storage-in-the-development-environment"></a><span data-ttu-id="34e85-127">在開發環境中的祕密儲存體</span><span class="sxs-lookup"><span data-stu-id="34e85-127">Secret storage in the Development environment</span></span>

<span data-ttu-id="34e85-128">設定在本機使用的祕密[Secret Manager 工具](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="34e85-128">Set secrets locally using the [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="34e85-129">當在開發環境中本機電腦上，執行範例應用程式時，會載入密碼從本機的祕密管理員存放區。</span><span class="sxs-lookup"><span data-stu-id="34e85-129">When the sample app runs on the local machine in the Development environment, secrets are loaded from the local Secret Manager store.</span></span>

<span data-ttu-id="34e85-130">Secret Manager 工具需要`<UserSecretsId>`應用程式的專案檔中的屬性。</span><span class="sxs-lookup"><span data-stu-id="34e85-130">The Secret Manager tool requires a `<UserSecretsId>` property in the app's project file.</span></span> <span data-ttu-id="34e85-131">設定屬性值 (`{GUID}`) 任何唯一的 guid:</span><span class="sxs-lookup"><span data-stu-id="34e85-131">Set the property value (`{GUID}`) to any unique GUID:</span></span>

```xml
<PropertyGroup>
  <UserSecretsId>{GUID}</UserSecretsId>
</PropertyGroup>
```

<span data-ttu-id="34e85-132">密碼會建立為名稱 / 值組。</span><span class="sxs-lookup"><span data-stu-id="34e85-132">Secrets are created as name-value pairs.</span></span> <span data-ttu-id="34e85-133">階層式的值 （組態區段） 會使用`:`（冒號） 為分隔符號[ASP.NET Core 組態](xref:fundamentals/configuration/index)索引鍵名稱。</span><span class="sxs-lookup"><span data-stu-id="34e85-133">Hierarchical values (configuration sections) use a `:` (colon) as a separator in [ASP.NET Core configuration](xref:fundamentals/configuration/index) key names.</span></span>

<span data-ttu-id="34e85-134">Secret Manager 可從命令殼層，開啟專案的內容根目錄，其中`{SECRET NAME}`名稱和`{SECRET VALUE}`的值：</span><span class="sxs-lookup"><span data-stu-id="34e85-134">The Secret Manager is used from a command shell opened to the project's content root, where `{SECRET NAME}` is the name and `{SECRET VALUE}` is the value:</span></span>

```console
dotnet user-secrets set "{SECRET NAME}" "{SECRET VALUE}"
```

<span data-ttu-id="34e85-135">從專案的內容根目錄設定範例應用程式祕密命令殼層中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="34e85-135">Execute the following commands in a command shell from the project's content root to set the secrets for the sample app:</span></span>

```console
dotnet user-secrets set "SecretName" "secret_value_1_dev"
dotnet user-secrets set "Section:SecretName" "secret_value_2_dev"
```

<span data-ttu-id="34e85-136">當這些祕密儲存在 Azure Key Vault[生產環境使用 Azure Key Vault 中密碼的儲存體](#secret-storage-in-the-production-environment-with-azure-key-vault)區段中，`_dev`後置詞變更為`_prod`。</span><span class="sxs-lookup"><span data-stu-id="34e85-136">When these secrets are stored in Azure Key Vault in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section, the `_dev` suffix is changed to `_prod`.</span></span> <span data-ttu-id="34e85-137">後置詞提供視覺提示，指出組態值的來源的應用程式的輸出中。</span><span class="sxs-lookup"><span data-stu-id="34e85-137">The suffix provides a visual cue in the app's output indicating the source of the configuration values.</span></span>

## <a name="secret-storage-in-the-production-environment-with-azure-key-vault"></a><span data-ttu-id="34e85-138">在生產環境使用 Azure Key Vault 的祕密儲存體</span><span class="sxs-lookup"><span data-stu-id="34e85-138">Secret storage in the Production environment with Azure Key Vault</span></span>

<span data-ttu-id="34e85-139">所提供的指示[快速入門：設定並從使用 Azure CLI 的 Azure 金鑰保存庫擷取祕密](/azure/key-vault/quick-create-cli)主題會彙總以供建立 Azure Key Vault 及儲存範例應用程式所使用的密碼。</span><span class="sxs-lookup"><span data-stu-id="34e85-139">The instructions provided by the [Quickstart: Set and retrieve a secret from Azure Key Vault using Azure CLI](/azure/key-vault/quick-create-cli) topic are summarized here for creating an Azure Key Vault and storing secrets used by the sample app.</span></span> <span data-ttu-id="34e85-140">請參閱本主題以取得詳細資料。</span><span class="sxs-lookup"><span data-stu-id="34e85-140">Refer to the topic for further details.</span></span>

1. <span data-ttu-id="34e85-141">開啟 Azure Cloud shell 中使用中的下列方法的任一[Azure 入口網站](https://portal.azure.com/):</span><span class="sxs-lookup"><span data-stu-id="34e85-141">Open Azure Cloud shell using any one of the following methods in the [Azure portal](https://portal.azure.com/):</span></span>

   * <span data-ttu-id="34e85-142">選取 **試試**的程式碼區塊右上角。</span><span class="sxs-lookup"><span data-stu-id="34e85-142">Select **Try It** in the upper-right corner of a code block.</span></span> <span data-ttu-id="34e85-143">使用搜尋字串"Azure CLI 」，在文字方塊中。</span><span class="sxs-lookup"><span data-stu-id="34e85-143">Use the search string "Azure CLI" in the text box.</span></span>
   * <span data-ttu-id="34e85-144">在您的瀏覽器中開啟 Cloud Shell**啟動 Cloud Shell**  按鈕。</span><span class="sxs-lookup"><span data-stu-id="34e85-144">Open Cloud Shell in your browser with the **Launch Cloud Shell** button.</span></span>
   * <span data-ttu-id="34e85-145">選取  **Cloud Shell**在 Azure 入口網站右上角的功能表上的按鈕。</span><span class="sxs-lookup"><span data-stu-id="34e85-145">Select the **Cloud Shell** button on the menu in the upper-right corner of the Azure portal.</span></span>

   <span data-ttu-id="34e85-146">如需詳細資訊，請參閱 < [Azure 命令列介面 (CLI)](/cli/azure/)並[Azure Cloud Shell 概觀](/azure/cloud-shell/overview)。</span><span class="sxs-lookup"><span data-stu-id="34e85-146">For more information, see [Azure Command-Line Interface (CLI)](/cli/azure/) and [Overview of Azure Cloud Shell](/azure/cloud-shell/overview).</span></span>

1. <span data-ttu-id="34e85-147">如果您不已通過驗證，登入的`az login`命令。</span><span class="sxs-lookup"><span data-stu-id="34e85-147">If you aren't already authenticated, sign in with the `az login` command.</span></span>

1. <span data-ttu-id="34e85-148">建立資源群組，使用下列命令，其中`{RESOURCE GROUP NAME}`是新的資源群組的資源群組名稱和`{LOCATION}`是 Azure 區域 （資料中心）：</span><span class="sxs-lookup"><span data-stu-id="34e85-148">Create a resource group with the following command, where `{RESOURCE GROUP NAME}` is the resource group name for the new resource group and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```console
   az group create --name "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="34e85-149">在資源群組中，使用下列命令，建立金鑰保存庫所在`{KEY VAULT NAME}`是新的金鑰保存庫的名稱和`{LOCATION}`是 Azure 區域 （資料中心）：</span><span class="sxs-lookup"><span data-stu-id="34e85-149">Create a key vault in the resource group with the following command, where `{KEY VAULT NAME}` is the name for the new key vault and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```console
   az keyvault create --name "{KEY VAULT NAME}" --resource-group "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="34e85-150">建立金鑰保存庫中的密碼，做為名稱 / 值組。</span><span class="sxs-lookup"><span data-stu-id="34e85-150">Create secrets in the key vault as name-value pairs.</span></span>

   <span data-ttu-id="34e85-151">Azure Key Vault 祕密名稱必須是限制為英數字元及虛線。</span><span class="sxs-lookup"><span data-stu-id="34e85-151">Azure Key Vault secret names are limited to alphanumeric characters and dashes.</span></span> <span data-ttu-id="34e85-152">階層式的值 （組態區段） 使用`--`（兩個連字號） 做為分隔符號。</span><span class="sxs-lookup"><span data-stu-id="34e85-152">Hierarchical values (configuration sections) use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="34e85-153">通常用來分隔的區段中的子機碼的冒號[ASP.NET Core 組態](xref:fundamentals/configuration/index)，不允許在金鑰保存庫祕密名稱。</span><span class="sxs-lookup"><span data-stu-id="34e85-153">Colons, which are normally used to delimit a section from a subkey in [ASP.NET Core configuration](xref:fundamentals/configuration/index), aren't allowed in key vault secret names.</span></span> <span data-ttu-id="34e85-154">因此，兩個連字號是用，而且密碼會載入應用程式的設定時，已還原為冒號。</span><span class="sxs-lookup"><span data-stu-id="34e85-154">Therefore, two dashes are used and swapped for a colon when the secrets are loaded into the app's configuration.</span></span>

   <span data-ttu-id="34e85-155">下列的機密資料是範例應用程式搭配使用。</span><span class="sxs-lookup"><span data-stu-id="34e85-155">The following secrets are for use with the sample app.</span></span> <span data-ttu-id="34e85-156">值包括`_prod`後置詞來區別從`_dev`後置詞從使用者的機密資訊的開發環境中載入的值。</span><span class="sxs-lookup"><span data-stu-id="34e85-156">The values include a `_prod` suffix to distinguish them from the `_dev` suffix values loaded in the Development environment from User Secrets.</span></span> <span data-ttu-id="34e85-157">取代`{KEY VAULT NAME}`與您在先前步驟中建立的金鑰保存庫名稱：</span><span class="sxs-lookup"><span data-stu-id="34e85-157">Replace `{KEY VAULT NAME}` with the name of the key vault that you created in the prior step:</span></span>

   ```console
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "SecretName" --value "secret_value_1_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "Section--SecretName" --value "secret_value_2_prod"
   ```

## <a name="use-application-id-and-x509-certificate-for-non-azure-hosted-apps"></a><span data-ttu-id="34e85-158">使用非 Azure 代管應用程式的應用程式識別碼和 X.509 憑證</span><span class="sxs-lookup"><span data-stu-id="34e85-158">Use Application ID and X.509 certificate for non-Azure-hosted apps</span></span>

<span data-ttu-id="34e85-159">設定 Azure AD、 Azure 金鑰保存庫和應用程式使用 Azure Active Directory 應用程式識別碼和 X.509 憑證來向 key vault**當應用程式裝載在 Azure 之外**。</span><span class="sxs-lookup"><span data-stu-id="34e85-159">Configure Azure AD, Azure Key Vault, and the app to use an Azure Active Directory Application ID and X.509 certificate to authenticate to a key vault **when the app is hosted outside of Azure**.</span></span> <span data-ttu-id="34e85-160">如需詳細資訊，請參閱 <<c0> [ 關於金鑰、 祕密和憑證](/azure/key-vault/about-keys-secrets-and-certificates)。</span><span class="sxs-lookup"><span data-stu-id="34e85-160">For more information, see [About keys, secrets, and certificates](/azure/key-vault/about-keys-secrets-and-certificates).</span></span>

> [!NOTE]
> <span data-ttu-id="34e85-161">雖然在 Azure 中託管的應用程式支援使用應用程式識別碼和 X.509 憑證，但我們建議您使用[管理 Azure 資源的身分識別](#use-managed-identities-for-azure-resources)裝載在 Azure 中的應用程式時。</span><span class="sxs-lookup"><span data-stu-id="34e85-161">Although using an Application ID and X.509 certificate is supported for apps hosted in Azure, we recommend using [Managed identities for Azure resources](#use-managed-identities-for-azure-resources) when hosting an app in Azure.</span></span> <span data-ttu-id="34e85-162">受管理的身分識別不需要將憑證儲存在應用程式或開發環境。</span><span class="sxs-lookup"><span data-stu-id="34e85-162">Managed identities don't require storing a certificate in the app or in the development environment.</span></span>

<span data-ttu-id="34e85-163">範例應用程式會使用的應用程式識別碼和 X.509 憑證的時機`#define`陳述式，在頂端*Program.cs*檔案設定為`Certificate`。</span><span class="sxs-lookup"><span data-stu-id="34e85-163">The sample app uses an Application ID and X.509 certificate when the `#define` statement at the top of the *Program.cs* file is set to `Certificate`.</span></span>

1. <span data-ttu-id="34e85-164">建立 PKCS #12 封存 (*.pfx*) 憑證。</span><span class="sxs-lookup"><span data-stu-id="34e85-164">Create a PKCS#12 archive (*.pfx*) certificate.</span></span> <span data-ttu-id="34e85-165">建立憑證的選項包括[在 Windows 上的 MakeCert](/windows/desktop/seccrypto/makecert)並[OpenSSL](https://www.openssl.org/)。</span><span class="sxs-lookup"><span data-stu-id="34e85-165">Options for creating certificates include [MakeCert on Windows](/windows/desktop/seccrypto/makecert) and [OpenSSL](https://www.openssl.org/).</span></span>
1. <span data-ttu-id="34e85-166">將憑證安裝到目前使用者的個人憑證存放區。</span><span class="sxs-lookup"><span data-stu-id="34e85-166">Install the certificate into the current user's personal certificate store.</span></span> <span data-ttu-id="34e85-167">將金鑰標示為可匯出是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="34e85-167">Marking the key as exportable is optional.</span></span> <span data-ttu-id="34e85-168">請注意憑證的指紋，這將會稍後在此程序。</span><span class="sxs-lookup"><span data-stu-id="34e85-168">Note the certificate's thumbprint, which is used later in this process.</span></span>
1. <span data-ttu-id="34e85-169">匯出 PKCS #12 封存 (*.pfx*) 做為 DER 編碼的憑證的憑證 (*.cer*)。</span><span class="sxs-lookup"><span data-stu-id="34e85-169">Export the PKCS#12 archive (*.pfx*) certificate as a DER-encoded certificate (*.cer*).</span></span>
1. <span data-ttu-id="34e85-170">使用 Azure AD 註冊應用程式 (**應用程式註冊**)。</span><span class="sxs-lookup"><span data-stu-id="34e85-170">Register the app with Azure AD (**App registrations**).</span></span>
1. <span data-ttu-id="34e85-171">上傳 DER 編碼的憑證 (*.cer*) 至 Azure AD:</span><span class="sxs-lookup"><span data-stu-id="34e85-171">Upload the DER-encoded certificate (*.cer*) to Azure AD:</span></span>
   1. <span data-ttu-id="34e85-172">在 Azure AD 中選取的應用程式。</span><span class="sxs-lookup"><span data-stu-id="34e85-172">Select the app in Azure AD.</span></span>
   1. <span data-ttu-id="34e85-173">瀏覽至**憑證與祕密**。</span><span class="sxs-lookup"><span data-stu-id="34e85-173">Navigate to **Certificates & secrets**.</span></span>
   1. <span data-ttu-id="34e85-174">選取 **上傳憑證**來上傳包含公開金鑰的憑證。</span><span class="sxs-lookup"><span data-stu-id="34e85-174">Select **Upload certificate** to upload the certificate, which contains the public key.</span></span> <span data-ttu-id="34e85-175">A *.cer*， *.pem*，或 *.crt*是可接受的憑證。</span><span class="sxs-lookup"><span data-stu-id="34e85-175">A *.cer*, *.pem*, or *.crt* certificate is acceptable.</span></span>
1. <span data-ttu-id="34e85-176">在應用程式中儲存的金鑰保存庫名稱、 應用程式識別碼和憑證指紋*appsettings.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="34e85-176">Store the key vault name, Application ID, and certificate thumbprint in the app's *appsettings.json* file.</span></span>
1. <span data-ttu-id="34e85-177">瀏覽至**金鑰保存庫**在 Azure 入口網站中。</span><span class="sxs-lookup"><span data-stu-id="34e85-177">Navigate to **Key vaults** in the Azure portal.</span></span>
1. <span data-ttu-id="34e85-178">選取您在建立金鑰保存庫[生產環境使用 Azure Key Vault 中密碼的儲存體](#secret-storage-in-the-production-environment-with-azure-key-vault)一節。</span><span class="sxs-lookup"><span data-stu-id="34e85-178">Select the key vault that you created in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section.</span></span>
1. <span data-ttu-id="34e85-179">選取 **存取原則**。</span><span class="sxs-lookup"><span data-stu-id="34e85-179">Select **Access policies**.</span></span>
1. <span data-ttu-id="34e85-180">選取 **新增**。</span><span class="sxs-lookup"><span data-stu-id="34e85-180">Select **Add new**.</span></span>
1. <span data-ttu-id="34e85-181">選取 **選取主體**依名稱選取的已註冊的應用程式。</span><span class="sxs-lookup"><span data-stu-id="34e85-181">Select **Select principal** and select the registered app by name.</span></span> <span data-ttu-id="34e85-182">選取 [**選取**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="34e85-182">Select the **Select** button.</span></span>
1. <span data-ttu-id="34e85-183">開啟**祕密權限**，並提供應用程式與**取得**並**清單**權限。</span><span class="sxs-lookup"><span data-stu-id="34e85-183">Open **Secret permissions** and provide the app with **Get** and **List** permissions.</span></span>
1. <span data-ttu-id="34e85-184">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="34e85-184">Select **OK**.</span></span>
1. <span data-ttu-id="34e85-185">選取 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="34e85-185">Select **Save**.</span></span>
1. <span data-ttu-id="34e85-186">部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="34e85-186">Deploy the app.</span></span>

<span data-ttu-id="34e85-187">`Certificate`範例應用程式取得其組態值從`IConfigurationRoot`具有相同名稱與祕密名稱：</span><span class="sxs-lookup"><span data-stu-id="34e85-187">The `Certificate` sample app obtains its configuration values from `IConfigurationRoot` with the same name as the secret name:</span></span>

* <span data-ttu-id="34e85-188">非階層式的值：值`SecretName`取得`config["SecretName"]`。</span><span class="sxs-lookup"><span data-stu-id="34e85-188">Non-hierarchical values: The value for `SecretName` is obtained with `config["SecretName"]`.</span></span>
* <span data-ttu-id="34e85-189">（區段） 的階層式值：使用`:`（冒號） 標記法或`GetSection`擴充方法。</span><span class="sxs-lookup"><span data-stu-id="34e85-189">Hierarchical values (sections): Use `:` (colon) notation or the `GetSection` extension method.</span></span> <span data-ttu-id="34e85-190">您可以使用任一種方法來取得組態值：</span><span class="sxs-lookup"><span data-stu-id="34e85-190">Use either of these approaches to obtain the configuration value:</span></span>
  * `config["Section:SecretName"]`
  * `config.GetSection("Section")["SecretName"]`

<span data-ttu-id="34e85-191">X.509 憑證是由 OS 管理。</span><span class="sxs-lookup"><span data-stu-id="34e85-191">The X.509 certificate is managed by the OS.</span></span> <span data-ttu-id="34e85-192">應用程式呼叫`AddAzureKeyVault`所提供的值*appsettings.json*檔案：</span><span class="sxs-lookup"><span data-stu-id="34e85-192">The app calls `AddAzureKeyVault` with values supplied by the *appsettings.json* file:</span></span>

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet1&highlight=20-23)]

<span data-ttu-id="34e85-193">範例值：</span><span class="sxs-lookup"><span data-stu-id="34e85-193">Example values:</span></span>

* <span data-ttu-id="34e85-194">金鑰保存庫名稱： `contosovault`</span><span class="sxs-lookup"><span data-stu-id="34e85-194">Key vault name: `contosovault`</span></span>
* <span data-ttu-id="34e85-195">應用程式識別碼： `627e911e-43cc-61d4-992e-12db9c81b413`</span><span class="sxs-lookup"><span data-stu-id="34e85-195">Application ID: `627e911e-43cc-61d4-992e-12db9c81b413`</span></span>
* <span data-ttu-id="34e85-196">憑證指紋： `fe14593dd66b2406c5269d742d04b6e1ab03adb1`</span><span class="sxs-lookup"><span data-stu-id="34e85-196">Certificate thumbprint: `fe14593dd66b2406c5269d742d04b6e1ab03adb1`</span></span>

<span data-ttu-id="34e85-197">*appsettings.json*：</span><span class="sxs-lookup"><span data-stu-id="34e85-197">*appsettings.json*:</span></span>

[!code-json[](key-vault-configuration/sample/appsettings.json)]

<span data-ttu-id="34e85-198">當您執行應用程式時，網頁就會顯示載入的祕密值。</span><span class="sxs-lookup"><span data-stu-id="34e85-198">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="34e85-199">使用，在開發環境中，載入祕密值`_dev`後置詞。</span><span class="sxs-lookup"><span data-stu-id="34e85-199">In the Development environment, secret values load with the `_dev` suffix.</span></span> <span data-ttu-id="34e85-200">在生產環境中，值則是使用載入`_prod`後置詞。</span><span class="sxs-lookup"><span data-stu-id="34e85-200">In the Production environment, the values load with the `_prod` suffix.</span></span>

## <a name="use-managed-identities-for-azure-resources"></a><span data-ttu-id="34e85-201">適用於 Azure 資源的使用受管理身分識別</span><span class="sxs-lookup"><span data-stu-id="34e85-201">Use Managed identities for Azure resources</span></span>

<span data-ttu-id="34e85-202">**應用程式部署至 Azure**可以充分善用[管理適用於 Azure 資源的身分識別](/azure/active-directory/managed-identities-azure-resources/overview)，可讓應用程式，以向 Azure Key Vault 使用不含認證的 Azure AD 驗證 (應用程式識別碼和Password/Client 密碼) 儲存在應用程式。</span><span class="sxs-lookup"><span data-stu-id="34e85-202">**An app deployed to Azure** can take advantage of [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview), which allows the app to authenticate with Azure Key Vault using Azure AD authentication without credentials (Application ID and Password/Client Secret) stored in the app.</span></span>

<span data-ttu-id="34e85-203">範例應用程式會使用適用於 Azure 資源的受管理身分識別時`#define`陳述式，在頂端*Program.cs*檔案設定為`Managed`。</span><span class="sxs-lookup"><span data-stu-id="34e85-203">The sample app uses Managed identities for Azure resources when the `#define` statement at the top of the *Program.cs* file is set to `Managed`.</span></span>

<span data-ttu-id="34e85-204">輸入應用程式的保存庫名稱*appsettings.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="34e85-204">Enter the vault name into the app's *appsettings.json* file.</span></span> <span data-ttu-id="34e85-205">範例應用程式不需要應用程式識別碼和密碼 （用戶端秘密），當設定為`Managed`版本，因此您可以忽略這些組態項目。</span><span class="sxs-lookup"><span data-stu-id="34e85-205">The sample app doesn't require an Application ID and Password (Client Secret) when set to the `Managed` version, so you can ignore those configuration entries.</span></span> <span data-ttu-id="34e85-206">應用程式部署至 Azure，以及 Azure 驗證應用程式存取只使用 保存庫名稱儲存在 Azure Key Vault *appsettings.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="34e85-206">The app is deployed to Azure, and Azure authenticates the app to access Azure Key Vault only using the vault name stored in the *appsettings.json* file.</span></span>

<span data-ttu-id="34e85-207">將範例應用程式部署至 Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="34e85-207">Deploy the sample app to Azure App Service.</span></span>

<span data-ttu-id="34e85-208">應用程式部署至 Azure App Service 與 Azure AD 建立服務時，會自動註冊。</span><span class="sxs-lookup"><span data-stu-id="34e85-208">An app deployed to Azure App Service is automatically registered with Azure AD when the service is created.</span></span> <span data-ttu-id="34e85-209">從下列命令中使用的部署中取得的物件識別碼。</span><span class="sxs-lookup"><span data-stu-id="34e85-209">Obtain the Object ID from the deployment for use in the following command.</span></span> <span data-ttu-id="34e85-210">物件識別碼時，會顯示在 Azure 入口網站上，在**識別**App Service 的面板。</span><span class="sxs-lookup"><span data-stu-id="34e85-210">The Object ID is shown in the Azure portal on the **Identity** panel of the App Service.</span></span>

<span data-ttu-id="34e85-211">使用 Azure CLI 和應用程式的物件識別碼，提供應用程式與`list`和`get`存取金鑰保存庫的權限：</span><span class="sxs-lookup"><span data-stu-id="34e85-211">Using Azure CLI and the app's Object ID, provide the app with `list` and `get` permissions to access the key vault:</span></span>

```console
az keyvault set-policy --name '{KEY VAULT NAME}' --object-id {OBJECT ID} --secret-permissions get list
```

<span data-ttu-id="34e85-212">**重新啟動應用程式**使用 Azure CLI、 PowerShell 或 Azure 入口網站。</span><span class="sxs-lookup"><span data-stu-id="34e85-212">**Restart the app** using Azure CLI, PowerShell, or the Azure portal.</span></span>

<span data-ttu-id="34e85-213">範例應用程式：</span><span class="sxs-lookup"><span data-stu-id="34e85-213">The sample app:</span></span>

* <span data-ttu-id="34e85-214">建立的執行個體`AzureServiceTokenProvider`類別，而不需要的連接字串。</span><span class="sxs-lookup"><span data-stu-id="34e85-214">Creates an instance of the `AzureServiceTokenProvider` class without a connection string.</span></span> <span data-ttu-id="34e85-215">當未提供的連接字串時，提供者會嘗試從適用於 Azure 資源的受管理身分識別取得存取權杖。</span><span class="sxs-lookup"><span data-stu-id="34e85-215">When a connection string isn't provided, the provider attempts to obtain an access token from Managed identities for Azure resources.</span></span>
* <span data-ttu-id="34e85-216">新`KeyVaultClient`會透過`AzureServiceTokenProvider`執行個體權杖回呼。</span><span class="sxs-lookup"><span data-stu-id="34e85-216">A new `KeyVaultClient` is created with the `AzureServiceTokenProvider` instance token callback.</span></span>
* <span data-ttu-id="34e85-217">`KeyVaultClient`的預設實作會使用執行個體`IKeyVaultSecretManager`載入所有祕密的值，會取代雙連字號 (`--`) 加上冒號 (`:`) 中索引鍵名稱。</span><span class="sxs-lookup"><span data-stu-id="34e85-217">The `KeyVaultClient` instance is used with a default implementation of `IKeyVaultSecretManager` that loads all secret values and replaces double-dashes (`--`) with colons (`:`) in key names.</span></span>

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet2&highlight=13-21)]

<span data-ttu-id="34e85-218">當您執行應用程式時，網頁就會顯示載入的祕密值。</span><span class="sxs-lookup"><span data-stu-id="34e85-218">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="34e85-219">在開發環境中，有祕密值`_dev`後置詞，因為它們由使用者祕密。</span><span class="sxs-lookup"><span data-stu-id="34e85-219">In the Development environment, secret values have the `_dev` suffix because they're provided by User Secrets.</span></span> <span data-ttu-id="34e85-220">在生產環境中，值則是使用載入`_prod`後置詞，因為它們由 Azure 金鑰保存庫。</span><span class="sxs-lookup"><span data-stu-id="34e85-220">In the Production environment, the values load with the `_prod` suffix because they're provided by Azure Key Vault.</span></span>

<span data-ttu-id="34e85-221">如果您收到`Access denied`錯誤，確認已向 Azure AD 註冊應用程式，且提供存取金鑰保存庫。</span><span class="sxs-lookup"><span data-stu-id="34e85-221">If you receive an `Access denied` error, confirm that the app is registered with Azure AD and provided access to the key vault.</span></span> <span data-ttu-id="34e85-222">請確認您已重新開啟的服務在 Azure 中。</span><span class="sxs-lookup"><span data-stu-id="34e85-222">Confirm that you've restarted the service in Azure.</span></span>

## <a name="use-a-key-name-prefix"></a><span data-ttu-id="34e85-223">使用索引鍵名稱前置詞</span><span class="sxs-lookup"><span data-stu-id="34e85-223">Use a key name prefix</span></span>

<span data-ttu-id="34e85-224">`AddAzureKeyVault` 提供的多載，接受的實作`IKeyVaultSecretManager`，這可讓您控制如何金鑰保存庫祕密會轉換成設定的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="34e85-224">`AddAzureKeyVault` provides an overload that accepts an implementation of `IKeyVaultSecretManager`, which allows you to control how key vault secrets are converted into configuration keys.</span></span> <span data-ttu-id="34e85-225">比方說，您可以實作的介面，將根據您在應用程式啟動時所提供的前置詞值的祕密值。</span><span class="sxs-lookup"><span data-stu-id="34e85-225">For example, you can implement the interface to load secret values based on a prefix value you provide at app startup.</span></span> <span data-ttu-id="34e85-226">這可讓您，比方說，若要載入的應用程式的版本為基礎的祕密。</span><span class="sxs-lookup"><span data-stu-id="34e85-226">This allows you, for example, to load secrets based on the version of the app.</span></span>

> [!WARNING]
> <span data-ttu-id="34e85-227">若要將多個應用程式的祕密放入相同的金鑰保存庫，或將環境的密碼，請勿使用前置詞上的金鑰保存庫祕密 (例如*開發*與*生產*祕密) 置於同一個保存庫。</span><span class="sxs-lookup"><span data-stu-id="34e85-227">Don't use prefixes on key vault secrets to place secrets for multiple apps into the same key vault or to place environmental secrets (for example, *development* versus *production* secrets) into the same vault.</span></span> <span data-ttu-id="34e85-228">我們建議，不同的應用程式和開發/生產環境使用不同的金鑰保存庫來隔離應用程式環境，以最高層級的安全性。</span><span class="sxs-lookup"><span data-stu-id="34e85-228">We recommend that different apps and development/production environments use separate key vaults to isolate app environments for the highest level of security.</span></span>

<span data-ttu-id="34e85-229">在下列範例中，祕密建立金鑰保存庫 」 （並使用 Secret Manager 工具開發環境） 的`5000-AppSecret`（期間不允許在金鑰保存庫祕密名稱）。</span><span class="sxs-lookup"><span data-stu-id="34e85-229">In the following example, a secret is established in the key vault (and using the Secret Manager tool for the Development environment) for `5000-AppSecret` (periods aren't allowed in key vault secret names).</span></span> <span data-ttu-id="34e85-230">此祕密表示 version=5.0.0.0 新版應用程式的應用程式祕密。</span><span class="sxs-lookup"><span data-stu-id="34e85-230">This secret represents an app secret for version 5.0.0.0 of the app.</span></span> <span data-ttu-id="34e85-231">應用程式，5.1.0.0，另一個版本的祕密加入至金鑰保存庫 （並使用 Secret Manager 工具） 的`5100-AppSecret`。</span><span class="sxs-lookup"><span data-stu-id="34e85-231">For another version of the app, 5.1.0.0, a secret is added to the key vault (and using the Secret Manager tool) for `5100-AppSecret`.</span></span> <span data-ttu-id="34e85-232">每個應用程式版本載入其組態設為其建立版本的祕密值`AppSecret`、 清除關閉版本載入祕密。</span><span class="sxs-lookup"><span data-stu-id="34e85-232">Each app version loads its versioned secret value into its configuration as `AppSecret`, stripping off the version as it loads the secret.</span></span>

<span data-ttu-id="34e85-233">`AddAzureKeyVault` 自訂呼叫`IKeyVaultSecretManager`:</span><span class="sxs-lookup"><span data-stu-id="34e85-233">`AddAzureKeyVault` is called with a custom `IKeyVaultSecretManager`:</span></span>

[!code-csharp[](key-vault-configuration/sample_snapshot/Program.cs?highlight=30-34)]

<span data-ttu-id="34e85-234">`IKeyVaultSecretManager`實作回應組態中載入適當的祕密的祕密版本前置詞：</span><span class="sxs-lookup"><span data-stu-id="34e85-234">The `IKeyVaultSecretManager` implementation reacts to the version prefixes of secrets to load the proper secret into configuration:</span></span>

[!code-csharp[](key-vault-configuration/sample_snapshot/Startup.cs?name=snippet1)]

<span data-ttu-id="34e85-235">`Load`逐一查看，找出有版本前置詞的保存庫祕密提供者演算法所呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="34e85-235">The `Load` method is called by a provider algorithm that iterates through the vault secrets to find the ones that have the version prefix.</span></span> <span data-ttu-id="34e85-236">當版本的前置詞上有`Load`，此演算法會使用`GetKey`方法傳回的密碼名稱的組態名稱。</span><span class="sxs-lookup"><span data-stu-id="34e85-236">When a version prefix is found with `Load`, the algorithm uses the `GetKey` method to return the configuration name of the secret name.</span></span> <span data-ttu-id="34e85-237">它會除去版本前置詞，從 祕密的名稱，並傳回載入的祕密名稱的其餘部分，將應用程式設定成名稱 / 值組。</span><span class="sxs-lookup"><span data-stu-id="34e85-237">It strips off the version prefix from the secret's name and returns the rest of the secret name for loading into the app's configuration name-value pairs.</span></span>

<span data-ttu-id="34e85-238">當這個方法的實作方式：</span><span class="sxs-lookup"><span data-stu-id="34e85-238">When this approach is implemented:</span></span>

1. <span data-ttu-id="34e85-239">應用程式的專案檔中指定的應用程式的版本。</span><span class="sxs-lookup"><span data-stu-id="34e85-239">The app's version specified in the app's project file.</span></span> <span data-ttu-id="34e85-240">在下列範例中，應用程式的版本設為`5.0.0.0`:</span><span class="sxs-lookup"><span data-stu-id="34e85-240">In the following example, the app's version is set to `5.0.0.0`:</span></span>

   ```xml
   <PropertyGroup>
     <Version>5.0.0.0</Version>
   </PropertyGroup>
   ```

1. <span data-ttu-id="34e85-241">確認`<UserSecretsId>`屬性會出現在應用程式的專案檔案，其中`{GUID}`是使用者所提供的 GUID:</span><span class="sxs-lookup"><span data-stu-id="34e85-241">Confirm that a `<UserSecretsId>` property is present in the app's project file, where `{GUID}` is a user-supplied GUID:</span></span>

   ```xml
   <PropertyGroup>
     <UserSecretsId>{GUID}</UserSecretsId>
   </PropertyGroup>
   ```

   <span data-ttu-id="34e85-242">儲存在本機使用下列的祕密[Secret Manager 工具](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="34e85-242">Save the following secrets locally with the [Secret Manager tool](xref:security/app-secrets):</span></span>

   ```console
   dotnet user-secrets set "5000-AppSecret" "5.0.0.0_secret_value_dev"
   dotnet user-secrets set "5100-AppSecret" "5.1.0.0_secret_value_dev"
   ```

1. <span data-ttu-id="34e85-243">密碼會儲存在 Azure Key Vault 中使用下列 Azure CLI 命令：</span><span class="sxs-lookup"><span data-stu-id="34e85-243">Secrets are saved in Azure Key Vault using the following Azure CLI commands:</span></span>

   ```console
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5000-AppSecret" --value "5.0.0.0_secret_value_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5100-AppSecret" --value "5.1.0.0_secret_value_prod"
   ```

1. <span data-ttu-id="34e85-244">執行應用程式時，會載入的金鑰保存庫祕密。</span><span class="sxs-lookup"><span data-stu-id="34e85-244">When the app is run, the key vault secrets are loaded.</span></span> <span data-ttu-id="34e85-245">字串密碼`5000-AppSecret`會對應到應用程式的專案檔中指定的應用程式的版本 (`5.0.0.0`)。</span><span class="sxs-lookup"><span data-stu-id="34e85-245">The string secret for `5000-AppSecret` is matched to the app's version specified in the app's project file (`5.0.0.0`).</span></span>

1. <span data-ttu-id="34e85-246">版本， `5000` （與 dash)，移除的索引鍵名稱。</span><span class="sxs-lookup"><span data-stu-id="34e85-246">The version, `5000` (with the dash), is stripped from the key name.</span></span> <span data-ttu-id="34e85-247">在整個應用程式，讀取具有索引鍵的組態`AppSecret`載入祕密的值。</span><span class="sxs-lookup"><span data-stu-id="34e85-247">Throughout the app, reading configuration with the key `AppSecret` loads the secret value.</span></span>

1. <span data-ttu-id="34e85-248">如果應用程式的版本會變更專案檔案中`5.1.0.0`再次執行應用程式，則傳回的祕密值是`5.1.0.0_secret_value_dev`在開發環境和`5.1.0.0_secret_value_prod`在生產環境中。</span><span class="sxs-lookup"><span data-stu-id="34e85-248">If the app's version is changed in the project file to `5.1.0.0` and the app is run again, the secret value returned is `5.1.0.0_secret_value_dev` in the Development environment and `5.1.0.0_secret_value_prod` in Production.</span></span>

> [!NOTE]
> <span data-ttu-id="34e85-249">您也可以提供您自己`KeyVaultClient`實作`AddAzureKeyVault`。</span><span class="sxs-lookup"><span data-stu-id="34e85-249">You can also provide your own `KeyVaultClient` implementation to `AddAzureKeyVault`.</span></span> <span data-ttu-id="34e85-250">自訂用戶端會允許跨應用程式共用用戶端的單一執行個體。</span><span class="sxs-lookup"><span data-stu-id="34e85-250">A custom client permits sharing a single instance of the client across the app.</span></span>

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="34e85-251">將陣列繫結到類別</span><span class="sxs-lookup"><span data-stu-id="34e85-251">Bind an array to a class</span></span>

<span data-ttu-id="34e85-252">提供者可以讀入繫結到 POCO 陣列的陣列中的組態值。</span><span class="sxs-lookup"><span data-stu-id="34e85-252">The provider is capable of reading configuration values into an array for binding to a POCO array.</span></span>

<span data-ttu-id="34e85-253">從允許包含冒號的索引鍵的組態來源讀取時 (`:`) 的數值索引鍵的區段分隔符號用來區別構成陣列的索引鍵 (`:0:`， `:1:`，...</span><span class="sxs-lookup"><span data-stu-id="34e85-253">When reading from a configuration source that allows keys to contain colon (`:`) separators, a numeric key segment is used to distinguish the keys that make up an array (`:0:`, `:1:`, …</span></span> <span data-ttu-id="34e85-254">`:{n}:`)。</span><span class="sxs-lookup"><span data-stu-id="34e85-254">`:{n}:`).</span></span> <span data-ttu-id="34e85-255">如需詳細資訊，請參閱[組態：將陣列繫結至類別](xref:fundamentals/configuration/index#bind-an-array-to-a-class)。</span><span class="sxs-lookup"><span data-stu-id="34e85-255">For more information, see [Configuration: Bind an array to a class](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span></span>

<span data-ttu-id="34e85-256">Azure Key Vault 金鑰無法使用冒號做為分隔符號。</span><span class="sxs-lookup"><span data-stu-id="34e85-256">Azure Key Vault keys can't use a colon as a separator.</span></span> <span data-ttu-id="34e85-257">本主題中所述的方法會使用雙連字號 (`--`) 當作分隔符號 （區段） 的階層式值。</span><span class="sxs-lookup"><span data-stu-id="34e85-257">The approach described in this topic uses double dashes (`--`) as a separator for hierarchical values (sections).</span></span> <span data-ttu-id="34e85-258">陣列索引鍵時，會儲存在 Azure 金鑰保存庫上，使用雙連字號和數值的重要片段 (`--0--`， `--1--`， &hellip; `--{n}--`)。</span><span class="sxs-lookup"><span data-stu-id="34e85-258">Array keys are stored in Azure Key Vault with double dashes and numeric key segments (`--0--`, `--1--`, &hellip; `--{n}--`).</span></span>

<span data-ttu-id="34e85-259">檢查下列[Serilog](https://serilog.net/)記錄的 JSON 檔案所提供的提供者組態。</span><span class="sxs-lookup"><span data-stu-id="34e85-259">Examine the following [Serilog](https://serilog.net/) logging provider configuration provided by a JSON file.</span></span> <span data-ttu-id="34e85-260">有兩個物件中定義的常值`WriteTo`陣列，其中會反映兩個 Serilog*接收*，可描述記錄輸出的目的地：</span><span class="sxs-lookup"><span data-stu-id="34e85-260">There are two object literals defined in the `WriteTo` array that reflect two Serilog *sinks*, which describe destinations for logging output:</span></span>

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

<span data-ttu-id="34e85-261">在上述的 JSON 檔案中所示的組態會儲存在 Azure 金鑰保存庫使用雙破折號 (`--`) 標記法和數值的區段：</span><span class="sxs-lookup"><span data-stu-id="34e85-261">The configuration shown in the preceding JSON file is stored in Azure Key Vault using double dash (`--`) notation and numeric segments:</span></span>

| <span data-ttu-id="34e85-262">Key</span><span class="sxs-lookup"><span data-stu-id="34e85-262">Key</span></span> | <span data-ttu-id="34e85-263">值</span><span class="sxs-lookup"><span data-stu-id="34e85-263">Value</span></span> |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="reload-secrets"></a><span data-ttu-id="34e85-264">重新載入祕密</span><span class="sxs-lookup"><span data-stu-id="34e85-264">Reload secrets</span></span>

<span data-ttu-id="34e85-265">密碼會快取直到`IConfigurationRoot.Reload()`呼叫。</span><span class="sxs-lookup"><span data-stu-id="34e85-265">Secrets are cached until `IConfigurationRoot.Reload()` is called.</span></span> <span data-ttu-id="34e85-266">過期，停用，以及更新金鑰保存庫中的祕密未遵守應用程式，直到`Reload`執行。</span><span class="sxs-lookup"><span data-stu-id="34e85-266">Expired, disabled, and updated secrets in the key vault are not respected by the app until `Reload` is executed.</span></span>

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a><span data-ttu-id="34e85-267">已停用和到期的祕密</span><span class="sxs-lookup"><span data-stu-id="34e85-267">Disabled and expired secrets</span></span>

<span data-ttu-id="34e85-268">已停用及過期的密碼會擲回`KeyVaultClientException`。</span><span class="sxs-lookup"><span data-stu-id="34e85-268">Disabled and expired secrets throw a `KeyVaultClientException`.</span></span> <span data-ttu-id="34e85-269">若要防止您的應用程式擲回，取代您的應用程式或更新已停用/過期的祕密。</span><span class="sxs-lookup"><span data-stu-id="34e85-269">To prevent your app from throwing, replace your app or update the disabled/expired secret.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="34e85-270">疑難排解</span><span class="sxs-lookup"><span data-stu-id="34e85-270">Troubleshoot</span></span>

<span data-ttu-id="34e85-271">當應用程式無法載入組態使用的提供者時，錯誤訊息會寫入[ASP.NET Core 記錄基礎結構](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="34e85-271">When the app fails to load configuration using the provider, an error message is written to the [ASP.NET Core Logging infrastructure](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="34e85-272">在下列情況會造成無法載入組態：</span><span class="sxs-lookup"><span data-stu-id="34e85-272">The following conditions will prevent configuration from loading:</span></span>

* <span data-ttu-id="34e85-273">應用程式或憑證未正確設定 Azure Active Directory 中。</span><span class="sxs-lookup"><span data-stu-id="34e85-273">The app or certificate isn't configured correctly in Azure Active Directory.</span></span>
* <span data-ttu-id="34e85-274">金鑰保存庫不存在於 Azure 金鑰保存庫。</span><span class="sxs-lookup"><span data-stu-id="34e85-274">The key vault doesn't exist in Azure Key Vault.</span></span>
* <span data-ttu-id="34e85-275">應用程式未獲授權存取金鑰保存庫。</span><span class="sxs-lookup"><span data-stu-id="34e85-275">The app isn't authorized to access the key vault.</span></span>
* <span data-ttu-id="34e85-276">存取原則不包含`Get`和`List`權限。</span><span class="sxs-lookup"><span data-stu-id="34e85-276">The access policy doesn't include `Get` and `List` permissions.</span></span>
* <span data-ttu-id="34e85-277">在金鑰保存庫中，設定資料 （名稱 / 值組） 錯誤命名為，遺漏，停用，或已過期。</span><span class="sxs-lookup"><span data-stu-id="34e85-277">In the key vault, the configuration data (name-value pair) is incorrectly named, missing, disabled, or expired.</span></span>
* <span data-ttu-id="34e85-278">應用程式有錯誤的金鑰保存庫名稱 (`KeyVaultName`)，Azure AD 應用程式識別碼 (`AzureADApplicationId`)，或 Azure AD 憑證指紋 (`AzureADCertThumbprint`)。</span><span class="sxs-lookup"><span data-stu-id="34e85-278">The app has the wrong key vault name (`KeyVaultName`), Azure AD Application Id (`AzureADApplicationId`), or Azure AD certificate thumbprint (`AzureADCertThumbprint`).</span></span>
* <span data-ttu-id="34e85-279">不正確的值，您想要載入的應用程式中設定金鑰 （名稱）。</span><span class="sxs-lookup"><span data-stu-id="34e85-279">The configuration key (name) is incorrect in the app for the value you're trying to load.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="34e85-280">其他資源</span><span class="sxs-lookup"><span data-stu-id="34e85-280">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* [<span data-ttu-id="34e85-281">Microsoft Azure:金鑰保存庫</span><span class="sxs-lookup"><span data-stu-id="34e85-281">Microsoft Azure: Key Vault</span></span>](https://azure.microsoft.com/services/key-vault/)
* [<span data-ttu-id="34e85-282">Microsoft Azure:Key Vault 文件</span><span class="sxs-lookup"><span data-stu-id="34e85-282">Microsoft Azure: Key Vault Documentation</span></span>](/azure/key-vault/)
* [<span data-ttu-id="34e85-283">如何產生並傳輸受 HSM 保護金鑰的 Azure 金鑰保存庫</span><span class="sxs-lookup"><span data-stu-id="34e85-283">How to generate and transfer HSM-protected keys for Azure Key Vault</span></span>](/azure/key-vault/key-vault-hsm-protected-keys)
* [<span data-ttu-id="34e85-284">KeyVaultClient 類別</span><span class="sxs-lookup"><span data-stu-id="34e85-284">KeyVaultClient Class</span></span>](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
* [<span data-ttu-id="34e85-285">快速入門：設定並從 Azure Key Vault 擷取密碼，利用.NET web 應用程式</span><span class="sxs-lookup"><span data-stu-id="34e85-285">Quickstart: Set and retrieve a secret from Azure Key Vault by using a .NET web app</span></span>](/azure/key-vault/quick-create-net)
* [<span data-ttu-id="34e85-286">教學課程：如何搭配 Azure Windows 虛擬機器在.NET 中使用 Azure 金鑰保存庫</span><span class="sxs-lookup"><span data-stu-id="34e85-286">Tutorial: How to use Azure Key Vault with Azure Windows Virtual Machine in .NET</span></span>](/azure/key-vault/tutorial-net-windows-virtual-machine)
