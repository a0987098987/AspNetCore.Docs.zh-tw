---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: 顯示資料項目及詳細資料 |Microsoft Docs
author: Erikre
description: 本教學課程系列會顯示您建置 ASP.NET 4.7 和 Microsoft Visual Studio 2017 的 ASP.NET Web Forms 應用程式的基本概念
ms.author: riande
ms.date: 1/04/2019
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: acc2f8e78375ef0455d467e2af750ecbee623224
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396217"
---
<a name="display-data-items-and-details"></a><span data-ttu-id="a5a48-103">顯示資料的項目和詳細資料</span><span class="sxs-lookup"><span data-stu-id="a5a48-103">Display data items and details</span></span>
====================
<span data-ttu-id="a5a48-104">藉由[Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="a5a48-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

> <span data-ttu-id="a5a48-105">本系列教學課程將教導您建置 ASP.NET 4.7 和 Microsoft Visual Studio 2017 的 ASP.NET Web Forms 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="a5a48-105">This tutorial series teaches you the basics of building an ASP.NET Web Forms application with ASP.NET 4.7 and Microsoft Visual Studio 2017.</span></span>

<span data-ttu-id="a5a48-106">在本教學課程中，您將了解如何顯示資料的項目和項目詳細資料與 ASP.NET Web Form 和 Entity Framework Code First。</span><span class="sxs-lookup"><span data-stu-id="a5a48-106">In this tutorial, you'll learn how to display data items and data item details with ASP.NET Web Forms and Entity Framework Code First.</span></span> <span data-ttu-id="a5a48-107">本教學課程是根據先前的"UI 和瀏覽 」 教學課程中，做為 Wingtip 玩具店教學課程系列的一部分。</span><span class="sxs-lookup"><span data-stu-id="a5a48-107">This tutorial builds on the previous "UI and Navigation" tutorial as part of the Wingtip Toy Store tutorial series.</span></span> <span data-ttu-id="a5a48-108">完成本教學課程中之後, 您會看到產品*ProductsList.aspx*頁面和產品的詳細資料*ProductDetails.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="a5a48-108">After completing this tutorial, you'll see products on the *ProductsList.aspx* page and a product's details on the *ProductDetails.aspx* page.</span></span>

## <a name="youll-learn-how-to"></a><span data-ttu-id="a5a48-109">您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="a5a48-109">You'll learn how to:</span></span>

- <span data-ttu-id="a5a48-110">加入資料控制項來顯示資料庫中的產品</span><span class="sxs-lookup"><span data-stu-id="a5a48-110">Add a data control to display products from the database</span></span>
- <span data-ttu-id="a5a48-111">資料控制項連接到選取的資料</span><span class="sxs-lookup"><span data-stu-id="a5a48-111">Connect a data control to the selected data</span></span>
- <span data-ttu-id="a5a48-112">加入資料控制項來顯示產品詳細資料，從資料庫</span><span class="sxs-lookup"><span data-stu-id="a5a48-112">Add a data control to display product details from the database</span></span>
- <span data-ttu-id="a5a48-113">擷取查詢字串中的值，並使用該值來限制從資料庫擷取的資料</span><span class="sxs-lookup"><span data-stu-id="a5a48-113">Retrieve a value from the query string and use that value to limit the data that's retrieved from the database</span></span>

### <a name="features-introduced-in-this-tutorial"></a><span data-ttu-id="a5a48-114">在本教學課程中所引進的功能：</span><span class="sxs-lookup"><span data-stu-id="a5a48-114">Features introduced in this tutorial:</span></span>

- <span data-ttu-id="a5a48-115">模型繫結</span><span class="sxs-lookup"><span data-stu-id="a5a48-115">Model binding</span></span>
- <span data-ttu-id="a5a48-116">值提供者</span><span class="sxs-lookup"><span data-stu-id="a5a48-116">Value providers</span></span>

## <a name="add-a-data-control"></a><span data-ttu-id="a5a48-117">加入資料控制項</span><span class="sxs-lookup"><span data-stu-id="a5a48-117">Add a data control</span></span>

<span data-ttu-id="a5a48-118">您可以使用幾個不同的選項，將資料繫結至伺服器控制項。</span><span class="sxs-lookup"><span data-stu-id="a5a48-118">You can use a few different options to bind data to a server control.</span></span> <span data-ttu-id="a5a48-119">最常見的包括：</span><span class="sxs-lookup"><span data-stu-id="a5a48-119">The most common include:</span></span>

 * <span data-ttu-id="a5a48-120">新增資料來源控制項</span><span class="sxs-lookup"><span data-stu-id="a5a48-120">Adding a data source control</span></span>
 * <span data-ttu-id="a5a48-121">以手動方式將程式碼</span><span class="sxs-lookup"><span data-stu-id="a5a48-121">Adding code by hand</span></span>
 * <span data-ttu-id="a5a48-122">使用模型繫結</span><span class="sxs-lookup"><span data-stu-id="a5a48-122">Using model binding</span></span>

### <a name="use-a-data-source-control-to-bind-data"></a><span data-ttu-id="a5a48-123">使用資料來源控制項繫結資料</span><span class="sxs-lookup"><span data-stu-id="a5a48-123">Use a data source control to bind data</span></span>

<span data-ttu-id="a5a48-124">新增資料來源控制項，可讓您連結至控制項顯示資料的資料來源控制項。</span><span class="sxs-lookup"><span data-stu-id="a5a48-124">Adding a data source control allows you to link the data source control to the control that displays the data.</span></span> <span data-ttu-id="a5a48-125">使用此方法時，您可以以宣告方式，而不必以程式設計方式連接到資料來源的 伺服器端控制項。</span><span class="sxs-lookup"><span data-stu-id="a5a48-125">With this approach, you can declaratively,  rather than programmatically, connect server-side controls to data sources.</span></span>

### <a name="code-by-hand-to-bind-data"></a><span data-ttu-id="a5a48-126">以手動方式將資料繫結的程式碼</span><span class="sxs-lookup"><span data-stu-id="a5a48-126">Code by hand to bind data</span></span>

<span data-ttu-id="a5a48-127">手動撰寫程式碼牽涉到：</span><span class="sxs-lookup"><span data-stu-id="a5a48-127">Coding by hand involves:</span></span>

1. <span data-ttu-id="a5a48-128">讀取的值</span><span class="sxs-lookup"><span data-stu-id="a5a48-128">Reading a value</span></span>
2. <span data-ttu-id="a5a48-129">檢查是否為 null</span><span class="sxs-lookup"><span data-stu-id="a5a48-129">Checking if it's null</span></span>
3. <span data-ttu-id="a5a48-130">將它轉換成適當的型別</span><span class="sxs-lookup"><span data-stu-id="a5a48-130">Converting it to an appropriate type</span></span>
4. <span data-ttu-id="a5a48-131">檢查的轉換成功</span><span class="sxs-lookup"><span data-stu-id="a5a48-131">Checking conversion success</span></span>
5. <span data-ttu-id="a5a48-132">在查詢中使用的值</span><span class="sxs-lookup"><span data-stu-id="a5a48-132">Using the value in the query</span></span> 

<span data-ttu-id="a5a48-133">這種方法可讓您可以完全掌控您的資料存取邏輯。</span><span class="sxs-lookup"><span data-stu-id="a5a48-133">This approach lets you have full control over your data-access logic.</span></span>

### <a name="use-model-binding-to-bind-data"></a><span data-ttu-id="a5a48-134">若要將資料繫結中使用模型繫結</span><span class="sxs-lookup"><span data-stu-id="a5a48-134">Use model binding to bind data</span></span>

<span data-ttu-id="a5a48-135">模型繫結可讓您將使用最少的程式碼的結果繫結，並讓您能夠重複使用在您的應用程式的功能。</span><span class="sxs-lookup"><span data-stu-id="a5a48-135">Model binding lets you bind results with far less code and gives you the ability to reuse the functionality throughout your application.</span></span> <span data-ttu-id="a5a48-136">它還能提供豐富的資料繫結架構，簡化程式碼為主的資料存取邏輯的處理。</span><span class="sxs-lookup"><span data-stu-id="a5a48-136">It simplifies working with code-focused data-access logic while still providing a rich, data-binding framework.</span></span>

## <a name="display-products"></a><span data-ttu-id="a5a48-137">顯示的產品</span><span class="sxs-lookup"><span data-stu-id="a5a48-137">Display products</span></span>

<span data-ttu-id="a5a48-138">在本教學課程中，您將使用模型繫結，將資料繫結。</span><span class="sxs-lookup"><span data-stu-id="a5a48-138">In this tutorial, you'll use model binding to bind data.</span></span> <span data-ttu-id="a5a48-139">若要設定資料控制項使用模型繫結來選取資料，您將控制項的`SelectMethod`至網頁的程式碼的方法名稱的屬性。</span><span class="sxs-lookup"><span data-stu-id="a5a48-139">To configure a data control to use model binding to select data, you set the control's `SelectMethod` property to a method name in the page's code.</span></span> <span data-ttu-id="a5a48-140">資料控制項在網頁生命週期中的適當時間呼叫的方法，並自動將傳回的資料繫結。</span><span class="sxs-lookup"><span data-stu-id="a5a48-140">The data control calls the method at the appropriate time in the page life cycle and automatically binds the returned data.</span></span> <span data-ttu-id="a5a48-141">不需要明確呼叫`DataBind`方法。</span><span class="sxs-lookup"><span data-stu-id="a5a48-141">There's no need to explicitly call the `DataBind` method.</span></span>

1. <span data-ttu-id="a5a48-142">在 **方案總管**，開啟*ProductList.aspx*。</span><span class="sxs-lookup"><span data-stu-id="a5a48-142">In **Solution Explorer**, open *ProductList.aspx*.</span></span>
2. <span data-ttu-id="a5a48-143">此標記，取代現有的標記：</span><span class="sxs-lookup"><span data-stu-id="a5a48-143">Replace the existing markup with this markup:</span></span>   

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample1.aspx)]

