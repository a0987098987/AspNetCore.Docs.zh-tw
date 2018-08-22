---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
title: 使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 部署 SQL Server 資料庫更新-11 小時，共 12 |Microsoft Docs
author: tdykstra
description: 這一系列的教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式專案，其中包含 SQL Server Compact 資料庫，使用 Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 5e2bb092-cb22-4511-ad0a-22ae12dd99b3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
msc.type: authoredcontent
ms.openlocfilehash: 2018b58207365a0a3829f1f359d46d765aad0a77
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824549"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-sql-server-database-update---11-of-12"></a>使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 部署 SQL Server 資料庫更新-11 小時，共 12
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

[下載入門專案](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> 這一系列的教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式專案使用 Visual Studio 2012 RC 或 Visual Studio Express 2012 RC for Web 包含 SQL Server Compact 資料庫。 如果您安裝 Web Publish Update，您也可以使用 Visual Studio 2010。 在數列的簡介，請參閱[系列的第一個教學課程](deployment-to-a-hosting-provider-introduction-1-of-12.md)。
> 
> 顯示 Visual Studio 2012 RC 版本之後引入的部署功能，示範如何部署 SQL Server Compact，以外的 SQL Server 版本，並示範如何部署至 Windows Azure 網站的教學課程，請參閱[ASP.NET Web 部署使用 Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md)。


## <a name="overview"></a>總覽

本教學課程會示範如何將資料庫更新部署至完整的 SQL Server 資料庫。 因為 Code First 移轉更新資料庫的所有工作，程序是幾乎完全相同，您的 SQL Server Compact 中做過[部署資料庫更新](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)教學課程。

提醒： 如果您收到錯誤訊息，或當您瀏覽本教學課程，項目無法運作，請務必檢查[疑難排解頁面](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)。

## <a name="adding-a-new-column-to-a-table"></a>將新的資料行加入至資料表

在本教學課程的這一節中，您要進行變更的資料庫和對應的程式碼變更，然後進行測試 Visual Studio 中以準備將它們部署到測試和生產環境。 變更部署，涉及新增`OfficeHours`資料行`Instructor`實體，並顯示在新的資訊**講師**網頁。

在 ContosoUniversity.DAL 專案中，開啟*Instructor.cs*並新增下列屬性之間`HireDate`和`Courses`屬性：

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample1.cs)]

更新的初始設定式類別，使它植入測試資料的新資料行。 開啟*Migrations\Configuration.cs*並取代程式碼區塊開頭`var instructors = new List<Instructor>`以下列程式碼區塊包含新的資料行：

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample2.cs)]

在 [ContosoUniversity] 專案中，開啟*Instructors.aspx* ，並將新的 [範本] 欄位加入結尾之前的上班`</Columns>`在第一個標記`GridView`控制項：

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample3.aspx)]

建置方案。

開啟**Package Manager Console**視窗中，並為選取 ContosoUniversity.DAL**預設專案**。

輸入下列命令：

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample4.ps1)]

執行應用程式，然後選取**講師**頁面。 頁面需要較長的時間比平常要載入，因為 Entity Framework 重新建立資料庫並植入測試資料。

[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>資料庫將更新部署至測試環境

當您使用 Code First 移轉時，則會將資料庫變更部署到 SQL Server 的方法是與 SQL Server Compact 時相同。 不過，您必須變更測試發行設定檔，因為它仍然設定為從 SQL Server Compact 移轉至 SQL Server。

第一步是移除您在上一個教學課程中建立的連接字串轉換。 不再需要用到這些因為您將發行設定檔中指定連接字串轉換設定之前一樣**封裝/發行 SQL**移轉至 SQL Server 索引標籤。

開啟*Web.Test.config*檔案，並移除`connectionStrings`項目。 中的唯一剩餘的轉換*Web.Test.config*檔案的用途`Environment`中的值`appSettings`項目。

現在您可以更新的發行設定檔和發行到測試環境。

開啟**發佈 Web**精靈，然後再切換到**設定檔** 索引標籤。

選取 **測試**發行設定檔。

選取 [**設定**] 索引標籤。

按一下 **啟用新的資料庫發行改進**。

在的連接字串中**SchoolContext**，輸入您在中使用的相同值*Web.Test.config*先前的教學課程中的轉換檔案：

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample5.cmd)]

選取  **Execute Code First Migrations （在應用程式啟動時執行）**。 (在您的 Visual Studio 版本，可能會標示為核取方塊**套用 Code First 移轉**。)

在的連接字串中**DefaultConnection**，輸入您在中使用的相同值*Web.Test.config*先前的教學課程中的轉換檔案：

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample6.cmd)]

離開**更新資料庫**清除。

