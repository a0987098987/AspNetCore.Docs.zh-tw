---
title: 限制 ASP.NET Core 中受保護承載的存留期
author: rick-anderson
description: 瞭解如何使用 ASP.NET Core 的資料保護 Api 來限制受保護承載的存留期。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/limited-lifetime-payloads
ms.openlocfilehash: 8dc3b856ec67477ec8ae777749c9bf3107eb4eda
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78656055"
---
# <a name="limit-the-lifetime-of-protected-payloads-in-aspnet-core"></a><span data-ttu-id="a73bb-103">限制 ASP.NET Core 中受保護承載的存留期</span><span class="sxs-lookup"><span data-stu-id="a73bb-103">Limit the lifetime of protected payloads in ASP.NET Core</span></span>

<span data-ttu-id="a73bb-104">在某些情況下，應用程式開發人員想要建立在一段時間後過期的受保護承載。</span><span class="sxs-lookup"><span data-stu-id="a73bb-104">There are scenarios where the application developer wants to create a protected payload that expires after a set period of time.</span></span> <span data-ttu-id="a73bb-105">比方說，受保護的承載可能代表密碼重設權杖，其應只在一小時內有效。</span><span class="sxs-lookup"><span data-stu-id="a73bb-105">For instance, the protected payload might represent a password reset token that should only be valid for one hour.</span></span> <span data-ttu-id="a73bb-106">開發人員當然可以建立自己的裝載格式，其中包含內嵌的到期日，而先進的開發人員也可能想要這麼做，但對於管理這些到期的大多數開發人員來說，這種情況會變得很繁瑣。</span><span class="sxs-lookup"><span data-stu-id="a73bb-106">It's certainly possible for the developer to create their own payload format that contains an embedded expiration date, and advanced developers may wish to do this anyway, but for the majority of developers managing these expirations can grow tedious.</span></span>

<span data-ttu-id="a73bb-107">為了讓開發人員更容易使用， [AspNetCore. DataProtection 副檔名](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/)包含公用程式 api，可用於建立在一段時間後自動到期的裝載。</span><span class="sxs-lookup"><span data-stu-id="a73bb-107">To make this easier for our developer audience, the package [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) contains utility APIs for creating payloads that automatically expire after a set period of time.</span></span> <span data-ttu-id="a73bb-108">這些 Api 會使 `ITimeLimitedDataProtector` 類型停止回應。</span><span class="sxs-lookup"><span data-stu-id="a73bb-108">These APIs hang off of the `ITimeLimitedDataProtector` type.</span></span>

## <a name="api-usage"></a><span data-ttu-id="a73bb-109">API 使用方式</span><span class="sxs-lookup"><span data-stu-id="a73bb-109">API usage</span></span>

<span data-ttu-id="a73bb-110">`ITimeLimitedDataProtector` 介面是用來保護和解除保護時間限制/自我過期承載的核心介面。</span><span class="sxs-lookup"><span data-stu-id="a73bb-110">The `ITimeLimitedDataProtector` interface is the core interface for protecting and unprotecting time-limited / self-expiring payloads.</span></span> <span data-ttu-id="a73bb-111">若要建立 `ITimeLimitedDataProtector`的實例，您必須先使用以特定目的所建立的一般[idataprotector 加密](xref:security/data-protection/consumer-apis/overview)實例。</span><span class="sxs-lookup"><span data-stu-id="a73bb-111">To create an instance of an `ITimeLimitedDataProtector`, you'll first need an instance of a regular [IDataProtector](xref:security/data-protection/consumer-apis/overview) constructed with a specific purpose.</span></span> <span data-ttu-id="a73bb-112">一旦 `IDataProtector` 實例可供使用，請呼叫 `IDataProtector.ToTimeLimitedDataProtector` 擴充方法，以使用內建的到期功能來取回保護裝置。</span><span class="sxs-lookup"><span data-stu-id="a73bb-112">Once the `IDataProtector` instance is available, call the `IDataProtector.ToTimeLimitedDataProtector` extension method to get back a protector with built-in expiration capabilities.</span></span>

<span data-ttu-id="a73bb-113">`ITimeLimitedDataProtector` 會公開下列 API 介面和擴充方法：</span><span class="sxs-lookup"><span data-stu-id="a73bb-113">`ITimeLimitedDataProtector` exposes the following API surface and extension methods:</span></span>

* <span data-ttu-id="a73bb-114">CreateProtector （字串用途）： ITimeLimitedDataProtector-此 API 類似現有的 `IDataProtectionProvider.CreateProtector`，可用來從根時間限制的保護裝置建立[目的鏈](xref:security/data-protection/consumer-apis/purpose-strings)。</span><span class="sxs-lookup"><span data-stu-id="a73bb-114">CreateProtector(string purpose) : ITimeLimitedDataProtector - This API is similar to the existing `IDataProtectionProvider.CreateProtector` in that it can be used to create [purpose chains](xref:security/data-protection/consumer-apis/purpose-strings) from a root time-limited protector.</span></span>

* <span data-ttu-id="a73bb-115">保護（byte [] 純文字，DateTimeOffset 過期）： byte []</span><span class="sxs-lookup"><span data-stu-id="a73bb-115">Protect(byte[] plaintext, DateTimeOffset expiration) : byte[]</span></span>

* <span data-ttu-id="a73bb-116">保護（byte [] 純文字，TimeSpan 存留期）： byte []</span><span class="sxs-lookup"><span data-stu-id="a73bb-116">Protect(byte[] plaintext, TimeSpan lifetime) : byte[]</span></span>

