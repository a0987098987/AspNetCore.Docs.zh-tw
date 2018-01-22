---
title: "新增搜尋"
author: rick-anderson
description: "示範如何將搜尋新增至簡易的 ASP.NET Core MVC 應用程式"
ms.author: riande
manager: wpickett
ms.date: 04/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-xplat/search
ms.openlocfilehash: 2d8a18365a0d46d6468d708e1cd02def071309b7
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
[!INCLUDE[adding-model](../../includes/mvc-intro/search1.md)]

注意：SQLlite 有區分大小寫，因此您必須搜尋 "Ghost" 而不是 "ghost"。

[!INCLUDE[adding-model](../../includes/mvc-intro/search2.md)]

變更 *Views\movie\Index.cshtml* Razor 檢視中的 `<form>` 標記，以指定 `method="get"`：

```html
<form asp-controller="Movies" asp-action="Index" method="get">
```

[!INCLUDE[adding-model](../../includes/mvc-intro/search3.md)]

>[!div class="step-by-step"]
[上一步 - 控制器方法和檢視](controller-methods-views.md)
[下一步 - 新增欄位](new-field.md)  
