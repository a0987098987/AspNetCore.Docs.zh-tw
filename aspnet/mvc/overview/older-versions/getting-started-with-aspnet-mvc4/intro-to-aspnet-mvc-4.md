---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
title: "ASP.NET MVC 4 簡介 |Microsoft 文件"
author: Rick-Anderson
description: "本教學課程是否可在此處使用 Visual Studio 2013 更新的版本。 新的教學課程會使用 ASP.NET MVC 5 提供許多改良 t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: ed66530a-04d5-49eb-b76a-85be1f57c437
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 92d9e583b6c26fa8c928d33e14593d280702a269
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="intro-to-aspnet-mvc-4"></a>ASP.NET MVC 4 簡介
====================
由[Rick Anderson](https://github.com/Rick-Anderson)

> 如果本教學課程中可用的更新的版本[這裡](../../getting-started/introduction/getting-started.md)使用[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)。 新的教學課程會使用 ASP.NET MVC 5，本教學課程提供許多改良。
> 
> 本教學課程將告訴您建置使用 Microsoft ASP.NET MVC 4 Web 應用程式的基本概念[Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express)或 Visual Web Developer 2010 Express Service Pack 1。 Visual Studio 2012，建議您不需要安裝任何項目才能完成本教學課程。 如果您使用 Visual Studio 2010，您必須安裝下列元件。 您可以安裝全部都依序按一下下列連結：
> 
> - [Visual Studio Web Developer Express SP1 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 4 的 WPI 安裝程式](https://go.microsoft.com/fwlink/?LinkId=243392)
> - [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0)
> - [SSDT](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx)
> 
> 如果您使用 Visual Studio 2010 而不 Visual Web Developer 2010，安裝[WPI ASP.NET MVC 4 installer](https://go.microsoft.com/fwlink/?LinkId=243392)和： [Visual Studio 2010 的必要條件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
> 
> 使用本主題隨附在 Visual Web Developer 專案中的使用 C# 原始程式碼。 [下載的 C# 版本](https://code.msdn.microsoft.com/Intro-to-ASPNET-MVC-4-61d0219d/file/114480/1/MvcMovie.zip)。
> 
> 教學課程中您要在 Visual Studio 執行應用程式。 您也可以讓應用程式可透過網際網路將它部署至主機服務提供者。 Microsoft 提供的免費 web 裝載中的最多 10 個 web sites[免費試用版的 Windows Azure 帳戶](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604)。 如需如何部署 Visual Studio web 專案至 Windows Azure 網站的資訊，請參閱[建立及部署 ASP.NET 網站和 Visual Studio 中的 SQL Database](https://docs.microsoft.com/dotnet/azure/)。 該教學課程也會示範如何使用 Entity Framework Code First 移轉將 SQL Server 資料庫部署到 Windows Azure SQL Database (先前稱為 SQL Azure)。
> 
> 本教學課程中所編寫的 Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )。


## <a name="what-youll-build"></a>您將建置

> [!NOTE]
> 如果本教學課程中可用的更新的版本[這裡](../../getting-started/introduction/getting-started.md)使用[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)。 新的教學課程會使用 ASP.NET MVC 5，本教學課程提供許多改良。


您將實作簡單的電影清單應用程式支援建立、 編輯、 搜尋和列出資料庫中的影片。 以下是您將建置的應用程式的兩個螢幕擷取畫面。 它包含顯示之頁面的資料庫中的電影清單：

![](intro-to-aspnet-mvc-4/_static/image1.png)

應用程式也可讓您新增、 編輯和刪除電影，以及查看個別的有關的詳細資料。 所有的資料輸入案例包括驗證，以確保儲存在資料庫中的資料正確無誤。

![](intro-to-aspnet-mvc-4/_static/image2.png)

## <a name="getting-started"></a>快速入門

開始執行 Visual Studio Express 2012 或 Visual Web Developer 2010 Express。 大部分的螢幕擷取畫面中這個數列使用 Visual Studio Express 2012，但您可以完成本教學課程中的使用 Visual Studio 2010 SP1 /、 Visual Studio 2012、 Visual Studio Express 2012 或 Visual Web Developer 2010 Express。 選取**新專案**從**啟動**頁面。

Visual Studio 是一個 IDE 中或整合式的開發環境。 就像您使用 Microsoft Word 來寫入的文件時，您將使用 IDE 來建立應用程式。 在 Visual Studio 沒有工具列上方顯示各種可用的選項給您。 也會提供另一種方式，在 IDE 中執行工作的功能表。 (例如，而不是選取**新的專案**從**啟動** 頁面上，您可以使用功能表並選取**檔案** &gt; **新專案**.)

![](intro-to-aspnet-mvc-4/_static/image3.png)

## <a name="creating-your-first-application"></a>建立第一個應用程式

您可以建立使用 Visual Basic 或 Visual C# 當做程式語言的應用程式。 選取 Visual C# 在左邊，然後選取**ASP.NET MVC 4 Web 應用程式**。 命名您的專案&quot;MvcMovie&quot; ，然後按一下 **確定**。

![](intro-to-aspnet-mvc-4/_static/image4.png)

在**新增 ASP.NET MVC 4 專案**對話方塊中，選取**網際網路應用程式**。 保留**Razor**做為預設檢視引擎。

![](intro-to-aspnet-mvc-4/_static/image5.png)

按一下 [確定 **Deploying Office Solutions**]。 Visual Studio 使用您剛才建立的 ASP.NET MVC 專案的預設範本，因此您有可用的應用程式立即而不做任何事 ！ 這是簡單&quot;Hello World ！&quot;專案和它的啟動您的應用程式的好地方。

![](intro-to-aspnet-mvc-4/_static/image6.png)

從 [偵錯] 功能表中，選取 [開始偵錯]。

![](intro-to-aspnet-mvc-4/_static/image7.png)

請注意，開始偵錯的鍵盤快速鍵 F5。

F5 會導致 Visual Studio 來啟動 IIS Express 並執行您 web 應用程式。 然後，visual Studio 會啟動瀏覽器，並開啟應用程式的首頁。 請注意，該處會指示瀏覽器的網址列`localhost`並不是類似`example.com`。 這是因為`localhost`一律會指向您自己的本機電腦，在此情況下執行您剛才建置的應用程式。 Visual Studio 執行時 web 專案，隨機連接埠用於 web 伺服器。 在下面的影像中的連接埠號碼是 41788。 當您執行應用程式時，您可能會看到不同的通訊埠編號。

![](intro-to-aspnet-mvc-4/_static/image8.png)

現成這個預設範本可讓您家用、 連絡人和相關頁面。 也提供支援註冊和登入，並連結到 Facebook 和 Twitter。 下一個步驟是變更這個應用程式的運作方式，並了解 ASP.NET MVC 的一點。 關閉瀏覽器，並讓我們將變更一些程式碼。

>[!div class="step-by-step"]
[下一步](adding-a-controller.md)
