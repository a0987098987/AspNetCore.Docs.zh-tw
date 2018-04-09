---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: 第 8 部分： 購物車 Ajax 更新 |Microsoft 文件
author: jongalloway
description: 此教學課程系列詳細列出所有建置 ASP.NET MVC 商店範例應用程式所採取的步驟。 第 8 部涵蓋使用 Ajax 更新購物車。
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: 195c01ff0d71b2bfd0c00e71244d47a166330921
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="part-8-shopping-cart-with-ajax-updates"></a>第 8 部： 與 Ajax 更新購物車
====================
由[Jon Galloway](https://github.com/jongalloway)

> MVC Music Store 是介紹並逐步說明如何使用 ASP.NET MVC 和 Visual Studio 針對 web 程式開發的教學課程應用程式。  
>   
> MVC Music Store 是輕量級範例存放區實作銷售音樂專輯上線，並實作基本的網站管理、 使用者登入和購物車的功能。  
>   
> 此教學課程系列詳細列出所有建置 ASP.NET MVC 商店範例應用程式所採取的步驟。 第 8 部涵蓋使用 Ajax 更新購物車。


我們會讓使用者能夠將購物車中的專輯未註冊，但他們需要註冊為來賓完成簽出。 在購物與簽出程序會分成兩個控制站： ShoppingCart 控制器以允許以匿名方式將項目新增至購物車和簽出控制器用來處理在結帳程序。 我們將這一節，購物車的開頭，然後建置在結帳程序下, 一節。

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a>加入購物車、 順序和 OrderDetail 的模型類別

我們的購物車及結帳程序將使用的一些新的類別。 以滑鼠右鍵按一下 [模型] 資料夾，並加入下列程式碼的購物車類別 (Cart.cs)。

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

這個類別是我們目前為止，使用 [RecordId] 屬性 [Key] 屬性外的其他項目非常類似。 我們的購物車項目將具有名為 允許匿名購物 CartID 字串識別項，但資料表包含名為 RecordId 的主索引鍵的整數。 依照慣例，名為購物車的資料表的主索引鍵將會為 CartId 或識別碼，但我們可以輕鬆地覆寫的註解或程式碼透過如果我們想要必須要有 Entity Framework 程式碼優先。 這是我們要如何使用簡單的慣例在 Entity Framework 程式碼優先當它們符合我們的範例，但我們正在不受限於它們時不會。

接下來，加入下列程式碼的 Order 類別 (Order.cs)。

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

此類別會追蹤訂單的摘要和傳遞資訊。 **它不會在尚未編譯**，因為它相依於尚未建立我們類別 OrderDetails 導覽屬性。 讓我們來修正問題，現在將類別命名為 OrderDetail.cs，加入下列程式碼。

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

我們要將一個上次更新對我們 MusicStoreEntities 類別，以包含會將公開這些新的模型類別，也包括 DbSet DbSets&lt;演出者&gt;。 更新的 MusicStoreEntities 類別會顯示為下面。

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a>管理購物車商務邏輯

接下來，我們將建立 ShoppingCart 類別 Models 資料夾中。 ShoppingCart 模型處理購物車資料表資料存取。 此外，它會處理商務邏輯來新增和移除購物車的項目。

因為我們不想要求使用者登入帳戶，只是為了將項目新增至購物車，我們將會指派使用者的暫存的唯一識別碼 （使用 GUID 或全域唯一識別碼） 當他們存取購物車。 我們會將儲存這個識別碼使用 ASP.NET 工作階段類別。

*注意： ASP.NET 工作階段是方便的位置來儲存使用者特定的資訊將到期後他們離開網站。不當使用的工作階段狀態可能會影響效能較大的站台上，而淺使用將適用於示範用途。*

ShoppingCart 類別會公開下列方法：

**AddToCart**採用相簿做為參數，並將它加入至使用者的購物車。 購物車資料表會追蹤每個相簿的數量，因為它包含邏輯，以視需要建立新的資料列，或如果使用者已排序過的相簿的一個複本，只要遞增數量。

**RemoveFromCart**採用專輯識別碼，並將它從使用者的購物車移除。 如果使用者只會有一份專輯購物車中，會移除資料列。

**EmptyCart**移除使用者的購物車中的所有項目。

**GetCartItems**擷取 CartItems 清單來顯示或處理。

**GetCount**擷取的使用者具有及其購物車中的專輯總數。

**GetTotal**計算總成本的購物車中的所有項目。

**CreateOrder**簽出階段期間會將訂單購物車。

**GetCart**是靜態方法可讓我們來取得購物車物件的控制站。 它會使用**GetCartId**方法以處理 CartId 讀取使用者的工作階段。 GetCartId 方法需要 HttpContextBase，使它能夠讀取使用者的 CartId 從使用者的工作階段。

以下是完整**ShoppingCart 類別**:

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a>ViewModels

我們的購物車控制器需要一些複雜資訊傳達給它的檢視不會清楚對應到我們的模型物件。 我們不想修改以符合我們的檢視; 我們模型模型類別應該代表我們的網域使用者介面。 一個解決方式是將資訊傳遞至我們使用 ViewBag 類別，如我們所做的存放區管理員下拉式清單中的資訊，但傳遞的大量資訊透過 ViewBag 取得難以管理的檢視。

對此的解決方案是使用*ViewModel*模式。 使用此模式時我們建立強型別的類別，最佳化的特定檢視案例中，以及其公開之動態值/內容我們檢視範本所需的屬性。 我們控制器類別可以填入並將這些檢視最佳化類別傳遞給我們要使用的檢視範本。 這可讓類型安全、 編譯時期檢查，以及編輯器 IntelliSense 中檢視範本。

我們將建立兩個檢視的模型，以供我們的購物車控制器中： ShoppingCartViewModel 將保存內容的使用者的購物車，並在使用者移除的項目時顯示確認資訊用於 ShoppingCartRemoveViewModel從購物車。

為了讓事情變組織受測專案的根目錄中，我們來建立新的 ViewModels 資料夾。 以滑鼠右鍵按一下專案，選取 新增 / 新資料夾。

![](mvc-music-store-part-8/_static/image1.jpg)

將資料夾命名為 ViewModels。

![](mvc-music-store-part-8/_static/image1.png)

接下來，加入 ShoppingCartViewModel 類別 ViewModels 資料夾中。 它有兩個屬性： 購物車的項目，且在購物車中保留的所有項目總價的十進位值的清單。

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

現在加入 ShoppingCartRemoveViewModel ViewModels 資料夾，具有下列四個屬性。

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a>購物車控制器

購物車控制器有三個主要用途： 將項目加入至購物車、 從購物車，移除項目和檢視購物車中的項目。 如此可讓我們使用三個類別的剛建立： ShoppingCartViewModel、 ShoppingCartRemoveViewModel 和 ShoppingCart。 如 StoreController 及 StoreManagerController 中，我們會將新增欄位以保留 MusicStoreEntities 的執行個體。

使用空白的控制器範本專案中加入新的購物車控制器。

![](mvc-music-store-part-8/_static/image2.png)

以下是完成 ShoppingCart 控制器。 索引和加入控制器動作應該不會感到陌生。 移除和 CartSummary 控制器的動作處理兩個特殊情況下，我們將在下一節中討論。

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a>使用 jQuery Ajax 更新

接下來，我們將建立的購物車索引頁面 ShoppingCartViewModel 強型別，並使用清單檢視的範本使用之前的相同方法。

![](mvc-music-store-part-8/_static/image3.png)

不過，而不是使用 Html.ActionLink 移除購物車的項目，我們會使用 jQuery 「 連接 」 的這個檢視中的所有連結具有 HTML 類別 RemoveLink click 事件。 而不是張貼的表單，此 click 事件處理常式將只會對 AJAX 回呼我們 RemoveFromCart 控制器動作。 RemoveFromCart 傳回 JSON 序列化的結果，我們 jQuery 回呼然後剖析，並執行四個快速更新使用 jQuery 頁面：

- 1. 從清單中移除已刪除的相簿
- 2. 更新購物車中的計數標頭
- 3. 向使用者顯示更新訊息
- 4. 更新購物車總價格

移除案例都會由索引檢視表中的 Ajax 回撥，因為我們不需要額外的檢視 RemoveFromCart 動作。 以下是 /ShoppingCart/Index 檢視的完整程式碼：

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

若要測試時，我們需要能夠將項目加入至我們的購物車。 我們會將更新我們**存放區詳細資料**檢視，以加入 [新增至購物車] 按鈕。 當我們在到達時，我們可以包含一些我們已加入的專輯額外資訊的自我們上次更新此檢視： 內容類型、 演出者、 價格和專輯封面。 更新的存放區詳細資料檢視程式碼會出現如下所示。

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

現在我們可以按一下存放區，而測試加入和移除與我們的購物車的專輯。 執行應用程式，並瀏覽至存放區索引。

![](mvc-music-store-part-8/_static/image4.png)

接下來，按一下以檢視專輯的清單類型。

![](mvc-music-store-part-8/_static/image5.png)

在專輯標題上按一下 立即顯示我們已更新的專輯詳細資料檢視，包括 「 新增至購物車 」 按鈕。

![](mvc-music-store-part-8/_static/image6.png)

按一下 新增至購物車 」 按鈕顯示我們的購物車索引檢視與購物車摘要清單。

![](mvc-music-store-part-8/_static/image7.png)

之後載入您的購物車，您可以按一下移除購物車連結以查看您的購物車的 Ajax 更新。

![](mvc-music-store-part-8/_static/image8.png)

我們已建置出購物車，可讓使用者取消註冊項目加入購物車可運作。 在下列區段中，我們將允許註冊，並且完成簽出程序。


> [!div class="step-by-step"]
> [上一頁](mvc-music-store-part-7.md)
> [下一頁](mvc-music-store-part-9.md)
