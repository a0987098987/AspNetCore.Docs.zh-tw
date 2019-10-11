---
title: 使用資料庫和 ASP.NET Core
author: rick-anderson
description: 說明如何使用資料庫和 ASP.NET Core。
ms.author: riande
ms.date: 7/22/2019
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 87d27b60940826e21b060f2e07d344b30ff75b27
ms.sourcegitcommit: 7d3c6565dda6241eb13f9a8e1e1fd89b1cfe4d18
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/11/2019
ms.locfileid: "72259791"
---
# <a name="work-with-a-database-and-aspnet-core"></a><span data-ttu-id="738e1-103">使用資料庫和 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="738e1-103">Work with a database and ASP.NET Core</span></span>

<span data-ttu-id="738e1-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="738e1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="738e1-105">`RazorPagesMovieContext` 物件會處理連線到資料庫和將 `Movie` 物件對應至資料庫記錄的工作。</span><span class="sxs-lookup"><span data-stu-id="738e1-105">The `RazorPagesMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="738e1-106">在 *Startup.cs* 的 `ConfigureServices` 方法中，以[相依性插入](xref:fundamentals/dependency-injection)容器登錄資料庫內容：</span><span class="sxs-lookup"><span data-stu-id="738e1-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in *Startup.cs*:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="738e1-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="738e1-107">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="738e1-108">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="738e1-108">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

---

<span data-ttu-id="738e1-109">ASP.NET Core [組態](xref:fundamentals/configuration/index)系統會讀取 `ConnectionString`。</span><span class="sxs-lookup"><span data-stu-id="738e1-109">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="738e1-110">對於本機開發，它會從 *appsettings.json* 檔案取得連接字串。</span><span class="sxs-lookup"><span data-stu-id="738e1-110">For local development, it gets the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="738e1-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="738e1-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="738e1-112">您的產生程式碼會有不同的資料庫 (`Database={Database name}`) 名稱值。</span><span class="sxs-lookup"><span data-stu-id="738e1-112">The name value for the database (`Database={Database name}`) will be different for your generated code.</span></span> <span data-ttu-id="738e1-113">名稱值為任意值。</span><span class="sxs-lookup"><span data-stu-id="738e1-113">The name value is arbitrary.</span></span>

[!code-json[](razor-pages-start/sample/RazorPagesMovie30/appsettings.json)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="738e1-114">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="738e1-114">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

---

<span data-ttu-id="738e1-115">將應用程式部署到測試或生產環境伺服器時，可以使用環境變數來設定實際資料庫伺服器的連接字串。</span><span class="sxs-lookup"><span data-stu-id="738e1-115">When the app is deployed to a test or production server, an environment variable can be used to set the connection string to a real database server.</span></span> <span data-ttu-id="738e1-116">如需詳細資訊，請參閱[組態](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="738e1-116">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="738e1-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="738e1-117">Visual Studio</span></span>](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a><span data-ttu-id="738e1-118">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="738e1-118">SQL Server Express LocalDB</span></span>

<span data-ttu-id="738e1-119">LocalDB 為輕量版的 SQL Server Express 資料庫引擎，鎖定程式開發為其目標。</span><span class="sxs-lookup"><span data-stu-id="738e1-119">LocalDB is a lightweight version of the SQL Server Express database engine that's targeted for program development.</span></span> <span data-ttu-id="738e1-120">LocalDB 會依需求啟動，並以使用者模式執行，因此沒有複雜的組態。</span><span class="sxs-lookup"><span data-stu-id="738e1-120">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="738e1-121">LocalDB 資料庫預設會在 `C:/Users/<user/>` 目錄中建立 `*.mdf` 檔案。</span><span class="sxs-lookup"><span data-stu-id="738e1-121">By default, LocalDB database creates `*.mdf` files in the `C:/Users/<user/>` directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="738e1-122">從 [檢視] 功能表中，開啟 [SQL Server 物件總管] (SSOX)。</span><span class="sxs-lookup"><span data-stu-id="738e1-122">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![檢視功能表](sql/_static/ssox.png)

* <span data-ttu-id="738e1-124">以滑鼠右鍵按一下 `Movie` 資料表，並選取 [檢視表設計工具]：</span><span class="sxs-lookup"><span data-stu-id="738e1-124">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![在 Movie 資料表上開啟的操作功能表](sql/_static/design.png)

  ![在設計工具中開啟的 Movie 資料表](sql/_static/dv.png)

<span data-ttu-id="738e1-127">請注意 `ID` 旁的索引鍵圖示。</span><span class="sxs-lookup"><span data-stu-id="738e1-127">Note the key icon next to `ID`.</span></span> <span data-ttu-id="738e1-128">根據預設，EF 會為主索引鍵建立名為 `ID` 的屬性。</span><span class="sxs-lookup"><span data-stu-id="738e1-128">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="738e1-129">以滑鼠右鍵按一下 `Movie` 資料表，並選取 [檢視資料]：</span><span class="sxs-lookup"><span data-stu-id="738e1-129">Right click on the `Movie` table and select **View Data**:</span></span>

  ![開啟的電影資料表顯示資料表資料](sql/_static/vd22.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="738e1-131">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="738e1-131">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

---

## <a name="seed-the-database"></a><span data-ttu-id="738e1-132">植入資料庫</span><span class="sxs-lookup"><span data-stu-id="738e1-132">Seed the database</span></span>

<span data-ttu-id="738e1-133">使用下列程式碼，在 *Models* 資料夾中建立名為 `SeedData` 的新類別：</span><span class="sxs-lookup"><span data-stu-id="738e1-133">Create a new class named `SeedData` in the *Models* folder with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="738e1-134">如果資料庫中有任何電影，則種子初始設定式會返回，而且不會新增任何電影。</span><span class="sxs-lookup"><span data-stu-id="738e1-134">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>

### <a name="add-the-seed-initializer"></a><span data-ttu-id="738e1-135">新增種子初始設定式</span><span class="sxs-lookup"><span data-stu-id="738e1-135">Add the seed initializer</span></span>

<span data-ttu-id="738e1-136">在 *Program.cs* 中，修改 `Main` 方法來執行下列動作：</span><span class="sxs-lookup"><span data-stu-id="738e1-136">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="738e1-137">從相依性插入容器中取得資料庫內容執行個體。</span><span class="sxs-lookup"><span data-stu-id="738e1-137">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="738e1-138">呼叫種子方法，並將其傳遞給內容。</span><span class="sxs-lookup"><span data-stu-id="738e1-138">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="738e1-139">種子方法完成時處理內容。</span><span class="sxs-lookup"><span data-stu-id="738e1-139">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="738e1-140">下列程式碼顯示已更新的 *Program.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="738e1-140">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Program.cs)]

<span data-ttu-id="738e1-141">未執行 `Update-Database` 時，會發生下列例外狀況：</span><span class="sxs-lookup"><span data-stu-id="738e1-141">The following exception occurs when `Update-Database` has not been run:</span></span>

`SqlException: Cannot open database "RazorPagesMovieContext-" requested by the login. The login failed.`
`Login failed for user 'user name'.`

### <a name="test-the-app"></a><span data-ttu-id="738e1-142">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="738e1-142">Test the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="738e1-143">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="738e1-143">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="738e1-144">刪除資料庫中的所有記錄。</span><span class="sxs-lookup"><span data-stu-id="738e1-144">Delete all the records in the DB.</span></span> <span data-ttu-id="738e1-145">您可以使用瀏覽器或 [SSOX](xref:tutorials/razor-pages/new-field#ssox) 的刪除連結來執行這項操作</span><span class="sxs-lookup"><span data-stu-id="738e1-145">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="738e1-146">強制應用程式初始化 (呼叫 `Startup` 類別中的方法)，以執行植入方法。</span><span class="sxs-lookup"><span data-stu-id="738e1-146">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="738e1-147">若要強制初始化，IIS Express 必須停止並重新啟動。</span><span class="sxs-lookup"><span data-stu-id="738e1-147">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="738e1-148">您可以使用下列其中一個方法來執行此工作：</span><span class="sxs-lookup"><span data-stu-id="738e1-148">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="738e1-149">以滑鼠右鍵按一下通知區域中的 IIS Express 系統匣圖示，然後點選 [結束] 或 [停止網站]：</span><span class="sxs-lookup"><span data-stu-id="738e1-149">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![IIS Express 系統匣圖示](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![操作功能表](sql/_static/stopIIS.png)

    * <span data-ttu-id="738e1-152">如果您已在非偵錯模式中執行 VS，請按 F5 以偵錯模式執行。</span><span class="sxs-lookup"><span data-stu-id="738e1-152">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
    * <span data-ttu-id="738e1-153">如果您已在偵錯模式中執行 VS，請停止偵錯工具，然後按 F5。</span><span class="sxs-lookup"><span data-stu-id="738e1-153">If you were running VS in debug mode, stop the debugger and press F5.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="738e1-154">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="738e1-154">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="738e1-155">請刪除資料庫中的所有記錄 (這樣會執行 seed 方法)。</span><span class="sxs-lookup"><span data-stu-id="738e1-155">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="738e1-156">停止並啟動應用程式來植入資料庫。</span><span class="sxs-lookup"><span data-stu-id="738e1-156">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="738e1-157">應用程式會顯示植入的資料。</span><span class="sxs-lookup"><span data-stu-id="738e1-157">The app shows the seeded data.</span></span>

---

<span data-ttu-id="738e1-158">接下來的教學課程將會改善資料的呈現。</span><span class="sxs-lookup"><span data-stu-id="738e1-158">The next tutorial will improve the presentation of the data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="738e1-159">其他資源</span><span class="sxs-lookup"><span data-stu-id="738e1-159">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="738e1-160">[上一步：Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
> [下一步：更新頁面](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="738e1-160">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="738e1-161">`RazorPagesMovieContext` 物件會處理連線到資料庫和將 `Movie` 物件對應至資料庫記錄的工作。</span><span class="sxs-lookup"><span data-stu-id="738e1-161">The `RazorPagesMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="738e1-162">在 *Startup.cs* 的 `ConfigureServices` 方法中，以[相依性插入](xref:fundamentals/dependency-injection)容器登錄資料庫內容：</span><span class="sxs-lookup"><span data-stu-id="738e1-162">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in *Startup.cs*:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="738e1-163">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="738e1-163">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="738e1-164">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="738e1-164">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

