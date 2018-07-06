---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
title: 了解部分頁面更新與 ASP.NET AJAX |Microsoft Docs
author: scottcate
description: 可能的 ASP.NET AJAX Extensions 最明顯的特色就是能夠執行的部分或累加頁面更新，而不進行完整回傳至 t...
ms.author: aspnetcontent
ms.date: 03/28/2008
ms.assetid: 54d9df99-1161-4899-b4e8-2679c85915e7
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
msc.type: authoredcontent
ms.openlocfilehash: 8ec4df5ffeaab4490eaea0f0093d543d517bd5f4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805268"
---
<a name="understanding-partial-page-updates-with-aspnet-ajax"></a>了解部分頁面更新與 ASP.NET AJAX
====================
藉由[Scott Cate](https://github.com/scottcate)

[下載 PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial01_Partial_Page_Updates_cs.pdf)

> 或許在 ASP.NET AJAX extensions 最明顯的特色就是能夠執行的部分或累加頁面更新，而不進行完整的回傳至伺服器，不含程式碼變更和最小的標記變更。 優點相當廣泛 – 您的多媒體 （例如 Adobe Flash 或 Windows Media） 狀態保持不變，可降低頻寬成本，而且用戶端不會遇到閃爍通常與回傳相關。


## <a name="introduction"></a>簡介

Microsoft ASP.NET 技術會以物件導向和事件導向的程式設計模型，以及結合編譯程式碼的優點。 不過，其伺服器端處理模型有幾個缺點固有的技術：

- 頁面更新需要往返伺服器，而這需要重新整理頁面。
- 來回行程不會保存任何 Javascript 或其他用戶端技術 （例如 Adobe Flash) 所產生的影響
- 在回傳時，非 Microsoft Internet Explorer 的瀏覽器不支援自動還原捲軸位置。 即使在 Internet Explorer 中，仍有重繪閃動的頁面在重新整理。
- 回傳可能牽涉到大量的頻寬， \_ \_VIEWSTATE 表單欄位可能會成長，特別是在處理例如 GridView 控制項或重複項控制項。
- 沒有任何統一的模型，可透過 JavaScript 或其他用戶端技術存取 Web 服務。

輸入 Microsoft ASP.NET AJAX 擴充功能。 AJAX 中，代表**A**同步**J** avaScript **A** nd **X** ML，是一個整合式的架構，提供累加頁面透過跨平台 JavaScript 的更新是由伺服器端程式碼，其中包含 Microsoft AJAX 架構中和一個稱為 Microsoft AJAX 指令碼程式庫的指令碼元件所組成。 ASP.NET AJAX 擴充功能也會提供用於存取 ASP.NET Web 服務，透過 JavaScript 的跨平台支援。

本白皮書探討 ASP.NET AJAX Extensions，其中包含 ScriptManager 元件，UpdatePanel 控制項和 UpdateProgress 控制項，並考慮他們應該或不應該是的案例的部分頁面更新功能利用。

本白皮書根據 Visual Studio 2008 Beta 2 版和.NET Framework 3.5，ASP.NET AJAX Extensions 整合基底類別庫 （其中是先前適用於 ASP.NET 2.0 的附加元件）。 本白皮書也假設您使用 Visual Studio 2008 並不 Visual Web Developer Express 版;某些所參考的專案範本可能無法供 Visual Web Developer Express 的使用者。

## <a name="partial-page-updates"></a>部分頁面更新

或許在 ASP.NET AJAX extensions 最明顯的特色就是能夠執行的部分或累加頁面更新，而不進行完整的回傳至伺服器，不含程式碼變更和最小的標記變更。 優點相當廣泛-您的多媒體 （例如 Adobe Flash 或 Windows Media） 狀態保持不變，可降低頻寬成本，而且用戶端不會遇到閃爍通常與回傳相關。

能夠整合部分網頁呈現會整合到 ASP.NET，幾乎不需要變更到您的專案。

## <a name="walkthrough-integrating-partial-rendering-into-an-existing-project"></a>逐步解說： 將部分呈現模型整合到現有的專案


