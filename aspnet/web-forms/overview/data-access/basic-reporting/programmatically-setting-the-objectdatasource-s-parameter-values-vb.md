---
uid: web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb
title: 以程式設計方式設定 ObjectDataSource 的參數值 (VB) |Microsoft Docs
author: rick-anderson
description: 在本教學課程中，我們將探討將方法加入我們的 DAL 和 BLL，會接受單一輸入的參數並傳回資料。 此範例會設定此參數...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 0ecb03b6-52a0-4731-8c7a-436391d36838
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb
msc.type: authoredcontent
ms.openlocfilehash: f823d1db7f98dcbbef12d20df4a28e39fae0ac26
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824300"
---
<a name="programmatically-setting-the-objectdatasources-parameter-values-vb"></a>以程式設計方式設定 ObjectDataSource 的參數值 (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_6_VB.exe)或[下載 PDF](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/datatutorial06vb1.pdf)

> 在本教學課程中，我們將探討將方法加入我們的 DAL 和 BLL，會接受單一輸入的參數並傳回資料。 此範例會以程式設計方式設定此參數。


## <a name="introduction"></a>簡介

如我們在中所見[先前的教學課程](declarative-parameters-vb.md)，有多種選項可供以宣告方式將參數值傳遞至 ObjectDataSource 的方法。 如果參數值為硬式編碼，來自 Web 控制項的頁面上，或在任何其他資料來源是可讀取的來源`Parameter`物件，例如，值可以結合到輸入參數，而不需要撰寫一行程式碼。

可能有的時間，不過，當參數值來自尚未納入考量的其中一個內建的資料來源所一些來源`Parameter`物件。 如果我們的網站支援的使用者帳戶我們可能會想要設定參數，根據目前登入訪客的使用者識別碼。 或者，我們可能需要自訂參數值之前將它傳送至 ObjectDataSource 的基礎物件的方法。

每當 ObjectDataSource`Select`方法會叫用 ObjectDataSource 第一次引發其[選取事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting%28VS.80%29.aspx)。 ObjectDataSource 的基礎物件的方法則會叫用。 一旦您已完成的 ObjectDataSource[選取事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selected%28VS.80%29.aspx)引發 （[圖 1] 說明這一系列的事件）。 傳入 ObjectDataSource 的基礎物件的方法的參數值可以設定或自訂事件處理常式中`Selecting`事件。


