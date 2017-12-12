---
uid: web-forms/videos/how-do-i/how-do-i-map-an-aspnet-server-control-to-the-adaptor-used-to-render-it
title: "[如何:]配接器，用來呈現其對應的 ASP.NET 伺服器控制項 |Microsoft 文件"
author: rick-anderson
description: "在這個視訊 Chris Pels 將示範如何使用控制項的介面卡的 ASP.NET 伺服器控制項提供不同的轉譯，而不實際變更 c..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/19/2008
ms.topic: article
ms.assetid: d4b498ef-8e1c-4fa2-9c35-1f32f20bb9b7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-map-an-aspnet-server-control-to-the-adaptor-used-to-render-it
msc.type: video
ms.openlocfilehash: 675874da54fac688fa4df70ea1efaba477d30139
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="how-do-i-map-an-aspnet-server-control-to-the-adaptor-used-to-render-it"></a><span data-ttu-id="82a52-103">[如何:]配接器，用來呈現其對應的 ASP.NET 伺服器控制項</span><span class="sxs-lookup"><span data-stu-id="82a52-103">[How Do I:] Map an ASP.NET Server Control to the Adaptor Used to Render It</span></span>
====================
<span data-ttu-id="82a52-104">由[Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="82a52-104">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="82a52-105">在這個視訊 Chris Pels 將示範如何使用 ASP.NET 伺服器控制項提供不同的轉譯，而不會實際變更控制項本身的控制項配接器。</span><span class="sxs-lookup"><span data-stu-id="82a52-105">In this video Chris Pels will show how to use a control adaptor to provide different renderings for an ASP.NET server control without actually changing the control itself.</span></span> <span data-ttu-id="82a52-106">在這段影片中，ASP.NET BulletList 控制會調整以顯示每個清單項目，以水平方式使用 DIV 項目，而不傳統的 UL 項目。</span><span class="sxs-lookup"><span data-stu-id="82a52-106">In this video, an ASP.NET BulletList control will be adapted to display each list item horizontally using DIV elements instead of the traditional UL elements.</span></span> <span data-ttu-id="82a52-107">首先，請參閱 < 如何建立繼承 WebControlAdaptor，然後實作來呈現在新的清單格式的程式碼的類別。</span><span class="sxs-lookup"><span data-stu-id="82a52-107">First, see how to create a class that inherits WebControlAdaptor and then implements the code to render the new list format.</span></span> <span data-ttu-id="82a52-108">接下來，了解如何將新的控制項配接器對應至 ASP.NET 伺服器控制項.browser 定義檔中。</span><span class="sxs-lookup"><span data-stu-id="82a52-108">Next, learn how to map the new control adaptor to the ASP.NET server control in the .browser definition file.</span></span> <span data-ttu-id="82a52-109">然後了解如何在網站上的頁面上，使用新的控制項配接器。</span><span class="sxs-lookup"><span data-stu-id="82a52-109">Then see how to use the new control adaptor on pages in a web site.</span></span> <span data-ttu-id="82a52-110">最後，了解如何控制配接器可以與所有瀏覽器或特定類型的瀏覽器相關聯。</span><span class="sxs-lookup"><span data-stu-id="82a52-110">Finally, learn how a control adaptor can be associated with either all browsers or specific types of browsers.</span></span>

[<span data-ttu-id="82a52-111">&#9654;觀看影片 （23 分鐘）</span><span class="sxs-lookup"><span data-stu-id="82a52-111">&#9654; Watch video (23 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-map-an-aspnet-server-control-to-the-adaptor-used-to-render-it)
