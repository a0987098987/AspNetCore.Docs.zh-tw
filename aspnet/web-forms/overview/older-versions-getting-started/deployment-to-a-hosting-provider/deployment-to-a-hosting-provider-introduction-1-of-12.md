---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
title: 使用 SQL Server Compact 使用 Visual Studio 將 ASP.NET Web 應用程式部署： 簡介-1，12 個 |Microsoft Docs
author: tdykstra
description: 這一系列的教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式專案，其中包含 SQL Server Compact 資料庫，使用 Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: a2d7f33b-8c4a-4b48-9fb1-9139cf9b9878
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
msc.type: authoredcontent
ms.openlocfilehash: 9dacafaacdab12b8005cb6073647ae526cefcfb4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831401"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-introduction---1-of-12"></a>使用 SQL Server Compact 使用 Visual Studio 將 ASP.NET Web 應用程式部署： 簡介-1，12 個
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

[下載入門專案](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> 這一系列的教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式專案使用 Visual Studio 2012 RC 或 Visual Studio Express 2012 RC for Web 包含 SQL Server Compact 資料庫。 如果您安裝 Web Publish Update，您也可以使用 Visual Studio 2010。
> 
> 顯示 Visual Studio 2012 RC 版本之後引入的部署功能，示範如何部署 SQL Server Compact，以外的 SQL Server 版本，並示範如何部署至 Azure App Service Web Apps 的教學課程，請參閱[ASP.NET Web 部署使用 Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md)。
> 
> 這些教學課程會引導您完成第一次部署到測試，在本機開發電腦上的 IIS，然後到協力廠商主機服務提供者。 您要部署的應用程式會使用應用程式資料庫和 ASP.NET 成員資格資料庫。 您開始使用 SQL Server Compact，並將部署到 SQL Server Compact，並稍後的教學課程說明如何部署資料庫變更，以及如何移轉至 SQL Server。
> 
> 教學課程假設您知道如何使用 Visual Studio 中的 ASP.NET。 如果不這麼做，是良好起點[基本的 ASP.NET Web Form 教學課程](../tailspin-spyworks/tailspin-spyworks-part-1.md)或是[基本的 ASP.NET MVC 教學課程](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)。
> 
> 如果您有不直接相關的教學課程中的問題，您可以張貼他們[ASP.NET 部署論壇](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment)。


## <a name="overview"></a>總覽

這些教學課程會引導您完成第一次部署到測試，在本機開發電腦上的 IIS，然後到協力廠商主機服務提供者。 您要部署的應用程式會使用應用程式資料庫和 ASP.NET 成員資格資料庫。 您開始使用 SQL Server Compact，並將部署到 SQL Server Compact，並稍後的教學課程說明如何部署資料庫變更，以及如何移轉至 SQL Server。

數字的教學課程-在所有的 11 加上疑難排解的頁面-可能會讓部署程序看起來很令人怯步。 事實上，部署站台的基本程序會產生相對較小的教學課程集的一部分。 不過，在真實世界的情況下，您通常需要部署的某些小型但重要的額外方面的相關資訊 — 例如，在目標伺服器上設定資料夾權限。 我們包含了許多這些額外的技巧在教學課程中，希望使教學課程不可能阻礙您成功部署實際的應用程式的資訊。

教學課程專為依序執行，以及每個部分是根據前一個部分。 不過，您可以略過與您的情況不相關的組件。 （略過組件可能需要您調整程序中稍後的教學課程）。

## <a name="intended-audience"></a>適用對象

教學課程的目標是 ASP.NET 開發人員在小型組織或其他環境中工作在其中：

- 不使用持續整合程序 （自動化的組建和部署）。
- 生產環境是協力廠商主機服務提供者。
- 一個人通常會填滿 （同一人開發、 測試，以及部署） 的多個角色。

在企業環境中更為常見實作持續整合程序，而實際執行環境通常由公司自己的伺服器。 不同的人通常也會執行不同的角色。 企業部署的相關資訊，請參閱[企業案例中部署 Web 應用程式](../../deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md)。

各種規模的組織也可以部署至 Azure web 應用程式，並在這些教學課程中所示的程序的大部分也適用於 Azure 應用程式服務 Web 應用程式。 Azure 簡介，請參閱 < [ https://azure.microsoft.com ](https://azure.microsoft.com)。

## <a name="the-hosting-provider-shown-in-the-tutorials"></a>在教學課程中所顯示的主機服務提供者

教學課程會引導您設定具有控管公司的帳戶和應用程式部署到該主機服務提供者的程序。 特定的控管公司選擇是讓教學課程可以說明部署到即時網站的完整體驗。 每個代管公司提供不同的功能，並部署至伺服器的經驗有所不同;不過，本教學課程中所述的程序是典型的整個程序。

用於此教學課程中，Cytanium.com，主機服務提供者是許多可供使用，且其使用在本教學課程不會構成作背書或推薦。

## <a name="deploying-web-site-projects"></a>部署網站專案

Contoso 大學是 Visual Studio web 應用程式專案。 大部分的部署方法和工具在本教學課程示範並不適用於[網站專案](https://msdn.microsoft.com/library/dd547590.aspx)。 如需如何部署網站專案的詳細資訊，請參閱[ASP.NET 部署內容對應](https://msdn.microsoft.com/library/bb386521.aspx#deployment_for_web_site_projects)。

## <a name="deploying-aspnet-mvc-projects"></a>部署 ASP.NET MVC 專案

在本教學課程中，您部署的 ASP.NET Web Form 專案，但您將了解如何進行的所有項目也會適用於 ASP.NET MVC。 Visual Studio MVC 專案是另一種 web 應用程式專案。 唯一的差別在於，如果您要部署至不支援 ASP.NET MVC 或您的目標版本，它的主控提供者，您必須確定您已安裝適當的 ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0)或[MVC 4](http://nuget.org/packages/aspnetmvc)) 在專案中的 NuGet 套件。

## <a name="programming-language"></a>程式設計語言

範例應用程式會使用 C# 教學課程不需要知道 C# 中，但這些教學課程所示的部署技術不因語言而異。

## <a name="troubleshooting-during-this-tutorial"></a>針對本教學課程期間進行疑難排解

在部署期間，發生錯誤時，或已部署的站台無法正確執行，錯誤訊息不一定會提供解決方案。 若要利用一些常見的問題案例，協助您[疑難排解參考頁面](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)可用。 如果您收到錯誤訊息，或當您瀏覽教學課程，項目無法運作，請務必檢查 [疑難排解] 頁面。

## <a name="comments-welcome"></a>註解歡迎畫面

教學課程的註解是 褖畫惎，且致力更新本教學課程時進行納入帳戶修正或改善教學課程的註解中所提供的建議。

## <a name="prerequisites"></a>必要條件

在開始之前，請確定您有 Windows 7 或更新版本，且您的電腦上安裝其中一個下列產品：

- [Visual Studio 2010 SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
- [Visual Web Developer Express 2010 SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VWD2010SP1Pack)
- [Visual Studio 2012 RC 或 Visual Studio Express 2012 RC for Web](https://go.microsoft.com/fwlink/?LinkId=240162)

如果您有 Visual Studio 2010 SP1 或 Visual Web Developer Express 2010 SP1，請也安裝下列產品：

- [Azure SDK for.NET (VS 2010 SP1)](https://go.microsoft.com/fwlink/?LinkID=208120) （包括 Web Publish Update）
- [Microsoft Visual Studio 2010 SP1 Tools for SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCEVSTools)

其他一些軟體，才能完成教學課程中，但您不需要有尚未載入。 本教學課程將引導您逐步完成在需要時，請安裝它。

## <a name="downloading-the-sample-application"></a>下載範例應用程式

您會將部署應用程式名為 Contoso 大學，並已為您建立。 它是一個簡化的版的大學網站上，根據 Contoso 大學應用程式中所述的鬆散[Entity Framework ASP.NET 網站上的教學課程](https://asp.net/entity-framework/tutorials)。

當您已安裝的必要條件時，下載[Contoso 大學 web 應用程式](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)。 *.Zip*檔案包含多個版本的專案和 PDF 檔案包含所有的 12 教學課程。 若要完成本教學課程的步驟，開始 ContosoUniversity 開始。 若要查看專案的教學課程結尾處的外觀，開啟 ContosoUniversity 端。 若要查看專案移轉至完整的 SQL Server 在教學課程中 10 之前的外觀，開啟 ContosoUniversity AfterTutorial09。

若要準備使用教學課程的步驟，將儲存到您使用 Visual Studio 專案所使用的任何資料夾的 ContosoUniversity 開始。 根據預設，這會是下列資料夾：

`C:\Users\<username>\Documents\Visual Studio 2012\Projects`

(在本教學課程螢幕擷取畫面，如 [專案] 資料夾位於根目錄中`C`： 磁碟機。)

啟動 Visual Studio 開啟專案，然後按 CTRL-F5 來執行它。

[![Home_page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image2.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image1.png)

網站頁面，可從功能表列存取，並可讓您執行下列功能：

- 顯示學生統計資料 （[關於] 頁面）。
- 顯示、 編輯、 刪除和新增學生。
- 顯示和編輯的課程。
- 顯示和編輯講師。
- 顯示和編輯的部門。

以下是一些具代表性的頁面的螢幕擷取畫面。

[![Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image4.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image3.png)

[![Add_Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image6.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image5.png)

## <a name="reviewing-application-features-that-affect-deployment"></a>檢閱會影響部署的應用程式功能

應用程式的下列功能會影響您部署的方式，或您必須手動將它部署。 每一種是更詳細說明在下列教學課程系列中。

- Contoso 大學使用 SQL Server Compact 資料庫來儲存應用程式資料，例如 student 和 instructor 的名稱。 資料庫包含混合的測試資料和實際執行資料，並部署至生產環境時需要排除測試資料。 稍後在教學課程系列中您會從 SQL Server Compact 到 SQL Server 移轉。
- 應用程式會使用 ASP.NET 成員資格系統，將使用者帳戶資訊儲存在 SQL Server Compact 資料庫。 應用程式會定義具有某些限制的資訊的存取權的系統管理員使用者。 您要部署的成員資格資料庫，而不需要測試帳戶，但具有一個系統管理員帳戶。
- 因為應用程式資料庫和成員資格資料庫做為 SQL Server Compact 資料庫引擎，您必須部署資料庫引擎主控提供者，以及資料庫本身。
- 應用程式會使用 ASP.NET 通用的成員資格提供者，讓您的成員資格系統可以將其資料儲存在 SQL Server Compact 資料庫。 包含通用的成員資格提供者的組件必須部署與應用程式。
- 應用程式會使用 Entity Framework 5.0 來存取應用程式資料庫中的資料。 包含 Entity Framework 5.0 的組件必須部署與應用程式。
- 應用程式會使用第三方錯誤記錄和報告公用程式。 必須與應用程式一起部署的組件中提供此公用程式。
- 錯誤記錄公用程式會在檔案資料夾，將錯誤資訊寫入 XML 檔案中。 您必須確定此資料夾中，已部署之網站中執行 ASP.NET 帳戶具有寫入權限，而且您不必從部署中排除此資料夾。 （否則錯誤記錄檔資料，從測試環境可能會部署到生產環境和/或生產錯誤記錄檔可能會被刪除。）
- 應用程式包含必須變更某些設定中部署*Web.config*取決於目的地環境 （測試或生產），以及必須根據組建變更其他設定檔案組態 （偵錯或發行）。
- Visual Studio 方案包含的類別庫專案。 此專案會產生的組件應該部署，而不是專案本身。

在此系列中的第一個教學課程，您已下載的範例 Visual Studio 專案，並檢閱會影響您如何部署應用程式的網站功能。 在下列教學課程中，您準備進行部署設定自動處理這些事項。 其他您手動處理。

> [!div class="step-by-step"]
> [下一步](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
