---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
title: 購物車 |Microsoft 文件
author: Erikre
description: 此教學課程將告訴您建置使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for 我們的 ASP.NET Web Form 應用程式的基本概念...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 6898c601-6c31-432f-8388-e6843f8a17cb
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
msc.type: authoredcontent
ms.openlocfilehash: a8e96da7737cdf649575711a464c4f7726cb6ded
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890716"
---
<a name="shopping-cart"></a>購物車
====================
由[Erik Reitan](https://github.com/Erikre)

[下載 Wingtip Toys 範例專案 (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)或[下載電子書 (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> 此教學課程將告訴您建置一個使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for Web 的 ASP.NET Web Form 應用程式的基本概念。 Visual Studio 2013[與 C# 原始程式碼的專案](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)隨附此教學課程了。


本教學課程將描述加入購物車 Wingtip Toys 範例 ASP.NET Web Form 應用程式所需的商務邏輯。 本教學課程上一個教學課程 」 顯示資料的項目和詳細資料 > 為基礎，而且是 Wingtip 玩具存放區的教學課程系列的一部分。 當您已經完成本教學課程中時，範例應用程式的使用者將能夠加入、 移除和修改客戶購物車中的產品。

## <a name="what-youll-learn"></a>您將學習：

1. 如何建立 web 應用程式的購物車。
2. 如何啟用使用者加入至購物車的項目。
3. 如何新增[GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview(v=vs.110).aspx#introduction)控制項來顯示購物車詳細資料。
4. 如何計算並顯示訂單總計。
5. 如何移除和更新購物車中的項目。
6. 如何加入購物車計數器。

## <a name="code-features-in-this-tutorial"></a>在本教學課程的程式碼功能：

1. Entity Framework Code First
2. 資料註釋
3. 強型別資料控制項
4. 模型繫結

## <a name="creating-a-shopping-cart"></a>建立購物車

稍早在本教學課程系列，您可以加入頁面和程式碼來檢視資料庫中的產品資料。 在此教學課程中，您將建立購物車管理產品的使用者有興趣購買。 使用者可以瀏覽並加入至購物車的項目，即使未註冊或登入。 若要管理購物車存取，您會將使用者指派唯一`ID`使用全域唯一識別碼 (GUID)，當使用者存取購物車第一次。 想要儲存這`ID`使用 ASP.NET 工作階段狀態。

> [!NOTE] 
> 
> ASP.NET 工作階段狀態是最方便的位置儲存在使用者離開網站將過期的使用者特定資訊。 不當使用的工作階段狀態可能會影響效能較大的站台上，而淺使用工作階段狀態的運作方式以及供示範之用。 Wingtip Toys 範例專案會示範如何使用其中的工作階段狀態是預存同處理序在裝載站台的網頁伺服器上沒有外部提供者的工作階段狀態。 對於較大的站台提供的應用程式的多個執行個體或不同伺服器執行的應用程式的多個執行個體的站台，請考慮使用**Windows Azure 快取服務**。 此快取服務提供分散式快取服務的外部網站，並解決使用同處理序工作階段狀態的問題。 如需詳細資訊，請參閱[如何使用 ASP.NET 工作階段狀態與 Windows Azure Web Sites](https://docs.microsoft.com/azure/redis-cache/cache-aspnet-session-state-provider)。


### <a name="add-cartitem-as-a-model-class"></a>新增 CartItem 做為模型類別

稍早在本教學課程系列，您必須定義的分類和產品資料的結構描述建立`Category`和`Product`中的類別*模型*資料夾。 現在，加入新類別來定義購物車的結構描述。 稍後在本教學課程中，您會新增一個類別來處理的資料存取`CartItem`資料表。 這個類別會提供商務邏輯，以新增、 移除和更新購物車中的項目。

1. 以滑鼠右鍵按一下*模型*資料夾，然後選取**新增** - &gt; **新項目**。 

    ![購物車-新項目](shopping-cart/_static/image1.png)
2. 隨即顯示 [ 新增項目] 對話方塊。 選取**程式碼**，然後選取**類別**。 

    ![購物車-加入新項目 對話方塊](shopping-cart/_static/image2.png)
3. 這個新類別命名*CartItem.cs*。
4. 按一下 [加入] 。  
   在編輯器中，會顯示新的類別檔案。
5. 下列程式碼取代預設程式碼：   

    [!code-csharp[Main](shopping-cart/samples/sample1.cs)]

`CartItem`類別包含會定義每個使用者新增至購物車的產品的結構描述。 這個類別是與其他您稍早在本教學課程系列中建立的結構描述類別類似。 根據慣例，Entity Framework Code First 預期的主索引鍵`CartItem`資料表會`CartItemId`或`ID`。 不過，程式碼會覆寫預設行為所使用資料註解`[Key]`屬性。 `Key` ItemId 屬性的屬性會指定`ItemID`屬性是主索引鍵。

`CartId`屬性會指定`ID`来購買的項目相關聯的使用者。 您要加入程式碼來建立此使用者`ID`當使用者存取購物車。 這`ID`也會儲存為 ASP.NET 工作階段變數。

### <a name="update-the-product-context"></a>更新的產品內容

除了新增`CartItem`類別，您將需要更新管理的實體類別，以及提供資料存取資料庫的資料庫內容類別。 若要這樣做，您將加入新建立`CartItem`模型類別來`ProductContext`類別。

1. 在**方案總管 中**、 尋找和開啟*ProductContext.cs*檔案*模型*資料夾。
2. 將反白顯示的程式碼加入*ProductContext.cs*檔案，如下所示：  

    [!code-csharp[Main](shopping-cart/samples/sample2.cs?highlight=14)]

如先前所述，本教學課程系列中的程式碼*ProductContext.cs*檔都會將`System.Data.Entity`命名空間，所以您需要存取的 Entity framework 的核心功能。 此功能包括查詢、 插入、 更新和刪除資料，藉由使用強型別物件的能力。 `ProductContext`類別加入至新加入的存取`CartItem`模型類別。

### <a name="managing-the-shopping-cart-business-logic"></a>管理購物車商務邏輯

接下來，您將建立`ShoppingCart`類別中的新*邏輯*資料夾。 `ShoppingCart`類別處理進行資料存取`CartItem`資料表。 類別也會包含商務邏輯，來新增、 移除和更新購物車中的項目。

您將加入購物車邏輯將會包含的功能來管理下列動作：

1. 將項目加入至購物車
2. 移除購物車的項目
3. 取得此購物車識別碼
4. 正在擷取從購物車的項目
5. 加總所有的購物車項目數量
6. 更新購物車資料

購物車網頁 (*ShoppingCart.aspx*) 和購物車類別會一起用來存取購物車資料。 購物車網頁會顯示使用者加入至購物車的項目。 除了購物車的頁面和類別，您將建立頁面 (*AddToCart.aspx*) 若要將產品加入購物車。 您也會加入程式碼以*ProductList.aspx*頁面和*ProductDetails.aspx*頁面將提供的連結*AddToCart.aspx*頁面上，好讓使用者可以加入放入購物車的產品。

下圖顯示發生於使用者將產品加入購物車的基本程序。

![購物車-新增至購物車](shopping-cart/_static/image3.png)

當使用者按一下**加入購物車**連結上的*ProductList.aspx*頁面或*ProductDetails.aspx*  頁面上，應用程式將會瀏覽至*AddToCart.aspx*頁面，然後自動為*ShoppingCart.aspx*頁面。 *AddToCart.aspx*頁面將選取的產品加入購物車透過 ShoppingCart 類別中呼叫的方法。 *ShoppingCart.aspx*頁面會顯示已新增至購物車的產品。

#### <a name="creating-the-shopping-cart-class"></a>建立購物車類別

`ShoppingCart`類別會加入至不同的資料夾中的應用程式，如此才會明確區分模型 （Models 資料夾）、 （根資料夾） 的頁面和邏輯 （邏輯資料夾）。

1. 在**方案總管 中**，以滑鼠右鍵按一下**WingtipToys**專案，然後選取**新增**-&gt;**新資料夾**. 將新的資料夾命名*邏輯*。
2. 以滑鼠右鍵按一下*邏輯*資料夾，然後選取**新增** - &gt; **新項目**。
3. 加入新的類別檔案命名為*ShoppingCartActions.cs*。
4. 下列程式碼取代預設程式碼：   

    [!code-csharp[Main](shopping-cart/samples/sample3.cs)]

`AddToCart`方法可讓個別產品加入購物車的產品為基礎`ID`。 產品加入購物車，或如果購物車中已包含該產品的項目，就會遞增數量。

`GetCartId`方法會傳回購物車`ID`使用者。 購物車`ID`用來追蹤使用者具有及其購物車中的項目。 如果使用者沒有現有的購物車`ID`，新車`ID`建立它們。 如果使用者登入做為已註冊的使用者，購物車`ID`會設為其使用者名稱。 不過，如果使用者並未登入購物車`ID`設為唯一的數值 (GUID)。 GUID 可確保該只有一個購物車會為每個使用者，根據工作階段建立。

`GetCartItems`方法傳回的購物車項目，使用者清單。 稍後在本教學課程中，您會看到模型繫結會用來顯示購物車的項目在購物車中使用`GetCartItems`方法。

### <a name="creating-the-add-to-cart-functionality"></a>建立加入--車功能

如先前所述，您將建立名為的處理頁面*AddToCart.aspx* ，將會用來將新的產品加入購物車的使用者。 此頁面將會呼叫`AddToCart`方法中的`ShoppingCart`您剛才建立的類別。 *AddToCart.aspx*頁面就會預期收到的產品`ID`傳遞給它。 此產品`ID`呼叫時將使用`AddToCart`方法中的`ShoppingCart`類別。

> [!NOTE] 
> 
> 您將修改程式碼後置 (*AddToCart.aspx.cs*) 的這個頁面上，而不是頁面的 UI (*AddToCart.aspx*)。


#### <a name="to-create-the-add-to-cart-functionality"></a>若要建立購物車新增功能：

1. 在**方案總管] 中**，以滑鼠右鍵按一下**WingtipToys**專案中，按一下 [**新增** - &gt; **新項目**。  
   隨即顯示 [ 新增項目] 對話方塊。
2. 加入指定的應用程式的標準新的頁面 （Web 表單） *AddToCart.aspx*。 

    ![購物車-加入 Web 表單](shopping-cart/_static/image4.png)
3. 在**方案總管 中**，以滑鼠右鍵按一下*AddToCart.aspx*頁面，然後按一下**檢視程式碼**。 *AddToCart.aspx.cs*在編輯器中開啟程式碼後置檔案。
4. 取代現有的程式碼中*AddToCart.aspx.cs*以下列程式碼後置：   

    [!code-csharp[Main](shopping-cart/samples/sample4.cs)]

當*AddToCart.aspx*網頁載入時，產品`ID`擷取自查詢字串。 接下來，購物車類別的執行個體是建立並用來呼叫`AddToCart`您稍早在本教學課程中加入的方法。 `AddToCart`中所包含的方法*ShoppingCartActions.cs*檔案中，包含邏輯，可將選取的產品加入購物車或遞增的所選產品的產品數量。 如果尚未加入購物車產品，產品會新增到`CartItem`資料庫資料表。 如果產品已經加入至購物車，使用者將額外的項目相同產品的產品數量就會遞增中`CartItem`資料表。 最後，頁面會重新導向回*ShoppingCart.aspx*頁面，您會加入在下一個步驟中，其中使用者會看到更新的購物車中的項目清單。

如先前所述，使用者`ID`用來識別與特定使用者相關聯的產品。 這`ID`中的資料列會加入`CartItem`資料表每次使用者加入至購物車的產品。

### <a name="creating-the-shopping-cart-ui"></a>建立購物車 UI

*ShoppingCart.aspx*頁面將顯示的使用者新增至購物車的產品。 它也會提供了可新增、 移除和更新購物車中的項目。

1. 在**方案總管] 中**，以滑鼠右鍵按一下**WingtipToys**，按一下 [**新增** - &gt; **新項目**。  
   隨即顯示 [ 新增項目] 對話方塊。
2. 加入新的頁面 （Web 表單），其中包含所選取的主版頁面**使用主版頁面的 Web Form**。 將新頁面*ShoppingCart.aspx*。
3. 選取**Site.Master**附加到新建立的主版頁面 *.aspx*頁面。
4. 在*ShoppingCart.aspx*頁面上，以下列標記取代現有的標記：   

    [!code-aspx[Main](shopping-cart/samples/sample5.aspx)]

*ShoppingCart.aspx*頁面包含**GridView**控制項，名為`CartList`。 此控制項會使用模型繫結將從資料庫繫結的購物車資料**GridView**控制項。 當您將`ItemType`屬性**GridView**控制項，資料繫結運算式`Item`位於控制項和控制項的標記會變成強型別。 如稍早在本教學課程中所述，您可以選取的詳細資料`Item`物件使用 IntelliSense。 若要設定要用來選取資料的模型繫結資料控制項，您將設定`SelectMethod`控制項的屬性。 在上述標記中，您將設定`SelectMethod`使用 GetShoppingCartItems 方法，這個方法會傳回一份`CartItem`物件。 **GridView**資料控制項在頁面生命週期中的適當時間呼叫方法，並自動將傳回的資料繫結。 `GetShoppingCartItems`仍會加入方法。

#### <a name="retrieving-the-shopping-cart-items"></a>正在擷取購物車項目

接下來，您將程式碼加入*ShoppingCart.aspx.cs*程式碼後置擷取並填入購物車 UI。

1. 在**方案總管 中**，以滑鼠右鍵按一下*ShoppingCart.aspx*頁面，然後按一下**檢視程式碼**。 *ShoppingCart.aspx.cs*在編輯器中開啟程式碼後置檔案。
2. 將現有的程式碼取代為下列程式碼：  

    [!code-csharp[Main](shopping-cart/samples/sample6.cs)]

如上所述，`GridView`資料控制呼叫`GetShoppingCartItems`方法頁面生命週期中的適當時間循環，並自動繫結傳回的資料。 `GetShoppingCartItems`方法建立的執行個體`ShoppingCartActions`物件。 然後，程式碼會使用該執行個體傳回購物車中的項目，藉由呼叫`GetCartItems`方法。

### <a name="adding-products-to-the-shopping-cart"></a>新增至購物車的產品

當任一*ProductList.aspx*或*ProductDetails.aspx*頁面隨即顯示，使用者可以將產品加入購物車使用的連結。 當使用者按一下連結時，應用程式瀏覽至名為處理頁面*AddToCart.aspx*。 *AddToCart.aspx*頁面將會呼叫`AddToCart`方法中的`ShoppingCart`您稍早在本教學課程中加入的類別。

現在，您將新增**加入購物車**同時連結*ProductList.aspx*頁面和*ProductDetails.aspx*頁面。 此連結會包含產品`ID`，從資料庫擷取。

1. 在**方案總管 中**、 尋找和開啟此頁面*ProductList.aspx*。
2. 加入以黃色反白顯示標記*ProductList.aspx*頁面上，讓整個頁面隨即出現，如下所示：  

    [!code-aspx[Main](shopping-cart/samples/sample7.aspx?highlight=50-54)]

### <a name="testing-the-shopping-cart"></a>測試購物車

執行以查看您如何將產品加入購物車應用程式。

1. 按 **F5** 執行應用程式。  
 專案會重新建立資料庫之後，瀏覽器會開啟並顯示*Default.aspx*頁面。
2. 選取**汽車**類別瀏覽功能表中。  
 *ProductList.aspx*頁面會顯示，其中包含 「 汽車 」 分類的產品。 

    ![購物車為輛汽車](shopping-cart/_static/image5.png)
3. 按一下**加入購物車**第一次產品旁邊的連結列 (轉換 car)。   
 *ShoppingCart.aspx*頁面隨即顯示，顯示您購物車中的選取項目。 

    ![購物車-車](shopping-cart/_static/image6.png)
4. 選取以檢視其他產品**平面**類別瀏覽功能表中。
5. 按一下**加入購物車**旁邊所列的第一個產品連結。  
 *ShoppingCart.aspx*頁面會顯示額外的項目。
6. 關閉瀏覽器。

### <a name="calculating-and-displaying-the-order-total"></a>計算並顯示訂單總計

除了新增至購物車的產品，您將加入`GetTotal`方法`ShoppingCart`類別，並在購物車網頁中顯示訂單總金額。

1. 在**方案總管 中**，開啟*ShoppingCartActions.cs*檔案*邏輯*資料夾。
2. 加入下列`GetTotal`方法以黃色反白顯示`ShoppingCart`類別，以便此類別會顯示，如下所示：   

    [!code-csharp[Main](shopping-cart/samples/sample8.cs?highlight=85-97)]

首先，`GetTotal`方法取得使用者的購物車識別碼。 然後方法會取得購物車總產品單價乘以購物車中所列每項產品的產品數量。

> [!NOTE] 
> 
> 上述程式碼會使用可為 null 的型別"`int?`"。 可為 null 的型別可以代表基礎類型，同時也為 null 值的所有值。 如需詳細資訊，請參閱[使用可為 Null 的型別](https://msdn.microsoft.com/library/2cf62fcy(v=vs.110).aspx)。


### <a name="modify-the-shopping-cart-display"></a>修改購物車顯示

接下來您將修改的程式碼*ShoppingCart.aspx*頁面，即可呼叫`GetTotal`方法，以及總的顯示*ShoppingCart.aspx*頁面載入頁面時。

1. 在**方案總管 中**，以滑鼠右鍵按一下*ShoppingCart.aspx*頁面，然後選取**檢視程式碼**。
2. 在*ShoppingCart.aspx.cs*檔案中，更新`Page_Load`處理常式中加入下列程式碼以黃色反白顯示：   

    [!code-csharp[Main](shopping-cart/samples/sample9.cs?highlight=16-31)]

當*ShoppingCart.aspx*頁面載入，則會載入購物車物件，並接著藉由呼叫會擷取購物車總計`GetTotal`方法`ShoppingCart`類別。 如果購物車是空的該效果會顯示訊息。

### <a name="testing-the-shopping-cart-total"></a>測試購物車總計

執行應用程式現在若要查看如何您無法只加入一項產品放入購物車，但您可以看到購物車總計。

1. 按 **F5** 執行應用程式。  
 瀏覽器會開啟並顯示*Default.aspx*頁面。
2. 選取**汽車**類別瀏覽功能表中。
3. 按一下**加入購物車**旁邊第一次的產品連結。   
 *ShoppingCart.aspx*頁面會顯示訂單總計。 

    ![購物車-車總計](shopping-cart/_static/image7.png)
4. 加入購物車中的某些其他產品 （例如，平面）。
5. *ShoppingCart.aspx*頁面會顯示已更新的總計，已新增的所有產品。 

    ![購物車的多項產品](shopping-cart/_static/image8.png)
6. 關閉瀏覽器視窗以停止執行中應用程式。

### <a name="adding-update-and-checkout-buttons-to-the-shopping-cart"></a>將更新和簽出按鈕加入至購物車

若要允許使用者修改購物車，您會加入**更新**按鈕和**簽出**購物車網頁的按鈕。 **簽出**按鈕不會等到稍後在本教學課程系列。

1. 在**方案總管 中**，開啟*ShoppingCart.aspx* web 應用程式專案的根目錄中的頁面。
2. 若要加入**更新**按鈕和**簽出**按鈕*ShoppingCart.aspx*頁面上，新增至現有的標記，黃色反白顯示的標記中所示下列程式碼：   

    [!code-aspx[Main](shopping-cart/samples/sample10.aspx?highlight=36-45)]

當使用者按一下**更新** 按鈕，`UpdateBtn_Click`會呼叫事件處理常式。 這個事件處理常式會呼叫在下一個步驟中加入的程式碼。

接下來，您可以在此更新中包含的程式碼*ShoppingCart.aspx.cs*迴圈購物車的項目並呼叫檔案`RemoveItem`和`UpdateItem`方法。

1. 在**方案總管 中**，開啟*ShoppingCart.aspx.cs*根目錄中的 web 應用程式專案的檔案。
2. 加入下列程式碼區段，以黃色反白顯示*ShoppingCart.aspx.cs*檔案：   

    [!code-csharp[Main](shopping-cart/samples/sample11.cs?highlight=9-11,33,44-89)]

當使用者按一下**更新**按鈕*ShoppingCart.aspx*  頁面上，會呼叫 UpdateCartItems 方法。 UpdateCartItems 方法購物車中取得每個項目之更新的的值。 接著，UpdateCartItems 方法會呼叫`UpdateShoppingCartDatabase`方法 （新增下, 一個步驟所述) 以新增或移除購物車的項目。 一旦資料庫已更新以反映放入購物車，更新**GridView**控制會藉由呼叫更新購物車網頁`DataBind`方法**GridView**。 此外，在購物車網頁上的總訂單金額會更新以反映更新的項目清單。

### <a name="updating-and-removing-shopping-cart-items"></a>更新與移除購物車的項目

在*ShoppingCart.aspx*頁面上，您可以看到更新項目的數量和移除項目已加入控制項。 現在，加入程式碼，可讓這些工作的控制項。

1. 在**方案總管 中**，開啟*ShoppingCartActions.cs*檔案*邏輯*資料夾。
2. 加入下列程式碼中以黃色反白顯示*ShoppingCartActions.cs*類別檔案：   

    [!code-csharp[Main](shopping-cart/samples/sample12.cs?highlight=99-213)]

`UpdateShoppingCartDatabase`方法，從呼叫`UpdateCartItems`方法*ShoppingCart.aspx.cs*頁面上，包含邏輯，可更新或移除購物車的項目。 `UpdateShoppingCartDatabase`方法會逐一購物車清單內的所有資料列。 如果為購物車的項目已標示為要移除或數量小於一，`RemoveItem`方法呼叫。 否則，購物車項目會檢查更新時`UpdateItem`方法呼叫。 購物車項目已移除或更新之後，資料庫會儲存變更。

`ShoppingCartUpdates`結構用來保存所有的購物車項目。 `UpdateShoppingCartDatabase`方法會使用`ShoppingCartUpdates`結構，以判斷任何項目是否需要更新或移除。

在下一個教學課程中，您將使用`EmptyCart`方法，以清除購物車之後購買產品。 但現在，您會使用`GetCount`剛加入的方法*ShoppingCartActions.cs*檔案，以判斷購物車中有多少項目。

### <a name="adding-a-shopping-cart-counter"></a>加入購物車計數器

若要允許使用者檢視購物車中的項目總數，您將加入至計數器*Site.Master*頁面。 此計數器也將當成放入購物車的連結。

1. 在**方案總管 中**，開啟*Site.Master*頁面。
2. 修改標記加入購物車計數器連結中所示瀏覽區段的黃色讓它出現，如下所示：  

    [!code-html[Main](shopping-cart/samples/sample13.html?highlight=6)]
3. 接下來，更新程式碼後置的*Site.Master.cs*檔案中加入程式碼中反白顯示黃色，如下所示：  

    [!code-csharp[Main](shopping-cart/samples/sample14.cs?highlight=11,77-84)]

頁面轉譯為 HTML 之前,`Page_PreRender`就會引發事件。 在`Page_PreRender`處理常式，購物車的總數取決於呼叫`GetCount`方法。 傳回的值加入至`cartCount`範圍中的標記包含*Site.Master*頁面。 `<span>`標記可讓內部的項目正確呈現。 顯示站台的任何頁面時，將會顯示購物車總計。 使用者也可以按一下要顯示購物車的購物車總計。

## <a name="testing-the-completed-shopping-cart"></a>測試已完成的購物車

您可以在購物車中執行應用程式現在若要查看您可以加入、 刪除和更新項目。 購物車總計會反映在購物車中的所有項目的總成本。

1. 按 **F5** 執行應用程式。  
 瀏覽器隨即開啟並顯示*Default.aspx*頁面。
2. 選取**汽車**類別瀏覽功能表中。
3. 按一下**加入購物車**旁邊第一次的產品連結。   
 *ShoppingCart.aspx*頁面會顯示訂單總計。
4. 選取**平面**類別瀏覽功能表中。
5. 按一下**加入購物車**旁邊第一次的產品連結。
6. 設定為 3 購物車中的第一個項目的數量，然後選取**移除項目**第二個項目的核取方塊。<a id="a"></a>
7. 按一下**更新**按鈕，以更新購物車網頁，並顯示新的訂單總計。 

    ![購物車-車更新](shopping-cart/_static/image9.png)

## <a name="summary"></a>總結

在本教學課程中，您已建立購物車 Wingtip Toys Web Form 範例應用程式。 在本教學課程中，您已使用 Entity Framework Code First、 資料註解、 強型別的資料控制項和模型繫結。

購物車支援加入、 刪除和更新使用者選取購買的項目。 除了實作的購物車功能，您已經學會如何顯示在購物車項目**GridView**控制，並計算訂單總金額。

## <a name="addition-information"></a>其他資訊

[ASP.NET 工作階段狀態概觀](https://msdn.microsoft.com/library/ms178581.aspx)

> [!div class="step-by-step"]
> [上一頁](display_data_items_and_details.md)
> [下一頁](checkout-and-payment-with-paypal.md)
