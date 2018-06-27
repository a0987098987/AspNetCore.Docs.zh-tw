---
title: 更新 ASP.NET Core 應用程式中產生的頁面
author: rick-anderson
description: 了解如何更新 ASP.NET Core 應用程式中產生的頁面。
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/da1
ms.openlocfilehash: 55ff98712da314e28e50a1b1b1e04530d5b3fedd
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278066"
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a><span data-ttu-id="7d060-103">更新 ASP.NET Core 應用程式中產生的頁面</span><span class="sxs-lookup"><span data-stu-id="7d060-103">Update the generated pages in an ASP.NET Core app</span></span>

<span data-ttu-id="7d060-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7d060-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7d060-105">剛開始使用此電影應用程式還不錯，但其呈現效果卻不理想。</span><span class="sxs-lookup"><span data-stu-id="7d060-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="7d060-106">我們不想看到時間 (下圖的 12:00:00 AM)，而且 **ReleaseDate** 應該是 **Release Date** (兩個分開的字)。</span><span class="sxs-lookup"><span data-stu-id="7d060-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![在 Chrome 中開啟的電影應用程式顯示電影資料](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="7d060-108">更新產生的程式碼</span><span class="sxs-lookup"><span data-stu-id="7d060-108">Update the generated code</span></span>

<span data-ttu-id="7d060-109">開啟 *Models/Movie.cs* 檔案，然後新增下列程式碼中顯示的醒目提示行：</span><span class="sxs-lookup"><span data-stu-id="7d060-109">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="7d060-110">[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]</span><span class="sxs-lookup"><span data-stu-id="7d060-110">[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="7d060-111">[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/MovieDate.cs?name=snippet_1&highlight=10-11,15)]</span><span class="sxs-lookup"><span data-stu-id="7d060-111">[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/MovieDate.cs?name=snippet_1&highlight=10-11,15)]</span></span>
::: moniker-end

<span data-ttu-id="7d060-112">以滑鼠右鍵按一下紅色曲線 > [快速動作及重構]。</span><span class="sxs-lookup"><span data-stu-id="7d060-112">Right click on a red squiggly line > **Quick Actions and Refactorings**.</span></span>

  ![操作功能表隨即顯示 **> [Quick Actions and Refactorings] (快速控制項目及重構)**。](da1/qa.png)

<span data-ttu-id="7d060-114">選取 `using System.ComponentModel.DataAnnotations;`。</span><span class="sxs-lookup"><span data-stu-id="7d060-114">Select `using System.ComponentModel.DataAnnotations;`</span></span>

  ![使用清單頂端的 System.ComponentModel.DataAnnotations](da1/da.png)

  <span data-ttu-id="7d060-116">Visual Studio 即會新增 `using System.ComponentModel.DataAnnotations;`。</span><span class="sxs-lookup"><span data-stu-id="7d060-116">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

[!INCLUDE [model1](~/includes/RP/da2.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="7d060-117">[上一步：使用 SQL Server LocalDB](xref:tutorials/razor-pages/sql)
> [新增搜尋](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="7d060-117">[Previous: Working with SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Add search](xref:tutorials/razor-pages/search)</span></span>
