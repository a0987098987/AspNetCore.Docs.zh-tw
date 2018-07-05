---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
title: 新增 Create 方法和 Create 檢視 |Microsoft Docs
author: shanselman
description: 這是初學者教學課程中，將會介紹 ASP.NET MVC 的基本概念。 建立簡單的 web 應用程式，從資料庫讀取與寫入。
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: a3a90963-0286-4fa0-9b3d-c230cc18b0a3
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
msc.type: authoredcontent
ms.openlocfilehash: 976df78ea22c30c094f70a57792d287f15d2c62d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400904"
---
<a name="adding-a-create-method-and-create-view"></a>新增 Create 方法和 Create 檢視
====================
藉由[Scott Hanselman](https://github.com/shanselman)

> 這是初學者教學課程中，將會介紹 ASP.NET MVC 的基本概念。 您將建立簡單 web 應用程式，從資料庫讀取與寫入。 請瀏覽[ASP.NET MVC 學習中心](../../../index.md)來尋找其他 ASP.NET MVC 教學課程和範例。


在這一節我們即將實作，讓使用者可以在資料庫中建立新的影片所需的支援。 我們會實作電影/建立 URL 動作。

實作電影/建立 URL 是兩個步驟的程序。 當使用者第一次造訪電影/建立 URL 我們想要顯示這些輸入新的影片可填寫 HTML 表單。 然後，當使用者提交表單和資料回傳至伺服器的文章，我們想要擷取的已張貼的內容，並將它儲存到資料庫。

我們將我們 MoviesController 類別內實作這兩個 create （） 方法內的兩個步驟。 其中一種方法將會顯示&lt;表單&gt;使用者應該填寫以建立新的電影。 第二個方法將處理處理張貼的資料，當使用者提交&lt;表單&gt;回傳至伺服器，並儲存在資料庫裡面的新影片。

以下是程式碼我們會將它新增至我們的 MoviesController 類別，以實作此：

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample1.cs)]

上述程式碼包含所有我們需要我們控制器內的程式碼。

讓我們現在實作建立檢視範本，我們將使用它來向使用者顯示的表單。 我們將第一個建立方法中以滑鼠右鍵按一下，然後選取 [新增檢視] 來建立我們的電影表單的檢視範本。

我們將選取，我們會將檢視範本 「 電影 」 作為檢視的資料類別，表示我們想要 「 建立 」 的 「 建立 」 範本。

[![新增檢視](getting-started-with-mvc-part6/_static/image2.png)](getting-started-with-mvc-part6/_static/image1.png)

按一下 [新增] 按鈕之後，就會為您建立 \Movies\Create.aspx 檢視範本。 因為我們從 [檢視內容] 下拉式清單中選取 [建立]，[新增檢視] 對話方塊自動 「 建構 」 某些預設內容為我們。 Scaffolding 建立 HTML&lt;表單&gt;、 位置，以讓驗證錯誤訊息前往的而且每個屬性的類別 scaffolding 知道電影相關，因為建立標籤和欄位。

[!code-aspx[Main](getting-started-with-mvc-part6/samples/sample2.aspx)]

因為我們的資料庫會自動提供影片的識別碼，讓我們移除這些欄位，參考模型。從我們建立的檢視識別碼。 移除之後 7 線條&lt;圖例&gt;欄位&lt;/legend&gt; ，它們會顯示 [識別碼] 欄位，我們不想。

讓我們現在建立新的電影，並將它新增至資料庫。 我們會執行一次的應用程式來執行這項操作，請瀏覽 「 / 電影 」 URL，然後按一下 [建立] 連結來新增新電影。

[![建立-Windows Internet Explorer](getting-started-with-mvc-part6/_static/image4.png)](getting-started-with-mvc-part6/_static/image3.png)

當我們按一下 [建立] 按鈕時，我們會公佈回 （透過 HTTP POST) /Movies/Create 方法我們剛建立這個表單上的資料。 如同時系統會自動採取 「 numTimes"和"name"參數，在 url，和它們對應到稍早在方法上的參數，系統會自動從某篇文章中取得表單欄位，並將它們對應至物件。 在此情況下，例如"ReleaseDate"和"Title"的 HTML 中的欄位值會自動放入電影的新執行個體的正確的屬性。

讓我們看看第二個 Create 方法從我們 MoviesController 一次。 請注意它如何採用 「 電影 」 物件做為引數：

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample3.cs)]

此影片物件接著傳遞至我們建立的動作方法的 [HttpPost] 版本，我們並儲存在資料庫中再被使用者重新導向回到 index （） 動作方法，這個方法會將電影清單中顯示已儲存的結果：

[![電影清單-Windows Internet Explorer](getting-started-with-mvc-part6/_static/image6.png)](getting-started-with-mvc-part6/_static/image5.png)

如果我們的影片都正確無誤，不過，而且資料庫不允許我們沒有標題儲存影片，我們不檢查。 如果我們還可以告訴使用者的資料庫之前擲回錯誤，那就天下太平了。 我們藉由將我們的應用程式的驗證支援就此下一步。

> [!div class="step-by-step"]
> [上一頁](getting-started-with-mvc-part5.md)
> [下一頁](getting-started-with-mvc-part7.md)
