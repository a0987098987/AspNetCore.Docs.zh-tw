---
title: ASP.NET Core 中的金鑰管理
author: rick-anderson
description: 瞭解 ASP.NET Core 資料保護金鑰管理 Api 的執行細節。
ms.author: riande
ms.date: 10/14/2016
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/data-protection/implementation/key-management
ms.openlocfilehash: 0f54ceedf3558fb4fa2349903461fdfdb4dc02e6
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82776899"
---
# <a name="key-management-in-aspnet-core"></a><span data-ttu-id="277bc-103">ASP.NET Core 中的金鑰管理</span><span class="sxs-lookup"><span data-stu-id="277bc-103">Key management in ASP.NET Core</span></span>

<a name="data-protection-implementation-key-management"></a>

<span data-ttu-id="277bc-104">資料保護系統會自動管理主要金鑰存留期，用來保護和解除保護承載。</span><span class="sxs-lookup"><span data-stu-id="277bc-104">The data protection system automatically manages the lifetime of master keys used to protect and unprotect payloads.</span></span> <span data-ttu-id="277bc-105">每個金鑰都可以存在四個階段的其中一個：</span><span class="sxs-lookup"><span data-stu-id="277bc-105">Each key can exist in one of four stages:</span></span>

* <span data-ttu-id="277bc-106">已建立-金鑰已存在於金鑰環中，但尚未啟用。</span><span class="sxs-lookup"><span data-stu-id="277bc-106">Created - the key exists in the key ring but has not yet been activated.</span></span> <span data-ttu-id="277bc-107">金鑰不應用於新的保護作業，直到有足夠的時間可將金鑰傳播到取用此金鑰信號的所有電腦為止。</span><span class="sxs-lookup"><span data-stu-id="277bc-107">The key shouldn't be used for new Protect operations until sufficient time has elapsed that the key has had a chance to propagate to all machines that are consuming this key ring.</span></span>

* <span data-ttu-id="277bc-108">Active-金鑰會出現在金鑰環中，而且應該用於所有新的保護作業。</span><span class="sxs-lookup"><span data-stu-id="277bc-108">Active - the key exists in the key ring and should be used for all new Protect operations.</span></span>

* <span data-ttu-id="277bc-109">已過期-金鑰已執行其自然存留期，不應再用於新的保護作業。</span><span class="sxs-lookup"><span data-stu-id="277bc-109">Expired - the key has run its natural lifetime and should no longer be used for new Protect operations.</span></span>

* <span data-ttu-id="277bc-110">已撤銷-金鑰遭到入侵，不得用於新的保護作業。</span><span class="sxs-lookup"><span data-stu-id="277bc-110">Revoked - the key is compromised and must not be used for new Protect operations.</span></span>

