---
uid: web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
title: 在 Visual Studio 2012 中使用 Page Inspector |Microsoft 文件
author: rick-anderson
description: 在這個實際操作實驗室中，您會發現新的工具，尋找並修正 Visual Studio-Page Inspector 中的網頁上的問題。 Page Inspector 這項新工具的 b...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 73232292-a5fe-4720-82a1-8f6553effd1f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 052d29dba170d403c2b1c1667c55fc2c34045615
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="using-page-inspector-in-visual-studio-2012"></a>Page Inspector 使用 Visual Studio 2012 中
====================
由[Web 營小組](https://twitter.com/webcamps)

> 在這個實際操作實驗室中，您會發現新的工具，尋找並修正 Visual Studio-Page Inspector 中的網頁上的問題。
> 
> Page Inspector 是一種新的工具，將瀏覽器診斷工具整合到 Visual Studio，並提供在瀏覽器、 ASP.NET 和原始程式碼之間的整合式的經驗。 它會呈現網頁 （HTML、 Web Form、 ASP.NET MVC 或 Web 網頁） 直接在 Visual Studio IDE，並可讓您檢查原始程式碼與產生的輸出。 Page Inspector 可讓您輕鬆分解網站，快速建置從頭頁面，以及快速診斷問題。
> 
> Screen 鍵我們有個及時，例如 ASP.NET MVC 和 WebForms 建立彈性且可擴充的網站的 Web 架構的數字。 相反地，它會取得很難找到問題的頁面上，因為 IDE 不支援在 template 為基礎的網頁和動態內容中的設計工具檢視。 因此，這些網站有要查看它們在使用者的瀏覽器中開啟。
> 
> Web 開發人員會使用外部工具來尋找定期瀏覽器中執行的問題。 然後，它們會返回 IDE，並開始修正。 這來回 IDE、 瀏覽器和程式碼剖析工具之間的活動可以效率不佳，而且有時候需要重新部署和快取清除每的次您想要重現問題。
> 
> Page Inspector 將結合在一起使用的功能集結合這兩個領域的最佳橋接器中 Web 程式開發 （瀏覽器工具） 的用戶端和伺服器 （ASP.NET 和原始程式碼的程式碼） 之間的間距。
> 
> 使用 Page Inspector 時，您可以查看哪些項目 （包括伺服器端程式碼） 的原始程式檔中已產生要在瀏覽器中呈現的 HTML 標記。 Page Inspector 也可讓您修改 CSS 屬性與 DOM 項目屬性，才能看到這些變更會立即反映在瀏覽器。
> 
> 這個實際操作實驗室將引導您完成 Page Inspector 功能，並示範如何使用它們以 Web 應用程式中修正問題。 **這個實驗室中包含兩個練習使用類似的流程，但目標為不同的技術。如果您是 ASP.NET MVC 開發人員，請依照下列練習中，如果您是兩個 WebForms 開發人員依照練習**。
> 
> 這個實驗室將引導您完成先前所述將微幅變更套用至來源資料夾中提供的範例 Web 應用程式的新功能與增強功能。
> 
> 所有的範例程式碼和程式碼片段會包含在 Web 營訓練套件，可在[ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409)。


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>目標

在這個實際操作實驗室中，您將學會如何：

- 分解使用 Page Inspector 的網站
- 檢查並預覽 Page inspector 的 CSS 樣式變更
- 偵測及修正您使用 Page Inspector 的網頁中的問題

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>必要條件

您必須具備下列項目才能完成本實驗室：

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)或上層 (讀取[附錄 A](#AppendixA)如需有關如何安裝指示)。
- Internet Explorer 9 或更高版本

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>練習

這個實際操作實驗室包含下列練習：

1. [練習 1： 在 ASP.NET MVC 專案中使用 Page Inspector](#Exercise1)
2. [練習 2： 在 WebForms 專案中使用 Page Inspector](#Exercise2)

> [!NOTE]
> 每個練習會伴隨起始方案，練習，可讓您依照每個練習各自的 Begin 資料夾中。 內部練習的原始程式碼，您也可以找到包含在 Visual Studio 方案中使用的程式碼所產生對應的練習中的步驟結束資料夾。 如果您需要其他說明，當您完成這個實際操作實驗室，您可以使用這些解決方案做為指導。


完成本實驗室估計時間： **30 分鐘**。

<a id="Exercise1"></a>

<a id="Exercise_1_Using_Page_Inspector_in_ASPNET_MVC_Projects"></a>
### <a name="exercise-1-using-page-inspector-in-aspnet-mvc-projects"></a>練習 1： 在 ASP.NET MVC 專案中使用 Page Inspector

在此練習中，您將學習如何預覽和偵錯**ASP.NET MVC 4**方案中使用**Page Inspector**。 首先，您將會執行一簡短圈周圍工具若要了解促進 Web 偵錯處理序的功能。 然後，您會使用包含樣式問題的網頁。 您將學習如何使用 Page Inspector 尋找會產生問題的原始程式碼，並加以修正。

<a id="Ex1Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>工作 1-Page Inspector 瀏覽

在這項工作，您將學習如何使用 Page Inspector 顯示相片圖庫的 ASP.NET MVC 4 專案的內容中。

1. 開啟**開始**方案位於**來源/Ex1MVC4/開始/**資料夾。

   1. 您必須下載某些遺漏的 NuGet 套件才能繼續。 若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 封裝**。
   2. 在**管理 NuGet 封裝**] 對話方塊中，按一下 [**還原**才能下載遺漏的封裝。
   3. 最後，按一下 建置方案**建置** | **建置方案**。

      > [!NOTE]
      > 使用 NuGet 的優點之一是您不需要在專案中，所有的程式庫的出貨減少專案大小。 NuGet 的強大工具，請藉由指定封裝版本在 Packages.config 檔案中，您將會成功下載所有必要的程式庫第一次您執行專案。 這就是為什麼您必須從這個實驗室中開啟現有的方案後執行這些步驟。
2. 在 [方案總管] 中，找出**Index.cshtml**檢視下**/檢視表/首頁**專案資料夾中，以滑鼠右鍵按一下並選取**Page Inspector 中的檢視**。

    ![選取要在 Page Inspector 中預覽檔案](using-page-inspector-in-visual-studio-2012/_static/image1.png "選取要在 Page Inspector 中的檔案")

    *選取要在 Page Inspector 中的檔案*
3. Page Inspector 視窗會顯示*/Home/Index* URL 對應至來源您選取的檢視。

    ![ThefirstcontactwithPageInspector](using-page-inspector-in-visual-studio-2012/_static/image2.png)

    *Page inspector 的第一個連絡人*

    Page Inspector 工具整合在 Visual Studio 環境中。 偵測器包含內嵌的瀏覽器，以及一個功能強大的 HTML 分析工具。 請注意，您不需要執行此方案，以查看您的網頁的外觀。

    > [!NOTE]
    > Page Inspector 瀏覽器的寬度小於開啟頁面的寬度，當您不會看到頁面正確。 如果發生這種情況，調整 Page Inspector 的寬度。
4. 按一下**檔案**Page Inspector 中的索引標籤。

    您會看到所撰寫的索引頁面的所有原始程式檔。 這項功能有助於識別您一眼的所有項目，特別是當您正在使用的部分檢視和範本。 請注意，您也可以開啟每個檔案是否按的連結。

    ![The-Files-tab](using-page-inspector-in-visual-studio-2012/_static/image3.png)

    *[檔案] 索引標籤*
5. 按一下**切換檢查模式**位於索引標籤左邊的按鈕。

    此工具可讓您選取網頁的任何項目，並查看其 HTML 和 Razor 程式碼。

    ![Toggle-Inspection-Mode-button](using-page-inspector-in-visual-studio-2012/_static/image4.png)

    *切換檢查模式按鈕*
6. 在 Page Inspector 瀏覽器中，滑鼠指標移到頁面項目上。 當您移動滑鼠指標移到所呈現頁面的任意處時，就會顯示項目類型和對應的來源標記或程式碼會反白顯示在 Visual Studio 編輯器中。

    ![Inspectionmodeinaction](using-page-inspector-in-visual-studio-2012/_static/image5.png)

    *檢查模式作用中*

    > [!NOTE]
    > 不會最大化 Page Inspector 視窗或您無法看到預覽索引標籤顯示的原始程式碼。 如果它最大化時，您可以按一下 Page Inspector 中的項目，顯示選取項目的原始碼，但是它將會隱藏頁面偵測器視窗。

    如果您注意**Index.cshtml**檔案中，您會注意到產生選取的項目之原始程式碼的一部分會反白顯示。 這項功能可協助長的來源檔案，提供直接和快速的方式來存取這些程式碼的編輯。

    ![Inspectingelements](using-page-inspector-in-visual-studio-2012/_static/image6.png)

    *檢查項目*
7. 按一下**切換檢查模式**按鈕 (![選取 [HTML] 索引標籤顯示在 Page Inspector 瀏覽器中呈現的 HTML 程式碼。](using-page-inspector-in-visual-studio-2012/_static/image7.png "選取 [HTML] 索引標籤顯示在 Page Inspector 瀏覽器中呈現的 HTML 程式碼。") ) 可停用資料指標。
8. 選取**HTML**索引標籤，顯示在 Page Inspector 瀏覽器中呈現的 HTML 程式碼。
9. 在 HTML 標記中，找出 Koala 連結的清單項目並選取它。

    請注意，當您選取的程式碼時，對應的輸出自動反白顯示在瀏覽器中。 此功能十分有用，若要查看如何轉譯頁面上的 HTML 區塊。

    ![在頁面中的選取 HTML 項目](using-page-inspector-in-visual-studio-2012/_static/image8.png "頁面中的選取 HTML 項目")

    *頁面中選取 HTML 項目*
10. 按一下**切換檢查模式**按鈕以啟用*檢查模式*按一下導覽列。 右邊的 [樣式] 窗格中的 HTML 程式碼，您會看到套用至選取的元素的 CSS 樣式清單。

    > [!NOTE]
    > 由於標頭是站台版面配置的一部分，也會開啟 Page Inspector \_Layout.cshtml 檔案並反白顯示程式碼的區段會受到影響。

    ![Discoveringstyles](using-page-inspector-in-visual-studio-2012/_static/image9.png)

    *探索樣式和原始程式檔的選取項目*
11. 切換檢查指標已啟用，將滑鼠指標在藍色精選列下方然後按一下 半圓形。

    ![選取項目](using-page-inspector-in-visual-studio-2012/_static/image10.png "選取項目")

    *選取項目*
12. 在 [樣式] 窗格中，找出**背景影像**項目底下**.main 內容**群組。 **取消核取****背景影像**看。 您會注意到瀏覽器會立即反映變更，並隱藏圓形。

    > [!NOTE]
    > 您在頁面偵測器樣式 索引標籤套用的變更不會影響原始的樣式表。 您可以取消核取樣式，或變更其值依需求多次，但將會還原之後重新整理頁面。

    ![啟用和停用的 CSS 樣式](using-page-inspector-in-visual-studio-2012/_static/image11.png "啟用和停用的 CSS 樣式")

    *啟用和停用的 CSS 樣式*
13. 現在，按一下 '**您的標誌**' 上使用檢查模式的標頭的文字。
14. 在**樣式**索引標籤上，找出**字型大小**CSS 屬性下**.site 標題**群組。 按兩下屬性值，並取代 2.3 em 值與**3 em**，然後按下**ENTER**。 請注意，標題起來更大。

    ![變更頁面偵測器中的 CSS 值](using-page-inspector-in-visual-studio-2012/_static/image12.png "Page Inspector 中的變更的 CSS 值")

    *Page Inspector 中變更的 CSS 值*
15. 按一下**追蹤樣式**索引標籤上，Page Inspector 的右窗格中。 這是以查看所有的樣式套用至選取範圍中，依屬性名稱的替代方法。

    ![CSSstylestracing](using-page-inspector-in-visual-studio-2012/_static/image13.png)

    *CSS 樣式追蹤 選取的項目*
16. Page Inspector 的另一個功能是 [配置] 窗格。 使用檢查模式時，選取導覽列，然後按一下**配置**在右窗格中的索引標籤。 您會看到的選取項目，確切的大小，以及其位移、 邊界、 邊框距離及框線的大小。 請注意，您也可以修改此檢視中的值。

    ![Page Inspector 中的項目配置](using-page-inspector-in-visual-studio-2012/_static/image14.png "Page Inspector 中的項目配置")

    *Page Inspector 中的項目配置*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>工作 2-尋找和修正相片圖庫中的樣式問題

您要如何診斷與舊版 Visual Studio 網頁問題？ 也可能已經熟悉 web 偵錯在 Visual Studio IDE，像是 Internet Explorer 開發人員工具或 firebug 這類外部執行的工具。 瀏覽器只了解 HTML、 指令碼和樣式，而基礎架構會產生將呈現的 HTML。 因此，您通常需要部署整個站台，以查看網頁外觀如何。

當您想要偵測及修正問題，您的網站中，您可能必須遵循下列步驟：

1. 從 Visual Studio 中，執行此方案，或部署在 web 伺服器上的頁面。
2. 在瀏覽器中開啟您使用，或只要開啟原始碼和樣式，然後試著比對此問題的開發人員工具。 若要尋找相關檔案，您會使用&quot;搜尋&quot;或&quot;檔案中的搜尋&quot;的樣式類別名稱的功能。
3. 一旦偵測到錯誤時，停止 Web 瀏覽器和伺服器。
4. 清除瀏覽器快取。
5. 返回 Visual Studio 套用修正程式。 重複執行測試的所有步驟。

由於 ASP.NET MVC 4 中沒有實際 WYSIWYG，大部分的樣式問題上偵測到後續階段中，執行或部署 web 應用程式之後。 現在，Page inspector，便可預覽任何頁面而無須執行方案。

在這項工作，您將使用 Page inspector，並修正相片圖庫應用程式的一些問題。

1. 使用 Page Inspector 找出**註冊**和**登入**標頭的左側的連結。

    請注意，連結不會顯示在右側，預期的位置都會顯示在類似項目符號清單。 您現在將會對齊右邊的連結，並據此重新加以設定樣式。

    ![尋找暫存器和記錄檔中連結](using-page-inspector-in-visual-studio-2012/_static/image15.png "尋找暫存器和記錄檔中的連結")

    *尋找暫存器和記錄檔中的連結*
2. 切換檢查模式中選取，按一下 [關閉] 以，但不是能在註冊連結以開啟其程式碼。

    請注意，連結的原始程式碼位於 **\_LoginPartial.cshtml**檔案，不 Index.cshtml 和\_Layout.cshtml，也就是您可能會在第一個位置中尋找的位置。 您在已直接在正確的來源檔案中放置。
3. 在**樣式**索引標籤上，找出並按一下**<section> #login</section>**是這些連結的 HTML 容器項目。

    請注意， **#login**樣式自動位於**Site.css**按一下之後。 此外，程式碼現在會反白。

    ![選取的 CSS 樣式](using-page-inspector-in-visual-studio-2012/_static/image16.png "選取的 CSS 樣式")

    *選取的 CSS 樣式*
4. 請取消註解**文字對齊**屬性反白顯示的程式碼中藉由移除開頭和結尾字元，並儲存**Site.css**檔案。

    Page Inspector 可感知撰寫目前的頁面上，所有不同檔案的而且它可以偵測到任何這些檔案的變更時。 它會提醒您目前的網頁瀏覽器中不會與原始程式檔同步時。
5. 在 Page Inspector 瀏覽器中，按一下下方重新載入該頁面的 [網址] 列的列。

    ![重新載入該頁面](using-page-inspector-in-visual-studio-2012/_static/image17.png)

    *重新載入該頁面*

    連結現在會在右側，但仍是它們看起來像是分項清單。 現在，您將會移除項目符號，水平對齊的連結。

    ![更新的頁面](using-page-inspector-in-visual-studio-2012/_static/image18.png)

    *更新的頁面*
6. 使用檢查模式中，選取任一**&lt;li&gt;**包含的項目&quot;註冊&quot;和&quot;登入&quot;連結。 然後，按一下  **&lt;區段&gt;#login**存取項目**Styles.css**程式碼。

    ![尋找樣式](using-page-inspector-in-visual-studio-2012/_static/image19.png "尋找樣式")

    *尋找樣式*
7. 在**Style.css**，請取消註解的程式碼**#login li**項目。 您要加入的樣式會隱藏項目符號，並以水平方式顯示的項目。

    ![右側的登入連結](using-page-inspector-in-visual-studio-2012/_static/image20.png "右側的登入連結")

    *右側的登入連結*
8. 儲存**Style.css**檔案，然後按一下下方重新載入該頁面的位址列的一次。 請注意，連結會正確出現。

    ![連結靠右對齊](using-page-inspector-in-visual-studio-2012/_static/image21.png "連結靠右對齊")

    *靠右對齊的連結*
9. 最後，您將變更標頭標題。 用於檢查模式按一下**您的標誌**文字和 get 會產生它的原始程式碼。
10. 現在您已在 **\_Layout.cshtml**，取代 '**您的標誌**'text 與'**相片圖庫**'。 儲存並更新 Page Inspector 瀏覽器。

    ![指派新的標題](using-page-inspector-in-visual-studio-2012/_static/image22.png "指派新的標題")

    *指派新的標題*

    ![PhotoGallerypage](using-page-inspector-in-visual-studio-2012/_static/image23.png)

    *相片圖庫頁面更新*
11. 最後，零件**PhotoGallery**專案並按**F5**執行應用程式。 簽出所有如預期般運作所做的變更。

* * *

<a id="Exercise2"></a>

<a id="Exercise_2_Using_Page_Inspector_in_WebForms_Projects"></a>
### <a name="exercise-2-using-page-inspector-in-webforms-projects"></a>練習 2： 在 WebForms 專案中使用 Page Inspector

在此練習中，您將學習如何預覽並使用 Page Inspector WebForms 方案進行偵錯。 您第一次將執行若要了解 Page Inspector 功能來協助偵錯處理序 Web 簡短圈周圍的工具。 然後，您會使用包含樣式問題的網頁。 您將學習如何使用 Page Inspector 尋找會產生問題的原始程式碼，並加以修正。

<a id="Ex2Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>工作 1-Page Inspector 瀏覽

在這項工作，您將學習如何在相片圖庫會顯示在 WebForms 專案內容中使用 Page Inspector 功能。

1. 開啟**開始**方案位於**來源/Ex2WebForms/開始/**資料夾。

   1. 您必須下載某些遺漏的 NuGet 套件才能繼續。 若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 封裝**。
   2. 在**管理 NuGet 封裝**] 對話方塊中，按一下 [**還原**才能下載遺漏的封裝。
   3. 最後，按一下 建置方案**建置** | **建置方案**。

      > [!NOTE]
      > 使用 NuGet 的優點之一是您不需要在專案中，所有的程式庫的出貨減少專案大小。 NuGet 的強大工具，請藉由指定封裝版本在 Packages.config 檔案中，您將會成功下載所有必要的程式庫第一次您執行專案。 這就是為什麼您必須從這個實驗室中開啟現有的方案後執行這些步驟。
2. 在 [方案總管] 中，找出**Default.aspx**頁面上，以滑鼠右鍵按一下它，然後選取**Page Inspector 中的檢視**。

    ![Page inspector 開啟 Default.aspx](using-page-inspector-in-visual-studio-2012/_static/image24.png "Page inspector 開啟 Default.aspx")

    *開啟 Default.aspx，Page inspector*
3. Page Inspector 視窗會顯示 Default.aspx。

    ![在 Page Inspector 中檢視 Default.aspx](using-page-inspector-in-visual-studio-2012/_static/image25.png "Page Inspector 中檢視 Default.aspx")

    *在 Page Inspector 中檢視 Default.aspx*

    Page Inspector 工具整合在 Visual Studio 環境中。 偵測器包含內嵌的瀏覽器，以及功能強大的 HTML 分析工具會顯示選取的程式碼。 請注意，您不需要執行此方案，以查看您的網頁的外觀。

    > [!NOTE]
    > Page Inspector 瀏覽器的寬度小於開啟頁面的寬度，當您不會看到頁面正確。 如果發生這種情況，調整 Page Inspector 的寬度。
4. 按一下**檔案**Page Inspector 中的索引標籤。

    您會看到正在撰寫呈現的預設網頁的所有原始程式檔。 這是一個實用的功能，來識別您一眼的所有項目，特別是當您正在使用使用者控制項和主版頁面。 請注意，您也可以導覽到每個檔案。

    ![[檔案] 索引標籤](using-page-inspector-in-visual-studio-2012/_static/image26.png "[檔案] 索引標籤")

    *[檔案] 索引標籤*
5. 按一下**切換檢查模式**位於索引標籤左邊的按鈕。

    此工具可讓您選取網頁的任何項目，並查看其 HTML 程式碼和.aspx 來源。

    ![切換檢查模式按鈕](using-page-inspector-in-visual-studio-2012/_static/image27.png "切換檢查模式按鈕")

    *切換檢查模式按鈕*
6. 在 Page Inspector 瀏覽器中，將滑鼠移頁面項目。 當您移動滑鼠指標移到所呈現頁面的任意處時，就會顯示項目類型和對應的來源標記或程式碼會反白顯示在 Visual Studio 編輯器中。

    ![在動作中檢查模式](using-page-inspector-in-visual-studio-2012/_static/image28.png "檢查模式作用中")

    *檢查模式作用中*

    > [!NOTE]
    > 不會最大化 Page Inspector 視窗或您無法看到預覽索引標籤顯示的原始程式碼。 如果它最大化時，您可以按一下 Page Inspector 中的項目，顯示選取項目的原始碼，但是它將會隱藏頁面偵測器視窗。

    如果您注意**Default.aspx**檔案中，您會注意到產生選取的項目之原始程式碼的一部分會反白顯示。 這項功能可協助長的原始程式檔，提供直接和快速的方式來存取這些程式碼的版本。

    ![檢查項目](using-page-inspector-in-visual-studio-2012/_static/image29.png "檢查項目")

    *檢查項目*
7. 按一下**切換檢查模式**按鈕 (![Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser。](using-page-inspector-in-visual-studio-2012/_static/image30.png "Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser。") )，位於 Page Inspector 的索引標籤中，若要停用資料指標。
8. 選取**HTML**索引標籤，顯示在 Page Inspector 瀏覽器中呈現的 HTML 程式碼。
9. 中的 HTML 程式碼中，找出 Koala 連結的清單項目並選取它。

    請注意，當您選取的程式碼時，對應的輸出會自動反白顯示的瀏覽器。 此功能十分有用，若要查看如何轉譯頁面上的 HTML 區塊。

    ![頁面中選取 HTML 項目](using-page-inspector-in-visual-studio-2012/_static/image31.png "頁面中選取 HTML 項目")

    *頁面中選取 HTML 項目*
10. 按一下**切換檢查模式**按鈕以啟用*檢查模式*按一下導覽列。 右邊的 [樣式] 窗格中的 HTML 程式碼，您會看到套用至選取的元素的 CSS 樣式清單。

    > [!NOTE]
    > 由於標頭是站台版面配置的一部分，Page Inspector 也會開啟 Site.Master 檔案，並反白顯示受影響的程式碼區段。

    ![DiscoveringstylesWebForms](using-page-inspector-in-visual-studio-2012/_static/image32.png "探索樣式和原始程式檔的選取項目")

    *探索樣式和原始程式檔的選取項目*
11. 切換檢查指標已啟用，移動滑鼠指標在功能表列下方，按一下空白的一半圓形。

    ![選取項目](using-page-inspector-in-visual-studio-2012/_static/image33.png "選取項目")

    *選取項目*
12. 在 [樣式] 窗格中，找出**背景影像**項目底下**.main 內容**群組。 **取消核取****背景影像**看。 您會注意到瀏覽器會立即反映變更，並隱藏圓形。

    > [!NOTE]
    > 您在頁面偵測器樣式 索引標籤套用的變更不會影響原始的樣式表。 您可以取消核取樣式，或變更其值依需求多次，但將會還原之後重新整理頁面。

    ![啟用和停用 CSS styles2](using-page-inspector-in-visual-studio-2012/_static/image34.png "啟用和停用的 CSS 樣式")

    *啟用和停用的 CSS 樣式*
13. 現在，按一下 '**您****的標誌在此'**上使用檢查模式的標頭的文字。
14. 在**樣式**索引標籤上，找出**字型大小**CSS 屬性下**.site 標題**群組。 按兩下以編輯其值的屬性一次。 值取代 2.3em **3em**，然後按 ENTER 鍵。 請注意，標題起來更大。

    ![變更頁面 Inspector2 中的 CSS 值](using-page-inspector-in-visual-studio-2012/_static/image35.png "Page Inspector 中的變更的 CSS 值")

    *Page Inspector 中變更的 CSS 值*
15. 按一下**追蹤樣式**索引標籤上，Page Inspector 的右窗格中。 這是以查看所有的樣式套用至選取範圍中，依屬性名稱的替代方法。

    ![所選項目的 CSS 樣式追蹤](using-page-inspector-in-visual-studio-2012/_static/image36.png "所選項目的 CSS 樣式追蹤")

    *CSS 樣式追蹤 選取的項目*
16. Page Inspector 的另一個功能是 [配置] 窗格。 使用檢查模式時，選取導覽列，然後按一下**配置**在右窗格中的索引標籤。 您會看到的選取項目，確切的大小，以及其位移、 邊界、 邊框距離及框線的大小。 請注意，您也可以修改此檢視中的值。

    ![Page Inspector 中的項目配置](using-page-inspector-in-visual-studio-2012/_static/image37.png "Page Inspector 中的項目配置")

    *Page Inspector 中的項目配置*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>工作 2-尋找和修正相片圖庫中的樣式問題

您要如何診斷與舊版 Visual Studio 網頁問題？ 也可能已經熟悉 web 偵錯在 Visual Studio IDE，像是 Internet Explorer 開發人員工具或 firebug 這類外部執行的工具。 瀏覽器只了解 HTML、 指令碼和樣式，而基礎架構會產生將呈現的 HTML。 因此，您通常需要部署整個站台，以查看網頁外觀如何。

當您想要偵測及修正問題，您的網站中，您可能必須遵循下列步驟：

1. 從 Visual Studio 中，執行此方案，或部署在 web 伺服器上的頁面。
2. 在瀏覽器中開啟您使用，或只要開啟原始碼和樣式，然後試著比對此問題的開發人員工具。 若要尋找相關檔案，您會使用&quot;搜尋&quot;或&quot;檔案中的搜尋&quot;的樣式類別名稱的功能。
3. 一旦偵測到錯誤時，停止 Web 瀏覽器和伺服器。
4. 清除瀏覽器快取。
5. 返回 Visual Studio 套用修正程式。 重複執行測試的所有步驟。

由於沒有真正 WYSIWYG 中 ASP.NET WebForms，一些樣式問題上偵測到後續階段中，執行或部署之後。 現在，Page inspector，便可預覽任何頁面而無須執行方案。

在這項工作，您將使用 Page inspector 修正相片圖庫應用程式的一些問題。 在下列步驟中，您將會偵測並快速修正標頭中的一些簡單樣式問題。

1. 使用頁面檢查，找出**註冊**和**登入**標頭的左側的連結。

    請注意，連結不會顯示在右側預期的位置。 您現在將會對齊右邊的連結，並據以重新設定樣式。

    ![登入位於左側的連結](using-page-inspector-in-visual-studio-2012/_static/image38.png "登入位於左側的連結")

    *位在左側的登入連結*
2. 切換選取的檢查模式下，選取 登入連結以開啟其程式碼。

    請注意，連結來源程式碼位於**Site.Master**檔案，不是 Default.aspx 頁面中您可以查看在第一個位置，則您在直接在正確的來源檔案中放置。
3. 在**樣式**索引標籤上，找出並按一下**&lt;區段&gt;#login**是這些連結的 HTML 容器項目。

    請注意， **#login**樣式自動位於**Site.css**按一下之後。 此外，程式碼現在會反白。

    ![選取的 CSS 樣式](using-page-inspector-in-visual-studio-2012/_static/image39.png "選取的 CSS 樣式")

    *選取的 CSS 樣式*
4. 請取消註解**文字對齊**屬性反白顯示的程式碼中藉由移除開頭和結尾字元，並儲存**Site.css**檔案。

    Page Inspector 可感知撰寫目前的頁面上，所有不同檔案的而且它可以偵測到任何這些檔案的變更時。 它會提醒您目前的網頁瀏覽器中不會與原始程式檔同步時。
5. 在 Page Inspector 瀏覽器中，按一下位於 [網址] 列，以儲存變更並重新載入頁面下方的列。

    ![Reloadingthepage](using-page-inspector-in-visual-studio-2012/_static/image40.png)

    *重新載入該頁面*

    連結現在會在右側，但仍是它們看起來像是分項清單。 現在，您將會移除項目符號，水平對齊的連結。

    ![更新的頁面](using-page-inspector-in-visual-studio-2012/_static/image41.png)

    *更新的頁面*
6. 使用檢查模式中，選取任一**&lt;li&gt;**包含的項目&quot;註冊&quot;和&quot;登入&quot;連結。 然後，按一下  **&lt;區段&gt;#login**存取項目**Styles.css**程式碼。

    ![尋找樣式](using-page-inspector-in-visual-studio-2012/_static/image42.png "尋找樣式")

    *尋找樣式*
7. 在**Style.css**，請取消註解的程式碼**#login li**項目。 您要加入的樣式會隱藏項目符號，並以水平方式顯示的項目。

    ![右側的登入連結](using-page-inspector-in-visual-studio-2012/_static/image43.png "右側的登入連結")

    *右側的登入連結*
8. 儲存**Style.css**檔案，然後按一下下方重新載入該頁面的位址列的一次。 請注意，連結會正確出現。

    ![連結靠右對齊](using-page-inspector-in-visual-studio-2012/_static/image44.png "連結靠右對齊")

    *靠右對齊的連結*
9. 最後，您將變更標頭標題。 而不是搜尋 '**您的標誌'**文字中的所有檔案，使用檢查模式按一下文字，並取得產生它的原始碼。

    ![尋找網站標題](using-page-inspector-in-visual-studio-2012/_static/image45.png "尋找網站標題")

    *尋找網站標題*
10. 現在您已在**Site.Master**，取代 '**您的標誌**'text 與'**相片圖庫**'。 儲存並更新 Page Inspector 瀏覽器。

    ![相片圖庫頁面更新](using-page-inspector-in-visual-studio-2012/_static/image46.png "相片圖庫頁面更新")

    *相片圖庫頁面更新*
11. 最後按**F5**執行應用程式的簽出所有如預期般運作所做的變更。

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>總結

藉由完成這個實際操作實驗室，您已學會如何使用 Page Inspector 預覽您的 Web 應用程式而不需要重新建置並在瀏覽器中執行的網站。 此外，您已學會如何快速找出並修正 bug，藉由從轉譯的輸出直接存取原始碼。

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>附錄 a： 安裝 Visual Studio Express 2012 for Web

您可以安裝**Microsoft Visual Studio Express 2012 for Web**或另一個&quot;Express&quot;版本使用**[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. 下列指示將引導您逐步完成安裝所需*Visual studio Express 2012 for Web*使用*Microsoft Web Platform Installer*。

1. 移至[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169)。 或者，如果您已安裝 Web Platform Installer，您可以開啟它，並搜尋產品&quot; <em>Visual Studio Express 2012 for Web 與 Windows Azure SDK</em>&quot;。
2. 按一下**立即安裝**。 如果您不需要**Web Platform Installer**您會重新導向至下載並安裝第一次。
3. 一次**Web Platform Installer**開啟時，按一下 **安裝**，啟動安裝程式。

    ![安裝 Visual Studio Express](using-page-inspector-in-visual-studio-2012/_static/image47.png "安裝 Visual Studio Express")

    *安裝 Visual Studio Express*
4. 閱讀所有產品的授權和詞彙，然後按一下**我接受**才能繼續。

    ![接受授權條款](using-page-inspector-in-visual-studio-2012/_static/image48.png)

    *接受授權條款*
5. 等待直到完成下載和安裝程序。

    ![安裝進度](using-page-inspector-in-visual-studio-2012/_static/image49.png)

    *安裝進度*
6. 當安裝完成時，按一下**完成**。

    ![安裝已完成](using-page-inspector-in-visual-studio-2012/_static/image50.png)

    *安裝已完成*
7. 按一下**結束**關閉 Web Platform Installer。
8. 若要開啟 Visual Studio Express for Web，請移至**啟動**畫面上，並開始書寫&quot; **VS Express**&quot;，然後按一下  **VS Express for Web**並排顯示。

    ![VS Express for Web 方塊](using-page-inspector-in-visual-studio-2012/_static/image51.png)

    *VS Express for Web 方塊*
