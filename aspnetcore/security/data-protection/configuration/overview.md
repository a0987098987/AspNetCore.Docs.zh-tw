---
title: 設定 ASP.NET Core 資料保護
author: rick-anderson
description: 瞭解如何在 ASP.NET Core 中設定資料保護。
ms.author: riande
ms.custom: mvc
ms.date: 10/07/2019
uid: security/data-protection/configuration/overview
ms.openlocfilehash: c0846aca4bb663b1d562ab0c877fefba02da460f
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78666590"
---
# <a name="configure-aspnet-core-data-protection"></a><span data-ttu-id="b1a16-103">設定 ASP.NET Core 資料保護</span><span class="sxs-lookup"><span data-stu-id="b1a16-103">Configure ASP.NET Core Data Protection</span></span>

<span data-ttu-id="b1a16-104">當資料保護系統初始化時，它會根據操作環境來套用[預設設定](xref:security/data-protection/configuration/default-settings)。</span><span class="sxs-lookup"><span data-stu-id="b1a16-104">When the Data Protection system is initialized, it applies [default settings](xref:security/data-protection/configuration/default-settings) based on the operational environment.</span></span> <span data-ttu-id="b1a16-105">這些設定通常適用于在單一電腦上執行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="b1a16-105">These settings are generally appropriate for apps running on a single machine.</span></span> <span data-ttu-id="b1a16-106">在某些情況下，開發人員可能會想要變更預設設定：</span><span class="sxs-lookup"><span data-stu-id="b1a16-106">There are cases where a developer may want to change the default settings:</span></span>

* <span data-ttu-id="b1a16-107">應用程式會散佈在多部電腦上。</span><span class="sxs-lookup"><span data-stu-id="b1a16-107">The app is spread across multiple machines.</span></span>
* <span data-ttu-id="b1a16-108">基於合規性因素。</span><span class="sxs-lookup"><span data-stu-id="b1a16-108">For compliance reasons.</span></span>

<span data-ttu-id="b1a16-109">在這些情況下，資料保護系統會提供豐富的設定 API。</span><span class="sxs-lookup"><span data-stu-id="b1a16-109">For these scenarios, the Data Protection system offers a rich configuration API.</span></span>

