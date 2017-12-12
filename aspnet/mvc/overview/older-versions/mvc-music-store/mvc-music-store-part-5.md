---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
title: "第 5 部分： 編輯表單和範本 |Microsoft 文件"
author: jongalloway
description: "此教學課程系列詳細列出所有建置 ASP.NET MVC 商店範例應用程式所採取的步驟。 第 5 部分涵蓋編輯表單和範本。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 6b09413a-6d6a-425a-87c9-629f91b91b28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
msc.type: authoredcontent
ms.openlocfilehash: cde6fe133291254531a797a434a4b2cdd226dd5f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="part-5-edit-forms-and-templating"></a><span data-ttu-id="31a3b-104">第 5 部分： 編輯表單和範本</span><span class="sxs-lookup"><span data-stu-id="31a3b-104">Part 5: Edit Forms and Templating</span></span>
====================
<span data-ttu-id="31a3b-105">由[Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="31a3b-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="31a3b-106">MVC Music Store 是介紹並逐步說明如何使用 ASP.NET MVC 和 Visual Studio 針對 web 程式開發的教學課程應用程式。</span><span class="sxs-lookup"><span data-stu-id="31a3b-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="31a3b-107">MVC Music Store 是輕量級範例存放區實作銷售音樂專輯上線，並實作基本的網站管理、 使用者登入和購物車的功能。</span><span class="sxs-lookup"><span data-stu-id="31a3b-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>
> 
> <span data-ttu-id="31a3b-108">此教學課程系列詳細列出所有建置 ASP.NET MVC 商店範例應用程式所採取的步驟。</span><span class="sxs-lookup"><span data-stu-id="31a3b-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="31a3b-109">第 5 部分涵蓋編輯表單和範本。</span><span class="sxs-lookup"><span data-stu-id="31a3b-109">Part 5 covers Edit Forms and Templating.</span></span>


<span data-ttu-id="31a3b-110">在過去的章節中，我們已從資料庫載入資料，並顯示它。</span><span class="sxs-lookup"><span data-stu-id="31a3b-110">In the past chapter, we were loading data from our database and displaying it.</span></span> <span data-ttu-id="31a3b-111">在本章中，我們也會啟用編輯的資料。</span><span class="sxs-lookup"><span data-stu-id="31a3b-111">In this chapter, we'll also enable editing the data.</span></span>

## <a name="creating-the-storemanagercontroller"></a><span data-ttu-id="31a3b-112">建立 StoreManagerController</span><span class="sxs-lookup"><span data-stu-id="31a3b-112">Creating the StoreManagerController</span></span>

<span data-ttu-id="31a3b-113">我們一開始會藉由建立新的控制器呼叫**StoreManagerController**。</span><span class="sxs-lookup"><span data-stu-id="31a3b-113">We'll begin by creating a new controller called **StoreManagerController**.</span></span> <span data-ttu-id="31a3b-114">此控制站，我們將會利用 Scaffolding 可用的功能在 ASP.NET MVC 3 Tools Update。</span><span class="sxs-lookup"><span data-stu-id="31a3b-114">For this controller, we will be taking advantage of the Scaffolding features available in the ASP.NET MVC 3 Tools Update.</span></span> <span data-ttu-id="31a3b-115">設定 [加入控制器] 對話方塊的選項，如下所示。</span><span class="sxs-lookup"><span data-stu-id="31a3b-115">Set the options for the Add Controller dialog as shown below.</span></span>

![](mvc-music-store-part-5/_static/image1.png)

<span data-ttu-id="31a3b-116">當您按一下 [新增] 按鈕時，您會看到 ASP.NET MVC 3 scaffolding 機制會為您工作的理想數量：</span><span class="sxs-lookup"><span data-stu-id="31a3b-116">When you click the Add button, you'll see that the ASP.NET MVC 3 scaffolding mechanism does a good amount of work for you:</span></span>

- <span data-ttu-id="31a3b-117">它會建立新的 StoreManagerController 與 Entity Framework 的區域變數</span><span class="sxs-lookup"><span data-stu-id="31a3b-117">It creates the new StoreManagerController with a local Entity Framework variable</span></span>
- <span data-ttu-id="31a3b-118">它將 StoreManager 資料夾加入專案的 [檢視] 資料夾</span><span class="sxs-lookup"><span data-stu-id="31a3b-118">It adds a StoreManager folder to the project's Views folder</span></span>
- <span data-ttu-id="31a3b-119">它會新增 Create.cshtml、 Delete.cshtml、 Details.cshtml、 Edit.cshtml 和 Index.cshtml 檢視中，強類型專輯類別</span><span class="sxs-lookup"><span data-stu-id="31a3b-119">It adds Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml, and Index.cshtml view, strongly typed to the Album class</span></span>

![](mvc-music-store-part-5/_static/image2.png)

<span data-ttu-id="31a3b-120">新的 StoreManager 控制器類別包含 CRUD （建立、 讀取、 更新、 刪除） 知道如何處理與專輯控制器動作模型類別，並使用我們的 Entity Framework 內容的資料庫存取權。</span><span class="sxs-lookup"><span data-stu-id="31a3b-120">The new StoreManager controller class includes CRUD (create, read, update, delete) controller actions which know how to work with the Album model class and use our Entity Framework context for database access.</span></span>

## <a name="modifying-a-scaffolded-view"></a><span data-ttu-id="31a3b-121">修改 Scaffold 的檢視</span><span class="sxs-lookup"><span data-stu-id="31a3b-121">Modifying a Scaffolded View</span></span>

<span data-ttu-id="31a3b-122">請務必記得，因為我們已產生這段程式碼，它是標準 ASP.NET MVC 的程式碼，如同我們已經撰寫本教學課程。</span><span class="sxs-lookup"><span data-stu-id="31a3b-122">It's important to remember that, while this code was generated for us, it's standard ASP.NET MVC code, just like we've been writing throughout this tutorial.</span></span> <span data-ttu-id="31a3b-123">它已經用來儲存您要花費同樣的時間寫入未定案控制器程式碼，並以手動方式建立強型別的檢視，但這不是產生的程式碼可能會看到您做為您絕不能如何變更的相關註解中的這一類警告開頭的種類程式碼。</span><span class="sxs-lookup"><span data-stu-id="31a3b-123">It's intended to save you the time you'd spend on writing boilerplate controller code and creating the strongly typed views manually, but this isn't the kind of generated code you may have seen prefaced with dire warnings in comments about how you mustn't change the code.</span></span> <span data-ttu-id="31a3b-124">這是您的程式碼，而且您打算將它變更。</span><span class="sxs-lookup"><span data-stu-id="31a3b-124">This is your code, and you're expected to change it.</span></span>

<span data-ttu-id="31a3b-125">因此，讓我們快速編輯 以 StoreManager 索引檢視的開頭 (/ Views/StoreManager/Index.cshtml)。</span><span class="sxs-lookup"><span data-stu-id="31a3b-125">So, let's start with a quick edit to the StoreManager Index view (/Views/StoreManager/Index.cshtml).</span></span> <span data-ttu-id="31a3b-126">此檢視會顯示資料表會列出在我們的存放區中編輯專輯 / 詳細資料 / 刪除的連結，並包含專輯的公用屬性。</span><span class="sxs-lookup"><span data-stu-id="31a3b-126">This view will display a table which lists the Albums in our store with Edit / Details / Delete links, and includes the Album's public properties.</span></span> <span data-ttu-id="31a3b-127">我們會將移除 AlbumArtUrl 欄位中，因為它不在此顯示中非常有用。</span><span class="sxs-lookup"><span data-stu-id="31a3b-127">We'll remove the AlbumArtUrl field, as it's not very useful in this display.</span></span> <span data-ttu-id="31a3b-128">在&lt;資料表&gt;區段的 檢視程式碼中，移除&lt;第&gt;和&lt;td&gt;周圍 AlbumArtUrl 參考下列反白顯示的程式行所示的項目：</span><span class="sxs-lookup"><span data-stu-id="31a3b-128">In &lt;table&gt; section of the view code, remove the &lt;th&gt; and &lt;td&gt; elements surrounding AlbumArtUrl references, as indicated by the highlighted lines below:</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample1.cshtml)]

