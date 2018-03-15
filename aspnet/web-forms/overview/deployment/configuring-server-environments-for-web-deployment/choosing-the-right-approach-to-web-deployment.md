---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
title: "選擇正確的方法，以 Web 部署 |Microsoft 文件"
author: jrjlee
description: "當您使用網際網路資訊服務 (IIS) Web Deployment Tool (Web Deploy) 2.0 或更新版本時，有三個主要的方法，可用來取得..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 787a53fd-9901-4a11-9d58-61e0509cda45
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: b77aa37160f3822f58908866e44497aea3d3bdc8
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/15/2018
---
<a name="choosing-the-right-approach-to-web-deployment"></a>選擇 Web 部署的正確方法
====================
由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 當您使用網際網路資訊服務 (IIS) Web Deployment Tool (Web Deploy) 2.0 或更新版本時，有三個主要的方法，可用來取得封裝的 web 應用程式到 web 伺服器。 您可以：
> 
> - 從遠端位置的應用程式部署將目標設為*Web Deployment Agent Service* （也稱為 「 遠端代理程式 」） 在目的地伺服器上。
> - 部署應用程式從遠端位置使用 Web 部署隨選安裝 （也稱為 「 暫存代理程式 」）。
> - 從遠端位置的應用程式部署將目標設為*IIS Web 部署處理常式*目的地伺服器上。
> - 部署應用程式，以手動方式將 web 套件複製到目的地伺服器，並透過 IIS 管理員匯入。
> 
> 設定您的目的地 web 伺服器的方式將取決於您想要使用哪種部署方法。 本主題將協助您決定最適合您要部署哪種方法。


下表顯示的主要優點和缺點通常最適合每一種方法的案例以及每個部署方式。

| 方法 | 優點 | 缺點 | 典型案例 |
| --- | --- | --- | --- |
| 遠端代理程式 | 所以可以輕鬆地設定。 適當的 web 應用程式和內容的定期更新。 | 使用者必須是目標伺服器的系統管理員。 使用者無法提供替代認證。 | 開發環境。 測試環境。 |
| 暫存的代理程式 | 沒有需要在目標電腦上安裝 Web Deploy。 會自動使用最新版的 Web Deploy。 | 使用者必須是目標伺服器的系統管理員。 使用者無法提供替代認證。 | 開發環境。 測試環境。 |
| Web Deploy 處理常式 | 非系統管理員的使用者可以將部署內容。 適當的 web 應用程式和內容的定期更新。 | 它是複雜許多設定。 | 預備環境。 內部網路的實際執行環境。 裝載的環境中。 |
| 離線部署 | 它是很容易設定。 它適合用來隔離的環境。 | 伺服器系統管理員必須手動複製，並將 web 套件匯入每次。 | 網際網路對向的實際執行環境。 隔離的網路環境。 |
  

## <a name="using-the-remote-agent"></a>使用遠端代理程式

當您安裝在目的地伺服器上使用預設設定的 Web Deploy 時，Web Deployment Agent Service （「 遠端代理程式 」） 會自動安裝並啟動。 根據預設，遠端代理程式會公開此位址的 HTTP 端點：


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample1.cmd)]


> [!NOTE]
> 您可以取代 [*伺服器*] 與您的 web 伺服器的電腦名稱，您的網頁伺服器或主機名稱的 IP 位址解析為您的 web 伺服器。


伺服器系統管理員可以部署 web 封裝，從遠端位置，例如開發人員電腦或組建伺服器，藉由指定這個端點位址。 例如，假設在 Fabrikam，Inc.Matt 世昕已在其開發人員電腦上建置 ContactManager.Mvc web 應用程式專案。 建置程序產生 web 封裝，並搭配*。 deploy.cmd*安裝封裝所需的檔案，其中包含 Web Deploy 的命令。 如果 Matt TESTWEB1 伺服器上的伺服器系統管理員，他可以部署到測試 web 伺服器的 web 應用程式開發人員電腦上執行此命令：


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample2.cmd)]


在實際事實中，Web Deploy 的可執行檔可以推斷遠端代理程式的端點位址，因此 Matt 只需要輸入下列命令，提供機器名稱時，如果：


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample3.cmd)]


> [!NOTE]
> 如需有關 Web Deploy 命令列語法和*。 deploy.cmd*檔，請參閱[How to: 部署封裝使用 deploy.cmd 檔案安裝](https://msdn.microsoft.com/library/ff356104.aspx)。


遠端代理程式提供直接的方法來部署內容從遠端位置，而且這個方法也可以使用一種單鍵或自動部署。 不過，執行的部署命令的使用者也必須是網域系統管理員或目的地伺服器上本機 administrators 群組的成員。 此外，遠端代理程式不支援基本驗證，所以您無法在命令列上傳遞替代認證。

遠端代理程式提供部署開發或測試案例，其中是很常見的開發人員提供完整的系統管理員控制測試伺服器環境中，而應用程式通常會重建和重新部署非常實用方法常見問題。 不過，這個方法是通常不能接受預備或生產環境。

如需端對端的範例案例中使用遠端代理程式的方法，請參閱[案例： 設定測試環境，用於 Web 部署](scenario-configuring-a-test-environment-for-web-deployment.md)。

## <a name="using-the-temp-agent"></a>使用暫存的代理程式

與遠端代理程式的方法相似暫存的代理程式部署方法。 不過，相較於遠端代理程式的方法，您不需要在目的地 web 伺服器上安裝 Web Deploy。 相反地，當您執行部署時，Web Deploy 將目的地伺服器上安裝 web 部署代理程式服務的暫存版本與將使用此內容部署到 IIS。 部署完成時，會移除所有的暫存檔案。

如果您想要使用暫時代理程式提供者設定，將**/g**旗標設為您的部署命令：


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample4.cmd)]


