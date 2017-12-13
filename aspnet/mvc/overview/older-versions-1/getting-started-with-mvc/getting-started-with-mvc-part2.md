---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
title: "加入控制器 |Microsoft 文件"
author: shanselman
description: "本教學課程是否可在此處使用 Visual Studio 2013 更新的版本。 新的教學課程會使用 ASP.NET MVC 5 提供許多改良 t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: ff03dcc0-da97-458d-838f-0823e7482642
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
msc.type: authoredcontent
ms.openlocfilehash: 93a362cf83d39b29fcba3f2dee0c28257805a89e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-controller"></a>新增控制器
====================
由[Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > 如果本教學課程中可用的更新的版本[這裡](../../getting-started/introduction/getting-started.md)使用[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)。 新的教學課程會使用 ASP.NET MVC 5，本教學課程提供許多改良。
> 
> 
> 這是初學者教學課程介紹基本概念的 ASP.NET MVC。 您將建立簡單的 web 應用程式可讀取和寫入資料庫中。 請瀏覽[ASP.NET MVC 學習中心](../../../index.md)教學課程和範例，請尋找其他 ASP.NET MVC。


MVC 代表模型、 檢視、 控制站。 MVC 是用於開發應用程式，使每個部分都有不同於另一個有責任的模式。

- 您的應用程式的資料模型：
- 檢視： 您的應用程式將用來動態產生 HTML 回應範本檔案。
- 控制站： 類別，處理內送的 URL 要求，應用程式，可擷取模型資料，然後指定 將回應傳回給用戶端呈現的檢視範本

我們將會在本教學課程涵蓋所有這些概念，並示範如何使用它們來建置應用程式。

以滑鼠右鍵按一下方案總管中的 控制器 資料夾，然後選取 加入控制器，讓我們來建立新的控制站。

[![AddControllerRightClick](getting-started-with-mvc-part2/_static/image2.png)](getting-started-with-mvc-part2/_static/image1.png)

命名您的新控制器"HelloWorldController"，並按一下 [新增]。

[![加入控制器 對話方塊](getting-started-with-mvc-part2/_static/image4.png)](getting-started-with-mvc-part2/_static/image3.png)

請注意，在 [方案總管]，右邊已為您呼叫 HelloWorldController.cs 建立新的檔案，該檔案現在會在中開啟**IDE**。

[![HelloWorldControllerCode](getting-started-with-mvc-part2/_static/image6.png)](getting-started-with-mvc-part2/_static/image5.png)

建立新公用類別 HelloWorldController 內看起來像這樣的兩個新方法。 做為範例，我們將直接從 controller 傳回 HTML 的字串。

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample1.cs)]

將控制器命名為 HelloWorldController，而且您的新方法呼叫索引。 您再次執行應用程式，只要如常 （按一下 [開始] 按鈕或按 f5 鍵，若要這樣做）。 一旦您的瀏覽器已啟動，變更 網址列中的路徑`http://localhost:xx/HelloWorld`其中 xx 是任意數字您的電腦已選擇。 現在您的瀏覽器應該看起來像下列螢幕擷取畫面。 在我們上述方法，我們傳回字串傳遞給方法，稱為 [內容]。 我們會告訴系統只會傳回某些 HTML，且啟動成功 ！

根據傳入的 URL 不同控制器類別 （和其中的不同動作方法），會叫用 ASP.NET MVC。 使用 ASP.NET MVC 的預設對應邏輯來控制哪些程式碼會執行使用的格式如下：

/ [控制器] / [ActionName] / [參數]

URL 的第一個部分會決定要執行的控制器類別。 因此 /HelloWorld 會對應至 HelloWorldController 類別。 URL 的第二個部分會決定要執行的類別上的動作方法。 /HelloWorld/Index 會導致要執行的 HelloWorldcontroller 類別的 index （） 方法。 請注意，我們只需要瀏覽 /HelloWorld 上述和索引所隱含的方法。 這是因為名為"Index"的方法是將會在控制站呼叫，如果沒有明確指定的預設方法。

[![這是我的預設動作](getting-started-with-mvc-part2/_static/image8.png)](getting-started-with-mvc-part2/_static/image7.png)

現在，讓我們來瀏覽`http://localhost:xx/HelloWorld/Welcome.`現在我們褖畫惎方法在執行，並傳回其 HTML 字串。

同樣地，/ [控制器] / [ActionName] / [參數] 因此控制器 HelloWorld 而且歡迎畫面在此情況下是方法。 我們還參數。

[![這是該褖畫惎動作方法](getting-started-with-mvc-part2/_static/image10.png)](getting-started-with-mvc-part2/_static/image9.png)

讓我們的範例稍微修改，讓我們可以從 URL 傳遞中的某些資訊至我們的控制站，例如就像這樣: / HelloWorld/歡迎畫面？ name = Scott&amp;numtimes = 4。 變更您褖畫惎方法，將包含兩個參數和它類似下面的的更新。 請注意，我們使用 C# 選擇性參數功能來指示參數 numTimes 應該預設為 1，是否它不會傳遞中。

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample2.cs)]

執行您的應用程式，請瀏覽`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`任意變更名稱和 numtimes 的值。 系統會自動對應具名的參數從您在網址列中的查詢字串參數，在您的方法。

在這些範例中這兩個控制器已執行的所有工作，並具有已傳回 HTML 直接。 通常，我們不希望我們控制站直接-傳回 HTML，因為，最後會在程式碼很麻煩。 改為我們通常會使用不同的檢視範本檔案協助產生 HTML 回應。 讓我們看看如何我們可以執行這項操作。 關閉瀏覽器，並返回 IDE。

>[!div class="step-by-step"]
[上一頁](getting-started-with-mvc-part1.md)
[下一頁](getting-started-with-mvc-part3.md)
