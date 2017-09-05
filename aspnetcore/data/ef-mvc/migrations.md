---
title: "ASP.NET Core MVC EF 核心--4 的移轉 10"
author: tdykstra
description: "在本教學課程中，您啟動管理資料模型變更 ASP.NET Core MVC 應用程式中的使用 EF 核心移轉功能。"
keywords: "ASP.NET Core，Entity Framework Core，移轉"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 81f6c9c2-a819-4f3a-97a4-4b0503b56c26
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/migrations
ms.openlocfilehash: 4d81099d1ab97a8a49d96657153a54aa96dd6bf8
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/25/2017
---
# <a name="migrations---ef-core-with-aspnet-core-mvc-tutorial-4-of-10"></a>移轉的 EF Core 與 ASP.NET Core MVC 教學課程 (10-4)

由[Tom Dykstra](https://github.com/tdykstra)和[Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso 大學範例 web 應用程式示範如何建立 ASP.NET Core MVC web 應用程式使用 Entity Framework Core 和 Visual Studio。 教學課程系列的相關資訊，請參閱[系列的第一個教學課程](intro.md)。

在本教學課程中，您啟動管理資料模型變更使用的 EF 核心移轉功能。 在之後的教學課程，您可以加入更多的移轉作業，當您變更資料模型。

## <a name="introduction-to-migrations"></a>移轉的簡介

當您開發新的應用程式時，您的資料模型變更常見問題，以及每次將模型變更時，它會取得與資料庫同步處理。 您可以設定 Entity Framework 來建立資料庫，如果不存在，以啟動這些教學課程。 然後每次您變更資料模型-加入、 移除或變更實體類別或變更您的 DbContext 類別-您可以刪除資料庫，而且 EF 會建立一個新符合模型，並設測試資料。

讓資料庫保持同步資料模型的這個方法適用於也直到您將部署到生產環境應用程式。 它通常會儲存您想要保留，且您不想遺失的所有項目每次資料的生產環境中執行應用程式時您變更例如加入新的資料行。 EF 核心移轉功能來解決這個問題，進而 EF 更新資料庫結構描述，而不是建立新的資料庫。

## <a name="entity-framework-core-nuget-packages-for-migrations"></a>進行移轉的 entity Framework Core NuGet 封裝

若要使用移轉，您可以使用**Package Manager Console** (PMC) 或命令列介面 (CLI)。  這些教學課程會示範如何使用 CLI 命令。 Pmc 依存的相關資訊位於[本教學課程結尾](#pmc)。

中所提供的命令列介面 (CLI) 的 EF 工具[Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet)。 若要安裝此套件，將它加入`DotNetCliToolReference`集合*.csproj*檔案，如下所示。 **注意：**您需要安裝此套件，藉由編輯*.csproj*檔案; 您不能使用`install-package`命令或封裝管理員 GUI。 您可以編輯*.csproj*檔案中的專案名稱上按一下滑鼠右鍵**方案總管 中**，然後選取**編輯 ContosoUniversity.csproj**。

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?range=12-15&highlight=2)]
  
（在此範例中的版本號碼是目前寫入教學課程時）。 

## <a name="change-the-connection-string"></a>變更連接字串

在*appsettings.json*檔案中，將連接字串中的資料庫名稱變更為 ContosoUniversity2 或您並未使用您所使用的電腦上的其他部份名稱。

[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]

這項變更設定，讓第一次移轉將會建立新的資料庫專案。 這不是為了開始使用移轉，不過您看到更新版本的原因是個不錯的主意。

> [!NOTE]
> 變更資料庫名稱的替代方案，您可以刪除資料庫。 使用**SQL Server 物件總管**(SSOX) 或`database drop`CLI 命令：
> ```console
> dotnet ef database drop
> ```
> 下節說明如何執行 CLI 命令。

## <a name="create-an-initial-migration"></a>建立初始的移轉

儲存您的變更，並建置專案。 開啟命令視窗，然後瀏覽至專案資料夾。 以下是快速的方法：