<span data-ttu-id="31a3b-129">修改過的檢視程式碼會出現，如下所示：</span><span class="sxs-lookup"><span data-stu-id="31a3b-129">The modified view code will appear as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample2.cshtml)]

## <a name="a-first-look-at-the-store-manager"></a><span data-ttu-id="31a3b-130">初探存放區管理員</span><span class="sxs-lookup"><span data-stu-id="31a3b-130">A first look at the Store Manager</span></span>

<span data-ttu-id="31a3b-131">現在執行應用程式，並瀏覽至/StoreManager /。</span><span class="sxs-lookup"><span data-stu-id="31a3b-131">Now run the application and browse to /StoreManager/.</span></span> <span data-ttu-id="31a3b-132">這會顯示我們未修改過，顯示一份專輯連結來編輯詳細資料，以及刪除存放區中儲存 Manager 索引。</span><span class="sxs-lookup"><span data-stu-id="31a3b-132">This displays the Store Manager Index we just modified, showing a list of the albums in the store with links to Edit, Details, and Delete.</span></span>

![](mvc-music-store-part-5/_static/image3.png)

<span data-ttu-id="31a3b-133">按一下 [編輯] 連結顯示的專輯、 音樂類型與演出者包括下拉式清單的編輯表單欄位。</span><span class="sxs-lookup"><span data-stu-id="31a3b-133">Clicking the Edit link displays an edit form with fields for the Album, including dropdowns for Genre and Artist.</span></span>

![](mvc-music-store-part-5/_static/image4.png)

<span data-ttu-id="31a3b-134">按一下底部的 「 回至清單 」 連結，然後按一下專輯的詳細資訊連結。</span><span class="sxs-lookup"><span data-stu-id="31a3b-134">Click the "Back to List" link at the bottom, then click on the Details link for an Album.</span></span> <span data-ttu-id="31a3b-135">這會顯示個別的專輯的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="31a3b-135">This displays the detail information for an individual Album.</span></span>

![](mvc-music-store-part-5/_static/image5.png)

<span data-ttu-id="31a3b-136">同樣地，按一下 [清單] 連結回到，然後按一下 [刪除] 連結。</span><span class="sxs-lookup"><span data-stu-id="31a3b-136">Again, click the Back to List link, then click on a Delete link.</span></span> <span data-ttu-id="31a3b-137">這會顯示確認對話方塊中，顯示專輯詳細資料，並詢問我們是否確定要刪除它。</span><span class="sxs-lookup"><span data-stu-id="31a3b-137">This displays a confirmation dialog, showing the album details and asking if we're sure we want to delete it.</span></span>

![](mvc-music-store-part-5/_static/image6.png)

<span data-ttu-id="31a3b-138">按一下底部的 [刪除] 按鈕會刪除專輯和您返回索引頁面，其中顯示刪除的專輯。</span><span class="sxs-lookup"><span data-stu-id="31a3b-138">Clicking the Delete button at the bottom will delete the album and return you to the Index page, which shows the album deleted.</span></span>

<span data-ttu-id="31a3b-139">我們無法完成與存放區管理員，但我們有使用控制器和檢視 CRUD 作業，從啟動的程式碼。</span><span class="sxs-lookup"><span data-stu-id="31a3b-139">We're not done with the Store Manager, but we have working controller and view code for the CRUD operations to start from.</span></span>

## <a name="looking-at-the-store-manager-controller-code"></a><span data-ttu-id="31a3b-140">從存放區管理員控制器程式碼</span><span class="sxs-lookup"><span data-stu-id="31a3b-140">Looking at the Store Manager Controller code</span></span>

