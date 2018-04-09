---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: 第 2 部分： 控制站 |Microsoft 文件
author: jongalloway
description: 此教學課程系列詳細列出所有建置 ASP.NET MVC 商店範例應用程式所採取的步驟。 第 2 部分涵蓋控制站。
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: 680cdea388d9b01961bd626643c0fd91c9205ed7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="part-2-controllers"></a>第 2 部分： 控制站
====================
由[Jon Galloway](https://github.com/jongalloway)

> MVC Music Store 是介紹並逐步說明如何使用 ASP.NET MVC 和 Visual Studio 針對 web 程式開發的教學課程應用程式。  
>   
> MVC Music Store 是輕量級範例存放區實作銷售音樂專輯上線，並實作基本的網站管理、 使用者登入和購物車的功能。  
>   
> 此教學課程系列詳細列出所有建置 ASP.NET MVC 商店範例應用程式所採取的步驟。 第 2 部分涵蓋控制站。


與傳統 web 架構，傳入的 Url 通常會對應至磁碟上的檔案。 例如： URL 的要求想"/ Products.aspx"或"/ Products.php 」 可能會處理由 「 Products.aspx"或"Products.php"檔案。

網頁型 MVC 架構會稍有不同的方式，將 Url 對應至伺服端程式碼。 而不是將傳入 Url 對應至檔案，它們改用對應 Url 上類別的方法。 這些類別稱為 「 控制項 」 而且負責處理傳入的 HTTP 要求處理使用者輸入，就會擷取和儲存資料，以及決定要傳送的回應傳回至用戶端 （顯示 HTML、 下載檔案，將重新導向至不同的URL 等）。

## <a name="adding-a-homecontroller"></a>加入 HomeController

我們一開始我們 MVC Music Store 的應用程式會藉由新增控制器類別處理本公司網站的首頁 Url。 我們會遵循的 ASP.NET MVC 的預設命名慣例，並呼叫它 HomeController。

以滑鼠右鍵按一下 [方案總管] 中的 「 控制項 」 資料夾，並選取 [新增]，然後 [控制器] 命令：

![](mvc-music-store-part-2/_static/image1.jpg)

這會顯示 「 新增控制器 」 對話方塊。 控制器"HomeController 」，然後按 [新增] 按鈕。

![](mvc-music-store-part-2/_static/image1.png)

這將建立新的檔案，HomeController.cs，下列程式碼：

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

若要啟動以盡可能簡單地，我們索引以取代方法只會傳回字串的簡單方法。 我們將會進行兩項變更：

- 變更傳回的字串，而不是 ActionResult 方法
- 變更傳回的陳述式，傳回"Hello 從 Home 」

此方法現在看起來應該像這樣：

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a>執行應用程式

現在讓我們來執行網站。 我們可以開始我們的 web 伺服器，然後再次嘗試使用任何下列網站::

- 選擇偵錯 ⇨ 開始偵錯 功能表項目
- 按一下工具列中的綠色箭頭按鈕 ![](mvc-music-store-part-2/_static/image2.jpg)
- 使用鍵盤快速鍵，F5。

使用任何上述的步驟會編譯專案，並則會導致是內建 Visual Web Developer，才能啟動 ASP.NET 程式開發伺服器。 表示 ASP.NET 程式開發伺服器已啟動，將螢幕的右下角會出現通知，並會用來顯示它在下執行的連接埠號碼。

![](mvc-music-store-part-2/_static/image2.png)

Visual Web Developer 然後會自動開啟瀏覽器視窗，其 URL 指到我們的 web 伺服器。 這可讓我們快速試用我們的 web 應用程式：

![](mvc-music-store-part-2/_static/image3.png)

好是非常快速 – 我們建立了新的網站，加入新三價線函式，而且我們在瀏覽器中有文字。 不我科學中，但它是起點。

*注意： Visual Web Developer 包含 ASP.NET 程式開發伺服器，將在隨機免費 「 連接埠 」 號碼執行您的網站。在上面的螢幕擷取畫面，在執行站台`http://localhost:26641/`，因此它正在使用連接埠 26641。連接埠號碼將會不同。當我們在本教學課程談論 URL 的 like /Store/Browse，會提供通訊埠編號後面。假設 26641 的連接埠號碼，瀏覽至存放區/瀏覽代表將會瀏覽至`http://localhost:26641/Store/Browse`。*

## <a name="adding-a-storecontroller"></a>加入 StoreController

我們加入簡單的 HomeController 實作本公司網站的首頁。 讓我們現在加入另一個控制站，我們將使用實作瀏覽我們的音樂存放區的功能。 存放區 controller 將會支援三種案例：

- 在我們的 music store 中的音樂內容類型清單頁面
- 瀏覽 頁面會列出所有音樂專輯特定內容類型
- 顯示有關特定音樂專輯資訊詳細資料頁面

我們會先加入新的 StoreController 類別... 如果您還沒有這麼做，請停止執行應用程式藉由關閉瀏覽器，或選取偵錯 ⇨ 停止偵錯 功能表項目。

現在，加入新 StoreController。 如同我們在與 HomeController 會執行此作業，在 [方案總管] 中的 「 控制項 」 資料夾上按一下滑鼠右鍵，然後選擇 [新增]-&gt;控制器功能表項目

![](mvc-music-store-part-2/_static/image4.png)

我們新 StoreController 已經有一個 「 索引 」 方法。 若要實作我們列出在我們的 music store 中的所有內容類型的清單頁面，我們將使用此 「 索引 」 方法。 我們也會加入兩個其他的方法，以實作兩個其他情況下我們想要處理我們 StoreController： 瀏覽和詳細資料。

會呼叫這些方法 （索引、 瀏覽和詳細資料），我們的控制器內 [控制器動作]，並且您已經看到與 HomeController.Index （） 的動作方法，其作業來回應 URL 要求，並 （一般來說，） 決定哪些內容應該傳回至瀏覽器或叫用的 URL 的使用者。

藉由變更 theIndex() 方法以傳回字串"Hello 從 Store.Index()"，就會開始我們 StoreController 的實作，我們將新增類似的方法 Browse() 和 Details():

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

重新執行專案，然後瀏覽下列 Url:

- /Store
- / 存放區/瀏覽
- / 存放區/詳細資料

存取下列 Url 會叫用動作方法，我們的控制器內，並會傳回字串的回應：

![](mvc-music-store-part-2/_static/image5.png)

太好了，但這些只常數字串。 我們把它們動態的好讓您從 URL 取得資訊並顯示在網頁輸出中。

首先，我們將會變更瀏覽動作方法，從 URL 擷取查詢字串值。 我們可以將 「 類型 」 參數加入至動作方法來執行這項操作。 當執行此作業，ASP.NET MVC 會自動傳遞叫用時，名為 「 類型 」 至我們的動作方法的任何查詢字串或表單張貼參數。

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

*注意： 我們會使用 HttpUtility.HtmlEncode 公用程式方法來處理使用者輸入。如此可防止使用者將 Javascript 插入我們檢視以便 /Store/Browse 類似的連結嗎？內容類型 =&lt;指令碼&gt;window.location='http://hackersite.com'&lt;/指令碼&gt;。*

現在讓我們來瀏覽至存放區/瀏覽？內容類型 = Disco

![](mvc-music-store-part-2/_static/image6.png)

讓我們來讀取和顯示輸入的參數的詳細資料動作的下一步 變更名為識別碼。 不同於我們先前的方法中，我們將不會內嵌的識別碼值做為查詢字串參數。 而是我們會將它內嵌直接在 URL 本身內。 例如： /Store/Details/5。

ASP.NET MVC 可讓我們能夠輕鬆地執行這項操作而不需要設定任何項目。 ASP.NET MVC 的預設路由慣例是在動作方法名稱後面的 URL 區段視為名為 「 識別碼 」 的參數。 如果動作方法有名稱為 ID 參數則 ASP.NET MVC 會自動傳遞的 URL 區段給您做為參數。

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

執行應用程式，並瀏覽至 /Store/Details/5:

![](mvc-music-store-part-2/_static/image7.png)

讓我們先複習到目前為止完成的內容：

- 我們已在 Visual Web Developer 中建立新的 ASP.NET MVC 專案
- 我們已討論過 ASP.NET MVC 應用程式的基本的資料夾結構
- 我們學到如何執行我們的網站使用 ASP.NET 程式開發伺服器
- 我們建立了兩個控制器類別： HomeController 和 StoreController
- 我們已將動作方法加入至我們控制站會回應 URL 要求，並傳回至瀏覽器的文字


> [!div class="step-by-step"]
> [上一頁](mvc-music-store-part-1.md)
> [下一頁](mvc-music-store-part-3.md)
