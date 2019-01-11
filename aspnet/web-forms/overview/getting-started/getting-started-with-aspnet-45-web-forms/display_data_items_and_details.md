---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: 顯示資料項目及詳細資料 |Microsoft Docs
author: Erikre
description: 本系列教學課程將教導您建置 ASP.NET Web Forms 應用程式與 ASP.NET 4.7 和 Microsoft Visual Studio Community 2017 for Web 的基本概念
ms.author: riande
ms.date: 1/09/2019
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 73ae1660f5d6e3e28c1c155e745a62936e3502df
ms.sourcegitcommit: cec77d5ad8a0cedb1ecbec32834111492afd0cd2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/10/2019
ms.locfileid: "54207430"
---
<a name="display-data-items-and-details"></a><span data-ttu-id="16321-103">顯示資料的項目和詳細資料</span><span class="sxs-lookup"><span data-stu-id="16321-103">Display data items and details</span></span>
====================
<span data-ttu-id="16321-104">藉由[Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="16321-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

> <span data-ttu-id="16321-105">本系列教學課程將教導您建置 ASP.NET Web Forms 應用程式與 ASP.NET 4.7 和 Microsoft Visual Studio Community 2017 for Web 的基本概念。</span><span class="sxs-lookup"><span data-stu-id="16321-105">This tutorial series teaches you the basics of building an ASP.NET Web Forms application with ASP.NET 4.7 and Microsoft Visual Studio Community 2017 for the Web.</span></span>

<span data-ttu-id="16321-106">在本教學課程中，您將了解如何顯示資料的項目和項目詳細資料與 ASP.NET Web Form 和 Entity Framework Code First。</span><span class="sxs-lookup"><span data-stu-id="16321-106">In this tutorial, you learn how to display data items and data item details with ASP.NET Web Forms and Entity Framework Code First.</span></span> <span data-ttu-id="16321-107">本教學課程是根據先前的"UI 和瀏覽 」 教學課程中，做為 Wingtip 玩具店教學課程系列的一部分。</span><span class="sxs-lookup"><span data-stu-id="16321-107">This tutorial builds on the previous "UI and Navigation" tutorial as part of the Wingtip Toy Store tutorial series.</span></span> <span data-ttu-id="16321-108">在完成教學課程中，產品*ProductsList.aspx*頁面和產品的詳細資料*ProductDetails.aspx*頁面會顯示。</span><span class="sxs-lookup"><span data-stu-id="16321-108">In the completed tutorial, products on the *ProductsList.aspx* page and a product's details on the *ProductDetails.aspx* page are displayed.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="16321-109">您學到什麼</span><span class="sxs-lookup"><span data-stu-id="16321-109">What you learn</span></span>

- <span data-ttu-id="16321-110">加入資料控制項來顯示資料庫的產品。</span><span class="sxs-lookup"><span data-stu-id="16321-110">Add a data control to display database products.</span></span>
- <span data-ttu-id="16321-111">資料控制項連接到選取的資料。</span><span class="sxs-lookup"><span data-stu-id="16321-111">Connect a data control to selected data.</span></span>
- <span data-ttu-id="16321-112">加入資料控制項來顯示產品詳細資料。</span><span class="sxs-lookup"><span data-stu-id="16321-112">Add a data control to display product details.</span></span>
- <span data-ttu-id="16321-113">剖析查詢字串值，並使用它來篩選擷取的資料庫資料。</span><span class="sxs-lookup"><span data-stu-id="16321-113">Parse a query string value and use it to filter retrieved database data.</span></span>

<span data-ttu-id="16321-114">在本教學課程中引進的功能包括模型繫結和值提供者。</span><span class="sxs-lookup"><span data-stu-id="16321-114">Features introduced in this tutorial include model binding and value providers.</span></span>

## <a name="add-a-data-control-to-display-products"></a><span data-ttu-id="16321-115">加入資料控制項顯示的產品</span><span class="sxs-lookup"><span data-stu-id="16321-115">Add a data control to display products</span></span>
 
<span data-ttu-id="16321-116">您必須將資料繫結至伺服器控制項的幾個選項。</span><span class="sxs-lookup"><span data-stu-id="16321-116">You have a few options to bind data to a server control.</span></span> <span data-ttu-id="16321-117">最常見的包括：</span><span class="sxs-lookup"><span data-stu-id="16321-117">The most common include:</span></span>

 * <span data-ttu-id="16321-118">新增資料來源控制項</span><span class="sxs-lookup"><span data-stu-id="16321-118">Adding a data source control</span></span>
 * <span data-ttu-id="16321-119">以手動方式將程式碼</span><span class="sxs-lookup"><span data-stu-id="16321-119">Adding code by hand</span></span>
 * <span data-ttu-id="16321-120">實作的模型繫結</span><span class="sxs-lookup"><span data-stu-id="16321-120">Implementing model binding</span></span>

### <a name="use-a-data-source-control-to-bind-data"></a><span data-ttu-id="16321-121">使用資料來源控制項繫結資料</span><span class="sxs-lookup"><span data-stu-id="16321-121">Use a data source control to bind data</span></span>

<span data-ttu-id="16321-122">新增資料來源控制項來顯示資料的控制項連結資料來源控制項。</span><span class="sxs-lookup"><span data-stu-id="16321-122">Adding a data source control links the data source control to the control that displays the data.</span></span> <span data-ttu-id="16321-123">使用此方法時，您可以以宣告方式，而不必以程式設計方式連接到資料來源的 伺服器端控制項。</span><span class="sxs-lookup"><span data-stu-id="16321-123">With this approach, you can declaratively, rather than programmatically, connect server-side controls to data sources.</span></span>

### <a name="code-by-hand-to-bind-data"></a><span data-ttu-id="16321-124">以手動方式將資料繫結的程式碼</span><span class="sxs-lookup"><span data-stu-id="16321-124">Code by hand to bind data</span></span>

<span data-ttu-id="16321-125">手動撰寫程式碼牽涉到：</span><span class="sxs-lookup"><span data-stu-id="16321-125">Coding by hand involves:</span></span>

1. <span data-ttu-id="16321-126">讀取的值</span><span class="sxs-lookup"><span data-stu-id="16321-126">Reading a value</span></span>
2. <span data-ttu-id="16321-127">檢查是否為 null</span><span class="sxs-lookup"><span data-stu-id="16321-127">Checking if it's null</span></span>
3. <span data-ttu-id="16321-128">將它轉換成適當的型別</span><span class="sxs-lookup"><span data-stu-id="16321-128">Converting it to an appropriate type</span></span>
4. <span data-ttu-id="16321-129">檢查的轉換成功</span><span class="sxs-lookup"><span data-stu-id="16321-129">Checking conversion success</span></span>
5. <span data-ttu-id="16321-130">進行轉換的值的查詢</span><span class="sxs-lookup"><span data-stu-id="16321-130">Making a query with the converted value</span></span> 

<span data-ttu-id="16321-131">使用此方法時，您會有完整控制您的資料存取邏輯。</span><span class="sxs-lookup"><span data-stu-id="16321-131">With this approach, you have full control over your data-access logic.</span></span>

### <a name="use-model-binding-to-bind-data"></a><span data-ttu-id="16321-132">若要將資料繫結中使用模型繫結</span><span class="sxs-lookup"><span data-stu-id="16321-132">Use model binding to bind data</span></span>

<span data-ttu-id="16321-133">使用模型繫結，最少的程式碼的結果繫結，並讓您能夠重複使用在您的應用程式的功能。</span><span class="sxs-lookup"><span data-stu-id="16321-133">With model binding, you bind results with far less code and it gives you the ability to reuse the functionality throughout your application.</span></span> <span data-ttu-id="16321-134">它還能提供豐富的資料繫結架構，簡化程式碼為主的資料存取邏輯的處理。</span><span class="sxs-lookup"><span data-stu-id="16321-134">It simplifies working with code-focused data-access logic while still providing a rich, data-binding framework.</span></span>

## <a name="display-products"></a><span data-ttu-id="16321-135">顯示的產品</span><span class="sxs-lookup"><span data-stu-id="16321-135">Display products</span></span>

<span data-ttu-id="16321-136">在本教學課程中，您可以使用模型繫結將資料繫結。</span><span class="sxs-lookup"><span data-stu-id="16321-136">In this tutorial, you use model binding to bind data.</span></span> <span data-ttu-id="16321-137">若要設定資料控制項使用模型繫結來選取資料，您將控制項的`SelectMethod`屬性頁面的程式碼中的方法。</span><span class="sxs-lookup"><span data-stu-id="16321-137">To configure a data control to use model binding to select data, you set the control's `SelectMethod` property to a method in the page's code.</span></span> <span data-ttu-id="16321-138">資料控制項在網頁生命週期中的適當時間呼叫的方法，並自動將傳回的資料繫結。</span><span class="sxs-lookup"><span data-stu-id="16321-138">The data control calls the method at the appropriate time in the page life cycle and automatically binds the returned data.</span></span> <span data-ttu-id="16321-139">不需要明確呼叫`DataBind`方法。</span><span class="sxs-lookup"><span data-stu-id="16321-139">There's no need to explicitly call the `DataBind` method.</span></span>

<span data-ttu-id="16321-140">您使用此選項，完成下列步驟，修改*ProductList.aspx*標記，以顯示產品。</span><span class="sxs-lookup"><span data-stu-id="16321-140">Working through the following steps, you modify *ProductList.aspx* markup to display products.</span></span>

1. <span data-ttu-id="16321-141">在 **方案總管**，開啟*ProductList.aspx*。</span><span class="sxs-lookup"><span data-stu-id="16321-141">In **Solution Explorer**, open *ProductList.aspx*.</span></span>

2. <span data-ttu-id="16321-142">以下列標記取代現有的標記：</span><span class="sxs-lookup"><span data-stu-id="16321-142">Replace the existing markup with the following markup:</span></span> 

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample1.aspx)]

