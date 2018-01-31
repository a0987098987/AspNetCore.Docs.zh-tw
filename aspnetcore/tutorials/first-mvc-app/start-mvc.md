---
title: "ASP.NET Core MVC 與 Visual Studio 使用者入門"
author: rick-anderson
description: "ASP.NET Core MVC 與 Visual Studio 使用者入門"
manager: wpickett
ms.author: riande
ms.date: 10/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: d07d133aa0ed83962b6dc60b9fa0c42993f87843
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio"></a>ASP.NET Core MVC 與 Visual Studio 使用者入門

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[consider RP](../../includes/razor.md)]

本教學課程有 3 個版本：

* macOS：[使用 Visual Studio for Mac 建立 ASP.NET Core MVC 應用程式](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows：[使用 Visual Studio 建立 ASP.NET Core MVC 應用程式](xref:tutorials/first-mvc-app/start-mvc)
* macOS、Linux 和 Windows：[使用 Visual Studio Code 建立 ASP.NET Core MVC 應用程式](xref:tutorials/first-mvc-app-xplat/start-mvc)

## <a name="install-visual-studio-and-net-core"></a>安裝 Visual Studio 和 .NET Core

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

安裝 Visual Studio Community 2017。 選取要下載的社群。 如已安裝 Visual Studio 2017 請跳過此步驟。

* [Visual Studio 2017 首頁的安裝程式](https://www.visualstudio.com/)

執行安裝程式並選取下列工作負載：

* **ASP.NET 與網頁程式開發** (位在 [Web & Cloud] (Web 與雲端) 下)
* **.NET Core 跨平台開發** (位在 [其他工具組] 下)

![**ASP.NET 與網頁程式開發** (位在 [Web & Cloud] (Web 與雲端)**** 下)](start-mvc/_static/web_workload.png)

![**.NET Core 跨平台開發** (位在 [其他工具組]**** 下)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a>建立 Web 應用程式

從 Visual Studio 中，選取 [檔案] > [新增] > [專案]。

![[檔案] > [新增] > [專案]](start-mvc/_static/alt_new_project.png)

完成 [新增專案] 對話方塊：

* 在左窗格中，點選 [.NET Core]。
* 在中央窗格中，點選 [ASP.NET Core Web 應用程式 (.NET Core)]。
* 將專案命名為 "MvcMovie" (請務必將專案命名為 "MvcMovie"，以便複製程式碼時命名空間相符)。
* 點選 [確定]。

![[新增專案] 對話方塊, 左窗格中的 .Net core, ASP.NET Core Web ](start-mvc/_static/new_project2.png)


# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

完成 [新增 ASP.NET Core Web 應用程式 (.NET Core) - MvcMovie] 對話方塊：

* 在版本選取器下拉式清單方塊中，選取 [ASP.NET Core 2.-]。
* 選取 [Web 應用程式(模型檢視控制器)]。
* 點選 [確定]。

![[新增專案] 對話方塊, 左窗格中的 .Net core, ASP.NET Core Web ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

完成 [新增 ASP.NET Core Web 應用程式 (.NET Core) - MvcMovie] 對話方塊：

* 在版本選取器下拉式清單方塊中，點選 [ASP.NET Core 1.1]。
* 點選 [Web 應用程式]
* 保留預設值 [No Authentication] (無驗證)
* 點選 [確定]。

![新的 ASP.NET Core Web 應用程式](start-mvc/_static/p3.png)

---

Visual Studio 在您剛才建立的 MVC 專案中使用了預設範本。 您只要輸入專案名稱，然後選取幾個選項，就立刻會有工作中的應用程式。 這是簡單的入門專案，是個好開始，

點選 **F5** 在偵錯模式中執行應用程式，或 **Ctrl-F5** 在非偵錯模式中執行。
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![執行中的應用程式](start-mvc/_static/1.png)

* Visual Studio 會啟動 [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行您的應用程式。 請注意，位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。 這是因為 `localhost` 是本機電腦的標準主機名稱。 當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。 在上圖中，連接埠號碼為 5000。 瀏覽器中的 URL 顯示`localhost:5000`。 當您執行應用程式時，會看到不同的連接埠編號。
* 使用 **Ctrl + F5** (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。 許多開發人員想要使用非偵錯模式，以便快速啟動應用程式並檢視變更。
* 您可以從 [偵錯] 功能表項目的偵錯或非偵錯模式中啟動應用程式：

![[偵錯] 功能表](start-mvc/_static/debug_menu.png)

* 您可以點選 [IIS Express] 按鈕偵錯應用程式

![IIS Express](start-mvc/_static/iis_express.png)

預設範本提供您作用中的**首頁、關於**和**連絡人**連結。 上圖的瀏覽器不會顯示這些連結。 根據瀏覽器大小，您可能需要按一下巡覽圖示來顯示連結。

![右上角的瀏覽圖示](start-mvc/_static/2.png)

如果您在偵錯模式中執行，請點選 **Shift + F5** 停止偵錯。

在本教學課程的下一個部分中，我們會了解 MVC，並開始撰寫一些程式碼。

>[!div class="step-by-step"]
[下一步](adding-controller.md)  
