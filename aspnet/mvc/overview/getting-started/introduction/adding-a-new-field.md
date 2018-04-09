---
uid: mvc/overview/getting-started/introduction/adding-a-new-field
title: 加入新欄位 |Microsoft 文件
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 4085de68-d243-4378-8a64-86236ea8d2da
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 0dac798eba586cdcc232cedd262e610b954004df
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-new-field"></a><span data-ttu-id="495fb-102">新增欄位</span><span class="sxs-lookup"><span data-stu-id="495fb-102">Adding a New Field</span></span>
====================
<span data-ttu-id="495fb-103">由[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="495fb-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

<span data-ttu-id="495fb-104">這一節中，您將使用 Entity Framework Code First 移轉來移轉至模型類別的某些變更，因此變更套用至資料庫。</span><span class="sxs-lookup"><span data-stu-id="495fb-104">In this section you'll use Entity Framework Code First Migrations to migrate some changes to the model classes so the change is applied to the database.</span></span>

<span data-ttu-id="495fb-105">根據預設，當您使用 Entity Framework Code First 來自動建立的資料庫，您可以如同稍早在本教學課程，第一個程式碼將資料表加入至要協助追蹤資料庫的結構描述是否從產生的模型類別同步的資料庫。</span><span class="sxs-lookup"><span data-stu-id="495fb-105">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="495fb-106">如果不是同步，Entity Framework 會擲回錯誤。</span><span class="sxs-lookup"><span data-stu-id="495fb-106">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="495fb-107">這可讓您更輕鬆地在開發期間，您可能否則只 （藉由晦澀難懂的錯誤） 在執行階段追蹤問題。</span><span class="sxs-lookup"><span data-stu-id="495fb-107">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span>

## <a name="setting-up-code-first-migrations-for-model-changes"></a><span data-ttu-id="495fb-108">Code First 移轉設定的模型變更</span><span class="sxs-lookup"><span data-stu-id="495fb-108">Setting up Code First Migrations for Model Changes</span></span>

<span data-ttu-id="495fb-109">瀏覽至 [方案總管] 中。</span><span class="sxs-lookup"><span data-stu-id="495fb-109">Navigate to Solution Explorer.</span></span> <span data-ttu-id="495fb-110">以滑鼠右鍵按一下*Movies.mdf*檔案，然後選取**刪除**移除電影資料庫。</span><span class="sxs-lookup"><span data-stu-id="495fb-110">Right click on the *Movies.mdf* file and select **Delete** to remove the movies database.</span></span> <span data-ttu-id="495fb-111">如果您沒有看到*Movies.mdf*檔案中，按一下**顯示所有檔案**圖示中的紅色外框，如下所示。</span><span class="sxs-lookup"><span data-stu-id="495fb-111">If you don't see the *Movies.mdf* file, click on the **Show All Files** icon shown below in the red outline.</span></span>

![](adding-a-new-field/_static/image1.png)

<span data-ttu-id="495fb-112">建置應用程式，確定沒有任何錯誤。</span><span class="sxs-lookup"><span data-stu-id="495fb-112">Build the application to make sure there are no errors.</span></span>

<span data-ttu-id="495fb-113">從**工具**功能表上，按一下  **NuGet 套件管理員**然後**Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="495fb-113">From the **Tools** menu, click **NuGet Package Manager** and then **Package Manager Console**.</span></span>

![加入組件攔截](adding-a-new-field/_static/image2.png)

<span data-ttu-id="495fb-115">在**Package Manager Console**視窗`PM>`提示字元中輸入</span><span class="sxs-lookup"><span data-stu-id="495fb-115">In the **Package Manager Console** window at the `PM>` prompt enter</span></span>

<span data-ttu-id="495fb-116">Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext</span><span class="sxs-lookup"><span data-stu-id="495fb-116">Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext</span></span>

![](adding-a-new-field/_static/image3.png)

<span data-ttu-id="495fb-117">**Enable-migrations** （如上所示） 的命令會建立*configuration.cs 中*檔案中的新*移轉*資料夾。</span><span class="sxs-lookup"><span data-stu-id="495fb-117">The **Enable-Migrations** command (shown above) creates a *Configuration.cs* file in a new *Migrations* folder.</span></span>

![](adding-a-new-field/_static/image4.png)

<span data-ttu-id="495fb-118">Visual Studio 隨即開啟*configuration.cs 中*檔案。</span><span class="sxs-lookup"><span data-stu-id="495fb-118">Visual Studio opens the *Configuration.cs* file.</span></span> <span data-ttu-id="495fb-119">取代`Seed`方法中的*configuration.cs 中*以下列程式碼檔案：</span><span class="sxs-lookup"><span data-stu-id="495fb-119">Replace the `Seed` method in the *Configuration.cs* file with the following code:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

<span data-ttu-id="495fb-120">將滑鼠停留在底下的紅色曲線`Movie`按一下`Show Potential Fixes`，然後按一下 **使用** **MvcMovie.Models;**</span><span class="sxs-lookup"><span data-stu-id="495fb-120">Hover over the red squiggly line under `Movie` and click `Show Potential Fixes` and then click **using** **MvcMovie.Models;**</span></span>

![](adding-a-new-field/_static/image5.png)

<span data-ttu-id="495fb-121">此舉會將下列 using 陳述式：</span><span class="sxs-lookup"><span data-stu-id="495fb-121">Doing so adds the following using statement:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

> [!NOTE]
> 
> <span data-ttu-id="495fb-122">Code First 移轉呼叫`Seed`方法之後每個移轉 (也就呼叫**更新資料庫**Package Manager Console 中)，這個方法會更新已經插入，或如果將它們插入的資料列和它們不存在。</span><span class="sxs-lookup"><span data-stu-id="495fb-122">Code First Migrations calls the `Seed` method after every migration (that is, calling **update-database** in the Package Manager Console), and this method updates rows that have already been inserted, or inserts them if they don't exist yet.</span></span>
> 
> <span data-ttu-id="495fb-123">[AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx)下列程式碼中的方法會執行 「 更新插入 」 作業：</span><span class="sxs-lookup"><span data-stu-id="495fb-123">The [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) method in the following code performs an "upsert" operation:</span></span>
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample3.cs)]
> 
> <span data-ttu-id="495fb-124">因為[種子](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx)方法執行每個移轉，因為您嘗試新增的資料列，就會有第一個移轉建立資料庫之後插入資料。</span><span class="sxs-lookup"><span data-stu-id="495fb-124">Because the [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) method runs with every migration, you can't just insert data, because the rows you are trying to add will already be there after the first migration that creates the database.</span></span> <span data-ttu-id="495fb-125">「[Upsert](http://en.wikipedia.org/wiki/Upsert)」 作業會讓您嘗試要插入的資料列已經存在，會發生的錯誤，但是它會覆寫任何您所做測試應用程式時的資料變更。</span><span class="sxs-lookup"><span data-stu-id="495fb-125">The "[upsert](http://en.wikipedia.org/wiki/Upsert)" operation prevents errors that would happen if you try to insert a row that already exists, but it overrides any changes to data that you may have made while testing the application.</span></span> <span data-ttu-id="495fb-126">測試資料表中的資料部分可能不會想才會發生： 在某些情況下測試時變更資料時要您的資料庫更新後要保持的變更。</span><span class="sxs-lookup"><span data-stu-id="495fb-126">With test data in some tables you might not want that to happen: in some cases when you change data while testing you want your changes to remain after database updates.</span></span> <span data-ttu-id="495fb-127">在此情況下您要執行條件式的 insert 作業： 插入資料列，只有當其不存在。</span><span class="sxs-lookup"><span data-stu-id="495fb-127">In that case you want to do a conditional insert operation: insert a row only if it doesn't already exist.</span></span>   
> 
> <span data-ttu-id="495fb-128">第一個參數傳遞至[AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx)方法會指定用來檢查資料列是否已經存在的屬性。</span><span class="sxs-lookup"><span data-stu-id="495fb-128">The first parameter passed to the [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) method specifies the property to use to check if a row already exists.</span></span> <span data-ttu-id="495fb-129">您提供的測試電影資料`Title`因為清單中的每個書名是唯一的可以針對此用途使用屬性：</span><span class="sxs-lookup"><span data-stu-id="495fb-129">For the test movie data that you are providing, the `Title` property can be used for this purpose since each title in the list is unique:</span></span>
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample4.cs)]
> 
> <span data-ttu-id="495fb-130">此程式碼假設項目都是唯一的。</span><span class="sxs-lookup"><span data-stu-id="495fb-130">This code assumes that titles are unique.</span></span> <span data-ttu-id="495fb-131">如果您手動加入重複的標題，您會取得下列的例外狀況下一次執行移轉。</span><span class="sxs-lookup"><span data-stu-id="495fb-131">If you manually add a duplicate title, you'll get the following exception the next time you perform a migration.</span></span>   
> 
>  <span data-ttu-id="495fb-132">*序列包含一個以上的項目*</span><span class="sxs-lookup"><span data-stu-id="495fb-132">*Sequence contains more than one element*</span></span>  
> 
> <span data-ttu-id="495fb-133">如需有關[AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx)方法，請參閱[小心以 EF 4.3 AddOrUpdate 方法](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)...</span><span class="sxs-lookup"><span data-stu-id="495fb-133">For more information about the [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) method, see [Take care with EF 4.3 AddOrUpdate Method](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)..</span></span>


