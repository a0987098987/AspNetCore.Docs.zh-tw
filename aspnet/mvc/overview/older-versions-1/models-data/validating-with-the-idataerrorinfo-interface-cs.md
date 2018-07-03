---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
title: 驗證與 IDataErrorInfo 介面 (C#) |Microsoft Docs
author: StephenWalther
description: Stephen Walther 會示範如何藉由在模型類別中實作的 IDataErrorInfo 介面中顯示自訂的驗證錯誤訊息。
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 4733b9f1-9999-48fb-8b73-6038fbcc5ecb
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: 0ed86c5467d6f55f83fa84144c374b3c63539174
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376587"
---
<a name="validating-with-the-idataerrorinfo-interface-c"></a><span data-ttu-id="694c9-103">驗證與 IDataErrorInfo 介面 (C#)</span><span class="sxs-lookup"><span data-stu-id="694c9-103">Validating with the IDataErrorInfo Interface (C#)</span></span>
====================
<span data-ttu-id="694c9-104">藉由[Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="694c9-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="694c9-105">Stephen Walther 會示範如何藉由在模型類別中實作的 IDataErrorInfo 介面中顯示自訂的驗證錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="694c9-105">Stephen Walther shows you how to display custom validation error messages by implementing the IDataErrorInfo interface in a model class.</span></span>


<span data-ttu-id="694c9-106">本教學課程的目標是說明其中一個方法來執行驗證的 ASP.NET MVC 應用程式中。</span><span class="sxs-lookup"><span data-stu-id="694c9-106">The goal of this tutorial is to explain one approach to performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="694c9-107">您了解如何防止有人提出 HTML 表單，而不提供所需的表單欄位的值。</span><span class="sxs-lookup"><span data-stu-id="694c9-107">You learn how to prevent someone from submitting an HTML form without providing values for required form fields.</span></span> <span data-ttu-id="694c9-108">在本教學課程中，您將了解如何使用 IErrorDataInfo 介面來執行驗證。</span><span class="sxs-lookup"><span data-stu-id="694c9-108">In this tutorial, you learn how to perform validation by using the IErrorDataInfo interface.</span></span>

## <a name="assumptions"></a><span data-ttu-id="694c9-109">假設</span><span class="sxs-lookup"><span data-stu-id="694c9-109">Assumptions</span></span>

<span data-ttu-id="694c9-110">在本教學課程中，我將使用 MoviesDB 資料庫及電影資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="694c9-110">In this tutorial, I'll use the MoviesDB database and the Movies database table.</span></span> <span data-ttu-id="694c9-111">此資料表具有下列資料行：</span><span class="sxs-lookup"><span data-stu-id="694c9-111">This table has the following columns:</span></span>

<a id="0.5_table01"></a>


| <span data-ttu-id="694c9-112">**資料行名稱**</span><span class="sxs-lookup"><span data-stu-id="694c9-112">**Column Name**</span></span> | <span data-ttu-id="694c9-113">**資料類型**</span><span class="sxs-lookup"><span data-stu-id="694c9-113">**Data Type**</span></span> | <span data-ttu-id="694c9-114">**允許 null 值**</span><span class="sxs-lookup"><span data-stu-id="694c9-114">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="694c9-115">ID</span><span class="sxs-lookup"><span data-stu-id="694c9-115">Id</span></span> | <span data-ttu-id="694c9-116">Int</span><span class="sxs-lookup"><span data-stu-id="694c9-116">Int</span></span> | <span data-ttu-id="694c9-117">False</span><span class="sxs-lookup"><span data-stu-id="694c9-117">False</span></span> |
| <span data-ttu-id="694c9-118">標題</span><span class="sxs-lookup"><span data-stu-id="694c9-118">Title</span></span> | <span data-ttu-id="694c9-119">Nvarchar(100)</span><span class="sxs-lookup"><span data-stu-id="694c9-119">Nvarchar(100)</span></span> | <span data-ttu-id="694c9-120">False</span><span class="sxs-lookup"><span data-stu-id="694c9-120">False</span></span> |
| <span data-ttu-id="694c9-121">總監</span><span class="sxs-lookup"><span data-stu-id="694c9-121">Director</span></span> | <span data-ttu-id="694c9-122">Nvarchar(100)</span><span class="sxs-lookup"><span data-stu-id="694c9-122">Nvarchar(100)</span></span> | <span data-ttu-id="694c9-123">False</span><span class="sxs-lookup"><span data-stu-id="694c9-123">False</span></span> |
| <span data-ttu-id="694c9-124">DateReleased</span><span class="sxs-lookup"><span data-stu-id="694c9-124">DateReleased</span></span> | <span data-ttu-id="694c9-125">DateTime</span><span class="sxs-lookup"><span data-stu-id="694c9-125">DateTime</span></span> | <span data-ttu-id="694c9-126">False</span><span class="sxs-lookup"><span data-stu-id="694c9-126">False</span></span> |


<span data-ttu-id="694c9-127">在本教學課程中，我可以使用 Microsoft Entity Framework 產生我的資料庫模型類別。</span><span class="sxs-lookup"><span data-stu-id="694c9-127">In this tutorial, I use the Microsoft Entity Framework to generate my database model classes.</span></span> <span data-ttu-id="694c9-128">Entity Framework 所產生的電影類別會顯示在 圖 1。</span><span class="sxs-lookup"><span data-stu-id="694c9-128">The Movie class generated by the Entity Framework is displayed in Figure 1.</span></span>


<span data-ttu-id="694c9-129">[![電影實體](validating-with-the-idataerrorinfo-interface-cs/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="694c9-129">[![The Movie entity](validating-with-the-idataerrorinfo-interface-cs/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image1.png)</span></span>

<span data-ttu-id="694c9-130">**圖 01**: 電影實體 ([按一下以檢視完整大小的影像](validating-with-the-idataerrorinfo-interface-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="694c9-130">**Figure 01**: The Movie entity([Click to view full-size image](validating-with-the-idataerrorinfo-interface-cs/_static/image2.png))</span></span>


> [!NOTE] 
> 
> <span data-ttu-id="694c9-131">若要深入瞭解如何使用 Entity Framework 來產生您的資料庫模型類別，請參閱我的教學課程中，標題為 「 使用 Entity Framework 建立模型類別。</span><span class="sxs-lookup"><span data-stu-id="694c9-131">To learn more about using the Entity Framework to generate your database model classes, see my tutorial entitled Creating Model Classes with the Entity Framework.</span></span>


## <a name="the-controller-class"></a><span data-ttu-id="694c9-132">控制器類別</span><span class="sxs-lookup"><span data-stu-id="694c9-132">The Controller Class</span></span>

<span data-ttu-id="694c9-133">我們會使用清單電影主控制器，並建立新的電影。</span><span class="sxs-lookup"><span data-stu-id="694c9-133">We use the Home controller to list movies and create new movies.</span></span> <span data-ttu-id="694c9-134">此類別的程式碼會包含在程式碼範例 1。</span><span class="sxs-lookup"><span data-stu-id="694c9-134">The code for this class is contained in Listing 1.</span></span>

<span data-ttu-id="694c9-135">**列表 1-Controllers\HomeController.cs**</span><span class="sxs-lookup"><span data-stu-id="694c9-135">**Listing 1 - Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample1.cs)]

<span data-ttu-id="694c9-136">列表 1 中的首頁控制器類別包含兩個 create （） 動作。</span><span class="sxs-lookup"><span data-stu-id="694c9-136">The Home controller class in Listing 1 contains two Create() actions.</span></span> <span data-ttu-id="694c9-137">第一個動作顯示的 HTML 表單建立新的電影。</span><span class="sxs-lookup"><span data-stu-id="694c9-137">The first action displays the HTML form for creating a new movie.</span></span> <span data-ttu-id="694c9-138">第二個 create （） 動作會執行新電影的實際插入至資料庫。</span><span class="sxs-lookup"><span data-stu-id="694c9-138">The second Create() action performs the actual insert of the new movie into the database.</span></span> <span data-ttu-id="694c9-139">第一個 create （） 動作所顯示的表單提交到伺服器時，會叫用第二個 create （） 動作。</span><span class="sxs-lookup"><span data-stu-id="694c9-139">The second Create() action is invoked when the form displayed by the first Create() action is submitted to the server.</span></span>

<span data-ttu-id="694c9-140">請注意第二個 create （） 動作，包含下列程式碼行：</span><span class="sxs-lookup"><span data-stu-id="694c9-140">Notice that the second Create() action contains the following lines of code:</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample2.cs)]

<span data-ttu-id="694c9-141">驗證錯誤時，則 IsValid 屬性會傳回 false。</span><span class="sxs-lookup"><span data-stu-id="694c9-141">The IsValid property returns false when there is a validation error.</span></span> <span data-ttu-id="694c9-142">在此情況下，會重新顯示建立檢視，其中包含用於建立電影的 HTML 表單。</span><span class="sxs-lookup"><span data-stu-id="694c9-142">In that case, the Create view that contains the HTML form for creating a movie is redisplayed.</span></span>

## <a name="creating-a-partial-class"></a><span data-ttu-id="694c9-143">建立部分類別</span><span class="sxs-lookup"><span data-stu-id="694c9-143">Creating a Partial Class</span></span>

<span data-ttu-id="694c9-144">影片類別是由 Entity Framework 產生的。</span><span class="sxs-lookup"><span data-stu-id="694c9-144">The Movie class is generated by the Entity Framework.</span></span> <span data-ttu-id="694c9-145">如果您展開 MoviesDBModel.edmx 檔案，在 方案總管 視窗中的，並在程式碼編輯器中開啟 MoviesDBModel.Designer.cs 檔案，您可以看到電影類別的程式碼 （請參閱 圖 2）。</span><span class="sxs-lookup"><span data-stu-id="694c9-145">You can see the code for the Movie class if you expand the MoviesDBModel.edmx file in the Solution Explorer window and open the MoviesDBModel.Designer.cs file in the Code Editor (see Figure 2).</span></span>


<span data-ttu-id="694c9-146">[![電影實體的程式碼](validating-with-the-idataerrorinfo-interface-cs/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="694c9-146">[![The code for the Movie entity](validating-with-the-idataerrorinfo-interface-cs/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image3.png)</span></span>

<span data-ttu-id="694c9-147">**圖 02**： 電影實體的程式碼 ([按一下以檢視完整大小的影像](validating-with-the-idataerrorinfo-interface-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="694c9-147">**Figure 02**: The code for the Movie entity([Click to view full-size image](validating-with-the-idataerrorinfo-interface-cs/_static/image4.png))</span></span>


<span data-ttu-id="694c9-148">影片類別是部分類別。</span><span class="sxs-lookup"><span data-stu-id="694c9-148">The Movie class is a partial class.</span></span> <span data-ttu-id="694c9-149">這表示我們可以加入至擴充功能的電影類別同名的其他部分類別。</span><span class="sxs-lookup"><span data-stu-id="694c9-149">That means that we can add another partial class with the same name to extend the functionality of the Movie class.</span></span> <span data-ttu-id="694c9-150">我們會將我們的驗證邏輯加入新的部分類別。</span><span class="sxs-lookup"><span data-stu-id="694c9-150">We'll add our validation logic to the new partial class.</span></span>

<span data-ttu-id="694c9-151">在 列表 2 中新增的類別，到 Models 資料夾。</span><span class="sxs-lookup"><span data-stu-id="694c9-151">Add the class in Listing 2 to the Models folder.</span></span>

<span data-ttu-id="694c9-152">**列表 2-Models\Movie.cs**</span><span class="sxs-lookup"><span data-stu-id="694c9-152">**Listing 2 - Models\Movie.cs**</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample3.cs)]

<span data-ttu-id="694c9-153">請注意，在 列表 2 中的類別包含*部分*修飾詞。</span><span class="sxs-lookup"><span data-stu-id="694c9-153">Notice that the class in Listing 2 includes the *partial* modifier.</span></span> <span data-ttu-id="694c9-154">任何方法或屬性，您將加入這個類別會成為 Entity Framework 所產生之電影類別的一部分。</span><span class="sxs-lookup"><span data-stu-id="694c9-154">Any methods or properties that you add to this class become part of the Movie class generated by the Entity Framework.</span></span>

## <a name="adding-onchanging-and-onchanged-partial-methods"></a><span data-ttu-id="694c9-155">新增 OnChanging 和 OnChanged 部分方法</span><span class="sxs-lookup"><span data-stu-id="694c9-155">Adding OnChanging and OnChanged Partial Methods</span></span>

<span data-ttu-id="694c9-156">當 Entity Framework 產生的實體類別時，Entity Framework 部分方法類別會自動新增。</span><span class="sxs-lookup"><span data-stu-id="694c9-156">When the Entity Framework generates an entity class, the Entity Framework adds partial methods to the class automatically.</span></span> <span data-ttu-id="694c9-157">Entity Framework 會產生 OnChanging 和 OnChanged 對應類別的每一個屬性的部分方法。</span><span class="sxs-lookup"><span data-stu-id="694c9-157">The Entity Framework generates OnChanging and OnChanged partial methods that correspond to each property of the class.</span></span>

<span data-ttu-id="694c9-158">如果影片類別是 Entity Framework 會建立下列方法：</span><span class="sxs-lookup"><span data-stu-id="694c9-158">In the case of the Movie class, the Entity Framework creates the following methods:</span></span>

- <span data-ttu-id="694c9-159">OnIdChanging</span><span class="sxs-lookup"><span data-stu-id="694c9-159">OnIdChanging</span></span>
- <span data-ttu-id="694c9-160">OnIdChanged</span><span class="sxs-lookup"><span data-stu-id="694c9-160">OnIdChanged</span></span>
- <span data-ttu-id="694c9-161">OnTitleChanging</span><span class="sxs-lookup"><span data-stu-id="694c9-161">OnTitleChanging</span></span>
- <span data-ttu-id="694c9-162">OnTitleChanged</span><span class="sxs-lookup"><span data-stu-id="694c9-162">OnTitleChanged</span></span>
- <span data-ttu-id="694c9-163">OnDirectorChanging</span><span class="sxs-lookup"><span data-stu-id="694c9-163">OnDirectorChanging</span></span>
- <span data-ttu-id="694c9-164">OnDirectorChanged</span><span class="sxs-lookup"><span data-stu-id="694c9-164">OnDirectorChanged</span></span>
- <span data-ttu-id="694c9-165">OnDateReleasedChanging</span><span class="sxs-lookup"><span data-stu-id="694c9-165">OnDateReleasedChanging</span></span>
- <span data-ttu-id="694c9-166">OnDateReleasedChanged</span><span class="sxs-lookup"><span data-stu-id="694c9-166">OnDateReleasedChanged</span></span>

<span data-ttu-id="694c9-167">OnChanging 方法稱為權限，才會變更對應的屬性。</span><span class="sxs-lookup"><span data-stu-id="694c9-167">The OnChanging method is called right before the corresponding property is changed.</span></span> <span data-ttu-id="694c9-168">變更屬性之後，稱為右 OnChanged 方法。</span><span class="sxs-lookup"><span data-stu-id="694c9-168">The OnChanged method is called right after the property is changed.</span></span>

<span data-ttu-id="694c9-169">您可以利用這些部分方法，將驗證邏輯新增至電影類別。</span><span class="sxs-lookup"><span data-stu-id="694c9-169">You can take advantage of these partial methods to add validation logic to the Movie class.</span></span> <span data-ttu-id="694c9-170">更新列表 3 中的電影類別會確認 標題 和 導演屬性，會指派非空值。</span><span class="sxs-lookup"><span data-stu-id="694c9-170">The update Movie class in Listing 3 verifies that the Title and Director properties are assigned nonempty values.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="694c9-171">部分方法是在您不需要實作類別中定義的方法。</span><span class="sxs-lookup"><span data-stu-id="694c9-171">A partial method is a method defined in a class that you are not required to implement.</span></span> <span data-ttu-id="694c9-172">如果您未實作部分方法，編譯器會移除方法簽章，並因此方法的所有呼叫都都沒有與部分方法相關聯的執行階段成本。</span><span class="sxs-lookup"><span data-stu-id="694c9-172">If you don't implement a partial method then the compiler removes the method signature and all calls to the method so there are no run-time costs associated with the partial method.</span></span> <span data-ttu-id="694c9-173">在 Visual Studio 程式碼編輯器 」 中，您可以藉由輸入關鍵字加入部分方法*部分*後面空間，以檢視來實作的部分清單。</span><span class="sxs-lookup"><span data-stu-id="694c9-173">In the Visual Studio Code Editor, you can add a partial method by typing the keyword *partial* followed by a space to view a list of partials to implement.</span></span>


<span data-ttu-id="694c9-174">**列表 3-Models\Movie.cs**</span><span class="sxs-lookup"><span data-stu-id="694c9-174">**Listing 3 - Models\Movie.cs**</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample4.cs)]

<span data-ttu-id="694c9-175">比方說，如果您嘗試將空字串指派給 Title 屬性，則錯誤訊息指派給名為字典\_錯誤。</span><span class="sxs-lookup"><span data-stu-id="694c9-175">For example, if you attempt to assign an empty string to the Title property, then an error message is assigned to a Dictionary named \_errors.</span></span>

<span data-ttu-id="694c9-176">此時，實際上沒有任何動靜當您將空字串指派給 Title 屬性，並將錯誤加入至私用\_錯誤欄位。</span><span class="sxs-lookup"><span data-stu-id="694c9-176">At this point, nothing actually happens when you assign an empty string to the Title property and an error is added to the private \_errors field.</span></span> <span data-ttu-id="694c9-177">我們必須實作 IDataErrorInfo 介面，公開 （expose） 這些的驗證錯誤，以 ASP.NET MVC 架構。</span><span class="sxs-lookup"><span data-stu-id="694c9-177">We need to implement the IDataErrorInfo interface to expose these validation errors to the ASP.NET MVC framework.</span></span>

## <a name="implementing-the-idataerrorinfo-interface"></a><span data-ttu-id="694c9-178">實作 IDataErrorInfo 介面</span><span class="sxs-lookup"><span data-stu-id="694c9-178">Implementing the IDataErrorInfo Interface</span></span>

<span data-ttu-id="694c9-179">IDataErrorInfo 介面已自第一版.NET framework 的一部分。</span><span class="sxs-lookup"><span data-stu-id="694c9-179">The IDataErrorInfo interface has been part of the .NET framework since the first version.</span></span> <span data-ttu-id="694c9-180">這個介面是一個非常簡單的介面：</span><span class="sxs-lookup"><span data-stu-id="694c9-180">This interface is a very simple interface:</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample5.cs)]

<span data-ttu-id="694c9-181">如果類別實作的 IDataErrorInfo 介面，ASP.NET MVC 架構會使用此介面，建立類別的執行個體時。</span><span class="sxs-lookup"><span data-stu-id="694c9-181">If a class implements the IDataErrorInfo interface, the ASP.NET MVC framework will use this interface when creating an instance of the class.</span></span> <span data-ttu-id="694c9-182">例如，主控制器 create （） 動作會接受影片類別的執行個體：</span><span class="sxs-lookup"><span data-stu-id="694c9-182">For example, the Home controller Create() action accepts an instance of the Movie class:</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample6.cs)]

<span data-ttu-id="694c9-183">ASP.NET MVC 架構會建立使用模型繫結 (DefaultModelBinder) 傳遞給 create （） 動作電影的執行個體。</span><span class="sxs-lookup"><span data-stu-id="694c9-183">The ASP.NET MVC framework creates the instance of the Movie passed to the Create() action by using a model binder (the DefaultModelBinder).</span></span> <span data-ttu-id="694c9-184">模型繫結器會負責建立電影物件的執行個體繫結至電影物件的執行個體的 HTML 表單欄位。</span><span class="sxs-lookup"><span data-stu-id="694c9-184">The model binder is responsible for creating an instance of the Movie object by binding the HTML form fields to an instance of the Movie object.</span></span>

<span data-ttu-id="694c9-185">DefaultModelBinder 會偵測類別實作的 IDataErrorInfo 介面。</span><span class="sxs-lookup"><span data-stu-id="694c9-185">The DefaultModelBinder detects whether or not a class implements the IDataErrorInfo interface.</span></span> <span data-ttu-id="694c9-186">如果類別實作此介面的模型繫結器會叫用類別的每一個屬性的 IDataErrorInfo.this 索引子。</span><span class="sxs-lookup"><span data-stu-id="694c9-186">If a class implements this interface then the model binder invokes the IDataErrorInfo.this indexer for each property of the class.</span></span> <span data-ttu-id="694c9-187">如果索引子會傳回錯誤訊息的模型繫結器就會加入此錯誤訊息，來自動建立模型的狀態。</span><span class="sxs-lookup"><span data-stu-id="694c9-187">If the indexer returns an error message then the model binder adds this error message to model state automatically.</span></span>

<span data-ttu-id="694c9-188">DefaultModelBinder 也會檢查 IDataErrorInfo.Error 屬性。</span><span class="sxs-lookup"><span data-stu-id="694c9-188">The DefaultModelBinder also checks the IDataErrorInfo.Error property.</span></span> <span data-ttu-id="694c9-189">這個屬性被要代表與類別相關聯的非屬性特定的驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="694c9-189">This property is intended to represent non-property specific validation errors associated with the class.</span></span> <span data-ttu-id="694c9-190">例如，您可能要強制套用驗證規則，取決於影片類別的多個屬性的值。</span><span class="sxs-lookup"><span data-stu-id="694c9-190">For example, you might want to enforce a validation rule that depends on the values of multiple properties of the Movie class.</span></span> <span data-ttu-id="694c9-191">在此情況下，您也會從 [錯誤] 屬性中傳回驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="694c9-191">In that case, you would return a validation error from the Error property.</span></span>

<span data-ttu-id="694c9-192">在 列表 4 中更新的電影類別會實作 IDataErrorInfo 介面。</span><span class="sxs-lookup"><span data-stu-id="694c9-192">The updated Movie class in Listing 4 implements the IDataErrorInfo interface.</span></span>

<span data-ttu-id="694c9-193">**列表 4-Models\Movie.cs （實作 IDataErrorInfo）**</span><span class="sxs-lookup"><span data-stu-id="694c9-193">**Listing 4 - Models\Movie.cs (implements IDataErrorInfo)**</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample7.cs)]

