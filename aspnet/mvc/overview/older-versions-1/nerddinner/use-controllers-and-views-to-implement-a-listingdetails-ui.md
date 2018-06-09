---
uid: mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
title: 使用控制器和檢視來實作詳細資料清單/UI |Microsoft 文件
author: microsoft
description: 步驟 4 說明如何利用我們的模型中為使用者提供的資料清單/詳細資料瀏覽體驗的應用程式中新增的控制站...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 64116e56-1c9a-4f07-8097-bb36cbb6e57f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
msc.type: authoredcontent
ms.openlocfilehash: ac3568941eeef24bd9857c5787471aadea15fc7f
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/08/2018
ms.locfileid: "30875730"
---
<a name="use-controllers-and-views-to-implement-a-listingdetails-ui"></a>使用控制器和檢視來實作詳細資料清單/UI
====================
by [Microsoft](https://github.com/microsoft)

[下載 PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 這是一套免費的步驟 4 ["NerdDinner 」 應用程式教學課程](introducing-the-nerddinner-tutorial.md)，會逐步引導式如何建置小，但是完成，web 應用程式使用 ASP.NET MVC 1。
> 
> 步驟 4 說明如何利用我們的模型中為使用者提供的資料清單/詳細資料瀏覽體驗 dinners 我們 NerdDinner 站台上的應用程式中新增的控制站。
> 
> 如果您使用 ASP.NET MVC 3，我們建議您遵循[取得啟動與 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教學課程。


## <a name="nerddinner-step-4-controllers-and-views"></a>NerdDinner 步驟 4： 控制器和檢視

與傳統 web 架構 （傳統 ASP、 PHP、 ASP.NET Web Form 等等），傳入的 Url 通常會對應至磁碟上的檔案。 例如： URL 的要求想"/ Products.aspx"或"/ Products.php 」 可能會處理由 「 Products.aspx"或"Products.php"檔案。

網頁型 MVC 架構會稍有不同的方式，將 Url 對應至伺服端程式碼。 而不是將傳入 Url 對應至檔案，它們改用對應 Url 上類別的方法。 這些類別稱為 「 控制項 」 而且負責處理傳入的 HTTP 要求處理使用者輸入，就會擷取和儲存資料，以及決定要傳送的回應傳回至用戶端 （顯示 HTML、 下載檔案，將重新導向至不同的URL 等）。

既然我們已經建立基本模型 NerdDinner 應用程式下, 一步是新增會利用它為使用者提供的資料清單/詳細資料瀏覽體驗 Dinners 在網站上的應用程式的控制站。

### <a name="adding-a-dinnerscontroller-controller"></a>加入 DinnersController 控制器

我們將開始在 web 專案中，「 控制項 」 資料夾上按一下滑鼠右鍵，然後選取**新增-&gt;控制器**（您也可以輸入 Ctrl-M、 Ctrl + C 來執行此命令） 的功能表命令：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image1.png)

這會顯示 「 新增控制器 」 對話方塊：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image2.png)

我們將新的控制器"DinnersController 」，然後按一下 [新增] 按鈕。 Visual Studio 會再加入 DinnersController.cs 檔案我們 \Controllers 目錄下：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image3.png)

它也會開啟新 DinnersController 類別在程式碼編輯器中。

### <a name="adding-index-and-details-action-methods-to-the-dinnerscontroller-class"></a>將 index （） 和 Details() 動作方法加入至 DinnersController 類別

我們想要讓訪客使用我們的應用程式瀏覽一份即將 dinners，並讓他們按一下清單中任何 Dinner，請參閱相關的特定詳細資料。 我們將會發行下列 Url 從我們的應用程式來執行這項操作：

| **URL** | **目的** |
| --- | --- |
| */Dinners/* | 顯示即將 dinners HTML 清單 |
| */Dinners/Details/[id]* | 顯示有關特定的 dinner 內嵌於 URL-會比對的資料庫中 dinner DinnerID 「 識別碼 」 參數所指示的詳細資料。 例如： /Dinners/Details/2 會顯示的 HTML 網頁 Dinner DinnerID 值為 2 的相關詳細資料。 |

我們將發佈這些 Url 初始的實作兩個公用 「 執行方法 」 類別新增為我們 DinnersController 類似如下：

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample1.cs)]

