---
uid: mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
title: "ASP.NET MVC 檢視概觀 (C#) |Microsoft 文件"
author: StephenWalther
description: "什麼是 ASP.NET MVC 檢視，以及如何不同的 HTML 網頁？ 在本教學課程中，作者： Stephen Walther 為您介紹檢視，並示範如何 t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: 152ab1e5-aec2-4ea7-b8cc-27a24dd9acb8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: 9de095b0621af3b6166a2e1cbcb1c63c26a88aa2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-views-overview-c"></a>ASP.NET MVC 檢視概觀 (C#)
====================
由[Stephen Walther](https://github.com/StephenWalther)

> 什麼是 ASP.NET MVC 檢視，以及如何不同的 HTML 網頁？ 在本教學課程中，作者： Stephen Walther 為您介紹檢視，並示範如何使用的檢視資料和檢視表中的 HTML Helper 利用。


本教學課程的目的是為您提供簡介 ASP.NET MVC 檢視、 檢視資料和 HTML Helper。 本教學課程結束時，您應該了解如何建立新的檢視、 傳遞資料從控制器到檢視，以及用來產生內容檢視中的 HTML Helper。

## <a name="understanding-views"></a>了解檢視

針對 ASP.NET 或動態伺服器網頁，ASP.NET MVC 不包含任何項目會直接對應至頁面。 ASP.NET MVC 應用程式中，沒有任何頁面對應至您在您的瀏覽器的網址列輸入的 url 路徑的磁碟上。 在 ASP.NET MVC 應用程式頁面的最接近動作是呼叫*檢視*。

ASP.NET MVC 應用程式中，連入的瀏覽器要求對應至控制器的動作。 控制器動作，可能會傳回檢視表。 不過，控制器動作，可能會執行其他類型的動作，例如將您重新導向至另一個控制器的動作。

清單 1 包含名為 HomeController 簡單控制器。 HomeController 公開 （expose) 名為 index （） 和 Details() 兩個控制器的動作。

**列出 1-HomeController.cs**

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample1.cs)]

您可以呼叫的第一個動作，index （） 的動作，您的瀏覽器網址列中輸入下列 URL:

/ Home/Index

您可以在您的瀏覽器中輸入此位址，叫用第二個動作，Details() 動作：

/ 主/詳細資料

Index 動作傳回的檢視。 您所建立的大部分動作將會傳回檢視表。 但是，動作可以傳回其他類型的動作結果。 例如，Details() 動作將會傳回傳入的要求重新導向至 index 動作 RedirectToActionResult。

Index 動作包含下列一行程式碼：

View();

這行程式碼會傳回必須位於下列路徑在您的網頁伺服器上的檢視：

\Views\Home\Index.aspx

檢視的路徑是從控制器名稱和控制器動作的名稱來推斷。

如果您想要的話，您可以明確了解檢視。 下列程式碼會傳回名為 Fred 的檢視：

檢視 (Fred);

執行這行程式碼時，檢視會傳回從下列路徑：

\Views\Home\Fred.aspx

> [!NOTE] 
> 
> 如果您打算建立 ASP.NET MVC 應用程式的單元測試它是個不錯的主意明確了解檢視名稱。 這樣一來，您可以建立單元測試以確認控制器動作未傳回預期的檢視。


## <a name="adding-content-to-a-view"></a>將內容加入至檢視

檢視是一種標準 (X) 可以包含指令碼的 HTML 文件。 您可以使用指令碼新增至檢視動態內容。

例如，列表 2 中的檢視會顯示目前的日期和時間。

**列出 2-\Views\Home\Index.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample2.aspx)]

請注意，清單 2 中的 HTML 網頁的主體會包含下列指令碼：

&lt;%Response.write(datetime.now); %&gt;

使用指令碼分隔符號&lt;%和 %&gt;標記開頭和結尾的指令碼。 此指令碼是以 C# 撰寫。 它會顯示目前的日期和時間，藉由呼叫 Response.write 方法轉譯至瀏覽器的內容。 指令碼分隔符號&lt;%和 %&gt;可用來執行一或多個陳述式。

因為您經常呼叫 Response.write，Microsoft 提供您與快速鍵呼叫 Response.write 方法。 範例 3 中的檢視使用分隔符號&lt;%= 和 %&gt;快捷 Response.write 函式呼叫。

**列出 3-Views\Home\Index2.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample3.aspx)]

您可以使用任何.NET 語言來產生動態內容檢視中。 一般來說，就會使用 Visual Basic.NET 或 C# 來撰寫您的控制器和檢視。

