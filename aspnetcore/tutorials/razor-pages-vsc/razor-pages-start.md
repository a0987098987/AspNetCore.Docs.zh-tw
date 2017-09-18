---
title: "利用 Visual Studio Code 開始使用 ASP.NET Core 中的 Razor 頁面"
author: rick-anderson
description: "利用 Visual Studio Code 開始使用 ASP.NET Core 中的 Razor 頁面"
keywords: "ASP.NET Core, Razor 頁面, Scaffolding, Entity Framework Core, EF, EF Core, 資料庫, mac, macOS, Visual Studio Code, Code"
ms.author: riande
manager: wpickett
ms.date: 8/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: 096a60dae171ac5dbfa935be4c16e0903d8f5bb3
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/12/2017
---
# <a name="getting-started-with-razor-pages-in-aspnet-core-with-visual-studio-code"></a>利用 Visual Studio Code 開始使用 ASP.NET Core 中的 Razor 頁面

由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供

本教學課程將教導您建置 ASP.NET Core Razor 頁面之 Web 應用程式的基本概念。 建議您先完成 [Razor 頁面的簡介](xref:mvc/razor-pages/index)，再開始本教學課程。 Razor 頁面是在 ASP.NET Core 中建置 Web 應用程式 UI 的建議方式。

## <a name="prerequisites"></a>必要條件

安裝下列項目：

* [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) (含) 以上版本
* [Visual Studio Code](https://code.visualstudio.com)
* VS Code [C# 延伸模組](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 

## <a name="create-a-razor-web-app"></a>建立 Razor Web 應用程式

從終端機中，執行下列命令：

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

上述命令使用 [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) 來建立和執行 Razor 頁面專案。 將瀏覽器開啟到 http://localhost:5000，以檢視應用程式。

![Home 或 Index 頁面](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a>開啟專案

按 Ctrl + C 以關閉應用程式。

從 Visual Studio Code (VS Code)，選取 [檔案] > [開啟資料夾]，然後選取 *RazorPagesMovie* 資料夾。

- 針對下列 [警告] 訊息選取 [是]：「'RazorPagesMovie' 中遺漏了建置和偵錯的必要資產。 新增它們嗎？」
- 選取 [還原] 至 [資訊] 訊息「有未解析的相依性」。

### <a name="launch-the-app"></a>啟動應用程式

(按 Ctrl+F5 即可啟動應用程式而不偵錯)。 從 [偵錯] 功能表中，選取 [啟動但不偵錯]。

在下一個教學課程中，我們可以將模型新增至專案。 

>[!div class="step-by-step"]
[下一步：新增模型](xref:tutorials/razor-pages-vsc/model)  
