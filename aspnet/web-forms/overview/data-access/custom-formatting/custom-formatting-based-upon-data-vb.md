---
uid: web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-vb
title: "自訂格式會根據資料 (VB) |Microsoft 文件"
author: rick-anderson
description: "以多種方式可以完成調整 GridView、 DetailsView 或根據繫結至它的資料在 FormView 的格式。 在本教學課程中，我們會 l..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: df5a1525-386f-4632-972c-57b199870bc3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 4122f55c09f94d41f7caa3f86d1fbd5870417a20
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="custom-formatting-based-upon-data-vb"></a><span data-ttu-id="954d1-104">自訂格式會根據資料 (VB)</span><span class="sxs-lookup"><span data-stu-id="954d1-104">Custom Formatting Based Upon Data (VB)</span></span>
====================
<span data-ttu-id="954d1-105">由[Scott Mitchell](https://twitter.com/ScottOnWriting)</span><span class="sxs-lookup"><span data-stu-id="954d1-105">by [Scott Mitchell](https://twitter.com/ScottOnWriting)</span></span>

<span data-ttu-id="954d1-106">[下載範例應用程式](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_11_VB.exe)或[下載 PDF](custom-formatting-based-upon-data-vb/_static/datatutorial11vb1.pdf)</span><span class="sxs-lookup"><span data-stu-id="954d1-106">[Download Sample App](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_11_VB.exe) or [Download PDF](custom-formatting-based-upon-data-vb/_static/datatutorial11vb1.pdf)</span></span>

> <span data-ttu-id="954d1-107">以多種方式可以完成調整 GridView、 DetailsView 或根據繫結至它的資料在 FormView 的格式。</span><span class="sxs-lookup"><span data-stu-id="954d1-107">Adjusting the format of the GridView, DetailsView, or FormView based upon the data bound to it can be accomplished in multiple ways.</span></span> <span data-ttu-id="954d1-108">在此教學課程中我們將探討如何完成資料繫結透過資料繫結和 RowDataBound 事件處理常式使用的格式。</span><span class="sxs-lookup"><span data-stu-id="954d1-108">In this tutorial we'll look at how to accomplish data bound formatting through the use of the DataBound and RowDataBound event handlers.</span></span>


## <a name="introduction"></a><span data-ttu-id="954d1-109">簡介</span><span class="sxs-lookup"><span data-stu-id="954d1-109">Introduction</span></span>

<span data-ttu-id="954d1-110">透過各種不同的樣式相關屬性，您可以自訂 GridView、 DetailsView 和 FormView 控制項的外觀。</span><span class="sxs-lookup"><span data-stu-id="954d1-110">The appearance of the GridView, DetailsView, and FormView controls can be customized through a myriad of style-related properties.</span></span> <span data-ttu-id="954d1-111">屬性，例如`CssClass`， `Font`， `BorderWidth`， `BorderStyle`， `BorderColor`， `Width`，和`Height`，和其他項目，指定一般呈現的控制項的外觀。</span><span class="sxs-lookup"><span data-stu-id="954d1-111">Properties like `CssClass`, `Font`, `BorderWidth`, `BorderStyle`, `BorderColor`, `Width`, and `Height`, among others, dictate the general appearance of the rendered control.</span></span> <span data-ttu-id="954d1-112">屬性包括`HeaderStyle`， `RowStyle`， `AlternatingRowStyle`，和其他項目允許這些相同的樣式設定来套用至特定區段。</span><span class="sxs-lookup"><span data-stu-id="954d1-112">Properties including `HeaderStyle`, `RowStyle`, `AlternatingRowStyle`, and others allow these same style settings to be applied to particular sections.</span></span> <span data-ttu-id="954d1-113">同樣地，可以在欄位層級套用這些樣式設定。</span><span class="sxs-lookup"><span data-stu-id="954d1-113">Likewise, these style settings can be applied at the field level.</span></span>

<span data-ttu-id="954d1-114">在許多情況下，格式需求取決於所顯示的資料值。</span><span class="sxs-lookup"><span data-stu-id="954d1-114">In many scenarios though, the formatting requirements depend upon the value of the displayed data.</span></span> <span data-ttu-id="954d1-115">例如，若要強調的內建的產品，報告，列出產品資訊可能會設定背景色彩為黃色，這些產品的`UnitsInStock`和`UnitsOnOrder`欄位都等於 0。</span><span class="sxs-lookup"><span data-stu-id="954d1-115">For example, to draw attention to out of stock products, a report listing product information might set the background color to yellow for those products whose `UnitsInStock` and `UnitsOnOrder` fields are both equal to 0.</span></span> <span data-ttu-id="954d1-116">反白顯示的價格的產品，我們可能會想要顯示成本超過 $75.00 以粗體字這些產品的價格。</span><span class="sxs-lookup"><span data-stu-id="954d1-116">To highlight the more expensive products, we may want to display the prices of those products costing more than $75.00 in a bold font.</span></span>

<span data-ttu-id="954d1-117">以多種方式可以完成調整 GridView、 DetailsView 或根據繫結至它的資料在 FormView 的格式。</span><span class="sxs-lookup"><span data-stu-id="954d1-117">Adjusting the format of the GridView, DetailsView, or FormView based upon the data bound to it can be accomplished in multiple ways.</span></span> <span data-ttu-id="954d1-118">在此教學課程中我們會探討如何完成資料繫結透過使用的格式`DataBound`和`RowDataBound`事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="954d1-118">In this tutorial we'll look at how to accomplish data bound formatting through the use of the `DataBound` and `RowDataBound` event handlers.</span></span> <span data-ttu-id="954d1-119">在下一個教學課程中，我們將探討的替代方式。</span><span class="sxs-lookup"><span data-stu-id="954d1-119">In the next tutorial we'll explore an alternative approach.</span></span>

## <a name="using-the-detailsview-controlsdataboundevent-handler"></a><span data-ttu-id="954d1-120">使用 DetailsView 控制項`DataBound`事件處理常式</span><span class="sxs-lookup"><span data-stu-id="954d1-120">Using the DetailsView Control's`DataBound`Event Handler</span></span>

<span data-ttu-id="954d1-121">當資料繫結至 DetailsView，從資料來源控制項，或是透過程式設計方式將資料指派給控制項的`DataSource`屬性，並呼叫其`DataBind()`方法，下列步驟順序發生：</span><span class="sxs-lookup"><span data-stu-id="954d1-121">When data is bound to a DetailsView, either from a data source control or through programmatically assigning data to the control's `DataSource` property and calling its `DataBind()` method, the following sequence of steps occur:</span></span>

1. <span data-ttu-id="954d1-122">資料 Web 控制項`DataBinding`事件引發。</span><span class="sxs-lookup"><span data-stu-id="954d1-122">The data Web control's `DataBinding` event fires.</span></span>
2. <span data-ttu-id="954d1-123">資料繫結至 Web 控制項的資料。</span><span class="sxs-lookup"><span data-stu-id="954d1-123">The data is bound to the data Web control.</span></span>
3. <span data-ttu-id="954d1-124">資料 Web 控制項`DataBound`事件引發。</span><span class="sxs-lookup"><span data-stu-id="954d1-124">The data Web control's `DataBound` event fires.</span></span>

<span data-ttu-id="954d1-125">可透過事件處理常式的步驟 1 和 3 之後立即插入自訂邏輯。</span><span class="sxs-lookup"><span data-stu-id="954d1-125">Custom logic can be injected immediately after steps 1 and 3 through an event handler.</span></span> <span data-ttu-id="954d1-126">藉由建立的事件處理常式`DataBound`我們以程式設計方式判斷已經過的資料繫結至資料 Web 控制項，並調整格式所需的事件。</span><span class="sxs-lookup"><span data-stu-id="954d1-126">By creating an event handler for the `DataBound` event we can programmatically determine the data that has been bound to the data Web control and adjust the formatting as needed.</span></span> <span data-ttu-id="954d1-127">為了說明這點讓我們來建立會列出產品的一般資訊，但將會顯示在 DetailsView`UnitPrice`值***粗體、 斜體字型***如果它超過 $75.00。</span><span class="sxs-lookup"><span data-stu-id="954d1-127">To illustrate this let's create a DetailsView that will list general information about a product, but will display the `UnitPrice` value in a ***bold, italic font*** if it exceeds $75.00.</span></span>

## <a name="step-1-displaying-the-product-information-in-a-detailsview"></a><span data-ttu-id="954d1-128">步驟 1： 在 DetailsView 中顯示的產品資訊。</span><span class="sxs-lookup"><span data-stu-id="954d1-128">Step 1: Displaying the Product Information in a DetailsView</span></span>

<span data-ttu-id="954d1-129">開啟`CustomColors.aspx`頁面`CustomFormatting`資料夾中，將 DetailsView 控制項從工具箱拖曳至設計工具，設定其`ID`屬性值設定為`ExpensiveProductsPriceInBoldItalic`，並將它繫結至新的 ObjectDataSource 控制項叫用`ProductsBLL`類別的`GetProducts()`方法。</span><span class="sxs-lookup"><span data-stu-id="954d1-129">Open the `CustomColors.aspx` page in the `CustomFormatting` folder, drag a DetailsView control from the Toolbox onto the Designer, set its `ID` property value to `ExpensiveProductsPriceInBoldItalic`, and bind it to a new ObjectDataSource control that invokes the `ProductsBLL` class's `GetProducts()` method.</span></span> <span data-ttu-id="954d1-130">為求簡單明瞭，因為我們仔細檢查其先前的教學課程中完成此的詳細的步驟會這裡省略。</span><span class="sxs-lookup"><span data-stu-id="954d1-130">The detailed steps for accomplishing this are omitted here for brevity since we examined them in detail in previous tutorials.</span></span>

<span data-ttu-id="954d1-131">一旦您已經在 detailsview 結合 ObjectDataSource，請花一點時間修改的欄位清單。</span><span class="sxs-lookup"><span data-stu-id="954d1-131">Once you've bound the ObjectDataSource to the DetailsView, take a moment to modify the field list.</span></span> <span data-ttu-id="954d1-132">我已選擇移除`ProductID`， `SupplierID`， `CategoryID`， `UnitsInStock`， `UnitsOnOrder`， `ReorderLevel`，和`Discontinued`BoundFields 和重新命名並重新格式化剩餘 BoundFields。</span><span class="sxs-lookup"><span data-stu-id="954d1-132">I've opted to remove the `ProductID`, `SupplierID`, `CategoryID`, `UnitsInStock`, `UnitsOnOrder`, `ReorderLevel`, and `Discontinued` BoundFields and renamed and reformatted the remaining BoundFields.</span></span> <span data-ttu-id="954d1-133">同時也清除`Width`和`Height`設定。</span><span class="sxs-lookup"><span data-stu-id="954d1-133">I also cleared out the `Width` and `Height` settings.</span></span> <span data-ttu-id="954d1-134">在 DetailsView 會顯示一筆記錄，因為我們需要啟用分頁，以便讓使用者檢視的所有產品。</span><span class="sxs-lookup"><span data-stu-id="954d1-134">Since the DetailsView displays only a single record, we need to enable paging in order to allow the end user to view all of the products.</span></span> <span data-ttu-id="954d1-135">這樣會檢查 DetailsView 的智慧標籤的 啟用分頁核取方塊。</span><span class="sxs-lookup"><span data-stu-id="954d1-135">Do so by checking the Enable Paging checkbox in the DetailsView's smart tag.</span></span>


<span data-ttu-id="954d1-136">[![圖 1： 檢查 DetailsView 的智慧標籤中啟用分頁核取方塊](custom-formatting-based-upon-data-vb/_static/image2.png)](custom-formatting-based-upon-data-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="954d1-136">[![Figure 1: Check the Enable Paging Checkbox in the DetailsView's Smart Tag](custom-formatting-based-upon-data-vb/_static/image2.png)](custom-formatting-based-upon-data-vb/_static/image1.png)</span></span>

<span data-ttu-id="954d1-137">**圖 1**： 圖 1： 檢查啟用分頁中的核取 DetailsView 的智慧標籤 ([按一下以檢視完整大小的影像](custom-formatting-based-upon-data-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="954d1-137">**Figure 1**: Figure 1: Check the Enable Paging Checkbox in the DetailsView's Smart Tag ([Click to view full-size image](custom-formatting-based-upon-data-vb/_static/image3.png))</span></span>


<span data-ttu-id="954d1-138">這些變更之後，請將 DetailsView 標記：</span><span class="sxs-lookup"><span data-stu-id="954d1-138">After these changes, the DetailsView markup will be:</span></span>


[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample1.aspx)]

<span data-ttu-id="954d1-139">請花一點時間來測試您的瀏覽器中的此頁面。</span><span class="sxs-lookup"><span data-stu-id="954d1-139">Take a moment to test out this page in your browser.</span></span>


<span data-ttu-id="954d1-140">[![在 DetailsView 控制項一次顯示一個產品](custom-formatting-based-upon-data-vb/_static/image5.png)](custom-formatting-based-upon-data-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="954d1-140">[![The DetailsView Control Displays One Product at a Time](custom-formatting-based-upon-data-vb/_static/image5.png)](custom-formatting-based-upon-data-vb/_static/image4.png)</span></span>

<span data-ttu-id="954d1-141">**圖 2**: DetailsView 控制項顯示一個產品一次 ([按一下以檢視完整大小的影像](custom-formatting-based-upon-data-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="954d1-141">**Figure 2**: The DetailsView Control Displays One Product at a Time ([Click to view full-size image](custom-formatting-based-upon-data-vb/_static/image6.png))</span></span>


## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a><span data-ttu-id="954d1-142">步驟 2： 以程式設計方式判斷此資料繫結事件處理常式中的資料值</span><span class="sxs-lookup"><span data-stu-id="954d1-142">Step 2: Programmatically Determining the Value of the Data in the DataBound Event Handler</span></span>

<span data-ttu-id="954d1-143">若要顯示的價格以粗體、 斜體字型為這些產品的`UnitPrice`值超過 $75.00，我們必須要先能以程式設計方式判斷`UnitPrice`值。</span><span class="sxs-lookup"><span data-stu-id="954d1-143">In order to display the price in a bold, italic font for those products whose `UnitPrice` value exceeds $75.00, we need to first be able to programmatically determine the `UnitPrice` value.</span></span> <span data-ttu-id="954d1-144">為 DetailsView，這可以透過完成`DataBound`事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="954d1-144">For the DetailsView, this can be accomplished in the `DataBound` event handler.</span></span> <span data-ttu-id="954d1-145">若要建立事件處理常式會 DetailsView 設計工具中按一下，然後瀏覽至 [屬性] 視窗。</span><span class="sxs-lookup"><span data-stu-id="954d1-145">To create the event handler click on the DetailsView in the Designer then navigate to the Properties window.</span></span> <span data-ttu-id="954d1-146">按 F4，即可啟動，如果不是可見的或移至 [檢視] 功能表，然後選取 [屬性] 視窗的功能表選項。</span><span class="sxs-lookup"><span data-stu-id="954d1-146">Press F4 to bring it up, if it's not visible, or go to the View menu and select the Properties Window menu option.</span></span> <span data-ttu-id="954d1-147">屬性 視窗中，按一下 閃電圖示來列出在 DetailsView 的事件。</span><span class="sxs-lookup"><span data-stu-id="954d1-147">From the Properties window, click on the lightning bolt icon to list the DetailsView's events.</span></span> <span data-ttu-id="954d1-148">接下來，按兩下`DataBound`事件或輸入您想要建立此事件處理常式的名稱。</span><span class="sxs-lookup"><span data-stu-id="954d1-148">Next, either double-click the `DataBound` event or type in the name of the event handler you want to create.</span></span>


![資料繫結事件建立事件處理常式](custom-formatting-based-upon-data-vb/_static/image7.png)

<span data-ttu-id="954d1-150">**圖 3**： 建立事件處理常式`DataBound`事件</span><span class="sxs-lookup"><span data-stu-id="954d1-150">**Figure 3**: Create an Event Handler for the `DataBound` Event</span></span>


> [!NOTE]
> <span data-ttu-id="954d1-151">您也可以建立事件處理常式從 ASP.NET 網頁的程式碼部分。</span><span class="sxs-lookup"><span data-stu-id="954d1-151">You can also create an event handler from the ASP.NET page's code portion.</span></span> <span data-ttu-id="954d1-152">那里，您會發現在頁面頂端的兩個下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="954d1-152">There you'll find two drop-down lists at the top of the page.</span></span> <span data-ttu-id="954d1-153">從左邊的下拉式清單中選取的物件，您想要從右邊下拉式清單和 Visual Studio 建立的處理常式的事件將會自動建立適當的事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="954d1-153">Select the object from the left drop-down list and the event you want to create a handler for from the right drop-down list and Visual Studio will automatically create the appropriate event handler.</span></span>


<span data-ttu-id="954d1-154">這樣會自動建立事件處理常式，並帶您前往的程式碼部分加入其中。</span><span class="sxs-lookup"><span data-stu-id="954d1-154">Doing so will automatically create the event handler and take you to the code portion where it has been added.</span></span> <span data-ttu-id="954d1-155">此時您會看到：</span><span class="sxs-lookup"><span data-stu-id="954d1-155">At this point you will see:</span></span>


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample2.vb)]

<span data-ttu-id="954d1-156">在 detailsview 繫結的資料可以透過存取`DataItem`屬性。</span><span class="sxs-lookup"><span data-stu-id="954d1-156">The data bound to the DetailsView can be accessed via the `DataItem` property.</span></span> <span data-ttu-id="954d1-157">提醒您，我們會將我們將控制項繫結至強型別 DataTable，其中強型別 DataRow 執行個體的集合所組成。</span><span class="sxs-lookup"><span data-stu-id="954d1-157">Recall that we are binding our controls to a strongly-typed DataTable, which is composed of a collection of strongly-typed DataRow instances.</span></span> <span data-ttu-id="954d1-158">在 DataTable 中的第一個 DataRow DataTable 繫結至 DetailsView，指派給 DetailsView 的`DataItem`屬性。</span><span class="sxs-lookup"><span data-stu-id="954d1-158">When the DataTable is bound to the DetailsView, the first DataRow in the DataTable is assigned to the DetailsView's `DataItem` property.</span></span> <span data-ttu-id="954d1-159">具體來說，`DataItem`屬性會被指派`DataRowView`物件。</span><span class="sxs-lookup"><span data-stu-id="954d1-159">Specifically, the `DataItem` property is assigned a `DataRowView` object.</span></span> <span data-ttu-id="954d1-160">我們可以使用`DataRowView`的`Row`屬性來存取基礎的 DataRow 物件是實際上`ProductsRow`執行個體。</span><span class="sxs-lookup"><span data-stu-id="954d1-160">We can use the `DataRowView`'s `Row` property to get access to the underlying DataRow object, which is actually a `ProductsRow` instance.</span></span> <span data-ttu-id="954d1-161">一旦我們有此`ProductsRow`我們就可以決定我們只要檢查物件的屬性值的執行個體。</span><span class="sxs-lookup"><span data-stu-id="954d1-161">Once we have this `ProductsRow` instance we can make our decision by simply inspecting the object's property values.</span></span>

<span data-ttu-id="954d1-162">下列程式碼說明如何判斷是否`UnitPrice`DetailsView 控制項繫結的值大於 $75.00:</span><span class="sxs-lookup"><span data-stu-id="954d1-162">The following code illustrates how to determine whether the `UnitPrice` value bound to the DetailsView control is greater than $75.00:</span></span>


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample3.vb)]

> [!NOTE]
> <span data-ttu-id="954d1-163">因為`UnitPrice`可以有`NULL`值在資料庫中，我們會先檢查以確定我們正在未處理的`NULL`前存取值`ProductsRow`的`UnitPrice`屬性。</span><span class="sxs-lookup"><span data-stu-id="954d1-163">Since `UnitPrice` can have a `NULL` value in the database, we first check to make sure that we're not dealing with a `NULL` value before accessing the `ProductsRow`'s `UnitPrice` property.</span></span> <span data-ttu-id="954d1-164">這項檢查，請務必因為如果我們嘗試存取`UnitPrice`屬性時，它有`NULL`值`ProductsRow`物件將會擲回[StrongTypingException 例外狀況](https://msdn.microsoft.com/en-us/library/system.data.strongtypingexception.aspx)。</span><span class="sxs-lookup"><span data-stu-id="954d1-164">This check is important because if we attempt to access the `UnitPrice` property when it has a `NULL` value the `ProductsRow` object will throw a [StrongTypingException exception](https://msdn.microsoft.com/en-us/library/system.data.strongtypingexception.aspx).</span></span>


## <a name="step-3-formatting-the-unitprice-value-in-the-detailsview"></a><span data-ttu-id="954d1-165">步驟 3： 格式化 UnitPrice 值 DetailsView 中</span><span class="sxs-lookup"><span data-stu-id="954d1-165">Step 3: Formatting the UnitPrice Value in the DetailsView</span></span>

<span data-ttu-id="954d1-166">此時我們可以判斷是否`UnitPrice`繫結至 DetailsView 值超過 $75.00，但我們尚未以了解如何以程式設計方式調整 DetailsView 的據以格式化。</span><span class="sxs-lookup"><span data-stu-id="954d1-166">At this point we can determine whether the `UnitPrice` value bound to the DetailsView has a value that exceeds $75.00, but we've yet to see how to programmatically adjust the DetailsView's formatting accordingly.</span></span> <span data-ttu-id="954d1-167">若要修改之格式設定的 DetailsView 中整個資料列，以程式設計方式存取資料列使用`DetailsViewID.Rows(index)`; 若要修改特定的資料格，存取使用`DetailsViewID.Rows(index).Cells(index)`。</span><span class="sxs-lookup"><span data-stu-id="954d1-167">To modify the formatting of an entire row in the DetailsView, programmatically access the row using `DetailsViewID.Rows(index)`; to modify a particular cell, access use `DetailsViewID.Rows(index).Cells(index)`.</span></span> <span data-ttu-id="954d1-168">一旦資料列或資料格的參考我們就可以設定其樣式相關屬性，然後調整其外觀。</span><span class="sxs-lookup"><span data-stu-id="954d1-168">Once we have a reference to the row or cell we can then adjust its appearance by setting its style-related properties.</span></span>

<span data-ttu-id="954d1-169">您必須了解資料列的索引，從 0 開始，以程式設計方式存取資料列。</span><span class="sxs-lookup"><span data-stu-id="954d1-169">Accessing a row programmatically requires that you know the row's index, which starts at 0.</span></span> <span data-ttu-id="954d1-170">`UnitPrice`資料列會提供 4 個索引，並且讓它成為以程式設計方式存取 DetailsView 中的第五個資料列使用`ExpensiveProductsPriceInBoldItalic.Rows(4)`。</span><span class="sxs-lookup"><span data-stu-id="954d1-170">The `UnitPrice` row is the fifth row in the DetailsView, giving it an index of 4 and making it programmatically accessible using `ExpensiveProductsPriceInBoldItalic.Rows(4)`.</span></span> <span data-ttu-id="954d1-171">此時，我們可能使用下列程式碼顯示粗體、 斜體字型中的整個資料列內容：</span><span class="sxs-lookup"><span data-stu-id="954d1-171">At this point we could have the entire row's content displayed in a bold, italic font by using the following code:</span></span>


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample4.vb)]

<span data-ttu-id="954d1-172">不過，這會讓*兩者*標籤 （價格） 和粗體和斜體的值。</span><span class="sxs-lookup"><span data-stu-id="954d1-172">However, this will make *both* the label (Price) and the value bold and italic.</span></span> <span data-ttu-id="954d1-173">如果我們想要只值粗體和斜體我們要將此套用的第二個資料格中的資料列，即可使用下列格式：</span><span class="sxs-lookup"><span data-stu-id="954d1-173">If we want to make just the value bold and italic we need to apply this formatting to the second cell in the row, which can be accomplished using the following:</span></span>


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample5.vb)]

<span data-ttu-id="954d1-174">由於教學課程到目前為止已使用的樣式表維持清楚分隔呈現的標記和樣式的相關資訊，而不是設定特定的樣式屬性，如上所示我們改為使用 CSS 類別。</span><span class="sxs-lookup"><span data-stu-id="954d1-174">Since our tutorials thus far have used stylesheets to maintain a clean separation between the rendered markup and style-related information, rather than setting the specific style properties as shown above let's instead use a CSS class.</span></span> <span data-ttu-id="954d1-175">開啟`Styles.css`樣式表和加入新的 CSS 類別，名為`ExpensivePriceEmphasis`具有下列定義：</span><span class="sxs-lookup"><span data-stu-id="954d1-175">Open the `Styles.css` stylesheet and add a new CSS class named `ExpensivePriceEmphasis` with the following definition:</span></span>


[!code-css[Main](custom-formatting-based-upon-data-vb/samples/sample6.css)]

<span data-ttu-id="954d1-176">然後，在`DataBound`事件處理常式中，設定儲存格的`CssClass`屬性`ExpensivePriceEmphasis`。</span><span class="sxs-lookup"><span data-stu-id="954d1-176">Then, in the `DataBound` event handler, set the cell's `CssClass` property to `ExpensivePriceEmphasis`.</span></span> <span data-ttu-id="954d1-177">下列程式碼會示範`DataBound`完整的事件處理常式：</span><span class="sxs-lookup"><span data-stu-id="954d1-177">The following code shows the `DataBound` event handler in its entirety:</span></span>


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample7.vb)]

