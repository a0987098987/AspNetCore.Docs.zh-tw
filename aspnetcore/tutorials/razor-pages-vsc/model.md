---
title: 使用 Visual Studio Code 將模型新增至 ASP.NET Core Razor 頁面應用程式
author: rick-anderson
description: 了解如何使用 Visual Studio Code 將模型新增至 ASP.NET Core 中的 Razor 頁面應用程式。
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: 3552b541c43375aef43838800855ec63e7fed372
ms.sourcegitcommit: ea7ec8d47f94cfb8e008d771f647f86bbb4baa44
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/06/2018
ms.locfileid: "37894039"
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-code"></a>使用 Visual Studio Code 將模型新增至 ASP.NET Core Razor 頁面應用程式

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a>新增資料模型

* 新增名為 *Models* 的資料夾。
* 將類別新增至名為 *Movie.cs* 的 *Models* 資料夾。
* 將下列程式碼新增至 *Models/Movie.cs* 檔案：

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-4)]

在 *Startup.cs* 最上方新增下列 `using` 陳述式：

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

請建置專案，以確認您沒有任何錯誤。

### <a name="entity-framework-core-nuget-packages-for-migrations"></a>用於移轉的 Entity Framework Core NuGet 套件

[Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) 中提供了命令列介面 (CLI) 的 EF 工具。 若要安裝這個套件，請將它新增至 *.csproj* 檔案中的 `DotNetCliToolReference` 集合。 **注意：** 您必須藉由編輯 *.csproj* 檔案來安裝這個套件；而不能使用 `install-package` 命令或套件管理員 GUI。

編輯 *RazorPagesMovie.csproj* 檔案：

* 選取 [檔案] > [開啟檔案]，然後選取 *RazorPagesMovie.csproj* 檔案。
* 將 `Microsoft.EntityFrameworkCore.Tools.DotNet` 的工具參考新增到第二個 **\<ItemGroup >**：

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj)]

[!INCLUDE [model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a>Scaffold 電影模型

* 在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。
* 執行下列命令：

**注意：請在 Windows 上執行下列命令。若為 MacOS 和 Linux，請參閱下一個命令**

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* 在 MacOS 和 Linux 上，執行下列命令：

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

如果您收到錯誤：
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

結束 Visual Studio 並再次執行命令。

[!INCLUDE [model 4](../../includes/RP/model4.md)]

下一個教學課程說明 Scaffolding 所建立的檔案。

> [!div class="step-by-step"]
> [上一步：開始使用](xref:tutorials/razor-pages-vsc/razor-pages-start)
> [下一步：Scaffold Razor 頁面](xref:tutorials/razor-pages-vsc/page)
