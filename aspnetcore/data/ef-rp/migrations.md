---
title: ASP.NET Core 中的 Razor 頁面與 EF Core - 移轉 - 4/8
author: tdykstra
description: 在本教學課程中，您將開始使用 EF Core 移轉功能來管理 ASP.NET Core MVC 應用程式中的資料模型變更。
ms.author: riande
ms.date: 07/22/2019
uid: data/ef-rp/migrations
ms.openlocfilehash: 8a4929a905c6a488231d7d29e1101f6fd887477f
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082074"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---migrations---4-of-8"></a>ASP.NET Core 中的 Razor 頁面與 EF Core - 移轉 - 4/8

作者：[Tom Dykstra](https://github.com/tdykstra)、[Jon P Smith](https://twitter.com/thereformedprog) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

::: moniker range=">= aspnetcore-3.0"

本教學課程介紹用於管理資料模型變更的 EF Core 移轉功能。

開發新的應用程式時，資料模型經常變更。 每次模型變更時，模型就與資料庫不同步。 本教學課程系列從設定 Entity Framework 來建立不存在的資料庫開始。 每次資料模型變更時，您都必須卸除資料庫。 下次執行應用程式時，對 `EnsureCreated` 的呼叫會重新建立資料庫，以符合新的資料模型。 然後，`DbInitializer` 類別會執行以植入新的資料庫。

在您將應用程式部署到生產環境之前，這種讓資料庫與資料模型保持同步的方法效果不錯。 當應用程式在生產環境中執行時，它通常會儲存需要維護的資料。 應用程式不能每次進行變更 (例如新增資料行) 時都從測試資料庫開始。 為了解決上述問題，EF Core 移轉功能可讓 EF Core 更新資料庫結構描述，而不是建立新的資料庫。

移轉會更新結構描述並保留現有的資料，而不是在資料模型變更時卸除並重新建立資料庫。

[!INCLUDE[](~/includes/sqlite-warn.md)]

## <a name="drop-the-database"></a>卸除資料庫

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

使用 **SQL Server 物件總管** (SSOX) 刪除資料庫，或在**套件管理員主控台** (PMC) 中執行下列命令：

```powershell
Drop-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* 請在命令提示字元執行下列命令以安裝 EF CLI 工具：

  ```dotnetcli
  dotnet tool install --global dotnet-ef --version 3.0.0-*
  ```

* 在命令提示字元中，巡覽至專案資料夾。 專案資料夾包含 *ContosoUniversity.csproj* 檔案。

* 刪除 *CU.db* 檔案，或執行下列命令：

  ```dotnetcli
  dotnet ef database drop --force
  ```

---

## <a name="create-an-initial-migration"></a>建立初始移轉

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

在 PMC 中執行下列命令：

```powershell
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

請確認命令提示字元位於專案資料夾中，然後執行下列命令：

```dotnetcli
dotnet ef migrations add InitialCreate
dotnet ef database update
```

---

## <a name="up-and-down-methods"></a>Up 和 Down 方法

EF Core `migrations add` 命令已產生用來建立資料庫的程式碼。 此移轉程式碼位於 Migrations\<時間戳記>_InitialCreate.cs 檔案中。 `InitialCreate` 類別的 `Up` 方法會建立對應至資料模型實體集的資料庫資料表。 `Down` 方法則會刪除它們，如下列範例所示：

[!code-csharp[](intro/samples/cu30/Migrations/20190731193522_InitialCreate.cs)]

上述程式碼適用於初始移轉。 程式碼：

* 由 `migrations add InitialCreate` 命令所產生。 
* 由 `database update` 命令執行。
* 會為資料庫內容類別所指定的資料模型建立資料庫。

移轉名稱參數 (在範例中為 "InitialCreate") 用於檔案名稱。 移轉名稱可以是任何有效的檔案名稱。 您最好選擇單字或片語來摘要列出移轉中所要完成的作業。 例如，新增了部門資料表的移轉可能稱為 "AddDepartmentTable"。

## <a name="the-migrations-history-table"></a>移轉記錄資料表

* 使用 SSOX 或您的 SQLite 工具來檢查資料庫。
* 請注意已新增 `__EFMigrationsHistory` 資料表。 `__EFMigrationsHistory` 資料表會追蹤哪些移轉已套用至資料庫。
* 檢視 `__EFMigrationsHistory` 資料表中的資料。 第一次移轉時會顯示一個資料列。

## <a name="the-data-model-snapshot"></a>資料模型快照集

移轉會在 *Migrations/SchoolContextModelSnapshot.cs* 中建立目前資料模型的「快照集」。 當您新增移轉時，EF 會比較目前資料模型與快照集檔案，以判斷變更的內容。

由於快照集檔案會追蹤資料模型的狀態，您無法藉由刪除 `<timestamp>_<migrationname>.cs` 檔案來刪除移轉。 若要退出最新的移轉，您必須使用 `migrations remove` 命令。 該命令會刪除移轉，並確保能正確地重設快照集。 如需詳細資訊，請參閱 [dotnet ef 移轉移除](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove)。

## <a name="remove-ensurecreated"></a>移除 EnsureCreated

本教學課程系列使用 `EnsureCreated` 來開始。 `EnsureCreated` 不會建立移轉記錄資料表，因此無法用於移轉。 其設計目的是用來測試或快速原型化經常卸除並重新建立資料庫的位置。

從這裡開始，教學課程會使用移轉。

在 *Data/dbinitializer.cs* 中，將下列程式程式碼標記為註解：

```csharp
context.Database.EnsureCreated();
```
執行應用程式，並確認資料庫已植入。

## <a name="applying-migrations-in-production"></a>在生產環境中套用移轉

建議在應用程式啟動時，生產環境應用程式**不應該**呼叫 [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_)。 不應該從部署至伺服器陣列的應用程式呼叫 `Migrate`。 如果應用程式相應放大至多個伺服器執行個體，則很難確保不從多部伺服器進行資料庫結構描述更新，或與讀取/寫入存取發生衝突。

資料庫移轉應該在部署中以受控制的方式完成。 生產環境資料庫移轉方法包括：

* 使用移轉來建立 SQL 指令碼，並在部署中使用 SQL 指令碼。
* 從受控制的環境中執行 `dotnet ef database update`。

## <a name="troubleshooting"></a>疑難排解

如果應用程式使用 SQL Server LocalDB 並顯示下列例外狀況：

```text
SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

可能的解決方案是在命令提示字元下執行 `dotnet ef database update`。

### <a name="additional-resources"></a>其他資源

* [EF Core CLI](/ef/core/miscellaneous/cli/dotnet)。
* [套件管理員主控台 (Visual Studio)](/ef/core/miscellaneous/cli/powershell)

## <a name="next-steps"></a>後續步驟

下一個教學課程會建立資料模型，並新增實體屬性和新實體。

> [!div class="step-by-step"]
> [上一個教學課程](xref:data/ef-rp/sort-filter-page)
> [下一個教學課程](xref:data/ef-rp/complex-data-model)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

在本教學課程中，會使用 EF Core 移轉功能來管理資料模型變更。

若您遇到無法解決的問題，請下載[完整應用程式](
https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)。

開發新的應用程式時，資料模型經常變更。 每次模型變更時，模型就與資料庫不同步。 本教學課程從設定 Entity Framework 來建立不存在的資料庫開始。 每次資料模型變更時：

* 會卸除資料庫。
* EF 會建立一個符合模型的新資料庫。
* 應用程式會將測試資料植入資料庫。

在您將應用程式部署到生產環境之前，這種讓資料庫與資料模型保持同步的方法效果不錯。 當應用程式在生產環境中執行時，它通常會儲存需要維護的資料。 應用程式不能每次進行變更 (例如新增資料行) 時，都從測試資料庫開始。 EF Core 移轉功能可解決這個問題，方法是啟用 EF Core 以更新資料庫結構描述，來代替建立新的資料庫。

移轉會更新結構描述，並保留現有的資料，而不是在資料模型變更時，卸除並重新建立資料庫。

## <a name="drop-the-database"></a>卸除資料庫

使用 [SQL Server 物件總管] (SSOX) 或 `database drop` 命令：

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

在 [套件管理員主控台] (PMC) 中，執行下列命令：

```PMC
Drop-Database
```

從 PMC 執行 `Get-Help about_EntityFrameworkCore` 以取得說明資訊。

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

開啟命令視窗並巡覽至專案資料夾。 專案資料夾中包含 *Startup.cs* 檔案。

在命令視窗中輸入下列命令：

 ```dotnetcli
 dotnet ef database drop
 ```

---

## <a name="create-an-initial-migration-and-update-the-db"></a>建立初始移轉並更新資料庫

建置專案並建立第一個移轉。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

```dotnetcli
dotnet ef migrations add InitialCreate
dotnet ef database update
```

---

### <a name="examine-the-up-and-down-methods"></a>檢查 Up 和 Down 方法

EF Core 命令 `migrations add` 已產生用來建立資料庫的程式碼。 此移轉程式碼位於 Migrations\<時間戳記>_InitialCreate.cs 檔案中。 `InitialCreate` 類別的 `Up` 方法會建立對應至資料模型實體集的資料庫資料表。 `Down` 方法則會刪除它們，如下列範例所示：

[!code-csharp[](intro/samples/cu21/Migrations/20180626224812_InitialCreate.cs?range=7-24,77-88)]

移轉會呼叫 `Up` 方法，以實作資料模型變更來進行移轉。 當您輸入命令以復原更新時，移轉會呼叫 `Down` 方法。

上述程式碼適用於初始移轉。 該程式碼是在執行 `migrations add InitialCreate` 命令時建立。 移轉名稱參數 (在範例中為 "InitialCreate") 用於檔案名稱。 移轉名稱可以是任何有效的檔案名稱。 您最好選擇單字或片語來摘要列出移轉中所要完成的作業。 例如，新增了部門資料表的移轉可能稱為 "AddDepartmentTable"。

如果已建立初始移轉並結束資料庫，則：

* 會產生資料庫建立程式碼。
* 不需要執行資料庫建立程式碼，因為資料庫已符合資料模型。 如果執行了資料庫建立程式碼，它不會進行任何變更，因為資料庫已符合資料模型。

當應用程式部署到新的環境時，必須執行資料庫建立程式碼來建立資料庫。

先前資料庫已卸除，並不存在，所以移轉會建立新資料庫。

### <a name="the-data-model-snapshot"></a>資料模型快照集

移轉會在 *Migrations/SchoolContextModelSnapshot.cs* 中建立目前資料庫結構描述的「快照」。 當您新增移轉時，EF 會比較資料模型與快照集檔案，以判斷變更的內容。

若要刪除移轉，請使用下列命令：

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Remove-Migration

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

```dotnetcli
dotnet ef migrations remove
```

如需詳細資訊，請參閱 [dotnet ef 移轉移除](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove)。

---

remove migrations 命令會刪除移轉，並確保正確地重設快照集。

### <a name="remove-ensurecreated-and-test-the-app"></a>移除 EnsureCreated 並測試應用程式

早期開發會使用 `EnsureCreated`。 在本教學課程中，則使用移轉。 `EnsureCreated` 具有下列限制：

* 略過移轉，並建立資料庫和結構描述。
* 不會建立移轉資料表。
* 「無法」與移轉搭配使用。
* 設計用來測試或快速原型化經常卸除並重新建立資料庫的位置。

移除 `EnsureCreated`：

```csharp
context.Database.EnsureCreated();
```

執行應用程式，並確認植入資料庫。

### <a name="inspect-the-database"></a>檢查資料庫

使用 **SQL Server 物件總管**來檢查資料庫。 請注意已新增 `__EFMigrationsHistory` 資料表。 `__EFMigrationsHistory` 資料表會追蹤哪些移轉經套用至資料庫。 檢視 `__EFMigrationsHistory` 資料表中的資料，它會顯示第一個移轉的某個資料列。 上述 CLI 輸出範例中的最後一則記錄會顯示建立此資料列的 INSERT 陳述式。

執行應用程式，並確認一切運作正常。

## <a name="applying-migrations-in-production"></a>在生產環境中套用移轉

建議在應用程式啟動時，生產環境應用程式**不**應該呼叫 [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_)。 `Migrate` 不應該從伺服器陣列中的應用程式進行呼叫。 例如，如果應用程式已使用向外延展 (執行應用程式的多個執行個體) 進行雲端部署。

資料庫移轉應該在部署中以受控制的方式完成。 生產環境資料庫移轉方法包括：

* 使用移轉來建立 SQL 指令碼，並在部署中使用 SQL 指令碼。
* 從受控制的環境中執行 `dotnet ef database update`。

EF Core 使用 `__MigrationsHistory` 資料表來查看是否有任何需要執行的移轉。 如果資料庫為最新狀態，則不會執行移轉。

## <a name="troubleshooting"></a>疑難排解

下載[完整應用程式](
https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu21snapshots/cu-part4-migrations)。

應用程式會產生下列例外狀況：

```text
SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

解決方案:執行 `dotnet ef database update`

### <a name="additional-resources"></a>其他資源

* [這個教學課程的 YouTube 版本](https://www.youtube.com/watch?v=OWSUuMLKTJo)
* [.NET Core CLI](/ef/core/miscellaneous/cli/dotnet)。
* [套件管理員主控台 (Visual Studio)](/ef/core/miscellaneous/cli/powershell)



> [!div class="step-by-step"]
> [上一頁](xref:data/ef-rp/sort-filter-page)
> [下一頁](xref:data/ef-rp/complex-data-model)

::: moniker-end