<span data-ttu-id="31a3b-141">存放區管理員控制器包含理想的程式碼數量。</span><span class="sxs-lookup"><span data-stu-id="31a3b-141">The Store Manager Controller contains a good amount of code.</span></span> <span data-ttu-id="31a3b-142">我們再逐行這由上而下。</span><span class="sxs-lookup"><span data-stu-id="31a3b-142">Let's go through this from top to bottom.</span></span> <span data-ttu-id="31a3b-143">控制器包含一些標準的命名空間，MVC 控制器，以及我們的模型命名空間的參考。</span><span class="sxs-lookup"><span data-stu-id="31a3b-143">The controller includes some standard namespaces for an MVC controller, as well as a reference to our Models namespace.</span></span> <span data-ttu-id="31a3b-144">控制器有 MusicStoreEntities，資料存取的每個控制器動作使用的私用執行個體。</span><span class="sxs-lookup"><span data-stu-id="31a3b-144">The controller has a private instance of MusicStoreEntities, used by each of the controller actions for data access.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample3.cs)]

### <a name="store-manager-index-and-details-actions"></a><span data-ttu-id="31a3b-145">存放管理員索引和詳細資料的動作</span><span class="sxs-lookup"><span data-stu-id="31a3b-145">Store Manager Index and Details actions</span></span>

<span data-ttu-id="31a3b-146">索引檢視擷取專輯，包括每個相簿參考內容類型與演出者資訊，我們之前看到時使用的存放區瀏覽方法的清單。</span><span class="sxs-lookup"><span data-stu-id="31a3b-146">The index view retrieves a list of Albums, including each album's referenced Genre and Artist information, as we previously saw when working on the Store Browse method.</span></span> <span data-ttu-id="31a3b-147">索引檢視，讓它讓控制器正在有效率且查詢的原始要求中的這項資訊可以顯示每個相簿的內容類型名稱和演出者名稱遵循連結物件的參考。</span><span class="sxs-lookup"><span data-stu-id="31a3b-147">The Index view is following the references to the linked objects so that it can display each album's Genre name and Artist name, so the controller is being efficient and querying for this information in the original request.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample4.cs)]

<span data-ttu-id="31a3b-148">StoreManager 控制站的詳細資料控制器動作的運作方式完全相同的存放區控制站的詳細資料動作，我們先前-撰寫查詢專輯依識別碼使用 Find() 方法，再將它傳至檢視。</span><span class="sxs-lookup"><span data-stu-id="31a3b-148">The StoreManager Controller's Details controller action works exactly the same as the Store Controller Details action we wrote previously - it queries for the Album by ID using the Find() method, then returns it to the view.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample5.cs)]

### <a name="the-create-action-methods"></a><span data-ttu-id="31a3b-149">建立動作方法</span><span class="sxs-lookup"><span data-stu-id="31a3b-149">The Create Action Methods</span></span>

<span data-ttu-id="31a3b-150">建立動作方法是從 1 到目前為止，我們看到稍有不同，因為它們處理表單的輸入。</span><span class="sxs-lookup"><span data-stu-id="31a3b-150">The Create action methods are a little different from ones we've seen so far, because they handle form input.</span></span> <span data-ttu-id="31a3b-151">當使用者第一次造訪時 /StoreManager/建立/它們將會顯示一個空白表單。</span><span class="sxs-lookup"><span data-stu-id="31a3b-151">When a user first visits /StoreManager/Create/ they will be shown an empty form.</span></span> <span data-ttu-id="31a3b-152">這個 HTML 網頁將會包含&lt;表單&gt;項目，其中包含下拉式清單和文字方塊中輸入項目可以在其中輸入專輯的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="31a3b-152">This HTML page will contain a &lt;form&gt; element that contains dropdown and textbox input elements where they can enter the album's details.</span></span>

<span data-ttu-id="31a3b-153">使用者填入專輯表單值之後，可按 [儲存] 按鈕回到我們的應用程式儲存在資料庫中的這些變更送出。</span><span class="sxs-lookup"><span data-stu-id="31a3b-153">After the user fills in the Album form values, they can press the "Save" button to submit these changes back to our application to save within the database.</span></span> <span data-ttu-id="31a3b-154">當使用者按下 [儲存] 按鈕&lt;表單&gt;會執行 HTTP POST 回到 /StoreManager/建立/URL，並提交&lt;表單&gt;值做為 HTTP post 要求的一部分。</span><span class="sxs-lookup"><span data-stu-id="31a3b-154">When the user presses the "save" button the &lt;form&gt; will perform an HTTP-POST back to the /StoreManager/Create/ URL and submit the &lt;form&gt; values as part of the HTTP-POST.</span></span>

<span data-ttu-id="31a3b-155">ASP.NET MVC 可讓我們來輕鬆地分割的這兩個 URL 引動過程案例邏輯藉由啟用我們實作 StoreManagerController 類別 – 其中一個來處理初始 HTTP GET 瀏覽至 /StoreManager/Create 內兩個個別 「 建立 」 動作方法/ URL 和其他以處理 HTTP POST 的提交的變更。</span><span class="sxs-lookup"><span data-stu-id="31a3b-155">ASP.NET MVC allows us to easily split up the logic of these two URL invocation scenarios by enabling us to implement two separate "Create" action methods within our StoreManagerController class – one to handle the initial HTTP-GET browse to the /StoreManager/Create/ URL, and the other to handle the HTTP-POST of the submitted changes.</span></span>

### <a name="passing-information-to-a-view-using-viewbag"></a><span data-ttu-id="31a3b-156">將資訊傳遞至檢視，使用 ViewBag</span><span class="sxs-lookup"><span data-stu-id="31a3b-156">Passing information to a View using ViewBag</span></span>

<span data-ttu-id="31a3b-157">我們稍早在本教學課程中使用 ViewBag，但您尚未討論許多資訊，請參閱。</span><span class="sxs-lookup"><span data-stu-id="31a3b-157">We've used the ViewBag earlier in this tutorial, but haven't talked much about it.</span></span> <span data-ttu-id="31a3b-158">ViewBag 可讓我們將資訊傳遞至檢視，而不需要使用強型別的模型物件。</span><span class="sxs-lookup"><span data-stu-id="31a3b-158">The ViewBag allows us to pass information to the view without using a strongly typed model object.</span></span> <span data-ttu-id="31a3b-159">在此情況下，需要這兩種內容類型和演出者名稱清單傳遞至表單，以填入下拉式清單中，我們編輯 HTTP-GET 控制器動作，並傳回它們作為 ViewBag 項目是最簡單的方式執行此作業。</span><span class="sxs-lookup"><span data-stu-id="31a3b-159">In this case, our Edit HTTP-GET controller action needs to pass both a list of Genres and Artists to the form to populate the dropdowns, and the simplest way to do that is to return them as ViewBag items.</span></span>

