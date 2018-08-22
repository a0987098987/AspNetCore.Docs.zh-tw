---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-vb
title: 主版/詳細篩選使用 dropdownlist 進行 (VB) |Microsoft Docs
author: rick-anderson
description: 在本教學課程中，我們看到如何來顯示單一的 web 網頁，以顯示 要顯示的 'master' 的記錄和 DataList 使用 dropdownlist 進行主版/詳細報告...
ms.author: riande
ms.date: 07/18/2007
ms.assetid: ad0f1014-1eff-465f-bdc6-93058de00e44
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: 13d67b3f4f2613c820baa3ec52d49ad6ea556f9d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825508"
---
<a name="masterdetail-filtering-with-a-dropdownlist-vb"></a><span data-ttu-id="ba209-103">主版/詳細篩選使用 dropdownlist 進行 (VB)</span><span class="sxs-lookup"><span data-stu-id="ba209-103">Master/Detail Filtering With a DropDownList (VB)</span></span>
====================
<span data-ttu-id="ba209-104">藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)</span><span class="sxs-lookup"><span data-stu-id="ba209-104">by [Scott Mitchell](https://twitter.com/ScottOnWriting)</span></span>

<span data-ttu-id="ba209-105">[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_33_VB.exe)或[下載 PDF](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/datatutorial33vb1.pdf)</span><span class="sxs-lookup"><span data-stu-id="ba209-105">[Download Sample App](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_33_VB.exe) or [Download PDF](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/datatutorial33vb1.pdf)</span></span>

> <span data-ttu-id="ba209-106">在本教學課程中，我們看到如何來顯示單一的 web 網頁，以顯示 「 主要 」 的記錄和 DataList，以顯示 [詳細資料] 中使用 dropdownlist 進行主版/詳細報告。</span><span class="sxs-lookup"><span data-stu-id="ba209-106">In this tutorial we see how to display master/detail reports in a single web page using DropDownLists to display the "master" records and a DataList to display the "details".</span></span>


## <a name="introduction"></a><span data-ttu-id="ba209-107">簡介</span><span class="sxs-lookup"><span data-stu-id="ba209-107">Introduction</span></span>

<span data-ttu-id="ba209-108">主版/詳細報告，其中我們第一次建立在先前使用 GridView[主版/詳細篩選使用 DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md)教學課程中，一開始會顯示某些的 「 主要 」 的記錄集。</span><span class="sxs-lookup"><span data-stu-id="ba209-108">The master/detail report, which we first created using a GridView in the earlier [Master/Detail Filtering With a DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) tutorial, begins by showing some set of "master" records.</span></span> <span data-ttu-id="ba209-109">使用者可以再向下切入到其中一個主要記錄中，藉此檢視的主要記錄的 [詳細資料。]</span><span class="sxs-lookup"><span data-stu-id="ba209-109">The user can then drill down into one of the master records, thereby viewing that master record's "details."</span></span> <span data-ttu-id="ba209-110">主版/詳細報告是以視覺化方式呈現一對多關聯性，以及顯示 （亦即有許多資料行） 特別 「 寬 」 的資料表的詳細的資訊的理想選擇。</span><span class="sxs-lookup"><span data-stu-id="ba209-110">Master/detail reports are an ideal choice for visualizing one-to-many relationships and for displaying detailed information from particularly "wide" tables (ones that have a lot of columns).</span></span> <span data-ttu-id="ba209-111">我們探討了如何實作在先前的教學課程使用 GridView 和 DetailsView 控制項的主版/詳細報告。</span><span class="sxs-lookup"><span data-stu-id="ba209-111">We've explored how to implement master/detail reports using the GridView and DetailsView controls in previous tutorials.</span></span> <span data-ttu-id="ba209-112">在本教學課程 和 下一步 的兩個，我們會檢查這些概念，但著重於使用 DataList 和 Repeater 控制項改。</span><span class="sxs-lookup"><span data-stu-id="ba209-112">In this tutorial and the next two, we'll reexamine these concepts, but focus on using DataList and Repeater controls instead.</span></span>

<span data-ttu-id="ba209-113">在本教學課程中，我們將探討使用 dropdownlist 進行包含的 「 主要 」 記錄，[詳細資料] 中顯示的記錄資料清單。</span><span class="sxs-lookup"><span data-stu-id="ba209-113">In this tutorial, we'll look at using a DropDownList to contain the "master" records, with the "details" records displayed in a DataList.</span></span>

## <a name="step-1-adding-the-masterdetail-tutorial-web-pages"></a><span data-ttu-id="ba209-114">步驟 1： 加入主版/詳細教學課程的 Web 網頁</span><span class="sxs-lookup"><span data-stu-id="ba209-114">Step 1: Adding the Master/Detail Tutorial Web Pages</span></span>

<span data-ttu-id="ba209-115">在開始本教學課程之前，讓我們先花一點時間加入資料夾和我們需要針對本教學課程和因應使用 DataList 與重複項控制項的主版/詳細報告的下一步 兩個 ASP.NET 網頁。</span><span class="sxs-lookup"><span data-stu-id="ba209-115">Before we start this tutorial, let's first take a moment to add the folder and ASP.NET pages we'll need for this tutorial and the next two dealing with master/detail reports using the DataList and Repeater controls.</span></span> <span data-ttu-id="ba209-116">藉由建立新的資料夾中名為專案啟動`DataListRepeaterFiltering`。</span><span class="sxs-lookup"><span data-stu-id="ba209-116">Start by creating a new folder in the project named `DataListRepeaterFiltering`.</span></span> <span data-ttu-id="ba209-117">接下來，新增到此資料夾，將所有設定成使用主版頁面的 下列五個 ASP.NET 頁面`Site.master`:</span><span class="sxs-lookup"><span data-stu-id="ba209-117">Next, add the following five ASP.NET pages to this folder, having all of them configured to use the master page `Site.master`:</span></span>

- `Default.aspx`
- `FilterByDropDownList.aspx`
- `CategoryListMaster.aspx`
- `ProductsForCategoryDetails.aspx`
- `CategoriesAndProducts.aspx`


![建立 DataListRepeaterFiltering 資料夾，並新增這些教學課程的 ASP.NET 網頁](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image1.png)

<span data-ttu-id="ba209-119">**圖 1**： 建立`DataListRepeaterFiltering`資料夾並新增教學課程的 ASP.NET 頁面</span><span class="sxs-lookup"><span data-stu-id="ba209-119">**Figure 1**: Create a `DataListRepeaterFiltering` Folder and Add the Tutorial ASP.NET Pages</span></span>


<span data-ttu-id="ba209-120">接下來，開啟`Default.aspx`頁面上，並拖曳`SectionLevelTutorialListing.ascx`從使用者控制`UserControls`拖曳至設計介面上的資料夾。</span><span class="sxs-lookup"><span data-stu-id="ba209-120">Next, open the `Default.aspx` page and drag the `SectionLevelTutorialListing.ascx` User Control from the `UserControls` folder onto the Design surface.</span></span> <span data-ttu-id="ba209-121">此使用者控制項中，我們在中建立[主版頁面與網站導覽](../introduction/master-pages-and-site-navigation-vb.md)教學課程中，會列舉站台對應，並顯示從目前的區段項目符號清單中的教學課程。</span><span class="sxs-lookup"><span data-stu-id="ba209-121">This User Control, which we created in the [Master Pages and Site Navigation](../introduction/master-pages-and-site-navigation-vb.md) tutorial, enumerates the site map and displays the tutorials from the current section in a bulleted list.</span></span>


<span data-ttu-id="ba209-122">[![將 SectionLevelTutorialListing.ascx 使用者控制項新增至 Default.aspx](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image3.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="ba209-122">[![Add the SectionLevelTutorialListing.ascx User Control to Default.aspx](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image3.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image2.png)</span></span>

<span data-ttu-id="ba209-123">**圖 2**： 新增`SectionLevelTutorialListing.ascx`使用者控制項`Default.aspx`([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="ba209-123">**Figure 2**: Add the `SectionLevelTutorialListing.ascx` User Control to `Default.aspx` ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image4.png))</span></span>


<span data-ttu-id="ba209-124">若要有項目符號清單顯示主版/詳細教學課程，我們將建立，我們要將它們新增至站台對應。</span><span class="sxs-lookup"><span data-stu-id="ba209-124">In order to have the bulleted list display the master/detail tutorials we'll be creating, we need to add them to the site map.</span></span> <span data-ttu-id="ba209-125">開啟`Web.sitemap`檔案，並 「 顯示的資料與 DataList 和 Repeater 」 站台對應節點標記之後新增下列標記：</span><span class="sxs-lookup"><span data-stu-id="ba209-125">Open the `Web.sitemap` file and add the following markup after the "Displaying Data with the DataList and Repeater" site map node markup:</span></span>

[!code-xml[Main](master-detail-filtering-with-a-dropdownlist-datalist-vb/samples/sample1.xml)]


![更新站台對應，以包含新的 ASP.NET 網頁](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image5.png)

<span data-ttu-id="ba209-127">**圖 3**： 更新站台對應，以包含新的 ASP.NET 網頁</span><span class="sxs-lookup"><span data-stu-id="ba209-127">**Figure 3**: Update the Site Map to Include the New ASP.NET Pages</span></span>


## <a name="step-2-displaying-the-categories-in-a-dropdownlist"></a><span data-ttu-id="ba209-128">步驟 2： 顯示在 DropDownList 中的類別</span><span class="sxs-lookup"><span data-stu-id="ba209-128">Step 2: Displaying the Categories in a DropDownList</span></span>

<span data-ttu-id="ba209-129">我們的主要/詳細資料報表會顯示選取的清單項目的產品清單的類別中的下拉式清單中，進一步在 DataList 的頁面。</span><span class="sxs-lookup"><span data-stu-id="ba209-129">Our master/detail report will list the categories in a DropDownList, with the selected list item's products displayed further down in the page in a DataList.</span></span> <span data-ttu-id="ba209-130">超越我們，第一項工作，則將顯示在 DropDownList 中的類別。</span><span class="sxs-lookup"><span data-stu-id="ba209-130">The first task ahead of us, then, is to have the categories displayed in a DropDownList.</span></span> <span data-ttu-id="ba209-131">首先開啟`FilterByDropDownList.aspx`頁面中`DataListRepeaterFiltering`資料夾，然後從 [工具箱] 拖曳至頁面的設計工具的 dropdownlist 進行拖曳。</span><span class="sxs-lookup"><span data-stu-id="ba209-131">Start by opening the `FilterByDropDownList.aspx` page in the `DataListRepeaterFiltering` folder and drag a DropDownList from the Toolbox onto the page's designer.</span></span> <span data-ttu-id="ba209-132">接下來，設定 DropDownList`ID`屬性設`Categories`。</span><span class="sxs-lookup"><span data-stu-id="ba209-132">Next, set the DropDownList's `ID` property to `Categories`.</span></span> <span data-ttu-id="ba209-133">按一下 從 DropDownList 的智慧標籤的 選擇資料來源 連結，建立名為新 ObjectDataSource `CategoriesDataSource`。</span><span class="sxs-lookup"><span data-stu-id="ba209-133">Click on the Choose Data Source link from the DropDownList's smart tag and create a new ObjectDataSource named `CategoriesDataSource`.</span></span>


<span data-ttu-id="ba209-134">[![新增名為 CategoriesDataSource 新 ObjectDataSource](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image7.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="ba209-134">[![Add a New ObjectDataSource Named CategoriesDataSource](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image7.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image6.png)</span></span>

<span data-ttu-id="ba209-135">**圖 4**： 新增新的 ObjectDataSource 名為`CategoriesDataSource`([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="ba209-135">**Figure 4**: Add a New ObjectDataSource Named `CategoriesDataSource` ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image8.png))</span></span>


<span data-ttu-id="ba209-136">設定，它會叫用的新 ObjectDataSource`CategoriesBLL`類別的`GetCategories()`方法。</span><span class="sxs-lookup"><span data-stu-id="ba209-136">Configure the new ObjectDataSource such that it invokes the `CategoriesBLL` class's `GetCategories()` method.</span></span> <span data-ttu-id="ba209-137">設定我們仍然需要指定哪些資料來源欄位應該會顯示在 DropDownList 和 ObjectDataSource 後其中一個應該為每個清單項目的值相關聯。</span><span class="sxs-lookup"><span data-stu-id="ba209-137">After configuring the ObjectDataSource we still need to specify what data source field should be displayed in the DropDownList and which one should be associated as the value for each list item.</span></span> <span data-ttu-id="ba209-138">已`CategoryName`欄位做為顯示和`CategoryID`做為每個清單項目的值。</span><span class="sxs-lookup"><span data-stu-id="ba209-138">Have the `CategoryName` field as the display and `CategoryID` as the value for each list item.</span></span>


<span data-ttu-id="ba209-139">[![做為值的類別名稱 欄位和使用 CategoryID 有 DropDownList 顯示](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image10.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="ba209-139">[![Have the DropDownList Display the CategoryName Field and Use CategoryID as the Value](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image10.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image9.png)</span></span>

<span data-ttu-id="ba209-140">**圖 5**: DropDownList 顯示`CategoryName`欄位並使用`CategoryID`做為值 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image11.png))</span><span class="sxs-lookup"><span data-stu-id="ba209-140">**Figure 5**: Have the DropDownList Display the `CategoryName` Field and Use `CategoryID` as the Value ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image11.png))</span></span>


<span data-ttu-id="ba209-141">現在我們有 DropDownList 控制項，其中會填入來自記錄`Categories`資料表 （全部在大約六秒內完成）。</span><span class="sxs-lookup"><span data-stu-id="ba209-141">At this point we have a DropDownList control that's populated with the records from the `Categories` table (all accomplished in about six seconds).</span></span> <span data-ttu-id="ba209-142">圖 6 顯示我們進行到目前為止透過瀏覽器檢視時。</span><span class="sxs-lookup"><span data-stu-id="ba209-142">Figure 6 shows our progress thus far when viewed through a browser.</span></span>


<span data-ttu-id="ba209-143">[![下拉式清單會列出目前的類別](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image13.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="ba209-143">[![A Drop-Down Lists the Current Categories](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image13.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image12.png)</span></span>

<span data-ttu-id="ba209-144">**圖 6**: 下拉式清單會列出目前的類別 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="ba209-144">**Figure 6**: A Drop-Down Lists the Current Categories ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image14.png))</span></span>


## <a name="step-2-adding-the-products-datalist"></a><span data-ttu-id="ba209-145">步驟 2： 加入產品 DataList</span><span class="sxs-lookup"><span data-stu-id="ba209-145">Step 2: Adding the Products DataList</span></span>

<span data-ttu-id="ba209-146">我們的主要/詳細資料報表的最後一個步驟是列出與選取的類別相關聯的產品。</span><span class="sxs-lookup"><span data-stu-id="ba209-146">The last step in our master/detail report is to list the products associated with the selected category.</span></span> <span data-ttu-id="ba209-147">若要這麼做，DataList 新增至頁面，並建立名為新 ObjectDataSource `ProductsByCategoryDataSource`。</span><span class="sxs-lookup"><span data-stu-id="ba209-147">To accomplish this, add a DataList to the page and create a new ObjectDataSource named `ProductsByCategoryDataSource`.</span></span> <span data-ttu-id="ba209-148">已`ProductsByCategoryDataSource`控制項擷取資料的來源`ProductsBLL`類別的`GetProductsByCategoryID(categoryID)`方法。</span><span class="sxs-lookup"><span data-stu-id="ba209-148">Have the `ProductsByCategoryDataSource` control retrieve its data from the `ProductsBLL` class's `GetProductsByCategoryID(categoryID)` method.</span></span> <span data-ttu-id="ba209-149">由於此主版/詳細報告是唯讀的選擇 （無） 選項在 INSERT、 UPDATE 和 DELETE 的索引標籤中。</span><span class="sxs-lookup"><span data-stu-id="ba209-149">Since this master/detail report is read-only, choose the (None) option in the INSERT, UPDATE, and DELETE tabs.</span></span>


<span data-ttu-id="ba209-150">[![選取 GetProductsByCategoryID(categoryID) 方法](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image16.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="ba209-150">[![Select the GetProductsByCategoryID(categoryID) Method](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image16.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image15.png)</span></span>

<span data-ttu-id="ba209-151">**圖 7**： 選取 `GetProductsByCategoryID(categoryID)`方法 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image17.png))</span><span class="sxs-lookup"><span data-stu-id="ba209-151">**Figure 7**: Select the `GetProductsByCategoryID(categoryID)` Method ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image17.png))</span></span>


<span data-ttu-id="ba209-152">按一下 [下一步] 之後, ObjectDataSource 精靈會提示我們輸入之值的來源`GetProductsByCategoryID(categoryID)`方法的*`categoryID`* 參數。</span><span class="sxs-lookup"><span data-stu-id="ba209-152">After clicking Next, the ObjectDataSource wizard prompts us for the source of the value for the `GetProductsByCategoryID(categoryID)` method's *`categoryID`* parameter.</span></span> <span data-ttu-id="ba209-153">若要使用選取的值`categories`DropDownList 項目設定參數來源控制與以 ControlID `Categories`。</span><span class="sxs-lookup"><span data-stu-id="ba209-153">To use the value of the selected `categories` DropDownList item set the Parameter source to Control and the ControlID to `Categories`.</span></span>


<span data-ttu-id="ba209-154">[![設定為值的分類 DropDownList categoryID 參數](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image19.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image18.png)</span><span class="sxs-lookup"><span data-stu-id="ba209-154">[![Set the categoryID Parameter to the Value of the Categories DropDownList](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image19.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image18.png)</span></span>

<span data-ttu-id="ba209-155">**圖 8**： 設定*`categoryID`* 參數的值`Categories`DropDownList ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image20.png))</span><span class="sxs-lookup"><span data-stu-id="ba209-155">**Figure 8**: Set the *`categoryID`* Parameter to the Value of the `Categories` DropDownList ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image20.png))</span></span>


<span data-ttu-id="ba209-156">完成後設定資料來源精靈，Visual Studio 會自動產生`ItemTemplate`的 DataList 顯示的名稱和每個資料欄位的值。</span><span class="sxs-lookup"><span data-stu-id="ba209-156">Upon completing the Configure Data Source wizard, Visual Studio will automatically generate an `ItemTemplate` for the DataList that displays the name and value of each data field.</span></span> <span data-ttu-id="ba209-157">讓我們來增強改用 DataList `ItemTemplate` ，顯示只是產品的名稱、 類別、 供應商、 每單位和價格以及數量`SeparatorTemplate`，會插入`<hr>`各個項目之間的項目。</span><span class="sxs-lookup"><span data-stu-id="ba209-157">Let's enhance the DataList to instead use an `ItemTemplate` that displays just the product's name, category, supplier, quantity per unit, and price along with a `SeparatorTemplate` that injects an `<hr>` element between each item.</span></span> <span data-ttu-id="ba209-158">我要使用`ItemTemplate`中的範例[使用 DataList 與重複項控制項顯示的資料](../displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-vb.md)教學課程中，但是可以隨意使用任何範本標記您找到最美觀。</span><span class="sxs-lookup"><span data-stu-id="ba209-158">I'm going to use the `ItemTemplate` from an example in the [Displaying Data with the DataList and Repeater Controls](../displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-vb.md) tutorial, but feel free to use whatever template markup you find most visually appealing.</span></span>

<span data-ttu-id="ba209-159">進行這些變更之後，您的 DataList 和其 ObjectDataSource 的標記看起來應該如下所示：</span><span class="sxs-lookup"><span data-stu-id="ba209-159">After making these changes, your DataList and its ObjectDataSource's markup should look similar to the following:</span></span>

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-vb/samples/sample2.aspx)]

<span data-ttu-id="ba209-160">花點時間查看我們的瀏覽器中的進度。</span><span class="sxs-lookup"><span data-stu-id="ba209-160">Take a moment to check out our progress in a browser.</span></span> <span data-ttu-id="ba209-161">當第一次瀏覽的頁面，（如 圖 9 所示），會顯示屬於所選取的類別 （飲料） 這些產品，但變更 DropDownList 不會更新資料。</span><span class="sxs-lookup"><span data-stu-id="ba209-161">When first visiting the page, those products belonging to the selected category (Beverages) are displayed (as shown in Figure 9), but changing the DropDownList doesn't update the data.</span></span> <span data-ttu-id="ba209-162">這是因為回傳會因發生更新 DataList。</span><span class="sxs-lookup"><span data-stu-id="ba209-162">This is because a postback must occur for the DataList to update.</span></span> <span data-ttu-id="ba209-163">若要這麼做我們可以設定 DropDownList`AutoPostBack`屬性設`true`或 Button Web 控制項加入頁面。</span><span class="sxs-lookup"><span data-stu-id="ba209-163">To accomplish this we can either set the DropDownList's `AutoPostBack` property to `true` or add a Button Web control to the page.</span></span> <span data-ttu-id="ba209-164">本教學課程中，我選擇設定 DropDownList`AutoPostBack`屬性設`true`。</span><span class="sxs-lookup"><span data-stu-id="ba209-164">For this tutorial, I've opted to set the DropDownList's `AutoPostBack` property to `true`.</span></span>

<span data-ttu-id="ba209-165">圖 9 和 10 說明主要/詳細資料報表動作中。</span><span class="sxs-lookup"><span data-stu-id="ba209-165">Figures 9 and 10 illustrate the master/detail report in action.</span></span>


<span data-ttu-id="ba209-166">[![當第一次瀏覽的頁面，會顯示飲料產品](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image22.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image21.png)</span><span class="sxs-lookup"><span data-stu-id="ba209-166">[![When First Visiting the Page, the Beverage Products are Displayed](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image22.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image21.png)</span></span>

<span data-ttu-id="ba209-167">**圖 9**： 當第一次瀏覽的頁面，會顯示飲料產品 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image23.png))</span><span class="sxs-lookup"><span data-stu-id="ba209-167">**Figure 9**: When First Visiting the Page, the Beverage Products are Displayed ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image23.png))</span></span>


