---
title: 設定 ASP.NET Core 資料保護
author: rick-anderson
description: 了解如何設定 ASP.NET Core 中的資料保護。
ms.author: riande
ms.custom: mvc
ms.date: 07/17/2017
uid: security/data-protection/configuration/overview
ms.openlocfilehash: b2fa1921120d297f4b0dbb0294a00e0e573f4e04
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272575"
---
# <a name="configure-aspnet-core-data-protection"></a><span data-ttu-id="aecf6-103">設定 ASP.NET Core 資料保護</span><span class="sxs-lookup"><span data-stu-id="aecf6-103">Configure ASP.NET Core Data Protection</span></span>

<span data-ttu-id="aecf6-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="aecf6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="aecf6-105">當初始化資料保護系統時，它會套用[預設設定](xref:security/data-protection/configuration/default-settings)根據作業環境。</span><span class="sxs-lookup"><span data-stu-id="aecf6-105">When the Data Protection system is initialized, it applies [default settings](xref:security/data-protection/configuration/default-settings) based on the operational environment.</span></span> <span data-ttu-id="aecf6-106">這些設定是通常適用於在單一機器上執行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="aecf6-106">These settings are generally appropriate for apps running on a single machine.</span></span> <span data-ttu-id="aecf6-107">一些情況下，在開發人員可能想要變更預設設定：</span><span class="sxs-lookup"><span data-stu-id="aecf6-107">There are cases where a developer may want to change the default settings:</span></span>

* <span data-ttu-id="aecf6-108">應用程式則會分散到多部電腦。</span><span class="sxs-lookup"><span data-stu-id="aecf6-108">The app is spread across multiple machines.</span></span>
* <span data-ttu-id="aecf6-109">基於相容性因素。</span><span class="sxs-lookup"><span data-stu-id="aecf6-109">For compliance reasons.</span></span>

<span data-ttu-id="aecf6-110">這些案例中，資料保護系統會提供豐富的組態 API。</span><span class="sxs-lookup"><span data-stu-id="aecf6-110">For these scenarios, the Data Protection system offers a rich configuration API.</span></span>