1. 在 Microsoft Visual Studio 2008 中，建立新的 ASP.NET 網站專案移至<em>檔案</em> <em>- &gt;新增</em> <em>- &gt;網站</em> ，然後從對話方塊選取 ASP.NET 網站。 您可以為它命名您喜歡，並可能會先安裝到檔案系統或到網際網路資訊服務 (IIS)。
2. 您會看到基本的 ASP.NET 標記的空白的預設頁面 (伺服器端表單和`@Page`指示詞)。 卸除稱為標籤`Label1`，而且按鈕呼叫`Button1`拖曳到頁面中的表單項目。 您可以為任何您喜歡設定其 text 屬性。
3. 在 設計 檢視中，按兩下 `Button1`來產生程式碼後置的事件處理常式。 在這個事件處理常式中，設定`Label1.Text`要按下按鈕 ！ 。

**列表 1: Default.aspx，才能啟用部分呈現的標記**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample1.aspx)]

**列表 2： 程式碼後置 （已修剪） 在 default.aspx.cs 中**

[!code-csharp[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample2.cs)]

1. 按 F5 以啟動您的網站。 Visual Studio 會提示您加入的 web.config 檔案，以啟用偵錯;這麼做。 當您按一下按鈕時，請注意，若要變更標籤中文字的頁面重新整理，而且沒有簡短的重繪閃動，頁面會重新繪製。
2. 關閉瀏覽器視窗之後, 會傳回 Visual Studio，然後至 [標記] 頁面。 在 Visual Studio 工具箱中，向下捲動並尋找標示為 [AJAX 延伸模組] 索引標籤。 （如果因為您正在使用較舊版本的 AJAX 或 Atlas 的延伸模組，您還沒有此索引標籤，請參閱本逐步解說稍後在本白皮書中，註冊 AJAX Extensions 的工具箱項目或使用可下載的 Windows Installer 安裝最新版本從網站）。


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image2.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image1.png)

([按一下以檢視完整大小的影像](understanding-partial-page-updates-with-asp-net-ajax/_static/image3.png))


1. <em>已知問題：</em>如果您已經有 Visual Studio 2005 與 ASP.NET 2.0 AJAX Extensions 一起安裝的電腦上安裝 Visual Studio 2008，Visual Studio 2008 將會匯入 AJAX Extensions 的工具箱項目。 您可以判斷是否這是藉由檢查元件; 的工具提示的情況它們應該是版本 3.5.0.0。 如果他們說 2.0.0.0 版，您已匯入舊的工具箱項目，而且必須以手動方式將它們匯入 Visual Studio 中使用 [選擇工具箱項目] 對話方塊。 您無法將透過設計工具的第 2 版控制項。

2. 之前`<asp:Label>`開始標記、 建立的空白區域，行和 UpdatePanel 控制項，在 [工具箱] 上按兩下。 請注意，新`@Register`指示詞就會包含表示該 System.Web.UI 命名空間內的控制項應該匯入使用的頁面頂端`asp:`前置詞。
3. 拖曳結尾`</asp:UpdatePanel>`按鈕項目結尾標記，以便使用標籤和按鈕控制項包裝的項目是語式正確。
4. 在開啟之後`<asp:UpdatePanel>`標記中，開啟新的標記的開頭。 請注意，IntelliSense 會提示您使用兩個選項。 在此案例中，建立`<ContentTemplate>`標記。 請務必包裝您的標籤與按鈕周圍此標記，標記是語式正確。


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image5.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image4.png)

([按一下以檢視完整大小的影像](understanding-partial-page-updates-with-asp-net-ajax/_static/image6.png))


1. 中的任何地方`<form>`項目，包含 ScriptManager 控制項，方法是按兩下`ScriptManager`工具箱 中的項目。
2. 編輯`<asp:ScriptManager>`標記，使其包含屬性`EnablePartialRendering= true`。

**列表 3： 使用部分呈現啟用 default.aspx 的標記**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample3.aspx)]

1. 開啟 web.config 檔案。 請注意，Visual Studio 會自動新增 System.Web.Extensions.dll 編譯參考。

1. What's New in Visual Studio 2008: web.config 所附 ASP.NET 網站專案範本會自動包含所有必要的參考，ASP.NET AJAX Extensions，並包含的組態資訊，可以加上註解的區段未加上註解來啟用其他功能。 已安裝 ASP.NET 2.0 AJAX Extensions 時，visual Studio 2005 就會有類似的範本。 不過，在 Visual Studio 2008 中，AJAX 擴充功能會退出預設 （也就是他們所參考的預設值，但是可以移除做為參考）。


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image8.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image7.png)

