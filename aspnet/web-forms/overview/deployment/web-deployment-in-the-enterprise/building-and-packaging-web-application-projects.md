---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
title: 建置和封裝 Web 應用程式專案 |Microsoft Docs
author: jrjlee
description: 當您想要將 web 應用程式專案部署到遠端伺服器的環境時，您的第一個工作是建置專案，並產生 web 部署 packa...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 94e92f80-a7e3-4d18-9375-ff8be5d666ac
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
msc.type: authoredcontent
ms.openlocfilehash: ff8312d16dff2a9eec9ae909bca5e72d52f17094
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382604"
---
<a name="building-and-packaging-web-application-projects"></a>建置和封裝 Web 應用程式專案
====================
藉由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 當您想要將 web 應用程式專案部署到遠端伺服器的環境時，您的第一個工作是建置專案，並產生 web 部署套件。 本主題會說明 web 應用程式專案的建置程序的運作方式。 特別是，它會說明：
> 
> - 如何 Web 發行管線 (WPP) 擴充建置程序，以包含部署功能。
> - 如何 Internet Information Services (IIS) Web Deployment Tool (Web Deploy)，就會開啟您的 web 應用程式封裝成部署套件。
> - 如何建置和封裝程序的運作方式，以及哪些檔案會建立。


在 Visual Studio 2010 中，WPP 支援 web 應用程式專案的組建和部署程序。 WPP 提供擴充功能的 MSBuild，並讓它利用 Web Deploy 整合的 Microsoft Build Engine (MSBuild) 目標的集。 在 Visual Studio 中，您可以看到這項擴充的功能屬性頁上的 web 應用程式專案。 **封裝/發行 Web**頁面上，搭配**封裝/發行 SQL**頁面上，可讓您設定組建程序完成時您的 web 應用程式專案會封裝部署的方式。

![](building-and-packaging-web-application-projects/_static/image1.png)

## <a name="how-does-the-wpp-work"></a>WPP 如何運作？

如果您看看專案檔的 C# 為基礎的 web 應用程式專案，您可以看到它匯入兩個.targets 檔案。


[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample1.xml)]


第一個**匯入**陳述式是通用於所有 Visual C# 專案。 此檔案中， *Microsoft.CSharp.targets*，包含目標和專為 Visual C# 的工作。 例如，C# 編譯器 (**Csc**) 這裡叫用工作。 *Microsoft.CSharp.targets*依次檔案匯入*Microsoft.Common.targets*檔案。 這會定義目標通用於所有專案，例如**建置**，**重建**，**執行**，**編譯**，和**清除**. 第二個**匯入**陳述式是特定 web 應用程式專案。 *Microsoft.WebApplication.targets*依次檔案匯入*Microsoft.Web.Publishing.targets*檔案。 *Microsoft.Web.Publishing.targets*檔案基本上*是*WPP。 它會定義目標，例如**封裝**並**MSDeployPublish**，叫用 Web Deploy 完成各種部署工作。

若要了解這些其他的目標中使用的方式，請連絡管理員範例解決方案中，開啟*Publish.proj*檔案，並看看**BuildProjects**目標。


[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample2.xml)]


這個目標會使用**MSBuild**工作來建置各種專案。 請注意**DeployOnBuild**並**DeployTarget**屬性：

- **DeployOnBuild = true**屬性基本上表示 「 我想要建置已成功完成時執行的其他目標 」。
- **DeployTarget**屬性會識別您想要執行時的目標名稱**DeployOnBuild**屬性等於**true**。 在此情況下，您指定您想要執行的 MSBuild**封裝**建置專案之後的目標。

**封裝**中所定義的目標*Microsoft.Web.Publishing.targets*檔案。 基本上，這個目標會採用您的 web 應用程式專案的建置輸出，並將它轉換成可以發行至 IIS 網頁伺服器的 web 部署封裝。