<span data-ttu-id="16321-143">上述標記會使用**ListView**控制項，名為`productList`顯示的產品。</span><span class="sxs-lookup"><span data-stu-id="16321-143">The preceding markup uses a **ListView** control named `productList` to display products.</span></span>

[!code-aspx-csharp[Main](display_data_items_and_details/samples/sample2.aspx)]

<span data-ttu-id="16321-144">透過範本和樣式，您可以定義如何**ListView**控制項會顯示資料。</span><span class="sxs-lookup"><span data-stu-id="16321-144">With templates and styles, you define how the **ListView** control displays data.</span></span> <span data-ttu-id="16321-145">它可用於任何重複的結構中的資料。</span><span class="sxs-lookup"><span data-stu-id="16321-145">It's useful for data in any repeating structure.</span></span> <span data-ttu-id="16321-146">雖然這**ListView**範例只會顯示資料庫資料，您也可以不需要程式碼，讓使用者編輯、 插入和刪除資料，以及排序和分頁資料。</span><span class="sxs-lookup"><span data-stu-id="16321-146">Though this **ListView** example simply displays database data, you can also, without code, enable users to edit, insert, and delete data, and to sort and page data.</span></span>

<span data-ttu-id="16321-147">當您設定`ItemType`中的屬性**ListView**控制項，資料繫結運算式`Item`可和控制項成為強型別。</span><span class="sxs-lookup"><span data-stu-id="16321-147">When you set the `ItemType` property in the **ListView** control, the data-binding expression `Item` is available and the control becomes strongly typed.</span></span> <span data-ttu-id="16321-148">如先前的教學課程中所述，您可以選取具有 IntelliSense、 項目物件的詳細資料，例如指定`ProductName`:</span><span class="sxs-lookup"><span data-stu-id="16321-148">As mentioned in the previous tutorial, you can select Item object details with IntelliSense, such as specifying the `ProductName`:</span></span>

