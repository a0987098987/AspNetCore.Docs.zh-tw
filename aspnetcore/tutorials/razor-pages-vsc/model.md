---
title: 使用 Visual Studio Code 將模型新增至 ASP.NET Core Razor 頁面應用程式
author: rick-anderson
description: 了解如何使用 Visual Studio Code 將模型新增至 ASP.NET Core 中的 Razor 頁面應用程式。
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: c4aef369bb3965b70d1b461cf63e6f5a26a00628
ms.sourcegitcommit: c43a6f1fe72d7c2db4b5815fd532f2b45d964e07
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244719"
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-code"></a>使用 Visual Studio Code 將模型新增至 ASP.NET Core Razor 頁面應用程式

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a>新增資料模型

* 新增名為 *Models* 的資料夾。
* 將類別新增至名為 *Movie.cs* 的 *Models* 資料夾。
* 將下列程式碼新增至 *Models/Movie.cs* 檔案：

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

### <a name="entity-framework-core-nuget-package-for-sqlite"></a>SQLite 的 Entity Framework Core NuGet 套件

從命令列中，執行下列 .NET Core CLI 命令：

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite
```

<a name="reg"></a>

### <a name="register-the-database-context"></a>登錄資料庫內容

以[相依性插入](xref:fundamentals/dependency-injection)容器在 *Startup.cs* 檔案中登錄資料庫內容。

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=10-11)]

在 *Startup.cs* 最上方新增下列 `using` 陳述式：

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

請建置專案，以確認您沒有任何錯誤。

[!INCLUDE [model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>

### <a name="scaffold-the-movie-model"></a>Scaffold 電影模型

* 在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。
* **針對 Windows**：執行下列命令：

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* **針對 macOS 與 Linux**：執行下列命令：

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [model 4](../../includes/RP/model4.md)]

下一個教學課程說明 Scaffolding 所建立的檔案。

> [!div class="step-by-step"]
> [上一步：開始使用](xref:tutorials/razor-pages-vsc/razor-pages-start)
> [下一步：Scaffold Razor 頁面](xref:tutorials/razor-pages-vsc/page)
