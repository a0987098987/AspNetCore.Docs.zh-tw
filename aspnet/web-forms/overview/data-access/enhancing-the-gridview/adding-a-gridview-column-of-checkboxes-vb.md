---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb
title: "加入 GridView 的資料行的核取方塊 (VB) |Microsoft 文件"
author: rick-anderson
description: "本教學課程會查看如何核取方塊的資料行加入提供給使用者以直覺的方式，選取代表 g 的多個資料列的 GridView 控制項..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2007
ms.topic: article
ms.assetid: 39253d05-75c0-41c7-b9d4-a6c58ecf69ce
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: 326201f9fe9ba5f482308dc8bfd7d2decb9fbd8f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-gridview-column-of-checkboxes-vb"></a><span data-ttu-id="eacf6-103">加入 GridView 的資料行的核取方塊 (VB)</span><span class="sxs-lookup"><span data-stu-id="eacf6-103">Adding a GridView Column of Checkboxes (VB)</span></span>
====================
<span data-ttu-id="eacf6-104">由[Scott Mitchell](https://twitter.com/ScottOnWriting)</span><span class="sxs-lookup"><span data-stu-id="eacf6-104">by [Scott Mitchell](https://twitter.com/ScottOnWriting)</span></span>

<span data-ttu-id="eacf6-105">[下載範例應用程式](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_52_VB.exe)或[下載 PDF](adding-a-gridview-column-of-checkboxes-vb/_static/datatutorial52vb1.pdf)</span><span class="sxs-lookup"><span data-stu-id="eacf6-105">[Download Sample App](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_52_VB.exe) or [Download PDF](adding-a-gridview-column-of-checkboxes-vb/_static/datatutorial52vb1.pdf)</span></span>

> <span data-ttu-id="eacf6-106">本教學課程會查看如何提供給使用者以直覺的方式選取多個資料列的 GridView 的 GridView 控制項中加入資料行的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="eacf6-106">This tutorial looks at how to add a column of check boxes to a GridView control to provide the user with an intuitive way of selecting multiple rows of the GridView.</span></span>


## <a name="introduction"></a><span data-ttu-id="eacf6-107">簡介</span><span class="sxs-lookup"><span data-stu-id="eacf6-107">Introduction</span></span>

<span data-ttu-id="eacf6-108">在前述教學課程中我們會檢查如何加入資料行的選項按鈕，以選取特定資料錄 GridView。</span><span class="sxs-lookup"><span data-stu-id="eacf6-108">In the preceding tutorial we examined how to add a column of radio buttons to the GridView for the purpose of selecting a particular record.</span></span> <span data-ttu-id="eacf6-109">當使用者選擇最多一個項目，從方格中有限時，選項按鈕的資料行是合適的使用者介面。</span><span class="sxs-lookup"><span data-stu-id="eacf6-109">A column of radio buttons is a suitable user interface when the user is limited to choosing at most one item from the grid.</span></span> <span data-ttu-id="eacf6-110">有時候，不過，我們可能想要讓使用者能夠選取任意數目的方格中的項目。</span><span class="sxs-lookup"><span data-stu-id="eacf6-110">At times, however, we may want to allow the user to pick an arbitrary number of items from the grid.</span></span> <span data-ttu-id="eacf6-111">例如，網頁型電子郵件用戶端，通常顯示具有資料行的核取方塊的訊息清單。</span><span class="sxs-lookup"><span data-stu-id="eacf6-111">Web-based email clients, for example, typically display the list of messages with a column of checkboxes.</span></span> <span data-ttu-id="eacf6-112">使用者可以選取任意數目的訊息，並接著執行某些動作，例如，電子郵件移到其他資料夾，或刪除它們。</span><span class="sxs-lookup"><span data-stu-id="eacf6-112">The user can select an arbitrary number of messages and then perform some action, such as moving the emails to another folder or deleting them.</span></span>

<span data-ttu-id="eacf6-113">在本教學課程中，我們會看到如何將資料行的核取方塊以及如何判斷在回傳簽入哪些核取方塊。</span><span class="sxs-lookup"><span data-stu-id="eacf6-113">In this tutorial we will see how to add a column of checkboxes and how to determine what checkboxes were checked on postback.</span></span> <span data-ttu-id="eacf6-114">特別是，我們會建置慎網頁型電子郵件用戶端使用者介面的範例。</span><span class="sxs-lookup"><span data-stu-id="eacf6-114">In particular, we'll build an example that closely mimics the web-based email client user interface.</span></span> <span data-ttu-id="eacf6-115">我們的範例中會包含以列出產品的分頁的 GridView`Products`資料庫資料表的核取方塊，在每個資料列 （請參閱圖 1）。</span><span class="sxs-lookup"><span data-stu-id="eacf6-115">Our example will include a paged GridView listing the products in the `Products` database table with a checkbox in each row (see Figure 1).</span></span> <span data-ttu-id="eacf6-116">刪除選取的產品按鈕，按一下時，將會刪除選取的產品。</span><span class="sxs-lookup"><span data-stu-id="eacf6-116">A Delete Selected Products button, when clicked, will delete those products selected.</span></span>


<span data-ttu-id="eacf6-117">[![每個產品的資料列包含核取方塊](adding-a-gridview-column-of-checkboxes-vb/_static/image1.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="eacf6-117">[![Each Product Row Includes a Checkbox](adding-a-gridview-column-of-checkboxes-vb/_static/image1.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image1.png)</span></span>

<span data-ttu-id="eacf6-118">**圖 1**： 每個產品的資料列包含核取方塊 ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-checkboxes-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="eacf6-118">**Figure 1**: Each Product Row Includes a Checkbox ([Click to view full-size image](adding-a-gridview-column-of-checkboxes-vb/_static/image2.png))</span></span>


## <a name="step-1-adding-a-paged-gridview-that-lists-product-information"></a><span data-ttu-id="eacf6-119">步驟 1： 加入分頁的 GridView 會列出產品資訊</span><span class="sxs-lookup"><span data-stu-id="eacf6-119">Step 1: Adding a Paged GridView that Lists Product Information</span></span>

<span data-ttu-id="eacf6-120">我們會擔心加入資料行的核取方塊之前，可讓 s 以列出產品的 GridView 會支援分頁上的第一個焦點。</span><span class="sxs-lookup"><span data-stu-id="eacf6-120">Before we worry about adding a column of checkboxes, let s first focus on listing the products in a GridView that supports paging.</span></span> <span data-ttu-id="eacf6-121">先開啟`CheckBoxField.aspx`頁面`EnhancedGridView`資料夾，然後拖曳 GridView 從工具箱拖曳至設計工具中，設定其`ID`至`Products`。</span><span class="sxs-lookup"><span data-stu-id="eacf6-121">Start by opening the `CheckBoxField.aspx` page in the `EnhancedGridView` folder and drag a GridView from the Toolbox onto the Designer, setting its `ID` to `Products`.</span></span> <span data-ttu-id="eacf6-122">接下來，選擇將 GridView 繫結至名為新 ObjectDataSource `ProductsDataSource`。</span><span class="sxs-lookup"><span data-stu-id="eacf6-122">Next, choose to bind the GridView to a new ObjectDataSource named `ProductsDataSource`.</span></span> <span data-ttu-id="eacf6-123">設定用於 ObjectDataSource`ProductsBLL`類別，呼叫`GetProducts()`方法傳回的資料。</span><span class="sxs-lookup"><span data-stu-id="eacf6-123">Configure the ObjectDataSource to use the `ProductsBLL` class, calling the `GetProducts()` method to return the data.</span></span> <span data-ttu-id="eacf6-124">此 GridView 將處於唯讀模式，因為設定下拉式清單中更新、 插入和刪除索引標籤為 （無）。</span><span class="sxs-lookup"><span data-stu-id="eacf6-124">Since this GridView will be read-only, set the drop-down lists in the UPDATE, INSERT, and DELETE tabs to (None) .</span></span>


<span data-ttu-id="eacf6-125">[![建立名為 ProductsDataSource 新 ObjectDataSource](adding-a-gridview-column-of-checkboxes-vb/_static/image2.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="eacf6-125">[![Create a New ObjectDataSource Named ProductsDataSource](adding-a-gridview-column-of-checkboxes-vb/_static/image2.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image3.png)</span></span>

<span data-ttu-id="eacf6-126">**圖 2**： 建立新的 ObjectDataSource 具名`ProductsDataSource`([按一下以檢視完整大小的影像](adding-a-gridview-column-of-checkboxes-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="eacf6-126">**Figure 2**: Create a New ObjectDataSource Named `ProductsDataSource` ([Click to view full-size image](adding-a-gridview-column-of-checkboxes-vb/_static/image4.png))</span></span>


<span data-ttu-id="eacf6-127">[![設定要擷取資料使用 GetProducts() 方法 ObjectDataSource](adding-a-gridview-column-of-checkboxes-vb/_static/image3.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="eacf6-127">[![Configure the ObjectDataSource to Retrieve Data Using the GetProducts() Method](adding-a-gridview-column-of-checkboxes-vb/_static/image3.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image5.png)</span></span>

<span data-ttu-id="eacf6-128">**圖 3**： 設定要擷取的資料使用 ObjectDataSource`GetProducts()`方法 ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-checkboxes-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="eacf6-128">**Figure 3**: Configure the ObjectDataSource to Retrieve Data Using the `GetProducts()` Method ([Click to view full-size image](adding-a-gridview-column-of-checkboxes-vb/_static/image6.png))</span></span>


<span data-ttu-id="eacf6-129">[![設定下拉式清單中更新、 插入和刪除 （無） 索引標籤](adding-a-gridview-column-of-checkboxes-vb/_static/image4.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="eacf6-129">[![Set the Drop-Down Lists in the UPDATE, INSERT, and DELETE Tabs to (None)](adding-a-gridview-column-of-checkboxes-vb/_static/image4.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image7.png)</span></span>

<span data-ttu-id="eacf6-130">**圖 4**： 下拉式清單中設定更新、 插入和刪除的索引標籤 （無） ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-checkboxes-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="eacf6-130">**Figure 4**: Set the Drop-Down Lists in the UPDATE, INSERT, and DELETE Tabs to (None) ([Click to view full-size image](adding-a-gridview-column-of-checkboxes-vb/_static/image8.png))</span></span>


<span data-ttu-id="eacf6-131">完成設定資料來源精靈之後，Visual Studio 會自動建立 BoundColumns 和 CheckBoxColumn 產品相關的資料欄位。</span><span class="sxs-lookup"><span data-stu-id="eacf6-131">After completing the Configure Data Source wizard, Visual Studio will automatically create BoundColumns and a CheckBoxColumn for the product-related data fields.</span></span> <span data-ttu-id="eacf6-132">我們未在上一個教學課程中的類似，請移除以外的所有`ProductName`， `CategoryName`，和`UnitPrice`BoundFields，並變更`HeaderText`產品、 類別和價格的屬性。</span><span class="sxs-lookup"><span data-stu-id="eacf6-132">Like we did in the previous tutorial, remove all but the `ProductName`, `CategoryName`, and `UnitPrice` BoundFields, and change the `HeaderText` properties to Product, Category, and Price.</span></span> <span data-ttu-id="eacf6-133">設定`UnitPrice`BoundField，使其值的格式為貨幣。</span><span class="sxs-lookup"><span data-stu-id="eacf6-133">Configure the `UnitPrice` BoundField so that its value is formatted as a currency.</span></span> <span data-ttu-id="eacf6-134">也請設定以支援分頁，方式是檢查智慧標籤啟用分頁 核取方塊 GridView。</span><span class="sxs-lookup"><span data-stu-id="eacf6-134">Also configure the GridView to support paging by checking the Enable Paging checkbox from the smart tag.</span></span>

<span data-ttu-id="eacf6-135">可讓 s 也將刪除所選的產品的使用者介面。</span><span class="sxs-lookup"><span data-stu-id="eacf6-135">Let s also add the user interface for deleting the selected products.</span></span> <span data-ttu-id="eacf6-136">加入按鈕 Web 控制項下方 GridView 中設定其`ID`至`DeleteSelectedProducts`及其`Text`屬性，以刪除選取的產品。</span><span class="sxs-lookup"><span data-stu-id="eacf6-136">Add a Button Web control beneath the GridView, setting its `ID` to `DeleteSelectedProducts` and its `Text` property to Delete Selected Products.</span></span> <span data-ttu-id="eacf6-137">而不實際刪除資料庫中的產品，此範例中我們將只會顯示訊息，指出會被刪除的產品。</span><span class="sxs-lookup"><span data-stu-id="eacf6-137">Rather than actually deleting products from the database, for this example we'll just display a message stating the products that would have been deleted.</span></span> <span data-ttu-id="eacf6-138">若要做到這一點，加入至按鈕下方的標籤 Web 控制項。</span><span class="sxs-lookup"><span data-stu-id="eacf6-138">To accommodate this, add a Label Web control beneath the Button.</span></span> <span data-ttu-id="eacf6-139">將其識別碼設`DeleteResults`，清除出其`Text`屬性，並設定其`Visible`和`EnableViewState`屬性`False`。</span><span class="sxs-lookup"><span data-stu-id="eacf6-139">Set its ID to `DeleteResults`, clear out its `Text` property, and set its `Visible` and `EnableViewState` properties to `False`.</span></span>

<span data-ttu-id="eacf6-140">進行這些變更之後，GridView、 ObjectDataSource、 按鈕和標籤 s 宣告式標記應該如下所示：</span><span class="sxs-lookup"><span data-stu-id="eacf6-140">After making these changes, the GridView, ObjectDataSource, Button, and Label s declarative markup should similar to the following:</span></span>


[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample1.aspx)]

<span data-ttu-id="eacf6-141">請花一點時間瀏覽器中檢視頁面 （請參閱圖 5）。</span><span class="sxs-lookup"><span data-stu-id="eacf6-141">Take a moment to view the page in a browser (see Figure 5).</span></span> <span data-ttu-id="eacf6-142">此時您應該會看到名稱、 類別和的前十個產品的價格。</span><span class="sxs-lookup"><span data-stu-id="eacf6-142">At this point you should see the name, category, and price of the first ten products.</span></span>


<span data-ttu-id="eacf6-143">[![會列出名稱、 類別和第十個產品的價格](adding-a-gridview-column-of-checkboxes-vb/_static/image5.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="eacf6-143">[![The Name, Category, and Price of the First Ten Products are Listed](adding-a-gridview-column-of-checkboxes-vb/_static/image5.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image9.png)</span></span>

<span data-ttu-id="eacf6-144">**圖 5**： 列出名稱、 類別和第十個產品的價格 ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-checkboxes-vb/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="eacf6-144">**Figure 5**: The Name, Category, and Price of the First Ten Products are Listed ([Click to view full-size image](adding-a-gridview-column-of-checkboxes-vb/_static/image10.png))</span></span>


## <a name="step-2-adding-a-column-of-checkboxes"></a><span data-ttu-id="eacf6-145">步驟 2： 加入資料行的核取方塊</span><span class="sxs-lookup"><span data-stu-id="eacf6-145">Step 2: Adding a Column of Checkboxes</span></span>

<span data-ttu-id="eacf6-146">因為 ASP.NET 2.0 包含 CheckBoxField，其中可能會認為它無法使用加入的 GridView 的資料行的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="eacf6-146">Since ASP.NET 2.0 includes a CheckBoxField, one might think that it could be used to add a column of checkboxes to a GridView.</span></span> <span data-ttu-id="eacf6-147">不幸的是，不情況下，因為將 CheckBoxField 設計來搭配布林值的資料欄位。</span><span class="sxs-lookup"><span data-stu-id="eacf6-147">Unfortunately, that is not the case, as the CheckBoxField is designed to work with a Boolean data field.</span></span> <span data-ttu-id="eacf6-148">也就是說，才能使用將 CheckBoxField 我們必須指定其值查閱來決定是否要檢查呈現的核取方塊的基礎資料欄位。</span><span class="sxs-lookup"><span data-stu-id="eacf6-148">That is, in order to use the CheckBoxField we must specify the underlying data field whose value is consulted to determine whether the rendered checkbox is checked.</span></span> <span data-ttu-id="eacf6-149">我們無法使用 CheckBoxField 只包含未選取核取方塊的資料行。</span><span class="sxs-lookup"><span data-stu-id="eacf6-149">We cannot use the CheckBoxField to just include a column of unchecked checkboxes.</span></span>

<span data-ttu-id="eacf6-150">相反地，我們必須加入為 TemplateField，將核取方塊 Web 控制項加入其`ItemTemplate`。</span><span class="sxs-lookup"><span data-stu-id="eacf6-150">Instead, we must add a TemplateField and add a CheckBox Web control to its `ItemTemplate`.</span></span> <span data-ttu-id="eacf6-151">請繼續並加入至 TemplateField `Products` GridView，並將其第一個 （最左邊） 欄位。</span><span class="sxs-lookup"><span data-stu-id="eacf6-151">Go ahead and add a TemplateField to the `Products` GridView and make it the first (far-left) field.</span></span> <span data-ttu-id="eacf6-152">從 GridView s 智慧標籤，按一下 [編輯樣板] 連結，然後拖曳核取方塊 Web 控制項從工具箱拖曳到`ItemTemplate`。</span><span class="sxs-lookup"><span data-stu-id="eacf6-152">From the GridView s smart tag, click on the Edit Templates link and then drag a CheckBox Web control from the Toolbox into the `ItemTemplate`.</span></span> <span data-ttu-id="eacf6-153">設定此核取方塊 s`ID`屬性`ProductSelector`。</span><span class="sxs-lookup"><span data-stu-id="eacf6-153">Set this CheckBox s `ID` property to `ProductSelector`.</span></span>


<span data-ttu-id="eacf6-154">[![加入名為 TemplateField 的 ItemTemplate 的 ProductSelector 核取方塊 Web 控制項](adding-a-gridview-column-of-checkboxes-vb/_static/image6.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="eacf6-154">[![Add a CheckBox Web Control Named ProductSelector to the TemplateField s ItemTemplate](adding-a-gridview-column-of-checkboxes-vb/_static/image6.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image11.png)</span></span>

<span data-ttu-id="eacf6-155">**圖 6**： 加入核取方塊 Web 控制項名為`ProductSelector`TemplateField s `ItemTemplate` ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-checkboxes-vb/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="eacf6-155">**Figure 6**: Add a CheckBox Web Control Named `ProductSelector` to the TemplateField s `ItemTemplate` ([Click to view full-size image](adding-a-gridview-column-of-checkboxes-vb/_static/image12.png))</span></span>


<span data-ttu-id="eacf6-156">與加入 TemplateField 和核取方塊 Web 控制項，每個資料列現在包含核取方塊。</span><span class="sxs-lookup"><span data-stu-id="eacf6-156">With the TemplateField and CheckBox Web control added, each row now includes a checkbox.</span></span> <span data-ttu-id="eacf6-157">圖 7 顯示此頁面上，新增 TemplateField 和核取方塊之後，透過瀏覽器中，檢視時。</span><span class="sxs-lookup"><span data-stu-id="eacf6-157">Figure 7 shows this page, when viewed through a browser, after the TemplateField and CheckBox have been added.</span></span>


<span data-ttu-id="eacf6-158">[![每個產品的資料列現在包含核取方塊](adding-a-gridview-column-of-checkboxes-vb/_static/image7.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="eacf6-158">[![Each Product Row Now Includes a Checkbox](adding-a-gridview-column-of-checkboxes-vb/_static/image7.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image13.png)</span></span>

<span data-ttu-id="eacf6-159">**圖 7**： 現在包含每個產品的資料列的核取方塊 ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-checkboxes-vb/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="eacf6-159">**Figure 7**: Each Product Row Now Includes a Checkbox ([Click to view full-size image](adding-a-gridview-column-of-checkboxes-vb/_static/image14.png))</span></span>


## <a name="step-3-determining-what-checkboxes-were-checked-on-postback"></a><span data-ttu-id="eacf6-160">步驟 3： 決定哪些核取方塊在回傳簽入</span><span class="sxs-lookup"><span data-stu-id="eacf6-160">Step 3: Determining What Checkboxes Were Checked On Postback</span></span>

<span data-ttu-id="eacf6-161">現在我們有核取方塊，但沒有方法可以判斷哪些核取方塊在回傳簽入的資料行。</span><span class="sxs-lookup"><span data-stu-id="eacf6-161">At this point we have a column of checkboxes but no way to determine what checkboxes were checked on postback.</span></span> <span data-ttu-id="eacf6-162">按一下 [刪除選取的產品] 按鈕時，不過，我們需要知道哪些核取方塊已接受檢查，以刪除這些產品。</span><span class="sxs-lookup"><span data-stu-id="eacf6-162">When the Delete Selected Products button is clicked, though, we need to know what checkboxes were checked in order to delete those products.</span></span>

<span data-ttu-id="eacf6-163">GridView s [ `Rows`屬性](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.rows.aspx)提供在 GridView 的資料列的存取。</span><span class="sxs-lookup"><span data-stu-id="eacf6-163">The GridView s [`Rows` property](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.rows.aspx) provides access to the data rows in the GridView.</span></span> <span data-ttu-id="eacf6-164">我們可以逐一查看這些資料列，以程式設計方式存取 [CheckBox] 控制項，然後再參閱其`Checked`屬性來判斷是否已選取核取方塊。</span><span class="sxs-lookup"><span data-stu-id="eacf6-164">We can iterate through these rows, programmatically access the CheckBox control, and then consult its `Checked` property to determine whether the CheckBox has been selected.</span></span>

<span data-ttu-id="eacf6-165">建立事件處理常式`DeleteSelectedProducts`按鈕 Web 控制項的`Click`事件並加入下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="eacf6-165">Create an event handler for the `DeleteSelectedProducts` Button Web control s `Click` event and add the following code:</span></span>


[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample2.vb)]

<span data-ttu-id="eacf6-166">`Rows`屬性傳回的集合`GridViewRow`GridView 的資料列執行個體的架構。</span><span class="sxs-lookup"><span data-stu-id="eacf6-166">The `Rows` property returns a collection of `GridViewRow` instances that makeup the GridView s data rows.</span></span> <span data-ttu-id="eacf6-167">`For Each`這裡迴圈列舉此集合。</span><span class="sxs-lookup"><span data-stu-id="eacf6-167">The `For Each` loop here enumerates this collection.</span></span> <span data-ttu-id="eacf6-168">每個`GridViewRow`物件，使用以程式設計方式存取資料列 s 核取方塊`row.FindControl("controlID")`。</span><span class="sxs-lookup"><span data-stu-id="eacf6-168">For each `GridViewRow` object, the row s CheckBox is programmatically accessed using `row.FindControl("controlID")`.</span></span> <span data-ttu-id="eacf6-169">如果勾選此核取方塊，則對應的資料列 s`ProductID`值從擷取`DataKeys`集合。</span><span class="sxs-lookup"><span data-stu-id="eacf6-169">If the CheckBox is checked, the row s corresponding `ProductID` value is retrieved from the `DataKeys` collection.</span></span> <span data-ttu-id="eacf6-170">在此練習中，我們只是顯示在資訊訊息`DeleteResults`加上標籤，雖然可用的應用程式中，我們 d 反而是讓呼叫`ProductsBLL`類別的`DeleteProduct(productID)`方法。</span><span class="sxs-lookup"><span data-stu-id="eacf6-170">In this exercise, we simply display an informative message in the `DeleteResults` Label, although in a working application we d instead make a call to the `ProductsBLL` class s `DeleteProduct(productID)` method.</span></span>

<span data-ttu-id="eacf6-171">加入此事件處理常式之後，按一下 [刪除選取的產品] 按鈕現在會顯示`ProductID`之選取的產品。</span><span class="sxs-lookup"><span data-stu-id="eacf6-171">With the addition of this event handler, clicking the Delete Selected Products button now displays the `ProductID` s of the selected products.</span></span>


<span data-ttu-id="eacf6-172">[![刪除選取的產品按鈕便會列出選取產品 Productid](adding-a-gridview-column-of-checkboxes-vb/_static/image8.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="eacf6-172">[![When the Delete Selected Products Button is Clicked the Selected Products ProductIDs are Listed](adding-a-gridview-column-of-checkboxes-vb/_static/image8.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image15.png)</span></span>

<span data-ttu-id="eacf6-173">**圖 8**： 時刪除選取的產品按鈕選取的產品`ProductID`橋接器列示 ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-checkboxes-vb/_static/image16.png))</span><span class="sxs-lookup"><span data-stu-id="eacf6-173">**Figure 8**: When the Delete Selected Products Button is Clicked the Selected Products `ProductID` s are Listed ([Click to view full-size image](adding-a-gridview-column-of-checkboxes-vb/_static/image16.png))</span></span>


## <a name="step-4-adding-check-all-and-uncheck-all-buttons"></a><span data-ttu-id="eacf6-174">步驟 4： 加入核取所有，並取消選取所有按鈕</span><span class="sxs-lookup"><span data-stu-id="eacf6-174">Step 4: Adding Check All and Uncheck All Buttons</span></span>

<span data-ttu-id="eacf6-175">如果使用者想要刪除目前的頁面上的所有產品，它們必須檢查每十個核取方塊。</span><span class="sxs-lookup"><span data-stu-id="eacf6-175">If a user wants to delete all products on the current page, they must check each of the ten checkboxes.</span></span> <span data-ttu-id="eacf6-176">我們可以協助加速此程序將檢查所有按鈕，按一下時，在方格中選取所有核取方塊。</span><span class="sxs-lookup"><span data-stu-id="eacf6-176">We can help expedite this process by adding a Check All button that, when clicked, selects all of the checkboxes in the grid.</span></span> <span data-ttu-id="eacf6-177">取消核取所有的按鈕將會同樣很有用。</span><span class="sxs-lookup"><span data-stu-id="eacf6-177">An Uncheck All button would be equally helpful.</span></span>

<span data-ttu-id="eacf6-178">將兩個按鈕 Web 控制項加入至頁面上，將它們全放置在 GridView 上方。</span><span class="sxs-lookup"><span data-stu-id="eacf6-178">Add two Button Web controls to the page, placing them above the GridView.</span></span> <span data-ttu-id="eacf6-179">設定第一個 s`ID`至`CheckAll`及其`Text`屬性來檢查所有; 設定第二個一個 s`ID`來`UncheckAll`及其`Text`要取消選取所有屬性。</span><span class="sxs-lookup"><span data-stu-id="eacf6-179">Set the first one s `ID` to `CheckAll` and its `Text` property to Check All ; set the second one s `ID` to `UncheckAll` and its `Text` property to Uncheck All .</span></span>


[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample3.aspx)]

<span data-ttu-id="eacf6-180">接下來，建立名為的程式碼後置類別中的 方法`ToggleCheckState(checkState)`，叫用時，會列舉`Products`GridView s`Rows`集合，並設定每個核取方塊 s`Checked`屬性設為值的傳入中*checkState*參數。</span><span class="sxs-lookup"><span data-stu-id="eacf6-180">Next, create a method in the code-behind class named `ToggleCheckState(checkState)` that, when invoked, enumerates the `Products` GridView s `Rows` collection and sets each CheckBox s `Checked` property to the value of the passed in *checkState* parameter.</span></span>


[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample4.vb)]

<span data-ttu-id="eacf6-181">接下來，建立`Click`事件處理常式`CheckAll`和`UncheckAll`按鈕。</span><span class="sxs-lookup"><span data-stu-id="eacf6-181">Next, create `Click` event handlers for the `CheckAll` and `UncheckAll` buttons.</span></span> <span data-ttu-id="eacf6-182">在`CheckAll`s 事件處理常式，只需呼叫`ToggleCheckState(True)`; 在`UncheckAll`，呼叫`ToggleCheckState(False)`。</span><span class="sxs-lookup"><span data-stu-id="eacf6-182">In `CheckAll` s event handler, simply call `ToggleCheckState(True)`; in `UncheckAll`, call `ToggleCheckState(False)`.</span></span>


[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample5.vb)]

<span data-ttu-id="eacf6-183">使用此程式碼中，按一下核取所有的按鈕導致回傳，並檢查所有在 GridView 的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="eacf6-183">With this code, clicking the Check All button causes a postback and checks all of the checkboxes in the GridView.</span></span> <span data-ttu-id="eacf6-184">同樣地，按一下 取消選取全部取消選取所有核取方塊。</span><span class="sxs-lookup"><span data-stu-id="eacf6-184">Likewise, clicking Uncheck All unselects all checkboxes.</span></span> <span data-ttu-id="eacf6-185">圖 9 顯示螢幕之後已核取核取所有的按鈕。</span><span class="sxs-lookup"><span data-stu-id="eacf6-185">Figure 9 shows the screen after the Check All button has been checked.</span></span>


<span data-ttu-id="eacf6-186">[![按一下核取所有按鈕會選取所有核取方塊](adding-a-gridview-column-of-checkboxes-vb/_static/image9.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image17.png)</span><span class="sxs-lookup"><span data-stu-id="eacf6-186">[![Clicking the Check All Button Selects All Checkboxes](adding-a-gridview-column-of-checkboxes-vb/_static/image9.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image17.png)</span></span>

<span data-ttu-id="eacf6-187">**圖 9**： 按一下核取所有按鈕選取所有核取方塊 ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-checkboxes-vb/_static/image18.png))</span><span class="sxs-lookup"><span data-stu-id="eacf6-187">**Figure 9**: Clicking the Check All Button Selects All Checkboxes ([Click to view full-size image](adding-a-gridview-column-of-checkboxes-vb/_static/image18.png))</span></span>


> [!NOTE]
> <span data-ttu-id="eacf6-188">當顯示的資料行的核取方塊，選取或取消選取所有核取方塊的其中一個方法是透過標頭列中的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="eacf6-188">When displaying a column of checkboxes, one approach for selecting or unselecting all of the checkboxes is through a checkbox in the header row.</span></span> <span data-ttu-id="eacf6-189">此外，目前核取所有/取消選取所有實作都必須回傳。</span><span class="sxs-lookup"><span data-stu-id="eacf6-189">Moreover, the current Check All / Uncheck All implementation requires a postback.</span></span> <span data-ttu-id="eacf6-190">核取方塊可以選取或取消選取，不過，會完全透過用戶端指令碼，進而提供較 snappier 的使用者體驗。</span><span class="sxs-lookup"><span data-stu-id="eacf6-190">The checkboxes can be checked or unchecked, however, entirely through client-side script, thereby providing a snappier user experience.</span></span> <span data-ttu-id="eacf6-191">若要瀏覽詳細資料，以及使用用戶端技術的討論中，使用的所有核取及取消核取所有的標頭資料列核取方塊取出[檢查所有核取方塊在 GridView 使用用戶端指令碼，並選取所有核取方塊](http://aspnet.4guysfromrolla.com/articles/053106-1.aspx)。</span><span class="sxs-lookup"><span data-stu-id="eacf6-191">To explore using a header row checkbox for Check All and Uncheck All in detail, along with a discussion on using client-side techniques, check out [Checking All CheckBoxes in a GridView Using Client-Side Script and a Check All CheckBox](http://aspnet.4guysfromrolla.com/articles/053106-1.aspx).</span></span>


## <a name="summary"></a><span data-ttu-id="eacf6-192">總結</span><span class="sxs-lookup"><span data-stu-id="eacf6-192">Summary</span></span>

<span data-ttu-id="eacf6-193">在您要讓使用者選擇從之前的 GridView 的任意數目的資料列的情況下，加入資料行的核取方塊是其中一個選項。</span><span class="sxs-lookup"><span data-stu-id="eacf6-193">In cases where you need to let users choose an arbitrary number of rows from a GridView before proceeding, adding a column of checkboxes is one option.</span></span> <span data-ttu-id="eacf6-194">如我們所見本教學課程中，核取方塊在 GridView 的資料行包括牽涉到新增為 TemplateField 與核取方塊 Web 控制項。</span><span class="sxs-lookup"><span data-stu-id="eacf6-194">As we saw in this tutorial, including a column of checkboxes in the GridView entails adding a TemplateField with a CheckBox Web control.</span></span> <span data-ttu-id="eacf6-195">使用 Web 控制項 （相對於如我們所做的上一個教學課程中，請直接將此範本中，插入標記） ASP.NET 自動會記住什麼核取方塊已和未檢查跨回傳。</span><span class="sxs-lookup"><span data-stu-id="eacf6-195">By using a Web control (versus injecting markup directly into the template, as we did in the previous tutorial) ASP.NET automatically remembers what CheckBoxes were and were not checked across postback.</span></span> <span data-ttu-id="eacf6-196">我們也以程式設計方式可存取的核取方塊中的程式碼來判斷是否會檢查指定的核取方塊，或變更的核取的狀態。</span><span class="sxs-lookup"><span data-stu-id="eacf6-196">We can also programmatically access the CheckBoxes in code to determine whether a given CheckBox is checked, or to chnage the checked state.</span></span>

<span data-ttu-id="eacf6-197">在 GridView 中加入資料列選取器的資料行，查看本教學課程和最後一個。</span><span class="sxs-lookup"><span data-stu-id="eacf6-197">This tutorial and the last one looked at adding a row selector column to the GridView.</span></span> <span data-ttu-id="eacf6-198">在我們的下一個教學課程中，我們將檢驗如何進行一些工作，我們可以新增插入功能至 GridView。</span><span class="sxs-lookup"><span data-stu-id="eacf6-198">In our next tutorial we'll examine how, with a bit of work, we can add inserting capabilities to the GridView.</span></span>

<span data-ttu-id="eacf6-199">祝您程式設計 ！</span><span class="sxs-lookup"><span data-stu-id="eacf6-199">Happy Programming!</span></span>

## <a name="about-the-author"></a><span data-ttu-id="eacf6-200">關於作者</span><span class="sxs-lookup"><span data-stu-id="eacf6-200">About the Author</span></span>

<span data-ttu-id="eacf6-201">[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。</span><span class="sxs-lookup"><span data-stu-id="eacf6-201">[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), author of seven ASP/ASP.NET books and founder of [4GuysFromRolla.com](http://www.4guysfromrolla.com), has been working with Microsoft Web technologies since 1998.</span></span> <span data-ttu-id="eacf6-202">Scott 可做為獨立顧問、 訓練和寫入器。</span><span class="sxs-lookup"><span data-stu-id="eacf6-202">Scott works as an independent consultant, trainer, and writer.</span></span> <span data-ttu-id="eacf6-203">他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。</span><span class="sxs-lookup"><span data-stu-id="eacf6-203">His latest book is [*Sams Teach Yourself ASP.NET 2.0 in 24 Hours*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco).</span></span> <span data-ttu-id="eacf6-204">他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。</span><span class="sxs-lookup"><span data-stu-id="eacf6-204">He can be reached at [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) or via his blog, which can be found at [http://ScottOnWriting.NET](http://ScottOnWriting.NET).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="eacf6-205">[上一頁](adding-a-gridview-column-of-radio-buttons-vb.md)
[下一頁](inserting-a-new-record-from-the-gridview-s-footer-vb.md)</span><span class="sxs-lookup"><span data-stu-id="eacf6-205">[Previous](adding-a-gridview-column-of-radio-buttons-vb.md)
[Next](inserting-a-new-record-from-the-gridview-s-footer-vb.md)</span></span>
