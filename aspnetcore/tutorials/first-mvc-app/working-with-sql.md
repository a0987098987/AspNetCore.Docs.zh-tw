---
title: 在 ASP.NET Core MVC 應用程式中使用 SQL
author: rick-anderson
description: 了解如何在 ASP.NET Core MVC 應用程式中使用 SQL Server LocalDB 或 SQLite。
ms.author: riande
ms.date: 8/16/2019
uid: tutorials/first-mvc-app/working-with-sql
ms.openlocfilehash: d2784d9edc32b79e67dbcd193be55b44bc8d2c49
ms.sourcegitcommit: 7a46973998623aead757ad386fe33602b1658793
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/15/2019
ms.locfileid: "69487635"
---
# <a name="work-with-sql-in-aspnet-core"></a><span data-ttu-id="f72bc-103">在 ASP.NET Core 中使用 SQL</span><span class="sxs-lookup"><span data-stu-id="f72bc-103">Work with SQL in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="f72bc-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f72bc-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f72bc-105">`MvcMovieContext` 物件會處理連線到資料庫和將 `Movie` 物件對應至資料庫記錄的工作。</span><span class="sxs-lookup"><span data-stu-id="f72bc-105">The `MvcMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="f72bc-106">在 *Startup.cs* 檔案的 `ConfigureServices` 方法中，以[相依性插入](xref:fundamentals/dependency-injection)容器登錄資料庫內容：</span><span class="sxs-lookup"><span data-stu-id="f72bc-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f72bc-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f72bc-107">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_ConfigureServices&highlight=6-7)]

<span data-ttu-id="f72bc-108">ASP.NET Core [組態](xref:fundamentals/configuration/index)系統會讀取 `ConnectionString`。</span><span class="sxs-lookup"><span data-stu-id="f72bc-108">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="f72bc-109">對於本機開發，它會從 *appsettings.json* 檔案取得連接字串：</span><span class="sxs-lookup"><span data-stu-id="f72bc-109">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="f72bc-110">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f72bc-110">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_UseSqlite&highlight=6-7)]

<span data-ttu-id="f72bc-111">ASP.NET Core [組態](xref:fundamentals/configuration/index)系統會讀取 `ConnectionString`。</span><span class="sxs-lookup"><span data-stu-id="f72bc-111">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="f72bc-112">對於本機開發，它會從 *appsettings.json* 檔案取得連接字串：</span><span class="sxs-lookup"><span data-stu-id="f72bc-112">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/appsettingsSQLite.json?highlight=2&range=8-10)]

---

<span data-ttu-id="f72bc-113">將應用程式部署到測試或實際執行環境的伺服器時，可以使用環境變數來設定實際執行環境 SQL Server 的連接字串。</span><span class="sxs-lookup"><span data-stu-id="f72bc-113">When the app is deployed to a test or production server, an environment variable can be used to set the connection string to a production SQL Server.</span></span> <span data-ttu-id="f72bc-114">如需詳細資訊，請參閱[組態](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="f72bc-114">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f72bc-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f72bc-115">Visual Studio</span></span>](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a><span data-ttu-id="f72bc-116">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="f72bc-116">SQL Server Express LocalDB</span></span>

<span data-ttu-id="f72bc-117">LocalDB 為輕量版的 SQL Server Express Database Engine，鎖定程式開發為其目標。</span><span class="sxs-lookup"><span data-stu-id="f72bc-117">LocalDB is a lightweight version of the SQL Server Express Database Engine that's targeted for program development.</span></span> <span data-ttu-id="f72bc-118">LocalDB 會依需求啟動，並以使用者模式執行，因此沒有複雜的組態。</span><span class="sxs-lookup"><span data-stu-id="f72bc-118">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="f72bc-119">根據預設，LocalDB 資料庫會在 *C:/Users/{user}* 目錄中建立 *.mdf* 檔案。</span><span class="sxs-lookup"><span data-stu-id="f72bc-119">By default, LocalDB database creates *.mdf* files in the *C:/Users/{user}* directory.</span></span>

* <span data-ttu-id="f72bc-120">從 [檢視]  功能表中，開啟 [SQL Server 物件總管]  (SSOX)。</span><span class="sxs-lookup"><span data-stu-id="f72bc-120">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![檢視功能表](working-with-sql/_static/ssox.png)

* <span data-ttu-id="f72bc-122">以滑鼠右鍵按一下 `Movie` 資料表 > [檢視表設計工具] </span><span class="sxs-lookup"><span data-stu-id="f72bc-122">Right click on the `Movie` table **> View Designer**</span></span>

  ![在電影資料表上開啟操作功能表](working-with-sql/_static/design.png)

  ![在設計工具中開啟電影資料表](working-with-sql/_static/dv.png)

<span data-ttu-id="f72bc-125">請注意 `ID` 旁的索引鍵圖示。</span><span class="sxs-lookup"><span data-stu-id="f72bc-125">Note the key icon next to `ID`.</span></span> <span data-ttu-id="f72bc-126">根據預設，EF 會將名為 `ID` 的屬性設為主索引鍵。</span><span class="sxs-lookup"><span data-stu-id="f72bc-126">By default, EF will make a property named `ID` the primary key.</span></span>

* <span data-ttu-id="f72bc-127">以滑鼠右鍵按一下 `Movie` 資料表 > [檢視資料] </span><span class="sxs-lookup"><span data-stu-id="f72bc-127">Right click on the `Movie` table **> View Data**</span></span>

  ![在電影資料表上開啟操作功能表](working-with-sql/_static/ssox2.png)

  ![開啟的電影資料表顯示資料表資料](working-with-sql/_static/vd22.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="f72bc-130">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f72bc-130">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

---
<!-- End of VS tabs -->

## <a name="seed-the-database"></a><span data-ttu-id="f72bc-131">植入資料庫</span><span class="sxs-lookup"><span data-stu-id="f72bc-131">Seed the database</span></span>

<span data-ttu-id="f72bc-132">在 *Models* 資料夾中建立名為 `SeedData` 的新類別。</span><span class="sxs-lookup"><span data-stu-id="f72bc-132">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="f72bc-133">使用下列程式碼取代產生的程式碼：</span><span class="sxs-lookup"><span data-stu-id="f72bc-133">Replace the generated code with the following:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="f72bc-134">如果資料庫中有任何電影，則種子初始設定式會返回，而且不會新增任何電影。</span><span class="sxs-lookup"><span data-stu-id="f72bc-134">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>

### <a name="add-the-seed-initializer"></a><span data-ttu-id="f72bc-135">新增種子初始設定式</span><span class="sxs-lookup"><span data-stu-id="f72bc-135">Add the seed initializer</span></span>

<span data-ttu-id="f72bc-136">以下列程式碼取代 *Program.cs* 的內容：</span><span class="sxs-lookup"><span data-stu-id="f72bc-136">Replace the contents of *Program.cs* with the following code:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Program.cs)]