([按一下以檢視完整大小的影像](understanding-partial-page-updates-with-asp-net-ajax/_static/image9.png))


1. 按 F5 以啟動您的網站。 請注意如何變更任何來源的程式碼都必須支援部分呈現-僅標記已變更。

當您啟動您的網站時，您應該會看到部分轉譯是現在已啟用，因為當您按一下按鈕有會無重繪閃動，也不會有頁面捲動位置 （此範例不示範的） 中的任何變更。 如果您要查看頁面的呈現來源，按一下按鈕後，它會確認實際上尚未發生回傳，-仍屬於來源標記，原始的標籤文字。，且透過 JavaScript 已變更的標籤。

Visual Studio 2008 似乎不隨附預先定義的範本，ASP.NET Ajax 的網站。 不過，這類範本已使用 Visual Studio 2005 內，如果已安裝 Visual Studio 2005 與 ASP.NET 2.0 AJAX Extensions。 因此，設定網站和開始使用採用 ajax 的網站範本可能會更加簡單，因為範本應該包含完全設定好 web.config 檔 （支援所有 ASP.NET AJAX Extensions，包括 Web 服務的存取和 JSON 序列化為 JavaScript 物件標記法），其中包括 UpdatePanel 和 ContentTemplate 內主要的 Web Form 網頁的預設值。 啟用與此預設頁面的部分呈現是簡單，只要重新認識本逐步解說的步驟 10，並拖放到頁面的控制項。

## <a name="the-scriptmanager-control"></a>ScriptManager 控制項

## <a name="scriptmanager-control-reference"></a>ScriptManager 控制項參考

啟用標記的屬性：

| **屬性名稱** | **Type** | **描述** |
| --- | --- | --- |
| AllowCustomErrors-Redirect | Bool | 指定是否要使用自訂錯誤區段的 web.config 檔案來處理錯誤。 |
| AsyncPostBackError-Message | String | 取得或設定傳送至用戶端，如果引發了錯誤的錯誤訊息。 |
| AsyncPostBack-Timeout | Int32 | 取得或設定預設的用戶端應該等待要完成的非同步要求的時間量。 |
| EnableScript-Globalization | Bool | 取得或設定是否已啟用指令碼的全球化。 |
| EnableScript-Localization | Bool | 取得或設定是否已啟用指令碼當地語系化。 |
| ScriptLoadTimeout | Int32 | 決定指令碼載入用戶端允許的秒數 |
| ScriptMode | 列舉 （Auto、 偵錯、 發行、 繼承） | 取得或設定是否要轉譯的指令碼的發行版本 |
| ScriptPath | String | 取得或設定根路徑傳送至用戶端的指令碼檔案的位置。 |

僅限程式碼的屬性：

| **屬性名稱** | **Type** | **描述** |
| --- | --- | --- |
| AuthenticationService | AuthenticationService-Manager | 取得有關 ASP.NET 驗證服務 proxy，將會傳送至用戶端的詳細資料。 |
| IsDebuggingEnabled | Bool | 取得是否編寫指令碼，並啟用程式碼偵錯。 |
| IsInAsyncPostback | Bool | 取得頁面目前是否非同步回傳要求。 |
| ProfileService | ProfileService-Manager | 取得詳細資料會傳送至用戶端的 ASP.NET 程式碼剖析服務 proxy。 |
| 指令碼 | Collection&lt;Script-Reference&gt; | 取得將傳送至用戶端的指令碼參考的集合。 |
| 服務 | Collection&lt;Service-Reference&gt; | 取得將傳送至用戶端的 Web 服務 proxy 參考的集合。 |
| SupportsPartialRendering | Bool | 取得指出是否目前的用戶端支援部分呈現。 如果這個屬性會傳回**false**，所有的網頁要求便會是標準的回傳。 |

公用程式碼的方法：

| **方法名稱** | **Type** | **描述** |
| --- | --- | --- |
| SetFocus(string) | Void | 當要求完成時，請用戶端將焦點設定至特定控制項。 |

標記下階：

| **標記** | **描述** |
| --- | --- |
| &lt;AuthenticationService&gt; | 提供 ASP.NET 驗證服務的 proxy 詳細資料。 |
| &lt;ProfileService&gt; | Asp.net 程式碼剖析服務提供 proxy 詳細資料。 |
| &lt;指令碼&gt; | 提供額外的指令碼參考。 |
| &lt;asp:ScriptReference&gt; | 表示特定的指令碼參考。 |
| &lt;Service&gt; | 提供額外的 Web 服務參考就會產生的 proxy 類別。 |
| &lt;asp:ServiceReference&gt; | 表示特定的 Web 服務參考。 |

