---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
title: 使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： Web.Config 檔案轉換為 12 3 |Microsoft Docs
author: tdykstra
description: 這一系列的教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式專案，其中包含 SQL Server Compact 資料庫，使用 Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 2b0df3d9-450b-4ea6-b315-4c9650722cad
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
msc.type: authoredcontent
ms.openlocfilehash: b9f8213920e9e69885021666143612ce7c542b69
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388072"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-webconfig-file-transformations---3-of-12"></a>使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： Web.Config 檔案轉換為 3 / 12
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

[下載入門專案](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> 這一系列的教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式專案使用 Visual Studio 2012 RC 或 Visual Studio Express 2012 RC for Web 包含 SQL Server Compact 資料庫。 如果您安裝 Web Publish Update，您也可以使用 Visual Studio 2010。 在數列的簡介，請參閱[系列的第一個教學課程](deployment-to-a-hosting-provider-introduction-1-of-12.md)。
> 
> 顯示 Visual Studio 2012 RC 版本之後引入的部署功能，示範如何部署 SQL Server Compact，以外的 SQL Server 版本，並示範如何部署至 Azure App Service Web Apps 的教學課程，請參閱[ASP.NET Web 部署使用 Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md)。


## <a name="overview"></a>總覽

本教學課程會示範如何將變更的程序自動化*Web.config*檔案時將它部署至不同目的地環境。 大部分的應用程式中有設定*Web.config*應用程式部署時必須是不同的檔案。 自動化程序可確保這些變更讓您不必手動執行它們，每次部署，這是很麻煩又容易出錯。

提醒： 如果您收到錯誤訊息，或當您瀏覽本教學課程，項目無法運作，請務必檢查[疑難排解頁面](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)。

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>與 Web 的 Web.config 轉換部署參數

有兩種方式可將變更的程序自動化*Web.config*檔案設定： [Web.config 轉換](https://msdn.microsoft.com/library/dd465326.aspx)並[Web Deploy 參數](https://msdn.microsoft.com/library/ff398068.aspx)。 A *Web.config*轉換檔案包含指定如何變更的 XML 標記*Web.config*時部署檔案。 您可以指定不同的變更，針對特定組建組態，以及針對特定發行設定檔。 預設組建組態偵錯和發行，而且您可以建立自訂組建組態。 發行設定檔通常會對應到目的地環境。 (您將了解發行中的設定檔[部署到 IIS 作為測試環境](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)教學課程。)

Web 部署參數可用來指定許多不同類型的設定必須設定在部署期間，包括設定中找到*Web.config*檔案。 當用來指定*Web.config*檔案變更 Web 部署參數是更複雜的設定，但其適用時您不知道您將部署之前設定的值。 例如，在企業環境中，您可能會建立*部署套件*給予 IT 部門的人員，將安裝在生產環境，以及該人員具有能夠輸入連接字串或不這麼做的密碼了解。

針對本教學課程涵蓋的案例，您知道的所有項目完成才能*Web.config*檔案，因此您不需要使用 Web Deploy 的參數。 您需要設定一些轉換，而有所不同，組建組態和一些使用發行設定檔而有所不同。

## <a name="creating-transformation-files-for-publish-profiles"></a>建立發行設定檔的轉換檔

在**方案總管 中**，展開*Web.config*若要查看*Web.Debug.config*並*Web.Release.config*轉換檔案會建立兩個預設組建組態的預設值。

![Web.config_transform_files](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image1.png)

您可以建立自訂組建組態的轉換檔案，用滑鼠右鍵按一下 Web.config 檔案，然後選擇**新增設定轉換**從操作功能表中，但本教學課程中您不需要這麼做。

您需要另外兩個轉換檔案，來設定部署目的地，而不是組建組態相關的變更。 這種設定的典型範例是針對測試與生產環境不同的 WCF 端點。 發行名為測試和生產環境中，因此您需要的設定檔中，您會建立稍後的教學課程*Web.Test.config*檔案並*Web.Production.config*檔案。

會繫結至發行設定檔的轉換檔必須以手動方式建立。 在 **方案總管**，以滑鼠右鍵按一下 ContosoUniversity 專案，然後選取**在 Windows 檔案總管中開啟資料夾**。

![Open_folder_in_Windows_Explorer](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image2.png)

在  **Windows 檔案總管**，選取*Web.Release.config*檔案，複製檔案，然後再貼上兩個副本。 重新命名這些複本*Web.Production.config*並*Web.Test.config*，然後關閉**Windows 檔案總管**。

在 **方案總管**，按一下**重新整理**以查看新的檔案。

選取新的檔案、 按一下滑鼠右鍵，然後按一下 **加入至專案**操作功能表中。

![在專案中包含測試和生產環境的組態檔](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image3.png)

若要避免部署這些檔案，選取 在**方案總管**，然後在**屬性**視窗中變更**建置動作**屬性從  **內容**要**無**。 （根據組建組態轉換檔會自動將無法部署。）

您現在已準備好輸入*Web.config*轉換成*Web.config*轉換檔案。

## <a name="limiting-error-log-access-to-administrators"></a>限制存取系統管理員的錯誤記錄檔

如果沒有錯誤的應用程式執行時，應用程式會顯示一般錯誤頁面，以系統產生的錯誤 頁面中，取代，而且它會使用[Elmah NuGet 套件](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx)記錄和報告的錯誤。 `customErrors`中的項目*Web.config*檔案會指定錯誤頁面：

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample1.xml)]

