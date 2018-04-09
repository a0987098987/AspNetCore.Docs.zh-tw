---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
title: 使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 部署資料庫更新-9 / 12 |Microsoft 文件
author: tdykstra
description: 這一系列的教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式專案，其中包含 SQL Server Compact 資料庫使用視覺化 Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a8d776af-4735-4612-87f6-9f326587f2d3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
msc.type: authoredcontent
ms.openlocfilehash: 0a00f9d3ed284ebbc1d83c1b5696436e5ba00f4b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-database-update---9-of-12"></a>使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 部署資料庫更新-9 / 12
====================
由[Tom Dykstra](https://github.com/tdykstra)

[下載入門專案](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> 這一系列的教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式專案，其中包含使用 Visual Studio 2012 RC 或 Visual Studio Express 2012 RC for Web 的 SQL Server Compact 資料庫。 如果您安裝 Web 發行更新，您也可以使用 Visual Studio 2010。 數列的簡介，請參閱[系列的第一個教學課程](deployment-to-a-hosting-provider-introduction-1-of-12.md)。
> 
> 顯示部署 Visual Studio 2012 RC 發行之後，引進的功能，示範如何將 SQL Server Compact 以外的 SQL Server 版本的部署和示範如何將部署至 Azure App Service Web 應用程式的教學課程，請參閱[ASP.NET Web 部署使用 Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md)。


## <a name="overview"></a>總覽

本教學課程中，您在資料庫變更及相關的程式碼變更，在 Visual Studio 中，測試所做的變更，然後將更新部署到測試和實際執行環境。

提示： 如果您收到錯誤訊息，或當您瀏覽教學課程，項目無法運作，請務必檢查[疑難排解頁面](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)。

## <a name="adding-a-new-column-to-a-table"></a>將新的資料行加入資料表

在本節中，您加入的出生日期資料行`Person`基底類別`Student`和`Instructor`實體。 然後您可以更新，使其顯示新的資料行顯示講師資料的頁面。

在*ContosoUniversity.DAL*專案中，開啟*Person.cs*和結尾加入下列屬性`Person`類別 （應該會有兩個左右大括號後面）：

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample1.cs)]

接著，更新 Seed 方法，使其提供新的資料行的值。 開啟*Migrations\Configuration.cs*取代開始的程式碼區塊和`var instructors = new List<Instructor>`與下列程式碼區塊其中包括出生日期資訊：

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample2.cs)]

在 ContosoUniversity 專案中，開啟*Instructors.aspx*並加入新的範本欄位，以顯示出生日期。 雇用日期和 office 指派的項目之間，將它加入：

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample3.aspx)]

（如果程式碼縮排不同步，您可以按 CTRL-K 然後 CTRL-D 設為自動重新格式化檔案）。

建置方案，然後再開啟**Package Manager Console**視窗。 請確定 ContosoUniversity.DAL 仍然做為選取狀態**預設專案**。

在**Package Manager Console**視窗中，選取**ContosoUniversity.DAL**為**預設專案**，然後輸入下列命令：

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample4.ps1)]

此命令完成時，Visual Studio 會開啟定義新的類別檔案`DbMIgration`類別，然後在`Up`方法，您可以看到建立新的資料行的程式碼。

![AddBirthDate_migration_code](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image1.png)

建置方案，，然後輸入下列命令中的**Package Manager Console**視窗 （請確定已選取 ContosoUniversity.DAL 專案）：

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample5.ps1)]

當命令完成時，執行應用程式，並選取講師頁面。 當網頁載入時，您就會看到有新出生日期欄位。

[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>將資料庫更新部署到測試環境

在**方案總管 中**選取 ContosoUniversity 專案。

在**Web 一個按一下 Publish**工具列上，選取**測試**發行設定檔，然後按一下 **發行 Web**。 (如果已停用工具列，ContosoUniversity 中選取專案**方案總管 中**。)

Visual Studio 會部署更新的應用程式，並瀏覽器開啟至首頁。 執行講師頁面以確認更新已成功部署。 當應用程式嘗試存取此頁面的資料庫時，Code First 更新資料庫結構描述並執行`Seed`方法。 當頁面顯示時，您會看到預期**出生日期**中它的日期資料行。

[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>將資料庫更新部署至生產環境

您現在可以部署到生產環境。 唯一的差別在於您將使用*應用程式\_offline.htm*以避免使用者存取網站，而您要部署的變更，因而更新資料庫。 生產環境部署中，執行下列步驟：

- 上傳*應用程式\_offline.htm*生產網站的檔案。
- 在 Visual Studio 中，選擇 在實際執行設定檔**Web 一個按一下 Publish**工具列，並按一下**發行 Web**。
- 刪除*應用程式\_offline.htm*從生產網站的檔案。

> [!NOTE]
> 在實際執行環境中使用您的應用程式時您應該實作備份計劃。 也就是說，您應該要定期複製*學校 Prod.sdf*和*aspnet Prod.sdf*檔案從實際站台至安全的儲存體位置，以及您應該保留這類的數個層代備份。 當您更新資料庫時，請立即變更之前的備份複本。 然後，如果您犯了錯誤，並不探索它，直到它部署到生產環境之後，您將仍然能夠將資料庫復原到它損毀前的狀態。


當 Visual Studio 會在瀏覽器中開啟首頁 URL*應用程式\_offline.htm*頁面隨即顯示。 當您刪除之後*應用程式\_offline.htm*檔案中，您可以瀏覽至您的首頁上，確認該更新已成功部署。

[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)

您現在已部署應用程式更新包含測試與實際的資料庫變更。 下一個教學課程會示範如何將資料庫從 SQL Server Compact 移轉至 SQL Server Express 和 SQL Server。

> [!div class="step-by-step"]
> [上一頁](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
> [下一頁](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