<span data-ttu-id="495fb-134">**按 CTRL-SHIFT-B 以建置專案。**（如果您不要在此時建置將會失敗的下列步驟）。</span><span class="sxs-lookup"><span data-stu-id="495fb-134">**Press CTRL-SHIFT-B to build the project.**(The following steps will fail if you don't build at this point.)</span></span>

<span data-ttu-id="495fb-135">下一個步驟是建立`DbMigration`初始移轉的類別。</span><span class="sxs-lookup"><span data-stu-id="495fb-135">The next step is to create a `DbMigration` class for the initial migration.</span></span> <span data-ttu-id="495fb-136">此移轉會建立新的資料庫，就是為什麼您刪除*movie.mdf*上一個步驟中的檔案。</span><span class="sxs-lookup"><span data-stu-id="495fb-136">This migration creates a new database, that's why you deleted the *movie.mdf* file in a previous step.</span></span>

<span data-ttu-id="495fb-137">在**Package Manager Console**視窗中，輸入命令`add-migration Initial`建立初始的移轉。</span><span class="sxs-lookup"><span data-stu-id="495fb-137">In the **Package Manager Console** window, enter the command `add-migration Initial` to create the initial migration.</span></span> <span data-ttu-id="495fb-138">「 初始 」 的名稱是任意的用來建立的移轉檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="495fb-138">The name "Initial" is arbitrary and is used to name the migration file created.</span></span>

![](adding-a-new-field/_static/image6.png)

<span data-ttu-id="495fb-139">Code First 移轉建立的另一個類別檔案*移轉*資料夾 (具有名稱*{DateStamp}\_Initial.cs* )，而且這個類別包含程式碼會建立資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="495fb-139">Code First Migrations creates another class file in the *Migrations* folder (with the name *{DateStamp}\_Initial.cs* ), and this class contains code that creates the database schema.</span></span> <span data-ttu-id="495fb-140">移轉 filename 預先固定時間戳記為協助進行排序。</span><span class="sxs-lookup"><span data-stu-id="495fb-140">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="495fb-141">檢查*{DateStamp}\_Initial.cs*檔案，它包含的指示來建立`Movies`影片資料庫的資料表。</span><span class="sxs-lookup"><span data-stu-id="495fb-141">Examine the *{DateStamp}\_Initial.cs* file, it contains the instructions to create the `Movies` table for the Movie DB.</span></span> <span data-ttu-id="495fb-142">當您更新的資料庫中的指示，在此之下*{DateStamp}\_Initial.cs*檔案將會執行並建立資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="495fb-142">When you update the database in the instructions below, this *{DateStamp}\_Initial.cs* file will run and create the DB schema.</span></span> <span data-ttu-id="495fb-143">然後在**種子**方法會執行填入資料庫的測試資料。</span><span class="sxs-lookup"><span data-stu-id="495fb-143">Then the **Seed** method will run to populate the DB with test data.</span></span>

<span data-ttu-id="495fb-144">在**Package Manager Console**，輸入命令`update-database`來建立資料庫和執行`Seed`方法。</span><span class="sxs-lookup"><span data-stu-id="495fb-144">In the **Package Manager Console**, enter the command `update-database` to create the database and run the `Seed` method.</span></span>

![](adding-a-new-field/_static/image7.png)

<span data-ttu-id="495fb-145">如果您收到錯誤，指出資料表已經存在，而且無法建立，它可能是因為刪除資料庫之後，您在執行之前，執行該應用程式`update-database`。</span><span class="sxs-lookup"><span data-stu-id="495fb-145">If you get an error that indicates a table already exists and can't be created, it is probably because you ran the application after you deleted the database and before you executed `update-database`.</span></span> <span data-ttu-id="495fb-146">在此情況下，刪除*Movies.mdf*檔案一次，然後重試`update-database`命令。</span><span class="sxs-lookup"><span data-stu-id="495fb-146">In that case, delete the *Movies.mdf* file again and retry the `update-database` command.</span></span> <span data-ttu-id="495fb-147">如果您仍然收到錯誤，刪除 migrations 資料夾內容，則開頭的指示，在此頁面最上方 (這是刪除*Movies.mdf*檔案，然後繼續進行 Enable-migrations)。</span><span class="sxs-lookup"><span data-stu-id="495fb-147">If you still get an error, delete the migrations folder and contents then start with the instructions at the top of this page (that is delete the *Movies.mdf* file then proceed to Enable-Migrations).</span></span> <span data-ttu-id="495fb-148">如果您仍然收到項目 」，請開啟 SQL Server 物件總管，並從清單中移除資料庫。</span><span class="sxs-lookup"><span data-stu-id="495fb-148">If you still get an eror, open SQL Server Object Explorer and remove the database from the list.</span></span>

<span data-ttu-id="495fb-149">執行應用程式，並瀏覽至*/Movies* URL。</span><span class="sxs-lookup"><span data-stu-id="495fb-149">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="495fb-150">種子資料會顯示。</span><span class="sxs-lookup"><span data-stu-id="495fb-150">The seed data is displayed.</span></span>

![](adding-a-new-field/_static/image8.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="495fb-151">將 Rating 屬性新增至電影模型</span><span class="sxs-lookup"><span data-stu-id="495fb-151">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="495fb-152">開始透過新增`Rating`屬性至現有`Movie`類別。</span><span class="sxs-lookup"><span data-stu-id="495fb-152">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="495fb-153">開啟*Models\Movie.cs*檔案，然後加入`Rating`屬性如下所示：</span><span class="sxs-lookup"><span data-stu-id="495fb-153">Open the *Models\Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

<span data-ttu-id="495fb-154">完整`Movie`類別現在看起來類似下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="495fb-154">The complete `Movie` class now looks like the following code:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs?highlight=12)]

