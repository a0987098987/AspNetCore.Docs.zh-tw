---
title: ASP.NET核心中的 Azure 金鑰保管庫配置提供者
author: rick-anderson
description: 瞭解如何使用 Azure 密鑰保管庫配置提供程式使用運行時載入的名稱值對配置應用。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2020
uid: security/key-vault-configuration
ms.openlocfilehash: d78584eaa3fcd50425698e4db581060b6ca845f8
ms.sourcegitcommit: fbdb8b9ab5a52656384b117ff6e7c92ae070813c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/13/2020
ms.locfileid: "81228162"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a><span data-ttu-id="5b862-103">ASP.NET核心中的 Azure 金鑰保管庫配置提供者</span><span class="sxs-lookup"><span data-stu-id="5b862-103">Azure Key Vault Configuration Provider in ASP.NET Core</span></span>

<span data-ttu-id="5b862-104">由[安德魯斯坦頓護士](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="5b862-104">By [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="5b862-105">本文件介紹如何使用[Microsoft Azure 密鑰保管庫](https://azure.microsoft.com/services/key-vault/)配置提供程式從 Azure 密鑰保管庫機密載入應用配置值。</span><span class="sxs-lookup"><span data-stu-id="5b862-105">This document explains how to use the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) Configuration Provider to load app configuration values from Azure Key Vault secrets.</span></span> <span data-ttu-id="5b862-106">Azure 密鑰保管庫是一種基於雲的服務,可幫助保護應用和服務使用的加密密鑰和機密。</span><span class="sxs-lookup"><span data-stu-id="5b862-106">Azure Key Vault is a cloud-based service that assists in safeguarding cryptographic keys and secrets used by apps and services.</span></span> <span data-ttu-id="5b862-107">將 Azure 金鑰保管庫用於 ASP.NET核心應用的常見方案包括:</span><span class="sxs-lookup"><span data-stu-id="5b862-107">Common scenarios for using Azure Key Vault with ASP.NET Core apps include:</span></span>

* <span data-ttu-id="5b862-108">控制對敏感配置數據的訪問。</span><span class="sxs-lookup"><span data-stu-id="5b862-108">Controlling access to sensitive configuration data.</span></span>
* <span data-ttu-id="5b862-109">在儲存配置資料時滿足 FIPS 140-2 級驗證的硬體安全模組 (HSM) 的要求。</span><span class="sxs-lookup"><span data-stu-id="5b862-109">Meeting the requirement for FIPS 140-2 Level 2 validated Hardware Security Modules (HSM's) when storing configuration data.</span></span>

<span data-ttu-id="5b862-110">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/samples)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5b862-110">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="packages"></a><span data-ttu-id="5b862-111">Packages</span><span class="sxs-lookup"><span data-stu-id="5b862-111">Packages</span></span>

<span data-ttu-id="5b862-112">向[Microsoft.擴展.配置.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/)包添加包引用。</span><span class="sxs-lookup"><span data-stu-id="5b862-112">Add a package reference to the [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) package.</span></span>

## <a name="sample-app"></a><span data-ttu-id="5b862-113">範例應用程式</span><span class="sxs-lookup"><span data-stu-id="5b862-113">Sample app</span></span>

<span data-ttu-id="5b862-114">範例應用以`#define`*Program.cs*檔案頂部的語句確定的兩種模式之一運行:</span><span class="sxs-lookup"><span data-stu-id="5b862-114">The sample app runs in either of two modes determined by the `#define` statement at the top of the *Program.cs* file:</span></span>

* <span data-ttu-id="5b862-115">`Certificate`&ndash;演示使用 Azure 金鑰保管庫客戶端 ID 和 X.509 憑證存取儲存在 Azure 密鑰保管庫中的秘密。</span><span class="sxs-lookup"><span data-stu-id="5b862-115">`Certificate` &ndash; Demonstrates the use of an Azure Key Vault Client ID and X.509 certificate to access secrets stored in Azure Key Vault.</span></span> <span data-ttu-id="5b862-116">此版本的示例可以從任何位置運行,部署到 Azure 應用服務或任何能夠為 ASP.NET 酷應用提供服務的主機。</span><span class="sxs-lookup"><span data-stu-id="5b862-116">This version of the sample can be run from any location, deployed to Azure App Service or any host capable of serving an ASP.NET Core app.</span></span>
* <span data-ttu-id="5b862-117">`Managed`&ndash;演示如何[使用 Azure 資源的託管識別](/azure/active-directory/managed-identities-azure-resources/overview)使用 Azure AD 身份驗證將應用驗證為 Azure 密鑰保管庫,而無需在應用的代碼或配置中儲存認證。</span><span class="sxs-lookup"><span data-stu-id="5b862-117">`Managed` &ndash; Demonstrates how to use [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview) to authenticate the app to Azure Key Vault with Azure AD authentication without credentials stored in the app's code or configuration.</span></span> <span data-ttu-id="5b862-118">使用託管標識進行身份驗證時,不需要 Azure AD 應用程式 ID 和密碼(用戶端密鑰)。</span><span class="sxs-lookup"><span data-stu-id="5b862-118">When using managed identities to authenticate, an Azure AD Application ID and Password (Client Secret) aren't required.</span></span> <span data-ttu-id="5b862-119">必須`Managed`將示例的版本部署到 Azure。</span><span class="sxs-lookup"><span data-stu-id="5b862-119">The `Managed` version of the sample must be deployed to Azure.</span></span> <span data-ttu-id="5b862-120">按照"[為 Azure 資源使用託管標識](#use-managed-identities-for-azure-resources)「部分中的指南進行操作。</span><span class="sxs-lookup"><span data-stu-id="5b862-120">Follow the guidance in the [Use the Managed identities for Azure resources](#use-managed-identities-for-azure-resources) section.</span></span>

<span data-ttu-id="5b862-121">有關如何使用預處理器指令 ()`#define`設定範例應用程式的詳細資訊,請<xref:index#preprocessor-directives-in-sample-code>參閱 。</span><span class="sxs-lookup"><span data-stu-id="5b862-121">For more information on how to configure a sample app using preprocessor directives (`#define`), see <xref:index#preprocessor-directives-in-sample-code>.</span></span>

## <a name="secret-storage-in-the-development-environment"></a><span data-ttu-id="5b862-122">開發環境中的秘密儲存</span><span class="sxs-lookup"><span data-stu-id="5b862-122">Secret storage in the Development environment</span></span>

<span data-ttu-id="5b862-123">使用[機密管理器工具](xref:security/app-secrets)在本地設置機密。</span><span class="sxs-lookup"><span data-stu-id="5b862-123">Set secrets locally using the [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="5b862-124">當示例應用在開發環境中的本地電腦上運行時,將從本地機密管理器儲存載入機密。</span><span class="sxs-lookup"><span data-stu-id="5b862-124">When the sample app runs on the local machine in the Development environment, secrets are loaded from the local Secret Manager store.</span></span>

<span data-ttu-id="5b862-125">「機密管理員」工具需要`<UserSecretsId>`應用的專案檔中的屬性。</span><span class="sxs-lookup"><span data-stu-id="5b862-125">The Secret Manager tool requires a `<UserSecretsId>` property in the app's project file.</span></span> <span data-ttu-id="5b862-126">將屬性值`{GUID}`( ) 設定為任何唯一的 GUID:</span><span class="sxs-lookup"><span data-stu-id="5b862-126">Set the property value (`{GUID}`) to any unique GUID:</span></span>

```xml
<PropertyGroup>
  <UserSecretsId>{GUID}</UserSecretsId>
</PropertyGroup>
```

<span data-ttu-id="5b862-127">機密創建為名稱值對。</span><span class="sxs-lookup"><span data-stu-id="5b862-127">Secrets are created as name-value pairs.</span></span> <span data-ttu-id="5b862-128">層次結構值(配置部分)在`:`[ASP.NET核心配置](xref:fundamentals/configuration/index)密鑰名稱中使用(冒號)作為分隔符。</span><span class="sxs-lookup"><span data-stu-id="5b862-128">Hierarchical values (configuration sections) use a `:` (colon) as a separator in [ASP.NET Core configuration](xref:fundamentals/configuration/index) key names.</span></span>

<span data-ttu-id="5b862-129">機密管理員從開啟到項目[內容根](xref:fundamentals/index#content-root)的命令外殼使用`{SECRET NAME}`,其中的`{SECRET VALUE}`名稱和 值是:</span><span class="sxs-lookup"><span data-stu-id="5b862-129">The Secret Manager is used from a command shell opened to the project's [content root](xref:fundamentals/index#content-root), where `{SECRET NAME}` is the name and `{SECRET VALUE}` is the value:</span></span>

```dotnetcli
dotnet user-secrets set "{SECRET NAME}" "{SECRET VALUE}"
```

<span data-ttu-id="5b862-130">從項目[的內容根](xref:fundamentals/index#content-root)在命令 shell 中執行以下指令,以設定範例的祕密:</span><span class="sxs-lookup"><span data-stu-id="5b862-130">Execute the following commands in a command shell from the project's [content root](xref:fundamentals/index#content-root) to set the secrets for the sample app:</span></span>

```dotnetcli
dotnet user-secrets set "SecretName" "secret_value_1_dev"
dotnet user-secrets set "Section:SecretName" "secret_value_2_dev"
```

<span data-ttu-id="5b862-131">當這些機密儲存在[Azure 金鑰保管庫部分的生產環境中的「機密記憶體」中的](#secret-storage-in-the-production-environment-with-azure-key-vault)Azure`_dev`金鑰保管 庫中`_prod`時,後綴將更改為 。</span><span class="sxs-lookup"><span data-stu-id="5b862-131">When these secrets are stored in Azure Key Vault in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section, the `_dev` suffix is changed to `_prod`.</span></span> <span data-ttu-id="5b862-132">後綴在應用的輸出中提供指示配置值來源的可視提示。</span><span class="sxs-lookup"><span data-stu-id="5b862-132">The suffix provides a visual cue in the app's output indicating the source of the configuration values.</span></span>

## <a name="secret-storage-in-the-production-environment-with-azure-key-vault"></a><span data-ttu-id="5b862-133">使用 Azure 金鑰保存式庫的生產環境</span><span class="sxs-lookup"><span data-stu-id="5b862-133">Secret storage in the Production environment with Azure Key Vault</span></span>

<span data-ttu-id="5b862-134">此處匯總了[「快速入門:使用 Azure CLI」主題從 Azure 密鑰保管庫設置和檢索機密](/azure/key-vault/quick-create-cli)的說明,用於創建 Azure 密鑰保管庫並儲存範例應用使用的秘密。</span><span class="sxs-lookup"><span data-stu-id="5b862-134">The instructions provided by the [Quickstart: Set and retrieve a secret from Azure Key Vault using Azure CLI](/azure/key-vault/quick-create-cli) topic are summarized here for creating an Azure Key Vault and storing secrets used by the sample app.</span></span> <span data-ttu-id="5b862-135">有關詳細資訊,請參閱主題。</span><span class="sxs-lookup"><span data-stu-id="5b862-135">Refer to the topic for further details.</span></span>

1. <span data-ttu-id="5b862-136">使用[Azure 門戶](https://portal.azure.com/)中的以下任一方法打開 Azure 雲外殼:</span><span class="sxs-lookup"><span data-stu-id="5b862-136">Open Azure Cloud shell using any one of the following methods in the [Azure portal](https://portal.azure.com/):</span></span>

   * <span data-ttu-id="5b862-137">選取程式碼區塊右上角的 [試試看]  。</span><span class="sxs-lookup"><span data-stu-id="5b862-137">Select **Try It** in the upper-right corner of a code block.</span></span> <span data-ttu-id="5b862-138">在文字框中使用搜索字串「Azure CLI」。。</span><span class="sxs-lookup"><span data-stu-id="5b862-138">Use the search string "Azure CLI" in the text box.</span></span>
   * <span data-ttu-id="5b862-139">使用**啟動雲外殼**按鈕在瀏覽器中打開雲外殼。</span><span class="sxs-lookup"><span data-stu-id="5b862-139">Open Cloud Shell in your browser with the **Launch Cloud Shell** button.</span></span>
   * <span data-ttu-id="5b862-140">選取 Azure 入口網站右上角功能表上的 **[Cloud Shell]** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="5b862-140">Select the **Cloud Shell** button on the menu in the upper-right corner of the Azure portal.</span></span>

   <span data-ttu-id="5b862-141">有關詳細資訊,請參閱[Azure CLI](/cli/azure/)和[Azure 雲外殼概述](/azure/cloud-shell/overview)。</span><span class="sxs-lookup"><span data-stu-id="5b862-141">For more information, see [Azure CLI](/cli/azure/) and [Overview of Azure Cloud Shell](/azure/cloud-shell/overview).</span></span>

1. <span data-ttu-id="5b862-142">如果您尚未通過身份驗證,請使用命令`az login`登錄。</span><span class="sxs-lookup"><span data-stu-id="5b862-142">If you aren't already authenticated, sign in with the `az login` command.</span></span>

1. <span data-ttu-id="5b862-143">使用以下指令建立資源群組,其中`{RESOURCE GROUP NAME}`為新資源群組的資源組名`{LOCATION}`稱為 Azure 區域(資料中心):</span><span class="sxs-lookup"><span data-stu-id="5b862-143">Create a resource group with the following command, where `{RESOURCE GROUP NAME}` is the resource group name for the new resource group and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```azurecli
   az group create --name "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="5b862-144">使用以下指令在資源群組中建立金鑰保管庫,其中`{KEY VAULT NAME}`為新密鑰保管庫的名稱`{LOCATION}`, 是 Azure 區域(資料中心):</span><span class="sxs-lookup"><span data-stu-id="5b862-144">Create a key vault in the resource group with the following command, where `{KEY VAULT NAME}` is the name for the new key vault and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```azurecli
   az keyvault create --name {KEY VAULT NAME} --resource-group "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="5b862-145">在密鑰保管庫中創建機密作為名稱值對。</span><span class="sxs-lookup"><span data-stu-id="5b862-145">Create secrets in the key vault as name-value pairs.</span></span>

   <span data-ttu-id="5b862-146">Azure 密鑰保管庫金鑰庫密鑰名稱僅限於字母數位字元和破折號。</span><span class="sxs-lookup"><span data-stu-id="5b862-146">Azure Key Vault secret names are limited to alphanumeric characters and dashes.</span></span> <span data-ttu-id="5b862-147">階層值(配置部分)使用`--`(兩個破折號)作為分隔符。</span><span class="sxs-lookup"><span data-stu-id="5b862-147">Hierarchical values (configuration sections) use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="5b862-148">在密鑰保管庫密鑰名稱中,通常不允許從[ASP.NET Core 配置](xref:fundamentals/configuration/index)中從子鍵中分隔節的冒號。</span><span class="sxs-lookup"><span data-stu-id="5b862-148">Colons, which are normally used to delimit a section from a subkey in [ASP.NET Core configuration](xref:fundamentals/configuration/index), aren't allowed in key vault secret names.</span></span> <span data-ttu-id="5b862-149">因此,當機密載入應用的配置中時,將使用兩個破折號並將其交換為冒號。</span><span class="sxs-lookup"><span data-stu-id="5b862-149">Therefore, two dashes are used and swapped for a colon when the secrets are loaded into the app's configuration.</span></span>

   <span data-ttu-id="5b862-150">以下機密用於示例應用。</span><span class="sxs-lookup"><span data-stu-id="5b862-150">The following secrets are for use with the sample app.</span></span> <span data-ttu-id="5b862-151">這些值包括後`_prod`綴,用於將它們與在開發環境`_dev`中 載入的後綴值與使用者機密區分開來。</span><span class="sxs-lookup"><span data-stu-id="5b862-151">The values include a `_prod` suffix to distinguish them from the `_dev` suffix values loaded in the Development environment from User Secrets.</span></span> <span data-ttu-id="5b862-152">取代為`{KEY VAULT NAME}`在上一步驟中建立的金鑰保管庫的名稱:</span><span class="sxs-lookup"><span data-stu-id="5b862-152">Replace `{KEY VAULT NAME}` with the name of the key vault that you created in the prior step:</span></span>

   ```azurecli
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "SecretName" --value "secret_value_1_prod"
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "Section--SecretName" --value "secret_value_2_prod"
   ```

## <a name="use-application-id-and-x509-certificate-for-non-azure-hosted-apps"></a><span data-ttu-id="5b862-153">對非 Azure 託管應用使用應用程式 ID 和 X.509 憑證</span><span class="sxs-lookup"><span data-stu-id="5b862-153">Use Application ID and X.509 certificate for non-Azure-hosted apps</span></span>

<span data-ttu-id="5b862-154">將 Azure AD、Azure 密鑰保管庫和應用配置為使用 Azure 活動目錄應用程式 ID 和 X.509 證書在**應用託管在 Azure 之外時**對金鑰保管庫進行身份驗證。</span><span class="sxs-lookup"><span data-stu-id="5b862-154">Configure Azure AD, Azure Key Vault, and the app to use an Azure Active Directory Application ID and X.509 certificate to authenticate to a key vault **when the app is hosted outside of Azure**.</span></span> <span data-ttu-id="5b862-155">如需詳細資訊，請參閱[關於金鑰、祕密和憑證](/azure/key-vault/about-keys-secrets-and-certificates)。</span><span class="sxs-lookup"><span data-stu-id="5b862-155">For more information, see [About keys, secrets, and certificates](/azure/key-vault/about-keys-secrets-and-certificates).</span></span>

> [!NOTE]
> <span data-ttu-id="5b862-156">儘管 Azure 中託管的應用支援使用應用程式 ID 和 X.509 憑證,但我們建議在 Azure 中託管應用時[對 Azure 資源使用託管標識](#use-managed-identities-for-azure-resources)。</span><span class="sxs-lookup"><span data-stu-id="5b862-156">Although using an Application ID and X.509 certificate is supported for apps hosted in Azure, we recommend using [Managed identities for Azure resources](#use-managed-identities-for-azure-resources) when hosting an app in Azure.</span></span> <span data-ttu-id="5b862-157">託管標識不需要在應用或開發環境中存儲證書。</span><span class="sxs-lookup"><span data-stu-id="5b862-157">Managed identities don't require storing a certificate in the app or in the development environment.</span></span>

<span data-ttu-id="5b862-158">當*Program.cs*檔案頂部的語句`Certificate`設定為`#define`時, 範例應用程式使用應用程式 ID 和 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="5b862-158">The sample app uses an Application ID and X.509 certificate when the `#define` statement at the top of the *Program.cs* file is set to `Certificate`.</span></span>

1. <span data-ttu-id="5b862-159">創建 PKCS_12 存檔 (*.pfx*) 證書。</span><span class="sxs-lookup"><span data-stu-id="5b862-159">Create a PKCS#12 archive (*.pfx*) certificate.</span></span> <span data-ttu-id="5b862-160">建立憑證的選項包括[Windows 上的 MakeCert](/windows/desktop/seccrypto/makecert)與[OpenSSL](https://www.openssl.org/)。</span><span class="sxs-lookup"><span data-stu-id="5b862-160">Options for creating certificates include [MakeCert on Windows](/windows/desktop/seccrypto/makecert) and [OpenSSL](https://www.openssl.org/).</span></span>
1. <span data-ttu-id="5b862-161">將憑證安裝到當前使用者的個人憑證儲存中。</span><span class="sxs-lookup"><span data-stu-id="5b862-161">Install the certificate into the current user's personal certificate store.</span></span> <span data-ttu-id="5b862-162">將密鑰標記為可匯出是可選的。</span><span class="sxs-lookup"><span data-stu-id="5b862-162">Marking the key as exportable is optional.</span></span> <span data-ttu-id="5b862-163">請注意證書的指紋,此過程稍後將使用。</span><span class="sxs-lookup"><span data-stu-id="5b862-163">Note the certificate's thumbprint, which is used later in this process.</span></span>
1. <span data-ttu-id="5b862-164">將 PKCS_12 存檔 *(.pfx*) 證書匯出為 DER 編碼證書 *(.cer*)。</span><span class="sxs-lookup"><span data-stu-id="5b862-164">Export the PKCS#12 archive (*.pfx*) certificate as a DER-encoded certificate (*.cer*).</span></span>
1. <span data-ttu-id="5b862-165">使用 Azure AD(**應用註冊)註冊**應用。</span><span class="sxs-lookup"><span data-stu-id="5b862-165">Register the app with Azure AD (**App registrations**).</span></span>
1. <span data-ttu-id="5b862-166">將 DER 編碼的憑證 *(.cer*) 上傳到 Azure AD:</span><span class="sxs-lookup"><span data-stu-id="5b862-166">Upload the DER-encoded certificate (*.cer*) to Azure AD:</span></span>
   1. <span data-ttu-id="5b862-167">在 Azure AD 中選擇應用。</span><span class="sxs-lookup"><span data-stu-id="5b862-167">Select the app in Azure AD.</span></span>
   1. <span data-ttu-id="5b862-168">導覽到**憑證&機密**。</span><span class="sxs-lookup"><span data-stu-id="5b862-168">Navigate to **Certificates & secrets**.</span></span>
   1. <span data-ttu-id="5b862-169">選擇 **「上載證書**」以上載包含公開金鑰的憑證。</span><span class="sxs-lookup"><span data-stu-id="5b862-169">Select **Upload certificate** to upload the certificate, which contains the public key.</span></span> <span data-ttu-id="5b862-170">*.cer* *、.pem*或 *.crt*證書是可以接受的。</span><span class="sxs-lookup"><span data-stu-id="5b862-170">A *.cer*, *.pem*, or *.crt* certificate is acceptable.</span></span>
1. <span data-ttu-id="5b862-171">將金鑰保管庫名稱、應用程式 ID 和證書指紋存儲在應用*的應用設置.json*檔中。</span><span class="sxs-lookup"><span data-stu-id="5b862-171">Store the key vault name, Application ID, and certificate thumbprint in the app's *appsettings.json* file.</span></span>
1. <span data-ttu-id="5b862-172">瀏覽到 Azure 門戶中的**金鑰保管庫**。</span><span class="sxs-lookup"><span data-stu-id="5b862-172">Navigate to **Key vaults** in the Azure portal.</span></span>
1. <span data-ttu-id="5b862-173">[使用 Azure 金鑰保管庫部分在「生產環境」 中的秘密儲存中選擇在「機密存儲」 中](#secret-storage-in-the-production-environment-with-azure-key-vault)創建的金鑰保管庫。</span><span class="sxs-lookup"><span data-stu-id="5b862-173">Select the key vault that you created in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section.</span></span>
1. <span data-ttu-id="5b862-174">選擇**存取原則**。</span><span class="sxs-lookup"><span data-stu-id="5b862-174">Select **Access policies**.</span></span>
1. <span data-ttu-id="5b862-175">選擇 **「添加存取策略**」。</span><span class="sxs-lookup"><span data-stu-id="5b862-175">Select **Add Access Policy**.</span></span>
1. <span data-ttu-id="5b862-176">打開 **「機密」許可權**,並提供具有**獲取**和**列表**許可權的應用。</span><span class="sxs-lookup"><span data-stu-id="5b862-176">Open **Secret permissions** and provide the app with **Get** and **List** permissions.</span></span>
1. <span data-ttu-id="5b862-177">選擇 **「選擇主體**」並按名稱選擇已註冊的應用。</span><span class="sxs-lookup"><span data-stu-id="5b862-177">Select **Select principal** and select the registered app by name.</span></span> <span data-ttu-id="5b862-178">選取 [選取]  按鈕。</span><span class="sxs-lookup"><span data-stu-id="5b862-178">Select the **Select** button.</span></span>
1. <span data-ttu-id="5b862-179">選取 [確定]  。</span><span class="sxs-lookup"><span data-stu-id="5b862-179">Select **OK**.</span></span>
1. <span data-ttu-id="5b862-180">選取 [儲存]  。</span><span class="sxs-lookup"><span data-stu-id="5b862-180">Select **Save**.</span></span>
1. <span data-ttu-id="5b862-181">部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="5b862-181">Deploy the app.</span></span>

<span data-ttu-id="5b862-182">範例`Certificate`套`IConfigurationRoot`用與機密名稱同名取得其設定值:</span><span class="sxs-lookup"><span data-stu-id="5b862-182">The `Certificate` sample app obtains its configuration values from `IConfigurationRoot` with the same name as the secret name:</span></span>

* <span data-ttu-id="5b862-183">非階層值:使用 取得`SecretName`的值`config["SecretName"]`。</span><span class="sxs-lookup"><span data-stu-id="5b862-183">Non-hierarchical values: The value for `SecretName` is obtained with `config["SecretName"]`.</span></span>
* <span data-ttu-id="5b862-184">分層值(節):使用`:`(冒號)表示法`GetSection`或 擴展方法。</span><span class="sxs-lookup"><span data-stu-id="5b862-184">Hierarchical values (sections): Use `:` (colon) notation or the `GetSection` extension method.</span></span> <span data-ttu-id="5b862-185">使用以下任一方法取得設定值值:</span><span class="sxs-lookup"><span data-stu-id="5b862-185">Use either of these approaches to obtain the configuration value:</span></span>
  * `config["Section:SecretName"]`
  * `config.GetSection("Section")["SecretName"]`

<span data-ttu-id="5b862-186">X.509 證書由操作系統管理。</span><span class="sxs-lookup"><span data-stu-id="5b862-186">The X.509 certificate is managed by the OS.</span></span> <span data-ttu-id="5b862-187">套用套<xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>用*應用程式設定.json*檔提供的值:</span><span class="sxs-lookup"><span data-stu-id="5b862-187">The app calls <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> with values supplied by the *appsettings.json* file:</span></span>

[!code-csharp[](key-vault-configuration/samples/3.x/SampleApp/Program.cs?name=snippet1&highlight=20-23)]

<span data-ttu-id="5b862-188">範例值：</span><span class="sxs-lookup"><span data-stu-id="5b862-188">Example values:</span></span>

* <span data-ttu-id="5b862-189">金鑰保存庫名稱:`contosovault`</span><span class="sxs-lookup"><span data-stu-id="5b862-189">Key vault name: `contosovault`</span></span>
* <span data-ttu-id="5b862-190">應用程式識別碼:`627e911e-43cc-61d4-992e-12db9c81b413`</span><span class="sxs-lookup"><span data-stu-id="5b862-190">Application ID: `627e911e-43cc-61d4-992e-12db9c81b413`</span></span>
* <span data-ttu-id="5b862-191">憑證指紋:`fe14593dd66b2406c5269d742d04b6e1ab03adb1`</span><span class="sxs-lookup"><span data-stu-id="5b862-191">Certificate thumbprint: `fe14593dd66b2406c5269d742d04b6e1ab03adb1`</span></span>

<span data-ttu-id="5b862-192">*應用程式設定.json*:</span><span class="sxs-lookup"><span data-stu-id="5b862-192">*appsettings.json*:</span></span>

[!code-json[](key-vault-configuration/samples/3.x/SampleApp/appsettings.json?highlight=10-12)]

<span data-ttu-id="5b862-193">運行應用時,網頁將顯示載入的機密值。</span><span class="sxs-lookup"><span data-stu-id="5b862-193">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="5b862-194">在開發環境中,機密值隨後綴一`_dev`起載入。</span><span class="sxs-lookup"><span data-stu-id="5b862-194">In the Development environment, secret values load with the `_dev` suffix.</span></span> <span data-ttu-id="5b862-195">在生產環境中,值使用`_prod`後綴載入。</span><span class="sxs-lookup"><span data-stu-id="5b862-195">In the Production environment, the values load with the `_prod` suffix.</span></span>

## <a name="use-managed-identities-for-azure-resources"></a><span data-ttu-id="5b862-196">對 Azure 資源使用託管識別</span><span class="sxs-lookup"><span data-stu-id="5b862-196">Use Managed identities for Azure resources</span></span>

<span data-ttu-id="5b862-197">**部署到 Azure 的應用**可以利用 Azure[資源的託管標識](/azure/active-directory/managed-identities-azure-resources/overview),這允許應用使用 Azure 密鑰保管庫進行身份驗證,而無需在應用中存儲認證(應用程式 ID 和密碼/用戶端金鑰)。</span><span class="sxs-lookup"><span data-stu-id="5b862-197">**An app deployed to Azure** can take advantage of [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview), which allows the app to authenticate with Azure Key Vault using Azure AD authentication without credentials (Application ID and Password/Client Secret) stored in the app.</span></span>

<span data-ttu-id="5b862-198">當*Program.cs*檔案頂部的語句`Managed`設定為`#define`時, 範例應用對 Azure 資源使用託管標識。</span><span class="sxs-lookup"><span data-stu-id="5b862-198">The sample app uses Managed identities for Azure resources when the `#define` statement at the top of the *Program.cs* file is set to `Managed`.</span></span>

<span data-ttu-id="5b862-199">在應用*的應用設置.json*檔中輸入保管庫名稱。</span><span class="sxs-lookup"><span data-stu-id="5b862-199">Enter the vault name into the app's *appsettings.json* file.</span></span> <span data-ttu-id="5b862-200">當範例應用設定為`Managed`版本時,不需要應用程式 ID 和密碼(用戶端密鑰),因此您可以忽略這些配置條目。</span><span class="sxs-lookup"><span data-stu-id="5b862-200">The sample app doesn't require an Application ID and Password (Client Secret) when set to the `Managed` version, so you can ignore those configuration entries.</span></span> <span data-ttu-id="5b862-201">應用將部署到 Azure,Azure 會驗證應用僅使用儲存在*appsettings.json*檔中的保管庫名稱訪問 Azure 密鑰保管庫。</span><span class="sxs-lookup"><span data-stu-id="5b862-201">The app is deployed to Azure, and Azure authenticates the app to access Azure Key Vault only using the vault name stored in the *appsettings.json* file.</span></span>

<span data-ttu-id="5b862-202">將示例應用部署到 Azure 應用服務。</span><span class="sxs-lookup"><span data-stu-id="5b862-202">Deploy the sample app to Azure App Service.</span></span>

<span data-ttu-id="5b862-203">創建服務時,部署到 Azure 應用服務的應用將自動註冊到 Azure AD。</span><span class="sxs-lookup"><span data-stu-id="5b862-203">An app deployed to Azure App Service is automatically registered with Azure AD when the service is created.</span></span> <span data-ttu-id="5b862-204">從部署中獲取對象 ID,以便在以下命令中使用。</span><span class="sxs-lookup"><span data-stu-id="5b862-204">Obtain the Object ID from the deployment for use in the following command.</span></span> <span data-ttu-id="5b862-205">物件識別碼顯示在應用服務的 **「標識」** 面板上的 Azure 入口中。</span><span class="sxs-lookup"><span data-stu-id="5b862-205">The Object ID is shown in the Azure portal on the **Identity** panel of the App Service.</span></span>

<span data-ttu-id="5b862-206">使用 Azure CLI 和應用程式 ID,向`list`應用程式`get`提供 存取 金鑰保管庫的許可權:</span><span class="sxs-lookup"><span data-stu-id="5b862-206">Using Azure CLI and the app's Object ID, provide the app with `list` and `get` permissions to access the key vault:</span></span>

```azurecli
az keyvault set-policy --name {KEY VAULT NAME} --object-id {OBJECT ID} --secret-permissions get list
```

<span data-ttu-id="5b862-207">使用 Azure CLI、PowerShell 或 Azure 門戶**重新啟動應用**。</span><span class="sxs-lookup"><span data-stu-id="5b862-207">**Restart the app** using Azure CLI, PowerShell, or the Azure portal.</span></span>

<span data-ttu-id="5b862-208">範例應用程式:</span><span class="sxs-lookup"><span data-stu-id="5b862-208">The sample app:</span></span>

* <span data-ttu-id="5b862-209">建立沒有連接字串的`AzureServiceTokenProvider`類實例。</span><span class="sxs-lookup"><span data-stu-id="5b862-209">Creates an instance of the `AzureServiceTokenProvider` class without a connection string.</span></span> <span data-ttu-id="5b862-210">未提供連接字串時,提供程式將嘗試從 Azure 資源的託管標識獲取訪問權杖。</span><span class="sxs-lookup"><span data-stu-id="5b862-210">When a connection string isn't provided, the provider attempts to obtain an access token from Managed identities for Azure resources.</span></span>
* <span data-ttu-id="5b862-211"><xref:Microsoft.Azure.KeyVault.KeyVaultClient>使用`AzureServiceTokenProvider`實例令牌回調創建新。</span><span class="sxs-lookup"><span data-stu-id="5b862-211">A new <xref:Microsoft.Azure.KeyVault.KeyVaultClient> is created with the `AzureServiceTokenProvider` instance token callback.</span></span>
* <span data-ttu-id="5b862-212">實例<xref:Microsoft.Azure.KeyVault.KeyVaultClient>與載入所有機密值的預設<xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>實現 一起使用,並將雙分線`--`() 替換為鍵`:`名中的冒號 ( )。</span><span class="sxs-lookup"><span data-stu-id="5b862-212">The <xref:Microsoft.Azure.KeyVault.KeyVaultClient> instance is used with a default implementation of <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> that loads all secret values and replaces double-dashes (`--`) with colons (`:`) in key names.</span></span>

[!code-csharp[](key-vault-configuration/samples/3.x/SampleApp/Program.cs?name=snippet2&highlight=13-21)]

<span data-ttu-id="5b862-213">金鑰保存庫名稱範例值:`contosovault`</span><span class="sxs-lookup"><span data-stu-id="5b862-213">Key vault name example value: `contosovault`</span></span>
    
<span data-ttu-id="5b862-214">*應用程式設定.json*:</span><span class="sxs-lookup"><span data-stu-id="5b862-214">*appsettings.json*:</span></span>

```json
{
  "KeyVaultName": "Key Vault Name"
}
```

<span data-ttu-id="5b862-215">運行應用時,網頁將顯示載入的機密值。</span><span class="sxs-lookup"><span data-stu-id="5b862-215">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="5b862-216">在開發環境中,機密值具有後綴`_dev`,因為它們由使用者機密提供。</span><span class="sxs-lookup"><span data-stu-id="5b862-216">In the Development environment, secret values have the `_dev` suffix because they're provided by User Secrets.</span></span> <span data-ttu-id="5b862-217">在生產環境中,值使用後綴載入,`_prod`因為它們由 Azure 密鑰保管庫提供。</span><span class="sxs-lookup"><span data-stu-id="5b862-217">In the Production environment, the values load with the `_prod` suffix because they're provided by Azure Key Vault.</span></span>

<span data-ttu-id="5b862-218">如果收到錯誤`Access denied`,請確認應用已註冊 Azure AD 並提供有關密鑰保管庫的訪問許可權。</span><span class="sxs-lookup"><span data-stu-id="5b862-218">If you receive an `Access denied` error, confirm that the app is registered with Azure AD and provided access to the key vault.</span></span> <span data-ttu-id="5b862-219">確認已重新啟動 Azure 中的服務。</span><span class="sxs-lookup"><span data-stu-id="5b862-219">Confirm that you've restarted the service in Azure.</span></span>

<span data-ttu-id="5b862-220">有關將提供程式與託管標識和 Azure DevOps 管道一起使用的資訊,請參閱創建 Azure[資源管理器服務連接到具有託管服務標識的 VM。](/azure/devops/pipelines/library/connect-to-azure#create-an-azure-resource-manager-service-connection-to-a-vm-with-a-managed-service-identity)</span><span class="sxs-lookup"><span data-stu-id="5b862-220">For information on using the provider with a managed identity and an Azure DevOps pipeline, see [Create an Azure Resource Manager service connection to a VM with a managed service identity](/azure/devops/pipelines/library/connect-to-azure#create-an-azure-resource-manager-service-connection-to-a-vm-with-a-managed-service-identity).</span></span>

## <a name="configuration-options"></a><span data-ttu-id="5b862-221">設定選項</span><span class="sxs-lookup"><span data-stu-id="5b862-221">Configuration options</span></span>

<span data-ttu-id="5b862-222"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>可以接受<xref:Microsoft.Extensions.Configuration.AzureKeyVault.AzureKeyVaultConfigurationOptions>:</span><span class="sxs-lookup"><span data-stu-id="5b862-222"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> can accept an <xref:Microsoft.Extensions.Configuration.AzureKeyVault.AzureKeyVaultConfigurationOptions>:</span></span>

```csharp
config.AddAzureKeyVault(
    new AzureKeyVaultConfigurationOptions()
    {
        ...
    });
```

| <span data-ttu-id="5b862-223">屬性</span><span class="sxs-lookup"><span data-stu-id="5b862-223">Property</span></span>         | <span data-ttu-id="5b862-224">描述</span><span class="sxs-lookup"><span data-stu-id="5b862-224">Description</span></span> |
| ---------------- | ----------- |
| `Client`         | <span data-ttu-id="5b862-225"><xref:Microsoft.Azure.KeyVault.KeyVaultClient>用於檢索值。</span><span class="sxs-lookup"><span data-stu-id="5b862-225"><xref:Microsoft.Azure.KeyVault.KeyVaultClient> to use for retrieving values.</span></span> |
| `Manager`        | <span data-ttu-id="5b862-226"><xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>用於控制機密載入的實例。</span><span class="sxs-lookup"><span data-stu-id="5b862-226"><xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> instance used to control secret loading.</span></span> |
| `ReloadInterval` | <span data-ttu-id="5b862-227">`Timespan`在輪詢金鑰保管庫以進行更改的嘗試之間等待。</span><span class="sxs-lookup"><span data-stu-id="5b862-227">`Timespan` to wait between attempts at polling the key vault for changes.</span></span> <span data-ttu-id="5b862-228">默認值為`null`(未重新載入配置)。</span><span class="sxs-lookup"><span data-stu-id="5b862-228">The default value is `null` (configuration isn't reloaded).</span></span> |
| `Vault`          | <span data-ttu-id="5b862-229">密鑰保管庫URI。</span><span class="sxs-lookup"><span data-stu-id="5b862-229">Key vault URI.</span></span> |

## <a name="use-a-key-name-prefix"></a><span data-ttu-id="5b862-230">使用鍵名稱前置文字</span><span class="sxs-lookup"><span data-stu-id="5b862-230">Use a key name prefix</span></span>

<span data-ttu-id="5b862-231"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>提供接受 的實現<xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>的重載,它允許您控制如何將密鑰保管庫機密轉換為設定金鑰。</span><span class="sxs-lookup"><span data-stu-id="5b862-231"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> provides an overload that accepts an implementation of <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>, which allows you to control how key vault secrets are converted into configuration keys.</span></span> <span data-ttu-id="5b862-232">例如,您可以實現介面,以基於您在應用啟動時提供的首碼值載入機密值。</span><span class="sxs-lookup"><span data-stu-id="5b862-232">For example, you can implement the interface to load secret values based on a prefix value you provide at app startup.</span></span> <span data-ttu-id="5b862-233">例如,這允許您根據應用版本載入機密。</span><span class="sxs-lookup"><span data-stu-id="5b862-233">This allows you, for example, to load secrets based on the version of the app.</span></span>

> [!WARNING]
> <span data-ttu-id="5b862-234">不要使用密鑰保管庫機密的首碼將多個應用的機密放入同一密鑰保管庫或將環境機密(例如 *,開發*與*生產*機密)放入同一保管庫。</span><span class="sxs-lookup"><span data-stu-id="5b862-234">Don't use prefixes on key vault secrets to place secrets for multiple apps into the same key vault or to place environmental secrets (for example, *development* versus *production* secrets) into the same vault.</span></span> <span data-ttu-id="5b862-235">我們建議不同的應用和開發/生產環境使用單獨的密鑰保管庫來隔離應用環境,以確保最高級別的安全性。</span><span class="sxs-lookup"><span data-stu-id="5b862-235">We recommend that different apps and development/production environments use separate key vaults to isolate app environments for the highest level of security.</span></span>

<span data-ttu-id="5b862-236">在下面的範例中,在密鑰保管庫中建立了機密(並使用開發環境的密鑰管理員工具)(`5000-AppSecret`密鑰保管庫機密名稱中不允許週期)。</span><span class="sxs-lookup"><span data-stu-id="5b862-236">In the following example, a secret is established in the key vault (and using the Secret Manager tool for the Development environment) for `5000-AppSecret` (periods aren't allowed in key vault secret names).</span></span> <span data-ttu-id="5b862-237">此機密表示應用版本 5.0.0.0 的應用機密。</span><span class="sxs-lookup"><span data-stu-id="5b862-237">This secret represents an app secret for version 5.0.0.0 of the app.</span></span> <span data-ttu-id="5b862-238">套用用的另一個版本 5.1.0.0,將機密新增到的金鑰保管庫(並使用機密管理員工具`5100-AppSecret`)中 。</span><span class="sxs-lookup"><span data-stu-id="5b862-238">For another version of the app, 5.1.0.0, a secret is added to the key vault (and using the Secret Manager tool) for `5100-AppSecret`.</span></span> <span data-ttu-id="5b862-239">每個應用版本將其版本控制的秘密值載入到其配置中`AppSecret`,作為,在載入金鑰時剝離版本。</span><span class="sxs-lookup"><span data-stu-id="5b862-239">Each app version loads its versioned secret value into its configuration as `AppSecret`, stripping off the version as it loads the secret.</span></span>

<span data-ttu-id="5b862-240"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>使用自訂<xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>呼叫 :</span><span class="sxs-lookup"><span data-stu-id="5b862-240"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> is called with a custom <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>:</span></span>

[!code-csharp[](key-vault-configuration/samples_snapshot/Program.cs)]

<span data-ttu-id="5b862-241">實現<xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>對機密的版本首碼做出反應,以將正確的機密載入到配置中:</span><span class="sxs-lookup"><span data-stu-id="5b862-241">The <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> implementation reacts to the version prefixes of secrets to load the proper secret into configuration:</span></span>

* <span data-ttu-id="5b862-242">`Load`當機密的名稱以前綴開頭時載入它。</span><span class="sxs-lookup"><span data-stu-id="5b862-242">`Load` loads a secret when its name starts with the prefix.</span></span> <span data-ttu-id="5b862-243">其他機密未載入。</span><span class="sxs-lookup"><span data-stu-id="5b862-243">Other secrets aren't loaded.</span></span>
* <span data-ttu-id="5b862-244">`GetKey`:</span><span class="sxs-lookup"><span data-stu-id="5b862-244">`GetKey`:</span></span>
  * <span data-ttu-id="5b862-245">從機密名稱中刪除前置碼。</span><span class="sxs-lookup"><span data-stu-id="5b862-245">Removes the prefix from the secret name.</span></span>
  * <span data-ttu-id="5b862-246">將任何名稱中的兩個破折號取代為`KeyDelimiter`, 是 設定中使用的分隔符(通常是冒號)。</span><span class="sxs-lookup"><span data-stu-id="5b862-246">Replaces two dashes in any name with the `KeyDelimiter`, which is the delimiter used in configuration (usually a colon).</span></span> <span data-ttu-id="5b862-247">Azure 密鑰保管庫不允許在機密名稱中冒號。</span><span class="sxs-lookup"><span data-stu-id="5b862-247">Azure Key Vault doesn't allow a colon in secret names.</span></span>

[!code-csharp[](key-vault-configuration/samples_snapshot/Startup.cs)]

<span data-ttu-id="5b862-248">該方法`Load`由提供程式演演演算法調用,該演演演算法通過保管庫機密進行遍舍以查找具有版本首碼的提供程式演演演算法。</span><span class="sxs-lookup"><span data-stu-id="5b862-248">The `Load` method is called by a provider algorithm that iterates through the vault secrets to find the ones that have the version prefix.</span></span> <span data-ttu-id="5b862-249">使用`Load`找到 版本前置碼時,演`GetKey`演演算法使用方法傳回機密名稱的配置名稱。</span><span class="sxs-lookup"><span data-stu-id="5b862-249">When a version prefix is found with `Load`, the algorithm uses the `GetKey` method to return the configuration name of the secret name.</span></span> <span data-ttu-id="5b862-250">它將版本首碼從機密的名稱中剝離,並返回用於載入到應用的配置名稱值對中的其他機密名稱。</span><span class="sxs-lookup"><span data-stu-id="5b862-250">It strips off the version prefix from the secret's name and returns the rest of the secret name for loading into the app's configuration name-value pairs.</span></span>

<span data-ttu-id="5b862-251">實作此方法時:</span><span class="sxs-lookup"><span data-stu-id="5b862-251">When this approach is implemented:</span></span>

1. <span data-ttu-id="5b862-252">應用在應用的專案檔中指定的版本。</span><span class="sxs-lookup"><span data-stu-id="5b862-252">The app's version specified in the app's project file.</span></span> <span data-ttu-id="5b862-253">在下面的範例中,套用的版本設定`5.0.0.0`為 :</span><span class="sxs-lookup"><span data-stu-id="5b862-253">In the following example, the app's version is set to `5.0.0.0`:</span></span>

   ```xml
   <PropertyGroup>
     <Version>5.0.0.0</Version>
   </PropertyGroup>
   ```

1. <span data-ttu-id="5b862-254">確認應用的項目`<UserSecretsId>`檔案中存在屬性,`{GUID}`其中 是使用者提供的 GUID:</span><span class="sxs-lookup"><span data-stu-id="5b862-254">Confirm that a `<UserSecretsId>` property is present in the app's project file, where `{GUID}` is a user-supplied GUID:</span></span>

   ```xml
   <PropertyGroup>
     <UserSecretsId>{GUID}</UserSecretsId>
   </PropertyGroup>
   ```

   <span data-ttu-id="5b862-255">[使用秘密管理員工具](xref:security/app-secrets)在本地儲存以下機密:</span><span class="sxs-lookup"><span data-stu-id="5b862-255">Save the following secrets locally with the [Secret Manager tool](xref:security/app-secrets):</span></span>

   ```dotnetcli
   dotnet user-secrets set "5000-AppSecret" "5.0.0.0_secret_value_dev"
   dotnet user-secrets set "5100-AppSecret" "5.1.0.0_secret_value_dev"
   ```

1. <span data-ttu-id="5b862-256">機密使用以下 Azure CLI 命令儲存在 Azure 金鑰保管庫中:</span><span class="sxs-lookup"><span data-stu-id="5b862-256">Secrets are saved in Azure Key Vault using the following Azure CLI commands:</span></span>

   ```azurecli
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "5000-AppSecret" --value "5.0.0.0_secret_value_prod"
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "5100-AppSecret" --value "5.1.0.0_secret_value_prod"
   ```

1. <span data-ttu-id="5b862-257">運行應用時,將載入金鑰保管庫機密。</span><span class="sxs-lookup"><span data-stu-id="5b862-257">When the app is run, the key vault secrets are loaded.</span></span> <span data-ttu-id="5b862-258">的`5000-AppSecret`字串機密與應用的專案檔 ()`5.0.0.0`中指定的應用版本匹配。</span><span class="sxs-lookup"><span data-stu-id="5b862-258">The string secret for `5000-AppSecret` is matched to the app's version specified in the app's project file (`5.0.0.0`).</span></span>

1. <span data-ttu-id="5b862-259">版本`5000`(使用破折號)從密鑰名稱中剝離。</span><span class="sxs-lookup"><span data-stu-id="5b862-259">The version, `5000` (with the dash), is stripped from the key name.</span></span> <span data-ttu-id="5b862-260">在整個應用中,使用密鑰`AppSecret`讀取配置將載入機密值。</span><span class="sxs-lookup"><span data-stu-id="5b862-260">Throughout the app, reading configuration with the key `AppSecret` loads the secret value.</span></span>

1. <span data-ttu-id="5b862-261">如果在專案檔中將應用的版本更改為,`5.1.0.0`並且應用再次運行,則傳回的機密`5.1.0.0_secret_value_dev`值位於 「開發`5.1.0.0_secret_value_prod`」環境和 「生產」 中。</span><span class="sxs-lookup"><span data-stu-id="5b862-261">If the app's version is changed in the project file to `5.1.0.0` and the app is run again, the secret value returned is `5.1.0.0_secret_value_dev` in the Development environment and `5.1.0.0_secret_value_prod` in Production.</span></span>

> [!NOTE]
> <span data-ttu-id="5b862-262">您還可以向<xref:Microsoft.Azure.KeyVault.KeyVaultClient><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>提供您自己的實現。</span><span class="sxs-lookup"><span data-stu-id="5b862-262">You can also provide your own <xref:Microsoft.Azure.KeyVault.KeyVaultClient> implementation to <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>.</span></span> <span data-ttu-id="5b862-263">自定義客戶端允許跨應用共用用戶端的單個實例。</span><span class="sxs-lookup"><span data-stu-id="5b862-263">A custom client permits sharing a single instance of the client across the app.</span></span>

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="5b862-264">將陣列繫結到類別</span><span class="sxs-lookup"><span data-stu-id="5b862-264">Bind an array to a class</span></span>

<span data-ttu-id="5b862-265">提供程式能夠將配置值讀取到陣列中以綁定到 POCO 陣列。</span><span class="sxs-lookup"><span data-stu-id="5b862-265">The provider is capable of reading configuration values into an array for binding to a POCO array.</span></span>

<span data-ttu-id="5b862-266">從允許鍵包含冒號`:`() 分隔符的設定源讀取時,使用數字鍵段來區分組成陣`:0:`列`:1:`&hellip;`:{n}:`( 的鍵 ) 。</span><span class="sxs-lookup"><span data-stu-id="5b862-266">When reading from a configuration source that allows keys to contain colon (`:`) separators, a numeric key segment is used to distinguish the keys that make up an array (`:0:`, `:1:`, &hellip; `:{n}:`).</span></span> <span data-ttu-id="5b862-267">有關詳細資訊,請參閱[設定:將陣列綁定到類](xref:fundamentals/configuration/index#bind-an-array-to-a-class)。</span><span class="sxs-lookup"><span data-stu-id="5b862-267">For more information, see [Configuration: Bind an array to a class](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span></span>

<span data-ttu-id="5b862-268">Azure 密鑰保管庫密鑰不能將冒號用作分隔符。</span><span class="sxs-lookup"><span data-stu-id="5b862-268">Azure Key Vault keys can't use a colon as a separator.</span></span> <span data-ttu-id="5b862-269">本主題中描述的方法使用雙破折號 ()`--`作為分層值(節)的分隔符。</span><span class="sxs-lookup"><span data-stu-id="5b862-269">The approach described in this topic uses double dashes (`--`) as a separator for hierarchical values (sections).</span></span> <span data-ttu-id="5b862-270">陣列鍵儲存在 Azure 金鑰保管庫中,具有雙破折號和數位`--0--``--1--`鍵段&hellip;`--{n}--`(, 。</span><span class="sxs-lookup"><span data-stu-id="5b862-270">Array keys are stored in Azure Key Vault with double dashes and numeric key segments (`--0--`, `--1--`, &hellip; `--{n}--`).</span></span>

<span data-ttu-id="5b862-271">檢查 JSON 檔提供的以下[Serilog](https://serilog.net/)記錄提供程式設定。</span><span class="sxs-lookup"><span data-stu-id="5b862-271">Examine the following [Serilog](https://serilog.net/) logging provider configuration provided by a JSON file.</span></span> <span data-ttu-id="5b862-272">`WriteTo`陣列中定義了兩個物件文本,反映了兩個 Serilog*接收器*,它們描述日誌記錄輸出的目標:</span><span class="sxs-lookup"><span data-stu-id="5b862-272">There are two object literals defined in the `WriteTo` array that reflect two Serilog *sinks*, which describe destinations for logging output:</span></span>

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

<span data-ttu-id="5b862-273">前面 JSON 檔中顯示的設定使用雙破折號`--`() 表示法和數位段儲存在 Azure 金鑰保管庫中:</span><span class="sxs-lookup"><span data-stu-id="5b862-273">The configuration shown in the preceding JSON file is stored in Azure Key Vault using double dash (`--`) notation and numeric segments:</span></span>

| <span data-ttu-id="5b862-274">Key</span><span class="sxs-lookup"><span data-stu-id="5b862-274">Key</span></span> | <span data-ttu-id="5b862-275">值</span><span class="sxs-lookup"><span data-stu-id="5b862-275">Value</span></span> |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="reload-secrets"></a><span data-ttu-id="5b862-276">重新載入機密</span><span class="sxs-lookup"><span data-stu-id="5b862-276">Reload secrets</span></span>

<span data-ttu-id="5b862-277">機密被緩存,直到`IConfigurationRoot.Reload()`被調用。</span><span class="sxs-lookup"><span data-stu-id="5b862-277">Secrets are cached until `IConfigurationRoot.Reload()` is called.</span></span> <span data-ttu-id="5b862-278">在執行密鑰保管庫之前`Reload`,應用不會尊重金鑰保管庫中過期、禁用和更新的機密。</span><span class="sxs-lookup"><span data-stu-id="5b862-278">Expired, disabled, and updated secrets in the key vault are not respected by the app until `Reload` is executed.</span></span>

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a><span data-ttu-id="5b862-279">已關閉和過期的機密</span><span class="sxs-lookup"><span data-stu-id="5b862-279">Disabled and expired secrets</span></span>

<span data-ttu-id="5b862-280">關閉與過期的機密引發<xref:Microsoft.Azure.KeyVault.Models.KeyVaultErrorException>。</span><span class="sxs-lookup"><span data-stu-id="5b862-280">Disabled and expired secrets throw a <xref:Microsoft.Azure.KeyVault.Models.KeyVaultErrorException>.</span></span> <span data-ttu-id="5b862-281">要防止應用引發,請使用其他配置提供程式提供配置或更新禁用或過期的機密。</span><span class="sxs-lookup"><span data-stu-id="5b862-281">To prevent the app from throwing, provide the configuration using a different configuration provider or update the disabled or expired secret.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="5b862-282">疑難排解</span><span class="sxs-lookup"><span data-stu-id="5b862-282">Troubleshoot</span></span>

<span data-ttu-id="5b862-283">當應用無法使用提供程式載入配置時,將寫入[ASP.NET 核心紀錄記錄基礎結構](xref:fundamentals/logging/index)中的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="5b862-283">When the app fails to load configuration using the provider, an error message is written to the [ASP.NET Core Logging infrastructure](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="5b862-284">以下條件將阻止載入設定:</span><span class="sxs-lookup"><span data-stu-id="5b862-284">The following conditions will prevent configuration from loading:</span></span>

* <span data-ttu-id="5b862-285">應用或證書未在 Azure 活動目錄中正確配置。</span><span class="sxs-lookup"><span data-stu-id="5b862-285">The app or certificate isn't configured correctly in Azure Active Directory.</span></span>
* <span data-ttu-id="5b862-286">密鑰保管庫在 Azure 密鑰保管庫中不存在。</span><span class="sxs-lookup"><span data-stu-id="5b862-286">The key vault doesn't exist in Azure Key Vault.</span></span>
* <span data-ttu-id="5b862-287">應用程式無權訪問密鑰保管庫。</span><span class="sxs-lookup"><span data-stu-id="5b862-287">The app isn't authorized to access the key vault.</span></span>
* <span data-ttu-id="5b862-288">訪問策略不包括`Get`和`List`許可權。</span><span class="sxs-lookup"><span data-stu-id="5b862-288">The access policy doesn't include `Get` and `List` permissions.</span></span>
* <span data-ttu-id="5b862-289">在金鑰保管庫中,配置數據(名稱值對)被錯誤地命名、丟失、禁用或過期。</span><span class="sxs-lookup"><span data-stu-id="5b862-289">In the key vault, the configuration data (name-value pair) is incorrectly named, missing, disabled, or expired.</span></span>
* <span data-ttu-id="5b862-290">應用具有錯誤的金鑰保管庫名稱`KeyVaultName`( )、Azure AD`AzureADApplicationId`應用程式`AzureADCertThumbprint`ID () 或 Azure AD 證書指紋 ()。</span><span class="sxs-lookup"><span data-stu-id="5b862-290">The app has the wrong key vault name (`KeyVaultName`), Azure AD Application Id (`AzureADApplicationId`), or Azure AD certificate thumbprint (`AzureADCertThumbprint`).</span></span>
* <span data-ttu-id="5b862-291">對於要載入的值,配置鍵(名稱)在應用中不正確。</span><span class="sxs-lookup"><span data-stu-id="5b862-291">The configuration key (name) is incorrect in the app for the value you're trying to load.</span></span>
* <span data-ttu-id="5b862-292">將應用的存取策略添加到密鑰保管庫時,將創建該策略,但在**Access 策略**UI 中未選擇 **「儲存**」按鈕。</span><span class="sxs-lookup"><span data-stu-id="5b862-292">When adding the access policy for the app to the key vault, the policy was created, but the **Save** button wasn't selected in the **Access policies** UI.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5b862-293">其他資源</span><span class="sxs-lookup"><span data-stu-id="5b862-293">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* [<span data-ttu-id="5b862-294">微軟 Azure:金鑰保管庫</span><span class="sxs-lookup"><span data-stu-id="5b862-294">Microsoft Azure: Key Vault</span></span>](https://azure.microsoft.com/services/key-vault/)
* [<span data-ttu-id="5b862-295">微軟 Azure:金鑰保管庫文件</span><span class="sxs-lookup"><span data-stu-id="5b862-295">Microsoft Azure: Key Vault Documentation</span></span>](/azure/key-vault/)
* [<span data-ttu-id="5b862-296">如何為 Azure 金鑰保存庫產生並傳輸受 HSM 保護的金鑰</span><span class="sxs-lookup"><span data-stu-id="5b862-296">How to generate and transfer HSM-protected keys for Azure Key Vault</span></span>](/azure/key-vault/key-vault-hsm-protected-keys)
* [<span data-ttu-id="5b862-297">金鑰庫用戶端類別</span><span class="sxs-lookup"><span data-stu-id="5b862-297">KeyVaultClient Class</span></span>](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
* [<span data-ttu-id="5b862-298">快速入門：使用 .NET Web 應用程式從 Azure Key Vault 設定及擷取祕密</span><span class="sxs-lookup"><span data-stu-id="5b862-298">Quickstart: Set and retrieve a secret from Azure Key Vault by using a .NET web app</span></span>](/azure/key-vault/quick-create-net)
* [<span data-ttu-id="5b862-299">教學課程：如何在 .NET 中搭配使用 Azure Key Vault 與 Azure Windows 虛擬機器</span><span class="sxs-lookup"><span data-stu-id="5b862-299">Tutorial: How to use Azure Key Vault with Azure Windows Virtual Machine in .NET</span></span>](/azure/key-vault/tutorial-net-windows-virtual-machine)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="5b862-300">本文件介紹如何使用[Microsoft Azure 密鑰保管庫](https://azure.microsoft.com/services/key-vault/)配置提供程式從 Azure 密鑰保管庫機密載入應用配置值。</span><span class="sxs-lookup"><span data-stu-id="5b862-300">This document explains how to use the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) Configuration Provider to load app configuration values from Azure Key Vault secrets.</span></span> <span data-ttu-id="5b862-301">Azure 密鑰保管庫是一種基於雲的服務,可幫助保護應用和服務使用的加密密鑰和機密。</span><span class="sxs-lookup"><span data-stu-id="5b862-301">Azure Key Vault is a cloud-based service that assists in safeguarding cryptographic keys and secrets used by apps and services.</span></span> <span data-ttu-id="5b862-302">將 Azure 金鑰保管庫用於 ASP.NET核心應用的常見方案包括:</span><span class="sxs-lookup"><span data-stu-id="5b862-302">Common scenarios for using Azure Key Vault with ASP.NET Core apps include:</span></span>

* <span data-ttu-id="5b862-303">控制對敏感配置數據的訪問。</span><span class="sxs-lookup"><span data-stu-id="5b862-303">Controlling access to sensitive configuration data.</span></span>
* <span data-ttu-id="5b862-304">在儲存配置資料時滿足 FIPS 140-2 級驗證的硬體安全模組 (HSM) 的要求。</span><span class="sxs-lookup"><span data-stu-id="5b862-304">Meeting the requirement for FIPS 140-2 Level 2 validated Hardware Security Modules (HSM's) when storing configuration data.</span></span>

<span data-ttu-id="5b862-305">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/samples)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5b862-305">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="packages"></a><span data-ttu-id="5b862-306">Packages</span><span class="sxs-lookup"><span data-stu-id="5b862-306">Packages</span></span>

<span data-ttu-id="5b862-307">向[Microsoft.擴展.配置.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/)包添加包引用。</span><span class="sxs-lookup"><span data-stu-id="5b862-307">Add a package reference to the [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) package.</span></span>

## <a name="sample-app"></a><span data-ttu-id="5b862-308">範例應用程式</span><span class="sxs-lookup"><span data-stu-id="5b862-308">Sample app</span></span>

<span data-ttu-id="5b862-309">範例應用以`#define`*Program.cs*檔案頂部的語句確定的兩種模式之一運行:</span><span class="sxs-lookup"><span data-stu-id="5b862-309">The sample app runs in either of two modes determined by the `#define` statement at the top of the *Program.cs* file:</span></span>

* <span data-ttu-id="5b862-310">`Certificate`&ndash;演示使用 Azure 金鑰保管庫客戶端 ID 和 X.509 憑證存取儲存在 Azure 密鑰保管庫中的秘密。</span><span class="sxs-lookup"><span data-stu-id="5b862-310">`Certificate` &ndash; Demonstrates the use of an Azure Key Vault Client ID and X.509 certificate to access secrets stored in Azure Key Vault.</span></span> <span data-ttu-id="5b862-311">此版本的示例可以從任何位置運行,部署到 Azure 應用服務或任何能夠為 ASP.NET 酷應用提供服務的主機。</span><span class="sxs-lookup"><span data-stu-id="5b862-311">This version of the sample can be run from any location, deployed to Azure App Service or any host capable of serving an ASP.NET Core app.</span></span>
* <span data-ttu-id="5b862-312">`Managed`&ndash;演示如何[使用 Azure 資源的託管識別](/azure/active-directory/managed-identities-azure-resources/overview)使用 Azure AD 身份驗證將應用驗證為 Azure 密鑰保管庫,而無需在應用的代碼或配置中儲存認證。</span><span class="sxs-lookup"><span data-stu-id="5b862-312">`Managed` &ndash; Demonstrates how to use [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview) to authenticate the app to Azure Key Vault with Azure AD authentication without credentials stored in the app's code or configuration.</span></span> <span data-ttu-id="5b862-313">使用託管標識進行身份驗證時,不需要 Azure AD 應用程式 ID 和密碼(用戶端密鑰)。</span><span class="sxs-lookup"><span data-stu-id="5b862-313">When using managed identities to authenticate, an Azure AD Application ID and Password (Client Secret) aren't required.</span></span> <span data-ttu-id="5b862-314">必須`Managed`將示例的版本部署到 Azure。</span><span class="sxs-lookup"><span data-stu-id="5b862-314">The `Managed` version of the sample must be deployed to Azure.</span></span> <span data-ttu-id="5b862-315">按照"[為 Azure 資源使用託管標識](#use-managed-identities-for-azure-resources)「部分中的指南進行操作。</span><span class="sxs-lookup"><span data-stu-id="5b862-315">Follow the guidance in the [Use the Managed identities for Azure resources](#use-managed-identities-for-azure-resources) section.</span></span>

<span data-ttu-id="5b862-316">有關如何使用預處理器指令 ()`#define`設定範例應用程式的詳細資訊,請<xref:index#preprocessor-directives-in-sample-code>參閱 。</span><span class="sxs-lookup"><span data-stu-id="5b862-316">For more information on how to configure a sample app using preprocessor directives (`#define`), see <xref:index#preprocessor-directives-in-sample-code>.</span></span>

## <a name="secret-storage-in-the-development-environment"></a><span data-ttu-id="5b862-317">開發環境中的秘密儲存</span><span class="sxs-lookup"><span data-stu-id="5b862-317">Secret storage in the Development environment</span></span>

<span data-ttu-id="5b862-318">使用[機密管理器工具](xref:security/app-secrets)在本地設置機密。</span><span class="sxs-lookup"><span data-stu-id="5b862-318">Set secrets locally using the [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="5b862-319">當示例應用在開發環境中的本地電腦上運行時,將從本地機密管理器儲存載入機密。</span><span class="sxs-lookup"><span data-stu-id="5b862-319">When the sample app runs on the local machine in the Development environment, secrets are loaded from the local Secret Manager store.</span></span>

<span data-ttu-id="5b862-320">「機密管理員」工具需要`<UserSecretsId>`應用的專案檔中的屬性。</span><span class="sxs-lookup"><span data-stu-id="5b862-320">The Secret Manager tool requires a `<UserSecretsId>` property in the app's project file.</span></span> <span data-ttu-id="5b862-321">將屬性值`{GUID}`( ) 設定為任何唯一的 GUID:</span><span class="sxs-lookup"><span data-stu-id="5b862-321">Set the property value (`{GUID}`) to any unique GUID:</span></span>

```xml
<PropertyGroup>
  <UserSecretsId>{GUID}</UserSecretsId>
</PropertyGroup>
```

<span data-ttu-id="5b862-322">機密創建為名稱值對。</span><span class="sxs-lookup"><span data-stu-id="5b862-322">Secrets are created as name-value pairs.</span></span> <span data-ttu-id="5b862-323">層次結構值(配置部分)在`:`[ASP.NET核心配置](xref:fundamentals/configuration/index)密鑰名稱中使用(冒號)作為分隔符。</span><span class="sxs-lookup"><span data-stu-id="5b862-323">Hierarchical values (configuration sections) use a `:` (colon) as a separator in [ASP.NET Core configuration](xref:fundamentals/configuration/index) key names.</span></span>

<span data-ttu-id="5b862-324">機密管理員從開啟到項目[內容根](xref:fundamentals/index#content-root)的命令外殼使用`{SECRET NAME}`,其中的`{SECRET VALUE}`名稱和 值是:</span><span class="sxs-lookup"><span data-stu-id="5b862-324">The Secret Manager is used from a command shell opened to the project's [content root](xref:fundamentals/index#content-root), where `{SECRET NAME}` is the name and `{SECRET VALUE}` is the value:</span></span>

```dotnetcli
dotnet user-secrets set "{SECRET NAME}" "{SECRET VALUE}"
```

<span data-ttu-id="5b862-325">從項目[的內容根](xref:fundamentals/index#content-root)在命令 shell 中執行以下指令,以設定範例的祕密:</span><span class="sxs-lookup"><span data-stu-id="5b862-325">Execute the following commands in a command shell from the project's [content root](xref:fundamentals/index#content-root) to set the secrets for the sample app:</span></span>

```dotnetcli
dotnet user-secrets set "SecretName" "secret_value_1_dev"
dotnet user-secrets set "Section:SecretName" "secret_value_2_dev"
```

<span data-ttu-id="5b862-326">當這些機密儲存在[Azure 金鑰保管庫部分的生產環境中的「機密記憶體」中的](#secret-storage-in-the-production-environment-with-azure-key-vault)Azure`_dev`金鑰保管 庫中`_prod`時,後綴將更改為 。</span><span class="sxs-lookup"><span data-stu-id="5b862-326">When these secrets are stored in Azure Key Vault in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section, the `_dev` suffix is changed to `_prod`.</span></span> <span data-ttu-id="5b862-327">後綴在應用的輸出中提供指示配置值來源的可視提示。</span><span class="sxs-lookup"><span data-stu-id="5b862-327">The suffix provides a visual cue in the app's output indicating the source of the configuration values.</span></span>

## <a name="secret-storage-in-the-production-environment-with-azure-key-vault"></a><span data-ttu-id="5b862-328">使用 Azure 金鑰保存式庫的生產環境</span><span class="sxs-lookup"><span data-stu-id="5b862-328">Secret storage in the Production environment with Azure Key Vault</span></span>

<span data-ttu-id="5b862-329">此處匯總了[「快速入門:使用 Azure CLI」主題從 Azure 密鑰保管庫設置和檢索機密](/azure/key-vault/quick-create-cli)的說明,用於創建 Azure 密鑰保管庫並儲存範例應用使用的秘密。</span><span class="sxs-lookup"><span data-stu-id="5b862-329">The instructions provided by the [Quickstart: Set and retrieve a secret from Azure Key Vault using Azure CLI](/azure/key-vault/quick-create-cli) topic are summarized here for creating an Azure Key Vault and storing secrets used by the sample app.</span></span> <span data-ttu-id="5b862-330">有關詳細資訊,請參閱主題。</span><span class="sxs-lookup"><span data-stu-id="5b862-330">Refer to the topic for further details.</span></span>

1. <span data-ttu-id="5b862-331">使用[Azure 門戶](https://portal.azure.com/)中的以下任一方法打開 Azure 雲外殼:</span><span class="sxs-lookup"><span data-stu-id="5b862-331">Open Azure Cloud shell using any one of the following methods in the [Azure portal](https://portal.azure.com/):</span></span>

   * <span data-ttu-id="5b862-332">選取程式碼區塊右上角的 [試試看]  。</span><span class="sxs-lookup"><span data-stu-id="5b862-332">Select **Try It** in the upper-right corner of a code block.</span></span> <span data-ttu-id="5b862-333">在文字框中使用搜索字串「Azure CLI」。。</span><span class="sxs-lookup"><span data-stu-id="5b862-333">Use the search string "Azure CLI" in the text box.</span></span>
   * <span data-ttu-id="5b862-334">使用**啟動雲外殼**按鈕在瀏覽器中打開雲外殼。</span><span class="sxs-lookup"><span data-stu-id="5b862-334">Open Cloud Shell in your browser with the **Launch Cloud Shell** button.</span></span>
   * <span data-ttu-id="5b862-335">選取 Azure 入口網站右上角功能表上的 **[Cloud Shell]** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="5b862-335">Select the **Cloud Shell** button on the menu in the upper-right corner of the Azure portal.</span></span>

   <span data-ttu-id="5b862-336">有關詳細資訊,請參閱[Azure CLI](/cli/azure/)和[Azure 雲外殼概述](/azure/cloud-shell/overview)。</span><span class="sxs-lookup"><span data-stu-id="5b862-336">For more information, see [Azure CLI](/cli/azure/) and [Overview of Azure Cloud Shell](/azure/cloud-shell/overview).</span></span>

1. <span data-ttu-id="5b862-337">如果您尚未通過身份驗證,請使用命令`az login`登錄。</span><span class="sxs-lookup"><span data-stu-id="5b862-337">If you aren't already authenticated, sign in with the `az login` command.</span></span>

1. <span data-ttu-id="5b862-338">使用以下指令建立資源群組,其中`{RESOURCE GROUP NAME}`為新資源群組的資源組名`{LOCATION}`稱為 Azure 區域(資料中心):</span><span class="sxs-lookup"><span data-stu-id="5b862-338">Create a resource group with the following command, where `{RESOURCE GROUP NAME}` is the resource group name for the new resource group and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```azurecli
   az group create --name "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="5b862-339">使用以下指令在資源群組中建立金鑰保管庫,其中`{KEY VAULT NAME}`為新密鑰保管庫的名稱`{LOCATION}`, 是 Azure 區域(資料中心):</span><span class="sxs-lookup"><span data-stu-id="5b862-339">Create a key vault in the resource group with the following command, where `{KEY VAULT NAME}` is the name for the new key vault and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```azurecli
   az keyvault create --name {KEY VAULT NAME} --resource-group "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="5b862-340">在密鑰保管庫中創建機密作為名稱值對。</span><span class="sxs-lookup"><span data-stu-id="5b862-340">Create secrets in the key vault as name-value pairs.</span></span>

   <span data-ttu-id="5b862-341">Azure 密鑰保管庫金鑰庫密鑰名稱僅限於字母數位字元和破折號。</span><span class="sxs-lookup"><span data-stu-id="5b862-341">Azure Key Vault secret names are limited to alphanumeric characters and dashes.</span></span> <span data-ttu-id="5b862-342">階層值(配置部分)使用`--`(兩個破折號)作為分隔符。</span><span class="sxs-lookup"><span data-stu-id="5b862-342">Hierarchical values (configuration sections) use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="5b862-343">在密鑰保管庫密鑰名稱中,通常不允許從[ASP.NET Core 配置](xref:fundamentals/configuration/index)中從子鍵中分隔節的冒號。</span><span class="sxs-lookup"><span data-stu-id="5b862-343">Colons, which are normally used to delimit a section from a subkey in [ASP.NET Core configuration](xref:fundamentals/configuration/index), aren't allowed in key vault secret names.</span></span> <span data-ttu-id="5b862-344">因此,當機密載入應用的配置中時,將使用兩個破折號並將其交換為冒號。</span><span class="sxs-lookup"><span data-stu-id="5b862-344">Therefore, two dashes are used and swapped for a colon when the secrets are loaded into the app's configuration.</span></span>

   <span data-ttu-id="5b862-345">以下機密用於示例應用。</span><span class="sxs-lookup"><span data-stu-id="5b862-345">The following secrets are for use with the sample app.</span></span> <span data-ttu-id="5b862-346">這些值包括後`_prod`綴,用於將它們與在開發環境`_dev`中 載入的後綴值與使用者機密區分開來。</span><span class="sxs-lookup"><span data-stu-id="5b862-346">The values include a `_prod` suffix to distinguish them from the `_dev` suffix values loaded in the Development environment from User Secrets.</span></span> <span data-ttu-id="5b862-347">取代為`{KEY VAULT NAME}`在上一步驟中建立的金鑰保管庫的名稱:</span><span class="sxs-lookup"><span data-stu-id="5b862-347">Replace `{KEY VAULT NAME}` with the name of the key vault that you created in the prior step:</span></span>

   ```azurecli
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "SecretName" --value "secret_value_1_prod"
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "Section--SecretName" --value "secret_value_2_prod"
   ```

## <a name="use-application-id-and-x509-certificate-for-non-azure-hosted-apps"></a><span data-ttu-id="5b862-348">對非 Azure 託管應用使用應用程式 ID 和 X.509 憑證</span><span class="sxs-lookup"><span data-stu-id="5b862-348">Use Application ID and X.509 certificate for non-Azure-hosted apps</span></span>

<span data-ttu-id="5b862-349">將 Azure AD、Azure 密鑰保管庫和應用配置為使用 Azure 活動目錄應用程式 ID 和 X.509 證書在**應用託管在 Azure 之外時**對金鑰保管庫進行身份驗證。</span><span class="sxs-lookup"><span data-stu-id="5b862-349">Configure Azure AD, Azure Key Vault, and the app to use an Azure Active Directory Application ID and X.509 certificate to authenticate to a key vault **when the app is hosted outside of Azure**.</span></span> <span data-ttu-id="5b862-350">如需詳細資訊，請參閱[關於金鑰、祕密和憑證](/azure/key-vault/about-keys-secrets-and-certificates)。</span><span class="sxs-lookup"><span data-stu-id="5b862-350">For more information, see [About keys, secrets, and certificates](/azure/key-vault/about-keys-secrets-and-certificates).</span></span>

> [!NOTE]
> <span data-ttu-id="5b862-351">儘管 Azure 中託管的應用支援使用應用程式 ID 和 X.509 憑證,但我們建議在 Azure 中託管應用時[對 Azure 資源使用託管標識](#use-managed-identities-for-azure-resources)。</span><span class="sxs-lookup"><span data-stu-id="5b862-351">Although using an Application ID and X.509 certificate is supported for apps hosted in Azure, we recommend using [Managed identities for Azure resources](#use-managed-identities-for-azure-resources) when hosting an app in Azure.</span></span> <span data-ttu-id="5b862-352">託管標識不需要在應用或開發環境中存儲證書。</span><span class="sxs-lookup"><span data-stu-id="5b862-352">Managed identities don't require storing a certificate in the app or in the development environment.</span></span>

<span data-ttu-id="5b862-353">當*Program.cs*檔案頂部的語句`Certificate`設定為`#define`時, 範例應用程式使用應用程式 ID 和 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="5b862-353">The sample app uses an Application ID and X.509 certificate when the `#define` statement at the top of the *Program.cs* file is set to `Certificate`.</span></span>

1. <span data-ttu-id="5b862-354">創建 PKCS_12 存檔 (*.pfx*) 證書。</span><span class="sxs-lookup"><span data-stu-id="5b862-354">Create a PKCS#12 archive (*.pfx*) certificate.</span></span> <span data-ttu-id="5b862-355">建立憑證的選項包括[Windows 上的 MakeCert](/windows/desktop/seccrypto/makecert)與[OpenSSL](https://www.openssl.org/)。</span><span class="sxs-lookup"><span data-stu-id="5b862-355">Options for creating certificates include [MakeCert on Windows](/windows/desktop/seccrypto/makecert) and [OpenSSL](https://www.openssl.org/).</span></span>
1. <span data-ttu-id="5b862-356">將憑證安裝到當前使用者的個人憑證儲存中。</span><span class="sxs-lookup"><span data-stu-id="5b862-356">Install the certificate into the current user's personal certificate store.</span></span> <span data-ttu-id="5b862-357">將密鑰標記為可匯出是可選的。</span><span class="sxs-lookup"><span data-stu-id="5b862-357">Marking the key as exportable is optional.</span></span> <span data-ttu-id="5b862-358">請注意證書的指紋,此過程稍後將使用。</span><span class="sxs-lookup"><span data-stu-id="5b862-358">Note the certificate's thumbprint, which is used later in this process.</span></span>
1. <span data-ttu-id="5b862-359">將 PKCS_12 存檔 *(.pfx*) 證書匯出為 DER 編碼證書 *(.cer*)。</span><span class="sxs-lookup"><span data-stu-id="5b862-359">Export the PKCS#12 archive (*.pfx*) certificate as a DER-encoded certificate (*.cer*).</span></span>
1. <span data-ttu-id="5b862-360">使用 Azure AD(**應用註冊)註冊**應用。</span><span class="sxs-lookup"><span data-stu-id="5b862-360">Register the app with Azure AD (**App registrations**).</span></span>
1. <span data-ttu-id="5b862-361">將 DER 編碼的憑證 *(.cer*) 上傳到 Azure AD:</span><span class="sxs-lookup"><span data-stu-id="5b862-361">Upload the DER-encoded certificate (*.cer*) to Azure AD:</span></span>
   1. <span data-ttu-id="5b862-362">在 Azure AD 中選擇應用。</span><span class="sxs-lookup"><span data-stu-id="5b862-362">Select the app in Azure AD.</span></span>
   1. <span data-ttu-id="5b862-363">導覽到**憑證&機密**。</span><span class="sxs-lookup"><span data-stu-id="5b862-363">Navigate to **Certificates & secrets**.</span></span>
   1. <span data-ttu-id="5b862-364">選擇 **「上載證書**」以上載包含公開金鑰的憑證。</span><span class="sxs-lookup"><span data-stu-id="5b862-364">Select **Upload certificate** to upload the certificate, which contains the public key.</span></span> <span data-ttu-id="5b862-365">*.cer* *、.pem*或 *.crt*證書是可以接受的。</span><span class="sxs-lookup"><span data-stu-id="5b862-365">A *.cer*, *.pem*, or *.crt* certificate is acceptable.</span></span>
1. <span data-ttu-id="5b862-366">將金鑰保管庫名稱、應用程式 ID 和證書指紋存儲在應用*的應用設置.json*檔中。</span><span class="sxs-lookup"><span data-stu-id="5b862-366">Store the key vault name, Application ID, and certificate thumbprint in the app's *appsettings.json* file.</span></span>
1. <span data-ttu-id="5b862-367">瀏覽到 Azure 門戶中的**金鑰保管庫**。</span><span class="sxs-lookup"><span data-stu-id="5b862-367">Navigate to **Key vaults** in the Azure portal.</span></span>
1. <span data-ttu-id="5b862-368">[使用 Azure 金鑰保管庫部分在「生產環境」 中的秘密儲存中選擇在「機密存儲」 中](#secret-storage-in-the-production-environment-with-azure-key-vault)創建的金鑰保管庫。</span><span class="sxs-lookup"><span data-stu-id="5b862-368">Select the key vault that you created in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section.</span></span>
1. <span data-ttu-id="5b862-369">選擇**存取原則**。</span><span class="sxs-lookup"><span data-stu-id="5b862-369">Select **Access policies**.</span></span>
1. <span data-ttu-id="5b862-370">選擇 **「添加存取策略**」。</span><span class="sxs-lookup"><span data-stu-id="5b862-370">Select **Add Access Policy**.</span></span>
1. <span data-ttu-id="5b862-371">打開 **「機密」許可權**,並提供具有**獲取**和**列表**許可權的應用。</span><span class="sxs-lookup"><span data-stu-id="5b862-371">Open **Secret permissions** and provide the app with **Get** and **List** permissions.</span></span>
1. <span data-ttu-id="5b862-372">選擇 **「選擇主體**」並按名稱選擇已註冊的應用。</span><span class="sxs-lookup"><span data-stu-id="5b862-372">Select **Select principal** and select the registered app by name.</span></span> <span data-ttu-id="5b862-373">選取 [選取]  按鈕。</span><span class="sxs-lookup"><span data-stu-id="5b862-373">Select the **Select** button.</span></span>
1. <span data-ttu-id="5b862-374">選取 [確定]  。</span><span class="sxs-lookup"><span data-stu-id="5b862-374">Select **OK**.</span></span>
1. <span data-ttu-id="5b862-375">選取 [儲存]  。</span><span class="sxs-lookup"><span data-stu-id="5b862-375">Select **Save**.</span></span>
1. <span data-ttu-id="5b862-376">部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="5b862-376">Deploy the app.</span></span>

<span data-ttu-id="5b862-377">範例`Certificate`套`IConfigurationRoot`用與機密名稱同名取得其設定值:</span><span class="sxs-lookup"><span data-stu-id="5b862-377">The `Certificate` sample app obtains its configuration values from `IConfigurationRoot` with the same name as the secret name:</span></span>

* <span data-ttu-id="5b862-378">非階層值:使用 取得`SecretName`的值`config["SecretName"]`。</span><span class="sxs-lookup"><span data-stu-id="5b862-378">Non-hierarchical values: The value for `SecretName` is obtained with `config["SecretName"]`.</span></span>
* <span data-ttu-id="5b862-379">分層值(節):使用`:`(冒號)表示法`GetSection`或 擴展方法。</span><span class="sxs-lookup"><span data-stu-id="5b862-379">Hierarchical values (sections): Use `:` (colon) notation or the `GetSection` extension method.</span></span> <span data-ttu-id="5b862-380">使用以下任一方法取得設定值值:</span><span class="sxs-lookup"><span data-stu-id="5b862-380">Use either of these approaches to obtain the configuration value:</span></span>
  * `config["Section:SecretName"]`
  * `config.GetSection("Section")["SecretName"]`

<span data-ttu-id="5b862-381">X.509 證書由操作系統管理。</span><span class="sxs-lookup"><span data-stu-id="5b862-381">The X.509 certificate is managed by the OS.</span></span> <span data-ttu-id="5b862-382">套用套<xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>用*應用程式設定.json*檔提供的值:</span><span class="sxs-lookup"><span data-stu-id="5b862-382">The app calls <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> with values supplied by the *appsettings.json* file:</span></span>

[!code-csharp[](key-vault-configuration/samples/2.x/SampleApp/Program.cs?name=snippet1&highlight=20-23)]

<span data-ttu-id="5b862-383">範例值：</span><span class="sxs-lookup"><span data-stu-id="5b862-383">Example values:</span></span>

* <span data-ttu-id="5b862-384">金鑰保存庫名稱:`contosovault`</span><span class="sxs-lookup"><span data-stu-id="5b862-384">Key vault name: `contosovault`</span></span>
* <span data-ttu-id="5b862-385">應用程式識別碼:`627e911e-43cc-61d4-992e-12db9c81b413`</span><span class="sxs-lookup"><span data-stu-id="5b862-385">Application ID: `627e911e-43cc-61d4-992e-12db9c81b413`</span></span>
* <span data-ttu-id="5b862-386">憑證指紋:`fe14593dd66b2406c5269d742d04b6e1ab03adb1`</span><span class="sxs-lookup"><span data-stu-id="5b862-386">Certificate thumbprint: `fe14593dd66b2406c5269d742d04b6e1ab03adb1`</span></span>

<span data-ttu-id="5b862-387">*應用程式設定.json*:</span><span class="sxs-lookup"><span data-stu-id="5b862-387">*appsettings.json*:</span></span>

[!code-json[](key-vault-configuration/samples/2.x/SampleApp/appsettings.json?highlight=10-12)]

<span data-ttu-id="5b862-388">運行應用時,網頁將顯示載入的機密值。</span><span class="sxs-lookup"><span data-stu-id="5b862-388">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="5b862-389">在開發環境中,機密值隨後綴一`_dev`起載入。</span><span class="sxs-lookup"><span data-stu-id="5b862-389">In the Development environment, secret values load with the `_dev` suffix.</span></span> <span data-ttu-id="5b862-390">在生產環境中,值使用`_prod`後綴載入。</span><span class="sxs-lookup"><span data-stu-id="5b862-390">In the Production environment, the values load with the `_prod` suffix.</span></span>

## <a name="use-managed-identities-for-azure-resources"></a><span data-ttu-id="5b862-391">對 Azure 資源使用託管識別</span><span class="sxs-lookup"><span data-stu-id="5b862-391">Use Managed identities for Azure resources</span></span>

<span data-ttu-id="5b862-392">**部署到 Azure 的應用**可以利用 Azure[資源的託管標識](/azure/active-directory/managed-identities-azure-resources/overview),這允許應用使用 Azure 密鑰保管庫進行身份驗證,而無需在應用中存儲認證(應用程式 ID 和密碼/用戶端金鑰)。</span><span class="sxs-lookup"><span data-stu-id="5b862-392">**An app deployed to Azure** can take advantage of [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview), which allows the app to authenticate with Azure Key Vault using Azure AD authentication without credentials (Application ID and Password/Client Secret) stored in the app.</span></span>

<span data-ttu-id="5b862-393">當*Program.cs*檔案頂部的語句`Managed`設定為`#define`時, 範例應用對 Azure 資源使用託管標識。</span><span class="sxs-lookup"><span data-stu-id="5b862-393">The sample app uses Managed identities for Azure resources when the `#define` statement at the top of the *Program.cs* file is set to `Managed`.</span></span>

<span data-ttu-id="5b862-394">在應用*的應用設置.json*檔中輸入保管庫名稱。</span><span class="sxs-lookup"><span data-stu-id="5b862-394">Enter the vault name into the app's *appsettings.json* file.</span></span> <span data-ttu-id="5b862-395">當範例應用設定為`Managed`版本時,不需要應用程式 ID 和密碼(用戶端密鑰),因此您可以忽略這些配置條目。</span><span class="sxs-lookup"><span data-stu-id="5b862-395">The sample app doesn't require an Application ID and Password (Client Secret) when set to the `Managed` version, so you can ignore those configuration entries.</span></span> <span data-ttu-id="5b862-396">應用將部署到 Azure,Azure 會驗證應用僅使用儲存在*appsettings.json*檔中的保管庫名稱訪問 Azure 密鑰保管庫。</span><span class="sxs-lookup"><span data-stu-id="5b862-396">The app is deployed to Azure, and Azure authenticates the app to access Azure Key Vault only using the vault name stored in the *appsettings.json* file.</span></span>

<span data-ttu-id="5b862-397">將示例應用部署到 Azure 應用服務。</span><span class="sxs-lookup"><span data-stu-id="5b862-397">Deploy the sample app to Azure App Service.</span></span>

<span data-ttu-id="5b862-398">創建服務時,部署到 Azure 應用服務的應用將自動註冊到 Azure AD。</span><span class="sxs-lookup"><span data-stu-id="5b862-398">An app deployed to Azure App Service is automatically registered with Azure AD when the service is created.</span></span> <span data-ttu-id="5b862-399">從部署中獲取對象 ID,以便在以下命令中使用。</span><span class="sxs-lookup"><span data-stu-id="5b862-399">Obtain the Object ID from the deployment for use in the following command.</span></span> <span data-ttu-id="5b862-400">物件識別碼顯示在應用服務的 **「標識」** 面板上的 Azure 入口中。</span><span class="sxs-lookup"><span data-stu-id="5b862-400">The Object ID is shown in the Azure portal on the **Identity** panel of the App Service.</span></span>

<span data-ttu-id="5b862-401">使用 Azure CLI 和應用程式 ID,向`list`應用程式`get`提供 存取 金鑰保管庫的許可權:</span><span class="sxs-lookup"><span data-stu-id="5b862-401">Using Azure CLI and the app's Object ID, provide the app with `list` and `get` permissions to access the key vault:</span></span>

```azurecli
az keyvault set-policy --name {KEY VAULT NAME} --object-id {OBJECT ID} --secret-permissions get list
```

<span data-ttu-id="5b862-402">使用 Azure CLI、PowerShell 或 Azure 門戶**重新啟動應用**。</span><span class="sxs-lookup"><span data-stu-id="5b862-402">**Restart the app** using Azure CLI, PowerShell, or the Azure portal.</span></span>

<span data-ttu-id="5b862-403">範例應用程式:</span><span class="sxs-lookup"><span data-stu-id="5b862-403">The sample app:</span></span>

* <span data-ttu-id="5b862-404">建立沒有連接字串的`AzureServiceTokenProvider`類實例。</span><span class="sxs-lookup"><span data-stu-id="5b862-404">Creates an instance of the `AzureServiceTokenProvider` class without a connection string.</span></span> <span data-ttu-id="5b862-405">未提供連接字串時,提供程式將嘗試從 Azure 資源的託管標識獲取訪問權杖。</span><span class="sxs-lookup"><span data-stu-id="5b862-405">When a connection string isn't provided, the provider attempts to obtain an access token from Managed identities for Azure resources.</span></span>
* <span data-ttu-id="5b862-406"><xref:Microsoft.Azure.KeyVault.KeyVaultClient>使用`AzureServiceTokenProvider`實例令牌回調創建新。</span><span class="sxs-lookup"><span data-stu-id="5b862-406">A new <xref:Microsoft.Azure.KeyVault.KeyVaultClient> is created with the `AzureServiceTokenProvider` instance token callback.</span></span>
* <span data-ttu-id="5b862-407">實例<xref:Microsoft.Azure.KeyVault.KeyVaultClient>與載入所有機密值的預設<xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>實現 一起使用,並將雙分線`--`() 替換為鍵`:`名中的冒號 ( )。</span><span class="sxs-lookup"><span data-stu-id="5b862-407">The <xref:Microsoft.Azure.KeyVault.KeyVaultClient> instance is used with a default implementation of <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> that loads all secret values and replaces double-dashes (`--`) with colons (`:`) in key names.</span></span>

[!code-csharp[](key-vault-configuration/samples/2.x/SampleApp/Program.cs?name=snippet2&highlight=13-21)]

<span data-ttu-id="5b862-408">金鑰保存庫名稱範例值:`contosovault`</span><span class="sxs-lookup"><span data-stu-id="5b862-408">Key vault name example value: `contosovault`</span></span>
    
<span data-ttu-id="5b862-409">*應用程式設定.json*:</span><span class="sxs-lookup"><span data-stu-id="5b862-409">*appsettings.json*:</span></span>

```json
{
  "KeyVaultName": "Key Vault Name"
}
```

<span data-ttu-id="5b862-410">運行應用時,網頁將顯示載入的機密值。</span><span class="sxs-lookup"><span data-stu-id="5b862-410">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="5b862-411">在開發環境中,機密值具有後綴`_dev`,因為它們由使用者機密提供。</span><span class="sxs-lookup"><span data-stu-id="5b862-411">In the Development environment, secret values have the `_dev` suffix because they're provided by User Secrets.</span></span> <span data-ttu-id="5b862-412">在生產環境中,值使用後綴載入,`_prod`因為它們由 Azure 密鑰保管庫提供。</span><span class="sxs-lookup"><span data-stu-id="5b862-412">In the Production environment, the values load with the `_prod` suffix because they're provided by Azure Key Vault.</span></span>

<span data-ttu-id="5b862-413">如果收到錯誤`Access denied`,請確認應用已註冊 Azure AD 並提供有關密鑰保管庫的訪問許可權。</span><span class="sxs-lookup"><span data-stu-id="5b862-413">If you receive an `Access denied` error, confirm that the app is registered with Azure AD and provided access to the key vault.</span></span> <span data-ttu-id="5b862-414">確認已重新啟動 Azure 中的服務。</span><span class="sxs-lookup"><span data-stu-id="5b862-414">Confirm that you've restarted the service in Azure.</span></span>

<span data-ttu-id="5b862-415">有關將提供程式與託管標識和 Azure DevOps 管道一起使用的資訊,請參閱創建 Azure[資源管理器服務連接到具有託管服務標識的 VM。](/azure/devops/pipelines/library/connect-to-azure#create-an-azure-resource-manager-service-connection-to-a-vm-with-a-managed-service-identity)</span><span class="sxs-lookup"><span data-stu-id="5b862-415">For information on using the provider with a managed identity and an Azure DevOps pipeline, see [Create an Azure Resource Manager service connection to a VM with a managed service identity](/azure/devops/pipelines/library/connect-to-azure#create-an-azure-resource-manager-service-connection-to-a-vm-with-a-managed-service-identity).</span></span>

## <a name="use-a-key-name-prefix"></a><span data-ttu-id="5b862-416">使用鍵名稱前置文字</span><span class="sxs-lookup"><span data-stu-id="5b862-416">Use a key name prefix</span></span>

<span data-ttu-id="5b862-417"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>提供接受 的實現<xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>的重載,它允許您控制如何將密鑰保管庫機密轉換為設定金鑰。</span><span class="sxs-lookup"><span data-stu-id="5b862-417"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> provides an overload that accepts an implementation of <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>, which allows you to control how key vault secrets are converted into configuration keys.</span></span> <span data-ttu-id="5b862-418">例如,您可以實現介面,以基於您在應用啟動時提供的首碼值載入機密值。</span><span class="sxs-lookup"><span data-stu-id="5b862-418">For example, you can implement the interface to load secret values based on a prefix value you provide at app startup.</span></span> <span data-ttu-id="5b862-419">例如,這允許您根據應用版本載入機密。</span><span class="sxs-lookup"><span data-stu-id="5b862-419">This allows you, for example, to load secrets based on the version of the app.</span></span>

> [!WARNING]
> <span data-ttu-id="5b862-420">不要使用密鑰保管庫機密的首碼將多個應用的機密放入同一密鑰保管庫或將環境機密(例如 *,開發*與*生產*機密)放入同一保管庫。</span><span class="sxs-lookup"><span data-stu-id="5b862-420">Don't use prefixes on key vault secrets to place secrets for multiple apps into the same key vault or to place environmental secrets (for example, *development* versus *production* secrets) into the same vault.</span></span> <span data-ttu-id="5b862-421">我們建議不同的應用和開發/生產環境使用單獨的密鑰保管庫來隔離應用環境,以確保最高級別的安全性。</span><span class="sxs-lookup"><span data-stu-id="5b862-421">We recommend that different apps and development/production environments use separate key vaults to isolate app environments for the highest level of security.</span></span>

<span data-ttu-id="5b862-422">在下面的範例中,在密鑰保管庫中建立了機密(並使用開發環境的密鑰管理員工具)(`5000-AppSecret`密鑰保管庫機密名稱中不允許週期)。</span><span class="sxs-lookup"><span data-stu-id="5b862-422">In the following example, a secret is established in the key vault (and using the Secret Manager tool for the Development environment) for `5000-AppSecret` (periods aren't allowed in key vault secret names).</span></span> <span data-ttu-id="5b862-423">此機密表示應用版本 5.0.0.0 的應用機密。</span><span class="sxs-lookup"><span data-stu-id="5b862-423">This secret represents an app secret for version 5.0.0.0 of the app.</span></span> <span data-ttu-id="5b862-424">套用用的另一個版本 5.1.0.0,將機密新增到的金鑰保管庫(並使用機密管理員工具`5100-AppSecret`)中 。</span><span class="sxs-lookup"><span data-stu-id="5b862-424">For another version of the app, 5.1.0.0, a secret is added to the key vault (and using the Secret Manager tool) for `5100-AppSecret`.</span></span> <span data-ttu-id="5b862-425">每個應用版本將其版本控制的秘密值載入到其配置中`AppSecret`,作為,在載入金鑰時剝離版本。</span><span class="sxs-lookup"><span data-stu-id="5b862-425">Each app version loads its versioned secret value into its configuration as `AppSecret`, stripping off the version as it loads the secret.</span></span>

<span data-ttu-id="5b862-426"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>使用自訂<xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>呼叫 :</span><span class="sxs-lookup"><span data-stu-id="5b862-426"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> is called with a custom <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>:</span></span>

[!code-csharp[](key-vault-configuration/samples_snapshot/Program.cs)]

<span data-ttu-id="5b862-427">實現<xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>對機密的版本首碼做出反應,以將正確的機密載入到配置中:</span><span class="sxs-lookup"><span data-stu-id="5b862-427">The <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> implementation reacts to the version prefixes of secrets to load the proper secret into configuration:</span></span>

* <span data-ttu-id="5b862-428">`Load`當機密的名稱以前綴開頭時載入它。</span><span class="sxs-lookup"><span data-stu-id="5b862-428">`Load` loads a secret when its name starts with the prefix.</span></span> <span data-ttu-id="5b862-429">其他機密未載入。</span><span class="sxs-lookup"><span data-stu-id="5b862-429">Other secrets aren't loaded.</span></span>
* <span data-ttu-id="5b862-430">`GetKey`:</span><span class="sxs-lookup"><span data-stu-id="5b862-430">`GetKey`:</span></span>
  * <span data-ttu-id="5b862-431">從機密名稱中刪除前置碼。</span><span class="sxs-lookup"><span data-stu-id="5b862-431">Removes the prefix from the secret name.</span></span>
  * <span data-ttu-id="5b862-432">將任何名稱中的兩個破折號取代為`KeyDelimiter`, 是 設定中使用的分隔符(通常是冒號)。</span><span class="sxs-lookup"><span data-stu-id="5b862-432">Replaces two dashes in any name with the `KeyDelimiter`, which is the delimiter used in configuration (usually a colon).</span></span> <span data-ttu-id="5b862-433">Azure 密鑰保管庫不允許在機密名稱中冒號。</span><span class="sxs-lookup"><span data-stu-id="5b862-433">Azure Key Vault doesn't allow a colon in secret names.</span></span>

[!code-csharp[](key-vault-configuration/samples_snapshot/Startup.cs)]

<span data-ttu-id="5b862-434">該方法`Load`由提供程式演演演算法調用,該演演演算法通過保管庫機密進行遍舍以查找具有版本首碼的提供程式演演演算法。</span><span class="sxs-lookup"><span data-stu-id="5b862-434">The `Load` method is called by a provider algorithm that iterates through the vault secrets to find the ones that have the version prefix.</span></span> <span data-ttu-id="5b862-435">使用`Load`找到 版本前置碼時,演`GetKey`演演算法使用方法傳回機密名稱的配置名稱。</span><span class="sxs-lookup"><span data-stu-id="5b862-435">When a version prefix is found with `Load`, the algorithm uses the `GetKey` method to return the configuration name of the secret name.</span></span> <span data-ttu-id="5b862-436">它將版本首碼從機密的名稱中剝離,並返回用於載入到應用的配置名稱值對中的其他機密名稱。</span><span class="sxs-lookup"><span data-stu-id="5b862-436">It strips off the version prefix from the secret's name and returns the rest of the secret name for loading into the app's configuration name-value pairs.</span></span>

<span data-ttu-id="5b862-437">實作此方法時:</span><span class="sxs-lookup"><span data-stu-id="5b862-437">When this approach is implemented:</span></span>

1. <span data-ttu-id="5b862-438">應用在應用的專案檔中指定的版本。</span><span class="sxs-lookup"><span data-stu-id="5b862-438">The app's version specified in the app's project file.</span></span> <span data-ttu-id="5b862-439">在下面的範例中,套用的版本設定`5.0.0.0`為 :</span><span class="sxs-lookup"><span data-stu-id="5b862-439">In the following example, the app's version is set to `5.0.0.0`:</span></span>

   ```xml
   <PropertyGroup>
     <Version>5.0.0.0</Version>
   </PropertyGroup>
   ```

1. <span data-ttu-id="5b862-440">確認應用的項目`<UserSecretsId>`檔案中存在屬性,`{GUID}`其中 是使用者提供的 GUID:</span><span class="sxs-lookup"><span data-stu-id="5b862-440">Confirm that a `<UserSecretsId>` property is present in the app's project file, where `{GUID}` is a user-supplied GUID:</span></span>

   ```xml
   <PropertyGroup>
     <UserSecretsId>{GUID}</UserSecretsId>
   </PropertyGroup>
   ```

   <span data-ttu-id="5b862-441">[使用秘密管理員工具](xref:security/app-secrets)在本地儲存以下機密:</span><span class="sxs-lookup"><span data-stu-id="5b862-441">Save the following secrets locally with the [Secret Manager tool](xref:security/app-secrets):</span></span>

   ```dotnetcli
   dotnet user-secrets set "5000-AppSecret" "5.0.0.0_secret_value_dev"
   dotnet user-secrets set "5100-AppSecret" "5.1.0.0_secret_value_dev"
   ```

1. <span data-ttu-id="5b862-442">機密使用以下 Azure CLI 命令儲存在 Azure 金鑰保管庫中:</span><span class="sxs-lookup"><span data-stu-id="5b862-442">Secrets are saved in Azure Key Vault using the following Azure CLI commands:</span></span>

   ```azurecli
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "5000-AppSecret" --value "5.0.0.0_secret_value_prod"
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "5100-AppSecret" --value "5.1.0.0_secret_value_prod"
   ```

1. <span data-ttu-id="5b862-443">運行應用時,將載入金鑰保管庫機密。</span><span class="sxs-lookup"><span data-stu-id="5b862-443">When the app is run, the key vault secrets are loaded.</span></span> <span data-ttu-id="5b862-444">的`5000-AppSecret`字串機密與應用的專案檔 ()`5.0.0.0`中指定的應用版本匹配。</span><span class="sxs-lookup"><span data-stu-id="5b862-444">The string secret for `5000-AppSecret` is matched to the app's version specified in the app's project file (`5.0.0.0`).</span></span>

1. <span data-ttu-id="5b862-445">版本`5000`(使用破折號)從密鑰名稱中剝離。</span><span class="sxs-lookup"><span data-stu-id="5b862-445">The version, `5000` (with the dash), is stripped from the key name.</span></span> <span data-ttu-id="5b862-446">在整個應用中,使用密鑰`AppSecret`讀取配置將載入機密值。</span><span class="sxs-lookup"><span data-stu-id="5b862-446">Throughout the app, reading configuration with the key `AppSecret` loads the secret value.</span></span>

1. <span data-ttu-id="5b862-447">如果在專案檔中將應用的版本更改為,`5.1.0.0`並且應用再次運行,則傳回的機密`5.1.0.0_secret_value_dev`值位於 「開發`5.1.0.0_secret_value_prod`」環境和 「生產」 中。</span><span class="sxs-lookup"><span data-stu-id="5b862-447">If the app's version is changed in the project file to `5.1.0.0` and the app is run again, the secret value returned is `5.1.0.0_secret_value_dev` in the Development environment and `5.1.0.0_secret_value_prod` in Production.</span></span>

> [!NOTE]
> <span data-ttu-id="5b862-448">您還可以向<xref:Microsoft.Azure.KeyVault.KeyVaultClient><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>提供您自己的實現。</span><span class="sxs-lookup"><span data-stu-id="5b862-448">You can also provide your own <xref:Microsoft.Azure.KeyVault.KeyVaultClient> implementation to <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>.</span></span> <span data-ttu-id="5b862-449">自定義客戶端允許跨應用共用用戶端的單個實例。</span><span class="sxs-lookup"><span data-stu-id="5b862-449">A custom client permits sharing a single instance of the client across the app.</span></span>

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="5b862-450">將陣列繫結到類別</span><span class="sxs-lookup"><span data-stu-id="5b862-450">Bind an array to a class</span></span>

<span data-ttu-id="5b862-451">提供程式能夠將配置值讀取到陣列中以綁定到 POCO 陣列。</span><span class="sxs-lookup"><span data-stu-id="5b862-451">The provider is capable of reading configuration values into an array for binding to a POCO array.</span></span>

<span data-ttu-id="5b862-452">從允許鍵包含冒號`:`() 分隔符的設定源讀取時,使用數字鍵段來區分組成陣`:0:`列`:1:`&hellip;`:{n}:`( 的鍵 ) 。</span><span class="sxs-lookup"><span data-stu-id="5b862-452">When reading from a configuration source that allows keys to contain colon (`:`) separators, a numeric key segment is used to distinguish the keys that make up an array (`:0:`, `:1:`, &hellip; `:{n}:`).</span></span> <span data-ttu-id="5b862-453">有關詳細資訊,請參閱[設定:將陣列綁定到類](xref:fundamentals/configuration/index#bind-an-array-to-a-class)。</span><span class="sxs-lookup"><span data-stu-id="5b862-453">For more information, see [Configuration: Bind an array to a class](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span></span>

<span data-ttu-id="5b862-454">Azure 密鑰保管庫密鑰不能將冒號用作分隔符。</span><span class="sxs-lookup"><span data-stu-id="5b862-454">Azure Key Vault keys can't use a colon as a separator.</span></span> <span data-ttu-id="5b862-455">本主題中描述的方法使用雙破折號 ()`--`作為分層值(節)的分隔符。</span><span class="sxs-lookup"><span data-stu-id="5b862-455">The approach described in this topic uses double dashes (`--`) as a separator for hierarchical values (sections).</span></span> <span data-ttu-id="5b862-456">陣列鍵儲存在 Azure 金鑰保管庫中,具有雙破折號和數位`--0--``--1--`鍵段&hellip;`--{n}--`(, 。</span><span class="sxs-lookup"><span data-stu-id="5b862-456">Array keys are stored in Azure Key Vault with double dashes and numeric key segments (`--0--`, `--1--`, &hellip; `--{n}--`).</span></span>

<span data-ttu-id="5b862-457">檢查 JSON 檔提供的以下[Serilog](https://serilog.net/)記錄提供程式設定。</span><span class="sxs-lookup"><span data-stu-id="5b862-457">Examine the following [Serilog](https://serilog.net/) logging provider configuration provided by a JSON file.</span></span> <span data-ttu-id="5b862-458">`WriteTo`陣列中定義了兩個物件文本,反映了兩個 Serilog*接收器*,它們描述日誌記錄輸出的目標:</span><span class="sxs-lookup"><span data-stu-id="5b862-458">There are two object literals defined in the `WriteTo` array that reflect two Serilog *sinks*, which describe destinations for logging output:</span></span>

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

<span data-ttu-id="5b862-459">前面 JSON 檔中顯示的設定使用雙破折號`--`() 表示法和數位段儲存在 Azure 金鑰保管庫中:</span><span class="sxs-lookup"><span data-stu-id="5b862-459">The configuration shown in the preceding JSON file is stored in Azure Key Vault using double dash (`--`) notation and numeric segments:</span></span>

| <span data-ttu-id="5b862-460">Key</span><span class="sxs-lookup"><span data-stu-id="5b862-460">Key</span></span> | <span data-ttu-id="5b862-461">值</span><span class="sxs-lookup"><span data-stu-id="5b862-461">Value</span></span> |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="reload-secrets"></a><span data-ttu-id="5b862-462">重新載入機密</span><span class="sxs-lookup"><span data-stu-id="5b862-462">Reload secrets</span></span>

<span data-ttu-id="5b862-463">機密被緩存,直到`IConfigurationRoot.Reload()`被調用。</span><span class="sxs-lookup"><span data-stu-id="5b862-463">Secrets are cached until `IConfigurationRoot.Reload()` is called.</span></span> <span data-ttu-id="5b862-464">在執行密鑰保管庫之前`Reload`,應用不會尊重金鑰保管庫中過期、禁用和更新的機密。</span><span class="sxs-lookup"><span data-stu-id="5b862-464">Expired, disabled, and updated secrets in the key vault are not respected by the app until `Reload` is executed.</span></span>

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a><span data-ttu-id="5b862-465">已關閉和過期的機密</span><span class="sxs-lookup"><span data-stu-id="5b862-465">Disabled and expired secrets</span></span>

<span data-ttu-id="5b862-466">關閉與過期的機密引發<xref:Microsoft.Azure.KeyVault.Models.KeyVaultErrorException>。</span><span class="sxs-lookup"><span data-stu-id="5b862-466">Disabled and expired secrets throw a <xref:Microsoft.Azure.KeyVault.Models.KeyVaultErrorException>.</span></span> <span data-ttu-id="5b862-467">要防止應用引發,請使用其他配置提供程式提供配置或更新禁用或過期的機密。</span><span class="sxs-lookup"><span data-stu-id="5b862-467">To prevent the app from throwing, provide the configuration using a different configuration provider or update the disabled or expired secret.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="5b862-468">疑難排解</span><span class="sxs-lookup"><span data-stu-id="5b862-468">Troubleshoot</span></span>

<span data-ttu-id="5b862-469">當應用無法使用提供程式載入配置時,將寫入[ASP.NET 核心紀錄記錄基礎結構](xref:fundamentals/logging/index)中的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="5b862-469">When the app fails to load configuration using the provider, an error message is written to the [ASP.NET Core Logging infrastructure](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="5b862-470">以下條件將阻止載入設定:</span><span class="sxs-lookup"><span data-stu-id="5b862-470">The following conditions will prevent configuration from loading:</span></span>

* <span data-ttu-id="5b862-471">應用或證書未在 Azure 活動目錄中正確配置。</span><span class="sxs-lookup"><span data-stu-id="5b862-471">The app or certificate isn't configured correctly in Azure Active Directory.</span></span>
* <span data-ttu-id="5b862-472">密鑰保管庫在 Azure 密鑰保管庫中不存在。</span><span class="sxs-lookup"><span data-stu-id="5b862-472">The key vault doesn't exist in Azure Key Vault.</span></span>
* <span data-ttu-id="5b862-473">應用程式無權訪問密鑰保管庫。</span><span class="sxs-lookup"><span data-stu-id="5b862-473">The app isn't authorized to access the key vault.</span></span>
* <span data-ttu-id="5b862-474">訪問策略不包括`Get`和`List`許可權。</span><span class="sxs-lookup"><span data-stu-id="5b862-474">The access policy doesn't include `Get` and `List` permissions.</span></span>
* <span data-ttu-id="5b862-475">在金鑰保管庫中,配置數據(名稱值對)被錯誤地命名、丟失、禁用或過期。</span><span class="sxs-lookup"><span data-stu-id="5b862-475">In the key vault, the configuration data (name-value pair) is incorrectly named, missing, disabled, or expired.</span></span>
* <span data-ttu-id="5b862-476">應用具有錯誤的金鑰保管庫名稱`KeyVaultName`( )、Azure AD`AzureADApplicationId`應用程式`AzureADCertThumbprint`ID () 或 Azure AD 證書指紋 ()。</span><span class="sxs-lookup"><span data-stu-id="5b862-476">The app has the wrong key vault name (`KeyVaultName`), Azure AD Application Id (`AzureADApplicationId`), or Azure AD certificate thumbprint (`AzureADCertThumbprint`).</span></span>
* <span data-ttu-id="5b862-477">對於要載入的值,配置鍵(名稱)在應用中不正確。</span><span class="sxs-lookup"><span data-stu-id="5b862-477">The configuration key (name) is incorrect in the app for the value you're trying to load.</span></span>
* <span data-ttu-id="5b862-478">將應用的存取策略添加到密鑰保管庫時,將創建該策略,但在**Access 策略**UI 中未選擇 **「儲存**」按鈕。</span><span class="sxs-lookup"><span data-stu-id="5b862-478">When adding the access policy for the app to the key vault, the policy was created, but the **Save** button wasn't selected in the **Access policies** UI.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5b862-479">其他資源</span><span class="sxs-lookup"><span data-stu-id="5b862-479">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* [<span data-ttu-id="5b862-480">微軟 Azure:金鑰保管庫</span><span class="sxs-lookup"><span data-stu-id="5b862-480">Microsoft Azure: Key Vault</span></span>](https://azure.microsoft.com/services/key-vault/)
* [<span data-ttu-id="5b862-481">微軟 Azure:金鑰保管庫文件</span><span class="sxs-lookup"><span data-stu-id="5b862-481">Microsoft Azure: Key Vault Documentation</span></span>](/azure/key-vault/)
* [<span data-ttu-id="5b862-482">如何為 Azure 金鑰保存庫產生並傳輸受 HSM 保護的金鑰</span><span class="sxs-lookup"><span data-stu-id="5b862-482">How to generate and transfer HSM-protected keys for Azure Key Vault</span></span>](/azure/key-vault/key-vault-hsm-protected-keys)
* [<span data-ttu-id="5b862-483">金鑰庫用戶端類別</span><span class="sxs-lookup"><span data-stu-id="5b862-483">KeyVaultClient Class</span></span>](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
* [<span data-ttu-id="5b862-484">快速入門：使用 .NET Web 應用程式從 Azure Key Vault 設定及擷取祕密</span><span class="sxs-lookup"><span data-stu-id="5b862-484">Quickstart: Set and retrieve a secret from Azure Key Vault by using a .NET web app</span></span>](/azure/key-vault/quick-create-net)
* [<span data-ttu-id="5b862-485">教學課程：如何在 .NET 中搭配使用 Azure Key Vault 與 Azure Windows 虛擬機器</span><span class="sxs-lookup"><span data-stu-id="5b862-485">Tutorial: How to use Azure Key Vault with Azure Windows Virtual Machine in .NET</span></span>](/azure/key-vault/tutorial-net-windows-virtual-machine)

::: moniker-end

