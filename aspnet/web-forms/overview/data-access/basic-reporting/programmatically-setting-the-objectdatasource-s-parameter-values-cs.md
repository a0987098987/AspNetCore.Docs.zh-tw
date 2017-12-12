---
uid: web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs
title: "以程式設計方式設定 ObjectDataSource 參數值 (C#) |Microsoft 文件"
author: rick-anderson
description: "在本教學課程，我們將探討將方法加入我們的 DAL 和 BLL 可接受單一輸入的參數和傳回資料。 此範例會將此參數設定..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 1c4588bb-255d-4088-b319-5208da756f4d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs
msc.type: authoredcontent
ms.openlocfilehash: 7a009d57f97838feb5b4a3253c6de9a872a9e9ee
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="programmatically-setting-the-objectdatasources-parameter-values-c"></a>以程式設計方式設定 ObjectDataSource 參數值 (C#)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_6_CS.exe)或[下載 PDF](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/datatutorial06cs1.pdf)

> 在本教學課程，我們將探討將方法加入我們的 DAL 和 BLL 可接受單一輸入的參數和傳回資料。 此範例會以程式設計方式設定這個參數。


## <a name="introduction"></a>簡介

如我們所見中[上一個教學課程](declarative-parameters-cs.md)，有多種選項可用以宣告方式將參數值傳遞至 ObjectDataSource 的方法。 如果參數值是硬式編碼，來自 Web 控制項在頁面上，或在任何其他資料來源是否可讀取的來源`Parameter`的物件，例如，值可以結合到輸入參數，而不需要撰寫一行程式碼。

有時候，不過，參數值來自其中一個內建資料來源所未佔用之某些來源`Parameter`物件。 如果網站支援的使用者帳戶我們可能會想要設定參數，根據目前登入的訪客的使用者 id。 或者，我們可能需要自訂的參數值，再將它傳送至 ObjectDataSource 基礎物件的方法。

每當 ObjectDataSource`Select`叫用方法時 ObjectDataSource 第一次引發其[Selecting 事件](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.objectdatasource.selecting%28VS.80%29.aspx)。 然後叫用 ObjectDataSource 基礎物件的方法。 一旦完成 ObjectDataSource[選取事件](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.objectdatasource.selected%28VS.80%29.aspx)引發 （圖 1 所示的事件，此順序）。 可設定或自訂的事件處理常式的參數值傳入 ObjectDataSource 基礎物件的方法`Selecting`事件。


[![ObjectDataSource 選取及選取事件引發之前和之後其基礎物件的方法會叫用](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image2.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image1.png)

**圖 1**: ObjectDataSource`Selected`和`Selecting`事件引發之前和之後其基礎物件的方法會叫用 ([按一下以檢視完整大小的影像](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image3.png))


在此教學課程中我們將探討將方法加入我們的 DAL 和 BLL 可接受單一輸入的參數`Month`，型別`int`並傳回`EmployeesDataTable`填入員工具有其招聘週年紀念日中指定的物件`Month`. 我們的範例會設定此參數，以程式設計的方式根據目前的月份，顯示一份 「 員工紀念日本月。 」

讓我們開始吧 ！

## <a name="step-1-adding-a-method-toemployeestableadapter"></a>步驟 1： 將方法加入`EmployeesTableAdapter`

此第一個範例中我們需要加入方法來擷取這些員工其`HireDate`發生指定的月份。 若要提供此功能，根據我們必須先建立中的方法，我們架構`EmployeesTableAdapter`對應至適當的 SQL 陳述式。 若要這麼做，首先開啟 Northwind 型別資料集。 以滑鼠右鍵按一下`EmployeesTableAdapter`加上標籤，然後選擇 加入查詢。


[![將新的查詢加入至 EmployeesTableAdapter](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image5.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image4.png)

**圖 2**： 加入新的查詢，來`EmployeesTableAdapter`([按一下以檢視完整大小的影像](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image6.png))


選擇要加入的 SQL 陳述式會傳回資料列。 當您來到 指定`SELECT`陳述式畫面上的預設`SELECT`陳述式`EmployeesTableAdapter`，就會載入。 只要加入在`WHERE`子句： `WHERE DATEPART(m, HireDate) = @Month`。 [DATEPART](https://msdn.microsoft.com/en-us/library/ms174420.aspx)的 T-SQL 函式會傳回特定日期部分的`datetime`類型; 在此情況下，我們會使用`DATEPART`傳回月份的`HireDate`資料行。


[![傳回只有那些資料列位置的 HireDate 資料行小於或等於@HiredBeforeDate參數](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image8.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image7.png)

**圖 3**： 傳回只有那些資料列位置`HireDate`資料行小於或等於`@HiredBeforeDate`參數 ([按一下以檢視完整大小的影像](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image9.png))


最後，變更`FillBy`和`GetDataBy`方法名稱至`FillByHiredDateMonth`和`GetEmployeesByHiredDateMonth`分別。


[![選擇比 FillBy 和 GetDataBy 更適當的方法名稱](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image11.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image10.png)

**圖 4**： 選擇更適當方法的名稱比`FillBy`和`GetDataBy`([按一下以檢視完整大小的影像](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image12.png))


按一下 完成 5d; 以完成精靈並返回 資料集的設計介面。 `EmployeesTableAdapter`現在應該包含一組新的方法來存取指定月份中雇用的員工。


[![新的方法會出現在設計介面中的資料集](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image14.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image13.png)

**圖 5**: 新方法會出現在設計介面中的資料集 ([按一下以檢視完整大小的影像](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image15.png))


## <a name="step-2-adding-thegetemployeesbyhireddatemonthmonthmethod-to-the-business-logic-layer"></a>步驟 2： 加入`GetEmployeesByHiredDateMonth(month)`商務邏輯層的方法

因為我們應用程式架構會使用不同的圖層的商務邏輯和資料存取邏輯，我們需要將方法加入至 DAL 呼叫擷取員工雇用指定日期之前我們 BLL。 開啟`EmployeesBLL.cs`檔案，然後加入下列方法：


[!code-csharp[Main](programmatically-setting-the-objectdatasource-s-parameter-values-cs/samples/sample1.cs)]

如同在此類別中，我們其他方法`GetEmployeesByHiredDateMonth(month)`只會呼叫向下至 DAL 並傳回結果。

## <a name="step-3-displaying-employees-whose-hiring-anniversary-is-this-month"></a>步驟 3： 顯示的員工的招聘週年日是本月

此範例中最後一個步驟是顯示的員工的招聘週年日是本月。 啟動新增至的 GridView`ProgrammaticParams.aspx`頁面`BasicReporting`資料夾並加入新 ObjectDataSource 做為資料來源。 設定用於 ObjectDataSource`EmployeesBLL`類別`SelectMethod`設`GetEmployeesByHiredDateMonth(month)`。


[![使用 EmployeesBLL 類別](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image17.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image16.png)

**圖 6**： 使用`EmployeesBLL`類別 ([按一下以檢視完整大小的影像](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image18.png))


[![選取從 GetEmployeesByHiredDateMonth(month) 方法](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image20.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image19.png)

**圖 7**: Select From`GetEmployeesByHiredDateMonth(month)`方法 ([按一下以檢視完整大小的影像](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image21.png))


最後一個畫面會要求我們提供`month`參數的值來源。 因為我們會以程式設計方式設定這個值，所以參數來源設定為預設無保留選項，然後按一下 [完成] 5d;。


[![保留參數來源設定為 None](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image23.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image22.png)

**圖 8**： 保留參數來源設定為 None ([按一下以檢視完整大小的影像](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image24.png))


這會建立`Parameter`物件在 ObjectDataSource`SelectParameters`並沒有指定值的集合。


[!code-aspx[Main](programmatically-setting-the-objectdatasource-s-parameter-values-cs/samples/sample2.aspx)]

若要以程式設計方式設定這個值，建立所需的事件處理常式用於 ObjectDataSource`Selecting`事件。 若要完成這項作業，請移至 [設計] 檢視並按兩下 ObjectDataSource。 或者，選取 ObjectDataSource、 移至 [屬性] 視窗，然後按一下出現閃電圖示。 接下來，請按兩下在文字方塊中旁`Selecting`事件或輸入您想要使用此事件處理常式的名稱。


![按一下 [屬性] 視窗，列出 Web 控制項的事件中閃電圖示](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image25.png)

**圖 9**： 按一下 [屬性] 視窗，列出 Web 控制項的事件中閃電圖示


這兩種方法是加入新的事件處理常式用於 ObjectDataSource`Selecting`網頁的程式碼後置類別的事件。 此事件處理常式中，我們可以讀取和寫入使用的參數值`e.InputParameters[parameterName]`，其中 *`parameterName`* 值`Name`屬性`<asp:Parameter>`標記 (`InputParameters`集合也可以序數，以在索引`e.InputParameters[index]`)。 若要設定`month`參數目前的月份，將下列內容加入`Selecting`事件處理常式：


[!code-csharp[Main](programmatically-setting-the-objectdatasource-s-parameter-values-cs/samples/sample3.cs)]

當這個頁面，透過瀏覽器瀏覽我們可以看到該只有一個員工的雇用本月 （年 3 月） 勞拉卡拉漢，已經在公司自 1994 年。


[![本月份會顯示其紀念日員工](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image27.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image26.png)

**圖 10**： 這些員工的紀念日這個月份顯示 ([按一下以檢視完整大小的影像](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image28.png))


## <a name="summary"></a>總結

ObjectDataSource 參數值通常會設定以宣告方式，而不需要程式碼時很容易就能以程式設計方式設定參數值。 我們要做為事件處理常式建立 ObjectDataSource`Selecting`基礎物件的方法是叫用，而且手動設定透過一或多個參數的值之前引發的事件`InputParameters`集合。

本教學課程結束時的基本報表區段。 [下一個教學課程](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md)篩選和主版詳細資料案例 區段中，我們將查看允許的訪問項來篩選資料的技術，並向下鑽研從主報表的詳細資料報表就會啟動。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 在此教學課程的前導檢閱者已 Hilton Giesenow。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一頁](declarative-parameters-cs.md)
[下一頁](displaying-data-with-the-objectdatasource-vb.md)
