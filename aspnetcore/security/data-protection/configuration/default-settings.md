---
title: 資料保護金鑰管理和 ASP.NET Core 存留期
author: rick-anderson
description: 了解資料保護的金鑰管理和 ASP.NET Core 存留期。
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/configuration/default-settings
ms.openlocfilehash: b43c14af015d5e03f46200c51a1218a581b1de0c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
ms.locfileid: "28887286"
---
# <a name="data-protection-key-management-and-lifetime-in-aspnet-core"></a><span data-ttu-id="412cb-103">資料保護金鑰管理和 ASP.NET Core 存留期</span><span class="sxs-lookup"><span data-stu-id="412cb-103">Data Protection key management and lifetime in ASP.NET Core</span></span>

<span data-ttu-id="412cb-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="412cb-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="key-management"></a><span data-ttu-id="412cb-105">金鑰管理</span><span class="sxs-lookup"><span data-stu-id="412cb-105">Key management</span></span>

<span data-ttu-id="412cb-106">應用程式會嘗試偵測其操作環境和處理自己的金鑰組態而有所不同。</span><span class="sxs-lookup"><span data-stu-id="412cb-106">The app attempts to detect its operational environment and handle key configuration on its own.</span></span>

1. <span data-ttu-id="412cb-107">如果應用程式裝載在[Azure 應用程式](https://azure.microsoft.com/services/app-service/)，金鑰會保存到 *%HOME%\ASP.NET\DataProtection-Keys*資料夾。</span><span class="sxs-lookup"><span data-stu-id="412cb-107">If the app is hosted in [Azure Apps](https://azure.microsoft.com/services/app-service/), keys are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="412cb-108">此資料夾受到網路儲存裝置，而且裝載應用程式的所有電腦上同步處理。</span><span class="sxs-lookup"><span data-stu-id="412cb-108">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span>
   * <span data-ttu-id="412cb-109">索引鍵未受保護靜止。</span><span class="sxs-lookup"><span data-stu-id="412cb-109">Keys aren't protected at rest.</span></span>
   * <span data-ttu-id="412cb-110">*DataProtection 金鑰*資料夾提供單一部署位置中的應用程式的所有執行個體索引鍵環形。</span><span class="sxs-lookup"><span data-stu-id="412cb-110">The *DataProtection-Keys* folder supplies the key ring to all instances of an app in a single deployment slot.</span></span>
   * <span data-ttu-id="412cb-111">不同的部署位置，例如預備和生產環境，不會共用金鑰環形。</span><span class="sxs-lookup"><span data-stu-id="412cb-111">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span> <span data-ttu-id="412cb-112">當您交換兩個部署位置，例如交換到生產環境的臨時區域，或使用 A / B 測試，使用資料保護的任何應用程式將無法使用金鑰環形內的前一個插槽的預存的資料解密。</span><span class="sxs-lookup"><span data-stu-id="412cb-112">When you swap between deployment slots, for example swapping Staging to Production or using A/B testing, any app using Data Protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="412cb-113">這會導致使用者記錄從應用程式會使用標準 ASP.NET Core cookie 驗證中，因為它使用資料保護來保護其 cookie。</span><span class="sxs-lookup"><span data-stu-id="412cb-113">This leads to users being logged out of an app that uses the standard ASP.NET Core cookie authentication, as it uses Data Protection to protect its cookies.</span></span> <span data-ttu-id="412cb-114">如果您想要獨立位置的索引鍵環形，使用外部索引鍵環提供者，例如 Azure Blob 儲存體，Azure 金鑰保存庫中，SQL 存放區或 Redis 快取。</span><span class="sxs-lookup"><span data-stu-id="412cb-114">If you desire slot-independent key rings, use an external key ring provider, such as Azure Blob Storage, Azure Key Vault, a SQL store, or Redis cache.</span></span>

1. <span data-ttu-id="412cb-115">如果使用者設定檔可用時，會保存金鑰 *%LOCALAPPDATA%\ASP.NET\DataProtection-Keys*資料夾。</span><span class="sxs-lookup"><span data-stu-id="412cb-115">If the user profile is available, keys are persisted to the *%LOCALAPPDATA%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="412cb-116">如果作業系統為 Windows，該金鑰會加密在靜止使用 DPAPI。</span><span class="sxs-lookup"><span data-stu-id="412cb-116">If the operating system is Windows, the keys are encrypted at rest using DPAPI.</span></span>

1. <span data-ttu-id="412cb-117">如果應用程式裝載在 IIS 中，會保存 ACLed 只是為了背景工作處理序帳戶的特殊的登錄機碼 HKLM 登錄機碼。</span><span class="sxs-lookup"><span data-stu-id="412cb-117">If the app is hosted in IIS, keys are persisted to the HKLM registry in a special registry key that's ACLed only to the worker process account.</span></span> <span data-ttu-id="412cb-118">在待用期間使用 DPAPI 加密金鑰。</span><span class="sxs-lookup"><span data-stu-id="412cb-118">Keys are encrypted at rest using DPAPI.</span></span>

1. <span data-ttu-id="412cb-119">如果上述條件均不相符時，索引鍵不被保存在目前的程序之外。</span><span class="sxs-lookup"><span data-stu-id="412cb-119">If none of these conditions match, keys aren't persisted outside of the current process.</span></span> <span data-ttu-id="412cb-120">處理程序關閉時，產生所有索引鍵將會遺失。</span><span class="sxs-lookup"><span data-stu-id="412cb-120">When the process shuts down, all generated keys are lost.</span></span>

<span data-ttu-id="412cb-121">開發人員一定是完全控制，而且可以覆寫的方式，以及索引鍵儲存位置。</span><span class="sxs-lookup"><span data-stu-id="412cb-121">The developer is always in full control and can override how and where keys are stored.</span></span> <span data-ttu-id="412cb-122">上面的前三個選項應該很好的預設值提供給大部分的應用程式，類似 ASP.NET  **\<machineKey >** 自動產生常式過去。</span><span class="sxs-lookup"><span data-stu-id="412cb-122">The first three options above should provide good defaults for most apps similar to how the ASP.NET **\<machineKey>** auto-generation routines worked in the past.</span></span> <span data-ttu-id="412cb-123">最後，後援選項是需要開發人員指定的唯一案例[組態](xref:security/data-protection/configuration/overview)前方如果他們想要的索引鍵的持續性，但此後援只會在極少數的情況下發生。</span><span class="sxs-lookup"><span data-stu-id="412cb-123">The final, fallback option is the only scenario that requires the developer to specify [configuration](xref:security/data-protection/configuration/overview) upfront if they want key persistence, but this fallback only occurs in rare situations.</span></span>

<span data-ttu-id="412cb-124">當主控在 Docker 容器中，金鑰應保留在 Docker 磁碟區 （共用磁碟區或主機掛接的磁碟區容器的存留期保存） 的資料夾或外部提供者，例如[Azure 金鑰保存庫](https://azure.microsoft.com/services/key-vault/)或[Redis](https://redis.io/)。</span><span class="sxs-lookup"><span data-stu-id="412cb-124">When hosting in a Docker container, keys should be persisted in a folder that's a Docker volume (a shared volume or a host-mounted volume that persists beyond the container's lifetime) or in an external provider, such as [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) or [Redis](https://redis.io/).</span></span> <span data-ttu-id="412cb-125">外部提供者也是在 web 伺服陣列案例中很有用的應用程式無法存取網路共用磁碟區 (請參閱[PersistKeysToFileSystem](xref:security/data-protection/configuration/overview#persistkeystofilesystem)如需詳細資訊)。</span><span class="sxs-lookup"><span data-stu-id="412cb-125">An external provider is also useful in web farm scenarios if apps can't access a shared network volume (see [PersistKeysToFileSystem](xref:security/data-protection/configuration/overview#persistkeystofilesystem) for more information).</span></span>

> [!WARNING]
> <span data-ttu-id="412cb-126">如果開發人員覆寫以上所述的規則，並指向特定的金鑰儲存機制的資料保護系統，會停用自動加密在靜止的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="412cb-126">If the developer overrides the rules outlined above and points the Data Protection system at a specific key repository, automatic encryption of keys at rest is disabled.</span></span> <span data-ttu-id="412cb-127">在 rest 保護可以透過重新啟用[組態](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="412cb-127">At-rest protection can be re-enabled via [configuration](xref:security/data-protection/configuration/overview).</span></span>

## <a name="key-lifetime"></a><span data-ttu-id="412cb-128">金鑰存留期</span><span class="sxs-lookup"><span data-stu-id="412cb-128">Key lifetime</span></span>

<span data-ttu-id="412cb-129">索引鍵具有預設的 90 天的存留期。</span><span class="sxs-lookup"><span data-stu-id="412cb-129">Keys have a 90-day lifetime by default.</span></span> <span data-ttu-id="412cb-130">金鑰過期時，應用程式會自動產生新的金鑰，然後將新的金鑰設定為作用中金鑰。</span><span class="sxs-lookup"><span data-stu-id="412cb-130">When a key expires, the app automatically generates a new key and sets the new key as the active key.</span></span> <span data-ttu-id="412cb-131">只要已停用的索引鍵會保留在系統上，您的應用程式可以解密與它們保護的任何資料。</span><span class="sxs-lookup"><span data-stu-id="412cb-131">As long as retired keys remain on the system, your app can decrypt any data protected with them.</span></span> <span data-ttu-id="412cb-132">請參閱[金鑰管理](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="412cb-132">See [key management](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling) for more information.</span></span>

## <a name="default-algorithms"></a><span data-ttu-id="412cb-133">預設的演算法</span><span class="sxs-lookup"><span data-stu-id="412cb-133">Default algorithms</span></span>

<span data-ttu-id="412cb-134">使用的預設裝載保護演算法為 256-AES-CBC 機密性和 HMACSHA256 的真實性。</span><span class="sxs-lookup"><span data-stu-id="412cb-134">The default payload protection algorithm used is AES-256-CBC for confidentiality and HMACSHA256 for authenticity.</span></span> <span data-ttu-id="412cb-135">512 位元主要金鑰，每 90 天變更，用來衍生這些每個裝載為基礎的演算法使用兩個子機碼。</span><span class="sxs-lookup"><span data-stu-id="412cb-135">A 512-bit master key, changed every 90 days, is used to derive the two sub-keys used for these algorithms on a per-payload basis.</span></span> <span data-ttu-id="412cb-136">請參閱[子機碼衍生](xref:security/data-protection/implementation/subkeyderivation#additional-authenticated-data-and-subkey-derivation)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="412cb-136">See [subkey derivation](xref:security/data-protection/implementation/subkeyderivation#additional-authenticated-data-and-subkey-derivation) for more information.</span></span>

## <a name="see-also"></a><span data-ttu-id="412cb-137">另請參閱</span><span class="sxs-lookup"><span data-stu-id="412cb-137">See also</span></span>

* [<span data-ttu-id="412cb-138">金鑰管理擴充性</span><span class="sxs-lookup"><span data-stu-id="412cb-138">Key management extensibility</span></span>](xref:security/data-protection/extensibility/key-management)
