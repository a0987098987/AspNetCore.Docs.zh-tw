---
title: ASP.NET Core 中的待用的金鑰加密
author: rick-anderson
description: 了解 ASP.NET Core 資料保護待用的金鑰加密的實作詳細資料。
ms.author: riande
ms.date: 07/16/2018
uid: security/data-protection/implementation/key-encryption-at-rest
ms.openlocfilehash: 52c3137dbe467096364b42430c92aecc7c15e313
ms.sourcegitcommit: 8f8924ce4eb9effeaf489f177fb01b66867da16f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/24/2018
ms.locfileid: "39219286"
---
# <a name="key-encryption-at-rest-in-aspnet-core"></a><span data-ttu-id="e25b2-103">ASP.NET Core 中的待用的金鑰加密</span><span class="sxs-lookup"><span data-stu-id="e25b2-103">Key encryption at rest in ASP.NET Core</span></span>

<span data-ttu-id="e25b2-104">資料保護系統[採用預設的探索機制](xref:security/data-protection/configuration/default-settings)來判斷如何密碼編譯金鑰應進行待用加密。</span><span class="sxs-lookup"><span data-stu-id="e25b2-104">The data protection system [employs a discovery mechanism by default](xref:security/data-protection/configuration/default-settings) to determine how cryptographic keys should be encrypted at rest.</span></span> <span data-ttu-id="e25b2-105">開發人員可以覆寫的探索機制，並以手動方式指定如何應該進行待用加密金鑰。</span><span class="sxs-lookup"><span data-stu-id="e25b2-105">The developer can override the discovery mechanism and manually specify how keys should be encrypted at rest.</span></span>

> [!WARNING]
> <span data-ttu-id="e25b2-106">如果您指定的明確[機碼的持續性位置](xref:security/data-protection/implementation/key-storage-providers)，資料保護系統的預設金鑰加密，在 rest 機制可以用來移除。</span><span class="sxs-lookup"><span data-stu-id="e25b2-106">If you specify an explicit [key persistence location](xref:security/data-protection/implementation/key-storage-providers), the data protection system deregisters the default key encryption at rest mechanism.</span></span> <span data-ttu-id="e25b2-107">因此，索引鍵不會再加密靜止。</span><span class="sxs-lookup"><span data-stu-id="e25b2-107">Consequently, keys are no longer encrypted at rest.</span></span> <span data-ttu-id="e25b2-108">我們建議您[指定明確的金鑰加密機制](xref:security/data-protection/implementation/key-encryption-at-rest)生產環境部署。</span><span class="sxs-lookup"><span data-stu-id="e25b2-108">We recommend that you [specify an explicit key encryption mechanism](xref:security/data-protection/implementation/key-encryption-at-rest) for production deployments.</span></span> <span data-ttu-id="e25b2-109">待用加密機制選項是以本主題所述。</span><span class="sxs-lookup"><span data-stu-id="e25b2-109">The encryption-at-rest mechanism options are described in this topic.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="azure-key-vault"></a><span data-ttu-id="e25b2-110">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="e25b2-110">Azure Key Vault</span></span>

