---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
title: 建置真實世界的雲端應用程式與 Azure |Microsoft Docs
author: MikeWasson
description: 本電子書會引導您透過模式為基礎的方法，用以建置真實世界的雲端解決方案。 這些模式可套用至開發程序也為...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: accfa16a-ab15-4c26-9ad4-babdc2a77d2e
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
msc.type: authoredcontent
ms.openlocfilehash: eade14bc27e2bface84fb0bdd2f3c5bf8ef28432
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826610"
---
<a name="building-real-world-cloud-apps-with-azure"></a>使用 Azure 建置真實世界的雲端應用程式
====================
藉由[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)

[下載修正此問題的專案](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下載電子書](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> 本電子書會引導您透過模式為基礎的方法，用以建置真實世界的雲端解決方案。 這些模式可套用至開發程序、 架構及程式碼實作。
> 
> 內容以 Scott Guthrie 所開發，由傳遞他在挪威開發人員 Conference (NDC) 在 2013 年 6 月中的簡報為依據 ([第 1 部分](http://vimeo.com/68215538)，[第 2 部分](http://vimeo.com/68215602))，而是在中的 Microsoft Tech Ed 澳洲2013 年 9 月日 ([第 1 部分](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR324)，[第 2 部分](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR325))。 [許多其他](more-patterns-and-guidance.md#acknowledgments)隨後更新和內容從視訊轉換成書面形式。


## <a name="intended-audience"></a>適用對象

Building real-world 定域機組，考慮移轉至雲端，或不熟悉雲端開發的開發人員會發現這裡的最重要的概念和做法，他們需要知道的簡要概觀。 搭配具體範例，以及更深入的資訊的其他資源的每個章節連結，來說明概念。 範例和其他資源連結是以 Microsoft 架構和服務，但說明的原則套用至其他 web 開發架構和雲端環境。

已針對雲端開發人員可能會發現這裡的概念將協助使其更大的成就。 數列中的每一章可以獨立地讀取，因此您可以挑選並選擇您感興趣的主題。

觀賞 Scott Guthrie 的任何人*建置真實世界雲端應用程式與 Azure*展示層和更多詳細資料和更新的資訊會發現以下的想。

<a id="patterns"></a>
## <a name="cloud-development-patterns"></a>雲端開發模式

本電子書會說明十三個建議的雲端開發模式。 「 模式 」 來這裡廣泛表示建議的方法做事： 如何以開發、 設計和撰寫程式碼的雲端應用程式的最佳方式。 這些是可協助您 「 成功的 pit 分為 「 如果您遵循這些索引鍵模式。

- [一切自動化](automate-everything.md)。

    - 您可以使用 指令碼效率最大化，然後重複處理序中的錯誤降至最低。
    - 示範： Azure 管理指令碼。
- [原始檔控制](source-control.md)。 

    - 設定原始檔控制中的分支結構，以加速 DevOps 工作流程。
    - 示範： 將指令碼新增至原始檔控制。
    - 示範： 保留出原始檔控制的敏感性資料。
    - 示範： 在 Visual Studio 中使用 Git。
- [持續整合與傳遞](continuous-integration-and-continuous-delivery.md)。 

    - 自動化組建和與每個原始檔控制簽入的部署。
- [Web 開發最佳作法](web-development-best-practices.md)。 

    - 讓 web 層保持無狀態。
    - 示範： 縮放比例和自動調整 Azure App Service 中的 Web 應用程式中。
    - 避免工作階段狀態。
    - 無法使用 CDN 時，則您可以使用後援 CDN。
    - 使用非同步程式設計模型。
    - 示範： 在 ASP.NET MVC 和 Entity Framework 中的非同步。
- [單一登入](single-sign-on.md)。 

    - Azure Active Directory 簡介。
    - 示範： 建立使用 Azure Active Directory 的 ASP.NET 應用程式。
- [資料儲存體選項](data-storage-options.md)。 

    - 類型的資料存放區。
    - 如何選擇正確的資料存放區。
    - 示範： Azure SQL Database。
- [資料分割策略](data-partitioning-strategies.md)。 

    - 垂直、 水平分割資料或兩者皆可讓您調整規模的關聯式資料庫。
- [非結構化的 blob 儲存體](unstructured-blob-storage.md)。 

    - 使用 blob 服務，在雲端中儲存檔案。
    - 示範： 修正其應用程式中使用 blob 儲存體。
- [設計存留失敗](design-to-survive-failures.md)。 

    - 類型的失敗。
    - 失敗的範圍。
    - 了解 Sla。
- [監視和遙測](monitoring-and-telemetry.md)。 

    - 為什麼您應該同時購買的遙測的應用程式並撰寫您自己的程式碼，來檢測您的應用程式。
    - 適用於 Azure 的示範： New Relic
    - 示範： 修正其應用程式中的記錄程式碼。
    - 示範： 修正其應用程式中的相依性插入。
    - 示範： 在 Azure 中的內建的記錄支援。
- [暫時性錯誤處理](transient-fault-handling.md)。 

    - 使用智慧型的重試/撤退邏輯來減少暫時性失敗的影響。
    - 示範： 重試/退避法在 Entity Framework 6。
- [分散式快取](distributed-caching.md)。 

    - 改善延展性，並使用分散式快取來降低資料庫的交易成本。
- [以佇列為主的工作模式](queue-centric-work-pattern.md)。 

    - 啟用高可用性和鬆散結合 web 和背景工作層，藉以改善延展性。
    - 示範： 修正其應用程式中的 Azure 儲存體佇列。
- [更多雲端應用程式模式和指導方針](more-patterns-and-guidance.md)。
- [附錄︰修正範例應用程式](the-fix-it-sample-application.md)

    - 已知問題
    - 最佳作法
    - 如何下載、 建置、 執行和部署。

這些模式可套用到所有的雲端環境，但我們將使用 Microsoft 技術和服務，例如 Visual Studio、 Team Foundation Service、 ASP.NET 和 Azure 為基礎的範例說明它們。

本章的其餘部分會介紹它修正範例應用程式和 Web 應用程式在 Azure App Service 中執行它修正應用程式的雲端環境中。

<a id="fixit"></a>
## <a name="the-fix-it-sample-application"></a>範例應用程式修正程式

大部分的螢幕擷取畫面和本電子書中所顯示的程式碼範例根據它修正應用程式原本由[Scott Guthrie](https://weblogs.asp.net/scottgu/)示範建議的雲端應用程式開發模式和作法。

![修正此問題的應用程式首頁](introduction/_static/image1.png)

範例應用程式是票證系統的簡單工作項目。 當您需要修正的項目時，您會建立票證，並指派它某人，或其他人可以登入，並查看 指派票證給它們，並標示為已完成的工作完成時的票證。

它是標準的 Visual Studio web 專案。 它建置在 ASP.NET MVC，並使用 SQL Server 資料庫。 它可以在 IIS Express 在本機執行，而且可以部署至 Azure 網站在雲端中執行。 您可以記錄中使用表單驗證和本機資料庫，或使用 Google 等社交提供者。 （稍後我們將也說明如何以 Active Directory 組織帳戶登入。）

![登入頁面](introduction/_static/image2.png)

一旦您登中建立票證、 將它指派給某個成員，以及上傳您想要修正的圖片。

![建立修正它的工作](introduction/_static/image3.png)

![修正此問題建立工作](introduction/_static/image4.png)

您可以追蹤您所建立的工作項目的進度，查看指派給您，檢視票證詳細資料，與標記的項目，為已完成的票證。

這是非常簡單的應用程式，從功能觀點來看，但您會看到如何建置可調整為數百萬位使用者，使其將會復原資料庫失敗與連接終止數目之類的事項。 您也將了解如何建立自動化且靈活的開發工作流程，可讓您輕鬆入門，並讓應用程式更好且更有效率且快速地反覆開發週期。

<a id="waws"></a>
## <a name="web-apps-in-azure-app-service"></a>在 Azure App Service 中的 web 應用程式

修正其應用程式所使用的雲端環境是 azure 的一種我們呼叫網站服務。 這項服務是方法，您可以裝載您自己在 Azure 中的 web 應用程式，而不需要建立 Vm，並進行更新、 安裝和設定 IIS 等。我們在我們的 Vm 上裝載您的網站，並自動提供備份和復原和其他服務，供您。 Web Sites 服務適用於 ASP.NET、 Node.js、 PHP 和 Python。 它可讓您非常快速地使用 Visual Studio、 Web Deploy、 FTP、 Git 或 TFS 部署。 這通常是在幾秒內開始部署的時間與您的更新功能在網際網路上的時間之間。 若要開始，所有免費且在流量增長時，您可以相應增加。

在幕後，Azure App Service 中的 Web 應用程式會提供許多架構的元件和功能，您必須自行建置，如果您要裝載您的 Vm 上使用 IIS 的網站。 一個元件是部署結束點，會自動設定 IIS，並且為您想要執行您的網站的多個 Vm 上安裝您的應用程式。

![部署服務](introduction/_static/image5.png)

當使用者叫用 web 站台時，它們不會直接達到 IIS Vm 時，會經歷[Application Request Routing (ARR)](https://www.iis.net/downloads/microsoft/application-request-routing)負載平衡器。 您可以使用這些項目與您自己的伺服器，但此處的優點是，它們為您自動設定。 使用智慧型的啟發學習法會將帳戶因素，例如工作階段親和性，在 IIS 中的佇列深度，以及 CPU 使用量，每個電腦，以將流量導向至虛擬機器裝載您的網站。

![ARR 負載平衡器](introduction/_static/image6.png)

如果機器已關閉時，Azure 自動提取從循環，會啟動新的 VM 執行個體，而且會開始將流量導向至新的執行個體--所有與您的應用程式不需要停機。

![從機器失敗的自動復原](introduction/_static/image7.png)

這會自動發生。 您要做的就是建立網站及部署您的應用程式，使用 Windows PowerShell、 Visual Studio 中或 Azure 管理入口網站。

如需快速且輕鬆地逐步教學課程示範如何在 Visual Studio 中建立 web 應用程式，並將它部署至 Azure 網站，請參閱 <<c0> [ 開始使用 Azure 和 ASP.NET](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)。

<a id="summary"></a>
## <a name="summary"></a>總結

本簡介提供一份活頁簿將會涵蓋的主題、 螢幕擷取畫面的範例應用程式，並在 Azure App Service 雲端環境中的 Web 應用程式的簡短概觀。 其中一個最大的開發應用程式中，和適用於雲端的優點是，就可以輕鬆地自動化重複性的開發工作，例如建立測試環境，並將您的程式碼部署到它。 如何執行這個動作的主旨[下一步 一章](automate-everything.md)。

## <a name="resources"></a>資源

如需有關本章節涵蓋之主題的詳細資訊，請參閱下列資源。

文件：

- [Web 應用程式在 Azure App Service 中的](https://azure.microsoft.com/services/app-service/web/)。 如需 Web 應用程式的 Azure 文件的入口網站頁面。
- [Web Apps、 雲端服務和 Vm： 使用時機？](https://azure.microsoft.com/documentation/articles/choose-web-site-cloud-service-vm/) 在這一章中所示的 WAWS 只是三種方式，您可以在 Azure 中執行 web 應用程式。 本文說明的三種方式之間的差異，並提供有關如何選擇哪一種適合您案例的指引。 Web Sites 中，例如雲端服務是一項 Azure PaaS 功能。 Vm 是 IaaS 功能。 如需 PaaS 與 IaaS 的說明，請參閱 <<c0> [ 資料選項](data-storage-options.md#paasiaas)一章。

影片：

- [Scott Guthrie 開始步驟 0-什麼是 Azure 雲端作業系統？](https://azure.microsoft.com/documentation/videos/what-is-the-cloud-os-scottgu/)
- [Web Sites 的架構-Stefan schackow](https://azure.microsoft.com/documentation/videos/why-azure-web-sites-plus-architecture/)。
- [Azure Web Sites 內部運作 Nir Mashkowski](https://channel9.msdn.com/Shows/Web+Camps+TV/Windows-Azure-Web-Sites-Internals-with-Nir-Mashkowski)。

> [!div class="step-by-step"]
> [下一步](automate-everything.md)
