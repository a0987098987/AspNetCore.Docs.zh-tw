---
uid: mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-vb
title: 了解模型、 檢視和控制站 (VB) |Microsoft 文件
author: StephenWalther
description: 搞不清楚模型、 檢視和控制器嗎？ 在本教學課程中，作者： Stephen Walther 向您介紹 ASP.NET MVC 應用程式的不同部分。
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: a106374a-5e74-4fd0-9ac0-1a32280e5d0d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-vb
msc.type: authoredcontent
ms.openlocfilehash: 36f70503f7e96210e5fb8a2d361da6759c18c85b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="understanding-models-views-and-controllers-vb"></a>了解模型、 檢視和控制站 (VB)
====================
由[Stephen Walther](https://github.com/StephenWalther)

> 搞不清楚模型、 檢視和控制器嗎？ 在本教學課程中，作者： Stephen Walther 向您介紹 ASP.NET MVC 應用程式的不同部分。


本教學課程為您提供的高階概觀 ASP.NET MVC 模型、 檢視和控制器。 換句話說，它會說明 M'，V'，與 C' ASP.NET MVC 中。

閱讀本教學課程之後, 您應該了解 ASP.NET MVC 應用程式的不同部分如何一起運作。 您也應該了解 ASP.NET MVC 應用程式的架構不同於 ASP.NET Web Form 應用程式或動態伺服器網頁應用程式的方式。

## <a name="the-sample-aspnet-mvc-application"></a>範例 ASP.NET MVC 應用程式

建立 ASP.NET MVC Web 應用程式的預設 Visual Studio 範本包含非常簡單的範例應用程式，可用來了解 ASP.NET MVC 應用程式的不同部分。 我們會利用這個簡單的應用程式，在本教學課程。

與 MVC 範本建立新的 ASP.NET MVC 應用程式啟動 Visual Studio 2008 中，選取 [檔案] 功能表選項，新增專案 （請參閱圖 1）。 在 新增專案 對話方塊中，選取 專案類型 (Visual Basic 或 C#） 您最愛的程式語言，然後選取**ASP.NET MVC Web 應用程式**範本下。 按一下 [確定] 按鈕。


[![新增專案 對話方塊](understanding-models-views-and-controllers-vb/_static/image1.jpg)](understanding-models-views-and-controllers-vb/_static/image1.png)

**圖 01**： 新增專案 對話方塊 ([按一下以檢視完整大小的影像](understanding-models-views-and-controllers-vb/_static/image2.png))


當您建立新的 ASP.NET MVC 應用程式，**建立單元測試專案**對話方塊隨即出現 （請參閱圖 2）。 這個對話方塊可讓您以測試 ASP.NET MVC 應用程式在方案中建立個別的專案。 選取的選項**否，不要建立單元測試專案**按一下**確定** 按鈕。


[![建立單元測試 對話方塊](understanding-models-views-and-controllers-vb/_static/image2.jpg)](understanding-models-views-and-controllers-vb/_static/image3.png)

**圖 02**： 建立單元測試對話方塊 ([按一下以檢視完整大小的影像](understanding-models-views-and-controllers-vb/_static/image4.png))


新的 ASP.NET MVC 應用程式建立之後。 您會看到數個資料夾和 [方案總管] 視窗中的檔案。 特別是，您會看到名為模型、 檢視和控制器的三個資料夾。 您猜的資料夾名稱，這些資料夾包含實作模型、 檢視和控制器的檔案。

如果您展開 Controllers 資料夾，您應該會看到名為 AccountController.vb 和名為 HomeController.vb 的檔案。 如果您展開 [檢視] 資料夾，您應該會看到三個名為帳戶首頁] 和 [共用的子資料夾。 如果您展開 Home 資料夾，您會看到兩個名為 about.aspx 的網頁和 Index.aspx （請參閱圖 3） 的其他檔案。 這些檔案便會產生預設的 ASP.NET MVC 範本所隨附的範例應用程式。


[![[方案總管] 視窗](understanding-models-views-and-controllers-vb/_static/image3.jpg)](understanding-models-views-and-controllers-vb/_static/image5.png)

**圖 03**: 方案總管 視窗 ([按一下以檢視完整大小的影像](understanding-models-views-and-controllers-vb/_static/image6.png))


您可以藉由選取功能表選項來執行範例應用程式**偵錯，開始偵錯**。 或者，您可以按 F5 鍵。

當您第一次執行 ASP.NET 應用程式時，在圖 4 對話方塊隨即出現，建議您啟用偵錯模式。 按一下 [確定] 按鈕，而且應用程式會執行。


[![未啟用的偵錯對話方塊](understanding-models-views-and-controllers-vb/_static/image4.jpg)](understanding-models-views-and-controllers-vb/_static/image7.png)

**圖 04**： 未啟用偵錯對話方塊 ([按一下以檢視完整大小的影像](understanding-models-views-and-controllers-vb/_static/image8.png))


當您執行 ASP.NET MVC 應用程式時，Visual Studio 會啟動網頁瀏覽器中的應用程式。 範例應用程式只有兩個頁面所組成： 索引頁面和 [關於] 頁面。 當應用程式第一次啟動時，索引頁會出現 （請參閱圖 5）。 您可以導覽至 [關於] 頁面按一下頂端的功能表連結應用程式的權限。


[![索引頁](understanding-models-views-and-controllers-vb/_static/image5.jpg)](understanding-models-views-and-controllers-vb/_static/image9.png)

**圖 05**: 索引頁 ([按一下以檢視完整大小的影像](understanding-models-views-and-controllers-vb/_static/image10.png))


請注意您的瀏覽器的網址列中的 Url。 例如，當您按一下 [關於] 功能表的連結，在瀏覽器網址列中的 URL 變更為 **/家用] / [關於**。

如果您關閉瀏覽器視窗並返回 Visual Studio，將無法與路徑首頁/約尋找檔案。 檔案不存在。 如何這可能會？

## <a name="a-url-does-not-equal-a-page"></a>URL 不等於頁面

當您建置傳統的 ASP.NET Web Form 應用程式或動態伺服器網頁應用程式時，但沒有 URL 和頁面之間的一對一對應。 如果您要求從伺服器名為 SomePage.aspx 的頁面，然後有更有頁面 SomePage.aspx 名為的磁碟上。 如果 SomePage.aspx 檔案不存在，您會收到優劣**404-找不到頁面**錯誤。

建置 ASP.NET MVC 應用程式時，相較之下，沒有任何您在瀏覽器的網址列輸入的 URL 與您在您的應用程式中找到的檔案之間的對應。 在 ASP.NET MVC 應用程式 URL 對應至控制器動作，而不是磁碟上的頁面。

在傳統 ASP.NET 或 ASP 應用程式中，瀏覽器要求，會對應至頁面。 相反地，ASP.NET MVC 應用程式中，瀏覽器要求，會對應至控制器的動作。 ASP.NET Web Form 應用程式是以內容為中心。 ASP.NET MVC 應用程式相較之下，為中心的應用程式邏輯。

## <a name="understanding-aspnet-routing"></a>了解 ASP.NET 路由

瀏覽器要求對應至呼叫的 ASP.NET framework 功能透過控制器動作*ASP.NET 路由*。 ASP.NET 路由正由 ASP.NET MVC 架構*路由*控制器動作的內送要求。

ASP.NET 路由會使用路由表，來處理傳入要求。 Web 應用程式第一次啟動時，會建立此路由表。 路由表是在 Global.asax 檔案中的安裝程式。 預設的 MVC Global.asax 檔案包含在程式碼範例 1。

**列出 1-Global.asax**

[!code-vb[Main](understanding-models-views-and-controllers-vb/samples/sample1.vb)]

ASP.NET 應用程式第一次啟動時，應用程式\_呼叫 start （） 方法。 在清單 1 中，這個方法會呼叫 RegisterRoutes() 方法和 RegisterRoutes() 方法會建立預設路由表。

預設路由表包含一個路由。 這個預設路由中分成三個區段 （URL 區段是正斜線之間的任何項目） 的所有連入要求。 第一個區段對應至控制器名稱、 第二個區段都會對應到動作名稱，以及最後區段對應到參數傳遞給名為識別碼的動作

例如，請考慮下列 URL:

/ 產品/詳細資料/3

此 URL 會剖析成三個參數，就像這樣：

控制器 = 產品

動作 = 詳細資料

識別碼 = 3

在 Global.asax 檔中定義的預設路由包含所有的三個參數的預設值。 預設控制器為首頁、 預設動作是索引，而識別碼的預設值為空字串。 請注意這些預設值，請考慮下列 URL 剖析的方式：

/ 所有員工

此 URL 會剖析成三個參數，就像這樣：

控制器 = 員工

動作 = 索引

識別碼 =

最後，如果您未提供任何 URL 開啟 ASP.NET MVC 應用程式 (例如， `http://localhost`) 將 URL 會剖析像這樣：

控制器 = Home

動作 = 索引

識別碼 =

要求會路由至 HomeController 類別中的 index 動作。

## <a name="understanding-controllers"></a>了解控制器

控制器負責控制使用者與 MVC 應用程式互動的方式。 控制器包含 ASP.NET MVC 應用程式的流程控制邏輯。 控制器會決定哪些回應傳送回給使用者，當使用者瀏覽器要求。

在控制站是只類別 （例如，Visual Basic 或 C# 類別）。 此範例的 ASP.NET MVC 應用程式包含名為 HomeController.vb 位於 [控制器] 資料夾中的控制站。 列出 2 中重現 HomeController.vb 檔案的內容。

**列出 2-HomeController.cs**

[!code-vb[Main](understanding-models-views-and-controllers-vb/samples/sample2.vb)]

請注意 HomeController 有兩個名為 index （） 和 about 的方法。 這兩種方法對應到兩個控制器所公開的動作。 URL /Home/索引叫用 HomeController.Index() 方法和 URL /Home/需叫用 HomeController.About() 方法。

在控制器中的任何公用方法會公開為控制器動作。 您需要謹慎這。 這表示，在控制器中包含任何公用方法可以叫用具有網際網路存取權的任何人都將正確的 URL 輸入瀏覽器。

## <a name="understanding-views"></a>了解檢視

兩個控制器的動作由 HomeController 類別，index （） 和 about，兩者都傳回檢視表。 檢視包含的 HTML 標記和傳送到瀏覽器的內容。 使用 ASP.NET MVC 應用程式時，檢視是網頁的對等項目。

您必須建立您的檢視，在正確的位置。 HomeController.Index() 動作將會傳回檢視位於下列路徑：

\Views\Home\Index.aspx

HomeController.About() 動作將會傳回檢視位於下列路徑：

\Views\Home\About.aspx

一般情況下，如果您想要傳回控制器動作的檢視，然後您要在與您的控制器名稱相同的 [檢視] 資料夾中建立子資料夾。 中子資料夾中，您必須建立.aspx 檔案具有相同名稱做為控制器動作。

列出的 3 中的檔案包含 about.aspx 的網頁檢視。

**列出 3-about.aspx 的網頁**

[!code-aspx[Main](understanding-models-views-and-controllers-vb/samples/sample3.aspx)]

如果您忽略列表 3 中的第一行，大部分的檢視的其餘部分所組成標準 HTML。 您可以輸入任何您想要的 HTML 修改檢視的內容。

檢視是非常類似於 Active Server Pages 或 ASP.NET Web Form 中的頁面。 檢視可以包含 HTML 內容和指令碼。 您可以撰寫指令碼，在您最愛的.NET 程式語言 （例如，C# 或 Visual Basic.NET）。 您可以使用指令碼來顯示動態內容，例如資料庫的資料。

## <a name="understanding-models"></a>了解模型

我們已經討論過控制站，我們已經討論過的檢視。 最後一個我們要討論的主題是模型。 MVC 模型是什麼？

MVC 模型包含的所有應用程式邏輯，並未包含在檢視或控制站。 模型應包含所有的應用程式商務邏輯，驗證邏輯和資料庫存取邏輯。 比方說，如果您使用 Microsoft Entity Framework 來存取您的資料庫，然後您會建立您 Entity Framework 類別 （在.edmx 檔案） 在 [模型] 資料夾中。

檢視應該包含相關以產生使用者介面的邏輯。 在控制站應該只包含邏輯傳回右方檢視，或將使用者重新導向至另一個動作 （流量控制） 所需的最低限度。 所有其他項目應包含在模型中。

一般情況下，您應盡可能 fat 模型和 skinny 控制站。 控制器方法應只有幾行程式碼。 如果太 fat 控制器動作，您應該考慮將邏輯移到 [模型] 資料夾中的新類別。

## <a name="summary"></a>總結

本教學課程提供您的 ASP.NET MVC 的不同部分的高層級概觀 web 應用程式。 您已學習如何 ASP.NET 路由傳入瀏覽器將要求對應至特定控制器的動作。 您已學習如何控制站協調瀏覽器傳回檢視的方式。 最後，您學到如何模型包含應用程式商務、 驗證和資料庫存取邏輯。
