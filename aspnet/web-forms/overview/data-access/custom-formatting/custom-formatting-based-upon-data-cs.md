---
uid: web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-cs
title: "自訂的格式為基礎的資料 (C#) |Microsoft 文件"
author: rick-anderson
description: "以多種方式可以完成調整 GridView、 DetailsView 或根據繫結至它的資料在 FormView 的格式。 在本教學課程中，我們會 l..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 871a4574-f89c-4214-b786-79253ed3653b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 7c44327e1196a9e7cb9f9d12c963fb5f9b6b1b41
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="custom-formatting-based-upon-data-c"></a><span data-ttu-id="92630-104">自訂的格式為基礎的資料 (C#)</span><span class="sxs-lookup"><span data-stu-id="92630-104">Custom Formatting Based Upon Data (C#)</span></span>
====================
<span data-ttu-id="92630-105">由[Scott Mitchell](https://twitter.com/ScottOnWriting)</span><span class="sxs-lookup"><span data-stu-id="92630-105">by [Scott Mitchell](https://twitter.com/ScottOnWriting)</span></span>

<span data-ttu-id="92630-106">[下載範例應用程式](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_11_CS.exe)或[下載 PDF](custom-formatting-based-upon-data-cs/_static/datatutorial11cs1.pdf)</span><span class="sxs-lookup"><span data-stu-id="92630-106">[Download Sample App](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_11_CS.exe) or [Download PDF](custom-formatting-based-upon-data-cs/_static/datatutorial11cs1.pdf)</span></span>

> <span data-ttu-id="92630-107">以多種方式可以完成調整 GridView、 DetailsView 或根據繫結至它的資料在 FormView 的格式。</span><span class="sxs-lookup"><span data-stu-id="92630-107">Adjusting the format of the GridView, DetailsView, or FormView based upon the data bound to it can be accomplished in multiple ways.</span></span> <span data-ttu-id="92630-108">在此教學課程中我們將探討如何完成資料繫結透過資料繫結和 RowDataBound 事件處理常式使用的格式。</span><span class="sxs-lookup"><span data-stu-id="92630-108">In this tutorial we'll look at how to accomplish data bound formatting through the use of the DataBound and RowDataBound event handlers.</span></span>


## <a name="introduction"></a><span data-ttu-id="92630-109">簡介</span><span class="sxs-lookup"><span data-stu-id="92630-109">Introduction</span></span>

<span data-ttu-id="92630-110">透過各種不同的樣式相關屬性，您可以自訂 GridView、 DetailsView 和 FormView 控制項的外觀。</span><span class="sxs-lookup"><span data-stu-id="92630-110">The appearance of the GridView, DetailsView, and FormView controls can be customized through a myriad of style-related properties.</span></span> <span data-ttu-id="92630-111">屬性，例如`CssClass`， `Font`， `BorderWidth`， `BorderStyle`， `BorderColor`， `Width`，和`Height`，和其他項目，指定一般呈現的控制項的外觀。</span><span class="sxs-lookup"><span data-stu-id="92630-111">Properties like `CssClass`, `Font`, `BorderWidth`, `BorderStyle`, `BorderColor`, `Width`, and `Height`, among others, dictate the general appearance of the rendered control.</span></span> <span data-ttu-id="92630-112">屬性包括`HeaderStyle`， `RowStyle`， `AlternatingRowStyle`，和其他項目允許這些相同的樣式設定来套用至特定區段。</span><span class="sxs-lookup"><span data-stu-id="92630-112">Properties including `HeaderStyle`, `RowStyle`, `AlternatingRowStyle`, and others allow these same style settings to be applied to particular sections.</span></span> <span data-ttu-id="92630-113">同樣地，可以在欄位層級套用這些樣式設定。</span><span class="sxs-lookup"><span data-stu-id="92630-113">Likewise, these style settings can be applied at the field level.</span></span>

<span data-ttu-id="92630-114">在許多情況下，格式需求取決於所顯示的資料值。</span><span class="sxs-lookup"><span data-stu-id="92630-114">In many scenarios though, the formatting requirements depend upon the value of the displayed data.</span></span> <span data-ttu-id="92630-115">例如，若要強調的內建的產品，報告，列出產品資訊可能會設定背景色彩為黃色，這些產品的`UnitsInStock`和`UnitsOnOrder`欄位都等於 0。</span><span class="sxs-lookup"><span data-stu-id="92630-115">For example, to draw attention to out of stock products, a report listing product information might set the background color to yellow for those products whose `UnitsInStock` and `UnitsOnOrder` fields are both equal to 0.</span></span> <span data-ttu-id="92630-116">反白顯示的價格的產品，我們可能會想要顯示成本超過 $75.00 以粗體字這些產品的價格。</span><span class="sxs-lookup"><span data-stu-id="92630-116">To highlight the more expensive products, we may want to display the prices of those products costing more than $75.00 in a bold font.</span></span>

<span data-ttu-id="92630-117">以多種方式可以完成調整 GridView、 DetailsView 或根據繫結至它的資料在 FormView 的格式。</span><span class="sxs-lookup"><span data-stu-id="92630-117">Adjusting the format of the GridView, DetailsView, or FormView based upon the data bound to it can be accomplished in multiple ways.</span></span> <span data-ttu-id="92630-118">在此教學課程中我們會探討如何完成資料繫結透過使用的格式`DataBound`和`RowDataBound`事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="92630-118">In this tutorial we'll look at how to accomplish data bound formatting through the use of the `DataBound` and `RowDataBound` event handlers.</span></span> <span data-ttu-id="92630-119">在下一個教學課程中，我們將探討的替代方式。</span><span class="sxs-lookup"><span data-stu-id="92630-119">In the next tutorial we'll explore an alternative approach.</span></span>

## <a name="using-the-detailsview-controlsdataboundevent-handler"></a><span data-ttu-id="92630-120">使用 DetailsView 控制項`DataBound`事件處理常式</span><span class="sxs-lookup"><span data-stu-id="92630-120">Using the DetailsView Control's`DataBound`Event Handler</span></span>

<span data-ttu-id="92630-121">當資料繫結至 DetailsView，從資料來源控制項，或是透過程式設計方式將資料指派給控制項的`DataSource`屬性，並呼叫其`DataBind()`方法，下列步驟順序發生：</span><span class="sxs-lookup"><span data-stu-id="92630-121">When data is bound to a DetailsView, either from a data source control or through programmatically assigning data to the control's `DataSource` property and calling its `DataBind()` method, the following sequence of steps occur:</span></span>

1. <span data-ttu-id="92630-122">資料 Web 控制項`DataBinding`事件引發。</span><span class="sxs-lookup"><span data-stu-id="92630-122">The data Web control's `DataBinding` event fires.</span></span>
2. <span data-ttu-id="92630-123">資料繫結至 Web 控制項的資料。</span><span class="sxs-lookup"><span data-stu-id="92630-123">The data is bound to the data Web control.</span></span>
3. <span data-ttu-id="92630-124">資料 Web 控制項`DataBound`事件引發。</span><span class="sxs-lookup"><span data-stu-id="92630-124">The data Web control's `DataBound` event fires.</span></span>

<span data-ttu-id="92630-125">可透過事件處理常式的步驟 1 和 3 之後立即插入自訂邏輯。</span><span class="sxs-lookup"><span data-stu-id="92630-125">Custom logic can be injected immediately after steps 1 and 3 through an event handler.</span></span> <span data-ttu-id="92630-126">藉由建立的事件處理常式`DataBound`我們以程式設計方式判斷已經過的資料繫結至資料 Web 控制項，並調整格式所需的事件。</span><span class="sxs-lookup"><span data-stu-id="92630-126">By creating an event handler for the `DataBound` event we can programmatically determine the data that has been bound to the data Web control and adjust the formatting as needed.</span></span> <span data-ttu-id="92630-127">為了說明這點讓我們來建立會列出產品的一般資訊，但將會顯示在 DetailsView`UnitPrice`值***粗體、 斜體字型***如果它超過 $75.00。</span><span class="sxs-lookup"><span data-stu-id="92630-127">To illustrate this let's create a DetailsView that will list general information about a product, but will display the `UnitPrice` value in a ***bold, italic font*** if it exceeds $75.00.</span></span>

## <a name="step-1-displaying-the-product-information-in-a-detailsview"></a><span data-ttu-id="92630-128">步驟 1： 在 DetailsView 中顯示的產品資訊。</span><span class="sxs-lookup"><span data-stu-id="92630-128">Step 1: Displaying the Product Information in a DetailsView</span></span>

<span data-ttu-id="92630-129">開啟`CustomColors.aspx`頁面`CustomFormatting`資料夾中，將 DetailsView 控制項從工具箱拖曳至設計工具，設定其`ID`屬性值設定為`ExpensiveProductsPriceInBoldItalic`，並將它繫結至新的 ObjectDataSource 控制項叫用`ProductsBLL`類別的`GetProducts()`方法。</span><span class="sxs-lookup"><span data-stu-id="92630-129">Open the `CustomColors.aspx` page in the `CustomFormatting` folder, drag a DetailsView control from the Toolbox onto the Designer, set its `ID` property value to `ExpensiveProductsPriceInBoldItalic`, and bind it to a new ObjectDataSource control that invokes the `ProductsBLL` class's `GetProducts()` method.</span></span> <span data-ttu-id="92630-130">為求簡單明瞭，因為我們仔細檢查其先前的教學課程中完成此的詳細的步驟會這裡省略。</span><span class="sxs-lookup"><span data-stu-id="92630-130">The detailed steps for accomplishing this are omitted here for brevity since we examined them in detail in previous tutorials.</span></span>

<span data-ttu-id="92630-131">一旦您已經在 detailsview 結合 ObjectDataSource，請花一點時間修改的欄位清單。</span><span class="sxs-lookup"><span data-stu-id="92630-131">Once you've bound the ObjectDataSource to the DetailsView, take a moment to modify the field list.</span></span> <span data-ttu-id="92630-132">我已選擇移除`ProductID`， `SupplierID`， `CategoryID`， `UnitsInStock`， `UnitsOnOrder`， `ReorderLevel`，和`Discontinued`BoundFields 和重新命名並重新格式化剩餘 BoundFields。</span><span class="sxs-lookup"><span data-stu-id="92630-132">I've opted to remove the `ProductID`, `SupplierID`, `CategoryID`, `UnitsInStock`, `UnitsOnOrder`, `ReorderLevel`, and `Discontinued` BoundFields and renamed and reformatted the remaining BoundFields.</span></span> <span data-ttu-id="92630-133">同時也清除`Width`和`Height`設定。</span><span class="sxs-lookup"><span data-stu-id="92630-133">I also cleared out the `Width` and `Height` settings.</span></span> <span data-ttu-id="92630-134">在 DetailsView 會顯示一筆記錄，因為我們需要啟用分頁，以便讓使用者檢視的所有產品。</span><span class="sxs-lookup"><span data-stu-id="92630-134">Since the DetailsView displays only a single record, we need to enable paging in order to allow the end user to view all of the products.</span></span> <span data-ttu-id="92630-135">這樣會檢查 DetailsView 的智慧標籤的 啟用分頁核取方塊。</span><span class="sxs-lookup"><span data-stu-id="92630-135">Do so by checking the Enable Paging checkbox in the DetailsView's smart tag.</span></span>


<span data-ttu-id="92630-136">[![請檢查 DetailsView 的智慧標籤中啟用分頁核取方塊](custom-formatting-based-upon-data-cs/_static/image2.png)](custom-formatting-based-upon-data-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="92630-136">[![Check the Enable Paging Checkbox in the DetailsView's Smart Tag](custom-formatting-based-upon-data-cs/_static/image2.png)](custom-formatting-based-upon-data-cs/_static/image1.png)</span></span>

<span data-ttu-id="92630-137">**圖 1**： 檢查啟用分頁中的核取 DetailsView 的智慧標籤 ([按一下以檢視完整大小的影像](custom-formatting-based-upon-data-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="92630-137">**Figure 1**: Check the Enable Paging Checkbox in the DetailsView's Smart Tag ([Click to view full-size image](custom-formatting-based-upon-data-cs/_static/image3.png))</span></span>


<span data-ttu-id="92630-138">這些變更之後，請將 DetailsView 標記：</span><span class="sxs-lookup"><span data-stu-id="92630-138">After these changes, the DetailsView markup will be:</span></span>


[!code-aspx[Main](custom-formatting-based-upon-data-cs/samples/sample1.aspx)]

<span data-ttu-id="92630-139">請花一點時間來測試您的瀏覽器中的此頁面。</span><span class="sxs-lookup"><span data-stu-id="92630-139">Take a moment to test out this page in your browser.</span></span>


<span data-ttu-id="92630-140">[![在 DetailsView 控制項一次顯示一個產品](custom-formatting-based-upon-data-cs/_static/image5.png)](custom-formatting-based-upon-data-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="92630-140">[![The DetailsView Control Displays One Product at a Time](custom-formatting-based-upon-data-cs/_static/image5.png)](custom-formatting-based-upon-data-cs/_static/image4.png)</span></span>

<span data-ttu-id="92630-141">**圖 2**: DetailsView 控制項顯示一個產品一次 ([按一下以檢視完整大小的影像](custom-formatting-based-upon-data-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="92630-141">**Figure 2**: The DetailsView Control Displays One Product at a Time ([Click to view full-size image](custom-formatting-based-upon-data-cs/_static/image6.png))</span></span>


## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a><span data-ttu-id="92630-142">步驟 2： 以程式設計方式判斷此資料繫結事件處理常式中的資料值</span><span class="sxs-lookup"><span data-stu-id="92630-142">Step 2: Programmatically Determining the Value of the Data in the DataBound Event Handler</span></span>

<span data-ttu-id="92630-143">若要顯示的價格以粗體、 斜體字型為這些產品的`UnitPrice`值超過 $75.00，我們必須要先能以程式設計方式判斷`UnitPrice`值。</span><span class="sxs-lookup"><span data-stu-id="92630-143">In order to display the price in a bold, italic font for those products whose `UnitPrice` value exceeds $75.00, we need to first be able to programmatically determine the `UnitPrice` value.</span></span> <span data-ttu-id="92630-144">為 DetailsView，這可以透過完成`DataBound`事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="92630-144">For the DetailsView, this can be accomplished in the `DataBound` event handler.</span></span> <span data-ttu-id="92630-145">若要建立事件處理常式會 DetailsView 設計工具中按一下，然後瀏覽至 [屬性] 視窗。</span><span class="sxs-lookup"><span data-stu-id="92630-145">To create the event handler click on the DetailsView in the Designer then navigate to the Properties window.</span></span> <span data-ttu-id="92630-146">按 F4，即可啟動，如果不是可見的或移至 [檢視] 功能表，然後選取 [屬性] 視窗的功能表選項。</span><span class="sxs-lookup"><span data-stu-id="92630-146">Press F4 to bring it up, if it's not visible, or go to the View menu and select the Properties Window menu option.</span></span> <span data-ttu-id="92630-147">屬性 視窗中，按一下 閃電圖示來列出在 DetailsView 的事件。</span><span class="sxs-lookup"><span data-stu-id="92630-147">From the Properties window, click on the lightning bolt icon to list the DetailsView's events.</span></span> <span data-ttu-id="92630-148">接下來，按兩下`DataBound`事件或輸入您想要建立此事件處理常式的名稱。</span><span class="sxs-lookup"><span data-stu-id="92630-148">Next, either double-click the `DataBound` event or type in the name of the event handler you want to create.</span></span>


![資料繫結事件建立事件處理常式](custom-formatting-based-upon-data-cs/_static/image7.png)

<span data-ttu-id="92630-150">**圖 3**： 建立事件處理常式`DataBound`事件</span><span class="sxs-lookup"><span data-stu-id="92630-150">**Figure 3**: Create an Event Handler for the `DataBound` Event</span></span>


<span data-ttu-id="92630-151">這樣會自動建立事件處理常式，並帶您前往的程式碼部分加入其中。</span><span class="sxs-lookup"><span data-stu-id="92630-151">Doing so will automatically create the event handler and take you to the code portion where it has been added.</span></span> <span data-ttu-id="92630-152">此時您會看到：</span><span class="sxs-lookup"><span data-stu-id="92630-152">At this point you will see:</span></span>


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample2.cs)]

<span data-ttu-id="92630-153">在 detailsview 繫結的資料可以透過存取`DataItem`屬性。</span><span class="sxs-lookup"><span data-stu-id="92630-153">The data bound to the DetailsView can be accessed via the `DataItem` property.</span></span> <span data-ttu-id="92630-154">提醒您，我們會將我們將控制項繫結至強型別 DataTable，其中強型別 DataRow 執行個體的集合所組成。</span><span class="sxs-lookup"><span data-stu-id="92630-154">Recall that we are binding our controls to a strongly-typed DataTable, which is composed of a collection of strongly-typed DataRow instances.</span></span> <span data-ttu-id="92630-155">在 DataTable 中的第一個 DataRow DataTable 繫結至 DetailsView，指派給 DetailsView 的`DataItem`屬性。</span><span class="sxs-lookup"><span data-stu-id="92630-155">When the DataTable is bound to the DetailsView, the first DataRow in the DataTable is assigned to the DetailsView's `DataItem` property.</span></span> <span data-ttu-id="92630-156">具體來說，`DataItem`屬性會被指派`DataRowView`物件。</span><span class="sxs-lookup"><span data-stu-id="92630-156">Specifically, the `DataItem` property is assigned a `DataRowView` object.</span></span> <span data-ttu-id="92630-157">我們可以使用`DataRowView`的`Row`屬性來存取基礎的 DataRow 物件是實際上`ProductsRow`執行個體。</span><span class="sxs-lookup"><span data-stu-id="92630-157">We can use the `DataRowView`'s `Row` property to get access to the underlying DataRow object, which is actually a `ProductsRow` instance.</span></span> <span data-ttu-id="92630-158">一旦我們有此`ProductsRow`我們就可以決定我們只要檢查物件的屬性值的執行個體。</span><span class="sxs-lookup"><span data-stu-id="92630-158">Once we have this `ProductsRow` instance we can make our decision by simply inspecting the object's property values.</span></span>

<span data-ttu-id="92630-159">下列程式碼說明如何判斷是否`UnitPrice`DetailsView 控制項繫結的值大於 $75.00:</span><span class="sxs-lookup"><span data-stu-id="92630-159">The following code illustrates how to determine whether the `UnitPrice` value bound to the DetailsView control is greater than $75.00:</span></span>


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample3.cs)]

> [!NOTE]
> <span data-ttu-id="92630-160">因為`UnitPrice`可以有`NULL`值在資料庫中，我們會先檢查以確定我們正在未處理的`NULL`前存取值`ProductsRow`的`UnitPrice`屬性。</span><span class="sxs-lookup"><span data-stu-id="92630-160">Since `UnitPrice` can have a `NULL` value in the database, we first check to make sure that we're not dealing with a `NULL` value before accessing the `ProductsRow`'s `UnitPrice` property.</span></span> <span data-ttu-id="92630-161">這項檢查，請務必因為如果我們嘗試存取`UnitPrice`屬性時，它有`NULL`值`ProductsRow`物件將會擲回[StrongTypingException 例外狀況](https://msdn.microsoft.com/en-us/library/system.data.strongtypingexception.aspx)。</span><span class="sxs-lookup"><span data-stu-id="92630-161">This check is important because if we attempt to access the `UnitPrice` property when it has a `NULL` value the `ProductsRow` object will throw a [StrongTypingException exception](https://msdn.microsoft.com/en-us/library/system.data.strongtypingexception.aspx).</span></span>


## <a name="step-3-formatting-the-unitprice-value-in-the-detailsview"></a><span data-ttu-id="92630-162">步驟 3： 格式化 UnitPrice 值 DetailsView 中</span><span class="sxs-lookup"><span data-stu-id="92630-162">Step 3: Formatting the UnitPrice Value in the DetailsView</span></span>

<span data-ttu-id="92630-163">此時我們可以判斷是否`UnitPrice`繫結至 DetailsView 值超過 $75.00，但我們尚未以了解如何以程式設計方式調整 DetailsView 的據以格式化。</span><span class="sxs-lookup"><span data-stu-id="92630-163">At this point we can determine whether the `UnitPrice` value bound to the DetailsView has a value that exceeds $75.00, but we've yet to see how to programmatically adjust the DetailsView's formatting accordingly.</span></span> <span data-ttu-id="92630-164">若要修改之格式設定的 DetailsView 中整個資料列，以程式設計方式存取資料列使用`DetailsViewID.Rows[index]`; 若要修改特定的資料格，存取使用`DetailsViewID.Rows[index].Cells[index]`。</span><span class="sxs-lookup"><span data-stu-id="92630-164">To modify the formatting of an entire row in the DetailsView, programmatically access the row using `DetailsViewID.Rows[index]`; to modify a particular cell, access use `DetailsViewID.Rows[index].Cells[index]`.</span></span> <span data-ttu-id="92630-165">一旦資料列或資料格的參考我們就可以設定其樣式相關屬性，然後調整其外觀。</span><span class="sxs-lookup"><span data-stu-id="92630-165">Once we have a reference to the row or cell we can then adjust its appearance by setting its style-related properties.</span></span>

<span data-ttu-id="92630-166">您必須了解資料列的索引，從 0 開始，以程式設計方式存取資料列。</span><span class="sxs-lookup"><span data-stu-id="92630-166">Accessing a row programmatically requires that you know the row's index, which starts at 0.</span></span> <span data-ttu-id="92630-167">`UnitPrice`資料列會提供 4 個索引，並且讓它成為以程式設計方式存取 DetailsView 中的第五個資料列使用`ExpensiveProductsPriceInBoldItalic.Rows[4]`。</span><span class="sxs-lookup"><span data-stu-id="92630-167">The `UnitPrice` row is the fifth row in the DetailsView, giving it an index of 4 and making it programmatically accessible using `ExpensiveProductsPriceInBoldItalic.Rows[4]`.</span></span> <span data-ttu-id="92630-168">此時，我們可能使用下列程式碼顯示粗體、 斜體字型中的整個資料列內容：</span><span class="sxs-lookup"><span data-stu-id="92630-168">At this point we could have the entire row's content displayed in a bold, italic font by using the following code:</span></span>


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample4.cs)]

<span data-ttu-id="92630-169">不過，這會讓*兩者*標籤 （價格） 和粗體和斜體的值。</span><span class="sxs-lookup"><span data-stu-id="92630-169">However, this will make *both* the label (Price) and the value bold and italic.</span></span> <span data-ttu-id="92630-170">如果我們想要只值粗體和斜體我們要將此套用的第二個資料格中的資料列，即可使用下列格式：</span><span class="sxs-lookup"><span data-stu-id="92630-170">If we want to make just the value bold and italic we need to apply this formatting to the second cell in the row, which can be accomplished using the following:</span></span>


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample5.cs)]

<span data-ttu-id="92630-171">由於教學課程到目前為止已使用的樣式表維持清楚分隔呈現的標記和樣式的相關資訊，而不是設定特定的樣式屬性，如上所示我們改為使用 CSS 類別。</span><span class="sxs-lookup"><span data-stu-id="92630-171">Since our tutorials thus far have used stylesheets to maintain a clean separation between the rendered markup and style-related information, rather than setting the specific style properties as shown above let's instead use a CSS class.</span></span> <span data-ttu-id="92630-172">開啟`Styles.css`樣式表和加入新的 CSS 類別，名為`ExpensivePriceEmphasis`具有下列定義：</span><span class="sxs-lookup"><span data-stu-id="92630-172">Open the `Styles.css` stylesheet and add a new CSS class named `ExpensivePriceEmphasis` with the following definition:</span></span>


[!code-css[Main](custom-formatting-based-upon-data-cs/samples/sample6.css)]

<span data-ttu-id="92630-173">然後，在`DataBound`事件處理常式中，設定儲存格的`CssClass`屬性`ExpensivePriceEmphasis`。</span><span class="sxs-lookup"><span data-stu-id="92630-173">Then, in the `DataBound` event handler, set the cell's `CssClass` property to `ExpensivePriceEmphasis`.</span></span> <span data-ttu-id="92630-174">下列程式碼會示範`DataBound`完整的事件處理常式：</span><span class="sxs-lookup"><span data-stu-id="92630-174">The following code shows the `DataBound` event handler in its entirety:</span></span>


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample7.cs)]

<span data-ttu-id="92630-175">當檢視 Chai，成本小於 $75.00，價格會以一般字型顯示 （請參閱圖 4）。</span><span class="sxs-lookup"><span data-stu-id="92630-175">When viewing Chai, which costs less than $75.00, the price is displayed in a normal font (see Figure 4).</span></span> <span data-ttu-id="92630-176">不過，當檢視 Mishi Kobe Niku 有價格為 $97.00，價格會顯示在粗體、 斜體字型 （請參閱圖 5）。</span><span class="sxs-lookup"><span data-stu-id="92630-176">However, when viewing Mishi Kobe Niku, which has a price of $97.00, the price is displayed in a bold, italic font (see Figure 5).</span></span>


<span data-ttu-id="92630-177">[![價格小於 $75.00 會以一般字型顯示](custom-formatting-based-upon-data-cs/_static/image9.png)](custom-formatting-based-upon-data-cs/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="92630-177">[![Prices Less than $75.00 are Displayed in a Normal Font](custom-formatting-based-upon-data-cs/_static/image9.png)](custom-formatting-based-upon-data-cs/_static/image8.png)</span></span>

<span data-ttu-id="92630-178">**圖 4**： 價格小於 $75.00 會以一般字型顯示 ([按一下以檢視完整大小的影像](custom-formatting-based-upon-data-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="92630-178">**Figure 4**: Prices Less than $75.00 are Displayed in a Normal Font ([Click to view full-size image](custom-formatting-based-upon-data-cs/_static/image10.png))</span></span>


<span data-ttu-id="92630-179">[![昂貴的產品價格會顯示在粗體、 斜體字型](custom-formatting-based-upon-data-cs/_static/image12.png)](custom-formatting-based-upon-data-cs/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="92630-179">[![Expensive Products' Prices are Displayed in a Bold, Italic Font](custom-formatting-based-upon-data-cs/_static/image12.png)](custom-formatting-based-upon-data-cs/_static/image11.png)</span></span>

<span data-ttu-id="92630-180">**圖 5**： 昂貴的產品價格會顯示在粗體、 斜體字型 ([按一下以檢視完整大小的影像](custom-formatting-based-upon-data-cs/_static/image13.png))</span><span class="sxs-lookup"><span data-stu-id="92630-180">**Figure 5**: Expensive Products' Prices are Displayed in a Bold, Italic Font ([Click to view full-size image](custom-formatting-based-upon-data-cs/_static/image13.png))</span></span>


## <a name="using-the-formview-controlsdataboundevent-handler"></a><span data-ttu-id="92630-181">使用 FormView 控制項`DataBound`事件處理常式</span><span class="sxs-lookup"><span data-stu-id="92630-181">Using the FormView Control's`DataBound`Event Handler</span></span>

<span data-ttu-id="92630-182">決定繫結至在 FormView 的基礎資料的步驟都與建立 DetailsView`DataBound`事件處理常式，轉換`DataItem`適當的物件類型的屬性繫結至控制項，並判斷要如何繼續執行。</span><span class="sxs-lookup"><span data-stu-id="92630-182">The steps for determining the underlying data bound to a FormView are identical to those for a DetailsView create a `DataBound` event handler, cast the `DataItem` property to the appropriate object type bound to the control, and determine how to proceed.</span></span> <span data-ttu-id="92630-183">在 FormView 和 DetailsView 有所不同，不過，在其使用者介面的外觀更新的方式。</span><span class="sxs-lookup"><span data-stu-id="92630-183">The FormView and DetailsView differ, however, in how their user interface's appearance is updated.</span></span>

<span data-ttu-id="92630-184">在 FormView 不包含任何 BoundFields，因此缺少`Rows`集合。</span><span class="sxs-lookup"><span data-stu-id="92630-184">The FormView does not contain any BoundFields and therefore lacks the `Rows` collection.</span></span> <span data-ttu-id="92630-185">相反地，在 FormView 組成的範本，其中可以包含靜態 HTML 混用，Web 控制項和資料繫結語法。</span><span class="sxs-lookup"><span data-stu-id="92630-185">Instead, a FormView is composed of templates, which can contain a mix of static HTML, Web controls, and databinding syntax.</span></span> <span data-ttu-id="92630-186">調整在 FormView 的樣式通常牽涉到調整一個或多個在 FormView 的範本中的 Web 控制項的樣式。</span><span class="sxs-lookup"><span data-stu-id="92630-186">Adjusting the style of a FormView typically involves adjusting the style of one or more of the Web controls within the FormView's templates.</span></span>

<span data-ttu-id="92630-187">為了說明這點，讓我們使用產品清單 FormView 如同上述範例中，但這次我們顯示產品名稱和單位庫存顯示紅色字型，如果它小於或等於 10 的庫存單位。</span><span class="sxs-lookup"><span data-stu-id="92630-187">To illustrate this, let's use a FormView to list products like in the previous example, but this time let's display just the product name and units in stock with the units in stock displayed in a red font if it is less than or equal to 10.</span></span>

## <a name="step-4-displaying-the-product-information-in-a-formview"></a><span data-ttu-id="92630-188">步驟 4： 在 FormView 中顯示的產品資訊。</span><span class="sxs-lookup"><span data-stu-id="92630-188">Step 4: Displaying the Product Information in a FormView</span></span>

<span data-ttu-id="92630-189">新增 FormView`CustomColors.aspx`頁面下方的 DetailsView 和設定其`ID`屬性`LowStockedProductsInRed`。</span><span class="sxs-lookup"><span data-stu-id="92630-189">Add a FormView to the `CustomColors.aspx` page beneath the DetailsView and set its `ID` property to `LowStockedProductsInRed`.</span></span> <span data-ttu-id="92630-190">將在 FormView 的繫結至從上一個步驟建立 ObjectDataSource 控制項。</span><span class="sxs-lookup"><span data-stu-id="92630-190">Bind the FormView to the ObjectDataSource control created from the previous step.</span></span> <span data-ttu-id="92630-191">這會建立`ItemTemplate`， `EditItemTemplate`，和`InsertItemTemplate`在 FormView 的。</span><span class="sxs-lookup"><span data-stu-id="92630-191">This will create an `ItemTemplate`, `EditItemTemplate`, and `InsertItemTemplate` for the FormView.</span></span> <span data-ttu-id="92630-192">移除`EditItemTemplate`和`InsertItemTemplate`並簡化`ItemTemplate`包含只`ProductName`和`UnitsInStock`值，每個自己適當命名的 Label 控制項。</span><span class="sxs-lookup"><span data-stu-id="92630-192">Remove the `EditItemTemplate` and `InsertItemTemplate` and simplify the `ItemTemplate` to include just the `ProductName` and `UnitsInStock` values, each in their own appropriately-named Label controls.</span></span> <span data-ttu-id="92630-193">如同在 DetailsView 從先前的範例，也請檢查在 FormView 的智慧標籤的 啟用分頁核取方塊。</span><span class="sxs-lookup"><span data-stu-id="92630-193">As with the DetailsView from the earlier example, also check the Enable Paging checkbox in the FormView's smart tag.</span></span>

<span data-ttu-id="92630-194">之後這些編輯 FormView 的標記看起來應該如下所示：</span><span class="sxs-lookup"><span data-stu-id="92630-194">After these edits your FormView's markup should look similar to the following:</span></span>


[!code-aspx[Main](custom-formatting-based-upon-data-cs/samples/sample8.aspx)]

<span data-ttu-id="92630-195">請注意，`ItemTemplate`包含：</span><span class="sxs-lookup"><span data-stu-id="92630-195">Note that the `ItemTemplate` contains:</span></span>

- <span data-ttu-id="92630-196">**靜態 HTML**文字 「 產品:"和"庫存單位:"連同`<br />`和`<b>`項目。</span><span class="sxs-lookup"><span data-stu-id="92630-196">**Static HTML** the text "Product:" and "Units In Stock:" along with the `<br />` and `<b>` elements.</span></span>
- <span data-ttu-id="92630-197">**Web 控制項**兩個 Label 控制項，`ProductNameLabel`和`UnitsInStockLabel`。</span><span class="sxs-lookup"><span data-stu-id="92630-197">**Web controls** the two Label controls, `ProductNameLabel` and `UnitsInStockLabel`.</span></span>
- <span data-ttu-id="92630-198">**資料繫結語法**`<%# Bind("ProductName") %>`和`<%# Bind("UnitsInStock") %>`語法，從這些欄位的值指定給 Label 控制項`Text`屬性。</span><span class="sxs-lookup"><span data-stu-id="92630-198">**Databinding syntax** the `<%# Bind("ProductName") %>` and `<%# Bind("UnitsInStock") %>` syntax, which assigns the values from these fields to the Label controls' `Text` properties.</span></span>

## <a name="step-5-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a><span data-ttu-id="92630-199">步驟 5： 以程式設計方式判斷此資料繫結事件處理常式中的資料值</span><span class="sxs-lookup"><span data-stu-id="92630-199">Step 5: Programmatically Determining the Value of the Data in the DataBound Event Handler</span></span>

<span data-ttu-id="92630-200">在 FormView 的標記完成下, 一個步驟是以程式設計方式判斷是否`UnitsInStock`值小於或等於 10。</span><span class="sxs-lookup"><span data-stu-id="92630-200">With the FormView's markup complete, the next step is to programmatically determine if the `UnitsInStock` value is less than or equal to 10.</span></span> <span data-ttu-id="92630-201">這是與 DetailsView 達成完全相同的方式與在 FormView 中。</span><span class="sxs-lookup"><span data-stu-id="92630-201">This is accomplished in the exact same manner with the FormView as it was with the DetailsView.</span></span> <span data-ttu-id="92630-202">從建立在 FormView 的事件處理常式開始`DataBound`事件。</span><span class="sxs-lookup"><span data-stu-id="92630-202">Start by creating an event handler for the FormView's `DataBound` event.</span></span>


![建立資料繫結事件處理常式](custom-formatting-based-upon-data-cs/_static/image14.png)

<span data-ttu-id="92630-204">**圖 6**： 建立`DataBound`事件處理常式</span><span class="sxs-lookup"><span data-stu-id="92630-204">**Figure 6**: Create the `DataBound` Event Handler</span></span>


<span data-ttu-id="92630-205">在事件處理常式轉換在 FormView 的`DataItem`屬性`ProductsRow`執行個體，並判斷是否`UnitsInPrice`值，這類我們需要顯示紅色字型。</span><span class="sxs-lookup"><span data-stu-id="92630-205">In the event handler cast the FormView's `DataItem` property to a `ProductsRow` instance and determine whether the `UnitsInPrice` value is such that we need to display it in a red font.</span></span>


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample9.cs)]

## <a name="step-6-formatting-the-unitsinstocklabel-label-control-in-the-formviews-itemtemplate"></a><span data-ttu-id="92630-206">步驟 6： 格式化在 FormView 的 ItemTemplate 的 UnitsInStockLabel 標籤控制項</span><span class="sxs-lookup"><span data-stu-id="92630-206">Step 6: Formatting the UnitsInStockLabel Label Control in the FormView's ItemTemplate</span></span>

<span data-ttu-id="92630-207">最後一個步驟是格式化顯示`UnitsInStock`紅色字型中的值如果值為 10 或更少。</span><span class="sxs-lookup"><span data-stu-id="92630-207">The final step is to format the displayed `UnitsInStock` value in a red font if the value is 10 or less.</span></span> <span data-ttu-id="92630-208">若要完成此我們需要以程式設計方式存取`UnitsInStockLabel`控制`ItemTemplate`並設定其樣式屬性，使其文字會以紅色顯示。</span><span class="sxs-lookup"><span data-stu-id="92630-208">To accomplish this we need to programmatically access the `UnitsInStockLabel` control in the `ItemTemplate` and set its style properties so that its text is displayed in red.</span></span> <span data-ttu-id="92630-209">若要存取的 Web 控制項範本中，使用`FindControl("controlID")`方法如下：</span><span class="sxs-lookup"><span data-stu-id="92630-209">To access a Web control in a template, use the `FindControl("controlID")` method like this:</span></span>


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample10.cs)]

<span data-ttu-id="92630-210">此範例中我們想要存取標籤控制項`ID`值是`UnitsInStockLabel`，因此我們會使用：</span><span class="sxs-lookup"><span data-stu-id="92630-210">For our example we want to access a Label control whose `ID` value is `UnitsInStockLabel`, so we'd use:</span></span>


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample11.cs)]

<span data-ttu-id="92630-211">一旦 Web 控制項的程式設計參考，我們可以視需要修改它的樣式相關屬性。</span><span class="sxs-lookup"><span data-stu-id="92630-211">Once we have a programmatic reference to the Web control, we can modify its style-related properties as needed.</span></span> <span data-ttu-id="92630-212">如先前範例中，我建立了中的 CSS 類別`Styles.css`名為`LowUnitsInStockEmphasis`。</span><span class="sxs-lookup"><span data-stu-id="92630-212">As with the earlier example, I've created a CSS class in `Styles.css` named `LowUnitsInStockEmphasis`.</span></span> <span data-ttu-id="92630-213">若要套用此樣式的標籤 Web 控制項，設定其`CssClass`屬性據此。</span><span class="sxs-lookup"><span data-stu-id="92630-213">To apply this style to the Label Web control, set its `CssClass` property accordingly.</span></span>


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample12.cs)]

