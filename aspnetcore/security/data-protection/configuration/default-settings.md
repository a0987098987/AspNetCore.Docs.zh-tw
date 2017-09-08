---
title: "金鑰管理和存留期"
author: rick-anderson
description: "描述金鑰管理和存留期。"
keywords: "ASP.NET Core 金鑰管理，DPAPI，DataProtection"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: ef7dad2a-7029-4ae5-8f06-1fbebedccaa4
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/default-settings
ms.openlocfilehash: 913eda69f88ef05a990d9465024f4fa6b08cd1b7
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2017
---
# <a name="key-management-and-lifetime"></a><span data-ttu-id="4bf52-104">金鑰管理和存留期</span><span class="sxs-lookup"><span data-stu-id="4bf52-104">Key management and lifetime</span></span>

<a name=data-protection-default-settings></a>

## <a name="key-management"></a><span data-ttu-id="4bf52-105">金鑰管理</span><span class="sxs-lookup"><span data-stu-id="4bf52-105">Key Management</span></span>

<span data-ttu-id="4bf52-106">系統會嘗試偵測其操作環境，並提供良好的零設定行為預設值。</span><span class="sxs-lookup"><span data-stu-id="4bf52-106">The system tries to detect its operational environment and provide good zero-configuration behavioral defaults.</span></span> <span data-ttu-id="4bf52-107">使用啟發學習法如下所示。</span><span class="sxs-lookup"><span data-stu-id="4bf52-107">The heuristic used is as follows.</span></span>

1. <span data-ttu-id="4bf52-108">如果系統正在裝載在 Azure 網站中，索引鍵會保存到"%home%\asp.net\dataprotection-keys"資料夾中。</span><span class="sxs-lookup"><span data-stu-id="4bf52-108">If the system is being hosted in Azure Web Sites, keys are persisted to the "%HOME%\ASP.NET\DataProtection-Keys" folder.</span></span> <span data-ttu-id="4bf52-109">此資料夾受到網路儲存裝置，而且裝載應用程式的所有電腦上同步處理。</span><span class="sxs-lookup"><span data-stu-id="4bf52-109">This folder is backed by network storage and is synchronized across all machines hosting the application.</span></span> <span data-ttu-id="4bf52-110">索引鍵未受保護靜止。</span><span class="sxs-lookup"><span data-stu-id="4bf52-110">Keys are not protected at rest.</span></span> <span data-ttu-id="4bf52-111">這個資料夾可提供單一部署位置中的應用程式的所有執行個體索引鍵環形。</span><span class="sxs-lookup"><span data-stu-id="4bf52-111">This folder supplies the key ring to all instances of an application in a single deployment slot.</span></span> <span data-ttu-id="4bf52-112">不同的部署位置，例如預備和生產環境，不會共用金鑰環形。</span><span class="sxs-lookup"><span data-stu-id="4bf52-112">Separate deployment slots, such as Staging and Production, will not share a key ring.</span></span> <span data-ttu-id="4bf52-113">當您交換兩個部署位置，例如交換到生產環境的臨時區域，或使用 A / B 測試，使用資料保護任何系統將無法使用金鑰環形內的前一個插槽的預存的資料解密。</span><span class="sxs-lookup"><span data-stu-id="4bf52-113">When you swap between deployment slots, for example swapping Staging to Production or using A/B testing, any system using data protection will not be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="4bf52-114">這會導致使用者記錄從 ASP.NET 應用程式使用標準 ASP.NET cookie 中介軟體，因為它使用資料保護來保護其 cookie。</span><span class="sxs-lookup"><span data-stu-id="4bf52-114">This will lead to users being logged out of an ASP.NET application that uses the standard ASP.NET cookie middleware, as it uses data protection to protect its cookies.</span></span> <span data-ttu-id="4bf52-115">如果您想要獨立位置的索引鍵環形，使用外部索引鍵環提供者，例如 Azure Blob 儲存體，Azure 金鑰保存庫中，SQL 存放區或 Redis 快取。</span><span class="sxs-lookup"><span data-stu-id="4bf52-115">If you desire slot-independent key rings, use an external key ring provider, such as Azure Blob Storage, Azure Key Vault, a SQL store, or Redis cache.</span></span>

2. <span data-ttu-id="4bf52-116">如果使用者設定檔可用時，金鑰會保存到"%localappdata%\asp.net\dataprotection-keys"資料夾。</span><span class="sxs-lookup"><span data-stu-id="4bf52-116">If the user profile is available, keys are persisted to the "%LOCALAPPDATA%\ASP.NET\DataProtection-Keys" folder.</span></span> <span data-ttu-id="4bf52-117">此外，如果作業系統為 Windows，它們會在待用期間使用 DPAPI 會加密。</span><span class="sxs-lookup"><span data-stu-id="4bf52-117">Additionally, if the operating system is Windows, they'll be encrypted at rest using DPAPI.</span></span>

