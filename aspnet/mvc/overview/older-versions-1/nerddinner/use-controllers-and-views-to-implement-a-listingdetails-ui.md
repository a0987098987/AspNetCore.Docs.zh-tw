---
uid: mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
title: 使用控制器和檢視來實作清單/詳細資料 UI |Microsoft Docs
author: microsoft
description: 步驟 4 說明如何將控制器新增至會充分利用我們的模型，為使用者提供的資料清單/詳細資料瀏覽體驗的應用程式...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 64116e56-1c9a-4f07-8097-bb36cbb6e57f
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
msc.type: authoredcontent
ms.openlocfilehash: 4fe065e29950a076de07d73205a97399f82f07d6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377872"
---
<a name="use-controllers-and-views-to-implement-a-listingdetails-ui"></a>使用控制器和檢視來實作清單/詳細資料 UI
====================
by [Microsoft](https://github.com/microsoft)

[下載 PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 這是一套免費的步驟 4 ["NerdDinner 」 應用程式教學課程](introducing-the-nerddinner-tutorial.md)，逐步解說如何建置一個小型的但在完成時，使用 ASP.NET MVC 1 的 web 應用程式。
> 
> 步驟 4 會示範如何將控制器新增至會充分利用我們的模型，為使用者提供的資料清單/詳細資料瀏覽體驗 dinners NerdDinner 網站上的應用程式。
> 
> 如果您使用 ASP.NET MVC 3，我們建議您遵循[取得開始使用 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或是[MVC Music 市集](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教學課程。


## <a name="nerddinner-step-4-controllers-and-views"></a>NerdDinner 步驟 4： 控制器和檢視

有了傳統的 web 架構 （傳統 ASP、 PHP、 ASP.NET Web Form 等等），傳入的 Url 通常會對應至磁碟上的檔案。 例如： URL 的要求想"/ Products.aspx"或"/ Products.php 」 可能會由 「 Products.aspx"或"Products.php 」 檔案的方式來處理。

Web 型 MVC 架構會稍有不同的方式，將 Url 對應至伺服器程式碼。 而不是將傳入的 Url 對應至檔案中，它們改為將 Url 對應至類別的方法。 這些類別稱為 「 控制項 」，他們會負責處理傳入的 HTTP 要求，處理使用者輸入擷取和儲存資料，並判斷要傳送的回應傳回至用戶端 （顯示 HTML、 下載檔案、 重新導向至不同URL 等等）。

既然我們已經建立基本模型 NerdDinner 應用程式，我們的下一個步驟是將控制器新增至會利用它為使用者提供的資料清單/詳細資料瀏覽體驗 Dinners 我們網站上的應用程式。

### <a name="adding-a-dinnerscontroller-controller"></a>新增 DinnersController 控制器

我們將開始在我們的 web 專案中的 「 控制項 」 資料夾上按一下滑鼠右鍵，然後選取**Add-&gt;控制器**功能表命令 （您也可以輸入 Ctrl-M、 Ctrl + C 來執行此命令）：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image1.png)

這會顯示 「 新增控制器 」 對話方塊：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image2.png)

我們將新的控制站 」 DinnersController"，然後按一下 [新增] 按鈕。 Visual Studio 將會再加入我們 \Controllers 目錄下的 DinnersController.cs 檔案：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image3.png)

它也會開啟新 DinnersController 類別程式碼編輯器。

### <a name="adding-index-and-details-action-methods-to-the-dinnerscontroller-class"></a>將 index （） 和 Details() 動作方法加入至 DinnersController 類別

我們想要讓訪客使用我們的應用程式來瀏覽即將推出的 dinners 的清單，並讓他們按一下清單中任何 Dinner，看到有關的特定詳細資料。 我們會發佈下列的 Url，從我們的應用程式來執行這項操作：

| **URL** | **目的** |
| --- | --- |
| */Dinners/* | 顯示即將推出的 dinners HTML 清單 |
| */Dinners/Details/[id]* | 顯示有關特定 dinner，內嵌於 URL-這會比對的資料庫中 dinner DinnerID 「 識別碼 」 參數所指示的詳細資料。 例如： /Dinners/Details/2 會顯示具有 Dinner DinnerID 值為 2 的詳細資料的 HTML 網頁。 |

我們將發佈這些 Url 的初始的實作加到 DinnersController 類別類似下面兩個公用 「 動作方法 」:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample1.cs)]

然後，我們將執行 NerdDinner 應用程式，並使用瀏覽器叫用它們。 在中輸入 *"Dinners / 」* URL 將會導致我們*index （)* 執行，以及它的方法會傳回下列回應：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image4.png)

在中輸入 *"/ Dinners/詳細資料/2 」* URL 將會導致我們*Details()* 方法來執行，並傳回下列回應：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image5.png)

您可能想知道-怎麼 ASP.NET MVC 知道建立我們 DinnersController 類別和叫用這些方法？ 若要了解，讓我們快速看一下路由的運作方式。

### <a name="understanding-aspnet-mvc-routing"></a>了解 ASP.NET MVC 路由

ASP.NET MVC 包含功能強大的 URL 路由引擎，提供很大的彈性，控制如何將 Url 對應至控制器類別。 它可讓我們完全自訂 ASP.NET MVC 如何選擇要建立，要在其上叫用，以及設定不同的方式，變數可以是會自動從 URL/查詢字串剖析和傳遞給方法的參數的方法的控制器類別引數。 它提供的彈性，完全最佳化 SEO （搜尋引擎最佳化） 的站台，以及發行任何我們想要從應用程式的 URL 結構。

根據預設，新的 ASP.NET MVC 專案會隨附一組預先設定的已註冊的 URL 路由規則。 這可讓我們能夠輕鬆地開始使用應用程式而不需要明確設定任何項目。 預設路由規則註冊可在我們的專案-我們可以按兩下以開啟 「 Global.asax 」 檔案的專案根目錄中的 「 應用程式 」 類別中：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image6.png)

