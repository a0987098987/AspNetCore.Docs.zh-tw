---
uid: web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
title: 使用 Visual Studio 的 ASP.NET Web 部署： Web.config 檔轉換 |Microsoft Docs
author: tdykstra
description: 本系列教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web Apps 或協力廠商裝載提供者，使用...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 5a2a927b-14cb-40bc-867a-f0680f9febd7
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
msc.type: authoredcontent
ms.openlocfilehash: 58f3daac5e9fbe6d327f19b6ae78ea17a26cd7e7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833242"
---
<a name="aspnet-web-deployment-using-visual-studio-webconfig-file-transformations"></a>使用 Visual Studio 的 ASP.NET Web 部署： Web.config 檔案轉換
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

[下載入門專案](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> 本系列教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web Apps 或協力廠商裝載提供者，使用 Visual Studio 2012 或 Visual Studio 2010。 這個系列的相關資訊，請參閱[系列的第一個教學課程](introduction.md)。


## <a name="overview"></a>總覽

本教學課程會示範如何將變更的程序自動化*Web.config*檔案時將它部署至不同目的地環境。 大部分的應用程式中有設定*Web.config*應用程式部署時必須是不同的檔案。 自動化程序可確保這些變更讓您不必手動執行它們，每次部署，這是很麻煩又容易出錯。

提醒： 如果您收到錯誤訊息，或當您瀏覽本教學課程，項目無法運作，請務必檢查[疑難排解頁面](troubleshooting.md)。

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>與 Web 部署參數的 Web.config 轉換

有兩種方式可將變更的程序自動化*Web.config*檔案設定： [Web.config 轉換](https://msdn.microsoft.com/library/dd465326.aspx)並[Web Deploy 參數](https://msdn.microsoft.com/library/ff398068.aspx)。 A *Web.config*轉換檔案包含指定如何變更的 XML 標記*Web.config*時部署檔案。 您可以指定不同的變更，針對特定組建組態，以及針對特定發行設定檔。 預設組建組態偵錯和發行，而且您可以建立自訂組建組態。 發行設定檔通常會對應到目的地環境。 (您將了解發行中的設定檔[部署到 IIS 作為測試環境](deploying-to-iis.md)教學課程。)

Web 部署參數可用來指定許多不同類型的設定必須設定在部署期間，包括設定中找到*Web.config*檔案。 當用來指定*Web.config*檔案變更 Web 部署參數是更複雜的設定，但其適用時您不知道您將部署之前設定的值。 例如，在企業環境中，您可能會建立*部署套件*給予 IT 部門的人員，將安裝在生產環境，以及該人員具有能夠輸入連接字串或不這麼做的密碼了解。

本教學課程系列涵蓋的案例中，您事先知道的所有項目完成才能*Web.config*檔案，因此您不需要使用 Web Deploy 的參數。 您需要設定一些轉換，而有所不同，組建組態和一些使用發行設定檔而有所不同。

<a id="watransforms"></a>

## <a name="specifying-webconfig-settings-in-azure"></a>在 Azure 中的指定 Web.config 設定

如果*Web.config*您想要變更的檔案設定位於`<connectionStrings>`或`<appSettings>`項目，而且如果您要部署至 Azure App Service 中的 Web 應用程式，也可用於自動化期間的變更另一個選項部署。 您可以輸入的設定，您想要在 Azure 中生效**設定**管理入口網站頁面 web 應用程式 索引標籤 (向下捲動至**應用程式設定**和**連接字串**章節)。 當您部署專案時，Azure 會自動套用所做的變更。 如需詳細資訊，請參閱 < [Windows Azure 網站： 應用程式字串與連接字串的運作](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx)。

## <a name="default-transformation-files"></a>預設轉換檔案

在**方案總管 中**，展開*Web.config*若要查看*Web.Debug.config*並*Web.Release.config*轉換檔案會建立兩個預設組建組態的預設值。

![Web.config_transform_files](web-config-transformations/_static/image1.png)

您可以建立自訂組建組態的轉換檔案，用滑鼠右鍵按一下 Web.config 檔案，然後選擇**新增設定轉換**從內容功能表。 本教學課程中，您不需要這麼做，和功能表選項會停用，因為您尚未建立任何自訂的組建組態。

稍後您將建立三個多個轉換檔案，每個測試、 預備和生產環境的發行設定檔。 典型的範例，因為這取決於目的地環境，您會處理在發行設定檔轉換檔中的設定是針對測試與生產環境不同的 WCF 端點。 您將建立發行設定檔轉換檔案中稍後的教學課程之後建立它們所採用的發行設定檔。

## <a name="disable-debug-mode"></a>停用偵錯模式

設定相依於組建組態，而不是目的地環境的範例`debug`屬性。 在發行組建中，您通常會想停用偵錯不論哪一個環境中，您要部署到。 因此，根據預設 Visual Studio 專案範本建立*Web.Release.config*轉換以移除的程式碼的檔案`debug`屬性從`compilation`項目。 以下是預設值*Web.Release.config*： 除了一些轉換程式碼範例標記為註解，包含在程式碼`compilation`移除的項目`debug`屬性：

[!code-xml[Main](web-config-transformations/samples/sample1.xml?highlight=18)]

`xdt:Transform="RemoveAttributes(debug)"`屬性會指定您想要`debug`屬性，以移除`system.web/compilation`中已部署的項目*Web.config*檔案。 這會在每次您部署的發行組建完成。

## <a name="limit-error-log-access-to-administrators"></a>限制存取系統管理員的錯誤記錄檔

如果沒有錯誤的應用程式執行時，應用程式會顯示一般錯誤頁面，以系統產生的錯誤 頁面中，取代，而且它會使用[Elmah NuGet 套件](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx)記錄和報告的錯誤。 `customErrors`應用程式中的項目*Web.config*檔案會指定錯誤頁面：

[!code-xml[Main](web-config-transformations/samples/sample2.xml)]

若要查看錯誤頁面，暫時變更`mode`屬性的`customErrors`"RemoteOnly"，為 [開啟]，然後執行應用程式從 Visual Studio 中的元素。 這類要求無效的 URL，而造成錯誤*Studentsxxx.aspx*。 而非 IIS 產生 「 資源無法找到 」 的錯誤頁面上，您會看到*GenericErrorPage.aspx*頁面。

![錯誤頁面](web-config-transformations/_static/image2.png)

若要查看錯誤記錄檔，取代 URL 中的所有項目之後使用的連接埠號碼*elmah.axd* (比方說， `http://localhost:51130/elmah.axd`) 然後按 Enter:

![ELMAH 頁面](web-config-transformations/_static/image3.png)

別忘了設定`customErrors`回"RemoteOnly"模式完成後的項目。

在您的開發電腦上就很方便地讓 [錯誤記錄] 頁面中，但會有安全性風險的生產環境中的免費存取。 為生產網站中，您想要新增授權規則，將錯誤記錄檔的存取限制為系統管理員，並確定，限制適用於您想在測試和預備也。 因此這是您想要實作每次部署發行組建裡，因此其所屬的另一個變更*Web.Release.config*檔案。

開啟*Web.Release.config*再加入新`location`項目結束之前，立即`configuration`標記，如下所示。

[!code-xml[Main](web-config-transformations/samples/sample3.xml?highlight=27-34)]

`Transform` "Insert"屬性值會導致這`location`成為同層級加入至任何現有的項目`location`中的項目*Web.config*檔案。 (有一個已經`location`項目，指定授權規則**更新信用額度**頁面。)

現在您可以預覽要確定您正確地編寫它的轉換。

在 **方案總管 中**，以滑鼠右鍵按一下*Web.Release.config* ，按一下 **預覽轉換**。

![預覽轉換功能表](web-config-transformations/_static/image4.png)

頁面隨即開啟，顯示您開發*Web.config*左邊和項目上的檔案已部署*Web.config*檔案會看起來像右側以反白顯示的變更。

![偵錯轉換的預覽](web-config-transformations/_static/image5.png)

![位置轉換預覽](web-config-transformations/_static/image6.png)

(在預覽中，您可能會發現不是您撰寫一些額外變更轉換： 這些通常牽涉到將不會影響功能的泛空白字元移除。)

當您測試網站，在部署後時，您也將測試確認授權規則有效。

> [!NOTE] 
> 
> **安全性注意事項**永遠不會公開在生產環境應用程式中，顯示錯誤詳細資料，或將該資訊儲存在公用位置。 若要探索的站台中的弱點，攻擊者可以使用資訊時發生錯誤。 如果您在自己的應用程式中使用 ELMAH，設定 ELMAH 安全性風險降到最低。 在本教學課程的 ELMAH 範例不應視為建議的設定。 例如，選擇以說明如何處理應用程式必須能夠在其中建立檔案的資料夾。 如需詳細資訊，請參閱 <<c0> [ 保護的 ELMAH 端點](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages)。


## <a name="a-setting-that-youll-handle-in-publish-profile-transformation-files"></a>您將會處理在設定發行設定檔轉換檔案

常見的案例是有*Web.config*檔案必須在您將部署到每個環境中不同的設定。 例如，呼叫 WCF 服務的應用程式可能需要在測試和生產環境不同的端點。 Contoso 大學應用程式也包含這類的設定。 此設定控制，告訴您哪一個環境，您會在中，例如開發、 測試或生產環境的站台的頁面上的可見指示。 設定值會決定應用程式是否將會附加"(Dev)"或"（測試） 」 的主標題中要*Site.Master*主版頁面：

![環境標記](web-config-transformations/_static/image7.png)

在預備環境或生產環境中執行應用程式，則會忽略環境指標。

Contoso 大學 web 網頁讀取的值中設定`appSettings`中*Web.config*檔案以判斷哪些應用程式正在執行的環境：

[!code-xml[Main](web-config-transformations/samples/sample4.xml)]

值應該是測試環境中，「 測試 」 和 「 生產 」 來預備和生產環境。

轉換檔案中的下列程式碼會實作這項轉換：

[!code-xml[Main](web-config-transformations/samples/sample5.xml)]

`xdt:Transform`屬性的值，"SetAttributes"表示這個轉換的目的是要變更屬性值中的現有項目*Web.config*檔案。 `xdt:Locator`屬性的值"Match(key)"表示要修改的項目是一個其`key`屬性相符項目`key`此處指定的屬性。 只有其他的屬性`add`項目是`value`，這將會變更的項目中已部署*Web.config*檔案。 程式碼如下所示的原因`value`的屬性`Environment``appSettings`會設為"Test"中的項目*Web.config*部署的檔案。

這項轉換屬於尚未建立發行設定檔轉換檔案。 您會建立並更新實作這項變更，當您建立的測試、 預備及生產環境的發行設定檔的轉換檔案。 您一下[部署至 IIS](deploying-to-iis.md)並[部署到生產環境](deploying-to-production.md)教學課程。

> [!NOTE]
> 因為此設定位在`<appSettings>`項目，您有另一個替代方式，在部署 Azure 應用程式服務，請參閱中的 Web 應用程式時，指定轉換[在 Azure 中的指定 Web.config 設定](#watransforms)稍早的本主題中。


## <a name="setting-connection-strings"></a>設定連接字串

雖然預設轉換檔包含的範例，示範如何更新連接字串，在大部分情況下您不需要設定連接字串的轉換，因為您可以在發行設定檔中指定連接字串。 您一下[部署至 IIS](deploying-to-iis.md)並[部署到生產環境](deploying-to-production.md)教學課程。

## <a name="summary"></a>總結

現在您已經儘可能地使用*Web.config*轉換之前建立的發行設定檔，以及您已了解所要部署的 Web.config 檔案中的預覽。

![位置轉換預覽](web-config-transformations/_static/image8.png)

在下列教學課程中，您將負責部署需要設定專案屬性的設定工作。

## <a name="more-information"></a>更多資訊

如需本教學課程所涵蓋的主題的詳細資訊，請參閱[若要變更目的 Web.config 檔案或 app.config 檔案中的設定，在部署期間使用 Web.config 轉換](https://go.microsoft.com/fwlink/p/?LinkId=282413#transforms)在 Web 部署的內容對應Visual Studio 及 ASP.NET。

> [!div class="step-by-step"]
> [上一頁](preparing-databases.md)
> [下一頁](project-properties.md)
