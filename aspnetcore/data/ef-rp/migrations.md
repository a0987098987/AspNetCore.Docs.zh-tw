---
title: "使用 EF 核心--4 的移轉 8 razor 頁面"
author: rick-anderson
description: "在本教學課程中，您啟動管理資料模型變更 ASP.NET Core MVC 應用程式中的使用 EF 核心移轉功能。"
ms.author: riande
manager: wpickett
ms.date: 10/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/migrations
ms.openlocfilehash: 7b0a3f73efd1d30b903b3258bea2082792eb6e8c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
# <a name="migrations---ef-core-with-razor-pages-tutorial-4-of-8"></a>移轉的 EF 核心 Razor 頁面教學課程 (8 個 4)

由[Tom Dykstra](https://github.com/tdykstra)， [Jon P Smith](https://twitter.com/thereformedprog)，和[Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

在本教學課程中，會使用管理資料模型所做變更的 EF 核心移轉功能。

如果您遇到無法解決的問題時，請下載[已完成的應用程式，為此階段](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations)。

當開發新的應用程式時，資料模型經常變更。 每次將模型變更，此模型取得與資料庫同步處理。 本教學課程，啟動設定 Entity Framework 來建立資料庫，如果不存在。 每次資料模型變更：

* 卸除資料庫。
* EF 會建立一個新符合模型。
* 應用程式植入的測試資料的資料庫。

這個方法讓資料庫保持同步資料模型的效果也直到您將應用程式部署到生產環境。 當應用程式在生產環境中執行時，它通常儲存需要維護的資料。 應用程式的開頭不能測試每次進行變更 （例如加入新的資料行） 的資料庫。 EF 核心移轉功能來解決這個問題，進而更新資料庫結構描述，而不是建立新的 DB EF 核心。

而不是卸除並重新建立資料庫的資料模型變更時，移轉更新的結構描述，並會保留現有的資料。

## <a name="entity-framework-core-nuget-packages-for-migrations"></a>用於移轉的 Entity Framework Core NuGet 套件

若要使用移轉，使用**Package Manager Console** (PMC) 或命令列介面 (CLI)。 這些教學課程會示範如何使用 CLI 命令。 Pmc 依存的相關資訊位於[本教學課程結尾](#pmc)。

中所提供的命令列介面 (CLI) 的 EF 核心工具[Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet)。 若要安裝此套件，將它加入`DotNetCliToolReference`集合*.csproj*檔案，如下所示。 **注意：**必須安裝此套件，藉由編輯*.csproj*檔案。 `install-package`命令或封裝管理員 GUI 無法用來安裝此套件。 編輯*.csproj*檔案中的專案名稱上按一下滑鼠右鍵**方案總管 中**，然後選取**編輯 ContosoUniversity.csproj**。

下列標記會顯示更新*.csproj*反白顯示的 EF 核心 CLI 工具檔：

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?highlight=12)]
  
在上述範例中的版本號碼是目前寫入教學課程時。 使用相同版本的其他封裝中的 EF 核心 CLI 工具。

## <a name="change-the-connection-string"></a>變更連接字串

在*appsettings.json*檔案中，將資料庫的連接字串的名稱變更為 ContosoUniversity2。

[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]

變更連接字串中的資料庫名稱，會導致第一個移轉建立新的 DB。 因為以該名稱不存在，會建立新的 DB。 變更連接字串並不需要移轉使用者入門。

變更資料庫名稱的替代方式刪除資料庫。 使用**SQL Server 物件總管**(SSOX) 或`database drop`CLI 命令：

 ```console
 dotnet ef database drop
 ```

下節說明如何執行 CLI 命令。

## <a name="create-an-initial-migration"></a>建立初始的移轉

建置專案。

開啟命令視窗並瀏覽至專案資料夾。 專案資料夾中包含*Startup.cs*檔案。

[命令] 視窗中輸入下列內容：

```console
dotnet ef migrations add InitialCreate
```

[命令] 視窗會顯示與下列類似的資訊：

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

如果移轉失敗，訊息 「*無法存取檔案...ContosoUniversity.dll 因為另一個處理序正在使用它。*" 會顯示：

