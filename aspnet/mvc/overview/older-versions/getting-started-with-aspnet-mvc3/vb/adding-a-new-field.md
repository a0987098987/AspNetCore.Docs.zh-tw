---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-new-field
title: 將新欄位新增至電影模型和資料庫資料表 (VB) |Microsoft Docs
author: Rick-Anderson
description: 本教學課程將教導您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，也就是 ASP.NET MVC Web 應用程式的基本概念...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 28970e1b-1845-4015-86ef-121e52a6c397
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 8fb0fb5c1db24f1961bba08f7b1c2182caca39ca
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577427"
---
<a name="adding-a-new-field-to-the-movie-model-and-database-table-vb"></a><span data-ttu-id="eecb9-103">將新欄位新增至電影模型和資料庫資料表 (VB)</span><span class="sxs-lookup"><span data-stu-id="eecb9-103">Adding a New Field to the Movie Model and Database Table (VB)</span></span>
====================
<span data-ttu-id="eecb9-104">藉由[Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="eecb9-104">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

> <span data-ttu-id="eecb9-105">本教學課程將教導您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，也就是 Microsoft Visual Studio 的免費版本的 ASP.NET MVC Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="eecb9-105">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="eecb9-106">在開始之前，請確定您已安裝符合下列先決條件。</span><span class="sxs-lookup"><span data-stu-id="eecb9-106">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="eecb9-107">您可以安裝所有人都按下列連結： [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。</span><span class="sxs-lookup"><span data-stu-id="eecb9-107">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="eecb9-108">或者，您可以個別安裝的必要條件，使用下列連結：</span><span class="sxs-lookup"><span data-stu-id="eecb9-108">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="eecb9-109">Visual Studio Web Developer Express SP1 必要條件</span><span class="sxs-lookup"><span data-stu-id="eecb9-109">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="eecb9-110">ASP.NET MVC 3 Tools Update</span><span class="sxs-lookup"><span data-stu-id="eecb9-110">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="eecb9-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（執行階段 + 工具支援）</span><span class="sxs-lookup"><span data-stu-id="eecb9-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="eecb9-112">如果您使用 Visual Studio 2010 而不 Visual Web Developer 2010，請按一下下列連結安裝必要的： [Visual Studio 2010 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。</span><span class="sxs-lookup"><span data-stu-id="eecb9-112">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="eecb9-113">使用本主題隨附了 VB.NET 原始程式碼的 Visual Web Developer 專案。</span><span class="sxs-lookup"><span data-stu-id="eecb9-113">A Visual Web Developer project with VB.NET source code is available to accompany this topic.</span></span> <span data-ttu-id="eecb9-114">[下載 VB.NET 版本](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)。</span><span class="sxs-lookup"><span data-stu-id="eecb9-114">[Download the VB.NET version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="eecb9-115">如果您偏好 C#，切換至[C# 版本](../cs/adding-a-new-field.md)本教學課程。</span><span class="sxs-lookup"><span data-stu-id="eecb9-115">If you prefer C#, switch to the [C# version](../cs/adding-a-new-field.md) of this tutorial.</span></span>


<span data-ttu-id="eecb9-116">在本節中您將模型類別來進行一些變更，並了解如何更新資料庫結構描述，以符合的模型變更。</span><span class="sxs-lookup"><span data-stu-id="eecb9-116">In this section you'll make some changes to the model classes and learn how you can update the database schema to match the model changes.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="eecb9-117">將 Rating 屬性新增至電影模型</span><span class="sxs-lookup"><span data-stu-id="eecb9-117">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="eecb9-118">藉由新增新開始`Rating`至現有的屬性`Movie`類別。</span><span class="sxs-lookup"><span data-stu-id="eecb9-118">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="eecb9-119">開啟 *Movie.cs* 檔案，並新增`Rating`與下列類似的屬性：</span><span class="sxs-lookup"><span data-stu-id="eecb9-119">Open the *Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-vb[Main](adding-a-new-field/samples/sample1.vb)]

<span data-ttu-id="eecb9-120">完整`Movie`類別現在看起來類似下列的程式碼：</span><span class="sxs-lookup"><span data-stu-id="eecb9-120">The complete `Movie` class now looks like the following code:</span></span>

[!code-vb[Main](adding-a-new-field/samples/sample2.vb)]

<span data-ttu-id="eecb9-121">重新編譯應用程式使用**偵錯** &gt;**建置電影**功能表命令。</span><span class="sxs-lookup"><span data-stu-id="eecb9-121">Recompile the application using the **Debug** &gt;**Build Movie** menu command.</span></span>

<span data-ttu-id="eecb9-122">既然您已更新`Model`類別，您也需要更新 *\Views\Movies\Index.vbhtml* 並 *\Views\Movies\Create.vbhtml* 檢視範本，以支援新`Rating`屬性。</span><span class="sxs-lookup"><span data-stu-id="eecb9-122">Now that you've updated the `Model` class, you also need to update the *\Views\Movies\Index.vbhtml* and *\Views\Movies\Create.vbhtml* view templates in order to support the new `Rating` property.</span></span>

<span data-ttu-id="eecb9-123">開啟<em>\Views\Movies\Index.vbhtml</em>檔案，並新增`<th>Rating</th>`資料行標題後方<strong>價格</strong>資料行。</span><span class="sxs-lookup"><span data-stu-id="eecb9-123">Open the<em>\Views\Movies\Index.vbhtml</em> file and add a `<th>Rating</th>` column heading just after the <strong>Price</strong> column.</span></span> <span data-ttu-id="eecb9-124">然後新增`<td>`要呈現的範本結尾附近的資料行`@item.Rating`值。</span><span class="sxs-lookup"><span data-stu-id="eecb9-124">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="eecb9-125">以下是 哪些更新<em>Index.vbhtml</em>檢視範本看起來像：</span><span class="sxs-lookup"><span data-stu-id="eecb9-125">Below is what the updated <em>Index.vbhtml</em> view template looks like:</span></span>

[!code-vbhtml[Main](adding-a-new-field/samples/sample3.vbhtml)]

<span data-ttu-id="eecb9-126">接下來，開啟 *\Views\Movies\Create.vbhtml* 檔案，並新增下列標記表單的結尾附近。</span><span class="sxs-lookup"><span data-stu-id="eecb9-126">Next, open the *\Views\Movies\Create.vbhtml* file and add the following markup near the end of the form.</span></span> <span data-ttu-id="eecb9-127">這會呈現文字方塊中，如此當您建立新的電影時，您可以指定評等。</span><span class="sxs-lookup"><span data-stu-id="eecb9-127">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample4.cshtml)]

## <a name="managing-model-and-database-schema-differences"></a><span data-ttu-id="eecb9-128">管理模型和資料庫結構描述相異</span><span class="sxs-lookup"><span data-stu-id="eecb9-128">Managing Model and Database Schema Differences</span></span>

<span data-ttu-id="eecb9-129">您現在已更新的應用程式程式碼，以支援新`Rating`屬性。</span><span class="sxs-lookup"><span data-stu-id="eecb9-129">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="eecb9-130">現在執行應用程式，並瀏覽至 */Movies* URL。</span><span class="sxs-lookup"><span data-stu-id="eecb9-130">Now run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="eecb9-131">當您這樣做時，不過，您會看到下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="eecb9-131">When you do this, though, you'll see the following error:</span></span>

![](adding-a-new-field/_static/image1.png)

<span data-ttu-id="eecb9-132">您之所以看到此錯誤，因為已更新`Movie`應用程式中的模型類別現在是不同的結構描述`Movie`現有資料庫的資料表。</span><span class="sxs-lookup"><span data-stu-id="eecb9-132">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="eecb9-133">(資料庫資料表中沒有任何 `Rating` 資料行)。</span><span class="sxs-lookup"><span data-stu-id="eecb9-133">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="eecb9-134">根據預設，當您使用 Entity Framework Code First 自動建立資料庫，如同稍早在本教學課程中，第一個程式碼將資料表加入至資料庫，以協助追蹤資料庫的結構描述是否與它所產生的模型類別同步。</span><span class="sxs-lookup"><span data-stu-id="eecb9-134">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="eecb9-135">如果未同步，Entity Framework 會擲回錯誤。</span><span class="sxs-lookup"><span data-stu-id="eecb9-135">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="eecb9-136">這可讓您更輕鬆地在開發期間，您可能否則只會找到 （藉由難以理解的錯誤） 在執行階段追蹤問題。</span><span class="sxs-lookup"><span data-stu-id="eecb9-136">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span> <span data-ttu-id="eecb9-137">「 同步處理檢查 」 功能是什麼情況導致的錯誤訊息，會顯示您剛剛看到。</span><span class="sxs-lookup"><span data-stu-id="eecb9-137">The sync-checking feature is what causes the error message to be displayed that you just saw.</span></span>

<span data-ttu-id="eecb9-138">有兩種方法可以解決這個錯誤：</span><span class="sxs-lookup"><span data-stu-id="eecb9-138">There are two approaches to resolving the error:</span></span>

1. <span data-ttu-id="eecb9-139">讓 Entity Framework 自動卸除資料庫，並重新依據新的模型類別結構描述來建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="eecb9-139">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="eecb9-140">這個方法會很方便的測試資料庫上進行開發時因為它可讓您一併調整更加快速的模型和資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="eecb9-140">This approach is very convenient when doing active development on a test database, because it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="eecb9-141">它的缺點是您在資料庫中現有的資料遺失，因此您 *不* 想要在生產資料庫上使用這種方法 ！</span><span class="sxs-lookup"><span data-stu-id="eecb9-141">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span>
2. <span data-ttu-id="eecb9-142">您可明確修改現有資料庫的結構描述，使其符合模型類別。</span><span class="sxs-lookup"><span data-stu-id="eecb9-142">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="eecb9-143">這種方法的優點是可以保留您的資料。</span><span class="sxs-lookup"><span data-stu-id="eecb9-143">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="eecb9-144">您可以手動方式或藉由建立資料庫變更指令碼來進行這項變更。</span><span class="sxs-lookup"><span data-stu-id="eecb9-144">You can make this change either manually or by creating a database change script.</span></span>

<span data-ttu-id="eecb9-145">本教學課程中，我們將使用第一種方法，就會看到 Entity Framework Code First 每當模型變更時自動重新建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="eecb9-145">For this tutorial, we'll use the first approach — you'll have the Entity Framework Code First automatically re-create the database anytime the model changes.</span></span>

## <a name="automatically-re-creating-the-database-on-model-changes"></a><span data-ttu-id="eecb9-146">自動重新建立資料庫模型變更</span><span class="sxs-lookup"><span data-stu-id="eecb9-146">Automatically Re-Creating the Database on Model Changes</span></span>

<span data-ttu-id="eecb9-147">讓我們更新應用程式，使程式碼第一個自動卸除並重新建立資料庫，每當您變更應用程式的模型。</span><span class="sxs-lookup"><span data-stu-id="eecb9-147">Let's update the application so that Code First automatically drops and re-creates the database anytime you change the model for the application.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="eecb9-148">**警告**您應該啟用這種方法是自動卸除並重新建立資料庫，只有當您使用開發或測試資料庫時，才與 *永遠不會* 生產資料庫，其中包含實際資料。</span><span class="sxs-lookup"><span data-stu-id="eecb9-148">**Warning** You should enable this approach of automatically dropping and re-creating the database only when you're using a development or test database, and *never* on a production database that contains real data.</span></span> <span data-ttu-id="eecb9-149">使用實際執行伺服器上可能會導致資料遺失。</span><span class="sxs-lookup"><span data-stu-id="eecb9-149">Using it on a production server can lead to data loss.</span></span>


<span data-ttu-id="eecb9-150">在 [**方案總管] 中**，以滑鼠右鍵按一下 *模型* 資料夾中，選取**新增**，然後選取**類別**。</span><span class="sxs-lookup"><span data-stu-id="eecb9-150">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-new-field/_static/image2.png)

<span data-ttu-id="eecb9-151">將類別命名為&quot;MovieInitializer&quot;。</span><span class="sxs-lookup"><span data-stu-id="eecb9-151">Name the class &quot;MovieInitializer&quot;.</span></span> <span data-ttu-id="eecb9-152">更新`MovieInitializer`類別包含下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="eecb9-152">Update the `MovieInitializer` class to contain the following code:</span></span>

[!code-vb[Main](adding-a-new-field/samples/sample5.vb)]

<span data-ttu-id="eecb9-153">`MovieInitializer`類別會指定應該卸除模型所使用的資料庫，並自動重新建立如果變更的模型類別。</span><span class="sxs-lookup"><span data-stu-id="eecb9-153">The `MovieInitializer` class specifies that the database used by the model should be dropped and automatically re-created if the model classes ever change.</span></span> <span data-ttu-id="eecb9-154">程式碼包含`Seed`方法，以指定一些預設資料，來自動新增資料庫中任何時間它已建立 （或重新建立）。</span><span class="sxs-lookup"><span data-stu-id="eecb9-154">The code includes a `Seed` method to specify some default data to automatically add to the database any time it's created (or re-created).</span></span> <span data-ttu-id="eecb9-155">這會提供實用的方式，以填入某些範例資料，資料庫，而不需要您手動填入每次進行變更的模型。</span><span class="sxs-lookup"><span data-stu-id="eecb9-155">This provides a useful way to populate the database with some sample data, without requiring you to manually populate it each time you make a model change.</span></span>

<span data-ttu-id="eecb9-156">既然您已定義`MovieInitializer`類別，您會想要連接它，以便每次應用程式執行時，它會檢查是否在資料庫中結構描述不同的模型類別。</span><span class="sxs-lookup"><span data-stu-id="eecb9-156">Now that you've defined the `MovieInitializer` class, you'll want to wire it up so that each time the application runs, it checks whether the model classes are different from the schema in the database.</span></span> <span data-ttu-id="eecb9-157">如有需要，您可以執行初始設定式來重新建立要符合的模型並於其中填入範例資料與資料庫的資料庫。</span><span class="sxs-lookup"><span data-stu-id="eecb9-157">If they are, you can run the initializer to re-create the database to match the model and then populate the database with the sample data.</span></span>

<span data-ttu-id="eecb9-158">開啟 *Global.asax* 檔案的根目錄`MvcMovies`專案：</span><span class="sxs-lookup"><span data-stu-id="eecb9-158">Open the *Global.asax* file that's at the root of the `MvcMovies` project:</span></span>

<span data-ttu-id="eecb9-159">*Global.asax* 檔案中包含的類別，定義整個應用程式專案，並包含`Application_Start`應用程式第一次啟動時執行的事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="eecb9-159">The *Global.asax* file contains the class that defines the entire application for the project, and contains an `Application_Start` event handler that runs when the application first starts.</span></span>

<span data-ttu-id="eecb9-160">尋找`Application_Start`方法並將呼叫加入`Database.SetInitializer`開頭的方法，如下所示：</span><span class="sxs-lookup"><span data-stu-id="eecb9-160">Find the `Application_Start` method and add a call to `Database.SetInitializer` at the beginning of the method, as shown below:</span></span>

[!code-vb[Main](adding-a-new-field/samples/sample6.vb)]

<span data-ttu-id="eecb9-161">`Database.SetInitializer`剛加入的陳述式指出所使用的資料庫`MovieDBContext`執行個體應該會自動刪除並重新建立如果不符合結構描述和資料庫。</span><span class="sxs-lookup"><span data-stu-id="eecb9-161">The `Database.SetInitializer` statement you just added indicates that the database used by the `MovieDBContext` instance should be automatically deleted and re-created if the schema and the database don't match.</span></span> <span data-ttu-id="eecb9-162">如您所見，它也會填入範例資料中指定資料庫和`MovieInitializer`類別。</span><span class="sxs-lookup"><span data-stu-id="eecb9-162">And as you saw, it will also populate the database with the sample data that's specified in the `MovieInitializer` class.</span></span>

<span data-ttu-id="eecb9-163">關閉 *Global.asax* 檔案。</span><span class="sxs-lookup"><span data-stu-id="eecb9-163">Close the *Global.asax* file.</span></span>

<span data-ttu-id="eecb9-164">重新執行應用程式，並瀏覽至 */Movies* URL。</span><span class="sxs-lookup"><span data-stu-id="eecb9-164">Re-run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="eecb9-165">當應用程式啟動時，它會偵測模型結構不再符合資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="eecb9-165">When the application starts, it detects that the model structure no longer matches the database schema.</span></span> <span data-ttu-id="eecb9-166">它會自動重新建立資料庫，以符合新的模型結構，並於其中填入資料庫與範例影片：</span><span class="sxs-lookup"><span data-stu-id="eecb9-166">It automatically re-creates the database to match the new model structure and populates the database with the sample movies:</span></span>

![7_MyMovieList_SM](adding-a-new-field/_static/image3.png)

<span data-ttu-id="eecb9-168">按一下 新建\*\*連結，可新增一部新電影。</span><span class="sxs-lookup"><span data-stu-id="eecb9-168">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="eecb9-169">請注意，您可以新增評分。</span><span class="sxs-lookup"><span data-stu-id="eecb9-169">Note that you can add a rating.</span></span>

<span data-ttu-id="eecb9-170">[![7_CreateRioII](adding-a-new-field/_static/image5.png)](adding-a-new-field/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="eecb9-170">[![7_CreateRioII](adding-a-new-field/_static/image5.png)](adding-a-new-field/_static/image4.png)</span></span>

<span data-ttu-id="eecb9-171">按一下 [建立] 。</span><span class="sxs-lookup"><span data-stu-id="eecb9-171">Click **Create**.</span></span> <span data-ttu-id="eecb9-172">新的影片，包括評等，現在會出現電影清單：</span><span class="sxs-lookup"><span data-stu-id="eecb9-172">The new movie, including the rating, now shows up in the movies listing:</span></span>

![7_ourNewMovie_SM](adding-a-new-field/_static/image6.png)

<span data-ttu-id="eecb9-174">在本節中您已看到如何，您便可以修改模型物件上，並讓資料庫保持同步的變更。</span><span class="sxs-lookup"><span data-stu-id="eecb9-174">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="eecb9-175">您也了解用來在填入新建立的資料庫，使用範例資料，以便您可以試試看案例。</span><span class="sxs-lookup"><span data-stu-id="eecb9-175">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="eecb9-176">接下來，讓我們看看如何將更豐富的驗證邏輯加入至模型類別，並啟用 強制執行某些商務規則。</span><span class="sxs-lookup"><span data-stu-id="eecb9-176">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="eecb9-177">[上一頁](examining-the-edit-methods-and-edit-view.md)
> [下一頁](adding-validation-to-the-model.md)</span><span class="sxs-lookup"><span data-stu-id="eecb9-177">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-validation-to-the-model.md)</span></span>