![顯示資料的項目和詳細資料-IntelliSense](display_data_items_and_details/_static/image1.png)

<span data-ttu-id="16321-150">您正在使用模型繫結，指定`SelectMethod`值 (`GetProducts`)。</span><span class="sxs-lookup"><span data-stu-id="16321-150">With model binding, you're specifying a `SelectMethod` value (`GetProducts`).</span></span> <span data-ttu-id="16321-151">這是您將加入程式碼的方法後面，以在下一個步驟中顯示的產品。</span><span class="sxs-lookup"><span data-stu-id="16321-151">This is the method you add to the code behind to display products in the next step.</span></span>

### <a name="add-code-to-display-products"></a><span data-ttu-id="16321-152">加入程式碼來顯示產品</span><span class="sxs-lookup"><span data-stu-id="16321-152">Add code to display products</span></span>

<span data-ttu-id="16321-153">在此步驟中，您會加入程式碼以填入**ListView**資料庫產品資料的控制項。</span><span class="sxs-lookup"><span data-stu-id="16321-153">In this step, you add code to populate the **ListView** control with database product data.</span></span> <span data-ttu-id="16321-154">程式碼支援顯示所有產品，以及個別的類別目錄產品。</span><span class="sxs-lookup"><span data-stu-id="16321-154">The code supports showing all products and individual category products.</span></span>

