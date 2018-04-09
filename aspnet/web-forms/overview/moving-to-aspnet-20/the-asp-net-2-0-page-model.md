---
uid: web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
title: ASP.NET 2.0 頁面模型 |Microsoft 文件
author: microsoft
description: 在 ASP.NET 中 1.x，開發人員就必須是內嵌程式碼模型與程式碼後置程式碼模型之間的選擇。 使用任一個 Src attr 實作程式碼後置...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: af4575a3-0ae3-4638-ba4d-218fad7a1642
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
msc.type: authoredcontent
ms.openlocfilehash: fda85ec03f845cafa7720382bf85652937932c44
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="the-aspnet-20-page-model"></a>ASP.NET 2.0 頁面模型
====================
by [Microsoft](https://github.com/microsoft)

> 在 ASP.NET 中 1.x，開發人員就必須是內嵌程式碼模型與程式碼後置程式碼模型之間的選擇。 無法使用的 Src 屬性或程式碼後置屬性的實作程式碼後置@Page指示詞。 在 ASP.NET 2.0 中，開發人員仍然可以內嵌程式碼和程式碼後置之間選擇，但是已有程式碼後置模型的重大增強功能。


在 ASP.NET 中 1.x，開發人員就必須是內嵌程式碼模型與程式碼後置程式碼模型之間的選擇。 無法使用的 Src 屬性或程式碼後置屬性的實作程式碼後置@Page指示詞。 在 ASP.NET 2.0 中，開發人員仍然可以內嵌程式碼和程式碼後置之間選擇，但是已有程式碼後置模型的重大增強功能。

## <a name="improvements-in-the-code-behind-model"></a>程式碼後置模型中的增強功能

若要完全了解程式碼後置模型，在 ASP.NET 2.0 中的變更，可快速檢閱模型的原貌最好存在於 ASP.NET 1.x。

## <a name="the-code-behind-model-in-aspnet-1x"></a>在 ASP.NET 中的程式碼後置模型 1.x

在 ASP.NET 中 1.x，程式碼後置模型組成 ASPX 檔案 (Webform) 包含在程式碼的程式碼後置檔案。 兩個檔案連接使用@PageASPX 檔指示詞。 每個 ASPX 頁面上的控制項的程式碼後置檔案中具有對應的宣告，做為執行個體變數。 程式碼後置檔案也包含事件繫結的程式碼，而且產生的程式碼所需的 Visual Studio 設計工具。 此模型中的工作很不錯，但因為每個 ASPX 頁面中的 ASP.NET 項目所需的程式碼後置檔案中對應的程式碼，沒有，則為 true 的程式碼和分隔內容。 例如，如果設計工具會將新的伺服器控制項加入 Visual Studio IDE 外部 ASPX 檔，應用程式會破壞因為沒有宣告該控制項的程式碼後置檔案中。

## <a name="the-code-behind-model-in-aspnet-20"></a>程式碼後置模型，在 ASP.NET 2.0

在此模型時可大幅提升 ASP.NET 2.0。 在 ASP.NET 2.0 中，程式碼後置會實作使用新*部分類別*ASP.NET 2.0 中提供。 在 ASP.NET 2.0 中的程式碼後置類別是定義為部分類別，表示它包含的類別定義的組件。 類別定義的其餘部分是動態產生的 ASP.NET 2.0 使用 ASPX 頁面，在執行階段，或在先行編譯網站。 程式碼後置檔案與 ASPX 頁面之間的連結仍是使用 @ Page 指示詞已建立。 不過，而不是程式碼後置或 Src 屬性，ASP.NET 2.0 現在會使用 CodeFile 屬性。 Inherits 屬性也用於指定網頁的類別名稱。

典型的 @ Page 指示詞可能看起來像這樣：

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample1.aspx)]

ASP.NET 2.0 程式碼後置檔案中的一般類別定義看起來可能像這樣：

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample2.cs)]

> [!NOTE]
> C# 和 Visual Basic 是只有目前支援的部分類別的 managed 的語言。 因此，使用 J# 的開發人員將無法使用 ASP.NET 2.0 中的程式碼後置模型。


新的模型增強程式碼後置模型，因為開發人員現在會有包含他們所建立的程式碼的程式碼檔案。 它也會提供 true 隔離的程式碼和內容，因為沒有任何執行個體的變數宣告中的程式碼後置檔案。

> [!NOTE]
> ASPX 頁面的部分類別是事件繫結發生的位置，因為 Visual Basic 開發人員可以利用控點關鍵字的程式碼後置中，繫結事件實現輕微的效能增加。 C# 擁有沒有對等的關鍵字。


## <a name="new--page-directive-attributes"></a>新的 @ Page 指示詞屬性

ASP.NET 2.0 到 @ Page 指示詞加入許多新的屬性。 下列屬性是在 ASP.NET 2.0 的新功能。

