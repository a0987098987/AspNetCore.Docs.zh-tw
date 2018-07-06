---
uid: visual-studio/overview/2013/visual-studio-2013-web-tools
title: 實習實驗室： Visual Studio 2013 Web 工具 |Microsoft Docs
author: rick-anderson
description: Visual Studio 是一個絕佳的開發環境來。以.NET 為基礎的 Windows 和 web 專案。 它包含一個功能強大的文字編輯器，輕鬆地用來...
ms.author: aspnetcontent
ms.date: 07/16/2014
ms.assetid: 09e82351-816b-402d-acd1-0f9ac6901d16
msc.legacyurl: /visual-studio/overview/2013/visual-studio-2013-web-tools
msc.type: authoredcontent
ms.openlocfilehash: 9553d3129ff4c942eacbba628d1daaf6c4e33635
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807524"
---
<a name="hands-on-lab-visual-studio-2013-web-tools"></a>實習實驗室： Visual Studio 2013 Web 工具
====================
藉由[Web Camp 小組](https://twitter.com/webcamps)

[下載 Web 研討會訓練套件](http://aka.ms/webcamps-training-kit)

> Visual Studio 是一個絕佳的開發環境來。以.NET 為基礎的 Windows 和 web 專案。 它包含一個功能強大的文字編輯器，可輕鬆用來編輯沒有專案的獨立檔案。
> 
> 當您編輯每個檔案，visual Studio 就會維護完整的剖析樹狀結構。 這可讓 Visual Studio 以進行更快速且更美觀的開發經驗，提供無與倫比的自動完成和文件為基礎的動作。 這些功能會在 HTML 和 CSS 文件中的功能十分強大。
> 
> 所有此能力也可供擴充功能，輕鬆擴充以符合您需求的編輯器使用強大的新功能。 Web Essentials 是 （大多數） web 相關的增強功能至 Visual Studio 的集合。 它包含許多新的 IntelliSense 完成 （特別是針對 CSS)、 新的瀏覽器連結功能、 自動 JSHint 適用於 JavaScript 檔案、 HTML 和 CSS 和其他許多功能，才能使用新式網頁程式開發的新警告。
> 
> 所有的範例程式碼和程式碼片段會包含在 Web 研討會訓練套件，可在[ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit)。


<a id="Overview"></a>
## <a name="overview"></a>總覽

<a id="Objectives"></a>
### <a name="objectives"></a>目標

在這個實際操作實驗室中，您將了解如何：

- 使用新的 HTML 編輯器功能，例如豐富 HTML5 的程式碼片段和 Zen 撰寫程式碼納入 Web Essentials
- 使用新的 CSS 編輯器功能，包含 Web Essentials 中，色彩選擇器等瀏覽器矩陣工具提示
- 使用新的 JavaScript 編輯器功能，包括 Web Essentials，例如擷取至檔案和 IntelliSense 中所有 HTML 項目
- 您的瀏覽器與 Visual Studio 中使用瀏覽器連結之間交換資料

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>必要條件

需要下列項目才能完成這個實際操作實驗室：

- [Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/)或更新版本
- [Web Essentials 2013](http://vswebessentials.com/)
- [Google Chrome](https://www.google.com/chrome/)

<a id="Setup"></a>
### <a name="setup"></a>安裝程式

若要在這個實際操作實驗室中執行的練習，您必須先設定您的環境。

1. 開啟 Windows 檔案總管視窗並瀏覽至實驗室**來源**資料夾。
2. 以滑鼠右鍵按一下**Setup.cmd** ，然後選取**系統管理員身分執行**來啟動安裝程序，將會設定您的環境並安裝這個實驗室的 Visual Studio 程式碼片段。
3. 如果會顯示 [使用者帳戶控制] 對話方塊中，確認要繼續進行的動作。

> [!NOTE]
> 請確定您執行安裝程式之前檢查這個實驗室的所有相依性。


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>使用程式碼片段

整個實驗室文件中，您將會指示要插入程式碼區塊。 為了方便起見，大部分的這段程式碼提供 Visual Studio 程式碼片段，您可以從 Visual Studio 2013，若要避免以手動方式新增內存取。

> [!NOTE]
> 每個練習會伴隨起始方案，位於**開始**練習，可讓您依照每個練習，獨立於其他的資料夾。 請留意練習期間新增的程式碼片段缺少這些啟動解決方案，並可能無法運作，直到您已完成練習。 在練習的原始程式碼，您也可以找到**結束**資料夾包含 Visual Studio 方案，以程式碼所產生的相對應的練習中的步驟。 如果您需要其他說明，當您完成這個實際操作實驗室，您可以使用這些解決方案與指引。


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>練習

這個實際操作實驗室還包含下列練習：

1. [使用瀏覽器連結和 Web Essentials](#Exercise1)
2. [利用程式碼片段和 IntelliSense](#Exercise2)

> [!NOTE]
> 當您第一次啟動 Visual Studio 時，您必須選取其中一個預先定義的設定集合。 每個預先定義的集合以符合特定的開發樣式設計，並決定視窗版面配置、 編輯器的行為、 IntelliSense 程式碼片段和對話方塊選項。 在這個實驗室中的程序說明完成指定的工作，在 Visual Studio 中使用時所需的動作**一般開發設定**集合。 如果您選擇不同的設定集合，您的開發環境，可能會有在步驟中，您應該考慮到的差異。


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-browser-link-and-web-essentials"></a>練習 1： 使用瀏覽器連結和 Web Essentials

**Web Essentials**是 Visual Studio 擴充功能，新增各種現代網頁程式開發，大部分是專注於讓 web 開發經驗更快速且更美觀的實用功能。 您可以從 Visual Studio 中的延伸模組資源庫安裝 Web Essentials。

**瀏覽器連結**是包含在 Visual Studio IDE 和任何開啟的瀏覽器，您的 web 應用程式和 Visual Studio 之間交換資料之間提供通道的 Visual Studio 2013 的新功能。 Web Essentials 擴充瀏覽器連結，這些工具，可操作的 DOM 物件模型和您的網頁，直接從瀏覽器的 CSS 樣式。

在這個練習中，您將探索所支援之功能的一些**Web Essentials**並**瀏覽器連結**增強簡單測驗頁面。

<a id="Ex1Task1"></a>
#### <a name="task-1---running-the-project-in-multiple-browsers"></a>工作 1-在多個瀏覽器中執行專案

在這個工作中，您將設定 web 應用程式在多個瀏覽器中執行的這很適合用於測試的跨瀏覽器。

1. 開啟**Microsoft Visual Studio**。
2. 在 **檔案**功能表上，選取**開啟 |專案/方案...** 並瀏覽至**Ex1 WorkingwithBrowserLinkandWebEssentials\Begin**中**來源**實驗室 (C:\WebCampsTK\HOL\VSWebTooling\Source) 的資料夾。 選取  **Begin.sln**然後按一下**開啟**。
3. 在 Visual Studio 工具列中，展開 [瀏覽器] 功能表，然後選取**瀏覽方式...**.

    ![瀏覽方式 功能表選項](visual-studio-2013-web-tools/_static/image1.png "...功能表瀏覽器中瀏覽")

    *瀏覽方式 功能表選項*
4. 在**瀏覽** 對話方塊中，選取**Google Chrome**並**Internet Explorer**按住**CTRL**鍵，然後按一下**設為預設值**。

    ![瀏覽對話方塊](visual-studio-2013-web-tools/_static/image2.png "瀏覽對話方塊")

    *選取多個預設瀏覽器*
5. Internet Explorer 和 Google Chrome 現在應該做為預設瀏覽器會出現。 按一下 **取消**以關閉對話方塊。

    ![Google Chrome 和 Internet Explorer 作為預設瀏覽器](visual-studio-2013-web-tools/_static/image3.png "Google Chrome 和 Internet Explorer 的預設瀏覽器")

    *Google Chrome 和 Internet Explorer 作為預設瀏覽器*

    > [!NOTE]
    > 設定預設瀏覽器之後**多個瀏覽器**瀏覽器功能表中選取選項。
    > 
    > ![多個瀏覽器](visual-studio-2013-web-tools/_static/image4.png "多個瀏覽器")
6. 按下**CTRL** + **F5**執行應用程式，但不偵錯。
7. 當這兩個瀏覽器視窗開啟時，放置在彼此上方其中一個同時查看這兩個瀏覽器上的更新。 在瀏覽器應該會顯示以淺藍色矩形的網頁。

    ![將放在彼此上方的一個瀏覽器](visual-studio-2013-web-tools/_static/image5.png "放置在彼此上方的一個瀏覽器")

    *將放在彼此上方的一個瀏覽器*
8. 請勿關閉瀏覽器。 您將在下一個工作中使用它們。

<a id="Ex1Task2"></a>
#### <a name="task-2---using-zen-coding-to-create-html-elements"></a>工作 2-使用 Zen 撰寫程式碼來建立 HTML 元素

**撰寫程式碼 Zen**高速的 HTML、 XML、 XSL （或任何其他的結構化的程式碼格式） 的外掛程式程式碼撰寫和編輯 」 是一個編輯器。 此外掛程式的核心是一個功能強大的縮寫引擎可讓您展開-類似於 CSS 選取器-的運算式，插入 HTML 程式碼。 Zen 撰寫程式碼是一種撰寫使用 CSS 的 HTML 樣式選取器語法的快速方式。

在此練習中，您將使用 Zen 撰寫程式碼所提供的功能 Web Essentials 所產生的 HTML 按鈕表示問題的選項。

1. 切換回到 Visual Studio。
2. 開啟**Index.cshtml**檔案位於**檢視** | **首頁**資料夾。
3. 取代**&lt;！-TODO： 加入選項-&gt;** 註解與下列程式碼，然後按 **索引標籤**。

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample1.css)]
4. 程式碼應該擴展成 HTML。

    ![展開 HTML](visual-studio-2013-web-tools/_static/image6.png "展開 HTML")

    *擴充的 HTML*

    > [!NOTE]
    > 若要深入了解 Zen 撰寫程式碼的語法，請參閱下列[文章](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/)。
5. 按一下 **重新整理連結的瀏覽器**按鈕以更新這兩個瀏覽器。

    ![重新整理連結的瀏覽器](visual-studio-2013-web-tools/_static/image7.png "重新整理連結的瀏覽器")

    *重新整理連結的瀏覽器*

    ![Internet Explorer-更新包含四個按鈕的頁面](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer-更新包含四個按鈕的頁面")

    *Internet Explorer-更新包含四個按鈕的頁面*

    ![Google Chrome-更新包含四個按鈕的頁面](visual-studio-2013-web-tools/_static/image9.png "Google Chrome-更新包含四個按鈕的頁面")

    *Google Chrome-更新包含四個按鈕的頁面*
6. 切換回到 Visual Studio。
7. 您的按鈕新增至頁面，但您仍要新增範本問題。 若要這樣做，您將使用的新功能中呼叫的 Web Essentials **Lorem Ipsum 產生器**。 找出**div**項目**類別**屬性**前端**。
8. 將下列程式碼新增的第一個子元素為**div**，然後按 **索引標籤**。

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample2.css)]
9. 程式碼應該擴展成 HTML。

    ![Lorem Ipsum 自動產生](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum 自動產生")

    *Lorem Ipsum 自動產生*

    > [!NOTE]
    > Zen 撰寫程式碼的一部分，您現在可以直接在 HTML 編輯器中，產生 Lorem Ipsum 的程式碼。 只要輸入**lorem**並點擊 **索引標籤**及 30 文字 Lorem Ipsum 會插入文字。 例如， *lorem10*插入 10 Lorem Ipsum 字。
10. 您也會呼叫的 Web Essentials 中使用另一項新功能頂端的問題新增標誌**Lorem 像素產生器**。 將下列程式碼新增的第一個子元素為**div**具有項目**容器**為**類別**，再按 **索引標籤**。

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample3.css)]
11. 程式碼應該展開為 HTML。

    ![Lorem 像素自動產生](visual-studio-2013-web-tools/_static/image11.png "Lorem 像素自動產生")

    *Lorem 像素自動產生*

    > [!NOTE]
    > Zen 撰寫程式碼的一部分，您也可以直接在 HTML 編輯器中，產生 Lorem 像素的程式碼。 只要輸入**pix-200 x 200-動物**並點擊 **索引標籤**和**img**要插入的代表動物 200 x 200 映像的標籤。 如需詳細資訊，請參閱[Lorem 像素](http://www.lorempixel.com)。
12. 按一下 **重新整理連結的瀏覽器**按鈕以更新這兩個瀏覽器。

    ![Internet Explorer-自動產生的影像和文字](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer-自動產生的影像和文字")

    *Internet Explorer-自動產生的影像和文字*

    ![Google Chrome-自動產生的影像和文字](visual-studio-2013-web-tools/_static/image13.png "Google Chrome-自動產生的影像和文字")

    *Google Chrome-自動產生的影像和文字*

    > [!NOTE]
    > 新增程式碼片段時，會隨機選取映像，因為瀏覽器中所顯示的影像可能會不同。
13. 請勿關閉瀏覽器。 您將在下一個工作中使用它們。

<a id="Ex1Task3"></a>
#### <a name="task-3---updating-a-style-property"></a>工作 3-更新的樣式屬性

在這個工作中，您將使用的瀏覽器連結**檢查模式**功能偵測到特定的 DOM 項目，產生的位置的確切位置，然後更新使用色彩選擇器提供的 Web 該元素的色彩屬性基本功能。

1. 在 Internet Explorer 瀏覽器中，按下**CTRL** + **ALT** + **我**啟用檢查的模式。
2. 將指標移淺藍色框線，然後按一下。

    ![將指標移淺藍色框線](visual-studio-2013-web-tools/_static/image14.png "將指標移淺藍色框線")

    *將指標移淺藍色框線*
3. 切換回到 Visual Studio。 請注意您在瀏覽器中選取 HTML 項目如何也在 Visual Studio HTML 編輯器中選取。

    ![Visual Studio HTML 編輯器中選取的 HTML 項目的](visual-studio-2013-web-tools/_static/image15.png "Visual Studio HTML 編輯器中選取的 HTML 項目")

    *在 Visual Studio HTML 編輯器中選取的 HTML 項目*
4. 您現在將會更新**front**以變更選取項目的樣式的 CSS 類別。 若要這樣做，請按**CTRL** + **，** 開啟**巡覽至**搜尋方塊。 型別**site.css**然後按**ENTER**開啟檔案。

    ![開啟檔案 Site.css](visual-studio-2013-web-tools/_static/image16.png "開啟檔案 Site.css")

    *開啟檔案 Site.css*
5. 按下**CTRL** + **F**和型別 **.flip containter.front**尋找 CSS 選取器。
6. 按一下淺藍色方塊中的類別，以開啟色彩選擇器的框線屬性。

    ![開啟色彩選擇器](visual-studio-2013-web-tools/_static/image17.png "開啟色彩選擇器")

    *開啟色彩選擇器*
7. 展開色彩選擇器依序按一下 > 形箭號按鈕，然後選取新的色彩。

    ![展開色彩選擇器](visual-studio-2013-web-tools/_static/image18.png "展開色彩選擇器")

    *展開色彩選擇器*
8. 按下**CTRL** + **ALT** + **ENTER**重新整理連結的瀏覽器。
9. 切換至 Internet Explorer，並注意如何變更框線的色彩。

    ![Internet Explorer-更新的框線色彩](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer-更新的框線色彩")

    *Internet Explorer-更新的框線色彩*
10. 切換至 Google Chrome，請注意如何變更框線的色彩。

    ![Google Chrome-更新的框線色彩](visual-studio-2013-web-tools/_static/image20.png "Google Chrome-更新的框線色彩")

    *Google Chrome-更新的框線色彩*
11. 切換回到 Visual Studio。
12. 移至結尾**Site.css**檔案，然後按**CTRL** + **F**找出 **.btn**選取器。
13. 請注意， **-webkit 框線半徑**屬性會以綠色顯示加上底線。

    ![-webkit 框線半徑屬性 btn selector](visual-studio-2013-web-tools/_static/image21.png "btn 選擇器-webkit 框線半徑屬性")

    *-webkit 框線半徑 btn 選取器屬性*
14. 在號 **-webkit 框線半徑**屬性。 藍線應該會出現在屬性的第一個單字的第一個字母。 這是**智慧標籤**。
15. 按下**CTRL** + **。** 若要開啟 [建議] 功能表，然後按一下**新增遺失的標準屬性 （框線半徑）**。

    ![新增遺漏的標準屬性的建議](visual-studio-2013-web-tools/_static/image22.png "新增遺失的標準屬性的建議")

    *新增遺漏的標準屬性的建議*
16. **框線半徑**屬性會自動新增至 CSS 規則。

    ![缺少標準的屬性加入](visual-studio-2013-web-tools/_static/image23.png "遺漏新增的標準屬性")

    *遺漏新增的標準屬性*
17. 將指標移**框線半徑**屬性來顯示**瀏覽器矩陣工具提示**。 **瀏覽器矩陣工具提示**在每個瀏覽器中顯示屬性的可用性。

    ![瀏覽器矩陣工具提示](visual-studio-2013-web-tools/_static/image24.png "瀏覽器矩陣工具提示")

    *瀏覽器矩陣工具提示*
18. 請注意，值**框線半徑**屬性是仍然加上底線。 您可以將指標移到看到警告訊息的值。

    ![框線半徑屬性值的警告](visual-studio-2013-web-tools/_static/image25.png "框線半徑屬性值的警告")

    *框線半徑屬性值的警告*
19. 移除單位**框線半徑**工具提示所建議的屬性值。
20. 作為**框線半徑**是 「 標準 」 屬性來定義如何圓角的框線是邊角，您可以移除 **-webkit 框線半徑**屬性和 CSS 規則的值。
21. 在號**自動換行**屬性和智慧標籤也會出現下方的注意。
22. 開啟功能表，然後按一下**加入遺漏的廠商細節**。

    ![新增遺漏的廠商細節建議](visual-studio-2013-web-tools/_static/image26.png "新增遺漏的廠商細節建議")

    *新增遺漏的廠商細節建議*
23. **Ms 自動換行**屬性會自動新增至 CSS 規則。

    ![廠商特定屬性加入](visual-studio-2013-web-tools/_static/image27.png "加入廠商特定屬性")

    *加入廠商特定屬性*

<a id="Ex1Task4"></a>
#### <a name="task-4---updating-the-html-code-from-the-browser"></a>工作 4-更新 HTML 程式碼，從瀏覽器

在這個工作中，您將使用瀏覽器連結**設計模式**編輯瀏覽器的 DOM 物件，並將變更傳輸到 HTML 原始程式檔，在 Visual Studio 中的功能。

1. 在 Google Chrome 中，按下**CTRL** + **ALT** + **D**啟用設計模式。
2. 將指標移**Lorem Ipsum dolor sit amet**加上標籤，然後按一下。

    ![編輯問題](visual-studio-2013-web-tools/_static/image28.png "編輯問題")

    *編輯問題*
3. 資料指標應該會出現。 取代原始的文字，與*什麼它實際看起來時我撰寫更長的問題？*，然後按**ESC**若要結束設計模式。

    ![編輯的問題](visual-studio-2013-web-tools/_static/image29.png "編輯的問題")

    *編輯的問題*
4. 切換回到 Visual Studio 並開啟**Index.cshtml**，如果尚未開啟。 請注意，內部文字**&lt;p&gt;** 已更新項目。

    ![已更新 HTML 網頁中問題的答案](visual-studio-2013-web-tools/_static/image30.png "HTML 網頁中的更新問題")

    *HTML 網頁中的更新的問題*

<a id="Ex1Task5"></a>
#### <a name="task-5---reviewing-seo-related-warnings"></a>工作 5-檢閱 SEO 相關警告

**搜尋引擎最佳化**(SEO) 是網站順位較高可確保在搜尋引擎結果清單上的程序。 站台的排名越高，越一致的方式，它會列，多個訪客的站台將會收到來自該搜尋引擎。 Web Essentials 納入分析工具，來檢查 HTML，報告的問題找到，且提供協助來加以修正。

1. 移至**檢視**功能表，然後按一下**錯誤清單**以開啟**錯誤清單**視窗。

    ![錯誤清單檢視中功能表](visual-studio-2013-web-tools/_static/image31.png "檢視功能表中的錯誤清單")

    *錯誤清單檢視功能表*
2. 請注意，通知的 SEO 警告**&lt;meta&gt;** 標記的頁面描述遺漏。 按兩下 SEO 警告項目，若要修正此問題。

    ![錯誤清單 視窗](visual-studio-2013-web-tools/_static/image32.png "錯誤清單 視窗")

    *錯誤清單視窗*
3. 在**Web Essentials**對話方塊中，按一下**是**插入描述&lt;中繼&gt;標記。

    ![Web Essentials 對話方塊](visual-studio-2013-web-tools/_static/image33.png "Web Essentials 對話方塊")

    *Web Essentials 對話方塊*
4. 編輯器 **\_Layout.cshtml**隨即開啟並**&lt;中繼&gt;** 標記會自動新增至**標頭**區段HTML 檔。

    ![自動配置 （_l） 頁面中加入的中繼標籤](visual-studio-2013-web-tools/_static/image34.png "自動配置 （_l） 頁面中加入的中繼標籤")

    *自動加入至中繼標籤\_版面配置頁*
5. 值變更**內容**屬性設定為*GeekQuiz*並儲存檔案。

<a id="Exercise2"></a>
### <a name="exercise-2-taking-advantage-of-code-snippets-and-intellisense"></a>練習 2： 利用程式碼片段和 IntelliSense

透過 Web Essentials，HTML 編輯器已擴充加上額外的功能。 在此練習中，您會看到一些新功能，開發 web 應用程式時很有幫助。

<a id="Ex2Task1"></a>
#### <a name="task-1---using-intellisense-in-html-documents"></a>工作 1-使用 HTML 文件中的 IntelliSense

您會看到這項工作中的第一個新功能稱為**動態 IntelliSense**。 動態 IntelliSense 讀取其他的標記和屬性對推斷可能會使用的識別碼。

在這個工作中，您將建立新的 HTML 表單項目包含標籤和輸入的欄位。 然後您會新增**針對**屬性至繫結至輸入、 標籤，您會看到的範圍中的輸入識別碼為基礎的 IntelliSense 建議。

1. 開啟**Visual Studio Express 2013 for Web**並**Begin.sln**解決方案位於**來源/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/開始**資料夾。 或者，您可以繼續使用解決方案您在前一個練習中取得。
2. 在**方案總管中**，開啟**Index.cshtml**檔案位於**檢視** | **首頁**資料夾。
3. 將下列表單內加入 **&lt; 區段&gt;** 項目。

    (程式碼片段- *VisualStudio2013WebTooling* - *Ex2* - *表單*)

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample4.html)]
4. 輸入的標記應該加上有一些描述之欄位的標籤。 新增下列標籤輸入標記之前。

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample5.html)]
5. **For**屬性**&lt;標籤&gt;** 指定的標籤的表單項目繫結至。 屬性的值應該等於相關項目的 id。 新增**for**屬性設定為**&lt;標籤&gt;** 項目。 下圖所示&quot;名稱&quot;值出現在 [IntelliSense] 方塊中，根據在相同範圍內項目的 id (封閉式**&lt;表單&gt;**)。

    ![顯示在 IntelliSense 中的識別碼](visual-studio-2013-web-tools/_static/image35.png "在 IntelliSense 中顯示的識別碼")

    *顯示在 IntelliSense 中的識別碼*
