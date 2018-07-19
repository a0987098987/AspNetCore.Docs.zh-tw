---
uid: web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
title: ASP.NET 2.0 頁面模型 |Microsoft Docs
author: microsoft
description: 在 ASP.NET 1.x 中，開發人員必須內嵌程式碼模型和程式碼後置程式碼模型之間做選擇。 無法使用其中一個的 Src 屬性來實作程式碼後置...
ms.author: aspnetcontent
ms.date: 02/20/2005
ms.assetid: af4575a3-0ae3-4638-ba4d-218fad7a1642
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
msc.type: authoredcontent
ms.openlocfilehash: 2c8a0624af30d93d2ba68dc4b0b0880d6e371616
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811697"
---
<a name="the-aspnet-20-page-model"></a>ASP.NET 2.0 頁面模型
====================
by [Microsoft](https://github.com/microsoft)

> 在 ASP.NET 1.x 中，開發人員必須內嵌程式碼模型和程式碼後置程式碼模型之間做選擇。 無法使用的 Src 屬性或程式碼後置屬性的實作程式碼後置@Page指示詞。 在 ASP.NET 2.0 中，開發人員仍然必須選擇內嵌程式碼和程式碼後置，但已有顯著的增強功能的程式碼後置模型。


在 ASP.NET 1.x 中，開發人員必須內嵌程式碼模型和程式碼後置程式碼模型之間做選擇。 無法使用的 Src 屬性或程式碼後置屬性的實作程式碼後置@Page指示詞。 在 ASP.NET 2.0 中，開發人員仍然必須選擇內嵌程式碼和程式碼後置，但已有顯著的增強功能的程式碼後置模型。

## <a name="improvements-in-the-code-behind-model"></a>在程式碼後置模型中的增強功能

若要完全了解 ASP.NET 2.0 中的程式碼後置模型中的變更，最好先快速的模型，因為它存在於 ASP.NET 1.x。

## <a name="the-code-behind-model-in-aspnet-1x"></a>程式碼後置模型，在 ASP.NET 1.x

在 ASP.NET 1.x 中，程式碼後置模型所組成的 ASPX 檔案 (Webform) 和包含在程式碼的程式碼後置檔案。 兩個檔案連接使用@PageASPX 檔案中的指示詞。 每個 ASPX 頁面上的控制項程式碼後置檔案中有對應的宣告，做為執行個體變數。 程式碼後置檔案也包含事件繫結的程式碼，而且產生的程式碼所需的 Visual Studio 設計工具。 此模型運作很不錯，但每個 ASPX 頁面中的 ASP.NET 項目所需的程式碼後置檔案中的對應程式碼，因為沒有，則為 true 的區隔的程式碼和內容。 例如，如果設計工具會加入 Visual Studio IDE 外部 ASPX 檔案中的新的伺服器控制項，應用程式會破壞由於沒有宣告該控制項的程式碼後置檔案中。

## <a name="the-code-behind-model-in-aspnet-20"></a>ASP.NET 2.0 中的程式碼後置模型

在此模型時可大幅提升 ASP.NET 2.0。 在 ASP.NET 2.0 中，程式碼後置會實作使用新*部分類別*ASP.NET 2.0 中提供。 在 ASP.NET 2.0 中的程式碼後置類別是定義為部分類別，表示它包含的類別定義的組件。 使用 ASPX 頁面，在執行階段，或當網站已先行編譯的 ASP.NET 2.0 來動態產生的類別定義的其餘部分。 仍然使用 @ Page 指示詞來建立程式碼後置檔案與 ASPX 頁面之間的連結。 不過，而不是程式碼後置或 Src 屬性，ASP.NET 2.0 現在會使用 CodeFile 屬性。 Inherits 屬性也用於指定頁面的類別名稱。

典型的 @ Page 指示詞可能如下所示：

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample1.aspx)]

ASP.NET 2.0 程式碼後置檔案中的一般類別定義可能如下所示：

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample2.cs)]

> [!NOTE]
> C# 和 Visual Basic 是唯一的 managed 的語言目前支援部分類別。 因此，使用 J# 開發人員將無法使用 ASP.NET 2.0 中的程式碼後置模型。


新的模型會增強程式碼後置模型，因為開發人員現在會有包含他們所建立的程式碼的程式碼檔案。 它也會提供 true 隔離的程式碼和內容，因為沒有任何執行個體的變數宣告中的程式碼後置檔案。

> [!NOTE]
> ASPX 頁面的部分類別是事件繫結發生的地方，因為 Visual Basic 開發人員可以利用控點關鍵字在程式碼後置中，若要將事件繫結實現輕微的效能增加。 C# 有沒有對等的關鍵字。


## <a name="new--page-directive-attributes"></a>新的 @ Page 指示詞屬性