## <a name="async"></a>Async

Async 屬性可讓您設定要以非同步方式執行的頁面。 也涵蓋稍後在此模組中的非同步頁面。

## <a name="asynctimeout"></a>AsyncTimeout

指定非同步頁面的逾時。 預設值為 45 秒。

## <a name="codefile"></a>CodeFile

CodeFile 屬性會取代程式碼後置屬性在 Visual Studio 2002/2003年。

### <a name="codefilebaseclass"></a>CodeFileBaseClass

在您想要從單一基底類別衍生的多個頁面的情況下使用 CodeFileBaseClass 屬性。 由於在 ASP.NET 中，若沒有這個屬性的部分類別的實作會使用共用通用的欄位來參考 ASPX 頁面中宣告控制項的基底類別會無法正確運作，因為 ASP。通道編譯引擎會自動建立新成員 頁面中的控制項為基礎。 因此，如果您想要在 ASP.NET 中的兩個或多個頁面通用基底類別，您必須定義 CodeFileBaseClass 屬性中指定基底類別，然後再衍生自該基底類別的每個頁面的類別。 這個屬性使用時，也需要 CodeFile 屬性。

## <a name="compilationmode"></a>CompilationMode

這個屬性可讓您設定 ASPX 頁面的 CompilationMode 屬性。 CompilationMode 屬性是包含值的列舉**永遠**，**自動**，和**永不**。 預設值是**永遠**。 **自動**設定會防止 ASP.NET 動態盡可能編譯網頁。 排除動態編譯的頁面，可增加效能。 不過，如果已排除的頁面包含必須編譯該程式碼，將會擲回錯誤瀏覽頁面時。

## <a name="enableeventvalidation"></a>EnableEventValidation

這個屬性會指定驗證回傳和回呼事件。 這啟用時，引數回傳或回呼事件會檢查以確保它們來自原本呈現它們的伺服器控制項。

## <a name="enabletheming"></a>EnableTheming

這個屬性會指定在頁面上使用 ASP.NET 佈景主題。 預設值為 **false**。 ASP.NET 主題涵蓋[單元 10](profiles-themes-and-web-parts.md)。

## <a name="linepragmas"></a>LinePragmas

這個屬性會指定是否應在編譯期間新增列 pragma。 行是由偵錯工具用來標記程式碼的特定區段的選項。

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostback

這個屬性會指定 JavaScript 插入頁面以維護回傳之間的捲動位置。 這個屬性是**false**預設。

當這個屬性是**true**，ASP.NET 會加入&lt;指令碼&gt;區塊在回傳時，看起來像這樣：

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample3.html)]

請注意，此指令碼區塊的 src WebResource.axd。 此資源不是實體路徑。 此指令碼要求時，ASP.NET 會動態建立指令碼。

### <a name="masterpagefile"></a>MasterPageFile

這個屬性會指定目前網頁的主版頁面檔案。 路徑可以是相對或絕對。 主版頁面都涵蓋[模組 4](master-pages.md)。

## <a name="stylesheettheme"></a>StyleSheetTheme

這個屬性可讓您覆寫 ASP.NET 2.0 佈景主題所定義的使用者介面外觀屬性。 佈景主題會說明[單元 10](profiles-themes-and-web-parts.md)。

## <a name="theme"></a>主題

指定網頁佈景主題。 如果未指定 StyleSheetTheme 屬性的值，Theme 屬性會覆寫所有樣式套用至頁面上的控制項。

## <a name="title"></a>標題

設定頁面的標題。 此處指定的值將會出現在&lt;標題&gt;所呈現頁面的項目。

### <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

設定 ViewStateEncryptionMode 列舉的值。 可用的值為**永遠**，**自動**，和**永不**。 預設值是**自動**。當這個屬性設定的值為**自動**，加密 viewstate 是控制項要求藉由呼叫**RegisterRequiresViewStateEncryption**方法。

## <a name="setting-public-property-values-via-the--page-directive"></a>設定公用屬性值透過 @ Page 指示詞

@ Page 指示詞，在 ASP.NET 2.0 中的另一個新功能是能夠設定基底類別的公用屬性的初始值。 假設您擁有的公用屬性，例如呼叫**SomeText**在基底類別和 d 與它類似您初始化為**Hello**網頁載入時。 您可以完成這項作業只需要設定 @ Page 指示詞中的值如下所示：

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample4.aspx)]

**SomeText** @ Page 指示詞屬性的基底類別中設定 SomeText 屬性的初始值*Hello ！*。 以下影片是在基底類別使用 @ Page 指示詞中設定的公用屬性的初始值的逐步解說。


![](the-asp-net-2-0-page-model/_static/image1.png)


[開啟全螢幕視訊](the-asp-net-2-0-page-model/_static/setprop1.wmv)


