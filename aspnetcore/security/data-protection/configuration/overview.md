---
title: 設定 ASP.NET Core 資料保護
author: rick-anderson
description: 瞭解如何在 ASP.NET Core 中設定資料保護。
ms.author: riande
ms.custom: mvc
ms.date: 10/07/2019
uid: security/data-protection/configuration/overview
ms.openlocfilehash: 380293f650c9548c286f98c0447c7ed08b918f2a
ms.sourcegitcommit: 3d082bd46e9e00a3297ea0314582b1ed2abfa830
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/07/2019
ms.locfileid: "72007373"
---
# <a name="configure-aspnet-core-data-protection"></a>設定 ASP.NET Core 資料保護

當資料保護系統初始化時，它會根據操作環境來套用[預設設定](xref:security/data-protection/configuration/default-settings)。 這些設定通常適用于在單一電腦上執行的應用程式。 在某些情況下，開發人員可能會想要變更預設設定：

* 應用程式會散佈在多部電腦上。
* 基於合規性因素。

在這些情況下，資料保護系統會提供豐富的設定 API。

> [!WARNING]
> 與設定檔類似，應使用適當的許可權來保護資料保護金鑰環。 您可以選擇加密待用金鑰，但這並不會阻止攻擊者建立新的金鑰。 因此，您的應用程式安全性會受到影響。 使用資料保護設定的儲存位置應該將其存取限制為應用程式本身，與保護設定檔的方式類似。 例如，如果您選擇將金鑰環形儲存在磁片上，請使用檔案系統許可權。 請確定您的 web 應用程式執行所用的身分識別，具有該目錄的讀取、寫入和建立存取權。 如果您使用 Azure Blob 儲存體，只有 web 應用程式應該能夠在 Blob 存放區中讀取、寫入或建立新的專案等等。
>
> 擴充方法[AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection)會傳回[IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder)。 `IDataProtectionBuilder` 會公開可連結在一起以設定資料保護選項的擴充方法。

::: moniker range=">= aspnetcore-2.1"

## <a name="protectkeyswithazurekeyvault"></a>ProtectKeysWithAzureKeyVault

