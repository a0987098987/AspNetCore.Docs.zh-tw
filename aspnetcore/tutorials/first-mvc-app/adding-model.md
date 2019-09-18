---
title: 新增模型到 ASP.NET Core MVC 應用程式
author: rick-anderson
description: 請將模型新增至簡單的 ASP.NET Core 應用程式。
ms.author: riande
ms.date: 8/15/2019
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: b0efaf76cb2172f5b7568e42065b99b1259949de
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2019
ms.locfileid: "71081988"
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a>新增模型到 ASP.NET Core MVC 應用程式

作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Ryan Nowak](https://github.com/tdykstra)

在本節中，您可以新增類別來管理資料庫中的電影。 這些類別是 **MVC** 應用程式的「模型」部分。

搭配 [Entity Framework Core](/ef/core) (EF Core) 使用這些類別，即可使用資料庫。 EF Core 是一種物件關聯式對應 (ORM) 架構，可簡化您必須撰寫的資料存取程式碼。

您所建立的模型類別稱為 POCO 類別 (來自「純舊 CLR 物件」)，因為它們對 EF Core 沒有任何相依性。 它們只會定義資料庫將儲存之資料的屬性。

在本教學課程中，您首先要撰寫模型類別，而 EF Core 會建立資料庫。 本文未提及的替代方法是從現有的資料庫產生模型類別。 如需該方法的資訊，請參閱 [ASP.NET Core - 現有的資料庫](/ef/core/get-started/aspnetcore/existing-db)。

::: moniker range=">= aspnetcore-3.0"

## <a name="add-a-data-model-class"></a>新增資料模型類別

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

以滑鼠右鍵按一下 *Models* 資料夾 > [新增] > [類別]。 將檔案命名為 *Movie.cs*。

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

將名為 *Movie.cs* 的檔案新增到 *Models* 資料夾。

---

使用下列程式碼更新 *Movie.cs* 檔案：

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Models/Movie.cs)]

`Movie` 類別包含 `Id` 欄位，該欄位是資料庫的必要欄位，將作為主索引鍵。

`ReleaseDate` 上的 [DateType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) 屬性會指定資料的型別 (`Date`)。 使用此屬性：

  * 使用者不需要在日期欄位中輸入時間資訊。
  * 只會顯示日期，不會顯示時間資訊。

稍後的教學課程會涵蓋 [DataAnnotations](/dotnet/api/system.componentmodel.dataannotations)。

## <a name="add-nuget-packages"></a>新增 NuGet 套件

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

從 [工具] 功能表中，選取 [NuGet 套件管理員] > [套件管理器主控台] (PMC)。

![PMC 功能表](~/tutorials/first-mvc-app/adding-model/_static/pmc.png)

在 PMC 中，執行下列命令：

```powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer -IncludePrerelease
```

上述命令會新增 EF Core SQL Server 提供者。 提供者套件會將 EF Core 套件作為相依性安裝。 其他套件會在本教學課程中稍後的 scaffolding 步驟內自動安裝。

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

執行下列 .NET Core CLI 命令：

```dotnetcli
dotnet tool install --global dotnet-ef --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.SQLite --version 3.0.0-*
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version 3.0.0-*
```

上述命令會新增：

* 適用於 .NET CLI 的 Entity Framework Core 工具。
* EF Core SQLite 提供者，該提供者會將 EF Core 套件作為相依性安裝。
* Scaffolding `Microsoft.VisualStudio.Web.CodeGeneration.Design` 和 `Microsoft.EntityFrameworkCore.SqlServer` 需要的套件。

---

<a name="dc"></a>

## <a name="create-a-database-context-class"></a>建立資料庫內容類別

資料庫內容類別是協調 `Movie` 模型 EF Core 功能 (建立、讀取、更新、刪除) 的必要項目。 資料庫內容衍生自 [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)，並指定要包含在資料模型中的實體。

建立 *Data* 資料夾。

使用下列程式碼新增 *Data/MvcMovieContext.cs* 內容： 

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/zDocOnly/MvcMovieContext.cs?name=snippet)]

上述程式碼會建立實體集的 [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) 屬性。 在 Entity Framework 詞彙中，實體集通常會對應至資料庫資料表。 實體會對應至資料表中的資料列。

<a name="reg"></a>

## <a name="register-the-database-context"></a>登錄資料庫內容