## <a name="new-public-properties-of-the-page-class"></a>頁面類別的新公用屬性

下列公用屬性是在 ASP.NET 2.0 的新功能。

## <a name="apprelativetemplatesourcedirectory"></a>AppRelativeTemplateSourceDirectory

回到網頁或控制項的應用程式的相對路徑。 例如，針對位於頁面http://app/folder/page.aspx，屬性會傳回 ~ / 資料夾 /。

## <a name="apprelativevirtualpath"></a>AppRelativeVirtualPath

回到網頁或控制項的相對虛擬目錄路徑。 例如對於頁面位於http://app/folder/page.aspx，屬性會傳回 ~ / folder/page.aspx。

## <a name="asynctimeout"></a>AsyncTimeout

取得或設定用來非同步頁面處理的逾時。 （本單元稍後將討論非同步頁面）。

## <a name="clientquerystring"></a>ClientQueryString

唯讀屬性，傳回要求的 URL 查詢字串部分。 這個值會是 URL 編碼。 您可以使用 UrlDecode HttpServerUtility 類別方法，以將它解碼。

## <a name="clientscript"></a>ClientScript

這個屬性會傳回可以用來管理的用戶端指令碼的 ASP.NETs 發出 ClientScriptManager 物件。 （ClientScriptManager 類別涵蓋此模組中的更新版本）。

## <a name="enableeventvalidation"></a>EnableEventValidation

這個屬性控制啟用事件驗證回傳和回呼事件。 啟用時，會驗證引數回傳或回呼事件以確保它們來自原本呈現它們的伺服器控制項。

## <a name="enabletheming"></a>EnableTheming

這個屬性會取得或設定布林值，指定 ASP.NET 2.0 佈景主題套用至頁面。

## <a name="form"></a>表單

這個屬性會傳回為 HtmlForm 物件 ASPX 頁面上的 HTML 表單。

## <a name="header"></a>頁首

這個屬性會傳回包含頁首的 HtmlHead 物件的參考。 取得或設定樣式表、 Meta 標記和其他內容，您可以使用傳回的 HtmlHead 物件。

## <a name="idseparator"></a>IdSeparator

這個唯讀屬性會取得用來分隔控制識別項，當 ASP.NET 建置網頁上控制項的唯一識別碼的字元。 但並不是針對直接從程式碼使用而設計。

## <a name="isasync"></a>IsAsync

這個屬性允許非同步頁面。 此模組稍後會討論非同步頁面。

## <a name="iscallback"></a>IsCallback

這個唯讀屬性會傳回**true**頁面是否為回呼的結果。 此模組稍後會討論程式回呼。

## <a name="iscrosspagepostback"></a>IsCrossPagePostBack

這個唯讀屬性會傳回**true**頁面是否跨網頁回傳的一部分。 此模組稍後會說明跨網頁回傳。

## <a name="items"></a>項目

傳回包含頁面內容中所儲存的所有物件的 IDictionary 執行個體的參考。 您可以將項目加入這個 IDictionary 物件，並將它們提供給您的整個內容的存留期。

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostBack

這個屬性會控制 ASP.NET 發出維護頁面捲動瀏覽器中的位置，就會發生回傳之後的 JavaScript。 （稍早在此模組中所討論的這個屬性的詳細資料）。

## <a name="master"></a>主要

這個唯讀屬性傳回已套用主版頁面的頁面的 MasterPage 執行個體的參考。

## <a name="masterpagefile"></a>MasterPageFile

取得或設定頁面的主版頁面檔案名稱。 這個屬性只可以設定 PreInit 方法中。

## <a name="maxpagestatefieldlength"></a>MaxPageStateFieldLength

這個屬性會取得或設定頁面狀態的最大長度 （位元組）。 如果屬性設定為正數，頁面檢視狀態會分成多個隱藏的欄位，讓它不會超過指定的位元組數目。 如果屬性是負數，檢視狀態將不會中斷為區塊。

## <a name="pageadapter"></a>PageAdapter

傳回修改的頁面要求瀏覽器 PageAdapter 物件的參考。

## <a name="previouspage"></a>PreviousPage

在 Server.Transfer 或跨網頁回傳的情況下會傳回前一個頁面的參考。

## <a name="skinid"></a>SkinID

指定要套用到頁面的 ASP.NET 2.0 面板。

## <a name="stylesheettheme"></a>StyleSheetTheme

這個屬性會取得或設定套用至頁面的樣式表。

## <a name="templatecontrol"></a>TemplateControl

傳回包含頁面控制項的參考。

## <a name="theme"></a>主題

取得或設定套用至網頁之 ASP.NET 2.0 佈景主題的名稱。 PreInit 方法之前，必須設定此值。

## <a name="title"></a>標題

這個屬性會取得或設定頁面的標題，取得頁面標頭。

## <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

