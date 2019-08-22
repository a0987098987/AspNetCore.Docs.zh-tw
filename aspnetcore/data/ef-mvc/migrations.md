---
title: 教學課程：使用移轉功能 - ASP.NET MVC 搭配 EF Core
description: 在本教學課程中，您將開始使用 EF Core 移轉功能來管理 ASP.NET Core MVC 應用程式中的資料模型變更。
author: tdykstra
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/27/2019
ms.topic: tutorial
uid: data/ef-mvc/migrations
ms.openlocfilehash: 7ee383ff5fcd9dd79503321aaa188fd85ef18d7a
ms.sourcegitcommit: 257cc3fe8c1d61341aa3b07e5bc0fa3d1c1c1d1c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/19/2019
ms.locfileid: "69583460"
---
# <a name="tutorial-using-the-migrations-feature---aspnet-mvc-with-ef-core"></a>教學課程：使用移轉功能 - ASP.NET MVC 搭配 EF Core

在本教學課程中，您會先使用 EF Core 移轉功能來管理資料模型變更。 在稍後的教學課程中，您將在變更資料模型時新增更多移轉作業。

在本教學課程中，您已：

> [!div class="checklist"]
> * 了解移轉
> * 變更連接字串
> * 建立初始移轉
> * 檢查 Up 和 Down 方法
> * 了解資料模型快照集
> * 套用移轉

## <a name="prerequisites"></a>必要條件

* [排序、篩選及分頁](sort-filter-page.md)

## <a name="about-migrations"></a>關於移轉

在開發新的應用程式時，您的資料模型會頻繁變更，而每次模型變更時，模型會與資料庫不同步。 首先，您會設定 Entity Framework 以建立資料庫 (如果尚未存在的話) 以進行本教學課程。 然後在每次變更資料模型 (新增、移除或變更實體類別或變更您的 DbContext 類別) 時，您可以刪除資料庫，EF 即會建立一個相符的新模型，並植入測試資料。

在您將應用程式部署到生產環境之前，都可以使用上述方法讓資料庫與資料模型保持同步。 但當應用程式在生產環境中執行時，通常會儲存您想要保留的資料，而您也不想在每次資料變更 (例如新增資料行) 時遺失任何項目。 為了解決上述問題，EF Core 移轉功能可讓 EF 更新資料庫結構描述，而不是建立新的資料庫。