> [!WARNING]
> <span data-ttu-id="b1a16-110">與設定檔類似，應使用適當的許可權來保護資料保護金鑰環。</span><span class="sxs-lookup"><span data-stu-id="b1a16-110">Similar to configuration files, the data protection key ring should be protected using appropriate permissions.</span></span> <span data-ttu-id="b1a16-111">您可以選擇加密待用金鑰，但這並不會阻止攻擊者建立新的金鑰。</span><span class="sxs-lookup"><span data-stu-id="b1a16-111">You can choose to encrypt keys at rest, but this doesn't prevent attackers from creating new keys.</span></span> <span data-ttu-id="b1a16-112">因此，您的應用程式安全性會受到影響。</span><span class="sxs-lookup"><span data-stu-id="b1a16-112">Consequently, your app's security is impacted.</span></span> <span data-ttu-id="b1a16-113">使用資料保護設定的儲存位置應該將其存取限制為應用程式本身，與保護設定檔的方式類似。</span><span class="sxs-lookup"><span data-stu-id="b1a16-113">The storage location configured with Data Protection should have its access limited to the app itself, similar to the way you would protect configuration files.</span></span> <span data-ttu-id="b1a16-114">例如，如果您選擇將金鑰環形儲存在磁片上，請使用檔案系統許可權。</span><span class="sxs-lookup"><span data-stu-id="b1a16-114">For example, if you choose to store your key ring on disk, use file system permissions.</span></span> <span data-ttu-id="b1a16-115">請確定您的 web 應用程式執行所用的身分識別，具有該目錄的讀取、寫入和建立存取權。</span><span class="sxs-lookup"><span data-stu-id="b1a16-115">Ensure only the identity under which your web app runs has read, write, and create access to that directory.</span></span> <span data-ttu-id="b1a16-116">如果您使用 Azure Blob 儲存體，只有 web 應用程式應該能夠在 Blob 存放區中讀取、寫入或建立新的專案等等。</span><span class="sxs-lookup"><span data-stu-id="b1a16-116">If you use Azure Blob Storage, only the web app should have the ability to read, write, or create new entries in the blob store, etc.</span></span>
>
> <span data-ttu-id="b1a16-117">擴充方法[AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection)會傳回[IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder)。</span><span class="sxs-lookup"><span data-stu-id="b1a16-117">The extension method [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) returns an [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span></span> <span data-ttu-id="b1a16-118">`IDataProtectionBuilder` 會公開可連結在一起以設定資料保護選項的擴充方法。</span><span class="sxs-lookup"><span data-stu-id="b1a16-118">`IDataProtectionBuilder` exposes extension methods that you can chain together to configure Data Protection options.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b1a16-119">本文中使用的資料保護延伸模組需要下列 NuGet 套件：</span><span class="sxs-lookup"><span data-stu-id="b1a16-119">The following NuGet packages are required for the Data Protection extensions used in this article:</span></span>

* [<span data-ttu-id="b1a16-120">AspNetCore. DataProtection. AzureStorage</span><span class="sxs-lookup"><span data-stu-id="b1a16-120">Microsoft.AspNetCore.DataProtection.AzureStorage</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/)
* [<span data-ttu-id="b1a16-121">AspNetCore. DataProtection. AzureKeyVault</span><span class="sxs-lookup"><span data-stu-id="b1a16-121">Microsoft.AspNetCore.DataProtection.AzureKeyVault</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureKeyVault/)

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="protectkeyswithazurekeyvault"></a><span data-ttu-id="b1a16-122">ProtectKeysWithAzureKeyVault</span><span class="sxs-lookup"><span data-stu-id="b1a16-122">ProtectKeysWithAzureKeyVault</span></span>

<span data-ttu-id="b1a16-123">若要將金鑰儲存在[Azure Key Vault](https://azure.microsoft.com/services/key-vault/)中，請在 `Startup` 類別中設定具有[ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault)的系統：</span><span class="sxs-lookup"><span data-stu-id="b1a16-123">To store keys in [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), configure the system with [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) in the `Startup` class:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

<span data-ttu-id="b1a16-124">設定金鑰環形儲存位置（例如， [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)）。</span><span class="sxs-lookup"><span data-stu-id="b1a16-124">Set the key ring storage location (for example, [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)).</span></span> <span data-ttu-id="b1a16-125">必須設定位置，因為呼叫 `ProtectKeysWithAzureKeyVault` 會執行停用自動資料保護設定的[IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor) ，包括金鑰環形儲存位置。</span><span class="sxs-lookup"><span data-stu-id="b1a16-125">The location must be set because calling `ProtectKeysWithAzureKeyVault` implements an [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor) that disables automatic data protection settings, including the key ring storage location.</span></span> <span data-ttu-id="b1a16-126">上述範例會使用 Azure Blob 儲存體來保存金鑰環。</span><span class="sxs-lookup"><span data-stu-id="b1a16-126">The preceding example uses Azure Blob Storage to persist the key ring.</span></span> <span data-ttu-id="b1a16-127">如需詳細資訊，請參閱[金鑰儲存提供者： Azure 儲存體](xref:security/data-protection/implementation/key-storage-providers#azure-storage)。</span><span class="sxs-lookup"><span data-stu-id="b1a16-127">For more information, see [Key storage providers: Azure Storage](xref:security/data-protection/implementation/key-storage-providers#azure-storage).</span></span> <span data-ttu-id="b1a16-128">您也可以使用[PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system)在本機保存金鑰信號。</span><span class="sxs-lookup"><span data-stu-id="b1a16-128">You can also persist the key ring locally with [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system).</span></span>

<span data-ttu-id="b1a16-129">`keyIdentifier` 是用於金鑰加密的金鑰保存庫金鑰識別碼。</span><span class="sxs-lookup"><span data-stu-id="b1a16-129">The `keyIdentifier` is the key vault key identifier used for key encryption.</span></span> <span data-ttu-id="b1a16-130">例如，在 `contosokeyvault` 中名為 `dataprotection` 的金鑰保存庫中建立的金鑰具有金鑰識別碼 `https://contosokeyvault.vault.azure.net/keys/dataprotection/`。</span><span class="sxs-lookup"><span data-stu-id="b1a16-130">For example, a key created in key vault named `dataprotection` in the `contosokeyvault` has the key identifier `https://contosokeyvault.vault.azure.net/keys/dataprotection/`.</span></span> <span data-ttu-id="b1a16-131">提供具有解除包裝**金鑰的**應用程式和金鑰保存庫的**金鑰**許可權。</span><span class="sxs-lookup"><span data-stu-id="b1a16-131">Provide the app with **Unwrap Key** and **Wrap Key** permissions to the key vault.</span></span>

<span data-ttu-id="b1a16-132">`ProtectKeysWithAzureKeyVault` 多載：</span><span class="sxs-lookup"><span data-stu-id="b1a16-132">`ProtectKeysWithAzureKeyVault` overloads:</span></span>

* <span data-ttu-id="b1a16-133">[ProtectKeysWithAzureKeyVault （IDataProtectionBuilder，KeyVaultClient，String）](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_)允許使用[KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) ，讓資料保護系統能夠使用金鑰保存庫。</span><span class="sxs-lookup"><span data-stu-id="b1a16-133">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, KeyVaultClient, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_) permits the use of a [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) to enable the data protection system to use the key vault.</span></span>
* <span data-ttu-id="b1a16-134">[ProtectKeysWithAzureKeyVault （IDataProtectionBuilder，string，string，X509Certificate2）](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_)允許使用 `ClientId` 和[X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) ，讓資料保護系統能夠使用金鑰保存庫。</span><span class="sxs-lookup"><span data-stu-id="b1a16-134">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, String, String, X509Certificate2)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_) permits the use of a `ClientId` and [X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) to enable the data protection system to use the key vault.</span></span>
* <span data-ttu-id="b1a16-135">[ProtectKeysWithAzureKeyVault （IDataProtectionBuilder，string，string，string）](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_)允許使用 `ClientId` 和 `ClientSecret`，讓資料保護系統能夠使用金鑰保存庫。</span><span class="sxs-lookup"><span data-stu-id="b1a16-135">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, String, String, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_) permits the use of a `ClientId` and `ClientSecret` to enable the data protection system to use the key vault.</span></span>

