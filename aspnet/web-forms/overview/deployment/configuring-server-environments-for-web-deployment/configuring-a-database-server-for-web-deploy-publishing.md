---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
title: 設定資料庫伺服器的 Web Deploy 發行 |Microsoft Docs
author: jrjlee
description: 本主題描述如何設定 SQL Server 2008 R2 資料庫伺服器以支援 web 部署和發行。 本主題中所述的工作會共同...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: e7c447f9-eddf-4bbe-9f18-3326d965d093
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
msc.type: authoredcontent
ms.openlocfilehash: 9ec938298ea63609c276aed807a67722bf9bd66d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841975"
---
<a name="configuring-a-database-server-for-web-deploy-publishing"></a>設定 Web Deploy 發行的資料庫伺服器
====================
藉由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主題描述如何設定 SQL Server 2008 R2 資料庫伺服器以支援 web 部署和發行。
> 
> 本主題中所述的工作通用於所有部署案例&#x2014;您的 web 伺服器設定為使用 IIS Web Deployment Tool (Web Deploy) 的遠端代理程式服務、 Web 部署的處理常式或離線部署並不重要或您單一 web 伺服器或伺服器陣列上執行應用程式。 部署資料庫的方式會根據安全性需求和其他考量可能變更。 比方說，您可能會部署資料庫和範例資料，以及您可能會部署角色的使用者對應，或在部署之後以手動方式設定。 不過，您將資料庫伺服器設定的方式保持不變。


您不必安裝任何其他產品或工具來設定資料庫伺服器以支援 web 部署。 假設您的資料庫伺服器和網頁伺服器會在不同機器上執行，您只需要：

- 允許 SQL Server 使用 TCP/IP 進行通訊。
- 允許透過任何防火牆的 SQL Server 流量。
- 提供 web 伺服器電腦帳戶的 SQL Server 登入。
- 對應至任何必要的資料庫角色的機器帳戶登入。
- 授與將會執行部署的 SQL Server 登入和資料庫建立者權限的帳戶。
- 若要支援重複的部署，將對應的部署的帳戶登入**db\_擁有者**資料庫角色。

本主題將說明如何執行上述各程序。 工作與本主題中的逐步解說假設您正在使用 Windows Server 2008 R2 上執行的 SQL Server 2008 R2 的預設執行個體。 在繼續之前，請確認：

- 安裝 Windows Server 2008 R2 Service Pack 1 和所有可用的更新。
- 伺服器已加入網域。
- 伺服器具有靜態 IP 位址。
- 安裝 SQL Server 2008 R2 Service Pack 1 和所有可用的更新。

SQL Server 執行個體必須只包含**Database Engine Services**角色，會自動包含在任何 SQL Server 安裝。 不過，為了方便組態和維護，我們建議您包含**管理工具-基本**並**管理工具-完整**伺服器角色。

> [!NOTE]
> 如需有關如何將電腦加入網域的詳細資訊，請參閱 <<c0> [ 將電腦加入網域並登入](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx)。 如需有關如何設定靜態 IP 位址的詳細資訊，請參閱 <<c0> [ 設定靜態 IP 位址](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx)。 如需有關安裝 SQL Server 的詳細資訊，請參閱 <<c0> [ 安裝 SQL Server 2008 R2](https://technet.microsoft.com/library/bb500395.aspx)。


## <a name="enable-remote-access-to-sql-server"></a>啟用 SQL server 的遠端存取

SQL Server 會使用 TCP/IP 的遠端電腦進行通訊。 如果您的資料庫伺服器和網頁伺服器位於不同的電腦上，您需要：

- 設定 SQL Server 的網路設定，以允許透過 TCP/IP 進行通訊。
- 設定 SQL Server 執行個體所使用的連接埠上允許 TCP 流量 （並在某些情況下使用者資料包通訊協定 (UDP) 流量） 的任何硬體或軟體防火牆。

若要啟用 SQL Server，透過 TCP/IP 進行通訊，請使用 SQL Server 組態管理員來變更您的 SQL Server 執行個體的網路組態。

**若要啟用 SQL Server 進行通訊使用 TCP/IP**

1. 上**開始**功能表上，指向**所有程式**，按一下  **Microsoft SQL Server 2008 R2**，按一下 **組態工具**，然後按一下**SQL Server 組態管理員**。
2. 在樹狀檢視窗格中，依序展開**SQL Server 網路組態**，然後按一下**MSSQLSERVER 的通訊協定**。

   > [!NOTE]
   > 如果您已安裝的 SQL Server 的多個執行個體，您會看到<strong>通訊協定</strong><em>[執行個體名稱]</em>每個執行個體的項目。 您需要執行個體的執行個體為基礎的網路設定。
3. 在 [詳細資料] 窗格中，以滑鼠右鍵按一下**TCP/IP**資料列，然後再按一下**啟用**。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image1.png)
4. 在 [**警告**] 對話方塊中，按一下**確定**。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image2.png)
5. 您需要重新啟動 MSSQLSERVER 服務，您的新網路設定才會生效。 您可以執行，在命令提示字元中，從 [服務] 主控台中，或從 SQL Server Management Studio。 在此程序中，您將使用 SQL Server Management Studio。
6. 關閉 SQL Server 組態管理員。
7. 在上**開始**功能表上，指向**所有程式**，按一下  **Microsoft SQL Server 2008 R2**，然後按一下**SQL Server Management Studio**。
8. 在 **連接到伺服器**對話方塊的 **伺服器名稱**方塊，輸入，在資料庫伺服器的名稱，然後按一下**Connect**。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image3.png)
9. 中**物件總管 中**窗格中，以滑鼠右鍵按一下父伺服器節點 (比方說， **TESTDB1**)，然後按一下**重新啟動**。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image4.png)
10. 在 [ **Microsoft SQL Server Management Studio** ] 對話方塊中，按一下**是**。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image5.png)
11. 當重新啟動服務後時，關閉 SQL Server Management Studio。

