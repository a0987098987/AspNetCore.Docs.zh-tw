---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
title: 設定的權限給小組建置部署 |Microsoft 文件
author: jrjlee
description: 本主題描述如何設定以啟用您的組建伺服器，做為自動化 b 的一部分，將內容部署至 web 伺服器和資料庫伺服器的權限...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 2488a91e-b0a8-465a-b874-3233f724b56b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
msc.type: authoredcontent
ms.openlocfilehash: 4698349d664816ec49475bbfe71fb32af79ea96d
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/08/2018
ms.locfileid: "30890277"
---
<a name="configuring-permissions-for-team-build-deployment"></a>設定的權限給小組建置部署
====================
由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主題描述如何設定讓您自動化的建置程序的一部分，將內容部署至 web 伺服器和資料庫伺服器的組建伺服器的權限。


本主題根據名為 Fabrikam，Inc.的虛構公司的企業部署需求的教學課程系列的一部分此教學課程使用範例方案&#x2014;[連絡人管理員解決方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;來表示實際層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows 通訊的 web 應用程式Foundation (WCF) 服務與資料庫專案。

這些教學課程的核心的部署方法為基礎所說明的分割專案檔案方法[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)，在建置流程控制的兩個專案檔&#x2014;一個包含建置適用於每個目的地環境中和包含特定環境的建置和部署設定的指示。 在建置時，環境特定專案檔就會合併環境無從驗證專案檔來形成一組完整組建指示。

## <a name="task-overview"></a>工作概觀

當您安裝的 Team Foundation Server (TFS) 2010年組建服務時，您會指定要用來執行服務的身分識別。 根據預設，這是在 Network Service 帳戶。 或者，您可以設定組建服務使用網域帳戶來執行。

需要 Windows 驗證，並想要自動化使用 Team Build，任何部署工作會使用組建服務身分識別來執行。 因此，您必須在您的 web 伺服器和資料庫伺服器上任何必要的權限授與組建服務識別。

> [!NOTE]
> 網路服務帳戶使用電腦帳戶驗證至其他電腦。 電腦帳戶會採用 * [網域名稱]\[電腦名稱] ***$**&#x2014;，例如**FABRIKAM\TFSBUILD$**。 因此，如果組建服務會使用 Network Service 識別執行，您應該授的電腦帳戶識別任何必要權限與針對您的組建伺服器。


## <a name="configuring-web-server-permissions"></a>設定 Web 伺服器權限

中所述[選擇 Web 部署的權限方法](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md)，有兩種主要方法，如果您想要將 web 套件部署到遠端網頁伺服器，您可以使用：

- 從遠端位置的應用程式部署將目標設為*Web Deployment Agent Service* （也稱為遠端代理程式） 在目的地伺服器上。
- 從遠端位置的應用程式部署將目標設為*Internet Information Services* (*IIS) Web 部署的處理常式*目的地伺服器上。

遠端代理程式會在此情況下有兩個索引鍵的限制：

- 遠端代理程式只支援 NTLM 驗證。 換句話說，此部署必須使用組建服務識別&#x2014;您無法模擬另一個帳戶。
- 若要使用遠端代理程式，執行部署的帳戶必須是目標伺服器的系統管理員。

在一起，這些兩個的限制進行遠端代理程式的方法讓人困擾的自動化 Team Build 部署。 若要使用這種方法，您需要讓系統管理員帳戶在任何目標 web 伺服器上的組建服務。

相反地，Web 部署的處理常式方法會提供各種優點：

- Web 部署處理常式支援基本驗證，透過 HTTPS，可讓您將替代帳戶的認證傳遞至 IIS Web Deployment Tool (Web Deploy)。
- 您可以設定目標 web 伺服器，以允許非管理員使用者將內容部署至特定的 IIS 網站，使用 Web 部署處理常式。

如此一來，最好明確目標 Web 部署處理常式，當您自動執行從 Team Build web 封裝部署。 這是建議的程序：

1. 建立要用於部署的低權限的網域帳戶。
2. 設定 Web 部署處理常式，並授與帳戶必要的權限，將內容部署至特定的 IIS 網站中所述[設定 Web 伺服器的 Web 部署發行 （網頁部署的處理常式）](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)。
3. 叫用 Web Deploy 和目標 Web 部署處理常式時，使用基本驗證，並提供網域帳戶的認證建立，可執行該部署。

在[連絡人管理員](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)範例方案，您指定的驗證類型 (基本或 NTLM)，Web Deploy 的認證，以及環境特定專案檔中的端點位址 （遠端代理程式或 Web 部署的處理常式）。 這些值可用來編寫和執行專案檔時，執行 Web Deploy 的命令。 如需詳細資訊，請參閱[部署 Web 封裝](../web-deployment-in-the-enterprise/deploying-web-packages.md)。

如需有關設定 Web 部署處理常式，包括如何設定權限，請參閱[設定 Web 伺服器的 Web 部署發行 （網頁部署的處理常式）](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)。 如需有關設定遠端代理程式的詳細資訊，請參閱[設定 Web 伺服器的 Web 部署發行 （遠端代理程式）](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)。

## <a name="configuring-database-server-permissions"></a>設定資料庫伺服器的權限

若要將資料庫部署到 SQL Server，您必須：

- SQL Server 執行個體上建立部署的帳戶的登入。
- 授與登入**DBCreator** SQL Server 執行個體上的權限。
- 初始部署之後，新增的登入**db\_擁有者**目標資料庫上的角色。 這是必要的因為在後續的部署中，您要修改現有的資料庫而建立新的資料庫。

您可以驗證使用 NTLM 驗證或 SQL Server 驗證的 SQL Server 執行個體：

- 如果您使用 NTLM 驗證，您需要授與上述組建服務帳戶的權限。
- 如果您使用 SQL Server 驗證時，您需要授與上述 SQL Server 帳戶的權限。 您也需要您使用部署資料庫的連接字串中包含的 SQL Server 使用者名稱和密碼。

如需如何設定權限的資料庫部署逐步的詳細資訊，請參閱[設定資料庫伺服器的 Web 部署發行](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md)。

## <a name="conclusion"></a>結論

此時，您應該了解必要的權限，以及驗證選項，您會開啟時自動從 Team Build web 應用程式和資料庫部署。 您也應該在 IIS web 伺服器和 SQL Server 資料庫伺服器上實作必要的權限。

## <a name="further-reading"></a>進一步閱讀

如需有關設定以支援遠端部署 Windows server 環境的詳細資訊，請參閱[用於 Web 部署設定伺服器環境](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md)。

> [!div class="step-by-step"]
> [上一步](deploying-a-specific-build.md)
