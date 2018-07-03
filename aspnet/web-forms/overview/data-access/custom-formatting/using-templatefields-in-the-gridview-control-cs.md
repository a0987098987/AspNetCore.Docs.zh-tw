---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-cs
title: 在 GridView 控制項 (C#) 中使用 TemplateFields |Microsoft Docs
author: rick-anderson
description: 為了提供彈性，GridView 會提供 TemplateField，會呈現使用範本。 範本可以包含混合的靜態 HTML Web 控制項和...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 11de31e8-a78a-4f96-bd75-66e994175902
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-cs
msc.type: authoredcontent
ms.openlocfilehash: fe84bd24824f4a0326a6e8d41c0d291c7ef585af
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363827"
---
<a name="using-templatefields-in-the-gridview-control-c"></a>在 GridView 控制項 (C#) 中使用 TemplateFields
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_12_CS.exe)或[下載 PDF](using-templatefields-in-the-gridview-control-cs/_static/datatutorial12cs1.pdf)

> 為了提供彈性，GridView 會提供 TemplateField，會呈現使用範本。 範本可以包含混合的靜態 HTML Web 控制項及資料繫結語法。 在本教學課程中，我們將檢驗如何使用 TemplateField 達到更高的自訂與 GridView 控制項。


## <a name="introduction"></a>簡介

GridView 組成一組欄位，指出來自哪些屬性`DataSource`要包含在轉譯的輸出，以及資料顯示方式。 資料值顯示為文字 BoundField 最簡單的欄位類型。 其他欄位型別顯示使用替代的 HTML 項目資料。 CheckBoxField，比方說，將呈現為指定的資料欄位; 的值取決於其核取的狀態的核取方塊ImageField 呈現的影像的影像來源為基礎的指定的資料欄位。 呈現超連結和按鈕的狀態取決於基礎的資料欄位值，可以使用 HyperLinkField 和 ButtonField 欄位類型。

雖然 CheckBoxField、 ImageField、 HyperLinkField 和 ButtonField 欄位型別允許替代的檢視的資料，它們仍是相當有限，相對於格式。 CheckBoxField 只能顯示單一的核取方塊，而 ImageField 只能顯示單一映像。 特定欄位要顯示的文字，核取方塊，如果*和*映像，全都以不同的資料欄位值為基礎？ 或者，如果我們想要顯示 使用核取方塊、 影像、 超連結或按鈕以外的 Web 控制項的資料？ 此外，BoundField 限制它的顯示畫面至單一資料欄位。 假如我們想要顯示單一的 GridView 資料行中的兩個或多個資料欄位值？

GridView 提供 TemplateField，會呈現使用以容納此程度的彈性*範本*。 範本可以包含混合的靜態 HTML Web 控制項及資料繫結語法。 此外，TemplateField 有各式各樣的範本可用來自訂轉譯為不同的情況。 例如，`ItemTemplate`用來呈現每個資料列，資料格預設但`EditItemTemplate`範本可以用來編輯資料時，自訂介面。

在本教學課程中，我們將檢驗如何使用 TemplateField 達到更高的自訂與 GridView 控制項。 在 [前述教學課程](custom-formatting-based-upon-data-cs.md)我們了解如何自訂格式的基礎的資料使用基礎`DataBound`和`RowDataBound`事件處理常式。 自訂格式的基礎資料為基礎的另一種方式藉由呼叫的格式化設定方法在範本內。 在本教學課程中，我們將探討這項技術。

在本教學課程中，我們將使用 TemplateFields 自訂的員工清單的外觀。 具體來說，我們會列出所有員工，但會顯示員工的名字和姓氏的名稱，在一個資料行、 在日曆控制項，以及 [狀態] 欄，指出多少天後他們已被採用公司雇用日期。


[![可用來自訂顯示三個 TemplateFields](using-templatefields-in-the-gridview-control-cs/_static/image2.png)](using-templatefields-in-the-gridview-control-cs/_static/image1.png)

**圖 1**： 用來自訂顯示三個 TemplateFields ([按一下以檢視完整大小的影像](using-templatefields-in-the-gridview-control-cs/_static/image3.png))


## <a name="step-1-binding-the-data-to-the-gridview"></a>步驟 1: 將資料繫結至 GridView

在報告情況下，您需要使用 TemplateFields 來自訂外觀，我覺得最簡單的方式著手建立第一次包含只 BoundFields 的 GridView 控制項，然後新增新的 TemplateFields 或轉換至現有的 BoundFields視 TemplateFields。 因此，現在就開始本教學課程中加入透過設計工具頁面上的 GridView 和繫結至傳回的員工清單 ObjectDataSource。 這些步驟將為每個員工欄位，以建立與 BoundFields GridView。

開啟`GridViewTemplateField.aspx`頁面上，然後從 [工具箱] 拖曳至設計工具拖曳的 GridView。 從 GridView 的智慧標籤選擇 加入新的 ObjectDataSource 控制項叫用`EmployeesBLL`類別的`GetEmployees()`方法。


[![加入新的 ObjectDataSource 控制項叫用 GetEmployees() 方法](using-templatefields-in-the-gridview-control-cs/_static/image5.png)](using-templatefields-in-the-gridview-control-cs/_static/image4.png)

**圖 2**： 將新的 ObjectDataSource 控制項加入該 Invokes`GetEmployees()`方法 ([按一下以檢視完整大小的影像](using-templatefields-in-the-gridview-control-cs/_static/image6.png))


這種方式中的繫結 GridView 會自動將新增 BoundField 的每個員工的屬性： `EmployeeID`， `LastName`， `FirstName`， `Title`， `HireDate`， `ReportsTo`，和`Country`。 這份報告讓我們不需要考慮顯示`EmployeeID`， `ReportsTo`，或`Country`屬性。 若要移除這些 BoundFields 中，您可以：

- 從 GridView 的智慧標籤的 [編輯資料行] 連結使用欄位對話方塊中，按一下，即可啟動此對話方塊。 接下來，選取左下方從 BoundFields 清單，然後按一下紅色的 X 按鈕移除 BoundField。
- 以手動方式編輯 GridView 的宣告式語法，從來源檢視、 刪除`<asp:BoundField>`BoundField 您想要移除的項目。

已移除之後`EmployeeID`， `ReportsTo`，和`Country`BoundFields，GridView 的標記看起來應該像：


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample1.aspx)]