<span data-ttu-id="e25b2-111">金鑰存放在[Azure Key Vault](https://azure.microsoft.com/services/key-vault/)，設定與系統[ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault)在`Startup`類別：</span><span class="sxs-lookup"><span data-stu-id="e25b2-111">To store keys in [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), configure the system with [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) in the `Startup` class:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

<span data-ttu-id="e25b2-112">如需詳細資訊，請參閱 <<c0> [ 設定 ASP.NET Core 資料保護： ProtectKeysWithAzureKeyVault](xref:security/data-protection/configuration/overview#protectkeyswithazurekeyvault)。</span><span class="sxs-lookup"><span data-stu-id="e25b2-112">For more information, see [Configure ASP.NET Core Data Protection: ProtectKeysWithAzureKeyVault](xref:security/data-protection/configuration/overview#protectkeyswithazurekeyvault).</span></span>

::: moniker-end

## <a name="windows-dpapi"></a><span data-ttu-id="e25b2-113">Windows DPAPI</span><span class="sxs-lookup"><span data-stu-id="e25b2-113">Windows DPAPI</span></span>

<span data-ttu-id="e25b2-114">**僅適用於 Windows 的部署。**</span><span class="sxs-lookup"><span data-stu-id="e25b2-114">**Only applies to Windows deployments.**</span></span>

<span data-ttu-id="e25b2-115">使用 Windows DPAPI 時，使用加密金鑰材料[CryptProtectData](/windows/desktop/api/dpapi/nf-dpapi-cryptprotectdata)之前要保存至儲存體。</span><span class="sxs-lookup"><span data-stu-id="e25b2-115">When Windows DPAPI is used, key material is encrypted with [CryptProtectData](/windows/desktop/api/dpapi/nf-dpapi-cryptprotectdata) before being persisted to storage.</span></span> <span data-ttu-id="e25b2-116">DPAPI 是永遠不會讀取目前的電腦以外的資料的適當的加密機制 (不過它可以備份到 Active Directory 的這些金鑰，請參閱[DPAPI 和漫遊設定檔](https://support.microsoft.com/kb/309408/#6))。</span><span class="sxs-lookup"><span data-stu-id="e25b2-116">DPAPI is an appropriate encryption mechanism for data that's never read outside of the current machine (though it's possible to back these keys up to Active Directory; see [DPAPI and Roaming Profiles](https://support.microsoft.com/kb/309408/#6)).</span></span> <span data-ttu-id="e25b2-117">若要設定 DPAPI 金鑰待用加密，請呼叫其中一個[ProtectKeysWithDpapi](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpapi)擴充方法：</span><span class="sxs-lookup"><span data-stu-id="e25b2-117">To configure DPAPI key-at-rest encryption, call one of the [ProtectKeysWithDpapi](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpapi) extension methods:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Only the local user account can decrypt the keys
    services.AddDataProtection()
        .ProtectKeysWithDpapi();
}
```

<span data-ttu-id="e25b2-118">如果`ProtectKeysWithDpapi`呼叫沒有參數，只有目前的 Windows 使用者帳戶可解密保存的金鑰環。</span><span class="sxs-lookup"><span data-stu-id="e25b2-118">If `ProtectKeysWithDpapi` is called with no parameters, only the current Windows user account can decipher the persisted key ring.</span></span> <span data-ttu-id="e25b2-119">您可以選擇性地指定任何電腦 （不只是目前的使用者帳戶） 上的使用者帳戶能夠解密金鑰環：</span><span class="sxs-lookup"><span data-stu-id="e25b2-119">You can optionally specify that any user account on the machine (not just the current user account) be able to decipher the key ring:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // All user accounts on the machine can decrypt the keys
    services.AddDataProtection()
        .ProtectKeysWithDpapi(protectToLocalMachine: true);
}
```

::: moniker range=">= aspnetcore-2.0"

## <a name="x509-certificate"></a><span data-ttu-id="e25b2-120">X.509 憑證</span><span class="sxs-lookup"><span data-stu-id="e25b2-120">X.509 certificate</span></span>

<span data-ttu-id="e25b2-121">如果應用程式則會分散到多部電腦，可能會方便分散機器上的共用的 X.509 憑證來設定裝載的應用程式，以使用憑證來加密待用的金鑰：</span><span class="sxs-lookup"><span data-stu-id="e25b2-121">If the app is spread across multiple machines, it may be convenient to distribute a shared X.509 certificate across the machines and configure the hosted apps to use the certificate for encryption of keys at rest:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .ProtectKeysWithCertificate("3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0");
}
```

<span data-ttu-id="e25b2-122">由於.NET Framework 的限制，支援只使用 CAPI 私用金鑰的憑證。</span><span class="sxs-lookup"><span data-stu-id="e25b2-122">Due to .NET Framework limitations, only certificates with CAPI private keys are supported.</span></span> <span data-ttu-id="e25b2-123">請參閱下面的內容，這些限制可能的因應措施。</span><span class="sxs-lookup"><span data-stu-id="e25b2-123">See the content below for possible workarounds to these limitations.</span></span>

::: moniker-end

## <a name="windows-dpapi-ng"></a><span data-ttu-id="e25b2-124">Windows DPAPI NG</span><span class="sxs-lookup"><span data-stu-id="e25b2-124">Windows DPAPI-NG</span></span>

<span data-ttu-id="e25b2-125">**這項機制是僅適用於 Windows 8/Windows Server 2012 或更新版本。**</span><span class="sxs-lookup"><span data-stu-id="e25b2-125">**This mechanism is available only on Windows 8/Windows Server 2012 or later.**</span></span>

<span data-ttu-id="e25b2-126">從 Windows 8 開始，Windows 作業系統支援 DPAPI NG （也稱為 CNG DPAPI）。</span><span class="sxs-lookup"><span data-stu-id="e25b2-126">Beginning with Windows 8, Windows OS supports DPAPI-NG (also called CNG DPAPI).</span></span> <span data-ttu-id="e25b2-127">如需詳細資訊，請參閱 <<c0> [ 需 CNG DPAPI](/windows/desktop/SecCNG/cng-dpapi)。</span><span class="sxs-lookup"><span data-stu-id="e25b2-127">For more information, see [About CNG DPAPI](/windows/desktop/SecCNG/cng-dpapi).</span></span>

<span data-ttu-id="e25b2-128">主體會編碼為保護描述項規則。</span><span class="sxs-lookup"><span data-stu-id="e25b2-128">The principal is encoded as a protection descriptor rule.</span></span> <span data-ttu-id="e25b2-129">在下列範例會呼叫[ProtectKeysWithDpapiNG](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpaping)，只有指定的 SID 的已加入網域的使用者可以解密金鑰環：</span><span class="sxs-lookup"><span data-stu-id="e25b2-129">In the following example that calls [ProtectKeysWithDpapiNG](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpaping), only the domain-joined user with the specified SID can decrypt the key ring:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Uses the descriptor rule "SID=S-1-5-21-..."
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG("SID=S-1-5-21-...",
        flags: DpapiNGProtectionDescriptorFlags.None);
}
```

<span data-ttu-id="e25b2-130">另外還有的無參數多載`ProtectKeysWithDpapiNG`。</span><span class="sxs-lookup"><span data-stu-id="e25b2-130">There's also a parameterless overload of `ProtectKeysWithDpapiNG`.</span></span> <span data-ttu-id="e25b2-131">使用這個便利方法，以指定的規則"SID = {CURRENT_ACCOUNT_SID}"，其中*CURRENT_ACCOUNT_SID*是目前的 Windows 使用者帳戶的 SID:</span><span class="sxs-lookup"><span data-stu-id="e25b2-131">Use this convenience method to specify the rule "SID={CURRENT_ACCOUNT_SID}", where *CURRENT_ACCOUNT_SID* is the SID of the current Windows user account:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Use the descriptor rule "SID={current account SID}"
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG();
}
```

<span data-ttu-id="e25b2-132">在此案例中，AD 網域控制站負責散發 DPAPI NG 作業所使用的加密金鑰。</span><span class="sxs-lookup"><span data-stu-id="e25b2-132">In this scenario, the AD domain controller is responsible for distributing the encryption keys used by the DPAPI-NG operations.</span></span> <span data-ttu-id="e25b2-133">目標使用者可解密加密的裝載，從任何已加入網域的電腦 （前提是處理序正在執行的身分識別）。</span><span class="sxs-lookup"><span data-stu-id="e25b2-133">The target user can decipher the encrypted payload from any domain-joined machine (provided that the process is running under their identity).</span></span>

## <a name="certificate-based-encryption-with-windows-dpapi-ng"></a><span data-ttu-id="e25b2-134">使用憑證式加密使用 Windows DPAPI NG</span><span class="sxs-lookup"><span data-stu-id="e25b2-134">Certificate-based encryption with Windows DPAPI-NG</span></span>

<span data-ttu-id="e25b2-135">如果應用程式在 Windows 8.1 / Windows Server 2012 R2 上執行，或更新版本中，您可以使用 Windows DPAPI NG，來執行憑證為基礎的加密。</span><span class="sxs-lookup"><span data-stu-id="e25b2-135">If the app is running on Windows 8.1/Windows Server 2012 R2 or later, you can use Windows DPAPI-NG to perform certificate-based encryption.</span></span> <span data-ttu-id="e25b2-136">使用 規則描述項字串 「 憑證 = HashId:THUMBPRINT"，其中*指紋*是十六進位編碼的 SHA1 指紋的憑證：</span><span class="sxs-lookup"><span data-stu-id="e25b2-136">Use the rule descriptor string "CERTIFICATE=HashId:THUMBPRINT", where *THUMBPRINT* is the hex-encoded SHA1 thumbprint of the certificate:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG("CERTIFICATE=HashId:3BCE558E2...B5AEA2A9BD2575A0",
            flags: DpapiNGProtectionDescriptorFlags.None);
}
```

<span data-ttu-id="e25b2-137">任何指向這個存放庫的應用程式必須執行 Windows 8.1 / Windows Server 2012 R2 或更新版本，才能解密金鑰。</span><span class="sxs-lookup"><span data-stu-id="e25b2-137">Any app pointed at this repository must be running on Windows 8.1/Windows Server 2012 R2 or later to decipher the keys.</span></span>

## <a name="custom-key-encryption"></a><span data-ttu-id="e25b2-138">自訂金鑰加密</span><span class="sxs-lookup"><span data-stu-id="e25b2-138">Custom key encryption</span></span>

<span data-ttu-id="e25b2-139">如果內建機制不適當，開發人員可以指定自己的金鑰加密機制藉由提供自訂[IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor)。</span><span class="sxs-lookup"><span data-stu-id="e25b2-139">If the in-box mechanisms aren't appropriate, the developer can specify their own key encryption mechanism by providing a custom [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor).</span></span>
