---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
title: 建立和封裝 Web 應用程式專案 |Microsoft 文件
author: jrjlee
description: 當您想要將 web 應用程式專案部署到遠端伺服器環境時，您的第一個工作是建置專案，並產生 web 部署 packa...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 94e92f80-a7e3-4d18-9375-ff8be5d666ac
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
msc.type: authoredcontent
ms.openlocfilehash: d630e1776607bd0bd7c61e1f0f7234ef58c7533b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="building-and-packaging-web-application-projects"></a>建立和封裝 Web 應用程式專案
====================
由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 當您想要將 web 應用程式專案部署到遠端伺服器環境時，您的第一個工作是建置專案，並產生 web 部署套件。 本主題說明如何建置程序適用於 web 應用程式專案。 特別是，它會說明：
> 
> - 如何 Web 發行管線 (WPP) 擴充建置程序，以包含部署的功能。
> - 如何網際網路資訊服務 (IIS) Web Deployment Tool (Web Deploy) 會開啟您的 web 應用程式，到部署套件。
> - 如何建置和封裝程序運作，並建立哪些檔案。


在 Visual Studio 2010、 WPP 支援 web 應用程式專案的建置和部署程序。 WPP 提供擴充功能的 MSBuild 並啟用整合與 Web Deploy 的 Microsoft Build Engine (MSBuild) 目標的集。 在 Visual Studio 中，您可以看到這項擴充的功能屬性頁上 web 應用程式專案。 **封裝/發行 Web**頁面上，搭配**封裝/發行 SQL**頁面上，可讓您設定建置程序完成後您的 web 應用程式專案會封裝部署的方式。

![](building-and-packaging-web-application-projects/_static/image1.png)

## <a name="how-does-the-wpp-work"></a>WPP 的運作方式為何？

如果您看一下專案檔的 C# 為基礎的 web 應用程式專案，您可以看到它匯入兩個.targets 檔案。


[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample1.xml)]


第一個**匯入**陳述式是通用於所有 Visual C# 專案。 這個檔案， *Microsoft.CSharp.targets*，包含目標和工作的特定 Visual C#。 例如，C# 編譯器 (**Csc**) 這裡叫用工作。 *Microsoft.CSharp.targets*依次檔案匯入*Microsoft.Common.targets*檔案。 這會定義目標通用於所有專案，例如**建置**，**重建**，**執行**，**編譯**，和**清除**. 第二個**匯入**陳述式是特定 web 應用程式專案。 *Microsoft.WebApplication.targets*依次檔案匯入*Microsoft.Web.Publishing.targets*檔案。 *Microsoft.Web.Publishing.targets*檔案基本上*是*WPP。 它會定義目標，例如**封裝**和**MSDeployPublish**，會叫用 Web Deploy 完成各種部署工作。

若要了解這些其他的目標中的使用方式，請連絡管理員範例方案中，開啟*Publish.proj*檔案，並看看**BuildProjects**目標。


[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample2.xml)]


這個目標會使用**MSBuild**來建立各種專案的工作。 請注意**DeployOnBuild**和**DeployTarget**屬性：

- **DeployOnBuild = true**屬性基本上是表示 「 我想要建置成功完成時執行的其他目標 」。
- **DeployTarget**屬性會識別您想要執行時的目標名稱**DeployOnBuild**屬性等於**true**。 在此情況下，您會指定您想要執行 MSBuild**封裝**建置專案之後的目標。

**封裝**中定義目標*Microsoft.Web.Publishing.targets*檔案。 基本上，此目標會採用您的 web 應用程式專案的建置輸出，並將它轉換成可以發行至 IIS web 伺服器的 web 部署封裝。

