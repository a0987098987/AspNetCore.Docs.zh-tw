---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: 第 9 節： 註冊和簽出 |Microsoft Docs
author: jongalloway
description: 本教學課程系列會詳細說明所有建置 ASP.NET MVC Music 市集範例應用程式所採取的步驟。 第 9 涵蓋註冊和簽出。
ms.author: riande
ms.date: 04/21/2011
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: b3babf1d935774b0ef93d6ab02c8b295998f8afc
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827036"
---
<a name="part-9-registration-and-checkout"></a>第 9 節： 註冊和簽出
====================
藉由[Jon Galloway](https://github.com/jongalloway)

> MVC Music 市集是介紹，並逐步說明如何使用 ASP.NET MVC 和 Visual Studio 進行 web 開發的教學課程應用程式。  
>   
> MVC Music 市集是銷售線上音樂 album 和實作基本的網站管理、 使用者登入時，和 「 購物車 」 功能的輕量級的範例存放區實作。  
>   
> 本教學課程系列會詳細說明所有建置 ASP.NET MVC Music 市集範例應用程式所採取的步驟。 第 9 涵蓋註冊和簽出。


在本節中，我們將建立 CheckoutController 來收集購物者的地址和付款資訊。 我們將需要向我們的網站前簽出，所以此控制器將會需要授權的使用者。

使用者會瀏覽至結帳程序從購物車 」 結帳 」 按鈕，即可。

![](mvc-music-store-part-9/_static/image1.jpg)

如果使用者未登入中，將會提示他們。

![](mvc-music-store-part-9/_static/image1.png)

成功登入，使用者則是顯示的地址和付款的檢視。

![](mvc-music-store-part-9/_static/image2.png)

一旦他們已填滿表單，並提交訂單，它們會顯示 [訂單確認] 畫面。

![](mvc-music-store-part-9/_static/image3.png)

嘗試檢視不存在順序或不屬於您的訂單會顯示 [錯誤] 檢視。

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a>移轉購物車

當使用者按一下 [簽出] 按鈕時，是匿名的購物程序時，必須註冊和登入。 使用者會預期我們會維持之間瀏覽次數，因此我們需要將購物車資訊關聯的使用者，當他們完成註冊或登入其購物車 」 資訊。

這是執行動作，其實很簡單，因為我們 ShoppingCart 類別已經有方法，這個方法會將目前的購物車中的所有項目關聯的使用者名稱。 我們只需要呼叫這個方法，當使用者完成註冊或登入。

開啟**AccountController**我們所設定的成員資格和授權時，我們新增的類別。 新增 using 陳述式參考 MvcMusicStore.Models，然後新增下列 MigrateShoppingCart 方法：

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

接下來，修改登入後動作來呼叫 MigrateShoppingCart 之後使用者已驗證，, 如下所示：

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

進行相同變更，即可註冊張貼動作，只有在成功建立使用者帳戶之後，立即：

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

現在匿名的購物車會自動轉移至在成功註冊或登入的使用者帳戶就是這樣。

## <a name="creating-the-checkoutcontroller"></a>建立 CheckoutController

以滑鼠右鍵按一下 [控制器] 資料夾，並將新的控制站新增至名為 CheckoutController 使用空白控制器範本專案。

![](mvc-music-store-part-9/_static/image5.png)

首先，新增 Authorize 屬性要求使用者註冊，才能簽出在控制器類別宣告之上：

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

*注意： 這是類似於我們先前所做 StoreManagerController，變更，但在此情況下 Authorize 屬性所需的使用者是系統管理員角色。在簽出控制器中，我們會要求使用者登入，但不需要他們系統管理員。*

為了簡單起見，我們將不會處理在本教學課程的付款資訊。 相反地，我們允許使用者查看使用促銷代碼。 我們將儲存此促銷代碼，使用名為促銷代碼的常數。

如同 StoreController，我們要宣告欄位以保留 MusicStoreEntities 類別，名為 storeDB 的執行個體。 為了讓 MusicStoreEntities 類別的使用，我們將需要新增 using 陳述式 MvcMusicStore.Models 命名空間。 簽出控制器的頂端會出現下方。

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

CheckoutController 將會有下列控制器動作：

**AddressAndPayment （GET 方法）** 會顯示表單，以允許使用者輸入他們的資訊。

**AddressAndPayment （POST 方法）** 會驗證輸入，並且處理訂單。

**完成**之後使用者已順利完成結帳程序將會顯示。 此檢視會包含使用者的訂單號碼，以確認。

首先，讓我們將索引控制器動作 （這我們建立控制器時，所產生） 重新命名為 AddressAndPayment。 此控制器動作只會顯示簽出的表單中，因此它不需要任何模型的資訊。

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

我們的 AddressAndPayment POST 方法會遵循相同的模式，我們用於 StoreManagerController： 它會嘗試接受送出表單，並完成訂單，並會在失敗時重新顯示表單。

驗證表單輸入符合我們的驗證需求的訂單之後，我們將直接檢查促銷代碼表單值。 假設一切正確無誤，我們會與訂單儲存更新的資訊，請告知 ShoppingCart 物件完成訂單程序，並重新導向至完成的動作。

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

成功完成時簽出程序，將使用者重新導向至完整的控制器動作。 此動作會執行簡單的檢查，驗證順序會確實屬於登入的使用者之前先顯示確認的訂單號碼。

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

*注意： 錯誤檢視時自動建立為我們 /Views/Shared 資料夾中我們已經開始專案。*

完整的 CheckoutController 程式碼如下所示：

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a>加入 AddressAndPayment 檢視

現在，讓我們建立 AddressAndPayment 檢視。 以滑鼠右鍵按一下其中一個 AddressAndPayment 控制器動作，並新增名為 AddressAndPayment 會強型別為訂單，並使用 編輯範本，如下所示的檢視。

![](mvc-music-store-part-9/_static/image6.png)

此檢視會使用兩個我們討論過建置 StoreManagerEdit 檢視時的技術：

- 我們將使用 Html.EditorForModel() 來顯示表單欄位的順序模型
- 我們將會利用驗證規則使用驗證屬性中的 Order 類別

我們一開始先更新表單程式碼使用 Html.EditorForModel()，後面接著其他文字方塊的促銷代碼。 AddressAndPayment 檢視的完整程式碼如下所示。

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a>定義驗證規則的順序

現在，我們檢視已設定時，我們會設定驗證規則為我們的訂單模型如同我們先前專輯模型。 Models 資料夾上按一下滑鼠右鍵，並新增名為順序的類別。 除了我們先前使用相簿的驗證屬性，我們將也會使用規則運算式來驗證使用者的電子郵件地址。

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

嘗試送出表單時遺漏或無效的資訊現在會顯示使用用戶端驗證的錯誤訊息。

![](mvc-music-store-part-9/_static/image7.png)

好了，已經完成大半的困難的工作，如結帳程序;我們只需要少數的機率 and 結束，才能完成。 我們需要加入兩個簡單的檢視，而且我們需要負責處理的登入程序的購物車資訊遞移式。

## <a name="adding-the-checkout-complete-view"></a>加入簽出完整的檢視

簽出完整的檢視也很簡單，因為它只需要顯示訂單識別碼。 以滑鼠右鍵按一下完成控制器動作，並新增名為完成，它強型別為整數的檢視

![](mvc-music-store-part-9/_static/image8.png)

現在我們將會更新檢視程式碼，以顯示訂單 ID，如下所示。

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a>正在更新檢視時發生錯誤

預設範本包含錯誤檢視共用的 [views] 資料夾中，以便它可以重複使用其他位置中的站台。 此錯誤檢視包含非常簡單的錯誤，而且不會使用我們的網站配置，因此我們將會更新它。

由於這是一般錯誤頁面時，內容就非常簡單。 我們將會包含訊息和連結，以便瀏覽至歷程記錄中的上一頁，如果使用者想要重試其動作。

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]


> [!div class="step-by-step"]
> [上一頁](mvc-music-store-part-8.md)
> [下一頁](mvc-music-store-part-10.md)