<span data-ttu-id="954d1-178">當檢視 Chai，成本小於 $75.00，價格會以一般字型顯示 （請參閱圖 4）。</span><span class="sxs-lookup"><span data-stu-id="954d1-178">When viewing Chai, which costs less than $75.00, the price is displayed in a normal font (see Figure 4).</span></span> <span data-ttu-id="954d1-179">不過，當檢視 Mishi Kobe Niku 有價格為 $97.00，價格會顯示在粗體、 斜體字型 （請參閱圖 5）。</span><span class="sxs-lookup"><span data-stu-id="954d1-179">However, when viewing Mishi Kobe Niku, which has a price of $97.00, the price is displayed in a bold, italic font (see Figure 5).</span></span>


<span data-ttu-id="954d1-180">[![價格小於 $75.00 會以一般字型顯示](custom-formatting-based-upon-data-vb/_static/image9.png)](custom-formatting-based-upon-data-vb/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="954d1-180">[![Prices Less than $75.00 are Displayed in a Normal Font](custom-formatting-based-upon-data-vb/_static/image9.png)](custom-formatting-based-upon-data-vb/_static/image8.png)</span></span>

<span data-ttu-id="954d1-181">**圖 4**： 價格小於 $75.00 會以一般字型顯示 ([按一下以檢視完整大小的影像](custom-formatting-based-upon-data-vb/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="954d1-181">**Figure 4**: Prices Less than $75.00 are Displayed in a Normal Font ([Click to view full-size image](custom-formatting-based-upon-data-vb/_static/image10.png))</span></span>


<span data-ttu-id="954d1-182">[![昂貴的產品價格會顯示在粗體、 斜體字型](custom-formatting-based-upon-data-vb/_static/image12.png)](custom-formatting-based-upon-data-vb/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="954d1-182">[![Expensive Products' Prices are Displayed in a Bold, Italic Font](custom-formatting-based-upon-data-vb/_static/image12.png)](custom-formatting-based-upon-data-vb/_static/image11.png)</span></span>

<span data-ttu-id="954d1-183">**圖 5**： 昂貴的產品價格會顯示在粗體、 斜體字型 ([按一下以檢視完整大小的影像](custom-formatting-based-upon-data-vb/_static/image13.png))</span><span class="sxs-lookup"><span data-stu-id="954d1-183">**Figure 5**: Expensive Products' Prices are Displayed in a Bold, Italic Font ([Click to view full-size image](custom-formatting-based-upon-data-vb/_static/image13.png))</span></span>


## <a name="using-the-formview-controlsdataboundevent-handler"></a><span data-ttu-id="954d1-184">使用 FormView 控制項`DataBound`事件處理常式</span><span class="sxs-lookup"><span data-stu-id="954d1-184">Using the FormView Control's`DataBound`Event Handler</span></span>

<span data-ttu-id="954d1-185">決定繫結至在 FormView 的基礎資料的步驟都與建立 DetailsView`DataBound`事件處理常式，轉換`DataItem`適當的物件類型的屬性繫結至控制項，並判斷要如何繼續執行。</span><span class="sxs-lookup"><span data-stu-id="954d1-185">The steps for determining the underlying data bound to a FormView are identical to those for a DetailsView create a `DataBound` event handler, cast the `DataItem` property to the appropriate object type bound to the control, and determine how to proceed.</span></span> <span data-ttu-id="954d1-186">在 FormView 和 DetailsView 有所不同，不過，在其使用者介面的外觀更新的方式。</span><span class="sxs-lookup"><span data-stu-id="954d1-186">The FormView and DetailsView differ, however, in how their user interface's appearance is updated.</span></span>

<span data-ttu-id="954d1-187">在 FormView 不包含任何 BoundFields，因此缺少`Rows`集合。</span><span class="sxs-lookup"><span data-stu-id="954d1-187">The FormView does not contain any BoundFields and therefore lacks the `Rows` collection.</span></span> <span data-ttu-id="954d1-188">相反地，在 FormView 組成的範本，其中可以包含靜態 HTML 混用，Web 控制項和資料繫結語法。</span><span class="sxs-lookup"><span data-stu-id="954d1-188">Instead, a FormView is composed of templates, which can contain a mix of static HTML, Web controls, and databinding syntax.</span></span> <span data-ttu-id="954d1-189">調整在 FormView 的樣式通常牽涉到調整一個或多個在 FormView 的範本中的 Web 控制項的樣式。</span><span class="sxs-lookup"><span data-stu-id="954d1-189">Adjusting the style of a FormView typically involves adjusting the style of one or more of the Web controls within the FormView's templates.</span></span>

<span data-ttu-id="954d1-190">為了說明這點，讓我們使用產品清單 FormView 如同上述範例中，但這次我們顯示產品名稱和單位庫存顯示紅色字型，如果它小於或等於 10 的庫存單位。</span><span class="sxs-lookup"><span data-stu-id="954d1-190">To illustrate this, let's use a FormView to list products like in the previous example, but this time let's display just the product name and units in stock with the units in stock displayed in a red font if it is less than or equal to 10.</span></span>

## <a name="step-4-displaying-the-product-information-in-a-formview"></a><span data-ttu-id="954d1-191">步驟 4： 在 FormView 中顯示的產品資訊。</span><span class="sxs-lookup"><span data-stu-id="954d1-191">Step 4: Displaying the Product Information in a FormView</span></span>

<span data-ttu-id="954d1-192">新增 FormView`CustomColors.aspx`頁面下方的 DetailsView 和設定其`ID`屬性`LowStockedProductsInRed`。</span><span class="sxs-lookup"><span data-stu-id="954d1-192">Add a FormView to the `CustomColors.aspx` page beneath the DetailsView and set its `ID` property to `LowStockedProductsInRed`.</span></span> <span data-ttu-id="954d1-193">將在 FormView 的繫結至從上一個步驟建立 ObjectDataSource 控制項。</span><span class="sxs-lookup"><span data-stu-id="954d1-193">Bind the FormView to the ObjectDataSource control created from the previous step.</span></span> <span data-ttu-id="954d1-194">這會建立`ItemTemplate`， `EditItemTemplate`，和`InsertItemTemplate`在 FormView 的。</span><span class="sxs-lookup"><span data-stu-id="954d1-194">This will create an `ItemTemplate`, `EditItemTemplate`, and `InsertItemTemplate` for the FormView.</span></span> <span data-ttu-id="954d1-195">移除`EditItemTemplate`和`InsertItemTemplate`並簡化`ItemTemplate`包含只`ProductName`和`UnitsInStock`值，每個自己適當命名的 Label 控制項。</span><span class="sxs-lookup"><span data-stu-id="954d1-195">Remove the `EditItemTemplate` and `InsertItemTemplate` and simplify the `ItemTemplate` to include just the `ProductName` and `UnitsInStock` values, each in their own appropriately-named Label controls.</span></span> <span data-ttu-id="954d1-196">如同在 DetailsView 從先前的範例，也請檢查在 FormView 的智慧標籤的 啟用分頁核取方塊。</span><span class="sxs-lookup"><span data-stu-id="954d1-196">As with the DetailsView from the earlier example, also check the Enable Paging checkbox in the FormView's smart tag.</span></span>

<span data-ttu-id="954d1-197">之後這些編輯 FormView 的標記看起來應該如下所示：</span><span class="sxs-lookup"><span data-stu-id="954d1-197">After these edits your FormView's markup should look similar to the following:</span></span>


[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample8.aspx)]

<span data-ttu-id="954d1-198">請注意，`ItemTemplate`包含：</span><span class="sxs-lookup"><span data-stu-id="954d1-198">Note that the `ItemTemplate` contains:</span></span>

- <span data-ttu-id="954d1-199">**靜態 HTML**文字 「 產品:"和"庫存單位:"連同`<br />`和`<b>`項目。</span><span class="sxs-lookup"><span data-stu-id="954d1-199">**Static HTML** the text "Product:" and "Units In Stock:" along with the `<br />` and `<b>` elements.</span></span>
- <span data-ttu-id="954d1-200">**Web 控制項**兩個 Label 控制項，`ProductNameLabel`和`UnitsInStockLabel`。</span><span class="sxs-lookup"><span data-stu-id="954d1-200">**Web controls** the two Label controls, `ProductNameLabel` and `UnitsInStockLabel`.</span></span>
- <span data-ttu-id="954d1-201">**資料繫結語法**`<%# Bind("ProductName") %>`和`<%# Bind("UnitsInStock") %>`語法，從這些欄位的值指定給 Label 控制項`Text`屬性。</span><span class="sxs-lookup"><span data-stu-id="954d1-201">**Databinding syntax** the `<%# Bind("ProductName") %>` and `<%# Bind("UnitsInStock") %>` syntax, which assigns the values from these fields to the Label controls' `Text` properties.</span></span>

## <a name="step-5-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a><span data-ttu-id="954d1-202">步驟 5： 以程式設計方式判斷此資料繫結事件處理常式中的資料值</span><span class="sxs-lookup"><span data-stu-id="954d1-202">Step 5: Programmatically Determining the Value of the Data in the DataBound Event Handler</span></span>

<span data-ttu-id="954d1-203">在 FormView 的標記完成下, 一個步驟是以程式設計方式判斷是否`UnitsInStock`值小於或等於 10。</span><span class="sxs-lookup"><span data-stu-id="954d1-203">With the FormView's markup complete, the next step is to programmatically determine if the `UnitsInStock` value is less than or equal to 10.</span></span> <span data-ttu-id="954d1-204">這是與 DetailsView 達成完全相同的方式與在 FormView 中。</span><span class="sxs-lookup"><span data-stu-id="954d1-204">This is accomplished in the exact same manner with the FormView as it was with the DetailsView.</span></span> <span data-ttu-id="954d1-205">從建立在 FormView 的事件處理常式開始`DataBound`事件。</span><span class="sxs-lookup"><span data-stu-id="954d1-205">Start by creating an event handler for the FormView's `DataBound` event.</span></span>


![建立資料繫結事件處理常式](custom-formatting-based-upon-data-vb/_static/image14.png)

<span data-ttu-id="954d1-207">**圖 6**： 建立`DataBound`事件處理常式</span><span class="sxs-lookup"><span data-stu-id="954d1-207">**Figure 6**: Create the `DataBound` Event Handler</span></span>


<span data-ttu-id="954d1-208">在事件處理常式轉換在 FormView 的`DataItem`屬性`ProductsRow`執行個體，並判斷是否`UnitsInPrice`值，這類我們需要顯示紅色字型。</span><span class="sxs-lookup"><span data-stu-id="954d1-208">In the event handler cast the FormView's `DataItem` property to a `ProductsRow` instance and determine whether the `UnitsInPrice` value is such that we need to display it in a red font.</span></span>


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample9.vb)]

## <a name="step-6-formatting-the-unitsinstocklabel-label-control-in-the-formviews-itemtemplate"></a><span data-ttu-id="954d1-209">步驟 6： 格式化在 FormView 的 ItemTemplate 的 UnitsInStockLabel 標籤控制項</span><span class="sxs-lookup"><span data-stu-id="954d1-209">Step 6: Formatting the UnitsInStockLabel Label Control in the FormView's ItemTemplate</span></span>

<span data-ttu-id="954d1-210">最後一個步驟是格式化顯示`UnitsInStock`紅色字型中的值如果值為 10 或更少。</span><span class="sxs-lookup"><span data-stu-id="954d1-210">The final step is to format the displayed `UnitsInStock` value in a red font if the value is 10 or less.</span></span> <span data-ttu-id="954d1-211">若要完成此我們需要以程式設計方式存取`UnitsInStockLabel`控制`ItemTemplate`並設定其樣式屬性，使其文字會以紅色顯示。</span><span class="sxs-lookup"><span data-stu-id="954d1-211">To accomplish this we need to programmatically access the `UnitsInStockLabel` control in the `ItemTemplate` and set its style properties so that its text is displayed in red.</span></span> <span data-ttu-id="954d1-212">若要存取的 Web 控制項範本中，使用`FindControl("controlID")`方法如下：</span><span class="sxs-lookup"><span data-stu-id="954d1-212">To access a Web control in a template, use the `FindControl("controlID")` method like this:</span></span>


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample10.vb)]

<span data-ttu-id="954d1-213">此範例中我們想要存取標籤控制項`ID`值是`UnitsInStockLabel`，因此我們會使用：</span><span class="sxs-lookup"><span data-stu-id="954d1-213">For our example we want to access a Label control whose `ID` value is `UnitsInStockLabel`, so we'd use:</span></span>


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample11.vb)]

