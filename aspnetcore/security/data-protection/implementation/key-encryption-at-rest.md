---
title: "金鑰加密在靜止"
author: rick-anderson
description: "本文概述 ASP.NET Core 資料保護加密在靜止的實作詳細資料。"
keywords: "ASP.NET Core 資料保護、 金鑰加密"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: f2bbbf4e-0945-43ce-be59-8bf19e448798
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-encryption-at-rest
ms.openlocfilehash: b56dc56ed94662dbedeea49022aa73941bc833c5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="key-encryption-at-rest"></a><span data-ttu-id="b23fe-104">金鑰加密在靜止</span><span class="sxs-lookup"><span data-stu-id="b23fe-104">Key Encryption At Rest</span></span>

<a name="data-protection-implementation-key-encryption-at-rest"></a>

<span data-ttu-id="b23fe-105">根據預設，資料保護系統[採用啟發學習法](xref:security/data-protection/configuration/default-settings)來判斷如何密碼編譯金鑰內容應該加密在靜止。</span><span class="sxs-lookup"><span data-stu-id="b23fe-105">By default, the data protection system [employs a heuristic](xref:security/data-protection/configuration/default-settings) to determine how cryptographic key material should be encrypted at rest.</span></span> <span data-ttu-id="b23fe-106">開發人員可以覆寫啟發學習法，並以手動方式指定索引鍵應該如何加密在靜止。</span><span class="sxs-lookup"><span data-stu-id="b23fe-106">The developer can override the heuristic and manually specify how keys should be encrypted at rest.</span></span>