ASP.NET 2.0 會將 @ Page 指示詞中的許多新的屬性。 下列屬性是在 ASP.NET 2.0 新功能。

## <a name="async"></a>Async

Atribut Async 可讓您設定頁面，以非同步方式執行。 我們將介紹稍後在本單元中的非同步頁面。

## <a name="asynctimeout"></a>AsyncTimeout

指定的非同步頁面的逾時。 預設值為 45 秒。

## <a name="codefile"></a>CodeFile

CodeFile 屬性會在 Visual Studio 2002/2003年 CodeBehind 屬性取代。

### <a name="codefilebaseclass"></a>CodeFileBaseClass

在您想要衍生自單一基底類別的多個頁面的情況下使用 CodeFileBaseClass 屬性。 由於在 ASP.NET 中，而這個屬性，不需要的部分類別的實作會使用共用通用的欄位來參考在 ASPX 頁面中宣告的控制項的基底類別將無法正常運作因為 ASP。網路編譯引擎會自動建立 頁面中的控制項為基礎的新成員。 因此，如果您想要在 ASP.NET 中的兩個或多個頁面通用的基底類別，您必須定義 CodeFileBaseClass 屬性中指定基底類別，然後再衍生自該基底類別的每個頁面的類別。 使用此屬性時，也是必要 CodeFile 屬性。

## <a name="compilationmode"></a>CompilationMode

這個屬性可讓您設定在 ASPX 頁面的 CompilationMode 屬性。 CompilationMode 屬性是包含值的列舉型別**永遠**，**自動**，並**永不**。 預設值是**永遠**。 **自動**設定將使 ASP.NET 從動態編譯的頁面的話。 排除動態編譯的頁面，可以提升效能。 不過，如果已排除的頁面包含必須編譯該程式碼，將會擲回錯誤時瀏覽網頁時。

## <a name="enableeventvalidation"></a>EnableEventValidation

這個屬性會指定驗證回傳和回呼事件。 當啟用此選項時，回傳的引數或回撥的事件會檢查以確定其來自原本呈現它們的伺服器控制項中。

## <a name="enabletheming"></a>EnableTheming

這個屬性會指定在頁面上使用 ASP.NET 佈景主題。 預設值為 **false**。 ASP.NET 佈景主題所述[模組 10](profiles-themes-and-web-parts.md)。

## <a name="linepragmas"></a>LinePragmas

這個屬性會指定是否應在編譯期間新增列 pragma。 行是由偵錯工具，用於標示的程式碼的特定區段的選項。

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostback

這個屬性會指定 JavaScript 會插入頁面維持回傳之間的捲軸位置。 這個屬性是**false**預設。

當這個屬性是 **，則為 true**，將 ASP.NET&lt;指令碼&gt;區塊在回傳時，看起來像這樣：

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample3.html)]

請注意，此指令碼區塊的 src WebResource.axd。 此資源不是實體路徑。 此指令碼要求時，ASP.NET 會動態建立指令碼。

### <a name="masterpagefile"></a>MasterPageFile

這個屬性會指定目前頁面的主版頁面檔案。 路徑可以是相對或絕對。 主版頁面所述[模組 4](master-pages.md)。

## <a name="stylesheettheme"></a>StyleSheetTheme

這個屬性可讓您覆寫 ASP.NET 2.0 主題所定義的使用者介面外觀屬性。 佈景主題所述[模組 10](profiles-themes-and-web-parts.md)。

## <a name="theme"></a>主題

指定網頁的佈景主題。 如果值不指定 StyleSheetTheme 屬性，佈景主題屬性會覆寫套用至頁面上控制項的所有樣式。

## <a name="title"></a>標題

設定頁面的標題。 在這裡指定的值會出現在&lt;title&gt;呈現之網頁的項目。

### <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

設定裡把 ViewStateEncryptionMode 列舉的值。 可用的值為**永遠**，**自動**，並**永不**。 預設值是**自動**。當這個屬性設定為值**自動**，viewstate 會加密是控制項要求呼叫**RegisterRequiresViewStateEncryption**方法。

## <a name="setting-public-property-values-via-the--page-directive"></a>設定公用屬性值透過 @ Page 指示詞

在 ASP.NET 2.0 中的 @ Page 指示詞的另一個新功能是能夠設定基底類別的公用屬性的初始值。 假設您擁有的公用屬性，例如呼叫**SomeText**基底類別和您喜歡 d 初始化為**Hello**網頁載入時。 您可以直接在 @ Page 指示詞中設定值來完成這就像這樣：

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample4.aspx)]

**SomeText** @ Page 指示詞屬性的基底類別中設定 SomeText 屬性的初始值*Hello ！*。 下列影片會逐步解說中使用 @ Page 指示詞的基底類別中設定的公用屬性的初始值。


![](the-asp-net-2-0-page-model/_static/image1.png)


[開啟全螢幕影片](the-asp-net-2-0-page-model/_static/setprop1.wmv)