ASP.NET Core 內建[相依性插入 (DI)](xref:fundamentals/dependency-injection)。 服務 (例如 EF Core 資料庫內容) 必須在應用程式啟動期間向 DI 進行註冊。 接著，會透過建構函式參數，針對需要這些服務的元件 (例如 Razor 頁面) 來提供服務。 取得資料庫內容執行個體的建構函式程式碼，本教學課程中稍後會示範。 在本節中，您會向 DI 容器註冊資料庫內容。

在 *Startup.cs* 最上方新增下列 `using` 陳述式：

```csharp
using MvcMovie.Data;
using Microsoft.EntityFrameworkCore;
```

在 `Startup.ConfigureServices` 中新增下列醒目提示的程式碼：

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_ConfigureServices&highlight=6-7)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_UseSqlite&highlight=6-7)]

---

連接字串的名稱，會透過對 [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) 物件呼叫方法來傳遞至內容。 作為本機開發之用，[ASP.NET Core 設定系統](xref:fundamentals/configuration/index)會從 *appsettings.json* 檔案讀取連接字串。

<a name="cs"></a>

## <a name="add-a-database-connection-string"></a>新增資料庫連線字串

將連接字串新增到 *appsettings.json* 檔案：

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/appsettings.json?highlight=10-12)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/appsettings_SQLite.json?highlight=10-12)]

---

建置專案以檢查編譯器錯誤。

## <a name="scaffold-movie-pages"></a>Scaffold 影片頁面

使用 scaffolding 工具來為影片模型產生建立、讀取、更新和刪除 (CRUD) 頁面。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

在 [方案總管] 中以滑鼠右鍵按一下 *Controllers* 資料夾 > [新增] > [新增 Scaffold 項目]。

![上方步驟的檢視](adding-model/_static/add_controller21.png)

在 [新增 Scaffold] 對話方塊中，選取 [使用 Entity Framework 執行檢視的 MVC 控制器] > [新增]。

![[新增 Scaffold] 對話方塊](adding-model/_static/add_scaffold21.png)

完成 [新增控制器] 對話方塊：

* **模型類別：** Movie (MvcMovie.Models)
* **資料內容類別：** *MvcMovieContext (MvcMovie.Data)*

![新增資料內容](adding-model/_static/dc3.png)

* **檢視：** 保持核取預設的每一個選項
* **控制器名稱：** 保留預設 *MoviesController*
* 選取 [新增]

Visual Studio 會建立：

* 電影控制器 (*Controllers/MoviesController.cs*)
* Create、Delete、Details、Edit 和 Index 頁面的 Razor 檢視檔案 (*Views/Movies/\*.cshtml*)

自動建立這些檔案的流程稱為 *scaffolding*。

### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code) 

* 在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。

* 在 Linux 上，匯出 scaffold 工具路徑：

  ```console
    export PATH=$HOME/.dotnet/tools:$PATH
  ```

