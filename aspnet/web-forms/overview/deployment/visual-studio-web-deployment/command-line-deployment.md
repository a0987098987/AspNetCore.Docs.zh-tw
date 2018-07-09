---
uid: web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
title: 使用 Visual Studio 的 ASP.NET Web 部署： 命令列部署 |Microsoft Docs
author: tdykstra
description: 本系列教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web Apps 或協力廠商裝載提供者，使用...
ms.author: aspnetcontent
ms.date: 02/15/2013
ms.assetid: 82b8dea0-f062-4ee4-8784-3ffa30fbb1ca
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
msc.type: authoredcontent
ms.openlocfilehash: f079beabccd253ff13c19d3192181ddbdf00b3bf
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830963"
---
<a name="aspnet-web-deployment-using-visual-studio-command-line-deployment"></a>使用 Visual Studio 的 ASP.NET Web 部署： 命令列部署
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

[下載入門專案](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> 本系列教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web Apps 或協力廠商裝載提供者，使用 Visual Studio 2012 或 Visual Studio 2010。 這個系列的相關資訊，請參閱[系列的第一個教學課程](introduction.md)。


## <a name="overview"></a>總覽

本教學課程會示範如何叫用 Visual Studio web 發行管線，從命令列。 這是適用於您想要的案例[部署程序自動化](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md)而不是以手動方式在 Visual Studio 中進行，通常藉由使用[原始程式碼版本控制系統](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md)。

## <a name="make-a-change-to-deploy"></a>若要部署變更

目前 [關於] 頁面會顯示範本程式碼。

![關於使用範本程式碼的頁面](command-line-deployment/_static/image1.png)

您將取代，以顯示學生註冊摘要的程式碼。

開啟*about.aspx 的網頁*頁面上，刪除所有的內部標記`MainContent``Content`項目，並插入其位置中的下列標記：

[!code-aspx[Main](command-line-deployment/samples/sample1.aspx)]

執行專案，並選取**關於**頁面。

![About 頁面](command-line-deployment/_static/image2.png)

## <a name="deploy-to-test-by-using-the-command-line"></a>使用命令列部署至測試

您不會部署另一個資料庫變更，因此 aspnet ContosoUniversity 資料庫停用 dbDacFx 資料庫部署。 開啟**發佈 Web**精靈，並在每個三個發行設定檔，清除**更新資料庫**核取方塊**設定** 索引標籤。

在 Windows 8 [開始] 頁面中，搜尋**適用於 VS2012 的開發人員命令提示字元**。

以滑鼠右鍵按一下圖示**適用於 VS2012 的開發人員命令提示字元**然後按一下**系統管理員身分執行**。

輸入下列命令在命令提示字元中，方案檔的路徑取代為您的方案檔案的路徑：

[!code-console[Main](command-line-deployment/samples/sample2.cmd)]

MSBuild 會建置方案，並將它部署到測試環境。

![命令列輸出](command-line-deployment/_static/image3.png)

開啟瀏覽器並移至`http://localhost/ContosoUniversity`，然後按一下**關於**頁面，確認部署是否成功。

如果您尚未建立任何學生在測試中，您會看到空白頁面底下**學生主體統計資料**標題。 移至**學生**頁面上，按一下**新增的學生**，並新增一些學生，然後再返回**關於**頁面，以查看學生統計資料。

![關於在測試環境中的頁面](command-line-deployment/_static/image4.png)

## <a name="key-command-line-options"></a>索引鍵的命令列選項

您輸入的命令傳遞至 MSBuild 的方案檔路徑和兩個屬性：

[!code-console[Main](command-line-deployment/samples/sample3.cmd)]

### <a name="deploying-the-solution-versus-deploying-individual-projects"></a>部署個別專案與方案部署

指定方案檔導致要建置方案中的所有專案。 如果您在方案中有多個 web 專案，適用於下列 MSBuild 行為：

- 您在命令列指定的屬性會傳遞至每個專案中。 因此，每個 web 專案必須具有您所指定名稱的發行設定檔。 如果您指定`/p:PublishProfile=Test`，每個 web 專案必須具有名為的發行設定檔*測試*。
- 當另一個甚至不會在建置時，可能會成功發佈一個專案。 如需詳細資訊，請參閱 stackoverflow 執行緒[MSBuild 因兩個套件](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages)。

如果您指定個別的專案，而不是方案時，您必須新增參數來指定 Visual Studio 版本。 如果您使用 Visual Studio 2012 命令列會類似下列範例：

[!code-console[Main](command-line-deployment/samples/sample4.cmd?highlight=1)]

Visual Studio 2010 的版本號碼為 10.0。 如需詳細資訊，請參閱 < [Visual Studio 專案相容性和 VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) Sayed Hashimi 部落格上。

### <a name="specifying-the-publish-profile"></a>指定發行設定檔

依名稱或完整路徑，您可以指定發行設定檔 *.pubxml*檔案，如下列範例所示：

[!code-console[Main](command-line-deployment/samples/sample5.cmd?highlight=1)]

### <a name="web-publish-methods-supported-for-command-line-publishing"></a>Web 發行支援的命令列發佈方法

三個發行方法支援命令列發佈：

- `MSDeploy` -使用 Web Deploy 發行。
- `Package` -建立 Web 部署套件發佈。 您必須將封裝分開安裝 MSBuild 命令來建立它。
- `FileSystem` -發行將檔案複製到指定的資料夾。

### <a name="specifying-the-build-configuration-and-platform"></a>指定組建組態與平台

在 Visual Studio 或命令列上，必須設定組建組態與平台。 發行設定檔包含名為的屬性`LastUsedBuildConfiguration`和`LastUsedPlatform`，但您無法設定這些屬性，以判斷專案的建置方式。 如需詳細資訊，請參閱 < [MSBuild： 如何設定組態屬性](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx)Sayed Hashimi 部落格上。

## <a name="deploy-to-staging"></a>部署至預備環境

若要部署至 Azure，您必須新增至命令列的密碼。 如果您在 Visual Studio 中的發行設定檔中儲存的密碼，它是加密形式儲存在您 *.pubxml.user*檔案。 不，由 MSBuild 來存取該檔案，當您執行命令列部署，因此您必須傳入命令列參數中的密碼。

1. 複製您需要從密碼 *.publishsettings*稍早的預備網站所下載檔案。 密碼是值`userPWD`屬性的 Web Deploy`publishProfile`項目。

    ![Web 部署密碼](command-line-deployment/_static/image5.png)
2. 在 Windows 8 [開始] 頁面中，搜尋**適用於 VS2012 的開發人員命令提示字元**，然後按一下圖示以開啟命令提示字元。 （您不需要其系統管理員身分開啟此時因為您不本機電腦上部署至 IIS）。
3. 輸入下列命令在命令提示字元，取代您的方案檔和您的密碼與密碼的路徑中的方案檔的路徑：

    [!code-console[Main](command-line-deployment/samples/sample6.cmd)]

    請注意，此命令列包含額外的參數： `/p:AllowUntrustedCertificate=true`。 在本教學課程中所寫入，`AllowUntrustedCertificate`從命令列發佈至 Azure 時，就必須設定屬性。 釋放這個 bug 的修正後，您不需要該參數。
4. 開啟瀏覽器並移至您的預備網站的 URL，然後按**關於**頁面，確認部署是否成功。

    如同您前文的測試環境中，您可能必須建立一些統計資料，讓學生**關於**頁面。

## <a name="deploy-to-production"></a>部署到生產環境

部署至生產環境的程序是針對預備環境的程序類似。

1. 複製您需要從密碼 *.publishsettings*稍早針對實際執行的網站所下載檔案。
2. 開啟**適用於 VS2012 的開發人員命令提示字元**。
3. 輸入下列命令在命令提示字元，取代您的方案檔和您的密碼與密碼的路徑中的方案檔的路徑：

    [!code-console[Main](command-line-deployment/samples/sample7.cmd)]

    對於實際生產站台，也是資料庫變更，您會通常複製*應用程式\_offline.htm*檔案到站台部署之前，並成功部署之後刪除它。
4. 開啟瀏覽器並移至您的預備網站的 URL，然後按**關於**頁面，確認部署是否成功。

## <a name="summary"></a>總結

您現在已使用命令列部署應用程式更新。

![關於在測試環境中的頁面](command-line-deployment/_static/image6.png)

在下一個教學課程中，您會看到範例如何擴充網站的發行管線。 此範例會示範如何部署不包含在專案中的檔案。

> [!div class="step-by-step"]
> [上一頁](deploying-a-database-update.md)
> [下一頁](deploying-extra-files.md)