<span data-ttu-id="ba209-168">[![選取新的產品 （產生） 會自動造成回傳，更新 DataList](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image25.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image24.png)</span><span class="sxs-lookup"><span data-stu-id="ba209-168">[![Selecting a New Product (Produce) Automatically Causes a PostBack, Updating the DataList](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image25.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image24.png)</span></span>

<span data-ttu-id="ba209-169">**圖 10**： 選取新的產品 （產生） 會自動造成回傳，更新 DataList ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image26.png))</span><span class="sxs-lookup"><span data-stu-id="ba209-169">**Figure 10**: Selecting a New Product (Produce) Automatically Causes a PostBack, Updating the DataList ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image26.png))</span></span>


## <a name="adding-a----choose-a-category----list-item"></a><span data-ttu-id="ba209-170">新增"--選擇類別目錄-」 清單項目</span><span class="sxs-lookup"><span data-stu-id="ba209-170">Adding a "-- Choose a Category --" List Item</span></span>

<span data-ttu-id="ba209-171">第一次瀏覽時`FilterByDropDownList.aspx`頁面 DropDownList 的第一個清單項目 （飲料） 根據預設，顯示飲料產品 DataList 中選取的類別。</span><span class="sxs-lookup"><span data-stu-id="ba209-171">When first visiting the `FilterByDropDownList.aspx` page the categories DropDownList's first list item (Beverages) is selected by default, showing the beverage products in the DataList.</span></span> <span data-ttu-id="ba209-172">在 *主版/詳細篩選使用 DropDownList*教學課程中我們將"--選擇類別目錄-」 的選項加入，已選取預設情況下，選取時，顯示 DropDownList*所有*的在資料庫中的產品。</span><span class="sxs-lookup"><span data-stu-id="ba209-172">In the *Master/Detail Filtering With a DropDownList* tutorial we added a "-- Choose a Category --" option to the DropDownList that was selected by default and, when selected, displayed *all* of the products in the database.</span></span> <span data-ttu-id="ba209-173">這種方式是可管理的列出一個 GridView 中的產品時，每個產品的資料列一職，少量的螢幕畫面。</span><span class="sxs-lookup"><span data-stu-id="ba209-173">Such an approach was manageable when listing the products in a GridView, as each product row took up a small amount of screen real estate.</span></span> <span data-ttu-id="ba209-174">具有 DataList，不過，每項產品的資訊會耗用較大區塊的畫面。</span><span class="sxs-lookup"><span data-stu-id="ba209-174">With the DataList, however, each product's information consumes a much larger chunk of the screen.</span></span> <span data-ttu-id="ba209-175">我們仍新增"--選擇類別目錄-」 的選項並讓它依預設選取，但而不是讓它顯示所有產品選取時，讓我們設定，讓它會顯示任何產品。</span><span class="sxs-lookup"><span data-stu-id="ba209-175">Let's still add a "-- Choose a Category --" option and have it selected by default, but instead of having it show all products when selected, let's configure it so that it shows no products.</span></span>

