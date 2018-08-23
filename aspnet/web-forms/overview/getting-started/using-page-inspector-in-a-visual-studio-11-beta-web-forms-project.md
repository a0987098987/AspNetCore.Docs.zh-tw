---
uid: web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
title: 使用 Page Inspector，Visual Studio 2012，在 ASP.NET Web form |Microsoft Docs
author: rick-anderson
description: Page Inspector for Visual Studio 2012 是 web 開發工具與整合式瀏覽器。 在 Page Inspector 的整合式瀏覽器中，選取任何項目...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: 2ece0bf4-aae5-4ff4-8f62-28e0819d4f86
msc.legacyurl: /web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: d2c377f8466f8f324b75ce60860aa00c11bc0ffe
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825821"
---
<a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a>在 ASP.NET Web Form 中的 Visual Studio 2012 中使用 Page Inspector
====================
由 Tim Ammann

> Page Inspector for Visual Studio 2012 是 web 開發工具與整合式瀏覽器。 在 整合式瀏覽器中，選取任何項目和 Page Inspector 立即會反白顯示項目的來源和 CSS。 您可以瀏覽您的應用程式中的任何頁面、 快速尋找呈現的標記的來源並使用瀏覽器工具，直接在 Visual Studio 環境中。
> 
> 本教學課程的 shwos 如何啟用檢查模式中，然後快速找出和編輯 CSS 規則與您的 web 專案內的文字。 本教學課程會使用 Web Forms 應用程式專案，但您也可以使用 Page Inspector 適用於網站專案和[MVC](https://go.microsoft.com/?linkid=9802002)應用程式。
> 
> 本教學課程有下列各節：
> 
> [必要條件](#_1_prerequisites)
> 
> [建立 Web 應用程式](#_2_creating_a)
> 
> [若要檢視應用程式中使用 Page Inspector](#_3_using_page)
> 
> [啟用檢查模式](#_4_inspection_mode)
> 
> [若要變更標記中使用 Page Inspector](#_5_using_page)
> 
> [檢查模式和 [HTML] 視窗](#_6_inspection_mode)
> 
> [在 [樣式] 視窗中預覽 CSS 變更](#_7_previewing_css)
> 
> [CSS 自動同步處理](#css_auto_sync)
> 
> [使用 CSS 色彩選擇器](#css_color_picker)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a>必要條件

- [Visual Studio 2012](https://www.microsoft.com/visualstudio/11)或是[Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web)。

> [!NOTE]
> 若要取得最新版的 Page Inspector，請使用[Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386)來安裝 Azure SDK for.NET 2.0。


Page Inspector 隨附於 Microsoft Web Developer Tools。 1.3 為最新版本。 若要檢查哪些版本您有執行 Visual Studio，選取**關於 Microsoft Visual Studio**從**協助**功能表。

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a>建立 Web 應用程式

首先，您將建立您將會使用 Page Inspector 與 web 應用程式。 在 Visual Studio 中，選擇**檔案** &gt; **新專案**。 在左側，展開**Visual C#**，選取**Web**，然後選取**ASP.NET Web Forms 應用程式**。

![新 Web Forms 應用程式](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

按一下 [確定 **Deploying Office Solutions**]。

在應用程式中開啟**來源**檢視。

![原始碼檢視中的新 Web Forms 應用程式](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

既然您已使用的應用程式時，您可以使用 Page Inspector 檢查和修改它。

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a>若要檢視應用程式中使用 Page Inspector

接下來，您將檢視 Page inspector 應用程式。 在 **方案總管**，以滑鼠右鍵按一下專案，然後選擇**Page Inspector 中的檢視**。

![Page Inspector 中的檢視](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

根據預設，第一次，啟動的 Page Inspector 時停駐為窄視窗左側的 Visual Studio 環境。 將它停駐在左邊，並將它設定為適合您，或使它其中一個工具區域中停駐在頂端、 底部或右邊的寬度：

![Page Inspector 停駐位置](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

如果您取消停駐的頁面偵測器視窗，您可以將它放 Visual Studio 外部，或甚至是在第二個監視器上如果有的話。 不過，為了 ALT + TAB，Page Inspector 和 Visual Studio 之間的頁面偵測器視窗停駐時，移至**工具** &gt; **選項** &gt; **環境** &gt; **索引標籤和 Windows**，然後在**索引標籤也**，清除核取方塊稱為**浮動工具視窗一律保持最上層的主視窗**:

![清除 浮動工具視窗核取方塊，按 ALT + TAB 鍵會與 Visual Studio 未停駐的視窗中，Page Inspector](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

Page Inspector 視窗的上方窗格會顯示目前的網頁瀏覽器視窗中。 下方窗格則顯示頁面的左側的 HTML 標記中，並在右側，可讓您的某些索引標籤檢查頁面的不同層面。 下方窗格大致[F12 開發人員工具](https://msdn.microsoft.com/ie/aa740478)Internet Explorer 中。 （不過，不同於開發人員工具中，您可以使用 Page Inspector 在 Visual Studio 內的權限。）

![Page Inspector](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

在本教學課程中，您會使用 Page Inspector 瀏覽器 窗格中，而**HTML**並**樣式**索引標籤，以協助您快速瀏覽並對應用程式中的變更。

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a>啟用檢查模式

接下來，您會看到 Page Inspector 檢查模式中的運作方式。 在 Page Inspector 視窗中，按一下**檢查** 按鈕。

![檢查項目](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

若要查看作用中的檢查模式，將滑鼠移到不同的部分，Page Inspector 瀏覽器視窗內的頁面上。 這麼做，將滑鼠指標變為大型的加號，和下方的項目會反白顯示：

![將滑鼠停留 div.content 包裝函式](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

當您移動滑鼠指標時，請注意，

- 中的內容**來源**檢視變更為顯示對應至所選的項目頁面上的標記。 相關的標記會反白顯示。 如果來源是在另一個檔案，該檔案會開啟原始碼檢視中，以反白顯示相關的標記。

- 中所顯示的標記**HTML** Page Inspector 中的索引標籤也會變更為對應至頁面上選取的元素。 在 [ **HTML** ] 索引標籤，相關的標記會簡要說明。

- **樣式**索引標籤會顯示 CSS 規則與目前的選取範圍。

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a>若要變更標記中使用 Page Inspector

現在您會看到如何使用 Page Inspector 尋找及變更標記或可能無法立即看出其位置的文字。

Page Inspector 進入檢查模式，然後捲動至底部的 [首頁] 頁面。

只要您輸入的頁尾區域時，Page Inspector 隨即開啟*Site.Master*版面配置中的檔案**來源**暫時右邊的其他索引標籤中檢視索引標籤，並反白顯示的主要區段頁面上，您已選取。 這會顯示如何尋找並顯示實際上可能來自不同的檔案比原本所開啟的頁面上的內容 Page Inspector。

![頁尾會反白顯示在檢查模式](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

在 Page Inspector 瀏覽器視窗中，將滑鼠指標移著作權那一行<a id="a"></a>注意到。

在 [ *Site.Master* ] 頁面上，對應的線條會反白顯示。

![頁尾的著作權列反白顯示](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

加入一些文字中的行結尾*Site.Master*檔案。

&lt;p&gt;&amp;複製;&lt;%: DateTime.Now.Year %&gt; -我的 ASP.NET 應用程式 Rocks ！&lt;/ p&gt;

現在，按 Ctrl + Alt + Enter 或按一下 更新列，以查看 Page Inspector 瀏覽器視窗中的結果。

![我的 ASP.NET 應用程式 Rocks ！](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

您或許以為頁尾所在*Default.aspx*  頁面上，但它原來是在主要的版面配置頁面中，和 Page Inspector 則會為您找到它。

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a>檢查模式和 [HTML] 視窗

接下來，您必須快速瀏覽 [HTML] 視窗，以及它如何為您對應項目。

將 Page Inspector 放在檢查模式中。

![檢查項目](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

按一下 顯示 「 您標誌的位置 」 頁面的上半部。 您正在檢查詳細資料，因此不再會在瀏覽器視窗中的顯示變更，您將滑鼠游標移特定項的目。

現在將滑鼠指標移到**HTML**視窗。 當您移動滑鼠指標時，Page Inspector 概要說明中的項目**HTML**視窗並反白顯示對應的項目，在瀏覽器視窗。

![HTML 視窗](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

如往常一般，Page Inspector 隨即開啟*Site.Master*您暫時索引標籤中的檔案。Site.Master 索引標籤，並以反白顯示對應的標記&lt;標頭&gt;區段：

![反白顯示的標記](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a>在 [樣式] 視窗中預覽 CSS 變更

接下來，您會看到如何使用 Page Inspector**樣式**預覽 CSS 變更的視窗。

按一下 **檢查**Page Inspector 進入檢查模式的按鈕。

在 Page Inspector 瀏覽器視窗中，將滑鼠指標移到 「 首頁 」 一節**div.content 包裝函式**標籤會出現。

![將滑鼠停留項目](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

按一下 div.content 包裝函式區段內，，然後將滑鼠指標移到**樣式**視窗。 底下的.featured.content 包裝函式類別選取器中，清除，然後選取背景色彩屬性的核取方塊。

![清除的背景色彩](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

請注意變更預覽立即的 Page Inspector 瀏覽器視窗中。

再次選取核取方塊，然後按兩下屬性值，並將它變更為`red`。 變更會立即顯示：

![紅色的背景色彩](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

**樣式**視窗可讓您輕鬆地測試並預覽 CSS 變更之前，您將變更認可到樣式表本身。

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a>CSS 自動同步處理

> [!NOTE]
> 這項功能需要 Page Inspector 1.3 版。


CSS 自動同步處理功能可讓您直接編輯 CSS 檔案並查看立即在 Page Inspector 瀏覽器中的變更。

按一下 **檢查**至 Page Inspector 進入檢查模式。

在 Page Inspector 瀏覽器中，將滑鼠指標移到 「 首頁 」 一節**div.content 包裝函式**標籤會出現。 再按一次就會選取這個項目。

**樣式**視窗會顯示所有此項目的 CSS 規則。 請向下捲動尋找.featured.content 包裝函式類別選取器。 按一下 「.featured.content-包裝函式 」。 Page Inspector 隨即開啟，定義此樣式 (Site.css)，並反白顯示對應的 CSS 樣式的 CSS 檔案。

![CSS 檔案](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

現在的值變更`background-color`為"red"。 變更會立即出現在 Page Inspector 瀏覽器。

![Page Inspector 瀏覽器](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a>使用 CSS 色彩選擇器

接下來，您將了解如何快速地尋找和變更 CSS 來反白顯示的文字，在預設應用程式中使用 Page Inspector。 在此範例中，您已決定，您不喜歡藍色反白顯示，且想要將它變更為另一種色彩。

按一下 [**檢查**] 按鈕。

![檢查項目](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

在 Page Inspector 瀏覽器視窗中，將滑鼠指標移反白顯示 」 影片、 教學課程和範例 」 文字，讓 CSS 的 「 標記 」 標籤會出現。

![將滑鼠游標停留在標記項目](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

按一下以選取它的文字。 對應的 CSS 標記選取器出現在底部**樣式**視窗。

![在 [樣式] 視窗中的標記選取器](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

按一下標記選取器。 這會開啟*Site.css* web 應用程式檔案。 Site.css 索引標籤，並反白顯示對應的 CSS 選取器：

![標記樣式表中的選取器](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

選取並移除該程式行使用的背景色彩屬性。

您現在會使用新的 Visual Studio 2012 CSS 色彩選擇器來選擇的新色彩**標示**背景色彩屬性。

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a>使用 Visual Studio 2012 CSS 色彩選擇器

在 Visual Studio 2012 CSS 編輯器有色彩選擇器，可讓您輕鬆地選擇並插入色彩。 它具有簡單的色軸和 「 pop 停擺 」 選擇器，可提供較細微的控制。

色彩選擇器包含標準的調色盤的色彩、 支援標準的色彩名稱、 雜湊程式碼、 RGB、 RGBA、 HSL 和 HSLA 色彩，並維護一份您已使用最新文件中的色彩。

在一行中的背景色彩屬性，輸入"bc"然後按向下箭號一次。

當您輸入連字號分隔的屬性，例如 [背景色彩] 中的每個單字的第一個字元時，則 IntelliSense 會篩選清單，為您顯示符合的屬性：

![Intellisense 篩選值](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

現在，請輸入一個冒號。 當您這樣做時，則會插入完整的背景色彩屬性名稱。 型別**#** 或是**rgb (**，並會出現色彩選擇器列：

![CSS 色彩選擇器列](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

若要查看色彩選擇器列的運作方式，按一下滑鼠指標，其色彩或按向下鍵，然後使用左邊和右邊的方向鍵來周遊的色彩。 當您造訪的色彩時，可預覽的背景色彩屬性的對應值：

![預覽的背景色彩屬性值](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

此時，您可以按 Enter，以選取值，然後完成 CSS 項目以分號 （;）。 現在，請繼續下一節，讓您可以看到色彩選擇器快顯清單中的運作方式。

#### <a name="using-the-color-picker-pop-down"></a>使用 色彩選擇器快顯清單

時的色軸沒有您要尋找的確切色彩，您可以使用色彩選擇器快顯清單。

若要開啟它，請按一下雙 > 形箭號，最右邊的 [色彩] 列中，或一兩次按鍵盤上的向下箭號。

![CSS 色彩選擇器快顯清單](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

按一下右側的垂直列的色彩。 這會在主視窗中顯示該色彩的漸層。 藉由按下 Enter，選擇直接從直條的色彩，或按一下 選擇更精確的主視窗中的任何時間點。

如果您想要使用的電腦螢幕上沒有色彩 （它不一定要在 Visual Studio 使用者介面），您可以使用滴管工具右下角來擷取其值。

您也可以移動滑桿底端的色彩選擇器來變更色彩的不透明度。 這樣變更色彩的 RGBA 值的值，因為 RGBA 格式可以代表不透明度。

選取色彩之後，按下 Enter、，然後輸入 完成中的背景色彩項目分號*Site.css*檔案。

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a>頁面偵測器更新列

Page Inspector 立即偵測到的變更*Site.css*檔案 （或應用程式中的任何檔案） 並在 更新列中顯示警示。

![更新列](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

若要儲存所有檔案，並重新整理 Page Inspector 瀏覽器，請按 Ctrl + Alt + Enter 或按一下更新列。 反白顯示色彩的變更會出現在瀏覽器：

![反白顯示色彩變更](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a>請注意您方便地重新整理 Page Inspector 瀏覽器直接從 Visual Studio 環境內。 而不外部瀏覽器中使用 Page Inspector 可讓您保持在編輯器中，當您開發您的 web 應用程式。

## <a name="see-also"></a>另請參閱

[簡介 Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) （Channel 9 影片）

[Page Inspector 錯誤訊息](https://go.microsoft.com/?linkid=9813062)(MSDN)
