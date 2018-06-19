---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
title: 了解部分頁面更新與 ASP.NET AJAX |Microsoft 文件
author: scottcate
description: 可能是最明顯的 ASP.NET AJAX 擴充功能的特色就是能夠進行部分或累加式頁面更新而不會進行完整回傳至 t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/28/2008
ms.topic: article
ms.assetid: 54d9df99-1161-4899-b4e8-2679c85915e7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
msc.type: authoredcontent
ms.openlocfilehash: 91a98bf1c9a71ae84c569f7ae40930422cb652e0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30891317"
---
<a name="understanding-partial-page-updates-with-aspnet-ajax"></a>了解部分頁面更新與 ASP.NET AJAX
====================
由[Scott 類別](https://github.com/scottcate)

[下載 PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial01_Partial_Page_Updates_cs.pdf)

> 可能是最明顯的 ASP.NET AJAX 擴充功能的特色是進行部分或累加式頁面更新而不需要執行完整頁面的回傳至伺服器，沒有程式碼變更和最少的標記變更的能力。 優點相當廣泛 – 的 （例如 Adobe Flash 或 Windows Media） 您多媒體狀態保持不變，可降低頻寬成本，而且用戶端不會遇到重繪閃動通常與回傳相關。


## <a name="introduction"></a>簡介

Microsoft ASP.NET 技術，帶來物件導向和事件驅動的程式設計模型，與聯集它已編譯程式碼的優點。 不過，其伺服器端處理模型有幾項技術中固有的錯誤：

- 頁面更新需要往返伺服器，需要重新整理頁面。
- 往返不會保存任何 Javascript 或其他用戶端技術 （如 Adobe Flash) 所產生的影響
- 在回傳時，非 Microsoft Internet Explorer 的瀏覽器不支援自動還原捲軸位置。 與中 Internet Explorer 中，即使仍有重繪閃動頁面在重新整理。
- 回傳可能牽涉到大量的頻寬為\_ \_VIEWSTATE 表單欄位可能會成長，尤其是在處理中繼器的 GridView 控制項等控制項。
- 沒有任何統一的模型，用於存取 Web 服務透過 JavaScript 或其他用戶端技術。

輸入 Microsoft ASP.NET AJAX 擴充功能。 AJAX，代表**A**同步**J** avaScript **A** nd **X** ML，是一個整合式的架構提供累加頁面跨平台 JavaScript 中，透過更新是由 Microsoft AJAX Framework 和名為 Microsoft AJAX 指令碼程式庫的指令碼元件所組成的伺服器端程式碼所組成。 ASP.NET AJAX 擴充功能也會提供用於存取 ASP.NET Web 服務透過 JavaScript 的跨平台支援。

本白皮書會檢查 ASP.NET AJAX 擴充功能，其中包含 ScriptManager 元件、 UpdatePanel 控制項和 UpdateProgress 控制項，並會考慮它們應該或不應該的案例的部分頁面更新功能過高。

本白皮書根據在 Beta 2 版本的 Visual Studio 2008 和.NET Framework 3.5，將 ASP.NET AJAX 擴充功能整合到基底類別庫 （其中先前適用於 ASP.NET 2.0 的附加元件）。 本白皮書也會假設您使用 Visual Studio 2008 並不 Visual Web Developer Express 版。參考某些專案範本可能無法使用 Visual Web Developer Express 的使用者。

## <a name="partial-page-updates"></a>部分頁面更新

可能是最明顯的 ASP.NET AJAX 擴充功能的特色是進行部分或累加式頁面更新而不需要執行完整頁面的回傳至伺服器，沒有程式碼變更和最少的標記變更的能力。 優點相當廣泛-的 （例如 Adobe Flash 或 Windows Media） 您多媒體狀態保持不變，可降低頻寬成本，而且用戶端不會遇到重繪閃動通常與回傳相關。

能夠整合部分網頁呈現整合 ASP.NET 中，幾乎不需要變更到您的專案。

## <a name="walkthrough-integrating-partial-rendering-into-an-existing-project"></a>逐步解說： 將部分呈現整合至現有的專案


