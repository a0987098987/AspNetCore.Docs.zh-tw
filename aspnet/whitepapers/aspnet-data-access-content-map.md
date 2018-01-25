---
uid: whitepapers/aspnet-data-access-content-map
title: "ASP.NET 資料存取-建議資源 |Microsoft 文件"
author: rick-anderson
description: "本主題提供有關如何存取資料，在 ASP.NET web 應用程式，主要是使用 Entity Framework 和 SQL Se 文件資源的連結..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/25/2013
ms.topic: article
ms.assetid: f8157be1-4ab9-469e-ad3a-0ccc80b56c00
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-data-access-content-map
msc.type: content
ms.openlocfilehash: 16364951544dff33c9cdb8c8e330cc93de192c47
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-data-access---recommended-resources"></a>ASP.NET 資料存取-建議資源
====================
> 本主題提供有關如何存取資料，在 ASP.NET web 應用程式，主要是由使用 Entity Framework 和 SQL Server 文件資源的連結。
> 
> 如果您知道好的部落格文章， [stackoverflow](http://stackoverflow.com)執行緒或任何其他連結，會很有用，[傳送一封電子郵件給我們](mailto:aspnetue@microsoft.com?subject=Data Access Content Map)與連結。
> 
> 上次更新的 4/3/2014


此主題包括下列各節：

- [開始使用 ASP.NET 中的資料存取](#gettingstarted)
- [使用 Entity Framework](#ef)

    - [第一次使用 Entity Framework 程式碼](#cf)
    - [使用 Entity Framework Code First 移轉](#efcfmigrations)
    - [第一次使用 Entity Framework 資料庫或模型的第一個 （EF 設計工具）](#efdbf)
    - [載入 （延遲載入、 積極式載入，和明確載入） 的 Entity Framework 中的相關的資料](#efrelateddata)
    - [最佳化 Entity Framework 效能](#optimizingef)
    - [處理 Entity Framework 應用程式中的並行存取](#efconcurrency)
    - [有關 Entity Framework 的書籍](#efbooks)
    - [Entity Framework 的其他資源](#otherefresources)
- [資料繫結的 ASP.NET Web Form 應用程式](#wfdatabinding)

    - [使用 Web Form 模型繫結](#wfmodelbinding)
    - [使用 Web Form 的資料來源控制項](#wfdsc)
    - [使用 Web Form 資料繫結控制項和資料繫結運算式](#wfdbc)
- [使用 SQL Server 資料庫](#sqlserver)

    - [使用 SQL Server Express LocalDB 資料庫](#sslocaldb)
    - [使用 SQL Server Express 資料庫](#sse)
    - [使用 Windows Azure SQL Database](#ssdb)
    - [SQL Server 和 Windows Azure SQL Database 之間選擇](#ssdbchoosing)
- [使用 NoSQL 資料庫管理系統](#nosql)
- [在 ASP.NET 應用程式中使用 LINQ 查詢](#linq)
- [使用動態資料 scaffolding](#dd)
- [保護資料的存取](#securing)
- [最佳化的資料存取效能](#optimizingdataaccess)
- [部署資料庫](#deploying)
- [透過 Web Service 存取資料](#webservice)
- [其他資源](#additional)

<a id="gettingstarted"></a>

## <a name="getting-started-with-data-access-in-aspnet"></a>開始使用 ASP.NET 中的資料存取

- [資料儲存體選項 （使用 Windows Azure 建置實際的雲端應用程式）](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md)。 關於雲端開發電子書的章節。 許多開發人員熟悉關聯式資料庫通常會忽略除了介紹的 NoSQL 資料庫。 列出思考時選擇關聯式或 NoSQL，或選擇特定的平台指導方針。
- [ASP.NET 資料存取選項](https://msdn.microsoft.com/library/ms178359.aspx)(MSDN)。 簡介適用於 ASP.NET 的關聯式資料庫的資料存取選項，以及有關如何選擇平台和存取方法適用於您案例的指引。
- [關聯式資料庫](http://en.wikipedia.org/wiki/Relational_database)。 維基百科）。 如果您沒有使用過關聯式資料庫，請參閱本頁的關聯式資料庫詞彙和概念簡介。 如需 SQL Server 的介紹尤其請參閱[使用 SQL Server 資料庫](#sqlserver)本主題稍後。

<a id="ef"></a>

## <a name="using-the-entity-framework"></a>使用 Entity Framework

- [Entity Framework 開發方式](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf)(MSDN)。 如何選擇 Entity Framework 開發方式 Database First Model First 或 Code First 指引。

<a id="cf"></a>

### <a name="using-entity-framework-code-first"></a>第一次使用 Entity Framework 程式碼
  

下列教學課程提供可下載的範例應用程式：

- [開始使用 EF 6 使用 MVC 5](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。 涵蓋廣泛的 Entity Framework Code First 案例，包括移轉和 EF 6 功能，例如連接恢復功能，命令攔截非同步處理。 這是更新的版本的[EF 5 / MVC 4 數列](../mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。 較早的序列上的儲存機制和工作單位的模式，不會包含在新的序列包含教學課程。
- [ASP.NET MVC 5 簡介](../mvc/overview/getting-started/introduction/getting-started.md)。 涵蓋較窄範圍的 Entity Framework 程式碼的第一個案例，但並未導入 MVC 功能更完整的工作。
- [模型繫結和 Web Form](https://go.microsoft.com/fwlink/?LinkId=286117)。 使用 Code First 中 Web Forms 應用程式。
- [開始使用 ASP.NET 4.5 Web Form](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md)。 簡介 Web Form 某些涵蓋範圍的第一個程式碼。 使用模型繫結。
- [MVC Music Store](../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1.md)。 使用 Code First 中也會實作成員資格和授權的電子商務 MVC 3 應用程式。 MVC 版本和 ASP.NET 成員資格 （驗證和授權） 系統用在這裡會過期。如需最新的 ASP.NET 成員資格，請參閱[https://asp.net/identity](https://asp.net/identity)。

其他資源：

- [Entity Framework 的程式碼先加入現有的資料庫](https://msdn.microsoft.com/data/jj200620)。 MSDN. 視訊和逐步解說，示範如何使用 Code First 與現有的資料庫。
- [資料開發人員中心 Entity Framework](https://msdn.microsoft.com/data/ef)。 MSDN. 已建立並由 Entity Framework 團隊負責維護的 Entity Framework 文件的指引，請參閱[開始](https://msdn.microsoft.com/data/ee712907)連結。

另請參閱[有關 Entity Framework 的書籍](#efbooks)和[其他 Entity Framework 資源](#otherefresources)本主題稍後。

<a id="efcfmigrations"></a>

### <a name="using-entity-framework-code-first-migrations"></a>使用 Entity Framework Code First 移轉
  

大部分程式碼第一個教學課程涵蓋移轉上面所列。 另請參閱下列資源。

- [使用 Visual Studio 的 ASP.NET Web 部署](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md)。 2-部分的教學課程，示範如何使用 Code First 移轉，將資料庫部署。
- [將成員資格、 OAuth、 與 SQL Database 的安全的 ASP.NET MVC 5 應用程式部署至 Windows Azure 網站](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)。 Microsoft Azure). 如何使用移轉將部署至 Azure 的成員資格和應用程式資料。
- [Visual Studio 和 ASP.NET 的 web 部署概觀](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment)。 請參閱**Visual Studio 中設定資料庫部署**一節說明如何 Code First 移轉已整合至 Visual Studio web 部署功能。
- [資料開發人員中心 Code First 移轉](https://msdn.microsoft.com/data/jj591621)(MSDN)。 Entity Framework 小組的移轉文件。
- [移轉錄影畫面數列](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx)。 EF 部落格）。 在 Code First 移轉中的進階主題上的三個視訊。
- [Code First 移轉與 ASP.NET Web Pages 站台中](http://www.mikesdotnetting.com/Article/217/Code-First-Migrations-With-ASP.NET-Web-Pages-Sites)。 Mikesdotnetting 部落格）。 示範如何使用 ASP.NET Web Pages 站台使用 Code First 移轉，將資料內容放在 Visual Studio 類別庫專案中。

<a id="efdbf"></a>

### <a name="using-entity-framework-database-first-or-model-first-the-ef-designer"></a>第一次使用 Entity Framework 資料庫或模型的第一個 （EF 設計工具）

- [開始使用 Entity Framework 6 資料庫優先使用 MVC 5](../mvc/overview/getting-started/database-first-development/setting-up-database.md)。 在伺服器總管 中建立資料庫，執行指令碼，然後使用 Entity Framework 設計工具來建立資料模型。 顯示如何建立簡單的 CRUD web 頁面，以及其他資料處理函式因為 EF 的所有工作流程使用相同的 DbContext API 遵循其中一個程式碼第一次的教學課程。

下列資源還舊。 如果您想要使用 4.0 版的 Entity Framework 中，而且您想要使用 Web Form 應用程式中的資料繫結資料來源控制項，它們非常有用。

- [開始使用 Entity Framework 4.0](../web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md)。 示範如何使用**EntityDataSource**控制項。
- [有了 Entity Framework 繼續](../web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)(示範如何使用**ObjectDataSource**控制項。 包含的教學課程的並行處理、 EF 效能上的教學課程和的 EF 4.0 新功能的教學課程。

<a id="efrelateddata"></a>

### <a name="handling-related-data-in-entity-framework-lazy-loading-eager-loading-and-explicit-loading"></a>處理相關之資料的 Entity Framework （延遲載入、 積極式載入，和明確載入）

- [讀取與 Entity Framework 中的 ASP.NET MVC 應用程式的相關的資料](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)。 首先，程式碼 MVC 範例應用程式。 所顯示的方法也適用於 Web Form 模型繫結與資料庫第一次的工作流程。
- [資料開發人員中心-載入相關的實體](https://msdn.microsoft.com/data/jj574232)(MSDN)。 載入相關的 Entity Framework 小組的文件的相關資料。

<a id="optimizingef"></a>

### <a name="optimizing-entity-framework-performance"></a>最佳化 Entity Framework 效能

- [進階 ASP.NET 應用程式的 Entity Framework 案例](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application.md)。 顯示如何執行您自己的 SQL 陳述式或呼叫預存程序、 如何停用變更偵測和如何停用驗證時儲存變更。
- [效能考量，Entity framework 5](https://msdn.microsoft.com/data/hh949853) (MSDN)。
- [效能考量 (Entity Framework)](https://msdn.microsoft.com/library/cc853327) (MSDN)。
- [使用 Entity Framework 中的 ASP.NET Web 應用程式的效能最大化](../web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)。 適用於 Entity Framework 4.0。
- 另請參閱[最佳化 ASP.NET 資料存取](#optimizingdataaccess)本主題稍後。

<a id="efconcurrency"></a>

### <a name="handling-concurrency-in-an-entity-framework-application"></a>處理 Entity Framework 應用程式中的並行存取

- [處理並行的 Entity Framework 中的 ASP.NET MVC 應用程式與](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)。 首先，程式碼 DbContext API，使用 MVC 範例應用程式。
- [資料開發人員中心 – 開放式並行存取模式](https://msdn.microsoft.cus/data/jj592904)(MSDN)。 Entity Framework 小組的並行存取文件。
- [處理並行的 Entity Framework 中的 ASP.NET Web 應用程式與](../web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)。 適用於 Entity Framework 4.0。 首先，資料庫 ObjectContext API，使用 Web Form 範例應用程式。

<a id="efrepository"></a><a id="efbooks"></a>

### <a name="books-about-the-entity-framework"></a>有關 Entity Framework 的書籍

- [程式設計 Entity Framework: DbContext](http://shop.oreilly.com/product/0636920022237.do) Julie Lerman 和 Rowan Miller。
- [程式設計 Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) Julie Lerman 和 Rowan Miller。

兩本書的是最新的目前項目的建議技術。 它們在網際網路上提供可用的任何項目比 Entity Framework 更完整還容易遵循介紹。 另一個活頁簿，[程式設計 Entity Framework](http://shop.oreilly.com/product/9780596807252.do)根據 Julie Lerman 是較大且更廣泛，但它是較舊且它涵蓋的技術大多不會再使用 Entity Framework 的建議的方式。 另請參閱 Entity Framework 團隊所建議的書籍清單[資料開發人員中心書籍](https://msdn.microsoft.com/data/aa937716)MSDN 網站上的。

<a id="otherefresources"></a>

### <a name="other-entity-framework-resources"></a>其他 Entity Framework 資源

- [Entity Framework (ADO.NET) 小組部落格](https://blogs.msdn.com/b/adonet/)。 其中一個最新的資訊以及宣布新的增強功能的最佳資源。 其他 EF 相關部落格，請參閱在推薦[開始使用 Entity Framework](https://msdn.microsoft.com/data/ee712907)。
- [MSDN Magazine](https://msdn.microsoft.com/magazine/default.aspx)。 請參閱**資料點**通常是 Entity Framework 的相關主題的相關資料行。

<a id="wfdatabinding"></a>

## <a name="data-binding-in-aspnet-web-forms-applications"></a>資料繫結的 ASP.NET Web Form 應用程式

- [ASP.NET Web Form 的資料存取選項](https://msdn.microsoft.com/library/jj822927.aspx)(MSDN)<a id="wfmodelbinding"></a>。

<a id="wfmodelbinding"></a>

### <a name="using-web-forms-model-binding"></a>使用 Web Form 模型繫結

- [模型繫結和 Web Form](https://go.microsoft.com/fwlink/?LinkId=286117)。 使用 EF Code First 教學課程的系列。
- [Web Form 模型繫結第 1 部分： 選取資料](https://weblogs.asp.net/scottgu/archive/2011/09/06/web-forms-model-binding-part-1-selecting-data-asp-net-vnext-series.aspx)（Scott Guthrie 的部落格）。 在這些較舊的部落格文章，目前名為 ItemType 屬性名為 ModelType，但所包含的資訊無效，否則為。
- [Web Form 模型繫結第 2 部分： 篩選資料](https://weblogs.asp.net/scottgu/archive/2011/09/12/web-forms-model-binding-part-2-filtering-data-asp-net-vnext-series.aspx)（Scott Guthrie 的部落格）。
- [Web Form 模型繫結第 3 部分： 更新和驗證](https://weblogs.asp.net/scottgu/archive/2011/10/30/web-forms-model-binding-part-3-updating-and-validation-asp-net-4-5-series.aspx)（Scott Guthrie 的部落格）。
- [ASP.NET 4.5 Web Form 模型繫結](../web-forms/videos/aspnet-web-forms-vnext/aspnet-45-web-forms-model-binding.md)。 （視訊）。
- [模型繫結組件 1-選取的資料](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-1-selecting-data.md)（視訊）。
- [模型繫結組件 2-篩選](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-2-filtering.md)（視訊）。
- [快速入門開始使用 ASP.NET 4.5 Web Form-顯示資料的項目和詳細資料](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md)。

<a id="wfdsc"></a>

### <a name="using-web-forms-data-source-controls"></a>使用 Web Form 的資料來源控制項

- [資料來源 Web 伺服器控制項](https://msdn.microsoft.com/library/ms247258.aspx)(MSDN)。
- [宣佈適用於 Entity Framework 6 EntityDataSource 控制項和動態的資料提供者版本](https://blogs.msdn.com/b/webdev/archive/2014/02/28/announcing-the-release-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx)（Microsoft Web 開發部落格）。

<a id="wfdbc"></a>

### <a name="using-web-forms-data-bound-controls-and-data-binding-expressions"></a>使用 Web Form 資料繫結控制項和資料繫結運算式

- [模型繫結和 Web Form](https://go.microsoft.com/fwlink/?LinkId=286117)。 使用 EF Code First 的教學課程系列。
- [快速入門開始使用 ASP.NET 4.5 Web Form-顯示資料的項目和詳細資料](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md)。
- [強型別資料控制項](https://weblogs.asp.net/scottgu/archive/2011/09/02/strongly-typed-data-controls-asp-net-vnext-series.aspx)（Scott Guthrie 的部落格）。
- [強型別資料控制項](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md)（視訊）。
- [ASP.NET 4.5 Web Form 強式型別資料控制項](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md)（視訊）。
- [資料繫結的 Web 伺服器控制項](https://msdn.microsoft.com/library/ms228214.aspx)(MSDN)。
- [資料繫結運算式概觀](https://msdn.microsoft.com/library/ms178366.aspx)(MSDN)。 此頁面只涵蓋**Eval**和**繫結**; 尚未更新以包含**項目**和**BindItem**。

<a id="sqlserver"></a>

## <a name="working-with-sql-server-databases"></a>使用 SQL Server 資料庫

- [SQL Server 資料庫功能](https://msdn.microsoft.com/library/hh230827.aspx)(MSDN)。 各種不同的 SQL Server 主題的一般簡介，請參閱下一個目錄中的項目。
- [SQL Server 版本](https://msdn.microsoft.com/library/ms178359.aspx#sqlserver)(MSDN)。 摘要可用的 SQL Server 版本，以及有關每個的詳細資訊的連結。）
- [ASP.NET Web 應用程式的 SQL Server 連接字串](https://msdn.microsoft.com/library/jj653752.aspx)(MSDN)。
- [ASP.NET Web 應用程式使用 SQL Server Compact](https://msdn.microsoft.com/library/ms247257.aspx) (MSDN)。
- [Microsoft SQL Server： 資料庫產品範例](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md)。 範例 AdventureWorks 資料庫。
- [安裝範例資料庫](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md)。 除了如下所示的方法，您也可以下載範例.mdf 檔案的其中一個應用程式\_Data 資料夾的 web 專案中，將資料庫轉換成 LocalDB，並建立 LocalDB 的連接字串。 如需如何進行這項資訊，請參閱[如何： 升級為 LocalDB](https://msdn.microsoft.com/library/hh873188.aspx)。

另請參閱下列各節處理與 SQL Server Express LocalDB 和 SQL Server 和 SQL Database 之間做選擇。

<a id="sslocaldb"></a>

### <a name="working-with-sql-server-express-localdb-databases"></a>使用 SQL Server Express LocalDB 資料庫

- [SQL Server Express 2012 LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (MSDN). 官方 MSDN 簡介 LocalDB。
- [ASP.NET Web 應用程式的 SQL Server 連接字串](https://msdn.microsoft.com/library/jj653752.aspx)(MSDN)。
- [如何： 升級為 LocalDB](https://msdn.microsoft.com/library/hh873188.aspx) (MSDN)。 如何將移轉的.mdf 檔案從舊版的 SQL Server Express localdb。 您也必須進行此程序，如果您要下載的其中一個[SQL Server 2012 範例資料庫](https://go.microsoft.com/fwlink/?linkid=117483)。
- [簡介改善 SQL Express LocalDB](https://go.microsoft.com/fwlink/?LinkId=234375) （SQL Server Express 的部落格）。 有比隨附於 MSDN，為何建立 LocalDB 的背景。
- [LocalDB： 其中是我的資料庫？](https://go.microsoft.com/fwlink/?LinkId=234376) （SQL Server Express 部落格）。 在其中建立 LocalDB 資料庫檔案的相關資訊。
- [使用 LocalDB 使用完整 IIS 時，第 1 部分： 使用者設定檔](https://blogs.msdn.com/b/sqlexpress/archive/2011/12/09/using-localdb-with-full-iis-part-1-user-profile.aspx)（SQL Server Express 的部落格）。 LocalDB 不是使用 IIS。 這一系列部落格文章的說明，以及一些因應措施的問題。

<a id="sse"></a>

### <a name="working-with-sql-server-express-databases"></a>使用 SQL Server Express 資料庫

- [ASP.NET Web 應用程式的 SQL Server 連接字串](https://msdn.microsoft.com/library/jj653752.aspx)(MSDN)。 如果您使用 AttachDBFileName 連接字串設定以 SQL Server Express，請參閱特別是此頁面的使用者執行個體一節。
- [如何取得擁有權，您本機 SQL Server Express 2008](https://blogs.msdn.com/b/sqlexpress/archive/2010/02/23/how-to-take-ownership-of-your-local-sql-server-2008-express.aspx) （SQL Server Express 的部落格）。 常見的問題無法使用 SQL Server Express 資料庫，因為您不是 SQL Server Express 執行個體上的系統管理員。 根據預設，只安裝 SQL Server Express 的人員是系統管理員。 這篇部落格說明如果您的電腦上的系統管理員，SQL Server Express 的系統管理員將自己的方法。
- [我的 ASP.NET web 應用程式可以在生產環境中使用的 SQL Server Express 資料庫嗎？](https://msdn.microsoft.com/library/jj653753.aspx#sql_express_in_production) (MSDN)。

<a id="ssdb"></a>

### <a name="working-with-windows-azure-sql-database"></a>使用 Windows Azure SQL Database

- [將成員資格、 OAuth、 與 SQL Database 的安全 ASP.NET MVC 應用程式部署至 Windows Azure 網站](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)（Microsoft Azure 站台）。
- [SQL 資料庫](https://docs.microsoft.com/azure/sql-database/)（Microsoft Azure 站台）。 快速入門教學課程和使用說明指南。
- [Windows Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/ee336279.aspx) (MSDN)。 SQL database MSDN 中的目錄中最上層節點。
- [Windows Azure SQL Database TechNet Wiki 文章索引](https://social.technet.microsoft.com/wiki/contents/articles/2267.windows-azure-sql-database-technet-wiki-articles-index-en-us.aspx)（Microsoft TechNet 網站）。
- [暫時性錯誤處理應用程式區塊](https://msdn.microsoft.com/library/hh680934(v=PandP.50).aspx)。 一種架構，可讓您處理暫時性網路錯誤及連接錯誤的節流的結果。 NuGet 套件中提供： [Enterprise Library 5.0-暫時性錯誤處理應用程式區塊](http://nuget.org/packages/EnterpriseLibrary.WindowsAzure.TransientFaultHandling)。
- [開始使用 SQL Database 和 Entity Framework](https://msdn.microsoft.com/data/jj556244) (MSDN)。
- [Windows Azure 訓練套件](https://www.microsoft.com/download/details.aspx?id=8396)（Microsoft 下載中心）。 包含 SQL Database 實際操作實驗室。
- [Windows Azure SQL Database 社群論壇](https://social.msdn.microsoft.com/Forums/ssdsgetstarted/threads)。
- [移至 Windows Azure SQL Database](https://msdn.microsoft.com/library/ff803375.aspx) (MSDN)。 一個完整的端對端案例，由 Microsoft Patterns and Practices 團隊的章節。 涵蓋您可能想要移轉，以及如何從 SQL Server 移轉至 SQL Database。
- [將 SQL Server 資料庫移轉至 Windows Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx) (MSDN)。
- [SQL Database 移轉精靈](http://sqlazuremw.codeplex.com/)。 與 SQL Database 的資料庫移轉的開放原始碼工具。

<a id="ssdbchoosing"></a>

### <a name="choosing-between-sql-server-and-windows-azure-sql-database"></a>SQL Server 和 Windows Azure SQL Database 之間選擇

- [比較 SQL Server 與 Windows Azure SQL Database](https://social.technet.microsoft.com/wiki/contents/articles/996.compare-sql-server-with-windows-azure-sql-database-en-us.aspx) （Microsoft TechNet 網站）。
- [資料移轉到 Windows Azure SQL Database： 工具和技術](https://msdn.microsoft.com/library/windowsazure/hh694043.aspx)(MSDN)。 包含比較 SQL 資料庫的 SQL Server，並提供指引時從 SQL Server 移轉至 SQL Database 的區段。
- [Windows Azure SQL Database 傳遞指南](https://social.technet.microsoft.com/wiki/contents/articles/3398.windows-azure-sql-database-delivery-guide-en-us.aspx)（Microsoft TechNet 網站）。
- [SQL Server 的功能限制 (Windows Azure SQL Database)](https://msdn.microsoft.com/library/windowsazure/ff394115.aspx) (MSDN)。
- [Windows Azure 資料表儲存體和 Windows Azure SQL Database-比較和對比](https://msdn.microsoft.com/library/jj553018.aspx)(MSDN)。 您將部署到 Windows Azure 的應用程式，Windows Azure 資料表儲存體可能是 Windows Azure SQL Database 的替代方式。 本主題可協助您決定這些替代方案。
- [Windows Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/ee336279) (MSDN)。
- [方針和限制 (Windows Azure SQL Database)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx)

<a id="nosql"></a>

## <a name="working-with-nosql-database-management-systems"></a>使用 NoSQL 資料庫管理系統

- [Windows Azure 資料服務](https://www.windowsazure.com/develop/net/data/)（Microsoft Azure 站台）。 請參閱[表格服務功能指南](https://docs.microsoft.com/azure/)和**巨量資料**頁面區段。
- [ASP.NET 多層式應用程式使用的儲存體資料表、 佇列和 Blob](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) （Microsoft Azure 站台）。 與使用 Windows Azure 儲存體 NoSQL 資料表的可下載範例應用程式的端對端教學課程。

<a id="linq"></a>

## <a name="using-linq-queries-in-aspnet-applications"></a>在 ASP.NET 應用程式中使用 LINQ 查詢

- [ASP.NET 資料存取選項](https://msdn.microsoft.com/library/ms178359.aspx#linq)(MSDN)。 包含 LINQ 的簡介。
- [LINQ 訓練影片](http://www.misfitgeek.com/windows-client-linq-training-videos-20/)（Joe stagner 以部落格）。
- [ASP.NET 論壇執行緒動態 LINQ 資源的連結](https://forums.asp.net/p/1961037/5601994.aspx?Please+suggest+good+books+so+that+one+can+write+and+understand+dynamic+linq)。

<a id="dd"></a>

## <a name="using-dynamic-data-scaffolding"></a>使用動態資料 Scaffolding

- [動態資料專案範本](https://msdn.microsoft.com/library/jj822927.aspx#dynamicdata)(MSDN)。 何時使用動態資料專案的指引。
- [ASP.NET 動態資料](https://msdn.microsoft.com/library/ee845452.aspx)(MSDN)。

<a id="securing"></a>

## <a name="securing-data-access"></a>保護資料的存取

- [保護在 ASP.NET 中的資料存取](https://msdn.microsoft.com/library/ms178375.aspx)(MSDN)。
- [安全性考量 (Entity Framework)](https://msdn.microsoft.com/library/cc716760.aspx) (MSDN)。
- [如何： 使用資料來源控制項時，使用安全連接字串](https://msdn.microsoft.com/library/ms178372.aspx)(MSDN)。

<a id="optimizingdataaccess"></a>

## <a name="optimizing-data-access-performance"></a>最佳化的資料存取效能

- [ASP.NET 效能概觀](https://msdn.microsoft.com/library/cc668225.aspx)(MSDN)。
- [ASP.NET 快取](https://msdn.microsoft.com/library/xsbfdd8c.aspx)(MSDN)。
- [改進 ASP.NET 效能](https://msdn.microsoft.com/library/ff647787)(MSDN)。 沒有頂端的這個頁面上，「 已停用內容 」 警告，但大部分的資訊仍相關，而且沒有任何可比較的更新的資源。
- [改善 SQL Server 效能](https://msdn.microsoft.com/library/ff647793)(MSDN)。 前一個連結為相同的註解。

另請參閱[最佳化 Entity Framework 效能](#optimizingef)稍早在本主題。

<a id="deploying"></a>

## <a name="deploying-a-database"></a>部署資料庫

- [ASP.NET Web 部署-建議資源](aspnet-web-deployment-content-map.md)(MSDN)。

<a id="webservice"></a>

## <a name="accessing-data-through-a-web-service"></a>透過 Web Service 存取資料

- [透過 Web Service 存取資料](https://msdn.microsoft.com/library/ms178359.aspx#webservice)(MSDN)。 何時使用 Web API 與 WCF 的指引。
- [開始使用 ASP.NET Web API](../web-api/index.md)。
- [WCF Data Services](https://msdn.microsoft.com/data/bb931106) (MSDN)。

<a id="additional"></a>

## <a name="additional-resources"></a>其他資源

- [ASP.NET 資料存取常見問題集](https://msdn.microsoft.com/library/jj653753.aspx)(MSDN)。
- [ASP.NET Web Form 教學課程-資料](../web-forms/overview/data-access/index.md)。 大部分這些教學課程是相對較舊的;請確定您先閱讀[ASP.NET 資料存取選項](https://msdn.microsoft.com/library/ms178359.aspx)和[資料儲存體選項 （建置真實世界雲端應用程式與 Windows Azure）](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md)第一個會在不太遠到不正確的資料存取方法您的案例。
- [ASP.NET MVC 內容地圖](../mvc/overview/getting-started/recommended-resources-for-mvc.md)。
- [ASP.NET Web Pages 教學課程-資料](../web-pages/overview/data/index.md)。
- [在 Visual Studio 中存取資料](https://msdn.microsoft.com/library/wzabh8c4.aspx)(MSDN)。 提供一份連結類似此內容對應，但將焦點設在 Visual Studio，而不是 ASP.NET。
