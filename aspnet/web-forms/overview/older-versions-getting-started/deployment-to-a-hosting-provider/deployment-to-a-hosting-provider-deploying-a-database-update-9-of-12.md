---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
title: 使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 部署資料庫更新-12 個 9 |Microsoft Docs
author: tdykstra
description: 這一系列的教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式專案，其中包含 SQL Server Compact 資料庫，使用 Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a8d776af-4735-4612-87f6-9f326587f2d3
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
msc.type: authoredcontent
ms.openlocfilehash: e94281e36192c91a04392735af318bbc517b0521
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382455"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-database-update---9-of-12"></a>使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 部署資料庫更新-12 個 9
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

[下載入門專案](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> 這一系列的教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式專案使用 Visual Studio 2012 RC 或 Visual Studio Express 2012 RC for Web 包含 SQL Server Compact 資料庫。 如果您安裝 Web Publish Update，您也可以使用 Visual Studio 2010。 在數列的簡介，請參閱[系列的第一個教學課程](deployment-to-a-hosting-provider-introduction-1-of-12.md)。
> 
> 顯示 Visual Studio 2012 RC 版本之後引入的部署功能，示範如何部署 SQL Server Compact，以外的 SQL Server 版本，並示範如何部署至 Azure App Service Web Apps 的教學課程，請參閱[ASP.NET Web 部署使用 Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md)。


## <a name="overview"></a>總覽

在本教學課程中，您讓資料庫變更和相關程式碼變更，在 Visual Studio 中，測試所做的變更，然後將更新部署至測試和生產環境。

提醒： 如果您收到錯誤訊息，或當您瀏覽本教學課程，項目無法運作，請務必檢查[疑難排解頁面](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)。

## <a name="adding-a-new-column-to-a-table"></a>將新的資料行加入至資料表

在本節中，您新增的出生日期資料行`Person`基底類別`Student`和`Instructor`實體。 然後，您會更新，以顯示新的資料行顯示講師資料的頁面。

在  *ContosoUniversity.DAL*專案中，開啟*Person.cs* ，並在結尾新增下列屬性`Person`（應該會有兩個左右大括號後面） 的類別：

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample1.cs)]

接下來，更新種子方法，讓它提供新的資料行的值。 開啟*Migrations\Configuration.cs*並取代程式碼區塊開頭`var instructors = new List<Instructor>`其中包括出生日期資訊的下列程式碼區塊：

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample2.cs)]

在 [ContosoUniversity] 專案中，開啟*Instructors.aspx*並加入新的 [範本] 欄位顯示的出生日期。 請將它加入的項目指派的雇用日期及 office 之間：

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample3.aspx)]

（如果程式碼縮排不同步，您可以按 CTRL-K 然後按 CTRL-D 會自動重新格式化檔案。）

建置方案，然後再開啟**Package Manager Console**視窗。 請確定 ContosoUniversity.DAL 仍已選取作為**預設專案**。

在  **Package Manager Console**視窗中，選取**ContosoUniversity.DAL**做為**預設專案**，然後輸入下列命令：

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample4.ps1)]

此命令完成時，Visual Studio 會開啟可定義新的類別檔案`DbMIgration`類別，然後在`Up`方法，您可以看到建立新的資料行的程式碼。

![AddBirthDate_migration_code](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image1.png)

建置方案，，然後輸入下列命令，在**Package Manager Console**視窗 （請確定 ContosoUniversity.DAL 專案仍為已選取）：

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample5.ps1)]

當命令完成時，請執行應用程式，然後選取講師頁面。 當頁面載入時，您就會看到它有新的出生日期欄位。

[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>資料庫將更新部署至測試環境

在 [**方案總管] 中**選取 ContosoUniversity 專案。

在  **Web 單鍵發佈**工具列上，選取**測試**發行設定檔，然後按一下**發佈 Web**。 (如果已停用工具列，選取 [ContosoUniversity 專案中的**方案總管] 中**。)

Visual Studio 會部署更新的應用程式和瀏覽器開啟至首頁。 執行 Instructors 頁面，以確認更新已成功部署。 當應用程式嘗試存取此頁面的資料庫時，Code First 會更新資料庫結構描述和執行`Seed`方法。 當頁面顯示時，您會看到預期**出生日期**中它的日期資料行。

[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>資料庫將更新部署至生產環境

您現在可以部署到生產環境。 唯一的差別是您將使用*應用程式\_offline.htm*來防止使用者存取網站，並因此更新資料庫，而您要部署的變更。 針對生產環境部署中，執行下列步驟：

- 上傳*應用程式\_offline.htm*到生產網站的檔案。
- 在 Visual Studio 中，選擇 在實際執行設定檔**Web 單鍵發佈**工具列，並按一下**發佈 Web**。
- 刪除*應用程式\_offline.htm*從生產網站的檔案。

> [!NOTE]
> 在生產環境中使用您的應用程式時您應該實作備份計劃。 也就是說，您應該將定期複製*學校 Prod.sdf*並*aspnet Prod.sdf*檔案從生產網站到安全的儲存體位置，以及您應該保留這類的數個層代備份。 當您更新資料庫時，您要立即在變更之前的備份複本。 然後，如果發生錯誤，而不加以探索直到您已將它部署到生產環境之後，您仍然能夠將資料庫復原到其損毀前的狀態。


當 Visual Studio 在瀏覽器中開啟首頁 URL*應用程式\_offline.htm*頁面隨即顯示。 當您刪除之後*應用程式\_offline.htm*檔案中，您可以瀏覽至您的首頁，以確認更新已成功部署。

[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)

您現在已部署應用程式更新包含資料庫變更至測試和生產環境。 下一個教學課程會示範如何從 SQL Server Compact 將資料庫移轉至 SQL Server Express 和 SQL Server。

> [!div class="step-by-step"]
> [上一頁](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
> [下一頁](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
