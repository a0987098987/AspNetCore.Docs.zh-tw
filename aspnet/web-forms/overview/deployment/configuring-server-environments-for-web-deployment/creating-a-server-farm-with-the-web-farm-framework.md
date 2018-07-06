---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
title: 使用 Web 伺服陣列架構建立伺服器陣列 |Microsoft Docs
author: jrjlee
description: 本主題描述如何使用 Web 伺服陣列架構 (WFF) 2.0 來建立及設定 web 伺服器陣列，集合中的伺服器。
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 656dd06d-806c-467c-863d-9fc45e5ba3ab
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
msc.type: authoredcontent
ms.openlocfilehash: 5fdd1088f1f4e21f5e79b8c8379b7865f825ad53
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808936"
---
<a name="creating-a-server-farm-with-the-web-farm-framework"></a>使用 Web 伺服陣列架構建立伺服器陣列
====================
藉由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主題描述如何使用 Web 伺服陣列架構 (WFF) 2.0 來建立及設定 web 伺服器陣列，集合中的伺服器。


WFF 可讓您跨多個負載平衡的 web 伺服器同步處理 web 平台產品和元件、 web 應用程式、 網站和組態設定。 在情況下，您需要一個以上的 web 伺服器，例如預備和生產環境中，這可大幅簡化您的部署和設定程序。 您可以部署單一伺服器的 web 應用程式&#x2014;*主要伺服器*&#x2014;和 WFF 會自動複寫所有其他 web 伺服器上伺服器陣列中該 web 應用程式。

## <a name="understanding-the-web-farm-framework"></a>了解 Web 伺服陣列架構

您可以使用 WFF 2.0 佈建、 管理及部署至 web 伺服器群組的內容。 WFF 部署是由三個重要伺服器角色所組成：

