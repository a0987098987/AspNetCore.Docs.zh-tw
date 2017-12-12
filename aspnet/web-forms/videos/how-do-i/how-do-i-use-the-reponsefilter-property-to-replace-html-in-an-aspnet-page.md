---
uid: web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
title: "[如何:]使用 Reponse.Filter 屬性來取代 ASP.NET 網頁中的 HTML |Microsoft 文件"
author: rick-anderson
description: "在這個視訊 Chris Pels 示範如何使用 Reponse.Filter 屬性來攔截並變更傳送到網頁的 HTML。 首先，範例頁面會建立 w..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/29/2009
ms.topic: article
ms.assetid: 3e5ae74a-9798-47d8-a2b3-0d8ad42dd4bc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
msc.type: video
ms.openlocfilehash: 3e7c1f2a947d185d7c2a01deb75e42c92ba914c3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page"></a><span data-ttu-id="980b7-104">[如何:]使用 Reponse.Filter 屬性來取代 ASP.NET 網頁中的 HTML</span><span class="sxs-lookup"><span data-stu-id="980b7-104">[How Do I:] Use the Reponse.Filter Property to Replace HTML in an ASP.NET Page</span></span>
====================
<span data-ttu-id="980b7-105">由[Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="980b7-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="980b7-106">在這個視訊 Chris Pels 示範如何使用 Reponse.Filter 屬性來攔截並變更傳送到網頁的 HTML。</span><span class="sxs-lookup"><span data-stu-id="980b7-106">In this video Chris Pels shows how to use the Reponse.Filter property to intercept and alter the HTML being sent to a page.</span></span> <span data-ttu-id="980b7-107">首先，範例頁面會建立一些簡單的文字。</span><span class="sxs-lookup"><span data-stu-id="980b7-107">First, a sample page is created with some simple text.</span></span> <span data-ttu-id="980b7-108">然後，會建立自訂的資料流類別，做為取代資料流正在傳送給使用者的瀏覽器的目前資料流。</span><span class="sxs-lookup"><span data-stu-id="980b7-108">Then, a custom Stream class is created which serves as the replacement stream for the current stream being sent to the user's browser.</span></span> <span data-ttu-id="980b7-109">自訂資料流該類中頁面的內容會從改變，而回應資料流寫出資料流擷取的。</span><span class="sxs-lookup"><span data-stu-id="980b7-109">In that custom stream class the contents of the page are retrieved from the stream, altered, and then written out to the response stream.</span></span> <span data-ttu-id="980b7-110">這個自訂的資料流類別中寫入方法已自訂為取代基底的回應資料流，藉此改變傳送到使用者的瀏覽器的 HTML。</span><span class="sxs-lookup"><span data-stu-id="980b7-110">In this custom Stream class the Write method is customized to replace the HTML in the base Response stream, thereby altering what is sent to the user's browser.</span></span> <span data-ttu-id="980b7-111">新的資料流類別指派給 Response.Filter 內容頁面中的最後，\_載入事件，因此，提供改變頁面內容的機制。</span><span class="sxs-lookup"><span data-stu-id="980b7-111">Finally, the new stream class is assigned to the Response.Filter property in the Page\_Load event, thereby, providing the mechanism for altering the page content.</span></span>

[<span data-ttu-id="980b7-112">&#9654;觀看影片 （13 分鐘）</span><span class="sxs-lookup"><span data-stu-id="980b7-112">&#9654; Watch video (13 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page)
