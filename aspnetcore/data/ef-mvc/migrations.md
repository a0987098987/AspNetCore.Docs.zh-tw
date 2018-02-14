---
title: "ASP.NET Core MVC 與 EF Core - 移轉 - 4/10"
author: tdykstra
description: "在本教學課程中，您將開始使用 EF Core 移轉功能來管理 ASP.NET Core MVC 應用程式中的資料模型變更。"
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/migrations
ms.openlocfilehash: fd466af8a73bf4c568fafe7e7fdcaa82021624da
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/31/2018
---
# <a name="migrations---ef-core-with-aspnet-core-mvc-tutorial-4-of-10"></a>移轉 - EF Core 與 ASP.NET Core MVC 教學課程 (4/10)

作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso 大學範例 Web 應用程式將示範如何以 Entity Framework Core 和 Visual Studio 來建立 ASP.NET Core MVC Web 應用程式。 如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](intro.md)。

在本教學課程中，您會先使用 EF Core 移轉功能來管理資料模型變更。 在稍後的教學課程中，您將在變更資料模型時新增更多移轉作業。

## <a name="introduction-to-migrations"></a>移轉簡介

在開發新的應用程式時，您的資料模型會頻繁變更，而每次模型變更時，模型會與資料庫不同步。 首先，您會設定 Entity Framework 以建立資料庫 (如果尚未存在的話) 以進行本教學課程。 然後在每次變更資料模型 (新增、移除或變更實體類別或變更您的 DbContext 類別) 時，您可以刪除資料庫，EF 即會建立一個相符的新模型，並植入測試資料。

在您將應用程式部署到生產環境之前，都可以使用上述方法讓資料庫與資料模型保持同步。 但當應用程式在生產環境中執行時，通常會儲存您想要保留的資料，而您也不想在每次資料變更 (例如新增資料行) 時遺失任何項目。 為了解決上述問題，EF Core 移轉功能可讓 EF 更新資料庫結構描述，而不是建立新的資料庫。

## <a name="entity-framework-core-nuget-packages-for-migrations"></a>用於移轉的 Entity Framework Core NuGet 套件

您可以使用**套件管理員主控台** (PMC) 或命令列介面 (CLI)，來進行移轉作業。  這些教學課程會示範如何使用 CLI 命令。 PMC 的資訊則位於[本教學課程結尾](#pmc)。

[Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) 中提供了命令列介面 (CLI) 的 EF 工具。 若要安裝這個套件，請將它新增至 *.csproj* 檔案中的 `DotNetCliToolReference` 集合，如下所示。 **注意：**您必須藉由編輯 *.csproj* 檔案來安裝這個套件；而不能使用 `install-package` 命令或套件管理員 GUI。 在方案總管中，以滑鼠右鍵按一下專案名稱，然後選取 [Edit ContosoUniversity.csproj] (編輯 ContosoUniversity.csproj) 以編輯 *.csproj* 檔案。

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?range=12-15&highlight=2)]
  
(範例中的版本號碼為教學課程撰寫當下的最新版本。) 

## <a name="change-the-connection-string"></a>變更連接字串

在 *appsettings.json* 檔案中，將連接字串中的資料庫名稱變更為 ContosoUniversity2 (或您所使用之電腦上未用過的其他名稱)。

[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]

這項變更會設定專案，以讓第一次移轉建立新的資料庫。 即使不採取上述動作，仍可開始進行移轉作業，但稍後您會了解這麼做的好處何在。

> [!NOTE]
> 如果您不想變更資料庫名稱，替代方法是刪除資料庫。 使用 [SQL Server 物件總管] (SSOX) 或 `database drop` CLI 命令：
> ```console
> dotnet ef database drop
> ```
> 下節說明如何執行 CLI 命令。

## <a name="create-an-initial-migration"></a>建立初始移轉

儲存您的變更，並建置專案。 接著，開啟命令視窗並巡覽至專案資料夾。 以下是執行這個動作的快速方法：

* 在方案總管中，以滑鼠右鍵按一下專案，然後選擇操作功能表的 [在檔案總管中開啟]。

  ![[在檔案總管中開啟] 功能表項目](migrations/_static/open-in-file-explorer.png)

* 在位址列中輸入 "cmd"，然後按 Enter。

  ![開啟命令視窗](migrations/_static/open-command-window.png)

在命令視窗中輸入下列命令：

```console
dotnet ef migrations add InitialCreate
```

在命令視窗中，您會看到類似如下的輸出：

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

> [!NOTE]
> 如果您看到錯誤訊息：「找不到符合命令 "dotnet-ef" 的可執行檔」，請參閱[這篇部落格文章](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/)以取得疑難排解說明。

如果您看到錯誤訊息：「無法存取檔案...ContosoUniversity.dll，因為其他處理序正在使用此檔案。」請在 Windows 系統匣中尋找 IIS Express 圖示，以滑鼠右鍵按一下它，然後按一下 [ContosoUniversity] > [停止網站]。

