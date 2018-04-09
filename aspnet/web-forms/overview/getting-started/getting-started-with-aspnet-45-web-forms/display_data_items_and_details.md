---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: 顯示資料的項目，並詳細說明 |Microsoft 文件
author: Erikre
description: 此教學課程將告訴您建置使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for 我們的 ASP.NET Web Form 應用程式的基本概念...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 5fea654aa5116193cb7496c1b9020ed8e25fc06f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="display-data-items-and-details"></a>顯示資料的項目，並詳細說明
====================
由[Erik Reitan](https://github.com/Erikre)

[下載 Wingtip Toys 範例專案 (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)或[下載電子書 (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> 此教學課程將告訴您建置一個使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for Web 的 ASP.NET Web Form 應用程式的基本概念。 Visual Studio 2013[與 C# 原始程式碼的專案](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)隨附此教學課程了。


本教學課程說明如何顯示資料的項目和使用 ASP.NET Web Form 和 Entity Framework Code First 資料的項目詳細資料。 本教學課程上一個教學課程 」 UI 和巡覽 」 為基礎，並是 Wingtip 玩具存放區的教學課程系列的一部分。 當您已經完成本教學課程時，您可以看到產品上*ProductsList.aspx*頁面，以及在個別產品詳細資料*ProductDetails.aspx*頁面。

## <a name="what-youll-learn"></a>您將學習：

- 如何加入資料控制項來顯示資料庫的產品。
- 如何連接資料控制項選取的資料。
- 如何加入資料控制項來顯示資料庫中的產品詳細資料。
- 如何擷取查詢字串中的值，並使用該值來限制從資料庫擷取的資料。

### <a name="these-are-the-features-introduced-in-the-tutorial"></a>這些是教學課程中所引進的功能：

- 模型繫結
- 值提供者

## <a name="adding-a-data-control-to-display-products"></a>加入資料控制項顯示的產品

當資料繫結至的伺服器控制項中，有幾個不同的選項，您可以使用。 最常見的選項包括新增資料來源控制項、 新增程式碼以手動方式或使用模型繫結。

### <a name="using-a-data-source-control-to-bind-data"></a>使用資料來源控制項將資料繫結

新增資料來源控制項，可讓您連結至控制項顯示之資料的資料來源控制項。 這種方法可讓您以宣告方式伺服器端控制項直接連接到資料來源，而不是使用以程式設計的方式。

### <a name="coding-by-hand-to-bind-data"></a>撰寫程式碼以手動方式將資料繫結

加入程式碼以手動方式牽涉到讀取值、 檢查 null 值，嘗試將它轉換成適當型別、 檢查轉換是否成功，以及最後，在查詢中使用的值。 當您需要完整控制您的資料存取邏輯時，您會使用這種方法。

### <a name="using-model-binding-to-bind-data"></a>使用模型繫結至繫結資料

使用模型繫結可讓您使用最少的程式碼的結果繫結，並讓您能夠重複使用整個應用程式的功能。 模型繫結以外的工具來簡化程式碼為重心資料存取邏輯的使用，同時保留的豐富的資料繫結架構的優點。

## <a name="displaying-products"></a>顯示產品

在本教學課程中，您將使用模型繫結，將資料繫結。 若要設定要用來選取資料的模型繫結資料控制項，您將控制項的`SelectMethod`網頁的程式碼中的方法名稱的屬性。 資料控制項在頁面生命週期中的適當時間呼叫的方法，並自動繫結傳回的資料。 沒有不需要明確地呼叫`DataBind`方法。

使用下列步驟，您將修改中的標記*ProductList.aspx*頁面上，讓網頁可以顯示產品。

1. 在**方案總管 中**，開啟*ProductList.aspx*頁面。
2. 以下列標記取代現有的標記：   

    [!code-aspx[Main](display_data_items_and_details/samples/sample1.aspx)]

此程式碼使用**ListView**控制項，名為 「 產品 」 清單，用於顯示產品。

[!code-aspx[Main](display_data_items_and_details/samples/sample2.aspx)]

**ListView**控制項會顯示資料，您可以使用的範本和樣式定義的格式。 它可用於任何重複結構中的資料。 這**ListView**範例只會顯示從資料庫中，但是您可以啟用使用者編輯、 插入和刪除資料，以及排序和頁資料，而不需程式碼的資料。

藉由設定`ItemType`屬性**ListView**控制項，資料繫結運算式`Item`可用和控制項變成強型別。 如先前的教學課程中所述，您可以選取使用 IntelliSense，例如指定的項目物件的詳細資料`ProductName`:

![顯示資料的項目和詳細資料的 IntelliSense](display_data_items_and_details/_static/image1.png)

此外，您會使用模型繫結來指定`SelectMethod`值。 這個值 (`GetProducts`) 會對應至您將會顯示下一個步驟中的產品加入後置程式碼的方法。

### <a name="adding-code-to-display-products"></a>加入程式碼，以顯示產品

在此步驟中，您會加入程式碼以填入**ListView**控制產品資料庫中的資料。 程式碼將會支援個別的類別，以及顯示所有產品都顯示產品。

1. 在**方案總管] 中**，以滑鼠右鍵按一下*ProductList.aspx* ，然後按一下 [**檢視程式碼**。
2. 取代現有的程式碼中*ProductList.aspx.cs*以下列程式碼檔案：   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

此程式碼會示範`GetProducts`所參考的方法`ItemType`屬性**ListView**控制*ProductList.aspx*頁面。 若要將結果限制為資料庫中的特定類別，此程式碼設定`categoryId`值的查詢字串值傳遞至*ProductList.aspx*頁面時*ProductList.aspx*頁面瀏覽至。 `QueryStringAttribute`類別`System.Web.ModelBinding`命名空間用來擷取查詢字串變數 id 的值。這會指示模型繫結至繫結的查詢字串中的值再試一次`categoryId`在執行階段參數。

查詢的結果時有效的類別目錄做為查詢字串傳遞至網頁，僅限於資料庫中符合這些產品`categoryId`值。 比方說，如果 URL *ProductsList.aspx*頁面如下所示：

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

頁面只會顯示產品其中`category`等於`1`。

如果沒有查詢字串是否包含巡覽時*ProductList.aspx*  頁面上，將會顯示所有產品。

這些方法的值的來源指*值提供者*(例如*QueryString*)，以及參數屬性，以指出要使用哪些值提供者指值提供者屬性 (例如"`id`")。 ASP.NET Web Form 應用程式，例如查詢字串、 cookies、 表單值、 控制項、 檢視狀態、 工作階段狀態，以及設定檔內容中，包含值提供者及所有的一般使用者輸入的來源對應屬性。 您也可以撰寫自訂值提供者。

### <a name="running-the-application"></a>執行應用程式

執行應用程式現在若要查看如何檢視所有的產品或只受到類別目錄的產品集。

1. 在**方案總管 中**，以滑鼠右鍵按一下*Default.aspx*頁面，然後選取**瀏覽器中的檢視**。  
 瀏覽器會開啟並顯示*Default.aspx*頁面。
2. 選取**汽車**產品類別目錄瀏覽功能表中。  
 *ProductList.aspx*頁面會顯示，其中包含 「 汽車 」 分類的產品。 稍後在本教學課程中，您將會顯示產品詳細資料。  

    ![顯示資料的項目和詳細資料為輛汽車](display_data_items_and_details/_static/image2.png)
3. 選取**產品**從導覽功能表頂端。  
 同樣地， *ProductList.aspx*頁面隨即顯示，但這次它會顯示完整的產品清單。   

    ![顯示資料的項目和詳細資料-產品](display_data_items_and_details/_static/image3.png)
4. 關閉瀏覽器，並返回 Visual Studio。

### <a name="adding-a-data-control-to-display-product-details"></a>加入資料控制項來顯示產品詳細資料

接下來，您將修改中的標記*ProductDetails.aspx*您在上一個教學課程中，讓網頁可以顯示個別產品的相關資訊加入頁面。

1. 在**方案總管 中**，開啟*ProductDetails.aspx*頁面。
2. 以下列標記取代現有的標記：   

    [!code-aspx[Main](display_data_items_and_details/samples/sample5.aspx)]

此程式碼使用**FormView**控制項來顯示有關個別產品的詳細資料。 此標記，會使用 「 類似，用來顯示資料的方法*ProductList.aspx*頁面。 **FormView**控制項用來從資料來源一次顯示單一記錄。 當您使用**FormView**控制項時，建立範本，以顯示和編輯資料繫結值。 範本中包含的控制項，繫結運算式和格式化的外觀和表單的功能。

若要連接到資料庫的上述的標記，您必須加入額外程式碼以*ProductDetails.aspx*程式碼。

1. 在**方案總管] 中**，以滑鼠右鍵按一下*ProductDetails.aspx* ，然後按一下 [**檢視程式碼**。  
   *ProductDetails.aspx.cs*檔案將會顯示。
2. 將現有的程式碼更換成下列程式碼：   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

此程式碼會檢查 「`productID`"查詢字串值。 如果找到有效的查詢字串值，就會顯示比對的產品。 如果找到查詢字串，或查詢字串值無效上, 會顯示任何產品*ProductDetails.aspx*頁面。

### <a name="running-the-application"></a>執行應用程式

現在，您可以執行的應用程式，以查看顯示個別產品為基礎的產品識別碼。

1. 按**F5** Visual Studio 執行應用程式中。  
 瀏覽器會開啟並顯示*Default.aspx*頁面。
2. 類別目錄瀏覽功能表中選取 「 船"。  
 *ProductList.aspx*頁面隨即顯示。
3. 從產品清單中選取 「 紙張船"產品。  
 *ProductDetails.aspx*頁面隨即顯示。   

    ![顯示資料的項目和詳細資料-產品](display_data_items_and_details/_static/image4.png)
4. 關閉瀏覽器。

## <a name="summary"></a>總結

您已在本教學課程系列的加入標記和程式碼以顯示一份產品清單，並顯示產品詳細資料。 此程序期間，您已經學會使用強型別的資料控制項、 模型繫結和值提供者相關。 在下一個教學課程中，您會加入至 Wingtip Toys 範例應用程式的購物車。

## <a name="additional-resources"></a>其他資源

[Web form 模型繫結與資料擷取和顯示](../../presenting-and-managing-data/model-binding/retrieving-data.md)

> [!div class="step-by-step"]
> [上一頁](ui_and_navigation.md)
> [下一頁](shopping-cart.md)