- *控制站伺服器*。 您可以使用這個伺服器來建立及設定 WFF 伺服器陣列。 控制站伺服器管理 web 平台元件和設定伺服器陣列中的 web 伺服器之間的應用程式同步的處理。 您在控制站伺服器上，安裝 WFF 2.0 和控制站伺服器接著會在每個伺服器陣列中的伺服器上安裝 WFF 代理程式。 控制站伺服器不在概念上屬於任何 WFF server 伺服器陣列中，與單一控制器伺服器可以管理多個伺服器陣列。 在此案例中，您可以使用單一的 WFF 控制器伺服器來建立和管理預備伺服器陣列] 和 [生產伺服器陣列。
- *主要伺服器*。 每個 WFF 伺服器陣列包含一部主要伺服器。 當您安裝 web 平台元件或應用程式部署到主要伺服器時，WFF 會同步處理您的變更，伺服器陣列中的所有其他伺服器。
- *次要伺服器*。 每個 WFF server 伺服器陣列包含一或多個次要伺服器。 您對主要伺服器的任何變更會複寫到伺服器陣列中每一部次要伺服器。

這會顯示這些伺服器角色如何與 Fabrikam，Inc.預備與生產環境相關聯：

![](creating-a-server-farm-with-the-web-farm-framework/_static/image1.png)

在此案例中，預備環境和生產環境會同時設定為 WFF 伺服器陣列。 單一的 WFF 控制器伺服器管理兩個伺服器陣列。 在每個伺服器陣列中，主要伺服器的任何變更會複寫到每一部次要伺服器。

開始設定您的預備和生產環境之前，我們建議您先閱讀下列文章來熟悉 WFF 2.0 的重要概念：

- [適用於 IIS 7 Web Farm Framework 2.0 的概觀](https://go.microsoft.com/?linkid=9805126)
- [適用於 IIS 7 Web Farm Framework 2.0 的伺服器陣列設定](https://go.microsoft.com/?linkid=9805127)
- [系統和 Web Farm Framework 2.0 for IIS 7 的平台需求](https://go.microsoft.com/?linkid=9805128)

## <a name="task-overview"></a>工作概觀

若要完成的工作與本主題中的逐步解說，您需要至少三部伺服器&#x2014;一個 WFF 控制器、 伺服器陣列，一個主要 web 伺服器和伺服器陣列的一個或多個次要的網頁伺服器。 您可以至 WFF 伺服器陣列中新增更多的次要伺服器，在任何時間。 在高層級，來建立及設定您將需要您預備或生產環境的 WFF 伺服器陣列：

- 安裝 Internet Information Services (IIS) 7.5 和 WFF 2.0 建立的控制站伺服器。
- 建立一般的系統管理員帳戶和設定防火牆例外，以準備主要和次要伺服器。
- 控制站伺服器上使用 IIS 管理員設定伺服器陣列。
- 設定負載平衡使用 IIS 應用程式要求路由 (ARR) 或替代的負載平衡技術。

工作與本主題中的逐步解說假設您正在使用執行 Windows Server 2008 R2 的全新的伺服器組建。 開始針對每部伺服器之前，請確認：

- 安裝 Windows Server 2008 R2 Service Pack 1 和所有可用的更新。
- 伺服器已加入網域。
- 伺服器具有靜態 IP 位址。

> [!NOTE]
> 如需有關如何將電腦加入網域的詳細資訊，請參閱 <<c0> [ 將電腦加入網域並登入](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx)。 如需有關如何設定靜態 IP 位址的詳細資訊，請參閱 <<c0> [ 設定靜態 IP 位址](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx)。


## <a name="create-the-wff-controller-server"></a>建立 WFF 控制器伺服器

若要建立 WFF 控制器伺服器，您必須安裝 IIS 7 或更新版本和 WFF 2.0 或更新版本。 實際上，WFF 使用 IIS Web Deployment Tool (Web Deploy) 來同步處理您的伺服器陣列中伺服器的 2.x。 如果您使用 Web Platform Installer 來安裝 WFF 時，安裝程式會自動下載，並為您安裝 Web Deploy。

**若要建立 WFF 控制器伺服器**

1. 下載並安裝[Web Platform Installer](https://go.microsoft.com/?linkid=9739157)。
2. 在頂端**Web Platform Installer 3.0**  視窗中，按一下**產品**。
3. 在左邊視窗中，在導覽窗格中，按一下  **Server**。
4. 在  **IIS 7 建議組態**資料列中，按一下**新增**。
5. 在  <strong>Web 伺服陣列架構 2。</strong><em>x</em>資料列中，按一下<strong>新增</strong>。

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image2.png)
6. 按一下 [安裝] 。 請注意，Web Platform Installer 已加入 Web Deployment Tool，以及其他各種相依性，[安裝] 清單中。

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image3.png)
7. 檢閱授權條款，然後如果您同意這些條款，按一下**我接受**。
8. 安裝完成時，按一下**完成**，然後關閉**Web Platform Installer 3.0**視窗。

## <a name="configure-the-primary-and-secondary-servers"></a>設定主要和次要伺服器

建立 WFF 伺服器陣列之前，您應該完成一些準備工作會組成伺服器陣列的 web 伺服器上：

- 新增防火牆例外以允許**核心網路功能**，**遠端管理**，並**File and Printer Sharing** WFF 控制器伺服器通訊的功能.
- 建立網域帳戶 (例如**FABRIKAM\stagingfarm**) 在 Active Directory 並將它新增至每一部伺服器上的本機 administrators 群組。 當您建立伺服器陣列時，您將使用此帳戶為伺服器陣列系統管理員帳戶。

如需有關如何設定 Windows 防火牆中的這些防火牆例外狀況的詳細資訊，請參閱 <<c0> [ 系統和適用於 IIS 7 Web Farm Framework 2.0 的平台需求](https://go.microsoft.com/?linkid=9805128)。 對於其他防火牆的系統，請參閱產品文件。

您可以使用下一個程序，將網域帳戶新增至 Windows Server 2008 R2 中的本機 administrators 群組。 您應該在您想要新增至伺服器陣列的每部伺服器上執行此程序&#x2014;也就是說，將相同的網域帳戶新增至每部次要伺服器和主要伺服器上的本機 administrators 群組。

**若要將網域帳戶新增至本機 administrators 群組**

1. 在上**開始**功能表上，指向**系統管理工具**，然後按一下**伺服器管理員**。
2. 在**伺服器管理員**視窗中的，在樹狀檢視窗格中，展開**Configuration**，展開**本機使用者和群組**，然後按一下 **群組**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image4.png)
3. 在 **群組**窗格中，按兩下**系統管理員**。
4. 在 [**系統管理員屬性**] 對話方塊中，按一下**新增**。
5. 在 **選取使用者、 電腦、 服務帳戶或群組** 對話方塊中，輸入 （或瀏覽） 到您的網域帳戶 (比方說， **FABRIKAM\stagingfarm**)，然後按一下  **確定**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image5.png)
6. 在 [**系統管理員屬性**] 對話方塊中，按一下**確定**。

您的伺服器現在準備好要加入至伺服器陣列。 在主要伺服器的情況下，您可以設定伺服器以符合您的應用程式需求，之前或之後建立的伺服器陣列&#x2014;在這兩種情況下，WFF 會藉由部署相同的產品、 元件或組態同步伺服器次要伺服器。 為了簡單起見，本教學課程會假設，您就會設定主要伺服器，當您完成建立伺服器陣列。

## <a name="create-the-wff-server-farm"></a>建立 WFF 伺服器陣列

此時，您的所有伺服器都已準備好新增至 WFF 伺服器陣列：

- 您已安裝 WFF 控制器伺服器上。
- 您已在您的主要和次要的 web 伺服器上設定防火牆例外狀況。
- 您已經新增至本機 administrators 群組的網域帳戶在您的主要和次要的 web 伺服器上。

下一個步驟是在 WFF 建立伺服器陣列。 您可以從 IIS 管理員 WFF 控制器伺服器上。

**若要建立 WFF 伺服器陣列**

1. 在 WFF 控制器伺服器上，在**開始**功能表上，指向**系統管理工具**，然後按一下  **Internet Information Services (IIS) 管理員**。
2. 在 **連線**窗格中，展開本機伺服器節點，以滑鼠右鍵按一下**伺服器陣列**，然後按一下**建立伺服器陣列**。
3. 中**建立伺服器陣列**對話方塊方塊中，輸入有意義的名稱伺服器陣列 (例如**預備環境陣列**)，然後選取**佈建伺服器陣列**。
4. 輸入使用者名稱和您新增至每一部伺服器上的本機 administrators 群組之網域帳戶的密碼。

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image6.png)
5. 按 [ **下一步**]。
6. 在上**新增伺服器**頁面上，輸入完整的網域名稱 (FQDN) 的主要伺服器，選取**主要伺服器**，然後按一下**新增**。
7. 此時，WFF 會嘗試連絡主要伺服器使用您所提供的認證。 如果連線成功，主要伺服器會新增至資料表上**新增伺服器**頁面。

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image7.png)

    > [!NOTE]
    > 您可能也注意**進行負載平衡的伺服器可**預設會選取。 WFF 實作負載平衡，並藉此將要求分散到您的伺服器陣列中的 web 伺服器使用 IIS ARR 模組。 在大部分情況下，您會只清除**伺服器可以使用負載平衡**選項，如果您想要使用協力廠商負載平衡解決方案改用。
