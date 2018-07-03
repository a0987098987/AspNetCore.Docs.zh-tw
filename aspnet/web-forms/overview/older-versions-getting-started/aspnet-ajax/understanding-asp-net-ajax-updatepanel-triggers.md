---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
title: 了解 ASP.NET AJAX UpdatePanel 觸發程序 |Microsoft Docs
author: scottcate
description: 在 Visual Studio 中標記編輯器中工作時，您可能會注意到 （舉凡 IntelliSense) 有兩個的 UpdatePanel 控制項的子項目。 當其中一項...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2008
ms.topic: article
ms.assetid: faab8503-2984-48a9-8a40-7728461abc50
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
msc.type: authoredcontent
ms.openlocfilehash: 180491b6aee188a9dca750d6d325217f1ad5212c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363717"
---
<a name="understanding-aspnet-ajax-updatepanel-triggers"></a>了解 ASP.NET AJAX UpdatePanel 觸發程序
====================
藉由[Scott Cate](https://github.com/scottcate)

[下載 PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial02_Triggers_cs.pdf)

> 在 Visual Studio 中標記編輯器中工作時，您可能會注意到 （舉凡 IntelliSense) 有兩個的 UpdatePanel 控制項的子項目。 其中之一就是觸發程序項目，其指定的頁面 （或使用者控制項，如果您有使用的話） 上的控制項就會觸發 UpdatePanel 控制項的項目所在的部分轉譯。


## <a name="introduction"></a>簡介

Microsoft ASP.NET 技術會以物件導向和事件導向的程式設計模型，以及結合編譯程式碼的優點。 不過，其伺服器端處理模型有幾個缺點固有的技術，而其中有許多可解決 Microsoft ASP.NET 3.5 AJAX Extensions 中包含的新功能。 這些擴充功能可讓許多豐富的用戶端的新功能，包括部分呈現的頁面，而不需要重新整理整個頁面，透過用戶端指令碼 （包括 ASP.NET 程式碼剖析 API），以及廣泛的用戶端 API 存取 Web 服務的能力設計用來鏡像處理許多 ASP.NET 伺服器端控制項集合中的控制項配置。

本白皮書會檢查 XML 觸發程序功能的 ASP.NET AJAX`UpdatePanel`元件。 XML 觸發程序提供更精確地控制造成特定 UpdatePanel 控制項的部分呈現的元件。

本白皮書根據 Visual Studio 2008 與.NET Framework 3.5 Beta 2 版本而定。 ASP.NET AJAX Extensions，先前的附加元件組件作為目標的 ASP.NET 2.0 中，現在已整合至.NET Framework 基底類別庫。 本白皮書也會假設您將使用 Visual Studio 2008，不 Visual Web Developer Express，並將提供逐步解說根據 Visual Studio 的使用者介面 （雖然程式碼清單將會完全相容，不論開發環境）。

## <a name="triggers"></a>*觸發程序*

指定的 UpdatePanel 中，根據預設，觸發程序會自動包含任何子控制項，叫用 （舉例來說） 包括有的文字方塊控制項的回傳他們`AutoPostBack`屬性設定為 **，則為 true**。 不過，觸發程序也可以包含以宣告方式使用標記，這是內`<triggers>`UpdatePanel 控制項宣告一節。 雖然可以透過存取觸發程序`Triggers`集合屬性，建議您註冊在執行階段的任何部分轉譯觸發程序，（比方說，如果控制項不是可在設計階段中） 使用`RegisterAsyncPostBackControl(Control)`方法ScriptManager 物件為您的頁面內`Page_Load`事件。 請記住，頁面都是無狀態，因此您應該重新註冊這些控制項每次建立。

自動的子系的觸發程序包含可以也停用 （以便建立回傳的子控制項不會自動觸發部分轉譯） 藉由設定`ChildrenAsTriggers`屬性，以**false**。 這可讓您在指派哪些特定的控制項最大的彈性可能叫用頁面轉譯，並建議使用，以便開發人員將 opt-in 以回應事件，而不是處理任何可能發生的事件。

