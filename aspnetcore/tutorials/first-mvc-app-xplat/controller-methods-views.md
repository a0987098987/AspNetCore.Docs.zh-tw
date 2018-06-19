---
title: ASP.NET Core 中的控制器方法和檢視
author: rick-anderson
description: 了解如何在 ASP.NET Core 中使用控制器方法、檢視和 DataAnnotations。
manager: wpickett
ms.author: riande
ms.date: 04/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-xplat/controller-methods-views
ms.openlocfilehash: 0bf9bffbf14ff958b28d9494600f55eb3f8e0c35
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30896935"
---
# <a name="controller-methods-and-views-in-aspnet-core"></a><span data-ttu-id="946b5-103">ASP.NET Core 中的控制器方法和檢視</span><span class="sxs-lookup"><span data-stu-id="946b5-103">Controller methods and views in ASP.NET Core</span></span>

<span data-ttu-id="946b5-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="946b5-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="946b5-105">剛開始使用此電影應用程式還不錯，但其呈現效果卻不理想。</span><span class="sxs-lookup"><span data-stu-id="946b5-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="946b5-106">我們不想看到時間 (下圖的 12:00:00 AM)，而且 **ReleaseDate** 應該是兩個分開的字。</span><span class="sxs-lookup"><span data-stu-id="946b5-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![索引檢視：Release Date (發行日期) 是一個字 (不含空格)，且每個電影的發行日期均顯示 12 AM 的時間](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

<span data-ttu-id="946b5-108">開啟 *Models/Movie.cs* 檔案，然後新增反白顯示的程式碼行，如下所示：</span><span class="sxs-lookup"><span data-stu-id="946b5-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1&highlight=2,11-12)]

<span data-ttu-id="946b5-109">建置和執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="946b5-109">Build and run the app.</span></span>

<!-- include start
![MVC Movie application open browser showing movie data](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

 -->

[!INCLUDE [adding-model](../../includes/mvc-intro/controller-methods-views.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="946b5-110">[上一步 - 使用 SQLite](working-with-sql.md)
> [下一步 - 新增搜尋](search.md)</span><span class="sxs-lookup"><span data-stu-id="946b5-110">[Previous - Working with SQLite](working-with-sql.md)
[Next - Add search](search.md)</span></span>  
