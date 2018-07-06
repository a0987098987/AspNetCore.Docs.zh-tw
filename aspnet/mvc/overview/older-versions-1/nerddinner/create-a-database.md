---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: 建立資料庫 |Microsoft Docs
author: microsoft
description: 步驟 2 顯示建立資料庫的保留所有 dinner 和回覆我們 NerdDinner 應用程式資料的步驟。
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: 2a2ed8c91aa3ec0da63cd8e8686ff601f6e66374
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818962"
---
<a name="create-a-database"></a>建立資料庫
====================
by [Microsoft](https://github.com/microsoft)

[下載 PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 這是一套免費的步驟 2 ["NerdDinner 」 應用程式教學課程](introducing-the-nerddinner-tutorial.md)，逐步解說如何建置一個小型的但在完成時，使用 ASP.NET MVC 1 的 web 應用程式。
> 
> 步驟 2 顯示建立資料庫的保留所有 dinner 和回覆我們 NerdDinner 應用程式資料的步驟。
> 
> 如果您使用 ASP.NET MVC 3，我們建議您遵循[取得開始使用 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或是[MVC Music 市集](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教學課程。


## <a name="nerddinner-step-2-creating-the-database"></a>NerdDinner 步驟 2： 建立資料庫

我們將使用資料庫來儲存所有 Dinner 和 RSVP 資料 NerdDinner 應用程式。

下列步驟會示範如何建立使用免費的 SQL Server Express edition 資料庫 (您可以輕鬆地安裝使用的 V2 [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx))。 我們將撰寫的程式碼的所有適用於 SQL Server Express 和 SQL Server 完整版。

### <a name="creating-a-new-sql-server-express-database"></a>建立新的 SQL Server Express 資料庫

我們會開始透過我們的 web 專案上按一下滑鼠右鍵，然後選取**Add-&gt;新的項目**功能表命令：

![](create-a-database/_static/image1.png)

這會顯示 Visual Studio 的 「 新增新的項目 對話方塊。 我們會依據 「 資料 」 類別目錄篩選，並選取 「 SQL Server 資料庫 」 項目範本：

![](create-a-database/_static/image2.png)

我們將會命名為我們想要建立 「 NerdDinner.mdf"，然後按 [確定] 在 SQL Server Express 的資料庫。 Visual Studio 會接著詢問我們是否我們想要將這個檔案加入我們 \App\_資料目錄 （即已設定與讀取和寫入安全性 Acl）：

![](create-a-database/_static/image3.png)

我們按一下 是，和我們新的資料庫會建立並加入我們的方案總管 中：

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a>建立資料庫中的資料表

我們現在有新的空白資料庫。 讓我們新增一些資料表。

若要這樣做，我們會瀏覽可讓我們來管理資料庫和伺服器中的 Visual Studio 中 [伺服器總管] 索引標籤視窗。 SQL Server Express 資料庫儲存在 \App\_我們的應用程式資料資料夾會自動顯示在 [伺服器總管] 中。 選擇性地，我們可以使用 [伺服器總管] 視窗頂端的 [連接到資料庫] 圖示將其他的 SQL Server 資料庫 （本機和遠端） 新增至清單以及：

![](create-a-database/_static/image5.png)

NerdDinner 資料庫 – 一個用來儲存我們 Dinners，和其他追蹤 RSVP 接受它們，我們會新增兩個資料表。 我們可以建立新的資料表，在資料庫裡面的 「 資料表 」 資料夾上按一下滑鼠右鍵，然後選擇 「 加入新的資料表 」 功能表命令：

![](create-a-database/_static/image6.png)

這會開啟可讓我們設定的資料表結構描述的資料表設計工具。 我們 「 Dinners 」 資料表中，我們會將加入 10 個資料行的資料：

![](create-a-database/_static/image7.png)

我們想要 「 DinnerID 」 資料行資料表唯一主索引鍵。 我們可以將它設定"DinnerID 」 資料行上按一下滑鼠右鍵，然後選擇 [設定主要金鑰] 功能表項目：

![](create-a-database/_static/image8.png)

除了更 DinnerID 主索引鍵，我們也想要將它設定為新的資料列加入至資料表，其值會自動遞增的 「 身分識別 」 資料行 （亦即第一個插入的 Dinner 資料列會有 DinnerID 為 1，第二個插入資料列將具有 DinnerID 2 等等)。

我們可以這麼做的選取"DinnerID 」 資料行，並再使用 [資料行屬性] 編輯器來設定 「 （」 身分識別） 的屬性為"Yes"的資料行上。 我們將使用標準的識別預設值 （從 1 開始，並遞增每個新的 Dinner 資料列上的 1）：

![](create-a-database/_static/image9.png)

按 CTRL-S 或使用，我們會接著儲存資料表**檔案&gt;儲存**功能表命令。 這會提示我們命名的資料表。 我們將它 「 Dinners"命名：

![](create-a-database/_static/image10.png)

我們新的 Dinners 資料表會再顯示在伺服器總管 中，在資料庫裡面。

然後，我們會重複上述步驟，並建立 「 RSVP 」 資料表。 此表格中的有 3 個資料行。 我們將設定 RsvpID 做的資料行主要金鑰，並也讓身分識別資料行：

![](create-a-database/_static/image11.png)

我們會將它儲存，並提供名稱 「 RSVP"。

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a>設定資料表之間的外部索引鍵關聯性

我們現在會在資料庫裡面有兩個資料表。 我們最後一個結構描述設計步驟會設定 「 一對多 」 之間的關聯性這兩個資料表 –，以便我們可以對其套用的零或多個 RSVP 資料列與相關聯的每一列 Dinner。 我們會設定"Dinners 」 資料表中有"DinnerID 」 資料行的外部索引鍵關聯性的 RSVP 資料表的"DinnerID 」 資料行來執行這項操作。

若要這樣做我們會藉由在 [伺服器總管] 中按兩下開啟資料表設計工具內的 RSVP 資料表。 我們會選取 「 DinnerID 」 內的資料行，以滑鼠右鍵按一下，然後選擇 [Relationshps] 內容功能表命令：

![](create-a-database/_static/image12.png)

這會顯示一個對話方塊，可用來在資料表之間的關聯性設定：

![](create-a-database/_static/image13.png)

我們會按一下 [新增] 按鈕，將新的關聯性新增至對話方塊。 一旦新增關聯性之後，我們將右邊的對話方塊中，展開 「 資料表和資料行規格 」 樹狀檢視節點，在屬性方格中的，然後按一下 ... 按鈕右邊：

![](create-a-database/_static/image14.png)

按一下 ... 按鈕會顯示另一個對話方塊，讓我們指定的資料表和資料行參與關聯性，以及讓我們命名關聯性。

我們會變更主索引鍵資料表要 「 Dinners"，並選取 Dinners 資料表中的"DinnerID 」 資料行的主索引鍵。 我們的 RSVP 資料表是外部索引鍵資料表和 RSVP。DinnerID 資料行將會是做為外部索引鍵相關聯的：

![](create-a-database/_static/image15.png)

現在 RSVP 資料表中的每個資料列會與 Dinner 資料表中的資料列相關聯。 SQL Server 會為我們 – 維護參考完整性，並讓我們無法新增新的 RSVP 資料列，如果它不是指向有效的 Dinner 資料列。 它也會防止我們刪除 Dinner 資料列，如果有仍然 RSVP 參考它的資料列。

### <a name="adding-data-to-our-tables"></a>將資料加入至資料表

讓我們先完成我們 Dinners 資料表中加入一些範例資料。 我們可以用滑鼠右鍵按一下它在 [伺服器總管] 中，然後選擇 [顯示資料表資料] 命令，將資料新增至資料表：

![](create-a-database/_static/image16.png)

我們將新增幾個資料列的 Dinner 稍後當我們開始實作應用程式，我們可以使用：

![](create-a-database/_static/image17.png)

### <a name="next-step"></a>下一個步驟

我們已經完成建立資料庫。 我們現在來建立模型類別，我們可以用來查詢和更新它。

> [!div class="step-by-step"]
> [上一頁](create-a-new-aspnet-mvc-project.md)
> [下一頁](build-a-model-with-business-rule-validations.md)
