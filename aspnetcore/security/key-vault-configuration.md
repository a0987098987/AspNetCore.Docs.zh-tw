---
title: ASP.NET Core 中的 Azure Key Vault 設定提供者
author: guardrex
description: 瞭解如何使用 Azure Key Vault 設定提供者，使用在執行時間載入的名稱/值配對來設定應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/16/2019
uid: security/key-vault-configuration
ms.openlocfilehash: cd31094884f53f34d55c0bceabae41ca2bacba4c
ms.sourcegitcommit: 0b0e485a8a6dfcc65a7a58b365622b3839f4d624
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/01/2020
ms.locfileid: "76928549"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a><span data-ttu-id="53841-103">ASP.NET Core 中的 Azure Key Vault 設定提供者</span><span class="sxs-lookup"><span data-stu-id="53841-103">Azure Key Vault Configuration Provider in ASP.NET Core</span></span>

<span data-ttu-id="53841-104">By [Luke Latham](https://github.com/guardrex)和[Andrew Stanton-護士](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="53841-104">By [Luke Latham](https://github.com/guardrex) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="53841-105">本檔說明如何使用[Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/)設定提供者，從 Azure Key Vault 秘密載入應用程式設定值。</span><span class="sxs-lookup"><span data-stu-id="53841-105">This document explains how to use the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) Configuration Provider to load app configuration values from Azure Key Vault secrets.</span></span> <span data-ttu-id="53841-106">Azure Key Vault 是一種雲端式服務，可協助保護應用程式和服務所使用的密碼編譯金鑰和密碼。</span><span class="sxs-lookup"><span data-stu-id="53841-106">Azure Key Vault is a cloud-based service that assists in safeguarding cryptographic keys and secrets used by apps and services.</span></span> <span data-ttu-id="53841-107">搭配 ASP.NET Core 應用程式使用 Azure Key Vault 的常見案例包括：</span><span class="sxs-lookup"><span data-stu-id="53841-107">Common scenarios for using Azure Key Vault with ASP.NET Core apps include:</span></span>

* <span data-ttu-id="53841-108">控制敏感性設定資料的存取權。</span><span class="sxs-lookup"><span data-stu-id="53841-108">Controlling access to sensitive configuration data.</span></span>
* <span data-ttu-id="53841-109">當儲存設定資料時，符合 FIPS 140-2 Level 2 驗證的硬體安全性模組（HSM）的需求。</span><span class="sxs-lookup"><span data-stu-id="53841-109">Meeting the requirement for FIPS 140-2 Level 2 validated Hardware Security Modules (HSM's) when storing configuration data.</span></span>

<span data-ttu-id="53841-110">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="53841-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="packages"></a><span data-ttu-id="53841-111">package</span><span class="sxs-lookup"><span data-stu-id="53841-111">Packages</span></span>

<span data-ttu-id="53841-112">將套件參考新增至[AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/)套件。</span><span class="sxs-lookup"><span data-stu-id="53841-112">Add a package reference to the [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) package.</span></span>

## <a name="sample-app"></a><span data-ttu-id="53841-113">範例應用程式</span><span class="sxs-lookup"><span data-stu-id="53841-113">Sample app</span></span>

<span data-ttu-id="53841-114">範例應用程式會以*Program.cs*檔案頂端的 `#define` 語句所決定的兩種模式之一來執行：</span><span class="sxs-lookup"><span data-stu-id="53841-114">The sample app runs in either of two modes determined by the `#define` statement at the top of the *Program.cs* file:</span></span>

* <span data-ttu-id="53841-115">`Certificate` &ndash; 示範如何使用 Azure Key Vault 用戶端識別碼和 x.509 憑證來存取儲存在 Azure Key Vault 中的秘密。</span><span class="sxs-lookup"><span data-stu-id="53841-115">`Certificate` &ndash; Demonstrates the use of an Azure Key Vault Client ID and X.509 certificate to access secrets stored in Azure Key Vault.</span></span> <span data-ttu-id="53841-116">這個版本的範例可以從任何位置執行，部署至 Azure App Service 或任何能夠提供 ASP.NET Core 應用程式的主機。</span><span class="sxs-lookup"><span data-stu-id="53841-116">This version of the sample can be run from any location, deployed to Azure App Service or any host capable of serving an ASP.NET Core app.</span></span>
* <span data-ttu-id="53841-117">`Managed` &ndash; 示範如何使用[適用于 Azure 資源的受控](/azure/active-directory/managed-identities-azure-resources/overview)識別來驗證應用程式，以使用 Azure AD 驗證來進行 Azure Key Vault，而不需要在應用程式的程式碼或設定中儲存認證。</span><span class="sxs-lookup"><span data-stu-id="53841-117">`Managed` &ndash; Demonstrates how to use [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview) to authenticate the app to Azure Key Vault with Azure AD authentication without credentials stored in the app's code or configuration.</span></span> <span data-ttu-id="53841-118">使用受控識別進行驗證時，不需要 Azure AD 應用程式識別碼和密碼（用戶端密碼）。</span><span class="sxs-lookup"><span data-stu-id="53841-118">When using managed identities to authenticate, an Azure AD Application ID and Password (Client Secret) aren't required.</span></span> <span data-ttu-id="53841-119">您必須將範例的 `Managed` 版本部署至 Azure。</span><span class="sxs-lookup"><span data-stu-id="53841-119">The `Managed` version of the sample must be deployed to Azure.</span></span> <span data-ttu-id="53841-120">請遵循[使用適用于 Azure 資源的受控](#use-managed-identities-for-azure-resources)識別一節中的指導方針。</span><span class="sxs-lookup"><span data-stu-id="53841-120">Follow the guidance in the [Use the Managed identities for Azure resources](#use-managed-identities-for-azure-resources) section.</span></span>

<span data-ttu-id="53841-121">如需有關如何使用預處理器指示詞（`#define`）來設定範例應用程式的詳細資訊，請參閱 <xref:index#preprocessor-directives-in-sample-code>。</span><span class="sxs-lookup"><span data-stu-id="53841-121">For more information on how to configure a sample app using preprocessor directives (`#define`), see <xref:index#preprocessor-directives-in-sample-code>.</span></span>

## <a name="secret-storage-in-the-development-environment"></a><span data-ttu-id="53841-122">開發環境中的秘密儲存</span><span class="sxs-lookup"><span data-stu-id="53841-122">Secret storage in the Development environment</span></span>

<span data-ttu-id="53841-123">使用[秘密管理員工具](xref:security/app-secrets)在本機設定秘密。</span><span class="sxs-lookup"><span data-stu-id="53841-123">Set secrets locally using the [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="53841-124">當範例應用程式在開發環境中的本機電腦上執行時，會從本機密碼管理員存放區載入秘密。</span><span class="sxs-lookup"><span data-stu-id="53841-124">When the sample app runs on the local machine in the Development environment, secrets are loaded from the local Secret Manager store.</span></span>

<span data-ttu-id="53841-125">「密碼管理員」工具需要應用程式專案檔中的 `<UserSecretsId>` 屬性。</span><span class="sxs-lookup"><span data-stu-id="53841-125">The Secret Manager tool requires a `<UserSecretsId>` property in the app's project file.</span></span> <span data-ttu-id="53841-126">將屬性值（`{GUID}`）設定為任何唯一的 GUID：</span><span class="sxs-lookup"><span data-stu-id="53841-126">Set the property value (`{GUID}`) to any unique GUID:</span></span>

```xml
<PropertyGroup>
  <UserSecretsId>{GUID}</UserSecretsId>
</PropertyGroup>
```

<span data-ttu-id="53841-127">密碼會以名稱/值組的形式建立。</span><span class="sxs-lookup"><span data-stu-id="53841-127">Secrets are created as name-value pairs.</span></span> <span data-ttu-id="53841-128">階層式值（設定區段）使用 `:` （冒號）做為[ASP.NET Core](xref:fundamentals/configuration/index)設定機碼名稱中的分隔符號。</span><span class="sxs-lookup"><span data-stu-id="53841-128">Hierarchical values (configuration sections) use a `:` (colon) as a separator in [ASP.NET Core configuration](xref:fundamentals/configuration/index) key names.</span></span>

<span data-ttu-id="53841-129">秘密管理員會從開啟的命令 shell 使用到專案的[內容根目錄](xref:fundamentals/index#content-root)，其中 `{SECRET NAME}` 是名稱，而 `{SECRET VALUE}` 是值：</span><span class="sxs-lookup"><span data-stu-id="53841-129">The Secret Manager is used from a command shell opened to the project's [content root](xref:fundamentals/index#content-root), where `{SECRET NAME}` is the name and `{SECRET VALUE}` is the value:</span></span>

```dotnetcli
dotnet user-secrets set "{SECRET NAME}" "{SECRET VALUE}"
```

<span data-ttu-id="53841-130">在命令 shell 中，從專案的[內容根目錄](xref:fundamentals/index#content-root)執行下列命令，以設定範例應用程式的秘密：</span><span class="sxs-lookup"><span data-stu-id="53841-130">Execute the following commands in a command shell from the project's [content root](xref:fundamentals/index#content-root) to set the secrets for the sample app:</span></span>

```dotnetcli
dotnet user-secrets set "SecretName" "secret_value_1_dev"
dotnet user-secrets set "Section:SecretName" "secret_value_2_dev"
```

<span data-ttu-id="53841-131">當這些秘密儲存在具有 Azure Key Vault 區段之[生產環境的秘密儲存](#secret-storage-in-the-production-environment-with-azure-key-vault)Azure Key Vault 中時，`_dev` 的尾碼會變更為 `_prod`。</span><span class="sxs-lookup"><span data-stu-id="53841-131">When these secrets are stored in Azure Key Vault in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section, the `_dev` suffix is changed to `_prod`.</span></span> <span data-ttu-id="53841-132">尾碼會在應用程式的輸出中提供視覺提示，指出設定值的來源。</span><span class="sxs-lookup"><span data-stu-id="53841-132">The suffix provides a visual cue in the app's output indicating the source of the configuration values.</span></span>

## <a name="secret-storage-in-the-production-environment-with-azure-key-vault"></a><span data-ttu-id="53841-133">在生產環境中使用 Azure Key Vault 的秘密儲存</span><span class="sxs-lookup"><span data-stu-id="53841-133">Secret storage in the Production environment with Azure Key Vault</span></span>

<span data-ttu-id="53841-134">以下摘要說明[快速入門：從 Azure Key Vault 使用 Azure CLI 主題設定和取出秘密](/azure/key-vault/quick-create-cli)，以建立 Azure Key Vault 並儲存範例應用程式所使用的秘密。</span><span class="sxs-lookup"><span data-stu-id="53841-134">The instructions provided by the [Quickstart: Set and retrieve a secret from Azure Key Vault using Azure CLI](/azure/key-vault/quick-create-cli) topic are summarized here for creating an Azure Key Vault and storing secrets used by the sample app.</span></span> <span data-ttu-id="53841-135">如需進一步的詳細資料，請參閱主題。</span><span class="sxs-lookup"><span data-stu-id="53841-135">Refer to the topic for further details.</span></span>

1. <span data-ttu-id="53841-136">使用下列其中一種方法，在[Azure 入口網站](https://portal.azure.com/)中開啟 Azure Cloud shell：</span><span class="sxs-lookup"><span data-stu-id="53841-136">Open Azure Cloud shell using any one of the following methods in the [Azure portal](https://portal.azure.com/):</span></span>

   * <span data-ttu-id="53841-137">選取程式碼區塊右上角的 [**試試看**]。</span><span class="sxs-lookup"><span data-stu-id="53841-137">Select **Try It** in the upper-right corner of a code block.</span></span> <span data-ttu-id="53841-138">在文字方塊中使用搜尋字串 "Azure CLI"。</span><span class="sxs-lookup"><span data-stu-id="53841-138">Use the search string "Azure CLI" in the text box.</span></span>
   * <span data-ttu-id="53841-139">使用 [**啟動 Cloud Shell** ] 按鈕，在瀏覽器中開啟 Cloud Shell。</span><span class="sxs-lookup"><span data-stu-id="53841-139">Open Cloud Shell in your browser with the **Launch Cloud Shell** button.</span></span>
   * <span data-ttu-id="53841-140">在 Azure 入口網站右上角的功能表上，選取 [ **Cloud Shell** ] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="53841-140">Select the **Cloud Shell** button on the menu in the upper-right corner of the Azure portal.</span></span>

   <span data-ttu-id="53841-141">如需詳細資訊，請參閱[Azure Cloud Shell 的](/azure/cloud-shell/overview) [Azure CLI](/cli/azure/)和總覽。</span><span class="sxs-lookup"><span data-stu-id="53841-141">For more information, see [Azure CLI](/cli/azure/) and [Overview of Azure Cloud Shell](/azure/cloud-shell/overview).</span></span>

1. <span data-ttu-id="53841-142">如果尚未驗證，請使用 `az login` 命令登入。</span><span class="sxs-lookup"><span data-stu-id="53841-142">If you aren't already authenticated, sign in with the `az login` command.</span></span>

1. <span data-ttu-id="53841-143">使用下列命令建立資源群組，其中 `{RESOURCE GROUP NAME}` 是新資源群組的資源組名，而 `{LOCATION}` 是 Azure 區域（datacenter）：</span><span class="sxs-lookup"><span data-stu-id="53841-143">Create a resource group with the following command, where `{RESOURCE GROUP NAME}` is the resource group name for the new resource group and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```azure-cli
   az group create --name "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="53841-144">使用下列命令，在資源群組中建立金鑰保存庫，其中 `{KEY VAULT NAME}` 是新金鑰保存庫的名稱，而 `{LOCATION}` 是 Azure 區域（datacenter）：</span><span class="sxs-lookup"><span data-stu-id="53841-144">Create a key vault in the resource group with the following command, where `{KEY VAULT NAME}` is the name for the new key vault and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```azure-cli
   az keyvault create --name {KEY VAULT NAME} --resource-group "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="53841-145">在金鑰保存庫中建立秘密，做為名稱/值配對。</span><span class="sxs-lookup"><span data-stu-id="53841-145">Create secrets in the key vault as name-value pairs.</span></span>

   <span data-ttu-id="53841-146">Azure Key Vault 秘密名稱僅限於英數位元和連字號。</span><span class="sxs-lookup"><span data-stu-id="53841-146">Azure Key Vault secret names are limited to alphanumeric characters and dashes.</span></span> <span data-ttu-id="53841-147">階層式值（設定區段）使用 `--` （兩個虛線）做為分隔符號。</span><span class="sxs-lookup"><span data-stu-id="53841-147">Hierarchical values (configuration sections) use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="53841-148">冒號，通常用來從[ASP.NET Core](xref:fundamentals/configuration/index)設定中的子機碼分隔區段，但不允許用在金鑰保存庫密碼名稱中。</span><span class="sxs-lookup"><span data-stu-id="53841-148">Colons, which are normally used to delimit a section from a subkey in [ASP.NET Core configuration](xref:fundamentals/configuration/index), aren't allowed in key vault secret names.</span></span> <span data-ttu-id="53841-149">因此，當密碼載入應用程式的設定時，會使用兩個破折號並交換冒號。</span><span class="sxs-lookup"><span data-stu-id="53841-149">Therefore, two dashes are used and swapped for a colon when the secrets are loaded into the app's configuration.</span></span>

   <span data-ttu-id="53841-150">下列秘密可用於範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="53841-150">The following secrets are for use with the sample app.</span></span> <span data-ttu-id="53841-151">這些值包含 `_prod` 後置詞，以與在開發環境中從使用者秘密載入的 `_dev` 尾碼值進行區別。</span><span class="sxs-lookup"><span data-stu-id="53841-151">The values include a `_prod` suffix to distinguish them from the `_dev` suffix values loaded in the Development environment from User Secrets.</span></span> <span data-ttu-id="53841-152">以您在上一個步驟中建立的金鑰保存庫名稱取代 `{KEY VAULT NAME}`：</span><span class="sxs-lookup"><span data-stu-id="53841-152">Replace `{KEY VAULT NAME}` with the name of the key vault that you created in the prior step:</span></span>

   ```azure-cli
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "SecretName" --value "secret_value_1_prod"
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "Section--SecretName" --value "secret_value_2_prod"
   ```

## <a name="use-application-id-and-x509-certificate-for-non-azure-hosted-apps"></a><span data-ttu-id="53841-153">將應用程式識別碼和 x.509 憑證用於非 Azure 託管的應用程式</span><span class="sxs-lookup"><span data-stu-id="53841-153">Use Application ID and X.509 certificate for non-Azure-hosted apps</span></span>

<span data-ttu-id="53841-154">設定 Azure AD、Azure Key Vault 和應用程式，以在應用程式裝載于**Azure 外部時**，使用 Azure Active Directory 應用程式識別碼和 x.509 憑證來驗證金鑰保存庫。</span><span class="sxs-lookup"><span data-stu-id="53841-154">Configure Azure AD, Azure Key Vault, and the app to use an Azure Active Directory Application ID and X.509 certificate to authenticate to a key vault **when the app is hosted outside of Azure**.</span></span> <span data-ttu-id="53841-155">如需詳細資訊，請參閱[關於金鑰、秘密和憑證](/azure/key-vault/about-keys-secrets-and-certificates)。</span><span class="sxs-lookup"><span data-stu-id="53841-155">For more information, see [About keys, secrets, and certificates](/azure/key-vault/about-keys-secrets-and-certificates).</span></span>

> [!NOTE]
> <span data-ttu-id="53841-156">雖然 Azure 中裝載的應用程式支援使用應用程式識別碼和 x.509 憑證，但建議您在 Azure 中裝載應用程式時，使用[適用于 azure 資源的受控](#use-managed-identities-for-azure-resources)識別。</span><span class="sxs-lookup"><span data-stu-id="53841-156">Although using an Application ID and X.509 certificate is supported for apps hosted in Azure, we recommend using [Managed identities for Azure resources](#use-managed-identities-for-azure-resources) when hosting an app in Azure.</span></span> <span data-ttu-id="53841-157">受控識別不需要在應用程式中或在開發環境中儲存憑證。</span><span class="sxs-lookup"><span data-stu-id="53841-157">Managed identities don't require storing a certificate in the app or in the development environment.</span></span>

<span data-ttu-id="53841-158">當*Program.cs*檔案頂端的 `#define` 語句設定為 `Certificate`時，範例應用程式會使用應用程式識別碼和 x.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="53841-158">The sample app uses an Application ID and X.509 certificate when the `#define` statement at the top of the *Program.cs* file is set to `Certificate`.</span></span>

1. <span data-ttu-id="53841-159">建立 PKCS # 12 封存檔案（ *.pfx*）憑證。</span><span class="sxs-lookup"><span data-stu-id="53841-159">Create a PKCS#12 archive (*.pfx*) certificate.</span></span> <span data-ttu-id="53841-160">建立憑證的選項包括[Windows](/windows/desktop/seccrypto/makecert)和[OpenSSL](https://www.openssl.org/)上的 MakeCert。</span><span class="sxs-lookup"><span data-stu-id="53841-160">Options for creating certificates include [MakeCert on Windows](/windows/desktop/seccrypto/makecert) and [OpenSSL](https://www.openssl.org/).</span></span>
1. <span data-ttu-id="53841-161">將憑證安裝到目前使用者的個人憑證存儲。</span><span class="sxs-lookup"><span data-stu-id="53841-161">Install the certificate into the current user's personal certificate store.</span></span> <span data-ttu-id="53841-162">將金鑰標示為可匯出是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="53841-162">Marking the key as exportable is optional.</span></span> <span data-ttu-id="53841-163">記下憑證的指紋，這會在此程式稍後使用。</span><span class="sxs-lookup"><span data-stu-id="53841-163">Note the certificate's thumbprint, which is used later in this process.</span></span>
1. <span data-ttu-id="53841-164">將 PKCS # 12 封存（ *.pfx*）憑證匯出為 DER 編碼憑證（ *.cer*）。</span><span class="sxs-lookup"><span data-stu-id="53841-164">Export the PKCS#12 archive (*.pfx*) certificate as a DER-encoded certificate (*.cer*).</span></span>
1. <span data-ttu-id="53841-165">使用 Azure AD （**應用程式註冊**）註冊應用程式。</span><span class="sxs-lookup"><span data-stu-id="53841-165">Register the app with Azure AD (**App registrations**).</span></span>
1. <span data-ttu-id="53841-166">將 DER 編碼的憑證（ *.cer*）上傳至 Azure AD：</span><span class="sxs-lookup"><span data-stu-id="53841-166">Upload the DER-encoded certificate (*.cer*) to Azure AD:</span></span>
   1. <span data-ttu-id="53841-167">在 Azure AD 中選取應用程式。</span><span class="sxs-lookup"><span data-stu-id="53841-167">Select the app in Azure AD.</span></span>
   1. <span data-ttu-id="53841-168">流覽至 [**憑證 & 密碼**]。</span><span class="sxs-lookup"><span data-stu-id="53841-168">Navigate to **Certificates & secrets**.</span></span>
   1. <span data-ttu-id="53841-169">選取 [**上傳憑證**] 來上傳包含公開金鑰的憑證。</span><span class="sxs-lookup"><span data-stu-id="53841-169">Select **Upload certificate** to upload the certificate, which contains the public key.</span></span> <span data-ttu-id="53841-170">*.Cer*、 *pem*或 *.crt*憑證是可接受的。</span><span class="sxs-lookup"><span data-stu-id="53841-170">A *.cer*, *.pem*, or *.crt* certificate is acceptable.</span></span>
1. <span data-ttu-id="53841-171">將金鑰保存庫名稱、應用程式識別碼和憑證指紋儲存在應用程式的*appsettings*中。</span><span class="sxs-lookup"><span data-stu-id="53841-171">Store the key vault name, Application ID, and certificate thumbprint in the app's *appsettings.json* file.</span></span>
1. <span data-ttu-id="53841-172">流覽至 Azure 入口網站中的 [**金鑰保存庫**]。</span><span class="sxs-lookup"><span data-stu-id="53841-172">Navigate to **Key vaults** in the Azure portal.</span></span>
1. <span data-ttu-id="53841-173">選取您在[生產環境中使用 Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault)一節所建立的金鑰保存庫。</span><span class="sxs-lookup"><span data-stu-id="53841-173">Select the key vault that you created in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section.</span></span>
1. <span data-ttu-id="53841-174">選取 [**存取原則**]。</span><span class="sxs-lookup"><span data-stu-id="53841-174">Select **Access policies**.</span></span>
1. <span data-ttu-id="53841-175">選取 [**新增存取原則**]。</span><span class="sxs-lookup"><span data-stu-id="53841-175">Select **Add Access Policy**.</span></span>
1. <span data-ttu-id="53841-176">開啟 [**秘密許可權**]，並提供具有 [**取得**] 和 [**列出**] 許可權的應用程式。</span><span class="sxs-lookup"><span data-stu-id="53841-176">Open **Secret permissions** and provide the app with **Get** and **List** permissions.</span></span>
1. <span data-ttu-id="53841-177">選取 [**選取主體**]，然後依名稱選取已註冊的應用程式。</span><span class="sxs-lookup"><span data-stu-id="53841-177">Select **Select principal** and select the registered app by name.</span></span> <span data-ttu-id="53841-178">選取 [**選取**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="53841-178">Select the **Select** button.</span></span>
1. <span data-ttu-id="53841-179">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="53841-179">Select **OK**.</span></span>
1. <span data-ttu-id="53841-180">選取 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="53841-180">Select **Save**.</span></span>
1. <span data-ttu-id="53841-181">部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="53841-181">Deploy the app.</span></span>

<span data-ttu-id="53841-182">`Certificate` 範例應用程式會從 `IConfigurationRoot` 與秘密名稱相同的名稱取得其設定值：</span><span class="sxs-lookup"><span data-stu-id="53841-182">The `Certificate` sample app obtains its configuration values from `IConfigurationRoot` with the same name as the secret name:</span></span>

* <span data-ttu-id="53841-183">非階層式值： `SecretName` 的值是以 `config["SecretName"]`取得。</span><span class="sxs-lookup"><span data-stu-id="53841-183">Non-hierarchical values: The value for `SecretName` is obtained with `config["SecretName"]`.</span></span>
* <span data-ttu-id="53841-184">階層式值（區段）：使用 `:` （冒號）標記法或 `GetSection` 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="53841-184">Hierarchical values (sections): Use `:` (colon) notation or the `GetSection` extension method.</span></span> <span data-ttu-id="53841-185">使用下列其中一種方法來取得設定值：</span><span class="sxs-lookup"><span data-stu-id="53841-185">Use either of these approaches to obtain the configuration value:</span></span>
  * `config["Section:SecretName"]`
  * `config.GetSection("Section")["SecretName"]`

<span data-ttu-id="53841-186">X.509 憑證是由作業系統所管理。</span><span class="sxs-lookup"><span data-stu-id="53841-186">The X.509 certificate is managed by the OS.</span></span> <span data-ttu-id="53841-187">應用程式會使用*appsettings*所提供的值來呼叫 <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>：</span><span class="sxs-lookup"><span data-stu-id="53841-187">The app calls <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> with values supplied by the *appsettings.json* file:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](key-vault-configuration/samples/3.x/SampleApp/Program.cs?name=snippet1&highlight=20-23)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](key-vault-configuration/samples/2.x/SampleApp/Program.cs?name=snippet1&highlight=20-23)]

::: moniker-end

<span data-ttu-id="53841-188">範例值：</span><span class="sxs-lookup"><span data-stu-id="53841-188">Example values:</span></span>

* <span data-ttu-id="53841-189">金鑰保存庫名稱： `contosovault`</span><span class="sxs-lookup"><span data-stu-id="53841-189">Key vault name: `contosovault`</span></span>
* <span data-ttu-id="53841-190">應用程式識別碼： `627e911e-43cc-61d4-992e-12db9c81b413`</span><span class="sxs-lookup"><span data-stu-id="53841-190">Application ID: `627e911e-43cc-61d4-992e-12db9c81b413`</span></span>
* <span data-ttu-id="53841-191">憑證指紋： `fe14593dd66b2406c5269d742d04b6e1ab03adb1`</span><span class="sxs-lookup"><span data-stu-id="53841-191">Certificate thumbprint: `fe14593dd66b2406c5269d742d04b6e1ab03adb1`</span></span>

<span data-ttu-id="53841-192">*appsettings.json*：</span><span class="sxs-lookup"><span data-stu-id="53841-192">*appsettings.json*:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](key-vault-configuration/samples/3.x/SampleApp/appsettings.json?highlight=10-12)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](key-vault-configuration/samples/2.x/SampleApp/appsettings.json?highlight=10-12)]

::: moniker-end

<span data-ttu-id="53841-193">當您執行應用程式時，網頁會顯示已載入的密碼值。</span><span class="sxs-lookup"><span data-stu-id="53841-193">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="53841-194">在開發環境中，會以 `_dev` 尾碼來載入密碼值。</span><span class="sxs-lookup"><span data-stu-id="53841-194">In the Development environment, secret values load with the `_dev` suffix.</span></span> <span data-ttu-id="53841-195">在生產環境中，值會以 `_prod` 尾碼來載入。</span><span class="sxs-lookup"><span data-stu-id="53841-195">In the Production environment, the values load with the `_prod` suffix.</span></span>

## <a name="use-managed-identities-for-azure-resources"></a><span data-ttu-id="53841-196">使用適用于 Azure 資源的受控識別</span><span class="sxs-lookup"><span data-stu-id="53841-196">Use Managed identities for Azure resources</span></span>

<span data-ttu-id="53841-197">**部署至 azure 的應用程式**可以利用[Azure 資源的受控](/azure/active-directory/managed-identities-azure-resources/overview)識別，讓應用程式使用 Azure AD 驗證，而不需要在應用程式中儲存認證（應用程式識別碼和密碼/用戶端密碼）來驗證 Azure Key Vault。</span><span class="sxs-lookup"><span data-stu-id="53841-197">**An app deployed to Azure** can take advantage of [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview), which allows the app to authenticate with Azure Key Vault using Azure AD authentication without credentials (Application ID and Password/Client Secret) stored in the app.</span></span>

<span data-ttu-id="53841-198">當*Program.cs*檔案頂端的 `#define` 語句設定為 `Managed`時，範例應用程式會使用適用于 Azure 資源的受控識別。</span><span class="sxs-lookup"><span data-stu-id="53841-198">The sample app uses Managed identities for Azure resources when the `#define` statement at the top of the *Program.cs* file is set to `Managed`.</span></span>

<span data-ttu-id="53841-199">在應用程式的*appsettings*中輸入保存庫名稱。</span><span class="sxs-lookup"><span data-stu-id="53841-199">Enter the vault name into the app's *appsettings.json* file.</span></span> <span data-ttu-id="53841-200">當設定為 `Managed` 版本時，範例應用程式不需要應用程式識別碼和密碼（用戶端秘密），因此您可以忽略這些設定專案。</span><span class="sxs-lookup"><span data-stu-id="53841-200">The sample app doesn't require an Application ID and Password (Client Secret) when set to the `Managed` version, so you can ignore those configuration entries.</span></span> <span data-ttu-id="53841-201">應用程式會部署至 Azure，而 Azure 只會使用儲存在*appsettings*中的保存庫名稱來驗證應用程式，以存取 Azure Key Vault。</span><span class="sxs-lookup"><span data-stu-id="53841-201">The app is deployed to Azure, and Azure authenticates the app to access Azure Key Vault only using the vault name stored in the *appsettings.json* file.</span></span>

<span data-ttu-id="53841-202">將範例應用程式部署至 Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="53841-202">Deploy the sample app to Azure App Service.</span></span>

<span data-ttu-id="53841-203">部署到 Azure App Service 的應用程式會在建立服務時，自動向 Azure AD 註冊。</span><span class="sxs-lookup"><span data-stu-id="53841-203">An app deployed to Azure App Service is automatically registered with Azure AD when the service is created.</span></span> <span data-ttu-id="53841-204">從部署取得物件識別碼，以便在下列命令中使用。</span><span class="sxs-lookup"><span data-stu-id="53841-204">Obtain the Object ID from the deployment for use in the following command.</span></span> <span data-ttu-id="53841-205">物件識別碼會顯示在 App Service 的 身分**識別** 面板上的 Azure 入口網站。</span><span class="sxs-lookup"><span data-stu-id="53841-205">The Object ID is shown in the Azure portal on the **Identity** panel of the App Service.</span></span>

<span data-ttu-id="53841-206">使用 Azure CLI 和應用程式的物件識別碼，為應用程式提供 `list` 和 `get` 許可權以存取金鑰保存庫：</span><span class="sxs-lookup"><span data-stu-id="53841-206">Using Azure CLI and the app's Object ID, provide the app with `list` and `get` permissions to access the key vault:</span></span>

```azure-cli
az keyvault set-policy --name {KEY VAULT NAME} --object-id {OBJECT ID} --secret-permissions get list
```

<span data-ttu-id="53841-207">使用 Azure CLI、PowerShell 或 Azure 入口網站**重新開機應用程式**。</span><span class="sxs-lookup"><span data-stu-id="53841-207">**Restart the app** using Azure CLI, PowerShell, or the Azure portal.</span></span>

<span data-ttu-id="53841-208">範例應用程式：</span><span class="sxs-lookup"><span data-stu-id="53841-208">The sample app:</span></span>

* <span data-ttu-id="53841-209">建立 `AzureServiceTokenProvider` 類別的實例，而不含連接字串。</span><span class="sxs-lookup"><span data-stu-id="53841-209">Creates an instance of the `AzureServiceTokenProvider` class without a connection string.</span></span> <span data-ttu-id="53841-210">未提供連接字串時，提供者會嘗試從 Azure 資源的受控識別取得存取權杖。</span><span class="sxs-lookup"><span data-stu-id="53841-210">When a connection string isn't provided, the provider attempts to obtain an access token from Managed identities for Azure resources.</span></span>
* <span data-ttu-id="53841-211">新的 <xref:Microsoft.Azure.KeyVault.KeyVaultClient> 會使用 `AzureServiceTokenProvider` 實例權杖回呼來建立。</span><span class="sxs-lookup"><span data-stu-id="53841-211">A new <xref:Microsoft.Azure.KeyVault.KeyVaultClient> is created with the `AzureServiceTokenProvider` instance token callback.</span></span>
* <span data-ttu-id="53841-212"><xref:Microsoft.Azure.KeyVault.KeyVaultClient> 實例會搭配使用 <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> 的預設執行，以載入所有秘密值，並以冒號（`:`）取代索引鍵名稱中的雙破折號（`--`）。</span><span class="sxs-lookup"><span data-stu-id="53841-212">The <xref:Microsoft.Azure.KeyVault.KeyVaultClient> instance is used with a default implementation of <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> that loads all secret values and replaces double-dashes (`--`) with colons (`:`) in key names.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](key-vault-configuration/samples/3.x/SampleApp/Program.cs?name=snippet2&highlight=13-21)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](key-vault-configuration/samples/2.x/SampleApp/Program.cs?name=snippet2&highlight=13-21)]

::: moniker-end

<span data-ttu-id="53841-213">金鑰保存庫名稱範例值： `contosovault`</span><span class="sxs-lookup"><span data-stu-id="53841-213">Key vault name example value: `contosovault`</span></span>
    
<span data-ttu-id="53841-214">*appsettings.json*：</span><span class="sxs-lookup"><span data-stu-id="53841-214">*appsettings.json*:</span></span>

```json
{
  "KeyVaultName": "Key Vault Name"
}
```

<span data-ttu-id="53841-215">當您執行應用程式時，網頁會顯示已載入的密碼值。</span><span class="sxs-lookup"><span data-stu-id="53841-215">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="53841-216">在開發環境中，秘密值具有 `_dev` 尾碼，因為它們是由使用者密碼提供。</span><span class="sxs-lookup"><span data-stu-id="53841-216">In the Development environment, secret values have the `_dev` suffix because they're provided by User Secrets.</span></span> <span data-ttu-id="53841-217">在生產環境中，值會以 `_prod` 尾碼載入，因為它們是由 Azure Key Vault 提供。</span><span class="sxs-lookup"><span data-stu-id="53841-217">In the Production environment, the values load with the `_prod` suffix because they're provided by Azure Key Vault.</span></span>

<span data-ttu-id="53841-218">如果您收到 `Access denied` 錯誤，請確認已向 Azure AD 註冊應用程式，並提供金鑰保存庫的存取權。</span><span class="sxs-lookup"><span data-stu-id="53841-218">If you receive an `Access denied` error, confirm that the app is registered with Azure AD and provided access to the key vault.</span></span> <span data-ttu-id="53841-219">確認您已在 Azure 中重新開機服務。</span><span class="sxs-lookup"><span data-stu-id="53841-219">Confirm that you've restarted the service in Azure.</span></span>

<span data-ttu-id="53841-220">如需將提供者與受控識別和 Azure DevOps 管線搭配使用的詳細資訊，請參閱使用[受控服務識別建立 VM 的 Azure Resource Manager 服務](/azure/devops/pipelines/library/connect-to-azure#create-an-azure-resource-manager-service-connection-to-a-vm-with-a-managed-service-identity)連線。</span><span class="sxs-lookup"><span data-stu-id="53841-220">For information on using the provider with a managed identity and an Azure DevOps pipeline, see [Create an Azure Resource Manager service connection to a VM with a managed service identity](/azure/devops/pipelines/library/connect-to-azure#create-an-azure-resource-manager-service-connection-to-a-vm-with-a-managed-service-identity).</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="configuration-options"></a><span data-ttu-id="53841-221">組態選項</span><span class="sxs-lookup"><span data-stu-id="53841-221">Configuration options</span></span>

<span data-ttu-id="53841-222"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> 可以接受 <xref:Microsoft.Extensions.Configuration.AzureKeyVault.AzureKeyVaultConfigurationOptions>：</span><span class="sxs-lookup"><span data-stu-id="53841-222"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> can accept an <xref:Microsoft.Extensions.Configuration.AzureKeyVault.AzureKeyVaultConfigurationOptions>:</span></span>

```csharp
config.AddAzureKeyVault(
    new AzureKeyVaultConfigurationOptions()
    {
        ...
    });
```

| <span data-ttu-id="53841-223">屬性</span><span class="sxs-lookup"><span data-stu-id="53841-223">Property</span></span>         | <span data-ttu-id="53841-224">描述</span><span class="sxs-lookup"><span data-stu-id="53841-224">Description</span></span> |
| ---------------- | ----------- |
| `Client`         | <span data-ttu-id="53841-225">用來抓取值的 <xref:Microsoft.Azure.KeyVault.KeyVaultClient>。</span><span class="sxs-lookup"><span data-stu-id="53841-225"><xref:Microsoft.Azure.KeyVault.KeyVaultClient> to use for retrieving values.</span></span> |
| `Manager`        | <span data-ttu-id="53841-226">用來控制密碼載入的 <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> 實例。</span><span class="sxs-lookup"><span data-stu-id="53841-226"><xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> instance used to control secret loading.</span></span> |
| `ReloadInterval` | <span data-ttu-id="53841-227">`Timespan` 在輪詢金鑰保存庫以進行變更的嘗試之間等待。</span><span class="sxs-lookup"><span data-stu-id="53841-227">`Timespan` to wait between attempts at polling the key vault for changes.</span></span> <span data-ttu-id="53841-228">預設值為 `null` （不重載設定）。</span><span class="sxs-lookup"><span data-stu-id="53841-228">The default value is `null` (configuration isn't reloaded).</span></span> |
| `Vault`          | <span data-ttu-id="53841-229">金鑰保存庫 URI。</span><span class="sxs-lookup"><span data-stu-id="53841-229">Key vault URI.</span></span> |

::: moniker-end

## <a name="use-a-key-name-prefix"></a><span data-ttu-id="53841-230">使用索引鍵名稱前置詞</span><span class="sxs-lookup"><span data-stu-id="53841-230">Use a key name prefix</span></span>

<span data-ttu-id="53841-231"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> 提供的多載可接受 <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>的執行，可讓您控制如何將金鑰保存庫密碼轉換成設定金鑰。</span><span class="sxs-lookup"><span data-stu-id="53841-231"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> provides an overload that accepts an implementation of <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>, which allows you to control how key vault secrets are converted into configuration keys.</span></span> <span data-ttu-id="53841-232">例如，您可以根據您在應用程式啟動時提供的首碼值，執行介面來載入密碼值。</span><span class="sxs-lookup"><span data-stu-id="53841-232">For example, you can implement the interface to load secret values based on a prefix value you provide at app startup.</span></span> <span data-ttu-id="53841-233">例如，這可讓您根據應用程式的版本來載入密碼。</span><span class="sxs-lookup"><span data-stu-id="53841-233">This allows you, for example, to load secrets based on the version of the app.</span></span>

> [!WARNING]
> <span data-ttu-id="53841-234">請勿在金鑰保存庫秘密上使用前置詞，將多個應用程式的秘密放入相同的金鑰保存庫，或將環境秘密（例如*開發*與*生產*密碼）放入相同的保存庫中。</span><span class="sxs-lookup"><span data-stu-id="53841-234">Don't use prefixes on key vault secrets to place secrets for multiple apps into the same key vault or to place environmental secrets (for example, *development* versus *production* secrets) into the same vault.</span></span> <span data-ttu-id="53841-235">我們建議不同的應用程式和開發/生產環境使用不同的金鑰保存庫，以隔離最高安全性層級的應用程式環境。</span><span class="sxs-lookup"><span data-stu-id="53841-235">We recommend that different apps and development/production environments use separate key vaults to isolate app environments for the highest level of security.</span></span>

<span data-ttu-id="53841-236">在下列範例中，會在金鑰保存庫中建立秘密（並使用適用于開發環境的密碼管理員工具）進行 `5000-AppSecret` （金鑰保存庫密碼名稱中不允許期間）。</span><span class="sxs-lookup"><span data-stu-id="53841-236">In the following example, a secret is established in the key vault (and using the Secret Manager tool for the Development environment) for `5000-AppSecret` (periods aren't allowed in key vault secret names).</span></span> <span data-ttu-id="53841-237">此秘密代表應用程式版本5.0.0.0 的應用程式密碼。</span><span class="sxs-lookup"><span data-stu-id="53841-237">This secret represents an app secret for version 5.0.0.0 of the app.</span></span> <span data-ttu-id="53841-238">針對其他版本的應用程式（5.1.0.0），會將密碼新增至金鑰保存庫（並使用秘密管理員工具）進行 `5100-AppSecret`。</span><span class="sxs-lookup"><span data-stu-id="53841-238">For another version of the app, 5.1.0.0, a secret is added to the key vault (and using the Secret Manager tool) for `5100-AppSecret`.</span></span> <span data-ttu-id="53841-239">每個應用程式版本都會將其版本設定的秘密值載入至其設定中做為 `AppSecret`，並在載入秘密時去除版本。</span><span class="sxs-lookup"><span data-stu-id="53841-239">Each app version loads its versioned secret value into its configuration as `AppSecret`, stripping off the version as it loads the secret.</span></span>

<span data-ttu-id="53841-240">使用自訂 <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>呼叫 <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>：</span><span class="sxs-lookup"><span data-stu-id="53841-240"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> is called with a custom <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>:</span></span>

[!code-csharp[](key-vault-configuration/samples_snapshot/Program.cs)]

<span data-ttu-id="53841-241"><xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> 的執行會回應密碼的版本前置詞，以將適當的密碼載入設定中：</span><span class="sxs-lookup"><span data-stu-id="53841-241">The <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> implementation reacts to the version prefixes of secrets to load the proper secret into configuration:</span></span>

* <span data-ttu-id="53841-242">`Load` 會在其名稱開頭為前置詞時載入密碼。</span><span class="sxs-lookup"><span data-stu-id="53841-242">`Load` loads a secret when its name starts with the prefix.</span></span> <span data-ttu-id="53841-243">其他秘密則不會載入。</span><span class="sxs-lookup"><span data-stu-id="53841-243">Other secrets aren't loaded.</span></span>
* <span data-ttu-id="53841-244">`GetKey`：</span><span class="sxs-lookup"><span data-stu-id="53841-244">`GetKey`:</span></span>
  * <span data-ttu-id="53841-245">移除秘密名稱中的前置詞。</span><span class="sxs-lookup"><span data-stu-id="53841-245">Removes the prefix from the secret name.</span></span>
  * <span data-ttu-id="53841-246">以 `KeyDelimiter`取代任何名稱中的兩個破折號，這是設定中使用的分隔符號（通常是冒號）。</span><span class="sxs-lookup"><span data-stu-id="53841-246">Replaces two dashes in any name with the `KeyDelimiter`, which is the delimiter used in configuration (usually a colon).</span></span> <span data-ttu-id="53841-247">Azure Key Vault 在密碼名稱中不允許冒號。</span><span class="sxs-lookup"><span data-stu-id="53841-247">Azure Key Vault doesn't allow a colon in secret names.</span></span>

[!code-csharp[](key-vault-configuration/samples_snapshot/Startup.cs)]

<span data-ttu-id="53841-248">`Load` 方法是由提供者演算法所呼叫，它會逐一查看保存庫秘密，以尋找具有版本前置詞的金鑰。</span><span class="sxs-lookup"><span data-stu-id="53841-248">The `Load` method is called by a provider algorithm that iterates through the vault secrets to find the ones that have the version prefix.</span></span> <span data-ttu-id="53841-249">當找到具有 `Load`的版本前置詞時，演算法會使用 `GetKey` 方法來傳回密碼名稱的設定名稱。</span><span class="sxs-lookup"><span data-stu-id="53841-249">When a version prefix is found with `Load`, the algorithm uses the `GetKey` method to return the configuration name of the secret name.</span></span> <span data-ttu-id="53841-250">它會從密碼的名稱中去除版本前置詞，並傳回其餘的秘密名稱，以載入至應用程式的設定名稱/值配對。</span><span class="sxs-lookup"><span data-stu-id="53841-250">It strips off the version prefix from the secret's name and returns the rest of the secret name for loading into the app's configuration name-value pairs.</span></span>

<span data-ttu-id="53841-251">當此方法執行時：</span><span class="sxs-lookup"><span data-stu-id="53841-251">When this approach is implemented:</span></span>

1. <span data-ttu-id="53841-252">應用程式的專案檔中指定的應用程式版本。</span><span class="sxs-lookup"><span data-stu-id="53841-252">The app's version specified in the app's project file.</span></span> <span data-ttu-id="53841-253">在下列範例中，應用程式的版本設定為 `5.0.0.0`：</span><span class="sxs-lookup"><span data-stu-id="53841-253">In the following example, the app's version is set to `5.0.0.0`:</span></span>

   ```xml
   <PropertyGroup>
     <Version>5.0.0.0</Version>
   </PropertyGroup>
   ```

1. <span data-ttu-id="53841-254">確認 `<UserSecretsId>` 屬性存在於應用程式的專案檔中，其中 `{GUID}` 是使用者提供的 GUID：</span><span class="sxs-lookup"><span data-stu-id="53841-254">Confirm that a `<UserSecretsId>` property is present in the app's project file, where `{GUID}` is a user-supplied GUID:</span></span>

   ```xml
   <PropertyGroup>
     <UserSecretsId>{GUID}</UserSecretsId>
   </PropertyGroup>
   ```

   <span data-ttu-id="53841-255">使用[秘密管理員工具](xref:security/app-secrets)在本機儲存下列秘密：</span><span class="sxs-lookup"><span data-stu-id="53841-255">Save the following secrets locally with the [Secret Manager tool](xref:security/app-secrets):</span></span>

   ```dotnetcli
   dotnet user-secrets set "5000-AppSecret" "5.0.0.0_secret_value_dev"
   dotnet user-secrets set "5100-AppSecret" "5.1.0.0_secret_value_dev"
   ```

1. <span data-ttu-id="53841-256">使用下列 Azure CLI 命令，將秘密儲存在 Azure Key Vault 中：</span><span class="sxs-lookup"><span data-stu-id="53841-256">Secrets are saved in Azure Key Vault using the following Azure CLI commands:</span></span>

   ```azure-cli
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "5000-AppSecret" --value "5.0.0.0_secret_value_prod"
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "5100-AppSecret" --value "5.1.0.0_secret_value_prod"
   ```

1. <span data-ttu-id="53841-257">當應用程式執行時，會載入金鑰保存庫密碼。</span><span class="sxs-lookup"><span data-stu-id="53841-257">When the app is run, the key vault secrets are loaded.</span></span> <span data-ttu-id="53841-258">`5000-AppSecret` 的字串秘密符合應用程式的專案檔（`5.0.0.0`）中所指定的應用程式版本。</span><span class="sxs-lookup"><span data-stu-id="53841-258">The string secret for `5000-AppSecret` is matched to the app's version specified in the app's project file (`5.0.0.0`).</span></span>

1. <span data-ttu-id="53841-259">會從索引鍵名稱中移除版本 `5000` （含破折號）。</span><span class="sxs-lookup"><span data-stu-id="53841-259">The version, `5000` (with the dash), is stripped from the key name.</span></span> <span data-ttu-id="53841-260">在整個應用程式中，使用金鑰來讀取設定 `AppSecret` 會載入秘密值。</span><span class="sxs-lookup"><span data-stu-id="53841-260">Throughout the app, reading configuration with the key `AppSecret` loads the secret value.</span></span>

1. <span data-ttu-id="53841-261">如果專案檔中的應用程式版本變更為 `5.1.0.0`，而應用程式再次執行，則傳回的秘密值會在開發環境中 `5.1.0.0_secret_value_dev`，並在生產環境中 `5.1.0.0_secret_value_prod`。</span><span class="sxs-lookup"><span data-stu-id="53841-261">If the app's version is changed in the project file to `5.1.0.0` and the app is run again, the secret value returned is `5.1.0.0_secret_value_dev` in the Development environment and `5.1.0.0_secret_value_prod` in Production.</span></span>

> [!NOTE]
> <span data-ttu-id="53841-262">您也可以提供您自己的 <xref:Microsoft.Azure.KeyVault.KeyVaultClient> 實作為 <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>。</span><span class="sxs-lookup"><span data-stu-id="53841-262">You can also provide your own <xref:Microsoft.Azure.KeyVault.KeyVaultClient> implementation to <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>.</span></span> <span data-ttu-id="53841-263">自訂用戶端允許跨應用程式共用單一用戶端實例。</span><span class="sxs-lookup"><span data-stu-id="53841-263">A custom client permits sharing a single instance of the client across the app.</span></span>

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="53841-264">將陣列繫結到類別</span><span class="sxs-lookup"><span data-stu-id="53841-264">Bind an array to a class</span></span>

<span data-ttu-id="53841-265">提供者能夠將設定值讀入陣列中，以系結至 POCO 陣列。</span><span class="sxs-lookup"><span data-stu-id="53841-265">The provider is capable of reading configuration values into an array for binding to a POCO array.</span></span>

<span data-ttu-id="53841-266">從允許金鑰包含冒號（`:`）分隔符號的設定來源讀取時，會使用數位索引鍵區段來區別組成陣列的索引鍵（`:0:`、`:1:`。</span><span class="sxs-lookup"><span data-stu-id="53841-266">When reading from a configuration source that allows keys to contain colon (`:`) separators, a numeric key segment is used to distinguish the keys that make up an array (`:0:`, `:1:`, …</span></span> <span data-ttu-id="53841-267">`:{n}:`)。</span><span class="sxs-lookup"><span data-stu-id="53841-267">`:{n}:`).</span></span> <span data-ttu-id="53841-268">如需詳細資訊，請參閱[Configuration：將陣列系結至類別](xref:fundamentals/configuration/index#bind-an-array-to-a-class)。</span><span class="sxs-lookup"><span data-stu-id="53841-268">For more information, see [Configuration: Bind an array to a class](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span></span>

<span data-ttu-id="53841-269">Azure Key Vault 索引鍵不能使用冒號做為分隔符號。</span><span class="sxs-lookup"><span data-stu-id="53841-269">Azure Key Vault keys can't use a colon as a separator.</span></span> <span data-ttu-id="53841-270">本主題中所述的方法會使用雙虛線（`--`）做為階層式值的分隔符號（區段）。</span><span class="sxs-lookup"><span data-stu-id="53841-270">The approach described in this topic uses double dashes (`--`) as a separator for hierarchical values (sections).</span></span> <span data-ttu-id="53841-271">陣列索引鍵會以雙虛線和數位索引鍵區段（`--0--`、`--1--`、&hellip; `--{n}--`）儲存在 Azure Key Vault 中。</span><span class="sxs-lookup"><span data-stu-id="53841-271">Array keys are stored in Azure Key Vault with double dashes and numeric key segments (`--0--`, `--1--`, &hellip; `--{n}--`).</span></span>

<span data-ttu-id="53841-272">檢查 JSON 檔案所提供的下列[Serilog](https://serilog.net/)記錄提供者設定。</span><span class="sxs-lookup"><span data-stu-id="53841-272">Examine the following [Serilog](https://serilog.net/) logging provider configuration provided by a JSON file.</span></span> <span data-ttu-id="53841-273">`WriteTo` 陣列中定義了兩個物件常值，會反映兩個 Serilog*接收*，其中描述記錄輸出的目的地：</span><span class="sxs-lookup"><span data-stu-id="53841-273">There are two object literals defined in the `WriteTo` array that reflect two Serilog *sinks*, which describe destinations for logging output:</span></span>

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

<span data-ttu-id="53841-274">先前 JSON 檔案中顯示的設定會使用雙虛線（`--`）標記法和數值區段儲存在 Azure Key Vault 中：</span><span class="sxs-lookup"><span data-stu-id="53841-274">The configuration shown in the preceding JSON file is stored in Azure Key Vault using double dash (`--`) notation and numeric segments:</span></span>

| <span data-ttu-id="53841-275">索引鍵</span><span class="sxs-lookup"><span data-stu-id="53841-275">Key</span></span> | <span data-ttu-id="53841-276">{2&gt;值&lt;2}</span><span class="sxs-lookup"><span data-stu-id="53841-276">Value</span></span> |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="reload-secrets"></a><span data-ttu-id="53841-277">重載秘密</span><span class="sxs-lookup"><span data-stu-id="53841-277">Reload secrets</span></span>

<span data-ttu-id="53841-278">系統會快取密碼，直到呼叫 `IConfigurationRoot.Reload()` 為止。</span><span class="sxs-lookup"><span data-stu-id="53841-278">Secrets are cached until `IConfigurationRoot.Reload()` is called.</span></span> <span data-ttu-id="53841-279">在執行 `Reload` 之前，應用程式不會遵守金鑰保存庫中已過期、已停用及更新的秘密。</span><span class="sxs-lookup"><span data-stu-id="53841-279">Expired, disabled, and updated secrets in the key vault are not respected by the app until `Reload` is executed.</span></span>

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a><span data-ttu-id="53841-280">已停用和過期的秘密</span><span class="sxs-lookup"><span data-stu-id="53841-280">Disabled and expired secrets</span></span>

<span data-ttu-id="53841-281">已停用和過期的秘密會擲回 <xref:Microsoft.Azure.KeyVault.Models.KeyVaultErrorException>。</span><span class="sxs-lookup"><span data-stu-id="53841-281">Disabled and expired secrets throw a <xref:Microsoft.Azure.KeyVault.Models.KeyVaultErrorException>.</span></span> <span data-ttu-id="53841-282">若要防止應用程式擲回，請使用不同的設定提供者來提供設定，或更新已停用或已過期的密碼。</span><span class="sxs-lookup"><span data-stu-id="53841-282">To prevent the app from throwing, provide the configuration using a different configuration provider or update the disabled or expired secret.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="53841-283">疑難排解</span><span class="sxs-lookup"><span data-stu-id="53841-283">Troubleshoot</span></span>

<span data-ttu-id="53841-284">當應用程式無法使用提供者載入設定時，會將錯誤訊息寫入[ASP.NET Core 記錄基礎結構](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="53841-284">When the app fails to load configuration using the provider, an error message is written to the [ASP.NET Core Logging infrastructure](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="53841-285">下列條件將導致無法載入設定：</span><span class="sxs-lookup"><span data-stu-id="53841-285">The following conditions will prevent configuration from loading:</span></span>

* <span data-ttu-id="53841-286">應用程式或憑證未在 Azure Active Directory 中正確設定。</span><span class="sxs-lookup"><span data-stu-id="53841-286">The app or certificate isn't configured correctly in Azure Active Directory.</span></span>
* <span data-ttu-id="53841-287">金鑰保存庫不存在於 Azure Key Vault 中。</span><span class="sxs-lookup"><span data-stu-id="53841-287">The key vault doesn't exist in Azure Key Vault.</span></span>
* <span data-ttu-id="53841-288">應用程式未獲授權，無法存取金鑰保存庫。</span><span class="sxs-lookup"><span data-stu-id="53841-288">The app isn't authorized to access the key vault.</span></span>
* <span data-ttu-id="53841-289">存取原則不包含 `Get` 和 `List` 許可權。</span><span class="sxs-lookup"><span data-stu-id="53841-289">The access policy doesn't include `Get` and `List` permissions.</span></span>
* <span data-ttu-id="53841-290">在金鑰保存庫中，設定資料（名稱/值組）未正確命名、遺失、停用或過期。</span><span class="sxs-lookup"><span data-stu-id="53841-290">In the key vault, the configuration data (name-value pair) is incorrectly named, missing, disabled, or expired.</span></span>
* <span data-ttu-id="53841-291">應用程式具有錯誤的金鑰保存庫名稱（`KeyVaultName`）、Azure AD 應用程式識別碼（`AzureADApplicationId`），或 Azure AD 憑證指紋（`AzureADCertThumbprint`）。</span><span class="sxs-lookup"><span data-stu-id="53841-291">The app has the wrong key vault name (`KeyVaultName`), Azure AD Application Id (`AzureADApplicationId`), or Azure AD certificate thumbprint (`AzureADCertThumbprint`).</span></span>
* <span data-ttu-id="53841-292">在應用程式中，設定金鑰（名稱）不正確，因為您嘗試載入的值。</span><span class="sxs-lookup"><span data-stu-id="53841-292">The configuration key (name) is incorrect in the app for the value you're trying to load.</span></span>
* <span data-ttu-id="53841-293">將應用程式的存取原則新增至金鑰保存庫時，已建立原則，但未在 [**存取原則**] UI 中選取 [**儲存**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="53841-293">When adding the access policy for the app to the key vault, the policy was created, but the **Save** button wasn't selected in the **Access policies** UI.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="53841-294">其他資源</span><span class="sxs-lookup"><span data-stu-id="53841-294">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* [<span data-ttu-id="53841-295">Microsoft Azure： Key Vault</span><span class="sxs-lookup"><span data-stu-id="53841-295">Microsoft Azure: Key Vault</span></span>](https://azure.microsoft.com/services/key-vault/)
* [<span data-ttu-id="53841-296">Microsoft Azure： Key Vault 檔</span><span class="sxs-lookup"><span data-stu-id="53841-296">Microsoft Azure: Key Vault Documentation</span></span>](/azure/key-vault/)
* [<span data-ttu-id="53841-297">如何為 Azure Key Vault 產生及傳輸受 HSM 保護的金鑰</span><span class="sxs-lookup"><span data-stu-id="53841-297">How to generate and transfer HSM-protected keys for Azure Key Vault</span></span>](/azure/key-vault/key-vault-hsm-protected-keys)
* [<span data-ttu-id="53841-298">KeyVaultClient 類別</span><span class="sxs-lookup"><span data-stu-id="53841-298">KeyVaultClient Class</span></span>](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
* [<span data-ttu-id="53841-299">快速入門：使用 .NET web 應用程式從 Azure Key Vault 設定和取出秘密</span><span class="sxs-lookup"><span data-stu-id="53841-299">Quickstart: Set and retrieve a secret from Azure Key Vault by using a .NET web app</span></span>](/azure/key-vault/quick-create-net)
* [<span data-ttu-id="53841-300">教學課程：如何在 .NET 中搭配使用 Azure Key Vault 與 Azure Windows 虛擬機器</span><span class="sxs-lookup"><span data-stu-id="53841-300">Tutorial: How to use Azure Key Vault with Azure Windows Virtual Machine in .NET</span></span>](/azure/key-vault/tutorial-net-windows-virtual-machine)
