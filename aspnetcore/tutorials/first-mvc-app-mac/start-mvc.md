---
title: ASP.NET Core MVC 與 Visual Studio for Mac 使用者入門
author: rick-anderson
description: 了解如何開始使用 ASP.NET Core MVC 與 Visual Studio
ms.author: riande
ms.date: 8/23/2017
uid: tutorials/first-mvc-app-mac/start-mvc
ms.openlocfilehash: e94b9aa6b6c594ae407792387788410f776d4c1d
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272289"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio-for-mac"></a>ASP.NET Core MVC 與 Visual Studio for Mac 使用者入門

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

本教學課程會讓您了解使用 [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/) 建置 ASP.NET Core MVC Web 應用程式的基本知識。 

[!INCLUDE [consider RP](../../includes/razor.md)]

本教學課程有 3 個版本：

* macOS：[使用 Visual Studio for Mac 建置 ASP.NET Core MVC 應用程式](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows：[使用 Visual Studio 建置 ASP.NET Core MVC 應用程式](xref:tutorials/first-mvc-app/start-mvc)
* Linux、macOS 和 Windows：[使用 Visual Studio Code 建置 ASP.NET Core MVC 應用程式](xref:tutorials/first-mvc-app-xplat/start-mvc)

## <a name="prerequisites"></a>必要條件

[!INCLUDE [](~/includes/net-core-prereqs-macos.md)]

## <a name="create-a-web-app"></a>建立 Web 應用程式

從 Visual Studio 中，選取 [檔案] > [新增方案]。

![macOS 新增方案](../first-web-api-mac/_static/sln.png)

選取 [.NET Core 應用程式] > [ASP.NET Core] > [Web 應用程式] > [下一步]。

![macOS [新增專案] 對話方塊](start-mvc/1.png)

將專案命名為 **MvcMovie**，然後選取 [建立]。

![macOS [新增專案] 對話方塊](start-mvc/2.png)

### <a name="launch-the-app"></a>啟動應用程式

在 Visual Studio 中，選取 [執行] > [啟動但不偵錯] 來啟動應用程式。 Visual Studio 會啟動 [Kestrel](xref:fundamentals/servers/index#kestrel)，再啟動瀏覽器並瀏覽至 `http://localhost:port`，其中 *port* 是隨機選擇的通訊埠編號。

![含有新專案的瀏覽器](start-mvc/b1.png)

* 位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。 這是因為 `localhost` 是本機電腦的標準主機名稱。 當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。 當您執行應用程式時，會看到不同的連接埠編號。
* 您可以從 [執行] 功能表的偵錯或非偵錯模式中啟動應用程式。

預設範本提供您**首頁、關於**和**連絡人**連結。 上圖的瀏覽器不會顯示這些連結。 根據瀏覽器大小，您可能需要按一下巡覽圖示來顯示連結。

![含有新專案的瀏覽器](start-mvc/b2.png)

在本教學課程的下一個部分中，您會了解 MVC，並開始撰寫一些程式碼。

> [!div class="step-by-step"]
> [下一步](adding-controller.md)  
