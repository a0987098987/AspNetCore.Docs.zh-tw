---
title: "金鑰的不變性和變更設定"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: fc911ae3-feca-4ffe-8b43-362bc632fe04
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 3afce8f84ebe3b709ea169c7db27f99829f157ab
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2017
---
# <a name="key-immutability-and-changing-settings"></a><span data-ttu-id="256e6-103">索引鍵不變性和變更設定</span><span class="sxs-lookup"><span data-stu-id="256e6-103">Key Immutability and changing settings</span></span>

<span data-ttu-id="256e6-104">一旦物件會保存到備份存放區，其表示是永久固定的。</span><span class="sxs-lookup"><span data-stu-id="256e6-104">Once an object is persisted to the backing store, its representation is forever fixed.</span></span> <span data-ttu-id="256e6-105">備份存放區中，可以加入新的資料，但是可以永遠不會變更現有的資料。</span><span class="sxs-lookup"><span data-stu-id="256e6-105">New data can be added to the backing store, but existing data can never be mutated.</span></span> <span data-ttu-id="256e6-106">此行為的主要用途是防止資料損毀。</span><span class="sxs-lookup"><span data-stu-id="256e6-106">The primary purpose of this behavior is to prevent data corruption.</span></span>

<span data-ttu-id="256e6-107">此行為的一種結果是一旦金鑰會寫入備份存放區，即不可變。</span><span class="sxs-lookup"><span data-stu-id="256e6-107">One consequence of this behavior is that once a key is written to the backing store, it is immutable.</span></span> <span data-ttu-id="256e6-108">絕不可以變更其建立、 啟用和到期的日期，但它可以使用 IKeyManager 撤銷。</span><span class="sxs-lookup"><span data-stu-id="256e6-108">Its creation, activation, and expiration dates can never be changed, though it can revoked by using IKeyManager.</span></span> <span data-ttu-id="256e6-109">此外，其基礎的演算法資訊、 主要金鑰處理內容和加密其他屬性也是不變。</span><span class="sxs-lookup"><span data-stu-id="256e6-109">Additionally, its underlying algorithmic information, master keying material, and encryption at rest properties are also immutable.</span></span>

<span data-ttu-id="256e6-110">如果開發人員變更任何設定，會影響金鑰的持續性，這些變更將不會進入效果下次產生金鑰，不論是透過明確呼叫 IKeyManager.CreateNewKey，或是透過資料保護系統本身[自動索引鍵產生](key-management.md#data-protection-implementation-key-management)行為。</span><span class="sxs-lookup"><span data-stu-id="256e6-110">If the developer changes any setting that affects key persistence, those changes will not go into effect until the next time a key is generated, either via an explicit call to IKeyManager.CreateNewKey or via the data protection system's own [automatic key generation](key-management.md#data-protection-implementation-key-management) behavior.</span></span> <span data-ttu-id="256e6-111">會影響金鑰的持續性的設定如下所示：</span><span class="sxs-lookup"><span data-stu-id="256e6-111">The settings that affect key persistence are as follows:</span></span>

* [<span data-ttu-id="256e6-112">預設金鑰存留期</span><span class="sxs-lookup"><span data-stu-id="256e6-112">The default key lifetime</span></span>](key-management.md#data-protection-implementation-key-management)

* [<span data-ttu-id="256e6-113">在 rest 機制金鑰加密</span><span class="sxs-lookup"><span data-stu-id="256e6-113">The key encryption at rest mechanism</span></span>](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)

* [<span data-ttu-id="256e6-114">包含金鑰的演算法資訊</span><span class="sxs-lookup"><span data-stu-id="256e6-114">The algorithmic information contained within the key</span></span>](../configuration/overview.md#data-protection-changing-algorithms)

<span data-ttu-id="256e6-115">如果您需要早於下一步 自動金鑰循環時間開始進行這些設定，請考慮製作 IKeyManager.CreateNewKey 強制新的金鑰建立的明確呼叫。</span><span class="sxs-lookup"><span data-stu-id="256e6-115">If you need these settings to kick in earlier than the next automatic key rolling time, consider making an explicit call to IKeyManager.CreateNewKey to force the creation of a new key.</span></span> <span data-ttu-id="256e6-116">請記得要提供明確的啟動日期 （{現在 + 2 天} 是最佳經驗法則，讓變更傳播的時間） 與呼叫中的到期日。</span><span class="sxs-lookup"><span data-stu-id="256e6-116">Remember to provide an explicit activation date ({ now + 2 days } is a good rule of thumb to allow time for the change to propagate) and expiration date in the call.</span></span>

>[!TIP]
> <span data-ttu-id="256e6-117">所有碰觸的儲存機制的應用程式應指定含有 IDataProtectionBuilder 擴充方法的相同設定，否則保存金鑰的屬性將取決於特定叫用的索引鍵產生常式的應用程式.</span><span class="sxs-lookup"><span data-stu-id="256e6-117">All applications touching the repository should specify the same settings with the IDataProtectionBuilder extension methods, otherwise the properties of the persisted key will be dependent on the particular application that invoked the key generation routines.</span></span>