若要讓 SQL Server 流量通過防火牆，您首先要知道您的 SQL Server 執行個體正在使用哪些連接埠。 這將取決於如何建立及設定 SQL Server 執行個體已：

- A*預設執行個體*的 SQL Server 會接聽 （和回應） TCP 通訊埠 1433年上的要求。
- A*具名執行個體*的 SQL Server 會接聽 （和回應） 的動態指派的 TCP 連接埠上的要求。
- 如果已啟用 SQL Server Browser 服務，用戶端可以查詢 UDP 通訊埠 1434，若要找出要使用特定的 SQL Server 執行個體的 TCP 連接埠上的服務。 不過，這項服務是通常停用安全性的理由。

假設您使用 SQL Server 的預設執行個體，您需要設定防火牆以允許流量。

| Direction | 從連接埠 | 連接埠 | 連接埠類型 |
| --- | --- | --- | --- |
| 輸入 | 任何 | 1433 | TCP |
| 輸出 | 1433 | 任何 | TCP |
  

> [!NOTE]
> 技術上來說，用戶端電腦會使用介於 1024年到 5000 之間的隨機指派的 TCP 通訊埠來與 SQL Server 通訊，而且您可以據此限制您的防火牆規則。 如需有關 SQL Server 連接埠和防火牆的詳細資訊，請參閱 <<c0> [ 才能透過防火牆對 SQL 進行通訊的 TCP/IP 通訊埠編號](https://go.microsoft.com/?linkid=9805125)和[How to： 設定伺服器接聽特定 TCP 通訊埠 （SQL Server 組態管理員）](https://msdn.microsoft.com/library/ms177440.aspx)。


在大部分的 Windows Server 環境中，您可能必須在資料庫伺服器上設定 Windows 防火牆。 根據預設，Windows 防火牆會允許所有輸出流量，除非規則特別禁止。 若要啟用 web 伺服器來連線到您的資料庫，您需要設定 SQL Server 執行個體所使用的通訊埠編號允許 TCP 流量的輸入的規則。 如果您使用 SQL Server 的預設執行個體，您可以使用下一個程序來設定此規則。

**若要設定 Windows 防火牆以允許使用預設 SQL Server 執行個體的通訊**

1. 在資料庫伺服器上，在**開始**功能表上，指向**系統管理工具**，然後按一下**具有進階安全性的 Windows 防火牆**。
2. 在 樹狀檢視窗格中，按一下**輸入規則**。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image6.png)
3. 在 **動作**窗格下方**輸入規則**，按一下**新規則**。
4. 在 新增輸入規則精靈，在**規則類型**頁面上，選取**連接埠**，然後按一下**下一步**。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image7.png)
5. 上**通訊協定和連接埠**頁面上，確認**TCP**已選取，然後在**特定本機連接埠**方塊中，輸入**1433年**，然後按一下**下一步**。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image8.png)
6. 在上**動作**頁面上，保留**允許該連線**選取並按下**下一步**。
7. 上**設定檔**頁面上，保留**網域**選取，清除**私人**並**公用**核取方塊，然後**下一步**。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image9.png)
8. 上**名稱**頁面上，為規則提供適當的描述性名稱 (例如**SQL Server 預設執行個體 – 網路存取**)，然後按一下**完成**。