[![ObjectDataSource 的選取和選取的事件引發之前和之後其基礎物件的方法會叫用](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image2.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image1.png)

**圖 1**: ObjectDataSource`Selected`並`Selecting`事件引發之前和之後其基礎物件的方法叫用 ([按一下以檢視完整大小的影像](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image3.png))


在本教學課程將探討將方法加入我們的 DAL 和接受單一輸入的參數的 BLL `Month`，型別的`Integer`，並傳回`EmployeesDataTable`填入員工具有其招聘年度中指定的物件`Month`. 我們的範例會設定此參數，以程式設計方式根據目前的月份中，顯示一份 「 員工主管本月。 」

讓我們開始吧 ！

## <a name="step-1-adding-a-method-toemployeestableadapter"></a>步驟 1： 將方法加入`EmployeesTableAdapter`

第一個範例中我們需要加入一個方法來擷取這些員工的`HireDate`發生在指定的月份。 若要提供此功能，根據我們必須先建立方法，以在我們架構`EmployeesTableAdapter`子可對應到適當的 SQL 陳述式。 若要這麼做，首先開啟 Northwind 具類型資料集。 以滑鼠右鍵按一下`EmployeesTableAdapter`加上標籤，然後選擇 加入查詢。


[![將新的查詢加入至 EmployeesTableAdapter](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image5.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image4.png)

**圖 2**： 加入新的查詢，以`EmployeesTableAdapter`([按一下以檢視完整大小的影像](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image6.png))


選擇此選項，將會傳回資料列的 SQL 陳述式。 當您來到 指定`SELECT`陳述式畫面的預設值`SELECT`陳述式`EmployeesTableAdapter`會載入。 只需在中新增`WHERE`子句： `WHERE DATEPART(m, HireDate) = @Month`。 [DATEPART](https://msdn.microsoft.com/library/ms174420.aspx)會傳回特定的日期部分的 T-SQL 函式`datetime`類型; 在此案例中，我們會使用`DATEPART`傳回當月`HireDate`資料行。


[![傳回只有那些資料列位置的 HireDate Sloupec je 小於或等於@HiredBeforeDate參數](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image8.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image7.png)

**圖 3**： 傳回只有那些資料列所在`HireDate`資料行小於或等於`@HiredBeforeDate`參數 ([按一下以檢視完整大小的影像](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image9.png))


最後，變更`FillBy`並`GetDataBy`方法名稱來`FillByHiredDateMonth`和`GetEmployeesByHiredDateMonth`分別。


[![選擇比 FillBy 和 GetDataBy 更適當的方法名稱](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image11.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image10.png)

**圖 4**： 選擇 更多適當方法名稱比`FillBy`並`GetDataBy`([按一下以檢視完整大小的影像](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image12.png))


按一下 完成 以完成精靈並返回 資料集的設計介面。 `EmployeesTableAdapter`現在應該包含一組新的方法，以存取指定的月份中雇用的員工。


[![新的方法會出現在資料集的設計介面](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image14.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image13.png)

**圖 5**: 新方法會出現在資料集的設計介面中 ([按一下以檢視完整大小的影像](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image15.png))


## <a name="step-2-adding-thegetemployeesbyhireddatemonthmonthmethod-to-the-business-logic-layer"></a>步驟 2： 加入`GetEmployeesByHiredDateMonth(month)`方法商業邏輯層

由於我們應用程式架構會使用不同的圖層的商務邏輯和資料存取邏輯，我們必須將方法加入到 DAL 呼叫擷取員工雇用指定日期之前我們 BLL。 開啟`EmployeesBLL.vb`檔案，並新增下列方法：


[!code-vb[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample1.vb)]

如同我們在這個類別中的其他方法`GetEmployeesByHiredDateMonth(month)`DAL 直接呼叫，並傳回結果。

## <a name="step-3-displaying-employees-whose-hiring-anniversary-is-this-month"></a>步驟 3： 顯示的員工的招聘年度是本月

此範例中最後一個步驟是顯示的員工的招聘年度是本月。 新增 GridView，以開始`ProgrammaticParams.aspx`頁面中`BasicReporting`資料夾並加入新的 ObjectDataSource 做為其資料來源。 設定要使用 ObjectDataSource`EmployeesBLL`類別搭配`SelectMethod`設定為`GetEmployeesByHiredDateMonth(month)`。


[![使用 EmployeesBLL 類別](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image17.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image16.png)

**圖 6**： 使用`EmployeesBLL`類別 ([按一下以檢視完整大小的影像](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image18.png))


[![選取從 GetEmployeesByHiredDateMonth(month) 方法](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image20.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image19.png)

**圖 7**: Select From`GetEmployeesByHiredDateMonth(month)`方法 ([按一下以檢視完整大小的影像](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image21.png))


最後一個畫面會要求我們提供`month`參數值的來源。 因為我們會以程式設計方式設定此值，將參數的來源 設定為預設值沒有任何選項，然後按一下 完成。


[![保留參數的 [來源] 設定為 None](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image23.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image22.png)

**圖 8**： 保留參數來源設定為 None ([按一下以檢視完整大小的影像](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image24.png))


這會建立`Parameter`物件的 ObjectDataSource`SelectParameters`並沒有指定值的集合。


[!code-aspx[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample2.aspx)]

若要以程式設計方式設定此值，我們需要建立 ObjectDataSource 的事件處理常式`Selecting`事件。 若要這麼做，請移至 [設計] 檢視，並按兩下 ObjectDataSource。 或者，選取 ObjectDataSource、 移至 [屬性] 視窗中，並按一下閃電圖示。 接下來，請按兩下在文字方塊旁`Selecting`事件或輸入您想要使用的事件處理常式的名稱。 第三個選項，您可以藉由選取 ObjectDataSource 建立事件處理常式並將其`Selecting`兩份下拉式清單上方的頁面的程式碼後置類別的事件。


![按一下 [屬性] 視窗，列出 Web 控制項的事件中的閃電圖示](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image25.png)

**圖 9**： 按一下 屬性 視窗，列出 Web 控制項的事件中的閃電圖示


這三種方法的 ObjectDataSource 的加入新的事件處理常式`Selecting`頁面的程式碼後置類別的事件。 這個事件處理常式中，我們可以讀取和寫入使用的參數值`e.InputParameters(parameterName)`，其中*`parameterName`* 的值`Name`屬性中`<asp:Parameter>`標記 (`InputParameters`集合也可以是索引序數，如同在`e.InputParameters(index)`)。 若要設定`month`參數，以目前的月份中，將下列內容加入`Selecting`事件處理常式：


[!code-vb[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample3.vb)]

當瀏覽此頁面，透過瀏覽器可以看到該只有一位員工的雇用本月 （年 3 月） Laura Callahan，自 1994 年以來已被使用的公司。


[![顯示這個月的週年紀念日員工](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image27.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image26.png)

**圖 10**： 這些員工的週年紀念日這個月所示 ([按一下以檢視完整大小的影像](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image28.png))


## <a name="summary"></a>總結

雖然 ObjectDataSource 的參數值通常會設定以宣告方式，而不需要任何一行程式碼中，很容易就能以程式設計方式設定參數值。 我們要做的就是建立 ObjectDataSource 的事件處理常式`Selecting`引發的事件，基礎物件的方法是叫用，並手動設定透過一或多個參數的值之前`InputParameters`集合。

本教學課程結束時的基本報表區段。 [下一個教學課程](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md)一開始會篩選和主版詳細資料的案例 區段中，我們將查看允許的訪問項來篩選資料的技術，並從主要報表鑽研至詳細資料報表。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於 [http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Hilton Giesenow。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一步](declarative-parameters-vb.md)
