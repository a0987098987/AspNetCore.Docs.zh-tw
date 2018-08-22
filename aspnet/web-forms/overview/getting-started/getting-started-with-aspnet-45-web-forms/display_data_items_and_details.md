---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: 顯示資料項目及詳細資料 |Microsoft Docs
author: Erikre
description: 本系列教學課程將教導您建置使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for 我們的 ASP.NET Web Forms 應用程式的基本概念...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 2184c04d5f2361526be0409178dc0a6c665ebc4f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830114"
---
<a name="display-data-items-and-details"></a>顯示資料項目及詳細資料
====================
藉由[Erik Reitan](https://github.com/Erikre)

[下載 Wingtip Toys 範例專案 (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)或[下載電子書 (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> 本系列教學課程將教導您建置使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for Web 的 ASP.NET Web Forms 應用程式的基本概念。 Visual Studio 2013[含有 C# 原始程式碼專案](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)隨附了本教學課程系列。


本教學課程說明如何顯示資料的項目和使用 ASP.NET Web Form 和 Entity Framework Code First 的項目詳細資料。 此教學課程先前的教學課程 」 UI 和瀏覽 」，並且是 Wingtip 玩具店教學課程系列的一部分。 當您完成本教學課程中時，您將能夠看到產品上*ProductsList.aspx*頁面和詳細資料上的個別產品*ProductDetails.aspx*頁面。

## <a name="what-youll-learn"></a>您將學到什麼：

- 如何加入資料控制項來顯示資料庫中的產品。
- 如何在資料控制項連接到選取的資料。
- 如何新增資料控制項來顯示產品詳細資料，從資料庫。
- 如何擷取查詢字串中的值，並使用該值來限制從資料庫擷取的資料。

### <a name="these-are-the-features-introduced-in-the-tutorial"></a>在本教學課程中所引進的功能如下：

- 模型繫結
- 值提供者

## <a name="adding-a-data-control-to-display-products"></a>將資料控制項加入顯示的產品

當資料繫結至伺服器控制項中，有幾個不同的選項，您可以使用。 最常見的選項包括新增資料來源控制項，以手動的方式，將程式碼或使用模型繫結。

### <a name="using-a-data-source-control-to-bind-data"></a>使用資料來源控制項繫結資料

新增資料來源控制項，可讓您連結至控制項顯示資料的資料來源控制項。 這種方法可讓您以宣告方式伺服器端控制項直接連接到資料來源，而不是使用以程式設計的方式。

### <a name="coding-by-hand-to-bind-data"></a>撰寫程式碼以手動方式將資料繫結

新增程式碼，以手動方式牽涉到讀取的值、 檢查 null 值、 嘗試將它轉換成適當的型別、 檢查轉換是否成功，以及最後，在查詢中使用的值。 當您需要保留您的資料存取邏輯的完整控制權時，您會使用這種方法。

### <a name="using-model-binding-to-bind-data"></a>使用模型繫結至資料繫結

使用模型繫結可讓您將使用最少的程式碼的結果繫結，並讓您能夠重複使用在您的應用程式的功能。 模型繫結的目標是要簡化程式碼為主的資料存取邏輯的使用，同時仍然保留的豐富的資料繫結架構的優點。

## <a name="displaying-products"></a>顯示產品

在本教學課程中，您將使用模型繫結，將資料繫結。 若要設定資料控制項使用模型繫結來選取資料，您將控制項的`SelectMethod`網頁的程式碼中的方法名稱的屬性。 資料控制項在網頁生命週期中的適當時間呼叫的方法，並自動將傳回的資料繫結。 不需要明確呼叫`DataBind`方法。

使用下列步驟，您會修改中的標記*ProductList.aspx*頁面上，讓網頁可以顯示產品。

1. 在 **方案總管**，開啟*ProductList.aspx*頁面。
2. 以下列標記取代現有的標記：   

    [!code-aspx[Main](display_data_items_and_details/samples/sample1.aspx)]

此程式碼會使用**ListView**控制項，名為"productList 」，用於顯示產品。

[!code-aspx[Main](display_data_items_and_details/samples/sample2.aspx)]

**ListView**控制項會在您使用範本和樣式定義的格式顯示資料。 它可用於任何重複的結構中的資料。 這**ListView**範例只會顯示從資料庫中，不過您可以讓使用者編輯、 插入和刪除資料，以及排序和頁面資料，不需要程式碼的資料。

藉由設定`ItemType`中的屬性**ListView**控制項，資料繫結運算式`Item`可和控制項成為強型別。 如先前的教學課程中所述，您可以選取使用 IntelliSense，例如指定的項目物件的詳細資料`ProductName`:

![顯示資料的項目和詳細資料-IntelliSense](display_data_items_and_details/_static/image1.png)

此外，您會使用模型繫結來指定`SelectMethod`值。 此值 (`GetProducts`) 將會對應到您將在下一個步驟中顯示的產品新增到程式碼後置的方法。

### <a name="adding-code-to-display-products"></a>加入程式碼來顯示產品

在此步驟中，您將新增程式碼以填入**ListView**控制項從資料庫的產品資料。 程式碼將會支援依個別的類別，以及顯示所有產品的都顯示產品。

1. 在 **方案總管**，以滑鼠右鍵按一下*ProductList.aspx* ，然後按一下 **檢視程式碼**。
2. 取代現有的程式碼中*ProductList.aspx.cs*為下列程式碼的檔案：   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

此程式碼示範`GetProducts`所參考的方法`ItemType`屬性**ListView**控制中*ProductList.aspx*頁面。 若要將結果限制為資料庫中的特定類別，程式碼會設定`categoryId`值從查詢字串值傳遞給*ProductList.aspx*頁面時*ProductList.aspx*頁面巡覽至。 `QueryStringAttribute`類別中`System.Web.ModelBinding`命名空間用來擷取查詢字串變數識別碼的值。這會指示嘗試將值繫結至查詢字串中的模型繫結`categoryId`在執行階段參數。

有效的類別目錄作為查詢字串傳遞至頁面時，查詢的結果僅限於這些資料庫中的產品符合`categoryId`值。 比方說，如果 URL *ProductsList.aspx*頁面如下：

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

此頁面會顯示只有產品所在`category`等於`1`。

瀏覽至時，如果沒有查詢字串包含*ProductList.aspx*  頁面上，將會顯示所有產品。

這些方法的值的來源指*值提供者*(例如*QueryString*)，並指出要使用哪些值提供者的參數屬性指值提供者屬性 (例如 「`id`")。 ASP.NET Web Form 應用程式中，例如查詢字串、 cookie、 表單值、 控制項、 檢視狀態、 工作階段狀態和設定檔屬性中包含值提供者和對應的屬性，針對所有一般使用者輸入的來源。 您也可以撰寫自訂值提供者。

### <a name="running-the-application"></a>執行應用程式

執行應用程式現在若要查看如何檢視所有的產品或只是一組限制類別目錄的產品。

1. 在 [**方案總管] 中**，以滑鼠右鍵按一下*Default.aspx*頁面，然後選取**瀏覽器中的檢視**。  
 瀏覽器會開啟並顯示*Default.aspx*頁面。
2. 選取 **汽車**產品類別目錄瀏覽功能表。  
 *ProductList.aspx*頁面會顯示 [Cars] 分類中包含的產品。 稍後在本教學課程中，您將會顯示產品詳細資料。  

    ![顯示資料的項目和詳細資料-汽車](display_data_items_and_details/_static/image2.png)
3. 選取 **產品**從頂端導覽功能表。  
 同樣地， *ProductList.aspx*顯示頁面，但這次它顯示的產品的完整清單。   

    ![顯示資料的項目和詳細資料-產品](display_data_items_and_details/_static/image3.png)
4. 關閉瀏覽器，並返回 Visual Studio。

### <a name="adding-a-data-control-to-display-product-details"></a>新增資料控制項來顯示產品詳細資料

接下來，您會修改中的標記*ProductDetails.aspx* ，讓網頁可以顯示個別產品的相關資訊加入先前的教學課程中的頁面。

1. 在 **方案總管**，開啟*ProductDetails.aspx*頁面。
2. 以下列標記取代現有的標記：   

    [!code-aspx[Main](display_data_items_and_details/samples/sample5.aspx)]

此程式碼會使用**FormView**控制項來顯示有關個別產品的詳細資料。 此標記會使用類似用來顯示資料的方法*ProductList.aspx*頁面。 **FormView**控制項用來從資料來源一次顯示單一記錄。 當您使用**FormView**控制項，建立範本，以顯示和編輯資料繫結值。 範本包含的控制項，繫結運算式和格式化的外觀和功能的形式。

若要連接到資料庫的上述的標記，您必須新增其他程式碼以*ProductDetails.aspx*程式碼。

1. 在 **方案總管**，以滑鼠右鍵按一下*ProductDetails.aspx* ，然後按一下 **檢視程式碼**。  
   *ProductDetails.aspx.cs*檔案就會顯示。
2. 將現有的程式碼更換成下列程式碼：   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

此程式碼會檢查 「`productID`"查詢字串值。 如果找到有效的查詢字串值，則會顯示相符的產品。 如果不找到任何查詢字串，或查詢字串值無效，沒有任何產品會顯示在*ProductDetails.aspx*頁面。

### <a name="running-the-application"></a>執行應用程式

現在，您可以執行的應用程式，以查看個別的產品，顯示為基礎的產品識別碼。

1. 按下**F5**當您在 Visual Studio 執行應用程式。  
 瀏覽器會開啟並顯示*Default.aspx*頁面。
2. 從 [類別瀏覽] 功能表中選取 > 潮水"。  
 *ProductList.aspx*頁面隨即顯示。
3. 從產品清單中選取 「 紙船隻 」 產品。  
 *ProductDetails.aspx*頁面隨即顯示。   

    ![顯示資料的項目和詳細資料-產品](display_data_items_and_details/_static/image4.png)
4. 關閉瀏覽器。

## <a name="summary"></a>總結

在本教學課程系列的中，您必須新增標記和程式碼顯示一份產品清單，並顯示產品詳細資料。 在此過程中，您已了解強型別的資料控制項、 模型繫結，以及值提供者。 在下一個教學課程中，您將新增 「 Wingtip Toys 範例應用程式的購物車。

## <a name="additional-resources"></a>其他資源

[擷取和顯示資料與模型繫結和 web form](../../presenting-and-managing-data/model-binding/retrieving-data.md)

> [!div class="step-by-step"]
> [上一頁](ui_and_navigation.md)
> [下一頁](shopping-cart.md)
