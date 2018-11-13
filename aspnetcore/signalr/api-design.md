---
title: SignalR 的 API 設計考量
author: anurse
description: 了解如何設計相容性的 SignalR 的 Api，在您的應用程式的版本。
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 11/06/2018
uid: signalr/api-design
ms.openlocfilehash: 3f17bf055b793e8fc91fbcc15f668928ca261f77
ms.sourcegitcommit: 408921a932448f66cb46fd53c307a864f5323fe5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/12/2018
ms.locfileid: "51571548"
---
# <a name="signalr-api-design-considerations"></a><span data-ttu-id="db492-103">SignalR 的 API 設計考量</span><span class="sxs-lookup"><span data-stu-id="db492-103">SignalR API design considerations</span></span>

<span data-ttu-id="db492-104">藉由[Andrew Stanton-nurse](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="db492-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="db492-105">本文章提供建置 SignalR 為基礎的 Api 的指引。</span><span class="sxs-lookup"><span data-stu-id="db492-105">This article provides guidance for building SignalR-based APIs.</span></span>

## <a name="use-custom-object-parameters-to-ensure-backwards-compatibility"></a><span data-ttu-id="db492-106">使用自訂的物件參數，確保回溯相容性</span><span class="sxs-lookup"><span data-stu-id="db492-106">Use custom object parameters to ensure backwards-compatibility</span></span>

<span data-ttu-id="db492-107">加入 （用戶端或伺服器） 上的 SignalR 中樞方法的參數是*重大變更*。</span><span class="sxs-lookup"><span data-stu-id="db492-107">Adding parameters to a SignalR hub method (on either the client or the server) is a *breaking change*.</span></span> <span data-ttu-id="db492-108">這表示較舊的用戶端/伺服器將會收到錯誤，當使用者試著叫用方法，而不需要適當的參數數目。</span><span class="sxs-lookup"><span data-stu-id="db492-108">This means older clients/servers will get errors when they try to invoke the method without the appropriate number of parameters.</span></span> <span data-ttu-id="db492-109">不過，將屬性加入至自訂的物件參數是**不**一項重大變更。</span><span class="sxs-lookup"><span data-stu-id="db492-109">However, adding properties to a custom object parameter is **not** a breaking change.</span></span> <span data-ttu-id="db492-110">這可用來設計相容的 Api，有彈性地在用戶端或伺服器上的變更。</span><span class="sxs-lookup"><span data-stu-id="db492-110">This can be used to design compatible APIs that are resilient to changes on the client or the server.</span></span>

<span data-ttu-id="db492-111">例如，請考慮伺服器端 API，如下所示：</span><span class="sxs-lookup"><span data-stu-id="db492-111">For example, consider a server-side API like the following:</span></span>

[!code-csharp[ParameterBasedOldVersion](api-design/sample/Samples.cs?name=ParameterBasedOldVersion)]

<span data-ttu-id="db492-112">JavaScript 用戶端會呼叫此方法使用`invoke`，如下所示：</span><span class="sxs-lookup"><span data-stu-id="db492-112">The JavaScript client calls this method using `invoke` as follows:</span></span>

[!code-typescript[CallWithOneParameter](api-design/sample/Samples.ts?name=CallWithOneParameter)]

<span data-ttu-id="db492-113">如果您稍後會將第二個參數新增至伺服器方法，較舊的用戶端不會提供此參數值。</span><span class="sxs-lookup"><span data-stu-id="db492-113">If you later add a second parameter to the server method, older clients won't provide this parameter value.</span></span> <span data-ttu-id="db492-114">例如: </span><span class="sxs-lookup"><span data-stu-id="db492-114">For example:</span></span>

[!code-csharp[ParameterBasedNewVersion](api-design/sample/Samples.cs?name=ParameterBasedNewVersion)]

<span data-ttu-id="db492-115">當舊的用戶端嘗試叫用這個方法時，它會取得如下的錯誤：</span><span class="sxs-lookup"><span data-stu-id="db492-115">When the old client tries to invoke this method, it will get an error like this:</span></span>

```
Microsoft.AspNetCore.SignalR.HubException: Failed to invoke 'GetTotalLength' due to an error on the server.
```

<span data-ttu-id="db492-116">在伺服器上，您會看到如下的記錄檔訊息：</span><span class="sxs-lookup"><span data-stu-id="db492-116">On the server, you'll see a log message like this:</span></span>

```
System.IO.InvalidDataException: Invocation provides 1 argument(s) but target expects 2.
```

<span data-ttu-id="db492-117">舊的用戶端只會傳送一個參數，但較新的伺服器 API 所需的兩個參數。</span><span class="sxs-lookup"><span data-stu-id="db492-117">The old client only sent one parameter, but the newer server API required two parameters.</span></span> <span data-ttu-id="db492-118">使用自訂物件做為參數，讓您更大的彈性。</span><span class="sxs-lookup"><span data-stu-id="db492-118">Using custom objects as parameters gives you more flexibility.</span></span> <span data-ttu-id="db492-119">讓我們重新設計要使用自訂物件的原始 API:</span><span class="sxs-lookup"><span data-stu-id="db492-119">Let's redesign the original API to use a custom object:</span></span>

[!code-csharp[ObjectBasedOldVersion](api-design/sample/Samples.cs?name=ObjectBasedOldVersion)]

<span data-ttu-id="db492-120">現在，讓用戶端物件呼叫方法：</span><span class="sxs-lookup"><span data-stu-id="db492-120">Now, the client uses an object to call the method:</span></span>

[!code-typescript[CallWithObject](api-design/sample/Samples.ts?name=CallWithObject)]

<span data-ttu-id="db492-121">而不是新增參數，將屬性新增至`TotalLengthRequest`物件：</span><span class="sxs-lookup"><span data-stu-id="db492-121">Instead of adding a parameter, add a property to the `TotalLengthRequest` object:</span></span>

[!code-csharp[ObjectBasedNewVersion](api-design/sample/Samples.cs?name=ObjectBasedNewVersion&highlight=4,9-13)]

<span data-ttu-id="db492-122">當舊的用戶端傳送的額外的參數`Param2`屬性會保留`null`。</span><span class="sxs-lookup"><span data-stu-id="db492-122">When the old client sends a single parameter, the extra `Param2` property will be left `null`.</span></span> <span data-ttu-id="db492-123">您可以藉由檢查較舊的用戶端所傳送的訊息來偵測`Param2`針對`null`並套用預設值。</span><span class="sxs-lookup"><span data-stu-id="db492-123">You can detect a message sent by an older client by checking the `Param2` for `null` and apply a default value.</span></span> <span data-ttu-id="db492-124">新的用戶端可以傳送兩個參數。</span><span class="sxs-lookup"><span data-stu-id="db492-124">A new client can send both parameters.</span></span>

[!code-typescript[CallWithObjectNew](api-design/sample/Samples.ts?name=CallWithObjectNew)]

<span data-ttu-id="db492-125">相同的技巧適用於在用戶端定義的方法。</span><span class="sxs-lookup"><span data-stu-id="db492-125">The same technique works for methods defined on the client.</span></span> <span data-ttu-id="db492-126">您可以從伺服器端來傳送自訂的物件：</span><span class="sxs-lookup"><span data-stu-id="db492-126">You can send a custom object from the server side:</span></span>

[!code-csharp[ClientSideObjectBasedOld](api-design/sample/Samples.cs?name=ClientSideObjectBasedOld)]

<span data-ttu-id="db492-127">在您存取用戶端，`Message`屬性，而不是使用參數：</span><span class="sxs-lookup"><span data-stu-id="db492-127">On the client side, you access the `Message` property rather than using a parameter:</span></span>

[!code-typescript[OnWithObjectOld](api-design/sample/Samples.ts?name=OnWithObjectOld)]

<span data-ttu-id="db492-128">如果您稍後決定裝載中加入訊息的寄件者，將屬性加入物件：</span><span class="sxs-lookup"><span data-stu-id="db492-128">If you later decide to add the sender of the message to the payload, add a property to the object:</span></span>

[!code-csharp[ClientSideObjectBasedNew](api-design/sample/Samples.cs?name=ClientSideObjectBasedNew&highlight=5)]

<span data-ttu-id="db492-129">不會預期較舊的用戶端`Sender`值，因此它們會忽略它。</span><span class="sxs-lookup"><span data-stu-id="db492-129">The older clients won't be expecting the `Sender` value, so they'll ignore it.</span></span> <span data-ttu-id="db492-130">新的用戶端可接受它藉由更新為新的屬性：</span><span class="sxs-lookup"><span data-stu-id="db492-130">A new client can accept it by updating to read the new property:</span></span>

[!code-typescript[OnWithObjectNew](api-design/sample/Samples.ts?name=OnWithObjectNew&highlight=2-5)]

<span data-ttu-id="db492-131">在此情況下，新的用戶端也是不提供舊伺服器的容錯`Sender`值。</span><span class="sxs-lookup"><span data-stu-id="db492-131">In this case, the new client is also tolerant of an old server that doesn't provide the `Sender` value.</span></span> <span data-ttu-id="db492-132">因為舊的伺服器將不會提供`Sender`值，用戶端會檢查以查看它是否存在才能存取它。</span><span class="sxs-lookup"><span data-stu-id="db492-132">Since the old server won't provide the `Sender` value, the client checks to see if it exists before accessing it.</span></span>