<span data-ttu-id="31a3b-160">ViewBag 是動態的物件，這表示，您可以輸入 ViewBag.Foo 或 ViewBag.YourNameHere 而不需要撰寫程式碼來定義這些屬性。</span><span class="sxs-lookup"><span data-stu-id="31a3b-160">The ViewBag is a dynamic object, meaning that you can type ViewBag.Foo or ViewBag.YourNameHere without writing code to define those properties.</span></span> <span data-ttu-id="31a3b-161">在此情況下，控制器會使用 ViewBag.GenreId 和 ViewBag.ArtistId，因此會 GenreId 與 ArtistId，它們將設定的專輯屬性的下拉式清單中值的表單提交。</span><span class="sxs-lookup"><span data-stu-id="31a3b-161">In this case, the controller code uses ViewBag.GenreId and ViewBag.ArtistId so that the dropdown values submitted with the form will be GenreId and ArtistId, which are the Album properties they will be setting.</span></span>

<span data-ttu-id="31a3b-162">使用 SelectList 物件、 建立只針對該目的表單會傳回這些下拉式清單中的值。</span><span class="sxs-lookup"><span data-stu-id="31a3b-162">These dropdown values are returned to the form using the SelectList object, which is built just for that purpose.</span></span> <span data-ttu-id="31a3b-163">這是類似的程式碼：</span><span class="sxs-lookup"><span data-stu-id="31a3b-163">This is done using code like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample6.cs)]

<span data-ttu-id="31a3b-164">您可以看到從動作方法的程式碼，建立此物件正在使用三個參數：</span><span class="sxs-lookup"><span data-stu-id="31a3b-164">As you can see from the action method code, three parameters are being used to create this object:</span></span>

- <span data-ttu-id="31a3b-165">將會顯示下拉式清單中的項目清單。</span><span class="sxs-lookup"><span data-stu-id="31a3b-165">The list of items the dropdown will be displaying.</span></span> <span data-ttu-id="31a3b-166">請注意，這不只是字串的-我們所傳遞的內容類型清單。</span><span class="sxs-lookup"><span data-stu-id="31a3b-166">Note that this isn't just a string - we're passing a list of Genres.</span></span>
- <span data-ttu-id="31a3b-167">下一個參數傳遞至 SelectList 為選取值。</span><span class="sxs-lookup"><span data-stu-id="31a3b-167">The next parameter being passed to the SelectList is the Selected Value.</span></span> <span data-ttu-id="31a3b-168">此 SelectList 如何知道如何預先選取清單中的項目。</span><span class="sxs-lookup"><span data-stu-id="31a3b-168">This how the SelectList knows how to pre-select an item in the list.</span></span> <span data-ttu-id="31a3b-169">這會是更易於了解當我們查看的編輯表單，這可能會很類似。</span><span class="sxs-lookup"><span data-stu-id="31a3b-169">This will be easier to understand when we look at the Edit form, which is pretty similar.</span></span>
- <span data-ttu-id="31a3b-170">最後一個參數是要顯示的屬性。</span><span class="sxs-lookup"><span data-stu-id="31a3b-170">The final parameter is the property to be displayed.</span></span> <span data-ttu-id="31a3b-171">在此情況下，這代表 Genre.Name 屬性會將顯示給使用者。</span><span class="sxs-lookup"><span data-stu-id="31a3b-171">In this case, this is indicating that the Genre.Name property is what will be shown to the user.</span></span>