請花一點時間瀏覽器中檢視進度。 此時應該會看到含有記錄資料表每一位員工和四個資料行： 一個用於員工的姓氏，一個用於他們的名字、 標題，其中，一個用於其雇用日期。


[![LastName、 FirstName、 標題和 HireDate 欄位會顯示每個員工](using-templatefields-in-the-gridview-control-cs/_static/image8.png)](using-templatefields-in-the-gridview-control-cs/_static/image7.png)

**圖 3**: `LastName`， `FirstName`， `Title`，並`HireDate`欄位會顯示每個員工 ([按一下以檢視完整大小的影像](using-templatefields-in-the-gridview-control-cs/_static/image9.png))


## <a name="step-2-displaying-the-first-and-last-names-in-a-single-column"></a>步驟 2： 顯示單一資料行中的第一個和最後一個名稱

目前，每一位員工的名字和姓氏會顯示在個別的資料行。 可能是很好將它們結合成單一資料行。 若要這麼做，我們需要使用 TemplateField。 我們可以加入新的 TemplateField、 加入資料繫結語法，與所需的標記，然後刪除`FirstName`並`LastName`BoundFields，或者我們可以將轉換`FirstName`BoundField 到 TemplateField，編輯包含 TemplateField`LastName`值，然後再移除`LastName`BoundField。

這兩種方法 net 相同的結果，不過我想將 BoundFields 轉換成 TemplateFields 可能的話，因為轉換會自動新增的個人`ItemTemplate`和`EditItemTemplate`Web 控制項與資料繫結語法，來模擬外觀和 BoundField 的功能。 優點是，我們需要進行較少工作使用 TemplateField 轉換程序會有執行某些工作的我們。

若要轉換為 TemplateField 現有 BoundField，請按一下 GridView 的智慧標籤，啟動 [欄位] 對話方塊中的 [編輯資料行] 連結。 選取來源檔的清單中較低的左上角，然後按一下 在右下角的"轉換為 TemplateField 到這個欄位 」 連結 BoundField。


[![BoundField 轉換從 [欄位] 對話方塊中的 TemplateField](using-templatefields-in-the-gridview-control-cs/_static/image11.png)](using-templatefields-in-the-gridview-control-cs/_static/image10.png)

**圖 4**： 從 [欄位] 對話方塊中轉換為 BoundField 到 TemplateField ([按一下以檢視完整大小的影像](using-templatefields-in-the-gridview-control-cs/_static/image12.png))


