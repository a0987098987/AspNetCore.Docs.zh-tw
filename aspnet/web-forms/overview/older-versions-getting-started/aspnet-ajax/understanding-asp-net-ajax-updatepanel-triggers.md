---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
title: 了解 ASP.NET AJAX UpdatePanel 觸發程序 |Microsoft 文件
author: scottcate
description: 當使用 Visual Studio 中的標記編輯器中，您可能會注意到 （從 IntelliSense) 有兩個子項目的 UpdatePanel 控制項。 其中一個北...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2008
ms.topic: article
ms.assetid: faab8503-2984-48a9-8a40-7728461abc50
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
msc.type: authoredcontent
ms.openlocfilehash: f30f2ead402d2f49a89b2caf47cc30b6445d4cfb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="understanding-aspnet-ajax-updatepanel-triggers"></a>了解 ASP.NET AJAX UpdatePanel 觸發程序
====================
由[Scott 類別](https://github.com/scottcate)

[下載 PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial02_Triggers_cs.pdf)

> 當使用 Visual Studio 中的標記編輯器中，您可能會注意到 （從 IntelliSense) 有兩個子項目的 UpdatePanel 控制項。 其中是觸發程序項目，其指定的頁面 （或使用者控制項，如果您使用其中一個） 控制項，將會觸發 UpdatePanel 控制項的項目所在的部分呈現。


## <a name="introduction"></a>簡介

Microsoft ASP.NET 技術，帶來物件導向和事件驅動的程式設計模型，與聯集它已編譯程式碼的優點。 不過，其伺服器端處理模型有幾項技術，其中有許多您可以解決 Microsoft ASP.NET 3.5 AJAX 擴充功能中包含的新功能的固有的錯誤。 這些延伸可讓許多豐富型用戶端的新功能，包括局部網頁呈現，而不需要完整頁面重新整理，透過用戶端指令碼 （包括 ASP.NET 程式碼剖析 API） 和廣泛的 API，用戶端存取 Web 服務的能力設計來鏡射許多 ASP.NET 伺服器端控制項集合中所見的控制項配置。

本白皮書會檢查 XML 觸發程序功能的 ASP.NET AJAX`UpdatePanel`元件。 XML 觸發程序提供更精確地控制造成部分呈現特定 UpdatePanel 控制項的元件。

本白皮書為基礎的.NET Framework 3.5 Beta 2 版本和 Visual Studio 2008。 ASP.NET AJAX Extensions 中先前的附加元件組件為目標的 ASP.NET 2.0 中，現在已整合至.NET Framework 基底類別程式庫。 本白皮書也會假設您會使用 Visual Studio 2008 中，不 Visual Web Developer Express，並將提供逐步解說根據 Visual Studio 的使用者介面 （雖然會完全相容，不論程式碼清單開發環境）。

## <a name="triggers"></a>*觸發程序*

觸發程序，為給定的 UpdatePanel，根據預設，會自動包含叫用回傳，（例如） 包括 TextBox 控制項具有任何子控制項及其`AutoPostBack`屬性設定為**true**。 不過，觸發程序也可以包含以宣告方式使用的標記。這是內`<triggers>`UpdatePanel 控制項宣告一節。 雖然可以透過存取觸發程序`Triggers`集合屬性，建議您註冊執行階段的任何部分呈現觸發程序，（比方說，如果控制項不是在設計階段可用） 使用`RegisterAsyncPostBackControl(Control)`方法ScriptManager 內物件的頁面，請`Page_Load`事件。 請記住，頁面是無狀態，因此您應該重新註冊這些控制項每次建立。

自動子觸發程序包含可以也會停用 （以便建立回傳的子控制項不會自動觸發部分呈現） 藉由設定`ChildrenAsTriggers`屬性**false**。 這可讓您指派哪些特定控制項的最大彈性可能叫用網頁呈現，並建議使用，以便開發人員會選擇加入以回應事件，而不是處理任何可能發生的事件。

請注意，當 UpdatePanel 控制項為巢狀，當 UpdateMode 設定成**條件**，如果子 UpdatePanel 隨即觸發，但父系不是，然後會重新整理 UpdatePanel 的子系。 不過，如果父 UpdatePanel 重新整理，然後 UpdatePanel 的子系也會重新整理。

