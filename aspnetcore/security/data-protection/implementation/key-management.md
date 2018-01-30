---
title: "金鑰管理"
author: rick-anderson
description: "本文件概述的 ASP.NET Core 資料保護金鑰管理應用程式開發介面的實作詳細資料。"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/key-management
ms.openlocfilehash: 00f48718ad8b9cc9070b7adc54b26ecd89eb320f
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="key-management"></a><span data-ttu-id="aa8f3-103">金鑰管理</span><span class="sxs-lookup"><span data-stu-id="aa8f3-103">Key Management</span></span>

<a name="data-protection-implementation-key-management"></a>

<span data-ttu-id="aa8f3-104">資料保護系統會自動管理用來保護且取消保護內容的主要金鑰的存留期。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-104">The data protection system automatically manages the lifetime of master keys used to protect and unprotect payloads.</span></span> <span data-ttu-id="aa8f3-105">每個索引鍵可以存在於其中一個四個階段：</span><span class="sxs-lookup"><span data-stu-id="aa8f3-105">Each key can exist in one of four stages:</span></span>

* <span data-ttu-id="aa8f3-106">建立索引鍵存在於鑰匙圈，但尚未啟動。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-106">Created - the key exists in the key ring but has not yet been activated.</span></span> <span data-ttu-id="aa8f3-107">機碼不應該用於新的保護作業有足夠的時間經過之前索引鍵有傳播到所有機器正在耗用此鑰匙圈的機會。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-107">The key shouldn't be used for new Protect operations until sufficient time has elapsed that the key has had a chance to propagate to all machines that are consuming this key ring.</span></span>

* <span data-ttu-id="aa8f3-108">作用中的索引鍵中存在鑰匙圈，適用於所有新的保護作業。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-108">Active - the key exists in the key ring and should be used for all new Protect operations.</span></span>

* <span data-ttu-id="aa8f3-109">過期-索引鍵已執行其自然的存留期，並不會再用於新的保護作業。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-109">Expired - the key has run its natural lifetime and should no longer be used for new Protect operations.</span></span>

* <span data-ttu-id="aa8f3-110">撤銷-金鑰受到危害，且必須不適用於新的保護作業。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-110">Revoked - the key is compromised and must not be used for new Protect operations.</span></span>

