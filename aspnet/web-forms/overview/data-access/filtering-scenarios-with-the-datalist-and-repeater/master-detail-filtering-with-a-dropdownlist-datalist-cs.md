---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-cs
title: "使用 DropDownList (C#) 進行篩選的主要/詳細資料 |Microsoft 文件"
author: rick-anderson
description: "在本教學課程中，我們看到如何使用 DropDownLists 顯示 'master' 的記錄和資料清單來顯示單一網頁中顯示主要/詳細資料報表..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 07fa47ae-e491-4a2f-b265-d342b9ddef46
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: c2199f0957f4cbe1d35dd971744087da9af1abce
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="masterdetail-filtering-with-a-dropdownlist-c"></a><span data-ttu-id="639a7-103">主從式篩選搭配 DropDownList (C#)</span><span class="sxs-lookup"><span data-stu-id="639a7-103">Master/Detail Filtering With a DropDownList (C#)</span></span>
====================
<span data-ttu-id="639a7-104">由[Scott Mitchell](https://twitter.com/ScottOnWriting)</span><span class="sxs-lookup"><span data-stu-id="639a7-104">by [Scott Mitchell](https://twitter.com/ScottOnWriting)</span></span>

<span data-ttu-id="639a7-105">[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_33_CS.exe)或[下載 PDF](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/datatutorial33cs1.pdf)</span><span class="sxs-lookup"><span data-stu-id="639a7-105">[Download Sample App](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_33_CS.exe) or [Download PDF](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/datatutorial33cs1.pdf)</span></span>

> <span data-ttu-id="639a7-106">本教學課程中我們會了解如何在單一網頁使用 DropDownLists 顯示 「 主要 」 的記錄和資料以顯示 [詳細資料] 清單中顯示主要/詳細資料報表。</span><span class="sxs-lookup"><span data-stu-id="639a7-106">In this tutorial we see how to display master/detail reports in a single web page using DropDownLists to display the "master" records and a DataList to display the "details".</span></span>


## <a name="introduction"></a><span data-ttu-id="639a7-107">簡介</span><span class="sxs-lookup"><span data-stu-id="639a7-107">Introduction</span></span>

<span data-ttu-id="639a7-108">主要/詳細資料報表，我們先建立在先前使用 GridView[主要/詳細資料篩選與 DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md)教學課程中，一開始會顯示某些的 「 主要 」 的記錄集。</span><span class="sxs-lookup"><span data-stu-id="639a7-108">The master/detail report, which we first created using a GridView in the earlier [Master/Detail Filtering With a DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) tutorial, begins by showing some set of "master" records.</span></span> <span data-ttu-id="639a7-109">使用者可以再向下鑽研到其中一個主要記錄中，藉此檢視主要記錄的 「 詳細資料。 」</span><span class="sxs-lookup"><span data-stu-id="639a7-109">The user can then drill down into one of the master records, thereby viewing that master record's "details."</span></span> <span data-ttu-id="639a7-110">主要/詳細資料報表是視覺化一對多關聯性並顯示 （是有很多資料行） 特別 「 寬 」 的資料表的詳細的資訊的理想選擇。</span><span class="sxs-lookup"><span data-stu-id="639a7-110">Master/detail reports are an ideal choice for visualizing one-to-many relationships and for displaying detailed information from particularly "wide" tables (ones that have a lot of columns).</span></span> <span data-ttu-id="639a7-111">我們已探索如何實作在上一個教學課程中使用的 GridView 和 DetailsView 控制項的主要/詳細資料報表。</span><span class="sxs-lookup"><span data-stu-id="639a7-111">We've explored how to implement master/detail reports using the GridView and DetailsView controls in previous tutorials.</span></span> <span data-ttu-id="639a7-112">在本教學課程和兩個 下一步，我們會重新檢查這些概念，但是著重於使用 DataList 和中繼器控制項改為。</span><span class="sxs-lookup"><span data-stu-id="639a7-112">In this tutorial and the next two, we'll reexamine these concepts, but focus on using DataList and Repeater controls instead.</span></span>

<span data-ttu-id="639a7-113">在本教學課程中，我們將探討使用 DropDownList 包含的 「 主要 」 記錄，[詳細資料] 中顯示的記錄資料清單。</span><span class="sxs-lookup"><span data-stu-id="639a7-113">In this tutorial, we'll look at using a DropDownList to contain the "master" records, with the "details" records displayed in a DataList.</span></span>

## <a name="step-1-adding-the-masterdetail-tutorial-web-pages"></a><span data-ttu-id="639a7-114">步驟 1： 加入主從式教學課程 Web 網頁</span><span class="sxs-lookup"><span data-stu-id="639a7-114">Step 1: Adding the Master/Detail Tutorial Web Pages</span></span>

<span data-ttu-id="639a7-115">開始本教學課程之前，讓我們先花一點時間，將資料夾加入我們需要本教學課程和下一步使用 DataList 和中繼器控制項的主要/詳細資料報表處理的兩個 ASP.NET 網頁。</span><span class="sxs-lookup"><span data-stu-id="639a7-115">Before we start this tutorial, let's first take a moment to add the folder and ASP.NET pages we'll need for this tutorial and the next two dealing with master/detail reports using the DataList and Repeater controls.</span></span> <span data-ttu-id="639a7-116">藉由建立新的資料夾中名為專案啟動`DataListRepeaterFiltering`。</span><span class="sxs-lookup"><span data-stu-id="639a7-116">Start by creating a new folder in the project named `DataListRepeaterFiltering`.</span></span> <span data-ttu-id="639a7-117">接下來，在此資料夾時，需要全部都設定為使用主版頁面中加入下列五個 ASP.NET 網頁`Site.master`:</span><span class="sxs-lookup"><span data-stu-id="639a7-117">Next, add the following five ASP.NET pages to this folder, having all of them configured to use the master page `Site.master`:</span></span>

- `Default.aspx`
- `FilterByDropDownList.aspx`
- `CategoryListMaster.aspx`
- `ProductsForCategoryDetails.aspx`
- `CategoriesAndProducts.aspx`


![建立 DataListRepeaterFiltering 資料夾，以及新增的教學課程的 ASP.NET 頁面](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image1.png)

<span data-ttu-id="639a7-119">**圖 1**： 建立`DataListRepeaterFiltering`資料夾並加入的教學課程的 ASP.NET 網頁</span><span class="sxs-lookup"><span data-stu-id="639a7-119">**Figure 1**: Create a `DataListRepeaterFiltering` Folder and Add the Tutorial ASP.NET Pages</span></span>


<span data-ttu-id="639a7-120">接下來，開啟`Default.aspx`頁面上，拖曳`SectionLevelTutorialListing.ascx`使用者控制項`UserControls`資料夾拖曳至設計介面。</span><span class="sxs-lookup"><span data-stu-id="639a7-120">Next, open the `Default.aspx` page and drag the `SectionLevelTutorialListing.ascx` User Control from the `UserControls` folder onto the Design surface.</span></span> <span data-ttu-id="639a7-121">此使用者控制項，我們在建立[主版頁面和站台瀏覽](../introduction/master-pages-and-site-navigation-cs.md)教學課程中，列舉站台對應，並顯示從目前的區段項目符號清單中的教學課程。</span><span class="sxs-lookup"><span data-stu-id="639a7-121">This User Control, which we created in the [Master Pages and Site Navigation](../introduction/master-pages-and-site-navigation-cs.md) tutorial, enumerates the site map and displays the tutorials from the current section in a bulleted list.</span></span>


<span data-ttu-id="639a7-122">[![將 SectionLevelTutorialListing.ascx 使用者控制項加入至 Default.aspx](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image3.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="639a7-122">[![Add the SectionLevelTutorialListing.ascx User Control to Default.aspx](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image3.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image2.png)</span></span>

<span data-ttu-id="639a7-123">**圖 2**： 新增`SectionLevelTutorialListing.ascx`使用者控制項加入`Default.aspx`([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="639a7-123">**Figure 2**: Add the `SectionLevelTutorialListing.ascx` User Control to `Default.aspx` ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image4.png))</span></span>


<span data-ttu-id="639a7-124">若要有項目符號清單顯示主從式教學課程，我們將建立，我們要將它們新增至站台對應。</span><span class="sxs-lookup"><span data-stu-id="639a7-124">In order to have the bulleted list display the master/detail tutorials we'll be creating, we need to add them to the site map.</span></span> <span data-ttu-id="639a7-125">開啟`Web.sitemap`檔案，然後加入下列標記 」 顯示的資料與 DataList 和中繼器 「 站台對應節點標記之後：</span><span class="sxs-lookup"><span data-stu-id="639a7-125">Open the `Web.sitemap` file and add the following markup after the "Displaying Data with the DataList and Repeater" site map node markup:</span></span>

[!code-xml[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample1.xml)]


![更新站台對應，以包含新的 ASP.NET 網頁](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image5.png)

<span data-ttu-id="639a7-127">**圖 3**： 更新包含新的 ASP.NET 網頁站台對應</span><span class="sxs-lookup"><span data-stu-id="639a7-127">**Figure 3**: Update the Site Map to Include the New ASP.NET Pages</span></span>


## <a name="step-2-displaying-the-categories-in-a-dropdownlist"></a><span data-ttu-id="639a7-128">步驟 2: DropDownList 以顯示類別目錄</span><span class="sxs-lookup"><span data-stu-id="639a7-128">Step 2: Displaying the Categories in a DropDownList</span></span>

<span data-ttu-id="639a7-129">我們的主要/詳細資料報表會列出 DropDownList 中的類別，與選取的清單項目的產品顯示進一步的頁面中的資料清單。</span><span class="sxs-lookup"><span data-stu-id="639a7-129">Our master/detail report will list the categories in a DropDownList, with the selected list item's products displayed further down in the page in a DataList.</span></span> <span data-ttu-id="639a7-130">晚於我們在第一個工作，則具有 dropdownlist 顯示類別目錄。</span><span class="sxs-lookup"><span data-stu-id="639a7-130">The first task ahead of us, then, is to have the categories displayed in a DropDownList.</span></span> <span data-ttu-id="639a7-131">先開啟`FilterByDropDownList.aspx`頁面`DataListRepeaterFiltering`資料夾，然後從 [工具箱] 拖曳至頁面的設計工具拖曳 DropDownList。</span><span class="sxs-lookup"><span data-stu-id="639a7-131">Start by opening the `FilterByDropDownList.aspx` page in the `DataListRepeaterFiltering` folder and drag a DropDownList from the Toolbox onto the page's designer.</span></span> <span data-ttu-id="639a7-132">接下來，設定 DropDownList`ID`屬性`Categories`。</span><span class="sxs-lookup"><span data-stu-id="639a7-132">Next, set the DropDownList's `ID` property to `Categories`.</span></span> <span data-ttu-id="639a7-133">按一下 從 DropDownList 的智慧標籤的 選擇資料來源 連結，建立名為新 ObjectDataSource `CategoriesDataSource`。</span><span class="sxs-lookup"><span data-stu-id="639a7-133">Click on the Choose Data Source link from the DropDownList's smart tag and create a new ObjectDataSource named `CategoriesDataSource`.</span></span>


<span data-ttu-id="639a7-134">[![加入名為 CategoriesDataSource 新 ObjectDataSource](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image7.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="639a7-134">[![Add a New ObjectDataSource Named CategoriesDataSource](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image7.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image6.png)</span></span>

<span data-ttu-id="639a7-135">**圖 4**： 加入新的 ObjectDataSource 名為`CategoriesDataSource`([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="639a7-135">**Figure 4**: Add a New ObjectDataSource Named `CategoriesDataSource` ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image8.png))</span></span>


<span data-ttu-id="639a7-136">設定新 ObjectDataSource，它會叫用`CategoriesBLL`類別的`GetCategories()`方法。</span><span class="sxs-lookup"><span data-stu-id="639a7-136">Configure the new ObjectDataSource such that it invokes the `CategoriesBLL` class's `GetCategories()` method.</span></span> <span data-ttu-id="639a7-137">設定我們仍需要指定哪些資料來源欄位應該會顯示在 DropDownList 和 ObjectDataSource 之後其中一個應該為每個清單項目的值相關聯。</span><span class="sxs-lookup"><span data-stu-id="639a7-137">After configuring the ObjectDataSource we still need to specify what data source field should be displayed in the DropDownList and which one should be associated as the value for each list item.</span></span> <span data-ttu-id="639a7-138">具有`CategoryName`欄位顯示器和`CategoryID`做為每個清單項目值。</span><span class="sxs-lookup"><span data-stu-id="639a7-138">Have the `CategoryName` field as the display and `CategoryID` as the value for each list item.</span></span>


<span data-ttu-id="639a7-139">[![此類別名稱欄位，然後使用 CategoryID 有 DropDownList 顯示做為值](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image10.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="639a7-139">[![Have the DropDownList Display the CategoryName Field and Use CategoryID as the Value](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image10.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image9.png)</span></span>

<span data-ttu-id="639a7-140">**圖 5**: DropDownList 顯示`CategoryName`欄位並使用`CategoryID`做為值 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image11.png))</span><span class="sxs-lookup"><span data-stu-id="639a7-140">**Figure 5**: Have the DropDownList Display the `CategoryName` Field and Use `CategoryID` as the Value ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image11.png))</span></span>


<span data-ttu-id="639a7-141">現在我們有從記錄中會填入的 DropDownList 控制項`Categories`資料表 （全部在六秒內完成）。</span><span class="sxs-lookup"><span data-stu-id="639a7-141">At this point we have a DropDownList control that's populated with the records from the `Categories` table (all accomplished in about six seconds).</span></span> <span data-ttu-id="639a7-142">圖 6 為止顯示進度，透過瀏覽器檢視時。</span><span class="sxs-lookup"><span data-stu-id="639a7-142">Figure 6 shows our progress thus far when viewed through a browser.</span></span>


<span data-ttu-id="639a7-143">[![下拉式清單會列出目前的分類](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image13.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="639a7-143">[![A Drop-Down Lists the Current Categories](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image13.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image12.png)</span></span>

<span data-ttu-id="639a7-144">**圖 6**: 下拉式清單會列出目前的分類 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="639a7-144">**Figure 6**: A Drop-Down Lists the Current Categories ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image14.png))</span></span>


## <a name="step-2-adding-the-products-datalist"></a><span data-ttu-id="639a7-145">步驟 2： 加入產品 DataList</span><span class="sxs-lookup"><span data-stu-id="639a7-145">Step 2: Adding the Products DataList</span></span>

<span data-ttu-id="639a7-146">我們的主要/詳細資料報表的最後一個步驟是列出與選取的類別相關聯的產品。</span><span class="sxs-lookup"><span data-stu-id="639a7-146">The last step in our master/detail report is to list the products associated with the selected category.</span></span> <span data-ttu-id="639a7-147">若要達成此目的，DataList 加入頁面並建立名為新 ObjectDataSource `ProductsByCategoryDataSource`。</span><span class="sxs-lookup"><span data-stu-id="639a7-147">To accomplish this, add a DataList to the page and create a new ObjectDataSource named `ProductsByCategoryDataSource`.</span></span> <span data-ttu-id="639a7-148">具有`ProductsByCategoryDataSource`控制項擷取資料的來源`ProductsBLL`類別的`GetProductsByCategoryID(categoryID)`方法。</span><span class="sxs-lookup"><span data-stu-id="639a7-148">Have the `ProductsByCategoryDataSource` control retrieve its data from the `ProductsBLL` class's `GetProductsByCategoryID(categoryID)` method.</span></span> <span data-ttu-id="639a7-149">由於此主要/詳細資料報表是唯讀的選擇 （無） 選項在 INSERT、 UPDATE 和 DELETE 的索引標籤中。</span><span class="sxs-lookup"><span data-stu-id="639a7-149">Since this master/detail report is read-only, choose the (None) option in the INSERT, UPDATE, and DELETE tabs.</span></span>


<span data-ttu-id="639a7-150">[![選取 GetProductsByCategoryID(categoryID) 方法](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image16.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="639a7-150">[![Select the GetProductsByCategoryID(categoryID) Method](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image16.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image15.png)</span></span>

<span data-ttu-id="639a7-151">**圖 7**： 選取`GetProductsByCategoryID(categoryID)`方法 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image17.png))</span><span class="sxs-lookup"><span data-stu-id="639a7-151">**Figure 7**: Select the `GetProductsByCategoryID(categoryID)` Method ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image17.png))</span></span>


<span data-ttu-id="639a7-152">按一下 下一步之後，ObjectDataSource 精靈會提示我們之值的來源`GetProductsByCategoryID(categoryID)`方法的 *`categoryID`* 參數。</span><span class="sxs-lookup"><span data-stu-id="639a7-152">After clicking Next, the ObjectDataSource wizard prompts us for the source of the value for the `GetProductsByCategoryID(categoryID)` method's *`categoryID`* parameter.</span></span> <span data-ttu-id="639a7-153">若要使用的所選值`categories`DropDownList 項目設定參數來源控制與以 ControlID `Categories`。</span><span class="sxs-lookup"><span data-stu-id="639a7-153">To use the value of the selected `categories` DropDownList item set the Parameter source to Control and the ControlID to `Categories`.</span></span>


<span data-ttu-id="639a7-154">[![CategoryID 參數值設定為類別 DropDownList](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image19.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image18.png)</span><span class="sxs-lookup"><span data-stu-id="639a7-154">[![Set the categoryID Parameter to the Value of the Categories DropDownList](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image19.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image18.png)</span></span>

<span data-ttu-id="639a7-155">**圖 8**： 設定 *`categoryID`* 參數的值`Categories`DropDownList ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image20.png))</span><span class="sxs-lookup"><span data-stu-id="639a7-155">**Figure 8**: Set the *`categoryID`* Parameter to the Value of the `Categories` DropDownList ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image20.png))</span></span>


<span data-ttu-id="639a7-156">完成設定資料來源精靈，Visual Studio 會自動產生`ItemTemplate`如 DataList 顯示的名稱和每個資料欄位的值。</span><span class="sxs-lookup"><span data-stu-id="639a7-156">Upon completing the Configure Data Source wizard, Visual Studio will automatically generate an `ItemTemplate` for the DataList that displays the name and value of each data field.</span></span> <span data-ttu-id="639a7-157">讓我們來增強改用 DataList`ItemTemplate`顯示只要產品的名稱、 類別、 供應商，每個單位和價格以及數量`SeparatorTemplate`，插入`<hr>`每個項目之間的項目。</span><span class="sxs-lookup"><span data-stu-id="639a7-157">Let's enhance the DataList to instead use an `ItemTemplate` that displays just the product's name, category, supplier, quantity per unit, and price along with a `SeparatorTemplate` that injects an `<hr>` element between each item.</span></span> <span data-ttu-id="639a7-158">我要使用`ItemTemplate`範例從[顯示的資料，以在 DataList 和中繼器控制項](../displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs.md)教學課程中，但可自由使用任何範本標記您找到最視覺上吸引人。</span><span class="sxs-lookup"><span data-stu-id="639a7-158">I'm going to use the `ItemTemplate` from an example in the [Displaying Data with the DataList and Repeater Controls](../displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs.md) tutorial, but feel free to use whatever template markup you find most visually appealing.</span></span>

<span data-ttu-id="639a7-159">進行這些變更之後，DataList 和其 ObjectDataSource 標記看起來應該如下所示：</span><span class="sxs-lookup"><span data-stu-id="639a7-159">After making these changes, your DataList and its ObjectDataSource's markup should look similar to the following:</span></span>

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample2.aspx)]

<span data-ttu-id="639a7-160">請花一點時間簽出的瀏覽器中的進度。</span><span class="sxs-lookup"><span data-stu-id="639a7-160">Take a moment to check out our progress in a browser.</span></span> <span data-ttu-id="639a7-161">當第一次造訪的頁面時，屬於所選類別目錄 （飲料） 這些產品會顯示 （如圖 9 所示），但變更 DropDownList 不會更新資料。</span><span class="sxs-lookup"><span data-stu-id="639a7-161">When first visiting the page, those products belonging to the selected category (Beverages) are displayed (as shown in Figure 9), but changing the DropDownList doesn't update the data.</span></span> <span data-ttu-id="639a7-162">這是因為更新在 DataList 一定會發生回傳。</span><span class="sxs-lookup"><span data-stu-id="639a7-162">This is because a postback must occur for the DataList to update.</span></span> <span data-ttu-id="639a7-163">若要完成此我們可以設定 DropDownList`AutoPostBack`屬性`true`或按鈕 Web 控制項加入至網頁。</span><span class="sxs-lookup"><span data-stu-id="639a7-163">To accomplish this we can either set the DropDownList's `AutoPostBack` property to `true` or add a Button Web control to the page.</span></span> <span data-ttu-id="639a7-164">此教學課程中，我選擇設定 DropDownList`AutoPostBack`屬性`true`。</span><span class="sxs-lookup"><span data-stu-id="639a7-164">For this tutorial, I've opted to set the DropDownList's `AutoPostBack` property to `true`.</span></span>

<span data-ttu-id="639a7-165">數字 9 和 10 說明主要/詳細資料中的報表動作。</span><span class="sxs-lookup"><span data-stu-id="639a7-165">Figures 9 and 10 illustrate the master/detail report in action.</span></span>


<span data-ttu-id="639a7-166">[![當第一次瀏覽頁面，會顯示飲料產品](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image22.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image21.png)</span><span class="sxs-lookup"><span data-stu-id="639a7-166">[![When First Visiting the Page, the Beverage Products are Displayed](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image22.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image21.png)</span></span>

<span data-ttu-id="639a7-167">**圖 9**： 當第一次瀏覽頁面，會顯示飲料產品 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image23.png))</span><span class="sxs-lookup"><span data-stu-id="639a7-167">**Figure 9**: When First Visiting the Page, the Beverage Products are Displayed ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image23.png))</span></span>


<span data-ttu-id="639a7-168">[![選取新的產品 （產生） 會自動導致回傳，更新資料清單](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image25.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image24.png)</span><span class="sxs-lookup"><span data-stu-id="639a7-168">[![Selecting a New Product (Produce) Automatically Causes a PostBack, Updating the DataList](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image25.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image24.png)</span></span>

<span data-ttu-id="639a7-169">**圖 10**： 選取新的產品 （產生） 會自動導致回傳，更新在 DataList ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image26.png))</span><span class="sxs-lookup"><span data-stu-id="639a7-169">**Figure 10**: Selecting a New Product (Produce) Automatically Causes a PostBack, Updating the DataList ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image26.png))</span></span>