* 停止 IIS Express。

   * 結束並重新啟動 Visual Studio 中，或
   * 尋找 Windows 系統匣中的 IIS Express 的圖示。
   * IIS Express] 圖示，以滑鼠右鍵按一下，然後按一下 [ **ContosoUniversity > 停止站台**。

如果錯誤訊息 「 建立失敗。 」 隨即顯示，請再次執行命令。 如果您收到這個錯誤，將附註保留在本教學課程的底部。

### <a name="examine-the-up-and-down-methods"></a>檢查向上和向下方法

EF 核心命令`migrations add`產生程式碼來建立從 DB。 此移轉程式碼位於*移轉\<時間戳記 > _InitialCreate.cs*檔案。 `Up`方法`InitialCreate`類別會建立對應至資料模型實體集的資料庫資料表。 `Down`方法會刪除它們，如下列範例所示：

[!code-csharp[Main](intro/samples/cu/Migrations/20171026010210_InitialCreate.cs?range=8-24,77-)]

移轉呼叫`Up`方法，以實作資料模型變更，以進行移轉。 當您輸入命令，以回復更新、 移轉呼叫`Down`方法。

上述程式碼是初始移轉。 該程式碼建立時`migrations add InitialCreate`執行命令。 移轉 name 參數 (在範例中為"InitialCreate 」) 用於檔案名稱。 移轉名稱可以是任何有效的檔案名稱。 您最好選擇的單字或片語來摘要列出所要完成移轉中的作業。 例如，加入 department 資料表移轉可能會呼叫"AddDepartmentTable。 」

如果初始的移轉會建立並 DB 結束：

* 資料庫建立程式碼會產生。
* 資料庫建立程式碼不需要執行，因為資料庫已經符合資料模型。 如果執行的 DB 建立程式碼時，它不會進行任何變更，因為資料庫已經符合資料模型。

當應用程式部署至新的環境時，資料庫建立程式碼必須執行來建立資料庫。

先前的連接字串已變更為使用新的資料庫名稱。 指定的資料庫不存在，因此，移轉會建立資料庫。

### <a name="examine-the-data-model-snapshot"></a>檢查資料模型快照集

建立移轉*快照*中目前的資料庫結構描述*Migrations/SchoolContextModelSnapshot.cs*:

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]

表示目前的資料庫結構描述時，程式碼中，因為 EF 核心沒有建立移轉資料庫與互動。 當您將在移轉時，EF 核心決定變更藉由比較資料模型，以快照集檔案的內容。 更新資料庫時，才會與 DB 互動 EF 核心。

快照集檔案必須與建立它的移轉作業的同步處理。 無法刪除名為該檔案移除移轉*\<時間戳記 > _\<migrationname >.cs*。 如果刪除該檔案時，剩餘的移轉將會與資料庫快照集檔案同步處理。 若要刪除加入的最後一個移轉，請使用[dotnet ef 移轉移除](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove)命令。

## <a name="remove-ensurecreated"></a>移除 EnsureCreated

早期開發，`EnsureCreated`命令的使用。 在本教學課程，請使用移轉。 `EnsureCreated`具有下列限制：

* 會略過移轉作業，並建立資料庫和結構描述。
* 不會建立移轉表格。
* 可以*不*搭配移轉。
* 可供測試或快速原型化的 DB 位置卸除並重新建立的頻率。

移除從下行`DbInitializer`:

```csharp
context.Database.EnsureCreated();
```

## <a name="apply-the-migration-to-the-db-in-development"></a>適用於資料庫開發的移轉

在 [命令] 視窗中，輸入下列命令來建立資料庫和資料表。

```console
dotnet ef database update
```

注意： 如果`update`命令會傳回錯誤 「 建置失敗。 」:

* 重新執行命令。
* 如果再次失敗，結束 Visual Studio，然後再執行`update`命令。
* 將保留在頁面底部的訊息。

此命令的輸出是類似於`migrations add`命令輸出。 在上述命令中，會顯示記錄檔中的設定資料庫的 SQL 命令。 大部分的記錄檔會在下列範例輸出中省略：

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

若要減少的記錄檔訊息詳細程度可以變更的記錄層級*appsettings。Development.json*檔案。 如需詳細資訊，請參閱[簡介記錄](xref:fundamentals/logging/index)。

使用**SQL Server 物件總管**檢查 DB。 請注意新增`__EFMigrationsHistory`資料表。 `__EFMigrationsHistory`資料表會追蹤的哪些移轉已經套用至資料庫。 檢視中的資料`__EFMigrationsHistory`資料表，它會顯示第一次移轉一個資料列。 上述的 CLI 輸出範例中的最後一個記錄檔會顯示建立此資料列 INSERT 陳述式。

執行應用程式，並確認一切運作。

## <a name="appling-migrations-in-production"></a>在生產環境中套用移轉

我們建議實際執行應用程式應該**不**呼叫[Database.Migrate](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_)應用程式啟動時。 `Migrate`不應該從伺服器陣列中的應用程式呼叫。 例如，如果應用程式已部署向外 （執行的應用程式的多個執行個體） 的雲端。

資料庫移轉應該在部署中，並在受控制的方式。 包括實際執行資料庫移轉方法：

* 若要建立 SQL 指令碼中使用移轉，並在部署中使用的 SQL 指令碼。
* 執行`dotnet ef database update`從受控制的環境。

使用 EF 核心`__MigrationsHistory`資料表是否有任何移轉作業執行。 如果資料庫是最新狀態，則會執行不移轉。

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a>命令列介面 (CLI) 與Package Manager Console (PMC)

EF 核心工具來管理移轉是可從項目：

* .NET core CLI 命令。
* Visual Studio 中的 PowerShell cmdlet **Package Manager Console** (PMC) 視窗。

本教學課程示範如何使用 CLI，有些開發人員會習慣使用 pmc 依存。

Pmc 依存的 EF 核心命令位於[Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools)封裝。 此套件包含在[Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage，所以您不需要安裝它。

**重要事項：**這不是相同的封裝，做為您藉由編輯安裝 CLI *.csproj*檔案。 這個名稱結尾`Tools`，不同於 CLI 封裝名稱中結尾`Tools.DotNet`。

如需 CLI 命令的詳細資訊，請參閱[.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet)。

如需 PMC 命令的詳細資訊，請參閱[Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell)。

## <a name="troubleshooting"></a>疑難排解

下載[已完成的應用程式，為此階段](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations)。

應用程式會產生下列例外狀況：

```text
`SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

解決方案： 執行`dotnet ef database update`

如果`update`命令會傳回錯誤 「 建置失敗。 」:

* 重新執行命令。
* 將保留在頁面底部的訊息。

>[!div class="step-by-step"]
[上一頁](xref:data/ef-rp/sort-filter-page)
[下一頁](xref:data/ef-rp/complex-data-model)
