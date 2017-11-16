---
title: "將模型加新增至 ASP.NET Core MVC 應用程式。"
author: rick-anderson
description: "請將模型新增至簡單的 ASP.NET Core 應用程式。"
ms.author: riande
ms.date: 09/18/2017
ms.topic: get-started-article
ms.technology: aspnet
keywords: "ASP.NET Core, WebAPI, Web API, REST, Mac, Linux, HTTP, 服務, HTTP 服務, VS Code"
ms.prod: asp.net-core
manager: wpickett
ms.assetid: 8dc28498-eeee-4666-b903-b593059e9f39
uid: tutorials/first-mvc-app-xplat/adding-model
ms.openlocfilehash: 70aa344ca4ceafacf53907c925fd595e47104d7e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
[!INCLUDE[adding-model1](../../includes/mvc-intro/adding-model1.md)]

* 將類別新增至名為 *Movie.cs* 的 *Models* 資料夾。
* 將下列程式碼新增至 *Models/Movie.cs* 檔案：

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

`ID` 欄位是資料庫對於主索引鍵的必要欄位。 

建置應用程式，以確認您沒有任何錯誤，而且您最後已將**模型**新增至 **MVC** 應用程式。

## <a name="prepare-the-project-for-scaffolding"></a>準備專案進行 Scaffolding

- 將下列反白顯示的 NuGet 套件新增至 *MvcMovie.csproj* 檔案：
             
   [!code-csharp[Main](start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- 選取檔案，然後選取 [還原] 至 [資訊] 訊息「有未解析的相依性」。
- 建立 *Models/MvcMovieContext.cs* 檔案，然後新增下列 `MvcMovieContext` 類別：

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]
   
- 開啟 *Startup.cs* 檔案，然後新增兩個 using：

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]

- 將資料庫內容新增至 *Startup.cs* 檔案：

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  這會告知 Entity Framework 哪些模型類別包含在資料模型中。 您是在定義電影物件的一個「實體集」，它會在資料庫中以「電影」資料表表示。

- 建置專案以確認沒有任何錯誤。

## <a name="scaffold-the-moviecontroller"></a>Scaffold MovieController

在專案資料夾中開啟終端機視窗，然後執行下列命令：

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```

> [!NOTE]
> 如果您在 Scaffolding 命令執行時收到錯誤，請參閱 [Scaffolding 儲存機制中的問題 444](https://github.com/aspnet/scaffolding/issues/444) 以了解因應措施。

Scaffolding 引擎會建立下列各項：

* 電影控制器 (*Controllers/MoviesController.cs*)
* Create、Delete、Details、Edit 和 Index 頁面的 Razor 檢視檔案 (*Views/Movies/\*.cshtml*)

自動建立 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (建立、讀取、更新和刪除) 動作方法和檢視稱為 *Scaffolding*。 您很快就會擁有一個正常運作的 Web 應用程式，可讓您管理電影資料庫。

[!INCLUDE[adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

您現在擁有一個資料庫和多個頁面，可用來顯示、編輯、更新和刪除資料。 在下一個教學課程中，我們將會使用資料庫。

### <a name="additional-resources"></a>其他資源

* [標記協助程式](xref:mvc/views/tag-helpers/intro)
* [全球化和當地語系化](xref:fundamentals/localization)

>[!div class="step-by-step"]
[上一步 - 新增檢視](adding-view.md)
[下一步 - 使用 SQLite](working-with-sql.md)