接著，我們會執行 NerdDinner 應用程式，並使用我們的瀏覽器叫用它們。 在中輸入 *"/ Dinners /"* URL 會導致我們*index*執行，以及它的方法會傳回下列回應：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image4.png)

在中輸入 *"/ Dinners/詳細資料/2"* URL 會導致我們*Details()* 方法來執行，並傳回下列回應：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image5.png)

您可能會懷疑-如何 ASP.NET MVC 知道建立 DinnersController 類別和叫用這些方法？ 若要了解，讓我們快速看看如何路由的運作。

### <a name="understanding-aspnet-mvc-routing"></a>了解 ASP.NET MVC 路由

ASP.NET MVC 包含功能強大的 URL 路由引擎提供很大的彈性來控制如何將 Url 對應至控制器類別。 它可讓我們完全自訂 ASP.NET MVC 如何選擇控制器類別建立時，要在其上叫用，以及設定不同的方式，變數可自動從 URL/Querystring 剖析和傳遞給方法的參數的方法引數。 它會傳遞完全最佳化 SEO （搜尋引擎最佳化） 的站台，以及發行任何我們想要從應用程式的 URL 結構的彈性。

根據預設，新的 ASP.NET MVC 專案會隨附一組預先設定的已登錄的 URL 路由規則。 這可讓我們來輕鬆地開始使用的應用程式不需要明確設定的任何項目。 預設路由規則註冊，請參閱我們的專案 – 我們可以按兩下以開啟 「 Global.asax"檔案受測專案的根目錄中的 「 應用程式 」 類別：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image6.png)

「 RegisterRoutes"方法，這個類別的內註冊預設的 ASP.NET MVC 路由規則：

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample2.cs)]

「 路由。MapRoute()"上述的方法呼叫註冊將傳入 Url 對應至控制器類別使用的 URL 格式的預設路由規則: 「 / {controller} / {action} / {id}"– 其中 「 控制器 」 是具現化，控制器類別的名稱 「 動作 」 是的名稱公用方法來叫用，「 識別碼 」 是可以做為引數傳遞給方法的 URL 中內嵌的選擇性參數。 傳遞到 「 MapRoute()"方法呼叫的第三個參數是一組用於控制器/動作識別碼值，它們不會出現在 URL 中的預設值 (控制器 ="Home"，動作 ="Index"，識別碼 ="")。

以下是使用預設對應的資料表，示範如何在各種不同的 Url"<em>/ {控制器} / {action} / {id}"</em>路由規則：

