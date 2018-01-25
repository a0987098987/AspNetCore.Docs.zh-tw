---
uid: mvc/overview/older-versions/aspnet-mvc-4-mobile-features
title: "ASP.NET MVC 4 行動功能 |Microsoft 文件"
author: Rick-Anderson
description: "現在是在部署 ASP.NET MVC 5 Mobile Web 應用程式在 Azure 網站的程式碼範例使用本教學課程的 MVC 5 版本。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: 27dc4fc8-1b51-43b0-933f-fc1b52476523
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/aspnet-mvc-4-mobile-features
msc.type: authoredcontent
ms.openlocfilehash: d47d8f61dc7af6e1dc5887338be862ea81d7bb17
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-mvc-4-mobile-features"></a>ASP.NET MVC 4 Mobile 功能
====================
由[Rick Anderson](https://github.com/Rick-Anderson)

> 現在是使用程式碼範例，在本教學課程的 MVC 5 版本[ASP.NET MVC 5 Mobile Web 應用程式部署在 Azure 網站](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/)。


本教學課程將告訴您如何使用 ASP.NET MVC 4 Web 應用程式中的行動裝置功能的基本概念。 此教學課程中，您可以使用[Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express)或 Visual Web Developer 2010 Express Service Pack 1 (&quot;Visual Web Developer 或 VWD&quot;)。 如果您已經有的您可以使用 Visual Studio professional 版。

開始之前，請確定您已安裝下面所列的必要條件。

- [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) （建議選項） 或 Visual Studio Web Developer Express SP1。 Visual Studio 2012 包含 ASP.NET MVC 4。 如果您使用 Visual Web Developer 2010，您必須安裝[ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)。

您也需要行動電話瀏覽器模擬器。 下列其中一項工作將會：

- [Windows 7 Phone 模擬器](https://msdn.microsoft.com/library/ff402563(VS.92).aspx)。 （這是用於此教學課程中大部分的螢幕擷取畫面中的模擬器）。
- 變更模擬 iPhone 的使用者代理字串。 請參閱[這](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/)部落格文章。
- [Opera Mobile 模擬器](http://www.opera.com/developer/tools/mobile/)
- [Apple Safari](http://www.apple.com/safari/download/)設 iPhone 使用者代理程式。 如需如何將使用者代理程式在 Safari 中設定為"iPhone 」 的指示，請參閱[如何讓 Safari 假裝其 IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) David Alison 部落格上。

使用 C# 原始程式碼的 visual Studio 專案會隨附本主題：

- [入門專案下載](https://go.microsoft.com/fwlink/?linkid=228307&amp;clcid=0x409)
- [完成專案下載](https://go.microsoft.com/fwlink/?linkid=228306&amp;clcid=0x409)

### <a name="what-youll-build"></a>您將建置

此教學課程中，您會加入 mobile 功能中提供的簡單會議清單應用程式至[入門專案](https://go.microsoft.com/fwlink/?LinkId=228307)。 下列螢幕擷取畫面會顯示完成的應用程式的標記頁面中所見[Windows 7 Phone 模擬器](https://msdn.microsoft.com/library/ff402563(VS.92).aspx)。 請參閱[鍵盤對應的 Windows Phone 模擬器](https://msdn.microsoft.com/library/ff754352(v=vs.92).aspx)簡化鍵盤輸入。

[![p1_Tags_CompletedProj](aspnet-mvc-4-mobile-features/_static/image2.png)](aspnet-mvc-4-mobile-features/_static/image1.png)

您可以使用 Internet Explorer 9 或 10、 FireFox 或 Chrome 開發所設定的行動應用程式版本[使用者代理字串](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/)。 下圖顯示已完成本教學課程，並使用模擬 iPhone 的 Internet Explorer。 您可以使用 Internet Explorer F-12 開發者工具與[Fiddler 工具](http://www.fiddler2.com/fiddler2/)協助偵錯您的應用程式。

![](aspnet-mvc-4-mobile-features/_static/image3.png)

### <a name="skills-youll-learn"></a>您將學習到的技術

以下是您將學習：

- ASP.NET MVC 4 範本如何使用 HTML5`viewport`屬性和調整呈現，若要改善顯示行動裝置上。
- 如何建立行動裝置的特定檢視。
- 如何建立檢視切換器，可讓使用者切換行動檢視和應用程式的桌面檢視。

### <a name="getting-started"></a>快速入門

下載會議清單應用程式入門專案，使用下列連結：[下載](https://go.microsoft.com/fwlink/?LinkId=228307)。 然後在 Windows 檔案總管中，以滑鼠右鍵按一下*MvcMobile.zip*檔案，然後選擇**屬性**。 在**MvcMobile.zip 屬性**對話方塊方塊中，選擇**解除封鎖** 按鈕。 (解除封鎖可避免安全性警告，當您嘗試使用時，就會發生*.zip*您已經從 web 下載的檔案。)

![p1_unBlock](aspnet-mvc-4-mobile-features/_static/image4.png)

以滑鼠右鍵按一下*MvcMobile.zip*檔案，然後選取**全部解壓縮**來解壓縮檔案。 在 Visual Studio 中開啟*MvcMobile.sln*檔案。

按 CTRL + F5 執行應用程式，將會顯示在您的桌面瀏覽器中。 啟動您的行動電話瀏覽器模擬器，將會議應用程式的 URL 複製到模擬器，然後按一下**標記來瀏覽**連結。 如果您使用 Windows Phone 模擬器，在 URL 橫條中按一下，然後按鍵盤存取 Pause 鍵。 下的圖顯示*AllTags*檢視 (從選擇**標記來瀏覽**)。

[![p1_browseTag](aspnet-mvc-4-mobile-features/_static/image6.png)](aspnet-mvc-4-mobile-features/_static/image5.png)

顯示是很容易閱讀，行動裝置上。 選擇 ASP.NET 連結。

[![p1_tagged_ASPNET](aspnet-mvc-4-mobile-features/_static/image8.png)](aspnet-mvc-4-mobile-features/_static/image7.png)

ASP.NET 標記檢視是得雜亂。 例如，**日期**資料行是非常難以閱讀。 稍後在本教學課程中，您將建立的版本*AllTags*檢視，是專為行動瀏覽器，並將會成為可讀取的顯示。

注意： 目前 bug 存在行動的快取引擎中。 對於生產應用程式，您必須安裝[固定 DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) nugget 封裝。 請參閱[ASP.NET MVC 4 行動快取 Bug 固定](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)如修正程式的詳細資訊。

## <a name="css-media-queries"></a>CSS 媒體查詢

[CSS 媒體查詢](http://www.w3.org/TR/css3-mediaqueries/)是 CSS 的媒體類型的擴充功能。 它們可讓您建立覆寫預設 CSS 規則的特定瀏覽器 （使用者代理程式） 的規則。 CSS 行動瀏覽器為目標的一般規則定義的最大螢幕大小。 *Content\Site.css*當您建立新的 ASP.NET MVC 4 網際網路專案時所建立的檔案包含下列的媒體查詢：

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample1.css)]

如果瀏覽器視窗中是 850 像素寬或較少，則會使用此媒體區塊內的 CSS 規則。 您可以使用像這樣的 CSS 媒體查詢提供較寬的桌面瀏覽器顯示 HTML 內容的更佳顯示小的瀏覽器 （例如行動瀏覽器） 比預設 CSS 規則所設計。

## <a name="the-viewport-meta-tag"></a>檢視區的中繼標籤

大部分行動瀏覽器定義虛擬瀏覽器視窗寬度 (*檢視區*)，會遠超過在行動裝置的實際寬度。 這可讓行動瀏覽器，以符合整個網頁內虛擬顯示器。 使用者可以再放大有趣的內容。 不過，如果您的檢視區寬度設定為在實際裝置寬度時，沒有縮放是必要項目，因為內容可以放入行動瀏覽器中。

檢視區`<meta>`ASP.NET MVC 4 版面配置檔案中的標記將檢視區設定為裝置寬度。 下列的線條將會顯示在檢視區`<meta>`ASP.NET MVC 4 版面配置檔案中的標記。

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample2.html)]

## <a name="examining-the-effect-of-css-media-queries-and-the-viewport-meta-tag"></a>檢查 CSS 媒體查詢和檢視區 Meta 標記的效果

開啟*_layout.cshtml\\_Layout.cshtml*編輯器和檢視區外的註解中的檔案`<meta>`標記。 下列標記顯示標記為註解。

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample3.cshtml)]

開啟*MvcMobile\Content\Site.css*檔案在編輯器中，並將媒體查詢中的最大寬度變更為零像素。 這會防止行動瀏覽器中使用 CSS 規則。 下面顯示的已修改的媒體查詢：

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample4.css)]

儲存您的變更，並瀏覽至會議中的應用程式的行動電話瀏覽器模擬器。 在下圖中的小字是移除檢視區的結果`<meta>`標記。 沒有檢視區與`<meta>`標籤中，瀏覽器會縮小為預設檢視區寬度 (850 像素或更多的大部分行動瀏覽器。)

[![p1_noViewPort](aspnet-mvc-4-mobile-features/_static/image10.png)](aspnet-mvc-4-mobile-features/_static/image9.png)

復原您的變更，請取消註解檢視區`<meta>`標記配置檔案中，並還原 850 像素的媒體查詢*Site.css*檔案。 儲存變更並重新整理行動瀏覽器，以確認行動設備友善顯示已經還原。

檢視區`<meta>`標記和 CSS 的媒體查詢不特定的 ASP.NET MVC 4，且您可在任何 web 應用程式中使用這些功能。 但現在建您建立新的 ASP.NET MVC 4 專案時，所產生的檔案。

如需有關檢視區`<meta>`標記中，請參閱[故事的兩個檢視區，第二部分](http://www.quirksmode.org/mobile/viewports2.html)。

下一節中，您會看到如何提供行動裝置瀏覽器的特定檢視。

## <a name="overriding-views-layouts-and-partial-views"></a>覆寫檢視、 配置和部分檢視

ASP.NET MVC 4 中重要的新功能是一個簡單的機制，可讓您覆寫任何檢視 （包括版面配置和部分檢視） 的行動瀏覽器，在一般情況下，針對個別的行動電話瀏覽器，或任何特定瀏覽器。 若要提供特定的行動檢視，您可以複製檢視檔案並加入*。Mobile*的檔案名稱。 例如，若要建立行動*索引*檢視中，複製*Views\Home\Index.cshtml*至*Views\Home\Index.Mobile.cshtml*。

在本節中，您將建立行動特定配置檔案。

若要開始，複製*_layout.cshtml\\_Layout.cshtml*至*_layout.cshtml\\_Layout.Mobile.cshtml*。 開啟 *\_Layout.Mobile.cshtml*並將變更從標題**MVC4 會議**至**會議 （行動裝置版）**。

在每個`Html.ActionLink`呼叫，移除 瀏覽方式 」 中每個連結*ActionLink*。 下列程式碼會顯示行動配置檔的已完成的主體區段。

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample5.cshtml)]

複製*Views\Home\AllTags.cshtml*檔案*Views\Home\AllTags.Mobile.cshtml*。 開啟新的檔案，並變更`<h2>`項目從 「 標記 」 到 「 標記 (M) 」:

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample6.html)]

