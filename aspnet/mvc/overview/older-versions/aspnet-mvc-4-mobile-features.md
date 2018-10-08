---
uid: mvc/overview/older-versions/aspnet-mvc-4-mobile-features
title: ASP.NET MVC 4 行動功能 |Microsoft Docs
author: Rick-Anderson
description: 現在是本教學課程，在部署 ASP.NET MVC 5 行動 Web 應用程式 Azure 網站上的程式碼範例的 MVC 5 版本。
ms.author: riande
ms.date: 08/15/2012
ms.assetid: 27dc4fc8-1b51-43b0-933f-fc1b52476523
msc.legacyurl: /mvc/overview/older-versions/aspnet-mvc-4-mobile-features
msc.type: authoredcontent
ms.openlocfilehash: 8b82b8b9b1ee6646072931da889c643afb34d474
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578155"
---
<a name="aspnet-mvc-4-mobile-features"></a>ASP.NET MVC 4 Mobile 功能
====================
藉由[Rick Anderson]((https://twitter.com/RickAndMSFT))

> 目前沒有程式碼範例，在本教學課程的 MVC 5 版本[部署 ASP.NET MVC 5 行動 Web 應用程式在 Azure Web Sites](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/)。


本教學課程將教導您如何使用 ASP.NET MVC 4 Web 應用程式中的行動功能的基本概念。 本教學課程中，您可以使用[Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express)或 Visual Web Developer 2010 Express Service Pack 1 (&quot;Visual Web Developer 或 VWD&quot;)。 如果您已具備，您可以使用 Visual Studio professional 版。

在開始之前，請確定您已安裝符合下列先決條件。

- [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) （建議選項） 或 Visual Studio Web Developer Express SP1。 Visual Studio 2012 包含 ASP.NET MVC 4。 如果您使用 Visual Web Developer 2010，您必須安裝[ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)。

您也需要行動瀏覽器模擬器。 下列其中一項將會運作：

- [Windows 7 Phone 模擬器](https://msdn.microsoft.com/library/ff402563(VS.92).aspx)。 （這是本教學課程使用大部分的螢幕擷取畫面中的模擬器）。
- 變更模擬 iPhone 的使用者代理字串。 請參閱[這](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/)部落格文章。
- [Opera Mobile 模擬器](http://www.opera.com/developer/tools/mobile/)
- [Apple Safari](http://www.apple.com/safari/download/)與使用者代理程式設定為 iPhone。 如需有關如何在 Safari 中的使用者代理程式設定為 「 iPhone 」 的指示，請參閱[如何讓假裝是 IE 的 Safari](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html)給 David Alison 的部落格上。

使用 C# 原始程式碼的 visual Studio 專案可用於本主題隨附了：

- [下載入門專案](https://go.microsoft.com/fwlink/?linkid=228307&amp;clcid=0x409)
- [完成專案下載](https://go.microsoft.com/fwlink/?linkid=228306&amp;clcid=0x409)

### <a name="what-youll-build"></a>您將建置

本教學課程中，會將行動功能新增至所提供的簡單會議清單應用程式[入門專案](https://go.microsoft.com/fwlink/?LinkId=228307)。 下列螢幕擷取畫面會顯示已完成的應用程式的 [標記] 頁面中所示[Windows 7 Phone 模擬器](https://msdn.microsoft.com/library/ff402563(VS.92).aspx)。 請參閱[鍵盤對應的 Windows Phone 模擬器](https://msdn.microsoft.com/library/ff754352(v=vs.92).aspx)簡化鍵盤輸入。

[![p1_Tags_CompletedProj](aspnet-mvc-4-mobile-features/_static/image2.png)](aspnet-mvc-4-mobile-features/_static/image1.png)

您可以使用 Internet Explorer 9 或 10，FireFox 或 Chrome 開發行動應用程式設定的版本[使用者代理字串](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/)。 下圖顯示已完成本教學課程，並使用模擬 iPhone 的 Internet Explorer。 您可以使用 Internet Explorer F-12 開發人員工具和有[Fiddler 工具](http://www.fiddler2.com/fiddler2/)協助偵錯您的應用程式。

![](aspnet-mvc-4-mobile-features/_static/image3.png)

### <a name="skills-youll-learn"></a>您將學習到的技能

以下是您將學到什麼：

- ASP.NET MVC 4 範本如何使用 HTML5`viewport`行動裝置上顯示的屬性和調整呈現改善。
- 如何建立行動專用的檢視。
- 如何建立檢視切換器，可讓使用者切換行動檢視和桌面應用程式檢視。

### <a name="getting-started"></a>快速入門

下載會議清單應用程式，使用下列連結的入門專案：[下載](https://go.microsoft.com/fwlink/?LinkId=228307)。 然後在 Windows 檔案總管中，以滑鼠右鍵按一下*MvcMobile.zip*檔案，然後選擇**屬性**。 在 [ **MvcMobile.zip 屬性**對話方塊方塊中，選擇**解除封鎖**] 按鈕。 (解除封鎖可避免安全性警告，當您嘗試使用時，就會發生 *.zip*您已經從 web 下載的檔案。)

![p1_unBlock](aspnet-mvc-4-mobile-features/_static/image4.png)

以滑鼠右鍵按一下*MvcMobile.zip*檔案，然後選取**全部解壓縮**來解壓縮檔案。 在 Visual Studio 中開啟*MvcMobile.sln*檔案。

按下 CTRL + F5 執行應用程式，將會顯示在桌面瀏覽器。 啟動您的行動瀏覽器模擬器，將會議應用程式的 URL 複製到模擬器，然後按一下**依標籤瀏覽**連結。 如果您使用的 Windows Phone 模擬器，請按一下 URL 列中，然後按鍵盤存取 Pause 鍵。 下的圖顯示*AllTags*檢視 (選擇**依標籤瀏覽**)。

[![p1_browseTag](aspnet-mvc-4-mobile-features/_static/image6.png)](aspnet-mvc-4-mobile-features/_static/image5.png)

顯示已在行動裝置上非常清楚易讀。 選擇 ASP.NET 連結。

[![p1_tagged_ASPNET](aspnet-mvc-4-mobile-features/_static/image8.png)](aspnet-mvc-4-mobile-features/_static/image7.png)

ASP.NET 標籤檢視是很雜亂。 例如，**日期**資料行是很難讀取。 稍後在本教學課程中，您將建立的版本*AllTags*檢視，是專為行動瀏覽器，並將會成為可讀取的顯示。

注意： Bug 中目前存在的行動裝置的快取引擎。 對於生產應用程式，您必須安裝[固定 DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) nuget 封裝。 請參閱[ASP.NET MVC 4 Mobile 快取 Bug 已修正](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)如修正程式的詳細資訊。

## <a name="css-media-queries"></a>CSS 媒體查詢

[CSS 媒體查詢](http://www.w3.org/TR/css3-mediaqueries/)CSS 媒體類型的延伸模組。 可讓您建立規則，以覆寫預設的 CSS 規則，針對特定瀏覽器 （使用者代理程式）。 以行動瀏覽器為目標的 CSS 的一般規則定義的最大螢幕大小。 *Content\Site.css*當您建立新的 ASP.NET MVC 4 網際網路專案建立的檔案包含下列的媒體查詢：

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample1.css)]

如果瀏覽器視窗中是 850 個像素寬或較少，它會使用此媒體區塊內的 CSS 規則。 您可以使用這類的 CSS 媒體查詢的桌面瀏覽器變寬顯示小型的瀏覽器 （例如行動瀏覽器） 所設計的預設 CSS 規則比提供更好的顯示的 HTML 內容。

## <a name="the-viewport-meta-tag"></a>將檢視區的中繼標籤

大部分的行動瀏覽器定義虛擬瀏覽器視窗寬度 ( *viewport*)，是遠超過在行動裝置的實際寬度。 這可讓行動瀏覽器，以符合整個網頁內虛擬顯示。 使用者可以再拉近關注的內容。 不過，如果您將檢視區寬度為實際裝置寬度時，沒有縮放是必要的因為內容符合行動瀏覽器中。

檢視區`<meta>`ASP.NET MVC 4 版面配置檔案中的標記會將檢視區設定為裝置寬度。 下面這一行會顯示在檢視區`<meta>`ASP.NET MVC 4 版面配置檔案中的標記。

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample2.html)]

## <a name="examining-the-effect-of-css-media-queries-and-the-viewport-meta-tag"></a>檢查 CSS 媒體查詢和檢視區 Meta 標記的效果

開啟*Views\Shared\\_Layout.cshtml*檔案中的編輯器和檢視區註解`<meta>`標記。 下列標記會顯示標記為註解的線條。

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample3.cshtml)]

開啟*MvcMobile\Content\Site.css*檔案在編輯器中，並變更在媒體查詢的最大寬度為零的像素。 如此可防止在行動瀏覽器中使用的 CSS 規則。 下面這一行會顯示已修改的媒體查詢：

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample4.css)]

儲存變更，並瀏覽至會議應用程式在行動瀏覽器模擬器。 在下圖中的小型文字是移除檢視區的結果`<meta>`標記。 沒有檢視區與`<meta>`標記中，瀏覽器會縮小成預設檢視區寬度 (850 個像素或針對大部分的行動瀏覽器變寬。)

[![p1_noViewPort](aspnet-mvc-4-mobile-features/_static/image10.png)](aspnet-mvc-4-mobile-features/_static/image9.png)

復原您的變更 — 檢視區取消註解`<meta>`標記中的配置檔案，並將媒體查詢還原至 850 像素*Site.css*檔案。 儲存變更並重新整理行動瀏覽器，以確認適合行動的顯示已經還原。

檢視區`<meta>`標記和 CSS 媒體查詢並不專屬於 ASP.NET MVC 4 中，以及您可以利用這些功能在任何 web 應用程式。 但現在內建到您建立新的 ASP.NET MVC 4 專案時，所產生的檔案。

如需詳細資訊，檢視區的相關`<meta>`標記，請參閱 <<c2> [ 源由兩個檢視區 — 第二部分](http://www.quirksmode.org/mobile/viewports2.html)。

下一節中，您會看到如何提供行動瀏覽器特定的檢視。

## <a name="overriding-views-layouts-and-partial-views"></a>覆寫檢視、 配置與部分檢視

ASP.NET MVC 4 中的重要新功能是簡單的機制，可讓您覆寫任何檢視中 （包括版面配置與部分檢視） 的行動瀏覽器，在一般情況下，針對個別的行動瀏覽器，或任何特定的瀏覽器。 若要提供行動裝置專屬檢視，您可以複製檢視檔案，並新增 *。Mobile*的檔案名稱。 例如，若要建立行動*Index*檢視中，複製*Views\Home\Index.cshtml*來*Views\Home\Index.Mobile.cshtml*。

在本節中，您將建立行動專用的配置檔案。

若要開始，複製*Views\Shared\\_Layout.cshtml*要*Views\Shared\\_Layout.Mobile.cshtml*。 開啟 *\_Layout.Mobile.cshtml*並將標題變更**MVC4 會議**來**會議 （行動裝置版）**。

在每個`Html.ActionLink`呼叫時，移除 「 Browse by 」 在每個 link *ActionLink*。 下列程式碼顯示行動配置檔案的已完成的主體區段。

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample5.cshtml)]

複製*Views\Home\AllTags.cshtml*的檔案*Views\Home\AllTags.Mobile.cshtml*。 開啟新的檔案，並變更`<h2>`項目從"Tags"到"Tags (M) 」:

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample6.html)]

