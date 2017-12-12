---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-vb
title: "使用 DropDownList (VB) 進行篩選的主要/詳細資料 |Microsoft 文件"
author: rick-anderson
description: "在本教學課程中，我們會看到如何 DropDownList 控制項和 GridView 中選取的清單項目的詳細資料中顯示的主要記錄。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: ea44717e-ab2e-46cd-a692-e4a9c0de194c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 88e5c65c100bea3cc39b1e08b1aa8a622b4ce7a6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="masterdetail-filtering-with-a-dropdownlist-vb"></a><span data-ttu-id="f05fa-103">使用 DropDownList (VB) 進行篩選的主要/詳細資料</span><span class="sxs-lookup"><span data-stu-id="f05fa-103">Master/Detail Filtering With a DropDownList (VB)</span></span>
====================
<span data-ttu-id="f05fa-104">由[Scott Mitchell](https://twitter.com/ScottOnWriting)</span><span class="sxs-lookup"><span data-stu-id="f05fa-104">by [Scott Mitchell](https://twitter.com/ScottOnWriting)</span></span>

<span data-ttu-id="f05fa-105">[下載範例應用程式](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_7_VB.exe)或[下載 PDF](master-detail-filtering-with-a-dropdownlist-vb/_static/datatutorial07vb1.pdf)</span><span class="sxs-lookup"><span data-stu-id="f05fa-105">[Download Sample App](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_7_VB.exe) or [Download PDF](master-detail-filtering-with-a-dropdownlist-vb/_static/datatutorial07vb1.pdf)</span></span>

> <span data-ttu-id="f05fa-106">在本教學課程中，我們會看到如何 DropDownList 控制項和 GridView 中選取的清單項目的詳細資料中顯示的主要記錄。</span><span class="sxs-lookup"><span data-stu-id="f05fa-106">In this tutorial we'll see how to display the master records in a DropDownList control and the details of the selected list item in a GridView.</span></span>


## <a name="introduction"></a><span data-ttu-id="f05fa-107">簡介</span><span class="sxs-lookup"><span data-stu-id="f05fa-107">Introduction</span></span>

<span data-ttu-id="f05fa-108">一種常見的報表是*主要/詳細資料報表*中的報表一開始會顯示某些的 「 主要 」 的記錄集。</span><span class="sxs-lookup"><span data-stu-id="f05fa-108">A common type of report is the *master/detail report*, in which the report begins by showing some set of "master" records.</span></span> <span data-ttu-id="f05fa-109">使用者可以再向下鑽研到其中一個主要記錄中，藉此檢視主要記錄的 「 詳細資料。 」</span><span class="sxs-lookup"><span data-stu-id="f05fa-109">The user can then drill down into one of the master records, thereby viewing that master record's "details."</span></span> <span data-ttu-id="f05fa-110">主要/詳細資料報告會視覺化為一對多關聯性，例如報告的理想選擇顯示所有類別，然後由使用者選取特定的類別，並顯示其相關聯的產品。</span><span class="sxs-lookup"><span data-stu-id="f05fa-110">Master/detail reports are an ideal choice for visualizing one-to-many relationships, such as a report showing all of the categories and then allowing a user to select a particular category and display its associated products.</span></span> <span data-ttu-id="f05fa-111">此外，主要/詳細資料報表是適用於顯示詳細的資訊，特別是 「 寬的 」 的資料表 （是有很多資料行）。</span><span class="sxs-lookup"><span data-stu-id="f05fa-111">Additionally, master/detail reports are useful for displaying detailed information from particularly "wide" tables (ones that have a lot of columns).</span></span> <span data-ttu-id="f05fa-112">例如，「 主要 」 的層級的主要/詳細資料報表可能會在資料庫中，顯示只的產品名稱和單位價格的產品和向下切入特定產品，則會顯示其他產品欄位 (類別、 供應商、 每個單位的數量和等等）。</span><span class="sxs-lookup"><span data-stu-id="f05fa-112">For example, the "master" level of a master/detail report might show just the product name and unit price of the products in the database, and drilling down into a particular product would show the additional product fields (category, supplier, quantity per unit, and so on).</span></span>

<span data-ttu-id="f05fa-113">有許多方法可以用實作主要/詳細資料報表。</span><span class="sxs-lookup"><span data-stu-id="f05fa-113">There are many ways with which a master/detail report can be implemented.</span></span> <span data-ttu-id="f05fa-114">透過這和接下來三個教學課程介紹各種不同的主要/詳細資料報表。</span><span class="sxs-lookup"><span data-stu-id="f05fa-114">Over this and the next three tutorials we'll look at a variety of master/detail reports.</span></span> <span data-ttu-id="f05fa-115">在本教學課程中，我們會看到如何顯示中的主要記錄[DropDownList 控制項](https://msdn.microsoft.com/en-us/library/dtx91y0z.aspx)和 GridView 中選取的清單項目的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="f05fa-115">In this tutorial we'll see how to display the master records in a [DropDownList control](https://msdn.microsoft.com/en-us/library/dtx91y0z.aspx) and the details of the selected list item in a GridView.</span></span> <span data-ttu-id="f05fa-116">尤其，本教學課程的主要/詳細資料報表會列出分類和產品資訊。</span><span class="sxs-lookup"><span data-stu-id="f05fa-116">In particular, this tutorial's master/detail report will list category and product information.</span></span>

## <a name="step-1-displaying-the-categories-in-a-dropdownlist"></a><span data-ttu-id="f05fa-117">步驟 1: DropDownList 以顯示類別目錄</span><span class="sxs-lookup"><span data-stu-id="f05fa-117">Step 1: Displaying the Categories in a DropDownList</span></span>

<span data-ttu-id="f05fa-118">我們的主要/詳細資料報表會列出與選取的清單項目的產品顯示的 DropDownList 中的類別，進一步向下 GridView 中的頁面。</span><span class="sxs-lookup"><span data-stu-id="f05fa-118">Our master/detail report will list the categories in a DropDownList, with the selected list item's products displayed further down in the page in a GridView.</span></span> <span data-ttu-id="f05fa-119">晚於我們在第一個工作，則具有 dropdownlist 顯示類別目錄。</span><span class="sxs-lookup"><span data-stu-id="f05fa-119">The first task ahead of us, then, is to have the categories displayed in a DropDownList.</span></span> <span data-ttu-id="f05fa-120">開啟`FilterByDropDownList.aspx`頁面`Filtering`資料夾中，拖曳 DropDownList 上從 [工具箱] 拖曳至頁面的設計工具，並設定其`ID`屬性`Categories`。</span><span class="sxs-lookup"><span data-stu-id="f05fa-120">Open the `FilterByDropDownList.aspx` page in the `Filtering` folder, drag on a DropDownList from the Toolbox onto the page's designer, and set its `ID` property to `Categories`.</span></span> <span data-ttu-id="f05fa-121">接下來，按一下 從 DropDownList 的智慧標籤的 選擇資料來源 連結。</span><span class="sxs-lookup"><span data-stu-id="f05fa-121">Next, click on the Choose Data Source link from the DropDownList's smart tag.</span></span> <span data-ttu-id="f05fa-122">這會顯示資料來源組態精靈。</span><span class="sxs-lookup"><span data-stu-id="f05fa-122">This will display the Data Source Configuration wizard.</span></span>


<span data-ttu-id="f05fa-123">[![指定的資料來源的 DropDownList](master-detail-filtering-with-a-dropdownlist-vb/_static/image2.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f05fa-123">[![Specify the DropDownList's Data Source](master-detail-filtering-with-a-dropdownlist-vb/_static/image2.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image1.png)</span></span>

<span data-ttu-id="f05fa-124">**圖 1**： 指定 DropDownList 的資料來源 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f05fa-124">**Figure 1**: Specify the DropDownList's Data Source ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-vb/_static/image3.png))</span></span>


<span data-ttu-id="f05fa-125">選擇加入名為新 ObjectDataSource`CategoriesDataSource`會叫用`CategoriesBLL`類別的`GetCategories()`方法。</span><span class="sxs-lookup"><span data-stu-id="f05fa-125">Choose to add a new ObjectDataSource named `CategoriesDataSource` that invokes the `CategoriesBLL` class's `GetCategories()` method.</span></span>


<span data-ttu-id="f05fa-126">[![加入名為 CategoriesDataSource 新 ObjectDataSource](master-detail-filtering-with-a-dropdownlist-vb/_static/image5.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="f05fa-126">[![Add a New ObjectDataSource Named CategoriesDataSource](master-detail-filtering-with-a-dropdownlist-vb/_static/image5.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image4.png)</span></span>

<span data-ttu-id="f05fa-127">**圖 2**： 加入新的 ObjectDataSource 名為`CategoriesDataSource`([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="f05fa-127">**Figure 2**: Add a New ObjectDataSource Named `CategoriesDataSource` ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-vb/_static/image6.png))</span></span>


<span data-ttu-id="f05fa-128">[![選擇使用 CategoriesBLL 類別](master-detail-filtering-with-a-dropdownlist-vb/_static/image8.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="f05fa-128">[![Choose to Use the CategoriesBLL Class](master-detail-filtering-with-a-dropdownlist-vb/_static/image8.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image7.png)</span></span>

<span data-ttu-id="f05fa-129">**圖 3**： 選擇使用`CategoriesBLL`類別 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-vb/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="f05fa-129">**Figure 3**: Choose to Use the `CategoriesBLL` Class ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-vb/_static/image9.png))</span></span>


<span data-ttu-id="f05fa-130">[![設定為使用 GetCategories() 方法 ObjectDataSource](master-detail-filtering-with-a-dropdownlist-vb/_static/image11.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="f05fa-130">[![Configure the ObjectDataSource to Use the GetCategories() Method](master-detail-filtering-with-a-dropdownlist-vb/_static/image11.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image10.png)</span></span>

<span data-ttu-id="f05fa-131">**圖 4**： 設定要使用 ObjectDataSource`GetCategories()`方法 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-vb/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="f05fa-131">**Figure 4**: Configure the ObjectDataSource to Use the `GetCategories()` Method ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-vb/_static/image12.png))</span></span>


<span data-ttu-id="f05fa-132">設定我們仍需要指定哪些資料來源欄位應該會顯示在 DropDownList 和 ObjectDataSource 之後其中一個應該做為清單項目值相關聯。</span><span class="sxs-lookup"><span data-stu-id="f05fa-132">After configuring the ObjectDataSource we still need to specify what data source field should be displayed in DropDownList and which one should be associated as the value for the list item.</span></span> <span data-ttu-id="f05fa-133">具有`CategoryName`欄位顯示器和`CategoryID`做為每個清單項目值。</span><span class="sxs-lookup"><span data-stu-id="f05fa-133">Have the `CategoryName` field as the display and `CategoryID` as the value for each list item.</span></span>


<span data-ttu-id="f05fa-134">[![此類別名稱欄位，然後使用 CategoryID 有 DropDownList 顯示做為值](master-detail-filtering-with-a-dropdownlist-vb/_static/image14.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="f05fa-134">[![Have the DropDownList Display the CategoryName Field and Use CategoryID as the Value](master-detail-filtering-with-a-dropdownlist-vb/_static/image14.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image13.png)</span></span>

<span data-ttu-id="f05fa-135">**圖 5**: DropDownList 顯示`CategoryName`欄位並使用`CategoryID`做為值 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-vb/_static/image15.png))</span><span class="sxs-lookup"><span data-stu-id="f05fa-135">**Figure 5**: Have the DropDownList Display the `CategoryName` Field and Use `CategoryID` as the Value ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-vb/_static/image15.png))</span></span>


<span data-ttu-id="f05fa-136">現在我們有從記錄中會填入的 DropDownList 控制項`Categories`資料表 （全部在六秒內完成）。</span><span class="sxs-lookup"><span data-stu-id="f05fa-136">At this point we have a DropDownList control that's populated with the records from the `Categories` table (all accomplished in about six seconds).</span></span> <span data-ttu-id="f05fa-137">圖 6 為止顯示進度，透過瀏覽器檢視時。</span><span class="sxs-lookup"><span data-stu-id="f05fa-137">Figure 6 shows our progress thus far when viewed through a browser.</span></span>


<span data-ttu-id="f05fa-138">[![下拉式清單會列出目前的分類](master-detail-filtering-with-a-dropdownlist-vb/_static/image17.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="f05fa-138">[![A Drop-Down Lists the Current Categories](master-detail-filtering-with-a-dropdownlist-vb/_static/image17.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image16.png)</span></span>

<span data-ttu-id="f05fa-139">**圖 6**: 下拉式清單會列出目前的分類 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-vb/_static/image18.png))</span><span class="sxs-lookup"><span data-stu-id="f05fa-139">**Figure 6**: A Drop-Down Lists the Current Categories ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-vb/_static/image18.png))</span></span>


## <a name="step-2-adding-the-products-gridview"></a><span data-ttu-id="f05fa-140">步驟 2： 加入產品 GridView</span><span class="sxs-lookup"><span data-stu-id="f05fa-140">Step 2: Adding the Products GridView</span></span>

<span data-ttu-id="f05fa-141">在我們的主要/詳細資料報表中的最後一個步驟是列出與選取的類別相關聯的產品。</span><span class="sxs-lookup"><span data-stu-id="f05fa-141">That last step in our master/detail report is to list the products associated with the selected category.</span></span> <span data-ttu-id="f05fa-142">若要達成此目的，GridView 加入頁面並建立名為新 ObjectDataSource `productsDataSource`。</span><span class="sxs-lookup"><span data-stu-id="f05fa-142">To accomplish this, add a GridView to the page and create a new ObjectDataSource named `productsDataSource`.</span></span> <span data-ttu-id="f05fa-143">具有`productsDataSource`控制項選出從其資料`ProductsBLL`類別的`GetProductsByCategoryID(categoryID)`方法。</span><span class="sxs-lookup"><span data-stu-id="f05fa-143">Have the `productsDataSource` control cull its data from the `ProductsBLL` class's `GetProductsByCategoryID(categoryID)` method.</span></span>


<span data-ttu-id="f05fa-144">[![選取 GetProductsByCategoryID(categoryID) 方法](master-detail-filtering-with-a-dropdownlist-vb/_static/image20.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="f05fa-144">[![Select the GetProductsByCategoryID(categoryID) Method](master-detail-filtering-with-a-dropdownlist-vb/_static/image20.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image19.png)</span></span>

<span data-ttu-id="f05fa-145">**圖 7**： 選取`GetProductsByCategoryID(categoryID)`方法 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-vb/_static/image21.png))</span><span class="sxs-lookup"><span data-stu-id="f05fa-145">**Figure 7**: Select the `GetProductsByCategoryID(categoryID)` Method ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-vb/_static/image21.png))</span></span>


<span data-ttu-id="f05fa-146">選擇這個方法之後，ObjectDataSource 精靈會提示輸入我們方法的值 *`categoryID`* 參數。</span><span class="sxs-lookup"><span data-stu-id="f05fa-146">After choosing this method, the ObjectDataSource wizard prompts us for the value for the method's *`categoryID`* parameter.</span></span> <span data-ttu-id="f05fa-147">若要使用的所選值`categories`DropDownList 項目設定參數來源控制與以 ControlID `Categories`。</span><span class="sxs-lookup"><span data-stu-id="f05fa-147">To use the value of the selected `categories` DropDownList item set the Parameter source to Control and the ControlID to `Categories`.</span></span>


<span data-ttu-id="f05fa-148">[![CategoryID 參數值設定為類別 DropDownList](master-detail-filtering-with-a-dropdownlist-vb/_static/image23.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image22.png)</span><span class="sxs-lookup"><span data-stu-id="f05fa-148">[![Set the categoryID Parameter to the Value of the Categories DropDownList](master-detail-filtering-with-a-dropdownlist-vb/_static/image23.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image22.png)</span></span>

<span data-ttu-id="f05fa-149">**圖 8**： 設定 *`categoryID`* 參數的值`Categories`DropDownList ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-vb/_static/image24.png))</span><span class="sxs-lookup"><span data-stu-id="f05fa-149">**Figure 8**: Set the *`categoryID`* Parameter to the Value of the `Categories` DropDownList ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-vb/_static/image24.png))</span></span>


<span data-ttu-id="f05fa-150">請花一點時間簽出的瀏覽器中的進度。</span><span class="sxs-lookup"><span data-stu-id="f05fa-150">Take a moment to check out our progress in a browser.</span></span> <span data-ttu-id="f05fa-151">當第一次造訪的頁面時，這些產品屬於所選類別目錄 （如圖 9 所示），會顯示 （飲料），但變更 DropDownList 不會更新資料。</span><span class="sxs-lookup"><span data-stu-id="f05fa-151">When first visiting the page, those products belong to the selected category (Beverages) are displayed (as shown in Figure 9), but changing the DropDownList doesn't update the data.</span></span> <span data-ttu-id="f05fa-152">這是因為更新 GridView 會因發生回傳。</span><span class="sxs-lookup"><span data-stu-id="f05fa-152">This is because a postback must occur for the GridView to update.</span></span> <span data-ttu-id="f05fa-153">若要完成這項作業中，我們有兩個選項 （其中都不需要撰寫任何程式碼）：</span><span class="sxs-lookup"><span data-stu-id="f05fa-153">To accomplish this we have two options (neither of which requires writing any code):</span></span>

- <span data-ttu-id="f05fa-154">**設定類別 DropDownList**[AutoPostBack 屬性](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.listcontrol.autopostback%28VS.80%29.aspx)**為 True。**</span><span class="sxs-lookup"><span data-stu-id="f05fa-154">**Set the categories DropDownList's**[AutoPostBack property](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.listcontrol.autopostback%28VS.80%29.aspx)**to True.**</span></span> <span data-ttu-id="f05fa-155">（您可以完成此作業藉由檢查 DropDownList 的智慧標籤的 [啟用 AutoPostBack] 選項。）如此將會觸發回傳，只要 DropDownList 的選取項目由使用者變更。</span><span class="sxs-lookup"><span data-stu-id="f05fa-155">(You can accomplish this by checking the Enable AutoPostBack option in the DropDownList's smart tag.) This will trigger a postback whenever the DropDownList's selected item is changed by the user.</span></span> <span data-ttu-id="f05fa-156">因此，當使用者選取新的類別從 DropDownList 將發生回傳，而且會以新選取的分類的產品更新 GridView。</span><span class="sxs-lookup"><span data-stu-id="f05fa-156">Therefore, when the user selects a new category from the DropDownList a postback will ensue and the GridView will be updated with the products for the newly selected category.</span></span> <span data-ttu-id="f05fa-157">（這是已在本教學課程使用的方法）。</span><span class="sxs-lookup"><span data-stu-id="f05fa-157">(This is the approach I've used in this tutorial.)</span></span>
- <span data-ttu-id="f05fa-158">**加入按鈕 Web 控制項旁 DropDownList。**</span><span class="sxs-lookup"><span data-stu-id="f05fa-158">**Add a Button Web control next to the DropDownList.**</span></span> <span data-ttu-id="f05fa-159">設定其`Text`屬性來重新整理或類似的項目。</span><span class="sxs-lookup"><span data-stu-id="f05fa-159">Set its `Text` property to Refresh or something similar.</span></span> <span data-ttu-id="f05fa-160">使用此方法時，使用者必須選取新的類別，然後按一下 [] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="f05fa-160">With this approach, the user will need to select a new category and then click the Button.</span></span> <span data-ttu-id="f05fa-161">按一下按鈕，將會導致回傳，並更新以列出所選類別的那些產品 GridView。</span><span class="sxs-lookup"><span data-stu-id="f05fa-161">Clicking the Button will cause a postback and update the GridView to list those products of the selected category.</span></span>

<span data-ttu-id="f05fa-162">數字 9 和 10 說明主要/詳細資料中的報表動作。</span><span class="sxs-lookup"><span data-stu-id="f05fa-162">Figures 9 and 10 illustrate the master/detail report in action.</span></span>


<span data-ttu-id="f05fa-163">[![當第一次瀏覽頁面，會顯示飲料產品](master-detail-filtering-with-a-dropdownlist-vb/_static/image26.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="f05fa-163">[![When First Visiting the Page, the Beverage Products are Displayed](master-detail-filtering-with-a-dropdownlist-vb/_static/image26.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image25.png)</span></span>

<span data-ttu-id="f05fa-164">**圖 9**： 當第一次瀏覽頁面，會顯示飲料產品 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-vb/_static/image27.png))</span><span class="sxs-lookup"><span data-stu-id="f05fa-164">**Figure 9**: When First Visiting the Page, the Beverage Products are Displayed ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-vb/_static/image27.png))</span></span>


<span data-ttu-id="f05fa-165">[![選取新的產品 （產生） 會自動導致回傳，更新 GridView](master-detail-filtering-with-a-dropdownlist-vb/_static/image29.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image28.png)</span><span class="sxs-lookup"><span data-stu-id="f05fa-165">[![Selecting a New Product (Produce) Automatically Causes a PostBack, Updating the GridView](master-detail-filtering-with-a-dropdownlist-vb/_static/image29.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image28.png)</span></span>

<span data-ttu-id="f05fa-166">**圖 10**： 選取新的產品 （產生） 會自動導致回傳，更新 GridView ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-vb/_static/image30.png))</span><span class="sxs-lookup"><span data-stu-id="f05fa-166">**Figure 10**: Selecting a New Product (Produce) Automatically Causes a PostBack, Updating the GridView ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-vb/_static/image30.png))</span></span>


