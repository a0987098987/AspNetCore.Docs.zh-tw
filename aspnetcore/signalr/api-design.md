---
title: SignalRAPI 設計考慮
author: anurse
description: 瞭解如何設計 SignalR 應用程式版本之間相容性的 api。
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: signalr/api-design
ms.openlocfilehash: 9ad8d30da552d3d3084534b8c7ca57386ad111ac
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85407794"
---
# <a name="signalr-api-design-considerations"></a>SignalR<span data-ttu-id="dca7f-103">API 設計考慮</span><span class="sxs-lookup"><span data-stu-id="dca7f-103"> API design considerations</span></span>

<span data-ttu-id="dca7f-104">[Andrew Stanton-護士](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="dca7f-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="dca7f-105">本文提供以建立為 SignalR 基礎的 api 指引。</span><span class="sxs-lookup"><span data-stu-id="dca7f-105">This article provides guidance for building SignalR-based APIs.</span></span>

## <a name="use-custom-object-parameters-to-ensure-backwards-compatibility"></a><span data-ttu-id="dca7f-106">使用自訂物件參數來確保回溯相容性</span><span class="sxs-lookup"><span data-stu-id="dca7f-106">Use custom object parameters to ensure backwards-compatibility</span></span>

<span data-ttu-id="dca7f-107">將參數新增至 SignalR 中樞方法（在用戶端或伺服器上）是一項*重大變更*。</span><span class="sxs-lookup"><span data-stu-id="dca7f-107">Adding parameters to a SignalR hub method (on either the client or the server) is a *breaking change*.</span></span> <span data-ttu-id="dca7f-108">這表示較舊的用戶端/伺服器在嘗試叫用方法時，如果沒有適當數目的參數，就會收到錯誤。</span><span class="sxs-lookup"><span data-stu-id="dca7f-108">This means older clients/servers will get errors when they try to invoke the method without the appropriate number of parameters.</span></span> <span data-ttu-id="dca7f-109">不過，將屬性新增至自訂物件參數並**不**是一種中斷變更。</span><span class="sxs-lookup"><span data-stu-id="dca7f-109">However, adding properties to a custom object parameter is **not** a breaking change.</span></span> <span data-ttu-id="dca7f-110">這可以用來設計可在用戶端或伺服器上復原變更的相容 Api。</span><span class="sxs-lookup"><span data-stu-id="dca7f-110">This can be used to design compatible APIs that are resilient to changes on the client or the server.</span></span>

<span data-ttu-id="dca7f-111">例如，假設有一個伺服器端 API，如下所示：</span><span class="sxs-lookup"><span data-stu-id="dca7f-111">For example, consider a server-side API like the following:</span></span>

[!code-csharp[ParameterBasedOldVersion](api-design/sample/Samples.cs?name=ParameterBasedOldVersion)]

<span data-ttu-id="dca7f-112">JavaScript 用戶端會使用呼叫這個方法 `invoke` ，如下所示：</span><span class="sxs-lookup"><span data-stu-id="dca7f-112">The JavaScript client calls this method using `invoke` as follows:</span></span>

[!code-typescript[CallWithOneParameter](api-design/sample/Samples.ts?name=CallWithOneParameter)]

<span data-ttu-id="dca7f-113">如果您稍後將第二個參數新增至伺服器方法，舊版用戶端就不會提供此參數值。</span><span class="sxs-lookup"><span data-stu-id="dca7f-113">If you later add a second parameter to the server method, older clients won't provide this parameter value.</span></span> <span data-ttu-id="dca7f-114">例如：</span><span class="sxs-lookup"><span data-stu-id="dca7f-114">For example:</span></span>

[!code-csharp[ParameterBasedNewVersion](api-design/sample/Samples.cs?name=ParameterBasedNewVersion)]

<span data-ttu-id="dca7f-115">當舊的用戶端嘗試叫用此方法時，會收到類似下面的錯誤：</span><span class="sxs-lookup"><span data-stu-id="dca7f-115">When the old client tries to invoke this method, it will get an error like this:</span></span>

```
Microsoft.AspNetCore.SignalR.HubException: Failed to invoke 'GetTotalLength' due to an error on the server.
```

<span data-ttu-id="dca7f-116">在伺服器上，您會看到類似下面的記錄訊息：</span><span class="sxs-lookup"><span data-stu-id="dca7f-116">On the server, you'll see a log message like this:</span></span>

```
System.IO.InvalidDataException: Invocation provides 1 argument(s) but target expects 2.
```

<span data-ttu-id="dca7f-117">舊的用戶端只會傳送一個參數，但較新的伺服器 API 需要兩個參數。</span><span class="sxs-lookup"><span data-stu-id="dca7f-117">The old client only sent one parameter, but the newer server API required two parameters.</span></span> <span data-ttu-id="dca7f-118">使用自訂物件做為參數可提供更大的彈性。</span><span class="sxs-lookup"><span data-stu-id="dca7f-118">Using custom objects as parameters gives you more flexibility.</span></span> <span data-ttu-id="dca7f-119">讓我們重新設計原始 API，以使用自訂物件：</span><span class="sxs-lookup"><span data-stu-id="dca7f-119">Let's redesign the original API to use a custom object:</span></span>

[!code-csharp[ObjectBasedOldVersion](api-design/sample/Samples.cs?name=ObjectBasedOldVersion)]

<span data-ttu-id="dca7f-120">現在，用戶端會使用物件來呼叫方法：</span><span class="sxs-lookup"><span data-stu-id="dca7f-120">Now, the client uses an object to call the method:</span></span>

[!code-typescript[CallWithObject](api-design/sample/Samples.ts?name=CallWithObject)]

<span data-ttu-id="dca7f-121">將屬性新增至物件，而不是新增參數 `TotalLengthRequest` ：</span><span class="sxs-lookup"><span data-stu-id="dca7f-121">Instead of adding a parameter, add a property to the `TotalLengthRequest` object:</span></span>

[!code-csharp[ObjectBasedNewVersion](api-design/sample/Samples.cs?name=ObjectBasedNewVersion&highlight=4,9-13)]

<span data-ttu-id="dca7f-122">當舊的用戶端傳送單一參數時， `Param2` 將會留下額外的屬性 `null` 。</span><span class="sxs-lookup"><span data-stu-id="dca7f-122">When the old client sends a single parameter, the extra `Param2` property will be left `null`.</span></span> <span data-ttu-id="dca7f-123">您可以藉由檢查 `Param2` 並套用預設值，偵測舊版用戶端所傳送的訊息 `null` 。</span><span class="sxs-lookup"><span data-stu-id="dca7f-123">You can detect a message sent by an older client by checking the `Param2` for `null` and apply a default value.</span></span> <span data-ttu-id="dca7f-124">新的用戶端可以傳送這兩個參數。</span><span class="sxs-lookup"><span data-stu-id="dca7f-124">A new client can send both parameters.</span></span>

[!code-typescript[CallWithObjectNew](api-design/sample/Samples.ts?name=CallWithObjectNew)]

<span data-ttu-id="dca7f-125">相同的技術適用于在用戶端上定義的方法。</span><span class="sxs-lookup"><span data-stu-id="dca7f-125">The same technique works for methods defined on the client.</span></span> <span data-ttu-id="dca7f-126">您可以從伺服器端傳送自訂物件：</span><span class="sxs-lookup"><span data-stu-id="dca7f-126">You can send a custom object from the server side:</span></span>

[!code-csharp[ClientSideObjectBasedOld](api-design/sample/Samples.cs?name=ClientSideObjectBasedOld)]

<span data-ttu-id="dca7f-127">在用戶端上，您可以存取 `Message` 屬性，而不是使用參數：</span><span class="sxs-lookup"><span data-stu-id="dca7f-127">On the client side, you access the `Message` property rather than using a parameter:</span></span>

[!code-typescript[OnWithObjectOld](api-design/sample/Samples.ts?name=OnWithObjectOld)]

<span data-ttu-id="dca7f-128">如果您稍後決定要將訊息的傳送者新增至裝載，請將屬性新增至物件：</span><span class="sxs-lookup"><span data-stu-id="dca7f-128">If you later decide to add the sender of the message to the payload, add a property to the object:</span></span>

[!code-csharp[ClientSideObjectBasedNew](api-design/sample/Samples.cs?name=ClientSideObjectBasedNew&highlight=5)]

<span data-ttu-id="dca7f-129">較舊的用戶端不會預期 `Sender` 值，因此會予以忽略。</span><span class="sxs-lookup"><span data-stu-id="dca7f-129">The older clients won't be expecting the `Sender` value, so they'll ignore it.</span></span> <span data-ttu-id="dca7f-130">新的用戶端可以藉由更新來接受它，以讀取新的屬性：</span><span class="sxs-lookup"><span data-stu-id="dca7f-130">A new client can accept it by updating to read the new property:</span></span>

[!code-typescript[OnWithObjectNew](api-design/sample/Samples.ts?name=OnWithObjectNew&highlight=2-5)]

<span data-ttu-id="dca7f-131">在此情況下，新的用戶端也會容忍不提供此值的舊伺服器 `Sender` 。</span><span class="sxs-lookup"><span data-stu-id="dca7f-131">In this case, the new client is also tolerant of an old server that doesn't provide the `Sender` value.</span></span> <span data-ttu-id="dca7f-132">由於舊伺服器不會提供 `Sender` 值，因此用戶端會先檢查它是否存在，然後再進行存取。</span><span class="sxs-lookup"><span data-stu-id="dca7f-132">Since the old server won't provide the `Sender` value, the client checks to see if it exists before accessing it.</span></span>
