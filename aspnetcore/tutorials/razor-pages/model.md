---
title: 將模型新增至 ASP.NET Core 中的 Razor 頁面應用程式
author: rick-anderson
description: 了解如何使用 Entity Framework Core (EF Core)，新增用來管理資料庫中電影的類別。
ms.author: riande
ms.date: 12/05/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: f6dbac81b4efceb30c379ab06dd715005d879228
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78658932"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a>將模型新增至 ASP.NET Core 中的 Razor 頁面應用程式

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

<!-- In the next update on the CLI version, let the scaffolder do the same work the VS driven scaffolder does. That is, create the DB context, etc -->

在本節中,將添加類,用於在跨平臺[SQLite 資料庫中](https://www.sqlite.org/index.html)管理影片。 從ASP.NET核心範本創建的應用使用SQLite資料庫。 該應用程式的模型類與[實體框架核心 (EF Core)](/ef/core) [(SQLite EF 核心資料庫提供者](/ef/core/providers/sqlite)) 一起使用,以便與資料庫一起使用。 EF Core 是一種物件關聯式對應 (ORM) 架構，可簡化資料存取。

模型類別稱為 POCO 類別 (來自「簡單的 CLR 物件」)，因為它們對 EF Core 沒有任何相依性。 它們會定義資料儲存在資料庫中的屬性。

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a>新增資料模型

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

以滑鼠右鍵按一下 **RazorPagesMovie** 專案 > [新增]**** > [新增資料夾]****。 將資料夾命名為 *Models*。

以滑鼠右鍵按一下 *Models* 資料夾。 選擇 **「添加** > **類**」。 將類別命名為 **Movie**。

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* 新增名為 *Models* 的資料夾。
* 將類別新增至名為 *Movie.cs* 的 *Models* 資料夾。

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* 在解決方案墊中,右鍵單擊**RazorPagesMovie**專案**Add**,然後>選擇"**添加新資料夾..."** 命名資料夾*模型*。
* 右鍵按下 *"模型"* 資料夾,**Add**>然後選擇 「**添加新檔..."。**
* 在 [新增檔案]**** 對話方塊中：

  * 在左窗格中選取 [一般]****。
  * 在中央窗格中選取 [類別是空的]****。
  * 將類別命名為 **Movie**，然後選取 [新增]****。

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

---

建置專案，以確認沒有任何編譯錯誤。

## <a name="scaffold-the-movie-model"></a>Scaffold 影片模型

在本節中會 scaffold 影片模型。 亦即 Scaffolding 工具會產生影片模型的建立、讀取、更新和刪除 (CRUD) 作業頁面。

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

建立 *Pages/Movies* 資料夾：

* 以滑鼠右鍵按一下 [Pages]** 資料夾 > [新增]** [新增資料夾]** > ****。
* 將資料夾命名為 *Movies*

以滑鼠右鍵按一下 [Pages/Movies]** 資料夾 > [新增]** [新增 Scaffolded 項目]** > ****。

![前述指示中的圖片。](model/_static/sca.png)

在 [新增 Scaffold]**** 對話方塊中，選取 [使用 Entity Framework 的 Razor Pages (CRUD)]** [新增]** > ****。

![前述指示中的圖片。](model/_static/add_scaffold.png)

完成 [Add Razor Pages using Entity Framework (CRUD)] \(新增使用 Entity Framework 的 Razor Pages (CRUD)\)**** 對話方塊：

* 在 [模型類別]**** 下拉式清單中選取 [Movie (RazorPagesMovie.Models)] \(影片 (RazorPagesMovie.Models)\)****。
* 在 [資料內容類別]**** 列中選取 [+]**** \(加號\)，並將產生的名稱 RazorPagesMovie.**Models**.RazorPagesMovieContext 變更為 RazorPagesMovie.**Data**.RazorPagesMovieContext。 這不是必要的[變更](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) \(英文\)。 它會使用正確的命名空間來建立資料庫內容類別。
* 選取 [新增]  。

![前述指示中的圖片。](model/_static/3/arp.png)

*appsettings.json* 檔案會隨即更新用來連線到本機資料庫的連接字串。

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* 在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。
* 安裝 Scaffolding 工具：

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* **對於 Windows**:執行以下指令:

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* **針對 macOS 與 Linux**：執行下列命令：

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  dotnet tool install --global dotnet-aspnet-codegenerator
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

[!INCLUDE [use SQL Server in production](~/includes/RP/sqlitedev.md)]

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

建立 *Pages/Movies* 資料夾：

* 以滑鼠右鍵按一下 [Pages]** 資料夾 > [新增]** [新增資料夾]** > ****。
* 將資料夾命名為 *Movies*

右鍵單擊 *"頁面/電影*"資料夾 **>添加新**>**基架..."**

![前述指示中的圖片。](model/_static/scaMac.png)

在 **"新建基架"對話框中**,選擇 **"使用實體框架 (CRUD)** > **下一步**剃刀頁面"。

![前述指示中的圖片。](model/_static/add_scaffoldMac.png)

完成 [Add Razor Pages using Entity Framework (CRUD)] \(新增使用 Entity Framework 的 Razor Pages (CRUD)\)**** 對話方塊：

* 在**模型"類**中,下拉、選擇或鍵入**影片(RazorPagesMovie.Model)。**
* 在 **「數據上下文」類**行中,鍵入新類 RazorPagesMovie 的名稱。**資料**。剃刀頁電影上下文。 這不是必要的[變更](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) \(英文\)。 它會使用正確的命名空間來建立資料庫內容類別。
* 選取 [新增]  。

![前述指示中的圖片。](model/_static/arpMac.png)

*appsettings.json* 檔案會隨即更新用來連線到本機資料庫的連接字串。

### <a name="add-ef-tools"></a>新增 EF 工具

執行以下 .NET 核心 CLI 指令:

```dotnetcli
dotnet tool install --global dotnet-ef
```

前面的命令為 .NET 核心 CLI 添加了實體框架核心工具。

---

### <a name="files-created"></a>建立的檔案

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

隨即建立 Scaffold 處理序並更新下列檔案：

* *Pages/Movies*：建立、刪除、詳細資料、編輯和索引。
* *Data/RazorPagesMovieContext.cs*

### <a name="updated"></a>已更新

* *Startup.cs*

下一節將說明所建立和更新的檔案。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

隨即建立 Scaffold 處理序並更新下列檔案：

* *Pages/Movies*：建立、刪除、詳細資料、編輯和索引。
* *Data/RazorPagesMovieContext.cs*

### <a name="updated"></a>已更新

* *Startup.cs*

下一節將說明所建立和更新的檔案。

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Scaffold 處理序會建立下列檔案：

* *Pages/Movies*：建立、刪除、詳細資料、編輯和索引。

下一節將說明所建立的檔案。

---

<a name="pmc"></a>

## <a name="initial-migration"></a>初始移轉

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

在本節中，您可以使用套件管理員主控台 (PMC) 進行下列作業：

* 新增初始移轉。
* 以初始移轉更新資料庫。

從 [工具]**** 功能表中，選取 [NuGet 封裝管理員]**[封裝管理員主控台]** > ****。

  ![PMC 功能表](../first-mvc-app/adding-model/_static/pmc.png)

在 PMC 中，輸入下列命令：

```powershell
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

前面的命令生成以下警告:「未為實體類型」Movie"上的十進位列"價格"指定類型。 如果它們不符合預設的有效位數和小數位數，會導致以無訊息模式截斷這些值。 使用 'HasColumnType()' 明確指定可容納所有值的 SQL Server 資料行類型。」

您可以忽略該警告，稍後的教學課程中將修正此問題。

遷移命令生成代碼以創建初始資料庫架構。 架構基於`DbContext`中 指定的模型。 `InitialCreate` 引數用來命名移轉。 您可以使用任何名稱，但依照慣例，會選取描述移轉的名稱。

該`update`命令`Up`在尚未應用的遷移中運行該方法。 在這種情況下,`update``Up`在*建立資料庫的移轉\</ 時間戳>_InitialCreate.cs*檔中執行該方法。

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a>檢查使用相依性插入所註冊的內容

ASP.NET Core 內建[相依性插入](xref:fundamentals/dependency-injection)。 服務 (例如 EF Core DB 內容) 是在應用程式啟動期間使用相依性插入來註冊。 接著，會透過建構函式參數，針對需要這些服務的元件 (例如 Razor 頁面) 來提供服務。 取得資料庫內容執行個體的建構函式程式碼，本教學課程中稍後會示範。

Scaffolding 工具會自動建立資料庫內容，並向相依性插入容器註冊。

檢查 `Startup.ConfigureServices` 方法。 強調顯示的行由 Scaffolder 新增：

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

`RazorPagesMovieContext` 會協調 `Movie` 模型的 EF Core 功能 (建立、更新、刪除等)。 資料內容 (`RazorPagesMovieContext`) 衍生自 [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)。 資料內容會指定資料模型包含哪些實體。

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

上述程式碼會建立實體集的 [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) 屬性。 在 Entity Framework 詞彙中，實體集通常會對應至資料庫資料表。 實體會對應至資料表中的資料列。

連接字串的名稱，會透過對 [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) 物件呼叫方法來傳遞至內容。 作為本機開發之用，[ASP.NET Core configuration system](xref:fundamentals/configuration/index) 會從 *appsettings.json* 檔案讀取連接字串。

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

檢查 `Up` 方法。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

檢查 `Up` 方法。

---

<a name="test"></a>

### <a name="test-the-app"></a>測試應用程式

* 執行應用程式，並將 `/Movies` 附加至瀏覽器中的 URL ( `http://localhost:port/movies` )。

如果您收到錯誤：

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

您遺失了[移轉步驟](#pmc)。

* 測試 **Create** 連結。

  ![Create page](model/_static/conan.png)

  > [!NOTE]
  > 您可能無法在 `Price` 欄位中輸入小數逗號。 若要對使用逗號 (",") 作為小數點的非英文地區設定和非英文日期格式支援 [jQuery 驗證](https://jqueryvalidation.org/)，則必須將應用程式全球化。 如需全球化指示，請參閱[此 GitHub 問題](https://github.com/dotnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) \(英文\)。

* 測試 [編輯]****、[詳細資料]**** 和 [刪除]**** 連結。

下一個教學課程說明 Scaffolding 所建立的檔案。

## <a name="additional-resources"></a>其他資源

> [!div class="step-by-step"]
> [上一篇:](xref:tutorials/razor-pages/razor-pages-start)
> [開始下一步: 腳手架剃刀頁](xref:tutorials/razor-pages/page)

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

在本節中,將添加類,用於在跨平臺[SQLite 資料庫中](https://www.sqlite.org/index.html)管理影片。 從ASP.NET核心範本創建的應用使用SQLite資料庫。 該應用程式的模型類與[實體框架核心 (EF Core)](/ef/core) [(SQLite EF 核心資料庫提供者](/ef/core/providers/sqlite)) 一起使用,以便與資料庫一起使用。 EF Core 是一種物件關聯式對應 (ORM) 架構，可簡化資料存取。

模型類別稱為 POCO 類別 (來自「簡單的 CLR 物件」)，因為它們對 EF Core 沒有任何相依性。 它們會定義資料儲存在資料庫中的屬性。

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a>新增資料模型

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

以滑鼠右鍵按一下 **RazorPagesMovie** 專案 > [新增]**** > [新增資料夾]****。 將資料夾命名為 *Models*。

以滑鼠右鍵按一下 *Models* 資料夾。 選擇 **「添加** > **類**」。 將類別命名為 **Movie**。

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* 新增名為 *Models* 的資料夾。
* 將類別新增至名為 *Movie.cs* 的 *Models* 資料夾。

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* 在方案總管中，以滑鼠右鍵按一下 **RazorPagesMovie** 專案，然後選取 [新增]**** > [新增資料夾]****。 將資料夾命名為 *Models*。
* 以滑鼠右鍵按一下 [Models]** 資料夾，然後選取 [新增]** [新增檔案]** > ****。
* 在 [新增檔案]**** 對話方塊中：

  * 在左窗格中選取 [一般]****。
  * 在中央窗格中選取 [類別是空的]****。
  * 將類別命名為 **Movie**，然後選取 [新增]****。

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

---

建置專案，以確認沒有任何編譯錯誤。

## <a name="scaffold-the-movie-model"></a>Scaffold 影片模型

在本節中會 scaffold 影片模型。 亦即 Scaffolding 工具會產生影片模型的建立、讀取、更新和刪除 (CRUD) 作業頁面。

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

建立 *Pages/Movies* 資料夾：

* 以滑鼠右鍵按一下 [Pages]** 資料夾 > [新增]** [新增資料夾]** > ****。
* 將資料夾命名為 *Movies*

以滑鼠右鍵按一下 [Pages/Movies]** 資料夾 > [新增]** [新增 Scaffolded 項目]** > ****。

![前述指示中的圖片。](model/_static/sca.png)

在 [新增 Scaffold]**** 對話方塊中，選取 [使用 Entity Framework 的 Razor Pages (CRUD)]** [新增]** > ****。

![前述指示中的圖片。](model/_static/add_scaffold.png)

完成 [Add Razor Pages using Entity Framework (CRUD)] \(新增使用 Entity Framework 的 Razor Pages (CRUD)\)**** 對話方塊：
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* 在 [模型類別]**** 下拉式清單中選取 [Movie (RazorPagesMovie.Models)] \(影片 (RazorPagesMovie.Models)\)****。
* 在 [資料內容類別]**** 列中選取 [+]**** (加號)，並接受產生的名稱 **RazorPagesMovie.Models.RazorPagesMovieContext**。
* 選取 [新增]  。

![前述指示中的圖片。](model/_static/arp.png)

*appsettings.json* 檔案會隨即更新用來連線到本機資料庫的連接字串。

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* 在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。

* **對於 Windows**:執行以下指令:

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* **針對 macOS 與 Linux**：執行下列命令：

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

建立 *Pages/Movies* 資料夾：

* 以滑鼠右鍵按一下 [Pages]** 資料夾 > [新增]** [新增資料夾]** > ****。
* 將資料夾命名為 *Movies*

以滑鼠右鍵按一下 [Pages/Movies]** 資料夾 > [新增]** [新增 Scaffolded 項目]** > ****。

![前述指示中的圖片。](model/_static/scaMac.png)

在 **「新增新基架」對話框中**,**選擇使用實體框架 (CRUD)** >**添加**的剃刀頁面。

![前述指示中的圖片。](model/_static/add_scaffoldMac.png)

完成 [Add Razor Pages using Entity Framework (CRUD)] \(新增使用 Entity Framework 的 Razor Pages (CRUD)\)**** 對話方塊：

* 在**模型"類**下拉清單中,選擇或鍵入 **「影片**」。
* 在 **「資料上下文類」行**中,鍵入 **「RazorPages MovieContext」,** 這將創建具有正確命名空間的新 db 上下文類。 在這種情況下,這將是**RazorPages 電影.模型.RazorPages 電影上下文**。
* 選取 [新增]  。

![前述指示中的圖片。](model/_static/arpMac.png)

*appsettings.json* 檔案會隨即更新用來連線到本機資料庫的連接字串。

---

隨即建立 Scaffold 處理序並更新下列檔案：

### <a name="files-created"></a>建立的檔案

* *Pages/Movies*：建立、刪除、詳細資料、編輯和索引。
* *Data/RazorPagesMovieContext.cs*

### <a name="file-updated"></a>檔案已更新

* *Startup.cs*

下一節將說明所建立和更新的檔案。

<a name="pmc"></a>

## <a name="initial-migration"></a>初始移轉

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

在本節中，您可以使用套件管理員主控台 (PMC) 進行下列作業：

* 新增初始移轉。
* 以初始移轉更新資料庫。

從 [工具]**** 功能表中，選取 [NuGet 封裝管理員]**[封裝管理員主控台]** > ****。

  ![PMC 功能表](../first-mvc-app/adding-model/_static/pmc.png)

在 PMC 中，輸入下列命令：

```powershell
Add-Migration Initial
Update-Database
```

`Add-Migration` 命令會產生程式碼來建立初始資料庫結構描述。 結構描述是以 `DbContext` (在 *RazorPagesMovieContext.cs* 檔案中) 中指定的模型為基礎。 參數`InitialCreate`用於命名遷移。 您可以使用任何名稱，但依照慣例，會使用描述移轉的名稱。 如需詳細資訊，請參閱 <xref:data/ef-mvc/migrations>。

`Update-Database` 命令會執行 *Migrations/\<時間戳記>_InitialCreate.cs* 檔案中的 `Up` 方法。 `Up` 方法會建立資料庫。

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---
> [!NOTE]
> 前面的命令生成以下警告:"*未為實體類型"Movie"上的十進位列"價格"指定類型。如果值不適合預設精度和比例,則會導致值被靜默截斷。顯式指定 SQL Server 列類型,該類型可以使用"HasColumnType()「容納所有值。* 您可以忽略該警告,它將在後面的教程中修復。

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a>檢查使用相依性插入所註冊的內容

ASP.NET Core 內建[相依性插入](xref:fundamentals/dependency-injection)。 服務 (例如 EF Core DB 內容) 是在應用程式啟動期間使用相依性插入來註冊。 接著，會透過建構函式參數，針對需要這些服務的元件 (例如 Razor 頁面) 來提供服務。 取得資料庫內容執行個體的建構函式程式碼，本教學課程中稍後會示範。

Scaffolding 工具會自動建立資料庫內容，並向相依性插入容器註冊。

檢查 `Startup.ConfigureServices` 方法。 強調顯示的行由 Scaffolder 新增：

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

`RazorPagesMovieContext` 會協調 `Movie` 模型的 EF Core 功能 (建立、更新、刪除等)。 資料內容 (`RazorPagesMovieContext`) 衍生自 [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)。 資料內容會指定資料模型包含哪些實體。

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

上述程式碼會建立實體集的 [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) 屬性。 在 Entity Framework 詞彙中，實體集通常會對應至資料庫資料表。 實體會對應至資料表中的資料列。

連接字串的名稱，會透過對 [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) 物件呼叫方法來傳遞至內容。 作為本機開發之用，[ASP.NET Core configuration system](xref:fundamentals/configuration/index) 會從 *appsettings.json* 檔案讀取連接字串。

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

檢查 `Up` 方法。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

檢查 `Up` 方法。

---

<a name="test"></a>

### <a name="test-the-app"></a>測試應用程式

* 執行應用程式，並將 `/Movies` 附加至瀏覽器中的 URL ( `http://localhost:port/movies` )。

如果您收到錯誤：

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

您遺失了[移轉步驟](#pmc)。

* 測試 **Create** 連結。

  ![Create page](model/_static/conan.png)

  > [!NOTE]
  > 您可能無法在 `Price` 欄位中輸入小數逗號。 若要對使用逗號 (",") 作為小數點的非英文地區設定和非英文日期格式支援 [jQuery 驗證](https://jqueryvalidation.org/)，則必須將應用程式全球化。 如需全球化指示，請參閱[此 GitHub 問題](https://github.com/dotnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) \(英文\)。

* 測試 [編輯]****、[詳細資料]**** 和 [刪除]**** 連結。

下一個教學課程說明 Scaffolding 所建立的檔案。

## <a name="additional-resources"></a>其他資源

> [!div class="step-by-step"]
> [上一篇:](xref:tutorials/razor-pages/razor-pages-start)
> [開始下一步: 腳手架剃刀頁](xref:tutorials/razor-pages/page)

::: moniker-end