<span data-ttu-id="31a3b-172">這一點之後，HTTP GET 建立動作是很簡單-ViewBag，會加入兩個 SelectLists 然後沒有模型物件會傳遞至表單，（因為它尚未尚未建立）。</span><span class="sxs-lookup"><span data-stu-id="31a3b-172">With that in mind, then, the HTTP-GET Create action is pretty simple - two SelectLists are added to the ViewBag, and no model object is passed to the form (since it hasn't been created yet).</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample7.cs)]

### <a name="html-helpers-to-display-the-drop-downs-in-the-create-view"></a><span data-ttu-id="31a3b-173">顯示卸除的清單中建立檢視的 HTML Helper</span><span class="sxs-lookup"><span data-stu-id="31a3b-173">HTML Helpers to display the Drop Downs in the Create View</span></span>

<span data-ttu-id="31a3b-174">因為我們談論的下拉式清單值傳遞至檢視的方式，讓我們快速查看檢視來查看這些值的顯示方式。</span><span class="sxs-lookup"><span data-stu-id="31a3b-174">Since we've talked about how the drop down values are passed to the view, let's take a quick look at the view to see how those values are displayed.</span></span> <span data-ttu-id="31a3b-175">在 檢視程式碼 (/ Views/StoreManager/Create.cshtml)，您會看到顯示的內容類型的卸除進行下列呼叫向下。</span><span class="sxs-lookup"><span data-stu-id="31a3b-175">In the view code (/Views/StoreManager/Create.cshtml), you'll see the following call is made to display the Genre drop down.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample8.cshtml)]

<span data-ttu-id="31a3b-176">這就是所謂的 HTML Helper-公用程式方法，而這個方法會執行的一般檢視工作。</span><span class="sxs-lookup"><span data-stu-id="31a3b-176">This is known as an HTML Helper - a utility method which performs a common view task.</span></span> <span data-ttu-id="31a3b-177">HTML Helper 會很有用的精簡、 可讀性，讓我們檢視程式碼。</span><span class="sxs-lookup"><span data-stu-id="31a3b-177">HTML Helpers are very useful in keeping our view code concise and readable.</span></span> <span data-ttu-id="31a3b-178">Html.DropDownList helper 係由 ASP.NET MVC 中，但我們稍後就會看到很可能建立自己的 helper 檢視程式碼，我們將在我們的應用程式中重複使用。</span><span class="sxs-lookup"><span data-stu-id="31a3b-178">The Html.DropDownList helper is provided by ASP.NET MVC, but as we'll see later it's possible to create our own helpers for view code we'll reuse in our application.</span></span>

<span data-ttu-id="31a3b-179">Html.DropDownList 呼叫只要被告知兩件事-其中 get 清單以顯示，以及哪些值 （如果有的話） 應該是預先選取。</span><span class="sxs-lookup"><span data-stu-id="31a3b-179">The Html.DropDownList call just needs to be told two things - where to get the list to display, and what value (if any) should be pre-selected.</span></span> <span data-ttu-id="31a3b-180">第一個參數 GenreId，告知 DropDownList 以尋找名 GenreId 為模型或 ViewBag 中的值。</span><span class="sxs-lookup"><span data-stu-id="31a3b-180">The first parameter, GenreId, tells the DropDownList to look for a value named GenreId in either the model or ViewBag.</span></span> <span data-ttu-id="31a3b-181">第二個參數用來指出要顯示的值為初始下拉式清單中選取。</span><span class="sxs-lookup"><span data-stu-id="31a3b-181">The second parameter is used to indicate the value to show as initially selected in the drop down list.</span></span> <span data-ttu-id="31a3b-182">這種形式是建立表單，因為沒有要預先選取的值，並傳遞 String.Empty。</span><span class="sxs-lookup"><span data-stu-id="31a3b-182">Since this form is a Create form, there's no value to be preselected and String.Empty is passed.</span></span>

### <a name="handling-the-posted-form-values"></a><span data-ttu-id="31a3b-183">處理張貼的表單值</span><span class="sxs-lookup"><span data-stu-id="31a3b-183">Handling the Posted Form values</span></span>

<span data-ttu-id="31a3b-184">如同我們討論之前，有兩個與每個表單相關聯的動作方法。</span><span class="sxs-lookup"><span data-stu-id="31a3b-184">As we discussed before, there are two action methods associated with each form.</span></span> <span data-ttu-id="31a3b-185">第一個會處理 HTTP GET 要求，並顯示表單。</span><span class="sxs-lookup"><span data-stu-id="31a3b-185">The first handles the HTTP-GET request and displays the form.</span></span> <span data-ttu-id="31a3b-186">第二個處理的 HTTP POST 要求，其中包含送出的表單值。</span><span class="sxs-lookup"><span data-stu-id="31a3b-186">The second handles the HTTP-POST request, which contains the submitted form values.</span></span> <span data-ttu-id="31a3b-187">請注意，控制器動作會有 [HttpPost] 屬性會告知 ASP.NET MVC 只應該回應 HTTP POST 要求。</span><span class="sxs-lookup"><span data-stu-id="31a3b-187">Notice that controller action has an [HttpPost] attribute, which tells ASP.NET MVC that it should only respond to HTTP-POST requests.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample9.cs)]

<span data-ttu-id="31a3b-188">這個動作具有四個責任：</span><span class="sxs-lookup"><span data-stu-id="31a3b-188">This action has four responsibilities:</span></span>

- 1. <span data-ttu-id="31a3b-189">讀取格式值</span><span class="sxs-lookup"><span data-stu-id="31a3b-189">Read the form values</span></span>
- 2. <span data-ttu-id="31a3b-190">檢查表單值是否傳遞的任何驗證規則</span><span class="sxs-lookup"><span data-stu-id="31a3b-190">Check if the form values pass any validation rules</span></span>
- 3. <span data-ttu-id="31a3b-191">如果表單提交有效時，將資料儲存和顯示更新的清單</span><span class="sxs-lookup"><span data-stu-id="31a3b-191">If the form submission is valid, save the data and display the updated list</span></span>
- 4. <span data-ttu-id="31a3b-192">如果表單提交不是有效的重新顯示發生驗證錯誤表單</span><span class="sxs-lookup"><span data-stu-id="31a3b-192">If the form submission is not valid, redisplay the form with validation errors</span></span>

#### <a name="reading-form-values-with-model-binding"></a><span data-ttu-id="31a3b-193">使用模型繫結的讀取格式值</span><span class="sxs-lookup"><span data-stu-id="31a3b-193">Reading Form Values with Model Binding</span></span>

<span data-ttu-id="31a3b-194">控制器動作正在處理的標題、 價格和 AlbumArtUrl 包含 GenreId 和值 ArtistId （從下拉式清單） 和文字方塊值的表單提交。</span><span class="sxs-lookup"><span data-stu-id="31a3b-194">The controller action is processing a form submission that includes values for GenreId and ArtistId (from the drop down list) and textbox values for Title, Price, and AlbumArtUrl.</span></span> <span data-ttu-id="31a3b-195">雖然可以直接存取表單值、 更好的方法是使用內建於 ASP.NET MVC 模型繫結功能。</span><span class="sxs-lookup"><span data-stu-id="31a3b-195">While it's possible to directly access form values, a better approach is to use the Model Binding capabilities built into ASP.NET MVC.</span></span> <span data-ttu-id="31a3b-196">當控制器動作，會採用類型做為參數的模型時，ASP.NET MVC 會嘗試將填入表單輸入 （以及路由和查詢字串值） 使用該類型的物件。</span><span class="sxs-lookup"><span data-stu-id="31a3b-196">When a controller action takes a model type as a parameter, ASP.NET MVC will attempt to populate an object of that type using form inputs (as well as route and querystring values).</span></span> <span data-ttu-id="31a3b-197">其做法是要尋找的名稱符合物件的內容模型，例如設定新的專輯物件 GenreId 值時，它會尋找名稱 GenreId 輸入。</span><span class="sxs-lookup"><span data-stu-id="31a3b-197">It does this by looking for values whose names match properties of the model object, e.g. when setting the new Album object's GenreId value, it looks for an input with the name GenreId.</span></span> <span data-ttu-id="31a3b-198">當您建立檢視，在 ASP.NET MVC 中使用標準方法時，將一律使用屬性名稱做為輸入的欄位名稱，所以此欄位名稱會只符合會呈現表單。</span><span class="sxs-lookup"><span data-stu-id="31a3b-198">When you create views using the standard methods in ASP.NET MVC, the forms will always be rendered using property names as input field names, so this the field names will just match up.</span></span>

