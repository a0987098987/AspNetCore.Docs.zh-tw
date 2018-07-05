---
uid: signalr/overview/security/persistent-connection-authorization
title: SignalR 持續連線的驗證和授權 |Microsoft Docs
author: pfletcher
description: 本主題描述如何強制執行授權的持續連線。 如需將安全性整合至 SignalR 應用程式的一般資訊...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: e264677b-9c01-47ec-94f9-3cd8f08f94af
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/security/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: d15ac6ec8b3bab041a13918a3577310c62e66b8f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372222"
---
<a name="authentication-and-authorization-for-signalr-persistent-connections"></a><span data-ttu-id="256da-104">SignalR 持續連線的驗證和授權</span><span class="sxs-lookup"><span data-stu-id="256da-104">Authentication and Authorization for SignalR Persistent Connections</span></span>
====================
<span data-ttu-id="256da-105">藉由[Patrick Fletcher](https://github.com/pfletcher)， [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="256da-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="256da-106">本主題描述如何強制執行授權的持續連線。</span><span class="sxs-lookup"><span data-stu-id="256da-106">This topic describes how to enforce authorization on a persistent connection.</span></span> <span data-ttu-id="256da-107">如需將安全性整合至 SignalR 應用程式的一般資訊，請參閱[安全性簡介](introduction-to-security.md)。</span><span class="sxs-lookup"><span data-stu-id="256da-107">For general information about integrating security into a SignalR application, see [Introduction to Security](introduction-to-security.md).</span></span> 
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="256da-108">本主題中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="256da-108">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="256da-109">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="256da-109">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="256da-110">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="256da-110">.NET 4.5</span></span>
> - <span data-ttu-id="256da-111">SignalR 第 2 版</span><span class="sxs-lookup"><span data-stu-id="256da-111">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="256da-112">本主題的上一個版本</span><span class="sxs-lookup"><span data-stu-id="256da-112">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="256da-113">如需舊版 SignalR 的資訊，請參閱[SignalR 舊版](../older-versions/index.md)。</span><span class="sxs-lookup"><span data-stu-id="256da-113">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="256da-114">提出問題或意見</span><span class="sxs-lookup"><span data-stu-id="256da-114">Questions and comments</span></span>
> 
> <span data-ttu-id="256da-115">您喜歡本教學課程中的方式，和我們可以改善在頁面底部的註解中，歡迎留下意見反應。</span><span class="sxs-lookup"><span data-stu-id="256da-115">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="256da-116">如果您有不直接相關的教學課程中的問題，您可以張貼他們[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或是[StackOverflow.com](http://stackoverflow.com/)。</span><span class="sxs-lookup"><span data-stu-id="256da-116">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="enforce-authorization"></a><span data-ttu-id="256da-117">強制執行授權</span><span class="sxs-lookup"><span data-stu-id="256da-117">Enforce authorization</span></span>

<span data-ttu-id="256da-118">若要強制執行時使用授權規則[PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx)您必須覆寫`AuthorizeRequest`方法。</span><span class="sxs-lookup"><span data-stu-id="256da-118">To enforce authorization rules when using a [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="256da-119">您無法使用`Authorize`具有持續性連線屬性。</span><span class="sxs-lookup"><span data-stu-id="256da-119">You cannot use the `Authorize` attribute with persistent connections.</span></span> <span data-ttu-id="256da-120">`AuthorizeRequest` SignalR 架構，以確認使用者已獲授權執行要求的動作每個要求之前所呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="256da-120">The `AuthorizeRequest` method is called by the SignalR Framework before every request to verify that the user is authorized to perform the requested action.</span></span> <span data-ttu-id="256da-121">`AuthorizeRequest`方法不會從用戶端呼叫; 相反地，透過您的應用程式的標準驗證機制來驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="256da-121">The `AuthorizeRequest` method is not called from the client; instead, you authenticate the user through your application's standard authentication mechanism.</span></span>

<span data-ttu-id="256da-122">下列範例顯示如何限制已驗證的使用者的要求。</span><span class="sxs-lookup"><span data-stu-id="256da-122">The example below shows how to limit requests to authenticated users.</span></span>

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

<span data-ttu-id="256da-123">您可以新增任何自訂的授權邏輯中的 AuthorizeRequest 方法;例如，檢查使用者是否屬於特定的角色。</span><span class="sxs-lookup"><span data-stu-id="256da-123">You can add any customized authorization logic in the AuthorizeRequest method; such as, checking whether a user belongs to a particular role.</span></span>