6. 刪除最近新增**&lt;表單&gt;** 項目和其內容。

<a id="Ex2Task2"></a>
#### <a name="task-2---using-html-code-snippets"></a>工作 2-使用 HTML 程式碼片段

HTML5 引進了超過 25 個新的語意標記。 已經有 visual Studio 的 IntelliSense 支援，這些標記，但 Visual Studio 2013 可以更快速且輕鬆地撰寫藉由新增新的程式碼片段的標記。 雖然這些標記不是複雜的但是需要付出一些小微妙之處，例如新增的正確轉碼器後援*音訊*標記。 在這個工作中，您會看到音訊的標記的 HTML 程式碼片段。

1. 在  **Index.cshtml**檔案中，輸入 **&lt;aud**內**&lt;區段&gt;** 項目，如下列圖所示。

    ![音訊的項目插入](visual-studio-2013-web-tools/_static/image36.png "插入音訊元素")

    *插入音訊元素*
2. 按**索引標籤**兩次注意如何在頁面上加入下列程式碼和資料指標置於**src**第一個來源的屬性。

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample6.html)]

    > [!NOTE]
    > 按下**索引標籤**金鑰兩次，在程式碼片段插入。 音訊的程式碼片段示範的標準用法*音訊*標記的改進支援的兩個原始程式檔。
