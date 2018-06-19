---
uid: web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb
title: 在 GridView 的頁尾 (VB) 中顯示摘要資訊 |Microsoft 文件
author: rick-anderson
description: 底部的摘要資料列中的報表通常顯示摘要資訊。 GridView 控制項可以包含頁尾資料列至其儲存格，我們可以 pr...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 41c818b7-603a-402b-8847-890a63547b6f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb
msc.type: authoredcontent
ms.openlocfilehash: d9a1a3f3c680f367395f984254da6cdcdd3c08d4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877612"
---
<a name="displaying-summary-information-in-the-gridviews-footer-vb"></a><span data-ttu-id="17176-104">在 GridView 的頁尾 (VB) 中顯示摘要資訊</span><span class="sxs-lookup"><span data-stu-id="17176-104">Displaying Summary Information in the GridView's Footer (VB)</span></span>
====================
<span data-ttu-id="17176-105">由[Scott Mitchell](https://twitter.com/ScottOnWriting)</span><span class="sxs-lookup"><span data-stu-id="17176-105">by [Scott Mitchell](https://twitter.com/ScottOnWriting)</span></span>

<span data-ttu-id="17176-106">[下載範例應用程式](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_15_VB.exe)或[下載 PDF](displaying-summary-information-in-the-gridview-s-footer-vb/_static/datatutorial15vb1.pdf)</span><span class="sxs-lookup"><span data-stu-id="17176-106">[Download Sample App](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_15_VB.exe) or [Download PDF](displaying-summary-information-in-the-gridview-s-footer-vb/_static/datatutorial15vb1.pdf)</span></span>

> <span data-ttu-id="17176-107">底部的摘要資料列中的報表通常顯示摘要資訊。</span><span class="sxs-lookup"><span data-stu-id="17176-107">Summary information is often displayed at the bottom of the report in a summary row.</span></span> <span data-ttu-id="17176-108">GridView 控制項可以包含頁尾資料列至其儲存格以程式設計的方式可以插入彙總資料。</span><span class="sxs-lookup"><span data-stu-id="17176-108">The GridView control can include a footer row into whose cells we can programmatically inject aggregate data.</span></span> <span data-ttu-id="17176-109">在本教學課程，我們會看到如何顯示此頁尾資料列中的彙總資料。</span><span class="sxs-lookup"><span data-stu-id="17176-109">In this tutorial we'll see how to display aggregate data in this footer row.</span></span>


## <a name="introduction"></a><span data-ttu-id="17176-110">簡介</span><span class="sxs-lookup"><span data-stu-id="17176-110">Introduction</span></span>

<span data-ttu-id="17176-111">除了查看順序，以及重新訂購層級上的每個產品的價格、 庫存、 單位，使用者可能也會想要彙總的資訊，例如平均價格，庫存的總數等等。</span><span class="sxs-lookup"><span data-stu-id="17176-111">In addition to seeing each of the products' prices, units in stock, units on order, and reorder levels, a user might also be interested in aggregate information, such as the average price, the total number of units in stock, and so on.</span></span> <span data-ttu-id="17176-112">這類的摘要資訊通常會顯示摘要資料列中的報表底部。</span><span class="sxs-lookup"><span data-stu-id="17176-112">Such summary information is often displayed at the bottom of the report in a summary row.</span></span> <span data-ttu-id="17176-113">GridView 控制項可以包含頁尾資料列至其儲存格以程式設計的方式可以插入彙總資料。</span><span class="sxs-lookup"><span data-stu-id="17176-113">The GridView control can include a footer row into whose cells we can programmatically inject aggregate data.</span></span>

<span data-ttu-id="17176-114">這項工作會顯示我們與這三個挑戰：</span><span class="sxs-lookup"><span data-stu-id="17176-114">This task presents us with three challenges:</span></span>

1. <span data-ttu-id="17176-115">設定要顯示其頁尾資料列 GridView</span><span class="sxs-lookup"><span data-stu-id="17176-115">Configuring the GridView to display its footer row</span></span>
2. <span data-ttu-id="17176-116">決定摘要的資料;也就是如何我們計算平均價格或庫存單位總數？</span><span class="sxs-lookup"><span data-stu-id="17176-116">Determining the summary data; that is, how do we compute the average price or the total of the units in stock?</span></span>
3. <span data-ttu-id="17176-117">摘要資料插入到適當的儲存格的頁尾資料列</span><span class="sxs-lookup"><span data-stu-id="17176-117">Injecting the summary data into the appropriate cells of the footer row</span></span>

<span data-ttu-id="17176-118">在本教學課程中，我們會看到如何克服這些挑戰。</span><span class="sxs-lookup"><span data-stu-id="17176-118">In this tutorial we'll see how to overcome these challenges.</span></span> <span data-ttu-id="17176-119">具體來說，我們將建立頁面，其中列出與顯示在 GridView 中選取類別目錄的產品下拉式清單中的類別。</span><span class="sxs-lookup"><span data-stu-id="17176-119">Specifically, we'll create a page that lists the categories in a drop-down list with the selected category's products displayed in a GridView.</span></span> <span data-ttu-id="17176-120">在 GridView 會包含頁尾資料列，其中顯示平均價格和之單元的總數，庫存和產品類別目錄中的順序。</span><span class="sxs-lookup"><span data-stu-id="17176-120">The GridView will include a footer row that shows the average price and total number of units in stock and on order for products in that category.</span></span>


<span data-ttu-id="17176-121">[![摘要資訊會顯示在 GridView 的頁尾資料列](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image2.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="17176-121">[![Summary Information is Displayed in the GridView's Footer Row](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image2.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image1.png)</span></span>

<span data-ttu-id="17176-122">**圖 1**： 摘要資訊會顯示在 GridView 的頁尾資料列 ([按一下以檢視完整大小的影像](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="17176-122">**Figure 1**: Summary Information is Displayed in the GridView's Footer Row ([Click to view full-size image](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image3.png))</span></span>


<span data-ttu-id="17176-123">此教學課程中，其產品的主要/詳細資料介面的類別目錄與根據先前所涵蓋的概念[主要/詳細資料篩選與 DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="17176-123">This tutorial, with its category to products master/detail interface, builds upon the concepts covered in the earlier [Master/Detail Filtering With a DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) tutorial.</span></span> <span data-ttu-id="17176-124">如果您已經還會執行較早的教學課程，請先進行繼續在這一個。</span><span class="sxs-lookup"><span data-stu-id="17176-124">If you've not yet worked through the earlier tutorial, please do so before continuing on with this one.</span></span>

## <a name="step-1-adding-the-categories-dropdownlist-and-products-gridview"></a><span data-ttu-id="17176-125">步驟 1： 加入 DropDownList 分類和產品的 GridView</span><span class="sxs-lookup"><span data-stu-id="17176-125">Step 1: Adding the Categories DropDownList and Products GridView</span></span>

<span data-ttu-id="17176-126">之前有關自己新增至 GridView 的頁尾的摘要資訊，讓我們第一次只建立主要/詳細資料報表。</span><span class="sxs-lookup"><span data-stu-id="17176-126">Before concerning ourselves with adding summary information to the GridView's footer, let's first simply build the master/detail report.</span></span> <span data-ttu-id="17176-127">當我們完成第一個步驟之後時，我們會探討如何將加入摘要資料。</span><span class="sxs-lookup"><span data-stu-id="17176-127">Once we've completed this first step, we'll look at how to include summary data.</span></span>

<span data-ttu-id="17176-128">先開啟`SummaryDataInFooter.aspx`頁面`CustomFormatting`資料夾。</span><span class="sxs-lookup"><span data-stu-id="17176-128">Start by opening the `SummaryDataInFooter.aspx` page in the `CustomFormatting` folder.</span></span> <span data-ttu-id="17176-129">將 DropDownList 控制項，並設定其`ID`至`Categories`。</span><span class="sxs-lookup"><span data-stu-id="17176-129">Add a DropDownList control and set its `ID` to `Categories`.</span></span> <span data-ttu-id="17176-130">接下來，按一下 選擇資料來源連結，從 DropDownList 的智慧標籤上，選擇加入名為新 ObjectDataSource`CategoriesDataSource`會叫用`CategoriesBLL`類別的`GetCategories()`方法。</span><span class="sxs-lookup"><span data-stu-id="17176-130">Next, click on the Choose Data Source link from the DropDownList's smart tag and opt to add a new ObjectDataSource named `CategoriesDataSource` that invokes the `CategoriesBLL` class's `GetCategories()` method.</span></span>


<span data-ttu-id="17176-131">[![加入名為 CategoriesDataSource 新 ObjectDataSource](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image5.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="17176-131">[![Add a New ObjectDataSource Named CategoriesDataSource](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image5.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image4.png)</span></span>

<span data-ttu-id="17176-132">**圖 2**： 加入新的 ObjectDataSource 名為`CategoriesDataSource`([按一下以檢視完整大小的影像](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="17176-132">**Figure 2**: Add a New ObjectDataSource Named `CategoriesDataSource` ([Click to view full-size image](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image6.png))</span></span>


<span data-ttu-id="17176-133">[![已叫用 CategoriesBLL 類別 GetCategories() 方法 ObjectDataSource](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image8.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="17176-133">[![Have the ObjectDataSource Invoke the CategoriesBLL Class's GetCategories() Method](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image8.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image7.png)</span></span>

<span data-ttu-id="17176-134">**圖 3**： 有 ObjectDataSource 叫用`CategoriesBLL`類別的`GetCategories()`方法 ([按一下以檢視完整大小的影像](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="17176-134">**Figure 3**: Have the ObjectDataSource Invoke the `CategoriesBLL` Class's `GetCategories()` Method ([Click to view full-size image](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image9.png))</span></span>


<span data-ttu-id="17176-135">設定後 ObjectDataSource，精靈就會返回我們 DropDownList 的資料來源組態精靈，我們要指定哪些資料欄位值應該會顯示與哪一個應該對應至的 DropDownList 的值`ListItem` s。</span><span class="sxs-lookup"><span data-stu-id="17176-135">After configuring the ObjectDataSource, the wizard returns us to the DropDownList's Data Source Configuration wizard from which we need to specify what data field value should be displayed and which one should correspond to the value of the DropDownList's `ListItem` s.</span></span> <span data-ttu-id="17176-136">具有`CategoryName`顯示欄位和使用`CategoryID`做為值。</span><span class="sxs-lookup"><span data-stu-id="17176-136">Have the `CategoryName` field displayed and use the `CategoryID` as the value.</span></span>


<span data-ttu-id="17176-137">[![分別為文字和值 ListItems，使用類別名稱和 CategoryID 欄位](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image11.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="17176-137">[![Use the CategoryName and CategoryID Fields as the Text and Value for the ListItems, Respectively](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image11.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image10.png)</span></span>

<span data-ttu-id="17176-138">**圖 4**： 使用`CategoryName`和`CategoryID`視欄位`Text`和`Value`如`ListItem`s，分別 ([按一下以檢視完整大小的影像](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="17176-138">**Figure 4**: Use the `CategoryName` and `CategoryID` Fields as the `Text` and `Value` for the `ListItem` s, Respectively ([Click to view full-size image](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image12.png))</span></span>


<span data-ttu-id="17176-139">現在我們有 DropDownList (`Categories`) 的系統中列出的類別。</span><span class="sxs-lookup"><span data-stu-id="17176-139">At this point we have a DropDownList (`Categories`) that lists the categories in the system.</span></span> <span data-ttu-id="17176-140">我們現在要加入的 GridView 會列出這些屬於所選類別目錄的產品。</span><span class="sxs-lookup"><span data-stu-id="17176-140">We now need to add a GridView that lists those products that belong to the selected category.</span></span> <span data-ttu-id="17176-141">我們執行動作，不過前, 先花點時間選取 DropDownList 的智慧標籤中啟用 AutoPostBack 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="17176-141">Before we do, though, take a moment to check the Enable AutoPostBack checkbox in the DropDownList's smart tag.</span></span> <span data-ttu-id="17176-142">中所述*主要/詳細資料篩選與 DropDownList*教學課程，藉由設定 DropDownList`AutoPostBack`屬性`True`頁面將會公佈回每次 DropDownList 值有所變更。</span><span class="sxs-lookup"><span data-stu-id="17176-142">As discussed in the *Master/Detail Filtering With a DropDownList* tutorial, by setting the DropDownList's `AutoPostBack` property to `True` the page will be posted back each time the DropDownList value is changed.</span></span> <span data-ttu-id="17176-143">這會導致重新整理 GridView 顯示新選取的類別目錄的產品。</span><span class="sxs-lookup"><span data-stu-id="17176-143">This will cause the GridView to be refreshed, showing those products for the newly selected category.</span></span> <span data-ttu-id="17176-144">如果`AutoPostBack`屬性設定為`False`（預設值），變更類別目錄不會造成回傳，並因此將不會更新所列的產品。</span><span class="sxs-lookup"><span data-stu-id="17176-144">If the `AutoPostBack` property is set to `False` (the default), changing the category won't cause a postback and therefore won't update the listed products.</span></span>


<span data-ttu-id="17176-145">[![選取 DropDownList 的智慧標籤中啟用 AutoPostBack 核取方塊](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image14.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="17176-145">[![Check the Enable AutoPostBack Checkbox in the DropDownList's Smart Tag](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image14.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image13.png)</span></span>

<span data-ttu-id="17176-146">**圖 5**： 核取啟用 AutoPostBack DropDownList 的智慧標籤中 ([按一下以檢視完整大小的影像](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image15.png))</span><span class="sxs-lookup"><span data-stu-id="17176-146">**Figure 5**: Check the Enable AutoPostBack Checkbox in the DropDownList's Smart Tag ([Click to view full-size image](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image15.png))</span></span>


<span data-ttu-id="17176-147">加入至頁面的 GridView 控制項，才能顯示所選類別目錄的產品。</span><span class="sxs-lookup"><span data-stu-id="17176-147">Add a GridView control to the page in order to display the products for the selected category.</span></span> <span data-ttu-id="17176-148">設定 GridView`ID`至`ProductsInCategory`和繫結到名為新 ObjectDataSource `ProductsInCategoryDataSource`。</span><span class="sxs-lookup"><span data-stu-id="17176-148">Set the GridView's `ID` to `ProductsInCategory` and bind it to a new ObjectDataSource named `ProductsInCategoryDataSource`.</span></span>


<span data-ttu-id="17176-149">[![加入名為 ProductsInCategoryDataSource 新 ObjectDataSource](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image17.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="17176-149">[![Add a New ObjectDataSource Named ProductsInCategoryDataSource](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image17.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image16.png)</span></span>

<span data-ttu-id="17176-150">**圖 6**： 加入新的 ObjectDataSource 名為`ProductsInCategoryDataSource`([按一下以檢視完整大小的影像](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image18.png))</span><span class="sxs-lookup"><span data-stu-id="17176-150">**Figure 6**: Add a New ObjectDataSource Named `ProductsInCategoryDataSource` ([Click to view full-size image](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image18.png))</span></span>


<span data-ttu-id="17176-151">因此，它會叫用設定 ObjectDataSource`ProductsBLL`類別的`GetProductsByCategoryID(categoryID)`方法。</span><span class="sxs-lookup"><span data-stu-id="17176-151">Configure the ObjectDataSource so that it invokes the `ProductsBLL` class's `GetProductsByCategoryID(categoryID)` method.</span></span>


<span data-ttu-id="17176-152">[![已叫用 GetProductsByCategoryID(categoryID) 方法 ObjectDataSource](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image20.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="17176-152">[![Have the ObjectDataSource Invoke the GetProductsByCategoryID(categoryID) Method](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image20.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image19.png)</span></span>

<span data-ttu-id="17176-153">**圖 7**： 有 ObjectDataSource 叫用`GetProductsByCategoryID(categoryID)`方法 ([按一下以檢視完整大小的影像](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image21.png))</span><span class="sxs-lookup"><span data-stu-id="17176-153">**Figure 7**: Have the ObjectDataSource Invoke the `GetProductsByCategoryID(categoryID)` Method ([Click to view full-size image](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image21.png))</span></span>


<span data-ttu-id="17176-154">因為`GetProductsByCategoryID(categoryID)`方法接受輸入參數，在精靈的最後一個步驟中，我們可以指定參數值的來源。</span><span class="sxs-lookup"><span data-stu-id="17176-154">Since the `GetProductsByCategoryID(categoryID)` method takes in an input parameter, in the final step of the wizard we can specify the source of the parameter value.</span></span> <span data-ttu-id="17176-155">若要顯示這些產品，從選取的類別，具有 取自參數`Categories`DropDownList。</span><span class="sxs-lookup"><span data-stu-id="17176-155">In order to display those products from the selected category, have the parameter pulled from the `Categories` DropDownList.</span></span>


<span data-ttu-id="17176-156">[![從選取類別 DropDownList 取得 categoryID 參數值](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image23.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image22.png)</span><span class="sxs-lookup"><span data-stu-id="17176-156">[![Get the categoryID Parameter Value from the Selected Categories DropDownList](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image23.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image22.png)</span></span>

<span data-ttu-id="17176-157">**圖 8**： 取得*`categoryID`* 參數值，從選取類別 DropDownList ([按一下以檢視完整大小的影像](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image24.png))</span><span class="sxs-lookup"><span data-stu-id="17176-157">**Figure 8**: Get the *`categoryID`* Parameter Value from the Selected Categories DropDownList ([Click to view full-size image](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image24.png))</span></span>


<span data-ttu-id="17176-158">在精靈完成後 GridView 將會有 BoundField 每個產品的屬性。</span><span class="sxs-lookup"><span data-stu-id="17176-158">After completing the wizard the GridView will have a BoundField for each of the product properties.</span></span> <span data-ttu-id="17176-159">讓我們來清除這些 BoundFields 使得只有`ProductName`， `UnitPrice`， `UnitsInStock`，和`UnitsOnOrder`BoundFields 會顯示。</span><span class="sxs-lookup"><span data-stu-id="17176-159">Let's clean up these BoundFields so that only the `ProductName`, `UnitPrice`, `UnitsInStock`, and `UnitsOnOrder` BoundFields are displayed.</span></span> <span data-ttu-id="17176-160">歡迎剩餘 BoundFields 中加入任何欄位層級設定 (例如格式化`UnitPrice`為貨幣)。</span><span class="sxs-lookup"><span data-stu-id="17176-160">Feel free to add any field-level settings to the remaining BoundFields (such as formatting the `UnitPrice` as a currency).</span></span> <span data-ttu-id="17176-161">進行這些變更之後，GridView 的宣告式標記看起來應該如下所示：</span><span class="sxs-lookup"><span data-stu-id="17176-161">After making these changes, the GridView's declarative markup should look similar to the following:</span></span>


[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample1.aspx)]

<span data-ttu-id="17176-162">現在我們有正常運作的主要/詳細資料報表，屬於所選類別目錄的產品的順序顯示名稱、 單位價格、 庫存量和單位。</span><span class="sxs-lookup"><span data-stu-id="17176-162">At this point we have a fully functioning master/detail report that shows the name, unit price, units in stock, and units on order for those products that belong to the selected category.</span></span>


<span data-ttu-id="17176-163">[![從選取類別 DropDownList 取得 categoryID 參數值](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image26.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="17176-163">[![Get the categoryID Parameter Value from the Selected Categories DropDownList](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image26.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image25.png)</span></span>

<span data-ttu-id="17176-164">**圖 9**： 取得*`categoryID`* 參數值，從選取類別 DropDownList ([按一下以檢視完整大小的影像](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image27.png))</span><span class="sxs-lookup"><span data-stu-id="17176-164">**Figure 9**: Get the *`categoryID`* Parameter Value from the Selected Categories DropDownList ([Click to view full-size image](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image27.png))</span></span>


## <a name="step-2-displaying-a-footer-in-the-gridview"></a><span data-ttu-id="17176-165">步驟 2： 在 GridView 中顯示頁尾</span><span class="sxs-lookup"><span data-stu-id="17176-165">Step 2: Displaying a Footer in the GridView</span></span>

<span data-ttu-id="17176-166">GridView 控制項可以顯示頁首和頁尾資料列。</span><span class="sxs-lookup"><span data-stu-id="17176-166">The GridView control can display both a header and footer row.</span></span> <span data-ttu-id="17176-167">這些資料列會顯示依據的值`ShowHeader`和`ShowFooter`屬性，分別與`ShowHeader`預設為`True`和`ShowFooter`至`False`。</span><span class="sxs-lookup"><span data-stu-id="17176-167">These rows are displayed depending on the values of the `ShowHeader` and `ShowFooter` properties, respectively, with `ShowHeader` defaulting to `True` and `ShowFooter` to `False`.</span></span> <span data-ttu-id="17176-168">若要在 GridView 中只包含頁尾設定其`ShowFooter`屬性`True`。</span><span class="sxs-lookup"><span data-stu-id="17176-168">To include a footer in the GridView simply set its `ShowFooter` property to `True`.</span></span>


<span data-ttu-id="17176-169">[![設定為 True 的 GridView ShowFooter 屬性](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image29.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image28.png)</span><span class="sxs-lookup"><span data-stu-id="17176-169">[![Set the GridView's ShowFooter Property to True](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image29.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image28.png)</span></span>

<span data-ttu-id="17176-170">**圖 10**： 設定 GridView`ShowFooter`屬性`True`([按一下以檢視完整大小的影像](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image30.png))</span><span class="sxs-lookup"><span data-stu-id="17176-170">**Figure 10**: Set the GridView's `ShowFooter` Property to `True` ([Click to view full-size image](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image30.png))</span></span>


<span data-ttu-id="17176-171">頁尾資料列會有一個資料格的每個 GridView; 中定義的欄位不過，這些資料格預設為空白。</span><span class="sxs-lookup"><span data-stu-id="17176-171">The footer row has a cell for each of the fields defined in the GridView; however, these cells are empty by default.</span></span> <span data-ttu-id="17176-172">請花一點時間瀏覽器中檢視進度。</span><span class="sxs-lookup"><span data-stu-id="17176-172">Take a moment to view our progress in a browser.</span></span> <span data-ttu-id="17176-173">與`ShowFooter`內容現在設定為`True`，GridView 包含空白的頁尾資料列。</span><span class="sxs-lookup"><span data-stu-id="17176-173">With the `ShowFooter` property now set to `True`, the GridView includes an empty footer row.</span></span>


<span data-ttu-id="17176-174">[![GridView 現在包含頁尾資料列](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image32.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image31.png)</span><span class="sxs-lookup"><span data-stu-id="17176-174">[![The GridView Now Includes a Footer Row](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image32.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image31.png)</span></span>

<span data-ttu-id="17176-175">**圖 11**: GridView 現在包含頁尾資料列 ([按一下以檢視完整大小的影像](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image33.png))</span><span class="sxs-lookup"><span data-stu-id="17176-175">**Figure 11**: The GridView Now Includes a Footer Row ([Click to view full-size image](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image33.png))</span></span>


<span data-ttu-id="17176-176">圖 11 頁尾資料列不會突顯出來，因為它具有白色背景。</span><span class="sxs-lookup"><span data-stu-id="17176-176">The footer row in Figure 11 doesn't stand out, as it has a white background.</span></span> <span data-ttu-id="17176-177">讓我們來建立`FooterStyle`CSS 類別`Styles.css`，指定深紅色背景，然後設定`GridView.skin`面板檔案中的`DataWebControls`GridView 的單位為這個 CSS 類別的佈景主題`FooterStyle`的`CssClass`屬性。</span><span class="sxs-lookup"><span data-stu-id="17176-177">Let's create a `FooterStyle` CSS class in `Styles.css` that specifies a dark red background and then configure the `GridView.skin` Skin file in the `DataWebControls` Theme to assign this CSS class to the GridView's `FooterStyle`'s `CssClass` property.</span></span> <span data-ttu-id="17176-178">如果您需要複習面板和佈景主題，請參閱上一步[顯示資料與 ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="17176-178">If you need to brush up on Skins and Themes, refer back to the [Displaying Data With the ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) tutorial.</span></span>

<span data-ttu-id="17176-179">藉由新增下列 CSS 類別以啟動`Styles.css`:</span><span class="sxs-lookup"><span data-stu-id="17176-179">Start by adding the following CSS class to `Styles.css`:</span></span>


[!code-css[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample2.css)]

<span data-ttu-id="17176-180">`FooterStyle` CSS 類別是類似的樣式`HeaderStyle`類別，雖然`HeaderStyle`的背景色彩越深微妙的地方，並以粗體字顯示其文字。</span><span class="sxs-lookup"><span data-stu-id="17176-180">The `FooterStyle` CSS class is similar in style to the `HeaderStyle` class, although the `HeaderStyle`'s background color is subtlety darker and its text is displayed in a bold font.</span></span> <span data-ttu-id="17176-181">此外，頁尾中的文字會靠右對齊而標頭的文字置中。</span><span class="sxs-lookup"><span data-stu-id="17176-181">Furthermore, the text in the footer is right-aligned whereas the header's text is centered.</span></span>

<span data-ttu-id="17176-182">接下來，若要將這個 CSS 類別與每個 GridView 頁尾產生關聯，開啟`GridView.skin`檔案`DataWebControls`佈景主題和集`FooterStyle`的`CssClass`屬性。</span><span class="sxs-lookup"><span data-stu-id="17176-182">Next, to associate this CSS class with every GridView's footer, open the `GridView.skin` file in the `DataWebControls` Theme and set the `FooterStyle`'s `CssClass` property.</span></span> <span data-ttu-id="17176-183">在此新增之後的檔案標記看起來應該像：</span><span class="sxs-lookup"><span data-stu-id="17176-183">After this addition the file's markup should look like:</span></span>


[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample3.aspx)]

<span data-ttu-id="17176-184">如螢幕擷取畫面所示，這項變更可讓您在頁尾突顯更清楚。</span><span class="sxs-lookup"><span data-stu-id="17176-184">As the screen shot below shows, this change makes the footer stand out more clearly.</span></span>


<span data-ttu-id="17176-185">[![在 GridView 的頁尾資料列現在有紅的背景色彩](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image35.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image34.png)</span><span class="sxs-lookup"><span data-stu-id="17176-185">[![The GridView's Footer Row Now Has a Reddish Background Color](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image35.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image34.png)</span></span>

<span data-ttu-id="17176-186">**圖 12**: GridView 頁尾資料列現在有紅的背景色彩 ([按一下以檢視完整大小的影像](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image36.png))</span><span class="sxs-lookup"><span data-stu-id="17176-186">**Figure 12**: The GridView's Footer Row Now Has a Reddish Background Color ([Click to view full-size image](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image36.png))</span></span>


## <a name="step-3-computing-the-summary-data"></a><span data-ttu-id="17176-187">步驟 3： 計算摘要資料</span><span class="sxs-lookup"><span data-stu-id="17176-187">Step 3: Computing the Summary Data</span></span>

<span data-ttu-id="17176-188">與顯示的 GridView 的頁尾，我們對向的下一步挑戰是如何計算摘要資料。</span><span class="sxs-lookup"><span data-stu-id="17176-188">With the GridView's footer displayed, the next challenge facing us is how to compute the summary data.</span></span> <span data-ttu-id="17176-189">有兩種方式來計算此彙總的資訊：</span><span class="sxs-lookup"><span data-stu-id="17176-189">There are two ways to compute this aggregate information:</span></span>

1. <span data-ttu-id="17176-190">透過 SQL 查詢中，我們無法發出的其他查詢資料庫以計算某個特定類別的摘要資料。</span><span class="sxs-lookup"><span data-stu-id="17176-190">Through a SQL query we could issue an additional query to the database to compute the summary data for a particular category.</span></span> <span data-ttu-id="17176-191">SQL 包含數個彙總函式，連同`GROUP BY`子句來指定哪些資料應該彙總的資料。</span><span class="sxs-lookup"><span data-stu-id="17176-191">SQL includes a number of aggregate functions along with a `GROUP BY` clause to specify the data over which the data should be summarized.</span></span> <span data-ttu-id="17176-192">下列 SQL 查詢會回復所需的資訊：</span><span class="sxs-lookup"><span data-stu-id="17176-192">The following SQL query would bring back the needed information:</span></span>  

    [!code-sql[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample4.sql)]

    <span data-ttu-id="17176-193">當然您不想要發行此查詢，直接從`SummaryDataInFooter.aspx`頁面上，而是藉由建立中的方法`ProductsTableAdapter`和`ProductsBLL`。</span><span class="sxs-lookup"><span data-stu-id="17176-193">Of course you wouldn't want to issue this query directly from the `SummaryDataInFooter.aspx` page, but rather by creating a method in the `ProductsTableAdapter` and the `ProductsBLL`.</span></span>
2. <span data-ttu-id="17176-194">計算這項資訊，因為它正在加入至 GridView 中所述[自訂格式化時資料](custom-formatting-based-upon-data-cs.md)教學課程中，在 GridView 的`RowDataBound`事件處理常式會在每個資料列之後 GridView 加入一次引發其已資料繫結。</span><span class="sxs-lookup"><span data-stu-id="17176-194">Compute this information as it's being added to the GridView as discussed in [Custom Formatting Based Upon Data](custom-formatting-based-upon-data-cs.md) tutorial, the GridView's `RowDataBound` event handler fires once for each row being added to the GridView after its been databound.</span></span> <span data-ttu-id="17176-195">藉由建立此事件的事件處理常式中，我們會保留所執行的總我們想要的值的彙總。</span><span class="sxs-lookup"><span data-stu-id="17176-195">By creating an event handler for this event we can keep a running total of the values we want to aggregate.</span></span> <span data-ttu-id="17176-196">最後一個之後的資料列已經繫結至 GridView 總計和計算平均時所需的資訊。</span><span class="sxs-lookup"><span data-stu-id="17176-196">After the last data row has been bound to the GridView we have the totals and the information needed to compute the average.</span></span>

<span data-ttu-id="17176-197">我通常採用第二種方法，它會將路線儲存至資料庫，以及商務邏輯層和資料存取層中實作的摘要功能所需的投入時間，但兩種方法即已足夠。</span><span class="sxs-lookup"><span data-stu-id="17176-197">I typically employ the second approach as it saves a trip to the database and the effort needed to implement the summary functionality in the Data Access Layer and Business Logic Layer, but either approach would suffice.</span></span> <span data-ttu-id="17176-198">此教學課程中我們使用第二個選項，並且追蹤累計總數使用`RowDataBound`事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="17176-198">For this tutorial let's use the second option and keep track of the running total using the `RowDataBound` event handler.</span></span>

<span data-ttu-id="17176-199">建立`RowDataBound`GridView 選取設計工具中，按一下閃電圖示從 [屬性] 視窗中，按兩下 GridView 的事件處理常式`RowDataBound`事件。</span><span class="sxs-lookup"><span data-stu-id="17176-199">Create a `RowDataBound` event handler for the GridView by selecting the GridView in the Designer, clicking the lightning bolt icon from the Properties window, and double-clicking the `RowDataBound` event.</span></span> <span data-ttu-id="17176-200">或者，您可以從在 ASP.NET 程式碼後置類別檔最上方的下拉式清單選取 GridView 以及其 RowDataBound 事件。</span><span class="sxs-lookup"><span data-stu-id="17176-200">Alternatively, you can select the GridView and its RowDataBound event from the drop-down lists at the top of the ASP.NET code-behind class file.</span></span> <span data-ttu-id="17176-201">這會建立新的事件處理常式，名為`ProductsInCategory_RowDataBound`中`SummaryDataInFooter.aspx`網頁的程式碼後置類別。</span><span class="sxs-lookup"><span data-stu-id="17176-201">This will create a new event handler named `ProductsInCategory_RowDataBound` in the `SummaryDataInFooter.aspx` page's code-behind class.</span></span>


[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample5.vb)]

<span data-ttu-id="17176-202">為了維護執行總數中，我們需要定義變數超出範圍的事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="17176-202">In order to maintain a running total we need to define variables outside of the scope of the event handler.</span></span> <span data-ttu-id="17176-203">建立下列四個的頁面層級變數：</span><span class="sxs-lookup"><span data-stu-id="17176-203">Create the following four page-level variables:</span></span>

- <span data-ttu-id="17176-204">`_totalUnitPrice`型別 `Decimal`</span><span class="sxs-lookup"><span data-stu-id="17176-204">`_totalUnitPrice`, of type `Decimal`</span></span>
- <span data-ttu-id="17176-205">`_totalNonNullUnitPriceCount`型別 `Integer`</span><span class="sxs-lookup"><span data-stu-id="17176-205">`_totalNonNullUnitPriceCount`, of type `Integer`</span></span>
- <span data-ttu-id="17176-206">`_totalUnitsInStock`型別 `Integer`</span><span class="sxs-lookup"><span data-stu-id="17176-206">`_totalUnitsInStock`, of type `Integer`</span></span>
- <span data-ttu-id="17176-207">`_totalUnitsOnOrder`型別 `Integer`</span><span class="sxs-lookup"><span data-stu-id="17176-207">`_totalUnitsOnOrder`, of type `Integer`</span></span>

<span data-ttu-id="17176-208">接下來，撰寫程式碼中的每個資料列時發生遞增這些三個變數`RowDataBound`事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="17176-208">Next, write the code to increment these three variables for each data row encountered in the `RowDataBound` event handler.</span></span>


[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample6.vb)]

<span data-ttu-id="17176-209">`RowDataBound`事件處理常式一開始會確保我們正在處理的資料列。</span><span class="sxs-lookup"><span data-stu-id="17176-209">The `RowDataBound` event handler starts by ensuring that we're dealing with a DataRow.</span></span> <span data-ttu-id="17176-210">一旦已建立的`Northwind.ProductsRow`只繫結至的執行個體`GridViewRow`物件存放至`e.Row`儲存在變數`product`。</span><span class="sxs-lookup"><span data-stu-id="17176-210">Once that's been established, the `Northwind.ProductsRow` instance that was just bound to the `GridViewRow` object in `e.Row` is stored in the variable `product`.</span></span> <span data-ttu-id="17176-211">接下來，執行的總變數目前產品之對應值以遞增 (假設它們不包含資料庫`NULL`值)。</span><span class="sxs-lookup"><span data-stu-id="17176-211">Next, running total variables are incremented by the current product's corresponding values (assuming that they don't contain a database `NULL` value).</span></span> <span data-ttu-id="17176-212">我們會持續追蹤的兩個執行`UnitPrice`總計和非數字`NULL``UnitPrice`記錄因為平均的價格是兩個數值的商數。</span><span class="sxs-lookup"><span data-stu-id="17176-212">We keep track of both the running `UnitPrice` total and the number of non-`NULL` `UnitPrice` records because the average price is the quotient of these two numbers.</span></span>

## <a name="step-4-displaying-the-summary-data-in-the-footer"></a><span data-ttu-id="17176-213">步驟 4： 在頁尾中顯示摘要資料</span><span class="sxs-lookup"><span data-stu-id="17176-213">Step 4: Displaying the Summary Data in the Footer</span></span>

<span data-ttu-id="17176-214">總計的摘要資料，最後一個步驟是在 GridView 的頁尾資料列中顯示。</span><span class="sxs-lookup"><span data-stu-id="17176-214">With the summary data totaled, the last step is to display it in the GridView's footer row.</span></span> <span data-ttu-id="17176-215">這項工作，也可以完成以程式設計方式透過`RowDataBound`事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="17176-215">This task, too, can be accomplished programmatically through the `RowDataBound` event handler.</span></span> <span data-ttu-id="17176-216">請記得，`RowDataBound`事件處理常式引發的*每*繫結至 GridView，包括頁尾資料列的資料列。</span><span class="sxs-lookup"><span data-stu-id="17176-216">Recall that the `RowDataBound` event handler fires for *every* row that's bound to the GridView, including the footer row.</span></span> <span data-ttu-id="17176-217">因此，我們可以加強我們要使用下列程式碼的頁尾資料列中顯示資料的事件處理常式：</span><span class="sxs-lookup"><span data-stu-id="17176-217">Therefore, we can augment our event handler to display the data in the footer row using the following code:</span></span>


[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample7.vb)]

<span data-ttu-id="17176-218">因為頁尾資料列加入至 GridView 加入所有資料列後，我們可以確信時由我們已準備好可以在將已完成的累計總數的計算頁尾顯示摘要資料。</span><span class="sxs-lookup"><span data-stu-id="17176-218">Since the footer row is added to the GridView after all of the data rows have been added, we can be confident that by the time we're ready to display the summary data in the footer the running total calculations will have completed.</span></span> <span data-ttu-id="17176-219">最後一個步驟中，接著是頁尾的資料格中設定這些值。</span><span class="sxs-lookup"><span data-stu-id="17176-219">The last step, then, is to set these values in the footer's cells.</span></span>

<span data-ttu-id="17176-220">若要在特定的頁尾資料格中顯示文字，請使用`e.Row.Cells(index).Text = value`，其中`Cells`索引 0 開始。</span><span class="sxs-lookup"><span data-stu-id="17176-220">To display text in a particular footer cell, use `e.Row.Cells(index).Text = value`, where the `Cells` indexing starts at 0.</span></span> <span data-ttu-id="17176-221">下列程式碼會計算平均價格 （總價除以產品數目），並顯示之單元的總數以及在股票圖和單位上的 GridView 的適當頁尾資料格的順序。</span><span class="sxs-lookup"><span data-stu-id="17176-221">The following code computes the average price (the total price divided by the number of products) and displays it along with the total number of units in stock and units on order in the appropriate footer cells of the GridView.</span></span>


[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample8.vb)]

<span data-ttu-id="17176-222">圖 13 在已加入此程式碼之後顯示的報表。</span><span class="sxs-lookup"><span data-stu-id="17176-222">Figure 13 shows the report after this code has been added.</span></span> <span data-ttu-id="17176-223">請注意如何`ToString("c")`像貨幣格式化，平均價格摘要資訊。</span><span class="sxs-lookup"><span data-stu-id="17176-223">Note how the `ToString("c")` causes the average price summary information to be formatted like a currency.</span></span>


<span data-ttu-id="17176-224">[![在 GridView 的頁尾資料列現在有紅的背景色彩](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image38.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image37.png)</span><span class="sxs-lookup"><span data-stu-id="17176-224">[![The GridView's Footer Row Now Has a Reddish Background Color](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image38.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image37.png)</span></span>

<span data-ttu-id="17176-225">**圖 13**: GridView 頁尾資料列現在有紅的背景色彩 ([按一下以檢視完整大小的影像](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image39.png))</span><span class="sxs-lookup"><span data-stu-id="17176-225">**Figure 13**: The GridView's Footer Row Now Has a Reddish Background Color ([Click to view full-size image](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image39.png))</span></span>


## <a name="summary"></a><span data-ttu-id="17176-226">總結</span><span class="sxs-lookup"><span data-stu-id="17176-226">Summary</span></span>

<span data-ttu-id="17176-227">顯示摘要資料是報表的一般需求，和 GridView 控制項可讓您輕鬆地在其頁尾資料列中包含這類資訊。</span><span class="sxs-lookup"><span data-stu-id="17176-227">Displaying summary data is a common report requirement, and the GridView control makes it easy to include such information in its footer row.</span></span> <span data-ttu-id="17176-228">顯示頁尾資料列時的 GridView`ShowFooter`屬性設定為`True`，而且可以透過程式設計方式設定其儲存格有文字`RowDataBound`事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="17176-228">The footer row is displayed when the GridView's `ShowFooter` property is set to `True` and can have the text in its cells set programmatically through the `RowDataBound` event handler.</span></span> <span data-ttu-id="17176-229">計算摘要資料可能可以藉由重新查詢資料庫，或使用 ASP.NET 網頁的程式碼後置類別中的程式碼，來以程式設計方式計算摘要資料。</span><span class="sxs-lookup"><span data-stu-id="17176-229">Computing the summary data can either be done by re-querying the database or by using code in the ASP.NET page's code-behind class to programmatically compute the summary data.</span></span>

<span data-ttu-id="17176-230">本教學課程結束時，我們檢驗使用 GridView、 DetailsView 和在 FormView 的控制項的自訂格式。</span><span class="sxs-lookup"><span data-stu-id="17176-230">This tutorial concludes our examination of custom formatting with the GridView, DetailsView, and FormView controls.</span></span> <span data-ttu-id="17176-231">我們的下一個教學課程一開始我們瀏覽的插入、 更新和刪除資料，使用這些相同的控制項。</span><span class="sxs-lookup"><span data-stu-id="17176-231">Our next tutorial kicks off our exploration of inserting, updating, and deleting data using these same controls.</span></span>

<span data-ttu-id="17176-232">祝您程式設計 ！</span><span class="sxs-lookup"><span data-stu-id="17176-232">Happy Programming!</span></span>

## <a name="about-the-author"></a><span data-ttu-id="17176-233">關於作者</span><span class="sxs-lookup"><span data-stu-id="17176-233">About the Author</span></span>

<span data-ttu-id="17176-234">[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。</span><span class="sxs-lookup"><span data-stu-id="17176-234">[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), author of seven ASP/ASP.NET books and founder of [4GuysFromRolla.com](http://www.4guysfromrolla.com), has been working with Microsoft Web technologies since 1998.</span></span> <span data-ttu-id="17176-235">Scott 可做為獨立顧問、 訓練和寫入器。</span><span class="sxs-lookup"><span data-stu-id="17176-235">Scott works as an independent consultant, trainer, and writer.</span></span> <span data-ttu-id="17176-236">他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。</span><span class="sxs-lookup"><span data-stu-id="17176-236">His latest book is [*Sams Teach Yourself ASP.NET 2.0 in 24 Hours*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco).</span></span> <span data-ttu-id="17176-237">他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。</span><span class="sxs-lookup"><span data-stu-id="17176-237">He can be reached at [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) or via his blog, which can be found at [http://ScottOnWriting.NET](http://ScottOnWriting.NET).</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="17176-238">上一步</span><span class="sxs-lookup"><span data-stu-id="17176-238">Previous</span></span>](using-the-formview-s-templates-vb.md)
