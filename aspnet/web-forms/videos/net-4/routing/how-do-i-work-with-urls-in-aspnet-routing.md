---
uid: web-forms/videos/net-4/routing/how-do-i-work-with-urls-in-aspnet-routing
title: 如何： 使用 ASP.NET 路由中的 Url 嗎？ | Microsoft Docs
author: rick-anderson
description: 在這段影片 Chris Pels 會示範如何使用 ASP.NET 路由的網站中指定的 Url。 首先，在建立網站，且路由定義在 Gl....
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/15/2010
ms.topic: article
ms.assetid: 08f9d0a7-cfa0-4914-a672-8a64295d7ba8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/net-4/routing/how-do-i-work-with-urls-in-aspnet-routing
msc.type: video
ms.openlocfilehash: 3d87db9589dc5d330a29b3a25546dc65234f5f41
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30893881"
---
<a name="how-do-i-work-with-urls-in-aspnet-routing"></a><span data-ttu-id="6930e-105">如何： 使用 ASP.NET 路由中的 Url 嗎？</span><span class="sxs-lookup"><span data-stu-id="6930e-105">How Do I: Work with URLs in ASP.NET Routing?</span></span>
====================
<span data-ttu-id="6930e-106">由[Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="6930e-106">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="6930e-107">在這段影片 Chris Pels 會示範如何使用 ASP.NET 路由的網站中指定的 Url。</span><span class="sxs-lookup"><span data-stu-id="6930e-107">In this video Chris Pels shows how to specify URLs in a web site that utilizes ASP.NET routing.</span></span> <span data-ttu-id="6930e-108">首先，建立網站，且路由定義在全域應用程式類別 (.asax)。</span><span class="sxs-lookup"><span data-stu-id="6930e-108">First, a web site is created and routing is defined in the Global Application Class (.asax).</span></span> <span data-ttu-id="6930e-109">接下來，網頁範例會建立，並根據定義的路由 URL 新增至使用的標準 「 硬式編碼 」 方法，例如，"~/Stats/Visitors 」 的頁面。</span><span class="sxs-lookup"><span data-stu-id="6930e-109">Next, a sample web page is created and a URL based upon a defined route is added to the page using the standard "hard coded" approach, e.g., "~/Stats/Visitors".</span></span> <span data-ttu-id="6930e-110">另一個連結接著會新增至網頁，這會使用 RouteValue 方法可接受的路由名稱和參數的標記中動態產生相同的 URL。</span><span class="sxs-lookup"><span data-stu-id="6930e-110">Another link is then added to the page which dynamically generates the same URL in markup using the RouteValue method which accepts the route name and parameters.</span></span> <span data-ttu-id="6930e-111">直接在網頁中使用程式碼，而非標記，則實作相同的 URL。</span><span class="sxs-lookup"><span data-stu-id="6930e-111">The same URL is then implemented using code rather than markup directly in the page.</span></span> <span data-ttu-id="6930e-112">然後變更實體頁面的位置與原始路由，不會再產生硬式編碼的連結中使用而同時動態產生連結函式正確。</span><span class="sxs-lookup"><span data-stu-id="6930e-112">The original route and physical page location are then changed, resulting in the hard coded link no longer working whereas both dynamically generated links function properly.</span></span> <span data-ttu-id="6930e-113">最後，再討論動態產生連結的值。</span><span class="sxs-lookup"><span data-stu-id="6930e-113">Finally, the value of dynamically generated links is then discussed.</span></span>

[<span data-ttu-id="6930e-114">&#9654;觀看影片 （20 分鐘）</span><span class="sxs-lookup"><span data-stu-id="6930e-114">&#9654; Watch video (20 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-work-with-urls-in-aspnet-routing)

> [!div class="step-by-step"]
> [<span data-ttu-id="6930e-115">上一步</span><span class="sxs-lookup"><span data-stu-id="6930e-115">Previous</span></span>](how-do-i-use-routing-with-aspnet-web-forms.md)