* <span data-ttu-id="a73bb-117">保護（byte [] 純文字）： byte []</span><span class="sxs-lookup"><span data-stu-id="a73bb-117">Protect(byte[] plaintext) : byte[]</span></span>

* <span data-ttu-id="a73bb-118">保護（字串純文字、DateTimeOffset 過期）：字串</span><span class="sxs-lookup"><span data-stu-id="a73bb-118">Protect(string plaintext, DateTimeOffset expiration) : string</span></span>

* <span data-ttu-id="a73bb-119">保護（字串純文字，TimeSpan 存留期）：字串</span><span class="sxs-lookup"><span data-stu-id="a73bb-119">Protect(string plaintext, TimeSpan lifetime) : string</span></span>

* <span data-ttu-id="a73bb-120">保護（字串純文字）：字串</span><span class="sxs-lookup"><span data-stu-id="a73bb-120">Protect(string plaintext) : string</span></span>

<span data-ttu-id="a73bb-121">除了只採用純文字的核心 `Protect` 方法以外，還有新的多載可讓您指定承載的到期日。</span><span class="sxs-lookup"><span data-stu-id="a73bb-121">In addition to the core `Protect` methods which take only the plaintext, there are new overloads which allow specifying the payload's expiration date.</span></span> <span data-ttu-id="a73bb-122">到期日可指定為絕對日期（透過 `DateTimeOffset`）或做為相對時間（從目前的系統時間，透過 `TimeSpan`）。</span><span class="sxs-lookup"><span data-stu-id="a73bb-122">The expiration date can be specified as an absolute date (via a `DateTimeOffset`) or as a relative time (from the current system time, via a `TimeSpan`).</span></span> <span data-ttu-id="a73bb-123">如果呼叫不接受到期的多載，則會假設承載永不過期。</span><span class="sxs-lookup"><span data-stu-id="a73bb-123">If an overload which doesn't take an expiration is called, the payload is assumed never to expire.</span></span>

* <span data-ttu-id="a73bb-124">取消保護（byte [] protectedData，out DateTimeOffset 過期）： byte []</span><span class="sxs-lookup"><span data-stu-id="a73bb-124">Unprotect(byte[] protectedData, out DateTimeOffset expiration) : byte[]</span></span>

* <span data-ttu-id="a73bb-125">取消保護（byte [] protectedData）： byte []</span><span class="sxs-lookup"><span data-stu-id="a73bb-125">Unprotect(byte[] protectedData) : byte[]</span></span>

* <span data-ttu-id="a73bb-126">取消保護（字串 protectedData，輸出 DateTimeOffset 到期）：字串</span><span class="sxs-lookup"><span data-stu-id="a73bb-126">Unprotect(string protectedData, out DateTimeOffset expiration) : string</span></span>

* <span data-ttu-id="a73bb-127">取消保護（字串 protectedData）：字串</span><span class="sxs-lookup"><span data-stu-id="a73bb-127">Unprotect(string protectedData) : string</span></span>

<span data-ttu-id="a73bb-128">`Unprotect` 方法會傳回原始未受保護的資料。</span><span class="sxs-lookup"><span data-stu-id="a73bb-128">The `Unprotect` methods return the original unprotected data.</span></span> <span data-ttu-id="a73bb-129">如果裝載尚未過期，則會以選擇性的 out 參數以及原始未受保護的資料來傳回絕對到期。</span><span class="sxs-lookup"><span data-stu-id="a73bb-129">If the payload hasn't yet expired, the absolute expiration is returned as an optional out parameter along with the original unprotected data.</span></span> <span data-ttu-id="a73bb-130">如果承載已過期，則取消保護方法的所有多載都會擲回 System.security.cryptography.cryptographicexception。</span><span class="sxs-lookup"><span data-stu-id="a73bb-130">If the payload is expired, all overloads of the Unprotect method will throw CryptographicException.</span></span>

>[!WARNING]
> <span data-ttu-id="a73bb-131">不建議使用這些 Api 來保護需要長期或無限持續性的承載。</span><span class="sxs-lookup"><span data-stu-id="a73bb-131">It's not advised to use these APIs to protect payloads which require long-term or indefinite persistence.</span></span> <span data-ttu-id="a73bb-132">「我可以承受受保護的承載在一個月後永久無法復原嗎？」</span><span class="sxs-lookup"><span data-stu-id="a73bb-132">"Can I afford for the protected payloads to be permanently unrecoverable after a month?"</span></span> <span data-ttu-id="a73bb-133">可以做為良好的經驗法則;如果答案為否，則開發人員應該考慮替代 Api。</span><span class="sxs-lookup"><span data-stu-id="a73bb-133">can serve as a good rule of thumb; if the answer is no then developers should consider alternative APIs.</span></span>

<span data-ttu-id="a73bb-134">下列範例會使用[非 DI 程式碼路徑](xref:security/data-protection/configuration/non-di-scenarios)來具現化資料保護系統。</span><span class="sxs-lookup"><span data-stu-id="a73bb-134">The sample below uses the [non-DI code paths](xref:security/data-protection/configuration/non-di-scenarios) for instantiating the data protection system.</span></span> <span data-ttu-id="a73bb-135">若要執行這個範例，請確定您已先加入 AspNetCore. DataProtection 封裝的參考。</span><span class="sxs-lookup"><span data-stu-id="a73bb-135">To run this sample, ensure that you have first added a reference to the Microsoft.AspNetCore.DataProtection.Extensions package.</span></span>

[!code-csharp[](limited-lifetime-payloads/samples/limitedlifetimepayloads.cs)]
