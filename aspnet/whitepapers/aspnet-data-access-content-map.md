---
uid: whitepapers/aspnet-data-access-content-map
title: ASP.NET 資料存取-建議資源 |Microsoft Docs
author: rick-anderson
description: 本主題提供有關如何存取資料，在 ASP.NET web 應用程式，主要是由使用 Entity Framework 和 SQL Se 文件資源的連結...
ms.author: riande
ms.date: 07/25/2013
ms.assetid: f8157be1-4ab9-469e-ad3a-0ccc80b56c00
msc.legacyurl: /whitepapers/aspnet-data-access-content-map
msc.type: content
ms.openlocfilehash: 6993c17c8de890cbaa40c619bcd20f494bfd2f90
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833028"
---
<a name="aspnet-data-access---recommended-resources"></a>ASP.NET 資料存取-建議資源
====================
> 本主題提供有關如何存取資料，在 ASP.NET web 應用程式，主要是由使用 Entity Framework 和 SQL Server 文件資源的連結。
> 
> 如果您知道很棒的部落格文章[stackoverflow](http://stackoverflow.com)執行緒或任何其他會很有用的連結[傳送一封電子郵件給我們](mailto:aspnetue@microsoft.com?subject=Data Access Content Map)與連結。
> 
> 上次更新的 4/3/2014


此主題包括下列各節：

- [Getting Started with ASP.NET 中的資料存取](#gettingstarted)
- [使用 Entity Framework](#ef)

    - [第一次使用 Entity Framework 程式碼](#cf)
    - [使用 Entity Framework Code First 移轉](#efcfmigrations)
    - [第一次使用 Entity Framework 資料庫或 Model First （EF 設計工具）](#efdbf)
    - [載入相關的資料，在 Entity Framework （消極式載入，積極式載入，並明確載入）](#efrelateddata)
    - [最佳化的 Entity Framework 效能](#optimizingef)
    - [處理 Entity Framework 應用程式中的並行存取](#efconcurrency)
    - [Entity Framework 相關書籍](#efbooks)
    - [Entity Framework 的其他資源](#otherefresources)
- [資料繫結在 ASP.NET Web Forms 應用程式](#wfdatabinding)

    - [使用 Web Form 模型繫結](#wfmodelbinding)
    - [使用 Web Form 資料來源控制項](#wfdsc)
    - [使用 Web Form 資料繫結控制項和資料繫結運算式](#wfdbc)
- [使用 SQL Server 資料庫](#sqlserver)

    - [使用 SQL Server Express LocalDB 資料庫](#sslocaldb)
    - [使用 SQL Server Express 資料庫](#sse)
    - [使用 Windows Azure SQL Database](#ssdb)
    - [SQL Server 和 Windows Azure SQL Database 之間進行選擇](#ssdbchoosing)
- [使用 NoSQL 資料庫管理系統](#nosql)
- [在 ASP.NET 應用程式中使用 LINQ 查詢](#linq)
- [使用動態資料 scaffolding](#dd)
- [保護資料的存取](#securing)
- [最佳化的資料存取效能](#optimizingdataaccess)
- [部署資料庫](#deploying)
- [透過 Web 服務存取資料](#webservice)
- [其他資源](#additional)

<a id="gettingstarted"></a>

## <a name="getting-started-with-data-access-in-aspnet"></a>Getting Started with ASP.NET 中的資料存取

- [資料儲存體選項 （使用 Windows Azure 建置真實世界的雲端應用程式）](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md)。 開發雲端產品的相關電子書的章節。 介紹許多開發人員熟悉關聯式資料庫會嘗試尋找替代的 NoSQL 資料庫。 提供在考慮事項時選擇關聯式或 NoSQL，或選擇特定的平台上的指導方針。
- [ASP.NET 資料存取選項](https://msdn.microsoft.com/library/ms178359.aspx)(MSDN)。 介紹適用於 ASP.NET 的關聯式資料庫的資料存取選項以及如何選擇平台和存取適用於您案例的方法指引。
- [關聯式資料庫](http://en.wikipedia.org/wiki/Relational_database)。 維基百科）。 如果您未曾用過關聯式資料庫，請參閱本頁的關聯式資料庫詞彙和概念的簡介。 如需 SQL Server 尤其請參閱[使用 SQL Server 資料庫](#sqlserver)本主題稍後的。

<a id="ef"></a>

## <a name="using-the-entity-framework"></a>使用 Entity Framework

- [Entity Framework 開發方式](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf)(MSDN)。 如何選擇 Entity Framework 的開發方式 Database First Model First 或 Code First 的指引。

<a id="cf"></a>

### <a name="using-entity-framework-code-first"></a>第一次使用 Entity Framework 程式碼
  

下列教學課程提供可下載的範例應用程式：

- [開始使用 MVC 5 的 EF 6](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。 涵蓋廣泛的 Entity Framework Code First 的案例，包括移轉和 EF 6 功能連線恢復、 命令攔截和非同步等。 這是更新的版[EF 5 / MVC 4 系列](../mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。 較早的系列中包括教學課程上儲存機制並不會納入新系列的工作單位模式。
- [ASP.NET MVC 5 簡介](../mvc/overview/getting-started/introduction/getting-started.md)。 涵蓋較窄範圍的 Entity Framework 程式碼的第一個案例，但並未導入的 MVC 功能更完整的工作。
- [模型繫結和 Web Form](https://go.microsoft.com/fwlink/?LinkId=286117)。 使用 Code First 中的 Web Form 應用程式。
- [Getting Started with ASP.NET 4.5 Web Form](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md)。 介紹 Web Form 部分涵蓋範圍的第一個程式碼。 使用模型繫結。
- [MVC Music 市集](../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1.md)。 使用 Code First 中也會實作成員資格和授權的電子商務 MVC 3 應用程式。 ASP.NET 成員資格 （驗證和授權） 系統此處所使用的 MVC 版本已過期;在 ASP.NET 成員資格的多個最新狀態資訊，請參閱[ https://asp.net/identity ](https://asp.net/identity)。

其他資源：

- [Entity Framework-Code First 至現有的資料庫](https://msdn.microsoft.com/data/jj200620)。 MSDN。 影片和逐步解說，示範如何使用 Code First 與現有的資料庫。
- [資料開發人員中心-Entity Framework](https://msdn.microsoft.com/data/ef)。 MSDN。 Entity Framework 文件，已建立並由 Entity Framework 小組所維護的指引，請參閱 <<c0> [ 開始](https://msdn.microsoft.com/data/ee712907)連結。

另請參閱[Entity Framework 相關書籍](#efbooks)並[其他 Entity Framework 資源](#otherefresources)本主題稍後的。

<a id="efcfmigrations"></a>

### <a name="using-entity-framework-code-first-migrations"></a>使用 Entity Framework Code First 移轉
  

大部分上述涵蓋移轉程式碼第一個教學課程。 另請參閱下列資源。

- [使用 Visual Studio 的 ASP.NET Web 部署](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md)。 2-部分教學課程系列，示範如何使用 Code First 移轉，以部署資料庫。
- [將使用成員資格、 OAuth 和 SQL Database 的安全 ASP.NET MVC 5 應用程式部署至 Windows Azure 網站](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)。 Microsoft Azure)。 如何使用移轉來將成員資格和應用程式資料部署至 Azure。
- [適用於 Visual Studio 和 ASP.NET web 部署概觀](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment)。 請參閱**在 Visual Studio 中的 設定資料庫部署**一節說明如何將 Code First 移轉整合到 Visual Studio web 部署功能。
- [資料開發人員中心-Code First 移轉](https://msdn.microsoft.com/data/jj591621)(MSDN)。 Entity Framework 小組的移轉文件。
- [移轉螢幕錄製影片系列](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx)。 EF 部落格）。 在 Code First 移轉中的進階主題的三部影片。
- [程式碼與 ASP.NET Web Pages 站台中的第一個移轉](http://www.mikesdotnetting.com/Article/217/Code-First-Migrations-With-ASP.NET-Web-Pages-Sites)。 Mikesdotnetting 部落格）。 示範如何使用 ASP.NET Web Pages 網站中的 Code First 移轉，將資料內容放在 Visual Studio 類別庫專案中。

<a id="efdbf"></a>

### <a name="using-entity-framework-database-first-or-model-first-the-ef-designer"></a>第一次使用 Entity Framework 資料庫或 Model First （EF 設計工具）

- [開始使用 Entity Framework 6 Database First 使用 MVC 5](../mvc/overview/getting-started/database-first-development/setting-up-database.md)。 執行指令碼在伺服器總管 中建立資料庫，並使用 Entity Framework 設計工具來建立資料模型。 示範如何建立簡單的 CRUD web 頁面，以及其他資料處理函式因為 EF 的所有工作流程使用相同的 DbContext API 遵循其中一個程式碼第一個教學課程。

下列資源還舊。 如果您想要使用 Entity Framework 4.0 版，而您想要用於 Web Form 應用程式中的資料繫結的資料來源控制項，它們非常有用。

- [開始使用 Entity Framework 4.0](../web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md)。 示範如何使用**EntityDataSource**控制項。
- [繼續使用 Entity Framework](../web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)(如何使用**ObjectDataSource**控制項。 包含有關並行處理、 EF 效能的教學課程和教學課程，說明的 EF 4.0 新功能的教學課程。

<a id="efrelateddata"></a>

### <a name="handling-related-data-in-entity-framework-lazy-loading-eager-loading-and-explicit-loading"></a>處理相關之資料的 Entity Framework （消極式載入，積極式載入，並明確載入）

- [讀取相關的資料，使用 ASP.NET MVC 應用程式中的 Entity Framework](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)。 首先，程式碼 MVC 範例應用程式。 顯示的方法也套用至 Web Form 模型繫結和資料庫優先的工作流程。
- [資料開發人員中心-載入相關的實體](https://msdn.microsoft.com/data/jj574232)(MSDN)。 載入相關的 Entity Framework 小組的文件的相關資料。

<a id="optimizingef"></a>

### <a name="optimizing-entity-framework-performance"></a>最佳化的 Entity Framework 效能

- [進階 ASP.NET 應用程式的 Entity Framework 案例](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application.md)。 示範如何執行您自己的 SQL 陳述式或呼叫您自己的預存程序、 如何停用變更偵測，以及如何儲存變更時，停用驗證。
- [Entity Framework 5 的效能考量](https://msdn.microsoft.com/data/hh949853)(MSDN)。
- [效能考量 (Entity Framework)](https://msdn.microsoft.com/library/cc853327) (MSDN)。
- [將使用 Entity Framework 中的 ASP.NET Web 應用程式的效能最大化](../web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)。 適用於 Entity Framework 4.0。
- 另請參閱[最佳化 ASP.NET 資料存取](#optimizingdataaccess)本主題稍後的。

<a id="efconcurrency"></a>

### <a name="handling-concurrency-in-an-entity-framework-application"></a>處理 Entity Framework 應用程式中的並行存取

- [處理使用 Entity Framework 的 ASP.NET MVC 應用程式中的並行](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)。 首先，程式碼使用 MVC 範例應用程式的 DbContext API。
- [資料開發人員中心-開放式並行存取模式](https://msdn.microsoft.cus/data/jj592904)(MSDN)。 Entity Framework 小組的並行存取文件。
- [處理使用 Entity Framework 中的 ASP.NET Web 應用程式的並行](../web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)。 適用於 Entity Framework 4.0。 資料庫第一，ObjectContext API，使用 Web Form 範例應用程式。

<a id="efrepository"></a><a id="efbooks"></a>

### <a name="books-about-the-entity-framework"></a>Entity Framework 相關書籍

- [程式設計 Entity Framework: DbContext](http://shop.oreilly.com/product/0636920022237.do) Julie Lerman 和 Rowan Miller。
- [程式設計 Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) Julie Lerman 和 Rowan Miller。

這兩本書的是最新的目前建議的技術。 它們提供的更完整但容易遵循簡介 Entity Framework 比可用的任何項目在網際網路上。 另一部著作[Programming Entity Framework](http://shop.oreilly.com/product/9780596807252.do)作者： Julie Lerman 更大、 更完整，但它是較舊且其所涵蓋的技術大多不會再使用 Entity Framework 的建議的方式。 另請參閱 Entity Framework 小組所建議的書籍清單[資料開發人員中心書籍](https://msdn.microsoft.com/data/aa937716)MSDN 網站上。

<a id="otherefresources"></a>

### <a name="other-entity-framework-resources"></a>其他 Entity Framework 資源

- [Entity Framework (ADO.NET) 小組部落格](https://blogs.msdn.com/b/adonet/)。 其中一個最新的資訊和宣布新的增強功能的最佳資源。 其他 EF 相關的部落格，請參閱在部落格榜[開始使用 Entity Framework](https://msdn.microsoft.com/data/ee712907)。
- [MSDN Magazine](https://msdn.microsoft.com/magazine/default.aspx)。 請參閱**資料點**資料行，也就是與 Entity Framework 相關的主題相關的常見問題。

<a id="wfdatabinding"></a>

## <a name="data-binding-in-aspnet-web-forms-applications"></a>資料繫結在 ASP.NET Web Forms 應用程式

- [ASP.NET Web Form 資料存取選項](https://msdn.microsoft.com/library/jj822927.aspx)(MSDN)<a id="wfmodelbinding"></a>。

<a id="wfmodelbinding"></a>

### <a name="using-web-forms-model-binding"></a>使用 Web Form 模型繫結

- [模型繫結和 Web Form](https://go.microsoft.com/fwlink/?LinkId=286117)。 使用 EF Code First 的教學課程系列。
- [Web Form 模型繫結第 1 部分： 選取資料](https://weblogs.asp.net/scottgu/archive/2011/09/06/web-forms-model-binding-part-1-selecting-data-asp-net-vnext-series.aspx)（Scott Guthrie 的部落格）。 在這些較舊的部落格文章，目前名為 ItemType 屬性名為模型類型，但所包含的資訊無效，否則為。
- [Web Form 模型繫結第 2 部分： 篩選資料](https://weblogs.asp.net/scottgu/archive/2011/09/12/web-forms-model-binding-part-2-filtering-data-asp-net-vnext-series.aspx)（Scott Guthrie 的部落格）。
- [Web Form 模型繫結第 3 部分： 更新和驗證](https://weblogs.asp.net/scottgu/archive/2011/10/30/web-forms-model-binding-part-3-updating-and-validation-asp-net-4-5-series.aspx)（Scott Guthrie 的部落格）。
- [ASP.NET 4.5 Web Form 模型繫結](../web-forms/videos/aspnet-web-forms-vnext/aspnet-45-web-forms-model-binding.md)。 （影片）。
- [模型繫結第 1 部分-選取資料](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-1-selecting-data.md)（影片）。
- [模型繫結第 2 部分-篩選](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-2-filtering.md)（影片）。
- [Getting Started with ASP.NET 4.5 Web Form-顯示資料的項目和詳細資料](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md)。

<a id="wfdsc"></a>

### <a name="using-web-forms-data-source-controls"></a>使用 Web Form 資料來源控制項

- [資料來源 Web 伺服器控制項](https://msdn.microsoft.com/library/ms247258.aspx)(MSDN)。
- [發表最新的動態資料提供者和 EntityDataSource 控制項的 Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/02/28/announcing-the-release-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx) （Microsoft Web 程式開發部落格）。

<a id="wfdbc"></a>

### <a name="using-web-forms-data-bound-controls-and-data-binding-expressions"></a>使用 Web Form 資料繫結控制項和資料繫結運算式

- [模型繫結和 Web Form](https://go.microsoft.com/fwlink/?LinkId=286117)。 使用 EF Code First 的教學課程系列。
- [Getting Started with ASP.NET 4.5 Web Form-顯示資料的項目和詳細資料](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md)。
- [強型別資料控制項](https://weblogs.asp.net/scottgu/archive/2011/09/02/strongly-typed-data-controls-asp-net-vnext-series.aspx)（Scott Guthrie 的部落格）。
- [強型別資料控制項](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md)（影片）。
- [ASP.NET 4.5 Web Form 強型別資料控制項](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md)（影片）。
- [資料繫結的 Web 伺服器控制項](https://msdn.microsoft.com/library/ms228214.aspx)(MSDN)。
- [資料繫結運算式概觀](https://msdn.microsoft.com/library/ms178366.aspx)(MSDN)。 此頁面僅涵蓋**Eval**並**繫結**; 尚未更新以包含**項目**並**BindItem**。

<a id="sqlserver"></a>

## <a name="working-with-sql-server-databases"></a>使用 SQL Server 資料庫

- [SQL Server 資料庫功能](https://msdn.microsoft.com/library/hh230827.aspx)(MSDN)。 各種不同的 SQL Server 主題的一般簡介，請參閱底下的目錄中的項目。
- [SQL Server 版本](https://msdn.microsoft.com/library/ms178359.aspx#sqlserver)(MSDN)。 摘要可用的 SQL Server 版本中，而每一項的詳細資訊的連結。）
- [ASP.NET Web 應用程式的 SQL Server 連接字串](https://msdn.microsoft.com/library/jj653752.aspx)(MSDN)。
- [ASP.NET Web 應用程式中使用 SQL Server Compact](https://msdn.microsoft.com/library/ms247257.aspx) (MSDN)。
- [Microsoft SQL Server： 資料庫產品範例](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md)。 範例 AdventureWorks 資料庫。
- [安裝範例資料庫](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md)。 除了如下所示的方法，您也可以下載範例.mdf 檔案的其中一個應用程式\_Data 資料夾的 web 專案時，將資料庫轉換成 LocalDB，並建立 LocalDB 連接字串。 如需如何執行該動作的資訊，請參閱[如何： 升級為 LocalDB](https://msdn.microsoft.com/library/hh873188.aspx)。

另請參閱下列各節上處理與 SQL Server Express LocalDB 和 SQL Server 和 SQL Database 之間進行選擇。

<a id="sslocaldb"></a>

### <a name="working-with-sql-server-express-localdb-databases"></a>使用 SQL Server Express LocalDB 資料庫

- [SQL Server Express 2012 LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (MSDN)。 官方 MSDN 簡介 LocalDB。
- [ASP.NET Web 應用程式的 SQL Server 連接字串](https://msdn.microsoft.com/library/jj653752.aspx)(MSDN)。
- [如何： 升級為 LocalDB](https://msdn.microsoft.com/library/hh873188.aspx) (MSDN)。 如何以.mdf 檔案從移轉舊版的 SQL Server Express localdb。 您也必須進行此程序，如果您下載其中一種[SQL Server 2012 範例資料庫](https://go.microsoft.com/fwlink/?linkid=117483)。
- [簡介改善 SQL Express LocalDB](https://go.microsoft.com/fwlink/?LinkId=234375) （SQL Server Express 部落格）。 有更多的背景上比在 MSDN，為何建立 LocalDB。
- [LocalDB： 其中是我的資料庫？](https://go.microsoft.com/fwlink/?LinkId=234376) （SQL Server Express 部落格）。 LocalDB 資料庫檔案建立所在的相關資訊。
- [使用 LocalDB 與完整 IIS，第 1 部分： 使用者設定檔](https://blogs.msdn.com/b/sqlexpress/archive/2011/12/09/using-localdb-with-full-iis-part-1-user-profile.aspx)（SQL Server Express 部落格）。 LocalDB 的目的不是搭配 IIS 運作。 這一系列部落格文章說明的問題，以及一些因應措施。

<a id="sse"></a>

### <a name="working-with-sql-server-express-databases"></a>使用 SQL Server Express 資料庫

- [ASP.NET Web 應用程式的 SQL Server 連接字串](https://msdn.microsoft.com/library/jj653752.aspx)(MSDN)。 如果您使用 SQL Server Express 使用 AttachDBFileName 連接字串設定，請參閱尤其是此頁面的使用者執行個體一節。
- [如何取得擁有權，您本機 SQL Server Express 2008](https://blogs.msdn.com/b/sqlexpress/archive/2010/02/23/how-to-take-ownership-of-your-local-sql-server-2008-express.aspx) （SQL Server Express 部落格）。 常見的問題不能夠使用 SQL Server Express 資料庫，因為您不是 SQL Server Express 執行個體上的系統管理員。 根據預設，只有在安裝 SQL Server Express 的人員會是系統管理員。 這篇部落格說明如何讓自己成為 SQL Server Express 的系統管理員，如果您是在電腦上的系統管理員。
- [我的 ASP.NET web 應用程式可以在生產環境中使用 SQL Server Express 資料庫？](https://msdn.microsoft.com/library/jj653753.aspx#sql_express_in_production) (MSDN)。

<a id="ssdb"></a>

### <a name="working-with-windows-azure-sql-database"></a>使用 Windows Azure SQL Database

- [將使用成員資格、 OAuth 和 SQL Database 的安全 ASP.NET MVC 應用程式部署至 Windows Azure 網站](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)（Microsoft Azure 站台）。
- [SQL Database](https://docs.microsoft.com/azure/sql-database/) （Microsoft Azure 站台）。 取得開始使用教學課程和操作方法指南。
- [Windows Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/ee336279.aspx) (MSDN)。 MSDN 中的 SQL database 的目錄中最上層節點。
- [Windows Azure SQL Database TechNet Wiki 文章索引](https://social.technet.microsoft.com/wiki/contents/articles/2267.windows-azure-sql-database-technet-wiki-articles-index-en-us.aspx)（Microsoft TechNet 網站）。
- [暫時性錯誤處理應用程式區塊](https://msdn.microsoft.com/library/hh680934(v=PandP.50).aspx)。 一種架構，可讓您以處理暫時性網路錯誤及連線錯誤所造成的節流。 NuGet 封裝： [Enterprise Library 5.0-暫時性錯誤處理應用程式區塊](http://nuget.org/packages/EnterpriseLibrary.WindowsAzure.TransientFaultHandling)。
- [開始使用 SQL Database 和 Entity Framework](https://msdn.microsoft.com/data/jj556244) (MSDN)。
- [Windows Azure 訓練套件](https://www.microsoft.com/download/details.aspx?id=8396)（Microsoft 下載中心取得）。 包含 SQL Database 實際操作實驗室。
- [Windows Azure SQL Database 社群論壇](https://social.msdn.microsoft.com/Forums/ssdsgetstarted/threads)。
- [移至 Windows Azure SQL Database](https://msdn.microsoft.com/library/ff803375.aspx) (MSDN)。 一個完整的端對端案例，由 Microsoft Patterns and Practices 小組的章節。 涵蓋您可能想要移轉，以及如何從 SQL Server 移轉至 SQL Database。
- [SQL Server 資料庫移轉至 Windows Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx) (MSDN)。
- [SQL Database 移轉精靈](http://sqlazuremw.codeplex.com/)。 開放原始碼工具，移轉資料庫與 SQL Database。

<a id="ssdbchoosing"></a>

### <a name="choosing-between-sql-server-and-windows-azure-sql-database"></a>SQL Server 和 Windows Azure SQL Database 之間進行選擇

- [比較 SQL Server 與 Windows Azure SQL Database](https://social.technet.microsoft.com/wiki/contents/articles/996.compare-sql-server-with-windows-azure-sql-database-en-us.aspx) （Microsoft TechNet 網站）。
- [資料移轉到 Windows Azure SQL Database： 工具和技術](https://msdn.microsoft.com/library/windowsazure/hh694043.aspx)(MSDN)。 包含比較 SQL Sever 到 SQL Database，並提供關於何時應該將從 SQL Server 移轉至 SQL Database 的指引的章節。
- [Windows Azure SQL Database 傳遞指南](https://social.technet.microsoft.com/wiki/contents/articles/3398.windows-azure-sql-database-delivery-guide-en-us.aspx)（Microsoft TechNet 網站）。
- [SQL Server 的功能限制 (Windows Azure SQL Database)](https://msdn.microsoft.com/library/windowsazure/ff394115.aspx) (MSDN)。
- [Windows Azure 資料表儲存體和 Windows Azure SQL Database-比較和對比](https://msdn.microsoft.com/library/jj553018.aspx)(MSDN)。 您將部署到 Windows Azure 的應用程式，Windows Azure 資料表儲存體可能是 Windows Azure SQL Database 的替代方案。 本主題將協助您決定這些替代方案。
- [Windows Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/ee336279) (MSDN)。
- [指導方針和限制 (Windows Azure SQL Database)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx)

<a id="nosql"></a>

## <a name="working-with-nosql-database-management-systems"></a>使用 NoSQL 資料庫管理系統

- [Windows Azure 資料服務](https://www.windowsazure.com/develop/net/data/)（Microsoft Azure 站台）。 請參閱[表格服務功能指南](https://docs.microsoft.com/azure/)並**巨量資料**頁面區段。
- [ASP.NET 多層式應用程式使用儲存體資料表、 佇列和 Blob](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) （Microsoft Azure 站台）。 使用 Windows Azure 儲存體 NoSQL 資料表的可下載範例應用程式的端對端教學課程。

<a id="linq"></a>

## <a name="using-linq-queries-in-aspnet-applications"></a>在 ASP.NET 應用程式中使用 LINQ 查詢

- [ASP.NET 資料存取選項](https://msdn.microsoft.com/library/ms178359.aspx#linq)(MSDN)。 包含 LINQ 的簡介。
- [LINQ 訓練影片](http://www.misfitgeek.com/windows-client-linq-training-videos-20/)（Joe Stagner 部落格）。
- [ASP.NET 論壇往來文章，動態 LINQ 資源的連結](https://forums.asp.net/p/1961037/5601994.aspx?Please+suggest+good+books+so+that+one+can+write+and+understand+dynamic+linq)。

<a id="dd"></a>

## <a name="using-dynamic-data-scaffolding"></a>使用動態資料 Scaffolding

- [動態資料專案範本](https://msdn.microsoft.com/library/jj822927.aspx#dynamicdata)(MSDN)。 關於何時應該使用動態資料專案的指引。
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
- [提升 ASP.NET 效能](https://msdn.microsoft.com/library/ff647787)(MSDN)。 沒有 「 已淘汰的內容 」 的警告，頂端的此頁面上，但大部分的資訊仍有相關且沒有任何可比較的更新的資源。
- [改善 SQL Server 效能](https://msdn.microsoft.com/library/ff647793)(MSDN)。 前一個連結為相同的註解。

另請參閱[最佳化 Entity Framework 效能](#optimizingef)稍早在本主題。

<a id="deploying"></a>

## <a name="deploying-a-database"></a>部署資料庫

- [ASP.NET Web 部署-建議資源](aspnet-web-deployment-content-map.md)(MSDN)。

<a id="webservice"></a>

## <a name="accessing-data-through-a-web-service"></a>透過 Web 服務存取資料

- [透過 Web 服務存取資料](https://msdn.microsoft.com/library/ms178359.aspx#webservice)(MSDN)。 關於何時應該使用 Web API 與 WCF 的指引。
- [開始使用 ASP.NET Web API](../web-api/index.md)。
- [WCF Data Services](https://msdn.microsoft.com/data/bb931106) (MSDN)。

<a id="additional"></a>

## <a name="additional-resources"></a>其他資源

- [ASP.NET 資料存取常見問題集](https://msdn.microsoft.com/library/jj653753.aspx)(MSDN)。
- [ASP.NET Web Form 教學課程-資料](../web-forms/overview/data-access/index.md)。 大部分這些教學課程是較舊的;請確定您閱讀[ASP.NET 資料存取選項](https://msdn.microsoft.com/library/ms178359.aspx)並[資料儲存體選項 （建置真實世界雲端應用程式與 Windows Azure）](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md)第一次，因此您不會太遠進入不正確的資料存取方法您的案例。
- [ASP.NET MVC 內容對應](../mvc/overview/getting-started/recommended-resources-for-mvc.md)。
- [ASP.NET Web Pages 教學課程-資料](../web-pages/overview/data/index.md)。
- [在 Visual Studio 中存取資料](https://msdn.microsoft.com/library/wzabh8c4.aspx)(MSDN)。 提供一份連結類似此內容的對應，但著重於 Visual Studio，而不是 ASP.NET。
