---
title: 取消保護的裝載中 ASP.NET Core 已撤銷之索引鍵
author: rick-anderson
description: 了解如何取消保護受保護，因為已撤銷，ASP.NET Core 應用程式中的索引鍵的資料。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/dangerous-unprotect
ms.openlocfilehash: ed0c2a309e899f018b09b3edc86b65b299647914
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272382"
---
# <a name="unprotect-payloads-whose-keys-have-been-revoked-in-aspnet-core"></a><span data-ttu-id="7b167-103">取消保護的裝載中 ASP.NET Core 已撤銷之索引鍵</span><span class="sxs-lookup"><span data-stu-id="7b167-103">Unprotect payloads whose keys have been revoked in ASP.NET Core</span></span>


<a name="data-protection-consumer-apis-dangerous-unprotect"></a>

<span data-ttu-id="7b167-104">ASP.NET Core 資料保護 Api 主要被不適用於機密裝載的無限期持續性。</span><span class="sxs-lookup"><span data-stu-id="7b167-104">The ASP.NET Core data protection APIs are not primarily intended for indefinite persistence of confidential payloads.</span></span> <span data-ttu-id="7b167-105">其他技術喜歡[Windows CNG api，DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx)和[Azure Rights Management](https://docs.microsoft.com/rights-management/)更適合的案例是無限期的儲存體，而且必須跟著強式金鑰的管理功能。</span><span class="sxs-lookup"><span data-stu-id="7b167-105">Other technologies like [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) and [Azure Rights Management](https://docs.microsoft.com/rights-management/) are more suited to the scenario of indefinite storage, and they have correspondingly strong key management capabilities.</span></span> <span data-ttu-id="7b167-106">話雖如此，沒有任何開發人員會禁止從 ASP.NET Core 資料保護應用程式開發介面使用的長期保護的機密資料。</span><span class="sxs-lookup"><span data-stu-id="7b167-106">That said, there's nothing prohibiting a developer from using the ASP.NET Core data protection APIs for long-term protection of confidential data.</span></span> <span data-ttu-id="7b167-107">永遠不會移除金鑰從金鑰環，因此`IDataProtector.Unprotect`，只要索引鍵可供使用且有效，則一律可以復原現有的裝載。</span><span class="sxs-lookup"><span data-stu-id="7b167-107">Keys are never removed from the key ring, so `IDataProtector.Unprotect` can always recover existing payloads as long as the keys are available and valid.</span></span>

<span data-ttu-id="7b167-108">不過，發生問題時，開發人員嘗試取消保護已受到保護以已撤銷的索引鍵成為的資料時`IDataProtector.Unprotect`在此情況下將會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="7b167-108">However, an issue arises when the developer tries to unprotect data that has been protected with a revoked key, as `IDataProtector.Unprotect` will throw an exception in this case.</span></span> <span data-ttu-id="7b167-109">裝載這類可以輕鬆地重新建立系統和站台訪客最糟的情況可能需要再次登入，這可能是問題 （例如驗證語彙基元），需短暫存在或暫時性的裝載。</span><span class="sxs-lookup"><span data-stu-id="7b167-109">This might be fine for short-lived or transient payloads (like authentication tokens), as these kinds of payloads can easily be recreated by the system, and at worst the site visitor might be required to log in again.</span></span> <span data-ttu-id="7b167-110">但具有的保存裝載`Unprotect`無法接受資料遺失可能會導致擲回。</span><span class="sxs-lookup"><span data-stu-id="7b167-110">But for persisted payloads, having `Unprotect` throw could lead to unacceptable data loss.</span></span>

## <a name="ipersisteddataprotector"></a><span data-ttu-id="7b167-111">IPersistedDataProtector</span><span class="sxs-lookup"><span data-stu-id="7b167-111">IPersistedDataProtector</span></span>

<span data-ttu-id="7b167-112">若要支援的案例是讓裝載為未受到保護，即使遇到已撤銷的索引鍵時，包含資料保護系統`IPersistedDataProtector`型別。</span><span class="sxs-lookup"><span data-stu-id="7b167-112">To support the scenario of allowing payloads to be unprotected even in the face of revoked keys, the data protection system contains an `IPersistedDataProtector` type.</span></span> <span data-ttu-id="7b167-113">若要取得的執行個體`IPersistedDataProtector`，只會收到的執行個體`IDataProtector`正常方式並再試一次轉換中`IDataProtector`至`IPersistedDataProtector`。</span><span class="sxs-lookup"><span data-stu-id="7b167-113">To get an instance of `IPersistedDataProtector`, simply get an instance of `IDataProtector` in the normal fashion and try casting the `IDataProtector` to `IPersistedDataProtector`.</span></span>

> [!NOTE]
> <span data-ttu-id="7b167-114">並非所有`IDataProtector`執行個體都可以轉換成`IPersistedDataProtector`。</span><span class="sxs-lookup"><span data-stu-id="7b167-114">Not all `IDataProtector` instances can be cast to `IPersistedDataProtector`.</span></span> <span data-ttu-id="7b167-115">開發人員應該使用 C# 運算子或類似以避免執行階段例外狀況起因是無效的轉型，而且應該有準備好要適當地處理失敗的情況。</span><span class="sxs-lookup"><span data-stu-id="7b167-115">Developers should use the C# as operator or similar to avoid runtime exceptions caused by invalid casts, and they should be prepared to handle the failure case appropriately.</span></span>

<span data-ttu-id="7b167-116">`IPersistedDataProtector` 會公開下列 API 介面：</span><span class="sxs-lookup"><span data-stu-id="7b167-116">`IPersistedDataProtector` exposes the following API surface:</span></span>

```csharp
DangerousUnprotect(byte[] protectedData, bool ignoreRevocationErrors,
     out bool requiresMigration, out bool wasRevoked) : byte[]
```

<span data-ttu-id="7b167-117">這個 API 會接受受保護的內容 （以位元組陣列），並傳回未受保護的內容。</span><span class="sxs-lookup"><span data-stu-id="7b167-117">This API takes the protected payload (as a byte array) and returns the unprotected payload.</span></span> <span data-ttu-id="7b167-118">沒有任何字串為基礎的多載。</span><span class="sxs-lookup"><span data-stu-id="7b167-118">There's no string-based overload.</span></span> <span data-ttu-id="7b167-119">這兩個 out 參數如下所示。</span><span class="sxs-lookup"><span data-stu-id="7b167-119">The two out parameters are as follows.</span></span>

* <span data-ttu-id="7b167-120">`requiresMigration`： 將設定為 true，如果用來保護此裝載金鑰不再使用中的預設索引鍵，例如，用來保護此內容的索引鍵是舊的復原作業的索引鍵已自採取的地方。</span><span class="sxs-lookup"><span data-stu-id="7b167-120">`requiresMigration`: will be set to true if the key used to protect this payload is no longer the active default key, e.g., the key used to protect this payload is old and a key rolling operation has since taken place.</span></span> <span data-ttu-id="7b167-121">呼叫端可能要考慮重新保護根據商務需求的裝載。</span><span class="sxs-lookup"><span data-stu-id="7b167-121">The caller may wish to consider reprotecting the payload depending on their business needs.</span></span>

* <span data-ttu-id="7b167-122">`wasRevoked`： 將設為 true，如果用來保護此內容的索引鍵已被撤銷。</span><span class="sxs-lookup"><span data-stu-id="7b167-122">`wasRevoked`: will be set to true if the key used to protect this payload was revoked.</span></span>

>[!WARNING]
> <span data-ttu-id="7b167-123">傳遞時小心`ignoreRevocationErrors: true`至`DangerousUnprotect`方法。</span><span class="sxs-lookup"><span data-stu-id="7b167-123">Exercise extreme caution when passing `ignoreRevocationErrors: true` to the `DangerousUnprotect` method.</span></span> <span data-ttu-id="7b167-124">如果呼叫這個方法後的`wasRevoked`值為 true，則用來保護此內容的索引鍵已被撤銷，並裝載的真實性應該被視為有疑問。</span><span class="sxs-lookup"><span data-stu-id="7b167-124">If after calling this method the `wasRevoked` value is true, then the key used to protect this payload was revoked, and the payload's authenticity should be treated as suspect.</span></span> <span data-ttu-id="7b167-125">在此情況下，只有繼續上未受保護的內容操作，如果您有一些個別的保證，它是真確的例如它來自安全的資料庫，而不是受信任的 web 用戶端所傳送。</span><span class="sxs-lookup"><span data-stu-id="7b167-125">In this case, only continue operating on the unprotected payload if you have some separate assurance that it's authentic, e.g. that it's coming from a secure database rather than being sent by an untrusted web client.</span></span>

[!code-csharp[](dangerous-unprotect/samples/dangerous-unprotect.cs)]
