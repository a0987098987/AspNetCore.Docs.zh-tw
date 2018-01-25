---
uid: whitepapers/aspnet-web-deployment-content-map
title: "ASP.NET Web 部署-建議資源 |Microsoft 文件"
author: rick-anderson
description: "本主題提供有關如何部署的資源 （發行） ASP.NET web 文件連結至 IIS 的應用程式，使用 Visual Studio 2010、 Visual Web De..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2014
ms.topic: article
ms.assetid: 58b583cd-c4ab-47a3-8527-8c92c298c91f
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-web-deployment-content-map
msc.type: content
ms.openlocfilehash: 78ff183394b5ff92f789b50551d01d28f9bff93b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-web-deployment---recommended-resources"></a>ASP.NET Web 部署-建議使用的資源
====================
> 本主題提供有關如何部署的資源 （發行） ASP.NET web 文件連結至 IIS 的應用程式，使用 Visual Studio 2010、 Visual Web Developer 2010 及更新版本。
> 
> 如果您知道好的部落格文章， [stackoverflow](http://stackoverflow.com)執行緒或任何其他連結，會很有用，[傳送一封電子郵件給我們](mailto:aspnetue@microsoft.com?subject=Deployment Content Map)與連結。
> 
> > [!NOTE] 
> > 
> > 許多這些資源的描述安裝最新版的時，才可以使用的部署功能[Visual Studio Web 發行 Update](https://go.microsoft.com/fwlink/?LinkID=208120)。 只在 Visual Studio 2012 或 Visual Studio 2013 的一些功能可用。


此主題包括下列章節：

- [了解 web 專案的部署選項](#understanding)
- [尋找裝載 ASP.NET 應用程式提供者](#findinghosting)
- [從 Visual Studio web 應用程式部署](#fromvs)
- [部署 web 應用程式建立及安裝 web 部署套件](#package)
- [部署 web 應用程式使用持續整合 (CI) 程序](#ci)
- [若要變更目的地 Web.config 檔或 app.config 檔案中的設定，在部署期間使用 Web.config 轉換](#transforms)
- [若要變更設定，在部署期間在目的地 web 應用程式使用 Web Deploy 參數](#webdeployparms)
- [確定應用程式是在部署期間離線](#appoffline)
- [部署資料庫或變更資料庫做為 web 應用程式部署的一部分](#databasewithweb)
- [部署資料庫，分開 web 應用程式部署](#databaseseparate)
- [部署 web 應用程式使用 ASP.NET 應用程式服務，例如成員資格和程式碼剖析](#aspnetmembership)
- [針對部署先行編譯](#precompiling)
- [內部網路 web 應用程式部署](#intranet)
- [自動化一般不會立即可用的自動化的部署工作](#automating)
- [設定 web 伺服器，讓開發人員可以使用 Web Deploy web 應用程式](#configuringservers)
- [設定伺服器為主控提供者](#hostingprovider)
- [疑難排解部署問題](#troubleshooting)
- [取得特定的部署問題的說明](#gettinghelp)
- [其他資源](#additional)


<a id="understanding"></a>


## <a name="understanding-deployment-options-for-web-projects"></a>了解 web 專案的部署選項

- [Visual Studio 和 ASP.NET 的 web 部署概觀](https://msdn.microsoft.com/library/dd394698.aspx)(MSDN)。
- [如何部署 Windows Azure 網站](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy)。 說明資源部署至 Windows Azure 網站，包括 web 專案的選項和連結[持續傳遞](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md)(從自動化[原始檔控制](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md)) 也可使用 Visual Studio。
- [Visual Studio 2012 Web 發行改進](../visual-studio/overview/2012/visual-studio-2012-web-publishing-improvements.md)（由 Scott Hanselman 主講）。
- [用於 VS 2010 中的 Web 部署概觀文章](http://vishaljoshi.blogspot.com/2009/09/overview-post-for-web-deployment-in-vs.html)（Vishal Joshi 部落格）。 較舊的部落格文章，但它會連結到的資訊仍與 Visual Studio 2012 的 Visual Studio 2010 資源部分。


<a id="findinghosting"></a>


## <a name="finding-hosting-providers-for-an-aspnet-application"></a>尋找裝載 ASP.NET 應用程式提供者

- [ASP.NET 裝載](https://asp.net/hosting)


<a id="fromvs"></a>


## <a name="deploying-a-web-application-from-visual-studio"></a>從 Visual Studio web 應用程式部署

- [如何部署 Windows Azure 網站](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy)。 說明選項，並提供資源的連結部署到 Windows Azure Web Sites 的 web 專案。 包括從 Visual Studio 部署的相關區段。
- [使用 Visual Studio 的 ASP.NET Web 部署](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md)。 12 一部分的教學課程，示範如何部署 web 應用程式與 SQL Server 資料庫。 資料庫的部署使用的 dbDacFx 提供者 」 和 「 Entity Framework Code First 移轉。 也包含下列資訊[Web.config 檔案轉換](../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md)，[部署個別的檔案](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md#specificfiles)，[命令列部署](../web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment.md)，和[如何自訂 Visual Studio web 發行管線藉由編輯.pubxml 檔案](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files.md)。 適用於所有的 ASP.NET web 專案，包括 Web Forms、 MVC、 及 Web 應用程式開發介面）。
- [如何： 部署 Web 專案使用單鍵發行 Visual Studio 中](https://msdn.microsoft.com/library/dd465337.aspx)（參考 Visual Studio Web Publish 精靈的資訊）。
- [部署 ASP.NET Web 應用程式與 SQL Server Compact 使用 Visual Studio](../web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md)。 這是舊版的**ASP.NET Web 部署使用 Visual Studio**列在這個區段的最上層。 主要用於現在如何部署 SQL Server Compact 資料庫，以及如何從 SQL Server Compact 移轉至 SQL Server 的完整版本的相關資訊。
- [.NET 多層式應用程式使用的儲存體資料表、 佇列和 Blob](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) （Microsoft Azure 站台）。 5 部分的教學課程，示範如何建立 MVC 專案，並將它部署至 Windows Azure 雲端服務。


<a id="package"></a>
## <a name="deploying-a-web-application-by-creating-and-installing-a-web-deployment-package"></a>部署 web 應用程式建立及安裝 web 部署套件

- [如何： 在 Visual Studio 中建立 Web 部署套件](https://msdn.microsoft.com/library/dd465323.aspx)(MSDN)。
- [如何： 建立使用 deploy.cmd 檔的部署套件的 Visual studio 的安裝](https://msdn.microsoft.com/library/ff356104.aspx)(MSDN)。
- [使用 Web Deploy 封裝部署的開發人員方塊上的 IIS 和協力廠商主機](http://sedodream.com/2011/11/08/UsingAWebDeployPackageToDeployToIISOnTheDevBoxAndToAThirdPartyHost.aspx)（Sayed Hashimi 部落格）。 如何使用 IIS 管理員在 IIS 中的部署套件安裝在本機電腦上，並在裝載公司支援 IIS 管理員進行遠端管理。
- [建立 Web 部署套件從 Visual Studio 2010](https://www.iis.net/learn/publish/using-web-deploy/building-a-web-deploy-package-from-visual-studio-2010) （IIS.NET 網站）。 包含命令列封裝建立和安裝的指示。
- [封裝一次發行隨處](http://sedodream.com/2012/03/14/PackageWebUpdatedAndVideoBelow.aspx)（Sayed Hashimi 部落格）。 導入了可自動化轉換多個目的地環境中，Web.config 檔案的程序，以便您可以將一個套件部署到多部伺服器的 NuGet 封裝。 另請參閱[PackageWeb 視訊](https://www.youtube.com/watch?v=-LvUJFI8CzM)由 Sayed Hashimi。

另請參閱下一節。


<a id="ci"></a>


## <a name="deploying-a-web-application-using-a-continuous-integration-ci-process"></a>部署 web 應用程式使用持續整合 (CI) 程序

- [持續整合與持續傳遞 （使用 Windows Azure 建置實際的雲端應用程式）。](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) 導入了持續整合及連續傳遞的電子書章節。
- [如何部署 Windows Azure 網站](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy)。 說明資源部署至 Windows Azure 網站的 web 專案的選項和連結。 包含有關自動從原始檔控制部署 > 一節。
- [部署企業案例中的 Web 應用程式](../web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md)。 40 組件的教學課程，示範如何自動化部署中使用 Visual Studio 2010 和 Team Foundation Server 2010 CI 程序。
- [Microsoft Build 引擎內： 使用 MSBuild 和 Team Foundation Build Sayed Hashimi 和 William Bartholomew](http://msbuildbook.com)。 這是活頁簿，而不需要 web 資源，但它是不可或缺的指南以了解如何設定 MSBuild 的連續整合案例。
- [MSBuild 延伸模組組件](https://github.com/mikefourie/MSBuildExtensionPack)。 包含部署的工作。
- [Team Foundation 組建自訂指南](https://aka.ms/vsarsolutions)。 設定 Team Foundation Server 的文件 ALM Ranger 所涵蓋 web 部署，而且包含教學課程和影片。
- [SlowCheetah XML 轉換從 CI 伺服器](http://sedodream.com/2011/12/12/SlowCheetahXMLTransformsFromACIServer.aspx)（Sayed Hashimi 部落格）。 說明如何使用 SlowCheetah，Visual Studio 增益集的轉換 app.config 和其他 XML 檔案。

另請參閱[確定應用程式是在部署期間離線](aspnet-web-deployment-content-map.md#appoffline)本頁稍後的。


<a id="transforms"></a>


## <a name="using-webconfig-transformations-to-change-settings-in-the-destination-webconfig-file-or-appconfig-file-during-deployment"></a>若要變更目的地 Web.config 檔或 app.config 檔案中的設定，在部署期間使用 Web.config 轉換

- [Web.config 檔案轉換](../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md)。
- [使用 Visual Studio Web 專案部署的 Web.config 轉換語法](https://msdn.microsoft.com/library/dd465326.aspx)(MSDN)。
- [Web 工具 2012.2-web.config 轉換](https://www.youtube.com/watch?v=HdPK8mxpKEI)（YouTube 影片所 Sayed Hashimi）。 示範如何設定並預覽 Web.config 轉換。
- [如何停用 Web.config 轉換？](https://msdn.microsoft.com/library/ee942158.aspx#disable_web_config_transformation) (MSDN)。
- [何時應該使用 Web Deploy 參數而不是 Web.config 轉換？](https://msdn.microsoft.com/library/ee942158.aspx#web_deploy_parameters) (MSDN)。
- [Codeplex.com 上釋出 （XML 文件轉換） XDT](https://blogs.msdn.com/b/webdev/archive/2013/04/23/xdt-xml-document-transform-released-on-codeplex-com.aspx) （.NET Web 開發及工具部落格）。 宣佈可用性的 Web.config 檔案轉換引擎的原始程式碼，並列出一些使用它的工具。
- [Windows Azure Web Sites： 應用程式的字串方式和連接字串工作](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx)（Microsoft Azure 部落格）。 如果目的地環境是 Windows Azure 網站，而且您想要轉換來轉換 Web.config 替代`appSettings`或`connectionStrings`。


<a id="webdeployparms"></a>


## <a name="using-web-deploy-parameters-to-change-settings-in-the-destination-web-application-during-deployment"></a>若要變更設定，在部署期間在目的地 web 應用程式使用 Web Deploy 參數

- [如何： 使用 Web Deploy Web 部署套件中的參數](https://msdn.microsoft.com/library/ff398068.aspx)(MSDN)。
- [MSDeploy： 如何更新上發行的應用程式設定為基礎的發行設定檔](http://sedodream.com/2013/03/02/MSDeployHowToUpdateAppSettingsOnPublishBasedOnThePublishProfile.aspx)（Sayed Hashimi 部落格）。 顯示如何整合 Web deploy 參數到 Visual Studio 發行設定檔。
- [Web 部署參數化](https://www.iis.net/learn/publish/using-web-deploy/web-deploy-parameterization)（IIS.NET 網站）。
- [Web 部署中動作的參數化](http://vishaljoshi.blogspot.com/2010/07/web-deploy-parameterization-in-action.html)（Vishal Joshi 部落格）。
- [Web 部署參數化 vs。Web.config 轉換](http://vishaljoshi.blogspot.com/2010/06/parameterization-vs-webconfig.html)（Vishal Joshi 部落格）。
- [Windows Azure Web Sites： 應用程式的字串方式和連接字串工作](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx)（Microsoft Azure 部落格）。 替代 Web 部署參數，如果目的地環境是 Windows Azure 網站，而且您想要參數化`appSettings`或`connectionStrings`。


<a id="appoffline"></a>


## <a name="making-sure-an-application-is-off-line-during-deployment"></a>確定應用程式是在部署期間離線

- [使用 Visual Studio 的 ASP.NET Web 部署： 部署程式碼更新](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md)。 請參閱節**應用程式離線期間部署。**
- [離線應用程式發行前](https://www.iis.net/learn/publish/deploying-application-packages/taking-an-application-offline-before-publishing)（IIS.net 網站）。 說明功能，Web Deploy 3.0 可自動處理應用程式的內建\_offline.htm 檔案。 這項功能不適用於自訂應用程式\_offline.htm 檔案。
- [在發行期間進行 web 應用程式離線](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx)（Sayed Hashimi 部落格）。 如何使用自訂的應用程式的程序自動化\_offline.htm 檔案。
- [Web 發行的更新，離線應用程式和 usechecksum](https://blogs.msdn.com/b/webdev/archive/2013/10/30/web-publishing-updates-for-app-offline-and-usechecksum.aspx) （Microsoft Web 開發部落格）。 另一種自動化的應用程式使用\_offline.htm 檔案。
- [Web 部署 3.5 RTW](https://blogs.iis.net/msdeploy/archive/2013/07/09/webdeploy-3-5-rtw.aspx) （IIS.net 網站）。 新功能的自訂應用程式 Web 部署 3.5\_offline.htm 檔案。


<a id="databasewithweb"></a>


## <a name="deploying-a-database-or-changes-to-a-database-as-part-of-web-application-deployment"></a>部署資料庫或變更資料庫做為 web 應用程式部署的一部分

- [Visual Studio 中設定資料庫部署](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment)(MSDN)。 部署 web 專案的資料庫選項的概觀。
- [使用 Visual Studio 的 ASP.NET Web 部署](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md)。 教學課程系列 12 一部分，會顯示資料庫部署使用 dbDacFx 提供者和 Entity Framework Code First 移轉。
- [如何： 部署 Web 專案中使用單鍵發行 Visual Studio 中](https://msdn.microsoft.com/library/dd465337.aspx)(MSDN)。
- [將成員資格、 OAuth、 與 SQL Database 的安全的 ASP.NET MVC 5 應用程式部署至 Windows Azure 網站](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)。 完整的教學課程，建置和部署的應用程式使用單一的 SQL Server 資料庫的成員資格和應用程式資料。
- [部署 ASP.NET Web 應用程式與 SQL Server Compact 使用 Visual Studio](../web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md)。 12 一部分的教學課程，示範如何部署 SQL Server Compact 資料庫以及如何從 SQL Server Compact 移轉至 SQL Server 的完整版本。

也可以建立及安裝 web 部署套件及部署 web 應用程式使用持續整合 (CI) 程序稍早在此頁面中部署 web 應用程式，請參閱。


<a id="databaseseparate"></a>


## <a name="deploying-a-database-separately-from-web-application-deployment"></a>部署資料庫，分開 web 應用程式部署

- [SQL Server Data Tools](https://msdn.microsoft.com/library/hh272686(v=vs.103).aspx) (MSDN)。
- [在 SQL Server 資料庫專案中包含資料](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx)（SQL Server Data Tools 團隊部落格）。 如何部署資料庫時，部署結構描述和資料。
- [如何將資料庫部署至 Windows Azure](https://docs.microsoft.com/azure/sql-database/sql-database-cloud-migrate) （Microsoft Azure 站台）
- [將資料庫移轉至 Windows Azure SQL Database (先前稱為 SQL Azure)](https://msdn.microsoft.com/library/windowsazure/ee730904.aspx) (MSDN)。
- [將資料庫移轉至 SQL Azure 使用 SSDT](https://blogs.msdn.com/b/ssdt/archive/2012/04/19/migrating-a-database-to-sql-azure-using-ssdt.aspx) （SQL Server Data Tools 團隊部落格）。
- [將資料中心的應用程式移轉至 Windows Azure](https://msdn.microsoft.com/library/jj156154.aspx) (MSDN)。
- [將 SQL Server 資料庫移轉至 Windows Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx) (MSDN)。


<a id="aspnetmembership"></a>


## <a name="deploying-a-web-application-that-uses-aspnet-application-services-such-as-membership-and-profiling"></a>部署 web 應用程式使用 ASP.NET 應用程式服務，例如成員資格和程式碼剖析

- [將成員資格、 OAuth、 與 SQL Database 的安全的 ASP.NET MVC 5 應用程式部署至 Windows Azure 網站](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)。 完整的教學課程，建置和部署的應用程式使用單一的 SQL Server 資料庫的成員資格和應用程式資料。
- [ASP.NET Identity](https://asp.net/identity/)。 ASP.NET 識別的資源。
- [使用 Visual Studio 的 ASP.NET Web 部署](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md)。 12 一部分的教學課程，示範如何部署 ASP.NET 成員資格資料庫。
- [設定網站使用應用程式服務](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-cs.md)。 網站專案但也很重要的 web 應用程式專案。
- [使用者和角色生產網站上的](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs.md)。 網站專案但也很重要的 web 應用程式專案。


<a id="precompiling"></a>


## <a name="precompiling-for-deployment"></a>針對部署先行編譯

- [ASP.NET Web 應用程式專案先行編譯概觀](https://msdn.microsoft.com/library/aa983464.aspx)(MSDN)。
- [封裝/發行 Web 索引標籤中，專案屬性](https://msdn.microsoft.com/library/dd410108.aspx)(MSDN)。
- [進階設定 對話方塊的先行編譯](https://msdn.microsoft.com/library/hh475319.aspx)(MSDN)。


<a id="intranet"></a>


## <a name="deploying-an-intranet-web-application"></a>內部網路 web 應用程式部署

- [使用內部部署組織的驗證選項 (ADFS) 與 Visual Studio 2013 中的 ASP.NET](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/) （部落格由 Vittorio Bertocci）。
- [如何建立使用 ASP.NET MVC 內部網路網站](https://msdn.microsoft.com/library/gg703322(VS.98).aspx)(MSDN)。 較舊的逐步解說寫入 for Visual Studio 2010，並不會反映在內部網路專案範本在 Visual Studio 2013 中導入重大變更。


<a id="automating"></a>


## <a name="automating-common-deployment-tasks-that-are-not-automated-out-of-the-box"></a>自動化一般不會立即可用的自動化的部署工作

- [使用 Visual Studio 的 ASP.NET Web 部署： 部署的額外檔案](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files.md)。
- [設定資料夾的權限 Web Publish](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) （Sayed Hashimi 部落格）。
- [如何將目標檔案擴充為包含 web 專案套件的登錄設定](https://blogs.msdn.com/webdevtools/archive/2010/02/09/how-to-extend-target-file-to-include-registry-settings-for-web-project-package.aspx)（Web Development Tools 部落格）。
- [擴充 XML (Web.config) 轉換](http://sedodream.com/2010/09/09/ExtendingXMLWebconfigConfigTransformation.aspx)（Sayed Hashimi 部落格）。 示範如何建立自訂的 XDT 轉型。
- [Web Deployment Tool (MSDeploy) 自訂提供者需要 1](http://sedodream.com/2010/03/11/WebDeploymentToolMSDeployCustomProviderTake1.aspx) （Sayed Hashimi 部落格）。 示範如何建立 Web Deploy 的自訂提供者。
- [如何封裝及部署 COM 元件](https://blogs.msdn.com/webdevtools/archive/2010/03/03/how-to-package-and-deploy-com-component.aspx)（Web Development Tools 部落格）。
- [封裝的.NET 組件如何](https://blogs.msdn.com/webdevtools/archive/2010/02/19/how-to-package-com-component.aspx)（Web Development Tools 部落格）。 如何將組件部署到 GAC。
- [編寫所有內容，初始化您 Windows Azure VM 的 IIS、 Web Deploy 和其他個人資料使用您的 Web 伺服器的指令碼](http://www.tugberkugurlu.com/archive/script-out-everything-initialize-your-windows-azure-vm-for-your-web-server-with-iis-web-deploy-and-other-stuff)（Tugberk Ugurlu 部落格）。


<a id="configuringservers"></a>


## <a name="configuring-web-servers-so-that-developers-can-deploy-web-applications-to-them-using-web-deploy"></a>設定 web 伺服器，讓開發人員可以使用 Web Deploy web 應用程式

- [系統管理員和非系統管理員部署的安裝和設定 Web Deploy](https://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy) （IIS.net 網站）。


<a id="hostingprovider"></a>


## <a name="configuring-servers-for-a-hosting-provider"></a>設定伺服器為主控提供者

- [Microsoft ASP.NET 4 主機部署指南](https://go.microsoft.com/fwlink/?LinkId=191365)（Microsoft 下載中心）。
- [產生設定檔 XML 檔](https://www.iis.net/learn/web-hosting/joining-the-web-hosting-gallery/generate-a-profile-xml-file)（IIS.net 網站）。


<a id="troubleshooting"></a>


## <a name="troubleshooting-deployment-problems"></a>疑難排解部署問題

- [疑難排解 Visual Studio 中的 Windows Azure Web Sites](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio) （Microsoft Azure 站台）。
- [使用 Visual Studio 的 ASP.NET Web 部署： 疑難排解](../web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting.md)。
- [疑難排解常見問題 Web 部署](https://www.iis.net/learn/publish/troubleshooting-web-deploy/troubleshooting-common-problems-with-web-deploy)。
- [Web 部署錯誤碼](https://www.iis.net/learn/publish/troubleshooting-web-deploy/web-deploy-error-codes)（IIS.net 網站）。
- [Web 部署常見問題集適用於 Visual Studio 和 ASP.NET](https://msdn.microsoft.com/library/ee942158.aspx) (MSDN)。
- [核心 IIS 和 ASP.NET 程式開發伺服器之間的差異](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md)。
- [一般組態差異開發和生產](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs.md)。
- [裝載在中度信任 ASP.NET 應用程式](http://www.4guysfromrolla.com/articles/100307-1.aspx)(4 Guy 從 Rolla 站台)。


<a id="gettinghelp"></a>


## <a name="getting-help-with-a-specific-deployment-question"></a>取得特定的部署問題的說明

- [ASP.NET 組態和部署論壇](https://forums.asp.net/26.aspx/1?Configuration and Deployment)。
- [StackOverflow.com](http://www.StackOverflow.com)。


<a id="additional"></a>


## <a name="additional-resources"></a>其他資源

本節提供可用於進一步了解如何使用 Visual Studio 和 IIS 部署工具的其他資源連結。

下列部落格通常包含 Visual Studio web 部署的相關資訊：

- [Microsoft 部落格 web 開發工具](https://blogs.msdn.com/b/webdevtools/)。
- [Sayed Hashimi 部落格](http://www.sedodream.com/)。

下列資源提供有關 Web Deploy，Visual Studio 用來執行 web 應用程式專案的部署工作的 IIS framework 文件。 您可以詢問有關 Web 部署中的問題[Web Deployment Tool 論壇](https://go.microsoft.com/fwlink/?LinkId=149411)IIS.net 網站上。

- [Introduction to Web 部署](https://www.iis.net/learn/publish/using-web-deploy/introduction-to-web-deploy)。
- [安裝和設定 Web 部署](https://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy)。
- [PowerShell 指令碼自動化 Web 部署安裝](https://www.iis.net/learn/publish/using-web-deploy/powershell-scripts-for-automating-web-deploy-setup)。
- [Web Deployment Tool](https://go.microsoft.com/fwlink/?LinkId=151481)。 Web Deploy TechNet 網站上的文件的內容節點的最上層的資料表。 包含的實用參考資訊，但大部分的 TechNet 頁面尚未更新年。
- [Microsoft.Web.Deployment 命名空間](https://go.microsoft.com/fwlink/?LinkId=148630)。 API 文件，尚未自 1.0 版進行更新。
- [Microsoft Web 部署團隊部落格](https://blogs.iis.net/msdeploy/default.aspx)。
- [IIS.net 網站中發佈 索引標籤](https://www.iis.net/learn/publish)。