3. 刪除第二行，並使用下列連結，以 WebCampsTV Katana 顯示更新的第一行的來源： [ http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3 ](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3)。 產生的程式碼如下所示。

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample7.html)]

    > [!NOTE]
    > 原始程式檔*KatanaProject.mp3*使用做為範例。 如果您想，您可以使用另一個來源。
4. 按下**CTRL** + **S**來儲存檔案。
5. 按下**CTRL** + **F5**啟動應用程式。
6. 請注意音訊播放器已新增至應用程式。

    ![在 Internet Explorer 中的音訊播放器](visual-studio-2013-web-tools/_static/image37.png "Internet Explorer 中的音訊播放程式")

    *在 Internet Explorer 中的音訊播放器*

    ![在 Google Chrome 中的音訊播放器](visual-studio-2013-web-tools/_static/image38.png "Google Chrome 中的音訊播放程式")

    *在 Google Chrome 中的音訊播放器*
7. 請勿關閉瀏覽器。 您將在下一個工作中使用它們。

<a id="Ex2Task3"></a>
#### <a name="task-3---using-intellisense-in-javascript-documents"></a>工作 3-使用 JavaScript 的文件中的 IntelliSense

有了 Web Essentials 2013，樣式表和 HTML 頁面產生識別碼和類別名稱的清單。 在這個工作中，您將了解這些清單如何改善 Web Essentials 2013 中的 JavaScript IntelliSense 支援。

