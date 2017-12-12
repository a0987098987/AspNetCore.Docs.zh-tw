---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: "建立資料庫 |Microsoft 文件"
author: microsoft
description: "步驟 2 顯示建立資料庫保存所有 dinner 和 RSVP NerdDinner 應用程式資料的步驟。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: 7635722fc357356edd06fb4cff301a8c4dfebbef
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="create-a-database"></a>建立資料庫
====================
由[Microsoft](https://github.com/microsoft)

[下載 PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 這是一套免費的步驟 2 ["NerdDinner 」 應用程式教學課程](introducing-the-nerddinner-tutorial.md)，會逐步引導式如何建置小，但是完成，web 應用程式使用 ASP.NET MVC 1。
> 
> 步驟 2 顯示建立資料庫保存所有 dinner 和 RSVP NerdDinner 應用程式資料的步驟。
> 
> 如果您使用 ASP.NET MVC 3，我們建議您遵循[取得啟動與 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教學課程。


## <a name="nerddinner-step-2-creating-the-database"></a>NerdDinner 步驟 2： 建立資料庫

我們會使用資料庫來儲存所有 Dinner 和 RSVP 資料 NerdDinner 應用程式。

下列步驟顯示建立使用免費的 SQL Server Express 版本的資料庫 (您可以輕鬆地安裝使用的 V2 [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx))。 我們會撰寫的程式碼的所有適用於 SQL Server Express 和 SQL Server 完整版。

### <a name="creating-a-new-sql-server-express-database"></a>建立新的 SQL Server Express 資料庫

我們會以滑鼠右鍵按一下 web 專案中，開始，然後選取**新增-&gt;新項目**功能表命令：

![](create-a-database/_static/image1.png)

這會顯示 Visual Studio 的 加入新項目 」 對話方塊。 我們會依 「 資料 」 類別目錄篩選，然後選取 「 SQL Server 資料庫 」 項目範本：

![](create-a-database/_static/image2.png)

我們會命名 SQL Server Express 我們想要的資料庫建立 「 NerdDinner.mdf"，然後按 [確定]。 Visual Studio 將會詢問我們是否我們想要將這個檔案加入我們 \App\_資料目錄 （這是一個目錄已經安裝程式具有兩個讀取和寫入安全性 Acl）：

![](create-a-database/_static/image3.png)

我們會按一下 [是]，並會建立並加入 [方案總管] 中我們我們新的資料庫：

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a>建立資料庫中的資料表

我們現在有新的空白資料庫。 讓我們加入一些資料表給它。

若要這樣做，我們會瀏覽 「 伺服器總管 」 索引標籤視窗可讓我們來管理資料庫和伺服器中的 Visual Studio 中。 SQL Server Express 資料庫儲存在 \App\_我們的應用程式資料資料夾會自動顯示在 [伺服器總管] 中。 我們可以選擇性地使用"連接到資料庫 」 圖示 「 伺服器總管 」 視窗的頂端加入清單以及其他 SQL Server 資料庫 （本機和遠端）：

![](create-a-database/_static/image5.png)

我們會將兩個資料表加入我們 NerdDinner 資料庫 – 一個用來儲存我們 Dinners，和其他 RSVP acceptances 追蹤它們。 我們可以建立新的資料表，我們資料庫內的 「 資料表 」 資料夾上按一下滑鼠右鍵，然後選擇 加入新的資料表 」 功能表命令：

![](create-a-database/_static/image6.png)

這將會開啟資料表設計工具，可讓我們設定的資料表結構描述。 我們"Dinners 」 資料表，我們將會加入 10 個資料行的資料：

![](create-a-database/_static/image7.png)

我們想要資料表唯一主索引鍵"DinnerID"資料行。 我們可以將它設定"DinnerID 」 資料行上按一下滑鼠右鍵，然後選擇 設定主索引鍵 」 功能表項目：

![](create-a-database/_static/image8.png)

除了讓 DinnerID 主索引鍵，我們也想將它設定為新的資料列加入至資料表時，其值會自動遞增的 「 識別 」 資料行 （這表示第一個插入的 Dinner 資料列會有 DinnerID 為 1，第二個插入資料列將具有 DinnerID 為 2，等等)。

我們可以選取 「 DinnerID 」 資料行，然後使用 [資料行屬性] 編輯器來設定 「 （」 身分識別） 的屬性為"Yes"的資料行上。 我們將使用標準的識別預設值 （從 1 開始，並遞增 1 上每個新的 Dinner 資料列）：

![](create-a-database/_static/image9.png)

藉由輸入 CTRL-S 或使用，我們會再儲存資料表**檔案-&gt;儲存**功能表命令。 這會提示我們命名資料表。 我們將要使用的名稱"Dinners 」:

![](create-a-database/_static/image10.png)

我們新增 Dinners 資料表會再顯示我們在 [伺服器總管] 中的資料庫內。

然後，我們會重複上述步驟，建立 「 RSVP 」 資料表。 此表格中的有 3 個資料行。 即將安裝 RsvpID 做的資料行主要金鑰，及也容易識別資料行：

![](create-a-database/_static/image11.png)

我們會將它儲存，並將其命名為"RSVP"。

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a>設定資料表之間的外部索引鍵關聯性

我們現在有兩個資料表，我們的資料庫內。 我們最後一個結構描述設計步驟會設定兩個資料表 – 之間的 「 一對多 」 關聯性，使我們可以將每個 Dinner 資料列與零或多個套用至該 RSVP 資料列。 藉由設定 RSVP 資料表的"DinnerID 」 資料行 」 Dinners 」 資料表中有"DinnerID 」 資料行的外部索引鍵關聯性，我們將會執行這項操作。

若要這樣做我們將 RSVP 資料表，資料表設計工具中按兩下 [伺服器總管] 中開啟。 我們會選取 「 DinnerID 」 內的資料行，按一下滑鼠右鍵，然後選擇 [Relationshps …] 內容功能表命令：

![](create-a-database/_static/image12.png)

這會顯示一個對話方塊，我們可以用來設定資料表之間的關聯性：

![](create-a-database/_static/image13.png)

我們會按一下 [新增] 按鈕，加入新關聯性對話方塊。 一旦加入關聯性，我們將右邊的對話方塊中，展開在屬性方格內的 「 資料表和資料行規格 」 樹狀檢視節點，然後按一下 "..."按鈕右邊：

![](create-a-database/_static/image14.png)

按一下"..."按鈕會顯示另一個對話方塊，讓我們指定哪些資料表和資料行參與關聯性，以及讓我們命名關聯性。

我們將變更主索引鍵資料表為"Dinners 」，並選取 Dinners 資料表內的"DinnerID 」 資料行的主索引鍵。 外部索引鍵資料表中，且 RSVP，將 RSVP 的資料表。DinnerID 資料行要做為外部索引鍵相關聯：

![](create-a-database/_static/image15.png)

現在 RSVP 資料表中的每個資料列會與 Dinner 資料表中的資料列相關聯。 SQL Server 會為我們 – 維護參考完整性，並導致我們無法加入新的 RSVP 資料列，如果它不是指向有效的 Dinner 資料列。 它也會防止我們刪除 Dinner 資料列，如果沒有仍 RSVP 參照它的資料列。

### <a name="adding-data-to-our-tables"></a>將資料加入至資料表

讓我們先完成一些範例資料加入我們 Dinners 資料表。 我們可以將資料加入資料表，以滑鼠右鍵按一下它在伺服器總管，然後選擇 顯示資料表資料 命令：

![](create-a-database/_static/image16.png)

我們會將新增 Dinner 資料，我們可以在稍後當我們開始實作應用程式使用的少數資料的列：

![](create-a-database/_static/image17.png)

### <a name="next-step"></a>下一個步驟

我們已完成建立資料庫。 我們現在來建立模型類別，我們可以用來查詢和更新它。

>[!div class="step-by-step"]
[上一頁](create-a-new-aspnet-mvc-project.md)
[下一頁](build-a-model-with-business-rule-validations.md)