> [!WARNING]
> <span data-ttu-id="aecf6-111">類似於組態檔，資料保護鑰匙圈應受到使用適當的權限。</span><span class="sxs-lookup"><span data-stu-id="aecf6-111">Similar to configuration files, the data protection key ring should be protected using appropriate permissions.</span></span> <span data-ttu-id="aecf6-112">您可以選擇加密在靜止時，金鑰，但這不會防止攻擊者建立新的金鑰。</span><span class="sxs-lookup"><span data-stu-id="aecf6-112">You can choose to encrypt keys at rest, but this doesn't prevent attackers from creating new keys.</span></span> <span data-ttu-id="aecf6-113">因此，您的應用程式安全性會受到影響。</span><span class="sxs-lookup"><span data-stu-id="aecf6-113">Consequently, your app's security is impacted.</span></span> <span data-ttu-id="aecf6-114">使用資料保護設定的儲存位置應該限制為應用程式本身，您會保護組態檔的方式類似其存取權。</span><span class="sxs-lookup"><span data-stu-id="aecf6-114">The storage location configured with Data Protection should have its access limited to the app itself, similar to the way you would protect configuration files.</span></span> <span data-ttu-id="aecf6-115">例如，如果您選擇儲存在磁碟上的索引鍵信號，使用檔案系統權限。</span><span class="sxs-lookup"><span data-stu-id="aecf6-115">For example, if you choose to store your key ring on disk, use file system permissions.</span></span> <span data-ttu-id="aecf6-116">確保識別下的執行您 web 應用程式具有讀取、 寫入和建立該目錄的存取權。</span><span class="sxs-lookup"><span data-stu-id="aecf6-116">Ensure only the identity under which your web app runs has read, write, and create access to that directory.</span></span> <span data-ttu-id="aecf6-117">如果您使用 Azure 資料表儲存體時，只有 web 應用程式應該有讀取、 寫入或建立新項目中的資料表存放區、 等等的能力。</span><span class="sxs-lookup"><span data-stu-id="aecf6-117">If you use Azure Table Storage, only the web app should have the ability to read, write, or create new entries in the table store, etc.</span></span>
>
> <span data-ttu-id="aecf6-118">擴充方法[AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection)傳回[IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder)。</span><span class="sxs-lookup"><span data-stu-id="aecf6-118">The extension method [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) returns an [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span></span> <span data-ttu-id="aecf6-119">`IDataProtectionBuilder` 會公開，您可以鏈結在一起選項來設定資料保護的擴充方法。</span><span class="sxs-lookup"><span data-stu-id="aecf6-119">`IDataProtectionBuilder` exposes extension methods that you can chain together to configure Data Protection options.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="protectkeyswithazurekeyvault"></a><span data-ttu-id="aecf6-120">ProtectKeysWithAzureKeyVault</span><span class="sxs-lookup"><span data-stu-id="aecf6-120">ProtectKeysWithAzureKeyVault</span></span>

<span data-ttu-id="aecf6-121">若要儲存在金鑰[Azure 金鑰保存庫](https://azure.microsoft.com/services/key-vault/)，設定與系統[ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault)中`Startup`類別：</span><span class="sxs-lookup"><span data-stu-id="aecf6-121">To store keys in [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), configure the system with [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) in the `Startup` class:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

<span data-ttu-id="aecf6-122">設定鑰匙圈儲存體位置 (例如， [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage))。</span><span class="sxs-lookup"><span data-stu-id="aecf6-122">Set the key ring storage location (for example, [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)).</span></span> <span data-ttu-id="aecf6-123">必須設定的位置，因為呼叫`ProtectKeysWithAzureKeyVault`實作[IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor)停用自動資料保護設定，包括鑰匙圈儲存位置。</span><span class="sxs-lookup"><span data-stu-id="aecf6-123">The location must be set because calling `ProtectKeysWithAzureKeyVault` implements an [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor) that disables automatic data protection settings, including the key ring storage location.</span></span> <span data-ttu-id="aecf6-124">上述範例會使用 Azure Blob 儲存體，以保存金鑰環形。</span><span class="sxs-lookup"><span data-stu-id="aecf6-124">The preceding example uses Azure Blob Storage to persist the key ring.</span></span> <span data-ttu-id="aecf6-125">如需詳細資訊，請參閱[金鑰儲存提供者： Azure 與 Redis](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis)。</span><span class="sxs-lookup"><span data-stu-id="aecf6-125">For more information, see [Key storage providers: Azure and Redis](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis).</span></span> <span data-ttu-id="aecf6-126">您也可以保存在本機以鑰匙圈[PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system)。</span><span class="sxs-lookup"><span data-stu-id="aecf6-126">You can also persist the key ring locally with [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system).</span></span>

<span data-ttu-id="aecf6-127">`keyIdentifier`是用於金鑰加密的金鑰保存庫金鑰識別碼 (例如， `https://contosokeyvault.vault.azure.net/keys/dataprotection/`)。</span><span class="sxs-lookup"><span data-stu-id="aecf6-127">The `keyIdentifier` is the key vault key identifier used for key encryption (for example, `https://contosokeyvault.vault.azure.net/keys/dataprotection/`).</span></span>

<span data-ttu-id="aecf6-128">`ProtectKeysWithAzureKeyVault` 多載：</span><span class="sxs-lookup"><span data-stu-id="aecf6-128">`ProtectKeysWithAzureKeyVault` overloads:</span></span>

* <span data-ttu-id="aecf6-129">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder，KeyVaultClient，String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_)允許使用[KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)啟用資料保護系統，若要使用金鑰保存庫。</span><span class="sxs-lookup"><span data-stu-id="aecf6-129">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, KeyVaultClient, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_) permits the use of a [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) to enable the data protection system to use the key vault.</span></span>
* <span data-ttu-id="aecf6-130">[ProtectKeysWithAzureKeyVault （IDataProtectionBuilder、 字串、 字串、 X509Certificate2）](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_)允許使用`ClientId`和[X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2)啟用資料保護系統，若要使用金鑰保存庫。</span><span class="sxs-lookup"><span data-stu-id="aecf6-130">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, String, String, X509Certificate2)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_) permits the use of a `ClientId` and [X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) to enable the data protection system to use the key vault.</span></span>
* <span data-ttu-id="aecf6-131">[ProtectKeysWithAzureKeyVault （IDataProtectionBuilder、 字串、 字串、 字串）](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_)允許使用`ClientId`和`ClientSecret`啟用資料保護系統，若要使用金鑰保存庫。</span><span class="sxs-lookup"><span data-stu-id="aecf6-131">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, String, String, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_) permits the use of a `ClientId` and `ClientSecret` to enable the data protection system to use the key vault.</span></span>