取得或設定頁面的 ViewStateEncryptionMode。 請參閱稍早在此模組中的這個屬性的詳細的討論。

## <a name="new-protected-properties-of-the-page-class"></a>網頁類別的新受保護的內容

以下是在 ASP.NET 2.0 頁面類別的新受保護的內容。

## <a name="adapter"></a>配接器

傳回參考轉譯的裝置上的頁面 ControlAdapter 要求它。

## <a name="asyncmode"></a>AsyncMode

這個屬性會指出要以非同步方式處理的頁面。 它適用於執行階段，而不是直接在程式碼。

## <a name="clientidseparator"></a>ClientIDSeparator

這個屬性會傳回為控制項建立唯一的用戶端識別碼時，做為分隔符號的字元。 它適用於執行階段，而不是直接在程式碼。

## <a name="pagestatepersister"></a>PageStatePersister

這個屬性會傳回頁面 PageStatePersister 物件。 這個屬性主要是由 ASP.NET 控制項開發人員使用。

## <a name="uniquefilepathsuffix"></a>UniqueFilePathSuffix

這個屬性會傳回唯一的 suffic 附加至瀏覽器快取的檔案路徑。 預設值是\_ \_ufps = 和 6 位數的數字。

## <a name="new-public-methods-for-the-page-class"></a>新的頁面類別的公用方法

下列的公用方法不熟悉 ASP.NET 2.0 中的頁面類別。

## <a name="addonprerendercompleteasync"></a>AddOnPreRenderCompleteAsync

這個方法會註冊事件處理常式委派的非同步頁面執行。 此模組稍後會討論非同步頁面。

## <a name="applystylesheetskin"></a>ApplyStyleSheetSkin

頁面樣式表中的屬性套用至頁面。

## <a name="executeregisteredasynctasks"></a>ExecuteRegisteredAsyncTasks

此方法操作的非同步工作。

### <a name="getvalidators"></a>GetValidators

如果未指定，則傳回驗證程式指定的驗證群組或預設的驗證群組的集合。

## <a name="registerasynctask"></a>RegisterAsyncTask

這個方法會註冊新的非同步工作。 此模組稍後會說明非同步頁面。

## <a name="registerrequirescontrolstate"></a>RegisterRequiresControlState

這個方法會告知 ASP.NET 網頁控制項狀態必須 persisted。

## <a name="registerrequiresviewstateencryption"></a>RegisterRequiresViewStateEncryption

這個方法會告知 ASP.NET 網頁 viewstate 需要加密。

## <a name="resolveclienturl"></a>ResolveClientUrl

傳回可用於等映像的用戶端要求的相對 URL。

## <a name="setfocus"></a>SetFocus

這個方法會將焦點設定一開始載入的頁面時指定的控制項。

## <a name="unregisterrequirescontrolstate"></a>UnregisterRequiresControlState

這個方法會將取消登錄不再需要控制狀態持續性傳遞給它的控制項。

## <a name="changes-to-the-page-lifecycle"></a>變更頁面生命週期

網頁生命週期，在 ASP.NET 2.0 中未變更急遽增加，但您應該注意的一些新方法。 ASP.NET 2.0 頁面生命週期如下所述。

## <a name="preinit-new-in-aspnet-20"></a>PreInit （新的 ASP.NET 2.0)

PreInit 事件是開發人員可以存取的生命週期中的最早階段。 此事件加入可讓您能夠以程式設計方式變更主版頁面、 ASP.NET 2.0 佈景主題，請存取 ASP.NET 2.0 設定檔等屬性。如果您是在回傳狀態，其一定要了解，Viewstate 具有尚未套用至這個時候生命週期中的控制項。 因此，如果開發人員變更控制項的屬性在這個階段，它將可能覆寫網頁生命週期的更新版本中。

## <a name="init"></a>Init

在 Init 事件未變更 ASP.NET 1.x。 這是您想要讀取或初始化您的頁面上的控制項屬性。 在此階段、 主版頁面、 佈景主題等已套用至頁面。

## <a name="initcomplete-new-in-20"></a>InitComplete （新功能 2.0)

在頁面初始化階段結尾稱為 InitComplete 事件。 此時生命週期中，您可以存取控制項在頁面上，但尚未擴展其狀態。

## <a name="preload-new-in-20"></a>預先載入 （2.0 的新功能）

此事件會呼叫在套用所有的回傳資料之後，和之前頁面\_負載。

## <a name="load"></a>Load

載入事件未變更 ASP.NET 1.x。

## <a name="loadcomplete-new-in-20"></a>LoadComplete （新功能 2.0)

LoadComplete 事件中的頁面載入階段是最後一個事件。 在這個階段，所有回傳和 viewstate 資料已都套用至頁面。

## <a name="prerender"></a>PreRender