<span data-ttu-id="495fb-155">建置應用程式 (Ctrl + Shift + B)。</span><span class="sxs-lookup"><span data-stu-id="495fb-155">Build the application (Ctrl+Shift+B).</span></span>

<span data-ttu-id="495fb-156">因為您已將新欄位加入到`Movie`類別，您也需要更新繫結*白名單*以便將包含新的屬性。</span><span class="sxs-lookup"><span data-stu-id="495fb-156">Because you've added a new field to the `Movie` class, you also need to update the binding *white list* so this new property will be included.</span></span> <span data-ttu-id="495fb-157">更新`bind`屬性`Create`和`Edit`動作方法，以包括`Rating`屬性：</span><span class="sxs-lookup"><span data-stu-id="495fb-157">Update the `bind` attribute for `Create` and `Edit` action methods to include the `Rating` property:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs?highlight=1)]

<span data-ttu-id="495fb-158">您也需要更新檢視範本，以便在瀏覽器檢視中顯示、建立和編輯新的 `Rating` 屬性。</span><span class="sxs-lookup"><span data-stu-id="495fb-158">You also need to update the view templates in order to display, create and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="495fb-159">開啟*\Views\Movies\Index.cshtml*檔案，然後加入`<th>Rating</th>`欄名後方**價格**資料行。</span><span class="sxs-lookup"><span data-stu-id="495fb-159">Open the *\Views\Movies\Index.cshtml* file and add a `<th>Rating</th>` column heading just after the **Price** column.</span></span> <span data-ttu-id="495fb-160">然後加入`<td>`即將要呈現之範本的結束欄`@item.Rating`值。</span><span class="sxs-lookup"><span data-stu-id="495fb-160">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="495fb-161">以下是哪些更新*Index.cshtml*檢視範本外觀就像：</span><span class="sxs-lookup"><span data-stu-id="495fb-161">Below is what the updated *Index.cshtml* view template looks like:</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample8.cshtml?highlight=31-33,52-54)]

