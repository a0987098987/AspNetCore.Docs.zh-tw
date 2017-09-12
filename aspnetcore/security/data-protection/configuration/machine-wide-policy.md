---
title: "寬型電腦的原則"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 285ae47d-e0bf-4b03-b0a8-2b1fb18bc3a1
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/machine-wide-policy
ms.openlocfilehash: 7ada940acfbb7fb0887fd7c0cd722bf62f211248
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/12/2017
---
# <a name="machine-wide-policy"></a><span data-ttu-id="11cc3-103">寬型電腦的原則</span><span class="sxs-lookup"><span data-stu-id="11cc3-103">Machine Wide Policy</span></span>

<a name=data-protection-configuration-machinewidepolicy></a>

<span data-ttu-id="11cc3-104">Windows 上執行時，資料保護系統具有有限的支援設定預設全機器原則的所有的應用程式的使用資料保護。</span><span class="sxs-lookup"><span data-stu-id="11cc3-104">When running on Windows, the data protection system has limited support for setting default machine-wide policy for all applications which consume data protection.</span></span> <span data-ttu-id="11cc3-105">常見用法是系統管理員可能會想要變更的某些預設設定 （例如演算法或金鑰使用存留期） 而不需要手動更新每個應用程式在電腦上。</span><span class="sxs-lookup"><span data-stu-id="11cc3-105">The general idea is that an administrator might wish to change some default setting (such as algorithms used or key lifetime) without needing to manually update every application on the machine.</span></span>

>[!WARNING]
> <span data-ttu-id="11cc3-106">系統管理員可以設定預設原則，但無法強制執行它。</span><span class="sxs-lookup"><span data-stu-id="11cc3-106">The system administrator can set default policy, but they cannot enforce it.</span></span> <span data-ttu-id="11cc3-107">應用程式開發人員可以永遠會覆寫以自己選擇的任何值。</span><span class="sxs-lookup"><span data-stu-id="11cc3-107">The application developer can always override any value with one of their own choosing.</span></span> <span data-ttu-id="11cc3-108">預設原則只會影響應用程式開發人員尚未指定某些特定設定的外顯值。</span><span class="sxs-lookup"><span data-stu-id="11cc3-108">The default policy only affects applications where the developer has not specified an explicit value for some particular setting.</span></span>

## <a name="setting-default-policy"></a><span data-ttu-id="11cc3-109">設定預設原則</span><span class="sxs-lookup"><span data-stu-id="11cc3-109">Setting default policy</span></span>

<span data-ttu-id="11cc3-110">若要設定預設原則，系統管理員可以設定下列機碼下的系統登錄中的已知的值。</span><span class="sxs-lookup"><span data-stu-id="11cc3-110">To set default policy, an administrator can set known values in the system registry under the following key.</span></span>

<span data-ttu-id="11cc3-111">登錄機碼：`HKLM\SOFTWARE\Microsoft\DotNetPackages\Microsoft.AspNetCore.DataProtection`</span><span class="sxs-lookup"><span data-stu-id="11cc3-111">Reg key: `HKLM\SOFTWARE\Microsoft\DotNetPackages\Microsoft.AspNetCore.DataProtection`</span></span>

<span data-ttu-id="11cc3-112">如果您在 64 位元作業系統上並想要影響的 32 位元應用程式行為，請記得也設定 Wow6432Node 相當於上述機碼。</span><span class="sxs-lookup"><span data-stu-id="11cc3-112">If you're on a 64-bit operating system and want to affect the behavior of 32-bit applications, remember also to configure the Wow6432Node equivalent of the above key.</span></span>

<span data-ttu-id="11cc3-113">支援的值為：</span><span class="sxs-lookup"><span data-stu-id="11cc3-113">The supported values are:</span></span>

