---
title: "ASP.NET Core 的資料保護"
author: rick-anderson
description: "本文件是各種 ASP.NET Core 資料保護主題的目錄。"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/index
ms.openlocfilehash: 7bbd203a67b32032ba2ab82448a5fc9a495b52aa
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="56e80-103">ASP.NET Core 中的資料保護：取用者 API、組態、擴充性 API 和實作</span><span class="sxs-lookup"><span data-stu-id="56e80-103">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="56e80-104">資料保護簡介</span><span class="sxs-lookup"><span data-stu-id="56e80-104">Introduction to data protection</span></span>](introduction.md)

* [<span data-ttu-id="56e80-105">資料保護 API 使用者入門</span><span class="sxs-lookup"><span data-stu-id="56e80-105">Get started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="56e80-106">取用者 API</span><span class="sxs-lookup"><span data-stu-id="56e80-106">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="56e80-107">取用者 API 概觀</span><span class="sxs-lookup"><span data-stu-id="56e80-107">Consumer APIs overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="56e80-108">目的字串</span><span class="sxs-lookup"><span data-stu-id="56e80-108">Purpose strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="56e80-109">目的階層和多租用戶</span><span class="sxs-lookup"><span data-stu-id="56e80-109">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="56e80-110">密碼雜湊</span><span class="sxs-lookup"><span data-stu-id="56e80-110">Password hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="56e80-111">限制受保護承載的存留期</span><span class="sxs-lookup"><span data-stu-id="56e80-111">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="56e80-112">取消索引鍵已撤銷之承載的保護</span><span class="sxs-lookup"><span data-stu-id="56e80-112">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="56e80-113">組態</span><span class="sxs-lookup"><span data-stu-id="56e80-113">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="56e80-114">設定資料保護</span><span class="sxs-lookup"><span data-stu-id="56e80-114">Configuring data protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="56e80-115">預設設定</span><span class="sxs-lookup"><span data-stu-id="56e80-115">Default settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="56e80-116">整個電腦的原則</span><span class="sxs-lookup"><span data-stu-id="56e80-116">Machine-wide policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="56e80-117">非 DI 感知案例</span><span class="sxs-lookup"><span data-stu-id="56e80-117">Non DI-aware scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="56e80-118">擴充性 API</span><span class="sxs-lookup"><span data-stu-id="56e80-118">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="56e80-119">核心加密擴充性</span><span class="sxs-lookup"><span data-stu-id="56e80-119">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="56e80-120">金鑰管理擴充性</span><span class="sxs-lookup"><span data-stu-id="56e80-120">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="56e80-121">其他 API</span><span class="sxs-lookup"><span data-stu-id="56e80-121">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="56e80-122">實作</span><span class="sxs-lookup"><span data-stu-id="56e80-122">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="56e80-123">已驗證的加密詳細資料</span><span class="sxs-lookup"><span data-stu-id="56e80-123">Authenticated encryption details</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="56e80-124">子機碼衍生和驗證的加密</span><span class="sxs-lookup"><span data-stu-id="56e80-124">Subkey derivation and authenticated encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="56e80-125">內容標頭</span><span class="sxs-lookup"><span data-stu-id="56e80-125">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="56e80-126">金鑰管理</span><span class="sxs-lookup"><span data-stu-id="56e80-126">Key management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="56e80-127">金鑰儲存體提供者</span><span class="sxs-lookup"><span data-stu-id="56e80-127">Key storage providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="56e80-128">待用時加密金鑰</span><span class="sxs-lookup"><span data-stu-id="56e80-128">Key encryption at rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="56e80-129">金鑰的不變性和變更設定</span><span class="sxs-lookup"><span data-stu-id="56e80-129">Key immutability and changing settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="56e80-130">金鑰儲存體格式</span><span class="sxs-lookup"><span data-stu-id="56e80-130">Key storage format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="56e80-131">暫時資料保護提供者</span><span class="sxs-lookup"><span data-stu-id="56e80-131">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="56e80-132">相容性</span><span class="sxs-lookup"><span data-stu-id="56e80-132">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="56e80-133">應用程式間共用 Cookie</span><span class="sxs-lookup"><span data-stu-id="56e80-133">Sharing cookies between apps</span></span>](compatibility/cookie-sharing.md)

  * [<span data-ttu-id="56e80-134">取代 ASP.NET 中的 <machineKey></span><span class="sxs-lookup"><span data-stu-id="56e80-134">Replacing <machineKey> in ASP.NET</span></span>](compatibility/replacing-machinekey.md)