<span data-ttu-id="495fb-162">接下來，開啟*\Views\Movies\Create.cshtml*檔案，然後加入`Rating`下列 highlighed 標記欄位。</span><span class="sxs-lookup"><span data-stu-id="495fb-162">Next, open the *\Views\Movies\Create.cshtml* file and add the `Rating` field with the following highlighed markup.</span></span> <span data-ttu-id="495fb-163">這會呈現文字方塊中，以便建立新的電影時，您可以指定評等。</span><span class="sxs-lookup"><span data-stu-id="495fb-163">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample9.cshtml?highlight=9-15)]

<span data-ttu-id="495fb-164">您現在已更新以支援新的應用程式程式碼`Rating`屬性。</span><span class="sxs-lookup"><span data-stu-id="495fb-164">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="495fb-165">執行應用程式，並瀏覽至*/Movies* URL。</span><span class="sxs-lookup"><span data-stu-id="495fb-165">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="495fb-166">當您這樣做時，不過，您會看到下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="495fb-166">When you do this, though, you'll see one of the following errors:</span></span>

![](adding-a-new-field/_static/image9.png)  
  
<span data-ttu-id="495fb-167">備份 'MovieDBContext' 內容的模型已變更，因為所建立的資料庫。</span><span class="sxs-lookup"><span data-stu-id="495fb-167">The model backing the 'MovieDBContext' context has changed since the database was created.</span></span> <span data-ttu-id="495fb-168">請考慮使用 Code First 移轉來更新資料庫 (https://go.microsoft.com/fwlink/?LinkId=238269)。</span><span class="sxs-lookup"><span data-stu-id="495fb-168">Consider using Code First Migrations to update the database (https://go.microsoft.com/fwlink/?LinkId=238269).</span></span>

![](adding-a-new-field/_static/image10.png)

<span data-ttu-id="495fb-169">您在看到這個錯誤，因為已更新`Movie`應用程式中的模型類別現在是不同的結構描述`Movie`現有資料庫的資料表。</span><span class="sxs-lookup"><span data-stu-id="495fb-169">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="495fb-170">(資料庫資料表中沒有任何 `Rating` 資料行)。</span><span class="sxs-lookup"><span data-stu-id="495fb-170">(There's no `Rating` column in the database table.)</span></span>


<span data-ttu-id="495fb-171">有幾個方法可以解決這個錯誤：</span><span class="sxs-lookup"><span data-stu-id="495fb-171">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="495fb-172">讓 Entity Framework 自動卸除資料庫，並重新依據新的模型類別結構描述來建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="495fb-172">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="495fb-173">在開發週期早期，當您在測試資料庫上進行開發時，這個方法會很方便；其可讓您一併調整模型和資料庫結構描述，更加快速。</span><span class="sxs-lookup"><span data-stu-id="495fb-173">This approach is very convenient early in the development cycle when you are doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="495fb-174">這個選項的缺點是，您在資料庫中現有的資料遺失，因此您*不*想要在實際執行資料庫上使用此方法 ！</span><span class="sxs-lookup"><span data-stu-id="495fb-174">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span> <span data-ttu-id="495fb-175">使用初始設定式將測試資料自動植入資料庫，通常是開發應用程式的有效方式。</span><span class="sxs-lookup"><span data-stu-id="495fb-175">Using an initializer to automatically seed a database with test data is often a productive way to develop an application.</span></span> <span data-ttu-id="495fb-176">如需有關 Entity Framework 資料庫初始設定式的詳細資訊，請參閱[ASP.NET MVC/Entity Framework 教學課程](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。</span><span class="sxs-lookup"><span data-stu-id="495fb-176">For more information on Entity Framework database initializers, see [ASP.NET MVC/Entity Framework tutorial](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>
2. <span data-ttu-id="495fb-177">您可明確修改現有資料庫的結構描述，使其符合模型類別。</span><span class="sxs-lookup"><span data-stu-id="495fb-177">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="495fb-178">這種方法的優點是可以保留您的資料。</span><span class="sxs-lookup"><span data-stu-id="495fb-178">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="495fb-179">您可以手動方式或藉由建立資料庫變更指令碼來進行這項變更。</span><span class="sxs-lookup"><span data-stu-id="495fb-179">You can make this change either manually or by creating a database change script.</span></span>
3. <span data-ttu-id="495fb-180">使用 Code First 移轉來更新資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="495fb-180">Use Code First Migrations to update the database schema.</span></span>


<span data-ttu-id="495fb-181">在本教學課程中，我們將使用 Code First 移轉。</span><span class="sxs-lookup"><span data-stu-id="495fb-181">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="495fb-182">更新 Seed 方法，使其提供新的資料行的值。</span><span class="sxs-lookup"><span data-stu-id="495fb-182">Update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="495fb-183">開啟 Migrations\Configuration.cs 檔案，並將評等 欄位加入至每個影片物件。</span><span class="sxs-lookup"><span data-stu-id="495fb-183">Open Migrations\Configuration.cs file and add a Rating field to each Movie object.</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample10.cs?highlight=6)]

<span data-ttu-id="495fb-184">建置方案，然後再開啟**Package Manager Console**視窗並輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="495fb-184">Build the solution, and then open the **Package Manager Console** window and enter the following command:</span></span>

`add-migration Rating`

<span data-ttu-id="495fb-185">`add-migration`命令告訴移轉架構，以檢查目前的電影模型，與目前的電影 DB 結構描述，並建立必要的程式碼將資料庫移轉至新的模型。</span><span class="sxs-lookup"><span data-stu-id="495fb-185">The `add-migration` command tells the migration framework to examine the current movie model with the current movie DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="495fb-186">名稱*分級*是任意的以及用來移轉檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="495fb-186">The name *Rating* is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="495fb-187">最好先使用有意義的名稱，如移轉步驟。</span><span class="sxs-lookup"><span data-stu-id="495fb-187">It's helpful to use a meaningful name for the migration step.</span></span>

<span data-ttu-id="495fb-188">此命令完成時，Visual Studio 會開啟定義新的類別檔案`DbMigration`衍生的類別，然後在`Up`方法，您可以看到建立新的資料行的程式碼。</span><span class="sxs-lookup"><span data-stu-id="495fb-188">When this command finishes, Visual Studio opens the class file that defines the new `DbMigration` derived class, and in the `Up` method you can see the code that creates the new column.</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample11.cs)]

