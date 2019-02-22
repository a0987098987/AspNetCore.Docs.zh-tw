---
title: 使用資料庫和 ASP.NET Core
author: rick-anderson
description: 說明如何使用資料庫和 ASP.NET Core。
ms.author: riande
ms.date: 12/07/2017
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 3e05f5dbc73c35f1f938346b2eaab8c0fa7d8ab9
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410293"
---
# <a name="work-with-a-database-and-aspnet-core"></a>使用資料庫和 ASP.NET Core

作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Joe Audette](https://twitter.com/joeaudette)

[!INCLUDE[](~/includes/rp/download.md)]

`RazorPagesMovieContext` 物件會處理連線到資料庫和將 `Movie` 物件對應至資料庫記錄的工作。 在 *Startup.cs* 的 `ConfigureServices` 方法中，以[相依性插入](xref:fundamentals/dependency-injection)容器登錄資料庫內容：

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

---  
<!-- End of VS tabs -->

如需 `ConfigureServices` 中所用之方法的詳細資訊，請參閱：

* 適用於 `CookiePolicyOptions` 的 [ASP.NET Core 中的 EU 一般資料保護規定 (GDPR) 支援](xref:security/gdpr)。
* [SetCompatibilityVersion](xref:mvc/compatibility-version)

ASP.NET Core [組態](xref:fundamentals/configuration/index)系統會讀取 `ConnectionString`。 對於本機開發，它會從 *appsettings.json* 檔案取得連接字串。

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

您的產生程式碼會有不同的資料庫 (`Database={Database name}`) 名稱值。 名稱值為任意值。

[!code-json[](razor-pages-start/sample/RazorPagesMovie22/appsettings.json)]

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

---  
<!-- End of VS tabs -->

將應用程式部署到測試或生產環境伺服器時，可以使用環境變數來設定實際資料庫伺服器的連接字串。 如需詳細資訊，請參閱[組態](xref:fundamentals/configuration/index)。

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

LocalDB 為輕量版的 SQL Server Express 資料庫引擎，鎖定程式開發為其目標。 LocalDB 會依需求啟動，並以使用者模式執行，因此沒有複雜的組態。 LocalDB 資料庫預設會在 `C:/Users/<user/>` 目錄中建立 `*.mdf` 檔案。

<a name="ssox"></a>
* 從 [檢視] 功能表中，開啟 [SQL Server 物件總管] (SSOX)。

  ![檢視功能表](sql/_static/ssox.png)

* 以滑鼠右鍵按一下 `Movie` 資料表，並選取 [檢視表設計工具]：

  ![在電影資料表上開啟操作功能表](sql/_static/design.png)

  ![在設計工具中開啟電影資料表](sql/_static/dv.png)

請注意 `ID` 旁的索引鍵圖示。 根據預設，EF 會為主索引鍵建立名為 `ID` 的屬性。

* 以滑鼠右鍵按一下 `Movie` 資料表，並選取 [檢視資料]：

  ![開啟的電影資料表顯示資料表資料](sql/_static/vd22.png)
<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/rp/sqlite.md)]

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]

---  
<!-- End of VS tabs -->

## <a name="seed-the-database"></a>植入資料庫

使用下列程式碼，在 *Models* 資料夾中建立名為 `SeedData` 的新類別：

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/SeedData.cs?name=snippet_1)]

如果資料庫中有任何電影，則種子初始設定式會返回，而且不會新增任何電影。

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```
<a name="si"></a>
### <a name="add-the-seed-initializer"></a>新增種子初始設定式

在 *Program.cs* 中，修改 `Main` 方法來執行下列動作：

* 從相依性插入容器中取得資料庫內容執行個體。
* 呼叫種子方法，並將其傳遞給內容。
* 種子方法完成時處理內容。

下列程式碼顯示已更新的 *Program.cs* 檔案。

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Program.cs)]

生產環境應用程式不會呼叫 `Database.Migrate`。 它會新增至前面的程式碼以免在尚未執行 `Update-Database` 時發生下列例外狀況：

SqlException：無法開啟登入要求的 "RazorPagesMovieContext-21" 資料庫。 登入失敗。
使用者 'user name' 登入失敗。

### <a name="test-the-app"></a>測試應用程式

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 刪除資料庫中的所有記錄。 您可以使用瀏覽器或 [SSOX](xref:tutorials/razor-pages/new-field#ssox) 的刪除連結來執行這項操作
* 強制應用程式初始化 (呼叫 `Startup` 類別中的方法)，以執行植入方法。 若要強制初始化，IIS Express 必須停止並重新啟動。 您可以使用下列其中一個方法來執行此工作：

  * 以滑鼠右鍵按一下通知區域中的 IIS Express 系統匣圖示，然後點選 [結束] 或 [停止網站]：

    ![IIS Express 系統匣圖示](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![操作功能表](sql/_static/stopIIS.png)

    * 如果您已在非偵錯模式中執行 VS，請按 F5 以偵錯模式執行。
    * 如果您已在偵錯模式中執行 VS，請停止偵錯工具，然後按 F5。

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

請刪除資料庫中的所有記錄 (這樣會執行 seed 方法)。 停止並啟動應用程式來植入資料庫。

應用程式會顯示植入的資料。

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

請刪除資料庫中的所有記錄 (這樣會執行 seed 方法)。 停止並啟動應用程式來植入資料庫。

應用程式會顯示植入的資料。

---  
<!-- End of VS tabs -->


   
應用程式會顯示植入的資料：

![在 Chrome 中開啟的電影應用程式顯示電影資料](sql/_static/m55.png)

接下來的教學課程將會清除資料的呈現。

> [!div class="step-by-step"]
> [上一步：Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
> [下一步：更新頁面](xref:tutorials/razor-pages/da1)
