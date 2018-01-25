---
uid: mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
title: "ASP.NET MVC 5 應用程式生命週期 |Microsoft 文件"
author: cephalin
description: "下載的 ASP.NET MVC 5 應用程式生命週期的圖表顯示的 PDF 文件。 生命週期本文提供 MVC 生命週期的高層級檢視..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/28/2014
ms.topic: article
ms.assetid: 9c1e3a75-b644-4480-8326-11300b1ec4b3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
msc.type: authoredcontent
ms.openlocfilehash: 50d58d10c11677fa72ede6a03e686cbde4cbae1d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="lifecycle-of-an-aspnet-mvc-5-application"></a><span data-ttu-id="d879c-104">ASP.NET MVC 5 應用程式生命週期</span><span class="sxs-lookup"><span data-stu-id="d879c-104">Lifecycle of an ASP.NET MVC 5 Application</span></span>
====================
<span data-ttu-id="d879c-105">由[Cephas 連結](https://github.com/cephalin)</span><span class="sxs-lookup"><span data-stu-id="d879c-105">by [Cephas Lin](https://github.com/cephalin)</span></span>

[<span data-ttu-id="d879c-106">下載 PDF 文件</span><span class="sxs-lookup"><span data-stu-id="d879c-106">Download PDF Document</span></span>](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)

<span data-ttu-id="d879c-107">您可以在這裡下載的圖表的生命週期的每個 ASP.NET MVC 5 應用程式，使其無法接收 HTTP 要求傳送 HTTP 回應傳回至用戶端的 PDF 文件。</span><span class="sxs-lookup"><span data-stu-id="d879c-107">Here you can download a PDF document that charts the lifecycle of every ASP.NET MVC 5 application, from receiving the HTTP request to sending the HTTP response back to the client.</span></span> <span data-ttu-id="d879c-108">其設計是做為那些新 ASP.NET mvc 的教育性工具和也需要向下鑽研的應用程式的特定層面的使用者的參考。</span><span class="sxs-lookup"><span data-stu-id="d879c-108">It is designed both as an educational tool for those who are new to ASP.NET MVC and also as a reference for those who need to drill into specific aspects of the application.</span></span> <span data-ttu-id="d879c-109">PDF 文件具有下列功能：</span><span class="sxs-lookup"><span data-stu-id="d879c-109">The PDF document has the following features:</span></span>

- <span data-ttu-id="d879c-110">相關[HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication.aspx)階段，協助您了解 MVC 整合至[ASP.NET 應用程式生命週期](https://msdn.microsoft.com/library/bb470252.aspx)。</span><span class="sxs-lookup"><span data-stu-id="d879c-110">Relevant [HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) stages to help you understand where MVC integrates into the [ASP.NET application lifecycle](https://msdn.microsoft.com/library/bb470252.aspx).</span></span>
- <span data-ttu-id="d879c-111">MVC 應用程式生命週期，您將可以了解每個 MVC 應用程式要求處理管線中通過的主要階段高層級檢視。</span><span class="sxs-lookup"><span data-stu-id="d879c-111">A high-level view of the MVC application lifecycle, where you can understand the major stages that every MVC application passes through in the request processing pipeline.</span></span>  
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image1.jpg)
- <span data-ttu-id="d879c-112">詳細資料檢視會顯示向下的向下切入到要求處理管線的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="d879c-112">A detail view that shows drills down into the details of the request processing pipeline.</span></span> <span data-ttu-id="d879c-113">您可以比較高層級檢視，以查看如何生命週期的詳細資料會收集到的不同階段的詳細資料檢視。</span><span class="sxs-lookup"><span data-stu-id="d879c-113">You can compare the high-level view and the detail view to see how the lifecycles details are collected into the various stages.</span></span> <span data-ttu-id="d879c-114">[下載 PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)若要查看較大的檢視。</span><span class="sxs-lookup"><span data-stu-id="d879c-114">[Download PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf) to see a larger view.</span></span>
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image2.jpg)
- <span data-ttu-id="d879c-115">放置和上的所有可覆寫方法的目的[控制器](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx)要求處理管線中的物件。</span><span class="sxs-lookup"><span data-stu-id="d879c-115">Placement and purpose of all overridable methods on the [Controller](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx) object in the request processing pipeline.</span></span> <span data-ttu-id="d879c-116">您可能會或可能不需要覆寫任何一種方法，但請務必了解在應用程式生命週期中的角色，讓您可以撰寫程式碼，在適當的生命週期階段，您想要的效果。</span><span class="sxs-lookup"><span data-stu-id="d879c-116">You may or may not have the need to override any one method, but it is important for you to understand their role in the application lifecycle so that you can write code at the appropriate life cycle stage for the effect you intend.</span></span>
- <span data-ttu-id="d879c-117">顯示每個篩選器類型 （驗證、 授權、 動作和結果） 叫用方式爆向上圖表。</span><span class="sxs-lookup"><span data-stu-id="d879c-117">Blown-up diagrams showing how each of the filter types (authentication, authorization, action, and result) is invoked.</span></span>
- <span data-ttu-id="d879c-118">從詳細資料檢視中的每個點，以連結到有用的發行項或部落格。</span><span class="sxs-lookup"><span data-stu-id="d879c-118">Link to a useful article or blog from each point of interest in the detail view.</span></span>


## <a name="next-steps"></a><span data-ttu-id="d879c-119">後續步驟</span><span class="sxs-lookup"><span data-stu-id="d879c-119">Next Steps</span></span>

<span data-ttu-id="d879c-120">這份文件是否符合您的需求？</span><span class="sxs-lookup"><span data-stu-id="d879c-120">Does this document meet your need?</span></span> <span data-ttu-id="d879c-121">我們非常感謝您的意見反應。</span><span class="sxs-lookup"><span data-stu-id="d879c-121">We'd appreciate your feedback.</span></span> <span data-ttu-id="d879c-122">如果您有任何問題在 ASP.NET MVC 生命週期應用程式中[Stackoverflow](http://stackoverflow.com/help)和[ASP.NET MVC 論壇](https://forums.asp.net/1146.aspx)所要求的好地方。</span><span class="sxs-lookup"><span data-stu-id="d879c-122">If you have any question on the ASP.NET MVC lifecycle in your application, [Stackoverflow](http://stackoverflow.com/help) and the [ASP.NET MVC forums](https://forums.asp.net/1146.aspx) are great places to ask.</span></span> <span data-ttu-id="d879c-123">請遵循[我](https://twitter.com/Cephas_MSFT)因此我最新的教學課程中的更新可能會發生在 twitter 上。</span><span class="sxs-lookup"><span data-stu-id="d879c-123">Follow [me](https://twitter.com/Cephas_MSFT) on twitter so you can get updates on my latest tutorials.</span></span>
