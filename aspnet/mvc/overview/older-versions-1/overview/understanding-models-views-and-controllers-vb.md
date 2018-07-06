---
uid: mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-vb
title: 了解模型、 檢視和控制器 (VB) |Microsoft Docs
author: StephenWalther
description: 不清楚模型、 檢視和控制器嗎？ 在本教學課程中，Stephen walther 將向您介紹 ASP.NET MVC 應用程式的不同部分。
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: a106374a-5e74-4fd0-9ac0-1a32280e5d0d
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-vb
msc.type: authoredcontent
ms.openlocfilehash: c651201a34b9ab6b459d0f2ecf491b49feb64434
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399895"
---
<a name="understanding-models-views-and-controllers-vb"></a>了解模型、 檢視和控制器 (VB)
====================
藉由[Stephen Walther](https://github.com/StephenWalther)

> 不清楚模型、 檢視和控制器嗎？ 在本教學課程中，Stephen walther 將向您介紹 ASP.NET MVC 應用程式的不同部分。


本教學課程提供您的 ASP.NET MVC 概觀模型、 檢視和控制器。 換句話說，它會說明 M'，V'，與 C' ASP.NET MVC 中。

閱讀本教學課程中之後, 您應該了解 ASP.NET MVC 應用程式的不同部分如何一起運作。 此外，您也應該了解 ASP.NET MVC 應用程式架構的 ASP.NET Web Forms 應用程式或動態伺服器網頁應用程式的不同方式。

## <a name="the-sample-aspnet-mvc-application"></a>範例 ASP.NET MVC 應用程式

預設的 Visual Studio 範本，建立 ASP.NET MVC Web 應用程式包含可用來了解 ASP.NET MVC 應用程式的不同部分的非常簡單的範例應用程式。 我們利用這個簡單的應用程式，在本教學課程。

使用 MVC 範本建立新的 ASP.NET MVC 應用程式啟動 Visual Studio 2008 中，選取 檔案 功能表選項，新增專案 （請參閱 圖 1）。 在 [新增專案] 對話方塊中，選取您慣用的程式設計語言，在 [專案類型 (Visual Basic 或 C#），然後選取**ASP.NET MVC Web 應用程式**範本] 下。 按一下 [確定] 按鈕。


[![新增專案 對話方塊](understanding-models-views-and-controllers-vb/_static/image1.jpg)](understanding-models-views-and-controllers-vb/_static/image1.png)

**[圖 01**： 新增專案] 對話方塊 ([按一下以檢視完整大小的影像](understanding-models-views-and-controllers-vb/_static/image2.png))


當您建立新的 ASP.NET MVC 應用程式中，**建立單元測試專案**對話方塊隨即出現 （請參閱 圖 2）。 此對話方塊可讓您在您的解決方案，以測試 ASP.NET MVC 應用程式建立個別的專案。 選取的選項**否，不要建立單元測試專案**然後按一下**確定** 按鈕。


[![建立單元測試 對話方塊](understanding-models-views-and-controllers-vb/_static/image2.jpg)](understanding-models-views-and-controllers-vb/_static/image3.png)

**[圖 02**： 建立單元測試] 對話方塊 ([按一下以檢視完整大小的影像](understanding-models-views-and-controllers-vb/_static/image4.png))


新的 ASP.NET MVC 應用程式建立之後。 您會看到數個資料夾和 [方案總管] 視窗中的檔案。 特別是，您會看到名為模型、 檢視和控制器的三個資料夾。 跟您猜的資料夾名稱，這些資料夾會包含實作模型、 檢視和控制器的檔案。

如果您展開 [控制器] 資料夾，您應該會看到名為 AccountController.vb 的檔案和名為 HomeController.vb。 如果您展開 [Views] 資料夾，您應該會看到名為常用帳戶 」 及 「 共用的三個子資料夾。 如果您展開 [主資料夾] 資料夾，您會看到兩個額外的檔案，名為 about.aspx 的網頁和 Index.aspx （請參閱 [圖 3]）。 這些檔案是由預設 ASP.NET MVC 範本所隨附的範例應用程式所組成。


[![[方案總管] 視窗](understanding-models-views-and-controllers-vb/_static/image3.jpg)](understanding-models-views-and-controllers-vb/_static/image5.png)

**[圖 03**: 方案總管] 視窗 ([按一下以檢視完整大小的影像](understanding-models-views-and-controllers-vb/_static/image6.png))


您可以選取功能表選項來執行範例應用程式**偵錯，請啟動偵錯**。 或者，您可以按 F5 鍵。

當您第一次執行的 ASP.NET 應用程式時，在 [圖 4] 對話方塊隨即出現，建議您啟用偵錯模式。 按一下 [確定] 按鈕，並將執行應用程式。


[![未啟用的偵錯對話方塊](understanding-models-views-and-controllers-vb/_static/image4.jpg)](understanding-models-views-and-controllers-vb/_static/image7.png)

**圖 04**： 未啟用偵錯對話方塊 ([按一下以檢視完整大小的影像](understanding-models-views-and-controllers-vb/_static/image8.png))


當您執行的 ASP.NET MVC 應用程式時，Visual Studio 會啟動網頁瀏覽器中的應用程式。 範例應用程式包含只有兩個頁面: [索引] 頁面和 [關於] 頁面。 當應用程式第一次啟動時，[索引] 頁面隨即出現 （請參閱 [圖 5]）。 您可以按一下瀏覽至 [關於] 頁面頂端的功能表連結應用程式的權限。


[![[索引] 頁面](understanding-models-views-and-controllers-vb/_static/image5.jpg)](understanding-models-views-and-controllers-vb/_static/image9.png)

**圖 05**: [索引] 頁面 ([按一下以檢視完整大小的影像](understanding-models-views-and-controllers-vb/_static/image10.png))


請注意您的瀏覽器的網址列中的 Url。 例如，當您按一下關於功能表連結，瀏覽器網址列中的 URL 變更為 **/Home/**。

如果您關閉瀏覽器視窗，並返回 Visual Studio，將無法使用路徑首頁] / [關於尋找檔案。 檔案不存在。 這是怎麼辦到？

## <a name="a-url-does-not-equal-a-page"></a>URL 不等於 頁面

當您建置傳統的 ASP.NET Web Forms 應用程式或動態伺服器網頁應用程式時，但沒有 URL 與頁面之間的一對一對應。 如果您要求名 SomePage.aspx 為來自伺服器的頁面，則有更好有頁面在名為 SomePage.aspx 磁碟上。 如果 SomePage.aspx 檔案不存在，您會收到麻煩**404-找不到頁面**時發生錯誤。

建置 ASP.NET MVC 應用程式時，相較之下，沒有任何您輸入您的瀏覽器網址列的 URL 和您在您的應用程式中找到的檔案之間的對應。 ASP.NET MVC 應用程式中，URL 會對應至控制器動作，而不是在磁碟上的頁面。

在傳統 ASP.NET 或 ASP 應用程式中，瀏覽器要求會對應至頁面。 相反地，ASP.NET MVC 應用程式中，瀏覽器要求會對應至控制器動作。 ASP.NET Web Forms 應用程式是以內容為中心。 ASP.NET MVC 應用程式時，相較之下，為中心的應用程式邏輯。

## <a name="understanding-aspnet-routing"></a>了解 ASP.NET 路由

瀏覽器要求對應至控制器動作透過一項功能的 ASP.NET 架構，稱為*ASP.NET 路由*。 ASP.NET 路由正由 ASP.NET MVC 架構，來*路由*至控制器動作的連入要求。

ASP.NET 路由處理傳入要求，使用路由表。 您的 web 應用程式第一次啟動時，會建立此路由表。 路由表是在 Global.asax 檔案中的安裝程式。 在 列表 1 中包含預設 MVC Global.asax 檔案。

**列表 1-Global.asax**

[!code-vb[Main](understanding-models-views-and-controllers-vb/samples/sample1.vb)]

ASP.NET 應用程式第一次啟動時，應用程式\_呼叫 start （） 方法。 在列表 1 中，這個方法會呼叫 RegisterRoutes() 方法並 RegisterRoutes() 方法建立預設路由表。

預設路由表包含一個路由。 這個預設路由會分成三個區段 （URL 區段是正斜線之間的項目） 中的所有連入要求。 第一個區段會對應至控制器名稱、 第二個區段會對應至動作名稱，和最後一個區段會對應到傳遞至名為識別碼。 此動作的參數

例如，請考慮下列 URL：

/ 產品/詳細資料/3

此 URL 會剖析為三個參數，就像這樣：

控制器 = 產品

動作 = 詳細資料

識別碼 = 3

在 Global.asax 檔案中定義的預設路由包含所有的三個參數的預設值。 預設控制器是家用版、 預設動作是索引，和識別碼的預設值為空字串。 記住這些預設值，請考慮下列 URL 的剖析方式：

/ 所有員工

此 URL 會剖析為三個參數，就像這樣：

控制器 = 員工

動作 = 索引

識別碼 =

最後，如果您開啟 ASP.NET MVC 應用程式而不提供任何的 URL (例如`http://localhost`) 將 URL 會剖析像這樣：

控制器 = 首頁

動作 = 索引

識別碼 =

要求會路由至 HomeController 類別中的 index （） 動作。

## <a name="understanding-controllers"></a>了解控制器

控制器負責控制使用者互動的 MVC 應用程式的方式。 控制器包含 ASP.NET MVC 應用程式的流程控制邏輯。 控制器會決定哪些傳送傳回給使用者，當使用者瀏覽器要求的回應。

控制器是只是一個類別 （比方說，Visual Basic 或 C# 類別）。 此範例 ASP.NET MVC 應用程式包含名為 HomeController.vb 位於 [控制器] 資料夾中的控制站。 在 列表 2 中重現 HomeController.vb 檔案的內容。

**列表 2-HomeController.cs**

[!code-vb[Main](understanding-models-views-and-controllers-vb/samples/sample2.vb)]

請注意 HomeController 有兩種方法，名為 index （） 和 about （）。 這兩種方法會對應至兩個控制器所公開的動作。 URL /Home/索引叫用 HomeController.Index() 方法和 URL /Home] / [關於叫用 HomeController.About() 方法。

在控制器中的任何公用方法會公開為控制器動作。 您需要小心一點。 這表示，內含在控制器中的任何公用方法可以叫用來存取網際網路的任何人都將正確的 URL 輸入瀏覽器。

## <a name="understanding-views"></a>了解檢視

由 HomeController 類別，index （） 和 about （），公開兩個控制器動作傳回檢視。 檢視包含 HTML 標記和傳送至瀏覽器的內容。 使用 ASP.NET MVC 應用程式時，檢視會是相當於頁面。

您必須在正確的位置中建立您的檢視。 HomeController.Index() 動作傳回檢視位於下列路徑：

\Views\Home\Index.aspx

HomeController.About() 動作傳回檢視位於下列路徑：

\Views\Home\About.aspx

一般情況下，如果您想要傳回一份檢視的控制器動作，然後您要在與您的控制器名稱相同的 [Views] 資料夾中建立子資料夾。 內的子資料夾中，您必須建立控制器動作同名的.aspx 檔案。

列表 3 中的檔案包含 about.aspx 的網頁檢視。

**列表 3-about.aspx 的網頁**

[!code-aspx[Main](understanding-models-views-and-controllers-vb/samples/sample3.aspx)]

如果您選擇忽略列表 3 中的第一行，檢視的其餘部分包含標準的 HTML。 您可以輸入任何您想要的 HTML，以修改檢視的內容。

檢視是非常類似於 Active Server Pages 或 ASP.NET Web Form 中的頁面。 檢視可以包含 HTML 內容和指令碼。 您可以撰寫指令碼，在您最愛的.NET 程式設計語言 （例如，C# 或 Visual Basic.NET）。 您可以使用指令碼來顯示 動態內容，例如資料庫的資料。

## <a name="understanding-models"></a>了解模型

我們已經討論過控制器，我們已經討論過的檢視。 模型是我們要討論的最後一個主題。 MVC 模型是什麼？

MVC 模型包含的所有應用程式邏輯，並未包含在檢視或控制站。 模型應該包含的所有應用程式商務邏輯，驗證邏輯和資料庫存取邏輯。 比方說，如果您使用 Microsoft Entity Framework 來存取您的資料庫，然後您會建立您的 Entity Framework 類別 （在.edmx 檔案） 在 Models 資料夾中。

檢視應包含產生使用者介面與相關的邏輯。 控制器應該只包含邏輯傳回正確的檢視，或將使用者重新導向至另一個動作 （流量控制） 所需的最低限度。 所有其他項目應該包含在模型中。

一般情況下，您應該盡量 fat 模型和 skinny 控制站。 控制器方法應該包含只需幾行程式碼。 如果太 fat 控制器動作，您應該考慮將的邏輯移至 [Models] 資料夾中的新類別。

## <a name="summary"></a>總結

本教學課程提供您的 ASP.NET MVC 的不同部分的高階概觀 web 應用程式。 您已了解如何 ASP.NET 路由傳入瀏覽器將要求對應至特定的控制器動作。 您已了解如何控制站協調檢視傳回至瀏覽器的方式。 最後，您已了解如何模型包含應用程式商務、 驗證和資料庫存取邏輯。
