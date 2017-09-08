---
title: "非 DI 注意案例"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: a7d8a962-80ff-48e3-96f6-8472b7ba2df9
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/non-di-scenarios
ms.openlocfilehash: 54a930c26f9f48ea0e6f7865e2927bcde0f4d6c0
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2017
---
# <a name="non-di-aware-scenarios"></a><span data-ttu-id="fef61-103">非 DI 注意案例</span><span class="sxs-lookup"><span data-stu-id="fef61-103">Non DI aware scenarios</span></span>

<span data-ttu-id="fef61-104">通常設計的資料保護系統[要加入至服務容器](../consumer-apis/overview.md)和要提供給透過 DI 機制相依元件。</span><span class="sxs-lookup"><span data-stu-id="fef61-104">The data protection system is normally designed [to be added to a service container](../consumer-apis/overview.md) and to be provided to dependent components via a DI mechanism.</span></span> <span data-ttu-id="fef61-105">不過，可能有某些情況下，這並不可行，尤其是系統匯入現有的應用程式。</span><span class="sxs-lookup"><span data-stu-id="fef61-105">However, there may be some cases where this is not feasible, especially when importing the system into an existing application.</span></span>

<span data-ttu-id="fef61-106">為了支援這些案例封裝 Microsoft.AspNetCore.DataProtection.Extensions 提供 DataProtectionProvider 它提供了簡單的方式來使用資料保護系統，而不需要透過 DI 特定程式碼路徑的具象類型。</span><span class="sxs-lookup"><span data-stu-id="fef61-106">To support these scenarios the package Microsoft.AspNetCore.DataProtection.Extensions provides a concrete type DataProtectionProvider which offers a simple way to use the data protection system without going through DI-specific code paths.</span></span> <span data-ttu-id="fef61-107">型別本身實作 IDataProtectionProvider，並建構它相當簡單，只要提供 DirectoryInfo 儲存提供者的密碼編譯金鑰的位置。</span><span class="sxs-lookup"><span data-stu-id="fef61-107">The type itself implements IDataProtectionProvider, and constructing it is as easy as providing a DirectoryInfo where this provider's cryptographic keys should be stored.</span></span>

<span data-ttu-id="fef61-108">例如: </span><span class="sxs-lookup"><span data-stu-id="fef61-108">For example:</span></span>

<span data-ttu-id="fef61-109">[!code-none[Main](non-di-scenarios/_static/nodisample1.cs)]</span><span class="sxs-lookup"><span data-stu-id="fef61-109">[!code-none[Main](non-di-scenarios/_static/nodisample1.cs)]</span></span>

>[!WARNING]
> <span data-ttu-id="fef61-110">根據預設 DataProtectionProvider 具象型別不會加密原始金鑰內容之前將它保存到檔案系統。</span><span class="sxs-lookup"><span data-stu-id="fef61-110">By default the DataProtectionProvider concrete type does not encrypt raw key material before persisting it to the file system.</span></span> <span data-ttu-id="fef61-111">這是為了支援的案例，其中網路指向的開發人員共用時，資料保護系統，無法在此情況下會自動推算在其餘的適當的金鑰加密機制。</span><span class="sxs-lookup"><span data-stu-id="fef61-111">This is to support scenarios where the developer points to a network share, in which case the data protection system cannot automatically deduce an appropriate at-rest key encryption mechanism.</span></span>
>
><span data-ttu-id="fef61-112">此外，DataProtectionProvider 具象型別並不會[隔離應用程式](overview.md#data-protection-configuration-per-app-isolation)依預設，因此所有指向相同的索引鍵目錄的應用程式可以共用裝載，只要符合其用途的參數。</span><span class="sxs-lookup"><span data-stu-id="fef61-112">Additionally, the DataProtectionProvider concrete type does not [isolate applications](overview.md#data-protection-configuration-per-app-isolation) by default, so all applications pointed at the same key directory can share payloads as long as their purpose parameters match.</span></span>

<span data-ttu-id="fef61-113">如有需要，應用程式開發人員可以這兩個位址。</span><span class="sxs-lookup"><span data-stu-id="fef61-113">The application developer can address both of these if desired.</span></span> <span data-ttu-id="fef61-114">DataProtectionProvider 建構函式接受[選擇性的組態回呼](overview.md#data-protection-configuration-callback)可用來調整系統的行為。</span><span class="sxs-lookup"><span data-stu-id="fef61-114">The DataProtectionProvider constructor accepts an [optional configuration callback](overview.md#data-protection-configuration-callback) which can be used to tweak the behaviors of the system.</span></span> <span data-ttu-id="fef61-115">下列範例示範如何透過明確呼叫 SetApplicationName，還原隔離，它也會示範設定系統以自動加密使用 Windows DPAPI 保存的金鑰。</span><span class="sxs-lookup"><span data-stu-id="fef61-115">The sample below demonstrates restoring isolation via an explicit call to SetApplicationName, and it also demonstrates configuring the system to automatically encrypt persisted keys using Windows DPAPI.</span></span> <span data-ttu-id="fef61-116">如果目錄指向 UNC 共用，您可能想所有相關的電腦上發佈共用的憑證，並設定改為使用憑證加密透過呼叫系統[ProtectKeysWithCertificate](overview.md#configuring-x509-certificate)。</span><span class="sxs-lookup"><span data-stu-id="fef61-116">If the directory points to a UNC share, you may wish to distribute a shared certificate across all relevant machines and to configure the system to use certificate-based encryption instead via a call to [ProtectKeysWithCertificate](overview.md#configuring-x509-certificate).</span></span>

<span data-ttu-id="fef61-117">[!code-none[Main](non-di-scenarios/_static/nodisample2.cs)]</span><span class="sxs-lookup"><span data-stu-id="fef61-117">[!code-none[Main](non-di-scenarios/_static/nodisample2.cs)]</span></span>

>[!TIP]
> <span data-ttu-id="fef61-118">DataProtectionProvider 具象類型的執行個體是相當費時建立。</span><span class="sxs-lookup"><span data-stu-id="fef61-118">Instances of the DataProtectionProvider concrete type are expensive to create.</span></span> <span data-ttu-id="fef61-119">如果應用程式會維護此類型的多個執行個體，而且它們所有指向相同的金鑰儲存目錄，可能會降低應用程式的效能。</span><span class="sxs-lookup"><span data-stu-id="fef61-119">If an application maintains multiple instances of this type and if they're all pointing at the same key storage directory, application performance may be degraded.</span></span> <span data-ttu-id="fef61-120">預定的使用方式是應用程式開發人員一次將此類型具現化，則保留重複使用這個單一參考盡量。</span><span class="sxs-lookup"><span data-stu-id="fef61-120">The intended usage is that the application developer instantiate this type once then keep reusing this single reference as much as possible.</span></span> <span data-ttu-id="fef61-121">DataProtectionProvider 型別和從它建立的所有 IDataProtector 執行個體是安全執行緒的多個呼叫端。</span><span class="sxs-lookup"><span data-stu-id="fef61-121">The DataProtectionProvider type and all IDataProtector instances created from it are thread-safe for multiple callers.</span></span>