> [!NOTE]
> 若要檢視的專案檔 (例如， <em>ContactManager.Mvc.csproj</em>) 在 Visual Studio 2010 中，您必須先卸載專案，從您的方案。 在<strong>方案總管 中</strong>視窗中，以滑鼠右鍵按一下專案節點，然後<strong>卸載專案</strong>。 同樣地，以滑鼠右鍵按一下專案節點，然後按一下<strong>編輯</strong><em>[專案檔]</em>)。 未經處理的 XML 格式，將會開啟專案檔。 請記住，當您完成時重新載入專案。  
> 如需 MSBuild 目標，工作詳細資訊和<strong>匯入</strong>陳述式，請參閱[了解專案檔](understanding-the-project-file.md)。 專案檔和 WPP 的更深入簡介，請參閱[內 Microsoft Build Engine： 使用 MSBuild 和 Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi 和 William Bartholomew，ISBN: 978-0-7356-4524-0。


## <a name="what-is-a-web-deployment-package"></a>Web 部署套件為何？

當您建置和部署 web 應用程式專案中，使用 Visual Studio 2010 或直接使用 MSBuild 時，最終結果通常是*web 部署套件*。 Web 部署封裝為.zip 檔案。 它包含的所有項目 IIS 和 Web Deploy 才能重新建立您的 web 應用程式，包括：

- Web 應用程式，包括內容、 資源檔、 組態檔、 JavaScript 和階層式樣式表 (CSS) 的資源，等編譯的輸出。
- 您的 web 應用程式專案和組件參考您的方案中的專案。
- 若要產生您要部署 web 應用程式的任何資料庫的 SQL 指令碼。

之後產生的 web 部署套件之後，您可以將它發行至 IIS web 伺服器上以各種方式。 例如，您可以將它部署從遠端將目標設為 Web 部署遠端代理程式服務或 Web 部署處理常式在目的地 web 伺服器上，或您可以使用 IIS 管理員以手動方式匯入目的地 web 伺服器上的套件。 如需有關部署這些方法的詳細資訊，請參閱[選擇 Web 部署的權限方法](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md)。

## <a name="how-does-the-build-process-work"></a>在建置程序的運作方式為何？

以下會示範當您建置並封裝 web 應用程式專案時，會發生什麼情況：

![](building-and-packaging-web-application-projects/_static/image2.png)

當您建置的 web 應用程式專案時，建置程序會產生名為*[專案名稱]。SourceManifest.xml*。 專案檔和組建輸出，以及這*。SourceManifest.xml*檔案會告知 Web Deploy 它需要包含 web 部署套件中。 這些輸入，使用 Web Deploy 會產生名為 web 部署套件*[專案名稱].zip*。

Web 部署套件，以及建置程序會產生兩個檔案，可協助您使用的封裝：

- *。 Deploy.cmd*檔案包含一組 web 部署套件發佈到遠端的 IIS 網頁伺服器的參數化 Web Deploy (MSDeploy.exe) 的命令。 執行*。 deploy.cmd*檔案，以適當的參數，通常會提供更快速和更容易替代來手動建構 MSDeploy.exe 命令自己。
- *SetParameters.xml*檔提供一組 MSDeploy.exe 命令的參數值。 這些值包括如 IIS web 應用程式可供您想要部署封裝時，任何服務端點的值，但在中定義連接字串名稱屬性*web.config*檔和任何部署屬性在專案屬性頁上所定義的值。

*SetParameters.xml*檔案是管理部署程序的關鍵。 這個檔案是以動態方式產生，根據您的 web 應用程式專案的內容。 例如，如果您加入連接字串，您*web.config*檔案，建置程序會自動偵測連接字串，因此，參數化部署並建立中的項目*SetParameters.xml*檔案，以讓您修改連接字串做為部署程序的一部分。 下一個主題[Web 封裝部署的設定參數](configuring-parameters-for-web-package-deployment.md)、 說明此檔案更詳細的角色，以及不同的方式，在其中您可以修改在建置和部署期間它。

> [!NOTE]
> 在 Visual Studio 2010、 WPP 不支援先行編譯封裝之前的 web 應用程式中的頁面。 下一版的 Visual Studio 和 WPP 會包含先行編譯 web 應用程式做為封裝選項的能力。


## <a name="conclusion"></a>結論

本主題提供 Visual Studio 2010 中的 web 應用程式專案的建置和封裝程序的概觀。 它說明如何 WPP 可讓您叫用 Web Deploy 命令從 MSBuild，和它說明如何建置和封裝程序運作。

建立 web 部署套件之後下, 一步是將其部署。 如需詳細資訊，請參閱[Web 封裝部署的設定參數](configuring-parameters-for-web-package-deployment.md)和[部署 Web 封裝](deploying-web-packages.md)。

## <a name="further-reading"></a>進一步閱讀

在本教學課程中的下一個主題[Web 封裝部署的設定參數](configuring-parameters-for-web-package-deployment.md)和[部署 Web 封裝](deploying-web-packages.md)，提供如何使用您已建立此 web 封裝的指引。 在這一系列，最終的教學課程[進階企業 Web 部署](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md)，提供有關如何自訂及疑難排解封裝程序指引。

專案檔和 WPP 的更深入簡介，請參閱[內 Microsoft Build Engine： 使用 MSBuild 和 Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi 和 William Bartholomew，ISBN: 978-0-7356-4524-0。

> [!div class="step-by-step"]
> [上一頁](understanding-the-build-process.md)
> [下一頁](configuring-parameters-for-web-package-deployment.md)