## <a name="the-lttriggersgt-element"></a>*&lt;觸發程序&gt;項目*

當使用 Visual Studio 中的標記編輯器中，您可能會注意到 （從 IntelliSense) 有兩個子元素的`UpdatePanel`控制項。 最常看到的項目，則`<ContentTemplate>`元素，其本質上會封裝將交易所更新面板的內容 （我們啟用局部呈現內容）。 另一個項目是`<Triggers>`元素，其指定的頁面 （或使用者控制項，如果您使用其中一個） 控制項，將會觸發 UpdatePanel 控制項中的部分呈現&lt;觸發程序&gt;位於項目。

`<Triggers>`項目可以包含任何數目每兩個子節點：`<asp:AsyncPostBackTrigger>`和`<asp:PostBackTrigger>`。 它們都接受兩個屬性，`ControlID`和`EventName`，而且可以指定目前的封裝 （encapsulation） 單位內的任何控制項 （例如，如果 UpdatePanel 控制項位於 Web 使用者控制項中，您不應嘗試參考上的控制項使用者控制項所在頁面）。

`<asp:AsyncPostBackTrigger>`項目會特別有用，因為它可以將目標從控制項的子系的任何事件*任何*UpdatePanel 控制項中的封裝，而不只是在其下此觸發程序是子系 UpdatePanel 單位. 因此，任何控制項可以對觸發程序的部分頁面更新。

同樣地，`<asp:PostBackTrigger>`項目可用來觸發程序部分頁面轉譯，但需要完整往返伺服器。 這個觸發程序項目也可用來強制執行完整頁面呈現控制項通常會否則觸發部分頁面轉譯時 (例如，當`Button`控制項存在於`<ContentTemplate>`UpdatePanel 控制項項目的)。 同樣地，PostBackTrigger 元素可以指定是在目前封裝的單位中任何 UpdatePanel 控制項的子系的任何控制項。

## <a name="lttriggersgt-element-reference"></a>*&lt;觸發程序&gt;項目參考*

*標記下階：*

| **標記** | **描述** |
| --- | --- |
| &lt;asp:AsyncPostBackTrigger&gt; | 指定控制項，可能會造成部分頁面更新，其中包含這個觸發程序參考 UpdatePanel 的事件。 |
| &lt;asp:PostBackTrigger&gt; | 指定控制項和事件，會造成完整的頁面更新 （完整頁面重新整理）。 這個標記可以用來控制否則會觸發局部呈現時，強制執行完整的重新整理。 |

## <a name="walkthrough-cross-updatepanel-triggers"></a>*逐步解說： 跨 UpdatePanel 觸發程序*

1. 建立新的 ASP.NET 網頁與設定以啟用局部呈現 ScriptManager 物件。 將兩個 updatepanel 就加入至這個頁面-第一個範圍中，包括標籤控制項 (Label1) 和兩個按鈕控制項 （Button1 和 Button2）。 Button1 應該會顯示按一下即可更新和 Button2 應該會顯示按一下即可更新，或沿著這些項目。 在第二個 UpdatePanel，包含只標籤控制項 (Label2)，但將其 ForeColor 屬性設定為預設值，使它有別以外的項目。
2. 設定更新模式屬性，這兩個 UpdatePanel 標記的**條件**。

**清單 1: Default.aspx 標記：** 

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample1.aspx)]

1. 在 Button1 Click 事件處理常式，設定為其他項目 （例如 DateTime.Now.ToLongTimeString()) 時間相依 Label1.Text 及 Label2.Text。 按一下 Button2 的事件處理常式會將只 Label1.Text 設時間相依值。

**表 2： 程式碼後置 （修剪） default.aspx.cs:** 

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample2.cs)]

1. 按 F5 以建置並執行專案。 請注意，當您按一下更新同時面板，這兩個標籤，變更文字;不過，當您按一下此面板的更新時，只有 Label1 更新。


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image2.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image1.png)

([按一下以檢視完整大小的影像](understanding-asp-net-ajax-updatepanel-triggers/_static/image3.png))