> [!NOTE]
> 您如果無法使用暫存的代理程式的 web 部署代理程式服務已安裝的目的地電腦上，即使服務未執行。


這個方法的優點是，您不需要維護您的目的地伺服器上的 Web Deploy 安裝。 此外，您不需要確定來源和目的地電腦正在執行相同版本的 Web Deploy。 不過，這種方法受到相同的主要限制遠端代理程式的方法，也就是，您必須是目的地伺服器上的本機系統管理員，才能部署內容，而且只有 NTLM 驗證支援。 暫存的代理程式方法也需要更多初始目的地環境的設定。

如需有關使用暫時代理程式的詳細資訊，請參閱[How to: 部署封裝使用 deploy.cmd 檔案安裝](https://msdn.microsoft.com/library/ff356104.aspx)和[Web 部署隨選](https://technet.microsoft.com/library/ee517345(WS.10).aspx)。

## <a name="using-the-web-deploy-handler"></a>使用 Web Deploy 處理常式

適用於 IIS 7 及更新版本，Web Deploy 提供替代的部署方法，透過 IIS Web 部署處理常式。 IIS Web 管理服務 (WMSvc)，其設計為讓使用者從遠端位置管理 IIS 網站與密切整合 Web 部署處理常式。

根據預設，遠端代理程式會公開此位址的 HTTP 端點：


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample5.cmd)]


> [!NOTE]
> 您可以取代 [*伺服器*] 與您的 web 伺服器的電腦名稱，您的網頁伺服器或主機名稱的 IP 位址解析為您的 web 伺服器。


遠端代理程式，並暫存的代理程式，透過 Web 部署處理常式的最大好處是，您可以設定 IIS 以允許非管理員使用者將應用程式和內容部署到特定的 IIS 網站。 Web 部署處理常式也支援基本驗證，因此您可以在 Web Deploy 命令中做為參數提供替代認證。 主要缺點是，Web 部署處理常式來安裝和設定一開始更加複雜。

在非管理員使用者的情況下 Web 管理服務 (WMSvc) 將只允許使用者連接到 IIS 站台層級的連接，而不是伺服器層級連線使用。 若要存取特定站台，您可以包含的站台特有的查詢字串中的端點位址：


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample6.cmd)]


例如，假設建置程序設定為自動部署到預備環境每次成功建置後的 web 應用程式。 如果您使用遠端代理程式的方法，您必須在目的地伺服器上進行建置處理序身分識別系統管理員。 相反地，使用 Web 部署的處理常式方法您可以提供給非系統管理員使用者 & #x 2014;**FABRIKAM\stagingdeployer**特定 IIS 網站，並在建置程序的權限可以在此情況下 & #x 2014; 提供這些認證來部署 web 封裝。


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample7.cmd)]


> [!NOTE]
> 如需有關 Web Deploy 命令列作業和語法的詳細資訊，請參閱[Web 部署命令列參考](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx)。 如需有關使用*。 deploy.cmd*檔案，請參閱[How to: 部署封裝使用 deploy.cmd 檔案安裝](https://msdn.microsoft.com/library/ff356104.aspx)。


Web 部署處理常式提供部署在預備環境、 託管的環境和內部網路為基礎的實際執行環境，其中可以遠端存取伺服器。 但系統管理員認證的有效方法。

端對端案例中使用的 Web 部署的處理常式方法的範例，請參閱[案例： 設定用於 Web 部署的預備環境](scenario-configuring-a-staging-environment-for-web-deployment.md)。

## <a name="using-offline-deployment"></a>使用離線部署

在某些情況下，就不可能或切實的方法是從遠端位置中將應用程式和內容部署至 IIS 網站。 例如，來源和目的電腦可能位於隔離的網路或網路區段或防火牆原則可能不會允許遠端存取。

在這些情況下，您仍然可以使用封裝和發行功能的 Web Deploy。您就無法使用它們從遠端位置。 相反地，在目的地伺服器上的系統管理員必須複製到伺服器之 web 封裝，並匯入它透過 IIS 管理員。

![](choosing-the-right-approach-to-web-deployment/_static/image1.png)

離線部署方法是在網際網路對向實際執行環境中，周邊網路中的伺服器可能有限制的連線與內部網路中的電腦通常很有用。

使用離線部署方法之案例的端對端範例，請參閱[案例： 實際執行環境中設定用於 Web 部署](scenario-configuring-a-production-environment-for-web-deployment.md)。

## <a name="further-reading"></a>進一步閱讀

如需有關 Web Deploy 命令列作業和語法的詳細資訊，請參閱[Web 部署命令列參考](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx)。 如需有關使用*。 deploy.cmd*檔案，請參閱[How to: 部署封裝使用 deploy.cmd 檔案安裝](https://msdn.microsoft.com/library/ff356104.aspx)。

多個一般指引，您可以在其中部署 web 封裝，從遠端電腦的不同方式的詳細資訊，請參閱[使用 Web 部署遠端](https://technet.microsoft.com/library/ee461175(WS.10).aspx)。 如需使用 Web 部署隨選安裝的詳細資訊，請參閱[Web 部署隨選](https://technet.microsoft.com/library/ee517345(WS.10).aspx)。

>[!div class="step-by-step"]
[上一頁](configuring-server-environments-for-web-deployment.md)
[下一頁](scenario-configuring-a-test-environment-for-web-deployment.md)
