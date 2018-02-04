---
uid: mvc/videos/mvc-2/how-do-i/how-do-i-create-a-custom-html-helper-for-an-mvc-application
title: "如何： 建立 MVC 應用程式的自訂 HTML Helper？ | Microsoft Docs"
author: rick-anderson
description: "在這段影片 Chris Pels 會示範如何建立自訂的 HtmlHelper，不適用於一組標準的 MVC 應用程式中。 第一個範例 MVC 應用程式..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/11/2009
ms.topic: article
ms.assetid: 58b5eb15-4160-4ce2-ae70-6ba94262ea73
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/videos/mvc-2/how-do-i/how-do-i-create-a-custom-html-helper-for-an-mvc-application
msc.type: video
ms.openlocfilehash: 50a03799336636a8ba622b4ee3e8da99dcbc2708
ms.sourcegitcommit: 7a87d66cf1d01febe6635c7306f2f679434901d1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/03/2018
---
<a name="how-do-i-create-a-custom-html-helper-for-an-mvc-application"></a><span data-ttu-id="08aa8-105">如何： 建立 MVC 應用程式的自訂 HTML Helper？</span><span class="sxs-lookup"><span data-stu-id="08aa8-105">How Do I: Create a Custom HTML Helper for an MVC Application?</span></span>
====================
<span data-ttu-id="08aa8-106">由[Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="08aa8-106">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="08aa8-107">在這段影片 Chris Pels 會示範如何建立自訂的 HtmlHelper，不適用於一組標準的 MVC 應用程式中。</span><span class="sxs-lookup"><span data-stu-id="08aa8-107">In this video Chris Pels shows how to create a custom HtmlHelper that is not available in the standard set in an MVC application.</span></span> <span data-ttu-id="08aa8-108">首先，範例 MVC 應用程式會建立具有示範控制器和檢視，來測試自訂 HtmlHelper。</span><span class="sxs-lookup"><span data-stu-id="08aa8-108">First, a sample MVC application is created with a demo controller and view to test the custom HtmlHelper.</span></span> <span data-ttu-id="08aa8-109">接下來，模組會建立為代表自訂 HtmlHelper 實作擴充方法的公用函式。</span><span class="sxs-lookup"><span data-stu-id="08aa8-109">Next, a module is created with a public function that is an extension method which represents the implementation of the custom HtmlHelper.</span></span> <span data-ttu-id="08aa8-110">是用來建立自訂 helper`<img>`標記頁面中，並將接收數個輸入的參數，包括識別碼、 url 和影像標記的替代文字。</span><span class="sxs-lookup"><span data-stu-id="08aa8-110">The custom helper is for creating `<img>` tags in a page and receives several inbound parameters including the id, url, and alt text for the image tag.</span></span> <span data-ttu-id="08aa8-111">傳回已完成的函式就會加入邏輯`<img>`標記指定的資訊。</span><span class="sxs-lookup"><span data-stu-id="08aa8-111">The logic is then added to the function for returning the completed `<img>` tag with the specified information.</span></span> <span data-ttu-id="08aa8-112">然後自訂 HtmlHelper 是示範頁面上用來顯示影像。</span><span class="sxs-lookup"><span data-stu-id="08aa8-112">Then the custom HtmlHelper is used on the demo page to display an image.</span></span> <span data-ttu-id="08aa8-113">最後，自訂 HtmlHelper 已擴展成包含多個建構函式覆寫可提供更輕鬆地建立不同的彈性`<img>`標記。</span><span class="sxs-lookup"><span data-stu-id="08aa8-113">Finally, the custom HtmlHelper is expanded to include multiple constructor overrides which provide flexibility for more easily creating different `<img>` tags.</span></span>

[<span data-ttu-id="08aa8-114">&#9654;觀看影片 （18 分鐘）</span><span class="sxs-lookup"><span data-stu-id="08aa8-114">&#9654; Watch video (18 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-create-a-custom-html-helper-for-an-mvc-application)

>[!div class="step-by-step"]
<span data-ttu-id="08aa8-115">[上一頁](how-do-i-implement-view-models-to-manage-data-for-aspnet-mvc-views.md)
[下一頁](how-do-i-work-with-model-binders-in-an-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="08aa8-115">[Previous](how-do-i-implement-view-models-to-manage-data-for-aspnet-mvc-views.md)
[Next](how-do-i-work-with-model-binders-in-an-mvc-application.md)</span></span>
