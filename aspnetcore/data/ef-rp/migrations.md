---
title: ASP.NET Core 中的 Razor 頁面與 EF Core - 移轉 - 4/8
author: rick-anderson
description: 在本教學課程中，您將開始使用 EF Core 移轉功能來管理 ASP.NET Core MVC 應用程式中的資料模型變更。
manager: wpickett
ms.author: riande
ms.date: 10/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/migrations
ms.openlocfilehash: 690beaabeab098cf9b764730b1bf1bd04bf6b003
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/03/2018
ms.locfileid: "32740071"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---migrations---4-of-8"></a>ASP.NET Core 中的 Razor 頁面與 EF Core - 移轉 - 4/8

作者：[Tom Dykstra](https://github.com/tdykstra)、[Jon P Smith](https://twitter.com/thereformedprog) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

在本教學課程中，會使用 EF Core 移轉功能來管理資料模型變更。

如果您遇到無法解決的問題，請下載[此階段已完成的應用程式](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations)。

開發新的應用程式時，資料模型經常變更。 每次模型變更時，模型就與資料庫不同步。 本教學課程從設定 Entity Framework 來建立不存在的資料庫開始。 每次資料模型變更時：

* 會卸除資料庫。
* EF 會建立一個符合模型的新資料庫。
* 應用程式會將測試資料植入資料庫。

在您將應用程式部署到生產環境之前，這種讓資料庫與資料模型保持同步的方法效果不錯。 當應用程式在生產環境中執行時，它通常會儲存需要維護的資料。 應用程式不能每次進行變更 (例如新增資料行) 時，都從測試資料庫開始。 EF Core 移轉功能可解決這個問題，方法是啟用 EF Core 以更新資料庫結構描述，來代替建立新的資料庫。

移轉會更新結構描述，並保留現有的資料，而不是在資料模型變更時，卸除並重新建立資料庫。

## <a name="entity-framework-core-nuget-packages-for-migrations"></a>用於移轉的 Entity Framework Core NuGet 套件

若要使用移轉，請使用 [套件管理員主控台] (PMC) 或命令列介面 (CLI)。 這些教學課程會示範如何使用 CLI 命令。 PMC 的資訊位於[本教學課程結尾](#pmc)。

[Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) 中提供了命令列介面 (CLI) 的 EF Core 工具。 若要安裝這個套件，請將它新增至 *.csproj* 檔案中的 `DotNetCliToolReference` 集合，如下所示。 **注意：** 此套件必須透過編輯 *.csproj* 檔案進行安裝。 `install-package` 命令或套件管理員 GUI 無法用來安裝此套件。 以滑鼠右鍵按一下方案總管 中的專案名稱，然後選取 [Edit ContosoUniversity.csproj] (編輯 ContosoUniversity.csproj)，以編輯 *.csproj* 檔案。

下列標記會顯示更新的 *.csproj* 檔案，並醒目提示 EF Core CLI 工具：

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?highlight=12)]
  
上述範例中的版本號碼在撰寫教學課程時是最新的。 請使用其他套件中相同版本的 EF Core CLI 工具。

## <a name="change-the-connection-string"></a>變更連接字串

在 *appsettings.json* 檔案中，將連接字串中的資料庫名稱變更為 ContosoUniversity2。

[!code-json[](intro/samples/cu/appsettings2.json?range=1-4)]

變更連接字串中的資料庫名稱，會導致第一個移轉建立新的資料庫。 因為不存在具有該名稱的資料庫，所以會建立新的資料庫。 開始使用移轉並不需要變更連接字串。

變更資料庫名稱的替代方式是刪除資料庫。 使用 [SQL Server 物件總管] (SSOX) 或 `database drop` CLI 命令：

 ```console
 dotnet ef database drop
 ```

下節說明如何執行 CLI 命令。

## <a name="create-an-initial-migration"></a>建立初始移轉

建置專案。