## <a name="adding-a----choose-a-category----list-item"></a><span data-ttu-id="f05fa-167">可以加入 「-選擇分類-」 清單項目</span><span class="sxs-lookup"><span data-stu-id="f05fa-167">Adding a "-- Choose a Category --" List Item</span></span>

<span data-ttu-id="f05fa-168">當第一次造訪`FilterByDropDownList.aspx`頁面 DropDownList 的第一個清單項目 （飲料） 根據預設，顯示飲料產品在 GridView 中選取的類別。</span><span class="sxs-lookup"><span data-stu-id="f05fa-168">When first visiting the `FilterByDropDownList.aspx` page the categories DropDownList's first list item (Beverages) is selected by default, showing the beverage products in the GridView.</span></span> <span data-ttu-id="f05fa-169">而非顯示第一個類別目錄的產品，我們可能會想要改為在 DropDownList 項目選取，顯示類似，「-選擇分類-」。</span><span class="sxs-lookup"><span data-stu-id="f05fa-169">Rather than showing the first category's products, we may want to instead have a DropDownList item selected that says something like, "-- Choose a Category --".</span></span>

<span data-ttu-id="f05fa-170">DropDownList 中加入新的清單項目，請前往 [屬性] 視窗，按一下省略符號在`Items`屬性。</span><span class="sxs-lookup"><span data-stu-id="f05fa-170">To add a new list item to the DropDownList, go to the Properties window and click on the ellipses in the `Items` property.</span></span> <span data-ttu-id="f05fa-171">加入新的清單項目與`Text`"-選擇一個類別目錄-"和`Value` `-1`。</span><span class="sxs-lookup"><span data-stu-id="f05fa-171">Add a new list item with the `Text` "-- Choose a Category --" and the `Value` `-1`.</span></span>