這個類別之 「 RegisterRoutes"方法內註冊預設 ASP.NET MVC 路由規則：

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample2.cs)]

「 路由。MapRoute()"上述的方法呼叫註冊的預設路由規則將連入的 Url 對應至控制器類別使用的 URL 格式:"/ {controller} / {action} / {id}"，其中 「 控制器 」 是以具現化控制器類別的名稱"action"是的名稱公用方法來叫用，然後 「 識別碼 」 是可以做為引數傳遞給方法的 URL 中內嵌的選擇性參數。 傳遞給 「 MapRoute() 」 方法呼叫的第三個參數是一組要用於控制器/動作/識別碼值，它們不會出現在 URL 中的預設值 (控制器 ="Home"，動作 ="Index"，識別碼 ="")。

以下是 使用預設對應的資料表，將示範如何在各種不同的 Url 」<em>/ {控制器} / {action} / {id}"</em>路由規則：

| **URL** | **控制器類別** | **動作方法** | **傳遞參數** |
| --- | --- | --- | --- |
| */Dinners/Details/2* | DinnersController | Details(id) | id=2 |
| */Dinners/Edit/5* | DinnersController | Edit(id) | id=5 |
| */Dinners/Create* | DinnersController | Create （) | N/A |
| */ Dinners* | DinnersController | Index （) | N/A |
| */ 首頁* | HomeController | Index （) | N/A |
| */* | HomeController | Index （) | N/A |

最後三個資料列會顯示預設值 (控制器 = 首頁，動作 = 索引，Id ="") 所使用。 因為如果未指定，"Index"方法註冊為預設的動作名稱"/ Dinners"和"/home"Url 原因，index （） 動作方法會叫用其控制器類別。 因為"Home"控制器註冊為預設控制器中，如果未指定，"/"的 URL，則會導致要建立、 HomeController 和 index （） 動作方法以叫用。

如果您不喜歡這些預設的 URL 路由規則，好消息是，可輕鬆地變更-只要將它們設為編輯上述的 RegisterRoutes 方法內。 NerdDinner 應用程式中，不過，我們不打算變更任何預設的 URL 路由規則 – 而我們只使用它們作為-是。

### <a name="using-the-dinnerrepository-from-our-dinnerscontroller"></a>使用從我們 DinnersController DinnerRepository

我們現在將我們的 DinnersController 目前的實作與使用我們的模型實作的 index （） 和 Details() 動作方法。

我們將使用我們稍早建置來實作行為的 DinnerRepository 類別。 我們會藉由新增參照"NerdDinner.Models"命名空間中，"using"陳述式開始，然後將我們 DinnerRepository 的執行個體宣告做為欄位中，我們 DinnerController 類別上變更。

在本章稍後我們將介紹 「 相依性插入 」 的概念，並說明為了取得啟用更好的單元 DinnerRepository 參考控制器的另一種方式，測試 – 但權限現在我們將只建立我們 DinnerRepository 的執行個體下面類似內嵌。

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample3.cs)]

現在我們已經準備好產生 HTML 回應使用擷取的資料模型物件。

### <a name="using-views-with-our-controller"></a>使用我們的控制器的檢視

雖然您可以撰寫程式碼來組合 HTML，然後使用我們的動作方法內*response.write （)* helper 方法，以將它傳送回用戶端，該方法會變得相當難以快速。 更好的方法是我們只能執行應用程式和我們的 DinnersController 動作方法內的資料邏輯，並將已轉譯 HTML 回應給負責輸出 HTML 表示個別的 「 檢視 」 範本所需的資料它。 如我們稍後所見，「 檢視 」 的範本是文字檔案，其中通常包含 HTML 標記和內嵌的轉譯程式碼的組合。

我們的控制器邏輯區分開來我們檢視轉譯提供數個優點。 特別是它可協助應用程式程式碼與 UI 呈現格式化程式碼強制清除 「 關注點分離 」。 這樣更容易在隔離的單元測試應用程式邏輯與 UI 呈現邏輯。 它可讓您稍後修改 UI 轉譯範本，而不必變更應用程式程式碼的工作變得更容易。 它可以讓它更容易開發人員和設計人員共同合作專案。

我們可以更新我們 DinnersController 類別，以指出我們想要藉由變更資訊，請參閱我們的兩個動作方法的方法簽章，不需要傳回型別"void"改為具有傳回型別"ActionResult 」 傳回的 HTML UI 回應中使用檢視範本。 我們接著可以呼叫*View()* helper 方法，在控制器的基底類別傳回"ViewResult 」 物件如下所示：

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample4.cs)]

簽章*View()* 我們在上面使用的 helper 方法如下所示：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image7.png)

第一個參數*View()* helper 方法，是我們想要用來呈現 HTML 回應檢視範本檔案的名稱。 第二個參數是模型物件，其中包含所檢視範本呈現 HTML 回應所需要的資料。

我們在我們的 index （） 動作方法內呼叫*View()* helper 方法，並指出我們想要呈現 dinners 使用 「 索引 」 檢視範本的 HTML 清單。 我們要將檢視範本傳遞一連串 Dinner 物件產生的清單：

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample5.cs)]

在我們 Details() 動作方法內我們會嘗試擷取 Dinner 物件，使用提供的 URL 中的識別碼。 如果我們找到有效的 Dinner *View()* 協助程式方法，並指出我們想要使用 [詳細資料] 檢視範本來呈現擷取的 Dinner 物件。 如果要求不正確的 dinner，我們會呈現表示 Dinner 不存在使用"NotFound"檢視範本的有用的錯誤訊息 (和多載的新版*View()* helper 方法，只會使用範本名稱):

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample6.cs)]

讓我們現在實作"NotFound"、 [詳細資料] 和 [索引] 檢視範本。

### <a name="implementing-the-notfound-view-template"></a>實作"NotFound"檢視範本

我們開始要先實作"NotFound"檢視範本 – 顯示易懂的錯誤訊息，指出找不到要求的 dinner。

我們將定位文字資料指標內控制器動作方法，來建立新的檢視範本，然後以滑鼠右鍵按一下並選擇 [新增檢視] 功能表命令 （我們也可以輸入 Ctrl-M、 Ctrl-V 來執行此命令）：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image8.png)

這會顯示類似下方的 [新增檢視] 對話方塊。 根據預設，對話方塊會預先填入的檢視，以建立名稱來比對動作方法的名稱資料指標時所在的對話方塊已啟動 （在此情況下 [詳細資料]）。 因為我們想要先實作"NotFound"範本，我們將會覆寫此檢視表名稱，然後將它改為"NotFound":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image9.png)

當我們按一下 [新增] 按鈕時，Visual Studio 會內的"\Views\Dinners"目錄 （如果目錄不存在時，它也會建立） 就讓我們建立新的 「 NotFound.aspx 」 檢視範本：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image10.png)

它也會開啟新 「 NotFound.aspx 」 檢視範本程式碼編輯器：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image11.png)

預設的檢視範本有兩個 「 內容地區 」，我們可以在其中新增內容及程式碼。 第一個可讓我們用來自訂"title"的 HTML 網頁傳回。 第二個可讓我們用來自訂 「 主要內容 」 的 HTML 網頁傳回。

若要實作"NotFound"檢視範本中，我們將新增一些基本的內容：

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample7.aspx)]

我們可以再試試看在瀏覽器中。 若要這樣做讓我們來要求 *"/ Dinners/詳細資料/9999"* URL。 這會參考目前不存在於資料庫中，並會導致我們 DinnersController.Details() 動作方法，以呈現"NotFound"檢視範本 dinner:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image12.png)

您會注意到在上述螢幕擷取畫面中的一件事是 HTML 的基本的檢視範本繼承了一大堆圍繞在螢幕上的主要內容。 這是因為我們檢視範本會使用 「 主版頁面 」 範本，可讓我們在網站上的所有檢視之間套用一致的版面配置。 我們將討論主版頁面詳細說明本教學課程的後半部分工作的方式。

### <a name="implementing-the-details-view-template"></a>實作 [詳細資料] 檢視範本

讓我們現在實作 [詳細資料] 檢視範本-將單一 Dinner 模型產生 HTML。

我們將可以定位文字資料指標在詳細資料動作方法中，然後以滑鼠右鍵按一下並選擇 [新增檢視] 功能表命令 （或按 Ctrl-M、 Ctrl-V）：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image13.png)

這會顯示 [新增檢視] 對話方塊。 我們將會保留預設的檢視名稱 （[詳細資料]）。 我們也會在對話方塊中選取 [建立強型別檢視表] 核取方塊，然後選取 （使用下拉式方塊的下拉式清單中） 從控制器傳遞至檢視的模型型別名稱。 此檢視中，我們會傳遞 Dinner 物件 (此類型的完整的名稱是: 「 NerdDinner.Models.Dinner"):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image14.png)

不同於先前的範本，在我們選擇要建立 「 空的檢視 」，這次我們會自動選擇"scaffold 」 檢視使用 [詳細資料] 範本。 我們可以變更 [檢視內容] 下拉式清單中上述對話方塊來表示。

「 Scaffolding"會產生我們根據 Dinner 物件傳遞給它的詳細資料檢視範本的初始實作。 這提供簡單的方法，讓我們快速地開始使用我們的檢視範本實作。

當我們按一下 [新增] 按鈕時，Visual Studio 會在我們的 「 \Views\Dinners"目錄內就讓我們建立新的 「 Details.aspx 」 檢視範本檔案：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image15.png)

它也會開啟新 「 Details.aspx 」 檢視範本程式碼編輯器。 它會包含 Dinner 模型為基礎的詳細資料檢視的初始 scaffold 實作。 Scaffolding 引擎會使用.NET 反映來查看傳遞給它，在類別上公開的公用屬性，並將適當的內容，根據每個找到的類型：

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample8.aspx)]

我們可以要求 *"/ Dinners/詳細資料/1"* URL，以查看此"details"scaffold 實作在瀏覽器的外觀。 使用此 URL 會顯示我們以手動方式加入至資料庫時我們第一次建立 dinners 的其中一個：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image16.png)

這會啟動並執行快速，讓我們，並提供我們 Details.aspx 檢視的初始實作。 然後，我們可以移，並調整它以自訂 UI 以我們的滿意度。

當我們更仔細看看 Details.aspx 範本時，我們會發現，它包含靜態 HTML，以及內嵌轉譯程式碼。 &lt;%%&gt;程式碼區塊執行的程式碼，當檢視範本呈現，並&lt;%= %&gt;程式碼區塊執行其中所包含的程式碼，並再呈現範本的輸出資料流的結果。

我們可以撰寫程式碼中我們存取傳遞從控制器使用強型別 「 模型 」 屬性 「 Dinner"模型物件的檢視。 Visual Studio 提供我們具有完整的 intellisense 程式碼時存取這個編輯器中的 「 模型 」 屬性：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image17.png)

讓我們請少許調整，讓我們的最終詳細資料檢視範本的來源如下所示：

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample9.aspx)]

當我們存取 *"/ Dinners/詳細資料/1"* URL 一次它將會立即呈現，如下所示：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image18.png)

### <a name="implementing-the-index-view-template"></a>實作 「 索引 」 檢視範本

讓我們現在實作 「 索引 」 檢視範本 – 將會產生即將推出的 Dinners 的清單。 待辦事項這我們會在索引動作方法中，我們文字游標的位置，然後以滑鼠右鍵按一下並選擇 [新增檢視] 功能表命令 （或按 Ctrl-M、 Ctrl-V）。

在 [新增檢視] 對話方塊中，我們將保留名為"Index"的檢視範本，並選取 [建立強型別檢視] 的核取方塊。 此時，我們將選擇自動產生 「 清單 」 檢視範本，並選取 「 NerdDinner.Models.Dinner"做為模型類型傳遞至檢視 (這因為我們已指出我們要建立 「 清單 」 scaffold 會假設我們的 [新增檢視] 對話方塊傳遞一連串 Dinner 物件從控制器到檢視）：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image19.png)

當我們按一下 [新增] 按鈕時，Visual Studio 會在我們的 「 \Views\Dinners"目錄內就讓我們建立新的 「 Index.aspx 」 檢視範本檔案。 它會 「 scaffold 」 內的初始實作提供 Dinners 我們傳遞至檢視的 HTML 資料表清單。

當我們執行的應用程式和存取權 *"Dinners /"* 轉譯的 dinners 名單的 URL，就像這樣：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image20.png)

上述表格解決方案讓我們吃晚餐資料 – 這並不是很希望我們為直接面對消費者 Dinner 清單的類似方格的配置。 我們可以更新 Index.aspx 檢視範本，並修改它以列出較少的資料行的資料，並用&lt;ul&gt;来呈現它們而不是資料表，使用下列程式碼項目：

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample10.aspx)]

因為我們使用迴圈處理每個 dinner 在我們的模型，我們會使用上述的 foreach 陳述式內的 「 var 」 關鍵字。 熟悉 C# 3.0 的那些可能會認為，使用 「 var 」 表示 dinner 物件是晚期繫結。 它表示編譯器使用型別推斷對強型別 「 模型 」 的屬性 (類型"IEnumerable&lt;Dinner&gt;") 和編譯為 Dinner 類型 – 這表示我們取得完整的本機 「 dinner"變數intellisense 和編譯時期檢查程式碼區塊內：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image21.png)

當我們叫用重新整理 */Dinners*在我們的更新的檢視現在看起來像下面我們瀏覽器中的 URL:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image22.png)