## <a name="new-public-properties-of-the-page-class"></a>頁面類別的新公用屬性

下列公用屬性是在 ASP.NET 2.0 新功能。

## <a name="apprelativetemplatesourcedirectory"></a>AppRelativeTemplateSourceDirectory

回到頁面或控制項的應用程式相對路徑。 例如，針對位於頁面 http://app/folder/page.aspx，此屬性傳回 ~ / 資料夾 /。

## <a name="apprelativevirtualpath"></a>AppRelativeVirtualPath

回到頁面或控制項的相對虛擬目錄路徑。 例如對於頁面位於 http://app/folder/page.aspx，此屬性傳回 ~ / folder/page.aspx。

## <a name="asynctimeout"></a>AsyncTimeout

取得或設定用於非同步頁面處理的逾時。 （稍後在本單元將說明非同步網頁）。

## <a name="clientquerystring"></a>ClientQueryString

唯讀屬性，傳回要求的 URL 查詢字串部分。 這個值是 URL 編碼。 您可以使用 UrlDecode HttpServerUtility 類別方法，將它解碼。

## <a name="clientscript"></a>ClientScript

這個屬性會傳回可用來管理用戶端指令碼 ASP.NETs 發出 ClientScriptManager 物件。 （ClientScriptManager 類別稍後在本單元涵蓋）。

## <a name="enableeventvalidation"></a>EnableEventValidation

此屬性控制啟用事件驗證回傳和回呼事件。 啟用時，要回傳引數或回撥的事件會驗證以確保它們來自原本呈現它們的伺服器控制項。

## <a name="enabletheming"></a>EnableTheming

此屬性會取得或設定布林值，指定 ASP.NET 2.0 主題套用至頁面。

## <a name="form"></a>表單

這個屬性會傳回做為 HtmlForm 物件 ASPX 頁面上的 HTML 表單。

## <a name="header"></a>頁首

這個屬性會傳回包含頁首的 HtmlHead 物件的參考。 您可以使用傳回的 HtmlHead 物件來取得/設定樣式表、 中繼標籤等等。

## <a name="idseparator"></a>IdSeparator

這個唯讀屬性取得用來分隔控制識別項，當 ASP.NET 建置頁面上控制項的唯一識別碼的字元。 但並不是針對直接從程式碼使用而設計。

## <a name="isasync"></a>IsAsync

這個屬性允許非同步頁面。 此模組稍後會討論非同步頁面。

## <a name="iscallback"></a>IsCallback

這個唯讀屬性會傳回 **，則為 true**頁面是否為回呼的結果。 回撥會討論這個模組中的更新版本。

## <a name="iscrosspagepostback"></a>IsCrossPagePostBack

這個唯讀屬性會傳回 **，則為 true**如果頁面是跨網頁回傳的一部分。 稍後在本單元涵蓋跨網頁回傳。

## <a name="items"></a>項目

傳回 IDictionary 執行個體，其中包含儲存在頁面內容中的所有物件的參考。 您可以加入這個 IDictionary 物件中的項目，並將其內容的存留期使用。

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostBack

此屬性控制 ASP.NET 會發出 JavaScript，維持頁面捲動瀏覽器中的位置之後會發生回傳。 （稍早在本單元中所討論的這個屬性的詳細資料）。

## <a name="master"></a>Master

這個唯讀屬性會傳回已套用主版頁面的頁面 MasterPage 執行個體的參考。

## <a name="masterpagefile"></a>MasterPageFile

取得或設定頁面的主版頁面檔案名稱。 這個屬性只能設定 PreInit 方法中。

## <a name="maxpagestatefieldlength"></a>MaxPageStateFieldLength

此屬性會取得或設定頁面狀態的最大長度 （位元組）。 如果屬性設定為正數，頁面檢視狀態將會分成多個隱藏的欄位，讓它不會超過指定的位元組數目。 如果屬性是負數，檢視狀態不會分割成區塊 （chunk）。

## <a name="pageadapter"></a>PageAdapter

傳回修改的頁面要求的瀏覽器 PageAdapter 物件的參考。

## <a name="previouspage"></a>上一頁

在 Server.Transfer 或跨網頁回傳的情況下會傳回前一頁的參考。

## <a name="skinid"></a>SkinID

指定要套用至網頁的 ASP.NET 2.0 面板。

## <a name="stylesheettheme"></a>StyleSheetTheme

此屬性會取得或設定套用至頁面的樣式表。

## <a name="templatecontrol"></a>TemplateControl

傳回包含控制項頁面的參考。

## <a name="theme"></a>主題

取得或設定套用至網頁的 ASP.NET 2.0 主題的名稱。 PreInit 方法之前，必須設定此值。

## <a name="title"></a>標題

此屬性會取得或設定頁面的標題，取得頁面標頭。

## <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

