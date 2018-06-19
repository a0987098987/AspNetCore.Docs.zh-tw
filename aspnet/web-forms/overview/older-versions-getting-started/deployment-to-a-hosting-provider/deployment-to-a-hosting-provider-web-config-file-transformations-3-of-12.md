---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
title: 使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： Web.Config 檔案轉換為 12 3 |Microsoft 文件
author: tdykstra
description: 這一系列的教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式專案，其中包含 SQL Server Compact 資料庫使用視覺化 Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 2b0df3d9-450b-4ea6-b315-4c9650722cad
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
msc.type: authoredcontent
ms.openlocfilehash: 86eb74ca35e8804978127412e2276eeee9d615dc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888132"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-webconfig-file-transformations---3-of-12"></a>使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： Web.Config 檔案轉換為 12 3
====================
由[Tom Dykstra](https://github.com/tdykstra)

[下載入門專案](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> 這一系列的教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式專案，其中包含使用 Visual Studio 2012 RC 或 Visual Studio Express 2012 RC for Web 的 SQL Server Compact 資料庫。 如果您安裝 Web 發行更新，您也可以使用 Visual Studio 2010。 數列的簡介，請參閱[系列的第一個教學課程](deployment-to-a-hosting-provider-introduction-1-of-12.md)。
> 
> 顯示部署 Visual Studio 2012 RC 發行之後，引進的功能，示範如何將 SQL Server Compact 以外的 SQL Server 版本的部署和示範如何將部署至 Azure App Service Web 應用程式的教學課程，請參閱[ASP.NET Web 部署使用 Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md)。


## <a name="overview"></a>總覽

本教學課程會示範如何變更的程序自動化*Web.config*檔案時將它部署至不同目的地環境。 大部分的應用程式中有設定*Web.config*部署應用程式時都必須使用不同的檔案。 自動化程序可確保這些變更可防止您不必手動執行它們，每次您部署時，就可能發生冗長又容易出錯。

提示： 如果您收到錯誤訊息，或當您瀏覽教學課程，項目無法運作，請務必檢查[疑難排解頁面](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)。

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>與 Web 的 Web.config 轉換部署參數

有兩種方式變更的程序自動化*Web.config*檔案設定： [Web.config 轉換](https://msdn.microsoft.com/library/dd465326.aspx)和[Web Deploy 參數](https://msdn.microsoft.com/library/ff398068.aspx)。 A *Web.config*轉換檔案包含會指定如何變更的 XML 標記*Web.config*部署到檔案。 您可以指定不同的變更，針對特定組建組態，以及針對特定的發行設定檔。 預設建置組態偵錯和發行，而且您可以建立自訂的建置組態。 發行設定檔通常會對應到目的地環境。 (您將學習到有關發行設定檔中的[部署至 IIS 做為測試環境](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)教學課程。)

Web 部署參數可以用來指定各種不同的設定，必須在部署期間，包括設定中找到設定*Web.config*檔案。 當用來指定*Web.config*檔案變更，Web Deploy 參數都是更複雜的設定，但您不知道要部署之前設定的值時，它們很有用。 例如，在企業環境中，您可能會建立*部署套件*給予 IT 部門中的人員，若要安裝在生產環境中，以及該人員可以輸入連接字串或不這麼做的密碼了解。

本教學課程涵蓋的案例中，針對您知道要進行的一切*Web.config*檔案，因此您不需要使用 Web Deploy 參數。 您需要設定一些轉換，而有所不同，所使用的組建組態，某些則可使用的發行設定檔而有所不同。

## <a name="creating-transformation-files-for-publish-profiles"></a>建立轉換檔案的發行設定檔

在**方案總管 中**，依序展開*Web.config*查看*Web.Debug.config*和*Web.Release.config*轉換的檔案會建立兩個預設建置組態的預設值。

![Web.config_transform_files](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image1.png)

您可以建立自訂的建置組態的轉換檔案的 Web.config 檔案上按一下滑鼠右鍵，然後選擇**新增 Config 轉換**從內容功能表中，但本教學課程不需要執行此作業。

您需要兩個以上的轉換檔案，來設定部署目的地，而不是組建組態相關的變更。 設定這種典型的範例是不同的測試與實際執行的 WCF 端點。 在之後的教學課程，您會建立發行設定檔名為測試和生產環境中，因此您需要*Web.Test.config*檔案和*Web.Production.config*檔案。

轉換會繫結至發行設定檔的檔案都必須手動建立。 在**方案總管 中**，ContosoUniversity 專案上按一下滑鼠右鍵，然後選取**在 Windows 檔案總管 中開啟資料夾**。

![Open_folder_in_Windows_Explorer](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image2.png)

在**Windows 檔案總管**，選取*Web.Release.config*檔案中複製檔案，然後再貼上兩份。 重新命名這些複本*Web.Production.config*和*Web.Test.config*，請關閉然後**Windows 檔案總管**。

在**方案總管] 中**，按一下 [**重新整理**以查看新的檔案。

選取新的檔案、 按一下滑鼠右鍵，然後按一下**加入至專案**內容功能表中。

![包括在專案中的測試與實際的組態檔](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image3.png)

若要避免這些檔案部署，請選取在**方案總管 中**，然後在**屬性**視窗變更**建置動作**屬性從**內容**至**無**。 （組建組態為基礎的轉換檔案會自動防止部署。）

現在您已經可以進入*Web.config*轉換分割為*Web.config*轉換檔案。

## <a name="limiting-error-log-access-to-administrators"></a>限制系統管理員的存取錯誤記錄檔

如果應用程式執行期間發生錯誤時，應用程式會顯示一般錯誤頁面取代系統產生的錯誤 頁面上，而且它會使用[Elmah NuGet 封裝](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx)記錄和報告的錯誤。 `customErrors`中的項目*Web.config*檔案會指定錯誤頁面：

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample1.xml)]

若要查看錯誤頁面，暫時變更`mode`屬性`customErrors`"RemoteOnly"以 「 On 」 並執行應用程式，從 Visual Studio 中的項目。 這類要求 URL 無效，而造成錯誤*Studentsxxx.aspx*。 而不是 IIS 產生 「 找不到頁面 」 錯誤頁面，您會看到*GenericErrorPage.aspx*頁面。

[![Error_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image5.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image4.png)

若要查看錯誤記錄檔，取代 URL 中的所有項目與通訊埠編號後面*elmah.axd* (例如螢幕擷取畫面中，在`http://localhost:51130/elmah.axd`) 按下 Enter:

[![Elmah_log_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image7.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image6.png)

別忘了設定`customErrors`回"RemoteOnly"模式，當您完成時的項目。

在開發電腦上並方便允許 [錯誤記錄] 頁面中，但會有安全性風險的生產環境中可用的存取。 生產網站，您可以新增授權規則，以限制系統管理員只是為了存取錯誤記錄檔，藉由設定中的轉換*Web.Production.config*檔案。

開啟*Web.Production.config*再加入新`location`緊接之後開啟項目`configuration`標記，如下所示。 (請確定您只有新增`location`項目並不只用來提供的一些內容如下所示在周圍標記。)

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample2.xml?highlight=2-9)]

`Transform` 「 插入 」 屬性值會造成這個`location`會成為同層級新增到任何現有項目`location`中的項目*Web.config*檔案。 (已經有一個`location`項目，指定授權規則**更新信用額度**頁面。)當您測試生產網站部署之後時，您將測試確認這個授權規則有效。

您不需要限制存取測試環境中的錯誤記錄檔，所以您不需要加入此程式碼以*Web.Test.config*檔案。

> [!NOTE] 
> 
> **安全性注意事項**永遠不會公開在實際執行應用程式中，顯示錯誤詳細資料，或將該資訊儲存在公用位置。 攻擊者可以使用錯誤的資訊來找出站台中的弱點。 如果您使用您自己的應用程式中的 ELMAH，請務必調查的方法中的 ELMAH 可以設定為安全性風險降到最低。 在此教學課程中的 ELMAH 範例不應視為建議的設定。 這是為了說明如何處理應用程式必須能夠在其中建立檔案的資料夾已選擇範例。


## <a name="setting-an-environment-indicator"></a>設定環境標記

常見的案例是讓*Web.config*檔案必須在您將部署到每個環境中不同的設定。 例如，呼叫 WCF 服務的應用程式可能需要測試和實際執行環境中的不同端點。 Contoso 大學應用程式也包含這類的設定。 此設定會控制，告訴您的環境，您會在中，例如開發、 測試或實際執行的網站頁面上的可見指示。 設定值會決定應用程式是否將會附加 「 （開發） 」 或 「 （測試） 」 中主要的標題以*Site.Master*主版頁面：

[![Environment_indicator](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image9.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image8.png)

在生產環境中執行應用程式時，會省略環境指標。

Contoso 大學網頁讀取的設定中的值`appSettings`中*Web.config*檔案以判斷哪些應用程式正在執行中的環境：

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample3.xml)]