請繼續進行，並將轉換`FirstName`BoundField TemplateField 到。 這項變更之後並無差別 perceptive 設計工具中。 這是因為轉換為 TemplateField BoundField 建立維護的外觀與風格 BoundField TemplateField。 儘管那里沒有視覺化差異在此時設計工具中的，此轉換程序已取代 BoundField 的宣告式語法- `<asp:BoundField DataField="FirstName" HeaderText="FirstName" SortExpression="FirstName" />` -使用下列的 TemplateField 語法：


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample2.aspx)]

TemplateField 如您所見，包含兩個範本`ItemTemplate`具有標籤的`Text`屬性設定的值為`FirstName`資料欄位，和`EditItemTemplate`搭配 TextBox 控制項`Text`屬性也設定若要`FirstName`資料欄位。 資料繫結語法- `<%# Bind("fieldName") %>` -指出資料欄位*`fieldName`* 繫結至指定的 Web 控制項屬性。

若要新增`LastName`資料欄位值為此我們需要加入另一個 Label Web 控制項中的 TemplateField`ItemTemplate`並繫結其`Text`屬性設`LastName`。 這可以是以手動方式或透過設計工具完成。 若要以手動方式執行，只要新增適當的宣告式語法， `ItemTemplate`:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample3.aspx)]

若要將它加入透過設計工具中，按一下從 GridView 的智慧標籤的 [編輯範本] 連結。 這會顯示 GridView 的範本編輯介面。 在此介面的智慧標籤是一份 GridView 中的範本。 因為我們只需要一個 TemplateField 此時，只有列在下拉式清單中的範本會針對這些範本`FirstName`連同 TemplateField`EmptyDataTemplate`和`PagerTemplate`。 `EmptyDataTemplate`範本，如果指定，會用來呈現 GridView 的輸出，如果資料繫結至 GridView; 中沒有任何結果`PagerTemplate`，如果指定，用來呈現的 GridView 會支援分頁的分頁介面。


[![GridView 的範本可以透過設計工具進行編輯](using-templatefields-in-the-gridview-control-cs/_static/image14.png)](using-templatefields-in-the-gridview-control-cs/_static/image13.png)

**圖 5**: GridView 的範本可以是編輯透過設計工具 ([按一下以檢視完整大小的影像](using-templatefields-in-the-gridview-control-cs/_static/image15.png))


若也要顯示`LastName`中`FirstName`TemplateField 拖曳 Label 控制項從工具箱拖曳到`FirstName`TemplateField 的`ItemTemplate`GridView 裡的範本編輯介面。


[![將標籤 Web 控制項加入 FirstName TemplateField 的 ItemTemplate](using-templatefields-in-the-gridview-control-cs/_static/image17.png)](using-templatefields-in-the-gridview-control-cs/_static/image16.png)

**圖 6**： 加入標籤 Web 控制項來`FirstName`TemplateField 的 ItemTemplate ([按一下以檢視完整大小的影像](using-templatefields-in-the-gridview-control-cs/_static/image18.png))


此時加入 TemplateField 標籤 Web 控制項有其`Text`屬性設定為 「 標籤 」。 我們要使這個屬性繫結至的值，請變更此`LastName`資料改為欄位。 若要完成此標籤控制項的智慧標籤上按一下，然後選擇 [編輯資料繫結] 選項。


[![從標籤的智慧標籤中選擇 編輯資料繫結選項](using-templatefields-in-the-gridview-control-cs/_static/image20.png)](using-templatefields-in-the-gridview-control-cs/_static/image19.png)

**圖 7**： 從標籤的智慧標籤中選擇 編輯資料繫結選項 ([按一下以檢視完整大小的影像](using-templatefields-in-the-gridview-control-cs/_static/image21.png))


這會顯示資料繫結 對話方塊。 從這裡您可以選取要參與資料繫結，從左側清單，然後選擇 繫結至資料，從下拉式清單，右側欄位的屬性。 選擇`Text`左邊的屬性和`LastName`欄位的右邊，然後按一下 [確定]。


[![文字屬性繫結到 LastName 資料欄位](using-templatefields-in-the-gridview-control-cs/_static/image23.png)](using-templatefields-in-the-gridview-control-cs/_static/image22.png)

**圖 8**： 繫結`Text`屬性設`LastName`資料欄位 ([按一下以檢視完整大小的影像](using-templatefields-in-the-gridview-control-cs/_static/image24.png))