<span data-ttu-id="f05fa-172">[![新增-選擇類別目錄--清單項目](master-detail-filtering-with-a-dropdownlist-vb/_static/image32.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image31.png)</span><span class="sxs-lookup"><span data-stu-id="f05fa-172">[![Add a -- Choose a Category -- List Item](master-detail-filtering-with-a-dropdownlist-vb/_static/image32.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image31.png)</span></span>

<span data-ttu-id="f05fa-173">**圖 11**： 新增為--選擇類別目錄--清單項目 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-vb/_static/image33.png))</span><span class="sxs-lookup"><span data-stu-id="f05fa-173">**Figure 11**: Add a -- Choose a Category -- List Item ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-vb/_static/image33.png))</span></span>


<span data-ttu-id="f05fa-174">或者，您可以將下列標記加入至 DropDownList 新增清單項目：</span><span class="sxs-lookup"><span data-stu-id="f05fa-174">Alternatively, you can add the list item by adding the following markup to the DropDownList:</span></span>


[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-vb/samples/sample1.aspx)]

<span data-ttu-id="f05fa-175">此外，我們需要將 DropDownList 控制項的`AppendDataBoundItems`設為 True，因為類別繫結至 DropDownList 從 ObjectDataSource 時將會被覆寫的任何手動新增清單項目如果`AppendDataBoundItems`不是 True。</span><span class="sxs-lookup"><span data-stu-id="f05fa-175">Additionally, we need to set the DropDownList control's `AppendDataBoundItems` to True because when the categories are bound to the DropDownList from the ObjectDataSource they'll overwrite any manually-added list items if `AppendDataBoundItems` isn't True.</span></span>


