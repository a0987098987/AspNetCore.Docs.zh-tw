---
uid: mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
title: ASP.NET MVC 檢視概觀 (C#) |Microsoft Docs
author: StephenWalther
description: 什麼是 ASP.NET MVC 檢視，它有何不同的 HTML 網頁？ 在本教學課程中，Stephen Walther 會為您介紹檢視，並向您示範如何 t...
ms.author: riande
ms.date: 02/16/2008
ms.assetid: 152ab1e5-aec2-4ea7-b8cc-27a24dd9acb8
msc.legacyurl: /mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: ac47caa46d93c6157926f1c9b5112555fae4f8f5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825765"
---
<a name="aspnet-mvc-views-overview-c"></a>ASP.NET MVC 檢視概觀 (C#)
====================
藉由[Stephen Walther](https://github.com/StephenWalther)

> 什麼是 ASP.NET MVC 檢視，它有何不同的 HTML 網頁？ 在本教學課程中，Stephen Walther 會介紹檢視，並示範如何使用檢視資料和檢視表中的 HTML Helper 的利用。


本教學課程的目的是為您提供 ASP.NET MVC 檢視、 檢視資料和 HTML 協助程式簡介。 本教學課程結束時，您應該了解如何建立新的檢視，將資料從控制器傳遞至檢視，並使用 HTML 協助程式產生內容檢視中。

## <a name="understanding-views"></a>了解檢視

針對 ASP.NET 或動態伺服器網頁，ASP.NET MVC 不包含任何直接對應至頁面的項目。 ASP.NET MVC 應用程式中，頁面上沒有對應至在您輸入您的瀏覽器的網址列的 URL 路徑的磁碟。 ASP.NET MVC 應用程式中最有用的頁面是呼叫*檢視*。

ASP.NET MVC 應用程式中，連入的瀏覽器要求對應至控制器動作。 控制器動作可能會傳回檢視。 不過，控制器動作可能會執行其他類型的動作，例如將您重新導向至另一個控制器動作。

列表 1 包含名為 HomeController 的簡單控制器。 HomeController 會公開名為 index （） 和 Details() 的兩個控制器動作。

**列表 1-HomeController.cs**

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample1.cs)]

您可以呼叫第一個動作，也就是 index （） 動作中，您的瀏覽器網址列中輸入下列 URL:

/ Home/Index

您可以呼叫第二個動作，也就是 Details() 動作中，您的瀏覽器中輸入此位址：

/ Home/詳細資料

Index （） 動作傳回的檢視。 您所建立的大部分動作會傳回檢視。 不過，動作可以傳回其他類型的動作結果。 例如，Details() 動作會傳回傳入的要求重新導向至 index （） 動作 RedirectToActionResult。

Index （） 動作包含下列一行程式碼：

View();

這行程式碼會傳回必須位於 web 伺服器上的下列路徑的檢視：

\Views\Home\Index.aspx

檢視的路徑會從控制器的名稱和控制器動作的名稱來推斷。

如果您想，您可以明確有關檢視。 下列程式碼會傳回名為 Fred 的檢視：

檢視 (Fred);

這行程式碼執行時，檢視會傳回從下列路徑：

\Views\Home\Fred.aspx

> [!NOTE] 
> 
> 如果您打算建立 ASP.NET MVC 應用程式的單元測試它是個不錯的主意，明確了檢視表名稱。 如此一來，您可以建立單元測試以確認控制器動作未傳回預期的檢視。


## <a name="adding-content-to-a-view"></a>將內容新增至檢視

檢視是一種標準 (X) 可以包含指令碼的 HTML 文件。 您可以使用指令碼新增至檢視動態內容。

例如，列表 2 中的檢視會顯示目前的日期和時間。

**列表 2-\Views\Home\Index.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample2.aspx)]

請注意，在 列表 2 中的 HTML 頁面的主體包含下列指令碼：

&lt;% Response.Write(DateTime.Now);%&gt;

使用指令碼分隔符號&lt;%和 %&gt;來標記的開頭和結尾的指令碼。 此指令碼是以 C# 撰寫的。 它會藉由呼叫 response.write （） 方法，來將內容呈現至瀏覽器中顯示目前的日期和時間。 指令碼分隔符號&lt;%和 %&gt;可用來執行一或多個陳述式。

因為您經常呼叫 response.write （） 時，Microsoft 提供您與快速鍵呼叫 response.write （） 方法。 [列表 3] 檢視會使用分隔符號&lt;%= %&gt;呼叫 response.write （） 的捷徑。

**列表 3-Views\Home\Index2.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample3.aspx)]

您可以使用任何.NET 語言來產生動態內容檢視中。 一般來說，您將會使用 Visual Basic.NET 或 C# 來撰寫您的控制器和檢視。

