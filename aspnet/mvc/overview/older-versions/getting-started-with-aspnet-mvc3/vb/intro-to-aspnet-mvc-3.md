---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
title: ASP.NET MVC 3 (VB) 簡介 |Microsoft Docs
author: Rick-Anderson
description: 本教學課程將教導您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，也就是 ASP.NET MVC Web 應用程式的基本概念...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: a1b3d789-93b4-487f-b90d-80c9c9b4f8fa
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: 1a870f10d39980335c7acb4a543907fecd408eb2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372349"
---
<a name="intro-to-aspnet-mvc-3-vb"></a>ASP.NET MVC 3 (VB) 簡介
====================
藉由[Rick Anderson](https://github.com/Rick-Anderson)

> 本教學課程將教導您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，也就是 Microsoft Visual Studio 的免費版本的 ASP.NET MVC Web 應用程式的基本概念。 在開始之前，請確定您已安裝符合下列先決條件。 您可以安裝所有人都按下列連結： [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。 或者，您可以個別安裝的必要條件，使用下列連結：
> 
> - [Visual Studio Web Developer Express SP1 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（執行階段 + 工具支援）
> 
> 如果您使用 Visual Studio 2010 而不 Visual Web Developer 2010，請按一下下列連結安裝必要的： [Visual Studio 2010 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。
> 
> 使用本主題隨附了 VB.NET 原始程式碼的 Visual Web Developer 專案。 [下載 VB.NET 版本](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)。 如果您偏好 C#，切換至[C# 版本](../cs/intro-to-aspnet-mvc-3.md)本教學課程。


本教學課程將教導您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，也就是 Microsoft Visual Studio 的免費版本的 ASP.NET MVC Web 應用程式的基本概念。 在開始之前，請確定您已安裝符合下列先決條件。 您可以安裝所有人都按下列連結： [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。 或者，您可以個別安裝的必要條件，使用下列連結：

- [Visual Studio Web Developer Express SP1 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（執行階段 + 工具支援）

如果您使用 Visual Studio 2010 而不 Visual Web Developer 2010，請按一下下列連結安裝必要的： [Visual Studio 2010 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。

使用本主題隨附了 VB 原始程式碼的 Visual Web Developer 專案。 [下載 VB 版本](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=14824)。 如果您偏好 CSharp，切換至[CSharp 版本](../cs/intro-to-aspnet-mvc-3.md)本教學課程。

## <a name="what-youll-build"></a>您將建置

您將實作簡單的電影清單應用程式可支援建立、 編輯和列出資料庫中的電影。 以下是兩個螢幕擷取畫面，您將建置的應用程式。 它包含顯示資料庫中的電影清單的頁面：

[![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image2.png)](intro-to-aspnet-mvc-3/_static/image1.png)

應用程式也可讓您新增、 編輯和刪除電影，以及查看個別的項目相關的詳細資料。 所有的輸入資料的案例包括驗證，以確保儲存在資料庫中的資料正確無誤。

[![CreateFormSo](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="skills-youll-learn"></a>您將學習到的技能

以下是您將學到什麼：

- 如何建立新的 ASP.NET MVC 專案
- 如何建立新的資料庫，使用 Entity Framework code first
- 如何建立 ASP.NET MVC 控制器和檢視
- 如何擷取及顯示資料
- 如何編輯資料，並啟用資料驗證

## <a name="getting-started"></a>快速入門

執行 Visual Web Developer 2010 Express (簡稱為 「 VWD") 來啟動，然後選取**新的專案**從**開始**頁面。

Visual Web Developer 是 IDE 或整合式的開發環境。 就像您使用 Microsoft Word 來撰寫文件時，您將使用 IDE 來建立應用程式。 在 Visual Web Developer 中，沒有工具列上方顯示各種可用的選項給您。 另外還有一個功能表，可讓您在 IDE 中執行工作的另一個辦法。 (例如，您可以使用功能表並選取檔案 &gt; 新專案，而不是從啟動 頁面上選取新的專案.)

[![](intro-to-aspnet-mvc-3/_static/image6.png)](intro-to-aspnet-mvc-3/_static/image5.png)

## <a name="creating-your-first-application"></a>建立第一個應用程式

您可以建立使用您選擇的 Visual Basic 或 Visual C# 程式設計語言的應用程式。 本教學課程中，選取 Visual Basic 在左邊，然後選取**ASP.NET MVC 3 Web 應用程式**。 命名您的專案"為 MvcMovie"，然後按一下**確定**。

![1NewMVCproj_sm](intro-to-aspnet-mvc-3/_static/image7.png)

在 **新的 ASP.NET MVC 3 專案**對話方塊中，選取**網際網路應用程式**。 離開**Razor**做為預設檢視引擎。

![1InternetAppRazor_SM](intro-to-aspnet-mvc-3/_static/image8.png)

按一下 [確定 **Deploying Office Solutions**]。 Visual Web Developer 中使用您剛才建立的 ASP.NET MVC 專案的預設範本，因此您有運作中應用程式現在無須執行任何動作 ！ 這是簡單"Hello World ！" 專案和它的啟動您的應用程式的好地方。

[![](intro-to-aspnet-mvc-3/_static/image10.png)](intro-to-aspnet-mvc-3/_static/image9.png)

從 [偵錯] 功能表中，選取 [開始偵錯]。

![](intro-to-aspnet-mvc-3/_static/image11.png)

請注意，若要開始偵錯的鍵盤快速鍵 F5。

F5 會導致 Visual Web Developer，以啟動開發 web 伺服器並執行您的 web 應用程式。 VWD 再啟動瀏覽器，並開啟應用程式的首頁。 通知，指出瀏覽器的網址列`localhost`而不是類似`example.com`。 這是因為`localhost`永遠指向您自己的本機電腦，在此情況下執行您剛建立的應用程式。 當 VWD 執行 web 專案時，專案使用隨機的連接埠。 下圖中的隨機連接埠號碼會是 43246。 您的專案可能會使用不同的連接埠號碼。

![](intro-to-aspnet-mvc-3/_static/image12.png)

根據預設，這個預設範本會提供您兩個頁面來瀏覽和基本的登入頁面。 讓我們變更此應用程式的運作方式，並在程序有點了解 ASP.NET MVC。 關閉瀏覽器，並讓我們變更一些程式碼。

> [!div class="step-by-step"]
> [下一步](adding-a-controller.md)