![AppendDataBoundItems 屬性設為 True](master-detail-filtering-with-a-dropdownlist-vb/_static/image34.png)

<span data-ttu-id="f05fa-177">**圖 12**： 設定`AppendDataBoundItems`屬性設定為 True</span><span class="sxs-lookup"><span data-stu-id="f05fa-177">**Figure 12**: Set the `AppendDataBoundItems` Property to True</span></span>


<span data-ttu-id="f05fa-178">這些變更之後，請先瀏覽頁面的 「-選擇分類-」 選項已選取，並不顯示任何產品時。</span><span class="sxs-lookup"><span data-stu-id="f05fa-178">After these changes, when first visiting the page the "-- Choose a Category --" option is selected and no products are displayed.</span></span>


<span data-ttu-id="f05fa-179">[![初始載入的頁面會不顯示任何產品](master-detail-filtering-with-a-dropdownlist-vb/_static/image36.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image35.png)</span><span class="sxs-lookup"><span data-stu-id="f05fa-179">[![On the Initial Page Load No Products are Displayed](master-detail-filtering-with-a-dropdownlist-vb/_static/image36.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image35.png)</span></span>

<span data-ttu-id="f05fa-180">**圖 13**： 上初始頁面載入不會顯示產品 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-vb/_static/image37.png))</span><span class="sxs-lookup"><span data-stu-id="f05fa-180">**Figure 13**: On the Initial Page Load No Products are Displayed ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-vb/_static/image37.png))</span></span>


