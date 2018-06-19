---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
title: 手動安裝 Web 封裝 |Microsoft 文件
author: jrjlee
description: 本主題描述如何手動匯入網際網路資訊服務 (IIS) web 部署套件。 主題建置並封裝 Web 某個應用程式...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f11d22a7-5d32-4ad0-8a9b-276460a61c06
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
msc.type: authoredcontent
ms.openlocfilehash: 4a28ea92b22e4928e41a39a8a91b62bfa4f08175
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890212"
---
<a name="manually-installing-web-packages"></a>手動安裝 Web 封裝
====================
由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主題描述如何手動匯入網際網路資訊服務 (IIS) web 部署套件。
> 
> 本主題[建置和封裝 Web 應用程式專案](building-and-packaging-web-application-projects.md)描述如何在 IIS Web Deployment Tool (Web Deploy)，搭配 Microsoft Build Engine (MSBuild) 及發行 Web 管線 (WPP)，可讓您封裝程式將單一 zip 檔案的 web 應用程式專案。 這個檔案，通常稱為 web 部署套件 （或 只部署套件），包含所有的內容與組態資訊才能重新建立您的 web 應用程式，網頁伺服器上需要 IIS。
> 
> 一旦您已建立 web 部署套件，您可以將它發行至 IIS 伺服器上以各種方式。 在許多案例中，您會想要利用 MSBuild、 WPP，和 Web Deploy 來建立和自動化或單一步驟建置和部署程序的一部分，從遠端安裝 web 封裝之間的整合點。 此程序所述[部署 Web 封裝](deploying-web-packages.md)。 不過，這不可能。 假設您想要部署到網際網路對向的生產環境 web 應用程式。 基於安全性理由，實際執行環境是在非常大可能位於周邊網路 （也稱為 DMZ 和遮蔽式子網路） 中的組建伺服器，不同的子網路防火牆後方。 在許多情況下，實際執行環境會在另一個網域或實際隔離的網路上。
> 
> 在這些情況下，可能是唯一的選擇移植到目的地伺服器上的 web 套件，且以手動方式將它匯入至 IIS。 雖然這種方法，即無需自動化的部署，但它仍然是 web 應用程式發行的高效率技術&#x2014;只要將單一 zip 檔案複製到您的 web 伺服器和使用精靈將引導您完成匯入程序。


本主題根據名為 Fabrikam，Inc.的虛構公司的企業部署需求的教學課程系列的一部分此教學課程使用範例方案&#x2014;[連絡人管理員解決方案](the-contact-manager-solution.md)&#x2014;來表示實際層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows 通訊的 web 應用程式Foundation (WCF) 服務與資料庫專案。

## <a name="task-overview"></a>工作概觀

您必須完成這些高層級的工作，才能匯入至 IIS 的 web 部署套件：

- 建立 web 部署套件使用 MSBuild 命令列、 Team Build 或 Visual Studio 2010。
- 將 web 套件複製到目的地 web 伺服器。
- 使用 IIS 管理員中匯入應用程式套件精靈，安裝 web 封裝，並提供值給變數，例如連接字串和服務端點。

本主題將說明如何執行這些程序。 工作與本主題中的逐步解說假設您已經熟悉 web 封裝、 Web Deploy 和 WPP 背後的概念。 如需詳細資訊，請參閱[建置和封裝 Web 應用程式專案](building-and-packaging-web-application-projects.md)。

> [!NOTE]
> 本主題最適合用於搭配[設定 Web 伺服器進行 Web 部署發行 （離線部署）](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)，其中說明如何安裝必要的元件，並準備封裝匯入的 IIS 網站。


## <a name="create-a-web-deployment-package"></a>建立 Web 部署套件

第一個工作是建立您想要部署 web 應用程式專案的 web 部署封裝。 您可以建立 web 封裝各種不同的方式。

**方法 1： 建立 Visual Studio 中的封裝，以做為建置流程的一部分**

您可以設定您的 web 應用程式專案來建立每次建置後透過 web 部署套件**封裝/發行 Web**專案屬性頁 索引標籤。 此程序所述[建置和封裝 Web 應用程式專案](building-and-packaging-web-application-projects.md)。

**方法 2： 使用 MSBuild 建立封裝，以做為建置流程的一部分**

如果您的 web 應用程式專案來建置使用 MSBuild 直接透過自訂的 MSBuild 專案檔，或從命令列中，您可以藉由包含做為建置流程的一部分建立 web 部署套件**DeployOnBuild = true**和**DeployTarget = 封裝**您在命令中的屬性。 此程序所述[瞭解建置程序](understanding-the-build-process.md)。

**方法 3： 在 Visual Studio 中視需要建立封裝**

您可以隨時在 Visual Studio 2010 建立 web 應用程式專案的 web 部署封裝。 若要這樣做，請在**方案總管 中**視窗中，以滑鼠右鍵按一下您的 web 應用程式專案，然後**建置部署封裝**。

![](manually-installing-web-packages/_static/image1.png)