* 執行下列命令：

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  [!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* 在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。

* 執行下列命令：

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  [!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

---

<!-- End of tabs                  -->

您目前尚無法使用 scaffold 頁面，因為資料庫不存在。 若您執行應用程式並按一下 [影片應用程式] 連結，您會收到「無法開啟資料庫」或「找不到資料表：Movie」錯誤訊息。

<a name="migration"></a>

## <a name="initial-migration"></a>初始移轉

使用 EF Core 的 [移轉](xref:data/ef-mvc/migrations) 功能來建立資料庫。 移轉是一組工具，可讓您建立和更新資料庫，使其與您的資料模型相符。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

從 [工具] 功能表中，選取 [NuGet 套件管理員] > [套件管理器主控台] (PMC)。

在 PMC 中，輸入下列命令：

```console
Add-Migration InitialCreate
Update-Database
```

* `Add-Migration InitialCreate`：產生 *Migrations/{timestamp}_InitialCreate.cs* 移轉檔案。 `InitialCreate` 引數是移轉名稱。 您可以使用任何名稱，但依照慣例，會選取描述移轉的名稱。 因為這是第一次移轉，所產生類別會包含建立資料庫結構描述的程式碼。 資料庫結構描述是以 `MvcMovieContext` 類別為基礎。

* `Update-Database`：將資料庫更新到最新的移轉，即上一個命令建立的移轉。 此命令會執行 *Migrations/{time-stamp}_InitialCreate.cs* 檔案中的 `Up` 方法，其會建立資料庫。

  資料庫更新命令會產生下列警告： 

  > 沒有為實體型別 'Movie' 上的十進位資料行 'Price' 指定型別。 如果它們不符合預設的有效位數和小數位數，會導致以無訊息模式截斷這些值。 使用 'HasColumnType()' 明確指定可容納所有值的 SQL 伺服器資料行型別。

  您可以忽略該警告，稍後的教學課程中將修正此問題。

[!INCLUDE [more information on the PMC tools for EF Core](~/includes/ef-pmc.md)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

執行下列 .NET Core CLI 命令：

```dotnetcli
dotnet ef migrations add InitialCreate
dotnet ef database update
```

* `ef migrations add InitialCreate`：產生 *Migrations/{timestamp}_InitialCreate.cs* 移轉檔案。 `InitialCreate` 引數是移轉名稱。 您可以使用任何名稱，但依照慣例，會選取描述移轉的名稱。 因為這是第一次移轉，所產生類別會包含建立資料庫結構描述的程式碼。 資料庫結構描述會以 `MvcMovieContext` 類別 (在 *Data/MvcMovieContext.cs* 檔案中) 中指定的模型為基礎。

* `ef database update`：將資料庫更新到最新的移轉，即上一個命令建立的移轉。 此命令會執行 *Migrations/{time-stamp}_InitialCreate.cs* 檔案中的 `Up` 方法，其會建立資料庫。

[!INCLUDE [ more information on the CLI tools for EF Core](~/includes/ef-cli.md)]

---

### <a name="the-initialcreate-class"></a>InitialCreate 類別

檢查 *Migrations/{timestamp}_InitialCreate.cs* 移轉檔案：

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Migrations/20190805165915_InitialCreate.cs?name=snippet)]

 `Up` 方法會建立 Movie 資料表，並將 `Id` 設為主索引鍵。 `Down` 方法會還原 `Up` 移轉所做的結構描述變更。

<a name="test"></a>

## <a name="test-the-app"></a>測試應用程式

* 執行應用程式，然後按一下 [影片應用程式] 連結。

  若您收到與下列內容相似的例外狀況：

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

  ```console
  SqlException: Cannot open database "MvcMovieContext-1" requested by the login. The login failed.
  ```

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

  ```console
  SqliteException: SQLite Error 1: 'no such table: Movie'.
  ```

---
  您可能遺漏了[移轉步驟](#migration)。

* 測試 **Create** 頁面。 輸入並提交資料。

  > [!NOTE]
  > 您可能無法在 `Price` 欄位中輸入小數逗號。 若要對使用逗號 (",") 作為小數點的非英文地區設定和非英文日期格式支援 [jQuery 驗證](https://jqueryvalidation.org/)，則必須將應用程式全球化。 如需全球化指示，請參閱[此 GitHub 問題](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) \(英文\)。

* 測試 **Edit**、**Details** 和 **Delete** 頁面。

## <a name="dependency-injection-in-the-controller"></a>控制器中的相依性插入

開啟 *Controllers/MoviesController.cs* 檔案，並檢查建構函式：

<!-- l.. Make copy of Movies controller (or use the old one as I did in the 3.0 upgrade) because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

建構函式會使用[相依性插入](xref:fundamentals/dependency-injection)將資料庫內容 (`MvcMovieContext`) 插入到控制器中。 控制器中的每一個 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) 方法都會使用資料庫內容。

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a>強型別模型和 @model 關鍵字

稍早在本教學課程中，您已看到控制器如何使用 `ViewData` 字典傳遞資料或物件給檢視。 `ViewData` 字典是一種動態物件，提供便利的晚期繫結方式，將資訊傳遞至檢視。

MVC 也提供將強型別模型物件傳遞至檢視的能力。 這種強型別的方法可在編譯時檢查程式碼。 此方法所使用的 scaffolding 機制 (即傳遞強型別模型)，包括 `MoviesController` 類別和檢視。

檢查 *Controllers/MoviesController.cs* 檔案中產生的 `Details` 方法：

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_details)]

`id` 參數通常會傳遞為路由資料。 例如，`https://localhost:5001/movies/details/1` 設定：

* 控制器為 `movies` 控制器 (第一個 URL 區段)。
* 動作為 `details` (第二個 URL 區段)。
* 識別碼為 1 (最後一個 URL 區段)。

您也可以在 `id` 中使用查詢字串傳遞，如下所示：

`https://localhost:5001/movies/details?id=1`

