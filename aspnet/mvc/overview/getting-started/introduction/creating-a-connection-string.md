---
uid: mvc/overview/getting-started/introduction/creating-a-connection-string
title: "建立的連接字串和使用 SQL Server LocalDB |Microsoft 文件"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 6127804d-c1a9-414d-8429-7f3dd0f56e97
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/creating-a-connection-string
msc.type: authoredcontent
ms.openlocfilehash: 41f1f30d86406580ab9fc7278a94d9c291913f9a
ms.sourcegitcommit: d1d8071d4093bf2444b5ae19d6e45c3d187e338b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/19/2017
---
<a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>建立的連接字串和使用 SQL Server LocalDB
====================
由[Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>建立的連接字串和使用 SQL Server LocalDB

`MovieDBContext`您所建立的類別會處理連接到資料庫和對應的工作`Movie`資料庫記錄的物件。 您可以詢問一個問題，是如何指定將會連線到哪一個資料庫。 您實際上沒有指定要使用哪一個資料庫，Entity Framework 將會預設為使用[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb)。 本節中我們會明確地將新增連接字串中的*Web.config*應用程式檔案。

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb)是輕量版 SQL Server Express Database Engine，視需要啟動並以使用者模式執行。 LocalDB 以特殊的執行模式執行的 SQL Server Express，可讓您能夠使用資料庫，做為*.mdf*檔案。 一般而言，LocalDB 資料庫檔案會保留在*應用程式\_資料*web 專案的資料夾。

SQL Server Express 不建議用於生產環境 web 應用程式。 LocalDB 特別不應用於生產環境與 web 應用程式因為它不是使用 IIS。 不過，您可以輕鬆地到 SQL Server 或 SQL Azure 移轉 LocalDB 資料庫。

在 Visual Studio 2017，LocalDB 被安裝預設會隨 Visual Studio。

根據預設，Entity Framework 會尋找名為與物件內容類別相同的連接字串 (`MovieDBContext`這個專案)。 如需詳細資訊，請參閱[ASP.NET Web 應用程式的 SQL Server 連接字串](https://msdn.microsoft.com/en-us/library/jj653752.aspx)。

開啟應用程式根目錄*Web.config*檔案如下所示。 (不*Web.config*檔案*檢視*資料夾。)

![](creating-a-connection-string/_static/image1.png)

尋找`<connectionStrings>`項目：

![](creating-a-connection-string/_static/image2.png)

加入下列連接字串至`<connectionStrings>`中的項目*Web.config*檔案。

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

下列範例顯示的某一部分*Web.config*檔案與新加入的連接字串：

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

兩個連接字串都很相似。 第一個連接字串名為`DefaultConnection`並用它來控制可以存取應用程式的成員資格資料庫。 已新增的連接字串會指定名為 LocalDB 資料庫*Movie.mdf*位於*應用程式\_資料*資料夾。 我們將不會使用成員資格資料庫，在本教學課程，如需成員資格、 驗證和安全性詳細資訊，請參閱我教學課程[驗證與 SQL 資料庫中建立 ASP.NET MVC 應用程式並部署至 Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)。

連接字串的名稱必須符合的名稱[DbContext](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext(v=vs.103).aspx)類別。

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

您不需要新增`MovieDBContext`連接字串。 如果您未指定連接字串，Entity Framework 的完整限定名稱的使用者目錄中會建立 LocalDB 資料庫[DbContext](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext(v=vs.103).aspx)類別 (在此情況下`MvcMovie.Models.MovieDBContext`)。 您可以為資料庫命名任何想要的話，因為它具有*。MDF*後置詞。 例如，我們無法為資料庫命名*MyFilms.mdf*。

接下來，您將建置新`MoviesController`類別可用來顯示電影，並允許使用者建立新的電影清單。

>[!div class="step-by-step"]
[上一頁](adding-a-model.md)
[下一頁](accessing-your-models-data-from-a-controller.md)