瀏覽至 [標記] 頁面中使用桌面瀏覽器和行動瀏覽器模擬器。 行動瀏覽器模擬器會顯示兩個您所做的變更。

[![p2m_layoutTags.mobile](aspnet-mvc-4-mobile-features/_static/image12.png)](aspnet-mvc-4-mobile-features/_static/image11.png)

相較之下，桌面顯示則沒有變更。

[![p2_layoutTagsDesktop](aspnet-mvc-4-mobile-features/_static/image14.png)](aspnet-mvc-4-mobile-features/_static/image13.png)

## <a name="browser-specific-views"></a>瀏覽器專用的檢視

除了行動與桌面專用的檢視，您可以建立個別的瀏覽器的檢視。 例如，您可以建立 iPhone 瀏覽器的專用的檢視。 在本節中，您將建立 iPhone 瀏覽器以及 iPhone 版的版面配置*AllTags*檢視。

開啟*Global.asax*檔案，並新增下列程式碼`Application_Start`方法。

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample7.cs)]

此程式碼會定義新的顯示模式，名為"iPhone"會符合每個傳入要求。 如果傳入要求符合您定義 （也就是如果使用者代理程式會包含"iPhone"字串） 的條件，ASP.NET MVC 會尋找其名稱中包含"iPhone"字尾的檢視。

