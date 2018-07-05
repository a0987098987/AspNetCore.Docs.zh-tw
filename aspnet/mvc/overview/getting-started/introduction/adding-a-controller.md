---
uid: mvc/overview/getting-started/introduction/adding-a-controller
title: 新增控制器 |Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 10/17/2013
ms.assetid: cc764f3b-6921-486a-8f44-c6ccd1249acd
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 77389bfa4774857eb2a607b0a70e982826312e03
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802288"
---
<a name="adding-a-controller"></a>新增控制器
====================
藉由[Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

代表 MVC*模型-檢視-控制器*。 MVC 是用於開發也架構，可測試且易於維護的應用程式的模式。 MVC 架構的應用程式包含：

- **M**模型： 代表應用程式的資料，並使用驗證邏輯來強制執行商務規則，該資料的類別。
- **V** iews： 您的應用程式用來動態產生 HTML 回應的範本檔案。
- **C** ontrollers： 類別，可處理連入的瀏覽器要求，擷取模型資料，然後指定 將回應傳回給瀏覽器的檢視範本。

我們就可以涵蓋在本教學課程系列中的所有這些概念，並示範如何使用它們來建置應用程式。

> [!NOTE]
> 預設的 MVC 上一個步驟中選取範本。 這會建立主資料夾]、 帳戶和預設的 [管理控制器。

讓我們開始建立控制器類別。 在 [**方案總管] 中**，以滑鼠右鍵按一下*控制站*資料夾，然後按一下**新增**，然後**控制器**。


![](adding-a-controller/_static/image1.png)

在 [**新增 Scaffold** ] 對話方塊中，按一下**MVC 5 控制器-空白**，然後按一下**新增**。

![](adding-a-controller/_static/image2.png)  
 

將新的控制器命名為"HelloWorldController 」，按一下**新增**。

![新增控制器](adding-a-controller/_static/image3.png)

請注意，在**方案總管] 中**新的檔案已經建立的具名*HelloWorldController.cs*和 [新資料夾*Views\HelloWorld*。 控制器是在 IDE 中開啟。

![](adding-a-controller/_static/image4.png)

取代下列程式碼檔案的內容。

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

控制器方法會傳回 HTML 的字串做為範例。 名為控制器`HelloWorldController`第一種方法稱為`Index`。 讓我們叫用它從瀏覽器。 執行應用程式 （按 F5 或 ctrl+f5）。 在瀏覽器中，附加&quot;HelloWorld&quot; [網址] 列中的路徑。 (例如，在圖中，其下的`http://localhost:1234/HelloWorld.`) 的瀏覽器頁面看起來像下列螢幕擷取畫面。 在上述的方法中，程式碼會直接傳回字串。 您告訴系統只會傳回一些 HTML，且啟動成功 ！

![](adding-a-controller/_static/image5.png)

根據傳入 URL 不同控制器類別 （和不同的動作方法），會叫用 ASP.NET MVC。 使用 ASP.NET MVC 的預設值的 URL 路由邏輯會使用像這樣的格式，以判斷哪些程式碼叫用：

`/[Controller]/[ActionName]/[Parameters]`

您設定的格式中的路由*應用程式\_Start/RouteConfig.cs*檔案。

[!code-csharp[Main](adding-a-controller/samples/sample2.cs?highlight=7-8)]

當您執行應用程式，並不提供任何 URL 區段時，則會預設為"Home"控制器和"Index"動作方法的區段中指定預設值的上述程式碼。

URL 的第一個部分會判斷要執行的控制器類別。 因此 */HelloWorld*對應至`HelloWorldController`類別。 URL 的第二部分會判斷要執行的類別上的動作方法。 因此 */HelloWorld/Index*會導致`Index`方法`HelloWorldController`類別來執行。 請注意，我們只需要瀏覽至 */HelloWorld*而`Index`方法已由預設值。 這是因為方法命名為`Index`是會在控制器呼叫，如果沒有明確指定的預設方法。 URL 區段的第三個部分 (`Parameters`) 是路由資料。 我們將在本教學課程中，於稍後看到路由資料。

瀏覽至 `http://localhost:xxxx/HelloWorld/Welcome`。 `Welcome`方法會執行，且會傳回字串&quot;這是 歡迎使用動作方法...&quot;. 預設的 MVC 對應是`/[Controller]/[ActionName]/[Parameters]`。 在此 URL 中，控制器是 `HelloWorld`，而 `Welcome` 是動作方法。 您尚未使用 URL 的 `[Parameters]` 部分。

![](adding-a-controller/_static/image6.png)

讓我們在範例稍微修改，讓您可以將從 URL 的某些參數資訊傳遞到控制器 (例如 */HelloWorld/歡迎？ 名稱 = Scott&amp;numtimes = 4*)。 變更您`Welcome`方法以包含兩個參數，如下所示。 請注意，程式碼會使用 C# 選擇性參數功能，在表示`numTimes`參數應該預設為 1，如果沒有傳遞任何值，該參數。

[!code-csharp[Main](adding-a-controller/samples/sample3.cs)]

> [!NOTE]
> 安全性注意事項： 上述程式碼會使用[HttpUtility.HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx)來保護應用程式免於遭受惡意輸入 (也就是 JavaScript)。 如需詳細資訊，請參閱[如何： 保護對指令碼會利用在 Web 應用程式中藉由套用 HTML 編碼字串](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx)。


 執行應用程式，並瀏覽至範例 URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`)。 您可以嘗試不同的值，如`name`和`numtimes`在 URL 中。 [ASP.NET MVC 模型繫結系統](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)會自動將對應方法中的參數從查詢字串，在網址列中的具名的參數。

![](adding-a-controller/_static/image7.png)

在範例中，上述的 URL 區段 ( `Parameters`) 未使用，`name`並`numTimes`參數會當做傳遞[查詢字串](http://en.wikipedia.org/wiki/Query_string)。 ? （問號） 上述 URL 中是分隔符號，並查詢字串遵循。 &amp; 字元可分隔查詢字串。

取代下列程式碼中的 歡迎使用方法：

[!code-csharp[Main](adding-a-controller/samples/sample4.cs)]

執行應用程式，並輸入下列 URL: `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`

![](adding-a-controller/_static/image8.png)

這次的第三個 URL 區段符合路由參數`ID.``Welcome`動作方法中包含的參數 (`ID`)，比對中的 URL 規格`RegisterRoutes`方法。

[!code-csharp[Main](adding-a-controller/samples/sample5.cs?highlight=7)]

在 ASP.NET MVC 應用程式，通常會更傳入作為路由資料 （如同上述的識別碼） 比將它們傳遞作為查詢字串參數。 您也可以加入的路由將兩者`name`和`numtimes`參數做為 URL 中的路由資料中。 在 *應用程式\_Start\RouteConfig.cs*檔案中，新增"Hello"路由：

[!code-csharp[Main](adding-a-controller/samples/sample6.cs?highlight=13-16)]

執行應用程式，並瀏覽至`/localhost:XXX/HelloWorld/Welcome/Scott/3`。

![](adding-a-controller/_static/image9.png)

對於許多的 MVC 應用程式，預設路由可正常運作。 稍後在本教學課程中將使用模型繫結器的資料，您將學習，您不需要修改預設路由。

在這些範例中已執行控制器&quot;VC&quot; MVC 部分 — 也就是檢視和控制器工作。 控制器會直接傳回 HTML。 通常您不希望控制器直接傳回 HTML，因為，變得很麻煩的程式碼。 而是我們通常會使用不同的檢視範本檔案協助產生 HTML 回應。 讓我們看如何能夠在下一步。

> [!div class="step-by-step"]
> [上一頁](getting-started.md)
> [下一頁](adding-a-view.md)