**方法 4： 從命令列中，視需要建立封裝**

您可以從命令列叫用來建立 web 部署套件**封裝**上 web 應用程式專案使用 MSBuild 目標。 此命令應該與以下相似：


[!code-console[Main](manually-installing-web-packages/samples/sample1.cmd)]


較接近您使用，最終結果相同。 WPP 為 zip 檔案，以及各種支援的資源，您的 web 應用程式專案的輸出資料夾中建立 web 部署套件。

![](manually-installing-web-packages/_static/image2.png)

當您計劃手動匯入 web 封裝時，您需要 zip 檔案。 將這個檔案複製到目標 web 伺服器，您可以開始匯入程序。

## <a name="import-a-web-package-into-iis"></a>匯入至 IIS 的 Web 套件

您可以使用下一個程序，從本機檔案系統的 web 部署套件匯入至 IIS 網站。 執行此程序之前，請確認您已經：

- 複製到 web 伺服器上的 web 部署套件。
- 設定 IIS web 伺服器來裝載您的應用程式。

如需有關如何設定 IIS web 伺服器以支援 web 部署套件的詳細資訊，請參閱[設定 Web 伺服器進行 Web 部署發行 （離線部署）](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)。

**若要匯入使用 IIS 管理員的 web 部署封裝**

1. 在 [IIS 管理員] 中，在**連線**] 窗格中，您的 IIS 網站上按一下滑鼠右鍵，指向**部署**，然後按一下 [**匯入應用程式**。

    ![](manually-installing-web-packages/_static/image3.png)
2. 在匯入應用程式套件精靈，在**選取的套件**頁面上，瀏覽至您的 web 部署封裝的位置，然後按一下**下一步**。
3. 在**選取封裝的內容**頁面上，清除您不需要此項目，然後再按一下的任何內容**下一步**。

    ![](manually-installing-web-packages/_static/image4.png)

    > [!NOTE]
    > 在許多情況下，您可能不想匯入 web 部署套件隨附的所有項目。 例如，您可能不想讓 Web Deploy 來取代相關聯的資料庫。  
    > **授與權限**項目設定目的地檔案系統，以確保應用程式集區身分識別可以存取儲存網站內容的實體資料夾的權限。 此外，匿名驗證使用者授與讀取資料夾權限，讓應用程式支援多用途網際網路郵件延伸標準 (MIME) 類型的檔案。 如果您想要的話，您可以移除這些項目，並手動設定權限。
4. 在**輸入應用程式封裝資訊**頁面上，提供要求的資訊。

    ![](manually-installing-web-packages/_static/image5.png)
5. 當您建立 web 封裝時，WPP 分析您的應用程式的組態檔，並偵測到任何變數，例如連接字串和服務端點。 在此情況下：

    1. **應用程式路徑**是您要安裝應用程式的 IIS 路徑。 此設定是通用於所有 WPP 建立的部署封裝。
    2. **ContactService 服務端點位址**是應用程式應該使用與已部署的 WCF 服務進行通訊的位址。 此設定會對應中的項目*web.config*檔案。
    3. 第一個**連接字串**設定是 Web Deploy 來部署與應用程式 （在此案例的 ASP.NET 成員資格資料庫） 中相關聯的資料庫應該使用的連接字串。 此設定會對應到上設定**封裝/發行 SQL** Visual Studio 中的索引標籤。
    4. 第二個**連接字串**設定是您的應用程式實際上將用來啟動並執行時，與資料庫通訊的連接字串。 這會對應至連接字串的項目中*web.config*檔案。

        > [!NOTE]
        > 如需這些參數來自何處的詳細資訊，請參閱[Web 封裝部署的設定參數](configuring-parameters-for-web-package-deployment.md)。
6. 按 [ **下一步**]。
7. 如果這不被您部署了應用程式瀏覽此網站的第一次，將提示您指定是否要刪除所有現有的內容，再進行安裝。 選擇適合您需求的選項，然後按一下**下一步**。

    ![](manually-installing-web-packages/_static/image6.png)
8. 完成 IIS 已安裝封裝，請按一下**完成**。

    ![](manually-installing-web-packages/_static/image7.png)

此時，您已成功發行至 IIS web 應用程式。

## <a name="conclusion"></a>結論

本主題描述如何匯入至 IIS 網站使用 IIS 管理員的 web 部署套件。 當安全性或基礎結構的條件約束進行遠端部署，無法執行或不想要適合 web 應用程式發行至這個方法。

## <a name="further-reading"></a>進一步閱讀

如需如何設定 IIS 網頁伺服器支援手動匯入 web 封裝的指引，請參閱[設定 Web 伺服器進行 Web 部署發行 （離線部署）](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)。 多個一般指引部署 web 封裝的詳細資訊，請參閱[逐步解說： 部署 Web 應用程式的專案使用的 Web 部署套件 (第 1 部分為 4)](https://msdn.microsoft.com/library/dd483479.aspx)。

> [!div class="step-by-step"]
> [上一步](creating-and-running-a-deployment-command-file.md)