> [!NOTE]
> <span data-ttu-id="92630-214">格式設定範本，以程式設計方式存取 Web 控制項使用的語法`FindControl("controlID")`，然後設定其樣式相關屬性也可使用時[TemplateFields](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.templatefield(VS.80).aspx) DetailsView 或 GridView 中控制項。</span><span class="sxs-lookup"><span data-stu-id="92630-214">The syntax for formatting a template programmatically accessing the Web control using `FindControl("controlID")` and then setting its style-related properties can also be used when using [TemplateFields](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.templatefield(VS.80).aspx) in the DetailsView or GridView controls.</span></span> <span data-ttu-id="92630-215">在我們的下一個教學課程中，我們將檢驗 TemplateFields。</span><span class="sxs-lookup"><span data-stu-id="92630-215">We'll examine TemplateFields in our next tutorial.</span></span>


<span data-ttu-id="92630-216">圖 7 顯示 FormView，檢視產品時其`UnitsInStock`值大於 10，而圖 8 中的產品有它的值小於 10。</span><span class="sxs-lookup"><span data-stu-id="92630-216">Figures 7 shows the FormView when viewing a product whose `UnitsInStock` value is greater than 10, while the product in Figure 8 has its value less than 10.</span></span>


<span data-ttu-id="92630-217">[![針對產品與夠大 庫存單位，套用無自訂格式](custom-formatting-based-upon-data-cs/_static/image16.png)](custom-formatting-based-upon-data-cs/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="92630-217">[![For Products With a Sufficiently Large Units In Stock, No Custom Formatting is Applied](custom-formatting-based-upon-data-cs/_static/image16.png)](custom-formatting-based-upon-data-cs/_static/image15.png)</span></span>

<span data-ttu-id="92630-218">**圖 7**： 套用的產品與夠大 庫存單位中，沒有自訂格式 ([按一下以檢視完整大小的影像](custom-formatting-based-upon-data-cs/_static/image17.png))</span><span class="sxs-lookup"><span data-stu-id="92630-218">**Figure 7**: For Products With a Sufficiently Large Units In Stock, No Custom Formatting is Applied ([Click to view full-size image](custom-formatting-based-upon-data-cs/_static/image17.png))</span></span>


<span data-ttu-id="92630-219">[![庫存數字的單位數以紅色顯示的那些產品 With Values 10 倍或更少](custom-formatting-based-upon-data-cs/_static/image19.png)](custom-formatting-based-upon-data-cs/_static/image18.png)</span><span class="sxs-lookup"><span data-stu-id="92630-219">[![The Units in Stock Number is Shown in Red for Those Products With Values of 10 or Less](custom-formatting-based-upon-data-cs/_static/image19.png)](custom-formatting-based-upon-data-cs/_static/image18.png)</span></span>

<span data-ttu-id="92630-220">**圖 8**: 單位庫存數字以紅色顯示的那些產品 With Values 10 倍或更少 ([按一下以檢視完整大小的影像](custom-formatting-based-upon-data-cs/_static/image20.png))</span><span class="sxs-lookup"><span data-stu-id="92630-220">**Figure 8**: The Units in Stock Number is Shown in Red for Those Products With Values of 10 or Less ([Click to view full-size image](custom-formatting-based-upon-data-cs/_static/image20.png))</span></span>


## <a name="formatting-with-the-gridviewsrowdataboundevent"></a><span data-ttu-id="92630-221">格式設定與 GridView`RowDataBound`事件</span><span class="sxs-lookup"><span data-stu-id="92630-221">Formatting with the GridView's`RowDataBound`Event</span></span>

<span data-ttu-id="92630-222">稍早我們檢查 DetailsView 步驟的順序和 FormView 控制項進行資料繫結期間。</span><span class="sxs-lookup"><span data-stu-id="92630-222">Earlier we examined the sequence of steps the DetailsView and FormView controls progress through during databinding.</span></span> <span data-ttu-id="92630-223">讓我們透過下列步驟再次為重新整理程式。</span><span class="sxs-lookup"><span data-stu-id="92630-223">Let's look over these steps once again as a refresher.</span></span>

1. <span data-ttu-id="92630-224">資料 Web 控制項`DataBinding`事件引發。</span><span class="sxs-lookup"><span data-stu-id="92630-224">The data Web control's `DataBinding` event fires.</span></span>
2. <span data-ttu-id="92630-225">資料繫結至 Web 控制項的資料。</span><span class="sxs-lookup"><span data-stu-id="92630-225">The data is bound to the data Web control.</span></span>
3. <span data-ttu-id="92630-226">資料 Web 控制項`DataBound`事件引發。</span><span class="sxs-lookup"><span data-stu-id="92630-226">The data Web control's `DataBound` event fires.</span></span>

<span data-ttu-id="92630-227">這三個簡單步驟已足夠的 DetailsView 和 FormView，因為它們會顯示單一記錄。</span><span class="sxs-lookup"><span data-stu-id="92630-227">These three simple steps are sufficient for the DetailsView and FormView because they display only a single record.</span></span> <span data-ttu-id="92630-228">Gridview，其中顯示*所有*記錄繫結它 （不只是第一個），步驟 2 會稍微更為複雜。</span><span class="sxs-lookup"><span data-stu-id="92630-228">For the GridView, which displays *all* records bound to it (not just the first), step 2 is a bit more involved.</span></span>

<span data-ttu-id="92630-229">在步驟的 2 GridView 會列舉資料來源，以及每個記錄，建立`GridViewRow`執行個體，並繫結到它的目前記錄。</span><span class="sxs-lookup"><span data-stu-id="92630-229">In step 2 the GridView enumerates the data source and, for each record, creates a `GridViewRow` instance and binds the current record to it.</span></span> <span data-ttu-id="92630-230">每個`GridViewRow`加入至 GridView，會引發兩個事件：</span><span class="sxs-lookup"><span data-stu-id="92630-230">For each `GridViewRow` added to the GridView, two events are raised:</span></span>

- <span data-ttu-id="92630-231">**`RowCreated`**之後，都會引發`GridViewRow`已建立</span><span class="sxs-lookup"><span data-stu-id="92630-231">**`RowCreated`** fires after the `GridViewRow` has been created</span></span>
- <span data-ttu-id="92630-232">**`RowDataBound`**目前的記錄繫結至後引發`GridViewRow`。</span><span class="sxs-lookup"><span data-stu-id="92630-232">**`RowDataBound`** fires after the current record has been bound to the `GridViewRow`.</span></span>

<span data-ttu-id="92630-233">Gridview，然後，資料繫結是更精確地分為下列一連串步驟：</span><span class="sxs-lookup"><span data-stu-id="92630-233">For the GridView, then, data binding is more accurately described by the following sequence of steps:</span></span>

1. <span data-ttu-id="92630-234">GridView`DataBinding`事件引發。</span><span class="sxs-lookup"><span data-stu-id="92630-234">The GridView's `DataBinding` event fires.</span></span>
2. <span data-ttu-id="92630-235">資料繫結至 GridView。</span><span class="sxs-lookup"><span data-stu-id="92630-235">The data is bound to the GridView.</span></span>   
  
 <span data-ttu-id="92630-236">資料來源中的每一筆記錄</span><span class="sxs-lookup"><span data-stu-id="92630-236">For each record in the data source</span></span> 

    1. <span data-ttu-id="92630-237">建立`GridViewRow`物件</span><span class="sxs-lookup"><span data-stu-id="92630-237">Create a `GridViewRow` object</span></span>
    2. <span data-ttu-id="92630-238">引發`RowCreated`事件</span><span class="sxs-lookup"><span data-stu-id="92630-238">Fire the `RowCreated` event</span></span>
    3. <span data-ttu-id="92630-239">繫結至資料錄`GridViewRow`</span><span class="sxs-lookup"><span data-stu-id="92630-239">Bind the record to the `GridViewRow`</span></span>
    4. <span data-ttu-id="92630-240">引發`RowDataBound`事件</span><span class="sxs-lookup"><span data-stu-id="92630-240">Fire the `RowDataBound` event</span></span>
    5. <span data-ttu-id="92630-241">新增`GridViewRow`至`Rows`集合</span><span class="sxs-lookup"><span data-stu-id="92630-241">Add the `GridViewRow` to the `Rows` collection</span></span>
3. <span data-ttu-id="92630-242">GridView`DataBound`事件引發。</span><span class="sxs-lookup"><span data-stu-id="92630-242">The GridView's `DataBound` event fires.</span></span>

<span data-ttu-id="92630-243">若要自訂的 GridView 的個別記錄格式，然後，我們必須建立事件處理常式`RowDataBound`事件。</span><span class="sxs-lookup"><span data-stu-id="92630-243">To customize the format of the GridView's individual records, then, we need to create an event handler for the `RowDataBound` event.</span></span> <span data-ttu-id="92630-244">為了說明這點，讓我們加入 GridView`CustomColors.aspx`列出名稱、 類別目錄，以及這些產品的價格是不超過 $10.00 以黃色背景的色彩反白顯示的每個產品價格的頁面。</span><span class="sxs-lookup"><span data-stu-id="92630-244">To illustrate this, let's add a GridView to the `CustomColors.aspx` page that lists the name, category, and price for each product, highlighting those products whose price is less than $10.00 with a yellow background color.</span></span>

## <a name="step-7-displaying-product-information-in-a-gridview"></a><span data-ttu-id="92630-245">步驟 7： 在 GridView 中顯示產品資訊</span><span class="sxs-lookup"><span data-stu-id="92630-245">Step 7: Displaying Product Information in a GridView</span></span>

<span data-ttu-id="92630-246">在 FormView 下方的 GridView 加入前一個範例並設定其`ID`屬性`HighlightCheapProducts`。</span><span class="sxs-lookup"><span data-stu-id="92630-246">Add a GridView beneath the FormView from the previous example and set its `ID` property to `HighlightCheapProducts`.</span></span> <span data-ttu-id="92630-247">因為我們已經在頁面上會傳回所有產品的 Sqldatasource，繫結的 GridView 的。</span><span class="sxs-lookup"><span data-stu-id="92630-247">Since we already have an ObjectDataSource that returns all products on the page, bind the GridView to that.</span></span> <span data-ttu-id="92630-248">最後，編輯 GridView 的 BoundFields 以包含剛才產品的名稱、 分類和價格。</span><span class="sxs-lookup"><span data-stu-id="92630-248">Finally, edit the GridView's BoundFields to include just the products' names, categories, and prices.</span></span> <span data-ttu-id="92630-249">之後這些編輯 GridView 標記看起來應該像：</span><span class="sxs-lookup"><span data-stu-id="92630-249">After these edits the GridView's markup should look like:</span></span>


[!code-aspx[Main](custom-formatting-based-upon-data-cs/samples/sample13.aspx)]

<span data-ttu-id="92630-250">圖 9 顯示我們透過瀏覽器檢視時，此點的進度。</span><span class="sxs-lookup"><span data-stu-id="92630-250">Figure 9 shows our progress to this point when viewed through a browser.</span></span>


<span data-ttu-id="92630-251">[![在 GridView 列出名稱、 類別和每項產品的價格](custom-formatting-based-upon-data-cs/_static/image22.png)](custom-formatting-based-upon-data-cs/_static/image21.png)</span><span class="sxs-lookup"><span data-stu-id="92630-251">[![The GridView Lists the Name, Category, and Price For Each Product](custom-formatting-based-upon-data-cs/_static/image22.png)](custom-formatting-based-upon-data-cs/_static/image21.png)</span></span>

<span data-ttu-id="92630-252">**圖 9**: GridView 列出名稱、 類別和每個產品的價格 ([按一下以檢視完整大小的影像](custom-formatting-based-upon-data-cs/_static/image23.png))</span><span class="sxs-lookup"><span data-stu-id="92630-252">**Figure 9**: The GridView Lists the Name, Category, and Price For Each Product ([Click to view full-size image](custom-formatting-based-upon-data-cs/_static/image23.png))</span></span>


## <a name="step-8-programmatically-determining-the-value-of-the-data-in-the-rowdatabound-event-handler"></a><span data-ttu-id="92630-253">步驟 8： 以程式設計方式判斷 RowDataBound 事件處理常式中的資料值</span><span class="sxs-lookup"><span data-stu-id="92630-253">Step 8: Programmatically Determining the Value of the Data in the RowDataBound Event Handler</span></span>

<span data-ttu-id="92630-254">當`ProductsDataTable`繫結至 GridView 其`ProductsRow`執行個體是列舉，以及對於每個`ProductsRow``GridViewRow`建立。</span><span class="sxs-lookup"><span data-stu-id="92630-254">When the `ProductsDataTable` is bound to the GridView its `ProductsRow` instances are enumerated and for each `ProductsRow` a `GridViewRow` is created.</span></span> <span data-ttu-id="92630-255">`GridViewRow`的`DataItem`屬性指派給特定`ProductRow`，其後 GridView`RowDataBound`事件處理常式，就會引發。</span><span class="sxs-lookup"><span data-stu-id="92630-255">The `GridViewRow`'s `DataItem` property is assigned to the particular `ProductRow`, after which the GridView's `RowDataBound` event handler is raised.</span></span> <span data-ttu-id="92630-256">若要判斷`UnitPrice`每項產品的值繫結至 GridView，則我們需要建立事件處理常式的 GridView`RowDataBound`事件。</span><span class="sxs-lookup"><span data-stu-id="92630-256">To determine the `UnitPrice` value for each product bound to the GridView, then, we need to create an event handler for the GridView's `RowDataBound` event.</span></span> <span data-ttu-id="92630-257">此事件處理常式中，我們可以檢查`UnitPrice`值目前`GridViewRow`做出格式化的決策，該資料列。</span><span class="sxs-lookup"><span data-stu-id="92630-257">In this event handler we can inspect the `UnitPrice` value for the current `GridViewRow` and make a formatting decision for that row.</span></span>

<span data-ttu-id="92630-258">可以使用相同的一系列步驟，做為 DetailsView FormView 與建立此事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="92630-258">This event handler can be created using the same series of steps as with the FormView and DetailsView.</span></span>


![GridView RowDataBound 事件建立事件處理常式](custom-formatting-based-upon-data-cs/_static/image24.png)

<span data-ttu-id="92630-260">**圖 10**： 建立事件處理常式的 GridView`RowDataBound`事件</span><span class="sxs-lookup"><span data-stu-id="92630-260">**Figure 10**: Create an Event Handler for the GridView's `RowDataBound` Event</span></span>


<span data-ttu-id="92630-261">以這種方式建立事件處理常式，將導致下列的程式碼會自動加入至 ASP.NET 網頁的程式碼部分：</span><span class="sxs-lookup"><span data-stu-id="92630-261">Creating the event hander in this manner will cause the following code to be automatically added to the ASP.NET page's code portion:</span></span>


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample14.cs)]

