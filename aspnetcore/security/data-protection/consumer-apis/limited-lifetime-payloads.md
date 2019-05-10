---
title: ASP.NET Core 中的受保護承載的存留期的限制
author: rick-anderson
description: 了解如何限制使用 ASP.NET Core 資料保護 Api 的受保護承載的存留期。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/limited-lifetime-payloads
ms.openlocfilehash: 8dc3b856ec67477ec8ae777749c9bf3107eb4eda
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897115"
---
# <a name="limit-the-lifetime-of-protected-payloads-in-aspnet-core"></a><span data-ttu-id="e9c28-103">ASP.NET Core 中的受保護承載的存留期的限制</span><span class="sxs-lookup"><span data-stu-id="e9c28-103">Limit the lifetime of protected payloads in ASP.NET Core</span></span>

<span data-ttu-id="e9c28-104">有些應用程式開發人員要建立會在一段時間後到期的受保護的承載的情況。</span><span class="sxs-lookup"><span data-stu-id="e9c28-104">There are scenarios where the application developer wants to create a protected payload that expires after a set period of time.</span></span> <span data-ttu-id="e9c28-105">比方說，受保護的內容可能代表應該有效時間是一小時的密碼重設語彙基元。</span><span class="sxs-lookup"><span data-stu-id="e9c28-105">For instance, the protected payload might represent a password reset token that should only be valid for one hour.</span></span> <span data-ttu-id="e9c28-106">當然可以建立自己裝載格式，其中包含內嵌的到期日，讓開發人員和進階開發人員可能會想要這樣做，但對於大部分的開發人員管理這些到期期限可以成長單調乏味。</span><span class="sxs-lookup"><span data-stu-id="e9c28-106">It's certainly possible for the developer to create their own payload format that contains an embedded expiration date, and advanced developers may wish to do this anyway, but for the majority of developers managing these expirations can grow tedious.</span></span>

<span data-ttu-id="e9c28-107">為了簡化起見針對我們的開發人員對象，封裝[Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/)包含公用程式 Api 來建立裝載在一段時間後自動過期。</span><span class="sxs-lookup"><span data-stu-id="e9c28-107">To make this easier for our developer audience, the package [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) contains utility APIs for creating payloads that automatically expire after a set period of time.</span></span> <span data-ttu-id="e9c28-108">這些 Api 的停止回應登出`ITimeLimitedDataProtector`型別。</span><span class="sxs-lookup"><span data-stu-id="e9c28-108">These APIs hang off of the `ITimeLimitedDataProtector` type.</span></span>

## <a name="api-usage"></a><span data-ttu-id="e9c28-109">API 使用量</span><span class="sxs-lookup"><span data-stu-id="e9c28-109">API usage</span></span>

<span data-ttu-id="e9c28-110">`ITimeLimitedDataProtector`介面是保護和解除期限 / 即將過期自我裝載的核心介面。</span><span class="sxs-lookup"><span data-stu-id="e9c28-110">The `ITimeLimitedDataProtector` interface is the core interface for protecting and unprotecting time-limited / self-expiring payloads.</span></span> <span data-ttu-id="e9c28-111">若要建立的執行個體`ITimeLimitedDataProtector`，您必須先執行個體的一般[IDataProtector](xref:security/data-protection/consumer-apis/overview)建構具有特定用途。</span><span class="sxs-lookup"><span data-stu-id="e9c28-111">To create an instance of an `ITimeLimitedDataProtector`, you'll first need an instance of a regular [IDataProtector](xref:security/data-protection/consumer-apis/overview) constructed with a specific purpose.</span></span> <span data-ttu-id="e9c28-112">一次`IDataProtector`執行個體可用，請呼叫`IDataProtector.ToTimeLimitedDataProtector`內建到期功能取回保護裝置的擴充方法。</span><span class="sxs-lookup"><span data-stu-id="e9c28-112">Once the `IDataProtector` instance is available, call the `IDataProtector.ToTimeLimitedDataProtector` extension method to get back a protector with built-in expiration capabilities.</span></span>

<span data-ttu-id="e9c28-113">`ITimeLimitedDataProtector` 會公開下列的 API 介面和擴充方法：</span><span class="sxs-lookup"><span data-stu-id="e9c28-113">`ITimeLimitedDataProtector` exposes the following API surface and extension methods:</span></span>

* <span data-ttu-id="e9c28-114">CreateProtector （字串用途）：ITimeLimitedDataProtector-此 API 是類似於現有`IDataProtectionProvider.CreateProtector`在於它可以用來建立[用途鏈結](xref:security/data-protection/consumer-apis/purpose-strings)從根期限保護裝置。</span><span class="sxs-lookup"><span data-stu-id="e9c28-114">CreateProtector(string purpose) : ITimeLimitedDataProtector - This API is similar to the existing `IDataProtectionProvider.CreateProtector` in that it can be used to create [purpose chains](xref:security/data-protection/consumer-apis/purpose-strings) from a root time-limited protector.</span></span>

* <span data-ttu-id="e9c28-115">Protect(byte[] plaintext, DateTimeOffset expiration) : byte[]</span><span class="sxs-lookup"><span data-stu-id="e9c28-115">Protect(byte[] plaintext, DateTimeOffset expiration) : byte[]</span></span>

* <span data-ttu-id="e9c28-116">Protect(byte[] plaintext, TimeSpan lifetime) : byte[]</span><span class="sxs-lookup"><span data-stu-id="e9c28-116">Protect(byte[] plaintext, TimeSpan lifetime) : byte[]</span></span>