## <a name="adding-a----choose-a-category----list-item"></a><span data-ttu-id="639a7-170">可以加入 「-選擇分類-」 清單項目</span><span class="sxs-lookup"><span data-stu-id="639a7-170">Adding a "-- Choose a Category --" List Item</span></span>

<span data-ttu-id="639a7-171">當第一次造訪`FilterByDropDownList.aspx`頁面 DropDownList 的第一個清單項目 （飲料） 根據預設，顯示飲料產品資料清單中選取的類別。</span><span class="sxs-lookup"><span data-stu-id="639a7-171">When first visiting the `FilterByDropDownList.aspx` page the categories DropDownList's first list item (Beverages) is selected by default, showing the beverage products in the DataList.</span></span> <span data-ttu-id="639a7-172">在*主要/詳細資料篩選與 DropDownList*教學課程中我們加入 「-選擇分類-」 的選項，已預設選取，如果選取，顯示 DropDownList*所有*的在資料庫中的產品。</span><span class="sxs-lookup"><span data-stu-id="639a7-172">In the *Master/Detail Filtering With a DropDownList* tutorial we added a "-- Choose a Category --" option to the DropDownList that was selected by default and, when selected, displayed *all* of the products in the database.</span></span> <span data-ttu-id="639a7-173">這種方法時可管理以列出產品 GridView，為每個產品的資料列花費少量的實際螢幕面積組成。</span><span class="sxs-lookup"><span data-stu-id="639a7-173">Such an approach was manageable when listing the products in a GridView, as each product row took up a small amount of screen real estate.</span></span> <span data-ttu-id="639a7-174">不過，DataList，與每個產品資訊耗用較大區塊的螢幕。</span><span class="sxs-lookup"><span data-stu-id="639a7-174">With the DataList, however, each product's information consumes a much larger chunk of the screen.</span></span> <span data-ttu-id="639a7-175">我們仍加入 「-選擇分類-」 選項，依預設選取，但而不是讓它顯示所有產品選取時，我們將它設定為讓它會顯示任何產品。</span><span class="sxs-lookup"><span data-stu-id="639a7-175">Let's still add a "-- Choose a Category --" option and have it selected by default, but instead of having it show all products when selected, let's configure it so that it shows no products.</span></span>

