---
title: ASP.NET Core 與 Azure 的 DevOps
author: CamSoper
description: 本指南為如何為 Azure 上裝載的 ASP.NET Core 應用程式，建置 DevOps 管線的完整指導。
ms.author: casoper
ms.date: 08/07/2018
ms.custom: mvc, seodec18
uid: azure/devops/index
ms.openlocfilehash: f45bb2a5dd4b3d1a820085ede7ce3219045ed80b
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64881953"
---
# <a name="devops-with-aspnet-core-and-azure"></a>ASP.NET Core 與 Azure 的 DevOps

[![封面影像](./media/cover-large.png)](https://aka.ms/devopsbook)

[Cam Soper](https://twitter.com/camsoper) 和 [Scott Addie](https://twitter.com/scottaddie) 著

本指南提供[可下載的 PDF 電子書](https://aka.ms/devopsbook)。

## <a name="welcome"></a>歡迎畫面 

歡迎使用適用於.NET 的 Azure 開發生命週期指南！ 本指南介紹如何使用.NET 工具及程序，為 Azure 建置開發生命週期的基本概念。 完成本指南之後，您就能充分運用完善 DevOps 工具鏈的各項優點。

## <a name="who-this-guide-is-for"></a>本指南的適用對象

您應是有經驗的 ASP.NET Core 開發人員 (200-300 級)。 因為本指南涵蓋 Azure 介紹，所以您無須具備這方面的知識。 本指南也適用於工作內容偏重於操作，而不在開發的 DevOps 工程師。

本指南的對象是 Windows 開發人員， 但 .NET Core 也 100% 支援 Linux 與 macOS。 若要將本指南應用到 Linux/macOS，請留意圖說文字所示的 Linux/macOS 差異。

## <a name="what-this-guide-doesnt-cover"></a>本指南未說明的內容

本指南適用於.NET 開發人員，著重於端對端的持續部署體驗， 而不會詳盡解說 Azure 的一切，也不會在 Azure 服務適用的 .NET API 上著墨太多。 其會將重點圍繞在持續整合、部署、監視及偵錯上。 在快速入門的最後，則會提供後續步驟的建議。 建議內含對 ASP.NET Core 開發人員來說很實用的 Azure 平台服務。

## <a name="whats-in-this-guide"></a>本指南內容

### <a name="tools-and-downloadsxrefazuredevopstools-and-downloads"></a>[工具及下載](xref:azure/devops/tools-and-downloads)

了解如何取得本指南中使用的工具。

### <a name="deploy-to-app-servicexrefazuredevopsdeploy-to-app-service"></a>[部署到 App Service](xref:azure/devops/deploy-to-app-service)

了解各種如何將 ASP.NET Core 應用程式部署到 Azure App Service 的方法。

### <a name="continuous-integration-and-deploymentxrefazuredevopscicd"></a>[持續整合與部署](xref:azure/devops/cicd)

使用 GitHub、Azure DevOps Services 與 Azure，為您的 ASP.NET Core 應用程式建置端對端的持續整合與部署解決方案。

### <a name="monitor-and-debugxrefazuredevopsmonitor"></a>[監視及偵錯](xref:azure/devops/monitor)

使用 Azure 工來監視、疑難排解問題，以及微調您的應用程式。

### <a name="next-stepsxrefazuredevopsnext-steps"></a>[後續步驟](xref:azure/devops/next-steps)

ASP.NET Core 開發人員學習 Azure 的其他學習途徑。

## <a name="additional-introductory-reading"></a>其他入門閱讀資料

如果這是您第一次接觸雲端運算，這些文章會說明基本概念。

* [什麼是雲端運算？](https://azure.microsoft.com/overview/what-is-cloud-computing/)
* [雲端運算的範例](https://azure.microsoft.com/overview/examples-of-cloud-computing/)
* [什麼是 IaaS？](https://azure.microsoft.com/overview/what-is-iaas/)
* [什麼是 PaaS？](https://azure.microsoft.com/overview/what-is-paas/)
