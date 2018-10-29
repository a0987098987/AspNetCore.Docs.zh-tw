---
title: Visual Studio Code 中的 ASP.NET Core Razor 頁面使用者入門
author: rick-anderson
description: 了解使用 Visual Studio Code 建置 ASP.NET Core Razor 頁面之 Web 應用程式的基本概念。
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: 9ea66134c524a6a1a670d55bae4e66cf38a45274
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/25/2018
ms.locfileid: "50089848"
---
# <a name="get-started-with-aspnet-core-razor-pages-in-visual-studio-code"></a>Visual Studio Code 中的 ASP.NET Core Razor 頁面使用者入門

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

本教學課程將教導您建置 ASP.NET Core Razor 頁面之 Web 應用程式的基本概念。 建議您先完成 [Razor 頁面的簡介](xref:razor-pages/index)，再開始本教學課程。 Razor 頁面是在 ASP.NET Core 中建置 Web 應用程式 UI 的建議方式。

## <a name="prerequisites"></a>必要條件

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-razor-web-app"></a>建立 Razor Web 應用程式

從終端機中，執行下列命令：

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

上述命令使用 [.NET Core CLI](/dotnet/core/tools/dotnet) 來建立和執行 Razor 頁面專案。 將瀏覽器開啟到 http://localhost:5000，以檢視應用程式。

![Home 或 Index 頁面](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a>開啟專案

按 Ctrl + C 以關閉應用程式。

從 Visual Studio Code (VS Code)，選取 [檔案] > [開啟資料夾]，然後選取 *RazorPagesMovie* 資料夾。

- 針對下列 [警告] 訊息選取 [是]：「'RazorPagesMovie' 中遺漏了建置和偵錯的必要資產。 新增它們嗎？」
- 選取 [還原] 至 [資訊] 訊息「有未解析的相依性」。

### <a name="launch-the-app"></a>啟動應用程式

(按 Ctrl+F5 即可啟動應用程式而不偵錯)。 從 [偵錯] 功能表中，選取 [啟動但不偵錯]。

在下一個教學課程中，我們可以將模型新增至專案。 

> [!div class="step-by-step"]
> [下一步：新增模型](xref:tutorials/razor-pages-vsc/model)  