值應該是 「 測試 」 中的測試環境和"Prod"生產環境中。

開啟*Web.Production.config*並加入`appSettings`項目之開頭標記之前，立即`location`您先前加入的項目：

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample4.xml)]

`xdt:Transform`屬性的值，"SetAttributes"表示這項轉換的目的是要變更屬性值中的現有項目*Web.config*檔案。 `xdt:Locator`屬性的值"Match(key)"表示要修改的項目是指其`key`屬性相符項目`key`此處指定的屬性。 而只有另一個屬性的`add`項目是`value`，而這就是將變更中已部署*Web.config*檔案。 此程式碼會造成`value`屬性`Environment``appSettings`設為"Prod"的項目*Web.config*部署到生產環境的檔案。

接下來，將套用相同的變更*Web.Test.config*檔案中的，設定`value`至 「 測試 」，而不是"Prod"。 當您完成之後時，`appSettings`一節中*Web.Test.config*看起來像下列的範例：

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample5.xml)]

## <a name="disabling-debug-mode"></a>停用偵錯模式

在發行組建，您不想啟用偵錯不論何種環境中，您要部署到。 根據預設*Web.Release.config*與移除的程式碼會自動建立轉換檔案`debug`屬性從`compilation`項目：

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample6.xml)]

`Transform`屬性原因`debug`省略的屬性從已部署*Web.config*檔案，每當您將部署的發行組建。