若未提供識別碼值，`id` 參數會定義為[可為 Null 的型別](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`)。

[Lambda 運算式](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions)會傳遞至 `FirstOrDefaultAsync`，以選取符合路由資料或查詢字串值的電影實體。

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.Id == id);
```

如果找到電影，則 `Movie` 模型的執行個體會傳遞至 `Details` 檢視：

```csharp
return View(movie);
   ```

檢查 *Views/Movies/Details.cshtml* 檔案的內容：

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/DetailsOriginal.cshtml)]

檢視檔案頂端的 `@model` 陳述式會指定檢視所預期物件型別。 建立影片控制器時，會包含下列 `@model` 陳述式：

```HTML
@model MvcMovie.Models.Movie
   ```

此 `@model` 指示詞會允許存取控制器傳遞給檢視的影片。 `Model` 物件為強型別物件。 例如，在 *Details.cshtml* 檢視中，程式碼會使用強型別的 `Model` 物件，將每個電影欄位傳遞至 `DisplayNameFor` 和 `DisplayFor` HTML 協助程式。 `Create` 與 `Edit` 方法和檢視也會傳遞 `Movie` 模型物件。

檢查電影控制器中的 *Index.cshtml* 檢視和 `Index` 方法。 請注意程式碼如何在呼叫 `View` 方法時建立 `List` 物件。 此程式碼會從 `Index` 動作方法將 `Movies` 清單傳遞至檢視：

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_index)]

建立影片控制器時，scaffolding 會在 *Index.cshtml* 檔案的頂端包含下列 `@model` 陳述式：

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?range=1)]

`@model` 指示詞可讓您使用強型別的 `Model` 物件，存取控制器傳遞至檢視的電影清單。 例如，在 *Index.cshtml* 檢視中，程式碼會透過強型別 `Model` 物件的 `foreach` 陳述式循環存取電影：

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

因為 `Model` 物件是強型別 (作為 `IEnumerable<Movie>` 物件)，所以迴圈中每個項目的類型為 `Movie`。 撇開其他優點，這表示您會進行程式碼編譯時期檢查。

## <a name="additional-resources"></a>其他資源

* [標記協助程式](xref:mvc/views/tag-helpers/intro)
* [全球化和當地語系化](xref:fundamentals/localization)

> [!div class="step-by-step"]
> [上一步：新增檢視](adding-view.md)
> [下一步：使用 SQL](working-with-sql.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

## <a name="add-a-data-model-class"></a>新增資料模型類別

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

以滑鼠右鍵按一下 *Models* 資料夾 > [新增] > [類別]。 將類別命名為 **Movie**。

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

* 將類別新增至名為 *Movie.cs* 的 *Models* 資料夾。

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]
[!INCLUDE [model 2](~/includes/mvc-intro/model2.md)]

---

## <a name="scaffold-the-movie-model"></a>Scaffold 影片模型

在本節中會 scaffold 影片模型。 亦即 Scaffolding 工具會產生影片模型的建立、讀取、更新和刪除 (CRUD) 作業頁面。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

在 [方案總管] 中以滑鼠右鍵按一下 *Controllers* 資料夾 > [新增] > [新增 Scaffold 項目]。

![上方步驟的檢視](adding-model/_static/add_controller21.png)

在 [新增 Scaffold] 對話方塊中，選取 [使用 Entity Framework 執行檢視的 MVC 控制器] > [新增]。

![[新增 Scaffold] 對話方塊](adding-model/_static/add_scaffold21.png)

完成 [新增控制器] 對話方塊：

* **模型類別：** Movie (MvcMovie.Models)
* **資料內容類別：** 選取 **+** 圖示並新增預設的 **MvcMovie.Models.MvcMovieContext**

![新增資料內容](adding-model/_static/dc.png)

* **檢視：** 保持核取預設的每一個選項
* **控制器名稱：** 保留預設 *MoviesController*
* 選取 [新增]

![[新增控制器] 對話方塊](adding-model/_static/add_controller2.png)

Visual Studio 會建立：

* Entity Framework Core [資料庫內容類別](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)
* 電影控制器 (*Controllers/MoviesController.cs*)
* Create、Delete、Details、Edit 和 Index 頁面的 Razor 檢視檔案 (*Views/Movies/\*.cshtml*)

自動建立資料庫內容與 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (建立、讀取、更新和刪除) 動作方法和檢視稱為 *Scaffolding*。

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* 在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。
* 安裝 Scaffolding 工具：

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* 在 Linux 上，匯出 scaffold 工具路徑：

  ```console
    export PATH=$HOME/.dotnet/tools:$PATH
  ```

* 執行下列命令：

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* 在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。
* 安裝 Scaffolding 工具：

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* 執行下列命令：

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

