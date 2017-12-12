---
uid: web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
title: "Visual Studio 2012 中 ASP.NET Web form 使用 Page Inspector |Microsoft 文件"
author: rick-anderson
description: "Visual Studio 2012 Page Inspector 是使用整合式瀏覽器的 web 開發工具。 選取整合式瀏覽器和 Page Inspector 中的任何項目..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: 2ece0bf4-aae5-4ff4-8f62-28e0819d4f86
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: a2ac8334e62e6ab7af7042572cfd5950c687001b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a>使用 Page Inspector 的 ASP.NET Web Form 中的 Visual Studio 2012
====================
由 Tim Ammann

> Visual Studio 2012 Page Inspector 是使用整合式瀏覽器的 web 開發工具。 在整合式瀏覽器中，選取任何項目和 Page Inspector 立即會反白顯示項目的來源和 CSS。 您可以瀏覽您的應用程式中的任何頁面、 快速尋找來源呈現標記，並使用瀏覽器工具，Visual Studio 環境內的權限。
> 
> 此教學課程 shwos 如何啟用檢查模式，然後快速找出並編輯 CSS 規則和您的 web 專案中的文字。 本教學課程會使用 Web Form 應用程式專案中，但您也可以使用 Page Inspector 適用於網站專案和[MVC](https://go.microsoft.com/?linkid=9802002)應用程式。
> 
> 本教學課程包含下列各節：
> 
> [必要條件](#_1_prerequisites)
> 
> [建立 Web 應用程式](#_2_creating_a)
> 
> [若要檢視應用程式中使用 Page Inspector](#_3_using_page)
> 
> [啟用檢查模式](#_4_inspection_mode)
> 
> [若要變更標記使用 Page Inspector](#_5_using_page)
> 
> [檢查模式和 [HTML] 視窗](#_6_inspection_mode)
> 
> [在 [樣式] 視窗中的預覽 CSS 變更](#_7_previewing_css)
> 
> [CSS 自動同步處理](#css_auto_sync)
> 
> [使用 CSS 色彩選擇器](#css_color_picker)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a>必要條件

- [Visual Studio 2012](https://www.microsoft.com/visualstudio/11/en-us)或[Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/en-us/downloads#express-web)。

> [!NOTE]
> 若要取得 Page Inspector 的最新版本，請使用[Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386)安裝 Azure SDK for.NET 2.0。


Page Inspector 隨附 Microsoft Web Developer Tools。 最新版本是 1.3。 若要檢查的版本中，執行 Visual Studio 也可以選取**關於 Microsoft Visual Studio**從**協助**功能表。

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a>建立 Web 應用程式

首先，您將建立會使用 Page Inspector 與 web 應用程式。 在 Visual Studio 中，選擇 **檔案** &gt; **新專案**。 在左邊展開**Visual C#**，選取**Web**，然後選取**ASP.NET Web Form 應用程式**。

![新的 Web Form 應用程式](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

按一下 [確定]。

應用程式中開啟**來源**檢視。

![新的 Web Form 應用程式原始碼檢視中](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

既然您已使用應用程式，您可以使用 Page Inspector 檢查和修改它。

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a>若要檢視應用程式中使用 Page Inspector

接下來，您將檢視 Page inspector 應用程式。 在**方案總管 中**，以滑鼠右鍵按一下專案，然後選擇**Page Inspector 中的檢視**。

![Page Inspector 中的檢視](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

根據預設，第一次，啟動 Page Inspector 時是停駐為窄視窗左邊的 Visual Studio 環境。 不要將它停駐在左邊，並將其設定能夠承受，或將它其中一個工具區域中停駐在頂端、 底部或右邊的寬度：

![Page Inspector 停駐位置](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

如果您卸除的頁面偵測器視窗，您可以將其放外部 Visual Studio 中，或甚至在第二個監視器上如果有的話。 不過，為了 ALT + TAB，Page Inspector 與 Visual Studio 之間未停駐 Page Inspector 視窗時，請移至**工具** &gt; **選項** &gt; **環境** &gt; **索引標籤和視窗**，然後在 **索引標籤也**，清除核取方塊為**浮動工具視窗一律保持最上層的主視窗**:

![清除浮動工具視窗核取方塊以 Visual Studio 和卸除的 Page Inspector 視窗之間的 ALT + TAB](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

頁面偵測器視窗的上方窗格會顯示目前的網頁瀏覽器視窗中。 下方窗格會顯示頁面的左側的 HTML 標記中，並在右側，可讓您的某些索引標籤檢查頁面的不同層面。 下方窗格會類似於[F12 開發人員工具](https://msdn.microsoft.com/en-us/ie/aa740478)Internet Explorer 中。 （不過，不同於開發人員工具 中，您可以使用 Page Inspector 在 Visual Studio 中的權限。）

![Page Inspector](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

在此教學課程中，您將使用 [Page Inspector 瀏覽器] 窗格中，而**HTML**和**樣式**索引標籤，以協助您快速瀏覽，並對應用程式中的變更。

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a>啟用檢查模式

接下來，您會看到 Page Inspector 檢查模式的運作方式。 在頁面偵測器視窗中，按一下 [**檢查**] 按鈕。

![檢查項目](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

若要查看檢查模式中的動作，請將滑鼠移 Page Inspector 瀏覽器視窗中頁面的不同部分。 當您這樣做，滑鼠指標會變成一個大型的加號和下方的項目會反白顯示：

![將滑鼠停留 div.content 包裝函式](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

當您移動滑鼠指標，請注意，

- 中的內容**來源**檢視變更為顯示對應至選取的項目頁面上的標記。 相關的標記會反白顯示。 如果來源是另一個檔案中，該檔案會開啟原始碼檢視中，以反白顯示相關的標記。

- 中所顯示的標記**HTML** Page Inspector 中的索引標籤也會變更為對應於頁面上選取的項目。 在**HTML**索引標籤上，相關的標記所述。

- **樣式**索引標籤會顯示 CSS 規則適用於目前的選取範圍。

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a>若要變更標記使用 Page Inspector

現在您會看到如何使用 Page Inspector 來尋找和標記或文字的位置可能不會立即進行變更。

將 Page Inspector 放在檢查模式中，然後捲動至 [首頁] 的底部。

只要您輸入的頁尾區域時，Page Inspector 隨即開啟*Site.Master*版面配置中的檔案**來源**右邊的其他暫存索引標籤中檢視索引標籤，並反白顯示的主要區段頁面上，您已選取。 這會顯示如何尋找並顯示實際上可能來自於不同的檔案比原先開啟一個頁面上的內容頁面偵測器。

![頁尾會在檢查模式中反白顯示](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

在 Page Inspector 瀏覽器視窗中，將滑鼠指標移線條 copyright <a id="a"></a>會注意到。

在*Site.Master*頁面上，對應的線條會反白顯示。

![頁尾著作權列反白顯示](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

中的一行的結尾加入一些文字*Site.Master*檔案。

&lt;p&gt;&amp;複製;&lt;%: DateTime.Now.Year %&gt; -我的 ASP.NET 應用程式有岩石 ！&lt;/ p&gt;

現在，請按 Ctrl + Alt + Enter 或按一下 [更新] 列，請參閱 Page Inspector 瀏覽器視窗中的結果。

![我的 ASP.NET 應用程式岩石 ！](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

您想頁尾是在*Default.aspx*  頁面上，但它已在主要的版面配置 頁面上，和 Page Inspector 則會為您找到它。

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a>檢查模式和 [HTML] 視窗

接下來，您必須快速瀏覽 [HTML] 視窗，以及它如何為您對應項目。

將 Page Inspector 放在檢查模式中。

![檢查項目](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

按一下網頁顯示 「 您的標誌在此 「 的上半部。 您正在檢查詳細資料，因此，在瀏覽器視窗中的顯示無法再變更當您移動滑鼠指標的特定項的目。

現在將滑鼠指標移至**HTML**視窗。 當您移動滑鼠指標時，Page Inspector 概要說明中的項目**HTML**視窗並反白顯示瀏覽器視窗中的對應項目。

![HTML 視窗](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

如往常一般，Page Inspector 隨即開啟*Site.Master*為您在暫時性索引標籤中的檔案。Site.Master 索引標籤，並以反白顯示對應的標記&lt;標頭&gt;> 一節：

![反白顯示的標記](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a>在 [樣式] 視窗中的預覽 CSS 變更

接下來，您會看到如何使用 Page Inspector**樣式**視窗來預覽 CSS 所做的變更。

按一下**檢查** 按鈕，將 Page Inspector 在檢查模式中。

在 Page Inspector 瀏覽器視窗中，將滑鼠指標移 「 首頁 」 一節，直到**div.content 包裝函式**標籤會出現。

![將滑鼠游標停留在項目](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

按一下 div.content 包裝函式區段內，，然後將滑鼠指標移至**樣式**視窗。 .Featured.content 包裝函式類別選取器，在清除並選取背景色彩屬性的核取方塊。

![清除的背景色彩](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

請注意變更預覽立即的 Page Inspector 瀏覽器視窗中。

再次選取核取方塊，然後按兩下屬性值，並將它變更為`red`。 變更會立即顯示：

![紅色的背景色彩](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

**樣式**視窗讓它易於測試和預覽 CSS 之前變更了您將變更認可到樣式表本身。

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a>CSS 自動同步處理

> [!NOTE]
> 此功能需要 Page Inspector 1.3 版。


CSS 自動同步處理功能可讓您直接編輯 CSS 檔案，並查看立即在 Page Inspector 瀏覽器中的變更。

按一下**檢查**將 Page Inspector 在檢查模式中。

Page Inspector 瀏覽器中將滑鼠指標移 「 首頁 」 一節，直到**div.content 包裝函式**標籤會出現。 按一下以選取此項目。

**Syles**  視窗會顯示所有此項目的 CSS 規則。 向下捲動至 尋找.featured.content 包裝函式類別選取器。 按一下 「.featured.content-包裝函式 」。 Page Inspector 開啟定義這個樣式 (Site.css)，並反白顯示對應的 CSS 樣式的 CSS 檔案。

![CSS 檔案](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

值現在變更`background-color`至"red"。 變更會立即出現在 Page Inspector 瀏覽器。

![Page Inspector 瀏覽器](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a>使用 CSS 色彩選擇器

接下來，您將學習如何使用 Page Inspector 快速找出並變更預設應用程式中反白顯示文字的 CSS。 在此範例中，您已決定，您不喜歡的藍色醒目提示，而且想要將它變更為另一種色彩。

按一下**檢查** 按鈕。

![檢查項目](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

在 Page Inspector 瀏覽器視窗中，將滑鼠指標移反白顯示 「 視訊、 教學課程和範例 」 文字，讓 CSS 的 「 標記 」 標籤會出現。

![將滑鼠游標停留在標記項目](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

按一下以選取它的文字。 對應的 CSS 標記選取器會出現在底部**樣式**視窗。

![在 [樣式] 視窗中的標記選取器](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

按一下標記選取器。 這會開啟*Site.css* web 應用程式檔案。 Site.css 索引標籤，並對應的 CSS 選取器會反白顯示：

![標記樣式表中的選取器](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

選取並移除背景色彩屬性的線條。

您現在將使用新的 Visual Studio 2012 CSS 色彩選擇器來選擇新的色彩**標示**背景色彩屬性。

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a>使用 Visual Studio 2012 CSS 色彩選擇器

CSS 編輯器，Visual Studio 2012 中的會有色彩選擇器，可讓您輕鬆地選擇及插入色彩。 它具有簡單的色軸和 「 快顯停擺 」 選擇器可提供較細微的控制。

色彩選擇器包含標準的調色盤的色彩、 支援標準的色彩名稱、 雜湊程式碼、 RGB、 RGBA、 HSL 和 HSLA 色彩，以及維護一份您在文件最近使用的色彩。

在背景色彩屬性所在的列，請輸入"bc"，再按一次向下箭號。

當您輸入的每個單字的第一個字元連字號分隔的屬性，例如 [背景色彩] 中時，IntelliSense 會篩選清單，為您顯示符合的屬性：

![Intellisense 篩選值](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

現在請輸入冒號。 當您這樣做時，則會插入完整的背景色彩屬性名稱。 型別 **#** 或**rgb (**，及色彩選擇器列就會出現：

![CSS 色彩選擇器列](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

色彩選擇器的運作方式，請按一下其色彩與滑鼠指標，或按向下鍵，然後使用左邊和右邊的方向鍵來周遊色彩。 當您瀏覽色彩時，預覽的背景色彩屬性的對應值：

![預覽的背景色彩屬性值](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

此時，您無法按下 Enter，以選取值，然後完成 CSS 項目分號 （;）。 現在，請移至下一個區段，讓您可以查看色彩選擇器快顯清單的運作方式。

#### <a name="using-the-color-picker-pop-down"></a>使用 色彩選擇器快顯清單

當在色軸沒有您想要尋求完全色彩時，您可以使用色彩選擇器快顯清單。

若要開啟它，請按一下色軸，右端 double > 形箭號或次按鍵盤上的向下箭號。

![CSS 色彩選擇器快顯清單](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

按一下右側的垂直列中的色彩。 這會在主視窗中顯示該色彩漸層。 按下 Enter，直接從垂直列中選擇的色彩，或按一下任何一點，而精確度卻選擇主視窗中。

如果您想要使用您電腦螢幕上沒有的色彩 （它不一定要在 Visual Studio 使用者介面內），您可以使用滴管工具右下方的擷取其值。

您也可以移動底部的色彩選擇器的滑桿來變更色彩的不透明度。 這麼做會變更色彩 RGBA 值的值，因為 RGBA 格式可以代表不透明度。

您已選擇色彩之後，按 Enter 鍵，並接著輸入分號以完成中的背景色彩項目*Site.css*檔案。

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a>頁面偵測器更新列

Page Inspector 可以立即偵測到變更*Site.css*檔案 （或應用程式中的任何檔案），並更新列中顯示警示。

![更新列](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

若要儲存您的檔案，並重新整理 Page Inspector 瀏覽器，請按 Ctrl + Alt + Enter 或按一下更新列。 反白顯示色彩的變更會出現在瀏覽器：

![反白顯示色彩變更](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a>請注意您方便地重新整理 Page Inspector 瀏覽器直接從 Visual Studio 環境中。 而不外部瀏覽器中使用 Page Inspector 可讓您保持在編輯器中，當您開發 web 應用程式。

## <a name="see-also"></a>另請參閱

[簡介頁面偵測器](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector)（Channel 9 影片）

[Page Inspector 錯誤訊息](https://go.microsoft.com/?linkid=9813062)(MSDN)
