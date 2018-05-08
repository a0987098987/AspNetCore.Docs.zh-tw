---
title: 將搜尋新增至 ASP.NET Core MVC 應用程式
author: rick-anderson
description: 示範如何將搜尋新增至簡易的 ASP.NET Core MVC 應用程式
manager: wpickett
ms.author: riande
ms.date: 04/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-mac/search
ms.openlocfilehash: 3f648bc6c6d095b9fe8b6ac65bf5f51741938ed1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
[!INCLUDE [adding-model](../../includes/mvc-intro/search1.md)]

<span data-ttu-id="42822-103">注意：SQLlite 有區分大小寫，因此您必須搜尋 "Ghost" 而不是 "ghost"。</span><span class="sxs-lookup"><span data-stu-id="42822-103">Note: SQLlite is case sensitive, so you'll need to search for "Ghost" and not "ghost".</span></span>

[!INCLUDE [adding-model](../../includes/mvc-intro/search2.md)]

<span data-ttu-id="42822-104">變更 *Views\movie\Index.cshtml* Razor 檢視中的 `<form>` 標記，以指定 `method="get"`：</span><span class="sxs-lookup"><span data-stu-id="42822-104">Change the `<form>` tag in the *Views\movie\Index.cshtml* Razor view to specify `method="get"`:</span></span>

```html
<form asp-controller="Movies" asp-action="Index" method="get">
```

[!INCLUDE [adding-model](../../includes/mvc-intro/search3.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="42822-105">[上一步 - 控制器方法和檢視](controller-methods-views.md)
> [下一步 - 新增欄位](new-field.md)</span><span class="sxs-lookup"><span data-stu-id="42822-105">[Previous - Controller methods and views](controller-methods-views.md)
[Next - Add a field](new-field.md)</span></span>