在程式碼中，以滑鼠右鍵按一下`DefaultDisplayMode`，選擇**解決**，然後選擇  `using System.Web.WebPages;`。 這會將參考加入`System.Web.WebPages`命名空間，這正是`DisplayModes`和`DefaultDisplayMode`類型定義。

[![p2_resolve](aspnet-mvc-4-mobile-features/_static/image16.png)](aspnet-mvc-4-mobile-features/_static/image15.png)

或者，您可以只是手動新增下列這一行加入`using`檔案區段。

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample8.cs)]

完整內容*Global.asax*檔案如下所示。

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample9.cs)]

儲存變更。 複製*MvcMobile\Views\Shared\\_Layout.Mobile.cshtml*檔案*MvcMobile\Views\Shared\\_Layout.iPhone.cshtml*。 開啟新的檔案，然後變更`h1`標題從`Conference (Mobile)`至`Conference (iPhone)`。

複製*MvcMobile\Views\Home\AllTags.Mobile.cshtml*的檔案*MvcMobile\Views\Home\AllTags.iPhone.cshtml*。 在新的檔案中，變更`<h2>`為 「 Tags (iPhone) 」 的項目從 「 Tags (M) 」。

執行應用程式。 執行行動瀏覽器模擬器，請確定其使用者代理程式設定為 「 iPhone 」，並瀏覽至*AllTags*檢視。 下列螢幕擷取畫面示*AllTags*檢視中轉譯[Safari](http://www.apple.com/safari/download/)瀏覽器。 您可以下載的 Windows Safari[此處](https://support.apple.com/kb/DL1531)。

[![p2_iphoneView](aspnet-mvc-4-mobile-features/_static/image18.png)](aspnet-mvc-4-mobile-features/_static/image17.png)

我們已了解如何建立行動配置和檢視以及如何建立版面配置和檢視特定的裝置，例如 iPhone 這一節。 下一節中，您會看到如何運用 jQuery Mobile 的更多吸引人的行動檢視。

## <a name="using-jquery-mobile"></a>使用 jQuery Mobile

[JQuery Mobile](http://jquerymobile.com/demos/1.0b3/#/demos/1.0b3/docs/about/intro.html)程式庫提供一種使用者介面架構，適用於所有主要的行動瀏覽器。 適用於 jQuery Mobile*漸進式增強*支援 CSS 和 JavaScript 的行動瀏覽器。 漸進式增強功能可讓所有的瀏覽器，以顯示網頁的基本內容，同時允許更強大的瀏覽器和裝置有更豐富的顯示。 隨附的 jQuery Mobile 的 JavaScript 和 CSS 檔案而不進行任何標記變更成行動瀏覽器的許多項目設定樣式。

在本節中，您會安裝*jQuery.Mobile.MVC* NuGet 套件，這會安裝 jQuery Mobile 和檢視切換器小工具。

若要開始，刪除*Shared\\_Layout.Mobile.cshtml*並*共用\\_Layout.iPhone.cshtml*您稍早建立的檔案。

重新命名*Views\Home\AllTags.Mobile.cshtml*並*Views\Home\AllTags.iPhone.cshtml*檔案到*Views\Home\AllTags.iPhone.cshtml.hide*並*Views\Home\AllTags.Mobile.cshtml.hide*。 因為檔案不再具有 *.cshtml*擴充功能，它們不使用 ASP.NET MVC 執行階段呈現*AllTags*檢視。

安裝*jQuery.Mobile.MVC*這樣的 NuGet 套件：

1. 從**工具**功能表上，選取**程式庫套件管理員**，然後選取**Package Manager Console**。

    [![p3_packageMgr](aspnet-mvc-4-mobile-features/_static/image20.png)](aspnet-mvc-4-mobile-features/_static/image19.png)
2. 在  **Package Manager Console**，輸入 `Install-Package jQuery.Mobile.MVC -version 1.0.0`

下圖顯示加入和變更為 MvcMobile 專案 jQuery.Mobile.MVC nuget 檔案。 [新增] 將其附加的檔案名稱之後新增的檔案。 映像不會顯示 GIF 和 PNG 檔案新增至*Content\images*資料夾。

![](aspnet-mvc-4-mobile-features/_static/image21.png)

JQuery.Mobile.MVC NuGet 套件會安裝下列項目：

- *應用程式\_Start\BundleMobileConfig.cs*才能參考加入 jQuery JavaScript 和 CSS 檔案的檔案。 您必須遵循下列指示，並參考這個檔案中定義的行動裝置組合。
- jQuery Mobile CSS 檔案。
- A`ViewSwitcher`控制器的小工具 (*Controllers\ViewSwitcherController.cs*)。
- jQuery Mobile JavaScript 檔案。
- JQuery Mobile 樣式的配置檔案 (*Views\Shared\\_Layout.Mobile.cshtml*)。
- 檢視切換器的部分檢視 *(MvcMobile\Views\Shared\\_ViewSwitcher.cshtml*)，提供要切換到行動裝置檢視，反之亦然的桌面檢視從每個頁面頂端的連結。
- 數個<em>.png</em>並<em>.gif</em>影像檔<em>Content\images</em>資料夾。

開啟*Global.asax*檔案，並新增下列程式碼的最後一行為`Application_Start`方法。

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample10.cs)]

