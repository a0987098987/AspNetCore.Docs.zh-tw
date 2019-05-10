---
title: 取消保護的金鑰已遭撤銷，ASP.NET Core 中裝載
author: rick-anderson
description: 了解如何取消保護受保護之後已撤銷，ASP.NET Core 應用程式中的索引鍵的資料。
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: security/data-protection/consumer-apis/dangerous-unprotect
ms.openlocfilehash: 26061d048dcd9c1e3d8909e9388d8b565376fa2f
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897105"
---
# <a name="unprotect-payloads-whose-keys-have-been-revoked-in-aspnet-core"></a><span data-ttu-id="603bc-103">取消保護的金鑰已遭撤銷，ASP.NET Core 中裝載</span><span class="sxs-lookup"><span data-stu-id="603bc-103">Unprotect payloads whose keys have been revoked in ASP.NET Core</span></span>

<a name="data-protection-consumer-apis-dangerous-unprotect"></a>

<span data-ttu-id="603bc-104">無限期的持續性的機密承載不主要是 ASP.NET Core 資料保護 Api。</span><span class="sxs-lookup"><span data-stu-id="603bc-104">The ASP.NET Core data protection APIs are not primarily intended for indefinite persistence of confidential payloads.</span></span> <span data-ttu-id="603bc-105">等其他技術[Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx)並[Azure Rights Management](/rights-management/)更適合的案例是無限制的儲存體，而且必須跟著強式金鑰管理功能。</span><span class="sxs-lookup"><span data-stu-id="603bc-105">Other technologies like [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) and [Azure Rights Management](/rights-management/) are more suited to the scenario of indefinite storage, and they have correspondingly strong key management capabilities.</span></span> <span data-ttu-id="603bc-106">話雖如此，沒有任何禁止開發人員使用 ASP.NET Core 資料保護 Api 進行長期保護的機密資料。</span><span class="sxs-lookup"><span data-stu-id="603bc-106">That said, there's nothing prohibiting a developer from using the ASP.NET Core data protection APIs for long-term protection of confidential data.</span></span> <span data-ttu-id="603bc-107">永遠不會移除金鑰從金鑰環，因此`IDataProtector.Unprotect`，只要索引鍵可供使用且有效，則一律可以復原現有的裝載。</span><span class="sxs-lookup"><span data-stu-id="603bc-107">Keys are never removed from the key ring, so `IDataProtector.Unprotect` can always recover existing payloads as long as the keys are available and valid.</span></span>

<span data-ttu-id="603bc-108">不過，就會發生問題而開發人員嘗試解除已為保護以已撤銷的索引鍵的資料時`IDataProtector.Unprotect`在此情況下將會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="603bc-108">However, an issue arises when the developer tries to unprotect data that has been protected with a revoked key, as `IDataProtector.Unprotect` will throw an exception in this case.</span></span> <span data-ttu-id="603bc-109">這類的承載可以輕鬆重新建立系統，以及最糟的情況，網站訪客可能必須再次登入，這可能是適合用於短期或非暫時性的承載 （例如驗證權杖）。</span><span class="sxs-lookup"><span data-stu-id="603bc-109">This might be fine for short-lived or transient payloads (like authentication tokens), as these kinds of payloads can easily be recreated by the system, and at worst the site visitor might be required to log in again.</span></span> <span data-ttu-id="603bc-110">保存的承載，有的但`Unprotect`throw 可能會導致無法接受資料遺失。</span><span class="sxs-lookup"><span data-stu-id="603bc-110">But for persisted payloads, having `Unprotect` throw could lead to unacceptable data loss.</span></span>

## <a name="ipersisteddataprotector"></a><span data-ttu-id="603bc-111">IPersistedDataProtector</span><span class="sxs-lookup"><span data-stu-id="603bc-111">IPersistedDataProtector</span></span>

<span data-ttu-id="603bc-112">若要支援的案例是讓裝載為未受到保護，即使已撤銷的索引鍵，包含資料保護系統`IPersistedDataProtector`型別。</span><span class="sxs-lookup"><span data-stu-id="603bc-112">To support the scenario of allowing payloads to be unprotected even in the face of revoked keys, the data protection system contains an `IPersistedDataProtector` type.</span></span> <span data-ttu-id="603bc-113">若要取得的執行個體`IPersistedDataProtector`，直接取得的執行個體`IDataProtector`在正常的方式，請嘗試轉型`IDataProtector`至`IPersistedDataProtector`。</span><span class="sxs-lookup"><span data-stu-id="603bc-113">To get an instance of `IPersistedDataProtector`, simply get an instance of `IDataProtector` in the normal fashion and try casting the `IDataProtector` to `IPersistedDataProtector`.</span></span>