<span data-ttu-id="277bc-111">[已建立]、[作用中] 和 [已過期] 金鑰可用來解除保護傳入的裝載。</span><span class="sxs-lookup"><span data-stu-id="277bc-111">Created, active, and expired keys may all be used to unprotect incoming payloads.</span></span> <span data-ttu-id="277bc-112">根據預設，已撤銷的金鑰可能不會用來取消保護裝載，但應用程式開發人員可以視需要覆[寫此行為](xref:security/data-protection/consumer-apis/dangerous-unprotect#data-protection-consumer-apis-dangerous-unprotect)。</span><span class="sxs-lookup"><span data-stu-id="277bc-112">Revoked keys by default may not be used to unprotect payloads, but the application developer can [override this behavior](xref:security/data-protection/consumer-apis/dangerous-unprotect#data-protection-consumer-apis-dangerous-unprotect) if necessary.</span></span>

>[!WARNING]
> <span data-ttu-id="277bc-113">開發人員可能會想要從金鑰環刪除金鑰（例如，從檔案系統中刪除對應的檔案）。</span><span class="sxs-lookup"><span data-stu-id="277bc-113">The developer might be tempted to delete a key from the key ring (e.g., by deleting the corresponding file from the file system).</span></span> <span data-ttu-id="277bc-114">此時，金鑰所保護的所有資料都會永久 undecipherable，而且不會有像撤銷金鑰一樣的緊急覆寫。</span><span class="sxs-lookup"><span data-stu-id="277bc-114">At that point, all data protected by the key is permanently undecipherable, and there's no emergency override like there's with revoked keys.</span></span> <span data-ttu-id="277bc-115">刪除金鑰是真正的破壞性行為，因此資料保護系統不會公開可執行這項作業的第一類 API。</span><span class="sxs-lookup"><span data-stu-id="277bc-115">Deleting a key is truly destructive behavior, and consequently the data protection system exposes no first-class API for performing this operation.</span></span>

## <a name="default-key-selection"></a><span data-ttu-id="277bc-116">預設金鑰選取專案</span><span class="sxs-lookup"><span data-stu-id="277bc-116">Default key selection</span></span>

<span data-ttu-id="277bc-117">當資料保護系統從支援存放庫讀取金鑰信號時，它會嘗試從金鑰環形找出「預設」金鑰。</span><span class="sxs-lookup"><span data-stu-id="277bc-117">When the data protection system reads the key ring from the backing repository, it will attempt to locate a "default" key from the key ring.</span></span> <span data-ttu-id="277bc-118">預設金鑰會用於新的保護作業。</span><span class="sxs-lookup"><span data-stu-id="277bc-118">The default key is used for new Protect operations.</span></span>

<span data-ttu-id="277bc-119">一般啟發學習法是資料保護系統會選擇具有最新啟用日期的金鑰做為預設金鑰。</span><span class="sxs-lookup"><span data-stu-id="277bc-119">The general heuristic is that the data protection system chooses the key with the most recent activation date as the default key.</span></span> <span data-ttu-id="277bc-120">（有一個小型的巧克力因素可允許伺服器對伺服器的時鐘誤差）。如果金鑰已過期或已撤銷，而且應用程式未停用自動產生金鑰，則會產生新的金鑰，並根據以下的[金鑰到期和](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management-expiration)輪流原則立即啟用。</span><span class="sxs-lookup"><span data-stu-id="277bc-120">(There's a small fudge factor to allow for server-to-server clock skew.) If the key is expired or revoked, and if the application has not disabled automatic key generation, then a new key will be generated with immediate activation per the [key expiration and rolling](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management-expiration) policy below.</span></span>

<span data-ttu-id="277bc-121">資料保護系統會立即產生新的金鑰而不是回到另一個金鑰的原因是，應該將新的金鑰產生視為在新金鑰之前啟動之所有金鑰的隱含到期。</span><span class="sxs-lookup"><span data-stu-id="277bc-121">The reason the data protection system generates a new key immediately rather than falling back to a different key is that new key generation should be treated as an implicit expiration of all keys that were activated prior to the new key.</span></span> <span data-ttu-id="277bc-122">一般的概念是，新的金鑰可能已設定為使用不同的演算法或待用加密機制，而不是舊的金鑰，而且系統應該偏好目前的設定，而不是回復。</span><span class="sxs-lookup"><span data-stu-id="277bc-122">The general idea is that new keys may have been configured with different algorithms or encryption-at-rest mechanisms than old keys, and the system should prefer the current configuration over falling back.</span></span>

<span data-ttu-id="277bc-123">發生例外狀況。</span><span class="sxs-lookup"><span data-stu-id="277bc-123">There's an exception.</span></span> <span data-ttu-id="277bc-124">如果應用程式開發人員已[停用自動產生金鑰](xref:security/data-protection/configuration/overview#disableautomatickeygeneration)，則資料保護系統必須選擇做為預設金鑰。</span><span class="sxs-lookup"><span data-stu-id="277bc-124">If the application developer has [disabled automatic key generation](xref:security/data-protection/configuration/overview#disableautomatickeygeneration), then the data protection system must choose something as the default key.</span></span> <span data-ttu-id="277bc-125">在此回溯案例中，系統會選擇具有最新啟動日期的非撤銷金鑰，並將其喜好設定指定給已有時間傳播到叢集中其他電腦的金鑰。</span><span class="sxs-lookup"><span data-stu-id="277bc-125">In this fallback scenario, the system will choose the non-revoked key with the most recent activation date, with preference given to keys that have had time to propagate to other machines in the cluster.</span></span> <span data-ttu-id="277bc-126">回退系統最後可能會選擇已過期的預設金鑰做為結果。</span><span class="sxs-lookup"><span data-stu-id="277bc-126">The fallback system may end up choosing an expired default key as a result.</span></span> <span data-ttu-id="277bc-127">Fallback 系統永遠不會選擇撤銷的金鑰做為預設金鑰，而且如果金鑰環是空的或每個金鑰都已被撤銷，則系統會在初始化時產生錯誤。</span><span class="sxs-lookup"><span data-stu-id="277bc-127">The fallback system will never choose a revoked key as the default key, and if the key ring is empty or every key has been revoked then the system will produce an error upon initialization.</span></span>

<a name="data-protection-implementation-key-management-expiration"></a>

## <a name="key-expiration-and-rolling"></a><span data-ttu-id="277bc-128">金鑰到期和輪流</span><span class="sxs-lookup"><span data-stu-id="277bc-128">Key expiration and rolling</span></span>

<span data-ttu-id="277bc-129">建立金鑰時，會自動將啟用日期指定為 {now + 2 天}，到期日為 {now + 90 天}。</span><span class="sxs-lookup"><span data-stu-id="277bc-129">When a key is created, it's automatically given an activation date of { now + 2 days } and an expiration date of { now + 90 days }.</span></span> <span data-ttu-id="277bc-130">啟用前的2天延遲會讓金鑰時間能夠透過系統傳播。</span><span class="sxs-lookup"><span data-stu-id="277bc-130">The 2-day delay before activation gives the key time to propagate through the system.</span></span> <span data-ttu-id="277bc-131">也就是說，它可讓指向備份存放區的其他應用程式在下一次自動重新整理期間觀察金鑰，藉此充分發揮金鑰信號變成作用中的機會，並將其傳播到可能需要使用它的所有應用程式。</span><span class="sxs-lookup"><span data-stu-id="277bc-131">That is, it allows other applications pointing at the backing store to observe the key at their next auto-refresh period, thus maximizing the chances that when the key ring does become active it has propagated to all applications that might need to use it.</span></span>

<span data-ttu-id="277bc-132">如果預設金鑰會在2天內過期，而且如果金鑰環形尚未有在預設金鑰到期時將會作用的金鑰，則資料保護系統會自動將新的金鑰保存到金鑰環。</span><span class="sxs-lookup"><span data-stu-id="277bc-132">If the default key will expire within 2 days and if the key ring doesn't already have a key that will be active upon expiration of the default key, then the data protection system will automatically persist a new key to the key ring.</span></span> <span data-ttu-id="277bc-133">此新金鑰的啟用日期為 {預設金鑰的到期日}，到期日為 {now + 90 天}。</span><span class="sxs-lookup"><span data-stu-id="277bc-133">This new key has an activation date of { default key's expiration date } and an expiration date of { now + 90 days }.</span></span> <span data-ttu-id="277bc-134">這可讓系統定期自動定期輪替金鑰，而不會中斷服務。</span><span class="sxs-lookup"><span data-stu-id="277bc-134">This allows the system to automatically roll keys on a regular basis with no interruption of service.</span></span>

<span data-ttu-id="277bc-135">在某些情況下，將會建立具有立即啟用的金鑰。</span><span class="sxs-lookup"><span data-stu-id="277bc-135">There might be circumstances where a key will be created with immediate activation.</span></span> <span data-ttu-id="277bc-136">其中一個範例是應用程式未執行一段時間，且金鑰通道中的所有金鑰已過期。</span><span class="sxs-lookup"><span data-stu-id="277bc-136">One example would be when the application hasn't run for a time and all keys in the key ring are expired.</span></span> <span data-ttu-id="277bc-137">發生這種情況時，系統會將啟用日期指定為 {now}，但不含正常的2天啟用延遲。</span><span class="sxs-lookup"><span data-stu-id="277bc-137">When this happens, the key is given an activation date of { now } without the normal 2-day activation delay.</span></span>

<span data-ttu-id="277bc-138">預設的金鑰存留期為90天，但可設定為，如下列範例所示。</span><span class="sxs-lookup"><span data-stu-id="277bc-138">The default key lifetime is 90 days, though this is configurable as in the following example.</span></span>

```csharp
services.AddDataProtection()
       // use 14-day lifetime instead of 90-day lifetime
       .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
```

<span data-ttu-id="277bc-139">管理員也可以變更全系統的預設值，不過明確呼叫`SetDefaultKeyLifetime`將會覆寫任何全系統原則。</span><span class="sxs-lookup"><span data-stu-id="277bc-139">An administrator can also change the default system-wide, though an explicit call to `SetDefaultKeyLifetime` will override any system-wide policy.</span></span> <span data-ttu-id="277bc-140">預設金鑰存留期不得少於7天。</span><span class="sxs-lookup"><span data-stu-id="277bc-140">The default key lifetime cannot be shorter than 7 days.</span></span>

## <a name="automatic-key-ring-refresh"></a><span data-ttu-id="277bc-141">自動金鑰迴圈重新整理</span><span class="sxs-lookup"><span data-stu-id="277bc-141">Automatic key ring refresh</span></span>

<span data-ttu-id="277bc-142">當資料保護系統初始化時，它會從基礎存放庫讀取金鑰信號，並將它快取到記憶體中。</span><span class="sxs-lookup"><span data-stu-id="277bc-142">When the data protection system initializes, it reads the key ring from the underlying repository and caches it in memory.</span></span> <span data-ttu-id="277bc-143">此快取可讓您進行保護和取消保護作業，而不需要叫用備份存放區。</span><span class="sxs-lookup"><span data-stu-id="277bc-143">This cache allows Protect and Unprotect operations to proceed without hitting the backing store.</span></span> <span data-ttu-id="277bc-144">系統每隔24小時會自動檢查備份存放區是否有變更，或目前的預設金鑰過期時（以先發生者為准）。</span><span class="sxs-lookup"><span data-stu-id="277bc-144">The system will automatically check the backing store for changes approximately every 24 hours or when the current default key expires, whichever comes first.</span></span>

>[!WARNING]
> <span data-ttu-id="277bc-145">開發人員應該很少（如果有的話）需要直接使用金鑰管理 Api。</span><span class="sxs-lookup"><span data-stu-id="277bc-145">Developers should very rarely (if ever) need to use the key management APIs directly.</span></span> <span data-ttu-id="277bc-146">資料保護系統會如上述所述執行自動金鑰管理。</span><span class="sxs-lookup"><span data-stu-id="277bc-146">The data protection system will perform automatic key management as described above.</span></span>

<span data-ttu-id="277bc-147">資料保護系統會公開介面`IKeyManager` ，可用來檢查並變更金鑰環。</span><span class="sxs-lookup"><span data-stu-id="277bc-147">The data protection system exposes an interface `IKeyManager` that can be used to inspect and make changes to the key ring.</span></span> <span data-ttu-id="277bc-148">提供實例的 DI 系統也`IDataProtectionProvider`可以`IKeyManager`為您的取用提供的實例。</span><span class="sxs-lookup"><span data-stu-id="277bc-148">The DI system that provided the instance of `IDataProtectionProvider` can also provide an instance of `IKeyManager` for your consumption.</span></span> <span data-ttu-id="277bc-149">或者，您可以`IKeyManager`直接從提取， `IServiceProvider`如下列範例所示。</span><span class="sxs-lookup"><span data-stu-id="277bc-149">Alternatively, you can pull the `IKeyManager` straight from the `IServiceProvider` as in the example below.</span></span>

<span data-ttu-id="277bc-150">修改金鑰通道（明確建立新的金鑰或執行撤銷）的任何作業，都會使記憶體中的快取失效。</span><span class="sxs-lookup"><span data-stu-id="277bc-150">Any operation which modifies the key ring (creating a new key explicitly or performing a revocation) will invalidate the in-memory cache.</span></span> <span data-ttu-id="277bc-151">下一次呼叫`Protect`或`Unprotect`時，將會導致資料保護系統重新讀取金鑰信號，並重建快取。</span><span class="sxs-lookup"><span data-stu-id="277bc-151">The next call to `Protect` or `Unprotect` will cause the data protection system to reread the key ring and recreate the cache.</span></span>

<span data-ttu-id="277bc-152">下列範例示範如何使用`IKeyManager`介面來檢查和操作金鑰信號，包括撤銷現有的金鑰，以及手動產生新的金鑰。</span><span class="sxs-lookup"><span data-stu-id="277bc-152">The sample below demonstrates using the `IKeyManager` interface to inspect and manipulate the key ring, including revoking existing keys and generating a new key manually.</span></span>

[!code-csharp[](key-management/samples/key-management.cs)]

[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

## <a name="key-storage"></a><span data-ttu-id="277bc-153">金鑰儲存</span><span class="sxs-lookup"><span data-stu-id="277bc-153">Key storage</span></span>

<span data-ttu-id="277bc-154">資料保護系統有一個啟發學習法，它會嘗試自動推算適當的金鑰儲存位置和待用加密機制。</span><span class="sxs-lookup"><span data-stu-id="277bc-154">The data protection system has a heuristic whereby it attempts to deduce an appropriate key storage location and encryption-at-rest mechanism automatically.</span></span> <span data-ttu-id="277bc-155">金鑰持續性機制也可由應用程式開發人員設定。</span><span class="sxs-lookup"><span data-stu-id="277bc-155">The key persistence mechanism is also configurable by the app developer.</span></span> <span data-ttu-id="277bc-156">下列檔將討論這些機制的內建的內部部署：</span><span class="sxs-lookup"><span data-stu-id="277bc-156">The following documents discuss the in-box implementations of these mechanisms:</span></span>

* <xref:security/data-protection/implementation/key-storage-providers>
* <xref:security/data-protection/implementation/key-encryption-at-rest>