8. 在 **新增伺服器**頁面上，輸入您的第一部次要伺服器的 FQDN，然後按一下**新增**。

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image8.png)
9. 重複步驟 7 的任何其他的次要伺服器，在您的伺服器陣列，然後再按一下**完成**。

您的 WFF 伺服器陣列現在會啟動並執行。 任何 web 平台產品或您在主要伺服器，任何 web 應用程式或內容，您將部署到主要伺服器安裝的元件將會自動佈建您所有的次要伺服器上。

WFF 是廣泛且複雜的主題，以及您可以深入了解上[Microsoft Web Farm Framework 2.0 適用於 IIS 7](https://go.microsoft.com/?linkid=9805129)網站。 目前，不過，有兩個功能區域，您需要留意的：

- *應用程式佈建*是從主要伺服器，例如 web 應用程式和組態設定，內容複寫到伺服器陣列中的所有次要伺服器的程序。 例如，如果您將 Contact Manager 範例方案部署到主要預備伺服器時，WFF 應用程式佈建程序將這個方案部署至所有次要預備伺服器。 根據預設，應用程式的佈建程序會執行每隔 30 秒。
- *平台佈建*是同步處理 web 平台產品，以及從主要伺服器到伺服器陣列中的所有次要伺服器的元件的程序。 比方說，如果您主要的臨時伺服器上安裝 ASP.NET MVC 3，平台佈建程序會使用 Web Platform Installer 來安裝所有次要臨時伺服器上的 ASP.NET MVC 3。 根據預設，平台佈建程序會執行每隔五分鐘。

您可以管理基本的應用程式與平台佈建的設定從 IIS Manager WFF 控制器伺服器上。

**探索應用程式和佈建設定的平台**

1. 在 IIS 管理員 中，在**連線** 窗格中，選取您的伺服器陣列。

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image9.png)
2. 在 **伺服器陣列**窗格中，按兩下**應用程式佈建**。

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image10.png)
3. 如您所見，伺服器陣列目前會設定為同步處理之間的主要伺服器和次要伺服器的 web 內容和組態設定每隔 30 秒。
4. 按一下 **回復**，然後按兩下**平台佈建**。

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image11.png)
5. 如您所見，伺服器陣列目前已同步處理 web 平台產品和元件之間的主要伺服器和次要伺服器每隔五分鐘。
6. 按一下 **回**。
7. 若要強制立即同步處理 web 平台產品的伺服器陣列中**動作**窗格中，按一下**佈建的平台**。

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image12.png)

    > [!NOTE]
    > 平台佈建，可能需要一些時間。 在您的伺服器陣列的次要伺服器上，在背景中執行安裝程式程序。