如果您想要的正確維護的控制項，以動態方式加入至頁面的 viewstate，PreRender 事件就會是將它們加入的最後機會。

## <a name="prerendercomplete-new-in-20"></a>PreRenderComplete （新功能 2.0)

在 PreRenderComplete 階段，所有控制項都已都加入至頁面，頁面已準備好呈現。 PreRenderComplete 事件是在儲存頁面 viewstate 之前引發的最後一個事件。

## <a name="savestatecomplete-new-in-20"></a>SaveStateComplete （新功能 2.0)

已儲存的所有頁面的 viewstate 和控制狀態之後，立即呼叫 SaveStateComplete 事件。 這是前頁面實際上會呈現在瀏覽器的最後一個事件。

## <a name="render"></a>轉譯

ASP.NET 之後並未變更轉譯方法 1.x。 這是初始化 HtmlTextWriter 和瀏覽器中呈現的網頁位置。

## <a name="cross-page-postback-in-aspnet-20"></a>在 ASP.NET 2.0 中的跨網頁回傳

在 ASP.NET 中 1.x 回傳都需要公佈到相同的頁面。 不允許跨網頁回傳。 ASP.NET 2.0 加入回傳至不同的頁面，透過 IButtonControl 介面的功能。 實作新的 IButtonControl 介面 （按鈕、 LinkButton 和 ImageButton 除了協力廠商的自訂控制項） 的任何控制項可以利用這個新的功能，透過使用 PostBackUrl 屬性。 下列程式碼會示範回傳至第二個頁面上的按鈕控制項。

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample5.aspx)]

網頁回傳，啟始回傳的頁面時，可透過第二頁上的 [上一頁] 屬性存取。 這項功能透過新 WebForm 實作\_DoPostBackWithOptions 用戶端函式，會呈現 ASP.NET 2.0 頁面，但控制項回傳至不同的頁面。 這個 JavaScript 函式會提供新的 WebResource.axd 處理常式，這時用戶端指令碼。

以下影片是跨網頁回傳的逐步解說。


![](the-asp-net-2-0-page-model/_static/image2.png)


[開啟全螢幕視訊](the-asp-net-2-0-page-model/_static/xpage1.wmv)


## <a name="more-details-on-cross-page-postbacks"></a>在跨網頁回傳的更多詳細資料

### <a name="viewstate"></a>Viewstate

您可能會要求您自己已經有關會發生什麼事 viewstate 從跨網頁回傳的案例中的第一頁。 所有之後，任何未實作 IPostBackDataHandler 的控制項將會因此保存 viewstate，透過其狀態，具有存取權的跨網頁回傳的第二頁上的控制項屬性，您必須擁有存取頁面的 viewstate。 ASP.NET 2.0 會處理此案例中使用新的隱藏的欄位，在第二個頁面呼叫\_\_上一頁。 \_\_上一頁表單欄位會包含第一個頁面的 viewstate，讓您在第二個頁面都可以存取所有控制項的屬性。

### <a name="circumventing-findcontrol"></a>規避 FindControl

在跨網頁回傳的視訊逐步解說中，我可以使用 FindControl 方法來取得第一頁上的文字方塊控制項的參考。 該方法適用於該目的，但 FindControl 成本很高，而且它需要撰寫額外的程式碼。 幸運的是，ASP.NET 2.0 提供此用途，在許多案例中運作 FindControl 的替代方案。 PreviousPageType 指示詞可讓您有使用 TypeName 或 VirtualPath 屬性前一頁的強型別參考。 TypeName 屬性可讓您指定先前的頁面類型而 VirtualPath 屬性可讓您參考至上一頁使用的虛擬路徑。 設定 PreviousPageType 指示詞之後，您必須再公開控制項，您要允許存取使用的公用屬性的等等。

## <a name="lab-1-cross-page-postback"></a>實驗室 1 跨網頁回傳

在此實驗室中，您將建立使用 ASP.NET 2.0 的新的跨網頁回傳功能的應用程式。

1. 開啟 Visual Studio 2005，並建立新的 ASP.NET 網站。
2. 加入新的 Webform 呼叫 page2.aspx。
3. 在 [設計] 檢視中開啟 Default.aspx，然後加入按鈕控制項和文字方塊控制項。 

    1. 按鈕控制項提供給識別碼為**交付按鈕**和文字方塊控制項的 ID **UserName**。
    2. 設 page2.aspx PostBackUrl 按鈕的屬性。
4. 開啟 page2.aspx 來源檢視中。
5. 加入 @ PreviousPageType 指示詞，如下所示：
6. 下列程式碼加入至頁面\_負載 page2.aspx 的程式碼後置： 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample6.cs)]
7. 按一下 [建置] 功能表建置建置專案。
8. 將下列程式碼加入至程式碼後置 Default.aspx: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample7.cs)]
9. 變更頁面\_負載下 page2.aspx 中： 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample8.cs)]
10. 建置專案。
11. 執行專案。
12. 在文字方塊中輸入您的名稱，然後按一下按鈕。
13. 什麼是結果？

