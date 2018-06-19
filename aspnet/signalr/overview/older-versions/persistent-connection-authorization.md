---
uid: signalr/overview/older-versions/persistent-connection-authorization
title: SignalR 持續連線的驗證和授權 (SignalR 1.x) |Microsoft 文件
author: pfletcher
description: 本主題描述如何強制執行授權持續連線。 如需安全性整合的 SignalR 應用程式，一般資訊...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/21/2013
ms.topic: article
ms.assetid: c34bc627-41af-4c21-a817-e97a19a7f252
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 2e97dfd03c61b110325c41a992b4af490fcd17de
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036098"
---
<a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a><span data-ttu-id="e885c-104">SignalR 持續連線的驗證和授權 (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="e885c-104">Authentication and Authorization for SignalR Persistent Connections (SignalR 1.x)</span></span>
====================
<span data-ttu-id="e885c-105">由[Patrick Fletcher](https://github.com/pfletcher)， [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="e885c-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="e885c-106">本主題描述如何強制執行授權持續連線。</span><span class="sxs-lookup"><span data-stu-id="e885c-106">This topic describes how to enforce authorization on a persistent connection.</span></span> <span data-ttu-id="e885c-107">如需安全性整合的 SignalR 應用程式的一般資訊，請參閱[安全性簡介](index.md)。</span><span class="sxs-lookup"><span data-stu-id="e885c-107">For general information about integrating security into a SignalR application, see [Introduction to Security](index.md).</span></span>


## <a name="enforce-authorization"></a><span data-ttu-id="e885c-108">強制執行授權</span><span class="sxs-lookup"><span data-stu-id="e885c-108">Enforce authorization</span></span>

<span data-ttu-id="e885c-109">若要強制執行時使用授權規則[PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx)必須覆寫`AuthorizeRequest`方法。</span><span class="sxs-lookup"><span data-stu-id="e885c-109">To enforce authorization rules when using a [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="e885c-110">您無法使用`Authorize`持續連線的屬性。</span><span class="sxs-lookup"><span data-stu-id="e885c-110">You cannot use the `Authorize` attribute with persistent connections.</span></span> <span data-ttu-id="e885c-111">`AuthorizeRequest`由 SignalR 架構，以確認使用者已獲授權執行要求的動作的每個要求之前呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="e885c-111">The `AuthorizeRequest` method is called by the SignalR Framework before every request to verify that the user is authorized to perform the requested action.</span></span> <span data-ttu-id="e885c-112">`AuthorizeRequest`方法不會從用戶端呼叫; 相反地，透過您的應用程式的標準驗證機制來驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="e885c-112">The `AuthorizeRequest` method is not called from the client; instead, you authenticate the user through your application's standard authentication mechanism.</span></span>

<span data-ttu-id="e885c-113">下列範例顯示如何限制已驗證的使用者要求。</span><span class="sxs-lookup"><span data-stu-id="e885c-113">The example below shows how to limit requests to authenticated users.</span></span>

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

<span data-ttu-id="e885c-114">您可以將任何自訂的授權邏輯 AuthorizeRequest 方法;例如，檢查使用者是否屬於特定角色。</span><span class="sxs-lookup"><span data-stu-id="e885c-114">You can add any customized authorization logic in the AuthorizeRequest method; such as, checking whether a user belongs to a particular role.</span></span>