取得或設定頁面的裡把 ViewStateEncryptionMode。 請參閱稍早在本單元中的這個屬性的詳細的討論。

## <a name="new-protected-properties-of-the-page-class"></a>頁面類別的新受保護的內容

以下是在 ASP.NET 2.0 頁面類別的新受保護的內容。

## <a name="adapter"></a>配接器

傳回的參考會呈現網頁在裝置上的 ControlAdapter 提出此要求。

## <a name="asyncmode"></a>AsyncMode

這個屬性會指出以非同步方式處理頁面。 它適用於執行階段，而非直接在程式碼。

## <a name="clientidseparator"></a>ClientIDSeparator

這個屬性會傳回為控制項建立唯一的用戶端識別碼時，做為分隔符號的字元。 它適用於執行階段，而非直接在程式碼。

## <a name="pagestatepersister"></a>PageStatePersister

這個屬性會傳回頁面 PageStatePersister 物件。 這個屬性主要是由 ASP.NET 控制項開發人員使用。

## <a name="uniquefilepathsuffix"></a>UniqueFilePathSuffix

這個屬性會傳回唯一的 suffic 附加至瀏覽器的快取的檔案路徑。 預設值是\_ \_ufps = 和 6 位數的數字。

## <a name="new-public-methods-for-the-page-class"></a>Page 類別的新公用方法

下列公用方法是在 ASP.NET 2.0 Page 類別的新項目。

## <a name="addonprerendercompleteasync"></a>AddOnPreRenderCompleteAsync

這個方法會註冊為非同步頁面執行的事件處理常式委派。 此模組稍後會討論非同步頁面。

## <a name="applystylesheetskin"></a>ApplyStyleSheetSkin

頁面樣式表中的屬性套用至頁面中。

## <a name="executeregisteredasynctasks"></a>ExecuteRegisteredAsyncTasks

這個方法就非同步工作。

### <a name="getvalidators"></a>GetValidators

如果未指定，則傳回驗證指定的驗證群組或預設的驗證群組的集合。

## <a name="registerasynctask"></a>RegisterAsyncTask

這個方法會註冊新的非同步工作。 稍後在本單元涵蓋的非同步頁面。

## <a name="registerrequirescontrolstate"></a>RegisterRequiresControlState

這個方法會告訴 ASP.NET 頁面控制項狀態必須 persisted。

## <a name="registerrequiresviewstateencryption"></a>RegisterRequiresViewStateEncryption

這個方法會告知 ASP.NET 頁面 viewstate，需要加密。

## <a name="resolveclienturl"></a>ResolveClientUrl

傳回可用的映像等的用戶端要求的相對 URL。

## <a name="setfocus"></a>SetFocus

這個方法會將焦點設定至一開始載入頁面時指定的控制項。

## <a name="unregisterrequirescontrolstate"></a>UnregisterRequiresControlState

這個方法會取消註冊為不再需要控制項狀態持續性傳遞給它的控制項。

## <a name="changes-to-the-page-lifecycle"></a>變更網頁生命週期

在 ASP.NET 2.0 中的頁面週期尚未變動很大，但有一些您應該要注意的新方法。 ASP.NET 2.0 頁面生命週期如下所述。

## <a name="preinit-new-in-aspnet-20"></a>（新的 ASP.NET 2.0) 中的 PreInit

PreInit 事件是在開發人員可以存取的生命週期中最早的階段。 這個事件的加入可讓您能夠以程式設計方式變更 ASP.NET 2.0 主題，主版頁面、 存取 ASP.NET 2.0 設定檔等的屬性。如果您是在回傳狀態，其必須了解，Viewstate 具有尚未套用至目前生命週期中的控制項。 因此，如果開發人員變更控制項的屬性在這個階段，它可能被覆寫稍後在頁面生命週期中。

## <a name="init"></a>Init

尚未變更 Init 事件，這是從 ASP.NET 1.x。 這是您想要讀取或初始化的頁面上控制項的屬性。 在此階段、 主版頁面、 佈景主題等已套用至網頁。

## <a name="initcomplete-new-in-20"></a>緊隨在 InitComplete （新功能 2.0)

緊隨在 InitComplete 事件會在頁面初始化階段結束時呼叫。 此時在生命週期中，您可以存取控制項在頁面上，但尚未填入其狀態。

## <a name="preload-new-in-20"></a>預先載入 （新功能 2.0）

已套用所有的回傳資料之後，頁面之前，會呼叫此事件\_負載。

## <a name="load"></a>Load

尚未變更的 Load 事件，這是從 ASP.NET 1.x。

## <a name="loadcomplete-new-in-20"></a>LoadComplete （新功能 2.0)

LoadComplete 事件是在頁面載入階段的最後一個事件。 在這個階段，回傳和 viewstate 的所有資料已都套用至頁面。

## <a name="prerender"></a>PreRender

