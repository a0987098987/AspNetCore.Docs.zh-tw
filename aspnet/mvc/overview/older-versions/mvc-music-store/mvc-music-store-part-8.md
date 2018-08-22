---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: 第 8 節： 購物車與 Ajax 更新 |Microsoft Docs
author: jongalloway
description: 本教學課程系列會詳細說明所有建置 ASP.NET MVC Music 市集範例應用程式所採取的步驟。 第 8 節涵蓋了購物車與 Ajax 更新。
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: cab338e56505c453532a26d794eb7bf4e94555a9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824703"
---
<a name="part-8-shopping-cart-with-ajax-updates"></a>第 8 節： 購物車與 Ajax 更新
====================
藉由[Jon Galloway](https://github.com/jongalloway)

> MVC Music 市集是介紹，並逐步說明如何使用 ASP.NET MVC 和 Visual Studio 進行 web 開發的教學課程應用程式。  
>   
> MVC Music 市集是銷售線上音樂 album 和實作基本的網站管理、 使用者登入時，和 「 購物車 」 功能的輕量級的範例存放區實作。  
>   
> 本教學課程系列會詳細說明所有建置 ASP.NET MVC Music 市集範例應用程式所採取的步驟。 第 8 節涵蓋了購物車與 Ajax 更新。


我們要允許使用者將專輯購物車中，而不註冊，但他們必須註冊為完成簽出的來賓。 將購物和簽出程序會分成兩個控制站： 允許以匿名方式將項目加入購物車，ShoppingCart 控制器和簽出控制器用來處理結帳程序。 我們將開始在此區段中，「 購物車 」，並建置下一節中的簽出程序。

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a>加入購物車、 訂單及 OrderDetail 的模型類別

我們的購物車及結帳程序將使用的一些新的類別。 以滑鼠右鍵按一下 [模型] 資料夾，並加入購物車類別 (Cart.cs) 為下列程式碼。

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

這個類別是非常類似於我們到目前為止，使用 [RecordId] 屬性的 [金鑰] 屬性除外的其他項目。 我們的購物車項目會具有名為允許匿名購物，CartID 的字串識別項，但資料表包含名為記錄識別碼的整數主索引鍵。 依照慣例，Entity Framework Code First 所預期，名為購物車的資料表的主索引鍵會 CartId 或識別碼，但我們可以輕鬆地覆寫，透過註解] 或 [程式碼如果我們想。 這是我們要如何使用簡單的慣例在 Entity Framework Code First 時他們符合我們的範例，但我們正在不受限於它們時沒有。

接下來，新增下列程式碼的 Order 類別 (Order.cs)。

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

此類別會追蹤訂單的摘要和傳遞資訊。 **它不會在尚未編譯**，因為它有相依於類別，我們尚未建立 OrderDetails 導覽屬性。 讓我們修正問題，現在將類別命名為 OrderDetail.cs，新增下列程式碼。

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

我們要將一個最後的更新對我們 MusicStoreEntities 類別，以包含它們會公開這些新的模型類別，也包括 DbSet DbSets&lt;演出者&gt;。 更新的 MusicStoreEntities 類別會顯示為下面。

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a>管理 「 購物車 」 的商務邏輯

接下來，我們將建立 ShoppingCart 類別 [Models] 資料夾中。 ShoppingCart 模型處理購物車資料表的資料存取。 此外，它會處理的商務邏輯來新增和移除購物車中的項目。

因為我們不想要要求使用者登入帳戶，只是為了將項目加入購物車，我們會將使用者指派暫時的唯一識別碼 （使用 GUID 或全域唯一識別碼） 時存取購物車。 我們將會儲存這個使用 ASP.NET 工作階段類別的識別碼。

*注意： ASP.NET 工作階段是方便的地方來儲存使用者專屬資訊到期後他們離開網站。雖然不當使用工作階段狀態可以有較大的網站上的效能影響，我們淺使用適用於示範用途。*

ShoppingCart 類別會公開下列方法：

**AddToCart**會專輯做為參數，並將它新增至使用者的購物車。 購物車資料表會追蹤每一個 album 的數量，因為它會包含邏輯，以視需要建立新的資料列，或只是遞增的數量，如果使用者已排序過的相簿的一個複本。

**RemoveFromCart**會專輯識別碼，並移除使用者的購物車。 如果使用者只會有一份專輯購物車中，會移除資料列。

**EmptyCart**移除使用者的購物車中的所有項目。

**GetCartItems**擷取一份 CartItems 顯示或處理。

**GetCount**擷取的使用者具有客戶購物車中的專輯總數。

**GetTotal**計算總成本的購物車中的所有項目。

**CreateOrder**簽出階段期間會將訂單購物車。

**GetCart**是可讓我們來取得購物車物件的控制站的靜態方法。 它會使用**GetCartId**方法以處理 CartId 讀取使用者的工作階段。 GetCartId 方法需要 HttpContextBase，以便它可以讀取使用者的 CartId 從使用者的工作階段。

以下是完整**ShoppingCart 類別**:

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a>ViewModels

購物車控制器需要一些複雜資訊傳達給它的檢視不會完全對應到我們的模型物件。 我們不想要修改以符合我們的檢視; 我們模型模型類別應該代表我們的網域，使用者介面。 一個解決方案是將資訊傳遞至我們的檢視使用 ViewBag 類別，我們使用存放區管理員下拉式清單中的資訊，但透過 ViewBag 傳遞大量資訊取得難以管理。

此解決方案是使用*ViewModel*模式。 使用此模式時我們會建立強型別的類別，適用於我們的特定檢視案例，以及其公開屬性，動態值/內容所需的我們的檢視範本。 然後填入並將這些檢視最佳化的類別傳遞至我們的檢視範本，若要使用我們的控制器類別。 這可讓型別安全、 編譯時間檢查，以及檢視範本中的編輯器 IntelliSense。

我們將建立兩個檢視的模型，以供在我們的購物車控制器： ShoppingCartViewModel 會保留使用者的購物車，內容和 ShoppingCartRemoveViewModel 會用來顯示確認資訊，當使用者移除的項目從購物車。

我們為了簡化組織的專案根目錄中，讓我們建立一個新的 Viewmodel 資料夾。 以滑鼠右鍵按一下專案，選取 新增 / 新的資料夾。

![](mvc-music-store-part-8/_static/image1.jpg)

將資料夾命名為 Viewmodel。

![](mvc-music-store-part-8/_static/image1.png)

接下來，新增 ShoppingCartViewModel 類別 Viewmodel 資料夾中。 它有兩個屬性： 購物車項目和保存的所有項目總價購物車中的十進位值的清單。

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

現在加入 ShoppingCartRemoveViewModel Viewmodel 資料夾中，使用下列四個屬性。

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a>購物車控制器

「 購物車 」 控制器有三個主要用途： 項目加入購物車移除購物車中的項目，檢視購物車中的項目。 它會利用三個類別我們剛建立： ShoppingCartViewModel、 ShoppingCartRemoveViewModel 和 ShoppingCart。 例如，StoreController 和 StoreManagerController，我們將新增欄位以保留 MusicStoreEntities 的執行個體。

使用空白控制器範本的專案中加入新的 「 購物車 」 控制站。

![](mvc-music-store-part-8/_static/image2.png)

以下是完成 ShoppingCart 控制器。 索引 和 新增控制器動作看起來應該很熟悉。 移除和 CartSummary 控制器的動作會處理兩個特殊案例，我們將在下一節中討論。

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a>與 jQuery Ajax 更新

接下來，我們將建立的購物車索引頁面 ShoppingCartViewModel 強型別，並使用使用之前的相同方法的清單檢視範本。

![](mvc-music-store-part-8/_static/image3.png)

不過，而不是使用 Html.ActionLink 移除購物車中的項目，我們將使用 jQuery 來 「 連接 」 在此檢視中的所有連結具有 HTML 類別 RemoveLink 的 click 事件。 而不是張貼的表單，此 click 事件處理常式就只會使 AJAX 回呼至我們 RemoveFromCart 控制器動作。 RemoveFromCart 傳回 JSON 序列化的結果，我們 jQuery 回呼然後剖析，並執行四個頁面使用 jQuery 的快速更新：

- 1. 從清單中移除已刪除的相簿
- 2. 更新標頭中的購物車計數
- 3. 向使用者顯示更新訊息
- 4. 更新的購物車 」 總價格

因為移除案例都會由 Ajax 回呼，[索引] 檢視中，我們不需要額外的檢視 RemoveFromCart 動作。 以下是 /ShoppingCart/Index 檢視的完整程式碼：

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

若要測試時，我們需要能夠加入我們的購物車中的項目。 我們會更新我們**存放區詳細資料**檢視中包含 [新增至購物車] 按鈕。 雖然我們注意到這個問題，我們可以包含一些我們已新增的專輯額外資訊自我們上次更新此檢視： 內容類型、 演出者、 價格和專輯封面。 更新的存放區詳細資料檢視程式碼會出現，如下所示。

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

現在我們可以按一下 透過市集，並測試新增和移除專輯，與我們的購物車。 執行應用程式，並瀏覽至存放區索引。

![](mvc-music-store-part-8/_static/image4.png)

接下來，按一下以檢視相簿一份內容類型。

![](mvc-music-store-part-8/_static/image5.png)

現在按一下專輯標題會顯示我們已更新的專輯詳細資料檢視，包括 [新增至購物車] 按鈕。

![](mvc-music-store-part-8/_static/image6.png)

按一下 [新增至購物車] 按鈕會顯示我們的購物車索引檢視與購物車摘要清單。

![](mvc-music-store-part-8/_static/image7.png)

之後載入 「 購物車 」，您可以按一下從購物車連結移除，以查看您的購物車的 Ajax 更新。

![](mvc-music-store-part-8/_static/image8.png)

我們建置出運作中的 「 購物車 」 可讓未註冊的使用者將項目新增至購物車。 在下列區段中，我們將允許其註冊，並完成結帳程序。


> [!div class="step-by-step"]
> [上一頁](mvc-music-store-part-7.md)
> [下一頁](mvc-music-store-part-9.md)
