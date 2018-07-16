---
title: ASP.NET Core 中的控制器方法和檢視
author: rick-anderson
description: 了解如何在 ASP.NET Core 中使用控制器方法、檢視和 DataAnnotations。
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: e94cb877576a68540a565225b2b3d79f9be53327
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/11/2018
ms.locfileid: "38194010"
---
# <a name="controller-methods-and-views-in-aspnet-core"></a>ASP.NET Core 中的控制器方法和檢視

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

剛開始使用此電影應用程式還不錯，但其呈現效果卻不理想。 我們不想看到時間 (下圖的 12:00:00 AM)，而且 **ReleaseDate** 應該是兩個分開的字。

![索引檢視：Release Date (發行日期) 是一個字 (不含空格)，且每個電影的發行日期均顯示 12 AM 的時間](working-with-sql/_static/m55.png)

開啟 *Models/Movie.cs* 檔案，然後新增反白顯示的程式碼行，如下所示：

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](start-mvc/sample/MvcMovie21/Models/MovieDateFixed.cs?name=snippet_1&highlight=2,3,12-13,17)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]
::: moniker-end

[!INCLUDE [adding-model](~/includes/mvc-intro/controller-methods-views.md)]

> [!div class="step-by-step"]
> [上一頁](working-with-sql.md)
> [下一頁](search.md)  