<span data-ttu-id="a5a48-144">此程式碼會使用**ListView**控制項，名為`productList`顯示的產品。</span><span class="sxs-lookup"><span data-stu-id="a5a48-144">This code uses a **ListView** control named `productList` to display products.</span></span>

[!code-aspx-csharp[Main](display_data_items_and_details/samples/sample2.aspx)]

<span data-ttu-id="a5a48-145">透過範本和樣式，您可以定義如何**ListView**控制項會顯示資料。</span><span class="sxs-lookup"><span data-stu-id="a5a48-145">With templates and styles, you define how the **ListView** control displays data.</span></span> <span data-ttu-id="a5a48-146">它可用於任何重複的結構中的資料。</span><span class="sxs-lookup"><span data-stu-id="a5a48-146">It's useful for data in any repeating structure.</span></span> <span data-ttu-id="a5a48-147">雖然這**ListView**範例只會顯示資料庫資料，您也可以不需要程式碼，讓使用者編輯、 插入和刪除資料，以及排序和分頁資料。</span><span class="sxs-lookup"><span data-stu-id="a5a48-147">Though this **ListView** example simply displays database data, you can also, without code, enable users to edit, insert, and delete data, and to sort and page data.</span></span>

<span data-ttu-id="a5a48-148">藉由設定`ItemType`中的屬性**ListView**控制項，資料繫結運算式`Item`可和控制項成為強型別。</span><span class="sxs-lookup"><span data-stu-id="a5a48-148">By setting the `ItemType` property in the **ListView** control, the data-binding expression `Item` is available and the control becomes strongly typed.</span></span> <span data-ttu-id="a5a48-149">如先前的教學課程中所述，您可以選取具有 IntelliSense、 項目物件的詳細資料，例如指定`ProductName`:</span><span class="sxs-lookup"><span data-stu-id="a5a48-149">As mentioned in the previous tutorial, you can select Item object details with IntelliSense, such as specifying the `ProductName`:</span></span>