## <a name="asynchronous-pages-in-aspnet-20"></a>在 ASP.NET 2.0 中的非同步頁面

在 ASP.NET 中的許多競爭問題 （例如 Web 服務或資料庫呼叫） 的外部呼叫、 檔案 IO 延遲等等的延遲所造成。對 ASP.NET 應用程式進行要求時，ASP.NET 會使用其中一個背景工作執行緒服務該要求。 該要求擁有該執行緒，直到完成要求且在傳送回應。 ASP.NET 2.0 會設法加入以非同步方式執行 「 網頁 」 功能來解決這類問題的延遲問題。 這表示工作者執行緒可以啟動要求，並再交給其他執行的其他的執行緒，進而快速地返回可用的執行緒集區。 檔案 IO、 資料庫呼叫等完成之後，新的執行緒被取自完成要求的執行緒集區中。

讓頁面以非同步方式執行的第一個步驟是設定**非同步**頁面指示詞屬性如下所示：

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample9.aspx)]

這個屬性會告知 ASP.NET 來實作頁面 IHttpAsyncHandler。

下一個步驟是呼叫 AddOnPreRenderCompleteAsync 方法在 PreRender 之前頁面生命週期中的點。 (在頁面中通常會呼叫這個方法\_負載。)AddOnPreRenderCompleteAsync 方法接受兩個參數。BeginEventHandler 和 EndEventHandler。 BeginEventHandler 傳回 IAsyncResult 再傳遞給 EndEventHandler 做為參數。

下列視訊是非同步頁面要求的逐步解說。


![](the-asp-net-2-0-page-model/_static/image3.png)


[開啟全螢幕視訊](the-asp-net-2-0-page-model/_static/async1.wmv)


> [!NOTE]
> 非同步頁面不會轉譯至瀏覽器 EndEventHandler 完成為止。 任何不確定，但有些開發人員會將視為類似非同步回呼的非同步要求。 請務必了解它們不是。 非同步要求的好處是可傳回第一個背景工作執行緒的執行緒集區，才能服務新要求，藉此減少由於繫結的 IO 的爭用等等。


## <a name="script-callbacks-in-aspnet-20"></a>ASP.NET 2.0 中的指令碼回呼

Web 開發人員一律尋找的方法可防止閃爍的關聯回呼。 在 ASP.NET 中 1.x SmartNavigation 避免閃爍，最常見的方法卻 SmartNavigation 某些開發人員造成問題，因為其在用戶端實作的複雜性。 ASP.NET 2.0 解決這個問題的指令碼的回呼。 回呼指令碼會利用 XMLHttp 針對透過 JavaScript 的 Web 伺服器提出要求。 XMLHttp 要求會傳回 XML 資料，然後可操作透過瀏覽器的 dom。 XMLHttp 程式碼會對使用者隱藏的新 WebResource.axd 處理常式。

有幾個步驟所需，若要設定 ASP.NET 2.0 中的指令碼回呼。

## <a name="step-1--implement-the-icallbackeventhandler-interface"></a>步驟 1： 實作 ICallbackEventHandler 介面

為了讓 ASP.NET 才能辨識您的網頁做為參與指令碼回呼，您必須實作 ICallbackEventHandler 介面。 您可以在程式碼後置檔案進行就像這樣：

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample10.cs)]

您同樣可以這樣因此使用 @ 實作指示詞類似：

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample11.aspx)]

使用內嵌的 ASP.NET 程式碼時，您通常會使用 @ 實作指示詞。

## <a name="step-2--call-getcallbackeventreference"></a>步驟 2： 呼叫 GetCallbackEventReference

如先前所述，XMLHttp 呼叫會封裝在 WebResource.axd 處理常式。 ASP.NET 頁面轉譯時，會將呼叫加入 WebForm\_DoCallback，係由 WebResource.axd 用戶端指令碼。 WebForm\_DoCallback 函式取代\_ \_doPostBack 函式的回呼。 請記住， \_ \_doPostBack 以程式設計的方式送出頁面上的表單。 在回呼案例中，您想要防止回傳，所以\_ \_doPostBack 會不敷使用。

> [!NOTE]
> \_\_doPostBack 仍然會轉譯到用戶端指令碼回呼案例中的頁面。 不過，它不用於回呼。


引數 WebForm\_DoCallback 用戶端函式可以透過伺服器端函式通常會在頁面中稱為 GetCallbackEventReference\_負載。 一般 GetCallbackEventReference 呼叫可能會看起來像這樣：

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample12.cs)]

> [!NOTE]
> 在此情況下，cm 是 ClientScriptManager 的執行個體。 稍後在此模組將涵蓋 ClientScriptManager 類別。