> [!NOTE]
> <span data-ttu-id="603bc-114">並非所有`IDataProtector`執行個體可以轉換成`IPersistedDataProtector`。</span><span class="sxs-lookup"><span data-stu-id="603bc-114">Not all `IDataProtector` instances can be cast to `IPersistedDataProtector`.</span></span> <span data-ttu-id="603bc-115">開發人員應該使用C#運算子或類似以避免執行階段例外狀況造成無效的轉型，而且應該有準備好適當地處理失敗的情況。</span><span class="sxs-lookup"><span data-stu-id="603bc-115">Developers should use the C# as operator or similar to avoid runtime exceptions caused by invalid casts, and they should be prepared to handle the failure case appropriately.</span></span>

<span data-ttu-id="603bc-116">`IPersistedDataProtector` 會公開下列的 API 介面：</span><span class="sxs-lookup"><span data-stu-id="603bc-116">`IPersistedDataProtector` exposes the following API surface:</span></span>

```csharp
DangerousUnprotect(byte[] protectedData, bool ignoreRevocationErrors,
     out bool requiresMigration, out bool wasRevoked) : byte[]
```

<span data-ttu-id="603bc-117">此 API 會採用受保護的內容 （以位元組陣列），並傳回未受保護的內容。</span><span class="sxs-lookup"><span data-stu-id="603bc-117">This API takes the protected payload (as a byte array) and returns the unprotected payload.</span></span> <span data-ttu-id="603bc-118">沒有任何字串為基礎的多載。</span><span class="sxs-lookup"><span data-stu-id="603bc-118">There's no string-based overload.</span></span> <span data-ttu-id="603bc-119">這兩個 out 參數如下所示。</span><span class="sxs-lookup"><span data-stu-id="603bc-119">The two out parameters are as follows.</span></span>

* <span data-ttu-id="603bc-120">`requiresMigration`： 會設定為 true，如果用來保護此承載的金鑰不再使用中的預設索引鍵，例如，用來保護此承載的金鑰是舊金鑰輪換作業以來進行的地方。</span><span class="sxs-lookup"><span data-stu-id="603bc-120">`requiresMigration`: will be set to true if the key used to protect this payload is no longer the active default key, e.g., the key used to protect this payload is old and a key rolling operation has since taken place.</span></span> <span data-ttu-id="603bc-121">呼叫端可能要考慮重新保護的承載，根據其商務需求而定。</span><span class="sxs-lookup"><span data-stu-id="603bc-121">The caller may wish to consider reprotecting the payload depending on their business needs.</span></span>

* <span data-ttu-id="603bc-122">`wasRevoked`： 將會設為 true，如果用來保護此承載的索引鍵已撤銷。</span><span class="sxs-lookup"><span data-stu-id="603bc-122">`wasRevoked`: will be set to true if the key used to protect this payload was revoked.</span></span>

>[!WARNING]
> <span data-ttu-id="603bc-123">特別小心謹慎時傳遞`ignoreRevocationErrors: true`至`DangerousUnprotect`方法。</span><span class="sxs-lookup"><span data-stu-id="603bc-123">Exercise extreme caution when passing `ignoreRevocationErrors: true` to the `DangerousUnprotect` method.</span></span> <span data-ttu-id="603bc-124">如果呼叫這個方法之後`wasRevoked`值為 true，則用來保護此承載的索引鍵已撤銷，且承載的真確性應該被視為有疑問。</span><span class="sxs-lookup"><span data-stu-id="603bc-124">If after calling this method the `wasRevoked` value is true, then the key used to protect this payload was revoked, and the payload's authenticity should be treated as suspect.</span></span> <span data-ttu-id="603bc-125">在此情況下，只有繼續操作未受保護的內容，如果您有一些不同的保證，它的真實性，例如它來自安全的資料庫，而不是受信任的 web 用戶端所傳送。</span><span class="sxs-lookup"><span data-stu-id="603bc-125">In this case, only continue operating on the unprotected payload if you have some separate assurance that it's authentic, e.g. that it's coming from a secure database rather than being sent by an untrusted web client.</span></span>

[!code-csharp[](dangerous-unprotect/samples/dangerous-unprotect.cs)]