<span data-ttu-id="954d1-214">一旦 Web 控制項的程式設計參考，我們可以視需要修改它的樣式相關屬性。</span><span class="sxs-lookup"><span data-stu-id="954d1-214">Once we have a programmatic reference to the Web control, we can modify its style-related properties as needed.</span></span> <span data-ttu-id="954d1-215">如先前範例中，我建立了中的 CSS 類別`Styles.css`名為`LowUnitsInStockEmphasis`。</span><span class="sxs-lookup"><span data-stu-id="954d1-215">As with the earlier example, I've created a CSS class in `Styles.css` named `LowUnitsInStockEmphasis`.</span></span> <span data-ttu-id="954d1-216">若要套用此樣式的標籤 Web 控制項，設定其`CssClass`屬性據此。</span><span class="sxs-lookup"><span data-stu-id="954d1-216">To apply this style to the Label Web control, set its `CssClass` property accordingly.</span></span>


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample12.vb)]

> [!NOTE]
> <span data-ttu-id="954d1-217">格式設定範本，以程式設計方式存取 Web 控制項使用的語法`FindControl("controlID")`，然後設定其樣式相關屬性也可使用時[TemplateFields](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.templatefield(VS.80).aspx) DetailsView 或 GridView 中控制項。</span><span class="sxs-lookup"><span data-stu-id="954d1-217">The syntax for formatting a template programmatically accessing the Web control using `FindControl("controlID")` and then setting its style-related properties can also be used when using [TemplateFields](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.templatefield(VS.80).aspx) in the DetailsView or GridView controls.</span></span> <span data-ttu-id="954d1-218">在我們的下一個教學課程中，我們將檢驗 TemplateFields。</span><span class="sxs-lookup"><span data-stu-id="954d1-218">We'll examine TemplateFields in our next tutorial.</span></span>