---

<!-- End of VS tabs                  -->

如果執行應用程式，然後按一下 **Mvc Movie** 連結，則會收到如下錯誤：

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

``` error
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString
```

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

``` error
An unhandled exception occurred while processing the request.
SqliteException: SQLite Error 1: 'no such table: Movie'.
Microsoft.Data.Sqlite.SqliteException.ThrowExceptionForRC(int rc, sqlite3 db)

```

---

您需要建立資料庫，而且您會使用 EF Core [移轉](xref:data/ef-mvc/migrations)功能來執行此作業。 移轉可讓您建立符合資料模型的資料庫，並在資料模型變更時，更新資料庫結構描述。

<a name="pmc"></a>

## <a name="initial-migration"></a>初始移轉

在本節中，將會完成下列作業：

* 新增初始移轉。
* 以初始移轉更新資料庫。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. 從 [工具] 功能表中，選取 [NuGet 套件管理員] > [套件管理器主控台] (PMC)。

   ![PMC 功能表](~/tutorials/first-mvc-app/adding-model/_static/pmc.png)

1. 在 PMC 中，輸入下列命令：

   ```console
   Add-Migration Initial
   Update-Database
   ```

   `Add-Migration` 命令會產生程式碼來建立初始資料庫結構描述。

   資料庫結構描述是以 `MvcMovieContext` 類別為基礎。 `Initial` 引數是移轉名稱。 您可以使用任何名稱，但依照慣例，會使用描述移轉的名稱。 如需詳細資訊，請參閱 <xref:data/ef-mvc/migrations>。

   `Update-Database` 命令會執行 *Migrations/{時間戳記}_InitialCreate.cs* 檔案中的 `Up` 方法，以建立資料庫。

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

`ef migrations add InitialCreate` 命令會產生程式碼來建立初始資料庫結構描述。

資料庫結構描述會以 `MvcMovieContext` 類別 (在 *Data/MvcMovieContext.cs* 檔案中) 中指定的模型為基礎。 `InitialCreate` 引數是移轉名稱。 您可以使用任何名稱，但依照慣例，會選取描述移轉的名稱。

---

## <a name="examine-the-context-registered-with-dependency-injection"></a>檢查使用相依性插入所註冊的內容

ASP.NET Core 內建[相依性插入 (DI)](xref:fundamentals/dependency-injection)。 服務 (例如 EF Core DB 內容) 會在應用程式啟動期間使用 DI 來註冊。 接著，會透過建構函式參數，針對需要這些服務的元件 (例如 Razor 頁面) 來提供服務。 取得資料庫內容執行個體的建構函式程式碼，本教學課程中稍後會示範。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Scaffolding 工具會自動建立 DB 內容，並向 DI 容器註冊該內容。

請檢查下列 `Startup.ConfigureServices` 方法。 Scaffolder 已新增醒目標示行：

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=14-15)]

`MvcMovieContext` 會協調 `Movie` 模型的 EF Core 功能 (建立、更新、刪除等)。 資料內容 (`MvcMovieContext`) 衍生自 [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)。 資料內容會指定資料模型包含哪些實體：

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Data/MvcMovieContext.cs)]

上述程式碼會建立實體集的 [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) 屬性。 在 Entity Framework 詞彙中，實體集通常會對應至資料庫資料表。 實體會對應至資料表中的資料列。

連接字串的名稱，會透過對 [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) 物件呼叫方法來傳遞至內容。 作為本機開發之用，[ASP.NET Core 設定系統](xref:fundamentals/configuration/index)會從 *appsettings.json* 檔案讀取連接字串。

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

您已建立 DB 內容，並向 DI 容器註冊該內容。

---

<a name="test"></a>

### <a name="test-the-app"></a>測試應用程式

* 執行應用程式，並將 `/Movies` 附加至瀏覽器中的 URL ( `http://localhost:port/movies` )。

如果您收到類似如下的資料庫例外狀況：