* 在**方案總管] 中**，以滑鼠右鍵按一下專案，然後選擇 [**在檔案總管中開啟**從內容功能表。

  ![在 [檔案總管] 功能表項目中開啟](migrations/_static/open-in-file-explorer.png)

* 在網址列中輸入"cmd 」，然後按 Enter。

  ![開啟命令視窗](migrations/_static/open-command-window.png)

在 [命令] 視窗中輸入下列命令：

```console
dotnet ef migrations add InitialCreate
```

您會看到類似 [命令] 視窗中的下列輸出：

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

> [!NOTE]
> 如果您看到錯誤訊息*任何可執行檔中找到相符命令"dotnet-ef"*，請參閱[此部落格文章](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/)如需疑難排解的協助。

如果您看到錯誤訊息 「*無法存取檔案...ContosoUniversity.dll 因為另一個處理序正在使用它。*」，在 Windows 系統匣中，尋找 [IIS Express] 圖示，然後按一下滑鼠右鍵，然後按一下**ContosoUniversity > 停止站台**。

## <a name="examine-the-up-and-down-methods"></a>檢查向上和向下方法

當您執行`migrations add`命令，EF 產生的程式碼，將會從頭開始建立資料庫。 此程式碼位於*移轉*資料夾中名為的檔案*\<時間戳記 > _InitialCreate.cs*。 `Up`方法`InitialCreate`類別會建立對應至資料模型實體集的資料庫資料表和`Down`方法會刪除它們，如下列範例所示。

[!code-csharp[Main](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]

移轉呼叫`Up`方法，以實作資料模型變更，以進行移轉。 當您輸入命令，以回復更新、 移轉呼叫`Down`方法。

此程式碼是您輸入時所建立的初始移轉`migrations add InitialCreate`命令。 移轉 name 參數 (在範例中為"InitialCreate 」) 使用的檔案名稱，而且可以是您所要的任何。 您最好選擇的單字或片語來摘要列出所要完成移轉中的作業。 例如，您可能會命名稍後移轉"AddDepartmentTable"。

如果資料庫已經存在時，您會建立初始的移轉，資料庫建立程式碼會產生，但它不一定要執行，因為資料庫已經符合資料模型。 當您部署應用程式到另一個環境，其中資料庫不存在，但這段程式碼會執行以建立您的資料庫時，因此，建議您先測試它。 這就是為什麼您使移轉可以建立一個新從頭變更先前-在連接字串中的資料庫名稱。

## <a name="examine-the-data-model-snapshot"></a>檢查資料模型快照集

移轉也會建立*快照*中目前的資料庫結構描述*Migrations/SchoolContextModelSnapshot.cs*。 以下是該程式碼看起來像：

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]

表示目前的資料庫結構描述時，程式碼中，因為 EF 核心沒有與要建立移轉作業的資料庫互動。 當您將在移轉時，EF 決定變更藉由比較資料模型，以快照集檔案的內容。 與資料庫的 EF 互動，才必須更新資料庫。 

快照集檔案必須加以建立，所以您不只要刪除名為的檔案移除移轉的移轉作業同步*\<時間戳記 > _\<migrationname >.cs*。 如果您刪除該檔案，將剩餘的移轉將會與資料庫快照集檔案同步處理。 若要刪除最後一個您所加入的移轉，請使用[dotnet ef 移轉移除](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove)命令。

## <a name="apply-the-migration-to-the-database"></a>套用至資料庫的移轉

在 [命令] 視窗中，輸入下列命令以在其中建立的資料庫和資料表。

```console
dotnet ef database update
```

此命令的輸出是類似於`migrations add`命令時，不同之處在於如的 SQL 命令，將資料庫設定，請參閱記錄檔。 下列範例輸出中將略過大部分的記錄檔。 如果您不想看到此層級記錄檔訊息詳細資料，您可以變更的記錄層級*appsettings。Development.json*檔案。 如需詳細資訊，請參閱[簡介記錄](xref:fundamentals/logging)。

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

使用**SQL Server 物件總管**檢查資料庫，您可以如同在第一個教學課程。  您會發現 __EFMigrationsHistory 資料表，可追蹤的哪些移轉已經套用至資料庫的加法。 檢視資料，該資料表中，您會看到第一次移轉一個資料列。 （最後一個記錄檔，在上述的 CLI 輸出範例顯示建立此資料列 INSERT 陳述式）。

執行應用程式驗證運作一切仍然正常與之前相同。

![學生索引頁](migrations/_static/students-index.png)

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a>命令列介面 (CLI) 與Package Manager Console (PMC)

EF，工具管理移轉為.NET Core CLI 命令或從 Visual Studio 中的 PowerShell 指令程式使用**Package Manager Console** (PMC) 視窗。 本教學課程示範如何使用 CLI，但是如果您想要的話，您可以使用 pmc 依存。

PMC 命令的 EF 命令[Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools)封裝。 此套件已隨附於[Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage，所以您不需要安裝它。

**重要事項：**這不是相同的封裝，做為您藉由編輯安裝 CLI *.csproj*檔案。 這個名稱結尾`Tools`，不同於 CLI 封裝名稱中結尾`Tools.DotNet`。

如需 CLI 命令的詳細資訊，請參閱[.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet)。 

如需 PMC 命令的詳細資訊，請參閱[Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell)。

## <a name="summary"></a>總結

在本教學課程中，您已經看到如何建立和套用您第一次移轉。 在下一個教學課程中，開始，您將查看更進階的主題，方法是展開的資料模型。 在過程中，您會建立並套用其他移轉。

>[!div class="step-by-step"]
[上一頁](sort-filter-page.md)
[下一頁](complex-data-model.md)  
