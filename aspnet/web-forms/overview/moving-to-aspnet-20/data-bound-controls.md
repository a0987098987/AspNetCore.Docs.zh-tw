---
uid: web-forms/overview/moving-to-aspnet-20/data-bound-controls
title: 資料繫結控制項 |Microsoft Docs
author: microsoft
description: 大部分的 ASP.NET 應用程式依賴某種程度的後端資料來源的資料呈現方式。 資料繫結控制項已經互動 w pivotal 的一部份...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 0e23ff32-646d-43f3-8bec-6b2313d3abd6
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-bound-controls
msc.type: authoredcontent
ms.openlocfilehash: adaf8a40c1877db4181e1b1c7a74a2ecbaa373ad
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368255"
---
<a name="data-bound-controls"></a>資料繫結控制項
====================
by [Microsoft](https://github.com/microsoft)

> 大部分的 ASP.NET 應用程式依賴某種程度的後端資料來源的資料呈現方式。 資料繫結控制項已被動態的 Web 應用程式中的資料互動的關鍵部分。 ASP.NET 2.0 導入了資料繫結控制項，包括新的 BaseDataBoundControl 類別與宣告式語法的一些重大改善。


大部分的 ASP.NET 應用程式依賴某種程度的後端資料來源的資料呈現方式。 資料繫結控制項已被動態的 Web 應用程式中的資料互動的關鍵部分。 ASP.NET 2.0 導入了資料繫結控制項，包括新的 BaseDataBoundControl 類別與宣告式語法的一些重大改善。

BaseDataBoundControl 做 DataBoundControl 類別和 HierarchicalDataBoundControl 類別的基底類別。 在這個模組中，我們將討論下列類別衍生自 DataBoundControl:

- AdRotator
- 清單控制項
- GridView
- FormView
- DetailsView

我們也將討論下列類別衍生自 HierarchicalDataBoundControl 類別：

- TreeView
- 功能表
- SiteMapPath

## <a name="databoundcontrol-class"></a>DataBoundControl 類別

DataBoundControl 類別是用來與表格式互動的抽象類別 （標記為 MustInherit 在 VB 中的） 或清單樣式的資料。 下列控制項是一些從 DataBoundControl 衍生的控制項。

## <a name="adrotator"></a>AdRotator

AdRotator 控制項可讓您連結至特定 URL 的網頁上顯示圖形的橫幅。 旋轉圖形顯示使用控制項的屬性。 您可以使用設定的頁面上顯示的特定 ad 頻率**印象**屬性廣告可以篩選及使用關鍵字篩選。

AdRotator 控制項使用的 XML 檔案或資料表的資料庫中的資料。 XML 檔案中使用下列屬性來設定 AdRotator 控制項。

### <a name="imageurl"></a>ImageUrl
廣告顯示影像 URL。

### <a name="navigateurl"></a>NavigateUrl
按一下廣告時，使用者應該要採取的 URL。 這應該是 URL 編碼。

### <a name="alternatetext"></a>AlternateText
替代文字，會顯示在工具提示與螢幕助讀程式讀取。 ImageUrl 所指定的影像無法使用時也會顯示。

### <a name="keyword"></a>關鍵字
定義使用關鍵字篩選時，可以使用的關鍵字。 如果指定，就會顯示符合關鍵字篩選器的關鍵字與這些廣告。

### <a name="impressions"></a>感想
決定特定廣告的可能出現的頻率的加權數字。 它是相對於相同檔案中的其他廣告的印象。 XML 檔案中的所有廣告的集體的廣告曝光數的最大值是 2,048,000,000 1。

### <a name="height"></a>高度
Ad 像素為單位的高度。

### <a name="width"></a>寬度
Ad 像素為單位的寬度。


> [!NOTE]
> 高度和寬度屬性覆寫 AdRotator 控制項本身的寬度與高度。


典型的 XML 檔案看起來可能如下所示：

[!code-xml[Main](data-bound-controls/samples/sample1.xml)]

在上述範例中，Contoso 的 ad 兩次，可能會因為印象屬性的值會顯示為適用於 ASP.NET 網站的 ad。

若要顯示從上述的 XML 檔案的廣告，AdRotator 控制項加入至頁面並設定**AdvertisementFile**屬性以指向的 XML 檔案，如下所示：

[!code-aspx[Main](data-bound-controls/samples/sample2.aspx)]

如果您選擇使用 AdRotator 控制項做為資料來源的資料庫資料表，您首先要設定資料庫，使用下列結構描述：

| **資料行名稱** | **資料類型** | **描述** |
| --- | --- | --- |
| 識別碼 | int | 主索引鍵。 此資料行可以具有任何名稱。 |
| ImageUrl | nvarchar (*長度*) | 相對或絕對影像的 URL 來顯示廣告。 |
| NavigateUrl | nvarchar (*長度*) | Ad 目標 URL。 如果您未提供值，ad 不是超連結。 |
| AlternateText | nvarchar (*長度*) | 如果找不到映像所顯示的文字。 在某些瀏覽器中，文字會顯示為工具提示。 替代文字也會使用協助工具，讓使用者能夠看圖形聽見大聲閱讀其描述。 |
| 關鍵字 | nvarchar (*長度*) | 網頁可以篩選在其的 ad 分類。 |
| 感想 | int(4) | 此數字表示的頻率顯示廣告的可能性。 較大的數字，越常 ad 將會顯示。 XML 檔案中的所有印象值的總和不得超過 2,048,000,000-1。 |
| 寬度 | int(4) | 單位為像素影像的寬度。 |
| 高度 | int(4) | 單位為像素影像的高度。 |

在您已經有具有不同的結構描述的資料庫的情況下，您可以使用**AlternateTextField**， **ImageUrlField**，並**NavigateUrlField**屬性對應AdRotator 屬性到您現有的資料庫。 AdRotator 控制項中顯示資料庫中的資料，請加入網頁中的資料來源控制項、 設定資料來源控制項，以指向您的資料庫的連接字串和將 AdRotator 控制項的**DataSourceID**資料來源控制項的 ID 屬性。 在其中您需要以程式設計方式設定 AdRotator 廣告的情況下，使用 AdCreated 事件。 AdCreated 事件接受兩個參數;一個物件，而另一個 AdCreatedEventArgs 的執行個體。 AdCreatedEventArgs 是正在建立的 ad 的參考。

在 DetailsView 控制項自動繫結至指定的資料來源控制項。

[!code-csharp[Main](data-bound-controls/samples/sample3.cs)]

## <a name="list-controls"></a>若要繫結至資料來源會實作System.Collections.IEnumerable介面，以程式設計方式設定為資料來源的 DetailsView 控制項的 DataSource 屬性，然後呼叫 DataBind 方法。

此控制項可用來顯示使用者輸入，其中可能包含惡意的用戶端指令碼。 檢查從用戶端可執行的指令碼、 SQL 陳述式，或其他程式碼傳送之前顯示在您的應用程式中的任何資訊。 ASP.NET 提供使用者輸入來封鎖指令碼和 HTML 的輸入的要求驗證功能。 明確宣告的資料行欄位可以顯示在具有自動產生的資料行欄位的組合。

同時使用時，明確宣告的資料行欄位會呈現第一次後, 接的自動產生的資料行欄位。 繫結至資料 GridView 控制項可以繫結至資料來源控制項 (例如**SqlDataSource**， **ObjectDataSource**等等)，以及實作 System.Collections.IEnumerable 任何資料來源介面 （例如 System.Data.DataView、 System.Collections.ArrayList 或 System.Collections.Hashtable）。 請使用下列方法之一的 GridView 控制項繫結至適當的資料來源類型：

| **運算式** | **描述** |
| --- | --- |
| 若要繫結至資料來源控制項，設定 GridView 控制項中的 [DataSourceID] 屬性的資料來源控制項的識別碼值。 {0:C} | GridView 控制項自動繫結至指定的資料來源控制項，並可讓您利用資料來源的控制項的功能來執行排序、 更新、 刪除和分頁功能。 這是要繫結至資料的慣用的方法。 要繫結至資料來源實作 System.Collections.IEnumerable 介面，以程式設計方式將 GridView 控制項的 DataSource 屬性設資料來源，然後呼叫 DataBind 方法。 |
| {0:D4} | 使用此方法時，在 GridView 控制項不提供內建排序、 更新、 刪除和分頁功能。 您必須自行提供這項功能。 GridView 控制項提供許多內建功能，可讓使用者排序、 更新、 刪除、 選取和逐頁瀏覽控制項中的項目。 |
| {0:N2}GridView 控制項繫結至資料來源控制項，在 GridView 控制項就可以利用資料來源控制項的功能，並提供自動排序、 更新和刪除功能。 | GridView 控制項可支援排序、 更新和刪除與其他類型的資料來源;不過，您必須提供這些作業的實作中的適當事件處理常式。 排序功能可以讓使用者按一下資料行的標頭排序 GridView 控制項相對於特定的資料行中的項目。 |
| {0:000.0} | GridView 控制項自動繫結至指定的資料來源控制項，並可讓您利用資料來源的控制項的功能來執行排序、 更新、 刪除和分頁功能。 若要啟用排序，請將 AllowSorting 屬性設定為，則為 true。 小於三位數的數字會填補零。 |
| {0:D} | 中的按鈕時，會啟用自動更新、 刪除和選取項目功能ButtonField或是TemplateField資料行 欄位中的，使用 編輯、 刪除 和 選取，命令名稱分別是已按下。 GridView 控制項可以自動將CommandField資料行欄位的編輯、 刪除或選取的按鈕，如果 [autogenerateeditbutton]、 [autogeneratedeletebutton] 或 AutoGenerateSelectButton 屬性設定為，則為 true分別。 日期格式取決於頁面或 Web.config 檔案的文化設定。 |
| {0:d} | 中的按鈕時，會啟用自動更新、 刪除和選取項目功能ButtonField或是TemplateField資料行 欄位中的，使用 編輯、 刪除 和 選取，命令名稱分別是已按下。 將記錄插入至資料來源不直接支援 GridView 控制項。 |
| {0:yy-MM-dd} | 中的按鈕時，會啟用自動更新、 刪除和選取項目功能ButtonField或是TemplateField資料行 欄位中的，使用 編輯、 刪除 和 選取，命令名稱分別是已按下。 不過，就可以使用的 GridView 控制項搭配 DetailsView 或 FormView 控制項來插入記錄。 |

## <a name="gridview"></a>GridView

而不是顯示資料來源中的所有記錄，在此同時，在 GridView 控制項可以自動分成記錄頁面。 您可以設定不同的組件控制項的樣式屬性，來自訂 GridView 控制項的外觀。

- 在 GridView 控制項中替代資料列樣式設定。
- 正在編輯的 GridView 控制項中的資料列樣式設定。
- 資料來源不包含任何記錄時，在 GridView 控制項中顯示之空白資料列樣式設定。
- GridView 控制項的頁尾資料列樣式設定。
- GridView 控制項的標頭資料列樣式設定。
- GridView 控制項的頁面巡覽區資料列樣式設定。
- 在 GridView 控制項中的資料列樣式設定。
- SelectedRowStyle
- 在 GridView 控制項中選取的資料列樣式設定。

**您也可以顯示或隱藏控制項的不同部分。**

下表列出可控制哪些部分會顯示或隱藏屬性。 ShowFooter 顯示或隱藏的 GridView 控制項頁尾區段。 ShowHeader 顯示或隱藏 GridView 控制項的標頭區段。

GridView 控制項提供數個事件，您可以進行程式設計。

| **下表列出支援 GridView 控制項的事件。** | **描述** |
| --- | --- |
| BoundField | 發生於按一下其中一個頁面巡覽區按鈕時，但之後 GridView 控制項處理分頁作業。 當您需要執行的工作之後使用者瀏覽到控制項中的不同頁面, 時，通常使用這個事件。 |
| ButtonField | 這個資料列的欄位型別通常用於顯示的布林值的欄位。 顯示內建的命令按鈕，以執行編輯、 插入或刪除在 DetailsView 控制項中的作業。 |
| CheckBoxField | 這個資料列的欄位型別可讓您的第二個欄位繫結至超連結的 URL。 在 DetailsView 控制項中顯示影像。 |
| CommandField | 顯示使用者定義內容根據指定的範本在 DetailsView 控制項中的資料列。 |
| HyperLinkField | 這個資料列的欄位型別可讓您建立自訂的資料列欄位。 根據預設，AutoGenerateRows 屬性設為，則為 true，這會自動產生繫結的資料列欄位物件的可繫結類型的每個欄位的資料來源中。 |
| ImageField | 有效的可繫結類型是字串、 DateTime、 Decimal、 Guid 和一組基本型別。 |
| 為文字，其中每個欄位會出現在資料來源的順序，即會每個欄位顯示在資料列。 | 自動產生的資料列提供快速且輕鬆的方式來顯示記錄中的每個欄位。 不過，利用 DetailsView 控制項的進階的功能，您必須明確宣告在 DetailsView 控制項中包含的資料列欄位。 |

若要宣告的資料列欄位，請先設定**AutoGenerateRows&lt;屬性設&gt;false**。 接下來，新增 開啟和關閉**&lt;欄位&gt;** 之間的開頭和結尾標記 DetailsView 控制項的標記。 最後，列出您想要包含的開頭和結尾之間的資料列欄位欄位標記。 指定資料列欄位都會加入至欄位集合中列出的順序。

欄位集合可讓您以程式設計方式管理在 DetailsView 控制項中的資料列欄位。 自動產生欄位不會新增至欄位集合的資料列。

## <a name="binding-to-data"></a>在 DetailsView 控制項可以繫結至資料來源控制項，例如SqlDataSource或 AccessDataSource，或實作 System.Collections.IEnumerable 介面，例如 System.Data.DataView，任何資料來源System.Collections.ArrayList 和 System.Collections.Hashtable。

請使用下列方法之一的 DetailsView 控制項繫結至適當的資料來源類型： 若要繫結至資料來源控制項，設定 DetailsView 控制項中的 [DataSourceID] 屬性的資料來源控制項的識別碼值。

- 在 DetailsView 控制項自動繫結至指定的資料來源控制項。 若要繫結至資料來源會實作System.Collections.IEnumerable介面，以程式設計方式設定為資料來源的 DetailsView 控制項的 DataSource 屬性，然後呼叫 DataBind 方法。 此控制項可用來顯示使用者輸入，其中可能包含惡意的用戶端指令碼。
- 檢查從用戶端可執行的指令碼、 SQL 陳述式，或其他程式碼傳送之前顯示在您的應用程式中的任何資訊。 ASP.NET 提供使用者輸入來封鎖指令碼和 HTML 的輸入的要求驗證功能。 您必須自行提供這項功能。

## <a name="data-operations"></a>資料作業

最後，列出您想要包含的開頭和結尾之間的資料列欄位欄位標記。 指定資料列欄位都會加入至欄位集合中列出的順序。

> [!NOTE]
> 欄位集合可讓您以程式設計方式管理在 DetailsView 控制項中的資料列欄位。


自動產生欄位不會新增至欄位集合的資料列。 在 DetailsView 控制項可以繫結至資料來源控制項，例如**SqlDataSource**或 AccessDataSource，或實作 System.Collections.IEnumerable 介面，例如 System.Data.DataView，任何資料來源System.Collections.ArrayList 和 System.Collections.Hashtable。

請使用下列方法之一的 DetailsView 控制項繫結至適當的資料來源類型： 若要繫結至資料來源控制項，設定 DetailsView 控制項中的 [DataSourceID] 屬性的資料來源控制項的識別碼值。

> [!NOTE]
> 在 DetailsView 控制項自動繫結至指定的資料來源控制項。 若要繫結至資料來源會實作System.Collections.IEnumerable介面，以程式設計方式設定為資料來源的 DetailsView 控制項的 DataSource 屬性，然後呼叫 DataBind 方法。


此控制項可用來顯示使用者輸入，其中可能包含惡意的用戶端指令碼。 若要啟用分頁，請將 AllowPaging 屬性設定為 **，則為 true**。

## <a name="customizing-the-user-interface"></a>自訂使用者介面

檢查從用戶端可執行的指令碼、 SQL 陳述式，或其他程式碼傳送之前顯示在您的應用程式中的任何資訊。 下表列出不同的樣式屬性。

| **樣式屬性** | **描述** |
| --- | --- |
| AlternatingRowStyle | ASP.NET 提供使用者輸入來封鎖指令碼和 HTML 的輸入的要求驗證功能。 當設定這個屬性時，資料列會顯示交替 RowStyle 設定並**AlternatingRowStyle**設定。 |
| EditRowStyle | 定義頁尾資料列的內容。 |
| EmptyDataRowStyle | 此範本通常會包含您想要顯示頁尾資料列中的任何其他內容。 |
| FooterStyle | 或者，您可以只指定 FooterText 屬性設定頁尾資料列中顯示的文字。 |
| HeaderStyle | HeaderTemplate |
| PagerStyle | 定義標頭資料列的內容。 |
| RowStyle | 此範本通常會包含您想要顯示標頭資料列中的任何其他內容。 當**AlternatingRowStyle**也會設定屬性，則資料列會顯示交替**RowStyle**設定和**AlternatingRowStyle**設定。 |
| 或者，您可以只指定藉由設定 HeaderText 屬性標頭資料列中顯示的文字。 | ItemTemplate |

FormView 控制項處於唯讀模式時，請定義資料列的內容。 此範本通常會包含用來顯示現有的資料錄的值。

| **Property** | **描述** |
| --- | --- |
| FormView 控制項處於插入模式時，請定義資料列的內容。 | 此範本通常會包含輸入的控制項和命令按鈕，使用者可以加入新的記錄。 |
| Pagertemplate 來 | 定義啟用分頁功能時顯示的頁面巡覽列的內容 (當 AllowPaging 屬性設定為，則為 true)。 |

### <a name="events"></a>事件

此範本通常會包含與使用者可以瀏覽至另一筆記錄的控制項。 這可讓您執行自訂的常式，每當事件發生時。 編輯項目範本和插入項目範本中的輸入的控制項可以使用雙向繫結運算式繫結至資料來源的欄位之用。

| **Event** | **描述** |
| --- | --- |
| PageIndexChanged | 這可讓 FormView 控制項，以自動擷取更新的輸入控制項的值或插入作業。 雙向繫結運算式也會允許編輯項目範本以自動顯示原始的欄位值中的輸入的控制項。 |
| PageIndexChanging | FormView 控制項可以繫結至資料來源控制項 (例如SqlDataSource，AccessDataSource， ObjectDataSource等等)，或會實作任何資料來源System.Collections.IEnumerable 介面 （例如 System.Data.DataView、 System.Collections.ArrayList 和 System.Collections.Hashtable）。 此事件通常用來取消分頁作業。 |
| 請使用下列方法之一的 FormView 控制項繫結至適當的資料來源類型： | 若要繫結至資料來源控制項，設定 FormView 控制項的 [DataSourceID] 屬性的資料來源控制項的識別碼值。 FormView 控制項自動繫結至指定的資料來源控制項，並可以利用資料來源控制項的功能，執行插入、 更新、 刪除和分頁功能。 |
| 若要繫結至資料來源會實作System.Collections.IEnumerable介面，以程式設計方式設定為資料來源的 FormView 控制項的 DataSource 屬性，然後呼叫 DataBind 方法。 | 使用此方法時，FormView 控制項不提供內建的插入、 更新、 刪除和分頁功能。 您需要使用適當的事件提供這項功能。 |
| FormView 控制項提供許多內建功能，可讓使用者更新、 刪除、 插入和控制項中的項目頁面上。 | FormView 控制項繫結至資料來源控制項，FormView 控制項就可以利用資料來源控制項的功能，並提供自動更新、 刪除、 插入和分頁功能。 FormView 控制項可以提供支援 update、 delete、 insert 和分頁作業，與其他類型的資料來源;不過，您必須將適當的事件處理常式提供這些作業的實作。 |
| 範本會使用 FormView 控制項，因為它不會提供自動產生命令按鈕，來執行更新、 刪除或插入作業。 | 您必須手動將這些命令按鈕，在適當的範本。 FormView 控制項可辨識某些按鈕具有他們CommandName設為特定值的屬性。 |
| 下表列出可在 FormView 控制項辨識的命令按鈕。 | Commandname 值 此事件通常用來檢查刪除作業的結果。 |
| 「 取消 」 | 用於更新或插入作業以取消作業，並捨棄使用者所輸入的值。 FormView 控制項然後回到 DefaultMode 屬性所指定的模式。 |
| 用於刪除作業，以刪除資料來源中顯示的資料錄。 | 引發的 ItemDeleting 和 ItemDeleted 事件。 [編輯] |
| 用於更新作業，以 FormView 控制項進入編輯模式。 | 中指定的內容EditItemTemplate屬性會顯示為資料列。 此事件通常會用來檢查更新作業的結果中。 |
| 「 插入 」 | 用於插入作業，以嘗試使用使用者所提供的值的資料來源中插入新的記錄。 引發 ItemInserting 和 ItemInserted 的事件。 |
| 「 新 」 | 用於插入作業，以 FormView 控制項放入插入模式。 指定資料列欄位都會加入至欄位集合中列出的順序。 |
| 欄位集合可讓您以程式設計方式管理在 DetailsView 控制項中的資料列欄位。 | 自動產生欄位不會新增至欄位集合的資料列。 在 DetailsView 控制項可以繫結至資料來源控制項，例如SqlDataSource或 AccessDataSource，或實作 System.Collections.IEnumerable 介面，例如 System.Data.DataView，任何資料來源System.Collections.ArrayList 和 System.Collections.Hashtable。 |
| 請使用下列方法之一的 DetailsView 控制項繫結至適當的資料來源類型： | 若要繫結至資料來源控制項，設定 DetailsView 控制項中的 [DataSourceID] 屬性的資料來源控制項的識別碼值。 在 DetailsView 控制項自動繫結至指定的資料來源控制項。 |
| 排序 | 若要繫結至資料來源會實作System.Collections.IEnumerable介面，以程式設計方式設定為資料來源的 DetailsView 控制項的 DataSource 屬性，然後呼叫 DataBind 方法。 通常在取消排序作業，或執行自訂排序常式，會使用這個事件。 |

## <a name="formview"></a>FormView

在 DetailsView 控制項可以繫結至資料來源控制項，例如SqlDataSource或 AccessDataSource，或實作 System.Collections.IEnumerable 介面，例如 System.Data.DataView，任何資料來源System.Collections.ArrayList 和 System.Collections.Hashtable。 請使用下列方法之一的 DetailsView 控制項繫結至適當的資料來源類型： 若要繫結至資料來源控制項，設定 DetailsView 控制項中的 [DataSourceID] 屬性的資料來源控制項的識別碼值。 在 DetailsView 控制項自動繫結至指定的資料來源控制項。

- 若要繫結至資料來源會實作System.Collections.IEnumerable介面，以程式設計方式設定為資料來源的 DetailsView 控制項的 DataSource 屬性，然後呼叫 DataBind 方法。
- 插入包含內建的功能。
- 資料來源不包含任何記錄時，在 GridView 控制項中顯示之空白資料列樣式設定。
- GridView 控制項的頁尾資料列樣式設定。
- 以程式設計方式存取 FormView 物件模型，以動態方式設定屬性，處理事件，等等。
- 透過使用者定義的範本、 主題和樣式的可自訂外觀。

## <a name="templates"></a>範本

FormView 控制項來顯示內容，您必須建立不同的組件控制項的範本。 大部分的範本是選擇性的。不過，您必須建立已在控制項模式的範本。 比方說，支援將記錄插入的 FormView 控制項都必須定義插入項目範本。 下表列出您可以建立不同的範本。

| **範本類型** | **描述** |
| --- | --- |
| EditItemTemplate | FormView 控制項處於編輯模式時，請定義資料列的內容。 此範本通常會包含輸入的控制項和命令按鈕，使用者可以編輯現有的記錄。 |
| EmptyDataTemplate | 定義當 FormView 控制項繫結至資料來源不包含任何資料錄時，顯示空的資料列的內容。 此範本通常會包含來提醒使用者資料來源不包含任何記錄的內容。 |
| FooterTemplate | 定義頁尾資料列的內容。 此範本通常會包含您想要顯示頁尾資料列中的任何其他內容。 或者，您可以只指定 FooterText 屬性設定頁尾資料列中顯示的文字。 |
| HeaderTemplate | 定義標頭資料列的內容。 此範本通常會包含您想要顯示標頭資料列中的任何其他內容。 或者，您可以只指定藉由設定 HeaderText 屬性標頭資料列中顯示的文字。 |
| ItemTemplate | FormView 控制項處於唯讀模式時，請定義資料列的內容。 此範本通常會包含用來顯示現有的資料錄的值。 |
| InsertItemTemplate | FormView 控制項處於插入模式時，請定義資料列的內容。 此範本通常會包含輸入的控制項和命令按鈕，使用者可以加入新的記錄。 |
| Pagertemplate 來 | 定義啟用分頁功能時顯示的頁面巡覽列的內容 (當 AllowPaging 屬性設定為 **，則為 true**)。 此範本通常會包含與使用者可以瀏覽至另一筆記錄的控制項。 |

編輯項目範本和插入項目範本中的輸入的控制項可以使用雙向繫結運算式繫結至資料來源的欄位之用。 這可讓 FormView 控制項，以自動擷取更新的輸入控制項的值或插入作業。 雙向繫結運算式也會允許編輯項目範本以自動顯示原始的欄位值中的輸入的控制項。

### <a name="binding-to-data"></a>在 DetailsView 控制項可以繫結至資料來源控制項，例如SqlDataSource或 AccessDataSource，或實作 System.Collections.IEnumerable 介面，例如 System.Data.DataView，任何資料來源System.Collections.ArrayList 和 System.Collections.Hashtable。

FormView 控制項可以繫結至資料來源控制項 (例如**SqlDataSource**，AccessDataSource， **ObjectDataSource**等等)，或會實作任何資料來源System.Collections.IEnumerable 介面 （例如 System.Data.DataView、 System.Collections.ArrayList 和 System.Collections.Hashtable）。 請使用下列方法之一的 FormView 控制項繫結至適當的資料來源類型：

- 若要繫結至資料來源控制項，設定 FormView 控制項的 [DataSourceID] 屬性的資料來源控制項的識別碼值。 FormView 控制項自動繫結至指定的資料來源控制項，並可以利用資料來源控制項的功能，執行插入、 更新、 刪除和分頁功能。 此控制項可用來顯示使用者輸入，其中可能包含惡意的用戶端指令碼。
- 若要繫結至資料來源會實作**System.Collections.IEnumerable**介面，以程式設計方式設定為資料來源的 FormView 控制項的 DataSource 屬性，然後呼叫 DataBind 方法。 使用此方法時，FormView 控制項不提供內建的插入、 更新、 刪除和分頁功能。 您需要使用適當的事件提供這項功能。

## <a name="data-operations"></a>資料作業

FormView 控制項提供許多內建功能，可讓使用者更新、 刪除、 插入和控制項中的項目頁面上。 FormView 控制項繫結至資料來源控制項，FormView 控制項就可以利用資料來源控制項的功能，並提供自動更新、 刪除、 插入和分頁功能。 FormView 控制項可以提供支援 update、 delete、 insert 和分頁作業，與其他類型的資料來源;不過，您必須將適當的事件處理常式提供這些作業的實作。

範本會使用 FormView 控制項，因為它不會提供自動產生命令按鈕，來執行更新、 刪除或插入作業。 您必須手動將這些命令按鈕，在適當的範本。 FormView 控制項可辨識某些按鈕具有他們**CommandName**設為特定值的屬性。 下表列出可在 FormView 控制項辨識的命令按鈕。

| **Button** | **Commandname 值** | **描述** |
| --- | --- | --- |
| 取消 | 「 取消 」 | 用於更新或插入作業以取消作業，並捨棄使用者所輸入的值。 FormView 控制項然後回到 DefaultMode 屬性所指定的模式。 |
| 刪除 | "Delete" | 用於刪除作業，以刪除資料來源中顯示的資料錄。 引發的 ItemDeleting 和 ItemDeleted 事件。 |
| Edit | [編輯] | 用於更新作業，以 FormView 控制項進入編輯模式。 中指定的內容**EditItemTemplate**屬性會顯示為資料列。 |
| Insert | 「 插入 」 | 用於插入作業，以嘗試使用使用者所提供的值的資料來源中插入新的記錄。 引發 ItemInserting 和 ItemInserted 的事件。 |
| 新增 | 「 新 」 | 用於插入作業，以 FormView 控制項放入插入模式。 中指定的內容**InsertItemTemplate**屬性會顯示為資料列。 |
| 頁面 | 「 頁面 」 | 用於分頁作業，以代表執行分頁的頁面巡覽列中的按鈕。 指定的分頁作業，請設定**CommandArgument** "Next"、"Prev"、"First"、"Last"，或要瀏覽至頁面的索引按鈕的屬性。 引發 PageIndexChanging 和 PageIndexChanged 的事件。 |
| 更新 | 「 更新 」 | 用於更新作業，以嘗試使用使用者所提供的值更新資料來源中所顯示的資料錄。 會引發 ItemUpdating 和 ItemUpdated 事件。 |

不同於刪除按鈕 （這會刪除顯示的資料錄立即），按一下 [編輯] 或 [新增] 按鈕時的 FormView 控制項進入編輯，或分別插入模式。 在編輯模式中，內容包含在**EditItemTemplate**屬性會顯示目前的資料項目。 一般而言，使 編輯 按鈕會取代的更新 和 取消 按鈕可以定義編輯項目範本。 適用於欄位的資料類型 （例如文字方塊或核取方塊控制項） 的輸入的控制項通常也會顯示使用者修改欄位的值。 按一下 [更新] 按鈕更新資料來源中的記錄，而按一下 [取消] 按鈕，則會放棄任何變更。

同樣地，將包含的內容**InsertItemTemplate**控制項處於插入模式時，將會顯示之資料項目的屬性。 新增 按鈕會取代插入 和 取消 按鈕，並讓使用者輸入新的資料錄的值會顯示空白的輸入的控制項，通常被定義插入項目範本。 按一下 [插入] 按鈕在資料來源中，插入資料錄，而按一下 [取消] 按鈕，則會放棄任何變更。

FormView 控制項提供分頁功能，可讓使用者瀏覽至其他資料來源中的記錄。 啟用時，頁面巡覽列會顯示在 FormView 控制項包含頁面導覽控制項中。 若要啟用分頁，將**AllowPaging**屬性設 **，則為 true**。 您可以藉由設定 PagerStyle 中包含的物件屬性和 PagerSettings 屬性自訂的頁面巡覽列。 而不是使用內建的頁面巡覽列 UI，您可以建立自己的 UI，使用**pagertemplate 來**屬性。

## <a name="customizing-the-user-interface"></a>自訂使用者介面

您可以設定不同的組件控制項的樣式屬性，來自訂 FormView 控制項的外觀。 下表列出不同的樣式屬性。

| **樣式屬性** | **描述** |
| --- | --- |
| EditRowStyle | 樣式設定為資料列時 FormView 控制項處於編輯模式。 |
| EmptyDataRowStyle | 當資料來源未包含任何記錄，FormView 控制項中顯示之空白資料列樣式設定。 |
| FooterStyle | FormView 控制項的頁尾資料列樣式設定。 |
| HeaderStyle | FormView 控制項標頭資料列樣式設定。 |
| InsertRowStyle | 樣式設定為資料列時 FormView 控制項處於插入模式。 |
| PagerStyle | 啟用分頁功能時，FormView 控制項中顯示的頁面巡覽列的樣式設定。 |
| RowStyle | FormView 控制項處於唯讀模式時的資料列樣式設定。 |

## <a name="events"></a>事件

FormView 控制項提供數個事件，您可以進行程式設計。 這可讓您執行自訂的常式，每當事件發生時。 下表列出支援 FormView 控制項的事件。

| **Event** | **描述** |
| --- | --- |
| ItemCommand | 發生於按一下 FormView 控制項內的按鈕。 您需要使用適當的事件提供這項功能。 |
| ItemCreated | FormView 控制項中建立所有 FormViewRow 物件之後，就會發生。 這個事件通常用於顯示之前修改記錄的值。 |
| ItemDeleted | 刪除按鈕時，就會發生 (按鈕，以使用其**CommandName**屬性設定為 「 刪除 」) 按一下時，但之後 FormView 控制項從 資料來源中刪除記錄。 此事件通常用來檢查刪除作業的結果。 |
| ItemDeleting | 發生於按一下 [刪除] 按鈕時，但之前 FormView 控制項，請刪除資料來源的記錄。 此事件通常用來取消刪除作業。 |
| ItemInserted | 發生於 [插入] 按鈕 (按鈕其**CommandName**屬性設定為"Insert") 按一下時，但之後 FormView 控制項插入資料錄。 若要檢查插入作業的結果通常使用這個事件。 |
| ItemInserting | 發生於按一下 [插入] 按鈕時，但之前 FormView 控制項插入資料錄。 此事件通常用來取消插入作業。 |
| ItemUpdated | [更新] 按鈕時，就會發生 (按鈕，以使用其**CommandName**屬性設定為 「 更新 」) 按一下時，但 FormView 控制項更新資料列之後。 此事件通常會用來檢查更新作業的結果中。 |
| ItemUpdating | 發生於按一下 [更新] 按鈕時，但之前 FormView 控制項更新資料錄。 此事件通常會用來取消更新作業中。 |
| ModeChanged | FormView 控制項變更模式之後，就會發生 （若要編輯、 插入或唯讀模式）。 這個事件通常用於 FormView 控制項來變更模式時執行工作。 |
| ModeChanging | 在 FormView 控制項來變更模式之前發生 （若要編輯、 插入或唯讀模式）。 若要取消模式變更通常使用這個事件。 |
| PageIndexChanged | 發生於按一下其中一個頁面巡覽區按鈕時，但之後 FormView 控制項處理分頁作業。 當您需要在使用者瀏覽到控制項中不同的記錄後，執行工作時，通常使用這個事件。 |
| PageIndexChanging | 發生於按一下其中一個頁面巡覽區按鈕時，但之前 FormView 控制項處理分頁作業。 此事件通常用來取消分頁作業。 |

## <a name="detailsview"></a>DetailsView

在 DetailsView 控制項來顯示資料來源的單一記錄在資料表中，其中每個記錄的欄位會顯示資料表的資料列中。 它可以用於搭配 GridView 控制項應用在主從式案例。 在 DetailsView 控制項支援下列功能：

- 在 GridView 控制項中替代資料列樣式設定。
- 插入包含內建的功能。
- 資料來源不包含任何記錄時，在 GridView 控制項中顯示之空白資料列樣式設定。
- GridView 控制項的頁尾資料列樣式設定。
- 以程式設計方式存取 DetailsView 物件模型，以動態方式設定屬性，處理事件，等等。
- 在 GridView 控制項中選取的資料列樣式設定。

## <a name="row-fields"></a>資料列欄位

在 DetailsView 控制項中的每個資料列的建立方式宣告欄位控制項。 不同的資料列的欄位類型決定在控制項中的資料列的行為。 欄位控制項是衍生自 DataControlField。 下表列出可用的不同的資料列欄位型別。

| **下表列出支援 GridView 控制項的事件。** | **描述** |
| --- | --- |
| BoundField | 資料來源中的欄位值顯示為文字中。 |
| ButtonField | 在 DetailsView 控制項中顯示命令按鈕。 這可讓您顯示具有自訂按鈕控制項，例如新增或移除按鈕的資料列。 |
| CheckBoxField | 在 DetailsView 控制項中顯示核取方塊。 這個資料列的欄位型別通常用於顯示的布林值的欄位。 |
| CommandField | 顯示內建的命令按鈕，以執行編輯、 插入或刪除在 DetailsView 控制項中的作業。 |
| HyperLinkField | 這個資料列的欄位型別可讓您建立自訂的資料列欄位。 這個資料列的欄位型別可讓您的第二個欄位繫結至超連結的 URL。 |
| ImageField | 在 DetailsView 控制項中顯示影像。 |
| 為文字，其中每個欄位會出現在資料來源的順序，即會每個欄位顯示在資料列。 | 顯示使用者定義內容根據指定的範本在 DetailsView 控制項中的資料列。 這個資料列的欄位型別可讓您建立自訂的資料列欄位。 |

根據預設，AutoGenerateRows 屬性設為 **，則為 true**，這會自動產生繫結的資料列欄位物件的可繫結類型的每個欄位的資料來源中。 有效的可繫結類型是字串、 DateTime、 Decimal、 Guid 和一組基本型別。 為文字，其中每個欄位會出現在資料來源的順序，即會每個欄位顯示在資料列。

自動產生的資料列提供快速且輕鬆的方式來顯示記錄中的每個欄位。 不過，利用 DetailsView 控制項的進階的功能，您必須明確宣告在 DetailsView 控制項中包含的資料列欄位。 若要宣告的資料列欄位，請先設定**AutoGenerateRows**屬性設**false**。 接下來，新增 開啟和關閉**&lt;欄位&gt;** 之間的開頭和結尾標記 DetailsView 控制項的標記。 最後，列出您想要包含的開頭和結尾之間的資料列欄位**&lt;欄位&gt;** 標記。 指定資料列欄位都會加入至欄位集合中列出的順序。 **欄位**集合可讓您以程式設計方式管理在 DetailsView 控制項中的資料列欄位。

> [!NOTE]
> 自動產生欄位不會新增至欄位集合的資料列。


## <a name="binding-to-data"></a>在 DetailsView 控制項可以繫結至資料來源控制項，例如SqlDataSource或 AccessDataSource，或實作 System.Collections.IEnumerable 介面，例如 System.Data.DataView，任何資料來源System.Collections.ArrayList 和 System.Collections.Hashtable。

在 DetailsView 控制項可以繫結至資料來源控制項，例如**SqlDataSource**或 AccessDataSource，或實作 System.Collections.IEnumerable 介面，例如 System.Data.DataView，任何資料來源System.Collections.ArrayList 和 System.Collections.Hashtable。

請使用下列方法之一的 DetailsView 控制項繫結至適當的資料來源類型：

- 若要繫結至資料來源控制項，設定 DetailsView 控制項中的 [DataSourceID] 屬性的資料來源控制項的識別碼值。 在 DetailsView 控制項自動繫結至指定的資料來源控制項。 此控制項可用來顯示使用者輸入，其中可能包含惡意的用戶端指令碼。
- 若要繫結至資料來源會實作**System.Collections.IEnumerable**介面，以程式設計方式設定為資料來源的 DetailsView 控制項的 DataSource 屬性，然後呼叫 DataBind 方法。

## <a name="security"></a>安全性

此控制項可用來顯示使用者輸入，其中可能包含惡意的用戶端指令碼。 檢查從用戶端可執行的指令碼、 SQL 陳述式，或其他程式碼傳送之前顯示在您的應用程式中的任何資訊。 ASP.NET 提供使用者輸入來封鎖指令碼和 HTML 的輸入的要求驗證功能。

## <a name="data-operations"></a>資料作業

在 DetailsView 控制項提供內建的功能，可讓使用者更新、 刪除、 插入和控制項中的項目頁面上。 在 DetailsView 控制項繫結至資料來源控制項，在 DetailsView 控制項就可以利用資料來源控制項的功能，並提供自動更新、 刪除、 插入和分頁功能。

在 DetailsView 控制項可以提供支援 update、 delete、 insert 和分頁作業，與其他類型的資料來源;不過，您必須提供適當的事件處理常式中的這些作業的實作。

在 DetailsView 控制項可以自動將**CommandField**編輯、 刪除或新增的按鈕，藉由設定 [autogenerateeditbutton]、 [autogeneratedeletebutton] 或 AutoGenerateInsertButton屬性的資料列欄位 **，則為 true**分別。 不同於刪除按鈕 （這會刪除選取的記錄立即），按一下 [編輯] 或 [新增] 按鈕時的 DetailsView 控制項進入編輯或插入模式中，分別。 在編輯模式中，編輯 按鈕會取代的更新 和 取消 按鈕。 使用者修改欄位的值會顯示適用於欄位的資料類型 （例如文字方塊或核取方塊控制項） 的輸入的控制項。 按一下 [更新] 按鈕更新資料來源中的記錄，而按一下 [取消] 按鈕，則會放棄任何變更。 同樣地，在插入模式中，新增 按鈕會取代插入 和 取消 按鈕，並讓使用者輸入新的資料錄的值會顯示空白的輸入的控制項。

在 DetailsView 控制項提供分頁功能，可讓使用者瀏覽至其他資料來源中的記錄。 啟用時，頁面導覽控制項將會顯示在頁面巡覽列中。 若要啟用分頁，請將 AllowPaging 屬性設定為 **，則為 true**。 您可以使用 PagerStyle 和 PagerSettings 屬性自訂的頁面巡覽列。

## <a name="customizing-the-user-interface"></a>自訂使用者介面

您可以設定不同的組件控制項的樣式屬性，來自訂 DetailsView 控制項的外觀。 下表列出不同的樣式屬性。

| **樣式屬性** | **描述** |
| --- | --- |
| AlternatingRowStyle | 在 DetailsView 控制項中替代資料列樣式設定。 當設定這個屬性時，資料列會顯示交替 RowStyle 設定並**AlternatingRowStyle**設定。 |
| CommandRowStyle | 包含內建的命令按鈕，在 DetailsView 控制項中的資料列樣式設定。 |
| EditRowStyle | 在 DetailsView 控制項中時的資料列的樣式設定編輯模式。 |
| EmptyDataRowStyle | 資料來源不包含任何記錄時，在 DetailsView 控制項中顯示之空白資料列樣式設定。 |
| FooterStyle | 在 DetailsView 控制項的頁尾資料列樣式設定。 |
| HeaderStyle | 在 DetailsView 控制項的標頭資料列樣式設定。 |
| InsertRowStyle | 在 DetailsView 控制項中時的資料列的樣式設定插入模式。 |
| PagerStyle | 在 DetailsView 控制項的頁面巡覽區資料列樣式設定。 |
| RowStyle | 在 DetailsView 控制項中的資料列樣式設定。 當**AlternatingRowStyle**也會設定屬性，則資料列會顯示交替**RowStyle**設定和**AlternatingRowStyle**設定。 |
| FieldHeaderStyle | 在 DetailsView 控制項的標頭資料行樣式設定。 |

## <a name="events"></a>事件

在 DetailsView 控制項提供數個事件，您可以進行程式設計。 這可讓您執行自訂的常式，每當事件發生時。 下表列出支援 DetailsView 控制項的事件。 在 DetailsView 控制項也會繼承這些事件及其基底類別： 資料繫結、 資料繫結、 Disposed，Init、 負載、 PreRender，以及轉譯。

| **Event** | **描述** |
| --- | --- |
| ItemCommand | 在 DetailsView 控制項中按一下按鈕時，就會發生。 |
| ItemCreated | 在 DetailsView 控制項中建立所有 DetailsViewRow 物件之後，就會發生。 這個事件通常用於顯示之前修改記錄的值。 |
| ItemDeleted | 發生於按一下 刪除 按鈕時，但之後 DetailsView 控制項從 資料來源中刪除記錄。 此事件通常用來檢查刪除作業的結果。 |
| ItemDeleting | 發生於按一下 [刪除] 按鈕時，但之前 DetailsView 控制項，請刪除資料來源的記錄。 此事件通常用來取消刪除作業。 |
| ItemInserted | 發生於按一下 [插入] 按鈕時，但之後 DetailsView 控制項插入資料錄。 若要檢查插入作業的結果通常使用這個事件。 |
| ItemInserting | 發生於按一下 [插入] 按鈕時，但之前 DetailsView 控制項插入資料錄。 此事件通常用來取消插入作業。 |
| ItemUpdated | 按一下 [更新] 按鈕時，但在 DetailsView 控制項更新資料列之後發生。 此事件通常會用來檢查更新作業的結果中。 |
| ItemUpdating | 發生於按一下 [更新] 按鈕時，但之前 DetailsView 控制項更新資料錄。 此事件通常會用來取消更新作業中。 |
| ModeChanged | 在 DetailsView 控制項變更模式 （編輯、 插入或唯讀模式） 之後，就會發生。 在 DetailsView 控制項變更模式時執行的工作通常使用這個事件。 |
| ModeChanging | 在 DetailsView 控制項來變更 （編輯、 插入或唯讀模式） 的模式之前發生。 若要取消模式變更通常使用這個事件。 |
| PageIndexChanged | 按一下其中一個頁面巡覽區按鈕時，但在 DetailsView 控制項處理分頁作業之後發生。 當您需要在使用者瀏覽到控制項中不同的記錄後，執行工作時，通常使用這個事件。 |
| PageIndexChanging | 發生於按一下其中一個頁面巡覽區按鈕時，但之前 DetailsView 控制項處理分頁作業。 此事件通常用來取消分頁作業。 |

## <a name="the-menu-control"></a>功能表控制項

在 ASP.NET 2.0 中的功能表控制項被設計為全功能的導覽系統。 它可以輕鬆地以階層式資料來源，例如，SiteMapDataSource 的資料繫結。

功能表控制項結構可以宣告方式或以動態方式定義，並且包含單一根節點和任意數目的子節點。 下列程式碼以宣告方式定義功能表控制項的功能表。

[!code-aspx[Main](data-bound-controls/samples/sample4.aspx)]

在上述範例中，Home.aspx 節點是根節點。 所有其他節點被巢狀在各種層級的根節點。

有兩種類型的功能表可呈現功能表控制項;靜態功能表和動態功能表。 靜態功能表是由永遠會顯示的功能表項目所組成。 動態功能表是由才看得見，當滑鼠使用者停留在其上方的功能表項目所組成。 客戶可能經常會混淆，靜態功能表，以宣告方式定義的功能表和功能表，都會在執行階段的繫結的動態功能表。 事實上，動態和靜態功能表無關的母體擴展的方法。 條款*靜態*並*動態*只能參考功能表是否是以靜態方式顯示預設或只有當使用者採取某些動作時顯示。

**StaticDisplayLevels**屬性用來設定多少層的功能表是靜態，因此根據預設顯示。 在上述範例中，設定**StaticDisplayLevels**屬性設為 2 的值會導致要以靜態方式顯示 [首頁] 節點、 [音樂] 節點中和 [影片] 節點的功能表。 使用者將滑鼠停留的父節點時，就會以動態方式顯示所有其他節點。

**MaximumDynamicDisplayLevels**屬性會設定最大的數字可顯示功能表是動態的層級。 在層級高於所指定之值的任何動態功能表**MaximumDynamicDisplayLevels**屬性都會被捨棄。

> [!NOTE]
> 就幾乎可以肯定您可能會遇到不呈現因為 MaximumDynamicDisplayLevels 屬性會出現功能表的情況。 在這些情況下，請確定屬性夠設為允許客戶功能表顯示。


## <a name="data-binding-the-menu-control"></a>資料繫結功能表控制項

功能表控制項可以繫結至 SiteMapDataSource 或 XMLDataSource 等任何階層式資料來源。 SiteMapDataSource 是最常用的方法進行資料繫結至功能表控制項因為它會送出的 Web.sitemap 檔案，而且其結構描述會提供一個已知的 API，可功能表控制項。 下列清單顯示簡單的 Web.sitemap 檔案。

[!code-xml[Main](data-bound-controls/samples/sample5.xml)]

請注意，只有一個根 siteMapNode 項目，在此情況下，首頁項目。 可以針對每個 siteMapNode 設定數個屬性。 最常使用的屬性包括：

- **url**指定當使用者按一下功能表項目時要顯示的 URL。 如果這個屬性不存在，按下時，將只會回傳的節點。
- **標題**指定功能表項目顯示的文字。
- **描述**作為文件節點。 滑鼠停留在節點上時，也會顯示為工具提示。
- **siteMapFile**允許巢狀的網站地圖。 這個屬性必須指向語式正確的 ASP.NET sitemap 檔案。
- **角色**可讓節點的外觀，以控制 ASP.NET 安全性調整。

請注意，雖然這些屬性都是選用項目，功能表的行為可能會不預期是否未指定。 例如，如果*url*屬性已指定但*描述*屬性、 節點將不會顯示並不會有沒有辦法瀏覽至指定的 URL。

## <a name="controlling-a-menus-operation"></a>控制功能表作業

有數個會影響 ASP.NET Menu 控制項之作業的內容**方向**屬性， **DisappearAfter**屬性**StaticItemFormatString**屬性，而**StaticPopoutImageUrl**屬性是幾種。

- **方向**可以設定為*水平*或是*垂直*及控制靜態功能表項目會配置水平資料列中或以垂直方式和堆疊時另一個。 這個屬性不會影響動態功能表。
- **DisappearAfter**屬性會設定多久動態功能表應保持可見之後滑鼠移開。 指定的值是以毫秒為單位，預設值為 500。 將此屬性設定為-1 值會讓功能表永遠不會自動消失。 在此情況下，當使用者按一下功能表之外，就會只消失的功能表。
- **StaticItemFormatString**屬性可讓您輕鬆地在您的功能表系統中維護一致的用語。 指定此屬性，當*{0}* 應該出現在資料來源的描述取代輸入。 例如，為了練習 1 say 瀏覽我們的產品頁面 功能表項目等，您會指定，請造訪我們{0}StaticItemFormatString 的頁面。 在執行階段，ASP.NET 會取代任何出現{0}正確功能表項目的描述。
- **StaticPopoutImageUrl**屬性會指定用來表示特定功能表節點有子節點可存取停留在它的映像。 動態功能表將繼續使用預設的映像。

## <a name="templated-menu-controls"></a>樣板化功能表控制項

功能表控制項是樣板化控制項，並允許兩個不同項目範本;StaticItemTemplate 和 DynamicItemTemplate。 使用這些範本，您可以輕鬆加入伺服器控制項或使用者控制項到您的功能表。

若要編輯的範本在 Visual Studio.NET 中，按一下 [[] 功能表上的智慧標籤] 按鈕，然後選擇編輯範本。 然後您可以選擇編輯 StaticItemTemplate 或 DynamicItemTemplate。

在頁面載入時，會出現靜態功能表中加入 StaticItemTemplate 任何控制項。 所有的快顯功能表上，會出現 DynamicItemTemplate 加入任何控制項。

## <a name="menu-events"></a>功能表事件

功能表控制項有兩個事件所特有，**MenuItemClicked**並**MenuItemDatabound**事件。

按一下功能表項目時，會引發 MenuItemClicked 事件。 當功能表項目是資料繫結時，會引發 MenuItemDatabound 事件。 **MenuEventArgs**傳遞至事件處理常式提供存取權的項目屬性是功能表項目。

## <a name="controlling-a-menus-appearance"></a>控制功能表外觀

您也會影響使用一或多個格式功能表提供許多樣式功能表控制項的外觀。 包括**StaticMenuStyle**， **DynamicMenuStyle**， **DynamicMenuItemStyle**，**想像**，以及**DynamicHoverStyle**。 這些屬性會設定使用標準的 HTML 樣式字串。 例如，下列會影響動態功能表的樣式。

[!code-aspx[Main](data-bound-controls/samples/sample6.aspx)]

> [!NOTE]
> 如果您使用的任何動態顯示樣式，您必須新增&lt;head&gt;頁面中的項目*runat*項目設定為*server*。


功能表控制項也支援使用 ASP.NET 2.0 主題。

## <a name="the-treeview-control"></a>TreeView 控制項

TreeView 控制項中的樹狀結構顯示資料。 如同功能表控制項，它可以輕鬆地繫結至 SiteMapDataSource 等任何階層式資料來源的資料。

客戶有可能會詢問有關在 ASP.NET 2.0 TreeView 控制項的第一個問題是，zda bude 與其相關適用於 ASP.NET TreeView IE WebControl 1.x。 不存在。 ASP.NET 2.0 TreeView 控制項從頭撰寫，並提供 IE TreeView WebControl 先前可用的重大改進。

我將不會詳述如何 Menu 控制項完全相同的方式在執行時，將樹狀檢視控制項繫結至站台對應。 然而，TreeView 控制項有一些顯著的差異，其運作方式。

根據預設，TreeView 控制項顯示為完全展開。 若要變更的初始載入時展開的層級，修改**ExpandDepth**控制項的屬性。 這是在樹狀檢視的資料繫結時展開特定節點的情況下特別重要。

## <a name="databinding-the-treeview-control"></a>資料繫結 TreeView 控制項

不同於功能表控制項，樹狀檢視本身也是用來處理大量的資料。 因此，除了資料繫結至 SiteMapDataSource 或 XMLDataSource，樹狀檢視通常是資料繫結至資料集或其他關聯式資料。 在 TreeView 控制項，繫結至大量資料的情況下，最好是只繫結至會實際顯示在控制項中的資料。 接著，您可以為已展開 TreeView 節點資料繫結至其他資料。

在這些情況下， **PopulateOnDemand**的樹狀檢視的屬性應設為 *，則為 true*。 然後您必須提供實作**TreeNodePopulate**方法。

## <a name="data-binding-without-postback"></a>資料繫結，而不需要回傳

請注意，當您第一次展開一個節點，在上述範例中，頁面回傳，並重新整理。 Thats 不是問題在此範例中，但您可以想像，它可能是在生產環境中使用大量的資料。 更好的案例就是在其中樹狀檢視會仍以動態方式填入它的節點，但不後續回傳至伺服器。

藉由設定**PopulateNodesFromClient**並**PopulateOnDemand**為 true 時，ASP.NET TreeView 控制項的屬性會動態填入節點，而回傳。 父節點展開時，從用戶端進行 XMLHttp 要求，且 OnTreeNodePopulate 引發事件時。 伺服器回應，使用 XML 資料島，然後用以資料繫結的子節點。

ASP.NET 會動態建立實作這項功能的用戶端程式碼。 &lt;指令碼&gt;標籤，其中包含指令碼會產生指向 AXD-ISAPID-2.0-64 檔案。 例如，下列清單會顯示產生 XMLHttp 要求，指令碼的指令碼連結。

[!code-html[Main](data-bound-controls/samples/sample7.html)]

如果您在您的瀏覽器中瀏覽上述的 AXD-ISAPID-2.0-64 檔案，並開啟它，您會看到實作 XMLHttp 要求的程式碼。 這個方法可防止客戶修改指令碼檔案。

## <a name="controlling-the-operation-of-the-treeview-control"></a>控制 TreeView 控制項的作業

TreeView 控制項有數個屬性會影響控制項的作業。 最明顯的屬性**ShowCheckBoxes**， **ShowExpandCollapse**，並**ShowLines**。

**ShowCheckBoxes**屬性會影響是否節點會顯示一個核取方塊，當呈現。 有效的值，這個屬性為**無**，**根**，**父**，**分葉**，以及**所有**。 這些變更會影響 [TreeView] 控制項，如下所示：

| **屬性值** | **Effect** |
| --- | --- |
| 無 | 核取方塊不會顯示在任何節點上。 這是預設設定。 |
| 根目錄 | 核取方塊只會顯示在根節點上。 |
| 父代 | 核取方塊只會顯示在這些子節點的節點上。 這些子節點可以是父節點或分葉節點。 |
| 分葉 | 沒有子節點的節點只會顯示一個核取方塊。 |
| 全部 | 核取方塊會顯示在所有節點上。 |

當使用核取方塊時， **CheckedNodes**屬性會傳回在回傳時簽入的樹狀檢視節點的集合。

**ShowExpandCollapse**屬性控制的根節點和父節點旁邊的展開/摺疊影像的外觀。 如果這個屬性設定為**false**，TreeView 節點會呈現為超連結，並會依序按一下連結的 展開/摺疊。

**ShowLines**屬性控制是否要顯示線連接至子節點的父節點。 當**false** （預設值），會顯示任何線條。 當**真**，TreeView 控制項將會使用行映像中所指定的資料夾**LineImagesFolder**屬性。

若要自訂的 TreeView 線條的外觀，Visual Studio.NET 2005年會包含行設計師工具。 您可以存取這項工具使用 TreeView 控制項做為下面上智慧標籤 按鈕。


![](data-bound-controls/_static/image1.jpg)

**圖 1**


當選取**來自訂線條影像**功能表選項，將啟動列 Designer 工具，可讓您設定的 TreeView 線條的外觀。

## <a name="treeview-events"></a>TreeView 事件

TreeView 控制項具有下列的唯一事件：

- 選取節點時，就會發生 SelectedNodeChanged 根據**SelectAction**屬性。
- 變更節點 checkboxs 狀態時，就會發生 TreeNodeCheckChanged。
- 展開節點時，就會發生 TreeNodeExpanded 根據**SelectAction**屬性。
- 摺疊節點時，就會發生 TreeNodeCollapsed。
- 資料節點時，就會發生 TreeNodeDataBound 繫結。
- 填入節點時，就會發生 TreeNodePopulate。

**SelectAction**屬性可讓您設定 選取節點時引發的事件。 SelectAction 屬性會提供下列動作：

- TreeNodeSelectAction.Expand 引發 TreeNodeExpanded 選取節點時。
- TreeNodeSelectAction.None 引發任何事件時選取的節點。
- TreeNodeSelectAction.Select 引發 SelectedNodeChanged 有更多的事件時選取的節點。
- SelectedNodeChanged 事件和 TreeNodeExpanded 事件選取節點時，會引發 TreeNodeSelectAction.SelectExpand。

## <a name="controlling-appearance-with-styles"></a>控制具有樣式的外觀

TreeView 控制項提供許多屬性，來控制具有樣式控制項的外觀。 有下列可用屬性。

| **屬性名稱** | **控制項** |
| --- | --- |
| HoverNodeStyle | 當滑鼠停留在其上時，控制節點的樣式。 |
| 示 | 控制的分葉節點的樣式。 |
| 有 | 控制所有節點的樣式。 特定的節點樣式 （例如示） 會覆寫此樣式。 |
| ParentNodeStyle | 控制所有的父節點的樣式。 |
| RootNodeStyle | 控制的根節點的樣式。 |
| SelectedNodeStyle | 控制所選節點的樣式。 |

每個屬性是唯讀。 不過，它們將會每個傳回**這些影像會呈現**物件，該物件的內容可以修改和使用*屬性子屬性*格式。 例如，若要設定**ForeColor**屬性**SelectedNodeStyle**，您可以使用下列語法：

[!code-aspx[Main](data-bound-controls/samples/sample8.aspx)]

請注意上述的標記不會關閉。 這是因為使用如下所示的宣告式語法時，您就會在 HTML 程式碼中包含的 Treeview 節點。

也可以在程式碼中使用指定的樣式屬性*property.subproperty*格式。 例如，若要設定**ForeColor**屬性**文字左邊**在程式碼中，您會使用下列語法：

[!code-csharp[Main](data-bound-controls/samples/sample9.cs)]

> [!NOTE]
> 針對不同的樣式屬性的完整清單，請參閱這些影像會呈現物件上的 MSDN 文件。


## <a name="the-sitemappath-control"></a>SiteMapPath 控制項

SiteMapPath 控制項為 ASP.NET 開發人員提供階層連結巡覽控制項。 如同其他導覽控制項，它可以輕鬆地資料繫結至階層式資料來源，例如 XmlDataSource 或 SiteMapDataSource。

SiteMapPath 控制項組成 SiteMapNodeItem 物件。 有三種類型的節點;根節點、 父節點和目前的節點。 根節點是在階層式結構頂端的節點。 目前的節點代表目前的頁面。 所有其他節點是父節點。

## <a name="controlling-the-operation-of-the-sitemappath-control"></a>控制 SiteMapPath 控制項的作業

控制作業 SiteMapPath 控制項的屬性如下所示：

| **Property** | **屬性的描述** |
| --- | --- |
| ParentLevelsDisplayed | 控制要顯示多少個父節點。 預設值為-1，不會限制顯示的父節點的數目。 |
| PathDirection | 控制 SiteMapPath 的方向。 有效的值為 RootToCurrent （預設值） 和 CurrentToRoot。 |
| PathSeparator | 控制區隔 SiteMapPath 控制項中的節點的字元字串。 預設值是:。 |
| RenderCurrentNodeAsLink | 布林值控制將目前的節點轉譯為連結。 預設值為 False。 |
| SkipLinkText | 使用頁面檢視螢幕助讀程式時的協助工具的協助。 這個屬性可讓螢幕助讀員略過 SiteMapPath 控制項。 若要停用這項功能，請將屬性設定為 String.Empty。 |

## <a name="templated-sitemappath-controls"></a>樣板化 SiteMapPath 控制項

SiteMapControl 是樣板化控制項，而且此情況下，您可以定義不同的範本，以用於顯示控制項。 若要編輯 SiteMapPath 控制項中的範本，請按一下智慧標籤按鈕控制項上的並從功能表中選擇 編輯範本。 這會顯示 SiteMapTasks 功能表，您可以在其中選擇不同的範本，如下所示。


![](data-bound-controls/_static/image2.jpg)

**圖 2**


**NodeTemplate**範本是指 SiteMapPath 中的任何節點。 如果節點為根節點或目前的節點，且**RootNodeTemplate**或是**CurrentNodeTemplate**是設定，NodeTemplate 會覆寫。

## <a name="sitemappath-events"></a>SiteMapPath 事件

SiteMapPath 控制項有兩個事件不是衍生自 Control 類別;**ItemCreated**事件並**ItemDataBound**事件。 SiteMapPath 項目建立時，會引發 ItemCreated 事件。 SiteMapPath 節點的資料繫結期間呼叫 DataBind 方法時，就會引發 ItemDataBound。 A **SiteMapNodeItemEventArgs**物件可讓您存取特定的 SiteMapNodeItem 透過項目屬性。

## <a name="controlling-appearance-with-styles"></a>控制具有樣式的外觀

下列樣式可供 SiteMapPath 控制項的格式。

| **屬性名稱** | **控制項** |
| --- | --- |
| CurrentNodeStyle | 控制項目前節點的文字的樣式。 |
| RootNodeStyle | 控制根節點的文字的樣式。 |
| 有 | 控制假設 CurrentNodeStyle 或文字左邊不適用的所有節點的文字的樣式。 |

有屬性，會覆寫 CurrentNodeStyle 或文字左邊。 每個屬性是唯讀的並傳回**樣式**物件。 若要影響使用這些屬性的其中一個節點的外觀，您必須設定傳回樣式物件的屬性。 例如，下列程式碼會變更目前節點的前景色彩屬性。

[!code-aspx[Main](data-bound-controls/samples/sample10.aspx)]

屬性也可以套用以程式設計方式，如下所示：

[!code-csharp[Main](data-bound-controls/samples/sample11.cs)]

> [!NOTE]
> 如果已套用範本，將不會套用的樣式。


## <a name="lab-1-configuring-an-aspnet-menu-control"></a>實驗室 1： 設定 ASP.NET Menu 控制項

1. 建立新的網站。
2. 選取 檔案 | 新增 | 檔案，然後從檔案範本清單中選擇站台對應加入網站地圖檔案。
3. 開啟網站地圖 (預設的 Web.sitemap)，並修改它，使它看起來像下列的清單。 實際上不存在連結的站台對應檔案中的頁面，但是，不會針對此練習的問題。

    [!code-xml[Main](data-bound-controls/samples/sample12.xml)]
4. 在 [設計] 檢視中開啟預設的 Web 表單。
5. 從 [工具箱] 的 [瀏覽] 區段中，請將新的功能表控制項加入頁面。
6. 從 [工具箱] 的 [資料] 區段中，加入新的 Treeview。 在您的網站，SiteMapDataSource 會自動使用的 Web.sitemap 檔案。 (Web.sitemap 檔案*必須*網站的根資料夾中。)
7. 按一下功能表控制項，然後按一下 [智慧標籤] 按鈕，以顯示功能表工作對話方塊。
8. 在 [選擇資料來源] 下拉式清單中，選取 [SiteMapDataSource1]。
9. 按一下 [表格自動格式設定] 連結，功能表中選擇的格式。
10. 在 [屬性] 窗格中，設定**StaticDisplayLevels**屬性設為 2。 功能表控制項現在應該在設計工具中顯示的首頁、 產品和服務的節點。
11. 瀏覽您的瀏覽器使用的功能表中的頁面。 （因為您已新增至站台對應的網頁實際上不存在，您會看到錯誤當您嘗試，並瀏覽至它們。）

體驗變更 StaticDisplayLevels MaximumDynamicDisplayLevels 屬性，並查看它們如何影響功能表的呈現方式。

## <a name="lab-2-dynamically-binding-a-treeview-control"></a>實驗室 2： 動態地繫結 TreeView 控制項

本練習假設您已在本機執行的 SQL Server 和 Northwind 資料庫位於 SQL Server 執行個體。 如果不符合這些條件，請變更此範例中的連接字串。 請注意，您可能也需要指定 「 SQL Server 驗證，而不是信任連接。

1. 建立新的網站。
2. 切換到 Default.aspx 的程式碼檢視，並使用下面所列的程式碼取代所有程式碼。 

    [!code-aspx[Main](data-bound-controls/samples/sample13.aspx)]
3. 將頁面儲存為 treeview.aspx 中。
4. 瀏覽的頁面。
5. 第一次顯示網頁時，您的瀏覽器中檢視頁面的來源。 請注意，只有可見的節點已傳送至用戶端。
6. 按一下任何節點旁邊的加號。
7. 在頁面上檢視原始檔。 請注意顯示新的節點現在都會出現。

## <a name="lab-3-details-view-and-editing-data-using-a-gridview-and-detailsview"></a>實驗室 3： 詳細資料檢視和編輯使用 GridView 和 DetailsView 的資料

1. 建立新的網站。
2. 網站中加入新的 web.config。
3. 將連接字串加入 web.config 檔案，如下所示： 

    [!code-xml[Main](data-bound-controls/samples/sample14.xml)]

    > [!NOTE]
    > 您可能需要變更連接字串，根據您的環境。
4. 儲存並關閉 web.config 檔案。
5. 開啟 Default.aspx，並加入新的 SqlDataSource 控制項。
6. 變更 SqlDataSource 控制項的 ID**產品**。
7. 在  **SqlDataSource 工作**功能表上，按一下**設定資料來源**。
8. 選取  **Northwind**連接下拉式清單中，按一下 下一步。
9. 選取 **產品**從**名稱**下拉式清單中，並檢查**ProductID**， **ProductName**， **UnitPrice**，並**UnitsInStock**核取方塊，如下所示。 

![](data-bound-controls/_static/image3.jpg)

    **Figure 3**
10. 按 [ **下一步**]。
11. 按一下 [ **完成**]。
12. 切換到來源檢視，並檢查所產生的程式碼。 請注意**SelectCommand**， **DeleteCommand**， **InsertCommand**，以及**UpdateCommand** ，已新增至 SqlDataSource控制項。 也請注意已新增的參數。
13. 切換至 [設計] 檢視，並將新的 GridView 控制項新增至頁面。
14. 選取 **產品**從**選擇資料來源**下拉式清單。
15. 請檢查**啟用分頁**並**啟用選取範圍**，如下所示。 

![](data-bound-controls/_static/image4.jpg)

    **Figure 4**
16. 按一下 **編輯資料行**連結，並確定**自動產生欄位**已核取。
17. 按一下 [確定 **Deploying Office Solutions**]。
18. 選取的 GridView 控制項，按一下  按鈕旁**DataKeyNames**屬性 窗格中的屬性。
19. 選取  **ProductID**從**可用的資料欄位**清單，然後按一下**&gt;** 按鈕以新增它。
20. 按一下 [確定]。
21. 新的 SqlDataSource 控制項加入網頁中。
22. 變更 SqlDataSource 控制項的 ID**詳細資料**。
23. 從 SqlDataSource 工作 功能表中，選擇**設定資料來源**。
24. 選擇**Northwind**從下拉式清單中，然後按一下**下一步**。
25. 選取 <strong>產品</strong>從<strong>名稱</strong>下拉式清單中，並檢查<strong> \</ s t > * 中的核取方塊<strong>資料行</strong>listbox。
26. 按一下 [**其中**] 按鈕。
27. 選取  **ProductID**從**資料行**下拉式清單。
28. 選取  **=** 運算子下拉式清單中。
29. 選取 **控制**從**來源**下拉式清單。
30. 選取  **GridView1**從**控制項 ID**下拉式清單。
31. 按一下 **新增**按鈕以新增 WHERE 子句。
32. 按一下 [確定 **Deploying Office Solutions**]。
33. 按一下 **進階**按鈕，然後檢查**產生 INSERT、 UPDATE 和 DELETE 陳述式**核取方塊。
34. 按一下 [確定 **Deploying Office Solutions**]。
35. 按一下 **下一步**然後按一下**完成**。
36. DetailsView 控制項加入頁面。
37. 在 **選擇資料來源**下拉式清單中，選擇**詳細資料**。
38. 請檢查**啟用編輯**核取方塊，如下所示。 

![](data-bound-controls/_static/image1.gif)

    **Figure 5**
39. 儲存頁面，並瀏覽 Default.aspx。
40. 按一下 **選取**即可自動看到 DetailsView 更新的不同記錄旁的連結。
41. 按一下 **編輯**DetailsView 控制項中的連結。
42. 變更的記錄，然後按一下**更新**。