1. <span data-ttu-id="16321-155">在 **方案總管**，以滑鼠右鍵按一下*ProductList.aspx* ，然後選取**檢視程式碼**。</span><span class="sxs-lookup"><span data-stu-id="16321-155">In **Solution Explorer**, right-click *ProductList.aspx* and then select **View Code**.</span></span>
2. <span data-ttu-id="16321-156">取代現有的程式碼中*ProductList.aspx.cs*與這個檔案：</span><span class="sxs-lookup"><span data-stu-id="16321-156">Replace the existing code in the *ProductList.aspx.cs* file with this:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

<span data-ttu-id="16321-157">此程式碼示範`GetProducts`方法所**ListView**控制項的`ItemType`中的屬性參考*ProductList.aspx*。</span><span class="sxs-lookup"><span data-stu-id="16321-157">This code shows the `GetProducts` method that the **ListView** control's `ItemType` property references in *ProductList.aspx*.</span></span> <span data-ttu-id="16321-158">若要將結果限制為特定資料庫的類別，程式碼會設定`categoryId`傳遞至查詢字串中的值*ProductList.aspx*。</span><span class="sxs-lookup"><span data-stu-id="16321-158">To limit the results to a specific database category, the code sets the `categoryId` value from the query string passed to *ProductList.aspx*.</span></span> <span data-ttu-id="16321-159">`QueryStringAttribute`類別內`System.Web.ModelBinding`命名空間用來擷取查詢字串變數`id`的值。</span><span class="sxs-lookup"><span data-stu-id="16321-159">The `QueryStringAttribute` class in the `System.Web.ModelBinding` namespace is used to retrieve the query string variable `id`'s value.</span></span> <span data-ttu-id="16321-160">這會指示，請以在執行階段，繫結至查詢字串值的模型繫結`categoryId`參數。</span><span class="sxs-lookup"><span data-stu-id="16321-160">This instructs model binding to, at run time, bind a query string value to the `categoryId` parameter.</span></span>

<span data-ttu-id="16321-161">當有效的分類 (`categoryId`) 是傳遞時，結果會限制為該類別目錄資料庫產品。</span><span class="sxs-lookup"><span data-stu-id="16321-161">When a valid category (`categoryId`) is passed, the results are limited to that category's database products.</span></span> <span data-ttu-id="16321-162">比方說，如果*ProductsList.aspx*網頁的 URL 如下：</span><span class="sxs-lookup"><span data-stu-id="16321-162">For instance, if the *ProductsList.aspx* page URL is this:</span></span>

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

<span data-ttu-id="16321-163">此頁面會顯示只有產品所在`categoryId`等於`1`。</span><span class="sxs-lookup"><span data-stu-id="16321-163">The page displays only the products where the `categoryId` equals `1`.</span></span>

<span data-ttu-id="16321-164">如果沒有查詢字串傳遞，則會顯示所有產品。</span><span class="sxs-lookup"><span data-stu-id="16321-164">All products are displayed if no query string is passed.</span></span>

<span data-ttu-id="16321-165">會呼叫這些方法的值來源*值提供者*(例如`QueryString`)，表示要使用哪些值提供者的參數屬性則稱為*值提供者屬性*(這類`id`)。</span><span class="sxs-lookup"><span data-stu-id="16321-165">The value sources for these methods are called *value providers* (such as `QueryString`), and the parameter attributes that indicate which value provider to use are called *value provider attributes* (such as `id`).</span></span> <span data-ttu-id="16321-166">ASP.NET 會包含值提供者和所有一般 Web Form 應用程式使用者輸入來源的屬性。</span><span class="sxs-lookup"><span data-stu-id="16321-166">ASP.NET includes value providers and attributes for all typical Web Forms application user input sources.</span></span> <span data-ttu-id="16321-167">其中包括查詢字串、 cookie、 表單值、 控制項、 檢視狀態、 工作階段狀態和設定檔屬性。</span><span class="sxs-lookup"><span data-stu-id="16321-167">These include the query string, cookies, form values, controls, view state, session state, and profile properties.</span></span> <span data-ttu-id="16321-168">您也可以撰寫自訂值提供者。</span><span class="sxs-lookup"><span data-stu-id="16321-168">You can also write custom value providers.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="16321-169">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="16321-169">Run the application</span></span>

