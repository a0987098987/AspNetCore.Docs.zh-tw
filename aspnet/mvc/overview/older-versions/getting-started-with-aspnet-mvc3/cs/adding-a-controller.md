---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
title: 新增控制器 (C#) |Microsoft Docs
author: Rick-Anderson
description: 本教學課程將教導您建置使用 Microsoft Visual Web Developer 2010 Express 服務 Pack 1，哪些 i 的 ASP.NET MVC Web 應用程式的基本概念...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 0b8c56b5-fdf3-42dd-a866-98fbe0ab78a0
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: b31cf3bdf18c144d2735973119367b01de0353fe
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830563"
---
<a name="adding-a-controller-c"></a>新增控制器 (C#)
====================
藉由[Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > 本教學課程中的更新的版本可[此處](../../../getting-started/introduction/getting-started.md)使用 ASP.NET MVC 5 和 Visual Studio 2013。 它更安全、 更容易遵循，並示範更多的功能。
> 
> 
> 本教學課程將教導您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，也就是 Microsoft Visual Studio 的免費版本的 ASP.NET MVC Web 應用程式的基本概念。 在開始之前，請確定您已安裝符合下列先決條件。 您可以安裝所有人都按下列連結： [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。 或者，您可以個別安裝的必要條件，使用下列連結：
> 
> - [Visual Studio Web Developer Express SP1 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（執行階段 + 工具支援）
> 
> 如果您使用 Visual Studio 2010 而不 Visual Web Developer 2010，請按一下下列連結安裝必要的： [Visual Studio 2010 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。
> 
> 使用本主題隨附了 C# 原始程式碼的 Visual Web Developer 專案。 [下載 C# 版本](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)。 如果您偏好 Visual Basic，切換到[Visual Basic 版本](../vb/intro-to-aspnet-mvc-3.md)本教學課程。


代表 MVC*模型-檢視-控制器*。 MVC 是開發應用程式的架構以及易於維護的模式。 MVC 架構的應用程式包含：

- 控制站： 類別，可處理傳入的要求，應用程式、 擷取模型資料，然後指定 將回應傳回給用戶端的檢視範本。
- 代表資料的應用程式，並強制執行商務規則，該資料使用的驗證邏輯的模型： 類別。
- 您的應用程式用來動態產生 HTML 回應的檢視： 範本檔案。

我們就可以涵蓋在本教學課程系列中的所有這些概念，並示範如何使用它們來建置應用程式。

讓我們開始建立控制器類別。 在 **方案總管**，以滑鼠右鍵按一下*控制站*資料夾，然後選取**新增控制器**。

[![](adding-a-controller/_static/image2.png)](adding-a-controller/_static/image1.png)

將新的控制器"HelloWorldController 」 的名稱。 保留預設範本，因為**空白控制器**然後按一下**新增**。

[![AddHelloWorldController](adding-a-controller/_static/image4.png)](adding-a-controller/_static/image3.png)

請注意，在**方案總管**新的檔案已經建立的具名*HelloWorldController.cs*。 在 IDE 中開啟檔案。

![](adding-a-controller/_static/image5.png)

內`public class HelloWorldController`區塊中，建立兩個方法，看起來像下列程式碼。 控制器會傳回 HTML 的字串做為範例。

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

名為您的控制器`HelloWorldController`和上述的第一個方法命名為`Index`。 讓我們叫用它從瀏覽器。 執行應用程式 （按 F5 或 ctrl+f5）。 在瀏覽器中，附加"HelloWorld"的網址列中的路徑。 (例如，在圖中，其下的`http://localhost:43246/HelloWorld.`) 的瀏覽器頁面看起來像下列螢幕擷取畫面。 在上述的方法中，程式碼會直接傳回字串。 您告訴系統只會傳回一些 HTML，且啟動成功 ！

![](adding-a-controller/_static/image6.png)

根據傳入 URL 不同控制器類別 （和不同的動作方法），會叫用 ASP.NET MVC。 使用 ASP.NET MVC 的預設對應邏輯會使用像這樣的格式，以判斷哪些程式碼叫用：

`/[Controller]/[ActionName]/[Parameters]`

URL 的第一個部分會判斷要執行的控制器類別。 因此 */HelloWorld*對應至`HelloWorldController`類別。 URL 的第二部分會判斷要執行的類別上的動作方法。 因此 */HelloWorld/Index*會導致`Index`方法`HelloWorldController`類別來執行。 請注意，我們只需要瀏覽至 */HelloWorld*而`Index`方法已由預設值。 這是因為方法命名為`Index`是會在控制器呼叫，如果沒有明確指定的預設方法。

瀏覽至 `http://localhost:xxxx/HelloWorld/Welcome`。 `Welcome` 方法隨即執行，並傳回字串 "This is the Welcome action method..."。 預設的 MVC 對應是`/[Controller]/[ActionName]/[Parameters]`。 在此 URL 中，控制器是 `HelloWorld`，而 `Welcome` 是動作方法。 您尚未使用 URL 的 `[Parameters]` 部分。

![](adding-a-controller/_static/image7.png)

讓我們在範例稍微修改，讓您可以將從 URL 的某些參數資訊傳遞到控制器 (例如 */HelloWorld/歡迎？ 名稱 = Scott&amp;numtimes = 4*)。 變更您`Welcome`方法以包含兩個參數，如下所示。 請注意，程式碼會使用 C# 選擇性參數功能，在表示`numTimes`參數應該預設為 1，如果沒有傳遞任何值，該參數。

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

執行應用程式，並瀏覽至範例 URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`。 您可以嘗試不同的值，如`name`和`numtimes`在 URL 中。 系統會自動對應方法中的參數從查詢字串，在網址列中的具名的參數。

![](adding-a-controller/_static/image8.png)

在這些範例中兩個控制器已執行 MVC 的"VC"部分，也就是檢視和控制器工作。 控制器會直接傳回 HTML。 通常您不希望控制器直接傳回 HTML，因為，變得很麻煩的程式碼。 而是我們通常會使用不同的檢視範本檔案協助產生 HTML 回應。 讓我們看如何能夠在下一步。

> [!div class="step-by-step"]
> [上一頁](intro-to-aspnet-mvc-3.md)
> [下一頁](adding-a-view.md)
