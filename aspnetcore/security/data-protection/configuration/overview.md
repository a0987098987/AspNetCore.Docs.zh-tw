---
title: 設定 ASP.NET Core 資料保護
author: rick-anderson
description: 了解如何設定 ASP.NET Core 中的資料保護。
ms.author: riande
ms.custom: mvc
ms.date: 04/11/2019
uid: security/data-protection/configuration/overview
ms.openlocfilehash: ee43427fa1e82a365d49df50567b4ca7afb5a5d3
ms.sourcegitcommit: 9b7fcb4ce00a3a32e153a080ebfaae4ef417aafa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/12/2019
ms.locfileid: "59516244"
---
# <a name="configure-aspnet-core-data-protection"></a><span data-ttu-id="b2727-103">設定 ASP.NET Core 資料保護</span><span class="sxs-lookup"><span data-stu-id="b2727-103">Configure ASP.NET Core Data Protection</span></span>

<span data-ttu-id="b2727-104">初始化資料保護系統時，它會套用[預設設定](xref:security/data-protection/configuration/default-settings)根據作業環境。</span><span class="sxs-lookup"><span data-stu-id="b2727-104">When the Data Protection system is initialized, it applies [default settings](xref:security/data-protection/configuration/default-settings) based on the operational environment.</span></span> <span data-ttu-id="b2727-105">這些設定是通常適用於在單一機器上執行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="b2727-105">These settings are generally appropriate for apps running on a single machine.</span></span> <span data-ttu-id="b2727-106">有開發人員可能要變更預設設定的情況：</span><span class="sxs-lookup"><span data-stu-id="b2727-106">There are cases where a developer may want to change the default settings:</span></span>

* <span data-ttu-id="b2727-107">應用程式則會分散到多部電腦。</span><span class="sxs-lookup"><span data-stu-id="b2727-107">The app is spread across multiple machines.</span></span>
* <span data-ttu-id="b2727-108">為了符合法規。</span><span class="sxs-lookup"><span data-stu-id="b2727-108">For compliance reasons.</span></span>

<span data-ttu-id="b2727-109">針對這些案例中，資料保護系統會提供豐富的組態 API。</span><span class="sxs-lookup"><span data-stu-id="b2727-109">For these scenarios, the Data Protection system offers a rich configuration API.</span></span>