<span data-ttu-id="639a7-176">DropDownList 中加入新的清單項目，請前往 [屬性] 視窗，按一下省略符號在`Items`屬性。</span><span class="sxs-lookup"><span data-stu-id="639a7-176">To add a new list item to the DropDownList, go to the Properties window and click on the ellipses in the `Items` property.</span></span> <span data-ttu-id="639a7-177">加入新的清單項目與`Text`"-選擇一個類別目錄-"和`Value` `0`。</span><span class="sxs-lookup"><span data-stu-id="639a7-177">Add a new list item with the `Text` "-- Choose a Category --" and the `Value` `0`.</span></span>


![新增](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image27.png)

<span data-ttu-id="639a7-179">**圖 11**： 加入 「-選擇分類-」 清單項目</span><span class="sxs-lookup"><span data-stu-id="639a7-179">**Figure 11**: Add a "-- Choose a Category --" List Item</span></span>


<span data-ttu-id="639a7-180">或者，您可以將下列標記加入至 DropDownList 新增清單項目：</span><span class="sxs-lookup"><span data-stu-id="639a7-180">Alternatively, you can add the list item by adding the following markup to the DropDownList:</span></span>

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample3.aspx)]

<span data-ttu-id="639a7-181">此外，我們需要將 DropDownList 控制項的`AppendDataBoundItems`至`true`因為如果設定為`false`（預設值），分類繫結至 DropDownList 從 ObjectDataSource 時，它們會覆寫任何手動加入清單項目。</span><span class="sxs-lookup"><span data-stu-id="639a7-181">Additionally, we need to set the DropDownList control's `AppendDataBoundItems` to `true` because if it's set to `false` (the default), when the categories are bound to the DropDownList from the ObjectDataSource they'll overwrite any manually-added list items.</span></span>