有數個多載的 GetCallbackEventReference 版本。 在此情況下，引數是的如下所示：

`this`

呼叫 GetCallbackEventReference 控制項的參考。 在此情況下，它是網頁本身。

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample13.js)]

從用戶端程式碼時傳遞給伺服器端事件字串引數。 在此情況下，傳遞的值的下拉式清單中的 Im 呼叫 ddlCompany。

`ShowCompanyName`

用戶端函式接受的傳回值 （如字串） 從伺服器端的回呼事件的名稱。 伺服器端回呼成功時，只會呼叫此函式。 因此，為了提高健全度，通常建議使用 GetCallbackEventReference 其他字串引數指定的名稱發生錯誤時所執行的用戶端函式多載的版本。

`null`

字串，表示系統已起始伺服器在回呼之前的用戶端函式。 在此情況下，沒有任何這類指令碼，因此引數為 null。

`true`

布林值，指定要以非同步方式進行回呼。

呼叫 WebForm\_DoCallback 用戶端上的會將這些引數傳遞。 因此，當用戶端上呈現此頁面時，該程式碼看起來就像這樣：

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample14.js)]

請注意，用戶端上的函式的簽章有些許不同。 用戶端函式會在 5 個字串、 布林值傳遞。 （這是 null，在上述範例中） 的其他字串包含用戶端函式會處理任何錯誤，伺服器端的回呼。

## <a name="step-3--hook-the-client-side-control-event"></a>步驟 3： 攔截 (Hook) 用戶端控制項事件

請注意，上述 GetCallbackEventReference 的傳回值已指派給字串變數。 該字串用來連結控制項初始化回呼用戶端事件。 在此範例中，回呼由起始下拉式清單中的頁面上，所以我想要攔截 (hook) *OnChange*事件。

若要攔截 (hook) 用戶端事件，只要加入處理常式至用戶端標記，如下所示：

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample15.cs)]

請記得， *cbRef*為 GetCallbackEventReference，呼叫的傳回值。 包含呼叫 WebForm\_已如上所示的 DoCallback。

## <a name="step-4--register-the-client-side-script"></a>步驟 4： 註冊用戶端指令碼

提醒您，GetCallbackEventReference 呼叫指定的用戶端指令碼呼叫**ShowCompanyName**伺服器端回呼成功時，就會執行。 該指令碼必須加入至頁面使用 ClientScriptManager 執行個體。 （ClientScriptManager 類別將稍後在本單元這。）因此，您執行該類似：

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample16.js)]

## <a name="step-5--call-the-methods-of-the-icallbackeventhandler-interface"></a>步驟 5： 呼叫 ICallbackEventHandler 介面的方法

ICallbackEventHandler 包含兩個方法，您需要在程式碼中實作。 它們是**RaiseCallbackEvent**和**GetCallbackEvent**。

**RaiseCallbackEvent**接受字串做為引數，並傳回任何項目。 字串引數會傳遞 WebForm，用戶端呼叫\_DoCallback。 在此情況下，該值是*值*呼叫 ddlCompany 下拉式清單中的屬性。 您的伺服器端程式碼應置於 RaiseCallbackEvent 方法。 例如，如果您的回呼正在進行 WebRequest 對外部資源時，該程式碼應放在 RaiseCallbackEvent。

**GetCallbackEvent**負責處理回傳給用戶端的回呼。 它會採用任何引數，並傳回字串。 它會傳回的字串將傳遞做為引數至用戶端函式，在此情況下*ShowCompanyName*。

完成上述步驟之後，即準備好在 ASP.NET 2.0 中執行的指令碼的回呼。


![](the-asp-net-2-0-page-model/_static/image4.png)


[開啟全螢幕視訊](the-asp-net-2-0-page-model/_static/callback1.wmv)


支援在 ASP.NET 中的指令碼回呼支援讓 XMLHttp 呼叫任何瀏覽器中。 包含所有現代化瀏覽器中使用今天。 其他 （包含即將發行的 IE 7） 的新式瀏覽器使用的內建 XMLHttp 物件時，Internet Explorer 使用 XMLHttp ActiveX 物件。 若要以程式設計方式判斷是否瀏覽器支援回呼，您可以使用**Request.Browser.SupportCallback**屬性。 這個屬性會傳回**true**如果要求的用戶端支援回呼指令碼。

## <a name="working-with-client-script-in-aspnet-20"></a>使用 ASP.NET 2.0 中的用戶端指令碼

透過使用 ClientScriptManager 類別管理 ASP.NET 2.0 中的用戶端指令碼。 ClientScriptManager 類別會追蹤的類型和名稱使用的用戶端指令碼。 這可防止相同的指令碼要以程式設計方式插入頁面上一次以上。

