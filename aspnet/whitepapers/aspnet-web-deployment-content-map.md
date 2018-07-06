---
uid: whitepapers/aspnet-web-deployment-content-map
title: ASP.NET Web 部署-建議資源 |Microsoft Docs
author: rick-anderson
description: 本主題提供有關如何部署資源 （發佈） ASP.NET web 文件連結至 IIS 的應用程式，使用 Visual Studio 2010 中，Visual Web De...
ms.author: aspnetcontent
ms.date: 03/14/2014
ms.assetid: 58b583cd-c4ab-47a3-8527-8c92c298c91f
msc.legacyurl: /whitepapers/aspnet-web-deployment-content-map
msc.type: content
ms.openlocfilehash: f46a588144fb1af958737b07b73acbff60318fe6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809416"
---
<a name="aspnet-web-deployment---recommended-resources"></a>ASP.NET Web 部署-建議資源
====================
> 本主題提供有關如何部署資源 （發佈） ASP.NET web 文件連結至 IIS 的應用程式，使用 Visual Studio 2010、 Visual Web Developer 2010 和更新版本。
> 
> 如果您知道很棒的部落格文章[stackoverflow](http://stackoverflow.com)執行緒或任何其他會很有用的連結[傳送一封電子郵件給我們](mailto:aspnetue@microsoft.com?subject=Deployment Content Map)與連結。
> 
> > [!NOTE] 
> > 
> > 許多這些資源的說明部署功能，只有在您安裝最新版的可用[Visual Studio Web Publish Update](https://go.microsoft.com/fwlink/?LinkID=208120)。 有些只適用於 Visual Studio 2012 或 Visual Studio 2013 的功能。


此主題包括下列章節：

- [了解 web 專案的部署選項](#understanding)
- [尋找主機服務提供者為 ASP.NET 應用程式](#findinghosting)
- [部署 web 應用程式從 Visual Studio](#fromvs)
- [部署 web 應用程式建立及安裝 web 部署套件](#package)
- [部署 web 應用程式使用持續整合 (CI) 程序](#ci)
- [若要變更目的 Web.config 檔案或 app.config 檔案中的設定，在部署期間使用 Web.config 轉換](#transforms)
- [若要變更設定，在目的地 web 應用程式，在部署期間使用 Web Deploy 的參數](#webdeployparms)
- [確定應用程式是在部署期間離線](#appoffline)
- [將資料庫或變更部署到資料庫作為 web 應用程式部署的一部分](#databasewithweb)
- [部署資料庫與 web 應用程式部署分開](#databaseseparate)
- [部署使用 ASP.NET 應用程式的 web 應用程式服務，例如成員資格和程式碼剖析](#aspnetmembership)
- [部署先行編譯](#precompiling)
- [部署內部網路 web 應用程式](#intranet)
- [根據預設，未自動化的自動化一般部署工作](#automating)
- [設定 web 伺服器，可讓開發人員可以將部署給他們使用 Web Deploy 的 web 應用程式](#configuringservers)
- [設定伺服器主控提供者](#hostingprovider)
- [疑難排解部署問題](#troubleshooting)
- [取得特定的部署問題的說明](#gettinghelp)
- [其他資源](#additional)


<a id="understanding"></a>


## <a name="understanding-deployment-options-for-web-projects"></a>了解 web 專案的部署選項

- [適用於 Visual Studio 和 ASP.NET web 部署概觀](https://msdn.microsoft.com/library/dd394698.aspx)(MSDN)。
- [如何部署 Windows Azure 網站](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy)。 說明 部署 web 專案至 Windows Azure 網站，包括資源的 選項和連結[持續傳遞](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md)(從自動化[原始檔控制](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md)) 以及使用 Visual Studio。
- [Visual Studio 2012 Web 發行改善](../visual-studio/overview/2012/visual-studio-2012-web-publishing-improvements.md)（由 Scott hanselman 主講的影片顯示）。
- [VS 2010 中的 Web 部署的概觀文章](http://vishaljoshi.blogspot.com/2009/09/overview-post-for-web-deployment-in-vs.html)（Vishal Joshi 部落格）。 較舊的部落格文章，但某些 Visual Studio 2010 資源它會將仍與 Visual Studio 2012 的資訊連結。


<a id="findinghosting"></a>


## <a name="finding-hosting-providers-for-an-aspnet-application"></a>尋找主機服務提供者為 ASP.NET 應用程式

- [ASP.NET 裝載](https://asp.net/hosting)


<a id="fromvs"></a>


## <a name="deploying-a-web-application-from-visual-studio"></a>部署 web 應用程式從 Visual Studio

- [如何部署 Windows Azure 網站](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy)。 說明選項，並提供資源的連結將 web 專案部署至 Windows Azure 網站。 包括從 Visual Studio 部署的相關區段。
- [使用 Visual Studio 的 ASP.NET Web 部署](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md)。 分成 12 個單元的教學課程，示範如何部署 web 應用程式與 SQL Server 資料庫。 資料庫的部署使用的 dbDacFx 提供者 」 和 「 Entity Framework Code First 移轉。 也包含下列資訊[Web.config 檔案轉換](../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md)，[部署個別的檔案](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md#specificfiles)，[命令列部署](../web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment.md)，和[如何自訂 Visual Studio web 發行管線，藉由編輯.pubxml 檔案](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files.md)。 適用於所有的 ASP.NET web 專案，包括 Web Form、 MVC 和 Web API）。
- [如何： 部署 Web 專案使用單鍵發行 Visual Studio 中](https://msdn.microsoft.com/library/dd465337.aspx)（參考 [Visual Studio Web 發行精靈] 的資訊）。
- [使用 SQL Server Compact 使用 Visual Studio 將 ASP.NET Web 應用程式部署](../web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md)。 這是舊版**使用 Visual Studio 的 ASP.NET Web 部署**列頂端的這一節。 如需有關如何部署 SQL Server Compact 資料庫，以及如何從 SQL Server Compact 移轉至 SQL Server 的完整版本資訊的主要適用於目前。
- [.NET 多層式應用程式使用儲存體資料表、 佇列和 Blob](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) （Microsoft Azure 站台）。 5 部分教學課程系列中，示範如何建立 MVC 專案，並將它部署至 Windows Azure 雲端服務。


<a id="package"></a>
## <a name="deploying-a-web-application-by-creating-and-installing-a-web-deployment-package"></a>部署 web 應用程式建立及安裝 web 部署套件

- [如何： 在 Visual Studio 中建立 Web 部署套件](https://msdn.microsoft.com/library/dd465323.aspx)(MSDN)。
- [如何： 使用 deploy.cmd 檔案的部署封裝建立的 Visual studio 的安裝](https://msdn.microsoft.com/library/ff356104.aspx)(MSDN)。
- [使用 Web Deploy 封裝部署在 dev 方塊上的 IIS 和協力廠商主機](http://sedodream.com/2011/11/08/UsingAWebDeployPackageToDeployToIISOnTheDevBoxAndToAThirdPartyHost.aspx)（Sayed Hashimi 部落格）。 如何使用 IIS 管理員在本機電腦上安裝 IIS 中的部署套件和裝載在公司的支援 IIS 管理員進行遠端管理。
- [建置 Web 部署套件從 Visual Studio 2010](https://www.iis.net/learn/publish/using-web-deploy/building-a-web-deploy-package-from-visual-studio-2010) （IIS.NET 網站）。 包含命令列的封裝建立和安裝的指示。
- [封裝一次發行任何地方](http://sedodream.com/2012/03/14/PackageWebUpdatedAndVideoBelow.aspx)（Sayed Hashimi 部落格）。 導入了 NuGet 套件，可自動轉換多個目的地環境的 Web.config 檔案的程序，以便您可以將一個封裝部署到多部伺服器。 另請參閱[PackageWeb 影片](https://www.youtube.com/watch?v=-LvUJFI8CzM)由 Sayed Hashimi。

另請參閱下一節。


<a id="ci"></a>


## <a name="deploying-a-web-application-using-a-continuous-integration-ci-process"></a>部署 web 應用程式使用持續整合 (CI) 程序

- [持續整合與持續傳遞 （使用 Windows Azure 建置真實世界的雲端應用程式）。](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) 導入了持續整合與持續傳遞的電子書章節。
- [如何部署 Windows Azure 網站](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy)。 部署 web 專案至 Windows Azure 網站的資源來說明選項和連結。 包含關於自動化部署，從原始檔控制的區段。
- [部署 Web 應用程式，在企業案例](../web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md)。 40 部分教學課程系列中，示範如何使用 Visual Studio 2010 和 Team Foundation Server 2010 CI 程序中的部署作業自動化。
- [在 Microsoft Build Engine： 使用 MSBuild 和 Team Foundation Build，Sayed Hashimi 和 William bartholomew< /](http://msbuildbook.com)。 這是一本書，而不需要 web 資源，但它是不可或缺的指南以了解如何設定連續整合案例的 MSBuild。
- [MSBuild 延伸模組套件](https://github.com/mikefourie/MSBuildExtensionPack)。 包含部署的工作。
- [Team Foundation 組建自訂指南](https://aka.ms/vsarsolutions)。 透過 ALM Ranger 上設定 Team Foundation Server 的文件涵蓋 web 部署，並包含教學課程和影片。
- [從 CI 伺服器的 SlowCheetah XML 轉換](http://sedodream.com/2011/12/12/SlowCheetahXMLTransformsFromACIServer.aspx)（Sayed Hashimi 部落格）。 說明如何使用 SlowCheetah，Visual Studio 增益集轉換 app.config 和其他 XML 檔案。

另請參閱[確定應用程式是在部署期間離線](aspnet-web-deployment-content-map.md#appoffline)稍後使用此頁面。


<a id="transforms"></a>


## <a name="using-webconfig-transformations-to-change-settings-in-the-destination-webconfig-file-or-appconfig-file-during-deployment"></a>若要變更目的 Web.config 檔案或 app.config 檔案中的設定，在部署期間使用 Web.config 轉換

- [Web.config 檔案轉換](../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md)。
- [使用 Visual Studio Web 專案部署的 Web.config 轉換語法](https://msdn.microsoft.com/library/dd465326.aspx)(MSDN)。
- [Web 工具 2012.2-web.config 轉換](https://www.youtube.com/watch?v=HdPK8mxpKEI)（YouTube 影片的 Sayed Hashimi）。 示範如何設定和預覽 Web.config 轉換。
- [如何停用 Web.config 轉換？](https://msdn.microsoft.com/library/ee942158.aspx#disable_web_config_transformation) (MSDN)。
- [何時應該使用 Web Deploy 的參數而不是 Web.config 轉換？](https://msdn.microsoft.com/library/ee942158.aspx#web_deploy_parameters) (MSDN)。
- [Codeplex.com 上發行 （XML 文件轉換） XDT](https://blogs.msdn.com/b/webdev/archive/2013/04/23/xdt-xml-document-transform-released-on-codeplex-com.aspx) （.NET Web 程式開發和工具部落格）。 發表的 Web.config 檔案轉換引擎的原始程式碼，並列出一些使用它的工具。
- [Windows Azure 網站： 應用程式的字串方式和連接字串的運作](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx)（Microsoft Azure 部落格）。 如果您的目的地環境是 Windows Azure 網站，而且您想要轉換，轉換 Web.config 的替代項目`appSettings`或`connectionStrings`。


<a id="webdeployparms"></a>


## <a name="using-web-deploy-parameters-to-change-settings-in-the-destination-web-application-during-deployment"></a>若要變更設定，在目的地 web 應用程式，在部署期間使用 Web Deploy 的參數

- [如何： 使用 Web Deploy Web 部署套件中的參數](https://msdn.microsoft.com/library/ff398068.aspx)(MSDN)。
- [MSDeploy： 如何更新在發佈的應用程式設定為基礎的發行設定檔](http://sedodream.com/2013/03/02/MSDeployHowToUpdateAppSettingsOnPublishBasedOnThePublishProfile.aspx)（Sayed Hashimi 部落格）。 顯示如何將 Web 部署參數到 Visual Studio 發行設定檔。
- [Web 部署參數化](https://www.iis.net/learn/publish/using-web-deploy/web-deploy-parameterization)（IIS.NET 網站）。
- [Web 部署中動作的參數化](http://vishaljoshi.blogspot.com/2010/07/web-deploy-parameterization-in-action.html)（Vishal Joshi 部落格）。
- [Web 部署參數化 vs。Web.config 轉換](http://vishaljoshi.blogspot.com/2010/06/parameterization-vs-webconfig.html)（Vishal Joshi 部落格）。
- [Windows Azure 網站： 應用程式的字串方式和連接字串的運作](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx)（Microsoft Azure 部落格）。 替代 Web 部署參數，如果您的目的地環境是 Windows Azure 網站，而且您想要參數化`appSettings`或`connectionStrings`。


<a id="appoffline"></a>


## <a name="making-sure-an-application-is-off-line-during-deployment"></a>確定應用程式是在部署期間離線

- [使用 Visual Studio 的 ASP.NET Web 部署： 部署程式碼更新](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md)。 請參閱節**將應用程式離線部署期間。**
- [離線應用程式發行前](https://www.iis.net/learn/publish/deploying-application-packages/taking-an-application-offline-before-publishing)（IIS.net 網站）。 說明功能，Web Deploy 3.0，可自動處理應用程式的內建\_offline.htm 檔案。 這項功能不適用於自訂應用程式\_offline.htm 檔案。
- [在發行期間進行 web 應用程式離線](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx)（Sayed Hashimi 部落格）。 如何使用自訂的應用程式的程序自動化\_offline.htm 檔案。
- [Web 發行的更新，離線應用程式和 usechecksum](https://blogs.msdn.com/b/webdev/archive/2013/10/30/web-publishing-updates-for-app-offline-and-usechecksum.aspx) （Microsoft Web 程式開發部落格）。 自動化使用應用程式的另一個選項\_offline.htm 檔案。
- [Web 部署 3.5 RTW](https://blogs.iis.net/msdeploy/archive/2013/07/09/webdeploy-3-5-rtw.aspx) （IIS.net 網站）。 Web 部署 3.5 中的自訂應用程式的新功能\_offline.htm 檔案。


<a id="databasewithweb"></a>


## <a name="deploying-a-database-or-changes-to-a-database-as-part-of-web-application-deployment"></a>將資料庫或變更部署到資料庫作為 web 應用程式部署的一部分

- [在 Visual Studio 中設定資料庫部署](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment)(MSDN)。 可用於部署 web 專案的資料庫選項的概觀。
- [使用 Visual Studio 的 ASP.NET Web 部署](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md)。 分成 12 個單元的教學課程系列，會顯示資料庫的部署使用 dbDacFx 提供者和 Entity Framework Code First 移轉。
- [如何： 部署 Web 專案使用單鍵發行 Visual Studio 中](https://msdn.microsoft.com/library/dd465337.aspx)(MSDN)。
- [將使用成員資格、 OAuth 和 SQL Database 的安全 ASP.NET MVC 5 應用程式部署至 Windows Azure 網站](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)。 長的教學課程，建置和部署的應用程式使用單一的 SQL Server 資料庫的成員資格和應用程式資料。
- [使用 SQL Server Compact 使用 Visual Studio 將 ASP.NET Web 應用程式部署](../web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md)。 分成 12 個單元的教學課程，示範如何部署 SQL Server Compact 資料庫，以及如何從 SQL Server Compact 移轉至 SQL Server 的完整版本。

請參閱部署所建立及安裝 web 部署封裝和部署 web 應用程式使用持續整合 (CI) 程序稍早在此頁面中的 web 應用程式。


<a id="databaseseparate"></a>


## <a name="deploying-a-database-separately-from-web-application-deployment"></a>部署資料庫與 web 應用程式部署分開

- [SQL Server Data Tools](https://msdn.microsoft.com/library/hh272686(v=vs.103).aspx) (MSDN)。
- [在 SQL Server 資料庫專案中納入資料](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx)（SQL Server Data Tools 團隊部落格）。 如何部署資料庫時，部署結構描述和資料。
- [如何將資料庫部署到 Windows Azure](https://docs.microsoft.com/azure/sql-database/sql-database-cloud-migrate) （Microsoft Azure 站台）
- [將資料庫移轉至 Windows Azure SQL Database (先前稱為 SQL Azure)](https://msdn.microsoft.com/library/windowsazure/ee730904.aspx) (MSDN)。
- [將資料庫移轉至 SQL Azure 使用 SSDT](https://blogs.msdn.com/b/ssdt/archive/2012/04/19/migrating-a-database-to-sql-azure-using-ssdt.aspx) （SQL Server Data Tools 團隊部落格）。
- [將以資料為中心的應用程式移轉至 Windows Azure](https://msdn.microsoft.com/library/jj156154.aspx) (MSDN)。
- [SQL Server 資料庫移轉至 Windows Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx) (MSDN)。


<a id="aspnetmembership"></a>


## <a name="deploying-a-web-application-that-uses-aspnet-application-services-such-as-membership-and-profiling"></a>部署使用 ASP.NET 應用程式的 web 應用程式服務，例如成員資格和程式碼剖析

- [將使用成員資格、 OAuth 和 SQL Database 的安全 ASP.NET MVC 5 應用程式部署至 Windows Azure 網站](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)。 長的教學課程，建置和部署的應用程式使用單一的 SQL Server 資料庫的成員資格和應用程式資料。
- [ASP.NET Identity](https://asp.net/identity/)。 ASP.NET 身分識別的資源。
- [使用 Visual Studio 的 ASP.NET Web 部署](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md)。 分成 12 個單元的教學課程，示範如何部署 ASP.NET 成員資格資料庫。
- [設定使用應用程式服務的網站](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-cs.md)。 網站專案也是與相關的 web 應用程式專案。
- [使用者和生產環境網站的角色](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs.md)。 網站專案也是與相關的 web 應用程式專案。


<a id="precompiling"></a>


## <a name="precompiling-for-deployment"></a>部署先行編譯

- [ASP.NET Web 應用程式專案先行編譯概觀](https://msdn.microsoft.com/library/aa983464.aspx)(MSDN)。
- [封裝/發行 Web 索引標籤中，專案屬性](https://msdn.microsoft.com/library/dd410108.aspx)(MSDN)。
- [進階先行編譯設定對話方塊](https://msdn.microsoft.com/library/hh475319.aspx)(MSDN)。


<a id="intranet"></a>


## <a name="deploying-an-intranet-web-application"></a>部署內部網路 web 應用程式

- [在內部部署組織驗證選項 (ADFS) 適用於 Visual Studio 2013 中 ASP.NET](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/) （Vittorio bertocci 部落格。）。
- [如何建立使用 ASP.NET MVC 內部網路網站](https://msdn.microsoft.com/library/gg703322(VS.98).aspx)(MSDN)。 較舊的逐步解說目標寫入器的 Visual Studio 2010 中，不會反映 Visual Studio 2013 中導入的內部網路專案範本中的重大變更。


<a id="automating"></a>


## <a name="automating-common-deployment-tasks-that-are-not-automated-out-of-the-box"></a>根據預設，未自動化的自動化一般部署工作

- [使用 Visual Studio 的 ASP.NET Web 部署： 部署的額外檔案](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files.md)。
- [在 Web 發行設定資料夾權限](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx)（Sayed Hashimi 部落格）。
- [如何擴充以包含 web 專案套件的登錄設定的目標檔案](https://blogs.msdn.com/webdevtools/archive/2010/02/09/how-to-extend-target-file-to-include-registry-settings-for-web-project-package.aspx)（Web 開發工具部落格）。
- [擴充 XML (Web.config) 轉換](http://sedodream.com/2010/09/09/ExtendingXMLWebconfigConfigTransformation.aspx)（Sayed Hashimi 部落格）。 示範如何建立自訂的 XDT 轉換。
- [Web Deployment Tool (MSDeploy) 自訂提供者進行 1](http://sedodream.com/2010/03/11/WebDeploymentToolMSDeployCustomProviderTake1.aspx) （Sayed Hashimi 部落格）。 示範如何建立 Web Deploy 的自訂提供者。
- [如何封裝及部署 COM 元件](https://blogs.msdn.com/webdevtools/archive/2010/03/03/how-to-package-and-deploy-com-component.aspx)（Web 開發工具部落格）。
- [封裝的.NET 組件如何](https://blogs.msdn.com/webdevtools/archive/2010/02/19/how-to-package-com-component.aspx)（Web 開發工具部落格）。 如何部署到 GAC 的組件。
- [所有項目-初始化您 Windows Azure VM 適用於您的網頁伺服器使用 IIS、 Web Deploy 和其他項目編寫](http://www.tugberkugurlu.com/archive/script-out-everything-initialize-your-windows-azure-vm-for-your-web-server-with-iis-web-deploy-and-other-stuff)（Tugberk Ugurlu 部落格）。


<a id="configuringservers"></a>


## <a name="configuring-web-servers-so-that-developers-can-deploy-web-applications-to-them-using-web-deploy"></a>設定 web 伺服器，可讓開發人員可以將部署給他們使用 Web Deploy 的 web 應用程式

- [系統管理員和非系統管理員部署的安裝和設定 Web Deploy](https://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy) （IIS.net 網站）。


<a id="hostingprovider"></a>


## <a name="configuring-servers-for-a-hosting-provider"></a>設定伺服器主控提供者

- [Microsoft ASP.NET 4 主機部署指南](https://go.microsoft.com/fwlink/?LinkId=191365)（Microsoft 下載中心取得）。
- [產生設定檔 XML 檔案](https://www.iis.net/learn/web-hosting/joining-the-web-hosting-gallery/generate-a-profile-xml-file)（IIS.net 網站）。


<a id="troubleshooting"></a>


## <a name="troubleshooting-deployment-problems"></a>疑難排解部署問題

- [疑難排解 Visual Studio 中的 Windows Azure Web Sites](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio) （Microsoft Azure 站台）。
- [使用 Visual Studio 的 ASP.NET Web 部署： 疑難排解](../web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting.md)。
- [疑難排解常見的問題，使用 Web 部署](https://www.iis.net/learn/publish/troubleshooting-web-deploy/troubleshooting-common-problems-with-web-deploy)。
- [Web 部署錯誤碼](https://www.iis.net/learn/publish/troubleshooting-web-deploy/web-deploy-error-codes)（IIS.net 網站）。
- [Web 部署常見問題集適用於 Visual Studio 和 ASP.NET](https://msdn.microsoft.com/library/ee942158.aspx) (MSDN)。
- [核心 IIS 和 ASP.NET Development Server 之間的差異](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md)。
- [開發與生產環境之間的常見組態差異](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs.md)。
- [中度信任 ASP.NET 應用程式裝載](http://www.4guysfromrolla.com/articles/100307-1.aspx)(4 Guy 從 Rolla 站台)。


<a id="gettinghelp"></a>


## <a name="getting-help-with-a-specific-deployment-question"></a>取得特定的部署問題的說明

- [ASP.NET 組態和部署論壇](https://forums.asp.net/26.aspx/1?Configuration and Deployment)。
- [StackOverflow.com](http://www.StackOverflow.com)。


<a id="additional"></a>


## <a name="additional-resources"></a>其他資源

本章節提供可用於進一步了解如何使用 Visual Studio 和 IIS 的部署工具的其他資源的連結。

下列部落格經常包含 Visual Studio web 部署的相關資訊：

- [Microsoft 部落格 web 程式開發工具](https://blogs.msdn.com/b/webdevtools/)。
- [Sayed Hashimi 部落格](http://www.sedodream.com/)。

下列資源提供有關 Web Deploy，Visual Studio 用來執行 web 應用程式專案部署工作的 IIS 架構文件。 您可以詢問有關 Web 部署中的問題[Web Deployment Tool 論壇](https://go.microsoft.com/fwlink/?LinkId=149411)IIS.net 網站上。

- [介紹 Web 部署](https://www.iis.net/learn/publish/using-web-deploy/introduction-to-web-deploy)。
- [安裝和設定 Web 部署](https://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy)。
- [PowerShell 指令碼來自動化 Web 部署設定](https://www.iis.net/learn/publish/using-web-deploy/powershell-scripts-for-automating-web-deploy-setup)。
- [Web Deployment Tool](https://go.microsoft.com/fwlink/?LinkId=151481)。 如需 Web Deploy TechNet 網站上的文件的內容節點的最上層的資料表。 包含有用的參考資訊，但大部分 TechNet 頁面尚未更新多年來。
- [Microsoft.Web.Deployment 命名空間](https://go.microsoft.com/fwlink/?LinkId=148630)。 API 文件，尚未從 1.0 版進行更新。
- [Microsoft Web 部署小組部落格](https://blogs.iis.net/msdeploy/default.aspx)。
- [發行在 IIS.net 網站上的索引標籤](https://www.iis.net/learn/publish)。
