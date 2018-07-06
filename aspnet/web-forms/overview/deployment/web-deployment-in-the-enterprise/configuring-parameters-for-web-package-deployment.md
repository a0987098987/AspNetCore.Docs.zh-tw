---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
title: 設定網頁套件部署的參數 |Microsoft Docs
author: jrjlee
description: 本主題描述如何設定參數值，例如 Internet Information Services (IIS) web 應用程式名稱、 連接字串和服務端點...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 37947d79-ab1e-4ba9-9017-52e7a2757414
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
msc.type: authoredcontent
ms.openlocfilehash: dd8924b0b0055bd32ef55a9ec3a139c4d9b4eb81
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825111"
---
<a name="configuring-parameters-for-web-package-deployment"></a>網頁套件部署的設定參數
====================
藉由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主題描述如何設定參數值，Internet Information Services (IIS) web 應用程式名稱、 連接字串和服務端點，例如，當您將 web 套件部署到遠端 IIS web 伺服器。


當您建置的 web 應用程式專案、 建置和封裝程序會產生三個索引鍵的檔案：

- A *[專案名稱].zip*檔案。 這是 web 應用程式專案的 web 部署套件。 此套件包含所有組件、 檔案、 資料庫指令碼和重新建立您的 web 應用程式，遠端的 IIS 網頁伺服器上所需的資源。
- A *[專案名稱].deploy.cmd*檔案。 這包含一組參數化 Web Deploy (MSDeploy.exe) 命令，將您的 web 部署套件發佈至遠端 IIS web 伺服器。
- A *[專案名稱]。之 SetParameters.xml*檔案。 這提供一組 MSDeploy.exe 命令的參數值。 您可以更新此檔案中的值，並將它傳遞至 Web Deploy 命令列參數為當您將 web 套件部署。

> [!NOTE]
> 如需有關建置和封裝程序的詳細資訊，請參閱 <<c0> [ 建置和封裝 Web Application Projects](building-and-packaging-web-application-projects.md)。


*之 SetParameters.xml*從您的 web 應用程式專案檔和您的專案內的任何設定檔，以動態方式產生檔案。 當您建置並封裝您的專案中，Web 發行管線 (WPP) 會自動偵測許多可能會變更部署的環境，例如目的地 IIS web 應用程式之間的任何資料庫連接字串的變數。 這些值會自動參數化的 web 部署套件中，並加入*之 SetParameters.xml*檔案。 例如，如果您加入的連接字串*web.config*檔案中您的 web 應用程式專案，建置流程會偵測到這項變更並將項目新增至*之 SetParameters.xml*檔案據此。

在許多情況下，此自動參數化會足夠。 不過，如果您的使用者需要變更其他設定之間部署環境，例如應用程式設定] 或 [服務端點 Url，您需要告訴參數化的部署套件中的這些值，並將對應的項目加入至WPP*之 SetParameters.xml*檔案。 下列各節說明如何執行這項操作。

### <a name="automatic-parameterization"></a>自動參數化

當您建置和封裝 web 應用程式時，WPP，將會自動將這些項目：

- 目的地 IIS web 應用程式路徑和名稱。
- 任何連接字串中您*web.config*檔案。
- 您將新增至任何資料庫的連接字串**封裝/發行 SQL**在專案屬性頁 索引標籤。

例如，如果您要建置和封裝[Contactmanager](the-contact-manager-solution.md)不必以任何方式，WPP 參數化程序的範例解決方案會產生這個*ContactManager.Mvc.SetParameters.xml*檔案：


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample1.xml)]


在此情況下：

- **IIS Web 應用程式名稱**參數是您要部署 web 應用程式的 IIS 路徑。 預設值取自**封裝/發行 Web**專案屬性頁中的頁面。
- **ApplicationServices 連接字串**參數所產生**connectionStrings/新增**中的項目*web.config*檔案。 它代表應用程式應該使用連絡成員資格資料庫的連接字串。 將會是您所提供的值替代至已部署*web.config*檔案。 預設值取自部署前*web.config*檔案。

WPP 也會參數化這些屬性，它會產生的部署套件中。 當您安裝的部署套件時，您便可以提供這些屬性的值。 如果您安裝套件以手動方式透過 IIS 管理員 中所述[手動安裝網頁套件](manually-installing-web-packages.md)，安裝精靈會提示您提供任何參數的值。 如果您安裝從遠端使用的套件 *。 deploy.cmd*檔案中所述[部署網頁套件](deploying-web-packages.md)，Web Deploy 起來至此*之 SetParameters.xml*檔案提供參數值。 您可以編輯中的值*之 SetParameters.xml*檔案以手動的方式，或您可以自訂檔案自動化建置和部署程序的一部分。 此程序所述本主題稍後的更多詳細資料。

### <a name="custom-parameterization"></a>自訂參數化

在更複雜的部署案例中，您通常會想要參數化的其他屬性，然後再部署您的專案。 一般而言，您應該參數化任何屬性與目的地環境之間而異的設定。 這些可能包括：

- 服務中的端點*web.config*檔案。
- 中的應用程式設定*web.config*檔案。
- 任何其他宣告式屬性，您想要提示使用者指定。

參數化這些屬性的最簡單方式是將*無效*web 應用程式專案的根資料夾的檔案。 比方說，在 連絡管理員解決方案，ContactManager.Mvc 專案包含*無效*根資料夾中的檔案。

![](configuring-parameters-for-web-package-deployment/_static/image1.png)

