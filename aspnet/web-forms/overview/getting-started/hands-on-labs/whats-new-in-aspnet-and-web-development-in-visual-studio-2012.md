---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
title: 在 ASP.NET 和 Visual Studio 2012 中的 Web 程式開發中最新消息 |Microsoft 文件
author: rick-anderson
description: 新版的 Visual Studio 導入了一些增強功能的重點在於提升體驗和效能，當使用 Web 技術...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 6d40d276-1642-4a77-b6c9-02ac914f6805
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 00b43cc548df44edded925521991a095ed856494
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="whats-new-in-aspnet-and-web-development-in-visual-studio-2012"></a>在 ASP.NET 和 Visual Studio 2012 中的 Web 程式開發中最新消息
====================
由[Web 營小組](https://twitter.com/webcamps)

> 新版的 Visual Studio 導入了一些增強功能的重點在於提升使用 Web 技術時的體驗和效能。 CSS、 JavaScript 和 HTML 的 visual Studio 編輯器已完全改寫包含許多最要求中的程式碼的輔助，IntelliSense 和自動縮排等。 關於效能，統合及縮製現在已整合為內建功能，以便輕鬆地減少頁面載入時間。
> 
> Visual Studio 可讓您能夠使用最新的網站技術。 您可以使用跨瀏覽器 CSS3 程式碼片段，並確定您的網站同時也利用新的 HTML5 項目和功能的運作無論用戶端平台。
> 
> 撰寫和分析 JavaScript 程式碼應該容易，因為這個 Visual Studio 版本。 IntelliSense 清單整合式的 XML 文件集和導覽功能，現在也適用於 JavaScript 程式碼。 現在，您會有 JavaScript 型錄垂手可得。 此外，您可以檢查 ECMAScript5 相容性與您的指令碼，而且在早期階段偵測出語法錯誤。
> 
> 上一次，但並非最不重要，內建統合及縮製，也會實作這個 Visual Studio 版本。 將封裝您的指令碼檔案和樣式表，並壓縮使站台執行的速度快。
> 
> 這個實驗室將引導您完成先前所述將微幅變更套用至來源資料夾中提供的範例 Web 應用程式的新功能與增強功能。
> 
> 所有的範例程式碼和程式碼片段會包含在 Web 營訓練套件，可在[ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409)。


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>目標

在此實習課程，您將學會如何：

- CSS 編輯器中使用的新功能與增強功能
- HTML 編輯器中使用的新功能與增強功能
- 在 JavaScript 編輯器中使用的新功能與增強功能
- 設定及使用統合及縮製

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>必要條件

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)或上層 (讀取[附錄 A](#AppendixA)如需有關如何安裝指示)。
- [Windows PowerShell](https://support.microsoft.com/kb/968930/) （適用於安裝指令碼-已安裝 Windows 8 和 Windows Server 2008 R2）
- [Internet Explorer 10](https://windows.microsoft.com/internet-explorer/products/ie/home) -或 HTML5 相容的瀏覽器

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>練習

此實習課程包括下列練習：

1. [練習 1： 的新 CSS 編輯器](#Exercise1)
2. [練習 2： 什麼是新的 HTML 編輯器](#Exercise2)
3. [練習 3： 的新功能的 JavaScript 編輯器](#Exercise3)
4. [練習 4： 組合和縮製](#Exercise4)

完成本實驗室估計時間： **60 分鐘**。

<a id="Exercise1"></a>

<a id="Exercise_1_Whats_New_in_the_CSS_Editor"></a>
### <a name="exercise-1-whats-new-in-the-css-editor"></a>練習 1： 的新 CSS 編輯器

Web 開發人員應該要熟悉許多 CSS 編輯相關問題。 跨瀏覽器相容性的嚴重問題的 CSS 樣式的其中一個。 它通常會發生之後將樣式套用至您的網站，, 看起來不同，如果您發現您開啟另一個瀏覽器或裝置中。 因此，您可能會花相當長的時間進行修正這些 visual 的問題，若要了解，當您最後讓它在瀏覽器中運作，它會折在其他。

Visual Studio 現在包括功能，可協助開發人員存取、 工作及有效地組織 CSS 樣式表。 在這個練習中，您將會符合有效的組織和版本，新的功能以及跨瀏覽器相容性 CSS3 程式碼片段。

<a id="Ex1Task1"></a>

<a id="Task_1_-_New_Editor_Features"></a>
#### <a name="task-1---new-editor-features"></a>工作 1-新編輯器功能

在這項工作，您會發現新的 CSS 編輯器的功能。 這個新的編輯器，可協助您提升產能藉由運用新的智慧型縮排、 提升程式碼註解和增強的 IntelliSense 清單。

1. 啟動**Visual Studio**並開啟**WhatsNewASPNET.sln**方案位於**Source\WhatsNewASPNET**本實驗室的資料夾。
2. 在 [方案總管] 中，開啟**Site.css**檔案位於**樣式**資料夾。 請確定**文字編輯器**工具會顯示在工具列上。 若要這樣做，請選取**檢視** | **工具列**功能表選項和核取**文字編輯器**選項。 您會注意到，因為新的版本，**註解**按鈕 (![註解按鈕](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png)) 和**取消註解**按鈕 (![取消註解按鈕](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png)) 也可供 CSS 編輯器。

    ![啟用編輯器和 CSS 工具](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "編輯器和 CSS 工具，啟用")

    *啟用編輯器和 CSS 工具*
3. 捲動程式碼，然後選取任何的 CSS 類別定義。 按一下**註解**(![註解按鈕](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png)) 按鈕來註解選取的行。 然後，按一下 **取消註解**(![取消註解按鈕](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png)) 按鈕，以復原變更。
4. 按一下**摺疊**(![摺疊](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png)) 和**展開**(![展開](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png)) 按鈕位於左邊界的文字。 請注意，您現在可以隱藏您不使用具有更清楚地檢視的樣式。

    ![摺疊的 CSS 類別](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "摺疊的 CSS 類別")

    *摺疊的 CSS 類別*
5. 請確定已啟用智慧型縮排功能。 選取**工具** | **選項**功能表選項，然後選取**文字編輯器** | **CSS**  | **格式**螢幕的左窗格中的頁面。 請檢查**階層式縮排**選項。

    ![啟用階層式縮排](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "啟用階層式縮排")

    *啟用階層式縮排*
6. 找出主要類別定義 (.main)，並附加至 div 項目的樣式。 您會注意到程式碼將會自動對齊協助使用者快速尋找父類別。

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample1.css)]

    ![在 CSS 中的階層式對齊](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "在 CSS 中的階層式對齊方式")

    *在 CSS 中的階層式對齊方式*
7. 內部**.main div**類別中，尋找資料指標的結尾**框線： 0px;**按**Enter**顯示 IntelliSense 清單。 開始輸入**頂端**，並注意當您輸入要篩選清單的方式。 清單會顯示包含的項目**頂端**在這個字的任何部分 (在舊版的 Visual Studio 中，由項目篩選清單，*開始*具有該詞彙)。

    ![在 CSS 中的 IntelliSense 增強功能](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "在 CSS 中的 IntelliSense 增強功能")

    *在 CSS 中的 IntelliSense 增強功能*

<a id="Ex1Task2"></a>

<a id="Task_2_-_The_Color_Picker"></a>
#### <a name="task-2---the-color-picker"></a>工作 2-色彩選擇器

在這項工作，您會發現新的 CSS 色彩選擇器整合到 Visual Studio IntelliSense。

1. 在**Site.css，**找出標頭類別定義 (.header)，並將游標放在旁邊**背景色彩**屬性之間&quot;:&quot;和&quot; #&quot;該程式碼行上的字元**。**

    ![尋找游標](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "尋找資料指標")

    *尋找資料指標*
2. 刪除**冒號**（:） 和寫入一次，以顯示色彩選擇器。 請注意，第一個色彩，您會看到您的站台的最常見的色彩。 如果您按一下白色色彩，其 HTML 色彩代碼 (#fff) 將會取代目前的程式碼色彩樣式表中。

    ![色彩選擇器](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "色彩選擇器")

    *色彩選擇器*
3. 按**展開**(![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png) ) 上顯示的色彩漸層，，然後拖曳以選取不同的色彩漸層的資料指標與色彩選擇器 按鈕。 在這之後，按一下 **滴管**按鈕，然後從畫面中選取任何色彩。 請注意，背景色彩值以動態方式變更您移動游標時。

    ![漸層色彩選擇器](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "漸層色彩選擇器")

    *漸層色彩選擇器*
4. 在**不透明度**滑桿，將選取器移到中央的列，以減少不透明度。 請注意，背景色彩值現在會變更其小數位數為 RGBA。

    ![色彩選擇器的不透明度](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "色彩選擇器的不透明度")

    *色彩選擇器的不透明度*

    > [!NOTE]
    > CSS3 中增加 RGBA （紅、 綠、 藍色、 Alpha） 色彩定義可讓您定義色彩的不透明度值單一項目。 不同於**不透明度-**類似的 CSS 屬性**-** RGBA 色彩也會與最新的瀏覽器相容。

<a id="Ex1Task3"></a>

<a id="Task_3_-_CSS_Compatible_Code_Snippets"></a>
#### <a name="task-3---css-compatible-code-snippets"></a>工作 3-CSS 相容的程式碼片段

在這項工作，您將學習如何使用跨瀏覽器相容 CSS3 程式碼片段，才能在您的網站中實作某些功能。

1. 在**Site.css**檔案中，找出**標頭**CSS 類別定義 (.header)，並將下列游標 **/\*框線 radius\* /**要加入新的程式碼片段的預留位置。 按**Enter**顯示 IntelliSense 清單和型別**radius**來篩選清單。 選取**框線 radius**從清單中按兩下，與選項，然後再按**索引標籤**插入程式碼片段的索引鍵。 然後，輸入在像素並按下的 radius 大小**Enter**。 例如，輸入**15px**。

    新增的程式碼片段 CSS3 屬性會呈現大部分 HTML5 的相容性瀏覽器，包括 Mozilla 和 WebKit 瀏覽器中的圓角的邊框。

    ![使用框線 radius 片段](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "使用框線 radius 程式碼片段")

    *使用框線 radius 程式碼片段*
2. 適用於相同**框線**頁面樣式 (.page) 中的程式碼片段。

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample2.css)]
3. 按**F5**執行解決方案。 請注意，每個頁面現在具有圓框線。

    ![圓角](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "圓角")

    *圓的角*
4. 關閉瀏覽器，並返回 Visual Studio。
5. 開啟**Custom.css**檔案位於**樣式**資料夾，並將游標放**div.images u l l i m g**類別定義。
6. 按 enter 鍵以顯示 [IntelliSense] 清單中，型別**方塊陰影**按**索引標籤**鍵兩次以插入類別定義中的預設陰影程式碼片段。 若要設定陰影值**10px 10px 5px #888**。 然後，輸入**框線 radius**和插入程式碼片段。 型別**15px**設定 radius 大小及按**ENTER**。

    ![圓角以陰影](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "圓角以陰影")

    *有陰影的圓的角*

    > [!NOTE]
    > 目前，陰影屬性會插入對應的前置詞 moz、 webkit （o） 以支援 Mozilla 與 (Chrome 或 Safari，Konkeror) Webkit 瀏覽器。
7. 建立新的類別**div.images u l l i img:hover**下方**div.images u l l i m g**類別定義，並將放在括號內的資料指標**。**

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample3.css)]
8. 型別**轉換**按**索引標籤**兩次，以插入轉換程式碼片段的索引鍵。 然後，輸入**rotate(-15deg)**時暫留在映像變更旋轉角度值。

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample4.css)]
9. 按**F5**執行方案，然後瀏覽至 CSS3 頁面。 請注意，映像有圓角方塊陰影。 將滑鼠游標影像，並觀察他們旋轉。

    ![轉換程式碼片段旋轉影像](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "轉換程式碼片段旋轉影像")

    *轉換程式碼片段旋轉影像*

    > [!NOTE]
    > 如果您使用 Internet Explorer 10，且看不到陰影，請確定文件模式設定為 IE10 標準。 按**F12**開啟 Internet Explorer 開發人員工具，然後按一下 **文件模式**變更為 IE10 標準。

    ![about-us](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image21.png)

<a id="Exercise2"></a>

<a id="Exercise_2_Whats_New_in_the_HTML_Editor"></a>
### <a name="exercise-2-whats-new-in-the-html-editor"></a>練習 2： 什麼是新的 HTML 編輯器

Visual Studio 的改進的 HTML 編輯器。 部分增強功能包含在這個版本會在 HTML 文件、 HTML5 的程式碼片段、 HTML 開始和結束標記比對，以及 HTML 驗證智慧縮排。 在這個練習中，您會看到這些變更如何改善您的流暢性，網站標記中使用時。

CSS 編輯器，例如 HTML 編輯器也已獲得改善。 這些改良功能的大部分都是小型的讓 Web 開發人員的工作更容易。 需要更多的程式碼片段 html5，智慧型縮排，比對開始和結束標記時編輯和驗證目標 HTML 文件 DOCTYPE 是這些增強功能的一些。

<a id="Ex2Task1"></a>

<a id="Task_1_-_Improved_DOCTYPE_Validation"></a>
#### <a name="task-1---improved-doctype-validation"></a>工作 1-改善的 DOCTYPE 驗證

HTML 編輯器現在能夠檢查的頁面，DOCTYPE 即使定義可能主版頁面中。 根據您的頁面文件類型，HTML 編輯器將會驗證正確的規則集，並會篩選 IntelliSense 清單考慮 DOCTYPE 項目。

在這個工作中，您將變更頁面的 DOCTYPE，若要查看如何 HTML 編輯器行為會隨之變更。

1. 如果尚未開啟，請啟動**Visual Studio**並開啟**WhatsNewASPNET.sln**方案位於**Source\WhatsNewASPNET**本實驗室的資料夾。
2. 開啟**Site.Master**頁面。
3. 請注意，目標結構描述驗證工具列。 HTML 編輯器 （驗證、 IntelliSense、 等等） 的行為的方式會正確地變更以符合選取的文件類型。

    ![使用 HTML 原始檔編輯工具列中的 Doctype](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "使用 HTML 原始檔編輯工具列中的 Doctype")

    *使用 HTML 原始檔編輯工具列中的 Doctype*
4. Html 4.01 變更目標結構描述。

    ![在 HTML 原始檔編輯工具列中變更 Doctype](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "變更 HTML 原始檔編輯工具列中的 Doctype")

    *在 HTML 原始檔編輯工具列中變更文件類型*
5. 在游標放在**主體**項目，並開始輸入 HTML5 項目的名稱 (例如，**視訊**)。 請注意，項目不可以使用 IntelliSense 清單中。

    ![HTML5 未列出的項目](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "HTML5 未列出的項目")

    *HTML5 未列出的項目*
6. 復原到目標結構描述變更驗證工具列，挑選 DOCTYPE: XHTML5 從下拉式清單。

    ![使用 HTML 原始檔編輯工具列中的 Doctype](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "使用 HTML 原始檔編輯工具列中的 Doctype")

    *在 HTML 原始檔編輯工具列中重設文件類型*
7. 在游標放在**主體**項目和開始再次輸入 HTML5 項目 (例如 like**視訊**)。 請注意 HTML5 項目現在會出現在 IntelliSense 清單。

    ![HTML5 所列出的項目](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "HTML5 所列出的項目")

    *HTML5 所列出的項目*

<a id="Ex2Task2"></a>

<a id="Task_2_-_StartEnd_Tags_Automatic_Update"></a>
#### <a name="task-2---startend-tags-automatic-update"></a>工作 2-開始/結束標記的自動更新

Visual Studio 現在更新開啟或關閉您所要編輯成互相匹配項目的標記的 HTML。 編輯的 HTML 標記時，這項新功能會提高生產力。

1. 在**Default.aspx**頁面上，新增**H3**標題 （例如，Visual Studio 2012 岩石 ！） 的項目。


~~~
[!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample5.aspx)]
~~~
2. 變更**H3**標記和型別**H2**或**H1。**

    請注意，結束標記會自動更新。 您也可以修改若要查看的開始標記會據以更新過的結束標記。

    ![結束標記的自動更新](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "的結束標記的自動更新")

    *結束標記的自動更新*

<a id="Ex2Task3"></a>

<a id="Task_3_-_New_HTML5_Code_Snippets"></a>
#### <a name="task-3---new-html5-code-snippets"></a>工作 3-新的 HTML5 的程式碼片段

Visual Studio 現在包含數個 HTML5 的程式碼片段。 在這個工作中，您將使用這些程式碼片段部分。

1. 加入新的資料夾，名為**音訊**網站資料夾的根目錄。 開啟 Windows 檔案總管，並將複製到任何音訊檔**音訊**資料夾**WhatsNewASPNET.sln**方案。
2. 在**Default.aspx**頁面上，找出下 Web11 岩石游標!! 標頭。 型別**音訊**按 TAB 鍵。

    新的 HTML 編輯器包含 HTML5 內容的程式碼片段。 請記得使用正確的文件類型定義啟用 HTML5 的程式碼片段。

    ![將 HTML5 的程式碼片段](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "插入 HTML5 的程式碼片段")

    *將 HTML5 的程式碼片段*
3. 更新為指向現有的音訊檔案的音訊的來源。


~~~
[!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample6.aspx)]

> [!NOTE]
> You will need to add the audio file to the solution.
~~~
4. 按**F5**執行站台，並播放音訊。

    ![執行音訊控制項](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "執行音訊控制項")

    *執行音訊控制項*

    > [!NOTE]
    > 您也可以嘗試更多的程式碼片段包含在 Visual Studio 中，例如視訊、 圖等。
5. 現在，請嘗試以插入控制項中頁面的某些部分。 例如，嘗試插入**GridView**控制項，而不是輸入，但 **&lt;Gri，**開始輸入 **&lt;GV**。 請注意，IntelliSense 清單顯示**asp: GridView**控制項。

    HTML 編輯器中 IntelliSense 現在提供標題大小寫搜尋，以及部分相符 （擷取包含詞彙的所有項目）。

    ![插入 IntelliSense 清單的 GridView](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "插入 IntelliSense 清單的 GridView")

    *插入 IntelliSense 清單的 GridView*

    如果您輸入**&lt;方格**就會符合詞彙的所有項目，但 Visual Studio 將會建議**gridview**控制項：

    ![插入 IntelliSense 清單和部分比對的 GridView](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "插入 IntelliSense 清單和部分比對的 GridView")

    *插入 IntelliSense 清單和部分比對的 GridView*

<a id="Ex2Task4"></a>

<a id="Task_4_-_HTML_Editor_Smart_Tags"></a>
#### <a name="task-4---html-editor-smart-tags"></a>工作 4-HTML 編輯器智慧標籤

HTML 編輯器中的另一個改善的幅度智慧標籤功能。 智慧標籤，讓您輕鬆執行每個控制項為基礎的通用或重複的開發工作。 這項功能已可供使用在 HTML 設計工具中，但不是在 HTML 編輯器。

1. 開啟**Site.Master**並找出**asp： 功能表**項目。 將游標置於開始標記與小型的圖像顯示在下方的項目-按一下以開啟 [智慧工作提示] 功能表的注意事項。 請注意，您可以快速存取功能表控制項相關的一些工作。

    ![此功能表控制項的工作 智慧](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "智慧工作的功能表控制項")

    *功能表控制項的智慧工作*

<a id="Ex2Task5"></a>

<a id="Task_5_-_Smart_Indentation"></a>
#### <a name="task-5---smart-indentation"></a>工作 5-智慧型縮排

其中一個 HTML 中的最佳作法縮排巢狀項目，以便讓程式碼更容易閱讀。 在 Visual Studio 2012 中，您會注意到，編輯器會自動縮排項目在撰寫程式碼時。

> [!NOTE]
> 在舊版的 Visual Studio 中，可用的智慧縮排的 XML 編輯器中，但不是會在 HTML 編輯器中。


1. 請確定縮排設定 HTML 編輯器上，設定為智慧型縮排。 若要這樣做，請選取**工具 |選項**功能表選項，然後選取**文字編輯器 |HTML |索引標籤**螢幕的左窗格中的頁面。 選取智慧縮排選項。

    ![HTML 編輯器設定](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "HTML 編輯器設定")

    *HTML 編輯器設定*
2. 在**Default.aspx**頁面上，移除 audio 項目底下的所有內容。
3. 將游標放在開頭結尾**音訊**項目，然後按**ENTER**。

    請注意，資料指標的新位置會有額外的縮排層級。

    ![智慧型縮排在 HTML 編輯器](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "智慧型縮排在 HTML 編輯器")

    *智慧型縮排在 HTML 編輯器*
4. 還原已移除，或關閉的內容與音訊的標記**Default.aspx**而不儲存變更。

<a id="Ex2Task6"></a>

<a id="Task_6_-_Extract_to_User_Control"></a>
#### <a name="task-6---extract-to-user-control"></a>工作 6-擷取至使用者控制項

包含在 Visual Studio 中，例如擷取部份函式的程式碼的重構工具是很棒的功能，以便改善和重構現有的程式碼。 適用於 ASP.NET 網頁的對應項目會擷取至使用者控制項的 HTML 程式碼。 手動進行牽涉到幾個步驟，例如建立新的使用者控制項、 移至使用者控制項的程式碼區段，登錄使用者控制項標記前置詞和，最後，具現化的頁面上的使用者控制項。 現在，新*擷取至使用者控制項*工具會自動為您執行所有這些步驟。

在這項工作，您將使用新的擷取至使用者控制項內容的作業，從選取的程式碼中產生新的使用者控制項。

1. 在**Default.aspx**頁面上，選取**H2**和**音訊**項目。
2. 以滑鼠右鍵按一下，然後選取**擷取至使用者控制項**。

    ![擷取至使用者控制項功能表選項](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "擷取至使用者控制項功能表選項")

    *擷取至使用者控制項功能表選項*
3. 輸入新的使用者控制項的名稱。 比方說， **Jukebox.ascx**，然後按一下 **確定**。

    ![儲存擷取的使用者控制項](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "儲存擷取的使用者控制項")

    *儲存擷取的使用者控制項*
4. 請注意，選取的程式碼擷取至使用者控制項選取的程式碼的原始位置取代為新的使用者控制項的執行個體。

    ![頁面會自動更新，以使用新的使用者控制項](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "頁面自動更新，以使用新的使用者控制項")

    *頁面會自動更新，以使用新的使用者控制項*
5. 按**F5**執行頁面，並確認此控制項的作用。

<a id="Exercise3"></a>

<a id="Exercise_3_Whats_New_in_the_JavaScript_Editor"></a>
### <a name="exercise-3-whats-new-in-the-javascript-editor"></a>練習 3： 的新功能的 JavaScript 編輯器

撰寫或編輯 JavaScript 程式碼不容易的工作，特別是當您的應用程式開始變大，而且您會發現自己處理長檔案及數以百計的函式。 指令碼開發人員通常需要執行一些額外的工作來維護的程式碼可讀性和檔案之間瀏覽。 藉由加入像 jQuery JavaScript 程式庫中，指令碼瀏覽已成為一項挑戰本身由於程式碼長度。

Visual Studio 已更新，讓程式碼模式，存取與組織承諾的 JavaScript 編輯器。 在 C# 或 VB 編輯器中已存在的許多 Visual Studio 功能現在都實作中的 JavaScript 編輯器： 移至定義、 自動縮排、 文件集和您在撰寫時的驗證。 更新的 IntelliSense 清單將您手邊有 JavaScript 函式類別目錄。

在此練習中，您將學習的一些新功能和改良的 JavaScript 編輯器。 您會瀏覽範例檔案，並探索每個新的特性，可讓您的 JavaScript 程式設計 Visual Studio 2012 中更有效率。

<a id="Ex3Task1"></a>

<a id="Task_1_-_JavaScript_Editor_New_Features"></a>
#### <a name="task-1---javascript-editor-new-features"></a>工作 1-JavaScript 編輯器的新功能

此工作將介紹一些新的 JavaScript 編輯器功能，專注於組織您的程式碼，並讓較佳使用者體驗。

1. 如果尚未開啟，請啟動**Visual Studio**並開啟**WhatsNewASPNET.sln**方案位於**Source\WhatsNewASPNET**本實驗室的資料夾。
2. 按**F5**執行應用程式，然後按一下 瀏覽列中的 JavaScript 連結。 重新整理頁面數的時間和核取方式時，計數器會遞增。

    ![頁面計數器](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "頁面計數器")

    *頁面計數器*
3. 關閉瀏覽器並移回至 Visual Studio。
4. 開啟**JavaScript.aspx**頁面上，並找出**&lt;指令碼&gt;**區塊 （如下所示）。

    下列程式碼會使用 HTML5 本機儲存體來儲存*pageLoadCount*變數儲存目前的使用者已瀏覽頁面的次數。 本機儲存體是 HTML5 標準導入的用戶端索引鍵-值資料庫。 資料會儲存在本機電腦、 使用者的瀏覽器內。

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample7.html)]

    > [!NOTE]
    > 請確定文件類型設為 XHTML5 後再繼續進行下一個步驟。
5. 編輯程式碼，請注意，javascript IntelliSense 包含 HTML5 功能，例如本機儲存體，以及其內部的方法。

    ![在 JavaScript 中的 HTML5 JavaScript 功能](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "JavaScript HTML5 JavaScript 功能")

    *在 JavaScript 中的 HTML5 JavaScript 功能*
6. 按一下任何左括號 (**{**) 的指令碼從程式碼，並請注意括號會反白顯示。

    ![方括號會反白顯示](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "方括號會反白顯示")

    *方括號會反白顯示*
7. 請取消註解函式**testAutoAlign()** (選取的三行，而且您可以使用**CTRL** + **K**;**CTRL** + **U**) 並找出函式程式碼的內部資料指標。 按 enter 鍵新增第二行。 請注意，程式碼現在**對齊**和**自動縮排**。

    ![JavaScript 程式碼會自動對齊](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "JavaScript 程式碼會自動對齊")

    *JavaScript 程式碼會自動對齊*

<a id="Ex3Task2"></a>

<a id="Task_2_-_Validating_JavaScript"></a>
#### <a name="task-2---validating-javascript"></a>工作 2-JavaScript 驗證

在這項工作，您會發現新的 JavaScript 驗證 ECMAScript5 標準。 此功能將會協助您撰寫相容的 JavaScript 程式碼，同時防止指令碼的問題，站台部署之前。

> [!NOTE]
> Visual Studio 2010 實作 ECMAStript3 相容性，而 Visual Studio 2012 提供 ECMAScript5 相容性。


1. 開啟**ECMA5script5.js**位於**Scripts\custom**專案資料夾。 現在，您將測試 ECMAScript5 標準的驗證。

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample8.html)]

    您可以簽出&quot;**使用嚴格**&quot;在第一行中的檔案，可讓 ECMAScript5 方向**strict 模式**。 此模式包含在將釐清模稜兩可從過去的版本，並且物件屬性上加入一些新功能，例如 getter 和 setter、 JSON 和更完整的反映的程式庫支援的語言的子集。
2. 開啟**錯誤清單**如果尚未開啟 (**檢視**功能表 |**錯誤清單**)。 請注意**函式**宣告會加上底線。 這是因為在 ECMA5 標準函式不能放在語言結構。 在錯誤清單低於您會看到警告詳細資料。

    ![JavaScript 驗證錯誤訊息](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "JavaScript 驗證錯誤訊息")

    *JavaScript 驗證錯誤訊息*
3. 標記為註解**&quot;使用嚴格&quot;**方向，請注意，錯誤會消失，但警告保留。
4. 檔案的最後一行，在寫入任何字串 like **&quot;測試&quot;** （加上引號，以指出它是以字串形式）。 寫入期間來顯示 [IntelliSense] 清單中，選取字串旁的**修剪**選項。

    ECMAScript5 標準字串值和變數也會有定義，例如修剪、 大寫、 搜尋和取代字串方法。

    ![在 JavaScript 中的 IntelliSense 清單](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "在 JavaScript 中的 IntelliSense 清單")

    *在 JavaScript 中的 IntelliSense 清單*

<a id="Ex3Task3"></a>

<a id="Task_3_-_XML_Documentation_for_JavaScript"></a>
#### <a name="task-3---xml-documentation-for-javascript"></a>工作 3-for JavaScript XML 文件

在這個工作中，您將探索在 JavaScript 中的 XML 文件的 Visual Studio 功能。 您會看到 JavaScript IntelliSense 清單現在會顯示每個函式的 XML 文件。 此外，您會發現在 JavaScript 中的巡覽功能。

1. 開啟**XMLDoc.js**檔案位於**指令碼/自訂**專案資料夾。 此檔案包含 JavaScript 函式的每個 XML 文件。

    ![JavaScript XML 文件整合 intellisense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "JavaScript XML 文件整合 intellisense")

    *JavaScript XML 文件整合 intellisense*
2. 下面**新增**函式在**XMLDoc.js**檔案中，建立新的函式，名為**測試**。
3. 在**測試**函式，呼叫**乘**接收兩個參數的函式。 請注意顯示工具提示方塊**乘**函式文件。

    [!code-javascript[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample9.js)]

    ![對於 JavaScript 函式的 XML 文件](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "JavaScript 函式的 XML 文件")

    *對於 JavaScript 函式的 XML 文件*
4. 完成的函式呼叫的陳述式和類型*點*開啟 IntelliSense 清單上傳回的值。 請注意，Visual Studio 會偵測**傳回**文件，視為數字的值中的值。

    ![傳回類型的 XML 文件](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "傳回類型的 XML 文件")

    *傳回類型的 XML 文件*
5. 現在，插入對加入函式的呼叫。 請注意 JavaScript 編輯器現在支援函式多載。 當您撰寫函式名稱時，您可以選取任何可用的多載指定文件中。

    ![XML 文件的多載](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "多載的 XML 文件")

    *多載的 XML 文件*
6. 開啟**GotoDefinition.js**檔案，並尋找**$().html()**函式呼叫。 將游標置於**html**。
7. 按**F12**並巡覽至定義。 請注意，您現在可以存取，並瀏覽您的 JavaScript 程式碼，而不使用**尋找**工具。
8. 在底部的 程式碼檔案的簽章區塊之前的 jQuery 指令中找到資料指標。 按**F12**。 您會瀏覽至 jQuery 程式庫檔案。 請注意，您也可以使用的 jQuery 檔案之間瀏覽**F12**。

    ![巡覽至 jQuery 定義](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "巡覽至 jQuery 定義")

    *巡覽至 jQuery 定義*

> [!NOTE]
> 確定 GotoDefinition.js 儲存檔案之前有沒有語法錯誤。


<a id="Exercise4"></a>

<a id="Exercise_4_Bundling_and_Minification"></a>
### <a name="exercise-4-bundling-and-minification"></a>練習 4： 組合和縮製

多少次，您的網站，並包含一個以上的 JavaScript 或 CSS 檔案？ 這是很常見的案例，其中統合及縮製有助於減少檔案大小，並讓執行得更快的站台。 ASP.NET 4.5 的新功能將一組 JS 或 CSS 檔案封裝成單一元素，並縮短內容 （也就是移除不必要的空白，移除註解，減少識別項） 時，減少其大小。

統合及縮製 ASP.NET 4.5 中的被執行在執行階段，好讓處理程序可以識別出使用者代理程式 （例如 IE、 Mozilla 等等），並因此，將目標設為使用者瀏覽器 （比方說，移除項目是特定 Mozilla 改善壓縮當要求來自 IE）。

在此練習中，您將學習如何啟用和 ASP.NET 4.5 中使用不同類型的統合及縮製。

<a id="Ex4Task1"></a>

<a id="Task_1_-_Installing_the_Bundling_and_Minification_Package_from_NuGet"></a>
#### <a name="task-1---installing-the-bundling-and-minification-package-from-nuget"></a>工作 1-安裝組合和縮製 NuGet 封裝

1. 如果尚未開啟，請啟動**Visual Studio**並開啟**WhatsNewASPNET.sln**方案位於**Source\WhatsNewASPNET**本實驗室的資料夾。
2. 開啟 NuGet 封裝管理員主控台。 若要這樣做，請使用功能表**檢視** | **其他視窗** | **Package Manager Console**。

    ![開啟 封裝管理員 file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "開啟封裝管理員主控台")

    *開啟 封裝管理員主控台*
3. 在**Package Manager Console**類型**Install-package Microsoft.Web.Optimization**按**ENTER**。

<a id="Ex4Task2"></a>

<a id="Task_2_-_Default_Bundles"></a>
#### <a name="task-2---default-bundles"></a>工作 2-預設組合

使用統合及縮製的最簡單方式是啟用預設組合。 這個方法會使用慣例，讓您參考配套並縮短 JS 和 CSS 資料夾中的檔案版本。

在這項工作，您將學習如何啟用和參考配套並縮短 JS 和 CSS 檔案，並檢視所產生的輸出。

1. 如果尚未開啟，請啟動**Visual Studio**並開啟**WhatsNewASPNET.sln**方案位於**Source\WhatsNewASPNET**本實驗室的資料夾。
2. 在**方案總管 中**，依序展開**樣式**， **Scripts\custom**和**Scripts\bundle**資料夾。

    請注意，應用程式使用多個 CSS 與 JS 檔案。

    ![應用程式中的多個樣式表和 JavaScript 檔案](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "應用程式中的多個樣式表與 JavaScript 檔案")

    *應用程式中的多個樣式表和 JavaScript 檔案*
3. 開啟**Global.asax.cs**檔案。

    請注意，新**Microsoft.Web.Optimization**命名空間已標記為註解檔案的開頭。 取消註解 using 指示詞包含統合及縮製的功能。


~~~
[!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample10.cs)]
~~~
4. 找出**應用程式\_啟動**方法。

    在此方法中，請取消註解 EnableDefaultBundles 呼叫以下程式碼片段所示。 這可讓我們來參考搭售的 CSS 資料夾中的檔案集合使用的路徑，該資料夾，再加上&quot;CSS&quot;或&quot;JS&quot;後置詞。


~~~
[!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample11.cs)]
~~~
5. 開啟**Optimization.aspx**檔案，並尋找的內容控制項**HeadContent**。

    請注意 CSS 檔案和 JS 檔案有一個參考的標記。


~~~
[!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample12.aspx)]

> [!NOTE]
> This code is for demo purposes. Ideally, you will reference the bundles in the Site.Master file. In this sample code, you will find that some of the bundled files are also being referenced by the Site.Master file, making this last reference redundant.
~~~
6. 請注意，使用連結中的將慣例**href**屬性以取得所有 CSS 或 JS 檔案樣式和 Scripts\custom 資料夾分別。

    您可以使用路徑**指令碼/自訂/JS**配套並縮短內的所有 JS 檔案，如下所示**指令碼/自訂**資料夾。 這是預設組合的預設行為。


~~~
[!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample13.aspx)]
~~~
7. 開啟**Styles\Site.css**檔案。

    請注意，原始的 CSS 檔案包含縮排程式碼、 空格和增加檔案大小的註解。 （也 JavaScript 檔案包含空格和註解）。

    ![其中一個原始 CSS 檔案中的指令碼資料夾](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "其中一個原始 CSS 檔案中的指令碼資料夾")

    *其中一個原始 CSS 檔案中的指令碼資料夾*
8. 按**F5**執行應用程式，並瀏覽到**最佳化**頁面。
9. 按一下**CSS 配套**連結來下載並開啟檔案。

    簽出縮短配套的檔案。 您會發現的所有空格、 註解和縮排的字元都已經都移除，正在產生較小的檔案。

    ![配套 CSS 檔案](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "配套 CSS 檔案")

    *配套的 CSS 檔案*
10. 現在按一下**JS 組合**開啟配套的 JavaScript 檔案連結。 您可以放心地忽略警告 [總管]。 請注意下的 JavaScript 檔案**自訂**資料夾也會搭配並縮短。

    ![配套的 JavaScript 檔案](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "配套 JavaScript 檔案")

    *配套的 JavaScript 檔案*

    啟用壓縮 CSS 或 JS 檔案是較為複雜舊版 ASP.NET 中。 現在，如您所見，您只需要加入這一行中的*Global.asax*檔案以便結合在一起，並再參考配套的檔案從您的網站。

<a id="Ex4Task3"></a>

<a id="Task_3_-_Static_Bundles"></a>
#### <a name="task-3---static-bundles"></a>工作 3-靜態組合

靜態套件組合方法可讓您自訂的組合、 參考和縮製方法將使用的檔案集。

在這項工作，您將設定來定義一組特定的配套並縮短檔案的靜態套件組合。

1. 關閉瀏覽器。
2. 開啟**Global.asax.cs**檔案，並尋找**應用程式\_啟動**方法。
3. 取消註解的靜態配套程式碼，如下列程式碼所示。

    您正在定義會參考具有靜態配套&quot; **~/StaticBundle** &quot;虛擬路徑及使用**JsMinify**具有所有指定檔案的縮製的**AddFile**方法。 最後，您要在其中加入靜態套件組合， **BundleTable**及啟用它。

    請注意，檔案不在相同的位置。這是另一個預設結合在一起的優勢。


~~~
[!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample14.cs)]
~~~
4. 開啟**Optimization.aspx**檔案。

    請注意，連結至**靜態 JS 組合**正在使用您已經宣告 Global.asax.cs 檔案中設定靜態套件組合時的路徑： **/StaticBundle**。


~~~
[!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample15.aspx)]
~~~
5. 按**F5**執行應用程式，並瀏覽至**最佳化**頁面。
6. 按一下**靜態 JS 組合**連結來開啟檔案。

    請注意縮短配套 JavaScript 檔案已設定靜態組合檔案的路徑下的所有 JavaScript 檔案的輸出&quot;/StaticBundle&quot;。

    ![靜態的 JavaScript 檔案配套](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "靜態 JavaScript 檔案的套件組合")

    *靜態的 JavaScript 檔案的配套*
7. 關閉瀏覽器，並返回 Visual Studio。

<a id="Ex4Task4"></a>

<a id="Task_4_-_Dynamic_Folder_Bundles"></a>
#### <a name="task-4---dynamic-folder-bundles"></a>工作 4-動態資料夾組合

在這項工作，您將學習如何設定動態資料夾組合。 動態結合在一起的功能是可以包含靜態 JavaScript，以及其他檔案中會編譯成 JavaScript 的語言，因此，需要一些處理，結合在一起會在執行之前。

在此範例中，您將學習如何使用**DynamicFolderBundle**類別來建立動態套件組合 CofeeScript 中撰寫的檔案。 CofeeScript 是編譯成 JavaScript，並提供更簡單的語法撰寫 JavaScript 程式碼、 增強 JavaScript 的保持簡潔和可讀性的程式設計語言。

1. 開啟**Global.asax.cs**檔案，並尋找**應用程式\_啟動**方法。
2. 取消註解的動態套件組合程式碼，如下列程式碼所示。

    您會定義將會使用動態資料夾組合**CoffeeMinify**只會套用到具有檔案的自訂縮製處理器&quot; **.coffee** &quot;延伸模組 (CoffeeScript 檔案）。 請注意，您可以選取要像配套資料夾內檔案使用搜尋模式 '\*.coffee'。


~~~
[!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample16.cs)]
~~~
3. 開啟 NuGet 封裝管理員主控台。 若要這樣做，請使用功能表**檢視** | **其他視窗** | **Package Manager Console**。
4. 在**Package Manager Console**類型**Install-package CoffeeSharp**按**ENTER**。
5. 按一下**顯示所有檔案**按鈕**方案總管 中**視窗

    ![顯示所有檔案](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "都顯示所有檔案")

    *顯示所有檔案*
6. 以滑鼠右鍵按一下**CoffeeMinify.cs**檔案**方案總管 中**選取**加入至專案**

    ![在專案中包含 CoffeeMinify.cs 檔案](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "CoffeeMinify.cs 檔案包含在專案")

    *在專案中包含 CoffeeMinify.cs 檔案*
7. 開啟**CoffeeMinify.cs**檔案。

    此類別繼承自 JsMinify 来縮短 CoffeeScript 程式碼編譯所產生的 JavaScript 輸出中。 它會呼叫 CoffeeScript 編譯器在第一次，產生的 JavaScript 程式碼，然後它傳送它至 JsMinify.Process 方法，以縮短產生的程式碼。


~~~
[!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample17.cs)]
~~~
8. 開啟**Script1.coffee**和**Script2.coffee**檔案從**指令碼/配套**資料夾。

    這些檔案會包含 CoffeScript 程式碼結合在一起以 CoffeeMinify 類別的執行時編譯。

    為了簡單起見，提供的 CoffeeScript 檔案只包含 CoffeeScript 範例程式碼。 註解會排除 JsMinify 程序。

    ![CoffeeScript 檔案](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "CoffeeScript 檔案")

    *CoffeeScript 檔案*

    > [!NOTE]
    > [CofeeScript](https://github.com/jashkenas/coffeescript/)撰寫 JavaScript 程式碼、 增強 JavaScript 的保持簡潔和可讀性，以及加入其他功能，例如陣列理解與模式比對會提供更簡單的語法。
9. 開啟**Optimization.aspx**檔案，並尋找配套連結。

    請注意，連結至**動態 JS 組合**參考**指令碼/配套**資料夾使用**/咖啡**您設定動態資料夾組合的後置詞。


~~~
[!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample18.aspx)]
~~~
10. 按**F5**執行應用程式，並瀏覽至**最佳化**頁面。
11. 按一下**動態 JS 組合**連結來開啟產生的檔案。

    請注意，此組合中所包含的內容僅包含**.coffee**檔案。 您也可以查看 CoffeeScript 程式碼編譯至 JavaScript，並已移除的標記為註解的線條。

    ![動態 JS 檔案配套](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "動態 JS 檔案配套")

    *動態 JS 檔案配套*

> [!NOTE]
> 此外，您可以部署此應用程式以 Windows Azure Web Sites 下列[附錄 b： 發佈 ASP.NET MVC 4 應用程式使用 Web Deploy](#AppendixB)。


<a id="Summary"></a>
## <a name="summary"></a>總結

這個實驗室可協助您了解新 ASP.NET 和 Visual Studio 2012 中的 Web 開發是什麼，以及如何利用 Visual Studio 2012 中的各種增強功能。

藉由完成這個實際操作實驗室，您已學會如何在 Visual Studio 2012 編輯器中使用的新功能與增強功能，CSS、 JavaScript 和 HTML。 此外，您已學會如何 Visual Studio 2012 實作內建統合及縮製的。

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>附錄 a： 安裝 Visual Studio Express 2012 for Web

您可以安裝**Microsoft Visual Studio Express 2012 for Web**或另一個&quot;Express&quot;版本使用**[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. 下列指示將引導您逐步完成安裝所需*Visual studio Express 2012 for Web*使用*Microsoft Web Platform Installer*。

1. 移至[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169)。 或者，如果您已安裝 Web Platform Installer，您可以開啟它，並搜尋產品&quot; <em>Visual Studio Express 2012 for Web 與 Windows Azure SDK</em>&quot;。
2. 按一下**立即安裝**。 如果您不需要**Web Platform Installer**您會重新導向至下載並安裝第一次。
3. 一次**Web Platform Installer**開啟時，按一下 **安裝**，啟動安裝程式。

    ![安裝 Visual Studio Express](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "安裝 Visual Studio Express")

    *安裝 Visual Studio Express*
4. 閱讀所有產品的授權和詞彙，然後按一下**我接受**才能繼續。

    ![接受授權條款](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image60.png)

    *接受授權條款*
5. 等待直到完成下載和安裝程序。

    ![安裝進度](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image61.png)

    *安裝進度*
6. 當安裝完成時，按一下**完成**。

    ![安裝已完成](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image62.png)

    *安裝已完成*
7. 按一下**結束**關閉 Web Platform Installer。
8. 若要開啟 Visual Studio Express for Web，請移至**啟動**畫面上，並開始書寫&quot; **VS Express**&quot;，然後按一下  **VS Express for Web**並排顯示。

    ![VS Express for Web 方塊](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image63.png)

    *VS Express for Web 方塊*

* * *

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>附錄 b： 發佈 ASP.NET MVC 4 應用程式使用 Web Deploy

此附錄將說明如何從 Windows Azure 管理入口網站建立新的網站和發行您取得依照實驗室中，利用 Web Deploy 發行功能的 Windows Azure 所提供的應用程式。

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>工作 1-建立新的網站，從 Windows Azure 入口網站

1. 移至[Windows Azure 管理入口網站](https://manage.windowsazure.com/)並使用與您訂用帳戶相關聯的 Microsoft 認證登入。

    > [!NOTE]
    > 與 Windows Azure 中，您可以免費裝載 10 個 ASP.NET 網站並再擴充隨流量成長。 您可以註冊[這裡](http://aka.ms/aspnet-hol-azure)。

    ![登入 Windows Azure 入口網站](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "登入 Windows Azure 入口網站")

    *登入 Windows Azure 管理入口網站*
2. 按一下**新增**命令列上。

    ![建立新的網站](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "建立新的網站")

    *建立新的網站*
3. 按一下**計算** | **網站**。 然後選取**快速建立**選項。 新的網站上提供可用的 URL，然後按一下**建立網站**。

    > [!NOTE]
    > Windows Azure 網站是您可以控制和管理在雲端中執行的 web 應用程式的主機。 快速建立 選項可讓您部署已完成的 web 應用程式至 Windows Azure 網站從入口網站外部。 它不包含設定資料庫的步驟。

    ![建立新的網站使用 快速建立](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "建立新的網站使用 快速建立")

    *建立新的網站使用 快速建立*
4. 等候新**網站**建立。
5. 建立網站之後按一下底下的連結**URL**資料行。 請檢查新的網站運作。

    ![瀏覽至新的網站](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "瀏覽至新的網站")

    *瀏覽至新的網站*

    ![執行網站](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "執行的網站")

    *執行的網站*
6. 返回入口網站並按一下底下的網站名稱**名稱**資料行會顯示 [管理] 頁面。

    ![開啟網站管理頁面](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "開啟網站管理頁面")

    *開啟網站管理頁面*
7. 在**儀表板**頁面的 **快速概覽**區段中，按一下**下載發行設定檔**連結。

    > [!NOTE]
    > *發行設定檔*包含所有 web 應用程式發行至 Windows Azure 網站的每個已啟用的發行方法所需的資訊。 發行設定檔包含 Url、 使用者認證和資料庫連接，並對每個端點已啟用的發行集的方法進行驗證所需的字串。 **Microsoft WebMatrix 2**， **Microsoft Visual Studio Express for Web**和**Microsoft Visual Studio 2012**支援讀取發行設定檔自動將這些程式的組態web 應用程式發行至 Windows Azure 網站。

    ![下載網站發行設定檔](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "下載網站發行設定檔")

    *下載網站發行設定檔*
8. 下載發行設定檔至已知位置。 進一步在這個練習中，您會看到如何使用這個檔案從 Visual Studio 將 web 應用程式至 Windows Azure 網站發行。

    ![儲存發行設定檔](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "儲存發行設定檔")

    *儲存發行設定檔*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>工作 2-設定資料庫伺服器

如果您的應用程式會使用 SQL Server 資料庫，您必須建立 SQL Database 伺服器。 如果您想要部署簡單的應用程式不會使用 SQL Server，您可能會略過這項工作。

1. 您將需要 SQL Database 伺服器來儲存應用程式資料庫。 您可以從您的訂用帳戶，在 Windows Azure 管理入口網站中檢視 SQL Database 伺服器**Sql 資料庫** | **伺服器** | **伺服器儀表板**。 如果您沒有建立伺服器，您可以建立一個使用**新增**命令列上的按鈕。 請注意**伺服器名稱和 URL、 系統管理員身分登入名稱和密碼**，因為您將在下一個工作中使用它們。 並未建立資料庫，因為它會在後續階段中建立。

    ![SQL Database 伺服器儀表板](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "SQL Database 伺服器儀表板")

    *SQL Database 伺服器儀表板*
2. 下一項工作在您將測試資料庫連接，從 Visual Studio，因此您需要在伺服器的清單中包含您的本機 IP 位址**允許的 IP 位址**。 若要這樣做，請按一下**設定**，選取 IP 位址從**目前的用戶端 IP 位址**並將它貼上**起始 IP 位址**和**結束 IP 位址**文字方塊。 輸入規則的名稱，然後按一下![add-client-ip-address-ok-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png)  按鈕。

    ![加入用戶端 IP 位址](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image74.png)

    *加入用戶端 IP 位址*
3. 一次**用戶端 IP 位址**新增至允許的 IP 位址清單中，按一下**儲存**來確認變更。

    ![確認變更](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image75.png)

    *確認變更*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>工作 3-發行 ASP.NET MVC 4 應用程式使用 Web Deploy

1. 返回至 ASP.NET MVC 4 方案。 在**方案總管 中**，以滑鼠右鍵按一下網站專案，然後選取**發行**。

    ![發行應用程式](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "發行應用程式")

    *發行網站*
2. 匯入發行設定檔儲存在第一項工作中。

    ![匯入發行設定檔](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "匯入發行設定檔")

    *匯入發行設定檔*
3. 按一下**驗證連線**。 驗證完成後按一下**下一步**。

    > [!NOTE]
    > 一旦您看到 [驗證連線] 按鈕旁邊會出現綠色核取記號完成驗證。

    ![驗證連線](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "驗證連線")

    *驗證連接*
4. 在**設定**頁面的 **資料庫**區段中，按一下您的資料庫連接文字方塊旁邊的按鈕 (也就是**DefaultConnection**)。

    ![Web 部署設定](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "Web 部署設定")

    *Web 部署設定*
5. 設定資料庫連接，如下所示：

   - 在**伺服器名稱**您 SQL Database 伺服器 URL 使用下列方法類型*tcp:*前置詞。
   - 在**使用者名**輸入您的伺服器系統管理員身分登入名稱。
   - 在**密碼**輸入伺服器系統管理員身分登入密碼。
   - 輸入新的資料庫名稱，例如： *MVC4SampleDB*。

     ![設定目的地連接字串](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "設定目的地連接字串")

     *設定目的地連接字串*
6. 然後按一下 [確定]。  當系統提示您建立資料庫依序按一下**是**。

    ![建立資料庫](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "建立的資料庫字串")

    *建立資料庫*
7. 您將用來連接到在 Windows Azure SQL Database 的連接字串會顯示在預設連接文字方塊中。 然後按 [下一步] 。

    ![連接字串指向 SQL Database](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "連接字串指向 SQL 資料庫")

    *連接字串指向 SQL 資料庫*
8. 在**預覽**頁面上，按一下**發行**。

    ![Web 應用程式發行](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "發行 web 應用程式")

    *發行 web 應用程式*
9. 一旦在發佈程序完成時，預設瀏覽器會開啟已發行的網站。

    ![應用程式發行至 Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "應用程式發行至 Windows Azure")

    *應用程式發行至 Windows Azure*