![顯示資料的項目和詳細資料-IntelliSense](display_data_items_and_details/_static/image1.png)

<span data-ttu-id="a5a48-151">您也使用模型繫結來指定`SelectMethod`值。</span><span class="sxs-lookup"><span data-stu-id="a5a48-151">You're also using model binding to specify a `SelectMethod` value.</span></span> <span data-ttu-id="a5a48-152">此值 (`GetProducts`) 對應至的方法，您會加入至程式碼後面，以在下一個步驟中顯示的產品。</span><span class="sxs-lookup"><span data-stu-id="a5a48-152">This value (`GetProducts`) corresponds to the method you'll add to the code behind to display products in the next step.</span></span>

### <a name="add-code-to-display-products"></a><span data-ttu-id="a5a48-153">加入程式碼來顯示產品</span><span class="sxs-lookup"><span data-stu-id="a5a48-153">Add code to display products</span></span>

<span data-ttu-id="a5a48-154">在此步驟中，您將新增程式碼以填入**ListView**控制項從資料庫的產品資料。</span><span class="sxs-lookup"><span data-stu-id="a5a48-154">In this step, you'll add code to populate the **ListView** control with product data from the database.</span></span> <span data-ttu-id="a5a48-155">程式碼支援顯示所有產品，以及個別的類別目錄產品。</span><span class="sxs-lookup"><span data-stu-id="a5a48-155">The code supports showing all products and  individual category products.</span></span>