1. 在 Microsoft Visual Studio 2008 中，建立新的 ASP.NET 網站專案，請前往<em>檔案</em> <em>- &gt;新增</em> <em>- &gt;網站</em>並從對話方塊選取 ASP.NET 網站。 您可以將檔案命名任何您喜歡，並可能會先安裝到檔案系統或到網際網路資訊服務 (IIS)。
2. 您會有基本的 ASP.NET 標記的空白預設頁面 (伺服器端表單和`@Page`指示詞)。 卸除稱為標籤`Label1`和按鈕呼叫`Button1`拖曳到頁面中的表單項目。 您設定的可能以任何您喜歡的 text 屬性。
3. 在 [設計] 檢視中，按兩下`Button1`產生程式碼後置事件處理常式。 在此事件處理常式中，設定`Label1.Text`至您已按下按鈕 ！ 。

**清單 1: Default.aspx 之前有啟用局部呈現標記**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample1.aspx)]

**表 2： 程式碼後置 （修剪） default.aspx.cs**

[!code-csharp[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample2.cs)]

1. 按 F5 啟動您的網站。 Visual Studio 會提示您加入 web.config 檔案以啟用偵錯。這樣做。 當您按一下按鈕時，請注意，若要變更標籤中文字的頁面重新整理沒有短暫的重繪閃動因為頁面會重新繪製。
2. 關閉瀏覽器視窗之後, 會傳回至 Visual Studio 和標記頁面。 在 Visual Studio 工具箱中，向下捲動並尋找標示為 [AJAX 擴充功能] 索引標籤。 （如果因為您使用較舊版本的 AJAX 或地圖集時進行的擴充功能，您並沒有此索引標籤，請參閱的逐步解說稍後在本白皮書中，註冊 AJAX 擴充功能的工具箱項目或安裝目前版本和可下載的 Windows 安裝程式從網站）。


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image2.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image1.png)

([按一下以檢視完整大小的影像](understanding-partial-page-updates-with-asp-net-ajax/_static/image3.png))


1. <em>已知問題：</em>如果您已經有與 ASP.NET 2.0 AJAX 擴充功能一起安裝的 Visual Studio 2005 的電腦上安裝 Visual Studio 2008，Visual Studio 2008 會匯入 AJAX 擴充功能的工具箱項目。 您可以判斷是否為情況藉由檢查元件; 的工具提示他們應該說 3.5.0.0 版本。 如果他們說 2.0.0.0 版，您已匯入舊的工具箱項目，而且必須以手動方式將其匯入 Visual Studio 中使用 [選擇工具箱項目] 對話方塊。 您無法將透過設計工具的第 2 版控制項。

2. 之前`<asp:Label>`開始標記、 建立的空白字元，行和工具箱 中的 UpdatePanel 控制項上按兩下。 請注意，新`@Register`指示詞是否包含在頁面上，指出應該使用會匯 System.Web.UI 命名空間內的控制項頂端`asp:`前置詞。
3. 拖曳結尾`</asp:UpdatePanel>`標記結尾的按鈕項目，以便使用包裝的標籤和按鈕控制項的項目是語式正確。
4. 在開啟之後`<asp:UpdatePanel>`標記中，開始開啟新的標記。 請注意，IntelliSense 會提示您有兩個選項。 在此案例中，建立`<ContentTemplate>`標記。 請務必使標記語式正確包裝您的標籤和按鈕周圍此標記。


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image5.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image4.png)

([按一下以檢視完整大小的影像](understanding-partial-page-updates-with-asp-net-ajax/_static/image6.png))


1. 中的任何地方`<form>`項目，包括 ScriptManager 控制項上按兩下`ScriptManager`工具箱中的項目。
2. 編輯`<asp:ScriptManager>`標記，使其包含屬性`EnablePartialRendering= true`。

**列出 3： 啟用局部呈現與 default.aspx 的標記**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample3.aspx)]

1. 開啟 web.config 檔案。 請注意，Visual Studio 已自動加入 System.Web.Extensions.dll 編譯參考。

1. What's New in Visual Studio 2008: web.config 中自動隨附的 ASP.NET 網站專案範本包含所有必要的參考加入至 ASP.NET AJAX 擴充功能，並包含的組態資訊可能會加上註解的區段未加上註解以啟用其他功能。 已安裝 ASP.NET 2.0 AJAX 擴充功能時，visual Studio 2005 會有類似的範本。 不過，在 Visual Studio 2008 中，AJAX 擴充功能會退出預設 （也就是他們所參考的預設值，但是可以移除做為參考）。


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image8.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image7.png)