> [!NOTE]
> 指令碼成功註冊頁面上之後，任何後續嘗試註冊相同的指令碼會直接導致未註冊第二次指令碼。 加入任何重複的指令碼，並沒有發生例外狀況。 若要避免不必要的計算，有可用來判斷 使未嘗試註冊一次以上，是否已註冊的指令碼的方法。


ClientScriptManager 方法應該熟悉所有目前的 ASP.NET 開發人員：

## <a name="registerclientscriptblock"></a>RegisterClientScriptBlock

這個方法所呈現頁面的頂端加入指令碼。 這是用於將加入將用戶端明確呼叫的函式。

有兩個多載的版本，這個方法。 四個引數中的三種通之間。 包括：

`type (string)`

***類型***引數會識別指令碼的類型。 通常會建議来使用的頁面類型 （此項。GetType()) 類型。

`key (string)`

***金鑰***引數是使用者定義索引鍵的指令碼。 這應該是唯一的每個指令碼。 如果您嘗試加入具有相同索引鍵和類型已經加入的指令碼的指令碼，它就不會加入。

`script (string)`

***指令碼***引數是字串，包含要加入的實際指令碼。 建議您在建立指令碼，然後在指派的 StringBuilder 上使用 tostring （） 方法使用 StringBuilder***指令碼***引數。

如果您使用多載的 RegisterClientScriptBlock 只採用三個引數時，您必須包含指令碼項目 (&lt;指令碼&gt;和&lt;/指令碼&gt;) 指令碼中。

您可以選擇使用 RegisterClientScriptBlock 接受第四個引數的多載。 第四個引數是布林值，指定 ASP.NET 應該為您加入指令碼項目。 如果這個引數是**true**，您的指令碼不應包含的指令碼項目明確。

您可以使用 IsClientScriptBlockRegistered 方法來判斷是否已登錄指令碼。 這可讓您避免嘗試重新註冊已註冊的指令碼。

### <a name="registerclientscriptinclude-new-in-20"></a>RegisterClientScriptInclude （新功能 2.0)

RegisterClientScriptInclude 標記建立指令碼區塊連結至外部指令碼檔案。 它有兩個多載。 一個採用索引鍵和 URL。 第二個新增的指定類型的第三個引數。

例如，下列程式碼會產生指令碼區塊連結至 jsfunctions.js 根目錄中的應用程式的指令碼資料夾：

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample17.cs)]

此程式碼會產生下列程式碼，在呈現的頁面：

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample18.html)]

> [!NOTE]
> 在頁面底部轉譯的指令碼區塊。


您可以使用 IsClientScriptIncludeRegistered 方法來判斷是否已登錄指令碼。 這可讓您避免嘗試重新註冊指令碼。

## <a name="registerstartupscript"></a>RegisterStartupScript

RegisterStartupScript 方法會採用 RegisterClientScriptBlock 方法相同的引數。 在頁面載入之後，但在這段用戶端之前，向 RegisterStartupScript 指令碼會執行。 在 1.X RegisterStartupScript 與註冊的指令碼已放置的結束&lt;/形成&gt;標記時向 RegisterClientScriptBlock 指令碼放置在開啟之後立即&lt;表單&gt;標記。 在 ASP.NET 2.0 中，同時位於關閉之前立即&lt;/形成&gt;標記。

> [!NOTE]
> 如果您使用 RegisterStartupScript 註冊函式，直到您明確呼叫它在用戶端程式碼中，將不會執行該函式。


若要判斷是否已登錄指令碼，並避免重新註冊的指令碼嘗試使用 IsStartupScriptRegistered 方法。

## <a name="other-clientscriptmanager-methods"></a>其他 ClientScriptManager 方法

以下是一些 ClientScriptManager 類別的其他有用的方法。


|  <strong>GetCallbackEventReference</strong>   |                                                 請參閱稍早在此模組中的指令碼回呼。                                                 |
|-----------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|  <strong>GetPostBackClientHyperlink</strong>  |                取得 JavaScript 參考 (javascript:&lt;呼叫&gt;)，可以用來從用戶端事件公佈。                 |
|  <strong>GetPostBackEventReference</strong>   |                                   取得可用來從用戶端的 post 的字串。                                    |
|      <strong>GetWebResourceUrl</strong>       | 內嵌組件中的資源傳回的 URL。 必須用於搭配<strong>RegisterClientScriptResource</strong>。 |
| <strong>RegisterClientScriptResource</strong> |     註冊 Web 資源的網頁。 這些是內嵌在組件，而且新 WebResource.axd 處理常式所處理的資源。      |
|     <strong>RegisterHiddenField</strong>      |                                                 與網頁註冊隱藏的欄位。                                                 |
|  <strong>RegisterOnSubmitStatement</strong>   |                                  註冊用戶端 HTML 表單送出時執行的程式碼。                                   |