這會尋找比較好，但尚未完全有。 我們最後一個步驟是讓使用者按一下清單中的個別 Dinners，請參閱相關的詳細資料。 我們將會轉譯 HTML 超連結項目連結至詳細資料動作方法，在我們 DinnersController 來實作。

在下列其中一種我們 [索引] 檢視中，我們可以產生這些超連結。 第一種是以手動方式建立 HTML &lt;&gt;項目類似下面的其中內嵌&lt;%%&gt;內封鎖&lt;&gt; HTML 項目：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image23.png)

我們可以使用替代方法是才能內建的 「 Html.ActionLink()"helper 方法，支援以程式設計方式建立 HTML 的 ASP.NET MVC 中充分&lt;&gt;連結至另一個動作方法的項目控制站：

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample11.aspx)]

Html.ActionLink() helper 方法的第一個參數是要顯示的連結文字 （在此案例中標題的 dinner），第二個參數是我們想要產生的連結 （在此案例的詳細資料的方法，） 的控制器動作名稱和第三個參數要傳送到其中的動作 （實作為具有屬性名稱/值的匿名型別） 的參數集。 在此情況下，我們會指定我們想要連結至，而且因為 ASP.NET MVC 中的預設 URL 路由規則 dinner 的 「 識別碼 」 參數"{Controller} / {Action} / {id}"Html.ActionLink() helper 方法會產生下列輸出：

