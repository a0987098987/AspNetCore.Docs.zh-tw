---
title: ASP.NET Core 中的金鑰不可變性和金鑰設定
author: rick-anderson
description: 瞭解 ASP.NET Core 資料保護金鑰永久性 Api 的執行細節。
ms.author: riande
ms.date: 10/14/2016
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 01fb3a155aefa34dcd9ede8e7d6ada8fe6bb751c
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85403816"
---
# <a name="key-immutability-and-key-settings-in-aspnet-core"></a><span data-ttu-id="54d81-103">ASP.NET Core 中的金鑰不可變性和金鑰設定</span><span class="sxs-lookup"><span data-stu-id="54d81-103">Key immutability and key settings in ASP.NET Core</span></span>

<span data-ttu-id="54d81-104">一旦物件保存到備份存放區，其標記法會永久固定。</span><span class="sxs-lookup"><span data-stu-id="54d81-104">Once an object is persisted to the backing store, its representation is forever fixed.</span></span> <span data-ttu-id="54d81-105">新的資料可以新增至備份存放區，但現有的資料永遠不會變化。</span><span class="sxs-lookup"><span data-stu-id="54d81-105">New data can be added to the backing store, but existing data can never be mutated.</span></span> <span data-ttu-id="54d81-106">此行為的主要目的是要防止資料損毀。</span><span class="sxs-lookup"><span data-stu-id="54d81-106">The primary purpose of this behavior is to prevent data corruption.</span></span>

<span data-ttu-id="54d81-107">這個行為的其中一個結果是，一旦金鑰寫入至備份存放區，它就是不可變的。</span><span class="sxs-lookup"><span data-stu-id="54d81-107">One consequence of this behavior is that once a key is written to the backing store, it's immutable.</span></span> <span data-ttu-id="54d81-108">它的建立、啟用和到期日永遠不會變更，但可以使用撤銷 `IKeyManager` 。</span><span class="sxs-lookup"><span data-stu-id="54d81-108">Its creation, activation, and expiration dates can never be changed, though it can revoked by using `IKeyManager`.</span></span> <span data-ttu-id="54d81-109">此外，其基礎演算法資訊、主要金鑰內容和待用加密屬性也是不可變的。</span><span class="sxs-lookup"><span data-stu-id="54d81-109">Additionally, its underlying algorithmic information, master keying material, and encryption at rest properties are also immutable.</span></span>

<span data-ttu-id="54d81-110">如果開發人員變更任何會影響金鑰持續性的設定，這些變更在下一次產生金鑰之前都不會生效，不論是透過明確呼叫 `IKeyManager.CreateNewKey` 或透過資料保護系統本身的[自動金鑰產生](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)行為。</span><span class="sxs-lookup"><span data-stu-id="54d81-110">If the developer changes any setting that affects key persistence, those changes won't go into effect until the next time a key is generated, either via an explicit call to `IKeyManager.CreateNewKey` or via the data protection system's own [automatic key generation](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) behavior.</span></span> <span data-ttu-id="54d81-111">影響金鑰持續性的設定如下：</span><span class="sxs-lookup"><span data-stu-id="54d81-111">The settings that affect key persistence are as follows:</span></span>

* [<span data-ttu-id="54d81-112">預設金鑰存留期</span><span class="sxs-lookup"><span data-stu-id="54d81-112">The default key lifetime</span></span>](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)

* [<span data-ttu-id="54d81-113">待用金鑰加密機制</span><span class="sxs-lookup"><span data-stu-id="54d81-113">The key encryption at rest mechanism</span></span>](xref:security/data-protection/implementation/key-encryption-at-rest)

* [<span data-ttu-id="54d81-114">包含在索引鍵中的演算法資訊</span><span class="sxs-lookup"><span data-stu-id="54d81-114">The algorithmic information contained within the key</span></span>](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

<span data-ttu-id="54d81-115">如果您需要這些設定，以在下一個自動的金鑰滾動時間之前開始進行，請考慮明確地呼叫 `IKeyManager.CreateNewKey` 以強制建立新的金鑰。</span><span class="sxs-lookup"><span data-stu-id="54d81-115">If you need these settings to kick in earlier than the next automatic key rolling time, consider making an explicit call to `IKeyManager.CreateNewKey` to force the creation of a new key.</span></span> <span data-ttu-id="54d81-116">請記得提供明確的啟用日期（{now + 2 天} 是一個好用的經驗法則，可允許變更傳播的時間）和到期日的呼叫。</span><span class="sxs-lookup"><span data-stu-id="54d81-116">Remember to provide an explicit activation date ({ now + 2 days } is a good rule of thumb to allow time for the change to propagate) and expiration date in the call.</span></span>

>[!TIP]
> <span data-ttu-id="54d81-117">所有觸及存放庫的應用程式都應該使用擴充方法來指定相同的設定 `IDataProtectionBuilder` 。</span><span class="sxs-lookup"><span data-stu-id="54d81-117">All applications touching the repository should specify the same settings with the `IDataProtectionBuilder` extension methods.</span></span> <span data-ttu-id="54d81-118">否則，保存的金鑰屬性會相依于叫用金鑰產生常式的特定應用程式。</span><span class="sxs-lookup"><span data-stu-id="54d81-118">Otherwise, the properties of the persisted key will be dependent on the particular application that invoked the key generation routines.</span></span>
