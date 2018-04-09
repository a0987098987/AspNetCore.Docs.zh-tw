---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment
title: 設定 Web 伺服器的 Web Deploy 發行 （離線部署） |Microsoft 文件
author: jrjlee
description: 本主題描述如何設定 IIS web 伺服器支援離線網頁發佈和部署。 當您使用 Internet Information Services (我...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: ba92788f-9f03-44b1-b6b2-af8413e6a35d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment
msc.type: authoredcontent
ms.openlocfilehash: e28bdea26847d4e660d6ee59b15eb38f749d2314
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="configuring-a-web-server-for-web-deploy-publishing-offline-deployment"></a>設定 Web 伺服器的 Web Deploy 發行 （離線部署）
====================
由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主題描述如何設定 IIS web 伺服器支援離線網頁發佈和部署。
> 
> 當您使用網際網路資訊服務 (IIS) Web Deployment Tool (Web Deploy) 2.0 或更新版本時，有三個主要的方法，可用來取得您的應用程式或 web 伺服器上的站台。 您可以：
> 
> - 使用*Web Deploy 遠端代理程式服務*。 這個方法需要較少的 web 伺服器設定，但您必須提供本機伺服器系統管理員的認證，以將任何項目部署至伺服器。
> - 使用*Web Deploy 處理常式*。 此方法很複雜，而且需要較多的初始工作，以便設定 web 伺服器。 不過，當您使用這個方法時，您可以設定 IIS 以允許非系統管理員使用者能夠執行部署。 只有 IIS 7 或更新版本中提供 Web 部署處理常式。
> - 使用*離線部署*。 這個方法會要求最低的網頁伺服器的設定，但伺服器系統管理員必須手動複製到伺服器的 web 套件並匯入它透過 IIS 管理員。
> 
> 如需有關主要功能、 優點，與這些方法的缺點的詳細資訊，請參閱[選擇 Web 部署的權限方法](choosing-the-right-approach-to-web-deployment.md)。


是，如果您的網路基礎結構或安全性限制導致無法遠端部署。 這是最有可能會發生這個情況，在網際網路對向實際執行環境中，網頁伺服器的隔離&#x2014;是實體或透過防火牆和子網路&#x2014;從您的伺服器基礎結構的其餘部分。

很明顯地，這個方法會變得較不建議，如果您的 web 應用程式會定期更新。 如果您的基礎結構可讓它，您可能要考慮啟用遠端部署，使用 Web 部署處理常式或 Web 部署遠端代理程式服務。

## <a name="task-overview"></a>工作概觀

若要設定 web 伺服器，以支援離線匯入和部署 web 封裝，您將需要：

- 安裝 IIS 7.5 和 IIS 7 建議的組態。
- 安裝 Web Deploy 2.1 或更新版本。
- 建立已部署的內容裝載在 IIS 網站。
- 停用 Web 部署代理程式服務。

若要特別裝載範例方案，您還需要：

- 安裝.NET Framework 4.0。
- 安裝 ASP.NET MVC 3。

本主題將說明如何執行這種程序。 工作與本主題中的逐步解說假設您要開始使用執行 Windows Server 2008 R2 的全新的伺服器組建。 在繼續之前，請確認：

- 已安裝 Windows Server 2008 R2 Service Pack 1 和所有可用的更新。
- 伺服器已加入網域。
- 伺服器具有靜態 IP 位址。

> [!NOTE]
> 如需有關如何將電腦加入網域的詳細資訊，請參閱[將電腦加入網域並登入](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx)。 如需有關如何設定靜態 IP 位址的詳細資訊，請參閱[設定靜態 IP 位址](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx)。


## <a name="install-products-and-components"></a>安裝的產品和元件

本節將引導您在 web 伺服器上安裝必要的產品和元件。 在開始之前，最好是執行 Windows Update，以確保您的伺服器是完全處於最新狀態。

在此情況下，您需要安裝下列項目：

- **IIS 7 建議的組態**。 這可讓**網頁伺服器 (IIS)** web 伺服器上的角色，並安裝 IIS 模組和元件，您需要以裝載 ASP.NET 應用程式集。
- **.NET framework 4.0**。 如此才能執行此版本的.NET Framework 所建置的應用程式。
- **Web Deployment Tool 2.1 或更新版本**。 這會在您的伺服器上安裝 Web Deploy （和其基礎可執行檔，MSDeploy.exe）。 Web Deploy 與 IIS 整合，並讓您匯入和匯出 web 封裝。
- **ASP.NET MVC 3**。 這會安裝您要執行 MVC 3 應用程式的組件。

> [!NOTE]
> 本逐步解說說明如何使用 Web Platform Installer 安裝及設定各種元件。 雖然您不一定要使用 Web Platform Installer，它可簡化安裝程序自動偵測相依性，並確保您一律取得最新的產品版本。 如需詳細資訊，請參閱[Microsoft Web Platform Installer 3.0](https://go.microsoft.com/?linkid=9805118)。


**若要安裝必要的產品和元件**

1. 下載並安裝[Web Platform Installer](https://go.microsoft.com/?linkid=9805118)。
2. 當安裝完成時，就會自動啟動 Web Platform Installer。

    > [!NOTE]
    > 您現在可以啟動 Web Platform Installer 的任何時候**啟動**功能表。 若要這樣做，請在**啟動**功能表上，按一下 **所有程式**，然後按一下  **Microsoft Web Platform Installer**。
3. 在頂端**Web Platform Installer 3.0**視窗中，按一下 **產品**。
4. 在左邊視窗中，瀏覽窗格中按一下 **架構**。
5. 在**Microsoft.NET Framework 4**資料列，如果尚未安裝.NET Framework，按一下 **新增**。

    > [!NOTE]
    > 您可能已安裝.NET Framework 4.0，透過 Windows Update。 如果已安裝的產品或元件，Web Platform Installer 將會指示這取代**新增**按鈕的文字**已安裝**。

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image1.png)
6. 在**ASP.NET MVC 3 (Visual Studio 2010)**資料列中，按一下 **新增**。
7. 在導覽窗格中，按一下**伺服器**。
8. 在**IIS 7 建議組態**資料列中，按一下 **新增**。
9. 在**Web 部署工具 2.1**資料列中，按一下 **新增**。
10. 按一下 [安裝] 。 Web Platform Installer 將會顯示您的產品清單&#x2014;以及任何相關聯相依性&#x2014;安裝就會提示您接受授權條款。

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image2.png)
11. 檢閱授權條款，然後如果您同意條款，按一下**我接受**。
12. 當安裝完成時，按一下 **完成**，然後關閉**Web Platform Installer 3.0**視窗。

如果您安裝 IIS 之前，您會安裝.NET Framework 4.0，您需要執行[ASP.NET IIS 註冊工具](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx)(aspnet\_regiis.exe) 向 IIS 註冊 ASP.NET 的最新版本。 如果沒有這樣做，您會發現，IIS 會提供靜態內容 （例如 HTML 檔） 不會有任何問題，但它會傳回**HTTP 錯誤 404.0 – 找不到**當您嘗試瀏覽至 ASP.NET 內容。 若要確定已註冊 ASP.NET 4.0，您可以使用下一個程序。

**若要向 IIS 註冊 ASP.NET 4.0**

1. 按一下**啟動**，然後輸入**命令提示字元**。
2. 在搜尋結果中，以滑鼠右鍵按一下**命令提示字元**，然後按一下 **系統管理員身分執行**。
3. 在 [命令提示字元] 視窗中，瀏覽至**%WINDIR%\Microsoft.NET\Framework\v4.0.30319**目錄。
4. 輸入下列命令，並按 Enter:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/samples/sample1.cmd)]
5. 如果您打算裝載在任何時間點的 64 位元 web 應用程式時，則您也應該向 IIS 註冊 64 位元版本的 ASP.NET。 若要這樣做，請在命令提示字元 視窗中，導覽至**%WINDIR%\Microsoft.NET\Framework64\v4.0.30319**目錄。
6. 輸入下列命令，並按 Enter:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/samples/sample2.cmd)]

