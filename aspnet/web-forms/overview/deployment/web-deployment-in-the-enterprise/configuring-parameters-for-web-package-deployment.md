---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
title: "用於 Web 套件部署中設定參數 |Microsoft 文件"
author: jrjlee
description: "本主題描述如何設定參數值，例如網際網路資訊服務 (IIS) web 應用程式名稱、 連接字串，以及服務端點..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 37947d79-ab1e-4ba9-9017-52e7a2757414
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
msc.type: authoredcontent
ms.openlocfilehash: dd1ae266740ea4728c0624b1833a98ac262e0e5a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="configuring-parameters-for-web-package-deployment"></a>用於 Web 套件部署中設定參數
====================
由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主題描述如何設定參數值，例如網際網路資訊服務 (IIS) web 應用程式名稱、 連接字串，以及服務端點，當您將 web 套件部署到遠端的 IIS 網頁伺服器。


當您建立 web 應用程式專案，建置和封裝程序會產生三個索引鍵的檔案：

- A *[專案名稱].zip*檔案。 這是 web 應用程式專案的 web 部署套件。 此套件包含所有組件、 檔案、 資料庫指令碼和重新建立您的 web 應用程式，遠端的 IIS 網頁伺服器上所需的資源。
- A *[專案名稱].deploy.cmd*檔案。 這包含一組的參數化 Web Deploy (MSDeploy.exe) 命令，將您的 web 部署套件發佈到遠端的 IIS 網頁伺服器。
- A *[專案名稱]。SetParameters.xml*檔案。 這會提供一組 MSDeploy.exe 命令的參數值。 您可以更新這個檔案中的值，並將它傳遞至 Web Deploy 做為命令列參數部署 web 封裝時。

> [!NOTE]
> 如需有關建置和封裝程序的詳細資訊，請參閱[建置和封裝 Web 應用程式專案](building-and-packaging-web-application-projects.md)。


*SetParameters.xml*從您的 web 應用程式專案檔和任何組態檔，在專案中的動態產生檔案。 當您建置並封裝您的專案，Web 發行管線 (WPP) 會自動偵測許多可能會變更部署的環境，例如目的 IIS web 應用程式之間的任何資料庫連接字串的變數。 這些值會自動參數化 web 部署套件中，並新增到*SetParameters.xml*檔案。 例如，如果您加入的連接字串*web.config*檔案在 web 應用程式專案中，在建置程序會偵測這項變更，並將新增的項目，以*SetParameters.xml*檔案據以。

在許多情況下，此自動參數化會足夠。 不過，如果您的使用者需要變更其他設定之間部署環境，例如應用程式設定或服務端點 Url，您需要告訴部署套件中的這些值參數化，並將對應的項目加入至WPP*SetParameters.xml*檔案。 以下各節說明如何執行這項操作。

### <a name="automatic-parameterization"></a>自動參數化

當您建置並封裝 web 應用程式時，WPP 將自動參數化這些項目：

- 目的地 IIS web 應用程式路徑和名稱。
- 中的任何連接字串您*web.config*檔案。
- 您將加入至任何資料庫的連接字串**封裝/發行 SQL**在專案屬性頁 索引標籤。

例如，如果您要建置並封裝[連絡人管理員](the-contact-manager-solution.md)範例方案，但沒有碰觸的參數化程序，以任何方式，WPP 仍會產生這*ContactManager.Mvc.SetParameters.xml*檔案：


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample1.xml)]


在此情況下：

- **IIS Web 應用程式名稱**參數是您要部署 web 應用程式的 IIS 路徑。 預設值取自**封裝/發行 Web**專案屬性頁中的頁面。
- **ApplicationServices Web.config 連接字串**參數產生自**connectionStrings/加入**中的項目*web.config*檔案。 它代表應用程式應該使用連絡成員資格資料庫的連接字串。 這裡將您所提供的值取代已部署至*web.config*檔案。 預設值取自預先部署*web.config*檔案。

WPP 也會參數化這些屬性，它會產生的部署套件中。 當您安裝的部署套件時，您便可以提供這些屬性的值。 如果您安裝封裝手動透過 IIS 管理員 中所述[手動安裝 Web 封裝](manually-installing-web-packages.md)，安裝精靈會提示您提供任何參數的值。 如果您使用安裝套件遠端*。 deploy.cmd*檔案中所述[部署 Web 封裝](deploying-web-packages.md)，Web Deploy 會尋找此*SetParameters.xml*檔案提供參數值。 您可以編輯中的值*SetParameters.xml*手動檔案，或者您可以自訂檔案做為自動化組建和部署程序的一部分。 此程序所述在本主題稍後的更多詳細資料。

### <a name="custom-parameterization"></a>自訂參數化

在更複雜的部署案例中，您通常會想要參數化的其他屬性，才能部署您的專案。 一般而言，您應該參數化任何屬性與目的地環境之間而異的設定。 這些可以包括：

- 服務端點中的*web.config*檔案。
- 中的應用程式設定*web.config*檔案。
- 任何其他宣告式屬性，您想要提示使用者指定。

參數化這些屬性的最簡單方式是加入*parameters.xml*檔案到您的 web 應用程式專案的根資料夾。 例如，在連絡人管理員解決方案中，ContactManager.Mvc 專案包含*parameters.xml*根資料夾中的檔案。

![](configuring-parameters-for-web-package-deployment/_static/image1.png)

