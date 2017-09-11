---
title: "在 Mac 上開始使用 ASP.NET Core 中的 Razor 頁面"
author: rick-anderson
description: "利用 Visual Studio for Mac 開始使用 ASP.NET Core 中的 Razor 頁面"
keywords: "ASP.NET Core ,Razor 頁面, Scaffolding, Entity Framework Core, EF, EF Core, 資料庫, mac, macOS, Visual Studio for Mac"
ms.author: riande
manager: wpickett
ms.date: 7/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-mac/razor-pages-start
ms.openlocfilehash: caadc3fcb3bb71abe0773aed4f6ff60a043e3a02
ms.sourcegitcommit: d9ec19e5452af83648074db5d96c0a0f4f9e7f9a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/29/2017
---
# <a name="getting-started-with-razor-pages-in-aspnet-core-with-visual-studio-for-mac"></a>利用 Visual Studio for Mac 開始使用 ASP.NET Core 中的 Razor 頁面

由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供

本教學課程將教導您建置 ASP.NET Core Razor 頁面的 Web 應用程式的基本概念。 建議您先完成 [Razor 頁面的簡介](xref:mvc/razor-pages/index)，再開始本教學課程。 Razor 頁面是在 ASP.NET Core 中建置 Web 應用程式 UI 的建議方式。

## <a name="prerequisites"></a>必要條件

安裝下列項目：

* [.NET Core 2.0.0 SDK](https://dot.net/core) (含) 以上版本
* [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/)

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

按 Ctrl + C 來關閉應用程式。

從 Visual Studio 中，選取 [檔案] > [開啟]，然後選取 *RazorPagesMovie.csproj* 檔案。

### <a name="launch-the-app"></a>啟動應用程式

在 Visual Studio 中，選取 [執行] > [啟動但不偵錯] 來啟動應用程式。 Visual Studio 會啟動 [IIS Express](http://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview)，啟動瀏覽器，然後巡覽至 `http://localhost:5000`。

在下一個教學課程中，我們可以將模型新增至專案。

>[!div class="step-by-step"]
[下一步：新增模型](xref:tutorials/razor-pages-mac/model)