如果您想要能夠正確保留會以動態方式加入至頁面的控制項的 viewstate，PreRender 事件就會是將它們加入的最後機會。

## <a name="prerendercomplete-new-in-20"></a>PreRenderComplete （新功能 2.0)

在 PreRenderComplete 階段中，所有控制項都已都加入至頁面，頁面可以開始呈現。 PreRenderComplete 事件是網頁檢視狀態儲存之前，所引發的最後一個事件。

## <a name="savestatecomplete-new-in-20"></a>SaveStateComplete （新功能 2.0)

SaveStateComplete 事件是只有在已儲存所有的頁面檢視狀態和控制項狀態之後，立即呼叫。 這是頁面實際轉譯至瀏覽器之前的最後一個事件。

## <a name="render"></a>轉譯

Render 方法尚未變更自 ASP.NET 1.x。 這是其中的 HtmlTextWriter 已初始化，且頁面會呈現在瀏覽器。

## <a name="cross-page-postback-in-aspnet-20"></a>ASP.NET 2.0 中的跨網頁回傳

在 ASP.NET 1.x 中，回傳都必須要張貼到相同的頁面。 不允許跨網頁回傳。 ASP.NET 2.0 新增回傳給不同的頁面，透過 IButtonControl 介面的能力。 實作新的 IButtonControl 介面 （按鈕、 LinkButton 和協力廠商自訂控制項除了 ImageButton） 的任何控制項可以利用這項新功能透過 PostBackUrl 屬性使用。 下列程式碼顯示的按鈕控制項回傳至第二個頁面。

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample5.aspx)]

頁面回傳，啟始回傳網頁時，可透過第二個頁面的 PreviousPage 屬性存取。 這項功能透過新的 WebForm 實作\_DoPostBackWithOptions 用戶端函式，會呈現 ASP.NET 2.0 頁面，但控制項回傳至不同的頁面。 這個 JavaScript 函式會提供新的 WebResource.axd 處理常式，這時用戶端的指令碼。

下列影片會跨網頁回傳的解說。


![](the-asp-net-2-0-page-model/_static/image2.png)


[開啟全螢幕影片](the-asp-net-2-0-page-model/_static/xpage1.wmv)


## <a name="more-details-on-cross-page-postbacks"></a>在跨網頁回傳的更多詳細資料

### <a name="viewstate"></a>Viewstate

您可能會要求您自己已經有關會發生什麼事 viewstate 在跨網頁回傳的案例中的第一頁從。 畢竟，任何控制項，不會實作 IPostBackDataHandler 會因此保存其狀態透過 viewstate，跨網頁回傳的第二個頁面上的該控制項的屬性存取，您必須能夠存取 viewstate 頁面。 ASP.NET 2.0 會處理此案例中使用新的隱藏的欄位，在呼叫的第二頁\_ \_PREVIOUSPAGE。 \_ \_PREVIOUSPAGE 表單欄位包含 viewstate 的第一頁，以便您可以在第二個頁面中的所有控制項的屬性的存取權。

### <a name="circumventing-findcontrol"></a>規避 FindControl

跨網頁回傳的視訊逐步解說中，在中，我可以使用的 FindControl 方法來取得第一頁上的文字方塊控制項的參考。 這個方法適用於這個目的，但 FindControl 昂貴，而且需要撰寫額外程式碼。 幸運的是，ASP.NET 2.0 提供可在許多情況下此用途 FindControl 的替代方案。 PreviousPageType 指示詞可讓您有使用 TypeName 或 VirtualPath 屬性的前一頁的強型別參考。 TypeName 屬性可讓您指定先前的頁面類型，VirtualPath 屬性可讓您參考前一個頁面使用的虛擬路徑。 設定 PreviousPageType 指示詞之後，您必須再公開控制項等您要允許存取使用的公用屬性。

## <a name="lab-1-cross-page-postback"></a>實驗室 1 跨網頁回傳

在此實驗室中，您將建立使用 ASP.NET 2.0 新的跨網頁回傳功能的應用程式。

1. 開啟 Visual Studio 2005 並建立新的 ASP.NET 網站。
2. 加入新的 Webform 呼叫 page2.aspx。
3. 在 [設計] 檢視中開啟 Default.aspx，並新增按鈕控制項和文字方塊控制項。 

    1. 提供按鈕控制項的 ID **SubmitButton**和文字方塊控制項的 ID **UserName**。
    2. 設 page2.aspx PostBackUrl 按鈕的屬性。
4. 開啟 page2.aspx 來源檢視中。
5. 加入 @ PreviousPageType 指示詞，如下所示：
6. 將下列程式碼新增至頁面\_page2.aspx 的程式碼後置的負載： 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample6.cs)]
7. 在 [建置] 功能表上按一下組建中建置專案。
8. 程式碼後置 Default.aspx 中加入下列程式碼： 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample7.cs)]
9. 變更頁面\_負載 page2.aspx 如下： 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample8.cs)]
10. 建置專案。
11. 執行專案。
12. 在文字方塊中輸入您的名稱，然後按一下  按鈕。
13. 什麼是結果？

