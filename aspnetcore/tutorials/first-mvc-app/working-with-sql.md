---
title: 在 ASP.NET Core MVC 應用程式中使用 SQL
author: rick-anderson
description: 了解如何在 ASP.NET Core MVC 應用程式中使用 SQL Server LocalDB 或 SQLite。
ms.author: riande
ms.date: 8/16/2019
uid: tutorials/first-mvc-app/working-with-sql
ms.openlocfilehash: de392f4220cf0182d02a20f387164d2f4b184b58
ms.sourcegitcommit: 020c3760492efed71b19e476f25392dda5dd7388
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/12/2019
ms.locfileid: "72289089"
---
# <a name="work-with-sql-in-aspnet-core"></a>在 ASP.NET Core 中使用 SQL

::: moniker range=">= aspnetcore-3.0"

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

`MvcMovieContext` 物件會處理連線到資料庫和將 `Movie` 物件對應至資料庫記錄的工作。 在 *Startup.cs* 檔案的 `ConfigureServices` 方法中，以[相依性插入](xref:fundamentals/dependency-injection)容器登錄資料庫內容：

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

ASP.NET Core [組態](xref:fundamentals/configuration/index)系統會讀取 `ConnectionString`。 對於本機開發，它會從 *appsettings.json* 檔案取得連接字串：

[!code-json[](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_UseSqlite&highlight=5-6)]

ASP.NET Core [組態](xref:fundamentals/configuration/index)系統會讀取 `ConnectionString`。 對於本機開發，它會從 *appsettings.json* 檔案取得連接字串：

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/appsettingsSQLite.json?highlight=2&range=8-10)]

---

將應用程式部署到測試或實際執行環境的伺服器時，可以使用環境變數來設定實際執行環境 SQL Server 的連接字串。 如需詳細資訊，請參閱[組態](xref:fundamentals/configuration/index)。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

LocalDB 為輕量版的 SQL Server Express Database Engine，鎖定程式開發為其目標。 LocalDB 會依需求啟動，並以使用者模式執行，因此沒有複雜的組態。 根據預設，LocalDB 資料庫會在 *C:/Users/{user}* 目錄中建立 *.mdf* 檔案。

* 從 [檢視] 功能表中，開啟 [SQL Server 物件總管] (SSOX)。

  ![檢視功能表](working-with-sql/_static/ssox.png)

* 以滑鼠右鍵按一下 `Movie` 資料表 > [檢視表設計工具]

  ![在電影資料表上開啟操作功能表](working-with-sql/_static/design.png)

  ![在設計工具中開啟電影資料表](working-with-sql/_static/dv.png)

請注意 `ID` 旁的索引鍵圖示。 根據預設，EF 會將名為 `ID` 的屬性設為主索引鍵。

* 以滑鼠右鍵按一下 `Movie` 資料表 > [檢視資料]

  ![在電影資料表上開啟操作功能表](working-with-sql/_static/ssox2.png)

  ![開啟的電影資料表顯示資料表資料](working-with-sql/_static/vd22.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

---
<!-- End of VS tabs -->

## <a name="seed-the-database"></a>植入資料庫

在 *Models* 資料夾中建立名為 `SeedData` 的新類別。 使用下列程式碼取代產生的程式碼：

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Models/SeedData.cs?name=snippet_1)]

如果資料庫中有任何電影，則種子初始設定式會返回，而且不會新增任何電影。

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>

### <a name="add-the-seed-initializer"></a>新增種子初始設定式

以下列程式碼取代 *Program.cs* 的內容：

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Program.cs)]

測試應用程式

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 刪除資料庫中的所有記錄。 您可以使用瀏覽器或 SSOX 的刪除連結來執行這項操作。
* 強制應用程式初始化 (呼叫 `Startup` 類別中的方法)，以執行植入方法。 若要強制初始化，IIS Express 必須停止並重新啟動。 您可以使用下列其中一個方法來執行此工作：

  * 以滑鼠右鍵按一下通知區域中的 IIS Express 系統匣圖示，然後點選 [結束] 或 [停止網站]

    ![IIS Express 系統匣圖示](working-with-sql/_static/iisExIcon.png)

    ![操作功能表](working-with-sql/_static/stopIIS.png)

    * 如果您已在非偵錯模式中執行 VS，請按 F5 以偵錯模式執行。
    * 如果您已在偵錯模式中執行 VS，請停止偵錯工具，然後按 F5。

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

