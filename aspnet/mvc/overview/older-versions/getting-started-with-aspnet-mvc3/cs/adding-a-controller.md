---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
title: 加入控制器 (C#) |Microsoft 文件
author: Rick-Anderson
description: 本教學課程將告訴您建置使用 Microsoft Visual Web Developer 2010 Express 服務組件 1，哪些 i 的 ASP.NET MVC Web 應用程式的基本概念...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 0b8c56b5-fdf3-42dd-a866-98fbe0ab78a0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 963d3bbbadf408d7045c50bfd693069e4097d45d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868291"
---
<a name="adding-a-controller-c"></a>加入控制器 (C#)
====================
由[Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > 本教學課程的更新的版本時使用[這裡](../../../getting-started/introduction/getting-started.md)使用 ASP.NET MVC 5 和 Visual Studio 2013。 它更安全、 容易遵循，及示範更多的功能。
> 
> 
> 本教學課程將告訴您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，這是 Microsoft Visual Studio 的免費版本的 ASP.NET MVC Web 應用程式的基本概念。 開始之前，請確定您已安裝下面所列的必要條件。 您可以安裝全部都按下列連結： [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。 或者，您可以個別安裝的必要條件，使用下列連結：
> 
> - [Visual Studio Web Developer Express SP1 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（執行階段 + 工具支援）
> 
> 如果您使用 Visual Studio 2010 而不 Visual Web Developer 2010，安裝必要元件，請按一下下列連結： [Visual Studio 2010 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。
> 
> 使用本主題隨附在 Visual Web Developer 專案中的使用 C# 原始程式碼。 [下載的 C# 版本](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)。 如果您偏好 Visual Basic，切換至[Visual Basic 版本](../vb/intro-to-aspnet-mvc-3.md)本教學課程。


代表 MVC*模型-檢視-控制器*。 MVC 是用於開發架構且容易維護的應用程式的模式。 MVC 架構的應用程式包含：

- 控制站： 類別，可處理傳入要求至應用程式中，擷取模型資料，然後將回應傳回至用戶端的檢視範本。
- 代表應用程式的資料，並使用驗證邏輯來強制執行商務規則，該資料的模型： 類別。
- 您的應用程式會使用動態產生 HTML 回應的檢視： 範本檔案。

我們將會涵蓋在本教學課程系列中的所有這些概念，並示範如何使用它們來建置應用程式。

讓我們先建立控制器類別。 在**方案總管 中**，以滑鼠右鍵按一下*控制器*資料夾，然後選取**加入控制器**。

[![](adding-a-controller/_static/image2.png)](adding-a-controller/_static/image1.png)

命名您的新控制器"HelloWorldController"。 保留為預設範本**空白控制器**按一下**新增**。

[![AddHelloWorldController](adding-a-controller/_static/image4.png)](adding-a-controller/_static/image3.png)

請注意，在**方案總管 中**新檔案已經建立的具名*HelloWorldController.cs*。 已在 IDE 中開啟的檔案。

![](adding-a-controller/_static/image5.png)

內部`public class HelloWorldController`封鎖，請建立兩個方法看起來像下列程式碼。 控制器將會傳回 HTML 的字串做為範例。

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

將控制器命名為`HelloWorldController`上述第一種方法稱為`Index`。 讓我們來叫用它從瀏覽器。 執行應用程式 （按 F5 或 Ctrl + F5）。 在瀏覽器中附加"HelloWorld"網址列中的路徑。 (例如，在圖例中，其下的`http://localhost:43246/HelloWorld.`) 網頁瀏覽器中的看起來像下列螢幕擷取畫面。 上述的方法，在程式碼會直接傳回字串。 告訴系統只傳回一些 HTML，且啟動成功 ！

![](adding-a-controller/_static/image6.png)

根據傳入的 URL 不同控制器類別 （和不同的動作方法內），會叫用 ASP.NET MVC。 使用 ASP.NET MVC 的預設對應邏輯來判斷哪些程式碼叫用使用的格式如下：

`/[Controller]/[ActionName]/[Parameters]`

URL 的第一個部分會決定要執行的控制器類別。 因此 */HelloWorld*對應至`HelloWorldController`類別。 URL 的第二個部分會決定要執行的類別上的動作方法。 因此 */HelloWorld/索引*導致`Index`方法`HelloWorldController`類別來執行。 請注意，我們只需要瀏覽至 */HelloWorld*和`Index`方法使用預設值。 這是因為方法名為`Index`是將會在控制站呼叫，如果沒有明確指定的預設方法。

瀏覽至 `http://localhost:xxxx/HelloWorld/Welcome`。 `Welcome` 方法隨即執行，並傳回字串 "This is the Welcome action method..."。 預設的 MVC 對應是否`/[Controller]/[ActionName]/[Parameters]`。 在此 URL 中，控制器是 `HelloWorld`，而 `Welcome` 是動作方法。 您尚未使用 URL 的 `[Parameters]` 部分。

![](adding-a-controller/_static/image7.png)

讓我們在範例稍微修改，讓您可以將從 URL 的某些參數資訊傳遞至控制器 (例如， */HelloWorld/歡迎畫面？ 名稱 = Scott&amp;numtimes = 4*)。 變更您`Welcome`方法，將包含兩個參數，如下所示。 請注意，程式碼會使用 C# 選擇性參數功能表示`numTimes`參數應該預設為 1，如果該參數不傳遞任何值。

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

執行您的應用程式，並瀏覽至範例 URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`。 您可以嘗試不同的值`name`和`numtimes`在 URL 中。 系統會自動對應網址列中的查詢字串中您的方法參數的具名的參數。

![](adding-a-controller/_static/image8.png)

在這些範例中這兩個控制器已執行 MVC"VC"部份，也就是，則檢視和控制器的工作。 控制器直接傳回 HTML。 通常您不想直接傳回 HTML，因為變得很麻煩的程式碼的控制站。 改為我們通常會使用不同的檢視範本檔案協助產生 HTML 回應。 讓我們來看我們如何執行這在下一步。

> [!div class="step-by-step"]
> [上一頁](intro-to-aspnet-mvc-3.md)
> [下一頁](adding-a-view.md)
