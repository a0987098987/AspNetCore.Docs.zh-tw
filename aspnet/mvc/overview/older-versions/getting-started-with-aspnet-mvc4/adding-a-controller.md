---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
title: "加入控制器 |Microsoft 文件"
author: Rick-Anderson
description: "注意： 本教學課程中的更新的版本這裡會提供使用 ASP.NET MVC 5 和 Visual Studio 2013。 這是更安全、 容易遵循，以及示範..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 0267d31c-892f-49a1-9e7a-3ae8cc12b2ca
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 69af91401e51470fbc0b67103345325201b06723
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-controller"></a>新增控制器
====================
由[Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > 本教學課程的更新的版本時使用[這裡](../../getting-started/introduction/getting-started.md)使用 ASP.NET MVC 5 和 Visual Studio 2013。 它更安全、 容易遵循，及示範更多的功能。


代表 MVC*模型-檢視-控制器*。 MVC 是用於開發架構，可測試且容易維護的應用程式的模式。 MVC 架構的應用程式包含：

- **M** odels： 類別，表示應用程式的資料，以及可使用的驗證邏輯將強制執行商務規則，該資料。
- **V** iews： 您的應用程式用來動態產生 HTML 回應的範本檔案。
- **C** ontrollers： 處理內送的瀏覽器要求類別擷取模型資料，然後指定 將回應傳回至瀏覽器的檢視範本。

我們將會涵蓋在本教學課程系列中的所有這些概念，並示範如何使用它們來建置應用程式。

讓我們先建立控制器類別。 在**方案總管 中**，以滑鼠右鍵按一下*控制器*資料夾，然後選取**加入控制器**。

![](adding-a-controller/_static/image1.png)

命名新的控制站&quot;HelloWorldController&quot;。 保留為預設範本**空白的 MVC 控制器**按一下**新增**。

![加入控制器](adding-a-controller/_static/image2.png)

請注意，在**方案總管 中**新檔案已經建立的具名*HelloWorldController.cs*。 已在 IDE 中開啟的檔案。

![](adding-a-controller/_static/image3.png)

取代下列程式碼檔案的內容。

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

控制器方法會傳回 HTML 的字串做為範例。 控制站命名為`HelloWorldController`上述第一種方法稱為`Index`。 讓我們來叫用它從瀏覽器。 執行應用程式 （按 F5 或 Ctrl + F5）。 在瀏覽器中附加&quot;HelloWorld&quot;網址列中的路徑。 (例如，在圖例中，其下的`http://localhost:1234/HelloWorld.`) 網頁瀏覽器中的看起來像下列螢幕擷取畫面。 上述的方法，在程式碼會直接傳回字串。 告訴系統只傳回一些 HTML，且啟動成功 ！

![](adding-a-controller/_static/image4.png)

根據傳入的 URL 不同控制器類別 （和不同的動作方法內），會叫用 ASP.NET MVC。 使用 ASP.NET MVC 的預設的 URL 路由邏輯來判斷哪些程式碼叫用使用的格式如下：

`/[Controller]/[ActionName]/[Parameters]`

URL 的第一個部分會決定要執行的控制器類別。 因此*/HelloWorld*對應至`HelloWorldController`類別。 URL 的第二個部分會決定要執行的類別上的動作方法。 因此*/HelloWorld/索引*導致`Index`方法`HelloWorldController`類別來執行。 請注意，我們只需要瀏覽至*/HelloWorld*和`Index`方法使用預設值。 這是因為方法名為`Index`是將會在控制站呼叫，如果沒有明確指定的預設方法。

瀏覽至 `http://localhost:xxxx/HelloWorld/Welcome`。 `Welcome`方法會執行，且會傳回字串&quot;這是該褖畫惎動作方法...&quot;. 預設的 MVC 對應是否`/[Controller]/[ActionName]/[Parameters]`。 在此 URL 中，控制器是 `HelloWorld`，而 `Welcome` 是動作方法。 您尚未使用 URL 的 `[Parameters]` 部分。

![](adding-a-controller/_static/image5.png)

讓我們在範例稍微修改，讓您可以將從 URL 的某些參數資訊傳遞至控制器 (例如， */HelloWorld/歡迎畫面？ 名稱 = Scott&amp;numtimes = 4*)。 變更您`Welcome`方法，將包含兩個參數，如下所示。 請注意，程式碼會使用 C# 選擇性參數功能表示`numTimes`參數應該預設為 1，如果該參數不傳遞任何值。

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

執行您的應用程式，並瀏覽至範例 URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`。 您可以嘗試不同的值`name`和`numtimes`在 URL 中。 [ASP.NET MVC 模型繫結系統](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)會自動對應 網址列中的查詢字串中您的方法參數的具名的參數。

![](adding-a-controller/_static/image6.png)

在這些範例中這兩個已執行控制器&quot;VC&quot; MVC 部分 — 也就是，則檢視和控制器的工作。 控制器直接傳回 HTML。 通常您不想直接傳回 HTML，因為變得很麻煩的程式碼的控制站。 改為我們通常會使用不同的檢視範本檔案協助產生 HTML 回應。 讓我們來看我們如何執行這在下一步。

>[!div class="step-by-step"]
[上一頁](intro-to-aspnet-mvc-4.md)
[下一頁](adding-a-view.md)