ScriptManager 控制項是 ASP.NET AJAX Extensions 的基本核心。 它提供 （包括大量的用戶端指令碼型別系統） 的指令碼程式庫的存取、 支援部分呈現，並可廣泛支援的其他 ASP.NET 服務 （例如驗證和程式碼剖析，還有其他 Web 服務）。 ScriptManager 控制項還提供全球化和當地語系化支援用戶端指令碼。

## <a name="providing-alterative-and-supplemental-scripts"></a>提供 Alterative 和補充指令碼

雖然 Microsoft ASP.NET 2.0 AJAX Extensions 包含整個指令碼，在這兩個偵錯和發行版本做為參考的組件中內嵌的資源，開發人員可以自由重新 ScriptManager 導向至自訂指令碼檔案，以及註冊其他必要的指令碼。

若要覆寫預設的繫結通常包含指令碼 （例如那些支援 Sys.WebForms 命名空間和自訂的類型系統），您可以註冊`ResolveScriptReference`ScriptManager 類別的事件。 事件處理常式呼叫這個方法時，有機會變更指令碼檔案的路徑指令碼管理員然後會將不同或自訂指令碼的複本傳送至用戶端。

此外，指令碼參考 (由`ScriptReference`類別) 可以包含以程式設計方式或透過標記。 若要這樣做，請以程式設計方式修改`ScriptManager.Scripts`集合，或包含`<asp:ScriptReference>`標記下`<Scripts>`ScriptManager 控制項的第一層子系的標記。

## <a name="custom-error-handling-for-updatepanels"></a>自訂錯誤處理以 Updatepanel

雖然更新會由 UpdatePanel 控制項所指定的觸發程序，支援錯誤處理和自訂錯誤訊息會處理網頁的 ScriptManager 控制項執行個體。 這是藉由公開事件， `AsyncPostBackError`，至可以網頁，然後提供自訂的例外狀況處理邏輯。

如果耗用 AsyncPostBackError 事件，您可以指定`AsyncPostBackErrorMessage`屬性，則會導致在回呼完成時引發的警示方塊。

用戶端自訂，也可以而不是使用預設的警示方塊;比方說，您可能想要顯示自訂`<div>`項目，而不是預設瀏覽器的強制回應對話方塊。 在此情況下，您可以處理用戶端指令碼中的錯誤訊息：

**列表 5： 用戶端指令碼以顯示自訂錯誤**

[!code-html[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample4.html)]

非常簡單，上述指令碼註冊的回呼與用戶端 AJAX 執行階段的非同步要求完成時。 然後它會檢查以查看是否已回報錯誤，而且若是如此，處理的詳細資料，最後執行階段指示錯誤已處理自訂的指令碼中。

## <a name="globalization-and-localization-support"></a>全球化和當地語系化支援

ScriptManager 控制項可廣泛支援當地語系化的指令碼字串和使用者介面元件;不過，該主題超出本白皮書的範圍。 如需詳細資訊，請參閱技術白皮書，ASP.NET AJAX Extensions 中的全球化支援。

## <a name="the-updatepanel-control"></a>UpdatePanel 控制項

## <a name="updatepanel-control-reference"></a>UpdatePanel 控制項參考

啟用標記的屬性：

| **屬性名稱** | **Type** | **描述** |
| --- | --- | --- |
| ChildrenAsTriggers | bool | 指定子控制項是否會自動啟動重新整理在回傳。 |
| RenderMode | 列舉 （「 區塊 」、 「 內嵌 」） | 指定將會以視覺化方式呈現內容的方式。 |
| UpdateMode | enum （Always、 條件式） | 指定 UpdatePanel 會永遠重新整理期間部分轉譯，或如果它只重新整理時叫用觸發程序。 |

僅限程式碼的屬性：

| **屬性名稱** | **Type** | **描述** |
| --- | --- | --- |
| IsInPartialRendering | bool | 取得是否 UpdatePanel 支援部分呈現目前要求。 |
| ContentTemplate | ITemplate | 取得標記的範本更新要求。 |
| ContentTemplateContainer | 控制項 | 更新要求中取得的程式化的範本。 |
| 觸發程序 | UpdatePanel- TriggerCollection | 取得與目前的 UpdatePanel 相關聯的觸發程序的清單。 |