> [!WARNING]
> <span data-ttu-id="b2727-110">類似於組態檔，資料保護 keyring 應受到保護使用適當的權限。</span><span class="sxs-lookup"><span data-stu-id="b2727-110">Similar to configuration files, the data protection key ring should be protected using appropriate permissions.</span></span> <span data-ttu-id="b2727-111">您可以選擇加密待用的金鑰，但這不會防止攻擊者建立新的金鑰。</span><span class="sxs-lookup"><span data-stu-id="b2727-111">You can choose to encrypt keys at rest, but this doesn't prevent attackers from creating new keys.</span></span> <span data-ttu-id="b2727-112">因此，您的應用程式的安全性會受到影響。</span><span class="sxs-lookup"><span data-stu-id="b2727-112">Consequently, your app's security is impacted.</span></span> <span data-ttu-id="b2727-113">設定資料保護的儲存體位置應該有其存取僅限於應用程式本身，類似於您想保護組態檔的方式。</span><span class="sxs-lookup"><span data-stu-id="b2727-113">The storage location configured with Data Protection should have its access limited to the app itself, similar to the way you would protect configuration files.</span></span> <span data-ttu-id="b2727-114">例如，如果您選擇您 keyring 儲存在磁碟上，使用檔案系統權限。</span><span class="sxs-lookup"><span data-stu-id="b2727-114">For example, if you choose to store your key ring on disk, use file system permissions.</span></span> <span data-ttu-id="b2727-115">請確定只有在識別您的 web 應用程式執行具有讀取、 寫入和建立該目錄的存取權。</span><span class="sxs-lookup"><span data-stu-id="b2727-115">Ensure only the identity under which your web app runs has read, write, and create access to that directory.</span></span> <span data-ttu-id="b2727-116">如果您使用 Azure 資料表儲存體時，web 應用程式應該能夠讀取、 寫入或建立新的項目中的資料表存放區等等。</span><span class="sxs-lookup"><span data-stu-id="b2727-116">If you use Azure Table Storage, only the web app should have the ability to read, write, or create new entries in the table store, etc.</span></span>
>
> <span data-ttu-id="b2727-117">擴充方法[AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection)會傳回[IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder)。</span><span class="sxs-lookup"><span data-stu-id="b2727-117">The extension method [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) returns an [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span></span> <span data-ttu-id="b2727-118">`IDataProtectionBuilder` 會公開，您可以鏈結在一起若要設定資料保護選項的擴充方法。</span><span class="sxs-lookup"><span data-stu-id="b2727-118">`IDataProtectionBuilder` exposes extension methods that you can chain together to configure Data Protection options.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="protectkeyswithazurekeyvault"></a><span data-ttu-id="b2727-119">ProtectKeysWithAzureKeyVault</span><span class="sxs-lookup"><span data-stu-id="b2727-119">ProtectKeysWithAzureKeyVault</span></span>

<span data-ttu-id="b2727-120">金鑰存放在[Azure Key Vault](https://azure.microsoft.com/services/key-vault/)，設定與系統[ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault)在`Startup`類別：</span><span class="sxs-lookup"><span data-stu-id="b2727-120">To store keys in [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), configure the system with [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) in the `Startup` class:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

<span data-ttu-id="b2727-121">設定金鑰環儲存體位置 (例如[PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage))。</span><span class="sxs-lookup"><span data-stu-id="b2727-121">Set the key ring storage location (for example, [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)).</span></span> <span data-ttu-id="b2727-122">必須設定的位置，因為呼叫`ProtectKeysWithAzureKeyVault`會實作[IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor)停用自動的資料保護設定，包括 keyring 存放區位置。</span><span class="sxs-lookup"><span data-stu-id="b2727-122">The location must be set because calling `ProtectKeysWithAzureKeyVault` implements an [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor) that disables automatic data protection settings, including the key ring storage location.</span></span> <span data-ttu-id="b2727-123">上述範例會使用 Azure Blob 儲存體，以保存 keyring。</span><span class="sxs-lookup"><span data-stu-id="b2727-123">The preceding example uses Azure Blob Storage to persist the key ring.</span></span> <span data-ttu-id="b2727-124">如需詳細資訊，請參閱[金鑰儲存提供者：Azure 與 Redis](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis)。</span><span class="sxs-lookup"><span data-stu-id="b2727-124">For more information, see [Key storage providers: Azure and Redis](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis).</span></span> <span data-ttu-id="b2727-125">您也可以保存在本機使用 keyring [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system)。</span><span class="sxs-lookup"><span data-stu-id="b2727-125">You can also persist the key ring locally with [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system).</span></span>

<span data-ttu-id="b2727-126">`keyIdentifier`是用於金鑰加密的金鑰保存庫金鑰識別碼。</span><span class="sxs-lookup"><span data-stu-id="b2727-126">The `keyIdentifier` is the key vault key identifier used for key encryption.</span></span> <span data-ttu-id="b2727-127">例如，建立名為金鑰保存庫中的索引鍵`dataprotection`中`contosokeyvault`具有金鑰識別碼`https://contosokeyvault.vault.azure.net/keys/dataprotection/`。</span><span class="sxs-lookup"><span data-stu-id="b2727-127">For example, a key created in key vault named `dataprotection` in the `contosokeyvault` has the key identifier `https://contosokeyvault.vault.azure.net/keys/dataprotection/`.</span></span> <span data-ttu-id="b2727-128">提供應用程式與**解除包裝金鑰**並**包裝金鑰**到 key vault 的權限。</span><span class="sxs-lookup"><span data-stu-id="b2727-128">Provide the app with **Unwrap Key** and **Wrap Key** permissions to the key vault.</span></span>

<span data-ttu-id="b2727-129">`ProtectKeysWithAzureKeyVault` 多載：</span><span class="sxs-lookup"><span data-stu-id="b2727-129">`ProtectKeysWithAzureKeyVault` overloads:</span></span>

* <span data-ttu-id="b2727-130">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder，KeyVaultClient，String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_)允許使用[KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)啟用資料保護系統，才能使用金鑰保存庫。</span><span class="sxs-lookup"><span data-stu-id="b2727-130">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, KeyVaultClient, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_) permits the use of a [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) to enable the data protection system to use the key vault.</span></span>
* <span data-ttu-id="b2727-131">[（IDataProtectionBuilder、 字串、 字串、 X509Certificate2） ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_)允許使用`ClientId`並[X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2)啟用資料保護系統，才能使用金鑰保存庫。</span><span class="sxs-lookup"><span data-stu-id="b2727-131">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, String, String, X509Certificate2)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_) permits the use of a `ClientId` and [X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) to enable the data protection system to use the key vault.</span></span>
* <span data-ttu-id="b2727-132">[（IDataProtectionBuilder、 字串、 字串、 字串） 的 ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_)允許使用`ClientId`和`ClientSecret`啟用資料保護系統，才能使用金鑰保存庫。</span><span class="sxs-lookup"><span data-stu-id="b2727-132">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, String, String, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_) permits the use of a `ClientId` and `ClientSecret` to enable the data protection system to use the key vault.</span></span>