1. 在  **Index.cshtml**檔案中，新增下列程式碼，以定義**指令碼**JavaScript 程式碼標記。

    [!code-cshtml[Main](visual-studio-2013-web-tools/samples/sample8.cshtml)]
2. 新增下列程式碼貼入**指令碼**定義好的回呼函式的標記。

    (程式碼片段- *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample9.js)]
3. 在號**指令碼**標記，然後按**CTRL** + **。** 若要開啟 [建議] 功能表。
4. 按一下 **擷取至檔案**。

    ![JavaScript 擷取至檔案的建議](visual-studio-2013-web-tools/_static/image39.png "JavaScript 擷取至檔案的建議")

    *JavaScript 擷取至檔案的建議*
5. 在 **另存新檔**視窗中，選取**指令碼**資料夾中，將檔案命名**init.js** ，按一下 **儲存**。

    ![另存新檔視窗](visual-studio-2013-web-tools/_static/image40.png "另存新檔視窗")

    *另存新檔視窗*

    > [!NOTE]
    > **Init.js**建立檔案和指令碼的內容移到檔案。
    > 
    > ![Init.js 檔案所包含的內容以建立](visual-studio-2013-web-tools/_static/image41.png "Init.js 檔案所包含的內容建立")
    > 
    > *使用包含的內容建立的 Init.js 檔案*
