---
title: 開始使用 ASP.NET Core 中的 Razor Pages
author: rick-anderson
description: 這一系列的教學課程會示範如何使用 ASP.NET Core 中的 Razor Pages。 了解如何建立模型、產生 Razor Pages 的程式碼、使用 Entity Framework Core 和 SQL Server 進行資料存取、新增搜尋功能、新增輸入驗證以及使用移轉來更新模型。
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 2e1c84a704856a22e1e105f56612194d4bb9c234
ms.sourcegitcommit: 599ebae5c2d6fcb22dfa6ae7d1f4bdfcacb79af4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2018
ms.locfileid: "47210996"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a>教學課程：開始使用 ASP.NET Core 中的 Razor Pages

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range="= aspnetcore-2.0"

我們建議您遵循本教學課程的 ASP.NET Core 2.1。 它**更**容易理解並涵蓋更多功能。 在版本選取器中選取 [ASP.NET Core 2.1]。

![目錄中的版本選取器](razor-pages-start/_static/v21.png)

::: moniker-end

本教學課程將教導您建置 ASP.NET Core Razor Pages之 Web 應用程式的基本概念。 Razor Pages 是在 ASP.NET Core 中建置 Web 應用程式 UI 的建議方式。

本教學課程有 3 個版本：

* Windows：本教學課程
* macOS：[Razor 頁面與 Visual Studio for Mac 使用者入門](xref:tutorials/razor-pages-mac/razor-pages-start)
* macOS、Linux 和 Windows：[Visual Studio Code 中的 ASP.NET Core Razor 頁面使用者入門](xref:tutorials/razor-pages-vsc/razor-pages-start)

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

::: moniker range=">= aspnetcore-2.1"

## <a name="prerequisites"></a>必要條件

[!INCLUDE [Prerequisites](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a>建立 Razor Web 應用程式

* 從 Visual Studio 的 [檔案] 功能表中，選取 [新增] > [專案]。
* 建立新的 ASP.NET Core Web 應用程式。 將專案命名為 **RazorPagesMovie**。 請務必將專案命名為 *RazorPagesMovie*，因此當您複製/貼上程式碼時，名稱空間會相符。
 ![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/np_2.1.png)
* 在下拉式清單中選取 [ASP.NET Core 2.1]，然後選取 [Web 應用程式]。

 ![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/np_2_2.1.png)

Visual Studio 範本會建立入門專案：

![底下提供說明，包括方案總管](razor-pages-start/_static/se2.1.png)

按 **F5** 在偵錯模式中執行應用程式，或按 **Ctrl-F5** 執行而不附加偵錯工具。 選取 [接受] 同意追蹤。 此應用程式不會追踪個人資訊。 範本產生之程式碼所包含的資產有利於滿足[一般資料保護規定 (GDPR)](xref:security/gdpr)。

![Home 或 Index 頁面](razor-pages-start/_static/homeGDPR.png)

下圖顯示接受追蹤之後的應用程式：

![Home 或 Index 頁面](razor-pages-start/_static/home2.1.png)

* Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行應用程式。 位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。 這是因為 `localhost` 是本機電腦的標準主機名稱。 Localhost 只會為來自本機電腦的 Web 要求提供服務。 當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。 在上述影像中，連接埠編號為 5001。 當您執行應用程式時，會看到不同的連接埠編號。
* 使用 **Ctrl + F5** (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。 許多開發人員想要使用非偵錯模式，以便快速啟動應用程式並檢視變更。

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

[!INCLUDE [F7](~/includes/RP/F7.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="prerequisites"></a>必要條件

[!INCLUDE [Prerequisites](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a>建立 Razor Web 應用程式

* 從 Visual Studio 的 [檔案] 功能表中，選取 [新增] > [專案]。
* 建立新的 ASP.NET Core Web 應用程式。 將專案命名為 **RazorPagesMovie**。 請務必將專案命名為 *RazorPagesMovie*，因此當您複製/貼上程式碼時，名稱空間會相符。
  ![新增 ASP.NET Core Web 應用程式](../../razor-pages/index/_static/np.png)
* 在下拉式清單中選取 [ASP.NET Core 2.0]，然後選取 [Web 應用程式]。

  [!INCLUDE [install 2.0](~/includes/dotnetcore-on-dotnetfx-vs.md)]

Visual Studio 範本會建立入門專案：

![底下提供說明，包括方案總管](razor-pages-start/_static/se.png)

按 **F5** 在偵錯模式中執行應用程式，或按 **Ctrl-F5** 執行而不附加偵錯工具。

![Home 或 Index 頁面](razor-pages-start/_static/home.png)

* Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行您的應用程式。 位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。 這是因為 `localhost` 是本機電腦的標準主機名稱。 Localhost 只會為來自本機電腦的 Web 要求提供服務。 當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。 在上述影像中，連接埠編號為 5000。 當您執行應用程式時，會看到不同的連接埠編號。
* 使用 **Ctrl + F5** (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。 許多開發人員想要使用非偵錯模式，以便快速啟動應用程式並檢視變更。

[!INCLUDE [razor-pages-start](~/includes/RP/razor-pages-start.md)]

::: moniker-end

> [!div class="step-by-step"]
> [下一步：新增模型](xref:tutorials/razor-pages/model)