<span data-ttu-id="ba209-176">若要將新的清單項目新增至 DropDownList 中，移至 [屬性] 視窗，然後按一下省略符號，`Items`屬性。</span><span class="sxs-lookup"><span data-stu-id="ba209-176">To add a new list item to the DropDownList, go to the Properties window and click on the ellipses in the `Items` property.</span></span> <span data-ttu-id="ba209-177">加入新的清單項目具有`Text`"--選擇類別目錄-"和`Value` `0`。</span><span class="sxs-lookup"><span data-stu-id="ba209-177">Add a new list item with the `Text` "-- Choose a Category --" and the `Value` `0`.</span></span>


![新增](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image27.png)

<span data-ttu-id="ba209-179">**圖 11**： 新增"--選擇類別目錄-」 的清單項目</span><span class="sxs-lookup"><span data-stu-id="ba209-179">**Figure 11**: Add a "-- Choose a Category --" List Item</span></span>


<span data-ttu-id="ba209-180">或者，您也可以藉由將下列標記新增至 DropDownList 新增清單項目：</span><span class="sxs-lookup"><span data-stu-id="ba209-180">Alternatively, you can add the list item by adding the following markup to the DropDownList:</span></span>

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-vb/samples/sample3.aspx)]

<span data-ttu-id="ba209-181">此外，我們需要設定 DropDownList 控制項`AppendDataBoundItems`要`true`因為如果設定為`false`（預設值），當類別繫結至 DropDownList 從 ObjectDataSource 它們將會覆寫任何手動加入的清單項目。</span><span class="sxs-lookup"><span data-stu-id="ba209-181">Additionally, we need to set the DropDownList control's `AppendDataBoundItems` to `true` because if it's set to `false` (the default), when the categories are bound to the DropDownList from the ObjectDataSource they'll overwrite any manually-added list items.</span></span>


