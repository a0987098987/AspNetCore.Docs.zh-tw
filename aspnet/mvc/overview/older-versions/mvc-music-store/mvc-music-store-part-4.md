---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: "第 4 部分： 模型和資料存取 |Microsoft 文件"
author: jongalloway
description: "此教學課程系列詳細列出所有建置 ASP.NET MVC 商店範例應用程式所採取的步驟。 第 4 部分涵蓋模型和資料存取。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: 727336344493b439130b2cf0ec6e7b5925dd5637
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="part-4-models-and-data-access"></a><span data-ttu-id="166ce-104">第 4 部分： 模型和資料存取</span><span class="sxs-lookup"><span data-stu-id="166ce-104">Part 4: Models and Data Access</span></span>
====================
<span data-ttu-id="166ce-105">由[Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="166ce-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="166ce-106">MVC Music Store 是介紹並逐步說明如何使用 ASP.NET MVC 和 Visual Studio 針對 web 程式開發的教學課程應用程式。</span><span class="sxs-lookup"><span data-stu-id="166ce-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="166ce-107">MVC Music Store 是輕量級範例存放區實作銷售音樂專輯上線，並實作基本的網站管理、 使用者登入和購物車的功能。</span><span class="sxs-lookup"><span data-stu-id="166ce-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>
> 
> <span data-ttu-id="166ce-108">此教學課程系列詳細列出所有建置 ASP.NET MVC 商店範例應用程式所採取的步驟。</span><span class="sxs-lookup"><span data-stu-id="166ce-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="166ce-109">第 4 部分涵蓋模型和資料存取。</span><span class="sxs-lookup"><span data-stu-id="166ce-109">Part 4 covers Models and Data Access.</span></span>


<span data-ttu-id="166ce-110">目前為止，我們已只已傳遞 「 虛擬資料 」 從我們的控制站我們檢視範本。</span><span class="sxs-lookup"><span data-stu-id="166ce-110">So far, we've just been passing "dummy data" from our Controllers to our View templates.</span></span> <span data-ttu-id="166ce-111">現在我們已經準備好實際資料庫相連結。</span><span class="sxs-lookup"><span data-stu-id="166ce-111">Now we're ready to hook up a real database.</span></span> <span data-ttu-id="166ce-112">本教學課程中我們將會涵蓋如何使用 SQL Server Compact Edition （通常稱為 SQL CE） 做為我們的資料庫引擎。</span><span class="sxs-lookup"><span data-stu-id="166ce-112">In this tutorial we'll be covering how to use SQL Server Compact Edition (often called SQL CE) as our database engine.</span></span> <span data-ttu-id="166ce-113">SQL CE 是免費的內嵌，檔案型的資料庫，不需要任何安裝或組態，讓它的本機開發很方便。</span><span class="sxs-lookup"><span data-stu-id="166ce-113">SQL CE is a free, embedded, file based database that doesn't require any installation or configuration, which makes it really convenient for local development.</span></span>

## <a name="database-access-with-entity-framework-code-first"></a><span data-ttu-id="166ce-114">資料庫存取權與 Entity Framework 程式碼優先 （contract-first）</span><span class="sxs-lookup"><span data-stu-id="166ce-114">Database access with Entity Framework Code-First</span></span>

<span data-ttu-id="166ce-115">我們會使用包含在 ASP.NET MVC 3 專案，以查詢和更新資料庫的 Entity Framework (EF) 支援。</span><span class="sxs-lookup"><span data-stu-id="166ce-115">We'll use the Entity Framework (EF) support that is included in ASP.NET MVC 3 projects to query and update the database.</span></span> <span data-ttu-id="166ce-116">EF 是彈性物件關聯式對應 (ORM) 資料的 API，可讓開發人員查詢及更新的資料儲存在資料庫中的物件導向的方式。</span><span class="sxs-lookup"><span data-stu-id="166ce-116">EF is a flexible object relational mapping (ORM) data API that enables developers to query and update data stored in a database in an object-oriented way.</span></span>

<span data-ttu-id="166ce-117">Entity Framework 第 4 版支援呼叫程式碼優先開發架構。</span><span class="sxs-lookup"><span data-stu-id="166ce-117">Entity Framework version 4 supports a development paradigm called code-first.</span></span> <span data-ttu-id="166ce-118">程式碼優先 （contract-first） 可讓您撰寫簡單的類別 (也稱為 POCO 從 「 純舊"CLR 物件)，來建立模型物件，甚至可以從您的類別上建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="166ce-118">Code-first allows you to create model object by writing simple classes (also known as POCO from "plain-old" CLR objects), and can even create the database on the fly from your classes.</span></span>

### <a name="changes-to-our-model-classes"></a><span data-ttu-id="166ce-119">變更模型類別</span><span class="sxs-lookup"><span data-stu-id="166ce-119">Changes to our Model Classes</span></span>

<span data-ttu-id="166ce-120">我們將在本教學課程充分利用 Entity Framework 中的資料庫建立功能。</span><span class="sxs-lookup"><span data-stu-id="166ce-120">We will be leveraging the database creation feature in Entity Framework in this tutorial.</span></span> <span data-ttu-id="166ce-121">這樣做之前，不過，讓我們進行一些次要變更給我們的模型類別，以新增中我們將在稍後使用的一些事項。</span><span class="sxs-lookup"><span data-stu-id="166ce-121">Before we do that, though, let's make a few minor changes to our model classes to add in some things we'll be using later on.</span></span>

#### <a name="adding-the-artist-model-classes"></a><span data-ttu-id="166ce-122">加入演出者模型類別</span><span class="sxs-lookup"><span data-stu-id="166ce-122">Adding the Artist Model Classes</span></span>

<span data-ttu-id="166ce-123">我們專輯會演出者、 相關聯，所以我們會將簡單的模型類別來描述演出者。</span><span class="sxs-lookup"><span data-stu-id="166ce-123">Our Albums will be associated with Artists, so we'll add a simple model class to describe an Artist.</span></span> <span data-ttu-id="166ce-124">新類別加入名為 Artist.cs 使用如下所示的程式碼的 [模型] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="166ce-124">Add a new class to the Models folder named Artist.cs using the code shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a><span data-ttu-id="166ce-125">更新模型類別</span><span class="sxs-lookup"><span data-stu-id="166ce-125">Updating our Model Classes</span></span>

<span data-ttu-id="166ce-126">更新專輯類別，如下所示。</span><span class="sxs-lookup"><span data-stu-id="166ce-126">Update the Album class as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

<span data-ttu-id="166ce-127">接下來，進行下列更新類型類別。</span><span class="sxs-lookup"><span data-stu-id="166ce-127">Next, make the following updates to the Genre class.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-appdata-folder"></a><span data-ttu-id="166ce-128">新增應用程式\_資料資料夾</span><span class="sxs-lookup"><span data-stu-id="166ce-128">Adding the App\_Data folder</span></span>

<span data-ttu-id="166ce-129">我們會將應用程式新增\_專案來保存我們 SQL Server Express 資料庫檔案的資料目錄。</span><span class="sxs-lookup"><span data-stu-id="166ce-129">We'll add an App\_Data directory to our project to hold our SQL Server Express database files.</span></span> <span data-ttu-id="166ce-130">應用程式\_資料是在 ASP.NET 中已經有資料庫存取權的正確的安全性存取權限的特殊目錄。</span><span class="sxs-lookup"><span data-stu-id="166ce-130">App\_Data is a special directory in ASP.NET which already has the correct security access permissions for database access.</span></span> <span data-ttu-id="166ce-131">從 專案 功能表中，選取 新增 ASP.NET 資料夾，然後應用程式\_資料。</span><span class="sxs-lookup"><span data-stu-id="166ce-131">From the Project menu, select Add ASP.NET Folder, then App\_Data.</span></span>

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a><span data-ttu-id="166ce-132">在 web.config 檔案中建立的連接字串</span><span class="sxs-lookup"><span data-stu-id="166ce-132">Creating a Connection String in the web.config file</span></span>

<span data-ttu-id="166ce-133">如此 Entity Framework 可得知如何連接到我們的資料庫，我們將幾行加入至網站的組態檔。</span><span class="sxs-lookup"><span data-stu-id="166ce-133">We will add a few lines to the website's configuration file so that Entity Framework knows how to connect to our database.</span></span> <span data-ttu-id="166ce-134">按兩下專案的根目錄中找到 Web.config 檔案。</span><span class="sxs-lookup"><span data-stu-id="166ce-134">Double-click on the Web.config file located in the root of the project.</span></span>

![](mvc-music-store-part-4/_static/image2.png)

<span data-ttu-id="166ce-135">捲動到這個檔案最下方，新增&lt;connectionStrings&gt;區段正上方的最後一行，如下所示。</span><span class="sxs-lookup"><span data-stu-id="166ce-135">Scroll to the bottom of this file and add a &lt;connectionStrings&gt; section directly above the last line, as shown below.</span></span>

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a><span data-ttu-id="166ce-136">加入內容類別</span><span class="sxs-lookup"><span data-stu-id="166ce-136">Adding a Context Class</span></span>

<span data-ttu-id="166ce-137">以滑鼠右鍵按一下 [模型] 資料夾，並加入新的類別，名為 MusicStoreEntities.cs。</span><span class="sxs-lookup"><span data-stu-id="166ce-137">Right-click the Models folder and add a new class named MusicStoreEntities.cs.</span></span>

![](mvc-music-store-part-4/_static/image3.png)

<span data-ttu-id="166ce-138">這個類別將代表 Entity Framework 資料庫內容中，和將處理我們建立、 讀取、 更新和刪除作業，讓我們。</span><span class="sxs-lookup"><span data-stu-id="166ce-138">This class will represent the Entity Framework database context, and will handle our create, read, update, and delete operations for us.</span></span> <span data-ttu-id="166ce-139">這個類別的程式碼如下所示。</span><span class="sxs-lookup"><span data-stu-id="166ce-139">The code for this class is shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

<span data-ttu-id="166ce-140">就這麼簡單-沒有任何其他組態、 特殊的介面和其他內容。透過擴充 DbContext 基底類別，我們 MusicStoreEntities 類別是能夠處理為我們我們資料庫作業。</span><span class="sxs-lookup"><span data-stu-id="166ce-140">That's it - there's no other configuration, special interfaces, etc. By extending the DbContext base class, our MusicStoreEntities class is able to handle our database operations for us.</span></span> <span data-ttu-id="166ce-141">現在，我們有連結，讓我們加入一些其他屬性給我們的模型類別，以利用我們的資料庫中的一些其他資訊。</span><span class="sxs-lookup"><span data-stu-id="166ce-141">Now that we've got that hooked up, let's add a few more properties to our model classes to take advantage of some of the additional information in our database.</span></span>

### <a name="adding-our-store-catalog-data"></a><span data-ttu-id="166ce-142">加入我們的儲存目錄資料</span><span class="sxs-lookup"><span data-stu-id="166ce-142">Adding our store catalog data</span></span>

<span data-ttu-id="166ce-143">我們會利用一項功能在 Entity Framework 將 「 種子 」 資料加入至新建立的資料庫。</span><span class="sxs-lookup"><span data-stu-id="166ce-143">We will take advantage of a feature in Entity Framework which adds "seed" data to a newly created database.</span></span> <span data-ttu-id="166ce-144">這會預先填入和清單的內容類型、 演出者、 專輯我們存放區的目錄。</span><span class="sxs-lookup"><span data-stu-id="166ce-144">This will pre-populate our store catalog with a list of Genres, Artists, and Albums.</span></span> <span data-ttu-id="166ce-145">-其中包含我們稍早在本教學課程中的站台設計檔案-MvcMusicStore Assets.zip 下載有這種子資料，位於名為程式碼的資料夾中的類別檔案。</span><span class="sxs-lookup"><span data-stu-id="166ce-145">The MvcMusicStore-Assets.zip download - which included our site design files used earlier in this tutorial - has a class file with this seed data, located in a folder named Code.</span></span>

<span data-ttu-id="166ce-146">在程式碼 / Models 資料夾找到 SampleData.cs 檔案，並將其放置到我們的受測專案，[模型] 資料夾，如下所示。</span><span class="sxs-lookup"><span data-stu-id="166ce-146">Within the Code / Models folder, locate the SampleData.cs file and drop it into the Models folder in our project, as shown below.</span></span>

![](mvc-music-store-part-4/_static/image4.png)

<span data-ttu-id="166ce-147">現在我們需要加入一行程式碼，以告訴 SampleData 該類的 Entity Framework。</span><span class="sxs-lookup"><span data-stu-id="166ce-147">Now we need to add one line of code to tell Entity Framework about that SampleData class.</span></span> <span data-ttu-id="166ce-148">按兩下加以開啟，並加入下列行至頂端應用程式專案的根目錄中的 Global.asax 檔案\_Start 方法。</span><span class="sxs-lookup"><span data-stu-id="166ce-148">Double-click on the Global.asax file in the root of the project to open it and add the following line to the top the Application\_Start method.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

<span data-ttu-id="166ce-149">此時，我們已經完成設定 Entity Framework 的受測專案的必要工作。</span><span class="sxs-lookup"><span data-stu-id="166ce-149">At this point, we've completed the work necessary to configure Entity Framework for our project.</span></span>

## <a name="querying-the-database"></a><span data-ttu-id="166ce-150">查詢資料庫</span><span class="sxs-lookup"><span data-stu-id="166ce-150">Querying the Database</span></span>

<span data-ttu-id="166ce-151">現在讓我們來更新我們 StoreController，使而不是使用 「 虛擬資料 」 會改為呼叫以查詢其資訊的所有資料庫。</span><span class="sxs-lookup"><span data-stu-id="166ce-151">Now let's update our StoreController so that instead of using "dummy data" it instead calls into our database to query all of its information.</span></span> <span data-ttu-id="166ce-152">藉由宣告上的欄位，就會開始**StoreController**來保存 MusicStoreEntities 類別，名為 storeDB 的執行個體：</span><span class="sxs-lookup"><span data-stu-id="166ce-152">We'll start by declaring a field on the **StoreController** to hold an instance of the MusicStoreEntities class, named storeDB:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a><span data-ttu-id="166ce-153">更新查詢資料庫存放區索引</span><span class="sxs-lookup"><span data-stu-id="166ce-153">Updating the Store Index to query the database</span></span>

<span data-ttu-id="166ce-154">MusicStoreEntities 類別由 Entity Framework 維護，並公開在資料庫中的每個資料表的集合屬性。</span><span class="sxs-lookup"><span data-stu-id="166ce-154">The MusicStoreEntities class is maintained by the Entity Framework and exposes a collection property for each table in our database.</span></span> <span data-ttu-id="166ce-155">讓我們來更新我們 StoreController 索引動作，以擷取資料庫中的所有內容類型。</span><span class="sxs-lookup"><span data-stu-id="166ce-155">Let's update our StoreController's Index action to retrieve all Genres in our database.</span></span> <span data-ttu-id="166ce-156">先前是由硬式編碼的字串資料。</span><span class="sxs-lookup"><span data-stu-id="166ce-156">Previously we did this by hard-coding string data.</span></span> <span data-ttu-id="166ce-157">現在我們可以改為只使用 Entity Framework 內容 Generes 集合：</span><span class="sxs-lookup"><span data-stu-id="166ce-157">Now we can instead just use the Entity Framework context Generes collection:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

<span data-ttu-id="166ce-158">需要剛好我們檢視的範本，因為我們仍在傳回之前-我們正在只會傳回即時資料從資料庫現在，我們會傳回相同 StoreIndexViewModel 沒有變更。</span><span class="sxs-lookup"><span data-stu-id="166ce-158">No changes need to happen to our View template since we're still returning the same StoreIndexViewModel we returned before - we're just returning live data from our database now.</span></span>

<span data-ttu-id="166ce-159">當我們重新執行專案，並瀏覽"/ 存放區 」 的 URL 時，我們現在會看到資料庫中的所有內容類型清單：</span><span class="sxs-lookup"><span data-stu-id="166ce-159">When we run the project again and visit the "/Store" URL, we'll now see a list of all Genres in our database:</span></span>

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a><span data-ttu-id="166ce-160">更新存放區瀏覽和詳細資料使用即時資料</span><span class="sxs-lookup"><span data-stu-id="166ce-160">Updating Store Browse and Details to use live data</span></span>

<span data-ttu-id="166ce-161">與儲存區/瀏覽？ 內容類型 =*[某些內容類型]*動作方法，我們要依名稱搜尋的內容類型。</span><span class="sxs-lookup"><span data-stu-id="166ce-161">With the /Store/Browse?genre=*[some-genre]* action method, we're searching for a Genre by name.</span></span> <span data-ttu-id="166ce-162">我們只預期有一個結果，因為我們永遠不應該有相同的內容類型名稱的兩個項目，因此我們可以使用。在 LINQ 查詢以取得適當的內容類型物件，像這樣的 Single() 延伸模組 （但不輸入）：</span><span class="sxs-lookup"><span data-stu-id="166ce-162">We only expect one result, since we shouldn't ever have two entries for the same Genre name, and so we can use the .Single() extension in LINQ to query for the appropriate Genre object like this (don't type this yet):</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

<span data-ttu-id="166ce-163">單一方法會使用 Lambda 運算式做為參數，指定我們想要將單一類型物件，例如其名稱符合，我們已經定義的值。</span><span class="sxs-lookup"><span data-stu-id="166ce-163">The Single method takes a Lambda expression as a parameter, which specifies that we want a single Genre object such that its name matches the value we've defined.</span></span> <span data-ttu-id="166ce-164">在上述情況中，我們正在載入的單一類型的物件名稱值相符的 Disco。</span><span class="sxs-lookup"><span data-stu-id="166ce-164">In the case above, we are loading a single Genre object with a Name value matching Disco.</span></span>

<span data-ttu-id="166ce-165">我們將利用 Entity Framework 功能，可讓我們來表示我們想要在擷取的內容類型的物件時所載入以及其他相關的實體。</span><span class="sxs-lookup"><span data-stu-id="166ce-165">We'll take advantage of an Entity Framework feature that allows us to indicate other related entities we want loaded as well when the Genre object is retrieved.</span></span> <span data-ttu-id="166ce-166">這項功能稱為查詢結果成形，並可讓我們來減少的次數，我們需要存取資料庫，以擷取所有需要的資訊。</span><span class="sxs-lookup"><span data-stu-id="166ce-166">This feature is called Query Result Shaping, and enables us to reduce the number of times we need to access the database to retrieve all of the information we need.</span></span> <span data-ttu-id="166ce-167">我們想要預先提取專輯我們擷取時，內容類型，因此我們會將更新我們的查詢來包含從 Genres.Include("Albums") 表示我們想要將相關的相簿。</span><span class="sxs-lookup"><span data-stu-id="166ce-167">We want to pre-fetch the Albums for Genre we retrieve, so we'll update our query to include from Genres.Include("Albums") to indicate that we want related albums as well.</span></span> <span data-ttu-id="166ce-168">這是更有效率，，因為它會擷取我們內容類型和專輯資料都在單一資料庫的要求中。</span><span class="sxs-lookup"><span data-stu-id="166ce-168">This is more efficient, since it will retrieve both our Genre and Album data in a single database request.</span></span>

<span data-ttu-id="166ce-169">開清楚，我們已更新的瀏覽控制器動作外觀如下：</span><span class="sxs-lookup"><span data-stu-id="166ce-169">With the explanations out of the way, here's how our updated Browse controller action looks:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

<span data-ttu-id="166ce-170">我們現在可以更新存放區瀏覽檢視來顯示每個內容類型中可用的專輯。</span><span class="sxs-lookup"><span data-stu-id="166ce-170">We can now update the Store Browse View to display the albums which are available in each Genre.</span></span> <span data-ttu-id="166ce-171">開啟檢視範本 (在中找到 /Views/Store/Browse.cshtml) 並加入項目清單的相簿，如下所示。</span><span class="sxs-lookup"><span data-stu-id="166ce-171">Open the view template (found in /Views/Store/Browse.cshtml) and add a bulleted list of Albums as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

<span data-ttu-id="166ce-172">執行我們的應用程式，並瀏覽至存放區/瀏覽？ 內容類型 = Jazz 顯示我們的結果現在會從資料庫中，我們選取內容類型顯示所有的專輯提取。</span><span class="sxs-lookup"><span data-stu-id="166ce-172">Running our application and browsing to /Store/Browse?genre=Jazz shows that our results are now being pulled from the database, displaying all albums in our selected Genre.</span></span>

![](mvc-music-store-part-4/_static/image2.jpg)

<span data-ttu-id="166ce-173">我們要變更至我們 /Store/詳細資料 / [id] URL，和我們空的資料取代資料庫查詢而載入的相簿的 ID 符合參數的值相同。</span><span class="sxs-lookup"><span data-stu-id="166ce-173">We'll make the same change to our /Store/Details/[id] URL, and replace our dummy data with a database query which loads an Album whose ID matches the parameter value.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

<span data-ttu-id="166ce-174">執行我們的應用程式，並瀏覽至 /Store/Details/1 顯示的現在從資料庫提取我們的結果。</span><span class="sxs-lookup"><span data-stu-id="166ce-174">Running our application and browsing to /Store/Details/1 shows that our results are now being pulled from the database.</span></span>

![](mvc-music-store-part-4/_static/image5.png)

<span data-ttu-id="166ce-175">既然我們存放區詳細資料頁面設定來顯示專輯識別碼相簿，讓我們更新**瀏覽**連結到詳細資料檢視的檢視。</span><span class="sxs-lookup"><span data-stu-id="166ce-175">Now that our Store Details page is set up to display an album by the Album ID, let's update the **Browse** view to link to the Details view.</span></span> <span data-ttu-id="166ce-176">我們將使用 Html.ActionLink，如同我們從存放區索引儲存區瀏覽連結結尾的上一節。</span><span class="sxs-lookup"><span data-stu-id="166ce-176">We will use Html.ActionLink, exactly as we did to link from Store Index to Store Browse at the end of the previous section.</span></span> <span data-ttu-id="166ce-177">下方瀏覽檢視完整的來源會出現。</span><span class="sxs-lookup"><span data-stu-id="166ce-177">The complete source for the Browse view appears below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

<span data-ttu-id="166ce-178">我們現在可以從我們的存放區頁面瀏覽到內容類型 頁面，列出可用的專輯和專輯上按一下我們可以檢視該專輯的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="166ce-178">We're now able to browse from our Store page to a Genre page, which lists the available albums, and by clicking on an album we can view details for that album.</span></span>

![](mvc-music-store-part-4/_static/image6.png)

>[!div class="step-by-step"]
<span data-ttu-id="166ce-179">[上一頁](mvc-music-store-part-3.md)
[下一頁](mvc-music-store-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="166ce-179">[Previous](mvc-music-store-part-3.md)
[Next](mvc-music-store-part-5.md)</span></span>