([按一下以檢視完整大小的影像](understanding-partial-page-updates-with-asp-net-ajax/_static/image9.png))


1. 按 F5 啟動您的網站。 請注意如何變更任何來源的程式碼中需要支援局部呈現-只標記已變更。

當您啟動您的網站時，您應該會看到部分轉譯為現在已啟用，因為當您按一下按鈕將會是沒有重繪閃動，也不會有頁面捲軸的位置 （此範例不示範的） 中的任何變更。 如果您要查看呈現網頁的來源，按一下按鈕後，它會確認尚未發生回傳事實上-原始標籤文字仍然是組件的來源標記中，標籤已變更透過 JavaScript。

Visual Studio 2008 似乎不會隨附預先定義的範本，ASP.NET AJAX 能力的網站。 不過，此範本已使用 Visual Studio 2005 中，如果已安裝的 Visual Studio 2005 和 ASP.NET 2.0 AJAX 擴充功能。 因此，設定網站和開始使用 AJAX-Enabled 網站範本可能會更容易，因為範本中應包含完整設定 web.config 檔案 （支援所有 ASP.NET AJAX 擴充功能，包括 Web 服務的存取和 JSON 序列化為 JavaScript 物件標記法），其中包括 UpdatePanel 和 ContentTemplate 主要 Web Forms 網頁內的預設值。 啟用局部呈現檢視的預設網頁的很簡單，重新查看本逐步解說的步驟 10 拖放到頁面的控制項。

## <a name="the-scriptmanager-control"></a>ScriptManager 控制項

## <a name="scriptmanager-control-reference"></a>ScriptManager 控制項參考

啟用標記的屬性：

| **屬性名稱** | **Type** | **描述** |
| --- | --- | --- |
| AllowCustomErrors-Redirect | Bool | 指定是否要使用 web.config 檔案的自訂錯誤區段，來處理錯誤。 |
| AsyncPostBackError-Message | String | 取得或設定傳送到用戶端，如果發生錯誤時引發的錯誤訊息。 |
| AsyncPostBack-Timeout | Int32 | 取得或設定預設的用戶端應該等待要完成之非同步要求的時間量。 |
| EnableScript-Globalization | Bool | 取得或設定是否已啟用指令碼的全球化。 |
| EnableScript-Localization | Bool | 取得或設定是否已啟用指令碼當地語系化。 |
| ScriptLoadTimeout | Int32 | 決定指令碼載入用戶端所允許的秒數 |
| ScriptMode | 列舉 （自動、 偵錯、 發行、 繼承） | 取得或設定是否要呈現的指令碼的發行版本 |
| ScriptPath | String | 取得或設定要傳送至用戶端指令碼檔案位置的根路徑。 |

僅限程式碼的屬性：

| **屬性名稱** | **Type** | **描述** |
| --- | --- | --- |
| AuthenticationService | AuthenticationService-Manager | 取得有關將傳送至用戶端 ASP.NET 驗證服務 proxy 的詳細資料。 |
| IsDebuggingEnabled | Bool | 取得是否編寫指令碼和程式碼偵錯已啟用。 |
| IsInAsyncPostback | Bool | 取得頁面目前是否在非同步回傳要求。 |
| ProfileService | ProfileService-Manager | 取得詳細資料會傳送至用戶端的 ASP.NET 程式碼剖析服務 proxy。 |
| 指令碼 | Collection&lt;Script-Reference&gt; | 取得將傳送至用戶端的指令碼參考的集合。 |
| 服務 | Collection&lt;Service-Reference&gt; | 取得將傳送至用戶端的 Web 服務 proxy 參考的集合。 |
| SupportsPartialRendering | Bool | 取得指出是否目前的用戶端支援局部呈現。 如果這個屬性會傳回**false**，所有的網頁要求便會是標準的回傳。 |

公用程式碼的方法：

| **方法名稱** | **Type** | **描述** |
| --- | --- | --- |
| SetFocus(string) | Void | 當要求完成時，請用戶端將焦點設定至特定控制項。 |

標記下階：