<span data-ttu-id="92630-262">當`RowDataBound`事件引發的事件處理常式會傳遞做為其第二個參數類型的物件`GridViewRowEventArgs`，其具有內容，名為`Row`。</span><span class="sxs-lookup"><span data-stu-id="92630-262">When the `RowDataBound` event fires, the event handler is passed as its second parameter an object of type `GridViewRowEventArgs`, which has a property named `Row`.</span></span> <span data-ttu-id="92630-263">這個屬性會傳回參考`GridViewRow`時只是資料繫結。</span><span class="sxs-lookup"><span data-stu-id="92630-263">This property returns a reference to the `GridViewRow` that was just data bound.</span></span> <span data-ttu-id="92630-264">若要存取`ProductsRow`執行個體繫結至`GridViewRow`我們使用`DataItem`屬性如下所示：</span><span class="sxs-lookup"><span data-stu-id="92630-264">To access the `ProductsRow` instance bound to the `GridViewRow` we use the `DataItem` property like so:</span></span>


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample15.cs)]

<span data-ttu-id="92630-265">當使用`RowDataBound`務必記住 GridView 組成不同類型的資料列，而且此事件針對引發的事件處理常式*所有*資料列型別。</span><span class="sxs-lookup"><span data-stu-id="92630-265">When working with the `RowDataBound` event handler it is important to keep in mind that the GridView is composed of different types of rows and that this event is fired for *all* row types.</span></span> <span data-ttu-id="92630-266">A`GridViewRow`的類型由其`RowType`屬性，且可以有一個可能的值：</span><span class="sxs-lookup"><span data-stu-id="92630-266">A `GridViewRow`'s type can be determined by its `RowType` property, and can have one of the possible values:</span></span>