![AppendDataBoundItems 屬性設定為 True](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image28.png)

<span data-ttu-id="ba209-183">**圖 12**： 設定`AppendDataBoundItems`屬性設為 True</span><span class="sxs-lookup"><span data-stu-id="ba209-183">**Figure 12**: Set the `AppendDataBoundItems` Property to True</span></span>


<span data-ttu-id="ba209-184">我們所選擇的值的原因`0`的 「-選擇類別目錄-」 的清單項目是因為值是系統中有無類別`0`，因此沒有產品記錄時，會傳回"--選擇類別目錄-」 清單項目已選取。</span><span class="sxs-lookup"><span data-stu-id="ba209-184">The reason we chose the value `0` for the "-- Choose a Category --" list item is because there are no categories in the system with a value of `0`, hence no product records will be returned when the "-- Choose a Category --" list item is selected.</span></span> <span data-ttu-id="ba209-185">若要確認這一點，請花一點時間瀏覽透過瀏覽器頁面。</span><span class="sxs-lookup"><span data-stu-id="ba209-185">To confirm this, take a moment to visit the page through a browser.</span></span> <span data-ttu-id="ba209-186">如 圖 13 所示，一開始檢視頁面的 「-選擇類別目錄-」 清單項目已選取，並不顯示任何產品時。</span><span class="sxs-lookup"><span data-stu-id="ba209-186">As Figure 13 shows, when initially viewing the page the "-- Choose a Category --" list item is selected and no products are displayed.</span></span>