| **URL** | **控制器類別** | **動作方法** | **傳遞參數** |
| --- | --- | --- | --- |
| */Dinners/Details/2* | DinnersController | Details(id) | id=2 |
| */Dinners/Edit/5* | DinnersController | Edit(id) | id=5 |
| */Dinners/Create* | DinnersController | Create （) | N/A |
| */ Dinners* | DinnersController | Index （) | N/A |
| */ 首頁* | HomeController | Index （) | N/A |
| */* | HomeController | Index （) | N/A |

最後三個資料列會顯示預設值 (控制器 = 首頁，動作 = 索引，Id ="") 正在使用。 因為 「 索引 」 方法註冊為預設動作名稱，如果沒有指定，"/ Dinners"和"/home"Url 原因 index 動作方法的控制器類別上叫用。 「 組織 」 控制站登錄為預設的控制站中，如果未指定，因為"/"的 URL，則會導致要建立、 HomeController 和 index 動作方法上叫用。

如果您不喜歡這些預設 URL 的路由規則，好消息是可輕鬆地變更-只內 RegisterRoutes 上述方法，編輯它們。 NerdDinner 應用程式，不過，我們不打算變更任何預設 URL 的路由規則 – 改為我們只是使用以-為。

### <a name="using-the-dinnerrepository-from-our-dinnerscontroller"></a>使用從我們 DinnersController DinnerRepository

現在讓我們來取代 DinnersController 我們目前實作與使用我們的模型中實作，index （） 和 Details() 動作方法。

我們會使用我們稍早建立實作該行為 DinnerRepository 類別。 我們會藉由新增參考的 「 NerdDinner.Models"命名空間中，"using"陳述式開始，然後將我們 DinnerRepository 的執行個體宣告做為欄位中，我們 DinnerController 類別上變更。

在本章稍後我們將介紹概念 「 相依性插入 」，並顯示我們的控制站，以取得更好的單元可讓 DinnerRepository 參考另一種方法，測試 –，但權限現在我們將只建立我們 DinnerRepository 的執行個體下面類似內嵌。

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample3.cs)]

現在我們已經準備好產生使用我們擷取的資料模型物件的 HTML 回應。

### <a name="using-views-with-our-controller"></a>使用我們的控制器與檢視

雖然您可以撰寫程式碼來組合 HTML，然後使用我們的動作方法內*Response.write* helper 方法，可將它傳送回用戶端，該方法就會成為變得相當快速。 更好的方法是讓我們只能執行應用程式和資料邏輯內我們 DinnersController 動作方法，並將再傳遞來呈現 HTML 回應給負責輸出 HTML 表示個別的 「 檢視 」 範本所需的資料它。 我們會看到在不久後，「 檢視 」 範本將會是文字檔案，其中通常包含 HTML 標記和內嵌的轉譯程式碼的組合。

我們控制器邏輯分離我們檢視轉譯會帶來幾個好處。 尤其是它會協助強化清楚 」 的重要性分離 」 應用程式程式碼與 UI 呈現格式化程式碼之間。 這使得更容易隔離單元測試應用程式邏輯與 UI 呈現邏輯。 它可讓您稍後修改 UI 轉譯範本，而不需要變更應用程式程式碼更容易。 它可以讓它更容易開發人員和設計師一起共同作業專案。

我們可以更新 DinnersController 類別來表示我們想要透過檢視表範本傳送回 HTML UI 回應變更我們的兩個動作方法的方法簽章的傳回類型為"void"改為使用"ActionResult"傳回型別。 我們可以呼叫*View()* 傳回到控制器基底類別的 helper 方法類似下面的 「 ViewResult"物件：

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample4.cs)]

簽章*View()* 我們使用上述的 helper 方法看起來如下：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image7.png)

第一個參數*View()* helper 方法是我們想要用來呈現 HTML 回應的檢視範本檔案的名稱。 第二個參數是包含資料中呈現 HTML 回應所需要的檢視表範本的模型物件。

在我們的 index 動作方法內，我們正在撥打*View()* helper 方法，並指出我們想要呈現 dinners 使用 「 索引 」 檢視範本的 HTML 清單。 我們在傳遞檢視範本的 Dinner 物件產生的清單，從序列：

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample5.cs)]

我們 Details() 動作方法內我們嘗試擷取 Dinner 物件使用的 URL 中提供的識別碼。 如果找到有效的 Dinner 我們呼叫*View()* helper 方法，表示我們想要使用 [詳細資料] 檢視範本來擷取的 Dinner 物件呈現。 如果要求不正確的 dinner，我們會呈現表示 Dinner 不存在使用"NotFound"檢視範本的有用的錯誤訊息 (與多載的版本*View()* helper 方法，只使用範本名稱):

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample6.cs)]

讓我們現在實作的"NotFound"、 「 詳細資料 」 和 「 索引 」 的 「 檢視 」 範本。

### <a name="implementing-the-notfound-view-template"></a>實作"NotFound"檢視範本

我們一開始會藉由實作"NotFound"檢視範本 – 顯示易懂的錯誤訊息，指出找不到要求的 dinner。

我們將建立新的檢視表範本放在控制器動作方法中，我們文字游標然後以滑鼠右鍵按一下並選擇 [新增檢視] 功能表命令 （我們也可以輸入 Ctrl-M、 Ctrl-V 來執行此命令）：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image8.png)

這會顯示類似的 [新增檢視] 對話方塊下方。 根據預設，對話方塊會預先填入要建立之檢視的名稱要比對動作方法的名稱資料指標時所在的對話方塊已啟動 （在此情況下 [詳細資料]）。 由於我們想要先實作"NotFound"範本，我們將會覆寫這個檢視表名稱，並設定它改為"NotFound":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image9.png)

當我們按一下 [新增] 按鈕時，Visual Studio 將"\Views\Dinners"目錄 （若目錄不存在時，它也會建立） 內就讓我們建立新的"NotFound.aspx 」 檢視範本：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image10.png)

它也會開啟我們新"NotFound.aspx 」 檢視的範本程式碼編輯器中：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image11.png)

檢視預設的範本有兩個 「 內容區域 」，我們可以在其中新增內容及程式碼。 第一個可讓我們自訂"title"的 HTML 網頁送回。 第二個可讓我們用來自訂 「 主要內容 」 的 HTML 網頁送回。

若要實作我們"NotFound"檢視範本中，我們會將新增一些基本的內容：

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample7.aspx)]

我們可以再現在就試試看瀏覽器中。 若要這樣做讓我們要求 *"/ Dinners/詳細資料/9999"* URL。 這會指 dinner，目前不存在於資料庫中，且會導致我們 DinnersController.Details() 動作方法，以呈現我們"NotFound"檢視的範本：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image12.png)

您會注意到上述螢幕擷取畫面中的一件事是 HTML 的我們基本的檢視的範本已繼承一堆圍繞在螢幕上的主要內容。 這是因為我們檢視範本使用的"master page"範本，讓我們在站台上的所有檢視之間套用一致的版面配置。 我們將討論主版頁面在本教學課程的後續部分中的多個工作的方式。

### <a name="implementing-the-details-view-template"></a>實作 [詳細資料] 檢視範本

讓我們現在實作的 [詳細資料] 檢視範本 – 將單一 Dinner 模型產生 HTML。

我們會這麼做定位在詳細資料動作方法中，我們文字游標然後以滑鼠右鍵按一下並選擇 [新增檢視] 功能表命令 （或按 Ctrl-M、 Ctrl-V）：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image13.png)

這會顯示 [新增檢視] 對話方塊。 我們會保留預設檢視表名稱 ("Details")。 我們也會在對話方塊中選取 [建立強型別檢視表] 核取方塊，然後選取 （使用下拉式方塊的下拉式清單中） 我們傳遞從控制器到檢視的模型型別名稱。 此檢視中，我們會傳遞 Dinner 物件 (此類型的完整的名稱是: 「 NerdDinner.Models.Dinner"):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image14.png)

不同於先前的範本，其中我們選擇建立的 「 空的檢視 」，的此時，我們會自動選擇"scaffold 」 檢視使用 [詳細資料] 範本。 我們可以變更 [檢視內容] 下拉式清單中上述對話方塊來表示。

「 Scaffolding"會產生初始我們根據我們要傳遞給它的 Dinner 物件的詳細資料檢視範本的實作。 這提供簡單的方法，讓我們快速地開始在我們檢視範本的實作。

當我們按一下 [新增] 按鈕時，Visual Studio 會我們"\Views\Dinners"目錄中就我們建立新的 「 Details.aspx 」 檢視的範本檔：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image15.png)

它也會開啟程式碼編輯器中我們新"Details.aspx 」 檢視範本。 它會包含詳細資料檢視 Dinner 模型為基礎的初始 scaffold 實作。 Scaffolding 引擎會使用.NET 反映來查看傳遞它，在類別上公開的公用屬性，並會加入適當的內容根據每個找到的類型：

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample8.aspx)]

我們可以要求 *"/ 1/Dinners/詳細資料 」* URL，請參閱此 [詳細資料] scaffold 實作在瀏覽器中的外觀。 使用此 URL 將會顯示我們手動新增至資料庫時，我們建立 dinners 的其中一個：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image16.png)

這會啟動並執行快速，讓我們，並提供我們的我們 Details.aspx 檢視的初始實作。 我們可以前往並調整其自訂的 UI，讓我們滿意。

當我們探討 Details.aspx 範本更緊密時，我們會發現它包含靜態的 HTML 以及內嵌轉譯程式碼。 &lt;%%&gt;程式碼區塊會執行程式碼時檢視範本轉譯時，和&lt;%= %&gt;程式碼區塊執行其中所包含的程式碼，然後轉譯輸出資料流的範本結果。

我們會存取 「 Dinner 」 模型物件傳遞從 controller 使用強型別 「 模型 」 屬性的檢視中，我們可以撰寫程式碼。 Visual Studio 提供完整的 intellisense 程式碼時存取這個 「 模型 」 的屬性，在編輯器中：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image17.png)

讓我們進行某些做調整，讓我們最後一個詳細資料檢視範本的來源看起來如下：

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample9.aspx)]

當存取 *"/ 1/Dinners/詳細資料 」* URL 一次它即將轉譯類似下面的：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image18.png)

### <a name="implementing-the-index-view-template"></a>實作 「 索引 」 檢視範本

讓我們現在實作的 「 索引 」 檢視範本 – 將會產生未來 Dinners 的清單。 待辦事項此我們會在索引動作方法中，我們文字游標的位置，然後以滑鼠右鍵按一下並選擇 [新增檢視] 功能表命令 （或按 Ctrl-M、 Ctrl-V）。

在 [新增檢視] 對話方塊中我們將保留檢視範本名為"Index"，並選取 [建立強型別檢視表] 核取方塊。 此時，我們將選擇要自動產生的 「 清單 」 檢視範本，並選取"NerdDinner.Models.Dinner"做為模型類型傳遞至檢視 (這因為我們已指出，我們會建立 scaffold 會導致 [新增檢視] 對話方塊，假設我們是一個 「 清單 」傳遞 Dinner 物件序列從 Controller 檢視）：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image19.png)

當我們按一下 [新增] 按鈕時，Visual Studio 將我們"\Views\Dinners"目錄中就讓我們建立新的"Index.aspx 」 檢視範本檔案。 它會 「 scaffold"初始實作中提供 Dinners 我們傳遞至檢視的 HTML 資料表清單。

當我們執行應用程式及存取 *"/ Dinners /"* URL 將會呈現 dinners 的清單，就像這樣：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image20.png)

上述表格方案提供我們 Dinner 資料 – 不相當是我們所要面對 Dinner 清單我們消費者類似格線版面配置。 我們可以更新 Index.aspx 檢視範本，然後修改它來列出較少的資料行的資料，並使用&lt;ul&gt;轉譯它們，而不是資料表，使用下列程式碼的項目：

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample10.aspx)]

因為我們會在我們的模型執行迴圈每個晚餐，我們使用上述的 foreach 陳述式內的"var"關鍵字。 這些熟悉 C# 3.0 可能會考慮，使用"var"表示 dinner 物件是晚期繫結。 而是表示編譯器使用型別-推斷針對強型別"Model"屬性 (類型"IEnumerable&lt;Dinner&gt;") 和編譯為 Dinner 類型 – 這表示我們取得完整的本機 「 dinner"變數intellisense 和編譯時間檢查它在程式碼區塊：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image21.png)

當我們點擊 重新整理 */Dinners*我們更新的檢視現在看起來像下面我們瀏覽器中的 URL:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image22.png)