很好的作法是使用 Windows Update 再次此時若要下載並安裝新的產品和元件已安裝任何可用的更新。

## <a name="configure-the-iis-website"></a>設定 IIS 網站

您可以將 web 內容部署至您的伺服器之前，您需要建立和設定內容裝載在 IIS 網站。 Web Deploy 只可以將 web 套件部署到現有的 IIS 網站。它不能為您建立網站。 在高層級，您必須完成這些工作：

- 若要裝載您的內容在檔案系統上建立資料夾。
- 建立的 IIS 網站來提供內容，並且讓它與本機資料夾。
- 授與讀取權限的本機資料夾的應用程式集區身分識別。

雖然沒有停止將內容部署到預設的網站在 IIS 中的項目，這個方法不會建議測試或示範案例以外的任何項目。 若要模擬實際執行環境，您應該建立新的 IIS 網站應用程式的需求所特定的設定。

**建立和設定 IIS 網站**

1. 在本機檔案系統上，建立資料夾來儲存您的內容 (例如， **C:\DemoSite**)。
2. 在**啟動**功能表上，指向**系統管理工具**，然後按一下 **網際網路資訊服務 (IIS) 管理員**。
3. 在 IIS 管理員 中，在**連線** 窗格中，展開伺服器節點 (例如， **PROWEB1**)。

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image3.png)
4. 以滑鼠右鍵按一下**網站**節點，然後再按一下**新增網站**。
5. 在**站台名稱**方塊中，輸入 IIS 網站的名稱 (例如， **DemoSite**)。
6. 在**實體路徑**方塊中，輸入 （或瀏覽至） 您的本機資料夾的路徑 (例如， **C:\DemoSite**)。
7. 在**連接埠**方塊中，輸入您要裝載網站的連接埠號碼 (例如， **85**)。

    > [!NOTE]
    > 標準連接埠號碼為 80 的 HTTP 和 HTTPS 為 443。 不過，如果您主控此網站連接埠 80 上的，您必須停止預設網站，才能存取您的網站。
