---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: 使用 Visual Studio 的 ASP.NET Web 部署： 部署程式碼更新 |Microsoft Docs
author: tdykstra
description: 本系列教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web Apps 或協力廠商裝載提供者，使用...
ms.author: aspnetcontent
ms.date: 02/15/2013
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: c058d4314ea0bed87e22c0e6b3fa09b89b2423d9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833351"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a>使用 Visual Studio 的 ASP.NET Web 部署： 部署程式碼更新
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

[下載入門專案](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> 本系列教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web Apps 或協力廠商裝載提供者，使用 Visual Studio 2012 或 Visual Studio 2010。 這個系列的相關資訊，請參閱[系列的第一個教學課程](introduction.md)。


## <a name="overview"></a>總覽

初始部署之後，您的工作，維護及開發您的網站會繼續，而且之前長時間中，您會想要將更新部署。 本教學課程會帶領您完成將更新部署到您的應用程式程式碼的程序。 會實作，以及在本教學課程中部署的更新並不包含資料庫變更;您會看到有什麼不一樣將在下一個教學課程中的資料庫變更部署。

提醒： 如果您收到錯誤訊息，或當您瀏覽本教學課程，項目無法運作，請務必檢查[疑難排解頁面](troubleshooting.md)。

## <a name="make-a-code-change"></a>變更的程式碼

做為更新的簡單範例，用您的應用程式，您會加入至**講師**頁面上選取的講師所教授的課程清單。

如果您執行**講師**頁面上，您會發現有**選取**連結在方格中，但它們不進行以外進行的任何資料列背景開啟灰色。

![選取的講師頁面](deploying-a-code-update/_static/image1.png)

現在您將新增時執行的程式碼**選取**連結按一下，並顯示一份所選講師所教授的課程。

1. 在  *Instructors.aspx*，加入下列標記的後面**ErrorMessageLabel** `Label`控制項：

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. 執行網頁，然後選取講師。 您會看到一份該講師所教授的課程。

    ![教授課程講師頁面](deploying-a-code-update/_static/image2.png)
3. 關閉瀏覽器。

## <a name="deploy-the-code-update-to-the-test-environment"></a>將程式碼更新部署到測試環境

您可以使用您的發行設定檔部署至測試、 預備及生產環境之前，您需要變更資料庫的發行選項。 您不再需要執行的 grant 和資料部署指令碼的成員資格資料庫。

1. 開啟**發佈 Web**精靈，以滑鼠右鍵按一下 ContosoUniversity 專案，然後按一下**發佈**。
2. 按一下 **測試**中程式碼剖析**設定檔**下拉式清單。
3. 按一下 [**設定**] 索引標籤。
4. 底下**DefaultConnection**中**資料庫**區段中，清除**更新資料庫**核取方塊。
5. 按一下 **設定檔**索引標籤，然後再按一下**預備**中程式碼剖析**的設定檔**下拉式清單。
6. 當詢問您是否要儲存所做的變更**測試**設定檔，請按一下 **是**。
7. 暫存設定檔中進行相同變更。
8. 重複的程序中的實際執行設定檔進行相同變更。
9. 關閉**發佈 Web**精靈。

現在只需執行一種單鍵發行一次是部署到測試環境。 若要讓這個程序更快速，您可以使用**Web 單鍵發佈**工具列。

1. 在 **檢視**功能表上，選擇**工具列**，然後選取**Web 單鍵發佈**。

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. 在 **方案總管 中**，選取 ContosoUniversity 專案。
3. **Web 單鍵發佈**工具列上，選擇**測試**發行設定檔，然後按一下 **發佈 Web** （指向左、 右的箭號圖示）。

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. Visual Studio 會部署更新的應用程式和瀏覽器會自動開啟至首頁。
5. 執行 Instructors 頁面，然後選取講師，以確認更新已成功部署。

您通常也進行迴歸測試 （也就是測試以確定新的變更沒有破壞任何現有功能的站台的其餘部分）。 但本教學課程中，您會略過該步驟並繼續將更新部署到預備和生產環境。

當您重新部署時，Web Deploy 會自動決定哪些檔案已變更，只複製變更檔案伺服器。 根據預設，Web Deploy 使用上次變更日期檔案以判斷哪些已變更。 某些原始檔控制系統時您不會變更檔案內容，變更檔案，甚至是日期。 在此情況下，您可能要設定 Web Deploy，用以判斷哪些檔案已變更的檔案總和檢查碼。 如需詳細資訊，請參閱 <<c0> [ 為什麼執行我的所有檔案重新部署未變更它們雖然？](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) ASP.NET 部署常見問題集。

## <a name="take-the-application-offline-during-deployment"></a>將應用程式離線部署期間

您要部署的變更現在是簡單的變更至單一頁面。 但有時候部署較大的變更，或部署程式碼和資料庫的變更，以及站台可能不正確地運作，如果使用者在完成部署之前要求的頁面。 若要防止使用者存取站台，在進行部署時，您可以使用*應用程式\_offline.htm*檔案。 當您將名為的檔案*應用程式\_offline.htm*在您的應用程式根資料夾中，IIS 會自動顯示該檔案，而不是執行您的應用程式。 因此若要防止存取在部署期間，您將放*應用程式\_offline.htm*根資料夾中，在執行部署程序，然後再移除*應用程式\_offline.htm*後成功部署。

您可以設定自動將預設的 Web Deploy*應用程式\_offline.htm*啟動部署時，在伺服器上檔案，並完成時移除它。 若要執行，所以您必須執行是將下列 XML 項目新增至您的發行設定檔 (.pubxml) 檔案：

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

在本教學課程中，您會看到如何建立及使用自訂*應用程式\_offline.htm*檔案。

使用*應用程式\_offline.htm*預備網站中不必要的因為您不需要存取預備網站的使用者。 但是，最好使用暫存的測試所有您打算在生產環境中部署的方式。

### <a name="create-appofflinehtm"></a>建立應用程式\_offline.htm

1. 在 **方案總管 中**，以滑鼠右鍵按一下方案，然後按一下**新增**，然後按一下 **新項目**。
2. 建立**HTML 網頁**名為*應用程式\_offline.htm* (在刪除最後一個"l" *.html* Visual Studio 會建立預設的延伸模組)。
3. 樣板的標記取代為下列標記：

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. 儲存並關閉檔案。

### <a name="copy-appofflinehtm-to-the-root-folder-of-the-web-site"></a>複製應用程式\_offline.htm 網站的根資料夾

您可以使用任何 FTP 工具將檔案複製到網站。 [FileZilla](http://filezilla-project.org/)是熱門的 FTP 工具和螢幕擷取畫面所示。

若要使用 FTP 工具，您需要三件事： FTP URL、 使用者名稱和密碼。

URL 會顯示在 Azure 管理入口網站的網站的儀表板頁面上，而且在中，則可以找到的使用者名稱和密碼的 FTP *.publishsettings*您稍早下載的檔案。 下列步驟示範如何取得這些值。

1. 在 Azure 管理入口網站中，按一下**網站**索引標籤，然後按一下 預備網站。
2. 在 **儀表板**頁面上，捲動以尋找中的 FTP 主機名稱**快速概覽**一節。

    ![FTP 主機名稱](deploying-a-code-update/_static/image5.png)
3. 開啟預備 *.publishsettings*在記事本或其他文字編輯器中的檔案。
4. 尋找`publishProfile`FTP 設定檔的項目。
5. 複製`userName`和`userPWD`值。

    ![FTP 認證](deploying-a-code-update/_static/image6.png)
6. 開啟您的 FTP 工具並登入 FTP URL。
7. 複製*應用程式\_offline.htm*從解決方案資料夾 */site/wwwroot*預備網站中的資料夾。

    ![複製 app_offline](deploying-a-code-update/_static/image7.png)
8. 瀏覽至您的預備網站 URL。 您會看到*應用程式\_offline.htm*頁面現在會顯示而不是您的首頁。

    ![在瀏覽器視窗的 app_offline.htm](deploying-a-code-update/_static/image8.png)

您現在已準備好部署至預備環境。

## <a name="deploy-the-code-update-to-staging-and-production"></a>將程式碼更新部署至預備和生產環境

1. 在  **Web 單鍵發佈**工具列上，選擇**預備**發行設定檔，然後按一下 **發佈 Web**。

    Visual Studio 會部署更新的應用程式和瀏覽器開啟至網站的首頁。 *應用程式\_offline.htm*檔案會顯示。 您可以測試以確認成功部署之前，必須先移除*應用程式\_offline.htm*檔案。
2. 返回至您的 FTP 工具，並刪除**應用程式\_offline.htm**從預備網站。
3. 在瀏覽器中，開啟 Instructors 頁面在預備網站中，然後選取講師，以確認更新已成功部署。
4. 如您所做預備環境，請遵循相同的程序，用於生產環境。

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a>正在檢閱變更，並部署特定的檔案

Visual Studio 2012 也讓您能夠部署個別的檔案。 選取的檔案中，您可以檢視本機版本與已部署的版本之間的差異、 將檔案部署到目的地環境中，或從目的地環境的檔案複製到本機的專案。 在本教學課程的這一節中，您會看到如何使用這些功能。

### <a name="make-a-change-to-deploy"></a>若要部署變更

1. 開啟*Content/Site.css*，並尋找的區塊`body`標記。
2. 值變更`background-color`從`#fff`至`darkblue`。

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a>在 [發佈預覽] 視窗檢視變更

當您使用**發佈 Web**精靈來發行專案中，您可以看到變更由按兩下該檔案中的發行的移**預覽**視窗。

1. 以滑鼠右鍵按一下 ContosoUniversity 專案，然後按一下 **發佈**。
2. 從**設定檔**下拉式清單中，選取**測試**發行設定檔。
3. 按一下  **Preview**，然後按一下**開始預覽**。
4. 在  **Preview**窗格中，按兩下**Site.css**。

    ![按兩下 Site.css](deploying-a-code-update/_static/image9.png)

    **預覽變更**對話方塊會顯示預覽將會部署變更。

    ![Site.css 預覽變更](deploying-a-code-update/_static/image10.png)

    如果您按兩下*Web.config*檔案，**預覽變更**對話方塊會顯示您的組建的效果組態轉換及發行設定檔轉換。 此時您還沒做任何項目會導致*Web.config*檔案伺服器上用來變更，因此您會不看到任何變更。 不過，**預覽變更**視窗未正確地顯示兩項變更。 看起來將會移除兩個 XML 項目。 當您選取這些項目會新增在發行程序**Execute Code First Migrations 在應用程式啟動**Code First 的內容類別。 比較完成發行程序會新增這些項目，讓它看起來像是他們正在移除雖然它們不會移除之前。 在未來版本中，將會更正這個錯誤。
5. 按一下 [ **關閉**]。
6. 按一下 [發行] 。
7. 當瀏覽器中開啟測試網站的首頁上時，按下 CTRL + F5，導致硬碟的重新整理以查看 CSS 變更的影響。

    ![CSS 變更的影響](deploying-a-code-update/_static/image11.png)
8. 關閉瀏覽器。

### <a name="publish-specific-files-from-solution-explorer"></a>將特定的檔案，從 方案總管發佈

假設您不喜歡的藍色背景並想要還原為原始的色彩。 在本節中您會還原原始設定，藉由發行特定的檔案，直接從**方案總管 中**。

1. 開啟*Content/Site.css* ，並還原`background-color`設為  `#fff`。
2. 在 **方案總管**，以滑鼠右鍵按一下*Content/Site.css*檔案。

    操作功能表會顯示三個發行選項。

    ![發行選項，從 方案總管](deploying-a-code-update/_static/image12.png)
3. 按一下 **預覽變更為 Site.css**。

    視窗會開啟顯示目的地環境中的本機檔案和其版本之間的差異。

    ![Diff-Content/Site.css](deploying-a-code-update/_static/image13.png)
4. 在**方案總管 中**，以滑鼠右鍵按一下**Site.css**再按一下**發行 Site.css**。

    **Web 發行活動**視窗會顯示已發行的檔案。

    ![Web 發行活動時段](deploying-a-code-update/_static/image14.png)
5. 開啟瀏覽器並前往`http://localhost/contosouniversity`URL] 和 [重新整理以查看變更的 CSS 效果然後按 CTRL + F5 鍵會造成一種硬式。

    ![使用一般的 CSS 的首頁](deploying-a-code-update/_static/image15.png)
6. 關閉瀏覽器。

## <a name="summary"></a>總結

您現在已了解幾種方式部署應用程式更新未牽涉到資料庫變更，以及您已了解如何預覽來確認更新項目是您所預期的變更。 Instructors 頁面現在具有**教授課程**一節。

![教授課程講師頁面](deploying-a-code-update/_static/image16.png)

下一個教學課程會示範如何將資料庫變更部署： 對資料庫和 Instructors 頁面，您將新增出生日期欄位。

> [!div class="step-by-step"]
> [上一頁](deploying-to-production.md)
> [下一頁](deploying-a-database-update.md)