## <a name="using-html-helpers-to-generate-view-content"></a>使用 HTML Helper 來產生檢視內容

若要讓您更輕鬆地將內容新增至檢視，您可以利用所謂*HTML 協助程式*。 HTML 協助程式，通常是產生字串的方法。 您可以使用 HTML 協助程式來產生標準的 HTML 項目，例如文字方塊、 連結、 下拉式清單和清單方塊。

例如，在 列表 4 善加利用三個 HTML 協助程式中，檢視 BeginForm()、 TextBox() 和 Password() 協助程式，來產生登入會形成 （請參閱 圖 1）。

**列表 4-\Views\Home\Login.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample4.aspx)]


[![[新增專案] 對話方塊](asp-net-mvc-views-overview-cs/_static/image1.jpg)](asp-net-mvc-views-overview-cs/_static/image1.png)

**圖 01**： 標準的登入表單 ([按一下以檢視完整大小的影像](asp-net-mvc-views-overview-cs/_static/image2.png))


所有 HTML 協助程式方法會呼叫檢視的 Html 屬性。 比方說，您會藉由呼叫 Html.TextBox() 方法呈現文字方塊。

請注意，您會使用指令碼分隔符號&lt;%= %&gt;時呼叫的 Html.TextBox() 」 和 「 Html.Password() 協助程式。 這些協助程式只會傳回字串。 您需要呼叫 response.write （），以呈現至瀏覽器的字串。

使用 HTML 協助程式方法是選擇性的。 可以讓您的生活更容易藉由縮減的 HTML 和您要撰寫的指令碼。 表 5 中的檢視轉譯完全相同的形式列表 4 中的檢視，而不需使用 HTML 協助程式。

**列表 5-\Views\Home\Login.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample5.aspx)]

您也可以選擇建立您自己的 HTML 協助程式。 例如，您可以建立會自動顯示 HTML 表格中的一組資料庫記錄的 GridView() helper 方法。 我們在本教學課程中探索此主題**建立的自訂 HTML 協助程式**。

## <a name="using-view-data-to-pass-data-to-a-view"></a>使用將資料傳遞至檢視的檢視資料

您可以使用 檢視資料來將資料從控制器傳遞至檢視。 將您透過郵件傳送與封裝相似的檢視資料。 必須使用這個套件傳送所有資料從控制器傳遞至檢視。 例如，列表 6 中的控制站新增一則訊息檢視資料。

**列表 6-ProductController.cs**

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample6.cs)]

控制器 ViewData 屬性表示名稱 / 值組的集合。 在列表 6 中，index （） 方法會將項目加入至名為具有值為 Hello World 訊息的檢視資料收集 ！。 Index （） 方法所傳回的檢視時，檢視資料會自動傳遞至檢視。

列表 7 中的檢視的檢視資料中擷取訊息，並呈現至瀏覽器的訊息。

**列表 7-\Views\Product\Index.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample7.aspx)]

請注意檢視在轉譯的訊息時，會採用 Html.Encode() HTML 協助程式方法的優點。 Html.Encode() HTML 協助程式的特殊字元編碼這類&lt;和&gt;成可放心地顯示在網頁中的字元。 每當您呈現使用者提交到網站的內容時，您應該編碼內容，以防止 JavaScript 插入式攻擊。

(因為我們建立訊息自行 ProductController 中，我們不 t 是否真的要對訊息進行編碼。 不過，它是很好的習慣，以顯示內容從檢視中檢視資料擷取時，務必呼叫 Html.Encode() 方法）。

在列表 7 中，我們將從控制器的簡單字串訊息傳遞至檢視的檢視資料的優點。 您也可以使用檢視的資料來傳遞其他類型的資料，例如資料庫記錄，從控制器到檢視的集合。 比方說，如果您要在檢視中，顯示產品資料庫資料表的內容，則您會傳遞集合資料庫的記錄檢視中的資料。

您也可以選擇將強型別的檢視資料從控制器傳遞至檢視。 我們在本教學課程中探索此主題**了解強型別檢視資料和檢視表**。

## <a name="summary"></a>總結

本教學課程簡短介紹 ASP.NET MVC 檢視、 檢視資料和 HTML 協助程式。 在第一個區段中，您已了解如何將新的檢視新增至您的專案。 您已了解您必須新增檢視正確的資料夾以從特定的控制器呼叫它。 接下來，我們討論過 HTML 協助程式的主題。 您已了解如何 HTML 協助程式可讓您輕鬆地產生標準的 HTML 內容。 最後，您已了解如何利用將資料從控制器傳遞至檢視的檢視資料。

> [!div class="step-by-step"]
> [下一步](creating-custom-html-helpers-cs.md)
