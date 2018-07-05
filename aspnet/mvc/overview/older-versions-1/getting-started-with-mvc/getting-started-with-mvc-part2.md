---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
title: 新增控制器 |Microsoft Docs
author: shanselman
description: 如果本教學課程，可在此處使用 Visual Studio 2013 更新的版本。 新的教學課程會使用 ASP.NET MVC 5，可提供許多增強功能，透過 t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: ff03dcc0-da97-458d-838f-0823e7482642
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
msc.type: authoredcontent
ms.openlocfilehash: f20889cf1c1cd9a2a69f0008d879b518c38334d6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395665"
---
<a name="adding-a-controller"></a>新增控制器
====================
藉由[Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > 如果本教學課程中可用的更新的版本[此處](../../getting-started/introduction/getting-started.md)使用[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)。 新的教學課程會使用 ASP.NET MVC 5，透過本教學課程提供許多增強功能。
> 
> 
> 這是初學者教學課程中，將會介紹 ASP.NET MVC 的基本概念。 您將建立簡單 web 應用程式，從資料庫讀取與寫入。 請瀏覽[ASP.NET MVC 學習中心](../../../index.md)來尋找其他 ASP.NET MVC 教學課程和範例。


MVC 代表模型、 檢視、 控制器。 MVC 是開發應用程式，使每個部分都有責任從另一個不同的模式。

- 您的應用程式的資料模型：
- 檢視： 您的應用程式會使用動態產生 HTML 回應到範本檔案。
- 控制站： 類別，可處理傳入的 URL 要求，應用程式、 擷取模型資料，然後指定 將回應傳回給用戶端呈現的檢視範本

我們就可以在本教學課程涵蓋所有這些概念，並示範如何使用它們來建置應用程式。

以滑鼠右鍵按一下方案總管中的 控制器 資料夾，然後選取 新增控制器，讓我們來建立新的控制器。

[![AddControllerRightClick](getting-started-with-mvc-part2/_static/image2.png)](getting-started-with-mvc-part2/_static/image1.png)

命名您新的控制站 」 HelloWorldController"，並按一下 [新增]。

[![新增控制器 對話方塊](getting-started-with-mvc-part2/_static/image4.png)](getting-started-with-mvc-part2/_static/image3.png)

請注意，在右邊稱為 HelloWorldController.cs 您已建立新的檔案，該檔案現在會在中開啟 [方案總管] **IDE**。

[![HelloWorldControllerCode](getting-started-with-mvc-part2/_static/image6.png)](getting-started-with-mvc-part2/_static/image5.png)

建立新公用類別 HelloWorldController 內看起來像這樣的兩個新方法。 我們會直接從控制器傳回 HTML 的字串，做為範例。

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample1.cs)]

您的控制器命名為 HelloWorldController 和新方法，稱為索引。 您再次執行應用程式，如同先前一樣 （按一下 [播放] 按鈕或按下 f5 鍵，若要這樣做）。 一旦您的瀏覽器已啟動，變更在網址列中的路徑`http://localhost:xx/HelloWorld`其中 xx 是任何數字您的電腦已選擇。 現在您的瀏覽器看起來應該像下列螢幕擷取畫面。 在上述我們方法我們傳回的字串傳遞至方法，稱為 [內容]。 我們只告訴系統只會傳回一些 HTML，且啟動成功 ！

根據傳入 URL 不同的控制器類別 （和其中的不同動作方法），會叫用 ASP.NET MVC。 使用 ASP.NET MVC 的預設對應邏輯來控制哪些程式碼會執行使用的格式如下：

/[Controller]/[ActionName]/[Parameters]

URL 的第一個部分會判斷要執行的控制器類別。 因此 /HelloWorld 會對應至 HelloWorldController 類別。 URL 的第二部分會判斷要執行的類別上的動作方法。 /HelloWorld/Index 會導致要執行的 HelloWorldcontroller 類別的 index （） 方法。 請注意，我們只有造訪上述 /HelloWorld 和索引隱含的方法。 這是因為名為"Index"的方法是將在控制器呼叫，如果沒有明確指定的預設方法。

[![這是我的預設動作](getting-started-with-mvc-part2/_static/image8.png)](getting-started-with-mvc-part2/_static/image7.png)

現在，讓我們來瞧`http://localhost:xx/HelloWorld/Welcome.`現在在執行我們歡迎畫面的方法，並將其傳回它的 HTML 字串。

同樣地，/ [Controller] / [ActionName] / [Parameters] 讓控制器是 HelloWorld 和歡迎畫面在此情況下是方法。 我們還參數。

[![這是 歡迎使用動作方法](getting-started-with-mvc-part2/_static/image10.png)](getting-started-with-mvc-part2/_static/image9.png)

讓我們的範例中稍微修改，讓我們也可以將部分資訊從 URL 中，傳遞至我們的控制器，例如像這樣: / HelloWorld/歡迎？ 名稱 = Scott&amp;numtimes = 4。 變更您的 歡迎使用方法，以包含兩個參數並更新其如下所示。 請注意，我們使用 C# 選擇性參數功能，表示參數 numTimes 應該預設為 1，是否它不傳入。

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample2.cs)]

執行應用程式，並瀏覽`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`任意變更名稱和 numtimes 的值。 系統會自動對應方法中的參數從您的查詢字串，在網址列中的具名的參數。

在這些範例中兩個控制器已執行所有的工作，並具有已直接傳回 HTML。 通常我們不希望我們控制站直接-傳回 HTML，因為結束程式碼很麻煩。 而是我們通常會使用不同的檢視範本檔案協助產生 HTML 回應。 讓我們看看如何我們可以執行這項操作。 關閉瀏覽器，並返回 IDE。

> [!div class="step-by-step"]
> [上一頁](getting-started-with-mvc-part1.md)
> [下一頁](getting-started-with-mvc-part3.md)