- <span data-ttu-id="92630-267">`DataRow`繫結至資料錄從 GridView 的資料列`DataSource`</span><span class="sxs-lookup"><span data-stu-id="92630-267">`DataRow` a row that is bound to a record from the GridView's `DataSource`</span></span>
- <span data-ttu-id="92630-268">`EmptyDataRow`如果顯示的資料列的 GridView`DataSource`是空的</span><span class="sxs-lookup"><span data-stu-id="92630-268">`EmptyDataRow` the row displayed if the GridView's `DataSource` is empty</span></span>
- <span data-ttu-id="92630-269">`Footer`頁尾資料列。顯示的如果 GridView`ShowFooter`屬性設定為`true`</span><span class="sxs-lookup"><span data-stu-id="92630-269">`Footer` the footer row; shown if the GridView's `ShowFooter` property is set to `true`</span></span>
- <span data-ttu-id="92630-270">`Header`標頭資料列，顯示是否 GridView ShowHeader 屬性會設定為`true`（預設值）</span><span class="sxs-lookup"><span data-stu-id="92630-270">`Header` the header row; shown if the GridView's ShowHeader property is set to `true` (the default)</span></span>
- <span data-ttu-id="92630-271">`Pager`會實作的 GridView 的分頁，顯示分頁介面的資料列</span><span class="sxs-lookup"><span data-stu-id="92630-271">`Pager` for GridView's that implement paging, the row that displays the paging interface</span></span>
- <span data-ttu-id="92630-272">`Separator`不會用於 GridView，但使用`RowType`DataList 和中繼器內容控制項，兩個資料中，我們會討論在未來教學課程的 Web 控制項</span><span class="sxs-lookup"><span data-stu-id="92630-272">`Separator` not used for the GridView, but used by the `RowType` properties for the DataList and Repeater controls, two data Web controls we'll discuss in future tutorials</span></span>