<span data-ttu-id="ba209-187">[![時](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image30.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image29.png)</span><span class="sxs-lookup"><span data-stu-id="ba209-187">[![When the](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image30.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image29.png)</span></span>

<span data-ttu-id="ba209-188">**圖 13**： 選取 [-選擇類別目錄-] 清單項目時，會顯示沒有產品 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image31.png))</span><span class="sxs-lookup"><span data-stu-id="ba209-188">**Figure 13**: When the "-- Choose a Category --" List Item is Selected, No Products are Displayed ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image31.png))</span></span>


<span data-ttu-id="ba209-189">而是會顯示如果*所有*的產品時選取 「-選擇類別目錄-」 的選項時，使用值`-1`改。</span><span class="sxs-lookup"><span data-stu-id="ba209-189">If you'd rather display *all* of the products when the "-- Choose a Category --" option is selected, use a value of `-1` instead.</span></span> <span data-ttu-id="ba209-190">精明的讀者應該還記得該回溯*主版/詳細篩選使用 DropDownList*我們已更新的教學課程`ProductsBLL`類別的`GetProductsByCategoryID(categoryID)`方法，讓如果*`categoryID`* 值為`-1`傳入，傳回的記錄的所有產品。</span><span class="sxs-lookup"><span data-stu-id="ba209-190">The astute reader will recall that back in the *Master/Detail Filtering With a DropDownList* tutorial we updated the `ProductsBLL` class's `GetProductsByCategoryID(categoryID)` method so that if a *`categoryID`* value of `-1` was passed in, all product records were returned.</span></span>