* <span data-ttu-id="e9c28-117">Protect(byte[] plaintext) : byte[]</span><span class="sxs-lookup"><span data-stu-id="e9c28-117">Protect(byte[] plaintext) : byte[]</span></span>

* <span data-ttu-id="e9c28-118">保護 （字串純文字，DateTimeOffset 到期）： 字串</span><span class="sxs-lookup"><span data-stu-id="e9c28-118">Protect(string plaintext, DateTimeOffset expiration) : string</span></span>

* <span data-ttu-id="e9c28-119">保護 （字串的純文字、 時間範圍存留期）： 字串</span><span class="sxs-lookup"><span data-stu-id="e9c28-119">Protect(string plaintext, TimeSpan lifetime) : string</span></span>

* <span data-ttu-id="e9c28-120">保護 （字串純文字）： 字串</span><span class="sxs-lookup"><span data-stu-id="e9c28-120">Protect(string plaintext) : string</span></span>

<span data-ttu-id="e9c28-121">除了核心`Protect`方法這需要僅純文字，有新的多載，可指定內容的到期日。</span><span class="sxs-lookup"><span data-stu-id="e9c28-121">In addition to the core `Protect` methods which take only the plaintext, there are new overloads which allow specifying the payload's expiration date.</span></span> <span data-ttu-id="e9c28-122">可以指定到期日為的絕對日期 (透過`DateTimeOffset`) 或相對的時間 (從目前系統時間，透過`TimeSpan`)。</span><span class="sxs-lookup"><span data-stu-id="e9c28-122">The expiration date can be specified as an absolute date (via a `DateTimeOffset`) or as a relative time (from the current system time, via a `TimeSpan`).</span></span> <span data-ttu-id="e9c28-123">如果呼叫多載，其不會到期，承載會假設永遠不會過期。</span><span class="sxs-lookup"><span data-stu-id="e9c28-123">If an overload which doesn't take an expiration is called, the payload is assumed never to expire.</span></span>

* <span data-ttu-id="e9c28-124">Unprotect(byte[] protectedData, out DateTimeOffset expiration) : byte[]</span><span class="sxs-lookup"><span data-stu-id="e9c28-124">Unprotect(byte[] protectedData, out DateTimeOffset expiration) : byte[]</span></span>

* <span data-ttu-id="e9c28-125">Unprotect(byte[] protectedData) : byte[]</span><span class="sxs-lookup"><span data-stu-id="e9c28-125">Unprotect(byte[] protectedData) : byte[]</span></span>

* <span data-ttu-id="e9c28-126">取消保護 (DateTimeOffset 到期時，字串 protectedData): 字串</span><span class="sxs-lookup"><span data-stu-id="e9c28-126">Unprotect(string protectedData, out DateTimeOffset expiration) : string</span></span>

* <span data-ttu-id="e9c28-127">取消保護 (字串 protectedData): 字串</span><span class="sxs-lookup"><span data-stu-id="e9c28-127">Unprotect(string protectedData) : string</span></span>

<span data-ttu-id="e9c28-128">`Unprotect`方法會傳回原始未受保護的資料。</span><span class="sxs-lookup"><span data-stu-id="e9c28-128">The `Unprotect` methods return the original unprotected data.</span></span> <span data-ttu-id="e9c28-129">如果裝載尚未尚未過期，絕對期限會傳回做為選擇性的 out 參數，以及原始的未受保護資料。</span><span class="sxs-lookup"><span data-stu-id="e9c28-129">If the payload hasn't yet expired, the absolute expiration is returned as an optional out parameter along with the original unprotected data.</span></span> <span data-ttu-id="e9c28-130">如果裝載已過期，所有多載的 Unprotect 方法會擲回 CryptographicException。</span><span class="sxs-lookup"><span data-stu-id="e9c28-130">If the payload is expired, all overloads of the Unprotect method will throw CryptographicException.</span></span>

>[!WARNING]
> <span data-ttu-id="e9c28-131">不建議使用這些 Api 來保護裝載需要長期或無限期的持續性。</span><span class="sxs-lookup"><span data-stu-id="e9c28-131">It's not advised to use these APIs to protect payloads which require long-term or indefinite persistence.</span></span> <span data-ttu-id="e9c28-132">「 我經得起受保護的承載是一個月之後的 永久無法復原的？ 」</span><span class="sxs-lookup"><span data-stu-id="e9c28-132">"Can I afford for the protected payloads to be permanently unrecoverable after a month?"</span></span> <span data-ttu-id="e9c28-133">可做為法則;如果答案是沒有然後開發人員應該考慮替代的 Api。</span><span class="sxs-lookup"><span data-stu-id="e9c28-133">can serve as a good rule of thumb; if the answer is no then developers should consider alternative APIs.</span></span>

<span data-ttu-id="e9c28-134">下列範例使用[非 DI 的程式碼路徑](xref:security/data-protection/configuration/non-di-scenarios)具現化的資料保護系統。</span><span class="sxs-lookup"><span data-stu-id="e9c28-134">The sample below uses the [non-DI code paths](xref:security/data-protection/configuration/non-di-scenarios) for instantiating the data protection system.</span></span> <span data-ttu-id="e9c28-135">若要執行此範例，請確定您已先新增 Microsoft.AspNetCore.DataProtection.Extensions 套件的參考。</span><span class="sxs-lookup"><span data-stu-id="e9c28-135">To run this sample, ensure that you have first added a reference to the Microsoft.AspNetCore.DataProtection.Extensions package.</span></span>

[!code-csharp[](limited-lifetime-payloads/samples/limitedlifetimepayloads.cs)]
