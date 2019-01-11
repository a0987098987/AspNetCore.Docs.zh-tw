---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: 顯示資料項目及詳細資料 |Microsoft Docs
author: Erikre
description: 本系列教學課程將教導您建置 ASP.NET Web Forms 應用程式與 ASP.NET 4.7 和 Microsoft Visual Studio Community 2017 for Web 的基本概念
ms.author: riande
ms.date: 1/09/2019
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 73ae1660f5d6e3e28c1c155e745a62936e3502df
ms.sourcegitcommit: cec77d5ad8a0cedb1ecbec32834111492afd0cd2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/10/2019
ms.locfileid: "54207430"
---
<a name="display-data-items-and-details"></a>顯示資料的項目和詳細資料
====================
藉由[Erik Reitan](https://github.com/Erikre)

> 本系列教學課程將教導您建置 ASP.NET Web Forms 應用程式與 ASP.NET 4.7 和 Microsoft Visual Studio Community 2017 for Web 的基本概念。

在本教學課程中，您將了解如何顯示資料的項目和項目詳細資料與 ASP.NET Web Form 和 Entity Framework Code First。 本教學課程是根據先前的"UI 和瀏覽 」 教學課程中，做為 Wingtip 玩具店教學課程系列的一部分。 在完成教學課程中，產品*ProductsList.aspx*頁面和產品的詳細資料*ProductDetails.aspx*頁面會顯示。

## <a name="what-you-learn"></a>您學到什麼

- 加入資料控制項來顯示資料庫的產品。
- 資料控制項連接到選取的資料。
- 加入資料控制項來顯示產品詳細資料。
- 剖析查詢字串值，並使用它來篩選擷取的資料庫資料。

在本教學課程中引進的功能包括模型繫結和值提供者。

## <a name="add-a-data-control-to-display-products"></a>加入資料控制項顯示的產品
 
您必須將資料繫結至伺服器控制項的幾個選項。 最常見的包括：

 * 新增資料來源控制項
 * 以手動方式將程式碼
 * 實作的模型繫結

### <a name="use-a-data-source-control-to-bind-data"></a>使用資料來源控制項繫結資料

新增資料來源控制項來顯示資料的控制項連結資料來源控制項。 使用此方法時，您可以以宣告方式，而不必以程式設計方式連接到資料來源的 伺服器端控制項。

### <a name="code-by-hand-to-bind-data"></a>以手動方式將資料繫結的程式碼

手動撰寫程式碼牽涉到：

1. 讀取的值
2. 檢查是否為 null
3. 將它轉換成適當的型別
4. 檢查的轉換成功
5. 進行轉換的值的查詢 

使用此方法時，您會有完整控制您的資料存取邏輯。

### <a name="use-model-binding-to-bind-data"></a>若要將資料繫結中使用模型繫結

使用模型繫結，最少的程式碼的結果繫結，並讓您能夠重複使用在您的應用程式的功能。 它還能提供豐富的資料繫結架構，簡化程式碼為主的資料存取邏輯的處理。

## <a name="display-products"></a>顯示的產品

在本教學課程中，您可以使用模型繫結將資料繫結。 若要設定資料控制項使用模型繫結來選取資料，您將控制項的`SelectMethod`屬性頁面的程式碼中的方法。 資料控制項在網頁生命週期中的適當時間呼叫的方法，並自動將傳回的資料繫結。 不需要明確呼叫`DataBind`方法。

您使用此選項，完成下列步驟，修改*ProductList.aspx*標記，以顯示產品。

1. 在 **方案總管**，開啟*ProductList.aspx*。

2. 以下列標記取代現有的標記： 

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample1.aspx)]

上述標記會使用**ListView**控制項，名為`productList`顯示的產品。

[!code-aspx-csharp[Main](display_data_items_and_details/samples/sample2.aspx)]

透過範本和樣式，您可以定義如何**ListView**控制項會顯示資料。 它可用於任何重複的結構中的資料。 雖然這**ListView**範例只會顯示資料庫資料，您也可以不需要程式碼，讓使用者編輯、 插入和刪除資料，以及排序和分頁資料。

當您設定`ItemType`中的屬性**ListView**控制項，資料繫結運算式`Item`可和控制項成為強型別。 如先前的教學課程中所述，您可以選取具有 IntelliSense、 項目物件的詳細資料，例如指定`ProductName`:

![顯示資料的項目和詳細資料-IntelliSense](display_data_items_and_details/_static/image1.png)

您正在使用模型繫結，指定`SelectMethod`值 (`GetProducts`)。 這是您將加入程式碼的方法後面，以在下一個步驟中顯示的產品。

### <a name="add-code-to-display-products"></a>加入程式碼來顯示產品