> [!NOTE]
> DataBindings 對話方塊可讓您指出是否要執行雙向資料繫結。 如果將此選項保留未核取的資料繫結語法`<%# Eval("LastName")%>`而不是用於將`<%# Bind("LastName")%>`。 本教學課程中，兩種方法是正常的。 雙向資料繫結就變得重要插入及編輯資料時。 只顯示資料，不過，兩種方法將同樣適用。 在未來的教學課程中，我們將討論在詳細資料中的雙向資料繫結。


請花一點時間才能檢視此頁面，透過瀏覽器。 如您所見，GridView 仍然會包含四個資料行;不過，`FirstName`資料行現在會列出*兩者*`FirstName`和`LastName`資料欄位值。


[![FirstName 和 LastName 值會顯示單一資料行中](using-templatefields-in-the-gridview-control-cs/_static/image26.png)](using-templatefields-in-the-gridview-control-cs/_static/image25.png)

**圖 9**： 同時`FirstName`並`LastName`值會顯示單一資料行中 ([按一下以檢視完整大小的影像](using-templatefields-in-the-gridview-control-cs/_static/image27.png))


若要完成此第一個步驟，移除`LastName`BoundField 和重新命名`FirstName`TemplateField 的`HeaderText`"Name"屬性。 在這些變更之後 GridView 的宣告式標記看起來應該如下所示：


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample4.aspx)]


[![每位員工的第一個和最後一個名稱會顯示在一個資料行](using-templatefields-in-the-gridview-control-cs/_static/image29.png)](using-templatefields-in-the-gridview-control-cs/_static/image28.png)

**圖 10**： 每個員工的第一個和最後一個名稱會顯示在一個資料行 ([按一下以檢視完整大小的影像](using-templatefields-in-the-gridview-control-cs/_static/image30.png))


## <a name="step-3-using-the-calendar-control-to-display-thehireddatefield"></a>步驟 3： 使用 Calendar 控制項來顯示`HiredDate`欄位

在 GridView 中以文字顯示資料欄位值是簡單，只要使用 BoundField 就行了。 適用於特定案例中，不過，資料是最適合使用來表示特定的 Web 控制項而非只是文字。 可以使用 TemplateFields 這類自訂資料的顯示。 比方說，而非以文字顯示員工的雇用日期，我們無法顯示行事曆 (使用[行事曆控制項](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar(VS.80).aspx)) 使用其雇用日期反白顯示。

若要達成此目的，先轉換`HiredDate`BoundField TemplateField 到。 只要移至 GridView 的智慧標籤，然後按一下 [編輯資料行] 連結，啟動 [欄位] 對話方塊。 選取`HiredDate`BoundField，然後按一下 」 這個欄位轉換為 TemplateField。 」


[![HiredDate BoundField 轉換為 TemplateField](using-templatefields-in-the-gridview-control-cs/_static/image32.png)](using-templatefields-in-the-gridview-control-cs/_static/image31.png)

**圖 11**： 轉換`HiredDate`TemplateField 到 BoundField ([按一下以檢視完整大小的影像](using-templatefields-in-the-gridview-control-cs/_static/image33.png))


如我們在步驟 2 中所見的這樣會取代 BoundField 包含 TemplateField`ItemTemplate`並`EditItemTemplate`其標籤與文字方塊使用其`Text`屬性繫結至`HiredDate`值使用的資料繫結語法`<%# Bind("HiredDate")%>`.

若要使用日曆控制項中取代的文字，請移除標籤，並新增日曆控制項中編輯範本。 從設計工具中，選取 從 GridView 的智慧標籤的 編輯範本，然後選擇`HireDate`TemplateField 的`ItemTemplate`從下拉式清單。 接下來，刪除標籤控制項，並將日曆控制項從 工具箱 拖曳至 範本的編輯介面。


[![加入行事曆控制項來 HireDate TemplateField 的 ItemTemplate](using-templatefields-in-the-gridview-control-cs/_static/image35.png)](using-templatefields-in-the-gridview-control-cs/_static/image34.png)

**圖 12**： 新增至行事曆控制項`HireDate`TemplateField 的`ItemTemplate`([按一下以檢視完整大小的影像](using-templatefields-in-the-gridview-control-cs/_static/image36.png))