<span data-ttu-id="f05fa-181">因為 「-選擇分類-」 清單項目被選取時顯示任何產品的原因是因為它的值是`-1`和含有資料庫中沒有產品`CategoryID`的`-1`。</span><span class="sxs-lookup"><span data-stu-id="f05fa-181">The reason no products are displayed when because the "-- Choose a Category --" list item is selected is because its value is `-1` and there are no products in the database with a `CategoryID` of `-1`.</span></span> <span data-ttu-id="f05fa-182">如果這是您想在此時完成時的行為 ！</span><span class="sxs-lookup"><span data-stu-id="f05fa-182">If this is the behavior you want then you're done at this point!</span></span> <span data-ttu-id="f05fa-183">如果您想要顯示的但是*所有*一個類別目錄選取 「-選擇分類-」 清單項目時，返回`ProductsBLL`類別和自訂`GetProductsByCategoryID(categoryID)`方法，使它會叫用`GetProducts()`方法如果傳入中 *`categoryID`* 參數小於零：</span><span class="sxs-lookup"><span data-stu-id="f05fa-183">If, however, you want to display *all* of the categories when the "-- Choose a Category --" list item is selected, return to the `ProductsBLL` class and customize the `GetProductsByCategoryID(categoryID)` method so that it invokes the `GetProducts()` method if the passed in *`categoryID`* parameter is less than zero:</span></span>