## <a name="summary"></a><span data-ttu-id="ba209-191">總結</span><span class="sxs-lookup"><span data-stu-id="ba209-191">Summary</span></span>

<span data-ttu-id="ba209-192">顯示階層關聯的資料時，通常最好來呈現使用主版/詳細報告，使用者可以從中開始研究過的資料階層的頂端，並向下鑽研詳細資料的資料。</span><span class="sxs-lookup"><span data-stu-id="ba209-192">When displaying hierarchically-related data, it often helps to present the data using master/detail reports, from which the user can start perusing the data from the top of the hierarchy and drill down into details.</span></span> <span data-ttu-id="ba209-193">在本教學課程中，我們檢查建置簡單的主要/詳細資料報表，顯示所選取之目錄的產品。</span><span class="sxs-lookup"><span data-stu-id="ba209-193">In this tutorial we examined building a simple master/detail report showing a selected category's products.</span></span> <span data-ttu-id="ba209-194">這被透過使用 dropdownlist 進行分類和產品屬於所選分類的 DataList 的清單。</span><span class="sxs-lookup"><span data-stu-id="ba209-194">This was accomplished by using a DropDownList for the list of categories and a DataList for the products belonging to the selected category.</span></span>

<span data-ttu-id="ba209-195">在下一個教學課程中，我們將探討跨兩個頁面分隔的主要和詳細資料記錄。</span><span class="sxs-lookup"><span data-stu-id="ba209-195">In the next tutorial we'll look at separating the master and details records across two pages.</span></span> <span data-ttu-id="ba209-196">在第一個頁面中，「 主要 」 的記錄清單隨即出現，以檢視詳細資料的連結。</span><span class="sxs-lookup"><span data-stu-id="ba209-196">In the first page, a list of "master" records will be displayed, with a link to view the details.</span></span> <span data-ttu-id="ba209-197">按一下連結，將 whisk 使用者第二個頁面上，將會顯示所選的主要記錄的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="ba209-197">Clicking on the link will whisk the user to the second page, which will display the details for the selected master record.</span></span>