8. 保留**主機名稱**方塊保留空白，除非您想要設定網站的網域名稱系統 (DNS) 記錄，然後按一下**確定**。

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image4.png)

    > [!NOTE]
    > 在實際執行環境中，您可能會想將網站連接埠 80 上的主控，並設定主機標頭，以及比對 DNS 記錄。 如需有關如何在 IIS 7 中設定主機標頭的詳細資訊，請參閱[設定網站 (IIS 7) 的主機標頭](https://technet.microsoft.com/library/cc753195(WS.10).aspx)。 如需有關 Windows Server 2008 R2 中 DNS 伺服器角色的詳細資訊，請參閱[DNS 伺服器概觀](https://technet.microsoft.com/en-gb/library/cc770392.aspx)和[DNS 伺服器](https://technet.microsoft.com/windowsserver/dd448607)。
9. 在 [ **動作** ] 窗格的 [ **編輯站台**] 下方，按一下 [ **繫結**]。
10. 在**站台繫結**對話方塊中，按一下 **新增**。

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image5.png)
11. 在**新增網站繫結** 對話方塊中，將**IP 位址**和**連接埠**以符合您現有的站台設定。
12. 在**主機名稱**方塊中，輸入您的網頁伺服器的名稱 (例如， **PROWEB1**)，然後按一下  **確定**。

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image6.png)

    > [!NOTE]
    > 第一個站台繫結可讓您存取在本機使用的 IP 位址和連接埠的站台或`http://localhost:85`。 第二個網站繫結可讓您存取網站，從其他電腦上使用電腦名稱的網域 (例如， http://proweb1:85)。
13. 在**站台繫結**對話方塊中，按一下 **關閉**。
14. 在**連線**] 窗格中，按一下 [**應用程式集區**。
15. 在**應用程式集區**] 窗格中，以滑鼠右鍵按一下您的應用程式集區的名稱，然後按一下 [**基本設定**。 根據預設，應用程式集區的名稱會符合網站的名稱 (例如， **DemoSite**)。
16. 在**.NET Framework 版本**清單中，選取**.NET Framework v4.0.30319**，然後按一下 **確定**。

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image7.png)

    > [!NOTE]
    > 範例解決方案需要.NET Framework 4.0。 這不是需要 Web Deploy 一般。

