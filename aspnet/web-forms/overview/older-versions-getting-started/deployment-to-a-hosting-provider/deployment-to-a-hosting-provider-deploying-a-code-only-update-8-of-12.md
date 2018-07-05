---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
title: 使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 部署 Code-Only 更新-12 個 8 |Microsoft Docs
author: tdykstra
description: 這一系列的教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式專案，其中包含 SQL Server Compact 資料庫，使用 Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: ddf6252f-9413-4c0c-a360-2cef8d231717
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
msc.type: authoredcontent
ms.openlocfilehash: 39075d2e979076f88200ea595c92f95f38f35911
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374009"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-code-only-update---8-of-12"></a>使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 部署 Code-Only 更新-8 12
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

[下載入門專案](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> 這一系列的教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式專案使用 Visual Studio 2012 RC 或 Visual Studio Express 2012 RC for Web 包含 SQL Server Compact 資料庫。 如果您安裝 Web Publish Update，您也可以使用 Visual Studio 2010。 在數列的簡介，請參閱[系列的第一個教學課程](deployment-to-a-hosting-provider-introduction-1-of-12.md)。
> 
> 顯示 Visual Studio 2012 RC 版本之後引入的部署功能，示範如何部署 SQL Server Compact，以外的 SQL Server 版本，並示範如何部署至 Azure App Service Web Apps 的教學課程，請參閱[ASP.NET Web 部署使用 Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md)。


## <a name="overview"></a>總覽

初始部署之後，您的工作，維護及開發您的網站會繼續，而且之前長時間中，您會想要將更新部署。 本教學課程會帶領您完成將更新部署到您的應用程式程式碼的程序。 此更新未牽涉到資料庫變更;您會看到有什麼不一樣將在下一個教學課程中的資料庫變更部署。

提醒： 如果您收到錯誤訊息，或當您瀏覽本教學課程，項目無法運作，請務必檢查[疑難排解頁面](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)。

## <a name="making-a-code-change"></a>進行程式碼變更

做為更新的簡單範例，用您的應用程式，您會加入至**講師**頁面上選取的講師所教授的課程清單。

如果您執行**講師**頁面上，您會發現有**選取**連結在方格中，但它們不進行以外進行的任何資料列背景開啟灰色。

[![Instructors_page](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image1.png)

現在您將新增時執行的程式碼**選取**連結按一下，並顯示一份所選講師所教授的課程。

在  *Instructors.aspx*，加入下列標記的後面**ErrorMessageLabel** `Label`控制項：

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample1.aspx)]

執行網頁，然後選取講師。 您會看到一份該講師所教授的課程。