這會尋找比較好，但尚未完全有。 我們最後一個步驟是讓使用者可以按一下清單中的個別 Dinners 和其相關詳細資料請參閱。 我們將會轉譯 HTML 超連結項目連結至詳細資料動作方法，在我們 DinnersController 來實作。

我們可以在其中一種我們索引檢視表中產生這些超連結。 第一個方法是以手動方式建立 HTML &lt;&gt;項目類似下面的其中我們內嵌&lt;%%&gt;中封鎖了&lt;&gt; HTML 項目：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image23.png)

我們可以使用的替代方式是利用內建的 「 Html.ActionLink()"helper 方法支援以程式設計方式建立 HTML 的 ASP.NET MVC 中&lt;&gt;連結至其他動作方法的項目控制站：

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample11.aspx)]

Html.ActionLink() helper 方法的第一個參數是要顯示的連結文字 （在此情況下標題的 dinner），第二個參數是我們想要產生的連結 （在此情況下的詳細資料的方法），控制器動作名稱和第三個參數一組要傳送 （實作為具有屬性名稱/值的匿名類型） 之動作的參數。 我們將在此情況下指定的 dinner 我們想要連結至，而且因為預設的 URL 路由規則在 ASP.NET MVC 中的 「 識別碼 」 參數"{Controller} / {Action} / {id}"Html.ActionLink() helper 方法會產生下列輸出：

