---
uid: mvc/videos/mvc-2/how-do-i/how-do-i-use-httpverbs-attributes-in-an-mvc-application
title: "如何： 使用 HttpVerbs 屬性在 MVC 應用程式嗎？ | Microsoft Docs"
author: rick-anderson
description: "在這個視訊 Chris Pels 示範如何使用 HttpVerbs 屬性來控制對 MVC 動作的存取。 首先，範例應用程式會建立具有預設 co..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/30/2009
ms.topic: article
ms.assetid: d2488a1d-0f3f-4994-8fbe-4f59b8c9503e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/videos/mvc-2/how-do-i/how-do-i-use-httpverbs-attributes-in-an-mvc-application
msc.type: video
ms.openlocfilehash: 278720a6762bedae357e23b368b6dc34e50568d6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="how-do-i-use-httpverbs-attributes-in-an-mvc-application"></a><span data-ttu-id="ffdfd-105">如何： 使用 HttpVerbs 屬性在 MVC 應用程式嗎？</span><span class="sxs-lookup"><span data-stu-id="ffdfd-105">How Do I: Use HttpVerbs Attributes in an MVC Application?</span></span>
====================
<span data-ttu-id="ffdfd-106">由[Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="ffdfd-106">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="ffdfd-107">在這個視訊 Chris Pels 示範如何使用 HttpVerbs 屬性來控制對 MVC 動作的存取。</span><span class="sxs-lookup"><span data-stu-id="ffdfd-107">In this video Chris Pels shows how to use the HttpVerbs attributes to control access to MVC actions.</span></span> <span data-ttu-id="ffdfd-108">首先，範例應用程式會建立具有預設的控制站和編輯資訊檢視。</span><span class="sxs-lookup"><span data-stu-id="ffdfd-108">First, a sample application is created with a default controller and view for editing the information.</span></span> <span data-ttu-id="ffdfd-109">接下來，第二個的索引動作會加入至具有 HttpPost 屬性，而將它限制為只有在使用 HTTP POST 時呼叫的控制站。</span><span class="sxs-lookup"><span data-stu-id="ffdfd-109">Next, a second Index action is added to the controller which has an HttpPost attribute which restricts it to being called only when an HTTP POST is used.</span></span> <span data-ttu-id="ffdfd-110">後續追蹤步驟，為 AcceptVerbs() 屬性會實作為替代語法 for Visual Studio 2008。</span><span class="sxs-lookup"><span data-stu-id="ffdfd-110">As a follow-up, the AcceptVerbs() attribute is implemented as an alternative syntax for Visual Studio 2008.</span></span> <span data-ttu-id="ffdfd-111">然後討論 HttpVerbs 避免安全性風險與使用 HTTP GET 從連結執行刪除相關的用法。</span><span class="sxs-lookup"><span data-stu-id="ffdfd-111">A use of the HttpVerbs for preventing the security risk associated with using an HTTP GET to perform a delete from a link is then discussed.</span></span>

[<span data-ttu-id="ffdfd-112">&#9654;觀看影片 （16 分鐘）</span><span class="sxs-lookup"><span data-stu-id="ffdfd-112">&#9654; Watch video (16 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-httpverbs-attributes-in-an-mvc-application)

>[!div class="step-by-step"]
<span data-ttu-id="ffdfd-113">[上一頁](how-do-i-work-with-model-binders-in-an-mvc-application.md)
[下一頁](mvc2-html-encoding.md)</span><span class="sxs-lookup"><span data-stu-id="ffdfd-113">[Previous](how-do-i-work-with-model-binders-in-an-mvc-application.md)
[Next](mvc2-html-encoding.md)</span></span>
