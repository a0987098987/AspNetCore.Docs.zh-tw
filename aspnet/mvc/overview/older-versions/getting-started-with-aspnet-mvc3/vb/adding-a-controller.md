---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
title: 加入控制器 (VB) |Microsoft 文件
author: Rick-Anderson
description: 本教學課程將告訴您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，也就是 ASP.NET MVC Web 應用程式的基本概念...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 741259e1-54ac-4f71-b4e8-2bd5560bb950
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 9a433083c31c7929f7599e52800c887f301d7727
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-controller-vb"></a>加入控制器 (VB)
====================
由[Rick Anderson](https://github.com/Rick-Anderson)

> 本教學課程將告訴您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，這是 Microsoft Visual Studio 的免費版本的 ASP.NET MVC Web 應用程式的基本概念。 開始之前，請確定您已安裝下面所列的必要條件。 您可以安裝全部都按下列連結： [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。 或者，您可以個別安裝的必要條件，使用下列連結：
> 
> - [Visual Studio Web Developer Express SP1 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（執行階段 + 工具支援）
> 
> 如果您使用 Visual Studio 2010 而不 Visual Web Developer 2010，安裝必要元件，請按一下下列連結： [Visual Studio 2010 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。
> 
> 使用本主題隨附在 Visual Web Developer 專案中的使用 VB.NET 原始程式碼。 [下載 VB.NET 版本](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)。 如果您偏好 C#，切換至[C# 版本](../cs/adding-a-controller.md)本教學課程。


代表 MVC*模型-檢視-控制器*。 MVC 是一種模式來開發應用程式，使每個部分都有個別的責任：

- 模型： 您的應用程式資料。
- 檢視： 您的應用程式將用來動態產生 HTML 回應範本檔案。
- 控制站： 類別，處理內送的 URL 要求，應用程式，可擷取模型資料，然後指定 呈現給用戶端的回應的檢視範本。

我們將會在本教學課程涵蓋所有這些概念，並示範如何使用它們來建置應用程式。

建立新的控制，以滑鼠右鍵按一下*控制器*資料夾中的**方案總管] 中**，然後選取 [**加入控制器**。

[![AddController](adding-a-controller/_static/image2.png "AddController")](adding-a-controller/_static/image1.png)

命名新的控制站&quot;HelloWorldController&quot;按一下**新增**。

[![2AddEmptyController](adding-a-controller/_static/image4.png "2AddEmptyController")](adding-a-controller/_static/image3.png)

請注意，在**方案總管 中**右邊確認已為您建立新的檔案命名為*HelloWorldController.cs* ，檔案已在 IDE 中開啟。

在新`public class HelloWorldController`封鎖，請建立兩個新方法看起來像下列程式碼。 我們會直接從控制器中傳回 HTML 的字串，做為範例。

[!code-vb[Main](adding-a-controller/samples/sample1.vb)]

將控制器命名為`HelloWorldController`，為您的新方法`Index`。 執行應用程式 （按 F5 或 Ctrl + F5）。 一旦您的瀏覽器開始，附加&quot;HelloWorld&quot;網址列中的路徑。 (我的電腦上有`http://localhost:43246/HelloWorld`) 您的瀏覽器看起來像下列螢幕擷取畫面。 上述的方法，在程式碼會直接傳回字串。 我們會告訴系統只傳回一些 HTML，且啟動成功 ！

![](adding-a-controller/_static/image5.png)

根據傳入的 URL 不同控制器類別 （和不同的動作方法內），會叫用 ASP.NET MVC。 使用 ASP.NET MVC 的預設對應邏輯來控制哪些程式碼會叫用使用的格式如下：

`/[Controller]/[ActionName]/[Parameters]`

URL 的第一個部分會決定要執行的控制器類別。 因此*/HelloWorld*對應至`HelloWorldController`類別。 URL 的第二個部分會決定要執行的類別上的動作方法。 因此*/HelloWorld/索引*導致`Index`方法`HelloWorldController`類別來執行。 請注意，我們只需要瀏覽*/HelloWorld*上方和`Index`方法使用預設值。 這是因為方法名為`Index`是將會在控制站呼叫，如果沒有明確指定的預設方法。

瀏覽至 `http://localhost:xxxx/HelloWorld/Welcome`。 `Welcome`方法會執行，且會傳回字串&quot;這是該  褖畫惎動作方法...&quot;. 預設的 MVC 對應是否`/[Controller]/[ActionName]/[Parameters]`。 此 url 中，控制器是`HelloWorld`和`Welcome`是方法。 我們尚未使用`[Parameters]`尚未 URL 的一部分。

![](adding-a-controller/_static/image6.png)

讓我們在範例稍微修改，讓我們可以從 URL 傳遞中的某些參數資訊，控制站 (例如， */HelloWorld/歡迎畫面？ 名稱 = Scott&amp;numtimes = 4*)。 變更您`Welcome`方法，將包含兩個參數，如下所示。 請注意，我們已經使用 VB 選擇性參數功能表示`numTimes`參數應該預設為 1，如果該參數不傳遞任何值。

[!code-vb[Main](adding-a-controller/samples/sample2.vb)]

執行您的應用程式，並瀏覽至`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4` **。** 您可以嘗試不同的值`name`和`numtimes`。 系統會自動對應具名的參數從您在網址列中的查詢字串參數，在您的方法。

![](adding-a-controller/_static/image7.png)

在這些範例中這兩個控制器已執行 MVC 的 VC 部分 — 這是檢視和控制器的工作。 控制器直接傳回 HTML。 通常，我們不想直接傳回 HTML，因為變得很麻煩的程式碼的控制站。 改為我們通常會使用不同的檢視範本檔案協助產生 HTML 回應。 讓我們看看如何我們可以執行這項操作。

> [!div class="step-by-step"]
> [上一頁](intro-to-aspnet-mvc-3.md)
> [下一頁](adding-a-view.md)
