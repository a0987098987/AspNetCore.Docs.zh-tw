---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
title: 使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 部署 SQL Server 資料庫更新-11 12 |Microsoft 文件
author: tdykstra
description: 這一系列的教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式專案，其中包含 SQL Server Compact 資料庫使用視覺化 Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 5e2bb092-cb22-4511-ad0a-22ae12dd99b3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
msc.type: authoredcontent
ms.openlocfilehash: d8cc072c631900937f31c8f2637869b53d46cf54
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887326"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-sql-server-database-update---11-of-12"></a>使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 部署 SQL Server 資料庫更新-11 12
====================
由[Tom Dykstra](https://github.com/tdykstra)

[下載入門專案](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> 這一系列的教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式專案，其中包含使用 Visual Studio 2012 RC 或 Visual Studio Express 2012 RC for Web 的 SQL Server Compact 資料庫。 如果您安裝 Web 發行更新，您也可以使用 Visual Studio 2010。 數列的簡介，請參閱[系列的第一個教學課程](deployment-to-a-hosting-provider-introduction-1-of-12.md)。
> 
> 顯示部署 Visual Studio 2012 RC 發行之後，引進的功能，示範如何將 SQL Server Compact 以外的 SQL Server 版本的部署和示範如何將部署至 Windows Azure 網站的教學課程，請參閱[ASP.NET Web 部署使用 Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md)。


## <a name="overview"></a>總覽

本教學課程會示範如何將資料庫更新部署至完整的 SQL Server 資料庫。 Code First 移轉將會更新資料庫的所有工作，因為在程序會與您未針對 SQL Server Compact 中幾乎完全相同[部署資料庫更新](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)教學課程。

提示： 如果您收到錯誤訊息，或當您瀏覽教學課程，項目無法運作，請務必檢查[疑難排解頁面](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)。

## <a name="adding-a-new-column-to-a-table"></a>將新的資料行加入資料表

在本章節的教學課程中您要進行變更的資料庫和對應的程式碼變更，然後在 Visual Studio 中進行測試，以準備將它們部署到測試和生產環境。 變更包括加入`OfficeHours`欄`Instructor`實體和顯示中的新資訊**講師**網頁。

在 ContosoUniversity.DAL 專案中，開啟*Instructor.cs*並加入下列屬性之間`HireDate`和`Courses`屬性：

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample1.cs)]

更新的初始設定式類別，使它植入新的資料行，以測試資料。 開啟*Migrations\Configuration.cs*取代開始的程式碼區塊和`var instructors = new List<Instructor>`與下列程式碼區塊包含新的資料行：

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample2.cs)]

在 ContosoUniversity 專案中，開啟*Instructors.aspx*並加入新的範本欄位的上班時間時的結束`</Columns>`中第一個標記`GridView`控制項：

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample3.aspx)]

建置方案。

開啟**Package Manager Console**視窗中，並為選取 ContosoUniversity.DAL**預設專案**。

輸入下列命令：

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample4.ps1)]

執行應用程式並選取**講師**頁面。 頁面會較長的時間比平常載入，因為 Entity Framework 重新建立資料庫，並設測試資料。

[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>將資料庫更新部署到測試環境

當您使用 Code First 移轉時，則會將資料庫變更部署到 SQL Server 的方法是與 SQL Server Compact 相同。 不過，您必須變更測試發行設定檔，因為它仍設定從 SQL Server Compact 移轉至 SQL Server。

第一步是移除您在上一個教學課程中建立的連接字串轉換。 不再需要用到這些因為您會指定連接字串轉換中的發行設定檔，如同您設定之前**封裝/發行 SQL**移轉至 SQL Server 索引標籤。

開啟*Web.Test.config*檔案，並移除`connectionStrings`項目。 中的唯一剩餘轉換*Web.Test.config*檔案適用於`Environment`值`appSettings`項目。

現在您可以更新的發行設定檔和發行到測試環境。

開啟**發行 Web**精靈後再切換至**設定檔** 索引標籤。

選取**測試**發行設定檔。

選取**設定** 索引標籤。

按一下**啟用新的資料庫發行改進**。

在連接字串 方塊的**SchoolContext**，輸入相同的值用於*Web.Test.config*轉換上一個教學課程中的檔案：

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample5.cmd)]

選取**執行 Code First 移轉 （在應用程式啟動時執行）**。 (在您的 Visual Studio 版本中，核取方塊可能會標示為**套用 Code First 移轉**。)

在連接字串 方塊的**DefaultConnection**，輸入相同的值用於*Web.Test.config*轉換上一個教學課程中的檔案：

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample6.cmd)]