若要查看錯誤頁面，暫時變更`mode`屬性的`customErrors`"RemoteOnly"，為 [開啟]，然後執行應用程式從 Visual Studio 中的元素。 這類要求無效的 URL，而造成錯誤*Studentsxxx.aspx*。 而不是 IIS 產生 「 找不到頁面 」 錯誤頁面，您會看到*GenericErrorPage.aspx*頁面。

[![Error_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image5.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image4.png)

若要查看錯誤記錄檔，取代 URL 中的所有項目之後使用的連接埠號碼*elmah.axd* (例如在螢幕擷取畫面， `http://localhost:51130/elmah.axd`) 然後按 Enter:

[![Elmah_log_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image7.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image6.png)

別忘了設定`customErrors`回"RemoteOnly"模式完成後的項目。

在您的開發電腦上就很方便地讓 [錯誤記錄] 頁面中，但會有安全性風險的生產環境中的免費存取。 為生產網站中，您可以新增授權規則以限制存取錯誤記錄檔只是系統管理員所設定中的轉型*Web.Production.config*檔案。

開啟*Web.Production.config*再加入新`location`項目開頭之後立即`configuration`標記，如下所示。 (請務必只新增`location`項目並不只是要提供一些內容如下所示周圍標記。)

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample2.xml?highlight=2-9)]

`Transform` "Insert"屬性值會導致這`location`成為同層級加入至任何現有的項目`location`中的項目*Web.config*檔案。 (有一個已經`location`項目，指定授權規則**更新信用額度**頁面。)當您在部署後測試生產網站時，您會測試以確認這個授權規則有效。

您不需要限制在測試環境中，存取錯誤記錄檔，讓您不必新增下列程式碼*Web.Test.config*檔案。

> [!NOTE] 
> 
> **安全性注意事項**永遠不會公開在生產環境應用程式中，顯示錯誤詳細資料，或將該資訊儲存在公用位置。 若要探索的站台中的弱點，攻擊者可以使用資訊時發生錯誤。 如果您在自己的應用程式中使用 ELMAH，請務必調查的如何在其中 ELMAH 可以設定為安全性風險降到最低。 在本教學課程的 ELMAH 範例不應視為建議的設定。 例如，選擇以說明如何處理應用程式必須能夠在其中建立檔案的資料夾。


## <a name="setting-an-environment-indicator"></a>設定環境指標

