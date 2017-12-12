---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
title: "使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 部署 Code-Only 更新-12 個 8 |Microsoft 文件"
author: tdykstra
description: "這一系列的教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式專案，其中包含 SQL Server Compact 資料庫使用視覺化 Stu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: ddf6252f-9413-4c0c-a360-2cef8d231717
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
msc.type: authoredcontent
ms.openlocfilehash: a07ac968482866ba9a4eca25722d99fd641080e6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-code-only-update---8-of-12"></a>使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 部署 Code-Only 更新-12 個 8
====================
由[Tom Dykstra](https://github.com/tdykstra)

[下載入門專案](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> 這一系列的教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式專案，其中包含使用 Visual Studio 2012 RC 或 Visual Studio Express 2012 RC for Web 的 SQL Server Compact 資料庫。 如果您安裝 Web 發行更新，您也可以使用 Visual Studio 2010。 數列的簡介，請參閱[系列的第一個教學課程](deployment-to-a-hosting-provider-introduction-1-of-12.md)。
> 
> 顯示部署 Visual Studio 2012 RC 發行之後，引進的功能，示範如何將 SQL Server Compact 以外的 SQL Server 版本的部署和示範如何將部署至 Azure App Service Web 應用程式的教學課程，請參閱[ASP.NET Web 部署使用 Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md)。


## <a name="overview"></a>概觀

初始部署之後，您的工作，維護及開發您的網站會繼續，而且之前長時間中，您會想要部署更新。 本教學課程會帶領您完成將更新部署到您的應用程式程式碼的程序。 此更新並不包含資料庫變更。您會看到有關部署資料庫變更下一個教學課程中的不同。

提示： 如果您收到錯誤訊息，或當您瀏覽教學課程，項目無法運作，請務必檢查[疑難排解頁面](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)。

## <a name="making-a-code-change"></a>進行程式碼變更

簡單舉例如下的更新您的應用程式，您會加入至**講師**頁面上所選取講師教導課程的清單。

如果您執行**講師** 頁面上，您會發現有**選取**連結在方格中，但他們不會做讓以外的任何資料列的背景開啟灰色。

[![Instructors_page](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image1.png)

現在您要加入時執行的程式碼**選取**連結按一下，並顯示一份所選講師教導的課程。

在*Instructors.aspx*，加入下列標記之後立即**ErrorMessageLabel** `Label`控制項：

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample1.aspx)]

執行網頁，然後選取 instructor。 您會看到一份由該講師教導的課程。

[![Instructors_page_with_courses](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image3.png)

## <a name="deploying-the-code-update-to-the-test-environment"></a>將程式碼更新部署到測試環境

部署到測試環境是簡單的 執行一種單鍵發行一次。 若要讓此程序的速度較快，您可以使用**Web 一個按一下 Publish**工具列。

在**檢視**功能表上，選擇**工具列**，然後選取  **Web 一個按一下 Publish**。

![Selecting_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image5.png)

在**方案總管 中**，選取 ContosoUniversity 專案。

**Web 一個按一下 Publish**工具列上，選擇**測試**發行設定檔，然後按一下 **發行 Web** （具有指向左、 右的箭號圖示）。

![Web_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image6.png)

Visual Studio 會部署更新的應用程式和瀏覽器會自動開啟至首頁。 執行講師頁面並選取 instructor 以確認更新已成功部署。

[![Instructors_page_with_courses_Test](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image7.png)

您平常也執行迴歸測試 （也就是說，測試以確定新的變更不會中斷任何現有功能的站台的其餘部分）。 但本教學課程中，您會略過該步驟並繼續進行到生產環境部署更新。

## <a name="preventing-redeployment-of-the-initial-database-state-to-production"></a>防止重新部署到生產環境的初始資料庫狀態

在實際應用中，在初始部署後，生產網站與使用者互動，資料庫會填入即時資料。 因此，您不想要重新部署成員資格資料庫處於其初始狀態，會清除所有的即時資料。 因為 SQL Server Compact 資料庫中的檔案*應用程式\_資料*資料夾中，您必須防止變更部署設定，因此，檔案中*應用程式\_資料*資料夾無法部署。

開啟**專案屬性**ContosoUniversity 專案，然後選取視窗**封裝/發行 Web**  索引標籤。請確定**組態**下拉式方塊已**作用中 （發行）**或**發行**選取，請選取**檔案排除應用程式\_資料資料夾**。

![Exclude_files_from_the_App_Data_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image9.png)

如果您決定未來部署偵錯組建，最好在其中進行相同的偵錯組建組態變更： 變更**組態**至**偵錯**，然後選取 **排除從應用程式檔案\_資料資料夾**。

儲存並關閉**封裝/發行 Web**  索引標籤。

> [!NOTE] 
> 
> [!IMPORTANT]
> 請確定您沒有**移除目的端的其他檔案**選取您的發行設定檔中。 如果您選取該選項時，部署程序將會刪除您在應用程式的資料庫\_資料位於已部署的站台，它將會刪除應用程式\_資料資料夾本身。


## <a name="preventing-user-access-to-the-production-site-during-update"></a>防止使用者存取更新期間將生產站台

您要部署的變更現在是簡單的變更，以在單一頁面。 有時候部署較大的變更，但在此情況下站台的行為怪異如果使用者在完成部署之前要求的頁面。 若要避免這個問題，您可以使用*應用程式\_offline.htm*檔案。 當您將檔案放名為*應用程式\_offline.htm*在應用程式的根資料夾中，IIS 會自動顯示該檔案，而不是執行您的應用程式。 因此若要防止存取在部署期間，您將放*應用程式\_offline.htm*根資料夾中，在執行部署程序中，然後再移除*應用程式\_offline.htm*。

在**方案總管 中**，以滑鼠右鍵按一下方案 （不是其中一個專案），然後選取**新的方案資料夾**。

![Creating_a_solution_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image10.png)

將資料夾命名為*SolutionFiles*。

新的資料夾中建立名為的 HTML 網頁*應用程式\_offline.htm*。 下列標記取代現有的內容：

[!code-html[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample2.html)]

您可以複製*應用程式\_offline.htm*檔案，以使用 FTP 連線的站台或**檔案管理員**裝載提供者的 [控制台] 中的公用程式。 此教學課程中，您將使用**檔案管理員**。

開啟控制台，並選取**檔案管理員**如同[部署到生產環境](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)教學課程。 選取**contosouniversity.com**然後**wwwroot**以取得您的應用程式根資料夾，然後按一下**上傳**。

[![Upload_button_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image11.png)

在**上傳檔案**對話方塊中，選取*應用程式\_offline.htm*檔案，然後按一下**上傳**。

[![Upload_dialog_box_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image13.png)

瀏覽至您的網站 URL。 您會看到*應用程式\_offline.htm*頁面現在會顯示而不是您的首頁。

[![App_offline.htm_page_in_production](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image15.png)

現在您已經準備好要部署到生產環境。

## <a name="deploying-the-code-update-to-the-production-environment"></a>將程式碼更新部署至生產環境

在**Web 一個按一下 Publish**工具列上，選擇**生產**發行設定檔，然後按一下 **發行 Web**。

Visual Studio 部署更新的應用程式，並開啟瀏覽器以網站的首頁。 *應用程式\_offline.htm*檔案會顯示。 您可以測試以確認成功部署之前，您必須移除*應用程式\_offline.htm*檔案。

返回**檔案管理員**控制台 中的應用程式。 選取**contosouniversity.com**和**wwwroot**，選取**應用程式\_offline.htm**，然後按一下 **刪除**。

[![Deleting_app_offline.htm](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image17.png)

在瀏覽器中開啟講師頁面在公用網站中，並選取講師以確認更新已成功部署。

[![Instructors_page_with_courses_Prod](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image19.png)

您現在已部署應用程式更新未含資料庫變更。 下一個教學課程會示範如何部署資料庫變更。

>[!div class="step-by-step"]
[上一頁](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
[下一頁](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
