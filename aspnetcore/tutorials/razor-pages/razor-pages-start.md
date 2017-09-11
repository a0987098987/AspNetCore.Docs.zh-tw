---
title: "開始使用 ASP.NET Core 中的 Razor 頁面"
author: rick-anderson
description: "開始使用 ASP.NET Core 中的 Razor 頁面"
keywords: "ASP.NET Core, Razor 頁面, Razor, MVC"
ms.author: riande
manager: wpickett
ms.date: 8/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: c22ee2554992d15df2f6b92ee5da6805ab80b73a
ms.sourcegitcommit: d9ec19e5452af83648074db5d96c0a0f4f9e7f9a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/29/2017
---
# <a name="getting-started-with-razor-pages-in-aspnet-core"></a>開始使用 ASP.NET Core 中的 Razor 頁面

由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供

本教學課程將教導您建置 ASP.NET Core Razor 頁面之 Web 應用程式的基本概念。 建議您先完成 [Razor 頁面的簡介](xref:mvc/razor-pages/index)，再開始本教學課程。 Razor 頁面是在 ASP.NET Core 中建置 Web 應用程式 UI 的建議方式。

## <a name="prerequisites"></a>必要條件

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="create-a-razor-web-app"></a>建立 Razor Web 應用程式

* 從 Visual Studio 的 [檔案]  功能表中，選取 [新增] > [專案] 。
* 建立新的 ASP.NET Core Web 應用程式。 將專案命名為 **RazorPagesMovie**。 請務必將專案命名為 *RazorPagesMovie*，因此當您複製/貼上程式碼時，名稱空間會相符。
 ![新增 ASP.NET Core Web 應用程式](../../mvc/razor-pages/index/_static/np.png)
* 在下拉式清單中選取 [ASP.NET Core 2.0]，然後選取 [Web 應用程式]。
 ![Web 應用程式 (Razor 頁面)](../../mvc/razor-pages/index/_static/np2.png)

Visual Studio 範本會建立入門專案：

![底下提供說明，包括方案總管](razor-pages-start/_static/se.png)

按 **F5** 在偵錯模式中執行應用程式，或按 **Ctrl-F5** 執行而不附加偵錯工具。

![Home 或 Index 頁面](razor-pages-start/_static/home.png)

* Visual Studio 會啟動 [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行您的應用程式。 位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。 這是因為 `localhost` 是本機電腦的標準主機名稱。 Localhost 只會為來自本機電腦的 Web 要求提供服務。 當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。 在上述影像中，連接埠編號為 5000。 當您執行應用程式時，會看到不同的連接埠編號。
* 使用 **Ctrl + F5** (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。 許多開發人員想要使用非偵錯模式，以便快速啟動應用程式並檢視變更。

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

>[!div class="step-by-step"]
[下一步：新增模型](xref:tutorials/razor-pages/modelz)  