<span data-ttu-id="954d1-219">圖 7 顯示 FormView，檢視產品時其`UnitsInStock`值大於 10，而圖 8 中的產品有它的值小於 10。</span><span class="sxs-lookup"><span data-stu-id="954d1-219">Figures 7 shows the FormView when viewing a product whose `UnitsInStock` value is greater than 10, while the product in Figure 8 has its value less than 10.</span></span>


<span data-ttu-id="954d1-220">[![針對產品與夠大 庫存單位，套用無自訂格式](custom-formatting-based-upon-data-vb/_static/image16.png)](custom-formatting-based-upon-data-vb/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="954d1-220">[![For Products With a Sufficiently Large Units In Stock, No Custom Formatting is Applied](custom-formatting-based-upon-data-vb/_static/image16.png)](custom-formatting-based-upon-data-vb/_static/image15.png)</span></span>

<span data-ttu-id="954d1-221">**圖 7**： 套用的產品與夠大 庫存單位中，沒有自訂格式 ([按一下以檢視完整大小的影像](custom-formatting-based-upon-data-vb/_static/image17.png))</span><span class="sxs-lookup"><span data-stu-id="954d1-221">**Figure 7**: For Products With a Sufficiently Large Units In Stock, No Custom Formatting is Applied ([Click to view full-size image](custom-formatting-based-upon-data-vb/_static/image17.png))</span></span>


<span data-ttu-id="954d1-222">[![庫存數字的單位數以紅色顯示的那些產品 With Values 10 倍或更少](custom-formatting-based-upon-data-vb/_static/image19.png)](custom-formatting-based-upon-data-vb/_static/image18.png)</span><span class="sxs-lookup"><span data-stu-id="954d1-222">[![The Units in Stock Number is Shown in Red for Those Products With Values of 10 or Less](custom-formatting-based-upon-data-vb/_static/image19.png)](custom-formatting-based-upon-data-vb/_static/image18.png)</span></span>

<span data-ttu-id="954d1-223">**圖 8**: 單位庫存數字以紅色顯示的那些產品 With Values 10 倍或更少 ([按一下以檢視完整大小的影像](custom-formatting-based-upon-data-vb/_static/image20.png))</span><span class="sxs-lookup"><span data-stu-id="954d1-223">**Figure 8**: The Units in Stock Number is Shown in Red for Those Products With Values of 10 or Less ([Click to view full-size image](custom-formatting-based-upon-data-vb/_static/image20.png))</span></span>


## <a name="formatting-with-the-gridviewsrowdataboundevent"></a><span data-ttu-id="954d1-224">格式設定與 GridView`RowDataBound`事件</span><span class="sxs-lookup"><span data-stu-id="954d1-224">Formatting with the GridView's`RowDataBound`Event</span></span>

<span data-ttu-id="954d1-225">稍早我們檢查 DetailsView 步驟的順序和 FormView 控制項進行資料繫結期間。</span><span class="sxs-lookup"><span data-stu-id="954d1-225">Earlier we examined the sequence of steps the DetailsView and FormView controls progress through during databinding.</span></span> <span data-ttu-id="954d1-226">讓我們透過下列步驟再次為重新整理程式。</span><span class="sxs-lookup"><span data-stu-id="954d1-226">Let's look over these steps once again as a refresher.</span></span>

1. <span data-ttu-id="954d1-227">資料 Web 控制項`DataBinding`事件引發。</span><span class="sxs-lookup"><span data-stu-id="954d1-227">The data Web control's `DataBinding` event fires.</span></span>
2. <span data-ttu-id="954d1-228">資料繫結至 Web 控制項的資料。</span><span class="sxs-lookup"><span data-stu-id="954d1-228">The data is bound to the data Web control.</span></span>
3. <span data-ttu-id="954d1-229">資料 Web 控制項`DataBound`事件引發。</span><span class="sxs-lookup"><span data-stu-id="954d1-229">The data Web control's `DataBound` event fires.</span></span>

<span data-ttu-id="954d1-230">這三個簡單步驟已足夠的 DetailsView 和 FormView，因為它們會顯示單一記錄。</span><span class="sxs-lookup"><span data-stu-id="954d1-230">These three simple steps are sufficient for the DetailsView and FormView because they display only a single record.</span></span> <span data-ttu-id="954d1-231">Gridview，其中顯示*所有*記錄繫結它 （不只是第一個），步驟 2 會稍微更為複雜。</span><span class="sxs-lookup"><span data-stu-id="954d1-231">For the GridView, which displays *all* records bound to it (not just the first), step 2 is a bit more involved.</span></span>

<span data-ttu-id="954d1-232">在步驟的 2 GridView 會列舉資料來源，以及每個記錄，建立`GridViewRow`執行個體，並繫結到它的目前記錄。</span><span class="sxs-lookup"><span data-stu-id="954d1-232">In step 2 the GridView enumerates the data source and, for each record, creates a `GridViewRow` instance and binds the current record to it.</span></span> <span data-ttu-id="954d1-233">每個`GridViewRow`加入至 GridView，會引發兩個事件：</span><span class="sxs-lookup"><span data-stu-id="954d1-233">For each `GridViewRow` added to the GridView, two events are raised:</span></span>

- <span data-ttu-id="954d1-234">**`RowCreated`**之後，都會引發`GridViewRow`已建立</span><span class="sxs-lookup"><span data-stu-id="954d1-234">**`RowCreated`** fires after the `GridViewRow` has been created</span></span>
- <span data-ttu-id="954d1-235">**`RowDataBound`**目前的記錄繫結至後引發`GridViewRow`。</span><span class="sxs-lookup"><span data-stu-id="954d1-235">**`RowDataBound`** fires after the current record has been bound to the `GridViewRow`.</span></span>

<span data-ttu-id="954d1-236">Gridview，然後，資料繫結是更精確地分為下列一連串步驟：</span><span class="sxs-lookup"><span data-stu-id="954d1-236">For the GridView, then, data binding is more accurately described by the following sequence of steps:</span></span>

1. <span data-ttu-id="954d1-237">GridView`DataBinding`事件引發。</span><span class="sxs-lookup"><span data-stu-id="954d1-237">The GridView's `DataBinding` event fires.</span></span>
2. <span data-ttu-id="954d1-238">資料繫結至 GridView。</span><span class="sxs-lookup"><span data-stu-id="954d1-238">The data is bound to the GridView.</span></span>   
  
 <span data-ttu-id="954d1-239">資料來源中的每一筆記錄</span><span class="sxs-lookup"><span data-stu-id="954d1-239">For each record in the data source</span></span> 

    1. <span data-ttu-id="954d1-240">建立`GridViewRow`物件</span><span class="sxs-lookup"><span data-stu-id="954d1-240">Create a `GridViewRow` object</span></span>
    2. <span data-ttu-id="954d1-241">引發`RowCreated`事件</span><span class="sxs-lookup"><span data-stu-id="954d1-241">Fire the `RowCreated` event</span></span>
    3. <span data-ttu-id="954d1-242">繫結至資料錄`GridViewRow`</span><span class="sxs-lookup"><span data-stu-id="954d1-242">Bind the record to the `GridViewRow`</span></span>
    4. <span data-ttu-id="954d1-243">引發`RowDataBound`事件</span><span class="sxs-lookup"><span data-stu-id="954d1-243">Fire the `RowDataBound` event</span></span>
    5. <span data-ttu-id="954d1-244">新增`GridViewRow`至`Rows`集合</span><span class="sxs-lookup"><span data-stu-id="954d1-244">Add the `GridViewRow` to the `Rows` collection</span></span>
3. <span data-ttu-id="954d1-245">GridView`DataBound`事件引發。</span><span class="sxs-lookup"><span data-stu-id="954d1-245">The GridView's `DataBound` event fires.</span></span>

<span data-ttu-id="954d1-246">若要自訂的 GridView 的個別記錄格式，然後，我們必須建立事件處理常式`RowDataBound`事件。</span><span class="sxs-lookup"><span data-stu-id="954d1-246">To customize the format of the GridView's individual records, then, we need to create an event handler for the `RowDataBound` event.</span></span> <span data-ttu-id="954d1-247">為了說明這點，讓我們加入 GridView`CustomColors.aspx`列出名稱、 類別目錄，以及這些產品的價格是不超過 $10.00 以黃色背景的色彩反白顯示的每個產品價格的頁面。</span><span class="sxs-lookup"><span data-stu-id="954d1-247">To illustrate this, let's add a GridView to the `CustomColors.aspx` page that lists the name, category, and price for each product, highlighting those products whose price is less than $10.00 with a yellow background color.</span></span>

## <a name="step-7-displaying-product-information-in-a-gridview"></a><span data-ttu-id="954d1-248">步驟 7： 在 GridView 中顯示產品資訊</span><span class="sxs-lookup"><span data-stu-id="954d1-248">Step 7: Displaying Product Information in a GridView</span></span>

<span data-ttu-id="954d1-249">在 FormView 下方的 GridView 加入前一個範例並設定其`ID`屬性`HighlightCheapProducts`。</span><span class="sxs-lookup"><span data-stu-id="954d1-249">Add a GridView beneath the FormView from the previous example and set its `ID` property to `HighlightCheapProducts`.</span></span> <span data-ttu-id="954d1-250">因為我們已經在頁面上會傳回所有產品的 Sqldatasource，繫結的 GridView 的。</span><span class="sxs-lookup"><span data-stu-id="954d1-250">Since we already have an ObjectDataSource that returns all products on the page, bind the GridView to that.</span></span> <span data-ttu-id="954d1-251">最後，編輯 GridView 的 BoundFields 以包含剛才產品的名稱、 分類和價格。</span><span class="sxs-lookup"><span data-stu-id="954d1-251">Finally, edit the GridView's BoundFields to include just the products' names, categories, and prices.</span></span> <span data-ttu-id="954d1-252">之後這些編輯 GridView 標記看起來應該像：</span><span class="sxs-lookup"><span data-stu-id="954d1-252">After these edits the GridView's markup should look like:</span></span>


[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample13.aspx)]

