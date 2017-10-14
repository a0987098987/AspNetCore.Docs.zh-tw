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
# <a name="configuring-data-protection"></a><span data-ttu-id="9d58b-103">設定資料保護</span><span class="sxs-lookup"><span data-stu-id="9d58b-103">Configuring data protection</span></span>

<a name="data-protection-configuring"></a>

<span data-ttu-id="9d58b-104">初始化資料保護系統時它會套用某些[預設設定](default-settings.md#data-protection-default-settings)根據作業環境。</span><span class="sxs-lookup"><span data-stu-id="9d58b-104">When the data protection system is initialized it applies some [default settings](default-settings.md#data-protection-default-settings) based on the operational environment.</span></span> <span data-ttu-id="9d58b-105">這些設定是一般適用於在單一機器上執行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="9d58b-105">These settings are generally good for applications running on a single machine.</span></span> <span data-ttu-id="9d58b-106">有某些情況的下，開發人員可能要變更這些設定 (可能是因為它們的應用程式分散在多部電腦，或基於相容性因素)，並針對這些案例的資料保護系統提供豐富的組態 API。</span><span class="sxs-lookup"><span data-stu-id="9d58b-106">There are some cases where a developer may want to change these (perhaps because their application is spread across multiple machines or for compliance reasons), and for these scenarios the data protection system offers a rich configuration API.</span></span>

<a name="data-protection-configuration-callback"></a>

<span data-ttu-id="9d58b-107">沒有擴充方法會傳回 IDataProtectionBuilder AddDataProtection 本身會公開，您可以鏈結在一起選項來設定各種不同的資料保護的擴充方法。</span><span class="sxs-lookup"><span data-stu-id="9d58b-107">There is an extension method AddDataProtection which returns an IDataProtectionBuilder which itself exposes extension methods that you can chain together to configure various data protection options.</span></span> <span data-ttu-id="9d58b-108">例如，若要儲存在 UNC 共用，而不是 %LOCALAPPDATA%（預設值），設定系統，如下所示：</span><span class="sxs-lookup"><span data-stu-id="9d58b-108">For instance, to store keys at a UNC share instead of %LOCALAPPDATA% (the default), configure the system as follows:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

>[!WARNING]
> <span data-ttu-id="9d58b-109">如果您變更金鑰的持續性位置時，系統將不會再自動加密金鑰靜止因為它不知道是否 DPAPI 是適當的加密機制。</span><span class="sxs-lookup"><span data-stu-id="9d58b-109">If you change the key persistence location, the system will no longer automatically encrypt keys at rest since it doesn't know whether DPAPI is an appropriate encryption mechanism.</span></span>

<a name="configuring-x509-certificate"></a>

<span data-ttu-id="9d58b-110">您可以設定系統以保護靜止的金鑰呼叫任何 ProtectKeysWith\*組態 Api。</span><span class="sxs-lookup"><span data-stu-id="9d58b-110">You can configure the system to protect keys at rest by calling any of the ProtectKeysWith\* configuration APIs.</span></span> <span data-ttu-id="9d58b-111">請考慮下列範例中，這會儲存在 UNC 共用的金鑰及加密這些金鑰在靜止與特定 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="9d58b-111">Consider the example below, which stores keys at a UNC share and encrypts those keys at rest with a specific X.509 certificate.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

<span data-ttu-id="9d58b-112">請參閱[金鑰加密在靜止](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)更多範例，以及內建的金鑰加密機制的討論。</span><span class="sxs-lookup"><span data-stu-id="9d58b-112">See [key encryption at rest](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) for more examples and for discussion on the built-in key encryption mechanisms.</span></span>

<span data-ttu-id="9d58b-113">若要設定系統使用的預設金鑰存留期為 14 天，而不是 90 天，請考慮下列範例：</span><span class="sxs-lookup"><span data-stu-id="9d58b-113">To configure the system to use a default key lifetime of 14 days instead of 90 days, consider the following example:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

<span data-ttu-id="9d58b-114">根據預設資料保護系統隔離應用程式，即使它們共用相同的實體索引鍵儲存機制。</span><span class="sxs-lookup"><span data-stu-id="9d58b-114">By default the data protection system isolates applications from one another, even if they're sharing the same physical key repository.</span></span> <span data-ttu-id="9d58b-115">這可防止從了解其他人的受保護的裝載的應用程式。</span><span class="sxs-lookup"><span data-stu-id="9d58b-115">This prevents the applications from understanding each other's protected payloads.</span></span> <span data-ttu-id="9d58b-116">若要共用受保護的裝載兩個不同的應用程式之間，設定系統傳遞兩個應用程式中的相同應用程式名稱中的下列範例：</span><span class="sxs-lookup"><span data-stu-id="9d58b-116">To share protected payloads between two different applications, configure the system passing in the same application name for both applications as in the below example:</span></span>

<a name="data-protection-code-sample-application-name"></a>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("my application");
}
```

<a name="data-protection-configuring-disable-automatic-key-generation"></a>

<span data-ttu-id="9d58b-117">最後，您可能需要的案例，您不想要自動將索引鍵，因為它們會很接近到期的應用程式。</span><span class="sxs-lookup"><span data-stu-id="9d58b-117">Finally, you may have a scenario where you do not want an application to automatically roll keys as they approach expiration.</span></span> <span data-ttu-id="9d58b-118">其中一個範例，這可能是應用程式設定中的主要 / 次要關聯性，其中只有主要的應用程式會負責金鑰管理考量，以及次要的所有應用程式只需要鑰匙圈的唯讀檢視。</span><span class="sxs-lookup"><span data-stu-id="9d58b-118">One example of this might be applications set up in a primary / secondary relationship, where only the primary application is responsible for key management concerns, and all secondary applications simply have a read-only view of the key ring.</span></span> <span data-ttu-id="9d58b-119">您可以設定次要應用程式將鑰匙圈視為唯讀，藉由系統設定，如下所示：</span><span class="sxs-lookup"><span data-stu-id="9d58b-119">The secondary applications can be configured to treat the key ring as read-only by configuring the system as below:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

<a name="data-protection-configuration-per-app-isolation"></a>

## <a name="per-application-isolation"></a><span data-ttu-id="9d58b-120">每個應用程式隔離</span><span class="sxs-lookup"><span data-stu-id="9d58b-120">Per-application isolation</span></span>

<span data-ttu-id="9d58b-121">ASP.NET Core 主機所提供的資料保護系統時，它會自動將應用程式隔離，即使這些應用程式相同的背景工作處理序帳戶下執行，並使用相同的主要金鑰處理內容。</span><span class="sxs-lookup"><span data-stu-id="9d58b-121">When the data protection system is provided by an ASP.NET Core host, it will automatically isolate applications from one another, even if those applications are running under the same worker process account and are using the same master keying material.</span></span> <span data-ttu-id="9d58b-122">這是有點像 IsolateApps 修飾詞將來自 System.Web 的<machineKey>項目。</span><span class="sxs-lookup"><span data-stu-id="9d58b-122">This is somewhat similar to the IsolateApps modifier from System.Web's <machineKey> element.</span></span>

<span data-ttu-id="9d58b-123">考慮在本機電腦上每個應用程式，做為唯一的租用戶隔離機制如何運作，因此 IDataProtector 根項目為任何指定的應用程式會自動包含應用程式識別碼為鑑別子。</span><span class="sxs-lookup"><span data-stu-id="9d58b-123">The isolation mechanism works by considering each application on the local machine as a unique tenant, thus the IDataProtector rooted for any given application automatically includes the application ID as a discriminator.</span></span> <span data-ttu-id="9d58b-124">應用程式的唯一識別碼來自兩個地方的其中一個。</span><span class="sxs-lookup"><span data-stu-id="9d58b-124">The application's unique ID comes from one of two places.</span></span>

1. <span data-ttu-id="9d58b-125">如果應用程式裝載在 IIS 中，唯一的識別項是應用程式的組態路徑。</span><span class="sxs-lookup"><span data-stu-id="9d58b-125">If the application is hosted in IIS, the unique identifier is the application's configuration path.</span></span> <span data-ttu-id="9d58b-126">如果伺服陣列環境中部署應用程式，這個值應該是假設同樣的伺服陣列中的所有電腦上設定 IIS 環境穩定。</span><span class="sxs-lookup"><span data-stu-id="9d58b-126">If an application is deployed in a farm environment, this value should be stable assuming that the IIS environments are configured similarly across all machines in the farm.</span></span>

2. <span data-ttu-id="9d58b-127">如果應用程式不會裝載在 IIS 中，唯一的識別項是應用程式的實體路徑。</span><span class="sxs-lookup"><span data-stu-id="9d58b-127">If the application is not hosted in IIS, the unique identifier is the physical path of the application.</span></span>

<span data-ttu-id="9d58b-128">不會被重設為個別的應用程式和電腦本身的旨在的唯一識別碼。</span><span class="sxs-lookup"><span data-stu-id="9d58b-128">The unique identifier is designed to survive resets - both of the individual application and of the machine itself.</span></span>

<span data-ttu-id="9d58b-129">此隔離機制假設應用程式不是惡意。</span><span class="sxs-lookup"><span data-stu-id="9d58b-129">This isolation mechanism assumes that the applications are not malicious.</span></span> <span data-ttu-id="9d58b-130">惡意的應用程式一律會影響任何其他在相同的背景工作處理序帳戶下執行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="9d58b-130">A malicious application can always impact any other application running under the same worker process account.</span></span> <span data-ttu-id="9d58b-131">在共用裝載環境中互相不受信任的應用程式所在位置，主機服務提供者應該採取步驟，以確保作業系統層級隔離的應用程式，包括將應用程式的基礎索引鍵的儲存機制。</span><span class="sxs-lookup"><span data-stu-id="9d58b-131">In a shared hosting environment where applications are mutually untrusted, the hosting provider should take steps to ensure OS-level isolation between applications, including separating the applications' underlying key repositories.</span></span>

<span data-ttu-id="9d58b-132">如果資料保護系統不會提供 ASP.NET Core 主機 （例如，如果開發人員會具現化它自己透過 DataProtectionProvider 具象型別）、 預設，會停用應用程式隔離和所有應用程式支援相同的金鑰處理資料可以共用裝載，只要它們提供適當的用途。</span><span class="sxs-lookup"><span data-stu-id="9d58b-132">If the data protection system is not provided by an ASP.NET Core host (e.g., if the developer instantiates it himself via the DataProtectionProvider concrete type), application isolation is disabled by default, and all applications backed by the same keying material can share payloads as long as they provide the appropriate purposes.</span></span> <span data-ttu-id="9d58b-133">若要提供應用程式隔離在此環境中的，組態物件上呼叫 SetApplicationName 方法，請參閱[程式碼範例](#data-protection-code-sample-application-name)上方。</span><span class="sxs-lookup"><span data-stu-id="9d58b-133">To provide application isolation in this environment, call the SetApplicationName method on the configuration object, see the [code sample](#data-protection-code-sample-application-name) above.</span></span>

<a name="data-protection-changing-algorithms"></a>

## <a name="changing-algorithms"></a><span data-ttu-id="9d58b-134">變更演算法</span><span class="sxs-lookup"><span data-stu-id="9d58b-134">Changing algorithms</span></span>

<span data-ttu-id="9d58b-135">資料保護堆疊允許變更新產生的索引鍵所使用的預設演算法。</span><span class="sxs-lookup"><span data-stu-id="9d58b-135">The data protection stack allows changing the default algorithm used by newly-generated keys.</span></span> <span data-ttu-id="9d58b-136">若要這樣做最簡單的方式是設定回呼，呼叫 UseCryptographicAlgorithms，在下列範例。</span><span class="sxs-lookup"><span data-stu-id="9d58b-136">The simplest way to do this is to call UseCryptographicAlgorithms from the configuration callback, as in the below example.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9d58b-137">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9d58b-137">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9d58b-138">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9d58b-138">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(new AuthenticatedEncryptionSettings()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

---

<span data-ttu-id="9d58b-139">預設的 EncryptionAlgorithm 和 ValidationAlgorithm 分別為 256-AES-CBC 和 HMACSHA256。</span><span class="sxs-lookup"><span data-stu-id="9d58b-139">The default EncryptionAlgorithm and ValidationAlgorithm are AES-256-CBC and HMACSHA256, respectively.</span></span> <span data-ttu-id="9d58b-140">透過系統管理員可以設定預設原則[機器寬原則](machine-wide-policy.md)，但 UseCryptographicAlgorithms 明確呼叫將會覆寫預設原則。</span><span class="sxs-lookup"><span data-stu-id="9d58b-140">The default policy can be set by a system administrator via [Machine Wide Policy](machine-wide-policy.md), but an explicit call to UseCryptographicAlgorithms will override the default policy.</span></span>

<span data-ttu-id="9d58b-141">呼叫 UseCryptographicAlgorithms 將允許開發人員指定所需的演算法 （從預先定義內建清單），以及開發人員不需要擔心演算法的實作。</span><span class="sxs-lookup"><span data-stu-id="9d58b-141">Calling UseCryptographicAlgorithms will allow the developer to specify the desired algorithm (from a predefined built-in list), and the developer does not need to worry about the implementation of the algorithm.</span></span> <span data-ttu-id="9d58b-142">比方說，在上面的資料保護系統案例會嘗試使用 AES 的 CNG 實作，如果在 Windows 上執行，否則它會切換回 managed System.Security.Cryptography.Aes 類別。</span><span class="sxs-lookup"><span data-stu-id="9d58b-142">For instance, in the scenario above the data protection system will attempt to use the CNG implementation of AES if running on Windows, otherwise it will fall back to the managed System.Security.Cryptography.Aes class.</span></span>

<span data-ttu-id="9d58b-143">如果想要透過 UseCustomCryptographicAlgorithms，呼叫中所述，開發人員可以手動指定實作下面範例。</span><span class="sxs-lookup"><span data-stu-id="9d58b-143">The developer can manually specify an implementation if desired via a call to UseCustomCryptographicAlgorithms, as show in the below examples.</span></span>

>[!TIP]
> <span data-ttu-id="9d58b-144">變更演算法不會影響鑰匙圈中現有的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="9d58b-144">Changing algorithms does not affect existing keys in the key ring.</span></span> <span data-ttu-id="9d58b-145">它只會影響新產生的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="9d58b-145">It only affects newly-generated keys.</span></span>

<a name="data-protection-changing-algorithms-custom-managed"></a>

### <a name="specifying-custom-managed-algorithms"></a><span data-ttu-id="9d58b-146">指定受管理的自訂演算法</span><span class="sxs-lookup"><span data-stu-id="9d58b-146">Specifying custom managed algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9d58b-147">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9d58b-147">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="9d58b-148">若要指定受管理的自訂演算法，建立指向實作類型的 ManagedAuthenticatedEncryptorConfiguration 執行個體。</span><span class="sxs-lookup"><span data-stu-id="9d58b-148">To specify custom managed algorithms, create a ManagedAuthenticatedEncryptorConfiguration instance that points to the implementation types.</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9d58b-149">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9d58b-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="9d58b-150">若要指定受管理的自訂演算法，建立指向實作類型的 ManagedAuthenticatedEncryptionSettings 執行個體。</span><span class="sxs-lookup"><span data-stu-id="9d58b-150">To specify custom managed algorithms, create a ManagedAuthenticatedEncryptionSettings instance that points to the implementation types.</span></span>

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

<span data-ttu-id="9d58b-151">通常\*類型屬性必須指向實體，可具現化 （透過公用的無參數建構函式） SymmetricAlgorithm 和 KeyedHashAlgorithm，實作雖然之系統特殊案例某些等值 typeof(Aes) 為了方便起見.</span><span class="sxs-lookup"><span data-stu-id="9d58b-151">Generally the \*Type properties must point to concrete, instantiable (via a public parameterless ctor) implementations of SymmetricAlgorithm and KeyedHashAlgorithm, though the system special-cases some values like typeof(Aes) for convenience.</span></span>

> [!NOTE]
> <span data-ttu-id="9d58b-152">SymmetricAlgorithm 必須 ≥ 128 位元金鑰長度和區塊大小為 ≥ 64 位元為單位，就必須支援 CBC 模式加密 PKCS #7 填補。</span><span class="sxs-lookup"><span data-stu-id="9d58b-152">The SymmetricAlgorithm must have a key length of ≥ 128 bits and a block size of ≥ 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="9d58b-153">KeyedHashAlgorithm 必須有摘要大小 > = 128 位元，而且它必須支援長度等於雜湊演算法的摘要長度的金鑰。</span><span class="sxs-lookup"><span data-stu-id="9d58b-153">The KeyedHashAlgorithm must have a digest size of >= 128 bits, and it must support keys of length equal to the hash algorithm's digest length.</span></span> <span data-ttu-id="9d58b-154">無法為 HMAC 絕對必要 KeyedHashAlgorithm。</span><span class="sxs-lookup"><span data-stu-id="9d58b-154">The KeyedHashAlgorithm is not strictly required to be HMAC.</span></span>

<a name="data-protection-changing-algorithms-cng"></a>

### <a name="specifying-custom-windows-cng-algorithms"></a><span data-ttu-id="9d58b-155">指定自訂的 Windows CNG 演算法</span><span class="sxs-lookup"><span data-stu-id="9d58b-155">Specifying custom Windows CNG algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9d58b-156">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9d58b-156">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="9d58b-157">若要指定自訂的 Windows CNG 演算法使用 CBC 模式加密 + HMAC 驗證，請建立 CngCbcAuthenticatedEncryptorConfiguration 執行個體，其中包含演算法的資訊。</span><span class="sxs-lookup"><span data-stu-id="9d58b-157">To specify a custom Windows CNG algorithm using CBC-mode encryption + HMAC validation, create a CngCbcAuthenticatedEncryptorConfiguration instance that contains the algorithmic information.</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9d58b-158">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9d58b-158">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="9d58b-159">若要指定自訂的 Windows CNG 演算法使用 CBC 模式加密 + HMAC 驗證，請建立 CngCbcAuthenticatedEncryptionSettings 執行個體，其中包含演算法的資訊。</span><span class="sxs-lookup"><span data-stu-id="9d58b-159">To specify a custom Windows CNG algorithm using CBC-mode encryption + HMAC validation, create a CngCbcAuthenticatedEncryptionSettings instance that contains the algorithmic information.</span></span>

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
> <span data-ttu-id="9d58b-160">對稱的區塊加密演算法必須 ≥ 128 位元金鑰長度和區塊大小為 ≥ 64 位元，且它必須支援 PKCS #7 填補 CBC 模式加密。</span><span class="sxs-lookup"><span data-stu-id="9d58b-160">The symmetric block cipher algorithm must have a key length of ≥ 128 bits and a block size of ≥ 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="9d58b-161">雜湊演算法必須有摘要大小 > = 128 位元，且必須支援開啟 BCRYPT_ALG_HANDLE_HMAC_FLAG 旗標。</span><span class="sxs-lookup"><span data-stu-id="9d58b-161">The hash algorithm must have a digest size of >= 128 bits and must support being opened with the BCRYPT_ALG_HANDLE_HMAC_FLAG flag.</span></span> <span data-ttu-id="9d58b-162">\*可以設定為 null，以使用指定之演算法的預設提供者提供者屬性。</span><span class="sxs-lookup"><span data-stu-id="9d58b-162">The \*Provider properties can be set to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="9d58b-163">請參閱[BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx)文件的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="9d58b-163">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9d58b-164">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9d58b-164">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="9d58b-165">若要指定自訂的 Windows CNG 演算法使用 Galois/計數器模式加密 + 驗證，請建立 CngGcmAuthenticatedEncryptorConfiguration 執行個體，其中包含演算法的資訊。</span><span class="sxs-lookup"><span data-stu-id="9d58b-165">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption + validation, create a CngGcmAuthenticatedEncryptorConfiguration instance that contains the algorithmic information.</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9d58b-166">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9d58b-166">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="9d58b-167">若要指定自訂的 Windows CNG 演算法使用 Galois/計數器模式加密 + 驗證，請建立 CngGcmAuthenticatedEncryptionSettings 執行個體，其中包含演算法的資訊。</span><span class="sxs-lookup"><span data-stu-id="9d58b-167">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption + validation, create a CngGcmAuthenticatedEncryptionSettings instance that contains the algorithmic information.</span></span>

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
> <span data-ttu-id="9d58b-168">對稱的區塊加密演算法必須 ≥ 128 位元金鑰長度和區塊大小為剛好為 128 位元，且它必須支援 GCM 加密。</span><span class="sxs-lookup"><span data-stu-id="9d58b-168">The symmetric block cipher algorithm must have a key length of ≥ 128 bits and a block size of exactly 128 bits, and it must support GCM encryption.</span></span> <span data-ttu-id="9d58b-169">EncryptionAlgorithmProvider 屬性可以設定為 null，使用指定之演算法的預設提供者。</span><span class="sxs-lookup"><span data-stu-id="9d58b-169">The EncryptionAlgorithmProvider property can be set to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="9d58b-170">請參閱[BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx)文件的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="9d58b-170">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

### <a name="specifying-other-custom-algorithms"></a><span data-ttu-id="9d58b-171">指定其他的自訂演算法</span><span class="sxs-lookup"><span data-stu-id="9d58b-171">Specifying other custom algorithms</span></span>

<span data-ttu-id="9d58b-172">雖然不公開為第一級的應用程式開發介面，資料保護系統是演算法的可擴充的特點可讓您指定幾乎任何類型。</span><span class="sxs-lookup"><span data-stu-id="9d58b-172">Though not exposed as a first-class API, the data protection system is extensible enough to allow specifying almost any kind of algorithm.</span></span> <span data-ttu-id="9d58b-173">例如，它是核心的可以保留在 HSM 中所包含的所有索引鍵，並提供自訂實作加密和解密常式。</span><span class="sxs-lookup"><span data-stu-id="9d58b-173">For example, it is possible to keep all keys contained within an HSM and to provide a custom implementation of the core encryption and decryption routines.</span></span> <span data-ttu-id="9d58b-174">IAuthenticatedEncryptorConfiguration 中核心加密擴充性區段，如需詳細資訊，請參閱。</span><span class="sxs-lookup"><span data-stu-id="9d58b-174">See IAuthenticatedEncryptorConfiguration in the core cryptography extensibility section for more information.</span></span>

### <a name="see-also"></a><span data-ttu-id="9d58b-175">請參閱</span><span class="sxs-lookup"><span data-stu-id="9d58b-175">See also</span></span>

* [<span data-ttu-id="9d58b-176">非 DI 感知案例</span><span class="sxs-lookup"><span data-stu-id="9d58b-176">Non DI Aware Scenarios</span></span>](non-di-scenarios.md)
* [<span data-ttu-id="9d58b-177">電腦全域原則</span><span class="sxs-lookup"><span data-stu-id="9d58b-177">Machine Wide Policy</span></span>](machine-wide-policy.md)