| **標記** | **描述** |
| --- | --- |
| &lt;AuthenticationService&gt; | 提供 ASP.NET 驗證服務 proxy 的詳細資料。 |
| &lt;ProfileService&gt; | 提供 asp.net 程式碼剖析服務的 proxy 詳細資料。 |
| &lt;Scripts&gt; | 提供額外的指令碼參考。 |
| &lt;asp:ScriptReference&gt; | 代表特定的指令碼參考。 |
| &lt;Service&gt; | 提供額外的 Web 服務參考其中將包含所產生的 proxy 類別。 |
| &lt;asp:ServiceReference&gt; | 代表特定的 Web 服務參考。 |

ScriptManager 控制項是 ASP.NET AJAX 擴充功能的基本核心。 它提供存取權 （包含大量的用戶端指令碼型別系統） 的指令碼程式庫，支援局部呈現，並提供其他的 ASP.NET 服務 （例如驗證和程式碼剖析，還有其他 Web 服務） 更詳盡的支援。 ScriptManager 控制項也會提供用戶端指令碼的全球化和當地語系化支援。

## <a name="providing-alterative-and-supplemental-scripts"></a>提供 Alterative 和補充指令碼

在 Microsoft ASP.NET 2.0 AJAX Extensions 中這兩個偵錯包含整個指令碼並做為參考組件中內嵌的資源釋放版本時，開發人員可用來將 ScriptManager 重新導向至自訂指令碼檔案，以及註冊其他必要的指令碼。

若要覆寫通常包含指令碼 （例如，支援 Sys.WebForms 命名空間和自訂的輸入系統） 的預設繫結，您可以註冊`ResolveScriptReference`ScriptManager 類別的事件。 此事件處理常式呼叫這個方法時，有機會將路徑變更指令碼檔案有問題;指令碼管理員會再將用戶端指令碼的不同或自訂的副本。

此外，指令碼參考 (由`ScriptReference`類別) 可包含以程式設計方式或透過標記。 若要這樣做，請以程式設計方式修改`ScriptManager.Scripts`集合，或包含`<asp:ScriptReference>`標記下`<Scripts>`ScriptManager 控制項的第一層子系的標記。

## <a name="custom-error-handling-for-updatepanels"></a>自訂錯誤處理 updatepanel 就

雖然程式 UpdatePanel 控制項所指定的觸發程序會處理更新，錯誤處理和自訂錯誤訊息的支援是由頁面 ScriptManager 控制項執行個體中處理。 這是藉由公開事件， `AsyncPostBackError`，至可以網頁，然後提供自訂例外狀況處理邏輯。

所耗用的 AsyncPostBackError 事件，您可以指定`AsyncPostBackErrorMessage`屬性，進而導致警示引發完成回呼的方塊。

用戶端的自訂，您也可以不使用預設警示方塊;比方說，您可能想要顯示的自訂`<div>`項目，而不是預設瀏覽器的強制回應對話方塊。 在此情況下，您可以處理用戶端指令碼中的錯誤訊息：

**列出 5： 用戶端指令碼顯示自訂錯誤**

[!code-html[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample4.html)]

非常簡單，上述指令碼使用註冊回呼的用戶端 AJAX 執行階段非同步要求完成時。 然後它會檢查是否已報告錯誤，請參閱，若是則處理的詳細資料，最後至執行階段指出自訂指令碼中處理錯誤狀況。

## <a name="globalization-and-localization-support"></a>全球化和當地語系化支援

ScriptManager 控制項來當地語系化的指令碼字串和使用者介面元件，提供更詳盡的支援不過，該主題不本白皮書的範圍內。 如需詳細資訊，請參閱白皮書，在 ASP.NET AJAX Extensions 中的全球化支援。

## <a name="the-updatepanel-control"></a>UpdatePanel 控制項

## <a name="updatepanel-control-reference"></a>UpdatePanel 控制項參考

啟用標記的屬性：

| **屬性名稱** | **Type** | **描述** |
| --- | --- | --- |
| ChildrenAsTriggers | bool | 指定子控制項是否自動啟動回傳重新整理。 |
| RenderMode | 列舉 （「 區塊 」、 「 內嵌 」） | 指定將會以視覺化方式呈現內容的方式。 |
| UpdateMode | 列舉 （Always、 條件式） | 指定 UpdatePanel 是否永遠重新整理部分呈現期間，或如果它只重新整理時叫用觸發程序。 |

僅限程式碼的屬性：