下列程式碼示範完整*Global.asax*檔案。

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample11.cs?highlight=26)]

> [!NOTE]
> 如果您使用 Internet Explorer 9，而且您沒有看到`BundleMobileConfig`黃色反白顯示在上方列中，按一下 [相容性檢視 按鈕](https://windows.microsoft.com/windows7/How-to-use-Compatibility-View-in-Internet-Explorer-9)![（關閉） 的相容性檢視 按鈕的圖片](http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "（關閉） 的相容性檢視 按鈕的圖片")在 IE 中進行變更的外框的圖示![（關閉） 的相容性檢視 按鈕的圖片](http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "（關閉） 的相容性檢視 按鈕的圖片")為純色![(on) 相容性檢視 按鈕的圖片](http://res1.windows.microsoft.com/resbox/en/Windows 7/main/156805ff-3130-481b-a12d-4d3a96470f36_14.jpg "(on) 相容性檢視 按鈕的圖片")。 或者，您可以在 FireFox 或 Chrome 中檢視本教學課程。


開啟*MvcMobile\Views\Shared\\_Layout.Mobile.cshtml*檔案，並新增下列標記直接之後`Html.Partial`呼叫：

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample12.cshtml)]

完整*MvcMobile\Views\Shared\\_Layout.Mobile.cshtml*檔案如下所示：

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample13.cshtml)]