按一下 [發行] 。

Visual Studio 會部署到測試環境的程式碼變更和瀏覽器開啟至 Contoso 大學首頁上。

選取的講師頁面。

當應用程式執行此頁面時，它會嘗試存取資料庫。 如果資料庫是最新狀態，並找到的最新的移轉具有尚未套用的資料，則會檢查 code First 移轉。 Code First 移轉適用於最新的移轉，便會執行`Seed`方法，然後按一下  頁面會正常執行。 您會看到新的辦公時間資料行，以植入的資料。

[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>資料庫將更新部署至生產環境

您必須也變更生產環境的發行設定檔。 在此情況下，您會移除現有的設定檔，並建立一個新的匯入更新的.publishsettings 檔案。 更新的檔案會包含在 Cytanium 的 SQL Server 資料庫的連接字串。

如您所見，當您部署到測試環境，您不再需要連接字串中的轉換*Web.Production.config*轉換檔案。 開啟了檔案，並移除`connectionStrings`項目。 「 剩餘 」 轉換對於`Environment`中的值`appSettings`項目和`location`Elmah 錯誤報表，會限制存取的項目。

建立新的發行設定檔，用於生產環境之前，下載更新的.publishsettings 檔案的相同方式稍早在[部署到生產環境](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)教學課程。 (在 Cytanium 控制面板中，按一下**Web Sites**，然後按一下**contosouniversity.com**網站。 選取**Web Publishing**索引標籤，然後再按一下**網站下載發行設定檔**。)您會執行此動作的原因是要挑選.publishsettings 檔案中的資料庫連接字串。 連接字串無法下載檔案，因為您仍使用 SQL Server Compact，並沒有 SQL Server 資料庫在 Cytanium 尚未建立的第一次使用。

現在您可以更新的發行設定檔和發行到生產環境。

開啟**發佈 Web**精靈，然後再切換到**設定檔** 索引標籤。

按一下 **管理設定檔**，然後再刪除實際執行設定檔。

關閉**發佈 Web** 」 精靈來儲存這項變更。

開啟**發佈 Web**再次強調，精靈，然後按一下**匯入**。

在 **連接**索引標籤中變更**目的地 URL**為適當的值，如果您使用暫存的 URL。

按 [ **下一步**]。

在 **設定**索引標籤上，按一下**啟用新的資料庫發行改進**。

中的連接字串下拉式清單**SchoolContext**，選取 Cytanium 連接字串。

![Selecting_Cytanium_connection_string](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image5.png)

選取 **執行 Code First 移轉 （在應用程式啟動時執行）**。

中的連接字串下拉式清單**DefaultConnection**，選取 Cytanium 連接字串。

選取 **設定檔**索引標籤上，按一下**管理設定檔**，並將設定檔從"contosouniversity.com-Web Deploy"重新命名為 「 生產 」。

關閉的發行設定檔，以儲存變更，然後再重新開啟。

按一下 [發行] 。 (用於實際生產網站，您會複製*應用程式\_offline.htm*到生產環境與 put 在您的專案資料夾，在發行之前，然後移除部署完成時。)

Visual Studio 會部署到測試環境的程式碼變更和瀏覽器開啟至 Contoso 大學首頁上。

選取的講師頁面。

Code First 移轉更新資料庫中的測試環境相同的方式。 您會看到新的辦公時間資料行，以植入的資料。

![Instructors_page_with_OfficeHours_Prod](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image6.png)

現在您已成功部署應用程式更新包含資料庫變更，使用 SQL Server 資料庫。

## <a name="more-information"></a>更多資訊

如此即完成本系列的教學課程將部署到協力廠商裝載提供者的 ASP.NET web 應用程式。 如需任何這些教學課程所涵蓋的主題的詳細資訊，請參閱[ASP.NET 部署內容對應](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx)MSDN 網站上。

## <a name="acknowledgements"></a>謝誌

我想要感謝下列人士提出重大貢獻的內容，本教學課程系列：

- [Alberto Poblacion、 MVP &amp; MCT，西班牙](https://mvp.support.microsoft.com/profile/Alberto)
- Jarod Ferguson，資料平台開發 MVP、 美國。
- Harsh Mittal，Microsoft
- [Kristina Olson Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Mike 主教 Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Mohit Srivastava Microsoft
- [Raffaele Rialdi 義大利](http://www.iamraf.net/)
- [Rick Anderson Microsoft](https://blogs.msdn.com/b/rickandy/)
- [Sayed Hashimi，Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))
- [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))
- [Scott Hunter、 Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))
- [Srđan Božović，塞爾維亞](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi、 Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))

> [!div class="step-by-step"]
> [上一頁](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
> [下一頁](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)