6. 開啟**Index.cshtml**檔，並檢查指令碼標記，已取代的參考**init.js**檔案。

    ![Init.js html 參考](visual-studio-2013-web-tools/_static/image42.png "Init.js html 參考")

    *Init.js html 參考*
7. 移至**方案總管中**並注意**init.js**檔案會自動包含在方案中。

    ![包含在方案中的 Init.js 檔案](visual-studio-2013-web-tools/_static/image43.png "Init.js 檔案包含在方案中")

    *在方案中所包含的 Init.js 檔*
8. 切換回**init.js**檔案，以更新**準備**函式回呼。
9. 傳遞至函式回呼定義內*準備*，新增下列程式碼，以取得特定的類別屬性的所有項目。

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample10.js)]
10. 按下**CTRL** + **空間**內的引號之間**getElementsByClassName**函式呼叫。

    ![顯示 getElementsByClassName 函式的 IntelliSense](visual-studio-2013-web-tools/_static/image44.png "顯示 getElementsByClassName 函式的 IntelliSense")

    *顯示 IntelliSense getElementsByClassName 函式*

    > [!NOTE]
    > 請注意，IntelliSense 會顯示專案樣式表中定義的類別。
11. 取代為下列程式碼，您已建立的那一行。

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample11.js)]
12. 將游標放在後的**au**在引號內**getElementsByTagName**函式，然後按**CTRL** + **空間**.

    ![顯示 IntelliSense getElementByTagName 方法](visual-studio-2013-web-tools/_static/image45.png "顯示 IntelliSense getElementByTagName 方法")

    *顯示 IntelliSense getElementsByTagName 方法*
