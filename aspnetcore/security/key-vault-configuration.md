---
<span data-ttu-id="758f2-101">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="758f2-101">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="758f2-102">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="758f2-102">'Blazor'</span></span>
- <span data-ttu-id="758f2-103">'Identity'</span><span class="sxs-lookup"><span data-stu-id="758f2-103">'Identity'</span></span>
- <span data-ttu-id="758f2-104">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="758f2-104">'Let's Encrypt'</span></span>
- <span data-ttu-id="758f2-105">'Razor'</span><span class="sxs-lookup"><span data-stu-id="758f2-105">'Razor'</span></span>
- <span data-ttu-id="758f2-106">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="758f2-106">'SignalR' uid:</span></span> 

---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a><span data-ttu-id="758f2-107">ASP.NET Core 中的 Azure Key Vault 設定提供者</span><span class="sxs-lookup"><span data-stu-id="758f2-107">Azure Key Vault Configuration Provider in ASP.NET Core</span></span>

<span data-ttu-id="758f2-108">[Andrew Stanton-護士](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="758f2-108">By [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="758f2-109">本檔說明如何使用[Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/)設定提供者，從 Azure Key Vault 秘密載入應用程式設定值。</span><span class="sxs-lookup"><span data-stu-id="758f2-109">This document explains how to use the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) Configuration Provider to load app configuration values from Azure Key Vault secrets.</span></span> <span data-ttu-id="758f2-110">Azure Key Vault 是一種雲端式服務，可協助保護應用程式和服務所使用的密碼編譯金鑰和密碼。</span><span class="sxs-lookup"><span data-stu-id="758f2-110">Azure Key Vault is a cloud-based service that assists in safeguarding cryptographic keys and secrets used by apps and services.</span></span> <span data-ttu-id="758f2-111">搭配 ASP.NET Core 應用程式使用 Azure Key Vault 的常見案例包括：</span><span class="sxs-lookup"><span data-stu-id="758f2-111">Common scenarios for using Azure Key Vault with ASP.NET Core apps include:</span></span>

* <span data-ttu-id="758f2-112">控制敏感性設定資料的存取權。</span><span class="sxs-lookup"><span data-stu-id="758f2-112">Controlling access to sensitive configuration data.</span></span>
* <span data-ttu-id="758f2-113">當儲存設定資料時，符合 FIPS 140-2 Level 2 驗證的硬體安全性模組（HSM）的需求。</span><span class="sxs-lookup"><span data-stu-id="758f2-113">Meeting the requirement for FIPS 140-2 Level 2 validated Hardware Security Modules (HSM's) when storing configuration data.</span></span>

<span data-ttu-id="758f2-114">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="758f2-114">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="packages"></a><span data-ttu-id="758f2-115">套件</span><span class="sxs-lookup"><span data-stu-id="758f2-115">Packages</span></span>

<span data-ttu-id="758f2-116">將套件參考新增至[AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/)套件。</span><span class="sxs-lookup"><span data-stu-id="758f2-116">Add a package reference to the [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) package.</span></span>

## <a name="sample-app"></a><span data-ttu-id="758f2-117">範例應用程式</span><span class="sxs-lookup"><span data-stu-id="758f2-117">Sample app</span></span>

<span data-ttu-id="758f2-118">範例應用程式會以 Program.cs 檔案頂端的語句所決定的兩種模式之一來執行 `#define` ： *Program.cs*</span><span class="sxs-lookup"><span data-stu-id="758f2-118">The sample app runs in either of two modes determined by the `#define` statement at the top of the *Program.cs* file:</span></span>

* <span data-ttu-id="758f2-119">`Certificate`：示範如何使用 Azure Key Vault 的用戶端識別碼和 x.509 憑證，來存取儲存在 Azure Key Vault 中的秘密。</span><span class="sxs-lookup"><span data-stu-id="758f2-119">`Certificate`: Demonstrates the use of an Azure Key Vault Client ID and X.509 certificate to access secrets stored in Azure Key Vault.</span></span> <span data-ttu-id="758f2-120">這個版本的範例可以從任何位置執行，部署至 Azure App Service 或任何能夠提供 ASP.NET Core 應用程式的主機。</span><span class="sxs-lookup"><span data-stu-id="758f2-120">This version of the sample can be run from any location, deployed to Azure App Service or any host capable of serving an ASP.NET Core app.</span></span>
* <span data-ttu-id="758f2-121">`Managed`：示範如何使用[適用于 Azure 資源的受控](/azure/active-directory/managed-identities-azure-resources/overview)識別來驗證應用程式，以在未儲存于應用程式代碼或設定中的認證 Azure AD 驗證 Azure Key Vault。</span><span class="sxs-lookup"><span data-stu-id="758f2-121">`Managed`: Demonstrates how to use [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview) to authenticate the app to Azure Key Vault with Azure AD authentication without credentials stored in the app's code or configuration.</span></span> <span data-ttu-id="758f2-122">使用受控識別進行驗證時，不需要 Azure AD 應用程式識別碼和密碼（用戶端密碼）。</span><span class="sxs-lookup"><span data-stu-id="758f2-122">When using managed identities to authenticate, an Azure AD Application ID and Password (Client Secret) aren't required.</span></span> <span data-ttu-id="758f2-123">`Managed`範例的版本必須部署至 Azure。</span><span class="sxs-lookup"><span data-stu-id="758f2-123">The `Managed` version of the sample must be deployed to Azure.</span></span> <span data-ttu-id="758f2-124">請遵循[使用適用于 Azure 資源的受控](#use-managed-identities-for-azure-resources)識別一節中的指導方針。</span><span class="sxs-lookup"><span data-stu-id="758f2-124">Follow the guidance in the [Use the Managed identities for Azure resources](#use-managed-identities-for-azure-resources) section.</span></span>

<span data-ttu-id="758f2-125">如需有關如何使用預處理器指示詞（）來設定範例應用程式的詳細資訊 `#define` ，請參閱 <xref:index#preprocessor-directives-in-sample-code> 。</span><span class="sxs-lookup"><span data-stu-id="758f2-125">For more information on how to configure a sample app using preprocessor directives (`#define`), see <xref:index#preprocessor-directives-in-sample-code>.</span></span>

## <a name="secret-storage-in-the-development-environment"></a><span data-ttu-id="758f2-126">開發環境中的秘密儲存</span><span class="sxs-lookup"><span data-stu-id="758f2-126">Secret storage in the Development environment</span></span>

<span data-ttu-id="758f2-127">使用[秘密管理員工具](xref:security/app-secrets)在本機設定秘密。</span><span class="sxs-lookup"><span data-stu-id="758f2-127">Set secrets locally using the [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="758f2-128">當範例應用程式在開發環境中的本機電腦上執行時，會從本機密碼管理員存放區載入秘密。</span><span class="sxs-lookup"><span data-stu-id="758f2-128">When the sample app runs on the local machine in the Development environment, secrets are loaded from the local Secret Manager store.</span></span>

<span data-ttu-id="758f2-129">「密碼管理員」工具需要 `<UserSecretsId>` 應用程式專案檔中的屬性。</span><span class="sxs-lookup"><span data-stu-id="758f2-129">The Secret Manager tool requires a `<UserSecretsId>` property in the app's project file.</span></span> <span data-ttu-id="758f2-130">將屬性值（ `{GUID}` ）設定為任何唯一的 GUID：</span><span class="sxs-lookup"><span data-stu-id="758f2-130">Set the property value (`{GUID}`) to any unique GUID:</span></span>

```xml
<PropertyGroup>
  <UserSecretsId>{GUID}</UserSecretsId>
</PropertyGroup>
```

<span data-ttu-id="758f2-131">密碼會以名稱/值組的形式建立。</span><span class="sxs-lookup"><span data-stu-id="758f2-131">Secrets are created as name-value pairs.</span></span> <span data-ttu-id="758f2-132">階層式值（設定區段）使用 `:` （冒號）做為[ASP.NET Core](xref:fundamentals/configuration/index)設定機碼名稱中的分隔符號。</span><span class="sxs-lookup"><span data-stu-id="758f2-132">Hierarchical values (configuration sections) use a `:` (colon) as a separator in [ASP.NET Core configuration](xref:fundamentals/configuration/index) key names.</span></span>

<span data-ttu-id="758f2-133">密碼管理員會從開啟的命令 shell 使用到專案的[內容根目錄](xref:fundamentals/index#content-root)，其中 `{SECRET NAME}` 是名稱，而 `{SECRET VALUE}` 是值：</span><span class="sxs-lookup"><span data-stu-id="758f2-133">The Secret Manager is used from a command shell opened to the project's [content root](xref:fundamentals/index#content-root), where `{SECRET NAME}` is the name and `{SECRET VALUE}` is the value:</span></span>

```dotnetcli
dotnet user-secrets set "{SECRET NAME}" "{SECRET VALUE}"
```

<span data-ttu-id="758f2-134">在命令 shell 中，從專案的[內容根目錄](xref:fundamentals/index#content-root)執行下列命令，以設定範例應用程式的秘密：</span><span class="sxs-lookup"><span data-stu-id="758f2-134">Execute the following commands in a command shell from the project's [content root](xref:fundamentals/index#content-root) to set the secrets for the sample app:</span></span>

```dotnetcli
dotnet user-secrets set "SecretName" "secret_value_1_dev"
dotnet user-secrets set "Section:SecretName" "secret_value_2_dev"
```

<span data-ttu-id="758f2-135">當這些秘密儲存在具有 Azure Key Vault 區段之[生產環境中的秘密儲存](#secret-storage-in-the-production-environment-with-azure-key-vault)Azure Key Vault 中時， `_dev` 尾碼會變更為 `_prod` 。</span><span class="sxs-lookup"><span data-stu-id="758f2-135">When these secrets are stored in Azure Key Vault in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section, the `_dev` suffix is changed to `_prod`.</span></span> <span data-ttu-id="758f2-136">尾碼會在應用程式的輸出中提供視覺提示，指出設定值的來源。</span><span class="sxs-lookup"><span data-stu-id="758f2-136">The suffix provides a visual cue in the app's output indicating the source of the configuration values.</span></span>

## <a name="secret-storage-in-the-production-environment-with-azure-key-vault"></a><span data-ttu-id="758f2-137">在生產環境中使用 Azure Key Vault 的秘密儲存</span><span class="sxs-lookup"><span data-stu-id="758f2-137">Secret storage in the Production environment with Azure Key Vault</span></span>

<span data-ttu-id="758f2-138">以下摘要說明[快速入門：從 Azure Key Vault 使用 Azure CLI 主題設定和取出秘密](/azure/key-vault/quick-create-cli)，以建立 Azure Key Vault 並儲存範例應用程式所使用的秘密。</span><span class="sxs-lookup"><span data-stu-id="758f2-138">The instructions provided by the [Quickstart: Set and retrieve a secret from Azure Key Vault using Azure CLI](/azure/key-vault/quick-create-cli) topic are summarized here for creating an Azure Key Vault and storing secrets used by the sample app.</span></span> <span data-ttu-id="758f2-139">如需進一步的詳細資料，請參閱主題。</span><span class="sxs-lookup"><span data-stu-id="758f2-139">Refer to the topic for further details.</span></span>

1. <span data-ttu-id="758f2-140">使用下列其中一種方法，在[Azure 入口網站](https://portal.azure.com/)中開啟 Azure Cloud shell：</span><span class="sxs-lookup"><span data-stu-id="758f2-140">Open Azure Cloud shell using any one of the following methods in the [Azure portal](https://portal.azure.com/):</span></span>

   * <span data-ttu-id="758f2-141">選取程式碼區塊右上角的 [試試看]  。</span><span class="sxs-lookup"><span data-stu-id="758f2-141">Select **Try It** in the upper-right corner of a code block.</span></span> <span data-ttu-id="758f2-142">在文字方塊中使用搜尋字串 "Azure CLI"。</span><span class="sxs-lookup"><span data-stu-id="758f2-142">Use the search string "Azure CLI" in the text box.</span></span>
   * <span data-ttu-id="758f2-143">使用 [**啟動 Cloud Shell** ] 按鈕，在瀏覽器中開啟 Cloud Shell。</span><span class="sxs-lookup"><span data-stu-id="758f2-143">Open Cloud Shell in your browser with the **Launch Cloud Shell** button.</span></span>
   * <span data-ttu-id="758f2-144">選取 Azure 入口網站右上角功能表上的 **[Cloud Shell]** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="758f2-144">Select the **Cloud Shell** button on the menu in the upper-right corner of the Azure portal.</span></span>

   <span data-ttu-id="758f2-145">如需詳細資訊，請參閱[Azure Cloud Shell 的](/azure/cloud-shell/overview) [Azure CLI](/cli/azure/)和總覽。</span><span class="sxs-lookup"><span data-stu-id="758f2-145">For more information, see [Azure CLI](/cli/azure/) and [Overview of Azure Cloud Shell](/azure/cloud-shell/overview).</span></span>

1. <span data-ttu-id="758f2-146">如果您尚未驗證，請使用命令登入 `az login` 。</span><span class="sxs-lookup"><span data-stu-id="758f2-146">If you aren't already authenticated, sign in with the `az login` command.</span></span>

1. <span data-ttu-id="758f2-147">使用下列命令建立資源群組，其中 `{RESOURCE GROUP NAME}` 是新資源群組的資源組名，而 `{LOCATION}` 是 Azure 區域（datacenter）：</span><span class="sxs-lookup"><span data-stu-id="758f2-147">Create a resource group with the following command, where `{RESOURCE GROUP NAME}` is the resource group name for the new resource group and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```azurecli
   az group create --name "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="758f2-148">使用下列命令在資源群組中建立 key vault，其中 `{KEY VAULT NAME}` 是新金鑰保存庫的名稱，而 `{LOCATION}` 是 Azure 區域（datacenter）：</span><span class="sxs-lookup"><span data-stu-id="758f2-148">Create a key vault in the resource group with the following command, where `{KEY VAULT NAME}` is the name for the new key vault and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```azurecli
   az keyvault create --name {KEY VAULT NAME} --resource-group "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="758f2-149">在金鑰保存庫中建立秘密，做為名稱/值配對。</span><span class="sxs-lookup"><span data-stu-id="758f2-149">Create secrets in the key vault as name-value pairs.</span></span>

   <span data-ttu-id="758f2-150">Azure Key Vault 秘密名稱僅限於英數位元和連字號。</span><span class="sxs-lookup"><span data-stu-id="758f2-150">Azure Key Vault secret names are limited to alphanumeric characters and dashes.</span></span> <span data-ttu-id="758f2-151">階層式值（設定區段）使用 `--` （兩個虛線）做為分隔符號。</span><span class="sxs-lookup"><span data-stu-id="758f2-151">Hierarchical values (configuration sections) use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="758f2-152">冒號，通常用來從[ASP.NET Core](xref:fundamentals/configuration/index)設定中的子機碼分隔區段，但不允許用在金鑰保存庫密碼名稱中。</span><span class="sxs-lookup"><span data-stu-id="758f2-152">Colons, which are normally used to delimit a section from a subkey in [ASP.NET Core configuration](xref:fundamentals/configuration/index), aren't allowed in key vault secret names.</span></span> <span data-ttu-id="758f2-153">因此，當密碼載入應用程式的設定時，會使用兩個破折號並交換冒號。</span><span class="sxs-lookup"><span data-stu-id="758f2-153">Therefore, two dashes are used and swapped for a colon when the secrets are loaded into the app's configuration.</span></span>

   <span data-ttu-id="758f2-154">下列秘密可用於範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="758f2-154">The following secrets are for use with the sample app.</span></span> <span data-ttu-id="758f2-155">這些值包含後置詞 `_prod` ，以區別它們與 `_dev` 開發環境中從使用者秘密載入的尾碼值。</span><span class="sxs-lookup"><span data-stu-id="758f2-155">The values include a `_prod` suffix to distinguish them from the `_dev` suffix values loaded in the Development environment from User Secrets.</span></span> <span data-ttu-id="758f2-156">將取代 `{KEY VAULT NAME}` 為您在上一個步驟中建立的金鑰保存庫名稱：</span><span class="sxs-lookup"><span data-stu-id="758f2-156">Replace `{KEY VAULT NAME}` with the name of the key vault that you created in the prior step:</span></span>

   ```azurecli
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "SecretName" --value "secret_value_1_prod"
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "Section--SecretName" --value "secret_value_2_prod"
   ```

## <a name="use-application-id-and-x509-certificate-for-non-azure-hosted-apps"></a><span data-ttu-id="758f2-157">將應用程式識別碼和 x.509 憑證用於非 Azure 託管的應用程式</span><span class="sxs-lookup"><span data-stu-id="758f2-157">Use Application ID and X.509 certificate for non-Azure-hosted apps</span></span>

<span data-ttu-id="758f2-158">設定 Azure AD、Azure Key Vault 和應用程式，以在應用程式裝載于**Azure 外部時**，使用 Azure Active Directory 應用程式識別碼和 x.509 憑證來驗證金鑰保存庫。</span><span class="sxs-lookup"><span data-stu-id="758f2-158">Configure Azure AD, Azure Key Vault, and the app to use an Azure Active Directory Application ID and X.509 certificate to authenticate to a key vault **when the app is hosted outside of Azure**.</span></span> <span data-ttu-id="758f2-159">如需詳細資訊，請參閱[關於金鑰、祕密和憑證](/azure/key-vault/about-keys-secrets-and-certificates)。</span><span class="sxs-lookup"><span data-stu-id="758f2-159">For more information, see [About keys, secrets, and certificates](/azure/key-vault/about-keys-secrets-and-certificates).</span></span>

> [!NOTE]
> <span data-ttu-id="758f2-160">雖然 Azure 中裝載的應用程式支援使用應用程式識別碼和 x.509 憑證，但建議您在 Azure 中裝載應用程式時，使用[適用于 azure 資源的受控](#use-managed-identities-for-azure-resources)識別。</span><span class="sxs-lookup"><span data-stu-id="758f2-160">Although using an Application ID and X.509 certificate is supported for apps hosted in Azure, we recommend using [Managed identities for Azure resources](#use-managed-identities-for-azure-resources) when hosting an app in Azure.</span></span> <span data-ttu-id="758f2-161">受控識別不需要在應用程式中或在開發環境中儲存憑證。</span><span class="sxs-lookup"><span data-stu-id="758f2-161">Managed identities don't require storing a certificate in the app or in the development environment.</span></span>

<span data-ttu-id="758f2-162">當 `#define` *Program.cs*檔案頂端的語句設定為時，範例應用程式會使用應用程式識別碼和 x.509 憑證 `Certificate` 。</span><span class="sxs-lookup"><span data-stu-id="758f2-162">The sample app uses an Application ID and X.509 certificate when the `#define` statement at the top of the *Program.cs* file is set to `Certificate`.</span></span>

1. <span data-ttu-id="758f2-163">建立 PKCS # 12 封存檔案（*.pfx*）憑證。</span><span class="sxs-lookup"><span data-stu-id="758f2-163">Create a PKCS#12 archive (*.pfx*) certificate.</span></span> <span data-ttu-id="758f2-164">建立憑證的選項包括[Windows](/windows/desktop/seccrypto/makecert)和[OpenSSL](https://www.openssl.org/)上的 MakeCert。</span><span class="sxs-lookup"><span data-stu-id="758f2-164">Options for creating certificates include [MakeCert on Windows](/windows/desktop/seccrypto/makecert) and [OpenSSL](https://www.openssl.org/).</span></span>
1. <span data-ttu-id="758f2-165">將憑證安裝到目前使用者的個人憑證存儲。</span><span class="sxs-lookup"><span data-stu-id="758f2-165">Install the certificate into the current user's personal certificate store.</span></span> <span data-ttu-id="758f2-166">將金鑰標示為可匯出是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="758f2-166">Marking the key as exportable is optional.</span></span> <span data-ttu-id="758f2-167">記下憑證的指紋，這會在此程式稍後使用。</span><span class="sxs-lookup"><span data-stu-id="758f2-167">Note the certificate's thumbprint, which is used later in this process.</span></span>
1. <span data-ttu-id="758f2-168">將 PKCS # 12 封存（*.pfx*）憑證匯出為 DER 編碼憑證（*.cer*）。</span><span class="sxs-lookup"><span data-stu-id="758f2-168">Export the PKCS#12 archive (*.pfx*) certificate as a DER-encoded certificate (*.cer*).</span></span>
1. <span data-ttu-id="758f2-169">使用 Azure AD （**應用程式註冊**）註冊應用程式。</span><span class="sxs-lookup"><span data-stu-id="758f2-169">Register the app with Azure AD (**App registrations**).</span></span>
1. <span data-ttu-id="758f2-170">將 DER 編碼的憑證（*.cer*）上傳至 Azure AD：</span><span class="sxs-lookup"><span data-stu-id="758f2-170">Upload the DER-encoded certificate (*.cer*) to Azure AD:</span></span>
   1. <span data-ttu-id="758f2-171">在 Azure AD 中選取應用程式。</span><span class="sxs-lookup"><span data-stu-id="758f2-171">Select the app in Azure AD.</span></span>
   1. <span data-ttu-id="758f2-172">流覽至 [**憑證 & 密碼**]。</span><span class="sxs-lookup"><span data-stu-id="758f2-172">Navigate to **Certificates & secrets**.</span></span>
   1. <span data-ttu-id="758f2-173">選取 [**上傳憑證**] 來上傳包含公開金鑰的憑證。</span><span class="sxs-lookup"><span data-stu-id="758f2-173">Select **Upload certificate** to upload the certificate, which contains the public key.</span></span> <span data-ttu-id="758f2-174">*.Cer*、 *pem*或 *.crt*憑證是可接受的。</span><span class="sxs-lookup"><span data-stu-id="758f2-174">A *.cer*, *.pem*, or *.crt* certificate is acceptable.</span></span>
1. <span data-ttu-id="758f2-175">將金鑰保存庫名稱、應用程式識別碼和憑證指紋儲存在應用程式的*appsettings*中。</span><span class="sxs-lookup"><span data-stu-id="758f2-175">Store the key vault name, Application ID, and certificate thumbprint in the app's *appsettings.json* file.</span></span>
1. <span data-ttu-id="758f2-176">流覽至 Azure 入口網站中的 [**金鑰保存庫**]。</span><span class="sxs-lookup"><span data-stu-id="758f2-176">Navigate to **Key vaults** in the Azure portal.</span></span>
1. <span data-ttu-id="758f2-177">選取您在[生產環境中使用 Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault)一節所建立的金鑰保存庫。</span><span class="sxs-lookup"><span data-stu-id="758f2-177">Select the key vault that you created in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section.</span></span>
1. <span data-ttu-id="758f2-178">選取 [**存取原則**]。</span><span class="sxs-lookup"><span data-stu-id="758f2-178">Select **Access policies**.</span></span>
1. <span data-ttu-id="758f2-179">選取 [**新增存取原則**]。</span><span class="sxs-lookup"><span data-stu-id="758f2-179">Select **Add Access Policy**.</span></span>
1. <span data-ttu-id="758f2-180">開啟 [**秘密許可權**]，並提供具有 [**取得**] 和 [**列出**] 許可權的應用程式。</span><span class="sxs-lookup"><span data-stu-id="758f2-180">Open **Secret permissions** and provide the app with **Get** and **List** permissions.</span></span>
1. <span data-ttu-id="758f2-181">選取 [**選取主體**]，然後依名稱選取已註冊的應用程式。</span><span class="sxs-lookup"><span data-stu-id="758f2-181">Select **Select principal** and select the registered app by name.</span></span> <span data-ttu-id="758f2-182">選取 [選取]  按鈕。</span><span class="sxs-lookup"><span data-stu-id="758f2-182">Select the **Select** button.</span></span>
1. <span data-ttu-id="758f2-183">選取 [確定]  。</span><span class="sxs-lookup"><span data-stu-id="758f2-183">Select **OK**.</span></span>
1. <span data-ttu-id="758f2-184">選取 [儲存]  。</span><span class="sxs-lookup"><span data-stu-id="758f2-184">Select **Save**.</span></span>
1. <span data-ttu-id="758f2-185">部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="758f2-185">Deploy the app.</span></span>

<span data-ttu-id="758f2-186">`Certificate`範例應用程式會從 `IConfigurationRoot` 使用與秘密名稱相同的名稱取得其設定值：</span><span class="sxs-lookup"><span data-stu-id="758f2-186">The `Certificate` sample app obtains its configuration values from `IConfigurationRoot` with the same name as the secret name:</span></span>

* <span data-ttu-id="758f2-187">非階層式值：的值 `SecretName` 是使用取得 `config["SecretName"]` 。</span><span class="sxs-lookup"><span data-stu-id="758f2-187">Non-hierarchical values: The value for `SecretName` is obtained with `config["SecretName"]`.</span></span>
* <span data-ttu-id="758f2-188">階層式值（區段）：使用 `:` （冒號）標記法或 `GetSection` 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="758f2-188">Hierarchical values (sections): Use `:` (colon) notation or the `GetSection` extension method.</span></span> <span data-ttu-id="758f2-189">使用下列其中一種方法來取得設定值：</span><span class="sxs-lookup"><span data-stu-id="758f2-189">Use either of these approaches to obtain the configuration value:</span></span>
  * `config["Section:SecretName"]`
  * `config.GetSection("Section")["SecretName"]`

<span data-ttu-id="758f2-190">X.509 憑證是由作業系統所管理。</span><span class="sxs-lookup"><span data-stu-id="758f2-190">The X.509 certificate is managed by the OS.</span></span> <span data-ttu-id="758f2-191">應用程式會 <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> 使用*appsettings*所提供的值來呼叫：</span><span class="sxs-lookup"><span data-stu-id="758f2-191">The app calls <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> with values supplied by the *appsettings.json* file:</span></span>

[!code-csharp[](key-vault-configuration/samples/3.x/SampleApp/Program.cs?name=snippet1&highlight=20-23)]

<span data-ttu-id="758f2-192">範例值：</span><span class="sxs-lookup"><span data-stu-id="758f2-192">Example values:</span></span>

* <span data-ttu-id="758f2-193">金鑰保存庫名稱：`contosovault`</span><span class="sxs-lookup"><span data-stu-id="758f2-193">Key vault name: `contosovault`</span></span>
* <span data-ttu-id="758f2-194">應用程式識別碼：`627e911e-43cc-61d4-992e-12db9c81b413`</span><span class="sxs-lookup"><span data-stu-id="758f2-194">Application ID: `627e911e-43cc-61d4-992e-12db9c81b413`</span></span>
* <span data-ttu-id="758f2-195">憑證指紋：`fe14593dd66b2406c5269d742d04b6e1ab03adb1`</span><span class="sxs-lookup"><span data-stu-id="758f2-195">Certificate thumbprint: `fe14593dd66b2406c5269d742d04b6e1ab03adb1`</span></span>

<span data-ttu-id="758f2-196">*appsettings. json*：</span><span class="sxs-lookup"><span data-stu-id="758f2-196">*appsettings.json*:</span></span>

[!code-json[](key-vault-configuration/samples/3.x/SampleApp/appsettings.json?highlight=10-12)]

<span data-ttu-id="758f2-197">當您執行應用程式時，網頁會顯示已載入的密碼值。</span><span class="sxs-lookup"><span data-stu-id="758f2-197">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="758f2-198">在開發環境中，秘密值會以 `_dev` 尾碼載入。</span><span class="sxs-lookup"><span data-stu-id="758f2-198">In the Development environment, secret values load with the `_dev` suffix.</span></span> <span data-ttu-id="758f2-199">在生產環境中，值會以後綴載入 `_prod` 。</span><span class="sxs-lookup"><span data-stu-id="758f2-199">In the Production environment, the values load with the `_prod` suffix.</span></span>

## <a name="use-managed-identities-for-azure-resources"></a><span data-ttu-id="758f2-200">使用適用于 Azure 資源的受控識別</span><span class="sxs-lookup"><span data-stu-id="758f2-200">Use Managed identities for Azure resources</span></span>

<span data-ttu-id="758f2-201">**部署至 azure 的應用程式**可以利用[Azure 資源的受控](/azure/active-directory/managed-identities-azure-resources/overview)識別，讓應用程式使用 Azure AD 驗證，而不需要在應用程式中儲存認證（應用程式識別碼和密碼/用戶端密碼）來驗證 Azure Key Vault。</span><span class="sxs-lookup"><span data-stu-id="758f2-201">**An app deployed to Azure** can take advantage of [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview), which allows the app to authenticate with Azure Key Vault using Azure AD authentication without credentials (Application ID and Password/Client Secret) stored in the app.</span></span>

<span data-ttu-id="758f2-202">當 `#define` *Program.cs*檔案頂端的語句設定為時，範例應用程式會使用適用于 Azure 資源的受控識別 `Managed` 。</span><span class="sxs-lookup"><span data-stu-id="758f2-202">The sample app uses Managed identities for Azure resources when the `#define` statement at the top of the *Program.cs* file is set to `Managed`.</span></span>

<span data-ttu-id="758f2-203">在應用程式的*appsettings*中輸入保存庫名稱。</span><span class="sxs-lookup"><span data-stu-id="758f2-203">Enter the vault name into the app's *appsettings.json* file.</span></span> <span data-ttu-id="758f2-204">在設定為版本時，範例應用程式不需要應用程式識別碼和密碼（用戶端密碼） `Managed` ，因此您可以忽略這些設定專案。</span><span class="sxs-lookup"><span data-stu-id="758f2-204">The sample app doesn't require an Application ID and Password (Client Secret) when set to the `Managed` version, so you can ignore those configuration entries.</span></span> <span data-ttu-id="758f2-205">應用程式會部署至 Azure，而 Azure 只會使用儲存在*appsettings*中的保存庫名稱來驗證應用程式，以存取 Azure Key Vault。</span><span class="sxs-lookup"><span data-stu-id="758f2-205">The app is deployed to Azure, and Azure authenticates the app to access Azure Key Vault only using the vault name stored in the *appsettings.json* file.</span></span>

<span data-ttu-id="758f2-206">將範例應用程式部署至 Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="758f2-206">Deploy the sample app to Azure App Service.</span></span>

<span data-ttu-id="758f2-207">部署到 Azure App Service 的應用程式會在建立服務時，自動向 Azure AD 註冊。</span><span class="sxs-lookup"><span data-stu-id="758f2-207">An app deployed to Azure App Service is automatically registered with Azure AD when the service is created.</span></span> <span data-ttu-id="758f2-208">從部署取得物件識別碼，以便在下列命令中使用。</span><span class="sxs-lookup"><span data-stu-id="758f2-208">Obtain the Object ID from the deployment for use in the following command.</span></span> <span data-ttu-id="758f2-209">物件識別碼會顯示在 App Service 面板的 Azure 入口網站中 **Identity** 。</span><span class="sxs-lookup"><span data-stu-id="758f2-209">The Object ID is shown in the Azure portal on the **Identity** panel of the App Service.</span></span>

<span data-ttu-id="758f2-210">使用 Azure CLI 和應用程式的物件識別碼，提供應用程式 `list` 和 `get` 存取金鑰保存庫的許可權：</span><span class="sxs-lookup"><span data-stu-id="758f2-210">Using Azure CLI and the app's Object ID, provide the app with `list` and `get` permissions to access the key vault:</span></span>

```azurecli
az keyvault set-policy --name {KEY VAULT NAME} --object-id {OBJECT ID} --secret-permissions get list
```

<span data-ttu-id="758f2-211">使用 Azure CLI、PowerShell 或 Azure 入口網站**重新開機應用程式**。</span><span class="sxs-lookup"><span data-stu-id="758f2-211">**Restart the app** using Azure CLI, PowerShell, or the Azure portal.</span></span>

<span data-ttu-id="758f2-212">範例應用程式：</span><span class="sxs-lookup"><span data-stu-id="758f2-212">The sample app:</span></span>

* <span data-ttu-id="758f2-213">建立 `AzureServiceTokenProvider` 不含連接字串之類別的實例。</span><span class="sxs-lookup"><span data-stu-id="758f2-213">Creates an instance of the `AzureServiceTokenProvider` class without a connection string.</span></span> <span data-ttu-id="758f2-214">未提供連接字串時，提供者會嘗試從 Azure 資源的受控識別取得存取權杖。</span><span class="sxs-lookup"><span data-stu-id="758f2-214">When a connection string isn't provided, the provider attempts to obtain an access token from Managed identities for Azure resources.</span></span>
* <span data-ttu-id="758f2-215">新的 <xref:Microsoft.Azure.KeyVault.KeyVaultClient> 會使用 `AzureServiceTokenProvider` 實例 token 回呼來建立。</span><span class="sxs-lookup"><span data-stu-id="758f2-215">A new <xref:Microsoft.Azure.KeyVault.KeyVaultClient> is created with the `AzureServiceTokenProvider` instance token callback.</span></span>
* <span data-ttu-id="758f2-216"><xref:Microsoft.Azure.KeyVault.KeyVaultClient>實例是與的預設執行搭配使用 <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> ，它會載入所有秘密值，並 `--` 以冒號（）取代索引 `:` 鍵名稱中的雙破折號（）。</span><span class="sxs-lookup"><span data-stu-id="758f2-216">The <xref:Microsoft.Azure.KeyVault.KeyVaultClient> instance is used with a default implementation of <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> that loads all secret values and replaces double-dashes (`--`) with colons (`:`) in key names.</span></span>

[!code-csharp[](key-vault-configuration/samples/3.x/SampleApp/Program.cs?name=snippet2&highlight=13-21)]

<span data-ttu-id="758f2-217">金鑰保存庫名稱範例值：`contosovault`</span><span class="sxs-lookup"><span data-stu-id="758f2-217">Key vault name example value: `contosovault`</span></span>
    
<span data-ttu-id="758f2-218">*appsettings. json*：</span><span class="sxs-lookup"><span data-stu-id="758f2-218">*appsettings.json*:</span></span>

```json
{
  "KeyVaultName": "Key Vault Name"
}
```

<span data-ttu-id="758f2-219">當您執行應用程式時，網頁會顯示已載入的密碼值。</span><span class="sxs-lookup"><span data-stu-id="758f2-219">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="758f2-220">在開發環境中，秘密值具有 `_dev` 尾碼，因為它們是由使用者密碼提供。</span><span class="sxs-lookup"><span data-stu-id="758f2-220">In the Development environment, secret values have the `_dev` suffix because they're provided by User Secrets.</span></span> <span data-ttu-id="758f2-221">在生產環境中，值是以後綴載入， `_prod` 因為它們是由 Azure Key Vault 所提供。</span><span class="sxs-lookup"><span data-stu-id="758f2-221">In the Production environment, the values load with the `_prod` suffix because they're provided by Azure Key Vault.</span></span>

<span data-ttu-id="758f2-222">如果您收到 `Access denied` 錯誤，請確認已向 Azure AD 註冊應用程式，並提供金鑰保存庫的存取權。</span><span class="sxs-lookup"><span data-stu-id="758f2-222">If you receive an `Access denied` error, confirm that the app is registered with Azure AD and provided access to the key vault.</span></span> <span data-ttu-id="758f2-223">確認您已在 Azure 中重新開機服務。</span><span class="sxs-lookup"><span data-stu-id="758f2-223">Confirm that you've restarted the service in Azure.</span></span>

<span data-ttu-id="758f2-224">如需將提供者與受控識別和 Azure DevOps 管線搭配使用的詳細資訊，請參閱使用[受控服務識別建立 VM 的 Azure Resource Manager 服務](/azure/devops/pipelines/library/connect-to-azure#create-an-azure-resource-manager-service-connection-to-a-vm-with-a-managed-service-identity)連線。</span><span class="sxs-lookup"><span data-stu-id="758f2-224">For information on using the provider with a managed identity and an Azure DevOps pipeline, see [Create an Azure Resource Manager service connection to a VM with a managed service identity](/azure/devops/pipelines/library/connect-to-azure#create-an-azure-resource-manager-service-connection-to-a-vm-with-a-managed-service-identity).</span></span>

## <a name="configuration-options"></a><span data-ttu-id="758f2-225">設定選項</span><span class="sxs-lookup"><span data-stu-id="758f2-225">Configuration options</span></span>

<span data-ttu-id="758f2-226"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>可以接受 <xref:Microsoft.Extensions.Configuration.AzureKeyVault.AzureKeyVaultConfigurationOptions> ：</span><span class="sxs-lookup"><span data-stu-id="758f2-226"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> can accept an <xref:Microsoft.Extensions.Configuration.AzureKeyVault.AzureKeyVaultConfigurationOptions>:</span></span>

```csharp
config.AddAzureKeyVault(
    new AzureKeyVaultConfigurationOptions()
    {
        ...
    });
```

| <span data-ttu-id="758f2-227">屬性</span><span class="sxs-lookup"><span data-stu-id="758f2-227">Property</span></span>         | <span data-ttu-id="758f2-228">描述</span><span class="sxs-lookup"><span data-stu-id="758f2-228">Description</span></span> |
| ---
<span data-ttu-id="758f2-229">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="758f2-229">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="758f2-230">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="758f2-230">'Blazor'</span></span>
- <span data-ttu-id="758f2-231">'Identity'</span><span class="sxs-lookup"><span data-stu-id="758f2-231">'Identity'</span></span>
- <span data-ttu-id="758f2-232">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="758f2-232">'Let's Encrypt'</span></span>
- <span data-ttu-id="758f2-233">'Razor'</span><span class="sxs-lookup"><span data-stu-id="758f2-233">'Razor'</span></span>
- <span data-ttu-id="758f2-234">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="758f2-234">'SignalR' uid:</span></span> 

-
<span data-ttu-id="758f2-235">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="758f2-235">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="758f2-236">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="758f2-236">'Blazor'</span></span>
- <span data-ttu-id="758f2-237">'Identity'</span><span class="sxs-lookup"><span data-stu-id="758f2-237">'Identity'</span></span>
- <span data-ttu-id="758f2-238">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="758f2-238">'Let's Encrypt'</span></span>
- <span data-ttu-id="758f2-239">'Razor'</span><span class="sxs-lookup"><span data-stu-id="758f2-239">'Razor'</span></span>
- <span data-ttu-id="758f2-240">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="758f2-240">'SignalR' uid:</span></span> 

-
<span data-ttu-id="758f2-241">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="758f2-241">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="758f2-242">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="758f2-242">'Blazor'</span></span>
- <span data-ttu-id="758f2-243">'Identity'</span><span class="sxs-lookup"><span data-stu-id="758f2-243">'Identity'</span></span>
- <span data-ttu-id="758f2-244">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="758f2-244">'Let's Encrypt'</span></span>
- <span data-ttu-id="758f2-245">'Razor'</span><span class="sxs-lookup"><span data-stu-id="758f2-245">'Razor'</span></span>
- <span data-ttu-id="758f2-246">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="758f2-246">'SignalR' uid:</span></span> 

-
<span data-ttu-id="758f2-247">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="758f2-247">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="758f2-248">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="758f2-248">'Blazor'</span></span>
- <span data-ttu-id="758f2-249">'Identity'</span><span class="sxs-lookup"><span data-stu-id="758f2-249">'Identity'</span></span>
- <span data-ttu-id="758f2-250">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="758f2-250">'Let's Encrypt'</span></span>
- <span data-ttu-id="758f2-251">'Razor'</span><span class="sxs-lookup"><span data-stu-id="758f2-251">'Razor'</span></span>
- <span data-ttu-id="758f2-252">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="758f2-252">'SignalR' uid:</span></span> 

-
<span data-ttu-id="758f2-253">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="758f2-253">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="758f2-254">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="758f2-254">'Blazor'</span></span>
- <span data-ttu-id="758f2-255">'Identity'</span><span class="sxs-lookup"><span data-stu-id="758f2-255">'Identity'</span></span>
- <span data-ttu-id="758f2-256">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="758f2-256">'Let's Encrypt'</span></span>
- <span data-ttu-id="758f2-257">'Razor'</span><span class="sxs-lookup"><span data-stu-id="758f2-257">'Razor'</span></span>
- <span data-ttu-id="758f2-258">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="758f2-258">'SignalR' uid:</span></span> 

-
<span data-ttu-id="758f2-259">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="758f2-259">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="758f2-260">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="758f2-260">'Blazor'</span></span>
- <span data-ttu-id="758f2-261">'Identity'</span><span class="sxs-lookup"><span data-stu-id="758f2-261">'Identity'</span></span>
- <span data-ttu-id="758f2-262">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="758f2-262">'Let's Encrypt'</span></span>
- <span data-ttu-id="758f2-263">'Razor'</span><span class="sxs-lookup"><span data-stu-id="758f2-263">'Razor'</span></span>
- <span data-ttu-id="758f2-264">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="758f2-264">'SignalR' uid:</span></span> 

<span data-ttu-id="758f2-265">-------- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="758f2-265">-------- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="758f2-266">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="758f2-266">'Blazor'</span></span>
- <span data-ttu-id="758f2-267">'Identity'</span><span class="sxs-lookup"><span data-stu-id="758f2-267">'Identity'</span></span>
- <span data-ttu-id="758f2-268">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="758f2-268">'Let's Encrypt'</span></span>
- <span data-ttu-id="758f2-269">'Razor'</span><span class="sxs-lookup"><span data-stu-id="758f2-269">'Razor'</span></span>
- <span data-ttu-id="758f2-270">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="758f2-270">'SignalR' uid:</span></span> 

-
<span data-ttu-id="758f2-271">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="758f2-271">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="758f2-272">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="758f2-272">'Blazor'</span></span>
- <span data-ttu-id="758f2-273">'Identity'</span><span class="sxs-lookup"><span data-stu-id="758f2-273">'Identity'</span></span>
- <span data-ttu-id="758f2-274">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="758f2-274">'Let's Encrypt'</span></span>
- <span data-ttu-id="758f2-275">'Razor'</span><span class="sxs-lookup"><span data-stu-id="758f2-275">'Razor'</span></span>
- <span data-ttu-id="758f2-276">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="758f2-276">'SignalR' uid:</span></span> 

-
<span data-ttu-id="758f2-277">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="758f2-277">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="758f2-278">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="758f2-278">'Blazor'</span></span>
- <span data-ttu-id="758f2-279">'Identity'</span><span class="sxs-lookup"><span data-stu-id="758f2-279">'Identity'</span></span>
- <span data-ttu-id="758f2-280">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="758f2-280">'Let's Encrypt'</span></span>
- <span data-ttu-id="758f2-281">'Razor'</span><span class="sxs-lookup"><span data-stu-id="758f2-281">'Razor'</span></span>
- <span data-ttu-id="758f2-282">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="758f2-282">'SignalR' uid:</span></span> 

<span data-ttu-id="758f2-283">------ | |`Client`         | <xref:Microsoft.Azure.KeyVault.KeyVaultClient>用來抓取值。</span><span class="sxs-lookup"><span data-stu-id="758f2-283">------ | | `Client`         | <xref:Microsoft.Azure.KeyVault.KeyVaultClient> to use for retrieving values.</span></span> <span data-ttu-id="758f2-284">| |`Manager`        | <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>用來控制密碼載入的實例。</span><span class="sxs-lookup"><span data-stu-id="758f2-284">| | `Manager`        | <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> instance used to control secret loading.</span></span> <span data-ttu-id="758f2-285">| |`ReloadInterval` | `Timespan`在輪詢金鑰保存庫以進行變更的嘗試之間等待。</span><span class="sxs-lookup"><span data-stu-id="758f2-285">| | `ReloadInterval` | `Timespan` to wait between attempts at polling the key vault for changes.</span></span> <span data-ttu-id="758f2-286">預設值為 `null` （不重載設定）。</span><span class="sxs-lookup"><span data-stu-id="758f2-286">The default value is `null` (configuration isn't reloaded).</span></span> <span data-ttu-id="758f2-287">| |`Vault`          |金鑰保存庫 URI。</span><span class="sxs-lookup"><span data-stu-id="758f2-287">| | `Vault`          | Key vault URI.</span></span> |

## <a name="use-a-key-name-prefix"></a><span data-ttu-id="758f2-288">使用索引鍵名稱前置詞</span><span class="sxs-lookup"><span data-stu-id="758f2-288">Use a key name prefix</span></span>

<span data-ttu-id="758f2-289"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>提供接受的執行的多載 <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> ，可讓您控制如何將金鑰保存庫密碼轉換成設定金鑰。</span><span class="sxs-lookup"><span data-stu-id="758f2-289"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> provides an overload that accepts an implementation of <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>, which allows you to control how key vault secrets are converted into configuration keys.</span></span> <span data-ttu-id="758f2-290">例如，您可以根據您在應用程式啟動時提供的首碼值，執行介面來載入密碼值。</span><span class="sxs-lookup"><span data-stu-id="758f2-290">For example, you can implement the interface to load secret values based on a prefix value you provide at app startup.</span></span> <span data-ttu-id="758f2-291">例如，這可讓您根據應用程式的版本來載入密碼。</span><span class="sxs-lookup"><span data-stu-id="758f2-291">This allows you, for example, to load secrets based on the version of the app.</span></span>

> [!WARNING]
> <span data-ttu-id="758f2-292">請勿在金鑰保存庫秘密上使用前置詞，將多個應用程式的秘密放入相同的金鑰保存庫，或將環境秘密（例如*開發*與*生產*密碼）放入相同的保存庫中。</span><span class="sxs-lookup"><span data-stu-id="758f2-292">Don't use prefixes on key vault secrets to place secrets for multiple apps into the same key vault or to place environmental secrets (for example, *development* versus *production* secrets) into the same vault.</span></span> <span data-ttu-id="758f2-293">我們建議不同的應用程式和開發/生產環境使用不同的金鑰保存庫，以隔離最高安全性層級的應用程式環境。</span><span class="sxs-lookup"><span data-stu-id="758f2-293">We recommend that different apps and development/production environments use separate key vaults to isolate app environments for the highest level of security.</span></span>

<span data-ttu-id="758f2-294">在下列範例中，會在金鑰保存庫中建立秘密（並使用適用于開發環境的秘密管理員工具）來進行 `5000-AppSecret` （金鑰保存庫密碼名稱中不允許週期）。</span><span class="sxs-lookup"><span data-stu-id="758f2-294">In the following example, a secret is established in the key vault (and using the Secret Manager tool for the Development environment) for `5000-AppSecret` (periods aren't allowed in key vault secret names).</span></span> <span data-ttu-id="758f2-295">此秘密代表應用程式版本5.0.0.0 的應用程式密碼。</span><span class="sxs-lookup"><span data-stu-id="758f2-295">This secret represents an app secret for version 5.0.0.0 of the app.</span></span> <span data-ttu-id="758f2-296">針對其他版本的應用程式（5.1.0.0），會將密碼新增至金鑰保存庫（並使用密碼管理員工具）來進行 `5100-AppSecret` 。</span><span class="sxs-lookup"><span data-stu-id="758f2-296">For another version of the app, 5.1.0.0, a secret is added to the key vault (and using the Secret Manager tool) for `5100-AppSecret`.</span></span> <span data-ttu-id="758f2-297">每個應用程式版本都會將其版本設定的秘密值載入至其設定中 `AppSecret` ，因為它會在載入秘密時去除版本。</span><span class="sxs-lookup"><span data-stu-id="758f2-297">Each app version loads its versioned secret value into its configuration as `AppSecret`, stripping off the version as it loads the secret.</span></span>

<span data-ttu-id="758f2-298"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>使用自訂進行呼叫 <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> ：</span><span class="sxs-lookup"><span data-stu-id="758f2-298"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> is called with a custom <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>:</span></span>

[!code-csharp[](key-vault-configuration/samples_snapshot/Program.cs)]

<span data-ttu-id="758f2-299">執行動作會 <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> 回應秘密的版本前置詞，以將適當的密碼載入設定中：</span><span class="sxs-lookup"><span data-stu-id="758f2-299">The <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> implementation reacts to the version prefixes of secrets to load the proper secret into configuration:</span></span>

* <span data-ttu-id="758f2-300">`Load`在名稱開頭為前置詞時，載入密碼。</span><span class="sxs-lookup"><span data-stu-id="758f2-300">`Load` loads a secret when its name starts with the prefix.</span></span> <span data-ttu-id="758f2-301">其他秘密則不會載入。</span><span class="sxs-lookup"><span data-stu-id="758f2-301">Other secrets aren't loaded.</span></span>
* <span data-ttu-id="758f2-302">`GetKey`:</span><span class="sxs-lookup"><span data-stu-id="758f2-302">`GetKey`:</span></span>
  * <span data-ttu-id="758f2-303">移除秘密名稱中的前置詞。</span><span class="sxs-lookup"><span data-stu-id="758f2-303">Removes the prefix from the secret name.</span></span>
  * <span data-ttu-id="758f2-304">將任何名稱中的兩個破折號取代為 `KeyDelimiter` ，這是設定中使用的分隔符號（通常是冒號）。</span><span class="sxs-lookup"><span data-stu-id="758f2-304">Replaces two dashes in any name with the `KeyDelimiter`, which is the delimiter used in configuration (usually a colon).</span></span> <span data-ttu-id="758f2-305">Azure Key Vault 在密碼名稱中不允許冒號。</span><span class="sxs-lookup"><span data-stu-id="758f2-305">Azure Key Vault doesn't allow a colon in secret names.</span></span>

[!code-csharp[](key-vault-configuration/samples_snapshot/Startup.cs)]

<span data-ttu-id="758f2-306">`Load`方法是由可逐一查看保存庫密碼的提供者演算法所呼叫，以尋找具有版本前置詞的金鑰。</span><span class="sxs-lookup"><span data-stu-id="758f2-306">The `Load` method is called by a provider algorithm that iterates through the vault secrets to find the ones that have the version prefix.</span></span> <span data-ttu-id="758f2-307">當找到版本前置詞時 `Load` ，演算法會使用 `GetKey` 方法來傳回密碼名稱的設定名稱。</span><span class="sxs-lookup"><span data-stu-id="758f2-307">When a version prefix is found with `Load`, the algorithm uses the `GetKey` method to return the configuration name of the secret name.</span></span> <span data-ttu-id="758f2-308">它會從密碼的名稱中去除版本前置詞，並傳回其餘的秘密名稱，以載入至應用程式的設定名稱/值配對。</span><span class="sxs-lookup"><span data-stu-id="758f2-308">It strips off the version prefix from the secret's name and returns the rest of the secret name for loading into the app's configuration name-value pairs.</span></span>

<span data-ttu-id="758f2-309">當此方法執行時：</span><span class="sxs-lookup"><span data-stu-id="758f2-309">When this approach is implemented:</span></span>

1. <span data-ttu-id="758f2-310">應用程式的專案檔中指定的應用程式版本。</span><span class="sxs-lookup"><span data-stu-id="758f2-310">The app's version specified in the app's project file.</span></span> <span data-ttu-id="758f2-311">在下列範例中，應用程式的版本會設定為 `5.0.0.0` ：</span><span class="sxs-lookup"><span data-stu-id="758f2-311">In the following example, the app's version is set to `5.0.0.0`:</span></span>

   ```xml
   <PropertyGroup>
     <Version>5.0.0.0</Version>
   </PropertyGroup>
   ```

1. <span data-ttu-id="758f2-312">確認 `<UserSecretsId>` 屬性存在於應用程式的專案檔中，其中 `{GUID}` 是使用者提供的 GUID：</span><span class="sxs-lookup"><span data-stu-id="758f2-312">Confirm that a `<UserSecretsId>` property is present in the app's project file, where `{GUID}` is a user-supplied GUID:</span></span>

   ```xml
   <PropertyGroup>
     <UserSecretsId>{GUID}</UserSecretsId>
   </PropertyGroup>
   ```

   <span data-ttu-id="758f2-313">使用[秘密管理員工具](xref:security/app-secrets)在本機儲存下列秘密：</span><span class="sxs-lookup"><span data-stu-id="758f2-313">Save the following secrets locally with the [Secret Manager tool](xref:security/app-secrets):</span></span>

   ```dotnetcli
   dotnet user-secrets set "5000-AppSecret" "5.0.0.0_secret_value_dev"
   dotnet user-secrets set "5100-AppSecret" "5.1.0.0_secret_value_dev"
   ```

1. <span data-ttu-id="758f2-314">使用下列 Azure CLI 命令，將秘密儲存在 Azure Key Vault 中：</span><span class="sxs-lookup"><span data-stu-id="758f2-314">Secrets are saved in Azure Key Vault using the following Azure CLI commands:</span></span>

   ```azurecli
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "5000-AppSecret" --value "5.0.0.0_secret_value_prod"
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "5100-AppSecret" --value "5.1.0.0_secret_value_prod"
   ```

1. <span data-ttu-id="758f2-315">當應用程式執行時，會載入金鑰保存庫密碼。</span><span class="sxs-lookup"><span data-stu-id="758f2-315">When the app is run, the key vault secrets are loaded.</span></span> <span data-ttu-id="758f2-316">的字串秘密 `5000-AppSecret` 會符合應用程式的專案檔（）中指定的應用程式版本 `5.0.0.0` 。</span><span class="sxs-lookup"><span data-stu-id="758f2-316">The string secret for `5000-AppSecret` is matched to the app's version specified in the app's project file (`5.0.0.0`).</span></span>

1. <span data-ttu-id="758f2-317">版本 `5000` （含破折號）會從索引鍵名稱中移除。</span><span class="sxs-lookup"><span data-stu-id="758f2-317">The version, `5000` (with the dash), is stripped from the key name.</span></span> <span data-ttu-id="758f2-318">在整個應用程式中，使用金鑰來讀取設定會 `AppSecret` 載入秘密值。</span><span class="sxs-lookup"><span data-stu-id="758f2-318">Throughout the app, reading configuration with the key `AppSecret` loads the secret value.</span></span>

1. <span data-ttu-id="758f2-319">如果應用程式的版本已在專案檔中變更為 `5.1.0.0` ，且應用程式再次執行，則傳回的秘密值會 `5.1.0.0_secret_value_dev` 在開發環境和 `5.1.0.0_secret_value_prod` 生產環境中。</span><span class="sxs-lookup"><span data-stu-id="758f2-319">If the app's version is changed in the project file to `5.1.0.0` and the app is run again, the secret value returned is `5.1.0.0_secret_value_dev` in the Development environment and `5.1.0.0_secret_value_prod` in Production.</span></span>

> [!NOTE]
> <span data-ttu-id="758f2-320">您也可以將自己的 <xref:Microsoft.Azure.KeyVault.KeyVaultClient> 實作為提供給 <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> 。</span><span class="sxs-lookup"><span data-stu-id="758f2-320">You can also provide your own <xref:Microsoft.Azure.KeyVault.KeyVaultClient> implementation to <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>.</span></span> <span data-ttu-id="758f2-321">自訂用戶端允許跨應用程式共用單一用戶端實例。</span><span class="sxs-lookup"><span data-stu-id="758f2-321">A custom client permits sharing a single instance of the client across the app.</span></span>

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="758f2-322">將陣列繫結到類別</span><span class="sxs-lookup"><span data-stu-id="758f2-322">Bind an array to a class</span></span>

<span data-ttu-id="758f2-323">提供者能夠將設定值讀入陣列中，以系結至 POCO 陣列。</span><span class="sxs-lookup"><span data-stu-id="758f2-323">The provider is capable of reading configuration values into an array for binding to a POCO array.</span></span>

<span data-ttu-id="758f2-324">從允許金鑰包含冒號（）分隔符號的設定來源讀取時 `:` ，會使用數值索引鍵區段來區別組成陣列的索引鍵（ `:0:` 、 `:1:` 、 &hellip; `:{n}:` ）。</span><span class="sxs-lookup"><span data-stu-id="758f2-324">When reading from a configuration source that allows keys to contain colon (`:`) separators, a numeric key segment is used to distinguish the keys that make up an array (`:0:`, `:1:`, &hellip; `:{n}:`).</span></span> <span data-ttu-id="758f2-325">如需詳細資訊，請參閱[Configuration：將陣列系結至類別](xref:fundamentals/configuration/index#bind-an-array-to-a-class)。</span><span class="sxs-lookup"><span data-stu-id="758f2-325">For more information, see [Configuration: Bind an array to a class](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span></span>

<span data-ttu-id="758f2-326">Azure Key Vault 索引鍵不能使用冒號做為分隔符號。</span><span class="sxs-lookup"><span data-stu-id="758f2-326">Azure Key Vault keys can't use a colon as a separator.</span></span> <span data-ttu-id="758f2-327">本主題中所述的方法會使用雙虛線（ `--` ）做為階層式值的分隔符號（區段）。</span><span class="sxs-lookup"><span data-stu-id="758f2-327">The approach described in this topic uses double dashes (`--`) as a separator for hierarchical values (sections).</span></span> <span data-ttu-id="758f2-328">陣列索引鍵會以雙虛線和數位索引鍵區段（ `--0--` 、 `--1--` 、 &hellip; `--{n}--` ）儲存在 Azure Key Vault 中。</span><span class="sxs-lookup"><span data-stu-id="758f2-328">Array keys are stored in Azure Key Vault with double dashes and numeric key segments (`--0--`, `--1--`, &hellip; `--{n}--`).</span></span>

<span data-ttu-id="758f2-329">檢查 JSON 檔案所提供的下列[Serilog](https://serilog.net/)記錄提供者設定。</span><span class="sxs-lookup"><span data-stu-id="758f2-329">Examine the following [Serilog](https://serilog.net/) logging provider configuration provided by a JSON file.</span></span> <span data-ttu-id="758f2-330">陣列中定義了兩個物件常值 `WriteTo` ，以反映兩個 Serilog*接收*，其中描述記錄輸出的目的地：</span><span class="sxs-lookup"><span data-stu-id="758f2-330">There are two object literals defined in the `WriteTo` array that reflect two Serilog *sinks*, which describe destinations for logging output:</span></span>

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

<span data-ttu-id="758f2-331">先前 JSON 檔案中顯示的設定會使用雙虛線（ `--` ）標記法和數值區段儲存在 Azure Key Vault 中：</span><span class="sxs-lookup"><span data-stu-id="758f2-331">The configuration shown in the preceding JSON file is stored in Azure Key Vault using double dash (`--`) notation and numeric segments:</span></span>

| <span data-ttu-id="758f2-332">Key</span><span class="sxs-lookup"><span data-stu-id="758f2-332">Key</span></span> | <span data-ttu-id="758f2-333">值</span><span class="sxs-lookup"><span data-stu-id="758f2-333">Value</span></span> |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="reload-secrets"></a><span data-ttu-id="758f2-334">重載秘密</span><span class="sxs-lookup"><span data-stu-id="758f2-334">Reload secrets</span></span>

<span data-ttu-id="758f2-335">會快取密碼，直到 `IConfigurationRoot.Reload()` 呼叫為止。</span><span class="sxs-lookup"><span data-stu-id="758f2-335">Secrets are cached until `IConfigurationRoot.Reload()` is called.</span></span> <span data-ttu-id="758f2-336">在執行之前，應用程式不會遵守金鑰保存庫中已過期、已停用及更新的秘密 `Reload` 。</span><span class="sxs-lookup"><span data-stu-id="758f2-336">Expired, disabled, and updated secrets in the key vault are not respected by the app until `Reload` is executed.</span></span>

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a><span data-ttu-id="758f2-337">已停用和過期的秘密</span><span class="sxs-lookup"><span data-stu-id="758f2-337">Disabled and expired secrets</span></span>

<span data-ttu-id="758f2-338">已停用和過期的秘密會擲回 <xref:Microsoft.Azure.KeyVault.Models.KeyVaultErrorException> 。</span><span class="sxs-lookup"><span data-stu-id="758f2-338">Disabled and expired secrets throw a <xref:Microsoft.Azure.KeyVault.Models.KeyVaultErrorException>.</span></span> <span data-ttu-id="758f2-339">若要防止應用程式擲回，請使用不同的設定提供者來提供設定，或更新已停用或已過期的密碼。</span><span class="sxs-lookup"><span data-stu-id="758f2-339">To prevent the app from throwing, provide the configuration using a different configuration provider or update the disabled or expired secret.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="758f2-340">疑難排解</span><span class="sxs-lookup"><span data-stu-id="758f2-340">Troubleshoot</span></span>

<span data-ttu-id="758f2-341">當應用程式無法使用提供者載入設定時，會將錯誤訊息寫入[ASP.NET Core 記錄基礎結構](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="758f2-341">When the app fails to load configuration using the provider, an error message is written to the [ASP.NET Core Logging infrastructure](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="758f2-342">下列條件將導致無法載入設定：</span><span class="sxs-lookup"><span data-stu-id="758f2-342">The following conditions will prevent configuration from loading:</span></span>

* <span data-ttu-id="758f2-343">應用程式或憑證未在 Azure Active Directory 中正確設定。</span><span class="sxs-lookup"><span data-stu-id="758f2-343">The app or certificate isn't configured correctly in Azure Active Directory.</span></span>
* <span data-ttu-id="758f2-344">金鑰保存庫不存在於 Azure Key Vault 中。</span><span class="sxs-lookup"><span data-stu-id="758f2-344">The key vault doesn't exist in Azure Key Vault.</span></span>
* <span data-ttu-id="758f2-345">應用程式未獲授權，無法存取金鑰保存庫。</span><span class="sxs-lookup"><span data-stu-id="758f2-345">The app isn't authorized to access the key vault.</span></span>
* <span data-ttu-id="758f2-346">存取原則不包含 `Get` 和 `List` 許可權。</span><span class="sxs-lookup"><span data-stu-id="758f2-346">The access policy doesn't include `Get` and `List` permissions.</span></span>
* <span data-ttu-id="758f2-347">在金鑰保存庫中，設定資料（名稱/值組）未正確命名、遺失、停用或過期。</span><span class="sxs-lookup"><span data-stu-id="758f2-347">In the key vault, the configuration data (name-value pair) is incorrectly named, missing, disabled, or expired.</span></span>
* <span data-ttu-id="758f2-348">應用程式具有錯誤的金鑰保存庫名稱（ `KeyVaultName` ）、Azure AD 應用程式識別碼（ `AzureADApplicationId` ），或 Azure AD 憑證指紋（ `AzureADCertThumbprint` ）。</span><span class="sxs-lookup"><span data-stu-id="758f2-348">The app has the wrong key vault name (`KeyVaultName`), Azure AD Application Id (`AzureADApplicationId`), or Azure AD certificate thumbprint (`AzureADCertThumbprint`).</span></span>
* <span data-ttu-id="758f2-349">在應用程式中，設定金鑰（名稱）不正確，因為您嘗試載入的值。</span><span class="sxs-lookup"><span data-stu-id="758f2-349">The configuration key (name) is incorrect in the app for the value you're trying to load.</span></span>
* <span data-ttu-id="758f2-350">將應用程式的存取原則新增至金鑰保存庫時，已建立原則，但未在 [**存取原則**] UI 中選取 [**儲存**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="758f2-350">When adding the access policy for the app to the key vault, the policy was created, but the **Save** button wasn't selected in the **Access policies** UI.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="758f2-351">其他資源</span><span class="sxs-lookup"><span data-stu-id="758f2-351">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* [<span data-ttu-id="758f2-352">Microsoft Azure： Key Vault</span><span class="sxs-lookup"><span data-stu-id="758f2-352">Microsoft Azure: Key Vault</span></span>](https://azure.microsoft.com/services/key-vault/)
* [<span data-ttu-id="758f2-353">Microsoft Azure： Key Vault 檔</span><span class="sxs-lookup"><span data-stu-id="758f2-353">Microsoft Azure: Key Vault Documentation</span></span>](/azure/key-vault/)
* [<span data-ttu-id="758f2-354">如何為 Azure Key Vault 產生及傳輸受 HSM 保護的金鑰</span><span class="sxs-lookup"><span data-stu-id="758f2-354">How to generate and transfer HSM-protected keys for Azure Key Vault</span></span>](/azure/key-vault/key-vault-hsm-protected-keys)
* [<span data-ttu-id="758f2-355">KeyVaultClient 類別</span><span class="sxs-lookup"><span data-stu-id="758f2-355">KeyVaultClient Class</span></span>](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
* [<span data-ttu-id="758f2-356">快速入門：使用 .NET Web 應用程式從 Azure Key Vault 設定及擷取祕密</span><span class="sxs-lookup"><span data-stu-id="758f2-356">Quickstart: Set and retrieve a secret from Azure Key Vault by using a .NET web app</span></span>](/azure/key-vault/quick-create-net)
* [<span data-ttu-id="758f2-357">教學課程：如何在 .NET 中搭配使用 Azure Key Vault 與 Azure Windows 虛擬機器</span><span class="sxs-lookup"><span data-stu-id="758f2-357">Tutorial: How to use Azure Key Vault with Azure Windows Virtual Machine in .NET</span></span>](/azure/key-vault/tutorial-net-windows-virtual-machine)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="758f2-358">本檔說明如何使用[Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/)設定提供者，從 Azure Key Vault 秘密載入應用程式設定值。</span><span class="sxs-lookup"><span data-stu-id="758f2-358">This document explains how to use the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) Configuration Provider to load app configuration values from Azure Key Vault secrets.</span></span> <span data-ttu-id="758f2-359">Azure Key Vault 是一種雲端式服務，可協助保護應用程式和服務所使用的密碼編譯金鑰和密碼。</span><span class="sxs-lookup"><span data-stu-id="758f2-359">Azure Key Vault is a cloud-based service that assists in safeguarding cryptographic keys and secrets used by apps and services.</span></span> <span data-ttu-id="758f2-360">搭配 ASP.NET Core 應用程式使用 Azure Key Vault 的常見案例包括：</span><span class="sxs-lookup"><span data-stu-id="758f2-360">Common scenarios for using Azure Key Vault with ASP.NET Core apps include:</span></span>

* <span data-ttu-id="758f2-361">控制敏感性設定資料的存取權。</span><span class="sxs-lookup"><span data-stu-id="758f2-361">Controlling access to sensitive configuration data.</span></span>
* <span data-ttu-id="758f2-362">當儲存設定資料時，符合 FIPS 140-2 Level 2 驗證的硬體安全性模組（HSM）的需求。</span><span class="sxs-lookup"><span data-stu-id="758f2-362">Meeting the requirement for FIPS 140-2 Level 2 validated Hardware Security Modules (HSM's) when storing configuration data.</span></span>

<span data-ttu-id="758f2-363">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="758f2-363">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="packages"></a><span data-ttu-id="758f2-364">套件</span><span class="sxs-lookup"><span data-stu-id="758f2-364">Packages</span></span>

<span data-ttu-id="758f2-365">將套件參考新增至[AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/)套件。</span><span class="sxs-lookup"><span data-stu-id="758f2-365">Add a package reference to the [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) package.</span></span>

## <a name="sample-app"></a><span data-ttu-id="758f2-366">範例應用程式</span><span class="sxs-lookup"><span data-stu-id="758f2-366">Sample app</span></span>

<span data-ttu-id="758f2-367">範例應用程式會以 Program.cs 檔案頂端的語句所決定的兩種模式之一來執行 `#define` ： *Program.cs*</span><span class="sxs-lookup"><span data-stu-id="758f2-367">The sample app runs in either of two modes determined by the `#define` statement at the top of the *Program.cs* file:</span></span>

* <span data-ttu-id="758f2-368">`Certificate`：示範如何使用 Azure Key Vault 的用戶端識別碼和 x.509 憑證，來存取儲存在 Azure Key Vault 中的秘密。</span><span class="sxs-lookup"><span data-stu-id="758f2-368">`Certificate`: Demonstrates the use of an Azure Key Vault Client ID and X.509 certificate to access secrets stored in Azure Key Vault.</span></span> <span data-ttu-id="758f2-369">這個版本的範例可以從任何位置執行，部署至 Azure App Service 或任何能夠提供 ASP.NET Core 應用程式的主機。</span><span class="sxs-lookup"><span data-stu-id="758f2-369">This version of the sample can be run from any location, deployed to Azure App Service or any host capable of serving an ASP.NET Core app.</span></span>
* <span data-ttu-id="758f2-370">`Managed`：示範如何使用[適用于 Azure 資源的受控](/azure/active-directory/managed-identities-azure-resources/overview)識別來驗證應用程式，以在未儲存于應用程式代碼或設定中的認證 Azure AD 驗證 Azure Key Vault。</span><span class="sxs-lookup"><span data-stu-id="758f2-370">`Managed`: Demonstrates how to use [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview) to authenticate the app to Azure Key Vault with Azure AD authentication without credentials stored in the app's code or configuration.</span></span> <span data-ttu-id="758f2-371">使用受控識別進行驗證時，不需要 Azure AD 應用程式識別碼和密碼（用戶端密碼）。</span><span class="sxs-lookup"><span data-stu-id="758f2-371">When using managed identities to authenticate, an Azure AD Application ID and Password (Client Secret) aren't required.</span></span> <span data-ttu-id="758f2-372">`Managed`範例的版本必須部署至 Azure。</span><span class="sxs-lookup"><span data-stu-id="758f2-372">The `Managed` version of the sample must be deployed to Azure.</span></span> <span data-ttu-id="758f2-373">請遵循[使用適用于 Azure 資源的受控](#use-managed-identities-for-azure-resources)識別一節中的指導方針。</span><span class="sxs-lookup"><span data-stu-id="758f2-373">Follow the guidance in the [Use the Managed identities for Azure resources](#use-managed-identities-for-azure-resources) section.</span></span>

<span data-ttu-id="758f2-374">如需有關如何使用預處理器指示詞（）來設定範例應用程式的詳細資訊 `#define` ，請參閱 <xref:index#preprocessor-directives-in-sample-code> 。</span><span class="sxs-lookup"><span data-stu-id="758f2-374">For more information on how to configure a sample app using preprocessor directives (`#define`), see <xref:index#preprocessor-directives-in-sample-code>.</span></span>

## <a name="secret-storage-in-the-development-environment"></a><span data-ttu-id="758f2-375">開發環境中的秘密儲存</span><span class="sxs-lookup"><span data-stu-id="758f2-375">Secret storage in the Development environment</span></span>

<span data-ttu-id="758f2-376">使用[秘密管理員工具](xref:security/app-secrets)在本機設定秘密。</span><span class="sxs-lookup"><span data-stu-id="758f2-376">Set secrets locally using the [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="758f2-377">當範例應用程式在開發環境中的本機電腦上執行時，會從本機密碼管理員存放區載入秘密。</span><span class="sxs-lookup"><span data-stu-id="758f2-377">When the sample app runs on the local machine in the Development environment, secrets are loaded from the local Secret Manager store.</span></span>

<span data-ttu-id="758f2-378">「密碼管理員」工具需要 `<UserSecretsId>` 應用程式專案檔中的屬性。</span><span class="sxs-lookup"><span data-stu-id="758f2-378">The Secret Manager tool requires a `<UserSecretsId>` property in the app's project file.</span></span> <span data-ttu-id="758f2-379">將屬性值（ `{GUID}` ）設定為任何唯一的 GUID：</span><span class="sxs-lookup"><span data-stu-id="758f2-379">Set the property value (`{GUID}`) to any unique GUID:</span></span>

```xml
<PropertyGroup>
  <UserSecretsId>{GUID}</UserSecretsId>
</PropertyGroup>
```

<span data-ttu-id="758f2-380">密碼會以名稱/值組的形式建立。</span><span class="sxs-lookup"><span data-stu-id="758f2-380">Secrets are created as name-value pairs.</span></span> <span data-ttu-id="758f2-381">階層式值（設定區段）使用 `:` （冒號）做為[ASP.NET Core](xref:fundamentals/configuration/index)設定機碼名稱中的分隔符號。</span><span class="sxs-lookup"><span data-stu-id="758f2-381">Hierarchical values (configuration sections) use a `:` (colon) as a separator in [ASP.NET Core configuration](xref:fundamentals/configuration/index) key names.</span></span>

<span data-ttu-id="758f2-382">密碼管理員會從開啟的命令 shell 使用到專案的[內容根目錄](xref:fundamentals/index#content-root)，其中 `{SECRET NAME}` 是名稱，而 `{SECRET VALUE}` 是值：</span><span class="sxs-lookup"><span data-stu-id="758f2-382">The Secret Manager is used from a command shell opened to the project's [content root](xref:fundamentals/index#content-root), where `{SECRET NAME}` is the name and `{SECRET VALUE}` is the value:</span></span>

```dotnetcli
dotnet user-secrets set "{SECRET NAME}" "{SECRET VALUE}"
```

<span data-ttu-id="758f2-383">在命令 shell 中，從專案的[內容根目錄](xref:fundamentals/index#content-root)執行下列命令，以設定範例應用程式的秘密：</span><span class="sxs-lookup"><span data-stu-id="758f2-383">Execute the following commands in a command shell from the project's [content root](xref:fundamentals/index#content-root) to set the secrets for the sample app:</span></span>

```dotnetcli
dotnet user-secrets set "SecretName" "secret_value_1_dev"
dotnet user-secrets set "Section:SecretName" "secret_value_2_dev"
```

<span data-ttu-id="758f2-384">當這些秘密儲存在具有 Azure Key Vault 區段之[生產環境中的秘密儲存](#secret-storage-in-the-production-environment-with-azure-key-vault)Azure Key Vault 中時， `_dev` 尾碼會變更為 `_prod` 。</span><span class="sxs-lookup"><span data-stu-id="758f2-384">When these secrets are stored in Azure Key Vault in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section, the `_dev` suffix is changed to `_prod`.</span></span> <span data-ttu-id="758f2-385">尾碼會在應用程式的輸出中提供視覺提示，指出設定值的來源。</span><span class="sxs-lookup"><span data-stu-id="758f2-385">The suffix provides a visual cue in the app's output indicating the source of the configuration values.</span></span>

## <a name="secret-storage-in-the-production-environment-with-azure-key-vault"></a><span data-ttu-id="758f2-386">在生產環境中使用 Azure Key Vault 的秘密儲存</span><span class="sxs-lookup"><span data-stu-id="758f2-386">Secret storage in the Production environment with Azure Key Vault</span></span>

<span data-ttu-id="758f2-387">以下摘要說明[快速入門：從 Azure Key Vault 使用 Azure CLI 主題設定和取出秘密](/azure/key-vault/quick-create-cli)，以建立 Azure Key Vault 並儲存範例應用程式所使用的秘密。</span><span class="sxs-lookup"><span data-stu-id="758f2-387">The instructions provided by the [Quickstart: Set and retrieve a secret from Azure Key Vault using Azure CLI](/azure/key-vault/quick-create-cli) topic are summarized here for creating an Azure Key Vault and storing secrets used by the sample app.</span></span> <span data-ttu-id="758f2-388">如需進一步的詳細資料，請參閱主題。</span><span class="sxs-lookup"><span data-stu-id="758f2-388">Refer to the topic for further details.</span></span>

1. <span data-ttu-id="758f2-389">使用下列其中一種方法，在[Azure 入口網站](https://portal.azure.com/)中開啟 Azure Cloud shell：</span><span class="sxs-lookup"><span data-stu-id="758f2-389">Open Azure Cloud shell using any one of the following methods in the [Azure portal](https://portal.azure.com/):</span></span>

   * <span data-ttu-id="758f2-390">選取程式碼區塊右上角的 [試試看]  。</span><span class="sxs-lookup"><span data-stu-id="758f2-390">Select **Try It** in the upper-right corner of a code block.</span></span> <span data-ttu-id="758f2-391">在文字方塊中使用搜尋字串 "Azure CLI"。</span><span class="sxs-lookup"><span data-stu-id="758f2-391">Use the search string "Azure CLI" in the text box.</span></span>
   * <span data-ttu-id="758f2-392">使用 [**啟動 Cloud Shell** ] 按鈕，在瀏覽器中開啟 Cloud Shell。</span><span class="sxs-lookup"><span data-stu-id="758f2-392">Open Cloud Shell in your browser with the **Launch Cloud Shell** button.</span></span>
   * <span data-ttu-id="758f2-393">選取 Azure 入口網站右上角功能表上的 **[Cloud Shell]** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="758f2-393">Select the **Cloud Shell** button on the menu in the upper-right corner of the Azure portal.</span></span>

   <span data-ttu-id="758f2-394">如需詳細資訊，請參閱[Azure Cloud Shell 的](/azure/cloud-shell/overview) [Azure CLI](/cli/azure/)和總覽。</span><span class="sxs-lookup"><span data-stu-id="758f2-394">For more information, see [Azure CLI](/cli/azure/) and [Overview of Azure Cloud Shell](/azure/cloud-shell/overview).</span></span>

1. <span data-ttu-id="758f2-395">如果您尚未驗證，請使用命令登入 `az login` 。</span><span class="sxs-lookup"><span data-stu-id="758f2-395">If you aren't already authenticated, sign in with the `az login` command.</span></span>

1. <span data-ttu-id="758f2-396">使用下列命令建立資源群組，其中 `{RESOURCE GROUP NAME}` 是新資源群組的資源組名，而 `{LOCATION}` 是 Azure 區域（datacenter）：</span><span class="sxs-lookup"><span data-stu-id="758f2-396">Create a resource group with the following command, where `{RESOURCE GROUP NAME}` is the resource group name for the new resource group and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```azurecli
   az group create --name "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="758f2-397">使用下列命令在資源群組中建立 key vault，其中 `{KEY VAULT NAME}` 是新金鑰保存庫的名稱，而 `{LOCATION}` 是 Azure 區域（datacenter）：</span><span class="sxs-lookup"><span data-stu-id="758f2-397">Create a key vault in the resource group with the following command, where `{KEY VAULT NAME}` is the name for the new key vault and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```azurecli
   az keyvault create --name {KEY VAULT NAME} --resource-group "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="758f2-398">在金鑰保存庫中建立秘密，做為名稱/值配對。</span><span class="sxs-lookup"><span data-stu-id="758f2-398">Create secrets in the key vault as name-value pairs.</span></span>

   <span data-ttu-id="758f2-399">Azure Key Vault 秘密名稱僅限於英數位元和連字號。</span><span class="sxs-lookup"><span data-stu-id="758f2-399">Azure Key Vault secret names are limited to alphanumeric characters and dashes.</span></span> <span data-ttu-id="758f2-400">階層式值（設定區段）使用 `--` （兩個虛線）做為分隔符號。</span><span class="sxs-lookup"><span data-stu-id="758f2-400">Hierarchical values (configuration sections) use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="758f2-401">冒號，通常用來從[ASP.NET Core](xref:fundamentals/configuration/index)設定中的子機碼分隔區段，但不允許用在金鑰保存庫密碼名稱中。</span><span class="sxs-lookup"><span data-stu-id="758f2-401">Colons, which are normally used to delimit a section from a subkey in [ASP.NET Core configuration](xref:fundamentals/configuration/index), aren't allowed in key vault secret names.</span></span> <span data-ttu-id="758f2-402">因此，當密碼載入應用程式的設定時，會使用兩個破折號並交換冒號。</span><span class="sxs-lookup"><span data-stu-id="758f2-402">Therefore, two dashes are used and swapped for a colon when the secrets are loaded into the app's configuration.</span></span>

   <span data-ttu-id="758f2-403">下列秘密可用於範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="758f2-403">The following secrets are for use with the sample app.</span></span> <span data-ttu-id="758f2-404">這些值包含後置詞 `_prod` ，以區別它們與 `_dev` 開發環境中從使用者秘密載入的尾碼值。</span><span class="sxs-lookup"><span data-stu-id="758f2-404">The values include a `_prod` suffix to distinguish them from the `_dev` suffix values loaded in the Development environment from User Secrets.</span></span> <span data-ttu-id="758f2-405">將取代 `{KEY VAULT NAME}` 為您在上一個步驟中建立的金鑰保存庫名稱：</span><span class="sxs-lookup"><span data-stu-id="758f2-405">Replace `{KEY VAULT NAME}` with the name of the key vault that you created in the prior step:</span></span>

   ```azurecli
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "SecretName" --value "secret_value_1_prod"
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "Section--SecretName" --value "secret_value_2_prod"
   ```

## <a name="use-application-id-and-x509-certificate-for-non-azure-hosted-apps"></a><span data-ttu-id="758f2-406">將應用程式識別碼和 x.509 憑證用於非 Azure 託管的應用程式</span><span class="sxs-lookup"><span data-stu-id="758f2-406">Use Application ID and X.509 certificate for non-Azure-hosted apps</span></span>

<span data-ttu-id="758f2-407">設定 Azure AD、Azure Key Vault 和應用程式，以在應用程式裝載于**Azure 外部時**，使用 Azure Active Directory 應用程式識別碼和 x.509 憑證來驗證金鑰保存庫。</span><span class="sxs-lookup"><span data-stu-id="758f2-407">Configure Azure AD, Azure Key Vault, and the app to use an Azure Active Directory Application ID and X.509 certificate to authenticate to a key vault **when the app is hosted outside of Azure**.</span></span> <span data-ttu-id="758f2-408">如需詳細資訊，請參閱[關於金鑰、祕密和憑證](/azure/key-vault/about-keys-secrets-and-certificates)。</span><span class="sxs-lookup"><span data-stu-id="758f2-408">For more information, see [About keys, secrets, and certificates](/azure/key-vault/about-keys-secrets-and-certificates).</span></span>

> [!NOTE]
> <span data-ttu-id="758f2-409">雖然 Azure 中裝載的應用程式支援使用應用程式識別碼和 x.509 憑證，但建議您在 Azure 中裝載應用程式時，使用[適用于 azure 資源的受控](#use-managed-identities-for-azure-resources)識別。</span><span class="sxs-lookup"><span data-stu-id="758f2-409">Although using an Application ID and X.509 certificate is supported for apps hosted in Azure, we recommend using [Managed identities for Azure resources](#use-managed-identities-for-azure-resources) when hosting an app in Azure.</span></span> <span data-ttu-id="758f2-410">受控識別不需要在應用程式中或在開發環境中儲存憑證。</span><span class="sxs-lookup"><span data-stu-id="758f2-410">Managed identities don't require storing a certificate in the app or in the development environment.</span></span>

<span data-ttu-id="758f2-411">當 `#define` *Program.cs*檔案頂端的語句設定為時，範例應用程式會使用應用程式識別碼和 x.509 憑證 `Certificate` 。</span><span class="sxs-lookup"><span data-stu-id="758f2-411">The sample app uses an Application ID and X.509 certificate when the `#define` statement at the top of the *Program.cs* file is set to `Certificate`.</span></span>

1. <span data-ttu-id="758f2-412">建立 PKCS # 12 封存檔案（*.pfx*）憑證。</span><span class="sxs-lookup"><span data-stu-id="758f2-412">Create a PKCS#12 archive (*.pfx*) certificate.</span></span> <span data-ttu-id="758f2-413">建立憑證的選項包括[Windows](/windows/desktop/seccrypto/makecert)和[OpenSSL](https://www.openssl.org/)上的 MakeCert。</span><span class="sxs-lookup"><span data-stu-id="758f2-413">Options for creating certificates include [MakeCert on Windows](/windows/desktop/seccrypto/makecert) and [OpenSSL](https://www.openssl.org/).</span></span>
1. <span data-ttu-id="758f2-414">將憑證安裝到目前使用者的個人憑證存儲。</span><span class="sxs-lookup"><span data-stu-id="758f2-414">Install the certificate into the current user's personal certificate store.</span></span> <span data-ttu-id="758f2-415">將金鑰標示為可匯出是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="758f2-415">Marking the key as exportable is optional.</span></span> <span data-ttu-id="758f2-416">記下憑證的指紋，這會在此程式稍後使用。</span><span class="sxs-lookup"><span data-stu-id="758f2-416">Note the certificate's thumbprint, which is used later in this process.</span></span>
1. <span data-ttu-id="758f2-417">將 PKCS # 12 封存（*.pfx*）憑證匯出為 DER 編碼憑證（*.cer*）。</span><span class="sxs-lookup"><span data-stu-id="758f2-417">Export the PKCS#12 archive (*.pfx*) certificate as a DER-encoded certificate (*.cer*).</span></span>
1. <span data-ttu-id="758f2-418">使用 Azure AD （**應用程式註冊**）註冊應用程式。</span><span class="sxs-lookup"><span data-stu-id="758f2-418">Register the app with Azure AD (**App registrations**).</span></span>
1. <span data-ttu-id="758f2-419">將 DER 編碼的憑證（*.cer*）上傳至 Azure AD：</span><span class="sxs-lookup"><span data-stu-id="758f2-419">Upload the DER-encoded certificate (*.cer*) to Azure AD:</span></span>
   1. <span data-ttu-id="758f2-420">在 Azure AD 中選取應用程式。</span><span class="sxs-lookup"><span data-stu-id="758f2-420">Select the app in Azure AD.</span></span>
   1. <span data-ttu-id="758f2-421">流覽至 [**憑證 & 密碼**]。</span><span class="sxs-lookup"><span data-stu-id="758f2-421">Navigate to **Certificates & secrets**.</span></span>
   1. <span data-ttu-id="758f2-422">選取 [**上傳憑證**] 來上傳包含公開金鑰的憑證。</span><span class="sxs-lookup"><span data-stu-id="758f2-422">Select **Upload certificate** to upload the certificate, which contains the public key.</span></span> <span data-ttu-id="758f2-423">*.Cer*、 *pem*或 *.crt*憑證是可接受的。</span><span class="sxs-lookup"><span data-stu-id="758f2-423">A *.cer*, *.pem*, or *.crt* certificate is acceptable.</span></span>
1. <span data-ttu-id="758f2-424">將金鑰保存庫名稱、應用程式識別碼和憑證指紋儲存在應用程式的*appsettings*中。</span><span class="sxs-lookup"><span data-stu-id="758f2-424">Store the key vault name, Application ID, and certificate thumbprint in the app's *appsettings.json* file.</span></span>
1. <span data-ttu-id="758f2-425">流覽至 Azure 入口網站中的 [**金鑰保存庫**]。</span><span class="sxs-lookup"><span data-stu-id="758f2-425">Navigate to **Key vaults** in the Azure portal.</span></span>
1. <span data-ttu-id="758f2-426">選取您在[生產環境中使用 Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault)一節所建立的金鑰保存庫。</span><span class="sxs-lookup"><span data-stu-id="758f2-426">Select the key vault that you created in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section.</span></span>
1. <span data-ttu-id="758f2-427">選取 [**存取原則**]。</span><span class="sxs-lookup"><span data-stu-id="758f2-427">Select **Access policies**.</span></span>
1. <span data-ttu-id="758f2-428">選取 [**新增存取原則**]。</span><span class="sxs-lookup"><span data-stu-id="758f2-428">Select **Add Access Policy**.</span></span>
1. <span data-ttu-id="758f2-429">開啟 [**秘密許可權**]，並提供具有 [**取得**] 和 [**列出**] 許可權的應用程式。</span><span class="sxs-lookup"><span data-stu-id="758f2-429">Open **Secret permissions** and provide the app with **Get** and **List** permissions.</span></span>
1. <span data-ttu-id="758f2-430">選取 [**選取主體**]，然後依名稱選取已註冊的應用程式。</span><span class="sxs-lookup"><span data-stu-id="758f2-430">Select **Select principal** and select the registered app by name.</span></span> <span data-ttu-id="758f2-431">選取 [選取]  按鈕。</span><span class="sxs-lookup"><span data-stu-id="758f2-431">Select the **Select** button.</span></span>
1. <span data-ttu-id="758f2-432">選取 [確定]  。</span><span class="sxs-lookup"><span data-stu-id="758f2-432">Select **OK**.</span></span>
1. <span data-ttu-id="758f2-433">選取 [儲存]  。</span><span class="sxs-lookup"><span data-stu-id="758f2-433">Select **Save**.</span></span>
1. <span data-ttu-id="758f2-434">部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="758f2-434">Deploy the app.</span></span>

<span data-ttu-id="758f2-435">`Certificate`範例應用程式會從 `IConfigurationRoot` 使用與秘密名稱相同的名稱取得其設定值：</span><span class="sxs-lookup"><span data-stu-id="758f2-435">The `Certificate` sample app obtains its configuration values from `IConfigurationRoot` with the same name as the secret name:</span></span>

* <span data-ttu-id="758f2-436">非階層式值：的值 `SecretName` 是使用取得 `config["SecretName"]` 。</span><span class="sxs-lookup"><span data-stu-id="758f2-436">Non-hierarchical values: The value for `SecretName` is obtained with `config["SecretName"]`.</span></span>
* <span data-ttu-id="758f2-437">階層式值（區段）：使用 `:` （冒號）標記法或 `GetSection` 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="758f2-437">Hierarchical values (sections): Use `:` (colon) notation or the `GetSection` extension method.</span></span> <span data-ttu-id="758f2-438">使用下列其中一種方法來取得設定值：</span><span class="sxs-lookup"><span data-stu-id="758f2-438">Use either of these approaches to obtain the configuration value:</span></span>
  * `config["Section:SecretName"]`
  * `config.GetSection("Section")["SecretName"]`

<span data-ttu-id="758f2-439">X.509 憑證是由作業系統所管理。</span><span class="sxs-lookup"><span data-stu-id="758f2-439">The X.509 certificate is managed by the OS.</span></span> <span data-ttu-id="758f2-440">應用程式會 <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> 使用*appsettings*所提供的值來呼叫：</span><span class="sxs-lookup"><span data-stu-id="758f2-440">The app calls <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> with values supplied by the *appsettings.json* file:</span></span>

[!code-csharp[](key-vault-configuration/samples/2.x/SampleApp/Program.cs?name=snippet1&highlight=20-23)]

<span data-ttu-id="758f2-441">範例值：</span><span class="sxs-lookup"><span data-stu-id="758f2-441">Example values:</span></span>

* <span data-ttu-id="758f2-442">金鑰保存庫名稱：`contosovault`</span><span class="sxs-lookup"><span data-stu-id="758f2-442">Key vault name: `contosovault`</span></span>
* <span data-ttu-id="758f2-443">應用程式識別碼：`627e911e-43cc-61d4-992e-12db9c81b413`</span><span class="sxs-lookup"><span data-stu-id="758f2-443">Application ID: `627e911e-43cc-61d4-992e-12db9c81b413`</span></span>
* <span data-ttu-id="758f2-444">憑證指紋：`fe14593dd66b2406c5269d742d04b6e1ab03adb1`</span><span class="sxs-lookup"><span data-stu-id="758f2-444">Certificate thumbprint: `fe14593dd66b2406c5269d742d04b6e1ab03adb1`</span></span>

<span data-ttu-id="758f2-445">*appsettings. json*：</span><span class="sxs-lookup"><span data-stu-id="758f2-445">*appsettings.json*:</span></span>

[!code-json[](key-vault-configuration/samples/2.x/SampleApp/appsettings.json?highlight=10-12)]

<span data-ttu-id="758f2-446">當您執行應用程式時，網頁會顯示已載入的密碼值。</span><span class="sxs-lookup"><span data-stu-id="758f2-446">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="758f2-447">在開發環境中，秘密值會以 `_dev` 尾碼載入。</span><span class="sxs-lookup"><span data-stu-id="758f2-447">In the Development environment, secret values load with the `_dev` suffix.</span></span> <span data-ttu-id="758f2-448">在生產環境中，值會以後綴載入 `_prod` 。</span><span class="sxs-lookup"><span data-stu-id="758f2-448">In the Production environment, the values load with the `_prod` suffix.</span></span>

## <a name="use-managed-identities-for-azure-resources"></a><span data-ttu-id="758f2-449">使用適用于 Azure 資源的受控識別</span><span class="sxs-lookup"><span data-stu-id="758f2-449">Use Managed identities for Azure resources</span></span>

<span data-ttu-id="758f2-450">**部署至 azure 的應用程式**可以利用[Azure 資源的受控](/azure/active-directory/managed-identities-azure-resources/overview)識別，讓應用程式使用 Azure AD 驗證，而不需要在應用程式中儲存認證（應用程式識別碼和密碼/用戶端密碼）來驗證 Azure Key Vault。</span><span class="sxs-lookup"><span data-stu-id="758f2-450">**An app deployed to Azure** can take advantage of [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview), which allows the app to authenticate with Azure Key Vault using Azure AD authentication without credentials (Application ID and Password/Client Secret) stored in the app.</span></span>

<span data-ttu-id="758f2-451">當 `#define` *Program.cs*檔案頂端的語句設定為時，範例應用程式會使用適用于 Azure 資源的受控識別 `Managed` 。</span><span class="sxs-lookup"><span data-stu-id="758f2-451">The sample app uses Managed identities for Azure resources when the `#define` statement at the top of the *Program.cs* file is set to `Managed`.</span></span>

<span data-ttu-id="758f2-452">在應用程式的*appsettings*中輸入保存庫名稱。</span><span class="sxs-lookup"><span data-stu-id="758f2-452">Enter the vault name into the app's *appsettings.json* file.</span></span> <span data-ttu-id="758f2-453">在設定為版本時，範例應用程式不需要應用程式識別碼和密碼（用戶端密碼） `Managed` ，因此您可以忽略這些設定專案。</span><span class="sxs-lookup"><span data-stu-id="758f2-453">The sample app doesn't require an Application ID and Password (Client Secret) when set to the `Managed` version, so you can ignore those configuration entries.</span></span> <span data-ttu-id="758f2-454">應用程式會部署至 Azure，而 Azure 只會使用儲存在*appsettings*中的保存庫名稱來驗證應用程式，以存取 Azure Key Vault。</span><span class="sxs-lookup"><span data-stu-id="758f2-454">The app is deployed to Azure, and Azure authenticates the app to access Azure Key Vault only using the vault name stored in the *appsettings.json* file.</span></span>

<span data-ttu-id="758f2-455">將範例應用程式部署至 Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="758f2-455">Deploy the sample app to Azure App Service.</span></span>

<span data-ttu-id="758f2-456">部署到 Azure App Service 的應用程式會在建立服務時，自動向 Azure AD 註冊。</span><span class="sxs-lookup"><span data-stu-id="758f2-456">An app deployed to Azure App Service is automatically registered with Azure AD when the service is created.</span></span> <span data-ttu-id="758f2-457">從部署取得物件識別碼，以便在下列命令中使用。</span><span class="sxs-lookup"><span data-stu-id="758f2-457">Obtain the Object ID from the deployment for use in the following command.</span></span> <span data-ttu-id="758f2-458">物件識別碼會顯示在 App Service 面板的 Azure 入口網站中 **Identity** 。</span><span class="sxs-lookup"><span data-stu-id="758f2-458">The Object ID is shown in the Azure portal on the **Identity** panel of the App Service.</span></span>

<span data-ttu-id="758f2-459">使用 Azure CLI 和應用程式的物件識別碼，提供應用程式 `list` 和 `get` 存取金鑰保存庫的許可權：</span><span class="sxs-lookup"><span data-stu-id="758f2-459">Using Azure CLI and the app's Object ID, provide the app with `list` and `get` permissions to access the key vault:</span></span>

```azurecli
az keyvault set-policy --name {KEY VAULT NAME} --object-id {OBJECT ID} --secret-permissions get list
```

<span data-ttu-id="758f2-460">使用 Azure CLI、PowerShell 或 Azure 入口網站**重新開機應用程式**。</span><span class="sxs-lookup"><span data-stu-id="758f2-460">**Restart the app** using Azure CLI, PowerShell, or the Azure portal.</span></span>

<span data-ttu-id="758f2-461">範例應用程式：</span><span class="sxs-lookup"><span data-stu-id="758f2-461">The sample app:</span></span>

* <span data-ttu-id="758f2-462">建立 `AzureServiceTokenProvider` 不含連接字串之類別的實例。</span><span class="sxs-lookup"><span data-stu-id="758f2-462">Creates an instance of the `AzureServiceTokenProvider` class without a connection string.</span></span> <span data-ttu-id="758f2-463">未提供連接字串時，提供者會嘗試從 Azure 資源的受控識別取得存取權杖。</span><span class="sxs-lookup"><span data-stu-id="758f2-463">When a connection string isn't provided, the provider attempts to obtain an access token from Managed identities for Azure resources.</span></span>
* <span data-ttu-id="758f2-464">新的 <xref:Microsoft.Azure.KeyVault.KeyVaultClient> 會使用 `AzureServiceTokenProvider` 實例 token 回呼來建立。</span><span class="sxs-lookup"><span data-stu-id="758f2-464">A new <xref:Microsoft.Azure.KeyVault.KeyVaultClient> is created with the `AzureServiceTokenProvider` instance token callback.</span></span>
* <span data-ttu-id="758f2-465"><xref:Microsoft.Azure.KeyVault.KeyVaultClient>實例是與的預設執行搭配使用 <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> ，它會載入所有秘密值，並 `--` 以冒號（）取代索引 `:` 鍵名稱中的雙破折號（）。</span><span class="sxs-lookup"><span data-stu-id="758f2-465">The <xref:Microsoft.Azure.KeyVault.KeyVaultClient> instance is used with a default implementation of <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> that loads all secret values and replaces double-dashes (`--`) with colons (`:`) in key names.</span></span>

[!code-csharp[](key-vault-configuration/samples/2.x/SampleApp/Program.cs?name=snippet2&highlight=13-21)]

<span data-ttu-id="758f2-466">金鑰保存庫名稱範例值：`contosovault`</span><span class="sxs-lookup"><span data-stu-id="758f2-466">Key vault name example value: `contosovault`</span></span>
    
<span data-ttu-id="758f2-467">*appsettings. json*：</span><span class="sxs-lookup"><span data-stu-id="758f2-467">*appsettings.json*:</span></span>

```json
{
  "KeyVaultName": "Key Vault Name"
}
```

<span data-ttu-id="758f2-468">當您執行應用程式時，網頁會顯示已載入的密碼值。</span><span class="sxs-lookup"><span data-stu-id="758f2-468">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="758f2-469">在開發環境中，秘密值具有 `_dev` 尾碼，因為它們是由使用者密碼提供。</span><span class="sxs-lookup"><span data-stu-id="758f2-469">In the Development environment, secret values have the `_dev` suffix because they're provided by User Secrets.</span></span> <span data-ttu-id="758f2-470">在生產環境中，值是以後綴載入， `_prod` 因為它們是由 Azure Key Vault 所提供。</span><span class="sxs-lookup"><span data-stu-id="758f2-470">In the Production environment, the values load with the `_prod` suffix because they're provided by Azure Key Vault.</span></span>

<span data-ttu-id="758f2-471">如果您收到 `Access denied` 錯誤，請確認已向 Azure AD 註冊應用程式，並提供金鑰保存庫的存取權。</span><span class="sxs-lookup"><span data-stu-id="758f2-471">If you receive an `Access denied` error, confirm that the app is registered with Azure AD and provided access to the key vault.</span></span> <span data-ttu-id="758f2-472">確認您已在 Azure 中重新開機服務。</span><span class="sxs-lookup"><span data-stu-id="758f2-472">Confirm that you've restarted the service in Azure.</span></span>

<span data-ttu-id="758f2-473">如需將提供者與受控識別和 Azure DevOps 管線搭配使用的詳細資訊，請參閱使用[受控服務識別建立 VM 的 Azure Resource Manager 服務](/azure/devops/pipelines/library/connect-to-azure#create-an-azure-resource-manager-service-connection-to-a-vm-with-a-managed-service-identity)連線。</span><span class="sxs-lookup"><span data-stu-id="758f2-473">For information on using the provider with a managed identity and an Azure DevOps pipeline, see [Create an Azure Resource Manager service connection to a VM with a managed service identity](/azure/devops/pipelines/library/connect-to-azure#create-an-azure-resource-manager-service-connection-to-a-vm-with-a-managed-service-identity).</span></span>

## <a name="use-a-key-name-prefix"></a><span data-ttu-id="758f2-474">使用索引鍵名稱前置詞</span><span class="sxs-lookup"><span data-stu-id="758f2-474">Use a key name prefix</span></span>

<span data-ttu-id="758f2-475"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>提供接受的執行的多載 <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> ，可讓您控制如何將金鑰保存庫密碼轉換成設定金鑰。</span><span class="sxs-lookup"><span data-stu-id="758f2-475"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> provides an overload that accepts an implementation of <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>, which allows you to control how key vault secrets are converted into configuration keys.</span></span> <span data-ttu-id="758f2-476">例如，您可以根據您在應用程式啟動時提供的首碼值，執行介面來載入密碼值。</span><span class="sxs-lookup"><span data-stu-id="758f2-476">For example, you can implement the interface to load secret values based on a prefix value you provide at app startup.</span></span> <span data-ttu-id="758f2-477">例如，這可讓您根據應用程式的版本來載入密碼。</span><span class="sxs-lookup"><span data-stu-id="758f2-477">This allows you, for example, to load secrets based on the version of the app.</span></span>

> [!WARNING]
> <span data-ttu-id="758f2-478">請勿在金鑰保存庫秘密上使用前置詞，將多個應用程式的秘密放入相同的金鑰保存庫，或將環境秘密（例如*開發*與*生產*密碼）放入相同的保存庫中。</span><span class="sxs-lookup"><span data-stu-id="758f2-478">Don't use prefixes on key vault secrets to place secrets for multiple apps into the same key vault or to place environmental secrets (for example, *development* versus *production* secrets) into the same vault.</span></span> <span data-ttu-id="758f2-479">我們建議不同的應用程式和開發/生產環境使用不同的金鑰保存庫，以隔離最高安全性層級的應用程式環境。</span><span class="sxs-lookup"><span data-stu-id="758f2-479">We recommend that different apps and development/production environments use separate key vaults to isolate app environments for the highest level of security.</span></span>

<span data-ttu-id="758f2-480">在下列範例中，會在金鑰保存庫中建立秘密（並使用適用于開發環境的秘密管理員工具）來進行 `5000-AppSecret` （金鑰保存庫密碼名稱中不允許週期）。</span><span class="sxs-lookup"><span data-stu-id="758f2-480">In the following example, a secret is established in the key vault (and using the Secret Manager tool for the Development environment) for `5000-AppSecret` (periods aren't allowed in key vault secret names).</span></span> <span data-ttu-id="758f2-481">此秘密代表應用程式版本5.0.0.0 的應用程式密碼。</span><span class="sxs-lookup"><span data-stu-id="758f2-481">This secret represents an app secret for version 5.0.0.0 of the app.</span></span> <span data-ttu-id="758f2-482">針對其他版本的應用程式（5.1.0.0），會將密碼新增至金鑰保存庫（並使用密碼管理員工具）來進行 `5100-AppSecret` 。</span><span class="sxs-lookup"><span data-stu-id="758f2-482">For another version of the app, 5.1.0.0, a secret is added to the key vault (and using the Secret Manager tool) for `5100-AppSecret`.</span></span> <span data-ttu-id="758f2-483">每個應用程式版本都會將其版本設定的秘密值載入至其設定中 `AppSecret` ，因為它會在載入秘密時去除版本。</span><span class="sxs-lookup"><span data-stu-id="758f2-483">Each app version loads its versioned secret value into its configuration as `AppSecret`, stripping off the version as it loads the secret.</span></span>

<span data-ttu-id="758f2-484"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>使用自訂進行呼叫 <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> ：</span><span class="sxs-lookup"><span data-stu-id="758f2-484"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> is called with a custom <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>:</span></span>

[!code-csharp[](key-vault-configuration/samples_snapshot/Program.cs)]

<span data-ttu-id="758f2-485">執行動作會 <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> 回應秘密的版本前置詞，以將適當的密碼載入設定中：</span><span class="sxs-lookup"><span data-stu-id="758f2-485">The <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> implementation reacts to the version prefixes of secrets to load the proper secret into configuration:</span></span>

* <span data-ttu-id="758f2-486">`Load`在名稱開頭為前置詞時，載入密碼。</span><span class="sxs-lookup"><span data-stu-id="758f2-486">`Load` loads a secret when its name starts with the prefix.</span></span> <span data-ttu-id="758f2-487">其他秘密則不會載入。</span><span class="sxs-lookup"><span data-stu-id="758f2-487">Other secrets aren't loaded.</span></span>
* <span data-ttu-id="758f2-488">`GetKey`:</span><span class="sxs-lookup"><span data-stu-id="758f2-488">`GetKey`:</span></span>
  * <span data-ttu-id="758f2-489">移除秘密名稱中的前置詞。</span><span class="sxs-lookup"><span data-stu-id="758f2-489">Removes the prefix from the secret name.</span></span>
  * <span data-ttu-id="758f2-490">將任何名稱中的兩個破折號取代為 `KeyDelimiter` ，這是設定中使用的分隔符號（通常是冒號）。</span><span class="sxs-lookup"><span data-stu-id="758f2-490">Replaces two dashes in any name with the `KeyDelimiter`, which is the delimiter used in configuration (usually a colon).</span></span> <span data-ttu-id="758f2-491">Azure Key Vault 在密碼名稱中不允許冒號。</span><span class="sxs-lookup"><span data-stu-id="758f2-491">Azure Key Vault doesn't allow a colon in secret names.</span></span>

[!code-csharp[](key-vault-configuration/samples_snapshot/Startup.cs)]

<span data-ttu-id="758f2-492">`Load`方法是由可逐一查看保存庫密碼的提供者演算法所呼叫，以尋找具有版本前置詞的金鑰。</span><span class="sxs-lookup"><span data-stu-id="758f2-492">The `Load` method is called by a provider algorithm that iterates through the vault secrets to find the ones that have the version prefix.</span></span> <span data-ttu-id="758f2-493">當找到版本前置詞時 `Load` ，演算法會使用 `GetKey` 方法來傳回密碼名稱的設定名稱。</span><span class="sxs-lookup"><span data-stu-id="758f2-493">When a version prefix is found with `Load`, the algorithm uses the `GetKey` method to return the configuration name of the secret name.</span></span> <span data-ttu-id="758f2-494">它會從密碼的名稱中去除版本前置詞，並傳回其餘的秘密名稱，以載入至應用程式的設定名稱/值配對。</span><span class="sxs-lookup"><span data-stu-id="758f2-494">It strips off the version prefix from the secret's name and returns the rest of the secret name for loading into the app's configuration name-value pairs.</span></span>

<span data-ttu-id="758f2-495">當此方法執行時：</span><span class="sxs-lookup"><span data-stu-id="758f2-495">When this approach is implemented:</span></span>

1. <span data-ttu-id="758f2-496">應用程式的專案檔中指定的應用程式版本。</span><span class="sxs-lookup"><span data-stu-id="758f2-496">The app's version specified in the app's project file.</span></span> <span data-ttu-id="758f2-497">在下列範例中，應用程式的版本會設定為 `5.0.0.0` ：</span><span class="sxs-lookup"><span data-stu-id="758f2-497">In the following example, the app's version is set to `5.0.0.0`:</span></span>

   ```xml
   <PropertyGroup>
     <Version>5.0.0.0</Version>
   </PropertyGroup>
   ```

1. <span data-ttu-id="758f2-498">確認 `<UserSecretsId>` 屬性存在於應用程式的專案檔中，其中 `{GUID}` 是使用者提供的 GUID：</span><span class="sxs-lookup"><span data-stu-id="758f2-498">Confirm that a `<UserSecretsId>` property is present in the app's project file, where `{GUID}` is a user-supplied GUID:</span></span>

   ```xml
   <PropertyGroup>
     <UserSecretsId>{GUID}</UserSecretsId>
   </PropertyGroup>
   ```

   <span data-ttu-id="758f2-499">使用[秘密管理員工具](xref:security/app-secrets)在本機儲存下列秘密：</span><span class="sxs-lookup"><span data-stu-id="758f2-499">Save the following secrets locally with the [Secret Manager tool](xref:security/app-secrets):</span></span>

   ```dotnetcli
   dotnet user-secrets set "5000-AppSecret" "5.0.0.0_secret_value_dev"
   dotnet user-secrets set "5100-AppSecret" "5.1.0.0_secret_value_dev"
   ```

1. <span data-ttu-id="758f2-500">使用下列 Azure CLI 命令，將秘密儲存在 Azure Key Vault 中：</span><span class="sxs-lookup"><span data-stu-id="758f2-500">Secrets are saved in Azure Key Vault using the following Azure CLI commands:</span></span>

   ```azurecli
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "5000-AppSecret" --value "5.0.0.0_secret_value_prod"
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "5100-AppSecret" --value "5.1.0.0_secret_value_prod"
   ```

1. <span data-ttu-id="758f2-501">當應用程式執行時，會載入金鑰保存庫密碼。</span><span class="sxs-lookup"><span data-stu-id="758f2-501">When the app is run, the key vault secrets are loaded.</span></span> <span data-ttu-id="758f2-502">的字串秘密 `5000-AppSecret` 會符合應用程式的專案檔（）中指定的應用程式版本 `5.0.0.0` 。</span><span class="sxs-lookup"><span data-stu-id="758f2-502">The string secret for `5000-AppSecret` is matched to the app's version specified in the app's project file (`5.0.0.0`).</span></span>

1. <span data-ttu-id="758f2-503">版本 `5000` （含破折號）會從索引鍵名稱中移除。</span><span class="sxs-lookup"><span data-stu-id="758f2-503">The version, `5000` (with the dash), is stripped from the key name.</span></span> <span data-ttu-id="758f2-504">在整個應用程式中，使用金鑰來讀取設定會 `AppSecret` 載入秘密值。</span><span class="sxs-lookup"><span data-stu-id="758f2-504">Throughout the app, reading configuration with the key `AppSecret` loads the secret value.</span></span>

1. <span data-ttu-id="758f2-505">如果應用程式的版本已在專案檔中變更為 `5.1.0.0` ，且應用程式再次執行，則傳回的秘密值會 `5.1.0.0_secret_value_dev` 在開發環境和 `5.1.0.0_secret_value_prod` 生產環境中。</span><span class="sxs-lookup"><span data-stu-id="758f2-505">If the app's version is changed in the project file to `5.1.0.0` and the app is run again, the secret value returned is `5.1.0.0_secret_value_dev` in the Development environment and `5.1.0.0_secret_value_prod` in Production.</span></span>

> [!NOTE]
> <span data-ttu-id="758f2-506">您也可以將自己的 <xref:Microsoft.Azure.KeyVault.KeyVaultClient> 實作為提供給 <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> 。</span><span class="sxs-lookup"><span data-stu-id="758f2-506">You can also provide your own <xref:Microsoft.Azure.KeyVault.KeyVaultClient> implementation to <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>.</span></span> <span data-ttu-id="758f2-507">自訂用戶端允許跨應用程式共用單一用戶端實例。</span><span class="sxs-lookup"><span data-stu-id="758f2-507">A custom client permits sharing a single instance of the client across the app.</span></span>

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="758f2-508">將陣列繫結到類別</span><span class="sxs-lookup"><span data-stu-id="758f2-508">Bind an array to a class</span></span>

<span data-ttu-id="758f2-509">提供者能夠將設定值讀入陣列中，以系結至 POCO 陣列。</span><span class="sxs-lookup"><span data-stu-id="758f2-509">The provider is capable of reading configuration values into an array for binding to a POCO array.</span></span>

<span data-ttu-id="758f2-510">從允許金鑰包含冒號（）分隔符號的設定來源讀取時 `:` ，會使用數值索引鍵區段來區別組成陣列的索引鍵（ `:0:` 、 `:1:` 、 &hellip; `:{n}:` ）。</span><span class="sxs-lookup"><span data-stu-id="758f2-510">When reading from a configuration source that allows keys to contain colon (`:`) separators, a numeric key segment is used to distinguish the keys that make up an array (`:0:`, `:1:`, &hellip; `:{n}:`).</span></span> <span data-ttu-id="758f2-511">如需詳細資訊，請參閱[Configuration：將陣列系結至類別](xref:fundamentals/configuration/index#bind-an-array-to-a-class)。</span><span class="sxs-lookup"><span data-stu-id="758f2-511">For more information, see [Configuration: Bind an array to a class](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span></span>

<span data-ttu-id="758f2-512">Azure Key Vault 索引鍵不能使用冒號做為分隔符號。</span><span class="sxs-lookup"><span data-stu-id="758f2-512">Azure Key Vault keys can't use a colon as a separator.</span></span> <span data-ttu-id="758f2-513">本主題中所述的方法會使用雙虛線（ `--` ）做為階層式值的分隔符號（區段）。</span><span class="sxs-lookup"><span data-stu-id="758f2-513">The approach described in this topic uses double dashes (`--`) as a separator for hierarchical values (sections).</span></span> <span data-ttu-id="758f2-514">陣列索引鍵會以雙虛線和數位索引鍵區段（ `--0--` 、 `--1--` 、 &hellip; `--{n}--` ）儲存在 Azure Key Vault 中。</span><span class="sxs-lookup"><span data-stu-id="758f2-514">Array keys are stored in Azure Key Vault with double dashes and numeric key segments (`--0--`, `--1--`, &hellip; `--{n}--`).</span></span>

<span data-ttu-id="758f2-515">檢查 JSON 檔案所提供的下列[Serilog](https://serilog.net/)記錄提供者設定。</span><span class="sxs-lookup"><span data-stu-id="758f2-515">Examine the following [Serilog](https://serilog.net/) logging provider configuration provided by a JSON file.</span></span> <span data-ttu-id="758f2-516">陣列中定義了兩個物件常值 `WriteTo` ，以反映兩個 Serilog*接收*，其中描述記錄輸出的目的地：</span><span class="sxs-lookup"><span data-stu-id="758f2-516">There are two object literals defined in the `WriteTo` array that reflect two Serilog *sinks*, which describe destinations for logging output:</span></span>

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

<span data-ttu-id="758f2-517">先前 JSON 檔案中顯示的設定會使用雙虛線（ `--` ）標記法和數值區段儲存在 Azure Key Vault 中：</span><span class="sxs-lookup"><span data-stu-id="758f2-517">The configuration shown in the preceding JSON file is stored in Azure Key Vault using double dash (`--`) notation and numeric segments:</span></span>

| <span data-ttu-id="758f2-518">Key</span><span class="sxs-lookup"><span data-stu-id="758f2-518">Key</span></span> | <span data-ttu-id="758f2-519">值</span><span class="sxs-lookup"><span data-stu-id="758f2-519">Value</span></span> |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="reload-secrets"></a><span data-ttu-id="758f2-520">重載秘密</span><span class="sxs-lookup"><span data-stu-id="758f2-520">Reload secrets</span></span>

<span data-ttu-id="758f2-521">會快取密碼，直到 `IConfigurationRoot.Reload()` 呼叫為止。</span><span class="sxs-lookup"><span data-stu-id="758f2-521">Secrets are cached until `IConfigurationRoot.Reload()` is called.</span></span> <span data-ttu-id="758f2-522">在執行之前，應用程式不會遵守金鑰保存庫中已過期、已停用及更新的秘密 `Reload` 。</span><span class="sxs-lookup"><span data-stu-id="758f2-522">Expired, disabled, and updated secrets in the key vault are not respected by the app until `Reload` is executed.</span></span>

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a><span data-ttu-id="758f2-523">已停用和過期的秘密</span><span class="sxs-lookup"><span data-stu-id="758f2-523">Disabled and expired secrets</span></span>

<span data-ttu-id="758f2-524">已停用和過期的秘密會擲回 <xref:Microsoft.Azure.KeyVault.Models.KeyVaultErrorException> 。</span><span class="sxs-lookup"><span data-stu-id="758f2-524">Disabled and expired secrets throw a <xref:Microsoft.Azure.KeyVault.Models.KeyVaultErrorException>.</span></span> <span data-ttu-id="758f2-525">若要防止應用程式擲回，請使用不同的設定提供者來提供設定，或更新已停用或已過期的密碼。</span><span class="sxs-lookup"><span data-stu-id="758f2-525">To prevent the app from throwing, provide the configuration using a different configuration provider or update the disabled or expired secret.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="758f2-526">疑難排解</span><span class="sxs-lookup"><span data-stu-id="758f2-526">Troubleshoot</span></span>

<span data-ttu-id="758f2-527">當應用程式無法使用提供者載入設定時，會將錯誤訊息寫入[ASP.NET Core 記錄基礎結構](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="758f2-527">When the app fails to load configuration using the provider, an error message is written to the [ASP.NET Core Logging infrastructure](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="758f2-528">下列條件將導致無法載入設定：</span><span class="sxs-lookup"><span data-stu-id="758f2-528">The following conditions will prevent configuration from loading:</span></span>

* <span data-ttu-id="758f2-529">應用程式或憑證未在 Azure Active Directory 中正確設定。</span><span class="sxs-lookup"><span data-stu-id="758f2-529">The app or certificate isn't configured correctly in Azure Active Directory.</span></span>
* <span data-ttu-id="758f2-530">金鑰保存庫不存在於 Azure Key Vault 中。</span><span class="sxs-lookup"><span data-stu-id="758f2-530">The key vault doesn't exist in Azure Key Vault.</span></span>
* <span data-ttu-id="758f2-531">應用程式未獲授權，無法存取金鑰保存庫。</span><span class="sxs-lookup"><span data-stu-id="758f2-531">The app isn't authorized to access the key vault.</span></span>
* <span data-ttu-id="758f2-532">存取原則不包含 `Get` 和 `List` 許可權。</span><span class="sxs-lookup"><span data-stu-id="758f2-532">The access policy doesn't include `Get` and `List` permissions.</span></span>
* <span data-ttu-id="758f2-533">在金鑰保存庫中，設定資料（名稱/值組）未正確命名、遺失、停用或過期。</span><span class="sxs-lookup"><span data-stu-id="758f2-533">In the key vault, the configuration data (name-value pair) is incorrectly named, missing, disabled, or expired.</span></span>
* <span data-ttu-id="758f2-534">應用程式具有錯誤的金鑰保存庫名稱（ `KeyVaultName` ）、Azure AD 應用程式識別碼（ `AzureADApplicationId` ），或 Azure AD 憑證指紋（ `AzureADCertThumbprint` ）。</span><span class="sxs-lookup"><span data-stu-id="758f2-534">The app has the wrong key vault name (`KeyVaultName`), Azure AD Application Id (`AzureADApplicationId`), or Azure AD certificate thumbprint (`AzureADCertThumbprint`).</span></span>
* <span data-ttu-id="758f2-535">在應用程式中，設定金鑰（名稱）不正確，因為您嘗試載入的值。</span><span class="sxs-lookup"><span data-stu-id="758f2-535">The configuration key (name) is incorrect in the app for the value you're trying to load.</span></span>
* <span data-ttu-id="758f2-536">將應用程式的存取原則新增至金鑰保存庫時，已建立原則，但未在 [**存取原則**] UI 中選取 [**儲存**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="758f2-536">When adding the access policy for the app to the key vault, the policy was created, but the **Save** button wasn't selected in the **Access policies** UI.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="758f2-537">其他資源</span><span class="sxs-lookup"><span data-stu-id="758f2-537">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* [<span data-ttu-id="758f2-538">Microsoft Azure： Key Vault</span><span class="sxs-lookup"><span data-stu-id="758f2-538">Microsoft Azure: Key Vault</span></span>](https://azure.microsoft.com/services/key-vault/)
* [<span data-ttu-id="758f2-539">Microsoft Azure： Key Vault 檔</span><span class="sxs-lookup"><span data-stu-id="758f2-539">Microsoft Azure: Key Vault Documentation</span></span>](/azure/key-vault/)
* [<span data-ttu-id="758f2-540">如何為 Azure Key Vault 產生及傳輸受 HSM 保護的金鑰</span><span class="sxs-lookup"><span data-stu-id="758f2-540">How to generate and transfer HSM-protected keys for Azure Key Vault</span></span>](/azure/key-vault/key-vault-hsm-protected-keys)
* [<span data-ttu-id="758f2-541">KeyVaultClient 類別</span><span class="sxs-lookup"><span data-stu-id="758f2-541">KeyVaultClient Class</span></span>](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
* [<span data-ttu-id="758f2-542">快速入門：使用 .NET Web 應用程式從 Azure Key Vault 設定及擷取祕密</span><span class="sxs-lookup"><span data-stu-id="758f2-542">Quickstart: Set and retrieve a secret from Azure Key Vault by using a .NET web app</span></span>](/azure/key-vault/quick-create-net)
* [<span data-ttu-id="758f2-543">教學課程：如何在 .NET 中搭配使用 Azure Key Vault 與 Azure Windows 虛擬機器</span><span class="sxs-lookup"><span data-stu-id="758f2-543">Tutorial: How to use Azure Key Vault with Azure Windows Virtual Machine in .NET</span></span>](/azure/key-vault/tutorial-net-windows-virtual-machine)

::: moniker-end