請注意，當 UpdatePanel 控制項巢狀的當 vlastnost UpdateMode nastavena na**條件式**，如果觸發 UpdatePanel 的子系時，但父代不是，則只會重新整理 UpdatePanel 的子系。 不過，如果父 UpdatePanel 重新整理，然後子 UpdatePanel 也會重新整理。

## <a name="the-lttriggersgt-element"></a>*&lt;觸發程序&gt;項目*

在 Visual Studio 中標記編輯器中工作時，您可能會注意到 （舉凡 IntelliSense)，有兩個的子元素的`UpdatePanel`控制項。 最常看到的項目是`<ContentTemplate>`元素，基本上會封裝將會更新面板所持有的內容 （我們啟用部分呈現內容）。 其他項目是`<Triggers>`元素，其指定的頁面 （或使用者控制項，如果您有使用的話） 上的控制項就會觸發 UpdatePanel 控制項中的部分轉譯&lt;觸發程序&gt;位於項目。

`<Triggers>`項目可以包含任意數目每兩個子節點：`<asp:AsyncPostBackTrigger>`和`<asp:PostBackTrigger>`。 它們都接受兩個屬性`ControlID`和`EventName`，而且可以指定封裝的目前單位內的任何控制項 （例如，如果您的 UpdatePanel 控制項便存在於 Web 使用者控制項內，您不應該嘗試參考上的控制項使用者控制項所在頁面）。

`<asp:AsyncPostBackTrigger>`項目是特別有用，因為它可以將目標從控制項的子系形式存在的任何事件*任何*UpdatePanel 控制項中的封裝，不只是在其下此觸發程序是子系的 UpdatePanel 單位. 因此，任何控制項可以對觸發部分頁面更新。

同樣地，`<asp:PostBackTrigger>`項目可以用來觸發程序部分頁面轉譯，但需要伺服器的完整往返。 這個觸發程序項目也可用來強制整頁轉譯控制項通常會否則觸發部分頁面轉譯時 (例如，當`Button`控制項存在於`<ContentTemplate>`UpdatePanel 控制項的項目)。 同樣地，PostBackTrigger 項目可以指定會封裝目前單位中任何 UpdatePanel 控制項的子系的任何控制項。

## <a name="lttriggersgt-element-reference"></a>*&lt;觸發程序&gt;項目參考*

*標記下階：*

| **標記** | **描述** |
| --- | --- |
| &lt;asp:AsyncPostBackTrigger&gt; | 指定的控制項和事件，可能會造成部分頁面更新 UpdatePanel 中包含此觸發程序參考。 |
| &lt;asp:PostBackTrigger&gt; | 指定的控制項和事件會導致完整頁面更新 （整頁重新整理）。 此標記可用來控制否則會觸發部分呈現時，強制執行完整的重新整理。 |

## <a name="walkthrough-cross-updatepanel-triggers"></a>*逐步解說： 跨 UpdatePanel 觸發程序*

1. 使用 ScriptManager 物件設定為啟用部分呈現模型中建立新的 ASP.NET 網頁。 本頁-第一次新增兩個 Updatepanel，請加入一個 Label 控制項 (Label1) 和兩個按鈕控制項 （Button1 和 Button2）。 Button1 應該會顯示按一下即可更新和 Button2 應該會顯示 按一下以更新，或諸如此類。 在第二個 UpdatePanel 中，包含只有一個 Label 控制項 (Label2)，但 ForeColor 屬性設定為區分文章與預設值以外的項目。
2. 這兩個 UpdatePanel 標記的 UpdateMode 屬性設定**條件式**。

**列表 1: Default.aspx 標記：** 

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample1.aspx)]

1. Button1 Click 事件處理常式，在設定時將 Label1.Text 和 Label2.Text 為時間相依 （例如 DateTime.Now.ToLongTimeString())。 按一下 Button2 的事件處理常式只時將 Label1.Text 設定的時間相依的值。

**列表 2： 程式碼後置 （已修剪） 在 default.aspx.cs:** 

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample2.cs)]

1. 按 F5 以建置並執行專案。 請注意，當您按一下更新同時面板，這兩個標籤會變更文字;不過，當您按一下 更新這個面板，只 Label1 更新。


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image2.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image1.png)

([按一下以檢視完整大小的影像](understanding-asp-net-ajax-updatepanel-triggers/_static/image3.png))