開啟命令視窗並巡覽至專案資料夾。 專案資料夾中包含 *Startup.cs* 檔案。

在命令視窗中輸入下列命令：

```console
dotnet ef migrations add InitialCreate
```

命令視窗會顯示與下列類似的資訊：

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

如果移轉失敗，訊息「無法存取檔案...ContosoUniversity.dll，因為其他處理序正在使用此檔。」 隨即顯示：

* 停止 IIS Express。

   * 結束並重新啟動 Visual Studio，或
   * 在 Windows 系統匣中尋找 IIS Express 圖示。
   * 以滑鼠右鍵按一下 IIS Express 圖示，然後按一下[ContosoUniversity] > [停止網站]

如果錯誤訊息「建置失敗。」 隨即顯示，請再次執行命令。 如果您收到這個錯誤，請在本教學課程的底部留下附註。

### <a name="examine-the-up-and-down-methods"></a>檢查 Up 和 Down 方法

EF Core 命令 `migrations add` 已產生用來建立資料庫的程式碼。 此移轉程式碼位於 Migrations\<時間戳記>_InitialCreate.cs 檔案中。 `InitialCreate` 類別的 `Up` 方法會建立對應至資料模型實體集的資料庫資料表。 `Down` 方法則會刪除它們，如下列範例所示：

[!code-csharp[](intro/samples/cu/Migrations/20171026010210_InitialCreate.cs?range=8-24,77-)]

移轉會呼叫 `Up` 方法，以實作資料模型變更來進行移轉。 當您輸入命令以復原更新時，移轉會呼叫 `Down` 方法。

上述程式碼適用於初始移轉。 該程式碼是在執行 `migrations add InitialCreate` 命令時建立。 移轉名稱參數 (在範例中為 "InitialCreate") 用於檔案名稱。 移轉名稱可以是任何有效的檔案名稱。 您最好選擇單字或片語來摘要列出移轉中所要完成的作業。 例如，新增了部門資料表的移轉可能稱為 "AddDepartmentTable"。

如果已建立初始移轉並結束資料庫，則：

* 會產生資料庫建立程式碼。
* 不需要執行資料庫建立程式碼，因為資料庫已符合資料模型。 如果執行了資料庫建立程式碼，它不會進行任何變更，因為資料庫已符合資料模型。

當應用程式部署到新的環境時，必須執行資料庫建立程式碼來建立資料庫。

之前，連接字串已變更為使用資料庫的新名稱。 指定的資料庫不存在；因此，移轉會建立資料庫。

### <a name="the-data-model-snapshot"></a>資料模型快照集

移轉會在 *Migrations/SchoolContextModelSnapshot.cs* 中建立目前資料庫結構描述的「快照集」。 當您新增移轉時，EF 會比較資料模型與快照集檔案，以判斷變更的內容。

刪除移轉時，請使用 [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) 命令。 `dotnet ef migrations remove` 會刪除移轉，並確保正確地重設快照集。

如需如何使用快照集檔案的詳細資訊，請參閱[小組環境中的 EF Core 移轉](/ef/core/managing-schemas/migrations/teams)。

## <a name="remove-ensurecreated"></a>移除 EnsureCreated

早期開發會使用 `EnsureCreated` 命令。 在本教學課程中，則使用移轉。 `EnsureCreated` 具有下列限制：

* 略過移轉，並建立資料庫和結構描述。
* 不會建立移轉資料表。
* 「無法」與移轉搭配使用。
* 設計用來測試或快速原型化經常卸除並重新建立資料庫的位置。

從 `DbInitializer` 移除下列各行：

```csharp
context.Database.EnsureCreated();
```

## <a name="apply-the-migration-to-the-db-in-development"></a>在開發環境中將移轉套用至資料庫

在命令視窗中輸入下列命令，以建立資料庫和資料表。

```console
dotnet ef database update
```

注意：如果 `update` 命令傳回「建置失敗。」錯誤：

