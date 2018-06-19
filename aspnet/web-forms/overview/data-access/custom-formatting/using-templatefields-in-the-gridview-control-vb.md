---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-vb
title: 在 GridView 控制項 (VB) 中使用 TemplateFields |Microsoft 文件
author: rick-anderson
description: 若要提供彈性，GridView 提供 TemplateField，呈現 使用範本。 範本可以包含靜態的 HTML、 Web 控制項的混合和...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: a92cd6ed-609a-4e40-ad23-004b54afd436
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-vb
msc.type: authoredcontent
ms.openlocfilehash: f236c1cfaaeaa00f30b6a90553ad4e468e05ca23
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876900"
---
<a name="using-templatefields-in-the-gridview-control-vb"></a>在 GridView 控制項 (VB) 中使用 TemplateFields
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_12_VB.exe)或[下載 PDF](using-templatefields-in-the-gridview-control-vb/_static/datatutorial12vb1.pdf)

> 若要提供彈性，GridView 提供 TemplateField，呈現 使用範本。 範本可以包含混合的靜態的 HTML 網頁的控制項和資料繫結語法。 在本教學課程中，我們將檢驗如何使用 TemplateField 以達到更大的 GridView 控制項的自訂。


## <a name="introduction"></a>簡介

在 GridView 組成的一組欄位，以指出哪些屬性從`DataSource`要包含在轉譯的輸出，以及資料顯示方式。 最簡單的欄位型別是 BoundField，後者會顯示為文字的資料值。 其他欄位的類型顯示的資料，使用替代的 HTML 項目。 CheckBoxField，比方說，會轉譯成指定的資料欄位; 的值取決於其核取的狀態的核取方塊ImageField 呈現的影像的影像來源為基礎的指定的資料欄位。 轉譯超連結和按鈕的狀態取決於基礎的資料欄位值，可以使用 HyperLinkField 和 ButtonField 欄位類型。

儘管 CheckBoxField、 ImageField、 HyperLinkField 和 ButtonField 欄位型別，允許替代檢視表的資料，它們仍是相當有限相對於格式設定。 CheckBoxField 只能顯示單一的核取方塊，而 ImageField 只能顯示單一映像。 特定欄位要顯示的文字，核取方塊，該怎麼辦*和*映像，所有根據不同的資料欄位值嗎？ 或者，如果我們想要顯示的資料，請使用核取方塊、 影像、 超連結或按鈕以外的 Web 控制項？ 此外，BoundField 限制其顯示單一資料欄位。 如果我們想要在單一的 GridView 資料行中顯示兩個或多個資料欄位值嗎？

為了容納此層級的彈性 GridView 提供 TemplateField，使用轉譯*範本*。 範本可以包含混合的靜態的 HTML 網頁的控制項和資料繫結語法。 此外，TemplateField 有各式各樣的範本可以用來自訂轉譯，針對不同的情況。 例如，`ItemTemplate`預設用來呈現的資料格的每個資料列，但`EditItemTemplate`範本可以用來編輯資料時，自訂介面。

在本教學課程中，我們將檢驗如何使用 TemplateField 以達到更大的 GridView 控制項的自訂。 在[前述教學課程](custom-formatting-based-upon-data-vb.md)我們可了解如何自訂根據基礎的資料使用的格式設定`DataBound`和`RowDataBound`事件處理常式。 另一種方式來自訂格式的基礎資料為基礎，就藉由呼叫格式化範本內的方法。 在本教學課程，我們會探討這項技術。

在此教學課程中，我們將使用 TemplateFields 訂員工清單。 具體來說，我們將會列出所有員工，但會顯示員工的一個資料行、 雇用日期的日曆控制項，以及指出多少天後他們已採用在公司的狀態資料行中的第一個和最後一個名稱。


[![三個 TemplateFields 可用來自訂顯示](using-templatefields-in-the-gridview-control-vb/_static/image2.png)](using-templatefields-in-the-gridview-control-vb/_static/image1.png)

**圖 1**： 三個 TemplateFields 可用來自訂顯示 ([按一下以檢視完整大小的影像](using-templatefields-in-the-gridview-control-vb/_static/image3.png))


## <a name="step-1-binding-the-data-to-the-gridview"></a>步驟 1: 將資料繫結至 GridView

