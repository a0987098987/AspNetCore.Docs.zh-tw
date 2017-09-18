---
title: "使用 ASP.NET Core 和 Visual Studio for Windows 建立 Web API"
author: rick-anderson
description: "使用 ASP.NET Core MVC 和 Visual Studio for Windows 建置 Web API"
keywords: "ASP.NET Core, WebAPI, Web API, REST, HTTP, 服務, HTTP 服務"
ms.author: riande
manager: wpickett
ms.date: 8/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-web-api
ms.openlocfilehash: 4aab61c7ee4498b33a4ea8bbec6033ce9828e2af
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/12/2017
---
#<a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a>使用 ASP.NET Core 和 Visual Studio for Windows 建立 Web API

由 [Rick Anderson](https://twitter.com/RickAndMSFT) 和 [Mike Wasson](https://github.com/mikewasson) 提供

在本教學課程中，您將建置 Web API 來管理「待辦事項」項目清單， 而不會建置 UI。

本教學課程有 3 個版本：

* Windows：使用 Visual Studio for Windows 建立 Web API (本教學課程)
* macOS：[使用 Visual Studio for Mac 建立 Web API](xref:tutorials/first-web-api-mac)
* macOS、Linux、Windows：[使用 Visual Studio Code 建立 Web API](xref:tutorials/web-api-vsc)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a>必要條件

[!INCLUDE[install 2.0](../includes/install2.0.md)]

若要了解 ASP.NET Core 1.1 版，請參閱[此 PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-web-api/_static/_webAPI.pdf)。

## <a name="create-the-project"></a>建立專案

從 Visual Studio 中，選取 [檔案] 功能表 > [新增] > [專案]。

選取 [ASP.NET Core Web 應用程式 (.NET Core)] 專案範本。 將專案命名為 `TodoApi`，然後按一下 [確定]。

![[新增專案] 對話方塊](first-web-api/_static/new-project.png)

在 [New ASP.NET Core Web Application - TodoApi] (新增 ASP.NET Core Web 應用程式 - TodoApi) 對話方塊中，選取 [Web API] 範本。 選取 [確定]。 請**勿**選取 [Enable Docker Support] (啟用 Docker 支援)。

![已從 ASP.NET Core 範本中選取 Web API 專案範本的 [新增 ASP.NET Web 應用程式] 對話方塊](first-web-api/_static/web-api-project.png)

### <a name="launch-the-app"></a>啟動應用程式

在 Visual Studio 中，按 CTRL + F5 來啟動應用程式。 Visual Studio 會啟動瀏覽器並巡覽至 `http://localhost:port/api/values`，其中 *port* 是隨機選擇的通訊埠編號。 Chrome、 Edge 和 Firefox 顯示下列項目：

```
["value1","value2"]
``` 

### <a name="add-a-model-class"></a>新增模型類別

模型是代表應用程式中資料的物件。 在此情況下，唯一的模型是待辦事項。

新增名為 "Models" 的資料夾。 在方案總管中，以滑鼠右鍵按一下專案。 選取 [新增] > [新增資料夾]。 將資料夾命名為 *Models*。

注意：模型類別可位於專案中的任何位置，但依照慣例會使用 *Models* 資料夾。

新增 `TodoItem` 類別。 以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增] > [類別]。 將類別命名為 `TodoItem`，然後選取 [新增]。

使用下列程式碼取代產生的程式碼：

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

此資料庫會在建立 `TodoItem` 時產生 `Id`。

### <a name="create-the-database-context"></a>建立資料庫內容

「資料庫內容」是為指定的資料模型協調 Entity Framework 功能的主要類別。 此類別是透過衍生自 `Microsoft.EntityFrameworkCore.DbContext` 類別來建立。

新增 `TodoContext` 類別。 以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增] > [類別]。 將類別命名為 `TodoContext`，然後選取 [新增]。

使用下列程式碼取代產生的程式碼：

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a>新增控制器

在方案總管中，以滑鼠右鍵按一下 *Controllers* 資料夾。 選取 [新增] > [新增項目]。 在 [新增項目] 對話方塊中，選取 [Web API 控制器類別] 範本。 將類別命名為 `TodoController` 。

![在搜尋方塊中輸入 controller 且已選取 Web API 控制器的 [新增項目] 對話方塊](first-web-api/_static/new_controller.png)

使用下列程式碼取代產生的程式碼：

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]
  
### <a name="launch-the-app"></a>啟動應用程式

在 Visual Studio 中，按 CTRL + F5 來啟動應用程式。 Visual Studio 會啟動瀏覽器並巡覽至 `http://localhost:port/api/values`，其中 *port* 是隨機選擇的通訊埠編號。 如果您使用的是 Chrome、Edge 或 Firefox，就會顯示資料。 如果您使用的是 IE，IE 會提示您開啟或儲存 *values.json* 檔案。 巡覽至剛才建立 `http://localhost:port/api/todo` 的 `Todo` 控制器。

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]

