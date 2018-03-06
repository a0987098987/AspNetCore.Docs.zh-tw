---
title: "將模型新增至 ASP.NET Core 中的 Razor Pages 應用程式"
author: rick-anderson
description: "將模型新增至 ASP.NET Core 中的 Razor Pages 應用程式"
manager: wpickett
ms.author: riande
ms.date: 07/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/model
ms.openlocfilehash: 0ce7693bfdc37d930488304b329dbcd533a5ec1d
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="adding-a-model-to-a-razor-pages-app"></a>將模型新增至 Razor Pages 應用程式

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a>新增資料模型

在方案總管中，以滑鼠右鍵按一下 **RazorPagesMovie** 專案 > [新增] > [新增資料夾]。 將資料夾命名為 *Models*。

以滑鼠右鍵按一下 *Models* 資料夾。 選取 [新增] > [類別]。 將類別命名為 **Movie** 並新增下列屬性：

[!INCLUDE[model 2](../../includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a>新增資料庫連線字串

將連線字串新增到 *appsettings.json* 檔案中。

[!code-json[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a>登錄資料庫內容

以[相依性插入](xref:fundamentals/dependency-injection)容器在 *Startup.cs* 檔案中登錄資料庫內容。

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

請建置專案，以確認您沒有任何錯誤。

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a>新增 Scaffold 工具並執行初始移轉

在本節中，您可以使用套件管理員主控台 (PMC) 進行下列作業：

* 新增 Visual Studio Web 程式碼產生套件。 執行 Scaffolding 引擎需要此套件。
* 新增初始移轉。
* 以初始移轉更新資料庫。

從 [工具] 功能表中，選取 [NuGet 套件管理員] > [套件管理員主控台]。

  ![PMC 功能表](../first-mvc-app/adding-model/_static/pmc.png)

在 PMC 中，輸入下列命令：

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Add-Migration Initial
Update-Database
```

`Install-Package` 命令會安裝執行 Scaffolding 引擎所需的工具。

`Add-Migration` 命令會產生程式碼來建立初始資料庫結構描述。 結構描述是以 `DbContext` (位在 *Models/MovieContext.cs* 檔案中) 中指定的模型為基礎。 `Initial` 引數用來命名移轉。 您可以使用任何名稱，但依照慣例，會選擇描述移轉的名稱。 如需詳細資訊，請參閱[移轉簡介](xref:data/ef-mvc/migrations#introduction-to-migrations)。

`Update-Database` 命令會執行 *Migrations/*\<時間戳記>_InitialCreate.cs 檔案中的 `Up` 方法，以建立資料庫。

[!INCLUDE[model 4windows](../../includes/RP/model4Win.md)]

[!INCLUDE[model 4](../../includes/RP/model4tbl.md)]

<a name="test"></a>
### <a name="test-the-app"></a>測試應用程式

* 執行應用程式，並將 `/Movies` 附加至瀏覽器中的 URL (`http://localhost:port/movies`)。
* 測試 **Create** 連結。

 ![建立頁面](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* 測試 **Edit**、**Details** 和 **Delete** 連結。

如果您收到 SQL 例外狀況，請確認您已執行移轉並更新資料庫：

下一個教學課程說明 Scaffolding 所建立的檔案。

>[!div class="step-by-step"]
[上一步：開始使用](xref:tutorials/razor-pages/razor-pages-start)
[下一步：Scaffold Razor Pages](xref:tutorials/razor-pages/page)    
