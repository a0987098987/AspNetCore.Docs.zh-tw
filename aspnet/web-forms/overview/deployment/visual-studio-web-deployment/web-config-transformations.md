---
uid: web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
title: "使用 Visual Studio 的 ASP.NET Web 部署： Web.config 檔案轉換 |Microsoft 文件"
author: tdykstra
description: "此教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web 應用程式或協力廠商裝載提供者，使用..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 5a2a927b-14cb-40bc-867a-f0680f9febd7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
msc.type: authoredcontent
ms.openlocfilehash: a88d8f35c770b362b74f787fee2c60a7577bccb2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-deployment-using-visual-studio-webconfig-file-transformations"></a>使用 Visual Studio 的 ASP.NET Web 部署： Web.config 檔案轉換
====================
由[Tom Dykstra](https://github.com/tdykstra)

[下載入門專案](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> 此教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web 應用程式或協力廠商裝載提供者，使用 Visual Studio 2012 或 Visual Studio 2010。 數列的相關資訊，請參閱[系列的第一個教學課程](introduction.md)。


## <a name="overview"></a>概觀

本教學課程會示範如何變更的程序自動化*Web.config*檔案時將它部署至不同目的地環境。 大部分的應用程式中有設定*Web.config*部署應用程式時都必須使用不同的檔案。 自動化程序可確保這些變更可防止您不必手動執行它們，每次您部署時，就可能發生冗長又容易出錯。

提示： 如果您收到錯誤訊息，或當您瀏覽教學課程，項目無法運作，請務必檢查[疑難排解頁面](troubleshooting.md)。

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Web Deploy 參數與 Web.config 轉換

有兩種方式變更的程序自動化*Web.config*檔案設定： [Web.config 轉換](https://msdn.microsoft.com/en-us/library/dd465326.aspx)和[Web Deploy 參數](https://msdn.microsoft.com/en-us/library/ff398068.aspx)。 A *Web.config*轉換檔案包含會指定如何變更的 XML 標記*Web.config*部署到檔案。 您可以指定不同的變更，針對特定組建組態，以及針對特定的發行設定檔。 預設建置組態偵錯和發行，而且您可以建立自訂的建置組態。 發行設定檔通常會對應到目的地環境。 (您將學習到有關發行設定檔中的[部署至 IIS 做為測試環境](deploying-to-iis.md)教學課程。)

Web 部署參數可以用來指定各種不同的設定，必須在部署期間，包括設定中找到設定*Web.config*檔案。 當用來指定*Web.config*檔案變更，Web Deploy 參數都是更複雜的設定，但您不知道要部署之前設定的值時，它們很有用。 例如，在企業環境中，您可能會建立*部署套件*給予 IT 部門中的人員，若要安裝在生產環境中，以及該人員可以輸入連接字串或不這麼做的密碼了解。

此教學課程系列涵蓋的案例中，您事先知道，為了的一切*Web.config*檔案，因此您不需要使用 Web Deploy 參數。 您需要設定一些轉換，而有所不同，所使用的組建組態，某些則可使用的發行設定檔而有所不同。

<a id="watransforms"></a>

## <a name="specifying-webconfig-settings-in-azure"></a>在 Azure 中指定 Web.config 設定

如果*Web.config*您想要變更的檔案設定位於`<connectionStrings>`或`<appSettings>`項目，而且如果您要部署至 Azure App Service 中的 Web 應用程式，也可自動化期間變更的另一個選項部署。 您可以輸入您想要在 Azure 中才會生效設定**設定**管理入口網站頁面，您的 web 應用程式 索引標籤 (向下捲動至**應用程式設定**和**連接字串**區段)。 當您部署專案時，Azure 會自動套用的變更。 如需詳細資訊，請參閱[Windows Azure Web Sites： 如何應用程式字串與連接字串工作](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx)。

## <a name="default-transformation-files"></a>預設轉換檔案

在**方案總管 中**，依序展開*Web.config*查看*Web.Debug.config*和*Web.Release.config*轉換的檔案會建立兩個預設建置組態的預設值。

![Web.config_transform_files](web-config-transformations/_static/image1.png)

您可以建立自訂的建置組態的轉換檔案的 Web.config 檔案上按一下滑鼠右鍵，然後選擇**新增 Config 轉換**從內容功能表。 此教學課程中，您不需要這麼做，和功能表選項會停用，因為您尚未建立任何自訂的建置組態。

稍後您將建立三個以上轉換的檔案，每個測試、 暫存、 和生產的發行設定檔。 典型的範例，因為它相依於目的地環境一樣處理中的發行設定檔轉換檔案的設定是不同的測試與實際執行的 WCF 端點。 您要建立發行設定檔轉換之後的教學課程中建立會移至與發行設定檔之後。

## <a name="disable-debug-mode"></a>停用偵錯模式

設定相依於組建組態，而不是目的地環境的範例`debug`屬性。 在發行組建，您通常會想停用偵錯不論何種環境中，您要部署到。 因此，根據預設 Visual Studio 專案範本建立*Web.Release.config*檔案中移除的程式碼轉換`debug`屬性從`compilation`項目。 以下是預設*Web.Release.config*： 標記為註解的一些範例轉換程式碼，除了它還包含中的程式碼`compilation`移除項目`debug`屬性：

[!code-xml[Main](web-config-transformations/samples/sample1.xml?highlight=18)]

`xdt:Transform="RemoveAttributes(debug)"`屬性會指定您想`debug`屬性，以移除`system.web/compilation`中已部署的項目*Web.config*檔案。 這會在每次部署發行的組建完成。

## <a name="limit-error-log-access-to-administrators"></a>限制系統管理員的存取錯誤記錄檔

如果應用程式執行期間發生錯誤時，應用程式會顯示一般錯誤頁面取代系統產生的錯誤 頁面上，而且它會使用[Elmah NuGet 封裝](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx)記錄和報告的錯誤。 `customErrors`應用程式中的項目*Web.config*檔案會指定錯誤頁面：

[!code-xml[Main](web-config-transformations/samples/sample2.xml)]

若要查看錯誤頁面，暫時變更`mode`屬性`customErrors`"RemoteOnly"以 「 On 」 並執行應用程式，從 Visual Studio 中的項目。 這類要求 URL 無效，而造成錯誤*Studentsxxx.aspx*。 而非 IIS 產生"找不資源 」 的錯誤頁面上，您會看到*GenericErrorPage.aspx*頁面。

![錯誤頁面](web-config-transformations/_static/image2.png)

若要查看錯誤記錄檔，取代 URL 中的所有項目與通訊埠編號後面*elmah.axd* (例如， `http://localhost:51130/elmah.axd`) 按下 Enter:

![ELMAH 頁面](web-config-transformations/_static/image3.png)

別忘了設定`customErrors`回"RemoteOnly"模式，當您完成時的項目。

在開發電腦上並方便允許 [錯誤記錄] 頁面中，但會有安全性風險的生產環境中可用的存取。 將生產站台，您想要新增授權規則，以限制存取錯誤記錄檔給系統管理員，並確定這項限制適用於您想測試和暫存也中。 因此這是您想要實作每次部署發行的組建，因此它屬於另一項變更*Web.Release.config*檔案。

開啟*Web.Release.config*再加入新`location`項目結束之前立即`configuration`標記，如下所示。

[!code-xml[Main](web-config-transformations/samples/sample3.xml?highlight=27-34)]

`Transform` 「 插入 」 屬性值會造成這個`location`會成為同層級新增到任何現有項目`location`中的項目*Web.config*檔案。 (已經有一個`location`項目，指定授權規則**更新信用額度**頁面。)

現在您可以預覽要確定您正確編碼它的轉換。

在**方案總管 中**，以滑鼠右鍵按一下*Web.Release.config*按一下**預覽轉換**。

![預覽轉換功能表](web-config-transformations/_static/image4.png)

頁面會隨即開啟，顯示您開發*Web.config*在左方和功能檔案已部署*Web.config*檔案看起來會像在右側，變更反白顯示。

![偵錯轉換預覽](web-config-transformations/_static/image5.png)

![位置轉換預覽](web-config-transformations/_static/image6.png)

(在預覽中，您可能會發現自己撰寫的某些其他變更轉換的： 這些通常並不會影響功能的泛空白字元移除。)

當您測試網站部署之後時，您也將測試確認授權規則有效。

> [!NOTE] 
> 
> **安全性注意事項**永遠不會公開在實際執行應用程式中，顯示錯誤詳細資料，或將該資訊儲存在公用位置。 攻擊者可以使用錯誤的資訊來找出站台中的弱點。 如果您使用您自己的應用程式中的 ELMAH，設定 ELMAH 安全性風險降到最低。 在此教學課程中的 ELMAH 範例不應視為建議的設定。 這是為了說明如何處理應用程式必須能夠在其中建立檔案的資料夾已選擇範例。 如需詳細資訊，請參閱[保護 ELMAH 端點](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages)。


## <a name="a-setting-that-youll-handle-in-publish-profile-transformation-files"></a>您會在中處理的設定發行設定檔轉換檔案

常見的案例是讓*Web.config*檔案必須在您將部署到每個環境中不同的設定。 例如，呼叫 WCF 服務的應用程式可能需要測試和實際執行環境中的不同端點。 Contoso 大學應用程式也包含這類的設定。 此設定會控制，告訴您的環境，您會在中，例如開發、 測試或實際執行的網站頁面上的可見指示。 設定值會決定應用程式是否將會附加 「 （開發） 」 或 「 （測試） 」 中主要的標題以*Site.Master*主版頁面：

![環境標記](web-config-transformations/_static/image7.png)

當預備或生產環境中執行應用程式時，會省略環境指標。

Contoso 大學網頁讀取的設定中的值`appSettings`中*Web.config*檔案以判斷哪些應用程式正在執行中的環境：

[!code-xml[Main](web-config-transformations/samples/sample4.xml)]

值應該是測試環境中，「 測試 」，而且"生產環境 」 針對預備和生產環境。

轉換檔案中的下列程式碼會實作這項轉換：

[!code-xml[Main](web-config-transformations/samples/sample5.xml)]

`xdt:Transform`屬性的值，"SetAttributes"表示這項轉換的目的是要變更屬性值中的現有項目*Web.config*檔案。 `xdt:Locator`屬性的值"Match(key)"表示要修改的項目是指其`key`屬性相符項目`key`此處指定的屬性。 而只有另一個屬性的`add`項目是`value`，而這就是將變更中已部署*Web.config*檔案。 程式碼如下所示原因`value`屬性`Environment``appSettings`設為 「 測試 」 的項目*Web.config*部署的檔案。

您尚未建立發行設定檔轉換檔案屬於這個轉換。 您將建立和更新您建立的測試、 預備及生產環境的發行設定檔時，實作這項變更轉換檔案。 您將會執行，[部署到 IIS](deploying-to-iis.md)和[部署到生產環境](deploying-to-production.md)教學課程。

> [!NOTE]
> 在此設定，所以`<appSettings>`項目，您有另一個替代方式，針對您要部署至 Azure 應用程式服務，請參閱中的 Web 應用程式時，指定轉換[在 Azure 中的指定 Web.config 設定](#watransforms)前面本主題中。


## <a name="setting-connection-strings"></a>設定連接字串

雖然預設轉換檔案中包含的範例會示範如何更新連接字串，在大部分情況下您不需要設定連接字串轉換，因為您可以指定連接字串中的發行設定檔。 您將會執行，[部署到 IIS](deploying-to-iis.md)和[部署到生產環境](deploying-to-production.md)教學課程。

## <a name="summary"></a>總結

現在您已經盡您可以使用*Web.config*之前建立的發行設定檔，以及您所見的已部署 Web.config 檔案中所要預覽的轉換。

![位置轉換預覽](web-config-transformations/_static/image8.png)

下列教學課程中，您將處理的部署需要設定專案屬性的設定工作。

## <a name="more-information"></a>更多資訊

如需本教學課程所涵蓋之主題的詳細資訊，請參閱[變更目的地 Web.config 檔或 app.config 檔案中的設定，在部署期間使用 Web.config 轉換](https://go.microsoft.com/fwlink/p/?LinkId=282413#transforms)中 Web 部署的內容對應Visual Studio 和 ASP.NET。

>[!div class="step-by-step"]
[上一頁](preparing-databases.md)
[下一頁](project-properties.md)