瀏覽至使用桌面瀏覽器，並使用行動電話瀏覽器模擬器中的 [標記] 頁面。 行動電話瀏覽器模擬器顯示兩個您所做的變更。

[![p2m_layoutTags.mobile](aspnet-mvc-4-mobile-features/_static/image12.png)](aspnet-mvc-4-mobile-features/_static/image11.png)

相反地，未變更桌面顯示。

[![p2_layoutTagsDesktop](aspnet-mvc-4-mobile-features/_static/image14.png)](aspnet-mvc-4-mobile-features/_static/image13.png)

## <a name="browser-specific-views"></a>瀏覽器的特定檢視

除了特定的行動和桌面特定檢視中，您可以建立個別的瀏覽器的檢視。 例如，您可以建立專為 iPhone 瀏覽器的檢視。 在本節中，您將建立之 iPhone 瀏覽器和 iPhone 版本的版面配置*AllTags*檢視。

開啟*Global.asax*檔案，然後加入下列程式碼加入`Application_Start`方法。

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample7.cs)]

此程式碼會定義名為"iPhone"將會針對每個傳入要求符合新的顯示模式。 如果傳入要求符合您定義 （亦即，如果使用者代理程式包含字串"iPhone"） 的條件，ASP.NET MVC 會尋找其名稱中包含"iPhone"後置詞的檢視。