您可以使用**套件管理員主控台** (PMC) 或命令列介面 (CLI)，來進行移轉作業。  這些教學課程會示範如何使用 CLI 命令。 PMC 的資訊位於[本教學課程結尾](#pmc)。

## <a name="change-the-connection-string"></a>變更連接字串

在 *appsettings.json* 檔案中，將連接字串中的資料庫名稱變更為 ContosoUniversity2 (或您所使用之電腦上未用過的其他名稱)。

[!code-json[](intro/samples/cu/appsettings2.json?range=1-4)]

這項變更會設定專案，以讓第一次移轉建立新的資料庫。 即使不採取上述動作，仍可開始進行移轉作業，但稍後您會了解這麼做的好處何在。

> [!NOTE]
> 如果您不想變更資料庫名稱，替代方法是刪除資料庫。 使用 [SQL Server 物件總管]  (SSOX) 或 `database drop` CLI 命令：
>
> ```console
> dotnet ef database drop
> ```
>
> 下節說明如何執行 CLI 命令。

## <a name="create-an-initial-migration"></a>建立初始移轉

儲存您的變更，並建置專案。 接著，開啟命令視窗並巡覽至專案資料夾。 以下是執行這個動作的快速方法：

* 在 [方案總管]  中，於專案上按一下滑鼠右鍵，然後從操作功能表中選擇 [在檔案總管中開啟資料夾]  。

  ![[在檔案總管中開啟] 功能表項目](migrations/_static/open-in-file-explorer.png)

* 在位址列中輸入 "cmd"，然後按 Enter。

  ![開啟命令視窗](migrations/_static/open-command-window.png)

在命令視窗中輸入下列命令：

```console
dotnet ef migrations add InitialCreate
```

在命令視窗中，您會看到類似如下的輸出：

```console
info: Microsoft.EntityFrameworkCore.Infrastructure[10403]
      Entity Framework Core 2.2.0-rtm-35687 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

> [!NOTE]
> 如果您看到錯誤訊息：「找不到符合命令 "dotnet-ef" 的可執行檔」  ，請參閱[這篇部落格文章](https://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/)以取得疑難排解說明。

如果您看到錯誤訊息：「無法存取檔案...  ContosoUniversity.dll，因為其他處理序正在使用此檔案。」請在 Windows 系統匣中尋找 IIS Express 圖示，以滑鼠右鍵按一下它，然後按一下 [ContosoUniversity] > [停止網站]  。

## <a name="examine-up-and-down-methods"></a>檢查 Up 和 Down 方法

當您執行 `migrations add` 命令時，EF 產生的程式碼會從頭開始建立資料庫。 您可以在 *Migrations* 資料夾，名為 \<時間戳記>_InitialCreate.cs  的檔案中，找到這個程式碼。 `InitialCreate` 類別的 `Up` 方法會建立對應至資料模型實體集的資料庫資料表，而 `Down` 方法則會刪除它們，如下列範例所示。

[!code-csharp[](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]

Migrations 會呼叫 `Up` 方法，以實作移轉所需的資料模型變更。 當您輸入命令以復原更新時，Migrations 會呼叫 `Down` 方法。

這個程式碼適用於您之前輸入 `migrations add InitialCreate` 命令時所建立的初始移轉。 移轉名稱參數 (在此範例中為 "InitialCreate") 可作為檔案名稱，您也可以任意命名。 建議您選擇某個單字或片語，以摘要說明移轉中所要完成的作業。 例如，您可以將稍後的移轉命名為 "AddDepartmentTable"。

如果在您建立初始移轉時資料庫已經存在，系統會產生資料庫建立程式碼，但不需要執行，因為資料庫已經符合資料模型。 當您將應用程式部署到資料庫尚未存在的其他環境中時，即會執行這個程式碼以建立您的資料庫；建議您先進行測試。 這就是為什麼稍早要您變更連接字串中資料庫名稱的原因，這樣一來，移轉即可從頭建立一個資料庫。

## <a name="the-data-model-snapshot"></a>資料模型快照集

移轉會在 *Migrations/SchoolContextModelSnapshot.cs* 中建立目前資料庫結構描述的「快照集」  。 當您新增移轉時，EF 會比較資料模型與快照集檔案，以判斷變更的內容。

刪除移轉時，請使用 [dotnet ef migrations remove](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) 命令。 `dotnet ef migrations remove` 會刪除移轉，並確保正確地重設快照集。

如需如何使用快照集檔案的詳細資訊，請參閱[小組環境中的 EF Core 移轉](/ef/core/managing-schemas/migrations/teams)。

## <a name="apply-the-migration"></a>套用移轉

在命令視窗中輸入下列命令，以建立資料庫和其中的資料表。

```console
dotnet ef database update
```

此命令的輸出類似於 `migrations add` 命令，不同之處在於其會顯示設定資料庫之 SQL 命令的記錄。 下列範例輸出中省略了大部分的記錄。 如果您不想看到這麼詳細的記錄訊息，可以變更 *appsettings.Development.json* 檔案中的記錄層級。 如需詳細資訊，請參閱 <xref:fundamentals/logging/index>。

```text
info: Microsoft.EntityFrameworkCore.Infrastructure[10403]
      Entity Framework Core 2.2.0-rtm-35687 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
info: Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (274ms) [Parameters=[], CommandType='Text', CommandTimeout='60']
      CREATE DATABASE [ContosoUniversity2];
info: Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (60ms) [Parameters=[], CommandType='Text', CommandTimeout='60']
      IF SERVERPROPERTY('EngineEdition') <> 5
      BEGIN
          ALTER DATABASE [ContosoUniversity2] SET READ_COMMITTED_SNAPSHOT ON;
      END;
info: Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (15ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      CREATE TABLE [__EFMigrationsHistory] (
          [MigrationId] nvarchar(150) NOT NULL,
          [ProductVersion] nvarchar(32) NOT NULL,
          CONSTRAINT [PK___EFMigrationsHistory] PRIMARY KEY ([MigrationId])
      );

<logs omitted for brevity>

info: Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (3ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      INSERT INTO [__EFMigrationsHistory] ([MigrationId], [ProductVersion])
      VALUES (N'20190327172701_InitialCreate', N'2.2.0-rtm-35687');
Done.
```

按照第一個教學課程的做法，使用 [SQL Server 物件總管]  檢查資料庫。  您會發現新增 \_\_EFMigrationsHistory 資料表，其會持續追蹤哪些移轉已套用至資料庫。 檢視該資料表中的資料，您會看到其中一個資料列代表第一個移轉。 (上述 CLI 輸出範例中的最後一則記錄會顯示建立此資料列的 INSERT 陳述式。)

執行應用程式，以驗證一切如往常般運作。

![Students [索引] 頁面](migrations/_static/students-index.png)

<a id="pmc"></a>

## <a name="compare-cli-and-pmc"></a>比較 CLI 與 PMC

您可以透過 .NET Core CLI 命令或 Visual Studio [套件管理員主控台]  (PMC) 視窗中的 PowerShell Cmdlet，取得適用於管理移轉的 EF 工具。 本教學課程示範如何使用 CLI，但是您也可以視習慣使用 PMC。

適用於 PMC 命令的 EF 命令位於 [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) 套件中。 此套件包含在 [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) 中；因此，如果您的應用程式具有 `Microsoft.AspNetCore.App` 的套件參考，則您不需要新增套件參考。

**重要：** 此套件與您透過編輯 *.csproj* 檔案為 CLI 安裝的套件不同。 這個套件的名稱以 `Tools` 結尾，不同於以 `Tools.DotNet` 結尾的 CLI 套件名稱。

如需 CLI 命令的詳細資訊，請參閱 [.NET Core CLI](/ef/core/miscellaneous/cli/dotnet)。

如需 PMC 命令的詳細資訊，請參閱[套件管理員主控台 (Visual Studio)](/ef/core/miscellaneous/cli/powershell)。

## <a name="get-the-code"></a>取得程式碼

[下載或檢視已完成的應用程式。](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

## <a name="next-step"></a>後續步驟

在本教學課程中，您已：

> [!div class="checklist"]
> * 了解移轉
> * 了解 NuGet 移轉套件
> * 變更連接字串
> * 建立初始移轉
> * 檢查 Up 和 Down 方法
> * 了解資料模型快照集
> * 套用移轉

若要開始查看更進階的主題，了解資料模型的擴展，請前往下一個教學課程。 在過程中，您會建立並套用其他移轉。

> [!div class="nextstepaction"]
> [建立並套用額外的移轉](complex-data-model.md)
