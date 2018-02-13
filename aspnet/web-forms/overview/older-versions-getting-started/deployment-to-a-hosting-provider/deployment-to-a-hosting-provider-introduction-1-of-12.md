---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
title: "部署 ASP.NET Web 應用程式與 SQL Server Compact 使用 Visual Studio： 簡介-12 個 1 |Microsoft 文件"
author: tdykstra
description: "這一系列的教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式專案，其中包含 SQL Server Compact 資料庫使用視覺化 Stu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a2d7f33b-8c4a-4b48-9fb1-9139cf9b9878
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
msc.type: authoredcontent
ms.openlocfilehash: a0f38c83bd9231dbd37d3d505c90316af521b336
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/12/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-introduction---1-of-12"></a>部署 ASP.NET Web 應用程式與 SQL Server Compact 使用 Visual Studio： 簡介-12 個 1
====================
由[Tom Dykstra](https://github.com/tdykstra)

[下載入門專案](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> 這一系列的教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式專案，其中包含使用 Visual Studio 2012 RC 或 Visual Studio Express 2012 RC for Web 的 SQL Server Compact 資料庫。 如果您安裝 Web 發行更新，您也可以使用 Visual Studio 2010。
> 
> 顯示部署 Visual Studio 2012 RC 發行之後，引進的功能，示範如何將 SQL Server Compact 以外的 SQL Server 版本的部署和示範如何將部署至 Azure App Service Web 應用程式的教學課程，請參閱[ASP.NET Web 部署使用 Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md)。
> 
> 這些教學課程將引導您完成部署第一次測試，在本機開發電腦上的 IIS 和協力廠商主機服務提供者。 應用程式資料庫及 ASP.NET 成員資格資料庫，則會使用您要部署的應用程式。 您開始使用 SQL Server Compact 和 SQL Server Compact，部署和更新版本的教學課程示範如何部署資料庫變更，以及如何移轉到 SQL Server。
> 
> 教學課程假設您知道如何使用 Visual Studio 中的 ASP.NET。 如果不這麼做，是很好的起點是[基本的 ASP.NET Web Form 教學課程](../tailspin-spyworks/tailspin-spyworks-part-1.md)或[基本 ASP.NET MVC 的教學課程](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)。
> 
> 如果您有與本教學課程不直接相關的問題，您可以將它們來公佈[ASP.NET 部署論壇](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment)。


## <a name="overview"></a>總覽

這些教學課程將引導您完成部署第一次測試，在本機開發電腦上的 IIS 和協力廠商主機服務提供者。 應用程式資料庫及 ASP.NET 成員資格資料庫，則會使用您要部署的應用程式。 您開始使用 SQL Server Compact 和 SQL Server Compact，部署和更新版本的教學課程示範如何部署資料庫變更，以及如何移轉到 SQL Server。

數字的教學課程 – 11 中所有加上疑難排解頁面-可能會導致部署程序似乎鉅的任務。 事實上，部署網站的基本程序便會產生相對較小的教學課程集的一部分。 不過，在真實世界的情況下，您通常需要部署的一些小型但重要的額外層面相關資訊，例如，設定目標伺服器上的資料夾權限。 我們包含了許多這些其他技術在教學課程中，而且希望讓教學課程不可能阻礙您成功部署實際的應用程式的資訊。

教學課程專為在執行順序，和每個組件為基礎的前述組件。 不過，您可以略過組件不是適用於您的情況。 （跳過部分可能需要您調整程序之後的教學課程中）。

## <a name="intended-audience"></a>適用對象

教學課程的目標是 ASP.NET 開發人員在小型組織或其他環境中工作在其中：

- 不使用持續整合程序 （自動化的組建和部署）。
- 在實際執行環境是協力廠商主機服務提供者。
- 一個人通常會填滿 （相同人員開發、 測試和部署） 的多個角色。

在企業環境中，它是較為典型來實作連續整合程序，而且生產環境通常由公司自己的伺服器。 不同的人通常也會執行不同的角色。 企業部署的相關資訊，請參閱[企業案例中部署 Web 應用程式](../../deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md)。

各種規模組織數量也可以部署至 Azure 時，web 應用程式和大部分的這些教學課程所示範的程序也適用於 Azure 應用程式服務 Web 應用程式。 如需 Azure 的簡介，請參閱[https://azure.microsoft.com](https://azure.microsoft.com)。

## <a name="the-hosting-provider-shown-in-the-tutorials"></a>教學課程所示範的主控提供者

教學課程會引導您進行設定與管理公司帳戶，並部署至該主控提供者應用程式的程序。 特定的控管公司選擇使教學課程可以說明將部署到即時網站的完整的經驗。 每個控管公司提供的不同功能和部署其伺服器的經驗有所不同。不過，本教學課程所述的程序的整體程序通常會。

本教學課程，Cytanium.com，所使用的主控提供者是其中一個許多可供使用，而且其用途，在本教學課程不會構成作背書或推薦。

## <a name="deploying-web-site-projects"></a>部署的網站專案

Contoso 大學是 Visual Studio web 應用程式專案。 大部分的部署方法，在本教學課程所示範的工具不會套用到[網站專案](https://msdn.microsoft.com/library/dd547590.aspx)。 如需如何部署網站專案的資訊，請參閱[ASP.NET 部署內容地圖](https://msdn.microsoft.com/library/bb386521.aspx#deployment_for_web_site_projects)。

## <a name="deploying-aspnet-mvc-projects"></a>部署 ASP.NET MVC 專案

本教學課程中，您部署為 ASP.NET Web Form 專案，但了解如何執行的所有項目也會是適用於 ASP.NET MVC。 Visual Studio MVC 專案是只是另一種 web 應用程式專案。 唯一的差別在於，如果您要部署至主機服務提供者不支援 ASP.NET MVC 或它的目標版本，您必須確定您已安裝適當的 ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0)或[MVC 4](http://nuget.org/packages/aspnetmvc)) 在您的專案中的 NuGet 套件。

## <a name="programming-language"></a>程式設計語言

範例應用程式使用 C# 教學課程不需要的 C# 中，知識但的教學課程所示的部署技術不因語言而異。

## <a name="troubleshooting-during-this-tutorial"></a>在本教學課程疑難排解

在部署期間，就會發生錯誤時，或已部署的站台無法正確執行，錯誤訊息不一定會提供解決方案。 幫助您進行一些常見的問題案例，[疑難排解參考頁面](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)可用。 如果您收到錯誤訊息，或當您瀏覽教學課程，項目無法運作，請務必檢查 [疑難排解] 頁面。

## <a name="comments-welcome"></a>註解歡迎畫面

教學課程的註解是 褖畫惎，且致力更新本教學課程時進行納入帳戶修正或改善教學課程的註解中所提供的建議。

## <a name="prerequisites"></a>必要條件

開始之前，請確定您有 Windows 7 或更新版本，而且您的電腦上安裝其中一項產品的：

- [Visual Studio 2010 SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
- [Visual Web Developer Express 2010 SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VWD2010SP1Pack)
- [Visual Studio 2012 RC or Visual Studio Express 2012 RC for Web](https://go.microsoft.com/fwlink/?LinkId=240162)

如果您有 Visual Studio 2010 SP1 或 Visual Web Developer Express 2010 SP1，也安裝下列產品：

- [Azure SDK for.NET (VS 2010 SP1)](https://go.microsoft.com/fwlink/?LinkID=208120) （包括 Web 發佈更新）
- [Microsoft Visual Studio 2010 SP1 Tools for SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCEVSTools)

一些其他軟體，才能完成此教學課程中，但您不需要有尚未載入。 本教學課程將引導您在需要時安裝它的步驟。

## <a name="downloading-the-sample-application"></a>下載範例應用程式

您要部署應用程式名為 Contoso 大學，並已為您建立。 它是 university 網站上，根據 Contoso 大學應用程式中所述的鬆散的簡化的版本[ASP.NET 網站上的 Entity Framework 教學課程](https://asp.net/entity-framework/tutorials)。

當您已安裝的必要條件時，下載[Contoso 大學 web 應用程式](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)。 *.Zip*檔案包含多個版本的專案和 PDF 檔案，其中包含所有的 12 教學課程。 若要逐步教學課程的步驟，請使用 ContosoUniversity 開始啟動。 若要查看專案的教學課程結尾處的起來，開啟 ContosoUniversity 結束。 若要查看專案移轉至完整的 SQL Server 在教學課程 10 之前的外觀，請開啟 ContosoUniversity AfterTutorial09。

若要準備逐步教學課程的步驟，將儲存到您使用 Visual Studio 專案所使用的任何資料夾的 ContosoUniversity 開始。 根據預設，這會是下列資料夾：

`C:\Users\<username>\Documents\Visual Studio 2012\Projects`

(在本教學課程螢幕擷取畫面中，針對專案資料夾位於根目錄中`C`： 磁碟機。)

啟動 Visual Studio、 開啟專案，然後按 CTRL + F5 來執行它。

[![Home_page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image2.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image1.png)

從功能表列存取網站頁面，並可讓您執行下列功能：

- 顯示學生統計資料 （關於頁面）。
- 顯示、 編輯、 刪除和新增學生。
- 顯示和編輯的課程。
- 顯示和編輯講師。
- 顯示和編輯的部門。

以下是少數代表性的頁面的螢幕擷取畫面。

[![Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image4.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image3.png)

[![Add_Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image6.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image5.png)

## <a name="reviewing-application-features-that-affect-deployment"></a>檢閱會影響部署的應用程式功能

應用程式的下列功能會影響您部署的方式，或您必須將其部署執行。 每一種是數列中的下列教學課程中所述更多詳細資料。

- Contoso 大學使用 SQL Server Compact 資料庫儲存應用程式資料，例如學生和講師名稱。 資料庫包含混合了測試資料和實際執行資料和您部署至實際執行時要排除的測試資料。 稍後在教學課程系列中您會從 SQL Server Compact 到 SQL Server 移轉。
- 應用程式會使用 ASP.NET 成員資格系統，將使用者帳戶資訊儲存在 SQL Server Compact 資料庫。 應用程式定義的系統管理員使用者，有一些限制的資訊的存取權。 您要部署的成員資格資料庫，不具測試帳戶，但具有一個系統管理員帳戶。
- 因為應用程式資料庫和成員資格資料庫做為 SQL Server Compact 資料庫引擎，您必須部署資料庫引擎主控提供者，以及資料庫本身。
- 應用程式會使用 ASP.NET 通用的成員資格提供者，讓成員資格系統可以將其資料儲存在 SQL Server Compact 資料庫。 包含通用成員資格提供者的組件必須部署與應用程式。
- 應用程式會使用 Entity Framework 5.0 存取應用程式資料庫中的資料。 包含 Entity Framework 5.0 的組件必須部署與應用程式。
- 應用程式會使用第三方錯誤記錄和報告公用程式。 必須與應用程式部署的組件中提供這個公用程式。
- 錯誤記錄公用程式將錯誤資訊寫入 XML 檔案中的檔案資料夾。 您必須確定 ASP.NET 中已部署的站台執行的帳戶具有寫入權限到此資料夾，而且您必須從部署中排除此資料夾。 （否則從測試環境的錯誤記錄檔資料可能會部署到生產環境和/或生產環境錯誤記錄檔可能已刪除。）
- 應用程式包含必須變更某些設定中已部署*Web.config*根據目的地環境 （測試或生產），以及根據組建必須變更其他設定檔組態 （偵錯或發行）。
- Visual Studio 方案可包含的類別庫專案。 應該部署此專案會產生的組件，而非專案本身。

在此數列中的第一個教學課程，您已下載範例 Visual Studio 專案，並檢閱影響您如何部署應用程式的網站功能。 下列教學課程中，在您準備進行部署的下列步驟來自動處理某些設定。 其他您以手動方式處理。

>[!div class="step-by-step"]
[下一步](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