為了讓您的網站提供內容，應用程式集區識別必須擁有讀取權限將內容儲存的本機資料夾。 在 IIS 7.5 中應用程式集區具有唯一的應用程式集區身分識別執行預設 （相較於舊版的 IIS，其中應用程式集區通常會使用 Network Service 帳戶）。 應用程式集區識別不是真正的使用者帳戶，並不會顯示任何所列之使用者或群組&#x2014;相反地，它會動態建立啟動應用程式集區時。 每個應用程式集區識別加入至本機**IIS\_IUSRS**安全性群組做為隱藏的項目。

若要授與應用程式集區識別的檔案或資料夾上的權限，您有兩個選項：

- 指派的權限的應用程式集區識別直接使用格式<strong>IIS AppPool\<n g ><em>[應用程式集區名稱]</em>(例如， <strong>IIS AppPool\DemoSite</strong>).
- 指派的權限**IIS\_IUSRS**群組。

最常見的作法是將權限指派給本機**IIS\_IUSRS**群組，因為這種方法可讓您變更應用程式集區不需要重新設定檔案系統權限。 下一個程序會使用此群組為基礎的方法。

> [!NOTE]
> 如需在 IIS 7.5 中的應用程式集區識別的詳細資訊，請參閱[應用程式集區識別](https://go.microsoft.com/?linkid=9805123)。


**若要設定 IIS 網站的資料夾權限**

1. 在 Windows 檔案總管，瀏覽至您的本機資料夾的位置。
2. 以滑鼠右鍵按一下資料夾，然後按一下**屬性**。
3. 在**安全性**索引標籤上，按一下 **編輯**，然後按一下 **新增**。
4. 按一下**位置**。 在**位置**對話方塊中，選取 本機伺服器，然後按一下**確定**。

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image8.png)
5. 在**選取使用者或群組**] 對話方塊中，輸入**IIS\_IUSRS**，按一下**檢查名稱**，然後按一下 [**確定**。
6. 在<strong>權限</strong><em>[資料夾名稱]</em>對話方塊中，請注意，新的群組已經被委派<strong>讀取&amp;執行</strong>，<strong>清單資料夾內容</strong>，和<strong>讀取</strong>預設權限。 保留不變，然後按一下<strong>確定</strong>。
7. 按一下<strong>確定</strong>關閉<em>[資料夾名稱]</em><strong>屬性</strong> 對話方塊。

## <a name="disable-the-remote-agent-service"></a>停用遠端代理程式服務

當您安裝 Web Deploy 時，Web Deployment Agent Service 安裝和自動啟動。 此服務可讓您部署及發行 web 封裝，從遠端位置。 您將不會使用遠端部署功能在此案例中，因此請停止並停用服務。

> [!NOTE]
> 您不需要停止遠端代理程式服務，才能匯入，並以手動方式部署 web 封裝。 不過，它是最好的作法是停止並停用服務，如果您不打算使用它。


您可以停止和停用服務，以多種方式使用各種命令列公用程式或 Windows PowerShell cmdlet。 此程序描述的簡單 UI 為基礎的方法。

**若要停止並停用遠端代理程式服務**

1. 在 [開始]  功能表上，指向 [系統管理工具] ，然後按一下 [服務] 。
2. 在 [服務] 主控台中，找出**Web Deployment Agent Service**資料列。

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image9.png)
3. 以滑鼠右鍵按一下**Web Deployment Agent Service**，然後按一下 **屬性**。
4. 在**Web 部署代理程式服務屬性**對話方塊中，按一下 **停止**。
5. 在**啟動類型**清單中，選取**已停用**，然後按一下 **確定**。

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image10.png)

## <a name="conclusion"></a>結論

此時，您的網頁伺服器可供離線 web 套件部署。 您嘗試匯入至 IIS 網站的 web 封裝之前，您可能想要檢查這些關鍵點：

- 您已向 IIS 註冊 ASP.NET 4.0？
- 應用程式集區身分識別是否有讀取權限為您的網站的 [來源] 資料夾？
- 已停止 Web Deployment Agent Service？

> [!div class="step-by-step"]
> [上一頁](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
> [下一頁](configuring-a-database-server-for-web-deploy-publishing.md)