![AppendDataBoundItems 屬性設為 True](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image28.png)

<span data-ttu-id="639a7-183">**圖 12**： 設定`AppendDataBoundItems`屬性設定為 True</span><span class="sxs-lookup"><span data-stu-id="639a7-183">**Figure 12**: Set the `AppendDataBoundItems` Property to True</span></span>


<span data-ttu-id="639a7-184">因此我們選擇值`0`「-選擇分類-」 清單項目是因為值是系統中有無類別`0`，因此沒有產品記錄時，會傳回 「-選擇分類-」 清單項目已選取。</span><span class="sxs-lookup"><span data-stu-id="639a7-184">The reason we chose the value `0` for the "-- Choose a Category --" list item is because there are no categories in the system with a value of `0`, hence no product records will be returned when the "-- Choose a Category --" list item is selected.</span></span> <span data-ttu-id="639a7-185">若要確認這一點，請花一點時間瀏覽的頁面，透過瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="639a7-185">To confirm this, take a moment to visit the page through a browser.</span></span> <span data-ttu-id="639a7-186">如圖 13 所示，一開始檢視頁面 「-選擇分類-」 清單中選取項目，並不顯示任何產品時。</span><span class="sxs-lookup"><span data-stu-id="639a7-186">As Figure 13 shows, when initially viewing the page the "-- Choose a Category --" list item is selected and no products are displayed.</span></span>


