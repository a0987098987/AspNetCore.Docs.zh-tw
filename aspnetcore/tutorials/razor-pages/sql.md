---
title: "使用 SQL Server LocalDB 與 ASP.NET Core"
author: rick-anderson
description: "說明如何使用 SQL Server LocalDB 與 ASP.NET Core。"
keywords: "ASP.NET Core, Razor 頁面, Razor,MVC, SQL, LocalDB"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 42fa98886f3e87e79ea1ea4a2223a79319676006
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/28/2017
---
# <a name="working-with-sql-server-localdb-and-aspnet-core"></a>使用 SQL Server LocalDB 與 ASP.NET Core

作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Joe Audette](https://twitter.com/joeaudette) 

`MovieContext` 物件會處理連線到資料庫和將 `Movie` 物件對應至資料庫記錄的工作。 在 *Startup.cs* 檔案的 `ConfigureServices` 方法中，以[相依性插入](xref:fundamentals/dependency-injection)容器登錄資料庫內容：

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=6-7)]

ASP.NET Core [組態](xref:fundamentals/configuration)系統會讀取 `ConnectionString`。 對於本機開發，它會從 *appsettings.json* 檔案取得連接字串：

[!code-json[Main](razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=2&range=8-10)]

當您將應用程式部署到測試或生產環境伺服器時，可以使用環境變數或另一個方法來設定實際 SQL Server 的連接字串。 如需詳細資訊，請參閱[組態](xref:fundamentals/configuration)。

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

LocalDB 是輕量版的 SQL Server Express Database Engine，以程式開發為目標。 LocalDB 會視需要啟動，並以使用者模式執行，因此沒有複雜的組態。 根據預設，LocalDB 資料庫會在 *C:/Users/*\<使用者\> 目錄中建立 "\*.mdf" 檔案。

<a name="ssox"></a>
* 從 [檢視] 功能表中，開啟 [SQL Server 物件總管] (SSOX)。

  ![檢視功能表](sql/_static/ssox.png)

* 以滑鼠右鍵按一下 `Movie` 資料表，並選取 [檢視表設計工具]：

  ![在電影資料表上開啟操作功能表](sql/_static/design.png)

  ![在設計工具中開啟電影資料表](sql/_static/dv.png)

請注意 `ID` 旁的索引鍵圖示。 根據預設，EF 會為主索引鍵建立名為 `ID` 的屬性。

* 以滑鼠右鍵按一下 `Movie` 資料表，並選取 [檢視資料]：

  ![開啟的電影資料表顯示資料表資料](sql/_static/vd22.png)

## <a name="seed-the-database"></a>植入資料庫

在 *Models* 資料夾中建立名為 `SeedData` 的新類別。 使用下列程式碼取代產生的程式碼：

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/SeedData.cs?name=snippet_1)]

如果資料庫中有任何電影，則種子初始設定式會返回，而且不會新增任何電影。

```csharp
if (context.Movies.Any())
{
    return;   // DB has been seeded.
}
```
<a name="si"></a>
### <a name="add-the-seed-initializer"></a>新增種子初始設定式

在 *Program.cs* 檔案中，將種子初始設定式新增至 `Main` 方法的結尾處：

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Program.cs?highlight=6,17-32)]

測試應用程式

* 刪除資料庫中的所有記錄。 您可以使用瀏覽器或 [SSOX](xref:tutorials/razor-pages/new-field#ssox) 的刪除連結來執行這項操作
* 強制應用程式初始化 (呼叫 `Startup` 類別中的方法)，以執行植入方法。 若要強制初始化，IIS Express 必須停止並重新啟動。 您可以使用下列其中一個方法來執行此工作：

  * 以滑鼠右鍵按一下通知區域中的 IIS Express 系統匣圖示，然後點選 [結束] 或 [停止網站]：

    ![IIS Express 系統匣圖示](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![操作功能表](sql/_static/stopIIS.png)

   * 如果您已在非偵錯模式中執行 VS，請按 F5 以偵錯模式執行。
   * 如果您已在偵錯模式中執行 VS，請停止偵錯工具，然後按 F5。
   
應用程式會顯示植入的資料：

![在 Chrome 中開啟的電影應用程式顯示電影資料](sql/_static/m55.png)

接下來的教學課程將會清除資料的呈現。

>[!div class="step-by-step"]
[上一步：包含 Scaffold 的 Razor 頁面](xref:tutorials/razor-pages/page)
[下一步：更新頁面](xref:tutorials/razor-pages/da1)