* 再次執行命令。
* 如果再次失敗，請結束 Visual Studio，然後執行 `update` 命令。
* 在頁面底部留下訊息。

此命令的輸出類似於 `migrations add` 命令輸出。 在上述命令中，會顯示設定資料庫之 SQL 命令的記錄。 下列範例輸出中省略了大部分的記錄：

```text
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (467ms) [Parameters=[], CommandType='Text', CommandTimeout='60']
      CREATE DATABASE [ContosoUniversity2];
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (20ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      CREATE TABLE [__EFMigrationsHistory] (
          [MigrationId] nvarchar(150) NOT NULL,
          [ProductVersion] nvarchar(32) NOT NULL,
          CONSTRAINT [PK___EFMigrationsHistory] PRIMARY KEY ([MigrationId])
      );

<logs omitted for brevity>

info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (3ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      INSERT INTO [__EFMigrationsHistory] ([MigrationId], [ProductVersion])
      VALUES (N'20170816151242_InitialCreate', N'2.0.0-rtm-26452');
Done.
```

若要降低記錄訊息的詳細資料層級，請變更 *appsettings.Development.json* 檔案中的記錄層級。 如需詳細資訊，請參閱[記錄簡介](xref:fundamentals/logging/index)。

使用 **SQL Server 物件總管**來檢查資料庫。 請注意已新增 `__EFMigrationsHistory` 資料表。 `__EFMigrationsHistory` 資料表會追蹤哪些移轉經套用至資料庫。 檢視 `__EFMigrationsHistory` 資料表中的資料，它會顯示第一個移轉的某個資料列。 上述 CLI 輸出範例中的最後一則記錄會顯示建立此資料列的 INSERT 陳述式。

執行應用程式，並確認一切運作正常。

## <a name="applying-migrations-in-production"></a>在生產環境中套用移轉

建議在應用程式啟動時，生產環境應用程式**不**應該呼叫 [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_)。 `Migrate` 不應該從伺服器陣列中的應用程式進行呼叫。 例如，如果應用程式已使用向外延展 (執行應用程式的多個執行個體) 進行雲端部署。

資料庫移轉應該在部署中以受控制的方式完成。 生產環境資料庫移轉方法包括：

* 使用移轉來建立 SQL 指令碼，並在部署中使用 SQL 指令碼。
* 從受控制的環境中執行 `dotnet ef database update`。

EF Core 使用 `__MigrationsHistory` 資料表來查看是否有任何需要執行的移轉。 如果資料庫是最新狀態，則不會執行移轉。

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a>命令列介面 (CLI) 與套件管理員主控台 (PMC)

下列各項可使用管理移轉的 EF Core 工具：

* .NET Core CLI 命令。
* Visual Studio [套件管理員主控台] (PMC) 視窗中的 PowerShell Cmdlet。

本教學課程示範如何使用 CLI，有些開發人員則偏好使用 PMC。

PMC 的 EF Core 命令位於 [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) 套件中。 此套件包含在 [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) 中繼套件內，因此您不需要安裝它。

**重要事項：** 此套件與透過編輯 *.csproj* 檔案為 CLI 安裝的套件不同。 這個套件的名稱以 `Tools` 結尾，不同於以 `Tools.DotNet` 結尾的 CLI 套件名稱。

如需 CLI 命令的詳細資訊，請參閱 [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet)。

如需 PMC 命令的詳細資訊，請參閱[套件管理員主控台 (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell)。

## <a name="troubleshooting"></a>疑難排解

下載[此階段已完成的應用程式](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations)。

應用程式會產生下列例外狀況：

```text
SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

解決方案：執行 `dotnet ef database update`

如果 `update` 命令傳回「建置失敗。」錯誤：

* 再次執行命令。
* 在頁面底部留下訊息。

> [!div class="step-by-step"]
> [上一頁](xref:data/ef-rp/sort-filter-page)
> [下一頁](xref:data/ef-rp/complex-data-model)
