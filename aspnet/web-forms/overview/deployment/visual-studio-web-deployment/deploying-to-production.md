---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
title: 使用 Visual Studio 的 ASP.NET Web 部署： 部署至生產環境 |Microsoft Docs
author: tdykstra
description: 本系列教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web Apps 或協力廠商裝載提供者，使用...
ms.author: aspnetcontent
ms.date: 02/15/2013
ms.assetid: 416438a1-3b2f-4d27-bf53-6b76223c33bf
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
msc.type: authoredcontent
ms.openlocfilehash: a91feedb9ac09b2a70ca256c72d312a1e78ecbc8
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827873"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-to-production"></a>使用 Visual Studio 的 ASP.NET Web 部署： 部署至生產環境
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

[下載入門專案](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> 本系列教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web Apps 或協力廠商裝載提供者，使用 Visual Studio 2012 或 Visual Studio 2010。 這個系列的相關資訊，請參閱[系列的第一個教學課程](introduction.md)。


## <a name="overview"></a>總覽

在本教學課程中，設定 Microsoft Azure 帳戶、 建立預備與生產環境和部署 ASP.NET web 應用程式至預備環境和生產環境中的使用 Visual Studio 單鍵發行功能。

如果您想，您可以部署到協力廠商裝載提供者。 本教學課程中所述的程序的大部分都是相同的主控提供者或 azure 中，不同之處在於每個提供者都有它自己的帳戶和網站管理的使用者介面。 您可以找到在主機服務提供者[提供者的資源庫](https://www.microsoft.com/web/hosting)Microsoft.com 網站上。

提醒： 如果您收到錯誤訊息，或當您瀏覽本教學課程，項目無法運作，請務必檢查在本教學課程系列中的 [疑難排解] 頁面。

## <a name="get-a-microsoft-azure-account"></a>取得 Microsoft Azure 帳戶

如果您還沒有 Azure 帳戶，您可以建立免費的試用帳戶，只需要幾分鐘的時間。 如需詳細資訊，請參閱 < [Azure 免費試用](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)。

## <a name="create-a-staging-environment"></a>建立預備環境

> [!NOTE]
> 因為撰寫本教學課程中，Azure App Service 就會加入新的功能，可自動化許多建立預備與生產環境的程序。 請參閱[設定預備環境中 Azure App Service 的 web apps](https://azure.microsoft.com/documentation/articles/web-sites-staged-publishing/)。


中所述[部署至測試環境教學課程](deploying-to-iis.md)、 最可靠的測試環境是在有就像實際執行的網站主機服務提供者的網站。 在許多主機服務提供者，您就必須權衡針對重大的額外成本，此優點，但在 Azure 中您可以建立其他的免費 web 應用程式和預備應用程式。 您也需要資料庫，而且，透過您的實際執行資料庫的費用的額外費用會是 none 或最小。 在 Azure 中您需支付您所使用的資料庫儲存體數量，而不是針對每個資料庫，並在預備環境中，您將使用的額外儲存空間量也能降低。

中所述[部署至測試環境教學課程](deploying-to-iis.md)、 預備和生產環境，您要將您的兩個資料庫部署至一個資料庫中。 如果您想要將它們分開，程序會相同，但是您會建立適用於每個環境的其他資料庫，而當您建立發行設定檔時，會選取每個資料庫的正確的目的地字串。

您將在本教學課程的這一節中建立 web 應用程式和預備環境中，使用的資料庫，而且您將部署至預備環境，並那里測試，再建立及部署到生產環境。

> [!NOTE]
> 下列步驟示範如何使用 Azure 管理入口網站，在 Azure App Service 中建立 web 應用程式。 在 Azure SDK 最新版本中，您也可以執行這不需要離開 Visual Studio 中，使用 [伺服器總管]。 在 Visual Studio 2013 中，您也可以直接從 [發佈] 對話方塊，以建立 web 應用程式。 如需詳細資訊，請參閱[Azure App Service 中建立 ASP.NET web 應用程式。](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)


1. 在  [Azure 管理入口網站](https://manage.windowsazure.com/)，按一下**網站**，然後按一下**新增**。
2. 按一下 **網站**，然後按一下**自訂建立**。

    **新的網站-自訂建立**精靈 隨即開啟。 **自訂建立**精靈可讓您在同一時間建立網站和資料庫。
3. 在 **建立網站**步驟的精靈中，輸入字串**URL**方塊，以使用唯一的 URL 做為您的應用程式的預備環境。 例如，輸入 ContosoUniversity staging123 （包括使其成為唯一萬一 ContosoUniversity 預備環境會在結尾的隨機數字）。

    完整的 URL 將會包含您在此處輸入，加上尾碼，您會看到的文字方塊中。
4. 在 **地區**下拉式清單中，選擇離您最近的區域。

    此設定會指定 web 應用程式將執行中的資料中心。
5. 在 **資料庫**下拉式清單中，選擇**建立新的 SQL database**。
6. 在  **DB 連接字串名稱**方塊中，保留預設值為*DefaultConnection*。
7. 按一下底部的方塊右邊的箭號。

    如下圖所示**建立網站**範例值 對話方塊。 URL 和您所輸入的區域將會不同。

    ![建立網站的步驟](deploying-to-production/_static/image1.png)

    精靈會進入**指定的資料庫設定**步驟。
8. 在 **名稱**方塊中，輸入*ContosoUniversity*再加上隨機的數字，使其成為唯一，例如*ContosoUniversity123*。
9. 在 **伺服器**方塊中，選取**新的 SQL Database 伺服器**。
10. 輸入系統管理員名稱和密碼。

    您不輸入現有的名稱和密碼。 您輸入新名稱和您定義現在以供未來存取資料庫時使用的密碼。
11. 在 **地區**方塊中，選擇您為 web 應用程式的相同區域。

    將 web 伺服器和資料庫伺服器保留在相同的區域提供您最佳的效能，並將費用降至最低。
12. 按一下底部的方塊以表示您已完成的核取記號。

    如下圖所示**指定的資料庫設定**範例值 對話方塊。 您輸入的值可能會不同。

    ![資料庫的設定步驟的 新增網站-建立資料庫精靈](deploying-to-production/_static/image2.png)

    管理入口網站會回到 網站 頁面中，而**狀態**欄顯示 正在建立 web 應用程式。 一段時間之後 （通常少於一分鐘），**狀態**資料行會顯示已成功建立 web 應用程式。 在左側導覽列中的 web 應用程式，您會在您的帳戶數目旁會出現**網站**圖示，然後資料庫數目旁會出現**SQL 資料庫**圖示。

    ![管理入口網站中，建立網站的 [網站] 頁面](deploying-to-production/_static/image3.png)

    您的 web 應用程式名稱會不同於在圖例中的範例應用程式。

## <a name="deploy-the-application-to-staging"></a>部署至預備環境的應用程式

既然您已建立 web 應用程式和預備環境的資料庫，您可以部署到它的專案。

> [!NOTE]
> 這些指示說明如何建立發行設定檔下載 *.publishsettings*檔案，這不只適用於 Azure，也適用於協力廠商主機服務提供者。 最新版的 Azure SDK 也可讓您直接連接至 Azure，從 Visual Studio，從您在您的 Azure 帳戶的 web 應用程式清單中選擇。 在 Visual Studio 2013 中，您可以登入 Azure **Web Publish**對話方塊或從**伺服器總管**視窗。 如需詳細資訊，請參閱 < [Azure App Service 中建立 ASP.NET web 應用程式](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)。


### <a name="download-the-publishsettings-file"></a>下載.publishsettings 檔案

1. 按一下您剛才建立的 web 應用程式的名稱。

    ![按一下以移至儀表板網站](deploying-to-production/_static/image4.png)
2. 底下**概覽**中**儀表板**索引標籤上，按一下 **下載發行設定檔**。

    ![下載發行設定檔連結](deploying-to-production/_static/image5.png)

    此步驟中下載檔案，其中包含所有您需要以應用程式部署至您的 web 應用程式的設定。 您將此檔案匯入到 Visual Studio 讓您不必手動輸入此資訊。
3. 儲存 *.publishsettings*檔案的資料夾中，您可以從 Visual Studio 存取。

    ![儲存.publishsettings 檔案](deploying-to-production/_static/image6.png)

    > [!WARNING]
    > 安全性- *.publishsettings*檔案包含您的認證 （未編碼），用來管理您的 Azure 訂用帳戶和服務。 此檔案的安全性最佳作法是暫時儲存 （例如在 Libraries\Documents 資料夾），在來源目錄之外，並匯入完成後再將它刪除。 惡意使用者取得存取權 *.publishsettings*檔案可以編輯、 建立和刪除您的 Azure 服務。

### <a name="create-a-publish-profile"></a>建立發行設定檔

1. 在 Visual Studio 中，以滑鼠右鍵按一下 ContosoUniversity 中的專案**方案總管**，然後選取**發佈**從內容功能表。

    **發佈 Web**精靈 隨即開啟。
2. 按一下 [**設定檔**] 索引標籤。
3. 按一下 [匯入] 。
4. 瀏覽至 *.publishsettings*您稍早下載的檔案，然後按一下**開啟**。

    ![匯入發行設定 對話方塊](deploying-to-production/_static/image7.png)
5. 在 **連接**索引標籤上，按一下**驗證連線**藉此確定設定正確無誤。

    當已驗證的連接時旁, 顯示綠色的核取記號**驗證連線** 按鈕。

    對於某些裝載的提供者，當您按一下 [**驗證連線**，您可能會看到**憑證錯誤**] 對話方塊。 如果您這樣做，請確認伺服器名稱是您的預期。 如果伺服器名稱正確，請選取**儲存此憑證以供未來的工作階段的 Visual Studio**然後按一下**接受**。 （此錯誤表示主機服務提供者已選擇來避免針對您要部署的 URL 中購買 SSL 憑證的費用。 如果您想要使用的有效憑證來建立安全連接，請連絡您的主機服務提供者。）
6. 按 [ **下一步**]。

    ![連接成功圖示，然後在 連線 索引標籤中的下一步 按鈕](deploying-to-production/_static/image8.png)
7. 在**設定**索引標籤上，展開**檔案發行選項**，然後選取**排除應用程式中的檔案\_Data 資料夾**。

    如需其他選項下,**檔案發佈選項**，請參閱[部署至 IIS](deploying-to-iis.md)教學課程。 螢幕擷取畫面的顯示此步驟以及下列資料庫的設定步驟的結果是資料庫的組態步驟的結尾。
8. 底下**DefaultConnection**中**資料庫**區段中，設定成員資格資料庫的資料庫部署。
9. 1. 選取 **更新資料庫**。

        **遠端的連接字串**下方的方塊**DefaultConnection** .publishsettings 檔案的連接字串會填入。連接字串包含會儲存在以純文字的 SQL Server 認證 *.pubxml*檔案。 如果您不想將它們儲存到永久那里，您可以在部署資料庫之後，從發行設定檔移除它們，並將它們儲存在 Azure 中。 如需詳細資訊，請參閱 <<c0> [ 如何保護您的 ASP.NET 資料庫連接字串從來源部署至 Azure 時](http://www.hanselman.com/blog/HowToKeepYourASPNETDatabaseConnectionStringsSecureWhenDeployingToAzureFromSource.aspx)Scott Hanselman 的部落格上。
      2. 按一下 **設定資料庫更新**。
      3. 在 [**設定資料庫更新**] 對話方塊中，按一下**新增 SQL 指令碼**。
      4. 在 **新增 SQL 指令碼**方塊中，瀏覽至*aspnet-資料-prod.sql*指令碼，您在方案資料夾中，稍早儲存，然後按一下**開啟**。
      5. 關閉**設定資料庫更新** 對話方塊。
10. 底下**SchoolContext**中**資料庫**區段中，選取**Execute Code First Migrations （在應用程式啟動時執行）**。

    Visual Studio 會顯示**Execute Code First Migrations**而不是**更新資料庫**如`DbContext`類別。 如果您想要的 dbDacFx 提供者而非移轉，若要部署的資料庫，您可以使用存取`DbContext`類別，請參閱[如何部署沒有移轉的 Code First 資料庫？](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations)中適用於 Visual Studio Web 部署常見問題集和 MSDN 上的 ASP.NET。

    **設定** 索引標籤現在看起來如下列範例所示：

    ![預備環境的 [設定] 索引標籤](deploying-to-production/_static/image9.png)
11. 執行下列步驟，以儲存設定檔，並將它重新命名*臨時*:

    1. 按一下 **設定檔**索引標籤，然後再按一下**管理設定檔**。
    2. 匯入建立兩個新的設定檔，一個用於 FTP，一個用於 Web Deploy。 設定 Web Deploy 的設定檔： 重新命名此設定檔*臨時*。

        ![重新命名設定檔至預備環境](deploying-to-production/_static/image10.png)
    3. 關閉**編輯 Web 發行設定檔** 對話方塊。
    4. 關閉**發佈 Web**精靈。

### <a name="configure-a-publish-profile-transform-for-the-environment-indicator"></a>設定環境指示器的發行設定檔轉換

> [!NOTE]
> 本節說明如何設定環境指標 Web.config 轉換。 因為指標位於`<appSettings>`項目，您有指定的轉換，當您部署至 Azure App Service 的另一個替代方式。 如需詳細資訊，請參閱 <<c0> [ 在 Azure 中的指定 Web.config 設定](web-config-transformations.md#watransforms)。


1. 在 [**方案總管] 中**，展開**屬性**，然後展開**PublishProfiles**。
2. 以滑鼠右鍵按一下*Staging.pubxml*，然後按一下**新增組態轉換**。

    ![新增設定轉換的預備環境](deploying-to-production/_static/image11.png)

    Visual Studio 會建立*Web.Staging.config*轉換檔案並開啟它。
3. 在  *Web.Staging.config*轉換檔中，開啟之後，立即插入下列程式碼`configuration`標記。

    [!code-xml[Main](deploying-to-production/samples/sample1.xml)]

    當您使用預備發行設定檔時，這項轉換就會設定到 「 生產 」 環境指標。 在已部署的 web 應用程式中，您不會看到任何尾碼，例如"(Dev)"（測試） 」 之後的"Contoso University"H1 標題。
4. 以滑鼠右鍵按一下*Web.Staging.config*檔案，然後按一下**預覽轉換**藉此確定您編寫的轉換會產生預期的變更。

    **Web.config 預覽** 視窗會顯示結果的同時套用*Web.Release.config*轉換並*Web.Staging.config*轉換。

### <a name="prevent-public-use-of-the-test-app"></a>防止公用測試應用程式使用

預備應用程式的一個重要考量是，會即時在網際網路上，但您不想將它公開。 人會尋找並使用它的可能性降到最低，您可以使用一或多個下列方法：

- 設定防火牆規則，允許只能從您用來測試預備環境的 IP 位址存取預備應用程式。
- 使用無法猜測的隱藏的 URL。
- 建立*robots.txt*檔案，以確保搜尋引擎在搜尋結果中，，不編目，測試應用程式和報表連結。

這些方法的第一個是最有效率的但因為它會要求您將部署到 Azure 雲端服務而不是 Azure App Service 未涵蓋在本教學課程。 如需有關雲端服務的詳細資訊和在 Azure 中的 IP 限制，請參閱[計算裝載選項所提供 Azure](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me)並[封鎖特定 IP 位址存取 Web 角色](https://msdn.microsoft.com/library/windowsazure/jj154098.aspx)。 如果您要部署到協力廠商主機服務提供者，請連絡提供者，以了解如何實作 IP 限制。

本教學課程中，您將建立*robots.txt*檔案。

1. 在 **方案總管**，以滑鼠右鍵按一下 ContosoUniversity 專案，然後按一下**加入新項目**。
2. 建立新**文字檔**名為*robots.txt*，並將下列文字放在它：

    [!code-console[Main](deploying-to-production/samples/sample2.cmd)]

    `User-agent`一行告訴檔案中的規則適用於所有搜尋引擎 web 編目程式 （機器人），搜尋引擎和`Disallow`一行可讓您指定應該進行編目的網站上的任何頁面。

    您希望搜尋引擎目錄生產應用程式，因此您需要從生產環境部署中排除此檔案。 若要執行，您會設定在生產環境中的設定發行設定檔，當您在建立時。

### <a name="deploy-to-staging"></a>部署至預備環境

1. 開啟**發佈 Web** Contoso 大學專案上按一下滑鼠右鍵，然後按一下精靈**發佈**。
2. 請確定**臨時**已選取設定檔。
3. 按一下 [發行] 。

    **輸出**視窗會顯示所執行的部署動作，並回報成功完成部署。 預設瀏覽器會自動開啟已部署的 web 應用程式的 URL。

## <a name="test-in-the-staging-environment"></a>在預備環境中測試

請注意環境指標不存在 (沒有 「 （測試） 」 或"(Dev)"之後的 H1 標題，其中會顯示*Web.config*環境指標的轉換是否成功。

![首頁上的預備環境](deploying-to-production/_static/image12.png)

執行**學生**頁面，確認已部署的資料庫有任何學生。

執行**講師**頁面，確認第一個程式碼植入資料庫與講師資料：

選取 **新增的學生**從**學生**功能表上，新增為學生，，然後檢視 在新的學生**學生**頁面，確認您可以成功地寫入至資料庫.

從**課程**頁面上，按一下**更新信用額度**。 **更新信用額度**頁面會要求系統管理員權限，所以**登入**頁面隨即顯示。 輸入您建立舊版 （"admin"和"prodpwd 」） 的系統管理員帳戶認證。 **更新信用額度**顯示網頁時，會驗證您在上一個教學課程中建立的系統管理員帳戶已正確部署到測試環境。

要求無效的 URL，以引發錯誤，ELMAH 會追蹤，並接著要求 ELMAH 錯誤報告。 如果您要部署到協力廠商主機服務提供者，您可能會發現基於相同原因，並將該空白先前的教學課程中，報告為空白。 您必須使用裝載提供者的帳戶管理工具來設定資料夾權限，以啟用要寫入的記錄檔資料夾的 ELMAH。

您所建立的應用程式現在正在雲端中就如同您會將它用於生產環境的 web 應用程式。 由於一切運作正常下, 一個步驟是部署到生產環境。

## <a name="deploy-to-production"></a>部署到生產環境

建立生產 web 應用程式和部署至生產環境的程序是預備環境，與相同，不同之處在於您需要排除*robots.txt*從部署。 若要這樣做，您將編輯發行設定檔。

### <a name="create-the-production-environment-and-the-production-publish-profile"></a>建立生產環境和生產環境的發行設定檔

1. 在 Azure 中，遵循相同的程序，您用於預備環境中建立的生產 web 應用程式和資料庫。

    當您建立資料庫時，您可以選擇將它放在您稍早建立的相同伺服器上，或建立新的伺服器。
2. 下載 *.publishsettings*檔案。
3. 建立發行設定檔匯入生產 *.publishsettings*遵循相同的程序，您用於暫存的檔案。

    別忘了設定底下的資料部署指令碼**DefaultConnection**中**資料庫**一節**設定** 索引標籤。
4. 重新命名發行設定檔*生產*。
5. 設定發行設定檔轉換環境指示器，遵循相同的程序，您用於預備環境...

### <a name="edit-the-pubxml-file-to-exclude-robotstxt"></a>編輯.pubxml 檔案，以排除 robots.txt

發行設定檔的檔案會命名為&lt;profilename&gt;*.pubxml*且位於*PublishProfiles*資料夾。 *PublishProfiles*的資料夾是下*屬性*資料夾中的 C# web 應用程式專案，在*我的專案*VB web 應用程式專案中或之下的資料夾*應用程式\_資料*資料夾中的 web 應用程式專案。 每個 *.pubxml*檔案包含套用至其中一個設定發行設定檔。 您在 [發行 Web] 精靈中輸入的值會儲存在這些檔案，以及您可以編輯它們以建立或變更不可在 Visual Studio UI 中的設定。

根據預設， *.pubxml*檔案會包含在專案上，當您建立發行設定檔，但您可以排除專案和 Visual Studio 仍然使用它們。 Visual Studio 尋找*PublishProfiles*資料夾 *.pubxml*檔案，不論它們是否包含在專案中。

每個 *.pubxml*檔案沒有 *.pubxml.user*檔案。 *.Pubxml.user*檔案包含加密的密碼，如果您選取**儲存密碼**選項，並依預設就會從專案排除。

A *.pubxml*檔案包含屬於特定發行設定檔的設定。 如果您想要設定適用於所有的設定檔的設定，您可以建立 *.wpp.targets*檔案。 建置程序匯入這些檔案放進 *.csproj*或是 *.vbproj*專案檔，因此大部分的設定，您可以設定專案檔中設定這些檔案中。 如需詳細資訊 *.pubxml*檔案並 *.wpp.targets*檔案，請參閱[How to： 編輯發行設定檔 (.pubxml) 檔中的部署設定和.wpp.targets 檔案在 Visual Studio 中Web 專案](https://msdn.microsoft.com/library/ff398069.aspx)。

1. 在 [**方案總管] 中**，展開**屬性**展開**PublishProfiles**。
2. 以滑鼠右鍵按一下*Production.pubxml*然後按一下**開啟**。

    ![開啟.pubxml 檔案](deploying-to-production/_static/image13.png)
3. 以滑鼠右鍵按一下*Production.pubxml*然後按一下**開啟**。
4. 新增下列幾行的結束之前立即`PropertyGroup`項目：

    [!code-xml[Main](deploying-to-production/samples/sample3.xml)]

    專屬的.pubxml 檔案現在看起來如下列範例所示：

    [!code-xml[Main](deploying-to-production/samples/sample4.xml?highlight=18-20)]

    如需如何排除檔案和資料夾的詳細資訊，請參閱[我是否能排除特定檔案或資料夾從部署？](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment)中**適用於 Visual Studio 和 ASP.NET Web 部署常見問題集**MSDN 上。

### <a name="deploy-to-production"></a>部署到生產環境

1. 開啟**發佈 Web**精靈，確定**生產**發行設定檔已選取，然後按一下**開始預覽**上**預覽** 索引標籤，確認*robots.txt*不將檔案複製到實際執行應用程式。

    ![檔案發行至生產環境的預覽](deploying-to-production/_static/image14.png)

    檢閱將複製之檔案的清單。 您會看到的所有 *.cs*檔案，包括 *。 aspx.cs*， *。 aspx.designer.cs*， *Master.cs*，和*Master.designer.cs*檔案會省略。 這段程式碼編譯成*ContosoUniversity.dll*並*ContosUniversity.pdb*中找到的檔案*bin*資料夾。 因為只有 *.dll*才能的執行應用程式，以及您稍早指定程式可部署到執行應用程式所需的檔案、 no *.cs*檔案複製到目的地環境。 *Obj*資料夾並*ContosoUniversity.csproj*並 *.csproj.user*檔案已省略基於相同原因。

    按一下 **發佈**来部署到生產環境。
2. 在生產環境中，遵循相同的程序，您用於預備環境中測試。

    所有項目等同於 URL 除外的預備環境和缺乏*robots.txt*檔案。

## <a name="summary"></a>總結

您現在已成功部署和測試您的 web 應用程式，它可公開在網際網路上。

![首頁上生產環境](deploying-to-production/_static/image15.png)

在下一個教學課程中，您將更新應用程式程式碼，並將變更部署到測試、 預備及生產環境。

> [!NOTE]
> 在生產環境中使用您的應用程式時您應該實作復原計劃。 也就是您應該會定期備份您的資料庫從生產環境應用程式到安全的儲存體位置，以及您應該保留數個層代的這種備份。 當您更新資料庫時，您要立即在變更之前的備份複本。 然後，如果發生錯誤，而不加以探索直到您已將它部署到生產環境之後，您仍然能夠將資料庫復原到其損毀前的狀態。 如需詳細資訊，請參閱 < [Azure SQL Database 備份和還原](https://msdn.microsoft.com/library/windowsazure/jj650016.aspx)。
> 
> 
> [!NOTE]
> 在本教學課程 SQL Server 版本，您要部署會是 Azure SQL Database。 部署程序類似於其他 SQL Server 版本時，實際的生產應用程式可能在某些情況下就為 Azure SQL Database 需要特殊的程式碼。 如需詳細資訊，請參閱 <<c0> [ 熟悉 Azure SQL Database](../../../../whitepapers/aspnet-data-access-content-map.md#ssdb)並[SQL Server 和 Azure SQL Database 之間進行選擇](../../../../whitepapers/aspnet-data-access-content-map.md#ssdbchoosing)。
> 
> [!div class="step-by-step"]
> [上一頁](setting-folder-permissions.md)
> [下一頁](deploying-a-code-update.md)
