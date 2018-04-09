---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: 第 5 部分： 商務邏輯 |Microsoft 文件
author: JoeStagner
description: 此教學課程系列詳細列出所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。 第 5 部分加入某個商務邏輯。
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: e4342e634ef8c4bcf4e0085650a28f414ab23736
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="part-5-business-logic"></a>第 5 部分： 商務邏輯
====================
由[Joe stagner 以](https://github.com/JoeStagner)

> Tailspin Spyworks 示範建立功能強大、 可調整的應用程式的.NET 平台是如何 hierarchy 簡單。 它會顯示如何使用 ASP.NET 4 中的強大的新功能，建置線上商店，包括購物、 簽出，以及管理關閉。
> 
> 此教學課程系列詳細列出所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。 第 5 部分加入某個商務邏輯。


## <a id="_Toc260221671"></a>  加入一些商務邏輯

我們想要我們購物的經驗，只要有人瀏覽我們的網站才能使用。 訪客可以瀏覽並加入至購物車的項目，即使未註冊或登入。 在準備簽出時即會獲得驗證選項，如果它們不是尚未成員將會讓它們能夠建立帳戶。

這表示我們必須實作邏輯，以購物車將匿名的狀態轉換為 「 已註冊的使用者 」 狀態。

讓我們來建立名為 「 類別 」 的目錄，然後在資料夾上按一下滑鼠右鍵並建立新的 「 類別 」 檔案，名為 MyShoppingCart.cs

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

如先前所述實作 MyShoppingCart.aspx 頁面的類別會加以擴充，我們會執行此作業使用。網路的功能強大的 「 部分類別 」 建構。

產生的呼叫我們 MyShoppingCart.aspx.cf 檔案看起來像這樣。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

請注意 「 部分 」 關鍵字的使用。

我們剛才產生的類別檔案如下所示。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

我們會將部分的關鍵字加入這個檔案以及合併我們實作。

我們新的類別檔案現在看起來像這樣。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

我們會將它加入至類別的第一個方法是 「 AddItem"方法。 這是將當使用者按一下 產品清單 和 產品詳細資料頁面的 「 新增圖案 」 連結最後呼叫的方法。

使用新增下列陳述式，在頁面頂端。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

並將此方法加入至 MyShoppingCart 類別。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

我們使用 LINQ to Entities 項目已在購物車中。 如果因此，我們可以更新訂單數量的項目，否則我們會建立新的項目選取的項目

若要呼叫這個方法中，我們會實作類別的這個方法不僅接著會顯示目前的項目加入後購物 = 購物車之 AddToCart.aspx 頁面。

以滑鼠右鍵按一下 [方案總管] 中的方案名稱，並加入與新命名 AddToCart.aspx，因為我們先前做過的頁面。

時要顯示暫時結果類似等低內建的問題，我們的實作中，我們可以使用此頁面，頁面將實際上不會呈現，而是呼叫 「 新增 」 邏輯和重新導向。

若要完成此我們會將下列程式碼新增至頁面\_載入事件。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

請注意，我們會擷取從查詢字串參數，然後呼叫類別的 AddItem 方法新增至購物車的產品。

不假設任何錯誤發生，則控制權會傳遞給我們將會完整實作下一步 SHoppingCart.aspx 頁面。 如果應該要有錯誤，我們會擲回例外狀況。

目前我們還沒有實作全域錯誤處理常式，此例外狀況將會進入未處理的應用程式，但我們將會很快解決這個。

也請注意使用陳述式 （可透過 Debug.Fail() `using System.Diagnostics;)`

是在偵錯工具執行應用程式，這個方法會顯示詳細的對話方塊，以及我們在此指定的錯誤訊息的應用程式狀態的相關資訊。

當生產環境中執行陳述式會忽略 Debug.Fail()。

您會注意到我們購物車類別名稱"GetShoppingCartId 」 中的方法呼叫上方的程式碼。

加入程式碼來實作的方法，如下所示。

請注意，我們也新增更新] 和 [簽出按鈕和標籤，我們可以在其中顯示購物車 」 總數 」。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

我們現在可以將項目加入我們的購物車，但我們未實作已加入一項產品之後顯示購物車的邏輯。

因此，在 MyShoppingCart.aspx 頁面我們要加入 EntityDataSource 控制項和 GridVire 控制項，如下所示。

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

在設計工具中的表單，如此您可以按兩下更新購物車 5d; 按鈕，並產生在標記宣告中所指定的 click 事件處理常式呼叫。

我們稍後會實作詳細資料，但這樣讓我們建置並執行應用程式不會發生錯誤。

當您執行應用程式，並新增至購物車的項目將會看見這。

![](tailspin-spyworks-part-5/_static/image2.jpg)

請注意我們有萬一從 「 預設 」 方格顯示藉由實作三個自訂的資料行。

第一個是編輯 」、 「 數量 」 繫結 」 欄位：

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

下一步是顯示的行項目總數 （項目成本時間來排序的數量） 的 「 計算 」 資料行：

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

最後，我們有自訂的資料行包含使用者要用來表示應該從購物圖表中移除之項目的核取方塊控制項。

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

如您所見，總計列讓我們是空的順序就會加入某些邏輯，來計算訂單總計。

我們第一次將我們 MyShoppingCart 類別實作 」 GetTotal"方法。

MyShoppingCart.cs 檔案中加入下列程式碼。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

然後在頁面\_Load 事件處理常式，我們可以呼叫 GetTotal 方法。 在相同的時間，我們會新增是否為空的購物車，並據以調整顯示畫面，如果是測試。

現在如果是空的購物車我們取得這：

![](tailspin-spyworks-part-5/_static/image4.jpg)

而且，如果不是，我們會了解我們的總計。

![](tailspin-spyworks-part-5/_static/image5.jpg)

不過，此頁面尚未完成。

我們需要額外邏輯，以重新計算購物車，藉由移除標記為要移除的項目，以及藉由判斷新的量值，因為某些可能已經變更方格中的使用者。

可讓 「 RemoveItem"方法加入購物車類別 MyShoppingCart.cs 以處理的情況，當使用者將標示為移除的項目中。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

現在讓我們 ad 方法以處理當時的情況，當使用者只要變更要在 GridView 中排序的品質。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

我們可以就地基本移除和更新功能實作實際更新購物車資料庫中的邏輯。 （在 MyShoppingCart.cs)

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

您會注意到這個方法必須要有兩個參數。 其中一個是購物車識別碼，另一個是使用者定義類型之物件的陣列。

以我們需使用者介面的特定項目的邏輯的相依性降到最低，我們已定義資料結構，我們將傳遞至我們的程式碼的購物車項目，而我們需要直接存取 GridView 控制項的方法不可以使用。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

在我們 MyShoppingCart.aspx.cs 檔案我們可以使用此結構在我們的更新按鈕 Click 事件處理常式，如下所示。 請注意，除了更新購物車我們將更新的購物車總計。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

請注意與特別需要注意這行程式碼：

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

GetValues() 是特殊的協助程式函式，我們將在實作 MyShoppingCart.aspx.cs，如下所示。

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

這提供明確的方法，以存取我們的 GridView 控制項中的繫結項目值。 因為我們的 「 移除項目 」 核取方塊控制項未繫結，所以我們會透過 FindControl() 方法來進行存取。

在這個階段您的專案開發中，我們會準備實作在結帳程序。

這樣讓我們之前使用 Visual Studio 產生的成員資格資料庫，並將使用者加入成員資格儲存機制。

> [!div class="step-by-step"]
> [上一頁](tailspin-spyworks-part-4.md)
> [下一頁](tailspin-spyworks-part-6.md)
