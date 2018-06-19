---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
title: 使用 Visual Studio 的 ASP.NET Web 部署： 部署到生產環境 |Microsoft 文件
author: tdykstra
description: 此教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web 應用程式或協力廠商裝載提供者，使用...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 416438a1-3b2f-4d27-bf53-6b76223c33bf
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
msc.type: authoredcontent
ms.openlocfilehash: f3b3898bd003ace100ba05619f2c45ca808462df
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889803"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-to-production"></a>使用 Visual Studio 的 ASP.NET Web 部署： 部署到生產環境
====================
由[Tom Dykstra](https://github.com/tdykstra)

[下載入門專案](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> 此教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web 應用程式或協力廠商裝載提供者，使用 Visual Studio 2012 或 Visual Studio 2010。 數列的相關資訊，請參閱[系列的第一個教學課程](introduction.md)。


## <a name="overview"></a>總覽

在本教學課程中，設定 Microsoft Azure 帳戶、 建立預備與生產環境和部署到預備環境的 ASP.NET web 應用程式和實際執行環境中的使用 Visual Studio 單鍵發行功能。

如果您想要的話，您可以將它部署到協力廠商主機服務提供者。 本教學課程中所述的程序的大部分都是相同的主控提供者或 azure，不同之處在於每個提供者有它自己的帳戶和網站管理的使用者介面。 您可以找到主控提供者中的[的提供者組件庫](https://www.microsoft.com/web/hosting)Microsoft.com 網站上。

提示： 如果您收到錯誤訊息，或當您瀏覽教學課程，項目無法運作，務必檢查本教學課程系列的 [疑難排解] 頁面。

## <a name="get-a-microsoft-azure-account"></a>取得 Microsoft Azure 帳戶

如果您還沒有 Azure 帳戶，您可以建立免費的試用帳戶只需要幾分鐘的時間。 如需詳細資訊，請參閱[Azure 免費試用](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)。

## <a name="create-a-staging-environment"></a>建立預備環境

> [!NOTE]
> 由於本教學課程所撰寫，Azure 應用程式服務會加入新功能，可自動化許多程序中建立暫存和實際執行環境。 請參閱[設定預備環境中 Azure App Service web 應用程式](https://azure.microsoft.com/documentation/articles/web-sites-staged-publishing/)。


中所述[部署至測試環境教學課程](deploying-to-iis.md)、 最可靠的測試環境是在具有方式就像是實際執行的網站主機服務提供者網站。 在許多主控提供者，您就必須權衡這個的優點和重大的額外成本，但在 Azure 中您可以建立其他可用的 web 應用程式為您執行的應用程式。 您也需要資料庫，並透過您的生產資料庫的費用的額外費用將會是 none 或最少。 在 Azure 中您必須支付您所使用的資料庫儲存體數量，而不是每個資料庫，而且您將使用在預備環境中的其他儲存體數量最小。

中所述[部署至測試環境教學課程](deploying-to-iis.md)、 預備和生產環境，您要將兩個資料庫部署至一個資料庫中。 如果您想要將它們分開，處理程序都是一樣，但是您會建立每個環境的其他資料庫，而當您建立的發行設定檔時，會選取每個資料庫的正確目的地字串。

本節的教學課程中，您將建立的 web 應用程式和資料庫来用於臨時環境中，而且您會部署到預備環境並建立及部署到生產環境之前，請測試那里。

> [!NOTE]
> 下列步驟顯示如何在 Azure App Service 中建立 web 應用程式，使用 Azure 管理入口網站。 在 Azure sdk 最新版本中，您同樣可以這樣不需要離開 Visual Studio 中，使用 [伺服器總管]。 在 Visual Studio 2013 中，您也可以直接從 [發行] 對話方塊建立 web 應用程式。 如需詳細資訊，請參閱[Azure App Service 中建立 ASP.NET web 應用程式。](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)


1. 在[Azure 管理入口網站](https://manage.windowsazure.com/)，按一下 **網站**，然後按一下 **新增**。
2. 按一下**網站**，然後按一下 **自訂建立**。

    **新增網站-自訂建立**精靈 隨即開啟。 **自訂建立**精靈可讓您建立的網站和資料庫在相同的時間。
3. 在**建立網站**步驟在精靈中，輸入字串**URL**方塊，以使用唯一的 URL 做為您的應用程式的預備環境。 例如，輸入 ContosoUniversity staging123 （包括讓其成為唯一以防 ContosoUniversity 臨時區域不會採取在最後的隨機數字）。

    完整的 URL 將會包含哪些您在此處輸入，加上尾碼，您所看到的文字方塊中。
4. 在**區域**下拉式清單中，選擇最接近您的地區。

    此設定指定您的 web 應用程式將執行中的資料中心。
5. 在**資料庫**下拉式清單中，選擇**建立新的 SQL database**。
6. 在**資料庫連接字串名稱**方塊中，保留預設值為*DefaultConnection*。
7. 按一下底部的方塊右邊的箭號。

    下圖顯示**建立網站**對話方塊的範例值。 URL 和您所輸入的區域將會不同。

    ![建立網站的步驟](deploying-to-production/_static/image1.png)

    精靈會進入**指定資料庫設定**步驟。
8. 在**名稱**方塊中，輸入*ContosoUniversity*加上隨機的數字，讓其成為唯一，例如*ContosoUniversity123*。
9. 在**伺服器**方塊中，選取**新 SQL Database 伺服器**。
10. 輸入系統管理員名稱和密碼。

    您不輸入現有的名稱和密碼。 您正在輸入的新名稱和您正在定義可存取資料庫時使用的密碼。
11. 在**區域**方塊中，選擇您選擇 web 應用程式的相同地區。

    保留網頁伺服器和資料庫伺服器位於相同區域中提供最佳的效能並減少費用。
12. 按一下底部的方塊以表示您已完成的核取記號。

    下圖顯示**指定資料庫設定**對話方塊的範例值。 您輸入的值可能會不同。

    ![資料庫的設定步驟的 新增網站-建立資料庫精靈](deploying-to-production/_static/image2.png)

    在管理入口網站返回網站] 頁面上，而**狀態**欄顯示 [正在建立 web 應用程式。 （通常小於一分鐘），一段時間之後**狀態**資料行會顯示已成功建立 web 應用程式。 在左側導覽列中的 web 應用程式，您會在您的帳戶數目旁會顯示**網站**圖示和資料庫數目旁會顯示**SQL 資料庫**圖示。

    ![管理入口網站，建立網站的網站頁面](deploying-to-production/_static/image3.png)

    您的 web 應用程式名稱會從圖例中的範例應用程式不同。

## <a name="deploy-the-application-to-staging"></a>部署到預備環境的應用程式

既然您已經建立的 web 應用程式和資料庫預備環境，您可以部署它的專案。

> [!NOTE]
> 以下指示說明如何建立發行設定檔下載 *.publishsettings*不只適用於 Azure，也為協力廠商裝載提供者的檔案。 最新版的 Azure SDK 也可讓您直接連接到 Azure，從 Visual Studio，然後選擇 從 web 應用程式必須在您的 Azure 帳戶中的清單。 在 Visual Studio 2013 中，您可以登入 Azure 從**Web Publish**對話方塊或從**伺服器總管**視窗。 如需詳細資訊，請參閱[Azure App Service 中建立 ASP.NET web 應用程式](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)。


### <a name="download-the-publishsettings-file"></a>下載.publishsettings 檔案

1. 按一下您剛才建立的 web 應用程式名稱。

    ![按一下以移至儀表板的站台](deploying-to-production/_static/image4.png)
2. 在下**快速概覽**中**儀表板**索引標籤上，按一下 **下載發行設定檔**。

    ![下載發行設定檔的連結](deploying-to-production/_static/image5.png)

    此步驟中下載檔案，其中包含所有您需要以應用程式部署至您的 web 應用程式的設定。 您將此檔案匯入到 Visual Studio，因此您不需要手動輸入此資訊。
3. 儲存 *.publishsettings*檔案，您可以從 Visual Studio 存取的資料夾中。

    ![儲存.publishsettings 檔案](deploying-to-production/_static/image6.png)

    > [!WARNING]
    > 安全性- *.publishsettings*檔案包含您的認證 （未編碼），可用來管理 Azure 訂用帳戶和服務。 這個檔案的安全性最佳作法是暫時儲存 （例如在 Libraries\Documents 資料夾），在來源目錄之外，並匯入完成後再將它刪除。 取得存取權的惡意使用者 *.publishsettings*檔案可以編輯、 建立和刪除您的 Azure 服務。

### <a name="create-a-publish-profile"></a>建立發行設定檔

1. 在 Visual Studio 中，以滑鼠右鍵按一下中的 ContosoUniversity 專案**方案總管 中**選取**發行**從內容功能表。

    **發行 Web**精靈 隨即開啟。
2. 按一下**設定檔** 索引標籤。
3. 按一下 [匯入] 。
4. 瀏覽至 *.publishsettings*您稍早，下載檔案，然後按一下**開啟**。

    ![匯入發行設定 對話方塊](deploying-to-production/_static/image7.png)
5. 在**連接**索引標籤上，按一下 **驗證連線**確定設定正確無誤。

    當已驗證的連接時，旁邊顯示綠色的核取記號**驗證連線** 按鈕。

    對於某些裝載的提供者，當您按一下**驗證連線**，您可能會看到**憑證錯誤** 對話方塊。 如果您這樣做，請確認伺服器名稱是您所預期。 如果伺服器名稱正確，請選取**儲存這個憑證以供未來 Visual Studio 工作階段**按一下**接受**。 （此錯誤表示主機服務提供者已選擇來避免所購買的 SSL 憑證適用於您要部署的 URL 的費用。 如果您想要使用的有效憑證來建立安全連接，請連絡您的主機服務提供者。）
6. 按 [ **下一步**]。

    ![連接成功圖示，然後在 [連線] 索引標籤中的下一步按鈕](deploying-to-production/_static/image8.png)
7. 在**設定**索引標籤上，依序展開**檔案發行選項**，然後選取**檔案排除應用程式\_資料資料夾**。

    如需下的其他選項**檔案發行選項**，請參閱[部署至 IIS](deploying-to-iis.md)教學課程。 螢幕擷取畫面，會顯示此步驟和下列資料庫的設定步驟的結果已結尾的資料庫設定步驟。
8. 在下**DefaultConnection**中**資料庫**區段中，成員資格資料庫的資料庫部署設定。
9. 1. 選取**更新資料庫**。

        **遠端的連接字串**方塊的正下方**DefaultConnection**填寫.publishsettings 檔案的連接字串。連接字串包含 SQL Server 認證，會以純文字儲存 *.pubxml*檔案。 如果您不想將它們儲存到永久那里，您可以部署資料庫之後從發行設定檔移除它們，並將其儲存在 Azure 中。 如需詳細資訊，請參閱[如何保護您的 ASP.NET 資料庫連接字串安全從來源部署至 Azure 時](http://www.hanselman.com/blog/HowToKeepYourASPNETDatabaseConnectionStringsSecureWhenDeployingToAzureFromSource.aspx)Scott Hanselman 部落格上。
      2. 按一下**設定資料庫更新**。
      3. 在**設定資料庫更新**對話方塊中，按一下 **加入 SQL 指令碼**。
      4. 在**加入 SQL 指令碼**方塊中，瀏覽至*aspnet-資料-prod.sql*指令碼，您稍早儲存的方案資料夾中，然後按一下**開啟**。
      5. 關閉**設定資料庫更新** 對話方塊。
10. 在下**SchoolContext**中**資料庫**區段中，選取**執行 Code First 移轉 （在應用程式啟動時執行）**。

    Visual Studio 會顯示**執行 Code First 移轉**而不是**更新資料庫**如`DbContext`類別。 如果您想要使用而不是移轉的 dbDacFx 提供者部署的資料庫，您可以使用存取`DbContext`類別，請參閱[如何部署 Code First 移轉沒有資料庫？](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations)中的 Visual Studio Web 部署常見問題集和 MSDN 上的 ASP.NET。

    **設定** 索引標籤現在看起來像下列的範例：

    ![臨時區域的 [設定] 索引標籤](deploying-to-production/_static/image9.png)
11. 執行下列步驟來儲存設定檔，並將它重新命名*臨時*:

    1. 按一下**設定檔**索引標籤，然後再按一下**管理設定檔**。
    2. 匯入建立兩個新的設定檔，另一個用於 FTP，一個用於 Web Deploy。 設定 Web Deploy 設定檔： 重新命名此設定檔來*臨時*。

        ![重新命名到預備環境的設定檔](deploying-to-production/_static/image10.png)
    3. 關閉**編輯 Web 發行設定檔** 對話方塊。
    4. 關閉**發行 Web**精靈。

### <a name="configure-a-publish-profile-transform-for-the-environment-indicator"></a>設定發行設定檔環境指標轉換

> [!NOTE]
> 本節說明如何設定 Web.config 轉換環境指標。 因為指標位於`<appSettings>`項目，您有另一個替代選項指定當您要部署至 Azure App Service 的轉換。 如需詳細資訊，請參閱[在 Azure 中的指定 Web.config 設定](web-config-transformations.md#watransforms)。


1. 在**方案總管 中**，依序展開**屬性**，然後展開**PublishProfiles**。
2. 以滑鼠右鍵按一下*Staging.pubxml*，然後按一下 **新增 Config 轉換**。

    ![新增預備 Config 轉換](deploying-to-production/_static/image11.png)

    Visual Studio 會建立*Web.Staging.config*轉換檔案並開啟它。
3. 在*Web.Staging.config*轉換檔案，請插入下列程式碼開頭之後立即`configuration`標記。

    [!code-xml[Main](deploying-to-production/samples/sample1.xml)]

    當您使用的暫存發行設定檔時，這項轉換就會設定"Prod"的環境標記。 在已部署的 web 應用程式時，就看任何尾碼，例如 「 （開發） 」 或 「 （測試） 」 之後的 「 Contoso 大學"H1 標題。
4. 以滑鼠右鍵按一下*Web.Staging.config*檔案，然後按一下 **預覽轉換**先確定您已編碼的轉換會產生預期的變更。

    **Web.config 預覽** 視窗會顯示結果的同時套用*Web.Release.config*轉換和*Web.Staging.config*轉換。

### <a name="prevent-public-use-of-the-test-app"></a>防止公用用途的測試應用程式

預備應用程式的一個重要考量是會即時在網際網路上，但您不想使用它公用。 人們會尋找並使用它的可能性降到最低，您可以使用一或多個下列方法：

- 設定存取權給暫存應用程式只允許您用來測試臨時的 IP 位址的防火牆規則。
- 使用模糊化會難以猜測的 URL。
- 建立*robots.txt*檔案，以確保搜尋引擎在搜尋結果中，，不編目給它的測試應用程式和報表的連結。

這些方法的第一個最有效，但未涵蓋在本教學課程因為將會要求您將部署到 Azure 雲端服務，而不是 Azure 應用程式服務。 在 Azure 中的 IP 限制和雲端服務的相關詳細資訊，請參閱[計算裝載選項所提供 Azure](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me)和[封鎖特定 IP 位址存取 Web 角色](https://msdn.microsoft.com/library/windowsazure/jj154098.aspx)。 如果您要部署到協力廠商主機服務提供者，請連絡提供者，以了解如何實作 IP 限制。

此教學課程中，您將建立*robots.txt*檔案。

1. 在**方案總管 中**，ContosoUniversity 專案上按一下滑鼠右鍵，然後按一下**加入新項目**。
2. 建立新**文字檔**名為*robots.txt*，並放入其中的下列文字：

    [!code-console[Main](deploying-to-production/samples/sample2.cmd)]

    `User-agent`一行告訴檔案中的規則套用至所有搜尋引擎網頁自動尋檢 (robots)，搜尋引擎和`Disallow`一行指定應該進行編目的網站上的任何頁面。

    您希望搜尋引擎目錄生產應用程式，因此您需要從生產環境部署中排除此檔案。 若要執行，您將設定在生產環境中的設定發行設定檔時建立它。

### <a name="deploy-to-staging"></a>部署到預備環境

1. 開啟**發行 Web** Contoso 大學專案上按一下滑鼠右鍵，然後按一下精靈**發行**。
2. 請確定**臨時**已選取設定檔。
3. 按一下 [發行] 。

    **輸出**視窗會顯示所執行的部署動作，並回報成功完成部署。 預設的瀏覽器會自動開啟至已部署的 web 應用程式的 URL。

## <a name="test-in-the-staging-environment"></a>在預備環境中測試

請注意，環境標記不存在 (沒有任何 「 （測試） 」 或 「 （開發） 」 之後的 H1 標題，其中顯示*Web.config*環境指標的轉換是否成功。

![首頁上的預備環境](deploying-to-production/_static/image12.png)

執行**學生**頁面，確認已部署的資料庫已經沒有學生。

執行**講師**頁面，即可確認 Code First 植入講師資料的資料庫：

選取**新增學生**從**學生**功能表上，新增一位學生，，然後再檢視中新的學生**學生**頁面，即可確認您可以成功地寫入至資料庫.

從**課程**頁面上，按一下**更新信用額度**。 **更新信用額度**頁面需要系統管理員權限，所以**登入**頁面隨即顯示。 輸入您建立更早 （"admin"、"prodpwd"） 的系統管理員帳戶認證。 **更新信用額度**頁面隨即顯示，以確認您在上一個教學課程中建立的系統管理員帳戶已正確地部署到測試環境。

要求會造成錯誤 ELMAH 會追蹤，並接著要求 ELMAH 錯誤報告無效的 URL。 如果您要部署到協力廠商主機服務提供者，您可能會發現基於相同理由，它是空的上一個教學課程中的報告為空白。 您必須使用裝載提供者的帳戶管理工具來設定啟用 ELMAH 寫入記錄檔資料夾的資料夾權限。

您所建立的應用程式現在正在執行，就如同您將使用生產環境 web 應用程式在雲端中。 因為一切運作正常下, 一個步驟是部署到生產環境。

## <a name="deploy-to-production"></a>部署到生產環境

建立生產環境 web 應用程式和部署到生產環境的程序是預備環境，與相同，不同之處在於您需要排除*robots.txt*從部署。 若要這樣做，您將編輯發行設定檔。

### <a name="create-the-production-environment-and-the-production-publish-profile"></a>建立生產環境和生產環境的發行設定檔

1. 在 Azure 中，您用於執行相同的程序建立生產環境 web 應用程式和資料庫。

    當您建立資料庫時，您可以選擇將您稍早建立的相同伺服器上，或建立新的伺服器。
2. 下載 *.publishsettings*檔案。
3. 建立發行設定檔匯入實際執行 *.publishsettings*檔案，您用於執行相同的程序。

    別忘了設定資料的部署指令碼，在**DefaultConnection**中**資料庫**區段**設定** 索引標籤。
4. 重新命名的發行設定檔*生產*。
5. 設定發行設定檔轉換環境指示器，相同的程序用來將暫存資料庫...

### <a name="edit-the-pubxml-file-to-exclude-robotstxt"></a>編輯排除 robots.txt.pubxml 檔案

發行設定檔的檔案會命名為&lt;profilename&gt;*.pubxml*和位於*PublishProfiles*資料夾。 *PublishProfiles*資料夾低於*屬性*資料夾中的 C# web 應用程式專案，在*我的專案*VB web 應用程式專案中或之下的資料夾*應用程式\_資料*資料夾中的 web 應用程式專案。 每個 *.pubxml*檔包含套用至其中一個設定發行設定檔。 您在發行網站精靈中輸入的值會儲存在這些檔案，以及您可以編輯它們，以便建立或變更不會在 Visual Studio UI 中提供的設定。

根據預設， *.pubxml*檔案包含在專案中，當您建立的發行設定檔，但您可以排除專案和 Visual Studio 仍會使用這些方法。 Visual Studio 尋找*PublishProfiles*資料夾 *.pubxml*檔案，無論是否包含在專案中。

每個 *.pubxml*檔案沒有 *。 pubxml.user*檔案。 *。 Pubxml.user*檔案包含加密的密碼，如果您選取**儲存密碼**選項，而且依預設就會從專案排除。

A *.pubxml*檔案包含屬於特定發行設定檔的設定。 如果您想要設定適用於所有設定檔的設定，您可以建立 *。 wpp.targets*檔案。 在建置程序會將這些檔案匯入 *.csproj*或 *.vbproj*專案檔，因此大部分的設定，您可以設定專案檔中設定這些檔案中。 如需有關 *.pubxml*檔案和 *。 wpp.targets*檔，請參閱[如何： 編輯發行設定檔 (.pubxml) 檔中的部署設定而。 wpp.targets Visual Studio 中的檔案Web 專案](https://msdn.microsoft.com/library/ff398069.aspx)。

1. 在**方案總管 中**，依序展開**屬性**展開**PublishProfiles**。
2. 以滑鼠右鍵按一下*Production.pubxml*按一下**開啟**。

    ![開啟.pubxml 檔案](deploying-to-production/_static/image13.png)
3. 以滑鼠右鍵按一下*Production.pubxml*按一下**開啟**。
4. 加入下列幾行的結束之前立即`PropertyGroup`項目：

    [!code-xml[Main](deploying-to-production/samples/sample3.xml)]

    .Pubxml 檔案現在看起來像下列的範例：

    [!code-xml[Main](deploying-to-production/samples/sample4.xml?highlight=18-20)]

    如需如何排除檔案和資料夾的詳細資訊，請參閱[可以我排除特定檔案或資料夾從部署嗎？](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment)中**適用於 Visual Studio 和 ASP.NET 的 Web 部署常見問題集**MSDN 上。

### <a name="deploy-to-production"></a>部署到生產環境

1. 開啟**發行 Web**精靈，確定**生產**發行設定檔已選取，然後按一下**啟動預覽**上**預覽** 索引標籤可讓您確認*robots.txt*不將檔案複製到生產環境應用程式。

    ![若要發行至生產環境的檔案的預覽](deploying-to-production/_static/image14.png)

    檢閱將複製的檔案清單。 您會看到所有 *.cs*檔案，包括 *.aspx.vb*， *。 aspx.designer.cs*， *Master.cs*，和*Master.designer.cs*檔案會省略。 這段程式碼編譯成*ContosoUniversity.dll*和*ContosUniversity.pdb*了中的檔案*bin*資料夾。 因為只有 *.dll*才能的執行應用程式，而且您稍早指定，應該部署到執行應用程式所需的檔案、 no *.cs*檔會複製到目的地環境。 *Obj*資料夾和*ContosoUniversity.csproj*和 *。 副檔名為.csproj.user*檔案已省略相同的原因。

    按一下**發行**將部署到實際執行環境。
2. 在生產環境中，您用於執行相同的程序中的測試。

    所有項目相當於除了 URL 的臨時區域與缺乏*robots.txt*檔案。

## <a name="summary"></a>總結

您現在已成功部署和測試您的 web 應用程式，它可公開在網際網路上。

![首頁生產環境](deploying-to-production/_static/image15.png)

在下一個教學課程中，您將更新應用程式程式碼，並將變更部署到測試、 預備及實際執行環境。

> [!NOTE]
> 在實際執行環境中使用您的應用程式時您應該實作復原計劃。 也就是說，您應該會定期備份您的資料庫從生產環境應用程式至安全的儲存體位置，並應該保持數個層代的這類備份。 當您更新資料庫時，請立即變更之前的備份複本。 然後，如果您犯了錯誤，並不探索它，直到它部署到生產環境之後，您將仍然能夠將資料庫復原到它損毀前的狀態。 如需詳細資訊，請參閱[Azure SQL Database 備份和還原](https://msdn.microsoft.com/library/windowsazure/jj650016.aspx)。
> 
> 
> [!NOTE]
> 在本教學課程 SQL Server 版本，您要部署為 Azure SQL Database。 部署程序類似於其他 SQL Server 版本時，真正的實際執行應用程式可能在某些情況下就 for Azure SQL Database 需要特殊的程式碼。 如需詳細資訊，請參閱[使用 Azure SQL Database](../../../../whitepapers/aspnet-data-access-content-map.md#ssdb)和[SQL Server 和 Azure SQL Database 之間選擇](../../../../whitepapers/aspnet-data-access-content-map.md#ssdbchoosing)。
> 
> [!div class="step-by-step"]
> [上一頁](setting-folder-permissions.md)
> [下一頁](deploying-a-code-update.md)