<span data-ttu-id="954d1-253">圖 9 顯示我們透過瀏覽器檢視時，此點的進度。</span><span class="sxs-lookup"><span data-stu-id="954d1-253">Figure 9 shows our progress to this point when viewed through a browser.</span></span>


<span data-ttu-id="954d1-254">[![在 GridView 列出名稱、 類別和每項產品的價格](custom-formatting-based-upon-data-vb/_static/image22.png)](custom-formatting-based-upon-data-vb/_static/image21.png)</span><span class="sxs-lookup"><span data-stu-id="954d1-254">[![The GridView Lists the Name, Category, and Price For Each Product](custom-formatting-based-upon-data-vb/_static/image22.png)](custom-formatting-based-upon-data-vb/_static/image21.png)</span></span>

<span data-ttu-id="954d1-255">**圖 9**: GridView 列出名稱、 類別和每個產品的價格 ([按一下以檢視完整大小的影像](custom-formatting-based-upon-data-vb/_static/image23.png))</span><span class="sxs-lookup"><span data-stu-id="954d1-255">**Figure 9**: The GridView Lists the Name, Category, and Price For Each Product ([Click to view full-size image](custom-formatting-based-upon-data-vb/_static/image23.png))</span></span>


## <a name="step-8-programmatically-determining-the-value-of-the-data-in-the-rowdatabound-event-handler"></a><span data-ttu-id="954d1-256">步驟 8： 以程式設計方式判斷 RowDataBound 事件處理常式中的資料值</span><span class="sxs-lookup"><span data-stu-id="954d1-256">Step 8: Programmatically Determining the Value of the Data in the RowDataBound Event Handler</span></span>