建置應用程式，並在您的行動瀏覽器模擬器瀏覽至*AllTags*檢視。 您看到下列內容：

[![p3_afterNuGet](aspnet-mvc-4-mobile-features/_static/image23.png)](aspnet-mvc-4-mobile-features/_static/image22.png)

> [!NOTE]
> 您可以偵錯的行動裝置的特定程式碼[設定的使用者代理字串](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/)IE 或 Chrome 中的，才能 iPhone，然後使用 F-12 開發人員工具。 如果未顯示您的行動瀏覽器**首頁**，**演講者備忘**，**標記**，以及**日期**為按鈕的連結，jQuery Mobile 的參考指令碼和 CSS 檔案可能不是正確的。


除了樣式變更，您會看到**顯示行動檢視**並可讓您從行動裝置檢視切換至 [桌面] 檢視的連結。 選擇**桌面檢視**連結和 [桌面] 檢視會顯示。

[![p3_desktopView](aspnet-mvc-4-mobile-features/_static/image25.png)](aspnet-mvc-4-mobile-features/_static/image24.png)

桌面檢視不提供直接瀏覽回到行動裝置檢視的方式。 您將會修正。 開啟*Views\Shared\\_Layout.cshtml*檔案。 頁面的下方`body`項目，新增下列程式碼呈現的檢視切換器 widget:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample14.cshtml)]

