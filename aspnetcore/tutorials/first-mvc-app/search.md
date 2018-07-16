---
title: 將搜尋新增至 ASP.NET Core MVC 應用程式
author: rick-anderson
description: 示範如何將搜尋新增至簡易的 ASP.NET Core MVC 應用程式
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/search
ms.openlocfilehash: d0581142b505323385feba441b707d29b3f6db5d
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/11/2018
ms.locfileid: "38216192"
---
[!INCLUDE [adding-model](~/includes/mvc-intro/search1.md)]

<span data-ttu-id="908fd-103">您可以使用 **rename** 命令，快速將 `searchString` 參數重新命名為 `id`。</span><span class="sxs-lookup"><span data-stu-id="908fd-103">You can quickly rename the `searchString` parameter to `id` with the **rename** command.</span></span> <span data-ttu-id="908fd-104">以滑鼠右鍵按一下 `searchString` > [重新命名]。</span><span class="sxs-lookup"><span data-stu-id="908fd-104">Right click on `searchString` **> Rename**.</span></span>

![操作功能表](search/_static/rename.png)

<span data-ttu-id="908fd-106">系統會反白顯示重新命名的目標。</span><span class="sxs-lookup"><span data-stu-id="908fd-106">The rename targets are highlighted.</span></span>

![程式碼編輯器，其中顯示整個 Index ActionResult 方法中反白顯示的變數](search/_static/rename2.png)

<span data-ttu-id="908fd-108">將參數變更為 `id`，並將所有出現的 `searchString` 變更為 `id`。</span><span class="sxs-lookup"><span data-stu-id="908fd-108">Change the parameter to `id` and all occurrences of `searchString` change to `id`.</span></span>

![程式碼編輯器，其中顯示變數已變更為 id](search/_static/rename3.png)

[!INCLUDE [adding-model](~/includes/mvc-intro/search2.md)]

<span data-ttu-id="908fd-110">請注意 IntelliSense 如何幫助我們更新標記。</span><span class="sxs-lookup"><span data-stu-id="908fd-110">Notice how intelliSense helps us update the markup.</span></span>

![IntelliSense 的操作功能表，其中已選取表單項目屬性清單中的 method](search/_static/int_m.png)

![IntelliSense 的操作功能表，其中已選取 method 屬性值清單中的 get](search/_static/int_get.png)

<span data-ttu-id="908fd-113">請注意 `<form>` 標記中的特殊字型。</span><span class="sxs-lookup"><span data-stu-id="908fd-113">Notice the distinctive font in the `<form>` tag.</span></span> <span data-ttu-id="908fd-114">該特殊字型表示[標記協助程式](~/mvc/views/tag-helpers/intro.md)可支援此標記。</span><span class="sxs-lookup"><span data-stu-id="908fd-114">That distinctive font indicates the tag is supported by [Tag Helpers](~/mvc/views/tag-helpers/intro.md).</span></span>

![含紫色文字的表單標記](search/_static/th_font.png)

[!INCLUDE [adding-model](~/includes/mvc-intro/search3.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="908fd-116">[上一頁](controller-methods-views.md)
> [下一頁](new-field.md)</span><span class="sxs-lookup"><span data-stu-id="908fd-116">[Previous](controller-methods-views.md)
[Next](new-field.md)</span></span>  
