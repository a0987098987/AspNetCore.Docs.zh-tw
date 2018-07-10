---
title: 將搜尋新增至 ASP.NET Core MVC 應用程式
author: rick-anderson
description: 示範如何將搜尋新增至簡易的 ASP.NET Core MVC 應用程式
ms.author: riande
ms.date: 04/07/2017
uid: tutorials/first-mvc-app-mac/search
ms.openlocfilehash: aca0835340977605cc84fad1970ac30fa1a9872a
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961538"
---
[!INCLUDE [adding-model](../../includes/mvc-intro/search1.md)]

<span data-ttu-id="d8750-103">注意：SQLlite 有區分大小寫，因此您必須搜尋 "Ghost" 而不是 "ghost"。</span><span class="sxs-lookup"><span data-stu-id="d8750-103">Note: SQLlite is case sensitive, so you'll need to search for "Ghost" and not "ghost".</span></span>

[!INCLUDE [adding-model](../../includes/mvc-intro/search2.md)]

<span data-ttu-id="d8750-104">變更 *Views\movie\Index.cshtml* Razor 檢視中的 `<form>` 標記，以指定 `method="get"`：</span><span class="sxs-lookup"><span data-stu-id="d8750-104">Change the `<form>` tag in the *Views\movie\Index.cshtml* Razor view to specify `method="get"`:</span></span>

```html
<form asp-controller="Movies" asp-action="Index" method="get">
```

[!INCLUDE [adding-model](../../includes/mvc-intro/search3.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="d8750-105">[上一步 - 控制器方法和檢視](controller-methods-views.md)
> [下一步 - 新增欄位](new-field.md)</span><span class="sxs-lookup"><span data-stu-id="d8750-105">[Previous - Controller methods and views](controller-methods-views.md)
[Next - Add a field](new-field.md)</span></span>