公用程式碼的方法：

| **方法名稱** | **Type** | **描述** |
| --- | --- | --- |
| Update （) | Void | 以程式設計方式更新指定的 UpdatePanel。 允許部分轉譯，否則為未觸發 UpdatePanel 的觸發程序的伺服器要求。 |

標記下階：

| **標記** | **描述** |
| --- | --- |
| &lt;ContentTemplate&gt; | 指定要用來呈現部分轉譯結果的標記。 子系&lt;asp: UpdatePanel&gt;。 |
| &lt;觸發程序&gt; | 指定的集合*n*與更新此 UpdatePanel 相關聯的控制項。 子系&lt;asp: UpdatePanel&gt;。 |
| &lt;asp:AsyncPostBackTrigger&gt; | 指定叫用指定的 UpdatePanel 部分頁面轉譯的觸發程序。 這可能會或可能不是那麼有問題的 UpdatePanel 的子系的控制項。 細微的事件名稱。子系&lt;觸發程序&gt;。 |
| &lt;asp:PostBackTrigger&gt; | 指定的控制項，會導致重新整理整個頁面。 這可能會或可能不是那麼有問題的 UpdatePanel 的子系的控制項。 細微的物件。 子系&lt;觸發程序&gt;。 |

`UpdatePanel`控制項是用來分隔，就需要在 AJAX 擴充功能的部分呈現功能的伺服器端內容的控制項。 可以在頁面上，UpdatePanel 控制項的數目沒有限制，它們可以巢狀。 每個 UpdatePanel 遭到隔離，以便可以單獨處理 （您可以有兩個 Updatepanel 在此同時，執行轉譯的頁面上，不同部分獨立於網頁回傳）。

UpdatePanel 控制項主要是負責控制觸發程序-根據預設，包含在 UpdatePanel 內的任何控制項`ContentTemplate`這樣將會產生回傳註冊為觸發程序進行 UpdatePanel。 這表示 UpdatePanel 能使用的預設資料繫結控制項 （例如 GridView)，使用使用者控制項，而且也可以設計在指令碼中。

根據預設，觸發部分頁面轉譯時，將會重新整理頁面上的所有 UpdatePanel 控制項，UpdatePanel 控制項已定義的觸發程序這類動作。 例如，如果其中一個 UpdatePanel 定義按鈕控制項，並按一下該按鈕控制項，預設會重新整理該頁面上的所有 UpdatePanel 控制項。 這是因為根據預設， `UpdateMode` UpdatePanel 的屬性設定為`Always`。 或者，您可能會在這裡將 UpdateMode 屬性設定為`Conditional`，這表示如果叫用特定的觸發程序時，才會重新整理 UpdatePanel。

## <a name="custom-control-notes"></a>自訂控制項備忘稿

UpdatePanel 可新增至任何使用者控制項或自訂的控制項;不過，這些控制項是包含頁面上也必須包含 ScriptManager 控制項屬性 EnablePartialRendering 設為 **，則為 true**。

其中一種方式在其中您可能會考慮這一點時使用 Web 自訂控制項來覆寫受保護`CreateChildControls()`方法的`CompositeControl`類別。 如此一來，您可以插入 UpdatePanel 控制項的子系與外界之間如果您判斷頁面支援部分呈現模型;您只可以到容器層的子控制項的否則為`Control`執行個體。

## <a name="updatepanel-considerations"></a>UpdatePanel 的考量

UpdatePanel 的運作方式的黑箱包裝 JavaScript XMLHttpRequest 的內容中的 ASP.NET 回傳。 不過，有顯著的效能考量應謹記在心，行為和速度方面。 若要了解 UpdatePanel 的運作方式，以便您最適合可以決定適當的用法時，您應該檢查 AJAX exchange。 下列範例會使用現有的站台和、 Mozilla Firefox Firebug 擴充功能 （Firebug 會擷取 XMLHttpRequest 資料）。

請考慮的表單，以及其他項目，應該要填入城市與縣市欄位在表單或控制項上的郵遞區號 textbox。 最後，此表單會收集成員資格資訊，包括使用者的名稱、 地址和連絡資訊。 有許多應該列入考量，根據特定專案需求的設計考量。


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image11.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image10.png)

