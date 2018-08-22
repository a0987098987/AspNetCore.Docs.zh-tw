---
uid: mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
title: ASP.NET MVC 4 中最新消息 |Microsoft Docs
author: rick-anderson
description: ASP.NET MVC 4 是用來建置可調整、 以標準為基礎的 web 應用程式，使用堅實的設計模式，以及使用 ASP.NET 的強大的架構和...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 48f7feb3-872f-485d-b96f-e30011ff8c4a
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: b1d80928d765bc71ea1579272662b6697371c47b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830559"
---
# <a name="whats-new-in-aspnet-mvc-4"></a>ASP.NET MVC 4 中最新消息

藉由[Web Camp 小組](https://twitter.com/webcamps)

[下載 Web 研討會訓練套件](https://aka.ms/webcamps-training-kit)

ASP.NET MVC 4 是建置可調整、 以標準為基礎的 web 應用程式使用完善的設計模式，以及 ASP.NET 和.NET framework 的強大功能的架構。 這個新、 第四個版本的 framework，著重於更輕鬆進行行動 web 應用程式開發。

一開始，當您建立新的 ASP.NET MVC 4 專案目前沒有可用來建置一個獨立應用程式，專為行動裝置的行動應用程式專案範本。 此外，ASP.NET MVC 4 與 jQuery Mobile jQuery.Mobile.MVC NuGet 套件透過整合。 jQuery Mobile 是以 HTML5 為基礎的架構開發 web 應用程式相容於所有熱門的行動裝置平台，包括 Windows Phone、 iPhone、 Android 等等。 不過，如果您需要特製化時，ASP.NET MVC 4 也可讓您提供不同裝置的不同檢視，並提供裝置特定的最佳化。

在這個實際操作實驗室中，您將開始使用 ASP.NET MVC 4&quot;網際網路應用程式&quot;建立相片圖庫的應用程式的專案範本。 您會以漸進方式來加強使用 jQuery Mobile 和 ASP.NET MVC 4 的新功能，讓它與不同的行動裝置和桌面的網頁瀏覽器相容的應用程式。 您也將了解程式碼產生和 ASP.NET MVC 4 如何讓您更輕鬆地撰寫非同步動作方法所支援工作的新程式碼配方&lt;ActionResult&gt;傳回型別。

> [!NOTE]
> 所有的範例程式碼和程式碼片段會包含在 Web 研討會訓練套件，可在[Microsoft-Web/WebCampTrainingKit 版本](https://aka.ms/webcamps-training-kit)。 這個實驗室中的特定專案將會位於[What's New in ASP.NET 4.5 Web Form](https://github.com/Microsoft-Web/HOL-ASPNETWebForms)。

<a id="Objectives"></a>
### <a name="objectives"></a>目標

在這個實際操作實驗室中，您將了解如何：

- 利用 ASP.NET MVC 專案範本包括新的行動應用程式專案範本的增強功能
- 使用 HTML5 檢視區屬性和 CSS 媒體查詢，以改善在行動裝置上顯示
- 使用 jQuery Mobile，漸進式增強功能，以及建置觸控最佳化的 web UI
- 建立行動專用的檢視
- 行動和桌面應用程式中的檢視之間切換時，用以檢視切換器元件
- 建立使用工作支援非同步控制器

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>必要條件

您必須具備下列項目，即可完成此實驗室：

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)或更好 (讀取[附錄 B](#AppendixB)如需有關如何安裝它)。
- [ASP.NET MVC 4](../../../mvc4.md) （隨附於 Microsoft Visual Studio 2012 安裝）
- Windows Phone 模擬器 (納入[7.1.1 的 Windows Phone SDK](https://www.microsoft.com/download/details.aspx?id=29233))
- 選用- [WebMatrix 2](https://www.microsoft.com/web/webmatrix/)具有**Electric Plum iPhone 模擬器**延伸模組 （僅適用於用來瀏覽 web 應用程式，與 iPhone 模擬器的練習 3)

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>安裝程式

整個實驗室文件中，您將會指示要插入程式碼區塊。 為了方便起見，大部分的程式碼提供 Visual Studio 程式碼片段，可以從 Visual Studio 內使用並避免以手動方式新增。

若要安裝的程式碼片段：

1. 開啟 Windows 檔案總管 視窗並瀏覽至實驗室**Source\Setup**資料夾。
2. 按兩下**Setup.cmd**這個資料夾，以安裝 Visual Studio 程式碼片段中的檔案。

如果您不熟悉 Visual Studio 程式碼片段，而且想要了解如何使用它們，您可以從這份文件參考附錄&quot;[附錄 a： 使用程式碼片段](#AppendixA)&quot;。

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>練習

這個實際操作實驗室還包含下列練習：

1. [新的 ASP.NET MVC 4 專案範本](#Exercise1)
2. [建立相片圖庫 Web 應用程式](#Exercise2)
3. [新增行動裝置的支援](#Exercise3)
4. [使用非同步控制器](#Exercise4)

> [!NOTE]
> 每個練習會伴隨**結束**包含完成練習之後，您應該取得所產生的方案資料夾。 如果您需要的所有練習所使用的其他說明，您可以使用此解決方案作為指南。


完成這個實驗室估計時間： **60 分鐘**。

<a id="Exercise1"></a>

<a id="Exercise_1_New_ASPNET_MVC_4_Project_Templates"></a>
### <a name="exercise-1-new-aspnet-mvc-4-project-templates"></a>練習 1： 新的 ASP.NET MVC 4 專案範本

在此練習中，您將探索 ASP.NET MVC 4 專案範本中的增強功能。 除了網際網路應用程式範本中，MVC 3 中已有此版本現在包含行動裝置應用程式的另一個範本。 首先，您將查看每個範本的一些相關功能。 然後，您將使用在使用正確的方式呈現您在不同的平台上正確的頁面。

<a id="Task_1_-_Exploring_the_Internet_Application_Template"></a>
#### <a name="task-1---exploring-the-internet-application-template"></a>工作 1-瀏覽網際網路應用程式範本

1. 開啟**Visual Studio**。
2. 選取**檔案 |新 |專案**功能表命令。 在 **新的專案**對話方塊中，選取**Visual C# |Web**範本，在左窗格中樹狀結構，然後選擇  **ASP.NET MVC 4 Web 應用程式。** 將專案命名為**PhotoGallery**、 選取的位置 （或保留預設值），按一下 **確定**。

    > [!NOTE]
    > 您稍後將會自訂您要現在建立 PhotoGallery ASP.NET MVC 4 解決方案。

    ![建立新的專案](whats-new-in-aspnet-mvc-4/_static/image1.png "建立新的專案")

    *建立新的專案*
3. 在 **新的 ASP.NET MVC 4 專案**對話方塊中，選取**網際網路應用程式**專案範本，然後按一下**確定**。 請確定您已選取 Razor 檢視引擎。

    ![建立新 ASP.NET MVC 4 網際網路應用程式](whats-new-in-aspnet-mvc-4/_static/image2.png "建立新 ASP.NET MVC 4 網際網路應用程式")

    *建立新 ASP.NET MVC 4 網際網路應用程式*

    > [!NOTE]
    > ASP.NET MVC 3 中已引進 razor 語法。 其目標是要減少字元和啟用快速和流暢的程式碼撰寫工作流程在檔案中，所需按鍵的數目。 Razor 會利用現有的 C# / VB （或其他） 的語言能力，並提供可讓一個很棒的 HTML 建構工作流程的範本標記語法。
4. 按下**F5**以執行此方案，並查看更新的範本。 您可以查看下列功能：

    **現代化樣式範本**

    範本已經更新，提供更多的現代化外觀的樣式。

    ![重新設定樣式的 ASP.NET MVC 4 範本](whats-new-in-aspnet-mvc-4/_static/image3.png "抵擋範本的 MVC 4")

    *重新設定樣式的 ASP.NET MVC 4 範本*

    ![新的 Contact 頁面](whats-new-in-aspnet-mvc-4/_static/image4.png "新連絡人 頁面")

    *新的 [連絡人] 頁面*

    **自適性轉譯**

    請參閱調整瀏覽器視窗的大小，並注意如何頁面配置動態調整為新的視窗大小。 這些範本會使用適應性呈現技巧，來正確轉譯，而不需要任何自訂的桌上型電腦和行動裝置平台。

    ![ASP.NET MVC 4 專案範本，以不同的瀏覽器尺寸](whats-new-in-aspnet-mvc-4/_static/image5.png "ASP.NET MVC 4 專案範本，在不同的瀏覽器大小")

    *ASP.NET MVC 4 專案範本，在不同的瀏覽器大小*

    **使用 JavaScript 更豐富的 UI**

    預設專案範本的其他增強功能是使用 JavaScript 來提供更具互動性的 JavaScript。 範本中使用的登入和註冊連結提供如何使用 jQuery 驗證來驗證用戶端的輸入的欄位。

    ![jQuery 驗證](whats-new-in-aspnet-mvc-4/_static/image6.png)

    *jQuery 驗證*

    > [!NOTE]
    > 請注意這兩個登入中的第一個區段的區段，您可以登入從站台的註冊帳戶並在第二個區段可以 altenativelly 登入使用另一個驗證服務，例如 google （預設為停用）。
5. 關閉瀏覽器以停止偵錯工具，並返回 Visual Studio。
6. 開啟檔案**AuthConfig.cs**位於**應用程式\_啟動**資料夾。
7. 移除註冊的 Google 用戶端的最後一行的註解*OAuth*驗證。

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample1.cs)]

    > [!NOTE]
    > 請注意，您可以輕鬆地啟用使用任何 OpenID 或 OAuth 的服務，例如 Facebook、 Twitter、 Microsoft 等的驗證。
8. 按下**F5**執行方案，並瀏覽到 登入頁面。
9. 選取  **Google**服務以登入。

    ![在服務中選取記錄檔](whats-new-in-aspnet-mvc-4/_static/image7.png)

    *在服務中選取記錄檔*
10. 使用您的 Google 帳戶登入。
11. 允許 Google 帳戶中擷取資訊的站台 (localhost)。
12. 最後，您必須在站台建立關聯的 Google 帳戶中註冊。

   ![將您的 Google 帳戶產生關聯](whats-new-in-aspnet-mvc-4/_static/image8.png)

   *將您的 Google 帳戶產生關聯*
13. 關閉瀏覽器以停止偵錯工具，並返回 Visual Studio。
14. 立即探索方案，以查看由 ASP.NET MVC 4 專案範本中有些其他新功能。

   ![ASP.NET MVC 4 的網際網路應用程式專案範本](whats-new-in-aspnet-mvc-4/_static/image9.png "ASP.NET MVC 4 的網際網路應用程式專案範本")

   *ASP.NET MVC 4 的網際網路應用程式專案範本*

   - **HTML 5 標記**

       瀏覽以找出新的佈景主題標記的範本檢視。

       ![新的範本，使用 Razor 和 HTML5 標記 About.cshtml。](whats-new-in-aspnet-mvc-4/_static/image10.png "使用 Razor 和 HTML5 標記 About.cshtml 的新範本。")

       *新的範本，使用 Razor 和 HTML5 的標記 (About.cshtml)。*
   - **更新的 JavaScript 程式庫**

       ASP.NET MVC 4 預設範本現在包含 KnockoutJS，可讓您建立豐富的 JavaScript MVVM 架構和靈敏的 web 應用程式使用 JavaScript 和 HTML。 例如在 MVC3，jQuery 和 jQuery UI 程式庫也包含 ASP.NET MVC 4 中。

     > [!NOTE]
     > 您可以取得 KnockOutJS 程式庫，在下列連結的詳細資訊： [ [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/) ](http://learn.knockoutjs.com/)。 此外，您可以了解 jQuery 和 jQuery UI 中[ [ http://docs.jquery.com/ ](http://docs.jquery.com/) ](http://docs.jquery.com/)。

<a id="Task_2_-_Exploring_the_Mobile_Application_Template"></a>
#### <a name="task-2---exploring-the-mobile-application-template"></a>工作 2-瀏覽行動裝置應用程式範本

ASP.NET MVC 4 可加速開發行動裝置的網站和平板電腦瀏覽器。 此範本有相同的應用程式結構，為網際網路應用程式範本 （請注意，控制器程式碼是幾乎完全相同），但其樣式已修改為觸控式行動裝置中正確轉譯。

1. 選取**檔案 |新 |專案**功能表命令。 在 **新的專案**對話方塊中，選取**Visual C# |Web**範本，在左窗格中樹狀結構，然後選擇  **ASP.NET MVC 4 Web 應用程式。** 將專案命名為**PhotoGallery.Mobile**、 選取的位置 （或保留預設值），選取&quot;加入至方案&quot;，按一下 **確定**。
2. 在 **新的 ASP.NET MVC 4 專案**對話方塊中，選取**行動裝置應用程式**專案範本，然後按一下**確定**。 請確定您已選取 Razor 檢視引擎。

    ![建立新 ASP.NET MVC 4 行動應用程式](whats-new-in-aspnet-mvc-4/_static/image11.png "建立新 ASP.NET MVC 4 行動應用程式")

    *建立新 ASP.NET MVC 4 行動應用程式*
3. 現在，您便能夠探索解決方案，以及查看一些 ASP.NET MVC 4 的解決方案範本，適用於行動裝置所引進的新功能：

    - **jQuery Mobile 程式庫**

        行動裝置應用程式專案範本包含 jQuery Mobile 文件庫，也就是行動瀏覽器相容性的開放原始碼程式庫。 jQuery Mobile 適用於行動瀏覽器支援 CSS 和 JavaScript 的漸進式增強功能。 漸進式增強功能可讓所有的瀏覽器，以顯示網頁的基本內容，而這只能啟用最強大的瀏覽器顯示豐富的內容。 JavaScript 和 CSS 檔案，納入 jQuery 行動樣式，說明要放入螢幕的內容，而不進行任何變更，在網頁標記中的行動瀏覽器。

        ![jQuery-mobile-library-included-in-the-template](whats-new-in-aspnet-mvc-4/_static/image12.png)

        *範本中所包含的 jQuery mobile 文件庫*
    - **HTML5 為基礎的標記**

        ![Mobile-application-template-using-HTML5-markup](whats-new-in-aspnet-mvc-4/_static/image13.png)

        *使用 HTML5 標記、 （Login.cshtml 和 index.cshtml） 的行動應用程式範本*
4. 按下**F5**執行方案。
5. 開啟**Windows Phone 7 模擬器**。
6. 在手機的開始畫面中，開啟 Internet Explorer。 簽出的桌面應用程式的啟動位置的 URL，並從行動電話瀏覽至該 URL (例如`http://localhost:[PortNumber]/`)。
7. 現在您可以輸入的登入頁面，或請參閱 about 頁面。 請注意，網站的樣式會根據新的 Metro 應用程式，適用於行動裝置。 ASP.NET MVC 4 專案範本會正確顯示行動裝置上，確定頁面上的所有項目會顯示並啟用。 請注意，標頭上的連結不夠大，按一下或點選。

    ![專案中的行動裝置的範本頁面](whats-new-in-aspnet-mvc-4/_static/image14.png "專案範本頁面，在行動裝置")

    *行動裝置中的專案範本頁面*
8. 也會使用新的範本**檢視區的中繼標籤**。 大部分的行動瀏覽器定義虛擬瀏覽器視窗的寬度或&quot;viewport&quot;，即大於行動裝置的實際寬度。 這可讓行動瀏覽器顯示整個 web 網頁內虛擬顯示。 **檢視區的中繼標籤**可讓 web 開發人員在行動裝置上設定的寬度、 高度和小數位數的瀏覽器區域 **。** 行動應用程式的 ASP.NET MVC 4 範本會將檢視區設定為裝置寬度 (&quot;寬度 = 裝置寬度&quot;) 中的版面配置範本 (*Views\Shared\_Layout.cshtml*)，因此所有頁面會有其設定為裝置螢幕寬度的檢視區。 請注意，將檢視區的中繼標籤不會變更預設瀏覽器檢視。
9. 開啟 **\_Layout.cshtml**位於**檢視 |共用**資料夾，然後將檢視區的中繼標籤的註解。 執行應用程式中，如果不是已開啟，並查看差異。


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample2.cshtml)]

![註解將檢視區的中繼標籤之後的站台](whats-new-in-aspnet-mvc-4/_static/image15.png "註化檢視區中繼標籤之後的站台")

*站台之後註解的檢視區的中繼標記*
10. 在 Visual Studio 中按**SHIFT** + **F5**停止偵錯應用程式。
11. 取消註解將檢視區的中繼標籤。


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample3.cshtml)]

<a id="Task_3_-_Using_Adaptive_Rendering"></a>
#### <a name="task-3---using-adaptive-rendering"></a>工作 3-使用適應性呈現

在這個工作中，您將學習另一種方法，而不需要任何自訂，同時呈現在行動裝置和網頁瀏覽器上正確的網頁。 您已使用類似的目的與檢視區的中繼標籤。 現在您將會符合另一個功能強大的方法：*適應性呈現*。

適應性呈現是使用一種技術**CSS3 媒體查詢**自訂樣式套用至頁面。 媒體查詢定義群組在特定條件下的 CSS 樣式的樣式表內的條件。 只有當條件為 true，則會將樣式套用至宣告的物件。

適應性呈現技術所提供的彈性可讓不同裝置上顯示站台的任何自訂。 您可以定義多個您要在單一樣式表上不需要撰寫邏輯程式碼選擇樣式的樣式。 因此，它是非常簡潔的方式，進行調整頁面的樣式，因為它會減少重複的程式碼和邏輯的轉譯用途的數量。 相反地，頻寬耗用量會增加，因為您的 CSS 檔案的大小可能會稍微變。

您的網站將會使用適應性呈現技術，**正常地顯示，不論瀏覽器。** 不過，您應該考慮是否有額外的頻寬載入是一項考量。

> [!NOTE]
> 媒體查詢的基本格式為： @media \[範圍： 所有 | 掌上型 | 列印 | 投影 | 畫面\]([屬性： 值] 和...[屬性： value])


媒體查詢的範例： &gt;  <strong>@media所有和 (最大寬度： 1000px) 和 (最小寬度： 700px) {}:</strong> 700px 和 1000px 之間的所有解析度。

> <strong>@media 螢幕和 (最小寬度： 400px) 和 (最大寬度： 700px) {...}:</strong>只針對螢幕。 解決方法必須是介於 400 到 700px。
> 
> <strong>@media 掌上型和 (最小寬度： 20em)，畫面和 (最小寬度： 20em) {...}:</strong>掌上型 （行動裝置和裝置） 和畫面。 最小寬度必須大於 20em。
> 
> 您可以找到相關的詳細資訊，在[W3C 網站](http://www.w3.org/TR/css3-mediaqueries/)。


您現在將探索適應性呈現的運作方式，改善可讀性的 ASP.NET MVC 4 預設的網站範本。

1. 開啟**PhotoGallery.sln**方案，您已在工作 1 中建立，並選取**PhotoGallery**專案。 按下**F5**執行方案。
2. 調整瀏覽器的寬度，設定 windows，一半或小於其原始大小的四分之一。 請注意，標頭中的項目會發生什麼事： 某些項目不會出現在標頭的可見區域。
3. 開啟<strong>Site.css</strong>檔案，從 Visual Studio 方案總管 中，位於<strong>內容</strong>專案資料夾。 按下<strong>CTRL + F</strong>以開啟 Visual Studio 整合式的搜尋，並撰寫<strong>@media</strong>找出<strong>CSS 媒體查詢</strong>。

    此範本中定義的媒體查詢條件適用於這種方式： 當瀏覽器視窗大小低於**850 px**，套用的 CSS 規則是定義在這個媒體區塊中的項目。

    ![尋找媒體查詢](whats-new-in-aspnet-mvc-4/_static/image16.png "尋找媒體查詢")

    *尋找媒體查詢*
4. 850 中設定的最大寬度屬性值取代 px **10px**，以停用自動調整的轉譯，然後按**CTRL + S**以儲存變更。 回到瀏覽器，並按下**CTRL + F5**重新整理該頁面與您所做的變更。 請注意這兩個頁面中的差異，當調整視窗的寬度。

    ![套用頁面左側@media樣式，請在右邊的樣式省略](whats-new-in-aspnet-mvc-4/_static/image17.png "方，頁面會將套用@media樣式，請在右邊的樣式省略")

    <em>在左邊，將套用頁面@media省略了樣式，請在右側，樣式</em>

    現在，讓我們查看行動裝置上發生的狀況：

    ![套用頁面左側@media樣式，請在右邊的樣式省略](whats-new-in-aspnet-mvc-4/_static/image18.png "方，頁面會將套用@media樣式，請在右邊的樣式省略")

    <em>在左邊，將套用頁面@media省略了樣式，請在右側，樣式</em>

    雖然您會注意到，在網頁瀏覽器中呈現的頁面時，會變更時，不非常重要的是，使用行動裝置的差異變得更明顯。 左邊的映像，我們可以看到自訂樣式改善可讀性。

    適應性呈現可以用於許多案例，讓您更輕鬆地套用條件式網站設定樣式和解決一般樣式問題很棒的方法。

    檢視區 meta 標記和 CSS 媒體查詢特有不 ASP.NET MVC 4 中，因此您可以利用這些功能的任何 web 應用程式中。
5. 在 Visual Studio 中按**SHIFT** + **F5**停止偵錯應用程式。

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_the_Photo_Gallery_Web_Application"></a>
### <a name="exercise-2-creating-the-photo-gallery-web-application"></a>練習 2： 建立相片圖庫 Web 應用程式

在此練習中，您將使用的相片圖庫應用程式，以顯示相片。 ASP.NET MVC 4 專案範本中，將會開始，然後您會在其中加入從服務擷取相片，並在 [首頁] 頁面中顯示的功能。

在下列練習中，您將會更新此解決方案，強化行動裝置的裝置顯示的方式。

<a id="Task_1_-_Creating_a_Mock_Photo_Service"></a>
#### <a name="task-1---creating-a-mock-photo-service"></a>工作 1-建立模擬 （mock） 的相片服務

在這個工作中，您將建立 mock 相片要擷取的服務將會顯示在資源庫的內容。 若要這樣做，您將加入新的控制站，只會傳回具有資料的每張相片的 JSON 檔案。

1. 開啟**Visual Studio**如果尚未開啟。
2. 選取**檔案 |新 |專案**功能表命令。 在 **新的專案**對話方塊中，選取**Visual C# |Web**範本，在左窗格中樹狀結構，然後選擇  **ASP.NET MVC 4 Web 應用程式。** 將專案命名為**PhotoGallery**、 選取的位置 （或保留預設值），按一下 **確定**。 或者，您可以繼續使用您現有的 ASP.NET MVC 4**網際網路應用程式**解決方案**練習 1**並略過下一個步驟。
3. 在 **新的 ASP.NET MVC 4 專案**對話方塊中，選取**網際網路應用程式**專案範本，然後按一下**確定**。 請確定您具有 Razor 選為檢視引擎。
4. 在 [**方案總管] 中**，以滑鼠右鍵按一下**應用程式\_資料**資料夾，您的專案，然後選取**新增 |現有的項目**。 瀏覽至**Source\Assets\App\_資料**本實驗室的資料夾，並新增**Photos.json**檔案。
5. 建立新的控制器名稱**PhotoController**。 若要這樣做，以滑鼠右鍵按一下**控制器**資料夾中，移至**新增**，然後選取**控制站。** 完成 控制器名稱，請將保留**空白 MVC 控制器**範本，然後按一下**新增**。

    ![新增 PhotoController](whats-new-in-aspnet-mvc-4/_static/image19.png "新增 PhotoController")

    *新增 PhotoController*
6. 取代**Index**方法取代為下列**資源庫**動作，以及最近加入至專案的 JSON 檔案中的傳回內容。

    (程式碼片段- *ASP.NET MVC 4 實驗室-Ex02-組件庫動作*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample4.cs)]
7. 按下**F5**執行方案，並然後若要測試的模擬 （mock） 的相片服務瀏覽至下列 URL: `http://localhost:[port]/photo/gallery` （取決於目前的連接埠已在其中啟動應用程式的 [連接埠] 值）。 此 URL 的要求時，應擷取的內容**Photos.json**檔案。

    ![測試模擬 （mock） 的相片服務](whats-new-in-aspnet-mvc-4/_static/image20.png "測試模擬 （mock） 的相片服務")

    *測試模擬 （mock） 的相片服務*

在實際的實作，您可以使用[ASP.NET Web API](../../../../web-api/index.md)實作相片圖庫 服務。 ASP.NET Web API 是一種架構，可讓您更輕鬆建置 HTTP 服務並擴及各種用戶端，包括瀏覽器和行動裝置。 ASP.NET Web 應用程式開發介面是在 .NET Framework 上建置 RESTful 應用程式的理想平台。

<a id="Task_2_-_Displaying_the_Photo_Gallery"></a>
#### <a name="task-2---displaying-the-photo-gallery"></a>工作 2-顯示相片圖庫

在這個工作中，您將會更新以顯示相片圖庫，使用您在本練習的第一項工作中建立的模擬 （mock） 的服務的 [首頁] 頁面。 您會將模型檔案，並更新 [資源庫] 檢視。

1. 在 Visual Studio 中按**SHIFT** + **F5**停止偵錯應用程式。
2. 建立**相片**類別中**模型**資料夾。 若要這樣做，以滑鼠右鍵按一下**模型**資料夾中，選取**新增**然後按一下**類別**。 然後，將名稱設定為**Photo.cs**然後按一下**新增**。
3. 新增下列成員來**相片**類別。

    (程式碼片段- *ASP.NET MVC 4 實驗室-Ex02-相片模型*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample5.cs)]
4. 從 **Controllers** 資料夾中，開啟 **HomeController.cs** 檔案。
5. 使用陳述式加入下列程式碼。

    (程式碼片段- *ASP.NET MVC 4 實驗室-Ex02-HomeController Using*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample6.cs)]
6. 更新**的索引**若要使用的動作**HttpClient**組件庫及擷取資料，然後使用**JavaScriptSerializer**將它還原序列化至檢視模型。

    (程式碼片段- *ASP.NET MVC 4 實驗室-Ex02-索引動作*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample7.cs)]
7. 開啟**Index.cshtml**下的檔案**Views\Home**資料夾，並將下列程式碼的所有內容。

    此程式碼從服務擷取的所有相片中執行都迴圈，並顯示到的未排序清單中。

    (程式碼片段- *ASP.NET MVC 4 實驗室-Ex02-相片清單*)

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample8.cshtml)]
8. 在 [**方案總管] 中**，以滑鼠右鍵按一下**內容**資料夾，您的專案，然後選取**新增 |現有的項目**。 瀏覽至**Source\Assets\Content**這個實驗室的資料夾，並新增**Site.css**檔案。 您必須確認其取代。 如果您有**Site.css**檔案開啟，您必須確認也重新載入檔案。
9. 開啟檔案總管，然後複製整個**相片**資料夾位於下面**Source\Assets**本實驗室的 方案總管 中的專案根資料夾的資料夾。
10. 執行應用程式。 現在，您應該會看到顯示相片圖庫中的 [首頁] 頁面。

    ![相片圖庫](whats-new-in-aspnet-mvc-4/_static/image21.png "相片圖庫")

    *相片圖庫*
11. 在 Visual Studio 中按**SHIFT** + **F5**停止偵錯應用程式。

<a id="Exercise3"></a>

<a id="Exercise_3_Adding_support_for_mobile_devices"></a>
### <a name="exercise-3-adding-support-for-mobile-devices"></a>練習 3： 新增行動裝置的支援

ASP.NET MVC 4 中的金鑰更新的其中一個是行動裝置開發的支援。 在此練習中，您將探索 ASP.NET MVC 4 行動裝置應用程式的新功能來擴充您在前一個練習中建立的 PhotoGallery 解決方案。

<a id="Task_1_-_Installing_jQuery_Mobile_in_an_ASPNET_MVC_4_Application"></a>
#### <a name="task-1---installing-jquery-mobile-in-an-aspnet-mvc-4-application"></a>工作 1-安裝 ASP.NET MVC 4 應用程式中的 jQuery Mobile

1. 開啟**開始**解決方案位於**來源/Ex3-MobileSupport/開始/** 資料夾。 否則，您可能會繼續使用**結束**方案取得完成前一個練習。

   1. 如果您開啟提供**開始**解決方案中，您必須下載某些缺少的 NuGet 封裝才能繼續。 若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 套件**。
   2. 在 [**管理 NuGet 套件**] 對話方塊中，按一下**還原**才能下載遺漏的套件。
   3. 最後，按一下 建置方案**建置** | **建置方案**。

      > [!NOTE]
      > 使用 NuGet 的優點之一是，您不需要寄送您的專案中的所有程式庫專案縮小。 NuGet Power tools，藉由指定封裝版本在 Packages.config 檔案中，您將能夠下載所有必要的程式庫第一次執行專案。 這就是為什麼您必須在您開啟現有的方案從這個實驗室之後，執行這些步驟。
2. 開啟**Package Manager Console**依序按一下**工具** &gt; **程式庫套件管理員** &gt; **套件管理員主控台**功能表選項。

    ![開啟 NuGet Package Manager Console](whats-new-in-aspnet-mvc-4/_static/image22.png "開啟 NuGet 套件管理員主控台")

    *開啟 NuGet 套件管理員主控台*
3. 在 Package Manager Console 執行下列命令以安裝**jQuery.Mobile.MVC**封裝。

    jQuery Mobile 是開放原始碼程式庫建置觸控最佳化的 web UI。 JQuery.Mobile.MVC NuGet 套件包含 ASP.NET MVC 4 應用程式中使用 jQuery Mobile 的協助程式。

    > [!NOTE]
    > 藉由執行下列命令，您將下載 jQuery.Mobile.MVC 程式庫從 Nuget。

    PM

    [!code-powershell[Main](whats-new-in-aspnet-mvc-4/samples/sample9.ps1)]

    此命令會安裝 jQuery Mobile 和一些協助程式檔案，包括下列：

    - **Views/Shared/\_Layout.Mobile.cshtml**： 是 jQuery mobile 的版面配置適用於較小的螢幕。 當網站收到要求時從行動瀏覽器時，它會取代原始的版面配置 (\_Layout.cshtml) 換成這一個。
    - 檢視切換器元件： 組成**Views/Shared/\_ViewSwitcher.cshtml**部分檢視和**ViewSwitcherController.cs**控制站。 此元件會顯示在行動瀏覽器，讓使用者可以切換到桌面版本的頁面上的連結。  
        ![行動裝置支援的相片圖庫專案](whats-new-in-aspnet-mvc-4/_static/image23.png "相片圖庫 專案的行動支援")

        *相片圖庫 專案的行動支援*
4. 註冊行動裝置的組合。 若要這樣做，請開啟**Global.asax.cs**檔案，並新增下面這一行。

    (程式碼片段- *ASP.NET MVC 4 實驗室-Ex03-註冊行動裝置組合*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample10.cs)]
5. 執行桌面的網頁瀏覽器應用程式。
6. 開啟**Windows Phone 7 模擬器，模擬器**位於 **[開始] 功能表 |所有程式 |Windows Phone SDK 7.1 |Windows Phone 模擬器。**
7. 在手機的開始畫面中，開啟 Internet Explorer。 查看 啟動應用程式的 URL，並瀏覽至該電話瀏覽器的 URL (例如`http://localhost:[PortNumber]/`)。

    您會發現您的應用程式會尋找在 Windows Phone 模擬器中，不同 jQuery.Mobile.MVC 已顯示針對行動裝置最佳化的檢視之專案中建立新的資產。

    請注意頂端的電話，顯示會切換成桌面檢視連結的訊息。 此外，  **\_Layout.Mobile.cshtml**由您已安裝的封裝所建立的版面配置在應用程式中，包括不同的版面配置。

    > [!NOTE]
    > 到目前為止，沒有任何連結回到行動檢視。 就會包含在較新版本。

    ![相片圖庫首頁上的行動裝置檢視](whats-new-in-aspnet-mvc-4/_static/image24.png "Photo Gallery 首頁上的行動檢視")

    *相片圖庫首頁上的行動檢視*
8. 在 Visual Studio 中按**SHIFT** + **F5**停止偵錯應用程式。

<a id="Task_2_-_Creating_Mobile_Views"></a>
#### <a name="task-2---creating-mobile-views"></a>工作 2-建立行動裝置檢視

在這個工作中，您將建立索引 檢視的行動裝置版與針對行動裝置中的較佳 appareance 調整的內容。

1. 複製**Views\Home\Index.cshtml**檢視，並將它貼到 建立複本、 重新命名新的檔案以**Index.Mobile.cshtml**。
2. 開啟新建立**Index.Mobile.cshtml**檢視，並取代現有&lt;ul&gt;標記，以下列程式碼。 如此一來，您將會更新&lt;ul&gt;使用來自 jQuery 行動的佈景主題的 jQuery 行動資料註解的標記。

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample11.html)]

    > [!NOTE] 
    > 
    > 請注意：
    > 
    > - **資料角色**屬性設為**listview**會呈現使用 listview 樣式的清單。
    > 
    > - **資料嵌入**屬性設為 true 將會顯示具有圓角的框線和邊界的清單。
    > 
    > - **資料篩選**屬性設為 **，則為 true**會產生一個搜尋方塊。
    > 
    > 您可以深入了解專案文件中的 jQuery Mobile 慣例： [[http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)
3. 按下**CTRL + S**以儲存變更。
4. 若要切換**Windows Phone 模擬器**和重新整理網站。 請注意新的外觀與風格的組件庫 清單中，為新的搜尋方塊位於頂端。 然後，在搜尋方塊中輸入文字 (例如**Tulips**) 才會在相片圖庫中開始搜尋。

    ![使用 listview 樣式已篩選的資源庫](whats-new-in-aspnet-mvc-4/_static/image25.png "使用 listview 樣式已篩選的組件庫")

    *使用 listview 樣式已篩選的組件庫*

    總結來說，您已用檢視 Mobilizer 配方來建立一份索引檢視，其中&quot;行動&quot;後置詞。 這個後置詞表示 ASP.NET MVC 4 從行動裝置產生的每個要求會使用這份索引。 此外，您已更新 [索引] 檢視，可以提升網站的外觀與風格，行動裝置中使用 jQuery Mobile 的行動裝置的版本。
5. 返回 Visual Studio 並開啟**Site.Mobile.css**位於**內容**資料夾。
6. 修正相片標題，讓它顯示在右側的映像的位置。 若要這樣做，請新增下列程式碼**Site.Mobile.css**檔案。

    CSS

    [!code-css[Main](whats-new-in-aspnet-mvc-4/samples/sample12.css)]
7. 按下**CTRL + S**以儲存變更。
8. 切換回**Windows Phone 模擬器**和重新整理網站。 請注意相片標題則適當定位現在。

    ![標題位於影像的右側](whats-new-in-aspnet-mvc-4/_static/image26.png "定位影像右側的標題")

    *標題位於右側的映像*

<a id="Task_3_-_jQuery_Mobile_Themes"></a>
#### <a name="task-3---jquery-mobile-themes"></a>工作 3-jQuery Mobile 佈景主題

每個配置和中的 jQuery Mobile 小工具的設計周圍的新物件導向 CSS 架構讓您能夠將完整整合的視覺化設計佈景主題套用至網站和應用程式。

jQuery Mobile 的預設佈景主題包含 5 個樣本指定字母 (a、 b、 c、 d、 e） 的快速參考。

在這個工作中，您將會更新行動裝置的版面配置，以使用不同的佈景主題，比預設值。

1. 切換回到 Visual Studio。
2. 開啟 **\_Layout.Mobile.cshtml**檔案位於**Views\Shared**。
3. 尋找 div 項目設定為資料角色&quot;頁面&quot;，並更新**資料佈景主題**來&quot;**電子**&quot;。

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample13.html)]
4. 按下**CTRL + S**以儲存變更。
5. 重新整理中的站台**Windows Phone 模擬器**並注意新的色彩配置。

    ![行動裝置使用不同的色彩配置的配置](whats-new-in-aspnet-mvc-4/_static/image27.png "行動配置使用不同的色彩配置")

    *行動裝置使用不同的色彩配置的配置*

<a id="Task_4_-_Using_the_View-Switcher_Component_and_the_Browser_Overriding_Features"></a>
#### <a name="task-4---using-the-view-switcher-component-and-the-browser-overriding-features"></a>工作 4-使用檢視切換器元件，並覆寫功能的瀏覽器

最適合行動裝置的網頁通常會加入其文字是像桌面檢視] 或 [完整的網站模式可讓使用者切換到桌面版本的頁面連結。 JQuery.Mobile.MVC 套件包含範例**檢視切換器**元件用於此用途 **\_Layout.Mobile.cshtml**檢視。

![若要切換至 [桌面] 檢視的連結](whats-new-in-aspnet-mvc-4/_static/image28.png "連結切換到桌面檢視")

*若要切換到桌面檢視連結*

檢視切換程式會使用新功能，稱為**瀏覽器覆寫**。 這項功能可讓您要求視為就像是來自它們實際上來自於不同瀏覽器 （使用者代理程式） 的應用程式。

在這個工作中，您將探索由 jQuery.Mobile.MVC 和新的瀏覽器覆寫 ASP.NET MVC 4 中的功能加入檢視切換器的範例實作。

1. 切換回到 Visual Studio。
2. 開啟 **\_Layout.Mobile.cshtml**檢視位於**Views\Shared**資料夾，請注意與部分檢視所參考的檢視切換器元件。

    ![行動裝置使用檢視切換器元件的版面配置](whats-new-in-aspnet-mvc-4/_static/image29.png "行動配置使用檢視切換器元件")

    *行動裝置使用檢視切換器元件的版面配置*
3. 開啟 **\_ViewSwitcher.cshtml**部分檢視。

    部分檢視會使用新的方法**ViewContext.HttpContext.GetOverriddenBrowser()** 判斷 web 要求的來源，並顯示對應的連結，以切換至 桌面 或 行動檢視。

    **GetOverridenBrowser**方法會傳回**HttpBrowserCapabilitiesBase**對應至目前設定為要求的使用者代理程式的執行個體 （實際或覆寫）。 您可以使用此值，取得屬性，例如**IsMobileDevice**。

    ![ViewSwitcher 部分檢視](whats-new-in-aspnet-mvc-4/_static/image30.png "ViewSwitcher 部分檢視")

    *ViewSwitcher 部分檢視*
4. 開啟**ViewSwitcherController.cs**類別位於**控制站**資料夾。 簽出該 SwitchView 動作會呼叫 ViewSwitcher 元件中的連結，並注意新的 HttpContext 方法。

    - **HttpContext.ClearOverridenBrowser()** 方法會移除目前要求的任何覆寫的使用者代理程式。
    - **HttpContext.SetOverridenBrowser()** 方法會覆寫要求使用指定的使用者代理程式的實際使用者代理程式值。  
        ![ViewSwitcher 控制器](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher 控制器")  
*ViewSwitcher 控制器*

        瀏覽器覆寫是核心功能的 ASP.NET MVC 4 中，即使您沒有安裝 jQuery.Mobile.MVC 封裝所也提供。 不過，這項功能會影響檢視、 配置和部分檢視，而不影響任何相依於 Request.Browser 物件的功能。

<a id="Task_5_-_Adding_the_View-Switcher_in_the_Desktop_View"></a>
#### <a name="task-5---adding-the-view-switcher-in-the-desktop-view"></a>工作 5-加入桌面檢視中的檢視切換器

在這個工作中，您將會更新桌面的版面配置，以包含檢視切換程式。 這可讓行動使用者返回行動檢視瀏覽桌面檢視時。

1. 重新整理中的站台**Windows Phone 模擬器**。
2. 按一下 **桌面檢視**頂端的 資源庫的連結。 請注意在桌面的檢視，讓您回到 [行動] 檢視中沒有任何檢視切換器。
3. 返回 Visual Studio 並開啟 **\_Layout.cshtml**檢視。
4. 尋找登入區段，並插入呼叫轉譯 **\_ViewSwitcher**下的部分檢視 **\_LogOnPartial**部分檢視。 然後按**CTRL + S**以儲存變更。

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample14.cshtml)]
5. 按下**CTRL + S**以儲存變更。
6. 重新整理頁面，在 Windows Phone 模擬器中的，然後按兩下 [放大] 畫面。 請注意，在 [首頁] 頁面現在會顯示**行動裝置檢視**從行動裝置會切換成桌面檢視的連結。

    ![檢視切換器在桌面檢視中呈現](whats-new-in-aspnet-mvc-4/_static/image32.png "在桌面檢視中呈現的檢視切換器")

    *在桌面檢視中呈現的檢視切換器*
7. 再次切換至 行動裝置檢視，然後瀏覽至<strong>關於</strong>網頁 (http://localhost[連接埠] / Home/有關)。 請注意，即使您沒有建立 About.Mobile.cshtml 檢視，[關於] 頁面會顯示使用行動配置 (\_Layout.Mobile.cshtml)。

    ![有關頁面](whats-new-in-aspnet-mvc-4/_static/image33.png "關於頁面")

    *關於頁面*
8. 最後，在桌面的網頁瀏覽器中開啟網站。 請注意，沒有任何先前的更新已受到影響桌面檢視。

    ![PhotoGallery 桌面檢視](whats-new-in-aspnet-mvc-4/_static/image34.png "PhotoGallery 桌面檢視")

    *PhotoGallery 桌面檢視*

<a id="Task_6_-_Creating_New_Display_Modes"></a>
#### <a name="task-6---creating-new-display-modes"></a>工作 6-建立新的顯示模式

新的顯示模式功能可讓應用程式選取視瀏覽器會將產生要求的檢視。 例如，如果桌面瀏覽器要求首頁上，應用程式會傳回**Views\Home\Index.cshtml**範本。 然後，如果在行動瀏覽器要求首頁上，應用程式會傳回**Views\Home\Index.mobile.cshtml**範本。

在這個工作中，您將建立自訂的版面配置，iPhone 裝置，您必須模擬要求從 iPhone 裝置。 若要這樣做，您可以使用任一 iPhone 模擬器/模擬器 (例如[Electric 所研發的行動電話模擬器](http://www.electricplum.com/)) 或修改使用者代理程式的附加元件的瀏覽器。 如何在 Safari 瀏覽器中設定的使用者代理字串來模擬在 iPhone 的指示，請參閱[如何讓假裝是 IE 的 Safari](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html)給 David Alison 的部落格。

**請注意，這項工作是選擇性的您可以繼續到整個實驗室，而不加以執行。**

1. 在 Visual Studio 中按**SHIFT** + **F5**停止偵錯應用程式。
2. 開啟**Global.asax.cs**並新增下列 using 陳述式。

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample15.cs)]
3. 將下列反白顯示的程式碼新增至應用程式\_Start 方法。

    (程式碼片段- *ASP.NET MVC 4 實驗室-Ex03-iPhone DisplayMode*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample16.cs)]

您已註冊的新**DefaultDisplayMode 名為&quot;iPhone&quot;**，在靜態**DisplayModeProvider.Instance.Modes**靜態清單，會比對每個傳入的要求。 如果連入要求包含字串&quot;iPhone&quot;，ASP.NET MVC 會尋找其名稱包含檢視&quot;iPhone&quot;後置詞。 0 的參數會指出特定的是新的模式;比方說，此檢視會比一般更明確&quot;.mobile&quot;符合要求，從行動裝置的規則。

此程式碼執行，iPhone 瀏覽器就會產生要求之後，將使用您的應用程式**Views\Shared\\_Layout.iPhone.cshtml**您將在接下來的步驟建立的配置。

> [!NOTE]
> 這種測試 iPhone 已簡化，供示範之用，而且可能無法正常運作的每個 iPhone 使用者代理字串 （如範例測試會區分大小寫） 的要求方法。

4. 建立一份 **\_Layout.Mobile.cshtml**中的檔案**Views\Shared**資料夾，並重新命名複製到&quot;  **\_Layout.iPhone.csthml**&quot;.
5. 開啟 **\_Layout.iPhone.csthml**您在上一個步驟中建立。
6. 資料角色屬性設定為尋找 div 項目**網頁**並變更**資料佈景主題**屬性設定為&quot; &quot;。


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample17.cshtml)]

現在您會在 ASP.NET MVC 4 應用程式中有 3 個版面配置：

1. **\_Layout.cshtml**： 用於桌面瀏覽器的預設版面配置。
2. **\_Layout.mobile.cshtml**： 適用於行動裝置所使用的預設配置。
3. **\_Layout.iPhone.cshtml**： 使用不同的色彩配置從區分的 iPhone 裝置特定配置\_Layout.mobile.cshtml。
7. 按下**F5**以執行應用程式，並瀏覽中的站台**Windows Phone 模擬器**。
8. 開啟**iPhone 模擬器**(請參閱[旓紵 C](#AppendixC)如需有關如何安裝和設定 iPhone 模擬器)，而且也瀏覽至站台。 請注意，每個電話使用特定的範本。

    ![Using-different-views-for-each-mobile-device2](whats-new-in-aspnet-mvc-4/_static/image35.png)

    *針對每個行動裝置使用不同的檢視*

<a id="Exercise4"></a>

<a id="Exercise_4_Using_Asynchronous_Controllers"></a>
### <a name="exercise-4-using-asynchronous-controllers"></a>練習 4： 使用非同步控制器

Microsoft.NET Framework 4.5 引進了新的語言功能，在 C# 和 Visual Basic.NET 程式設計中非同步提供新的基礎。 這個新的基礎架構可讓非同步程式設計，類似於-且大約一樣直接-同步程式設計。 現在您就可以使用 ASP.NET MVC 4 中撰寫非同步動作方法**AsyncController**類別。 您可以使用非同步動作方法的長時間執行，不受限於 CPU 的要求。 如此可避免在處理要求時執行工作的 Web 伺服器。 AsyncController 類別通常用於長時間執行的 Web 服務呼叫。

這個練習說明 ASP.NET MVC 4 中的非同步作業的基本概念。 如果您想更深入的了解，您可以查看下列文章： [[https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)

<a id="Task_1_-_Implementing_an_Asynchronous_Controller"></a>
#### <a name="task-1---implementing-an-asynchronous-controller"></a>工作 1-實作非同步控制器

1. 開啟**開始**解決方案位於**來源/Ex4-非同步處理/開始/** 資料夾。 否則，您可能會繼續使用**結束**方案取得完成前一個練習。

   1. 如果您開啟提供**開始**解決方案中，您必須下載某些缺少的 NuGet 封裝才能繼續。 若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 套件**。
   2. 在 [**管理 NuGet 套件**] 對話方塊中，按一下**還原**才能下載遺漏的套件。
   3. 最後，按一下 建置方案**建置** | **建置方案**。

      > [!NOTE]
      > 使用 NuGet 的優點之一是，您不需要寄送您的專案中的所有程式庫專案縮小。 NuGet Power tools，藉由指定封裝版本在 Packages.config 檔案中，您將能夠下載所有必要的程式庫第一次執行專案。 這就是為什麼您必須在您開啟現有的方案從這個實驗室之後，執行這些步驟。
2. 開啟**HomeController.cs**從類別**控制站**資料夾。
3. 新增下列 using 陳述式。

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample18.cs)]
4. 更新**HomeController**類別繼承自**AsyncController**。 衍生自 AsyncController 的控制站可讓 ASP.NET 處理非同步要求，並仍然可以服務同步動作方法。

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample19.cs)]
5. 新增**非同步**關鍵字來**Index**方法，並使其傳回型別**工作&lt;ActionResult&gt;**。

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample20.cs)]

    > [!NOTE]
    > **非同步**關鍵字是其中一個.NET Framework 4.5 提供新的關鍵字; 它會告訴編譯器設定，這個方法所包含的非同步程式碼。 A**任務**物件都代表可能在未來某個時間點在完成的非同步作業。
6. 取代**用戶端。GetAsync()** 呼叫，但完整的非同步版本使用 await 關鍵字，如下所示。

    (程式碼片段- *ASP.NET MVC 4 實驗室-Ex04-GetAsync*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample21.cs)]

    > [!NOTE]
    > 在舊版中，您使用**結果**屬性從**工作**物件封鎖執行緒，直到傳回 （同步處理版本） 的結果。
    > 
    > 新增**await**關鍵字會指示編譯器將以非同步方式等候工作傳回方法呼叫。 這表示，其餘的程式碼會執行為回呼在等候的方法完成之後，才。 另外要注意的是您不需要變更您的 try / catch 區塊，若要這麼做： 在背景或前景中發生的例外狀況將仍可找出不需要任何額外的工作，使用 framework 所提供的處理常式。
7. 變更繼續在非同步處理實作新的程式碼行取代成如下所示的程式碼

    (程式碼片段- *ASP.NET MVC 4 實驗室-Ex04-ReadAsStringAsync*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample22.cs)]
8. 執行應用程式。 您會注意到沒有重大變更，但您的程式碼將不會封鎖執行緒，以從執行緒集區進行更好的伺服器資源使用量，並改善效能。

    > [!NOTE]
    > 您可以深入了解在實驗室中的新非同步程式設計功能&quot;**在.NET 4.5 中以 C# 和 Visual Basic 進行非同步程式設計**&quot;包含在 Visual Studio 訓練套件。

<a id="Task_2_-_Handling_Time-Outs_with_Cancellation_Tokens"></a>
#### <a name="task-2---handling-time-outs-with-cancellation-tokens"></a>工作 2-使用取消語彙基元的處理逾時

傳回工作的執行個體的非同步動作方法也可支援逾時。 在這個工作中，您將會更新索引方法程式碼來處理逾時的案例，使用取消語彙基元。

1. 請返回 Visual Studio，然後按**SHIFT + F5**停止偵錯。
2. 新增下列 using 陳述式**HomeController.cs**檔案。

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample23.cs)]
3. 更新索引動作，以接收**CancellationToken**引數。

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample24.cs)]
4. 更新**GetAsync**呼叫傳遞取消語彙基元。

    (程式碼片段- *CancellationToken 與 ASP.NET MVC 4 實驗室-Ex04-SendAsync*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample25.cs)]
5. 裝飾*Index*方法**AsyncTimeout**屬性設為 500 毫秒和**HandleError**屬性設定為處理**TaskCanceledException**重新導向至**TimedOut**檢視。

    (程式碼片段- *ASP.NET MVC 4 實驗室-Ex04-屬性*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample26.cs)]
6. 開啟**PhotoController**類別和更新**資源庫**方法來延遲執行 1000年毫秒 （1 秒），以模擬長時間執行的工作。

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample27.cs)]
7. 開啟**Web.config**檔案，並新增下列項目，以啟用自訂錯誤。

    [!code-xml[Main](whats-new-in-aspnet-mvc-4/samples/sample28.xml)]
8. 建立新的檢視，在**Views\Shared**名為**TimedOut** ，並使用預設的版面配置。 在 [方案總管] 中，以滑鼠右鍵按一下**Views\Shared**資料夾，然後選取**新增 |檢視**。

    ![針對每個行動裝置使用不同的檢視](whats-new-in-aspnet-mvc-4/_static/image36.png "使用針對每個行動裝置的不同檢視")

    *針對每個行動裝置使用不同的檢視*
9. 更新**TimedOut**檢視內容，如下所示。

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample29.cshtml)]
10. 執行應用程式，並瀏覽至根 URL。 因為您已新增**Thread.Sleep** 1000年毫秒，您會取得所產生的逾時錯誤**AsyncTimeout**屬性，並攔截**HandleError**屬性。

    ![逾時例外狀況處理](whats-new-in-aspnet-mvc-4/_static/image37.png "逾時例外狀況處理")

    *逾時例外狀況處理*

> [!NOTE]
> 此外，您可以在其中部署此應用程式以 Windows Azure 網站的下列[附錄 d： 發佈 ASP.NET MVC 4 應用程式使用 Web Deploy](#AppendixD)。


<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>總結

在這個實際操作實驗室，您觀察到的一些新功能駐留在 ASP.NET MVC 4 中。 已經討論過下列概念：

- 利用 ASP.NET MVC 專案範本包括新的行動應用程式專案範本的增強功能
- 使用 HTML5 檢視區屬性和 CSS 媒體查詢，以改善在行動裝置上顯示
- 使用 jQuery Mobile，漸進式增強功能，以及建置觸控最佳化的 web UI
- 建立行動專用的檢視
- 行動和桌面應用程式中的檢視之間切換時，用以檢視切換器元件
- 建立使用工作支援非同步控制器

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>附錄 a： 使用程式碼片段

使用程式碼片段，您會有您需要在隨手可得的所有程式碼。 實驗室課程文件會告訴您完全時您可以使用它們，如下圖所示。

![若要將程式碼插入您的專案使用 Visual Studio 程式碼片段](whats-new-in-aspnet-mvc-4/_static/image38.png "程式碼插入您的專案使用 Visual Studio 程式碼片段")

*若要將程式碼插入您的專案使用 Visual Studio 程式碼片段*

***若要新增的程式碼片段，使用鍵盤 （僅限 C#)***

1. 將游標放在您想要插入的程式碼。
2. 開始輸入程式碼片段名稱 （不含空格或連字號）。
3. 觀看為 IntelliSense 會顯示相符的程式碼片段的名稱。
4. 選取正確的程式碼片段 （或直到選取整個程式碼片段名稱時，保留 輸入）。
5. 按下 Tab 鍵兩次的游標位置插入程式碼片段。

![開始鍵入程式碼片段名稱](whats-new-in-aspnet-mvc-4/_static/image39.png "開始輸入程式碼片段名稱")

*開始鍵入程式碼片段名稱*

![按 Tab 鍵以選取反白顯示的程式碼片段](whats-new-in-aspnet-mvc-4/_static/image40.png "按 Tab 鍵以選取反白顯示的程式碼片段")

*按 Tab 鍵以選取反白顯示的程式碼片段*

![再次按 Tab 鍵和程式碼片段會依序展開](whats-new-in-aspnet-mvc-4/_static/image41.png "再次按 Tab 鍵和程式碼片段會展開")

*再次按 Tab 鍵和程式碼片段會展開*

***若要新增的程式碼片段，使用滑鼠 （C#、 Visual Basic 和 XML）***

1. 以滑鼠右鍵按一下您要插入程式碼片段。
2. 選取 **插入程式碼片段**後面**My Code Snippets**。
3. 選擇相關的程式片段，從清單中，對它按一下。

![以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段](whats-new-in-aspnet-mvc-4/_static/image42.png "以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段")

*以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段*

![對它按一下挑選清單中，相關程式碼片段](whats-new-in-aspnet-mvc-4/_static/image43.png "對它按一下挑選清單中，相關程式碼片段")

*對它按一下挑選清單中，相關程式碼片段*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>附錄 b： 安裝 Visual Studio Express 2012 for Web

您可以安裝**Microsoft Visual Studio Express 2012 for Web**或另一個&quot;Express&quot;使用版本**[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. 下列指示會引導您完成安裝所需的步驟*Visual studio Express 2012 for Web*使用*Microsoft Web Platform Installer*。

1. 移至[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169)。 或者，如果您已安裝 Web Platform Installer，您可以開啟它，並搜尋產品&quot; <em>Visual Studio Express 2012 for Web 含 Windows Azure SDK</em>&quot;。
2. 按一下 **立即安裝**。 如果您不需要**Web Platform Installer**您將會重新導向至下載並安裝第一次。
3. 一次**Web Platform Installer**已開啟，按一下**安裝**，啟動安裝程式。

    ![安裝 Visual Studio Express](whats-new-in-aspnet-mvc-4/_static/image44.png "安裝 Visual Studio Express")

    *安裝 Visual Studio Express*
4. 閱讀所有產品的授權和詞彙，然後按一下**我接受**以繼續。

    ![接受授權條款](whats-new-in-aspnet-mvc-4/_static/image45.png)

    *接受授權條款*
5. 等候完成的下載與安裝程序。

    ![安裝進度](whats-new-in-aspnet-mvc-4/_static/image46.png)

    *安裝進度*
6. 安裝完成時，按一下**完成**。

    ![安裝已完成](whats-new-in-aspnet-mvc-4/_static/image47.png)

    *安裝已完成*
7. 按一下 **結束**關閉 Web Platform Installer。
8. 若要開啟 Visual Studio Express for Web，請前往**開始**畫面，即可開始撰寫&quot; **VS Express**&quot;，然後按一下**VS Express for Web**圖格。

    ![VS Express for Web 圖格](whats-new-in-aspnet-mvc-4/_static/image48.png)

    *VS Express for Web 圖格*

<a id="AppendixC"></a>

<a id="Appendix_C_Installing_WebMatrix_2_and_iPhone_Simulator"></a>
## <a name="appendix-c-installing-webmatrix-2-and-iphone-simulator"></a>附錄 c： 安裝 WebMatrix 2 和 iPhone 模擬器

若要模擬的 iPhone 裝置中執行您的網站中，您可以使用 WebMatrix 延伸&quot;Electric 所研發的行動裝置模擬器，在適用於 iPhone&quot;。 此外，您可以設定要執行模擬器，從 Visual Studio 2012 的相同擴充功能。

<a id="ApxCTask1"></a>

<a id="Task_1_-_Installing_WebMatrix_2"></a>
#### <a name="task-1---installing-webmatrix-2"></a>工作 1-安裝 WebMatrix 2

1. 移至[ [ https://go.microsoft.com/?linkid=9809776 ](https://go.microsoft.com/?linkid=9809776) ](https://go.microsoft.com/?linkid=9810169)。 或者，如果您已安裝 Web Platform Installer，您可以開啟它，並搜尋產品&quot; <em>WebMatrix 2</em>&quot;。
2. 按一下 **立即安裝**。 如果您不需要**Web Platform Installer**您將會重新導向至下載並安裝第一次。
3. 一次**Web Platform Installer**已開啟，按一下**安裝**，啟動安裝程式。

    ![安裝 WebMatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "安裝 WebMatrix 2")

    *安裝 WebMatrix 2*
4. 閱讀所有產品的授權和詞彙，然後按一下**我接受**以繼續。

    ![接受授權條款](whats-new-in-aspnet-mvc-4/_static/image50.png "接受授權條款")

    *接受授權條款*
5. 等候完成的下載與安裝程序。

    ![Průběh instalace](whats-new-in-aspnet-mvc-4/_static/image51.png "安裝進度")

    *安裝進度*
6. 安裝完成時，按一下**完成**。

    ![安裝已完成](whats-new-in-aspnet-mvc-4/_static/image52.png "安裝已完成")

    *安裝已完成*
7. 按一下 **結束**關閉 Web Platform Installer。

<a id="ApxCTask2"></a>

<a id="Task_2_-_Installing_the_iPhone_Simulator_Extension"></a>
#### <a name="task-2---installing-the-iphone-simulator-extension"></a>工作 2-安裝在 iPhone 模擬器延伸模組

1. 執行**WebMatrix**並開啟任何現有的網站，或建立新的。
2. 按一下 **執行**按鈕**Home**功能區，然後選取**新增**。

    ![加入新的 WebMatrix 延伸](whats-new-in-aspnet-mvc-4/_static/image53.png "新增新的 WebMatrix 延伸模組")

    *加入新的 WebMatrix 延伸模組*
3. 選取  **iPhone 模擬器**然後按一下**安裝**。

    ![瀏覽 WebMatrix 延伸模組](whats-new-in-aspnet-mvc-4/_static/image54.png "瀏覽 WebMatrix 延伸模組")

    *瀏覽 WebMatrix 延伸模組*
4. 封裝的詳細資訊，請按一下**安裝**來繼續執行延伸模組安裝。

    ![iPhone 模擬器延伸模組](whats-new-in-aspnet-mvc-4/_static/image55.png "iPhone 模擬器延伸模組")

    *iPhone 模擬器延伸模組*
5. 閱讀並接受使用者授權合約的延伸模組。

    ![WebMatrix 延伸 EULA](whats-new-in-aspnet-mvc-4/_static/image56.png "WebMatrix 延伸使用者授權合約")

    *WebMatrix 延伸使用者授權合約*
6. 現在，您可以執行您的網站從 WebMatrix 使用 iPhone 模擬器選項。

    ![執行使用 iPhone](whats-new-in-aspnet-mvc-4/_static/image57.png "執行使用 iPhone")

    *執行使用 iPhone*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Configuring_Visual_Studio_2012_to_run_iPhone_Simulator"></a>
#### <a name="task-3---configuring-visual-studio-2012-to-run-iphone-simulator"></a>工作 3-設定執行 iPhone 模擬器的 Visual Studio 2012

1. 開啟**Visual Studio 2012**並開啟任何網站，或建立新的專案。
2. 按一下向下的箭號，從 [執行] 按鈕，然後選取**瀏覽包含**。

    ![瀏覽方式](whats-new-in-aspnet-mvc-4/_static/image58.png "瀏覽方式")

    *瀏覽方式*
3. 在 [&quot;瀏覽&quot;] 對話方塊中，按一下**新增**。
4. 在 [&quot;新增程式&quot;] 對話方塊中，使用下列值：

   - <strong>計劃</strong>: C:\Users\*{CurrentUser}<em>\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe * （據以更新的路徑）</em>
   - **引數**: &quot;1&quot;
   - **易記名稱**: iPhone 模擬器

     ![新增程式](whats-new-in-aspnet-mvc-4/_static/image59.png "新增程式")

     *新增程式，瀏覽包含*
5. 按一下 **確定**並關閉對話方塊。
6. 現在，您就能夠在 iPhone 模擬器中執行您的 Web 應用程式，從 Visual Studio 2012。

    ![使用 iPhone 模擬器進行瀏覽](whats-new-in-aspnet-mvc-4/_static/image60.png "瀏覽與 iPhone 模擬器")

    *使用 iPhone 模擬器進行瀏覽*

<a id="AppendixD"></a>

<a id="Appendix_D_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-d-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>附錄 d： 發佈 ASP.NET MVC 4 應用程式使用 Web Deploy

本附錄將說明如何從 Windows Azure 管理入口網站中建立新的網站和發行您取得所遵循的實驗室中，利用 Web Deploy 發行功能的 Windows Azure 所提供的應用程式。

<a id="ApxDTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>工作 1-建立新的網站，從 Windows Azure 入口網站

1. 移至[Windows Azure 管理入口網站](https://manage.windowsazure.com/)並使用您的訂用帳戶相關聯的 Microsoft 認證登入。

    > [!NOTE]
    > 使用 Windows Azure，您可以免費託管 10 個 ASP.NET 網站，並再隨著流量成長而調整。 您可以註冊申請[此處](http://aka.ms/aspnet-hol-azure)。

    ![登入 Windows Azure 入口網站](whats-new-in-aspnet-mvc-4/_static/image61.png "登入 Windows Azure 入口網站")

    *登入 Windows Azure 管理入口網站*
2. 按一下 **新增**命令列上。

    ![建立新的 Web 站台](whats-new-in-aspnet-mvc-4/_static/image62.png "建立新的網站")

    *建立新的網站*
3. 按一下 **計算** | **網站**。 然後選取**快速建立**選項。 新的網站提供可用的 URL，然後按**建立網站**。

    > [!NOTE]
    > Windows Azure 網站時，您可以控制和管理雲端中執行的 web 應用程式的主應用程式。 [快速建立] 選項可讓您部署已完成的 web 應用程式至 Windows Azure 網站從入口網站外部。 它不包含設定資料庫的步驟。

    ![建立新的網站上，使用 快速建立](whats-new-in-aspnet-mvc-4/_static/image63.png "建立新的網站上，使用 快速建立")

    *建立新的網站上，使用 快速建立*
4. 等到新**網站**建立。
5. 建立網站後按一下底下的連結**URL**資料行。 檢查新的網站運作。

    ![瀏覽至新的 web 站台](whats-new-in-aspnet-mvc-4/_static/image64.png "瀏覽至新的網站")

    *瀏覽至新的網站*

    ![執行的 web 站台](whats-new-in-aspnet-mvc-4/_static/image65.png "執行的網站")

    *執行的網站*
6. 返回入口網站並按一下底下的網站名稱**名稱**資料行來顯示 [管理] 頁面。

    ![開啟 網站管理頁面](whats-new-in-aspnet-mvc-4/_static/image66.png "開啟的網站管理頁面")

    *開啟 網站管理頁面*
7. 在 **儀表板**頁面的 **快速概覽**區段中，按一下**下載發行設定檔**連結。

    > [!NOTE]
    > *發行設定檔*包含所有發行至 Windows Azure 網站的每個已啟用的發行方法的 web 應用程式所需的資訊。 發行設定檔包含 Url、 使用者認證和連接到並對每個發行集的方法已啟用的端點進行驗證所需的資料庫字串。 **Microsoft WebMatrix 2**， **Microsoft Visual Studio Express for Web**並**Microsoft Visual Studio 2012**支援讀取發行設定檔，以自動化的這些程式的組態正在發行至 Windows Azure 網站的 web 應用程式。

    ![正在下載網站發行設定檔](whats-new-in-aspnet-mvc-4/_static/image67.png "下載網站發行設定檔")

    *正在下載網站發行設定檔*
8. 下載發行設定檔至已知位置。 進一步在這個練習中，您會看到如何從 Visual Studio 的 web 應用程式至 Windows Azure 網站發行使用此檔案。

    ![儲存發行設定檔](whats-new-in-aspnet-mvc-4/_static/image68.png "儲存發佈設定檔")

    *儲存發行設定檔*

<a id="ApxDTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>工作 2-設定資料庫伺服器

如果您的應用程式會使用 SQL Server 資料庫，您必須建立 SQL Database 伺服器。 如果您想要部署簡單的應用程式不會使用 SQL Server，您可能會略過這項工作。

1. 您將需要 SQL Database 伺服器來儲存應用程式資料庫。 您可以從您的訂用帳戶，在 Windows Azure Management 入口網站中檢視 SQL Database 伺服器**Sql Database** | **伺服器** | **伺服器儀表板**。 如果您沒有建立伺服器，您可以建立一個使用**新增**命令列上的按鈕。 請記下的**伺服器名稱和 URL、 系統管理員身分登入名稱和密碼**，如同您會在下一個工作中使用它們。 並未建立資料庫，因為它會建立在稍後的階段。

    ![SQL Database 伺服器儀表板](whats-new-in-aspnet-mvc-4/_static/image69.png "SQL Database 伺服器儀表板")

    *SQL Database 伺服器儀表板*
2. 下一個工作在您將測試資料庫連接，從 Visual Studio 中，因此您需要在伺服器的清單包含您的本機 IP 位址**允許的 IP 位址**。 若要這樣做，請按一下**設定**，選取的 IP 位址**目前的用戶端 IP 位址**並將它貼上**起始 IP 位址**並**結束 IP 位址**文字方塊中，然後按一下 [ ![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png) ] 按鈕。

    ![新增用戶端 IP 位址](whats-new-in-aspnet-mvc-4/_static/image71.png)

    *新增用戶端 IP 位址*
3. 一次**用戶端 IP 位址**新增至允許的 IP 位址清單中，按一下**儲存**來確認變更。

    ![確認變更](whats-new-in-aspnet-mvc-4/_static/image72.png)

    *確認變更*

<a id="ApxDTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>工作 3-發行 ASP.NET MVC 4 應用程式使用 Web Deploy

1. 返回至 ASP.NET MVC 4 的方案。 在 **方案總管**，以滑鼠右鍵按一下網站專案，然後選取**發佈**。

    ![發行應用程式](whats-new-in-aspnet-mvc-4/_static/image73.png "發行應用程式")

    *發行網站*
2. 匯入發行設定檔儲存在第一項工作。

    ![匯入發行設定檔](whats-new-in-aspnet-mvc-4/_static/image74.png "匯入發行設定檔")

    *匯入發行設定檔*
3. 按一下 **驗證連線**。 驗證完成後按一下**下一步**。

    > [!NOTE]
    > 一旦您看到 [驗證連線] 按鈕旁邊會出現綠色核取記號完成驗證。

    ![驗證連接](whats-new-in-aspnet-mvc-4/_static/image75.png "驗證連線")

    *驗證連接*
4. 在 **設定**頁面的 **資料庫**區段中，按一下您的資料庫連接文字方塊旁邊的按鈕 (也就是**DefaultConnection**)。

    ![Web 部署組態](whats-new-in-aspnet-mvc-4/_static/image76.png "Web 部署設定")

    *Web 部署設定*
5. 設定資料庫連線，如下所示：

   - 在 **伺服器名稱**輸入您使用 SQL Database 伺服器的 URL *tcp:* 前置詞。
   - 在 **使用者名**輸入您的伺服器系統管理員身分登入名稱。
   - 在 **密碼**輸入您的伺服器系統管理員身分登入密碼。
   - 輸入新的資料庫名稱，例如： *MVC4SampleDB*。

     ![設定目的地連接字串](whats-new-in-aspnet-mvc-4/_static/image77.png "設定目的地連接字串")

     *設定目的地連接字串*
6. 然後按一下 [確定]。  當系統提示您建立資料庫時，按一下**是**。

    ![建立資料庫](whats-new-in-aspnet-mvc-4/_static/image78.png "建立的資料庫字串")

    *建立資料庫*
7. 您將用來連接至 Windows Azure 中的 SQL 資料庫的連接字串會顯示在文字方塊中的預設連線。 然後按 [下一步] 。

    ![連接字串指向 SQL Database](whats-new-in-aspnet-mvc-4/_static/image79.png "連接字串指向 SQL Database")

    *連接字串指向 SQL Database*
8. 在  **Preview**頁面上，按一下**發佈**。

    ![Web 應用程式發行](whats-new-in-aspnet-mvc-4/_static/image80.png "發行 web 應用程式")

    *發行 web 應用程式*
9. 當發行程序完成時，預設瀏覽器會開啟已發行的網站。

    ![應用程式發行至 Windows Azure](whats-new-in-aspnet-mvc-4/_static/image81.png "應用程式發行至 Windows Azure")

    *應用程式發佈至 Windows Azure*
