---
title: 在 ASP.NET Core MVC 應用程式中使用 SQL
author: rick-anderson
description: 了解如何在 ASP.NET Core MVC 應用程式中使用 SQL Server LocalDB 或 SQLite。
ms.author: riande
ms.date: 8/16/2019
uid: tutorials/first-mvc-app/working-with-sql
ms.openlocfilehash: d556f07111fb2022a1c2f1a066459566e302835d
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78665036"
---
# <a name="work-with-sql-in-aspnet-core"></a><span data-ttu-id="c5f87-103">在 ASP.NET Core 中使用 SQL</span><span class="sxs-lookup"><span data-stu-id="c5f87-103">Work with SQL in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c5f87-104">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供</span><span class="sxs-lookup"><span data-stu-id="c5f87-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c5f87-105">`MvcMovieContext` 物件會處理連線到資料庫和將 `Movie` 物件對應至資料庫記錄的工作。</span><span class="sxs-lookup"><span data-stu-id="c5f87-105">The `MvcMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="c5f87-106">在 [Startup.cs](xref:fundamentals/dependency-injection) 檔案的 `ConfigureServices` 方法中，以*相依性插入*容器登錄資料庫內容：</span><span class="sxs-lookup"><span data-stu-id="c5f87-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="c5f87-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c5f87-107">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

<span data-ttu-id="c5f87-108">ASP.NET Core [組態](xref:fundamentals/configuration/index)系統會讀取 `ConnectionString`。</span><span class="sxs-lookup"><span data-stu-id="c5f87-108">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="c5f87-109">對於本機開發，它會從 *appsettings.json* 檔案取得連接字串：</span><span class="sxs-lookup"><span data-stu-id="c5f87-109">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="c5f87-110">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="c5f87-110">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_UseSqlite&highlight=5-6)]

<span data-ttu-id="c5f87-111">ASP.NET Core [組態](xref:fundamentals/configuration/index)系統會讀取 `ConnectionString`。</span><span class="sxs-lookup"><span data-stu-id="c5f87-111">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="c5f87-112">對於本機開發，它會從 *appsettings.json* 檔案取得連接字串：</span><span class="sxs-lookup"><span data-stu-id="c5f87-112">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/appsettingsSQLite.json?highlight=2&range=8-10)]

---

<span data-ttu-id="c5f87-113">將應用程式部署到測試或實際執行環境的伺服器時，可以使用環境變數來設定實際執行環境 SQL Server 的連接字串。</span><span class="sxs-lookup"><span data-stu-id="c5f87-113">When the app is deployed to a test or production server, an environment variable can be used to set the connection string to a production SQL Server.</span></span> <span data-ttu-id="c5f87-114">如需詳細資訊，請參閱[組態](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="c5f87-114">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="c5f87-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c5f87-115">Visual Studio</span></span>](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a><span data-ttu-id="c5f87-116">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="c5f87-116">SQL Server Express LocalDB</span></span>

<span data-ttu-id="c5f87-117">LocalDB 為輕量版的 SQL Server Express Database Engine，鎖定程式開發為其目標。</span><span class="sxs-lookup"><span data-stu-id="c5f87-117">LocalDB is a lightweight version of the SQL Server Express Database Engine that's targeted for program development.</span></span> <span data-ttu-id="c5f87-118">LocalDB 會依需求啟動，並以使用者模式執行，因此沒有複雜的組態。</span><span class="sxs-lookup"><span data-stu-id="c5f87-118">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="c5f87-119">根據預設，LocalDB 資料庫會在 *C:/Users/{user}* 目錄中建立 *.mdf* 檔案。</span><span class="sxs-lookup"><span data-stu-id="c5f87-119">By default, LocalDB database creates *.mdf* files in the *C:/Users/{user}* directory.</span></span>

* <span data-ttu-id="c5f87-120">從 [檢視] 功能表中，開啟 [SQL Server 物件總管] (SSOX)。</span><span class="sxs-lookup"><span data-stu-id="c5f87-120">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![檢視功能表](working-with-sql/_static/ssox.png)

* <span data-ttu-id="c5f87-122">以滑鼠右鍵按一下 `Movie` 資料表 > [檢視表設計工具]</span><span class="sxs-lookup"><span data-stu-id="c5f87-122">Right click on the `Movie` table **> View Designer**</span></span>

  ![在電影資料表上開啟操作功能表](working-with-sql/_static/design.png)

  ![在設計工具中開啟電影資料表](working-with-sql/_static/dv.png)

<span data-ttu-id="c5f87-125">請注意 `ID` 旁的索引鍵圖示。</span><span class="sxs-lookup"><span data-stu-id="c5f87-125">Note the key icon next to `ID`.</span></span> <span data-ttu-id="c5f87-126">根據預設，EF 會將名為 `ID` 的屬性設為主索引鍵。</span><span class="sxs-lookup"><span data-stu-id="c5f87-126">By default, EF will make a property named `ID` the primary key.</span></span>

* <span data-ttu-id="c5f87-127">以滑鼠右鍵按一下 `Movie` 資料表 > [檢視資料]</span><span class="sxs-lookup"><span data-stu-id="c5f87-127">Right click on the `Movie` table **> View Data**</span></span>

  ![在電影資料表上開啟操作功能表](working-with-sql/_static/ssox2.png)

  ![開啟的電影資料表顯示資料表資料](working-with-sql/_static/vd22.png)

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="c5f87-130">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="c5f87-130">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

---
<!-- End of VS tabs -->

## <a name="seed-the-database"></a><span data-ttu-id="c5f87-131">植入資料庫</span><span class="sxs-lookup"><span data-stu-id="c5f87-131">Seed the database</span></span>

<span data-ttu-id="c5f87-132">在 `SeedData`Models*資料夾中建立名為* 的新類別。</span><span class="sxs-lookup"><span data-stu-id="c5f87-132">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="c5f87-133">使用下列程式碼取代產生的程式碼：</span><span class="sxs-lookup"><span data-stu-id="c5f87-133">Replace the generated code with the following:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="c5f87-134">如果資料庫中有任何電影，則種子初始設定式會返回，而且不會新增任何電影。</span><span class="sxs-lookup"><span data-stu-id="c5f87-134">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>

### <a name="add-the-seed-initializer"></a><span data-ttu-id="c5f87-135">新增種子初始設定式</span><span class="sxs-lookup"><span data-stu-id="c5f87-135">Add the seed initializer</span></span>

<span data-ttu-id="c5f87-136">以下列程式碼取代 *Program.cs* 的內容：</span><span class="sxs-lookup"><span data-stu-id="c5f87-136">Replace the contents of *Program.cs* with the following code:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Program.cs)]

