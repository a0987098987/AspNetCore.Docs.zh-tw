---
uid: web-forms/videos/aspnet-ajax/how-do-i-implement-the-persistent-communications-pattern-with-the-updatepanel
title: '[How Do i:]實作與 UpdatePanel 的持續性通訊模式？ | Microsoft Docs'
author: JoeStagner
description: 在傳統的網站上瀏覽器和伺服器不會維護進行中的通訊，但只在執行動作的使用者回應通訊...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/01/2007
ms.topic: article
ms.assetid: 49c7a74d-dce7-4d5c-8282-c7846f478e11
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/videos/aspnet-ajax/how-do-i-implement-the-persistent-communications-pattern-with-the-updatepanel
msc.type: video
ms.openlocfilehash: 3812b41085f6ad0e08bd37599af845cfa4ff08e8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380548"
---
<a name="how-do-i-implement-the-persistent-communications-pattern-with-the-updatepanel"></a><span data-ttu-id="66717-104">[How Do i:]實作與 UpdatePanel 的持續性通訊模式？</span><span class="sxs-lookup"><span data-stu-id="66717-104">[How Do I:] Implement the Persistent Communications Pattern with the UpdatePanel?</span></span>
====================
<span data-ttu-id="66717-105">藉由[Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="66717-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

<span data-ttu-id="66717-106">在傳統的網站上瀏覽器和伺服器不會維護進行中的通訊，但只會在使用者執行動作的回應進行通訊。</span><span class="sxs-lookup"><span data-stu-id="66717-106">In a traditional Web site the browser and the server do not maintain an ongoing communication, but communicate only in response to the user performing an action.</span></span> <span data-ttu-id="66717-107">在新式網站頁面，就會變成應用程式容器，很有幫助瀏覽器和伺服器維護進行中的通訊，以便進行頁面更新而不需要執行動作的使用者。</span><span class="sxs-lookup"><span data-stu-id="66717-107">In a modern Web site where the page becomes an application container, it can be advantageous for the browser and the server to maintain an ongoing communication so that page updates can occur without the user performing an action.</span></span> <span data-ttu-id="66717-108">這就是持續性通訊模式的 AJAX。</span><span class="sxs-lookup"><span data-stu-id="66717-108">This is known as the Persistent Communications Pattern for AJAX.</span></span> <span data-ttu-id="66717-109">ASP.NET AJAX 會提供兩種主要的方式來實作持續性通訊模式的 Web 開發人員。</span><span class="sxs-lookup"><span data-stu-id="66717-109">ASP.NET AJAX provides two main ways for Web developers to implement the Persistent Communications Pattern.</span></span> <span data-ttu-id="66717-110">這部影片示範簡單的方式，就是使用 ASP.NET AJAX UpdatePanel 作為實作的基礎。</span><span class="sxs-lookup"><span data-stu-id="66717-110">This video demonstrates the simple way, which is to use the ASP.NET AJAX UpdatePanel as the basis of the implementation.</span></span> <span data-ttu-id="66717-111">在稍後影片中，我們將了解如何實作相同的模式，而不使用 ASP.NET AJAX UpdatePanel。</span><span class="sxs-lookup"><span data-stu-id="66717-111">In a later video we will learn how to implement the same pattern without the use of the ASP.NET AJAX UpdatePanel.</span></span>

[<span data-ttu-id="66717-112">&#9654;觀看影片 （12 分鐘）</span><span class="sxs-lookup"><span data-stu-id="66717-112">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-implement-the-persistent-communications-pattern-with-the-updatepanel)

> [!div class="step-by-step"]
> <span data-ttu-id="66717-113">[上一頁](how-do-i-use-the-conditional-updatemode-of-the-updatepanel.md)
> [下一頁](how-do-i-localize-an-aspnet-ajax-application.md)</span><span class="sxs-lookup"><span data-stu-id="66717-113">[Previous](how-do-i-use-the-conditional-updatemode-of-the-updatepanel.md)
[Next](how-do-i-localize-an-aspnet-ajax-application.md)</span></span>
