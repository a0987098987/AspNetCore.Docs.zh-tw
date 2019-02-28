---
title: 新增模型到 ASP.NET Core MVC 應用程式
author: rick-anderson
description: 請將模型新增至簡單的 ASP.NET Core 應用程式。
ms.author: riande
ms.date: 02/25/2019
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: ccdb7b920517c94b9154fe73b4ef1633f4ad0157
ms.sourcegitcommit: 2c7ffe349eabdccf2ed748dd303ffd0ba6e1cfe3
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/26/2019
ms.locfileid: "56833549"
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a>新增模型到 ASP.NET Core MVC 應用程式

作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Ryan Nowak](https://github.com/tdykstra)

在本節中，您可以新增類別來管理資料庫中的電影。 這些類別是 **MVC** 應用程式的「模型」部分。

搭配 [Entity Framework Core](/ef/core) (EF Core) 使用這些類別，即可使用資料庫。 EF Core 是一種物件關聯式對應 (ORM) 架構，可簡化您必須撰寫的資料存取程式碼。

您所建立的模型類別稱為 POCO 類別 (來自「純舊 CLR 物件」)，因為它們對 EF Core 沒有任何相依性。 它們只會定義資料庫將儲存之資料的屬性。

在本教學課程中，您首先要撰寫模型類別，而 EF Core 會建立資料庫。 本文未提及的替代方法是從現有的資料庫產生模型類別。 如需該方法的資訊，請參閱 [ASP.NET Core - 現有的資料庫](/ef/core/get-started/aspnetcore/existing-db)。

## <a name="add-a-data-model-class"></a>新增資料模型類別

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

以滑鼠右鍵按一下 *Models* 資料夾 > [新增] > [類別]。 將類別命名為 **Movie**。

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]

<!-- Code -------------------------->
# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

* 將類別新增至名為 *Movie.cs* 的 *Models* 資料夾。

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]
[!INCLUDE [model 2](~/includes/mvc-intro/model2.md)]

---  
<!-- End of VS tabs -->

## <a name="scaffold-the-movie-model"></a>Scaffold 影片模型

在本節中會 scaffold 影片模型。 亦即 Scaffolding 工具會產生影片模型的建立、讀取、更新和刪除 (CRUD) 作業頁面。

<!-- VS -------------------------->
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
* Create、Delete、Details、Edit 和 Index 頁面的 Razor 檢視檔案 (<em>Views/Movies/&ast;.cshtml</em>)

自動建立資料庫內容與 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (建立、讀取、更新和刪除) 動作方法和檢視稱為 *Scaffolding*。

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* 在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。
* 安裝 Scaffolding 工具：

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* 執行下列命令：

  ```console
     dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* 在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。
* 安裝 Scaffolding 工具：

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* 執行下列命令：

  ```console
     dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<!-- End of VS tabs                  -->

如果執行應用程式，然後按一下 **Mvc Movie** 連結，則會收到如下錯誤：

``` error
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString
```

您需要建立資料庫，而且您會使用 EF Core [移轉](xref:data/ef-mvc/migrations)功能來執行此作業。 移轉可讓您建立符合資料模型的資料庫，並在資料模型變更時，更新資料庫結構描述。

<a name="pmc"></a>

## <a name="initial-migration"></a>初始移轉

在本節中，將會完成下列作業：

* 新增初始移轉。
* 以初始移轉更新資料庫。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. 從 [工具] 功能表中，選取 [NuGet 套件管理員] > [套件管理員主控台] (PMC)。

   ![PMC 功能表](~/tutorials/first-mvc-app/adding-model/_static/pmc.png)

1. 在 PMC 中，輸入下列命令：

   ```console
   Add-Migration Initial
   Update-Database
   ```

   `Add-Migration` 命令會產生程式碼來建立初始資料庫結構描述。

   資料庫結構描述會以 `MvcMovieContext` 類別 (在 *Data/MvcMovieContext.cs* 檔案中) 中指定的模型為基礎。 `Initial` 引數是移轉名稱。 您可以使用任何名稱，但依照慣例，會使用描述移轉的名稱。 如需詳細資訊，請參閱<xref:data/ef-mvc/migrations>。

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

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

`MvcMovieContext` 會協調 `Movie` 模型的 EF Core 功能 (建立、更新、刪除等)。 資料內容 (`MvcMovieContext`) 衍生自 [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)。 資料內容會指定資料模型包含哪些實體：

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Data/MvcMovieContext.cs)]

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

* 測試 **Create** 連結。

  > [!NOTE]
  > 您可能無法在 `Price` 欄位中輸入小數逗號。 若要對使用逗號 (",") 作為小數點的非英文地區設定和非英文日期格式支援 [jQuery 驗證](https://jqueryvalidation.org/)，則必須將應用程式全球化。 如需全球化指示，請參閱[此 GitHub 問題](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) \(英文\)。

* 測試 **Edit**、**Details** 和 **Delete** 連結。

檢查 `Startup` 類別：

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=13-99)]

上述醒目提示的程式碼會顯示要新增至[相依性插入](xref:fundamentals/dependency-injection)容器的電影資料庫內容：

* `services.AddDbContext<MvcMovieContext>(options =>` 指定要使用的資料庫和連線字串。
* `=>` 是 [Lambda 運算子](/dotnet/articles/csharp/language-reference/operators/lambda-operator)

開啟 *Controllers/MoviesController.cs* 檔案，並檢查建構函式：

<!-- l.. Make copy of Movies controller because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

建構函式會使用[相依性插入](xref:fundamentals/dependency-injection)將資料庫內容 (`MvcMovieContext `) 插入到控制器中。 控制器中的每一個 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) 方法都會使用資料庫內容。

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