在程式碼中，以滑鼠右鍵按一下`DefaultDisplayMode`，選擇**解決**，然後選擇  `using System.Web.WebPages;`。 這將參考加入`System.Web.WebPages`命名空間，這是 where`DisplayModes`和`DefaultDisplayMode`類型定義。

[![p2_resolve](aspnet-mvc-4-mobile-features/_static/image16.png)](aspnet-mvc-4-mobile-features/_static/image15.png)

或者，您可以只手動新增下列這一行`using`檔的區段。

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample8.cs)]

完整內容*Global.asax*檔案如下所示。

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample9.cs)]

儲存變更。 複製*MvcMobile\Views\Shared\\_Layout.Mobile.cshtml*檔案*MvcMobile\Views\Shared\\_Layout.iPhone.cshtml*。 開啟新的檔案，然後變更`h1`標題從`Conference (Mobile)`至`Conference (iPhone)`。

複製*MvcMobile\Views\Home\AllTags.Mobile.cshtml*檔案*MvcMobile\Views\Home\AllTags.iPhone.cshtml*。 在新的檔案中，變更`<h2>`要"Tags (iPhone)"的項目從 「 標記 (M)"。

執行應用程式。 執行行動瀏覽器模擬器，請確定其使用者代理程式設定為"iPhone"，並瀏覽至*AllTags*檢視。 下列螢幕擷取畫面顯示*AllTags*中檢視轉譯[Safari](http://www.apple.com/safari/download/)瀏覽器。 您可以下載 Windows 的 Safari[這裡](https://support.apple.com/kb/DL1531)。

[![p2_iphoneView](aspnet-mvc-4-mobile-features/_static/image18.png)](aspnet-mvc-4-mobile-features/_static/image17.png)

本節中，我們已看到如何建立行動裝置的版面配置和檢視以及如何建立版面配置和特定裝置，例如 iPhone 的檢視。 下一節中，您會看到如何運用 jQuery Mobile 的更多強大的行動檢視。

## <a name="using-jquery-mobile"></a>使用 jQuery Mobile

[JQuery Mobile](http://jquerymobile.com/demos/1.0b3/#/demos/1.0b3/docs/about/intro.html)程式庫會提供適用於所有主要的行動瀏覽器的使用者介面架構。 jQuery Mobile 套用*漸進式增強功能*行動瀏覽器支援 CSS 和 JavaScript。 漸進式增強功能可讓所有瀏覽器同時允許更強大的瀏覽器及裝置有更豐富的顯示中顯示的網頁，基本的內容。 隨附於 jQuery Mobile 的 JavaScript 和 CSS 檔案設定許多項目以符合行動瀏覽器，但不變更標記樣式。

本節中，您將安裝*jQuery.Mobile.MVC* NuGet 封裝，這會安裝 jQuery Mobile 和檢視切換程式 widget。

若要開始，刪除*共用\\_Layout.Mobile.cshtml*和*共用\\_Layout.iPhone.cshtml*您稍早建立的檔案。

重新命名*Views\Home\AllTags.Mobile.cshtml*和*Views\Home\AllTags.iPhone.cshtml*檔案到*Views\Home\AllTags.iPhone.cshtml.hide*和*Views\Home\AllTags.Mobile.cshtml.hide*。 因為這些檔案不再有*.cshtml*擴充功能，它們不使用 ASP.NET MVC 執行階段呈現*AllTags*檢視。

安裝*jQuery.Mobile.MVC*藉此 NuGet 封裝：

1. 從**工具**功能表上，選取**程式庫套件管理員**，然後選取**Package Manager Console**。

    [![p3_packageMgr](aspnet-mvc-4-mobile-features/_static/image20.png)](aspnet-mvc-4-mobile-features/_static/image19.png)
2. 在**Package Manager Console**，輸入`Install-Package jQuery.Mobile.MVC -version 1.0.0`

下圖顯示加入和變更 MvcMobile 專案 NuGet jQuery.Mobile.MVC 封裝的檔案。 [新增] 的檔案名稱後面加入的檔案。 映像不會顯示 GIF 和 PNG 檔案新增至*Content\images*資料夾。

![](aspnet-mvc-4-mobile-features/_static/image21.png)

JQuery.Mobile.MVC NuGet 封裝會安裝下列項目：

- *應用程式\_Start\BundleMobileConfig.cs*必要參考加入 jQuery JavaScript 和 CSS 檔案的檔案。 您必須遵循下列指示，並參考這個檔案中定義的行動配套。
- jQuery Mobile CSS 檔案。
- A`ViewSwitcher`控制器 widget (*Controllers\ViewSwitcherController.cs*)。
- jQuery Mobile JavaScript 檔案。
- JQuery Mobile 樣式的配置檔案 (*_layout.cshtml\\_Layout.Mobile.cshtml*)。
- 檢視切換器的部分檢視*(MvcMobile\Views\Shared\\_ViewSwitcher.cshtml*)，提供從桌面檢視行動檢視，反之亦然切換每個頁面頂端的連結。
- 數個*.png*和*.gif*影像檔*Content\images*資料夾。

開啟*Global.asax*檔案，然後加入下列程式碼的最後一行`Application_Start`方法。

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample10.cs)]

