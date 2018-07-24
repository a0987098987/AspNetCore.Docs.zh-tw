---
title: 金鑰的不變性和 ASP.NET Core 中的機碼設定
author: rick-anderson
description: 了解 ASP.NET Core 資料保護金鑰的不變性 Api 的實作詳細資料。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 7796cb102c0f6f03809704016fd36b28c7a82438
ms.sourcegitcommit: 8f8924ce4eb9effeaf489f177fb01b66867da16f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/24/2018
ms.locfileid: "39219299"
---
# <a name="key-immutability-and-key-settings-in-aspnet-core"></a><span data-ttu-id="3c5df-103">金鑰的不變性和 ASP.NET Core 中的機碼設定</span><span class="sxs-lookup"><span data-stu-id="3c5df-103">Key immutability and key settings in ASP.NET Core</span></span>

<span data-ttu-id="3c5df-104">一旦將物件保存至備份存放區，其表示法是永久固定的。</span><span class="sxs-lookup"><span data-stu-id="3c5df-104">Once an object is persisted to the backing store, its representation is forever fixed.</span></span> <span data-ttu-id="3c5df-105">新的資料可以加入至備份存放區，但現有的資料永遠不會變動。</span><span class="sxs-lookup"><span data-stu-id="3c5df-105">New data can be added to the backing store, but existing data can never be mutated.</span></span> <span data-ttu-id="3c5df-106">此行為的主要目的是為了避免資料損毀。</span><span class="sxs-lookup"><span data-stu-id="3c5df-106">The primary purpose of this behavior is to prevent data corruption.</span></span>

<span data-ttu-id="3c5df-107">此行為的一個結果是，一旦金鑰會寫入備份存放區，它是不變。</span><span class="sxs-lookup"><span data-stu-id="3c5df-107">One consequence of this behavior is that once a key is written to the backing store, it's immutable.</span></span> <span data-ttu-id="3c5df-108">其建立、 啟動及到期的日期可以永遠不會變更，但它可以使用撤銷`IKeyManager`。</span><span class="sxs-lookup"><span data-stu-id="3c5df-108">Its creation, activation, and expiration dates can never be changed, though it can revoked by using `IKeyManager`.</span></span> <span data-ttu-id="3c5df-109">此外，其基礎的演算法資訊、 主要金鑰材料和加密在 rest 屬性也是不可變。</span><span class="sxs-lookup"><span data-stu-id="3c5df-109">Additionally, its underlying algorithmic information, master keying material, and encryption at rest properties are also immutable.</span></span>

<span data-ttu-id="3c5df-110">如果開發人員變更任何設定，會影響金鑰持續性，這些變更將不會生效之前會產生金鑰，透過明確呼叫在下一次`IKeyManager.CreateNewKey`或透過資料保護系統本身[自動金鑰產生](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)行為。</span><span class="sxs-lookup"><span data-stu-id="3c5df-110">If the developer changes any setting that affects key persistence, those changes won't go into effect until the next time a key is generated, either via an explicit call to `IKeyManager.CreateNewKey` or via the data protection system's own [automatic key generation](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) behavior.</span></span> <span data-ttu-id="3c5df-111">會影響金鑰持續性的設定如下所示：</span><span class="sxs-lookup"><span data-stu-id="3c5df-111">The settings that affect key persistence are as follows:</span></span>

* [<span data-ttu-id="3c5df-112">預設金鑰存留期</span><span class="sxs-lookup"><span data-stu-id="3c5df-112">The default key lifetime</span></span>](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)

* [<span data-ttu-id="3c5df-113">在 rest 機制金鑰的加密</span><span class="sxs-lookup"><span data-stu-id="3c5df-113">The key encryption at rest mechanism</span></span>](xref:security/data-protection/implementation/key-encryption-at-rest)

* [<span data-ttu-id="3c5df-114">機碼內所包含的演算法資訊</span><span class="sxs-lookup"><span data-stu-id="3c5df-114">The algorithmic information contained within the key</span></span>](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

<span data-ttu-id="3c5df-115">如果您需要早於下一步 的自動金鑰輪換的時間開始進行這些設定，請考慮進行明確呼叫`IKeyManager.CreateNewKey`強制建立新的金鑰。</span><span class="sxs-lookup"><span data-stu-id="3c5df-115">If you need these settings to kick in earlier than the next automatic key rolling time, consider making an explicit call to `IKeyManager.CreateNewKey` to force the creation of a new key.</span></span> <span data-ttu-id="3c5df-116">請記住，若要提供明確的啟動日期 （{現在 + 2 天} 是很好的經驗法則，讓變更傳播的時間） 和到期日，在呼叫中的。</span><span class="sxs-lookup"><span data-stu-id="3c5df-116">Remember to provide an explicit activation date ({ now + 2 days } is a good rule of thumb to allow time for the change to propagate) and expiration date in the call.</span></span>

>[!TIP]
> <span data-ttu-id="3c5df-117">觸碰存放庫的所有應用程式應指定相同的設定與`IDataProtectionBuilder`擴充方法。</span><span class="sxs-lookup"><span data-stu-id="3c5df-117">All applications touching the repository should specify the same settings with the `IDataProtectionBuilder` extension methods.</span></span> <span data-ttu-id="3c5df-118">否則，保存金鑰的屬性會取決於特定的應用程式叫用的金鑰產生常式。</span><span class="sxs-lookup"><span data-stu-id="3c5df-118">Otherwise, the properties of the persisted key will be dependent on the particular application that invoked the key generation routines.</span></span>
