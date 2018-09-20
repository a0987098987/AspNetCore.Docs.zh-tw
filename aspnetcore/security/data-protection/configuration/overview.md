---
title: 設定 ASP.NET Core 資料保護
author: rick-anderson
description: 了解如何設定 ASP.NET Core 中的資料保護。
ms.author: riande
ms.custom: mvc
ms.date: 07/17/2017
uid: security/data-protection/configuration/overview
ms.openlocfilehash: 0377fe9fbe5a1eeddb384443370751baa3c0ee43
ms.sourcegitcommit: 8bf4dff3069e62972c1b0839a93fb444e502afe7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/20/2018
ms.locfileid: "46482992"
---
# <a name="configure-aspnet-core-data-protection"></a>設定 ASP.NET Core 資料保護

初始化資料保護系統時，它會套用[預設設定](xref:security/data-protection/configuration/default-settings)根據作業環境。 這些設定是通常適用於在單一機器上執行的應用程式。 有開發人員可能要變更預設設定的情況：

* 應用程式則會分散到多部電腦。
* 為了符合法規。

針對這些案例中，資料保護系統會提供豐富的組態 API。

> [!WARNING]
> 類似於組態檔，資料保護 keyring 應受到保護使用適當的權限。 您可以選擇加密待用的金鑰，但這不會防止攻擊者建立新的金鑰。 因此，您的應用程式的安全性會受到影響。 設定資料保護的儲存體位置應該有其存取僅限於應用程式本身，類似於您想保護組態檔的方式。 例如，如果您選擇您 keyring 儲存在磁碟上，使用檔案系統權限。 請確定只有在識別您的 web 應用程式執行具有讀取、 寫入和建立該目錄的存取權。 如果您使用 Azure 資料表儲存體時，web 應用程式應該能夠讀取、 寫入或建立新的項目中的資料表存放區等等。
>
> 擴充方法[AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection)會傳回[IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder)。 `IDataProtectionBuilder` 會公開，您可以鏈結在一起若要設定資料保護選項的擴充方法。

::: moniker range=">= aspnetcore-2.1"

## <a name="protectkeyswithazurekeyvault"></a>ProtectKeysWithAzureKeyVault