重新整理*AllTags*行動瀏覽器中的檢視。 您現在可以桌上型電腦和行動裝置檢視之間巡覽。

[![p3_desktopViewWithMobileLink](aspnet-mvc-4-mobile-features/_static/image27.png)](aspnet-mvc-4-mobile-features/_static/image26.png)

> [!NOTE]
> 偵錯附註： 您可以將下列程式碼新增至 Views\Shared 結尾\\_ViewSwitcher.cshtml 協助偵錯檢視，當使用瀏覽器使用者代理字串設定為行動裝置。
> 
> [!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample15.cs)]
> 
>  並新增下列標題，即可*Views\Shared\\_Layout.cshtml*檔案。  
> 
> [!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample16.html)]


瀏覽至*AllTags*桌面瀏覽器中的頁面。 檢視切換器小工具不會顯示在桌面瀏覽器，因為它只加入至行動裝置的版面配置頁。 稍後在本教學課程中，您會看到如何將檢視切換器小工具新增到桌面檢視。

[![p3_desktopBrowser](aspnet-mvc-4-mobile-features/_static/image29.png)](aspnet-mvc-4-mobile-features/_static/image28.png)

## <a name="improving-the-speakers-list"></a>改善演講者清單

在行動瀏覽器中，選取**喇叭**連結。 因為沒有行動檢視 (*AllSpeakers.Mobile.cshtml*)，預設的演講者顯示 (*AllSpeakers.cshtml*) 會使用行動配置檢視呈現 ( *\_Layout.Mobile.cshtml*)。

[![p3_speakersDeskTop](aspnet-mvc-4-mobile-features/_static/image31.png)](aspnet-mvc-4-mobile-features/_static/image30.png)

您可以藉由設定全域停用預設 （非行動） 檢視行動配置內呈現`RequireConsistentDisplayMode`要`true`中*檢視\\_ViewStart.cshtml*檔案，像這樣：

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample17.cshtml)]

當`RequireConsistentDisplayMode`設定為`true`，行動配置 (<em>\_Layout.Mobile.cshtml</em>) 只適用於行動檢視。 (也就是檢視檔案屬於表單<em>* * ViewName</em><em>.Mobile.cshtml</em>。)您可能想要設定`RequireConsistentDisplayMode`至`true`若行動配置不適用於您的非行動檢視。 下列螢幕擷取畫面如何<em>喇叭</em>呈現頁面時`RequireConsistentDisplayMode`設為`true`。

[![p3_speakersConsistent](aspnet-mvc-4-mobile-features/_static/image33.png)](aspnet-mvc-4-mobile-features/_static/image32.png)