::: moniker-end

## <a name="persistkeystofilesystem"></a><span data-ttu-id="b1a16-136">PersistKeysToFileSystem</span><span class="sxs-lookup"><span data-stu-id="b1a16-136">PersistKeysToFileSystem</span></span>

<span data-ttu-id="b1a16-137">若要將金鑰儲存在 UNC 共用上，而不是在 *% LOCALAPPDATA%* 的預設位置，請使用[PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem)設定系統：</span><span class="sxs-lookup"><span data-stu-id="b1a16-137">To store keys on a UNC share instead of at the *%LOCALAPPDATA%* default location, configure the system with [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

> [!WARNING]
> <span data-ttu-id="b1a16-138">如果您變更金鑰持續性位置，系統就不會再自動加密待用金鑰，因為它並不知道 DPAPI 是否為適當的加密機制。</span><span class="sxs-lookup"><span data-stu-id="b1a16-138">If you change the key persistence location, the system no longer automatically encrypts keys at rest, since it doesn't know whether DPAPI is an appropriate encryption mechanism.</span></span>

## <a name="protectkeyswith"></a><span data-ttu-id="b1a16-139">ProtectKeysWith\*</span><span class="sxs-lookup"><span data-stu-id="b1a16-139">ProtectKeysWith\*</span></span>

<span data-ttu-id="b1a16-140">您可以藉由呼叫任何[ProtectKeysWith\*](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions)設定 api 來設定系統來保護待用金鑰。</span><span class="sxs-lookup"><span data-stu-id="b1a16-140">You can configure the system to protect keys at rest by calling any of the [ProtectKeysWith\*](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) configuration APIs.</span></span> <span data-ttu-id="b1a16-141">請考慮下列範例，其會將金鑰儲存在 UNC 共用上，並使用特定的 x.509 憑證來加密待用金鑰：</span><span class="sxs-lookup"><span data-stu-id="b1a16-141">Consider the example below, which stores keys on a UNC share and encrypts those keys at rest with a specific X.509 certificate:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="b1a16-142">在 ASP.NET Core 2.1 或更新版本中，您可以提供[ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate)的[X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) ，例如從檔案載入的憑證：</span><span class="sxs-lookup"><span data-stu-id="b1a16-142">In ASP.NET Core 2.1 or later, you can provide an [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) to [ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate), such as a certificate loaded from a file:</span></span>

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

<span data-ttu-id="b1a16-143">如需有關內建金鑰加密機制的其他範例和討論，請參閱待用[金鑰加密](xref:security/data-protection/implementation/key-encryption-at-rest)。</span><span class="sxs-lookup"><span data-stu-id="b1a16-143">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more examples and discussion on the built-in key encryption mechanisms.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="unprotectkeyswithanycertificate"></a><span data-ttu-id="b1a16-144">UnprotectKeysWithAnyCertificate</span><span class="sxs-lookup"><span data-stu-id="b1a16-144">UnprotectKeysWithAnyCertificate</span></span>

<span data-ttu-id="b1a16-145">在 ASP.NET Core 2.1 或更新版本中，您可以使用具有[UnprotectKeysWithAnyCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.unprotectkeyswithanycertificate)的[X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2)憑證陣列，旋轉憑證並將待用金鑰解密：</span><span class="sxs-lookup"><span data-stu-id="b1a16-145">In ASP.NET Core 2.1 or later, you can rotate certificates and decrypt keys at rest using an array of [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) certificates with [UnprotectKeysWithAnyCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.unprotectkeyswithanycertificate):</span></span>

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

## <a name="setdefaultkeylifetime"></a><span data-ttu-id="b1a16-146">SetDefaultKeyLifetime</span><span class="sxs-lookup"><span data-stu-id="b1a16-146">SetDefaultKeyLifetime</span></span>

<span data-ttu-id="b1a16-147">若要將系統設定為使用14天的金鑰存留期，而不是預設的90天，請使用[SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime)：</span><span class="sxs-lookup"><span data-stu-id="b1a16-147">To configure the system to use a key lifetime of 14 days instead of the default 90 days, use [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

## <a name="setapplicationname"></a><span data-ttu-id="b1a16-148">SetApplicationName</span><span class="sxs-lookup"><span data-stu-id="b1a16-148">SetApplicationName</span></span>

<span data-ttu-id="b1a16-149">根據預設，資料保護系統會依據其[內容根](xref:fundamentals/index#content-root)路徑，將應用程式彼此隔離，即使它們共用相同的實體金鑰存放庫也是一樣。</span><span class="sxs-lookup"><span data-stu-id="b1a16-149">By default, the Data Protection system isolates apps from one another based on their [content root](xref:fundamentals/index#content-root) paths, even if they're sharing the same physical key repository.</span></span> <span data-ttu-id="b1a16-150">這可防止應用程式瞭解彼此受保護的裝載。</span><span class="sxs-lookup"><span data-stu-id="b1a16-150">This prevents the apps from understanding each other's protected payloads.</span></span>

<span data-ttu-id="b1a16-151">若要在應用程式之間共用受保護的承載：</span><span class="sxs-lookup"><span data-stu-id="b1a16-151">To share protected payloads among apps:</span></span>

* <span data-ttu-id="b1a16-152">使用相同的值來設定每個應用程式中的 <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*>。</span><span class="sxs-lookup"><span data-stu-id="b1a16-152">Configure <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> in each app with the same value.</span></span>
* <span data-ttu-id="b1a16-153">跨應用程式使用相同版本的資料保護 API 堆疊。</span><span class="sxs-lookup"><span data-stu-id="b1a16-153">Use the same version of the Data Protection API stack across the apps.</span></span> <span data-ttu-id="b1a16-154">在應用程式的專案檔案中執行下列**其中一**項：</span><span class="sxs-lookup"><span data-stu-id="b1a16-154">Perform **either** of the following in the apps' project files:</span></span>
  * <span data-ttu-id="b1a16-155">透過[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)參考相同的共用架構版本。</span><span class="sxs-lookup"><span data-stu-id="b1a16-155">Reference the same shared framework version via the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
  * <span data-ttu-id="b1a16-156">參考相同的[資料保護套件](xref:security/data-protection/introduction#package-layout)版本。</span><span class="sxs-lookup"><span data-stu-id="b1a16-156">Reference the same [Data Protection package](xref:security/data-protection/introduction#package-layout) version.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("shared app name");
}
```

## <a name="disableautomatickeygeneration"></a><span data-ttu-id="b1a16-157">DisableAutomaticKeyGeneration</span><span class="sxs-lookup"><span data-stu-id="b1a16-157">DisableAutomaticKeyGeneration</span></span>

<span data-ttu-id="b1a16-158">您可能會想要讓應用程式在接近到期的情況下自動變換金鑰（建立新的金鑰）。</span><span class="sxs-lookup"><span data-stu-id="b1a16-158">You may have a scenario where you don't want an app to automatically roll keys (create new keys) as they approach expiration.</span></span> <span data-ttu-id="b1a16-159">其中一個範例可能是在主要/次要關係中設定的應用程式，其中只有主要應用程式會負責金鑰管理考慮，而次要應用程式只會有金鑰環的唯讀視圖。</span><span class="sxs-lookup"><span data-stu-id="b1a16-159">One example of this might be apps set up in a primary/secondary relationship, where only the primary app is responsible for key management concerns and secondary apps simply have a read-only view of the key ring.</span></span> <span data-ttu-id="b1a16-160">您可以設定具有 <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.DisableAutomaticKeyGeneration*>的系統，將次要應用程式設定為將金鑰環視為唯讀：</span><span class="sxs-lookup"><span data-stu-id="b1a16-160">The secondary apps can be configured to treat the key ring as read-only by configuring the system with <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.DisableAutomaticKeyGeneration*>:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

## <a name="per-application-isolation"></a><span data-ttu-id="b1a16-161">每個應用程式隔離</span><span class="sxs-lookup"><span data-stu-id="b1a16-161">Per-application isolation</span></span>

<span data-ttu-id="b1a16-162">當資料保護系統是由 ASP.NET Core 主機提供時，它會自動將應用程式彼此隔離，即使這些應用程式是在相同的工作者進程帳戶下執行，而且使用相同的主要金鑰內容。</span><span class="sxs-lookup"><span data-stu-id="b1a16-162">When the Data Protection system is provided by an ASP.NET Core host, it automatically isolates apps from one another, even if those apps are running under the same worker process account and are using the same master keying material.</span></span> <span data-ttu-id="b1a16-163">這有點類似于 System.web 的 `<machineKey>` 元素中的 IsolateApps 修飾詞。</span><span class="sxs-lookup"><span data-stu-id="b1a16-163">This is somewhat similar to the IsolateApps modifier from System.Web's `<machineKey>` element.</span></span>

<span data-ttu-id="b1a16-164">隔離機制的運作方式是將本機電腦上的每個應用程式視為唯一的租使用者，因此任何指定應用程式的根 <xref:Microsoft.AspNetCore.DataProtection.IDataProtector> 會自動將應用程式識別碼包含為鑒別子。</span><span class="sxs-lookup"><span data-stu-id="b1a16-164">The isolation mechanism works by considering each app on the local machine as a unique tenant, thus the <xref:Microsoft.AspNetCore.DataProtection.IDataProtector> rooted for any given app automatically includes the app ID as a discriminator.</span></span> <span data-ttu-id="b1a16-165">應用程式的唯一識別碼是應用程式的實體路徑：</span><span class="sxs-lookup"><span data-stu-id="b1a16-165">The app's unique ID is the app's physical path:</span></span>

* <span data-ttu-id="b1a16-166">對於裝載于 IIS 中的應用程式，唯一識別碼是應用程式的 IIS 實體路徑。</span><span class="sxs-lookup"><span data-stu-id="b1a16-166">For apps hosted in IIS, the unique ID is the IIS physical path of the app.</span></span> <span data-ttu-id="b1a16-167">如果應用程式部署在 web 伺服陣列環境中，此值會穩定，假設 IIS 環境在 web 伺服陣列中的所有電腦上都有類似的設定。</span><span class="sxs-lookup"><span data-stu-id="b1a16-167">If an app is deployed in a web farm environment, this value is stable assuming that the IIS environments are configured similarly across all machines in the web farm.</span></span>
* <span data-ttu-id="b1a16-168">對於在[Kestrel 伺服器](xref:fundamentals/servers/index#kestrel)上執行的自我裝載應用程式，唯一識別碼是磁片上應用程式的實體路徑。</span><span class="sxs-lookup"><span data-stu-id="b1a16-168">For self-hosted apps running on the [Kestrel server](xref:fundamentals/servers/index#kestrel), the unique ID is the physical path to the app on disk.</span></span>

<span data-ttu-id="b1a16-169">唯一識別碼是設計用來在個別應用程式和電腦本身的&mdash;重設。</span><span class="sxs-lookup"><span data-stu-id="b1a16-169">The unique identifier is designed to survive resets&mdash;both of the individual app and of the machine itself.</span></span>

<span data-ttu-id="b1a16-170">這種隔離機制會假設應用程式不是惡意的。</span><span class="sxs-lookup"><span data-stu-id="b1a16-170">This isolation mechanism assumes that the apps are not malicious.</span></span> <span data-ttu-id="b1a16-171">惡意應用程式一律會影響在同一個工作者進程帳戶下執行的任何其他應用程式。</span><span class="sxs-lookup"><span data-stu-id="b1a16-171">A malicious app can always impact any other app running under the same worker process account.</span></span> <span data-ttu-id="b1a16-172">在應用程式互相不受信任的共用裝載環境中，主機服務提供者應採取步驟，以確保應用程式之間的作業系統層級隔離，包括分隔應用程式的基礎金鑰存放庫。</span><span class="sxs-lookup"><span data-stu-id="b1a16-172">In a shared hosting environment where apps are mutually untrusted, the hosting provider should take steps to ensure OS-level isolation between apps, including separating the apps' underlying key repositories.</span></span>

<span data-ttu-id="b1a16-173">如果資料保護系統不是由 ASP.NET Core 主機所提供（例如，如果您透過 `DataProtectionProvider` 具象類型來具現化），則預設會停用應用程式隔離。</span><span class="sxs-lookup"><span data-stu-id="b1a16-173">If the Data Protection system isn't provided by an ASP.NET Core host (for example, if you instantiate it via the `DataProtectionProvider` concrete type) app isolation is disabled by default.</span></span> <span data-ttu-id="b1a16-174">當應用程式隔離停用時，所有由相同的金鑰內容所支援的應用程式都可以共用承載，只要它們提供適當的[用途](xref:security/data-protection/consumer-apis/purpose-strings)即可。</span><span class="sxs-lookup"><span data-stu-id="b1a16-174">When app isolation is disabled, all apps backed by the same keying material can share payloads as long as they provide the appropriate [purposes](xref:security/data-protection/consumer-apis/purpose-strings).</span></span> <span data-ttu-id="b1a16-175">若要在此環境中提供應用程式隔離，請在設定物件上呼叫[SetApplicationName](#setapplicationname)方法，並為每個應用程式提供唯一的名稱。</span><span class="sxs-lookup"><span data-stu-id="b1a16-175">To provide app isolation in this environment, call the [SetApplicationName](#setapplicationname) method on the configuration object and provide a unique name for each app.</span></span>

## <a name="changing-algorithms-with-usecryptographicalgorithms"></a><span data-ttu-id="b1a16-176">使用 UseCryptographicAlgorithms 變更演算法</span><span class="sxs-lookup"><span data-stu-id="b1a16-176">Changing algorithms with UseCryptographicAlgorithms</span></span>

<span data-ttu-id="b1a16-177">資料保護堆疊可讓您變更新產生的索引鍵所使用的預設演算法。</span><span class="sxs-lookup"><span data-stu-id="b1a16-177">The Data Protection stack allows you to change the default algorithm used by newly-generated keys.</span></span> <span data-ttu-id="b1a16-178">若要這麼做，最簡單的方法是從設定回呼呼叫[UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) ：</span><span class="sxs-lookup"><span data-stu-id="b1a16-178">The simplest way to do this is to call [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) from the configuration callback:</span></span>

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

<span data-ttu-id="b1a16-179">預設 EncryptionAlgorithm 為 AES-256-CBC，而預設 ValidationAlgorithm 為 HMACSHA256。</span><span class="sxs-lookup"><span data-stu-id="b1a16-179">The default EncryptionAlgorithm is AES-256-CBC, and the default ValidationAlgorithm is HMACSHA256.</span></span> <span data-ttu-id="b1a16-180">系統管理員可以透過[整部電腦的原則](xref:security/data-protection/configuration/machine-wide-policy)來設定預設原則，但是明確呼叫 `UseCryptographicAlgorithms` 會覆寫預設原則。</span><span class="sxs-lookup"><span data-stu-id="b1a16-180">The default policy can be set by a system administrator via a [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy), but an explicit call to `UseCryptographicAlgorithms` overrides the default policy.</span></span>

<span data-ttu-id="b1a16-181">呼叫 `UseCryptographicAlgorithms` 可讓您從預先定義的內建清單中指定所需的演算法。</span><span class="sxs-lookup"><span data-stu-id="b1a16-181">Calling `UseCryptographicAlgorithms` allows you to specify the desired algorithm from a predefined built-in list.</span></span> <span data-ttu-id="b1a16-182">您不需要擔心演算法的執行。</span><span class="sxs-lookup"><span data-stu-id="b1a16-182">You don't need to worry about the implementation of the algorithm.</span></span> <span data-ttu-id="b1a16-183">在上述案例中，如果在 Windows 上執行，資料保護系統會嘗試使用 AES 的 CNG 實行。</span><span class="sxs-lookup"><span data-stu-id="b1a16-183">In the scenario above, the Data Protection system attempts to use the CNG implementation of AES if running on Windows.</span></span> <span data-ttu-id="b1a16-184">否則，它會回復[至受管理](/dotnet/api/system.security.cryptography.aes)的 system.string 類別。</span><span class="sxs-lookup"><span data-stu-id="b1a16-184">Otherwise, it falls back to the managed [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) class.</span></span>

<span data-ttu-id="b1a16-185">您可以透過呼叫[UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms)來手動指定執行。</span><span class="sxs-lookup"><span data-stu-id="b1a16-185">You can manually specify an implementation via a call to [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span></span>

> [!TIP]
> <span data-ttu-id="b1a16-186">變更演算法不會影響金鑰環中的現有索引鍵。</span><span class="sxs-lookup"><span data-stu-id="b1a16-186">Changing algorithms doesn't affect existing keys in the key ring.</span></span> <span data-ttu-id="b1a16-187">它只會影響新產生的金鑰。</span><span class="sxs-lookup"><span data-stu-id="b1a16-187">It only affects newly-generated keys.</span></span>

### <a name="specifying-custom-managed-algorithms"></a><span data-ttu-id="b1a16-188">指定自訂受控演算法</span><span class="sxs-lookup"><span data-stu-id="b1a16-188">Specifying custom managed algorithms</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="b1a16-189">若要指定自訂的 managed 演算法，請建立指向執行類型的[ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration)實例：</span><span class="sxs-lookup"><span data-stu-id="b1a16-189">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) instance that points to the implementation types:</span></span>

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

<span data-ttu-id="b1a16-190">若要指定自訂的 managed 演算法，請建立指向執行類型的[ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings)實例：</span><span class="sxs-lookup"><span data-stu-id="b1a16-190">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) instance that points to the implementation types:</span></span>

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

<span data-ttu-id="b1a16-191">一般來說，\*型別屬性必須指向[system.security.cryptography.symmetricalgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm)和[KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm)的具象、可具現化（透過公用無參數 ctor）的實作為，不過，為了方便起見，`typeof(Aes)` 有一些值，例如。</span><span class="sxs-lookup"><span data-stu-id="b1a16-191">Generally the \*Type properties must point to concrete, instantiable (via a public parameterless ctor) implementations of [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) and [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), though the system special-cases some values like `typeof(Aes)` for convenience.</span></span>

> [!NOTE]
> <span data-ttu-id="b1a16-192">System.security.cryptography.symmetricalgorithm 必須具有≥128位的金鑰長度和≥64位的區塊大小，而且它必須支援具有 PKCS #7 填補的 CBC 模式加密。</span><span class="sxs-lookup"><span data-stu-id="b1a16-192">The SymmetricAlgorithm must have a key length of ≥ 128 bits and a block size of ≥ 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="b1a16-193">KeyedHashAlgorithm 必須具有 > = 128 位的摘要大小，而且它必須支援長度等於雜湊演算法摘要長度的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="b1a16-193">The KeyedHashAlgorithm must have a digest size of >= 128 bits, and it must support keys of length equal to the hash algorithm's digest length.</span></span> <span data-ttu-id="b1a16-194">KeyedHashAlgorithm 不一定要是 HMAC。</span><span class="sxs-lookup"><span data-stu-id="b1a16-194">The KeyedHashAlgorithm isn't strictly required to be HMAC.</span></span>

### <a name="specifying-custom-windows-cng-algorithms"></a><span data-ttu-id="b1a16-195">指定自訂的 Windows CNG 演算法</span><span class="sxs-lookup"><span data-stu-id="b1a16-195">Specifying custom Windows CNG algorithms</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="b1a16-196">若要使用 CBC 模式加密搭配 HMAC 驗證來指定自訂 Windows CNG 演算法，請建立包含演算法資訊的[CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration)實例：</span><span class="sxs-lookup"><span data-stu-id="b1a16-196">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

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

<span data-ttu-id="b1a16-197">若要使用 CBC 模式加密搭配 HMAC 驗證來指定自訂 Windows CNG 演算法，請建立包含演算法資訊的[CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings)實例：</span><span class="sxs-lookup"><span data-stu-id="b1a16-197">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

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
> <span data-ttu-id="b1a16-198">對稱式區塊加密演算法的金鑰長度必須為 > = 128 位、區塊大小 > = 64 位，而且必須支援具有 PKCS #7 填補的 CBC 模式加密。</span><span class="sxs-lookup"><span data-stu-id="b1a16-198">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of >= 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="b1a16-199">雜湊演算法的摘要大小必須為 > = 128 位，而且必須支援以 BCRYPT\_ALG 來開啟，\_處理\_HMAC\_旗標旗標。</span><span class="sxs-lookup"><span data-stu-id="b1a16-199">The hash algorithm must have a digest size of >= 128 bits and must support being opened with the BCRYPT\_ALG\_HANDLE\_HMAC\_FLAG flag.</span></span> <span data-ttu-id="b1a16-200">\*提供者屬性可以設定為 null，以針對指定的演算法使用預設提供者。</span><span class="sxs-lookup"><span data-stu-id="b1a16-200">The \*Provider properties can be set to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="b1a16-201">如需詳細資訊，請參閱[BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx)檔。</span><span class="sxs-lookup"><span data-stu-id="b1a16-201">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="b1a16-202">若要使用 Galois/計數器模式加密搭配驗證來指定自訂的 Windows CNG 演算法，請建立包含演算法資訊的[CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration)實例：</span><span class="sxs-lookup"><span data-stu-id="b1a16-202">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

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

<span data-ttu-id="b1a16-203">若要使用 Galois/計數器模式加密搭配驗證來指定自訂的 Windows CNG 演算法，請建立包含演算法資訊的[CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings)實例：</span><span class="sxs-lookup"><span data-stu-id="b1a16-203">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

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
> <span data-ttu-id="b1a16-204">對稱式區塊加密演算法的金鑰長度必須為 > = 128 位、剛好為128位的區塊大小，而且必須支援 GCM 加密。</span><span class="sxs-lookup"><span data-stu-id="b1a16-204">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of exactly 128 bits, and it must support GCM encryption.</span></span> <span data-ttu-id="b1a16-205">您可以將[EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider)屬性設定為 null，以針對指定的演算法使用預設提供者。</span><span class="sxs-lookup"><span data-stu-id="b1a16-205">You can set the [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) property to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="b1a16-206">如需詳細資訊，請參閱[BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx)檔。</span><span class="sxs-lookup"><span data-stu-id="b1a16-206">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

### <a name="specifying-other-custom-algorithms"></a><span data-ttu-id="b1a16-207">指定其他自訂演算法</span><span class="sxs-lookup"><span data-stu-id="b1a16-207">Specifying other custom algorithms</span></span>

<span data-ttu-id="b1a16-208">雖然不會公開為第一類的 API，但資料保護系統的擴充性足以讓您指定幾乎任何類型的演算法。</span><span class="sxs-lookup"><span data-stu-id="b1a16-208">Though not exposed as a first-class API, the Data Protection system is extensible enough to allow specifying almost any kind of algorithm.</span></span> <span data-ttu-id="b1a16-209">例如，您可以保留硬體安全模組（HSM）中包含的所有金鑰，以及提供核心加密和解密常式的自訂執行。</span><span class="sxs-lookup"><span data-stu-id="b1a16-209">For example, it's possible to keep all keys contained within a Hardware Security Module (HSM) and to provide a custom implementation of the core encryption and decryption routines.</span></span> <span data-ttu-id="b1a16-210">如需詳細資訊，請參閱[核心密碼](xref:security/data-protection/extensibility/core-crypto)編譯擴充性中的[IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) 。</span><span class="sxs-lookup"><span data-stu-id="b1a16-210">See [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) in [Core cryptography extensibility](xref:security/data-protection/extensibility/core-crypto) for more information.</span></span>

## <a name="persisting-keys-when-hosting-in-a-docker-container"></a><span data-ttu-id="b1a16-211">在 Docker 容器中裝載時保存金鑰</span><span class="sxs-lookup"><span data-stu-id="b1a16-211">Persisting keys when hosting in a Docker container</span></span>

<span data-ttu-id="b1a16-212">在[Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/)容器中裝載時，應該在下列其中一項維護金鑰：</span><span class="sxs-lookup"><span data-stu-id="b1a16-212">When hosting in a [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) container, keys should be maintained in either:</span></span>

* <span data-ttu-id="b1a16-213">一種資料夾，其為保存在容器存留期之外的 Docker 磁片區，例如共用磁片區或裝載掛接的磁片區。</span><span class="sxs-lookup"><span data-stu-id="b1a16-213">A folder that's a Docker volume that persists beyond the container's lifetime, such as a shared volume or a host-mounted volume.</span></span>
* <span data-ttu-id="b1a16-214">外部提供者，例如[Azure Key Vault](https://azure.microsoft.com/services/key-vault/)或[Redis](https://redis.io/)。</span><span class="sxs-lookup"><span data-stu-id="b1a16-214">An external provider, such as [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) or [Redis](https://redis.io/).</span></span>

## <a name="persisting-keys-with-redis"></a><span data-ttu-id="b1a16-215">使用 Redis 保存金鑰</span><span class="sxs-lookup"><span data-stu-id="b1a16-215">Persisting keys with Redis</span></span>

<span data-ttu-id="b1a16-216">只有支援[Redis 資料持續](/azure/azure-cache-for-redis/cache-how-to-premium-persistence)性的 Redis 版本才應該用來儲存索引鍵。</span><span class="sxs-lookup"><span data-stu-id="b1a16-216">Only Redis versions supporting [Redis Data Persistence](/azure/azure-cache-for-redis/cache-how-to-premium-persistence) should be used to store keys.</span></span> <span data-ttu-id="b1a16-217">[Azure Blob 儲存體](/azure/storage/blobs/storage-blobs-introduction)是持續性的，可以用來儲存金鑰。</span><span class="sxs-lookup"><span data-stu-id="b1a16-217">[Azure Blob storage](/azure/storage/blobs/storage-blobs-introduction) is persistent and can be used to store keys.</span></span> <span data-ttu-id="b1a16-218">如需詳細資訊，請參閱[這個 GitHub 問題](https://github.com/dotnet/AspNetCore/issues/13476) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="b1a16-218">For more information, see [this GitHub issue](https://github.com/dotnet/AspNetCore/issues/13476).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b1a16-219">其他資源</span><span class="sxs-lookup"><span data-stu-id="b1a16-219">Additional resources</span></span>

* <xref:security/data-protection/configuration/non-di-scenarios>
* <xref:security/data-protection/configuration/machine-wide-policy>
* <xref:host-and-deploy/web-farm>
* <xref:security/data-protection/implementation/key-storage-providers>