<span data-ttu-id="92630-273">因為`EmptyDataRow`， `Header`， `Footer`，和`Pager`與相關聯的資料列不是`DataSource`記錄，它們一定會`null`值及其`DataItem`屬性。</span><span class="sxs-lookup"><span data-stu-id="92630-273">Since the `EmptyDataRow`, `Header`, `Footer`, and `Pager` rows aren't associated with a `DataSource` record, they will always have a `null` value for their `DataItem` property.</span></span> <span data-ttu-id="92630-274">基於這個原因，然後再嘗試將工作與目前`GridViewRow`的`DataItem`屬性，我們必須先確定我們正在處理的`DataRow`。</span><span class="sxs-lookup"><span data-stu-id="92630-274">For this reason, before attempting to work with the current `GridViewRow`'s `DataItem` property, we first must make sure that we're dealing with a `DataRow`.</span></span> <span data-ttu-id="92630-275">這可以透過檢查`GridViewRow`的`RowType`屬性如下所示：</span><span class="sxs-lookup"><span data-stu-id="92630-275">This can be accomplished by checking the `GridViewRow`'s `RowType` property like so:</span></span>


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample16.cs)]

## <a name="step-9-highlighting-the-row-yellow-when-the-unitprice-value-is-less-than-1000"></a><span data-ttu-id="92630-276">步驟 9： 反白顯示資料列黃色時 UnitPrice 值是不超過 $10.00</span><span class="sxs-lookup"><span data-stu-id="92630-276">Step 9: Highlighting the Row Yellow When the UnitPrice Value is Less than $10.00</span></span>