常見的案例是有*Web.config*檔案必須在您將部署到每個環境中不同的設定。 例如，呼叫 WCF 服務的應用程式可能需要在測試和生產環境不同的端點。 Contoso 大學應用程式也包含這類的設定。 此設定控制，告訴您哪一個環境，您會在中，例如開發、 測試或生產環境的站台的頁面上的可見指示。 設定值會決定應用程式是否將會附加"(Dev)"或"（測試） 」 的主標題中要*Site.Master*主版頁面：

[![Environment_indicator](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image9.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image8.png)

在生產環境中執行應用程式時，則會忽略環境指標。

Contoso 大學 web 網頁讀取的值中設定`appSettings`中*Web.config*檔案以判斷哪些應用程式正在執行的環境：

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample3.xml)]

值應該是"Test"的測試環境中和 「 生產 」 在生產環境中。

開啟*Web.Production.config*並加入`appSettings`項目的開頭標記之前立即`location`您先前加入的項目：

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample4.xml)]

`xdt:Transform`屬性的值，"SetAttributes"表示這個轉換的目的是要變更屬性值中的現有項目*Web.config*檔案。 `xdt:Locator`屬性的值"Match(key)"表示要修改的項目是一個其`key`屬性相符項目`key`此處指定的屬性。 只有其他的屬性`add`項目是`value`，這將會變更的項目中已部署*Web.config*檔案。 此程式碼會造成`value`的屬性`Environment``appSettings`會設為"Prod"中的項目*Web.config*部署到生產環境的檔案。

接下來，將套用至相同的變更*Web.Test.config*檔案，除了組`value`以"Test"，而不是"Prod"。 當您完成時，`appSettings`一節*Web.Test.config*看起來如下列範例所示：

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample5.xml)]

## <a name="disabling-debug-mode"></a>停用偵錯模式

發行組建，您不想啟用偵錯不論哪一個環境中，您要部署到。 依預設*Web.Release.config*轉換檔會自動建立與移除的程式碼`debug`屬性從`compilation`項目：

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample6.xml)]

`Transform`屬性的原因`debug`省略屬性從已部署*Web.config*檔案每當您部署的發行組建。

這個相同的轉換是在測試和生產階段轉換檔案因為您藉由複製版本轉換檔建立。 您不需要它那里重複的每個檔案開啟、 移除**編譯**項目，並儲存並關閉每個檔案。

## <a name="setting-connection-strings"></a>設定連接字串

在大部分情況下您不需要設定連接字串的轉換，因為您可以在發行設定檔中指定連接字串。 但沒有例外狀況時，您要部署的 SQL Server Compact 資料庫，而且您使用 Entity Framework Code First 移轉來更新目的地伺服器上的資料庫。 此案例中，您必須指定將用於在伺服器更新資料庫結構描述的其他連接字串。 若要設定此轉換，將**&lt;connectionStrings&gt;** 項目開頭之後立即**&lt;configuration&gt;** 中的標記*Web.Test.config*並*Web.Production.config*轉換檔案：

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample7.xml)]

`Transform`屬性會指定此連接字串將會新增至*connectionStrings*中已部署的項目*Web.config*檔案。 (在發行程序此額外的連接字串會自動建立，如果不存在，但預設**providerName**屬性會設成`System.Data.SqlClient`，不適用於 SQL Server Compact 運作不到其中。 藉由手動新增連接字串，您防止部署程序建立的連接字串項目具有錯誤的提供者名稱。）

您現在已指定的所有*Web.config*部署 Contoso 大學應用程式來測試和生產環境所需的轉換。 在下列教學課程中，您將負責部署需要設定專案屬性的設定工作。

## <a name="more-information"></a>更多資訊

如需本教學課程所涵蓋的主題的詳細資訊，請參閱中的 Web.config 轉換案例[ASP.NET 部署內容對應](https://msdn.microsoft.com/library/bb386521.aspx)。

> [!div class="step-by-step"]
> [上一頁](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
> [下一頁](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
