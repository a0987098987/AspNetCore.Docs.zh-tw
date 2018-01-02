---
uid: mvc/overview/getting-started/introduction/adding-a-controller
title: "加入控制器 |Microsoft 文件"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: cc764f3b-6921-486a-8f44-c6ccd1249acd
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 878d957344a08450b82b0249d8ca2a205810da4a
ms.sourcegitcommit: 9ecd4e9fb0c40c3693dab079eab1ff94b461c922
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/28/2017
---
<a name="adding-a-controller"></a>新增控制器
====================
由[Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

代表 MVC*模型-檢視-控制器*。 MVC 是用於開發架構，可測試且容易維護的應用程式的模式。 MVC 架構的應用程式包含：

- **M** odels： 類別，表示應用程式的資料，以及可使用的驗證邏輯將強制執行商務規則，該資料。
- **V** iews： 您的應用程式用來動態產生 HTML 回應的範本檔案。
- **C** ontrollers： 處理內送的瀏覽器要求類別擷取模型資料，然後指定 將回應傳回至瀏覽器的檢視範本。

我們將會涵蓋在本教學課程系列中的所有這些概念，並示範如何使用它們來建置應用程式。

> [!NOTE]
> 預設的 MVC 在先前步驟中選取範本。 這會建立主資料夾、 帳戶和預設的管理控制站。

讓我們先建立控制器類別。 在**方案總管 中**，以滑鼠右鍵按一下*控制器*資料夾，然後按一下**新增**，然後**控制器**。


![](adding-a-controller/_static/image1.png)

在**加入 Scaffold**對話方塊中，按一下  **MVC 5 控制器空白**，然後按一下 **新增**。

![](adding-a-controller/_static/image2.png)  
 

將新的控制站 」 HelloWorldController"，然後按一下**新增**。

![加入控制器](adding-a-controller/_static/image3.png)

請注意，在**方案總管 中**新檔案已經建立的具名*HelloWorldController.cs*和新的資料夾*Views\HelloWorld*。 控制器是在 IDE 中開啟。

![](adding-a-controller/_static/image4.png)

取代下列程式碼檔案的內容。

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

控制器方法會傳回 HTML 的字串做為範例。 控制站命名為`HelloWorldController`第一種方法稱為`Index`。 讓我們來叫用它從瀏覽器。 執行應用程式 （按 F5 或 Ctrl + F5）。 在瀏覽器中附加&quot;HelloWorld&quot;網址列中的路徑。 (例如，在圖例中，其下的`http://localhost:1234/HelloWorld.`) 網頁瀏覽器中的看起來像下列螢幕擷取畫面。 上述的方法，在程式碼會直接傳回字串。 告訴系統只傳回一些 HTML，且啟動成功 ！

![](adding-a-controller/_static/image5.png)

根據傳入的 URL 不同控制器類別 （和不同的動作方法內），會叫用 ASP.NET MVC。 使用 ASP.NET MVC 的預設的 URL 路由邏輯來判斷哪些程式碼叫用使用的格式如下：

`/[Controller]/[ActionName]/[Parameters]`

您設定的格式中的路由*應用程式\_Start/RouteConfig.cs*檔案。

[!code-csharp[Main](adding-a-controller/samples/sample2.cs?highlight=7-8)]

當您執行應用程式，並不提供任何 URL 區段時，其預設值為"Home"控制器和 「 索引 」 動作方法的區段中指定預設值的上述程式碼。

URL 的第一個部分會決定要執行的控制器類別。 因此*/HelloWorld*對應至`HelloWorldController`類別。 URL 的第二個部分會決定要執行的類別上的動作方法。 因此*/HelloWorld/索引*導致`Index`方法`HelloWorldController`類別來執行。 請注意，我們只需要瀏覽至*/HelloWorld*和`Index`方法使用預設值。 這是因為方法名為`Index`是將會在控制站呼叫，如果沒有明確指定的預設方法。 URL 區段的第三個部分 (`Parameters`) 是路由資料。 我們會在本教學課程稍後看到路由資料。

瀏覽至 `http://localhost:xxxx/HelloWorld/Welcome`。 `Welcome`方法會執行，且會傳回字串&quot;這是該褖畫惎動作方法...&quot;. 預設的 MVC 對應是否`/[Controller]/[ActionName]/[Parameters]`。 在此 URL 中，控制器是 `HelloWorld`，而 `Welcome` 是動作方法。 您尚未使用 URL 的 `[Parameters]` 部分。

![](adding-a-controller/_static/image6.png)

讓我們在範例稍微修改，讓您可以將從 URL 的某些參數資訊傳遞至控制器 (例如， */HelloWorld/歡迎畫面？ 名稱 = Scott&amp;numtimes = 4*)。 變更您`Welcome`方法，將包含兩個參數，如下所示。 請注意，程式碼會使用 C# 選擇性參數功能表示`numTimes`參數應該預設為 1，如果該參數不傳遞任何值。

[!code-csharp[Main](adding-a-controller/samples/sample3.cs)]

> [!NOTE]
> 安全性注意事項： 上方的程式碼會使用[HttpUtility.HtmlEncode](https://msdn.microsoft.com/en-us/library/ee360286(v=vs.110).aspx)為了防止惡意的輸入 (也就是 JavaScript) 的應用程式。 如需詳細資訊，請參閱[How to： 保護對指令碼會利用 Web 應用程式中藉由套用 HTML 編碼字串](https://msdn.microsoft.com/en-us/library/a2a4yykt(v=vs.100).aspx)。


 執行您的應用程式，並瀏覽至範例 URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`)。 您可以嘗試不同的值`name`和`numtimes`在 URL 中。 [ASP.NET MVC 模型繫結系統](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)會自動對應 網址列中的查詢字串中您的方法參數的具名的參數。

![](adding-a-controller/_static/image7.png)

在範例中，上述的 URL 區段 ( `Parameters`) 不使用`name`和`numTimes`做為參數傳遞[查詢字串](http://en.wikipedia.org/wiki/Query_string)。 ? （問號） 上述 URL 中是分隔符號，並遵循查詢字串。 &amp; 字元可分隔查詢字串。

下列程式碼取代該褖畫惎方法：

[!code-csharp[Main](adding-a-controller/samples/sample4.cs)]

執行應用程式，並輸入下列 URL:`http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`

![](adding-a-controller/_static/image8.png)

此時第三個 URL 區段的比對路由參數`ID.``Welcome`動作的方法包含參數 (`ID`) 符合中的 URL 規格`RegisterRoutes`方法。

[!code-csharp[Main](adding-a-controller/samples/sample5.cs?highlight=7)]

在 ASP.NET MVC 應用程式更通常會傳入做為路由資料 （如同我們上述識別碼） 比查詢字串的形式傳遞這些參數。 您也可以新增的路由將兩者`name`和`numtimes`參數做為 URL 中的路由資料中。 在*應用程式\_Start\RouteConfig.cs*檔案中加入"Hello"路由：

[!code-csharp[Main](adding-a-controller/samples/sample6.cs?highlight=13-16)]

執行應用程式，並瀏覽至`/localhost:XXX/HelloWorld/Welcome/Scott/3`。

![](adding-a-controller/_static/image9.png)

對於許多的 MVC 應用程式的預設路由會正常運作。 您將學習稍後在本教學課程，將使用模型繫結器的資料，就不需要修改該預設路由。

這些範例中已執行控制器&quot;VC&quot; MVC 部分 — 也就是，則檢視和控制器的工作。 控制器直接傳回 HTML。 通常您不想直接傳回 HTML，因為變得很麻煩的程式碼的控制站。 改為我們通常會使用不同的檢視範本檔案協助產生 HTML 回應。 讓我們來看我們如何執行這在下一步。

>[!div class="step-by-step"]
[上一頁](getting-started.md)
[下一頁](adding-a-view.md)