<span data-ttu-id="92630-277">最後一個步驟是以程式設計的方式反白顯示整個`GridViewRow`如果`UnitPrice`值該資料列是不超過 $10.00。</span><span class="sxs-lookup"><span data-stu-id="92630-277">The last step is to programmatically highlight the entire `GridViewRow` if the `UnitPrice` value for that row is less than $10.00.</span></span> <span data-ttu-id="92630-278">存取的 GridView 資料列或資料格的語法是如同 DetailsView`GridViewID.Rows[index]`存取整個資料列中，`GridViewID.Rows[index].Cells[index]`存取特定資料格。</span><span class="sxs-lookup"><span data-stu-id="92630-278">The syntax for accessing a GridView's rows or cells is the same as with the DetailsView `GridViewID.Rows[index]` to access the entire row, `GridViewID.Rows[index].Cells[index]` to access a particular cell.</span></span> <span data-ttu-id="92630-279">不過，當`RowDataBound`事件處理常式就會引發資料繫結`GridViewRow`尚未加入至 GridView`Rows`集合。</span><span class="sxs-lookup"><span data-stu-id="92630-279">However, when the `RowDataBound` event handler fires the data bound `GridViewRow` has yet to be added to the GridView's `Rows` collection.</span></span> <span data-ttu-id="92630-280">因此，您無法存取目前`GridViewRow`的執行個體`RowDataBound`使用資料列集合的事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="92630-280">Therefore you cannot access the current `GridViewRow` instance from the `RowDataBound` event handler using the Rows collection.</span></span>

