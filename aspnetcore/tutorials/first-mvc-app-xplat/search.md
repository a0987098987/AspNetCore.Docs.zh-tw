---
title: 新增搜尋
author: rick-anderson
description: 示範如何將搜尋新增至簡易的 ASP.NET Core MVC 應用程式
ms.author: riande
ms.date: 04/07/2017
uid: tutorials/first-mvc-app-xplat/search
ms.openlocfilehash: dc84eb38c0487d90451979ec9572bf1641571357
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276002"
---
[!INCLUDE [adding-model](../../includes/mvc-intro/search1.md)]

注意：SQLlite 有區分大小寫，因此您必須搜尋 "Ghost" 而不是 "ghost"。

[!INCLUDE [adding-model](../../includes/mvc-intro/search2.md)]

變更 *Views\movie\Index.cshtml* Razor 檢視中的 `<form>` 標記，以指定 `method="get"`：

```html
<form asp-controller="Movies" asp-action="Index" method="get">
```

[!INCLUDE [adding-model](../../includes/mvc-intro/search3.md)]

> [!div class="step-by-step"]
> [上一步 - 控制器方法和檢視](controller-methods-views.md)
> [下一步 - 新增欄位](new-field.md)  
