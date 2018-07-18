---
title: 資料保護的金鑰管理和 ASP.NET Core 中的存留期
author: rick-anderson
description: 深入了解資料保護的金鑰管理和 ASP.NET Core 中的存留期。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/configuration/default-settings
ms.openlocfilehash: beff17dd81143db02a0cbc79fa7cb3a6a4deeda6
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095095"
---
# <a name="data-protection-key-management-and-lifetime-in-aspnet-core"></a><span data-ttu-id="240cb-103">資料保護的金鑰管理和 ASP.NET Core 中的存留期</span><span class="sxs-lookup"><span data-stu-id="240cb-103">Data Protection key management and lifetime in ASP.NET Core</span></span>

<span data-ttu-id="240cb-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="240cb-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="key-management"></a><span data-ttu-id="240cb-105">金鑰管理</span><span class="sxs-lookup"><span data-stu-id="240cb-105">Key management</span></span>

<span data-ttu-id="240cb-106">應用程式會嘗試偵測其操作環境和處理上自己的金鑰設定。</span><span class="sxs-lookup"><span data-stu-id="240cb-106">The app attempts to detect its operational environment and handle key configuration on its own.</span></span>

1. <span data-ttu-id="240cb-107">如果應用程式裝載於[Azure 應用程式](https://azure.microsoft.com/services/app-service/)，金鑰會保存到 *%HOME%\ASP.NET\DataProtection-Keys*資料夾。</span><span class="sxs-lookup"><span data-stu-id="240cb-107">If the app is hosted in [Azure Apps](https://azure.microsoft.com/services/app-service/), keys are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="240cb-108">此資料夾使用網路儲存體進行保存，並會在裝載應用程式的所有電腦上同步。</span><span class="sxs-lookup"><span data-stu-id="240cb-108">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span>
   * <span data-ttu-id="240cb-109">金鑰待用時不受保護。</span><span class="sxs-lookup"><span data-stu-id="240cb-109">Keys aren't protected at rest.</span></span>
   * <span data-ttu-id="240cb-110">*DataProtection 金鑰*資料夾提供金鑰環單一部署位置中的應用程式的所有執行個體。</span><span class="sxs-lookup"><span data-stu-id="240cb-110">The *DataProtection-Keys* folder supplies the key ring to all instances of an app in a single deployment slot.</span></span>
   * <span data-ttu-id="240cb-111">各部署位置，例如預備和生產位置，不會共用金鑰環。</span><span class="sxs-lookup"><span data-stu-id="240cb-111">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span> <span data-ttu-id="240cb-112">當您交換部署位置，例如交換預備環境或生產環境，或使用 A 之間 / B 測試、 使用資料保護的任何應用程式將無法解密儲存的資料使用前一個位置內的金鑰環。</span><span class="sxs-lookup"><span data-stu-id="240cb-112">When you swap between deployment slots, for example swapping Staging to Production or using A/B testing, any app using Data Protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="240cb-113">這會導致正在登入的使用者複製應用程式使用標準的 ASP.NET Core cookie 驗證，因為它使用資料保護來保護其 cookie。</span><span class="sxs-lookup"><span data-stu-id="240cb-113">This leads to users being logged out of an app that uses the standard ASP.NET Core cookie authentication, as it uses Data Protection to protect its cookies.</span></span> <span data-ttu-id="240cb-114">如果您想要的位置無關的金鑰環，請使用外部金鑰環提供者，例如 Azure Blob 儲存體、 Azure Key Vault，SQL 存放區中，或 Redis 快取。</span><span class="sxs-lookup"><span data-stu-id="240cb-114">If you desire slot-independent key rings, use an external key ring provider, such as Azure Blob Storage, Azure Key Vault, a SQL store, or Redis cache.</span></span>

1. <span data-ttu-id="240cb-115">如果使用者設定檔可用時，會保存索引鍵 *%LOCALAPPDATA%\ASP.NET\DataProtection-Keys*資料夾。</span><span class="sxs-lookup"><span data-stu-id="240cb-115">If the user profile is available, keys are persisted to the *%LOCALAPPDATA%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="240cb-116">如果作業系統是 Windows，在待用期間使用 DPAPI 加密金鑰。</span><span class="sxs-lookup"><span data-stu-id="240cb-116">If the operating system is Windows, the keys are encrypted at rest using DPAPI.</span></span>

1. <span data-ttu-id="240cb-117">如果應用程式裝載在 IIS 中，金鑰會保存到特殊的登錄機碼，只是背景工作處理序帳戶列入 Acl 中的 HKLM 登錄中。</span><span class="sxs-lookup"><span data-stu-id="240cb-117">If the app is hosted in IIS, keys are persisted to the HKLM registry in a special registry key that's ACLed only to the worker process account.</span></span> <span data-ttu-id="240cb-118">在待用期間使用 DPAPI 加密金鑰。</span><span class="sxs-lookup"><span data-stu-id="240cb-118">Keys are encrypted at rest using DPAPI.</span></span>

1. <span data-ttu-id="240cb-119">如果上述條件均不相符時，金鑰不會保存在目前的程序之外。</span><span class="sxs-lookup"><span data-stu-id="240cb-119">If none of these conditions match, keys aren't persisted outside of the current process.</span></span> <span data-ttu-id="240cb-120">處理程序關閉時，所有已產生的金鑰將會遺失。</span><span class="sxs-lookup"><span data-stu-id="240cb-120">When the process shuts down, all generated keys are lost.</span></span>

<span data-ttu-id="240cb-121">開發人員一律可以完全控制，並儲存金鑰，以及可以覆寫。</span><span class="sxs-lookup"><span data-stu-id="240cb-121">The developer is always in full control and can override how and where keys are stored.</span></span> <span data-ttu-id="240cb-122">上述的前三個選項應該提供不錯的預設值對於大多數的應用程式，類似 ASP.NET  **\<machineKey >** 自動產生常式過去。</span><span class="sxs-lookup"><span data-stu-id="240cb-122">The first three options above should provide good defaults for most apps similar to how the ASP.NET **\<machineKey>** auto-generation routines worked in the past.</span></span> <span data-ttu-id="240cb-123">最終，後援選項是需要開發人員指定的唯一案例[組態](xref:security/data-protection/configuration/overview)預付如果他們想要金鑰的持續性，但此後援才會發生在少數情況下。</span><span class="sxs-lookup"><span data-stu-id="240cb-123">The final, fallback option is the only scenario that requires the developer to specify [configuration](xref:security/data-protection/configuration/overview) upfront if they want key persistence, but this fallback only occurs in rare situations.</span></span>

<span data-ttu-id="240cb-124">當裝載的 Docker 容器中，金鑰應保留在 Docker 磁碟區 （共用磁碟區或主應用程式掛接的磁碟區會保存超過容器的存留期） 的資料夾或外部提供者，例如[Azure Key Vault](https://azure.microsoft.com/services/key-vault/)或是[Redis](https://redis.io/)。</span><span class="sxs-lookup"><span data-stu-id="240cb-124">When hosting in a Docker container, keys should be persisted in a folder that's a Docker volume (a shared volume or a host-mounted volume that persists beyond the container's lifetime) or in an external provider, such as [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) or [Redis](https://redis.io/).</span></span> <span data-ttu-id="240cb-125">如果應用程式無法存取的共用的網路磁碟區，還有在 web 伺服陣列案例中有用外部提供者 (請參閱[PersistKeysToFileSystem](xref:security/data-protection/configuration/overview#persistkeystofilesystem)如需詳細資訊)。</span><span class="sxs-lookup"><span data-stu-id="240cb-125">An external provider is also useful in web farm scenarios if apps can't access a shared network volume (see [PersistKeysToFileSystem](xref:security/data-protection/configuration/overview#persistkeystofilesystem) for more information).</span></span>

> [!WARNING]
> <span data-ttu-id="240cb-126">如果開發人員會覆寫上面所述的規則，並指向特定的金鑰存放庫資料保護系統，會停用自動加密待用的金鑰。</span><span class="sxs-lookup"><span data-stu-id="240cb-126">If the developer overrides the rules outlined above and points the Data Protection system at a specific key repository, automatic encryption of keys at rest is disabled.</span></span> <span data-ttu-id="240cb-127">待用保護可以透過重新啟用[組態](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="240cb-127">At-rest protection can be re-enabled via [configuration](xref:security/data-protection/configuration/overview).</span></span>

## <a name="key-lifetime"></a><span data-ttu-id="240cb-128">金鑰存留期</span><span class="sxs-lookup"><span data-stu-id="240cb-128">Key lifetime</span></span>

<span data-ttu-id="240cb-129">根據預設，索引鍵具有 90 天的存留期。</span><span class="sxs-lookup"><span data-stu-id="240cb-129">Keys have a 90-day lifetime by default.</span></span> <span data-ttu-id="240cb-130">索引鍵過期時，應用程式自動產生新的金鑰，並將新的金鑰設定為作用中金鑰。</span><span class="sxs-lookup"><span data-stu-id="240cb-130">When a key expires, the app automatically generates a new key and sets the new key as the active key.</span></span> <span data-ttu-id="240cb-131">只要已停用的索引鍵會保留在系統上，您的應用程式可以解密與其受保護的任何資料。</span><span class="sxs-lookup"><span data-stu-id="240cb-131">As long as retired keys remain on the system, your app can decrypt any data protected with them.</span></span> <span data-ttu-id="240cb-132">請參閱[金鑰管理](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="240cb-132">See [key management](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling) for more information.</span></span>

## <a name="default-algorithms"></a><span data-ttu-id="240cb-133">預設的演算法</span><span class="sxs-lookup"><span data-stu-id="240cb-133">Default algorithms</span></span>

<span data-ttu-id="240cb-134">使用預設承載的保護演算法會是 AES-256-CBC 機密性和 HMACSHA256 的真實性。</span><span class="sxs-lookup"><span data-stu-id="240cb-134">The default payload protection algorithm used is AES-256-CBC for confidentiality and HMACSHA256 for authenticity.</span></span> <span data-ttu-id="240cb-135">變更每隔 90 天，512 位元主要金鑰用來衍生兩個的子機碼，以使用這些演算法，每個裝載為基礎。</span><span class="sxs-lookup"><span data-stu-id="240cb-135">A 512-bit master key, changed every 90 days, is used to derive the two sub-keys used for these algorithms on a per-payload basis.</span></span> <span data-ttu-id="240cb-136">請參閱[子機碼衍生](xref:security/data-protection/implementation/subkeyderivation#additional-authenticated-data-and-subkey-derivation)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="240cb-136">See [subkey derivation](xref:security/data-protection/implementation/subkeyderivation#additional-authenticated-data-and-subkey-derivation) for more information.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="240cb-137">其他資源</span><span class="sxs-lookup"><span data-stu-id="240cb-137">Additional resources</span></span>

* <xref:security/data-protection/extensibility/key-management>
* <xref:host-and-deploy/web-farm>
