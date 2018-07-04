---
uid: mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
title: ASP.NET MVC 5 應用程式的生命週期 |Microsoft Docs
author: cephalin
description: 下載圖表上的 ASP.NET MVC 5 應用程式的生命週期的 PDF 文件。 此生命週期文件提供 MVC 生命週期的高層級檢視...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/28/2014
ms.topic: article
ms.assetid: 9c1e3a75-b644-4480-8326-11300b1ec4b3
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
msc.type: authoredcontent
ms.openlocfilehash: 98735f2e04bdd0f2fec19524e59f6272dbc4ca57
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393911"
---
<a name="lifecycle-of-an-aspnet-mvc-5-application"></a><span data-ttu-id="823d1-104">ASP.NET MVC 5 應用程式的生命週期</span><span class="sxs-lookup"><span data-stu-id="823d1-104">Lifecycle of an ASP.NET MVC 5 Application</span></span>
====================
<span data-ttu-id="823d1-105">藉由[Cephas 連結](https://github.com/cephalin)</span><span class="sxs-lookup"><span data-stu-id="823d1-105">by [Cephas Lin](https://github.com/cephalin)</span></span>

[<span data-ttu-id="823d1-106">下載 PDF 文件</span><span class="sxs-lookup"><span data-stu-id="823d1-106">Download PDF Document</span></span>](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)

<span data-ttu-id="823d1-107">您可以在這裡下載的圖表生命週期的每個 ASP.NET MVC 5 應用程式，從接收 HTTP 要求傳送 HTTP 回應傳回至用戶端的 PDF 文件。</span><span class="sxs-lookup"><span data-stu-id="823d1-107">Here you can download a PDF document that charts the lifecycle of every ASP.NET MVC 5 application, from receiving the HTTP request to sending the HTTP response back to the client.</span></span> <span data-ttu-id="823d1-108">它是作為教育性工具的人不熟悉 ASP.NET MVC 和也需要向下鑽研至應用程式的特定層面的參考。</span><span class="sxs-lookup"><span data-stu-id="823d1-108">It is designed both as an educational tool for those who are new to ASP.NET MVC and also as a reference for those who need to drill into specific aspects of the application.</span></span> <span data-ttu-id="823d1-109">PDF 文件具有下列功能：</span><span class="sxs-lookup"><span data-stu-id="823d1-109">The PDF document has the following features:</span></span>

- <span data-ttu-id="823d1-110">相關[HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication.aspx)協助您了解 MVC 整合到階段[ASP.NET 應用程式生命週期](https://msdn.microsoft.com/library/bb470252.aspx)。</span><span class="sxs-lookup"><span data-stu-id="823d1-110">Relevant [HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) stages to help you understand where MVC integrates into the [ASP.NET application lifecycle](https://msdn.microsoft.com/library/bb470252.aspx).</span></span>
- <span data-ttu-id="823d1-111">MVC 應用程式生命週期中，您將可以了解每個 MVC 應用程式通過要求處理管線中的主要階段高層級檢視。</span><span class="sxs-lookup"><span data-stu-id="823d1-111">A high-level view of the MVC application lifecycle, where you can understand the major stages that every MVC application passes through in the request processing pipeline.</span></span>  
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image1.jpg)
- <span data-ttu-id="823d1-112">詳細資料檢視會顯示向下鑽研，要求處理管線的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="823d1-112">A detail view that shows drills down into the details of the request processing pipeline.</span></span> <span data-ttu-id="823d1-113">您可以比較高層級檢視，以查看如何生命週期的詳細資料會收集到的各個階段的詳細資料檢視。</span><span class="sxs-lookup"><span data-stu-id="823d1-113">You can compare the high-level view and the detail view to see how the lifecycles details are collected into the various stages.</span></span> <span data-ttu-id="823d1-114">[下載 PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)若要查看較大的檢視。</span><span class="sxs-lookup"><span data-stu-id="823d1-114">[Download PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf) to see a larger view.</span></span>
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image2.jpg)
- <span data-ttu-id="823d1-115">位置與用途上的所有可覆寫方法[控制器](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx)要求處理管線中的物件。</span><span class="sxs-lookup"><span data-stu-id="823d1-115">Placement and purpose of all overridable methods on the [Controller](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx) object in the request processing pipeline.</span></span> <span data-ttu-id="823d1-116">您可能會或可能沒有需要覆寫任何一種方法，但務必了解在應用程式生命週期中的角色，以便您可以撰寫程式碼，在適當的生命週期階段，您想要的效果。</span><span class="sxs-lookup"><span data-stu-id="823d1-116">You may or may not have the need to override any one method, but it is important for you to understand their role in the application lifecycle so that you can write code at the appropriate life cycle stage for the effect you intend.</span></span>
- <span data-ttu-id="823d1-117">震撼人心向上圖表，顯示每個篩選器類型 （驗證、 授權、 動作和結果） 叫用方式。</span><span class="sxs-lookup"><span data-stu-id="823d1-117">Blown-up diagrams showing how each of the filter types (authentication, authorization, action, and result) is invoked.</span></span>
- <span data-ttu-id="823d1-118">從 詳細資料檢視中的每個點，以連結至篇實用的文章或部落格。</span><span class="sxs-lookup"><span data-stu-id="823d1-118">Link to a useful article or blog from each point of interest in the detail view.</span></span>


## <a name="next-steps"></a><span data-ttu-id="823d1-119">後續步驟</span><span class="sxs-lookup"><span data-stu-id="823d1-119">Next Steps</span></span>

<span data-ttu-id="823d1-120">這份文件是否符合您的需求？</span><span class="sxs-lookup"><span data-stu-id="823d1-120">Does this document meet your need?</span></span> <span data-ttu-id="823d1-121">我們非常感謝您的意見反應。</span><span class="sxs-lookup"><span data-stu-id="823d1-121">We'd appreciate your feedback.</span></span> <span data-ttu-id="823d1-122">如果您有任何問題的 ASP.NET MVC 生命週期中您的應用程式[Stackoverflow](http://stackoverflow.com/help)並[ASP.NET MVC 論壇](https://forums.asp.net/1146.aspx)是很棒的地方，要求。</span><span class="sxs-lookup"><span data-stu-id="823d1-122">If you have any question on the ASP.NET MVC lifecycle in your application, [Stackoverflow](http://stackoverflow.com/help) and the [ASP.NET MVC forums](https://forums.asp.net/1146.aspx) are great places to ask.</span></span> <span data-ttu-id="823d1-123">請遵循[我](https://twitter.com/Cephas_MSFT)twitter，因此您可以取得我最新的教學課程的更新。</span><span class="sxs-lookup"><span data-stu-id="823d1-123">Follow [me](https://twitter.com/Cephas_MSFT) on twitter so you can get updates on my latest tutorials.</span></span>