<span data-ttu-id="16321-170">執行應用程式現在若要檢視所有的產品或分類的產品。</span><span class="sxs-lookup"><span data-stu-id="16321-170">Run the application now to view all products or a category's products.</span></span>

1. <span data-ttu-id="16321-171">在 Visual Studio 中按**F5**執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="16321-171">In Visual Studio, press **F5** to run the application.</span></span>
 <span data-ttu-id="16321-172">瀏覽器隨即開啟並顯示*Default.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="16321-172">The browser opens and shows the *Default.aspx* page.</span></span>

2. <span data-ttu-id="16321-173">從 [產品類別目錄] 功能表中，選取**汽車**。</span><span class="sxs-lookup"><span data-stu-id="16321-173">From the product category menu, select **Cars**.</span></span>

   <span data-ttu-id="16321-174">*ProductList.aspx*頁面隨即出現，顯示只從產品**汽車**類別目錄。</span><span class="sxs-lookup"><span data-stu-id="16321-174">The *ProductList.aspx* page appears, showing only products from the **Cars** category.</span></span> <span data-ttu-id="16321-175">稍後在本教學課程中，您可以顯示產品詳細資料。</span><span class="sxs-lookup"><span data-stu-id="16321-175">Later in this tutorial, you display product details.</span></span>

    ![顯示資料的項目和詳細資料-汽車](display_data_items_and_details/_static/image2.png)

3. <span data-ttu-id="16321-177">選取 **產品**從上方的功能表。</span><span class="sxs-lookup"><span data-stu-id="16321-177">Select **Products** from the top menu.</span></span>
 <span data-ttu-id="16321-178">*ProductList.aspx*頁面現在會顯示所有產品。</span><span class="sxs-lookup"><span data-stu-id="16321-178">The *ProductList.aspx* page now displays all products.</span></span> 

    ![顯示資料的項目和詳細資料-產品](display_data_items_and_details/_static/image3.png)

4. <span data-ttu-id="16321-180">關閉瀏覽器，並返回 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="16321-180">Close the browser and return to Visual Studio.</span></span>

### <a name="add-a-data-control-to-display-product-details"></a><span data-ttu-id="16321-181">加入資料控制項來顯示產品詳細資料</span><span class="sxs-lookup"><span data-stu-id="16321-181">Add a Data Control to display product details</span></span>

<span data-ttu-id="16321-182">修改*ProductDetails.aspx*您在上一個教學課程，以顯示特定產品的資訊中新增的標記：</span><span class="sxs-lookup"><span data-stu-id="16321-182">Modify the *ProductDetails.aspx* markup that you added in the previous tutorial to display specific product information:</span></span>

1. <span data-ttu-id="16321-183">在 **方案總管**，開啟*ProductDetails.aspx*。</span><span class="sxs-lookup"><span data-stu-id="16321-183">In **Solution Explorer**, open *ProductDetails.aspx*.</span></span>

2. <span data-ttu-id="16321-184">此標記，取代現有的標記：</span><span class="sxs-lookup"><span data-stu-id="16321-184">Replace the existing markup with this markup:</span></span>

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample5.aspx)] 

<span data-ttu-id="16321-185">此標記會使用**FormView**控制項來顯示特定產品詳細資料。</span><span class="sxs-lookup"><span data-stu-id="16321-185">This markup uses a **FormView** control to display specific product details.</span></span> <span data-ttu-id="16321-186">它會使用類似用來顯示資料的方法*ProductList.aspx*。</span><span class="sxs-lookup"><span data-stu-id="16321-186">It uses methods like those used to display data in *ProductList.aspx*.</span></span> <span data-ttu-id="16321-187">**FormView**控制項用來從資料來源一次顯示單一記錄。</span><span class="sxs-lookup"><span data-stu-id="16321-187">The **FormView** control is used to display a single record at a time from a data source.</span></span> <span data-ttu-id="16321-188">當您使用**FormView**控制項，建立範本，以顯示和編輯資料繫結值。</span><span class="sxs-lookup"><span data-stu-id="16321-188">When you use the **FormView** control, you create templates to display and edit data-bound values.</span></span> <span data-ttu-id="16321-189">這些範本包含控制項，繫結運算式，並格式化，定義表單的外觀和功能。</span><span class="sxs-lookup"><span data-stu-id="16321-189">These templates contain controls, binding expressions, and formatting that define the form's look and functionality.</span></span>

