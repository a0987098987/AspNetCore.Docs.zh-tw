---
title: "使用 ASP.NET Core 和 VS Code 來建立 Web API"
author: rick-anderson
description: "在 macOS、Linux 或 Windows 上，使用 ASP.NET Core MVC 和 Visual Studio Code 建置 Web API"
manager: wpickett
ms.author: riande
ms.date: 09/22/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/web-api-vsc
ms.openlocfilehash: 44566c4014400aa2ca3d512eeaa226637b5f0b97
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-code-on-linux-macos-and-windows"></a>在 macOS、Linux 和 Windows 上，使用 ASP.NET Core MVC 和 Visual Studio Code 建立 Web API

由 [Rick Anderson](https://twitter.com/RickAndMSFT) 和 [Mike Wasson](https://github.com/mikewasson) 提供

在本教學課程中，請建置 Web API 來管理「待辦事項」項目清單。 不會建構 UI。

本教學課程有 3 個版本：

* macOS、Linux、Windows：使用 Visual Studio Code 建立 Web API (本教學課程)
* macOS：[使用 Visual Studio for Mac 建立 Web API](xref:tutorials/first-web-api-mac)
* Windows：[使用 Visual Studio for Windows 建立 Web API](xref:tutorials/first-web-api)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="set-up-your-development-environment"></a>設定您的開發環境

下載與安裝：
- [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) 或更新版本。
- [Visual Studio Code](https://code.visualstudio.com)
- Visual Studio Code [C# 延伸模組](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)

## <a name="create-the-project"></a>建立專案

從主控台中，執行下列命令：

```console
mkdir TodoApi
cd TodoApi
dotnet new webapi
```

在 Visual Studio Code (VS Code) 中，開啟 *TodoApi* 資料夾，然後選取 *Startup.cs* 檔案。

- 針對下列 [警告] 訊息選取 [是]：「'TodoApi' 中遺漏了建置和偵錯的必要資產。 新增它們嗎？」
- 針對下列 [資訊] 訊息選取 [還原]：「有未解析的相依性」。

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![VS Code 與警告：'TodoApi' 中遺漏了建置和偵錯的必要資產。 新增它們嗎？ 不再詢問, 現在不要, 是](web-api-vsc/_static/vsc_restore.png)

按 [偵錯] (F5) 以建置並執行程式。 在瀏覽器中，巡覽至 http://localhost:5000/api/values。 此時會顯示下列對話方塊：

`["value1","value2"]`

如需使用 VS Code 的祕訣，請參閱 [Visual Studio Code 說明](#visual-studio-code-help)。

## <a name="add-support-for-entity-framework-core"></a>新增 Entity Framework Core 的支援

在 .NET Core 2.0 中建立新專案會在 *TodoApi.csproj* 檔案中新增 'Microsoft.AspNetCore.All' 提供者。 無須另行安裝 [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) 資料庫提供者。 此資料庫提供者可讓 Entity Framework Core 搭配使用記憶體內部資料庫。

[!code-xml[Main](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]

## <a name="add-a-model-class"></a>新增模型類別

模型是代表應用程式中資料的物件。 在此情況下，唯一的模型是待辦事項。

新增名為 *Models* 的資料夾。 您可以將模型類別放在專案中的任何位置，但依照慣例會使用 *Models* 資料夾。

使用下列程式碼新增 `TodoItem` 類別：

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

此資料庫會在建立 `TodoItem` 時產生 `Id`。

## <a name="create-the-database-context"></a>建立資料庫內容

「資料庫內容」是為指定的資料模型協調 Entity Framework 功能的主要類別。 若要建立此類別，您可以從 `Microsoft.EntityFrameworkCore.DbContext` 類別來衍生。

在 *Models* 資料夾中，新增 `TodoContext` 類別：

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a>新增控制器

在 *Controllers* 資料夾中，建立名為 `TodoController` 的類別。 加入下列程式碼：

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>啟動應用程式

在 VS Code 中，按 F5 啟動應用程式。 巡覽至 http://localhost:5000/api/todo   (剛才所建立的 `Todo` 控制器)。

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a>Visual Studio Code 說明

- [快速入門](https://code.visualstudio.com/docs)
- [偵錯](https://code.visualstudio.com/docs/editor/debugging)
- [整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [鍵盤快速鍵](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [Mac 鍵盤快速鍵](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [Linux 鍵盤快速鍵](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [Windows 鍵盤快速鍵](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]


