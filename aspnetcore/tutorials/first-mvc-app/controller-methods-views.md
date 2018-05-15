---
title: ASP.NET Core 中的控制器方法和檢視
author: rick-anderson
description: 了解如何在 ASP.NET Core 中使用控制器方法、檢視和 DataAnnotations。
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: 6fe0a0e71079bebcbd3a76abee0f2917f562e766
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
# <a name="controller-methods-and-views-in-aspnet-core"></a><span data-ttu-id="bd668-103">ASP.NET Core 中的控制器方法和檢視</span><span class="sxs-lookup"><span data-stu-id="bd668-103">Controller methods and views in ASP.NET Core</span></span>

<span data-ttu-id="bd668-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="bd668-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="bd668-105">剛開始使用此電影應用程式還不錯，但其呈現效果卻不理想。</span><span class="sxs-lookup"><span data-stu-id="bd668-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="bd668-106">我們不想看到時間 (下圖的 12:00:00 AM)，而且 **ReleaseDate** 應該是兩個分開的字。</span><span class="sxs-lookup"><span data-stu-id="bd668-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![索引檢視：Release Date (發行日期) 是一個字 (不含空格)，且每個電影的發行日期均顯示 12 AM 的時間](working-with-sql/_static/m55.png)

<span data-ttu-id="bd668-108">開啟 *Models/Movie.cs* 檔案，然後新增醒目提示的程式碼行，如下所示：</span><span class="sxs-lookup"><span data-stu-id="bd668-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]

<span data-ttu-id="bd668-109">以滑鼠右鍵按一下紅色曲線 > [Quick Actions and Refactorings] \(快速控制項目及重構)。</span><span class="sxs-lookup"><span data-stu-id="bd668-109">Right click on a red squiggly line **> Quick Actions and Refactorings**.</span></span>

  ![操作功能表隨即顯示 **> [Quick Actions and Refactorings] (快速控制項目及重構)**。](controller-methods-views/_static/qa.png)


<span data-ttu-id="bd668-111">點選 `using System.ComponentModel.DataAnnotations;`</span><span class="sxs-lookup"><span data-stu-id="bd668-111">Tap `using System.ComponentModel.DataAnnotations;`</span></span>

  ![使用清單頂端的 System.ComponentModel.DataAnnotations](controller-methods-views/_static/da.png)

  <span data-ttu-id="bd668-113">Visual Studio 即會新增 `using System.ComponentModel.DataAnnotations;`。</span><span class="sxs-lookup"><span data-stu-id="bd668-113">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

<span data-ttu-id="bd668-114">讓我們移除不需要的 `using` 陳述式。</span><span class="sxs-lookup"><span data-stu-id="bd668-114">Let's remove the `using` statements that are not needed.</span></span> <span data-ttu-id="bd668-115">預設會以淺灰色字型顯示這些陳述式。</span><span class="sxs-lookup"><span data-stu-id="bd668-115">They show up by default in a light grey font.</span></span> <span data-ttu-id="bd668-116">以滑鼠右鍵按一下 *Movie.cs* 檔案的任一處 > [移除和排序 Using]。</span><span class="sxs-lookup"><span data-stu-id="bd668-116">Right click anywhere in the *Movie.cs* file **> Remove and Sort Usings**.</span></span>

![移除和排序 Using](controller-methods-views/_static/rm.png)

<span data-ttu-id="bd668-118">更新過的程式碼：</span><span class="sxs-lookup"><span data-stu-id="bd668-118">The updated code:</span></span>

[!code-csharp[](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]

<!-- include start -->

[!INCLUDE [adding-model](../../includes/mvc-intro/controller-methods-views.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="bd668-119">[上一頁](working-with-sql.md)
> [下一頁](search.md)</span><span class="sxs-lookup"><span data-stu-id="bd668-119">[Previous](working-with-sql.md)
[Next](search.md)</span></span>  