1. <span data-ttu-id="a5a48-156">在 **方案總管**，以滑鼠右鍵按一下*ProductList.aspx* ，然後選取**檢視程式碼**。</span><span class="sxs-lookup"><span data-stu-id="a5a48-156">In **Solution Explorer**, right-click *ProductList.aspx* and then select **View Code**.</span></span>
2. <span data-ttu-id="a5a48-157">取代現有的程式碼中*ProductList.aspx.cs*與這個檔案：</span><span class="sxs-lookup"><span data-stu-id="a5a48-157">Replace the existing code in the *ProductList.aspx.cs* file with this:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

<span data-ttu-id="a5a48-158">此程式碼示範`GetProducts`方法所**ListView**控制項的`ItemType`中的屬性參考*ProductList.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="a5a48-158">This code shows the `GetProducts` method that the **ListView** control's `ItemType` property references in the *ProductList.aspx* page.</span></span> <span data-ttu-id="a5a48-159">若要將結果限制為特定資料庫的類別，程式碼會設定`categoryId`值從查詢字串值傳遞給*ProductList.aspx*頁面時*ProductList.aspx*頁面巡覽至。</span><span class="sxs-lookup"><span data-stu-id="a5a48-159">To limit the results to a specific database category, the code sets the `categoryId` value from the query string value passed to the *ProductList.aspx* page when the *ProductList.aspx* page is navigated to.</span></span> <span data-ttu-id="a5a48-160">`QueryStringAttribute`類別內`System.Web.ModelBinding`命名空間用來擷取查詢字串變數的值`id`。</span><span class="sxs-lookup"><span data-stu-id="a5a48-160">The `QueryStringAttribute` class in the `System.Web.ModelBinding` namespace is used to retrieve the value of the query string variable `id`.</span></span> <span data-ttu-id="a5a48-161">這會指示嘗試將值繫結至查詢字串中的模型繫結`categoryId`在執行階段參數。</span><span class="sxs-lookup"><span data-stu-id="a5a48-161">This instructs model binding to try to bind a value from the query string to the `categoryId` parameter at run time.</span></span>

<span data-ttu-id="a5a48-162">有效的類別目錄作為查詢字串傳遞至頁面時，查詢的結果僅限於這些資料庫中的產品符合`categoryId`值。</span><span class="sxs-lookup"><span data-stu-id="a5a48-162">When a valid category is passed as a query string to the page, the results of the query are limited to those products in the database that match the `categoryId` value.</span></span> <span data-ttu-id="a5a48-163">比方說，如果*ProductsList.aspx*網頁的 URL 如下：</span><span class="sxs-lookup"><span data-stu-id="a5a48-163">For instance, if the *ProductsList.aspx* page URL is this:</span></span>