<span data-ttu-id="639a7-187">[![當](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image30.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image29.png)</span><span class="sxs-lookup"><span data-stu-id="639a7-187">[![When the](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image30.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image29.png)</span></span>

<span data-ttu-id="639a7-188">**圖 13**： 選取 「-選擇分類-」 清單項目時，會顯示沒有產品 ([按一下以檢視完整大小的影像](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image31.png))</span><span class="sxs-lookup"><span data-stu-id="639a7-188">**Figure 13**: When the "-- Choose a Category --" List Item is Selected, No Products are Displayed ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image31.png))</span></span>


<span data-ttu-id="639a7-189">而是會顯示如果*所有*產品的 「-選擇分類-」 選取選項時，使用值`-1`改為。</span><span class="sxs-lookup"><span data-stu-id="639a7-189">If you'd rather display *all* of the products when the "-- Choose a Category --" option is selected, use a value of `-1` instead.</span></span> <span data-ttu-id="639a7-190">精明讀取器將會回收該回*主要/詳細資料篩選與 DropDownList*我們已更新的教學課程`ProductsBLL`類別的`GetProductsByCategoryID(categoryID)`方法以便如果 *`categoryID`* 值`-1`未傳回記錄的所有產品中傳遞。</span><span class="sxs-lookup"><span data-stu-id="639a7-190">The astute reader will recall that back in the *Master/Detail Filtering With a DropDownList* tutorial we updated the `ProductsBLL` class's `GetProductsByCategoryID(categoryID)` method so that if a *`categoryID`* value of `-1` was passed in, all product records were returned.</span></span>

