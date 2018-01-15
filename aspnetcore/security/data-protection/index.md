---
title: "ASP.NET Core 的資料保護"
author: rick-anderson
description: "本文件是各種 ASP.NET Core 資料保護主題的目錄。"
keywords: "ASP.NET Core, 資料保護"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 1f402da8-1052-4970-9835-9f9f16a02dbc
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/index
ms.openlocfilehash: cbf18c6ec867fefec22980f3e3493562594ef72d
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/11/2018
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="5a52e-104">ASP.NET Core 中的資料保護：取用者 API、組態、擴充性 API 和實作</span><span class="sxs-lookup"><span data-stu-id="5a52e-104">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="5a52e-105">資料保護簡介</span><span class="sxs-lookup"><span data-stu-id="5a52e-105">Introduction to data protection</span></span>](introduction.md)

* [<span data-ttu-id="5a52e-106">資料保護 API 使用者入門</span><span class="sxs-lookup"><span data-stu-id="5a52e-106">Get started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="5a52e-107">取用者 API</span><span class="sxs-lookup"><span data-stu-id="5a52e-107">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="5a52e-108">取用者 API 概觀</span><span class="sxs-lookup"><span data-stu-id="5a52e-108">Consumer APIs overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="5a52e-109">目的字串</span><span class="sxs-lookup"><span data-stu-id="5a52e-109">Purpose strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="5a52e-110">目的階層和多租用戶</span><span class="sxs-lookup"><span data-stu-id="5a52e-110">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="5a52e-111">密碼雜湊</span><span class="sxs-lookup"><span data-stu-id="5a52e-111">Password hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="5a52e-112">限制受保護承載的存留期</span><span class="sxs-lookup"><span data-stu-id="5a52e-112">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="5a52e-113">取消索引鍵已撤銷之承載的保護</span><span class="sxs-lookup"><span data-stu-id="5a52e-113">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="5a52e-114">組態</span><span class="sxs-lookup"><span data-stu-id="5a52e-114">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="5a52e-115">設定資料保護</span><span class="sxs-lookup"><span data-stu-id="5a52e-115">Configuring data protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="5a52e-116">預設設定</span><span class="sxs-lookup"><span data-stu-id="5a52e-116">Default settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="5a52e-117">整個電腦的原則</span><span class="sxs-lookup"><span data-stu-id="5a52e-117">Machine-wide policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="5a52e-118">非 DI 感知案例</span><span class="sxs-lookup"><span data-stu-id="5a52e-118">Non DI-aware scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="5a52e-119">擴充性 API</span><span class="sxs-lookup"><span data-stu-id="5a52e-119">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="5a52e-120">核心加密擴充性</span><span class="sxs-lookup"><span data-stu-id="5a52e-120">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="5a52e-121">金鑰管理擴充性</span><span class="sxs-lookup"><span data-stu-id="5a52e-121">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="5a52e-122">其他 API</span><span class="sxs-lookup"><span data-stu-id="5a52e-122">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="5a52e-123">實作</span><span class="sxs-lookup"><span data-stu-id="5a52e-123">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="5a52e-124">已驗證的加密詳細資料</span><span class="sxs-lookup"><span data-stu-id="5a52e-124">Authenticated encryption details</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="5a52e-125">子機碼衍生和驗證的加密</span><span class="sxs-lookup"><span data-stu-id="5a52e-125">Subkey derivation and authenticated encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="5a52e-126">內容標頭</span><span class="sxs-lookup"><span data-stu-id="5a52e-126">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="5a52e-127">金鑰管理</span><span class="sxs-lookup"><span data-stu-id="5a52e-127">Key management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="5a52e-128">金鑰儲存體提供者</span><span class="sxs-lookup"><span data-stu-id="5a52e-128">Key storage providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="5a52e-129">待用時加密金鑰</span><span class="sxs-lookup"><span data-stu-id="5a52e-129">Key encryption at rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="5a52e-130">金鑰的不變性和變更設定</span><span class="sxs-lookup"><span data-stu-id="5a52e-130">Key immutability and changing settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="5a52e-131">金鑰儲存體格式</span><span class="sxs-lookup"><span data-stu-id="5a52e-131">Key storage format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="5a52e-132">暫時資料保護提供者</span><span class="sxs-lookup"><span data-stu-id="5a52e-132">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="5a52e-133">相容性</span><span class="sxs-lookup"><span data-stu-id="5a52e-133">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="5a52e-134">應用程式間共用 Cookie</span><span class="sxs-lookup"><span data-stu-id="5a52e-134">Sharing cookies between apps</span></span>](compatibility/cookie-sharing.md)

  * [<span data-ttu-id="5a52e-135">取代 ASP.NET 中的 <machineKey></span><span class="sxs-lookup"><span data-stu-id="5a52e-135">Replacing <machineKey> in ASP.NET</span></span>](compatibility/replacing-machinekey.md)