[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

<span data-ttu-id="a5a48-164">此頁面會顯示只有產品所在`categoryId`等於`1`。</span><span class="sxs-lookup"><span data-stu-id="a5a48-164">The page displays only the products where the `categoryId` equals `1`.</span></span>

<span data-ttu-id="a5a48-165">如果沒有查詢字串包含時，會顯示所有的產品*ProductList.aspx*呼叫頁面。</span><span class="sxs-lookup"><span data-stu-id="a5a48-165">All products are displayed if no query string is included when the *ProductList.aspx* page is called.</span></span>

<span data-ttu-id="a5a48-166">這些方法的值的來源指*值提供者*(例如*QueryString*)，並指出要使用哪些值提供者的參數屬性指*值提供者屬性*(例如`id`)。</span><span class="sxs-lookup"><span data-stu-id="a5a48-166">The sources of values for these methods are referred to as *value providers* (such as *QueryString*), and the parameter attributes that indicate which value provider to use are referred to as *value provider attributes* (such as `id`).</span></span> <span data-ttu-id="a5a48-167">ASP.NET 會包含在 Web Form 應用程式，例如查詢字串、 cookie、 表單值、 控制項、 檢視狀態、 工作階段狀態和設定檔屬性值提供者和對應的屬性，針對所有一般使用者輸入的來源。</span><span class="sxs-lookup"><span data-stu-id="a5a48-167">ASP.NET includes value providers and corresponding attributes for all of the typical sources of user input in a Web Forms application such as the query string, cookies, form values, controls, view state, session state, and profile properties.</span></span> <span data-ttu-id="a5a48-168">您也可以撰寫自訂值提供者。</span><span class="sxs-lookup"><span data-stu-id="a5a48-168">You can also write custom value providers.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="a5a48-169">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="a5a48-169">Run the application</span></span>

<span data-ttu-id="a5a48-170">執行應用程式現在若要檢視所有的產品或分類的產品。</span><span class="sxs-lookup"><span data-stu-id="a5a48-170">Run the application now to view all products or a category's products.</span></span>

1. <span data-ttu-id="a5a48-171">按下**F5**當您在 Visual Studio 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="a5a48-171">Press **F5** while in Visual Studio to run the application.</span></span>  
   <span data-ttu-id="a5a48-172">瀏覽器隨即開啟並顯示*Default.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="a5a48-172">The browser opens and shows the *Default.aspx* page.</span></span>

2. <span data-ttu-id="a5a48-173">選取 **汽車**產品類別目錄瀏覽功能表。</span><span class="sxs-lookup"><span data-stu-id="a5a48-173">Select **Cars** from the product category navigation menu.</span></span>  
   <span data-ttu-id="a5a48-174">*ProductList.aspx*頁面會顯示僅顯示**汽車**類別目錄的產品。</span><span class="sxs-lookup"><span data-stu-id="a5a48-174">The *ProductList.aspx* page displays showing only **Cars** category products.</span></span> <span data-ttu-id="a5a48-175">稍後在本教學課程中，您將會顯示產品詳細資料。</span><span class="sxs-lookup"><span data-stu-id="a5a48-175">Later in this tutorial, you'll display product details.</span></span>  

    ![顯示資料的項目和詳細資料-汽車](display_data_items_and_details/_static/image2.png)

3. <span data-ttu-id="a5a48-177">選取 **產品**從頂端導覽功能表。</span><span class="sxs-lookup"><span data-stu-id="a5a48-177">Select **Products** from the navigation menu at the top.</span></span>  
   <span data-ttu-id="a5a48-178">同樣地， *ProductList.aspx*顯示頁面，但這次它顯示的產品的完整清單。</span><span class="sxs-lookup"><span data-stu-id="a5a48-178">Again, the *ProductList.aspx* page is displayed, however this time it shows the entire list of products.</span></span>   

    ![顯示資料的項目和詳細資料-產品](display_data_items_and_details/_static/image3.png)

4. <span data-ttu-id="a5a48-180">關閉瀏覽器，並返回 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="a5a48-180">Close the browser and return to Visual Studio.</span></span>

### <a name="add-a-data-control-to-display-product-details"></a><span data-ttu-id="a5a48-181">加入資料控制項來顯示產品詳細資料</span><span class="sxs-lookup"><span data-stu-id="a5a48-181">Add a data control to display product details</span></span>

<span data-ttu-id="a5a48-182">接下來，您會修改中的標記*ProductDetails.aspx*您加入在先前的教學課程，以顯示特定產品資訊網頁。</span><span class="sxs-lookup"><span data-stu-id="a5a48-182">Next, you'll modify the markup in the *ProductDetails.aspx* page that you added in the previous tutorial to display specific product information.</span></span>

1. <span data-ttu-id="a5a48-183">在 **方案總管**，開啟*ProductDetails.aspx*。</span><span class="sxs-lookup"><span data-stu-id="a5a48-183">In **Solution Explorer**, open *ProductDetails.aspx*.</span></span>

2. <span data-ttu-id="a5a48-184">此標記，取代現有的標記：</span><span class="sxs-lookup"><span data-stu-id="a5a48-184">Replace the existing markup with this markup:</span></span>

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample5.aspx)] 

    <span data-ttu-id="a5a48-185">此程式碼會使用**FormView**控制項來顯示特定產品詳細資料。</span><span class="sxs-lookup"><span data-stu-id="a5a48-185">This code uses a **FormView** control to display specific product details.</span></span> <span data-ttu-id="a5a48-186">此標記會使用用來顯示資料的方法類似的方法*ProductList.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="a5a48-186">This markup uses methods like the methods used to display data in the *ProductList.aspx* page.</span></span> <span data-ttu-id="a5a48-187">**FormView**控制項用來從資料來源一次顯示單一記錄。</span><span class="sxs-lookup"><span data-stu-id="a5a48-187">The **FormView** control is used to display a single record at a time from a data source.</span></span> <span data-ttu-id="a5a48-188">當您使用**FormView**控制項，建立範本，以顯示和編輯資料繫結值。</span><span class="sxs-lookup"><span data-stu-id="a5a48-188">When you use the **FormView** control, you create templates to display and edit data-bound values.</span></span> <span data-ttu-id="a5a48-189">這些範本包含控制項，繫結運算式，並格式化，定義表單的外觀和功能。</span><span class="sxs-lookup"><span data-stu-id="a5a48-189">These templates contain controls, binding expressions, and formatting that define the form's look and functionality.</span></span>

