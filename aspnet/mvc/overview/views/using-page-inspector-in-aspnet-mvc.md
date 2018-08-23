---
uid: mvc/overview/views/using-page-inspector-in-aspnet-mvc
title: 在 ASP.NET MVC 中使用 Page Inspector |Microsoft Docs
author: rick-anderson
description: Visual Studio 2012 中的 Page Inspector 是整合式瀏覽器的 web 開發工具。 在 Page Inspector i 與整合式瀏覽器中，選取任何項目...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: c7e4e1ab-4932-4614-9f53-aaf7c706d498
msc.legacyurl: /mvc/overview/views/using-page-inspector-in-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: c465b0bac9af90a892d6e62a327ba36977d08d4a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832027"
---
<a name="using-page-inspector-in-aspnet-mvc"></a>在 ASP.NET MVC 中使用 Page Inspector
====================
由 Tim Ammann

> Visual Studio 2012 中的 Page Inspector 是整合式瀏覽器的 web 開發工具。 在 整合式瀏覽器中，選取任何項目和 Page Inspector 立即會反白顯示項目的來源和 CSS。 您可以瀏覽任何的 MVC 檢視，快速尋找呈現的標記的來源並使用瀏覽器工具，直接在 Visual Studio 環境中。
> 
> [觀看影片](../../videos/mvc-4/using-page-inspector-in-aspnet-mvc.md)
> 
> 本教學課程會示範如何啟用檢查模式中，然後快速找出並編輯您的 web 專案內的標記和 CSS。 本教學課程使用 MVC 專案中，但您也可以使用的 Page Inspector [Web Form](https://go.microsoft.com/?linkid=9802001)和其他 ASP.NET 應用程式。
> 
> 本教學課程有下列各節：
> 
> - [必要條件](#_1_prerequisites)
> - [建立 Web 應用程式](#_2_creating_a)
> - [使用 Page Inspector 瀏覽至檢視](#_3_using_page)
> - [啟用檢查模式](#_4_inspection_mode)
> - [若要變更標記中使用 Page Inspector](#_5_using_page)
> - [檢查模式和 [HTML] 視窗](#_6_inspection_mode)
> - [在 [樣式] 視窗中預覽 CSS 變更](#_7_previewing_css)
> - [CSS 自動同步處理](#css_auto_sync)
> - [使用 CSS 色彩選擇器](#css_color_picker)
> - [將動態頁面項目對應至 JavaScript](#map_dynamic_elements)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a>必要條件

- [Visual Studio 2012](https://www.microsoft.com/visualstudio/11)或是[Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web)。

> [!NOTE]
> 若要取得最新版的 Page Inspector，請使用[Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386)安裝 Windows Azure SDK for.NET 2.0。


Page Inspector 隨附於 Microsoft Web Developer Tools。 1.3 為最新版本。 若要檢查哪些版本您有執行 Visual Studio，選取**關於 Microsoft Visual Studio**從**協助**功能表。

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a>建立 Web 應用程式

首先，建立您將會使用 Page Inspector 與 web 應用程式。 在 Visual Studio 中，選擇**檔案** &gt; **新專案**。 在左側，展開**Visual C#**，選取**Web**，然後選取**ASP.NET MVC4 Web 應用程式**。

![新的 ASP.NET MVC 應用程式](using-page-inspector-in-aspnet-mvc/_static/image2.png)

按一下 [確定 **Deploying Office Solutions**]。

在 **新的 ASP.NET MVC 4 專案**對話方塊中，選取**網際網路應用程式**。 離開**Razor**做為預設檢視引擎。

![新的 ASP.NET MVC 專案-網際網路應用程式](using-page-inspector-in-aspnet-mvc/_static/image4.png)

在應用程式中開啟**來源**檢視。

![原始碼檢視中的新 ASP.NET MVC 應用程式](using-page-inspector-in-aspnet-mvc/_static/image6.png)

既然您已使用的應用程式時，您可以使用 Page Inspector 檢查和修改它。

<a id="_starting_page_inspector"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-browse-to-a-view"></a>使用 Page Inspector 瀏覽至檢視

在 Visual Studio 2012 中，您可以以滑鼠右鍵按一下任何檢視在您專案中，選取**Page Inspector 中的檢視**，Page Inspector 會找出路由，並且顯示頁面。

在 [**方案總管] 中**，展開**檢視**資料夾，然後**首頁**資料夾。 以滑鼠右鍵按一下 Index.cshtml 檔案，然後選擇**Page Inspector 中的檢視**。

![Page Inspector 中檢視 Index.cshtml](using-page-inspector-in-aspnet-mvc/_static/image8.png)

根據預設，Page Inspector 停駐成為視窗左側的 Visual Studio 環境。 如果想要的話，您可以將它固定到其他位置，或取消停駐視窗。 請參閱[如何： 排列和停駐 Windows](https://msdn.microsoft.com/library/z4y0hsax.aspx)。

Page Inspector 視窗的上方窗格會顯示目前的網頁瀏覽器視窗中。 下方窗格會顯示 HTML 標記，以及可讓您檢查頁面的不同層面的某些索引標籤中的頁面。 下方窗格大致[F12 開發人員工具](https://msdn.microsoft.com/ie/aa740478)Internet Explorer 中。

![Page Inspector 中的 ASP.NET MVC 應用程式](using-page-inspector-in-aspnet-mvc/_static/image10.png)

在本教學課程中，您將使用**HTML**並**樣式**索引標籤，以快速瀏覽並對應用程式進行變更。

<a id="_examining_(&quot;decomposing&quot;)_the"></a><a id="_inspection_mode_and"></a><a id="_4_inspection_mode"></a>

## <a name="enableinspection-mode"></a>EnableInspection 模式

若要讓 Page Inspector 進入檢查模式，按一下**檢查** 按鈕。 在檢查模式中，當滑鼠指標停留在轉譯的頁面上的任何部分的對應的來源標記或程式碼會反白顯示。

![切換檢查模式](using-page-inspector-in-aspnet-mvc/_static/image12.png)

現在將滑鼠移 Page Inspector 中頁面的不同部分。 這麼做，將滑鼠指標變為大型的加號，和下方的項目會反白顯示：

![將滑鼠停留 div.content 包裝函式](using-page-inspector-in-aspnet-mvc/_static/image14.png)

當您移動滑鼠指標時，Visual Studio 就會反白顯示對應的 Razor 語法，在原始程式檔。 如果 HTML 項目來自於另一個原始程式檔，Visual Studio 會自動開啟檔案。

在 Page Inspector **HTML**索引標籤會顯示所產生的 Razor 語法的 HTML。 當您移動滑鼠指標時，會反白顯示的 HTML 項目。 **樣式**索引標籤會顯示項目的 CSS 規則。

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a>若要變更標記中使用 Page Inspector

Page Inspector 可讓您尋找其位置可能不明顯的標記。 然後您可以修改標記，並查看產生的變更。

若要查看這種情況，請按一下**檢查**，然後捲動至頁面底部的 [Page Inspector] 視窗中。

當您將滑鼠指標移到 [頁尾] 區域時，Page Inspector 隨即開啟\_Layout.cshtml 檔案並反白顯示您所選取的 [配置] 頁面的區段。 如您所見，頁尾會定義於版面配置檔，並不是檢視本身。

![頁尾](using-page-inspector-in-aspnet-mvc/_static/image16.png)

現在將您的滑鼠指標移入線條 copyright <a id="a"></a>注意到。 在 [ \_Layout.cshtml] 頁面上，對應的線條會反白顯示。

![頁尾的著作權列反白顯示](using-page-inspector-in-aspnet-mvc/_static/image18.png)

在行結尾加入一些文字\_Layout.cshtml 檔案。

&lt;p&gt;&amp;複製;@DateTime.Now.Year -撼動我的 ASP.NET MVC 應用程式 ！ &lt; /p&gt;

現在，按 Ctrl + Alt + Enter 或按一下 更新列，以查看 Page Inspector 瀏覽器視窗中的結果。

![我的 ASP.NET 應用程式 Rocks ！](using-page-inspector-in-aspnet-mvc/_static/image20.png)

您或許以為 Index.cshtml 中的頁尾定義，但它原來是在\_Layout.cshtml，並找到您的 Page Inspector。

<a id="_inspection_mode_and_1"></a><a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a>檢查模式和 [HTML] 視窗

接下來，您必須快速瀏覽 [HTML] 視窗，以及它如何為您對應項目。

按一下 **檢查**至 Page Inspector 進入檢查模式。

按一下 顯示 「 您 logohere 」 頁面的上半部。 您正在檢查詳細資料，因此不再會在瀏覽器視窗中的顯示變更，您將滑鼠游標移特定項的目。

現在將滑鼠指標移到**HTML**視窗。 當您移動滑鼠指標時，Page Inspector 概要說明中的項目**HTML**視窗並反白顯示對應的項目，在瀏覽器視窗。

![HTML 視窗](using-page-inspector-in-aspnet-mvc/_static/image22.png)

如往常一般，Page Inspector 隨即開啟\_Layout.cshtml 檔案供您在暫存的索引標籤中。按一下  \_Layout.cshtml 暫時索引標籤，以及對應的標記會反白顯示&lt;標頭&gt;為您的區段：

![反白顯示的標記](using-page-inspector-in-aspnet-mvc/_static/image24.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a>在 [樣式] 視窗中預覽 CSS 變更

接下來，您會使用 Page Inspector**樣式**預覽 CSS 變更的視窗。

按一下 **檢查**至 Page Inspector 進入檢查模式。

在 Page Inspector 瀏覽器視窗中，將滑鼠指標移到 「 首頁 」 一節**div.content 包裝函式**標籤會出現。

![將滑鼠停留 div.content 包裝函式](using-page-inspector-in-aspnet-mvc/_static/image26.png)

按一下 div.content 包裝函式區段內，，然後將滑鼠指標移到**樣式**視窗。 **樣式**視窗會顯示所有此項目的 CSS 規則。 請向下捲動尋找.featured.content 包裝函式類別選取器。 現在，請清除的背景色彩屬性的核取方塊。

![清除的背景色彩](using-page-inspector-in-aspnet-mvc/_static/image28.png)

請注意變更預覽立即的 Page Inspector 瀏覽器視窗中。

再次選取核取方塊，然後按兩下屬性值，並將它變更為紅色。 變更會立即顯示：

![紅色的背景色彩](using-page-inspector-in-aspnet-mvc/_static/image30.png)

**樣式**視窗可讓您輕鬆地測試並預覽 CSS 變更之前，您將變更認可到樣式表本身。

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a>CSS 自動同步處理

> [!NOTE]
> 這項功能需要 Page Inspector 1.3 版。


CSS 自動同步處理功能可讓您直接編輯 CSS 檔案並查看立即在 Page Inspector 瀏覽器中的變更。

按一下 **檢查**至 Page Inspector 進入檢查模式。

在 Page Inspector 瀏覽器中，將滑鼠指標移到 「 首頁 」 一節**div.content 包裝函式**標籤會出現。 再按一次就會選取這個項目。

**樣式**視窗會顯示所有此項目的 CSS 規則。 請向下捲動尋找.featured.content 包裝函式類別選取器。 按一下 「.featured.content-包裝函式 」。 Page Inspector 隨即開啟，定義此樣式 (Site.css)，並反白顯示對應的 CSS 樣式的 CSS 檔案。

![](using-page-inspector-in-aspnet-mvc/_static/image32.png)

現在的值變更`background-color`為"red"。 變更會立即出現在 Page Inspector 瀏覽器。

![](using-page-inspector-in-aspnet-mvc/_static/image34.png)

<a id="css_color_picker"></a>
## <a name="using-the-css-color-picker"></a>使用 CSS 色彩選擇器

在 Visual Studio 2012 CSS 編輯器有色彩選擇器，可讓您輕鬆地選擇並插入色彩。 色彩選擇器包含標準的調色盤的色彩、 支援標準的色彩名稱、 雜湊程式碼、 RGB、 RGBA、 HSL 和 HSLA 色彩，並維護一份您已使用最新文件中的色彩。

在上一個區段中，您可以變更的值`background-color`屬性。 要叫用色彩選擇器，將插入點之後的屬性名稱和類型**#** 或是**rgb (**。

![CSS 色彩選擇器列](using-page-inspector-in-aspnet-mvc/_static/image36.png)

按一下色彩，以選取它，或按向下鍵，然後使用左邊和右邊的方向鍵來周遊的色彩。 當您造訪的色彩時，可預覽對應的十六進位值：

![預覽的背景色彩屬性值](using-page-inspector-in-aspnet-mvc/_static/image38.png)

如果在色軸沒有您想要的確切色彩，您可以使用色彩選擇器快顯清單。 若要開啟它，請按一下雙 > 形箭號，最右邊的 [色彩] 列中，或一兩次按鍵盤上的向下箭號。

![CSS 色彩選擇器快顯清單](using-page-inspector-in-aspnet-mvc/_static/image40.png)

按一下右側的垂直列的色彩。 這會在主視窗中顯示該色彩的漸層。 藉由按下 Enter，選擇直接從直條的色彩，或按一下 選擇更精確的主視窗中的任何時間點。

如果您想要使用的電腦螢幕上沒有色彩 （它不一定要在 Visual Studio 使用者介面），您可以使用滴管工具右下角來擷取其值。

您也可以移動滑桿底端的色彩選擇器來變更色彩的不透明度。 這樣做的變更色彩值的 RGBA 值，因為 RGBA 格式可以代表不透明度。

選取色彩之後，按下 Enter、，然後輸入 完成中的背景色彩項目分號*Site.css*檔案。

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a>頁面偵測器更新列

Page Inspector 立即偵測到的變更*Site.css*檔案，並在 更新列會顯示警示。

![更新列](using-page-inspector-in-aspnet-mvc/_static/image42.png)

若要儲存所有檔案，並重新整理 Page Inspector 瀏覽器，請按 Ctrl + Alt + Enter 或按一下更新列。 反白顯示色彩的變更會出現在瀏覽器中。

<a id="map_dynamic_elements"></a>
## <a name="mapping-dynamic-page-elements-to-javascript"></a>將動態頁面項目對應至 JavaScript

現代化 web 應用程式，在網頁中的項目通常動態產生使用 JavaScript。 這表示沒有靜態標記 （HTML 或 Razor） 對應至這些頁面項目。

1.3 版中，使用 Page Inspector 可以現在對應的頁面回傳至相對應的 JavaScript 程式碼以動態方式新增的項目。 為了示範這項功能，我們將使用[單一頁面應用程式 (SPA) 範本](../../../single-page-application/overview/introduction/knockoutjs-template.md)。

> [!NOTE]
> SPA 範本需要[ASP.NET 和 Web 工具 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650)更新。


在 Visual Studio 中，選擇**檔案** &gt; **新專案**。 在左側，展開**Visual C#**，選取**Web**，然後選取**ASP.NET MVC4 Web 應用程式**。 按一下 [確定 **Deploying Office Solutions**]。

在 **新的 ASP.NET MVC 4 專案**對話方塊中，選取**單一頁面應用程式**。

在 [方案總管] 中，展開**檢視**資料夾，然後**首頁**資料夾。 以滑鼠右鍵按一下 Index.cshtml 檔案，然後選擇**Page Inspector 中的檢視**。

在 Page Inspector 瀏覽器中第一件事也就是顯示為登入頁面。 按一下 「 註冊 」，並建立使用者名稱和密碼。 一旦您註冊，應用程式登入，並建立一些範例項目待辦事項清單。

按一下 **檢查**至 Page Inspector 進入檢查模式。 在 Page Inspector 瀏覽器中，按一下 待辦事項項目。 請注意，而不是正在以藍色強調顯示，項目以橙色醒目提示，使用"JS"的項目名稱旁邊。 這表示透過指令碼建立的項目是以動態方式。

![](using-page-inspector-in-aspnet-mvc/_static/image44.png)

此外，橙色底線出現在**呼叫堆疊** 索引標籤。這表示**呼叫堆疊**窗格就會有項目相關的詳細資訊。

按一下 **呼叫堆疊** 索引標籤。**呼叫堆疊** 窗格會顯示建立項目的 JavaScript 呼叫的呼叫堆疊。 呼叫外部程式庫例如 jQuery 會摺疊，以便您可以輕易看到您的應用程式指令碼的呼叫。

![](using-page-inspector-in-aspnet-mvc/_static/image46.png)

若要查看完整的堆疊，包括呼叫外部程式庫，您可以展開標示為 [外部程式庫] 的節點：

![](using-page-inspector-in-aspnet-mvc/_static/image48.png)

如果您按一下呼叫堆疊中的項目時，Visual Studio 開啟程式碼檔，並反白顯示對應的指令碼。

![](using-page-inspector-in-aspnet-mvc/_static/image50.png)

## <a name="see-also"></a>另請參閱

[使用 Visual Studio 的 ASP.NET MVC 4 簡介](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md)（ASP.net 網站）

[簡介 Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) （Channel 9 影片）

[Page Inspector 錯誤訊息](https://go.microsoft.com/?linkid=9813062)(MSDN)