請刪除資料庫中的所有記錄 (這樣會執行 seed 方法)。 停止並啟動應用程式來植入資料庫。

---

應用程式會顯示植入的資料。

![在 Microsoft Edge 中開啟 MVC 電影應用程式顯示電影資料](working-with-sql/_static/m55.png)

> [!div class="step-by-step"]
> [上一頁](adding-model.md)
> [下一頁](controller-methods-views.md)
::: moniker-end

::: moniker range="< aspnetcore-3.0"

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

`MvcMovieContext` 物件會處理連線到資料庫和將 `Movie` 物件對應至資料庫記錄的工作。 在 *Startup.cs* 檔案的 `ConfigureServices` 方法中，以[相依性插入](xref:fundamentals/dependency-injection)容器登錄資料庫內容：

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=13-99)]

ASP.NET Core [組態](xref:fundamentals/configuration/index)系統會讀取 `ConnectionString`。 對於本機開發，它會從 *appsettings.json* 檔案取得連接字串：

[!code-json[](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

ASP.NET Core [組態](xref:fundamentals/configuration/index)系統會讀取 `ConnectionString`。 對於本機開發，它會從 *appsettings.json* 檔案取得連接字串：

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/appsettingsSQLite.json?highlight=2&range=8-10)]

---

當您將應用程式部署到測試或生產環境伺服器時，可以使用環境變數或另一個方法來設定實際 SQL Server 的連接字串。 如需詳細資訊，請參閱[組態](xref:fundamentals/configuration/index)。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

LocalDB 為輕量版的 SQL Server Express Database Engine，鎖定程式開發為其目標。 LocalDB 會依需求啟動，並以使用者模式執行，因此沒有複雜的組態。 根據預設，LocalDB 資料庫會在 *C:/Users/{user}* 目錄中建立 *.mdf* 檔案。

* 從 [檢視] 功能表中，開啟 [SQL Server 物件總管] (SSOX)。

  ![檢視功能表](working-with-sql/_static/ssox.png)

* 以滑鼠右鍵按一下 `Movie` 資料表 > [檢視表設計工具]

  ![在電影資料表上開啟操作功能表](working-with-sql/_static/design.png)

  ![在設計工具中開啟電影資料表](working-with-sql/_static/dv.png)

請注意 `ID` 旁的索引鍵圖示。 根據預設，EF 會將名為 `ID` 的屬性設為主索引鍵。

* 以滑鼠右鍵按一下 `Movie` 資料表 > [檢視資料]

  ![在電影資料表上開啟操作功能表](working-with-sql/_static/ssox2.png)

  ![開啟的電影資料表顯示資料表資料](working-with-sql/_static/vd22.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

---
<!-- End of VS tabs -->

## <a name="seed-the-database"></a>植入資料庫

在 *Models* 資料夾中建立名為 `SeedData` 的新類別。 使用下列程式碼取代產生的程式碼：

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Models/SeedData.cs?name=snippet_1)]

如果資料庫中有任何電影，則種子初始設定式會返回，而且不會新增任何電影。

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>

### <a name="add-the-seed-initializer"></a>新增種子初始設定式

以下列程式碼取代 *Program.cs* 的內容：

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Program.cs)]

測試應用程式

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 刪除資料庫中的所有記錄。 您可以使用瀏覽器或 SSOX 的刪除連結來執行這項操作。
* 強制應用程式初始化 (呼叫 `Startup` 類別中的方法)，以執行植入方法。 若要強制初始化，IIS Express 必須停止並重新啟動。 您可以使用下列其中一個方法來執行此工作：

  * 以滑鼠右鍵按一下通知區域中的 IIS Express 系統匣圖示，然後點選 [結束] 或 [停止網站]

    ![IIS Express 系統匣圖示](working-with-sql/_static/iisExIcon.png)

    ![操作功能表](working-with-sql/_static/stopIIS.png)

    * 如果您已在非偵錯模式中執行 VS，請按 F5 以偵錯模式執行。
    * 如果您已在偵錯模式中執行 VS，請停止偵錯工具，然後按 F5。

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

請刪除資料庫中的所有記錄 (這樣會執行 seed 方法)。 停止並啟動應用程式來植入資料庫。

---

應用程式會顯示植入的資料。

![在 Microsoft Edge 中開啟 MVC 電影應用程式顯示電影資料](working-with-sql/_static/m55.png)

> [!div class="step-by-step"]
> [上一頁](adding-model.md)
> [下一頁](controller-methods-views.md)

::: moniker-end