<span data-ttu-id="954d1-257">當`ProductsDataTable`繫結至 GridView 其`ProductsRow`執行個體是列舉，以及對於每個`ProductsRow``GridViewRow`建立。</span><span class="sxs-lookup"><span data-stu-id="954d1-257">When the `ProductsDataTable` is bound to the GridView its `ProductsRow` instances are enumerated and for each `ProductsRow` a `GridViewRow` is created.</span></span> <span data-ttu-id="954d1-258">`GridViewRow`的`DataItem`屬性指派給特定`ProductRow`，其後 GridView`RowDataBound`事件處理常式，就會引發。</span><span class="sxs-lookup"><span data-stu-id="954d1-258">The `GridViewRow`'s `DataItem` property is assigned to the particular `ProductRow`, after which the GridView's `RowDataBound` event handler is raised.</span></span> <span data-ttu-id="954d1-259">若要判斷`UnitPrice`每項產品的值繫結至 GridView，則我們需要建立事件處理常式的 GridView`RowDataBound`事件。</span><span class="sxs-lookup"><span data-stu-id="954d1-259">To determine the `UnitPrice` value for each product bound to the GridView, then, we need to create an event handler for the GridView's `RowDataBound` event.</span></span> <span data-ttu-id="954d1-260">此事件處理常式中，我們可以檢查`UnitPrice`值目前`GridViewRow`做出格式化的決策，該資料列。</span><span class="sxs-lookup"><span data-stu-id="954d1-260">In this event handler we can inspect the `UnitPrice` value for the current `GridViewRow` and make a formatting decision for that row.</span></span>