<span data-ttu-id="495fb-189">建置方案，然後再輸入`update-database`命令**Package Manager Console**視窗。</span><span class="sxs-lookup"><span data-stu-id="495fb-189">Build the solution, and then enter the `update-database` command in the **Package Manager Console** window.</span></span>

<span data-ttu-id="495fb-190">下圖顯示在輸出**Package Manager Console**視窗 (日期戳記前面加上*分級*會不同。)</span><span class="sxs-lookup"><span data-stu-id="495fb-190">The following image shows the output in the **Package Manager Console** window (The date stamp prepending *Rating* will be different.)</span></span>

![](adding-a-new-field/_static/image11.png)

<span data-ttu-id="495fb-191">重新執行應用程式，並瀏覽至 /Movies URL。</span><span class="sxs-lookup"><span data-stu-id="495fb-191">Re-run the application and navigate to the /Movies URL.</span></span> <span data-ttu-id="495fb-192">您可以看到新的 [分級] 欄位。</span><span class="sxs-lookup"><span data-stu-id="495fb-192">You can see the new Rating field.</span></span>

![](adding-a-new-field/_static/image12.png)

<span data-ttu-id="495fb-193">按一下**新建**連結加入新的電影。</span><span class="sxs-lookup"><span data-stu-id="495fb-193">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="495fb-194">請注意，您可以加入評等。</span><span class="sxs-lookup"><span data-stu-id="495fb-194">Note that you can add a rating.</span></span>