## <a name="summary"></a><span data-ttu-id="639a7-191">總結</span><span class="sxs-lookup"><span data-stu-id="639a7-191">Summary</span></span>

<span data-ttu-id="639a7-192">顯示階層關聯的資料時，這通常有助於呈現的資料，使用主要/詳細資料報表，使用者可以從中啟動研究的資料階層的頂端，並向下鑽研到詳細資料。</span><span class="sxs-lookup"><span data-stu-id="639a7-192">When displaying hierarchically-related data, it often helps to present the data using master/detail reports, from which the user can start perusing the data from the top of the hierarchy and drill down into details.</span></span> <span data-ttu-id="639a7-193">在此教學課程中我們會檢查建置簡單的主要/詳細資料報表，顯示所選類別目錄的產品。</span><span class="sxs-lookup"><span data-stu-id="639a7-193">In this tutorial we examined building a simple master/detail report showing a selected category's products.</span></span> <span data-ttu-id="639a7-194">這是使用 DropDownList DataList 屬於所選類別目錄的產品類別目錄清單以完成。</span><span class="sxs-lookup"><span data-stu-id="639a7-194">This was accomplished by using a DropDownList for the list of categories and a DataList for the products belonging to the selected category.</span></span>

<span data-ttu-id="639a7-195">在下一個教學課程中，我們將探討跨兩個的頁面分隔 master 和詳細資料記錄。</span><span class="sxs-lookup"><span data-stu-id="639a7-195">In the next tutorial we'll look at separating the master and details records across two pages.</span></span> <span data-ttu-id="639a7-196">在第一個頁面中，「 主要 」 記錄的清單隨即出現，以檢視詳細資料的連結。</span><span class="sxs-lookup"><span data-stu-id="639a7-196">In the first page, a list of "master" records will be displayed, with a link to view the details.</span></span> <span data-ttu-id="639a7-197">按一下連結，將 whisk 使用者第二個頁面上，將會顯示選取的主要記錄的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="639a7-197">Clicking on the link will whisk the user to the second page, which will display the details for the selected master record.</span></span>

