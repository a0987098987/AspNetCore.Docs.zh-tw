---
uid: mvc/overview/views/using-page-inspector-in-aspnet-mvc
title: 在 ASP.NET MVC 中使用 Page Inspector |Microsoft 文件
author: rick-anderson
description: Page Inspector 在 Visual Studio 2012 中是以整合式瀏覽器的 web 開發工具。 在 Page Inspector i 與整合式瀏覽器中，選取任何項目...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: c7e4e1ab-4932-4614-9f53-aaf7c706d498
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/views/using-page-inspector-in-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 5b443963a089f96a9dab11b7db4a25451075d6be
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="using-page-inspector-in-aspnet-mvc"></a>在 ASP.NET MVC 中使用 Page Inspector
====================
由 Tim Ammann

> Page Inspector 在 Visual Studio 2012 中是以整合式瀏覽器的 web 開發工具。 在整合式瀏覽器中，選取任何項目和 Page Inspector 立即會反白顯示項目的來源和 CSS。 您可以瀏覽任何的 MVC 檢視，快速找不到來源呈現標記，並使用瀏覽器工具，Visual Studio 環境內的權限。
> 
> [觀賞視訊](../../videos/mvc-4/using-page-inspector-in-aspnet-mvc.md)
> 
> 本教學課程會示範如何啟用檢查模式中，然後快速找出並編輯您的 web 專案內的標記和 CSS。 本教學課程使用 MVC 專案，但您也可以使用 Page Inspector 的[Web Form](https://go.microsoft.com/?linkid=9802001)和其他的 ASP.NET 應用程式。
> 
> 本教學課程包含下列各節：
> 
> - [必要條件](#_1_prerequisites)
> - [建立 Web 應用程式](#_2_creating_a)
> - [使用 Page Inspector 瀏覽 以檢視](#_3_using_page)
> - [啟用檢查模式](#_4_inspection_mode)
> - [若要變更標記使用 Page Inspector](#_5_using_page)
> - [檢查模式和 [HTML] 視窗](#_6_inspection_mode)
> - [在 [樣式] 視窗中的預覽 CSS 變更](#_7_previewing_css)
> - [CSS 自動同步處理](#css_auto_sync)
> - [使用 CSS 色彩選擇器](#css_color_picker)
> - [對應至 JavaScript 的動態頁面項目](#map_dynamic_elements)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a>必要條件

- [Visual Studio 2012](https://www.microsoft.com/visualstudio/11)或[Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web)。

> [!NOTE]
> 若要取得 Page Inspector 的最新版本，請使用[Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386)安裝 Windows Azure SDK for.NET 2.0。


Page Inspector 隨附 Microsoft Web Developer Tools。 最新版本是 1.3。 若要檢查的版本中，執行 Visual Studio 也可以選取**關於 Microsoft Visual Studio**從**協助**功能表。

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a>建立 Web 應用程式

首先，建立會使用 Page Inspector 與 web 應用程式。 在 Visual Studio 中，選擇 **檔案** &gt; **新專案**。 在左邊展開**Visual C#**，選取**Web**，然後選取**ASP.NET MVC4 Web 應用程式**。

![新的 ASP.NET MVC 應用程式](using-page-inspector-in-aspnet-mvc/_static/image2.png)

按一下 [確定 **Deploying Office Solutions**]。

在**新增 ASP.NET MVC 4 專案**對話方塊中，選取**網際網路應用程式**。 保留**Razor**做為預設檢視引擎。

![新的 ASP.NET MVC 專案-網際網路應用程式](using-page-inspector-in-aspnet-mvc/_static/image4.png)

應用程式中開啟**來源**檢視。

![原始碼檢視中的新 ASP.NET MVC 應用程式](using-page-inspector-in-aspnet-mvc/_static/image6.png)

既然您已使用應用程式，您可以使用 Page Inspector 檢查和修改它。

<a id="_starting_page_inspector"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-browse-to-a-view"></a>使用 Page Inspector 瀏覽 以檢視

在 Visual Studio 2012 中，您可以以滑鼠右鍵按一下任何檢視在專案中，選取**Page Inspector 中的檢視**，Page Inspector 會找出路由，並且會顯示頁面。

在**方案總管 中**，依序展開**檢視**資料夾，然後**首頁**資料夾。 以滑鼠右鍵按一下 Index.cshtml 檔案，然後選擇**Page Inspector 中的檢視**。

![在 Page Inspector 中檢視 Index.cshtml](using-page-inspector-in-aspnet-mvc/_static/image8.png)

根據預設，Page Inspector 停駐成為視窗在 Visual Studio 環境的左半部。 如果您想要的話，您可以在其他地方，將它停駐或取消停駐視窗。 請參閱[如何： 排列和停駐視窗](https://msdn.microsoft.com/library/z4y0hsax.aspx)。

頁面偵測器視窗的上方窗格會顯示目前的網頁瀏覽器視窗中。 下方窗格會顯示在 HTML 標記中，以及可讓您檢查頁面的不同層面的某些索引標籤頁面。 下方窗格會類似於[F12 開發人員工具](https://msdn.microsoft.com/ie/aa740478)Internet Explorer 中。

![Page Inspector 中的 ASP.NET MVC 應用程式](using-page-inspector-in-aspnet-mvc/_static/image10.png)

在此教學課程中，您將使用**HTML**和**樣式**索引標籤，以快速巡覽並對應用程式進行變更。

<a id="_examining_(&quot;decomposing&quot;)_the"></a><a id="_inspection_mode_and"></a><a id="_4_inspection_mode"></a>

## <a name="enableinspection-mode"></a>EnableInspection 模式

若要讓 Page Inspector 進入檢查模式，按一下**檢查** 按鈕。 在檢查模式中，當您滑鼠指標停在呈現的任何的網頁部分的對應的來源標記或程式碼會反白顯示。

![切換檢查模式](using-page-inspector-in-aspnet-mvc/_static/image12.png)

現在將滑鼠移 Page Inspector 中頁面的不同部分。 當您這樣做，滑鼠指標會變成一個大型的加號和下方的項目會反白顯示：

![將滑鼠停留 div.content 包裝函式](using-page-inspector-in-aspnet-mvc/_static/image14.png)

當您移動滑鼠指標時，Visual Studio 會反白顯示對應的 Razor 語法的原始程式檔中。 如果 HTML 項目來自於另一個原始程式檔，Visual Studio 會自動開啟檔案。

Page Inspector 中**HTML**索引標籤會顯示所產生的 Razor 語法的 HTML。 當您移動滑鼠指標時，會反白顯示的 HTML 項目。 **樣式**索引標籤會顯示項目的 CSS 規則。

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a>若要變更標記使用 Page Inspector

Page Inspector 可讓您尋找其位置可能不明顯的標記。 然後您可以修改標記，以及查看產生的變更。

若要查看此，請按一下**檢查**然後捲動至頁面底部的 頁面偵測器視窗中。

當您將滑鼠指標移到 [頁尾] 區域時，Page Inspector 會開啟\_Layout.cshtml 檔案並反白顯示您已選取 [配置] 頁面的區段。 如您所見，頁尾會配置檔，並不檢視本身中定義。

![頁尾](using-page-inspector-in-aspnet-mvc/_static/image16.png)

現在將滑鼠指標移線條 copyright <a id="a"></a>會注意到。 在\_Layout.cshtml 頁面上，對應的線條會反白顯示。

![頁尾著作權列反白顯示](using-page-inspector-in-aspnet-mvc/_static/image18.png)

中的一行的結尾加入一些文字\_Layout.cshtml 檔案。

&lt;p&gt;&amp;複製;@DateTime.Now.Year -撼動我的 ASP.NET MVC 應用程式 ！ &lt; /p&gt;

現在，請按 Ctrl + Alt + Enter 或按一下 [更新] 列，請參閱 Page Inspector 瀏覽器視窗中的結果。

![我的 ASP.NET 應用程式岩石 ！](using-page-inspector-in-aspnet-mvc/_static/image20.png)

您想頁尾 Index.cshtml 中, 所定義，但它已在\_Layout.cshtml 和為您找到它的 Page Inspector。

<a id="_inspection_mode_and_1"></a><a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a>檢查模式和 [HTML] 視窗

接下來，您必須快速瀏覽 [HTML] 視窗，以及它如何為您對應項目。

按一下**檢查**將 Page Inspector 在檢查模式中。

按一下網頁顯示 「 您 logohere 」 的上半部。 您正在檢查詳細資料，因此，在瀏覽器視窗中的顯示無法再變更當您移動滑鼠指標的特定項的目。

現在將滑鼠指標移至**HTML**視窗。 當您移動滑鼠指標時，Page Inspector 概要說明中的項目**HTML**視窗並反白顯示瀏覽器視窗中的對應項目。

![HTML 視窗](using-page-inspector-in-aspnet-mvc/_static/image22.png)

如往常一般，Page Inspector 隨即開啟\_Layout.cshtml 檔案您在暫時性索引標籤。按一下\_Layout.cshtml 暫存索引標籤上，而對應的標記將會反白顯示在&lt;標頭&gt;> 一節讓您：

![反白顯示的標記](using-page-inspector-in-aspnet-mvc/_static/image24.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a>在 [樣式] 視窗中的預覽 CSS 變更

接下來，您會使用 Page Inspector**樣式**視窗來預覽 CSS 所做的變更。

按一下**檢查**將 Page Inspector 在檢查模式中。

在 Page Inspector 瀏覽器視窗中，將滑鼠指標移 「 首頁 」 一節，直到**div.content 包裝函式**標籤會出現。

![將滑鼠停留 div.content 包裝函式](using-page-inspector-in-aspnet-mvc/_static/image26.png)

按一下 div.content 包裝函式區段內，，然後將滑鼠指標移至**樣式**視窗。 **Syles**  視窗會顯示所有此項目的 CSS 規則。 向下捲動至 尋找.featured.content 包裝函式類別選取器。 現在，請清除的背景色彩屬性的核取方塊。

![清除的背景色彩](using-page-inspector-in-aspnet-mvc/_static/image28.png)

請注意變更預覽立即的 Page Inspector 瀏覽器視窗中。

再次選取核取方塊，然後按兩下屬性值，並將它變更為紅色。 變更會立即顯示：

![紅色的背景色彩](using-page-inspector-in-aspnet-mvc/_static/image30.png)

**樣式**視窗讓它易於測試和預覽 CSS 之前變更了您將變更認可到樣式表本身。

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a>CSS 自動同步處理

> [!NOTE]
> 此功能需要 Page Inspector 1.3 版。


CSS 自動同步處理功能可讓您直接編輯 CSS 檔案，並查看立即在 Page Inspector 瀏覽器中的變更。

按一下**檢查**將 Page Inspector 在檢查模式中。

Page Inspector 瀏覽器中將滑鼠指標移 「 首頁 」 一節，直到**div.content 包裝函式**標籤會出現。 按一下以選取此項目。

**Syles**  視窗會顯示所有此項目的 CSS 規則。 向下捲動至 尋找.featured.content 包裝函式類別選取器。 按一下 「.featured.content-包裝函式 」。 Page Inspector 開啟定義這個樣式 (Site.css)，並反白顯示對應的 CSS 樣式的 CSS 檔案。

![](using-page-inspector-in-aspnet-mvc/_static/image32.png)

值現在變更`background-color`至"red"。 變更會立即出現在 Page Inspector 瀏覽器。

![](using-page-inspector-in-aspnet-mvc/_static/image34.png)

<a id="css_color_picker"></a>
## <a name="using-the-css-color-picker"></a>使用 CSS 色彩選擇器

CSS 編輯器，Visual Studio 2012 中的會有色彩選擇器，可讓您輕鬆地選擇及插入色彩。 色彩選擇器包含標準的調色盤的色彩、 支援標準的色彩名稱、 雜湊程式碼、 RGB、 RGBA、 HSL 和 HSLA 色彩，以及維護一份您在文件最近使用的色彩。

在前一個區段中，您可以變更的值`background-color`屬性。 要叫用色彩選擇器，將插入點的屬性名稱和型別之後**#** 或**rgb (**。

![CSS 色彩選擇器列](using-page-inspector-in-aspnet-mvc/_static/image36.png)

按一下色彩選取它，或按向下鍵，然後使用左邊和右邊的方向鍵來周遊色彩。 當您瀏覽色彩時，可預覽對應的十六進位值：

![預覽的背景色彩屬性值](using-page-inspector-in-aspnet-mvc/_static/image38.png)

如果在色軸沒有完全您想要的色彩，您可以使用色彩選擇器快顯清單。 若要開啟它，請按一下色軸，右端 double > 形箭號或次按鍵盤上的向下箭號。

![CSS 色彩選擇器快顯清單](using-page-inspector-in-aspnet-mvc/_static/image40.png)

按一下右側的垂直列中的色彩。 這會在主視窗中顯示該色彩漸層。 按下 Enter，直接從垂直列中選擇的色彩，或按一下任何一點，而精確度卻選擇主視窗中。

如果您想要使用您電腦螢幕上沒有的色彩 （它不一定要在 Visual Studio 使用者介面內），您可以使用滴管工具右下方的擷取其值。

您也可以移動底部的色彩選擇器的滑桿來變更色彩的不透明度。 這麼做會變更色彩 RGBA 值的值因為 RGBA 格式可以代表不透明度。

您已選擇色彩之後，按 Enter 鍵，並接著輸入分號以完成中的背景色彩項目*Site.css*檔案。

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a>頁面偵測器更新列

Page Inspector 可以立即偵測到變更*Site.css*檔並且顯示在 更新列中的警示。

![更新列](using-page-inspector-in-aspnet-mvc/_static/image42.png)

若要儲存您的檔案，並重新整理 Page Inspector 瀏覽器，請按 Ctrl + Alt + Enter 或按一下更新列。 反白顯示色彩的變更會出現在瀏覽器。

<a id="map_dynamic_elements"></a>
## <a name="mapping-dynamic-page-elements-to-javascript"></a>對應至 JavaScript 的動態頁面項目

現代化 web 應用程式，在網頁中的項目通常動態產生的 JavaScript。 這表示沒有任何靜態標記 （HTML 或 Razor） 對應至這些頁面項目。

1.3 版中，使用 Page Inspector 才能現在對應頁面回傳至對應的 JavaScript 程式碼以動態方式加入的項目。 若要示範這項功能，我們將使用[單一頁面應用程式 (SPA) 範本](../../../single-page-application/overview/introduction/knockoutjs-template.md)。

> [!NOTE]
> SPA 範本需要[ASP.NET 和 Web 工具 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650)更新。


在 Visual Studio 中，選擇 **檔案** &gt; **新專案**。 在左邊展開**Visual C#**，選取**Web**，然後選取**ASP.NET MVC4 Web 應用程式**。 按一下 [確定 **Deploying Office Solutions**]。

在**新增 ASP.NET MVC 4 專案**對話方塊中，選取**單一網頁應用程式**。

在 [方案總管] 中，展開**檢視**資料夾，然後**首頁**資料夾。 以滑鼠右鍵按一下 Index.cshtml 檔案，然後選擇**Page Inspector 中的檢視**。

首先也就是顯示在 Page Inspector 瀏覽器是登入頁面。 按一下 「 註冊 」 並建立使用者名稱和密碼。 一旦您註冊時，應用程式會您登入，並建立一些範例項目待辦事項清單。

按一下**檢查**將 Page Inspector 在檢查模式中。 在 Page Inspector 瀏覽器中，按一下其中一個待辦項目。 請注意，而不是所反白顯示為藍色，項目以橙色醒目提示，與 「 JS"的項目名稱旁邊。 這表示透過指令碼建立的項目是以動態方式。

![](using-page-inspector-in-aspnet-mvc/_static/image44.png)

此外，橙色會出現底線上**呼叫堆疊** 索引標籤。這表示**呼叫堆疊**窗格就會有多個項目的資訊。

按一下**呼叫堆疊** 索引標籤。**呼叫堆疊** 窗格會顯示建立項目的 JavaScript 呼叫的呼叫堆疊。 呼叫外部程式庫例如 jQuery 會摺疊，使您可以輕鬆地看到您的應用程式指令碼的呼叫。

![](using-page-inspector-in-aspnet-mvc/_static/image46.png)

若要查看完整的堆疊，包括呼叫外部程式庫，您可以展開標示為 [外部程式庫] 的節點：

![](using-page-inspector-in-aspnet-mvc/_static/image48.png)

如果您按一下呼叫堆疊中的項目時，Visual Studio 開啟程式碼檔，並反白顯示對應的指令碼。

![](using-page-inspector-in-aspnet-mvc/_static/image50.png)

## <a name="see-also"></a>請參閱

[簡介 Visual Studio 中的 ASP.NET MVC 4](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) （ASP.net 網站）

[簡介頁面偵測器](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/)（Channel 9 影片）

[Page Inspector 錯誤訊息](https://go.microsoft.com/?linkid=9813062)(MSDN)
