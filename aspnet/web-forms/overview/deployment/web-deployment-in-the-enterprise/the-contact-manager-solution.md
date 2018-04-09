---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
title: 請連絡管理員解決方案 |Microsoft 文件
author: jrjlee
description: 這一系列的教學課程使用範例方案&#x2014;連絡人管理員解決方案&#x2014;來表示實際 leve 的企業規模應用程式...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 4d8c8d19-055b-4b70-9ee1-f748f0db3a01
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: d7034f800df98747d10401d7e2c7297fea0e46d4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="the-contact-manager-solution"></a>請連絡管理員解決方案
====================
由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 這[系列的教學課程](web-deployment-in-the-enterprise.md)會使用範例方案&#x2014;連絡人管理員解決方案&#x2014;來代表企業規模應用程式與實際的層級的複雜性。 本主題導入了連絡人管理員解決方案、 描述方案的重要元件，並識別部署這類企業環境中的各種目標平台的應用程式的挑戰。
> 
> 當您完成主題這些教學課程中，您可以使用連絡人管理員解決方案做為示範如何滿足特定的挑戰，在企業部署案例中參考實作。 下一個主題[設定總連絡人管理員解決方案](setting-up-the-contact-manager-solution.md)，說明如何下載並在開發人員工作站上執行解決方案。


## <a name="solution-overview"></a>解決方案概觀

連絡管理員方案包含四個個別專案：

![](the-contact-manager-solution/_static/image1.png)

- **ContactManager.Mvc**. 這是 ASP.NET MVC 3 web 應用程式專案，表示方案的進入點。 它提供一些基本 web 應用程式功能，例如使用者提供的功能，以建立及檢視連絡人詳細資料。 應用程式依賴 Windows Communication Foundation (WCF) 服務來管理連絡人及管理驗證和授權的 ASP.NET 應用程式服務資料庫。
- **ContactManager.Database**. 這是 Visual Studio 資料庫專案。 專案定義的商店連絡人詳細資料的資料庫結構描述。
- **ContactManager.Service**. 這是 WCF web 服務專案。 WCF 服務會公開端點，可讓呼叫端執行建立、 擷取、 更新和刪除 (CRUD) 作業上**ContactManager**資料庫。 服務會仰賴**ContactManager**資料庫和**ContactManager.Common.dll**組件。
- **ContactManager.Common**. 這是類別庫專案。 WCF 服務依賴這個組件中定義的型別。

解決方案也會包含名為發行的方案資料夾。 這包含各種自訂專案檔和示範如何控制和管理組建與部署處理序的命令檔。 這些是稍後在本教學課程涵蓋更多詳細資料。

在概念層級，解決方案的元件搭配像這樣：

![](the-contact-manager-solution/_static/image2.png)

> [!NOTE]
> ASP.NET MVC 3 web 應用程式使用 ASP.NET 成員資格提供者，而 web 應用程式內的所有頁面就會都允許匿名存取。 這顯然不是實際的組態。 但是，方案是設定在這種方式，讓您更輕鬆地部署和測試解決方案，而不需設定使用者帳戶和角色。


## <a name="deployment-challenges"></a>部署的挑戰

請連絡管理員解決方案說明企業部署案例的許多常見的幾個部署挑戰：

- 方案包含多個相依專案。 您必須同時部署這些專案。
- 連接字串和服務端點需要更新每個環境中，而且在許多情況下這項資訊將無法供開發人員。
- 當您部署**ContactManager**資料庫預備和生產環境，您需要保留現有的資料，後續部署。
- 當您部署 ASP.NET 應用程式服務資料庫時，您需要部署某些組態資料，但略過任何使用者帳戶的資料。
- 專案包含某些檔案和不應部署的資料夾。 您需要這些檔案和資料夾排除部署程序。
- 解決方案必須支援從 Team Foundation Server (TFS) 組建伺服器的自動的部署。

## <a name="conclusion"></a>結論

本主題提供連絡人管理員解決方案的高階概觀，並識別通用於許多企業部署案例的固有部署挑戰。 本教學課程中的其他主題會說明一些您可以使用為了克服這些挑戰的技術。

下一個主題[設定總連絡人管理員解決方案](setting-up-the-contact-manager-solution.md)，說明如何下載並在開發人員工作站上執行解決方案。

> [!div class="step-by-step"]
> [上一頁](web-deployment-in-the-enterprise.md)
> [下一頁](setting-up-the-contact-manager-solution.md)
