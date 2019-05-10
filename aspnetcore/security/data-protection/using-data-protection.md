---
title: 開始使用 ASP.NET Core 中的資料保護 Api
author: rick-anderson
description: 了解如何保護和解除資料的應用程式中使用 ASP.NET Core 資料保護 Api。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/using-data-protection
ms.openlocfilehash: 25bf099a3d9edd7e6e0872725cbc3707750314e6
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087644"
---
# <a name="get-started-with-the-data-protection-apis-in-aspnet-core"></a><span data-ttu-id="04cc2-103">開始使用 ASP.NET Core 中的資料保護 Api</span><span class="sxs-lookup"><span data-stu-id="04cc2-103">Get started with the Data Protection APIs in ASP.NET Core</span></span>

<a name="security-data-protection-getting-started"></a>

<span data-ttu-id="04cc2-104">在其最簡單、 保護資料包含下列步驟：</span><span class="sxs-lookup"><span data-stu-id="04cc2-104">At its simplest, protecting data consists of the following steps:</span></span>

1. <span data-ttu-id="04cc2-105">從資料保護提供者建立資料保護裝置。</span><span class="sxs-lookup"><span data-stu-id="04cc2-105">Create a data protector from a data protection provider.</span></span>

2. <span data-ttu-id="04cc2-106">呼叫`Protect`與您想要保護的資料的方法。</span><span class="sxs-lookup"><span data-stu-id="04cc2-106">Call the `Protect` method with the data you want to protect.</span></span>

3. <span data-ttu-id="04cc2-107">呼叫`Unprotect`要變成純文字資料的方法。</span><span class="sxs-lookup"><span data-stu-id="04cc2-107">Call the `Unprotect` method with the data you want to turn back into plain text.</span></span>

<span data-ttu-id="04cc2-108">大部分的架構和應用程式模型，例如 ASP.NET Core 或 SignalR，已設定資料保護系統，並將它新增至您存取透過相依性插入服務容器。</span><span class="sxs-lookup"><span data-stu-id="04cc2-108">Most frameworks and app models, such as ASP.NET Core or SignalR, already configure the data protection system and add it to a service container you access via dependency injection.</span></span> <span data-ttu-id="04cc2-109">下列範例會示範設定相依性插入的服務容器和註冊資料保護堆疊、 接收透過 DI 的資料保護提供者、 建立保護裝置和保護，則正在解除保護的資料。</span><span class="sxs-lookup"><span data-stu-id="04cc2-109">The following sample demonstrates configuring a service container for dependency injection and registering the data protection stack, receiving the data protection provider via DI, creating a protector and protecting then unprotecting data.</span></span>

[!code-csharp[](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

<span data-ttu-id="04cc2-110">當您建立的保護裝置時您必須提供一或多個[目的字串](xref:security/data-protection/consumer-apis/purpose-strings)。</span><span class="sxs-lookup"><span data-stu-id="04cc2-110">When you create a protector you must provide one or more [Purpose Strings](xref:security/data-protection/consumer-apis/purpose-strings).</span></span> <span data-ttu-id="04cc2-111">目的字串提供取用者之間的隔離。</span><span class="sxs-lookup"><span data-stu-id="04cc2-111">A purpose string provides isolation between consumers.</span></span> <span data-ttu-id="04cc2-112">比方說，使用 「 綠色 」 的目的字串建立的保護裝置將無法取消保護 「 紫色 」 的目的在於提供保護的資料。</span><span class="sxs-lookup"><span data-stu-id="04cc2-112">For example, a protector created with a purpose string of "green" wouldn't be able to unprotect data provided by a protector with a purpose of "purple".</span></span>

>[!TIP]
> <span data-ttu-id="04cc2-113">執行個體`IDataProtectionProvider`和`IDataProtector`是安全執行緒的多個呼叫端。</span><span class="sxs-lookup"><span data-stu-id="04cc2-113">Instances of `IDataProtectionProvider` and `IDataProtector` are thread-safe for multiple callers.</span></span> <span data-ttu-id="04cc2-114">其目的是，一旦元件取得的參考`IDataProtector`透過呼叫`CreateProtector`，它會使用該參考的多個呼叫`Protect`和`Unprotect`。</span><span class="sxs-lookup"><span data-stu-id="04cc2-114">It's intended that once a component gets a reference to an `IDataProtector` via a call to `CreateProtector`, it will use that reference for multiple calls to `Protect` and `Unprotect`.</span></span>
>
><span data-ttu-id="04cc2-115">呼叫`Unprotect`將會擲回 CryptographicException，如果無法驗證或用來解密受保護的內容。</span><span class="sxs-lookup"><span data-stu-id="04cc2-115">A call to `Unprotect` will throw CryptographicException if the protected payload cannot be verified or deciphered.</span></span> <span data-ttu-id="04cc2-116">某些元件可能會想要忽略錯誤期間取消保護作業;讀取驗證 cookie 的元件可能會處理此錯誤和處理要求，如同它有完全沒有 cookie 而非直接讓要求失敗。</span><span class="sxs-lookup"><span data-stu-id="04cc2-116">Some components may wish to ignore errors during unprotect operations; a component which reads authentication cookies might handle this error and treat the request as if it had no cookie at all rather than fail the request outright.</span></span> <span data-ttu-id="04cc2-117">想要此行為元件特別應該攔截 CryptographicException，而不是抑制所有的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="04cc2-117">Components which want this behavior should specifically catch CryptographicException instead of swallowing all exceptions.</span></span>