<span data-ttu-id="a5a48-190">連接到資料庫的上一個標記需要額外的程式碼。</span><span class="sxs-lookup"><span data-stu-id="a5a48-190">Connecting the previous markup to the database requires additional code.</span></span>

1. <span data-ttu-id="a5a48-191">在 **方案總管**，以滑鼠右鍵按一下*ProductDetails.aspx* ，然後按一下 **檢視程式碼**。</span><span class="sxs-lookup"><span data-stu-id="a5a48-191">In **Solution Explorer**, right-click *ProductDetails.aspx* and then click **View Code**.</span></span>  
   <span data-ttu-id="a5a48-192">*ProductDetails.aspx.cs*檔案會顯示。</span><span class="sxs-lookup"><span data-stu-id="a5a48-192">The *ProductDetails.aspx.cs* file is displayed.</span></span>

2. <span data-ttu-id="a5a48-193">此程式碼取代現有的程式碼：</span><span class="sxs-lookup"><span data-stu-id="a5a48-193">Replace the existing code with this code:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

<span data-ttu-id="a5a48-194">此程式碼會檢查 「`productID`"查詢字串值。</span><span class="sxs-lookup"><span data-stu-id="a5a48-194">This code checks for a "`productID`" query-string value.</span></span> <span data-ttu-id="a5a48-195">如果找到有效的查詢字串值，則會顯示相符的產品。</span><span class="sxs-lookup"><span data-stu-id="a5a48-195">If a valid query-string value is found, the matching product is displayed.</span></span> <span data-ttu-id="a5a48-196">如果找不到查詢字串，或其值不是有效的則會不顯示任何產品。</span><span class="sxs-lookup"><span data-stu-id="a5a48-196">If the query-string isn't found, or its value isn't valid, no product is displayed.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="a5a48-197">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="a5a48-197">Run the application</span></span>

