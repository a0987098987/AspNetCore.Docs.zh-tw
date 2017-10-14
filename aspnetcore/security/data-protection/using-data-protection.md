---
title: "開始使用資料保護應用程式開發介面"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 39b7a73c-29d4-4137-b311-49529adcf3cb
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/using-data-protection
ms.openlocfilehash: 9489b55b1de626b77bcbe21cb9678e561b559099
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/13/2017
---
# <a name="getting-started-with-the-data-protection-apis"></a><span data-ttu-id="d92a5-103">開始使用資料保護應用程式開發介面</span><span class="sxs-lookup"><span data-stu-id="d92a5-103">Getting Started with the Data Protection APIs</span></span>

<a name="security-data-protection-getting-started"></a>

<span data-ttu-id="d92a5-104">在其最簡單的保護資料包含下列步驟：</span><span class="sxs-lookup"><span data-stu-id="d92a5-104">At its simplest protecting data consists of the following steps:</span></span>

1. <span data-ttu-id="d92a5-105">從資料保護提供者建立資料保護裝置。</span><span class="sxs-lookup"><span data-stu-id="d92a5-105">Create a data protector from a data protection provider.</span></span>

2. <span data-ttu-id="d92a5-106">呼叫您要保護的資料保護方法。</span><span class="sxs-lookup"><span data-stu-id="d92a5-106">Call the Protect method with the data you want to protect.</span></span>

3. <span data-ttu-id="d92a5-107">呼叫 Unprotect 方法與您想要變成純文字資料。</span><span class="sxs-lookup"><span data-stu-id="d92a5-107">Call the Unprotect method with the data you want to turn back into plain text.</span></span>

<span data-ttu-id="d92a5-108">大部分的架構，例如 ASP.NET 或 SignalR 已設定資料保護系統，並將它加入至您存取透過相依性插入的服務容器。</span><span class="sxs-lookup"><span data-stu-id="d92a5-108">Most frameworks such as ASP.NET or SignalR already configure the data protection system and add it to a service container you access via dependency injection.</span></span> <span data-ttu-id="d92a5-109">下列範例將示範如何設定服務容器相依性插入和註冊資料保護堆疊、 接收資料保護提供者，透過 DI、 建立保護裝置和保護，然後取消保護的資料</span><span class="sxs-lookup"><span data-stu-id="d92a5-109">The following sample demonstrates configuring a service container for dependency injection and registering the data protection stack, receiving the data protection provider via DI, creating a protector and protecting then unprotecting data</span></span>

[!code-csharp[Main](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

<span data-ttu-id="d92a5-110">當您建立保護裝置時必須提供一個或多個[目的字串](consumer-apis/purpose-strings.md)。</span><span class="sxs-lookup"><span data-stu-id="d92a5-110">When you create a protector you must provide one or more [Purpose Strings](consumer-apis/purpose-strings.md).</span></span> <span data-ttu-id="d92a5-111">目的字串之間提供隔離取用者，例如建立目的字串 「 綠色 」 的保護裝置就無法取消保護的保護裝置所提供的目的為 「 紫色"的資料。</span><span class="sxs-lookup"><span data-stu-id="d92a5-111">A purpose string provides isolation between consumers, for example a protector created with a purpose string of "green" would not be able to unprotect data provided by a protector with a purpose of "purple".</span></span>

>[!TIP]
> <span data-ttu-id="d92a5-112">IDataProtectionProvider 和 IDataProtector 的執行個體是安全執行緒的多個呼叫端。</span><span class="sxs-lookup"><span data-stu-id="d92a5-112">Instances of IDataProtectionProvider and IDataProtector are thread-safe for multiple callers.</span></span> <span data-ttu-id="d92a5-113">是，一旦元件取得透過呼叫 CreateProtector IDataProtector 的參考，它會使用該參考的多個呼叫保護且取消保護。</span><span class="sxs-lookup"><span data-stu-id="d92a5-113">It is intended that once a component gets a reference to an IDataProtector via a call to CreateProtector, it will use that reference for multiple calls to Protect and Unprotect.</span></span>
>
><span data-ttu-id="d92a5-114">取消保護的呼叫將會擲回 CryptographicException，如果無法驗證或是用來解密受保護的內容。</span><span class="sxs-lookup"><span data-stu-id="d92a5-114">A call to Unprotect will throw CryptographicException if the protected payload cannot be verified or deciphered.</span></span> <span data-ttu-id="d92a5-115">某些元件可能會想要忽略的錯誤時取消保護作業;元件會讀取驗證 cookie，可能會處理這種錯誤和任何 cookie 已完全處理要求而徹底要求失敗。</span><span class="sxs-lookup"><span data-stu-id="d92a5-115">Some components may wish to ignore errors during unprotect operations; a component which reads authentication cookies might handle this error and treat the request as if it had no cookie at all rather than fail the request outright.</span></span> <span data-ttu-id="d92a5-116">元件的這個行為特別應該攔截 CryptographicException，而不是抑制所有例外狀況。</span><span class="sxs-lookup"><span data-stu-id="d92a5-116">Components which want this behavior should specifically catch CryptographicException instead of swallowing all exceptions.</span></span>