| **屬性名稱** | **Type** | **描述** |
| --- | --- | --- |
| IsInPartialRendering | bool | 取得是否 UpdatePanel 支援局部呈現目前的要求。 |
| ContentTemplate | ITemplate | 取得標記範本更新要求。 |
| ContentTemplateContainer | 控制項 | 更新要求中取得的程式設計的範本。 |
| 觸發程序 | UpdatePanel- TriggerCollection | 取得與目前的 UpdatePanel 相關聯的觸發程序的清單。 |

公用程式碼的方法：

| **方法名稱** | **Type** | **描述** |
| --- | --- | --- |
| Update （) | Void | 以程式設計方式更新指定的 UpdatePanel。 可讓伺服器要求來觸發其他未觸發的 UpdatePanel 部分呈現。 |

標記下階：

| **標記** | **描述** |
| --- | --- |
| &lt;ContentTemplate&gt; | 指定要用來呈現部分的轉譯結果的標記。 子系&lt;asp: UpdatePanel&gt;。 |
| &lt;觸發程序&gt; | 指定的集合*n*與更新此 UpdatePanel 相關聯的控制項。 子系&lt;asp: UpdatePanel&gt;。 |
| &lt;asp:AsyncPostBackTrigger&gt; | 指定叫用的給定 UpdatePanel 的部分網頁呈現的觸發程序。 這可能會或可能不是那麼 UpdatePanel 有問題的子系的控制項。 細微的事件名稱。子系&lt;觸發程序&gt;。 |
| &lt;asp:PostBackTrigger&gt; | 指定的控制項，會導致整個頁面重新整理。 這可能會或可能不是那麼 UpdatePanel 有問題的子系的控制項。 細微的物件。 子系&lt;觸發程序&gt;。 |

`UpdatePanel`控制項是用來分隔，就需要在 AJAX 擴充功能的部分呈現功能的伺服器端內容控制項。 沒有可以在頁面上，UpdatePanel 控制項數目沒有限制，也可以巢狀。 每個 UpdatePanel 遭到隔離，使每個都可以獨立工作 （您可以有兩個 updatepanel 就相同時間執行呈現不同部分的頁面上，獨立於網頁回傳）。

UpdatePanel 控制主要負責控制觸發程序-根據預設，UpdatePanel 內所含的任何控制項`ContentTemplate`會建立回傳是否註冊為觸發程序中的 UpdatePanel。 這表示 UpdatePanel 可以使用使用者控制項 （例如 GridView) 中的預設資料繫結控制項，而且指令碼中編寫程式。

根據預設，觸發部分頁面轉譯時，將會重新整理頁面上的所有 UpdatePanel 控制項、 UpdatePanel 控制項定義這類動作的觸發程序。 例如，如果一個 UpdatePanel 定義按鈕控制項，並按一下該按鈕控制項，依預設會重新整理該頁面上的所有 UpdatePanel 控制項。 這是因為，根據預設， `UpdateMode` UpdatePanel 的屬性設定為`Always`。 或者，您可能會在這裡將更新模式屬性設定為`Conditional`，這表示如果叫用特定的觸發程序時，才會重新整理 UpdatePanel。

## <a name="custom-control-notes"></a>自訂控制項資訊

UpdatePanel 可以加入至任何使用者控制項或自訂控制項。不過，這些控制項隨附的頁面必須也包含 ScriptManager 控制項與屬性 EnablePartialRendering 設為**true**。

其中一種方式，您可能會考慮這時使用 Web 自訂控制項來覆寫的受保護`CreateChildControls()`方法`CompositeControl`類別。 如此一來，您可以將插入 UpdatePanel 控制項的子系與外界之間如果您判斷該分頁支援部分呈現。否則，您可以只是圖層的子控制項至容器`Control`執行個體。

## <a name="updatepanel-considerations"></a>UpdatePanel 的考量

UpdatePanel 的運作方式為黑色方塊的項目內容中的 JavaScript XMLHttpRequest ASP.NET 回傳文繞圖。 不過，有顯著的效能考量，務必牢記在心方面的行為和速度。 若要了解 UpdatePanel 的運作方式，讓您最好能夠決定其用途的適用時機，您應該檢查 AJAX exchange。 下列範例會使用現有的站台和、 Mozilla Firefox firebug 這類擴充功能 （firebug 這類擷取 XMLHttpRequest 資料）。

