---
title: "限制受保護的裝載的存留期"
author: rick-anderson
description: "本文件說明如何限制使用 ASP.NET Core 資料保護 Api 的受保護內容的存留期。"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/limited-lifetime-payloads
ms.openlocfilehash: 7909f21057f22e78c03b41464a19a18ce0908216
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
# <a name="limiting-the-lifetime-of-protected-payloads"></a><span data-ttu-id="fb38c-103">限制受保護的裝載的存留期</span><span class="sxs-lookup"><span data-stu-id="fb38c-103">Limiting the lifetime of protected payloads</span></span>

<span data-ttu-id="fb38c-104">沒有應用程式開發人員要建立一段時間之後過期的受保護的內容的案例。</span><span class="sxs-lookup"><span data-stu-id="fb38c-104">There are scenarios where the application developer wants to create a protected payload that expires after a set period of time.</span></span> <span data-ttu-id="fb38c-105">例如，受保護的內容可能代表一小時只能為有效的密碼重設語彙基元。</span><span class="sxs-lookup"><span data-stu-id="fb38c-105">For instance, the protected payload might represent a password reset token that should only be valid for one hour.</span></span> <span data-ttu-id="fb38c-106">很可能建立自己裝載格式，其中包含內嵌的到期日，開發人員和進階開發人員可能會想要這樣做，但對於大部分的開發人員管理這些到期日的成長繁瑣。</span><span class="sxs-lookup"><span data-stu-id="fb38c-106">It's certainly possible for the developer to create their own payload format that contains an embedded expiration date, and advanced developers may wish to do this anyway, but for the majority of developers managing these expirations can grow tedious.</span></span>

<span data-ttu-id="fb38c-107">若要進行簡化我們開發人員對象，封裝[Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/)公用程式 Api 包含用於建立在一段時間後自動過期的裝載。</span><span class="sxs-lookup"><span data-stu-id="fb38c-107">To make this easier for our developer audience, the package [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) contains utility APIs for creating payloads that automatically expire after a set period of time.</span></span> <span data-ttu-id="fb38c-108">這些 Api 的停止回應登出`ITimeLimitedDataProtector`型別。</span><span class="sxs-lookup"><span data-stu-id="fb38c-108">These APIs hang off of the `ITimeLimitedDataProtector` type.</span></span>

## <a name="api-usage"></a><span data-ttu-id="fb38c-109">API 使用方式</span><span class="sxs-lookup"><span data-stu-id="fb38c-109">API usage</span></span>

<span data-ttu-id="fb38c-110">`ITimeLimitedDataProtector`介面是保護和解除時間限制 / 即將過期自我裝載的核心介面。</span><span class="sxs-lookup"><span data-stu-id="fb38c-110">The `ITimeLimitedDataProtector` interface is the core interface for protecting and unprotecting time-limited / self-expiring payloads.</span></span> <span data-ttu-id="fb38c-111">若要建立的執行個體`ITimeLimitedDataProtector`，您必須先執行個體的一般[IDataProtector](overview.md)特定目的所建構。</span><span class="sxs-lookup"><span data-stu-id="fb38c-111">To create an instance of an `ITimeLimitedDataProtector`, you'll first need an instance of a regular [IDataProtector](overview.md) constructed with a specific purpose.</span></span> <span data-ttu-id="fb38c-112">一次`IDataProtector`執行個體可用時，請呼叫`IDataProtector.ToTimeLimitedDataProtector`擴充方法，讓回復保護裝置具有內建到期功能。</span><span class="sxs-lookup"><span data-stu-id="fb38c-112">Once the `IDataProtector` instance is available, call the `IDataProtector.ToTimeLimitedDataProtector` extension method to get back a protector with built-in expiration capabilities.</span></span>

<span data-ttu-id="fb38c-113">`ITimeLimitedDataProtector`會公開下列 API 介面和擴充方法：</span><span class="sxs-lookup"><span data-stu-id="fb38c-113">`ITimeLimitedDataProtector` exposes the following API surface and extension methods:</span></span>

* <span data-ttu-id="fb38c-114">CreateProtector （字串用途）： 這個 API 已類似現有 ITimeLimitedDataProtector- `IDataProtectionProvider.CreateProtector` ，它可以用來建立[用途鏈結](purpose-strings.md)從根時間限制保護裝置。</span><span class="sxs-lookup"><span data-stu-id="fb38c-114">CreateProtector(string purpose) : ITimeLimitedDataProtector - This API is similar to the existing `IDataProtectionProvider.CreateProtector` in that it can be used to create [purpose chains](purpose-strings.md) from a root time-limited protector.</span></span>

* <span data-ttu-id="fb38c-115">Protect(byte[] plaintext, DateTimeOffset expiration) : byte[]</span><span class="sxs-lookup"><span data-stu-id="fb38c-115">Protect(byte[] plaintext, DateTimeOffset expiration) : byte[]</span></span>

* <span data-ttu-id="fb38c-116">保護 （位元組 [純文字、 TimeSpan 存留時間）： byte]</span><span class="sxs-lookup"><span data-stu-id="fb38c-116">Protect(byte[] plaintext, TimeSpan lifetime) : byte[]</span></span>

* <span data-ttu-id="fb38c-117">保護 （位元組 [純文字）： byte]</span><span class="sxs-lookup"><span data-stu-id="fb38c-117">Protect(byte[] plaintext) : byte[]</span></span>