## <a name="under-the-hood"></a>*深入探討*

使用我們剛才建構的範例，我們可以看看 ASP.NET AJAX 正在做什麼，以及我們 UpdatePanel 跨面板觸發程序的運作方式。 若要這樣做，我們會使用產生的頁面原始碼的 HTML，以及 Mozilla Firefox 擴充功能呼叫 FireBug-有了它，我們可以輕鬆地檢查 AJAX 回傳。 我們也會使用 Lutz Roeder 的.NET Reflector 工具。 這兩種工具可免費使用線上，並在網際網路上搜尋可找到。

檢查網頁原始碼顯示幾乎沒有異常;UpdatePanel 控制項轉譯為`<div>`容器，而且我們可以看到指令碼資源包含提供由`<asp:ScriptManager>`。 另外還有一些新 AJAX 特定呼叫 PageRequestManager 內部 AJAX 用戶端指令碼程式庫。 最後，我們看到兩個 UpdatePanel 容器-一個包含呈現`<input>`具有兩個按鈕`<asp:Label>`控制項轉譯為`<span>`容器。 （如果您檢查 DOM 樹狀目錄中的，在 FireBug 中，您會注意到標籤會呈現灰色，表示它們未產生可見內容）。

按一下 [更新這個面板] 按鈕，並注意頂端 UpdatePanel 會更新為目前的伺服器時間。 在 FireBug，請選擇 [主控台] 索引標籤，以便您可以檢查要求。 第一次檢查 POST 要求參數：


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image5.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image4.png)

([按一下以檢視完整大小的影像](understanding-asp-net-ajax-updatepanel-triggers/_static/image6.png))


請注意，UpdatePanel 已指出伺服器端的 AJAX 程式碼來精確地透過 ScriptManager1 參數引發哪些控制項樹狀結構：`Button1`的`UpdatePanel1`控制項。 現在，按一下 [更新兩個面板] 按鈕。 接著，檢查回應，我們看到直立線符號分隔的一連串字串; 字串中設定變數具體來說，我們會看到最上層的 UpdatePanel 中， `UpdatePanel1`，具有其傳送到瀏覽器的 HTML 的全部內容。 AJAX 用戶端指令碼程式庫會取代 UpdatePanel 的原始 HTML 內容以新的內容，透過`.innerHTML`屬性，因此伺服器從伺服器以 html 格式傳送變更的內容。

現在，按一下 更新兩個面板按鈕，並檢查伺服器的結果。 結果會類似-這兩個 Updatepanel 從伺服器收到新的 HTML。 如同先前的回呼，會傳送其他的頁面狀態。

我們可以看到，因為沒有特殊的程式碼會利用執行 AJAX 回傳，AJAX 用戶端指令碼程式庫可攔截表單回傳，而不需要任何額外的程式碼。 伺服器控制項自動運用 JavaScript，使它們不會自動送出表單-ASP.NET 自動插入程式碼的表單驗證和狀態，主要，即可自動指令碼資源包含 PostBackOptions 類別與 ClientScriptManager 類別。

例如，假設 [CheckBox] 控制項;檢查.NET 反射程式中的類別反組譯碼。 若要這樣做，請確定您 System.Web 組件已開啟，並瀏覽至`System.Web.UI.WebControls.CheckBox`類別中，開啟`RenderInputTag`方法。 尋找檢查的條件式`AutoPostBack`屬性：


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image8.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image7.png)

([按一下以檢視完整大小的影像](understanding-asp-net-ajax-updatepanel-triggers/_static/image9.png))


當已啟用自動回傳`CheckBox`控制 （透過將 AutoPostBack 屬性要則為 true），結果`<input>`與 ASP.NET 事件處理中的指令碼，因此呈現標記其`onclick`屬性。 攔截表單送出，然後，可讓 ASP.NET AJAX nonintrusively，插入頁面協助以避免任何潛在的重大變更，可能是藉由使用可能不精確的字串取代。 此外，這可讓*任何*利用強大的 ASP.NET AJAX 不需要任何額外的程式碼支援使用 UpdatePanel 容器內的自訂 ASP.NET 控制項。

