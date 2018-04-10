---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: 將資料行加入至模型 |Microsoft 文件
author: shanselman
description: 這是初學者教學課程介紹基本概念的 ASP.NET MVC。 建立簡單的 web 應用程式可讀取和寫入資料庫中。
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 7a0b64ce00fc5ee6d49990f1d4d93a154c467bf5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/10/2018
---
<a name="adding-a-column-to-the-model"></a>將資料行加入至模型
====================
由[Scott Hanselman](https://github.com/shanselman)

> 這是初學者教學課程介紹基本概念的 ASP.NET MVC。 您將建立簡單的 web 應用程式可讀取和寫入資料庫中。 請瀏覽[ASP.NET MVC 學習中心](../../../index.md)教學課程和範例，請尋找其他 ASP.NET MVC。


本節中，我們即將逐步解說我們如何對我們的資料庫，結構描述變更及處理應用程式中所做的變更。

讓我們影片資料表中加入"評等 」 的資料行。 返回 IDE，並按一下 [資料庫總管]。 以滑鼠右鍵按一下 影片 資料表，然後選取 開啟資料表定義。

新增 「 等級 」 資料行，如下所示。 我們現在不需要任何評等，因為資料行可允許 null。 按一下 [儲存]。

[![編輯電影資料表](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)

接下來，返回 [方案總管]，然後開啟 Movies.edmx 檔案 （這是 \Models 資料夾中）。 以滑鼠右鍵按一下設計介面 （白色區域） 上，選取 從資料庫更新模型。

[![影片-Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)

這會啟動 「 更新精靈 」。 按一下 [重新整理] 索引標籤中的，然後按一下 [完成]。 我們的影片模型類別就會更新與新的資料行。

![更新精靈 (2)](getting-started-with-mvc-part8/_static/image5.png)

之後按一下 [完成]，您可以看到新的評等資料行已加入至電影中的實體我們的模型。

[![影片實體](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)

我們已在資料庫模型中，加入資料行，但檢視不了解它。

## <a name="update-views-with-model-changes"></a>更新檢視的模型變更

有幾個方法，我們無法更新以反映新的評等資料行我們檢視範本。 我們建立這些檢視透過 [新增檢視] 對話方塊產生它們，因為我們無法加以刪除，再重新建立它們。 不過，通常人員將已經完成初始 scaffold 產生從其檢視範本的修改，會想要加入或刪除欄位，以手動方式，就像我們之前使用 [識別碼] 欄位建立。

開啟 \Views\Movies\Index.aspx 範本，並新增&lt;th&gt;分級&lt;t&gt;標頭的電影資料表。 我將我加入內容類型之後。 然後，在相同的資料行位置，但下面的部分，將行加入輸出我們新的評等。

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

我們最終 Index.aspx 範本看起來像這樣：

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

讓我們再開啟 \Views\Movies\Create.aspx 範本，並將標籤和文字方塊中加入新的評等屬性：

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

我們最終 Create.aspx 範本才會看起來像這樣，並讓我們來變更 我們瀏覽器的標題和次要&lt;h2&gt;標題為類似"建立電影"雖然我們在這裡 ！

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

執行您的應用程式，現在您已經有新的欄位已加入到 [建立] 頁面的資料庫中。 加入新的電影-分級-這個階段，並按一下 建立。

[![建立電影-Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)

按一下 [建立] 之後，您要傳送至索引頁面在新的電影列出您使用新的評等資料行在資料庫中

[![電影清單-Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)

此基本教學課程了您開始進行控制站，檢視與建立關聯，並傳遞硬式編碼的資料。 然後我們建立及設計資料庫，並將一些資料放在。 我們會從資料庫擷取資料，並顯示 HTML 表格中。 然後我們會加入可讓使用者將資料加入至資料庫本身從 Web 應用程式內建立新表單。 我們加入驗證，然後進行驗證用戶端使用 JavaScript。 最後，我們變更了資料庫以包含新的資料行的資料，然後更新我們來建立並顯示這項新資料的兩個頁面。

現在鼓勵您移到我們的中繼層級教學課程 」[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)」 以及許多視訊和資源的[ https://asp.net/mvc ](https://asp.net/mvc)若要了解更多關於 ASP.NET MVC ！

敬祝您使用愉快！

- Scott Hanselman- [ http://hanselman.com ](http://hanselman.com)和[ @shanselman ](http://twitter.com/shanselman) Twitter 上。

> [!div class="step-by-step"]
> [上一步](getting-started-with-mvc-part7.md)
