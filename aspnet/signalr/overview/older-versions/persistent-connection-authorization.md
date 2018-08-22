---
uid: signalr/overview/older-versions/persistent-connection-authorization
title: SignalR 持續連線的驗證和授權 (SignalR 1.x) |Microsoft Docs
author: pfletcher
description: 本主題描述如何強制執行授權的持續連線。 如需將安全性整合至 SignalR 應用程式的一般資訊...
ms.author: riande
ms.date: 10/21/2013
ms.assetid: c34bc627-41af-4c21-a817-e97a19a7f252
msc.legacyurl: /signalr/overview/older-versions/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 6c13a63630948a8b6bd1f6e3a6e752b9695256bf
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830191"
---
<a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a><span data-ttu-id="d9c91-104">SignalR 持續連線的驗證和授權 (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="d9c91-104">Authentication and Authorization for SignalR Persistent Connections (SignalR 1.x)</span></span>
====================
<span data-ttu-id="d9c91-105">藉由[Patrick Fletcher](https://github.com/pfletcher)， [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d9c91-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="d9c91-106">本主題描述如何強制執行授權的持續連線。</span><span class="sxs-lookup"><span data-stu-id="d9c91-106">This topic describes how to enforce authorization on a persistent connection.</span></span> <span data-ttu-id="d9c91-107">如需將安全性整合至 SignalR 應用程式的一般資訊，請參閱[安全性簡介](index.md)。</span><span class="sxs-lookup"><span data-stu-id="d9c91-107">For general information about integrating security into a SignalR application, see [Introduction to Security](index.md).</span></span>


## <a name="enforce-authorization"></a><span data-ttu-id="d9c91-108">強制執行授權</span><span class="sxs-lookup"><span data-stu-id="d9c91-108">Enforce authorization</span></span>

<span data-ttu-id="d9c91-109">若要強制執行時使用授權規則[PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx)您必須覆寫`AuthorizeRequest`方法。</span><span class="sxs-lookup"><span data-stu-id="d9c91-109">To enforce authorization rules when using a [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="d9c91-110">您無法使用`Authorize`具有持續性連線屬性。</span><span class="sxs-lookup"><span data-stu-id="d9c91-110">You cannot use the `Authorize` attribute with persistent connections.</span></span> <span data-ttu-id="d9c91-111">`AuthorizeRequest` SignalR 架構，以確認使用者已獲授權執行要求的動作每個要求之前所呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="d9c91-111">The `AuthorizeRequest` method is called by the SignalR Framework before every request to verify that the user is authorized to perform the requested action.</span></span> <span data-ttu-id="d9c91-112">`AuthorizeRequest`方法不會從用戶端呼叫; 相反地，透過您的應用程式的標準驗證機制來驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="d9c91-112">The `AuthorizeRequest` method is not called from the client; instead, you authenticate the user through your application's standard authentication mechanism.</span></span>

<span data-ttu-id="d9c91-113">下列範例顯示如何限制已驗證的使用者的要求。</span><span class="sxs-lookup"><span data-stu-id="d9c91-113">The example below shows how to limit requests to authenticated users.</span></span>

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

<span data-ttu-id="d9c91-114">您可以新增任何自訂的授權邏輯中的 AuthorizeRequest 方法;例如，檢查使用者是否屬於特定的角色。</span><span class="sxs-lookup"><span data-stu-id="d9c91-114">You can add any customized authorization logic in the AuthorizeRequest method; such as, checking whether a user belongs to a particular role.</span></span>