在報表中您要使用自訂的外觀 TemplateFields 的案例，我簡便的方法藉由建立 GridView 控制項，其中包含只 BoundFields 先啟動然後再新增新 TemplateFields 或轉換至現有的 BoundFields視 TemplateFields。 因此，讓我們開始本教學課程頁面透過設計工具中加入 GridView 和繫結至 ObjectDataSource 傳回員工的清單。 這些步驟將每個員工欄位，以建立與 BoundFields GridView。

開啟`GridViewTemplateField.aspx`頁面上，並從 [工具箱] 拖曳至設計工具拖曳 GridView。 從 GridView 的智慧標籤中，選擇要加入新的 ObjectDataSource 控制項叫用`EmployeesBLL`類別的`GetEmployees()`方法。


[![加入新的 ObjectDataSource 控制項叫用 GetEmployees() 方法](using-templatefields-in-the-gridview-control-vb/_static/image5.png)](using-templatefields-in-the-gridview-control-vb/_static/image4.png)

**圖 2**: ObjectDataSource 控制項新增該 Invokes`GetEmployees()`方法 ([按一下以檢視完整大小的影像](using-templatefields-in-the-gridview-control-vb/_static/image6.png))


這種方式中的繫結的 GridView 會自動將新增 BoundField 的每一個員工屬性： `EmployeeID`， `LastName`， `FirstName`， `Title`， `HireDate`， `ReportsTo`，和`Country`。 此報表讓我們費心思顯示`EmployeeID`， `ReportsTo`，或`Country`屬性。 若要移除這些 BoundFields 中，您可以：

- 使用欄位對話方塊中，按一下從 GridView 的智慧標籤的 [編輯資料行] 連結，以顯示此對話方塊。 接下來，選取從左下方 BoundFields 清單，然後按一下紅色的 X 按鈕移除 BoundField。
- 從來源檢視，以手動方式編輯 GridView 的宣告式語法，請刪除`<asp:BoundField>`BoundField 您想要移除的項目。

移除之後`EmployeeID`， `ReportsTo`，和`Country`BoundFields，您的 GridView 標記應該看起來：


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample1.aspx)]

請花一點時間瀏覽器中檢視進度。 此時應該會看到與記錄資料表每位員工和四個資料行： 一個用於員工的姓氏第一個名稱、 其標題，其中一個，一個其雇用日期。


[![顯示每個員工的姓氏、 名字、 標題和 HireDate 欄位](using-templatefields-in-the-gridview-control-vb/_static/image8.png)](using-templatefields-in-the-gridview-control-vb/_static/image7.png)

**圖 3**: `LastName`， `FirstName`， `Title`，和`HireDate`欄位會顯示每個員工 ([按一下以檢視完整大小的影像](using-templatefields-in-the-gridview-control-vb/_static/image9.png))


## <a name="step-2-displaying-the-first-and-last-names-in-a-single-column"></a>步驟 2： 顯示單一資料行中的第一個和最後一個名稱

目前，每位員工的名字和姓氏會顯示在個別的資料行。 可能是很好，改為將它們結合成單一資料行。 若要完成這項作業，我們需要使用 TemplateField。 我們可以是加入新 TemplateField、 加入所需的標記和資料繫結的語法，然後再刪除`FirstName`和`LastName`BoundFields，或我們可以將轉換`FirstName`BoundField 到 TemplateField，編輯要包含 TemplateField`LastName`值，然後再移除`LastName`BoundField。

這兩種方法 net 相同的結果，但我喜歡 BoundFields 轉為 TemplateFields 可能的話，因為轉換會自動新增的個人`ItemTemplate`和`EditItemTemplate`Web 控制項與資料繫結語法來模仿外觀和 BoundField 的功能。 好處是，我們需要進行較少工作與 TemplateField 轉換執行此程序將會有幫助我們。

若要轉換為 TemplateField 現有 BoundField，請按一下編輯資料行中的連結 GridView 的智慧標籤，顯示欄位 對話方塊。 選取要轉換從左下角中的清單，然後按一下右下角的"轉換為 TemplateField 到這個欄位 」 連結 BoundField。


[![BoundField 轉換為 TemplateField 從 [欄位] 對話方塊](using-templatefields-in-the-gridview-control-vb/_static/image11.png)](using-templatefields-in-the-gridview-control-vb/_static/image10.png)

**圖 4**： 從 [欄位] 對話方塊轉換 BoundField 到 TemplateField ([按一下以檢視完整大小的影像](using-templatefields-in-the-gridview-control-vb/_static/image12.png))


