---
uid: web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-vb
title: 分頁和排序報告資料 (VB) |Microsoft Docs
author: rick-anderson
description: 分頁和排序是兩個常見的功能，在線上應用程式中顯示資料。 在本教學課程中我們將率先一睹加入排序和...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: b895e37e-0e69-45cc-a7e4-17ddd2e1b38d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 6de1f78bab795ef191553bf0d58b3b29c26d7d85
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387646"
---
<a name="paging-and-sorting-report-data-vb"></a><span data-ttu-id="32574-104">分頁和排序報告資料 (VB)</span><span class="sxs-lookup"><span data-stu-id="32574-104">Paging and Sorting Report Data (VB)</span></span>
====================
<span data-ttu-id="32574-105">藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)</span><span class="sxs-lookup"><span data-stu-id="32574-105">by [Scott Mitchell](https://twitter.com/ScottOnWriting)</span></span>

<span data-ttu-id="32574-106">[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_24_VB.exe)或[下載 PDF](paging-and-sorting-report-data-vb/_static/datatutorial24vb1.pdf)</span><span class="sxs-lookup"><span data-stu-id="32574-106">[Download Sample App](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_24_VB.exe) or [Download PDF](paging-and-sorting-report-data-vb/_static/datatutorial24vb1.pdf)</span></span>

> <span data-ttu-id="32574-107">分頁和排序是兩個常見的功能，在線上應用程式中顯示資料。</span><span class="sxs-lookup"><span data-stu-id="32574-107">Paging and sorting are two very common features when displaying data in an online application.</span></span> <span data-ttu-id="32574-108">在本教學課程中，我們將率先一睹加入排序和分頁加入我們的報告，我們將在未來的教學課程時建置。</span><span class="sxs-lookup"><span data-stu-id="32574-108">In this tutorial we'll take a first look at adding sorting and paging to our reports, which we will then build upon in future tutorials.</span></span>


## <a name="introduction"></a><span data-ttu-id="32574-109">簡介</span><span class="sxs-lookup"><span data-stu-id="32574-109">Introduction</span></span>

<span data-ttu-id="32574-110">分頁和排序是兩個常見的功能，在線上應用程式中顯示資料。</span><span class="sxs-lookup"><span data-stu-id="32574-110">Paging and sorting are two very common features when displaying data in an online application.</span></span> <span data-ttu-id="32574-111">比方說，搜尋時在線上書店的 ASP.NET 書籍，可能有數百個這類的書籍，但報告，列出搜尋結果會列出每頁只十個相符項目。</span><span class="sxs-lookup"><span data-stu-id="32574-111">For example, when searching for ASP.NET books at an online bookstore, there may be hundreds of such books, but the report listing the search results lists only ten matches per page.</span></span> <span data-ttu-id="32574-112">此外，結果可以依照標題、 價格、 頁數、 作者名稱等等。</span><span class="sxs-lookup"><span data-stu-id="32574-112">Moreover, the results can be sorted by title, price, page count, author name, and so on.</span></span> <span data-ttu-id="32574-113">雖然的過去 23 的教學課程檢驗了如何建置各種不同的報表，包括介面，允許新增、 編輯和刪除資料，我們不討論過如何排序資料，而且只發生分頁範例我們 ve 看到已與 DetailsView 和 FormView控制項。</span><span class="sxs-lookup"><span data-stu-id="32574-113">While the past 23 tutorials have examined how to build a variety of reports, including interfaces that permit adding, editing, and deleting data, we ve not looked at how to sort data and the only paging examples we ve seen have been with the DetailsView and FormView controls.</span></span>

<span data-ttu-id="32574-114">在本教學課程中，我們會看到如何將排序和分頁加入我們的報告，可透過只會檢查幾個核取方塊。</span><span class="sxs-lookup"><span data-stu-id="32574-114">In this tutorial we'll see how to add sorting and paging to our reports, which can be accomplished by simply checking a few checkboxes.</span></span> <span data-ttu-id="32574-115">不幸的是，這個簡單的實作有一些需要排序的介面，離開其缺點，分頁常式不專為有效率地分頁的大型結果集。</span><span class="sxs-lookup"><span data-stu-id="32574-115">Unfortunately, this simplistic implementation has its drawbacks the sorting interface leaves a bit to be desired and the paging routines are not designed for efficiently paging through large result sets.</span></span> <span data-ttu-id="32574-116">未來的教學課程將探討如何克服--現成的分頁和排序解決方案的限制。</span><span class="sxs-lookup"><span data-stu-id="32574-116">Future tutorials will explore how to overcome the limitations of the out-of-the-box paging and sorting solutions.</span></span>

## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a><span data-ttu-id="32574-117">步驟 1： 加入分頁和排序教學課程的網頁</span><span class="sxs-lookup"><span data-stu-id="32574-117">Step 1: Adding the Paging and Sorting Tutorial Web Pages</span></span>

<span data-ttu-id="32574-118">在開始本教學課程之前，可讓 s 先花點時間，將 ASP.NET 頁面，我們需要針對本教學課程和下列三個。</span><span class="sxs-lookup"><span data-stu-id="32574-118">Before we start this tutorial, let s first take a moment to add the ASP.NET pages we'll need for this tutorial and the next three.</span></span> <span data-ttu-id="32574-119">藉由建立新的資料夾中名為專案啟動`PagingAndSorting`。</span><span class="sxs-lookup"><span data-stu-id="32574-119">Start by creating a new folder in the project named `PagingAndSorting`.</span></span> <span data-ttu-id="32574-120">接下來，新增到此資料夾，將所有設定成使用主版頁面的 下列五個 ASP.NET 頁面`Site.master`:</span><span class="sxs-lookup"><span data-stu-id="32574-120">Next, add the following five ASP.NET pages to this folder, having all of them configured to use the master page `Site.master`:</span></span>

- `Default.aspx`
- `SimplePagingSorting.aspx`
- `EfficientPaging.aspx`
- `SortParameter.aspx`
- `CustomSortingUI.aspx`


![建立 PagingAndSorting 資料夾，並新增這些教學課程的 ASP.NET 網頁](paging-and-sorting-report-data-vb/_static/image1.png)

<span data-ttu-id="32574-122">**圖 1**： 建立 PagingAndSorting 資料夾，並新增這些教學課程的 ASP.NET 網頁</span><span class="sxs-lookup"><span data-stu-id="32574-122">**Figure 1**: Create a PagingAndSorting Folder and Add the Tutorial ASP.NET Pages</span></span>


<span data-ttu-id="32574-123">接下來，開啟`Default.aspx`頁面上，並拖曳`SectionLevelTutorialListing.ascx`從使用者控制`UserControls`拖曳至設計介面上的資料夾。</span><span class="sxs-lookup"><span data-stu-id="32574-123">Next, open the `Default.aspx` page and drag the `SectionLevelTutorialListing.ascx` User Control from the `UserControls` folder onto the Design surface.</span></span> <span data-ttu-id="32574-124">此使用者控制項中，我們在中建立[主版頁面與網站導覽](../introduction/master-pages-and-site-navigation-vb.md)教學課程中，列舉站台對應，並顯示這些教學課程中的項目符號清單中目前的區段。</span><span class="sxs-lookup"><span data-stu-id="32574-124">This User Control, which we created in the [Master Pages and Site Navigation](../introduction/master-pages-and-site-navigation-vb.md) tutorial, enumerates the site map and displays those tutorials in the current section in a bulleted list.</span></span>


![將 SectionLevelTutorialListing.ascx 使用者控制項新增至 Default.aspx](paging-and-sorting-report-data-vb/_static/image2.png)

<span data-ttu-id="32574-126">**圖 2**： 將 SectionLevelTutorialListing.ascx 使用者控制項新增至 Default.aspx</span><span class="sxs-lookup"><span data-stu-id="32574-126">**Figure 2**: Add the SectionLevelTutorialListing.ascx User Control to Default.aspx</span></span>


<span data-ttu-id="32574-127">若要有項目符號清單顯示分頁和排序我們將建立的教學課程，我們要將它們新增至站台對應。</span><span class="sxs-lookup"><span data-stu-id="32574-127">In order to have the bulleted list display the paging and sorting tutorials we'll be creating, we need to add them to the site map.</span></span> <span data-ttu-id="32574-128">開啟`Web.sitemap`檔案，並在編輯、 插入，以及刪除站台對應節點標記之後新增下列標記：</span><span class="sxs-lookup"><span data-stu-id="32574-128">Open the `Web.sitemap` file and add the following markup after the Editing, Inserting, and Deleting site map node markup:</span></span>


[!code-xml[Main](paging-and-sorting-report-data-vb/samples/sample1.xml)]


![更新站台對應，以包含新的 ASP.NET 網頁](paging-and-sorting-report-data-vb/_static/image3.png)

<span data-ttu-id="32574-130">**圖 3**： 更新站台對應，以包含新的 ASP.NET 網頁</span><span class="sxs-lookup"><span data-stu-id="32574-130">**Figure 3**: Update the Site Map to Include the New ASP.NET Pages</span></span>


## <a name="step-2-displaying-product-information-in-a-gridview"></a><span data-ttu-id="32574-131">步驟 2: GridView 中顯示產品資訊</span><span class="sxs-lookup"><span data-stu-id="32574-131">Step 2: Displaying Product Information in a GridView</span></span>

<span data-ttu-id="32574-132">我們實際實作分頁和排序功能之前，讓第一次建立標準非-srotable，不可分頁的 GridView 會列出產品資訊。</span><span class="sxs-lookup"><span data-stu-id="32574-132">Before we actually implement paging and sorting capabilities, let s first create a standard non-srotable, non-pageable GridView that lists the product information.</span></span> <span data-ttu-id="32574-133">這是工作我們完成之前多次在此教學課程系列讓這些步驟的 ve 應該很熟悉。</span><span class="sxs-lookup"><span data-stu-id="32574-133">This is a task we ve done many times before throughout this tutorial series so these steps should be familiar.</span></span> <span data-ttu-id="32574-134">首先開啟`SimplePagingSorting.aspx`頁面上，並將 GridView 控制項從工具箱拖曳至設計工具中，設定其`ID`屬性設`Products`。</span><span class="sxs-lookup"><span data-stu-id="32574-134">Start by opening the `SimplePagingSorting.aspx` page and drag a GridView control from the Toolbox onto the Designer, setting its `ID` property to `Products`.</span></span> <span data-ttu-id="32574-135">接下來，建立新的 ObjectDataSource 使用 ProductsBLL 類別的`GetProducts()`方法傳回的所有產品資訊。</span><span class="sxs-lookup"><span data-stu-id="32574-135">Next, create a new ObjectDataSource that uses the ProductsBLL class s `GetProducts()` method to return all of the product information.</span></span>


![擷取所有的產品使用 GetProducts() 方法的相關資訊](paging-and-sorting-report-data-vb/_static/image4.png)

<span data-ttu-id="32574-137">**圖 4**： 擷取所有的產品使用 GetProducts() 方法的相關資訊</span><span class="sxs-lookup"><span data-stu-id="32574-137">**Figure 4**: Retrieve Information About All of the Products Using the GetProducts() Method</span></span>


<span data-ttu-id="32574-138">因為這是唯讀的報表時，沒有 s 不需要對應的 ObjectDataSource 秒`Insert()`， `Update()`，或`Delete()`方法，以對應`ProductsBLL`方法; 因此，（無） 從清單中選擇下拉式清單的 INSERT、 UPDATE和刪除索引標籤。</span><span class="sxs-lookup"><span data-stu-id="32574-138">Since this report is a read-only report, there s no need to map the ObjectDataSource s `Insert()`, `Update()`, or `Delete()` methods to corresponding `ProductsBLL` methods; therefore, choose (None) from the drop-down list for the UPDATE, INSERT, and DELETE tabs.</span></span>


![選擇 （無） 選項下拉式清單中更新、 插入和刪除索引標籤](paging-and-sorting-report-data-vb/_static/image5.png)

<span data-ttu-id="32574-140">**圖 5**： 選擇 （無） 選項下拉式清單中更新、 插入和刪除索引標籤</span><span class="sxs-lookup"><span data-stu-id="32574-140">**Figure 5**: Choose the (None) Option in the Drop-Down List in the UPDATE, INSERT, and DELETE Tabs</span></span>


<span data-ttu-id="32574-141">接下來，讓 s 自訂 GridView 的欄位，以便只有產品名稱、 供應商、 類別、 價格和已停止的狀態會顯示。</span><span class="sxs-lookup"><span data-stu-id="32574-141">Next, let s customize the GridView s fields so that only the products names, suppliers, categories, prices, and discontinued statuses are displayed.</span></span> <span data-ttu-id="32574-142">此外，隨時進行欄位層級格式變更時，例如調整`HeaderText`內容] 或 [格式化為貨幣的價格。</span><span class="sxs-lookup"><span data-stu-id="32574-142">Furthermore, feel free to make any field-level formatting changes, such as adjusting the `HeaderText` properties or formatting the price as a currency.</span></span> <span data-ttu-id="32574-143">這些變更之後，請 GridView s 的宣告式標記看起來應該如下所示：</span><span class="sxs-lookup"><span data-stu-id="32574-143">After these changes, your GridView s declarative markup should look similar to the following:</span></span>


[!code-aspx[Main](paging-and-sorting-report-data-vb/samples/sample2.aspx)]

<span data-ttu-id="32574-144">圖 6 顯示我們進行到目前為止透過瀏覽器檢視時。</span><span class="sxs-lookup"><span data-stu-id="32574-144">Figure 6 shows our progress thus far when viewed through a browser.</span></span> <span data-ttu-id="32574-145">請注意，頁面會列出所有在一個畫面中，顯示每個產品的名稱、 類別、 供應商、 價格、 產品已停止的狀態。</span><span class="sxs-lookup"><span data-stu-id="32574-145">Note that the page lists all of the products in one screen, showing each product s name, category, supplier, price, and discontinued status.</span></span>


<span data-ttu-id="32574-146">[![每個產品詳列](paging-and-sorting-report-data-vb/_static/image7.png)](paging-and-sorting-report-data-vb/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="32574-146">[![Each of the Products are Listed](paging-and-sorting-report-data-vb/_static/image7.png)](paging-and-sorting-report-data-vb/_static/image6.png)</span></span>

<span data-ttu-id="32574-147">**圖 6**： 每個產品詳列 ([按一下以檢視完整大小的影像](paging-and-sorting-report-data-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="32574-147">**Figure 6**: Each of the Products are Listed ([Click to view full-size image](paging-and-sorting-report-data-vb/_static/image8.png))</span></span>


## <a name="step-3-adding-paging-support"></a><span data-ttu-id="32574-148">步驟 3： 加入分頁支援</span><span class="sxs-lookup"><span data-stu-id="32574-148">Step 3: Adding Paging Support</span></span>

<span data-ttu-id="32574-149">列出*所有*在某個畫面上的產品可能會導致使用者瀏覽資料的資訊負荷過重。</span><span class="sxs-lookup"><span data-stu-id="32574-149">Listing *all* of the products on one screen can lead to information overload for the user perusing the data.</span></span> <span data-ttu-id="32574-150">為了讓結果更容易管理，我們可以分割成較小的頁面資料的資料，並允許使用者在一頁資料透過一次。</span><span class="sxs-lookup"><span data-stu-id="32574-150">To help make the results more manageable, we can break up the data into smaller pages of data and allow the user to step through the data one page at a time.</span></span> <span data-ttu-id="32574-151">若要完成，這只是檢查啟用分頁 核取方塊的 GridView s 智慧標籤 (這會將 GridView s [ `AllowPaging`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowpaging.aspx)至`true`)。</span><span class="sxs-lookup"><span data-stu-id="32574-151">To accomplish this simply check the Enable Paging checkbox from the GridView s smart tag (this sets the GridView s [`AllowPaging` property](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowpaging.aspx) to `true`).</span></span>


<span data-ttu-id="32574-152">[![請檢查啟用分頁核取方塊，以新增分頁支援](paging-and-sorting-report-data-vb/_static/image10.png)](paging-and-sorting-report-data-vb/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="32574-152">[![Check the Enable Paging Checkbox to Add Paging Support](paging-and-sorting-report-data-vb/_static/image10.png)](paging-and-sorting-report-data-vb/_static/image9.png)</span></span>

<span data-ttu-id="32574-153">**圖 7**： 請檢查啟用分頁核取方塊以新增分頁支援 ([按一下以檢視完整大小的影像](paging-and-sorting-report-data-vb/_static/image11.png))</span><span class="sxs-lookup"><span data-stu-id="32574-153">**Figure 7**: Check the Enable Paging Checkbox to Add Paging Support ([Click to view full-size image](paging-and-sorting-report-data-vb/_static/image11.png))</span></span>


<span data-ttu-id="32574-154">啟用分頁限制每頁顯示的記錄數目，並將*分頁介面*至 GridView。</span><span class="sxs-lookup"><span data-stu-id="32574-154">Enabling paging limits the number of records shown per page and adds a *paging interface* to the GridView.</span></span> <span data-ttu-id="32574-155">[圖 7] 所示的預設分頁介面是一連串的頁面數字，讓使用者能夠從一頁資料快速巡覽至另一個。</span><span class="sxs-lookup"><span data-stu-id="32574-155">The default paging interface, shown in Figure 7, is a series of page numbers, allowing the user to quickly navigate from one page of data to another.</span></span> <span data-ttu-id="32574-156">此分頁介面應該看起來很熟悉，因為我們已見識過它時在過去的教學課程中，新增 DetailsView 和 FormView 控制項的分頁支援。</span><span class="sxs-lookup"><span data-stu-id="32574-156">This paging interface should look familiar, as we ve seen it when adding paging support to the DetailsView and FormView controls in past tutorials.</span></span>

<span data-ttu-id="32574-157">在 DetailsView 和 FormView 控制項只會顯示每個頁面的單一記錄。</span><span class="sxs-lookup"><span data-stu-id="32574-157">Both the DetailsView and FormView controls only show a single record per page.</span></span> <span data-ttu-id="32574-158">Gridview 裡，不過，會參考其[`PageSize`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.pagesize.aspx)來判斷每頁顯示的多少筆記錄 （此屬性預設為 10）。</span><span class="sxs-lookup"><span data-stu-id="32574-158">The GridView, however, consults its [`PageSize` property](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.pagesize.aspx) to determine how many records to show per page (this property defaults to a value of 10).</span></span>

<span data-ttu-id="32574-159">您可以使用下列屬性自訂此 GridView、 DetailsView 和 FormView 的分頁介面：</span><span class="sxs-lookup"><span data-stu-id="32574-159">This GridView, DetailsView, and FormView s paging interface can be customized using the following properties:</span></span>

- <span data-ttu-id="32574-160">`PagerStyle` 表示分頁介面的樣式資訊可以指定設定，例如`BackColor`， `ForeColor`， `CssClass`， `HorizontalAlign`，依此類推。</span><span class="sxs-lookup"><span data-stu-id="32574-160">`PagerStyle` indicates the style information for the paging interface; can specify settings like `BackColor`, `ForeColor`, `CssClass`, `HorizontalAlign`, and so on.</span></span>
- <span data-ttu-id="32574-161">`PagerSettings` 包含一系列的可自訂的分頁介面功能的屬性`PageButtonCount`指出數值頁面 （預設值為 10） 的分頁介面中顯示的數字的最大數目，而[`Mode`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pagersettings.mode.aspx)表示分頁介面的運作方式，且可以設定為：</span><span class="sxs-lookup"><span data-stu-id="32574-161">`PagerSettings` contains a bevy of properties that can customize the functionality of the paging interface; `PageButtonCount` indicates the maximum number of numeric page numbers displayed in the paging interface (the default is 10); the [`Mode` property](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pagersettings.mode.aspx) indicates how the paging interface operates and can be set to:</span></span> 

    - <span data-ttu-id="32574-162">`NextPrevious` 顯示下一個] 和 [上一步的按鈕，讓使用者能夠逐步向前或向後一頁一次</span><span class="sxs-lookup"><span data-stu-id="32574-162">`NextPrevious` shows a Next and Previous buttons, allowing the user to step forwards or backwards one page at a time</span></span>
    - <span data-ttu-id="32574-163">`NextPreviousFirstLast` 除了下一步 和 上一頁 按鈕，第一個和最後一個按鈕也會包含在內，可讓使用者快速移至第一個或最後一頁的資料</span><span class="sxs-lookup"><span data-stu-id="32574-163">`NextPreviousFirstLast` in addition to Next and Previous buttons, First and Last buttons are also included, allowing the user to quickly move to the first or last page of data</span></span>
    - <span data-ttu-id="32574-164">`Numeric` 顯示一系列可讓使用者立即跳到任何頁面的頁碼</span><span class="sxs-lookup"><span data-stu-id="32574-164">`Numeric` shows a series of page numbers, allowing the user to immediately jump to any page</span></span>
    - <span data-ttu-id="32574-165">`NumericFirstLast` 列印的頁碼，除了包含 第一個和最後一個按鈕，讓使用者能夠快速移至第一個或最後一頁的資料;第一個/最後一個按鈕才會顯示所有的數字的頁碼不能配合</span><span class="sxs-lookup"><span data-stu-id="32574-165">`NumericFirstLast` in addition to the page numbers, includes First and Last buttons, allowing the user to quickly move to the first or last page of data; the First/Last buttons are only shown if all of the numeric page numbers cannot fit</span></span>

<span data-ttu-id="32574-166">此外，GridView、 DetailsView 和 FormView 所有優惠`PageIndex`和`PageCount`分別表示目前正在檢視的頁面和資料的總頁數的屬性。</span><span class="sxs-lookup"><span data-stu-id="32574-166">Moreover, the GridView, DetailsView, and FormView all offer the `PageIndex` and `PageCount` properties, which indicate the current page being viewed and the total number of pages of data, respectively.</span></span> <span data-ttu-id="32574-167">`PageIndex`屬性索引從 0，也就是說，檢視資料的第一頁時開始`PageIndex`會等於 0。</span><span class="sxs-lookup"><span data-stu-id="32574-167">The `PageIndex` property is indexed starting at 0, meaning that when viewing the first page of data `PageIndex` will equal 0.</span></span> <span data-ttu-id="32574-168">`PageCount`相反地，啟動 計數 1，表示`PageIndex`僅限於值介於 0 和`PageCount - 1`。</span><span class="sxs-lookup"><span data-stu-id="32574-168">`PageCount`, on the other hand, starts counting at 1, meaning that `PageIndex` is limited to the values between 0 and `PageCount - 1`.</span></span>

<span data-ttu-id="32574-169">可讓 s 花一點時間來改善我們的 GridView 的分頁介面的預設外觀。</span><span class="sxs-lookup"><span data-stu-id="32574-169">Let s take a moment to improve the default appearance of our GridView s paging interface.</span></span> <span data-ttu-id="32574-170">具體來說，可讓 s 靠右對齊，淺灰色背景分頁介面。</span><span class="sxs-lookup"><span data-stu-id="32574-170">Specifically, let s have the paging interface right-aligned with a light gray background.</span></span> <span data-ttu-id="32574-171">而不是設定這些屬性直接透過 GridView s`PagerStyle`屬性，建立可讓 s 中的 CSS 類別`Styles.css`名為`PagerRowStyle`，然後指派`PagerStyle`s`CssClass`透過我們的佈景主題的屬性。</span><span class="sxs-lookup"><span data-stu-id="32574-171">Rather than setting these properties directly through the GridView s `PagerStyle` property, let s create a CSS class in `Styles.css` named `PagerRowStyle` and then assign the `PagerStyle` s `CssClass` property through our Theme.</span></span> <span data-ttu-id="32574-172">首先開啟`Styles.css`並新增下列 CSS 類別定義：</span><span class="sxs-lookup"><span data-stu-id="32574-172">Start by opening `Styles.css` and adding the following CSS class definition:</span></span>


[!code-css[Main](paging-and-sorting-report-data-vb/samples/sample3.css)]

<span data-ttu-id="32574-173">接下來，開啟`GridView.skin`檔案中`DataWebControls`資料夾內`App_Themes`資料夾。</span><span class="sxs-lookup"><span data-stu-id="32574-173">Next, open the `GridView.skin` file in the `DataWebControls` folder within the `App_Themes` folder.</span></span> <span data-ttu-id="32574-174">如我們所述*主版頁面與網站導覽*教學課程，面板檔案可以用來指定網頁控制項的預設屬性值。</span><span class="sxs-lookup"><span data-stu-id="32574-174">As we discussed in the *Master Pages and Site Navigation* tutorial, Skin files can be used to specify the default property values for a Web control.</span></span> <span data-ttu-id="32574-175">因此，增強現有的設定，以包含設定`PagerStyle`s`CssClass`屬性設`PagerRowStyle`。</span><span class="sxs-lookup"><span data-stu-id="32574-175">Therefore, augment the existing settings to include setting the `PagerStyle` s `CssClass` property to `PagerRowStyle`.</span></span> <span data-ttu-id="32574-176">此外，可讓 s 設定分頁介面，以顯示最多五個數值的頁面按鈕使用`NumericFirstLast`分頁介面。</span><span class="sxs-lookup"><span data-stu-id="32574-176">Also, let s configure the paging interface to show at most five numeric page buttons using the `NumericFirstLast` paging interface.</span></span>


[!code-aspx[Main](paging-and-sorting-report-data-vb/samples/sample4.aspx)]

## <a name="the-paging-user-experience"></a><span data-ttu-id="32574-177">分頁的使用者體驗</span><span class="sxs-lookup"><span data-stu-id="32574-177">The Paging User Experience</span></span>

<span data-ttu-id="32574-178">圖 8 顯示當 GridView s 啟用分頁 核取方塊已核取之後，透過瀏覽器瀏覽的網頁和`PagerStyle`並`PagerSettings`設定已透過`GridView.skin`檔案。</span><span class="sxs-lookup"><span data-stu-id="32574-178">Figure 8 shows the web page when visited through a browser after the GridView s Enable Paging checkbox has been checked and the `PagerStyle` and `PagerSettings` configurations have been made through the `GridView.skin` file.</span></span> <span data-ttu-id="32574-179">請注意如何唯一的十個記錄會顯示，且分頁介面表示我們正在檢視資料的第一頁。</span><span class="sxs-lookup"><span data-stu-id="32574-179">Note how only ten records are shown, and the paging interface indicates that we are viewing the first page of data.</span></span>


<span data-ttu-id="32574-180">[![使用已啟用分頁，一次顯示記錄的子集](paging-and-sorting-report-data-vb/_static/image13.png)](paging-and-sorting-report-data-vb/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="32574-180">[![With Paging Enabled, Only a Subset of the Records are Displayed at a Time](paging-and-sorting-report-data-vb/_static/image13.png)](paging-and-sorting-report-data-vb/_static/image12.png)</span></span>

<span data-ttu-id="32574-181">**[圖 8**： 只記錄的子集分頁已啟用] 會顯示一次 ([按一下以檢視完整大小的影像](paging-and-sorting-report-data-vb/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="32574-181">**Figure 8**: With Paging Enabled, Only a Subset of the Records are Displayed at a Time ([Click to view full-size image](paging-and-sorting-report-data-vb/_static/image14.png))</span></span>


<span data-ttu-id="32574-182">當使用者按一下其中一個頁面中的數字的分頁介面上時，回傳是兩邊彼此乾瞪眼，頁面會重新載入要求的頁面的記錄的顯示。</span><span class="sxs-lookup"><span data-stu-id="32574-182">When the user clicks on one of the page numbers in the paging interface, a postback ensues and the page reloads showing that requested page s records.</span></span> <span data-ttu-id="32574-183">選擇要檢視資料的最後一頁之後，[圖 9] 顯示結果。</span><span class="sxs-lookup"><span data-stu-id="32574-183">Figure 9 shows the results after opting to view the final page of data.</span></span> <span data-ttu-id="32574-184">請注意，最後一頁只會有一筆記錄;這是因為有 81 記錄總數，導致 含有單一記錄在每個頁面，再加上一頁的 10 筆記錄的八個頁面。</span><span class="sxs-lookup"><span data-stu-id="32574-184">Notice that the final page only has one record; this is because there are 81 records in total, resulting in eight pages of 10 records per page plus one page with a lone record.</span></span>


<span data-ttu-id="32574-185">[![按一下頁碼造成回傳，並顯示適當的記錄的子集](paging-and-sorting-report-data-vb/_static/image16.png)](paging-and-sorting-report-data-vb/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="32574-185">[![Clicking On a Page Number Causes a Postback and Shows the Appropriate Subset of Records](paging-and-sorting-report-data-vb/_static/image16.png)](paging-and-sorting-report-data-vb/_static/image15.png)</span></span>

<span data-ttu-id="32574-186">**圖 9**： 按一下頁碼造成回傳，並顯示適當的資料錄子集 ([按一下以檢視完整大小的影像](paging-and-sorting-report-data-vb/_static/image17.png))</span><span class="sxs-lookup"><span data-stu-id="32574-186">**Figure 9**: Clicking On a Page Number Causes a Postback and Shows the Appropriate Subset of Records ([Click to view full-size image](paging-and-sorting-report-data-vb/_static/image17.png))</span></span>


## <a name="paging-s-server-side-workflow"></a><span data-ttu-id="32574-187">工作流程-s 伺服器端分頁</span><span class="sxs-lookup"><span data-stu-id="32574-187">Paging s Server-Side Workflow</span></span>

<span data-ttu-id="32574-188">當使用者按一下分頁介面中的按鈕時，回傳是兩邊彼此乾瞪眼和下列伺服器端工作流程開始：</span><span class="sxs-lookup"><span data-stu-id="32574-188">When the end user clicks on a button in the paging interface, a postback ensues and the following server-side workflow begins:</span></span>

1. <span data-ttu-id="32574-189">GridView s （或 DetailsView 或 FormView）`PageIndexChanging`引發事件</span><span class="sxs-lookup"><span data-stu-id="32574-189">The GridView s (or DetailsView or FormView) `PageIndexChanging` event fires</span></span>
2. <span data-ttu-id="32574-190">ObjectDataSource 重新要求*所有*BLL; 中的資料的 GridView s`PageIndex`和`PageSize`屬性值來判斷需要在 GridView 中顯示的 BLL 從傳回的記錄</span><span class="sxs-lookup"><span data-stu-id="32574-190">The ObjectDataSource re-requests *all* of the data from the BLL; the GridView s `PageIndex` and `PageSize` property values are used to determine what records returned from the BLL need to be displayed in the GridView</span></span>
3. <span data-ttu-id="32574-191">GridView 的`PageIndexChanged`引發事件</span><span class="sxs-lookup"><span data-stu-id="32574-191">The GridView s `PageIndexChanged` event fires</span></span>

<span data-ttu-id="32574-192">在步驟 2，ObjectDataSource 重新要求所有的資料來源的資料。</span><span class="sxs-lookup"><span data-stu-id="32574-192">In Step 2, the ObjectDataSource re-requests all of the data from its data source.</span></span> <span data-ttu-id="32574-193">這種樣式的分頁通常稱為*預設分頁*，s 的分頁行為的預設設定時，所使用如同`AllowPaging`屬性設`true`。</span><span class="sxs-lookup"><span data-stu-id="32574-193">This style of paging is commonly referred to as *default paging*, as it s the paging behavior used by default when setting the `AllowPaging` property to `true`.</span></span> <span data-ttu-id="32574-194">預設值這個分頁資料 Web 控制項擷取所有記錄每一頁的資料，即使只記錄的子集實際會轉譯成 HTML s 傳送到瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="32574-194">With default paging the data Web control naively retrieves all records for each page of data, even though only a subset of records are actually rendered into the HTML that s sent to the browser.</span></span> <span data-ttu-id="32574-195">除非由 BLL 或 ObjectDataSource 快取的資料庫資料，預設的分頁是夠大的結果集或與許多並行使用者的 web 應用程式無法運作。</span><span class="sxs-lookup"><span data-stu-id="32574-195">Unless the database data is cached by the BLL or ObjectDataSource, default paging is unworkable for sufficiently large result sets or for web applications with many concurrent users.</span></span>

<span data-ttu-id="32574-196">在下一個教學課程中，我們將檢驗如何實作*自訂分頁*。</span><span class="sxs-lookup"><span data-stu-id="32574-196">In the next tutorial we'll examine how to implement *custom paging*.</span></span> <span data-ttu-id="32574-197">使用自訂分頁時，您可以特別指示，只擷取的記錄所需的要求的頁面資料的一組精確 ObjectDataSource。</span><span class="sxs-lookup"><span data-stu-id="32574-197">With custom paging you can specifically instruct the ObjectDataSource to only retrieve the precise set of records needed for the requested page of data.</span></span> <span data-ttu-id="32574-198">您可以想像得到，自訂分頁便可大幅提升對大型結果集進行分頁的效率。</span><span class="sxs-lookup"><span data-stu-id="32574-198">As you can imagine, custom paging greatly improves the efficiency of paging through large result sets.</span></span>

> [!NOTE]
> <span data-ttu-id="32574-199">雖然有許多同時使用者透過夠大的結果集或網站翻頁時，預設的分頁功能不適合，了解自訂分頁需要更多的變更和精力來實作，並不簡單，只要選取核取方塊，（因為是預設值分頁）。</span><span class="sxs-lookup"><span data-stu-id="32574-199">While default paging is not suitable when paging through sufficiently large result sets or for sites with many simultaneous users, realize that custom paging requires more changes and effort to implement and is not as simple as checking a checkbox (as is default paging).</span></span> <span data-ttu-id="32574-200">因此，預設的分頁功能可能是小型、 低流量網站或相當小的結果進行分頁的設定時，因為它的理想選擇 s 更容易、 更快速地實作。</span><span class="sxs-lookup"><span data-stu-id="32574-200">Therefore, default paging may be the ideal choice for small, low-traffic websites or when paging through relatively small result sets, as it s much easier and quicker to implement.</span></span>


<span data-ttu-id="32574-201">比方說，如果我們知道，我們會在資料庫中，永遠不會有 100 個以上的產品，藉由自訂分頁的最小的效能改善的實作所需的心力可能位移。</span><span class="sxs-lookup"><span data-stu-id="32574-201">For example, if we know that we'll never have more than 100 products in our database, the minimal performance gain enjoyed by custom paging is likely offset by the effort required to implement it.</span></span> <span data-ttu-id="32574-202">如果，不過，我們可能一天有數千或數以萬計的產品*不*實作自訂的分頁會大幅會拖累我們的應用程式的延展性。</span><span class="sxs-lookup"><span data-stu-id="32574-202">If, however, we may one day have thousands or tens of thousands of products, *not* implementing custom paging would greatly hamper the scalability of our application.</span></span>

## <a name="step-4-customizing-the-paging-experience"></a><span data-ttu-id="32574-203">步驟 4： 自訂分頁體驗</span><span class="sxs-lookup"><span data-stu-id="32574-203">Step 4: Customizing the Paging Experience</span></span>

<span data-ttu-id="32574-204">資料 Web 控制項提供許多可用來增強使用者的分頁經驗的屬性。</span><span class="sxs-lookup"><span data-stu-id="32574-204">The data Web controls provide a number of properties that can be used to enhance the user s paging experience.</span></span> <span data-ttu-id="32574-205">`PageCount`屬性，例如，指出有多少總頁數，雖然`PageIndex`屬性表示目前所造訪的頁面，並可以快速移至特定頁面的 使用者設定。</span><span class="sxs-lookup"><span data-stu-id="32574-205">The `PageCount` property, for example, indicates how many total pages there are, while the `PageIndex` property indicates the current page being visited and can be set to quickly move a user to a specific page.</span></span> <span data-ttu-id="32574-206">若要說明如何使用這些屬性來改善使用者的分頁體驗，可讓新增標籤的 s Web 控制對我們會通知使用者頁面的頁面它們 re 目前瀏覽時，以及讓他們快速跳至指定的頁面 DropDownList 控制項.</span><span class="sxs-lookup"><span data-stu-id="32574-206">To illustrate how to use these properties to improve upon the user s paging experience, let s add a Label Web control to our page that informs the user what page they re currently visiting, along with a DropDownList control that allows them to quickly jump to any given page.</span></span>

<span data-ttu-id="32574-207">首先，將 Label Web 控制項新增至您的頁面，設定其`ID`屬性，以`PagingInformation`，並清除其`Text`屬性。</span><span class="sxs-lookup"><span data-stu-id="32574-207">First, add a Label Web control to your page, set its `ID` property to `PagingInformation`, and clear out its `Text` property.</span></span> <span data-ttu-id="32574-208">接下來，建立事件處理常式 GridView s`DataBound`事件，並新增下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="32574-208">Next, create an event handler for the GridView s `DataBound` event and add the following code:</span></span>


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample5.vb)]

<span data-ttu-id="32574-209">這個事件處理常式指派`PagingInformation`標籤 s`Text`訊息，通知使用者頁面目前瀏覽屬性`Products.PageIndex + 1`多少的總頁數超出`Products.PageCount`(我們將新增 1 至`Products.PageIndex`屬性因為`PageIndex`編製索引從 0 開始)。</span><span class="sxs-lookup"><span data-stu-id="32574-209">This event handler assigns the `PagingInformation` Label s `Text` property to a message informing the user the page they are currently visiting `Products.PageIndex + 1` out of how many total pages `Products.PageCount` (we add 1 to the `Products.PageIndex` property because `PageIndex` is indexed starting at 0).</span></span> <span data-ttu-id="32574-210">我選擇指派此標籤 s`Text`中的屬性`DataBound`事件處理常式，而不是`PageIndexChanged`事件處理常式因為`DataBound`事件引發時，每次資料繫結至 GridView 而`PageIndexChanged`只有事件處理常式頁面索引變更時引發。</span><span class="sxs-lookup"><span data-stu-id="32574-210">I chose the assign this Label s `Text` property in the `DataBound` event handler as opposed to the `PageIndexChanged` event handler because the `DataBound` event fires every time data is bound to the GridView whereas the `PageIndexChanged` event handler only fires when the page index is changed.</span></span> <span data-ttu-id="32574-211">當 GridView 一開始會繫結的第一頁上的資料瀏覽`PageIndexChanging`事件不 t 引發 (而`DataBound`事件沒有)。</span><span class="sxs-lookup"><span data-stu-id="32574-211">When the GridView is initially data bound on the first page visit, the `PageIndexChanging` event doesn t fire (whereas the `DataBound` event does).</span></span>

<span data-ttu-id="32574-212">此步驟中，使用者現在會顯示一則訊息指出他們造訪哪些頁面而的資料有多少的總頁數。</span><span class="sxs-lookup"><span data-stu-id="32574-212">With this addition, the user is now shown a message indicating what page they are visiting and how many total pages of data there are.</span></span>


<span data-ttu-id="32574-213">[![會顯示目前的頁碼和總頁數](paging-and-sorting-report-data-vb/_static/image19.png)](paging-and-sorting-report-data-vb/_static/image18.png)</span><span class="sxs-lookup"><span data-stu-id="32574-213">[![The Current Page Number and Total Number of Pages are Displayed](paging-and-sorting-report-data-vb/_static/image19.png)](paging-and-sorting-report-data-vb/_static/image18.png)</span></span>

<span data-ttu-id="32574-214">**圖 10**： 會顯示目前的頁碼和總頁數 ([按一下以檢視完整大小的影像](paging-and-sorting-report-data-vb/_static/image20.png))</span><span class="sxs-lookup"><span data-stu-id="32574-214">**Figure 10**: The Current Page Number and Total Number of Pages are Displayed ([Click to view full-size image](paging-and-sorting-report-data-vb/_static/image20.png))</span></span>


<span data-ttu-id="32574-215">除了標籤控制項，可讓 s 也新增 DropDownList 控制項，與目前所檢視的頁面，選取列出的 GridView 內的頁碼。</span><span class="sxs-lookup"><span data-stu-id="32574-215">In addition to the Label control, let s also add a DropDownList control that lists the page numbers in the GridView with the currently viewed page selected.</span></span> <span data-ttu-id="32574-216">這意思是，使用者可以快速跳從目前的頁面之間只需從 DropDownList 中選取新的頁面索引。</span><span class="sxs-lookup"><span data-stu-id="32574-216">The idea here is that the user can quickly jump from the current page to another by simply selecting the new page index from the DropDownList.</span></span> <span data-ttu-id="32574-217">開始將 dropdownlist 進行加入至設計工具中，設定其`ID`屬性設`PageList`並檢查它的智慧標籤的 啟用 AutoPostBack 選項。</span><span class="sxs-lookup"><span data-stu-id="32574-217">Start by adding a DropDownList to the Designer, setting its `ID` property to `PageList` and checking the Enable AutoPostBack option from its smart tag.</span></span>

<span data-ttu-id="32574-218">接下來，回到`DataBound`事件處理常式，並新增下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="32574-218">Next, return to the `DataBound` event handler and add the following code:</span></span>


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample6.vb)]

<span data-ttu-id="32574-219">此程式碼一開始會清除中的項目`PageList`DropDownList。</span><span class="sxs-lookup"><span data-stu-id="32574-219">This code begins by clearing out the items in the `PageList` DropDownList.</span></span> <span data-ttu-id="32574-220">這似乎是多餘的因為一個不 t 預期的頁數，若要變更，但其他使用者可能會同時使用系統、 新增或移除記錄從`Products`資料表。</span><span class="sxs-lookup"><span data-stu-id="32574-220">This may seem superfluous, since one wouldn t expect the number of pages to change, but other users may be using the system simultaneously, adding or removing records from the `Products` table.</span></span> <span data-ttu-id="32574-221">這類插入或刪除無法改變資料的頁數。</span><span class="sxs-lookup"><span data-stu-id="32574-221">Such insertions or deletions could alter the number of pages of data.</span></span>

<span data-ttu-id="32574-222">接下來，我們需要建立一次列印的頁碼已對應至目前的 GridView`PageIndex`預設選取。</span><span class="sxs-lookup"><span data-stu-id="32574-222">Next, we need to create the page numbers again and have the one that maps to the current GridView `PageIndex` selected by default.</span></span> <span data-ttu-id="32574-223">我們完成這項作業具有從 0 到迴圈`PageCount - 1`，加入新`ListItem`中每個反覆項目和設定其`Selected`屬性設定為 true，如果目前的反覆項目索引等於 GridView 的`PageIndex`屬性。</span><span class="sxs-lookup"><span data-stu-id="32574-223">We accomplish this with a loop from 0 to `PageCount - 1`, adding a new `ListItem` in each iteration and setting its `Selected` property to true if the current iteration index equals the GridView s `PageIndex` property.</span></span>

<span data-ttu-id="32574-224">最後，我們需要建立事件處理常式 DropDownList s`SelectedIndexChanged`引發的事件，每當使用者選擇不同的項目，從清單中。</span><span class="sxs-lookup"><span data-stu-id="32574-224">Finally, we need to create an event handler for the DropDownList s `SelectedIndexChanged` event, which fires each time the user pick a different item from the list.</span></span> <span data-ttu-id="32574-225">若要建立這個事件處理常式，只要按兩下 DropDownList 在設計師中，然後新增下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="32574-225">To create this event handler, simply double-click the DropDownList in the Designer, then add the following code:</span></span>


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample7.vb)]

<span data-ttu-id="32574-226">如 [圖 11] 所示，只要變更的 GridView s`PageIndex`屬性會導致資料重新繫結至 GridView。</span><span class="sxs-lookup"><span data-stu-id="32574-226">As Figure 11 shows, merely changing the GridView s `PageIndex` property causes the data to be rebound to the GridView.</span></span> <span data-ttu-id="32574-227">在 GridView`DataBound`事件處理常式中，適當的 DropDownList`ListItem`已選取。</span><span class="sxs-lookup"><span data-stu-id="32574-227">In the GridView s `DataBound` event handler, the appropriate DropDownList `ListItem` is selected.</span></span>


<span data-ttu-id="32574-228">[![使用者會自動前往第六個頁面時選取的頁面 6 下拉式清單項目](paging-and-sorting-report-data-vb/_static/image22.png)](paging-and-sorting-report-data-vb/_static/image21.png)</span><span class="sxs-lookup"><span data-stu-id="32574-228">[![The User is Automatically Taken to the Sixth Page When Selecting the Page 6 Drop-Down List Item](paging-and-sorting-report-data-vb/_static/image22.png)](paging-and-sorting-report-data-vb/_static/image21.png)</span></span>

<span data-ttu-id="32574-229">**圖 11**: 使用者會自動前往第六個頁面時選取的頁面 6 下拉式清單項目 ([按一下以檢視完整大小的影像](paging-and-sorting-report-data-vb/_static/image23.png))</span><span class="sxs-lookup"><span data-stu-id="32574-229">**Figure 11**: The User is Automatically Taken to the Sixth Page When Selecting the Page 6 Drop-Down List Item ([Click to view full-size image](paging-and-sorting-report-data-vb/_static/image23.png))</span></span>


## <a name="step-5-adding-bi-directional-sorting-support"></a><span data-ttu-id="32574-230">步驟 5： 新增雙向排序支援</span><span class="sxs-lookup"><span data-stu-id="32574-230">Step 5: Adding Bi-Directional Sorting Support</span></span>

<span data-ttu-id="32574-231">加入雙向排序支援很簡單，只要新增分頁支援只需要檢查 GridView s 智慧標籤的 啟用排序選項 (可設定 GridView s [ `AllowSorting`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowsorting.aspx)至`true`)。</span><span class="sxs-lookup"><span data-stu-id="32574-231">Adding bi-directional sorting support is as simple as adding paging support simply check the Enable Sorting option from the GridView s smart tag (which sets the GridView s [`AllowSorting` property](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowsorting.aspx) to `true`).</span></span> <span data-ttu-id="32574-232">顯示如下的 GridView 的欄位標頭的每個 Linkbutton，按下時，造成回傳並傳回根據按下的資料行以遞增順序排序的資料。</span><span class="sxs-lookup"><span data-stu-id="32574-232">This renders each of the headers of the GridView s fields as LinkButtons that, when clicked, cause a postback and return the data sorted by the clicked column in ascending order.</span></span> <span data-ttu-id="32574-233">再次按一下相同的標頭 LinkButton 重新排序的資料，以遞減順序。</span><span class="sxs-lookup"><span data-stu-id="32574-233">Clicking the same header LinkButton again re-sorts the data in descending order.</span></span>

> [!NOTE]
> <span data-ttu-id="32574-234">如果您使用自訂的資料存取層，而不是具類型資料集，您不能啟用排序選項中的 GridView s 智慧標籤。</span><span class="sxs-lookup"><span data-stu-id="32574-234">If you are using a custom Data Access Layer rather than a Typed DataSet, you may not have an Enable Sorting option in the GridView s smart tag.</span></span> <span data-ttu-id="32574-235">繫結至原生支援 排序的資料來源的 Gridview 有可以使用此核取方塊。</span><span class="sxs-lookup"><span data-stu-id="32574-235">Only GridViews bound to data sources that natively support sorting have this checkbox available.</span></span> <span data-ttu-id="32574-236">輸入資料集提供的立即可用的排序支援，因為提供 ADO.NET DataTable`Sort`方法，叫用時，排序 s 使用指定之準則的 Datarow 的 DataTable。</span><span class="sxs-lookup"><span data-stu-id="32574-236">The Typed DataSet provides out-of-the-box sorting support since the ADO.NET DataTable provides a `Sort` method that, when invoked, sorts the DataTable s DataRows using the criteria specified.</span></span>


<span data-ttu-id="32574-237">如果您的 DAL 未傳回 dal 的原生支援可讓您排序您要設定將排序的資訊傳遞至商業邏輯層，可以排序資料或有資料 ObjectDataSource 排序的物件。</span><span class="sxs-lookup"><span data-stu-id="32574-237">If your DAL does not return objects that natively support sorting you will need to configure the ObjectDataSource to pass sorting information to the Business Logic Layer, which can either sort the data or have the data sorted by the DAL.</span></span> <span data-ttu-id="32574-238">我們將探討如何在未來的教學課程中排序資料，商務邏輯和資料存取層級。</span><span class="sxs-lookup"><span data-stu-id="32574-238">We'll explore how to sort data at the Business Logic and Data Access Layers in a future tutorial.</span></span>

<span data-ttu-id="32574-239">排序的 Linkbutton 會轉譯為 HTML 超連結，其目前的色彩 （藍色的未瀏覽的連結，然後瀏覽連結為深紅色） 衝突與標頭資料列的背景色彩。</span><span class="sxs-lookup"><span data-stu-id="32574-239">The sorting LinkButtons are rendered as HTML hyperlinks, whose current colors (blue for an unvisited link and a dark red for a visited link) clash with the background color of the header row.</span></span> <span data-ttu-id="32574-240">相反地，可讓 s 具有白色的不論是否在顯示的所有標頭資料列連結它們 ve 已瀏覽與否。</span><span class="sxs-lookup"><span data-stu-id="32574-240">Instead, let s have all header row links displayed in white, regardless of whether they ve been visited or not.</span></span> <span data-ttu-id="32574-241">這可藉由將下列內容加入`Styles.css`類別：</span><span class="sxs-lookup"><span data-stu-id="32574-241">This can be accomplished by adding the following to the `Styles.css` class:</span></span>


[!code-css[Main](paging-and-sorting-report-data-vb/samples/sample8.css)]

<span data-ttu-id="32574-242">此語法表示要顯示這些項目使用 HeaderStyle 類別內的超連結時，使用白色文字。</span><span class="sxs-lookup"><span data-stu-id="32574-242">This syntax indicates to use white text when displaying those hyperlinks within an element that uses the HeaderStyle class.</span></span>

<span data-ttu-id="32574-243">在此 CSS 新增功能之後, 造訪透過瀏覽器頁面時您的畫面看起來應該類似 圖 12。</span><span class="sxs-lookup"><span data-stu-id="32574-243">After this CSS addition, when visiting the page through a browser your screen should look similar to Figure 12.</span></span> <span data-ttu-id="32574-244">價格欄位標頭連結已按下之後，特別是，圖 12 顯示結果。</span><span class="sxs-lookup"><span data-stu-id="32574-244">In particular, Figure 12 shows the results after the Price field s header link has been clicked.</span></span>


<span data-ttu-id="32574-245">[![以遞增順序 UnitPrice 已經排序結果](paging-and-sorting-report-data-vb/_static/image25.png)](paging-and-sorting-report-data-vb/_static/image24.png)</span><span class="sxs-lookup"><span data-stu-id="32574-245">[![The Results Have Been Sorted by the UnitPrice in Ascending Order](paging-and-sorting-report-data-vb/_static/image25.png)](paging-and-sorting-report-data-vb/_static/image24.png)</span></span>

<span data-ttu-id="32574-246">**圖 12**: 結果已經排序以遞增順序 UnitPrice ([按一下以檢視完整大小的影像](paging-and-sorting-report-data-vb/_static/image26.png))</span><span class="sxs-lookup"><span data-stu-id="32574-246">**Figure 12**: The Results Have Been Sorted by the UnitPrice in Ascending Order ([Click to view full-size image](paging-and-sorting-report-data-vb/_static/image26.png))</span></span>


## <a name="examining-the-sorting-workflow"></a><span data-ttu-id="32574-247">檢查 排序的工作流程</span><span class="sxs-lookup"><span data-stu-id="32574-247">Examining the Sorting Workflow</span></span>

<span data-ttu-id="32574-248">所有的 GridView 欄位 BoundField CheckBoxField、 TemplateField，還有等`SortExpression`屬性，指出應該用來排序標頭連結該欄位 s 按一下時排序資料的運算式。</span><span class="sxs-lookup"><span data-stu-id="32574-248">All GridView fields the BoundField, CheckBoxField, TemplateField, and so on have a `SortExpression` property that indicates the expression that should be used to sort the data when that field s sorting header link is clicked.</span></span> <span data-ttu-id="32574-249">也有 GridView`SortExpression`屬性。</span><span class="sxs-lookup"><span data-stu-id="32574-249">The GridView also has a `SortExpression` property.</span></span> <span data-ttu-id="32574-250">排序的標頭，在按一下 LinkButton，GridView 會指派該欄位 s`SortExpression`值以其`SortExpression`屬性。</span><span class="sxs-lookup"><span data-stu-id="32574-250">When a sorting header LinkButton is clicked, the GridView assigns that field s `SortExpression` value to its `SortExpression` property.</span></span> <span data-ttu-id="32574-251">接下來，資料是從 ObjectDataSource 重新擷取，而且排序 GridView s 根據`SortExpression`屬性。</span><span class="sxs-lookup"><span data-stu-id="32574-251">Next, the data is re-retrieved from the ObjectDataSource and sorted according to the GridView s `SortExpression` property.</span></span> <span data-ttu-id="32574-252">下列清單詳細說明瓿當終端使用者排序 GridView 中的資料的步驟順序：</span><span class="sxs-lookup"><span data-stu-id="32574-252">The following list details the sequence of steps that transpires when an end user sorts the data in a GridView:</span></span>

1. <span data-ttu-id="32574-253">GridView s [Sorting 事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorting(VS.80).aspx)引發</span><span class="sxs-lookup"><span data-stu-id="32574-253">The GridView s [Sorting event](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorting(VS.80).aspx) fires</span></span>
2. <span data-ttu-id="32574-254">GridView s [ `SortExpression`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx)設定為`SortExpression`欄位的 LinkButton 已按下其排序的標頭</span><span class="sxs-lookup"><span data-stu-id="32574-254">The GridView s [`SortExpression` property](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx) is set to the `SortExpression` of the field whose sorting header LinkButton was clicked</span></span>
3. <span data-ttu-id="32574-255">ObjectDataSource 重新擷取所有的 BLL 中的資料，然後排序 GridView s 上使用的資料 `SortExpression`</span><span class="sxs-lookup"><span data-stu-id="32574-255">The ObjectDataSource re-retrieves all of the data from the BLL and then sorts the data using the GridView s `SortExpression`</span></span>
4. <span data-ttu-id="32574-256">GridView 的`PageIndex`屬性重設為 0，表示排序使用者時傳回給資料 （假設已實作的分頁支援） 的第一頁</span><span class="sxs-lookup"><span data-stu-id="32574-256">The GridView s `PageIndex` property is reset to 0, meaning that when sorting the user is returned to the first page of data (assuming paging support has been implemented)</span></span>
5. <span data-ttu-id="32574-257">GridView s [ `Sorted`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorted(VS.80).aspx)引發</span><span class="sxs-lookup"><span data-stu-id="32574-257">The GridView s [`Sorted` event](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorted(VS.80).aspx) fires</span></span>

<span data-ttu-id="32574-258">使用預設分頁時，預設的排序選項重新擷取像是*所有*記錄的 BLL。</span><span class="sxs-lookup"><span data-stu-id="32574-258">Like with default paging, the default sorting option re-retrieves *all* of the records from the BLL.</span></span> <span data-ttu-id="32574-259">使用未分頁排序時，或使用排序時的預設分頁，該處 s 沒有辦法避免這樣叫用 （除了快取的資料庫資料） 的效能。</span><span class="sxs-lookup"><span data-stu-id="32574-259">When using sorting without paging or when using sorting with default paging, there s no way to circumvent this performance hit (short of caching the database data).</span></span> <span data-ttu-id="32574-260">不過，我們會看到在未來的教學課程中，它可以使用自訂分頁時，有效率地排序的資料。</span><span class="sxs-lookup"><span data-stu-id="32574-260">However, as we'll see in a future tutorial, it s possible to efficiently sort data when using custom paging.</span></span>

<span data-ttu-id="32574-261">每個 GridView 欄位繫結時 ObjectDataSource 至 GridView 透過下拉式清單中的 GridView s 智慧標籤，自動擁有其`SortExpression`屬性中的資料欄位的名稱指派`ProductsRow`類別。</span><span class="sxs-lookup"><span data-stu-id="32574-261">When binding an ObjectDataSource to the GridView through the drop-down list in the GridView s smart tag, each GridView field automatically has its `SortExpression` property assigned to the name of the data field in the `ProductsRow` class.</span></span> <span data-ttu-id="32574-262">例如， `ProductName` BoundField s`SortExpression`設定為`ProductName`，如下列宣告式標記中所示：</span><span class="sxs-lookup"><span data-stu-id="32574-262">For example, the `ProductName` BoundField s `SortExpression` is set to `ProductName`, as shown in the following declarative markup:</span></span>


[!code-aspx[Main](paging-and-sorting-report-data-vb/samples/sample9.aspx)]

<span data-ttu-id="32574-263">您可以設定欄位，讓它 s 不可排序清除其`SortExpression`（將它指派給空字串） 的屬性。</span><span class="sxs-lookup"><span data-stu-id="32574-263">A field can be configured so that it s not sortable by clearing out its `SortExpression` property (assigning it to an empty string).</span></span> <span data-ttu-id="32574-264">若要舉例來說，想像一下，我們不想要讓我們依價格排序產品的客戶。</span><span class="sxs-lookup"><span data-stu-id="32574-264">To illustrate this, imagine that we didn t want to let our customers sort our products by price.</span></span> <span data-ttu-id="32574-265">`UnitPrice` BoundField 的`SortExpression`從宣告式標記，或是透過 [欄位] 對話方塊中 （也就是可存取上的 GridView s 智慧標籤中的 [編輯資料行] 連結即可），就可以移除屬性。</span><span class="sxs-lookup"><span data-stu-id="32574-265">The `UnitPrice` BoundField s `SortExpression` property can be removed either from the declarative markup or through the Fields dialog box (which is accessible by clicking on the Edit Columns link in the GridView s smart tag).</span></span>


![以遞增順序 UnitPrice 已經排序結果](paging-and-sorting-report-data-vb/_static/image27.png)

<span data-ttu-id="32574-267">**圖 13**： 以遞增順序 UnitPrice 已經排序結果</span><span class="sxs-lookup"><span data-stu-id="32574-267">**Figure 13**: The Results Have Been Sorted by the UnitPrice in Ascending Order</span></span>


<span data-ttu-id="32574-268">一次`SortExpression`屬性已移除`UnitPrice`BoundField 頁首會轉譯為文字而不是連結，藉此防止使用者排序資料的價格。</span><span class="sxs-lookup"><span data-stu-id="32574-268">Once the `SortExpression` property has been removed for the `UnitPrice` BoundField, the header is rendered as text rather than as a link, thereby preventing users from sorting the data by price.</span></span>


<span data-ttu-id="32574-269">[![藉由移除 SortExpression 屬性，使用者可以不會再排序依價格的產品](paging-and-sorting-report-data-vb/_static/image29.png)](paging-and-sorting-report-data-vb/_static/image28.png)</span><span class="sxs-lookup"><span data-stu-id="32574-269">[![By Removing the SortExpression Property, Users Can No Longer Sort the Products By Price](paging-and-sorting-report-data-vb/_static/image29.png)](paging-and-sorting-report-data-vb/_static/image28.png)</span></span>

<span data-ttu-id="32574-270">**圖 14**： 藉由移除 SortExpression 屬性，使用者可以不會再排序產品的價格 ([按一下以檢視完整大小的影像](paging-and-sorting-report-data-vb/_static/image30.png))</span><span class="sxs-lookup"><span data-stu-id="32574-270">**Figure 14**: By Removing the SortExpression Property, Users Can No Longer Sort the Products By Price ([Click to view full-size image](paging-and-sorting-report-data-vb/_static/image30.png))</span></span>


## <a name="programmatically-sorting-the-gridview"></a><span data-ttu-id="32574-271">以程式設計的方式排序 GridView</span><span class="sxs-lookup"><span data-stu-id="32574-271">Programmatically Sorting the GridView</span></span>

<span data-ttu-id="32574-272">您也可以排序 GridView 的內容以程式設計方式使用 GridView s [ `Sort`方法](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sort.aspx)。</span><span class="sxs-lookup"><span data-stu-id="32574-272">You can also sort the contents of the GridView programmatically by using the GridView s [`Sort` method](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sort.aspx).</span></span> <span data-ttu-id="32574-273">只需傳入`SortExpression`連同排序所依據的值[ `SortDirection` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sortdirection.aspx) (`Ascending`或`Descending`)，而且會重新排序 GridView 的資料。</span><span class="sxs-lookup"><span data-stu-id="32574-273">Simply pass in the `SortExpression` value to sort by along with the [`SortDirection`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sortdirection.aspx) (`Ascending` or `Descending`), and the GridView s data will be re-sorted.</span></span>

<span data-ttu-id="32574-274">想像一下，我們關閉排序原因`UnitPrice`是因為我們所擔心我們的客戶會只是購買只最低價的產品。</span><span class="sxs-lookup"><span data-stu-id="32574-274">Imagine that the reason we turned off sorting by the `UnitPrice` was because we were worried that our customers would simply buy only the lowest-priced products.</span></span> <span data-ttu-id="32574-275">不過，我們想要鼓勵他們購買成本最高的產品，因此我們 d 希望能夠排序到最少的產品價格，但只會從最耗費資源的價格。</span><span class="sxs-lookup"><span data-stu-id="32574-275">However, we want to encourage them to buy the most expensive products, so we d like them to be able to sort the products by price, but only from the most expensive price to the least.</span></span>

<span data-ttu-id="32574-276">若要完成這 Button Web 控制項加入頁面上，設定其`ID`屬性，以`SortPriceDescending`，並將其`Text`價格依排序的屬性。</span><span class="sxs-lookup"><span data-stu-id="32574-276">To accomplish this add a Button Web control to the page, set its `ID` property to `SortPriceDescending`, and its `Text` property to Sort by Price.</span></span> <span data-ttu-id="32574-277">接下來，建立 s 按鈕的 事件處理常式`Click`按兩下按鈕控制項設計工具中的事件。</span><span class="sxs-lookup"><span data-stu-id="32574-277">Next, create an event handler for the Button s `Click` event by double-clicking the Button control in the Designer.</span></span> <span data-ttu-id="32574-278">這個事件處理常式中加入下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="32574-278">Add the following code to this event handler:</span></span>


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample10.vb)]

<span data-ttu-id="32574-279">按一下此按鈕會傳回使用者第一頁從耗費最多的資源成本最低 （請參閱 圖 15） 的價格，依排序的產品。</span><span class="sxs-lookup"><span data-stu-id="32574-279">Clicking this Button returns the user to the first page with the products sorted by price, from most expensive to least expensive (see Figure 15).</span></span>


<span data-ttu-id="32574-280">[![按一下按鈕排序的產品成本最高到最低](paging-and-sorting-report-data-vb/_static/image32.png)](paging-and-sorting-report-data-vb/_static/image31.png)</span><span class="sxs-lookup"><span data-stu-id="32574-280">[![Clicking the Button Orders the Products From the Most Expensive to the Least](paging-and-sorting-report-data-vb/_static/image32.png)](paging-and-sorting-report-data-vb/_static/image31.png)</span></span>

<span data-ttu-id="32574-281">**圖 15**： 按一下按鈕排序產品從成本最高到最低 ([按一下以檢視完整大小的影像](paging-and-sorting-report-data-vb/_static/image33.png))</span><span class="sxs-lookup"><span data-stu-id="32574-281">**Figure 15**: Clicking the Button Orders the Products From the Most Expensive to the Least ([Click to view full-size image](paging-and-sorting-report-data-vb/_static/image33.png))</span></span>


## <a name="summary"></a><span data-ttu-id="32574-282">總結</span><span class="sxs-lookup"><span data-stu-id="32574-282">Summary</span></span>

<span data-ttu-id="32574-283">在本教學課程，我們了解如何實作分頁和排序功能的預設值，這兩者都是簡單，只要選取核取方塊項目 ！</span><span class="sxs-lookup"><span data-stu-id="32574-283">In this tutorial we saw how to implement default paging and sorting capabilities, both of which were as easy as checking a checkbox!</span></span> <span data-ttu-id="32574-284">當使用者排序，或透過資料頁時，事件發生類似的工作流程：</span><span class="sxs-lookup"><span data-stu-id="32574-284">When a user sorts or pages through data, a similar workflow unfolds:</span></span>

1. <span data-ttu-id="32574-285">回傳是兩邊彼此乾瞪眼</span><span class="sxs-lookup"><span data-stu-id="32574-285">A postback ensues</span></span>
2. <span data-ttu-id="32574-286">資料 Web 控制項 s 預先層級事件引發 (`PageIndexChanging`或`Sorting`)</span><span class="sxs-lookup"><span data-stu-id="32574-286">The data Web control s pre-level event fires (`PageIndexChanging` or `Sorting`)</span></span>
3. <span data-ttu-id="32574-287">所有的資料重新擷取 ObjectDataSource</span><span class="sxs-lookup"><span data-stu-id="32574-287">All of the data is re-retrieved by the ObjectDataSource</span></span>
4. <span data-ttu-id="32574-288">後置資料 Web 控制項 s 層級的事件引發 (`PageIndexChanged`或`Sorted`)</span><span class="sxs-lookup"><span data-stu-id="32574-288">The data Web control s post-level event fires (`PageIndexChanged` or `Sorted`)</span></span>

<span data-ttu-id="32574-289">實作基本的分頁和排序是變得輕而易舉，利用更有效率的自訂分頁，或進一步加強的分頁或排序介面必須施加投入更多心力。</span><span class="sxs-lookup"><span data-stu-id="32574-289">While implementing basic paging and sorting is a breeze, more effort must be exerted to utilize the more efficient custom paging or to further enhance the paging or sorting interface.</span></span> <span data-ttu-id="32574-290">未來的教學課程將探討這些主題。</span><span class="sxs-lookup"><span data-stu-id="32574-290">Future tutorials will explore these topics.</span></span>

<span data-ttu-id="32574-291">快樂地寫程式 ！</span><span class="sxs-lookup"><span data-stu-id="32574-291">Happy Programming!</span></span>

## <a name="about-the-author"></a><span data-ttu-id="32574-292">關於作者</span><span class="sxs-lookup"><span data-stu-id="32574-292">About the Author</span></span>

<span data-ttu-id="32574-293">[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。</span><span class="sxs-lookup"><span data-stu-id="32574-293">[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), author of seven ASP/ASP.NET books and founder of [4GuysFromRolla.com](http://www.4guysfromrolla.com), has been working with Microsoft Web technologies since 1998.</span></span> <span data-ttu-id="32574-294">Scott 會擔任獨立的顧問、 培訓講師和作家。</span><span class="sxs-lookup"><span data-stu-id="32574-294">Scott works as an independent consultant, trainer, and writer.</span></span> <span data-ttu-id="32574-295">他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。</span><span class="sxs-lookup"><span data-stu-id="32574-295">His latest book is [*Sams Teach Yourself ASP.NET 2.0 in 24 Hours*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco).</span></span> <span data-ttu-id="32574-296">他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)</span><span class="sxs-lookup"><span data-stu-id="32574-296">He can be reached at [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)</span></span> <span data-ttu-id="32574-297">或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。</span><span class="sxs-lookup"><span data-stu-id="32574-297">or via his blog, which can be found at [http://ScottOnWriting.NET](http://ScottOnWriting.NET).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="32574-298">[上一頁](creating-a-customized-sorting-user-interface-cs.md)
> [下一頁](efficiently-paging-through-large-amounts-of-data-vb.md)</span><span class="sxs-lookup"><span data-stu-id="32574-298">[Previous](creating-a-customized-sorting-user-interface-cs.md)
[Next](efficiently-paging-through-large-amounts-of-data-vb.md)</span></span>