::: moniker-end

## <a name="persistkeystofilesystem"></a><span data-ttu-id="b2727-133">PersistKeysToFileSystem</span><span class="sxs-lookup"><span data-stu-id="b2727-133">PersistKeysToFileSystem</span></span>

<span data-ttu-id="b2727-134">若要將金鑰存放在 UNC 共用而不是在 *%LOCALAPPDATA%* 的預設位置，設定與系統[PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span><span class="sxs-lookup"><span data-stu-id="b2727-134">To store keys on a UNC share instead of at the *%LOCALAPPDATA%* default location, configure the system with [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

> [!WARNING]
> <span data-ttu-id="b2727-135">如果您變更金鑰的持續性位置時，系統將不會再自動加密待用的金鑰因為它並不知道是否 DPAPI 是適當的加密機制。</span><span class="sxs-lookup"><span data-stu-id="b2727-135">If you change the key persistence location, the system no longer automatically encrypts keys at rest, since it doesn't know whether DPAPI is an appropriate encryption mechanism.</span></span>

## <a name="protectkeyswith"></a><span data-ttu-id="b2727-136">ProtectKeysWith\*</span><span class="sxs-lookup"><span data-stu-id="b2727-136">ProtectKeysWith\*</span></span>

<span data-ttu-id="b2727-137">您可以設定來保護待用的金鑰呼叫任何的系統[ProtectKeysWith\* ](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions)組態 Api。</span><span class="sxs-lookup"><span data-stu-id="b2727-137">You can configure the system to protect keys at rest by calling any of the [ProtectKeysWith\*](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) configuration APIs.</span></span> <span data-ttu-id="b2727-138">請考慮下列範例中，這在 UNC 共用上儲存金鑰，並加密待用與特定 X.509 憑證的這些金鑰：</span><span class="sxs-lookup"><span data-stu-id="b2727-138">Consider the example below, which stores keys on a UNC share and encrypts those keys at rest with a specific X.509 certificate:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="b2727-139">在 ASP.NET Core 2.1 或更新版本，您可以提供[X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2)要[ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate)，例如從檔案載入憑證：</span><span class="sxs-lookup"><span data-stu-id="b2727-139">In ASP.NET Core 2.1 or later, you can provide an [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) to [ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate), such as a certificate loaded from a file:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate(
            new X509Certificate2("certificate.pfx", "password"));
}
```

::: moniker-end

<span data-ttu-id="b2727-140">請參閱[加密的金鑰在 Rest](xref:security/data-protection/implementation/key-encryption-at-rest)的更多範例和討論內建的金鑰加密機制。</span><span class="sxs-lookup"><span data-stu-id="b2727-140">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more examples and discussion on the built-in key encryption mechanisms.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="unprotectkeyswithanycertificate"></a><span data-ttu-id="b2727-141">UnprotectKeysWithAnyCertificate</span><span class="sxs-lookup"><span data-stu-id="b2727-141">UnprotectKeysWithAnyCertificate</span></span>

<span data-ttu-id="b2727-142">在 ASP.NET Core 2.1 或更新版本，您可以旋轉憑證和解密金鑰在待用期間使用的陣列[X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2)與憑證[UnprotectKeysWithAnyCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.unprotectkeyswithanycertificate):</span><span class="sxs-lookup"><span data-stu-id="b2727-142">In ASP.NET Core 2.1 or later, you can rotate certificates and decrypt keys at rest using an array of [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) certificates with [UnprotectKeysWithAnyCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.unprotectkeyswithanycertificate):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate(
            new X509Certificate2("certificate.pfx", "password"));
        .UnprotectKeysWithAnyCertificate(
            new X509Certificate2("certificate_old_1.pfx", "password_1"),
            new X509Certificate2("certificate_old_2.pfx", "password_2"));
}
```

::: moniker-end

## <a name="setdefaultkeylifetime"></a><span data-ttu-id="b2727-143">SetDefaultKeyLifetime</span><span class="sxs-lookup"><span data-stu-id="b2727-143">SetDefaultKeyLifetime</span></span>

<span data-ttu-id="b2727-144">若要設定系統使用的金鑰的存留期為 14 天，而非預設的 90 天，請使用[SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span><span class="sxs-lookup"><span data-stu-id="b2727-144">To configure the system to use a key lifetime of 14 days instead of the default 90 days, use [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

## <a name="setapplicationname"></a><span data-ttu-id="b2727-145">SetApplicationName</span><span class="sxs-lookup"><span data-stu-id="b2727-145">SetApplicationName</span></span>

<span data-ttu-id="b2727-146">根據預設，資料保護系統會隔離應用程式不會根據其內容根路徑，彼此，即使它們共用相同的實體索引鍵存放庫。</span><span class="sxs-lookup"><span data-stu-id="b2727-146">By default, the Data Protection system isolates apps from one another based on their content root paths, even if they're sharing the same physical key repository.</span></span> <span data-ttu-id="b2727-147">這可防止應用程式了解彼此的受保護的承載。</span><span class="sxs-lookup"><span data-stu-id="b2727-147">This prevents the apps from understanding each other's protected payloads.</span></span>

<span data-ttu-id="b2727-148">若要共用的受保護應用程式間的承載：</span><span class="sxs-lookup"><span data-stu-id="b2727-148">To share protected payloads among apps:</span></span>

* <span data-ttu-id="b2727-149">設定<xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*>中具有相同值的每個應用程式。</span><span class="sxs-lookup"><span data-stu-id="b2727-149">Configure <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> in each app with the same value.</span></span>
* <span data-ttu-id="b2727-150">跨應用程式使用相同版本的資料保護 API 堆疊。</span><span class="sxs-lookup"><span data-stu-id="b2727-150">Use the same version of the Data Protection API stack across the apps.</span></span> <span data-ttu-id="b2727-151">執行**任一**的應用程式的專案檔中的下列：</span><span class="sxs-lookup"><span data-stu-id="b2727-151">Perform **either** of the following in the apps' project files:</span></span>
  * <span data-ttu-id="b2727-152">參考相同的共用的 framework 版本，透過[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)。</span><span class="sxs-lookup"><span data-stu-id="b2727-152">Reference the same shared framework version via the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
  * <span data-ttu-id="b2727-153">參考相同[Data Protection 套件](xref:security/data-protection/introduction#package-layout)版本。</span><span class="sxs-lookup"><span data-stu-id="b2727-153">Reference the same [Data Protection package](xref:security/data-protection/introduction#package-layout) version.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("shared app name");
}
```

## <a name="disableautomatickeygeneration"></a><span data-ttu-id="b2727-154">DisableAutomaticKeyGeneration</span><span class="sxs-lookup"><span data-stu-id="b2727-154">DisableAutomaticKeyGeneration</span></span>

<span data-ttu-id="b2727-155">您可能會有您不想要自動彙總 （建立新的金鑰） 的索引鍵，因為他們接近到期的應用程式的案例。</span><span class="sxs-lookup"><span data-stu-id="b2727-155">You may have a scenario where you don't want an app to automatically roll keys (create new keys) as they approach expiration.</span></span> <span data-ttu-id="b2727-156">這其中一個範例可能是應用程式設定中的主要/次要關聯性，其中主要應用程式必須負責針對金鑰管理問題，而次要應用程式只需要金鑰環的唯讀檢視。</span><span class="sxs-lookup"><span data-stu-id="b2727-156">One example of this might be apps set up in a primary/secondary relationship, where only the primary app is responsible for key management concerns and secondary apps simply have a read-only view of the key ring.</span></span> <span data-ttu-id="b2727-157">可以設定次要應用程式，將視為唯讀的金鑰環，藉由設定與系統<xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.DisableAutomaticKeyGeneration*>:</span><span class="sxs-lookup"><span data-stu-id="b2727-157">The secondary apps can be configured to treat the key ring as read-only by configuring the system with <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.DisableAutomaticKeyGeneration*>:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

## <a name="per-application-isolation"></a><span data-ttu-id="b2727-158">每個應用程式隔離</span><span class="sxs-lookup"><span data-stu-id="b2727-158">Per-application isolation</span></span>

<span data-ttu-id="b2727-159">ASP.NET Core 主機所提供資料保護系統時，它會自動隔離應用程式不會彼此，即使這些應用程式相同的背景工作處理序帳戶下執行，並使用相同的主要金鑰材料。</span><span class="sxs-lookup"><span data-stu-id="b2727-159">When the Data Protection system is provided by an ASP.NET Core host, it automatically isolates apps from one another, even if those apps are running under the same worker process account and are using the same master keying material.</span></span> <span data-ttu-id="b2727-160">這是有點類似 IsolateApps 修飾詞將來自 System.Web 的`<machineKey>`項目。</span><span class="sxs-lookup"><span data-stu-id="b2727-160">This is somewhat similar to the IsolateApps modifier from System.Web's `<machineKey>` element.</span></span>

<span data-ttu-id="b2727-161">隔離機制的運作方式是為唯一的租用戶，考慮在本機電腦上的每個應用程式，因此<xref:Microsoft.AspNetCore.DataProtection.IDataProtector>root 破解的任何指定的應用程式會自動包含為鑑別子的應用程式識別碼。</span><span class="sxs-lookup"><span data-stu-id="b2727-161">The isolation mechanism works by considering each app on the local machine as a unique tenant, thus the <xref:Microsoft.AspNetCore.DataProtection.IDataProtector> rooted for any given app automatically includes the app ID as a discriminator.</span></span> <span data-ttu-id="b2727-162">應用程式的唯一識別碼是應用程式的實體路徑：</span><span class="sxs-lookup"><span data-stu-id="b2727-162">The app's unique ID is the app's physical path:</span></span>

* <span data-ttu-id="b2727-163">針對應用程式裝載於[IIS](xref:fundamentals/servers/index#iis-http-server)，唯一的識別碼是應用程式的 IIS 實體路徑。</span><span class="sxs-lookup"><span data-stu-id="b2727-163">For apps hosted in [IIS](xref:fundamentals/servers/index#iis-http-server), the unique ID is the IIS physical path of the app.</span></span> <span data-ttu-id="b2727-164">如果應用程式部署在 web 伺服陣列環境中，這個值是穩定假設 IIS 環境 web 伺服陣列中的所有電腦上設定方式均類似。</span><span class="sxs-lookup"><span data-stu-id="b2727-164">If an app is deployed in a web farm environment, this value is stable assuming that the IIS environments are configured similarly across all machines in the web farm.</span></span>
* <span data-ttu-id="b2727-165">上執行的自我裝載應用程式[Kestrel 伺服器](xref:fundamentals/servers/index#kestrel)，唯一的識別碼是在磁碟上的應用程式的實體路徑。</span><span class="sxs-lookup"><span data-stu-id="b2727-165">For self-hosted apps running on the [Kestrel server](xref:fundamentals/servers/index#kestrel), the unique ID is the physical path to the app on disk.</span></span>

<span data-ttu-id="b2727-166">唯一識別項設計來重設之後存留下來&mdash;個別的應用程式和的電腦本身。</span><span class="sxs-lookup"><span data-stu-id="b2727-166">The unique identifier is designed to survive resets&mdash;both of the individual app and of the machine itself.</span></span>

<span data-ttu-id="b2727-167">此隔離機制假設應用程式不是惡意的。</span><span class="sxs-lookup"><span data-stu-id="b2727-167">This isolation mechanism assumes that the apps are not malicious.</span></span> <span data-ttu-id="b2727-168">惡意應用程式一律會影響任何其他在相同的背景工作處理序帳戶下執行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="b2727-168">A malicious app can always impact any other app running under the same worker process account.</span></span> <span data-ttu-id="b2727-169">在共用主控環境中彼此不受信任的應用程式所在，主機服務提供者應該採取步驟，以確保應用程式，包括將應用程式的基礎金鑰存放庫之間的 OS 層級隔離。</span><span class="sxs-lookup"><span data-stu-id="b2727-169">In a shared hosting environment where apps are mutually untrusted, the hosting provider should take steps to ensure OS-level isolation between apps, including separating the apps' underlying key repositories.</span></span>

<span data-ttu-id="b2727-170">ASP.NET Core 主應用程式不提供資料保護系統 (例如，如果您將透過其執行個體化`DataProtectionProvider`具象類型) 預設為停用應用程式隔離。</span><span class="sxs-lookup"><span data-stu-id="b2727-170">If the Data Protection system isn't provided by an ASP.NET Core host (for example, if you instantiate it via the `DataProtectionProvider` concrete type) app isolation is disabled by default.</span></span> <span data-ttu-id="b2727-171">只要提供適當停用應用程式隔離時，受到相同的金鑰材料的所有應用程式可以共用承載[用途](xref:security/data-protection/consumer-apis/purpose-strings)。</span><span class="sxs-lookup"><span data-stu-id="b2727-171">When app isolation is disabled, all apps backed by the same keying material can share payloads as long as they provide the appropriate [purposes](xref:security/data-protection/consumer-apis/purpose-strings).</span></span> <span data-ttu-id="b2727-172">若要提供應用程式隔離，在此環境中的，呼叫[SetApplicationName](#setapplicationname)方法設定物件，並提供每個應用程式的唯一名稱。</span><span class="sxs-lookup"><span data-stu-id="b2727-172">To provide app isolation in this environment, call the [SetApplicationName](#setapplicationname) method on the configuration object and provide a unique name for each app.</span></span>

## <a name="changing-algorithms-with-usecryptographicalgorithms"></a><span data-ttu-id="b2727-173">變更具有 UseCryptographicAlgorithms 演算法</span><span class="sxs-lookup"><span data-stu-id="b2727-173">Changing algorithms with UseCryptographicAlgorithms</span></span>

<span data-ttu-id="b2727-174">資料保護堆疊可讓您變更使用新產生的索引鍵的預設演算法。</span><span class="sxs-lookup"><span data-stu-id="b2727-174">The Data Protection stack allows you to change the default algorithm used by newly-generated keys.</span></span> <span data-ttu-id="b2727-175">若要這樣做最簡單的方式是呼叫[UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms)設定回呼：</span><span class="sxs-lookup"><span data-stu-id="b2727-175">The simplest way to do this is to call [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) from the configuration callback:</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptionSettings()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

::: moniker-end

<span data-ttu-id="b2727-176">EncryptionAlgorithm 的預設值是 AES-256-CBC，預設 ValidationAlgorithm HMACSHA256。</span><span class="sxs-lookup"><span data-stu-id="b2727-176">The default EncryptionAlgorithm is AES-256-CBC, and the default ValidationAlgorithm is HMACSHA256.</span></span> <span data-ttu-id="b2727-177">透過系統管理員可以設定預設原則[全電腦原則](xref:security/data-protection/configuration/machine-wide-policy)，但明確呼叫`UseCryptographicAlgorithms`會覆寫預設原則。</span><span class="sxs-lookup"><span data-stu-id="b2727-177">The default policy can be set by a system administrator via a [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy), but an explicit call to `UseCryptographicAlgorithms` overrides the default policy.</span></span>

<span data-ttu-id="b2727-178">呼叫`UseCryptographicAlgorithms`可讓您指定的所需的演算法，從預先定義的內建清單。</span><span class="sxs-lookup"><span data-stu-id="b2727-178">Calling `UseCryptographicAlgorithms` allows you to specify the desired algorithm from a predefined built-in list.</span></span> <span data-ttu-id="b2727-179">您不需要擔心演算法的實作。</span><span class="sxs-lookup"><span data-stu-id="b2727-179">You don't need to worry about the implementation of the algorithm.</span></span> <span data-ttu-id="b2727-180">在上述案例中，資料保護系統會嘗試使用 AES 的 CNG 實作，如果在 Windows 上執行。</span><span class="sxs-lookup"><span data-stu-id="b2727-180">In the scenario above, the Data Protection system attempts to use the CNG implementation of AES if running on Windows.</span></span> <span data-ttu-id="b2727-181">否則，它會回復到 managed [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes)類別。</span><span class="sxs-lookup"><span data-stu-id="b2727-181">Otherwise, it falls back to the managed [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) class.</span></span>

<span data-ttu-id="b2727-182">您可以手動指定透過呼叫實作[UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms)。</span><span class="sxs-lookup"><span data-stu-id="b2727-182">You can manually specify an implementation via a call to [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span></span>

> [!TIP]
> <span data-ttu-id="b2727-183">變更演算法不會影響金鑰環中的現有金鑰。</span><span class="sxs-lookup"><span data-stu-id="b2727-183">Changing algorithms doesn't affect existing keys in the key ring.</span></span> <span data-ttu-id="b2727-184">它只會影響新產生的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="b2727-184">It only affects newly-generated keys.</span></span>

### <a name="specifying-custom-managed-algorithms"></a><span data-ttu-id="b2727-185">指定受管理的自訂演算法</span><span class="sxs-lookup"><span data-stu-id="b2727-185">Specifying custom managed algorithms</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="b2727-186">若要指定受管理的自訂演算法，建立[ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration)指向實作類型的執行個體：</span><span class="sxs-lookup"><span data-stu-id="b2727-186">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) instance that points to the implementation types:</span></span>

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new ManagedAuthenticatedEncryptorConfiguration()
    {
        // A type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // A type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="b2727-187">若要指定受管理的自訂演算法，建立[ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings)指向實作類型的執行個體：</span><span class="sxs-lookup"><span data-stu-id="b2727-187">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) instance that points to the implementation types:</span></span>

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new ManagedAuthenticatedEncryptionSettings()
    {
        // A type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // A type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

::: moniker-end

<span data-ttu-id="b2727-188">通常\*類型屬性必須指向實體，可具現化 （透過公用的無參數建構函式） 實作[SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm)並[KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm)，不過系統特殊案例某些等值`typeof(Aes)`為了方便起見。</span><span class="sxs-lookup"><span data-stu-id="b2727-188">Generally the \*Type properties must point to concrete, instantiable (via a public parameterless ctor) implementations of [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) and [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), though the system special-cases some values like `typeof(Aes)` for convenience.</span></span>

> [!NOTE]
> <span data-ttu-id="b2727-189">SymmetricAlgorithm 必須 ≥ 128 位元的金鑰長度和 ≥ 64 位元為單位的區塊大小，而且它必須支援使用 PKCS #7 填補的 CBC 模式加密。</span><span class="sxs-lookup"><span data-stu-id="b2727-189">The SymmetricAlgorithm must have a key length of ≥ 128 bits and a block size of ≥ 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="b2727-190">KeyedHashAlgorithm 必須有摘要大小 > = 128 位元，而且它必須支援的長度等於雜湊演算法的摘要長度索引鍵。</span><span class="sxs-lookup"><span data-stu-id="b2727-190">The KeyedHashAlgorithm must have a digest size of >= 128 bits, and it must support keys of length equal to the hash algorithm's digest length.</span></span> <span data-ttu-id="b2727-191">KeyedHashAlgorithm 不是 HMAC 是絕對必要。</span><span class="sxs-lookup"><span data-stu-id="b2727-191">The KeyedHashAlgorithm isn't strictly required to be HMAC.</span></span>

### <a name="specifying-custom-windows-cng-algorithms"></a><span data-ttu-id="b2727-192">指定自訂的 Windows CNG 演算法</span><span class="sxs-lookup"><span data-stu-id="b2727-192">Specifying custom Windows CNG algorithms</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="b2727-193">若要指定 HMAC 驗證搭配使用 CBC 模式加密的自訂 Windows CNG 演算法，建立[CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration)執行個體，包含演算法的資訊：</span><span class="sxs-lookup"><span data-stu-id="b2727-193">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngCbcAuthenticatedEncryptorConfiguration()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // Passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="b2727-194">若要指定 HMAC 驗證搭配使用 CBC 模式加密的自訂 Windows CNG 演算法，建立[CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings)執行個體，包含演算法的資訊：</span><span class="sxs-lookup"><span data-stu-id="b2727-194">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngCbcAuthenticatedEncryptionSettings()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // Passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

::: moniker-end

> [!NOTE]
> <span data-ttu-id="b2727-195">對稱區塊加密演算法金鑰長度必須 > = 128 位元為單位的區塊大小 > = 64 位元，而且它必須支援使用 PKCS #7 填補的 CBC 模式加密。</span><span class="sxs-lookup"><span data-stu-id="b2727-195">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of >= 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="b2727-196">雜湊演算法必須有摘要大小 > = 128 位元，且必須支援正在開啟與 BCRYPT\_ALG\_處理\_HMAC\_旗標的旗標。</span><span class="sxs-lookup"><span data-stu-id="b2727-196">The hash algorithm must have a digest size of >= 128 bits and must support being opened with the BCRYPT\_ALG\_HANDLE\_HMAC\_FLAG flag.</span></span> <span data-ttu-id="b2727-197">\*可以設定為 null，以指定的演算法所用的預設提供者提供者屬性。</span><span class="sxs-lookup"><span data-stu-id="b2727-197">The \*Provider properties can be set to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="b2727-198">請參閱[BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx)文件的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="b2727-198">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="b2727-199">若要指定使用驗證 Galois/計數器模式加密的自訂 Windows CNG 演算法，建立[CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration)執行個體，包含演算法的資訊：</span><span class="sxs-lookup"><span data-stu-id="b2727-199">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngGcmAuthenticatedEncryptorConfiguration()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="b2727-200">若要指定使用驗證 Galois/計數器模式加密的自訂 Windows CNG 演算法，建立[CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings)執行個體，包含演算法的資訊：</span><span class="sxs-lookup"><span data-stu-id="b2727-200">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngGcmAuthenticatedEncryptionSettings()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

::: moniker-end

> [!NOTE]
> <span data-ttu-id="b2727-201">對稱區塊加密演算法金鑰長度必須 > = 128 位元，區塊大小為剛好為 128 位元，而且它必須支援 GCM 加密。</span><span class="sxs-lookup"><span data-stu-id="b2727-201">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of exactly 128 bits, and it must support GCM encryption.</span></span> <span data-ttu-id="b2727-202">您可以設定[EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider)屬性設為 null 指定演算法所用的預設提供者。</span><span class="sxs-lookup"><span data-stu-id="b2727-202">You can set the [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) property to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="b2727-203">請參閱[BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx)文件的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="b2727-203">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

### <a name="specifying-other-custom-algorithms"></a><span data-ttu-id="b2727-204">指定其他的自訂演算法</span><span class="sxs-lookup"><span data-stu-id="b2727-204">Specifying other custom algorithms</span></span>

<span data-ttu-id="b2727-205">雖然不公開做為第一級的 API，資料保護系統就會是可延伸，足以讓您指定幾乎任何一種演算法。</span><span class="sxs-lookup"><span data-stu-id="b2727-205">Though not exposed as a first-class API, the Data Protection system is extensible enough to allow specifying almost any kind of algorithm.</span></span> <span data-ttu-id="b2727-206">比方說，就可以將包含在硬體安全性模組 (HSM) 的所有索引鍵，並提供核心的自訂實作加密和解密的常式。</span><span class="sxs-lookup"><span data-stu-id="b2727-206">For example, it's possible to keep all keys contained within a Hardware Security Module (HSM) and to provide a custom implementation of the core encryption and decryption routines.</span></span> <span data-ttu-id="b2727-207">請參閱[IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor)中[核心加密擴充性](xref:security/data-protection/extensibility/core-crypto)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="b2727-207">See [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) in [Core cryptography extensibility](xref:security/data-protection/extensibility/core-crypto) for more information.</span></span>

## <a name="persisting-keys-when-hosting-in-a-docker-container"></a><span data-ttu-id="b2727-208">裝載的 Docker 容器中時，保存索引鍵</span><span class="sxs-lookup"><span data-stu-id="b2727-208">Persisting keys when hosting in a Docker container</span></span>

<span data-ttu-id="b2727-209">裝載於[Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/)容器中，索引鍵應保存在其中一個：</span><span class="sxs-lookup"><span data-stu-id="b2727-209">When hosting in a [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) container, keys should be maintained in either:</span></span>

* <span data-ttu-id="b2727-210">是保存超過容器的存留期，例如共用的磁碟區或主應用程式掛接的磁碟區的 Docker 磁碟區的資料夾。</span><span class="sxs-lookup"><span data-stu-id="b2727-210">A folder that's a Docker volume that persists beyond the container's lifetime, such as a shared volume or a host-mounted volume.</span></span>
* <span data-ttu-id="b2727-211">外部提供者，例如[Azure Key Vault](https://azure.microsoft.com/services/key-vault/)或是[Redis](https://redis.io/)。</span><span class="sxs-lookup"><span data-stu-id="b2727-211">An external provider, such as [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) or [Redis](https://redis.io/).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b2727-212">其他資源</span><span class="sxs-lookup"><span data-stu-id="b2727-212">Additional resources</span></span>

* <xref:security/data-protection/configuration/non-di-scenarios>
* <xref:security/data-protection/configuration/machine-wide-policy>
* <xref:host-and-deploy/web-farm>
* <xref:security/data-protection/implementation/key-storage-providers>
