---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: 第 2 部分： 控制站 |Microsoft Docs
author: jongalloway
description: 本教學課程系列會詳細說明所有建置 ASP.NET MVC Music 市集範例應用程式所採取的步驟。 第 2 部分涵蓋控制站。
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: 8824d5d2f5670aee2df6dc6e74767e4a851dd4ae
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841091"
---
<a name="part-2-controllers"></a>第 2 部分： 控制站
====================
藉由[Jon Galloway](https://github.com/jongalloway)

> MVC Music 市集是介紹，並逐步說明如何使用 ASP.NET MVC 和 Visual Studio 進行 web 開發的教學課程應用程式。  
>   
> MVC Music 市集是銷售線上音樂 album 和實作基本的網站管理、 使用者登入時，和 「 購物車 」 功能的輕量級的範例存放區實作。  
>   
> 本教學課程系列會詳細說明所有建置 ASP.NET MVC Music 市集範例應用程式所採取的步驟。 第 2 部分涵蓋控制站。


有了傳統的 web 架構，傳入的 Url 通常會對應至磁碟上的檔案。 例如： URL 的要求想"/ Products.aspx"或"/ Products.php 」 可能會由 「 Products.aspx"或"Products.php 」 檔案的方式來處理。

Web 型 MVC 架構會稍有不同的方式，將 Url 對應至伺服器程式碼。 而不是將傳入的 Url 對應至檔案中，它們改為將 Url 對應至類別的方法。 這些類別稱為 「 控制項 」，他們會負責處理傳入的 HTTP 要求，處理使用者輸入擷取和儲存資料，並判斷要傳送的回應傳回至用戶端 （顯示 HTML、 下載檔案、 重新導向至不同URL 等）。

## <a name="adding-a-homecontroller"></a>加入 HomeController

首先我們 MVC Music 市集應用程式新增至我們的網站的首頁處理 Url 的控制器類別。 我們將遵循的 ASP.NET MVC 的預設命名慣例，並呼叫 HomeController。

以滑鼠右鍵按一下方案總管] 內的 「 控制項 」 資料夾，然後選取 [新增]，然後按一下 [[控制器] 命令：

![](mvc-music-store-part-2/_static/image1.jpg)

這會顯示 「 新增控制器 」 對話方塊。 控制器"HomeController"命名，然後按 [新增] 按鈕。

![](mvc-music-store-part-2/_static/image1.png)

這會建立新的檔案，HomeController.cs 中，為下列程式碼：

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

若要開始以盡可能簡單地，我們取代 Index 方法只會傳回字串的簡單方法。 我們會進行兩項變更：

- 變更要傳回的字串，而不是 ActionResult 方法
- 變更要傳回"Hello 從 Home"return 的陳述式

此方法現在看起來應該像這樣：

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a>執行應用程式

現在讓我們執行網站。 我們可以開始我們的 web 伺服器，並試試我們的網站使用下列任一項：

- 選擇偵錯 ⇨ 開始偵錯 功能表項目
- 按一下工具列中的綠色箭號按鈕 ![](mvc-music-store-part-2/_static/image2.jpg)
- 使用鍵盤快速鍵 F5。

使用任何上述步驟會編譯專案，並則導致是內建於 Visual Web Developer 中啟動 ASP.NET 程式開發伺服器。 通知會出現在表示 ASP.NET Development Server 已啟動，將螢幕的右下角，而且會用來顯示它下執行的連接埠號碼。

![](mvc-music-store-part-2/_static/image2.png)

Visual Web Developer 會接著會自動開啟瀏覽器視窗中的 URL 會指向我們的 web 伺服器。 這可讓我們快速試用我們的 web 應用程式：

![](mvc-music-store-part-2/_static/image3.png)

好，是相當快速的 – 我們建立新的網站，加入三價線函式，而且我們已在瀏覽器中的文字。 不我科學中，但它是一個起點。

*注意： Visual Web Developer 包含 ASP.NET Development Server，將在幾個隨機免費 「 連接埠 」 執行您的網站。在上面的螢幕擷取畫面，在執行站台`http://localhost:26641/`，因此它使用連接埠 26641。您的連接埠號碼將會不同。當我們談到 URL 的 like /Store/Browse 在本教學課程時，可將會移之後的連接埠號碼。假設 26641 的連接埠號碼，瀏覽至存放區/瀏覽表示瀏覽至`http://localhost:26641/Store/Browse`。*

## <a name="adding-a-storecontroller"></a>新增 StoreController

我們新增了一個簡單的 HomeController 實作本公司網站的首頁。 讓我們現在就加入我們將使用來實作瀏覽我們的音樂市集功能的另一個控制站。 我們的存放控制器將會支援三種案例：

- 在我們的 music store 中的音樂內容類型的清單頁面
- 列出所有在特定內容類型中的音樂 album 瀏覽 頁面
- 詳細資料頁面，其中顯示有關特定音樂專輯資訊

我們一開始先加入 StoreController 類別... 如果您還沒有這麼做，請停止執行應用程式藉由關閉瀏覽器，或選取偵錯 ⇨ 停止偵錯 功能表項目。

現在，加入新的 StoreController。 就像我們一樣 HomeController，會執行此作業，以滑鼠右鍵按一下方案總管 內的 「 控制項 」 資料夾，然後選擇 加入-&gt;控制器功能表項目

![](mvc-music-store-part-2/_static/image4.png)

我們新 StoreController 已經有 「 索引 」 方法。 我們將使用此 「 索引 」 方法來實作我們列出在我們的 music store 中的所有內容類型的清單頁面。 我們也會新增兩個額外的方法，來實作兩個其他情況下我們想要以處理我們 StoreController： 瀏覽和詳細資料。

「 控制器動作 」，會呼叫這些方法 （索引、 瀏覽和詳細資料），我們的控制器內，而且您已經看到與 HomeController.Index （） 動作方法，他們的工作回應 URL 的要求，並 （一般而言） 判斷哪些內容應該傳送回瀏覽器或叫用的 URL 的使用者。

我們一開始我們 StoreController 的實作，藉由變更 theIndex() 方法，以傳回字串"Hello 從 Store.Index() 」，我們將新增類似的方法 Browse() 和 Details():

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

再次執行專案，並瀏覽下列 Url:

- / 存放區
- / Store/瀏覽
- / 存放區/詳細資料

存取這些 Url，將會叫用控制器的動作方法，並傳回字串的回應：

![](mvc-music-store-part-2/_static/image5.png)

太棒了，但這些只是常數字串。 我們把它們動態的讓他們從 URL 取得資訊，並在網頁輸出中顯示它。

首先我們要變更瀏覽動作方法，從 URL 擷取查詢字串值。 我們可以將"genre"參數新增至我們的動作方法來執行這項操作。 當我們執行此動作 ASP.NET MVC 會自動將叫用時，名為"genre"至我們的動作方法的任何查詢字串或表單張貼參數傳遞。

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

*注意： 我們要使用 HttpUtility.HtmlEncode 公用程式方法來處理使用者輸入。這可防止使用者插入檢視中的 Javascript，例如 /Store/Browse 的連結嗎？內容類型 =&lt;指令碼&gt;window.location = 'http://hackersite.com'&lt;/script&gt;。*

現在讓我們瀏覽至存放區/瀏覽？內容類型 = Disco

![](mvc-music-store-part-2/_static/image6.png)

接下來讓我們變更即可讀取並顯示名為識別碼的輸入的參數的詳細資料動作 不同於我們的前一個方法，我們將不會進行內嵌的識別碼值做為查詢字串參數。 而我們會將它內嵌直接在 URL 本身內。 例如： /Store/Details/5。

ASP.NET MVC 可讓我們輕鬆地執行這項操作不需要設定任何項目。 ASP.NET MVC 的預設路由慣例是將 URL 的區段之後的動作方法名稱視為名為 「 識別碼 」 的參數。 如果動作方法有一個名為 ID 參數然後 ASP.NET MVC 會自動傳遞的 URL 區段您做為參數。

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

執行應用程式，並瀏覽至 /Store/Details/5:

![](mvc-music-store-part-2/_static/image7.png)

讓我們複習一下到目前為止完成的內容：

- 我們在 Visual Web Developer 中建立新的 ASP.NET MVC 專案
- 我們所討論的 ASP.NET MVC 應用程式的基本的資料夾結構
- 我們已了解如何執行我們的網站使用 ASP.NET 程式開發伺服器
- 我們建立了兩個控制器類別： HomeController 和 StoreController
- 我們已加入我們控制站會回應 URL 的要求，並傳回至瀏覽器的文字中的動作方法


> [!div class="step-by-step"]
> [上一頁](mvc-music-store-part-1.md)
> [下一頁](mvc-music-store-part-3.md)