<span data-ttu-id="ba209-198">快樂地寫程式 ！</span><span class="sxs-lookup"><span data-stu-id="ba209-198">Happy Programming!</span></span>

## <a name="about-the-author"></a><span data-ttu-id="ba209-199">關於作者</span><span class="sxs-lookup"><span data-stu-id="ba209-199">About the Author</span></span>

<span data-ttu-id="ba209-200">[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。</span><span class="sxs-lookup"><span data-stu-id="ba209-200">[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), author of seven ASP/ASP.NET books and founder of [4GuysFromRolla.com](http://www.4guysfromrolla.com), has been working with Microsoft Web technologies since 1998.</span></span> <span data-ttu-id="ba209-201">Scott 會擔任獨立的顧問、 培訓講師和作家。</span><span class="sxs-lookup"><span data-stu-id="ba209-201">Scott works as an independent consultant, trainer, and writer.</span></span> <span data-ttu-id="ba209-202">他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。</span><span class="sxs-lookup"><span data-stu-id="ba209-202">His latest book is [*Sams Teach Yourself ASP.NET 2.0 in 24 Hours*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco).</span></span> <span data-ttu-id="ba209-203">他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)</span><span class="sxs-lookup"><span data-stu-id="ba209-203">He can be reached at [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)</span></span> <span data-ttu-id="ba209-204">或透過他的部落格，這位於 [http://ScottOnWriting.NET](http://ScottOnWriting.NET)。</span><span class="sxs-lookup"><span data-stu-id="ba209-204">or via his blog, which can be found at [http://ScottOnWriting.NET](http://ScottOnWriting.NET).</span></span>

## <a name="special-thanks-to"></a><span data-ttu-id="ba209-205">特別感謝...</span><span class="sxs-lookup"><span data-stu-id="ba209-205">Special Thanks To…</span></span>

<span data-ttu-id="ba209-206">本教學課程系列是由許多實用的檢閱者檢閱。</span><span class="sxs-lookup"><span data-stu-id="ba209-206">This tutorial series was reviewed by many helpful reviewers.</span></span> <span data-ttu-id="ba209-207">本教學課程中的潛在客戶檢閱者已 Randy Schmidt。</span><span class="sxs-lookup"><span data-stu-id="ba209-207">Lead reviewer for this tutorial was Randy Schmidt.</span></span> <span data-ttu-id="ba209-208">有興趣檢閱我即將推出的 MSDN 文章嗎？</span><span class="sxs-lookup"><span data-stu-id="ba209-208">Interested in reviewing my upcoming MSDN articles?</span></span> <span data-ttu-id="ba209-209">如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)</span><span class="sxs-lookup"><span data-stu-id="ba209-209">If so, drop me a line at [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ba209-210">[上一頁](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md)
> [下一頁](master-detail-filtering-acess-two-pages-datalist-vb.md)</span><span class="sxs-lookup"><span data-stu-id="ba209-210">[Previous](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md)
[Next](master-detail-filtering-acess-two-pages-datalist-vb.md)</span></span>
