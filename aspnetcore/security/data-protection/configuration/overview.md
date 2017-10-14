---
title: "設定資料保護"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 0e4881a3-a94d-4e35-9c1c-f025d65dcff0
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/overview
ms.openlocfilehash: d35e0e806999ffd2e0f8f82e0adfc940ea2b503d
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/13/2017
---
# <a name="configuring-data-protection"></a>設定資料保護

<a name="data-protection-configuring"></a>

初始化資料保護系統時它會套用某些[預設設定](default-settings.md#data-protection-default-settings)根據作業環境。 這些設定是一般適用於在單一機器上執行的應用程式。 有某些情況的下，開發人員可能要變更這些設定 (可能是因為它們的應用程式分散在多部電腦，或基於相容性因素)，並針對這些案例的資料保護系統提供豐富的組態 API。

<a name="data-protection-configuration-callback"></a>

沒有擴充方法會傳回 IDataProtectionBuilder AddDataProtection 本身會公開，您可以鏈結在一起選項來設定各種不同的資料保護的擴充方法。 例如，若要儲存在 UNC 共用，而不是 %LOCALAPPDATA%（預設值），設定系統，如下所示：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

>[!WARNING]
> 如果您變更金鑰的持續性位置時，系統將不會再自動加密金鑰靜止因為它不知道是否 DPAPI 是適當的加密機制。

<a name="configuring-x509-certificate"></a>

您可以設定系統以保護靜止的金鑰呼叫任何 ProtectKeysWith\*組態 Api。 請考慮下列範例中，這會儲存在 UNC 共用的金鑰及加密這些金鑰在靜止與特定 X.509 憑證。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

請參閱[金鑰加密在靜止](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)更多範例，以及內建的金鑰加密機制的討論。

若要設定系統使用的預設金鑰存留期為 14 天，而不是 90 天，請考慮下列範例：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

根據預設資料保護系統隔離應用程式，即使它們共用相同的實體索引鍵儲存機制。 這可防止從了解其他人的受保護的裝載的應用程式。 若要共用受保護的裝載兩個不同的應用程式之間，設定系統傳遞兩個應用程式中的相同應用程式名稱中的下列範例：

<a name="data-protection-code-sample-application-name"></a>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("my application");
}
```

<a name="data-protection-configuring-disable-automatic-key-generation"></a>

最後，您可能需要的案例，您不想要自動將索引鍵，因為它們會很接近到期的應用程式。 其中一個範例，這可能是應用程式設定中的主要 / 次要關聯性，其中只有主要的應用程式會負責金鑰管理考量，以及次要的所有應用程式只需要鑰匙圈的唯讀檢視。 您可以設定次要應用程式將鑰匙圈視為唯讀，藉由系統設定，如下所示：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

<a name="data-protection-configuration-per-app-isolation"></a>

## <a name="per-application-isolation"></a>每個應用程式隔離

ASP.NET Core 主機所提供的資料保護系統時，它會自動將應用程式隔離，即使這些應用程式相同的背景工作處理序帳戶下執行，並使用相同的主要金鑰處理內容。 這是有點像 IsolateApps 修飾詞將來自 System.Web 的<machineKey>項目。

考慮在本機電腦上每個應用程式，做為唯一的租用戶隔離機制如何運作，因此 IDataProtector 根項目為任何指定的應用程式會自動包含應用程式識別碼為鑑別子。 應用程式的唯一識別碼來自兩個地方的其中一個。

1. 如果應用程式裝載在 IIS 中，唯一的識別項是應用程式的組態路徑。 如果伺服陣列環境中部署應用程式，這個值應該是假設同樣的伺服陣列中的所有電腦上設定 IIS 環境穩定。

2. 如果應用程式不會裝載在 IIS 中，唯一的識別項是應用程式的實體路徑。

不會被重設為個別的應用程式和電腦本身的旨在的唯一識別碼。

此隔離機制假設應用程式不是惡意。 惡意的應用程式一律會影響任何其他在相同的背景工作處理序帳戶下執行的應用程式。 在共用裝載環境中互相不受信任的應用程式所在位置，主機服務提供者應該採取步驟，以確保作業系統層級隔離的應用程式，包括將應用程式的基礎索引鍵的儲存機制。

如果資料保護系統不會提供 ASP.NET Core 主機 （例如，如果開發人員會具現化它自己透過 DataProtectionProvider 具象型別）、 預設，會停用應用程式隔離和所有應用程式支援相同的金鑰處理資料可以共用裝載，只要它們提供適當的用途。 若要提供應用程式隔離在此環境中的，組態物件上呼叫 SetApplicationName 方法，請參閱[程式碼範例](#data-protection-code-sample-application-name)上方。

<a name="data-protection-changing-algorithms"></a>

## <a name="changing-algorithms"></a>變更演算法

資料保護堆疊允許變更新產生的索引鍵所使用的預設演算法。 若要這樣做最簡單的方式是設定回呼，呼叫 UseCryptographicAlgorithms，在下列範例。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(new AuthenticatedEncryptionSettings()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

---

預設的 EncryptionAlgorithm 和 ValidationAlgorithm 分別為 256-AES-CBC 和 HMACSHA256。 透過系統管理員可以設定預設原則[機器寬原則](machine-wide-policy.md)，但 UseCryptographicAlgorithms 明確呼叫將會覆寫預設原則。

呼叫 UseCryptographicAlgorithms 將允許開發人員指定所需的演算法 （從預先定義內建清單），以及開發人員不需要擔心演算法的實作。 比方說，在上面的資料保護系統案例會嘗試使用 AES 的 CNG 實作，如果在 Windows 上執行，否則它會切換回 managed System.Security.Cryptography.Aes 類別。

如果想要透過 UseCustomCryptographicAlgorithms，呼叫中所述，開發人員可以手動指定實作下面範例。

>[!TIP]
> 變更演算法不會影響鑰匙圈中現有的索引鍵。 它只會影響新產生的索引鍵。

<a name="data-protection-changing-algorithms-custom-managed"></a>

### <a name="specifying-custom-managed-algorithms"></a>指定受管理的自訂演算法

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

若要指定受管理的自訂演算法，建立指向實作類型的 ManagedAuthenticatedEncryptorConfiguration 執行個體。

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(new ManagedAuthenticatedEncryptorConfiguration()
    {
        // a type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // specified in bits
        EncryptionAlgorithmKeySize = 256,

        // a type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

若要指定受管理的自訂演算法，建立指向實作類型的 ManagedAuthenticatedEncryptionSettings 執行個體。

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(new ManagedAuthenticatedEncryptionSettings()
    {
        // a type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // specified in bits
        EncryptionAlgorithmKeySize = 256,

        // a type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

---

通常\*類型屬性必須指向實體，可具現化 （透過公用的無參數建構函式） SymmetricAlgorithm 和 KeyedHashAlgorithm，實作雖然之系統特殊案例某些等值 typeof(Aes) 為了方便起見.

> [!NOTE]
> SymmetricAlgorithm 必須 ≥ 128 位元金鑰長度和區塊大小為 ≥ 64 位元為單位，就必須支援 CBC 模式加密 PKCS #7 填補。 KeyedHashAlgorithm 必須有摘要大小 > = 128 位元，而且它必須支援長度等於雜湊演算法的摘要長度的金鑰。 無法為 HMAC 絕對必要 KeyedHashAlgorithm。

<a name="data-protection-changing-algorithms-cng"></a>

### <a name="specifying-custom-windows-cng-algorithms"></a>指定自訂的 Windows CNG 演算法

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

若要指定自訂的 Windows CNG 演算法使用 CBC 模式加密 + HMAC 驗證，請建立 CngCbcAuthenticatedEncryptorConfiguration 執行個體，其中包含演算法的資訊。

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(new CngCbcAuthenticatedEncryptorConfiguration()
    {
        // passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // specified in bits
        EncryptionAlgorithmKeySize = 256,

        // passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

若要指定自訂的 Windows CNG 演算法使用 CBC 模式加密 + HMAC 驗證，請建立 CngCbcAuthenticatedEncryptionSettings 執行個體，其中包含演算法的資訊。

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(new CngCbcAuthenticatedEncryptionSettings()
    {
        // passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // specified in bits
        EncryptionAlgorithmKeySize = 256,

        // passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

---

> [!NOTE]
> 對稱的區塊加密演算法必須 ≥ 128 位元金鑰長度和區塊大小為 ≥ 64 位元，且它必須支援 PKCS #7 填補 CBC 模式加密。 雜湊演算法必須有摘要大小 > = 128 位元，且必須支援開啟 BCRYPT_ALG_HANDLE_HMAC_FLAG 旗標。 \*可以設定為 null，以使用指定之演算法的預設提供者提供者屬性。 請參閱[BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx)文件的詳細資訊。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

若要指定自訂的 Windows CNG 演算法使用 Galois/計數器模式加密 + 驗證，請建立 CngGcmAuthenticatedEncryptorConfiguration 執行個體，其中包含演算法的資訊。

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(new CngGcmAuthenticatedEncryptorConfiguration()
    {
        // passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

若要指定自訂的 Windows CNG 演算法使用 Galois/計數器模式加密 + 驗證，請建立 CngGcmAuthenticatedEncryptionSettings 執行個體，其中包含演算法的資訊。

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(new CngGcmAuthenticatedEncryptionSettings()
    {
        // passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

---

> [!NOTE]
> 對稱的區塊加密演算法必須 ≥ 128 位元金鑰長度和區塊大小為剛好為 128 位元，且它必須支援 GCM 加密。 EncryptionAlgorithmProvider 屬性可以設定為 null，使用指定之演算法的預設提供者。 請參閱[BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx)文件的詳細資訊。

### <a name="specifying-other-custom-algorithms"></a>指定其他的自訂演算法

雖然不公開為第一級的應用程式開發介面，資料保護系統是演算法的可擴充的特點可讓您指定幾乎任何類型。 例如，它是核心的可以保留在 HSM 中所包含的所有索引鍵，並提供自訂實作加密和解密常式。 IAuthenticatedEncryptorConfiguration 中核心加密擴充性區段，如需詳細資訊，請參閱。

### <a name="see-also"></a>請參閱

* [非 DI 感知案例](non-di-scenarios.md)
* [電腦全域原則](machine-wide-policy.md)
