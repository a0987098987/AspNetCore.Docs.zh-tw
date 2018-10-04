---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
title: ASP.NET MVC 3 (C#) 簡介 |Microsoft Docs
author: Rick-Anderson
description: 本教學課程將教導您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，也就是 ASP.NET MVC Web 應用程式的基本概念...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 86a80b35-88bd-4b7c-bd58-f6e7997197d4
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: 53306318ab1a782d3605876aac53ec903d053269
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2018
ms.locfileid: "48576283"
---
<a name="intro-to-aspnet-mvc-3-c"></a>ASP.NET MVC 3 (C#) 簡介
====================
藉由[Rick Anderson]((https://twitter.com/RickAndMSFT))

> > [!NOTE]
> > 本教學課程中的更新的版本可[此處](../../../getting-started/introduction/getting-started.md)使用 ASP.NET MVC 5 和 Visual Studio 2013。 它更安全、 更容易遵循，並示範更多的功能。
> 
> 
> 本教學課程將教導您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，也就是 Microsoft Visual Studio 的免費版本的 ASP.NET MVC Web 應用程式的基本概念。 在開始之前，請確定您已安裝符合下列先決條件。 您可以安裝所有人都按下列連結： [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。 或者，您可以個別安裝的必要條件，使用下列連結：
> 
> - [Visual Studio Web Developer Express SP1 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（執行階段 + 工具支援）
> 
> 如果您使用 Visual Studio 2010 而不 Visual Web Developer 2010，請按一下下列連結安裝必要的： [Visual Studio 2010 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。
> 
> 使用本主題隨附了 C# 原始程式碼的 Visual Web Developer 專案。 [下載 C# 版本](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)。 如果您偏好 Visual Basic，切換到[Visual Basic 版本](../vb/intro-to-aspnet-mvc-3.md)本教學課程。


## <a name="what-youll-build"></a>您將建置

您將實作簡單的電影清單應用程式可支援建立、 編輯和列出資料庫中的電影。 以下是兩個螢幕擷取畫面，您將建置的應用程式。 它包含顯示資料庫中的電影清單的頁面：

![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image1.png)

應用程式也可讓您新增、 編輯和刪除電影，以及查看個別的項目相關的詳細資料。 所有的輸入資料的案例包括驗證，以確保儲存在資料庫中的資料正確無誤。

![](intro-to-aspnet-mvc-3/_static/image2.png)

## <a name="skills-youll-learn"></a>您將學習到的技能

以下是您將學到什麼：

- 如何建立新的 ASP.NET MVC 專案。
- 如何建立 ASP.NET MVC 控制器和檢視。
- 如何建立新的資料庫，使用 Entity Framework Code First 開發架構。
- 如何擷取及顯示資料。
- 如何編輯資料，並啟用資料驗證。

## <a name="getting-started"></a>快速入門

啟動執行 Visual Web Developer 2010 Express ("Visual Web Developer"簡稱)，並選取**新的專案**從**開始**頁面。

Visual Web Developer 是 IDE 或整合式的開發環境。 就像您使用 Microsoft Word 來撰寫文件時，您將使用 IDE 來建立應用程式。 在 Visual Web Developer 中，沒有工具列上方顯示各種可用的選項給您。 另外還有一個功能表，可讓您在 IDE 中執行工作的另一個辦法。 (例如，您可以使用功能表並選取檔案 &gt; 新專案，而不是從啟動 頁面上選取新的專案.)

[![](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="creating-your-first-application"></a>建立第一個應用程式

您可以建立使用 Visual Basic 或 Visual C# 程式設計語言的應用程式。 選取 Visual C# 在左邊，然後選取**ASP.NET MVC 3 Web 應用程式**。 命名您的專案"為 MvcMovie"，然後按一下**確定**。 (如果您偏好 Visual Basic，切換到[Visual Basic 版本](../vb/intro-to-aspnet-mvc-3.md)本教學課程。)

![](intro-to-aspnet-mvc-3/_static/image5.png)

在 **新的 ASP.NET MVC 3 專案**對話方塊中，選取**網際網路應用程式**。 請檢查**使用 HTML5 標記**並保留**Razor**做為預設檢視引擎。

![](intro-to-aspnet-mvc-3/_static/image6.png)

按一下 [確定 **Deploying Office Solutions**]。 Visual Web Developer 中使用您剛才建立的 ASP.NET MVC 專案的預設範本，因此您有運作中應用程式現在無須執行任何動作 ！ 這是簡單"Hello World ！" 專案和它的啟動您的應用程式的好地方。

[![](intro-to-aspnet-mvc-3/_static/image8.png)](intro-to-aspnet-mvc-3/_static/image7.png)

從 [偵錯] 功能表中，選取 [開始偵錯]。

![](intro-to-aspnet-mvc-3/_static/image9.png)

請注意，若要開始偵錯的鍵盤快速鍵 F5。

F5 會導致 Visual Web Developer，以啟動開發 web 伺服器並執行您的 web 應用程式。 然後 visual Web Developer 會啟動瀏覽器，並開啟應用程式的首頁。 通知，指出瀏覽器的網址列`localhost`而不是類似`example.com`。 這是因為`localhost`永遠指向您自己的本機電腦，在此情況下執行您剛建立的應用程式。 Visual Web Developer 中執行時 web 專案，會將隨機的連接埠用於 web 伺服器。 下圖中的隨機連接埠號碼會是 43246。 當您執行應用程式時，您可能會看到不同的連接埠號碼。

![](intro-to-aspnet-mvc-3/_static/image10.png)

現成這個預設範本可讓您瀏覽的兩個頁面和基本的登入頁面。 下一個步驟是要變更此應用程式的運作方式，並在程序有點了解 ASP.NET MVC。 關閉瀏覽器，並讓我們變更一些程式碼。

> [!div class="step-by-step"]
> [下一步](adding-a-controller.md)
