---
title: 將模型新增至 ASP.NET Core 中的 Razor 頁面應用程式
author: rick-anderson
description: 了解如何使用 Entity Framework Core (EF Core)，新增用來管理資料庫中電影的類別。
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/model
ms.openlocfilehash: 0d33901805d6728fb8006f14d41090b874ab28b1
ms.sourcegitcommit: e8d80ff566bfe505b43389d7bc4551edb1c0c872
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/28/2018
ms.locfileid: "52549106"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a>將模型新增至 ASP.NET Core 中的 Razor 頁面應用程式

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a>新增資料模型

在方案總管中，以滑鼠右鍵按一下 **RazorPagesMovie** 專案 > [新增] > [新增資料夾]。 將資料夾命名為 *Models*。

以滑鼠右鍵按一下 *Models* 資料夾。 選取 [新增] > [類別]。 將類別命名為 **Movie**，並以下列程式碼取代 `Movie` 類別的內容：

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]

## <a name="scaffold-the-movie-model"></a>Scaffold 影片模型

在本節中會 scaffold 影片模型。 亦即 Scaffolding 工具會產生影片模型的建立、讀取、更新和刪除 (CRUD) 作業頁面。

建立 *Pages/Movies* 資料夾：

* 在 [方案總管] 中，以滑鼠右鍵按一下 *Pages* 資料夾 > [新增] > [新增資料夾]。
* 將資料夾命名為 *Movies*

在 [方案總管] 中，以滑鼠右鍵按一下 *Pages/Movies* 資料夾 > [新增] > [新增 Scaffold 項目]。

![前述指示中的圖片。](model/_static/sca.png)

在 [新增 Scaffold] 對話方塊中，選取 [使用 Entity Framework 的 Razor Pages (CRUD)] > [新增]。

![前述指示中的圖片。](model/_static/add_scaffold.png)

完成 [Add Razor Pages using Entity Framework (CRUD)] \(新增使用 Entity Framework 的 Razor Pages (CRUD)\) 對話方塊：

* 在 [模型類別] 下拉式清單中選取 [Movie (RazorPagesMovie.Models)] \(影片 (RazorPagesMovie.Models)\)。
* 在 [資料內容類別] 列中選取 [+] (加號)，並接受產生的名稱 **RazorPagesMovie.Models.RazorPagesMovieContext**。
* 選取 [新增]。

![前述指示中的圖片。](model/_static/arp.png)

隨即建立 Scaffold 處理序並更新下列檔案：

### <a name="files-created"></a>建立的檔案

* *Pages/Movies*：建立、刪除、詳細資料、編輯、索引。 下一個教學課程會詳述這些頁面。
* *Data/RazorPagesMovieContext.cs*

### <a name="file-updated"></a>檔案已更新

* *Startup.cs*：這個檔案的變更會於下一節中詳述。
* *appsettings.json*：已新增用來連線到本機資料庫的連接字串。

## <a name="examine-the-context-registered-with-dependency-injection"></a>檢查使用相依性插入所註冊的內容

ASP.NET Core 內建[相依性插入](xref:fundamentals/dependency-injection)。 服務 (例如 EF Core DB 內容) 是在應用程式啟動期間使用相依性插入來註冊。 接著，會透過建構函式參數，針對需要這些服務的元件 (例如 Razor 頁面) 來提供服務。 取得資料庫內容執行個體的建構函式程式碼，此教學課程中稍後會示範。

Scaffolding 工具會自動建立資料庫內容，並向相依性插入容器註冊。

檢查 `Startup.ConfigureServices` 方法。 Scaffolder 已新增醒目標示行：

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]

為給定的資料模型協調 EF Core 功能的主要類別是資料庫內容類別。 資料內容衍生自 [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)。 資料內容會指定資料模型包含哪些實體。 在此專案中，類別命名為 `RazorPagesMovieContext`。

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Data/RazorPagesMovieContext.cs)]

上述程式碼會建立實體集的 [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) 屬性。 在 Entity Framework 詞彙中，實體集通常會對應至資料庫資料表。 實體會對應至資料表中的資料列。

連接字串的名稱，會透過對 [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) 物件呼叫方法來傳遞至內容。 作為本機開發之用，[ASP.NET Core 設定系統](xref:fundamentals/configuration/index)會從 *appsettings.json* 檔案讀取連接字串。

<a name="pmc"></a>
## <a name="perform-initial-migration"></a>執行初始移轉

在本節中，您可以使用套件管理員主控台 (PMC) 進行下列作業：

* 新增初始移轉。
* 以初始移轉更新資料庫。

從 [工具] 功能表中，選取 [NuGet 套件管理員] > [套件管理員主控台]。

  ![PMC 功能表](../first-mvc-app/adding-model/_static/pmc.png)

