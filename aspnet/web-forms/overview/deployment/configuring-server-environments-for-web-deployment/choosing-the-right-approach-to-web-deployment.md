---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
title: 選擇 Web 部署的正確方法 |Microsoft Docs
author: jrjlee
description: 當您使用 Internet Information Services (IIS) Web Deployment Tool (Web Deploy) 2.0 或更新版本時，有三種主要的方法，可用來取得...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 787a53fd-9901-4a11-9d58-61e0509cda45
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: f1c3a09d9e960a53a25e0dd0b953224204a9ba7b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37367741"
---
<a name="choosing-the-right-approach-to-web-deployment"></a>選擇 Web 部署的正確方法
====================
藉由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 當您使用 Internet Information Services (IIS) Web Deployment Tool (Web Deploy) 2.0 或更新版本時，有三種主要的方法，可用來取得封裝的 web 應用程式到 web 伺服器。 您可以：
> 
> - 將從遠端位置的應用程式部署將目標設為*Web Deployment Agent Service* （也稱為 「 遠端代理程式 」） 在目的地伺服器上。
> - 部署應用程式從遠端位置使用 Web 部署 On Demand （也稱為 「 暫存代理程式 」）。
> - 將從遠端位置的應用程式部署將目標設為*IIS Web 部署處理常式*目的地伺服器上。
> - 部署應用程式，以手動方式將 web 套件複製到目的地伺服器，並透過 IIS 管理員匯入。
> 
> 設定您的目的地 web 伺服器的方式將取決於您想要使用哪種方法來部署。 本主題將協助您決定要部署哪種方法最適合您。


下表顯示主要的優點和缺點，每個部署方法，以及最常配合每一種方法的案例。

| 方法 | 優點 | 缺點 | 典型的案例 |
| --- | --- | --- | --- |
| 遠端代理程式 | 它很容易設定。 它是適合 web 應用程式和內容的定期更新。 | 使用者必須是目標伺服器的系統管理員。 使用者無法提供替代的認證。 | 開發環境。 測試環境。 |
| 暫存的代理程式 | 沒有需要在目標電腦上安裝 Web Deploy。 會自動使用最新版的 Web Deploy。 | 使用者必須是目標伺服器的系統管理員。 使用者無法提供替代的認證。 | 開發環境。 測試環境。 |
| Web Deploy 處理常式 | 非系統管理員使用者可以將內容部署。 它是適合 web 應用程式和內容的定期更新。 | 它是設為更加複雜。 | 預備環境。 內部網路的實際執行環境。 主控的環境。 |
| 離線部署 | 它是非常容易設定。 它適合隔離的環境。 | 伺服器系統管理員必須手動複製並匯入 web 封裝每次。 | 網際網路對向的生產環境。 隔離的網路環境。 |
  

## <a name="using-the-remote-agent"></a>使用遠端代理程式

當您安裝 Web Deploy 使用目的地伺服器上的預設設定時，Web Deployment Agent Service （「 遠端代理程式 」） 會自動安裝並啟動。 根據預設，遠端代理程式會公開 HTTP 端點，在此位址：


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample1.cmd)]


> [!NOTE]
> 您可以取代 [*server*] 與您的 web 伺服器的電腦名稱，您的 web 伺服器或主機名稱的 IP 位址解析為您的 web 伺服器。


伺服器系統管理員可以部署 web 封裝，從遠端位置，例如開發人員電腦或組建伺服器，藉由指定這個端點位址。 例如，假設 Fabrikam，inc.的 Matt 世昕他的開發人員機器上建置 ContactManager.Mvc web 應用程式專案。 建置程序會產生 web 封裝，並搭配 *。 deploy.cmd*安裝套件所需的檔案，其中包含的 Web Deploy 命令。 如果 Matt 是常青州立大學 TESTWEB1 伺服器上的伺服器管理員，他可以部署來測試 web 伺服器的 web 應用程式開發人員機器上執行下列命令：


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample2.cmd)]


實際事實上，如果您提供的電腦名稱，因此 Matt 只需要鍵入此 Web Deploy 的可執行檔時，可以推斷遠端代理程式的端點位址：


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample3.cmd)]


> [!NOTE]
> 如需有關 Web Deploy 命令列語法和 *。 deploy.cmd*檔，請參閱[如何： 部署封裝使用 deploy.cmd 檔案安裝](https://msdn.microsoft.com/library/ff356104.aspx)。


遠端代理程式提供直接的方法，從遠端位置，部署內容，這種方法也可以使用一種單鍵或自動部署。 不過，執行部署命令的使用者也必須是網域系統管理員或目的地伺服器上的本機 administrators 群組的成員。 此外，遠端代理程式不支援基本驗證，因此您無法傳遞命令列上的替代認證。

遠端代理程式提供部署在開發或測試案例，其中並不是罕見狀況測試伺服器環境的完整系統管理員控制的開發人員和應用程式通常會重建和重新部署非常有用的方式常見問題。 不過，這種方法是通常較低可接受的預備環境或生產環境。

如需端對端的範例會使用遠端代理程式方法的案例，請參閱[案例： 設定 Web 部署的測試環境](scenario-configuring-a-test-environment-for-web-deployment.md)。

## <a name="using-the-temp-agent"></a>使用暫存的代理程式

部署暫存的代理程式方法是類似於遠端代理程式的方法。 不過，相較於遠端代理程式的方法，您不需要在目的地 web 伺服器上安裝 Web Deploy。 相反地，當您執行部署時，Web Deploy 會在目的地伺服器上安裝 web deployment agent service 暫存版本並將使用此 url 將內容部署至 IIS。 部署完成時，會移除所有的暫存檔案。

如果您想要使用暫存的代理程式提供者設定，請將 **/g**旗標，以您的部署命令：


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample4.cmd)]


