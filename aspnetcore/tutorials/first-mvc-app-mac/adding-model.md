---
title: "將模型新增至 ASP.NET MVC Core 應用程式"
author: rick-anderson
description: "請將模型新增至簡單的 ASP.NET Core 應用程式。"
keywords: ASP.NET Core, MVC, Scaffold, Scaffolding
ms.author: riande
manager: wpickett
ms.date: 03/30/2017
ms.topic: get-started-article
ms.assetid: 8dc28498-eeee-1638-b903-b593059e9f39
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-mac/adding-model
ms.openlocfilehash: 4a158802a19011cbb45da1b3ca43d67706efe4cd
ms.sourcegitcommit: d9e2c99c837078fcac0e315025f8fbfbd45ea6e8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/28/2017
---
[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model1.md)]

* 以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增] > [新增檔案]。 
* 在 [新增檔案] 對話方塊中：

  * 在左窗格中選取 [一般]。
  * 在中央窗格中選取 [空類別]。
  * 將類別命名為 **Movie**，然後選取 [新增]。

將下列屬性新增至 `Movie` 類別：

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

`ID` 欄位是資料庫對於主索引鍵的必要欄位。

請建置專案，以確認您沒有任何錯誤。 您目前即會在 **MVC** 應用程式中具有一個**模型**。

## <a name="prepare-the-project-for-scaffolding"></a>準備專案進行 Scaffolding

- 以滑鼠右鍵按一下專案檔，然後選取 [工具] > [編輯檔案]。

  ![上方步驟的檢視](adding-model/_static/1.png)

- 將下列反白顯示的 NuGet 套件新增至 *MvcMovie.csproj* 檔案：
             
  [!code-csharp[Main](../first-mvc-app-xplat/start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- 儲存檔案。

- 建立 *Models/MvcMovieContext.cs* 檔案，然後新增下列 `MvcMovieContext` 類別：[!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]
   
- 開啟 *Startup.cs* 檔案，然後新增兩個 using：[!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]

- 將資料庫內容新增至 *Startup.cs* 檔案：

   [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  這會告知 Entity Framework 哪些模型類別包含在資料模型中。 您是在定義電影物件的一個「實體集」，它會在資料庫中以「電影」資料表表示。

- 建置專案以確認沒有任何錯誤。

## <a name="scaffold-the-moviecontroller"></a>Scaffold MovieController

在專案資料夾中開啟終端機視窗，然後執行下列命令：

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```
如果您收到錯誤 `No executable found matching command "dotnet-aspnet-codegenerator", verify`：

 * 您位在專案目錄中。 專案目錄中含有 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案。
 * Dotnet 版本是 1.1 版 (含) 以上版本。 執行 `dotnet` 以取得版本。
 * 您已將 `<DotNetCliToolReference>` 項目新增至 [MvcMovie.csproj 檔案](#prepare-the-project-for-scaffolding)。
 
<!--
> [!NOTE]
> If you get an error when the scaffolding command runs, see [issue 444 in the scaffolding repository](https://github.com/aspnet/scaffolding/issues/444) for a workaround.
-->

Scaffolding 引擎會建立下列各項：

* 電影控制器 (*Controllers/MoviesController.cs*)
* Create、Delete、Details、Edit 和 Index 頁面的 Razor 檢視檔案 (*Views/Movies/\*.cshtml*)

自動建立 [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (建立、讀取、更新和刪除) 動作方法和檢視稱為 *Scaffolding*。 您很快就會擁有一個正常運作的 Web 應用程式，可讓您管理電影資料庫。

### <a name="add-the-files-to-visual-studio"></a>將檔案新增至 Visual Studio

* 將 *MovieController.cs* 檔案新增至 Visual Studio 專案中：

  * 以滑鼠右鍵按一下 *Controllers* 資料夾，然後選取 [新增] > [新增檔案]。
  * 選取 *MovieController.cs* 檔案。

* 新增 *Movies* 資料夾和檢視：

  * 以滑鼠右鍵按一下 *Views* 資料夾，然後選取 [新增] > [新增現有資料夾]。
  * 巡覽至 *Views* 資料夾中，選取 *Views\Movies*，然後選取 [開啟]。
  * 在 [Select files to add from Movies] (選取要從 Movies 新增的檔案) 對話方塊中，選取 [全部包含]，然後選取 [確定]。

[!INCLUDE[adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

您現在擁有一個資料庫和多個頁面，可用來顯示、編輯、更新和刪除資料。 在下一個教學課程中，我們將會使用資料庫。

## <a name="additional-resources"></a>其他資源

* [標記協助程式](xref:mvc/views/tag-helpers/intro)
* [全球化和當地語系化](xref:fundamentals/localization)

>[!div class="step-by-step"]
[上一步：新增檢視](adding-view.md)
[下一步：使用 SQL](working-with-sql.md)  