請繼續進行，並將轉換`FirstName`BoundField TemplateField 到。 這項變更後沒有任何 perceptive 的差異，在設計工具中。 這是因為 BoundField 轉換為 TemplateField 建立為 TemplateField 維護 BoundField 外觀與風格。 儘管那里在任何 visual 差異在此時設計工具中的，此轉換程序已取代宣告式語法 BoundField- `<asp:BoundField DataField="FirstName" HeaderText="FirstName" SortExpression="FirstName" />` -含有下列 TemplateField 語法：


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample2.aspx)]

如您所見，TemplateField 所組成的兩個範本`ItemTemplate`具有標籤其`Text`屬性設定的值為`FirstName`資料欄位和`EditItemTemplate`與文字方塊控制項`Text`屬性也設定若要`FirstName`資料欄位。 資料繫結語法- `<%# Bind("fieldName") %>` -指出資料欄位*`fieldName`* 繫結至指定的 Web 控制項屬性。

若要加入`LastName`資料欄位值，我們需要加入另一個標籤 Web 控制項，在此 TemplateField`ItemTemplate`並繫結其`Text`屬性`LastName`。 這可以手動或透過設計工具完成。 若要以手動方式執行，只要加入至適當的宣告式語法`ItemTemplate`:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample3.aspx)]

若要將它加入透過設計工具中，按一下從 GridView 的智慧標籤的 [編輯樣板] 連結。 這會顯示在 GridView 的範本編輯介面。 在此介面的智慧標籤是一份 GridView 中的範本。 因為只有一個 TemplateField 此時，只有列在下拉式清單中的範本有這些範本`FirstName`連同 TemplateField`EmptyDataTemplate`和`PagerTemplate`。 `EmptyDataTemplate`範本時，如果指定，用來呈現 GridView 輸出是否有任何結果中的資料繫結至 GridView; `PagerTemplate`，如果指定，用來呈現分頁介面的支援分頁的 GridView。


[![可以透過設計工具編輯 GridView 的範本](using-templatefields-in-the-gridview-control-vb/_static/image14.png)](using-templatefields-in-the-gridview-control-vb/_static/image13.png)

**圖 5**: GridView 範本可以會編輯透過設計工具 ([按一下以檢視完整大小的影像](using-templatefields-in-the-gridview-control-vb/_static/image15.png))


若也要顯示`LastName`中`FirstName`TemplateField 拖曳 Label 控制項從工具箱拖曳到`FirstName`TemplateField 的`ItemTemplate`在 GridView 的樣板編輯介面。


[![FirstName TemplateField 的 ItemTemplate 中新增標籤 Web 控制項](using-templatefields-in-the-gridview-control-vb/_static/image17.png)](using-templatefields-in-the-gridview-control-vb/_static/image16.png)

**圖 6**： 將標籤 Web 控制項加入`FirstName`TemplateField 的 ItemTemplate ([按一下以檢視完整大小的影像](using-templatefields-in-the-gridview-control-vb/_static/image18.png))


此時加入 TemplateField 標籤 Web 控制項有其`Text`屬性設定為 「 標籤 」。 我們需要變更這讓此屬性的值繫結`LastName`資料改為欄位。 若要完成此標籤控制項的智慧標籤上按一下，然後選擇 [編輯資料繫結] 選項。


[![選擇 編輯資料繫結從標籤的智慧標籤](using-templatefields-in-the-gridview-control-vb/_static/image20.png)](using-templatefields-in-the-gridview-control-vb/_static/image19.png)

**圖 7**： 選擇 編輯資料繫結選項，從標籤的智慧標籤 ([按一下以檢視完整大小的影像](using-templatefields-in-the-gridview-control-vb/_static/image21.png))


這會顯示資料繫結 對話方塊。 從這裡您可以選取要參與資料繫結，從左側清單，並選擇資料繫結至從下拉式清單右側欄位的屬性。 選擇`Text`從左邊的屬性和`LastName`從右側欄位，然後按一下 [確定]。


[![文字屬性繫結到 LastName 資料欄位](using-templatefields-in-the-gridview-control-vb/_static/image23.png)](using-templatefields-in-the-gridview-control-vb/_static/image22.png)

**圖 8**： 繫結`Text`屬性`LastName`資料欄位 ([按一下以檢視完整大小的影像](using-templatefields-in-the-gridview-control-vb/_static/image24.png))


