---
title: 新增模型到 ASP.NET Core MVC 應用程式
author: rick-anderson
description: 請將模型新增至簡單的 ASP.NET Core 應用程式。
ms.author: riande
ms.date: 12/8/2017
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 28a63498bc1a3c7b6ad6be038209dacdb49e44ee
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2018
ms.locfileid: "36960664"
---
[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model1.md)]

以滑鼠右鍵按一下 *Models* 資料夾 > [新增] > [類別]。 將類別命名為 **Movie** 並新增下列屬性：

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

`ID` 欄位是資料庫對於主索引鍵的必要欄位。 

請建置專案，以確認您沒有任何錯誤。 您目前即會在 **MVC** 應用程式中具有一個**模型**。

## <a name="scaffolding-a-controller"></a>Scaffold 控制器

::: moniker range=">= aspnetcore-2.1"

在 [方案總管] 中以滑鼠右鍵按一下 *Controllers* 資料夾 > [新增] > [新增 Scaffold 項目]。

![上方步驟的檢視](adding-model/_static/add_controller21.png)

在 [新增 Scaffold] 對話方塊中，點選 [使用 Entity Framework 執行檢視的 MVC 控制器] > [新增]。

![[新增 Scaffold] 對話方塊](adding-model/_static/add_scaffold21.png)

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

在**方案總管**中，以滑鼠右鍵按一下 *Controllers* 資料夾 > [新增] > [控制器]。

![上方步驟的檢視](adding-model/_static/add_controller.png)

如果出現 [新增 MVC 相依性] 對話方塊：

* [將 Visual Studio 更新為最新版本](https://www.visualstudio.com/downloads/)。 15.5 之前的 Visual Studio 版本會顯示此對話方塊。
* 如果您無法更新，請選取 [新增]，然後再次遵循新增控制器步驟進行。

在 [新增 Scaffold] 對話方塊中，點選 [使用 Entity Framework 執行檢視的 MVC 控制器] > [新增]。

![[新增 Scaffold] 對話方塊](adding-model/_static/add_scaffold2.png)

::: moniker-end

完成 [新增控制器] 對話方塊：

* **模型類別：***Movie (MvcMovie.Models)*
* **資料內容類別：** 選取 **+** 圖示並新增預設的 **MvcMovie.Models.MvcMovieContext**

![新增資料內容](adding-model/_static/dc.png)

* **檢視：** 保持核取預設的每一個選項
* **控制器名稱：** 保留預設值 *MoviesController*
* 點選 [新增]

![[新增控制器] 對話方塊](adding-model/_static/add_controller2.png)

Visual Studio 會建立：

* Entity Framework Core [資料庫內容類別](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)
* 電影控制器 (*Controllers/MoviesController.cs*)
* Create、Delete、Details、Edit 和 Index 頁面的 Razor 檢視檔案 (<em>Views/Movies/&ast;.cshtml</em>)

自動建立資料庫內容與 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (建立、讀取、更新和刪除) 動作方法和檢視稱為 *Scaffolding*。 您很快就會擁有一個正常運作的 Web 應用程式，可讓您管理電影資料庫。

如果執行應用程式，然後按一下 **Mvc Movie** 連結，則會收到如下錯誤：

``` error
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString 
```

您需要建立資料庫，而且您將使用 EF Core [移轉](xref:data/ef-mvc/migrations)功能來執行此作業。 移轉可讓您建立符合資料模型的資料庫，並在資料模型變更時，更新資料庫結構描述。

## <a name="add-ef-tooling-and-perform-initial-migration"></a>新增 EF 工具並執行初始移轉

在本節中，您將使用套件管理員主控台 (PMC) 進行下列作業：

* 新增 Entity Framework Core Tools 套件。 必須有此套件，才能新增移轉並更新資料庫。
* 新增初始移轉。
* 以初始移轉更新資料庫。

從 [工具] 功能表中，選取 [NuGet 套件管理員] > [套件管理員主控台]。

<!-- following image shared with uid: tutorials/razor-pages/model --> ![PMC 功能表](adding-model/_static/pmc.png)

在 PMC 中，輸入下列命令：

::: moniker range=">= aspnetcore-2.1"
``` PMC
Add-Migration Initial
Update-Database
```

請忽略下列錯誤訊息，我們將在接下來的教學課程中修正該問題：

*Microsoft.EntityFrameworkCore.Model.Validation[30000]*  
      *實體類型「影片」上的十進位資料行 'Price' 未指定任何類型。如果它們不符合預設的有效位數和小數位數，會導致以無訊息模式截斷這些值。使用 'ForHasColumnType()' 明確指定可容納所有值的 SQL 伺服器資料行類型。*

::: moniker-end
::: moniker range="<= aspnetcore-2.0"

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

**注意：** 如果使用 `Install-Package` 命令但收到錯誤，請開啟 NuGet 套件管理員，並搜尋 `Microsoft.EntityFrameworkCore.Tools` 套件。 您可利用此安裝該套件，或檢查其是否已安裝。 此外，若您有 PMC 的問題，可以參閱 [CLI 方法](#cli)。

::: moniker-end

`Add-Migration` 命令會建立程式碼來建立初始資料庫結構描述。 結構描述是以 `DbContext` (位在 *Data/MvcMovieContext.cs* 檔案中) 中指定的模型為基礎。 `Initial` 引數用來命名移轉。 您可以使用任何名稱，但依照慣例，會選擇描述移轉的名稱。 如需詳細資訊，請參閱[移轉簡介](xref:data/ef-mvc/migrations#introduction-to-migrations)。

`Update-Database` 命令會執行 Migrations/\<時間戳記>_Initial.cs 檔案中的 `Up` 方法，以建立資料庫。

<a name="cli"></a> 您可以使用命令列介面 (CLI) 而不是 PMC，來執行先前步驟：

* 將 [EF Core 工具](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations)新增至 *.csproj* 檔案。
* 從主控台 (在專案目錄中) 中執行下列命令：

  ```console
  dotnet ef migrations add Initial
  dotnet ef database update
  ```

  如果您執行應用程式並收到錯誤：

  ```text
  SqlException: Cannot open database "Movie" requested by the login.
  The login failed.
  Login failed for user 'user name'.
  ```

您可能尚未執行 `dotnet ef database update`。

[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model3.md)]

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Startup.cs?name=ConfigureServices&highlight=13-99)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]
::: moniker-end

[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model4.md)]

![列出識別碼、價格、發行日期及標題等可用屬性的模型項目上 IntelliSense 的操作功能表](adding-model/_static/ints.png)

## <a name="additional-resources"></a>其他資源

* [標記協助程式](xref:mvc/views/tag-helpers/intro)
* [全球化和當地語系化](xref:fundamentals/localization)

> [!div class="step-by-step"]
> [上一步：新增檢視](adding-view.md)
> [下一步：使用 SQL](working-with-sql.md)  
