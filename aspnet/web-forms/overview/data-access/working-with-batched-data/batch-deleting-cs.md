---
uid: web-forms/overview/data-access/working-with-batched-data/batch-deleting-cs
title: 批次刪除 (C#) |Microsoft Docs
author: rick-anderson
description: 了解如何刪除單一作業中的多個資料庫記錄。 在使用者介面層，我們建置時建立在先前的工作階段 tut 增強型 GridView...
ms.author: riande
ms.date: 06/26/2007
ms.assetid: ac6916d0-a5ab-4218-9760-7ba9e72d258c
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-deleting-cs
msc.type: authoredcontent
ms.openlocfilehash: c5b4d3c21fad9000ae50ecb35a5d94d176a135ee
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832575"
---
<a name="batch-deleting-c"></a>批次刪除 (C#)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_65_CS.zip)或[下載 PDF](batch-deleting-cs/_static/datatutorial65cs1.pdf)

> 了解如何刪除單一作業中的多個資料庫記錄。 在使用者介面層，我們建置時在先前的教學課程中建立的增強型 GridView。 資料存取層中，我們換行以確保所有的刪除動作成功或所有刪除都會都回復在交易內的多個刪除作業的內容。


## <a name="introduction"></a>簡介

[前述教學課程](batch-updating-cs.md)探索如何建立批次編輯使用完全可編輯的 GridView 介面。 編輯介面批次中，使用者經常編輯多筆記錄一次的情況下，需要回傳和鍵盤-滑鼠內容更少的參數，藉此改善終端使用者的效率。 這項技術是同樣適用於很常見的使用者刪除一次的多筆記錄的頁面。

已使用的線上電子郵件用戶端的任何人已熟悉其中一個最常見的批次刪除介面： 對應刪除所有選取的項目具有的方格中的每個資料列中的核取方塊按鈕 （請參閱 圖 1）。 本教學課程是簡短而不是因為我們已經完成所有困難的工作先前的教學課程中建立的 web 型介面與方法來刪除的一系列記錄做為單一不可部分完成的作業中發生。 在 [新增 GridView 的核取方塊欄](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs.md)教學課程中我們建立 GridView 資料行的核取方塊，並在[資料庫修改包裝在交易](wrapping-database-modifications-within-a-transaction-cs.md)教學課程中我們建立中的方法會使用交易，以刪除 BLL`List<T>`的`ProductID`值。 在本教學課程中，我們將為基礎，並合併我們先前的體驗，以建立工作批次刪除範例。


[![每個資料列包含一個核取方塊](batch-deleting-cs/_static/image1.gif)](batch-deleting-cs/_static/image1.png)

**圖 1**： 每個資料列包含一個核取方塊 ([按一下以檢視完整大小的影像](batch-deleting-cs/_static/image2.png))


## <a name="step-1-creating-the-batch-deleting-interface"></a>步驟 1： 建立批次刪除介面

因為我們已經建立批次刪除介面[新增 GridView 的核取方塊欄](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs.md)教學課程中，我們可以直接將它複製到`BatchDelete.aspx`而不是從頭開始建立它。 首先開啟`BatchDelete.aspx`頁面中`BatchData`資料夾並`CheckBoxField.aspx`頁面中`EnhancedGridView`資料夾。 從`CheckBoxField.aspx`頁面上，移至來源檢視並複製之間的標記`<asp:Content>`標記如 圖 2 所示。


[![將 CheckBoxField.aspx 的宣告式標記複製到剪貼簿](batch-deleting-cs/_static/image2.gif)](batch-deleting-cs/_static/image3.png)

**圖 2**： 複製的宣告式標記`CheckBoxField.aspx`到剪貼簿 ([按一下以檢視完整大小的影像](batch-deleting-cs/_static/image4.png))


接下來，移至 [來源] 檢視中`BatchDelete.aspx`並貼上剪貼簿內的內容`<asp:Content>`標記。 也請複製和貼上從程式碼中的程式碼後置類別內`CheckBoxField.aspx.cs`到中的程式碼後置類別內`BatchDelete.aspx.cs`(`DeleteSelectedProducts`按鈕 s`Click`事件處理常式，`ToggleCheckState`方法，而`Click`事件處理常式針對`CheckAll`和`UncheckAll`按鈕)。 透過此內容中，複製之後`BatchDelete.aspx`頁面 s 程式碼後置類別應該包含下列程式碼：


[!code-csharp[Main](batch-deleting-cs/samples/sample1.cs)]

複製之後透過宣告式標記和原始程式碼，請花一點時間測試`BatchDelete.aspx`透過瀏覽器中檢視它。 您應該會看到 GridView，列出前十個產品 GridView 中的每一列列出產品的名稱、 類別和價格，以及一個核取方塊。 應該有三個按鈕： 檢查所有、 取消核取 [全部]，並刪除選取的產品。 取消核取 全部清除所有核取方塊時，按一下 查看全部 按鈕便會選取所有的核取方塊。 按一下 刪除選取的產品，會顯示訊息，其中列出`ProductID`值所選的產品，但不會實際刪除產品。


[![從 CheckBoxField.aspx 介面已移至 BatchDeleting.aspx](batch-deleting-cs/_static/image3.gif)](batch-deleting-cs/_static/image5.png)

**圖 3**： 從介面`CheckBoxField.aspx`已移至`BatchDeleting.aspx`([按一下以檢視完整大小的影像](batch-deleting-cs/_static/image6.png))


## <a name="step-2-deleting-the-checked-products-using-transactions"></a>步驟 2： 刪除選取的產品使用交易

正在刪除已成功複製到的介面之批次`BatchDeleting.aspx`，則所有剩下是更新程式碼，以便刪除選取的產品按鈕會刪除使用的已檢查的產品`DeleteProductsWithTransaction`方法中的`ProductsBLL`類別。 這種方法，在中新增[資料庫修改包裝在交易](wrapping-database-modifications-within-a-transaction-cs.md)教學課程中，接受做為其輸入`List<T>`的`ProductID`值，並刪除每個對應`ProductID`的範圍內交易。

`DeleteSelectedProducts`  按鈕 s`Click`目前事件處理常式會使用下列`foreach`迴圈來逐一查看每個 GridView 資料列：


[!code-csharp[Main](batch-deleting-cs/samples/sample2.cs)]

針對每個資料列，`ProductSelector`核取方塊 Web 控制項以程式設計方式所參考。 如果勾選它，則 s 的資料列`ProductID`擷取自`DataKeys`集合並`DeleteResults`標籤的`Text`屬性更新為包含訊息，指出選取的資料列時，已刪除。

上述程式碼不會實際刪除任何記錄為呼叫`ProductsBLL`類別的`Delete`方法標記為註解。即將套用此刪除邏輯，程式碼會刪除產品，但不是在不可部分完成的作業。 也就是如果序列中第一個的幾個刪除成功，但更新版本失敗 （可能是因為外部索引鍵條件約束違規），會擲回例外狀況，但已刪除這些產品會保持已刪除。

為了確保不可部分完成性，我們必須改用`ProductsBLL`類別的`DeleteProductsWithTransaction`方法。 因為這個方法會接受一份`ProductID`值，我們必須先編譯的方格中的這份清單，然後將它傳遞做為參數。 我們會先建立的執行個體`List<T>`型別的`int`。 內`foreach`我們需要加入所選的產品的迴圈`ProductID`值，這個`List<T>`。 在迴圈後這`List<T>`必須傳遞給`ProductsBLL`類別的`DeleteProductsWithTransaction`方法。 更新`DeleteSelectedProducts` 按鈕的`Click`為下列程式碼的事件處理常式：


[!code-csharp[Main](batch-deleting-cs/samples/sample3.cs)]

已更新的程式碼會建立`List<T>`型別的`int`(`productIDsToDelete`)，並填入它與`ProductID`来刪除的值。 在後`foreach`迴圈中，如果沒有選取，至少一個產品`ProductsBLL`類別的`DeleteProductsWithTransaction`方法呼叫並傳遞這份清單。 `DeleteResults`也會顯示標籤和資料重新繫結至 GridView，（使新已刪除的記錄不會再出現在方格中資料列）。

圖 4 顯示 GridView 之後已刪除選取的資料列數目。 圖 5 顯示的畫面，只有在已按下 刪除選取的產品 按鈕之後，立即。 請注意，在 圖 5`ProductID`刪除資料錄的值會顯示在下 GridView 的標籤，而這些資料列不再 GridView。


[![將刪除選取的產品](batch-deleting-cs/_static/image4.gif)](batch-deleting-cs/_static/image7.png)

**圖 4**: 選取產品將會刪除 ([按一下以檢視完整大小的影像](batch-deleting-cs/_static/image8.png))


[![刪除產品的 ProductID 值會列在下方的 GridView](batch-deleting-cs/_static/image5.gif)](batch-deleting-cs/_static/image9.png)

**圖 5**: 刪除產品`ProductID`的值為列下方的 GridView ([按一下以檢視完整大小的影像](batch-deleting-cs/_static/image10.png))


> [!NOTE]
> 若要測試`DeleteProductsWithTransaction`方法 s 不可部分完成性，以手動方式新增項目中的產品`Order Details`資料表，然後再嘗試刪除該產品 （以及其他項目）。 當您嘗試刪除相關聯的訂單的產品，但請注意如何選取的產品刪除回復時，您會收到的外部索引鍵條件約束違規。


## <a name="summary"></a>總結

建立批次刪除介面包含新增 GridView 的核取方塊資料行和 Button Web 控制項，當按下時，將會刪除所有選取的資料列做為單一不可部分完成的作業。 在本教學課程中建置這類介面所拼湊在兩個先前的教學課程中完成工作[新增 GridView 的核取方塊欄](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs.md)並[資料庫修改包裝在交易](wrapping-database-modifications-within-a-transaction-cs.md)。 在第一個教學課程中建立 GridView 的核取方塊資料行，並在後者中，我們實作到 BLL 中的方法，傳遞`List<T>`的`ProductID`值，則會全部都在交易的範圍內刪除它們。

在下一個教學課程中，我們將建立一個介面來執行批次插入。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於 [http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Hilton giesenow 將和 Teresa murphy 徹底檢驗了。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](batch-updating-cs.md)
> [下一頁](batch-inserting-cs.md)