![7_CreateRioII](adding-a-new-field/_static/image13.png)

<span data-ttu-id="495fb-196">按一下 [建立] 。</span><span class="sxs-lookup"><span data-stu-id="495fb-196">Click **Create**.</span></span> <span data-ttu-id="495fb-197">新的電影，包括評等，現在會顯示列出的電影中：</span><span class="sxs-lookup"><span data-stu-id="495fb-197">The new movie, including the rating, now shows up in the movies listing:</span></span>

![7_ourNewMovie_SM](adding-a-new-field/_static/image14.png)

<span data-ttu-id="495fb-199">現在，專案使用移轉，您不需要卸除資料庫，當您加入新的欄位或否則更新結構描述。</span><span class="sxs-lookup"><span data-stu-id="495fb-199">Now that the project is using migrations, you won't need to drop the database when you add a new field or otherwise update the schema.</span></span> <span data-ttu-id="495fb-200">在下一步 區段中，我們將進行多個結構描述變更，以更新資料庫中使用的移轉。</span><span class="sxs-lookup"><span data-stu-id="495fb-200">In the next section, we'll make more schema changes and use migrations to update the database.</span></span>

<span data-ttu-id="495fb-201">您也應該加入`Rating`欄位設為 編輯、 詳細資料和 刪除檢視範本。</span><span class="sxs-lookup"><span data-stu-id="495fb-201">You should also add the `Rating` field to the Edit, Details, and Delete view templates.</span></span>

