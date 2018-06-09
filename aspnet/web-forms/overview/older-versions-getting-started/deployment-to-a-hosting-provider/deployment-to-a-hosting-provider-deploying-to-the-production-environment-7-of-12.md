---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
title: 使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 部署到生產環境-7 個 12 |Microsoft 文件
author: tdykstra
description: 這一系列的教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式專案，其中包含 SQL Server Compact 資料庫使用視覺化 Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: b83ab819-2b05-4776-b7b4-79ef78d457a5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
msc.type: authoredcontent
ms.openlocfilehash: ab3b7ba332deddae7d04fc37c7aabc72bdb2d17e
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/08/2018
ms.locfileid: "30889679"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-the-production-environment---7-of-12"></a>使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 部署到生產環境-7 個 12
====================
由[Tom Dykstra](https://github.com/tdykstra)

[下載入門專案](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> 這一系列的教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式專案，其中包含使用 Visual Studio 2012 RC 或 Visual Studio Express 2012 RC for Web 的 SQL Server Compact 資料庫。 如果您安裝 Web 發行更新，您也可以使用 Visual Studio 2010。 數列的簡介，請參閱[系列的第一個教學課程](deployment-to-a-hosting-provider-introduction-1-of-12.md)。
> 
> 顯示部署 Visual Studio 2012 RC 發行之後，引進的功能，示範如何將 SQL Server Compact 以外的 SQL Server 版本的部署和示範如何將部署至 Azure App Service Web 應用程式的教學課程，請參閱[ASP.NET Web 部署使用 Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md)。


## <a name="overview"></a>總覽

在本教學課程中，主機服務提供者的帳戶設定和部署您的 ASP.NET web 應用程式至生產環境使用 Visual Studio 單鍵發行功能。

提示： 如果您收到錯誤訊息，或當您瀏覽教學課程，項目無法運作，請務必檢查[疑難排解頁面](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)。

## <a name="selecting-a-hosting-provider"></a>選取主控提供者

針對 Contoso 大學應用程式和此教學課程系列，您需要支援 ASP.NET 4 和 Web Deploy 提供者。 特定的控管公司選擇使教學課程可以說明將部署到即時網站的完整的經驗。 每個控管公司提供的不同功能和部署其伺服器的經驗有所不同。 不過，本教學課程所述的程序的整體程序通常會。 本教學課程，Cytanium.com，所使用的主控提供者是其中一個許多可供使用，而且其用途，在本教學課程不會構成作背書或推薦。

當您準備好要選取您自己的主機服務提供者時，您可以比較功能與價格以[的提供者組件庫](https://www.microsoft.com/web/hosting)Microsoft.com/web 站台上。

## <a name="creating-an-account"></a>建立帳戶

在您選取的提供者中建立帳戶。 如果支援完整的 SQL Server 資料庫加入額外，您不需要選取本教學課程，但是您必須為[移轉至 SQL Server](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)教學課程稍後在本系列。

如需這些教學課程中，您不需要註冊新的網域名稱。 您可以測試以確認成功部署使用暫時提供者所指派到網站的 URL。

在建立帳戶之後，您通常會收到包含部署及管理您的網站需要的所有資訊的歡迎電子郵件。 您的主機服務提供者會傳送給您的資訊將會類似於這裡所顯示。 Cytanium 歡迎電子郵件傳送給新的帳戶擁有者包括下列資訊：

- 提供者的控制項面板站台，您可以在其中管理您的網站設定 URL。 識別碼和您指定的密碼包含在這個部分中的 歡迎使用電子郵件，以方便參考。 （兩者都已經變更為示範此圖中的值。）

    [![Welcome_Email_Control_Panel_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image1.png)
- 預設.NET Framework 版本和如何變更它的相關資訊。 許多裝載站台預設值為 2.0，適用於 ASP.NET 應用程式的目標為.NET Framework 2.0、 3.0 或 3.5。 但 Contoso 大學是.NET Framework 4 應用程式，因此您必須變更此設定。 （ASP.NET 4.5 應用程式為您將會使用.NET 4.0 設定）。

    [![Welcome_Email_Framework_Version](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image3.png)
- 暫存 URL 可讓您存取您的網站。 建立此帳戶後，「 contosouniversity.com"輸入做為現有的網域名稱。 因此暫存的 URL 是`http://contosouniversity.com.vserver01.cytanium.com`。

    [![Welcome_Email_Temporary_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image5.png)
- 有關如何設定資料庫和您需要以存取它們的連接字串：

    [![Welcome_Email_Database_Info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image7.png)
- 工具和部署您的站台設定的相關資訊。 （從 Cytanium 電子郵件也會提及 WebMatrix，這裡省略。）

    [![Welcome_Email_Deploy_info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image10.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image9.png)

## <a name="setting-the-net-framework-version"></a>設定.NET Framework 版本

Cytanium 歡迎電子郵件包含有關如何變更.NET Framework 版本的指示的連結。 這些指示會說明這可以在透過 Cytanium 控制台來完成。 其他提供者可控制面板網站，看起來不同，或它們可能會指示您這麼做的方式不同。

移至控制台 URL。 登入您的使用者名稱和密碼之後，您會看到控制台。

[![Cytanium_Control_Panel](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image11.png)

在**裝載空格**，請將指標的 [Web] 圖示並選取**網站**從功能表。

[![Cytanium_Control_Panel_selecting_Web_Sites](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image13.png)

在**網站**方塊中，按一下**contosouniversity.com** （您已建立的帳戶時所使用的站台的名稱）。

[![Cytanium_Control_Panel_selecting_contosouniversity](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image15.png)

在**網站內容**方塊中，選取**延伸** 索引標籤。

[![Cytanium_Control_Panel_Extensions_tab](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image17.png)

變更從 ASP.NET **2.0 整合式的管線**至**4.0 （整合式管線）**，然後按一下 **更新**。

## <a name="publishing-to-the-hosting-provider"></a>發行至主控提供者

歡迎使用電子郵件，主機服務提供者從包含所有必要的設定，您才能將專案發行，您可以手動輸入這些資訊至發行設定檔。 但是您將設定部署給提供者使用容易和較容易發生錯誤的方法： 您將下載 *.publishsettings*檔案並匯入發行設定檔。

在您的瀏覽器，移至 Cytanium 控制台中並選取**Web** ，然後選取 **網站。**

![選取網站的控制台](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image19.png)

選取**contosouniversity.com**網站。

![選取 contosouniversity.com 控制台](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image20.png)

選取**Web 發佈** 索引標籤。

![控制項面板發行 Web 索引標籤](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image21.png)

建立要用於 web 發行輸入使用者名稱和密碼認證。 您可以輸入您用來登入控制台中的相同認證。 然後按一下 **啟用**。

![控制台中建立發行認證](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image22.png)

按一下**這個網站下載發行設定檔**。

![控制項面板下載發行設定檔](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image23.png)

當提示您開啟或儲存檔案時，請將其儲存。

![儲存發行設定檔](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image24.png)

在**方案總管 中**在 Visual Studio 中，以滑鼠右鍵按一下 ContosoUniversity 專案並選取**發行**。 **發行 Web**  對話方塊隨即開啟**預覽**索引標籤與**測試**選取，因為這是您所使用的最後一個設定檔的設定檔。

選取**設定檔**索引標籤，然後按一下 **匯入**。

![發行網站精靈匯入 按鈕](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image25.png)

在**匯入發行設定**對話方塊中，選取 *.publishsettings*檔案您下載，然後按一下**開啟**。 精靈將使用的所有欄位填入都進入 [連線] 索引標籤。

![發行網站精靈連線 索引標籤](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image26.png)

.Publishsettings 檔案置於 [目的地 URL] 方塊中，站台的計劃永久 URL，但您尚未購買該網域，如果值取代為暫存的 URL。 此範例中，URL 是 *[ http://contosouniversity.com.vserver01.cytanium.com ](http://contosouniversity.com.vserver01.cytanium.com)。* 此方塊的唯一目的是指定何種瀏覽器會開啟自動之後成功部署之後的 URL。 如果保留空白，唯一的結果會是瀏覽器不會自動啟動部署之後。

按一下**驗證連線**確認設定正確無誤，而且您可以連接到伺服器。 如稍早所見，綠色的核取記號會驗證是成功的連線。

當您按一下 [驗證連線時，您可能會看到**憑證錯誤**] 對話方塊。 如果您這樣做，請確認伺服器名稱是您所預期。 如果是，選取**儲存這個憑證以供未來 Visual Studio 工作階段**按一下**接受**。 （此錯誤表示主機服務提供者已選擇來避免所購買的 SSL 憑證適用於您要部署的 URL 的費用。 如果您想要使用的有效憑證來建立安全連接，請連絡您的主機服務提供者。）

![憑證錯誤](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image27.png)

按 [ **下一步**]。

在**資料庫**區段**設定**索引標籤上，輸入相同的測試輸入值的發行設定檔。 在下拉式清單中，您會發現您需要的連接字串。

- 在連接字串 方塊的**SchoolContext，** 選取 `Data Source=|DataDirectory|School-Prod.sdf`
- 在下**SchoolContext**，選取**套用 Code First 移轉**。
- 在連接字串 方塊的**DefaultConnection**，請選取 `Data Source=|DataDirectory|aspnet-Prod.sdf`
- 在下**DefaultConnection**，保留**更新資料庫**清除。

![發行網站精靈設定 索引標籤](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image28.png)

按 [ **下一步**]。

在**預覽**索引標籤上，按一下 **啟動預覽**若要查看將複製的檔案清單。 您會看到相同之前看到當您部署至 IIS 在本機電腦上的清單。

在發行之前，變更設定檔的名稱，使您 Web.Production.config 轉換檔案將會套用。 選取**設定檔**索引標籤上，按一下 **管理設定檔**。

![發行網站精靈管理設定檔](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image29.png)

在**編輯 Web 發行設定檔**對話方塊方塊中，選取實際執行設定檔中，按一下**重新命名**，並將設定檔名稱變更為生產環境。 然後按一下 **關閉**。

![編輯 Web 發行設定檔 對話方塊](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image30.png)

按一下 [發行] 。

應用程式發行至主控提供者。 結果會顯示在**輸出**視窗。

![在部署後的 [輸出] 視窗](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image31.png)

您在輸入的 url 會自動開啟瀏覽器**目的地 URL**方塊上**連接** 索引標籤**發行 Web**精靈。 您會看到相同的首頁上，做為當您執行站台在 Visual Studio 中，但現在是沒有 「 （測試） 」 或 「 （開發） 」 的標題列中的環境指標。 這表示環境指標*Web.config*轉換正常運作。

> [!NOTE]
> 如果您仍然可以看到 「 （測試） 」 標題中，刪除*obj*從 ContosoUniversity 專案及重新部署的資料夾。 在發行前版本的軟體，先前套用的轉換檔 (Web.Test.config) 可能會取得重新套用雖然使用實際執行設定檔。


[![Home_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image33.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image32.png)

執行會導致資料庫存取的頁面之前，請確定 Elmah 能夠記錄就會發生任何錯誤。

## <a name="setting-folder-permissions-for-elmah"></a>設定 Elmah 資料夾權的限

當您從上一個教學課程，在這一系列請記住，您必須確定應用程式有 Elmah 儲存錯誤記錄檔中您的應用程式資料夾的寫入權限。 當您部署至 IIS 在本機電腦上時，會以手動方式設定這些權限。 本節中，您將了解如何在 Cytanium 設定權限。 （有些主控提供者不能讓您執行這項操作，可能提供了一個或多個預先定義的資料夾，具有寫入權限。 在此情況下您必須修改應用程式以使用指定的資料夾。）

您可以在 Cytanium 控制台中設定資料夾權限。 請移至控制項面板 URL，然後選取**檔案管理員**。

[![Cytanium_Control_Panel_with_File_Manager_selected](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image35.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image34.png)

在**檔案管理員**方塊中，選取**contosouniversity.com**然後**wwwrooot**若要查看應用程式的根資料夾。 按一下掛鎖圖示旁**Elmah**。

[![Cytanium_Control_Panel_File_Manager_at_root_folder](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image37.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image36.png)

在**檔案**/**資料夾權限**視窗中，選取**讀取**和**寫入**的核取方塊**contosouniversity.com**按一下**設定權限**。

[![Cytanium_Control_Panel_File_Folder_Permissions_Elmah](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image39.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image38.png)

請確定 Elmah 具有寫入權限*Elmah*導致錯誤發生，並顯示 Elmah 錯誤報告的資料夾。 要求無效的 URL，例如*Studentsxxx.aspx*。 如往常一般，您會看到*GenericErrorPage.aspx*頁面。 按一下**登出**連結，然後再執行*Elmah.axd*。 您取得**登入**首先，頁面會驗證*Web.config*轉換已成功加入 Elmah 授權。 登入之後，您會看到顯示您剛才造成錯誤的報表。

[![Elmah.axd_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image41.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image40.png)

## <a name="testing-in-the-production-environment"></a>在實際執行環境中測試

執行**學生**頁面。 應用程式嘗試存取 School 資料庫第一次，Code First 移轉來建立資料庫觸發程序。 當此頁面會顯示目前的延遲之後時，它會顯示有沒有學生。

[![Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image43.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image42.png)

執行**講師**頁面，即可確認種子資料已成功在資料庫中插入講師資料。

[![Instructors_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image45.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image44.png)

您可以如同在測試環境中，您會想要確認資料庫更新實際執行環境中運作，但通常不想將測試資料輸入您的生產資料庫。 此教學課程中，您將使用中測試相同的方法。 但是在實際的應用程式，您可能想要尋找的方法，會驗證該資料庫更新成功且不會造成到實際執行資料庫的測試資料。 在某些應用程式，可能適合加入一些內容，然後再刪除它。

新增一位學生，然後再檢視您在輸入資料**學生**頁面，即可確認您可以更新資料庫中的資料。

[![Add_Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image47.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image46.png)

[![Students_page_with_new_student_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image49.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image48.png)

驗證授權規則是否正常運作所選取**更新信用額度**從**課程**功能表。 **登入**頁面隨即顯示。 輸入您的系統管理員帳戶認證，請按一下**登入**，而**更新信用額度**頁面隨即顯示。

[![Log_In_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image51.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image50.png)

如果成功，登入**更新信用額度**頁面隨即顯示。 這表示已成功部署 ASP.NET 成員資格資料庫 （使用單一的系統管理員帳戶）。

[![Update_Credits_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image53.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image52.png)

您現在已成功部署和測試您的網站，而且已使用可公開在網際網路上。

## <a name="creating-a-more-reliable-test-environment"></a>建立更可靠的測試環境

中所述[部署到測試環境](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)教學課程中，最可靠的測試環境必須是具有一樣生產帳戶主控提供者上的第二個帳戶。 這會是成本高於使用本機 IIS 做為您的測試環境，因為您必須登入第二個裝載帳戶。 但是，如果它會防止生產網站錯誤或中斷情形，您可能會決定是否值得成本。

大部分的建立及部署至測試帳戶的程序是類似什麼您已經部署到生產環境：

- 建立*Web.config*轉換檔案。
- 主控提供者上建立帳戶。
- 建立新的發行設定檔，並部署至測試帳戶。

### <a name="preventing-public-access-to-the-test-site"></a>防止測試站台的公用存取

測試帳戶的一個重要考量是會即時在網際網路上，但您不想使用它公開。 要保密的站台，您可以使用一或多個下列方法：

- 請連絡主機服務提供者設定允許只從您用於測試的 IP 位址測試網站存取的防火牆規則。
- 偽裝成 URL，使它不是類似於公用網站的 URL。
- 使用*robots.txt*檔案，以確保搜尋引擎在搜尋結果中，，不編目，測試站台和報表連結。

這些方法的第一個是很明顯地，最不安全，但該程序專屬於每個主機服務提供者並不會涵蓋在本教學課程。 如果您不要排列您裝載的提供者，允許只瀏覽至測試帳戶 URL 的 IP 位址，您理論上不需要擔心搜尋引擎編目它。 但是即使在此情況下，部署*robots.txt*檔案，最好以當作備份以防該防火牆規則曾經意外關閉。

*Robots.txt*檔案則是放在您的專案資料夾，以及應該在它有下列文字：

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/samples/sample1.cmd)]

`User-agent`一行告訴檔案中的規則套用至所有搜尋引擎網頁自動尋檢 (robots)，搜尋引擎和`Disallow`一行指定應該進行編目的網站上的任何頁面。

您可能想搜尋引擎目錄生產網站，因此您需要從生產環境部署中排除此檔案。 若要這樣做，請參閱**可以我排除特定檔案或資料夾從部署嗎？** 中[ASP.NET Web 應用程式專案部署常見問題集](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment)。 請確定您指定在排除，只會針對實際執行發行設定檔。

建立第二個裝載帳戶是一種方法使用的測試環境，不是必要，但是可能值得額外的費用。 在下列的教學課程中，您將會繼續使用 IIS 做為您的測試環境。

下一個教學課程中，您將更新應用程式程式碼，並將您的變更部署到測試和生產環境。

> [!div class="step-by-step"]
> [上一頁](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
> [下一頁](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