如需有關設定適用於 SQL Server 的 Windows 防火牆，特別是如果您需要透過標準或動態連接埠與 SQL Server 通訊，請參閱[如何： 設定用於 Database Engine 存取的 Windows 防火牆](https://technet.microsoft.com/library/ms175043.aspx)。

## <a name="configure-logins-and-database-permissions"></a>設定登入和資料庫權限

當您部署 web 應用程式到網際網路資訊服務 (IIS) 時，應用程式會使用執行的應用程式集區身分識別。 在網域環境中，應用程式集區身分識別會使用執行來存取網路資源所在的伺服器電腦帳戶。 電腦帳戶的形式<em>[網域名稱]</em><strong>\</ s t ><em>[電腦名稱]</em><strong>$</strong>&#x2014;，例如<strong>FABRIKAM\TESTWEB1$</strong>。 若要允許您的 web 應用程式透過網路存取的資料庫，您需要：

- 加入 SQL Server 執行個體中的 web 伺服器電腦帳戶的登入。
- 將電腦帳戶登入對應至任何必要的資料庫角色 (通常**db\_datareader**並**db\_資料寫入元**)。

如果伺服器陣列中，而不是在單一伺服器上執行您的 web 應用程式，您必須重複這些程序的伺服器陣列中每部 web 伺服器。

> [!NOTE]
> 如需有關應用程式集區身分識別和存取網路資源的詳細資訊，請參閱 <<c0> [ 應用程式集區識別](https://go.microsoft.com/?linkid=9805123)。


您可以達到各種方式處理這些工作。 若要建立登入，您可以：

- 使用 TRANSACT-SQL 或 SQL Server Management Studio，在資料庫伺服器上手動建立登入。
- 使用 Visual Studio 中的 SQL Server 2008 伺服器專案，來建立及部署的登入。

SQL Server 登入是伺服器層級物件，而不是資料庫層級物件，因此不是取決於您想要部署的資料庫。 因此，您可以在任何時間點，來建立登入，而最簡單的方法通常是登入以手動方式在伺服器上建立資料庫再開始部署資料庫。 SQL Server Management Studio 中建立登入，您可以使用下一個程序。

**若要建立 SQL Server 登入 web 伺服器電腦帳戶**

1. 在資料庫伺服器上，在**開始**功能表上，指向**所有程式**，按一下  **Microsoft SQL Server 2008 R2**，然後按一下  **SQL Server Management Studio**.
2. 在 **連接到伺服器**對話方塊的 **伺服器名稱**方塊，輸入，在資料庫伺服器的名稱，然後按一下**Connect**。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image10.png)
3. 中**物件總管** 窗格中，以滑鼠右鍵按一下**安全性**，指向**新增**，然後按一下**登入**。
4. 在 **登入-新增**對話方塊的 **登入名稱**方塊中，輸入您的 web 伺服器電腦帳戶的名稱 (例如**FABRIKAM\TESTWEB1$**)。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image11.png)
5. 按一下 [確定 **Deploying Office Solutions**]。

此時，您的資料庫伺服器可供 Web Deploy 發行。 不過，任何您所部署的解決方案也將無法運作，直到您將機器帳戶登入對應至所需的資料庫角色。 登入對應到資料庫角色需要更以為很多，您無法對應到中的角色之後已部署資料庫。 若要對應至必要的資料庫角色的機器帳戶登入，您可以：

- 資料庫角色後指派給登入以手動的方式，您已部署的資料庫第一次。
- 若要將資料庫角色指派給登入使用部署後指令碼。

如需有關如何自動建立登入和資料庫角色對應的詳細資訊，請參閱[部署資料庫角色成員資格測試環境](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md)。 或者，您可以使用下一個程序來手動對應到必要的資料庫角色的電腦帳戶登入。 請記住，您無法執行此程序，直到*之後*您已部署的資料庫。

**將資料庫角色對應至 web 伺服器電腦帳戶登入**

1. 和以前一樣開啟 SQL Server Management Studio。
2. 中**物件總管** 窗格中，展開**安全性**節點，展開**登入** 節點，然後按兩下 電腦帳戶登入 (比方說， **FABRIKAM\TESTWEB1$**)。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image12.png)
3. 在 [**登入屬性**] 對話方塊中，按一下**使用者對應**。
4. 在 **對應到此登入的使用者**資料表中，選取您的資料庫名稱 (例如**ContactManager**)。
5. 在 **資料庫角色成員資格對象：** *[資料庫名稱]* 清單中，選取所需的權限。 在連絡人管理員範例解決方案中，您必須選取**db\_datareader**並**db\_資料寫入元**角色。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image13.png)
6. 按一下 [確定 **Deploying Office Solutions**]。