<span data-ttu-id="c5f87-137">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="c5f87-137">Test the app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="c5f87-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c5f87-138">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c5f87-139">刪除資料庫中的所有記錄。</span><span class="sxs-lookup"><span data-stu-id="c5f87-139">Delete all the records in the DB.</span></span> <span data-ttu-id="c5f87-140">您可以使用瀏覽器或 SSOX 的刪除連結來執行這項操作。</span><span class="sxs-lookup"><span data-stu-id="c5f87-140">You can do this with the delete links in the browser or from SSOX.</span></span>
* <span data-ttu-id="c5f87-141">強制應用程式初始化 (呼叫 `Startup` 類別中的方法)，以執行植入方法。</span><span class="sxs-lookup"><span data-stu-id="c5f87-141">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="c5f87-142">若要強制初始化，IIS Express 必須停止並重新啟動。</span><span class="sxs-lookup"><span data-stu-id="c5f87-142">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="c5f87-143">您可以使用下列其中一個方法來執行此工作：</span><span class="sxs-lookup"><span data-stu-id="c5f87-143">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="c5f87-144">以滑鼠右鍵按一下通知區域中的 IIS Express 系統匣圖示，然後點選 [結束] 或 [停止網站]</span><span class="sxs-lookup"><span data-stu-id="c5f87-144">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**</span></span>

    ![IIS Express 系統匣圖示](working-with-sql/_static/iisExIcon.png)

    ![操作功能表](working-with-sql/_static/stopIIS.png)

    * <span data-ttu-id="c5f87-147">如果您已在非偵錯模式中執行 VS，請按 F5 以偵錯模式執行。</span><span class="sxs-lookup"><span data-stu-id="c5f87-147">If you were running VS in non-debug mode, press F5 to run in debug mode</span></span>
    * <span data-ttu-id="c5f87-148">如果您已在偵錯模式中執行 VS，請停止偵錯工具，然後按 F5。</span><span class="sxs-lookup"><span data-stu-id="c5f87-148">If you were running VS in debug mode, stop the debugger and press F5</span></span>

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="c5f87-149">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="c5f87-149">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="c5f87-150">請刪除資料庫中的所有記錄 (這樣會執行 seed 方法)。</span><span class="sxs-lookup"><span data-stu-id="c5f87-150">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="c5f87-151">停止並啟動應用程式來植入資料庫。</span><span class="sxs-lookup"><span data-stu-id="c5f87-151">Stop and start the app to seed the database.</span></span>

---

<span data-ttu-id="c5f87-152">應用程式會顯示植入的資料。</span><span class="sxs-lookup"><span data-stu-id="c5f87-152">The app shows the seeded data.</span></span>

