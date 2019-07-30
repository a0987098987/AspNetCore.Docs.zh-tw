---
title: 使用資料庫和 ASP.NET Core
author: rick-anderson
description: 說明如何使用資料庫和 ASP.NET Core。
ms.author: riande
ms.date: 7/22/2019
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 197697f28e9faa45c1ac2b7f993bde15994957e5
ms.sourcegitcommit: 051f068c78931432e030b60094c38376d64d013e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/24/2019
ms.locfileid: "68440380"
---
# <a name="work-with-a-database-and-aspnet-core"></a><span data-ttu-id="e4e8a-103">使用資料庫和 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e4e8a-103">Work with a database and ASP.NET Core</span></span>

<span data-ttu-id="e4e8a-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="e4e8a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="e4e8a-105">`RazorPagesMovieContext` 物件會處理連線到資料庫和將 `Movie` 物件對應至資料庫記錄的工作。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-105">The `RazorPagesMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="e4e8a-106">在 *Startup.cs* 的 `ConfigureServices` 方法中，以[相依性插入](xref:fundamentals/dependency-injection)容器登錄資料庫內容：</span><span class="sxs-lookup"><span data-stu-id="e4e8a-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in *Startup.cs*:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e4e8a-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4e8a-107">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e4e8a-108">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e4e8a-108">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

---

<span data-ttu-id="e4e8a-109">ASP.NET Core [組態](xref:fundamentals/configuration/index)系統會讀取 `ConnectionString`。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-109">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="e4e8a-110">對於本機開發，它會從 *appsettings.json* 檔案取得連接字串。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-110">For local development, it gets the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e4e8a-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4e8a-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e4e8a-112">您的產生程式碼會有不同的資料庫 (`Database={Database name}`) 名稱值。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-112">The name value for the database (`Database={Database name}`) will be different for your generated code.</span></span> <span data-ttu-id="e4e8a-113">名稱值為任意值。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-113">The name value is arbitrary.</span></span>

