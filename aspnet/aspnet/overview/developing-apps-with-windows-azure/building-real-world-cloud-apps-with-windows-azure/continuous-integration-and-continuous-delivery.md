---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
title: 持續整合與持續傳遞 （使用 Azure 建置真實世界的雲端應用程式） |Microsoft Docs
author: MikeWasson
description: 建置真實世界雲端應用程式與 Azure 的電子書是以 Scott Guthrie 所開發的簡報為依據。 它說明 13 模式與做法，他可以...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: eaece9f5-f80c-428b-b771-5db66d275b7d
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
msc.type: authoredcontent
ms.openlocfilehash: 787aacfd843f5f72e567670d601fb036a2c474bc
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825358"
---
<a name="continuous-integration-and-continuous-delivery-building-real-world-cloud-apps-with-azure"></a>持續整合與持續傳遞 （使用 Azure 建置真實世界的雲端應用程式）
====================
藉由[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)

[下載修正此問題的專案](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下載電子書](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **建置真實世界雲端應用程式與 Azure**電子書以 Scott Guthrie 所開發的簡報為依據。 它說明 13 的模式，並可協助您的作法是成功開發適用於雲端的 web 應用程式。 電子書的相關資訊，請參閱[第 1 章](introduction.md)。


前兩個建議的開發程序模式所[自動執行的所有項目](automate-everything.md)並[原始檔控制](source-control.md)，第三個程序模式中將它們合併。 持續整合 (CI) 表示，每當開發人員簽入至來源存放庫的程式碼，會自動觸發建置。 持續傳遞 (CD) 則更進一步： 組建及自動的單元測試都成功之後，您會自動部署應用程式，您可以執行更深入的測試環境。

雲端可讓您維護測試環境，因為您只需支付環境資源，只要您使用它們的成本降到最低。 您的 CD 程序可以設定測試環境時需要它，以及當您完成時，您可以採取的環境下測試。

## <a name="continuous-integration-and-continuous-delivery-workflow"></a>持續整合與持續傳遞的工作流程

通常我們會建議您連續傳遞至您的開發和預備環境。 大部分的團隊，甚至在 Microsoft，需要手動檢閱及核准程序，針對生產環境部署。 對於實際執行部署，您可能想要確定它時會關鍵人士開發小組可如需支援，或在低流量時段。 但沒有東西可以讓您無法完全自動化開發和測試環境，使所有開發人員只需要簽入變更和環境設定為接受度測試。

下圖來自[Microsoft Patterns and Practices 電子書相關持續傳遞](http://aka.ms/ReleasePipeline)說明典型的工作流程。 按一下影像，若要查看其完整的大小，以其原始內容。

[![持續傳遞工作流程](continuous-integration-and-continuous-delivery/_static/image1.png)](https://msdn.microsoft.com/library/dn449955.aspx)

## <a name="how-the-cloud-enables-cost-effective-ci-and-cd"></a>雲端可讓符合成本效益的 CI 和 CD

自動化在 Azure 中的這些程序很簡單。 因為您在雲端中執行的所有項目，表示您不必購買或管理您的組建或測試環境的伺服器。 您不必等待可執行您的測試上的伺服器。 這麼做的每個建置，可以讓使用您的自動化指令碼、 執行接受度測試或更多深入的測試，在 Azure 中的測試環境開始運作，然後當您完成時就將它清除。 如果您只有 2 小時或 8 個小時或一天中執行該伺服器，您不必支付的金額很少，因為您只支付實際執行機器的時間。 例如，環境所需的修正它的應用程式基本上成本大約 1%，每小時，如果您從免費層級向上一層。 在一個月的過程中，如果您只執行一小時環境一次測試環境可能成本小於您買在星巴克咖啡裡 latte。

## <a name="visual-studio-team-services-vsts"></a>Visual Studio Team Services (VSTS)

VSTS 提供多種功能，以協助您進行從規劃到部署的應用程式開發。

- 它支援 （散發） 的 Git 和 TFVC （集中式） 的原始檔控制。
- 它提供彈性的建置服務，這表示它以動態方式在需要時，會建立組建伺服器，並會將他們帶往下當他們完成。 您自動可以開始建置，當有人簽入原始程式碼變更，而且您沒有已配置，並支付您自己的組建伺服器位於閒置大部分的情況。 只要您不會超過特定數目的組建，組建服務是免費的。 如果您預期要有大量的組建，您可以少的額外支付保留的組建伺服器。
- 它支援連續傳遞至 Azure。
- 它支援自動化的負載測試。 負載測試很重要的雲端應用程式，但之前已經太遲常會遭到忽視。 負載測試會模擬數千名使用者，讓您找出瓶頸並提高輸送量的應用程式大量使用 — 您發行至生產環境應用程式之前。
- 它支援小組室共同作業，有助於即時通訊和小型的敏捷式軟體開發團隊的共同作業。
- 它支援敏捷式專案管理。


如需有關的持續整合和傳遞 VSTS 功能的詳細資訊，請參閱[Visual Studio Team Services](https://www.visualstudio.com/team-services/)。

如果您想要尋求周全專案管理、 小組共同作業、 和原始檔控制解決方案，簽出 VSTS。 服務最多 5 位使用者免費，而您可以註冊在[Visual Studio Team Services](https://www.visualstudio.com/team-services/)。

## <a name="summary"></a>總結

第三個雲端開發模式是有關如何實作具有低的週期時間可重複、 可靠、 可預測的開發程序。 在 [[下一步] 一章](web-development-best-practices.md)我們先來看看架構和程式碼撰寫模式。

## <a name="resources"></a>資源

如需詳細資訊，請參閱 <<c0> [ 部署 Azure App Service 中的 web 應用程式](https://azure.microsoft.com/documentation/articles/web-sites-deploy/)。

另請參閱下列資源：

- [建置與 Team Foundation Server 2012 的發行管線](http://aka.ms/ReleasePipeline)。 電子書的實際操作實驗室和範例程式碼，藉由 Microsoft Patterns and Practices，提供持續傳遞的深入介紹。 涵蓋使用 Visual Studio Lab Management 和 Visual Studio Release Management。
- [ALM Ranger DevOps 工具和指引](https://aka.ms/vsarsolutions/)。 ALM Ranger 導入的 DevOps Workbench 範例隨附的方案和共同作業的模式中的實用指引&amp;作法的書籍*建置與 TFS 2012 的發行管線*，做為啟動的絕佳方法學習開發營運的概念&amp;Release Management for TFS 2012，並親身。 本指南示範如何一次建置並部署至多個環境。
- [Testing for Continuous Delivery with Visual Studio 2012](https://msdn.microsoft.com/library/jj159345.aspx)。 電子書由 Microsoft Patterns and Practices，說明如何整合與持續傳遞的自動化測試。
- [WindowsAzureDeploymentTracker](https://github.com/RyanTBerry/WindowsAzureDeploymentTracker)。 原始碼工具，設計用來擷取從 TFS （根據標籤而定），組建的建置、 封裝、 DevOps 角色，才能設定它的特定層面，允許某人和推送至 Azure。 工具會追蹤部署程序，若要讓 「 回復 」 到先前部署版本的作業。 此工具沒有外部相依性，並可在獨立使用 TFS 的 Api 和 Azure SDK。
- [持續傳遞： 透過建置、 測試和部署自動化可靠的軟體版本](https://www.amazon.com/Continuous-Delivery-Deployment-Automation-Addison-Wesley/dp/0321601912/ref=sr_1_1?s=books&amp;ie=UTF8&amp;qid=1377126361)。 由 Jez Humble 的書籍。
- [釋放它 ！設計及部署生產就緒軟體](https://www.amazon.com/Release-It-Production-Ready-Pragmatic-Programmers/dp/0978739213)。 本書由 Michael T Nygard。

> [!div class="step-by-step"]
> [上一頁](source-control.md)
> [下一頁](web-development-best-practices.md)
