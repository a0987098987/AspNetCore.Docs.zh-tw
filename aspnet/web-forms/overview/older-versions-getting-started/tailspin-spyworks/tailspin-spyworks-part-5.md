---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: 第 5 節： 商務邏輯 |Microsoft Docs
author: JoeStagner
description: 本教學課程系列會詳細說明所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。 第 5 部分加入一些商務邏輯。
ms.author: riande
ms.date: 07/21/2010
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: e18acb66dbdb3bd3e0dfa21193f617dad82afc74
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826429"
---
<a name="part-5-business-logic"></a>第 5 節： 商務邏輯
====================
藉由[Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks 示範建立功能強大、 可擴充的應用程式，適用於.NET 平台是如何富含簡單。 它會展示如何在 ASP.NET 4 中使用最棒的新功能，建置線上商店，包括購物、 簽出，以及系統管理。
> 
> 本教學課程系列會詳細說明所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。 第 5 部分加入一些商務邏輯。


## <a id="_Toc260221671"></a>  加入一些商務邏輯

我們想要我們的購物體驗，只要有人瀏覽我們的網站才能使用。 訪客可以瀏覽並放入購物車新增項目，即使未註冊或登入。 當他們準備好要簽出即會獲得驗證選項，如果它們不是尚未成員都能夠建立帳戶。

這表示，我們必須實作邏輯以將購物車從匿名狀態轉換為 「 已註冊使用者 」 狀態。

讓我們建立名為 「 類別 」 的目錄，然後在資料夾上按一下滑鼠右鍵並建立新的 「 類別 」 檔案，名為 MyShoppingCart.cs

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

我們將擴充實作 MyShoppingCart.aspx 頁面的類別，我們將會執行這項操作使用如先前所述。NET 的功能強大的 「 部分類別 」 建構。

我們 MyShoppingCart.aspx.cf 檔案產生的呼叫看起來像這樣。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

請注意 「 部分 」 關鍵字的使用。

我們剛剛產生的類別檔案如下所示。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

我們會將合併我們實作 partial 關鍵字加入這個檔案。

我們的新類別檔案現在看起來像這樣。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

我們會將我們的類別加入的第一個方法是 「 AddItem 」 方法。 這是會在使用者按一下 產品清單 和 產品詳細資料頁面的 「 新增術 」 連結時最後呼叫的方法。

將下列附加至使用頁面頂端的陳述式。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

並將下列方法新增至 MyShoppingCart 類別。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

若要查看項目是否已在購物車中，我們會使用 LINQ to Entities。 如果因此，我們將更新項目的數量，否則我們會建立新的項目選取的項目

若要呼叫這個方法中，我們將實作 AddToCart.aspx 頁面，不僅類別此方法接著會顯示目前的購物 = 購物車新增項目之後。

以滑鼠右鍵按一下 [方案總管] 中的方案名稱，並新增和名為 AddToCart.aspx，因為我們先前做過的新頁面。

雖然我們無法使用此頁面顯示暫時結果等低內建的問題，我們的實作中，頁面將不會實際呈現，但而不是呼叫 「 Add 」 邏輯和重新導向。

若要完成此我們會加入下列程式碼頁面\_Load 事件。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

請注意，我們會擷取要從查詢字串參數，然後呼叫類別的 AddItem 方法新增至購物車的產品。

假設沒有錯誤發生，則控制權會傳遞至我們將會完全實作下一步 SHoppingCart.aspx 頁面。 如果應該擲出例外狀況錯誤。

目前我們還沒有實作全域錯誤處理常式使這個例外狀況會形成未處理的應用程式，但我們將會解決這個問題很快。

也請注意使用陳述式 （可透過 Debug.Fail() `using System.Diagnostics;)`

是偵錯工具內執行應用程式，這個方法會顯示詳細的對話方塊，以及我們所指定的錯誤訊息的應用程式狀態的相關資訊。

當生產環境中執行陳述式會忽略 Debug.Fail()。

您會發現我們購物車類別名稱"GetShoppingCartId 」 中的方法呼叫上方的程式碼。

加入程式碼以實作方法，如下所示。

請注意，我們也新增更新] 和 [簽出按鈕和標籤，我們可以在其中顯示購物車 」 總數 」。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

我們現在可以將項目加入我們的購物車，但我們尚未實作邏輯，以新增一項產品之後，顯示購物車。

因此，MyShoppingCart.aspx 頁面中我們將新增 EntityDataSource 控制項和 GridVire 控制項，如下所示。

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

表單設計工具中呼叫，以便您可以按兩下更新購物車 按鈕，並產生在標記中宣告中指定的 click 事件處理常式。

我們稍後將實作詳細資料，但如此一來我們建置並執行我們的應用程式，而且未發生錯誤。

當您執行應用程式，並放入購物車新增項目時，您會看到這。

![](tailspin-spyworks-part-5/_static/image2.jpg)

請注意，我們必須偏離 「 預設 」 方格顯示藉由實作三個自訂資料行。

第一個是可編輯 」、 「 數量 」 繫結 」 欄位：

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

下一步 會顯示一行的項目總數 （項目成本時間來排序的數量） 的 「 計算 」 資料行：

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

最後，我們會有包含使用者要用來表示應該從購物圖表中移除之項目的核取方塊控制項的自訂資料行。

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

如您所見，總計列讓我們是空的順序就會加入一些邏輯來計算訂單總計。

我們先將 MyShoppingCart 類別實作 」 GetTotal 」 方法。

MyShoppingCart.cs 檔案中新增下列程式碼。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

然後在頁面\_Load 事件處理常式，我們可以呼叫我們 GetTotal 方法。 同時，我們將新增至購物車是否為空，並據以調整顯示，如果是測試。

現在如果是空的購物車出現這樣：

![](tailspin-spyworks-part-5/_static/image4.jpg)

如果不是，我們會看到我們的總計。

![](tailspin-spyworks-part-5/_static/image5.jpg)

不過，此頁面尚未完成。

我們需要其他邏輯來重新計算購物車，藉由移除標記為要移除的項目，以及藉由判斷新的量值，因為部分可能已變更方格中的使用者。

可讓 「 RemoveItem 」 方法加入購物車類別 MyShoppingCart.cs 來處理此案例，當使用者將標示為移除項目中。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

現在讓我們 ad 方法來處理的情況下，當使用者只需變更排序 GridView 裡的品質。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

使用中位置的基本移除和更新功能，我們可以實作實際更新購物車在資料庫中的邏輯。 （在 MyShoppingCart.cs)

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

您會注意到這個方法需要兩個參數。 一個是購物車識別碼，另一個是使用者定義型別之物件的陣列。

以我們的邏輯，在使用者介面特性上的相依性降到最低，我們已經定義了我們可以將購物車項目的程式碼，而我們無須直接存取 GridView 控制項的方法不使用的資料結構。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

在我們 MyShoppingCart.aspx.cs 檔案我們可以使用此結構在我們更新按鈕 Click 事件處理常式中，如下所示。 請注意，除了更新購物車我們將更新的購物車總計。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

請注意特別需要注意這行程式碼：

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

GetValues() 是特殊的協助程式函式，我們會在實作 MyShoppingCart.aspx.cs，如下所示。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

這提供明確的方法，以存取我們的 GridView 控制項繫結項目的值。 因為我們的 [移除項目] 核取方塊控制項未繫結，所以我們會透過 FindControl() 方法來進行存取。

在這個階段，在您的專案開發過程中，我們會準備實作簽出程序。

這麼做讓我們先使用 Visual Studio 產生的成員資格資料庫，並將使用者新增至成員資格儲存機制。

> [!div class="step-by-step"]
> [上一頁](tailspin-spyworks-part-4.md)
> [下一頁](tailspin-spyworks-part-6.md)