<span data-ttu-id="16321-190">連接到資料庫的上一個標記需要額外的程式碼。</span><span class="sxs-lookup"><span data-stu-id="16321-190">Connecting the previous markup to the database requires additional code.</span></span>

1. <span data-ttu-id="16321-191">在 **方案總管**，以滑鼠右鍵按一下*ProductDetails.aspx* ，然後選取**檢視程式碼**。</span><span class="sxs-lookup"><span data-stu-id="16321-191">In **Solution Explorer**, right-click *ProductDetails.aspx* and then select **View Code**.</span></span>  
   <span data-ttu-id="16321-192">*ProductDetails.aspx.cs*檔案會顯示。</span><span class="sxs-lookup"><span data-stu-id="16321-192">The *ProductDetails.aspx.cs* file is displayed.</span></span>

2. <span data-ttu-id="16321-193">取代現有的程式碼：</span><span class="sxs-lookup"><span data-stu-id="16321-193">Replace the existing code with this:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

<span data-ttu-id="16321-194">此程式碼會檢查 「`productID`"查詢字串值。</span><span class="sxs-lookup"><span data-stu-id="16321-194">This code checks for a "`productID`" query string value.</span></span> <span data-ttu-id="16321-195">如果找到有效的值，則會顯示相符的產品。</span><span class="sxs-lookup"><span data-stu-id="16321-195">If a valid value is found, the matching product is displayed.</span></span> <span data-ttu-id="16321-196">如果找不到查詢字串，或其值不是有效的則會不顯示任何產品。</span><span class="sxs-lookup"><span data-stu-id="16321-196">If the query string isn't found, or its value isn't valid, no product is displayed.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="16321-197">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="16321-197">Run the application</span></span>

<span data-ttu-id="16321-198">現在您可以在其中執行的應用程式，以查看特定的產品詳細資料，根據產品 id。</span><span class="sxs-lookup"><span data-stu-id="16321-198">Now you can run the application to see specific product details based on product ID.</span></span>

1. <span data-ttu-id="16321-199">在 Visual Studio 中按**F5**執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="16321-199">In Visual Studio, press **F5** to run the application.</span></span>  
 <span data-ttu-id="16321-200">瀏覽器中開啟*Default.aspx*。</span><span class="sxs-lookup"><span data-stu-id="16321-200">The browser opens to *Default.aspx*.</span></span>

2. <span data-ttu-id="16321-201">從 [分類] 功能表中，選取**潮水**。</span><span class="sxs-lookup"><span data-stu-id="16321-201">From the category menu, select **Boats**.</span></span>  
 <span data-ttu-id="16321-202">*ProductList.aspx*頁面隨即顯示。</span><span class="sxs-lookup"><span data-stu-id="16321-202">The *ProductList.aspx* page is displayed.</span></span>

3. <span data-ttu-id="16321-203">選取 **紙張船隻**。</span><span class="sxs-lookup"><span data-stu-id="16321-203">Select **Paper Boat**.</span></span>  
 <span data-ttu-id="16321-204">*ProductDetails.aspx*頁面隨即顯示。</span><span class="sxs-lookup"><span data-stu-id="16321-204">The *ProductDetails.aspx* page is displayed.</span></span>   

    ![顯示資料的項目和詳細資料-產品](display_data_items_and_details/_static/image4.png)

<span data-ttu-id="16321-206">在下一個教學課程中，您加入購物車 Wingtip Toys 應用程式。</span><span class="sxs-lookup"><span data-stu-id="16321-206">In the next tutorial, you add a shopping cart to the Wingtip Toys application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="16321-207">其他資源</span><span class="sxs-lookup"><span data-stu-id="16321-207">Additional resources</span></span>

[<span data-ttu-id="16321-208">擷取和顯示資料與模型繫結和 web form</span><span class="sxs-lookup"><span data-stu-id="16321-208">Retrieving and displaying data with model binding and web forms</span></span>](../../presenting-and-managing-data/model-binding/retrieving-data.md)

> [!div class="step-by-step"]
> <span data-ttu-id="16321-209">[上一頁](ui_and_navigation.md)
> [下一頁](shopping-cart.md)</span><span class="sxs-lookup"><span data-stu-id="16321-209">[Previous](ui_and_navigation.md)
[Next](shopping-cart.md)</span></span>
