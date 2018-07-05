---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
title: 連絡管理員解決方案 |Microsoft Docs
author: jrjlee
description: 這一系列的教學課程使用範例解決方案&#x2014;連絡管理員解決方案&#x2014;來代表實際的層級的企業級應用程式...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 4d8c8d19-055b-4b70-9ee1-f748f0db3a01
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: 8187766190da43ded52359892601f8129b9940ce
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388104"
---
<a name="the-contact-manager-solution"></a>連絡管理員解決方案
====================
藉由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 這[系列的教學課程](web-deployment-in-the-enterprise.md)範例解決方案會使用&#x2014;連絡管理員解決方案&#x2014;來代表實際的複雜程度的企業級應用程式。 本主題介紹連絡管理員解決方案、 將告訴您方案的重要元件，並識別部署這類企業環境中的各種目標平台的應用程式的挑戰。
> 
> 當您完成主題在這些教學課程中，您可以使用連絡管理員解決方案做為示範如何滿足特定的挑戰，在企業部署案例中的參考實作。 下一個主題中，[設定註冊連絡管理員解決方案](setting-up-the-contact-manager-solution.md)，說明如何下載並在您的開發人員工作站上執行解決方案。


## <a name="solution-overview"></a>解決方案概觀

連絡管理員解決方案包含四個個別專案：

![](the-contact-manager-solution/_static/image1.png)

- **ContactManager.Mvc**。 這是 ASP.NET MVC 3 web 應用程式專案，表示方案的進入點。 它提供一些基本的 web 應用程式功能，例如讓使用者能夠建立及檢視連絡人詳細資料。 應用程式依賴 Windows Communication Foundation (WCF) 服務來管理連絡人和 ASP.NET 應用程式服務資料庫來管理驗證和授權。
- **ContactManager.Database**。 這是 Visual Studio 資料庫專案。 專案定義商店連絡人詳細資料的資料庫結構描述。
- **ContactManager.Service**。 這是 WCF web 服務專案。 WCF 服務所公開的端點，可讓呼叫端執行建立、 擷取、 更新和刪除 (CRUD) 作業上**ContactManager**資料庫。 服務會仰賴**ContactManager**資料庫並**ContactManager.Common.dll**組件。
- **ContactManager.Common**。 這是類別庫專案。 WCF 服務依賴這個組件中定義的型別。

解決方案也會包含名為發行的方案資料夾。 這包含各種自訂的專案檔和示範如何控制和管理組建和部署處理序的命令檔。 這些是稍後在本教學課程涵蓋更多詳細資料。

就概念而言，解決方案的元件彼此搭配運作就像這樣：

![](the-contact-manager-solution/_static/image2.png)

> [!NOTE]
> 雖然 ASP.NET MVC 3 web 應用程式使用 ASP.NET 成員資格提供者，但在 web 應用程式中的所有頁面都允許匿名存取。 這顯然是不實際的組態。 不過，解決方案是設定在這種方式，讓您更輕鬆地部署和測試解決方案，但未設定使用者帳戶和角色。


## <a name="deployment-challenges"></a>部署挑戰

連絡管理員解決方案說明通用於許多企業部署案例的幾項部署挑戰：

- 解決方案包含多個相依專案。 您要同時部署這些專案。
- 連接字串和服務端點需要更新每個環境，並在許多情況下這項資訊將無法供開發人員。
- 當您部署**ContactManager**資料庫來預備和生產環境中，您需要保留現有的資料，在後續的部署。
- 當您部署 ASP.NET 應用程式服務資料庫時，您需要部署某些組態資料，但省略任何使用者帳戶資料。
- 專案會包含某些檔案和不應部署的資料夾。 您需要在部署程序中排除這些檔案和資料夾。
- 解決方案必須支援從 Team Foundation Server (TFS) 組建伺服器自動化的部署。

## <a name="conclusion"></a>結論

本主題提供連絡管理員解決方案的高階概觀，並識別一些通用於許多企業部署案例的固有的部署挑戰。 在本教學課程的其餘主題將描述的一些技術可用來克服這些挑戰。

下一個主題中，[設定註冊連絡管理員解決方案](setting-up-the-contact-manager-solution.md)，說明如何下載並在您的開發人員工作站上執行解決方案。

> [!div class="step-by-step"]
> [上一頁](web-deployment-in-the-enterprise.md)
> [下一頁](setting-up-the-contact-manager-solution.md)