## <a name="examine-the-up-and-down-methods"></a>檢查 Up 和 Down 方法

當您執行 `migrations add` 命令時，EF 產生的程式碼會從頭開始建立資料庫。 您可以在 *Migrations* 資料夾，名為 \<時間戳記>_InitialCreate.cs 的檔案中，找到這個程式碼。 `InitialCreate` 類別的 `Up` 方法會建立對應至資料模型實體集的資料庫資料表，而 `Down` 方法則會刪除它們，如下列範例所示。

[!code-csharp[Main](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]

Migrations 會呼叫 `Up` 方法，以實作移轉所需的資料模型變更。 當您輸入命令以復原更新時，Migrations 會呼叫 `Down` 方法。

這個程式碼適用於您之前輸入 `migrations add InitialCreate` 命令時所建立的初始移轉。 移轉名稱參數 (在此範例中為 "InitialCreate") 可作為檔案名稱，您也可以任意命名。 建議您選擇某個單字或片語，以摘要說明移轉中所要完成的作業。 例如，您可以將稍後的移轉命名為 "AddDepartmentTable"。

如果在您建立初始移轉時資料庫已經存在，系統會產生資料庫建立程式碼，但不需要執行，因為資料庫已經符合資料模型。 當您將應用程式部署到資料庫尚未存在的其他環境中時，即會執行這個程式碼以建立您的資料庫；建議您先進行測試。 這就是為什麼稍早要您變更連接字串中資料庫名稱的原因，這樣一來，移轉即可從頭建立一個資料庫。

## <a name="examine-the-data-model-snapshot"></a>檢查資料模型快照集

Migrations 也會在 *Migrations/SchoolContextModelSnapshot.cs* 中建立目前資料庫結構描述的「快照集」。 程式碼應如下所示：

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]

因為目前的資料庫結構描述是以程式碼表示，所以 EF Core 不需與資料庫互動即可建立移轉。 當您新增移轉時，EF 會比較資料模型與快照集檔案，以判斷變更的內容。 只有當 EF 必須更新資料庫時，它才會與資料庫互動。 

快照集檔案必須與建立快照集檔案的移轉同步；因此，要移除移轉時您不能直接刪除名為 \<時間戳記>_\<migrationname>.cs 的檔案。 如果刪除該檔案，剩餘的移轉會與資料庫快照集檔案不同步。 若要刪除最後一個新增的移轉，請使用 [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) 命令。

## <a name="apply-the-migration-to-the-database"></a>將移轉套用至資料庫

在命令視窗中輸入下列命令，以建立資料庫和其中的資料表。

```console
dotnet ef database update
```

此命令的輸出類似於 `migrations add` 命令，不同之處在於其會顯示設定資料庫之 SQL 命令的記錄。 下列範例輸出中省略了大部分的記錄。 如果您不想看到這麼詳細的記錄訊息，可以變更 *appsettings.Development.json* 檔案中的記錄層級。 如需詳細資訊，請參閱[記錄簡介](xref:fundamentals/logging/index)。

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

按照第一個教學課程的做法，使用 [SQL Server 物件總管] 檢查資料庫。  您會發現其中新增了 __EFMigrationsHistory 資料表，其會持續追蹤哪些移轉已套用至資料庫。 檢視該資料表中的資料，您會看到其中一個資料列代表第一個移轉。 (上述 CLI 輸出範例中的最後一則記錄會顯示建立此資料列的 INSERT 陳述式。)

執行應用程式，以驗證一切如往常般運作。

![Students [索引] 頁面](migrations/_static/students-index.png)

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a>命令列介面 (CLI) 與套件管理員主控台 (PMC)

您可以透過 .NET Core CLI 命令或 Visual Studio [套件管理員主控台] (PMC) 視窗中的 PowerShell Cmdlet，取得適用於管理移轉的 EF 工具。 本教學課程示範如何使用 CLI，但是您也可以視習慣使用 PMC。

適用於 PMC 命令的 EF 命令位於 [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) 套件中。 此套件已包含在 [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) 中繼套件內，因此您不需要進行安裝。

**重要事項：**此套件與您為 CLI 安裝的套件不同 (透過編輯 *.csproj* 檔案來進行)。 這個套件的名稱以 `Tools` 結尾，不同於以 `Tools.DotNet` 結尾的 CLI 套件名稱。

如需 CLI 命令的詳細資訊，請參閱 [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet)。 

如需 PMC 命令的詳細資訊，請參閱[套件管理員主控台 (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell)。

## <a name="summary"></a>總結

在本教學課程中，您已了解如何建立並套用第一次移轉。 在下一個教學課程中，您就可以展開資料模型以學習更進階的主題。 在過程中，您會建立並套用其他移轉。

>[!div class="step-by-step"]
[上一頁](sort-filter-page.md)
[下一頁](complex-data-model.md)  