* <span data-ttu-id="11cc3-114">EncryptionType [字串]-指定哪些演算法適用於資料保護。</span><span class="sxs-lookup"><span data-stu-id="11cc3-114">EncryptionType [string] - specifies which algorithms should be used for data protection.</span></span> <span data-ttu-id="11cc3-115">此值必須是 「 CNG CBC 」、 「 CNG-GCM 」 或 「 受管理 」，並且會更詳細地描述[下方](#data-protection-encryption-types)。</span><span class="sxs-lookup"><span data-stu-id="11cc3-115">This value must be "CNG-CBC", "CNG-GCM", or "Managed" and is described in more detail [below](#data-protection-encryption-types).</span></span>

* <span data-ttu-id="11cc3-116">DefaultKeyLifetime [DWORD]-指定新產生的索引鍵的存留期。</span><span class="sxs-lookup"><span data-stu-id="11cc3-116">DefaultKeyLifetime [DWORD] - specifies the lifetime for newly-generated keys.</span></span> <span data-ttu-id="11cc3-117">這個值會指定以天為單位，而且必須 ≥ 7。</span><span class="sxs-lookup"><span data-stu-id="11cc3-117">This value is specified in days and must be ≥ 7.</span></span>

* <span data-ttu-id="11cc3-118">KeyEscrowSinks [字串]-指定將使用金鑰委付的類型。</span><span class="sxs-lookup"><span data-stu-id="11cc3-118">KeyEscrowSinks [string] - specifies the types which will be used for key escrow.</span></span> <span data-ttu-id="11cc3-119">這個值是以分號分隔清單的金鑰委付接收，其中每個清單中的項目是實作 IKeyEscrowSink 類型的組件限定的名稱。</span><span class="sxs-lookup"><span data-stu-id="11cc3-119">This value is a semicolon-delimited list of key escrow sinks, where each element in the list is the assembly qualified name of a type which implements IKeyEscrowSink.</span></span>

<a name=data-protection-encryption-types></a>

### <a name="encryption-types"></a><span data-ttu-id="11cc3-120">加密類型</span><span class="sxs-lookup"><span data-stu-id="11cc3-120">Encryption types</span></span>

<span data-ttu-id="11cc3-121">如果 「 CNG CBC"EncryptionType，系統將設定為搭配 Windows CNG 提供服務的真實性使用 CBC 模式對稱的區塊編碼器的機密性和 HMAC (請參閱[指定自訂的 Windows CNG 演算法](overview.md#data-protection-changing-algorithms-cng)如需詳細資訊)。</span><span class="sxs-lookup"><span data-stu-id="11cc3-121">If EncryptionType is "CNG-CBC", the system will be configured to use a CBC-mode symmetric block cipher for confidentiality and HMAC for authenticity with services provided by Windows CNG (see [Specifying custom Windows CNG algorithms](overview.md#data-protection-changing-algorithms-cng) for more details).</span></span> <span data-ttu-id="11cc3-122">支援下列的其他值，其中每一個都對應至 CngCbcAuthenticatedEncryptionSettings 類型上的屬性：</span><span class="sxs-lookup"><span data-stu-id="11cc3-122">The following additional values are supported, each of which corresponds to a property on the CngCbcAuthenticatedEncryptionSettings type:</span></span>

* <span data-ttu-id="11cc3-123">EncryptionAlgorithm [字串]-了解的 CNG 對稱的區塊加密演算法的名稱。</span><span class="sxs-lookup"><span data-stu-id="11cc3-123">EncryptionAlgorithm [string] - the name of a symmetric block cipher algorithm understood by CNG.</span></span> <span data-ttu-id="11cc3-124">此演算法會 CBC 模式中開啟。</span><span class="sxs-lookup"><span data-stu-id="11cc3-124">This algorithm will be opened in CBC mode.</span></span>

* <span data-ttu-id="11cc3-125">EncryptionAlgorithmProvider [字串]-這可能會產生 EncryptionAlgorithm 演算法的 CNG 提供者實作的名稱。</span><span class="sxs-lookup"><span data-stu-id="11cc3-125">EncryptionAlgorithmProvider [string] - the name of the CNG provider implementation which can produce the algorithm EncryptionAlgorithm.</span></span>

* <span data-ttu-id="11cc3-126">EncryptionAlgorithmKeySize [DWORD]-要衍生對稱的區塊加密演算法的金鑰長度 （以位元）。</span><span class="sxs-lookup"><span data-stu-id="11cc3-126">EncryptionAlgorithmKeySize [DWORD] - the length (in bits) of the key to derive for the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="11cc3-127">HashAlgorithm [字串]-了解 CNG 的雜湊演算法的名稱。</span><span class="sxs-lookup"><span data-stu-id="11cc3-127">HashAlgorithm [string] - the name of a hash algorithm understood by CNG.</span></span> <span data-ttu-id="11cc3-128">此演算法將 HMAC 模式中開啟。</span><span class="sxs-lookup"><span data-stu-id="11cc3-128">This algorithm will be opened in HMAC mode.</span></span>

* <span data-ttu-id="11cc3-129">HashAlgorithmProvider [字串]-這可能會產生 HashAlgorithm 演算法的 CNG 提供者實作的名稱。</span><span class="sxs-lookup"><span data-stu-id="11cc3-129">HashAlgorithmProvider [string] - the name of the CNG provider implementation which can produce the algorithm HashAlgorithm.</span></span>

<span data-ttu-id="11cc3-130">如果 EncryptionType 為 「 CNG GCM"，系統將會設定用於 Galois/計數器模式對稱的區塊編碼器機密性與驗證與 Windows CNG 所提供的服務 (請參閱[指定自訂的 Windows CNG 演算法](overview.md#data-protection-changing-algorithms-cng)如需詳細資訊)。</span><span class="sxs-lookup"><span data-stu-id="11cc3-130">If EncryptionType is "CNG-GCM", the system will be configured to use a Galois/Counter Mode symmetric block cipher for confidentiality and authenticity with services provided by Windows CNG (see [Specifying custom Windows CNG algorithms](overview.md#data-protection-changing-algorithms-cng) for more details).</span></span> <span data-ttu-id="11cc3-131">支援下列的其他值，其中每一個都對應至 CngGcmAuthenticatedEncryptionSettings 類型上的屬性：</span><span class="sxs-lookup"><span data-stu-id="11cc3-131">The following additional values are supported, each of which corresponds to a property on the CngGcmAuthenticatedEncryptionSettings type:</span></span>

* <span data-ttu-id="11cc3-132">EncryptionAlgorithm [字串]-了解的 CNG 對稱的區塊加密演算法的名稱。</span><span class="sxs-lookup"><span data-stu-id="11cc3-132">EncryptionAlgorithm [string] - the name of a symmetric block cipher algorithm understood by CNG.</span></span> <span data-ttu-id="11cc3-133">此演算法將 Galois/計數器模式中開啟。</span><span class="sxs-lookup"><span data-stu-id="11cc3-133">This algorithm will be opened in Galois/Counter Mode.</span></span>

* <span data-ttu-id="11cc3-134">EncryptionAlgorithmProvider [字串]-這可能會產生 EncryptionAlgorithm 演算法的 CNG 提供者實作的名稱。</span><span class="sxs-lookup"><span data-stu-id="11cc3-134">EncryptionAlgorithmProvider [string] - the name of the CNG provider implementation which can produce the algorithm EncryptionAlgorithm.</span></span>

* <span data-ttu-id="11cc3-135">EncryptionAlgorithmKeySize [DWORD]-要衍生對稱的區塊加密演算法的金鑰長度 （以位元）。</span><span class="sxs-lookup"><span data-stu-id="11cc3-135">EncryptionAlgorithmKeySize [DWORD] - the length (in bits) of the key to derive for the symmetric block cipher algorithm.</span></span>

<span data-ttu-id="11cc3-136">如果 EncryptionType 為 「 受管理 」，系統將會設定要用於受管理的 SymmetricAlgorithm 機密性和 KeyedHashAlgorithm 的真實性 (請參閱[指定自訂管理演算法](overview.md#data-protection-changing-algorithms-custom-managed)如需詳細資訊)。</span><span class="sxs-lookup"><span data-stu-id="11cc3-136">If EncryptionType is "Managed", the system will be configured to use a managed SymmetricAlgorithm for confidentiality and KeyedHashAlgorithm for authenticity (see [Specifying custom managed algorithms](overview.md#data-protection-changing-algorithms-custom-managed) for more details).</span></span> <span data-ttu-id="11cc3-137">支援下列的其他值，其中每一個都對應至 ManagedAuthenticatedEncryptionSettings 類型上的屬性：</span><span class="sxs-lookup"><span data-stu-id="11cc3-137">The following additional values are supported, each of which corresponds to a property on the ManagedAuthenticatedEncryptionSettings type:</span></span>

* <span data-ttu-id="11cc3-138">EncryptionAlgorithmType [字串]-實作 SymmetricAlgorithm 類型的組件限定名稱。</span><span class="sxs-lookup"><span data-stu-id="11cc3-138">EncryptionAlgorithmType [string] - the assembly-qualified name of a type which implements SymmetricAlgorithm.</span></span>

* <span data-ttu-id="11cc3-139">EncryptionAlgorithmKeySize [DWORD]-要衍生的對稱式加密演算法的金鑰長度 （以位元為單位）。</span><span class="sxs-lookup"><span data-stu-id="11cc3-139">EncryptionAlgorithmKeySize [DWORD] - the length (in bits) of the key to derive for the symmetric encryption algorithm.</span></span>

* <span data-ttu-id="11cc3-140">ValidationAlgorithmType [字串]-實作 KeyedHashAlgorithm 類型的組件限定名稱。</span><span class="sxs-lookup"><span data-stu-id="11cc3-140">ValidationAlgorithmType [string] - the assembly-qualified name of a type which implements KeyedHashAlgorithm.</span></span>

<span data-ttu-id="11cc3-141">如果 EncryptionType 有任何其他值 （null 或空白），資料保護系統會在啟動時擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="11cc3-141">If EncryptionType has any other value (other than null / empty), the data protection system will throw an exception at startup.</span></span>

>[!WARNING]
> <span data-ttu-id="11cc3-142">時設定預設原則設定，包括 EncryptionAlgorithmType、 ValidationAlgorithmType （KeyEscrowSinks） 的型別名稱，類型必須是可用於應用程式。</span><span class="sxs-lookup"><span data-stu-id="11cc3-142">When configuring a default policy setting that involves type names (EncryptionAlgorithmType, ValidationAlgorithmType, KeyEscrowSinks), the types must be available to the application.</span></span> <span data-ttu-id="11cc3-143">在實務上，這表示，若為桌面 CLR 上執行的應用程式，其中包含這些類型的組件應該是 Gac 中。</span><span class="sxs-lookup"><span data-stu-id="11cc3-143">In practice, this means that for applications running on Desktop CLR, the assemblies which contain these types should be GACed.</span></span> <span data-ttu-id="11cc3-144">ASP.NET Core 應用程式上執行[.NET Core](https://www.microsoft.com/net/core)，應安裝包含這些類型的套件。</span><span class="sxs-lookup"><span data-stu-id="11cc3-144">For ASP.NET Core applications running on [.NET Core](https://www.microsoft.com/net/core), the packages which contain these types should be installed.</span></span>