您可以設定連線，停用在檢視中的一致的顯示模式`RequireConsistentDisplayMode`至`false`檢視檔案中。 中的以下標記*Views\Home\AllSpeakers.cshtml*檔中設定`RequireConsistentDisplayMode`到`false`:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample18.cshtml)]

## <a name="creating-a-mobile-speakers-view"></a>建立行動裝置的演講者檢視

如您剛才所見，*喇叭*檢視是否可讀取，但連結非常微小而不容易點選行動裝置上。 在本節中，您將建立行動裝置專屬*喇叭*檢視看起來像現代的行動應用程式，它會顯示大型、 簡單點選連結，以及包含 [搜尋] 方塊快速找到演講者。

複製*AllSpeakers.cshtml*要*AllSpeakers.Mobile.cshtml*。 開啟*AllSpeakers.Mobile.cshtml*檔案，並移除`<h2>`標題項目。

在 `<ul>`標記中加入`data-role`屬性，並將其值設定為`listview`。 就像其他[`data-*`屬性](http://html5doctor.com/html5-custom-data-attributes/)，`data-role="listview"`點選時，對大型清單項目更容易。 這是完整的標記外觀如下：

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample19.cshtml)]

重新整理行動瀏覽器。 [更新] 檢視看起來像這樣：

[![p3_updatedSpeakerView1](aspnet-mvc-4-mobile-features/_static/image35.png)](aspnet-mvc-4-mobile-features/_static/image34.png)

雖然已經改善行動檢視，很難瀏覽冗長的演講者清單。 若要解決此問題，在`<ul>`標記中加入`data-filter`屬性，並將它設定為`true`。 以下顯示的程式碼`ul`標記。

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample20.html)]

下圖顯示在所產生的頁面頂端的 [搜尋] 篩選方塊`data-filter`屬性。

[![ps_Data_Filter](aspnet-mvc-4-mobile-features/_static/image37.png)](aspnet-mvc-4-mobile-features/_static/image36.png)

當您在搜尋方塊中輸入每個字母，jQuery Mobile 會篩選顯示的清單，如下圖所示。

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image39.png)](aspnet-mvc-4-mobile-features/_static/image38.png)

## <a name="improving-the-tags-list"></a>改善標籤清單

喜歡預設值*主講人*檢視中，*標記*檢視是否可讀取，但連結卻非常小且容易點選行動裝置上。 在本節中，您將會修正*標記*檢視的相同方式，您已修正*喇叭*檢視。

移除&quot;隱藏&quot;尾碼*Views\Home\AllTags.Mobile.cshtml.hide*檔案的名稱是因此*Views\Home\AllTags.Mobile.cshtml*。 開啟已重新命名的檔案，並移除`<h2>`項目。

新增`data-role`並`data-filter`屬性加入`<ul>`標記，如下所示：

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample21.html)]

下圖顯示 [標記] 頁面上篩選字母`J`。

[![p3_tags_J](aspnet-mvc-4-mobile-features/_static/image41.png)](aspnet-mvc-4-mobile-features/_static/image40.png)

## <a name="improving-the-dates-list"></a>改善日期清單

您可以改善*日期*檢視一樣您改善*喇叭*和*標記*檢視，以便更輕鬆地在行動裝置上使用。

複製*Views\Home\AllDates.cshtml*的檔案*Views\Home\AllDates.Mobile.cshtml*。 開啟新的檔案，並移除`<h2>`項目。

新增`data-role="listview"`至`<ul>`標記，如下：

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample22.html)]

下圖顯示什麼**日期** 頁面看起來像使用`data-role`就地的屬性。

[![p3_dates1](aspnet-mvc-4-mobile-features/_static/image43.png)](aspnet-mvc-4-mobile-features/_static/image42.png)的內容取代*Views\Home\AllDates.Mobile.cshtml*為下列程式碼的檔案：

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample23.cshtml)]

