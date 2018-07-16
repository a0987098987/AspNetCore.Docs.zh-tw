---
title: 更新 ASP.NET Core 應用程式中產生的頁面
author: rick-anderson
description: 了解如何更新 ASP.NET Core 應用程式中產生的頁面。
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/da1
ms.openlocfilehash: 55ff98712da314e28e50a1b1b1e04530d5b3fedd
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/11/2018
ms.locfileid: "38186227"
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a>更新 ASP.NET Core 應用程式中產生的頁面

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

剛開始使用此電影應用程式還不錯，但其呈現效果卻不理想。 我們不想看到時間 (下圖的 12:00:00 AM)，而且 **ReleaseDate** 應該是 **Release Date** (兩個分開的字)。

![在 Chrome 中開啟的電影應用程式顯示電影資料](sql/_static/m55.png)

## <a name="update-the-generated-code"></a>更新產生的程式碼

開啟 *Models/Movie.cs* 檔案，然後新增下列程式碼中顯示的醒目提示行：

::: moniker range="= aspnetcore-2.0"
[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/MovieDate.cs?name=snippet_1&highlight=10-11,15)]
::: moniker-end

以滑鼠右鍵按一下紅色曲線 > [快速動作及重構]。

  ![操作功能表隨即顯示 **> [Quick Actions and Refactorings] (快速控制項目及重構)**。](da1/qa.png)

選取 `using System.ComponentModel.DataAnnotations;`。

  ![使用清單頂端的 System.ComponentModel.DataAnnotations](da1/da.png)

  Visual Studio 即會新增 `using System.ComponentModel.DataAnnotations;`。

[!INCLUDE [model1](~/includes/RP/da2.md)]

> [!div class="step-by-step"]
> [上一步：使用 SQL Server LocalDB](xref:tutorials/razor-pages/sql)
> [新增搜尋](xref:tutorials/razor-pages/search)
