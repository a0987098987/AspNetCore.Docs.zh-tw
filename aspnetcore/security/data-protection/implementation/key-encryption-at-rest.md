---
title: "金鑰加密在靜止"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: f2bbbf4e-0945-43ce-be59-8bf19e448798
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-encryption-at-rest
ms.openlocfilehash: 5d0eb4036a3d491336cbe9357779c150b5cbb236
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/13/2017
---
# <a name="key-encryption-at-rest"></a><span data-ttu-id="ff812-103">金鑰加密在靜止</span><span class="sxs-lookup"><span data-stu-id="ff812-103">Key Encryption At Rest</span></span>

<a name="data-protection-implementation-key-encryption-at-rest"></a>

<span data-ttu-id="ff812-104">根據預設資料保護系統[採用啟發學習法](../configuration/default-settings.md#data-protection-default-settings)來判斷如何密碼編譯金鑰內容應該加密在靜止。</span><span class="sxs-lookup"><span data-stu-id="ff812-104">By default the data protection system [employs a heuristic](../configuration/default-settings.md#data-protection-default-settings) to determine how cryptographic key material should be encrypted at rest.</span></span> <span data-ttu-id="ff812-105">開發人員可以覆寫啟發學習法，並以手動方式指定索引鍵應該如何加密在靜止。</span><span class="sxs-lookup"><span data-stu-id="ff812-105">The developer can override the heuristic and manually specify how keys should be encrypted at rest.</span></span>

> [!NOTE]
> <span data-ttu-id="ff812-106">如果您指定明確的金鑰加密，在其餘的機制，資料保護系統將會取消註冊啟發學習法所提供的預設金鑰儲存機制。</span><span class="sxs-lookup"><span data-stu-id="ff812-106">If you specify an explicit key encryption at rest mechanism, the data protection system will deregister the default key storage mechanism that the heuristic provided.</span></span> <span data-ttu-id="ff812-107">您必須[指定明確的金鑰儲存機制](key-storage-providers.md#data-protection-implementation-key-storage-providers)，否則資料保護系統將無法啟動。</span><span class="sxs-lookup"><span data-stu-id="ff812-107">You must [specify an explicit key storage mechanism](key-storage-providers.md#data-protection-implementation-key-storage-providers), otherwise the data protection system will fail to start.</span></span>

<a name="data-protection-implementation-key-encryption-at-rest-providers"></a>

<span data-ttu-id="ff812-108">資料保護系統隨附三種內建金鑰加密機制。</span><span class="sxs-lookup"><span data-stu-id="ff812-108">The data protection system ships with three in-box key encryption mechanisms.</span></span>

## <a name="windows-dpapi"></a><span data-ttu-id="ff812-109">Windows DPAPI</span><span class="sxs-lookup"><span data-stu-id="ff812-109">Windows DPAPI</span></span>

<span data-ttu-id="ff812-110">*這項機制可只在 Windows 上。*</span><span class="sxs-lookup"><span data-stu-id="ff812-110">*This mechanism is available only on Windows.*</span></span>

<span data-ttu-id="ff812-111">使用 Windows DPAPI 時，會透過加密金錀[CryptProtectData](https://msdn.microsoft.com/library/windows/desktop/aa380261(v=vs.85).aspx)之前要保存至儲存體。</span><span class="sxs-lookup"><span data-stu-id="ff812-111">When Windows DPAPI is used, key material will be encrypted via [CryptProtectData](https://msdn.microsoft.com/library/windows/desktop/aa380261(v=vs.85).aspx) before being persisted to storage.</span></span> <span data-ttu-id="ff812-112">DPAPI 是適當的加密機制將永遠不會讀取目前電腦以外的資料 (雖然它可備份到 Active Directory 這些機碼，請參閱 < [DPAPI 和漫遊設定檔](https://support.microsoft.com/kb/309408/#6))。</span><span class="sxs-lookup"><span data-stu-id="ff812-112">DPAPI is an appropriate encryption mechanism for data that will never be read outside of the current machine (though it is possible to back these keys up to Active Directory; see [DPAPI and Roaming Profiles](https://support.microsoft.com/kb/309408/#6)).</span></span> <span data-ttu-id="ff812-113">例如若要設定 DPAPI 金鑰靜態加密。</span><span class="sxs-lookup"><span data-stu-id="ff812-113">For example to configure DPAPI key-at-rest encryption.</span></span>

```csharp
sc.AddDataProtection()
       // only the local user account can decrypt the keys
       .ProtectKeysWithDpapi();
   ```

<span data-ttu-id="ff812-114">如果 ProtectKeysWithDpapi 呼叫不含任何參數，只有目前 Windows 使用者帳戶可以破解保存的金鑰內容。</span><span class="sxs-lookup"><span data-stu-id="ff812-114">If ProtectKeysWithDpapi is called with no parameters, only the current Windows user account can decipher the persisted key material.</span></span> <span data-ttu-id="ff812-115">您可以選擇性地指定中所示，電腦 （不只是目前的使用者帳戶） 上的任何使用者帳戶，應該可以進行破解金鑰的內容中，下列範例。</span><span class="sxs-lookup"><span data-stu-id="ff812-115">You can optionally specify that any user account on the machine (not just the current user account) should be able to decipher the key material, as shown in the below example.</span></span>

```csharp
sc.AddDataProtection()
       // all user accounts on the machine can decrypt the keys
       .ProtectKeysWithDpapi(protectToLocalMachine: true);
   ```

## <a name="x509-certificate"></a><span data-ttu-id="ff812-116">X.509 憑證</span><span class="sxs-lookup"><span data-stu-id="ff812-116">X.509 certificate</span></span>

<span data-ttu-id="ff812-117">*這項機制並不適用於`.NET Core 1.0`或`1.1`。*</span><span class="sxs-lookup"><span data-stu-id="ff812-117">*This mechanism is not available on `.NET Core 1.0` or `1.1`.*</span></span>

<span data-ttu-id="ff812-118">如果您的應用程式則會分散到多部電腦，可能會很方便分散的機器上的共用的 X.509 憑證，並設定應用程式使用此憑證進行加密在靜止的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="ff812-118">If your application is spread across multiple machines, it may be convenient to distribute a shared X.509 certificate across the machines and to configure applications to use this certificate for encryption of keys at rest.</span></span> <span data-ttu-id="ff812-119">如需範例，請參閱下方內容。</span><span class="sxs-lookup"><span data-stu-id="ff812-119">See below for an example.</span></span>

```csharp
sc.AddDataProtection()
       // searches the cert store for the cert with this thumbprint
       .ProtectKeysWithCertificate("3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0");
   ```

<span data-ttu-id="ff812-120">由於.NET Framework 限制支援只使用 CAPI 私用金鑰的憑證。</span><span class="sxs-lookup"><span data-stu-id="ff812-120">Due to .NET Framework limitations only certificates with CAPI private keys are supported.</span></span> <span data-ttu-id="ff812-121">請參閱[憑證為基礎的加密與 Windows DPAPI-NG](#data-protection-implementation-key-encryption-at-rest-dpapi-ng)下面這些限制可能的解決方法。</span><span class="sxs-lookup"><span data-stu-id="ff812-121">See [Certificate-based encryption with Windows DPAPI-NG](#data-protection-implementation-key-encryption-at-rest-dpapi-ng) below for possible workarounds to these limitations.</span></span>

<a name="data-protection-implementation-key-encryption-at-rest-dpapi-ng"></a>

## <a name="windows-dpapi-ng"></a><span data-ttu-id="ff812-122">Windows DPAPI NG</span><span class="sxs-lookup"><span data-stu-id="ff812-122">Windows DPAPI-NG</span></span>

<span data-ttu-id="ff812-123">*這項機制是僅適用於 Windows 8 / Windows Server 2012 及更新版本。*</span><span class="sxs-lookup"><span data-stu-id="ff812-123">*This mechanism is available only on Windows 8 / Windows Server 2012 and later.*</span></span>

<span data-ttu-id="ff812-124">從 Windows 8 開始，作業系統支援 DPAPI NG （也稱為 CNG DPAPI）。</span><span class="sxs-lookup"><span data-stu-id="ff812-124">Beginning with Windows 8, the operating system supports DPAPI-NG (also called CNG DPAPI).</span></span> <span data-ttu-id="ff812-125">Microsoft 會配置其使用案例，如下所示。</span><span class="sxs-lookup"><span data-stu-id="ff812-125">Microsoft lays out its usage scenario as follows.</span></span>

   <span data-ttu-id="ff812-126">雲端運算，不過，通常需要該內容的加密在一部電腦會在另一台解密。</span><span class="sxs-lookup"><span data-stu-id="ff812-126">Cloud computing, however, often requires that content encrypted on one computer be decrypted on another.</span></span> <span data-ttu-id="ff812-127">因此，從 Windows 8 開始，Microsoft 擴充至涵蓋雲端案例使用相當簡單的應用程式開發介面的概念。</span><span class="sxs-lookup"><span data-stu-id="ff812-127">Therefore, beginning with Windows 8, Microsoft extended the idea of using a relatively straightforward API to encompass cloud scenarios.</span></span> <span data-ttu-id="ff812-128">這個新的 API，稱為 DPAPI NG，可讓您安全地共用機密資料 （金鑰、 密碼、 金鑰內容） 和訊息，藉由一組可用來取消它們在不同電腦上保護之後適當的驗證和授權的主體來保護它們。</span><span class="sxs-lookup"><span data-stu-id="ff812-128">This new API, called DPAPI-NG, enables you to securely share secrets (keys, passwords, key material) and messages by protecting them to a set of principals that can be used to unprotect them on different computers after proper authentication and authorization.</span></span>

   <span data-ttu-id="ff812-129">從[CNG Dpapi](https://msdn.microsoft.com/library/windows/desktop/hh706794(v=vs.85).aspx)</span><span class="sxs-lookup"><span data-stu-id="ff812-129">From [About CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794(v=vs.85).aspx)</span></span>

<span data-ttu-id="ff812-130">主體會編碼成保護描述項規則。</span><span class="sxs-lookup"><span data-stu-id="ff812-130">The principal is encoded as a protection descriptor rule.</span></span> <span data-ttu-id="ff812-131">請考慮下列範例中，其加密金鑰內容，只有已加入網域的使用者具有指定 SID 可以解密金鑰資料。</span><span class="sxs-lookup"><span data-stu-id="ff812-131">Consider the below example, which encrypts key material such that only the domain-joined user with the specified SID can decrypt the key material.</span></span>

```csharp
sc.AddDataProtection()
     // uses the descriptor rule "SID=S-1-5-21-..."
     .ProtectKeysWithDpapiNG("SID=S-1-5-21-...",
       flags: DpapiNGProtectionDescriptorFlags.None);
   ```

<span data-ttu-id="ff812-132">另外還有 ProtectKeysWithDpapiNG 的無參數多載。</span><span class="sxs-lookup"><span data-stu-id="ff812-132">There is also a parameterless overload of ProtectKeysWithDpapiNG.</span></span> <span data-ttu-id="ff812-133">這是便利的方法來指定規則 」 SID = 採擷"，其中我是目前的 Windows 使用者帳戶的 SID。</span><span class="sxs-lookup"><span data-stu-id="ff812-133">This is a convenience method for specifying the rule "SID=mine", where mine is the SID of the current Windows user account.</span></span>

```csharp
sc.AddDataProtection()
     // uses the descriptor rule "SID={current account SID}"
     .ProtectKeysWithDpapiNG();
   ```

<span data-ttu-id="ff812-134">在此案例中，AD 網域控制站會負責將 DPAPI NG 作業所使用的加密金鑰。</span><span class="sxs-lookup"><span data-stu-id="ff812-134">In this scenario, the AD domain controller is responsible for distributing the encryption keys used by the DPAPI-NG operations.</span></span> <span data-ttu-id="ff812-135">目標使用者將能夠解密從任何已加入網域的電腦的加密的內容 （前提是處理程序以其識別執行）。</span><span class="sxs-lookup"><span data-stu-id="ff812-135">The target user will be able to decipher the encrypted payload from any domain-joined machine (provided that the process is running under their identity).</span></span>

## <a name="certificate-based-encryption-with-windows-dpapi-ng"></a><span data-ttu-id="ff812-136">憑證為基礎的加密與 Windows DPAPI-NG 中</span><span class="sxs-lookup"><span data-stu-id="ff812-136">Certificate-based encryption with Windows DPAPI-NG</span></span>

<span data-ttu-id="ff812-137">如果您執行 Windows 8.1 / Windows Server 2012 R2 或更新版本中，您可以使用 Windows DPAPI NG 執行憑證架構的加密，即使應用程式執行[.NET Core](https://www.microsoft.com/net/core)。</span><span class="sxs-lookup"><span data-stu-id="ff812-137">If you're running on Windows 8.1 / Windows Server 2012 R2 or later, you can use Windows DPAPI-NG to perform certificate-based encryption, even if the application is running on [.NET Core](https://www.microsoft.com/net/core).</span></span> <span data-ttu-id="ff812-138">若要利用這一點，使用 規則描述項字串"憑證 = HashId:thumbprint"，其中的憑證指紋是要使用的憑證十六進位編碼 SHA1 指紋。</span><span class="sxs-lookup"><span data-stu-id="ff812-138">To take advantage of this, use the rule descriptor string "CERTIFICATE=HashId:thumbprint", where thumbprint is the hex-encoded SHA1 thumbprint of the certificate to use.</span></span> <span data-ttu-id="ff812-139">如需範例，請參閱下方內容。</span><span class="sxs-lookup"><span data-stu-id="ff812-139">See below for an example.</span></span>

```csharp
sc.AddDataProtection()
       // searches the cert store for the cert with this thumbprint
       .ProtectKeysWithDpapiNG("CERTIFICATE=HashId:3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0",
           flags: DpapiNGProtectionDescriptorFlags.None);
   ```

<span data-ttu-id="ff812-140">任何指向這個儲存機制的應用程式必須執行 Windows 8.1 / Windows Server 2012 R2 或更新版本能夠解密此機碼。</span><span class="sxs-lookup"><span data-stu-id="ff812-140">Any application which is pointed at this repository must be running on Windows 8.1 / Windows Server 2012 R2 or later to be able to decipher this key.</span></span>

## <a name="custom-key-encryption"></a><span data-ttu-id="ff812-141">自訂金鑰加密</span><span class="sxs-lookup"><span data-stu-id="ff812-141">Custom key encryption</span></span>

<span data-ttu-id="ff812-142">如果內建機制不適當，開發人員可以藉由提供自訂 IXmlEncryptor 指定自己的金鑰加密機制。</span><span class="sxs-lookup"><span data-stu-id="ff812-142">If the in-box mechanisms are not appropriate, the developer can specify their own key encryption mechanism by providing a custom IXmlEncryptor.</span></span>