此時在 gridview 裡的每個資料列會包含中的 Calendar 控制項其`HiredDate`TemplateField。 不過，員工的實際`HiredDate`日曆控制項，造成每個預設為顯示目前的月份和日期的日曆控制項中未設定值任何位置。 若要解決此問題，我們需要指派每一位員工`HiredDate`行事曆控制項[SelectedDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selecteddate(VS.80).aspx)並[VisibleDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.visibledate(VS.80).aspx)屬性。

從行事曆控制項的智慧標籤上，選擇 [編輯資料繫結]。 接下來，繫結兩者`SelectedDate`並`VisibleDate`屬性，以`HiredDate`資料欄位。


[![SelectedDate 和 VisibleDate 屬性繫結至 HiredDate 資料欄位](using-templatefields-in-the-gridview-control-cs/_static/image38.png)](using-templatefields-in-the-gridview-control-cs/_static/image37.png)

**圖 13**： 繫結`SelectedDate`並`VisibleDate`屬性，以`HiredDate`資料欄位 ([按一下以檢視完整大小的影像](using-templatefields-in-the-gridview-control-cs/_static/image39.png))


> [!NOTE]
> 日曆控制項選取的日期不一定需要顯示。 例如，行事曆可能有 1 年 8 月<sup>st</sup>，1999 做為選取的日期，但會顯示在目前的月份和年份。 月曆控制項的指定選取的日期和可見日期`SelectedDate`和`VisibleDate`屬性。 因為我們想要同時選取員工`HiredDate`，並確定它會顯示我們需要將這兩個屬性要繫結`HireDate`資料欄位。


檢視時的網頁瀏覽器中，行事曆現在會顯示員工的雇用日期的月份，並選取該特定的日期。


[![日曆控制項中顯示員工的 HiredDate](using-templatefields-in-the-gridview-control-cs/_static/image41.png)](using-templatefields-in-the-gridview-control-cs/_static/image40.png)

**圖 14**: 員工`HiredDate`顯示日曆控制項中 ([按一下以檢視完整大小的影像](using-templatefields-in-the-gridview-control-cs/_static/image42.png))


> [!NOTE]
> 相對於所有範例中我們見到目前為止，本教學課程中我們*未*設定`EnableViewState`屬性設`false`針對此 GridView。 此決策的原因是因為按一下日期的日曆控制項造成回傳，行事曆選取的日期設定為僅需要按一下日期。 GridView 的檢視狀態已停用，不過，在每次回傳 GridView 的資料會重新繫結至其基礎的資料來源，這會導致的行事曆選取的日期設定*回復*員工`HireDate`、 覆寫由使用者選擇日期。


本教學課程中這是無意義的討論區因為使用者不能更新員工的`HireDate`。 它可能會設定月曆控制項，使其日期不能選取最理想的做法。 不論如何，本教學課程會示範在某些情況下檢視狀態必須啟用才能提供特定功能。

## <a name="step-4-showing-the-number-of-days-the-employee-has-worked-for-the-company"></a>步驟 4： 顯示指定天數員工曾任職的公司

到目前為止，我們討論過 TemplateFields 的兩個應用的程式：

- 將兩個或多個資料欄位值結合成一個資料行，以及
- 表示資料欄位值，而不文字中使用的 Web 控制項

TemplateFields 第三個使用中顯示的 GridView 有關的中繼資料基礎資料。 除了顯示員工的雇用日期，比方說，我們可能也要有一個會顯示總多少天後，就已經出現在工作的資料行。

尚未 TemplateFields 的另一個用法時，會發生在案例中需要會以不同的方式中顯示的網頁報表的格式儲存在資料庫中基礎的資料。 想像`Employees`資料表中有`Gender`儲存字元的欄位`M`或`F`指出員工的性別。 當這項資訊顯示在網頁上，我們可能想要顯示為"Male"或"Female"，而不只是"M"或"F"的性別。

這兩種案例可以藉由建立處理*格式化方法*ASP.NET 網頁的程式碼後置類別中 (或在個別的類別庫中，實作為`static`方法)，會叫用的範本。 從範本使用相同的資料繫結語法前文所見，這種格式的方法會叫用。 可以接受任意數目的參數，以格式化方法，但必須傳回字串。 這個傳回的字串會插入至範本的 HTML。

為了說明這個概念，讓我們來加強我們的教學課程，以顯示列出員工已在工作總天數的資料行。 此格式的方法將會在採取`Northwind.EmployeesRow`物件，並傳回的員工已採用字串形式的天數。 這個方法可以加入至 ASP.NET 網頁的程式碼後置類別，但*必須*標示為`protected`或`public`才能存取從該範本。