## <a name="asynchronous-pages-in-aspnet-20"></a>ASP.NET 2.0 中的非同步頁面

在 ASP.NET 中的許多競爭問題被因外部呼叫 （例如 Web 服務或資料庫呼叫），檔案 IO 延遲等等的延遲。當要求是針對 ASP.NET 應用程式，ASP.NET 會使用其中一個背景工作執行緒來服務該要求。 該要求會擁有該執行緒，直到完成要求且在傳送回應。 ASP.NET 2.0 會設法加入以非同步方式執行頁面的功能來解決這類問題與延遲問題。 這表示工作者執行緒可啟動要求，並接著遞交到另一個執行緒，進而快速地返回可用的執行緒集區的其他執行。 檔案 IO、 資料庫呼叫等完成時，新的執行緒被取自來完成要求的執行緒集區中。

讓頁面，以非同步方式執行的第一個步驟是設定**非同步**頁面指示詞屬性就像這樣：

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample9.aspx)]

此屬性會告知 ASP.NET 實作 IHttpAsyncHandler 頁面。

下一個步驟是在生命週期中頁面在 PreRender 之前的時間點呼叫 AddOnPreRenderCompleteAsync 方法。 (在頁面中通常會呼叫此方法\_負載。)AddOnPreRenderCompleteAsync 方法採用兩個參數;BeginEventHandler 和 EndEventHandler。 BeginEventHandler 傳回再做為參數傳遞至 EndEventHandler IAsyncResult。

下列影片會逐步解說中的非同步頁面要求。


![](the-asp-net-2-0-page-model/_static/image3.png)


[開啟全螢幕影片](the-asp-net-2-0-page-model/_static/async1.wmv)


> [!NOTE]
> 非同步頁面不會呈現至瀏覽器，直到完成 EndEventHandler。 毫無疑問，但有些開發人員會將視為類似非同步回呼的非同步要求。 請務必了解它們不是。 非同步要求的好處是第一個背景工作執行緒可以回到執行緒集區，才能服務新要求，因此也降低因 IO 繫結的爭用等等。


## <a name="script-callbacks-in-aspnet-20"></a>Script Callbacks in ASP.NET 2.0

Web 開發人員一律尋找的方法可防止與回呼相關聯的閃爍。 在 ASP.NET 1.x，SmartNavigation 避免閃爍的最常見的方法卻 SmartNavigation 某些開發人員造成問題，因為其在用戶端上實作的複雜性。 ASP.NET 2.0 解決這個問題的指令碼回呼。 指令碼回呼使用 XMLHttp 對透過 JavaScript 的網頁伺服器提出要求。 XMLHttp 要求會傳回 XML 資料，然後可操作透過瀏覽器的 dom。 XMLHttp 程式碼隱藏使用者從新的 WebResource.axd 處理常式。

有幾個步驟所需以 ASP.NET 2.0 中設定指令碼回呼。

## <a name="step-1--implement-the-icallbackeventhandler-interface"></a>步驟 1： 實作 ICallbackEventHandler 介面

為了讓 ASP.NET 才能辨識您的頁面為參與的指令碼回呼，您必須實作 ICallbackEventHandler 介面。 您可以在您的程式碼後置檔案就像這樣：

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample10.cs)]

您也可以執行這讓使用 @ Implements 指示詞類似：

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample11.aspx)]

使用內嵌 ASP.NET 程式碼時，您通常會使用 @ Implements 指示詞。

## <a name="step-2--call-getcallbackeventreference"></a>步驟 2： 呼叫 GetCallbackEventReference

如先前所述，XMLHttp 呼叫封裝中的 WebResource.axd 處理常式。 ASP.NET 呈現您的頁面時，會將呼叫加入 WebForm\_DoCallback，由 WebResource.axd 提供用戶端指令碼。 WebForm\_DoCallback 函式會取代\_ \_doPostBack 函式的回呼。 請記住\_ \_doPostBack 以程式設計方式提交頁面上的表單。 在回撥案例中，您想要防止回傳，因此\_ \_doPostBack 會不敷使用。

> [!NOTE]
> \_\_doPostBack 仍會呈現在用戶端指令碼回呼案例頁面。 不過，它不會使用回呼。


WebForm 的引數\_DoCallback 用戶端函式會提供透過伺服器端函式通常會在頁面中稱為 GetCallbackEventReference\_負載。 GetCallbackEventReference 的典型呼叫看起來可能像這樣：

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample12.cs)]

> [!NOTE]
> 在此情況下，cm 是 ClientScriptManager 執行個體。 本單元之後，將會說明 ClientScriptManager 類別。


