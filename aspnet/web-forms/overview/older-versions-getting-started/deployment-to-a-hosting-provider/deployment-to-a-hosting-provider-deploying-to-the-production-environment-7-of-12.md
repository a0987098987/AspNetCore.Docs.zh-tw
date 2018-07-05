---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
title: 使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 部署到生產環境-12 個 7 |Microsoft Docs
author: tdykstra
description: 這一系列的教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式專案，其中包含 SQL Server Compact 資料庫，使用 Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: b83ab819-2b05-4776-b7b4-79ef78d457a5
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
msc.type: authoredcontent
ms.openlocfilehash: 26a832fd336f886ba1ddfb1682930afa4df56c58
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37383632"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-the-production-environment---7-of-12"></a>使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 部署到生產環境-12 個 7
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

[下載入門專案](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> 這一系列的教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式專案使用 Visual Studio 2012 RC 或 Visual Studio Express 2012 RC for Web 包含 SQL Server Compact 資料庫。 如果您安裝 Web Publish Update，您也可以使用 Visual Studio 2010。 在數列的簡介，請參閱[系列的第一個教學課程](deployment-to-a-hosting-provider-introduction-1-of-12.md)。
> 
> 顯示 Visual Studio 2012 RC 版本之後引入的部署功能，示範如何部署 SQL Server Compact，以外的 SQL Server 版本，並示範如何部署至 Azure App Service Web Apps 的教學課程，請參閱[ASP.NET Web 部署使用 Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md)。


## <a name="overview"></a>總覽

在本教學課程中，方法，您可以設定具有主機服務提供者的帳戶，並部署您的 ASP.NET web 應用程式至生產環境使用 Visual Studio 單鍵發行功能。

提醒： 如果您收到錯誤訊息，或當您瀏覽本教學課程，項目無法運作，請務必檢查[疑難排解頁面](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)。

## <a name="selecting-a-hosting-provider"></a>選取 主控提供者

對於 Contoso 大學應用程式，本教學課程系列中，您需要支援 ASP.NET 4 和 Web Deploy 的提供者。 特定的控管公司選擇是讓教學課程可以說明部署到即時網站的完整體驗。 每個代管公司提供不同的功能，並部署至伺服器的經驗有所不同。 不過，本教學課程中所述的程序是典型的整個程序。 用於此教學課程中，Cytanium.com，主機服務提供者是許多可供使用，且其使用在本教學課程不會構成作背書或推薦。

當您準備好要選取您自己的主機服務提供者時，您可以比較功能與價格[提供者的資源庫](https://www.microsoft.com/web/hosting)Microsoft.com/web site 上。

## <a name="creating-an-account"></a>建立帳戶

在您選取的提供者建立的帳戶。 如果支援完整的 SQL Server 資料庫是新增額外，您就不必在本教學課程中，選取它，但您必須針對[移轉至 SQL Server](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)稍後在本系列教學課程。

這些教學課程中，您不需要註冊新的網域名稱。 您可以測試以確認成功部署，使用暫存的提供者指派給網站的 URL。

在建立帳戶之後，您通常會收到歡迎電子郵件，其中包含部署及管理您的網站所需的所有資訊。 您的主機服務提供者會傳送給您的資訊將會類似於這裡顯示。 Cytanium 歡迎電子郵件傳送給新的帳戶擁有者，包括下列資訊：

- 提供者的控制項面板站台，您可以在其中管理您的網站設定 URL。 在歡迎電子郵件，以方便參考的這個部分包含的識別碼和您指定的密碼。 （兩者都有已變更為下圖示範值）。

    [![Welcome_Email_Control_Panel_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image1.png)
- 預設.NET Framework 版本以及如何變更它的相關資訊。 許多裝載站台預設值為 2.0 時，2.0、 3.0 或 3.5 與.NET Framework 為目標的 ASP.NET 應用程式搭配運作。 但 Contoso 大學是.NET Framework 4 應用程式，因此您必須變更此設定。 （ASP.NET 4.5 應用程式您會使用.NET 4.0 設定）。

    [![Welcome_Email_Framework_Version](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image3.png)
- 暫存 URL 可供您存取您的網站。 建立此帳戶時，「 contosouniversity.com"輸入為現有的網域名稱。 因此暫存的 URL 是`http://contosouniversity.com.vserver01.cytanium.com`。

    [![Welcome_Email_Temporary_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image5.png)
- 如何設定資料庫和您需要才能存取它們的連接字串的相關資訊：

    [![Welcome_Email_Database_Info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image7.png)
- 工具和部署您的網站設定的相關資訊。 （來自 Cytanium 的電子郵件也提及 WebMatrix，在這裡省略）。

    [![Welcome_Email_Deploy_info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image10.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image9.png)

## <a name="setting-the-net-framework-version"></a>設定.NET Framework 版本

Cytanium 歡迎電子郵件會包含有關如何變更.NET Framework 版本的指示的連結。 下列指示說明，這可以透過 Cytanium 控制台。 其他提供者有控制面板的網站，看起來不同，或它們可能會指示您這麼做的方式不同。

請移至控制台 URL。 登入您的使用者名稱和密碼之後，您會看到在控制台中。

[![Cytanium_Control_Panel](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image11.png)

在 **裝載空格**，將指標停留 Web 圖示並選取**網站**從功能表。

[![Cytanium_Control_Panel_selecting_Web_Sites](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image13.png)

在  **Web Sites**方塊中，按一下**contosouniversity.com** （您已建立的帳戶時所使用的站台的名稱）。

[![Cytanium_Control_Panel_selecting_contosouniversity](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image15.png)

在 [**的網站內容**方塊中，選取**延伸模組**] 索引標籤。

[![Cytanium_Control_Panel_Extensions_tab](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image17.png)

變更從 ASP.NET **2.0 整合式的管線**要**4.0 （整合式管線）**，然後按一下**更新**。

## <a name="publishing-to-the-hosting-provider"></a>發行至主控提供者

從主控提供者的歡迎電子郵件包含所有必要的設定才能發佈專案，請和您可以手動輸入這些資訊至發行設定檔。 但是您將設定部署給提供者使用更容易和更少出錯的方法： 您將下載 *.publishsettings*檔案，然後匯入發行設定檔。

在瀏覽器中移至 [Cytanium] 控制台，然後選取**Web** ，然後選取**網站。**

![選取網站的控制台](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image19.png)

選取  **contosouniversity.com**網站。

![控制台中選取 contosouniversity.com](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image20.png)

選取 [ **Web Publishing** ] 索引標籤。

![控制面板 Web 發行 索引標籤](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image21.png)

建立要用於 web 發行輸入使用者名稱和密碼認證。 您可以輸入您用以登入控制台中的相同認證。 然後按一下**啟用**。

![控制台中建立發佈認證](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image22.png)

按一下 **這個網站下載發行設定檔**。

![控制面板下載發行設定檔](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image23.png)

當系統提示使用者開啟或儲存檔案時，請將它儲存。

![儲存發佈設定檔](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image24.png)

在 **方案總管**在 Visual Studio 中，以滑鼠右鍵按一下 ContosoUniversity 專案，然後選取**發佈**。 **發佈 Web**  對話方塊會開啟**預覽**索引標籤**測試**選取，因為這是您所使用的最後一個設定檔設定檔。

選取 **設定檔**索引標籤，然後按一下**匯入**。

![發佈 Web 精靈匯入 按鈕](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image25.png)

在 **匯入發佈設定**對話方塊中，選取 *.publishsettings*檔案，您已下載，然後按一下 **開啟**。 精靈將進入 [連線] 索引標籤中，所有填入的欄位。

![發佈 Web 精靈連線 索引標籤](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image26.png)

.Publishsettings 檔案會將站台的計劃的永久 URL 放在 [目的地 URL] 方塊中，但如果您尚未購買該網域，將值替換為暫存的 URL。 此範例中，URL 是 *[ http://contosouniversity.com.vserver01.cytanium.com ](http://contosouniversity.com.vserver01.cytanium.com)。* 這個方塊的唯一目的是指定何種瀏覽器會開啟自動之後成功部署後的 URL。 如果保留空白，唯一的結果會是瀏覽器不會在部署後自動啟動。

按一下 **驗證連線**確認設定是否正確，而且您可以連接到伺服器。 如您稍早所見，綠色的核取記號會驗證是成功的連線。

當您按一下 [驗證連線時，您可能會看到**憑證錯誤**] 對話方塊。 如果您這樣做，請確認伺服器名稱是您的預期。 如果是，選取**儲存此憑證以供未來的工作階段的 Visual Studio**然後按一下**接受**。 （此錯誤表示主機服務提供者已選擇來避免針對您要部署的 URL 中購買 SSL 憑證的費用。 如果您想要使用的有效憑證來建立安全連接，請連絡您的主機服務提供者。）

![憑證錯誤](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image27.png)

按 [ **下一步**]。

在 **資料庫**一節**設定**索引標籤上，輸入相同測試輸入值的發行設定檔。 您可以在下拉式清單中找到所需的連接字串。

- 在的連接字串中**SchoolContext，** 選取 `Data Source=|DataDirectory|School-Prod.sdf`
- 底下**SchoolContext**，選取**套用 Code First 移轉**。
- 在的連接字串中**DefaultConnection**，選取 `Data Source=|DataDirectory|aspnet-Prod.sdf`
- 底下**DefaultConnection**，保留**更新資料庫**清除。

![發佈 Web 精靈設定索引標籤](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image28.png)

按 [ **下一步**]。

在  **Preview**索引標籤上，按一下**開始預覽**若要查看將複製的檔案清單。 您會看到您稍早看到當您部署至 IIS，在本機電腦上的相同清單。

在發行之前，變更設定檔的名稱，使您 Web.Production.config 轉換檔案將會套用。 選取 **設定檔**索引標籤，然後按一下**管理設定檔**。

![發佈 Web 精靈管理設定檔](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image29.png)

在**編輯 Web 發行設定檔**對話方塊方塊中，選取實際執行設定檔中，按一下**重新命名**，並將設定檔名稱變更為生產環境。 然後按一下**關閉**。

![編輯 Web 發行設定檔 對話方塊](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image30.png)

按一下 [發行] 。

應用程式發行至主控提供者。 結果會顯示在**輸出**視窗。

![在部署後的 [輸出] 視窗](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image31.png)

在中所輸入的 URL 會自動開啟瀏覽器**目的地 URL**方塊**連線** 索引標籤**發佈 Web**精靈。 您會看到相同的首頁上，做為當您執行的站台在 Visual Studio 中，但現在沒有 「 （測試） 」 或 「 （開發） 」 的標題列中的環境指標。 這表示環境指示器*Web.config*轉換正常運作。

> [!NOTE]
> 如果您仍然會看到標題中的 [（測試）]，刪除*obj*從 ContosoUniversity 專案，然後重新部署的資料夾。 在發行前版本軟體，先前套用的轉換檔 (Web.Test.config) 可能會取得適用於一次雖然您使用實際執行設定檔。


[![Home_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image33.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image32.png)

執行會導致資料庫存取的頁面之前，請確定 Elmah 能夠記錄任何發生的錯誤。

## <a name="setting-folder-permissions-for-elmah"></a>設定資料夾的權限的 Elmah

如您記得此系列的上一個教學課程中，您必須確定應用程式有 Elmah 儲存錯誤記錄檔的位置中您的應用程式資料夾的寫入權限。 當您部署至 IIS 在本機電腦上時，會以手動方式設定這些權限。 在本節中，您會看到如何在 Cytanium 設定權限。 （某些裝載提供者可能不會啟用您若要這樣做，他們可能會提供一或多個預先定義的資料夾，以寫入權限。 在此情況下您必須修改應用程式以使用指定的資料夾。）

Cytanium 控制項台中，您可以設定資料夾權限。 請移至控制項面板 URL，然後選取**檔案管理員**。

[![Cytanium_Control_Panel_with_File_Manager_selected](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image35.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image34.png)

在 **檔案管理員**方塊中，選取**contosouniversity.com** ，然後**wwwrooot**若要查看應用程式的根資料夾。 按一下 掛鎖圖示旁**Elmah**。

[![Cytanium_Control_Panel_File_Manager_at_root_folder](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image37.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image36.png)

在 **檔案**/**資料夾權限**視窗中，選取**讀取**及**撰寫**核取方塊**contosouniversity.com** ，按一下 **設定的權限**。

[![Cytanium_Control_Panel_File_Folder_Permissions_Elmah](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image39.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image38.png)

請確定 Elmah 有寫入權限*Elmah*導致錯誤發生，並顯示 Elmah 錯誤報告的資料夾。 要求無效的 URL，例如*Studentsxxx.aspx*。 如往常一般，您會看到*GenericErrorPage.aspx*頁面。 按一下 **登出**連結，然後再執行*Elmah.axd*。 得到**登入**頁面上第一次，會驗證*Web.config*轉換已成功新增 Elmah 授權。 登入之後，您會看到顯示您剛才所造成的錯誤的報表。

[![Elmah.axd_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image41.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image40.png)

## <a name="testing-in-the-production-environment"></a>在生產環境中測試

執行**學生**頁面。 應用程式嘗試存取 School 資料庫第一次，Code First 移轉來建立資料庫觸發程序。 當頁面顯示時的延遲之後時，它會顯示不有任何學生。

[![Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image43.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image42.png)

執行**講師**頁面，即可確認種子資料已成功在資料庫中插入講師資料。

[![Instructors_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image45.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image44.png)

如同在測試環境中，您會想要驗證的資料庫更新適用於生產環境中，但通常不想要輸入至您的生產資料庫的測試資料。 本教學課程中，您將使用中測試相同的方法。 但在實際的應用程式，您可能想要尋找的方法，驗證該資料庫中更新成功且不會造成測試資料到生產環境資料庫。 在某些應用程式，它可能實際加入一些內容，然後再刪除它。

新增一位學生，然後檢視 您輸入的資料**學生**頁面，確認您可以更新資料庫中的資料。

[![Add_Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image47.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image46.png)

[![Students_page_with_new_student_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image49.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image48.png)

驗證授權規則會正常運作，方法是選取**更新信用額度**從**課程**功能表。 **登入**頁面隨即顯示。 輸入您的系統管理員帳戶認證，請按一下**登入**，而**更新信用額度**頁面隨即顯示。

[![Log_In_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image51.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image50.png)

如果成功，登入**更新信用額度**頁面隨即顯示。 這表示已成功部署 ASP.NET 成員資格資料庫 （含單一系統管理員帳戶）。

[![Update_Credits_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image53.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image52.png)

您現在已成功部署和測試您的網站，它可公開在網際網路上。

## <a name="creating-a-more-reliable-test-environment"></a>建立更可靠的測試環境

中所述[部署到測試環境](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)教學課程中，最可靠的測試環境是在具有一樣的實際執行帳戶的主控提供者的第二個帳戶。 這是成本高於做為測試環境，使用本機 IIS，因為您必須登入第二個裝載帳戶。 但是，如果它讓生產環境網站錯誤或中斷，您可能會決定它是與成本相等。

大部分的建立和部署至測試帳戶的程序會類似於什麼您已經部署到生產環境：

- 建立*Web.config*轉換檔案。
- 在主機服務提供者建立的帳戶。
- 建立新的發行設定檔並部署至測試帳戶。

### <a name="preventing-public-access-to-the-test-site"></a>防止公用存取來測試網站

測試帳戶的一個重要考量是，會即時在網際網路上，但您不想將它公開。 若要將私用網站中，您可以使用一或多個下列方法：

- 請連絡主機服務提供者設定存取至測試的站台僅允許來自您用於測試的 IP 位址的防火牆規則。
- 偽裝成 URL，使它不是類似於公用網站 URL。
- 使用*robots.txt*檔案，以確保，搜尋引擎將不會編目測試站台和報表連結，在搜尋結果中。

這些方法的第一個是很明顯地最安全的但該程序專屬於每個主機服務提供者，並不涵蓋在本教學課程。 如果您不要排列主控提供者，以允許只瀏覽至測試帳戶 URL 的 IP 位址，您理論上不需要擔心搜尋引擎編目它。 但即使在此情況下，部署*robots.txt*檔案會是做為備份是個好主意萬一不慎關閉該防火牆規則。

*Robots.txt*檔案會在您的專案資料夾中，並應該在它有下列文字：

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/samples/sample1.cmd)]

`User-agent`一行告訴檔案中的規則適用於所有搜尋引擎 web 編目程式 （機器人），搜尋引擎和`Disallow`一行可讓您指定應該進行編目的網站上的任何頁面。

您可能想要為您的生產網站，因此您需要從生產環境部署中排除此檔案的搜尋引擎。 若要這樣做，請參閱**我是否能排除特定檔案或資料夾從部署？** 中[ASP.NET Web 應用程式專案部署常見問題集](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment)。 請確定您只針對生產環境的發行設定檔時，才指定排除。

建立第二個裝載帳戶是一種方法使用的測試環境，不需要，但是可能值得額外的費用。 在下列教學課程中，您將繼續使用 IIS 作為測試環境。

在下一個教學課程中，您將更新應用程式程式碼，並將變更部署到測試和生產環境。

> [!div class="step-by-step"]
> [上一頁](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
> [下一頁](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