下列程式碼顯示完整*Global.asax*檔案。

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample11.cs?highlight=26)]

> [!NOTE]
> 如果您使用 Internet Explorer 9，而且您沒有看到`BundleMobileConfig`以黃色反白顯示列上方，按一下[相容性檢視 按鈕](https://windows.microsoft.com/windows7/How-to-use-Compatibility-View-in-Internet-Explorer-9)![（關閉） 的相容性檢視 按鈕的圖片](http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "（關閉） 的相容性檢視 按鈕的圖片")讓變更從大綱圖示 ie ![（關閉） 的相容性檢視 按鈕的圖片](http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "（關閉） 的相容性檢視 按鈕的圖片")為純色![(on) 相容性檢視 按鈕的圖片](http://res1.windows.microsoft.com/resbox/en/Windows 7/main/156805ff-3130-481b-a12d-4d3a96470f36_14.jpg "(on) 相容性檢視 按鈕的圖片")。 或者，您可以在 FireFox 或 Chrome 檢視本教學課程。


開啟*MvcMobile\Views\Shared\\_Layout.Mobile.cshtml*檔案，然後加入下列標記直接之後`Html.Partial`呼叫：

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample12.cshtml)]

完整*MvcMobile\Views\Shared\\_Layout.Mobile.cshtml*檔案如下所示：

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample13.cshtml)]

