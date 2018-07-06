---
uid: mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
title: 重複使用使用主版頁面及部分的 UI |Microsoft Docs
author: microsoft
description: 步驟 7 會探討在我們的檢視範本，以排除程式碼重複，使用部分檢視範本和主版頁面的方式，我們可以套用 ' DRY 準則 '。
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: d4243a4a-e91c-4116-9ae0-5c08e5285677
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
msc.type: authoredcontent
ms.openlocfilehash: 961378d6d710c678a0cd223a5c31544f014ace7f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839590"
---
<a name="re-use-ui-using-master-pages-and-partials"></a>重複使用使用主版頁面及部分的 UI
====================
by [Microsoft](https://github.com/microsoft)

[下載 PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 這是一套免費的步驟 7 ["NerdDinner 」 應用程式教學課程](introducing-the-nerddinner-tutorial.md)，逐步解說如何建置一個小型的但在完成時，使用 ASP.NET MVC 1 的 web 應用程式。
> 
> 步驟 7 會探討在我們的檢視範本，以排除程式碼重複，使用部分檢視範本和主版頁面的方式，我們可以套用 「 DRY 準則 」。
> 
> 如果您使用 ASP.NET MVC 3，我們建議您遵循[取得開始使用 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或是[MVC Music 市集](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教學課程。


## <a name="nerddinner-step-7-partials-and-master-pages"></a>NerdDinner 步驟 7： 部分和主版頁面

ASP.NET MVC 應用了絕佳的設計原理的其中一個是 「 執行不自行重複 」 原則 （通常稱為 「 試"）。 DRY 的設計可協助消除重複的程式碼和邏輯，最終讓應用程式更快速地建置和維護變得更容易。

我們已經看過在數個我們 NerdDinner 的案例中套用 DRY 準則。 一些範例： 我們的模型層，讓它能夠跨這兩個編輯強制執行，並建立控制器; 中的案例中實作我們的驗證邏輯我們要重複使用的編輯、 詳細資料和刪除動作方法中，跨"NotFound"檢視範本我們使用我們的檢視範本，就不需要明確指定名稱，當我們呼叫 View() 協助程式方法; 中的慣例-命名模式和我們會重複使用這兩個編輯 DinnerFormViewModel 類別，並建立動作的案例。

現在來看看我們可以套用 「 DRY 準則 」 的方式在我們的檢視範本，以及消除那里程式碼重複。

### <a name="re-visiting-our-edit-and-create-view-templates"></a>重新瀏覽我們的編輯和建立檢視範本

目前我們會使用兩個不同的檢視範本 –"Edit.aspx 」 和 「 Create.aspx"– 顯示 Dinner 表單 UI。 快速的視覺比較，其中反白顯示它們是類似。 以下是建立表單的外觀：

![](re-use-ui-using-master-pages-and-partials/_static/image1.png)

此外，以下是我們的 [編輯] 表單的外觀：

![](re-use-ui-using-master-pages-and-partials/_static/image2.png)

不是大的差別有這樣做嗎？ 以外的標題和標頭的文字，表單版面配置和輸入控制項都相同。

如果我們開啟 「 Edit.aspx"和"Create.aspx 」 檢視範本，我們會發現它們包含相同的表單版面配置和輸入控制項的程式碼。 這種重複狀況，表示我們最後會有進行變更，我們導入，或變更新的 Dinner 屬性--也就是不好每當，兩次。

### <a name="using-partial-view-templates"></a>使用部分檢視範本

ASP.NET MVC 支援定義可用來封裝頁面子部分檢視轉譯邏輯的 「 部分檢視 」 範本的能力。 「 部分 」 提供實用的方式，來定義檢視轉譯邏輯一次，然後再重新使用它在多個位置整個應用程式。

為了協助 「 試啟動 」 我們 Edit.aspx 和 Create.aspx 檢視範本重複資料刪除，我們可以建立名為"DinnerForm.ascx 」 封裝的表單版面配置和輸入的項目，同時的部分檢視範本。 我們這樣我們/檢視/Dinners 目錄上按一下滑鼠右鍵，然後選擇 「 Add-&gt;檢視 」 功能表命令：

![](re-use-ui-using-master-pages-and-partials/_static/image3.png)

這會顯示 [新增檢視] 對話方塊。 我們將會命名為我們想要建立 「 DinnerForm"，選取 [建立部分檢視] 的核取方塊在對話方塊中，並指出，我們會將它傳遞 DinnerFormViewModel 類別的新檢視：

![](re-use-ui-using-master-pages-and-partials/_static/image4.png)

當我們按一下 [新增] 按鈕時，Visual Studio 會"\Views\Dinners 」 目錄內就讓我們建立新的 「 DinnerForm.ascx 」 檢視範本。

我們可以接著複製/貼上重複的表單版面配置 / 我們新的 「 DinnerForm.ascx 」 部分檢視範本中所輸入的控制碼，從我們 Edit.aspx/ Create.aspx 檢視範本：

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample1.aspx)]

然後，我們可以更新我們的編輯和建立檢視範本呼叫 DinnerForm 部分範本，並排除與表單重複。 我們可以執行這項操作所呼叫的 Html.RenderPartial("DinnerForm") 我們檢視的範本內：

##### <a name="createaspx"></a>Create.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample2.aspx)]

##### <a name="editaspx"></a>Edit.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample3.aspx)]

您可以明確地限定時呼叫 Html.RenderPartial 所要的部分範本的路徑 (例如: ~ Views/Dinners/DinnerForm.ascx")。 在上述程式碼，不過，我們利用 ASP.NET MVC 中，以慣例為基礎的命名模式並且僅做為呈現部分的名稱中指定 」 DinnerForm"。 當我們執行此動作 ASP.NET MVC 會尋找以慣例為基礎的檢視目錄中的第一個 （這會是/檢視/Dinners DinnersController)。 如果找不到部分範本那里它會接著尋找它 /Views/Shared 目錄中。

Html.RenderPartial() 呼叫時與部分檢視的名稱，ASP.NET MVC 會將傳遞給部分檢視相同模型和 ViewData 字典所使用的物件呼叫的檢視範本。 或者，有多載可讓您傳遞的替代模型物件和/或要使用的部分檢視的 ViewData 字典 Html.RenderPartial() 版本。 這非常有用的情況下，您只想將完整的模型/ViewModel 的子集。

| **側邊的主題內容： 為什麼&lt;%%&gt;而不是&lt;%= %&gt;嗎？** |
| --- |
| 您可能已經注意到上述程式碼難以察覺的事情之一是，我們會使用&lt;%%&gt;而不是封鎖&lt;%= %&gt;封鎖呼叫 Html.RenderPartial() 時。 &lt;%= %&gt;在 ASP.NET 中的區塊表示開發人員想要呈現指定的值 (例如： &lt;%="Hello"%&gt;會轉譯"Hello")。 &lt;%%&gt;區塊，改為指出的開發人員想要執行程式碼，而任何呈現其中的輸出必須明確 (例如： &lt;%response.write("hello")&gt;。 我們將使用的原因&lt;%%&gt;上述 Html.RenderPartial 程式碼區塊是因為 Html.RenderPartial() 方法不會傳回字串，並改為將輸出直接向呼叫端檢視範本內容的輸出資料流。 它會基於效能效率，並藉由讓它可避免需要建立一個 （可能會有極的大型） 的暫存字串物件。 這樣可減少記憶體使用量，並改善整體的應用程式輸送量。 一個常見的錯誤時使用 Html.RenderPartial() 忘記內時，呼叫的結尾加入分號&lt;%%&gt;區塊。 比方說，這段程式碼會造成編譯器錯誤： &lt;%html.renderpartial("dinnerform")&gt;相反地，您需要撰寫： &lt;%html.renderpartial("dinnerform"); %&gt;這是因為&lt;%%&gt;區塊是獨立的程式碼陳述式，並且使用 C# 程式碼陳述式必須以分號結束時。 |

### <a name="using-partial-view-templates-to-clarify-code"></a>若要釐清程式碼中使用部分檢視範本

我們建立 「 DinnerForm 」 部分檢視範本，以避免複製多個位置中的檢視轉譯邏輯。 這是最常見的原因，若要建立部分檢視範本。

有時候它仍然可以合理地建立部分檢視，即使他們只會在呼叫在單一位置。 非常複雜的檢視範本很容易就會更容易閱讀時擷取和分割成一或多也名為樣板部分其檢視呈現邏輯。

例如，請考慮下列程式碼片段從 Site.master 檔案，在我們的專案 （這我們將查看短時間內）。 程式碼是相當簡單讀取 – 這是因為頂端的 [邏輯，以顯示登入/登出] 連結時，才在畫面右邊封裝在"LogOnUserControl 」 部分：

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample4.aspx)]

每當您發現自己知道嘗試了解檢視範本內的 html/程式碼標記，請考慮是否就不能更清楚的某些部分擷取及重構為妥善具名的部分檢視。

### <a name="master-pages"></a>主版頁面

除了支援部分檢視，ASP.NET MVC 也支援建立可用來定義一般版面配置和站台的最上層 html 的 「 主版頁面 」 範本的能力。 內容控制項則可以新增至主版頁面，即可識別可取代的區域，可以覆寫或 「 填入 「 檢視的預留位置。 這提供非常有效 （且 DRY） 的方式，將整個應用程式的常見的版面配置。

根據預設，新的 ASP.NET MVC 專案會有自動新增至它們的主版頁面範本。 「 Site.master"及生活 \Views\Shared\ 資料夾內名為此主版頁面：

![](re-use-ui-using-master-pages-and-partials/_static/image5.png)

預設 Site.master 檔如下所示。 它會定義外部站台，以及在頂端導覽功能表的 html。 它包含兩個可取代內容預留位置控制項 – 一個標題，而另一個則用於其中應該取代主頁面的內容：

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample5.aspx)]