[!code-json[](razor-pages-start/sample/RazorPagesMovie30/appsettings.json)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e4e8a-114">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e4e8a-114">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

---

<span data-ttu-id="e4e8a-115">將應用程式部署到測試或生產環境伺服器時，可以使用環境變數來設定實際資料庫伺服器的連接字串。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-115">When the app is deployed to a test or production server, an environment variable can be used to set the connection string to a real database server.</span></span> <span data-ttu-id="e4e8a-116">如需詳細資訊，請參閱[組態](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-116">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e4e8a-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4e8a-117">Visual Studio</span></span>](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a><span data-ttu-id="e4e8a-118">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="e4e8a-118">SQL Server Express LocalDB</span></span>

<span data-ttu-id="e4e8a-119">LocalDB 為輕量版的 SQL Server Express 資料庫引擎，鎖定程式開發為其目標。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-119">LocalDB is a lightweight version of the SQL Server Express database engine that's targeted for program development.</span></span> <span data-ttu-id="e4e8a-120">LocalDB 會依需求啟動，並以使用者模式執行，因此沒有複雜的組態。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-120">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="e4e8a-121">LocalDB 資料庫預設會在 `C:/Users/<user/>` 目錄中建立 `*.mdf` 檔案。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-121">By default, LocalDB database creates `*.mdf` files in the `C:/Users/<user/>` directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="e4e8a-122">從 [檢視]  功能表中，開啟 [SQL Server 物件總管]  (SSOX)。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-122">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![檢視功能表](sql/_static/ssox.png)

* <span data-ttu-id="e4e8a-124">以滑鼠右鍵按一下 `Movie` 資料表，並選取 [檢視表設計工具]  ：</span><span class="sxs-lookup"><span data-stu-id="e4e8a-124">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![在 Movie 資料表上開啟的操作功能表](sql/_static/design.png)

  ![在設計工具中開啟的 Movie 資料表](sql/_static/dv.png)

<span data-ttu-id="e4e8a-127">請注意 `ID` 旁的索引鍵圖示。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-127">Note the key icon next to `ID`.</span></span> <span data-ttu-id="e4e8a-128">根據預設，EF 會為主索引鍵建立名為 `ID` 的屬性。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-128">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="e4e8a-129">以滑鼠右鍵按一下 `Movie` 資料表，並選取 [檢視資料]  ：</span><span class="sxs-lookup"><span data-stu-id="e4e8a-129">Right click on the `Movie` table and select **View Data**:</span></span>

  ![開啟的電影資料表顯示資料表資料](sql/_static/vd22.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e4e8a-131">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e4e8a-131">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

---

## <a name="seed-the-database"></a><span data-ttu-id="e4e8a-132">植入資料庫</span><span class="sxs-lookup"><span data-stu-id="e4e8a-132">Seed the database</span></span>

<span data-ttu-id="e4e8a-133">使用下列程式碼，在 *Models* 資料夾中建立名為 `SeedData` 的新類別：</span><span class="sxs-lookup"><span data-stu-id="e4e8a-133">Create a new class named `SeedData` in the *Models* folder with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="e4e8a-134">如果資料庫中有任何電影，則種子初始設定式會返回，而且不會新增任何電影。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-134">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>

### <a name="add-the-seed-initializer"></a><span data-ttu-id="e4e8a-135">新增種子初始設定式</span><span class="sxs-lookup"><span data-stu-id="e4e8a-135">Add the seed initializer</span></span>

<span data-ttu-id="e4e8a-136">在 *Program.cs* 中，修改 `Main` 方法來執行下列動作：</span><span class="sxs-lookup"><span data-stu-id="e4e8a-136">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="e4e8a-137">從相依性插入容器中取得資料庫內容執行個體。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-137">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="e4e8a-138">呼叫種子方法，並將其傳遞給內容。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-138">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="e4e8a-139">種子方法完成時處理內容。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-139">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="e4e8a-140">下列程式碼顯示已更新的 *Program.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-140">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Program.cs)]

<span data-ttu-id="e4e8a-141">生產環境應用程式不會呼叫 `Database.Migrate`。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-141">A production app would not call `Database.Migrate`.</span></span> <span data-ttu-id="e4e8a-142">它會新增至前面的程式碼以免在尚未執行 `Update-Database` 時發生下列例外狀況：</span><span class="sxs-lookup"><span data-stu-id="e4e8a-142">It's added to the preceding code to prevent the following exception when `Update-Database` has not been run:</span></span>