建置應用程式，並在模擬器中您行動電話瀏覽器瀏覽至*AllTags*檢視。 您看到下列訊息：

[![p3_afterNuGet](aspnet-mvc-4-mobile-features/_static/image23.png)](aspnet-mvc-4-mobile-features/_static/image22.png)

> [!NOTE]
> 您可以將行動裝置的特定程式碼的偵錯[設定的使用者代理字串](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/)IE 或 Chrome 才能 iPhone，然後使用 F-12 開發人員工具。 如果未顯示您的行動瀏覽器**首頁**，**喇叭**，**標記**，和**日期**為按鈕的連結，jQuery Mobile 的參考指令碼和 CSS 檔案可能不是正確的。


除了樣式變更，您會看到**顯示行動檢視**以及可讓您從行動檢視切換到桌面檢視的連結。 選擇**桌面檢視**連結和桌面檢視會顯示。

[![p3_desktopView](aspnet-mvc-4-mobile-features/_static/image25.png)](aspnet-mvc-4-mobile-features/_static/image24.png)

桌面檢視不會提供方法來直接瀏覽回到行動檢視。 您可以立即修正。 開啟*_layout.cshtml\\_Layout.cshtml*檔案。 頁面的下方`body`項目，加入下列程式碼，會呈現檢視切換程式 widget:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample14.cshtml)]

