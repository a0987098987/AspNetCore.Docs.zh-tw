---
uid: mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
title: "重複使用 UI 使用主版頁面和 Partials |Microsoft 文件"
author: microsoft
description: "步驟 7 會查看我們排除程式碼重複，使用部分檢視的範本和主版頁面的檢視範本內的方式，我們可以使用最低 ' 乾 '。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: d4243a4a-e91c-4116-9ae0-5c08e5285677
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
msc.type: authoredcontent
ms.openlocfilehash: c42cd6aca40b08a9f8461532fbfd0589901b64ad
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="re-use-ui-using-master-pages-and-partials"></a>重複使用使用主版頁面和 Partials UI
====================
由[Microsoft](https://github.com/microsoft)

[下載 PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 這是一套免費的步驟 7 ["NerdDinner 」 應用程式教學課程](introducing-the-nerddinner-tutorial.md)，會逐步引導式如何建置小，但是完成，web 應用程式使用 ASP.NET MVC 1。
> 
> 步驟 7 會查看我們排除程式碼重複，使用部分檢視的範本和主版頁面的檢視範本內的方式，我們可以使用最低"乾"。
> 
> 如果您使用 ASP.NET MVC 3，我們建議您遵循[取得啟動與 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教學課程。


## <a name="nerddinner-step-7-partials-and-master-pages"></a>NerdDinner 步驟 7: Partials 和主版頁面

ASP.NET MVC 滿足設計原理的其中一個是 「 沒有不重複自行 」 原則 （通常稱為 「 乾"）。 乾設計可協助您排除重複的程式碼和邏輯，最終讓應用程式更快、 更容易維護。

我們已經看過乾幾個我們 NerdDinner 案例中套用的原則。 一些範例： 我們驗證邏輯中我們讓它能夠跨兩個編輯強制執行，並建立案例，我們控制器; 中的模型層實作我們重新跨編輯、 詳細資料和 Delete 動作方法; 使用"NotFound"檢視範本我們使用慣例的命名模式我們檢視範本，就不需要明確指定名稱，當我們呼叫 View() helper 方法。我們要重複使用這兩個編輯 DinnerFormViewModel 類別，並建立動作的案例。

我們現在看我們可以套用 「 乾原則 」 的方式排除程式碼重複那里以及我們檢視範本中。

### <a name="re-visiting-our-edit-and-create-view-templates"></a>重新瀏覽我們的編輯和建立檢視範本

目前我們使用兩個不同的檢視範本 –"Edit.aspx"和"Create.aspx"– 來顯示 Dinner 表單 UI。 其中的快速視覺比較反白顯示它們是類似。 以下是建立表單的外觀：

![](re-use-ui-using-master-pages-and-partials/_static/image1.png)

以下是我們的 [編輯] 表單的外觀：

![](re-use-ui-using-master-pages-and-partials/_static/image2.png)

不是多差異是否有？ 標題、 標頭文字以外表單版面配置和輸入控制項都相同。

如果我們開啟 「 Edit.aspx"和"Create.aspx 」 檢視範本，我們會發現它們包含相同的表單版面配置和輸入控制項的程式碼。 這種重複狀況表示我們最後會有進行變更，我們導入，或變更新 Dinner 屬性-不是好每當兩次。

### <a name="using-partial-view-templates"></a>使用部分檢視範本

ASP.NET MVC 支援定義可以用來封裝頁面子部分檢視轉譯邏輯的 「 部分檢視 」 範本的能力。 "Partials 」 提供有用的方式來定義檢視轉譯邏輯一次，然後再重新使用它在多個位置整個應用程式。

若要協助"乾總 「 我們 Edit.aspx 及 Create.aspx 檢視範本重複，我們可以建立名為"DinnerForm.ascx 」 封裝表單的版面配置和輸入的項目，同時的部分檢視範本。 將執行此作業，我們/檢視表/Dinners 目錄上按一下滑鼠右鍵，然後選擇 「 加入-&gt;檢視 」 功能表命令：

![](re-use-ui-using-master-pages-and-partials/_static/image3.png)

這會顯示 [新增檢視] 對話方塊。 我們將我們想要建立 「 DinnerForm 」，選取在對話方塊中，「 建立部分檢視 」 核取方塊，並指出，我們會將它傳遞 DinnerFormViewModel 類別的新檢視的名稱：

![](re-use-ui-using-master-pages-and-partials/_static/image4.png)

當我們按一下 [新增] 按鈕時，Visual Studio 將"\Views\Dinners"目錄中就讓我們建立新的"DinnerForm.ascx 」 檢視範本。

我們可以再複製/貼上重複的表單配置 / 將我們新的 「 DinnerForm.ascx"部分檢視範本輸入控制項從我們 Edit.aspx/ Create.aspx 檢視範本的程式碼：

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample1.aspx)]

然後，我們可以更新呼叫 DinnerForm 部分範本，並排除表單重複我們編輯和建立檢視範本。 我們這樣可以呼叫 Html.RenderPartial("DinnerForm") 我們檢視的範本內：

##### <a name="createaspx"></a>Create.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample2.aspx)]

##### <a name="editaspx"></a>Edit.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample3.aspx)]

您可以明確限定您想要呼叫 Html.RenderPartial 部分範本的路徑 (例如: ~ Views/Dinners/DinnerForm.ascx")。 在上面的程式碼，不過，我們會運用 ASP.NET MVC 中，以慣例為基礎的命名模式，僅指定"DinnerForm"做為呈現部分的名稱。 當執行此作業，ASP.NET MVC 會尋找以慣例為基礎的檢視目錄中的第一個 （這會是/檢視表/Dinners DinnersController)。 如果找不到部分樣板那里它會接著尋找它 /Views/Shared 目錄中。

Html.RenderPartial() 部分檢視的名稱呼叫時，ASP.NET MVC 會傳遞至部分檢視相同模型和別的 ViewData 字典所使用的物件呼叫檢視範本。 或者，有多載可讓您將其他模型物件和 （或） 要使用的部分檢視的別的 ViewData 字典 Html.RenderPartial() 版本。 這是適用於您只想傳遞完整的模型/ViewModel 子集的案例。

| **側邊主題內容： 為什麼&lt;%%&gt;而不是&lt;%= %&gt;嗎？** |
| --- |
| 您可能已注意到上述程式碼難以察覺的事情之一是我們使用&lt;%%&gt;而不是封鎖&lt;%= %&gt;封鎖呼叫 Html.RenderPartial() 時。 &lt;%= %&gt; ASP.NET 中的區塊，表示開發人員要呈現指定的值 (例如： &lt;%="Hello"%&gt;呈現"Hello")。 &lt;%%&gt;區塊而是表示，開發人員想要執行程式碼，且任何轉譯輸出中的必須進行明確 (例如： &lt;%response.write("hello")&gt;。 我們將使用的原因&lt;%%&gt;具有上述我們 Html.RenderPartial 程式碼區塊是因為 Html.RenderPartial() 方法不會傳回字串，並改為將輸出直接要呼叫的檢視表範本的內容的輸出資料流。 它會基於效能的效率，並藉由讓它可避免需要建立暫存的 （可能會有極的大型） 字串物件。 這會減少記憶體使用量，並改善整體應用程式輸送量。 一個常見的錯誤時使用 Html.RenderPartial() 忘記內時，呼叫的結尾加入分號&lt;%%&gt;區塊。 例如，此程式碼會造成編譯器錯誤： &lt;%html.renderpartial("dinnerform")&gt;相反地，您需要撰寫： &lt;%html.renderpartial("dinnerform"); %&gt;這是因為&lt;%%&gt;區塊是獨立的程式碼陳述式，並使用 C# 程式碼必須以分號結束陳述式時。 |

### <a name="using-partial-view-templates-to-clarify-code"></a>若要釐清程式碼使用的部分檢視範本

我們建立 「 DinnerForm"部分檢視範本，以避免重複的多個位置中的檢視轉譯邏輯。 這是要建立部分檢視範本的最常見原因。

有時仍合理建立部分檢視，即使它們只會呼叫在單一位置。 非常複雜的檢視範本很容易就會更容易閱讀時擷取和分割成一個或多也名為樣板部分其檢視呈現邏輯。

例如，請考慮下列程式碼片段從 Site.master 檔我們專案中 （我們將會尋找在短時間內）。 程式碼是相當直截了當讀取 – 這是因為頂端] 連結以顯示 [登入/登出邏輯螢幕右邊封裝在 「 LogOnUserControl"部分：

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample4.aspx)]

每當您發現自己混淆嘗試了解檢視範本內的 html/程式碼標記，請考慮是否也不是更清楚，如果某些從中擷取及重構為妥善命名的部分檢視。

### <a name="master-pages"></a>主版頁面

除了支援部分檢視，ASP.NET MVC 也支援建立可用來定義一般版面配置和站台的最上層 html 的"master page"範本的能力。 內容的預留位置然後可以控制項加入至主版頁面會識別出可取代可以覆寫或 「 填入 」 檢視的區域。 這提供非常有效率 （且乾） 方法，將整個應用程式套用一般的版面配置。

根據預設，新的 ASP.NET MVC 專案已自動加入至它們的主版頁面範本。 「 Site.master"和生活 \Views\Shared\ 資料夾內名為此主版頁面：

![](re-use-ui-using-master-pages-and-partials/_static/image5.png)

預設 Site.master 檔看起來像下面。 它會定義外部站台，以及在頂端導覽功能表的 html。 它包含兩個可取代內容預留位置控制項 – 個標題，而另一個則用於主頁面的內容應該取代其中一個：

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample5.aspx)]