[!code-csharp[Main](using-templatefields-in-the-gridview-control-cs/samples/sample5.cs)]

由於`HiredDate`欄位可以包含`NULL`資料庫的值，我們必須先確定值不是`NULL`再繼續進行計算。 如果`HiredDate`值是`NULL`，我們只會傳回字串 「 未知 」; 如果不是`NULL`，我們會計算目前時間之間的差異和`HiredDate`值，並傳回的天數。

若要利用這個方法，我們需要從使用資料繫結語法 GridView 中的 TemplateField 叫用它。 啟動新的 TemplateField 加入按一下 GridView 的智慧標籤中的 [編輯資料行] 連結，並加入新的 TemplateField GridView。


[![加入新的 TemplateField 至 GridView](using-templatefields-in-the-gridview-control-cs/_static/image44.png)](using-templatefields-in-the-gridview-control-cs/_static/image43.png)

**圖 15**： 將新的 TemplateField 新增至 GridView ([按一下以檢視完整大小的影像](using-templatefields-in-the-gridview-control-cs/_static/image45.png))


設定這個新的 TemplateField `HeaderText` 「 天上作業 」 的屬性並將其`ItemStyle`的`HorizontalAlign`屬性設`Center`。 若要呼叫`DisplayDaysOnJob`方法，從範本中，新增`ItemTemplate`，並使用下列資料繫結語法：


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample6.aspx)]

`Container.DataItem` 會傳回`DataRowView`物件對應至`DataSource`記錄繫結至`GridViewRow`。 其`Row`屬性會傳回強型別`Northwind.EmployeesRow`，這會傳遞至`DisplayDaysOnJob`方法。 此資料繫結語法會顯示可以直接在`ItemTemplate`（如下列宣告式語法所示） 或可指派給`Text`Label Web 控制項的屬性。

> [!NOTE]
> 或者，而不是傳入`EmployeesRow`執行個體，我們可以只使用傳入`HireDate`值使用`<%# DisplayDaysOnJob(Eval("HireDate")) %>`。 不過，`Eval`方法會傳回`object`，因此我們必須變更我們`DisplayDaysOnJob`接受類型的輸入的參數的方法簽章`object`，改為。 我們不能盲目地轉型`Eval("HireDate")`呼叫`DateTime`因為`HireDate`中的資料行`Employees`資料表可以包含`NULL`值。 因此，我們需要接受`object`做為輸入的參數`DisplayDaysOnJob`方法，檢查是否有資料庫`NULL`值 (這可以使用來完成`Convert.IsDBNull(objectToCheck)`)，視情況繼續執行。


由於這些微妙之處，我選擇將整個`EmployeesRow`執行個體。 在下一個教學課程中，我們將看到使用的多個調整範例`Eval("columnName")`傳入的格式化方法的輸入的參數的語法。

下圖顯示的宣告式語法的我們 GridView TemplateField 新增之後，`DisplayDaysOnJob`方法呼叫從`ItemTemplate`:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample7.aspx)]

[圖 16] 顯示已完成的教學課程中，透過瀏覽器檢視時。


[![員工已在工作的日期數字顯示](using-templatefields-in-the-gridview-control-cs/_static/image47.png)](using-templatefields-in-the-gridview-control-cs/_static/image46.png)

**圖 16**: 數字的天員工已在工作上顯示 ([按一下以檢視完整大小的影像](using-templatefields-in-the-gridview-control-cs/_static/image48.png))


## <a name="summary"></a>總結

在 GridView 控制項中的 TemplateField 允許較大程度的彈性，比起其他欄位控制項顯示資料。 是非常理想的情況下的 TemplateFields 其中：

- 多個資料欄位需要一個 GridView 資料行中顯示
- 使用 Web 控制項，而不是純文字最能表示的資料
- 輸出取決於基礎資料，例如顯示中繼資料或是在重新格式化資料

除了自訂資料的顯示，也會使用 TemplateFields 自訂使用者介面，如稍後所示在未來的教學課程，用於編輯和插入資料。

下面兩個教學課程會繼續探索開始了解在 DetailsView 中使用 TemplateFields 的範本。 接下來，我們將轉向 FormView，取代欄位會使用範本，以提供更大彈性的配置和資料結構。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Dan Jagers。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](custom-formatting-based-upon-data-cs.md)
> [下一頁](using-templatefields-in-the-detailsview-control-cs.md)
