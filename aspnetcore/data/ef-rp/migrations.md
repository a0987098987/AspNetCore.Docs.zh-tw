---
title: ASP.NET Core 中的 Razor 頁面與 EF Core - 移轉 - 4/8
author: rick-anderson
description: 在本教學課程中，您將開始使用 EF Core 移轉功能來管理 ASP.NET Core MVC 應用程式中的資料模型變更。
ms.author: riande
ms.date: 6/31/2017
uid: data/ef-rp/migrations
ms.openlocfilehash: 5b8228130378059aebe21c9c3ea1eb72e4c6aad9
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/06/2019
ms.locfileid: "65086162"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---migrations---4-of-8"></a>ASP.NET Core 中的 Razor 頁面與 EF Core - 移轉 - 4/8

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

作者：[Tom Dykstra](https://github.com/tdykstra)、[Jon P Smith](https://twitter.com/thereformedprog) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

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

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

開啟命令視窗並巡覽至專案資料夾。 專案資料夾中包含 *Startup.cs* 檔案。

在命令視窗中輸入下列命令：

 ```console
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

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```console
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

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```console
dotnet ef migrations remove
```

如需詳細資訊，請參閱 [dotnet ef migrations remove](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove)。

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
https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations)。

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

::: moniker-end

> [!div class="step-by-step"]
> [上一頁](xref:data/ef-rp/sort-filter-page)
> [下一頁](xref:data/ef-rp/complex-data-model)