![在 Microsoft Edge 中開啟 MVC 電影應用程式顯示電影資料](working-with-sql/_static/m55.png)

> [!div class="step-by-step"]
> <span data-ttu-id="c5f87-154">[上一頁](adding-model.md)
> [下一頁](controller-methods-views.md)</span><span class="sxs-lookup"><span data-stu-id="c5f87-154">[Previous](adding-model.md)
[Next](controller-methods-views.md)</span></span>
::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="c5f87-155">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供</span><span class="sxs-lookup"><span data-stu-id="c5f87-155">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c5f87-156">`MvcMovieContext` 物件會處理連線到資料庫和將 `Movie` 物件對應至資料庫記錄的工作。</span><span class="sxs-lookup"><span data-stu-id="c5f87-156">The `MvcMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="c5f87-157">在 [Startup.cs](xref:fundamentals/dependency-injection) 檔案的 `ConfigureServices` 方法中，以*相依性插入*容器登錄資料庫內容：</span><span class="sxs-lookup"><span data-stu-id="c5f87-157">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="c5f87-158">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c5f87-158">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=13-99)]

<span data-ttu-id="c5f87-159">ASP.NET Core [組態](xref:fundamentals/configuration/index)系統會讀取 `ConnectionString`。</span><span class="sxs-lookup"><span data-stu-id="c5f87-159">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="c5f87-160">對於本機開發，它會從 *appsettings.json* 檔案取得連接字串：</span><span class="sxs-lookup"><span data-stu-id="c5f87-160">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="c5f87-161">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="c5f87-161">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<span data-ttu-id="c5f87-162">ASP.NET Core [組態](xref:fundamentals/configuration/index)系統會讀取 `ConnectionString`。</span><span class="sxs-lookup"><span data-stu-id="c5f87-162">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="c5f87-163">對於本機開發，它會從 *appsettings.json* 檔案取得連接字串：</span><span class="sxs-lookup"><span data-stu-id="c5f87-163">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/appsettingsSQLite.json?highlight=2&range=8-10)]

---

<span data-ttu-id="c5f87-164">當您將應用程式部署到測試或生產環境伺服器時，可以使用環境變數或另一個方法來設定實際 SQL Server 的連接字串。</span><span class="sxs-lookup"><span data-stu-id="c5f87-164">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="c5f87-165">如需詳細資訊，請參閱[組態](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="c5f87-165">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="c5f87-166">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c5f87-166">Visual Studio</span></span>](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a><span data-ttu-id="c5f87-167">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="c5f87-167">SQL Server Express LocalDB</span></span>

<span data-ttu-id="c5f87-168">LocalDB 為輕量版的 SQL Server Express Database Engine，鎖定程式開發為其目標。</span><span class="sxs-lookup"><span data-stu-id="c5f87-168">LocalDB is a lightweight version of the SQL Server Express Database Engine that's targeted for program development.</span></span> <span data-ttu-id="c5f87-169">LocalDB 會依需求啟動，並以使用者模式執行，因此沒有複雜的組態。</span><span class="sxs-lookup"><span data-stu-id="c5f87-169">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="c5f87-170">根據預設，LocalDB 資料庫會在 *C:/Users/{user}* 目錄中建立 *.mdf* 檔案。</span><span class="sxs-lookup"><span data-stu-id="c5f87-170">By default, LocalDB database creates *.mdf* files in the *C:/Users/{user}* directory.</span></span>

* <span data-ttu-id="c5f87-171">從 [檢視] 功能表中，開啟 [SQL Server 物件總管] (SSOX)。</span><span class="sxs-lookup"><span data-stu-id="c5f87-171">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![檢視功能表](working-with-sql/_static/ssox.png)

* <span data-ttu-id="c5f87-173">以滑鼠右鍵按一下 `Movie` 資料表 > [檢視表設計工具]</span><span class="sxs-lookup"><span data-stu-id="c5f87-173">Right click on the `Movie` table **> View Designer**</span></span>

  ![在電影資料表上開啟操作功能表](working-with-sql/_static/design.png)

  ![在設計工具中開啟電影資料表](working-with-sql/_static/dv.png)

<span data-ttu-id="c5f87-176">請注意 `ID` 旁的索引鍵圖示。</span><span class="sxs-lookup"><span data-stu-id="c5f87-176">Note the key icon next to `ID`.</span></span> <span data-ttu-id="c5f87-177">根據預設，EF 會將名為 `ID` 的屬性設為主索引鍵。</span><span class="sxs-lookup"><span data-stu-id="c5f87-177">By default, EF will make a property named `ID` the primary key.</span></span>

* <span data-ttu-id="c5f87-178">以滑鼠右鍵按一下 `Movie` 資料表 > [檢視資料]</span><span class="sxs-lookup"><span data-stu-id="c5f87-178">Right click on the `Movie` table **> View Data**</span></span>

  ![在電影資料表上開啟操作功能表](working-with-sql/_static/ssox2.png)

  ![開啟的電影資料表顯示資料表資料](working-with-sql/_static/vd22.png)

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="c5f87-181">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="c5f87-181">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

---
<!-- End of VS tabs -->

## <a name="seed-the-database"></a><span data-ttu-id="c5f87-182">植入資料庫</span><span class="sxs-lookup"><span data-stu-id="c5f87-182">Seed the database</span></span>

<span data-ttu-id="c5f87-183">在 `SeedData`Models*資料夾中建立名為* 的新類別。</span><span class="sxs-lookup"><span data-stu-id="c5f87-183">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="c5f87-184">使用下列程式碼取代產生的程式碼：</span><span class="sxs-lookup"><span data-stu-id="c5f87-184">Replace the generated code with the following:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="c5f87-185">如果資料庫中有任何電影，則種子初始設定式會返回，而且不會新增任何電影。</span><span class="sxs-lookup"><span data-stu-id="c5f87-185">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>

### <a name="add-the-seed-initializer"></a><span data-ttu-id="c5f87-186">新增種子初始設定式</span><span class="sxs-lookup"><span data-stu-id="c5f87-186">Add the seed initializer</span></span>

<span data-ttu-id="c5f87-187">以下列程式碼取代 *Program.cs* 的內容：</span><span class="sxs-lookup"><span data-stu-id="c5f87-187">Replace the contents of *Program.cs* with the following code:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Program.cs)]

