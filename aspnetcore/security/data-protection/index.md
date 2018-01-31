---
title: "ASP.NET Core 的資料保護"
author: rick-anderson
description: "本文件是各種 ASP.NET Core 資料保護主題的目錄。"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/index
ms.openlocfilehash: b846fb7cb28eeceb8c0bdb47135e1cf014ae08a7
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="66863-103">ASP.NET Core 中的資料保護：取用者 API、組態、擴充性 API 和實作</span><span class="sxs-lookup"><span data-stu-id="66863-103">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="66863-104">資料保護簡介</span><span class="sxs-lookup"><span data-stu-id="66863-104">Introduction to data protection</span></span>](introduction.md)

* [<span data-ttu-id="66863-105">資料保護 API 使用者入門</span><span class="sxs-lookup"><span data-stu-id="66863-105">Get started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="66863-106">取用者 API</span><span class="sxs-lookup"><span data-stu-id="66863-106">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="66863-107">取用者 API 概觀</span><span class="sxs-lookup"><span data-stu-id="66863-107">Consumer APIs overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="66863-108">目的字串</span><span class="sxs-lookup"><span data-stu-id="66863-108">Purpose strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="66863-109">目的階層和多租用戶</span><span class="sxs-lookup"><span data-stu-id="66863-109">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="66863-110">密碼雜湊</span><span class="sxs-lookup"><span data-stu-id="66863-110">Password hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="66863-111">限制受保護承載的存留期</span><span class="sxs-lookup"><span data-stu-id="66863-111">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="66863-112">取消索引鍵已撤銷之承載的保護</span><span class="sxs-lookup"><span data-stu-id="66863-112">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="66863-113">組態</span><span class="sxs-lookup"><span data-stu-id="66863-113">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="66863-114">設定資料保護</span><span class="sxs-lookup"><span data-stu-id="66863-114">Configuring data protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="66863-115">預設設定</span><span class="sxs-lookup"><span data-stu-id="66863-115">Default settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="66863-116">整個電腦的原則</span><span class="sxs-lookup"><span data-stu-id="66863-116">Machine-wide policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="66863-117">非 DI 感知案例</span><span class="sxs-lookup"><span data-stu-id="66863-117">Non DI-aware scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="66863-118">擴充性 API</span><span class="sxs-lookup"><span data-stu-id="66863-118">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="66863-119">核心加密擴充性</span><span class="sxs-lookup"><span data-stu-id="66863-119">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="66863-120">金鑰管理擴充性</span><span class="sxs-lookup"><span data-stu-id="66863-120">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="66863-121">其他 API</span><span class="sxs-lookup"><span data-stu-id="66863-121">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="66863-122">實作</span><span class="sxs-lookup"><span data-stu-id="66863-122">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="66863-123">已驗證的加密詳細資料</span><span class="sxs-lookup"><span data-stu-id="66863-123">Authenticated encryption details</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="66863-124">子機碼衍生和驗證的加密</span><span class="sxs-lookup"><span data-stu-id="66863-124">Subkey derivation and authenticated encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="66863-125">內容標頭</span><span class="sxs-lookup"><span data-stu-id="66863-125">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="66863-126">金鑰管理</span><span class="sxs-lookup"><span data-stu-id="66863-126">Key management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="66863-127">金鑰儲存體提供者</span><span class="sxs-lookup"><span data-stu-id="66863-127">Key storage providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="66863-128">待用時加密金鑰</span><span class="sxs-lookup"><span data-stu-id="66863-128">Key encryption at rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="66863-129">金鑰的不變性和變更設定</span><span class="sxs-lookup"><span data-stu-id="66863-129">Key immutability and changing settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="66863-130">金鑰儲存體格式</span><span class="sxs-lookup"><span data-stu-id="66863-130">Key storage format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="66863-131">暫時資料保護提供者</span><span class="sxs-lookup"><span data-stu-id="66863-131">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="66863-132">相容性</span><span class="sxs-lookup"><span data-stu-id="66863-132">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="66863-133">在應用程式間共用 Cookie</span><span class="sxs-lookup"><span data-stu-id="66863-133">Sharing cookies among apps</span></span>](xref:security/data-protection/compatibility/cookie-sharing)

  * [<span data-ttu-id="66863-134">取代 ASP.NET 中的 <machineKey></span><span class="sxs-lookup"><span data-stu-id="66863-134">Replacing <machineKey> in ASP.NET</span></span>](xref:security/data-protection/compatibility/replacing-machinekey)
