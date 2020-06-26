---
title: 開始使用 ASP.NET Core 中的資料保護 Api
author: rick-anderson
description: 瞭解如何使用 ASP.NET Core 的資料保護 Api 來保護和解除保護應用程式中的資料。
ms.author: riande
ms.date: 11/12/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/data-protection/using-data-protection
ms.openlocfilehash: 1b0dc6756de55d9ce35eb08ca037e4d4b1fede75
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85405610"
---
# <a name="get-started-with-the-data-protection-apis-in-aspnet-core"></a><span data-ttu-id="3eec5-103">開始使用 ASP.NET Core 中的資料保護 Api</span><span class="sxs-lookup"><span data-stu-id="3eec5-103">Get started with the Data Protection APIs in ASP.NET Core</span></span>

<a name="security-data-protection-getting-started"></a>

<span data-ttu-id="3eec5-104">最簡單的是，保護資料是由下列步驟所組成：</span><span class="sxs-lookup"><span data-stu-id="3eec5-104">At its simplest, protecting data consists of the following steps:</span></span>

1. <span data-ttu-id="3eec5-105">從資料保護提供者建立資料保護裝置。</span><span class="sxs-lookup"><span data-stu-id="3eec5-105">Create a data protector from a data protection provider.</span></span>

2. <span data-ttu-id="3eec5-106">`Protect`使用您想要保護的資料呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="3eec5-106">Call the `Protect` method with the data you want to protect.</span></span>

3. <span data-ttu-id="3eec5-107">`Unprotect`使用您想要轉換成純文字的資料來呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="3eec5-107">Call the `Unprotect` method with the data you want to turn back into plain text.</span></span>

<span data-ttu-id="3eec5-108">大部分的架構和應用程式模型（例如 ASP.NET Core 或 SignalR ）已經設定資料保護系統，並將它新增至您透過相依性插入存取的服務容器。</span><span class="sxs-lookup"><span data-stu-id="3eec5-108">Most frameworks and app models, such as ASP.NET Core or SignalR, already configure the data protection system and add it to a service container you access via dependency injection.</span></span> <span data-ttu-id="3eec5-109">下列範例示範如何設定服務容器以進行相依性插入和註冊資料保護堆疊、透過 DI 接收資料保護提供者、建立保護裝置，以及保護然後解除保護資料。</span><span class="sxs-lookup"><span data-stu-id="3eec5-109">The following sample demonstrates configuring a service container for dependency injection and registering the data protection stack, receiving the data protection provider via DI, creating a protector and protecting then unprotecting data.</span></span>

[!code-csharp[](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

<span data-ttu-id="3eec5-110">當您建立保護裝置時，必須提供一或多個[目的字串](xref:security/data-protection/consumer-apis/purpose-strings)。</span><span class="sxs-lookup"><span data-stu-id="3eec5-110">When you create a protector you must provide one or more [Purpose Strings](xref:security/data-protection/consumer-apis/purpose-strings).</span></span> <span data-ttu-id="3eec5-111">目的字串提供取用者之間的隔離。</span><span class="sxs-lookup"><span data-stu-id="3eec5-111">A purpose string provides isolation between consumers.</span></span> <span data-ttu-id="3eec5-112">例如，使用「綠色」目的字串所建立的保護裝置，將無法取消保護裝置提供的資料，目的為「紫色」。</span><span class="sxs-lookup"><span data-stu-id="3eec5-112">For example, a protector created with a purpose string of "green" wouldn't be able to unprotect data provided by a protector with a purpose of "purple".</span></span>

>[!TIP]
> <span data-ttu-id="3eec5-113">和的 `IDataProtectionProvider` 實例 `IDataProtector` 是多個呼叫端的安全線程。</span><span class="sxs-lookup"><span data-stu-id="3eec5-113">Instances of `IDataProtectionProvider` and `IDataProtector` are thread-safe for multiple callers.</span></span> <span data-ttu-id="3eec5-114">這是因為一旦元件透過呼叫取得的參考 `IDataProtector` `CreateProtector` ，它就會使用該參考進行對和的多次呼叫 `Protect` `Unprotect` 。</span><span class="sxs-lookup"><span data-stu-id="3eec5-114">It's intended that once a component gets a reference to an `IDataProtector` via a call to `CreateProtector`, it will use that reference for multiple calls to `Protect` and `Unprotect`.</span></span>
>
><span data-ttu-id="3eec5-115">`Unprotect`如果無法驗證或解密受保護的內容，則的呼叫將會擲回 system.security.cryptography.cryptographicexception。</span><span class="sxs-lookup"><span data-stu-id="3eec5-115">A call to `Unprotect` will throw CryptographicException if the protected payload cannot be verified or deciphered.</span></span> <span data-ttu-id="3eec5-116">某些元件可能會想要在取消保護作業期間忽略錯誤;讀取驗證 cookie 的元件可能會處理此錯誤，並將要求視為完全沒有 cookie，而不是直接讓要求失敗。</span><span class="sxs-lookup"><span data-stu-id="3eec5-116">Some components may wish to ignore errors during unprotect operations; a component which reads authentication cookies might handle this error and treat the request as if it had no cookie at all rather than fail the request outright.</span></span> <span data-ttu-id="3eec5-117">需要此行為的元件應該特別捕捉 System.security.cryptography.cryptographicexception，而不是抑制所有例外狀況。</span><span class="sxs-lookup"><span data-stu-id="3eec5-117">Components which want this behavior should specifically catch CryptographicException instead of swallowing all exceptions.</span></span>