<span data-ttu-id="92630-281">而不是`GridViewID.Rows[index]`，我們可以參考目前`GridViewRow`執行個體中`RowDataBound`事件處理常式使用`e.Row`。</span><span class="sxs-lookup"><span data-stu-id="92630-281">Instead of `GridViewID.Rows[index]`, we can reference the current `GridViewRow` instance in the `RowDataBound` event handler using `e.Row`.</span></span> <span data-ttu-id="92630-282">也就是為了反白顯示目前`GridViewRow`的執行個體`RowDataBound`我們要使用的事件處理常式：</span><span class="sxs-lookup"><span data-stu-id="92630-282">That is, in order to highlight the current `GridViewRow` instance from the `RowDataBound` event handler we would use:</span></span>


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample17.cs)]

<span data-ttu-id="92630-283">而非設定`GridViewRow`的`BackColor`屬性直接管理，讓我們堅持使用 CSS 類別。</span><span class="sxs-lookup"><span data-stu-id="92630-283">Rather than set the `GridViewRow`'s `BackColor` property directly, let's stick with using CSS classes.</span></span> <span data-ttu-id="92630-284">我已經建立名為的 CSS 類別`AffordablePriceEmphasis`所設定的背景色彩為黃色。</span><span class="sxs-lookup"><span data-stu-id="92630-284">I've created a CSS class named `AffordablePriceEmphasis` that sets the background color to yellow.</span></span> <span data-ttu-id="92630-285">已完成`RowDataBound`事件處理常式會遵循：</span><span class="sxs-lookup"><span data-stu-id="92630-285">The completed `RowDataBound` event handler follows:</span></span>


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample18.cs)]