> [!NOTE]
> 若要檢視的專案檔 (例如<em>ContactManager.Mvc.csproj</em>) 在 Visual Studio 2010 中，您需要將專案卸載，從您的方案。 在 [<strong>方案總管</strong>] 視窗中，以滑鼠右鍵按一下專案節點，然後<strong>卸載專案</strong>。 同樣地，以滑鼠右鍵按一下專案節點，然後按一下<strong>編輯</strong><em>[專案檔]</em>)。 專案檔會開啟以其原始的 XML 格式。 請記住，當您完成時，請重新載入專案。  
> 如需有關 MSBuild 目標、 工作，並<strong>匯入</strong>陳述式，請參閱[了解專案檔](understanding-the-project-file.md)。 專案檔和 WPP 更深入的簡介，請參閱[在 Microsoft Build Engine： 使用 MSBuild 和 Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi 和 William bartholomew< /，ISBN: 978-0-7356-4524-0。


## <a name="what-is-a-web-deployment-package"></a>什麼是 Web 部署套件？

當您建置和部署 web 應用程式專案，使用 Visual Studio 2010 或直接使用 MSBuild 時，最終結果是通常*web 部署套件*。 Web 部署封裝為.zip 檔案。 它包含的所有項目，IIS 和 Web Deploy 才能重新建立您的 web 應用程式，包括：

- 您的 web 應用程式，包括內容、 資源檔、 組態檔、 JavaScript 及階層式樣式表 (CSS) 的資源，等等編譯的輸出。
- Web 應用程式專案和任何組件參考方案中的專案。
- 若要產生您要部署您的 web 應用程式的任何資料庫的 SQL 指令碼。

之後產生的 web 部署封裝之後，您可以將它發行到 IIS web 伺服器以各種方式。 例如，您可以將它部署從遠端將目標設為 「 Web 部署遠端代理程式服務或 Web 部署處理常式在目的地 web 伺服器上，或您可以使用 IIS 管理員以手動方式匯入目的地 web 伺服器上的套件。 如需有關部署這些方法的詳細資訊，請參閱[選擇 Web 部署的權限方法](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md)。

## <a name="how-does-the-build-process-work"></a>建置程序如何運作？

這會顯示當您建置和封裝 web 應用程式專案時，會發生什麼事：

![](building-and-packaging-web-application-projects/_static/image2.png)

當您建置的 web 應用程式專案時，建置程序會產生名為 *[專案名稱]。SourceManifest.xml*。 專案檔和組建輸出，以及這 *。SourceManifest.xml*檔案會告訴 Web Deploy 它需要包含 web 部署套件中。 這些輸入，使用 Web Deploy 會產生名為 web 部署封裝 *[專案名稱].zip*。

隨 web 部署套件，建置程序會產生可協助您使用封裝的兩個檔案：

- *。 Deploy.cmd*檔案包含一組的參數化 Web Deploy (MSDeploy.exe) 命令，將您的 web 部署套件發佈至遠端 IIS web 伺服器。 執行 *。 deploy.cmd*檔案，使用適當的參數，通常讓您更快和更容易替代來以手動方式建構 MSDeploy.exe 命令自己。
- *之 SetParameters.xml*檔案提供一組 MSDeploy.exe 命令的參數值。 這些值包括屬性，例如，您想要部署封裝，也就是任何服務端點的值，以及連接字串中所定義的 IIS web 應用程式名稱*web.config*檔案和任何部署屬性在專案的 [屬性] 頁面上定義的值。

*之 SetParameters.xml*檔案是以管理部署程序的索引鍵。 這個檔案是以動態方式產生，根據您的 web 應用程式專案的內容。 例如，如果您加入連接字串，您*web.config*檔案，建置程序會自動偵測連接字串，因此，會參數化為部署並建立中的項目*之 SetParameters.xml*檔案，讓您修改連接字串做為部署程序的一部分。 下一個主題中，[網頁套件部署的設定參數](configuring-parameters-for-web-package-deployment.md)，說明更多詳細資料中的這個檔案的角色，並說明不同的方式，在其中您可以修改期間建置和部署它。

> [!NOTE]
> 在 Visual Studio 2010 中，WPP 不支援先行編譯 web 應用程式封裝之前的頁面。 Visual Studio 和 WPP 的下一個版本會包含先行編譯 web 應用程式做為封裝選項的功能。


## <a name="conclusion"></a>結論

本主題提供在 Visual Studio 2010 中的 web 應用程式專案的建置和封裝程序的概觀。 它說明如何 WPP 可讓您叫用 MSBuild，Web Deploy 命令，它說明如何建置和封裝程序運作。

一旦您已建立 web 部署封裝下, 一個步驟是將它部署。 如需詳細資訊，請參閱[網頁套件部署的設定參數](configuring-parameters-for-web-package-deployment.md)並[部署網頁套件](deploying-web-packages.md)。

## <a name="further-reading"></a>進一步閱讀

在本教學課程中的下一個主題[網頁套件部署的設定參數](configuring-parameters-for-web-package-deployment.md)並[部署網頁套件](deploying-web-packages.md)，指導您如何使用您已建立的 web 套件。 在此系列中，最後一個教學課程[進階企業 Web 部署](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md)，提供如何自訂和疑難排解封裝程序的指引。

專案檔和 WPP 更深入的簡介，請參閱[在 Microsoft Build Engine： 使用 MSBuild 和 Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi 和 William bartholomew< /，ISBN: 978-0-7356-4524-0。

> [!div class="step-by-step"]
> [上一頁](understanding-the-build-process.md)
> [下一頁](configuring-parameters-for-web-package-deployment.md)
