---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: "使用 Code First 移轉來植入資料庫 |Microsoft 文件"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: df75a69644033cc76fee86b5a9692ab65beb4d01
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="use-code-first-migrations-to-seed-the-database"></a>使用 Code First 移轉來植入資料庫
====================
由[Mike Wasson](https://github.com/MikeWasson)

[下載完成的專案](https://github.com/MikeWasson/BookService)

在本節中，您將使用[Code First 移轉](https://msdn.microsoft.com/en-us/data/jj591621)EF 來植入的測試資料的資料庫中。

從**工具**功能表上，選取**程式庫套件管理員**，然後選取**Package Manager Console**。 在 [封裝管理員主控台] 視窗中，輸入下列命令：

[!code-console[Main](part-3/samples/sample1.cmd)]

此命令會加入至專案，名為移轉的資料夾，再加上名為 configuration.cs 中移轉資料夾中的程式碼檔案。

![](part-3/_static/image1.png)

開啟 configuration.cs 中的檔案。 加入下列**使用**陳述式。

[!code-csharp[Main](part-3/samples/sample2.cs)]

然後加入下列程式碼加入**Configuration.Seed**方法：

[!code-csharp[Main](part-3/samples/sample3.cs)]

在 [封裝管理員主控台] 視窗中，輸入下列命令：

[!code-console[Main](part-3/samples/sample4.cmd)]

第一個命令會產生程式碼會建立資料庫，並且第二個命令執行該程式碼。 資料庫使用本機上，建立[LocalDB](https://msdn.microsoft.com/en-us/library/hh510202.aspx)。

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a>瀏覽應用程式開發介面 （選擇性）

按 F5 以偵錯模式執行應用程式。 Visual Studio 會啟動 IIS Express，並執行您 web 應用程式。 Visual Studio 會啟動瀏覽器，然後開啟應用程式的首頁。

Visual Studio 執行時 web 專案，它會指派通訊埠編號。 在下面的影像中的連接埠號碼是 50524。 當您執行應用程式時，您會看到不同的通訊埠編號。

![](part-3/_static/image3.png)

[首頁] 是使用 ASP.NET MVC 所實作的。 在頁面頂端，會顯示 「 應用程式開發介面 」 的連結。 此連結讓您自動產生的說明頁面 web API。 (若要了解如何產生這個說明網頁，以及如何將您自己的文件加入至頁面，請參閱[建立 ASP.NET Web API 的說明 頁面](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md)。)若要查看有關應用程式開發介面，包括要求和回應格式的詳細資料頁面連結，您可以按一下 [說明] 中。

![](part-3/_static/image4.png)

此 API 讓資料庫上的 CRUD 作業。 以下摘述應用程式開發介面。

| 作者 |  |
| --- | -- |
| 取得應用程式開發介面/作者 | 取得所有作者。 |
| 取得應用程式開發介面/作者 / {id} | 取得作者的 id。 |
| POST/api/作者 | 建立新的作者。 |
| PUT /api/作者 / {id} | 更新現有作者。 |
| 刪除 /api/作者 / {id} | 刪除作者。 |

| 書籍 |  |
| --- | -- |
| 取得 /api/books | 取得所有書籍。 |
| 取得 /api/書籍 / {id} | 取得活頁簿的識別碼。 |
| 張貼/api/書籍 | 建立新的書籍。 |
| PUT /api/書籍 / {id} | 更新現有的書籍。 |
| 刪除 /api/書籍 / {id} | 刪除活頁簿。 |

## <a name="view-the-database-optional"></a>檢視資料庫 （選擇性）

當您執行 Update-database 命令時，在 EF 建立資料庫和呼叫`Seed`方法。 當您在本機執行應用程式時，會使用 EF [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx)。 您可以在 Visual Studio 中檢視的資料庫。 從**檢視**功能表上，選取**SQL Server 物件總管**。

![](part-3/_static/image5.png)

在**連接到伺服器**對話方塊，請在**伺服器名稱**編輯方塊中，輸入"(localdb) \v11.0"。 保留**驗證**選項設定為 「 Windows 驗證 」。 按一下 **[Connect]**(連線)。

![](part-3/_static/image6.png)

Visual Studio 會連接到 LocalDB 和 SQL Server 物件總管 視窗中顯示現有的資料庫。 您可以展開節點，以查看 EF 建立的資料表。

![](part-3/_static/image7.png)

若要檢視資料，以滑鼠右鍵按一下資料表，然後選取**檢視資料**。

![](part-3/_static/image8.png)

下列螢幕擷取畫面顯示資料表的結果。 請注意，EF 填入種子資料，與資料庫資料表包含 Authors 資料表的外部索引鍵。

![](part-3/_static/image9.png)

>[!div class="step-by-step"]
[上一頁](part-2.md)
[下一頁](part-4.md)