## <a name="under-the-hood"></a>*深入探討*

使用我們剛才建構的範例，我們可以看一下 ASP.NET AJAX 做什麼，以及我們 UpdatePanel 跨面板觸發程序的運作方式。 若要這樣做，我們將使用產生的網頁原始檔 HTML、 以及 Mozilla Firefox 擴充功能呼叫 firebug 這類-有了它，我們可以輕鬆地檢查 AJAX 回傳。 我們也會使用.NET 反映程式工具，以選取 Lutz Roeder。 這兩種工具都在線上，免費提供，並在網際網路上搜尋，即可找到。

檢查網頁原始碼顯示幾乎是 nothing 的事項。UpdatePanel 控制項轉譯為`<div>`容器，所以我們可以看到指令碼資源包括提供由`<asp:ScriptManager>`。 也有一些新 AJAX 特定呼叫 PageRequestManager AJAX 用戶端指令碼程式庫內部的。 最後，我們會看到兩個 UpdatePanel 容器-呈現一個`<input>`含有兩個按鈕`<asp:Label>`控制項轉譯為`<span>`容器。 （如果您檢查 firebug 這類在 DOM 樹狀結構，您會注意到標籤會呈暗灰色，表示他們無法產生可見的內容）。

按一下 [更新這個面板] 按鈕，並注意上方的 UpdatePanel 會更新為目前的伺服器時間。 在 firebug 這類，選擇 [主控台] 索引標籤，以便您可以檢查要求。 第一次檢查 POST 要求參數：


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image5.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image4.png)

([按一下以檢視完整大小的影像](understanding-asp-net-ajax-updatepanel-triggers/_static/image6.png))


請注意，UpdatePanel 指出伺服器端 AJAX 程式碼的控制項樹狀結構精確地引發透過 ScriptManager1 參數：`Button1`的`UpdatePanel1`控制項。 現在，按一下 更新同時面板按鈕。 接著，檢查回應，我們看到垂直線分隔一連串的字串; 字串中設定變數具體來說，我們會了解最上層的 UpdatePanel， `UpdatePanel1`，具有其 HTML 瀏覽器的全部內容。 AJAX 用戶端指令碼程式庫會替代 UpdatePanel 的原始的 HTML 內容，以透過新的內容`.innerHTML`屬性，並讓伺服器傳送 html 格式的伺服器已變更的內容。

現在，按一下 更新同時面板按鈕，並檢查結果，從伺服器。 會有非常類似的結果-這兩個 updatepanel 就從伺服器收到新的 HTML。 如同先前的回呼，會傳送其他的頁面狀態。

我們可以看到，因為沒有特殊的程式碼用來執行 AJAX 回傳，AJAX 用戶端指令碼程式庫便能攔截表單回傳，沒有任何額外的程式碼。 伺服器控制項自動使用 JavaScript，使它們不會自動送出表單-ASP.NET 自動會將程式碼的表單驗證和狀態，主要是透過自動指令碼資源包含 PostBackOptions 類別與 ClientScriptManager 類別。

例如，請考慮核取方塊控制項。檢查.NET 反映程式中的類別反組譯碼。 若要這樣做，請確定您 System.Web 組件已經開啟，並瀏覽至`System.Web.UI.WebControls.CheckBox`類別中，開啟`RenderInputTag`方法。 尋找檢查的條件`AutoPostBack`屬性：


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image8.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image7.png)

([按一下以檢視完整大小的影像](understanding-asp-net-ajax-updatepanel-triggers/_static/image9.png))


當已啟用自動回傳`CheckBox`控制 （透過 AutoPostBack 屬性為 true），結果`<input>`標記因此呈現使用 ASP.NET 事件處理中的指令碼其`onclick`屬性。 表單提交，然後，攔截允許 nonintrusively，無法插入至網頁的 ASP.NET AJAX 協助避免任何可能的重大變更，可能藉由使用可能是不精確的字串取代。 此外，這可讓*任何*自訂 ASP.NET 控制項，以運用 ASP.NET AJAX 沒有任何額外的程式碼以支援其使用 UpdatePanel 容器內。

