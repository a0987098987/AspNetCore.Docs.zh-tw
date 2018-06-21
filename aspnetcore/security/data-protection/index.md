---
title: ASP.NET Core 的資料保護
author: rick-anderson
description: 本文件是各種 ASP.NET Core 資料保護主題的目錄。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/index
ms.openlocfilehash: 1c38c587956dd99078fa4386c2f4423d784abc60
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277120"
---
# <a name="data-protection-in-aspnet-core"></a><span data-ttu-id="74418-103">ASP.NET Core 的資料保護</span><span class="sxs-lookup"><span data-stu-id="74418-103">Data Protection in ASP.NET Core</span></span>

* [<span data-ttu-id="74418-104">資料保護簡介</span><span class="sxs-lookup"><span data-stu-id="74418-104">Introduction to data protection</span></span>](xref:security/data-protection/introduction)

* [<span data-ttu-id="74418-105">資料保護 API 使用者入門</span><span class="sxs-lookup"><span data-stu-id="74418-105">Get started with the Data Protection APIs</span></span>](xref:security/data-protection/using-data-protection)

* [<span data-ttu-id="74418-106">取用者 API</span><span class="sxs-lookup"><span data-stu-id="74418-106">Consumer APIs</span></span>](xref:security/data-protection/consumer-apis/index)

  * [<span data-ttu-id="74418-107">取用者 API 概觀</span><span class="sxs-lookup"><span data-stu-id="74418-107">Consumer APIs overview</span></span>](xref:security/data-protection/consumer-apis/overview)

  * [<span data-ttu-id="74418-108">目的字串</span><span class="sxs-lookup"><span data-stu-id="74418-108">Purpose strings</span></span>](xref:security/data-protection/consumer-apis/purpose-strings)

  * [<span data-ttu-id="74418-109">目的階層和多租用戶</span><span class="sxs-lookup"><span data-stu-id="74418-109">Purpose hierarchy and multi-tenancy</span></span>](xref:security/data-protection/consumer-apis/purpose-strings-multitenancy)

  * [<span data-ttu-id="74418-110">雜湊密碼</span><span class="sxs-lookup"><span data-stu-id="74418-110">Hash passwords</span></span>](xref:security/data-protection/consumer-apis/password-hashing)

  * [<span data-ttu-id="74418-111">限制受保護承載的存留期</span><span class="sxs-lookup"><span data-stu-id="74418-111">Limit the lifetime of protected payloads</span></span>](xref:security/data-protection/consumer-apis/limited-lifetime-payloads)

  * [<span data-ttu-id="74418-112">取消索引鍵已撤銷之承載的保護</span><span class="sxs-lookup"><span data-stu-id="74418-112">Unprotect payloads whose keys have been revoked</span></span>](xref:security/data-protection/consumer-apis/dangerous-unprotect)

* [<span data-ttu-id="74418-113">組態</span><span class="sxs-lookup"><span data-stu-id="74418-113">Configuration</span></span>](xref:security/data-protection/configuration/index)

  * [<span data-ttu-id="74418-114">設定 ASP.NET Core 資料保護</span><span class="sxs-lookup"><span data-stu-id="74418-114">Configure ASP.NET Core Data Protection</span></span>](xref:security/data-protection/configuration/overview)

  * [<span data-ttu-id="74418-115">預設設定</span><span class="sxs-lookup"><span data-stu-id="74418-115">Default settings</span></span>](xref:security/data-protection/configuration/default-settings)

  * [<span data-ttu-id="74418-116">整個電腦的原則</span><span class="sxs-lookup"><span data-stu-id="74418-116">Machine-wide policy</span></span>](xref:security/data-protection/configuration/machine-wide-policy)

  * [<span data-ttu-id="74418-117">非 DI 感知案例</span><span class="sxs-lookup"><span data-stu-id="74418-117">Non DI-aware scenarios</span></span>](xref:security/data-protection/configuration/non-di-scenarios)

* [<span data-ttu-id="74418-118">擴充性 API</span><span class="sxs-lookup"><span data-stu-id="74418-118">Extensibility APIs</span></span>](xref:security/data-protection/extensibility/index)

  * [<span data-ttu-id="74418-119">核心加密擴充性</span><span class="sxs-lookup"><span data-stu-id="74418-119">Core cryptography extensibility</span></span>](xref:security/data-protection/extensibility/core-crypto)

  * [<span data-ttu-id="74418-120">金鑰管理擴充性</span><span class="sxs-lookup"><span data-stu-id="74418-120">Key management extensibility</span></span>](xref:security/data-protection/extensibility/key-management)

  * [<span data-ttu-id="74418-121">其他 API</span><span class="sxs-lookup"><span data-stu-id="74418-121">Miscellaneous APIs</span></span>](xref:security/data-protection/extensibility/misc-apis)

* [<span data-ttu-id="74418-122">實作</span><span class="sxs-lookup"><span data-stu-id="74418-122">Implementation</span></span>](xref:security/data-protection/implementation/index)

  * [<span data-ttu-id="74418-123">已驗證的加密詳細資料</span><span class="sxs-lookup"><span data-stu-id="74418-123">Authenticated encryption details</span></span>](xref:security/data-protection/implementation/authenticated-encryption-details)

  * [<span data-ttu-id="74418-124">子機碼衍生和驗證的加密</span><span class="sxs-lookup"><span data-stu-id="74418-124">Subkey derivation and authenticated encryption</span></span>](xref:security/data-protection/implementation/subkeyderivation)

  * [<span data-ttu-id="74418-125">內容標頭</span><span class="sxs-lookup"><span data-stu-id="74418-125">Context headers</span></span>](xref:security/data-protection/implementation/context-headers)

  * [<span data-ttu-id="74418-126">金鑰管理</span><span class="sxs-lookup"><span data-stu-id="74418-126">Key management</span></span>](xref:security/data-protection/implementation/key-management)

  * [<span data-ttu-id="74418-127">金鑰儲存體提供者</span><span class="sxs-lookup"><span data-stu-id="74418-127">Key storage providers</span></span>](xref:security/data-protection/implementation/key-storage-providers)

  * [<span data-ttu-id="74418-128">待用時加密金鑰</span><span class="sxs-lookup"><span data-stu-id="74418-128">Key encryption at rest</span></span>](xref:security/data-protection/implementation/key-encryption-at-rest)

  * [<span data-ttu-id="74418-129">金鑰的不變性和設定</span><span class="sxs-lookup"><span data-stu-id="74418-129">Key immutability and settings</span></span>](xref:security/data-protection/implementation/key-immutability)

  * [<span data-ttu-id="74418-130">金鑰儲存體格式</span><span class="sxs-lookup"><span data-stu-id="74418-130">Key storage format</span></span>](xref:security/data-protection/implementation/key-storage-format)

  * [<span data-ttu-id="74418-131">暫時資料保護提供者</span><span class="sxs-lookup"><span data-stu-id="74418-131">Ephemeral data protection providers</span></span>](xref:security/data-protection/implementation/key-storage-ephemeral)

* [<span data-ttu-id="74418-132">相容性</span><span class="sxs-lookup"><span data-stu-id="74418-132">Compatibility</span></span>](xref:security/data-protection/compatibility/index)

  * [<span data-ttu-id="74418-133">取代 ASP.NET Core 中的 ASP.NET <machineKey></span><span class="sxs-lookup"><span data-stu-id="74418-133">Replacing ASP.NET <machineKey> in ASP.NET Core</span></span>](xref:security/data-protection/compatibility/replacing-machinekey)