#### <a name="validating-the-model"></a><span data-ttu-id="31a3b-199">驗證模型</span><span class="sxs-lookup"><span data-stu-id="31a3b-199">Validating the Model</span></span>

<span data-ttu-id="31a3b-200">簡單 ModelState.IsValid 呼叫驗證模型。</span><span class="sxs-lookup"><span data-stu-id="31a3b-200">The model is validated with a simple call to ModelState.IsValid.</span></span> <span data-ttu-id="31a3b-201">我們還沒有加入任何驗證規則，我們專輯類別，但-我們將會執行位元-所以現在這項檢查不是多執行。</span><span class="sxs-lookup"><span data-stu-id="31a3b-201">We haven't added any validation rules to our Album class yet - we'll do that in a bit - so right now this check doesn't have much to do.</span></span> <span data-ttu-id="31a3b-202">重要的是此 ModelStat.IsValid 檢查將會調整我們打我們的模型，讓未來的變更，驗證規則不需要任何更新控制器動作的程式碼的驗證規則。</span><span class="sxs-lookup"><span data-stu-id="31a3b-202">What's important is that this ModelStat.IsValid check will adapt to the validation rules we put on our model, so future changes to validation rules won't require any updates to the controller action code.</span></span>

#### <a name="saving-the-submitted-values"></a><span data-ttu-id="31a3b-203">正在儲存送出的值</span><span class="sxs-lookup"><span data-stu-id="31a3b-203">Saving the submitted values</span></span>

<span data-ttu-id="31a3b-204">如果表單提交通過驗證，則將值儲存到資料庫的時間。</span><span class="sxs-lookup"><span data-stu-id="31a3b-204">If the form submission passes validation, it's time to save the values to the database.</span></span> <span data-ttu-id="31a3b-205">使用 Entity Framework 中，只需要將模型加入到專輯集合，然後呼叫 SaveChanges。</span><span class="sxs-lookup"><span data-stu-id="31a3b-205">With Entity Framework, that just requires adding the model to the Albums collection and calling SaveChanges.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample10.cs)]

<span data-ttu-id="31a3b-206">Entity Framework 產生適當的 SQL 命令，以保存值。</span><span class="sxs-lookup"><span data-stu-id="31a3b-206">Entity Framework generates the appropriate SQL commands to persist the value.</span></span> <span data-ttu-id="31a3b-207">將資料儲存之後, 我們將重新導向回相簿，我們會看到我們更新。</span><span class="sxs-lookup"><span data-stu-id="31a3b-207">After saving the data, we redirect back to the list of Albums so we can see our update.</span></span> <span data-ttu-id="31a3b-208">這是藉由傳回 RedirectToAction 控制器動作，我們想要顯示的名稱。</span><span class="sxs-lookup"><span data-stu-id="31a3b-208">This is done by returning RedirectToAction with the name of the controller action we want displayed.</span></span> <span data-ttu-id="31a3b-209">在此情況下，這是索引方法。</span><span class="sxs-lookup"><span data-stu-id="31a3b-209">In this case, that's the Index method.</span></span>

#### <a name="displaying-invalid-form-submissions-with-validation-errors"></a><span data-ttu-id="31a3b-210">顯示無效的表單提交發生驗證錯誤</span><span class="sxs-lookup"><span data-stu-id="31a3b-210">Displaying invalid form submissions with Validation Errors</span></span>

<span data-ttu-id="31a3b-211">在無效的格式輸入的情況下的下拉式清單中的值會加入至 ViewBag （如同 HTTP GET 案例中） 且繫結的模型值已傳送回檢視顯示。</span><span class="sxs-lookup"><span data-stu-id="31a3b-211">In the case of invalid form input, the dropdown values are added to the ViewBag (as in the HTTP-GET case) and the bound model values are passed back to the view for display.</span></span> <span data-ttu-id="31a3b-212">驗證錯誤會自動顯示使用@Html.ValidationMessageForHTML Helper。</span><span class="sxs-lookup"><span data-stu-id="31a3b-212">Validation errors are automatically displayed using the @Html.ValidationMessageFor HTML Helper.</span></span>

#### <a name="testing-the-create-form"></a><span data-ttu-id="31a3b-213">測試建立表單</span><span class="sxs-lookup"><span data-stu-id="31a3b-213">Testing the Create Form</span></span>

<span data-ttu-id="31a3b-214">若要測試這種情況時執行的應用程式和瀏覽至 /StoreManager/建立 /-，這會顯示空白表單 StoreController 建立 HTTP GET 方法所傳回。</span><span class="sxs-lookup"><span data-stu-id="31a3b-214">To test this out, run the application and browse to /StoreManager/Create/ - this will show you the blank form which was returned by the StoreController Create HTTP-GET method.</span></span>

<span data-ttu-id="31a3b-215">填滿一些值，然後按一下 送出表單的 建立 按鈕。</span><span class="sxs-lookup"><span data-stu-id="31a3b-215">Fill in some values and click the Create button to submit the form.</span></span>

![](mvc-music-store-part-5/_static/image7.png)

![](mvc-music-store-part-5/_static/image8.png)

### <a name="handling-edits"></a><span data-ttu-id="31a3b-216">處理編輯</span><span class="sxs-lookup"><span data-stu-id="31a3b-216">Handling Edits</span></span>