[!code-html[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample12.html)]

對 Index.aspx 檢視，我們將使用 Html.ActionLink() 協助程式方法的方法，並有清單連結至適當的詳細資料的 URL 中的每個 dinner:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample13.aspx)]

而且現在當按下 */Dinners* dinner 清單如下所示的 URL:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image24.png)

當我們按一下任一清單中 Dinners 我們會瀏覽以查看其相關的詳細資料：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image25.png)

### <a name="convention-based-naming-and-the-views-directory-structure"></a>以慣例為基礎的命名和 \Views 目錄結構

預設的 ASP.NET MVC 應用程式會使用以慣例為基礎的目錄，解析檢視範本時，命名結構。 這可讓開發人員不必完整限定的位置路徑參考從控制器類別內的檢視時。 根據預設 ASP.NET MVC 會尋找檢視範本檔案中的 * \Views\[ControllerName]\*目錄應用程式下方。

比方說，我們一直在處理明確參考三個檢視範本的 DinnersController 類別 –:"Index"、"Details"和"NotFound"。 ASP.NET MVC 會預設情況下尋找這些檢視內*\Views\Dinners*目錄底下的應用程式根目錄：

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image26.png)

請注意上述方式有目前專案中的三個控制器類別 (DinnersController HomeController，AccountController – 我們在建立專案時預設便會加入這兩個)，而且有三個子目錄 （一個用於每個控制站） \Views 目錄內。

