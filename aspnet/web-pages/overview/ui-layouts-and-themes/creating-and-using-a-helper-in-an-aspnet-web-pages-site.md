---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: 建立和使用協助程式中 ASP.NET Web Pages (Razor) 站台 |Microsoft 文件
author: tfitzmac
description: 本文說明如何在 ASP.NET Web Pages (Razor) 網站上建立協助程式。 協助程式是可重複使用的元件，其中包含程式碼和效能標記...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 5d0c1ae09d8fbc91ff76cd4045d439abafee7736
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
ms.locfileid: "26529997"
---
<a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a>建立和使用 ASP.NET Web Pages (Razor) 網站中的協助程式
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 本文說明如何在 ASP.NET Web Pages (Razor) 網站上建立協助程式。 A *helper*是可重複使用的元件，包括程式碼和標記，以執行可能很繁瑣或是複雜的工作。
> 
> **您將學習：** 
> 
> - 如何建立及使用簡單的 helper。
> 
> 這些是發行項中所引進的 ASP.NET 功能：
> 
> - `@helper`語法。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> 本教學課程也適用於 ASP.NET Web Pages 2。


## <a name="overview-of-helpers"></a>協助程式的概觀

如果您需要在您的網站不同頁面上執行相同的工作，您可以使用協助程式。 ASP.NET Web Pages 包含數個 helper，而且還有更多，您可以下載並安裝。 (ASP.NET Web Pages 中內建的協助程式的清單會列於[ASP.NET 應用程式開發介面的快速參考](https://go.microsoft.com/fwlink/?LinkId=202907)。)如果沒有現有的協助程式符合您的需求，您可以建立您自己的 helper。

協助程式可讓您使用一般程式碼區塊，跨多個頁面。 假設，在您的網頁通常想要建立附註項目，除了一般段落格式設定。 請注意可能會建立為`<div>`項目，具有樣式為框線方塊。 而非每次您想要顯示一個提示，請將這個相同的標記加入頁面，您就可以封裝標記成 helper。 然後，您就可以插入一行程式碼用在您需要的任何位置。

使用像這樣的 helper 在每個頁面中讓程式碼，更簡單且容易閱讀。 它可更輕鬆地維護您的網站，因為如果您需要變更便箋的外觀，您可以變更在一個位置的標記。

## <a name="creating-a-helper"></a>建立 Helper

此程序會示範如何建立建立便箋，如上所述的協助程式。 這是簡單的範例，但自訂 helper 可以包含任何標記，且您需要的 ASP.NET 程式碼。

1. 在網站的根資料夾中建立資料夾，名為*應用程式\_程式碼*。 這是您可以在其中放置像協助程式元件的程式碼在 ASP.NET 中保留的資料夾名稱。
2. 在*應用程式\_程式碼*資料夾建立新 *.cshtml*檔案並將其命名*MyHelpers.cshtml*。
3. 以下列內容取代現有的內容：

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    程式碼會使用`@helper`語法以宣告新的協助程式，名為`MakeNote`。 此特定的協助程式可讓您將名為參數傳遞`content`，可以包含文字與標記的組合。 協助專家將字串插入注意本文會使用`@content`變數。

    請注意檔案名為*MyHelpers.cshtml*，但是協助專家名為`MakeNote`。 您可以將多個自訂協助程式放入單一檔案。
4. 儲存並關閉檔案。

## <a name="using-the-helper-in-a-page"></a>在網頁中使用 Helper

1. 在根資料夾中建立新的空白檔案，稱為*TestHelper.cshtml*。
2. 將下列程式碼加入檔案：

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    若要呼叫您建立的協助，請使用`@`後面的檔案名稱，協助專家，所在的點，再接著的協助程式名稱。 (如果您有多個資料夾*應用程式\_程式碼*資料夾中，您可以使用語法`@FolderName.FileName.HelperName`呼叫您的協助專家內任何巢狀資料夾層級)。 您加入以引號括號內的文字是協助專家將一部分請注意，在網頁上顯示的文字。
3. 儲存頁面，並在瀏覽器中執行。 協助專家會產生附註項目右您用來呼叫 helper： 之間兩個段落。

    ![在瀏覽器，並協助專家產生標記，放入指定的文字周圍方塊的方式顯示頁面的螢幕擷取畫面。](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.jpg)

## <a name="additional-resources"></a>其他資源


[水平 功能表做 Razor helper](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341)。 Mike 教宗此部落格文章會示範如何建立與協助專家使用標記、 CSS 和程式碼的水平的功能表。

[利用 HTML5 中 ASP.NET Web Pages Helper for WebMatrix 及 ASP.NET MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx)。 由 Sam 林肯此部落格文章示範 helper 呈現 HTML5`Canvas`項目。

[之間的差異@Helpers和@Functions在 WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix)。 Mike Brind 此部落格文章說明`@helper`語法和`@function`語法以及何時使用每個。