金鑰存放在[Azure Key Vault](https://azure.microsoft.com/services/key-vault/)，設定與系統[ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault)在`Startup`類別：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

設定金鑰環儲存體位置 (例如[PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage))。 必須設定的位置，因為呼叫`ProtectKeysWithAzureKeyVault`會實作[IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor)停用自動的資料保護設定，包括 keyring 存放區位置。 上述範例會使用 Azure Blob 儲存體，以保存 keyring。 如需詳細資訊，請參閱 <<c0> [ 金鑰儲存提供者： Azure 與 Redis](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis)。 您也可以保存在本機使用 keyring [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system)。

`keyIdentifier`是用於金鑰加密的金鑰保存庫金鑰識別碼 (例如`https://contosokeyvault.vault.azure.net/keys/dataprotection/`)。

`ProtectKeysWithAzureKeyVault` 多載：

* [ProtectKeysWithAzureKeyVault (IDataProtectionBuilder，KeyVaultClient，String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_)允許使用[KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)啟用資料保護系統，才能使用金鑰保存庫。
* [（IDataProtectionBuilder、 字串、 字串、 X509Certificate2） ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_)允許使用`ClientId`並[X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2)啟用資料保護系統，才能使用金鑰保存庫。
* [（IDataProtectionBuilder、 字串、 字串、 字串） 的 ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_)允許使用`ClientId`和`ClientSecret`啟用資料保護系統，才能使用金鑰保存庫。

::: moniker-end

## <a name="persistkeystofilesystem"></a>PersistKeysToFileSystem

若要將金鑰存放在 UNC 共用而不是在 *%LOCALAPPDATA%* 的預設位置，設定與系統[PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

> [!WARNING]
> 如果您變更金鑰的持續性位置時，系統將不會再自動加密待用的金鑰因為它並不知道是否 DPAPI 是適當的加密機制。

## <a name="protectkeyswith"></a>ProtectKeysWith\*

您可以設定來保護待用的金鑰呼叫任何的系統[ProtectKeysWith\* ](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions)組態 Api。 請考慮下列範例中，這在 UNC 共用上儲存金鑰，並加密待用與特定 X.509 憑證的這些金鑰：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

::: moniker range=">= aspnetcore-2.1"

在 ASP.NET Core 2.1 或更新版本，您可以提供[X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2)要[ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate)，例如從檔案載入憑證：

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

請參閱[加密的金鑰在 Rest](xref:security/data-protection/implementation/key-encryption-at-rest)的更多範例和討論內建的金鑰加密機制。

::: moniker range=">= aspnetcore-2.1"

## <a name="unprotectkeyswithanycertificate"></a>UnprotectKeysWithAnyCertificate

在 ASP.NET Core 2.1 或更新版本，您可以旋轉憑證和解密金鑰在待用期間使用的陣列[X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2)與憑證[UnprotectKeysWithAnyCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.unprotectkeyswithanycertificate):

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

## <a name="setdefaultkeylifetime"></a>SetDefaultKeyLifetime

若要設定系統使用的金鑰的存留期為 14 天，而非預設的 90 天，請使用[SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

## <a name="setapplicationname"></a>SetApplicationName

根據預設，資料保護系統隔離應用程式不會彼此，即使它們共用相同的實體索引鍵存放庫。 這可防止應用程式了解彼此的受保護的承載。 若要共用受保護的承載，兩個應用程式之間，使用[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname)與每個應用程式相同的值：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("shared app name");
}
```

## <a name="disableautomatickeygeneration"></a>DisableAutomaticKeyGeneration

您可能會有您不想要自動彙總 （建立新的金鑰） 的索引鍵，因為他們接近到期的應用程式的案例。 這其中一個範例可能是應用程式設定中的主要/次要關聯性，其中主要應用程式必須負責針對金鑰管理問題，而次要應用程式只需要金鑰環的唯讀檢視。 次要的應用程式可以設定將 keyring 視為唯讀，藉由設定系統[DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

## <a name="per-application-isolation"></a>每個應用程式隔離

ASP.NET Core 主機所提供資料保護系統時，它會自動隔離應用程式不會彼此，即使這些應用程式相同的背景工作處理序帳戶下執行，並使用相同的主要金鑰材料。 這是有點類似 IsolateApps 修飾詞將來自 System.Web 的 **\<machineKey >** 項目。

隔離機制的運作方式是為唯一的租用戶，考慮在本機電腦上的每個應用程式，因此[IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) root 破解的任何指定的應用程式會自動包含為鑑別子的應用程式識別碼。 應用程式的唯一識別碼來自兩個地方的其中一個：

1. 如果應用程式裝載在 IIS 中，唯一的識別碼會是應用程式的組態路徑。 如果應用程式部署在 web 伺服陣列環境中，這個值應該是穩定，假設 IIS 環境 web 伺服陣列中的所有電腦上設定方式均類似。

2. 如果未在 IIS 中裝載應用程式，唯一的識別碼會是應用程式的實體路徑。

唯一識別項設計來重設之後存留下來&mdash;個別的應用程式和的電腦本身。

此隔離機制假設應用程式不是惡意的。 惡意應用程式一律會影響任何其他在相同的背景工作處理序帳戶下執行的應用程式。 在共用主控環境中彼此不受信任的應用程式所在，主機服務提供者應該採取步驟，以確保應用程式，包括將應用程式的基礎金鑰存放庫之間的 OS 層級隔離。

ASP.NET Core 主應用程式不提供資料保護系統 (例如，如果您將透過其執行個體化`DataProtectionProvider`具象類型) 預設為停用應用程式隔離。 只要提供適當停用應用程式隔離時，受到相同的金鑰材料的所有應用程式可以共用承載[用途](xref:security/data-protection/consumer-apis/purpose-strings)。 若要提供應用程式隔離，在此環境中的，呼叫[SetApplicationName](#setapplicationname)方法設定物件，並提供每個應用程式的唯一名稱。

## <a name="changing-algorithms-with-usecryptographicalgorithms"></a>變更具有 UseCryptographicAlgorithms 演算法

資料保護堆疊可讓您變更使用新產生的索引鍵的預設演算法。 若要這樣做最簡單的方式是呼叫[UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms)設定回呼：

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

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

EncryptionAlgorithm 的預設值是 AES-256-CBC，預設 ValidationAlgorithm HMACSHA256。 透過系統管理員可以設定預設原則[全電腦原則](xref:security/data-protection/configuration/machine-wide-policy)，但明確呼叫`UseCryptographicAlgorithms`會覆寫預設原則。

呼叫`UseCryptographicAlgorithms`可讓您指定的所需的演算法，從預先定義的內建清單。 您不需要擔心演算法的實作。 在上述案例中，資料保護系統會嘗試使用 AES 的 CNG 實作，如果在 Windows 上執行。 否則，它會回復到 managed [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes)類別。

您可以手動指定透過呼叫實作[UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms)。

> [!TIP]
> 變更演算法不會影響金鑰環中的現有金鑰。 它只會影響新產生的索引鍵。

### <a name="specifying-custom-managed-algorithms"></a>指定受管理的自訂演算法

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

若要指定受管理的自訂演算法，建立[ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration)指向實作類型的執行個體：

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

若要指定受管理的自訂演算法，建立[ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings)指向實作類型的執行個體：

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

通常\*類型屬性必須指向實體，可具現化 （透過公用的無參數建構函式） 實作[SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm)並[KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm)，不過系統特殊案例某些等值`typeof(Aes)`為了方便起見。

> [!NOTE]
> SymmetricAlgorithm 必須 ≥ 128 位元的金鑰長度和 ≥ 64 位元為單位的區塊大小，而且它必須支援使用 PKCS #7 填補的 CBC 模式加密。 KeyedHashAlgorithm 必須有摘要大小 > = 128 位元，而且它必須支援的長度等於雜湊演算法的摘要長度索引鍵。 KeyedHashAlgorithm 不是 HMAC 是絕對必要。

### <a name="specifying-custom-windows-cng-algorithms"></a>指定自訂的 Windows CNG 演算法

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

若要指定 HMAC 驗證搭配使用 CBC 模式加密的自訂 Windows CNG 演算法，建立[CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration)執行個體，包含演算法的資訊：

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

若要指定 HMAC 驗證搭配使用 CBC 模式加密的自訂 Windows CNG 演算法，建立[CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings)執行個體，包含演算法的資訊：

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
> 對稱區塊加密演算法金鑰長度必須 > = 128 位元為單位的區塊大小 > = 64 位元，而且它必須支援使用 PKCS #7 填補的 CBC 模式加密。 雜湊演算法必須有摘要大小 > = 128 位元，且必須支援正在開啟與 BCRYPT\_ALG\_處理\_HMAC\_旗標的旗標。 \*可以設定為 null，以指定的演算法所用的預設提供者提供者屬性。 請參閱[BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx)文件的詳細資訊。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

若要指定使用驗證 Galois/計數器模式加密的自訂 Windows CNG 演算法，建立[CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration)執行個體，包含演算法的資訊：

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

若要指定使用驗證 Galois/計數器模式加密的自訂 Windows CNG 演算法，建立[CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings)執行個體，包含演算法的資訊：

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
> 對稱區塊加密演算法金鑰長度必須 > = 128 位元，區塊大小為剛好為 128 位元，而且它必須支援 GCM 加密。 您可以設定[EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider)屬性設為 null 指定演算法所用的預設提供者。 請參閱[BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx)文件的詳細資訊。

### <a name="specifying-other-custom-algorithms"></a>指定其他的自訂演算法

雖然不公開做為第一級的 API，資料保護系統就會是可延伸，足以讓您指定幾乎任何一種演算法。 比方說，就可以將包含在硬體安全性模組 (HSM) 的所有索引鍵，並提供核心的自訂實作加密和解密的常式。 請參閱[IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor)中[核心加密擴充性](xref:security/data-protection/extensibility/core-crypto)如需詳細資訊。

## <a name="persisting-keys-when-hosting-in-a-docker-container"></a>裝載的 Docker 容器中時，保存索引鍵

裝載於[Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/)容器中，索引鍵應保存在其中一個：

* 是保存超過容器的存留期，例如共用的磁碟區或主應用程式掛接的磁碟區的 Docker 磁碟區的資料夾。
* 外部提供者，例如[Azure Key Vault](https://azure.microsoft.com/services/key-vault/)或是[Redis](https://redis.io/)。

## <a name="see-also"></a>另請參閱

* <xref:security/data-protection/configuration/non-di-scenarios>
* <xref:security/data-protection/configuration/machine-wide-policy>
* <xref:host-and-deploy/web-farm>