從首頁和帳戶控制器所參考的檢視將會自動解除其檢視範本，從個別*\Views\Home*並*\Views\Account*目錄。 *\Views\Shared*子目錄提供儲存在應用程式內的多個控制站都是重複使用的檢視範本的方式。 當 ASP.NET MVC 會嘗試解析檢視範本時，它首先會檢查內*\Views\[控制器]* 特定目錄中，如果找不到檢視範本那里它看起來會內*\Views\共用*目錄。

談到命名個別的檢視範本的範例，建議的指引，就是讓共用相同的名稱，做為動作方法，導致呈現的檢視範本。 例如，上面我們"Index"動作方法使用 [索引] 檢視來呈現檢視結果，而 [詳細資料] 動作的方法使用 [詳細資料] 檢視來呈現其結果。 這可讓您方便您快速查看哪一個範本是與每個動作建立關聯。

開發人員不需要時檢視範本具有相同的名稱，作為在控制器上叫用動作方法，明確指定檢視範本名稱。 我們可以改為只是模型物件傳遞給 「 View()"helper 方法 （如果沒有指定的檢視表名稱），和 ASP.NET MVC 會自動推斷我們想要使用*\Views\[ControllerName]\[ActionName]* 檢視範本呈現的磁碟上。

這可讓我們有點，清除我們的控制器程式碼，並避免重複兩次相同的程式碼名稱：

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample14.cs)]

上述程式碼是實作不錯 Dinner 清單/詳細資料所需的所有站台的體驗。

#### <a name="next-step"></a>下一個步驟

我們現在有不錯的 Dinner 瀏覽體驗。

我們現在就讓編輯支援的 CRUD （建立、 讀取、 更新、 刪除） 資料格式。

> [!div class="step-by-step"]
> [上一頁](build-a-model-with-business-rule-validations.md)
> [下一頁](provide-crud-create-read-update-delete-data-form-entry-support.md)
