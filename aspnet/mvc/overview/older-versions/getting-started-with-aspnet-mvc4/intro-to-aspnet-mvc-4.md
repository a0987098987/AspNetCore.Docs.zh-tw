---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
title: ASP.NET MVC 4 簡介 |Microsoft Docs
author: Rick-Anderson
description: 如果本教學課程，可在此處使用 Visual Studio 2013 更新的版本。 新的教學課程會使用 ASP.NET MVC 5，可提供許多增強功能，透過 t...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: ed66530a-04d5-49eb-b76a-85be1f57c437
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: ea3d1517192ded0e5372c49897bb1fec33324b6f
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912393"
---
<a name="intro-to-aspnet-mvc-4"></a>ASP.NET MVC 4 簡介
====================
藉由[Rick Anderson]((https://twitter.com/RickAndMSFT))

> 如果本教學課程中可用的更新的版本[此處](../../getting-started/introduction/getting-started.md)使用[Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)。 新的教學課程會使用 ASP.NET MVC 5，透過本教學課程提供許多增強功能。
>
> 本教學課程將教導您建置使用 Microsoft ASP.NET MVC 4 Web 應用程式的基本概念[Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express)或 Visual Web Developer 2010 Express Service Pack 1。 Visual Studio 2012，建議您不需要安裝任何項目才能完成本教學課程。 如果您使用 Visual Studio 2010，您必須安裝下列元件。 您可以安裝所有人都依序按一下下列連結：
>
> - [Visual Studio Web Developer Express SP1 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 4 的 WPI 安裝程式](https://go.microsoft.com/fwlink/?LinkId=243392)
> - [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0)
> - [SSDT](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx)
>
> 如果您使用 Visual Studio 2010 而不 Visual Web Developer 2010，請安裝[WPI ASP.NET MVC 4 的安裝程式](https://go.microsoft.com/fwlink/?LinkId=243392)而： [Visual Studio 2010 的必要元件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
>
> 使用本主題隨附了 C# 原始程式碼的 Visual Web Developer 專案。 [下載 C# 版本](https://code.msdn.microsoft.com/Intro-to-ASPNET-MVC-4-61d0219d/file/114480/1/MvcMovie.zip)。
>
> 在本教學課程中，您可以執行應用程式在 Visual Studio 中。 您也可以提供應用程式透過網際網路將它部署至主機服務提供者。 Microsoft 提供免費的 web 裝載中的最多 10 個 web sites[免費的 Windows Azure 試用帳戶](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604)。 如需有關如何將 Visual Studio web 專案部署至 Windows Azure 網站的資訊，請參閱 <<c0> [ 建立及部署 ASP.NET 網站和 SQL Database，使用 Visual Studio](https://docs.microsoft.com/dotnet/azure/)。 該教學課程也會示範如何使用 Entity Framework Code First Migrations，將 SQL Server 資料庫部署到 Windows Azure SQL Database (先前稱為 SQL Azure)。
>
> 本教學課程中所編寫的 Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )。


## <a name="what-youll-build"></a>您將建置

> [!NOTE]
> 如果本教學課程中可用的更新的版本[此處](../../getting-started/introduction/getting-started.md)使用[Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)。 新的教學課程會使用 ASP.NET MVC 5，透過本教學課程提供許多增強功能。


您將實作簡單的電影清單應用程式可支援建立、 編輯、 搜尋和列出資料庫中的電影。 以下是兩個螢幕擷取畫面，您將建置的應用程式。 它包含顯示資料庫中的電影清單的頁面：

![](intro-to-aspnet-mvc-4/_static/image1.png)

應用程式也可讓您新增、 編輯和刪除電影，以及查看個別的項目相關的詳細資料。 所有的輸入資料的案例包括驗證，以確保儲存在資料庫中的資料正確無誤。

![](intro-to-aspnet-mvc-4/_static/image2.png)

## <a name="getting-started"></a>快速入門

從執行 Visual Studio Express 2012 或 Visual Web Developer 2010 Express 開始。 大部分的螢幕擷取畫面，在此系列使用 Visual Studio Express 2012，但您可以完成本教學課程，Visual Studio 2010 SP1 中，Visual Studio 2012、 Visual Studio Express 2012 或 Visual Web Developer 2010 Express。 選取 **新的專案**從**開始**頁面。

Visual Studio 是 IDE 或整合式的開發環境。 就像您使用 Microsoft Word 來撰寫文件時，您將使用 IDE 來建立應用程式。 在 Visual Studio 沒有工具列上方顯示各種可用的選項給您。 另外還有一個功能表，可讓您在 IDE 中執行工作的另一個辦法。 (例如，您可以使用功能表並選取檔案 &gt; 新專案，而不是從啟動 頁面上選取新的專案.)

![](intro-to-aspnet-mvc-4/_static/image3.png)

## <a name="creating-your-first-application"></a>建立第一個應用程式

您可以建立使用 Visual Basic 或 Visual C# 程式設計語言的應用程式。 選取 Visual C# 在左邊，然後選取**ASP.NET MVC 4 Web 應用程式**。 命名您的專案&quot;MvcMovie&quot; ，然後按一下**確定**。

![](intro-to-aspnet-mvc-4/_static/image4.png)

在 **新的 ASP.NET MVC 4 專案**對話方塊中，選取**網際網路應用程式**。 離開**Razor**做為預設檢視引擎。

![](intro-to-aspnet-mvc-4/_static/image5.png)

按一下 [確定 **Deploying Office Solutions**]。 Visual Studio 使用您剛才建立的 ASP.NET MVC 專案預設範本，因此您有運作中應用程式現在無須執行任何動作 ！ 這是一項簡單&quot;Hello World ！&quot;專案和它的啟動您的應用程式的好地方。

![](intro-to-aspnet-mvc-4/_static/image6.png)

從 [偵錯] 功能表中，選取 [開始偵錯]。

![](intro-to-aspnet-mvc-4/_static/image7.png)

請注意，若要開始偵錯的鍵盤快速鍵 F5。

F5 會導致 Visual Studio 啟動 IIS Express，並執行您的 web 應用程式。 Visual Studio 接著會啟動瀏覽器，並開啟應用程式的首頁。 通知，指出瀏覽器的網址列`localhost`而不是類似`example.com`。 這是因為`localhost`永遠指向您自己的本機電腦，在此情況下執行您剛建立的應用程式。 Visual Studio 執行時 web 專案，會將隨機的連接埠用於 web 伺服器。 下圖中的連接埠號碼會是 41788。 當您執行應用程式時，您可能會看到不同的連接埠號碼。

![](intro-to-aspnet-mvc-4/_static/image8.png)

現成這個預設範本可讓您住家、 連絡人和相關頁面。 它也提供支援，以註冊和登入，並連結至 Facebook 和 Twitter。 下一個步驟是要變更此應用程式的運作方式有點了解 ASP.NET MVC。 關閉瀏覽器，並讓我們變更一些程式碼。

> [!div class="step-by-step"]
> [下一步](adding-a-controller.md)