## <a name="using-html-helpers-to-generate-view-content"></a>使用 HTML Helper 來產生檢視內容

若要讓您更輕鬆地將內容加入檢視，您可以利用所謂*HTML Helper*。 HTML Helper，通常是產生的字串的方法。 您可以使用 HTML Helper 來產生標準 HTML 項目，例如文字方塊、 連結、 下拉式清單和清單方塊。

例如，在三個 HTML Helper-列出 4 利用檢視 BeginForm()、 TextBox() 和 Password() helper-產生登入表單 （請參閱圖 1）。

**列出 4-\Views\Home\Login.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample4.aspx)]


[![新增專案 對話方塊](asp-net-mvc-views-overview-cs/_static/image1.jpg)](asp-net-mvc-views-overview-cs/_static/image1.png)

**圖 01**： 標準登入表單 ([按一下以檢視完整大小的影像](asp-net-mvc-views-overview-cs/_static/image2.png))


所有 HTML Helper 方法會呼叫檢視的 Html 屬性。 比方說，您會藉由呼叫 Html.TextBox() 方法呈現文字方塊。

請注意，您會使用指令碼分隔符號&lt;%= 和 %&gt;呼叫 Html.TextBox() 和 Html.Password() helper 時。 這些 helper 就只會傳回字串。 您需要呼叫 Response.write 才能轉譯至瀏覽器的字串。

使用 HTML Helper 方法是選擇性的。 它們會簡化您的生活減少的 HTML 和需要撰寫的指令碼。 列出 5 中的檢視而不需使用 HTML Helper 呈現完全相同的形式中列出的 4 的檢視。

**列出 5-\Views\Home\Login.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample5.aspx)]

您也可以選擇建立您自己的 HTML Helper。 例如，您可以建立自動 HTML 表格中顯示的一組資料庫記錄的 GridView() helper 方法。 我們在本教學課程中瀏覽本主題**建立自訂 HTML Helper**。

## <a name="using-view-data-to-pass-data-to-a-view"></a>將資料傳遞至檢視中使用檢視資料

您可以使用檢視資料將從控制器的資料傳遞至檢視。 將檢視資料，例如您透過郵件傳送的封裝。 必須使用這個套件傳送傳遞從控制器到檢視的所有資料。 例如，程式碼範例 6 中的控制站新增一則訊息檢視資料。

**列出 6-ProductController.cs**

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample6.cs)]

控制器別的 ViewData 屬性代表名稱 / 值組的集合。 列出第 6 index （） 方法會將項目加入至檢視資料收集，名為訊息的值為 Hello World ！。 檢視 index （） 方法傳回時，檢視資料會自動傳遞到檢視。

列出 7 的檢視從檢視資料擷取訊息，並呈現至瀏覽器的訊息。

**列出 7-\Views\Product\Index.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample7.aspx)]

請注意，檢視利用 Html.Encode() HTML Helper 方法轉譯訊息時。 Html.Encode() HTML Helper 的特殊字元編碼例如&lt;和&gt;成安全的網頁中顯示的字元。 每當您轉譯使用者提交到網站的內容，您應該編碼內容，以防止 JavaScript 插入式攻擊。

(因為我們訊息自行建立 ProductController 中，我們不需要對訊息進行編碼。 不過，它是個好習慣以顯示內容從檢視表中的檢視資料擷取時請務必呼叫 Html.Encode() 方法）。

在列出 7 中，我們利用檢視来傳遞的資料與簡單的字串訊息從控制器到檢視。 您也可以使用檢視資料，將其他類型的資料，例如資料庫記錄，從控制器到檢視的集合。 例如，如果您想要在檢視中，顯示產品資料庫資料表的內容則將集合資料庫的記錄檢視中資料。

您也可以選擇從控制器的強型別的檢視資料傳遞至檢視。 我們在本教學課程中瀏覽本主題**了解強型別檢視資料和檢視表**。

## <a name="summary"></a>總結

本教學課程提供簡介 ASP.NET MVC 檢視、 檢視資料和 HTML Helper。 在第一個區段中，您將學會如何將新的檢視加入至您的專案。 您已了解，您必須加入檢視到正確的資料夾才能呼叫從特定的控制站。 接下來，我們將討論主題的 HTML Helper。 您已學習如何 HTML Helper 可讓您輕鬆地產生標準 HTML 內容。 最後，您會學到如何利用將從控制器的資料傳遞至檢視的檢視資料。

>[!div class="step-by-step"]
[下一步](creating-custom-html-helpers-cs.md)