3. <span data-ttu-id="4bf52-118">如果應用程式裝載在 IIS 中，會保存 ACLed 只是為了背景工作處理序帳戶的特殊的登錄機碼 HKLM 登錄機碼。</span><span class="sxs-lookup"><span data-stu-id="4bf52-118">If the application is hosted in IIS, keys are persisted to the HKLM registry in a special registry key that is ACLed only to the worker process account.</span></span> <span data-ttu-id="4bf52-119">索引鍵會在待用期間使用 DPAPI 加密。</span><span class="sxs-lookup"><span data-stu-id="4bf52-119">Keys are encrypted at rest using DPAPI.</span></span>

4. <span data-ttu-id="4bf52-120">如果上述條件符合時，索引鍵不會保存在目前的程序之外。</span><span class="sxs-lookup"><span data-stu-id="4bf52-120">If none of these conditions matches, keys are not persisted outside of the current process.</span></span> <span data-ttu-id="4bf52-121">處理程序關閉時，產生所有索引鍵將會遺失。</span><span class="sxs-lookup"><span data-stu-id="4bf52-121">When the process shuts down, all generated keys will be lost.</span></span>

<span data-ttu-id="4bf52-122">開發人員一定是完全控制，而且可以覆寫的方式，以及索引鍵儲存位置。</span><span class="sxs-lookup"><span data-stu-id="4bf52-122">The developer is always in full control and can override how and where keys are stored.</span></span> <span data-ttu-id="4bf52-123">上面的前三個選項都應該很好的預設值對於大部分應用程式類似 ASP.NET<machineKey>自動產生常式過去。</span><span class="sxs-lookup"><span data-stu-id="4bf52-123">The first three options above should good defaults for most applications similar to how the ASP.NET <machineKey> auto-generation routines worked in the past.</span></span> <span data-ttu-id="4bf52-124">最後，控制項會回復選項是真正需要開發人員指定的唯一案例[組態](overview.md)前方如果他們想要的索引鍵的持續性，但只在極少數的情況下發生此後援。</span><span class="sxs-lookup"><span data-stu-id="4bf52-124">The final, fall back option is the only scenario that truly requires the developer to specify [configuration](overview.md) upfront if they want key persistence, but this fall-back would only occur in rare situations.</span></span>

>[!WARNING]
> <span data-ttu-id="4bf52-125">如果開發人員會覆寫這個啟發學習法，並指向特定的金鑰儲存機制的資料保護系統，將會停用自動加密在靜止的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="4bf52-125">If the developer overrides this heuristic and points the data protection system at a specific key repository, automatic encryption of keys at rest will be disabled.</span></span> <span data-ttu-id="4bf52-126">待用保護可以透過重新啟用[組態](overview.md)。</span><span class="sxs-lookup"><span data-stu-id="4bf52-126">At rest protection can be re-enabled via [configuration](overview.md).</span></span>

## <a name="key-lifetime"></a><span data-ttu-id="4bf52-127">金鑰存留期</span><span class="sxs-lookup"><span data-stu-id="4bf52-127">Key Lifetime</span></span>

<span data-ttu-id="4bf52-128">預設的索引鍵具有 90 天的存留期。</span><span class="sxs-lookup"><span data-stu-id="4bf52-128">Keys by default have a 90-day lifetime.</span></span> <span data-ttu-id="4bf52-129">金鑰過期時，系統會自動產生新的金鑰，並將新的金鑰設定為作用中金鑰。</span><span class="sxs-lookup"><span data-stu-id="4bf52-129">When a key expires, the system will automatically generate a new key and set the new key as the active key.</span></span> <span data-ttu-id="4bf52-130">只要已停用的索引鍵會保留在系統上仍然可以解密受保護的任何資料。</span><span class="sxs-lookup"><span data-stu-id="4bf52-130">As long as retired keys remain on the system you will still be able to decrypt any data protected with them.</span></span> <span data-ttu-id="4bf52-131">請參閱[金鑰管理](../implementation/key-management.md#data-protection-implementation-key-management-expiration)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="4bf52-131">See [key management](../implementation/key-management.md#data-protection-implementation-key-management-expiration) for more information.</span></span>

## <a name="default-algorithms"></a><span data-ttu-id="4bf52-132">預設的演算法</span><span class="sxs-lookup"><span data-stu-id="4bf52-132">Default Algorithms</span></span>

<span data-ttu-id="4bf52-133">使用的預設裝載保護演算法為 256-AES-CBC 機密性和 HMACSHA256 的真實性。</span><span class="sxs-lookup"><span data-stu-id="4bf52-133">The default payload protection algorithm used is AES-256-CBC for confidentiality and HMACSHA256 for authenticity.</span></span> <span data-ttu-id="4bf52-134">512 位元主要金鑰，回復每隔 90 天，用來衍生這些每個裝載為基礎的演算法使用兩個子機碼。</span><span class="sxs-lookup"><span data-stu-id="4bf52-134">A 512-bit master key, rolled every 90 days, is used to derive the two sub-keys used for these algorithms on a per-payload basis.</span></span> <span data-ttu-id="4bf52-135">請參閱[子機碼衍生](../implementation/subkeyderivation.md#data-protection-implementation-subkey-derivation-aad)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="4bf52-135">See [subkey derivation](../implementation/subkeyderivation.md#data-protection-implementation-subkey-derivation-aad) for more information.</span></span>