<span data-ttu-id="e4e8a-143">SqlException：無法開啟登入要求的 "RazorPagesMovieContext-21" 資料庫。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-143">SqlException: Cannot open database "RazorPagesMovieContext-21" requested by the login.</span></span> <span data-ttu-id="e4e8a-144">登入失敗。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-144">The login failed.</span></span>
<span data-ttu-id="e4e8a-145">使用者 'user name' 登入失敗。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-145">Login failed for user 'user name'.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="e4e8a-146">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="e4e8a-146">Test the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e4e8a-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4e8a-147">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e4e8a-148">刪除資料庫中的所有記錄。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-148">Delete all the records in the DB.</span></span> <span data-ttu-id="e4e8a-149">您可以使用瀏覽器或 [SSOX](xref:tutorials/razor-pages/new-field#ssox) 的刪除連結來執行這項操作</span><span class="sxs-lookup"><span data-stu-id="e4e8a-149">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="e4e8a-150">強制應用程式初始化 (呼叫 `Startup` 類別中的方法)，以執行植入方法。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-150">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="e4e8a-151">若要強制初始化，IIS Express 必須停止並重新啟動。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-151">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="e4e8a-152">您可以使用下列其中一個方法來執行此工作：</span><span class="sxs-lookup"><span data-stu-id="e4e8a-152">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="e4e8a-153">以滑鼠右鍵按一下通知區域中的 IIS Express 系統匣圖示，然後點選 [結束]  或 [停止網站]  ：</span><span class="sxs-lookup"><span data-stu-id="e4e8a-153">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![IIS Express 系統匣圖示](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![操作功能表](sql/_static/stopIIS.png)

    * <span data-ttu-id="e4e8a-156">如果您已在非偵錯模式中執行 VS，請按 F5 以偵錯模式執行。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-156">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
    * <span data-ttu-id="e4e8a-157">如果您已在偵錯模式中執行 VS，請停止偵錯工具，然後按 F5。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-157">If you were running VS in debug mode, stop the debugger and press F5.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e4e8a-158">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e4e8a-158">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="e4e8a-159">請刪除資料庫中的所有記錄 (這樣會執行 seed 方法)。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-159">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="e4e8a-160">停止並啟動應用程式來植入資料庫。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-160">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="e4e8a-161">應用程式會顯示植入的資料。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-161">The app shows the seeded data.</span></span>

---

<span data-ttu-id="e4e8a-162">接下來的教學課程將會改善資料的呈現。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-162">The next tutorial will improve the presentation of the data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e4e8a-163">其他資源</span><span class="sxs-lookup"><span data-stu-id="e4e8a-163">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e4e8a-164">[上一步：Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
> [下一步：更新頁面](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="e4e8a-164">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="e4e8a-165">`RazorPagesMovieContext` 物件會處理連線到資料庫和將 `Movie` 物件對應至資料庫記錄的工作。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-165">The `RazorPagesMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="e4e8a-166">在 *Startup.cs* 的 `ConfigureServices` 方法中，以[相依性插入](xref:fundamentals/dependency-injection)容器登錄資料庫內容：</span><span class="sxs-lookup"><span data-stu-id="e4e8a-166">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in *Startup.cs*:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e4e8a-167">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4e8a-167">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e4e8a-168">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e4e8a-168">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

---

<span data-ttu-id="e4e8a-169">如需 `ConfigureServices` 中所用之方法的詳細資訊，請參閱：</span><span class="sxs-lookup"><span data-stu-id="e4e8a-169">For more information on the methods used in `ConfigureServices`, see:</span></span>

* <span data-ttu-id="e4e8a-170">適用於 `CookiePolicyOptions` 的 [ASP.NET Core 中的 EU 一般資料保護規定 (GDPR) 支援](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-170">[EU General Data Protection Regulation (GDPR) support in ASP.NET Core](xref:security/gdpr) for `CookiePolicyOptions`.</span></span>
* [<span data-ttu-id="e4e8a-171">SetCompatibilityVersion</span><span class="sxs-lookup"><span data-stu-id="e4e8a-171">SetCompatibilityVersion</span></span>](xref:mvc/compatibility-version)

<span data-ttu-id="e4e8a-172">ASP.NET Core [組態](xref:fundamentals/configuration/index)系統會讀取 `ConnectionString`。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-172">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="e4e8a-173">對於本機開發，它會從 *appsettings.json* 檔案取得連接字串。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-173">For local development, it gets the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e4e8a-174">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4e8a-174">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e4e8a-175">您的產生程式碼會有不同的資料庫 (`Database={Database name}`) 名稱值。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-175">The name value for the database (`Database={Database name}`) will be different for your generated code.</span></span> <span data-ttu-id="e4e8a-176">名稱值為任意值。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-176">The name value is arbitrary.</span></span>

[!code-json[](razor-pages-start/sample/RazorPagesMovie22/appsettings.json)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e4e8a-177">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e4e8a-177">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e4e8a-178">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e4e8a-178">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

---

<span data-ttu-id="e4e8a-179">將應用程式部署到測試或生產環境伺服器時，可以使用環境變數來設定實際資料庫伺服器的連接字串。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-179">When the app is deployed to a test or production server, an environment variable can be used to set the connection string to a real database server.</span></span> <span data-ttu-id="e4e8a-180">如需詳細資訊，請參閱[組態](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-180">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e4e8a-181">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4e8a-181">Visual Studio</span></span>](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a><span data-ttu-id="e4e8a-182">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="e4e8a-182">SQL Server Express LocalDB</span></span>

<span data-ttu-id="e4e8a-183">LocalDB 為輕量版的 SQL Server Express 資料庫引擎，鎖定程式開發為其目標。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-183">LocalDB is a lightweight version of the SQL Server Express database engine that's targeted for program development.</span></span> <span data-ttu-id="e4e8a-184">LocalDB 會依需求啟動，並以使用者模式執行，因此沒有複雜的組態。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-184">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="e4e8a-185">LocalDB 資料庫預設會在 `C:/Users/<user/>` 目錄中建立 `*.mdf` 檔案。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-185">By default, LocalDB database creates `*.mdf` files in the `C:/Users/<user/>` directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="e4e8a-186">從 [檢視]  功能表中，開啟 [SQL Server 物件總管]  (SSOX)。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-186">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![檢視功能表](sql/_static/ssox.png)

* <span data-ttu-id="e4e8a-188">以滑鼠右鍵按一下 `Movie` 資料表，並選取 [檢視表設計工具]  ：</span><span class="sxs-lookup"><span data-stu-id="e4e8a-188">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![在電影資料表上開啟操作功能表](sql/_static/design.png)

  ![在設計工具中開啟電影資料表](sql/_static/dv.png)

<span data-ttu-id="e4e8a-191">請注意 `ID` 旁的索引鍵圖示。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-191">Note the key icon next to `ID`.</span></span> <span data-ttu-id="e4e8a-192">根據預設，EF 會為主索引鍵建立名為 `ID` 的屬性。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-192">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="e4e8a-193">以滑鼠右鍵按一下 `Movie` 資料表，並選取 [檢視資料]  ：</span><span class="sxs-lookup"><span data-stu-id="e4e8a-193">Right click on the `Movie` table and select **View Data**:</span></span>

  ![開啟的電影資料表顯示資料表資料](sql/_static/vd22.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e4e8a-195">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e4e8a-195">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e4e8a-196">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e4e8a-196">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

---

## <a name="seed-the-database"></a><span data-ttu-id="e4e8a-197">植入資料庫</span><span class="sxs-lookup"><span data-stu-id="e4e8a-197">Seed the database</span></span>

<span data-ttu-id="e4e8a-198">使用下列程式碼，在 *Models* 資料夾中建立名為 `SeedData` 的新類別：</span><span class="sxs-lookup"><span data-stu-id="e4e8a-198">Create a new class named `SeedData` in the *Models* folder with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="e4e8a-199">如果資料庫中有任何電影，則種子初始設定式會返回，而且不會新增任何電影。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-199">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>

### <a name="add-the-seed-initializer"></a><span data-ttu-id="e4e8a-200">新增種子初始設定式</span><span class="sxs-lookup"><span data-stu-id="e4e8a-200">Add the seed initializer</span></span>

<span data-ttu-id="e4e8a-201">在 *Program.cs* 中，修改 `Main` 方法來執行下列動作：</span><span class="sxs-lookup"><span data-stu-id="e4e8a-201">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="e4e8a-202">從相依性插入容器中取得資料庫內容執行個體。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-202">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="e4e8a-203">呼叫種子方法，並將其傳遞給內容。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-203">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="e4e8a-204">種子方法完成時處理內容。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-204">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="e4e8a-205">下列程式碼顯示已更新的 *Program.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-205">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Program.cs)]

<span data-ttu-id="e4e8a-206">生產環境應用程式不會呼叫 `Database.Migrate`。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-206">A production app would not call `Database.Migrate`.</span></span> <span data-ttu-id="e4e8a-207">它會新增至前面的程式碼以免在尚未執行 `Update-Database` 時發生下列例外狀況：</span><span class="sxs-lookup"><span data-stu-id="e4e8a-207">It's added to the preceding code to prevent the following exception when `Update-Database` has not been run:</span></span>

<span data-ttu-id="e4e8a-208">SqlException：無法開啟登入要求的 "RazorPagesMovieContext-21" 資料庫。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-208">SqlException: Cannot open database "RazorPagesMovieContext-21" requested by the login.</span></span> <span data-ttu-id="e4e8a-209">登入失敗。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-209">The login failed.</span></span>
<span data-ttu-id="e4e8a-210">使用者 'user name' 登入失敗。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-210">Login failed for user 'user name'.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="e4e8a-211">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="e4e8a-211">Test the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e4e8a-212">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4e8a-212">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e4e8a-213">刪除資料庫中的所有記錄。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-213">Delete all the records in the DB.</span></span> <span data-ttu-id="e4e8a-214">您可以使用瀏覽器或 [SSOX](xref:tutorials/razor-pages/new-field#ssox) 的刪除連結來執行這項操作</span><span class="sxs-lookup"><span data-stu-id="e4e8a-214">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="e4e8a-215">強制應用程式初始化 (呼叫 `Startup` 類別中的方法)，以執行植入方法。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-215">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="e4e8a-216">若要強制初始化，IIS Express 必須停止並重新啟動。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-216">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="e4e8a-217">您可以使用下列其中一個方法來執行此工作：</span><span class="sxs-lookup"><span data-stu-id="e4e8a-217">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="e4e8a-218">以滑鼠右鍵按一下通知區域中的 IIS Express 系統匣圖示，然後點選 [結束]  或 [停止站台]  ：</span><span class="sxs-lookup"><span data-stu-id="e4e8a-218">Right-click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![IIS Express 系統匣圖示](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![操作功能表](sql/_static/stopIIS.png)

    * <span data-ttu-id="e4e8a-221">如果您已在非偵錯模式中執行 VS，請按 F5 以偵錯模式執行。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-221">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
    * <span data-ttu-id="e4e8a-222">如果您已在偵錯模式中執行 VS，請停止偵錯工具，然後按 F5。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-222">If you were running VS in debug mode, stop the debugger and press F5.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e4e8a-223">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e4e8a-223">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="e4e8a-224">請刪除資料庫中的所有記錄 (這樣會執行 seed 方法)。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-224">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="e4e8a-225">停止並啟動應用程式來植入資料庫。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-225">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="e4e8a-226">應用程式會顯示植入的資料。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-226">The app shows the seeded data.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e4e8a-227">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e4e8a-227">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="e4e8a-228">請刪除資料庫中的所有記錄 (這樣會執行 seed 方法)。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-228">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="e4e8a-229">停止並啟動應用程式來植入資料庫。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-229">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="e4e8a-230">應用程式會顯示植入的資料。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-230">The app shows the seeded data.</span></span>

---

<span data-ttu-id="e4e8a-231">應用程式會顯示植入的資料：</span><span class="sxs-lookup"><span data-stu-id="e4e8a-231">The app shows the seeded data:</span></span>

![在 Chrome 中開啟的電影應用程式顯示電影資料](sql/_static/m55.png)

<span data-ttu-id="e4e8a-233">接下來的教學課程將會清除資料的呈現。</span><span class="sxs-lookup"><span data-stu-id="e4e8a-233">The next tutorial will clean up the presentation of the data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e4e8a-234">其他資源</span><span class="sxs-lookup"><span data-stu-id="e4e8a-234">Additional resources</span></span>

* [<span data-ttu-id="e4e8a-235">這個教學課程的 YouTube 版本</span><span class="sxs-lookup"><span data-stu-id="e4e8a-235">YouTube version of this tutorial</span></span>](https://youtu.be/A_5ff11sDHY)

> [!div class="step-by-step"]
> <span data-ttu-id="e4e8a-236">[上一步：Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
> [下一步：更新頁面](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="e4e8a-236">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>

::: moniker-end