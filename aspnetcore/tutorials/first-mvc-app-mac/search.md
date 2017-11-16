---
title: "將搜尋新增至 ASP.NET Core MVC 應用程式"
author: rick-anderson
description: "示範如何將搜尋新增至簡易的 ASP.NET Core MVC 應用程式"
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 04/07/2017
ms.topic: get-started-article
ms.assetid: d69e5529-ffff-4628-855d-200202096269
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-mac/search
ms.openlocfilehash: 8dab5293ab6a6fe65288bb230e4f39af462ba28b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
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