我們已為我們 NerdDinner 的應用程式 （"List"、 詳細資料 」、 「 編輯 」、 「 建立 」、"NotFound"等等） 建立的檢視範本的所有已根據此 Site.master 範本。 這表示透過 「 MasterPageFile"屬性，依預設加入至頂端&lt;%@ 頁面 %&gt;當我們建立我們使用 [新增檢視] 對話方塊的檢視時的指示詞：

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample6.aspx)]

這表示是，我們可以變更 Site.master 內容，而且有所做的變更會自動套用與我們轉譯任何我們檢視的範本時使用。

讓我們來更新我們 Site.master 標頭區段，讓我們的應用程式的標頭是"NerdDinner"，而不是 「 我的 MVC 應用程式 」。 我們也更新我們的導覽功能表上，因此第一個索引標籤 」 尋找 Dinner"（由 HomeController index 動作方法處理），且讓我們加入新的索引標籤，稱為 「 主機 Dinner"（由 DinnersController create （） 動作方法處理）：

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample7.aspx)]

當我們儲存 Site.master 檔並重新整理瀏覽器我們會看到我們標頭變更顯示跨應用程式中的所有檢視。 例如: 

![](re-use-ui-using-master-pages-and-partials/_static/image6.png)

與*/Dinners/編輯 / [id]* URL:

![](re-use-ui-using-master-pages-and-partials/_static/image7.png)

### <a name="next-step"></a>下一個步驟

Partials 和主版頁面提供富彈性，可讓您完全組織檢視的選項。 您可以找到還可協助您避免重複的檢視內容 / 程式碼，並讓您更容易閱讀和維護檢視範本。

我們現在再次瀏覽我們稍早建立的清單案例，並啟用可擴充的分頁支援。

>[!div class="step-by-step"]
[上一頁](use-viewdata-and-implement-viewmodel-classes.md)
[下一頁](implement-efficient-data-paging.md)
