---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: 顯示資料項目及詳細資料 |Microsoft Docs
author: Erikre
description: 本系列教學課程將教導您建置使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for 我們的 ASP.NET Web Forms 應用程式的基本概念...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 08efafc1ae076bcf481c5c7d8aea2bbaa704af17
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379734"
---
<a name="display-data-items-and-details"></a><span data-ttu-id="a419c-103">顯示資料項目及詳細資料</span><span class="sxs-lookup"><span data-stu-id="a419c-103">Display Data Items and Details</span></span>
====================
<span data-ttu-id="a419c-104">藉由[Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="a419c-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="a419c-105">[下載 Wingtip Toys 範例專案 (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)或[下載電子書 (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="a419c-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

> <span data-ttu-id="a419c-106">本系列教學課程將教導您建置使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for Web 的 ASP.NET Web Forms 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="a419c-106">This tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="a419c-107">Visual Studio 2013[含有 C# 原始程式碼專案](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)隨附了本教學課程系列。</span><span class="sxs-lookup"><span data-stu-id="a419c-107">A Visual Studio 2013 [project with C# source code](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) is available to accompany this tutorial series.</span></span>


<span data-ttu-id="a419c-108">本教學課程說明如何顯示資料的項目和使用 ASP.NET Web Form 和 Entity Framework Code First 的項目詳細資料。</span><span class="sxs-lookup"><span data-stu-id="a419c-108">This tutorial describes how to display data items and data item details using ASP.NET Web Forms and Entity Framework Code First.</span></span> <span data-ttu-id="a419c-109">此教學課程先前的教學課程 」 UI 和瀏覽 」，並且是 Wingtip 玩具店教學課程系列的一部分。</span><span class="sxs-lookup"><span data-stu-id="a419c-109">This tutorial builds on the previous tutorial "UI and Navigation" and is part of the Wingtip Toy Store tutorial series.</span></span> <span data-ttu-id="a419c-110">當您完成本教學課程中時，您將能夠看到產品上*ProductsList.aspx*頁面和詳細資料上的個別產品*ProductDetails.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="a419c-110">When you've completed this tutorial, you'll be able to see products on the *ProductsList.aspx* page and details about an individual product on the *ProductDetails.aspx* page.</span></span>

## <a name="what-youll-learn"></a><span data-ttu-id="a419c-111">您將學到什麼：</span><span class="sxs-lookup"><span data-stu-id="a419c-111">What you'll learn:</span></span>

- <span data-ttu-id="a419c-112">如何加入資料控制項來顯示資料庫中的產品。</span><span class="sxs-lookup"><span data-stu-id="a419c-112">How to add a data control to display products from the database.</span></span>
- <span data-ttu-id="a419c-113">如何在資料控制項連接到選取的資料。</span><span class="sxs-lookup"><span data-stu-id="a419c-113">How to connect a data control to the selected data.</span></span>
- <span data-ttu-id="a419c-114">如何新增資料控制項來顯示產品詳細資料，從資料庫。</span><span class="sxs-lookup"><span data-stu-id="a419c-114">How to add a data control to display product details from the database.</span></span>
- <span data-ttu-id="a419c-115">如何擷取查詢字串中的值，並使用該值來限制從資料庫擷取的資料。</span><span class="sxs-lookup"><span data-stu-id="a419c-115">How to retrieve a value from the query string and use that value to limit the data that's retrieved from the database.</span></span>

### <a name="these-are-the-features-introduced-in-the-tutorial"></a><span data-ttu-id="a419c-116">在本教學課程中所引進的功能如下：</span><span class="sxs-lookup"><span data-stu-id="a419c-116">These are the features introduced in the tutorial:</span></span>

- <span data-ttu-id="a419c-117">模型繫結</span><span class="sxs-lookup"><span data-stu-id="a419c-117">Model Binding</span></span>
- <span data-ttu-id="a419c-118">值提供者</span><span class="sxs-lookup"><span data-stu-id="a419c-118">Value providers</span></span>

## <a name="adding-a-data-control-to-display-products"></a><span data-ttu-id="a419c-119">將資料控制項加入顯示的產品</span><span class="sxs-lookup"><span data-stu-id="a419c-119">Adding a Data Control to Display Products</span></span>

<span data-ttu-id="a419c-120">當資料繫結至伺服器控制項中，有幾個不同的選項，您可以使用。</span><span class="sxs-lookup"><span data-stu-id="a419c-120">When binding data to a server control, there are a few different options you can use.</span></span> <span data-ttu-id="a419c-121">最常見的選項包括新增資料來源控制項，以手動的方式，將程式碼或使用模型繫結。</span><span class="sxs-lookup"><span data-stu-id="a419c-121">The most common options include adding a data source control, adding code by hand, or using model binding.</span></span>

### <a name="using-a-data-source-control-to-bind-data"></a><span data-ttu-id="a419c-122">使用資料來源控制項繫結資料</span><span class="sxs-lookup"><span data-stu-id="a419c-122">Using a Data Source Control to Bind Data</span></span>

<span data-ttu-id="a419c-123">新增資料來源控制項，可讓您連結至控制項顯示資料的資料來源控制項。</span><span class="sxs-lookup"><span data-stu-id="a419c-123">Adding a data source control allows you to link the data source control to the control that displays the data.</span></span> <span data-ttu-id="a419c-124">這種方法可讓您以宣告方式伺服器端控制項直接連接到資料來源，而不是使用以程式設計的方式。</span><span class="sxs-lookup"><span data-stu-id="a419c-124">This approach allows you to declaratively connect server-side controls directly to data sources, rather than using a programmatic approach.</span></span>

### <a name="coding-by-hand-to-bind-data"></a><span data-ttu-id="a419c-125">撰寫程式碼以手動方式將資料繫結</span><span class="sxs-lookup"><span data-stu-id="a419c-125">Coding By Hand to Bind Data</span></span>

<span data-ttu-id="a419c-126">新增程式碼，以手動方式牽涉到讀取的值、 檢查 null 值、 嘗試將它轉換成適當的型別、 檢查轉換是否成功，以及最後，在查詢中使用的值。</span><span class="sxs-lookup"><span data-stu-id="a419c-126">Adding code by hand involves reading the value, checking for a null value, attempting to convert it to the appropriate type, checking whether the conversion was successful, and finally, using the value in the query.</span></span> <span data-ttu-id="a419c-127">當您需要保留您的資料存取邏輯的完整控制權時，您會使用這種方法。</span><span class="sxs-lookup"><span data-stu-id="a419c-127">You would use this approach when you need to retain full control over your data-access logic.</span></span>

### <a name="using-model-binding-to-bind-data"></a><span data-ttu-id="a419c-128">使用模型繫結至資料繫結</span><span class="sxs-lookup"><span data-stu-id="a419c-128">Using Model Binding to Bind Data</span></span>

<span data-ttu-id="a419c-129">使用模型繫結可讓您將使用最少的程式碼的結果繫結，並讓您能夠重複使用在您的應用程式的功能。</span><span class="sxs-lookup"><span data-stu-id="a419c-129">Using model binding allows you to bind results using far less code and gives you the ability to reuse the functionality throughout your application.</span></span> <span data-ttu-id="a419c-130">模型繫結的目標是要簡化程式碼為主的資料存取邏輯的使用，同時仍然保留的豐富的資料繫結架構的優點。</span><span class="sxs-lookup"><span data-stu-id="a419c-130">Model binding aims to simplify working with code-focused data-access logic while still retaining the benefits of a rich, data-binding framework.</span></span>

## <a name="displaying-products"></a><span data-ttu-id="a419c-131">顯示產品</span><span class="sxs-lookup"><span data-stu-id="a419c-131">Displaying Products</span></span>

<span data-ttu-id="a419c-132">在本教學課程中，您將使用模型繫結，將資料繫結。</span><span class="sxs-lookup"><span data-stu-id="a419c-132">In this tutorial, you'll use model binding to bind data.</span></span> <span data-ttu-id="a419c-133">若要設定資料控制項使用模型繫結來選取資料，您將控制項的`SelectMethod`網頁的程式碼中的方法名稱的屬性。</span><span class="sxs-lookup"><span data-stu-id="a419c-133">To configure a data control to use model binding to select data, you set the control's `SelectMethod` property to the name of a method in the page's code.</span></span> <span data-ttu-id="a419c-134">資料控制項在網頁生命週期中的適當時間呼叫的方法，並自動將傳回的資料繫結。</span><span class="sxs-lookup"><span data-stu-id="a419c-134">The data control calls the method at the appropriate time in the page life cycle and automatically binds the returned data.</span></span> <span data-ttu-id="a419c-135">不需要明確呼叫`DataBind`方法。</span><span class="sxs-lookup"><span data-stu-id="a419c-135">There's no need to explicitly call the `DataBind` method.</span></span>

<span data-ttu-id="a419c-136">使用下列步驟，您會修改中的標記*ProductList.aspx*頁面上，讓網頁可以顯示產品。</span><span class="sxs-lookup"><span data-stu-id="a419c-136">Using the steps below, you'll modify the markup in the *ProductList.aspx* page so that the page can display products.</span></span>

1. <span data-ttu-id="a419c-137">在 **方案總管**，開啟*ProductList.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="a419c-137">In **Solution Explorer**, open the *ProductList.aspx* page.</span></span>
2. <span data-ttu-id="a419c-138">以下列標記取代現有的標記：</span><span class="sxs-lookup"><span data-stu-id="a419c-138">Replace the existing markup with the following markup:</span></span>   

    [!code-aspx[Main](display_data_items_and_details/samples/sample1.aspx)]

<span data-ttu-id="a419c-139">此程式碼會使用**ListView**控制項，名為"productList 」，用於顯示產品。</span><span class="sxs-lookup"><span data-stu-id="a419c-139">This code uses a **ListView** control named "productList" to display the products.</span></span>

[!code-aspx[Main](display_data_items_and_details/samples/sample2.aspx)]

<span data-ttu-id="a419c-140">**ListView**控制項會在您使用範本和樣式定義的格式顯示資料。</span><span class="sxs-lookup"><span data-stu-id="a419c-140">The **ListView** control displays data in a format that you define by using templates and styles.</span></span> <span data-ttu-id="a419c-141">它可用於任何重複的結構中的資料。</span><span class="sxs-lookup"><span data-stu-id="a419c-141">It is useful for data in any repeating structure.</span></span> <span data-ttu-id="a419c-142">這**ListView**範例只會顯示從資料庫中，不過您可以讓使用者編輯、 插入和刪除資料，以及排序和頁面資料，不需要程式碼的資料。</span><span class="sxs-lookup"><span data-stu-id="a419c-142">This **ListView** example simply shows data from the database, however you can enable users to edit, insert, and delete data, and to sort and page data, all without code.</span></span>

<span data-ttu-id="a419c-143">藉由設定`ItemType`中的屬性**ListView**控制項，資料繫結運算式`Item`可和控制項成為強型別。</span><span class="sxs-lookup"><span data-stu-id="a419c-143">By setting the `ItemType` property in the **ListView** control, the data-binding expression `Item` is available and the control becomes strongly typed.</span></span> <span data-ttu-id="a419c-144">如先前的教學課程中所述，您可以選取使用 IntelliSense，例如指定的項目物件的詳細資料`ProductName`:</span><span class="sxs-lookup"><span data-stu-id="a419c-144">As mentioned in the previous tutorial, you can select details of the Item object using IntelliSense, such as specifying the `ProductName`:</span></span>

![顯示資料的項目和詳細資料-IntelliSense](display_data_items_and_details/_static/image1.png)

<span data-ttu-id="a419c-146">此外，您會使用模型繫結來指定`SelectMethod`值。</span><span class="sxs-lookup"><span data-stu-id="a419c-146">In addition, you are using model binding to specify a `SelectMethod` value.</span></span> <span data-ttu-id="a419c-147">此值 (`GetProducts`) 將會對應到您將在下一個步驟中顯示的產品新增到程式碼後置的方法。</span><span class="sxs-lookup"><span data-stu-id="a419c-147">This value (`GetProducts`) will correspond to the method that you will add to the code behind to display products in the next step.</span></span>

### <a name="adding-code-to-display-products"></a><span data-ttu-id="a419c-148">加入程式碼來顯示產品</span><span class="sxs-lookup"><span data-stu-id="a419c-148">Adding Code to Display Products</span></span>

<span data-ttu-id="a419c-149">在此步驟中，您將新增程式碼以填入**ListView**控制項從資料庫的產品資料。</span><span class="sxs-lookup"><span data-stu-id="a419c-149">In this step, you'll add code to populate the **ListView** control with product data from the database.</span></span> <span data-ttu-id="a419c-150">程式碼將會支援依個別的類別，以及顯示所有產品的都顯示產品。</span><span class="sxs-lookup"><span data-stu-id="a419c-150">The code will support showing products by individual category, as well as showing all products.</span></span>

1. <span data-ttu-id="a419c-151">在 **方案總管**，以滑鼠右鍵按一下*ProductList.aspx* ，然後按一下 **檢視程式碼**。</span><span class="sxs-lookup"><span data-stu-id="a419c-151">In **Solution Explorer**, right-click *ProductList.aspx* and then click **View Code**.</span></span>
2. <span data-ttu-id="a419c-152">取代現有的程式碼中*ProductList.aspx.cs*為下列程式碼的檔案：</span><span class="sxs-lookup"><span data-stu-id="a419c-152">Replace the existing code in the *ProductList.aspx.cs* file with the following code:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

<span data-ttu-id="a419c-153">此程式碼示範`GetProducts`所參考的方法`ItemType`屬性**ListView**控制中*ProductList.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="a419c-153">This code shows the `GetProducts` method that's referenced by the `ItemType` property of the **ListView** control in the *ProductList.aspx* page.</span></span> <span data-ttu-id="a419c-154">若要將結果限制為資料庫中的特定類別，程式碼會設定`categoryId`值從查詢字串值傳遞給*ProductList.aspx*頁面時*ProductList.aspx*頁面巡覽至。</span><span class="sxs-lookup"><span data-stu-id="a419c-154">To limit the results to a specific category in the database, the code sets the `categoryId` value from the query string value passed to the *ProductList.aspx* page when the *ProductList.aspx* page is navigated to.</span></span> <span data-ttu-id="a419c-155">`QueryStringAttribute`類別中`System.Web.ModelBinding`命名空間用來擷取查詢字串變數識別碼的值。這會指示嘗試將值繫結至查詢字串中的模型繫結`categoryId`在執行階段參數。</span><span class="sxs-lookup"><span data-stu-id="a419c-155">The `QueryStringAttribute` class in the `System.Web.ModelBinding` namespace is used to retrieve the value of the query string variable id. This instructs model binding to try to bind a value from the query string to the `categoryId` parameter at run time.</span></span>

<span data-ttu-id="a419c-156">有效的類別目錄作為查詢字串傳遞至頁面時，查詢的結果僅限於這些資料庫中的產品符合`categoryId`值。</span><span class="sxs-lookup"><span data-stu-id="a419c-156">When a valid category is passed as a query string to the page, the results of the query are limited to those products in the database that match the `categoryId` value.</span></span> <span data-ttu-id="a419c-157">比方說，如果 URL *ProductsList.aspx*頁面如下：</span><span class="sxs-lookup"><span data-stu-id="a419c-157">For instance, if the URL to the *ProductsList.aspx* page is the following:</span></span>

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

<span data-ttu-id="a419c-158">此頁面會顯示只有產品所在`category`等於`1`。</span><span class="sxs-lookup"><span data-stu-id="a419c-158">The page displays only the products where the `category` equals `1`.</span></span>

<span data-ttu-id="a419c-159">瀏覽至時，如果沒有查詢字串包含*ProductList.aspx*  頁面上，將會顯示所有產品。</span><span class="sxs-lookup"><span data-stu-id="a419c-159">If no query string is included when navigating to the *ProductList.aspx* page, all products will be displayed.</span></span>

<span data-ttu-id="a419c-160">這些方法的值的來源指*值提供者*(例如*QueryString*)，並指出要使用哪些值提供者的參數屬性指值提供者屬性 (例如 「`id`")。</span><span class="sxs-lookup"><span data-stu-id="a419c-160">The sources of values for these methods are referred to as *value providers* (such as *QueryString*), and the parameter attributes that indicate which value provider to use are referred to as value provider attributes (such as "`id`").</span></span> <span data-ttu-id="a419c-161">ASP.NET Web Form 應用程式中，例如查詢字串、 cookie、 表單值、 控制項、 檢視狀態、 工作階段狀態和設定檔屬性中包含值提供者和對應的屬性，針對所有一般使用者輸入的來源。</span><span class="sxs-lookup"><span data-stu-id="a419c-161">ASP.NET includes value providers and corresponding attributes for all of the typical sources of user input in a Web Forms application, such as the query string, cookies, form values, controls, view state, session state, and profile properties.</span></span> <span data-ttu-id="a419c-162">您也可以撰寫自訂值提供者。</span><span class="sxs-lookup"><span data-stu-id="a419c-162">You can also write custom value providers.</span></span>

### <a name="running-the-application"></a><span data-ttu-id="a419c-163">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="a419c-163">Running the Application</span></span>

<span data-ttu-id="a419c-164">執行應用程式現在若要查看如何檢視所有的產品或只是一組限制類別目錄的產品。</span><span class="sxs-lookup"><span data-stu-id="a419c-164">Run the application now to see how you can view all of the products or just a set of products limited by category.</span></span>

1. <span data-ttu-id="a419c-165">在 [**方案總管] 中**，以滑鼠右鍵按一下*Default.aspx*頁面，然後選取**瀏覽器中的檢視**。</span><span class="sxs-lookup"><span data-stu-id="a419c-165">In the **Solution Explorer**, right-click the *Default.aspx* page and select **View in Browser**.</span></span>  
 <span data-ttu-id="a419c-166">瀏覽器會開啟並顯示*Default.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="a419c-166">The browser will open and show the *Default.aspx* page.</span></span>
2. <span data-ttu-id="a419c-167">選取 **汽車**產品類別目錄瀏覽功能表。</span><span class="sxs-lookup"><span data-stu-id="a419c-167">Select **Cars** from the product category navigation menu.</span></span>  
 <span data-ttu-id="a419c-168">*ProductList.aspx*頁面會顯示 [Cars] 分類中包含的產品。</span><span class="sxs-lookup"><span data-stu-id="a419c-168">The *ProductList.aspx* page is displayed showing only products included in the "Cars" category.</span></span> <span data-ttu-id="a419c-169">稍後在本教學課程中，您將會顯示產品詳細資料。</span><span class="sxs-lookup"><span data-stu-id="a419c-169">Later in this tutorial, you will display product details.</span></span>  

    ![顯示資料的項目和詳細資料-汽車](display_data_items_and_details/_static/image2.png)
3. <span data-ttu-id="a419c-171">選取 **產品**從頂端導覽功能表。</span><span class="sxs-lookup"><span data-stu-id="a419c-171">Select **Products** from the navigation menu at the top.</span></span>  
 <span data-ttu-id="a419c-172">同樣地， *ProductList.aspx*顯示頁面，但這次它顯示的產品的完整清單。</span><span class="sxs-lookup"><span data-stu-id="a419c-172">Again, the *ProductList.aspx* page is displayed, however this time it shows the entire list of products.</span></span>   

    ![顯示資料的項目和詳細資料-產品](display_data_items_and_details/_static/image3.png)
4. <span data-ttu-id="a419c-174">關閉瀏覽器，並返回 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="a419c-174">Close the browser and return to Visual Studio.</span></span>

### <a name="adding-a-data-control-to-display-product-details"></a><span data-ttu-id="a419c-175">新增資料控制項來顯示產品詳細資料</span><span class="sxs-lookup"><span data-stu-id="a419c-175">Adding a Data Control to Display Product Details</span></span>

<span data-ttu-id="a419c-176">接下來，您會修改中的標記*ProductDetails.aspx* ，讓網頁可以顯示個別產品的相關資訊加入先前的教學課程中的頁面。</span><span class="sxs-lookup"><span data-stu-id="a419c-176">Next, you'll modify the markup in the *ProductDetails.aspx* page that you added in the previous tutorial so that the page can display information about an individual product.</span></span>

1. <span data-ttu-id="a419c-177">在 **方案總管**，開啟*ProductDetails.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="a419c-177">In **Solution Explorer**, open the *ProductDetails.aspx* page.</span></span>
2. <span data-ttu-id="a419c-178">以下列標記取代現有的標記：</span><span class="sxs-lookup"><span data-stu-id="a419c-178">Replace the existing markup with the following markup:</span></span>   

    [!code-aspx[Main](display_data_items_and_details/samples/sample5.aspx)]

<span data-ttu-id="a419c-179">此程式碼會使用**FormView**控制項來顯示有關個別產品的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="a419c-179">This code uses a **FormView** control to display details about an individual product.</span></span> <span data-ttu-id="a419c-180">此標記會使用類似用來顯示資料的方法*ProductList.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="a419c-180">This markup uses methods like those that are used to display data in the *ProductList.aspx* page.</span></span> <span data-ttu-id="a419c-181">**FormView**控制項用來從資料來源一次顯示單一記錄。</span><span class="sxs-lookup"><span data-stu-id="a419c-181">The **FormView** control is used to display a single record at a time from a data source.</span></span> <span data-ttu-id="a419c-182">當您使用**FormView**控制項，建立範本，以顯示和編輯資料繫結值。</span><span class="sxs-lookup"><span data-stu-id="a419c-182">When you use the **FormView** control, you create templates to display and edit data-bound values.</span></span> <span data-ttu-id="a419c-183">範本包含的控制項，繫結運算式和格式化的外觀和功能的形式。</span><span class="sxs-lookup"><span data-stu-id="a419c-183">The templates contain controls, binding expressions, and formatting that define the look and functionality of the form.</span></span>

<span data-ttu-id="a419c-184">若要連接到資料庫的上述的標記，您必須新增其他程式碼以*ProductDetails.aspx*程式碼。</span><span class="sxs-lookup"><span data-stu-id="a419c-184">To connect the above markup to the database, you must add additional code to the *ProductDetails.aspx* code.</span></span>

1. <span data-ttu-id="a419c-185">在 **方案總管**，以滑鼠右鍵按一下*ProductDetails.aspx* ，然後按一下 **檢視程式碼**。</span><span class="sxs-lookup"><span data-stu-id="a419c-185">In **Solution Explorer**, right-click *ProductDetails.aspx* and then click **View Code**.</span></span>  
   <span data-ttu-id="a419c-186">*ProductDetails.aspx.cs*檔案就會顯示。</span><span class="sxs-lookup"><span data-stu-id="a419c-186">The *ProductDetails.aspx.cs* file will be displayed.</span></span>
2. <span data-ttu-id="a419c-187">將現有的程式碼更換成下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="a419c-187">Replace the existing code with the following code:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

<span data-ttu-id="a419c-188">此程式碼會檢查 「`productID`"查詢字串值。</span><span class="sxs-lookup"><span data-stu-id="a419c-188">This code checks for a "`productID`" query-string value.</span></span> <span data-ttu-id="a419c-189">如果找到有效的查詢字串值，則會顯示相符的產品。</span><span class="sxs-lookup"><span data-stu-id="a419c-189">If a valid query-string value is found, the matching product is displayed.</span></span> <span data-ttu-id="a419c-190">如果不找到任何查詢字串，或查詢字串值無效，沒有任何產品會顯示在*ProductDetails.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="a419c-190">If no query-string is found, or the query-string value is not valid, no product is displayed on the *ProductDetails.aspx* page.</span></span>

### <a name="running-the-application"></a><span data-ttu-id="a419c-191">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="a419c-191">Running the Application</span></span>

<span data-ttu-id="a419c-192">現在，您可以執行的應用程式，以查看個別的產品，顯示為基礎的產品識別碼。</span><span class="sxs-lookup"><span data-stu-id="a419c-192">Now you can run the application to see an individual product displayed based on the id of the product.</span></span>

1. <span data-ttu-id="a419c-193">按下**F5**當您在 Visual Studio 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="a419c-193">Press **F5** while in Visual Studio to run the application.</span></span>  
 <span data-ttu-id="a419c-194">瀏覽器會開啟並顯示*Default.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="a419c-194">The browser will open and show the *Default.aspx* page.</span></span>
2. <span data-ttu-id="a419c-195">從 [類別瀏覽] 功能表中選取 > 潮水"。</span><span class="sxs-lookup"><span data-stu-id="a419c-195">Select "Boats" from the category navigation menu.</span></span>  
 <span data-ttu-id="a419c-196">*ProductList.aspx*頁面隨即顯示。</span><span class="sxs-lookup"><span data-stu-id="a419c-196">The *ProductList.aspx* page is displayed.</span></span>
3. <span data-ttu-id="a419c-197">從產品清單中選取 「 紙船隻 」 產品。</span><span class="sxs-lookup"><span data-stu-id="a419c-197">Select the "Paper Boat" product from the product list.</span></span>  
 <span data-ttu-id="a419c-198">*ProductDetails.aspx*頁面隨即顯示。</span><span class="sxs-lookup"><span data-stu-id="a419c-198">The *ProductDetails.aspx* page is displayed.</span></span>   

    ![顯示資料的項目和詳細資料-產品](display_data_items_and_details/_static/image4.png)
4. <span data-ttu-id="a419c-200">關閉瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="a419c-200">Close the browser.</span></span>

## <a name="summary"></a><span data-ttu-id="a419c-201">總結</span><span class="sxs-lookup"><span data-stu-id="a419c-201">Summary</span></span>

<span data-ttu-id="a419c-202">在本教學課程系列的中，您必須新增標記和程式碼顯示一份產品清單，並顯示產品詳細資料。</span><span class="sxs-lookup"><span data-stu-id="a419c-202">In this tutorial of the series you have add markup and code to display a product list and to display product details.</span></span> <span data-ttu-id="a419c-203">在此過程中，您已了解強型別的資料控制項、 模型繫結，以及值提供者。</span><span class="sxs-lookup"><span data-stu-id="a419c-203">During this process you have learned about strongly typed data controls, model binding, and value providers.</span></span> <span data-ttu-id="a419c-204">在下一個教學課程中，您將新增 「 Wingtip Toys 範例應用程式的購物車。</span><span class="sxs-lookup"><span data-stu-id="a419c-204">In the next tutorial, you'll add a shopping cart to the Wingtip Toys sample application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a419c-205">其他資源</span><span class="sxs-lookup"><span data-stu-id="a419c-205">Additional Resources</span></span>

[<span data-ttu-id="a419c-206">擷取和顯示資料與模型繫結和 web form</span><span class="sxs-lookup"><span data-stu-id="a419c-206">Retrieving and displaying data with model binding and web forms</span></span>](../../presenting-and-managing-data/model-binding/retrieving-data.md)

> [!div class="step-by-step"]
> <span data-ttu-id="a419c-207">[上一頁](ui_and_navigation.md)
> [下一頁](shopping-cart.md)</span><span class="sxs-lookup"><span data-stu-id="a419c-207">[Previous](ui_and_navigation.md)
[Next](shopping-cart.md)</span></span>
