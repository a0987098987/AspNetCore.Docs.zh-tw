---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: 將新欄位新增至電影模型和資料表 |Microsoft Docs
author: Rick-Anderson
description: 注意： 本教學課程中的更新的版本就可以使用這裡使用 ASP.NET MVC 5 和 Visual Studio 2013。 這是更安全、 更容易遵循，並示範...
ms.author: aspnetcontent
ms.date: 08/28/2012
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: 2435bb50bb8124cce3150ba488ad76c012107b27
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840171"
---
<a name="adding-a-new-field-to-the-movie-model-and-table"></a><span data-ttu-id="263be-104">將新欄位新增至電影模型和資料表</span><span class="sxs-lookup"><span data-stu-id="263be-104">Adding a New Field to the Movie Model and Table</span></span>
====================
<span data-ttu-id="263be-105">藉由[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="263be-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="263be-106">本教學課程中的更新的版本可[此處](../../getting-started/introduction/getting-started.md)使用 ASP.NET MVC 5 和 Visual Studio 2013。</span><span class="sxs-lookup"><span data-stu-id="263be-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="263be-107">它更安全、 更容易遵循，並示範更多的功能。</span><span class="sxs-lookup"><span data-stu-id="263be-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="263be-108">在本節中，您將使用 Entity Framework Code First 移轉來移轉到模型類別的一些變更，因此變更套用至資料庫。</span><span class="sxs-lookup"><span data-stu-id="263be-108">In this section you'll use Entity Framework Code First Migrations to migrate some changes to the model classes so the change is applied to the database.</span></span>

<span data-ttu-id="263be-109">根據預設，當您使用 Entity Framework Code First 自動建立資料庫，如同稍早在本教學課程中，第一個程式碼將資料表加入至資料庫，以協助追蹤資料庫的結構描述是否與它所產生的模型類別同步。</span><span class="sxs-lookup"><span data-stu-id="263be-109">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="263be-110">如果未同步，Entity Framework 會擲回錯誤。</span><span class="sxs-lookup"><span data-stu-id="263be-110">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="263be-111">這可讓您更輕鬆地在開發期間，您可能否則只會找到 （藉由難以理解的錯誤） 在執行階段追蹤問題。</span><span class="sxs-lookup"><span data-stu-id="263be-111">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span>

## <a name="setting-up-code-first-migrations-for-model-changes"></a><span data-ttu-id="263be-112">Code First 移轉設定的模型變更</span><span class="sxs-lookup"><span data-stu-id="263be-112">Setting up Code First Migrations for Model Changes</span></span>

<span data-ttu-id="263be-113">如果您使用 Visual Studio 2012，按兩下*Movies.mdf*從方案總管 以開啟資料庫工具的檔案。</span><span class="sxs-lookup"><span data-stu-id="263be-113">If you are using Visual Studio 2012, double click the *Movies.mdf* file from Solution Explorer to open the database tool.</span></span> <span data-ttu-id="263be-114">Visual Studio Express for Web 會顯示資料庫總管 中，Visual Studio 2012 會顯示伺服器總管。</span><span class="sxs-lookup"><span data-stu-id="263be-114">Visual Studio Express for Web will show Database Explorer, Visual Studio 2012 will show Server Explorer.</span></span> <span data-ttu-id="263be-115">如果您使用 Visual Studio 2010，請使用 SQL Server 物件總管。</span><span class="sxs-lookup"><span data-stu-id="263be-115">If you are using Visual Studio 2010, use SQL Server Object Explorer.</span></span>

<span data-ttu-id="263be-116">在資料庫工具 （資料庫總管、 伺服器總管或 SQL Server 物件總管），以滑鼠右鍵按一下`MovieDBContext`，然後選取**刪除**卸除電影資料庫。</span><span class="sxs-lookup"><span data-stu-id="263be-116">In the database tool (Database Explorer, Server Explorer or SQL Server Object Explorer), right click on `MovieDBContext` and select **Delete** to drop the movies database.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

<span data-ttu-id="263be-117">瀏覽至 方案總管。</span><span class="sxs-lookup"><span data-stu-id="263be-117">Navigate back to Solution Explorer.</span></span> <span data-ttu-id="263be-118">以滑鼠右鍵按一下*Movies.mdf*檔案，然後選取**刪除**移除電影資料庫。</span><span class="sxs-lookup"><span data-stu-id="263be-118">Right click on the *Movies.mdf* file and select **Delete** to remove the movies database.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

<span data-ttu-id="263be-119">建置的應用程式，請確定沒有任何錯誤。</span><span class="sxs-lookup"><span data-stu-id="263be-119">Build the application to make sure there are no errors.</span></span>

<span data-ttu-id="263be-120">從**工具**功能表上，按一下**程式庫套件管理員**，然後**Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="263be-120">From the **Tools** menu, click **Library Package Manager** and then **Package Manager Console**.</span></span>

![新增組件 Man](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

<span data-ttu-id="263be-122">在  **Package Manager Console**視窗`PM>`提示字元中輸入"Enable-migrations-ContextTypeName MvcMovie.Models.MovieDBContext"。</span><span class="sxs-lookup"><span data-stu-id="263be-122">In the **Package Manager Console** window at the `PM>` prompt enter "Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext".</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

<span data-ttu-id="263be-123">**Enable-migrations** （如上所示） 的命令會建立*Configuration.cs*檔案中的新*移轉*資料夾。</span><span class="sxs-lookup"><span data-stu-id="263be-123">The **Enable-Migrations** command (shown above) creates a *Configuration.cs* file in a new *Migrations* folder.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

<span data-ttu-id="263be-124">Visual Studio 會開啟*Configuration.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="263be-124">Visual Studio opens the *Configuration.cs* file.</span></span> <span data-ttu-id="263be-125">取代`Seed`方法中的*Configuration.cs*為下列程式碼的檔案：</span><span class="sxs-lookup"><span data-stu-id="263be-125">Replace the `Seed` method in the *Configuration.cs* file with the following code:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

<span data-ttu-id="263be-126">以滑鼠右鍵按一下紅色曲線底下`Movie`，然後選取**解決**然後**使用** **MvcMovie.Models;**</span><span class="sxs-lookup"><span data-stu-id="263be-126">Right click on the red squiggly line under `Movie` and select **Resolve** then **using** **MvcMovie.Models;**</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

<span data-ttu-id="263be-127">此舉會將下列 using 陳述式：</span><span class="sxs-lookup"><span data-stu-id="263be-127">Doing so adds the following using statement:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> <span data-ttu-id="263be-128">程式碼 First Migrations 會呼叫`Seed`方法之後每個移轉 (也就呼叫**更新資料庫**在套件管理員主控台)，這個方法會更新具有已插入，或將其插入，如果資料列和它們不存在。</span><span class="sxs-lookup"><span data-stu-id="263be-128">Code First Migrations calls the `Seed` method after every migration (that is, calling **update-database** in the Package Manager Console), and this method updates rows that have already been inserted, or inserts them if they don't exist yet.</span></span>


<span data-ttu-id="263be-129">**按 CTRL-SHIFT-B 來建置專案。**(下列步驟將會失敗，如果您不要在此時建置。)</span><span class="sxs-lookup"><span data-stu-id="263be-129">**Press CTRL-SHIFT-B to build the project.**(The following steps will fail if your don't build at this point.)</span></span>

<span data-ttu-id="263be-130">下一個步驟是建立`DbMigration`類別初始移轉。</span><span class="sxs-lookup"><span data-stu-id="263be-130">The next step is to create a `DbMigration` class for the initial migration.</span></span> <span data-ttu-id="263be-131">此移轉建立的新資料庫，就是為什麼您刪除*movie.mdf*上一個步驟中的檔案。</span><span class="sxs-lookup"><span data-stu-id="263be-131">This migration to creates a new database, that's why you deleted the *movie.mdf* file in a previous step.</span></span>

<span data-ttu-id="263be-132">在 [ **Package Manager Console** ] 視窗中，輸入命令"add-migration Initial 」 來建立初始移轉。</span><span class="sxs-lookup"><span data-stu-id="263be-132">In the **Package Manager Console** window, enter the command "add-migration Initial" to create the initial migration.</span></span> <span data-ttu-id="263be-133">「 初始 」 的名稱是任意的用來命名移轉檔案建立。</span><span class="sxs-lookup"><span data-stu-id="263be-133">The name "Initial" is arbitrary and is used to name the migration file created.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

<span data-ttu-id="263be-134">Code First 移轉會建立另一個類別檔案中的*移轉*資料夾 (同名 *{時間戳記}\_Initial.cs* )，而且這個類別包含程式碼會建立資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="263be-134">Code First Migrations creates another class file in the *Migrations* folder (with the name *{DateStamp}\_Initial.cs* ), and this class contains code that creates the database schema.</span></span> <span data-ttu-id="263be-135">預先移轉檔案名稱被固定時間戳記的來協助進行排序。</span><span class="sxs-lookup"><span data-stu-id="263be-135">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="263be-136">檢查 *{時間戳記}\_Initial.cs*檔案，它包含的指示來建立電影資料表的電影資料庫。</span><span class="sxs-lookup"><span data-stu-id="263be-136">Examine the *{DateStamp}\_Initial.cs* file, it contains the instructions to create the Movies table for the Movie DB.</span></span> <span data-ttu-id="263be-137">當您更新的資料庫中的指示，在此之下 *{時間戳記}\_Initial.cs*檔案將會執行並建立資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="263be-137">When you update the database in the instructions below, this *{DateStamp}\_Initial.cs* file will run and create the DB schema.</span></span> <span data-ttu-id="263be-138">然後**種子**方法會執行以填入測試資料的資料庫。</span><span class="sxs-lookup"><span data-stu-id="263be-138">Then the **Seed** method will run to populate the DB with test data.</span></span>

<span data-ttu-id="263be-139">在  **Package Manager Console**，輸入命令"更新資料庫內 」 來建立資料庫和執行**種子**方法。</span><span class="sxs-lookup"><span data-stu-id="263be-139">In the **Package Manager Console**, enter the command "update-database" to create the database and run the **Seed** method.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

<span data-ttu-id="263be-140">如果您收到錯誤，指出資料表已經存在，而且無法建立時，可能是因為您已刪除的資料庫之後，您在執行之前，執行應用程式`update-database`。</span><span class="sxs-lookup"><span data-stu-id="263be-140">If you get an error that indicates a table already exists and can't be created, it is probably because you ran the application after you deleted the database and before you executed `update-database`.</span></span> <span data-ttu-id="263be-141">在此情況下，刪除*Movies.mdf*檔案一次，然後再試一次`update-database`命令。</span><span class="sxs-lookup"><span data-stu-id="263be-141">In that case, delete the *Movies.mdf* file again and retry the `update-database` command.</span></span> <span data-ttu-id="263be-142">如果您仍然收到錯誤，刪除移轉資料夾內容，然後開始在此頁面頂端的指示 (這是刪除*Movies.mdf*檔案，然後繼續進行 Enable-migrations)。</span><span class="sxs-lookup"><span data-stu-id="263be-142">If you still get an error, delete the migrations folder and contents then start with the instructions at the top of this page (that is delete the *Movies.mdf* file then proceed to Enable-Migrations).</span></span>

<span data-ttu-id="263be-143">執行應用程式，並瀏覽至 */Movies* URL。</span><span class="sxs-lookup"><span data-stu-id="263be-143">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="263be-144">會顯示種子資料。</span><span class="sxs-lookup"><span data-stu-id="263be-144">The seed data is displayed.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="263be-145">將 Rating 屬性新增至電影模型</span><span class="sxs-lookup"><span data-stu-id="263be-145">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="263be-146">藉由新增新開始`Rating`至現有的屬性`Movie`類別。</span><span class="sxs-lookup"><span data-stu-id="263be-146">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="263be-147">開啟*Models\Movie.cs*檔案，並新增`Rating`與下列類似的屬性：</span><span class="sxs-lookup"><span data-stu-id="263be-147">Open the *Models\Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

<span data-ttu-id="263be-148">完整`Movie`類別現在看起來類似下列的程式碼：</span><span class="sxs-lookup"><span data-stu-id="263be-148">The complete `Movie` class now looks like the following code:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

<span data-ttu-id="263be-149">建置應用程式使用**建置** &gt;**建置電影**功能表命令，或是按 CTRL shift 鍵。</span><span class="sxs-lookup"><span data-stu-id="263be-149">Build the application using the **Build** &gt;**Build Movie** menu command or by pressing CTRL-SHIFT-B.</span></span>

<span data-ttu-id="263be-150">既然您已更新`Model`類別，您也需要更新*\Views\Movies\Index.cshtml*並*\Views\Movies\Create.cshtml*檢視範本，以顯示新`Rating`瀏覽器檢視中的屬性。</span><span class="sxs-lookup"><span data-stu-id="263be-150">Now that you've updated the `Model` class, you also need to update the *\Views\Movies\Index.cshtml* and *\Views\Movies\Create.cshtml* view templates in order to display the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="263be-151">開啟<em>\Views\Movies\Index.cshtml</em>檔案，並新增`<th>Rating</th>`資料行標題後方<strong>價格</strong>資料行。</span><span class="sxs-lookup"><span data-stu-id="263be-151">Open the<em>\Views\Movies\Index.cshtml</em> file and add a `<th>Rating</th>` column heading just after the <strong>Price</strong> column.</span></span> <span data-ttu-id="263be-152">然後新增`<td>`要呈現的範本結尾附近的資料行`@item.Rating`值。</span><span class="sxs-lookup"><span data-stu-id="263be-152">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="263be-153">以下是 哪些更新<em>Index.cshtml</em>檢視範本看起來像：</span><span class="sxs-lookup"><span data-stu-id="263be-153">Below is what the updated <em>Index.cshtml</em> view template looks like:</span></span>

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

<span data-ttu-id="263be-154">接下來，開啟*\Views\Movies\Create.cshtml*檔案，並新增下列標記表單的結尾附近。</span><span class="sxs-lookup"><span data-stu-id="263be-154">Next, open the *\Views\Movies\Create.cshtml* file and add the following markup near the end of the form.</span></span> <span data-ttu-id="263be-155">這會呈現文字方塊中，如此當您建立新的電影時，您可以指定評等。</span><span class="sxs-lookup"><span data-stu-id="263be-155">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

<span data-ttu-id="263be-156">您現在已更新的應用程式程式碼，以支援新`Rating`屬性。</span><span class="sxs-lookup"><span data-stu-id="263be-156">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="263be-157">現在執行應用程式，並瀏覽至 */Movies* URL。</span><span class="sxs-lookup"><span data-stu-id="263be-157">Now run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="263be-158">當您這樣做時，不過，您會看到下列錯誤的其中一個：</span><span class="sxs-lookup"><span data-stu-id="263be-158">When you do this, though, you'll see one of the following errors:</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

<span data-ttu-id="263be-159">您之所以看到此錯誤，因為已更新`Movie`應用程式中的模型類別現在是不同的結構描述`Movie`現有資料庫的資料表。</span><span class="sxs-lookup"><span data-stu-id="263be-159">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="263be-160">(資料庫資料表中沒有任何 `Rating` 資料行)。</span><span class="sxs-lookup"><span data-stu-id="263be-160">(There's no `Rating` column in the database table.)</span></span>


<span data-ttu-id="263be-161">有幾個方法可以解決這個錯誤：</span><span class="sxs-lookup"><span data-stu-id="263be-161">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="263be-162">讓 Entity Framework 自動卸除資料庫，並重新依據新的模型類別結構描述來建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="263be-162">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="263be-163">這個方法會很方便，當測試資料庫上進行開發它可讓您一併調整更加快速的模型和資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="263be-163">This approach is very convenient when doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="263be-164">它的缺點是您在資料庫中現有的資料遺失，因此您*不*想要在生產資料庫上使用這種方法 ！</span><span class="sxs-lookup"><span data-stu-id="263be-164">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span> <span data-ttu-id="263be-165">使用初始設定式，將自動植入測試資料的資料庫，通常是開發應用程式的有效方式。</span><span class="sxs-lookup"><span data-stu-id="263be-165">Using an initializer to automatically seed a database with test data is often a productive way to develope an application.</span></span> <span data-ttu-id="263be-166">如需有關 Entity Framework 資料庫初始設定式的詳細資訊，請參閱 Tom Dykstra [ASP.NET MVC/Entity Framework 教學課程](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。</span><span class="sxs-lookup"><span data-stu-id="263be-166">For more information on Entity Framework database initializers, see Tom Dykstra's [ASP.NET MVC/Entity Framework tutorial](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>
2. <span data-ttu-id="263be-167">您可明確修改現有資料庫的結構描述，使其符合模型類別。</span><span class="sxs-lookup"><span data-stu-id="263be-167">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="263be-168">這種方法的優點是可以保留您的資料。</span><span class="sxs-lookup"><span data-stu-id="263be-168">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="263be-169">您可以手動方式或藉由建立資料庫變更指令碼來進行這項變更。</span><span class="sxs-lookup"><span data-stu-id="263be-169">You can make this change either manually or by creating a database change script.</span></span>
3. <span data-ttu-id="263be-170">使用 Code First 移轉來更新資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="263be-170">Use Code First Migrations to update the database schema.</span></span>


<span data-ttu-id="263be-171">在本教學課程中，我們將使用 Code First 移轉。</span><span class="sxs-lookup"><span data-stu-id="263be-171">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="263be-172">更新種子方法，使它提供新的資料行的值。</span><span class="sxs-lookup"><span data-stu-id="263be-172">Update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="263be-173">開啟 Migrations\Configuration.cs 檔案，並將評等 欄位新增至電影中的每個物件。</span><span class="sxs-lookup"><span data-stu-id="263be-173">Open Migrations\Configuration.cs file and add a Rating field to each Movie object.</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

<span data-ttu-id="263be-174">建置方案，然後再開啟**Package Manager Console**視窗並輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="263be-174">Build the solution, and then open the **Package Manager Console** window and enter the following command:</span></span>

`add-migration AddRatingMig`

<span data-ttu-id="263be-175">`add-migration`命令會告知移轉架構，檢查目前的電影模型，與目前的電影資料庫結構描述，並建立必要的程式碼將資料庫移轉至新的模型。</span><span class="sxs-lookup"><span data-stu-id="263be-175">The `add-migration` command tells the migration framework to examine the current movie model with the current movie DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="263be-176">AddRatingMig 是任意的用來命名移轉檔案。</span><span class="sxs-lookup"><span data-stu-id="263be-176">The AddRatingMig is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="263be-177">您最好使用有意義的名稱，如移轉步驟。</span><span class="sxs-lookup"><span data-stu-id="263be-177">It's helpful to use a meaningful name for the migration step.</span></span>

<span data-ttu-id="263be-178">此命令完成時，Visual Studio 會開啟可定義新的類別檔案`DbMIgration`衍生類別，然後在`Up`方法，您可以看到建立新的資料行的程式碼。</span><span class="sxs-lookup"><span data-stu-id="263be-178">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` derived class, and in the `Up` method you can see the code that creates the new column.</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

<span data-ttu-id="263be-179">建置方案，並輸入中的 [更新資料庫] 命令**Package Manager Console**視窗。</span><span class="sxs-lookup"><span data-stu-id="263be-179">Build the solution, and then enter the "update-database" command in the **Package Manager Console** window.</span></span>

<span data-ttu-id="263be-180">下圖顯示中的輸出**Package Manager Console**視窗 （前面加上 AddRatingMig 日期戳記將會不同。）</span><span class="sxs-lookup"><span data-stu-id="263be-180">The following image shows the output in the **Package Manager Console** window (The date stamp prepending AddRatingMig will be different.)</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

<span data-ttu-id="263be-181">重新執行應用程式，並瀏覽至 /Movies URL。</span><span class="sxs-lookup"><span data-stu-id="263be-181">Re-run the application and navigate to the /Movies URL.</span></span> <span data-ttu-id="263be-182">您可以看到新的 [評等] 欄位。</span><span class="sxs-lookup"><span data-stu-id="263be-182">You can see the new Rating field.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

<span data-ttu-id="263be-183">按一下 **新建**連結，可新增一部新電影。</span><span class="sxs-lookup"><span data-stu-id="263be-183">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="263be-184">請注意，您可以新增評分。</span><span class="sxs-lookup"><span data-stu-id="263be-184">Note that you can add a rating.</span></span>

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

<span data-ttu-id="263be-186">按一下 [建立] 。</span><span class="sxs-lookup"><span data-stu-id="263be-186">Click **Create**.</span></span> <span data-ttu-id="263be-187">新的影片，包括評等，現在會出現電影清單：</span><span class="sxs-lookup"><span data-stu-id="263be-187">The new movie, including the rating, now shows up in the movies listing:</span></span>

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

<span data-ttu-id="263be-189">您也應該新增`Rating`欄位來編輯、 詳細資料和 SearchIndex 檢視範本。</span><span class="sxs-lookup"><span data-stu-id="263be-189">You should also add the `Rating` field to the Edit, Details and SearchIndex view templates.</span></span>

<span data-ttu-id="263be-190">您可以輸入中的 [更新資料庫] 命令**Package Manager Console**視窗一次，而且不需要變更做，因為結構描述符合模型。</span><span class="sxs-lookup"><span data-stu-id="263be-190">You could enter the "update-database" command in the **Package Manager Console** window again and no changes would be made, because the schema matches the model.</span></span>

<span data-ttu-id="263be-191">在本節中您已看到如何，您便可以修改模型物件上，並讓資料庫保持同步的變更。</span><span class="sxs-lookup"><span data-stu-id="263be-191">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="263be-192">您也了解用來在填入新建立的資料庫，使用範例資料，以便您可以試試看案例。</span><span class="sxs-lookup"><span data-stu-id="263be-192">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="263be-193">接下來，讓我們看看如何將更豐富的驗證邏輯加入至模型類別，並啟用 強制執行某些商務規則。</span><span class="sxs-lookup"><span data-stu-id="263be-193">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="263be-194">[上一頁](examining-the-edit-methods-and-edit-view.md)
> [下一頁](adding-validation-to-the-model.md)</span><span class="sxs-lookup"><span data-stu-id="263be-194">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-validation-to-the-model.md)</span></span>