13. 選取  **&quot;音訊&quot;** 從 清單 和 按下**ENTER**。 其結果如下圖所示。

    ![擷取音訊元素](visual-studio-2013-web-tools/_static/image46.png "擷取音訊的項目")

    *擷取音訊的項目*
14. 在**方案總管中**，以滑鼠右鍵按一下**init.js**檔案**指令碼**資料夾，然後選取**縮短 JavaScript 檔案**從**Web Essentials**功能表。

    ![縮短 JavaScript 檔案](visual-studio-2013-web-tools/_static/image47.png "縮短 JavaScript 檔案")

    *縮短 JavaScript 檔案*
15. 當系統提示時按一下變更來源檔案啟用自動縮製**是**。

    ![啟用自動縮製警告](visual-studio-2013-web-tools/_static/image48.png "啟用自動縮製警告")

    *啟用自動縮製警告*

    > [!NOTE]
    > **Init.min.js**會建立並加入為具有相依性**init.js**檔案。
    > 
    > ![建立檔案，Init.min.js](visual-studio-2013-web-tools/_static/image49.png "Init.min.js 建立的檔案")
    > 
    > *建立 Init.min.js 檔案*
16. 開啟**init.min.js**檔案，並注意該檔案會縮減。

    ![Init.min.js 檔案內容](visual-studio-2013-web-tools/_static/image50.png "Init.min.js 檔案內容")

    *Init.min.js 檔案內容*