在此步驟中，您會加入程式碼以填入**ListView**資料庫產品資料的控制項。 程式碼支援顯示所有產品，以及個別的類別目錄產品。

1. 在 **方案總管**，以滑鼠右鍵按一下*ProductList.aspx* ，然後選取**檢視程式碼**。
2. 取代現有的程式碼中*ProductList.aspx.cs*與這個檔案：   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

此程式碼示範`GetProducts`方法所**ListView**控制項的`ItemType`中的屬性參考*ProductList.aspx*。 若要將結果限制為特定資料庫的類別，程式碼會設定`categoryId`傳遞至查詢字串中的值*ProductList.aspx*。 `QueryStringAttribute`類別內`System.Web.ModelBinding`命名空間用來擷取查詢字串變數`id`的值。 這會指示，請以在執行階段，繫結至查詢字串值的模型繫結`categoryId`參數。

當有效的分類 (`categoryId`) 是傳遞時，結果會限制為該類別目錄資料庫產品。 比方說，如果*ProductsList.aspx*網頁的 URL 如下：

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

此頁面會顯示只有產品所在`categoryId`等於`1`。

如果沒有查詢字串傳遞，則會顯示所有產品。

會呼叫這些方法的值來源*值提供者*(例如`QueryString`)，表示要使用哪些值提供者的參數屬性則稱為*值提供者屬性*(這類`id`)。 ASP.NET 會包含值提供者和所有一般 Web Form 應用程式使用者輸入來源的屬性。 其中包括查詢字串、 cookie、 表單值、 控制項、 檢視狀態、 工作階段狀態和設定檔屬性。 您也可以撰寫自訂值提供者。

### <a name="run-the-application"></a>執行應用程式

執行應用程式現在若要檢視所有的產品或分類的產品。

1. 在 Visual Studio 中按**F5**執行應用程式。
 瀏覽器隨即開啟並顯示*Default.aspx*頁面。

2. 從 [產品類別目錄] 功能表中，選取**汽車**。

   *ProductList.aspx*頁面隨即出現，顯示只從產品**汽車**類別目錄。 稍後在本教學課程中，您可以顯示產品詳細資料。

    ![顯示資料的項目和詳細資料-汽車](display_data_items_and_details/_static/image2.png)

3. 選取 **產品**從上方的功能表。
 *ProductList.aspx*頁面現在會顯示所有產品。 

    ![顯示資料的項目和詳細資料-產品](display_data_items_and_details/_static/image3.png)

4. 關閉瀏覽器，並返回 Visual Studio。

### <a name="add-a-data-control-to-display-product-details"></a>加入資料控制項來顯示產品詳細資料

修改*ProductDetails.aspx*您在上一個教學課程，以顯示特定產品的資訊中新增的標記：

1. 在 **方案總管**，開啟*ProductDetails.aspx*。

2. 此標記，取代現有的標記：

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample5.aspx)] 

此標記會使用**FormView**控制項來顯示特定產品詳細資料。 它會使用類似用來顯示資料的方法*ProductList.aspx*。 **FormView**控制項用來從資料來源一次顯示單一記錄。 當您使用**FormView**控制項，建立範本，以顯示和編輯資料繫結值。 這些範本包含控制項，繫結運算式，並格式化，定義表單的外觀和功能。

連接到資料庫的上一個標記需要額外的程式碼。

1. 在 **方案總管**，以滑鼠右鍵按一下*ProductDetails.aspx* ，然後選取**檢視程式碼**。  
   *ProductDetails.aspx.cs*檔案會顯示。

2. 取代現有的程式碼：   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

此程式碼會檢查 「`productID`"查詢字串值。 如果找到有效的值，則會顯示相符的產品。 如果找不到查詢字串，或其值不是有效的則會不顯示任何產品。

### <a name="run-the-application"></a>執行應用程式

現在您可以在其中執行的應用程式，以查看特定的產品詳細資料，根據產品 id。

1. 在 Visual Studio 中按**F5**執行應用程式。  
 瀏覽器中開啟*Default.aspx*。

2. 從 [分類] 功能表中，選取**潮水**。  
 *ProductList.aspx*頁面隨即顯示。

3. 選取 **紙張船隻**。  
 *ProductDetails.aspx*頁面隨即顯示。   

    ![顯示資料的項目和詳細資料-產品](display_data_items_and_details/_static/image4.png)

在下一個教學課程中，您加入購物車 Wingtip Toys 應用程式。

## <a name="additional-resources"></a>其他資源

[擷取和顯示資料與模型繫結和 web form](../../presenting-and-managing-data/model-binding/retrieving-data.md)

> [!div class="step-by-step"]
> [上一頁](ui_and_navigation.md)
> [下一頁](shopping-cart.md)