如果您開啟此檔案時，您會看到它包含單一**參數**項目。 項目會使用 XML 路徑語言 (XPath) 查詢來找出並參數化中的 ContactService Windows Communication Foundation (WCF) 服務的端點 URL *web.config*檔案。


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample2.xml)]


除了參數化部署套件中的端點 URL，WPP 也新增至對應的項目*之 SetParameters.xml*隨部署套件產生的檔案。


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample3.xml)]


如果您手動安裝的部署套件時，IIS 管理員 會提示您的服務端點位址，以及已自動參數化的屬性。 如果您執行安裝的部署套件 *。 deploy.cmd*檔案中，您可以編輯*之 SetParameters.xml*檔案，以提供的服務端點位址，以及值的值已自動參數化的屬性。

如需如何建立完整的詳細資訊*無效*檔案，請參閱[如何： 使用參數來設定部署設定時封裝會安裝](https://msdn.microsoft.com/library/ff398068.aspx)。 名為程序**要用於 Web.config 檔案設定的部署參數**提供逐步指示。

## <a name="modifying-the-setparametersxml-file"></a>修改之 SetParameters.xml 檔案

如果您打算以手動方式部署 web 應用程式封裝&#x2014;藉由執行 *。 deploy.cmd*檔案或從命令列執行 MSDeploy.exe&#x2014;沒有要阻止您以手動方式編輯*之 SetParameters.xml*之前部署的檔案。 不過，如果您正在操作的企業級解決方案，您可能需要將 web 應用程式封裝部署為更大的自動化建置和部署程序的一部分。 在此案例中，您需要 Microsoft Build Engine (MSBuild) 來修改*之 SetParameters.xml*為您的檔案。 您可以使用 MSBuild **XmlPoke**工作。

[Contact Manager 範例方案](the-contact-manager-solution.md)說明此程序。 請依照下列的程式碼範例有經過編輯，以顯示與此範例的詳細資料。

> [!NOTE]
> 專案檔案模型中的範例解決方案，以及自訂的專案檔的一般簡介的廣泛概觀，請參閱 <<c0> [ 了解專案檔](understanding-the-project-file.md)並[了解建置程序](understanding-the-build-process.md)。


首先，感興趣的參數值會定義為環境特有的專案檔中的屬性 (例如*Env Dev.proj*)。


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample4.xml)]


> [!NOTE]
> 如需如何自訂您自己的伺服器環境的特定環境的專案檔的指引，請參閱 <<c0> [ 設定的目標環境的部署屬性](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md)。


下一步 *Publish.proj*檔匯入這些屬性。 因為每個*之 SetParameters.xml*檔案相關聯 *。 deploy.cmd*檔案，以及我們最終想要叫用每個專案檔 *。 deploy.cmd*檔案，專案檔案會建立 MSBuild*項目*每個 *。 deploy.cmd*檔案，並為想要的屬性會定義*項目中繼資料*。


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample5.xml)]


在此情況下：

- **ParametersXml**中繼資料值表示的位置*之 SetParameters.xml*檔案。
- **IisWebAppName**值是您要部署 web 應用程式的 IIS 路徑。
- **MembershipDBConnectionString**值為連接字串的成員資格資料庫中，而**MembershipDBConnectionName**值是**名稱**屬性中的對應參數*之 SetParameters.xml*檔案。
- **ServiceEndpointValue**值為在目的地伺服器上的 WCF 服務的端點位址和**ServiceEndpointParamName**值是 name 屬性中的對應參數*之 SetParameters.xml*檔案。

最後，在*Publish.proj*檔案， **PublishWebPackages**目標會使用**XmlPoke**工作來修改這些值在*之 SetParameters.xml*檔案。


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample6.xml)]


您會在每個發現**XmlPoke**工作可讓您指定四個屬性值：

- **XmlInputPath**屬性會告知工作哪裡可以找到您想要修改的檔案。
- **查詢**屬性會識別您想要變更的 XML 節點的 XPath 查詢。
- **值**屬性是您想要插入至所選取的 XML 節點的新值。
- **條件**屬性是工作應該在其執行，或無法執行的準則。 在這些情況下，條件可確保您不要嘗試為 null 或空白值插入*之 SetParameters.xml*檔案。

## <a name="conclusion"></a>結論

本主題所述的角色*之 SetParameters.xml*檔案，並說明如何產生當您建置的 web 應用程式專案。 它說明如何將其他設定參數化加*無效*檔案加入專案。 它也說明如何修改*之 SetParameters.xml*更大的自動化建置程序，利用一部分的檔案**XmlPoke**專案檔中的工作。

下一個主題中，[部署網頁套件](deploying-web-packages.md)，說明如何部署網頁套件藉由執行 *。 deploy.cmd*檔案，或直接透過 MSDeploy.exe 命令。 在這兩種情況下，您可以指定您*之 SetParameters.xml*作為部署參數的檔案。

## <a name="further-reading"></a>進一步閱讀

如需如何建立 web 封裝的資訊，請參閱[建置和封裝 Web Application Projects](building-and-packaging-web-application-projects.md)。 如需如何實際部署網頁套件的指引，請參閱[部署網頁套件](deploying-web-packages.md)。 如需如何建立的逐步解說*無效*檔案，請參閱[如何： 使用參數來設定部署設定時封裝會安裝](https://msdn.microsoft.com/library/ff398068.aspx)。

更多一般參數化的 Web Deploy 的詳細資訊，請參閱[動作中的 Web 部署參數化](https://go.microsoft.com/?linkid=9805119)（部落格文章）。

> [!div class="step-by-step"]
> [上一頁](building-and-packaging-web-application-projects.md)
> [下一頁](deploying-web-packages.md)
