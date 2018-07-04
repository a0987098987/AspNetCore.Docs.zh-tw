---
uid: signalr/overview/older-versions/persistent-connection-authorization
title: SignalR 持續連線的驗證和授權 (SignalR 1.x) |Microsoft Docs
author: pfletcher
description: 本主題描述如何強制執行授權的持續連線。 如需將安全性整合至 SignalR 應用程式的一般資訊...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/21/2013
ms.topic: article
ms.assetid: c34bc627-41af-4c21-a817-e97a19a7f252
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/older-versions/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 471bce03a37a401c2afe3735afef6f838555cc26
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365106"
---
<a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a><span data-ttu-id="d281e-104">SignalR 持續連線的驗證和授權 (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="d281e-104">Authentication and Authorization for SignalR Persistent Connections (SignalR 1.x)</span></span>
====================
<span data-ttu-id="d281e-105">藉由[Patrick Fletcher](https://github.com/pfletcher)， [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d281e-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="d281e-106">本主題描述如何強制執行授權的持續連線。</span><span class="sxs-lookup"><span data-stu-id="d281e-106">This topic describes how to enforce authorization on a persistent connection.</span></span> <span data-ttu-id="d281e-107">如需將安全性整合至 SignalR 應用程式的一般資訊，請參閱[安全性簡介](index.md)。</span><span class="sxs-lookup"><span data-stu-id="d281e-107">For general information about integrating security into a SignalR application, see [Introduction to Security](index.md).</span></span>


## <a name="enforce-authorization"></a><span data-ttu-id="d281e-108">強制執行授權</span><span class="sxs-lookup"><span data-stu-id="d281e-108">Enforce authorization</span></span>

<span data-ttu-id="d281e-109">若要強制執行時使用授權規則[PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx)您必須覆寫`AuthorizeRequest`方法。</span><span class="sxs-lookup"><span data-stu-id="d281e-109">To enforce authorization rules when using a [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="d281e-110">您無法使用`Authorize`具有持續性連線屬性。</span><span class="sxs-lookup"><span data-stu-id="d281e-110">You cannot use the `Authorize` attribute with persistent connections.</span></span> <span data-ttu-id="d281e-111">`AuthorizeRequest` SignalR 架構，以確認使用者已獲授權執行要求的動作每個要求之前所呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="d281e-111">The `AuthorizeRequest` method is called by the SignalR Framework before every request to verify that the user is authorized to perform the requested action.</span></span> <span data-ttu-id="d281e-112">`AuthorizeRequest`方法不會從用戶端呼叫; 相反地，透過您的應用程式的標準驗證機制來驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="d281e-112">The `AuthorizeRequest` method is not called from the client; instead, you authenticate the user through your application's standard authentication mechanism.</span></span>

<span data-ttu-id="d281e-113">下列範例顯示如何限制已驗證的使用者的要求。</span><span class="sxs-lookup"><span data-stu-id="d281e-113">The example below shows how to limit requests to authenticated users.</span></span>

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

<span data-ttu-id="d281e-114">您可以新增任何自訂的授權邏輯中的 AuthorizeRequest 方法;例如，檢查使用者是否屬於特定的角色。</span><span class="sxs-lookup"><span data-stu-id="d281e-114">You can add any customized authorization logic in the AuthorizeRequest method; such as, checking whether a user belongs to a particular role.</span></span>