---

<span data-ttu-id="738e1-165">如需 `ConfigureServices` 中所用之方法的詳細資訊，請參閱：</span><span class="sxs-lookup"><span data-stu-id="738e1-165">For more information on the methods used in `ConfigureServices`, see:</span></span>

* <span data-ttu-id="738e1-166">適用於 `CookiePolicyOptions` 的 [ASP.NET Core 中的 EU 一般資料保護規定 (GDPR) 支援](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="738e1-166">[EU General Data Protection Regulation (GDPR) support in ASP.NET Core](xref:security/gdpr) for `CookiePolicyOptions`.</span></span>
* [<span data-ttu-id="738e1-167">SetCompatibilityVersion</span><span class="sxs-lookup"><span data-stu-id="738e1-167">SetCompatibilityVersion</span></span>](xref:mvc/compatibility-version)

<span data-ttu-id="738e1-168">ASP.NET Core [組態](xref:fundamentals/configuration/index)系統會讀取 `ConnectionString`。</span><span class="sxs-lookup"><span data-stu-id="738e1-168">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="738e1-169">對於本機開發，它會從 *appsettings.json* 檔案取得連接字串。</span><span class="sxs-lookup"><span data-stu-id="738e1-169">For local development, it gets the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="738e1-170">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="738e1-170">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="738e1-171">您的產生程式碼會有不同的資料庫 (`Database={Database name}`) 名稱值。</span><span class="sxs-lookup"><span data-stu-id="738e1-171">The name value for the database (`Database={Database name}`) will be different for your generated code.</span></span> <span data-ttu-id="738e1-172">名稱值為任意值。</span><span class="sxs-lookup"><span data-stu-id="738e1-172">The name value is arbitrary.</span></span>

[!code-json[](razor-pages-start/sample/RazorPagesMovie22/appsettings.json)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="738e1-173">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="738e1-173">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="738e1-174">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="738e1-174">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

---

<span data-ttu-id="738e1-175">將應用程式部署到測試或生產環境伺服器時，可以使用環境變數來設定實際資料庫伺服器的連接字串。</span><span class="sxs-lookup"><span data-stu-id="738e1-175">When the app is deployed to a test or production server, an environment variable can be used to set the connection string to a real database server.</span></span> <span data-ttu-id="738e1-176">如需詳細資訊，請參閱[組態](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="738e1-176">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="738e1-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="738e1-177">Visual Studio</span></span>](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a><span data-ttu-id="738e1-178">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="738e1-178">SQL Server Express LocalDB</span></span>

<span data-ttu-id="738e1-179">LocalDB 為輕量版的 SQL Server Express 資料庫引擎，鎖定程式開發為其目標。</span><span class="sxs-lookup"><span data-stu-id="738e1-179">LocalDB is a lightweight version of the SQL Server Express database engine that's targeted for program development.</span></span> <span data-ttu-id="738e1-180">LocalDB 會依需求啟動，並以使用者模式執行，因此沒有複雜的組態。</span><span class="sxs-lookup"><span data-stu-id="738e1-180">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="738e1-181">LocalDB 資料庫預設會在 `C:/Users/<user/>` 目錄中建立 `*.mdf` 檔案。</span><span class="sxs-lookup"><span data-stu-id="738e1-181">By default, LocalDB database creates `*.mdf` files in the `C:/Users/<user/>` directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="738e1-182">從 [檢視] 功能表中，開啟 [SQL Server 物件總管] (SSOX)。</span><span class="sxs-lookup"><span data-stu-id="738e1-182">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![檢視功能表](sql/_static/ssox.png)

* <span data-ttu-id="738e1-184">以滑鼠右鍵按一下 `Movie` 資料表，並選取 [檢視表設計工具]：</span><span class="sxs-lookup"><span data-stu-id="738e1-184">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![在電影資料表上開啟操作功能表](sql/_static/design.png)

  ![在設計工具中開啟電影資料表](sql/_static/dv.png)

<span data-ttu-id="738e1-187">請注意 `ID` 旁的索引鍵圖示。</span><span class="sxs-lookup"><span data-stu-id="738e1-187">Note the key icon next to `ID`.</span></span> <span data-ttu-id="738e1-188">根據預設，EF 會為主索引鍵建立名為 `ID` 的屬性。</span><span class="sxs-lookup"><span data-stu-id="738e1-188">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="738e1-189">以滑鼠右鍵按一下 `Movie` 資料表，並選取 [檢視資料]：</span><span class="sxs-lookup"><span data-stu-id="738e1-189">Right click on the `Movie` table and select **View Data**:</span></span>

  ![開啟的電影資料表顯示資料表資料](sql/_static/vd22.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="738e1-191">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="738e1-191">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="738e1-192">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="738e1-192">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

---

## <a name="seed-the-database"></a><span data-ttu-id="738e1-193">植入資料庫</span><span class="sxs-lookup"><span data-stu-id="738e1-193">Seed the database</span></span>

<span data-ttu-id="738e1-194">使用下列程式碼，在 *Models* 資料夾中建立名為 `SeedData` 的新類別：</span><span class="sxs-lookup"><span data-stu-id="738e1-194">Create a new class named `SeedData` in the *Models* folder with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="738e1-195">如果資料庫中有任何電影，則種子初始設定式會返回，而且不會新增任何電影。</span><span class="sxs-lookup"><span data-stu-id="738e1-195">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>

### <a name="add-the-seed-initializer"></a><span data-ttu-id="738e1-196">新增種子初始設定式</span><span class="sxs-lookup"><span data-stu-id="738e1-196">Add the seed initializer</span></span>

<span data-ttu-id="738e1-197">在 *Program.cs* 中，修改 `Main` 方法來執行下列動作：</span><span class="sxs-lookup"><span data-stu-id="738e1-197">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="738e1-198">從相依性插入容器中取得資料庫內容執行個體。</span><span class="sxs-lookup"><span data-stu-id="738e1-198">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="738e1-199">呼叫種子方法，並將其傳遞給內容。</span><span class="sxs-lookup"><span data-stu-id="738e1-199">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="738e1-200">種子方法完成時處理內容。</span><span class="sxs-lookup"><span data-stu-id="738e1-200">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="738e1-201">下列程式碼顯示已更新的 *Program.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="738e1-201">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Program.cs)]

<span data-ttu-id="738e1-202">生產環境應用程式不會呼叫 `Database.Migrate`。</span><span class="sxs-lookup"><span data-stu-id="738e1-202">A production app would not call `Database.Migrate`.</span></span> <span data-ttu-id="738e1-203">它會新增至前面的程式碼以免在尚未執行 `Update-Database` 時發生下列例外狀況：</span><span class="sxs-lookup"><span data-stu-id="738e1-203">It's added to the preceding code to prevent the following exception when `Update-Database` has not been run:</span></span>

<span data-ttu-id="738e1-204">SqlException：無法開啟登入要求的 "RazorPagesMovieContext-21" 資料庫。</span><span class="sxs-lookup"><span data-stu-id="738e1-204">SqlException: Cannot open database "RazorPagesMovieContext-21" requested by the login.</span></span> <span data-ttu-id="738e1-205">登入失敗。</span><span class="sxs-lookup"><span data-stu-id="738e1-205">The login failed.</span></span>
<span data-ttu-id="738e1-206">使用者 'user name' 登入失敗。</span><span class="sxs-lookup"><span data-stu-id="738e1-206">Login failed for user 'user name'.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="738e1-207">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="738e1-207">Test the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="738e1-208">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="738e1-208">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="738e1-209">刪除資料庫中的所有記錄。</span><span class="sxs-lookup"><span data-stu-id="738e1-209">Delete all the records in the DB.</span></span> <span data-ttu-id="738e1-210">您可以使用瀏覽器或 [SSOX](xref:tutorials/razor-pages/new-field#ssox) 的刪除連結來執行這項操作</span><span class="sxs-lookup"><span data-stu-id="738e1-210">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="738e1-211">強制應用程式初始化 (呼叫 `Startup` 類別中的方法)，以執行植入方法。</span><span class="sxs-lookup"><span data-stu-id="738e1-211">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="738e1-212">若要強制初始化，IIS Express 必須停止並重新啟動。</span><span class="sxs-lookup"><span data-stu-id="738e1-212">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="738e1-213">您可以使用下列其中一個方法來執行此工作：</span><span class="sxs-lookup"><span data-stu-id="738e1-213">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="738e1-214">以滑鼠右鍵按一下通知區域中的 IIS Express 系統匣圖示，然後點選 [結束] 或 [停止站台]：</span><span class="sxs-lookup"><span data-stu-id="738e1-214">Right-click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![IIS Express 系統匣圖示](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![操作功能表](sql/_static/stopIIS.png)

    * <span data-ttu-id="738e1-217">如果您已在非偵錯模式中執行 VS，請按 F5 以偵錯模式執行。</span><span class="sxs-lookup"><span data-stu-id="738e1-217">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
    * <span data-ttu-id="738e1-218">如果您已在偵錯模式中執行 VS，請停止偵錯工具，然後按 F5。</span><span class="sxs-lookup"><span data-stu-id="738e1-218">If you were running VS in debug mode, stop the debugger and press F5.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="738e1-219">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="738e1-219">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="738e1-220">請刪除資料庫中的所有記錄 (這樣會執行 seed 方法)。</span><span class="sxs-lookup"><span data-stu-id="738e1-220">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="738e1-221">停止並啟動應用程式來植入資料庫。</span><span class="sxs-lookup"><span data-stu-id="738e1-221">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="738e1-222">應用程式會顯示植入的資料。</span><span class="sxs-lookup"><span data-stu-id="738e1-222">The app shows the seeded data.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="738e1-223">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="738e1-223">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="738e1-224">請刪除資料庫中的所有記錄 (這樣會執行 seed 方法)。</span><span class="sxs-lookup"><span data-stu-id="738e1-224">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="738e1-225">停止並啟動應用程式來植入資料庫。</span><span class="sxs-lookup"><span data-stu-id="738e1-225">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="738e1-226">應用程式會顯示植入的資料。</span><span class="sxs-lookup"><span data-stu-id="738e1-226">The app shows the seeded data.</span></span>

---

<span data-ttu-id="738e1-227">應用程式會顯示植入的資料：</span><span class="sxs-lookup"><span data-stu-id="738e1-227">The app shows the seeded data:</span></span>

![在 Chrome 中開啟的電影應用程式顯示電影資料](sql/_static/m55.png)

<span data-ttu-id="738e1-229">接下來的教學課程將會清除資料的呈現。</span><span class="sxs-lookup"><span data-stu-id="738e1-229">The next tutorial will clean up the presentation of the data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="738e1-230">其他資源</span><span class="sxs-lookup"><span data-stu-id="738e1-230">Additional resources</span></span>

* [<span data-ttu-id="738e1-231">這個教學課程的 YouTube 版本</span><span class="sxs-lookup"><span data-stu-id="738e1-231">YouTube version of this tutorial</span></span>](https://youtu.be/A_5ff11sDHY)

> [!div class="step-by-step"]
> <span data-ttu-id="738e1-232">[上一步：Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
> [下一步：更新頁面](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="738e1-232">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>

::: moniker-end
