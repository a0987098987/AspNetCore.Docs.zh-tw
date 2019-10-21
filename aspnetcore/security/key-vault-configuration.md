---
title: ASP.NET Core 中的 Azure Key Vault 設定提供者
author: guardrex
description: 瞭解如何使用 Azure Key Vault 設定提供者，使用在執行時間載入的名稱/值配對來設定應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/14/2019
uid: security/key-vault-configuration
ms.openlocfilehash: c8e76068dbcf2a59a15fa75a1fc5aa0032e6acc5
ms.sourcegitcommit: 07d98ada57f2a5f6d809d44bdad7a15013109549
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/15/2019
ms.locfileid: "72334203"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a><span data-ttu-id="87c12-103">ASP.NET Core 中的 Azure Key Vault 設定提供者</span><span class="sxs-lookup"><span data-stu-id="87c12-103">Azure Key Vault Configuration Provider in ASP.NET Core</span></span>

<span data-ttu-id="87c12-104">By [Luke Latham](https://github.com/guardrex)和[Andrew Stanton-護士](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="87c12-104">By [Luke Latham](https://github.com/guardrex) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="87c12-105">本檔說明如何使用[Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/)設定提供者，從 Azure Key Vault 秘密載入應用程式設定值。</span><span class="sxs-lookup"><span data-stu-id="87c12-105">This document explains how to use the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) Configuration Provider to load app configuration values from Azure Key Vault secrets.</span></span> <span data-ttu-id="87c12-106">Azure Key Vault 是一種雲端式服務，可協助保護應用程式和服務所使用的密碼編譯金鑰和密碼。</span><span class="sxs-lookup"><span data-stu-id="87c12-106">Azure Key Vault is a cloud-based service that assists in safeguarding cryptographic keys and secrets used by apps and services.</span></span> <span data-ttu-id="87c12-107">搭配 ASP.NET Core 應用程式使用 Azure Key Vault 的常見案例包括：</span><span class="sxs-lookup"><span data-stu-id="87c12-107">Common scenarios for using Azure Key Vault with ASP.NET Core apps include:</span></span>

* <span data-ttu-id="87c12-108">控制敏感性設定資料的存取權。</span><span class="sxs-lookup"><span data-stu-id="87c12-108">Controlling access to sensitive configuration data.</span></span>
* <span data-ttu-id="87c12-109">當儲存設定資料時，符合 FIPS 140-2 Level 2 驗證的硬體安全性模組（HSM）的需求。</span><span class="sxs-lookup"><span data-stu-id="87c12-109">Meeting the requirement for FIPS 140-2 Level 2 validated Hardware Security Modules (HSM's) when storing configuration data.</span></span>

<span data-ttu-id="87c12-110">此案例適用于以 ASP.NET Core 2.1 或更新版本為目標的應用程式。</span><span class="sxs-lookup"><span data-stu-id="87c12-110">This scenario is available for apps that target ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="87c12-111">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="87c12-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="packages"></a><span data-ttu-id="87c12-112">package</span><span class="sxs-lookup"><span data-stu-id="87c12-112">Packages</span></span>

<span data-ttu-id="87c12-113">若要使用 Azure Key Vault 設定提供者，請將套件參考新增至[AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/)套件。</span><span class="sxs-lookup"><span data-stu-id="87c12-113">To use the Azure Key Vault Configuration Provider, add a package reference to the [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) package.</span></span>

<span data-ttu-id="87c12-114">若要採用[適用于 Azure 資源的受控](/azure/active-directory/managed-identities-azure-resources/overview)識別案例，請將套件參考新增至[AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/)套件。</span><span class="sxs-lookup"><span data-stu-id="87c12-114">To adopt the [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview) scenario, add a package reference to the [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) package.</span></span>

> [!NOTE]
> <span data-ttu-id="87c12-115">在撰寫本文時，最新穩定版本的 `Microsoft.Azure.Services.AppAuthentication` （版本 `1.0.3`）可支援[系統指派的受控](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-work)識別。</span><span class="sxs-lookup"><span data-stu-id="87c12-115">At the time of writing, the latest stable release of `Microsoft.Azure.Services.AppAuthentication`, version `1.0.3`, provides support for [system-assigned managed identities](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-work).</span></span> <span data-ttu-id="87c12-116">@No__t_1 套件中提供*使用者指派受控*識別的支援。</span><span class="sxs-lookup"><span data-stu-id="87c12-116">Support for *user-assigned managed identities* is available in the `1.2.0-preview2` package.</span></span> <span data-ttu-id="87c12-117">本主題示範如何使用系統管理的身分識別，而提供的範例應用程式會使用 `Microsoft.Azure.Services.AppAuthentication` 套件的版本 `1.0.3`。</span><span class="sxs-lookup"><span data-stu-id="87c12-117">This topic demonstrates the use of system-managed identities, and the provided sample app uses version `1.0.3` of the `Microsoft.Azure.Services.AppAuthentication` package.</span></span>

## <a name="sample-app"></a><span data-ttu-id="87c12-118">範例應用程式</span><span class="sxs-lookup"><span data-stu-id="87c12-118">Sample app</span></span>

<span data-ttu-id="87c12-119">範例應用程式會以*Program.cs*檔案頂端的 `#define` 語句所決定的兩種模式之一來執行：</span><span class="sxs-lookup"><span data-stu-id="87c12-119">The sample app runs in either of two modes determined by the `#define` statement at the top of the *Program.cs* file:</span></span>

* <span data-ttu-id="87c12-120">`Certificate` &ndash; 示範如何使用 Azure Key Vault 用戶端識別碼和 x.509 憑證來存取儲存在 Azure Key Vault 中的秘密。</span><span class="sxs-lookup"><span data-stu-id="87c12-120">`Certificate` &ndash; Demonstrates the use of an Azure Key Vault Client ID and X.509 certificate to access secrets stored in Azure Key Vault.</span></span> <span data-ttu-id="87c12-121">這個版本的範例可以從任何位置執行，部署至 Azure App Service 或任何能夠提供 ASP.NET Core 應用程式的主機。</span><span class="sxs-lookup"><span data-stu-id="87c12-121">This version of the sample can be run from any location, deployed to Azure App Service or any host capable of serving an ASP.NET Core app.</span></span>
* <span data-ttu-id="87c12-122">`Managed` &ndash; 示範如何使用[適用于 Azure 資源的受控](/azure/active-directory/managed-identities-azure-resources/overview)識別來驗證應用程式，以使用 Azure AD 驗證來進行 Azure Key Vault，而不需要在應用程式的程式碼或設定中儲存認證。</span><span class="sxs-lookup"><span data-stu-id="87c12-122">`Managed` &ndash; Demonstrates how to use [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview) to authenticate the app to Azure Key Vault with Azure AD authentication without credentials stored in the app's code or configuration.</span></span> <span data-ttu-id="87c12-123">使用受控識別進行驗證時，不需要 Azure AD 應用程式識別碼和密碼（用戶端密碼）。</span><span class="sxs-lookup"><span data-stu-id="87c12-123">When using managed identities to authenticate, an Azure AD Application ID and Password (Client Secret) aren't required.</span></span> <span data-ttu-id="87c12-124">您必須將範例的 `Managed` 版本部署至 Azure。</span><span class="sxs-lookup"><span data-stu-id="87c12-124">The `Managed` version of the sample must be deployed to Azure.</span></span> <span data-ttu-id="87c12-125">請遵循[使用適用于 Azure 資源的受控](#use-managed-identities-for-azure-resources)識別一節中的指導方針。</span><span class="sxs-lookup"><span data-stu-id="87c12-125">Follow the guidance in the [Use the Managed identities for Azure resources](#use-managed-identities-for-azure-resources) section.</span></span>

<span data-ttu-id="87c12-126">如需有關如何使用預處理器指示詞（`#define`）來設定範例應用程式的詳細資訊，請參閱 <xref:index#preprocessor-directives-in-sample-code>。</span><span class="sxs-lookup"><span data-stu-id="87c12-126">For more information on how to configure a sample app using preprocessor directives (`#define`), see <xref:index#preprocessor-directives-in-sample-code>.</span></span>

## <a name="secret-storage-in-the-development-environment"></a><span data-ttu-id="87c12-127">開發環境中的秘密儲存</span><span class="sxs-lookup"><span data-stu-id="87c12-127">Secret storage in the Development environment</span></span>

<span data-ttu-id="87c12-128">使用[秘密管理員工具](xref:security/app-secrets)在本機設定秘密。</span><span class="sxs-lookup"><span data-stu-id="87c12-128">Set secrets locally using the [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="87c12-129">當範例應用程式在開發環境中的本機電腦上執行時，會從本機密碼管理員存放區載入秘密。</span><span class="sxs-lookup"><span data-stu-id="87c12-129">When the sample app runs on the local machine in the Development environment, secrets are loaded from the local Secret Manager store.</span></span>

<span data-ttu-id="87c12-130">「密碼管理員」工具需要應用程式專案檔中的 `<UserSecretsId>` 屬性。</span><span class="sxs-lookup"><span data-stu-id="87c12-130">The Secret Manager tool requires a `<UserSecretsId>` property in the app's project file.</span></span> <span data-ttu-id="87c12-131">將屬性值（`{GUID}`）設定為任何唯一的 GUID：</span><span class="sxs-lookup"><span data-stu-id="87c12-131">Set the property value (`{GUID}`) to any unique GUID:</span></span>

```xml
<PropertyGroup>
  <UserSecretsId>{GUID}</UserSecretsId>
</PropertyGroup>
```

<span data-ttu-id="87c12-132">密碼會以名稱/值組的形式建立。</span><span class="sxs-lookup"><span data-stu-id="87c12-132">Secrets are created as name-value pairs.</span></span> <span data-ttu-id="87c12-133">階層式值（設定區段）使用 `:` （冒號）做為[ASP.NET Core](xref:fundamentals/configuration/index)設定機碼名稱中的分隔符號。</span><span class="sxs-lookup"><span data-stu-id="87c12-133">Hierarchical values (configuration sections) use a `:` (colon) as a separator in [ASP.NET Core configuration](xref:fundamentals/configuration/index) key names.</span></span>

<span data-ttu-id="87c12-134">秘密管理員會從開啟的命令 shell 使用到專案的[內容根目錄](xref:fundamentals/index#content-root)，其中 `{SECRET NAME}` 是名稱，而 `{SECRET VALUE}` 是值：</span><span class="sxs-lookup"><span data-stu-id="87c12-134">The Secret Manager is used from a command shell opened to the project's [content root](xref:fundamentals/index#content-root), where `{SECRET NAME}` is the name and `{SECRET VALUE}` is the value:</span></span>

```dotnetcli
dotnet user-secrets set "{SECRET NAME}" "{SECRET VALUE}"
```

<span data-ttu-id="87c12-135">在命令 shell 中，從專案的[內容根目錄](xref:fundamentals/index#content-root)執行下列命令，以設定範例應用程式的秘密：</span><span class="sxs-lookup"><span data-stu-id="87c12-135">Execute the following commands in a command shell from the project's [content root](xref:fundamentals/index#content-root) to set the secrets for the sample app:</span></span>

```dotnetcli
dotnet user-secrets set "SecretName" "secret_value_1_dev"
dotnet user-secrets set "Section:SecretName" "secret_value_2_dev"
```

<span data-ttu-id="87c12-136">當這些秘密儲存在具有 Azure Key Vault 區段之[生產環境的秘密儲存](#secret-storage-in-the-production-environment-with-azure-key-vault)Azure Key Vault 中時，`_dev` 的尾碼會變更為 `_prod`。</span><span class="sxs-lookup"><span data-stu-id="87c12-136">When these secrets are stored in Azure Key Vault in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section, the `_dev` suffix is changed to `_prod`.</span></span> <span data-ttu-id="87c12-137">尾碼會在應用程式的輸出中提供視覺提示，指出設定值的來源。</span><span class="sxs-lookup"><span data-stu-id="87c12-137">The suffix provides a visual cue in the app's output indicating the source of the configuration values.</span></span>

## <a name="secret-storage-in-the-production-environment-with-azure-key-vault"></a><span data-ttu-id="87c12-138">在生產環境中使用 Azure Key Vault 的秘密儲存</span><span class="sxs-lookup"><span data-stu-id="87c12-138">Secret storage in the Production environment with Azure Key Vault</span></span>

<span data-ttu-id="87c12-139">以下摘要說明[快速入門：從 Azure Key Vault 使用 Azure CLI 主題設定和取出秘密](/azure/key-vault/quick-create-cli)，以建立 Azure Key Vault 並儲存範例應用程式所使用的秘密。</span><span class="sxs-lookup"><span data-stu-id="87c12-139">The instructions provided by the [Quickstart: Set and retrieve a secret from Azure Key Vault using Azure CLI](/azure/key-vault/quick-create-cli) topic are summarized here for creating an Azure Key Vault and storing secrets used by the sample app.</span></span> <span data-ttu-id="87c12-140">如需進一步的詳細資料，請參閱主題。</span><span class="sxs-lookup"><span data-stu-id="87c12-140">Refer to the topic for further details.</span></span>

1. <span data-ttu-id="87c12-141">使用下列其中一種方法，在[Azure 入口網站](https://portal.azure.com/)中開啟 Azure Cloud shell：</span><span class="sxs-lookup"><span data-stu-id="87c12-141">Open Azure Cloud shell using any one of the following methods in the [Azure portal](https://portal.azure.com/):</span></span>

   * <span data-ttu-id="87c12-142">選取程式碼區塊右上角的 [**試試看**]。</span><span class="sxs-lookup"><span data-stu-id="87c12-142">Select **Try It** in the upper-right corner of a code block.</span></span> <span data-ttu-id="87c12-143">在文字方塊中使用搜尋字串 "Azure CLI"。</span><span class="sxs-lookup"><span data-stu-id="87c12-143">Use the search string "Azure CLI" in the text box.</span></span>
   * <span data-ttu-id="87c12-144">使用 [**啟動 Cloud Shell** ] 按鈕，在瀏覽器中開啟 Cloud Shell。</span><span class="sxs-lookup"><span data-stu-id="87c12-144">Open Cloud Shell in your browser with the **Launch Cloud Shell** button.</span></span>
   * <span data-ttu-id="87c12-145">在 Azure 入口網站右上角的功能表上，選取 [ **Cloud Shell** ] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="87c12-145">Select the **Cloud Shell** button on the menu in the upper-right corner of the Azure portal.</span></span>

   <span data-ttu-id="87c12-146">如需詳細資訊，請參閱[Azure 命令列介面（CLI）](/cli/azure/)和[Azure Cloud Shell 的總覽](/azure/cloud-shell/overview)。</span><span class="sxs-lookup"><span data-stu-id="87c12-146">For more information, see [Azure Command-Line Interface (CLI)](/cli/azure/) and [Overview of Azure Cloud Shell](/azure/cloud-shell/overview).</span></span>

1. <span data-ttu-id="87c12-147">如果尚未驗證，請使用 `az login` 命令登入。</span><span class="sxs-lookup"><span data-stu-id="87c12-147">If you aren't already authenticated, sign in with the `az login` command.</span></span>

1. <span data-ttu-id="87c12-148">使用下列命令建立資源群組，其中 `{RESOURCE GROUP NAME}` 是新資源群組的資源組名，而 `{LOCATION}` 是 Azure 區域（datacenter）：</span><span class="sxs-lookup"><span data-stu-id="87c12-148">Create a resource group with the following command, where `{RESOURCE GROUP NAME}` is the resource group name for the new resource group and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```azure-cli
   az group create --name "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="87c12-149">使用下列命令，在資源群組中建立金鑰保存庫，其中 `{KEY VAULT NAME}` 是新金鑰保存庫的名稱，而 `{LOCATION}` 是 Azure 區域（datacenter）：</span><span class="sxs-lookup"><span data-stu-id="87c12-149">Create a key vault in the resource group with the following command, where `{KEY VAULT NAME}` is the name for the new key vault and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```azure-cli
   az keyvault create --name "{KEY VAULT NAME}" --resource-group "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="87c12-150">在金鑰保存庫中建立秘密，做為名稱/值配對。</span><span class="sxs-lookup"><span data-stu-id="87c12-150">Create secrets in the key vault as name-value pairs.</span></span>

   <span data-ttu-id="87c12-151">Azure Key Vault 秘密名稱僅限於英數位元和連字號。</span><span class="sxs-lookup"><span data-stu-id="87c12-151">Azure Key Vault secret names are limited to alphanumeric characters and dashes.</span></span> <span data-ttu-id="87c12-152">階層式值（設定區段）使用 `--` （兩個虛線）做為分隔符號。</span><span class="sxs-lookup"><span data-stu-id="87c12-152">Hierarchical values (configuration sections) use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="87c12-153">冒號，通常用來從[ASP.NET Core](xref:fundamentals/configuration/index)設定中的子機碼分隔區段，但不允許用在金鑰保存庫密碼名稱中。</span><span class="sxs-lookup"><span data-stu-id="87c12-153">Colons, which are normally used to delimit a section from a subkey in [ASP.NET Core configuration](xref:fundamentals/configuration/index), aren't allowed in key vault secret names.</span></span> <span data-ttu-id="87c12-154">因此，當密碼載入應用程式的設定時，會使用兩個破折號並交換冒號。</span><span class="sxs-lookup"><span data-stu-id="87c12-154">Therefore, two dashes are used and swapped for a colon when the secrets are loaded into the app's configuration.</span></span>

   <span data-ttu-id="87c12-155">下列秘密可用於範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="87c12-155">The following secrets are for use with the sample app.</span></span> <span data-ttu-id="87c12-156">這些值包含 `_prod` 後置詞，以與在開發環境中從使用者秘密載入的 `_dev` 尾碼值進行區別。</span><span class="sxs-lookup"><span data-stu-id="87c12-156">The values include a `_prod` suffix to distinguish them from the `_dev` suffix values loaded in the Development environment from User Secrets.</span></span> <span data-ttu-id="87c12-157">以您在上一個步驟中建立的金鑰保存庫名稱取代 `{KEY VAULT NAME}`：</span><span class="sxs-lookup"><span data-stu-id="87c12-157">Replace `{KEY VAULT NAME}` with the name of the key vault that you created in the prior step:</span></span>

   ```azure-cli
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "SecretName" --value "secret_value_1_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "Section--SecretName" --value "secret_value_2_prod"
   ```

## <a name="use-application-id-and-x509-certificate-for-non-azure-hosted-apps"></a><span data-ttu-id="87c12-158">將應用程式識別碼和 x.509 憑證用於非 Azure 託管的應用程式</span><span class="sxs-lookup"><span data-stu-id="87c12-158">Use Application ID and X.509 certificate for non-Azure-hosted apps</span></span>

<span data-ttu-id="87c12-159">設定 Azure AD、Azure Key Vault 和應用程式，以在應用程式裝載于**Azure 外部時**，使用 Azure Active Directory 應用程式識別碼和 x.509 憑證來驗證金鑰保存庫。</span><span class="sxs-lookup"><span data-stu-id="87c12-159">Configure Azure AD, Azure Key Vault, and the app to use an Azure Active Directory Application ID and X.509 certificate to authenticate to a key vault **when the app is hosted outside of Azure**.</span></span> <span data-ttu-id="87c12-160">如需詳細資訊，請參閱[關於金鑰、秘密和憑證](/azure/key-vault/about-keys-secrets-and-certificates)。</span><span class="sxs-lookup"><span data-stu-id="87c12-160">For more information, see [About keys, secrets, and certificates](/azure/key-vault/about-keys-secrets-and-certificates).</span></span>

> [!NOTE]
> <span data-ttu-id="87c12-161">雖然 Azure 中裝載的應用程式支援使用應用程式識別碼和 x.509 憑證，但建議您在 Azure 中裝載應用程式時，使用[適用于 azure 資源的受控](#use-managed-identities-for-azure-resources)識別。</span><span class="sxs-lookup"><span data-stu-id="87c12-161">Although using an Application ID and X.509 certificate is supported for apps hosted in Azure, we recommend using [Managed identities for Azure resources](#use-managed-identities-for-azure-resources) when hosting an app in Azure.</span></span> <span data-ttu-id="87c12-162">受控識別不需要在應用程式中或在開發環境中儲存憑證。</span><span class="sxs-lookup"><span data-stu-id="87c12-162">Managed identities don't require storing a certificate in the app or in the development environment.</span></span>

<span data-ttu-id="87c12-163">當*Program.cs*檔案頂端的 `#define` 語句設定為 `Certificate` 時，範例應用程式會使用應用程式識別碼和 x.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="87c12-163">The sample app uses an Application ID and X.509 certificate when the `#define` statement at the top of the *Program.cs* file is set to `Certificate`.</span></span>

1. <span data-ttu-id="87c12-164">建立 PKCS # 12 封存檔案（ *.pfx*）憑證。</span><span class="sxs-lookup"><span data-stu-id="87c12-164">Create a PKCS#12 archive (*.pfx*) certificate.</span></span> <span data-ttu-id="87c12-165">建立憑證的選項包括[Windows](/windows/desktop/seccrypto/makecert)和[OpenSSL](https://www.openssl.org/)上的 MakeCert。</span><span class="sxs-lookup"><span data-stu-id="87c12-165">Options for creating certificates include [MakeCert on Windows](/windows/desktop/seccrypto/makecert) and [OpenSSL](https://www.openssl.org/).</span></span>
1. <span data-ttu-id="87c12-166">將憑證安裝到目前使用者的個人憑證存儲。</span><span class="sxs-lookup"><span data-stu-id="87c12-166">Install the certificate into the current user's personal certificate store.</span></span> <span data-ttu-id="87c12-167">將金鑰標示為可匯出是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="87c12-167">Marking the key as exportable is optional.</span></span> <span data-ttu-id="87c12-168">記下憑證的指紋，這會在此程式稍後使用。</span><span class="sxs-lookup"><span data-stu-id="87c12-168">Note the certificate's thumbprint, which is used later in this process.</span></span>
1. <span data-ttu-id="87c12-169">將 PKCS # 12 封存（ *.pfx*）憑證匯出為 DER 編碼憑證（ *.cer*）。</span><span class="sxs-lookup"><span data-stu-id="87c12-169">Export the PKCS#12 archive (*.pfx*) certificate as a DER-encoded certificate (*.cer*).</span></span>
1. <span data-ttu-id="87c12-170">使用 Azure AD （**應用程式註冊**）註冊應用程式。</span><span class="sxs-lookup"><span data-stu-id="87c12-170">Register the app with Azure AD (**App registrations**).</span></span>
1. <span data-ttu-id="87c12-171">將 DER 編碼的憑證（ *.cer*）上傳至 Azure AD：</span><span class="sxs-lookup"><span data-stu-id="87c12-171">Upload the DER-encoded certificate (*.cer*) to Azure AD:</span></span>
   1. <span data-ttu-id="87c12-172">在 Azure AD 中選取應用程式。</span><span class="sxs-lookup"><span data-stu-id="87c12-172">Select the app in Azure AD.</span></span>
   1. <span data-ttu-id="87c12-173">流覽至 [**憑證 & 密碼**]。</span><span class="sxs-lookup"><span data-stu-id="87c12-173">Navigate to **Certificates & secrets**.</span></span>
   1. <span data-ttu-id="87c12-174">選取 [**上傳憑證**] 來上傳包含公開金鑰的憑證。</span><span class="sxs-lookup"><span data-stu-id="87c12-174">Select **Upload certificate** to upload the certificate, which contains the public key.</span></span> <span data-ttu-id="87c12-175">*.Cer*、 *pem*或 *.crt*憑證是可接受的。</span><span class="sxs-lookup"><span data-stu-id="87c12-175">A *.cer*, *.pem*, or *.crt* certificate is acceptable.</span></span>
1. <span data-ttu-id="87c12-176">將金鑰保存庫名稱、應用程式識別碼和憑證指紋儲存在應用程式的*appsettings*中。</span><span class="sxs-lookup"><span data-stu-id="87c12-176">Store the key vault name, Application ID, and certificate thumbprint in the app's *appsettings.json* file.</span></span>
1. <span data-ttu-id="87c12-177">流覽至 Azure 入口網站中的 [**金鑰保存庫**]。</span><span class="sxs-lookup"><span data-stu-id="87c12-177">Navigate to **Key vaults** in the Azure portal.</span></span>
1. <span data-ttu-id="87c12-178">選取您在[生產環境中使用 Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault)一節所建立的金鑰保存庫。</span><span class="sxs-lookup"><span data-stu-id="87c12-178">Select the key vault that you created in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section.</span></span>
1. <span data-ttu-id="87c12-179">選取 [**存取原則**]。</span><span class="sxs-lookup"><span data-stu-id="87c12-179">Select **Access policies**.</span></span>
1. <span data-ttu-id="87c12-180">選取 [**加入新**的]。</span><span class="sxs-lookup"><span data-stu-id="87c12-180">Select **Add new**.</span></span>
1. <span data-ttu-id="87c12-181">選取 [**選取主體**]，然後依名稱選取已註冊的應用程式。</span><span class="sxs-lookup"><span data-stu-id="87c12-181">Select **Select principal** and select the registered app by name.</span></span> <span data-ttu-id="87c12-182">選取 [**選取**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="87c12-182">Select the **Select** button.</span></span>
1. <span data-ttu-id="87c12-183">開啟 [**秘密許可權**]，並提供具有 [**取得**] 和 [**列出**] 許可權的應用程式。</span><span class="sxs-lookup"><span data-stu-id="87c12-183">Open **Secret permissions** and provide the app with **Get** and **List** permissions.</span></span>
1. <span data-ttu-id="87c12-184">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="87c12-184">Select **OK**.</span></span>
1. <span data-ttu-id="87c12-185">選取 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="87c12-185">Select **Save**.</span></span>
1. <span data-ttu-id="87c12-186">部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="87c12-186">Deploy the app.</span></span>

<span data-ttu-id="87c12-187">@No__t_0 範例應用程式會從 `IConfigurationRoot` 與秘密名稱相同的名稱取得其設定值：</span><span class="sxs-lookup"><span data-stu-id="87c12-187">The `Certificate` sample app obtains its configuration values from `IConfigurationRoot` with the same name as the secret name:</span></span>

* <span data-ttu-id="87c12-188">非階層式值： `SecretName` 的值是以 `config["SecretName"]` 取得。</span><span class="sxs-lookup"><span data-stu-id="87c12-188">Non-hierarchical values: The value for `SecretName` is obtained with `config["SecretName"]`.</span></span>
* <span data-ttu-id="87c12-189">階層式值（區段）：使用 `:` （冒號）標記法或 `GetSection` 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="87c12-189">Hierarchical values (sections): Use `:` (colon) notation or the `GetSection` extension method.</span></span> <span data-ttu-id="87c12-190">使用下列其中一種方法來取得設定值：</span><span class="sxs-lookup"><span data-stu-id="87c12-190">Use either of these approaches to obtain the configuration value:</span></span>
  * `config["Section:SecretName"]`
  * `config.GetSection("Section")["SecretName"]`

<span data-ttu-id="87c12-191">X.509 憑證是由作業系統所管理。</span><span class="sxs-lookup"><span data-stu-id="87c12-191">The X.509 certificate is managed by the OS.</span></span> <span data-ttu-id="87c12-192">應用程式會使用*appsettings*所提供的值來呼叫 `AddAzureKeyVault`：</span><span class="sxs-lookup"><span data-stu-id="87c12-192">The app calls `AddAzureKeyVault` with values supplied by the *appsettings.json* file:</span></span>

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet1&highlight=20-23)]

<span data-ttu-id="87c12-193">範例值：</span><span class="sxs-lookup"><span data-stu-id="87c12-193">Example values:</span></span>

* <span data-ttu-id="87c12-194">金鑰保存庫名稱： `contosovault`</span><span class="sxs-lookup"><span data-stu-id="87c12-194">Key vault name: `contosovault`</span></span>
* <span data-ttu-id="87c12-195">應用程式識別碼： `627e911e-43cc-61d4-992e-12db9c81b413`</span><span class="sxs-lookup"><span data-stu-id="87c12-195">Application ID: `627e911e-43cc-61d4-992e-12db9c81b413`</span></span>
* <span data-ttu-id="87c12-196">憑證指紋： `fe14593dd66b2406c5269d742d04b6e1ab03adb1`</span><span class="sxs-lookup"><span data-stu-id="87c12-196">Certificate thumbprint: `fe14593dd66b2406c5269d742d04b6e1ab03adb1`</span></span>

<span data-ttu-id="87c12-197">*appsettings.json*：</span><span class="sxs-lookup"><span data-stu-id="87c12-197">*appsettings.json*:</span></span>

[!code-json[](key-vault-configuration/sample/appsettings.json)]

<span data-ttu-id="87c12-198">當您執行應用程式時，網頁會顯示已載入的密碼值。</span><span class="sxs-lookup"><span data-stu-id="87c12-198">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="87c12-199">在開發環境中，會以 `_dev` 尾碼來載入密碼值。</span><span class="sxs-lookup"><span data-stu-id="87c12-199">In the Development environment, secret values load with the `_dev` suffix.</span></span> <span data-ttu-id="87c12-200">在生產環境中，值會以 `_prod` 尾碼來載入。</span><span class="sxs-lookup"><span data-stu-id="87c12-200">In the Production environment, the values load with the `_prod` suffix.</span></span>

## <a name="use-managed-identities-for-azure-resources"></a><span data-ttu-id="87c12-201">使用適用于 Azure 資源的受控識別</span><span class="sxs-lookup"><span data-stu-id="87c12-201">Use Managed identities for Azure resources</span></span>

<span data-ttu-id="87c12-202">**部署至 azure 的應用程式**可以利用[Azure 資源的受控](/azure/active-directory/managed-identities-azure-resources/overview)識別，讓應用程式使用不需認證的 Azure AD 驗證（應用程式識別碼和密碼/用戶端密碼）驗證 Azure Key Vault儲存在應用程式中。</span><span class="sxs-lookup"><span data-stu-id="87c12-202">**An app deployed to Azure** can take advantage of [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview), which allows the app to authenticate with Azure Key Vault using Azure AD authentication without credentials (Application ID and Password/Client Secret) stored in the app.</span></span>

<span data-ttu-id="87c12-203">當*Program.cs*檔案頂端的 `#define` 語句設定為 `Managed` 時，範例應用程式會使用適用于 Azure 資源的受控識別。</span><span class="sxs-lookup"><span data-stu-id="87c12-203">The sample app uses Managed identities for Azure resources when the `#define` statement at the top of the *Program.cs* file is set to `Managed`.</span></span>

<span data-ttu-id="87c12-204">在應用程式的*appsettings*中輸入保存庫名稱。</span><span class="sxs-lookup"><span data-stu-id="87c12-204">Enter the vault name into the app's *appsettings.json* file.</span></span> <span data-ttu-id="87c12-205">當設定為 `Managed` 版本時，範例應用程式不需要應用程式識別碼和密碼（用戶端秘密），因此您可以忽略這些設定專案。</span><span class="sxs-lookup"><span data-stu-id="87c12-205">The sample app doesn't require an Application ID and Password (Client Secret) when set to the `Managed` version, so you can ignore those configuration entries.</span></span> <span data-ttu-id="87c12-206">應用程式會部署至 Azure，而 Azure 只會使用儲存在*appsettings*中的保存庫名稱來驗證應用程式，以存取 Azure Key Vault。</span><span class="sxs-lookup"><span data-stu-id="87c12-206">The app is deployed to Azure, and Azure authenticates the app to access Azure Key Vault only using the vault name stored in the *appsettings.json* file.</span></span>

<span data-ttu-id="87c12-207">將範例應用程式部署至 Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="87c12-207">Deploy the sample app to Azure App Service.</span></span>

<span data-ttu-id="87c12-208">部署到 Azure App Service 的應用程式會在建立服務時，自動向 Azure AD 註冊。</span><span class="sxs-lookup"><span data-stu-id="87c12-208">An app deployed to Azure App Service is automatically registered with Azure AD when the service is created.</span></span> <span data-ttu-id="87c12-209">從部署取得物件識別碼，以便在下列命令中使用。</span><span class="sxs-lookup"><span data-stu-id="87c12-209">Obtain the Object ID from the deployment for use in the following command.</span></span> <span data-ttu-id="87c12-210">物件識別碼會顯示在 App Service 的 身分**識別** 面板上的 Azure 入口網站。</span><span class="sxs-lookup"><span data-stu-id="87c12-210">The Object ID is shown in the Azure portal on the **Identity** panel of the App Service.</span></span>

<span data-ttu-id="87c12-211">使用 Azure CLI 和應用程式的物件識別碼，為應用程式提供 `list` 和 `get` 許可權以存取金鑰保存庫：</span><span class="sxs-lookup"><span data-stu-id="87c12-211">Using Azure CLI and the app's Object ID, provide the app with `list` and `get` permissions to access the key vault:</span></span>

```azure-cli
az keyvault set-policy --name '{KEY VAULT NAME}' --object-id {OBJECT ID} --secret-permissions get list
```

<span data-ttu-id="87c12-212">使用 Azure CLI、PowerShell 或 Azure 入口網站**重新開機應用程式**。</span><span class="sxs-lookup"><span data-stu-id="87c12-212">**Restart the app** using Azure CLI, PowerShell, or the Azure portal.</span></span>

<span data-ttu-id="87c12-213">範例應用程式：</span><span class="sxs-lookup"><span data-stu-id="87c12-213">The sample app:</span></span>

* <span data-ttu-id="87c12-214">建立 `AzureServiceTokenProvider` 類別的實例，而不含連接字串。</span><span class="sxs-lookup"><span data-stu-id="87c12-214">Creates an instance of the `AzureServiceTokenProvider` class without a connection string.</span></span> <span data-ttu-id="87c12-215">未提供連接字串時，提供者會嘗試從 Azure 資源的受控識別取得存取權杖。</span><span class="sxs-lookup"><span data-stu-id="87c12-215">When a connection string isn't provided, the provider attempts to obtain an access token from Managed identities for Azure resources.</span></span>
* <span data-ttu-id="87c12-216">新的 `KeyVaultClient` 會使用 `AzureServiceTokenProvider` 實例權杖回呼來建立。</span><span class="sxs-lookup"><span data-stu-id="87c12-216">A new `KeyVaultClient` is created with the `AzureServiceTokenProvider` instance token callback.</span></span>
* <span data-ttu-id="87c12-217">@No__t_0 實例會搭配使用 `IKeyVaultSecretManager` 的預設執行，以載入所有秘密值，並以冒號（`:`）取代索引鍵名稱中的雙破折號（`--`）。</span><span class="sxs-lookup"><span data-stu-id="87c12-217">The `KeyVaultClient` instance is used with a default implementation of `IKeyVaultSecretManager` that loads all secret values and replaces double-dashes (`--`) with colons (`:`) in key names.</span></span>

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet2&highlight=13-21)]

<span data-ttu-id="87c12-218">金鑰保存庫名稱範例值： `contosovault`</span><span class="sxs-lookup"><span data-stu-id="87c12-218">Key vault name example value: `contosovault`</span></span>
    
<span data-ttu-id="87c12-219">*appsettings.json*：</span><span class="sxs-lookup"><span data-stu-id="87c12-219">*appsettings.json*:</span></span>

```json
{
  "KeyVaultName": "Key Vault Name"
}
```

<span data-ttu-id="87c12-220">當您執行應用程式時，網頁會顯示已載入的密碼值。</span><span class="sxs-lookup"><span data-stu-id="87c12-220">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="87c12-221">在開發環境中，秘密值具有 `_dev` 尾碼，因為它們是由使用者密碼提供。</span><span class="sxs-lookup"><span data-stu-id="87c12-221">In the Development environment, secret values have the `_dev` suffix because they're provided by User Secrets.</span></span> <span data-ttu-id="87c12-222">在生產環境中，值會以 `_prod` 尾碼載入，因為它們是由 Azure Key Vault 提供。</span><span class="sxs-lookup"><span data-stu-id="87c12-222">In the Production environment, the values load with the `_prod` suffix because they're provided by Azure Key Vault.</span></span>

<span data-ttu-id="87c12-223">如果您收到 `Access denied` 錯誤，請確認已向 Azure AD 註冊應用程式，並提供金鑰保存庫的存取權。</span><span class="sxs-lookup"><span data-stu-id="87c12-223">If you receive an `Access denied` error, confirm that the app is registered with Azure AD and provided access to the key vault.</span></span> <span data-ttu-id="87c12-224">確認您已在 Azure 中重新開機服務。</span><span class="sxs-lookup"><span data-stu-id="87c12-224">Confirm that you've restarted the service in Azure.</span></span>

## <a name="use-a-key-name-prefix"></a><span data-ttu-id="87c12-225">使用索引鍵名稱前置詞</span><span class="sxs-lookup"><span data-stu-id="87c12-225">Use a key name prefix</span></span>

<span data-ttu-id="87c12-226">`AddAzureKeyVault` 提供的多載可接受 `IKeyVaultSecretManager` 的執行，可讓您控制如何將金鑰保存庫密碼轉換成設定金鑰。</span><span class="sxs-lookup"><span data-stu-id="87c12-226">`AddAzureKeyVault` provides an overload that accepts an implementation of `IKeyVaultSecretManager`, which allows you to control how key vault secrets are converted into configuration keys.</span></span> <span data-ttu-id="87c12-227">例如，您可以根據您在應用程式啟動時提供的首碼值，執行介面來載入密碼值。</span><span class="sxs-lookup"><span data-stu-id="87c12-227">For example, you can implement the interface to load secret values based on a prefix value you provide at app startup.</span></span> <span data-ttu-id="87c12-228">例如，這可讓您根據應用程式的版本來載入密碼。</span><span class="sxs-lookup"><span data-stu-id="87c12-228">This allows you, for example, to load secrets based on the version of the app.</span></span>

> [!WARNING]
> <span data-ttu-id="87c12-229">請勿在金鑰保存庫秘密上使用前置詞，將多個應用程式的秘密放入相同的金鑰保存庫，或將環境秘密（例如*開發*與*生產*密碼）放入相同的保存庫中。</span><span class="sxs-lookup"><span data-stu-id="87c12-229">Don't use prefixes on key vault secrets to place secrets for multiple apps into the same key vault or to place environmental secrets (for example, *development* versus *production* secrets) into the same vault.</span></span> <span data-ttu-id="87c12-230">我們建議不同的應用程式和開發/生產環境使用不同的金鑰保存庫，以隔離最高安全性層級的應用程式環境。</span><span class="sxs-lookup"><span data-stu-id="87c12-230">We recommend that different apps and development/production environments use separate key vaults to isolate app environments for the highest level of security.</span></span>

<span data-ttu-id="87c12-231">在下列範例中，會在金鑰保存庫中建立秘密（並使用適用于開發環境的密碼管理員工具）進行 `5000-AppSecret` （金鑰保存庫密碼名稱中不允許期間）。</span><span class="sxs-lookup"><span data-stu-id="87c12-231">In the following example, a secret is established in the key vault (and using the Secret Manager tool for the Development environment) for `5000-AppSecret` (periods aren't allowed in key vault secret names).</span></span> <span data-ttu-id="87c12-232">此秘密代表應用程式版本5.0.0.0 的應用程式密碼。</span><span class="sxs-lookup"><span data-stu-id="87c12-232">This secret represents an app secret for version 5.0.0.0 of the app.</span></span> <span data-ttu-id="87c12-233">針對其他版本的應用程式（5.1.0.0），會將密碼新增至金鑰保存庫（並使用秘密管理員工具）進行 `5100-AppSecret`。</span><span class="sxs-lookup"><span data-stu-id="87c12-233">For another version of the app, 5.1.0.0, a secret is added to the key vault (and using the Secret Manager tool) for `5100-AppSecret`.</span></span> <span data-ttu-id="87c12-234">每個應用程式版本都會將其版本設定的秘密值載入至其設定中做為 `AppSecret`，並在載入秘密時去除版本。</span><span class="sxs-lookup"><span data-stu-id="87c12-234">Each app version loads its versioned secret value into its configuration as `AppSecret`, stripping off the version as it loads the secret.</span></span>

<span data-ttu-id="87c12-235">使用自訂 `IKeyVaultSecretManager` 呼叫 `AddAzureKeyVault`：</span><span class="sxs-lookup"><span data-stu-id="87c12-235">`AddAzureKeyVault` is called with a custom `IKeyVaultSecretManager`:</span></span>

[!code-csharp[](key-vault-configuration/sample_snapshot/Program.cs?highlight=30-34)]

<span data-ttu-id="87c12-236">@No__t_0 的執行會回應密碼的版本前置詞，以將適當的密碼載入設定中：</span><span class="sxs-lookup"><span data-stu-id="87c12-236">The `IKeyVaultSecretManager` implementation reacts to the version prefixes of secrets to load the proper secret into configuration:</span></span>

[!code-csharp[](key-vault-configuration/sample_snapshot/Startup.cs?name=snippet1)]

<span data-ttu-id="87c12-237">@No__t_0 方法是由提供者演算法所呼叫，它會逐一查看保存庫秘密，以尋找具有版本前置詞的金鑰。</span><span class="sxs-lookup"><span data-stu-id="87c12-237">The `Load` method is called by a provider algorithm that iterates through the vault secrets to find the ones that have the version prefix.</span></span> <span data-ttu-id="87c12-238">當找到具有 `Load` 的版本前置詞時，演算法會使用 `GetKey` 方法來傳回密碼名稱的設定名稱。</span><span class="sxs-lookup"><span data-stu-id="87c12-238">When a version prefix is found with `Load`, the algorithm uses the `GetKey` method to return the configuration name of the secret name.</span></span> <span data-ttu-id="87c12-239">它會從密碼的名稱中去除版本前置詞，並傳回其餘的秘密名稱，以載入至應用程式的設定名稱/值配對。</span><span class="sxs-lookup"><span data-stu-id="87c12-239">It strips off the version prefix from the secret's name and returns the rest of the secret name for loading into the app's configuration name-value pairs.</span></span>

<span data-ttu-id="87c12-240">當此方法執行時：</span><span class="sxs-lookup"><span data-stu-id="87c12-240">When this approach is implemented:</span></span>

1. <span data-ttu-id="87c12-241">應用程式的專案檔中指定的應用程式版本。</span><span class="sxs-lookup"><span data-stu-id="87c12-241">The app's version specified in the app's project file.</span></span> <span data-ttu-id="87c12-242">在下列範例中，應用程式的版本設定為 `5.0.0.0`：</span><span class="sxs-lookup"><span data-stu-id="87c12-242">In the following example, the app's version is set to `5.0.0.0`:</span></span>

   ```xml
   <PropertyGroup>
     <Version>5.0.0.0</Version>
   </PropertyGroup>
   ```

1. <span data-ttu-id="87c12-243">確認 `<UserSecretsId>` 屬性存在於應用程式的專案檔中，其中 `{GUID}` 是使用者提供的 GUID：</span><span class="sxs-lookup"><span data-stu-id="87c12-243">Confirm that a `<UserSecretsId>` property is present in the app's project file, where `{GUID}` is a user-supplied GUID:</span></span>

   ```xml
   <PropertyGroup>
     <UserSecretsId>{GUID}</UserSecretsId>
   </PropertyGroup>
   ```

   <span data-ttu-id="87c12-244">使用[秘密管理員工具](xref:security/app-secrets)在本機儲存下列秘密：</span><span class="sxs-lookup"><span data-stu-id="87c12-244">Save the following secrets locally with the [Secret Manager tool](xref:security/app-secrets):</span></span>

   ```dotnetcli
   dotnet user-secrets set "5000-AppSecret" "5.0.0.0_secret_value_dev"
   dotnet user-secrets set "5100-AppSecret" "5.1.0.0_secret_value_dev"
   ```

1. <span data-ttu-id="87c12-245">使用下列 Azure CLI 命令，將秘密儲存在 Azure Key Vault 中：</span><span class="sxs-lookup"><span data-stu-id="87c12-245">Secrets are saved in Azure Key Vault using the following Azure CLI commands:</span></span>

   ```azure-cli
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5000-AppSecret" --value "5.0.0.0_secret_value_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5100-AppSecret" --value "5.1.0.0_secret_value_prod"
   ```

1. <span data-ttu-id="87c12-246">當應用程式執行時，會載入金鑰保存庫密碼。</span><span class="sxs-lookup"><span data-stu-id="87c12-246">When the app is run, the key vault secrets are loaded.</span></span> <span data-ttu-id="87c12-247">@No__t_0 的字串秘密符合應用程式的專案檔（`5.0.0.0`）中所指定的應用程式版本。</span><span class="sxs-lookup"><span data-stu-id="87c12-247">The string secret for `5000-AppSecret` is matched to the app's version specified in the app's project file (`5.0.0.0`).</span></span>

1. <span data-ttu-id="87c12-248">會從索引鍵名稱中移除版本 `5000` （含破折號）。</span><span class="sxs-lookup"><span data-stu-id="87c12-248">The version, `5000` (with the dash), is stripped from the key name.</span></span> <span data-ttu-id="87c12-249">在整個應用程式中，使用金鑰來讀取設定 `AppSecret` 會載入秘密值。</span><span class="sxs-lookup"><span data-stu-id="87c12-249">Throughout the app, reading configuration with the key `AppSecret` loads the secret value.</span></span>

1. <span data-ttu-id="87c12-250">如果專案檔中的應用程式版本變更為 `5.1.0.0`，而應用程式再次執行，則傳回的秘密值會在開發環境中 `5.1.0.0_secret_value_dev`，並在生產環境中 `5.1.0.0_secret_value_prod`。</span><span class="sxs-lookup"><span data-stu-id="87c12-250">If the app's version is changed in the project file to `5.1.0.0` and the app is run again, the secret value returned is `5.1.0.0_secret_value_dev` in the Development environment and `5.1.0.0_secret_value_prod` in Production.</span></span>

> [!NOTE]
> <span data-ttu-id="87c12-251">您也可以提供您自己的 `KeyVaultClient` 實作為 `AddAzureKeyVault`。</span><span class="sxs-lookup"><span data-stu-id="87c12-251">You can also provide your own `KeyVaultClient` implementation to `AddAzureKeyVault`.</span></span> <span data-ttu-id="87c12-252">自訂用戶端允許跨應用程式共用單一用戶端實例。</span><span class="sxs-lookup"><span data-stu-id="87c12-252">A custom client permits sharing a single instance of the client across the app.</span></span>

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="87c12-253">將陣列繫結到類別</span><span class="sxs-lookup"><span data-stu-id="87c12-253">Bind an array to a class</span></span>

<span data-ttu-id="87c12-254">提供者能夠將設定值讀入陣列中，以系結至 POCO 陣列。</span><span class="sxs-lookup"><span data-stu-id="87c12-254">The provider is capable of reading configuration values into an array for binding to a POCO array.</span></span>

<span data-ttu-id="87c12-255">從允許金鑰包含冒號（`:`）分隔符號的設定來源讀取時，會使用數位索引鍵區段來區別組成陣列的索引鍵（`:0:`、`:1:` 。</span><span class="sxs-lookup"><span data-stu-id="87c12-255">When reading from a configuration source that allows keys to contain colon (`:`) separators, a numeric key segment is used to distinguish the keys that make up an array (`:0:`, `:1:`, …</span></span> <span data-ttu-id="87c12-256">`:{n}:`)。</span><span class="sxs-lookup"><span data-stu-id="87c12-256">`:{n}:`).</span></span> <span data-ttu-id="87c12-257">如需詳細資訊，請參閱[Configuration：將陣列系結至類別](xref:fundamentals/configuration/index#bind-an-array-to-a-class)。</span><span class="sxs-lookup"><span data-stu-id="87c12-257">For more information, see [Configuration: Bind an array to a class](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span></span>

<span data-ttu-id="87c12-258">Azure Key Vault 索引鍵不能使用冒號做為分隔符號。</span><span class="sxs-lookup"><span data-stu-id="87c12-258">Azure Key Vault keys can't use a colon as a separator.</span></span> <span data-ttu-id="87c12-259">本主題中所述的方法會使用雙虛線（`--`）做為階層式值的分隔符號（區段）。</span><span class="sxs-lookup"><span data-stu-id="87c12-259">The approach described in this topic uses double dashes (`--`) as a separator for hierarchical values (sections).</span></span> <span data-ttu-id="87c12-260">陣列索引鍵會以雙虛線和數位索引鍵區段（`--0--`、`--1--`、&hellip; `--{n}--`）儲存在 Azure Key Vault 中。</span><span class="sxs-lookup"><span data-stu-id="87c12-260">Array keys are stored in Azure Key Vault with double dashes and numeric key segments (`--0--`, `--1--`, &hellip; `--{n}--`).</span></span>

<span data-ttu-id="87c12-261">檢查 JSON 檔案所提供的下列[Serilog](https://serilog.net/)記錄提供者設定。</span><span class="sxs-lookup"><span data-stu-id="87c12-261">Examine the following [Serilog](https://serilog.net/) logging provider configuration provided by a JSON file.</span></span> <span data-ttu-id="87c12-262">@No__t_0 陣列中定義了兩個物件常值，會反映兩個 Serilog*接收*，其中描述記錄輸出的目的地：</span><span class="sxs-lookup"><span data-stu-id="87c12-262">There are two object literals defined in the `WriteTo` array that reflect two Serilog *sinks*, which describe destinations for logging output:</span></span>

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

<span data-ttu-id="87c12-263">先前 JSON 檔案中顯示的設定會使用雙虛線（`--`）標記法和數值區段儲存在 Azure Key Vault 中：</span><span class="sxs-lookup"><span data-stu-id="87c12-263">The configuration shown in the preceding JSON file is stored in Azure Key Vault using double dash (`--`) notation and numeric segments:</span></span>

| <span data-ttu-id="87c12-264">機碼</span><span class="sxs-lookup"><span data-stu-id="87c12-264">Key</span></span> | <span data-ttu-id="87c12-265">值</span><span class="sxs-lookup"><span data-stu-id="87c12-265">Value</span></span> |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="reload-secrets"></a><span data-ttu-id="87c12-266">重載秘密</span><span class="sxs-lookup"><span data-stu-id="87c12-266">Reload secrets</span></span>

<span data-ttu-id="87c12-267">系統會快取密碼，直到呼叫 `IConfigurationRoot.Reload()` 為止。</span><span class="sxs-lookup"><span data-stu-id="87c12-267">Secrets are cached until `IConfigurationRoot.Reload()` is called.</span></span> <span data-ttu-id="87c12-268">在執行 `Reload` 之前，應用程式不會遵守金鑰保存庫中已過期、已停用及更新的秘密。</span><span class="sxs-lookup"><span data-stu-id="87c12-268">Expired, disabled, and updated secrets in the key vault are not respected by the app until `Reload` is executed.</span></span>

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a><span data-ttu-id="87c12-269">已停用和過期的秘密</span><span class="sxs-lookup"><span data-stu-id="87c12-269">Disabled and expired secrets</span></span>

<span data-ttu-id="87c12-270">已停用和過期的秘密會在執行時間擲回 `KeyVaultClientException`。</span><span class="sxs-lookup"><span data-stu-id="87c12-270">Disabled and expired secrets throw a `KeyVaultClientException` at runtime.</span></span> <span data-ttu-id="87c12-271">若要防止應用程式擲回，請使用不同的設定提供者來提供設定，或更新已停用或已過期的密碼。</span><span class="sxs-lookup"><span data-stu-id="87c12-271">To prevent the app from throwing, provide the configuration using a different configuration provider or update the disabled or expired secret.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="87c12-272">疑難排解</span><span class="sxs-lookup"><span data-stu-id="87c12-272">Troubleshoot</span></span>

<span data-ttu-id="87c12-273">當應用程式無法使用提供者載入設定時，會將錯誤訊息寫入[ASP.NET Core 記錄基礎結構](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="87c12-273">When the app fails to load configuration using the provider, an error message is written to the [ASP.NET Core Logging infrastructure](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="87c12-274">下列條件將導致無法載入設定：</span><span class="sxs-lookup"><span data-stu-id="87c12-274">The following conditions will prevent configuration from loading:</span></span>

* <span data-ttu-id="87c12-275">應用程式或憑證未在 Azure Active Directory 中正確設定。</span><span class="sxs-lookup"><span data-stu-id="87c12-275">The app or certificate isn't configured correctly in Azure Active Directory.</span></span>
* <span data-ttu-id="87c12-276">金鑰保存庫不存在於 Azure Key Vault 中。</span><span class="sxs-lookup"><span data-stu-id="87c12-276">The key vault doesn't exist in Azure Key Vault.</span></span>
* <span data-ttu-id="87c12-277">應用程式未獲授權，無法存取金鑰保存庫。</span><span class="sxs-lookup"><span data-stu-id="87c12-277">The app isn't authorized to access the key vault.</span></span>
* <span data-ttu-id="87c12-278">存取原則不包含 `Get` 和 `List` 許可權。</span><span class="sxs-lookup"><span data-stu-id="87c12-278">The access policy doesn't include `Get` and `List` permissions.</span></span>
* <span data-ttu-id="87c12-279">在金鑰保存庫中，設定資料（名稱/值組）未正確命名、遺失、停用或過期。</span><span class="sxs-lookup"><span data-stu-id="87c12-279">In the key vault, the configuration data (name-value pair) is incorrectly named, missing, disabled, or expired.</span></span>
* <span data-ttu-id="87c12-280">應用程式具有錯誤的金鑰保存庫名稱（`KeyVaultName`）、Azure AD 應用程式識別碼（`AzureADApplicationId`），或 Azure AD 憑證指紋（`AzureADCertThumbprint`）。</span><span class="sxs-lookup"><span data-stu-id="87c12-280">The app has the wrong key vault name (`KeyVaultName`), Azure AD Application Id (`AzureADApplicationId`), or Azure AD certificate thumbprint (`AzureADCertThumbprint`).</span></span>
* <span data-ttu-id="87c12-281">在應用程式中，設定金鑰（名稱）不正確，因為您嘗試載入的值。</span><span class="sxs-lookup"><span data-stu-id="87c12-281">The configuration key (name) is incorrect in the app for the value you're trying to load.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="87c12-282">其他資源</span><span class="sxs-lookup"><span data-stu-id="87c12-282">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* [<span data-ttu-id="87c12-283">Microsoft Azure： Key Vault</span><span class="sxs-lookup"><span data-stu-id="87c12-283">Microsoft Azure: Key Vault</span></span>](https://azure.microsoft.com/services/key-vault/)
* [<span data-ttu-id="87c12-284">Microsoft Azure： Key Vault 檔</span><span class="sxs-lookup"><span data-stu-id="87c12-284">Microsoft Azure: Key Vault Documentation</span></span>](/azure/key-vault/)
* [<span data-ttu-id="87c12-285">如何為 Azure Key Vault 產生及傳輸受 HSM 保護的金鑰</span><span class="sxs-lookup"><span data-stu-id="87c12-285">How to generate and transfer HSM-protected keys for Azure Key Vault</span></span>](/azure/key-vault/key-vault-hsm-protected-keys)
* [<span data-ttu-id="87c12-286">KeyVaultClient 類別</span><span class="sxs-lookup"><span data-stu-id="87c12-286">KeyVaultClient Class</span></span>](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
* [<span data-ttu-id="87c12-287">快速入門：使用 .NET web 應用程式從 Azure Key Vault 設定和取出秘密</span><span class="sxs-lookup"><span data-stu-id="87c12-287">Quickstart: Set and retrieve a secret from Azure Key Vault by using a .NET web app</span></span>](/azure/key-vault/quick-create-net)
* [<span data-ttu-id="87c12-288">教學課程：如何在 .NET 中搭配使用 Azure Key Vault 與 Azure Windows 虛擬機器</span><span class="sxs-lookup"><span data-stu-id="87c12-288">Tutorial: How to use Azure Key Vault with Azure Windows Virtual Machine in .NET</span></span>](/azure/key-vault/tutorial-net-windows-virtual-machine)