雖然手動對應資料庫角色通常是多個適合測試環境，卻是較不適合自動 」 或 「 單鍵部署至預備或生產環境。 您可以找到更多有關自動化工作使用中的部署後指令碼的這類[部署資料庫角色成員資格測試環境](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md)。

> [!NOTE]
> 如需有關伺服器專案和資料庫專案的詳細資訊，請參閱 < [Visual Studio 2010 SQL Server 資料庫專案](https://msdn.microsoft.com/library/ff678491.aspx)。


## <a name="configure-permissions-for-the-deployment-account"></a>設定部署帳戶的權限

如果您將使用來執行部署的帳戶不是 SQL Server 系統管理員，您也必須建立此帳戶的登入。 若要建立資料庫，帳戶必須隸屬**dbcreator**伺服器角色或具有同等權限。

> [!NOTE]
> 當您使用 Web Deploy 或 VSDBCMD 將資料庫部署時，您可以使用 Windows 認證或 SQL Server 認證 （如果您的 SQL Server 執行個體已設定為支援混合的模式驗證）。 下一個程序假設您想要使用 Windows 認證，但並不禁止您從 SQL Server 使用者名稱和密碼指定連接字串中，當您將部署設定。


**若要設定部署帳戶的權限**

1. 和以前一樣開啟 SQL Server Management Studio。
2. 中**物件總管** 窗格中，以滑鼠右鍵按一下**安全性**，指向**新增**，然後按一下**登入**。
3. 在**登入-新增**對話方塊中，於**登入名稱**方塊中，輸入您部署的帳戶名稱 (例如**FABRIKAM\matt**)。
4. 在 **選取頁面**窗格中，按一下**伺服器角色**。
5. 選取  **dbcreator**，然後按一下**確定**。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image14.png)

若要支援後續的部署，您也必須將部署的帳戶加入**db\_擁有者**上第一次部署之後的資料庫角色。 這是資料庫的因為後續部署您要修改現有結構描述而非建立新的資料庫。 上一節所述，您無法將使用者新增至資料庫角色中直到您已建立資料庫，原因很明顯。

**若要部署的帳戶登入對應至資料庫\_擁有者的資料庫角色**

1. 和以前一樣開啟 SQL Server Management Studio。
2. 中**物件總管** 視窗中，展開**安全性**節點，展開**登入** 節點，然後按兩下 電腦帳戶登入 (比方說， **FABRIKAM\matt**)。
3. 在 [**登入屬性**] 對話方塊中，按一下**使用者對應**。
4. 在 **對應到此登入的使用者**資料表中，選取您的資料庫名稱 (例如**ContactManager**)。
5. 在 **資料庫角色成員資格對象：** *[資料庫名稱]* 清單中，選取**db\_擁有者**角色。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image15.png)
6. 按一下 [確定 **Deploying Office Solutions**]。

## <a name="conclusion"></a>結論

您的資料庫伺服器現在應該可以接受遠端的資料庫部署，並允許遠端 IIS web 伺服器，以存取您的資料庫。 您嘗試部署，並使用資料庫之前，您可能想要檢查的這些關鍵點：

- 您是否已設定成接受遠端 TCP/IP 連接的 SQL Server？
- 您是否已設定任何防火牆以允許 SQL Server 流量？
- 您已建立將存取 SQL Server 每一部 web 伺服器的電腦帳戶登入？
- 您的資料庫部署是否包含指令碼來建立使用者角色對應，或您需要手動建立這些之後將資料庫部署在第一次,？
- 您建立部署帳戶的登入並加入它**dbcreator**伺服器角色？

## <a name="further-reading"></a>進一步閱讀

如需部署資料庫專案的指引，請參閱 <<c0> [ 部署資料庫專案](../web-deployment-in-the-enterprise/deploying-database-projects.md)。 如需指引，以執行部署後指令碼建立資料庫角色成員資格，請參閱[部署資料庫角色成員資格測試環境](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md)。 如需有關如何符合成員資格資料庫造成的唯一部署挑戰的指引，請參閱[部署至企業環境的成員資格資料庫](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md)。

> [!div class="step-by-step"]
> [上一頁](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
> [下一頁](creating-a-server-farm-with-the-web-farm-framework.md)