[!code-html[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample12.html)]

我們 Index.aspx 檢視我們將使用 Html.ActionLink() helper 方法的方法以及每個 dinner 清單連結至適當的詳細資料的 URL 中：

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample13.aspx)]

現在我們叫用時和 */Dinners* dinner 清單看起來像下列的 URL:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image24.png)

當我們按一下任一清單中 Dinners 我們會瀏覽以查看其相關詳細資料：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image25.png)

### <a name="convention-based-naming-and-the-views-directory-structure"></a>命名慣例為基礎和 \Views 目錄結構

預設的 ASP.NET MVC 應用程式使用以慣例為基礎的目錄解析檢視範本時，命名結構。 這可讓開發人員不必完整限定的位置路徑參考從控制器類別中的檢視時。 根據預設，ASP.NET MVC 會尋找檢視的範本檔案內 * \Views\[ControllerName]\*目錄底下的應用程式。

比方說，我們已使用的 DinnersController 類別 – 明確參考三個檢視的範本: 「 索引 」、 「 詳細資料 」 和"NotFound"。 ASP.NET MVC 會依預設尋找這些檢視內*\Views\Dinners*我們的應用程式根目錄底下的目錄：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image26.png)

請注意上述方式沒有目前專案中的三個控制器類別 (DinnersController、 HomeController 和 AccountController – 最後兩個已加入依預設，當我們建立專案時)，而且有三個子目錄 （一個用於每個控制站） \Views 目錄內。