> [!NOTE]
> 您無法使用暫存的代理程式如果 web 部署代理程式服務已安裝的目的地電腦上，即使服務並未執行。


此方法的優點是，您不需要維護您的目的地伺服器上的 Web Deploy 的安裝。 此外，您不需要確定來源和目的地電腦執行相同版本的 Web Deploy。 不過，這種方法有相同的主要限制遠端代理程式的方法，也就是，您必須是目的地伺服器上的本機系統管理員，才能部署內容，而且支援只 NTLM 驗證。 暫存的代理程式的方法也需要更多初始設定目的地環境。

如需有關如何使用暫存的代理程式的詳細資訊，請參閱 <<c0> [ 如何： 部署封裝使用 deploy.cmd 檔案安裝](https://msdn.microsoft.com/library/ff356104.aspx)並[On Demand Web 部署](https://technet.microsoft.com/library/ee517345(WS.10).aspx)。

## <a name="using-the-web-deploy-handler"></a>使用 Web Deploy 處理常式

適用於 IIS 7 及更新版本，Web Deploy 提供其他部署方法，透過 IIS Web 部署處理常式。 與 IIS Web 管理服務 (WMSvc)，其設計目的是要讓使用者從遠端位置管理 IIS 網站緊密整合，Web 部署處理常式。

根據預設，遠端代理程式會公開 HTTP 端點，在此位址：


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample5.cmd)]


> [!NOTE]
> 您可以取代 [*server*] 與您的 web 伺服器的電腦名稱，您的 web 伺服器或主機名稱的 IP 位址解析為您的 web 伺服器。


遠端代理程式，以及暫存的代理程式，Web 部署處理常式的最大好處是，您可以設定 IIS 以允許非系統管理員將應用程式和內容部署到特定的 IIS 網站的使用者。 Web 部署處理常式也會支援基本驗證，因此您可以做為參數提供替代的認證，在您的 Web Deploy 命令。 主要的缺點是，Web 部署處理常式一開始更多複雜而無法安裝和設定。

在非系統管理員使用者的情況下 Web 管理服務 (WMSvc) 只允許使用者連接到 IIS 使用站台層級的連線，而不是伺服器層級的連線。 若要存取特定的網站，您可以包含站台特有的查詢字串中的端點位址：


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample6.cmd)]


例如，假設您的建置流程設定為自動部署至預備環境之後每次成功建置 web 應用程式。 如果您使用遠端代理程式的方法，您必須在目的地伺服器上進行建置處理序身分識別系統管理員。 相反地，使用 Web 部署的處理常式方法您可以授與非系統管理員使用者&#x2014;**FABRIKAM\stagingdeployer**在此情況下&#x2014;的特定 IIS 網站和建置程序的權限可以提供這些若要部署網頁套件的認證。


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample7.cmd)]


> [!NOTE]
> 如需有關 Web Deploy 命令列的作業和語法的詳細資訊，請參閱 < [Web 部署的命令列參考](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx)。 如需有關使用 *。 deploy.cmd*檔案，請參閱[如何： 部署封裝使用 deploy.cmd 檔案安裝](https://msdn.microsoft.com/library/ff356104.aspx)。


Web 部署處理常式提供部署在預備環境、 託管的環境和內部網路為基礎的生產環境中，遠端存取伺服器可用，但系統管理員認證不是很實用的方法。

如需端對端的範例會使用 Web 部署的處理常式方法的案例，請參閱 <<c0> [ 案例： 設定 Web 部署的預備環境](scenario-configuring-a-staging-environment-for-web-deployment.md)。

## <a name="using-offline-deployment"></a>使用離線部署

在某些情況下，它是不可行或不合實際從遠端位置中將應用程式和內容部署至 IIS 網站。 比方說，在來源與目的地電腦可能會在隔離的網路或網路區段或防火牆原則可能不會允許遠端存取。

在這類的情況下，您仍然可以使用封裝和發佈的 Web Deploy; 的功能您只是無法使用它們從遠端位置。 相反地，在目的地伺服器上的系統管理員必須複製到伺服器上的 web 套件，並匯入它透過 IIS 管理員。

![](choosing-the-right-approach-to-web-deployment/_static/image1.png)

離線部署方法是在網際網路對向生產環境中，周邊網路中的伺服器可能有限制的連線與內部網路中的電腦通常很有用。

如需端對端的範例會使用離線部署方法的案例，請參閱 <<c0> [ 案例： 設定 Web 部署的生產環境](scenario-configuring-a-production-environment-for-web-deployment.md)。

## <a name="further-reading"></a>進一步閱讀

如需有關 Web Deploy 命令列的作業和語法的詳細資訊，請參閱 < [Web 部署的命令列參考](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx)。 如需有關使用 *。 deploy.cmd*檔案，請參閱[如何： 部署封裝使用 deploy.cmd 檔案安裝](https://msdn.microsoft.com/library/ff356104.aspx)。

更多一般指引，您可以在其中部署 web 封裝，從遠端電腦的不同方式的詳細資訊，請參閱 <<c0> [ 使用 Web 部署遠端](https://technet.microsoft.com/library/ee461175(WS.10).aspx)。 如需有關使用 On Demand Web 部署的詳細資訊，請參閱[On Demand Web 部署](https://technet.microsoft.com/library/ee517345(WS.10).aspx)。

> [!div class="step-by-step"]
> [上一頁](configuring-server-environments-for-web-deployment.md)
> [下一頁](scenario-configuring-a-test-environment-for-web-deployment.md)