<span data-ttu-id="aa8f3-111">建立、 使用中和已過期的索引鍵可能都可以用來取消保護內送裝載。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-111">Created, active, and expired keys may all be used to unprotect incoming payloads.</span></span> <span data-ttu-id="aa8f3-112">依預設已撤銷的索引鍵不能取消保護裝載，但應用程式開發人員可以[覆寫這個行為](../consumer-apis/dangerous-unprotect.md#data-protection-consumer-apis-dangerous-unprotect)如有必要。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-112">Revoked keys by default may not be used to unprotect payloads, but the application developer can [override this behavior](../consumer-apis/dangerous-unprotect.md#data-protection-consumer-apis-dangerous-unprotect) if necessary.</span></span>

>[!WARNING]
> <span data-ttu-id="aa8f3-113">開發人員可能會想要從索引鍵環刪除機碼 （例如，藉由從檔案系統中刪除對應的檔案）。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-113">The developer might be tempted to delete a key from the key ring (e.g., by deleting the corresponding file from the file system).</span></span> <span data-ttu-id="aa8f3-114">此時，金鑰所保護的所有資料都都會永久觸，並都沒有緊急覆寫沒有使用已撤銷的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-114">At that point, all data protected by the key is permanently undecipherable, and there's no emergency override like there's with revoked keys.</span></span> <span data-ttu-id="aa8f3-115">刪除索引鍵是真正破壞性的行為，因此資料保護系統會公開任何第一級的 API 執行此作業。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-115">Deleting a key is truly destructive behavior, and consequently the data protection system exposes no first-class API for performing this operation.</span></span>

## <a name="default-key-selection"></a><span data-ttu-id="aa8f3-116">預設選取項目索引鍵</span><span class="sxs-lookup"><span data-stu-id="aa8f3-116">Default key selection</span></span>

<span data-ttu-id="aa8f3-117">當資料保護系統從備份儲存機制讀取鑰匙圈時，它會嘗試找出鑰匙圈的 「 預設 」 金鑰。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-117">When the data protection system reads the key ring from the backing repository, it will attempt to locate a "default" key from the key ring.</span></span> <span data-ttu-id="aa8f3-118">預設索引鍵用於新的保護作業。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-118">The default key is used for new Protect operations.</span></span>

<span data-ttu-id="aa8f3-119">一般的啟發學習法是資料保護系統選擇做為預設索引鍵的最新的啟動日期索引鍵。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-119">The general heuristic is that the data protection system chooses the key with the most recent activation date as the default key.</span></span> <span data-ttu-id="aa8f3-120">（沒有小型是因素，以允許伺服器對伺服器的時鐘誤差。）如果金鑰已過期或撤銷，以及如果應用程式沒有停用自動金鑰產生，則會產生新的金鑰，以立即啟用每個[金鑰到期以及輪流](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management-expiration)下方的原則。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-120">(There's a small fudge factor to allow for server-to-server clock skew.) If the key is expired or revoked, and if the application has not disabled automatic key generation, then a new key will be generated with immediate activation per the [key expiration and rolling](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management-expiration) policy below.</span></span>

<span data-ttu-id="aa8f3-121">資料保護系統的原因會立即產生新的金鑰，而不是正在回復到不同的索引鍵是新的金鑰產生應視為新的金鑰之前已啟用的所有索引鍵的隱含到期日。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-121">The reason the data protection system generates a new key immediately rather than falling back to a different key is that new key generation should be treated as an implicit expiration of all keys that were activated prior to the new key.</span></span> <span data-ttu-id="aa8f3-122">新的金鑰可能已設定使用不同的演算法或靜態加密機制，與舊的索引鍵，而系統應該而不用的目前組態回到一般概念。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-122">The general idea is that new keys may have been configured with different algorithms or encryption-at-rest mechanisms than old keys, and the system should prefer the current configuration over falling back.</span></span>

<span data-ttu-id="aa8f3-123">沒有例外狀況。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-123">There's an exception.</span></span> <span data-ttu-id="aa8f3-124">如果應用程式開發人員有[停用自動產生金鑰](xref:security/data-protection/configuration/overview#disableautomatickeygeneration)，然後在資料保護系統必須選擇做為預設索引鍵的項目。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-124">If the application developer has [disabled automatic key generation](xref:security/data-protection/configuration/overview#disableautomatickeygeneration), then the data protection system must choose something as the default key.</span></span> <span data-ttu-id="aa8f3-125">在此後援的案例中，系統會選擇非撤銷鍵的最新的啟動日期，加上指定的時間才會傳播到叢集中的其他機器索引鍵為喜好設定。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-125">In this fallback scenario, the system will choose the non-revoked key with the most recent activation date, with preference given to keys that have had time to propagate to other machines in the cluster.</span></span> <span data-ttu-id="aa8f3-126">回溯系統可能會因此選擇過期的預設索引鍵。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-126">The fallback system may end up choosing an expired default key as a result.</span></span> <span data-ttu-id="aa8f3-127">回溯系統將會永遠不會選擇已撤銷的索引鍵，做為預設索引鍵，如果鑰匙圈是空的或每個索引鍵已被撤銷。 然後系統會產生錯誤時初始化。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-127">The fallback system will never choose a revoked key as the default key, and if the key ring is empty or every key has been revoked then the system will produce an error upon initialization.</span></span>

<a name="data-protection-implementation-key-management-expiration"></a>

## <a name="key-expiration-and-rolling"></a><span data-ttu-id="aa8f3-128">金鑰到期時間和循環</span><span class="sxs-lookup"><span data-stu-id="aa8f3-128">Key expiration and rolling</span></span>

<span data-ttu-id="aa8f3-129">建立金鑰時，它會自動具 {now + 2 天} 啟用日和到期日的 {now + 90 天}。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-129">When a key is created, it's automatically given an activation date of { now + 2 days } and an expiration date of { now + 90 days }.</span></span> <span data-ttu-id="aa8f3-130">2 天之前的延遲啟動提供的索引鍵的時間才能傳播到整個系統。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-130">The 2-day delay before activation gives the key time to propagate through the system.</span></span> <span data-ttu-id="aa8f3-131">也就是說，它可讓其他應用程式指向備份存放區來觀察在其下一步 自動重新整理期間，索引鍵，因此最大化，金鑰環變成沒有作用中傳播到它時可能需要的所有應用程式使用它的機會。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-131">That is, it allows other applications pointing at the backing store to observe the key at their next auto-refresh period, thus maximizing the chances that when the key ring does become active it has propagated to all applications that might need to use it.</span></span>

<span data-ttu-id="aa8f3-132">如果預設金鑰將在 2 天內過期，而鑰匙圈還沒有將會啟用在到期時的預設索引鍵的索引鍵，資料保護系統將會自動保存鑰匙圈新的金鑰。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-132">If the default key will expire within 2 days and if the key ring doesn't already have a key that will be active upon expiration of the default key, then the data protection system will automatically persist a new key to the key ring.</span></span> <span data-ttu-id="aa8f3-133">這個新的金鑰已啟用的日期 {預設金鑰到期日} 和 {now + 90 天} 的到期日。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-133">This new key has an activation date of { default key's expiration date } and an expiration date of { now + 90 days }.</span></span> <span data-ttu-id="aa8f3-134">這可讓系統自動復原不中斷服務的定期的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-134">This allows the system to automatically roll keys on a regular basis with no interruption of service.</span></span>

<span data-ttu-id="aa8f3-135">可能的情況下會立即啟動建立金鑰。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-135">There might be circumstances where a key will be created with immediate activation.</span></span> <span data-ttu-id="aa8f3-136">當應用程式尚未執行的時間，而鑰匙圈中的所有索引鍵已過期時，就是一個範例。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-136">One example would be when the application hasn't run for a time and all keys in the key ring are expired.</span></span> <span data-ttu-id="aa8f3-137">發生這種情況，索引鍵有 {現在} 沒有一般 2 天的啟用延遲啟動日期。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-137">When this happens, the key is given an activation date of { now } without the normal 2-day activation delay.</span></span>

<span data-ttu-id="aa8f3-138">預設金鑰存留時間為 90 天，但這是可設定，如下列範例所示。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-138">The default key lifetime is 90 days, though this is configurable as in the following example.</span></span>

```csharp
services.AddDataProtection()
       // use 14-day lifetime instead of 90-day lifetime
       .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
```

<span data-ttu-id="aa8f3-139">雖然明確呼叫，以系統管理員也可以變更預設全系統`SetDefaultKeyLifetime`將會覆寫任何全系統原則。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-139">An administrator can also change the default system-wide, though an explicit call to `SetDefaultKeyLifetime` will override any system-wide policy.</span></span> <span data-ttu-id="aa8f3-140">預設金鑰存留期不可短於 7 天。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-140">The default key lifetime cannot be shorter than 7 days.</span></span>

## <a name="automatic-key-ring-refresh"></a><span data-ttu-id="aa8f3-141">自動鑰匙圈重新整理</span><span class="sxs-lookup"><span data-stu-id="aa8f3-141">Automatic key ring refresh</span></span>

<span data-ttu-id="aa8f3-142">當資料保護系統初始化時，它會鑰匙圈讀取從基礎儲存機制，並放入快取在記憶體中。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-142">When the data protection system initializes, it reads the key ring from the underlying repository and caches it in memory.</span></span> <span data-ttu-id="aa8f3-143">此快取可讓您保護且取消保護的作業繼續進行而不叫用的備份存放區。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-143">This cache allows Protect and Unprotect operations to proceed without hitting the backing store.</span></span> <span data-ttu-id="aa8f3-144">大約每隔 24 小時或目前的預設金鑰過期時，何者較早，系統將自動檢查有變更的備份存放區。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-144">The system will automatically check the backing store for changes approximately every 24 hours or when the current default key expires, whichever comes first.</span></span>

>[!WARNING]
> <span data-ttu-id="aa8f3-145">（如果有的話） 開發人員應該很少需要直接使用 Api 的金鑰管理。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-145">Developers should very rarely (if ever) need to use the key management APIs directly.</span></span> <span data-ttu-id="aa8f3-146">資料保護系統將會執行自動金鑰管理，如上面所述。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-146">The data protection system will perform automatic key management as described above.</span></span>

<span data-ttu-id="aa8f3-147">資料保護系統公開介面`IKeyManager`，可用來檢查及變更索引鍵環形。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-147">The data protection system exposes an interface `IKeyManager` that can be used to inspect and make changes to the key ring.</span></span> <span data-ttu-id="aa8f3-148">DI 系統提供的執行個體`IDataProtectionProvider`也可以提供的執行個體`IKeyManager`供您使用。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-148">The DI system that provided the instance of `IDataProtectionProvider` can also provide an instance of `IKeyManager` for your consumption.</span></span> <span data-ttu-id="aa8f3-149">或者，您可以拉`IKeyManager`直接從`IServiceProvider`如下列範例所示。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-149">Alternatively, you can pull the `IKeyManager` straight from the `IServiceProvider` as in the example below.</span></span>

<span data-ttu-id="aa8f3-150">任何可修改鑰匙圈 （明確地建立新的金鑰，或執行撤銷） 作業會使記憶體中快取失效。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-150">Any operation which modifies the key ring (creating a new key explicitly or performing a revocation) will invalidate the in-memory cache.</span></span> <span data-ttu-id="aa8f3-151">下次呼叫`Protect`或`Unprotect`會導致重新讀取鑰匙圈，並重新建立快取的資料保護系統。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-151">The next call to `Protect` or `Unprotect` will cause the data protection system to reread the key ring and recreate the cache.</span></span>

<span data-ttu-id="aa8f3-152">下列範例將示範如何使用`IKeyManager`介面來檢查及操作鑰匙圈，包括撤銷，則現有的索引鍵，並手動產生新的金鑰。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-152">The sample below demonstrates using the `IKeyManager` interface to inspect and manipulate the key ring, including revoking existing keys and generating a new key manually.</span></span>

[!code-csharp[Main](key-management/samples/key-management.cs)]

## <a name="key-storage"></a><span data-ttu-id="aa8f3-153">金鑰儲存</span><span class="sxs-lookup"><span data-stu-id="aa8f3-153">Key storage</span></span>

<span data-ttu-id="aa8f3-154">資料保護系統有啟發學習法，它會嘗試自動推算適當金鑰的儲存位置和 rest 機制加密。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-154">The data protection system has a heuristic whereby it tries to deduce an appropriate key storage location and encryption at rest mechanism automatically.</span></span> <span data-ttu-id="aa8f3-155">這也是由應用程式開發人員可設定。</span><span class="sxs-lookup"><span data-stu-id="aa8f3-155">This is also configurable by the app developer.</span></span> <span data-ttu-id="aa8f3-156">下列文件將討論這些機制的內建實作：</span><span class="sxs-lookup"><span data-stu-id="aa8f3-156">The following documents discuss the in-box implementations of these mechanisms:</span></span>

* [<span data-ttu-id="aa8f3-157">內建金鑰儲存提供者</span><span class="sxs-lookup"><span data-stu-id="aa8f3-157">In-box key storage providers</span></span>](key-storage-providers.md#data-protection-implementation-key-storage-providers)

* [<span data-ttu-id="aa8f3-158">在其他提供者的內建金鑰加密</span><span class="sxs-lookup"><span data-stu-id="aa8f3-158">In-box key encryption at rest providers</span></span>](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest-providers)