[!code-vb[Main](master-detail-filtering-with-a-dropdownlist-vb/samples/sample2.vb)]

<span data-ttu-id="f05fa-184">這裡使用的技巧是類似的方法，我們用來顯示所有供應商回[宣告式參數](../basic-reporting/declarative-parameters-cs.md)教學課程，雖然此範例中，我們會使用值為`-1`表示應該所有記錄擷取與`Nothing`。</span><span class="sxs-lookup"><span data-stu-id="f05fa-184">The technique used here is similar to the approach we used to display all suppliers back in the [Declarative Parameters](../basic-reporting/declarative-parameters-cs.md) tutorial, although for this example we're using a value of `-1` to indicate that all records should be retrieved as opposed to `Nothing`.</span></span> <span data-ttu-id="f05fa-185">這是因為 *`categoryID`* 參數`GetProductsByCategoryID(categoryID)`方法應為整數值傳入，而宣告式參數教學課程中我們所傳入的字串輸入參數。</span><span class="sxs-lookup"><span data-stu-id="f05fa-185">This is because the *`categoryID`* parameter of the `GetProductsByCategoryID(categoryID)` method expects as integer value passed in, whereas in the Declarative Parameters tutorial we were passing in a string input parameter.</span></span>

<span data-ttu-id="f05fa-186">圖 14 顯示的螢幕擷取畫面`FilterByDropDownList.aspx`如果選取 「-選擇分類-」 的選項。</span><span class="sxs-lookup"><span data-stu-id="f05fa-186">Figure 14 shows a screen shot of `FilterByDropDownList.aspx` when the "-- Choose a Category --" option is selected.</span></span> <span data-ttu-id="f05fa-187">在這裡，根據預設，會顯示所有產品，使用者可以選擇特定的類別目錄來縮小顯示。</span><span class="sxs-lookup"><span data-stu-id="f05fa-187">Here, all of the products are displayed by default, and the user can narrow the display by choosing a specific category.</span></span>