<span data-ttu-id="f72bc-137">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="f72bc-137">Test the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f72bc-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f72bc-138">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f72bc-139">刪除資料庫中的所有記錄。</span><span class="sxs-lookup"><span data-stu-id="f72bc-139">Delete all the records in the DB.</span></span> <span data-ttu-id="f72bc-140">您可以使用瀏覽器或 SSOX 的刪除連結來執行這項操作。</span><span class="sxs-lookup"><span data-stu-id="f72bc-140">You can do this with the delete links in the browser or from SSOX.</span></span>
* <span data-ttu-id="f72bc-141">強制應用程式初始化 (呼叫 `Startup` 類別中的方法)，以執行植入方法。</span><span class="sxs-lookup"><span data-stu-id="f72bc-141">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="f72bc-142">若要強制初始化，IIS Express 必須停止並重新啟動。</span><span class="sxs-lookup"><span data-stu-id="f72bc-142">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="f72bc-143">您可以使用下列其中一個方法來執行此工作：</span><span class="sxs-lookup"><span data-stu-id="f72bc-143">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="f72bc-144">以滑鼠右鍵按一下通知區域中的 IIS Express 系統匣圖示，然後點選 [結束]  或 [停止網站] </span><span class="sxs-lookup"><span data-stu-id="f72bc-144">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**</span></span>

    ![IIS Express 系統匣圖示](working-with-sql/_static/iisExIcon.png)

    ![操作功能表](working-with-sql/_static/stopIIS.png)

    * <span data-ttu-id="f72bc-147">如果您已在非偵錯模式中執行 VS，請按 F5 以偵錯模式執行。</span><span class="sxs-lookup"><span data-stu-id="f72bc-147">If you were running VS in non-debug mode, press F5 to run in debug mode</span></span>
    * <span data-ttu-id="f72bc-148">如果您已在偵錯模式中執行 VS，請停止偵錯工具，然後按 F5。</span><span class="sxs-lookup"><span data-stu-id="f72bc-148">If you were running VS in debug mode, stop the debugger and press F5</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="f72bc-149">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f72bc-149">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="f72bc-150">請刪除資料庫中的所有記錄 (這樣會執行 seed 方法)。</span><span class="sxs-lookup"><span data-stu-id="f72bc-150">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="f72bc-151">停止並啟動應用程式來植入資料庫。</span><span class="sxs-lookup"><span data-stu-id="f72bc-151">Stop and start the app to seed the database.</span></span>

---