在 PMC 中，輸入下列命令：

```PMC
Add-Migration Initial
Update-Database
```

或者，您可以從專案資料夾使用下列 .NET Core CLI 命令：

```console
dotnet ef migrations add Initial
dotnet ef database update
```

請略過下列警告訊息，您將在稍後的教學課程中修正該問題：

```console
Microsoft.EntityFrameworkCore.Model.Validation[30000]
      No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.
```

`Add-Migration` 命令會產生程式碼來建立初始資料庫結構描述。 結構描述是以 `RazorPagesMovieContext` (位在 *Data/RazorPagesMovieContext.cs* 檔案中) 中指定的模型為基礎。 `Initial` 引數用來命名移轉。 您可以使用任何名稱，但依照慣例，會選擇描述移轉的名稱。 如需詳細資訊，請參閱[移轉簡介](xref:data/ef-mvc/migrations#introduction-to-migrations)。

`Update-Database` 命令會執行 *Migrations/{時間戳記}_InitialCreate.cs* 檔案中的 `Up` 方法，以建立資料庫。

如果您收到錯誤：

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

您遺失了[移轉步驟](#pmc)。

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a>新增資料模型

在方案總管中，以滑鼠右鍵按一下 **RazorPagesMovie** 專案 > [新增] > [新增資料夾]。 將資料夾命名為 *Models*。

以滑鼠右鍵按一下 *Models* 資料夾。 選取 [新增] > [類別]。 將類別命名為 **Movie** 並新增下列屬性：

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a>新增資料庫連線字串

將連線字串新增到 *appsettings.json* 檔案中。

[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a>登錄資料庫內容

用 [Startup 類別的 ConfigureServices 方法](xref:fundamentals/startup#the-startup-class) (*Startup.cs*) 向[相依性插入](xref:fundamentals/dependency-injection)容器註冊資料庫內容：

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

請建置專案，以確認您沒有任何錯誤。

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a>新增 Scaffold 工具並執行初始移轉

在本節中，您可以使用套件管理員主控台 (PMC) 進行下列作業：

* 新增 Visual Studio Web 程式碼產生套件。 執行 Scaffolding 引擎需要此套件。
* 新增初始移轉。
* 以初始移轉更新資料庫。

從 [工具] 功能表中，選取 [NuGet 套件管理員] > [套件管理員主控台]。

  ![PMC 功能表](../first-mvc-app/adding-model/_static/pmc.png)

在 PMC 中，輸入下列命令：

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.3
Add-Migration Initial
Update-Database
```

或者，您可以使用下列 .NET Core CLI 命令：

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet ef migrations add Initial
dotnet ef database update
```

請略過下列訊息：

```console
Microsoft.EntityFrameworkCore.Model.Validation[30000]
      No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'
```

您將在接下來的教學課程中修正該問題。

`Install-Package` 命令會安裝執行 Scaffolding 引擎所需的工具。

`Add-Migration` 命令會產生程式碼來建立初始資料庫結構描述。 結構描述是以 `DbContext` (位在 *Models/MovieContext.cs* 檔案中) 中指定的模型為基礎。 `Initial` 引數用來命名移轉。 您可以使用任何名稱，但依照慣例，會選擇描述移轉的名稱。 如需詳細資訊，請參閱[移轉簡介](xref:data/ef-mvc/migrations#introduction-to-migrations)。

`Update-Database` 命令會執行 *Migrations/{時間戳記}_InitialCreate.cs* 檔案中的 `Up` 方法，以建立資料庫。

[!INCLUDE [model 4windows](~/includes/RP/model4Win.md)]

[!INCLUDE [model 4](~/includes/RP/model4tbl.md)]

::: moniker-end

<a name="test"></a>

### <a name="test-the-app"></a>測試應用程式

* 執行應用程式，並將 `/Movies` 附加至瀏覽器中的 URL (`http://localhost:port/movies`)。
* 測試 **Create** 連結。

  > [!NOTE]
  > 您可能無法在 `Price` 欄位中輸入小數逗號。 若要對使用逗號 (",") 作為小數點的非英文地區設定和非英文日期欄位支援 [jQuery 驗證](https://jqueryvalidation.org/)，您必須將您的應用程式全球化。 如需全球化指示，請參閱[此 GitHub 問題](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) \(英文\)。

  ![建立頁面](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* 測試 **Edit**、**Details** 和 **Delete** 連結。

如果您收到 SQL 例外狀況，請確認您已執行移轉並更新資料庫。

下一個教學課程說明 Scaffolding 所建立的檔案。

> [!div class="step-by-step"]
> [上一步：開始使用](xref:tutorials/razor-pages/razor-pages-start)
> [下一步：Scaffold Razor 頁面](xref:tutorials/razor-pages/page)