<span data-ttu-id="f05fa-188">[![所有的產品是現在所列的預設值](master-detail-filtering-with-a-dropdownlist-vb/_static/image39.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image38.png)</span><span class="sxs-lookup"><span data-stu-id="f05fa-188">[![All of the Products are Now Listed By Default](master-detail-filtering-with-a-dropdownlist-vb/_static/image39.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image38.png)</span></span>

<span data-ttu-id="f05fa-189">**圖 14**： 所有產品會立即列出預設 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-vb/_static/image40.png))</span><span class="sxs-lookup"><span data-stu-id="f05fa-189">**Figure 14**: All of the Products are Now Listed By Default ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-vb/_static/image40.png))</span></span>


## <a name="summary"></a><span data-ttu-id="f05fa-190">總結</span><span class="sxs-lookup"><span data-stu-id="f05fa-190">Summary</span></span>

<span data-ttu-id="f05fa-191">顯示階層關聯的資料時，這通常有助於呈現的資料，使用主要/詳細資料報表，使用者可以從中啟動研究的資料階層的頂端，並向下鑽研到詳細資料。</span><span class="sxs-lookup"><span data-stu-id="f05fa-191">When displaying hierarchically-related data, it often helps to present the data using master/detail reports, from which the user can start perusing the data from the top of the hierarchy and drill down into details.</span></span> <span data-ttu-id="f05fa-192">在此教學課程中我們會檢查建置簡單的主要/詳細資料報表，顯示所選類別目錄的產品。</span><span class="sxs-lookup"><span data-stu-id="f05fa-192">In this tutorial we examined building a simple master/detail report showing a selected category's products.</span></span> <span data-ttu-id="f05fa-193">這是透過 DropDownList 的分類和產品屬於所選類別目錄的 GridView 的清單以完成。</span><span class="sxs-lookup"><span data-stu-id="f05fa-193">This was accomplished by using a DropDownList for the list of categories and a GridView for the products belonging to the selected category.</span></span>