重新整理*AllTags*行動瀏覽器中的檢視。 您現在可以桌面和行動裝置版的檢視之間巡覽。

[![p3_desktopViewWithMobileLink](aspnet-mvc-4-mobile-features/_static/image27.png)](aspnet-mvc-4-mobile-features/_static/image26.png)

> [!NOTE]
> 偵錯附註： 您可以將下列程式碼加入 _layout.cshtml 結尾\\_ViewSwitcher.cshtml 協助偵錯檢視，當使用瀏覽器使用者代理字串設定為行動裝置。
> 
> [!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample15.cs)]
> 
>  並加入下列的標題， *_layout.cshtml\\_Layout.cshtml*檔案。  
> 
> [!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample16.html)]


瀏覽至*AllTags*桌面瀏覽器中的頁面。 檢視切換程式小工具不會顯示在桌面瀏覽器，因為它只加入至行動裝置的版面配置頁面。 稍後在教學課程中，您會看到您，如何將檢視切換程式 widget 加入桌面檢視。

[![p3_desktopBrowser](aspnet-mvc-4-mobile-features/_static/image29.png)](aspnet-mvc-4-mobile-features/_static/image28.png)

## <a name="improving-the-speakers-list"></a>提高喇叭清單

在行動裝置瀏覽器中，選取**喇叭**連結。 因為沒有行動檢視 (*AllSpeakers.Mobile.cshtml*)，顯示預設喇叭 (*AllSpeakers.cshtml*) 使用行動裝置的版面配置檢視呈現 ( *\_Layout.Mobile.cshtml*)。

[![p3_speakersDeskTop](aspnet-mvc-4-mobile-features/_static/image31.png)](aspnet-mvc-4-mobile-features/_static/image30.png)

您可以藉由設定全域停用預設的 （非行動電話） 檢視行動配置內呈現`RequireConsistentDisplayMode`至`true`中*檢視\\_viewstart.vbhtml*檔案，就像這樣：

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample17.cshtml)]

當`RequireConsistentDisplayMode`設`true`，行動裝置的版面配置 (*\_Layout.Mobile.cshtml*) 只適用於行動檢視。 (亦即，檢視檔案是表單的 ***ViewName**。Mobile.cshtml*。)要設定`RequireConsistentDisplayMode`至`true`如果您的行動配置效果不佳非行動檢視。 螢幕擷取畫面所示方式*喇叭*頁面轉譯時`RequireConsistentDisplayMode`設`true`。

[![p3_speakersConsistent](aspnet-mvc-4-mobile-features/_static/image33.png)](aspnet-mvc-4-mobile-features/_static/image32.png)

