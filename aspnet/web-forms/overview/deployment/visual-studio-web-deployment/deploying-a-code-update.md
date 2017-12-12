---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: "使用 Visual Studio 的 ASP.NET Web 部署： 部署程式碼更新 |Microsoft 文件"
author: tdykstra
description: "此教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web 應用程式或協力廠商裝載提供者，使用..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: 10da2b5013ae1348b69ea4f456d81bb4c4b73df6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a>使用 Visual Studio 的 ASP.NET Web 部署： 部署程式碼更新
====================
由[Tom Dykstra](https://github.com/tdykstra)

[下載入門專案](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> 此教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web 應用程式或協力廠商裝載提供者，使用 Visual Studio 2012 或 Visual Studio 2010。 數列的相關資訊，請參閱[系列的第一個教學課程](introduction.md)。


## <a name="overview"></a>概觀

初始部署之後，您的工作，維護及開發您的網站會繼續，而且之前長時間中，您會想要部署更新。 本教學課程會帶領您完成將更新部署到您的應用程式程式碼的程序。 在實作和部署本教學課程中的更新並不包含資料庫變更。您會看到有關部署資料庫變更下一個教學課程中的不同。

提示： 如果您收到錯誤訊息，或當您瀏覽教學課程，項目無法運作，請務必檢查[疑難排解頁面](troubleshooting.md)。

## <a name="make-a-code-change"></a>變更程式

簡單舉例如下的更新您的應用程式，您會加入至**講師**頁面上所選取講師教導課程的清單。

如果您執行**講師** 頁面上，您會發現有**選取**連結在方格中，但他們不會做讓以外的任何資料列的背景開啟灰色。

![講師頁面與選取項目](deploying-a-code-update/_static/image1.png)

現在您要加入時執行的程式碼**選取**連結按一下，並顯示一份所選講師教導的課程。

1. 在*Instructors.aspx*，加入下列標記之後立即**ErrorMessageLabel** `Label`控制項：

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. 執行網頁，然後選取 instructor。 您會看到一份由該講師教導的課程。

    ![課程教導講師頁面](deploying-a-code-update/_static/image2.png)
3. 關閉瀏覽器。

## <a name="deploy-the-code-update-to-the-test-environment"></a>程式碼更新部署至測試環境

您可以使用您的發行設定檔部署到測試、 預備及生產環境之前，您需要變更資料庫的發行選項。 您不再需要執行的成員資格資料庫授與和資料部署指令碼。

1. 開啟**發行 Web** ContosoUniversity 專案上按一下滑鼠右鍵，然後按一下精靈**發行**。
2. 按一下**測試**中程式碼剖析**設定檔**下拉式清單。
3. 按一下**設定** 索引標籤。
4. 在下**DefaultConnection**中**資料庫**區段中，清除**更新資料庫**核取方塊。
5. 按一下**設定檔**索引標籤，然後再按一下**臨時**中程式碼剖析**設定檔**下拉式清單。
6. 當詢問您是否要儲存的變更**測試**設定檔，請按一下**是**。
7. 暫存設定檔中進行相同變更。
8. 重複此程序中的實際執行設定檔進行相同變更。
9. 關閉**發行 Web**精靈。

現在再執行一種單鍵的簡單方式發行是部署到測試環境。 若要讓此程序的速度較快，您可以使用**Web 一個按一下 Publish**工具列。

1. 在**檢視**功能表上，選擇**工具列**，然後選取  **Web 一個按一下 Publish**。

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. 在**方案總管 中**，選取 ContosoUniversity 專案。
3. **Web 一個按一下 Publish**工具列上，選擇**測試**發行設定檔，然後按一下 **發行 Web** （具有指向左、 右的箭號圖示）。

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. Visual Studio 會部署更新的應用程式和瀏覽器會自動開啟至首頁。
5. 執行講師頁面並選取 instructor 以確認更新已成功部署。

您平常也執行迴歸測試 （也就是說，測試以確定新的變更不會中斷任何現有功能的站台的其餘部分）。 但本教學課程中您會略過該步驟，並繼續進行更新部署至預備和生產環境。

當您重新部署時，Web Deploy，自動決定哪些檔案已變更，只有複本變更為伺服器的檔案。 根據預設，Web Deploy 使用上次變更日期檔案來判斷哪些已經變更。 當您不要變更檔案內容，某些原始檔控制系統將變更檔案甚至的日期。 在此情況下，您可能想要設定 Web Deploy 來使用檔案總和檢查碼，以判斷哪些檔案已變更。 如需詳細資訊，請參閱[為什麼執行我的所有檔案取得重新部署雖然並沒有變更它們？](https://msdn.microsoft.com/en-us/library/ee942158.aspx#use_checksum)中 ASP.NET 部署常見問題集。

## <a name="take-the-application-offline-during-deployment"></a>應用程式離線在部署期間

您要部署的變更現在是簡單的變更，以在單一頁面。 但有時候您部署較大的變更，或部署的程式碼和資料庫的變更，以及站台可能不正確地運作，如果使用者在完成部署之前要求的頁面。 若要防止使用者存取站台部署正在進行時，您可以使用*應用程式\_offline.htm*檔案。 當您將檔案放名為*應用程式\_offline.htm*在應用程式的根資料夾中，IIS 會自動顯示該檔案，而不是執行您的應用程式。 因此若要防止存取在部署期間，您將放*應用程式\_offline.htm*根資料夾中，在執行部署程序中，然後再移除*應用程式\_offline.htm*後成功部署。

您可以設定自動在 預設的 Web Deploy*應用程式\_offline.htm*啟動部署時，檔案伺服器上，並完成時將它移除。 若要執行所有您可執行是將下列 XML 項目加入至您的發行設定檔 (.pubxml) 檔：

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

在此教學課程中，您會看到如何建立及使用自訂*應用程式\_offline.htm*檔案。

使用*應用程式\_offline.htm*預備網站中不必要的因為您不需要使用者存取的預備網站。 但是，最好使用暫存的測試所有您計劃在生產環境中部署的方式。

### <a name="create-appofflinehtm"></a>建立應用程式\_offline.htm

1. 在**方案總管] 中**，以滑鼠右鍵按一下方案，然後按一下**新增**，然後按一下 [**新項目**。
2. 建立**HTML 網頁**名為*應用程式\_offline.htm* (在刪除最後一個"l" *.html* Visual Studio 會建立預設的延伸模組)。
3. 以下列標記取代範本標記：

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. 儲存並關閉檔案。

### <a name="copy-appofflinehtm-to-the-root-folder-of-the-web-site"></a>複製應用程式\_offline.htm 網站的根資料夾

您可以使用任何 FTP 工具，將檔案複製到網站。 [FileZilla](http://filezilla-project.org/)是常用的 FTP 工具和螢幕擷取畫面所示。

若要使用 FTP 工具，您需要三件事： FTP URL、 使用者名稱和密碼。

URL 顯示在 Azure 管理入口網站的網站的儀表板頁面上，且可以在中找到的使用者名稱和密碼的 FTP *.publishsettings*您之前下載的檔案。 下列步驟示範如何取得這些值。

1. 在 Azure 管理入口網站中，按一下**網站**索引標籤，然後按一下 預備網站。
2. 在**儀表板**頁面上，捲動以尋找名稱中的 FTP 主機**快速概覽**> 一節。

    ![FTP 主機名稱](deploying-a-code-update/_static/image5.png)
3. 開啟暫存*.publishsettings*在記事本或其他文字編輯器中的檔案。
4. 尋找`publishProfile`FTP 設定檔的項目。
5. 複製`userName`和`userPWD`值。

    ![FTP 認證](deploying-a-code-update/_static/image6.png)
6. 開啟您的 FTP 工具和登入 FTP URL。
7. 複製*應用程式\_offline.htm*從方案資料夾*/站台/wwwroot*資料夾中的預備網站。

    ![複製 app_offline](deploying-a-code-update/_static/image7.png)
8. 瀏覽至您的預備網站 URL。 您會看到*應用程式\_offline.htm*頁面現在會顯示而不是您的首頁。

    ![在瀏覽器視窗 app_offline.htm](deploying-a-code-update/_static/image8.png)

現在您已經準備好要部署到預備環境。

## <a name="deploy-the-code-update-to-staging-and-production"></a>程式碼更新部署至預備和生產環境

1. 在**Web 一個按一下 Publish**工具列上，選擇**臨時**發行設定檔，然後按一下 **發行 Web**。

    Visual Studio 部署更新的應用程式，並開啟瀏覽器以網站的首頁。 *應用程式\_offline.htm*檔案會顯示。 您可以測試以確認成功部署之前，您必須移除*應用程式\_offline.htm*檔案。
2. 返回至您的 FTP 工具，並刪除**應用程式\_offline.htm**從預備網站。
3. 在瀏覽器中開啟講師頁面中的預備網站，並選取講師以確認更新已成功部署。
4. 您對暫存依照實際執行相同的程序。

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a>檢閱修改和部署特定的檔案

Visual Studio 2012 也可讓您部署的個別檔案的能力。 選取的檔案中，您可以檢視本機版本和部署的版本之間的差異，將檔案部署到目的地環境中，或從目的地環境的檔案複製到局部的專案。 本章節的教學課程中，您會了解如何使用這些功能。

### <a name="make-a-change-to-deploy"></a>若要部署變更

1. 開啟*Content/Site.css*，並尋找的區塊`body`標記。
2. 值變更`background-color`從`#fff`至`darkblue`。

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a>在發行預覽視窗中檢視變更

當您使用**發行 Web**精靈以發行專案中，您可以查看變更會移至發行中按兩下**預覽**視窗。

1. ContosoUniversity 專案上按一下滑鼠右鍵，然後按一下**發行**。
2. 從**設定檔**下拉式清單中，選取**測試**發行設定檔。
3. 按一下**預覽**，然後按一下 **啟動預覽**。
4. 在**預覽** 窗格中，按兩下**Site.css**。

    ![按兩下 Site.css](deploying-a-code-update/_static/image9.png)

    **預覽變更**對話方塊會顯示預覽將部署的變更。

    ![Site.css 預覽變更](deploying-a-code-update/_static/image10.png)

    如果您按兩下*Web.config*檔案，**預覽變更**對話方塊會顯示您的組建的效果組態轉換和發行設定檔的轉換。 此時您已沒有做任何事，可能導致*Web.config*變更，您會不看到任何變更，所以在伺服器上的檔案。 不過，**預覽變更**視窗不正確地顯示兩個變更。 您似乎將移除兩個 XML 項目。 當您選取這些項目加入發行流程所**執行 Code First 移轉在應用程式啟動**第一個程式碼的內容類別。 發佈程序會新增這些項目，讓它看起來像它們正在移除雖然它們不會移除之前進行比較。 未來的版本中，將會更正這個錯誤。
5. 按一下 [ **關閉**]。
6. 按一下 [發行] 。
7. 當瀏覽器會開啟至測試網站的首頁上時，按 CTRL + F5，讓硬碟重新整理以查看 CSS 變更的效果。

    ![CSS 變更的效果](deploying-a-code-update/_static/image11.png)
8. 關閉瀏覽器。

### <a name="publish-specific-files-from-solution-explorer"></a>發行特定的檔案，從 方案總管

假設您不喜歡的藍色背景，且想要還原為原始的色彩。 本節中，您會還原原始設定發行特定的檔案直接從**方案總管 中**。

1. 開啟*Content/Site.css*和還原`background-color`設`#fff`。
2. 在**方案總管 中**，以滑鼠右鍵按一下*Content/Site.css*檔案。

    內容功能表會顯示三個發行選項。

    ![發行選項，從 方案總管](deploying-a-code-update/_static/image12.png)
3. 按一下**預覽變更為 Site.css**。

    視窗會開啟，顯示目的地環境中的本機檔案和它的版本之間的差異。

    ![Diff-內容/Site.css](deploying-a-code-update/_static/image13.png)
4. 在**方案總管 中**，以滑鼠右鍵按一下**Site.css**再次按一下**發行 Site.css**。

    **Web 發行活動** 視窗會顯示已發行的檔案。

    ![Web 發行活動視窗](deploying-a-code-update/_static/image14.png)
5. 開啟瀏覽器並前往`http://localhost/contosouniversity`URL，然後按下 CTRL + F5 鍵會造成一種硬式重新整理才能看到變更 CSS 的效果。

    ![使用標準 CSS 的首頁](deploying-a-code-update/_static/image15.png)
6. 關閉瀏覽器。

## <a name="summary"></a>總結

您現在已看過幾種方式可以部署應用程式更新未牽涉到資料庫變更，以及您已經看到如何預覽來確認更新項目是您所預期的變更。 講師頁面現在具有**課程教導**> 一節。

![課程教導講師頁面](deploying-a-code-update/_static/image16.png)

下一個教學課程會示範如何部署資料庫變更： 資料庫和講師頁面，您將新增的出生日期 欄位。

>[!div class="step-by-step"]
[上一頁](deploying-to-production.md)
[下一頁](deploying-a-database-update.md)