> [!NOTE]
> 資料繫結 對話方塊可讓您指出是否要執行雙向資料繫結。 如果將此選項保留未選取，資料繫結語法`<%# Eval("LastName")%>`將而不是使用`<%# Bind("LastName")%>`。 此教學課程中，兩種方法是正常的。 雙向資料繫結變得插入和編輯資料時很重要的。 只顯示資料，不過，兩種方法將同樣適用。 在未來教學課程中，我們將討論雙向資料繫結，在詳細資料。


請花一點時間來檢視此頁面，透過瀏覽器。 如您所見，GridView 仍然會包含四個資料行。不過，`FirstName`資料行現在會列出*兩者*`FirstName`和`LastName`資料欄位值。


[![FirstName 和 LastName 值顯示單一資料行](using-templatefields-in-the-gridview-control-vb/_static/image26.png)](using-templatefields-in-the-gridview-control-vb/_static/image25.png)

**圖 9**： 同時`FirstName`和`LastName`值會顯示單一資料行中 ([按一下以檢視完整大小的影像](using-templatefields-in-the-gridview-control-vb/_static/image27.png))


若要完成此第一個步驟，移除`LastName`BoundField 並重新命名`FirstName`TemplateField 的`HeaderText`「 名稱 」 的屬性。 在這些變更之後 GridView 的宣告式標記看起來應該如下所示：


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample4.aspx)]


[![每位員工的第一個和最後一個名稱會顯示在一個資料行](using-templatefields-in-the-gridview-control-vb/_static/image29.png)](using-templatefields-in-the-gridview-control-vb/_static/image28.png)

**圖 10**： 每個員工的第一個和最後一個的名稱會顯示一個資料行中 ([按一下以檢視完整大小的影像](using-templatefields-in-the-gridview-control-vb/_static/image30.png))


## <a name="step-3-using-the-calendar-control-to-display-thehireddatefield"></a>步驟 3： 使用行事曆控制項來顯示`HiredDate`欄位

在 GridView 中以文字顯示資料欄位值是使用 BoundField 一樣簡單。 某些情況下，不過，資料是最適合表示使用特定的 Web 控制項，而非只是文字。 這類自訂資料是顯示的 TemplateFields 可行的。 例如，而不是比： 員工的雇用日期顯示為文字，我們無法顯示行事曆 (使用[月曆控制項](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar(VS.80).aspx)) 具有其雇用日期反白顯示。

若要完成這項作業，一開始轉換`HiredDate`BoundField TemplateField 到。 只需前往 GridView 的智慧標籤，然後按一下 編輯資料行連結，顯示欄位 對話方塊。 選取`HiredDate`BoundField 並按一下 」 這個欄位轉換為 TemplateField。 」


[![HiredDate BoundField 轉換為 TemplateField](using-templatefields-in-the-gridview-control-vb/_static/image32.png)](using-templatefields-in-the-gridview-control-vb/_static/image31.png)

**圖 11**： 轉換`HiredDate`TemplateField 到 BoundField ([按一下以檢視完整大小的影像](using-templatefields-in-the-gridview-control-vb/_static/image33.png))


如我們所見在步驟 2 中，這將會取代 BoundField 具有為 TemplateField 包含`ItemTemplate`和`EditItemTemplate`文字方塊中的標籤與其`Text`屬性繫結至`HiredDate`值使用的資料繫結語法`<%# Bind("HiredDate")%>`.

若要取代月曆控制項中的文字，請移除標籤，並新增日曆控制項編輯該範本。 從設計工具中，從 GridView 的智慧標籤選取 編輯範本，然後選擇  `HireDate` TemplateField 的`ItemTemplate`從下拉式清單。 接著，刪除 Label 控制項，並將月曆控制項從 [工具箱] 拖曳到 [編輯範本] 介面。


[![將月曆控制項加入 HireDate TemplateField 的 ItemTemplate](using-templatefields-in-the-gridview-control-vb/_static/image35.png)](using-templatefields-in-the-gridview-control-vb/_static/image34.png)

**圖 12**： 將行事曆控制項加入`HireDate`TemplateField 的`ItemTemplate`([按一下以檢視完整大小的影像](using-templatefields-in-the-gridview-control-vb/_static/image36.png))