<span data-ttu-id="31a3b-217">（HTTP GET 和 HTTP POST） 的編輯動作組是非常類似於剛剛建立動作方法。</span><span class="sxs-lookup"><span data-stu-id="31a3b-217">The Edit action pair (HTTP-GET and HTTP-POST) are very similar to the Create action methods we just looked at.</span></span> <span data-ttu-id="31a3b-218">因為編輯案例牽涉到使用現有的相簿，編輯 HTTP-GET 方法會根據 「 識別碼 」 參數的相簿，載入透過路由傳遞中。</span><span class="sxs-lookup"><span data-stu-id="31a3b-218">Since the edit scenario involves working with an existing album, the Edit HTTP-GET method loads the Album based on the "id" parameter, passed in via the route.</span></span> <span data-ttu-id="31a3b-219">因為我們已經先前討論過，詳細資料控制器動作中擷取的 AlbumId 相簿的這段程式碼都是相同的。</span><span class="sxs-lookup"><span data-stu-id="31a3b-219">This code for retrieving an album by AlbumId is the same as we've previously looked at in the Details controller action.</span></span> <span data-ttu-id="31a3b-220">使用 建立 / HTTP GET 方法，透過 ViewBag 傳回下拉式值清單。</span><span class="sxs-lookup"><span data-stu-id="31a3b-220">As with the Create / HTTP-GET method, the drop down values are returned via the ViewBag.</span></span> <span data-ttu-id="31a3b-221">這可讓我們回到相簿為我們的模型物件的檢視 （這強型別專輯類別） 並透過 ViewBag 傳遞其他資料 （例如內容類型清單）。</span><span class="sxs-lookup"><span data-stu-id="31a3b-221">This allows us to return an Album as our model object to the view (which is strongly typed to the Album class) while passing additional data (e.g. a list of Genres) via the ViewBag.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample11.cs)]

<span data-ttu-id="31a3b-222">編輯 HTTP POST 動作是非常類似於建立 HTTP POST 動作。</span><span class="sxs-lookup"><span data-stu-id="31a3b-222">The Edit HTTP-POST action is very similar to the Create HTTP-POST action.</span></span> <span data-ttu-id="31a3b-223">唯一的差別，而非資料庫中新增新的相簿。專輯集合中，我們發現專輯的目前執行個體使用 db。Entry(album) 並將其狀態設定為已修改。</span><span class="sxs-lookup"><span data-stu-id="31a3b-223">The only difference is that instead of adding a new album to the db.Albums collection, we're finding the current instance of the Album using db.Entry(album) and setting its state to Modified.</span></span> <span data-ttu-id="31a3b-224">這告訴我們要修改現有的相簿，而非建立新的 Entity Framework。</span><span class="sxs-lookup"><span data-stu-id="31a3b-224">This tells Entity Framework that we are modifying an existing album as opposed to creating a new one.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample12.cs)]

<span data-ttu-id="31a3b-225">我們可以執行應用程式，並瀏覽至/StoreManger/，然後按一下專輯的編輯連結此進行測試。</span><span class="sxs-lookup"><span data-stu-id="31a3b-225">We can test this out by running the application and browsing to /StoreManger/, then clicking the Edit link for an album.</span></span>

![](mvc-music-store-part-5/_static/image9.png)

<span data-ttu-id="31a3b-226">這會顯示編輯 HTTP GET 方法所顯示的編輯表單。</span><span class="sxs-lookup"><span data-stu-id="31a3b-226">This displays the Edit form shown by the Edit HTTP-GET method.</span></span> <span data-ttu-id="31a3b-227">填滿一些值，然後按一下 [儲存] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="31a3b-227">Fill in some values and click the Save button.</span></span>

![](mvc-music-store-part-5/_static/image10.png)

<span data-ttu-id="31a3b-228">這將表單、 儲存值，並傳回我們相簿清單中，顯示已更新的值。</span><span class="sxs-lookup"><span data-stu-id="31a3b-228">This posts the form, saves the values, and returns us to the Album list, showing that the values were updated.</span></span>

![](mvc-music-store-part-5/_static/image11.png)

### <a name="handling-deletion"></a><span data-ttu-id="31a3b-229">處理刪除</span><span class="sxs-lookup"><span data-stu-id="31a3b-229">Handling Deletion</span></span>

<span data-ttu-id="31a3b-230">刪除遵循相同的模式，以編輯和建立、 使用一個控制器動作，以顯示 [確認] 表單中和另一個控制器動作，以處理表單送出。</span><span class="sxs-lookup"><span data-stu-id="31a3b-230">Deletion follows the same pattern as Edit and Create, using one controller action to display the confirmation form, and another controller action to handle the form submission.</span></span>

<span data-ttu-id="31a3b-231">HTTP GET 刪除控制器動作完全是我們先前存放區管理員詳細資料控制器動作相同。</span><span class="sxs-lookup"><span data-stu-id="31a3b-231">The HTTP-GET Delete controller action is exactly the same as our previous Store Manager Details controller action.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample13.cs)]

<span data-ttu-id="31a3b-232">我們使用 刪除檢視的內容樣板專輯類型顯示表單的強型別。</span><span class="sxs-lookup"><span data-stu-id="31a3b-232">We display a form that's strongly typed to an Album type, using the Delete view content template.</span></span>

![](mvc-music-store-part-5/_static/image12.png)

<span data-ttu-id="31a3b-233">刪除範本會顯示模型的所有欄位，但我們可以稍微簡化該清單。</span><span class="sxs-lookup"><span data-stu-id="31a3b-233">The Delete template shows all the fields for the model, but we can simplify that down quite a bit.</span></span> <span data-ttu-id="31a3b-234">變更 /Views/StoreManager/Delete.cshtml 中的檢視程式碼所示。</span><span class="sxs-lookup"><span data-stu-id="31a3b-234">Change the view code in /Views/StoreManager/Delete.cshtml to the following.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample14.cshtml)]

<span data-ttu-id="31a3b-235">這會顯示確認簡化的刪除。</span><span class="sxs-lookup"><span data-stu-id="31a3b-235">This displays a simplified Delete confirmation.</span></span>

![](mvc-music-store-part-5/_static/image13.png)

<span data-ttu-id="31a3b-236">按一下 [刪除] 按鈕會導致表單回傳至伺服器，它會執行 DeleteConfirmed 動作。</span><span class="sxs-lookup"><span data-stu-id="31a3b-236">Clicking the Delete button causes the form to be posted back to the server, which executes the DeleteConfirmed action.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample15.cs)]

<span data-ttu-id="31a3b-237">我們 HTTP POST 刪除控制器動作會採取下列動作：</span><span class="sxs-lookup"><span data-stu-id="31a3b-237">Our HTTP-POST Delete Controller Action takes the following actions:</span></span>