```console
SqlException: Cannot open database "MvcMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

您遺失了[移轉步驟](#pmc)。

* 測試 **Create** 連結。 輸入並提交資料。

  > [!NOTE]
  > 您可能無法在 `Price` 欄位中輸入小數逗號。 若要對使用逗號 (",") 作為小數點的非英文地區設定和非英文日期格式支援 [jQuery 驗證](https://jqueryvalidation.org/)，則必須將應用程式全球化。 如需全球化指示，請參閱[此 GitHub 問題](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) \(英文\)。

* 測試 **Edit**、**Details** 和 **Delete** 連結。

檢查 `Startup` 類別：

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=13-99)]

上述醒目提示的程式碼會顯示要新增至[相依性插入](xref:fundamentals/dependency-injection)容器的電影資料庫內容：

* `services.AddDbContext<MvcMovieContext>(options =>` 指定要使用的資料庫和連線字串。
* `=>` 是 [Lambda 運算子](/dotnet/articles/csharp/language-reference/operators/lambda-operator)

開啟 *Controllers/MoviesController.cs* 檔案，並檢查建構函式：

<!-- l.. Make copy of Movies controller because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

建構函式會使用[相依性插入](xref:fundamentals/dependency-injection)將資料庫內容 (`MvcMovieContext`) 插入到控制器中。 控制器中的每一個 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) 方法都會使用資料庫內容。

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a>強型別模型和 @model 關鍵字

稍早在本教學課程中，您已看到控制器如何使用 `ViewData` 字典傳遞資料或物件給檢視。 `ViewData` 字典是一種動態物件，提供便利的晚期繫結方式，將資訊傳遞至檢視。

MVC 也提供將強型別模型物件傳遞至檢視的能力。 強型別方法可讓程式碼的編譯時期檢查變得更佳。 Scaffolding 機制在建立方法和檢視時，已使用此方法 (也就是傳遞強型別模型) 來搭配 `MoviesController` 類別和檢視。

檢查 *Controllers/MoviesController.cs* 檔案中產生的 `Details` 方法：

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_details)]

`id` 參數通常會傳遞為路由資料。 例如，`https://localhost:5001/movies/details/1` 設定：

* 控制器為 `movies` 控制器 (第一個 URL 區段)。
* 動作為 `details` (第二個 URL 區段)。
* 識別碼為 1 (最後一個 URL 區段)。

您也可以在 `id` 中使用查詢字串傳遞，如下所示：

`https://localhost:5001/movies/details?id=1`

若未提供識別碼值，`id` 參數會定義為[可為 Null 的型別](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`)。

[Lambda 運算式](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions)會傳遞至 `FirstOrDefaultAsync`，以選取符合路由資料或查詢字串值的電影實體。

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.Id == id);
```

如果找到電影，則 `Movie` 模型的執行個體會傳遞至 `Details` 檢視：

```csharp
return View(movie);
   ```

檢查 *Views/Movies/Details.cshtml* 檔案的內容：

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/DetailsOriginal.cshtml)]

藉由在檢視檔案的最上方包含 `@model` 陳述式，您可以指定檢視預期要有的物件類型。 當您建立電影控制器時，*Details.cshtml* 檔案的最上方會自動包含下列 `@model` 陳述式：

```HTML
@model MvcMovie.Models.Movie
   ```

這個 `@model` 指示詞可讓您使用強型別的 `Model` 物件，存取控制器傳遞至檢視的電影。 例如，在 *Details.cshtml* 檢視中，程式碼會使用強型別的 `Model` 物件，將每個電影欄位傳遞至 `DisplayNameFor` 和 `DisplayFor` HTML 協助程式。 `Create` 與 `Edit` 方法和檢視也會傳遞 `Movie` 模型物件。

檢查電影控制器中的 *Index.cshtml* 檢視和 `Index` 方法。 請注意程式碼如何在呼叫 `View` 方法時建立 `List` 物件。 此程式碼會從 `Index` 動作方法將 `Movies` 清單傳遞至檢視：

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_index)]

當您建立電影控制器時，Scaffolding 會在 *Index.cshtml* 檔案的最上方自動包含下列 `@model` 陳述式：

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?range=1)]

`@model` 指示詞可讓您使用強型別的 `Model` 物件，存取控制器傳遞至檢視的電影清單。 例如，在 *Index.cshtml* 檢視中，程式碼會透過強型別 `Model` 物件的 `foreach` 陳述式循環存取電影：

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

因為 `Model` 物件是強型別 (作為 `IEnumerable<Movie>` 物件)，所以迴圈中每個項目的類型為 `Movie`。 撇開其他優點，這表示您會進行程式碼編譯時期檢查：

## <a name="additional-resources"></a>其他資源

* [標記協助程式](xref:mvc/views/tag-helpers/intro)
* [全球化和當地語系化](xref:fundamentals/localization)

> [!div class="step-by-step"]
> [上一步：新增檢視](adding-view.md)
> [下一步：使用 SQL](working-with-sql.md)

::: moniker-end