<span data-ttu-id="f72bc-152">應用程式會顯示植入的資料。</span><span class="sxs-lookup"><span data-stu-id="f72bc-152">The app shows the seeded data.</span></span>

![在 Microsoft Edge 中開啟 MVC 電影應用程式顯示電影資料](working-with-sql/_static/m55.png)

> [!div class="step-by-step"]
> <span data-ttu-id="f72bc-154">[上一頁](adding-model.md)
> [下一頁](controller-methods-views.md)</span><span class="sxs-lookup"><span data-stu-id="f72bc-154">[Previous](adding-model.md)
[Next](controller-methods-views.md)</span></span>
::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="f72bc-155">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f72bc-155">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f72bc-156">`MvcMovieContext` 物件會處理連線到資料庫和將 `Movie` 物件對應至資料庫記錄的工作。</span><span class="sxs-lookup"><span data-stu-id="f72bc-156">The `MvcMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="f72bc-157">在 *Startup.cs* 檔案的 `ConfigureServices` 方法中，以[相依性插入](xref:fundamentals/dependency-injection)容器登錄資料庫內容：</span><span class="sxs-lookup"><span data-stu-id="f72bc-157">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f72bc-158">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f72bc-158">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=13-99)]

<span data-ttu-id="f72bc-159">ASP.NET Core [組態](xref:fundamentals/configuration/index)系統會讀取 `ConnectionString`。</span><span class="sxs-lookup"><span data-stu-id="f72bc-159">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="f72bc-160">對於本機開發，它會從 *appsettings.json* 檔案取得連接字串：</span><span class="sxs-lookup"><span data-stu-id="f72bc-160">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="f72bc-161">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f72bc-161">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<span data-ttu-id="f72bc-162">ASP.NET Core [組態](xref:fundamentals/configuration/index)系統會讀取 `ConnectionString`。</span><span class="sxs-lookup"><span data-stu-id="f72bc-162">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="f72bc-163">對於本機開發，它會從 *appsettings.json* 檔案取得連接字串：</span><span class="sxs-lookup"><span data-stu-id="f72bc-163">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/appsettingsSQLite.json?highlight=2&range=8-10)]

---

<span data-ttu-id="f72bc-164">當您將應用程式部署到測試或生產環境伺服器時，可以使用環境變數或另一個方法來設定實際 SQL Server 的連接字串。</span><span class="sxs-lookup"><span data-stu-id="f72bc-164">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="f72bc-165">如需詳細資訊，請參閱[組態](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="f72bc-165">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f72bc-166">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f72bc-166">Visual Studio</span></span>](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a><span data-ttu-id="f72bc-167">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="f72bc-167">SQL Server Express LocalDB</span></span>

<span data-ttu-id="f72bc-168">LocalDB 為輕量版的 SQL Server Express Database Engine，鎖定程式開發為其目標。</span><span class="sxs-lookup"><span data-stu-id="f72bc-168">LocalDB is a lightweight version of the SQL Server Express Database Engine that's targeted for program development.</span></span> <span data-ttu-id="f72bc-169">LocalDB 會依需求啟動，並以使用者模式執行，因此沒有複雜的組態。</span><span class="sxs-lookup"><span data-stu-id="f72bc-169">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="f72bc-170">根據預設，LocalDB 資料庫會在 *C:/Users/{user}* 目錄中建立 *.mdf* 檔案。</span><span class="sxs-lookup"><span data-stu-id="f72bc-170">By default, LocalDB database creates *.mdf* files in the *C:/Users/{user}* directory.</span></span>

* <span data-ttu-id="f72bc-171">從 [檢視]  功能表中，開啟 [SQL Server 物件總管]  (SSOX)。</span><span class="sxs-lookup"><span data-stu-id="f72bc-171">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![檢視功能表](working-with-sql/_static/ssox.png)

* <span data-ttu-id="f72bc-173">以滑鼠右鍵按一下 `Movie` 資料表 > [檢視表設計工具] </span><span class="sxs-lookup"><span data-stu-id="f72bc-173">Right click on the `Movie` table **> View Designer**</span></span>

  ![在電影資料表上開啟操作功能表](working-with-sql/_static/design.png)

  ![在設計工具中開啟電影資料表](working-with-sql/_static/dv.png)

<span data-ttu-id="f72bc-176">請注意 `ID` 旁的索引鍵圖示。</span><span class="sxs-lookup"><span data-stu-id="f72bc-176">Note the key icon next to `ID`.</span></span> <span data-ttu-id="f72bc-177">根據預設，EF 會將名為 `ID` 的屬性設為主索引鍵。</span><span class="sxs-lookup"><span data-stu-id="f72bc-177">By default, EF will make a property named `ID` the primary key.</span></span>

* <span data-ttu-id="f72bc-178">以滑鼠右鍵按一下 `Movie` 資料表 > [檢視資料] </span><span class="sxs-lookup"><span data-stu-id="f72bc-178">Right click on the `Movie` table **> View Data**</span></span>

  ![在電影資料表上開啟操作功能表](working-with-sql/_static/ssox2.png)

  ![開啟的電影資料表顯示資料表資料](working-with-sql/_static/vd22.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="f72bc-181">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f72bc-181">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

---
<!-- End of VS tabs -->

## <a name="seed-the-database"></a><span data-ttu-id="f72bc-182">植入資料庫</span><span class="sxs-lookup"><span data-stu-id="f72bc-182">Seed the database</span></span>

<span data-ttu-id="f72bc-183">在 *Models* 資料夾中建立名為 `SeedData` 的新類別。</span><span class="sxs-lookup"><span data-stu-id="f72bc-183">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="f72bc-184">使用下列程式碼取代產生的程式碼：</span><span class="sxs-lookup"><span data-stu-id="f72bc-184">Replace the generated code with the following:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="f72bc-185">如果資料庫中有任何電影，則種子初始設定式會返回，而且不會新增任何電影。</span><span class="sxs-lookup"><span data-stu-id="f72bc-185">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>

### <a name="add-the-seed-initializer"></a><span data-ttu-id="f72bc-186">新增種子初始設定式</span><span class="sxs-lookup"><span data-stu-id="f72bc-186">Add the seed initializer</span></span>

<span data-ttu-id="f72bc-187">以下列程式碼取代 *Program.cs* 的內容：</span><span class="sxs-lookup"><span data-stu-id="f72bc-187">Replace the contents of *Program.cs* with the following code:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Program.cs)]

<span data-ttu-id="f72bc-188">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="f72bc-188">Test the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f72bc-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f72bc-189">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f72bc-190">刪除資料庫中的所有記錄。</span><span class="sxs-lookup"><span data-stu-id="f72bc-190">Delete all the records in the DB.</span></span> <span data-ttu-id="f72bc-191">您可以使用瀏覽器或 SSOX 的刪除連結來執行這項操作。</span><span class="sxs-lookup"><span data-stu-id="f72bc-191">You can do this with the delete links in the browser or from SSOX.</span></span>
* <span data-ttu-id="f72bc-192">強制應用程式初始化 (呼叫 `Startup` 類別中的方法)，以執行植入方法。</span><span class="sxs-lookup"><span data-stu-id="f72bc-192">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="f72bc-193">若要強制初始化，IIS Express 必須停止並重新啟動。</span><span class="sxs-lookup"><span data-stu-id="f72bc-193">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="f72bc-194">您可以使用下列其中一個方法來執行此工作：</span><span class="sxs-lookup"><span data-stu-id="f72bc-194">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="f72bc-195">以滑鼠右鍵按一下通知區域中的 IIS Express 系統匣圖示，然後點選 [結束]  或 [停止網站] </span><span class="sxs-lookup"><span data-stu-id="f72bc-195">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**</span></span>

    ![IIS Express 系統匣圖示](working-with-sql/_static/iisExIcon.png)

    ![操作功能表](working-with-sql/_static/stopIIS.png)

    * <span data-ttu-id="f72bc-198">如果您已在非偵錯模式中執行 VS，請按 F5 以偵錯模式執行。</span><span class="sxs-lookup"><span data-stu-id="f72bc-198">If you were running VS in non-debug mode, press F5 to run in debug mode</span></span>
    * <span data-ttu-id="f72bc-199">如果您已在偵錯模式中執行 VS，請停止偵錯工具，然後按 F5。</span><span class="sxs-lookup"><span data-stu-id="f72bc-199">If you were running VS in debug mode, stop the debugger and press F5</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="f72bc-200">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f72bc-200">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="f72bc-201">請刪除資料庫中的所有記錄 (這樣會執行 seed 方法)。</span><span class="sxs-lookup"><span data-stu-id="f72bc-201">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="f72bc-202">停止並啟動應用程式來植入資料庫。</span><span class="sxs-lookup"><span data-stu-id="f72bc-202">Stop and start the app to seed the database.</span></span>

---

<span data-ttu-id="f72bc-203">應用程式會顯示植入的資料。</span><span class="sxs-lookup"><span data-stu-id="f72bc-203">The app shows the seeded data.</span></span>

![在 Microsoft Edge 中開啟 MVC 電影應用程式顯示電影資料](working-with-sql/_static/m55.png)

> [!div class="step-by-step"]
> <span data-ttu-id="f72bc-205">[上一頁](adding-model.md)
> [下一頁](controller-methods-views.md)</span><span class="sxs-lookup"><span data-stu-id="f72bc-205">[Previous](adding-model.md)
[Next](controller-methods-views.md)</span></span>

::: moniker-end