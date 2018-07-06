---
uid: web-forms/overview/moving-to-aspnet-20/server-controls
title: 伺服器控制項 |Microsoft Docs
author: microsoft
description: ASP.NET 2.0 增強伺服器控制項，在許多方面。 在本單元中，我們將討論一些方式 ASP.NET 2.0 和 Visual Studio 200 的架構變更...
ms.author: aspnetcontent
ms.date: 02/20/2005
ms.assetid: 43f6ac47-76fc-4cf7-8e9f-c18ce673dfd8
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/server-controls
msc.type: authoredcontent
ms.openlocfilehash: da06429f3949a47a02fccef45666d1220781e473
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837065"
---
<a name="server-controls"></a>伺服器控制項
====================
by [Microsoft](https://github.com/microsoft)

> ASP.NET 2.0 增強伺服器控制項，在許多方面。 在這個模組中，我們將討論一些架構的變更，ASP.NET 2.0 的方式，而 Visual Studio 2005 處理的伺服器控制項。


ASP.NET 2.0 增強伺服器控制項，在許多方面。 在這個模組中，我們將討論一些架構的變更，ASP.NET 2.0 的方式，而 Visual Studio 2005 處理的伺服器控制項。

## <a name="view-state"></a>檢視狀態

在檢視狀態中的 ASP.NET 2.0 的主要變更是大小可以大幅減少。 請考慮只在其上的行事曆控制項的頁面。 以下是在 ASP.NET 1.1 中的檢視狀態。

[!code-css[Main](server-controls/samples/sample1.css)]

現在如下的檢視狀態上完全相同的頁面在 ASP.NET 2.0。

[!code-css[Main](server-controls/samples/sample2.css)]

這是重大的變更，並考慮檢視狀態來回傳送透過網路，這項變更可以讓開發人員大幅提升效能。 檢視狀態大小縮減主要是因為我們在內部處理的方式。 請記住，檢視狀態是以 Base64 編碼的字串。 若要進一步了解在檢視狀態中的 ASP.NET 2.0 的變更，讓我們看看從上述範例中的已解碼的值。

以下是已解碼的 1.1 版的檢視狀態：

[!code-css[Main](server-controls/samples/sample3.css)]

這可能會看起來有點像無意義，但沒有在這裡的模式。 在 ASP.NET 1.x 中，我們用來識別資料類型的單一字元，並分隔使用值&lt;&gt;字元。 在檢視狀態上述範例中的"t"代表 Triplet。 三物件組包含一組 ArrayLists （"l"代表 ArrayList）。其中一個這些 ArrayLists 包含 Int32 ("i") 值是 1，而另一個包含另一個 Triplet。 三物件組包含一組 ArrayLists 等等。要記住的重點是我們會使用包含配對的 Triplet、 我們識別的資料型別是字母，透過和我們會使用&lt;和&gt;做為分隔符號的字元。

在 ASP.NET 2.0 中，已解碼的檢視狀態看起來會有點不同。

[!code-powershell[Main](server-controls/samples/sample4.ps1)]

您應該會注意到顯著的不同，已解碼的檢視狀態的外觀。 這項變更會有數個結構性的支援。 檢視狀態在 ASP.NET 1.x 用於 LosFormatter 序列化資料。 在 2.0 中，我們會使用新的 ObjectStateFormatter 類別。 這個類別被專為協助序列化和還原序列化檢視狀態和控制項狀態。 （下一節將說明控制項狀態）。有許多優點，獲得變更的序列化和還原序列化發生的方法。 最明顯的其中一項是不使用 TextWriter LosFormatter，像 ObjectStateFormatter 使用 BinaryWriter 的事實。 這可讓 ASP.NET 2.0，來儲存檢視狀態一系列的位元組，而不是字串。 例如，需要為整數。 在 ASP.NET 1.1 中，整數所需的檢視狀態的 4 個位元組。 在 ASP.NET 2.0 中，該相同的整數，只需要 1 個位元組。 其他增強功能對減少儲存的檢視狀態。 例如，日期時間值現在儲存使用 TickCount 而非字串。

如果一切都不足夠，特別注意已付費之事實的 1.x 中的檢視狀態的最大取用者的 DataGrid 和類似的控制項。 控制項，例如 DataGrid 而言檢視狀態的主要缺點是資訊的，它通常會包含大量重複。 在 ASP.NET 1.x 中，重複資訊直接儲存和上一次而造成過大的檢視狀態。 在 ASP.NET 2.0 中，我們會使用新 IndexedString 類別來儲存這類資料。 若重複出現的字串，我們只會儲存權杖 IndexedString 和執行 IndexedString 物件的表格中的索引。

## <a name="control-state"></a>控制項狀態

其中一個主要開發人員必須與檢視狀態的牢騷就是它新增至 HTTP 承載的大小。 如先前所述，檢視狀態的最大取用者會在 DataGrid 控制項。 若要避免大量資料格所產生的檢視狀態，許多開發人員只要停用該控制項的檢視狀態。 不幸的是，該方案不一定是一個很好。 檢視狀態在 ASP.NET 1.x 不只包含資料之控制項的正確功能所需。 它也包含有關控制項的 UI 狀態的資訊。 這表示，如果您想要允許分頁上的資料格，您必須先啟用檢視狀態，即使您不需要的所有檢視的 UI 資訊狀態就會包含。 這是孤注一擲的案例。

在 ASP.NET 2.0 中，控制項狀態會解決該問題，妥善透過控制項狀態的簡介。 控制項狀態會包含控制項的適當功能絕對必要的資料。 不同的檢視狀態，無法停用控制項狀態。 因此，務必小心控制 控制項狀態中所儲存的資料。

> [!NOTE]
> 中的檢視狀態以及保存控制項狀態\_ \_VIEWSTATE 的隱藏的表單欄位。


這段影片會逐步解說中的檢視狀態和控制項狀態。


![](server-controls/_static/image1.png)


[開啟全螢幕影片](server-controls/_static/state1.wmv)


為了讓伺服器控制項，以讀取和寫入至控制狀態，您必須採取三個步驟。

## <a name="step-1-call-the-registerrequirescontrolstate-method"></a>步驟 1： 呼叫 RegisterRequiresControlState 方法

RegisterRequiresControlState 方法會通知 ASP.NET 控制項必須保存控制項狀態。 它會採用一個引數的型別即所註冊之控制項的控制項。

請務必注意註冊不會保存在要求。 因此，這個方法必須呼叫每個要求的控制項是否保存控制項狀態。 建議在 OnInit 會呼叫此方法。

[!code-csharp[Main](server-controls/samples/sample5.cs)]

## <a name="step-2-override-savecontrolstate"></a>步驟 2： 覆寫 SaveControlState

SaveControlState 方法儲存自上次的回傳控制項的控制項狀態變更。 它會傳回物件，代表控制項的狀態。

## <a name="step-3-override-loadcontrolstate"></a>步驟 3： 覆寫 LoadControlState

LoadControlState 方法會載入控制項中的已儲存的狀態。 此方法會採用一個引數的型別物件，其中包含控制項的已儲存的狀態。

## <a name="full-xhtml-compliance"></a>完全符合 XHTML 的規範

任何 Web 開發人員知道 Web 應用程式中的標準的重要性。 若要維護標準為基礎的開發環境，ASP.NET 2.0 是完全符合 XHTML。 因此，所有標記都都呈現根據符合 XHTML 標準，在瀏覽器支援 HTML 4.0 或更新版本。

在 ASP.NET 1.1 中的文件類型定義如下所示：

[!code-html[Main](server-controls/samples/sample6.html)]

在 ASP.NET 2.0 中，預設文件類型定義如下所示：

[!code-html[Main](server-controls/samples/sample7.html)]

如果您選擇，您可以改變預設 XHML 合規性，透過組態檔中的 [xhtmlConformance] 節點。 比方說，在 web.config 檔案中的下列節點會將符合 XHTML 的規範變更為 XHTML 1.0 Strict:

[!code-xml[Main](server-controls/samples/sample8.xml)]

如果您選擇，您也可以設定要使用舊版 ASP.NET 中使用的設定的 ASP.NET 1.x，如下所示：

[!code-xml[Main](server-controls/samples/sample9.xml)]

## <a name="adaptive-rendering-using-adapters"></a>自適性轉譯時使用配接器

在 ASP.NET 1.x 中，所包含的組態檔案&lt;browserCaps&gt;填入 HttpBrowserCapabilities 物件的區段。 這個物件允許開發人員判斷哪些裝置提出特定要求，並適當地轉譯程式碼。 在 ASP.NET 2.0 中，模型已經改良，現在會使用新的 ControlAdapter 類別 ControlAdapter 類別會覆寫控制項的生命週期中的事件，並控制使用者代理程式的功能為基礎的控制項的呈現。 儲存在 c:\windows\microsoft.net\framework\v2.0 瀏覽器定義檔 （.browser 檔案副檔名的檔案） 會定義特定的使用者代理程式的功能。\* \* \* \*\CONFIG\Browsers 資料夾。

> [!NOTE]
> ControlAdapter 類別是抽象類別。


就像是&lt;browserCaps&gt; 1.x 中，瀏覽器定義檔中的一節會使用規則運算式剖析使用者代理字串來識別要求的瀏覽器。 它它們定義該使用者代理程式的特定功能。 ControlAdapter 呈現 Render 方法透過控制項。 因此，如果您覆寫 Render 方法，您不應該呼叫轉譯的基底類別。 如此一來，可能會造成轉譯發生兩次，一次的配接器，一次是控制項本身。

## <a name="developing-a-custom-adapter"></a>開發自訂配接器

您可以藉由繼承自 ControlAdapter 開發您自己自訂的配接器。 此外，您可以繼承自抽象類別 PageAdapter 萬一頁面需要配接器的位置。 透過即可完成對應的自訂配接器的控制項&lt;的 controlAdapters&gt;瀏覽器定義檔案中的項目。 例如，下列 XML 程式碼從瀏覽器定義檔案對應功能表控制項 MenuAdapter 類別：

[!code-html[Main](server-controls/samples/sample10.html)]

使用此模型時，會變得很容易就可以針對特定裝置或瀏覽器控制項開發人員。 它也是開發人員能夠完全控制每個裝置上呈現頁面的方式相當簡單。

## <a name="per-device-rendering"></a>每一裝置轉譯

在 ASP.NET 2.0 中的伺服器控制項屬性可以指定每個裝置使用的瀏覽器特定前置詞。 例如，下列程式碼會變更標籤，視哪一種裝置用來瀏覽網頁的文字。

[!code-aspx[Main](server-controls/samples/sample11.aspx)]

當從 Internet Explorer，瀏覽包含此標籤的頁面時，標籤會顯示文字指出 「 您正在瀏覽從 Internet Explorer。 」 當從 Firefox 瀏覽網頁時，標籤會顯示文字 「 瀏覽從 Firefox。 」 當從任何其他裝置，瀏覽網頁時，它會顯示 「 您正在瀏覽從未知裝置。 」 可以使用這個特殊的語法來指定任何屬性。

## <a name="setting-focus"></a>設定焦點

有關如何在特定的控制項上設定初始焦點的 ASP.NET 1.x 開發人員常見問題集。 比方說，在登入頁面上，最好將 [使用者識別碼] 文字方塊中第一次載入頁面時，取得焦點。 在 ASP.NET 1.x 中，執行此動作會需要撰寫一些用戶端指令碼。 即使這類指令碼是簡單的工作，就不會再感謝 SetFocus 方法必須在 ASP.NET 2.0。 SetFocus 方法會採用一個引數指出應該接收焦點的控制項。 這個引數可以是用戶端控制項的 ID 做為字串或伺服器控制項，以控制物件的名稱。 比方說，若要設定初始焦點至第一次載入頁面時，呼叫 txtUserID 文字方塊控制項，將下列程式碼新增至頁面\_負載：

[!code-csharp[Main](server-controls/samples/sample12.cs)]

-或

[!code-csharp[Main](server-controls/samples/sample13.cs)]

ASP.NET 2.0 使用 Webresource.axd 處理常式 （先前所述），來呈現設定焦點的用戶端功能。 用戶端函式的名稱是 WebForm\_AutoFocus 如下所示：

[!code-html[Main](server-controls/samples/sample14.html)]

或者，您可以使用控制項的 Focus 方法，若要設定初始焦點的控制項。 焦點方法衍生自 Control 類別，並可用於所有的 ASP.NET 2.0 控制項。 它也可將焦點設定至特定控制項發生驗證錯誤時。 將會在更新版本的模組說明。

## <a name="new-server-controls-in-aspnet-20"></a>ASP.NET 2.0 中的新伺服器控制項

以下是在 ASP.NET 2.0 的新伺服器控制項。 我們將會進入更多詳細資料，在其中更新版本的模組中一些。

## <a name="imagemap-control"></a>ImageMap 控制項

ImageMap 控制項可讓您將加入的映像，可初始化回傳，或瀏覽至 URL 中的熱點。 有三種類型的作用點;CircleHotSpot、 RectangleHotSpot 和 PolygonHotSpot。 透過 Visual Studio 中，或以程式設計方式在程式碼中的集合編輯器新增作用點。 不沒有可用於繪製的映像上的作用點的任何使用者介面。 必須以宣告方式指定的座標和大小或作用點的半徑。 另外還有一個作用區設計工具中沒有視覺表示法。 如果作用點設定為巡覽至 URL，透過 NavigateUrl 屬性的作用被指定的 URL。 如果是 post 回熱點，PostBackValue 屬性可讓您在伺服器端程式碼中傳遞回文章中可擷取的字串。


![在 Visual Studio 中的作用點集合編輯器](server-controls/_static/image1.jpg)

**圖 1**： 在 Visual Studio 中的作用點集合編輯器


## <a name="bulletedlist-control"></a>BulletedList 控制項

BulletedList 控制項是可以輕鬆地進行資料繫結項目符號清單。 （編號的） 排序清單或未按順序透過 BulletStyle 屬性。 在清單中的每個項目被以清單項目物件。


![Visual Studio 中的 BulletedList 控制項](server-controls/_static/image1.gif)

**圖 2**: BulletedList Visual Studio 中的控制項


## <a name="hiddenfield-control"></a>Hiddenfield

HiddenField 控制隱藏的表單將欄位加入至您的頁面上，其值可用於伺服器端程式碼。 隱藏的表單欄位的值通常預期回傳之間保持不變。 但是，它可能是惡意使用者變更值之前，要回傳。 如果發生這種情況，HiddenField 控制項就會引發 ValueChanged 事件。 如果您有 HiddenField 控制項中的機密資訊，您想要確保它會維持不變，您應該處理 ValueChanged 事件，在程式碼中。

## <a name="fileupload-control"></a>FileUpload 控制項

在 ASP.NET 2.0 FileUpload 控制項可讓您能夠將檔案上傳至 Web 伺服器透過 ASP.NET 網頁。 此控制項是相當類似於 ASP.NET 1.x HtmlInputFile 類別有一些例外狀況。 在 ASP.NET 1.x 中，會建議 null 檢查 PostedFile 屬性，以判斷您是否有正確的檔案。 FileUpload 控制項在 ASP.NET 2.0 中的，將新的 HasFile 屬性可用於相同的目的，並會更有效率。

PostedFile 屬性仍可供存取 HttpPostedFile 物件，但某些 HttpPostedFile 功能現已本質上與 FileUpload 控制項。 例如，若要將上傳的檔案儲存在 ASP.NET 1.x 中，您另存新檔上呼叫方法 HttpPostedFile 物件。 使用 FileUpload 控制項在 ASP.NET 2.0 中，您另存新檔上呼叫方法一樣 FileUpload 控制項本身。

2.0 的行為 （與可能是最重要的變更） 中的另一個重大變更是，它不再需要載入記憶體中的整個上傳的檔案，然後再將它儲存。 在 1.x 中，已上傳任何檔案會儲存完全讀入記憶體之前寫入至磁碟。 此架構可避免大型檔案上的傳。

在 ASP.NET 2.0 中，httpRuntime 元素 requestLengthDiskThreshold 屬性可讓您設定多少 Kb 會保留在記憶體中之前寫入緩衝區中到磁碟。

**重要**: MSDN 文件 （及其他位置的文件） 指定這個值是以位元組為單位 （而不是千位元組），且預設值是 256。 值實際上以 kb 為單位指定，預設值為 80。 擁有預設值是 80 K，我們會確保大型物件堆積不未抵達緩衝區。

## <a name="wizard-control"></a>精靈控制項

它會致力於嘗試收集一系列中的資訊的 「 頁面 」 使用面板，或藉由傳送 頁面的 ASP.NET 開發人員相當常見。 多半的努力是令人沮喪，相當耗時。 新的精靈控制項讓使用者熟悉的精靈介面中的線性和非線性步驟解決的問題。 精靈控制項提供輸入的表單中一系列的步驟。 每個步驟是控制項的特定類型; vlastnost StepType 屬性所指定。 可用的步驟類型如下所示：

| **步驟類型** | **說明** |
| --- | --- |
| 自動 | 精靈會自動決定根據其位置步驟階層中的步驟的類型。 |
| 啟動 | 第一個步驟，通常用來呈現簡介的陳述式。 |
| 步驟 | 一般的步驟。 |
| 完成 | 最後一個步驟，通常用來呈現按鈕以完成精靈。 |
| 完成 | 顯示訊息，通訊成功或失敗。 |

> [!NOTE]
> 它使用 ASP.NET 控制項狀態的狀態記錄的精靈控制項。 因此，EnableViewState 屬性可以設定為 false，而不需要任何有利有弊。


這段影片會 Wizard 控制項逐步解說。


![](server-controls/_static/image2.png)


[開啟全螢幕影片](server-controls/_static/wizard1.wmv)


## <a name="localize-control"></a>當地語系化控制項

Localize 控制項很類似常值的控制項。 不過，Localize 控制項具有**模式**控制項加入至它的標記的呈現方式的屬性。 Mode 屬性支援下列值：

| **模式** | **說明** |
| --- | --- |
| Transform | 標記是根據提出要求的瀏覽器的通訊協定轉換。 |
| 通過 | 標記會轉譯成-是。 |
| 編碼 | 使用 HtmlEncode 編碼新增至控制項的標記。 |

## <a name="multiview-and-view-controls"></a>MultiView 和 View 控制項

MultiView 控制項做為檢視控制項的容器，並檢視控制項做為其他控制項 （如同面板控制項） 的容器。 MultiView 控制項中的每個檢視被以單一的檢視控制項。 第一個檢視中的控制項 MultiView 是檢視 0，第二個是檢視 1，等等。您可以藉由指定 MultiView 控制項的 ActiveViewIndex 切換檢視。

## <a name="substitution-control"></a>替代控制項

替代控制項用於搭配 ASP.NET 快取。 當您想要利用的快取，但您有必須更新每個要求 （亦即，頁面上的部分豁免於快取） 的頁面上的部分，替代元件會提供絕佳的解決方案。 控制項未實際呈現本身的任何輸出。 相反地，它會繫結至伺服器端程式碼中的方法。 要求頁面時，會呼叫的方法，並傳回的標記來取代替代控制項呈現。

替代控制項所繫結的方法，並透過指定**MethodName**屬性。 該方法必須符合下列準則：

- 它必須是靜態 (在中共用 VB) 方法。
- 它會接受一個類型參數的 HttpContext。
- 它會傳回字串，表示應該取代頁面上的控制項的標記。

替代控制項沒有能夠修改任何其他控制項在頁面上，但它並沒有存取目前的 HttpContext 透過其參數。

## <a name="gridview-control"></a>GridView 控制項

GridView 控制項是 DataGrid 控制項取代。 這個控制項將會涵蓋在更新版本的模組中的更多詳細資料。

## <a name="detailsview-control"></a>在 DetailsView 控制項

在 DetailsView 控制項可讓您顯示資料來源的單一記錄，以及編輯或刪除它。 它會在更新版本的模組中的更詳細地討論。

## <a name="formview-control"></a>FormView 控制項

FormView 控制項來顯示資料來源中的單一記錄中的可設定的介面。 它會在更新版本的模組中的更詳細地討論。

## <a name="accessdatasource-control"></a>AccessDataSource 控制項

AccessDataSource 控制項是用來將資料繫結的 Access 資料庫。 它會在更新版本的模組中的更詳細地討論。

## <a name="objectdatasource-control"></a>ObjectDataSource 控制項

ObjectDataSource 控制項用來支援三層式架構，使控制項可以是資料繫結至中介層商務物件而不是兩層的模型會直接與資料來源繫結控制項。 它會在更新版本的模組的詳細討論。

## <a name="xmldatasource-control"></a>XmlDataSource 控制項

XmlDataSource 控制項用來資料繫結至 XML 資料來源。 它會在更新版本的模組中的更詳細地討論。

## <a name="sitemapdatasource-control"></a>SiteMapDataSource 控制項

SiteMapDataSource 控制項提供站台地圖為基礎的網站巡覽控制項的資料繫結。 它會在更新版本的模組的詳細討論。

## <a name="sitemappath-control"></a>SiteMapPath 控制項

SiteMapPath 控制項顯示一系列通常稱為階層連結的導覽連結。 它會在更新版本的模組中的更詳細地討論。

## <a name="menu-control"></a>功能表控制項

功能表控制項顯示使用 DHTML 的動態功能表。 它會在更新版本的模組中的更詳細地討論。

## <a name="treeview-control"></a>TreeView 控制項

TreeView 控制項來顯示資料的階層式樹狀結構檢視。 它會在更新版本的模組中的更詳細地討論。

## <a name="login-control"></a>Login 控制項

Login 控制項提供一個機制來登入網站。 它會在更新版本的模組中的更詳細地討論。

## <a name="loginview-control"></a>LoginView 控制項

LoginView 控制項可讓不同的範本，根據使用者的登入狀態的顯示。 它會在更新版本的模組中的更詳細地討論。

## <a name="passwordrecovery-control"></a>Provider 控制項

在 ASP.NET 應用程式的使用者，Provider 控制項用來擷取遺忘的密碼。 它會在更新版本的模組中的更詳細地討論。

## <a name="loginstatus"></a>LoginStatus

LoginStatus 控制項顯示使用者的登入狀態。 它會在更新版本的模組中的更詳細地討論。

## <a name="loginname"></a>LoginName

LoginName 控制項之後都記錄在 ASP.NET 應用程式顯示使用者的使用者名稱。 它會在更新版本的模組中的更詳細地討論。

## <a name="createuserwizard"></a>CreateUserWizard

CreateUserWizard 是一個可設定的精靈，讓使用者能夠在 ASP.NET 應用程式建立使用 ASP.NET 成員資格帳戶。 它會在更新版本的模組中的更詳細地討論。

## <a name="changepassword"></a>ChangePassword

ChangePassword 控制項可讓使用者變更其密碼為 ASP.NET 應用程式。 它會在更新版本的模組中的更詳細地討論。

## <a name="various-webparts"></a>各種網頁組件

ASP.NET 2.0 隨附於各種網頁組件。 這些將會在更新版本的模組中有詳細說明。