<span data-ttu-id="954d1-261">可以使用相同的一系列步驟，做為 DetailsView FormView 與建立此事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="954d1-261">This event handler can be created using the same series of steps as with the FormView and DetailsView.</span></span>


![GridView RowDataBound 事件建立事件處理常式](custom-formatting-based-upon-data-vb/_static/image24.png)

<span data-ttu-id="954d1-263">**圖 10**： 建立事件處理常式的 GridView`RowDataBound`事件</span><span class="sxs-lookup"><span data-stu-id="954d1-263">**Figure 10**: Create an Event Handler for the GridView's `RowDataBound` Event</span></span>


<span data-ttu-id="954d1-264">以這種方式建立事件處理常式，將導致下列的程式碼會自動加入至 ASP.NET 網頁的程式碼部分：</span><span class="sxs-lookup"><span data-stu-id="954d1-264">Creating the event hander in this manner will cause the following code to be automatically added to the ASP.NET page's code portion:</span></span>


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample14.vb)]

<span data-ttu-id="954d1-265">當`RowDataBound`事件引發的事件處理常式會傳遞做為其第二個參數類型的物件`GridViewRowEventArgs`，其具有內容，名為`Row`。</span><span class="sxs-lookup"><span data-stu-id="954d1-265">When the `RowDataBound` event fires, the event handler is passed as its second parameter an object of type `GridViewRowEventArgs`, which has a property named `Row`.</span></span> <span data-ttu-id="954d1-266">這個屬性會傳回參考`GridViewRow`時只是資料繫結。</span><span class="sxs-lookup"><span data-stu-id="954d1-266">This property returns a reference to the `GridViewRow` that was just data bound.</span></span> <span data-ttu-id="954d1-267">若要存取`ProductsRow`執行個體繫結至`GridViewRow`我們使用`DataItem`屬性如下所示：</span><span class="sxs-lookup"><span data-stu-id="954d1-267">To access the `ProductsRow` instance bound to the `GridViewRow` we use the `DataItem` property like so:</span></span>


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample15.vb)]

<span data-ttu-id="954d1-268">當使用`RowDataBound`務必記住 GridView 組成不同類型的資料列，而且此事件針對引發的事件處理常式*所有*資料列型別。</span><span class="sxs-lookup"><span data-stu-id="954d1-268">When working with the `RowDataBound` event handler it is important to keep in mind that the GridView is composed of different types of rows and that this event is fired for *all* row types.</span></span> <span data-ttu-id="954d1-269">A`GridViewRow`的類型由其`RowType`屬性，且可以有一個可能的值：</span><span class="sxs-lookup"><span data-stu-id="954d1-269">A `GridViewRow`'s type can be determined by its `RowType` property, and can have one of the possible values:</span></span>

- <span data-ttu-id="954d1-270">`DataRow`繫結至資料錄從 GridView 的資料列`DataSource`</span><span class="sxs-lookup"><span data-stu-id="954d1-270">`DataRow` a row that is bound to a record from the GridView's `DataSource`</span></span>
- <span data-ttu-id="954d1-271">`EmptyDataRow`如果顯示的資料列的 GridView`DataSource`是空的</span><span class="sxs-lookup"><span data-stu-id="954d1-271">`EmptyDataRow` the row displayed if the GridView's `DataSource` is empty</span></span>
- <span data-ttu-id="954d1-272">`Footer`頁尾資料列。顯示的如果 GridView`ShowFooter`屬性設定為`True`</span><span class="sxs-lookup"><span data-stu-id="954d1-272">`Footer` the footer row; shown if the GridView's `ShowFooter` property is set to `True`</span></span>
- <span data-ttu-id="954d1-273">`Header`標頭資料列，顯示是否 GridView ShowHeader 屬性會設定為`True`（預設值）</span><span class="sxs-lookup"><span data-stu-id="954d1-273">`Header` the header row; shown if the GridView's ShowHeader property is set to `True` (the default)</span></span>
- <span data-ttu-id="954d1-274">`Pager`會實作的 GridView 的分頁，顯示分頁介面的資料列</span><span class="sxs-lookup"><span data-stu-id="954d1-274">`Pager` for GridView's that implement paging, the row that displays the paging interface</span></span>
- <span data-ttu-id="954d1-275">`Separator`不會用於 GridView，但使用`RowType`DataList 和中繼器內容控制項，兩個資料中，我們會討論在未來教學課程的 Web 控制項</span><span class="sxs-lookup"><span data-stu-id="954d1-275">`Separator` not used for the GridView, but used by the `RowType` properties for the DataList and Repeater controls, two data Web controls we'll discuss in future tutorials</span></span>

<span data-ttu-id="954d1-276">因為`EmptyDataRow`， `Header`， `Footer`，和`Pager`與相關聯的資料列不是`DataSource`記錄，它們會一直有一個值的`Nothing`針對其`DataItem`屬性。</span><span class="sxs-lookup"><span data-stu-id="954d1-276">Since the `EmptyDataRow`, `Header`, `Footer`, and `Pager` rows aren't associated with a `DataSource` record, they will always have a value of `Nothing` for their `DataItem` property.</span></span> <span data-ttu-id="954d1-277">基於這個原因，然後再嘗試將工作與目前`GridViewRow`的`DataItem`屬性，我們必須先確定我們正在處理的`DataRow`。</span><span class="sxs-lookup"><span data-stu-id="954d1-277">For this reason, before attempting to work with the current `GridViewRow`'s `DataItem` property, we first must make sure that we're dealing with a `DataRow`.</span></span> <span data-ttu-id="954d1-278">這可以透過檢查`GridViewRow`的`RowType`屬性如下所示：</span><span class="sxs-lookup"><span data-stu-id="954d1-278">This can be accomplished by checking the `GridViewRow`'s `RowType` property like so:</span></span>


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample16.vb)]

## <a name="step-9-highlighting-the-row-yellow-when-the-unitprice-value-is-less-than-1000"></a><span data-ttu-id="954d1-279">步驟 9： 反白顯示資料列黃色時 UnitPrice 值是不超過 $10.00</span><span class="sxs-lookup"><span data-stu-id="954d1-279">Step 9: Highlighting the Row Yellow When the UnitPrice Value is Less than $10.00</span></span>

<span data-ttu-id="954d1-280">最後一個步驟是以程式設計的方式反白顯示整個`GridViewRow`如果`UnitPrice`值該資料列是不超過 $10.00。</span><span class="sxs-lookup"><span data-stu-id="954d1-280">The last step is to programmatically highlight the entire `GridViewRow` if the `UnitPrice` value for that row is less than $10.00.</span></span> <span data-ttu-id="954d1-281">存取的 GridView 資料列或資料格的語法是如同 DetailsView`GridViewID.Rows(index)`存取整個資料列中，`GridViewID.Rows(index).Cells(index)`存取特定資料格。</span><span class="sxs-lookup"><span data-stu-id="954d1-281">The syntax for accessing a GridView's rows or cells is the same as with the DetailsView `GridViewID.Rows(index)` to access the entire row, `GridViewID.Rows(index).Cells(index)` to access a particular cell.</span></span> <span data-ttu-id="954d1-282">不過，當`RowDataBound`事件處理常式就會引發資料繫結`GridViewRow`尚未加入至 GridView`Rows`集合。</span><span class="sxs-lookup"><span data-stu-id="954d1-282">However, when the `RowDataBound` event handler fires the data bound `GridViewRow` has yet to be added to the GridView's `Rows` collection.</span></span> <span data-ttu-id="954d1-283">因此，您無法存取目前`GridViewRow`的執行個體`RowDataBound`使用資料列集合的事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="954d1-283">Therefore you cannot access the current `GridViewRow` instance from the `RowDataBound` event handler using the Rows collection.</span></span>

