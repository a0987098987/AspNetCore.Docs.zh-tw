---
title: "在 ASP.NET Core 中設定資料保護"
author: rick-anderson
description: "了解如何設定 ASP.NET Core 中的資料保護。"
ms.author: riande
manager: wpickett
ms.date: 07/17/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/overview
ms.openlocfilehash: c19b1a9cba89a02420c8792b575827d439bc262b
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="configuring-data-protection-in-aspnet-core"></a>在 ASP.NET Core 中設定資料保護

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

當初始化資料保護系統時，它會套用[預設設定](xref:security/data-protection/configuration/default-settings)根據作業環境。 這些設定是通常適用於在單一機器上執行的應用程式。 沒有開發人員可能的想来變更的預設設定，可能是因為其應用程式分散在多部電腦，或基於相容性因素。 這些案例中，資料保護系統會提供豐富的組態 API。

擴充方法[AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection)傳回[IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder)。 `IDataProtectionBuilder`會公開，您可以鏈結在一起選項來設定資料保護的擴充方法。

## <a name="persistkeystofilesystem"></a>PersistKeysToFileSystem

若要將金鑰儲存在 UNC 共用而不是在*%LOCALAPPDATA%*預設位置，設定與系統[PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

> [!WARNING]
> 如果您變更金鑰的持續性位置時，系統將不會再自動加密靜止的索引鍵因為它不知道是否 DPAPI 是適當的加密機制。

## <a name="protectkeyswith"></a>ProtectKeysWith\*

您可以設定系統以保護靜止的金鑰呼叫上述任一[ProtectKeysWith\* ](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions)組態 Api。 請考慮下列範例中，將金鑰儲存在 UNC 共用，且加密待用與特定 X.509 憑證的金鑰：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

請參閱[金鑰靜態加密](xref:security/data-protection/implementation/key-encryption-at-rest)如需詳細的範例和內建的金鑰加密機制的討論。

## <a name="setdefaultkeylifetime"></a>SetDefaultKeyLifetime

若要設定系統而非預設的 90 天使用的金鑰的存留期為 14 天，請使用[SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

## <a name="setapplicationname"></a>SetApplicationName

根據預設，資料保護系統隔離應用程式，即使它們共用相同的實體索引鍵儲存機制。 這可防止應用程式的了解其他人的受保護的內容。 若要共用受保護的裝載兩個應用程式之間，使用[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname)與每個應用程式相同的值：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("shared app name");
}
```

## <a name="disableautomatickeygeneration"></a>DisableAutomaticKeyGeneration

您可能需要的案例，您不想自動回復 （建立新的索引鍵） 的索引鍵，因為它們會很接近到期的應用程式。 其中一個範例，這可能是應用程式設定中的主要/次要關聯性，其中只有主要的應用程式是負責金鑰管理問題，而次要應用程式只需要鑰匙圈的唯讀檢視。 次要的應用程式可以設定將鑰匙圈視為唯讀，藉由設定與系統[DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

## <a name="per-application-isolation"></a>每個應用程式隔離

ASP.NET Core 主機所提供資料保護系統時，它會自動會隔離應用程式，即使這些應用程式相同的背景工作處理序帳戶下執行，並使用相同的主要金鑰處理內容。 這是有點像 IsolateApps 修飾詞將來自 System.Web 的 **\<machineKey >**項目。

隔離機制的運作方式是做為唯一的租用戶，考慮在本機電腦上的每個應用程式，因此[IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector)根項目，針對任何給定的應用程式會自動包含應用程式識別碼，表示為鑑別子。 應用程式的唯一識別碼來自兩個地方的其中一個：

1. 如果應用程式裝載在 IIS 中，唯一的識別項是應用程式的組態路徑。 如果在 web 伺服陣列環境中部署應用程式，這個值應該是假設同樣的 web 伺服陣列中的所有電腦上設定 IIS 環境穩定。

2. 如果應用程式不裝載在 IIS 中，唯一的識別項是應用程式的實體路徑。

唯一識別項設計來重設不會被&mdash;個別應用程式和的電腦本身。

此隔離機制假設應用程式不是惡意。 惡意的應用程式一律會影響任何其他在相同的背景工作處理序帳戶下執行的應用程式。 在共用裝載環境中互相不受信任的應用程式所在位置，主機服務提供者應該採取步驟來確保應用程式，包括將應用程式的基礎索引鍵的儲存機制之間的作業系統層級隔離。

如果 ASP.NET Core 主機未提供資料保護系統 (例如，如果您透過它執行個體化`DataProtectionProvider`具象類型) 預設會停用應用程式隔離。 停用應用程式隔離時，由相同的金鑰處理內容的所有應用程式可以共用裝載，只要它們提供適當[用途](xref:security/data-protection/consumer-apis/purpose-strings)。 若要提供應用程式隔離在此環境中的，呼叫[SetApplicationName](#setapplicationname)方法的設定物件，並提供每個應用程式的唯一名稱。

## <a name="changing-algorithms-with-usecryptographicalgorithms"></a>變更與 UseCryptographicAlgorithms 演算法

資料保護堆疊可讓您變更新產生的索引鍵所使用的預設演算法。 若要這樣做最簡單的方式是呼叫[UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms)從組態回呼：

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

預設 EncryptionAlgorithm AES 256 CBC，而預設 ValidationAlgorithm HMACSHA256。 透過系統管理員可以設定預設原則[全機器原則](xref:security/data-protection/configuration/machine-wide-policy)，但明確呼叫`UseCryptographicAlgorithms`會覆寫預設原則。

呼叫`UseCryptographicAlgorithms`可讓您指定從預先定義的內建清單所需的演算法。 您不需要擔心演算法的實作。 在上述案例中，資料保護系統會嘗試使用 AES 的 CNG 實作，如果在 Windows 上執行。 否則，它就會回到 managed [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes)類別。

您可以手動指定的實作，透過對呼叫[UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms)。

> [!TIP]
> 變更演算法不會影響鑰匙圈中現有的索引鍵。 它只會影響新產生的索引鍵。

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

通常\*類型屬性必須指向實體，可具現化 （透過公用的無參數建構函式） 的實作[SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm)和[KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm)，但是系統特殊案例某些等值`typeof(Aes)`為了方便起見。

> [!NOTE]
> SymmetricAlgorithm 必須 ≥ 128 位元金鑰長度和區塊大小為 ≥ 64 位元為單位，就必須支援 CBC 模式加密 PKCS #7 填補。 KeyedHashAlgorithm 必須有摘要大小 > = 128 位元，而且它必須支援長度等於雜湊演算法的摘要長度的金鑰。 無法為 HMAC 絕對必要 KeyedHashAlgorithm。

### <a name="specifying-custom-windows-cng-algorithms"></a>指定自訂的 Windows CNG 演算法

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

若要指定自訂的 Windows CNG 演算法使用 CBC 模式的加密 HMAC 驗證時，建立[CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration)執行個體，其中包含演算法的資訊：

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

若要指定自訂的 Windows CNG 演算法使用 CBC 模式的加密 HMAC 驗證時，建立[CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings)執行個體，其中包含演算法的資訊：

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
> 對稱的區塊加密演算法的金鑰長度必須 > = 128 位元的區塊大小 > = 64 位元，而且它必須支援 CBC 模式加密 PKCS #7 填補。 雜湊演算法必須有摘要大小 > = 128 位元，且必須支援正在開啟與 BCRYPT\_ALG\_處理\_HMAC\_旗標的旗標。 \*可以設定為 null，以使用指定之演算法的預設提供者提供者屬性。 請參閱[BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx)文件的詳細資訊。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

若要指定自訂的 Windows CNG 演算法使用 Galois/計數器模式加密驗證時，建立[CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration)執行個體，其中包含演算法的資訊：

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

若要指定自訂的 Windows CNG 演算法使用 Galois/計數器模式加密驗證時，建立[CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings)執行個體，其中包含演算法的資訊：

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
> 對稱的區塊加密演算法的金鑰長度必須 > = 128 位元，區塊大小為剛好為 128 位元，而且它必須支援 GCM 加密。 您可以設定[EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider)屬性設為 null 的預設提供者用於指定的演算法。 請參閱[BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx)文件的詳細資訊。

### <a name="specifying-other-custom-algorithms"></a>指定其他的自訂演算法

雖然不公開為第一級的應用程式開發介面，資料保護系統是演算法的可足夠的延伸，可讓您指定幾乎任何類型。 例如，它是核心的可以保留包含在硬體安全性模組 (HSM) 的所有索引鍵，並提供自訂實作加密和解密常式。 請參閱[IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor)中[核心加密擴充性](xref:security/data-protection/extensibility/core-crypto)如需詳細資訊。

## <a name="persisting-keys-when-hosting-in-a-docker-container"></a>裝載在 Docker 容器中時，保存索引鍵

在裝載時[Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/)容器中，索引鍵應該維護在：

* 是持續發生，例如共用磁碟區或主機掛接的磁碟區的容器的存留期的 Docker 磁碟區的資料夾。
* 外部提供者，例如[Azure 金鑰保存庫](https://azure.microsoft.com/services/key-vault/)或[Redis](https://redis.io/)。

## <a name="see-also"></a>另請參閱

* [非 DI 感知案例](xref:security/data-protection/configuration/non-di-scenarios)
* [電腦全域原則](xref:security/data-protection/configuration/machine-wide-policy)