我們建立我們 NerdDinner 的應用程式 （"List"、"Details"、 「 編輯 」、 「 建立 」、"NotFound"等等） 的檢視範本的所有已取決於此 Site.master 範本。 這經由 「 MasterPageFile"屬性，依預設加入至頂端&lt;%@ %頁&gt;指示詞，當我們建立我們使用 [新增檢視] 對話方塊的檢視：

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample6.aspx)]

意思就是說，我們可以變更 Site.master 內容，而且有所做的變更會自動套用與我們呈現任何我們檢視範本時使用。

讓我們更新我們 Site.master 標頭區段，使我們的應用程式的標頭 」 NerdDinner"，而不是 「 我的 MVC 應用程式 」。 我們也更新我們瀏覽功能表，讓第一個索引標籤是 「 尋找 Dinner"（由 HomeController 的 index （） 動作方法），並讓我們加入新的索引標籤，稱為 「 主機 Dinner 」 （DinnersController create （） 動作方法處理）：

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample7.aspx)]

當我們儲存 Site.master 檔並重新整理瀏覽器就會看到我們的標頭變更顯現到我們的應用程式內的所有檢視。 例如: 

![](re-use-ui-using-master-pages-and-partials/_static/image6.png)

與 */Dinners/編輯 / [id]* URL:

![](re-use-ui-using-master-pages-and-partials/_static/image7.png)

### <a name="next-step"></a>下一個步驟

部分及主版頁面提供極具彈性的選項，可讓您完全組織檢視。 您會發現這些報告可協助您避免重複的檢視內容 / 程式碼，並讓您更容易閱讀和維護您的檢視範本。

讓我們現在再次瀏覽我們稍早建立的清單案例，並啟用可調整的分頁支援。

> [!div class="step-by-step"]
> [上一頁](use-viewdata-and-implement-viewmodel-classes.md)
> [下一頁](implement-efficient-data-paging.md)