<span data-ttu-id="694c9-194">在 [列表 4] 的索引子屬性會檢查\_錯誤集合，以查看它是否包含索引鍵對應至屬性名稱傳遞給索引子。</span><span class="sxs-lookup"><span data-stu-id="694c9-194">In Listing 4, the indexer property checks the \_errors collection to see if it contains a key that corresponds to the property name passed to the indexer.</span></span> <span data-ttu-id="694c9-195">如果不沒有與屬性有關聯任何驗證錯誤則會傳回空字串。</span><span class="sxs-lookup"><span data-stu-id="694c9-195">If there is no validation error associated with the property then an empty string is returned.</span></span>

<span data-ttu-id="694c9-196">您不需要修改 Home 控制器中使用修改過的電影類別的任何方法。</span><span class="sxs-lookup"><span data-stu-id="694c9-196">You don't need to modify the Home controller in any way to use the modified Movie class.</span></span> <span data-ttu-id="694c9-197">圖 3 中所顯示的網頁說明的標題或主管的表單欄位不輸入任何值時，會發生什麼事。</span><span class="sxs-lookup"><span data-stu-id="694c9-197">The page displayed in Figure 3 illustrates what happens when no value is entered for the Title or Director form fields.</span></span>


<span data-ttu-id="694c9-198">[![自動建立動作方法](validating-with-the-idataerrorinfo-interface-cs/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="694c9-198">[![Creating action methods automatically](validating-with-the-idataerrorinfo-interface-cs/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image5.png)</span></span>

<span data-ttu-id="694c9-199">**圖 03**： 有遺漏值的表單 ([按一下以檢視完整大小的影像](validating-with-the-idataerrorinfo-interface-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="694c9-199">**Figure 03**: A form with missing values ([Click to view full-size image](validating-with-the-idataerrorinfo-interface-cs/_static/image6.png))</span></span>


<span data-ttu-id="694c9-200">請注意 DateReleased 值會自動驗證。</span><span class="sxs-lookup"><span data-stu-id="694c9-200">Notice that the DateReleased value is validated automatically.</span></span> <span data-ttu-id="694c9-201">DateReleased 屬性不接受 NULL 值，所以 DefaultModelBinder 驗證錯誤，這個屬性會自動產生它沒有值時。</span><span class="sxs-lookup"><span data-stu-id="694c9-201">Because the DateReleased property does not accept NULL values, the DefaultModelBinder generates a validation error for this property automatically when it does not have a value.</span></span> <span data-ttu-id="694c9-202">如果您想要修改 DateReleased 屬性的錯誤訊息，則您需要建立自訂模型繫結。</span><span class="sxs-lookup"><span data-stu-id="694c9-202">If you want to modify the error message for the DateReleased property then you need to create a custom model binder.</span></span>

## <a name="summary"></a><span data-ttu-id="694c9-203">總結</span><span class="sxs-lookup"><span data-stu-id="694c9-203">Summary</span></span>

<span data-ttu-id="694c9-204">在本教學課程中，您已了解如何使用 IDataErrorInfo 介面來產生驗證錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="694c9-204">In this tutorial, you learned how to use the IDataErrorInfo interface to generate validation error messages.</span></span> <span data-ttu-id="694c9-205">首先，我們會建立擴充功能的 Entity Framework 所產生的部分影片類別的部分影片類別。</span><span class="sxs-lookup"><span data-stu-id="694c9-205">First, we created a partial Movie class that extends the functionality of the partial Movie class generated by the Entity Framework.</span></span> <span data-ttu-id="694c9-206">接下來，我們加入驗證邏輯的電影類別 OnTitleChanging() 和 OnDirectorChanging() 部分方法。</span><span class="sxs-lookup"><span data-stu-id="694c9-206">Next, we added validation logic to the Movie class OnTitleChanging() and OnDirectorChanging() partial methods.</span></span> <span data-ttu-id="694c9-207">最後，我們實作 IDataErrorInfo 介面，以便公開 （expose） 這些 ASP.NET MVC framework 的驗證訊息。</span><span class="sxs-lookup"><span data-stu-id="694c9-207">Finally, we implemented the IDataErrorInfo interface in order to expose these validation messages to the ASP.NET MVC framework.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="694c9-208">[上一頁](performing-simple-validation-cs.md)
> [下一頁](validating-with-a-service-layer-cs.md)</span><span class="sxs-lookup"><span data-stu-id="694c9-208">[Previous](performing-simple-validation-cs.md)
[Next](validating-with-a-service-layer-cs.md)</span></span>
