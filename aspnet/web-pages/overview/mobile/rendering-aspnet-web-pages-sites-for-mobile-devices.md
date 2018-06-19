---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: 呈現 ASP.NET Web Pages (Razor) 站台行動裝置 |Microsoft 文件
author: tfitzmac
description: 本文說明如何建立會適當地呈現在行動裝置的 ASP.NET Web Pages (Razor) 網站中的網頁。 您將學習： 您如何...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: 641798c5b835be959d02dd0d854b61ca21d83016
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30897315"
---
<a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a>呈現 ASP.NET Web Pages (Razor) 站台的行動裝置
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 本文說明如何建立會適當地呈現在行動裝置的 ASP.NET Web Pages (Razor) 網站中的網頁。
> 
> 您將學習：
> 
> - 如何使用命名慣例，以指定頁面所專用的行動裝置。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> 本教學課程也適用於 ASP.NET Web Pages 2。


ASP.NET Web 網頁可讓您建立來呈現內容的自訂顯示行動裝置或其他裝置上。

在 ASP.NET Web Pages 站台中建立裝置的特定頁面的最簡單方式是使用檔案命名模式如下：<em>檔名。</em><em>行動</em><em>.cshtml</em>。 您可以建立兩個版本的網頁 (例如，一個名為<em>MyFile.cshtml</em> ，而另一個名為<em>MyFile.Mobile.cshtml</em>)。 在執行的階段，當行動裝置的要求<em>MyFile.cshtml</em>，ASP.NET 會呈現從內容<em>MyFile.Mobile.cshtml</em>。 否則， <em>MyFile.cshtml</em>轉譯。

下列範例會示範如何藉由新增行動裝置的內容頁面中啟用行動裝置的轉譯。 *Page1.cshtml*包含內容加上導覽提要欄位。 *Page1.Mobile.cshtml*包含相同的內容，但會省略 [資訊看板]。

1. 在 ASP.NET Web Pages 站台中，建立名為*Page1.cshtml* ，並以下列標記取代目前的內容。

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. 建立名為*Page1.Mobile.cshtml* ，並以下列標記取代現有的內容。 請注意行動版的頁面會省略更好的呈現較小螢幕上的瀏覽區段。

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. 執行桌面瀏覽器並瀏覽至*Page1.cshtml*。 ![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)
4. 執行行動瀏覽器 （或行動裝置模擬器），並瀏覽至*Page1.cshtml*。 (請注意，不包含 *.mobile。* 做為 URL 的一部分。）即使該要求是*Page1.cshtml*，ASP.NET 會呈現*Page1.Mobile.cshtml*。

    ![mobilesites-2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> 若要測試行動頁面，您可以使用行動裝置模擬器，桌面的電腦上執行。 此工具可讓您測試網頁，因為它們會在行動裝置上看起來 （也就是通常具有較小顯示區域）。 模擬器的其中一個範例是[使用者代理程式切換器附加元件](http://addons.mozilla.org/firefox/addon/user-agent-switcher/)Mozilla Firefox，這可讓您模擬 Firefox 桌面版本從各種行動瀏覽器。


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>其他資源


[Windows Phone 模擬器](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)