從首頁和帳戶控制器所參考的檢視將會自動解除其檢視範本，從個別*\Views\Home*和*\Views\Account*目錄。 *\Views\Shared*子目錄提供方法來儲存跨多個應用程式中的控制器會重複使用的檢視範本。 當 ASP.NET MVC 會嘗試解析檢視範本時，它會先檢查，內*\Views\[控制站]* 特定目錄中，如果找不到檢視表範本那里它將會尋找內*\Views\共用*目錄。

命名個別檢視範本時，若要將共用相同的名稱做為動作方法，導致呈現檢視範本是建議的指引。 例如，上面我們 「 索引 」 動作的方法呈現檢視結果，使用 「 索引 」 檢視和 [詳細資料] 動作方法使用 [詳細資料] 檢視來呈現其結果。 這可讓您輕鬆快速地查看哪一個範本是與每個動作相關聯。

若要檢視範本有相同名稱做為控制器上叫用動作方法時明確指定檢視表範本名稱不需要開發人員。 我們可以改為只傳遞模型物件 」 View()"helper 方法 （而不指定檢視表名稱），和 ASP.NET MVC 會自動推斷我們想要使用*\Views\[ControllerName]\[ActionName]* 檢視範本來對應它的磁碟上。

這可讓我們稍有清除控制器的程式碼，並避免重複兩次中的程式碼的名稱：

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample14.cs)]

上述程式碼會實作 nice Dinner 清單/詳細資料所需的所有站台的經驗。

#### <a name="next-step"></a>下一個步驟

我們現在有 nice Dinner，瀏覽建置體驗。

我們現在啟用編輯支援的 CRUD （建立、 讀取、 更新、 刪除） 資料格式。

> [!div class="step-by-step"]
> [上一頁](build-a-model-with-business-rule-validations.md)
> [下一頁](provide-crud-create-read-update-delete-data-form-entry-support.md)