<span data-ttu-id="954d1-284">而不是`GridViewID.Rows(index)`，我們可以參考目前`GridViewRow`執行個體中`RowDataBound`事件處理常式使用`e.Row`。</span><span class="sxs-lookup"><span data-stu-id="954d1-284">Instead of `GridViewID.Rows(index)`, we can reference the current `GridViewRow` instance in the `RowDataBound` event handler using `e.Row`.</span></span> <span data-ttu-id="954d1-285">也就是為了反白顯示目前`GridViewRow`的執行個體`RowDataBound`我們要使用的事件處理常式：</span><span class="sxs-lookup"><span data-stu-id="954d1-285">That is, in order to highlight the current `GridViewRow` instance from the `RowDataBound` event handler we would use:</span></span>


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample17.vb)]

<span data-ttu-id="954d1-286">而非設定`GridViewRow`的`BackColor`屬性直接管理，讓我們堅持使用 CSS 類別。</span><span class="sxs-lookup"><span data-stu-id="954d1-286">Rather than set the `GridViewRow`'s `BackColor` property directly, let's stick with using CSS classes.</span></span> <span data-ttu-id="954d1-287">我已經建立名為的 CSS 類別`AffordablePriceEmphasis`所設定的背景色彩為黃色。</span><span class="sxs-lookup"><span data-stu-id="954d1-287">I've created a CSS class named `AffordablePriceEmphasis` that sets the background color to yellow.</span></span> <span data-ttu-id="954d1-288">已完成`RowDataBound`事件處理常式會遵循：</span><span class="sxs-lookup"><span data-stu-id="954d1-288">The completed `RowDataBound` event handler follows:</span></span>


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample18.vb)]


<span data-ttu-id="954d1-289">[![最合理的產品為黃色反白顯示](custom-formatting-based-upon-data-vb/_static/image26.png)](custom-formatting-based-upon-data-vb/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="954d1-289">[![The Most Affordable Products are Highlighted Yellow](custom-formatting-based-upon-data-vb/_static/image26.png)](custom-formatting-based-upon-data-vb/_static/image25.png)</span></span>

<span data-ttu-id="954d1-290">**圖 11**: 大部分價格合理的產品為黃色反白顯示 ([按一下以檢視完整大小的影像](custom-formatting-based-upon-data-vb/_static/image27.png))</span><span class="sxs-lookup"><span data-stu-id="954d1-290">**Figure 11**: The Most Affordable Products are Highlighted Yellow ([Click to view full-size image](custom-formatting-based-upon-data-vb/_static/image27.png))</span></span>


## <a name="summary"></a><span data-ttu-id="954d1-291">總結</span><span class="sxs-lookup"><span data-stu-id="954d1-291">Summary</span></span>

<span data-ttu-id="954d1-292">在本教學課程中，我們看到如何格式化 GridView、 DetailsView 和根據資料繫結至控制項的程式設計。</span><span class="sxs-lookup"><span data-stu-id="954d1-292">In this tutorial we saw how to format the GridView, DetailsView, and FormView based on the data bound to the control.</span></span> <span data-ttu-id="954d1-293">若要完成此我們建立的事件處理常式`DataBound`或`RowDataBound`其中基礎資料已檢查的格式設定的變更，以及視需要的事件。</span><span class="sxs-lookup"><span data-stu-id="954d1-293">To accomplish this we created an event handler for the `DataBound` or `RowDataBound` events, where the underlying data was examined along with a formatting change, if needed.</span></span> <span data-ttu-id="954d1-294">若要存取的資料繫結至 DetailsView 或 FormView，我們使用`DataItem`屬性`DataBound`事件處理常式; GridView，每個`GridViewRow`執行個體的`DataItem`屬性包含的資料繫結至該資料列，其可於`RowDataBound`事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="954d1-294">To access the data bound to a DetailsView or FormView, we use the `DataItem` property in the `DataBound` event handler; for a GridView, each `GridViewRow` instance's `DataItem` property contains the data bound to that row, which is available in the `RowDataBound` event handler.</span></span>

<span data-ttu-id="954d1-295">以程式設計方式調整資料 Web 控制項的格式設定的語法，取決於 Web 控制項和格式化資料的顯示方式。</span><span class="sxs-lookup"><span data-stu-id="954d1-295">The syntax for programmatically adjusting the data Web control's formatting depends upon the Web control and how the data to be formatted is displayed.</span></span> <span data-ttu-id="954d1-296">DetailsView 和 GridView 控制項、 資料列和資料格可以透過序數索引。</span><span class="sxs-lookup"><span data-stu-id="954d1-296">For DetailsView and GridView controls, the rows and cells can be accessed by an ordinal index.</span></span> <span data-ttu-id="954d1-297">FormView 中，會使用範本，如`FindControl("controlID")`方法通常用來找出範本內的 Web 控制項。</span><span class="sxs-lookup"><span data-stu-id="954d1-297">For the FormView, which uses templates, the `FindControl("controlID")` method is commonly used to locate a Web control from within the template.</span></span>

<span data-ttu-id="954d1-298">下一個教學課程中我們將探討如何使用 GridView 和 DetailsView 的範本。</span><span class="sxs-lookup"><span data-stu-id="954d1-298">In the next tutorial we'll look at how to use templates with the GridView and DetailsView.</span></span> <span data-ttu-id="954d1-299">此外，我們會看到另一個自訂的基礎資料的格式設定的技術。</span><span class="sxs-lookup"><span data-stu-id="954d1-299">Additionally, we'll see another technique for customizing the formatting based on the underlying data.</span></span>

<span data-ttu-id="954d1-300">祝您程式設計 ！</span><span class="sxs-lookup"><span data-stu-id="954d1-300">Happy Programming!</span></span>

## <a name="about-the-author"></a><span data-ttu-id="954d1-301">關於作者</span><span class="sxs-lookup"><span data-stu-id="954d1-301">About the Author</span></span>

<span data-ttu-id="954d1-302">[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。</span><span class="sxs-lookup"><span data-stu-id="954d1-302">[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), author of seven ASP/ASP.NET books and founder of [4GuysFromRolla.com](http://www.4guysfromrolla.com), has been working with Microsoft Web technologies since 1998.</span></span> <span data-ttu-id="954d1-303">Scott 可做為獨立顧問、 訓練和寫入器。</span><span class="sxs-lookup"><span data-stu-id="954d1-303">Scott works as an independent consultant, trainer, and writer.</span></span> <span data-ttu-id="954d1-304">他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。</span><span class="sxs-lookup"><span data-stu-id="954d1-304">His latest book is [*Sams Teach Yourself ASP.NET 2.0 in 24 Hours*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco).</span></span> <span data-ttu-id="954d1-305">他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。</span><span class="sxs-lookup"><span data-stu-id="954d1-305">He can be reached at [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) or via his blog, which can be found at [http://ScottOnWriting.NET](http://ScottOnWriting.NET).</span></span>

## <a name="special-thanks-to"></a><span data-ttu-id="954d1-306">特別感謝</span><span class="sxs-lookup"><span data-stu-id="954d1-306">Special Thanks To</span></span>

<span data-ttu-id="954d1-307">許多有用的檢閱者已檢閱本教學課程系列。</span><span class="sxs-lookup"><span data-stu-id="954d1-307">This tutorial series was reviewed by many helpful reviewers.</span></span> <span data-ttu-id="954d1-308">本教學課程總計 E.R.導致檢閱者</span><span class="sxs-lookup"><span data-stu-id="954d1-308">Lead reviewers for this tutorial were E.R.</span></span> <span data-ttu-id="954d1-309">Gilmore，Dennis Patterson 和 Dan Jagers。</span><span class="sxs-lookup"><span data-stu-id="954d1-309">Gilmore, Dennis Patterson, and Dan Jagers.</span></span> <span data-ttu-id="954d1-310">檢閱我即將推出的 MSDN 文件有興趣嗎？</span><span class="sxs-lookup"><span data-stu-id="954d1-310">Interested in reviewing my upcoming MSDN articles?</span></span> <span data-ttu-id="954d1-311">如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)</span><span class="sxs-lookup"><span data-stu-id="954d1-311">If so, drop me a line at [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="954d1-312">[上一頁](displaying-summary-information-in-the-gridview-s-footer-cs.md)
[下一頁](using-templatefields-in-the-gridview-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="954d1-312">[Previous](displaying-summary-information-in-the-gridview-s-footer-cs.md)
[Next](using-templatefields-in-the-gridview-control-vb.md)</span></span>
