---
uid: web-forms/overview/data-access/working-with-batched-data/batch-deleting-cs
title: 批次刪除 (C#) |Microsoft 文件
author: rick-anderson
description: 瞭解如何刪除單一作業中的多個資料庫記錄。 在使用者介面層，我們建置時建立在稍早 tut 增強 GridView...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: ac6916d0-a5ab-4218-9760-7ba9e72d258c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-deleting-cs
msc.type: authoredcontent
ms.openlocfilehash: 59c90dcf373d19aad42250ee6dedba5f09f833b5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="batch-deleting-c"></a>批次刪除 (C#)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_65_CS.zip)或[下載 PDF](batch-deleting-cs/_static/datatutorial65cs1.pdf)

> 瞭解如何刪除單一作業中的多個資料庫記錄。 使用者介面層級中，我們建置時增強 GridView 較早的教學課程中建立。 資料存取層中我們會換行，確保所有刪除動作成功或所有刪除動作就會回復在交易內的多個刪除作業。


## <a name="introduction"></a>簡介

[前述教學課程](batch-updating-cs.md)探索如何建立批次編輯使用完全可編輯 GridView 的介面。 編輯介面批次在其中使用者正在經常編輯多筆記錄一次的情況下，需要遠少於回傳及鍵盤-滑鼠內容參數，藉此改善使用者的效率。 這項技術是同樣適用於網頁很常見的使用者刪除許多記錄一次。

已使用的線上電子郵件用戶端的任何人在已熟悉其中一個最常見的批次刪除介面： 對應刪除所有選取的項目具有的方格中的每個資料列中的核取方塊按鈕 （請參閱圖 1）。 本教學課程是簡短而不是因為我們我們已經完成所有在上一個教學課程中建立的 web 型介面和方法，以刪除一系列的記錄是以單一不可部分完成作業的困難工作。 在[加入 GridView 的資料行的核取方塊](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs.md)教學課程中我們建立具有核取方塊，以及在資料行的 GridView[包裝在交易內的資料庫修改](wrapping-database-modifications-within-a-transaction-cs.md)教學課程中我們建立了中的方法會使用交易來刪除 BLL`List<T>`的`ProductID`值。 在本教學課程中，我們將為基礎，並合併我們先前的經驗，若要建立工作批次刪除範例。


[![每個資料列包含核取方塊](batch-deleting-cs/_static/image1.gif)](batch-deleting-cs/_static/image1.png)

**圖 1**： 每個資料列包含核取方塊 ([按一下以檢視完整大小的影像](batch-deleting-cs/_static/image2.png))


## <a name="step-1-creating-the-batch-deleting-interface"></a>步驟 1： 建立批次刪除介面

因為我們已經建立批次刪除中的介面[加入 GridView 的資料行的核取方塊](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs.md)教學課程中，我們可以直接將它複製到`BatchDelete.aspx`而不是從頭開始建立它。 先開啟`BatchDelete.aspx`頁面`BatchData`資料夾和`CheckBoxField.aspx`頁面`EnhancedGridView`資料夾。 從`CheckBoxField.aspx`頁面上，移至來源檢視並將複製之間的標記`<asp:Content>`標記放在圖 2 所示。


[![複製到剪貼簿 CheckBoxField.aspx 宣告式標記](batch-deleting-cs/_static/image2.gif)](batch-deleting-cs/_static/image3.png)

**圖 2**： 複製的宣告式標記`CheckBoxField.aspx`到剪貼簿 ([按一下以檢視完整大小的影像](batch-deleting-cs/_static/image4.png))


接下來，請移至來源檢視中`BatchDelete.aspx`並貼上剪貼簿中的內容`<asp:Content>`標記。 也請複製和貼上從程式碼中的程式碼後置類別內`CheckBoxField.aspx.cs`到中的程式碼後置類別內`BatchDelete.aspx.cs`(`DeleteSelectedProducts`按鈕 s`Click`事件處理常式，`ToggleCheckState`方法，而`Click`事件處理常式如`CheckAll`和`UncheckAll`按鈕)。 此內容，請在複製之後`BatchDelete.aspx`頁面 s 程式碼後置類別應包含下列程式碼：


[!code-csharp[Main](batch-deleting-cs/samples/sample1.cs)]

複製後的宣告式標記和程式碼，請花一點時間來測試`BatchDelete.aspx`透過瀏覽器中檢視它。 您應該會看到列出的每一列列出產品的名稱、 類別和價格的核取方塊以及 GridView 中的前十個產品的 GridView。 應該有三個按鈕： 請檢查所有、 取消選取所有和刪除選取的產品。 按一下核取所有的按鈕會選取所有核取方塊，取消核取所有清除所有核取方塊時。 按一下 刪除選取的產品會顯示訊息，其中列出`ProductID`值選取的產品，但不會實際刪除產品。


[![從 CheckBoxField.aspx 介面已移至 BatchDeleting.aspx](batch-deleting-cs/_static/image3.gif)](batch-deleting-cs/_static/image5.png)

**圖 3**： 從介面`CheckBoxField.aspx`已移至`BatchDeleting.aspx`([按一下以檢視完整大小的影像](batch-deleting-cs/_static/image6.png))


## <a name="step-2-deleting-the-checked-products-using-transactions"></a>步驟 2： 刪除選取的產品使用交易

與批次刪除成功複製到介面`BatchDeleting.aspx`，則所有剩下是更新程式碼，[刪除選取的產品] 按鈕會刪除選取的產品使用`DeleteProductsWithTransaction`方法中的`ProductsBLL`類別。 這個方法，在加入[包裝在交易內的資料庫修改](wrapping-database-modifications-within-a-transaction-cs.md)教學課程中，接受做為其輸入`List<T>`的`ProductID`值與刪除各有一個對應`ProductID`的範圍內交易。

`DeleteSelectedProducts`按鈕 s`Click`目前事件處理常式會使用下列`foreach`迴圈，逐一查看每個 GridView 資料列：


[!code-csharp[Main](batch-deleting-cs/samples/sample2.cs)]

每個資料列，`ProductSelector`核取方塊 Web 控制項以程式設計的方式參照。 如果已核取，資料列 s`ProductID`擷取自`DataKeys`集合和`DeleteResults`標籤的`Text`屬性會更新為包含訊息，指出資料列已選取為刪除。

上述程式碼不會實際刪除任何記錄在呼叫`ProductsBLL`類別的`Delete`方法標記為註解。要套用了這個刪除邏輯，該程式碼會刪除產品，但不是在不可部分完成的作業。 也就是說，如果序列中第一個的幾個刪除動作成功，但更新版本失敗 （可能是因為外部索引鍵條件約束違規），就會擲回例外狀況，但已刪除這些產品會維持已刪除。

若要確保不可部份完成性，我們要改用`ProductsBLL`類別的`DeleteProductsWithTransaction`方法。 因為這個方法會接受一份`ProductID`值，我們需要先編譯的方格中的這份清單並將它傳遞為參數。 我們會先建立的執行個體`List<T>`型別的`int`。 內`foreach`我們需要加入所選的產品的迴圈`ProductID`至此值`List<T>`。 在迴圈後這`List<T>`必須傳遞給`ProductsBLL`類別的`DeleteProductsWithTransaction`方法。 更新`DeleteSelectedProducts`按鈕的`Click`事件處理常式，以下列程式碼：


[!code-csharp[Main](batch-deleting-cs/samples/sample3.cs)]

更新程式碼會建立`List<T>`型別的`int`(`productIDsToDelete`)，並填入具有`ProductID`来刪除的值。 之後`foreach`迴圈中，如果沒有選取，至少一個產品`ProductsBLL`類別的`DeleteProductsWithTransaction`方法呼叫並傳遞這份清單。 `DeleteResults`也會顯示標籤和資料重新繫結至 GridView，（以便最近刪除的記錄不會再顯示為方格中的資料列）。

圖 4 顯示 GridView 之後已刪除選取的資料列數目。 圖 5 顯示螢幕，已按一下 [刪除選取的產品] 按鈕之後，立即。 請注意，在圖 5`ProductID`已刪除的記錄的值會顯示在下方 GridView 標籤中，而這些資料列不再 GridView。


[![將刪除選取的產品](batch-deleting-cs/_static/image4.gif)](batch-deleting-cs/_static/image7.png)

**圖 4**: 選取產品將會刪除 ([按一下以檢視完整大小的影像](batch-deleting-cs/_static/image8.png))


[![刪除產品 ProductID 值會列下方的 GridView](batch-deleting-cs/_static/image5.gif)](batch-deleting-cs/_static/image9.png)

**圖 5**: 「 刪除產品`ProductID`值會列在下方的 GridView ([按一下以檢視完整大小的影像](batch-deleting-cs/_static/image10.png))


> [!NOTE]
> 若要測試`DeleteProductsWithTransaction`方法 s 不可部份完成性，以手動方式為新增項目中的產品`Order Details`資料表，然後再嘗試刪除該產品 （以及其他）。 當您嘗試刪除的產品相關聯的順序，但請注意如何的所選的產品刪除作業會回復時，您會收到的外部索引鍵條件約束違規。


## <a name="summary"></a>總結

建立批次刪除介面牽涉到新增的 GridView 的資料行的核取方塊和按鈕 Web 控制項，按下時，會刪除所有選取的資料列是以單一不可部分完成作業。 在此教學課程中建立這類介面由拼湊在兩個先前的教學課程中完成的工作[加入 GridView 的資料行的核取方塊](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs.md)和[包裝在交易內的資料庫修改](wrapping-database-modifications-within-a-transaction-cs.md)。 第一個教學課程中建立 GridView 具有資料行的核取方塊，並在後者中，我們實作 BLL 中的方法，傳遞時`List<T>`的`ProductID`值，會刪除它們全部都在交易範圍內。

在下一個教學課程中，我們將建立執行批次插入的介面。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 此教學課程中的前導檢閱者已 Hilton Giesenow 和本文菲。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](batch-updating-cs.md)
> [下一頁](batch-inserting-cs.md)
