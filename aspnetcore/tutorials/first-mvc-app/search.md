---
title: "新增搜尋"
author: rick-anderson
description: "示範如何將搜尋新增至簡易的 ASP.NET Core MVC 應用程式"
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/search
ms.openlocfilehash: 3ab9086275ec4c3651383c4c845e40db55f67f4c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
[!INCLUDE[adding-model](../../includes/mvc-intro/search1.md)]

您可以使用 **rename** 命令，快速將 `searchString` 參數重新命名為 `id`。 以滑鼠右鍵按一下 `searchString` > [重新命名]。

![操作功能表](search/_static/rename.png)

系統會反白顯示重新命名的目標。

![程式碼編輯器，其中顯示整個 Index ActionResult 方法中反白顯示的變數](search/_static/rename2.png)

將參數變更為 `id`，並將所有出現的 `searchString` 變更為 `id`。

![程式碼編輯器，其中顯示變數已變更為 id](search/_static/rename3.png)

[!INCLUDE[adding-model](../../includes/mvc-intro/search2.md)]

請注意 IntelliSense 如何幫助我們更新標記。

![IntelliSense 的操作功能表，其中已選取表單項目屬性清單中的 method](search/_static/int_m.png)

![IntelliSense 的操作功能表，其中已選取 method 屬性值清單中的 get](search/_static/int_get.png)

請注意 `<form>` 標記中的特殊字型。 該特殊字型表示[標記協助程式](../../mvc/views/tag-helpers/intro.md)可支援此標記。

![含紫色文字的表單標記](search/_static/th_font.png)

[!INCLUDE[adding-model](../../includes/mvc-intro/search3.md)]

>[!div class="step-by-step"]
[上一頁](controller-methods-views.md)
[下一頁](new-field.md)  