若要將金鑰儲存在[Azure Key Vault](https://azure.microsoft.com/services/key-vault/)中，請在 `Startup` 類別中設定具有[ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault)的系統：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

設定金鑰環形儲存位置（例如， [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)）。 必須設定位置，因為呼叫 `ProtectKeysWithAzureKeyVault` 會執行停用自動資料保護設定的[IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor) ，包括金鑰環形儲存位置。 上述範例會使用 Azure Blob 儲存體來保存金鑰環。 如需詳細資訊，請參閱 @no__t 0Key 存放裝置提供者：Azure 儲存體](xref:security/data-protection/implementation/key-storage-providers#azure-storage)。 您也可以使用[PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system)在本機保存金鑰信號。

@No__t-0 是用於金鑰加密的金鑰保存庫金鑰識別碼。 例如，在 `contosokeyvault` 中名為 `dataprotection` 的金鑰保存庫中建立的金鑰具有 `https://contosokeyvault.vault.azure.net/keys/dataprotection/` 的金鑰識別碼。 提供具有解除包裝**金鑰的**應用程式和金鑰保存庫的**金鑰**許可權。

`ProtectKeysWithAzureKeyVault` 多載：

* [ProtectKeysWithAzureKeyVault （IDataProtectionBuilder，KeyVaultClient，String）](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_)允許使用[KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) ，讓資料保護系統能夠使用金鑰保存庫。
* [ProtectKeysWithAzureKeyVault （IDataProtectionBuilder，string，string，X509Certificate2）](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_)允許使用 `ClientId` 和[X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) ，讓資料保護系統能夠使用金鑰保存庫。
* [ProtectKeysWithAzureKeyVault （IDataProtectionBuilder，string，string，string）](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_)允許使用 `ClientId` 和 `ClientSecret`，讓資料保護系統能夠使用金鑰保存庫。

::: moniker-end

## <a name="persistkeystofilesystem"></a>PersistKeysToFileSystem

若要將金鑰儲存在 UNC 共用上，而不是在 *% LOCALAPPDATA%* 的預設位置，請使用[PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem)設定系統：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

> [!WARNING]
> 如果您變更金鑰持續性位置，系統就不會再自動加密待用金鑰，因為它並不知道 DPAPI 是否為適當的加密機制。

## <a name="protectkeyswith"></a>ProtectKeysWith\*

您可以藉由呼叫任何[ProtectKeysWith @ no__t-1](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions)設定 api，設定系統來保護待用金鑰。 請考慮下列範例，其會將金鑰儲存在 UNC 共用上，並使用特定的 x.509 憑證來加密待用金鑰：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

::: moniker range=">= aspnetcore-2.1"

在 ASP.NET Core 2.1 或更新版本中，您可以提供[ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate)的[X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) ，例如從檔案載入的憑證：

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

如需有關內建金鑰加密機制的其他範例和討論，請參閱待用[金鑰加密](xref:security/data-protection/implementation/key-encryption-at-rest)。

::: moniker range=">= aspnetcore-2.1"

## <a name="unprotectkeyswithanycertificate"></a>UnprotectKeysWithAnyCertificate

在 ASP.NET Core 2.1 或更新版本中，您可以使用具有[UnprotectKeysWithAnyCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.unprotectkeyswithanycertificate)的[X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2)憑證陣列，旋轉憑證並將待用金鑰解密：

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

若要將系統設定為使用14天的金鑰存留期，而不是預設的90天，請使用[SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime)：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

## <a name="setapplicationname"></a>SetApplicationName

根據預設，資料保護系統會依據其[內容根](xref:fundamentals/index#content-root)路徑，將應用程式彼此隔離，即使它們共用相同的實體金鑰存放庫也是一樣。 這可防止應用程式瞭解彼此受保護的裝載。

若要在應用程式之間共用受保護的承載：

* 使用相同的值，在每個應用程式中設定 <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*>。
* 跨應用程式使用相同版本的資料保護 API 堆疊。 在應用程式的專案檔案中執行下列**其中一**項：
  * 透過[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)參考相同的共用架構版本。
  * 參考相同的[資料保護套件](xref:security/data-protection/introduction#package-layout)版本。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("shared app name");
}
```

## <a name="disableautomatickeygeneration"></a>DisableAutomaticKeyGeneration

您可能會想要讓應用程式在接近到期的情況下自動變換金鑰（建立新的金鑰）。 其中一個範例可能是在主要/次要關係中設定的應用程式，其中只有主要應用程式會負責金鑰管理考慮，而次要應用程式只會有金鑰環的唯讀視圖。 次要應用程式可以設定為使用 <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.DisableAutomaticKeyGeneration*> 來將金鑰環視為唯讀：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

## <a name="per-application-isolation"></a>每個應用程式隔離

當資料保護系統是由 ASP.NET Core 主機提供時，它會自動將應用程式彼此隔離，即使這些應用程式是在相同的工作者進程帳戶下執行，而且使用相同的主要金鑰內容。 這有點類似于 System.web 的 `<machineKey>` 元素中的 IsolateApps 修飾詞。

隔離機制的運作方式是將本機電腦上的每個應用程式視為唯一的租使用者，因此任何指定應用程式的根 @no__t 0 會自動將應用程式識別碼包含為鑒別子。 應用程式的唯一識別碼是應用程式的實體路徑：

* 對於裝載于 IIS 中的應用程式，唯一識別碼是應用程式的 IIS 實體路徑。 如果應用程式部署在 web 伺服陣列環境中，此值會穩定，假設 IIS 環境在 web 伺服陣列中的所有電腦上都有類似的設定。
* 對於在[Kestrel 伺服器](xref:fundamentals/servers/index#kestrel)上執行的自我裝載應用程式，唯一識別碼是磁片上應用程式的實體路徑。

唯一識別碼的設計是要繼續重設個別應用程式和電腦本身的 @ no__t-0both。

這種隔離機制會假設應用程式不是惡意的。 惡意應用程式一律會影響在同一個工作者進程帳戶下執行的任何其他應用程式。 在應用程式互相不受信任的共用裝載環境中，主機服務提供者應採取步驟，以確保應用程式之間的作業系統層級隔離，包括分隔應用程式的基礎金鑰存放庫。

如果資料保護系統不是由 ASP.NET Core 主機提供（例如，如果您透過 `DataProtectionProvider` 具象類型來具現化），則預設會停用應用程式隔離。 當應用程式隔離停用時，所有由相同的金鑰內容所支援的應用程式都可以共用承載，只要它們提供適當的[用途](xref:security/data-protection/consumer-apis/purpose-strings)即可。 若要在此環境中提供應用程式隔離，請在設定物件上呼叫[SetApplicationName](#setapplicationname)方法，並為每個應用程式提供唯一的名稱。

## <a name="changing-algorithms-with-usecryptographicalgorithms"></a>使用 UseCryptographicAlgorithms 變更演算法

資料保護堆疊可讓您變更新產生的索引鍵所使用的預設演算法。 若要這麼做，最簡單的方法是從設定回呼呼叫[UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) ：

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

預設 EncryptionAlgorithm 為 AES-256-CBC，而預設 ValidationAlgorithm 為 HMACSHA256。 系統管理員可以透過[整部電腦的原則](xref:security/data-protection/configuration/machine-wide-policy)來設定預設原則，但 `UseCryptographicAlgorithms` 的明確呼叫會覆寫預設原則。

呼叫 `UseCryptographicAlgorithms` 可讓您從預先定義的內建清單中指定所需的演算法。 您不需要擔心演算法的執行。 在上述案例中，如果在 Windows 上執行，資料保護系統會嘗試使用 AES 的 CNG 實行。 否則，它會回復[至受管理](/dotnet/api/system.security.cryptography.aes)的 system.string 類別。

您可以透過呼叫[UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms)來手動指定執行。

> [!TIP]
> 變更演算法不會影響金鑰環中的現有索引鍵。 它只會影響新產生的金鑰。

### <a name="specifying-custom-managed-algorithms"></a>指定自訂受控演算法

::: moniker range=">= aspnetcore-2.0"

若要指定自訂的 managed 演算法，請建立指向執行類型的[ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration)實例：

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

若要指定自訂的 managed 演算法，請建立指向執行類型的[ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings)實例：

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

一般來說，@no__t 0Type 的屬性必須指向[system.security.cryptography.symmetricalgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm)和[KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm)的具象、可具現化（透過公用無參數 ctor）的實值，不過，系統特殊案例的某些值（例如 `typeof(Aes)`）便於.

> [!NOTE]
> System.security.cryptography.symmetricalgorithm 必須具有≥128位的金鑰長度和≥64位的區塊大小，而且它必須支援具有 PKCS #7 填補的 CBC 模式加密。 KeyedHashAlgorithm 必須具有 > = 128 位的摘要大小，而且它必須支援長度等於雜湊演算法摘要長度的索引鍵。 KeyedHashAlgorithm 不一定要是 HMAC。

### <a name="specifying-custom-windows-cng-algorithms"></a>指定自訂的 Windows CNG 演算法

::: moniker range=">= aspnetcore-2.0"

若要使用 CBC 模式加密搭配 HMAC 驗證來指定自訂 Windows CNG 演算法，請建立包含演算法資訊的[CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration)實例：

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

若要使用 CBC 模式加密搭配 HMAC 驗證來指定自訂 Windows CNG 演算法，請建立包含演算法資訊的[CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings)實例：

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
> 對稱式區塊加密演算法的金鑰長度必須為 > = 128 位、區塊大小 > = 64 位，而且必須支援具有 PKCS #7 填補的 CBC 模式加密。 雜湊演算法的摘要大小必須為 > = 128 位，而且必須支援以 BCRYPT @ no__t-0ALG @ no__t-1HANDLE @ no__t-2HMAC @ no__t-3FLAG 旗標開啟。 @No__t 0Provider 的屬性可以設定為 null，以針對指定的演算法使用預設提供者。 如需詳細資訊，請參閱[BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx)檔。

::: moniker range=">= aspnetcore-2.0"

若要使用 Galois/計數器模式加密搭配驗證來指定自訂的 Windows CNG 演算法，請建立包含演算法資訊的[CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration)實例：

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

若要使用 Galois/計數器模式加密搭配驗證來指定自訂的 Windows CNG 演算法，請建立包含演算法資訊的[CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings)實例：

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
> 對稱式區塊加密演算法的金鑰長度必須為 > = 128 位、剛好為128位的區塊大小，而且必須支援 GCM 加密。 您可以將[EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider)屬性設定為 null，以針對指定的演算法使用預設提供者。 如需詳細資訊，請參閱[BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx)檔。

### <a name="specifying-other-custom-algorithms"></a>指定其他自訂演算法

雖然不會公開為第一類的 API，但資料保護系統的擴充性足以讓您指定幾乎任何類型的演算法。 例如，您可以保留硬體安全模組（HSM）中包含的所有金鑰，以及提供核心加密和解密常式的自訂執行。 如需詳細資訊，請參閱[核心密碼](xref:security/data-protection/extensibility/core-crypto)編譯擴充性中的[IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) 。

## <a name="persisting-keys-when-hosting-in-a-docker-container"></a>在 Docker 容器中裝載時保存金鑰

在[Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/)容器中裝載時，應該在下列其中一項維護金鑰：

* 一種資料夾，其為保存在容器存留期之外的 Docker 磁片區，例如共用磁片區或裝載掛接的磁片區。
* 外部提供者，例如[Azure Key Vault](https://azure.microsoft.com/services/key-vault/)或[Redis](https://redis.io/)。

## <a name="persisting-keys-with-redis"></a>使用 Redis 保存金鑰

只有支援[Redis 資料持續](/azure/azure-cache-for-redis/cache-how-to-premium-persistence)性的 Redis 版本才應該用來儲存索引鍵。 [Azure Blob 儲存體](/azure/storage/blobs/storage-blobs-introduction)是持續性的，可以用來儲存金鑰。 如需詳細資訊，請參閱 <<c0> [ 此 GitHub 問題](https://github.com/aspnet/AspNetCore/issues/13476)。

## <a name="additional-resources"></a>其他資源

* <xref:security/data-protection/configuration/non-di-scenarios>
* <xref:security/data-protection/configuration/machine-wide-policy>
* <xref:host-and-deploy/web-farm>
* <xref:security/data-protection/implementation/key-storage-providers>