此程式碼會分組天中的所有工作階段。 它會建立每一天，清單分隔線，並列出分割線底下的每一天的所有工作階段。 以下是看起來像是執行此程式碼：

[![p3_dates2](aspnet-mvc-4-mobile-features/_static/image45.png)](aspnet-mvc-4-mobile-features/_static/image44.png)

## <a name="improving-the-sessionstable-view"></a>改善 SessionsTable 檢視

在本節中，您將建立行動專用檢視的工作階段。 我們所做的變更將會比在檢視中，我們建立了更廣泛。

在行動瀏覽器中，點選**演講者備忘**按鈕，然後輸入`Sc`在搜尋方塊中。

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image47.png)](aspnet-mvc-4-mobile-features/_static/image46.png)

點選**Scott Hanselman**連結。

[![p3_scottHa](aspnet-mvc-4-mobile-features/_static/image49.png)](aspnet-mvc-4-mobile-features/_static/image48.png)

如您所見，顯示會很難讀取行動瀏覽器上。 日期資料行很難讀取，並超出檢視的標記 資料行。 若要修正此問題，請將*Views\Home\SessionsTable.cshtml*要*Views\Home\SessionsTable.Mobile.cshtml*，並以下列程式碼取代檔案的內容：

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample24.cshtml)]

程式碼移除聊天室和標籤資料行，並格式化標題、 演說家和日期垂直，使行動瀏覽器上閱讀所有資訊。 下圖會反映程式碼變更。

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image51.png)](aspnet-mvc-4-mobile-features/_static/image50.png)

## <a name="improving-the-sessionbycode-view"></a>改善 SessionByCode 檢視

最後，您將建立的行動裝置專屬檢視*SessionByCode*檢視。 在行動瀏覽器中，點選**演講者備忘**按鈕，然後輸入`Sc`在搜尋方塊中。

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image53.png)](aspnet-mvc-4-mobile-features/_static/image52.png)

點選**Scott Hanselman**連結。 Scott Hanselman 的工作階段會顯示。

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image55.png)](aspnet-mvc-4-mobile-features/_static/image54.png)

選擇**概觀 MS Web 堆疊的熱愛**連結。

[![ps_love](aspnet-mvc-4-mobile-features/_static/image57.png)](aspnet-mvc-4-mobile-features/_static/image56.png)

預設的桌面檢視雖然不錯，但您可以改善它。

複製*Views\Home\SessionByCode.cshtml*要*Views\Home\SessionByCode.Mobile.cshtml* ，並取代的內容*Views\Home\SessionByCode.Mobile.cshtml*檔案，以下列標記：

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample25.cshtml)]

新的標記會使用`data-role`屬性來改善檢視的配置。

重新整理行動瀏覽器。 下圖反映您剛建立的程式碼變更：

[![p3_love2](aspnet-mvc-4-mobile-features/_static/image59.png)](aspnet-mvc-4-mobile-features/_static/image58.png)

## <a name="wrapup-and-review"></a>簡便與檢閱

本教學課程中導入了新的行動功能的 ASP.NET MVC 4 Developer Preview。 行動裝置的功能包括：

- 覆寫版面配置、 檢視和部分檢視，全域和個別檢視的能力。
- 控制版面配置和部分覆寫強制使用`RequireConsistentDisplayMode`屬性。
- 適用於行動裝置檢視切換器小工具也可顯示在桌面檢視中檢視。
- 支援特定的瀏覽器，例如 iPhone 瀏覽器的支援。

## <a name="see-also"></a>另請參閱

- [jQuery Mobile](http://jquerymobile.com)站台。
- [jQuery Mobile 概觀](http://jquerymobile.com/demos/1.0b3/docs/about/intro.html)
- [W3C 推薦行動 Web 應用程式最佳做法](http://www.w3.org/TR/mwabp/)
- [W3C 針對媒體查詢的候選推薦做法](http://www.w3.org/TR/css3-mediaqueries/)