::: moniker-end

## <a name="persistkeystofilesystem"></a><span data-ttu-id="aecf6-132">PersistKeysToFileSystem</span><span class="sxs-lookup"><span data-stu-id="aecf6-132">PersistKeysToFileSystem</span></span>

<span data-ttu-id="aecf6-133">若要將金鑰儲存在 UNC 共用而不是在 *%LOCALAPPDATA%* 預設位置，設定與系統[PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span><span class="sxs-lookup"><span data-stu-id="aecf6-133">To store keys on a UNC share instead of at the *%LOCALAPPDATA%* default location, configure the system with [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

> [!WARNING]
> <span data-ttu-id="aecf6-134">如果您變更金鑰的持續性位置時，系統將不會再自動加密靜止的索引鍵因為它不知道是否 DPAPI 是適當的加密機制。</span><span class="sxs-lookup"><span data-stu-id="aecf6-134">If you change the key persistence location, the system no longer automatically encrypts keys at rest, since it doesn't know whether DPAPI is an appropriate encryption mechanism.</span></span>

## <a name="protectkeyswith"></a><span data-ttu-id="aecf6-135">ProtectKeysWith\*</span><span class="sxs-lookup"><span data-stu-id="aecf6-135">ProtectKeysWith\*</span></span>

<span data-ttu-id="aecf6-136">您可以設定系統以保護靜止的金鑰呼叫上述任一[ProtectKeysWith\* ](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions)組態 Api。</span><span class="sxs-lookup"><span data-stu-id="aecf6-136">You can configure the system to protect keys at rest by calling any of the [ProtectKeysWith\*](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) configuration APIs.</span></span> <span data-ttu-id="aecf6-137">請考慮下列範例中，將金鑰儲存在 UNC 共用，且加密待用與特定 X.509 憑證的金鑰：</span><span class="sxs-lookup"><span data-stu-id="aecf6-137">Consider the example below, which stores keys on a UNC share and encrypts those keys at rest with a specific X.509 certificate:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

<span data-ttu-id="aecf6-138">請參閱[金鑰靜態加密](xref:security/data-protection/implementation/key-encryption-at-rest)如需詳細的範例和內建的金鑰加密機制的討論。</span><span class="sxs-lookup"><span data-stu-id="aecf6-138">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more examples and discussion on the built-in key encryption mechanisms.</span></span>

## <a name="setdefaultkeylifetime"></a><span data-ttu-id="aecf6-139">SetDefaultKeyLifetime</span><span class="sxs-lookup"><span data-stu-id="aecf6-139">SetDefaultKeyLifetime</span></span>

<span data-ttu-id="aecf6-140">若要設定系統而非預設的 90 天使用的金鑰的存留期為 14 天，請使用[SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span><span class="sxs-lookup"><span data-stu-id="aecf6-140">To configure the system to use a key lifetime of 14 days instead of the default 90 days, use [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

## <a name="setapplicationname"></a><span data-ttu-id="aecf6-141">SetApplicationName</span><span class="sxs-lookup"><span data-stu-id="aecf6-141">SetApplicationName</span></span>

<span data-ttu-id="aecf6-142">根據預設，資料保護系統隔離應用程式，即使它們共用相同的實體索引鍵儲存機制。</span><span class="sxs-lookup"><span data-stu-id="aecf6-142">By default, the Data Protection system isolates apps from one another, even if they're sharing the same physical key repository.</span></span> <span data-ttu-id="aecf6-143">這可防止應用程式的了解其他人的受保護的內容。</span><span class="sxs-lookup"><span data-stu-id="aecf6-143">This prevents the apps from understanding each other's protected payloads.</span></span> <span data-ttu-id="aecf6-144">若要共用受保護的裝載兩個應用程式之間，使用[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname)與每個應用程式相同的值：</span><span class="sxs-lookup"><span data-stu-id="aecf6-144">To share protected payloads between two apps, use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) with the same value for each app:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("shared app name");
}
```

## <a name="disableautomatickeygeneration"></a><span data-ttu-id="aecf6-145">DisableAutomaticKeyGeneration</span><span class="sxs-lookup"><span data-stu-id="aecf6-145">DisableAutomaticKeyGeneration</span></span>

<span data-ttu-id="aecf6-146">您可能需要的案例，您不想自動回復 （建立新的索引鍵） 的索引鍵，因為它們會很接近到期的應用程式。</span><span class="sxs-lookup"><span data-stu-id="aecf6-146">You may have a scenario where you don't want an app to automatically roll keys (create new keys) as they approach expiration.</span></span> <span data-ttu-id="aecf6-147">其中一個範例，這可能是應用程式設定中的主要/次要關聯性，其中只有主要的應用程式是負責金鑰管理問題，而次要應用程式只需要鑰匙圈的唯讀檢視。</span><span class="sxs-lookup"><span data-stu-id="aecf6-147">One example of this might be apps set up in a primary/secondary relationship, where only the primary app is responsible for key management concerns and secondary apps simply have a read-only view of the key ring.</span></span> <span data-ttu-id="aecf6-148">次要的應用程式可以設定將鑰匙圈視為唯讀，藉由設定與系統[DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):</span><span class="sxs-lookup"><span data-stu-id="aecf6-148">The secondary apps can be configured to treat the key ring as read-only by configuring the system with [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

## <a name="per-application-isolation"></a><span data-ttu-id="aecf6-149">每個應用程式隔離</span><span class="sxs-lookup"><span data-stu-id="aecf6-149">Per-application isolation</span></span>

<span data-ttu-id="aecf6-150">ASP.NET Core 主機所提供資料保護系統時，它會自動會隔離應用程式，即使這些應用程式相同的背景工作處理序帳戶下執行，並使用相同的主要金鑰處理內容。</span><span class="sxs-lookup"><span data-stu-id="aecf6-150">When the Data Protection system is provided by an ASP.NET Core host, it automatically isolates apps from one another, even if those apps are running under the same worker process account and are using the same master keying material.</span></span> <span data-ttu-id="aecf6-151">這是有點像 IsolateApps 修飾詞將來自 System.Web 的 **\<machineKey >** 項目。</span><span class="sxs-lookup"><span data-stu-id="aecf6-151">This is somewhat similar to the IsolateApps modifier from System.Web's **\<machineKey>** element.</span></span>

<span data-ttu-id="aecf6-152">隔離機制的運作方式是做為唯一的租用戶，考慮在本機電腦上的每個應用程式，因此[IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector)根項目，針對任何給定的應用程式會自動包含應用程式識別碼，表示為鑑別子。</span><span class="sxs-lookup"><span data-stu-id="aecf6-152">The isolation mechanism works by considering each app on the local machine as a unique tenant, thus the [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) rooted for any given app automatically includes the app ID as a discriminator.</span></span> <span data-ttu-id="aecf6-153">應用程式的唯一識別碼來自兩個地方的其中一個：</span><span class="sxs-lookup"><span data-stu-id="aecf6-153">The app's unique ID comes from one of two places:</span></span>

1. <span data-ttu-id="aecf6-154">如果應用程式裝載在 IIS 中，唯一的識別項是應用程式的組態路徑。</span><span class="sxs-lookup"><span data-stu-id="aecf6-154">If the app is hosted in IIS, the unique identifier is the app's configuration path.</span></span> <span data-ttu-id="aecf6-155">如果在 web 伺服陣列環境中部署應用程式，這個值應該是假設同樣的 web 伺服陣列中的所有電腦上設定 IIS 環境穩定。</span><span class="sxs-lookup"><span data-stu-id="aecf6-155">If an app is deployed in a web farm environment, this value should be stable assuming that the IIS environments are configured similarly across all machines in the web farm.</span></span>

2. <span data-ttu-id="aecf6-156">如果應用程式不裝載在 IIS 中，唯一的識別項是應用程式的實體路徑。</span><span class="sxs-lookup"><span data-stu-id="aecf6-156">If the app isn't hosted in IIS, the unique identifier is the physical path of the app.</span></span>

<span data-ttu-id="aecf6-157">唯一識別項設計來重設不會被&mdash;個別應用程式和的電腦本身。</span><span class="sxs-lookup"><span data-stu-id="aecf6-157">The unique identifier is designed to survive resets &mdash; both of the individual app and of the machine itself.</span></span>

<span data-ttu-id="aecf6-158">此隔離機制假設應用程式不是惡意。</span><span class="sxs-lookup"><span data-stu-id="aecf6-158">This isolation mechanism assumes that the apps are not malicious.</span></span> <span data-ttu-id="aecf6-159">惡意的應用程式一律會影響任何其他在相同的背景工作處理序帳戶下執行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="aecf6-159">A malicious app can always impact any other app running under the same worker process account.</span></span> <span data-ttu-id="aecf6-160">在共用裝載環境中互相不受信任的應用程式所在位置，主機服務提供者應該採取步驟來確保應用程式，包括將應用程式的基礎索引鍵的儲存機制之間的作業系統層級隔離。</span><span class="sxs-lookup"><span data-stu-id="aecf6-160">In a shared hosting environment where apps are mutually untrusted, the hosting provider should take steps to ensure OS-level isolation between apps, including separating the apps' underlying key repositories.</span></span>

<span data-ttu-id="aecf6-161">如果 ASP.NET Core 主機未提供資料保護系統 (例如，如果您透過它執行個體化`DataProtectionProvider`具象類型) 預設會停用應用程式隔離。</span><span class="sxs-lookup"><span data-stu-id="aecf6-161">If the Data Protection system isn't provided by an ASP.NET Core host (for example, if you instantiate it via the `DataProtectionProvider` concrete type) app isolation is disabled by default.</span></span> <span data-ttu-id="aecf6-162">停用應用程式隔離時，由相同的金鑰處理內容的所有應用程式可以共用裝載，只要它們提供適當[用途](xref:security/data-protection/consumer-apis/purpose-strings)。</span><span class="sxs-lookup"><span data-stu-id="aecf6-162">When app isolation is disabled, all apps backed by the same keying material can share payloads as long as they provide the appropriate [purposes](xref:security/data-protection/consumer-apis/purpose-strings).</span></span> <span data-ttu-id="aecf6-163">若要提供應用程式隔離在此環境中的，呼叫[SetApplicationName](#setapplicationname)方法的設定物件，並提供每個應用程式的唯一名稱。</span><span class="sxs-lookup"><span data-stu-id="aecf6-163">To provide app isolation in this environment, call the [SetApplicationName](#setapplicationname) method on the configuration object and provide a unique name for each app.</span></span>

## <a name="changing-algorithms-with-usecryptographicalgorithms"></a><span data-ttu-id="aecf6-164">變更與 UseCryptographicAlgorithms 演算法</span><span class="sxs-lookup"><span data-stu-id="aecf6-164">Changing algorithms with UseCryptographicAlgorithms</span></span>

<span data-ttu-id="aecf6-165">資料保護堆疊可讓您變更新產生的索引鍵所使用的預設演算法。</span><span class="sxs-lookup"><span data-stu-id="aecf6-165">The Data Protection stack allows you to change the default algorithm used by newly-generated keys.</span></span> <span data-ttu-id="aecf6-166">若要這樣做最簡單的方式是呼叫[UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms)從組態回呼：</span><span class="sxs-lookup"><span data-stu-id="aecf6-166">The simplest way to do this is to call [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) from the configuration callback:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="aecf6-167">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="aecf6-167">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="aecf6-168">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="aecf6-168">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptionSettings()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

---

<span data-ttu-id="aecf6-169">預設 EncryptionAlgorithm AES 256 CBC，而預設 ValidationAlgorithm HMACSHA256。</span><span class="sxs-lookup"><span data-stu-id="aecf6-169">The default EncryptionAlgorithm is AES-256-CBC, and the default ValidationAlgorithm is HMACSHA256.</span></span> <span data-ttu-id="aecf6-170">透過系統管理員可以設定預設原則[全機器原則](xref:security/data-protection/configuration/machine-wide-policy)，但明確呼叫`UseCryptographicAlgorithms`會覆寫預設原則。</span><span class="sxs-lookup"><span data-stu-id="aecf6-170">The default policy can be set by a system administrator via a [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy), but an explicit call to `UseCryptographicAlgorithms` overrides the default policy.</span></span>

<span data-ttu-id="aecf6-171">呼叫`UseCryptographicAlgorithms`可讓您指定從預先定義的內建清單所需的演算法。</span><span class="sxs-lookup"><span data-stu-id="aecf6-171">Calling `UseCryptographicAlgorithms` allows you to specify the desired algorithm from a predefined built-in list.</span></span> <span data-ttu-id="aecf6-172">您不需要擔心演算法的實作。</span><span class="sxs-lookup"><span data-stu-id="aecf6-172">You don't need to worry about the implementation of the algorithm.</span></span> <span data-ttu-id="aecf6-173">在上述案例中，資料保護系統會嘗試使用 AES 的 CNG 實作，如果在 Windows 上執行。</span><span class="sxs-lookup"><span data-stu-id="aecf6-173">In the scenario above, the Data Protection system attempts to use the CNG implementation of AES if running on Windows.</span></span> <span data-ttu-id="aecf6-174">否則，它就會回到 managed [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes)類別。</span><span class="sxs-lookup"><span data-stu-id="aecf6-174">Otherwise, it falls back to the managed [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) class.</span></span>

<span data-ttu-id="aecf6-175">您可以手動指定的實作，透過對呼叫[UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms)。</span><span class="sxs-lookup"><span data-stu-id="aecf6-175">You can manually specify an implementation via a call to [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span></span>

> [!TIP]
> <span data-ttu-id="aecf6-176">變更演算法不會影響鑰匙圈中現有的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="aecf6-176">Changing algorithms doesn't affect existing keys in the key ring.</span></span> <span data-ttu-id="aecf6-177">它只會影響新產生的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="aecf6-177">It only affects newly-generated keys.</span></span>

### <a name="specifying-custom-managed-algorithms"></a><span data-ttu-id="aecf6-178">指定受管理的自訂演算法</span><span class="sxs-lookup"><span data-stu-id="aecf6-178">Specifying custom managed algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="aecf6-179">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="aecf6-179">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="aecf6-180">若要指定受管理的自訂演算法，建立[ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration)指向實作類型的執行個體：</span><span class="sxs-lookup"><span data-stu-id="aecf6-180">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) instance that points to the implementation types:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="aecf6-181">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="aecf6-181">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="aecf6-182">若要指定受管理的自訂演算法，建立[ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings)指向實作類型的執行個體：</span><span class="sxs-lookup"><span data-stu-id="aecf6-182">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) instance that points to the implementation types:</span></span>

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

---

<span data-ttu-id="aecf6-183">通常\*類型屬性必須指向實體，可具現化 （透過公用的無參數建構函式） 的實作[SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm)和[KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm)，但是系統特殊案例某些等值`typeof(Aes)`為了方便起見。</span><span class="sxs-lookup"><span data-stu-id="aecf6-183">Generally the \*Type properties must point to concrete, instantiable (via a public parameterless ctor) implementations of [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) and [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), though the system special-cases some values like `typeof(Aes)` for convenience.</span></span>

> [!NOTE]
> <span data-ttu-id="aecf6-184">SymmetricAlgorithm 必須 ≥ 128 位元金鑰長度和區塊大小為 ≥ 64 位元為單位，就必須支援 CBC 模式加密 PKCS #7 填補。</span><span class="sxs-lookup"><span data-stu-id="aecf6-184">The SymmetricAlgorithm must have a key length of ≥ 128 bits and a block size of ≥ 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="aecf6-185">KeyedHashAlgorithm 必須有摘要大小 > = 128 位元，而且它必須支援長度等於雜湊演算法的摘要長度的金鑰。</span><span class="sxs-lookup"><span data-stu-id="aecf6-185">The KeyedHashAlgorithm must have a digest size of >= 128 bits, and it must support keys of length equal to the hash algorithm's digest length.</span></span> <span data-ttu-id="aecf6-186">KeyedHashAlgorithm 不是絕對必要，是 HMAC。</span><span class="sxs-lookup"><span data-stu-id="aecf6-186">The KeyedHashAlgorithm isn't strictly required to be HMAC.</span></span>

### <a name="specifying-custom-windows-cng-algorithms"></a><span data-ttu-id="aecf6-187">指定自訂的 Windows CNG 演算法</span><span class="sxs-lookup"><span data-stu-id="aecf6-187">Specifying custom Windows CNG algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="aecf6-188">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="aecf6-188">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="aecf6-189">若要指定自訂的 Windows CNG 演算法使用 CBC 模式的加密 HMAC 驗證時，建立[CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration)執行個體，其中包含演算法的資訊：</span><span class="sxs-lookup"><span data-stu-id="aecf6-189">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="aecf6-190">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="aecf6-190">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="aecf6-191">若要指定自訂的 Windows CNG 演算法使用 CBC 模式的加密 HMAC 驗證時，建立[CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings)執行個體，其中包含演算法的資訊：</span><span class="sxs-lookup"><span data-stu-id="aecf6-191">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

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

---

> [!NOTE]
> <span data-ttu-id="aecf6-192">對稱的區塊加密演算法的金鑰長度必須 > = 128 位元的區塊大小 > = 64 位元，而且它必須支援 CBC 模式加密 PKCS #7 填補。</span><span class="sxs-lookup"><span data-stu-id="aecf6-192">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of >= 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="aecf6-193">雜湊演算法必須有摘要大小 > = 128 位元，且必須支援正在開啟與 BCRYPT\_ALG\_處理\_HMAC\_旗標的旗標。</span><span class="sxs-lookup"><span data-stu-id="aecf6-193">The hash algorithm must have a digest size of >= 128 bits and must support being opened with the BCRYPT\_ALG\_HANDLE\_HMAC\_FLAG flag.</span></span> <span data-ttu-id="aecf6-194">\*可以設定為 null，以使用指定之演算法的預設提供者提供者屬性。</span><span class="sxs-lookup"><span data-stu-id="aecf6-194">The \*Provider properties can be set to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="aecf6-195">請參閱[BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx)文件的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="aecf6-195">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="aecf6-196">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="aecf6-196">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="aecf6-197">若要指定自訂的 Windows CNG 演算法使用 Galois/計數器模式加密驗證時，建立[CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration)執行個體，其中包含演算法的資訊：</span><span class="sxs-lookup"><span data-stu-id="aecf6-197">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="aecf6-198">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="aecf6-198">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="aecf6-199">若要指定自訂的 Windows CNG 演算法使用 Galois/計數器模式加密驗證時，建立[CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings)執行個體，其中包含演算法的資訊：</span><span class="sxs-lookup"><span data-stu-id="aecf6-199">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

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

---

> [!NOTE]
> <span data-ttu-id="aecf6-200">對稱的區塊加密演算法的金鑰長度必須 > = 128 位元，區塊大小為剛好為 128 位元，而且它必須支援 GCM 加密。</span><span class="sxs-lookup"><span data-stu-id="aecf6-200">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of exactly 128 bits, and it must support GCM encryption.</span></span> <span data-ttu-id="aecf6-201">您可以設定[EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider)屬性設為 null 的預設提供者用於指定的演算法。</span><span class="sxs-lookup"><span data-stu-id="aecf6-201">You can set the [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) property to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="aecf6-202">請參閱[BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx)文件的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="aecf6-202">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

### <a name="specifying-other-custom-algorithms"></a><span data-ttu-id="aecf6-203">指定其他的自訂演算法</span><span class="sxs-lookup"><span data-stu-id="aecf6-203">Specifying other custom algorithms</span></span>

<span data-ttu-id="aecf6-204">雖然不公開為第一級的應用程式開發介面，資料保護系統是演算法的可足夠的延伸，可讓您指定幾乎任何類型。</span><span class="sxs-lookup"><span data-stu-id="aecf6-204">Though not exposed as a first-class API, the Data Protection system is extensible enough to allow specifying almost any kind of algorithm.</span></span> <span data-ttu-id="aecf6-205">例如，它是核心的可以保留包含在硬體安全性模組 (HSM) 的所有索引鍵，並提供自訂實作加密和解密常式。</span><span class="sxs-lookup"><span data-stu-id="aecf6-205">For example, it's possible to keep all keys contained within a Hardware Security Module (HSM) and to provide a custom implementation of the core encryption and decryption routines.</span></span> <span data-ttu-id="aecf6-206">請參閱[IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor)中[核心加密擴充性](xref:security/data-protection/extensibility/core-crypto)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="aecf6-206">See [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) in [Core cryptography extensibility](xref:security/data-protection/extensibility/core-crypto) for more information.</span></span>

## <a name="persisting-keys-when-hosting-in-a-docker-container"></a><span data-ttu-id="aecf6-207">裝載在 Docker 容器中時，保存索引鍵</span><span class="sxs-lookup"><span data-stu-id="aecf6-207">Persisting keys when hosting in a Docker container</span></span>

<span data-ttu-id="aecf6-208">在裝載時[Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/)容器中，索引鍵應該維護在：</span><span class="sxs-lookup"><span data-stu-id="aecf6-208">When hosting in a [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) container, keys should be maintained in either:</span></span>

* <span data-ttu-id="aecf6-209">是持續發生，例如共用磁碟區或主機掛接的磁碟區的容器的存留期的 Docker 磁碟區的資料夾。</span><span class="sxs-lookup"><span data-stu-id="aecf6-209">A folder that's a Docker volume that persists beyond the container's lifetime, such as a shared volume or a host-mounted volume.</span></span>
* <span data-ttu-id="aecf6-210">外部提供者，例如[Azure 金鑰保存庫](https://azure.microsoft.com/services/key-vault/)或[Redis](https://redis.io/)。</span><span class="sxs-lookup"><span data-stu-id="aecf6-210">An external provider, such as [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) or [Redis](https://redis.io/).</span></span>

## <a name="see-also"></a><span data-ttu-id="aecf6-211">另請參閱</span><span class="sxs-lookup"><span data-stu-id="aecf6-211">See also</span></span>

* [<span data-ttu-id="aecf6-212">非 DI 感知案例</span><span class="sxs-lookup"><span data-stu-id="aecf6-212">Non DI Aware Scenarios</span></span>](xref:security/data-protection/configuration/non-di-scenarios)
* [<span data-ttu-id="aecf6-213">電腦全域原則</span><span class="sxs-lookup"><span data-stu-id="aecf6-213">Machine Wide Policy</span></span>](xref:security/data-protection/configuration/machine-wide-policy)
