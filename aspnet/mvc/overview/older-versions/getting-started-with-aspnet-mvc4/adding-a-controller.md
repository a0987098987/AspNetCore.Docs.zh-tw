---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
title: 新增控制器 |Microsoft Docs
author: Rick-Anderson
description: 注意： 本教學課程中的更新的版本就可以使用這裡使用 ASP.NET MVC 5 和 Visual Studio 2013。 這是更安全、 更容易遵循，並示範...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 0267d31c-892f-49a1-9e7a-3ae8cc12b2ca
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 100f5623bafa33548f9c979e98765c92327eb369
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37373285"
---
<a name="adding-a-controller"></a>新增控制器
====================
藉由[Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > 本教學課程中的更新的版本可[此處](../../getting-started/introduction/getting-started.md)使用 ASP.NET MVC 5 和 Visual Studio 2013。 它更安全、 更容易遵循，並示範更多的功能。


代表 MVC*模型-檢視-控制器*。 MVC 是用於開發也架構，可測試且易於維護的應用程式的模式。 MVC 架構的應用程式包含：

- **M**模型： 代表應用程式的資料，並使用驗證邏輯來強制執行商務規則，該資料的類別。
- **V** iews： 您的應用程式用來動態產生 HTML 回應的範本檔案。
- **C** ontrollers： 類別，可處理連入的瀏覽器要求，擷取模型資料，然後指定 將回應傳回給瀏覽器的檢視範本。

我們就可以涵蓋在本教學課程系列中的所有這些概念，並示範如何使用它們來建置應用程式。

讓我們開始建立控制器類別。 在 **方案總管**，以滑鼠右鍵按一下*控制站*資料夾，然後選取**新增控制器**。

![](adding-a-controller/_static/image1.png)

命名您的新控制器&quot;HelloWorldController&quot;。 保留預設範本，因為**空白 MVC 控制器**然後按一下**新增**。

![新增控制器](adding-a-controller/_static/image2.png)

請注意，在**方案總管**新的檔案已經建立的具名*HelloWorldController.cs*。 在 IDE 中開啟檔案。

![](adding-a-controller/_static/image3.png)

取代下列程式碼檔案的內容。

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

控制器方法會傳回 HTML 的字串做為範例。 名為控制器`HelloWorldController`和上述的第一個方法命名為`Index`。 讓我們叫用它從瀏覽器。 執行應用程式 （按 F5 或 ctrl+f5）。 在瀏覽器中，附加&quot;HelloWorld&quot; [網址] 列中的路徑。 (例如，在圖中，其下的`http://localhost:1234/HelloWorld.`) 的瀏覽器頁面看起來像下列螢幕擷取畫面。 在上述的方法中，程式碼會直接傳回字串。 您告訴系統只會傳回一些 HTML，且啟動成功 ！

![](adding-a-controller/_static/image4.png)

根據傳入 URL 不同控制器類別 （和不同的動作方法），會叫用 ASP.NET MVC。 使用 ASP.NET MVC 的預設值的 URL 路由邏輯會使用像這樣的格式，以判斷哪些程式碼叫用：

`/[Controller]/[ActionName]/[Parameters]`

URL 的第一個部分會判斷要執行的控制器類別。 因此 */HelloWorld*對應至`HelloWorldController`類別。 URL 的第二部分會判斷要執行的類別上的動作方法。 因此 */HelloWorld/Index*會導致`Index`方法`HelloWorldController`類別來執行。 請注意，我們只需要瀏覽至 */HelloWorld*而`Index`方法已由預設值。 這是因為方法命名為`Index`是會在控制器呼叫，如果沒有明確指定的預設方法。

瀏覽至 `http://localhost:xxxx/HelloWorld/Welcome`。 `Welcome`方法會執行，且會傳回字串&quot;這是 歡迎使用動作方法...&quot;. 預設的 MVC 對應是`/[Controller]/[ActionName]/[Parameters]`。 在此 URL 中，控制器是 `HelloWorld`，而 `Welcome` 是動作方法。 您尚未使用 URL 的 `[Parameters]` 部分。

![](adding-a-controller/_static/image5.png)

讓我們在範例稍微修改，讓您可以將從 URL 的某些參數資訊傳遞到控制器 (例如 */HelloWorld/歡迎？ 名稱 = Scott&amp;numtimes = 4*)。 變更您`Welcome`方法以包含兩個參數，如下所示。 請注意，程式碼會使用 C# 選擇性參數功能，在表示`numTimes`參數應該預設為 1，如果沒有傳遞任何值，該參數。

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

執行應用程式，並瀏覽至範例 URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`。 您可以嘗試不同的值，如`name`和`numtimes`在 URL 中。 [ASP.NET MVC 模型繫結系統](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)會自動將對應方法中的參數從查詢字串，在網址列中的具名的參數。

![](adding-a-controller/_static/image6.png)

在這些範例中兩個已執行控制器&quot;VC&quot; MVC 部分 — 也就是檢視和控制器工作。 控制器會直接傳回 HTML。 通常您不希望控制器直接傳回 HTML，因為，變得很麻煩的程式碼。 而是我們通常會使用不同的檢視範本檔案協助產生 HTML 回應。 讓我們看如何能夠在下一步。

> [!div class="step-by-step"]
> [上一頁](intro-to-aspnet-mvc-4.md)
> [下一頁](adding-a-view.md)