您可以藉由設定停用在檢視中的一致的顯示模式`RequireConsistentDisplayMode`至`false`檢視檔案中。 中的下列標記*Views\Home\AllSpeakers.cshtml*檔案集`RequireConsistentDisplayMode`至`false`:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample18.cshtml)]

## <a name="creating-a-mobile-speakers-view"></a>建立行動喇叭檢視

如您剛才所見，*喇叭* 檢視來得容易讀懂，但連結都很小而且很難進行行動裝置上點選。 在本節中，您將建立行動裝置特定*喇叭*檢視看起來像現代的行動應用程式，它會顯示大型、 簡單點選連結，並包含在搜尋方塊，以快速找出喇叭。

複製*AllSpeakers.cshtml*至*AllSpeakers.Mobile.cshtml*。 開啟*AllSpeakers.Mobile.cshtml*檔案，並移除`<h2>`標題項目。

在`<ul>`標記中加入`data-role`屬性，並將其值設定為`listview`。 就像其他[`data-*`屬性](http://html5doctor.com/html5-custom-data-attributes/)，`data-role="listview"`點選時，對大型清單項目更容易。 這是完整的標記外觀如下：

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample19.cshtml)]

重新整理行動瀏覽器。 更新的檢視看起來像這樣：

[![p3_updatedSpeakerView1](aspnet-mvc-4-mobile-features/_static/image35.png)](aspnet-mvc-4-mobile-features/_static/image34.png)

雖然行動檢視已經改善，很難瀏覽的喇叭長的清單。 若要解決此問題，在`<ul>`標記中加入`data-filter`屬性，並將它設定為`true`。 程式碼所示`ul`標記。

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample20.html)]

下圖顯示在所產生的頁面頂端的 [搜尋] 篩選器方塊`data-filter`屬性。

[![ps_Data_Filter](aspnet-mvc-4-mobile-features/_static/image37.png)](aspnet-mvc-4-mobile-features/_static/image36.png)

當您在 [搜尋] 方塊中輸入每個字母，jQuery Mobile 來篩選所顯示的清單中下圖所示。

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image39.png)](aspnet-mvc-4-mobile-features/_static/image38.png)

## <a name="improving-the-tags-list"></a>改善標籤清單

喜歡預設值*喇叭* 檢視中，*標記* 檢視來得容易讀懂，但連結是小型且難以行動裝置上點選。 在本節中，您將會修正*標記*檢視您固定的相同方式*喇叭*檢視。

移除&quot;隱藏&quot;尾碼*Views\Home\AllTags.Mobile.cshtml.hide*檔案讓名稱*Views\Home\AllTags.Mobile.cshtml*。 開啟 重新命名的檔案，然後移除`<h2>`項目。

新增`data-role`和`data-filter`屬性至`<ul>`標記，如下所示：

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample21.html)]

下圖顯示 [標記] 頁面上篩選的字母`J`。

[![p3_tags_J](aspnet-mvc-4-mobile-features/_static/image41.png)](aspnet-mvc-4-mobile-features/_static/image40.png)

## <a name="improving-the-dates-list"></a>改善 [日期] 清單

您可以改善*日期*檢視您的改善，例如*喇叭*和*標記*檢視，以便更輕鬆地在行動裝置上使用。

複製*Views\Home\AllDates.cshtml*檔案*Views\Home\AllDates.Mobile.cshtml*。 開啟新的檔案，然後移除`<h2>`項目。

新增`data-role="listview"`至`<ul>`標記，像這樣：

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample22.html)]

下圖顯示當**日期**頁面看起來像與`data-role`就地屬性。

[![p3_dates1](aspnet-mvc-4-mobile-features/_static/image43.png)](aspnet-mvc-4-mobile-features/_static/image42.png)的內容取代*Views\Home\AllDates.Mobile.cshtml*以下列程式碼檔案：

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample23.cshtml)]

