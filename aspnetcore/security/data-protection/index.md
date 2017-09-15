---
title: "ASP.NET Core 的資料保護"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 1f402da8-1052-4970-9835-9f9f16a02dbc
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/index
ms.openlocfilehash: 39f2ca96f8542de033274ea957b5c7736948c981
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2017
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="bc852-103">ASP.NET Core 中的資料保護：取用者 API、組態、擴充性 API 和實作</span><span class="sxs-lookup"><span data-stu-id="bc852-103">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="bc852-104">資料保護簡介</span><span class="sxs-lookup"><span data-stu-id="bc852-104">Introduction to Data Protection</span></span>](introduction.md)

* [<span data-ttu-id="bc852-105">資料保護 API 使用者入門</span><span class="sxs-lookup"><span data-stu-id="bc852-105">Getting Started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="bc852-106">取用者 API</span><span class="sxs-lookup"><span data-stu-id="bc852-106">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="bc852-107">取用者 API 概觀</span><span class="sxs-lookup"><span data-stu-id="bc852-107">Consumer APIs Overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="bc852-108">目的字串</span><span class="sxs-lookup"><span data-stu-id="bc852-108">Purpose Strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="bc852-109">目的階層和多租用戶</span><span class="sxs-lookup"><span data-stu-id="bc852-109">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="bc852-110">密碼雜湊</span><span class="sxs-lookup"><span data-stu-id="bc852-110">Password Hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="bc852-111">限制受保護承載的存留期</span><span class="sxs-lookup"><span data-stu-id="bc852-111">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="bc852-112">取消索引鍵已撤銷之承載的保護</span><span class="sxs-lookup"><span data-stu-id="bc852-112">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="bc852-113">組態</span><span class="sxs-lookup"><span data-stu-id="bc852-113">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="bc852-114">設定資料保護</span><span class="sxs-lookup"><span data-stu-id="bc852-114">Configuring Data Protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="bc852-115">預設設定</span><span class="sxs-lookup"><span data-stu-id="bc852-115">Default Settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="bc852-116">電腦全域原則</span><span class="sxs-lookup"><span data-stu-id="bc852-116">Machine Wide Policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="bc852-117">非 DI 感知案例</span><span class="sxs-lookup"><span data-stu-id="bc852-117">Non DI Aware Scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="bc852-118">擴充性 API</span><span class="sxs-lookup"><span data-stu-id="bc852-118">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="bc852-119">核心加密擴充性</span><span class="sxs-lookup"><span data-stu-id="bc852-119">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="bc852-120">金鑰管理擴充性</span><span class="sxs-lookup"><span data-stu-id="bc852-120">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="bc852-121">其他 API</span><span class="sxs-lookup"><span data-stu-id="bc852-121">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="bc852-122">實作</span><span class="sxs-lookup"><span data-stu-id="bc852-122">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="bc852-123">已驗證的加密詳細資料</span><span class="sxs-lookup"><span data-stu-id="bc852-123">Authenticated encryption details.</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="bc852-124">子機碼衍生和驗證的加密</span><span class="sxs-lookup"><span data-stu-id="bc852-124">Subkey Derivation and Authenticated Encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="bc852-125">內容標頭</span><span class="sxs-lookup"><span data-stu-id="bc852-125">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="bc852-126">金鑰管理</span><span class="sxs-lookup"><span data-stu-id="bc852-126">Key Management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="bc852-127">金鑰儲存體提供者</span><span class="sxs-lookup"><span data-stu-id="bc852-127">Key Storage Providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="bc852-128">靜止時的金鑰加密</span><span class="sxs-lookup"><span data-stu-id="bc852-128">Key Encryption At Rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="bc852-129">金鑰的不變性和變更設定</span><span class="sxs-lookup"><span data-stu-id="bc852-129">Key Immutability and Changing Settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="bc852-130">金鑰儲存體格式</span><span class="sxs-lookup"><span data-stu-id="bc852-130">Key Storage Format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="bc852-131">暫時資料保護提供者</span><span class="sxs-lookup"><span data-stu-id="bc852-131">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="bc852-132">相容性</span><span class="sxs-lookup"><span data-stu-id="bc852-132">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="bc852-133">共用應用程式之間的 Cookie</span><span class="sxs-lookup"><span data-stu-id="bc852-133">Sharing cookies between applications</span></span>](compatibility/cookie-sharing.md)

  * [<span data-ttu-id="bc852-134">取代 ASP.NET 中的 <machineKey></span><span class="sxs-lookup"><span data-stu-id="bc852-134">Replacing <machineKey> in ASP.NET</span></span>](compatibility/replacing-machinekey.md)