<span data-ttu-id="a5a48-198">現在，您可以執行的應用程式，以查看個別的產品，顯示根據產品 id。</span><span class="sxs-lookup"><span data-stu-id="a5a48-198">Now you can run the application to see an individual product displayed based on product ID.</span></span>

1. <span data-ttu-id="a5a48-199">按下**F5**當您在 Visual Studio 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="a5a48-199">Press **F5** while in Visual Studio to run the application.</span></span>  
   <span data-ttu-id="a5a48-200">瀏覽器隨即開啟並顯示*Default.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="a5a48-200">The browser opens and shows the *Default.aspx* page.</span></span>

2. <span data-ttu-id="a5a48-201">選取 **潮水**類別瀏覽功能表。</span><span class="sxs-lookup"><span data-stu-id="a5a48-201">Select **Boats** from the category navigation menu.</span></span>  
   <span data-ttu-id="a5a48-202">*ProductList.aspx*頁面隨即顯示。</span><span class="sxs-lookup"><span data-stu-id="a5a48-202">The *ProductList.aspx* page is displayed.</span></span>

3. <span data-ttu-id="a5a48-203">選取 **紙張船隻**從產品清單。</span><span class="sxs-lookup"><span data-stu-id="a5a48-203">Select **Paper Boat** from the product list.</span></span>
   <span data-ttu-id="a5a48-204">*ProductDetails.aspx*頁面隨即顯示。</span><span class="sxs-lookup"><span data-stu-id="a5a48-204">The *ProductDetails.aspx* page is displayed.</span></span>

    ![顯示資料的項目和詳細資料-產品](display_data_items_and_details/_static/image4.png)
    
4. <span data-ttu-id="a5a48-206">關閉瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="a5a48-206">Close the browser.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="a5a48-207">其他資源</span><span class="sxs-lookup"><span data-stu-id="a5a48-207">Additional resources</span></span>

[<span data-ttu-id="a5a48-208">擷取和顯示資料與模型繫結和 web form</span><span class="sxs-lookup"><span data-stu-id="a5a48-208">Retrieving and displaying data with model binding and web forms</span></span>](../../presenting-and-managing-data/model-binding/retrieving-data.md)

## <a name="next-steps"></a><span data-ttu-id="a5a48-209">後續步驟</span><span class="sxs-lookup"><span data-stu-id="a5a48-209">Next steps</span></span>

<span data-ttu-id="a5a48-210">在本教學課程中，您可以加入標記和程式碼，以顯示產品和產品詳細資料。</span><span class="sxs-lookup"><span data-stu-id="a5a48-210">In this tutorial, you added markup and code to display products and product details.</span></span> <span data-ttu-id="a5a48-211">您已了解強型別的資料控制項、 模型繫結，以及值提供者。</span><span class="sxs-lookup"><span data-stu-id="a5a48-211">You learned about strongly typed data controls, model binding, and value providers.</span></span> <span data-ttu-id="a5a48-212">在下一個教學課程中，您將新增 「 Wingtip Toys 範例應用程式的購物車。</span><span class="sxs-lookup"><span data-stu-id="a5a48-212">In the next tutorial, you'll add a shopping cart to the Wingtip Toys sample application.</span></span> 

> [!div class="step-by-step"]
> <span data-ttu-id="a5a48-213">[上一頁](ui_and_navigation.md)
> [下一頁](shopping-cart.md)</span><span class="sxs-lookup"><span data-stu-id="a5a48-213">[Previous](ui_and_navigation.md)
[Next](shopping-cart.md)</span></span>
