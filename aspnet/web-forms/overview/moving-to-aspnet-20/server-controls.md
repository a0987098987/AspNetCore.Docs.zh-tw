---
uid: web-forms/overview/moving-to-aspnet-20/server-controls
title: 伺服器控制項 |Microsoft 文件
author: microsoft
description: ASP.NET 2.0 增強伺服器控制項，在許多方面。 在此模組中，我們會部分的 ASP.NET 2.0 的方式與 Visual Studio 200 的架構變更...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 43f6ac47-76fc-4cf7-8e9f-c18ce673dfd8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/server-controls
msc.type: authoredcontent
ms.openlocfilehash: 72e9cac7cf9a01791c30783fa56ad7ea205a5a11
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
ms.locfileid: "28885189"
---
<a name="server-controls"></a>伺服器控制項
====================
by [Microsoft](https://github.com/microsoft)

> ASP.NET 2.0 增強伺服器控制項，在許多方面。 在此模組中，我們將討論的一些架構 ASP.NET 2.0 的方式變更，而 Visual Studio 2005 處理的伺服器控制項。


ASP.NET 2.0 增強伺服器控制項，在許多方面。 在此模組中，我們將討論的一些架構 ASP.NET 2.0 的方式變更，而 Visual Studio 2005 處理的伺服器控制項。

## <a name="view-state"></a>檢視狀態

在檢視狀態在 ASP.NET 2.0 的主要變更是大幅減少大小。 請考慮只在其上的行事曆控制項的頁面。 以下是 ASP.NET 1.1 中的檢視狀態。

[!code-css[Main](server-controls/samples/sample1.css)]

現在如下的檢視狀態上完全相同的頁面在 ASP.NET 2.0。

[!code-css[Main](server-controls/samples/sample2.css)]

這是很重要的變更，並考慮檢視狀態來回傳送透過網路傳輸，這項變更可以讓開發人員顯著的效能增加。 檢視狀態的大小縮減主要是因為我們在內部處理的方式。 請記住檢視狀態是 Base64 編碼字串。 若要進一步了解在 ASP.NET 2.0 在檢視狀態變更，讓我們看看從上述的範例已解碼的值。

以下是解碼 1.1 的檢視狀態：

[!code-css[Main](server-controls/samples/sample3.css)]

這可能會看起來有點像胡說八道，但有一種模式。 在 ASP.NET 中 1.x，我們用來識別資料類型的單一字元，並分隔值使用&lt;&gt;字元。 上面的檢視狀態範例"t"表示數。 數包含一組 ArrayLists （"l"代表陣列）。其中一個這些 ArrayLists 包含 Int32 ("i") 值是 1，另包含另一個數。 數包含一組 ArrayLists 等等。要記得的重點是，我們使用 Triplets 包含組、 我們識別字母，透過資料型別和我們使用&lt;和&gt;做為分隔符號的字元。

在 ASP.NET 2.0 中，已解碼的檢視狀態看起來稍有不同。

[!code-powershell[Main](server-controls/samples/sample4.ps1)]

您應該注意到巨大的變更，已解碼的檢視狀態的外觀。 這項變更有數個架構 underpinnings。 檢視狀態中 ASP.NET 1.x 用於 LosFormatter 序列化資料。 在 2.0 中，我們會使用新的 ObjectStateFormatter 類別。 這個類別已特別設計來協助序列化和還原序列化的檢視狀態，以及控制項狀態。 （下一節將討論控制項狀態）。有許多變更之序列化和還原序列化發生的方法取得的優勢。 不像使用 TextWriter LosFormatter，ObjectStateFormatter 使用 BinaryWriter 事實的最大的其中一個。 這可讓 ASP.NET 2.0，若要儲存檢視狀態一連串的位元組，而不是字串。 例如，採用整數。 在 ASP.NET 1.1 中，整數所需的檢視狀態的 4 個位元組。 在 ASP.NET 2.0 中，該相同的整數，只需要 1 個位元組。 進行其他的增強功能減少儲存的檢視狀態的變更。 日期時間值，例如，現在儲存 TickCount 而非字串。

如同所有作業，都沒有，特別注意支付至其中一個 1.x 中的檢視狀態的最大取用者是資料格和類似之控制項的事實。 例如而言檢視狀態的 DataGrid 控制項的主要缺點是資訊的它通常包含大量重複。 在 ASP.NET 中 1.x 重複資訊只會儲存和上一次導致過大的檢視狀態。 在 ASP.NET 2.0 中，我們可以使用 新 IndexedString 類別來儲存這類資料。 若重複出現的字串，我們只會儲存權杖 IndexedString 和 IndexedString 物件的執行中的表格中的索引。

## <a name="control-state"></a>控制項的狀態

其中一個開發人員就必須檢視狀態主要 gripes 是它加入至 HTTP 內容的大小。 如先前所述，檢視狀態的最大取用者的其中一個是 DataGrid 控制項。 若要避免大量的資料格，所產生的檢視狀態，許多開發人員只是停用該控制項的檢視狀態。 不幸的是，該方案不一定是一個很好。 檢視狀態中 ASP.NET 1.x 不只包含資料所需的正確控制項的功能。 它也包含有關控制項的 UI 狀態的資訊。 這表示如果您想要允許重新編頁上的資料格，您必須啟用檢視狀態，即使您不需要的所有 UI 資訊的檢視狀態包含。 它是想到的案例。

在 ASP.NET 2.0 中，控制項狀態以解決該問題妥善透過引進的控制項狀態。 控制項狀態會包含適當的控制項功能的絕對必要的資料。 不同於檢視狀態，無法停用控制項狀態。 因此，很重要的小心控制儲存在控制項狀態的資料。

> [!NOTE]
> 中的檢視狀態與保存控制項狀態\_ \_VIEWSTATE 隱藏的表單欄位。


這段影片會逐步解說中的檢視狀態，以及控制項狀態。


![](server-controls/_static/image1.png)


[開啟全螢幕視訊](server-controls/_static/state1.wmv)


為了讓伺服器控制項，以讀取和寫入控制狀態，您必須採取三個步驟。

## <a name="step-1-call-the-registerrequirescontrolstate-method"></a>步驟 1： 呼叫 RegisterRequiresControlState 方法

RegisterRequiresControlState 方法會通知 ASP.NET 控制項需要保存控制項狀態。 它會控制哪些是已註冊的控制項類型的一個引數。

請務必注意註冊不會保存要求要求。 因此，呼叫這個方法必須是每個要求控制項是否保存控制項狀態。 建議的方法呼叫 OnInit 中。

[!code-csharp[Main](server-controls/samples/sample5.cs)]

## <a name="step-2-override-savecontrolstate"></a>步驟 2： 覆寫 SaveControlState

SaveControlState 方法儲存自上次的回傳控制項的控制項的狀態變更。 它會傳回物件，代表控制項的狀態。

## <a name="step-3-override-loadcontrolstate"></a>步驟 3： 覆寫 LoadControlState

LoadControlState 方法會將已儲存的狀態載入控制項。 此方法會採用一個引數的型別物件，包含控制項的儲存的狀態。

## <a name="full-xhtml-compliance"></a>完整的 XHTML 相容性

任何 Web 開發人員知道標準 Web 應用程式中的重要性。 為了維持標準為基礎的開發環境，ASP.NET 2.0 完全是相容的 XHTML。 因此，所有標記都會轉譯根據 XHTML 標準的瀏覽器支援 HTML 4.0 或更新版本。

在 ASP.NET 1.1 中的文件類型定義如下所示：

[!code-html[Main](server-controls/samples/sample6.html)]

在 ASP.NET 2.0 的預設文件類型定義如下所示：

[!code-html[Main](server-controls/samples/sample7.html)]

如果您選擇，您可以變更透過組態檔中的 xhtmlConformance 節點的預設 XHML 相容性。 例如，在 web.config 檔案中的下列節點會變成 XHTML 相容性 XHTML 1.0 Strict:

[!code-xml[Main](server-controls/samples/sample8.xml)]

如果您選擇，您也可以設定要使用舊版 ASP.NET 中使用的設定 ASP.NET 1.x，如下所示：

[!code-xml[Main](server-controls/samples/sample9.xml)]

## <a name="adaptive-rendering-using-adapters"></a>自動調整轉譯時使用配接器

在 ASP.NET 中 1.x，組態檔包含&lt;browserCaps&gt;填入 HttpBrowserCapabilities 物件的區段。 這個物件允許開發人員以判斷哪些裝置正在進行特定的要求，並適當地轉譯程式碼。 在 ASP.NET 2.0 中，模型已經改良，現在會使用新的 ControlAdapter 類別。 ControlAdapter 類別覆寫控制項的生命週期中的事件，並控制使用者代理程式的功能為基礎的控制項的呈現方式。 瀏覽器定義檔案 （副檔名為.browser 檔案的檔案） 儲存在 c:\windows\microsoft.net\framework\v2.0 定義特定使用者代理程式的功能。\* \* \* \*\CONFIG\Browsers 資料夾。

> [!NOTE]
> ControlAdapter 類別是抽象類別。


就像是&lt;browserCaps&gt;區段 1.x 瀏覽器定義檔中的使用規則運算式剖析以找出要求的瀏覽器使用者代理字串。 它它們定義該使用者代理程式的特定功能。 ControlAdapter 呈現控制項透過轉譯方法。 因此，如果您覆寫轉譯方法，您不應該呼叫轉譯基底類別。 如此一來，可能會導致發生兩次，一次的配接器，一次在控制項本身呈現。

## <a name="developing-a-custom-adapter"></a>開發自訂配接器

您可以開發自訂配接器，繼承自 ControlAdapter。 此外，您可以繼承自抽象類別 PageAdapter 萬一頁面需要配接器的位置。 對應的控制項，以自訂配接器透過完成&lt;controlAdapters&gt;瀏覽器定義檔案中的項目。 例如，下列 XML 程式碼從瀏覽器定義檔案對應功能表控制項 MenuAdapter 類別：

[!code-html[Main](server-controls/samples/sample10.html)]

使用此模型時，會變得很容易讓控制項開發人員為目標的特定裝置或瀏覽器。 它也是開發人員將擁有完整控制權頁面每個裝置上轉譯的方式相當簡單。

## <a name="per-device-rendering"></a>每一裝置轉譯

在 ASP.NET 2.0 的伺服器控制項屬性可以指定每個裝置使用瀏覽器專屬的前置詞。 例如，下列程式碼會變更標籤，視哪一種裝置用來瀏覽網頁的文字。

[!code-aspx[Main](server-controls/samples/sample11.aspx)]

當從 Internet Explorer 瀏覽包含此標籤的頁面時，標籤會顯示文字說 「 瀏覽從 Internet Explorer。 」 當從 Firefox 瀏覽網頁時，標籤會顯示文字 「 瀏覽從 Firefox。 」 當從其他任何裝置瀏覽網頁時，它會顯示 「 您瀏覽從未知的裝置。 」 可以使用這個特殊的語法來指定任何屬性。

## <a name="setting-focus"></a>設定焦點

有關如何在特定的控制項上設定初始焦點常見問題集 ASP.NET 1.x 開發人員。 比方說，在登入頁面上，最好擁有第一次載入頁面時，取得焦點的 [使用者識別碼] 文字方塊。 在 ASP.NET 1.x，如此一來會需要寫入一些用戶端指令碼。 即使這類指令碼是簡單的工作，它不再需要 ASP.NET 2.0 中這點受惠 SetFocus 方法。 SetFocus 方法會採用一個引數指出應該接收焦點的控制項。 這個引數可以是用戶端控制項的 ID 做為字串或伺服器控制項，以控制物件的名稱。 比方說，若要設定文字方塊控制項第一次載入頁面時，呼叫 txtUserID 初始焦點，將下列程式碼加入至頁面\_負載：

[!code-csharp[Main](server-controls/samples/sample12.cs)]

-或

[!code-csharp[Main](server-controls/samples/sample13.cs)]

ASP.NET 2.0 使用呈現的用戶端函式，將焦點設定 Webresource.axd 處理常式 （先前所討論）。 用戶端函式的名稱是 WebForm\_AutoFocus 如下所示：

[!code-html[Main](server-controls/samples/sample14.html)]

或者，您可以使用控制項的焦點方法，將初始焦點設定該控制項。 焦點方法衍生自 Control 類別，並使用所有 ASP.NET 2.0 控制項。 它也可將焦點設定至特定控制項，當發生驗證錯誤。 將涵蓋的更新版本的模組中。

## <a name="new-server-controls-in-aspnet-20"></a>在 ASP.NET 2.0 的新伺服器控制項

以下是在 ASP.NET 2.0 的新伺服器控制項。 我們將在這些更新版本的模組中的某些移到更多詳細資料。

## <a name="imagemap-control"></a>ImageMap 控制項

ImageMap 控制項可讓您加入的映像，可以起始回傳或巡覽至 URL 的作用點。 有可用的三種類型的作用點。CircleHotSpot、 RectangleHotSpot 和 PolygonHotSpot。 透過 Visual Studio 中或以程式設計方式在程式碼中的集合編輯器加入熱點。 沒有適用於繪製在影像上的作用點的使用者介面。 必須以宣告方式指定的座標和大小或作用點的半徑。 也是沒有作用區設計工具中的視覺表示。 如果作用區設定為瀏覽至 URL，URL 會指定透過 NavigateUrl 屬性的作用點。 在 post 回作用點，PostBackValue 屬性可讓您在伺服器端程式碼中傳遞回傳可擷取的字串。


![在 Visual Studio 中的作用點集合編輯器](server-controls/_static/image1.jpg)

**圖 1**： 在 Visual Studio 中的作用點集合編輯器


## <a name="bulletedlist-control"></a>BulletedList 控制項

BulletedList 控制項是可以輕鬆地進行資料繫結項目符號清單。 清單可以排序 （編號的） 或未透過 BulletStyle 屬性排序。 在清單中的每個項目會以清單項目物件。


![Visual Studio 中設定 BulletedList 控制項](server-controls/_static/image1.gif)

**圖 2**: Visual Studio 中設定 BulletedList 控制項


## <a name="hiddenfield-control"></a>Hiddenfield

HiddenField 控制頁面上，其值可用於伺服器端程式碼中加入隱藏的表單欄位。 通常應該隱藏的表單欄位的值會維持不變回傳之間。 但是，它可能是惡意使用者可以變更為值先前的回傳。 如果發生這種情況，HiddenField 控制項將會引發 ValueChanged 事件。 如果您有 HiddenField 控制項中的機密資訊，而且您想要確保維持不變，您應該在程式碼中處理 ValueChanged 事件。

## <a name="fileupload-control"></a>檔案上傳控制項

在 ASP.NET 2.0 中的檔案上傳控制項讓您可以將檔案上傳至 Web 伺服器，透過 ASP.NET 網頁。 此控制項是相當類似於 ASP.NET 1.x HtmlInputFile 類別有一些例外狀況。 在 ASP.NET 中 1.x 曾經建議 null 檢查 PostedFile 屬性，以判斷您是否有正確的檔案。 在 ASP.NET 2.0 中的檔案上傳控制項，將新的 HasFile 屬性達成相同目的，您可以使用而且更有效率。

PostedFile 屬性仍可供存取 HttpPostedFile 物件，但某些 HttpPostedFile 功能就可在本質上與檔案上傳控制項。 例如，若要將上傳的檔案儲存在 ASP.NET 1.x 您另存新檔上呼叫方法 HttpPostedFile 物件。 使用 ASP.NET 2.0 中的檔案上傳控制項，您可以在檔案上傳控制項本身呼叫 SaveAs 方法。

另一個 2.0 行為 （與可能是最重要的變更） 中的重大變更是它已不再需要儲存之前，先將整個上傳的檔案載入記憶體。 1.x，在已上傳任何檔案會儲存完全讀入記憶體之前寫入至磁碟。 這個架構可預防大型檔案上的傳。

在 ASP.NET 2.0 httpRuntime 元素 requestLengthDiskThreshold 屬性可讓您設定多少 Kb 會保留在記憶體之前寫入的緩衝區寫入磁碟。

**重要**: MSDN 文件 （及其他位置的文件） 指定這個值是以位元組為單位 （千位元組） 且預設值是 256。 實際上以 kb 為單位指定的值，預設值為 80。 有預設值是 80 K，我們會確保在大型物件堆積不 estado intermedio 緩衝區。

## <a name="wizard-control"></a>精靈控制項

它是會遇到 ASP.NET 開發人員掙扎與嘗試收集資訊數列中的 「 頁面 」 使用面板或傳輸 頁面，以相當常見。 常常會，工作是令人沮喪，會耗用的時間。 新的精靈控制項藉由使用中的精靈介面，使用者熟悉的線性和非線性步驟解決的問題。 精靈控制項提供輸入的表單，以一系列的步驟。 每個步驟是控制項的特定類型 StepType 屬性所指定。 可用的步驟類型如下所示：

| **步驟類型** | **Explanation** |
| --- | --- |
| 自動 | 精靈會自動決定它步驟階層內的位置為基礎的步驟的類型。 |
| 啟動 | 第一個步驟中，通常用來呈現簡介的陳述式。 |
| 步驟 | 一般的步驟。 |
| 完成 | 最後一個步驟，通常用來呈現按鈕以完成精靈。 |
| 完成 | 顯示通訊成功或失敗的訊息。 |

> [!NOTE]
> 精靈控制項會追蹤的狀態使用 ASP.NET 控制項狀態。 因此，EnableViewState 屬性可以設定為 false，不含任何而妨礙。


這段影片是精靈控制項的逐步解說。


![](server-controls/_static/image2.png)


[開啟全螢幕視訊](server-controls/_static/wizard1.wmv)


## <a name="localize-control"></a>當地語系化控制項

與常值的控制項相似 Localize 控制項。 不過，有 Localize 控制項**模式**控制項加入至它的標記的呈現方式的屬性。 Mode 屬性支援下列值：

| **模式** | **Explanation** |
| --- | --- |
| Transform | 標記是根據提出要求之瀏覽器的通訊協定轉換。 |
| 通過 | 標記會轉譯為位是。 |
| 編碼 | 使用 HtmlEncode 編碼加入至控制項的標記。 |

## <a name="multiview-and-view-controls"></a>MultiView 和檢視控制項

MultiView 控制項做為容器的檢視控制項，並檢視控制項做為其他控制項的容器 （非常類似面板控制項）。 MultiView 控制項中的每個檢視是由單一的檢視控制項表示。 MultiView 之第一個檢視控制項是檢視 0，第二個是 1、 檢視等等。您可以藉由指定 MultiView 控制項 ActiveViewIndex 切換檢視。

## <a name="substitution-control"></a>替代控制項

替代控制項用於搭配 ASP.NET 快取。 在您想要充分利用快取，但您有必須更新每個要求 （亦即，頁面上的部分豁免於快取） 的頁面上的部分情況下，替代元件提供絕佳的解決方案。 控制項不會實際呈現其本身的任何輸出。 相反地，它會繫結中伺服器端程式碼的方法。 要求頁面時，會呼叫的方法，傳回的標記代替替代控制項。

替代控制項所繫結的方法透過指定**MethodName**屬性。 該方法必須符合下列準則：

- 它必須是靜態 (共用中的 VB) 方法。
- 它會接受一個參數的型別 HttpContext。
- 它會傳回字串，表示應該取代頁面上的控制項的標記。

替代控制項沒有能夠修改任何其他控制項在頁面上，但它並沒有存取目前的 HttpContext 透過其參數。

## <a name="gridview-control"></a>GridView 控制項

GridView 控制項是 DataGrid 控制項取代。 此控制項將涵蓋的更新版本的模組中詳細說明。

## <a name="detailsview-control"></a>在 DetailsView 控制項

在 DetailsView 控制項可讓您顯示資料來源的單一記錄，以及編輯或刪除它。 它會在更新版本的模組中的更詳細地討論。

## <a name="formview-control"></a>在 FormView 控制項

在 FormView 控制項來顯示資料來源的單一記錄的可設定的介面。 它會在更新版本的模組中的更詳細地討論。

## <a name="accessdatasource-control"></a>AccessDataSource Control

AccessDataSource 控制項是用來存取資料庫的資料繫結。 它會在更新版本的模組中的更詳細地討論。

## <a name="objectdatasource-control"></a>ObjectDataSource 控制項

ObjectDataSource 控制項用來支援三層式架構，使控制項可以是資料繫結至中介層商務物件，而不是兩層的模型會直接與資料來源繫結控制項。 將更新版本的模組中的更詳細地討論。

## <a name="xmldatasource-control"></a>XmlDataSource 控制項

XmlDataSource 控制項用來資料繫結到 XML 資料來源。 它會在更新版本的模組中的更詳細地討論。

## <a name="sitemapdatasource-control"></a>Treeview 控制項

Treeview 控制項提供站台地圖為基礎的站台瀏覽控制項的資料繫結。 將更新版本的模組中的更詳細地討論。

## <a name="sitemappath-control"></a>SiteMapPath 控制項

SiteMapPath 控制項顯示一系列統稱為階層連結列瀏覽連結。 它會在更新版本的模組中的更詳細地討論。

## <a name="menu-control"></a>功能表控制項

此功能表控制項顯示使用 DHTML 的動態功能表。 它會在更新版本的模組中的更詳細地討論。

## <a name="treeview-control"></a>TreeView 控制項

TreeView 控制項用來顯示資料的階層式樹狀檢視。 它會在更新版本的模組中的更詳細地討論。

## <a name="login-control"></a>登入控制項

登入控制項提供一個機制來登入網站。 它會在更新版本的模組中的更詳細地討論。

## <a name="loginview-control"></a>LoginView 控制項

LoginView 控制項可讓顯示的不同使用者的登入狀態為基礎的範本。 它會在更新版本的模組中的更詳細地討論。

## <a name="passwordrecovery-control"></a>Provider 控制項

Provider 控制項用來擷取忘記密碼，ASP.NET 應用程式的使用者。 它會在更新版本的模組中的更詳細地討論。

## <a name="loginstatus"></a>LoginStatus

LoginStatus 控制項顯示使用者的登入狀態。 它會在更新版本的模組中的更詳細地討論。

## <a name="loginname"></a>LoginName

LoginName 控制項之後正在登入的 ASP.NET 應用程式顯示使用者的使用者名稱。 它會在更新版本的模組中的更詳細地討論。

## <a name="createuserwizard"></a>CreateUserWizard

適用於 CreateUserWizard 是一個可設定的精靈，讓使用者能夠建立 ASP.NET 應用程式中使用的 ASP.NET 成員資格帳戶。 它會在更新版本的模組中的更詳細地討論。

## <a name="changepassword"></a>ChangePassword

ChangePassword 控制項可讓使用者變更其密碼，ASP.NET 應用程式。 它會在更新版本的模組中的更詳細地討論。

## <a name="various-webparts"></a>各種 web 組件

ASP.NET 2.0 提供了各種網頁組件。 這些都將涵蓋於更新版本的模組中的詳細資料。