> [!NOTE]
> <span data-ttu-id="b23fe-107">如果您指定明確的金鑰加密，在其餘的機制，資料保護系統將會取消註冊啟發學習法所提供的預設金鑰儲存機制。</span><span class="sxs-lookup"><span data-stu-id="b23fe-107">If you specify an explicit key encryption at rest mechanism, the data protection system will deregister the default key storage mechanism that the heuristic provided.</span></span> <span data-ttu-id="b23fe-108">您必須[指定明確的金鑰儲存機制](key-storage-providers.md#data-protection-implementation-key-storage-providers)，否則資料保護系統將無法啟動。</span><span class="sxs-lookup"><span data-stu-id="b23fe-108">You must [specify an explicit key storage mechanism](key-storage-providers.md#data-protection-implementation-key-storage-providers), otherwise the data protection system will fail to start.</span></span>

<a name="data-protection-implementation-key-encryption-at-rest-providers"></a>

<span data-ttu-id="b23fe-109">資料保護系統隨附三種內建金鑰加密機制。</span><span class="sxs-lookup"><span data-stu-id="b23fe-109">The data protection system ships with three in-box key encryption mechanisms.</span></span>

## <a name="windows-dpapi"></a><span data-ttu-id="b23fe-110">Windows DPAPI</span><span class="sxs-lookup"><span data-stu-id="b23fe-110">Windows DPAPI</span></span>

<span data-ttu-id="b23fe-111">*這項機制可只在 Windows 上。*</span><span class="sxs-lookup"><span data-stu-id="b23fe-111">*This mechanism is available only on Windows.*</span></span>

<span data-ttu-id="b23fe-112">使用 Windows DPAPI 時，會透過加密金錀[CryptProtectData](https://msdn.microsoft.com/library/windows/desktop/aa380261(v=vs.85).aspx)之前要保存至儲存體。</span><span class="sxs-lookup"><span data-stu-id="b23fe-112">When Windows DPAPI is used, key material will be encrypted via [CryptProtectData](https://msdn.microsoft.com/library/windows/desktop/aa380261(v=vs.85).aspx) before being persisted to storage.</span></span> <span data-ttu-id="b23fe-113">DPAPI 是適當的加密機制將永遠不會讀取目前電腦以外的資料 (雖然它可備份到 Active Directory 這些機碼，請參閱 < [DPAPI 和漫遊設定檔](https://support.microsoft.com/kb/309408/#6))。</span><span class="sxs-lookup"><span data-stu-id="b23fe-113">DPAPI is an appropriate encryption mechanism for data that will never be read outside of the current machine (though it is possible to back these keys up to Active Directory; see [DPAPI and Roaming Profiles](https://support.microsoft.com/kb/309408/#6)).</span></span> <span data-ttu-id="b23fe-114">例如若要設定 DPAPI 金鑰靜態加密。</span><span class="sxs-lookup"><span data-stu-id="b23fe-114">For example to configure DPAPI key-at-rest encryption.</span></span>

```csharp
sc.AddDataProtection()
    // only the local user account can decrypt the keys
    .ProtectKeysWithDpapi();
```

<span data-ttu-id="b23fe-115">如果`ProtectKeysWithDpapi`是不使用參數呼叫，只有目前 Windows 使用者帳戶可以破解保存的金鑰內容。</span><span class="sxs-lookup"><span data-stu-id="b23fe-115">If `ProtectKeysWithDpapi` is called with no parameters, only the current Windows user account can decipher the persisted key material.</span></span> <span data-ttu-id="b23fe-116">您可以選擇性地指定中所示，電腦 （不只是目前的使用者帳戶） 上的任何使用者帳戶，應該可以進行破解金鑰的內容中，下列範例。</span><span class="sxs-lookup"><span data-stu-id="b23fe-116">You can optionally specify that any user account on the machine (not just the current user account) should be able to decipher the key material, as shown in the below example.</span></span>

```csharp
sc.AddDataProtection()
    // all user accounts on the machine can decrypt the keys
    .ProtectKeysWithDpapi(protectToLocalMachine: true);
```

## <a name="x509-certificate"></a><span data-ttu-id="b23fe-117">X.509 憑證</span><span class="sxs-lookup"><span data-stu-id="b23fe-117">X.509 certificate</span></span>

<span data-ttu-id="b23fe-118">*這項機制並不適用於`.NET Core 1.0`或`1.1`。*</span><span class="sxs-lookup"><span data-stu-id="b23fe-118">*This mechanism is not available on `.NET Core 1.0` or `1.1`.*</span></span>

<span data-ttu-id="b23fe-119">如果您的應用程式則會分散到多部電腦，可能會很方便分散的機器上的共用的 X.509 憑證，並設定應用程式使用此憑證進行加密在靜止的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="b23fe-119">If your application is spread across multiple machines, it may be convenient to distribute a shared X.509 certificate across the machines and to configure applications to use this certificate for encryption of keys at rest.</span></span> <span data-ttu-id="b23fe-120">如需範例，請參閱下方內容。</span><span class="sxs-lookup"><span data-stu-id="b23fe-120">See below for an example.</span></span>

```csharp
sc.AddDataProtection()
    // searches the cert store for the cert with this thumbprint
    .ProtectKeysWithCertificate("3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0");
```

<span data-ttu-id="b23fe-121">由於.NET Framework 限制支援只使用 CAPI 私用金鑰的憑證。</span><span class="sxs-lookup"><span data-stu-id="b23fe-121">Due to .NET Framework limitations only certificates with CAPI private keys are supported.</span></span> <span data-ttu-id="b23fe-122">請參閱[憑證為基礎的加密與 Windows DPAPI-NG](#data-protection-implementation-key-encryption-at-rest-dpapi-ng)下面這些限制可能的解決方法。</span><span class="sxs-lookup"><span data-stu-id="b23fe-122">See [Certificate-based encryption with Windows DPAPI-NG](#data-protection-implementation-key-encryption-at-rest-dpapi-ng) below for possible workarounds to these limitations.</span></span>

<a name="data-protection-implementation-key-encryption-at-rest-dpapi-ng"></a>

## <a name="windows-dpapi-ng"></a><span data-ttu-id="b23fe-123">Windows DPAPI NG</span><span class="sxs-lookup"><span data-stu-id="b23fe-123">Windows DPAPI-NG</span></span>

<span data-ttu-id="b23fe-124">*這項機制是僅適用於 Windows 8 / Windows Server 2012 及更新版本。*</span><span class="sxs-lookup"><span data-stu-id="b23fe-124">*This mechanism is available only on Windows 8 / Windows Server 2012 and later.*</span></span>

<span data-ttu-id="b23fe-125">從 Windows 8 開始，作業系統支援 DPAPI NG （也稱為 CNG DPAPI）。</span><span class="sxs-lookup"><span data-stu-id="b23fe-125">Beginning with Windows 8, the operating system supports DPAPI-NG (also called CNG DPAPI).</span></span> <span data-ttu-id="b23fe-126">Microsoft 會配置其使用案例，如下所示。</span><span class="sxs-lookup"><span data-stu-id="b23fe-126">Microsoft lays out its usage scenario as follows.</span></span>

   <span data-ttu-id="b23fe-127">雲端運算，不過，通常需要該內容的加密在一部電腦會在另一台解密。</span><span class="sxs-lookup"><span data-stu-id="b23fe-127">Cloud computing, however, often requires that content encrypted on one computer be decrypted on another.</span></span> <span data-ttu-id="b23fe-128">因此，從 Windows 8 開始，Microsoft 擴充至涵蓋雲端案例使用相當簡單的應用程式開發介面的概念。</span><span class="sxs-lookup"><span data-stu-id="b23fe-128">Therefore, beginning with Windows 8, Microsoft extended the idea of using a relatively straightforward API to encompass cloud scenarios.</span></span> <span data-ttu-id="b23fe-129">這個新的 API，稱為 DPAPI NG，可讓您安全地共用機密資料 （金鑰、 密碼、 金鑰內容） 和訊息，藉由一組可用來取消它們在不同電腦上保護之後適當的驗證和授權的主體來保護它們。</span><span class="sxs-lookup"><span data-stu-id="b23fe-129">This new API, called DPAPI-NG, enables you to securely share secrets (keys, passwords, key material) and messages by protecting them to a set of principals that can be used to unprotect them on different computers after proper authentication and authorization.</span></span>

   <span data-ttu-id="b23fe-130">從[CNG Dpapi](https://msdn.microsoft.com/library/windows/desktop/hh706794(v=vs.85).aspx)</span><span class="sxs-lookup"><span data-stu-id="b23fe-130">From [About CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794(v=vs.85).aspx)</span></span>

<span data-ttu-id="b23fe-131">主體會編碼成保護描述項規則。</span><span class="sxs-lookup"><span data-stu-id="b23fe-131">The principal is encoded as a protection descriptor rule.</span></span> <span data-ttu-id="b23fe-132">請考慮下列範例中，其加密金鑰內容，只有已加入網域的使用者具有指定 SID 可以解密金鑰資料。</span><span class="sxs-lookup"><span data-stu-id="b23fe-132">Consider the below example, which encrypts key material such that only the domain-joined user with the specified SID can decrypt the key material.</span></span>

```csharp
sc.AddDataProtection()
    // uses the descriptor rule "SID=S-1-5-21-..."
    .ProtectKeysWithDpapiNG("SID=S-1-5-21-...",
    flags: DpapiNGProtectionDescriptorFlags.None);
```

<span data-ttu-id="b23fe-133">也是無參數多載`ProtectKeysWithDpapiNG`。</span><span class="sxs-lookup"><span data-stu-id="b23fe-133">There is also a parameterless overload of `ProtectKeysWithDpapiNG`.</span></span> <span data-ttu-id="b23fe-134">這是便利的方法來指定規則 」 SID = 採擷"，其中我是目前的 Windows 使用者帳戶的 SID。</span><span class="sxs-lookup"><span data-stu-id="b23fe-134">This is a convenience method for specifying the rule "SID=mine", where mine is the SID of the current Windows user account.</span></span>

```csharp
sc.AddDataProtection()
    // uses the descriptor rule "SID={current account SID}"
    .ProtectKeysWithDpapiNG();
```

<span data-ttu-id="b23fe-135">在此案例中，AD 網域控制站會負責將 DPAPI NG 作業所使用的加密金鑰。</span><span class="sxs-lookup"><span data-stu-id="b23fe-135">In this scenario, the AD domain controller is responsible for distributing the encryption keys used by the DPAPI-NG operations.</span></span> <span data-ttu-id="b23fe-136">目標使用者將能夠解密從任何已加入網域的電腦的加密的內容 （前提是處理程序以其識別執行）。</span><span class="sxs-lookup"><span data-stu-id="b23fe-136">The target user will be able to decipher the encrypted payload from any domain-joined machine (provided that the process is running under their identity).</span></span>

## <a name="certificate-based-encryption-with-windows-dpapi-ng"></a><span data-ttu-id="b23fe-137">憑證為基礎的加密與 Windows DPAPI-NG 中</span><span class="sxs-lookup"><span data-stu-id="b23fe-137">Certificate-based encryption with Windows DPAPI-NG</span></span>

<span data-ttu-id="b23fe-138">如果您執行 Windows 8.1 / Windows Server 2012 R2 或更新版本中，您可以使用 Windows DPAPI NG 執行憑證架構的加密，即使應用程式執行[.NET Core](https://www.microsoft.com/net/core)。</span><span class="sxs-lookup"><span data-stu-id="b23fe-138">If you're running on Windows 8.1 / Windows Server 2012 R2 or later, you can use Windows DPAPI-NG to perform certificate-based encryption, even if the application is running on [.NET Core](https://www.microsoft.com/net/core).</span></span> <span data-ttu-id="b23fe-139">若要利用這一點，使用 規則描述項字串"憑證 = HashId:thumbprint"，其中的憑證指紋是要使用的憑證十六進位編碼 SHA1 指紋。</span><span class="sxs-lookup"><span data-stu-id="b23fe-139">To take advantage of this, use the rule descriptor string "CERTIFICATE=HashId:thumbprint", where thumbprint is the hex-encoded SHA1 thumbprint of the certificate to use.</span></span> <span data-ttu-id="b23fe-140">如需範例，請參閱下方內容。</span><span class="sxs-lookup"><span data-stu-id="b23fe-140">See below for an example.</span></span>

```csharp
sc.AddDataProtection()
    // searches the cert store for the cert with this thumbprint
    .ProtectKeysWithDpapiNG("CERTIFICATE=HashId:3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0",
        flags: DpapiNGProtectionDescriptorFlags.None);
```

<span data-ttu-id="b23fe-141">任何指向這個儲存機制的應用程式必須執行 Windows 8.1 / Windows Server 2012 R2 或更新版本能夠解密此機碼。</span><span class="sxs-lookup"><span data-stu-id="b23fe-141">Any application which is pointed at this repository must be running on Windows 8.1 / Windows Server 2012 R2 or later to be able to decipher this key.</span></span>

## <a name="custom-key-encryption"></a><span data-ttu-id="b23fe-142">自訂金鑰加密</span><span class="sxs-lookup"><span data-stu-id="b23fe-142">Custom key encryption</span></span>

<span data-ttu-id="b23fe-143">如果內建機制不適當，開發人員可以指定自己的金鑰加密機制藉由提供自訂`IXmlEncryptor`。</span><span class="sxs-lookup"><span data-stu-id="b23fe-143">If the in-box mechanisms are not appropriate, the developer can specify their own key encryption mechanism by providing a custom `IXmlEncryptor`.</span></span>
