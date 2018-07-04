---
uid: web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
title: Visual Studio 2012 中使用 Page Inspector |Microsoft Docs
author: rick-anderson
description: 在這個實際操作實驗室中，您會發現新的工具，找出並修正 Visual Studio-Page Inspector 中的網頁上的問題。 Page Inspector 是新工具 b...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 73232292-a5fe-4720-82a1-8f6553effd1f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 236b739abb8c9073535361040dd7d921da9dba6e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365720"
---
<a name="using-page-inspector-in-visual-studio-2012"></a>Visual Studio 2012 中使用 Page Inspector
====================
藉由[Web Camp 小組](https://twitter.com/webcamps)

> 在這個實際操作實驗室中，您會發現新的工具，找出並修正 Visual Studio-Page Inspector 中的網頁上的問題。
> 
> Page Inspector 是一種新的工具，帶入 Visual Studio 中的瀏覽器診斷工具，並提供在瀏覽器、 ASP.NET 和原始程式碼之間的整合式的體驗。 它會呈現網頁 （HTML、 Web Form、 ASP.NET MVC 或 Web 網頁） 直接在 Visual Studio IDE，並可讓您檢查原始程式碼和產生的輸出。 Page Inspector 可讓您輕鬆分解網站，快速建置全新的頁面，並快速診斷問題。
> 
> 現在我們有幾個即時的方式，例如 ASP.NET MVC 和 WebForms 建立彈性且可調整的網站的 Web 架構。 相反地，它會取得難以找出頁面上的問題，因為 IDE 不支援範本為基礎的頁面和動態內容中的設計工具檢視。 因此，這些網站有在以查看其顯示給使用者的瀏覽器中開啟。
> 
> Web 開發人員會使用外部工具來尋找定期瀏覽器中執行的問題。 然後，他們會返回 IDE，並開始修正。 這來回 IDE、 瀏覽器與程式碼剖析工具之間的活動可以是效率不佳，而且有時候需要全新的部署和快取清除每的次您想要重現問題。
> 
> Page Inspector 橋接 Web 程式開發，用戶端 （瀏覽器工具） 和伺服器 （ASP.NET 和原始程式碼的程式碼） 之間的間距，藉由雙方使用結合的功能集的優點結合在一起。
> 
> 使用 Page Inspector，您可以看到哪些項目 （包括伺服器端程式碼） 的原始程式檔中產生要在瀏覽器中呈現的 HTML 標記。 Page Inspector 也可讓您修改 CSS 屬性，才能看到這些變更會立即反映在瀏覽器的 DOM 項目屬性。
> 
> 這個實際操作實驗室會逐步引導您完成 Page Inspector 功能，並顯示您如何使用它們來修正問題，在 Web 應用程式。 **這個實驗室中包含兩個練習使用類似的流程，但目標為不同的技術。如果您是 ASP.NET MVC 開發人員，請依照下列練習中其中一個;如果您是兩個 WebForms 開發人員遵循練習**。
> 
> 這個實驗室會引導您完成先前所述將微幅的變更套用到來源資料夾中提供的範例 Web 應用程式的新功能與增強功能。
> 
> 所有的範例程式碼和程式碼片段會包含在 Web 研討會訓練套件，可在[ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409)。


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>目標

在這個實際操作實驗室中，您將了解如何：

- 分解網站，使用 Page Inspector
- 檢查並預覽 Page inspector 的 CSS 樣式變更
- 偵測並修正問題，在您使用 Page Inspector 的網頁

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>必要條件

您必須具備下列項目，即可完成此實驗室：

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)或更好 (讀取[附錄 A](#AppendixA)如需有關如何安裝它)。
- Internet Explorer 9 或更高版本

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>練習

這個實際操作實驗室還包含下列練習：

1. [練習 1： 在 ASP.NET MVC 專案中使用 Page Inspector](#Exercise1)
2. [練習 2: WebForms 專案中使用 Page Inspector](#Exercise2)

> [!NOTE]
> 每個練習會伴隨開始的解決方案，可讓您遵循獨立於其他每個練習的練習，開始資料夾中。 在練習的原始程式碼，您也可以找到包含 Visual Studio 方案，以產生對應的練習中的步驟的程式碼端資料夾。 如果您需要其他說明，當您完成這個實際操作實驗室，您可以使用這些解決方案與指引。


完成這個實驗室估計時間： **30 分鐘**。

<a id="Exercise1"></a>

<a id="Exercise_1_Using_Page_Inspector_in_ASPNET_MVC_Projects"></a>
### <a name="exercise-1-using-page-inspector-in-aspnet-mvc-projects"></a>練習 1： 在 ASP.NET MVC 專案中使用 Page Inspector

在此練習中，您將了解如何預覽和偵錯**ASP.NET MVC 4**解決方案使用**Page Inspector**。 首先，您將執行工具以了解功能，以協助 Web 偵錯程序簡短巡覽。 然後，您可在網頁上包含樣式問題。 您將了解如何使用頁面偵測器來尋找會產生問題的原始程式碼，並加以修正。

<a id="Ex1Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>工作 1-瀏覽 Page Inspector

在這個工作中，您將學習如何顯示相片圖庫的 ASP.NET MVC 4 專案的內容中使用 Page Inspector。

1. 開啟**開始**解決方案位於**來源/Ex1-MVC4/開始/** 資料夾。

   1. 您必須下載某些缺少的 NuGet 封裝才能繼續。 若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 套件**。
   2. 在 [**管理 NuGet 套件**] 對話方塊中，按一下**還原**才能下載遺漏的套件。
   3. 最後，按一下 建置方案**建置** | **建置方案**。

      > [!NOTE]
      > 使用 NuGet 的優點之一是，您不需要寄送您的專案中的所有程式庫專案縮小。 NuGet Power tools，藉由指定封裝版本在 Packages.config 檔案中，您將能夠下載所有必要的程式庫第一次執行專案。 這就是為什麼您必須在您開啟現有的方案從這個實驗室之後，執行這些步驟。
2. 在 [方案總管] 中，找出**Index.cshtml**下方檢視 **/views/home**專案資料夾，以滑鼠右鍵按一下它，然後選取**Page Inspector 中的檢視**。

    ![選取要在 Page Inspector 中預覽檔案](using-page-inspector-in-visual-studio-2012/_static/image1.png "選取要在 Page Inspector 中預覽檔案")

    *選取要在 Page Inspector 中預覽檔案*
3. Page Inspector 視窗會顯示 */Home/Index* URL 對應至您所選的檢視的來源。

    ![ThefirstcontactwithPageInspector](using-page-inspector-in-visual-studio-2012/_static/image2.png)

    *第一個連絡人，Page inspector*

    Page Inspector 工具已整合在 Visual Studio 環境中。 偵測器會包含內嵌的瀏覽器，以及功能強大的 HTML 剖析工具。 請注意，您就不必在執行的解決方案，以查看您的網頁的外觀。

    > [!NOTE]
    > Page Inspector 瀏覽器的寬度小於開啟頁面的寬度，當您不會看到頁面正確。 如果發生這種情況，調整 Page Inspector 的寬度。
4. 按一下 **檔案**Page Inspector 中的索引標籤。

    您會看到正在撰寫 [索引] 頁面的所有原始程式檔。 這項功能有助於識別一眼的所有項目，尤其是當您正在使用部分檢視和範本。 請注意，您也可以開啟每個檔案是否您按一下連結。

    ![The-Files-tab](using-page-inspector-in-visual-studio-2012/_static/image3.png)

    *[檔案] 索引標籤*
5. 按一下 **切換檢查模式**位於左側的索引標籤的按鈕。

    這項工具可讓您選取頁面的任何項目，並查看其 HTML 和 Razor 程式碼。

    ![Toggle-Inspection-Mode-button](using-page-inspector-in-visual-studio-2012/_static/image4.png)

    *切換檢查模式按鈕*
6. 在 Page Inspector 瀏覽器中，將滑鼠指標移到頁面項目上。 當您將滑鼠指標移到所呈現任何的頁面部分時，項目類型會顯示，而且對應的來源標記或程式碼會反白顯示 Visual Studio 編輯器中。

    ![Inspectionmodeinaction](using-page-inspector-in-visual-studio-2012/_static/image5.png)

    *檢查模式作用中*

    > [!NOTE]
    > 未最大化的頁面偵測器視窗，或您不能看到預覽索引標籤顯示的原始程式碼。 如果它最大化時，您可以按一下 Page Inspector 中的項目，原始碼的選取項目會出現，但是它會隱藏頁面偵測器視窗。

    如果您注意**Index.cshtml**檔案中，您會注意到的部分的原始程式碼，產生所選的項目會反白顯示。 這項功能有助於長的原始程式檔，提供直接和快速的方式來存取這些程式碼的編輯。

    ![Inspectingelements](using-page-inspector-in-visual-studio-2012/_static/image6.png)

    *檢查項目*
7. 按一下 [**檢查模式中切換**] 按鈕 (![選取 [HTML] 索引標籤，以顯示在 Page Inspector 瀏覽器中呈現的 HTML 程式碼。](using-page-inspector-in-visual-studio-2012/_static/image7.png "選取要顯示在 Page Inspector 瀏覽器中呈現的 HTML 程式碼的 [HTML] 索引標籤。") ) 若要停用資料指標。
8. 選取  **HTML**索引標籤，顯示在 Page Inspector 瀏覽器中呈現的 HTML 程式碼。
9. 在 HTML 標記中，找出與 Koala 連結的清單項目，並加以選取。

    請注意，當您選取的程式碼時，對應的輸出會自動反白顯示在瀏覽器中。 此功能非常有用，若要查看如何呈現網頁上的 HTML 區塊。

    ![在頁面中的選取 HTML 項目](using-page-inspector-in-visual-studio-2012/_static/image8.png "頁面中的選取 HTML 項目")

    *在頁面中選取 HTML 項目*
10. 按一下 [**檢查模式中切換**] 按鈕以啟用*檢查模式*，按一下巡覽列。 右側的 [樣式] 窗格中中的 HTML 程式碼，您會看到套用至所選元素的 CSS 樣式的清單。

    > [!NOTE]
    > 由於標頭是站台版面配置的一部分，也會開啟 Page Inspector \_Layout.cshtml 檔案並反白顯示受影響的程式碼的區段。

    ![Discoveringstyles](using-page-inspector-in-visual-studio-2012/_static/image9.png)

    *探索樣式和選取項目的原始程式檔*
11. 啟用切換檢查指標，將滑鼠指標下方的藍色的精選列，然後按一下 一半的圓形。

    ![選取項目](using-page-inspector-in-visual-studio-2012/_static/image10.png "選取項目")

    *選取項目*
12. 在 [樣式] 窗格中，找出**背景影像**項目底下 **.main 內容**群組。 **取消核取****背景影像**，看看結果。 您會發現瀏覽器將會立即反映所做的變更，並隱藏圓形。

    > [!NOTE]
    > 您在頁面偵測器樣式 索引標籤套用的變更不會影響原始的樣式表。 您可以取消核取樣式，或變更其值做為許多次，但它們會還原之後重新整理頁面。

    ![啟用和停用 CSS 樣式](using-page-inspector-in-visual-studio-2012/_static/image11.png "啟用和停用 CSS 樣式")

    *啟用和停用 CSS 樣式*
13. 現在，按一下 '**您的標誌**' 上使用檢查模式中的標頭的文字。
14. 在 **樣式**索引標籤上，找出**字型大小**CSS 屬性底下 **.site 標題**群組。 按兩下屬性值，並將 2.3 em 值取代**3 em**，然後按**ENTER**。 請注意，標題看起來更大。

    ![Page Inspector 中的 CSS 值的變化](using-page-inspector-in-visual-studio-2012/_static/image12.png "Page Inspector 中的變更的 CSS 值")

    *Page Inspector 中變更的 CSS 值*
15. 按一下 **追蹤樣式**索引標籤上，Page Inspector 的右窗格中。 這是替代的方式，以查看套用至選取範圍，按照屬性名稱的所有樣式。

    ![CSSstylestracing](using-page-inspector-in-visual-studio-2012/_static/image13.png)

    *CSS 樣式追蹤 選取的項目*
16. Page Inspector 的另一個功能是版面配置 窗格。 使用檢查模式中，選取導覽列，然後再按一下**版面配置**在右窗格中的索引標籤。 您會看到選取的項目，確切的大小，以及其位移、 邊界、 邊框距離及框線的大小。 請注意，您也可以修改此檢視中的值。

    ![Page Inspector 中的項目配置](using-page-inspector-in-visual-studio-2012/_static/image14.png "Page Inspector 中的項目配置")

    *Page Inspector 中的項目配置*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>工作 2-尋找和修正相片圖庫中的樣式問題

如何診斷與舊版 Visual Studio 的 Web 網頁問題？ 您就可能已經熟悉的 web 偵錯 Visual Studio IDE，例如 Internet Explorer 開發人員工具或 Firebug 外部執行的工具。 只有瀏覽器了解的 HTML 指令碼和樣式，雖然基礎架構會產生將轉譯的 HTML。 基於這個理由，您通常需要部署整個網站，以查看網頁的外觀。

當您想要偵測及修正的問題，在您的網站時，您可能必須遵循下列步驟：

1. 從 Visual Studio 中，執行方案，或部署 web 伺服器上的頁面。
2. 在瀏覽器中，開啟您使用，或只要開啟原始碼和樣式，然後試著比對此問題的開發人員工具。 若要尋找相關的檔案，您會使用&quot;搜尋&quot;或&quot;在檔案中的搜尋&quot;的樣式類別名稱的功能。
3. 一旦偵測到錯誤時，會停止 Web 瀏覽器和伺服器。
4. 清除瀏覽器快取。
5. 返回 Visual Studio 來套用修正程式。 重複執行測試的所有步驟。

因為 ASP.NET MVC 4 中沒有任何實際的 WYSIWYG，大部分的樣式問題上所偵測到稍後的階段之後執行，或部署 web 應用程式。 現在，Page inspector 就可能需要先預覽任何頁面，而不需執行解決方案。

在這個工作中，您會使用 Page inspector，並修正相片圖庫應用程式的一些問題。

1. 使用 Page Inspector，找出**註冊**並**登入**標頭的左側的連結。

    請注意連結不會顯示在右側，預期的位置，而且它們會顯示類似於項目符號清單。 您現在將會對齊右邊的連結，並據以重新加以設定樣式。

    ![尋找連結中的暫存器和記錄](using-page-inspector-in-visual-studio-2012/_static/image15.png "尋找暫存器和記錄檔中的連結")

    *尋找連結中的暫存器和記錄檔*
2. 切換檢查模式中選取，按一下 [關閉] 以，但不是能在以開啟其程式碼的 [註冊] 連結。

    請注意，連結的原始程式碼位於 **\_LoginPartial.cshtml**檔案，不 Index.cshtml 和\_Layout.cshtml，也就是您可以看看第一個位置中的位置。 您已經放入正確的原始程式檔直接。
3. 在 **樣式**索引標籤上，找出並按一下  **<section> #login</section>** 項目，也就是這些連結的 HTML 容器。

    請注意， **#login**樣式會自動位於**Site.css**按一下之後。 此外，現在會醒目提示程式碼。

    ![選取的 CSS 樣式](using-page-inspector-in-visual-studio-2012/_static/image16.png "選取的 CSS 樣式")

    *選取的 CSS 樣式*
4. 取消註解**文字對齊**醒目提示的程式碼中移除開頭和結尾字元的屬性，並儲存**Site.css**檔案。

    Page Inspector 知道撰寫目前的頁面上，所有不同檔案的存在，而且它可以偵測到任何這些檔案的變更時。 它會警告您只要在瀏覽器中目前的頁面不是與原始程式檔的同步處理。
5. 在 Page Inspector 瀏覽器中，按一下位於 [網址] 列，以重新載入頁面下方的列。

    ![重新載入頁面](using-page-inspector-in-visual-studio-2012/_static/image17.png)

    *重新載入頁面*

    連結現在會在右側，但它們仍然看起來像是項目符號清單。 現在，您將會移除項目符號，並水平對齊的連結。

    ![更新後的頁面](using-page-inspector-in-visual-studio-2012/_static/image18.png)

    *更新後的頁面*
6. 使用檢查模式中，選取任一**&lt;li&gt;** 包含的項目&quot;註冊&quot;並&quot;登入&quot;連結。 然後，按一下 **&lt; 區段&gt;#login**項目來存取**Styles.css**程式碼。

    ![尋找樣式](using-page-inspector-in-visual-studio-2012/_static/image19.png "尋找樣式")

    *尋找樣式*
7. 在  **Style.css**，取消註解的程式碼 **#login li**項目。 您要加入的樣式會隱藏項目符號，並以水平方式顯示的項目。

    ![右側的登入連結](using-page-inspector-in-visual-studio-2012/_static/image20.png "樣式重新設定的登入連結")

    *右側的登入連結*
8. 儲存**Style.css**檔案，然後按一下下方 要重新載入頁面的位址列上的一次。 請注意，連結會正確出現。

    ![連結靠右對齊](using-page-inspector-in-visual-studio-2012/_static/image21.png "連結靠右對齊")

    *靠右對齊的連結*
9. 最後，您將變更標頭標題。 使用檢查模式中，按一下**您的標誌**文字並開始產生的原始程式碼。
10. 現在您已在 **\_Layout.cshtml**，取代 '**您的標誌**'與 text'**相片圖庫**'。 儲存並更新 Page Inspector 瀏覽器。

    ![指派新的標題](using-page-inspector-in-visual-studio-2012/_static/image22.png "指派新的標題")

    *指派新的標題*

    ![PhotoGallerypage](using-page-inspector-in-visual-studio-2012/_static/image23.png)

    *更新的 [相片圖庫] 頁面*
11. 最後，選取**PhotoGallery**專案，然後按**F5**執行應用程式。 查看所有變更運作如預期般運作。

* * *

<a id="Exercise2"></a>

<a id="Exercise_2_Using_Page_Inspector_in_WebForms_Projects"></a>
### <a name="exercise-2-using-page-inspector-in-webforms-projects"></a>練習 2: WebForms 專案中使用 Page Inspector

在此練習中，您將了解如何預覽，並使用 Page Inspector WebForms 方案進行偵錯。 您會先執行簡短瀏覽工具若要了解 Page Inspector 功能，以協助 Web 偵錯程序。 然後，您可在網頁上包含樣式問題。 您將了解如何使用頁面偵測器來尋找會產生問題的原始程式碼，並加以修正。

<a id="Ex2Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>工作 1-瀏覽 Page Inspector

在這個工作中，您將學習如何在 顯示相片圖庫 WebForms 專案內容中使用 Page Inspector 功能。

1. 開啟**開始**解決方案位於**來源/Ex2-WebForms/開始/** 資料夾。

   1. 您必須下載某些缺少的 NuGet 封裝才能繼續。 若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 套件**。
   2. 在 [**管理 NuGet 套件**] 對話方塊中，按一下**還原**才能下載遺漏的套件。
   3. 最後，按一下 建置方案**建置** | **建置方案**。

      > [!NOTE]
      > 使用 NuGet 的優點之一是，您不需要寄送您的專案中的所有程式庫專案縮小。 NuGet Power tools，藉由指定封裝版本在 Packages.config 檔案中，您將能夠下載所有必要的程式庫第一次執行專案。 這就是為什麼您必須在您開啟現有的方案從這個實驗室之後，執行這些步驟。
2. 在 [方案總管] 中，找出**Default.aspx**頁面上，以滑鼠右鍵按一下它，然後選取**Page Inspector 中的檢視**。

    ![Page inspector 中開啟 Default.aspx](using-page-inspector-in-visual-studio-2012/_static/image24.png "開啟 Default.aspx，Page inspector")

    *開啟 Default.aspx，Page inspector*
3. Page Inspector 視窗會顯示 Default.aspx。

    ![Page Inspector 中檢視 Default.aspx](using-page-inspector-in-visual-studio-2012/_static/image25.png "Page Inspector 中檢視 Default.aspx")

    *Page Inspector 中檢視 Default.aspx*

    Page Inspector 工具已整合在 Visual Studio 環境中。 偵測器會包含內嵌的瀏覽器，以及功能強大的 HTML 程式碼，而該剖析工具會顯示選取的程式碼。 請注意，您就不必在執行的解決方案，以查看您的網頁的外觀。

    > [!NOTE]
    > Page Inspector 瀏覽器的寬度小於開啟頁面的寬度，當您不會看到頁面正確。 如果發生這種情況，調整 Page Inspector 的寬度。
4. 按一下 **檔案**Page Inspector 中的索引標籤。

    您會看到正在撰寫所呈現的預設網頁的所有原始程式檔。 這是很有用的功能，以識別一眼的所有項目，尤其是當您正在使用使用者控制項和主版頁面。 請注意，您也可以巡覽至每個檔案。

    ![[檔案] 索引標籤](using-page-inspector-in-visual-studio-2012/_static/image26.png "檔案 索引標籤")

    *[檔案] 索引標籤*
5. 按一下 **切換檢查模式**位於左側的索引標籤的按鈕。

    這項工具可讓您選取頁面的任何項目，並查看它的 HTML 程式碼和.aspx 原始檔。

    ![切換檢查模式按鈕](using-page-inspector-in-visual-studio-2012/_static/image27.png "切換檢查模式按鈕")

    *切換檢查模式按鈕*
6. 在 Page Inspector 瀏覽器中，請將滑鼠移頁面項目。 當您將滑鼠指標移到所呈現任何的頁面部分時，項目類型會顯示，而且對應的來源標記或程式碼會反白顯示 Visual Studio 編輯器中。

    ![檢查模式中運作](using-page-inspector-in-visual-studio-2012/_static/image28.png "檢查模式作用中")

    *檢查模式作用中*

    > [!NOTE]
    > 未最大化的頁面偵測器視窗，或您不能看到預覽索引標籤顯示的原始程式碼。 如果它最大化時，您可以按一下 Page Inspector 中的項目，原始碼的選取項目會出現，但是它會隱藏頁面偵測器視窗。

    如果您注意**Default.aspx**檔案中，您會注意到的部分的原始程式碼，產生所選的項目會反白顯示。 這項功能有助於長的原始程式檔，提供直接和快速的方式來存取這些程式碼的版本。

    ![檢查項目](using-page-inspector-in-visual-studio-2012/_static/image29.png "檢查項目")

    *檢查項目*
7. 按一下 [**檢查模式中切換**] 按鈕 (![Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser。](using-page-inspector-in-visual-studio-2012/_static/image30.png "Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser。") )，位於 Page Inspector 的索引標籤中，若要停用資料指標。
8. 選取  **HTML**索引標籤，顯示在 Page Inspector 瀏覽器中呈現的 HTML 程式碼。
9. 中的 HTML 程式碼中，找出與 Koala 連結的清單項目，並加以選取。

    請注意，當您選取的程式碼時，對應的輸出會自動反白顯示的瀏覽器。 此功能非常有用，若要查看如何呈現網頁上的 HTML 區塊。

    ![在頁面中選取 HTML 項目](using-page-inspector-in-visual-studio-2012/_static/image31.png "在頁面中選取 HTML 項目")

    *在頁面中選取 HTML 項目*
10. 按一下 [**檢查模式中切換**] 按鈕以啟用*檢查模式*，按一下巡覽列。 右側的 [樣式] 窗格中中的 HTML 程式碼，您會看到套用至所選元素的 CSS 樣式的清單。

    > [!NOTE]
    > 由於標頭是站台版面配置的一部分，Page Inspector 也會開啟 Site.Master 檔案，並反白顯示受影響的程式碼區段。

    ![DiscoveringstylesWebForms](using-page-inspector-in-visual-studio-2012/_static/image32.png "探索樣式和選取項目的原始程式檔")

    *探索樣式和選取項目的原始程式檔*
11. 啟用切換檢查指標，將滑鼠指標在功能表列下方，然後按一下 空白的二分之一圓。

    ![選取項目](using-page-inspector-in-visual-studio-2012/_static/image33.png "選取項目")

    *選取項目*
12. 在 [樣式] 窗格中，找出**背景影像**項目底下 **.main 內容**群組。 **取消核取****背景影像**，看看結果。 您會發現瀏覽器將會立即反映所做的變更，並隱藏圓形。

    > [!NOTE]
    > 您在頁面偵測器樣式 索引標籤套用的變更不會影響原始的樣式表。 您可以取消核取樣式，或變更其值做為許多次，但它們會還原之後重新整理頁面。

    ![啟用和停用 CSS styles2](using-page-inspector-in-visual-studio-2012/_static/image34.png "啟用和停用 CSS 樣式")

    *啟用和停用 CSS 樣式*
13. 現在，按一下 '**您****標誌的位置'** 上使用檢查模式中的標頭的文字。
14. 在 **樣式**索引標籤上，找出**字型大小**CSS 屬性底下 **.site 標題**群組。 按兩下以編輯其值的屬性一次。 值取代 2.3em **3em**，然後按 ENTER 鍵。 請注意，標題看起來更大。

    ![變更頁面 Inspector2 中的 CSS 值](using-page-inspector-in-visual-studio-2012/_static/image35.png "Page Inspector 中的變更的 CSS 值")

    *Page Inspector 中變更的 CSS 值*
15. 按一下 **追蹤樣式**索引標籤上，Page Inspector 的右窗格中。 這是替代的方式，以查看套用至選取範圍，按照屬性名稱的所有樣式。

    ![選取項目的 CSS 樣式追蹤](using-page-inspector-in-visual-studio-2012/_static/image36.png "所選項目的 CSS 樣式追蹤")

    *CSS 樣式追蹤 選取的項目*
16. Page Inspector 的另一個功能是版面配置 窗格。 使用檢查模式中，選取導覽列，然後再按一下**版面配置**在右窗格中的索引標籤。 您會看到選取的項目，確切的大小，以及其位移、 邊界、 邊框距離及框線的大小。 請注意，您也可以修改此檢視中的值。

    ![Page Inspector 中的項目配置](using-page-inspector-in-visual-studio-2012/_static/image37.png "Page Inspector 中的項目配置")

    *Page Inspector 中的項目配置*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>工作 2-尋找和修正相片圖庫中的樣式問題

如何診斷與舊版 Visual Studio 的 Web 網頁問題？ 您就可能已經熟悉的 web 偵錯 Visual Studio IDE，例如 Internet Explorer 開發人員工具或 Firebug 外部執行的工具。 只有瀏覽器了解的 HTML 指令碼和樣式，雖然基礎架構會產生將轉譯的 HTML。 基於這個理由，您通常需要部署整個網站，以查看網頁的外觀。

當您想要偵測及修正的問題，在您的網站時，您可能必須遵循下列步驟：

1. 從 Visual Studio 中，執行方案，或部署 web 伺服器上的頁面。
2. 在瀏覽器中，開啟您使用，或只要開啟原始碼和樣式，然後試著比對此問題的開發人員工具。 若要尋找相關的檔案，您會使用&quot;搜尋&quot;或&quot;在檔案中的搜尋&quot;的樣式類別名稱的功能。
3. 一旦偵測到錯誤時，會停止 Web 瀏覽器和伺服器。
4. 清除瀏覽器快取。
5. 返回 Visual Studio 來套用修正程式。 重複執行測試的所有步驟。

因為沒有不在 ASP.NET WebForms 真正 WYSIWYG，某些樣式問題上偵測到稍後的階段之後執行，或部署。 現在，Page inspector 就可能需要先預覽任何頁面，而不需執行解決方案。

在這個工作中，您將使用 Page inspector 修正相片圖庫應用程式的一些問題。 在下列步驟中，您將會偵測並快速修正標頭中的 簡單樣式的一些問題。

1. 使用頁面檢查，找出**註冊**並**登入**標頭的左側的連結。

    請注意，連結不會顯示在右邊的預期位置。 您現在將會對齊右邊的連結，並據以重新設定樣式。

    ![登入位於左側的連結](using-page-inspector-in-visual-studio-2012/_static/image38.png "登入位於左側的連結")

    *位於左側的登入連結*
2. 切換選取的檢查模式下，選取 登入連結以開啟其程式碼。

    請注意，連結來源的程式碼位於**Site.Master**檔案，不是取代 Default.aspx 頁面中您可以看看在第一個位置; 您已放置在正確的原始程式檔直接。
3. 在 **樣式**索引標籤上，找出並按一下  **&lt;一節&gt;#login**項目，也就是這些連結的 HTML 容器。

    請注意， **#login**樣式會自動位於**Site.css**按一下之後。 此外，現在會醒目提示程式碼。

    ![選取的 CSS 樣式](using-page-inspector-in-visual-studio-2012/_static/image39.png "選取的 CSS 樣式")

    *選取的 CSS 樣式*
4. 取消註解**文字對齊**醒目提示的程式碼中移除開頭和結尾字元的屬性，並儲存**Site.css**檔案。

    Page Inspector 知道撰寫目前的頁面上，所有不同檔案的存在，而且它可以偵測到任何這些檔案的變更時。 它會警告您只要在瀏覽器中目前的頁面不是與原始程式檔的同步處理。
5. 在 Page Inspector 瀏覽器中，按一下位於 [網址] 列，以儲存變更並重新載入頁面下方的列。

    ![Reloadingthepage](using-page-inspector-in-visual-studio-2012/_static/image40.png)

    *重新載入頁面*

    連結現在會在右側，但它們仍然看起來像是項目符號清單。 現在，您將會移除項目符號，並水平對齊的連結。

    ![更新後的頁面](using-page-inspector-in-visual-studio-2012/_static/image41.png)

    *更新後的頁面*
6. 使用檢查模式中，選取任一**&lt;li&gt;** 包含的項目&quot;註冊&quot;並&quot;登入&quot;連結。 然後，按一下 **&lt; 區段&gt;#login**項目來存取**Styles.css**程式碼。

    ![尋找樣式](using-page-inspector-in-visual-studio-2012/_static/image42.png "尋找樣式")

    *尋找樣式*
7. 在  **Style.css**，取消註解的程式碼 **#login li**項目。 您要加入的樣式會隱藏項目符號，並以水平方式顯示的項目。

    ![右側的登入連結](using-page-inspector-in-visual-studio-2012/_static/image43.png "樣式重新設定的登入連結")

    *右側的登入連結*
8. 儲存**Style.css**檔案，然後按一下下方 要重新載入頁面的位址列上的一次。 請注意，連結會正確出現。

    ![連結靠右對齊](using-page-inspector-in-visual-studio-2012/_static/image44.png "連結靠右對齊")

    *靠右對齊的連結*
9. 最後，您將變更標頭標題。 而不是搜尋 '**您的標誌'** 文字中的所有檔案，按一下該文字，並取得會產生它的原始碼中使用檢查模式。

    ![尋找網站標題](using-page-inspector-in-visual-studio-2012/_static/image45.png "尋找網站標題")

    *尋找網站標題*
10. 現在您已在**Site.Master**，取代 '**您的標誌**'與 text'**相片圖庫**'。 儲存並更新 Page Inspector 瀏覽器。

    ![相片圖庫 頁面更新](using-page-inspector-in-visual-studio-2012/_static/image46.png "相片圖庫 頁面更新")

    *更新的 [相片圖庫] 頁面*
11. 最後按下**F5**執行應用程式的簽出所有變更運作如預期般運作。

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>總結

藉由完成這個實際操作實驗室，您已學會如何使用 Page Inspector 預覽您的 Web 應用程式，而不需要重建和瀏覽器中執行的網站。 此外，您已學會如何快速找到並修正 bug，直接從轉譯的輸出存取原始程式碼。

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>附錄 a： 安裝 Visual Studio Express 2012 for Web

您可以安裝**Microsoft Visual Studio Express 2012 for Web**或另一個&quot;Express&quot;使用版本**[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. 下列指示會引導您完成安裝所需的步驟*Visual studio Express 2012 for Web*使用*Microsoft Web Platform Installer*。

1. 移至[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169)。 或者，如果您已安裝 Web Platform Installer，您可以開啟它，並搜尋產品&quot; <em>Visual Studio Express 2012 for Web 含 Windows Azure SDK</em>&quot;。
2. 按一下 **立即安裝**。 如果您不需要**Web Platform Installer**您將會重新導向至下載並安裝第一次。
3. 一次**Web Platform Installer**已開啟，按一下**安裝**，啟動安裝程式。

    ![安裝 Visual Studio Express](using-page-inspector-in-visual-studio-2012/_static/image47.png "安裝 Visual Studio Express")

    *安裝 Visual Studio Express*
4. 閱讀所有產品的授權和詞彙，然後按一下**我接受**以繼續。

    ![接受授權條款](using-page-inspector-in-visual-studio-2012/_static/image48.png)

    *接受授權條款*
5. 等候完成的下載與安裝程序。

    ![安裝進度](using-page-inspector-in-visual-studio-2012/_static/image49.png)

    *安裝進度*
6. 安裝完成時，按一下**完成**。

    ![安裝已完成](using-page-inspector-in-visual-studio-2012/_static/image50.png)

    *安裝已完成*
7. 按一下 **結束**關閉 Web Platform Installer。
8. 若要開啟 Visual Studio Express for Web，請前往**開始**畫面，即可開始撰寫&quot; **VS Express**&quot;，然後按一下**VS Express for Web**圖格。

    ![VS Express for Web 圖格](using-page-inspector-in-visual-studio-2012/_static/image51.png)

    *VS Express for Web 圖格*
