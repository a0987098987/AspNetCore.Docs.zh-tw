---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: 部分 9： 註冊並簽出 |Microsoft 文件
author: jongalloway
description: 此教學課程系列詳細列出所有建置 ASP.NET MVC 商店範例應用程式所採取的步驟。 第 9 部份涵蓋註冊和簽出。
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: e7e83b70f2508b6dfc0c078b992747a76e4d0ff2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="part-9-registration-and-checkout"></a>部分 9： 註冊並簽出
====================
由[Jon Galloway](https://github.com/jongalloway)

> MVC Music Store 是介紹並逐步說明如何使用 ASP.NET MVC 和 Visual Studio 針對 web 程式開發的教學課程應用程式。  
>   
> MVC Music Store 是輕量級範例存放區實作銷售音樂專輯上線，並實作基本的網站管理、 使用者登入和購物車的功能。  
>   
> 此教學課程系列詳細列出所有建置 ASP.NET MVC 商店範例應用程式所採取的步驟。 第 9 部份涵蓋註冊和簽出。


在本節中，我們將建立 CheckoutController 來收集購物者的地址和付款資訊。 我們會要求使用者註冊，簽入之前網站，讓此控制器將會需要授權。

使用者會巡覽至在結帳程序從購物車 「 結帳 」 按鈕，即可。

![](mvc-music-store-part-9/_static/image1.jpg)

如果使用者尚未登入中，將會提示他們。

![](mvc-music-store-part-9/_static/image1.png)

成功登入，使用者會再顯示位址 」 和 「 付款檢視。

![](mvc-music-store-part-9/_static/image2.png)

一旦他們有填滿表單，並送出訂單，它們將會顯示 [訂單確認] 畫面。

![](mvc-music-store-part-9/_static/image3.png)

嘗試檢視不存在順序或不屬於您的訂單會顯示檢視時發生錯誤。

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a>移轉購物車

雖然購物程序是匿名的當使用者簽出 5d; 按鈕時，他們必須註冊和登入。 使用者將預期我們會維持之間瀏覽，因此我們必須要與使用者產生關聯的購物車資訊，當他們完成註冊或登入及其購物車資訊。

這是執行動作，其實很簡單，因為我們 ShoppingCart 類別已經有方法，這個方法會將目前的購物車中的所有項目關聯的使用者名稱。 我們只需要呼叫這個方法，當使用者完成註冊或登入。

開啟**AccountController**我們所設定的成員資格和授權時，我們加入的類別。 加入 using 陳述式參考 MvcMusicStore.Models，然後加入下列 MigrateShoppingCart 方法：

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

接下來，修改登入後動作之後, 要呼叫 MigrateShoppingCart 已驗證使用者，如下所示：

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

進行相同變更註冊後動作，，已成功建立使用者帳戶之後，立即：

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

這是它-現在匿名的購物車會自動轉移至在成功註冊或登入的使用者帳戶。

## <a name="creating-the-checkoutcontroller"></a>建立 CheckoutController

以滑鼠右鍵按一下 [控制器] 資料夾，並將新的控制站新增至名為 CheckoutController 使用空白的控制器範本的專案。

![](mvc-music-store-part-9/_static/image5.png)

首先，新增控制器類別宣告，以要求使用者註冊，才能簽出上方 Authorize 屬性：

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

*注意： 這是類似於我們先前對 StoreManagerController，變更，但在此情況下 Authorize 屬性所需的使用者是系統管理員角色中。簽出控制器，我們正在要求使用者登入，但不需要它們由系統管理員。*

為了簡單起見，我們將不會處理與本教學課程中的付款資訊。 相反地，我們會讓使用者查看促銷代碼。 我們會儲存使用名為 PromoCode 常數此促銷代碼。

如同 StoreController，我們會宣告來保存 MusicStoreEntities 類別，名為 storeDB 的執行個體欄位。 為了讓 MusicStoreEntities 類別使用，我們需要加入 using MvcMusicStore.Models 命名空間陳述式。 簽出 controller 頂端會出現下方。

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

CheckoutController 會有下列的控制器動作：

**AddressAndPayment （GET 方法）**會顯示表單，以允許使用者輸入他們的資訊。

**AddressAndPayment （POST 方法）**會驗證輸入和處理順序。

**完成**使用者順利完成在結帳程序之後將會顯示。 此檢視會包含使用者的訂單號碼，以確認。

首先，請將 AddressAndPayment 來重新命名索引控制器動作 （其中我們建立控制器時，所產生）。 此控制器的動作只會顯示簽出的表單中，因此它並不需要任何模型的資訊。

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

我們 AddressAndPayment POST 方法會依循相同的模式，我們使用 StoreManagerController： 它會嘗試接受送出表單，並完成順序，並會在失敗時重新顯示表單。

驗證表單輸入符合我們的驗證需求的訂單後，我們將直接簽 PromoCode 表單值。 假設所有設定都正確，我們會與訂單儲存更新的資訊，請告知 ShoppingCart 物件完成訂單程序，並會重新導向至完成的動作。

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

成功完成時簽出程序，將使用者重新導向至完整的控制器動作。 此動作將會執行驗證，順序並確實屬於已登入使用者顯示訂單號碼當做確認訊息之前的簡單檢查。

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

*注意： 錯誤檢視時自動建立為我們 /Views/Shared 資料夾中我們已經開始專案。*

完整 CheckoutController 程式碼如下所示：

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a>加入 AddressAndPayment 檢視

現在，讓我們來建立 AddressAndPayment 檢視。 以滑鼠右鍵按一下其中一個 AddressAndPayment 控制器動作，並加入名為 AddressAndPayment 強型別為訂單，並且使用 編輯範本，如下所示的檢視。

![](mvc-music-store-part-9/_static/image6.png)

這個檢視可讓我們看建置 StoreManagerEdit 檢視時的技術的兩個使用：

- 我們將使用 Html.EditorForModel() 顯示表單欄位的順序模型
- 我們將利用 Order 類別中使用驗證屬性的驗證規則

我們會先更新表單程式碼使用 Html.EditorForModel()，後面接著其他的文字方塊中的促銷代碼。 AddressAndPayment 檢視的完整程式碼如下所示。

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a>定義順序的驗證規則

既然我們檢視已設定，我們將設定的驗證規則為我們順序的模型可以如同我們先前專輯模型。 以滑鼠右鍵按一下 [模型] 資料夾，然後加入類別的順序。 除了我們先前使用相簿的驗證屬性，我們將也使用規則運算式來驗證使用者的電子郵件地址。

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

嘗試送出表單時遺失或無效的資訊現在將會顯示錯誤訊息使用用戶端驗證。

![](mvc-music-store-part-9/_static/image7.png)

好了，我們所做過大部分的困難工作在結帳程序中;我們只需要完成幾個勝算的特徵 and 結束。 我們需要加入兩個簡單的檢視，因此需要處理的登入程序期間的購物車資訊遞交。

## <a name="adding-the-checkout-complete-view"></a>新增簽出完整的檢視

簽出完整的檢視是相當簡單，只需要顯示順序識別碼。 以滑鼠右鍵按一下 完成控制器動作，並加入名為完成其強型別當做整數的檢視

![](mvc-music-store-part-9/_static/image8.png)

現在我們將會更新以顯示訂單 ID，檢視程式碼，如下所示。

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a>更新檢視時發生錯誤

預設範本包括錯誤檢視共用的 [檢視] 資料夾中，使它可以重複使用其他位置中的站台。 此錯誤檢視包含非常簡單的錯誤，而且不會使用配置中，我們的網站，因此我們會將它更新。

由於這是一般錯誤網頁，內容是非常簡單。 我們將會包含訊息和連結以瀏覽到記錄中上一頁如果使用者想要再試一次其動作。

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]


> [!div class="step-by-step"]
> [上一頁](mvc-music-store-part-8.md)
> [下一頁](mvc-music-store-part-10.md)