有數個 GetCallbackEventReference 的多載的版本。 在此情況下，引數是的如下所示：

`this`

正在呼叫 GetCallbackEventReference 控制項的參考。 在此情況下，它是網頁本身。

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample13.js)]

字串引數會從用戶端程式碼傳遞至伺服器端事件。 在此情況下，傳遞值的下拉式清單中的 Im 呼叫 ddlCompany。

`ShowCompanyName`

（如字串） 要接受的傳回值的伺服器端回呼事件的用戶端函式名稱。 伺服器端回呼成功時，只會呼叫此函數。 因此，為了穩定性，通常建議使用 GetCallbackEventReference 採用指定的名稱，用戶端函式發生錯誤時所執行的其他字串引數的多載的版本。

`null`

字串，表示系統已起始與伺服器在回呼之前的用戶端函式。 在此情況下，沒有任何這類指令碼，因此引數為 null。

`true`

布林值，指定要以非同步方式進行回呼。

WebForm 呼叫\_DoCallback 在用戶端會將這些引數傳遞。 因此，當用戶端上呈現此頁面時，該程式碼看起來就像這樣：

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample14.js)]

請注意用戶端上的函式的簽章會有點不同。 用戶端函式會將 5 個字串和布林值傳遞。 （這是在上述範例中為 null） 的其他字串包含用戶端函式會處理任何錯誤的伺服器端的回呼。

## <a name="step-3--hook-the-client-side-control-event"></a>步驟 3： 將連結的用戶端控制項事件

請注意，上述 GetCallbackEventReference 的傳回值已指派給字串變數。 該字串用來將連結控制項初始化回呼用戶端事件。 在此範例中，回呼由下拉式清單中的頁面上，我想要將連結*OnChange*事件。

若要攔截用戶端事件，只要將處理常式加入用戶端標記，如下所示：

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample15.cs)]

請記得， *cbRef* GetCallbackEventReference 呼叫的傳回值。 它包含 WebForm 呼叫\_已如上所示的 DoCallback。

## <a name="step-4--register-the-client-side-script"></a>步驟 4： 註冊用戶端指令碼

回想一下，GetCallbackEventReference 呼叫指定的用戶端指令碼，呼叫**ShowCompanyName**會在伺服器端回呼成功時執行。 該指令碼必須加入至頁面使用 ClientScriptManager 執行個體。 （ClientScriptManager 類別將會是這稍後在本單元中的）。因此，您執行該類似：

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample16.js)]

## <a name="step-5--call-the-methods-of-the-icallbackeventhandler-interface"></a>步驟 5： 呼叫 ICallbackEventHandler 介面的方法

ICallbackEventHandler 包含您要在您的程式碼中實作的兩個方法。 它們**RaiseCallbackEvent**並**GetCallbackEvent**。

**RaiseCallbackEvent**會做為引數的字串，並且會傳回任何內容。 字串引數會傳遞從用戶端呼叫 WebForm\_DoCallback。 在此案例中，該值是*值*呼叫 ddlCompany 下拉式清單中的屬性。 您的伺服器端程式碼應該置於 RaiseCallbackEvent 方法。 例如，如果您的回呼正在對外部資源的 WebRequest，該程式碼應該置於 RaiseCallbackEvent。

**GetCallbackEvent**負責處理回呼的傳回至用戶端。 它會採用任何引數，並傳回字串。 它會傳回的字串會傳遞做為引數至用戶端函式，在此情況下*ShowCompanyName*。

完成上述步驟後，您已準備好在 ASP.NET 2.0 中執行的指令碼回呼。


![](the-asp-net-2-0-page-model/_static/image4.png)


[開啟全螢幕影片](the-asp-net-2-0-page-model/_static/callback1.wmv)


支援在 ASP.NET 中的指令碼回呼支援讓 XMLHttp 呼叫任何瀏覽器中。 其中包含所有現代瀏覽器中使用今天。 其他 （包括即將推出 IE 7） 的現代瀏覽器使用的內建的 XMLHttp 物件時，Internet Explorer 會使用 XMLHttp ActiveX 物件。 若要以程式設計方式判斷是否瀏覽器支援回呼，您可以使用**Request.Browser.SupportCallback**屬性。 這個屬性會傳回 **，則為 true**如果要求的用戶端支援指令碼回呼。

## <a name="working-with-client-script-in-aspnet-20"></a>使用 ASP.NET 2.0 中的用戶端指令碼

透過使用 ClientScriptManager 類別來管理 ASP.NET 2.0 中的用戶端指令碼。 ClientScriptManager 類別會追蹤的用戶端指令碼使用的型別和名稱。 這可防止相同的指令碼要以程式設計方式插入頁面上一次以上。