<span data-ttu-id="92630-286">[![最合理的產品為黃色反白顯示](custom-formatting-based-upon-data-cs/_static/image26.png)](custom-formatting-based-upon-data-cs/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="92630-286">[![The Most Affordable Products are Highlighted Yellow](custom-formatting-based-upon-data-cs/_static/image26.png)](custom-formatting-based-upon-data-cs/_static/image25.png)</span></span>

<span data-ttu-id="92630-287">**圖 11**: 大部分價格合理的產品為黃色反白顯示 ([按一下以檢視完整大小的影像](custom-formatting-based-upon-data-cs/_static/image27.png))</span><span class="sxs-lookup"><span data-stu-id="92630-287">**Figure 11**: The Most Affordable Products are Highlighted Yellow ([Click to view full-size image](custom-formatting-based-upon-data-cs/_static/image27.png))</span></span>


## <a name="summary"></a><span data-ttu-id="92630-288">總結</span><span class="sxs-lookup"><span data-stu-id="92630-288">Summary</span></span>

<span data-ttu-id="92630-289">在本教學課程中，我們看到如何格式化 GridView、 DetailsView 和根據資料繫結至控制項的程式設計。</span><span class="sxs-lookup"><span data-stu-id="92630-289">In this tutorial we saw how to format the GridView, DetailsView, and FormView based on the data bound to the control.</span></span> <span data-ttu-id="92630-290">若要完成此我們建立的事件處理常式`DataBound`或`RowDataBound`其中基礎資料已檢查的格式設定的變更，以及視需要的事件。</span><span class="sxs-lookup"><span data-stu-id="92630-290">To accomplish this we created an event handler for the `DataBound` or `RowDataBound` events, where the underlying data was examined along with a formatting change, if needed.</span></span> <span data-ttu-id="92630-291">若要存取的資料繫結至 DetailsView 或 FormView，我們使用`DataItem`屬性`DataBound`事件處理常式; GridView，每個`GridViewRow`執行個體的`DataItem`屬性包含的資料繫結至該資料列，其可於`RowDataBound`事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="92630-291">To access the data bound to a DetailsView or FormView, we use the `DataItem` property in the `DataBound` event handler; for a GridView, each `GridViewRow` instance's `DataItem` property contains the data bound to that row, which is available in the `RowDataBound` event handler.</span></span>

<span data-ttu-id="92630-292">以程式設計方式調整資料 Web 控制項的格式設定的語法，取決於 Web 控制項和格式化資料的顯示方式。</span><span class="sxs-lookup"><span data-stu-id="92630-292">The syntax for programmatically adjusting the data Web control's formatting depends upon the Web control and how the data to be formatted is displayed.</span></span> <span data-ttu-id="92630-293">DetailsView 和 GridView 控制項、 資料列和資料格可以透過序數索引。</span><span class="sxs-lookup"><span data-stu-id="92630-293">For DetailsView and GridView controls, the rows and cells can be accessed by an ordinal index.</span></span> <span data-ttu-id="92630-294">FormView 中，會使用範本，如`FindControl("controlID")`方法通常用來找出範本內的 Web 控制項。</span><span class="sxs-lookup"><span data-stu-id="92630-294">For the FormView, which uses templates, the `FindControl("controlID")` method is commonly used to locate a Web control from within the template.</span></span>

<span data-ttu-id="92630-295">下一個教學課程中我們將探討如何使用 GridView 和 DetailsView 的範本。</span><span class="sxs-lookup"><span data-stu-id="92630-295">In the next tutorial we'll look at how to use templates with the GridView and DetailsView.</span></span> <span data-ttu-id="92630-296">此外，我們會看到另一個自訂的基礎資料的格式設定的技術。</span><span class="sxs-lookup"><span data-stu-id="92630-296">Additionally, we'll see another technique for customizing the formatting based on the underlying data.</span></span>

<span data-ttu-id="92630-297">祝您程式設計 ！</span><span class="sxs-lookup"><span data-stu-id="92630-297">Happy Programming!</span></span>

## <a name="about-the-author"></a><span data-ttu-id="92630-298">關於作者</span><span class="sxs-lookup"><span data-stu-id="92630-298">About the Author</span></span>

<span data-ttu-id="92630-299">[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。</span><span class="sxs-lookup"><span data-stu-id="92630-299">[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), author of seven ASP/ASP.NET books and founder of [4GuysFromRolla.com](http://www.4guysfromrolla.com), has been working with Microsoft Web technologies since 1998.</span></span> <span data-ttu-id="92630-300">Scott 可做為獨立顧問、 訓練和寫入器。</span><span class="sxs-lookup"><span data-stu-id="92630-300">Scott works as an independent consultant, trainer, and writer.</span></span> <span data-ttu-id="92630-301">他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。</span><span class="sxs-lookup"><span data-stu-id="92630-301">His latest book is [*Sams Teach Yourself ASP.NET 2.0 in 24 Hours*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco).</span></span> <span data-ttu-id="92630-302">他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。</span><span class="sxs-lookup"><span data-stu-id="92630-302">He can be reached at [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) or via his blog, which can be found at [http://ScottOnWriting.NET](http://ScottOnWriting.NET).</span></span>

## <a name="special-thanks-to"></a><span data-ttu-id="92630-303">特別感謝</span><span class="sxs-lookup"><span data-stu-id="92630-303">Special Thanks To</span></span>

<span data-ttu-id="92630-304">許多有用的檢閱者已檢閱本教學課程系列。</span><span class="sxs-lookup"><span data-stu-id="92630-304">This tutorial series was reviewed by many helpful reviewers.</span></span> <span data-ttu-id="92630-305">本教學課程總計 E.R.導致檢閱者</span><span class="sxs-lookup"><span data-stu-id="92630-305">Lead reviewers for this tutorial were E.R.</span></span> <span data-ttu-id="92630-306">Gilmore，Dennis Patterson 和 Dan Jagers。</span><span class="sxs-lookup"><span data-stu-id="92630-306">Gilmore, Dennis Patterson, and Dan Jagers.</span></span> <span data-ttu-id="92630-307">檢閱我即將推出的 MSDN 文件有興趣嗎？</span><span class="sxs-lookup"><span data-stu-id="92630-307">Interested in reviewing my upcoming MSDN articles?</span></span> <span data-ttu-id="92630-308">如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)</span><span class="sxs-lookup"><span data-stu-id="92630-308">If so, drop me a line at [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="92630-309">下一步</span><span class="sxs-lookup"><span data-stu-id="92630-309">Next</span></span>](using-templatefields-in-the-gridview-control-cs.md)
