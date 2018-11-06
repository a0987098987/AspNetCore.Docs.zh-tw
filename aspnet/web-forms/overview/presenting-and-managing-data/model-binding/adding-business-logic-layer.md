---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: 新增商業邏輯層級使用模型繫結和 web forms 的專案 |Microsoft Docs
author: Rick-Anderson
description: 本系列教學課程示範使用 ASP.NET Web Form 專案中的模型繫結的基本層面。 模型繫結進行資料互動更多簡單-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: 4ba1830ce51f257e18880f752b249dbeb77f504e
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021647"
---
<a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a><span data-ttu-id="92396-104">新增商業邏輯層級使用模型繫結和 web forms 的專案</span><span class="sxs-lookup"><span data-stu-id="92396-104">Adding business logic layer to a project that uses model binding and web forms</span></span>
====================
<span data-ttu-id="92396-105">藉由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="92396-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="92396-106">本系列教學課程示範使用 ASP.NET Web Form 專案中的模型繫結的基本層面。</span><span class="sxs-lookup"><span data-stu-id="92396-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="92396-107">模型繫結進行更簡單的比處理資料來源物件 （例如 ObjectDataSource 或 SqlDataSource） 的資料互動。</span><span class="sxs-lookup"><span data-stu-id="92396-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="92396-108">此系列簡介資料的開頭，並在稍後的教學課程中將移至更進階的概念。</span><span class="sxs-lookup"><span data-stu-id="92396-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="92396-109">本教學課程會示範如何使用商務邏輯層的模型繫結。</span><span class="sxs-lookup"><span data-stu-id="92396-109">This tutorial shows how to use model binding with a business logic layer.</span></span> <span data-ttu-id="92396-110">您將設定 OnCallingDataMethods 成員指定目前頁面以外的物件用來呼叫資料方法。</span><span class="sxs-lookup"><span data-stu-id="92396-110">You will set the OnCallingDataMethods member to specify that an object other than the current page is used to call the data methods.</span></span>
> 
> <span data-ttu-id="92396-111">本教學課程中建立的專案是根據[早](retrieving-data.md)系列的一部分。</span><span class="sxs-lookup"><span data-stu-id="92396-111">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="92396-112">您可以[下載](https://go.microsoft.com/fwlink/?LinkId=286116)完整的專案，以 C# 或 vb。</span><span class="sxs-lookup"><span data-stu-id="92396-112">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="92396-113">可下載的程式碼適用於 Visual Studio 2012 或 Visual Studio 2013。</span><span class="sxs-lookup"><span data-stu-id="92396-113">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="92396-114">它會使用 Visual Studio 2012 範本，也就是本教學課程中的 Visual Studio 2013 範本稍有不同。</span><span class="sxs-lookup"><span data-stu-id="92396-114">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="92396-115">您將建置</span><span class="sxs-lookup"><span data-stu-id="92396-115">What you'll build</span></span>

<span data-ttu-id="92396-116">模型繫結可讓您將資料互動的程式碼，可能是程式碼後置檔案中的網頁或個別的商務邏輯類別中。</span><span class="sxs-lookup"><span data-stu-id="92396-116">Model binding enables you to put your data interaction code in either the code-behind file for a web page or in a separate business logic class.</span></span> <span data-ttu-id="92396-117">先前的教學課程已示範如何使用資料互動的程式碼的程式碼後置檔案。</span><span class="sxs-lookup"><span data-stu-id="92396-117">The previous tutorials have shown how to use the code-behind files for data interaction code.</span></span> <span data-ttu-id="92396-118">這種方法適用於小型站台，但它可能會導致程式碼重複和更不容易維護的大型網站時。</span><span class="sxs-lookup"><span data-stu-id="92396-118">This approach works for small sites but it can lead to code repetition and greater difficulty when maintaining a large site.</span></span> <span data-ttu-id="92396-119">它也可以是很難進行程式設計的方式測試程式碼位於程式碼後置檔案，因為沒有任何抽象層。</span><span class="sxs-lookup"><span data-stu-id="92396-119">It can also be very difficult to programmatically test code that resides in code behind files because there is no abstraction layer.</span></span>

<span data-ttu-id="92396-120">集中管理的資料互動的程式碼，您可以建立商業邏輯層，其中包含所有的邏輯與資料互動。</span><span class="sxs-lookup"><span data-stu-id="92396-120">To centralize the data interaction code, you can create a business logic layer that contains all of the logic for interacting with data.</span></span> <span data-ttu-id="92396-121">然後，您會呼叫商務邏輯層從您的網頁。</span><span class="sxs-lookup"><span data-stu-id="92396-121">You then call the business logic layer from your web pages.</span></span> <span data-ttu-id="92396-122">本教學課程會示範如何移動所有的商務邏輯層，到先前的教學課程中，您已撰寫的程式碼，然後使用該頁面的程式碼。</span><span class="sxs-lookup"><span data-stu-id="92396-122">This tutorial shows how to move all of the code you have written in the previous tutorials into a business logic layer, and then use that code from the pages.</span></span>

<span data-ttu-id="92396-123">在本教學課程中，您將會：</span><span class="sxs-lookup"><span data-stu-id="92396-123">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="92396-124">從程式碼後置檔案的程式碼移到商業邏輯層</span><span class="sxs-lookup"><span data-stu-id="92396-124">Move the code from code-behind files to a business logic layer</span></span>
2. <span data-ttu-id="92396-125">變更您的資料繫結控制項，以呼叫商務邏輯層中的方法</span><span class="sxs-lookup"><span data-stu-id="92396-125">Change your data bound controls to call the methods in the business logic layer</span></span>

## <a name="create-business-logic-layer"></a><span data-ttu-id="92396-126">建立商業邏輯層</span><span class="sxs-lookup"><span data-stu-id="92396-126">Create business logic layer</span></span>

<span data-ttu-id="92396-127">現在，您將建立會從網頁呼叫的類別。</span><span class="sxs-lookup"><span data-stu-id="92396-127">Now, you will create the class that is called from the web pages.</span></span> <span data-ttu-id="92396-128">此類別中的方法看起來類似您在先前的教學課程中使用的方法，並包含值提供者屬性。</span><span class="sxs-lookup"><span data-stu-id="92396-128">The methods in this class look similar to the methods you used in the previous tutorials and include the value provider attributes.</span></span>

<span data-ttu-id="92396-129">首先，新增 新資料夾，稱為**BLL**。</span><span class="sxs-lookup"><span data-stu-id="92396-129">First, add a new folder called **BLL**.</span></span>

![新增資料夾](adding-business-logic-layer/_static/image1.png)

<span data-ttu-id="92396-131">在 [BLL] 資料夾中，建立新類別，名為**SchoolBL.cs**。</span><span class="sxs-lookup"><span data-stu-id="92396-131">In the BLL folder, create a new class named **SchoolBL.cs**.</span></span> <span data-ttu-id="92396-132">它會包含所有最初在程式碼後置檔案中的資料作業。</span><span class="sxs-lookup"><span data-stu-id="92396-132">It will contain all of the data operations that originally resided in code-behind files.</span></span> <span data-ttu-id="92396-133">方法的方法，在程式碼後置檔案中，幾乎相同，但會包括一些變更。</span><span class="sxs-lookup"><span data-stu-id="92396-133">The methods are almost the same as the methods in the code-behind file, but will include some changes.</span></span>

<span data-ttu-id="92396-134">要注意的最重要變更是，您無法再執行中的程式碼執行個體內**網頁**類別。</span><span class="sxs-lookup"><span data-stu-id="92396-134">The most important change to note is that you are no longer executing the code from within an instance of **Page** class.</span></span> <span data-ttu-id="92396-135">頁面類別包含**TryUpdateModel**方法並**ModelState**屬性。</span><span class="sxs-lookup"><span data-stu-id="92396-135">The Page class contains the **TryUpdateModel** method and the **ModelState** property.</span></span> <span data-ttu-id="92396-136">此程式碼移到商業邏輯層，當您不再需要呼叫這些成員的頁面類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="92396-136">When this code is moved to a business logic layer, you no longer have an instance of the Page class to call these members.</span></span> <span data-ttu-id="92396-137">若要解決此問題，您必須新增**ModelMethodContext**存取 TryUpdateModel 或 ModelState 任何方法的參數。</span><span class="sxs-lookup"><span data-stu-id="92396-137">To get around this issue, you must add a **ModelMethodContext** parameter to any method that accesses TryUpdateModel or ModelState.</span></span> <span data-ttu-id="92396-138">您可以使用這個 ModelMethodContext 參數呼叫 TryUpdateModel 或擷取 ModelState。</span><span class="sxs-lookup"><span data-stu-id="92396-138">You use this ModelMethodContext parameter to call TryUpdateModel or retrieve ModelState.</span></span> <span data-ttu-id="92396-139">您不需要變更網頁以說明這個新的參數中的任何項目。</span><span class="sxs-lookup"><span data-stu-id="92396-139">You do not need to change anything in the web page to account for this new parameter.</span></span>

<span data-ttu-id="92396-140">SchoolBL.cs 中的程式碼取代為下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="92396-140">Replace the code in SchoolBL.cs with the following code.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a><span data-ttu-id="92396-141">修改現有的頁面，來擷取商業邏輯層中的資料</span><span class="sxs-lookup"><span data-stu-id="92396-141">Revise existing pages to retrieve data from business logic layer</span></span>

<span data-ttu-id="92396-142">最後，您會從使用商務邏輯層的程式碼後置檔案中使用查詢來轉換 Students.aspx、 AddStudent.aspx 和 Courses.aspx 的頁面。</span><span class="sxs-lookup"><span data-stu-id="92396-142">Finally, you will convert the pages Students.aspx, AddStudent.aspx, and Courses.aspx from using queries in the code-behind file to using the business logic layer.</span></span>

<span data-ttu-id="92396-143">在程式碼後置檔案學生、 AddStudent，及課程中，刪除或註解化下列的查詢方法：</span><span class="sxs-lookup"><span data-stu-id="92396-143">In the code-behind files for Students, AddStudent, and Courses, delete or comment out the following query methods:</span></span>

- <span data-ttu-id="92396-144">studentsGrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="92396-144">studentsGrid\_GetData</span></span>
- <span data-ttu-id="92396-145">studentsGrid\_UpdateItem</span><span class="sxs-lookup"><span data-stu-id="92396-145">studentsGrid\_UpdateItem</span></span>
- <span data-ttu-id="92396-146">studentsGrid\_DeleteItem</span><span class="sxs-lookup"><span data-stu-id="92396-146">studentsGrid\_DeleteItem</span></span>
- <span data-ttu-id="92396-147">addStudentForm\_InsertItem</span><span class="sxs-lookup"><span data-stu-id="92396-147">addStudentForm\_InsertItem</span></span>
- <span data-ttu-id="92396-148">coursesGrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="92396-148">coursesGrid\_GetData</span></span>

<span data-ttu-id="92396-149">您現在應該有程式碼不屬於資料作業的程式碼後置檔案中。</span><span class="sxs-lookup"><span data-stu-id="92396-149">You now should have no code in the code-behind file that pertains to data operations.</span></span>

<span data-ttu-id="92396-150">**OnCallingDataMethods**事件處理常式可讓您指定要用於資料方法的物件。</span><span class="sxs-lookup"><span data-stu-id="92396-150">The **OnCallingDataMethods** event handler enables you to specify an object to use for the data methods.</span></span> <span data-ttu-id="92396-151">Students.aspx，在加入該事件處理常式的值並將變更之資料方法的名稱之商務邏輯類別中方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="92396-151">In Students.aspx, add a value for that event handler and change the names of the data methods to the names of the methods in the business logic class.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

<span data-ttu-id="92396-152">在 Students.aspx 的程式碼後置檔案，定義 CallingDataMethods 事件的事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="92396-152">In the code-behind file for Students.aspx, define the event handler for the CallingDataMethods event.</span></span> <span data-ttu-id="92396-153">在這個事件處理常式中，您可以指定資料作業的商務邏輯類別。</span><span class="sxs-lookup"><span data-stu-id="92396-153">In this event handler, you specify the business logic class for data operations.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

<span data-ttu-id="92396-154">在 AddStudent.aspx，進行類似變更。</span><span class="sxs-lookup"><span data-stu-id="92396-154">In AddStudent.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

<span data-ttu-id="92396-155">在 Courses.aspx，進行類似變更。</span><span class="sxs-lookup"><span data-stu-id="92396-155">In Courses.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

<span data-ttu-id="92396-156">執行應用程式，並請注意，所有的頁面做為它們之前已擁有。</span><span class="sxs-lookup"><span data-stu-id="92396-156">Run the application and notice that all of the pages function as they had previously.</span></span> <span data-ttu-id="92396-157">驗證邏輯能正確運作。</span><span class="sxs-lookup"><span data-stu-id="92396-157">The validation logic also works correctly.</span></span>

## <a name="conclusion"></a><span data-ttu-id="92396-158">結論</span><span class="sxs-lookup"><span data-stu-id="92396-158">Conclusion</span></span>

<span data-ttu-id="92396-159">在本教學課程中，重新建構您的應用程式使用的資料存取層和商務邏輯層。</span><span class="sxs-lookup"><span data-stu-id="92396-159">In this tutorial, you re-structured your application to use a data access layer and business logic layer.</span></span> <span data-ttu-id="92396-160">您指定資料控制項使用不是資料作業的目前頁面的物件。</span><span class="sxs-lookup"><span data-stu-id="92396-160">You specified that the data controls use an object that is not the current page for data operations.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="92396-161">上一步</span><span class="sxs-lookup"><span data-stu-id="92396-161">Previous</span></span>](using-query-string-values-to-retrieve-data.md)
