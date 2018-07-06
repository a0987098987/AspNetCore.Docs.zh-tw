---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: 轉譯 ASP.NET Web Pages (Razor) 網站的行動裝置 |Microsoft Docs
author: tfitzmac
description: 本文說明如何建立會適當地呈現在 行動裝置的 ASP.NET Web Pages (Razor) 網站中的頁面。 您將學到什麼： 您如何...
ms.author: aspnetcontent
ms.date: 02/17/2014
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: dcc44e0d78814ef9a8537b01efa17259a12e9c76
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816339"
---
<a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a>行動裝置轉譯 ASP.NET Web Pages (Razor) 網站
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> 本文說明如何建立會適當地呈現在 行動裝置的 ASP.NET Web Pages (Razor) 網站中的頁面。
> 
> 您將學到什麼：
> 
> - 如何使用命名慣例，以指定的頁面被專為行動裝置。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> 本教學課程也適用於 ASP.NET Web Pages 2。


ASP.NET Web Pages 可讓您在行動裝置或其他裝置上建立自訂的顯示，來呈現內容。

在 ASP.NET Web Pages 網站中建立裝置的特定頁面的最簡單方式是藉由使用檔案命名模式如下：<em>檔名。</em><em>行動</em><em>.cshtml</em>。 您可以建立兩個版本的頁面 (例如，一個名為<em>MyFile.cshtml</em>一個名為<em>MyFile.Mobile.cshtml</em>)。 在執行的階段，當行動裝置的要求<em>MyFile.cshtml</em>，ASP.NET 會呈現從內容<em>MyFile.Mobile.cshtml</em>。 否則，請<em>MyFile.cshtml</em>轉譯。

下列範例示範如何藉由新增行動裝置的內容頁面中啟用行動裝置的轉譯。 *Page1.cshtml*包含內容，加上導覽提要欄位。 *Page1.Mobile.cshtml*包含相同的內容，但省略了資訊看板。

1. 在 ASP.NET Web Pages 網站中，建立名為*Page1.cshtml* ，並以下列標記取代目前的內容。

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. 建立名為*Page1.Mobile.cshtml* ，並以下列標記取代現有的內容。 請注意頁面的行動裝置的版本會省略更好的呈現較小螢幕上的 [導覽] 區段。

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. 執行桌面瀏覽器並瀏覽至*Page1.cshtml*。 ![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)
4. 執行行動瀏覽器 （或行動裝置模擬器），並瀏覽至*Page1.cshtml*。 (請注意，不包括 *.mobile。* 為 URL 的一部分。）即使要求是要*Page1.cshtml*，ASP.NET 會呈現*Page1.Mobile.cshtml*。

    ![mobilesites-2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> 若要測試行動網頁，您可以使用行動裝置模擬器，在桌面的電腦上執行。 此工具可讓您測試網頁，因為它們看起來在行動裝置上 （也就是通常具有較小顯示區域）。 模擬器的其中一個範例是[使用者代理程式切換器附加元件](http://addons.mozilla.org/firefox/addon/user-agent-switcher/)適用於 Mozilla Firefox，可讓您模擬各種不同的行動瀏覽器，從 Firefox 的桌面版本。


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>其他資源


[Windows Phone 模擬器](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)
