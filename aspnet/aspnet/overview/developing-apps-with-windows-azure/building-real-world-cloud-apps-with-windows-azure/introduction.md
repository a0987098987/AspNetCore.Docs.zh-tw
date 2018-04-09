---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
title: 建立真實世界雲端應用程式與 Azure |Microsoft 文件
author: MikeWasson
description: 這個電子書引領您完成模式為基礎的方法來建置實際雲端解決方案。 在開發程序以及與模式適用於...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: accfa16a-ab15-4c26-9ad4-babdc2a77d2e
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
msc.type: authoredcontent
ms.openlocfilehash: 5a62818a2dc21128bb0a42a8b296ade460e7b060
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="building-real-world-cloud-apps-with-azure"></a>建立真實世界雲端應用程式與 Azure
====================
由[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)

[下載修正專案](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下載電子書](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> 這個電子書引領您完成模式為基礎的方法來建置實際雲端解決方案。 模式適用於開發程序以及架構和編碼方式。
> 
> 內容為基礎的簡報 Scott Guthrie 所開發並由他在挪威開發人員會議 (NDC) 中傳遞的 2013 年 6 月 ([第 1 部分](http://vimeo.com/68215538)，[第 2 部分](http://vimeo.com/68215602))，而是在 Microsoft 技術 Ed 澳洲中2013 年 9 月日 ([第 1 部分](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR324)，[第 2 部分](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR325))。 [許多其他](more-patterns-and-guidance.md#acknowledgments)更新和增強內容時從視訊來書寫形式轉換它。


## <a name="intended-audience"></a>適用對象

對於雲端開發感到好奇考慮移至雲端，或為新雲端開發人員會發現以下的最重要的概念和做法，他們需要知道的簡要概觀。 概念說明具體範例，與其他資源，如需進一步資訊每個章節連結。 範例和其他資源連結適用於 Microsoft 架構及服務，但是所闡述的原則套用至其他 web 開發架構和雲端部署環境。

已經雲端開發人員可能會發現想法此處可協助使其更順利。 可以獨立地讀取數列中的每一章，讓您可以挑選並選擇您感興趣的主題。

監看 Scott Guthrie 的任何人*建置真實世界雲端應用程式與 Azure*展示和要有更多詳細資料和更新的資訊會發現這裡。

<a id="patterns"></a>
## <a name="cloud-development-patterns"></a>雲端開發模式

這個電子書說明十三個建議的雲端開發模式。 「 」 中使用模式這裡的廣泛的意義上表示執行動作的建議的方式： 如何將進行開發、 設計和撰寫程式碼的雲端應用程式。 這些是可協助您 」 都會歸類為成功的 pit 「 如果您遵循這些重要模式。

- [一切自動化](automate-everything.md)。

    - 使用指令碼效率最大化，並減少重複性的程序中的錯誤。
    - 示範： Azure 管理指令碼。
- [原始檔控制](source-control.md)。 

    - 設定原始檔控制中的分支結構，以促進 DevOps 工作流程。
    - 示範： 在原始檔控制中加入指令碼。
    - 示範： 保留出原始檔控制的敏感性資料。
    - 示範： 使用 Visual Studio 中的 Git。
- [持續整合與傳遞](continuous-integration-and-continuous-delivery.md)。 

    - 自動化建置和部署與每個原始檔控制簽入。
- [Web 開發最佳作法](web-development-best-practices.md)。 

    - Web 層保持無狀態的。
    - 示範： 調整和 Azure App Service Web 應用程式中的自動調整。
    - 避免工作階段狀態。
    - 無法使用 CDN 時，請使用後援 CDN。
    - 使用非同步程式設計模型。
    - 示範： 在 ASP.NET MVC 和 Entity Framework 中的非同步。
- [單一登入](single-sign-on.md)。 

    - Azure Active Directory 的簡介。
    - 示範： 建立使用 Azure Active Directory 的 ASP.NET 應用程式。
- [資料儲存體選項](data-storage-options.md)。 

    - 類型的資料存放區。
    - 如何選擇正確的資料存放區。
    - 示範： Azure SQL Database。
- [資料分割策略](data-partitioning-strategies.md)。 

    - 垂直、 水平資料分割或兩者來協助調整關聯式資料庫。
- [非結構化的 blob 儲存體](unstructured-blob-storage.md)。 

    - 藉由使用 blob 服務將檔案儲存在雲端中。
    - 示範： 使用 blob 儲存體中修正該應用程式。
- [設計存留失敗](design-to-survive-failures.md)。 

    - 類型的錯誤。
    - 失敗的範圍。
    - 了解 Sla。
- [監控與遙測](monitoring-and-telemetry.md)。 

    - 為什麼您應該同時購買的遙測應用程式並撰寫您自己的程式碼來檢測您的應用程式。
    - 示範： New Relic for Azure
    - 示範： 記錄程式碼中修正該應用程式。
    - 示範： 相依性插入它修正應用程式中。
    - 示範： 在 Azure 中的內建的記錄支援。
- [暫時性錯誤處理](transient-fault-handling.md)。 

    - 使用智慧的重試/撤退邏輯來減少暫時性失敗的影響。
    - 示範： 重試/撤退 Entity Framework 6 中。
- [分散式快取](distributed-caching.md)。 

    - 改善延展性，並使用分散式快取可減少資料庫的交易成本。
- [佇列為主的工作模式](queue-centric-work-pattern.md)。 

    - 啟用高可用性，並透過鬆散結合的 web 和背景工作層提升可調適性。
    - 示範： 修正它應用程式中的 Azure 儲存體佇列。
- [多個雲端應用程式模式和指引](more-patterns-and-guidance.md)。
- [附錄︰修正範例應用程式](the-fix-it-sample-application.md)

    - 已知問題
    - 最佳作法
    - 如何下載、 建置、 執行及部署。

這些模式套用到所有的雲端環境中，但我們將說明這些點使用的 Microsoft 技術和服務，例如 Visual Studio、 Team Foundation Service、 ASP.NET 和 Azure 為基礎的範例。

本指南的這個餘數導入了修正它範例應用程式和 Web 應用程式中執行它修正應用程式的 Azure App Service 雲端環境中。

<a id="fixit"></a>
## <a name="the-fix-it-sample-application"></a>修正它範例應用程式

大部分的螢幕擷取畫面和此的電子書中所顯示的程式碼範例根據它修正應用程式原本是由開發[Scott Guthrie](https://weblogs.asp.net/scottgu/)示範建議的雲端應用程式開發模式和作法。

![修正應用程式首頁](introduction/_static/image1.png)

範例應用程式是票證系統簡單的工作項目。 當您需要修復的功能時，您建立票證，並指派它某人，和其他人可以登入，並查看票證指派給它們，並將票證標示為已完成時完成的工作。

它是標準的 Visual Studio web 專案。 它建置在 ASP.NET MVC，並使用 SQL Server 資料庫。 它可以在 IIS Express 在本機執行，而且可以部署至 Azure 網站雲端中執行。 您可以記錄中使用表單驗證和本機資料庫，或使用例如 Google 社交提供者。 （稍後我們將也示範如何在 Active Directory 組織帳戶登入。）

![登入頁面](introduction/_static/image2.png)

一旦您登入中建立票證、 將它指派給某人，和上傳您想要獲得修正的圖片。

![建立修正它的工作](introduction/_static/image3.png)

![修正此問題建立工作](introduction/_static/image4.png)

您可以追蹤您所建立的工作項目的進度，查看指派給您、 檢視票證的詳細資訊，以及標記項目，為已完成的票證。

這是非常簡單的應用程式功能的觀點而言，但您會看到如何建置它，使其可以擴充至數百萬名使用者，然後將彈性資料庫失敗與連接終止數目等事項。 您也會看到如何建立自動化和敏捷式軟體開發的開發工作流程，可讓您能夠啟動簡單和更佳及更好讓應用程式有效率地快速地逐一查看的開發週期。

<a id="waws"></a>
## <a name="web-apps-in-azure-app-service"></a>Azure App Service 中的 web 應用程式

修正它應用程式使用的雲端環境是 azure 的一種，我們會呼叫 Web Sites 服務。 此服務是方法，您可以裝載自己在 Azure 中的 web 應用程式，而不必建立 Vm，再進行更新、 安裝和設定 IIS，依此類推。我們在我們的 Vm 上裝載您的網站，自動提供備份和復原和其他服務。 Web Sites 服務使用 ASP.NET、 Node.js、 PHP 和 Python。 它可讓您非常快速地使用 Visual Studio、 Web Deploy、 FTP、 Git 或 TFS 部署。 它通常是在幾秒內啟動部署的時間更新可在網際網路上的時間之間。 若要開始，所有完全免費，您可以在您的流量增加而向上延展。

在幕後，Azure App Service 中的 Web 應用程式會提供許多架構的元件和功能，您必須自行建置，如果您要裝載您自己的 Vm 上使用 IIS 的網站。 一個元件是部署結束點，會自動設定 IIS，並且您想要執行您的網站的許多 Vm 上安裝應用程式。

![部署服務](introduction/_static/image5.png)

當使用者叫用的網站時，他們不直接叫用 IIS Vm、 會經歷[Application Request Routing (ARR)](https://www.iis.net/downloads/microsoft/application-request-routing)負載平衡器。 您可以使用這些與您自己的伺服器，但此處的優點是，它們為您自動設定。 使用智慧的啟發學習法會將帳戶因素，例如工作階段親和性，在 IIS 中的佇列深度，以及 CPU 使用量，在每個機器 Vm 的流量引導至裝載您的網站。

![ARR 負載平衡器](introduction/_static/image6.png)

如果電腦關閉時，Azure 會自動提取從循環，產生新的 VM 執行個體，而且會開始將新的執行個體--而不需要停機應用程式的所有流量導向。

![從機器失敗的自動復原](introduction/_static/image7.png)

這會自動發生。 您只需要是建立網站和部署應用程式，使用 Windows PowerShell、 Visual Studio 中或在 Azure 管理入口網站。

如需快速且輕鬆逐步教學課程示範如何在 Visual Studio 中建立 web 應用程式，並將它部署至 Azure 網站，請參閱[開始使用 Azure 和 ASP.NET](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)。

<a id="summary"></a>
## <a name="summary"></a>總結

本簡介提供一份活頁簿將涵蓋的主題、 螢幕擷取畫面的範例應用程式，並在雲端環境中 Azure App Service Web 應用程式的簡短概觀。 開發應用程式中，以及雲端的最大的優點之一是，所以可以輕鬆地自動化重複的開發工作，例如建立測試環境，並將您的程式碼部署至它。 如何執行這個動作的主旨[下一章](automate-everything.md)。

## <a name="resources"></a>資源

如需有關本章節涵蓋之主題的詳細資訊，請參閱下列資源。

文件集：

- [Web 應用程式在 Azure App Service 中的](https://azure.microsoft.com/services/app-service/web/)。 如需 Web 應用程式的 Azure 文件入口網站頁面。
- [Web 應用程式、 雲端服務和 Vm： 使用時機？](https://azure.microsoft.com/documentation/articles/choose-web-site-cloud-service-vm/) 如本章所示的 WAWS-DEV.JSON 只是三種方式，您可以在 Azure 中執行 web 應用程式。 本文說明的三種方式之間的差異，並提供有關如何選擇哪一種最適合您案例的指引。 Web Sites 中，例如雲端服務是 Azure PaaS 功能。 Vm 是 IaaS 功能。 如需 PaaS 和 IaaS 的說明，請參閱[資料選項](data-storage-options.md#paasiaas)章節。

影片：

- [Scott Guthrie 開始步驟 0-什麼是 Azure 雲端作業系統？](https://azure.microsoft.com/documentation/videos/what-is-the-cloud-os-scottgu/)
- [Web Sites 架構-與 Stefan Schackow](https://azure.microsoft.com/documentation/videos/why-azure-web-sites-plus-architecture/)。
- [Azure Web Sites 內部項目與 Nir Mashkowski](https://channel9.msdn.com/Shows/Web+Camps+TV/Windows-Azure-Web-Sites-Internals-with-Nir-Mashkowski)。

> [!div class="step-by-step"]
> [下一步](automate-everything.md)