此時在 GridView 中的每個資料列會包含日曆控制項中的其`HiredDate`TemplateField。 不過，員工的實際`HiredDate`值不設定中的任何行事曆控制，造成每個行事曆控制項，顯示目前的月份和日期的預設值。 若要補救這種情況，我們必須指派每位員工的`HiredDate`月曆控制項[SelectedDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selecteddate(VS.80).aspx)和[VisibleDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.visibledate(VS.80).aspx)屬性。

從日曆控制項的智慧標籤上，選擇 編輯資料繫結。 接下來，繫結兩者`SelectedDate`和`VisibleDate`屬性`HiredDate`資料欄位。


[![SelectedDate 和 VisibleDate 屬性繫結至 HiredDate 資料欄位](using-templatefields-in-the-gridview-control-vb/_static/image38.png)](using-templatefields-in-the-gridview-control-vb/_static/image37.png)

**圖 13**： 繫結`SelectedDate`和`VisibleDate`屬性`HiredDate`資料欄位 ([按一下以檢視完整大小的影像](using-templatefields-in-the-gridview-control-vb/_static/image39.png))


> [!NOTE]
> 月曆控制項選取的日期不一定需要可見。 例如，行事曆可能年 8 月 1<sup>st</sup>，1999 設為選取的日期，但會顯示在目前的月份和年份。 行事曆控制項指定選取的日期和可見日期`SelectedDate`和`VisibleDate`屬性。 因為我們想要這兩個選取的員工`HiredDate`，並確定它會顯示我們需要繫結這兩個屬性到`HireDate`資料欄位。


當瀏覽器中檢視網頁、 行事曆現在顯示員工的進日期的月份，並選取該特定的日期。


[![月曆控制項中顯示員工的 HiredDate](using-templatefields-in-the-gridview-control-vb/_static/image41.png)](using-templatefields-in-the-gridview-control-vb/_static/image40.png)

**圖 14**: 員工`HiredDate`日曆控制項中顯示 ([按一下以檢視完整大小的影像](using-templatefields-in-the-gridview-control-vb/_static/image42.png))


> [!NOTE]
> Fci 的所有範例為止，我們已看到，本教學課程中我們*不*設定`EnableViewState`屬性`False`此 gridview。 這項決策的原因是因為按一下日期的行事曆控制，導致回傳，行事曆選取的日期設定為直接按一下日期。 在 GridView 的檢視狀態已停用，不過，在每次回傳 GridView 的資料重新繫結至其基礎資料來源，這會導致的行事曆選取的日期設定*回*位員工的`HireDate`、 覆寫使用者選擇日期。


此教學課程中這是無意義的討論因為使用者不是能夠更新員工的`HireDate`。 它可能會設定月曆控制項，讓其日期不能選取最佳。 不論如何，此教學課程會示範在某些情況下檢視狀態必須啟用才能提供特定功能。

## <a name="step-4-showing-the-number-of-days-the-employee-has-worked-for-the-company"></a>步驟 4： 顯示員工的天數，曾公司

到目前為止，我們已經看到 TemplateFields 的兩個應用程式：

- 將兩個或多個資料欄位值結合成一個資料行，以及
- 表示資料欄位值，而非文字使用 Web 控制項

TemplateFields 第三個使用中顯示有關 GridView 的中繼資料為基礎的資料。 除了顯示員工的雇用日期，比方說，我們可能也要有顯示多少就已經在工作的總天數的資料行。

尚未 TemplateFields 另一個用途，就會發生案例中需要有不同報表中顯示網頁比格式儲存在資料庫中基礎資料時。 想像`Employees`資料表具有`Gender`儲存字元的欄位`M`或`F`指出員工的性別。 當我們可能會想要為"Male"或"Female"，而非只"M"或"F"顯示性別的網頁中顯示這項資訊。

藉由建立可處理這兩種案例*格式方法*ASP.NET 網頁的程式碼後置類別中 (或在個別的類別庫中，實作為`Shared`方法)，會叫用的範本。 這種格式化方法會叫用從範本使用先前看過的資料繫結語法相同。 格式化的方法可以接受任意數目的參數，但必須傳回的字串。 這個傳回的字串會插入至範本的 HTML。

為了說明這個概念，讓我們來加強我們的教學課程顯示列出員工已在工作總天數的資料行。 此格式的方法會將`Northwind.EmployeesRow`物件，並傳回的員工具有已採用字串形式的天數。 這個方法可以加入至 ASP.NET 網頁的程式碼後置類別，但*必須*標示為`Protected`或`Public`才能存取的範本。


