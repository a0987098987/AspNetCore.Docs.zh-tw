---
title: "控制器方法和檢視"
author: rick-anderson
description: "使用控制器方法、檢視和 DataAnnotations"
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: 200f02f9815966653b3b46918737c60d11f11d5a
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="controller-methods-and-views"></a>控制器方法和檢視

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

剛開始使用此電影應用程式還不錯，但其呈現效果卻不理想。 我們不想看到時間 (下圖的 12:00:00 AM)，而且 **ReleaseDate** 應該是兩個分開的字。

![索引檢視：Release Date (發行日期) 是一個字 (不含空格)，且每個電影的發行日期均顯示 12 AM 的時間](working-with-sql/_static/m55.png)

開啟 *Models/Movie.cs* 檔案，然後新增醒目提示的程式碼行，如下所示：

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]

以滑鼠右鍵按一下紅色曲線 > [Quick Actions and Refactorings] \(快速控制項目及重構)。

  ![操作功能表隨即顯示 **> [Quick Actions and Refactorings] (快速控制項目及重構)**。](controller-methods-views/_static/qa.png)


點選 `using System.ComponentModel.DataAnnotations;`

  ![使用清單頂端的 System.ComponentModel.DataAnnotations](controller-methods-views/_static/da.png)

  Visual Studio 即會新增 `using System.ComponentModel.DataAnnotations;`。

讓我們移除不需要的 `using` 陳述式。 預設會以淺灰色字型顯示這些陳述式。 以滑鼠右鍵按一下 *Movie.cs* 檔案的任一處 > [移除和排序 Using]。

![移除和排序 Using](controller-methods-views/_static/rm.png)

更新過的程式碼：

[!code-csharp[Main](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]

<!-- include start -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
[上一頁](working-with-sql.md)
[下一頁](search.md)  