- 1. <span data-ttu-id="31a3b-238">載入的專輯依識別碼</span><span class="sxs-lookup"><span data-stu-id="31a3b-238">Loads the Album by ID</span></span>
- 2. <span data-ttu-id="31a3b-239">專輯將它刪除，並儲存變更</span><span class="sxs-lookup"><span data-stu-id="31a3b-239">Deletes it the album and save changes</span></span>
- 3. <span data-ttu-id="31a3b-240">重新導向至索引中，顯示已從清單中移除相簿</span><span class="sxs-lookup"><span data-stu-id="31a3b-240">Redirects to the Index, showing that the Album was removed from the list</span></span>

<span data-ttu-id="31a3b-241">若要測試這種情況，執行應用程式並瀏覽至 /StoreManager。</span><span class="sxs-lookup"><span data-stu-id="31a3b-241">To test this, run the application and browse to /StoreManager.</span></span> <span data-ttu-id="31a3b-242">從清單中選取相簿，然後按一下 [刪除] 連結。</span><span class="sxs-lookup"><span data-stu-id="31a3b-242">Select an album from the list and click the Delete link.</span></span>

![](mvc-music-store-part-5/_static/image14.png)

<span data-ttu-id="31a3b-243">這會顯示我們刪除確認 畫面中。</span><span class="sxs-lookup"><span data-stu-id="31a3b-243">This displays our Delete confirmation screen.</span></span>

![](mvc-music-store-part-5/_static/image15.png)

<span data-ttu-id="31a3b-244">按一下 [刪除] 按鈕時，移除專輯，並傳回我們至存放區管理員索引頁中，顯示已刪除的專輯。</span><span class="sxs-lookup"><span data-stu-id="31a3b-244">Clicking the Delete button removes the album and returns us to the Store Manager Index page, which shows that the album has been deleted.</span></span>

![](mvc-music-store-part-5/_static/image16.png)

### <a name="using-a-custom-html-helper-to-truncate-text"></a><span data-ttu-id="31a3b-245">使用自訂的 HTML Helper 來截斷文字</span><span class="sxs-lookup"><span data-stu-id="31a3b-245">Using a custom HTML Helper to truncate text</span></span>

<span data-ttu-id="31a3b-246">我們有一個可能的問題與我們的存放區管理員索引頁面。</span><span class="sxs-lookup"><span data-stu-id="31a3b-246">We've got one potential issue with our Store Manager Index page.</span></span> <span data-ttu-id="31a3b-247">我們專輯標題和演出者名稱屬性可以同時為時間夠長，它們可能會擲回，關閉我們表格格式。</span><span class="sxs-lookup"><span data-stu-id="31a3b-247">Our Album Title and Artist Name properties can both be long enough that they could throw off our table formatting.</span></span> <span data-ttu-id="31a3b-248">我們將建立自訂的 HTML Helper，可讓我們來輕鬆地截斷這些和其他屬性，在我們的檢視。</span><span class="sxs-lookup"><span data-stu-id="31a3b-248">We'll create a custom HTML Helper to allow us to easily truncate these and other properties in our Views.</span></span>

![](mvc-music-store-part-5/_static/image17.png)

<span data-ttu-id="31a3b-249">Razor 的@helper語法，變成很容易就能在您的檢視中建立您自己的 helper 函式使用。</span><span class="sxs-lookup"><span data-stu-id="31a3b-249">Razor's @helper syntax has made it pretty easy to create your own helper functions for use in your views.</span></span> <span data-ttu-id="31a3b-250">開啟 /Views/StoreManager/Index.cshtml 檢視，然後加入下列程式碼直接之後@model列。</span><span class="sxs-lookup"><span data-stu-id="31a3b-250">Open the /Views/StoreManager/Index.cshtml view and add the following code directly after the @model line.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample16.cshtml)]

<span data-ttu-id="31a3b-251">這個 helper 方法採用字串和允許的最大長度。</span><span class="sxs-lookup"><span data-stu-id="31a3b-251">This helper method takes a string and a maximum length to allow.</span></span> <span data-ttu-id="31a3b-252">如果提供的文字長度小於指定的長度，協助專家將其輸出為-是。</span><span class="sxs-lookup"><span data-stu-id="31a3b-252">If the text supplied is shorter than the length specified, the helper outputs it as-is.</span></span> <span data-ttu-id="31a3b-253">如果是較長，它會截斷文字，並呈現"..."的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="31a3b-253">If it is longer, then it truncates the text and renders "…" for the remainder.</span></span>

<span data-ttu-id="31a3b-254">現在我們可以使用我們截斷的協助程式，以確保專輯標題和演出者名稱的屬性是少於 25 個字元。</span><span class="sxs-lookup"><span data-stu-id="31a3b-254">Now we can use our Truncate helper to ensure that both the Album Title and Artist Name properties are less than 25 characters.</span></span> <span data-ttu-id="31a3b-255">使用新的截斷協助程式的完整檢視程式碼下方會出現。</span><span class="sxs-lookup"><span data-stu-id="31a3b-255">The complete view code using our new Truncate helper appears below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample17.cshtml)]

<span data-ttu-id="31a3b-256">現在我們瀏覽 /StoreManager/ URL 時，專輯和標題會維持以下我們的最大長度。</span><span class="sxs-lookup"><span data-stu-id="31a3b-256">Now when we browse the /StoreManager/ URL, the albums and titles are kept below our maximum lengths.</span></span>

![](mvc-music-store-part-5/_static/image18.png)

<span data-ttu-id="31a3b-257">注意： 這會顯示簡單的情況下，建立及使用協助程式在一個檢視中。</span><span class="sxs-lookup"><span data-stu-id="31a3b-257">Note: This shows the simple case of creating and using a helper in one view.</span></span> <span data-ttu-id="31a3b-258">若要深入了解建立協助程式，您可以在整個網站使用，請參閱我的部落格文章： [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)</span><span class="sxs-lookup"><span data-stu-id="31a3b-258">To learn more about creating helpers that you can use throughout your site, see my blog post: [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)</span></span>


>[!div class="step-by-step"]
<span data-ttu-id="31a3b-259">[上一頁](mvc-music-store-part-4.md)
[下一頁](mvc-music-store-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="31a3b-259">[Previous](mvc-music-store-part-4.md)
[Next](mvc-music-store-part-6.md)</span></span>
