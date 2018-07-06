---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
title: 設定權限的小組組建部署 |Microsoft Docs
author: jrjlee
description: 本主題描述如何設定以啟用您的組建伺服器，做為自動化的 b 的一部分時，將內容部署至 web 伺服器和資料庫伺服器的權限...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 2488a91e-b0a8-465a-b874-3233f724b56b
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
msc.type: authoredcontent
ms.openlocfilehash: b577d887b4a4476b6796ae9f1df538d16eededa3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820356"
---
<a name="configuring-permissions-for-team-build-deployment"></a>組建部署小組設定權限。
====================
藉由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主題描述如何設定以啟用您的自動化的建置程序的一部分時，將內容部署至 web 伺服器和資料庫伺服器的組建伺服器的權限。


本主題是構成一系列以名為 Fabrikam，Inc.的虛構公司的企業部署需求為基礎的教學課程的一部分本教學課程系列會使用範例解決方案&#x2014;[連絡管理員解決方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;來代表實際的層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows Communication 的 web 應用程式Foundation (WCF) 服務與資料庫專案。

這些教學課程的核心的部署方法根據分割專案檔案方法中所述[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)，在建置流程控制的兩個專案檔&#x2014;包含建置適用於每個目的地環境中和包含環境特定建置和部署設定的指示。 在建置階段的特定環境的專案檔會合併到無從驗證環境的專案檔中，以構成一組完整的組建指示。

## <a name="task-overview"></a>工作概觀

當您安裝 Team Foundation Server (TFS) 2010年組建服務時，您會指定要用來執行服務的身分識別。 根據預設，這是在 Network Service 帳戶。 或者，您可以設定為使用網域帳戶來執行組建服務。

任何需要 Windows 驗證，並想要自動化 Team Build，所使用的部署工作會使用組建服務身分識別來執行。 因此，您必須在您的 web 伺服器和資料庫伺服器上任何必要的權限授與組建服務身分識別。

> [!NOTE]
> 網路服務帳戶使用電腦帳戶來向其他電腦。 電腦帳戶的形式 * [網域名稱]\[電腦名稱] ***$**&#x2014;，例如**FABRIKAM\TFSBUILD$**。 同樣地，如果您的組建服務執行使用 Network Service 身分識別，您應該授與的電腦帳戶識別任何必要權限為您的組建伺服器。


## <a name="configuring-web-server-permissions"></a>設定網頁伺服器權限

中所述[選擇 Web 部署的權限方法](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md)，有兩種主要的方法，如果您想要將 web 套件部署至遠端網頁伺服器，您可以使用：

- 將從遠端位置的應用程式部署將目標設為*Web Deployment Agent Service* （也稱為遠端代理程式） 在目的地伺服器上。
- 將從遠端位置的應用程式部署將目標設為*Internet Information Services* (*IIS) Web 部署的處理常式*目的地伺服器上。

遠端代理程式在此情況下有兩個主要限制：

- 遠端代理程式只支援 NTLM 驗證。 換句話說，此部署必須使用組建服務身分識別&#x2014;您無法模擬另一個帳戶。
- 若要使用遠端代理程式，請執行部署的帳戶必須是目標伺服器的系統管理員。

放在一起，這些兩個的限制進行遠端代理程式的方法不想要自動化 Team Build 部署。 若要使用這種方法，您必須進行組建服務帳戶在任何目標 web 伺服器上的系統管理員。

相反地，Web 部署的處理常式方法會提供各種不同的優點：

- Web 部署處理常式支援基本驗證，透過 HTTPS，可讓您將替代帳戶的認證傳遞至 IIS Web Deployment Tool (Web Deploy)。
- 您可以設定目標 web 伺服器，以允許非系統管理員的使用者，將內容部署至特定的 IIS 網站，使用 Web 部署處理常式。

如此一來，最好是清楚地將目標設 Web 部署處理常式，當您從 Team Build web 套件部署自動化。 這是建議的程序：

1. 建立要用於部署的低權限的網域帳戶。
2. 設定 Web 部署處理常式，並授與帳戶必要的權限，將內容部署至特定的 IIS 網站，如中所述[設定 Web 伺服器的 Web 部署發行 （Web 部署的處理常式）](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)。
3. 叫用 Web Deploy 和目標 Web 部署處理常式時，使用基本驗證，並提供網域帳戶的認證建立，若要執行部署。

在  [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)範例方案，指定驗證類型 (基本或 NTLM)，Web Deploy 認證，以及環境特定專案檔中的端點位址 （遠端代理程式或 Web 部署的處理常式）。 這些值用來編寫和執行專案檔時，執行 Web Deploy 命令。 如需詳細資訊，請參閱 <<c0> [ 部署網頁套件](../web-deployment-in-the-enterprise/deploying-web-packages.md)。

如需有關設定 Web 部署處理常式，包括如何設定權限，請參閱[設定 Web 伺服器的 Web 部署發行 （Web 部署的處理常式）](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)。 如需有關設定遠端代理程式的詳細資訊，請參閱 <<c0> [ 設定 Web 伺服器的 Web 部署發行 （遠端代理程式）](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)。

## <a name="configuring-database-server-permissions"></a>設定資料庫伺服器的權限

若要將資料庫部署到 SQL Server 中，您必須：

- SQL Server 執行個體上建立之部署的帳戶登入。
- 授與登入**DBCreator** SQL Server 執行個體上的權限。
- 初始部署之後，新增 登入**db\_擁有者**目標資料庫上的角色。 這是必要的因為在後續的部署中，您要修改現有的資料庫而建立新的資料庫。

您可以驗證使用 NTLM 驗證或 SQL Server 驗證的 SQL Server 執行個體：

- 如果您使用 NTLM 驗證時，您需要授與上述組建服務帳戶的權限。
- 如果您使用 SQL Server 驗證時，您需要授與上面所述，SQL Server 帳戶的權限。 您也需要您使用部署資料庫的連接字串中包含的 SQL Server 使用者名稱和密碼。

如需如何設定資料庫部署的權限的逐步詳細資訊，請參閱[設定資料庫伺服器的 Web 部署發佈](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md)。

## <a name="conclusion"></a>結論

此時，您應該了解時所需，驗證選項，開啟您 web 應用程式與資料庫從將部署自動化 Team Build 的權限。 您也應該能夠在 IIS web 伺服器和 SQL Server 資料庫伺服器上實作必要的權限。

## <a name="further-reading"></a>進一步閱讀

如需有關如何設定 Windows server 環境，以支援遠端部署的詳細資訊，請參閱 <<c0> [ 設定 Web 部署的伺服器環境](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md)。

> [!div class="step-by-step"]
> [上一步](deploying-a-specific-build.md)
