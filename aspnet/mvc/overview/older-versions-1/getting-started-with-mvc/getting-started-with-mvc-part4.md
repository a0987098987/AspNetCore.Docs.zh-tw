---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: 建立資料庫 |Microsoft 文件
author: shanselman
description: 這是初學者教學課程介紹基本概念的 ASP.NET MVC。 建立簡單的 web 應用程式可讀取和寫入資料庫中。
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: ff2a41803cd31ce50bbf79e630d827b6de441ba3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868070"
---
<a name="creating-a-database"></a>建立資料庫
====================
由[Scott Hanselman](https://github.com/shanselman)

> 這是初學者教學課程介紹基本概念的 ASP.NET MVC。 您將建立簡單的 web 應用程式可讀取和寫入資料庫中。 請瀏覽[ASP.NET MVC 學習中心](../../../index.md)教學課程和範例，請尋找其他 ASP.NET MVC。


本節中，我們即將建立新的 SQL Express 資料庫，我們將使用來儲存和擷取我們電影資料。 從選取 在 Visual Web Developer ide 的 檢視 |伺服器總管。 以滑鼠右鍵按一下資料連接，並按一下 新增連接...

![AddConnection](getting-started-with-mvc-part4/_static/image1.png)

在 選擇資料來源 對話方塊中，選取 Microsoft SQL Server，並選取 繼續。

![](getting-started-with-mvc-part4/_static/image2.png)

在 [加入連接] 對話方塊中，輸入 「。 \SQLEXPRESS 」 為您的伺服器名稱，並輸入"影片"做為您的新資料庫的名稱。

[![新增連接對話方塊](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)

按一下 [確定]，您將需要在想要建立該資料庫。 選取 [是]。

[![建立電影嗎？](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)

現在您已經在伺服器總管 中有空白資料庫。

![加入新的資料表](getting-started-with-mvc-part4/_static/image7.png)

以滑鼠右鍵按一下資料表，然後按一下 加入資料表。 資料表設計工具將會出現。 加入資料行的識別碼、 標題、 ReleaseDate、 類型和價格。 識別碼資料行上按一下滑鼠右鍵，按一下 設定主索引鍵。 以下是我外觀的設計區域。

[![資料庫資料表編輯器](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)

此外，選取識別碼資料行，並在下方的資料行屬性將 「 身分識別規格 」 變更為"Yes"。

[![IsIdentity-資料行屬性](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)

當您選擇了它完成時，請按一下工具列中的 [儲存] 圖示，或選取檔案 |從功能表中，儲存並命名您的資料表 」**影片**」 （單數）。 我們有資料庫和資料表 ！

[![選擇名稱](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)

返回 [伺服器總管] 並以滑鼠右鍵按一下 [影片] 資料表，然後選取 [顯示資料表資料]。 因此我們的資料庫具有一些資料，請輸入一些影片。

[![資料庫表格編輯](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)

## <a name="creating-a-model"></a>建立模型

現在，切換回上 IDE 的右手邊的 方案總管 和 模型 資料夾上按一下滑鼠右鍵，然後選取 加入 |新項目。

[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)

我們將我們新的資料庫中建立實體模型。 這會將一組類別，加入受測專案，以簡化我們來查詢與管理資料庫內的資料。 選取對話方塊左側的資料節點，然後選取 ADO.NET 實體資料模型項目範本。 命名 Movies.edmx。

[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)

按一下 [新增] 按鈕。 然後，這會啟動 「 實體資料模型精靈 」。

在新的快顯對話方塊中，選取 從資料庫產生。 因為我們剛才資料庫，我們只需要告訴我們新的資料庫和其資料表的 Entity Framework。 按一下 旁邊，儲存在我們的 web 應用程式設定資料庫連線。 現在，請檢查資料表及電影核取方塊，然後按一下 [完成]。

[![實體資料模型精靈](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)

現在我們可以看到我們新的電影資料表在 Entity Framework Designer 中，並從程式碼存取它。

[![影片-Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)

在設計介面上，您可以看到 「 電影 」 類別。 這個類別會對應到 「 電影 」 資料表在資料庫中，並在其中每個屬性會對應至資料表的資料行。 「 電影"類別的每個執行個體將會對應到在 「 電影 」 資料表中的資料列。

如果您不喜歡的預設命名和對應 Entity Framework 所使用的慣例，您可以使用 Entity Framework 設計工具來變更或自訂這兩者。 此應用程式，我們將使用預設值，只將檔案儲存為-是。

現在，讓我們來處理一些實際資料 ！

> [!div class="step-by-step"]
> [上一頁](getting-started-with-mvc-part3.md)
> [下一頁](getting-started-with-mvc-part5.md)
