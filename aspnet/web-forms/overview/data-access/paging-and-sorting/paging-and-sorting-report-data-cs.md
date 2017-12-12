---
uid: web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-cs
title: "分頁和排序報表資料 (C#) |Microsoft 文件"
author: rick-anderson
description: "分頁和排序的兩個常見的功能，在線上的應用程式中顯示資料。 在此教學課程中我們將加入排序第一眼和..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: 811a6ef2-ec66-4c8e-a089-6f795056e288
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-cs
msc.type: authoredcontent
ms.openlocfilehash: fd365ca3ae8e832e368fa4c29c33af8a42cf41d2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="paging-and-sorting-report-data-c"></a><span data-ttu-id="ac25f-104">分頁和排序報表資料 (C#)</span><span class="sxs-lookup"><span data-stu-id="ac25f-104">Paging and Sorting Report Data (C#)</span></span>
====================
<span data-ttu-id="ac25f-105">由[Scott Mitchell](https://twitter.com/ScottOnWriting)</span><span class="sxs-lookup"><span data-stu-id="ac25f-105">by [Scott Mitchell](https://twitter.com/ScottOnWriting)</span></span>

<span data-ttu-id="ac25f-106">[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_24_CS.exe)或[下載 PDF](paging-and-sorting-report-data-cs/_static/datatutorial24cs1.pdf)</span><span class="sxs-lookup"><span data-stu-id="ac25f-106">[Download Sample App](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_24_CS.exe) or [Download PDF](paging-and-sorting-report-data-cs/_static/datatutorial24cs1.pdf)</span></span>

> <span data-ttu-id="ac25f-107">分頁和排序的兩個常見的功能，在線上的應用程式中顯示資料。</span><span class="sxs-lookup"><span data-stu-id="ac25f-107">Paging and sorting are two very common features when displaying data in an online application.</span></span> <span data-ttu-id="ac25f-108">在本教學課程中，我們要花第一眼加入排序和分頁我們的報表，我們將在未來教學課程時建置。</span><span class="sxs-lookup"><span data-stu-id="ac25f-108">In this tutorial we'll take a first look at adding sorting and paging to our reports, which we will then build upon in future tutorials.</span></span>


## <a name="introduction"></a><span data-ttu-id="ac25f-109">簡介</span><span class="sxs-lookup"><span data-stu-id="ac25f-109">Introduction</span></span>

<span data-ttu-id="ac25f-110">分頁和排序的兩個常見的功能，在線上的應用程式中顯示資料。</span><span class="sxs-lookup"><span data-stu-id="ac25f-110">Paging and sorting are two very common features when displaying data in an online application.</span></span> <span data-ttu-id="ac25f-111">比方說，搜尋時在線上書店 ASP.NET 書籍，可能有數百個這類的書籍，但列出的搜尋結果此報表會列出每個頁面僅十個相符項目。</span><span class="sxs-lookup"><span data-stu-id="ac25f-111">For example, when searching for ASP.NET books at an online bookstore, there may be hundreds of such books, but the report listing the search results lists only ten matches per page.</span></span> <span data-ttu-id="ac25f-112">此外，可以根據標題、 價格、 頁面計數、 作者姓名等排序結果。</span><span class="sxs-lookup"><span data-stu-id="ac25f-112">Moreover, the results can be sorted by title, price, page count, author name, and so on.</span></span> <span data-ttu-id="ac25f-113">雖然的過去 23 教學課程檢查如何建立各種不同的報表，包括介面，允許以新增、 編輯和刪除資料，我們不如何排序資料，而且只在查看過分頁範例我們我們看到已 FormView DetailsView 與控制項。</span><span class="sxs-lookup"><span data-stu-id="ac25f-113">While the past 23 tutorials have examined how to build a variety of reports, including interfaces that permit adding, editing, and deleting data, we ve not looked at how to sort data and the only paging examples we ve seen have been with the DetailsView and FormView controls.</span></span>

<span data-ttu-id="ac25f-114">在本教學課程中，我們會看到如何加入排序和分頁報表，這可以藉由只檢查幾個核取方塊來達成。</span><span class="sxs-lookup"><span data-stu-id="ac25f-114">In this tutorial we'll see how to add sorting and paging to our reports, which can be accomplished by simply checking a few checkboxes.</span></span> <span data-ttu-id="ac25f-115">不幸的是，這個簡單的實作就有其缺點排序介面會保留到需要位元和分頁常式不專為有效率地分頁的大型結果集。</span><span class="sxs-lookup"><span data-stu-id="ac25f-115">Unfortunately, this simplistic implementation has its drawbacks the sorting interface leaves a bit to be desired and the paging routines are not designed for efficiently paging through large result sets.</span></span> <span data-ttu-id="ac25f-116">未來的教學課程將探討如何克服--現成的分頁和排序解決方案的限制。</span><span class="sxs-lookup"><span data-stu-id="ac25f-116">Future tutorials will explore how to overcome the limitations of the out-of-the-box paging and sorting solutions.</span></span>

## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a><span data-ttu-id="ac25f-117">步驟 1： 加入分頁和排序教學課程的 Web 網頁</span><span class="sxs-lookup"><span data-stu-id="ac25f-117">Step 1: Adding the Paging and Sorting Tutorial Web Pages</span></span>

<span data-ttu-id="ac25f-118">開始本教學課程之前，可讓 s 先花點時間加入我們需要針對此教學課程中，下一步的三個 ASP.NET 網頁。</span><span class="sxs-lookup"><span data-stu-id="ac25f-118">Before we start this tutorial, let s first take a moment to add the ASP.NET pages we'll need for this tutorial and the next three.</span></span> <span data-ttu-id="ac25f-119">藉由建立新的資料夾中名為專案啟動`PagingAndSorting`。</span><span class="sxs-lookup"><span data-stu-id="ac25f-119">Start by creating a new folder in the project named `PagingAndSorting`.</span></span> <span data-ttu-id="ac25f-120">接下來，在此資料夾時，需要全部都設定為使用主版頁面中加入下列五個 ASP.NET 網頁`Site.master`:</span><span class="sxs-lookup"><span data-stu-id="ac25f-120">Next, add the following five ASP.NET pages to this folder, having all of them configured to use the master page `Site.master`:</span></span>

- `Default.aspx`
- `SimplePagingSorting.aspx`
- `EfficientPaging.aspx`
- `SortParameter.aspx`
- `CustomSortingUI.aspx`


![建立 PagingAndSorting 資料夾，以及新增的教學課程的 ASP.NET 頁面](paging-and-sorting-report-data-cs/_static/image1.png)

<span data-ttu-id="ac25f-122">**圖 1**： 建立 PagingAndSorting 資料夾，以及新增的教學課程的 ASP.NET 頁面</span><span class="sxs-lookup"><span data-stu-id="ac25f-122">**Figure 1**: Create a PagingAndSorting Folder and Add the Tutorial ASP.NET Pages</span></span>


<span data-ttu-id="ac25f-123">接下來，開啟`Default.aspx`頁面上，拖曳`SectionLevelTutorialListing.ascx`使用者控制項`UserControls`資料夾拖曳至設計介面。</span><span class="sxs-lookup"><span data-stu-id="ac25f-123">Next, open the `Default.aspx` page and drag the `SectionLevelTutorialListing.ascx` User Control from the `UserControls` folder onto the Design surface.</span></span> <span data-ttu-id="ac25f-124">此使用者控制項，我們在建立[主版頁面和站台瀏覽](../introduction/master-pages-and-site-navigation-cs.md)教學課程中，列舉站台對應，並在項目符號清單中目前區段中顯示這些教學課程。</span><span class="sxs-lookup"><span data-stu-id="ac25f-124">This User Control, which we created in the [Master Pages and Site Navigation](../introduction/master-pages-and-site-navigation-cs.md) tutorial, enumerates the site map and displays those tutorials in the current section in a bulleted list.</span></span>


![將 SectionLevelTutorialListing.ascx 使用者控制項加入至 Default.aspx](paging-and-sorting-report-data-cs/_static/image2.png)

<span data-ttu-id="ac25f-126">**圖 2**： 將 SectionLevelTutorialListing.ascx 使用者控制項加入至 Default.aspx</span><span class="sxs-lookup"><span data-stu-id="ac25f-126">**Figure 2**: Add the SectionLevelTutorialListing.ascx User Control to Default.aspx</span></span>


<span data-ttu-id="ac25f-127">若要顯示分頁和排序教學課程，我們將建立的項目符號清單後，我們要將它們新增至站台對應。</span><span class="sxs-lookup"><span data-stu-id="ac25f-127">In order to have the bulleted list display the paging and sorting tutorials we'll be creating, we need to add them to the site map.</span></span> <span data-ttu-id="ac25f-128">開啟`Web.sitemap`檔案，並編輯、 插入，以及刪除站台對應節點標記後面加入下列標記：</span><span class="sxs-lookup"><span data-stu-id="ac25f-128">Open the `Web.sitemap` file and add the following markup after the Editing, Inserting, and Deleting site map node markup:</span></span>


[!code-xml[Main](paging-and-sorting-report-data-cs/samples/sample1.xml)]


![更新站台對應，以包含新的 ASP.NET 網頁](paging-and-sorting-report-data-cs/_static/image3.png)

<span data-ttu-id="ac25f-130">**圖 3**： 更新包含新的 ASP.NET 網頁站台對應</span><span class="sxs-lookup"><span data-stu-id="ac25f-130">**Figure 3**: Update the Site Map to Include the New ASP.NET Pages</span></span>


## <a name="step-2-displaying-product-information-in-a-gridview"></a><span data-ttu-id="ac25f-131">步驟 2： 在 GridView 中顯示產品資訊</span><span class="sxs-lookup"><span data-stu-id="ac25f-131">Step 2: Displaying Product Information in a GridView</span></span>

<span data-ttu-id="ac25f-132">我們實際上會實作分頁和排序功能之前，可讓第一次建立標準非-srotable，不可分頁的 GridView 會列出產品資訊。</span><span class="sxs-lookup"><span data-stu-id="ac25f-132">Before we actually implement paging and sorting capabilities, let s first create a standard non-srotable, non-pageable GridView that lists the product information.</span></span> <span data-ttu-id="ac25f-133">這是工作我們 ve 多次之前在此教學課程系列因此這些步驟的完成應該很熟悉。</span><span class="sxs-lookup"><span data-stu-id="ac25f-133">This is a task we ve done many times before throughout this tutorial series so these steps should be familiar.</span></span> <span data-ttu-id="ac25f-134">先開啟`SimplePagingSorting.aspx`頁面上，並將 GridView 控制項從工具箱拖曳至設計工具中，設定其`ID`屬性`Products`。</span><span class="sxs-lookup"><span data-stu-id="ac25f-134">Start by opening the `SimplePagingSorting.aspx` page and drag a GridView control from the Toolbox onto the Designer, setting its `ID` property to `Products`.</span></span> <span data-ttu-id="ac25f-135">接下來，建立新的 ObjectDataSource 使用 ProductsBLL 類別的`GetProducts()`方法來傳回所有產品資訊。</span><span class="sxs-lookup"><span data-stu-id="ac25f-135">Next, create a new ObjectDataSource that uses the ProductsBLL class s `GetProducts()` method to return all of the product information.</span></span>


![擷取的所有產品使用 GetProducts() 方法的相關資訊](paging-and-sorting-report-data-cs/_static/image4.png)

<span data-ttu-id="ac25f-137">**圖 4**： 擷取的所有產品使用 GetProducts() 方法的相關資訊</span><span class="sxs-lookup"><span data-stu-id="ac25f-137">**Figure 4**: Retrieve Information About All of the Products Using the GetProducts() Method</span></span>


<span data-ttu-id="ac25f-138">由於此報告可是唯讀的報表時，有 s 不需要對應 ObjectDataSource s `Insert()`， `Update()`，或`Delete()`方法來對應`ProductsBLL`方法; 因此，（無） 從清單中選擇下拉式清單的 INSERT、 UPDATE和刪除索引標籤。</span><span class="sxs-lookup"><span data-stu-id="ac25f-138">Since this report is a read-only report, there s no need to map the ObjectDataSource s `Insert()`, `Update()`, or `Delete()` methods to corresponding `ProductsBLL` methods; therefore, choose (None) from the drop-down list for the UPDATE, INSERT, and DELETE tabs.</span></span>


![選擇 （無） 選項下拉式清單中更新、 插入和刪除索引標籤](paging-and-sorting-report-data-cs/_static/image5.png)

<span data-ttu-id="ac25f-140">**圖 5**： 選擇 （無） 選項下拉式清單中更新、 插入和刪除索引標籤</span><span class="sxs-lookup"><span data-stu-id="ac25f-140">**Figure 5**: Choose the (None) Option in the Drop-Down List in the UPDATE, INSERT, and DELETE Tabs</span></span>


<span data-ttu-id="ac25f-141">接下來，讓自訂的 GridView 的欄位以便只產品名稱、 供應商、 分類、 價格和已停止的狀態會顯示 s。</span><span class="sxs-lookup"><span data-stu-id="ac25f-141">Next, let s customize the GridView s fields so that only the products names, suppliers, categories, prices, and discontinued statuses are displayed.</span></span> <span data-ttu-id="ac25f-142">此外，可自由進行欄位層級格式變更時，例如調整`HeaderText`屬性或格式化為貨幣的價格。</span><span class="sxs-lookup"><span data-stu-id="ac25f-142">Furthermore, feel free to make any field-level formatting changes, such as adjusting the `HeaderText` properties or formatting the price as a currency.</span></span> <span data-ttu-id="ac25f-143">這些變更之後，請 GridView s 的宣告式標記看起來應該如下所示：</span><span class="sxs-lookup"><span data-stu-id="ac25f-143">After these changes, your GridView s declarative markup should look similar to the following:</span></span>


[!code-aspx[Main](paging-and-sorting-report-data-cs/samples/sample2.aspx)]

<span data-ttu-id="ac25f-144">圖 6 為止顯示進度，透過瀏覽器檢視時。</span><span class="sxs-lookup"><span data-stu-id="ac25f-144">Figure 6 shows our progress thus far when viewed through a browser.</span></span> <span data-ttu-id="ac25f-145">請注意頁面列出所有在一個畫面中，顯示每個產品的名稱、 類別、 供應商、 價格、 產品，並已停止的狀態。</span><span class="sxs-lookup"><span data-stu-id="ac25f-145">Note that the page lists all of the products in one screen, showing each product s name, category, supplier, price, and discontinued status.</span></span>


<span data-ttu-id="ac25f-146">[![列出每個產品](paging-and-sorting-report-data-cs/_static/image7.png)](paging-and-sorting-report-data-cs/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="ac25f-146">[![Each of the Products are Listed](paging-and-sorting-report-data-cs/_static/image7.png)](paging-and-sorting-report-data-cs/_static/image6.png)</span></span>

<span data-ttu-id="ac25f-147">**圖 6**： 列出每個產品 ([按一下以檢視完整大小的影像](paging-and-sorting-report-data-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="ac25f-147">**Figure 6**: Each of the Products are Listed ([Click to view full-size image](paging-and-sorting-report-data-cs/_static/image8.png))</span></span>


## <a name="step-3-adding-paging-support"></a><span data-ttu-id="ac25f-148">步驟 3： 加入分頁支援</span><span class="sxs-lookup"><span data-stu-id="ac25f-148">Step 3: Adding Paging Support</span></span>

<span data-ttu-id="ac25f-149">列出*所有*的上一個螢幕的產品可能會導致資訊的使用者瀏覽資料的多載。</span><span class="sxs-lookup"><span data-stu-id="ac25f-149">Listing *all* of the products on one screen can lead to information overload for the user perusing the data.</span></span> <span data-ttu-id="ac25f-150">為了讓結果更容易管理，我們可以分解成較小的頁面資料的資料，並允許使用者在一頁資料透過一次。</span><span class="sxs-lookup"><span data-stu-id="ac25f-150">To help make the results more manageable, we can break up the data into smaller pages of data and allow the user to step through the data one page at a time.</span></span> <span data-ttu-id="ac25f-151">若要完成這只會檢查啟用分頁 核取方塊的 GridView s 智慧標籤 (這會設定 GridView s [ `AllowPaging`屬性](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.allowpaging.aspx)至`true`)。</span><span class="sxs-lookup"><span data-stu-id="ac25f-151">To accomplish this simply check the Enable Paging checkbox from the GridView s smart tag (this sets the GridView s [`AllowPaging` property](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.allowpaging.aspx) to `true`).</span></span>


<span data-ttu-id="ac25f-152">[![選取要加入的分頁支援啟用分頁核取方塊](paging-and-sorting-report-data-cs/_static/image10.png)](paging-and-sorting-report-data-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="ac25f-152">[![Check the Enable Paging Checkbox to Add Paging Support](paging-and-sorting-report-data-cs/_static/image10.png)](paging-and-sorting-report-data-cs/_static/image9.png)</span></span>

<span data-ttu-id="ac25f-153">**圖 7**： 檢查啟用分頁核取方塊以新增分頁支援 ([按一下以檢視完整大小的影像](paging-and-sorting-report-data-cs/_static/image11.png))</span><span class="sxs-lookup"><span data-stu-id="ac25f-153">**Figure 7**: Check the Enable Paging Checkbox to Add Paging Support ([Click to view full-size image](paging-and-sorting-report-data-cs/_static/image11.png))</span></span>


<span data-ttu-id="ac25f-154">啟用分頁限制每頁顯示的記錄數目，並將*分頁介面*至 GridView。</span><span class="sxs-lookup"><span data-stu-id="ac25f-154">Enabling paging limits the number of records shown per page and adds a *paging interface* to the GridView.</span></span> <span data-ttu-id="ac25f-155">圖 7 所示的預設分頁介面是一連串的頁面數字，讓使用者快速瀏覽一頁的資料從另一個。</span><span class="sxs-lookup"><span data-stu-id="ac25f-155">The default paging interface, shown in Figure 7, is a series of page numbers, allowing the user to quickly navigate from one page of data to another.</span></span> <span data-ttu-id="ac25f-156">此分頁介面應該看起來很熟悉，當我們 ve DetailsView 和 FormView 控制項加入分頁支援，在過去的教學課程時看到它。</span><span class="sxs-lookup"><span data-stu-id="ac25f-156">This paging interface should look familiar, as we ve seen it when adding paging support to the DetailsView and FormView controls in past tutorials.</span></span>

<span data-ttu-id="ac25f-157">DetailsView 和 FormView 控制項只會顯示每個頁面的單一記錄。</span><span class="sxs-lookup"><span data-stu-id="ac25f-157">Both the DetailsView and FormView controls only show a single record per page.</span></span> <span data-ttu-id="ac25f-158">在 GridView，不過，參照其[`PageSize`屬性](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.gridview.pagesize.aspx)來判斷每頁顯示的多少筆記錄 （此屬性預設為 10）。</span><span class="sxs-lookup"><span data-stu-id="ac25f-158">The GridView, however, consults its [`PageSize` property](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.gridview.pagesize.aspx) to determine how many records to show per page (this property defaults to a value of 10).</span></span>

<span data-ttu-id="ac25f-159">使用下列屬性可以自訂此 GridView、 DetailsView 和 FormView 的分頁介面：</span><span class="sxs-lookup"><span data-stu-id="ac25f-159">This GridView, DetailsView, and FormView s paging interface can be customized using the following properties:</span></span>

- <span data-ttu-id="ac25f-160">`PagerStyle`表示樣式資訊的分頁介面。可以指定設定，例如`BackColor`， `ForeColor`， `CssClass`， `HorizontalAlign`，依此類推。</span><span class="sxs-lookup"><span data-stu-id="ac25f-160">`PagerStyle` indicates the style information for the paging interface; can specify settings like `BackColor`, `ForeColor`, `CssClass`, `HorizontalAlign`, and so on.</span></span>
- <span data-ttu-id="ac25f-161">`PagerSettings`包含屬性，可以自訂分頁介面; 功能的 first`PageButtonCount`表示數值頁面 （預設值為 10） 的分頁介面中顯示的數字的最大數目; [ `Mode`屬性](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.pagersettings.mode.aspx)表示分頁介面如何運作，且可以設定為：</span><span class="sxs-lookup"><span data-stu-id="ac25f-161">`PagerSettings` contains a bevy of properties that can customize the functionality of the paging interface; `PageButtonCount` indicates the maximum number of numeric page numbers displayed in the paging interface (the default is 10); the [`Mode` property](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.pagersettings.mode.aspx) indicates how the paging interface operates and can be set to:</span></span> 

    - <span data-ttu-id="ac25f-162">`NextPrevious`顯示下一個] 和 [上一步的按鈕，讓使用者逐步向前或向後一頁</span><span class="sxs-lookup"><span data-stu-id="ac25f-162">`NextPrevious` shows a Next and Previous buttons, allowing the user to step forwards or backwards one page at a time</span></span>
    - <span data-ttu-id="ac25f-163">`NextPreviousFirstLast`除了下一步 和 上一步 按鈕，第一個和最後一個 按鈕也會包含，讓使用者快速移至第一個或最後一頁的資料</span><span class="sxs-lookup"><span data-stu-id="ac25f-163">`NextPreviousFirstLast` in addition to Next and Previous buttons, First and Last buttons are also included, allowing the user to quickly move to the first or last page of data</span></span>
    - <span data-ttu-id="ac25f-164">`Numeric`顯示一系列的頁碼，讓使用者立即跳至任何頁面</span><span class="sxs-lookup"><span data-stu-id="ac25f-164">`Numeric` shows a series of page numbers, allowing the user to immediately jump to any page</span></span>
    - <span data-ttu-id="ac25f-165">`NumericFirstLast`列印的頁碼，除了包含 第一個和最後一個按鈕，讓使用者快速移至第一個或最後一頁的資料;第一個/最後一個按鈕只會顯示於所有數字的頁碼不能配合</span><span class="sxs-lookup"><span data-stu-id="ac25f-165">`NumericFirstLast` in addition to the page numbers, includes First and Last buttons, allowing the user to quickly move to the first or last page of data; the First/Last buttons are only shown if all of the numeric page numbers cannot fit</span></span>

<span data-ttu-id="ac25f-166">此外，GridView、 DetailsView 和所有提供的 FormView`PageIndex`和`PageCount`屬性，分別表示目前正在檢視的頁面和資料的總頁數。</span><span class="sxs-lookup"><span data-stu-id="ac25f-166">Moreover, the GridView, DetailsView, and FormView all offer the `PageIndex` and `PageCount` properties, which indicate the current page being viewed and the total number of pages of data, respectively.</span></span> <span data-ttu-id="ac25f-167">`PageIndex`屬性具有索引 0，表示檢視資料的第一頁時開始`PageIndex`會等於 0。</span><span class="sxs-lookup"><span data-stu-id="ac25f-167">The `PageIndex` property is indexed starting at 0, meaning that when viewing the first page of data `PageIndex` will equal 0.</span></span> <span data-ttu-id="ac25f-168">`PageCount`相反地，啟動 計數 1，表示`PageIndex`僅限於的值介於 0 和`PageCount - 1`。</span><span class="sxs-lookup"><span data-stu-id="ac25f-168">`PageCount`, on the other hand, starts counting at 1, meaning that `PageIndex` is limited to the values between 0 and `PageCount - 1`.</span></span>

<span data-ttu-id="ac25f-169">可讓 s 花一點時間來改善我們的 GridView 的分頁介面的預設外觀。</span><span class="sxs-lookup"><span data-stu-id="ac25f-169">Let s take a moment to improve the default appearance of our GridView s paging interface.</span></span> <span data-ttu-id="ac25f-170">具體來說，可讓 s 擁有分頁介面靠右對齊，淺灰色背景。</span><span class="sxs-lookup"><span data-stu-id="ac25f-170">Specifically, let s have the paging interface right-aligned with a light gray background.</span></span> <span data-ttu-id="ac25f-171">而不是設定這些屬性直接透過 GridView s`PagerStyle`屬性，建立讓 s 中的 CSS 類別`Styles.css`名為`PagerRowStyle`然後指派`PagerStyle`s`CssClass`透過我們的佈景主題的屬性。</span><span class="sxs-lookup"><span data-stu-id="ac25f-171">Rather than setting these properties directly through the GridView s `PagerStyle` property, let s create a CSS class in `Styles.css` named `PagerRowStyle` and then assign the `PagerStyle` s `CssClass` property through our Theme.</span></span> <span data-ttu-id="ac25f-172">先開啟`Styles.css`並新增下列 CSS 類別定義：</span><span class="sxs-lookup"><span data-stu-id="ac25f-172">Start by opening `Styles.css` and adding the following CSS class definition:</span></span>


[!code-css[Main](paging-and-sorting-report-data-cs/samples/sample3.css)]

<span data-ttu-id="ac25f-173">接下來，開啟`GridView.skin`檔案`DataWebControls`資料夾內`App_Themes`資料夾。</span><span class="sxs-lookup"><span data-stu-id="ac25f-173">Next, open the `GridView.skin` file in the `DataWebControls` folder within the `App_Themes` folder.</span></span> <span data-ttu-id="ac25f-174">如我們所述*主版頁面和站台瀏覽*教學課程，面板檔案可以用來指定 Web 控制項的預設屬性值。</span><span class="sxs-lookup"><span data-stu-id="ac25f-174">As we discussed in the *Master Pages and Site Navigation* tutorial, Skin files can be used to specify the default property values for a Web control.</span></span> <span data-ttu-id="ac25f-175">因此，增強現有的設定，以包含設定`PagerStyle`s`CssClass`屬性`PagerRowStyle`。</span><span class="sxs-lookup"><span data-stu-id="ac25f-175">Therefore, augment the existing settings to include setting the `PagerStyle` s `CssClass` property to `PagerRowStyle`.</span></span> <span data-ttu-id="ac25f-176">此外，讓 s 設定分頁介面，以顯示最多五個數值頁面按鈕使用`NumericFirstLast`分頁介面。</span><span class="sxs-lookup"><span data-stu-id="ac25f-176">Also, let s configure the paging interface to show at most five numeric page buttons using the `NumericFirstLast` paging interface.</span></span>


[!code-aspx[Main](paging-and-sorting-report-data-cs/samples/sample4.aspx)]

## <a name="the-paging-user-experience"></a><span data-ttu-id="ac25f-177">分頁的使用者經驗</span><span class="sxs-lookup"><span data-stu-id="ac25f-177">The Paging User Experience</span></span>

<span data-ttu-id="ac25f-178">圖 8 顯示網頁時已檢查的 GridView s 啟用分頁核取方塊之後，透過瀏覽器瀏覽和`PagerStyle`和`PagerSettings`組態可能已透過`GridView.skin`檔案。</span><span class="sxs-lookup"><span data-stu-id="ac25f-178">Figure 8 shows the web page when visited through a browser after the GridView s Enable Paging checkbox has been checked and the `PagerStyle` and `PagerSettings` configurations have been made through the `GridView.skin` file.</span></span> <span data-ttu-id="ac25f-179">請注意如何只有 10 記錄會顯示，且分頁介面指出我們正在檢視資料的第一頁。</span><span class="sxs-lookup"><span data-stu-id="ac25f-179">Note how only ten records are shown, and the paging interface indicates that we are viewing the first page of data.</span></span>


<span data-ttu-id="ac25f-180">[![以啟用分頁，一次顯示記錄的子集](paging-and-sorting-report-data-cs/_static/image13.png)](paging-and-sorting-report-data-cs/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="ac25f-180">[![With Paging Enabled, Only a Subset of the Records are Displayed at a Time](paging-and-sorting-report-data-cs/_static/image13.png)](paging-and-sorting-report-data-cs/_static/image12.png)</span></span>

<span data-ttu-id="ac25f-181">**圖 8**： 記錄的子集分頁已啟用 會顯示一次 ([按一下以檢視完整大小的影像](paging-and-sorting-report-data-cs/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="ac25f-181">**Figure 8**: With Paging Enabled, Only a Subset of the Records are Displayed at a Time ([Click to view full-size image](paging-and-sorting-report-data-cs/_static/image14.png))</span></span>


<span data-ttu-id="ac25f-182">當使用者按一下其中一個分頁介面中的頁碼時，回傳展示和頁面會重新載入要求頁面的記錄的顯示。</span><span class="sxs-lookup"><span data-stu-id="ac25f-182">When the user clicks on one of the page numbers in the paging interface, a postback ensues and the page reloads showing that requested page s records.</span></span> <span data-ttu-id="ac25f-183">圖 9 顯示選擇以檢視資料的最後一頁之後的結果。</span><span class="sxs-lookup"><span data-stu-id="ac25f-183">Figure 9 shows the results after opting to view the final page of data.</span></span> <span data-ttu-id="ac25f-184">請注意，最後一頁只會有一筆記錄。這是因為有 81 記錄，導致八個頁面，以單獨的記錄在每個頁面，再加上一頁的 10 筆記錄的總計。</span><span class="sxs-lookup"><span data-stu-id="ac25f-184">Notice that the final page only has one record; this is because there are 81 records in total, resulting in eight pages of 10 records per page plus one page with a lone record.</span></span>


<span data-ttu-id="ac25f-185">[![按一下頁碼導致回傳，並顯示記錄的適當子集合](paging-and-sorting-report-data-cs/_static/image16.png)](paging-and-sorting-report-data-cs/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="ac25f-185">[![Clicking On a Page Number Causes a Postback and Shows the Appropriate Subset of Records](paging-and-sorting-report-data-cs/_static/image16.png)](paging-and-sorting-report-data-cs/_static/image15.png)</span></span>

<span data-ttu-id="ac25f-186">**圖 9**： 按一下頁碼導致回傳，並顯示適當的資料錄子集 ([按一下以檢視完整大小的影像](paging-and-sorting-report-data-cs/_static/image17.png))</span><span class="sxs-lookup"><span data-stu-id="ac25f-186">**Figure 9**: Clicking On a Page Number Causes a Postback and Shows the Appropriate Subset of Records ([Click to view full-size image](paging-and-sorting-report-data-cs/_static/image17.png))</span></span>


## <a name="paging-s-server-side-workflow"></a><span data-ttu-id="ac25f-187">分頁的伺服器端工作流程</span><span class="sxs-lookup"><span data-stu-id="ac25f-187">Paging s Server-Side Workflow</span></span>

<span data-ttu-id="ac25f-188">當使用者按一下分頁介面中的按鈕時，回傳展示和下列伺服器端工作流程會開始：</span><span class="sxs-lookup"><span data-stu-id="ac25f-188">When the end user clicks on a button in the paging interface, a postback ensues and the following server-side workflow begins:</span></span>

1. <span data-ttu-id="ac25f-189">GridView s （或 DetailsView 或 FormView）`PageIndexChanging`事件引發</span><span class="sxs-lookup"><span data-stu-id="ac25f-189">The GridView s (or DetailsView or FormView) `PageIndexChanging` event fires</span></span>
2. <span data-ttu-id="ac25f-190">ObjectDataSource 重新要求*所有*BLL; 資料的 GridView s`PageIndex`和`PageSize`屬性值可用來判斷哪些記錄從 BLL 傳回需要在 GridView 中顯示</span><span class="sxs-lookup"><span data-stu-id="ac25f-190">The ObjectDataSource re-requests *all* of the data from the BLL; the GridView s `PageIndex` and `PageSize` property values are used to determine what records returned from the BLL need to be displayed in the GridView</span></span>
3. <span data-ttu-id="ac25f-191">GridView 的`PageIndexChanged`事件引發</span><span class="sxs-lookup"><span data-stu-id="ac25f-191">The GridView s `PageIndexChanged` event fires</span></span>

<span data-ttu-id="ac25f-192">步驟 2 中，在 ObjectDataSource 重新要求所有其資料來源的資料。</span><span class="sxs-lookup"><span data-stu-id="ac25f-192">In Step 2, the ObjectDataSource re-requests all of the data from its data source.</span></span> <span data-ttu-id="ac25f-193">這種樣式的分頁通常稱為*預設分頁*，因為它 s 分頁行為時預設使用的設定`AllowPaging`屬性`true`。</span><span class="sxs-lookup"><span data-stu-id="ac25f-193">This style of paging is commonly referred to as *default paging*, as it s the paging behavior used by default when setting the `AllowPaging` property to `true`.</span></span> <span data-ttu-id="ac25f-194">預設值這個分頁 Web 控制項的資料擷取所有記錄每一頁的資料，即使記錄的子集實際會轉譯成 HTML s 傳送到瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="ac25f-194">With default paging the data Web control naively retrieves all records for each page of data, even though only a subset of records are actually rendered into the HTML that s sent to the browser.</span></span> <span data-ttu-id="ac25f-195">資料庫資料會由 BLL 或 ObjectDataSource 快取，除非預設分頁是夠大的結果集或具有許多並行使用者的 web 應用程式處於無法運作。</span><span class="sxs-lookup"><span data-stu-id="ac25f-195">Unless the database data is cached by the BLL or ObjectDataSource, default paging is unworkable for sufficiently large result sets or for web applications with many concurrent users.</span></span>

<span data-ttu-id="ac25f-196">在下一個教學課程中，我們將檢驗如何實作*自訂分頁*。</span><span class="sxs-lookup"><span data-stu-id="ac25f-196">In the next tutorial we'll examine how to implement *custom paging*.</span></span> <span data-ttu-id="ac25f-197">使用自訂分頁時，您可以特別指示 ObjectDataSource 來只擷取精確的記錄所需的資料要求的頁面集合。</span><span class="sxs-lookup"><span data-stu-id="ac25f-197">With custom paging you can specifically instruct the ObjectDataSource to only retrieve the precise set of records needed for the requested page of data.</span></span> <span data-ttu-id="ac25f-198">您可以想像，自訂分頁可大幅提升效率的大型結果集進行分頁。</span><span class="sxs-lookup"><span data-stu-id="ac25f-198">As you can imagine, custom paging greatly improves the efficiency of paging through large result sets.</span></span>

> [!NOTE]
> <span data-ttu-id="ac25f-199">雖然有許多同時使用者分頁夠大的結果集，或站台時，就不適當預設分頁，了解自訂分頁需要更多的變更和投入時間來實作，而且不只要選取核取方塊，（因為是預設值分頁）。</span><span class="sxs-lookup"><span data-stu-id="ac25f-199">While default paging is not suitable when paging through sufficiently large result sets or for sites with many simultaneous users, realize that custom paging requires more changes and effort to implement and is not as simple as checking a checkbox (as is default paging).</span></span> <span data-ttu-id="ac25f-200">因此，預設分頁可能小型、 低流量網站或相對較小的結果進行分頁的設定時，因為它的理想選擇 s 更輕鬆且快速實作。</span><span class="sxs-lookup"><span data-stu-id="ac25f-200">Therefore, default paging may be the ideal choice for small, low-traffic websites or when paging through relatively small result sets, as it s much easier and quicker to implement.</span></span>


<span data-ttu-id="ac25f-201">例如，如果我們知道我們永遠不會有超過 100 個產品在資料庫中，自訂分頁所用的最少的效能改善由實作所需的工作可能位移。</span><span class="sxs-lookup"><span data-stu-id="ac25f-201">For example, if we know that we'll never have more than 100 products in our database, the minimal performance gain enjoyed by custom paging is likely offset by the effort required to implement it.</span></span> <span data-ttu-id="ac25f-202">不過，我們可能一天有數千、 數萬數千張的產品，如果*不*實作自訂分頁會大幅阻礙我們的應用程式的延展性。</span><span class="sxs-lookup"><span data-stu-id="ac25f-202">If, however, we may one day have thousands or tens of thousands of products, *not* implementing custom paging would greatly hamper the scalability of our application.</span></span>

## <a name="step-4-customizing-the-paging-experience"></a><span data-ttu-id="ac25f-203">步驟 4： 自訂分頁體驗</span><span class="sxs-lookup"><span data-stu-id="ac25f-203">Step 4: Customizing the Paging Experience</span></span>

<span data-ttu-id="ac25f-204">Web 控制項的資料提供許多可用來加強使用者的分頁體驗的屬性。</span><span class="sxs-lookup"><span data-stu-id="ac25f-204">The data Web controls provide a number of properties that can be used to enhance the user s paging experience.</span></span> <span data-ttu-id="ac25f-205">`PageCount`屬性，例如，指出有多少總頁數，雖然`PageIndex`屬性指出目前正在瀏覽的網頁，而且可以快速移動至特定頁面的 使用者設定。</span><span class="sxs-lookup"><span data-stu-id="ac25f-205">The `PageCount` property, for example, indicates how many total pages there are, while the `PageIndex` property indicates the current page being visited and can be set to quickly move a user to a specific page.</span></span> <span data-ttu-id="ac25f-206">若要說明如何使用這些屬性來改善使用者的分頁體驗，讓 s 將標籤加入 Web 控制項加入我們會通知使用者哪一頁的頁面它們 re 目前造訪以及 DropDownList 控制項使其能快速跳至指定的頁面.</span><span class="sxs-lookup"><span data-stu-id="ac25f-206">To illustrate how to use these properties to improve upon the user s paging experience, let s add a Label Web control to our page that informs the user what page they re currently visiting, along with a DropDownList control that allows them to quickly jump to any given page.</span></span>

<span data-ttu-id="ac25f-207">首先，將標籤 Web 控制項加入至您的頁面，設定其`ID`屬性`PagingInformation`，並以清除其`Text`屬性。</span><span class="sxs-lookup"><span data-stu-id="ac25f-207">First, add a Label Web control to your page, set its `ID` property to `PagingInformation`, and clear out its `Text` property.</span></span> <span data-ttu-id="ac25f-208">接下來，建立事件處理常式 GridView s`DataBound`事件並加入下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="ac25f-208">Next, create an event handler for the GridView s `DataBound` event and add the following code:</span></span>


[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample5.cs)]

<span data-ttu-id="ac25f-209">這個事件處理常式指派`PagingInformation`標籤 s`Text`屬性至訊息，通知使用者頁面目前瀏覽`Products.PageIndex + 1`多少的總頁數超出`Products.PageCount`(我們將加入 1 到`Products.PageIndex`屬性因為`PageIndex`具有索引 0 開始)。</span><span class="sxs-lookup"><span data-stu-id="ac25f-209">This event handler assigns the `PagingInformation` Label s `Text` property to a message informing the user the page they are currently visiting `Products.PageIndex + 1` out of how many total pages `Products.PageCount` (we add 1 to the `Products.PageIndex` property because `PageIndex` is indexed starting at 0).</span></span> <span data-ttu-id="ac25f-210">選取指派此標籤 s`Text`屬性中的`DataBound`與事件處理常式`PageIndexChanged`事件處理常式因為`DataBound`就會引發事件每次資料繫結至 GridView 而`PageIndexChanged`事件處理常式只頁面索引變更時引發。</span><span class="sxs-lookup"><span data-stu-id="ac25f-210">I chose the assign this Label s `Text` property in the `DataBound` event handler as opposed to the `PageIndexChanged` event handler because the `DataBound` event fires every time data is bound to the GridView whereas the `PageIndexChanged` event handler only fires when the page index is changed.</span></span> <span data-ttu-id="ac25f-211">當 GridView 一開始是資料繫結的第一頁上造訪`PageIndexChanging`事件規定 t 引發 (而`DataBound`事件沒有)。</span><span class="sxs-lookup"><span data-stu-id="ac25f-211">When the GridView is initially data bound on the first page visit, the `PageIndexChanging` event doesn t fire (whereas the `DataBound` event does).</span></span>

<span data-ttu-id="ac25f-212">與此新增功能，使用者現在會顯示訊息，指出他們造訪哪些頁面與有的資料有多少總頁數。</span><span class="sxs-lookup"><span data-stu-id="ac25f-212">With this addition, the user is now shown a message indicating what page they are visiting and how many total pages of data there are.</span></span>


<span data-ttu-id="ac25f-213">[![會顯示目前的頁碼和總頁數](paging-and-sorting-report-data-cs/_static/image19.png)](paging-and-sorting-report-data-cs/_static/image18.png)</span><span class="sxs-lookup"><span data-stu-id="ac25f-213">[![The Current Page Number and Total Number of Pages are Displayed](paging-and-sorting-report-data-cs/_static/image19.png)](paging-and-sorting-report-data-cs/_static/image18.png)</span></span>

<span data-ttu-id="ac25f-214">**圖 10**： 會顯示目前的頁碼和總頁數 ([按一下以檢視完整大小的影像](paging-and-sorting-report-data-cs/_static/image20.png))</span><span class="sxs-lookup"><span data-stu-id="ac25f-214">**Figure 10**: The Current Page Number and Total Number of Pages are Displayed ([Click to view full-size image](paging-and-sorting-report-data-cs/_static/image20.png))</span></span>


<span data-ttu-id="ac25f-215">除了 Label 控制項，可讓 s 也加入 DropDownList 控制項，其中列出與目前檢視的頁面，選取在 GridView 中的頁碼。</span><span class="sxs-lookup"><span data-stu-id="ac25f-215">In addition to the Label control, let s also add a DropDownList control that lists the page numbers in the GridView with the currently viewed page selected.</span></span> <span data-ttu-id="ac25f-216">這裡的概念是，使用者可以快速跳從目前網頁到另一個，只要從 DropDownList 選取新的頁面索引。</span><span class="sxs-lookup"><span data-stu-id="ac25f-216">The idea here is that the user can quickly jump from the current page to another by simply selecting the new page index from the DropDownList.</span></span> <span data-ttu-id="ac25f-217">啟動者 DropDownList 加入設計工具中，設定其`ID`屬性`PageList`並檢查其智慧標籤的 啟用 AutoPostBack 選項。</span><span class="sxs-lookup"><span data-stu-id="ac25f-217">Start by adding a DropDownList to the Designer, setting its `ID` property to `PageList` and checking the Enable AutoPostBack option from its smart tag.</span></span>

<span data-ttu-id="ac25f-218">接下來，返回`DataBound`事件處理常式並加入下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="ac25f-218">Next, return to the `DataBound` event handler and add the following code:</span></span>


[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample6.cs)]

<span data-ttu-id="ac25f-219">此程式碼一開始會清除中的項目`PageList`DropDownList。</span><span class="sxs-lookup"><span data-stu-id="ac25f-219">This code begins by clearing out the items in the `PageList` DropDownList.</span></span> <span data-ttu-id="ac25f-220">這似乎是多餘的因為一個不 t 預期的頁數，若要變更，但其他使用者可能會同時使用系統、 新增或移除記錄`Products`資料表。</span><span class="sxs-lookup"><span data-stu-id="ac25f-220">This may seem superfluous, since one wouldn t expect the number of pages to change, but other users may be using the system simultaneously, adding or removing records from the `Products` table.</span></span> <span data-ttu-id="ac25f-221">這類插入或刪除無法改變資料的頁數。</span><span class="sxs-lookup"><span data-stu-id="ac25f-221">Such insertions or deletions could alter the number of pages of data.</span></span>

<span data-ttu-id="ac25f-222">接下來，我們需要重新建立頁碼有一個對應至目前的 GridView`PageIndex`依預設已選取。</span><span class="sxs-lookup"><span data-stu-id="ac25f-222">Next, we need to create the page numbers again and have the one that maps to the current GridView `PageIndex` selected by default.</span></span> <span data-ttu-id="ac25f-223">我們完成這項作業以及從 0 到迴圈`PageCount - 1`，加入新`ListItem`在每個反覆項目和設定其`Selected`屬性設定為 true，如果目前的反覆項目索引等於 GridView 的`PageIndex`屬性。</span><span class="sxs-lookup"><span data-stu-id="ac25f-223">We accomplish this with a loop from 0 to `PageCount - 1`, adding a new `ListItem` in each iteration and setting its `Selected` property to true if the current iteration index equals the GridView s `PageIndex` property.</span></span>

<span data-ttu-id="ac25f-224">最後，我們需要建立事件處理常式 DropDownList s`SelectedIndexChanged`引發的事件，每當使用者選取不同的項目從清單中。</span><span class="sxs-lookup"><span data-stu-id="ac25f-224">Finally, we need to create an event handler for the DropDownList s `SelectedIndexChanged` event, which fires each time the user pick a different item from the list.</span></span> <span data-ttu-id="ac25f-225">若要建立此事件處理常式，只需按兩下 DropDownList 在設計師中，然後加入下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="ac25f-225">To create this event handler, simply double-click the DropDownList in the Designer, then add the following code:</span></span>


[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample7.cs)]

<span data-ttu-id="ac25f-226">圖 11 顯示，因為只要變更 GridView 的`PageIndex`屬性會導致重新繫結至 GridView 的資料。</span><span class="sxs-lookup"><span data-stu-id="ac25f-226">As Figure 11 shows, merely changing the GridView s `PageIndex` property causes the data to be rebound to the GridView.</span></span> <span data-ttu-id="ac25f-227">在 GridView s`DataBound`事件處理常式，適當的 DropDownList`ListItem`已選取。</span><span class="sxs-lookup"><span data-stu-id="ac25f-227">In the GridView s `DataBound` event handler, the appropriate DropDownList `ListItem` is selected.</span></span>


<span data-ttu-id="ac25f-228">[![使用者會自動前往第六個頁面時選取 頁面 6 下拉式清單項目](paging-and-sorting-report-data-cs/_static/image22.png)](paging-and-sorting-report-data-cs/_static/image21.png)</span><span class="sxs-lookup"><span data-stu-id="ac25f-228">[![The User is Automatically Taken to the Sixth Page When Selecting the Page 6 Drop-Down List Item](paging-and-sorting-report-data-cs/_static/image22.png)](paging-and-sorting-report-data-cs/_static/image21.png)</span></span>

<span data-ttu-id="ac25f-229">**圖 11**: 使用者會自動前往第六個頁面時選取 頁面 6 下拉式清單項目 ([按一下以檢視完整大小的影像](paging-and-sorting-report-data-cs/_static/image23.png))</span><span class="sxs-lookup"><span data-stu-id="ac25f-229">**Figure 11**: The User is Automatically Taken to the Sixth Page When Selecting the Page 6 Drop-Down List Item ([Click to view full-size image](paging-and-sorting-report-data-cs/_static/image23.png))</span></span>


## <a name="step-5-adding-bi-directional-sorting-support"></a><span data-ttu-id="ac25f-230">步驟 5： 加入雙向排序支援</span><span class="sxs-lookup"><span data-stu-id="ac25f-230">Step 5: Adding Bi-Directional Sorting Support</span></span>

<span data-ttu-id="ac25f-231">加入雙向排序支援很簡單，只新增分頁支援只會檢查 GridView s 智慧標籤的 啟用排序選項 (它會設定 GridView s [ `AllowSorting`屬性](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.gridview.allowsorting.aspx)至`true`)。</span><span class="sxs-lookup"><span data-stu-id="ac25f-231">Adding bi-directional sorting support is as simple as adding paging support simply check the Enable Sorting option from the GridView s smart tag (which sets the GridView s [`AllowSorting` property](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.gridview.allowsorting.aspx) to `true`).</span></span> <span data-ttu-id="ac25f-232">顯示如下的 GridView 的欄位標頭的每個 LinkButtons，按下時、 導致回傳，將資料依按資料行，以遞增順序排序。</span><span class="sxs-lookup"><span data-stu-id="ac25f-232">This renders each of the headers of the GridView s fields as LinkButtons that, when clicked, cause a postback and return the data sorted by the clicked column in ascending order.</span></span> <span data-ttu-id="ac25f-233">再次按一下相同的標頭 LinkButton 重新排序的資料，以遞減的順序。</span><span class="sxs-lookup"><span data-stu-id="ac25f-233">Clicking the same header LinkButton again re-sorts the data in descending order.</span></span>

> [!NOTE]
> <span data-ttu-id="ac25f-234">如果您使用自訂的資料存取層，而不是型別資料集，您可能沒有啟用排序選項的 GridView s 智慧標籤。</span><span class="sxs-lookup"><span data-stu-id="ac25f-234">If you are using a custom Data Access Layer rather than a Typed DataSet, you may not have an Enable Sorting option in the GridView s smart tag.</span></span> <span data-ttu-id="ac25f-235">繫結至原生支援排序的資料來源的 GridViews 有使用此核取方塊。</span><span class="sxs-lookup"><span data-stu-id="ac25f-235">Only GridViews bound to data sources that natively support sorting have this checkbox available.</span></span> <span data-ttu-id="ac25f-236">型別資料集提供的方塊外排序支援，因為 ADO.NET DataTable 提供`Sort`方法，叫用時，排序資料表 s Datarow 使用指定的準則。</span><span class="sxs-lookup"><span data-stu-id="ac25f-236">The Typed DataSet provides out-of-the-box sorting support since the ADO.NET DataTable provides a `Sort` method that, when invoked, sorts the DataTable s DataRows using the criteria specified.</span></span>


<span data-ttu-id="ac25f-237">如果您的 DAL dal 不會傳回原生支援排序您必須設定排序資訊傳遞到商務邏輯層，可以排序資料，或具有資料 ObjectDataSource 排序的物件。</span><span class="sxs-lookup"><span data-stu-id="ac25f-237">If your DAL does not return objects that natively support sorting you will need to configure the ObjectDataSource to pass sorting information to the Business Logic Layer, which can either sort the data or have the data sorted by the DAL.</span></span> <span data-ttu-id="ac25f-238">我們將探討如何排序資料，在商務邏輯和資料存取層在未來的教學課程。</span><span class="sxs-lookup"><span data-stu-id="ac25f-238">We'll explore how to sort data at the Business Logic and Data Access Layers in a future tutorial.</span></span>

<span data-ttu-id="ac25f-239">排序 LinkButtons 會轉譯為 HTML 的超連結，其目前的色彩 （未瀏覽的連結和瀏覽連結為深紅色藍色） 相衝突的標頭資料列的背景色彩。</span><span class="sxs-lookup"><span data-stu-id="ac25f-239">The sorting LinkButtons are rendered as HTML hyperlinks, whose current colors (blue for an unvisited link and a dark red for a visited link) clash with the background color of the header row.</span></span> <span data-ttu-id="ac25f-240">O d e s 相反地，具有所有標頭資料列連結以白色，無論它們 ve 已瀏覽或不。</span><span class="sxs-lookup"><span data-stu-id="ac25f-240">Instead, let s have all header row links displayed in white, regardless of whether they ve been visited or not.</span></span> <span data-ttu-id="ac25f-241">這可以透過加入下列命令以`Styles.css`類別：</span><span class="sxs-lookup"><span data-stu-id="ac25f-241">This can be accomplished by adding the following to the `Styles.css` class:</span></span>


[!code-css[Main](paging-and-sorting-report-data-cs/samples/sample8.css)]

<span data-ttu-id="ac25f-242">這個語法表示要顯示這些項目使用 HeaderStyle 類別內的超連結時，使用白色文字。</span><span class="sxs-lookup"><span data-stu-id="ac25f-242">This syntax indicates to use white text when displaying those hyperlinks within an element that uses the HeaderStyle class.</span></span>

<span data-ttu-id="ac25f-243">之後此 CSS 新增功能，當瀏覽透過瀏覽器頁面螢幕看起來應該類似於圖 12。</span><span class="sxs-lookup"><span data-stu-id="ac25f-243">After this CSS addition, when visiting the page through a browser your screen should look similar to Figure 12.</span></span> <span data-ttu-id="ac25f-244">已按下價格欄位 s 標頭連結之後，特別是，圖 12 顯示結果。</span><span class="sxs-lookup"><span data-stu-id="ac25f-244">In particular, Figure 12 shows the results after the Price field s header link has been clicked.</span></span>


<span data-ttu-id="ac25f-245">[![以遞增順序單價已經排序結果](paging-and-sorting-report-data-cs/_static/image25.png)](paging-and-sorting-report-data-cs/_static/image24.png)</span><span class="sxs-lookup"><span data-stu-id="ac25f-245">[![The Results Have Been Sorted by the UnitPrice in Ascending Order](paging-and-sorting-report-data-cs/_static/image25.png)](paging-and-sorting-report-data-cs/_static/image24.png)</span></span>

<span data-ttu-id="ac25f-246">**圖 12**: 結果有已依照依遞增順序 UnitPrice ([按一下以檢視完整大小的影像](paging-and-sorting-report-data-cs/_static/image26.png))</span><span class="sxs-lookup"><span data-stu-id="ac25f-246">**Figure 12**: The Results Have Been Sorted by the UnitPrice in Ascending Order ([Click to view full-size image](paging-and-sorting-report-data-cs/_static/image26.png))</span></span>


## <a name="examining-the-sorting-workflow"></a><span data-ttu-id="ac25f-247">檢查 排序的工作流程</span><span class="sxs-lookup"><span data-stu-id="ac25f-247">Examining the Sorting Workflow</span></span>

<span data-ttu-id="ac25f-248">所有的 GridView 欄位 BoundField CheckBoxField、 TemplateField，而且等有`SortExpression`屬性，指出應該用來排序資料，在按下欄位 s 排序標頭連結時的運算式。</span><span class="sxs-lookup"><span data-stu-id="ac25f-248">All GridView fields the BoundField, CheckBoxField, TemplateField, and so on have a `SortExpression` property that indicates the expression that should be used to sort the data when that field s sorting header link is clicked.</span></span> <span data-ttu-id="ac25f-249">在 GridView 還有`SortExpression`屬性。</span><span class="sxs-lookup"><span data-stu-id="ac25f-249">The GridView also has a `SortExpression` property.</span></span> <span data-ttu-id="ac25f-250">在 GridView 時排序的標頭，LinkButton 已按下，指派欄位 s`SortExpression`值設定為其`SortExpression`屬性。</span><span class="sxs-lookup"><span data-stu-id="ac25f-250">When a sorting header LinkButton is clicked, the GridView assigns that field s `SortExpression` value to its `SortExpression` property.</span></span> <span data-ttu-id="ac25f-251">接下來，資料是從 ObjectDataSource 重新擷取，而且排序 GridView s 根據`SortExpression`屬性。</span><span class="sxs-lookup"><span data-stu-id="ac25f-251">Next, the data is re-retrieved from the ObjectDataSource and sorted according to the GridView s `SortExpression` property.</span></span> <span data-ttu-id="ac25f-252">下列清單詳細說明步驟的順序，顯示瓿當使用者排序 GridView 中的資料：</span><span class="sxs-lookup"><span data-stu-id="ac25f-252">The following list details the sequence of steps that transpires when an end user sorts the data in a GridView:</span></span>

1. <span data-ttu-id="ac25f-253">GridView s [Sorting 事件](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.sorting(VS.80).aspx)引發</span><span class="sxs-lookup"><span data-stu-id="ac25f-253">The GridView s [Sorting event](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.sorting(VS.80).aspx) fires</span></span>
2. <span data-ttu-id="ac25f-254">GridView s [ `SortExpression`屬性](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.sortexpression.aspx)設`SortExpression`欄位的 LinkButton 已按下其排序的標頭</span><span class="sxs-lookup"><span data-stu-id="ac25f-254">The GridView s [`SortExpression` property](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.sortexpression.aspx) is set to the `SortExpression` of the field whose sorting header LinkButton was clicked</span></span>
3. <span data-ttu-id="ac25f-255">ObjectDataSource 重新擷取的所有資料從 BLL，，然後排序的資料，使用 GridView s`SortExpression`</span><span class="sxs-lookup"><span data-stu-id="ac25f-255">The ObjectDataSource re-retrieves all of the data from the BLL and then sorts the data using the GridView s `SortExpression`</span></span>
4. <span data-ttu-id="ac25f-256">GridView 的`PageIndex`屬性重設為 0，表示排序使用者時傳回第一頁的資料 （假設已實作的分頁支援）</span><span class="sxs-lookup"><span data-stu-id="ac25f-256">The GridView s `PageIndex` property is reset to 0, meaning that when sorting the user is returned to the first page of data (assuming paging support has been implemented)</span></span>
5. <span data-ttu-id="ac25f-257">GridView s [ `Sorted`事件](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.sorted(VS.80).aspx)引發</span><span class="sxs-lookup"><span data-stu-id="ac25f-257">The GridView s [`Sorted` event](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.sorted(VS.80).aspx) fires</span></span>

<span data-ttu-id="ac25f-258">使用預設分頁時，預設的排序選項重新擷取像*所有*記錄 BLL 的。</span><span class="sxs-lookup"><span data-stu-id="ac25f-258">Like with default paging, the default sorting option re-retrieves *all* of the records from the BLL.</span></span> <span data-ttu-id="ac25f-259">使用分頁沒有排序時或在使用含有排序預設分頁時，那里 s 沒有方法可以避免這個效能影響 （除非快取的資料庫資料）。</span><span class="sxs-lookup"><span data-stu-id="ac25f-259">When using sorting without paging or when using sorting with default paging, there s no way to circumvent this performance hit (short of caching the database data).</span></span> <span data-ttu-id="ac25f-260">不過，我們會看到在未來的教學課程中，如它 s 能夠有效率地使用自訂分頁時，排序資料。</span><span class="sxs-lookup"><span data-stu-id="ac25f-260">However, as we'll see in a future tutorial, it s possible to efficiently sort data when using custom paging.</span></span>

<span data-ttu-id="ac25f-261">每個 GridView 欄位時 ObjectDataSource 繫結至 GridView 透過下拉式清單中的清單 GridView s 智慧標籤會自動擁有其`SortExpression`指派中的資料欄位的名稱屬性`ProductsRow`類別。</span><span class="sxs-lookup"><span data-stu-id="ac25f-261">When binding an ObjectDataSource to the GridView through the drop-down list in the GridView s smart tag, each GridView field automatically has its `SortExpression` property assigned to the name of the data field in the `ProductsRow` class.</span></span> <span data-ttu-id="ac25f-262">例如， `ProductName` BoundField s`SortExpression`設`ProductName`，如下列宣告式標記中所示：</span><span class="sxs-lookup"><span data-stu-id="ac25f-262">For example, the `ProductName` BoundField s `SortExpression` is set to `ProductName`, as shown in the following declarative markup:</span></span>


[!code-aspx[Main](paging-and-sorting-report-data-cs/samples/sample9.aspx)]

<span data-ttu-id="ac25f-263">您可以設定欄位，讓它 s 不可清除排序其`SortExpression`（將其指派給空字串） 的屬性。</span><span class="sxs-lookup"><span data-stu-id="ac25f-263">A field can be configured so that it s not sortable by clearing out its `SortExpression` property (assigning it to an empty string).</span></span> <span data-ttu-id="ac25f-264">為了說明這點，假設我們 professionals t 想要讓客戶排序我們的產品價格。</span><span class="sxs-lookup"><span data-stu-id="ac25f-264">To illustrate this, imagine that we didn t want to let our customers sort our products by price.</span></span> <span data-ttu-id="ac25f-265">`UnitPrice` BoundField 的`SortExpression`從宣告式標記，或透過 欄位 對話方塊 （也就是可存取，即可編輯資料行中的連結 GridView s 智慧標籤），就可以移除屬性。</span><span class="sxs-lookup"><span data-stu-id="ac25f-265">The `UnitPrice` BoundField s `SortExpression` property can be removed either from the declarative markup or through the Fields dialog box (which is accessible by clicking on the Edit Columns link in the GridView s smart tag).</span></span>


![以遞增順序單價已經排序結果](paging-and-sorting-report-data-cs/_static/image27.png)

<span data-ttu-id="ac25f-267">**圖 13**： 已經單價以遞增順序排序結果</span><span class="sxs-lookup"><span data-stu-id="ac25f-267">**Figure 13**: The Results Have Been Sorted by the UnitPrice in Ascending Order</span></span>


<span data-ttu-id="ac25f-268">一次`SortExpression`已移除屬性，以`UnitPrice`BoundField，頁首會轉譯為文字而不是連結，藉此防止使用者排序資料的價格。</span><span class="sxs-lookup"><span data-stu-id="ac25f-268">Once the `SortExpression` property has been removed for the `UnitPrice` BoundField, the header is rendered as text rather than as a link, thereby preventing users from sorting the data by price.</span></span>


<span data-ttu-id="ac25f-269">[![藉由移除 SortExpression 屬性，使用者不再可以排序產品的價格](paging-and-sorting-report-data-cs/_static/image29.png)](paging-and-sorting-report-data-cs/_static/image28.png)</span><span class="sxs-lookup"><span data-stu-id="ac25f-269">[![By Removing the SortExpression Property, Users Can No Longer Sort the Products By Price](paging-and-sorting-report-data-cs/_static/image29.png)](paging-and-sorting-report-data-cs/_static/image28.png)</span></span>

<span data-ttu-id="ac25f-270">**圖 14**： 藉由移除 SortExpression 屬性，使用者不再可以排序產品的價格 ([按一下以檢視完整大小的影像](paging-and-sorting-report-data-cs/_static/image30.png))</span><span class="sxs-lookup"><span data-stu-id="ac25f-270">**Figure 14**: By Removing the SortExpression Property, Users Can No Longer Sort the Products By Price ([Click to view full-size image](paging-and-sorting-report-data-cs/_static/image30.png))</span></span>


## <a name="programmatically-sorting-the-gridview"></a><span data-ttu-id="ac25f-271">以程式設計的方式排序 GridView</span><span class="sxs-lookup"><span data-stu-id="ac25f-271">Programmatically Sorting the GridView</span></span>

<span data-ttu-id="ac25f-272">您也可以排序 GridView 內容以程式設計方式使用 GridView s [ `Sort`方法](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.sort.aspx)。</span><span class="sxs-lookup"><span data-stu-id="ac25f-272">You can also sort the contents of the GridView programmatically by using the GridView s [`Sort` method](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.sort.aspx).</span></span> <span data-ttu-id="ac25f-273">只傳入`SortExpression`排序所依據連同值[ `SortDirection` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.sortdirection.aspx) (`Ascending`或`Descending`)，而且會重新排序 GridView 的資料。</span><span class="sxs-lookup"><span data-stu-id="ac25f-273">Simply pass in the `SortExpression` value to sort by along with the [`SortDirection`](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.sortdirection.aspx) (`Ascending` or `Descending`), and the GridView s data will be re-sorted.</span></span>

<span data-ttu-id="ac25f-274">想像一下，我們關閉排序原因`UnitPrice`因為我們擔心我們的客戶會只是購買只低價產品。</span><span class="sxs-lookup"><span data-stu-id="ac25f-274">Imagine that the reason we turned off sorting by the `UnitPrice` was because we were worried that our customers would simply buy only the lowest-priced products.</span></span> <span data-ttu-id="ac25f-275">不過，我們想要建議他們購買成本最高的產品，因此，我們 d 希望他們能夠排序到最少的產品價格，但只能從成本最高價格。</span><span class="sxs-lookup"><span data-stu-id="ac25f-275">However, we want to encourage them to buy the most expensive products, so we d like them to be able to sort the products by price, but only from the most expensive price to the least.</span></span>

<span data-ttu-id="ac25f-276">若要完成此按鈕 Web 控制項加入頁面上，設定其`ID`屬性`SortPriceDescending`，及其`Text`價格依排序的屬性。</span><span class="sxs-lookup"><span data-stu-id="ac25f-276">To accomplish this add a Button Web control to the page, set its `ID` property to `SortPriceDescending`, and its `Text` property to Sort by Price.</span></span> <span data-ttu-id="ac25f-277">接下來，建立事件處理常式按鈕 s`Click`按兩下按鈕控制項設計工具中的事件。</span><span class="sxs-lookup"><span data-stu-id="ac25f-277">Next, create an event handler for the Button s `Click` event by double-clicking the Button control in the Designer.</span></span> <span data-ttu-id="ac25f-278">這個事件處理常式中加入下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="ac25f-278">Add the following code to this event handler:</span></span>


[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample10.cs)]

<span data-ttu-id="ac25f-279">按一下此按鈕傳回使用者第一頁的價格，從最便宜 （請參閱圖 15） 成本最高依排序產品。</span><span class="sxs-lookup"><span data-stu-id="ac25f-279">Clicking this Button returns the user to the first page with the products sorted by price, from most expensive to least expensive (see Figure 15).</span></span>


<span data-ttu-id="ac25f-280">[![按一下按鈕訂單的產品成本最高到最低](paging-and-sorting-report-data-cs/_static/image32.png)](paging-and-sorting-report-data-cs/_static/image31.png)</span><span class="sxs-lookup"><span data-stu-id="ac25f-280">[![Clicking the Button Orders the Products From the Most Expensive to the Least](paging-and-sorting-report-data-cs/_static/image32.png)](paging-and-sorting-report-data-cs/_static/image31.png)</span></span>

<span data-ttu-id="ac25f-281">**圖 15**： 按一下按鈕的訂單產品從成本最高到最低 ([按一下以檢視完整大小的影像](paging-and-sorting-report-data-cs/_static/image33.png))</span><span class="sxs-lookup"><span data-stu-id="ac25f-281">**Figure 15**: Clicking the Button Orders the Products From the Most Expensive to the Least ([Click to view full-size image](paging-and-sorting-report-data-cs/_static/image33.png))</span></span>


## <a name="summary"></a><span data-ttu-id="ac25f-282">總結</span><span class="sxs-lookup"><span data-stu-id="ac25f-282">Summary</span></span>

<span data-ttu-id="ac25f-283">在本教學課程，我們可了解如何實作分頁和排序功能的預設值，這兩者都是簡單，只要選取核取方塊 ！</span><span class="sxs-lookup"><span data-stu-id="ac25f-283">In this tutorial we saw how to implement default paging and sorting capabilities, both of which were as easy as checking a checkbox!</span></span> <span data-ttu-id="ac25f-284">當使用者排序或資料頁時，可以掀起類似的工作流程：</span><span class="sxs-lookup"><span data-stu-id="ac25f-284">When a user sorts or pages through data, a similar workflow unfolds:</span></span>

1. <span data-ttu-id="ac25f-285">回傳展示</span><span class="sxs-lookup"><span data-stu-id="ac25f-285">A postback ensues</span></span>
2. <span data-ttu-id="ac25f-286">資料 Web 控制項 s 預先層級事件引發 (`PageIndexChanging`或`Sorting`)</span><span class="sxs-lookup"><span data-stu-id="ac25f-286">The data Web control s pre-level event fires (`PageIndexChanging` or `Sorting`)</span></span>
3. <span data-ttu-id="ac25f-287">ObjectDataSource 重新擷取的所有資料</span><span class="sxs-lookup"><span data-stu-id="ac25f-287">All of the data is re-retrieved by the ObjectDataSource</span></span>
4. <span data-ttu-id="ac25f-288">後置資料 Web 控制項 s 層級的事件引發 (`PageIndexChanged`或`Sorted`)</span><span class="sxs-lookup"><span data-stu-id="ac25f-288">The data Web control s post-level event fires (`PageIndexChanged` or `Sorted`)</span></span>

<span data-ttu-id="ac25f-289">雖然實作基本的分頁和排序更是輕而易舉，利用更有效率的自訂分頁，或進一步加強分頁或排序介面必須諸較多工作。</span><span class="sxs-lookup"><span data-stu-id="ac25f-289">While implementing basic paging and sorting is a breeze, more effort must be exerted to utilize the more efficient custom paging or to further enhance the paging or sorting interface.</span></span> <span data-ttu-id="ac25f-290">未來的教學課程將會探索這些主題。</span><span class="sxs-lookup"><span data-stu-id="ac25f-290">Future tutorials will explore these topics.</span></span>

<span data-ttu-id="ac25f-291">祝您程式設計 ！</span><span class="sxs-lookup"><span data-stu-id="ac25f-291">Happy Programming!</span></span>

## <a name="about-the-author"></a><span data-ttu-id="ac25f-292">關於作者</span><span class="sxs-lookup"><span data-stu-id="ac25f-292">About the Author</span></span>

<span data-ttu-id="ac25f-293">[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。</span><span class="sxs-lookup"><span data-stu-id="ac25f-293">[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), author of seven ASP/ASP.NET books and founder of [4GuysFromRolla.com](http://www.4guysfromrolla.com), has been working with Microsoft Web technologies since 1998.</span></span> <span data-ttu-id="ac25f-294">Scott 可做為獨立顧問、 訓練和寫入器。</span><span class="sxs-lookup"><span data-stu-id="ac25f-294">Scott works as an independent consultant, trainer, and writer.</span></span> <span data-ttu-id="ac25f-295">他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。</span><span class="sxs-lookup"><span data-stu-id="ac25f-295">His latest book is [*Sams Teach Yourself ASP.NET 2.0 in 24 Hours*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco).</span></span> <span data-ttu-id="ac25f-296">他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。</span><span class="sxs-lookup"><span data-stu-id="ac25f-296">He can be reached at [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) or via his blog, which can be found at [http://ScottOnWriting.NET](http://ScottOnWriting.NET).</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="ac25f-297">下一步</span><span class="sxs-lookup"><span data-stu-id="ac25f-297">Next</span></span>](efficiently-paging-through-large-amounts-of-data-cs.md)