> [!NOTE]
> 指令碼已成功註冊頁面上之後，任何後續的嘗試註冊相同的指令碼只會在指令碼中未註冊第二次。 加入任何重複的指令碼，並沒有發生例外狀況。 若要避免不必要的計算，有可用來判斷指令碼是否已經註冊，讓您不會嘗試向一次以上的方法。


ClientScriptManager 方法應該是所有目前的 ASP.NET 開發人員所熟悉的：

## <a name="registerclientscriptblock"></a>RegisterClientScriptBlock

這個方法會將指令碼，來呈現頁面的頂端。 這可用於新增將用戶端明確呼叫的函式。

有兩個多載的版本，這個方法。 三個四個引數是在它們之間通用的。 包括：

`type (string)`

***型別***引數會識別指令碼的類型。 它通常是個不錯的主意，若要使用的頁面類型 （這點。GetType()) 類型。

`key (string)`

***金鑰***引數是使用者定義索引鍵的指令碼。 這應該是唯一的每個指令碼。 如果您嘗試加入具有相同的索引鍵和型別已新增的指令碼的指令碼，它將不會加入。

`script (string)`

***指令碼***引數是字串，包含要新增的實際指令碼。 建議您在建立指令碼，然後在指派的 StringBuilder 上使用 tostring （） 方法使用 StringBuilder***指令碼***引數。

如果您使用多載的 RegisterClientScriptBlock 只接受三個引數時，您必須包括指令碼項目 (&lt;指令碼&gt;並&lt;/script&gt;) 指令碼中。

您可以選擇使用 RegisterClientScriptBlock 接受第四個引數的多載。 第四個引數是布林值，指定 ASP.NET 應該為您新增指令碼項目。 如果這個引數是 **，則為 true**，您的指令碼不應包含的指令碼項目明確。

您可以使用 IsClientScriptBlockRegistered 方法來判斷是否已登錄的指令碼。 這可讓您避免嘗試重新註冊已註冊的指令碼。

### <a name="registerclientscriptinclude-new-in-20"></a>RegisterClientScriptInclude （新功能 2.0)

RegisterClientScriptInclude 標記建立指令碼區塊連結至外部指令碼檔案。 它有兩個多載。 其中一個採用索引鍵和 URL。 第二個加入的指定類型的第三個引數。

例如，下列程式碼會產生指令碼區塊連結至 jsfunctions.js 根目錄中的應用程式的 [指令碼] 資料夾：

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample17.cs)]

此程式碼會產生下列程式碼在呈現的頁面：

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample18.html)]

> [!NOTE]
> 在頁面底部轉譯的指令碼區塊。


您可以使用 IsClientScriptIncludeRegistered 方法來判斷是否已登錄的指令碼。 這可讓您避免嘗試重新註冊的指令碼。

## <a name="registerstartupscript"></a>RegisterStartupScript

RegisterStartupScript 方法採用相同的引數，做為 RegisterClientScriptBlock 方法。 在頁面載入之後但在這段用戶端之前，就會執行向 RegisterStartupScript 指令碼。 在 1.X 中，向 RegisterStartupScript 的指令碼已置於結尾之前&lt;/形成&gt;標記時向 RegisterClientScriptBlock 的指令碼放置在開頭之後立即&lt;表單&gt;標記。 在 ASP.NET 2.0 中，同時會放在結尾之前，立即&lt;/形成&gt;標記。

> [!NOTE]
> 如果您向 RegisterStartupScript 函式，該函式將不會執行，直到明確地在用戶端程式碼中呼叫。


您可以使用 IsStartupScriptRegistered 方法來判斷是否已登錄的指令碼，並避免嘗試重新註冊的指令碼。

## <a name="other-clientscriptmanager-methods"></a>其他 ClientScriptManager 方法

以下是一些 ClientScriptManager 類別的其他有用的方法。


|  <strong>GetCallbackEventReference</strong>   |                                                 請參閱稍早在本單元中的指令碼回呼。                                                 |
|-----------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|  <strong>GetPostBackClientHyperlink</strong>  |                取得 JavaScript 參考 (javascript:&lt;呼叫&gt;)，可用來從用戶端事件回傳。                 |
|  <strong>GetPostBackEventReference</strong>   |                                   取得字串，可用來起始從用戶端 post。                                    |
|      <strong>GetWebResourceUrl</strong>       | 內嵌組件中的資源傳回的 URL。 必須用於搭配<strong>RegisterClientScriptResource</strong>。 |
| <strong>RegisterClientScriptResource</strong> |     向頁面註冊 Web 資源。 這些是內嵌在組件，而且新的 WebResource.axd 處理常式所處理的資源。      |
|     <strong>RegisterHiddenField</strong>      |                                                 向頁面註冊隱藏的表單欄位。                                                 |
|  <strong>RegisterOnSubmitStatement</strong>   |                                  註冊用戶端程式碼的 HTML 表單提交時執行。                                   |

