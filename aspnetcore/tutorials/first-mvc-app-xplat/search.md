---
title: "新增搜尋"
author: rick-anderson
description: "示範如何將搜尋新增至簡易的 ASP.NET Core MVC 應用程式"
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 04/07/2017
ms.topic: get-started-article
ms.assetid: d69e5529-ffff-4628-855d-200206d96269
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-xplat/search
ms.openlocfilehash: 57419c697aea80a054e906c75002f5a39c512fc6
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2017
---
[!INCLUDE[adding-model](../../includes/mvc-intro/search1.md)]

<span data-ttu-id="4fe6e-104">注意：SQLlite 有區分大小寫，因此您必須搜尋 "Ghost" 而不是 "ghost"。</span><span class="sxs-lookup"><span data-stu-id="4fe6e-104">Note: SQLlite is case sensitive, so you'll need to search for "Ghost" and not "ghost".</span></span>

[!INCLUDE[adding-model](../../includes/mvc-intro/search2.md)]

<span data-ttu-id="4fe6e-105">變更 *Views\movie\Index.cshtml* Razor 檢視中的 `<form>` 標記，以指定 `method="get"`：</span><span class="sxs-lookup"><span data-stu-id="4fe6e-105">Change the `<form>` tag in the *Views\movie\Index.cshtml* Razor view to specify `method="get"`:</span></span>

```html
<form asp-controller="Movies" asp-action="Index" method="get">
```

[!INCLUDE[adding-model](../../includes/mvc-intro/search3.md)]

>[!div class="step-by-step"]
<span data-ttu-id="4fe6e-106">[上一步 - 控制器方法和檢視](controller-methods-views.md)
[下一步 - 新增欄位](new-field.md)</span><span class="sxs-lookup"><span data-stu-id="4fe6e-106">[Previous - Controller methods and views](controller-methods-views.md)
[Next - Add a field](new-field.md)</span></span>  