如果您開啟這個檔案，您會看到它包含單一**參數**項目。 項目會使用 XML 路徑語言 (XPath) 查詢來找出並參數化的 ContactService Windows Communication Foundation (WCF) 服務的端點 URL *web.config*檔案。


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample2.xml)]


除了將部署套件中的端點 URL 參數化，WPP 也會加入至對應的項目*SetParameters.xml*取得部署套件與產生的檔案。


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample3.xml)]


如果您手動安裝的部署套件時，IIS 管理員會提示您輸入的服務端點位址，連同已自動參數化的屬性。 如果您執行安裝的部署套件*。 deploy.cmd*檔案中，您可以編輯*SetParameters.xml*檔案，以提供的服務端點位址，以及值的值已自動參數化的屬性。

如需如何建立完整細節*parameters.xml*檔案，請參閱[How to： 使用參數來設定部署設定時封裝會安裝](https://msdn.microsoft.com/en-us/library/ff398068.aspx)。 名為程序**部署參數用於 Web.config 檔案設定**提供逐步指示。

## <a name="modifying-the-setparametersxml-file"></a>修改 SetParameters.xml 檔案

如果您打算以手動方式部署 web 應用程式套件 & #x 2014年; 藉由執行*。 deploy.cmd*檔案，或從命令列 & #x 2014; 執行 MSDeploy.exe 沒有阻止您以手動方式編輯*SetParameters.xml*之前部署的檔案。 不過，如果您使用企業規模方案，您可能需要較大，自動化組建和部署程序的一部分部署 web 應用程式套件。 在此案例中，您需要 Microsoft Build Engine (MSBuild) 修改*SetParameters.xml*為您的檔案。 您可以使用 MSBuild **XmlPoke**工作。

[連絡人管理員範例方案](the-contact-manager-solution.md)說明此程序。 以下程式碼範例都已經過編輯，以顯示與此範例的詳細資料。

> [!NOTE]
> 如需更廣泛的專案檔案模型中的範例解決方案，以及自訂的專案檔的一般簡介概觀，請參閱[了解專案檔](understanding-the-project-file.md)和[瞭解建置程序](understanding-the-build-process.md)。


首先，您感興趣的參數值會定義為特定環境的專案檔中的屬性 (例如， *Env Dev.proj*)。


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample4.xml)]


> [!NOTE]
> 如需如何自訂伺服器環境的特定環境的專案檔案的指引，請參閱[目標環境設定部署屬性](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md)。


下一步 *Publish.proj*檔匯入這些屬性。 因為每個*SetParameters.xml*與檔案關聯*。 deploy.cmd*檔案，以及我們最終需要在專案檔以便可以叫用每個*。 deploy.cmd*檔案，專案檔案建立 MSBuild*項目*每個*。 deploy.cmd*檔案，並定義為感興趣的屬性*項目中繼資料*。


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample5.xml)]


在此情況下：

- **ParametersXml**中繼資料值表示的位置*SetParameters.xml*檔案。
- **IisWebAppName**值是您要部署 web 應用程式的 IIS 路徑。
- **MembershipDBConnectionString**值是成員資格資料庫中，連接字串和**MembershipDBConnectionName**值是**名稱**屬性中的對應參數*SetParameters.xml*檔案。
- **ServiceEndpointValue**值是目的地伺服器上的 WCF 服務的端點位址和**ServiceEndpointParamName**值是中的對應參數的名稱屬性*SetParameters.xml*檔案。

最後，在*Publish.proj*檔案， **PublishWebPackages**目標使用**XmlPoke**工作來修改這些值在*SetParameters.xml*檔案。


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample6.xml)]


您將會注意到，每個**XmlPoke**工作指定四個屬性值：

- **XmlInputPath**屬性會告知工作如何尋找您想要修改的檔案。
- **查詢**屬性是識別您想要變更的 XML 節點的 XPath 查詢。
- **值**屬性是您想要插入選取的 XML 節點的新值。
- **條件**是屬性的準則，工作應該在其執行，或無法執行。 在這些情況下，條件可確保您不要嘗試 null 或空白值插入*SetParameters.xml*檔案。

## <a name="conclusion"></a>結論

本主題所述的角色*SetParameters.xml*檔案，並說明其產生的方法時建立的 web 應用程式專案。 它說明如何將其他設定參數化加入*parameters.xml*檔案加入專案。 它也說明如何修改*SetParameters.xml*檔案的較大的自動建置程序，使用為**XmlPoke**專案檔中的工作。

下一個主題[部署 Web 封裝](deploying-web-packages.md)，說明如何部署 web 封裝藉由執行*。 deploy.cmd*檔案，或直接使用 MSDeploy.exe 命令。 在這兩種情況下，您可以指定您*SetParameters.xml*檔案做為部署參數。

## <a name="further-reading"></a>進一步閱讀

如需如何建立 web 封裝的資訊，請參閱[建置和封裝 Web 應用程式專案](building-and-packaging-web-application-projects.md)。 如需如何部署 web 封裝的指引，請參閱[部署 Web 封裝](deploying-web-packages.md)。 如需如何建立的逐步解說*parameters.xml*檔案，請參閱[How to： 使用參數來設定部署設定時封裝會安裝](https://msdn.microsoft.com/en-us/library/ff398068.aspx)。

一般參數化的 Web Deploy 的詳細資訊，請參閱[動作中的 Web 部署參數化](https://go.microsoft.com/?linkid=9805119)（部落格文章）。

>[!div class="step-by-step"]
[上一頁](building-and-packaging-web-application-projects.md)
[下一頁](deploying-web-packages.md)