請考慮的表單，以及其他項目，它應該填入表單或控制項上的 city 和 state 欄位郵遞區號文字方塊。 最後，此表單會收集成員資格資訊，包括使用者的名稱、 地址及連絡資訊。 有許多的設計考量，若要納入考量，根據特定專案需求。


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image11.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image10.png)

([按一下以檢視完整大小的影像](understanding-partial-page-updates-with-asp-net-ajax/_static/image12.png))


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image14.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image13.png)

([按一下以檢視完整大小的影像](understanding-partial-page-updates-with-asp-net-ajax/_static/image15.png))


此應用程式的原始反覆項目，在控制項已建置納入使用者的登錄資料，包括郵遞區號、 城市及狀態的全部內容。 整個控制項已包裝在 UpdatePanel，並放到 Web 表單。 當使用者輸入的郵遞區號時，UpdatePanel 會偵測事件 （中的對應 TextChanged 事件後端，指定觸發程序，或使用 ChildrenAsTriggers 屬性會設為 true）。 由 firebug 這類擷取 AJAX 張貼所有 UpdatePanel 內的欄位 （在右側，請參閱圖）。

表示螢幕擷取畫面，因為傳遞 UpdatePanel 內的每個控制項中的值 （在此情況下，它們是全部空白），以及檢視狀態欄位。 所有 told，超過 9 kb 的資料傳送，實際上只有五個位元組的資料需要進行此特定要求時。 回應會更為繁雜： 總共 57 kb 傳送至用戶端，只是為了更新文字和下拉式清單 欄位。

它也可能是有興趣，請參閱 ASP.NET AJAX 更新展示檔的方式。 UpdatePanel 更新要求的回應部分會顯示在左邊; firebug 這類主控台顯示它是特別構成管線分隔字串會依用戶端指令碼和重組頁面上。 具體而言，ASP.NET AJAX 設定*innerHTML*用戶端，表示您的 UpdatePanel 上 HTML 項目的屬性。 瀏覽器會重新產生 DOM，即沒有稍微延遲，視需要處理的資訊數量而定。

重新產生作業的 DOM 的觸發程序的一些其他問題：


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image17.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image16.png)

([按一下以檢視完整大小的影像](understanding-partial-page-updates-with-asp-net-ajax/_static/image18.png))


- 如果在 UpdatePanel 內的焦點的 HTML 項目，就可能會失去焦點。 因此，如果按下 Tab 鍵以結束郵遞區號文字方塊中的使用者，其下一個目的地已經 [縣 （市）] 文字方塊。 不過，一旦 UpdatePanel 重新整理顯示畫面，表單就不再擁有焦點，並按 Tab 鍵已經啟動 （例如連結） 的焦點項目反白顯示。
- 如果任何類型的自訂用戶端指令碼正在使用中，存取 DOM 項目，參考會保存函式可能就會變成無用之後部分回傳。

Updatepanel 就不會是所有方案。 相反地，提供特定狀況，包括建立原型，小型控制更新快速解決方案和 ASP.NET 開發人員可能熟悉.NET 物件模型，但較不因此與 DOM 提供熟悉的介面 有幾種方式，可能會導致更好的效能，視應用程式案例而定：

- 請考慮使用 PageMethods 和 JSON （JavaScript 物件標記法） 可讓開發人員，如同叫用 web 服務呼叫，時，叫用網頁上的靜態方法。 因為方法是靜態的無狀態是必要的。指令碼呼叫端提供的參數，並以非同步方式傳回結果。
- Web 服務和 JSON 如果需要考慮使用單一控制項跨應用程式在幾個地方使用。 這一次需要特殊的工作相當少，並以非同步方式運作。

將透過 Web 服務或網頁方法的功能有也有其缺點。 首先和最重要，ASP.NET 開發人員通常傾向功能的小型元件建置至使用者控制項 （.ascx 檔案）。 無法在這些檔案; 裝載網頁方法它們必須裝載在實際的.aspx 網頁類別。 同樣地，web 服務必須裝載在.asmx 類別。 不同的應用程式，此架構會違反單一的責任原則，在於單一元件的功能現在橫跨兩個或多個實體元件可能會有少量或沒有一致的繫結。

最後，如果應用程式需要 updatepanel 就會使用，下列指導方針應該協助疑難排解與維護。