<span data-ttu-id="c5f87-188">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="c5f87-188">Test the app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="c5f87-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c5f87-189">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c5f87-190">刪除資料庫中的所有記錄。</span><span class="sxs-lookup"><span data-stu-id="c5f87-190">Delete all the records in the DB.</span></span> <span data-ttu-id="c5f87-191">您可以使用瀏覽器或 SSOX 的刪除連結來執行這項操作。</span><span class="sxs-lookup"><span data-stu-id="c5f87-191">You can do this with the delete links in the browser or from SSOX.</span></span>
* <span data-ttu-id="c5f87-192">強制應用程式初始化 (呼叫 `Startup` 類別中的方法)，以執行植入方法。</span><span class="sxs-lookup"><span data-stu-id="c5f87-192">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="c5f87-193">若要強制初始化，IIS Express 必須停止並重新啟動。</span><span class="sxs-lookup"><span data-stu-id="c5f87-193">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="c5f87-194">您可以使用下列其中一個方法來執行此工作：</span><span class="sxs-lookup"><span data-stu-id="c5f87-194">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="c5f87-195">以滑鼠右鍵按一下通知區域中的 IIS Express 系統匣圖示，然後點選 [結束] 或 [停止網站]</span><span class="sxs-lookup"><span data-stu-id="c5f87-195">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**</span></span>

    ![IIS Express 系統匣圖示](working-with-sql/_static/iisExIcon.png)

    ![操作功能表](working-with-sql/_static/stopIIS.png)

    * <span data-ttu-id="c5f87-198">如果您已在非偵錯模式中執行 VS，請按 F5 以偵錯模式執行。</span><span class="sxs-lookup"><span data-stu-id="c5f87-198">If you were running VS in non-debug mode, press F5 to run in debug mode</span></span>
    * <span data-ttu-id="c5f87-199">如果您已在偵錯模式中執行 VS，請停止偵錯工具，然後按 F5。</span><span class="sxs-lookup"><span data-stu-id="c5f87-199">If you were running VS in debug mode, stop the debugger and press F5</span></span>

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="c5f87-200">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="c5f87-200">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="c5f87-201">請刪除資料庫中的所有記錄 (這樣會執行 seed 方法)。</span><span class="sxs-lookup"><span data-stu-id="c5f87-201">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="c5f87-202">停止並啟動應用程式來植入資料庫。</span><span class="sxs-lookup"><span data-stu-id="c5f87-202">Stop and start the app to seed the database.</span></span>

---

<span data-ttu-id="c5f87-203">應用程式會顯示植入的資料。</span><span class="sxs-lookup"><span data-stu-id="c5f87-203">The app shows the seeded data.</span></span>

![在 Microsoft Edge 中開啟 MVC 電影應用程式顯示電影資料](working-with-sql/_static/m55_mac.png)

> [!div class="step-by-step"]
> <span data-ttu-id="c5f87-205">[上一頁](adding-model.md)
> [下一頁](controller-methods-views.md)</span><span class="sxs-lookup"><span data-stu-id="c5f87-205">[Previous](adding-model.md)
[Next](controller-methods-views.md)</span></span>

::: moniker-end
