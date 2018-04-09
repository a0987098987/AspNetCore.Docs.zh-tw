---
uid: mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
title: ASP.NET MVC 4 中最新消息 |Microsoft 文件
author: rick-anderson
description: ASP.NET MVC 4 是用於建置使用信譽良好的設計模式與強大的 ASP.NET 的可擴充、 以標準為基礎的 web 應用程式的架構和...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 48f7feb3-872f-485d-b96f-e30011ff8c4a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 977a6b5a84825ebd087752dcc2ebc0c5410e1657
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
# <a name="whats-new-in-aspnet-mvc-4"></a>ASP.NET MVC 4 中最新消息

由[Web 營小組](https://twitter.com/webcamps)

[下載 Web 營訓練套件](https://aka.ms/webcamps-training-kit)

ASP.NET MVC 4 是用於建置使用信譽良好的設計模式與強大的 ASP.NET 和.NET framework 的可擴充、 以標準為基礎的 web 應用程式的架構。 這個新，第四個版本的 framework 著重於讓行動裝置 web 應用程式開發更容易。

一開始，當您建立新的 ASP.NET MVC 4 專案沒有現在可用來建置針對行動裝置的獨立應用程式的行動應用程式專案範本。 此外，與 jQuery Mobile jQuery.Mobile.MVC NuGet 封裝透過整合 ASP.NET MVC 4。 jQuery Mobile 是以 HTML5 為基礎的架構，用於開發 web 應用程式所相容的所有熱門的行動裝置平台，包括 Windows Phone、 iPhone、 Android，依此類推。 不過，如果您需要特製化，ASP.NET MVC 4 也可讓您提供不同裝置的不同檢視，並提供裝置特定的最佳化。

在這個實際操作實驗室中，您將啟動 ASP.NET MVC 4&quot;網際網路應用程式&quot;建立相片圖庫的應用程式的專案範本。 您將漸進式增強使用 jQuery Mobile 和 ASP.NET MVC 4 的新功能，才能讓它與不同的行動裝置和桌面的網頁瀏覽器相容的應用程式。 您也將了解程式碼產生和 ASP.NET MVC 4 如何讓您更輕鬆地撰寫非同步動作方法，支援工作的新程式碼訣竅&lt;ActionResult&gt;傳回型別。

> [!NOTE]
> 所有的範例程式碼和程式碼片段會包含在 Web 營訓練套件，可在[Microsoft-Web/WebCampTrainingKit 版本](https://aka.ms/webcamps-training-kit)。 這個實驗室中的特定專案將會位於[What's New in ASP.NET 4.5 Web Form](https://github.com/Microsoft-Web/HOL-ASPNETWebForms)。

<a id="Objectives"></a>
### <a name="objectives"></a>目標

在這個實際操作實驗室中，您將學會如何：

- 利用 ASP.NET MVC 專案範本包括新的行動應用程式專案範本的增強功能
- 若要改善在行動裝置上的顯示使用 HTML5 檢視區屬性和 CSS 的媒體查詢
- 使用 jQuery Mobile 漸進式增強功能，以及建置觸控最佳化 web UI
- 建立行動裝置的特定檢視
- 使用檢視切換程式元件，將行動和桌面應用程式中的檢視之間切換
- 建立使用工作支援非同步控制器

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>必要條件

您必須具備下列項目才能完成本實驗室：

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)或上層 (讀取[附錄 B](#AppendixB)如需有關如何安裝指示)。
- [ASP.NET MVC 4](../../../mvc4.md) （隨附於 Microsoft Visual Studio 2012 安裝）
- Windows Phone 模擬器 (包含在[Windows Phone 7.1.1 SDK](https://www.microsoft.com/download/details.aspx?id=29233))
- 選用- [WebMatrix 2](https://www.microsoft.com/web/webmatrix/)與**Electric Plum iPhone 模擬器**延伸模組 （僅適用於 iPhone 模擬器與瀏覽 web 應用程式的練習 3)

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>安裝程式

在實驗室文件，您會指示要插入的程式碼區塊。 為了方便起見，大部分的程式碼是依現狀 Visual Studio 程式碼片段，也可以從 Visual Studio 內使用並不必以手動方式新增。

若要安裝的程式碼片段：

1. 開啟 Windows 檔案總管 視窗並瀏覽至實驗室**Source\Setup**資料夾。
2. 按兩下**Setup.cmd**安裝 Visual Studio 程式碼片段的這個資料夾中的檔案。

如果您不熟悉 Visual Studio 程式碼片段，而且想来了解如何使用它們，您可以從這份文件參考附錄&quot;[附錄 a： 使用程式碼片段](#AppendixA)&quot;。

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>練習

這個實際操作實驗室包含下列練習：

1. [新的 ASP.NET MVC 4 專案範本](#Exercise1)
2. [建立相片圖庫的 Web 應用程式](#Exercise2)
3. [新增行動裝置的支援](#Exercise3)
4. [使用非同步控制器](#Exercise4)

> [!NOTE]
> 每個練習會伴隨著**結束**包含所產生的解決方案完成練習之後，您應該取得資料夾。 如果您需要其他協助使用的所有練習，您可以使用這個方案做為指南。


完成本實驗室估計時間： **60 分鐘**。

<a id="Exercise1"></a>

<a id="Exercise_1_New_ASPNET_MVC_4_Project_Templates"></a>
### <a name="exercise-1-new-aspnet-mvc-4-project-templates"></a>練習 1： 新的 ASP.NET MVC 4 專案範本

在此練習中，您會瀏覽 ASP.NET MVC 4 專案範本中的增強功能。 除了網際網路應用程式範本，MVC 3 中已有此版本現在包含行動應用程式的另一個範本。 首先，您會看到一些相關的每個範本功能。 然後，您會在使用正確的方法來呈現您的頁面在不同的平台上正確作用。

<a id="Task_1_-_Exploring_the_Internet_Application_Template"></a>
#### <a name="task-1---exploring-the-internet-application-template"></a>工作 1-瀏覽網際網路應用程式範本

1. 開啟**Visual Studio**。
2. 選取**檔案 |新 |專案**功能表命令。 在**新專案**對話方塊中，選取**Visual C# |Web**在左窗格中的範本樹狀結構，然後選擇  **ASP.NET MVC 4 Web 應用程式。** 將專案命名**PhotoGallery**、 選取的位置 （或保留預設值），按一下 **確定**。

    > [!NOTE]
    > 您稍後將會自訂 PhotoGallery ASP.NET MVC 4 方案，您現在建立。

    ![建立新的專案](whats-new-in-aspnet-mvc-4/_static/image1.png "建立新的專案")

    *建立新的專案*
3. 在**新增 ASP.NET MVC 4 專案**對話方塊中，選取**網際網路應用程式**專案範本，然後按一下**確定**。 請確定您已選取 Razor 檢視引擎。

    ![建立新 ASP.NET MVC 4 網際網路應用程式](whats-new-in-aspnet-mvc-4/_static/image2.png "建立新 ASP.NET MVC 4 網際網路應用程式")

    *建立新 ASP.NET MVC 4 網際網路應用程式*

    > [!NOTE]
    > ASP.NET MVC 3 中已經導入 razor 語法。 其目的是字元和所需在檔案中，啟用快速和程式碼撰寫工作流程的流體按鍵數目降至最低。 Razor 運用現有的 C# / VB （或其他） 的語言能力，並傳遞可讓實用的 HTML 建構工作流程範本標記語法。
4. 按**F5**以執行此方案，並查看更新的範本。 您可以查看下列功能：

    **現代化樣式範本**

    已經更新的範本，提供更多現代尋找樣式。

    ![ASP.NET MVC 4 重新設定樣式範本](whats-new-in-aspnet-mvc-4/_static/image3.png "MVC 4 重新設定樣式，範本")

    *ASP.NET MVC 4 重新設定樣式範本*

    ![新的連絡人 頁面](whats-new-in-aspnet-mvc-4/_static/image4.png "新連絡人頁面")

    *新的連絡人 頁面*

    **調整呈現**

    簽出調整瀏覽器視窗，並注意如何頁面配置動態適應新的視窗大小。 這些範本會使用適應性呈現技術，在桌上型電腦和行動平台，而無需自訂正確轉譯。

    ![ASP.NET MVC 4 專案範本中不同的瀏覽器大小](whats-new-in-aspnet-mvc-4/_static/image5.png "ASP.NET MVC 4 專案範本中不同的瀏覽器的大小")

    *ASP.NET MVC 4 專案範本中不同的瀏覽器的大小*

    **更豐富的 UI 使用 JavaScript**

    預設專案範本的其他增強功能，就是使用 JavaScript 來提供更具互動性的 JavaScript。 範本中使用的登入和註冊連結提供使用 jQuery 驗證來驗證來自用戶端的輸入的欄位。

    ![jQuery 驗證](whats-new-in-aspnet-mvc-4/_static/image6.png)

    *jQuery 驗證*

    > [!NOTE]
    > 請注意這兩個登入中的第一個區段的區段，您可以記錄在使用 registerd 帳戶從站台，您可以 altenativelly 使用登入 google （預設為停用） 等其他的驗證服務的第二個區段中。
5. 關閉瀏覽器以停止偵錯工具，並返回 Visual Studio。
6. 開啟檔案**AuthConfig.cs**位於**應用程式\_啟動**資料夾。
7. 從註冊 Google 用戶端的最後一行移除註解*OAuth*驗證。


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample1.cs)]

> [!NOTE]
> Notice you can easily enable authentication using any OpenID or OAuth service like Facebook, Twitter, Microsoft, etc.
~~~
8. 按**F5**執行方案，並瀏覽到登入頁面。
9. 選取**Google**服務以登入。

    ![在服務中選取記錄檔](whats-new-in-aspnet-mvc-4/_static/image7.png)

    *在服務中選取記錄檔*
10. 使用 Google 帳戶登入。
11. 允許 Google 帳戶中擷取資訊的站台 (localhost)。
12. 最後，您必須註冊在 Google 帳戶相關聯的站台。

   ![將您的 Google 帳戶建立關聯](whats-new-in-aspnet-mvc-4/_static/image8.png)

   *將您的 Google 帳戶產生關聯*
13. 關閉瀏覽器以停止偵錯工具，並返回 Visual Studio。
14. 現在您可以瀏覽簽出由 ASP.NET MVC 4 專案範本中導入了一些其他新功能的方案。

   ![ASP.NET MVC 4 網際網路應用程式專案範本](whats-new-in-aspnet-mvc-4/_static/image9.png "ASP.NET MVC 4 網際網路應用程式專案範本")

   *ASP.NET MVC 4 網際網路應用程式專案範本*

   - **HTML 5 標記**

       瀏覽以找出新的佈景主題標記的樣板檢視。

       ![新的範本，使用 Razor 和 HTML5 標記 About.cshtml。](whats-new-in-aspnet-mvc-4/_static/image10.png "使用 Razor 和 HTML5 標記 About.cshtml 的新範本。")

       *新的範本，使用 Razor 和 HTML5 的標記 (About.cshtml)。*
   - **更新的 JavaScript 程式庫**

       ASP.NET MVC 4 預設範本現在包含 KnockoutJS，可讓您建立豐富的 JavaScript MVVM 架構和高回應性的 web 應用程式使用 JavaScript 和 HTML。 例如在 MVC3、 jQuery 和 jQuery UI 程式庫是也包含在 ASP.NET MVC 4。

     > [!NOTE]
     > 您可以取得有關 KnockOutJS 此連結中的程式庫的詳細資訊： [ [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/) ](http://learn.knockoutjs.com/)。 此外，您可以了解 jQuery 和 jQuery UI 中[ [ http://docs.jquery.com/ ](http://docs.jquery.com/) ](http://docs.jquery.com/)。

<a id="Task_2_-_Exploring_the_Mobile_Application_Template"></a>
#### <a name="task-2---exploring-the-mobile-application-template"></a>工作 2-瀏覽 行動應用程式範本

ASP.NET MVC 4 可加速開發行動應用程式的網站和平板電腦瀏覽器。 此範本具有相同的應用程式結構，為網際網路應用程式範本 （請注意，控制器的程式碼是幾乎完全相同），但其樣式已修改為在觸控式行動裝置中正確轉譯。

1. 選取**檔案 |新 |專案**功能表命令。 在**新專案**對話方塊中，選取**Visual C# |Web**在左窗格中的範本樹狀結構，然後選擇  **ASP.NET MVC 4 Web 應用程式。** 為專案名稱， **PhotoGallery.Mobile**、 選取的位置 （或保留預設值），選取&quot;將加入至方案&quot;按一下**確定**。
2. 在**新增 ASP.NET MVC 4 專案**對話方塊中，選取**行動應用程式**專案範本，然後按一下**確定**。 請確定您已選取 Razor 檢視引擎。

    ![建立新 ASP.NET MVC 4 行動應用程式](whats-new-in-aspnet-mvc-4/_static/image11.png "建立新 ASP.NET MVC 4 行動應用程式")

    *建立新 ASP.NET MVC 4 行動應用程式*
3. 現在您可以瀏覽方案，並查看一些行動裝置版的 ASP.NET MVC 4 方案範本所引進的新功能：

    - **jQuery Mobile 的程式庫**

        行動應用程式專案範本包括 jQuery 行動程式庫，也就是行動瀏覽器相容性的開放原始碼程式庫。 jQuery Mobile 適用於行動瀏覽器支援 CSS 和 JavaScript 的漸進式增強功能。 漸進式增強功能可讓所有瀏覽器來顯示網頁的基本內容，而只可讓最強大的瀏覽器顯示豐富的內容。 JavaScript 和 CSS 檔案，包含在 jQuery 行動樣式，協助以容納至畫面中的內容，而不進行任何變更，在網頁標記中的行動瀏覽器。

        ![jQuery-mobile-library-included-in-the-template](whats-new-in-aspnet-mvc-4/_static/image12.png)

        *範本包含 jQuery 行動程式庫*
    - **HTML5 基礎標記**

        ![Mobile-application-template-using-HTML5-markup](whats-new-in-aspnet-mvc-4/_static/image13.png)

        *使用 HTML5 標記、 （Login.cshtml 和 index.cshtml） 的行動應用程式範本*
4. 按**F5**執行解決方案。
5. 開啟**Windows Phone 7 模擬器**。
6. 在 phone [開始] 畫面中，開啟 Internet Explorer。 簽出的桌面應用程式的開始位置的 URL，並從手機瀏覽至該 URL (例如`http://localhost:[PortNumber]/`)。
7. 現在您可以輸入登入頁面，或查看有關頁面。 請注意該網站的樣式，根據行動應用程式的新 Metro 應用程式。 ASP.NET MVC 4 專案範本會正確顯示在行動裝置，並確認頁面的所有項目可見並已啟用。 請注意，標頭上的連結不夠大，按一下或點選。

    ![專案範本在行動裝置的網頁](whats-new-in-aspnet-mvc-4/_static/image14.png "專案範本頁面，在行動裝置")

    *專案範本頁面，在行動裝置*
8. 新的範本也會使用**檢視區 meta 標記**。 大部分行動瀏覽器定義虛擬瀏覽器視窗的寬度或&quot;檢視區&quot;，其大於行動裝置的實際寬度。 這可讓行動瀏覽器顯示整個網頁內虛擬顯示器。 **檢視區 meta 標記**可讓 web 開發人員在行動裝置上設定的寬度、 高度和小數位數的瀏覽器區域**。** 行動應用程式的 ASP.NET MVC 4 範本將檢視區設定為裝置寬度 (&quot;寬度 = 裝置寬度&quot;) 中的版面配置範本 (*_layout.cshtml\_Layout.cshtml*)，如此所有頁面會有其檢視區設定的裝置的螢幕寬度。 請注意的檢視區 meta 標記，不會變更預設瀏覽器檢視。
9. 開啟 **\_Layout.cshtml**，位於**檢視 |共用**資料夾和註解的檢視區 meta 標記。 執行應用程式中，如果不是已開啟，且簽出差異。


~~~
[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample2.cshtml)]

![The site after commenting the viewport meta tag](whats-new-in-aspnet-mvc-4/_static/image15.png "The site after commenting the viewport meta tag")

*The site after commenting the viewport meta tag*
~~~
10. 在 Visual Studio 中，按**SHIFT** + **F5**停止偵錯應用程式。
11. 取消註解的檢視區 meta 標記。


~~~
[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample3.cshtml)]
~~~

<a id="Task_3_-_Using_Adaptive_Rendering"></a>
#### <a name="task-3---using-adaptive-rendering"></a>工作 3-使用適應性呈現

在這項工作，您將學習要在同一時間而無需自訂呈現在行動裝置及網頁瀏覽器上正確的網頁另一個方法。 您已經有類似用途使用檢視區的中繼標籤。 現在您將會符合另一個功能強大的方法：*適應性呈現*。

調整呈現是一種技術使用**CSS3 媒體查詢**自訂套用至頁面的樣式。 媒體查詢定義在樣式表，分組在特定條件下的 CSS 樣式內的條件。 只有當條件為 true，則會將樣式套用至宣告的物件。

調整呈現技術所提供的彈性可讓不同的裝置上顯示站台的任何自訂。 您可以定義多您要在單一樣式表上不需要撰寫邏輯程式碼選擇樣式的樣式。 因此，它是非常簡潔的方式調整頁面樣式，因為它會減少重複的程式碼和邏輯的轉譯用途數量。 相反地，頻寬耗用量會增加，CSS 檔案的大小可能會稍微變。

根據使用適應性呈現技術，將會是您的站台**正常地顯示，不論瀏覽器。** 不過，您應該考慮是否載入額外的頻寬是一項考量。

> [!NOTE]
> 媒體查詢的基本格式為： @media \[範圍： 所有 | 掌上型 | 列印 | 投影 | 螢幕\]([屬性： 值] 和...[屬性： 值]）


媒體查詢的範例： &gt;  <strong>@media所有和 (最大寬度： 1000px) 和 (最小寬度： 700px) {}:</strong> 700px 和 1000px 之間所有解析的。

> <strong>@media 螢幕和 (最小寬度： 400px) 和 (最大寬度： 700px) {...}:</strong>只針對螢幕。 解析度必須介於 400 到 700px。
> 
> <strong>@media 掌上型和 (最小寬度： 20em)、 螢幕以及 (最小寬度： 20em) {...}:</strong>的掌上型 （行動裝置和裝置） 和畫面。 最小寬度必須大於 20em。
> 
> 您可以找到相關資訊上[W3C 網站](http://www.w3.org/TR/css3-mediaqueries/)。


您現在將探索調整呈現的運作方式、 提升可讀性的 ASP.NET MVC 4 預設的網站範本。

1. 開啟**PhotoGallery.sln**方案，您必須建立在工作 1，並選取**PhotoGallery**專案。 按**F5**執行解決方案。
2. 調整瀏覽器的寬度，設定 windows，一半或少於原始大小的四分之一。 請注意，標頭中的項目會發生什麼事： 某些項目不會出現在標頭的可見區域。
3. 開啟<strong>Site.css</strong>檔從 Visual Studio 方案總管 中，位於<strong>內容</strong>專案資料夾。 按<strong>CTRL + F</strong>開啟 Visual Studio 整合式的搜尋，和寫入<strong>@media</strong>找出<strong>CSS 媒體查詢</strong>。

    此範本中定義的媒體查詢條件適用於以這種方式： 當瀏覽器視窗大小低於**850 px**，套用的 CSS 規則可用來定義在這個媒體區塊內。

    ![尋找的媒體查詢](whats-new-in-aspnet-mvc-4/_static/image16.png "尋找的媒體查詢")

    *尋找的媒體查詢*
4. 取代 850 中設定的最大寬度屬性值與像素**10px**，以停用適應性呈現，然後按**CTRL + S**儲存的變更。 返回瀏覽器，然後按**CTRL + F5**重新整理頁面與您所做的變更。 調整視窗的寬度時，請注意這兩個頁面中的差異。

    ![在左邊，將套用頁面@media省略樣式，請在右側，樣式](whats-new-in-aspnet-mvc-4/_static/image17.png "頁面套用在左邊，@media省略樣式，請在右側，樣式")

    <em>在左邊，將套用頁面@media省略樣式，請在右側，樣式</em>

    現在，讓我們查看的行動裝置上發生的狀況：

    ![在左邊，將套用頁面@media省略樣式，請在右側，樣式](whats-new-in-aspnet-mvc-4/_static/image18.png "頁面套用在左邊，@media省略樣式，請在右側，樣式")

    <em>在左邊，將套用頁面@media省略樣式，請在右側，樣式</em>

    雖然您會注意到使用行動裝置時不非常重要的是，變更 Web 瀏覽器中呈現網頁時的差異變得更為明顯。 在左邊的映像中，我們可以看到的自訂樣式改善可讀性。

    適應性呈現可用於許多案例，以便更容易套用條件式樣式至網站並解決一般樣式問題很棒的方法。

    檢視區 meta 標記和 CSS 的媒體查詢不特定的 ASP.NET MVC 4，因此您可在任何 web 應用程式中使用這些功能。
5. 在 Visual Studio 中，按**SHIFT** + **F5**停止偵錯應用程式。

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_the_Photo_Gallery_Web_Application"></a>
### <a name="exercise-2-creating-the-photo-gallery-web-application"></a>練習 2： 建立相片圖庫的 Web 應用程式

在此練習中，您會在相片圖庫應用程式顯示相片作用。 您會開始使用 ASP.NET MVC 4 專案範本，然後您會加入一項功能可以從服務擷取相片，並且顯示在首頁上。

在下列練習中，您將會更新此解決方案，以強化行動裝置顯示的方式。

<a id="Task_1_-_Creating_a_Mock_Photo_Service"></a>
#### <a name="task-1---creating-a-mock-photo-service"></a>工作 1-建立模擬的相片服務

在這項工作，您將建立相片服務將會顯示在圖庫中的內容擷取來源的模擬。 若要這樣做，您將加入新的控制站，只會傳回 JSON 檔案，含有每張相片的資料。

1. 開啟**Visual Studio**如果尚未開啟。
2. 選取**檔案 |新 |專案**功能表命令。 在**新專案**對話方塊中，選取**Visual C# |Web**在左窗格中的範本樹狀結構，然後選擇  **ASP.NET MVC 4 Web 應用程式。** 將專案命名**PhotoGallery**、 選取的位置 （或保留預設值），按一下 **確定**。 或者，您可以繼續使用您現有的 ASP.NET MVC 4**網際網路應用程式**方案從**練習 1**並略過下一個步驟。
3. 在**新增 ASP.NET MVC 4 專案**對話方塊中，選取**網際網路應用程式**專案範本，然後按一下**確定**。 請確定您具有 Razor 檢視引擎為選取。
4. 在**方案總管 中**，以滑鼠右鍵按一下**應用程式\_資料**您的專案，然後選取資料夾**新增 |現有項目**。 瀏覽至**Source\Assets\App\_資料**本實驗室的資料夾並加入**Photos.json**檔案。
5. 建立新的控制站名稱**PhotoController**。 若要這樣做，以滑鼠右鍵按一下**控制器**資料夾中，移至**新增**選取**控制站。** 完成控制站名稱，請將保留**空白的 MVC 控制器**範本，然後按一下**新增**。

    ![加入 PhotoController](whats-new-in-aspnet-mvc-4/_static/image19.png "加入 PhotoController")

    *加入 PhotoController*
6. 取代**索引**以下列方法**圖庫**動作，並傳回內容最近加入至專案的 JSON 檔案中。

    (程式碼片段- *ASP.NET MVC 4 實驗室-Ex02-圖庫動作*)


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample4.cs)]
~~~
7. 按**F5**執行方案，並瀏覽至下列 URL 以測試模擬的相片服務： `http://localhost:[port]/photo/gallery` （取決於應用程式的啟動位置目前的連接埠的 [連接埠] 值）。 此 URL 的要求應該擷取內容的**Photos.json**檔案。

    ![測試模擬的相片服務](whats-new-in-aspnet-mvc-4/_static/image20.png "測試模擬的相片服務")

    *測試模擬的相片服務*

您可以在實際的實作使用[ASP.NET Web API](../../../../web-api/index.md)實作相片圖庫服務。 ASP.NET Web API 是一種架構，可讓您更輕鬆地建立連線的用戶端，包括瀏覽器和行動裝置的較大範圍的 HTTP 服務。 ASP.NET Web 應用程式開發介面是在 .NET Framework 上建置 RESTful 應用程式的理想平台。

<a id="Task_2_-_Displaying_the_Photo_Gallery"></a>
#### <a name="task-2---displaying-the-photo-gallery"></a>工作 2-顯示相片圖庫

在這項工作，您將更新以顯示相片圖庫會使用您在此練習中的第一個工作中建立的模擬的服務的 [首頁] 頁面。 您會加入模型檔案，以及更新組件庫檢視。

1. 在 Visual Studio 中，按**SHIFT** + **F5**停止偵錯應用程式。
2. 建立**相片**類別**模型**資料夾。 若要這樣做，以滑鼠右鍵按一下**模型**資料夾中，選取**新增**按一下**類別**。 然後，將名稱設定為**Photo.cs**按一下**新增**。
3. 將下列成員加入**相片**類別。

    (程式碼片段- *ASP.NET MVC 4 實驗室-Ex02-相片模型*)


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample5.cs)]
~~~
4. 從 **Controllers** 資料夾中，開啟 **HomeController.cs** 檔案。
5. 使用陳述式加入下列程式碼。

    (程式碼片段- *ASP.NET MVC 4 實驗室-Ex02-HomeController Using*)


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample6.cs)]
~~~
6. 更新**索引**動作使用**HttpClient**圖庫及擷取資料，然後使用**JavaScriptSerializer**其還原序列化至檢視模型。

    (程式碼片段- *ASP.NET MVC 4 實驗室-Ex02-索引動作*)


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample7.cs)]
~~~
7. 開啟**Index.cshtml**檔案位於**Views\Home**資料夾，然後取代為下列程式碼的所有內容。

    此程式碼執行迴圈，從服務擷取的所有相片，並顯示為未排序清單。

    (程式碼片段- *ASP.NET MVC 4 實驗室-Ex02-相片清單*)


~~~
[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample8.cshtml)]
~~~
8. 在**方案總管 中**，以滑鼠右鍵按一下**內容**您的專案，然後選取資料夾**新增 |現有項目**。 瀏覽至**Source\Assets\Content**本實驗室的資料夾並加入**Site.css**檔案。 您必須確認其取代。 如果您有**Site.css**檔案開啟，您必須確認也重新載入檔案。
9. 開啟檔案總管，並複製整個**相片**資料夾位於下面**Source\Assets**本實驗室中 [方案總管] 中的專案根資料夾的資料夾。
10. 執行應用程式。 您現在應該會看到顯示相片圖庫中的 [首頁] 頁面。

    ![相片圖庫](whats-new-in-aspnet-mvc-4/_static/image21.png "相片圖庫")

    *相片圖庫*
11. 在 Visual Studio 中，按**SHIFT** + **F5**停止偵錯應用程式。

<a id="Exercise3"></a>

<a id="Exercise_3_Adding_support_for_mobile_devices"></a>
### <a name="exercise-3-adding-support-for-mobile-devices"></a>練習 3： 新增行動裝置的支援

在 ASP.NET MVC 4 中的金鑰更新的其中一個是行動應用程式開發的支援。 在這個練習中，您將藉由擴充 PhotoGallery 方案，您已經在上一個練習建立探索行動應用程式的 ASP.NET MVC 4 新功能。

<a id="Task_1_-_Installing_jQuery_Mobile_in_an_ASPNET_MVC_4_Application"></a>
#### <a name="task-1---installing-jquery-mobile-in-an-aspnet-mvc-4-application"></a>工作 1-安裝 ASP.NET MVC 4 應用程式中的 jQuery Mobile

1. 開啟**開始**方案位於**來源/Ex3MobileSupport/開始/**資料夾。 否則，您可能會繼續使用**結束**方案所完成的上一個練習中取得。

   1. 如果您開啟提供**開始**方案，您必須下載某些遺漏的 NuGet 套件才能繼續。 若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 封裝**。
   2. 在**管理 NuGet 封裝**] 對話方塊中，按一下 [**還原**才能下載遺漏的封裝。
   3. 最後，按一下 建置方案**建置** | **建置方案**。

      > [!NOTE]
      > 使用 NuGet 的優點之一是您不需要在專案中，所有的程式庫的出貨減少專案大小。 NuGet 的強大工具，請藉由指定封裝版本在 Packages.config 檔案中，您將會成功下載所有必要的程式庫第一次您執行專案。 這就是為什麼您必須從這個實驗室中開啟現有的方案後執行這些步驟。
2. 開啟**Package Manager Console**按一下**工具** &gt; **程式庫套件管理員** &gt; **封裝管理員主控台**功能表選項。

    ![開啟 NuGet 封裝管理員主控台](whats-new-in-aspnet-mvc-4/_static/image22.png "開啟 NuGet 封裝管理員主控台")

    *開啟 NuGet 封裝管理員主控台*
3. Package Manager Console 中執行下列命令以安裝**jQuery.Mobile.MVC**封裝。

    jQuery Mobile 是開放原始碼程式庫建置觸控最佳化 web UI。 JQuery.Mobile.MVC NuGet 套件包含 ASP.NET MVC 4 應用程式中使用 jQuery Mobile 的協助程式。

    > [!NOTE]
    > 藉由執行下列命令，您將 jQuery.Mobile.MVC 程式庫從下載 Nuget。

    PM

    [!code-powershell[Main](whats-new-in-aspnet-mvc-4/samples/sample9.ps1)]

    此命令會安裝 jQuery Mobile 和一些協助程式檔案，包括下列：

    - **Views/Shared/\_Layout.Mobile.cshtml**： 為 jQuery Mobile 架構版面配置最佳化的較小的螢幕。 當網站從行動裝置瀏覽器收到要求時，它會取代原始的版面配置 (\_Layout.cshtml) 換成這一個。
    - 檢視切換程式元件： 組成**Views/Shared/\_ViewSwitcher.cshtml**部分檢視和**ViewSwitcherController.cs**控制站。 此元件會顯示行動瀏覽器可讓使用者切換到桌面版本 頁面的連結。  
        ![相片圖庫的行動支援專案](whats-new-in-aspnet-mvc-4/_static/image23.png "相片圖庫專案使用的行動支援")

        *相片圖庫專案使用的行動支援*
4. 註冊行動裝置的組合。 若要這樣做，請開啟**Global.asax.cs**檔案，然後加入下列一行。

    (程式碼片段- *ASP.NET MVC 4 實驗室-Ex03-註冊行動裝置組合*)


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample10.cs)]
~~~
5. 執行使用桌面的網頁瀏覽器的應用程式。
6. 開啟**Windows Phone 7 模擬器，**位於**開始功能表 |所有程式 |Windows Phone SDK 7.1 |Windows Phone 模擬器。**
7. 在 phone [開始] 畫面中，開啟 Internet Explorer。 簽出啟動應用程式的 URL，並瀏覽至該電話瀏覽器的 URL (例如`http://localhost:[PortNumber]/`)。

    您會注意到您的應用程式會尋找在 Windows Phone 模擬器中，不同 jQuery.Mobile.MVC 已顯示針對行動裝置最佳化的檢視之專案中建立新的資產。

    請注意上方的電話，顯示 切換到桌面檢視連結的訊息。 此外，  **\_Layout.Mobile.cshtml**由您已安裝的封裝所建立的配置包括應用程式中不同的版面配置。

    > [!NOTE]
    > 目前為止，沒有任何連結回到行動檢視。 它將會包含在較新版本。

    ![相片圖庫首頁的行動檢視](whats-new-in-aspnet-mvc-4/_static/image24.png "相片圖庫首頁的行動檢視")

    *相片圖庫首頁的行動檢視*
8. 在 Visual Studio 中，按**SHIFT** + **F5**停止偵錯應用程式。

<a id="Task_2_-_Creating_Mobile_Views"></a>
#### <a name="task-2---creating-mobile-views"></a>工作 2-建立行動檢視

在這項工作，您將建立索引檢視的行動裝置版的行動裝置的較佳 appareance 調整的內容。

1. 複製**Views\Home\Index.cshtml**檢視，並將它貼到 建立複本，請重新命名新的檔案， **Index.Mobile.cshtml**。
2. 開啟新建立**Index.Mobile.cshtml**檢視，並取代現有&lt;ul&gt;標記這個程式碼。 如此一來，您將會更新&lt;ul&gt;使用從 jQuery 行動的佈景主題的 jQuery 行動資料的註解的標記。


~~~
[!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample11.html)]

> [!NOTE] 
> 
> Notice that:
> 
> - The **data-role** attribute set to **listview** will render the list using the listview styles.
> 
> - The **data-inset** attribute set to true will show the list with rounded border and margin.
> 
> - The **data-filter** attribute set to **true** will generate a search box.
> 
> You can learn more about jQuery Mobile conventions in the project documentation: [[http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)
~~~
3. 按**CTRL + S**儲存的變更。
4. 切換至**Windows Phone 模擬器**和重新整理網站。 請注意新的外觀與風格的組件庫 清單中，為新的搜尋方塊位於最上方。 然後，在 [搜尋] 方塊中輸入文字 (例如， **Tulips**) 才會開始搜尋相片圖庫中。

    ![使用已篩選清單檢視樣式的組件庫](whats-new-in-aspnet-mvc-4/_static/image25.png "使用 listview 樣式已篩選的組件庫")

    *使用已篩選清單檢視樣式的組件庫*

    若要總括而言，您已使用的檢視 Mobilizer 配方來建立一份索引檢視與&quot;行動&quot;後置詞。 這個後置詞表示 ASP.NET MVC 4 行動裝置所產生的每個要求會使用這份索引。 此外，您已更新索引檢視，可以提升網站的外觀與風格，行動裝置的使用 jQuery Mobile 的行動裝置的版本。
5. 返回 Visual Studio] 和 [開啟**Site.Mobile.css**位於**內容**資料夾。
6. 修正相片標題，讓它顯示在右側的映像的位置。 若要這樣做，請將加入下列程式碼加入**Site.Mobile.css**檔案。

    CSS

    [!code-css[Main](whats-new-in-aspnet-mvc-4/samples/sample12.css)]
7. 按**CTRL + S**儲存的變更。
8. 切換回**Windows Phone 模擬器**和重新整理網站。 請注意現在正確置於相片標題。

    ![標題位於影像右側](whats-new-in-aspnet-mvc-4/_static/image26.png "放置影像右側的標題")

    *標題位於右側的映像*

<a id="Task_3_-_jQuery_Mobile_Themes"></a>
#### <a name="task-3---jquery-mobile-themes"></a>工作 3-jQuery Mobile 佈景主題

每個配置和 jQuery Mobile 中的小工具會針對新物件導向 CSS 的架構，可讓完整統一的視覺化設計佈景主題套用至站台和應用程式而設計。

jQuery Mobile 預設佈景主題包含 5 個樣本指定字母 (a、 b、 c、 d，e） 以供快速參考。

在這項工作，您將更新要使用與預設值不同的佈景主題的行動配置。

1. 切換回 Visual Studio。
2. 開啟 **\_Layout.Mobile.cshtml**檔案位於**_layout.cshtml**。
3. 尋找 div 項目設定為資料角色&quot;頁面&quot;並更新**資料佈景主題**至&quot; **e**&quot;。


~~~
[!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample13.html)]
~~~
4. 按**CTRL + S**儲存的變更。
5. 重新整理中的站台**Windows Phone 模擬器**，並注意新的色彩配置。

    ![行動裝置的版面配置與不同的色彩配置](whats-new-in-aspnet-mvc-4/_static/image27.png "行動的版面配置與不同的色彩配置")

    *行動裝置的版面配置與不同的色彩配置*

<a id="Task_4_-_Using_the_View-Switcher_Component_and_the_Browser_Overriding_Features"></a>
#### <a name="task-4---using-the-view-switcher-component-and-the-browser-overriding-features"></a>工作 4-使用檢視切換程式元件和瀏覽器覆寫功能

行動裝置最佳化網頁的慣例是頁面的加入連結，其文字是頁面的像桌面檢視或完整的網站模式可讓使用者切換到桌面版本。 JQuery.Mobile.MVC 封裝包含的範例，**檢視切換程式**元件用於此用途 **\_Layout.Mobile.cshtml**檢視。

![連結切換到桌面檢視](whats-new-in-aspnet-mvc-4/_static/image28.png "連結切換到桌面檢視")

*若要切換到桌面檢視連結*

檢視切換程式會使用新功能，稱為**瀏覽器覆寫**。 這項功能可讓您的應用程式要求視為就像是來自它們實際上來自於不同瀏覽器 （使用者代理程式）。

在這個工作中，您將探索加入 jQuery.Mobile.MVC 和新的瀏覽器覆寫功能的 ASP.NET MVC 4 檢視切換器的範例實作。

1. 切換回 Visual Studio。
2. 開啟 **\_Layout.Mobile.cshtml**檢視位於**_layout.cshtml**資料夾，請注意為部分檢視所參考的檢視切換程式元件。

    ![使用檢視切換程式元件的版面配置行動](whats-new-in-aspnet-mvc-4/_static/image29.png "行動使用檢視切換程式元件的版面配置")

    *行動裝置使用檢視切換程式元件的版面配置*
3. 開啟 **\_ViewSwitcher.cshtml**部分檢視。

    部分檢視會使用新的方法**ViewContext.HttpContext.GetOverriddenBrowser()**判斷 web 要求的來源，並顯示對應的連結，以切換至 桌面 或 行動檢視。

    **GetOverridenBrowser**方法會傳回**HttpBrowserCapabilitiesBase**對應於目前針對要求設定的使用者代理程式執行個體 （實際或覆寫）。 您可以使用此值來取得屬性，例如**IsMobileDevice**。

    ![ViewSwitcher 部分檢視](whats-new-in-aspnet-mvc-4/_static/image30.png "ViewSwitcher 部分檢視")

    *ViewSwitcher 部分檢視*
4. 開啟**ViewSwitcherController.cs**類別位於**控制器**資料夾。 簽出該 SwitchView 動作稱為 ViewSwitcher 元件中的連結，並注意新的 HttpContext 方法。

    - **HttpContext.ClearOverridenBrowser()**方法會移除目前要求的任何覆寫的使用者代理程式。
    - **HttpContext.SetOverridenBrowser()**方法會覆寫使用指定的使用者代理程式要求的實際使用者代理程式值。  
        ![ViewSwitcher 控制器](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher 控制器")  
*ViewSwitcher 控制器*

        瀏覽器正在覆寫是核心功能的 ASP.NET MVC 4，這也會提供即使您沒有安裝 jQuery.Mobile.MVC 封裝。 不過，這項功能會影響檢視、 配置和部分檢視，而且不會影響任何相依於 Request.Browser 物件的功能。

<a id="Task_5_-_Adding_the_View-Switcher_in_the_Desktop_View"></a>
#### <a name="task-5---adding-the-view-switcher-in-the-desktop-view"></a>工作 5-在 [桌面] 檢視中加入檢視切換程式

在這項工作，您將更新以包含檢視切換程式桌面的配置。 這可讓行動使用者返回行動檢視瀏覽桌面檢視時。

1. 重新整理中的站台**Windows Phone 模擬器**。
2. 按一下**桌面檢視**頂端的組件庫的連結。 請注意在桌面的檢視，以讓您回到 [行動] 檢視中沒有任何檢視切換程式。
3. 返回 Visual Studio] 和 [開啟 **\_Layout.cshtml**檢視。
4. 尋找登入區段，然後插入要呈現的呼叫 **\_ViewSwitcher**下方的部分檢視 **\_LogOnPartial**部分檢視。 然後按下**CTRL + S**儲存的變更。


~~~
[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample14.cshtml)]
~~~
5. 按**CTRL + S**儲存的變更。
6. 重新整理頁面，在 Windows Phone 模擬器中的，按兩下 放大螢幕。 請注意，現在顯示在首頁上**行動檢視**從行動切換到桌面檢視的連結。

    ![檢視在桌面檢視中呈現的切換器](whats-new-in-aspnet-mvc-4/_static/image32.png "在桌面檢視中呈現的檢視切換程式")

    *在桌面檢視中呈現的檢視切換程式*
7. 再次切換至行動裝置的檢視，並瀏覽至<strong>有關</strong>頁面 (http://localhost[port] 首頁/有關)。 請注意，即使您沒有建立 About.Mobile.cshtml 檢視，[關於] 頁面會顯示使用行動裝置的配置 (\_Layout.Mobile.cshtml)。

    ![有關頁面](whats-new-in-aspnet-mvc-4/_static/image33.png "關於頁面")

    *有關頁面*
8. 最後，桌面的網頁瀏覽器中開啟網站。 請注意，沒有任何先前的更新已受到影響桌面檢視。

    ![PhotoGallery 桌面檢視](whats-new-in-aspnet-mvc-4/_static/image34.png "PhotoGallery 桌面檢視")

    *PhotoGallery 桌面檢視*

<a id="Task_6_-_Creating_New_Display_Modes"></a>
#### <a name="task-6---creating-new-display-modes"></a>工作 6-建立新的顯示模式

新的顯示模式功能可讓應用程式選取依產生要求的瀏覽器的檢視。 例如，如果桌面瀏覽器要求在首頁上，應用程式會傳回**Views\Home\Index.cshtml**範本。 然後，如果行動瀏覽器要求在首頁上，應用程式會傳回**Views\Home\Index.mobile.cshtml**範本。

在這項工作，您將建立的自訂版面配置的 iPhone 裝置，而且您必須以模擬來自 iPhone 裝置的要求。 若要這樣做，您可以使用任一 iPhone 模擬器/模擬器 (像是[電動行動模擬器](http://www.electricplum.com/)) 或修改使用者代理程式的附加元件的瀏覽器。 模擬在 iPhone 上 Safari 瀏覽器中設定的使用者代理字串的方式指示，請參閱[如何讓 Safari 假裝其 IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) David Alison 部落格中。

**請注意這項工作是選擇性，而且您可以繼續在實驗室，而不加以執行。**

1. 在 Visual Studio 中，按**SHIFT** + **F5**停止偵錯應用程式。
2. 開啟**Global.asax.cs**並加入下列 using 陳述式。


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample15.cs)]
~~~
3. 將下列反白顯示的程式碼加入至應用程式\_Start 方法。

    (程式碼片段- *ASP.NET MVC 4 實驗室-Ex03-iPhone DisplayMode*)


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample16.cs)]

You have registered a new **DefaultDisplayMode named &quot;iPhone&quot;**, within the static **DisplayModeProvider.Instance.Modes** static list, that will be matched against each incoming request. If the incoming request contains the string &quot;iPhone&quot;, ASP.NET MVC will find the views whose name contain the &quot;iPhone&quot; suffix. The 0 parameter indicates how specific is the new mode; for instance, this view is more specific than the general &quot;.mobile&quot; rule that matches requests from mobile devices.

After this code runs, when an iPhone browser generates a request, your application will use the **Views\Shared\\_Layout.iPhone.cshtml** layout you will create in the next steps.

> [!NOTE]
> This way of testing the request for iPhone has been simplified for demo purposes and might not work as expected for every iPhone user agent string (for example test is case sensitive).
~~~
4. 建立一份 **\_Layout.Mobile.cshtml**檔案**_layout.cshtml**資料夾，並重新命名複製到&quot;  **\_Layout.iPhone.csthml**&quot;.
5. 開啟 **\_Layout.iPhone.csthml**您在上一個步驟中建立。
6. 尋找 div 項目資料角色屬性設定為**頁面**並變更**資料佈景主題**屬性&quot; &quot;。


~~~
[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample17.cshtml)]

Now you have 3 layouts in your ASP.NET MVC 4 application:

1. **\_Layout.cshtml**: default layout used for desktop browsers.
2. **\_Layout.mobile.cshtml**: default layout used for mobile devices.
3. **\_Layout.iPhone.cshtml**: specific layout for iPhone devices, using a different color scheme to differentiate from \_Layout.mobile.cshtml.
~~~
7. 按**F5**執行應用程式，然後瀏覽的網站中**Windows Phone 模擬器**。
8. 開啟**iPhone 模擬器**(請參閱[旓紵 C](#AppendixC)如需有關如何安裝及設定 iPhone 模擬器指示)，並瀏覽至站台太。 請注意，每個電話使用特定範本。

    ![Using-different-views-for-each-mobile-device2](whats-new-in-aspnet-mvc-4/_static/image35.png)

    *使用每個行動裝置的不同檢視*

<a id="Exercise4"></a>

<a id="Exercise_4_Using_Asynchronous_Controllers"></a>
### <a name="exercise-4-using-asynchronous-controllers"></a>練習 4： 使用非同步控制器

Microsoft.NET Framework 4.5 引進了新的語言功能，在 C# 和 Visual Basic.NET 程式設計中非同步提供新的基礎架構。 此新基礎架構，會使非同步程式設計類似於-大約像是那樣-同步程式設計。 您現在已可使用 ASP.NET MVC 4 撰寫非同步動作方法**AsyncController**類別。 您可以使用非同步動作方法的長時間執行、 不受限於 CPU 的要求。 如此可避免在處理要求時執行工作的 Web 伺服器。 AsyncController 類別通常用於長時間執行的 Web 服務呼叫。

這個練習說明 ASP.NET MVC 4 中的非同步作業的基本概念。 如果您想深入探討，您可以查看下列文章： [[https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)

<a id="Task_1_-_Implementing_an_Asynchronous_Controller"></a>
#### <a name="task-1---implementing-an-asynchronous-controller"></a>工作 1-實作非同步控制器

1. 開啟**開始**方案位於**來源/Ex4 非同步處理/開始/**資料夾。 否則，您可能會繼續使用**結束**方案所完成的上一個練習中取得。

   1. 如果您開啟提供**開始**方案，您必須下載某些遺漏的 NuGet 套件才能繼續。 若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 封裝**。
   2. 在**管理 NuGet 封裝**] 對話方塊中，按一下 [**還原**才能下載遺漏的封裝。
   3. 最後，按一下 建置方案**建置** | **建置方案**。

      > [!NOTE]
      > 使用 NuGet 的優點之一是您不需要在專案中，所有的程式庫的出貨減少專案大小。 NuGet 的強大工具，請藉由指定封裝版本在 Packages.config 檔案中，您將會成功下載所有必要的程式庫第一次您執行專案。 這就是為什麼您必須從這個實驗室中開啟現有的方案後執行這些步驟。
2. 開啟**HomeController.cs**類別從**控制器**資料夾。
3. 加入下列 using 陳述式。


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample18.cs)]
~~~
4. 更新**HomeController**類別繼承自**AsyncController**。 衍生自 AsyncController 的控制站會啟用 ASP.NET 處理非同步要求，並仍然可以服務同步動作方法。


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample19.cs)]
~~~
5. 新增**非同步**關鍵字**索引**方法，並將其傳回型別**工作&lt;ActionResult&gt;**。


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample20.cs)]

> [!NOTE]
> The **async** keyword is one of the new keywords the .NET Framework 4.5 provides; it tells the compiler that this method contains asynchronous code. A **Task** object represents an asynchronous operation that may complete at some point in the future.
~~~
6. 取代**用戶端。GetAsync()**呼叫具有完整的非同步版本使用 await 關鍵字，如下所示。

    (程式碼片段- *ASP.NET MVC 4 實驗室-Ex04-GetAsync*)


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample21.cs)]

> [!NOTE]
> In the previous version, you were using the **Result** property from the **Task** object to block the thread until the result is returned (sync version).
> 
> Adding the **await** keyword tells the compiler to asynchronously wait for the task returned from the method call. This means that the rest of the code will be executed as a callback only after the awaited method completes. Another thing to notice is that you do not need to change your try-catch block in order to make this work: the exceptions that happen in background or in foreground will still be caught without any extra work using a handler provided by the framework.
~~~
7. 程式碼，以繼續新的程式碼行取代成如下所示的非同步實作

    (程式碼片段- *ASP.NET MVC 4 實驗室-Ex04-ReadAsStringAsync*)


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample22.cs)]
~~~
8. 執行應用程式。 您會發現沒有重大變更，但您的程式碼不會封鎖執行緒的執行緒集區進行更好的使用方式的伺服器資源，並改善效能。

    > [!NOTE]
    > 您可以進一步了解在實驗室中的新非同步程式設計功能&quot;**以 C# 和 Visual Basic.NET 4.5 中進行非同步程式設計**&quot;包含在 Visual Studio 訓練套件。

<a id="Task_2_-_Handling_Time-Outs_with_Cancellation_Tokens"></a>
#### <a name="task-2---handling-time-outs-with-cancellation-tokens"></a>工作 2-處理逾時，使用取消語彙基元

傳回工作執行個體的非同步動作方法也可支援逾時。 在這項工作，您將會更新索引方法的程式碼來處理逾時案例中使用取消語彙基元。

1. 返回 Visual Studio 並再按**SHIFT + F5**停止偵錯。
2. 加入下列 using 陳述式來**HomeController.cs**檔案。


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample23.cs)]
~~~
3. 更新要接收的索引動作**CancellationToken**引數。


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample24.cs)]
~~~
4. 更新**GetAsync**呼叫傳遞取消語彙基元。

    (程式碼片段- *CancellationToken 與 ASP.NET MVC 4 實驗室-Ex04-SendAsync*)


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample25.cs)]
~~~
5. 裝飾*索引*方法**AsyncTimeout**屬性設為 500 毫秒和**HandleError**屬性設定為處理**TaskCanceledException**重新導向至**TimedOut**檢視。

    (程式碼片段- *ASP.NET MVC 4 實驗室-Ex04-屬性*)


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample26.cs)]
~~~
6. 開啟**PhotoController**類別和更新**圖庫**延遲執行 1000年毫秒 （1 秒），以模擬長時間執行工作的方法。


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample27.cs)]
~~~
7. 開啟**Web.config**檔案，並新增下列項目，以啟用自訂錯誤。


~~~
[!code-xml[Main](whats-new-in-aspnet-mvc-4/samples/sample28.xml)]
~~~
8. 建立新的檢視中**_layout.cshtml**名為**TimedOut**並使用預設配置。 在 [方案總管] 中，以滑鼠右鍵按一下**_layout.cshtml**資料夾，然後選取**新增 |檢視**。

    ![使用每個行動裝置的不同檢視](whats-new-in-aspnet-mvc-4/_static/image36.png "使用每個行動裝置的不同檢視")

    *使用每個行動裝置的不同檢視*
9. 更新**TimedOut**檢視內容，如下所示。


~~~
[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample29.cshtml)]
~~~
10. 執行應用程式，並瀏覽至根 URL。 當您加入**Thread.Sleep** 1000年毫秒就會逾時錯誤，所產生**AsyncTimeout**屬性，並攔截的**HandleError**屬性。

    ![逾時例外狀況處理](whats-new-in-aspnet-mvc-4/_static/image37.png "逾時例外狀況處理")

    *逾時例外狀況處理*

> [!NOTE]
> 此外，您可以部署此應用程式以 Windows Azure Web Sites 下列[附錄 d： 發佈 ASP.NET MVC 4 應用程式使用 Web Deploy](#AppendixD)。


<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>總結

在這個實際操作實驗室，您所觀察到的一些新功能在 ASP.NET MVC 4 中駐留。 已經討論過下列概念：

- 利用 ASP.NET MVC 專案範本包括新的行動應用程式專案範本的增強功能
- 若要改善在行動裝置上的顯示使用 HTML5 檢視區屬性和 CSS 的媒體查詢
- 使用 jQuery Mobile 漸進式增強功能，以及建置觸控最佳化 web UI
- 建立行動裝置的特定檢視
- 使用檢視切換程式元件，將行動和桌面應用程式中的檢視之間切換
- 建立使用工作支援非同步控制器

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>附錄 a： 使用程式碼片段

程式碼片段，您會有您需要在您可以的所有程式碼。 實驗室文件會告訴您完全時您可以使用它們，如下圖所示。

![若要將程式碼插入您的專案中使用 Visual Studio 程式碼片段](whats-new-in-aspnet-mvc-4/_static/image38.png "使用 Visual Studio 程式碼片段，將程式碼插入您的專案")

*若要將程式碼插入您的專案中使用 Visual Studio 程式碼片段*

***若要加入的程式碼片段，使用鍵盤 （僅限 C#)***

1. 將游標放在您想要插入的程式碼。
2. 開始鍵入片段名稱 （不含空格或連字號）。
3. 觀察 IntelliSense 會顯示比對程式碼片段的名稱。
4. 選取正確的程式碼片段 （或直到選取整個程式碼片段名稱時，保留 輸入）。
5. 按下 Tab 鍵兩次的游標位置插入程式碼片段。

![開始輸入程式碼片段名稱](whats-new-in-aspnet-mvc-4/_static/image39.png "開始鍵入片段名稱")

*開始輸入程式碼片段名稱*

![按 Tab 鍵選取反白顯示的程式碼片段](whats-new-in-aspnet-mvc-4/_static/image40.png "按 Tab 鍵以選取反白顯示的程式碼片段")

*按 Tab 鍵選取反白顯示的程式碼片段*

![按 Tab 鍵一次程式碼片段就會擴展](whats-new-in-aspnet-mvc-4/_static/image41.png "按 Tab 鍵一次程式碼片段就會擴展")

*按 Tab 鍵一次程式碼片段就會擴展*

***若要加入的程式碼片段，使用滑鼠 （C#、 Visual Basic 和 XML）***

1. 以滑鼠右鍵按一下您要插入程式碼片段。
2. 選取**插入程式碼片段**後面**我的程式碼片段**。
3. 透過在按一下挑選清單中，從相關的程式碼片段。

![以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段](whats-new-in-aspnet-mvc-4/_static/image42.png "以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段")

*以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段*

![透過在按一下挑選清單中，從相關的程式碼片段](whats-new-in-aspnet-mvc-4/_static/image43.png "上它即可挑選清單中，從相關的程式碼片段")

*透過在按一下挑選清單中，從相關的程式碼片段*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>附錄 b： 安裝 Visual Studio Express 2012 for Web

您可以安裝**Microsoft Visual Studio Express 2012 for Web**或另一個&quot;Express&quot;版本使用**[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. 下列指示將引導您逐步完成安裝所需*Visual studio Express 2012 for Web*使用*Microsoft Web Platform Installer*。

1. 移至[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169)。 或者，如果您已安裝 Web Platform Installer，您可以開啟它，並搜尋產品&quot; <em>Visual Studio Express 2012 for Web 與 Windows Azure SDK</em>&quot;。
2. 按一下**立即安裝**。 如果您不需要**Web Platform Installer**您會重新導向至下載並安裝第一次。
3. 一次**Web Platform Installer**開啟時，按一下 **安裝**，啟動安裝程式。

    ![安裝 Visual Studio Express](whats-new-in-aspnet-mvc-4/_static/image44.png "安裝 Visual Studio Express")

    *安裝 Visual Studio Express*
4. 閱讀所有產品的授權和詞彙，然後按一下**我接受**才能繼續。

    ![接受授權條款](whats-new-in-aspnet-mvc-4/_static/image45.png)

    *接受授權條款*
5. 等待直到完成下載和安裝程序。

    ![安裝進度](whats-new-in-aspnet-mvc-4/_static/image46.png)

    *安裝進度*
6. 當安裝完成時，按一下**完成**。

    ![安裝已完成](whats-new-in-aspnet-mvc-4/_static/image47.png)

    *安裝已完成*
7. 按一下**結束**關閉 Web Platform Installer。
8. 若要開啟 Visual Studio Express for Web，請移至**啟動**畫面上，並開始書寫&quot; **VS Express**&quot;，然後按一下  **VS Express for Web**並排顯示。

    ![VS Express for Web 方塊](whats-new-in-aspnet-mvc-4/_static/image48.png)

    *VS Express for Web 方塊*

<a id="AppendixC"></a>

<a id="Appendix_C_Installing_WebMatrix_2_and_iPhone_Simulator"></a>
## <a name="appendix-c-installing-webmatrix-2-and-iphone-simulator"></a>附錄 c： 安裝 WebMatrix 2 和 iPhone 模擬器

在模擬的 iPhone 裝置中執行您的網站中，您可以使用 WebMatrix 延伸&quot;電動適用於 iPhone 的行動模擬器&quot;。 此外，您可以設定要從 Visual Studio 2012 中執行的模擬器相同的副檔名。

<a id="ApxCTask1"></a>

<a id="Task_1_-_Installing_WebMatrix_2"></a>
#### <a name="task-1---installing-webmatrix-2"></a>工作 1-安裝 WebMatrix 2

1. 移至[ [ https://go.microsoft.com/?linkid=9809776 ](https://go.microsoft.com/?linkid=9809776) ](https://go.microsoft.com/?linkid=9810169)。 或者，如果您已安裝 Web Platform Installer，您可以開啟它，並搜尋產品&quot; <em>WebMatrix 2</em>&quot;。
2. 按一下**立即安裝**。 如果您不需要**Web Platform Installer**您會重新導向至下載並安裝第一次。
3. 一次**Web Platform Installer**開啟時，按一下 **安裝**，啟動安裝程式。

    ![安裝 WebMatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "安裝 WebMatrix 2")

    *安裝 WebMatrix 2*
4. 閱讀所有產品的授權和詞彙，然後按一下**我接受**才能繼續。

    ![接受授權條款](whats-new-in-aspnet-mvc-4/_static/image50.png "接受授權條款")

    *接受授權條款*
5. 等待直到完成下載和安裝程序。

    ![安裝進度](whats-new-in-aspnet-mvc-4/_static/image51.png "安裝進度")

    *安裝進度*
6. 當安裝完成時，按一下**完成**。

    ![安裝已完成](whats-new-in-aspnet-mvc-4/_static/image52.png "安裝已完成")

    *安裝已完成*
7. 按一下**結束**關閉 Web Platform Installer。

<a id="ApxCTask2"></a>

<a id="Task_2_-_Installing_the_iPhone_Simulator_Extension"></a>
#### <a name="task-2---installing-the-iphone-simulator-extension"></a>工作 2-安裝在 iPhone 模擬器延伸模組

1. 執行**WebMatrix**和開啟任何現有的網站或另外新建一個。
2. 按一下**執行**按鈕**首頁**功能區，然後選取**新增**。

    ![加入新的 WebMatrix 延伸](whats-new-in-aspnet-mvc-4/_static/image53.png "加入新的 WebMatrix 延伸模組")

    *加入新的 WebMatrix 延伸模組*
3. 選取**iPhone 模擬器**按一下**安裝**。

    ![瀏覽 WebMatrix 延伸](whats-new-in-aspnet-mvc-4/_static/image54.png "瀏覽 WebMatrix 延伸模組")

    *瀏覽 WebMatrix 延伸模組*
4. 封裝的詳細資訊，請按一下**安裝**繼續擴充功能安裝。

    ![iPhone 模擬器延伸](whats-new-in-aspnet-mvc-4/_static/image55.png "iPhone 模擬器延伸模組")

    *iPhone 模擬器延伸模組*
5. 閱讀並接受使用者授權合約的延伸模組。

    ![WebMatrix 延伸 EULA](whats-new-in-aspnet-mvc-4/_static/image56.png "WebMatrix 延伸使用者授權合約")

    *WebMatrix 延伸使用者授權合約*
6. 現在，您可以執行您的網站從 WebMatrix 使用 iPhone 模擬器選項。

    ![使用 iPhone 執行](whats-new-in-aspnet-mvc-4/_static/image57.png "執行使用 iPhone")

    *使用 iPhone 執行*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Configuring_Visual_Studio_2012_to_run_iPhone_Simulator"></a>
#### <a name="task-3---configuring-visual-studio-2012-to-run-iphone-simulator"></a>工作 3-設定要執行 iPhone 模擬器的 Visual Studio 2012

1. 開啟**Visual Studio 2012**和開啟任何網站，或建立新的專案。
2. 按一下向下的箭號，從 [執行] 按鈕，然後選取**瀏覽包含**。

    ![瀏覽包含](whats-new-in-aspnet-mvc-4/_static/image58.png "瀏覽方式")

    *瀏覽方式*
3. 在&quot;瀏覽&quot;] 對話方塊中，按一下 [**新增**。
4. 在&quot;新增程式&quot; 對話方塊中，使用下列值：

   - <strong>程式</strong>: C:\Users\*{CurrentUser}<em>\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe * （據以更新路徑）。</em>
   - **引數**: &quot;1&quot;
   - **易記名稱**: iPhone 模擬器

     ![加入程式](whats-new-in-aspnet-mvc-4/_static/image59.png "加入程式")

     *新增程式，瀏覽包含*
5. 按一下**確定**並關閉對話方塊。
6. 現在，您就能夠在 iPhone 模擬器中執行您 Web 應用程式，從 Visual Studio 2012。

    ![瀏覽與 iPhone 模擬器](whats-new-in-aspnet-mvc-4/_static/image60.png "瀏覽與 iPhone 模擬器")

    *瀏覽與 iPhone 模擬器*

<a id="AppendixD"></a>

<a id="Appendix_D_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-d-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>附錄 d： 發佈 ASP.NET MVC 4 應用程式使用 Web Deploy

此附錄將說明如何從 Windows Azure 管理入口網站建立新的網站和發行您取得依照實驗室中，利用 Web Deploy 發行功能的 Windows Azure 所提供的應用程式。

<a id="ApxDTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>工作 1-建立新的網站，從 Windows Azure 入口網站

1. 移至[Windows Azure 管理入口網站](https://manage.windowsazure.com/)並使用與您訂用帳戶相關聯的 Microsoft 認證登入。

    > [!NOTE]
    > 與 Windows Azure 中，您可以免費裝載 10 個 ASP.NET 網站並再擴充隨流量成長。 您可以註冊[這裡](http://aka.ms/aspnet-hol-azure)。

    ![登入 Windows Azure 入口網站](whats-new-in-aspnet-mvc-4/_static/image61.png "登入 Windows Azure 入口網站")

    *登入 Windows Azure 管理入口網站*
2. 按一下**新增**命令列上。

    ![建立新的網站](whats-new-in-aspnet-mvc-4/_static/image62.png "建立新的網站")

    *建立新的網站*
3. 按一下**計算** | **網站**。 然後選取**快速建立**選項。 新的網站上提供可用的 URL，然後按一下**建立網站**。

    > [!NOTE]
    > Windows Azure 網站是您可以控制和管理在雲端中執行的 web 應用程式的主機。 快速建立 選項可讓您部署已完成的 web 應用程式至 Windows Azure 網站從入口網站外部。 它不包含設定資料庫的步驟。

    ![建立新的網站使用 快速建立](whats-new-in-aspnet-mvc-4/_static/image63.png "建立新的網站使用 快速建立")

    *建立新的網站使用 快速建立*
4. 等候新**網站**建立。
5. 建立網站之後按一下底下的連結**URL**資料行。 請檢查新的網站運作。

    ![瀏覽至新的網站](whats-new-in-aspnet-mvc-4/_static/image64.png "瀏覽至新的網站")

    *瀏覽至新的網站*

    ![執行網站](whats-new-in-aspnet-mvc-4/_static/image65.png "執行的網站")

    *執行的網站*
6. 返回入口網站並按一下底下的網站名稱**名稱**資料行會顯示 [管理] 頁面。

    ![開啟網站管理頁面](whats-new-in-aspnet-mvc-4/_static/image66.png "開啟網站管理頁面")

    *開啟網站管理頁面*
7. 在**儀表板**頁面的 **快速概覽**區段中，按一下**下載發行設定檔**連結。

    > [!NOTE]
    > *發行設定檔*包含所有 web 應用程式發行至 Windows Azure 網站的每個已啟用的發行方法所需的資訊。 發行設定檔包含 Url、 使用者認證和資料庫連接，並對每個端點已啟用的發行集的方法進行驗證所需的字串。 **Microsoft WebMatrix 2**， **Microsoft Visual Studio Express for Web**和**Microsoft Visual Studio 2012**支援讀取發行設定檔自動將這些程式的組態web 應用程式發行至 Windows Azure 網站。

    ![下載網站發行設定檔](whats-new-in-aspnet-mvc-4/_static/image67.png "下載網站發行設定檔")

    *下載網站發行設定檔*
8. 下載發行設定檔至已知位置。 進一步在這個練習中，您會看到如何使用這個檔案從 Visual Studio 將 web 應用程式至 Windows Azure 網站發行。

    ![儲存發行設定檔](whats-new-in-aspnet-mvc-4/_static/image68.png "儲存發行設定檔")

    *儲存發行設定檔*

<a id="ApxDTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>工作 2-設定資料庫伺服器

如果您的應用程式會使用 SQL Server 資料庫，您必須建立 SQL Database 伺服器。 如果您想要部署簡單的應用程式不會使用 SQL Server，您可能會略過這項工作。

1. 您將需要 SQL Database 伺服器來儲存應用程式資料庫。 您可以從您的訂用帳戶，在 Windows Azure 管理入口網站中檢視 SQL Database 伺服器**Sql 資料庫** | **伺服器** | **伺服器儀表板**。 如果您沒有建立伺服器，您可以建立一個使用**新增**命令列上的按鈕。 請注意**伺服器名稱和 URL、 系統管理員身分登入名稱和密碼**，因為您將在下一個工作中使用它們。 並未建立資料庫，因為它會在後續階段中建立。

    ![SQL Database 伺服器儀表板](whats-new-in-aspnet-mvc-4/_static/image69.png "SQL Database 伺服器儀表板")

    *SQL Database 伺服器儀表板*
2. 下一項工作在您將測試資料庫連接，從 Visual Studio，因此您需要在伺服器的清單中包含您的本機 IP 位址**允許的 IP 位址**。 若要這樣做，請按一下**設定**，選取 [IP 位址從**目前的用戶端 IP 位址**並將它貼上**起始 IP 位址**和**結束 IP 位址**文字方塊，然後按一下![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png) ] 按鈕。

    ![加入用戶端 IP 位址](whats-new-in-aspnet-mvc-4/_static/image71.png)

    *加入用戶端 IP 位址*
3. 一次**用戶端 IP 位址**新增至允許的 IP 位址清單中，按一下**儲存**來確認變更。

    ![確認變更](whats-new-in-aspnet-mvc-4/_static/image72.png)

    *確認變更*

<a id="ApxDTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>工作 3-發行 ASP.NET MVC 4 應用程式使用 Web Deploy

1. 返回至 ASP.NET MVC 4 方案。 在**方案總管 中**，以滑鼠右鍵按一下網站專案，然後選取**發行**。

    ![發行應用程式](whats-new-in-aspnet-mvc-4/_static/image73.png "發行應用程式")

    *發行網站*
2. 匯入發行設定檔儲存在第一項工作中。

    ![匯入發行設定檔](whats-new-in-aspnet-mvc-4/_static/image74.png "匯入發行設定檔")

    *匯入發行設定檔*
3. 按一下**驗證連線**。 驗證完成後按一下**下一步**。

    > [!NOTE]
    > 一旦您看到 [驗證連線] 按鈕旁邊會出現綠色核取記號完成驗證。

    ![驗證連線](whats-new-in-aspnet-mvc-4/_static/image75.png "驗證連線")

    *驗證連接*
4. 在**設定**頁面的 **資料庫**區段中，按一下您的資料庫連接文字方塊旁邊的按鈕 (也就是**DefaultConnection**)。

    ![Web 部署設定](whats-new-in-aspnet-mvc-4/_static/image76.png "Web 部署設定")

    *Web 部署設定*
5. 設定資料庫連接，如下所示：

   - 在**伺服器名稱**您 SQL Database 伺服器 URL 使用下列方法類型*tcp:*前置詞。
   - 在**使用者名**輸入您的伺服器系統管理員身分登入名稱。
   - 在**密碼**輸入伺服器系統管理員身分登入密碼。
   - 輸入新的資料庫名稱，例如： *MVC4SampleDB*。

     ![設定目的地連接字串](whats-new-in-aspnet-mvc-4/_static/image77.png "設定目的地連接字串")

     *設定目的地連接字串*
6. 然後按一下 [確定]。  當系統提示您建立資料庫依序按一下**是**。

    ![建立資料庫](whats-new-in-aspnet-mvc-4/_static/image78.png "建立的資料庫字串")

    *建立資料庫*
7. 您將用來連接到在 Windows Azure SQL Database 的連接字串會顯示在預設連接文字方塊中。 然後按 [下一步] 。

    ![連接字串指向 SQL Database](whats-new-in-aspnet-mvc-4/_static/image79.png "連接字串指向 SQL 資料庫")

    *連接字串指向 SQL 資料庫*
8. 在**預覽**頁面上，按一下**發行**。

    ![Web 應用程式發行](whats-new-in-aspnet-mvc-4/_static/image80.png "發行 web 應用程式")

    *發行 web 應用程式*
9. 一旦在發佈程序完成時，預設瀏覽器會開啟已發行的網站。

    ![應用程式發行至 Windows Azure](whats-new-in-aspnet-mvc-4/_static/image81.png "應用程式發行至 Windows Azure")

    *應用程式發行至 Windows Azure*
