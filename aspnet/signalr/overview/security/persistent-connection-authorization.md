---
uid: signalr/overview/security/persistent-connection-authorization
title: "SignalR 持續連線的驗證和授權 |Microsoft 文件"
author: pfletcher
description: "本主題描述如何強制執行授權持續連線。 如需安全性整合的 SignalR 應用程式，一般資訊..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: e264677b-9c01-47ec-94f9-3cd8f08f94af
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/security/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 9c6fff86ae6b1b65e6ba9922b6b8448643ef1f15
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="authentication-and-authorization-for-signalr-persistent-connections"></a><span data-ttu-id="23f6e-104">SignalR 持續連線的驗證和授權</span><span class="sxs-lookup"><span data-stu-id="23f6e-104">Authentication and Authorization for SignalR Persistent Connections</span></span>
====================
<span data-ttu-id="23f6e-105">由[Patrick Fletcher](https://github.com/pfletcher)， [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="23f6e-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="23f6e-106">本主題描述如何強制執行授權持續連線。</span><span class="sxs-lookup"><span data-stu-id="23f6e-106">This topic describes how to enforce authorization on a persistent connection.</span></span> <span data-ttu-id="23f6e-107">如需安全性整合的 SignalR 應用程式的一般資訊，請參閱[安全性簡介](introduction-to-security.md)。</span><span class="sxs-lookup"><span data-stu-id="23f6e-107">For general information about integrating security into a SignalR application, see [Introduction to Security](introduction-to-security.md).</span></span> 
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="23f6e-108">本主題中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="23f6e-108">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="23f6e-109">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="23f6e-109">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="23f6e-110">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="23f6e-110">.NET 4.5</span></span>
> - <span data-ttu-id="23f6e-111">SignalR 第 2 版</span><span class="sxs-lookup"><span data-stu-id="23f6e-111">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="23f6e-112">本主題的先前版本</span><span class="sxs-lookup"><span data-stu-id="23f6e-112">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="23f6e-113">如需舊版 SignalR 的資訊，請參閱[SignalR 舊版](../older-versions/index.md)。</span><span class="sxs-lookup"><span data-stu-id="23f6e-113">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="23f6e-114">問題和註解</span><span class="sxs-lookup"><span data-stu-id="23f6e-114">Questions and comments</span></span>
> 
> <span data-ttu-id="23f6e-115">請留下上如何您所喜歡的本教學課程，我們可以改進中將註解放在頁面底部的意見反應。</span><span class="sxs-lookup"><span data-stu-id="23f6e-115">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="23f6e-116">如果您有與本教學課程不直接相關的問題，您可以將它們來公佈[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或[StackOverflow.com](http://stackoverflow.com/)。</span><span class="sxs-lookup"><span data-stu-id="23f6e-116">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="enforce-authorization"></a><span data-ttu-id="23f6e-117">強制執行授權</span><span class="sxs-lookup"><span data-stu-id="23f6e-117">Enforce authorization</span></span>

<span data-ttu-id="23f6e-118">若要強制執行時使用授權規則[PersistentConnection](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx)必須覆寫`AuthorizeRequest`方法。</span><span class="sxs-lookup"><span data-stu-id="23f6e-118">To enforce authorization rules when using a [PersistentConnection](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="23f6e-119">您無法使用`Authorize`持續連線的屬性。</span><span class="sxs-lookup"><span data-stu-id="23f6e-119">You cannot use the `Authorize` attribute with persistent connections.</span></span> <span data-ttu-id="23f6e-120">`AuthorizeRequest`由 SignalR 架構，以確認使用者已獲授權執行要求的動作的每個要求之前呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="23f6e-120">The `AuthorizeRequest` method is called by the SignalR Framework before every request to verify that the user is authorized to perform the requested action.</span></span> <span data-ttu-id="23f6e-121">`AuthorizeRequest`方法不會從用戶端呼叫; 相反地，透過您的應用程式的標準驗證機制來驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="23f6e-121">The `AuthorizeRequest` method is not called from the client; instead, you authenticate the user through your application's standard authentication mechanism.</span></span>

<span data-ttu-id="23f6e-122">下列範例顯示如何限制已驗證的使用者要求。</span><span class="sxs-lookup"><span data-stu-id="23f6e-122">The example below shows how to limit requests to authenticated users.</span></span>

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

<span data-ttu-id="23f6e-123">您可以將任何自訂的授權邏輯 AuthorizeRequest 方法;例如，檢查使用者是否屬於特定角色。</span><span class="sxs-lookup"><span data-stu-id="23f6e-123">You can add any customized authorization logic in the AuthorizeRequest method; such as, checking whether a user belongs to a particular role.</span></span>