8. 一旦您已允許有足夠的時間才能完成佈建程序，您可以確認產品和您加入至主要伺服器的元件已現在已複寫的次要伺服器上。 例如，您可以在 登入至次要伺服器，並使用**伺服器管理員**視窗，來確認已安裝網頁伺服器角色。

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image13.png)
9. 您也可以檢查以確認已新增各種 web 平台元件的安裝的程式清單。

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image14.png)

## <a name="configure-load-balancing"></a>設定負載平衡

當您建立的 web 伺服陣列時，您需要設定某種形式的負載平衡來散發您的 web 伺服器之間的 HTTP 要求。 這可能是 Windows Server 2008 網路負載平衡 IIS ARR 或協力廠商軟體或硬體型負載平衡解決方案。

WFF 被設計來與 IIS ARR 密切整合 若要利用這項整合，您需要安裝 ARR 模組 WFF 控制器伺服器上。 您隨後導向到控制站伺服器，您所有的 web 流量通常是藉由設定網域名稱系統 (DNS) 記錄。 控制站伺服器接著會發佈在的伺服器，您的伺服陣列中，根據伺服器可用性及各種其他準則之間的連入要求。

> [!NOTE]
> 您不必使用 WFF; 中的 ARR您可以設定 WFF 使用協力廠商負載平衡解決方案。 如需詳細資訊，請參閱 <<c0> [ 適用於 IIS 7 Web Farm Framework 2.0 概觀](https://go.microsoft.com/?linkid=9805126)。


使用 ARR 負載平衡是複雜的主題中，最其中已超出本教學課程的範圍。 不過，您可以使用下一個程序來安裝 ARR 模組並開始使用負載平衡。

**若要設定負載平衡 WFF 控制器伺服器上**

1. 在 WFF 控制器伺服器上，啟動 Web Platform Installer。
2. 在頂端**Web Platform Installer 3.0**  視窗中，按一下**產品**。
3. 在左邊視窗中，在導覽窗格中，按一下  **Server**。
4. 在 **應用程式要求路由 2.5**資料列中，按一下**新增**。

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image15.png)
5. 按一下 **安裝**，然後依照中的指示**Web Platform 安裝**視窗。
6. 安裝完成時，啟動 IIS 管理員 中，然後在**連線** 窗格中，按一下您的伺服器陣列節點。 請注意，已新增數個新的圖示**伺服器陣列**窗格。

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image16.png)
7. 在 **伺服器陣列**窗格中，按兩下**負載平衡**。
8. 在 [**負載平衡**] 窗格中選取負載平衡演算法 (例如**至少目前要求**)。

    > [!NOTE]
    > 如需有關負載平衡演算法和其他組態設定的詳細資訊，請參閱 <<c0> [ 應用程式要求路由模組](https://go.microsoft.com/?linkid=9805130)。

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image17.png)
9. 在 **動作**窗格中，按一下**套用**。

您現在已設定基本的負載平衡您的伺服器陣列中的伺服器。 如果引導您所有 web 伺服陣列的流量控制站伺服器時，會根據可用性伺服器陣列中的部署伺服器與您所選擇的負載平衡演算法之間分散要求。

如需有關如何設定使用 ARR 負載平衡的詳細資訊，請參閱 <<c0> [ 應用程式要求路由模組](https://go.microsoft.com/?linkid=9805130)。

## <a name="monitor-the-server-farm"></a>監視的伺服器陣列

您可以隨時透過 IIS 管理員的控制站伺服器上監視您的伺服器陣列的健全狀況。 在 **連線**窗格中，展開您的伺服器陣列，然後按一下**伺服器**。 中央窗格會顯示最近活動的追蹤記錄檔以及伺服器陣列中的每一部伺服器的摘要。

![](creating-a-server-farm-with-the-web-farm-framework/_static/image18.png)

## <a name="conclusion"></a>結論

您的 WFF 伺服器陣列現在應該啟動並執行。 您可以設定主要伺服器，以支援您偏好的任何部署方法&#x2014;[深入閱讀] 區段，如需詳細資訊請參閱 <<c1>和您的組態將會複寫到伺服器陣列中每個次要伺服器上。</c1>

## <a name="further-reading"></a>進一步閱讀

如需詳細指引設定和使用 WFF 的所有層面的詳細資訊，請參閱[Microsoft Web Farm Framework 2.0 適用於 IIS 7](https://go.microsoft.com/?linkid=9805129)網站。

> [!div class="step-by-step"]
> [上一頁](configuring-a-database-server-for-web-deploy-publishing.md)
> [下一頁](configuring-deployment-properties-for-a-target-environment.md)