17. 在  **init.js**檔案中，新增下列程式碼下方**getElementsByTagName**函式呼叫來播放音訊的項目。

    (程式碼片段- *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)

    [!code-csharp[Main](visual-studio-2013-web-tools/samples/sample12.cs)]
18. 按一下  **CTRL** + **S**來儲存檔案。 因為縮製的檔案已經開啟，您會看到對話方塊，指出檔案的原始檔編輯器之外修改。 按一下 [ **是**]。

    ![Microsoft Visual Studio 警告](visual-studio-2013-web-tools/_static/image51.png "Microsoft Visual Studio 警告")

    *Microsoft Visual Studio 警告*
19. 切換回**init.min.js**檔案以確認檔案已更新與新的程式碼。

    ![更新的 Init.min.js 檔案](visual-studio-2013-web-tools/_static/image52.png "Init.min.js 檔案已更新")

    *Init.min.js 檔案已更新*
20. 按一下**重新整理瀏覽器連結**按鈕。
21. 一旦這兩個瀏覽器會重新整理您在上一個工作中看到的音訊播放程式會開始自動播放。

    ![在檢視中包含的音訊播放器](visual-studio-2013-web-tools/_static/image53.png "檢視中包含的音訊播放程式")

    *在檢視中包含的音訊播放器*

* * *

<a id="Summary"></a>
## <a name="summary"></a>總結

藉由完成這個實際操作實驗室，您已經學會如何：

- 使用新的 HTML 編輯器功能，例如豐富 HTML5 的程式碼片段和 Zen 撰寫程式碼納入 Web Essentials
- 使用新的 CSS 編輯器功能，包含 Web Essentials 中，色彩選擇器等瀏覽器矩陣工具提示
- 使用新的 JavaScript 編輯器功能，包括 Web Essentials，例如擷取至檔案和 IntelliSense 中所有 HTML 項目
- 您的瀏覽器與 Visual Studio 中使用瀏覽器連結之間交換資料
