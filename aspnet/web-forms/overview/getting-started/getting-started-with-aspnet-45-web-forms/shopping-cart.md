---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
title: 購物車 |Microsoft Docs
author: Erikre
description: 本系列教學課程將教導您建置使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for 我們的 ASP.NET Web Forms 應用程式的基本概念...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 6898c601-6c31-432f-8388-e6843f8a17cb
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
msc.type: authoredcontent
ms.openlocfilehash: 22253b8708efd9ce505c9fbeb9cb2e942588b37d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375774"
---
<a name="shopping-cart"></a>購物車
====================
藉由[Erik Reitan](https://github.com/Erikre)

[下載 Wingtip Toys 範例專案 (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)或[下載電子書 (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> 本系列教學課程將教導您建置使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for Web 的 ASP.NET Web Forms 應用程式的基本概念。 Visual Studio 2013[含有 C# 原始程式碼專案](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)隨附了本教學課程系列。


本教學課程說明將購物車新增 「 Wingtip Toys 範例 ASP.NET Web Forms 應用程式所需的商務邏輯。 此教學課程的上一個教學課程 [顯示資料的項目和詳細資料]，並且是 Wingtip 玩具店教學課程系列的一部分。 當您完成本教學課程中時，您的範例應用程式的使用者可以新增、 移除及修改客戶購物車中的產品。

## <a name="what-youll-learn"></a>您將學到什麼：

1. 如何建立 web 應用程式的購物車。
2. 如何讓使用者將項目加入至購物車。
3. 如何新增[GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview(v=vs.110).aspx#introduction)控制項來顯示購物車 」 詳細資料。
4. 如何計算並顯示訂單總計。
5. 如何移除和更新購物車中的項目。
6. 如何加入購物車 」 計數器。

## <a name="code-features-in-this-tutorial"></a>在本教學課程的程式碼功能：

1. Entity Framework Code First
2. 資料註釋
3. 強型別資料控制項
4. 模型繫結

## <a name="creating-a-shopping-cart"></a>建立購物車

稍早在本教學課程系列中，您可以加入頁面和程式碼，以檢視資料庫中的產品資料。 在本教學課程中，您將建立購物車管理產品的使用者有興趣購買。 使用者將能夠瀏覽並放入購物車新增項目，即使未註冊或登入。 若要管理購物車存取權，您會將使用者指派的唯一`ID`第一次使用全域唯一識別碼 (GUID)，當使用者存取購物車。 想要儲存這`ID`使用 ASP.NET 工作階段狀態。

> [!NOTE] 
> 
> ASP.NET 工作階段狀態是方便的地方來儲存使用者專屬資訊的使用者離開網站就會到期。 雖然不當使用工作階段狀態可以有較大的網站上的效能影響，受到使用的工作階段狀態的運作也供示範之用。 Wingtip Toys 範例專案會示範如何使用工作階段狀態而不需要外部提供者，其中的工作階段狀態是儲存在同處理序裝載站台的 web 伺服器上。 提供的應用程式的多個執行個體的大型網站或在不同的伺服器執行的應用程式的多個執行個體的站台，請考慮使用**Windows Azure 快取服務**。 此快取服務提供分散式快取服務是在外部網站，可解決使用同處理序工作階段狀態的問題。 如需詳細資訊，請參閱[如何使用 ASP.NET 工作階段狀態與 Windows Azure 網站](https://docs.microsoft.com/azure/redis-cache/cache-aspnet-session-state-provider)。


### <a name="add-cartitem-as-a-model-class"></a>新增 CartItem 做為模型類別

稍早在本教學課程系列中，您必須定義的分類和產品資料的結構描述建立`Category`並`Product`中的類別*模型*資料夾。 現在，加入新的類別，來定義購物車的結構描述。 稍後在本教學課程中，您會將類別來處理資料的存取權`CartItem`資料表。 這個類別會提供商務邏輯，以新增、 移除及更新購物車中的項目。

1. 以滑鼠右鍵按一下*模型*資料夾，然後選取**新增** - &gt; **新項目**。 

    ![「 購物車 」-新的項目](shopping-cart/_static/image1.png)
2. 隨即顯示 [ 新增項目] 對話方塊。 選取 **程式碼**，然後選取**類別**。 

    ![「 購物車 」-加入新項目 對話方塊](shopping-cart/_static/image2.png)
3. 這個新類別命名*CartItem.cs*。
4. 按一下 [加入] 。  
   新的類別檔案會顯示在編輯器中。
5. 取代為下列程式碼中的預設程式碼：   

    [!code-csharp[Main](shopping-cart/samples/sample1.cs)]

`CartItem`類別包含的結構描述會定義每個使用者新增至購物車的產品。 這個類別是與其他您稍早在本教學課程系列中建立的結構描述類別類似。 依照慣例，Entity Framework Code First 所預期的主索引鍵`CartItem`資料表會`CartItemId`或`ID`。 不過，程式碼會覆寫預設行為使用資料註解`[Key]`屬性。 `Key` ItemId 屬性的屬性會指定`ItemID`屬性是主索引鍵。

`CartId`屬性會指定`ID`来購買的項目相關聯的使用者。 您將新增程式碼來建立此使用者`ID`當使用者存取購物車。 這`ID`也會儲存為 ASP.NET 工作階段變數。

### <a name="update-the-product-context"></a>更新的產品內容

除了新增`CartItem`類別，您必須更新資料庫內容類別，可管理的實體類別，並提供對資料庫的資料存取。 若要這樣做，您將新建`CartItem`模型類別來`ProductContext`類別。

1. 在 **方案總管**，尋找並開啟*ProductContext.cs*中的檔案*模型*資料夾。
2. 將反白顯示的程式碼加入*ProductContext.cs*檔案，如下所示：  

    [!code-csharp[Main](shopping-cart/samples/sample2.cs?highlight=14)]

如先前在本教學課程系列中的程式碼中所述*ProductContext.cs*檔都會將`System.Data.Entity`命名空間，所以您需要的 Entity framework 的所有核心功能的存取權。 這項功能包括查詢、 插入、 更新和刪除資料，藉由使用強型別物件的功能。 `ProductContext`類別加入至新加入的存取`CartItem`模型類別。

### <a name="managing-the-shopping-cart-business-logic"></a>管理購物車商務邏輯

接下來，您將建立`ShoppingCart`類別的新*邏輯*資料夾。 `ShoppingCart`類別處理的資料存取`CartItem`資料表。 類別也會包含商務邏輯，來新增、 移除及更新購物車中的項目。

您將加入購物車邏輯將會包含的功能來管理下列動作：

1. 將項目加入購物車
2. 移除購物車中的項目
3. 取得此購物車識別碼
4. 正在擷取項目從購物車 」
5. 加總所有的購物車項目數量
6. 更新的購物車資料

購物車 頁面 (*ShoppingCart.aspx*) 和購物車類別會一起用來存取購物車資料。 購物車 頁面會顯示所有使用者新增至購物車的項目。 Besides 購物車網頁和類別，您將建立的頁面 (*AddToCart.aspx*) 若要將產品加入購物車。 您也將加入程式碼*ProductList.aspx*頁面並*ProductDetails.aspx*會提供連結的頁面*AddToCart.aspx*頁面上，好讓使用者可以加入放入購物車的產品。

下圖顯示當使用者加入至購物車的產品，就會發生的基本程序。

![「 購物車 」-新增至購物車](shopping-cart/_static/image3.png)

當使用者按一下**加入購物車**上的連結*ProductList.aspx*頁面或*ProductDetails.aspx*  頁面上，應用程式會瀏覽至*AddToCart.aspx*頁面，然後自動為*ShoppingCart.aspx*頁面。 *AddToCart.aspx*頁面將選取的產品加入購物車 ShoppingCart 類別中呼叫方法。 *ShoppingCart.aspx*頁面會顯示已新增至購物車的產品。

#### <a name="creating-the-shopping-cart-class"></a>建立購物車類別

`ShoppingCart`類別會新增到不同的資料夾中的應用程式，以便將會有明顯的區分模型 （[模型] 資料夾）、 （根資料夾） 的頁面和邏輯 （邏輯資料夾）。

1. 在 [**方案總管] 中**，以滑鼠右鍵按一下**WingtipToys**專案，然後選取**新增**-&gt;**新資料夾**. 將新資料夾命名*邏輯*。
2. 以滑鼠右鍵按一下*邏輯*資料夾，然後選取**新增** - &gt; **新項目**。
3. 加入新的類別檔案，名為*ShoppingCartActions.cs*。
4. 取代為下列程式碼中的預設程式碼：   

    [!code-csharp[Main](shopping-cart/samples/sample3.cs)]

`AddToCart`方法可讓要納入購物車的產品為基礎的個別產品`ID`。 產品加入購物車中，或如果購物車已經包含該產品的項目，就會遞增數量。

`GetCartId`方法會傳回購物車`ID`使用者。 購物車`ID`用來追蹤使用者具有客戶購物車中的項目。 如果使用者沒有現有的購物車`ID`，新車`ID`為其建立。 如果使用者身分登入已註冊的使用者，購物車`ID`設為其使用者名稱。 不過，如果使用者未登入時，購物車`ID`設為唯一的值 (GUID)。 GUID 可確保該只有一個購物車會建立每個使用者工作階段為基礎。

`GetCartItems`方法會傳回一份 「 購物車項目的使用者。 稍後在本教學課程中，您會看到模型繫結，用來顯示購物車的項目購物車 」 使用`GetCartItems`方法。

### <a name="creating-the-add-to-cart-functionality"></a>建立加入--車功能

如先前所述，您將建立名為處理網頁*AddToCart.aspx* ，用以將新產品加入購物車的使用者。 此頁面會呼叫`AddToCart`方法中的`ShoppingCart`您剛才建立的類別。 *AddToCart.aspx*頁面就會預期收到的產品`ID`傳遞給它。 此產品`ID`呼叫時，會使用`AddToCart`方法中的`ShoppingCart`類別。

> [!NOTE] 
> 
> 您將修改程式碼後置 (*AddToCart.aspx.cs*) 的這個頁面上，而不是頁面的 UI (*AddToCart.aspx*)。


#### <a name="to-create-the-add-to-cart-functionality"></a>若要建立購物車新增功能：

1. 在 **方案總管 中**，以滑鼠右鍵按一下**WingtipToys**專案，按一下 **新增** - &gt; **新項目**。  
   隨即顯示 [ 新增項目] 對話方塊。
2. 將標準的新頁面 (Web Form) 新增至名為應用程式*AddToCart.aspx*。 

    ![「 購物車 」-加入 Web Form](shopping-cart/_static/image4.png)
3. 在 **方案總管**，以滑鼠右鍵按一下*AddToCart.aspx*頁面，然後按一下**檢視程式碼**。 *AddToCart.aspx.cs*在編輯器中開啟程式碼後置檔案。
4. 取代現有的程式碼中*AddToCart.aspx.cs*以下列程式碼後置：   

    [!code-csharp[Main](shopping-cart/samples/sample4.cs)]

當*AddToCart.aspx*載入頁面時，產品`ID`會從查詢字串。 接下來，購物車 」 類別的執行個體，並用來呼叫`AddToCart`您稍早在本教學課程中新增的方法。 `AddToCart`方法，包含在*ShoppingCartActions.cs*檔案中，包含邏輯，可將選取的產品加入購物車或遞增的所選產品的產品數量。 如果尚未加入購物車產品，產品會新增至`CartItem`資料庫資料表。 如果產品已經加入至購物車，使用者會新增額外的項目相同產品之產品數量會在遞增`CartItem`資料表。 最後，頁面重新導向回到*ShoppingCart.aspx*將在下一個步驟中，其中使用者會看到更新的購物車中的項目清單的頁面。

如先前所述，使用者`ID`用來識別特定使用者相關聯的產品。 這`ID`中的資料列加入`CartItem`資料表每次使用者加入至購物車的產品。

### <a name="creating-the-shopping-cart-ui"></a>建立購物車 UI

*ShoppingCart.aspx*頁面會顯示使用者已新增至購物車的產品。 它也會提供能夠新增、 移除及更新購物車中的項目。

1. 在 **方案總管 中**，以滑鼠右鍵按一下**WingtipToys**，按一下 **新增** - &gt; **新項目**。  
   隨即顯示 [ 新增項目] 對話方塊。
2. 加入新的頁面 (Web Form)，其中包含所選取的主版頁面**使用主版頁面的 Web Form**。 將新頁面命名*ShoppingCart.aspx*。
3. 選取  **Site.Master**附加至新建立的主版頁面 *.aspx*頁面。
4. 在  *ShoppingCart.aspx*頁面上，以下列標記取代現有的標記：   

    [!code-aspx[Main](shopping-cart/samples/sample5.aspx)]

*ShoppingCart.aspx*頁面包含**GridView**控制項，名為`CartList`。 這個控制項將從資料庫繫結的購物車資料使用模型繫結**GridView**控制項。 當您設定`ItemType`的屬性**GridView**控制項，資料繫結運算式`Item`位於控制項和控制項的標記會變成強型別。 如稍早在本教學課程系列中所述，您可以選取 詳細資料的`Item`物件使用 IntelliSense。 若要設定資料控制項來使用模型繫結選取資料，您將`SelectMethod`控制項的屬性。 在上述的標記，您可以設定`SelectMethod`使用 GetShoppingCartItems 方法，這個方法會傳回一份`CartItem`物件。 **GridView**資料控制項在網頁生命週期中的適當時間呼叫的方法，並自動繫結傳回的資料。 `GetShoppingCartItems`必須仍會加入方法。

#### <a name="retrieving-the-shopping-cart-items"></a>正在擷取購物車項目

接下來，您可以加入程式碼*ShoppingCart.aspx.cs*程式碼後置擷取並填入購物車 UI。

1. 在 **方案總管**，以滑鼠右鍵按一下*ShoppingCart.aspx*頁面，然後按一下**檢視程式碼**。 *ShoppingCart.aspx.cs*在編輯器中開啟程式碼後置檔案。
2. 將現有的程式碼取代為下列程式碼：  

    [!code-csharp[Main](shopping-cart/samples/sample6.cs)]

如前文所述，`GridView`資料控制呼叫`GetShoppingCartItems`方法在適當的時間，在頁面生命週期，並自動將傳回的資料繫結。 `GetShoppingCartItems`方法建立的執行個體`ShoppingCartActions`物件。 然後，程式碼會使用該執行個體傳回購物車中的項目，藉由呼叫`GetCartItems`方法。

### <a name="adding-products-to-the-shopping-cart"></a>加入至購物車的產品

當任一*ProductList.aspx*或*ProductDetails.aspx*頁面顯示時，使用者將能夠將產品加入購物車的連結。 當他們按一下連結時，應用程式瀏覽至名為處理頁面*AddToCart.aspx*。 *AddToCart.aspx*頁面會呼叫`AddToCart`方法中的`ShoppingCart`您稍早在本教學課程中新增的類別。

現在，您可以新增**加入購物車**連結，同時*ProductList.aspx*頁面和*ProductDetails.aspx*頁面。 此連結會包含產品`ID`，從資料庫擷取。

1. 在 **方案總管**，尋找並開啟名為頁面*ProductList.aspx*。
2. 新增以黃色反白顯示的標記*ProductList.aspx*頁面上，使整個頁面出現，如下所示：  

    [!code-aspx[Main](shopping-cart/samples/sample7.aspx?highlight=50-54)]

### <a name="testing-the-shopping-cart"></a>測試購物車

執行以查看您如何將產品加入購物車應用程式。

1. 按 **F5** 執行應用程式。  
 專案重新建立資料庫之後，瀏覽器會開啟並顯示*Default.aspx*頁面。
2. 選取 **汽車**類別瀏覽功能表。  
 *ProductList.aspx*頁面會顯示 [Cars] 分類中包含的產品。 

    ![「 購物車 」-汽車](shopping-cart/_static/image5.png)
3. 按一下 **加入購物車**第一項產品旁邊的連結列 (轉換 car)。   
 *ShoppingCart.aspx*顯示頁面，顯示購物車 」 中的選取項目。 

    ![「 購物車 」-購物車](shopping-cart/_static/image6.png)
4. 選取以檢視其他產品**平面**類別瀏覽功能表。
5. 按一下 **加入購物車**旁邊所列的第一個產品的連結。  
 *ShoppingCart.aspx*頁面會顯示額外的項目。
6. 關閉瀏覽器。

### <a name="calculating-and-displaying-the-order-total"></a>計算並顯示訂單總計

除了產品加入購物車中，您會新增`GetTotal`方法，以`ShoppingCart`類別，並在 [購物車] 頁面中顯示訂單總金額。

1. 在 [**方案總管] 中**，開啟*ShoppingCartActions.cs*中的檔案*邏輯*資料夾。
2. 新增下列`GetTotal`方法以黃色反白顯示`ShoppingCart`類別，使類別看起來像這樣：   

    [!code-csharp[Main](shopping-cart/samples/sample8.cs?highlight=85-97)]

首先，`GetTotal`方法取得使用者的購物車識別碼。 然後方法會取得購物車總計購物車中所列每個產品的產品數量乘以產品價格。

> [!NOTE] 
> 
> 上述程式碼會使用可為 null 的型別 」`int?`"。 可為 null 的型別可以代表基礎類型，同時也為 null 值的所有值。 如需詳細資訊，請參閱[使用可為 Null 的型別](https://msdn.microsoft.com/library/2cf62fcy(v=vs.110).aspx)。


### <a name="modify-the-shopping-cart-display"></a>修改購物車顯示

接下來，您將修改的程式碼*ShoppingCart.aspx*頁面，即可呼叫`GetTotal`方法，以及總的顯示*ShoppingCart.aspx*頁面載入頁面時。

1. 在 **方案總管**，以滑鼠右鍵按一下*ShoppingCart.aspx*頁面，然後選取**檢視程式碼**。
2. 在  *ShoppingCart.aspx.cs*檔案中，更新`Page_Load`處理常式，加入下列程式碼以黃色反白顯示：   

    [!code-csharp[Main](shopping-cart/samples/sample9.cs?highlight=16-31)]

當*ShoppingCart.aspx*頁面載入時，會載入購物車物件並呼叫，以擷取購物車總計`GetTotal`方法`ShoppingCart`類別。 購物車是空的如果該效果會顯示訊息。

### <a name="testing-the-shopping-cart-total"></a>測試的購物車總計

執行應用程式現在以查看您如何可以不只將產品加入購物車，但您所見的購物車總計。

1. 按 **F5** 執行應用程式。  
 瀏覽器會開啟並顯示*Default.aspx*頁面。
2. 選取 **汽車**類別瀏覽功能表。
3. 按一下 **加入購物車**第一項產品旁邊的連結。   
 *ShoppingCart.aspx*頁面會顯示訂單總計。 

    ![「 購物車 」-車總計](shopping-cart/_static/image7.png)
4. 加入購物車中的某些其他產品 （例如平面）。
5. *ShoppingCart.aspx*頁面會顯示已更新的總計，您已新增的所有產品。 

    ![「 購物車 」-有很多產品](shopping-cart/_static/image8.png)
6. 關閉瀏覽器視窗以停止執行中應用程式。

### <a name="adding-update-and-checkout-buttons-to-the-shopping-cart"></a>將更新和簽出 按鈕加入至購物車

若要允許使用者修改購物車，您將新增**更新** 按鈕並**簽出**購物車 頁面的按鈕。 **簽出**按鈕不會等到稍後在本教學課程系列。

1. 在 [**方案總管] 中**，開啟*ShoppingCart.aspx* web 應用程式專案的根目錄中的頁面。
2. 新增**更新** 按鈕並**簽出**按鈕來*ShoppingCart.aspx*頁面上，新增至現有的標記，黃色反白顯示的標記中所示下列程式碼：   

    [!code-aspx[Main](shopping-cart/samples/sample10.aspx?highlight=36-45)]

當使用者按一下**更新** 按鈕，`UpdateBtn_Click`會呼叫事件處理常式。 這個事件處理常式會呼叫您將在下一個步驟新增的程式碼。

接下來，您可以在此更新中所包含的程式碼*ShoppingCart.aspx.cs*檔案，以循環購物車項目並呼叫`RemoveItem`和`UpdateItem`方法。

1. 在 [**方案總管] 中**，開啟*ShoppingCart.aspx.cs* web 應用程式專案的根目錄中的檔案。
2. 新增以黃色反白顯示的下列程式碼區段*ShoppingCart.aspx.cs*檔案：   

    [!code-csharp[Main](shopping-cart/samples/sample11.cs?highlight=9-11,33,44-89)]

當使用者按一下**更新**按鈕*ShoppingCart.aspx*  頁面上，會呼叫 UpdateCartItems 方法。 UpdateCartItems 方法會取得在購物車中的每個項目之更新的值。 接著，UpdateCartItems 方法會呼叫`UpdateShoppingCartDatabase`方法 （新增和下一個步驟中所述） 以新增或移除購物車中的項目。 一旦資料庫更新以反映放入購物車中，更新**GridView**控制項，會藉由呼叫更新購物車 頁面上`DataBind`方法**GridView**。 此外，在購物車 頁面上的訂單總金額會更新以反映更新的項目清單。

### <a name="updating-and-removing-shopping-cart-items"></a>更新與移除購物車項目

在 [ *ShoppingCart.aspx* ] 頁面上，您可以看到更新的項目數量，以及移除項目已加入的控制項。 現在，加入將使用這些控制項的程式碼。

1. 在 [**方案總管] 中**，開啟*ShoppingCartActions.cs*中的檔案*邏輯*資料夾。
2. 新增下列程式碼中以黃色反白顯示*ShoppingCartActions.cs*類別檔案：   

    [!code-csharp[Main](shopping-cart/samples/sample12.cs?highlight=99-213)]

`UpdateShoppingCartDatabase`方法，從呼叫`UpdateCartItems`方法*ShoppingCart.aspx.cs*頁面上，包含邏輯，可更新或移除購物車中的項目。 `UpdateShoppingCartDatabase`方法逐一查看購物車 」 清單中的所有資料列。 如果要移除已標示的購物車項目或數量小於一，`RemoveItem`呼叫方法。 否則，購物車項目會檢查更新時`UpdateItem`呼叫方法。 已移除或更新購物車項目之後，資料庫會儲存變更。

`ShoppingCartUpdates`結構用來保存所有的購物車項目。 `UpdateShoppingCartDatabase`方法會使用`ShoppingCartUpdates`結構，以判斷任何項目，是要更新或移除。

在下一個教學課程中，您將使用`EmptyCart`方法，以清除 購物車後購買產品。 但現在，您將使用`GetCount`剛加入的方法*ShoppingCartActions.cs*檔案，以判斷購物車中有多少項目。

### <a name="adding-a-shopping-cart-counter"></a>加入購物車計數器

若要允許使用者檢視購物車中的項目總數，將會加入至計數器*Site.Master*頁面。 這個計數器也將當成放入購物車的連結。

1. 在 **方案總管**，開啟*Site.Master*頁面。
2. 修改標記加入購物車計數器連結，如黃色的導覽 」 一節中所示，使其出現，如下所示：  

    [!code-html[Main](shopping-cart/samples/sample13.html?highlight=6)]
3. 接下來，更新的程式碼後置*Site.Master.cs*檔案加上黃色反白顯示，如下所示的程式碼：  

    [!code-csharp[Main](shopping-cart/samples/sample14.cs?highlight=11,77-84)]

頁面會轉譯為 HTML 之前,`Page_PreRender`就會引發事件。 在 `Page_PreRender`處理常式，購物車的總數取決於呼叫`GetCount`方法。 傳回的值加入至`cartCount`標記中包含的範圍*Site.Master*頁面。 `<span>`標記可讓內部的項目正確呈現。 當站台的任何頁面顯示時，將會顯示購物車總計。 使用者也可以按一下以顯示購物車的購物車總計。

## <a name="testing-the-completed-shopping-cart"></a>測試已完成的購物車

購物車 」 中，您可以執行的應用程式現在以查看您可以新增、 刪除和更新項目。 購物車總計將會反映在購物車中的所有項目的總成本。

1. 按 **F5** 執行應用程式。  
 瀏覽器隨即開啟並顯示*Default.aspx*頁面。
2. 選取 **汽車**類別瀏覽功能表。
3. 按一下 **加入購物車**第一項產品旁邊的連結。   
 *ShoppingCart.aspx*頁面會顯示訂單總計。
4. 選取 **平面**類別瀏覽功能表。
5. 按一下 **加入購物車**第一項產品旁邊的連結。
6. 設定為 3 的購物車中的第一個項目的數量，然後選取**移除的項目**第二個項目的核取方塊。<a id="a"></a>
7. 按一下 [**更新**按鈕以更新購物車] 頁面，並顯示新的訂單總數。 

    ![購物車-車更新](shopping-cart/_static/image9.png)

## <a name="summary"></a>總結

在本教學課程中，您已建立購物車 Wingtip Toys Web Form 範例應用程式。 在本教學課程中，您已使用 Entity Framework Code First 資料註解、 強型別的資料控制項，模型繫結。

購物車支援加入、 刪除和更新的使用者已選擇要購買的項目。 除了實作的購物車 」 功能，您已了解如何顯示在 購物車項目的**GridView**控制，並計算訂單總計。

## <a name="addition-information"></a>其他資訊

[ASP.NET 工作階段狀態概觀](https://msdn.microsoft.com/library/ms178581.aspx)

> [!div class="step-by-step"]
> [上一頁](display_data_items_and_details.md)
> [下一頁](checkout-and-payment-with-paypal.md)
