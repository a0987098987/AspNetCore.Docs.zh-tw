---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
title: "持續整合與持續傳遞 （使用 Azure 建置實際的雲端應用程式） |Microsoft 文件"
author: MikeWasson
description: "Scott Guthrie 所開發的簡報是以基礎建置真實世界雲端應用程式與 Azure 的電子書。 它說明 13 模式和做法，他可以..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: eaece9f5-f80c-428b-b771-5db66d275b7d
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
msc.type: authoredcontent
ms.openlocfilehash: 4a5433a7dd70e27b59163822ba427b026c3f4ce0
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="continuous-integration-and-continuous-delivery-building-real-world-cloud-apps-with-azure"></a>持續整合與持續傳遞 （使用 Azure 建置實際的雲端應用程式）
====================
由[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)

[下載修正專案](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下載電子書](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **建置真實世界雲端應用程式與 Azure**電子書根據 Scott Guthrie 所開發的簡報。 它說明 13 模式，並可協助您的作法是成功開發雲端的 web 應用程式。 E 書籍的相關資訊，請參閱[第一章](introduction.md)。


前兩個建議的開發程序模式已[一切自動化](automate-everything.md)和[原始檔控制](source-control.md)，第三個程序模式中將它們合併。 持續整合 (CI) 表示每當開發人員程式碼存至來源儲存機制，組建會自動觸發。 持續傳遞 (CD) 則更進一步： 組建及自動的單元測試都成功之後，您會自動部署應用程式，您可以更深入了解測試的環境。

雲端可讓您維護在測試環境，因為您只需支付環境資源，只要您使用它們的成本降至最低。 您的 CD 程序可以設定測試環境時，您需要它，而且當您完成時，您可以採取環境下測試。

## <a name="continuous-integration-and-continuous-delivery-workflow"></a>持續整合及連續傳遞的工作流程

通常我們建議您採用持續傳遞至您的開發和預備環境。 大部分的小組，甚至是 Microsoft 在生產環境部署需要手動檢閱及核准程序。 生產環境部署可能會想要確定其會發生在索引鍵的開發團隊的人員都可支援，或在低流量期間。 但是沒有執行任何動作可防止您在完全自動化您的開發和測試環境，使所有開發人員需要簽入變更和環境設定為接受度測試。

下圖從[Microsoft Patterns and Practices 電子書持續傳遞有關](http://aka.ms/ReleasePipeline)說明典型的工作流程。 按一下以查看它的映像完整大小，以其原始內容。

[![持續傳遞工作流程](continuous-integration-and-continuous-delivery/_static/image1.png)](https://msdn.microsoft.com/library/dn449955.aspx)

## <a name="how-the-cloud-enables-cost-effective-ci-and-cd"></a>雲端可以讓具成本效益的 CI 和 CD

自動執行這些程序，在 Azure 中的很容易。 因為您在雲端中執行的所有項目，表示您不必購買或管理您的組建或測試環境的伺服器。 您不必等候伺服器可供執行上進行測試。 與您執行的每個組建，可以加速使用自動化指令碼、 執行的接受度測試或更多的深度測試，在 Azure 中的測試環境，然後當您完成就終止它。 如果您只會執行該伺服器的 2 小時或 8 個小時或一天，您必須支付費用的金額很少，因為您只支付電腦實際執行的時間。 例如，環境所需的修正應用程式基本上成本約每小時的 1%如果您從 可用層級向上一層。 在一個月的過程中，如果您只執行一小時環境一次測試環境會可能成本小於您購買星巴克 latte。

## <a name="visual-studio-team-services-vsts"></a>Visual Studio Team Services (VSTS)

VSTS 提供功能，可協助您從規劃到部署的應用程式開發的數的字。

- 它支援 （散發） 的 Git 和 TFVC （集中式） 的原始檔控制。
- 它提供彈性的組建服務，這表示它以動態方式在需要時，會建立組建伺服器，並將他們帶往下當他們完成。 您可以自動開始進行建置時有人簽入原始程式碼變更，而且不需要有配置及支付介於閒置大部分的情況下您的組建伺服器。 組建服務是免費的只要您不要超過組建數目。 如果您預期執行大量的組建，您可以很少的額外支付保留的組建伺服器。
- 它支援持續傳遞至 Azure。
- 它支援自動化的負載測試。 負載測試，請務必使用雲端應用程式，但通常會忽略，直到已經太遲了。 負載測試會模擬數千位使用者，讓您找出瓶頸並提高輸送量應用程式大量使用 — 您發行至生產環境應用程式之前。
- 它支援小組室共同作業，可用於促進即時通訊和小型的敏捷式軟體開發小組共同作業。
- 它支援敏捷式軟體開發專案管理。


如需有關的連續整合和 VSTS 傳遞功能的詳細資訊，請參閱[Visual Studio Team Services](https://www.visualstudio.com/team-services/)。

如果您想要尋求周全專案管理，小組共同作業和原始檔控制方案，請檢查 VSTS 出。 最多 5 位使用者，免費服務而您可以註冊在[Visual Studio Team Services](https://www.visualstudio.com/team-services/)。

## <a name="summary"></a>總結

有關如何實作與低的週期時間可重複、 可靠、 可預測的開發程序已將第三種雲端開發模式。 在[下一章](web-development-best-practices.md)我們開始查看架構和程式碼撰寫模式。

## <a name="resources"></a>資源

如需詳細資訊，請參閱[部署在 Azure App Service web 應用程式](https://azure.microsoft.com/documentation/articles/web-sites-deploy/)。

另請參閱下列資源：

- [建立與 Team Foundation Server 2012 的發行管線](http://aka.ms/ReleasePipeline)。 電子書的實際操作實驗室和範例程式碼由 Microsoft Patterns and Practices，深入了解介紹持續傳遞。 涵蓋如何使用 Visual Studio Lab Management 和 Visual Studio Release Management。
- [ALM Ranger DevOps 工具和指引](https://aka.ms/vsarsolutions/)。 DevOps Workbench 範例隨附的方案和共同作業與模式中的實用指導方針，導入了 ALM Ranger&amp;實務書籍*建置與 TFS 2012 的發行管線*，做為啟動的絕佳方式了解的概念 DevOps &amp; TFS 2012，促使輪胎 Release Management。 本指南示範如何一次建置並部署至多個環境。
- [For Continuous Delivery with Visual Studio 2012 測試](https://msdn.microsoft.com/library/jj159345.aspx)。 電子書，由 Microsoft Patterns and Practices，說明如何將自動化測試持續傳遞與整合。
- [WindowsAzureDeploymentTracker](https://github.com/RyanTBerry/WindowsAzureDeploymentTracker). 設計用來擷取從 TFS （根據標籤），建置工具的原始程式碼建置、 封裝、 讓其他人 DevOps 角色中設定特定層面，和將它推送至 Azure。 分析工具會追蹤部署程序，才能啟用 「 回復 」 到先前部署版本的作業。 此工具沒有外部相依性，並可在 TFS 應用程式開發介面和 Azure SDK 獨立使用。
- [持續傳遞： 透過組建、 測試和自動化部署可靠軟體釋放](https://www.amazon.com/Continuous-Delivery-Deployment-Automation-Addison-Wesley/dp/0321601912/ref=sr_1_1?s=books&amp;ie=UTF8&amp;qid=1377126361)。 Jez 卑微書。
- [釋放它 ！設計和部署可實際執行的軟體](https://www.amazon.com/Release-It-Production-Ready-Pragmatic-Programmers/dp/0978739213)。 本書由 Michael T Nygard。

>[!div class="step-by-step"]
[上一頁](source-control.md)
[下一頁](web-development-best-practices.md)