`<triggers>`功能對應至 PageRequestManager 呼叫中初始化的值\_updateControls （ASP.NET AJAX 用戶端指令碼程式庫使用的慣例，該方法、 事件和欄位名稱開頭的附註以底線標示為內部，並不一定會用於程式庫本身以外使用）。 有了它，我們可以觀察哪些控制項要使 AJAX 回傳。

比方說，我們將兩個其他控制項加入頁面上，完全，留下一個 updatepanel 就之外的控制項，並讓其中一個 UpdatePanel 內。 我們會將核取方塊控制項上方 UpdatePanel 內拖放 DropDownList 清單內所定義的色彩數目。 以下是新標記：

**列出 3： 新增標記**

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample3.aspx)]

此外，這裡有新程式碼後置：

**列出 4： 程式碼後置**

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample4.cs)]

此頁面背後的概念是下拉式清單選取一個三種色彩顯示第二個標籤的核取方塊決定是否為粗體，和標籤是否顯示日期和時間。 核取方塊不會造成 AJAX 更新，但應該下拉式清單，即使它不會儲存在 UpdatePanel 內。


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image11.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image10.png)

([按一下以檢視完整大小的影像](understanding-asp-net-ajax-updatepanel-triggers/_static/image12.png))


如同上面的螢幕擷取畫面呈現，最新的按鈕，按一下已更新此面板，更新前的時間無關的底部時間是右按鈕。 日期已也關閉之間按下，因為日期是顯示在下方的標籤。 最後的優點是下方的標籤色彩： 比標籤的文字，示範控制項狀態，是重要的是，最近有更新，而且使用者預期會保留透過 AJAX 回傳。 *不過*，未更新的時間。 時間會自動重新擴展，透過持續性的\_\_解譯 ASP.NET 執行階段時控制在伺服器上重新轉譯頁面的 VIEWSTATE 欄位。 ASP.NET AJAX 伺服器程式碼無法辨識方法控制項要在其中變更狀態;它只是重新填入從檢視狀態，並執行適當的事件。

它應該要指出，不過，，有我初始化頁面內的時間\_載入事件的時間會有已正確地遞增。 因此，開發人員應該小心在適當的事件處理常式中，執行適當的程式碼，並避免使用分頁\_控制項的事件處理常式會是適當時載入。

## <a name="summary"></a>總結

ASP.NET AJAX 擴充功能 UpdatePanel 控制項的用途多，而且可以利用多種方法來識別應該會更新它的控制項事件。 它支援正由它的子控制項的自動更新，但是也可以回應至其他位置在頁面上的控制項事件。

若要減少潛在的伺服器處理負載，建議`ChildrenAsTriggers`UpdatePanel 的屬性設定為`false`，而且會選擇執行而不是預設包含的事件。 這也可防止任何不必要的事件造成潛在有害的效果，包括驗證和輸入欄位的變更。 這種錯誤可能會難以找出，因為頁面會以透明的方式更新使用者，而且可能的原因可能因此不會立即。

藉由檢查 ASP.NET AJAX 表單的內部運作張貼攔截模型，我們無法判斷此案例使用所提供的 ASP.NET framework。 在此情況下，它會保留最大的相容性與使用相同的架構設計的控制項，並干擾最少需任何額外的 JavaScript 頁面寫入。

## <a name="bio"></a>簡歷

Rob Paveza 是資深的.NET 應用程式開發人員，在 Terralever ([www.terralever.com](http://www.terralever.com))，在 Tempe，AZ.前置的互動式行銷公司 他可以在達到[ robpaveza@gmail.com ](mailto:robpaveza@gmail.com)，他的部落格位於[ http://geekswithblogs.net/robp/ ](http://geekswithblogs.net/robp/)。

Scott 是否已從 1997 年使用 Microsoft Web 技術，且 myKB.com 總統 ([www.myKB.com](http://www.myKB.com)) 擅長撰寫 ASP.NET 架構的重點 Knowledge Base 軟體解決方案的應用程式。 透過在電子郵件，即可以聯繫 Scott [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com)或在他的部落格[ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [上一頁](understanding-partial-page-updates-with-asp-net-ajax.md)
> [下一頁](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