此相同轉換是以測試與實際執行轉換檔案因為您藉由複製版本轉換檔案建立。 您不需要它那里重複，每個檔案開啟，移除**編譯**項目，並儲存並關閉每個檔案。

## <a name="setting-connection-strings"></a>設定連接字串

在大部分情況下您不需要設定連接字串轉換，因為您可以指定連接字串中的發行設定檔。 但有例外狀況時，您要部署的 SQL Server Compact 資料庫，且您使用 Entity Framework Code First 移轉來更新目的地伺服器上的資料庫。 此案例中，您必須指定將在伺服器用來更新資料庫結構描述的其他連接字串。 若要設定此轉換，加入**&lt;connectionStrings&gt;** 緊接之後開啟項目**&lt;組態&gt;** 中兩者都標記*Web.Test.config*和*Web.Production.config*轉換檔案：

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample7.xml)]

`Transform`屬性會指定此連接字串將會加入至*connectionStrings*中已部署的項目*Web.config*檔案。 (發行程序這個額外的連接字串會自動建立，如果它不存在，但預設**providerName**屬性會設成`System.Data.SqlClient`，這無法運作不適用於 SQL Server Compact。 藉由手動加入連接字串，您將部署程序建立錯誤的提供者名稱的連接字串項目。）

現在您已指定所有*Web.config* Contoso 大學應用程式來測試和實際執行部署所需的轉換。 下列教學課程中，您將處理的部署需要設定專案屬性的設定工作。

## <a name="more-information"></a>更多資訊

如需本教學課程所涵蓋之主題的詳細資訊，請參閱中的 Web.config 轉換案例[ASP.NET 部署內容地圖](https://msdn.microsoft.com/library/bb386521.aspx)。

> [!div class="step-by-step"]
> [上一頁](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
> [下一頁](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