[!code-vb[Main](using-templatefields-in-the-gridview-control-vb/samples/sample5.vb)]

因為`HiredDate`欄位只能包含`NULL`資料庫我們必須先確定值不是的值`NULL`再繼續計算。 如果`HiredDate`值是`NULL`，我們就只會傳回字串"Unknown"; 如果不是`NULL`，我們會計算目前時間之間的差異和`HiredDate`值，並傳回的天數。

若要利用我們要叫用它在使用資料繫結語法 GridView TemplateField 從這個方法。 啟動新 TemplateField 加入編輯資料行中的連結 GridView 的智慧標籤上按一下，然後加入新 TemplateField GridView。


[![在 GridView 中加入新 TemplateField](using-templatefields-in-the-gridview-control-vb/_static/image44.png)](using-templatefields-in-the-gridview-control-vb/_static/image43.png)

**圖 15**: GridView 中加入新 TemplateField ([按一下以檢視完整大小的影像](using-templatefields-in-the-gridview-control-vb/_static/image45.png))


設定此新 TemplateField `HeaderText` 「 天的工作 」 的屬性和其`ItemStyle`的`HorizontalAlign`屬性`Center`。 若要呼叫`DisplayDaysOnJob`方法在範本中，加入`ItemTemplate`，並使用下列資料繫結語法：


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample6.aspx)]

`Container.DataItem` 傳回`DataRowView`物件對應至`DataSource`記錄繫結至`GridViewRow`。 其`Row`屬性會傳回強型別`Northwind.EmployeesRow`，傳遞給`DisplayDaysOnJob`方法。 此資料繫結語法會直接在`ItemTemplate`（如下列宣告式語法中所示） 或可指派給`Text`標籤 Web 控制項的屬性。

> [!NOTE]
> 或者，而不是傳入`EmployeesRow`執行個體，我們可以只傳入`HireDate`值使用`<%# DisplayDaysOnJob(Eval("HireDate")) %>`。 不過，`Eval`方法會傳回`Object`，因此我們必須變更我們`DisplayDaysOnJob`接受類型的輸入的參數的方法簽章`Object`請改用。 我們無法盲目轉換`Eval("HireDate")`呼叫`DateTime`因為`HireDate`中的資料行`Employees`資料表可以包含`NULL`值。 因此，我們需要接受`Object`做為輸入參數的`DisplayDaysOnJob`方法，檢查是否有資料庫`NULL`值 (這可以透過`Convert.IsDBNull(objectToCheck)`)，然後據以繼續。


由於這些微妙我選擇要傳入整個`EmployeesRow`執行個體。 在下一個教學課程中，我們會看到更多的調整範例使用`Eval("columnName")`傳遞至格式化方法的輸入的參數的語法。

下列範例示範宣告式語法的我們 GridView 加入 TemplateField 後和`DisplayDaysOnJob`方法呼叫從`ItemTemplate`:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample7.aspx)]

圖 16 顯示已完成教學課程中，透過瀏覽器檢視時。


[![顯示員工已於作業的天數數目](using-templatefields-in-the-gridview-control-vb/_static/image47.png)](using-templatefields-in-the-gridview-control-vb/_static/image46.png)

**圖 16**: 數字的天員工已在工作上會顯示 ([按一下以檢視完整大小的影像](using-templatefields-in-the-gridview-control-vb/_static/image48.png))


## <a name="summary"></a>總結

在 GridView 控制項 TemplateField 允許較高的彈性，可與其他欄位控制項顯示資料。 TemplateFields 很理想的情況下其中：

- 需要一個 GridView 的資料行中顯示多個資料欄位
- 資料是最適合使用表示 Web 控制項，而不是純文字
- 輸出取決於基礎資料，例如顯示中繼資料，或在重新格式化資料

自訂資料的顯示，除了 TemplateFields 也會用於自訂我們未來將會看到教學課程，用於編輯和插入資料的使用者介面。

下面兩個教學課程將繼續探索開頭看使用 TemplateFields DetailsView 中的範本。 接下來，我們將會開啟 FormView 中，這個欄位會使用範本，以提供更大彈性的配置和資料結構。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 在此教學課程的前導檢閱者已 Dan Jagers。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](custom-formatting-based-upon-data-vb.md)
> [下一頁](using-templatefields-in-the-detailsview-control-vb.md)