([按一下以檢視完整大小的影像](understanding-partial-page-updates-with-asp-net-ajax/_static/image12.png))


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image14.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image13.png)

([按一下以檢視完整大小的影像](understanding-partial-page-updates-with-asp-net-ajax/_static/image15.png))


在此應用程式的原始反覆項目，控制項已內建納入使用者註冊資料，其包含郵遞區號、 城市和狀態的全部內容。 整個控制項包裝在 UpdatePanel 中，放到 Web 表單。 當使用者輸入的郵遞區號時，UpdatePanel 會偵測事件 （對應 TextChanged 事件後端中，指定觸發程序，或使用 ChildrenAsTriggers 屬性設定為 true）。 AJAX 回傳的所有欄位在 UpdatePanel 中，由 FireBug 擷取 （請查看右側的圖表）。

如所指出的螢幕擷取畫面，傳遞從 UpdatePanel 內的每個控制項的值 （在此情況下，它們是全部空白），以及檢視狀態欄位。 所有 told，超過 9 kb 的資料會傳送，當事實上只有五個位元組的資料所需進行此特定的要求。 回應是更為繁雜： 總計 57 kb 傳送至用戶端，只是要更新的文字欄位和下拉式清單欄位。

它也可能是有興趣，請參閱 ASP.NET AJAX 更新簡報的方式。 UpdatePanel 的更新要求的回應部分會顯示在 Firebug 主控台顯示在左邊;它是特別制訂直立線符號分隔的字串，依據用戶端指令碼再重組頁面上。 具體來說，ASP.NET AJAX 設定*innerHTML* HTML 項目，表示您 UpdatePanel 用戶端上的屬性。 瀏覽器會重新產生 DOM，因為沒有會稍有延遲，視需要處理的資訊數量而定。

重新產生 DOM 的觸發程序的一些其他問題：


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image17.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image16.png)

([按一下以檢視完整大小的影像](understanding-partial-page-updates-with-asp-net-ajax/_static/image18.png))


- 如果焦點的 HTML 項目是在 UpdatePanel 中，它就會失去焦點。 因此，如果使用者按下 Tab 鍵以結束郵遞區號文字方塊中，其下一個目的地已經 [縣 （市）] 文字方塊。 一旦在 UpdatePanel 重新整理顯示畫面，表單將不會再擁有焦點，但按下 Tab 鍵會啟動反白顯示的焦點項目 （例如連結）。
- 如果任何類型的自訂用戶端指令碼正在使用中，存取 DOM 項目，參考會保存函式可能會變成無用之後部分回傳。

Updatepanel 並非是全面的解決方案。 相反地，某些情況下，包括建立原型，小型控制項的更新，提供快速的解決方案和 ASP.NET 的開發人員熟悉的.NET 物件模型，但為小於-所以使用 DOM，可能會提供熟悉的介面 有幾個可能會導致更好的效能，視應用程式案例而定的替代項目：

- 請考慮使用 PageMethods 和 JSON （JavaScript 物件標記法） 可讓開發人員，如同叫用 web 服務呼叫，時，請叫用頁面上的靜態方法。 方法是靜態的因為沒有任何狀態是必要的;指令碼呼叫端提供的參數，並以非同步方式傳回的結果。
- 請考慮使用 Web 服務和 JSON，如果需要可用於整個應用程式的幾個地方的單一控制項。 這一次需要極少的特殊工作，並以非同步方式運作。

將透過 Web 服務或頁面方法的功能有也有缺點。 首先，ASP.NET 開發人員通常會傾向於建置功能的小型元件至使用者控制項 （.ascx 檔案）。 頁面方法無法裝載在這些檔案;它們必須裝載在實際的.aspx 頁面類別中。 同樣地，必須從.asmx 類別內裝載 web 服務。 根據應用程式，此架構可能會違反 Single Responsibility Principle，在於單一元件的功能現在會散佈在兩個或多個實體元件可能會有少量或沒有一致的繫結。

最後，如果應用程式需要，會使用 Updatepanel，下列指導方針應該協助疑難排解與維護。