<span data-ttu-id="639a7-198">祝您程式設計 ！</span><span class="sxs-lookup"><span data-stu-id="639a7-198">Happy Programming!</span></span>

## <a name="about-the-author"></a><span data-ttu-id="639a7-199">關於作者</span><span class="sxs-lookup"><span data-stu-id="639a7-199">About the Author</span></span>

<span data-ttu-id="639a7-200">[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。</span><span class="sxs-lookup"><span data-stu-id="639a7-200">[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), author of seven ASP/ASP.NET books and founder of [4GuysFromRolla.com](http://www.4guysfromrolla.com), has been working with Microsoft Web technologies since 1998.</span></span> <span data-ttu-id="639a7-201">Scott 可做為獨立顧問、 訓練和寫入器。</span><span class="sxs-lookup"><span data-stu-id="639a7-201">Scott works as an independent consultant, trainer, and writer.</span></span> <span data-ttu-id="639a7-202">他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。</span><span class="sxs-lookup"><span data-stu-id="639a7-202">His latest book is [*Sams Teach Yourself ASP.NET 2.0 in 24 Hours*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco).</span></span> <span data-ttu-id="639a7-203">他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。</span><span class="sxs-lookup"><span data-stu-id="639a7-203">He can be reached at [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) or via his blog, which can be found at [http://ScottOnWriting.NET](http://ScottOnWriting.NET).</span></span>

## <a name="special-thanks-to"></a><span data-ttu-id="639a7-204">特別感謝...</span><span class="sxs-lookup"><span data-stu-id="639a7-204">Special Thanks To…</span></span>

<span data-ttu-id="639a7-205">許多有用的檢閱者已檢閱本教學課程系列。</span><span class="sxs-lookup"><span data-stu-id="639a7-205">This tutorial series was reviewed by many helpful reviewers.</span></span> <span data-ttu-id="639a7-206">在此教學課程的前導檢閱者已袁 Schmidt。</span><span class="sxs-lookup"><span data-stu-id="639a7-206">Lead reviewer for this tutorial was Randy Schmidt.</span></span> <span data-ttu-id="639a7-207">檢閱我即將推出的 MSDN 文件有興趣嗎？</span><span class="sxs-lookup"><span data-stu-id="639a7-207">Interested in reviewing my upcoming MSDN articles?</span></span> <span data-ttu-id="639a7-208">如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)</span><span class="sxs-lookup"><span data-stu-id="639a7-208">If so, drop me a line at [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="639a7-209">下一步</span><span class="sxs-lookup"><span data-stu-id="639a7-209">Next</span></span>](master-detail-filtering-acess-two-pages-datalist-cs.md)
