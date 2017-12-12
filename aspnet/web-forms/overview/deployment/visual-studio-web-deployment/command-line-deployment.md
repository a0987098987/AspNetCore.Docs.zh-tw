---
uid: web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
title: "使用 Visual Studio 的 ASP.NET Web 部署： 命令列部署 |Microsoft 文件"
author: tdykstra
description: "此教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web 應用程式或協力廠商裝載提供者，使用..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 82b8dea0-f062-4ee4-8784-3ffa30fbb1ca
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
msc.type: authoredcontent
ms.openlocfilehash: 8446b3fc05e3ef4a5a30c753c989252fd7f1a56f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-deployment-using-visual-studio-command-line-deployment"></a>使用 Visual Studio 的 ASP.NET Web 部署： 命令列部署
====================
由[Tom Dykstra](https://github.com/tdykstra)

[下載入門專案](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> 此教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web 應用程式或協力廠商裝載提供者，使用 Visual Studio 2012 或 Visual Studio 2010。 數列的相關資訊，請參閱[系列的第一個教學課程](introduction.md)。


## <a name="overview"></a>概觀

本教學課程會示範如何叫用 Visual Studio web 發行管線就會從命令列。 這是適用於您想要的案例[自動化部署程序](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md)而不以手動方式在 Visual Studio 中進行，通常使用[原始程式碼版本控制系統](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md)。

## <a name="make-a-change-to-deploy"></a>若要部署變更

目前 [關於] 頁面會顯示範本程式碼。

![有關頁面範本程式碼](command-line-deployment/_static/image1.png)

您會取代所顯示的學生註冊摘要的程式碼。

開啟*about.aspx 的網頁*頁面上，刪除所有內部標記`MainContent``Content`項目，然後插入其所在位置的下列標記：

[!code-aspx[Main](command-line-deployment/samples/sample1.aspx)]

執行專案並選取**有關**頁面。

![有關頁面](command-line-deployment/_static/image2.png)

## <a name="deploy-to-test-by-using-the-command-line"></a>使用命令列部署至測試

您將不會部署另一個資料庫變更，因此停用 dbDacFx 資料庫 aspnet ContosoUniversity 資料庫的部署。 開啟**發行 Web**精靈，並在每個，這三個發行設定檔，清除**更新資料庫** 核取方塊**設定** 索引標籤。

在 Windows 8 [開始] 頁面上，搜尋**VS2012 開發人員命令提示字元**。

以滑鼠右鍵按一下圖示**VS2012 開發人員命令提示字元**按一下**系統管理員身分執行**。

輸入下列命令，在命令提示字元中，方案檔案的路徑取代為您的方案檔的路徑：

[!code-console[Main](command-line-deployment/samples/sample2.cmd)]

MSBuild 建置方案，並將其部署到測試環境。

![命令列輸出](command-line-deployment/_static/image3.png)

開啟瀏覽器並移至`http://localhost/ContosoUniversity`，然後按一下 **有關**頁面，以驗證部署是否成功。

如果您尚未建立任何學生在測試中，您會看到下的空白頁面**學生主體統計資料**標題。 移至**學生**頁面上，按一下**新增學生**，加入一些學生，然後返回**有關**頁面，即可查看學生統計資料。

![關於測試環境中的頁面](command-line-deployment/_static/image4.png)

## <a name="key-command-line-options"></a>索引鍵的命令列選項

您輸入的命令傳遞至 MSBuild 的方案檔路徑和兩個屬性：

[!code-console[Main](command-line-deployment/samples/sample3.cmd)]

### <a name="deploying-the-solution-versus-deploying-individual-projects"></a>部署與部署個別專案方案

指定的方案檔導致要建置方案中的所有專案。 如果您在方案中有多個 web 專案，適用於下列 MSBuild 行為：

- 您在命令列指定的內容會傳遞至每個專案。 因此，每個 web 專案必須具有您指定之名稱的發行設定檔。 如果您指定`/p:PublishProfile=Test`，每個 web 專案必須具有名為的發行設定檔*測試*。
- 已成功，甚至不會建立另一個時，您可能會發行一個專案。 如需詳細資訊，請參閱 stackoverflow 執行緒[MSBuild 因兩個封裝](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages)。

如果您指定個別的專案，而不是方案，您必須加入的參數，指定 Visual Studio 版本。 如果您使用 Visual Studio 2012 命令列會類似下列的範例：

[!code-console[Main](command-line-deployment/samples/sample4.cmd?highlight=1)]

Visual Studio 2010 的版本號碼為 10.0。 如需詳細資訊，請參閱[Visual Studio 專案相容性和 VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) Sayed Hashimi 部落格上。

### <a name="specifying-the-publish-profile"></a>指定發行設定檔

依名稱或完整路徑，您可以指定發行設定檔*.pubxml*檔案，如下列範例所示：

[!code-console[Main](command-line-deployment/samples/sample5.cmd?highlight=1)]

### <a name="web-publish-methods-supported-for-command-line-publishing"></a>Web 發行支援的命令列發行方法

三個發行方法支援的命令列發行：

- `MSDeploy`-使用 Web Deploy 發行。
- `Package`藉由建立 Web 部署套件發佈。 您必須從 MSBuild 命令會建立個別安裝套件。
- `FileSystem`-發行檔案複製到指定的資料夾。

### <a name="specifying-the-build-configuration-and-platform"></a>指定組建組態與平台

在 Visual Studio 或命令列上，必須設定組建組態與平台。 發行設定檔包含屬性是名為`LastUsedBuildConfiguration`和`LastUsedPlatform`，但您無法設定這些屬性，以判斷如何建置專案。 如需詳細資訊，請參閱[MSBuild： 如何設定組態屬性](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx)Sayed Hashimi 部落格上。

## <a name="deploy-to-staging"></a>部署到預備環境

若要部署至 Azure，您必須新增至命令列的密碼。 如果您在 Visual Studio 中的發行設定檔中儲存的密碼，它已加密形式儲存在您*。 pubxml.user*檔案。 當您執行命令列部署中，您必須傳遞命令列參數中的密碼，該檔案不會由 MSBuild 存取。

1. 複製您需要從密碼*.publishsettings*稍早的預備網站下載的檔案。 密碼是值`userPWD`Web deploy 屬性`publishProfile`項目。

    ![Web 部署密碼](command-line-deployment/_static/image5.png)
2. 在 Windows 8 開始 頁面上，搜尋**VS2012 開發人員命令提示字元**，然後按一下以開啟 命令提示字元圖示。 （您不需要其系統管理員身分開啟此時因為您不部署至 IIS 在本機電腦上）。
3. 輸入下列命令，在命令提示字元中的方案檔的路徑取代為您的方案檔和您的密碼與密碼的路徑：

    [!code-console[Main](command-line-deployment/samples/sample6.cmd)]

    請注意，此命令列包含額外的參數： `/p:AllowUntrustedCertificate=true`。 正在寫入本教學課程，`AllowUntrustedCertificate`從命令列發行至 Azure 時，就必須設定屬性。 釋放這個 bug 的修正後，您不需要該參數。
4. 開啟瀏覽器並移至您的預備網站的 URL，然後按一下**有關**頁面，以驗證部署是否成功。

    如同稍早為測試環境，您可能需要建立統計資料，讓某些學生**有關**頁面。

## <a name="deploy-to-production"></a>部署到生產環境

部署到生產環境的程序是供暫存程序類似。

1. 複製您需要從密碼*.publishsettings*提早針對實際執行的網站下載的檔案。
2. 開啟**VS2012 開發人員命令提示字元**。
3. 輸入下列命令，在命令提示字元中的方案檔的路徑取代為您的方案檔和您的密碼與密碼的路徑：

    [!code-console[Main](command-line-deployment/samples/sample7.cmd)]

    真實的生產網站，如果還沒有資料庫變更，您會通常複製*應用程式\_offline.htm*檔案至站台部署之前，並成功部署之後刪除它。
4. 開啟瀏覽器並移至您的預備網站的 URL，然後按一下**有關**頁面，以驗證部署是否成功。

## <a name="summary"></a>總結

您現在已使用命令列部署應用程式更新。

![關於測試環境中的頁面](command-line-deployment/_static/image6.png)

在下一個教學課程中，您會看到範例說明如何擴充 web 發行管線。 此範例將說明如何部署不包含在專案中的檔案。

>[!div class="step-by-step"]
[上一頁](deploying-a-database-update.md)
[下一頁](deploying-extra-files.md)