<span data-ttu-id="f05fa-194">在[下一個教學課程](master-detail-filtering-with-two-dropdownlists-vb.md)我們 DropDownList 介面的一個步驟，使用兩個 DropDownLists。</span><span class="sxs-lookup"><span data-stu-id="f05fa-194">In the [next tutorial](master-detail-filtering-with-two-dropdownlists-vb.md) we'll take the DropDownList interface one step further, using two DropDownLists.</span></span>

<span data-ttu-id="f05fa-195">祝您程式設計 ！</span><span class="sxs-lookup"><span data-stu-id="f05fa-195">Happy Programming!</span></span>

## <a name="about-the-author"></a><span data-ttu-id="f05fa-196">關於作者</span><span class="sxs-lookup"><span data-stu-id="f05fa-196">About the Author</span></span>

<span data-ttu-id="f05fa-197">[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。</span><span class="sxs-lookup"><span data-stu-id="f05fa-197">[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), author of seven ASP/ASP.NET books and founder of [4GuysFromRolla.com](http://www.4guysfromrolla.com), has been working with Microsoft Web technologies since 1998.</span></span> <span data-ttu-id="f05fa-198">Scott 可做為獨立顧問、 訓練和寫入器。</span><span class="sxs-lookup"><span data-stu-id="f05fa-198">Scott works as an independent consultant, trainer, and writer.</span></span> <span data-ttu-id="f05fa-199">他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。</span><span class="sxs-lookup"><span data-stu-id="f05fa-199">His latest book is [*Sams Teach Yourself ASP.NET 2.0 in 24 Hours*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco).</span></span> <span data-ttu-id="f05fa-200">他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。</span><span class="sxs-lookup"><span data-stu-id="f05fa-200">He can be reached at [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) or via his blog, which can be found at [http://ScottOnWriting.NET](http://ScottOnWriting.NET).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="f05fa-201">[上一頁](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)
[下一頁](master-detail-filtering-with-two-dropdownlists-vb.md)</span><span class="sxs-lookup"><span data-stu-id="f05fa-201">[Previous](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)
[Next](master-detail-filtering-with-two-dropdownlists-vb.md)</span></span>
