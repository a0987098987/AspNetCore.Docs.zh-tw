---
title: "ASP.NET Core MVC 應用程式中的控制器方法和檢視"
author: rick-anderson
description: "使用控制器方法、檢視和 DataAnnotations"
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 04/07/2017
ms.topic: get-started-article
ms.assetid: c7313211-babe-babe-babe-8e72603cc0ce
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-mac/controller-methods-views
ms.openlocfilehash: 846a78a09cdde0cfdbcf35bb899c1822ebe8fea2
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2017
---
# <a name="controller-methods-and-views-in-an-aspnet-core-mvc-app"></a>ASP.NET Core MVC 應用程式中的控制器方法和檢視

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

我們開始使用電影應用程式的情況很不錯，但呈現效果卻不理想。 我們不想看到時間 (下圖的 12:00:00 AM)，而且 **ReleaseDate** 應該是兩個分開的字。

![索引檢視：Release Date (發行日期) 是一個字 (不含空格)，且每個電影的發行日期均顯示 12 AM 的時間](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

開啟 *Models/Movie.cs* 檔案，然後新增反白顯示的程式碼行，如下所示：

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1&highlight=2,11-12)]

建置和執行應用程式。

<!-- include start
![MVC Movie application open browser showing movie data](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

 -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
[上一步 - 使用 SQLite](working-with-sql.md)
[下一步 - 新增搜尋](search.md)
