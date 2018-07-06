---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: 加入至模型的資料行 |Microsoft Docs
author: shanselman
description: 這是初學者教學課程中，將會介紹 ASP.NET MVC 的基本概念。 建立簡單的 web 應用程式，從資料庫讀取與寫入。
ms.author: aspnetcontent
ms.date: 08/14/2010
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 5dbda1280c073d5d4f2d71918ca31bc44475f64d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817197"
---
<a name="adding-a-column-to-the-model"></a>將資料行加入至模型
====================
藉由[Scott Hanselman](https://github.com/shanselman)

> 這是初學者教學課程中，將會介紹 ASP.NET MVC 的基本概念。 您將建立簡單 web 應用程式，從資料庫讀取與寫入。 請瀏覽[ASP.NET MVC 學習中心](../../../index.md)來尋找其他 ASP.NET MVC 教學課程和範例。


這一節我們要逐步解說如何對資料庫中的結構描述中的變更並在我們的應用程式內處理變更。

讓我們將 「 評等 」 的資料行新增至電影資料表中。 返回 IDE，然後按一下 [資料庫總管] 中。 以滑鼠右鍵按一下之電影資料表，然後選取 開啟資料表定義。

將 「 評等 」 資料行，如下所示。 我們現在還沒有任何評等，因為資料行可允許 null。 按一下 [儲存]。

[![編輯電影資料表](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)

接下來，返回 [方案總管]，然後開啟 Movies.edmx 檔案 （這是 \Models 資料夾中）。 以滑鼠右鍵按一下設計介面 （白色區域），並選取 從資料庫更新模型。

[![影片-Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)

這會啟動 「 更新精靈 」。 按一下 [重新整理] 索引標籤中的，按一下 [完成]。 我們的電影模型類別就會更新與新的資料行。

![更新精靈 (2)](getting-started-with-mvc-part8/_static/image5.png)

之後按一下 [完成]，您可以看到新的評等資料行已新增至電影實體，在我們的模型。

[![電影實體](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)

我們已新增資料行在資料庫模型中，但檢視不知道。

## <a name="update-views-with-model-changes"></a>更新檢視的模型變更

有幾種方式，我們無法更新我們的檢視範本，以反映新的評等資料行。 我們產生它們透過 [新增檢視] 對話方塊來建立這些檢視，所以我們無法刪除它們，然後再重新建立它們。 不過，通常人將已經做了修改其檢視的範本，從初始的 scaffold 層代而且會想要加入或刪除欄位，以手動方式，就如同我們使用 [識別碼] 欄位建立。

開啟 \Views\Movies\Index.aspx 範本，並新增&lt;th&gt;評等&lt;/td>< /&gt;至之電影資料表標頭。 我加入了我的內容類型之後。 然後，在相同的資料行位置，但下方，新增一行輸出我們新評比。

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

我們最終的 Index.aspx 範本看起來像這樣：

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

讓我們再開啟 \Views\Movies\Create.aspx 範本，並新增標籤和文字方塊中，我們新的評等屬性：

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

我們最終 Create.aspx 範本會看起來像這樣，並讓我們變更我們瀏覽器的標題和次要&lt;h2&gt;標題為類似"建立電影 」 我們在這裡 ！

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

執行您的應用程式，現在您已經有新資料庫中的欄位已加入至 [建立] 頁面。 新增新的電影-評分-這次，然後按一下 建立。

[![建立電影-Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)

按一下 [建立] 之後，您要傳送至 [索引] 頁面在您新增電影會列出新的評等資料行在資料庫中

[![電影清單-Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)

此基本教學課程中有您開始進行控制站，將它們關聯的檢視，並傳遞硬式編碼的資料。 然後我們建立及設計資料庫，並放入一些資料中。 我們會從資料庫擷取資料，並顯示 HTML 表格中。 然後，我們會新增可讓使用者將資料加入至資料庫本身從 Web 應用程式內建立表單。 我們新增驗證，然後進行用戶端使用 JavaScript 的驗證。 最後，我們變更了資料庫以包含新的資料行的資料，然後更新我們的兩個頁面，來建立並顯示這項新資料。

現在建議您前往我們的中繼層級教學課程 」[MVC Music 市集](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)」 以及許多的影片和辦公[ https://asp.net/mvc ](https://asp.net/mvc)更進一步了解 ASP.NET MVC ！

敬祝您使用愉快！

- Scott Hanselman- [ http://hanselman.com ](http://hanselman.com)並[ @shanselman ](http://twitter.com/shanselman)在 Twitter 上。

> [!div class="step-by-step"]
> [上一步](getting-started-with-mvc-part7.md)