保留**更新資料庫**清除。

按一下 [發行] 。

Visual Studio 將程式碼變更部署到測試環境，並開啟瀏覽器以 Contoso 大學首頁上。

選取講師頁面。

當應用程式執行此頁面時，它會嘗試存取資料庫。 Code First 移轉會檢查的資料庫是最新，並找到的最新的移轉具有尚未套用。 Code First 移轉適用於最新的移轉，便會執行`Seed`方法，然後按一下 網頁會正常執行。 您會看到新的上班時間時資料行的植入的資料。

[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>將資料庫更新部署至生產環境

您必須變更也會在實際執行環境的發行設定檔。 在此情況下，您會移除現有的設定檔，並建立一個新匯入更新的.publishsettings 檔案。 更新的檔案會包含在 Cytanium 的 SQL Server 資料庫的連接字串。

如您所見，當您部署到測試環境，您不再需要的連接字串轉換*Web.Production.config*轉換檔案。 開啟了檔案，並移除`connectionStrings`項目。 「 剩餘 」 轉換對於`Environment`值`appSettings`項目和`location`Elmah 錯誤報表會限制存取的項目。

建立新的發行設定檔用於生產環境之前，下載更新的.publishsettings 檔案相同的方式就像您稍早在[部署到生產環境](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)教學課程。 (Cytanium 控制台中，按一下 **網站**，然後按一下  **contosouniversity.com**網站。 選取**Web 發佈**索引標籤，然後再按一下**這個網站下載發行設定檔**。)您會這樣的原因是要挑選.publishsettings 檔案中的資料庫連接字串。 連接字串無法下載檔案，因為您已仍在使用 SQL Server Compact，以往在 SQL Server 資料庫在 Cytanium 尚未建立的第一次使用。

現在您可以更新的發行設定檔和發行到生產環境。

開啟**發行 Web**精靈後再切換至**設定檔** 索引標籤。

按一下**管理設定檔**，然後再刪除實際執行設定檔。

關閉**發行 Web**精靈，以儲存這項變更。

開啟**發行 Web**精靈一次，然後再按一下**匯入**。

在**連接**索引標籤上，變更**目的地 URL**至適當的值，如果您使用暫存的 URL。

按 [ **下一步**]。

在**設定**索引標籤上，按一下 **啟用新的資料庫發行改進**。

中的連接字串下拉式清單**SchoolContext**，選取 Cytanium 連接字串。

![Selecting_Cytanium_connection_string](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image5.png)

選取**執行 Code First 移轉 （在應用程式啟動時執行）**。

中的連接字串下拉式清單**DefaultConnection**，選取 Cytanium 連接字串。

選取**設定檔**索引標籤上，按一下 **管理設定檔**，將設定檔從"contosouniversity.com-Web Deploy"重新命名為 「 生產 」。

關閉的發行設定檔，以儲存變更，然後再重新開啟。

按一下 [發行] 。 (真實的生產網站，您可以將複製*應用程式\_offline.htm*到生產環境與 put 在您的專案資料夾，在發行之前，然後移除部署完成時。)

Visual Studio 將程式碼變更部署到測試環境，並開啟瀏覽器以 Contoso 大學首頁上。

選取講師頁面。

Code First 移轉來測試環境中相同的方式更新資料庫。 您會看到新的上班時間時資料行的植入的資料。

![Instructors_page_with_OfficeHours_Prod](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image6.png)

您現在成功部署應用程式更新包含資料庫變更，使用 SQL Server 資料庫。

## <a name="more-information"></a>更多資訊

如此即完成這一系列的 ASP.NET web 應用程式部署到協力廠商主機服務提供者上的教學課程。 如需任何這些教學課程涵蓋之主題的詳細資訊，請參閱[ASP.NET 部署內容地圖](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx)MSDN 網站上。

## <a name="acknowledgements"></a>謝誌

我想要感謝下列重大貢獻內容本教學課程系列的人：

- [Alberto Poblacion、 MVP &amp; mct 規範、 西班牙](https://mvp.support.microsoft.com/profile/Alberto)
- Jarod Ferguson，資料平台開發 MVP，美國
- 惡劣 Mittal、 Microsoft
- [Kristina Olson, Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Mike 教宗 Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Mohit Srivastava Microsoft
- [Raffaele Rialdi （義大利）](http://www.iamraf.net/)
- [Rick Anderson Microsoft](https://blogs.msdn.com/b/rickandy/)
- [Sayed Hashimi、 Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))
- [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))
- [Scott 獵人、 Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))
- [Srđan Božović、 塞爾維亞](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi、 Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))

> [!div class="step-by-step"]
> [上一頁](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
> [下一頁](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)