`<triggers>` PageRequestManager 呼叫中初始化的值相對應功能\_updateControls （ASP.NET AJAX 用戶端指令碼程式庫會使用慣例，該方法、 事件和欄位名稱開頭的附註以底線標記為內部，並不適用於在程式庫本身外部使用）。 有了它，我們可以觀察哪些控制項要造成 AJAX 回傳。

比方說，我們將兩個其他控制項加入頁面上，保持 Updatepanel 之外的一個控制項，請完全的而且讓 UpdatePanel 內的其中一個。 我們將新增核取方塊控制項上方的 UpdatePanel 中，在拖放 DropDownList 以數字的清單中定義的色彩。 以下是新的標記：

**列表 3： 新的標記**

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample3.aspx)]

而以下是新程式碼後置：

**列表 4： 程式碼後置**

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample4.cs)]

此頁面背後的概念是下拉式清單選取以顯示第二個標籤的三種色彩之一，因此，核取方塊決定同時是否為粗體，以及標籤是否顯示日期與時間。 核取方塊不會造成 AJAX 更新，但應該下拉式清單，即使是在 UpdatePanel 位於不屬性。


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image11.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image10.png)

([按一下以檢視完整大小的影像](understanding-asp-net-ajax-updatepanel-triggers/_static/image12.png))


由於是顯示在上述螢幕擷取畫面，最新的按鈕，按一下已向右按鈕更新此面板，更新下時間前的時間無關。 日期也關閉之間需按幾下，日期會顯示在下方的標籤。 最後感興趣的是下方的標籤色彩： 標籤的文字，示範如何控制狀態是重要的比最近已更新，並且使用者預期它會保留透過 AJAX 回傳。 *不過*，未更新的時間。 時間會自動重新擴展，透過持續性\_\_解譯由 ASP.NET 執行階段當控制項已在伺服器上重新轉譯頁面的檢視狀態欄位。 ASP.NET AJAX 伺服器程式碼無法辨識所在方法之控制項的狀態會變更;它只是重新填入從檢視狀態，並接著執行適當的事件。

它應該會指出，不過，，有我初始化頁面內的時間\_Load 事件中，時間會有已正確地遞增。 因此，開發人員應該小心期間的適當事件處理常式中，執行適當的程式碼，並避免頁面使用\_控制項事件處理常式會是適當時載入。

## <a name="summary"></a>總結

ASP.NET AJAX Extensions UpdatePanel 控制項是用途廣泛，並可以利用多種方法來識別控制項事件，讓它更新。 它支援其子控制項，正在自動更新，但也可以回應至其他位置在頁面上的控制項事件。

若要減少潛在的伺服器處理負載，建議`ChildrenAsTriggers`UpdatePanel 的屬性設定為`false`，而且事件是選擇入而不是預設包含。 這也可防止任何不必要的事件造成可能有害的效果，包括驗證和輸入欄位的變更。 這種錯誤可能很難隔離，因為此頁面會以透明的方式更新給使用者，而且原因可能會因此不會立即顯現。

藉由檢查 ASP.NET AJAX 表單的內部運作方式張貼攔截模型，我們可以判斷它會利用 ASP.NET 已經提供的架構。 在此情況下，它會保留最大的相容性與使用相同的架構，所設計的控制項，並最少干擾任何其他的 JavaScript 撰寫的頁面上。

## <a name="bio"></a>簡歷

Rob Paveza 是 Terralever 的資深.NET 應用程式開發人員 ([www.terralever.com](http://www.terralever.com))，在 Tempe，AZ.領先業界的互動式行銷公司 他可以在觸達[ robpaveza@gmail.com ](mailto:robpaveza@gmail.com)，他的部落格位於[ http://geekswithblogs.net/robp/ ](http://geekswithblogs.net/robp/)。

Scott Cate 從事 Microsoft Web 技術工作自 1997 年，而且 myKB.com 總裁 ([www.myKB.com](http://www.myKB.com))，專精於撰寫 ASP.NET 架構著重於知識庫軟體解決方案的應用程式。 可以透過電子郵件連絡 Scott [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com)或他的部落格[ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [上一頁](understanding-partial-page-updates-with-asp-net-ajax.md)
> [下一頁](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
