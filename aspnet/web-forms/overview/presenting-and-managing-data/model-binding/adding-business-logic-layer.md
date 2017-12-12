---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: "若要使用模型繫結和 web form 的專案加入商務邏輯層 |Microsoft 文件"
author: tfitzmac
description: "此教學課程系列示範與 ASP.NET Web Form 專案中使用模型繫結的基本層面。 模型繫結進行資料互動詳細直線-..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: ca50690052cca73a718342a9725c8096a72f1187
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a><span data-ttu-id="52676-104">加入商務邏輯層級使用模型繫結和 web form 的專案</span><span class="sxs-lookup"><span data-stu-id="52676-104">Adding business logic layer to a project that uses model binding and web forms</span></span>
====================
<span data-ttu-id="52676-105">由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="52676-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="52676-106">此教學課程系列示範與 ASP.NET Web Form 專案中使用模型繫結的基本層面。</span><span class="sxs-lookup"><span data-stu-id="52676-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="52676-107">模型繫結進行更直覺比處理的資料來源物件 （例如 ObjectDataSource 或 SqlDataSource） 的資料互動。</span><span class="sxs-lookup"><span data-stu-id="52676-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="52676-108">此數列開頭簡介資料，並會移到更進階的概念中之後的教學課程。</span><span class="sxs-lookup"><span data-stu-id="52676-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="52676-109">本教學課程會示範如何使用商務邏輯層包含模型繫結。</span><span class="sxs-lookup"><span data-stu-id="52676-109">This tutorial shows how to use model binding with a business logic layer.</span></span> <span data-ttu-id="52676-110">您將設定 OnCallingDataMethods 成員來指定目前的頁面以外的物件用來呼叫資料方法。</span><span class="sxs-lookup"><span data-stu-id="52676-110">You will set the OnCallingDataMethods member to specify that an object other than the current page is used to call the data methods.</span></span>
> 
> <span data-ttu-id="52676-111">本教學課程中建立的專案是[早](retrieving-data.md)數列的組件。</span><span class="sxs-lookup"><span data-stu-id="52676-111">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="52676-112">您可以[下載](https://go.microsoft.com/fwlink/?LinkId=286116)以 C# 或 VB 完整的專案</span><span class="sxs-lookup"><span data-stu-id="52676-112">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="52676-113">可下載的程式碼適用於 Visual Studio 2012 或 Visual Studio 2013。</span><span class="sxs-lookup"><span data-stu-id="52676-113">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="52676-114">它會使用 Visual Studio 2012 範本，這可能會比 Visual Studio 2013 範本顯示在本教學課程稍有不同。</span><span class="sxs-lookup"><span data-stu-id="52676-114">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="52676-115">您將建置</span><span class="sxs-lookup"><span data-stu-id="52676-115">What you'll build</span></span>

<span data-ttu-id="52676-116">模型繫結可讓您放置資料互動的程式碼，可能是程式碼後置檔案中的網頁或不同的商務邏輯類別中。</span><span class="sxs-lookup"><span data-stu-id="52676-116">Model binding enables you to put your data interaction code in either the code-behind file for a web page or in a separate business logic class.</span></span> <span data-ttu-id="52676-117">先前的教學課程顯示如何針對資料互動的程式碼使用程式碼後置檔案。</span><span class="sxs-lookup"><span data-stu-id="52676-117">The previous tutorials have shown how to use the code-behind files for data interaction code.</span></span> <span data-ttu-id="52676-118">這個方法適用於小型站台，但它可能會導致程式碼重複和更不容易維護的大型網站時。</span><span class="sxs-lookup"><span data-stu-id="52676-118">This approach works for small sites but it can lead to code repetition and greater difficulty when maintaining a large site.</span></span> <span data-ttu-id="52676-119">它也可以很難測試位於程式碼後置檔案，因為沒有任何抽象層的程式碼，以程式設計的方式。</span><span class="sxs-lookup"><span data-stu-id="52676-119">It can also be very difficult to programmatically test code that resides in code behind files because there is no abstraction layer.</span></span>

<span data-ttu-id="52676-120">要集中管理資料互動的程式碼，您可以建立包含所有的邏輯與資料互動的商務邏輯層。</span><span class="sxs-lookup"><span data-stu-id="52676-120">To centralize the data interaction code, you can create a business logic layer that contains all of the logic for interacting with data.</span></span> <span data-ttu-id="52676-121">然後，您會呼叫商務邏輯層從您網頁。</span><span class="sxs-lookup"><span data-stu-id="52676-121">You then call the business logic layer from your web pages.</span></span> <span data-ttu-id="52676-122">本教學課程會示範如何將所有的商務邏輯層到先前的教學課程中您撰寫的程式碼，然後使用 從網頁程式碼。</span><span class="sxs-lookup"><span data-stu-id="52676-122">This tutorial shows how to move all of the code you have written in the previous tutorials into a business logic layer, and then use that code from the pages.</span></span>

<span data-ttu-id="52676-123">在此教學課程中，您將會：</span><span class="sxs-lookup"><span data-stu-id="52676-123">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="52676-124">程式碼後置檔案中的程式碼移到商務邏輯層</span><span class="sxs-lookup"><span data-stu-id="52676-124">Move the code from code-behind files to a business logic layer</span></span>
2. <span data-ttu-id="52676-125">變更您的資料繫結控制項，以呼叫商務邏輯層中的方法</span><span class="sxs-lookup"><span data-stu-id="52676-125">Change your data bound controls to call the methods in the business logic layer</span></span>

## <a name="create-business-logic-layer"></a><span data-ttu-id="52676-126">建立商務邏輯層</span><span class="sxs-lookup"><span data-stu-id="52676-126">Create business logic layer</span></span>

<span data-ttu-id="52676-127">現在，您將建立會從網頁呼叫的類別。</span><span class="sxs-lookup"><span data-stu-id="52676-127">Now, you will create the class that is called from the web pages.</span></span> <span data-ttu-id="52676-128">這個類別中的方法看起來類似您在先前的教學課程中使用的方法，並包含值提供者屬性。</span><span class="sxs-lookup"><span data-stu-id="52676-128">The methods in this class look similar to the methods you used in the previous tutorials and include the value provider attributes.</span></span>

<span data-ttu-id="52676-129">首先，新增新的資料夾，稱為**BLL**。</span><span class="sxs-lookup"><span data-stu-id="52676-129">First, add a new folder called **BLL**.</span></span>

![新增資料夾](adding-business-logic-layer/_static/image1.png)

<span data-ttu-id="52676-131">在 BLL 資料夾中建立新的類別，名為**SchoolBL.cs**。</span><span class="sxs-lookup"><span data-stu-id="52676-131">In the BLL folder, create a new class named **SchoolBL.cs**.</span></span> <span data-ttu-id="52676-132">它會包含所有的程式碼後置檔案最初的資料作業。</span><span class="sxs-lookup"><span data-stu-id="52676-132">It will contain all of the data operations that originally resided in code-behind files.</span></span> <span data-ttu-id="52676-133">方法會在程式碼後置檔案中，方法幾乎完全相同，但會包括一些變更。</span><span class="sxs-lookup"><span data-stu-id="52676-133">The methods are almost the same as the methods in the code-behind file, but will include some changes.</span></span>

<span data-ttu-id="52676-134">請注意最重要的變更是不會再執行中的程式碼執行個體內**頁面**類別。</span><span class="sxs-lookup"><span data-stu-id="52676-134">The most important change to note is that you are no longer executing the code from within an instance of **Page** class.</span></span> <span data-ttu-id="52676-135">頁面類別包含**TryUpdateModel**方法和**ModelState**屬性。</span><span class="sxs-lookup"><span data-stu-id="52676-135">The Page class contains the **TryUpdateModel** method and the **ModelState** property.</span></span> <span data-ttu-id="52676-136">此程式碼移到商務邏輯層中，當您不再需要呼叫這些成員頁面類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="52676-136">When this code is moved to a business logic layer, you no longer have an instance of the Page class to call these members.</span></span> <span data-ttu-id="52676-137">若要解決此問題，您必須新增**ModelMethodContext**存取 TryUpdateModel 或 ModelState 任何方法的參數。</span><span class="sxs-lookup"><span data-stu-id="52676-137">To get around this issue, you must add a **ModelMethodContext** parameter to any method that accesses TryUpdateModel or ModelState.</span></span> <span data-ttu-id="52676-138">您可以使用這個 ModelMethodContext 參數來呼叫 TryUpdateModel 或擷取 ModelState。</span><span class="sxs-lookup"><span data-stu-id="52676-138">You use this ModelMethodContext parameter to call TryUpdateModel or retrieve ModelState.</span></span> <span data-ttu-id="52676-139">您不需要變更任何項目在網頁上的這個新的參數。</span><span class="sxs-lookup"><span data-stu-id="52676-139">You do not need to change anything in the web page to account for this new parameter.</span></span>

<span data-ttu-id="52676-140">SchoolBL.cs 中的程式碼取代為下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="52676-140">Replace the code in SchoolBL.cs with the following code.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a><span data-ttu-id="52676-141">修改現有的頁面，來擷取商務邏輯層中的資料</span><span class="sxs-lookup"><span data-stu-id="52676-141">Revise existing pages to retrieve data from business logic layer</span></span>

<span data-ttu-id="52676-142">最後，您會將頁面 Students.aspx、 AddStudent.aspx，以及 Courses.aspx 從在要使用商務邏輯層的程式碼後置檔案中使用查詢轉換。</span><span class="sxs-lookup"><span data-stu-id="52676-142">Finally, you will convert the pages Students.aspx, AddStudent.aspx, and Courses.aspx from using queries in the code-behind file to using the business logic layer.</span></span>

<span data-ttu-id="52676-143">在程式碼後置檔案學生版 AddStudent，和課程中，刪除或標記為註解下列的查詢方法：</span><span class="sxs-lookup"><span data-stu-id="52676-143">In the code-behind files for Students, AddStudent, and Courses, delete or comment out the following query methods:</span></span>

- <span data-ttu-id="52676-144">studentsGrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="52676-144">studentsGrid\_GetData</span></span>
- <span data-ttu-id="52676-145">studentsGrid\_UpdateItem</span><span class="sxs-lookup"><span data-stu-id="52676-145">studentsGrid\_UpdateItem</span></span>
- <span data-ttu-id="52676-146">studentsGrid\_DeleteItem</span><span class="sxs-lookup"><span data-stu-id="52676-146">studentsGrid\_DeleteItem</span></span>
- <span data-ttu-id="52676-147">addStudentForm\_InsertItem</span><span class="sxs-lookup"><span data-stu-id="52676-147">addStudentForm\_InsertItem</span></span>
- <span data-ttu-id="52676-148">coursesGrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="52676-148">coursesGrid\_GetData</span></span>

<span data-ttu-id="52676-149">您現在應該有任何程式碼的相關資料的作業程式碼後置檔案中。</span><span class="sxs-lookup"><span data-stu-id="52676-149">You now should have no code in the code-behind file that pertains to data operations.</span></span>

<span data-ttu-id="52676-150">**OnCallingDataMethods**事件處理常式可讓您指定要用於資料方法的物件。</span><span class="sxs-lookup"><span data-stu-id="52676-150">The **OnCallingDataMethods** event handler enables you to specify an object to use for the data methods.</span></span> <span data-ttu-id="52676-151">在 Students.aspx，新增該事件處理常式的值，將資料方法的名稱變更為商務邏輯類別中方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="52676-151">In Students.aspx, add a value for that event handler and change the names of the data methods to the names of the methods in the business logic class.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

<span data-ttu-id="52676-152">在 Students.aspx 的程式碼後置檔案，定義 CallingDataMethods 事件的事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="52676-152">In the code-behind file for Students.aspx, define the event handler for the CallingDataMethods event.</span></span> <span data-ttu-id="52676-153">在此事件處理常式中，您可以指定資料作業的商務邏輯類別。</span><span class="sxs-lookup"><span data-stu-id="52676-153">In this event handler, you specify the business logic class for data operations.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

<span data-ttu-id="52676-154">在 AddStudent.aspx，進行類似的變更。</span><span class="sxs-lookup"><span data-stu-id="52676-154">In AddStudent.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

<span data-ttu-id="52676-155">在 Courses.aspx，進行類似的變更。</span><span class="sxs-lookup"><span data-stu-id="52676-155">In Courses.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

<span data-ttu-id="52676-156">執行應用程式，請注意，所有頁面函式，它們在先前。</span><span class="sxs-lookup"><span data-stu-id="52676-156">Run the application and notice that all of the pages function as they had previously.</span></span> <span data-ttu-id="52676-157">驗證邏輯也運作正常。</span><span class="sxs-lookup"><span data-stu-id="52676-157">The validation logic also works correctly.</span></span>

## <a name="conclusion"></a><span data-ttu-id="52676-158">結論</span><span class="sxs-lookup"><span data-stu-id="52676-158">Conclusion</span></span>

<span data-ttu-id="52676-159">在本教學課程中，重新建構您的應用程式使用資料存取層和商務邏輯層。</span><span class="sxs-lookup"><span data-stu-id="52676-159">In this tutorial, you re-structured your application to use a data access layer and business logic layer.</span></span> <span data-ttu-id="52676-160">您指定的資料控制項使用不是資料作業的目前頁面的物件。</span><span class="sxs-lookup"><span data-stu-id="52676-160">You specified that the data controls use an object that is not the current page for data operations.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="52676-161">上一步</span><span class="sxs-lookup"><span data-stu-id="52676-161">Previous</span></span>](using-query-string-values-to-retrieve-data.md)