<span data-ttu-id="495fb-202">您可以輸入中的 [更新資料庫] 命令**Package Manager Console**視窗一次並沒有移轉程式碼會執行，因為結構描述符合模型。</span><span class="sxs-lookup"><span data-stu-id="495fb-202">You could enter the "update-database" command in the **Package Manager Console** window again and no migration code would run, because the schema matches the model.</span></span> <span data-ttu-id="495fb-203">不過，執行 [更新資料庫] 會執行`Seed`方法一次，而且，因為如果您變更任何的種子資料，所做的變更將會遺失`Seed`方法 upserts 資料。</span><span class="sxs-lookup"><span data-stu-id="495fb-203">However, running "update-database" will run the `Seed` method again, and if you changed any of the Seed data, the changes will be lost because the `Seed` method upserts data.</span></span> <span data-ttu-id="495fb-204">您可以深入了解`Seed`熱門 Tom Dykstra 方法[ASP.NET MVC/Entity Framework 教學課程](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。</span><span class="sxs-lookup"><span data-stu-id="495fb-204">You can read more about the `Seed` method in Tom Dykstra's popular [ASP.NET MVC/Entity Framework tutorial](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="495fb-205">本節中您已看到如何修改模型物件和資料庫中與變更保持同步。</span><span class="sxs-lookup"><span data-stu-id="495fb-205">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="495fb-206">您也學到如何填入新建立的資料庫中的範例資料，因此您可以嘗試的案例。</span><span class="sxs-lookup"><span data-stu-id="495fb-206">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="495fb-207">這是只為第一個程式碼快速簡介，請參閱[建立 ASP.NET MVC 應用程式的 Entity Framework 資料模型](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)更完整的教學課程主題上。</span><span class="sxs-lookup"><span data-stu-id="495fb-207">This was just a quick introduction to Code First, see [Creating an Entity Framework Data Model for an ASP.NET MVC Application](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) for a more complete tutorial on the subject.</span></span> <span data-ttu-id="495fb-208">接下來，讓我們看看如何將更豐富的驗證邏輯加入至模型類別，並啟用某些商務規則，以強制執行。</span><span class="sxs-lookup"><span data-stu-id="495fb-208">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="495fb-209">[上一頁](adding-search.md)
> [下一頁](adding-validation.md)</span><span class="sxs-lookup"><span data-stu-id="495fb-209">[Previous](adding-search.md)
[Next](adding-validation.md)</span></span>