此程式碼群組依日期的所有工作階段。 建立清單分隔每個新的一天，並列出在分割線的每一天的所有工作階段。 以下是什麼樣子執行此程式碼：

[![p3_dates2](aspnet-mvc-4-mobile-features/_static/image45.png)](aspnet-mvc-4-mobile-features/_static/image44.png)

## <a name="improving-the-sessionstable-view"></a>改善 SessionsTable 檢視

在本節中，您將建立工作階段特定的行動的檢視。 我們所做的變更會比在檢視中，我們建立了更廣泛。

在行動裝置瀏覽器中，點選**喇叭**按鈕，然後輸入`Sc`[搜尋] 方塊中。

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image47.png)](aspnet-mvc-4-mobile-features/_static/image46.png)

點選**Scott Hanselman**連結。

[![p3_scottHa](aspnet-mvc-4-mobile-features/_static/image49.png)](aspnet-mvc-4-mobile-features/_static/image48.png)

如您所見，顯示會很難讀取行動瀏覽器上。 日期資料行是難以讀取的標記 資料行超出檢視。 若要修正此問題，將複製*Views\Home\SessionsTable.cshtml*至*Views\Home\SessionsTable.Mobile.cshtml*，並以下列程式碼取代檔案的內容：

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample24.cshtml)]

程式碼移除聊天室和標記資料行，並格式化標題、 演講者和日期，使這項資訊可在行動裝置瀏覽器上讀取。 下圖會反映程式碼變更。

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image51.png)](aspnet-mvc-4-mobile-features/_static/image50.png)

## <a name="improving-the-sessionbycode-view"></a>改善 SessionByCode 檢視

最後，您將建立的行動裝置的特定檢視*SessionByCode*檢視。 在行動裝置瀏覽器中，點選**喇叭**按鈕，然後輸入`Sc`[搜尋] 方塊中。

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image53.png)](aspnet-mvc-4-mobile-features/_static/image52.png)

點選**Scott Hanselman**連結。 Scott Hanselman 工作階段會顯示。

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image55.png)](aspnet-mvc-4-mobile-features/_static/image54.png)

選擇**概觀 MS Web 堆疊的愛**連結。

[![ps_love](aspnet-mvc-4-mobile-features/_static/image57.png)](aspnet-mvc-4-mobile-features/_static/image56.png)

預設的桌面檢視沒有問題，但可加以改進。

複製*Views\Home\SessionByCode.cshtml*至*Views\Home\SessionByCode.Mobile.cshtml*取代的內容和*Views\Home\SessionByCode.Mobile.cshtml*檔案以下列標記：

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample25.cshtml)]

使用新的標記`data-role`屬性，以改善檢視的配置。

重新整理行動瀏覽器。 下圖會反映您剛建立的程式碼變更：

[![p3_love2](aspnet-mvc-4-mobile-features/_static/image59.png)](aspnet-mvc-4-mobile-features/_static/image58.png)

## <a name="wrapup-and-review"></a>簡便和檢閱

本教學課程已引進新的行動裝置功能的 ASP.NET MVC 4 Developer Preview。 行動裝置的功能包括：

- 覆寫版面配置、 檢視和部分檢視，全域和個別的檢視能力。
- 控制版面配置，以及部分覆寫強制使用`RequireConsistentDisplayMode`屬性。
- 行動應用程式檢視切換程式 widget 檢視比也可以在桌面檢視中顯示。
- 支援特定瀏覽器，例如 iPhone 瀏覽器支援。

## <a name="see-also"></a>請參閱

- [jQuery Mobile](http://jquerymobile.com)站台。
- [jQuery Mobile 概觀](http://jquerymobile.com/demos/1.0b3/docs/about/intro.html)
- [W3C 建議行動裝置 Web 應用程式最佳作法](http://www.w3.org/TR/mwabp/)
- [W3C Candidate Recommendation 的媒體查詢](http://www.w3.org/TR/css3-mediaqueries/)