[![Instructors_page_with_courses](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image3.png)

## <a name="deploying-the-code-update-to-the-test-environment"></a>程式碼將更新部署至測試環境

部署到測試環境是只需執行一種單鍵發行一次。 若要讓這個程序更快速，您可以使用**Web 單鍵發佈**工具列。

在 **檢視**功能表上，選擇**工具列**，然後選取**Web 單鍵發佈**。

![Selecting_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image5.png)

在 **方案總管 中**，選取 ContosoUniversity 專案。

**Web 單鍵發佈**工具列上，選擇**測試**發行設定檔，然後按一下 **發佈 Web** （指向左、 右的箭號圖示）。

![Web_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image6.png)

Visual Studio 會部署更新的應用程式和瀏覽器會自動開啟至首頁。 執行 Instructors 頁面，然後選取講師，以確認更新已成功部署。

[![Instructors_page_with_courses_Test](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image7.png)

您通常也進行迴歸測試 （也就是測試以確定新的變更沒有破壞任何現有功能的站台的其餘部分）。 但本教學課程中，您會略過該步驟並繼續將更新部署到生產環境。

## <a name="preventing-redeployment-of-the-initial-database-state-to-production"></a>防止重新部署到生產環境的初始資料庫狀態

在實際的應用程式中，您初始部署之後，您的生產網站與使用者互動，資料庫會以填入即時資料。 因此，您不打算重新部署其初始狀態，會清除所有的即時資料中的成員資格資料庫。 因為 SQL Server Compact 資料庫中的檔案*應用程式\_資料*資料夾中，您必須防止變更部署設定，因此，中的檔案*應用程式\_資料*資料夾無法部署。

開啟**專案屬性**ContosoUniversity 專案，然後選取視窗**封裝/發行 Web**  索引標籤。請確定**組態**下拉式方塊已**作用 （發行）** 或**版本**選取，請選取**檔案排除應用程式\_Data 資料夾**。

![Exclude_files_from_the_App_Data_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image9.png)

如果您決定要在未來部署偵錯組建，最好在其中進行相同的偵錯組建組態變更： 變更**組態**要**偵錯**，然後選取**排除從應用程式的檔案\_Data 資料夾**。

儲存並關閉**封裝/發行 Web**  索引標籤。

> [!NOTE] 
> 
> [!IMPORTANT]
> 請確定您沒有**移除目的地上的其他檔案**選取您的發行設定檔中。 如果您選取該選項時，部署程序會刪除您在應用程式的資料庫\_中的已部署的站台，而且資料將會刪除應用程式\_資料資料夾本身。


## <a name="preventing-user-access-to-the-production-site-during-update"></a>防止使用者存取到生產網站，在更新期間

您要部署的變更現在是簡單的變更至單一頁面。 但有時候您部署較大的變更，和在此情況下站台可以行為怪異如果使用者在完成部署之前要求的頁面。 若要避免這個問題，您可以使用*應用程式\_offline.htm*檔案。 當您將名為的檔案*應用程式\_offline.htm*在您的應用程式根資料夾中，IIS 會自動顯示該檔案，而不是執行您的應用程式。 因此若要防止存取在部署期間，您將放*應用程式\_offline.htm*根資料夾中，在執行部署程序，然後再移除*應用程式\_offline.htm*。

在 **方案總管**，以滑鼠右鍵按一下方案 （不是其中一個專案），然後選取**新的方案資料夾**。

![Creating_a_solution_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image10.png)

將資料夾命名*SolutionFiles*。

新的資料夾中建立名為 HTML 網頁*應用程式\_offline.htm*。 以下列標記取代現有的內容：

[!code-html[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample2.html)]

您可以複製*應用程式\_offline.htm*檔案，以使用 FTP 連線的站台或**檔案管理員**裝載提供者的控制項台中公用程式。 本教學課程中，您將使用**檔案管理員**。

開啟控制台，然後選取**檔案管理員**如同在[部署到生產環境](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)教學課程。 選取  **contosouniversity.com** ，然後**wwwroot**以取得您的應用程式根資料夾，然後按一下**上載**。

[![Upload_button_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image11.png)

在 **上傳檔案**對話方塊中，選取*應用程式\_offline.htm*檔案，然後按一下**上載**。

[![Upload_dialog_box_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image13.png)

瀏覽至您的網站 URL。 您會看到*應用程式\_offline.htm*頁面現在會顯示而不是您的首頁。

[![App_offline.htm_page_in_production](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image15.png)

您現在已準備好部署至生產環境。

## <a name="deploying-the-code-update-to-the-production-environment"></a>程式碼將更新部署至生產環境

在  **Web 單鍵發佈**工具列上，選擇**生產**發行設定檔，然後按一下 **發佈 Web**。

Visual Studio 會部署更新的應用程式和瀏覽器開啟至網站的首頁。 *應用程式\_offline.htm*檔案會顯示。 您可以測試以確認成功部署之前，必須先移除*應用程式\_offline.htm*檔案。

返回**檔案管理員**控制項台中應用程式。 選取  **contosouniversity.com**並**wwwroot**，選取**應用程式\_offline.htm**，然後按一下**刪除**。

[![Deleting_app_offline.htm](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image17.png)

在瀏覽器中，開啟 Instructors 頁面在公用網站中，然後選取講師，以確認更新已成功部署。

[![Instructors_page_with_courses_Prod](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image19.png)

您現在已部署應用程式更新未未牽涉到資料庫變更。 下一個教學課程會示範如何將資料庫變更部署。

> [!div class="step-by-step"]
> [上一頁](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
> [下一頁](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