- **巢狀 updatepanel 就盡可能，不僅內單位，同時也是跨單位的程式碼。** 例如，UpdatePanel 包裝控制項，而該控制項也包含 UpdatePanel，其中包含含有 UpdatePanel 的另一個控制項，在頁面上是跨單位巢狀結構。 這有助於清除保留哪些項目應該重新整理，並且避免子 updatepanel 就未預期的重新整理。
- **保留*ChildrenAsTriggers*屬性設定為 false，而且明確設定 觸發事件。** 利用`<Triggers>`集合更輕鬆的方式，來處理事件，而可能會導致非預期的行為，協助維護工作，並強制開發人員選擇加入的事件。
- **使用可能的最小單位來達成的功能。** 如中所述的 「 郵遞區號 」 服務，包裝討論只有最低限度降低時間伺服器、 總處理，以及用戶端-伺服器交換的使用量，增強效能。

## <a name="the-updateprogress-control"></a>UpdateProgress 控制項

## <a name="updateprogress-control-reference"></a>UpdateProgress 控制項參考

啟用標記的屬性：

| **屬性名稱** | **Type** | **描述** |
| --- | --- | --- |
| AssociatedUpdate-PanelID | String | 指定此 UpdateProgress 應該報告的 UpdatePanel 的識別碼。 |
| DisplayAfter | Int | 在開始非同步的要求之後，會顯示此控制項之前，請以毫秒為單位指定的逾時。 |
| DynamicLayout | bool | 指定是否要以動態方式呈現進度。 |

標記下階：

| **標記** | **描述** |
| --- | --- |
| &lt;ProgressTemplate&gt; | 包含設定將與此控制項顯示之內容的控制項範本。 |

UpdateProgress 控制項可讓您的意見來進行必要的工作來傳輸到伺服器時保留您的使用者感興趣的量值。 這可協助使用者了解，您所做的項目即使它可能不明顯，尤其是大部分使用者可用來重新整理，並查看 [狀態] 列反白顯示的頁面。

請注意，為 UpdateProgress 控制項可以出現的任何位置頁面階層上。 不過，在部分回傳起始從子系 UpdatePanel （UpdatePanel 會巢狀另一個 UpdatePanel） 所在的情況下，回傳的觸發程序 UpdatePanel 會導致 UpdateProgress 範本顯示為子系的子系UpdatePanel 以及 UpdatePanel 的父系。 但是，如果觸發程序的直接子系的父系 UpdatePanel，則只有與父代相關聯的 UpdateProgress 範本會顯示。

## <a name="summary"></a>總結

Microsoft ASP.NET AJAX 擴充功能是設計用來協助您讓網頁更容易存取，並提供更豐富的使用者體驗，對您 web 應用程式的高性能的產品。 ASP.NET AJAX Extensions 中的部分頁面轉譯控制項，一部分包括 ScriptManager、 UpdatePanel 和 UpdateProgress 控制項是一些最明顯的工具組元件。

ScriptManager 元件整合的用戶端 JavaScript 的擴充，佈建，以及可讓不同的伺服器和用戶端元件一起開發的最小投資。

UpdatePanel 控制項是明顯 magic 方塊-UpdatePanel 中的標記可以有伺服器端程式碼後置並不會觸發頁面重新整理。 UpdatePanel 控制項可以巢狀，而且可以相依於其他 updatepanel 就中的控制項。 根據預設，updatepanel 就會處理及其子系控制項，來叫用任何回傳，雖然這項功能可以是要更為精確，以宣告方式或以程式設計的方式。

當使用 UpdatePanel 控制項時，開發人員應該留意可能就會發生的效能影響。 可能的替代項目包含 web 服務和頁面的方法，但應視為應用程式的設計。

UpdateProgress 控制項可讓使用者知道她或他不會被忽略，而幕後作業的要求將要頁面否則不會進行任何動作來回應使用者輸入。 它也包含中止局部呈現結果。

在一起，這些工具可協助提供豐富、 順暢的使用者體驗的伺服器工作給使用者較不明顯，較不會中斷工作流程。

## <a name="bio"></a>簡歷

Scott 是否已從 1997 年使用 Microsoft Web 技術，且 myKB.com 總統 ([www.myKB.com](http://www.myKB.com)) 擅長撰寫 ASP.NET 架構的重點 Knowledge Base 軟體解決方案的應用程式。 透過在電子郵件，即可以聯繫 Scott [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com)或在他的部落格[ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [下一步](understanding-asp-net-ajax-updatepanel-triggers.md)
