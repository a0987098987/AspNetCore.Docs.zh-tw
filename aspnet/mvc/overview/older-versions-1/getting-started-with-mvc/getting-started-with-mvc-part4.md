---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: 建立資料庫 |Microsoft Docs
author: shanselman
description: 這是初學者教學課程中，將會介紹 ASP.NET MVC 的基本概念。 建立簡單的 web 應用程式，從資料庫讀取與寫入。
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: 0e72359095e4c40ef7e56f1290a45ded257143c9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362352"
---
<a name="creating-a-database"></a>建立資料庫
====================
藉由[Scott Hanselman](https://github.com/shanselman)

> 這是初學者教學課程中，將會介紹 ASP.NET MVC 的基本概念。 您將建立簡單 web 應用程式，從資料庫讀取與寫入。 請瀏覽[ASP.NET MVC 學習中心](../../../index.md)來尋找其他 ASP.NET MVC 教學課程和範例。


我們要建立新的 SQL Express 資料庫，我們將使用它來儲存和擷取電影資料這一節。 從的 在 Visual Web Developer IDE 中，選取 檢視 |伺服器總管。 在資料連線上按一下滑鼠右鍵，然後按一下 [新增連接...]

![AddConnection](getting-started-with-mvc-part4/_static/image1.png)

在 選擇資料來源 對話方塊中，選取 Microsoft SQL Server，並選取 繼續。

![](getting-started-with-mvc-part4/_static/image2.png)

在 [新增連接] 對話方塊中，輸入 「。 \SQLEXPRESS 」 為您的伺服器名稱，並輸入 「 Movies 」 做為您的新資料庫的名稱。

[![[新增連接] 對話方塊](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)

按一下 [確定]，並將系統要求您想要建立該資料庫。 選取 [是]。

[![建立電影嗎？](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)

現在您已經在伺服器總管 中有空白的資料庫。

![加入新的資料表](getting-started-with-mvc-part4/_static/image7.png)

以滑鼠右鍵按一下資料表，然後按一下 加入資料表。 資料表設計工具將會出現。 加入資料行的識別碼、 標題、 ReleaseDate、 內容類型和價格。 以滑鼠右鍵按一下 識別碼 欄，並按一下 設定主索引鍵。 以下是看起來像什麼我設計區域。

[![資料庫資料表編輯器](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)

此外，選取 [識別碼] 資料行和下列資料行屬性底下將 「 身分識別規格 」 變更為 「 是 」。

[![IsIdentity-資料行屬性](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)

當您有完成工作時，按一下工具列中的 [儲存] 圖示，或選取檔案 |從功能表中，儲存並命名您的資料表 」**電影**」 （個別）。 我們有資料庫和資料表 ！

[![選擇名稱](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)

返回 伺服器總管 中以滑鼠右鍵按一下 影片 資料表中，然後選取 顯示資料表資料 」。 因此我們的資料庫有一些資料，請輸入幾個電影。

[![資料庫表格編輯](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)

## <a name="creating-a-model"></a>建立模型

現在，切換回在 IDE 的右手邊的 方案總管 和 Models 資料夾上按一下滑鼠右鍵並選取 新增 |新的項目。

[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)

我們建立我們新的資料庫中的實體模型。 這會將一組類別，加入我們的專案，可讓您更容易讓我們來查詢及管理資料庫中的資料。 選取對話方塊中，左側的 資料 節點，然後選取 ADO.NET 實體資料模型項目範本。 命名 Movies.edmx。

[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)

按一下 [新增] 按鈕。 然後，這會啟動 「 實體資料模型精靈 」。

在新快顯對話方塊中，選取 從資料庫產生。 我們剛了資料庫，因為我們只需要告訴我們新的資料庫和其資料表中的 Entity Framework。 按一下 下一步儲存我們在我們的 web 應用程式組態中的資料庫連接。 現在，請檢查資料表與電影核取方塊，然後按一下 [完成]。

[![實體資料模型精靈](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)

現在我們可以看到我們在 Entity Framework Designer 中的新電影資料表，並從程式碼中存取。

[![影片-Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)

在設計介面上，您可以看到 「 電影 」 類別。 此類別會對應到在資料庫中之 「 電影 」 資料表，並在其中每個屬性會對應至資料表的資料行。 「 電影 」 類別的每個執行個體將會對應到在 「 電影 」 資料表中的資料列。

如果您不喜歡預設命名和對應 Entity Framework 所使用的慣例，您可以使用 Entity Framework 設計工具變更或加以自訂。 我們將此應用程式使用預設值，並只將檔案儲存為集是。

現在，我們將使用一些實際資料 ！

> [!div class="step-by-step"]
> [上一頁](getting-started-with-mvc-part3.md)
> [下一頁](getting-started-with-mvc-part5.md)
