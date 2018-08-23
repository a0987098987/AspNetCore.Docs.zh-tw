---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: 建立和使用協助程式中 ASP.NET Web Pages (Razor) 網站 |Microsoft Docs
author: tfitzmac
description: 本文說明如何在 ASP.NET Web Pages (Razor) 網站上建立協助程式。 協助程式是一種可重複使用的元件，包括程式碼和標記，以效能...
ms.author: riande
ms.date: 02/17/2014
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: a6d3074f39a1d7b81b173813a73380fe2a536e7a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834701"
---
<a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a>建立和使用 ASP.NET Web Pages (Razor) 網站中的協助程式
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> 本文說明如何在 ASP.NET Web Pages (Razor) 網站上建立協助程式。 A*協助程式*是一種可重複使用的元件，包括程式碼和標記，以執行可能冗長或複雜的工作。
> 
> **您將學到什麼：** 
> 
> - 如何建立及使用簡單的協助程式。
> 
> 以下是所引進的發行項 ASP.NET 功能：
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

如果您需要在您的網站不同頁面上執行相同的工作，您可以使用協助程式。 ASP.NET Web 網頁包含數個協助程式，而且有更多，您可以下載並安裝。 (一份內建的協助程式 ASP.NET Web Pages 中列入[ASP.NET API 快速參考](https://go.microsoft.com/fwlink/?LinkId=202907)。)如果沒有現有的協助程式符合您的需求，您可以建立您自己的協助程式。

協助程式可讓您使用的常見程式碼區塊，跨多個頁面。 假設您的頁面中您通常要建立附註項目，除了一般的段落設定。 附註可能會建立為`<div>`項目，有樣式設定為具有框線的方塊。 而不是每次您想要的方式顯示便箋，請將這個相同的標記新增至頁面中，您就可以封裝標記做為協助程式。 然後，您就可以插入一行程式碼與附註您需要的任何位置。

使用像這樣的協助程式在每個頁面中會讓程式碼，更簡單且容易閱讀。 它也可讓您更輕鬆地維護您的網站，因為如果您需要變更便箋的外觀，您可以變更一個位置中的標記。

## <a name="creating-a-helper"></a>建立 Helper

此程序會示範如何建立協助程式會建立附註，如上所述。 這是一個簡單的範例，但自訂協助程式可以包含任何標記和您所需的 ASP.NET 程式碼。

1. 在網站的根資料夾中，建立名為*應用程式\_程式碼*。 這是您可以在其中放置例如協助程式元件的程式碼在 ASP.NET 中保留的資料夾名稱。
2. 在 *應用程式\_程式碼*資料夾中建立的新 *.cshtml*檔案並將它命名*MyHelpers.cshtml*。
3. 以下列內容取代現有的內容：

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    程式碼會使用`@helper`語法來宣告名為新的協助程式`MakeNote`。 此特定協助程式可讓您將名為參數傳遞`content`可包含文字和標記的組合。 協助程式會將字串插入便箋內文使用`@content`變數。

    請注意，檔案名稱為*MyHelpers.cshtml*，但是協助專家名為`MakeNote`。 您可以將多個自訂的協助程式放入單一檔案。
4. 儲存並關閉檔案。

## <a name="using-the-helper-in-a-page"></a>在網頁中使用協助程式

1. 在根資料夾中，建立新的空白檔案，稱為*TestHelper.cshtml*。
2. 將下列程式碼加入檔案：

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    若要呼叫您所建立的協助程式，使用`@`後面接著檔案名稱，協助程式所在位置，一個點，然後按一下 是協助程式名稱。 (如果您有多個資料夾*應用程式\_程式碼*資料夾中，您可以使用語法`@FolderName.FileName.HelperName`呼叫您的協助專家內任何巢狀資料夾層級)。 您加入以引號括號內的文字是協助程式會顯示過程中網頁的附註文字。
3. 儲存頁面，並在瀏覽器中執行它。 協助程式會產生附註項目直接呼叫的協助程式的位置： 這兩個段落。

    ![在瀏覽器，並協助程式產生的標記，將指定的文字周圍方塊的方式顯示頁面的螢幕擷取畫面。](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.jpg)

## <a name="additional-resources"></a>其他資源


[做為 Razor 協助程式的水平功能表](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341)。 由 Mike 主教此部落格文章會示範如何建立水平功能表做為使用標記、 CSS 和程式碼的協助程式。

[利用 HTML5，在 ASP.NET Web Pages Helper for WebMatrix 及 ASP.NET MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx)。 此部落格文章，Sam abraham （英文） 所顯示的協助程式呈現 HTML5`Canvas`項目。

[之間的差異@Helpers並@Functions在 WebMatrix 中](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix)。 由 Mike Brind 此部落格文章說明`@helper`語法和`@function`語法和使用時機。
