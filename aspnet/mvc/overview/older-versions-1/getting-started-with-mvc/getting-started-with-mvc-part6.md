---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
title: "加入建立方法，並建立檢視 |Microsoft 文件"
author: shanselman
description: "這是初學者教學課程介紹基本概念的 ASP.NET MVC。 您將建立簡單的 web 應用程式可讀取和寫入資料庫中。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: a3a90963-0286-4fa0-9b3d-c230cc18b0a3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
msc.type: authoredcontent
ms.openlocfilehash: 4792689087ab85be25fe186b2ec97915af448ef9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-create-method-and-create-view"></a>加入建立方法，並建立檢視
====================
由[Scott Hanselman](https://github.com/shanselman)

> 這是初學者教學課程介紹基本概念的 ASP.NET MVC。 您將建立簡單的 web 應用程式可讀取和寫入資料庫中。 請瀏覽[ASP.NET MVC 學習中心](../../../index.md)教學課程和範例，請尋找其他 ASP.NET MVC。


本節中，我們即將實作需要讓使用者在資料庫中建立新的電影的支援。 藉由實作電影/建立 URL 動作，我們會執行這項操作。

實作電影/建立 URL 是兩個步驟的程序。 當使用者第一次造訪電影/建立 URL 我們想要顯示它們可以填寫輸入新的電影的 HTML 表單。 然後，當使用者提交表單和文章回傳至伺服器的資料，我們想要擷取的已張貼的內容，並將它儲存到資料庫。

我們將我們 MoviesController 類別內實作這兩個 create （） 方法內的兩個步驟。 一種方法將會顯示&lt;表單&gt;使用者應該填寫建立新的電影。 第二種方法將處理處理張貼的資料，當使用者提交&lt;表單&gt;回傳至伺服器，並儲存新的電影中我們的資料庫。

以下是程式碼我們會加入可實作此 MoviesController 類別：

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample1.cs)]

上述程式碼包含所有我們需要我們控制器內的程式碼。

讓我們現在實作我們將使用它來向使用者顯示的表單建立檢視範本。 我們會在第一次的 Create 方法中以滑鼠右鍵按一下，然後選取 建立電影表單檢視範本的 「 加入檢視 」。

我們將會選取我們會為其檢視資料類別中，移至傳遞檢視範本"影片"，表示我們想要 「 scaffold 」 的 「 建立 」 範本。

[![加入檢視](getting-started-with-mvc-part6/_static/image2.png)](getting-started-with-mvc-part6/_static/image1.png)

按一下 [新增] 按鈕之後，就會為您建立 \Movies\Create.aspx 檢視範本。 因為我們從 檢視內容 的下拉式清單中選取 建立，加入檢視 對話方塊會自動"scaffold"某些預設內容為我們。 Scaffolding 建立 HTML&lt;表單&gt;、 移，訊息驗證錯誤的位置和 scaffolding 知道電影，因為其標籤和欄位用於建立每一個屬性的類別。

[!code-aspx[Main](getting-started-with-mvc-part6/samples/sample2.aspx)]

因為我們的資料庫會自動提供給影片識別碼，讓我們來移除這些欄位，該參考模型。從我們建立檢視的識別碼。 移除 7 之後的行&lt;圖例&gt;欄位&lt;/legend&gt;它們會顯示 [識別碼] 欄位，所以我們不想為。

我們現在建立新的電影，並將它加入至資料庫。 我們會執行的應用程式來執行這項操作，請瀏覽 」 / 電影 」 URL，然後按一下 [建立] 連結以新增新的電影。

[![建立-Windows Internet Explorer](getting-started-with-mvc-part6/_static/image4.png)](getting-started-with-mvc-part6/_static/image3.png)

當我們按一下 [建立] 按鈕時，我們將會公佈回 （透過 HTTP POST) 此表單以剛才所建立的 /Movies/Create 方法上的資料。 如同時系統會自動採取"numTimes 」 和 「 名稱 」 參數從 URL 和它們對應至先前的方法上的參數，系統會自動採取從張貼的表單欄位，並將其對應的物件。 在此情況下，例如"ReleaseDate"和"Title"的 HTML 中欄位的值會自動放電影的新執行個體的正確屬性。

讓我們看看第二個建立方法從我們 MoviesController 一次。 請注意花費"影片"物件做為引數的方式：

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample3.cs)]

此影片物件接著傳遞給我們建立動作方法中，[HttpPost] 版本，我們儲存在資料庫中，然後使用者重新導向至 index 動作方法，這個方法會將電影清單中顯示已儲存的結果：

[![電影清單-Windows Internet Explorer](getting-started-with-mvc-part6/_static/image6.png)](getting-started-with-mvc-part6/_static/image5.png)

我們不檢查是否我們電影是正確的不過，而且資料庫將不會讓我們將電影儲存沒有標題。 如果我們無法告訴使用者的資料庫之前擲回錯誤，它應該不錯。 我們將會執行這個下一步，方法是加入我們的應用程式的驗證支援。

>[!div class="step-by-step"]
[上一頁](getting-started-with-mvc-part5.md)
[下一頁](getting-started-with-mvc-part7.md)
