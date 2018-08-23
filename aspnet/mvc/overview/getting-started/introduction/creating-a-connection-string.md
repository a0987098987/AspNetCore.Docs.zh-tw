---
uid: mvc/overview/getting-started/introduction/creating-a-connection-string
title: 建立連接字串和使用 SQL Server LocalDB |Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 6127804d-c1a9-414d-8429-7f3dd0f56e97
msc.legacyurl: /mvc/overview/getting-started/introduction/creating-a-connection-string
msc.type: authoredcontent
ms.openlocfilehash: 73a3149c8c789221baa2e3804ebf15ed5a509ddf
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825773"
---
<a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>建立連接字串和使用 SQL Server LocalDB
====================
藉由[Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>建立連接字串和使用 SQL Server LocalDB

`MovieDBContext`您所建立的類別會處理連接到資料庫和對應的工作`Movie`資料庫記錄的物件。 您可能會問的一個問題，是如何指定要連線到哪一個資料庫。 您實際上沒有指定要使用哪一個資料庫，Entity Framework 會使用預設[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb)。 在這一節我們將明確新增中的連接字串*Web.config*應用程式檔案。

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb)是輕量版的 SQL Server Express Database Engine 會視需要啟動，並以使用者模式執行。 LocalDB 以特殊的執行模式執行的 SQL Server Express，可讓您使用資料庫作為 *.mdf*檔案。 一般而言，LocalDB 資料庫檔案會保留在*應用程式\_資料*web 專案的資料夾。

不建議在生產環境 web 應用程式中使用 SQL Server Express。 LocalDB 尤其不應用於生產環境的 web 應用程式因為它不是在搭配 IIS 運作。 不過，LocalDB 資料庫可以輕鬆地移轉至 SQL Server 或 SQL Azure。

在 Visual Studio 2017 中，使用 Visual Studio 的預設會安裝 LocalDB。

根據預設，Entity Framework 會尋找名為物件內容類別相同的連接字串 (`MovieDBContext`這個專案)。 如需詳細資訊，請參閱[ASP.NET Web 應用程式的 SQL Server 連接字串](https://msdn.microsoft.com/library/jj653752.aspx)。

開啟應用程式根目錄*Web.config*檔案如下所示。 (沒有*Web.config*中的檔案*檢視*資料夾。)

![](creating-a-connection-string/_static/image1.png)

尋找`<connectionStrings>`項目：

![](creating-a-connection-string/_static/image2.png)

加入下列連接字串`<connectionStrings>`中的項目*Web.config*檔案。

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

下列範例示範的一部分*Web.config*檔案以加入新的連接字串：

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

兩個連接字串都很相似。 第一個連接字串名為`DefaultConnection`並用它來控制可以存取應用程式的成員資格資料庫。 您已新增連接字串會指定名為 LocalDB 資料庫*Movie.mdf*位於*應用程式\_資料*資料夾。 我們不會使用成員資格資料庫中，請在本教學課程，如需成員資格、 驗證和安全性的詳細資訊，請參閱我的教學課程[使用驗證和 SQL DB 建立 ASP.NET MVC 應用程式並部署至 Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)。

連接字串的名稱必須符合的名稱[DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)類別。

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

您實際上不需要新增`MovieDBContext`連接字串。 如果您未指定連接字串，Entity Framework 將 LocalDB 資料庫的目錄中建立使用者的完整名稱[DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)類別 (在此情況下`MvcMovie.Models.MovieDBContext`)。 您可以為資料庫命名任何想要的話，只要它擁有 *。MDF*後置詞。 比方說，我們無法為資料庫命名*MyFilms.mdf*。

接下來，您將建立新`MoviesController`類別可用來顯示電影資料，並允許使用者建立新的電影清單。

> [!div class="step-by-step"]
> [上一頁](adding-a-model.md)
> [下一頁](accessing-your-models-data-from-a-controller.md)
