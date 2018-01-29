---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs
title: "執行批次更新 (C#) |Microsoft 文件"
author: rick-anderson
description: "了解如何建立完全可編輯 DataList 其中所有的項目位於編輯模式，而且其值可以儲存 ' 全部更新 按鈕，即可..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 57743ca7-5695-4e07-aed1-44b297f245a9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs
msc.type: authoredcontent
ms.openlocfilehash: 46db3c5d733b9c8b6e749a9b8ff1aa9a061c36df
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="performing-batch-updates-c"></a><span data-ttu-id="02686-103">執行批次更新 (C#)</span><span class="sxs-lookup"><span data-stu-id="02686-103">Performing Batch Updates (C#)</span></span>
====================
<span data-ttu-id="02686-104">由[Scott Mitchell](https://twitter.com/ScottOnWriting)</span><span class="sxs-lookup"><span data-stu-id="02686-104">by [Scott Mitchell](https://twitter.com/ScottOnWriting)</span></span>

<span data-ttu-id="02686-105">[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_37_CS.exe)或[下載 PDF](performing-batch-updates-cs/_static/datatutorial37cs1.pdf)</span><span class="sxs-lookup"><span data-stu-id="02686-105">[Download Sample App](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_37_CS.exe) or [Download PDF](performing-batch-updates-cs/_static/datatutorial37cs1.pdf)</span></span>

> <span data-ttu-id="02686-106">了解如何建立完全可編輯 DataList 其中所有的項目位於編輯模式，而且其值可以儲存在頁面上的 [更新全部] 按鈕，即可。</span><span class="sxs-lookup"><span data-stu-id="02686-106">Learn how to create a fully-editable DataList where all of its items are in edit mode and whose values can be saved by clicking an "Update All" button on the page.</span></span>


## <a name="introduction"></a><span data-ttu-id="02686-107">簡介</span><span class="sxs-lookup"><span data-stu-id="02686-107">Introduction</span></span>

<span data-ttu-id="02686-108">在[前述教學課程](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md)我們檢查如何建立項目層級資料清單。</span><span class="sxs-lookup"><span data-stu-id="02686-108">In the [preceding tutorial](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md) we examined how to create an item-level DataList.</span></span> <span data-ttu-id="02686-109">像是標準可以讓您編輯 GridView 資料清單中的每個項目包含編輯 按鈕，按下時，會讓項目可供編輯。</span><span class="sxs-lookup"><span data-stu-id="02686-109">Like the standard editable GridView, each item in the DataList included an Edit button that, when clicked, would make the item editable.</span></span> <span data-ttu-id="02686-110">雖然這項目層級編輯適用於僅會不定期更新的資料，則特定使用案例會需要使用者編輯多筆記錄。</span><span class="sxs-lookup"><span data-stu-id="02686-110">While this item-level editing works well for data that is only updated occasionally, certain use case scenarios require the user to edit many records.</span></span> <span data-ttu-id="02686-111">如果使用者需要編輯數十個記錄，並且會強制按一下 編輯、 其變更，並按一下每個更新，按一下 的數量可能會妨礙其產能。</span><span class="sxs-lookup"><span data-stu-id="02686-111">If a user needs to edit dozens of records and is forced to click Edit, make their changes, and click Update for each one, the amount of clicking can hamper her productivity.</span></span> <span data-ttu-id="02686-112">在這種情況下，較好的選擇是要提供完全可編輯的資料清單中，一個 where*所有*的項目會處於編輯模式，可以編輯其值，依序按一下 [全部更新] 頁面上的按鈕 （請參閱圖 1）。</span><span class="sxs-lookup"><span data-stu-id="02686-112">In such situations, a better option is to provide a fully-editable DataList, one where *all* of its items are in edit mode and whose values can be edited by clicking an Update All button on the page (see Figure 1).</span></span>


<span data-ttu-id="02686-113">[![可以修改每個完整的可編輯 DataList 項目](performing-batch-updates-cs/_static/image2.png)](performing-batch-updates-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="02686-113">[![Each Item in a Fully Editable DataList can be Modified](performing-batch-updates-cs/_static/image2.png)](performing-batch-updates-cs/_static/image1.png)</span></span>

<span data-ttu-id="02686-114">**圖 1**： 完整的可編輯 DataList 中的每個項目可以進行修改 ([按一下以檢視完整大小的影像](performing-batch-updates-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="02686-114">**Figure 1**: Each Item in a Fully Editable DataList can be Modified ([Click to view full-size image](performing-batch-updates-cs/_static/image3.png))</span></span>


<span data-ttu-id="02686-115">在本教學課程中，我們將檢驗如何讓使用者以更新使用完全可進行編輯 DataList 供應商位址資訊。</span><span class="sxs-lookup"><span data-stu-id="02686-115">In this tutorial we'll examine how to enable users to update suppliers address information using a fully editable DataList.</span></span>

## <a name="step-1-create-the-editable-user-interface-in-the-datalist-s-itemtemplate"></a><span data-ttu-id="02686-116">步驟 1: S DataList ItemTemplate 中建立的可編輯的使用者介面</span><span class="sxs-lookup"><span data-stu-id="02686-116">Step 1: Create the Editable User Interface in the DataList s ItemTemplate</span></span>

<span data-ttu-id="02686-117">前述教學課程中，我們建立標準的項目層級的可編輯 DataList，我們使用的位置兩個範本：</span><span class="sxs-lookup"><span data-stu-id="02686-117">In the preceding tutorial, where we creating a standard, item-level editable DataList, we used two templates:</span></span>

- <span data-ttu-id="02686-118">`ItemTemplate`包含唯讀的使用者介面 （Label Web 控制項來顯示每個產品的名稱與價格）。</span><span class="sxs-lookup"><span data-stu-id="02686-118">`ItemTemplate` contained the read-only user interface (the Label Web controls for displaying each product s name and price).</span></span>
- <span data-ttu-id="02686-119">`EditItemTemplate`包含編輯模式使用者介面 （兩個文字方塊中的 Web 控制項）。</span><span class="sxs-lookup"><span data-stu-id="02686-119">`EditItemTemplate` contained the edit mode user interface (the two TextBox Web controls).</span></span>

<span data-ttu-id="02686-120">DataList s`EditItemIndex`屬性規定哪些`DataListItem`（如果有的話） 使用轉譯`EditItemTemplate`。</span><span class="sxs-lookup"><span data-stu-id="02686-120">The DataList s `EditItemIndex` property dictates what `DataListItem` (if any) is rendered using the `EditItemTemplate`.</span></span> <span data-ttu-id="02686-121">特別是，`DataListItem`其`ItemIndex`值符合 DataList s`EditItemIndex`屬性使用呈現`EditItemTemplate`。</span><span class="sxs-lookup"><span data-stu-id="02686-121">In particular, the `DataListItem` whose `ItemIndex` value matches the DataList s `EditItemIndex` property is rendered using the `EditItemTemplate`.</span></span> <span data-ttu-id="02686-122">只有一個項目都是可編輯的時間，但分開的落在建立完全可編輯的資料清單時，此模型會運作。</span><span class="sxs-lookup"><span data-stu-id="02686-122">This model works well when only one item can be edited at a time, but falls apart when creating a fully-editable DataList.</span></span>

<span data-ttu-id="02686-123">對於完全可進行編輯的資料清單中，我們希望*所有*的`DataListItem`s 來呈現使用可編輯的介面。</span><span class="sxs-lookup"><span data-stu-id="02686-123">For a fully editable DataList, we want *all* of the `DataListItem` s to render using the editable interface.</span></span> <span data-ttu-id="02686-124">若要完成這項作業最簡單的方式是定義中的可編輯介面`ItemTemplate`。</span><span class="sxs-lookup"><span data-stu-id="02686-124">The simplest way to accomplish this is to define the editable interface in the `ItemTemplate`.</span></span> <span data-ttu-id="02686-125">修改供應商位址資訊，可編輯的介面會包含地址、 縣市和國家 （地區） 的值做為文字，再文字方塊供應商名稱。</span><span class="sxs-lookup"><span data-stu-id="02686-125">For modifying the suppliers address information, the editable interface contains the supplier name as text and then TextBoxes for the address, city, and country values.</span></span>

<span data-ttu-id="02686-126">先開啟`BatchUpdate.aspx`頁面上，加入 DataList 控制項，然後設定其`ID`屬性`Suppliers`。</span><span class="sxs-lookup"><span data-stu-id="02686-126">Start by opening the `BatchUpdate.aspx` page, add a DataList control, and set its `ID` property to `Suppliers`.</span></span> <span data-ttu-id="02686-127">從 DataList s 智慧標籤，選擇 加入新的 ObjectDataSource 控制項，名為`SuppliersDataSource`。</span><span class="sxs-lookup"><span data-stu-id="02686-127">From the DataList s smart tag, opt to add a new ObjectDataSource control named `SuppliersDataSource`.</span></span>


<span data-ttu-id="02686-128">[![建立名為 SuppliersDataSource 新 ObjectDataSource](performing-batch-updates-cs/_static/image5.png)](performing-batch-updates-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="02686-128">[![Create a New ObjectDataSource Named SuppliersDataSource](performing-batch-updates-cs/_static/image5.png)](performing-batch-updates-cs/_static/image4.png)</span></span>

<span data-ttu-id="02686-129">**圖 2**： 建立新的 ObjectDataSource 具名`SuppliersDataSource`([按一下以檢視完整大小的影像](performing-batch-updates-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="02686-129">**Figure 2**: Create a New ObjectDataSource Named `SuppliersDataSource` ([Click to view full-size image](performing-batch-updates-cs/_static/image6.png))</span></span>


<span data-ttu-id="02686-130">設定要使用擷取資料 ObjectDataSource`SuppliersBLL`類別的`GetSuppliers()`方法 （請參閱圖 3）。</span><span class="sxs-lookup"><span data-stu-id="02686-130">Configure the ObjectDataSource to retrieve data using the `SuppliersBLL` class s `GetSuppliers()` method (see Figure 3).</span></span> <span data-ttu-id="02686-131">與先前的教學課程中，而非更新供應商的資訊透過 ObjectDataSource，我們將直接使用商務邏輯層。</span><span class="sxs-lookup"><span data-stu-id="02686-131">As with the preceding tutorial, rather than updating the supplier information through the ObjectDataSource, we'll work directly with the Business Logic Layer.</span></span> <span data-ttu-id="02686-132">因此，在 更新 索引標籤中設定下拉式清單，為 （無） （請參閱圖 4）。</span><span class="sxs-lookup"><span data-stu-id="02686-132">Therefore, set the drop-down list to (None) in the UPDATE tab (see Figure 4).</span></span>


<span data-ttu-id="02686-133">[![擷取使用 GetSuppliers() 方法的供應商資訊](performing-batch-updates-cs/_static/image8.png)](performing-batch-updates-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="02686-133">[![Retrieve Supplier Information Using the GetSuppliers() Method](performing-batch-updates-cs/_static/image8.png)](performing-batch-updates-cs/_static/image7.png)</span></span>

<span data-ttu-id="02686-134">**圖 3**： 擷取供應商資訊使用`GetSuppliers()`方法 ([按一下以檢視完整大小的影像](performing-batch-updates-cs/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="02686-134">**Figure 3**: Retrieve Supplier Information Using the `GetSuppliers()` Method ([Click to view full-size image](performing-batch-updates-cs/_static/image9.png))</span></span>


<span data-ttu-id="02686-135">[![在 [更新] 索引標籤中設定為（無） 下拉式清單](performing-batch-updates-cs/_static/image11.png)](performing-batch-updates-cs/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="02686-135">[![Set the Drop-Down List to (None) in the UPDATE Tab](performing-batch-updates-cs/_static/image11.png)](performing-batch-updates-cs/_static/image10.png)</span></span>

<span data-ttu-id="02686-136">**圖 4**： 在 [更新] 索引標籤中設定為（無） 下拉式清單 ([按一下以檢視完整大小的影像](performing-batch-updates-cs/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="02686-136">**Figure 4**: Set the Drop-Down List to (None) in the UPDATE Tab ([Click to view full-size image](performing-batch-updates-cs/_static/image12.png))</span></span>


<span data-ttu-id="02686-137">完成精靈之後，Visual Studio 會自動產生 DataList 的`ItemTemplate`顯示標籤 Web 控制項中的資料來源傳回的每個資料欄位。</span><span class="sxs-lookup"><span data-stu-id="02686-137">After completing the wizard, Visual Studio automatically generates the DataList s `ItemTemplate` to display each data field returned by the data source in a Label Web control.</span></span> <span data-ttu-id="02686-138">我們需要修改此範本，使其改為提供編輯介面。</span><span class="sxs-lookup"><span data-stu-id="02686-138">We need to modify this template so that it provides the editing interface instead.</span></span> <span data-ttu-id="02686-139">`ItemTemplate`可以自訂透過使用從 DataList s 智慧標籤的 [編輯樣板] 選項設計工具，或直接透過宣告式語法。</span><span class="sxs-lookup"><span data-stu-id="02686-139">The `ItemTemplate` can be customized through the Designer using the Edit Templates option from the DataList s smart tag or directly through the declarative syntax.</span></span>

<span data-ttu-id="02686-140">請花一點時間來建立供應商的名稱顯示為文字，但包含供應商 s 地址、 縣市和國家 （地區） 值的文字方塊的編輯介面。</span><span class="sxs-lookup"><span data-stu-id="02686-140">Take a moment to create an editing interface that displays the supplier s name as text, but includes TextBoxes for the supplier s address, city, and country values.</span></span> <span data-ttu-id="02686-141">進行這些變更之後，頁面 s 的宣告式語法看起來應該如下所示：</span><span class="sxs-lookup"><span data-stu-id="02686-141">After making these changes, your page s declarative syntax should look similar to the following:</span></span>


[!code-aspx[Main](performing-batch-updates-cs/samples/sample1.aspx)]

> [!NOTE]
> <span data-ttu-id="02686-142">為先前的教學課程中，在本教學課程 DataList 必須具有啟用其檢視狀態。</span><span class="sxs-lookup"><span data-stu-id="02686-142">As with the preceding tutorial, the DataList in this tutorial must have its view state enabled.</span></span>


<span data-ttu-id="02686-143">在`ItemTemplate`我使用兩個新的 CSS 類別、 m`SupplierPropertyLabel`和`SupplierPropertyValue`，其中已新增至`Styles.css`類別，並設定為使用相同的樣式設定`ProductPropertyLabel`和`ProductPropertyValue`CSS 類別。</span><span class="sxs-lookup"><span data-stu-id="02686-143">In the `ItemTemplate` I m using two new CSS classes, `SupplierPropertyLabel` and `SupplierPropertyValue`, which have been added to the `Styles.css` class and configured to use the same style settings as the `ProductPropertyLabel` and `ProductPropertyValue` CSS classes.</span></span>


[!code-css[Main](performing-batch-updates-cs/samples/sample2.css)]

<span data-ttu-id="02686-144">進行這些變更之後，請瀏覽此頁面，透過瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="02686-144">After making these changes, visit this page through a browser.</span></span> <span data-ttu-id="02686-145">如圖 5 所示，每個 DataList 項目顯示為文字的供應商名稱，並使用文字方塊顯示地址、 縣市和國家/地區。</span><span class="sxs-lookup"><span data-stu-id="02686-145">As Figure 5 shows, each DataList item displays the supplier name as text and uses TextBoxes to display the address, city, and country.</span></span>


<span data-ttu-id="02686-146">[![在 DataList 中的每個供應商是編輯](performing-batch-updates-cs/_static/image14.png)](performing-batch-updates-cs/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="02686-146">[![Each Supplier in the DataList is Editable](performing-batch-updates-cs/_static/image14.png)](performing-batch-updates-cs/_static/image13.png)</span></span>

<span data-ttu-id="02686-147">**圖 5**: DataList 中的每個供應商是編輯 ([按一下以檢視完整大小的影像](performing-batch-updates-cs/_static/image15.png))</span><span class="sxs-lookup"><span data-stu-id="02686-147">**Figure 5**: Each Supplier in the DataList is Editable ([Click to view full-size image](performing-batch-updates-cs/_static/image15.png))</span></span>


## <a name="step-2-adding-an-update-all-button"></a><span data-ttu-id="02686-148">步驟 2： 加入更新所有的按鈕</span><span class="sxs-lookup"><span data-stu-id="02686-148">Step 2: Adding an Update All Button</span></span>

<span data-ttu-id="02686-149">圖 5 中的每個供應商具有其地址、 縣市和國家/地區欄位顯示在文字方塊中，而目前沒有任何更新 按鈕。</span><span class="sxs-lookup"><span data-stu-id="02686-149">While each supplier in Figure 5 has its address, city, and country fields displayed in a TextBox, there currently is no Update button available.</span></span> <span data-ttu-id="02686-150">而不是讓每個項目 更新 按鈕，以完全可進行編輯 DataLists 則通常單一全部更新 按鈕在頁面上，按一下時，更新*所有*DataList 的記錄。</span><span class="sxs-lookup"><span data-stu-id="02686-150">Rather than having an Update button per item, with fully editable DataLists there is typically a single Update All button on the page that, when clicked, updates *all* of the records in the DataList.</span></span> <span data-ttu-id="02686-151">本教學課程，可讓 s （雖然按一下任一個 按鈕會有相同的效果） 中加入兩個全部更新 按鈕-一頁頂端和底部的其中一個。</span><span class="sxs-lookup"><span data-stu-id="02686-151">For this tutorial, let s add two Update All buttons - one at the top of the page and one at the bottom (although clicking either button will have the same effect).</span></span>

<span data-ttu-id="02686-152">藉由新增按鈕 Web 控制項上方 DataList 組開始其`ID`屬性`UpdateAll1`。</span><span class="sxs-lookup"><span data-stu-id="02686-152">Start by adding a Button Web control above the DataList and set its `ID` property to `UpdateAll1`.</span></span> <span data-ttu-id="02686-153">接下來，加入第二個按鈕 Web 控制項下方 DataList，設定其`ID`至`UpdateAll2`。</span><span class="sxs-lookup"><span data-stu-id="02686-153">Next, add the second Button Web control beneath the DataList, setting its `ID` to `UpdateAll2`.</span></span> <span data-ttu-id="02686-154">設定`Text`要更新所有的兩個按鈕的屬性。</span><span class="sxs-lookup"><span data-stu-id="02686-154">Set the `Text` properties for the two Buttons to Update All.</span></span> <span data-ttu-id="02686-155">最後，建立兩個按鈕的事件處理常式`Click`事件。</span><span class="sxs-lookup"><span data-stu-id="02686-155">Lastly, create event handlers for both Buttons `Click` events.</span></span> <span data-ttu-id="02686-156">而不是複製每個事件處理常式中的更新邏輯，可讓 s 重構給第三個方法時，該邏輯`UpdateAllSupplierAddresses`，有直接叫用這個第三個方法的事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="02686-156">Rather than duplicating the update logic in each of the event handlers, let s refactor that logic to a third method, `UpdateAllSupplierAddresses`, having the event handlers simply invoking this third method.</span></span>


[!code-csharp[Main](performing-batch-updates-cs/samples/sample3.cs)]

<span data-ttu-id="02686-157">圖 6 顯示頁面之後已加入全部更新 按鈕。</span><span class="sxs-lookup"><span data-stu-id="02686-157">Figure 6 shows the page after the Update All buttons have been added.</span></span>


<span data-ttu-id="02686-158">[![兩個更新的所有按鈕已都加入至頁面](performing-batch-updates-cs/_static/image17.png)](performing-batch-updates-cs/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="02686-158">[![Two Update All Buttons have been Added to the Page](performing-batch-updates-cs/_static/image17.png)](performing-batch-updates-cs/_static/image16.png)</span></span>

<span data-ttu-id="02686-159">**圖 6**： 兩個更新的所有按鈕已都加入至頁面 ([按一下以檢視完整大小的影像](performing-batch-updates-cs/_static/image18.png))</span><span class="sxs-lookup"><span data-stu-id="02686-159">**Figure 6**: Two Update All Buttons have been Added to the Page ([Click to view full-size image](performing-batch-updates-cs/_static/image18.png))</span></span>


## <a name="step-3-updating-all-of-the-suppliers-address-information"></a><span data-ttu-id="02686-160">步驟 3： 更新的所有供應商的位址資訊</span><span class="sxs-lookup"><span data-stu-id="02686-160">Step 3: Updating All of the Suppliers Address Information</span></span>

<span data-ttu-id="02686-161">所有顯示的編輯介面 DataList s 項目加上的 [全部更新] 按鈕，而所有維持正在寫入的程式碼執行批次更新。</span><span class="sxs-lookup"><span data-stu-id="02686-161">With all of the DataList s items displaying the editing interface and with the addition of the Update All buttons, all that remains is writing the code to perform the batch update.</span></span> <span data-ttu-id="02686-162">具體而言，我們需要迴圈 DataList s 項目並呼叫`SuppliersBLL`類別的`UpdateSupplierAddress`針對每個方法。</span><span class="sxs-lookup"><span data-stu-id="02686-162">Specifically, we need to loop through the DataList s items and call the `SuppliersBLL` class s `UpdateSupplierAddress` method for each one.</span></span>

<span data-ttu-id="02686-163">集合`DataListItem`DataList 可以透過 DataList s 該結構的執行個體[`Items`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.items.aspx)。</span><span class="sxs-lookup"><span data-stu-id="02686-163">The collection of `DataListItem` instances that makeup the DataList can be accessed via the DataList s [`Items` property](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.items.aspx).</span></span> <span data-ttu-id="02686-164">參考`DataListItem`，我們可以抓取對應`SupplierID`從`DataKeys`集合並以程式設計方式參考的文字方塊中 Web 控制項內`ItemTemplate`如下列程式碼所示：</span><span class="sxs-lookup"><span data-stu-id="02686-164">With a reference to a `DataListItem`, we can grab the corresponding `SupplierID` from the `DataKeys` collection and programmatically reference the TextBox Web controls within the `ItemTemplate` as the following code illustrates:</span></span>


[!code-csharp[Main](performing-batch-updates-cs/samples/sample4.cs)]

<span data-ttu-id="02686-165">當使用者按一下 [全部更新] 按鈕，其中`UpdateAllSupplierAddresses`方法逐一查看每個`DataListItem`中`Suppliers`DataList 和呼叫`SuppliersBLL`類別的`UpdateSupplierAddress`方法，傳入的對應值。</span><span class="sxs-lookup"><span data-stu-id="02686-165">When the user clicks one of the Update All buttons, the `UpdateAllSupplierAddresses` method iterates through each `DataListItem` in the `Suppliers` DataList and calls the `SuppliersBLL` class s `UpdateSupplierAddress` method, passing in the corresponding values.</span></span> <span data-ttu-id="02686-166">地址、 縣市或國家/地區會傳遞非輸入值是值為`Nothing`至`UpdateSupplierAddress`（而不是空白的字串），這會導致資料庫`NULL`基礎 s 記錄欄位。</span><span class="sxs-lookup"><span data-stu-id="02686-166">A non-entered value for address, city, or country passes is a value of `Nothing` to `UpdateSupplierAddress` (rather than a blank string), which results in a database `NULL` for the underlying record s fields.</span></span>

> [!NOTE]
> <span data-ttu-id="02686-167">為了加強，您可以加入標籤 Web 控制項的狀態可提供一些確認訊息，執行批次更新後的頁面。</span><span class="sxs-lookup"><span data-stu-id="02686-167">As an enhancement, you may want to add a status Label Web control to the page that provides some confirmation message after the batch update is performed.</span></span>


## <a name="updating-only-those-addresses-that-have-been-modified"></a><span data-ttu-id="02686-168">更新已修改這些地址</span><span class="sxs-lookup"><span data-stu-id="02686-168">Updating Only Those Addresses That Have Been Modified</span></span>

<span data-ttu-id="02686-169">用於此教學課程的呼叫的批次更新演算法`UpdateSupplierAddress`方法*每*供應商在 DataList，不論是否已變更其位址資訊。</span><span class="sxs-lookup"><span data-stu-id="02686-169">The batch update algorithm used for this tutorial calls the `UpdateSupplierAddress` method for *every* supplier in the DataList, regardless of whether their address information has been changed.</span></span> <span data-ttu-id="02686-170">這類 blind 更新通常並不是效能問題，而它們可能會導致過多記錄如果作為稽核您變更至資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="02686-170">While such blind updates aren t usually a performance issue, they can lead to superfluous records if you re auditing changes to the database table.</span></span> <span data-ttu-id="02686-171">例如，如果您使用觸發程序來記錄所有`UPDATE`s`Suppliers`資料表至稽核的資料表，每次使用者按一下 [全部更新] 按鈕，每個供應商在系統中，不論使用者是否進行任何建立新的稽核記錄變更。</span><span class="sxs-lookup"><span data-stu-id="02686-171">For example, if you use triggers to record all `UPDATE` s to the `Suppliers` table to an auditing table, every time a user clicks the Update All button a new audit record will be created for each supplier in the system, regardless of whether the user made any changes.</span></span>

<span data-ttu-id="02686-172">ADO.NET DataTable 和資料配接器類別被設計來支援批次更新只修改、 刪除及新的記錄會導致任何資料庫進行通訊。</span><span class="sxs-lookup"><span data-stu-id="02686-172">The ADO.NET DataTable and DataAdapter classes are designed to support batch updates where only modified, deleted, and new records results in any database communication.</span></span> <span data-ttu-id="02686-173">在 DataTable 中的每個資料列都有[`RowState`屬性](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx)，指出是否已加入至 DataTable，從它修改、 刪除或維持不變的資料列。</span><span class="sxs-lookup"><span data-stu-id="02686-173">Each row in the DataTable has a [`RowState` property](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx) that indicates whether the row has been added to the DataTable, deleted from it, modified, or remains unchanged.</span></span> <span data-ttu-id="02686-174">當一開始填入 DataTable 時，所有資料列會標示為不變。</span><span class="sxs-lookup"><span data-stu-id="02686-174">When a DataTable is initially populated, all rows are marked unchanged.</span></span> <span data-ttu-id="02686-175">變更任何資料列 s 資料行的值標示資料列，為已修改。</span><span class="sxs-lookup"><span data-stu-id="02686-175">Changing the value of any of the row s columns marks the row as modified.</span></span>

<span data-ttu-id="02686-176">在`SuppliersBLL`我們更新成單一的供應商記錄中的第一個讀取指定的供應商 s 地址資訊的類別`SuppliersDataTable`，然後設定`Address`， `City`，和`Country`使用下列程式碼的資料行值：</span><span class="sxs-lookup"><span data-stu-id="02686-176">In the `SuppliersBLL` class we update the specified supplier s address information by first reading in the single supplier record into a `SuppliersDataTable` and then set the `Address`, `City`, and `Country` column values using the following code:</span></span>


[!code-csharp[Main](performing-batch-updates-cs/samples/sample5.cs)]

<span data-ttu-id="02686-177">傳入的地址、 縣市和國家 （地區） 值，這個會指派此程式碼`SuppliersRow`中`SuppliersDataTable`不論是否已變更的值。</span><span class="sxs-lookup"><span data-stu-id="02686-177">This code naively assigns the passed-in address, city, and country values to the `SuppliersRow` in the `SuppliersDataTable` regardless of whether or not the values have changed.</span></span> <span data-ttu-id="02686-178">這些修改會導致`SuppliersRow`s`RowState`將會標示為已修改的屬性。</span><span class="sxs-lookup"><span data-stu-id="02686-178">These modifications cause the `SuppliersRow` s `RowState` property to be marked as modified.</span></span> <span data-ttu-id="02686-179">當資料存取層 s`Update`方法呼叫，它會看到`SupplierRow`已修改，因此傳送`UPDATE`命令到資料庫。</span><span class="sxs-lookup"><span data-stu-id="02686-179">When the Data Access Layer s `Update` method is called, it sees that the `SupplierRow` has been modified and therefore sends an `UPDATE` command to the database.</span></span>

<span data-ttu-id="02686-180">想像一下，我們將程式碼加入至這個方法，只能指定傳入的地址、 縣市和國家 （地區） 值，如果有何差異的`SuppliersRow`s 現有的值。</span><span class="sxs-lookup"><span data-stu-id="02686-180">Imagine, however, that we added code to this method to only assign the passed-in address, city, and country values if they differ from the `SuppliersRow` s existing values.</span></span> <span data-ttu-id="02686-181">地址、 縣市和國家/地區的現有資料相同的情況下，將會進行任何變更， `SupplierRow` s`RowState`會被標示為不變。</span><span class="sxs-lookup"><span data-stu-id="02686-181">In the case where the address, city, and country are the same as the existing data, no changes will be made and the `SupplierRow` s `RowState` will be left marked as unchanged.</span></span> <span data-ttu-id="02686-182">最後結果就是，當 DAL s`Update`方法呼叫，將會進行任何資料庫的呼叫，因為`SuppliersRow`尚未修改。</span><span class="sxs-lookup"><span data-stu-id="02686-182">The net result is that when the DAL s `Update` method is called, no database call will be made because the `SuppliersRow` has not been modified.</span></span>

<span data-ttu-id="02686-183">若要制定這項變更，取代盲目地指派傳入的地址、 縣市和國家 （地區） 值，以下列程式碼陳述式：</span><span class="sxs-lookup"><span data-stu-id="02686-183">To enact this change, replace the statements that blindly assign the passed-in address, city, and country values with the following code:</span></span>


[!code-csharp[Main](performing-batch-updates-cs/samples/sample6.cs)]

<span data-ttu-id="02686-184">與此加入程式碼中，DAL s`Update`方法傳送`UPDATE`陳述式的資料庫，其地址相關的值已變更的記錄。</span><span class="sxs-lookup"><span data-stu-id="02686-184">With this added code, the DAL s `Update` method sends an `UPDATE` statement to the database for only those records whose address-related values have changed.</span></span>

<span data-ttu-id="02686-185">或者，我們無法追蹤是否有任何傳入的地址欄位和資料庫資料之間的差異，並，如果沒有任何項目，只是略過呼叫 DAL 的`Update`方法。</span><span class="sxs-lookup"><span data-stu-id="02686-185">Alternatively, we could keep track of whether there are any differences between the passed-in address fields and the database data and, if there are none, simply bypass the call to the DAL s `Update` method.</span></span> <span data-ttu-id="02686-186">這個方法適用於您會使用 DB 直接方法，因為傳遞的 DB 直接方法目前 t 如果`SuppliersRow`執行個體，其`RowState`可以檢查以判斷是否確實需要資料庫的呼叫。</span><span class="sxs-lookup"><span data-stu-id="02686-186">This approach works well if you re using the DB direct method, since the DB direct method isn t passed a `SuppliersRow` instance whose `RowState` can be checked to determine whether a database call is actually needed.</span></span>

> [!NOTE]
> <span data-ttu-id="02686-187">每次`UpdateSupplierAddress`叫用方法時，資料庫進行呼叫以擷取有關更新的記錄資訊。</span><span class="sxs-lookup"><span data-stu-id="02686-187">Each time the `UpdateSupplierAddress` method is invoked, a call is made to the database to retrieve information about the updated record.</span></span> <span data-ttu-id="02686-188">然後，如果資料中有任何變更，另一個資料庫呼叫來更新資料表資料列。</span><span class="sxs-lookup"><span data-stu-id="02686-188">Then, if there are any changes in data, another call to the database is made to update the table row.</span></span> <span data-ttu-id="02686-189">此工作流程無法最佳化藉由建立`UpdateSupplierAddress`方法多載接受`EmployeesDataTable`具有執行個體*所有*從變更的`BatchUpdate.aspx`頁面。</span><span class="sxs-lookup"><span data-stu-id="02686-189">This workflow could be optimized by creating an `UpdateSupplierAddress` method overload that accepts an `EmployeesDataTable` instance that has *all* of the changes from the `BatchUpdate.aspx` page.</span></span> <span data-ttu-id="02686-190">然後，它可以讓一個呼叫資料庫取得所有記錄的`Suppliers`資料表。</span><span class="sxs-lookup"><span data-stu-id="02686-190">Then, it could make one call to the database to get all of the records from the `Suppliers` table.</span></span> <span data-ttu-id="02686-191">然後可以列舉的兩個結果集，因此無法更新已發生變更的記錄。</span><span class="sxs-lookup"><span data-stu-id="02686-191">The two resultsets could then be enumerated and only those records where changes have occurred could be updated.</span></span>


## <a name="summary"></a><span data-ttu-id="02686-192">總結</span><span class="sxs-lookup"><span data-stu-id="02686-192">Summary</span></span>

<span data-ttu-id="02686-193">在此教學課程中我們可了解如何建立完整的可編輯 DataList，讓使用者能夠快速修改多個供應商的地址資訊。</span><span class="sxs-lookup"><span data-stu-id="02686-193">In this tutorial we saw how to create a fully editable DataList, allowing a user to quickly modify the address information for multiple suppliers.</span></span> <span data-ttu-id="02686-194">我們已開始編輯介面供應商 s 地址、 縣市和國家 （地區） 值的文字方塊中的 Web 控制項定義在 DataList 的`ItemTemplate`。</span><span class="sxs-lookup"><span data-stu-id="02686-194">We started by defining the editing interface a TextBox Web control for the supplier s address, city, and country values in the DataList s `ItemTemplate`.</span></span> <span data-ttu-id="02686-195">接下來，我們加入上方和下方 DataList 全部更新 按鈕。</span><span class="sxs-lookup"><span data-stu-id="02686-195">Next, we added Update All buttons above and below the DataList.</span></span> <span data-ttu-id="02686-196">使用者在其變更，並按下的 [全部更新] 按鈕，其中一個之後`DataListItem`s 會列舉而呼叫`SuppliersBLL`類別的`UpdateSupplierAddress`方法進行。</span><span class="sxs-lookup"><span data-stu-id="02686-196">After a user has made his changes and clicked one of the Update All buttons, the `DataListItem` s are enumerated and a call to the `SuppliersBLL` class s `UpdateSupplierAddress` method is made.</span></span>

<span data-ttu-id="02686-197">祝您程式設計 ！</span><span class="sxs-lookup"><span data-stu-id="02686-197">Happy Programming!</span></span>

## <a name="about-the-author"></a><span data-ttu-id="02686-198">關於作者</span><span class="sxs-lookup"><span data-stu-id="02686-198">About the Author</span></span>

<span data-ttu-id="02686-199">[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。</span><span class="sxs-lookup"><span data-stu-id="02686-199">[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), author of seven ASP/ASP.NET books and founder of [4GuysFromRolla.com](http://www.4guysfromrolla.com), has been working with Microsoft Web technologies since 1998.</span></span> <span data-ttu-id="02686-200">Scott 可做為獨立顧問、 訓練和寫入器。</span><span class="sxs-lookup"><span data-stu-id="02686-200">Scott works as an independent consultant, trainer, and writer.</span></span> <span data-ttu-id="02686-201">他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。</span><span class="sxs-lookup"><span data-stu-id="02686-201">His latest book is [*Sams Teach Yourself ASP.NET 2.0 in 24 Hours*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco).</span></span> <span data-ttu-id="02686-202">他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。</span><span class="sxs-lookup"><span data-stu-id="02686-202">He can be reached at [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) or via his blog, which can be found at [http://ScottOnWriting.NET](http://ScottOnWriting.NET).</span></span>

## <a name="special-thanks-to"></a><span data-ttu-id="02686-203">特別感謝</span><span class="sxs-lookup"><span data-stu-id="02686-203">Special Thanks To</span></span>

<span data-ttu-id="02686-204">許多有用的檢閱者已檢閱本教學課程系列。</span><span class="sxs-lookup"><span data-stu-id="02686-204">This tutorial series was reviewed by many helpful reviewers.</span></span> <span data-ttu-id="02686-205">此教學課程中的前導檢閱者已 Zack Jones，Ken Pespisa。</span><span class="sxs-lookup"><span data-stu-id="02686-205">Lead reviewers for this tutorial were Zack Jones and Ken Pespisa.</span></span> <span data-ttu-id="02686-206">檢閱我即將推出的 MSDN 文件有興趣嗎？</span><span class="sxs-lookup"><span data-stu-id="02686-206">Interested in reviewing my upcoming MSDN articles?</span></span> <span data-ttu-id="02686-207">如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)</span><span class="sxs-lookup"><span data-stu-id="02686-207">If so, drop me a line at [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="02686-208">[上一頁](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md)
[下一頁](handling-bll-and-dal-level-exceptions-cs.md)</span><span class="sxs-lookup"><span data-stu-id="02686-208">[Previous](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md)
[Next](handling-bll-and-dal-level-exceptions-cs.md)</span></span>