* <span data-ttu-id="fb38c-118">保護 （字串純文字、 DateTimeOffset 到期）： 字串</span><span class="sxs-lookup"><span data-stu-id="fb38c-118">Protect(string plaintext, DateTimeOffset expiration) : string</span></span>

* <span data-ttu-id="fb38c-119">保護 （字串純文字、 TimeSpan 存留時間）： 字串</span><span class="sxs-lookup"><span data-stu-id="fb38c-119">Protect(string plaintext, TimeSpan lifetime) : string</span></span>

* <span data-ttu-id="fb38c-120">保護 （字串純文字）： 字串</span><span class="sxs-lookup"><span data-stu-id="fb38c-120">Protect(string plaintext) : string</span></span>

<span data-ttu-id="fb38c-121">除了核心`Protect`採取只為純文字的方法有新的多載可讓您指定的內容到期日。</span><span class="sxs-lookup"><span data-stu-id="fb38c-121">In addition to the core `Protect` methods which take only the plaintext, there are new overloads which allow specifying the payload's expiration date.</span></span> <span data-ttu-id="fb38c-122">可以指定到期日為的絕對日期 (透過`DateTimeOffset`) 或相對的時間 (從目前系統時間，透過`TimeSpan`)。</span><span class="sxs-lookup"><span data-stu-id="fb38c-122">The expiration date can be specified as an absolute date (via a `DateTimeOffset`) or as a relative time (from the current system time, via a `TimeSpan`).</span></span> <span data-ttu-id="fb38c-123">如果呼叫時，才會到期的多載，承載會假設永遠不會為過期。</span><span class="sxs-lookup"><span data-stu-id="fb38c-123">If an overload which doesn't take an expiration is called, the payload is assumed never to expire.</span></span>

* <span data-ttu-id="fb38c-124">取消保護 (位元組 [] protectedData，DateTimeOffset 到期時): byte]</span><span class="sxs-lookup"><span data-stu-id="fb38c-124">Unprotect(byte[] protectedData, out DateTimeOffset expiration) : byte[]</span></span>

* <span data-ttu-id="fb38c-125">取消保護 ([] protectedData 位元組): byte]</span><span class="sxs-lookup"><span data-stu-id="fb38c-125">Unprotect(byte[] protectedData) : byte[]</span></span>

* <span data-ttu-id="fb38c-126">取消保護 (DateTimeOffset 到期時，字串 protectedData): 字串</span><span class="sxs-lookup"><span data-stu-id="fb38c-126">Unprotect(string protectedData, out DateTimeOffset expiration) : string</span></span>

* <span data-ttu-id="fb38c-127">取消保護 (字串 protectedData): 字串</span><span class="sxs-lookup"><span data-stu-id="fb38c-127">Unprotect(string protectedData) : string</span></span>

<span data-ttu-id="fb38c-128">`Unprotect`方法會傳回原始未受保護的資料。</span><span class="sxs-lookup"><span data-stu-id="fb38c-128">The `Unprotect` methods return the original unprotected data.</span></span> <span data-ttu-id="fb38c-129">如果裝載尚未尚未過期，絕對期限會傳回做為選擇性的 out 參數，以及原始未受保護的資料。</span><span class="sxs-lookup"><span data-stu-id="fb38c-129">If the payload hasn't yet expired, the absolute expiration is returned as an optional out parameter along with the original unprotected data.</span></span> <span data-ttu-id="fb38c-130">如果裝載已過期，取消保護方法的所有多載會擲回 CryptographicException。</span><span class="sxs-lookup"><span data-stu-id="fb38c-130">If the payload is expired, all overloads of the Unprotect method will throw CryptographicException.</span></span>

>[!WARNING]
> <span data-ttu-id="fb38c-131">它已不建議使用這些 Api 來保護裝載需要長期或無限期的持續性。</span><span class="sxs-lookup"><span data-stu-id="fb38c-131">It's not advised to use these APIs to protect payloads which require long-term or indefinite persistence.</span></span> <span data-ttu-id="fb38c-132">「 可我負擔的月份是永久無法復原受保護的內容嗎？ 」</span><span class="sxs-lookup"><span data-stu-id="fb38c-132">"Can I afford for the protected payloads to be permanently unrecoverable after a month?"</span></span> <span data-ttu-id="fb38c-133">可以做為最佳經驗法則;如果答案是沒有然後開發人員應該考慮替代的 Api。</span><span class="sxs-lookup"><span data-stu-id="fb38c-133">can serve as a good rule of thumb; if the answer is no then developers should consider alternative APIs.</span></span>

<span data-ttu-id="fb38c-134">使用下列的範例[非 DI 程式碼路徑](../configuration/non-di-scenarios.md)具現化的資料保護系統。</span><span class="sxs-lookup"><span data-stu-id="fb38c-134">The sample below uses the [non-DI code paths](../configuration/non-di-scenarios.md) for instantiating the data protection system.</span></span> <span data-ttu-id="fb38c-135">若要執行此範例，請確認您有第一次加入 Microsoft.AspNetCore.DataProtection.Extensions 封裝的參考。</span><span class="sxs-lookup"><span data-stu-id="fb38c-135">To run this sample, ensure that you have first added a reference to the Microsoft.AspNetCore.DataProtection.Extensions package.</span></span>

[!code-csharp[Main](limited-lifetime-payloads/samples/limitedlifetimepayloads.cs)]