- **巢狀 Updatepanel 稍微盡，不僅內單位，同時也是跨單位的程式碼。** 比方說，UpdatePanel 包裝控制項，而該控制項也會包含 UpdatePanel 中，其中包含含有 UpdatePanel 的另一個控制項，在頁面上是跨單位的巢狀結構。 這有助於要搞清楚哪些項目應該重新整理，並防止未預期的重新整理至子系 Updatepanel 所示。
- **保持*ChildrenAsTriggers*屬性設為 false，而且明確設定 觸發事件。** 利用`<Triggers>`集合是要清楚得多的方式來處理事件，而且可能會導致非預期的行為，協助進行維護工作，並強迫開發人員選擇加入的事件。
- **使用的最小可能的單位來達成的功能。** 如所述，在討論中的 「 郵遞區號 」 服務，包裝中只有最低限度伺服器、 總處理，以及用戶端-伺服器 exchange 的使用量來縮短時間，提昇效能。

## <a name="the-updateprogress-control"></a>UpdateProgress 控制項

## <a name="updateprogress-control-reference"></a>UpdateProgress 控制項參考

啟用標記的屬性：

| **屬性名稱** | **Type** | **描述** |
| --- | --- | --- |
| AssociatedUpdate-PanelID | String | 指定應該報告此 UpdateProgress UpdatePanel 的識別碼。 |
| DisplayAfter | Int | 開始非同步要求之後，會顯示此控制項之前，請以毫秒為單位指定的逾時。 |
| DynamicLayout | bool | 指定是否要以動態方式呈現進度。 |

標記下階：

| **標記** | **描述** |
| --- | --- |
| &lt;ProgressTemplate&gt; | 包含設定將會顯示與這個控制項之內容的 [控制項] 範本。 |

UpdateProgress 控制項可讓您的意見反應以進行必要的工作來傳輸到伺服器時保留您的使用者感興趣的量值。 這可協助使用者了解，您進行一些動作，即使它可能不明顯，尤其是大部分的使用者用來重新整理，並查看 [狀態] 列反白顯示頁面。

為附註，UpdateProgress 控制項可以出現任何位置上的頁面階層。 不過，在部分回傳源頭子系 （UpdatePanel 巢狀另一個 UpdatePanel） 的 UpdatePanel 的情況下，回傳的觸發程序 UpdatePanel 會導致 UpdateProgress 範本顯示為子系的子系UpdatePanel，以及 UpdatePanel 的父系。 但是，如果觸發程序的直接子系的父系 UpdatePanel，則只有與父代相關聯的 UpdateProgress 範本會顯示。

## <a name="summary"></a>總結

Microsoft ASP.NET AJAX 擴充功能是設計來協助做出更容易存取的 web 內容，並提供更豐富的使用者體驗，您的 web 應用程式的複雜的產品。 ASP.NET AJAX Extensions 中的部分頁面轉譯控制項中，一部分包括 ScriptManager，UpdatePanel 和 UpdateProgress 控制項是一些最明顯的工具組元件。

ScriptManager 元件整合擴充功能中，佈建用戶端 JavaScript，以及可讓各種伺服器和用戶端元件以搭配最低限度的開發投資一起運作。

UpdatePanel 控制項是最明顯的魔術方塊-標記在 UpdatePanel 可包含伺服器端程式碼後置，而不會觸發重新整理頁面。 UpdatePanel 控制項可以巢狀，並可能相依於其他 Updatepanel 中的控制項。 根據預設，Updatepanel 會處理由及其子系的控制項，叫用任何回傳，雖然這項功能可以細微調整，以宣告方式或以程式設計方式。

當使用 UpdatePanel 控制項，開發人員應該要注意的可能，就會發生的效能影響。 可能的替代方案包括 web 服務和頁面方法，但應視為應用程式的設計。

UpdateProgress 控制項可讓使用者知道她或他不會被忽略，並在幕後的要求將頁面不會執行任何動作來回應使用者輸入。 它也包含中止部分呈現結果。

在一起，這些工具可協助建立豐富且順暢的使用者體驗的伺服器工作使用者可較不明顯，較不中斷工作流程。

## <a name="bio"></a>簡歷

Scott Cate 從事 Microsoft Web 技術工作自 1997 年，而且 myKB.com 總裁 ([www.myKB.com](http://www.myKB.com))，專精於撰寫 ASP.NET 架構著重於知識庫軟體解決方案的應用程式。 可以透過電子郵件連絡 Scott [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com)或他的部落格[ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [下一步](understanding-asp-net-ajax-updatepanel-triggers.md)
