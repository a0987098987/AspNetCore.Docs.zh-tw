---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: 第 4 部分： 模型和資料存取 |Microsoft Docs
author: jongalloway
description: 本教學課程系列會詳細說明所有建置 ASP.NET MVC Music 市集範例應用程式所採取的步驟。 第 4 部分涵蓋模型和資料存取。
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: ea8fe623a1b59b80fd7f087036b9ed716eafadbe
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402034"
---
<a name="part-4-models-and-data-access"></a><span data-ttu-id="e3124-104">第 4 部分： 模型和資料存取</span><span class="sxs-lookup"><span data-stu-id="e3124-104">Part 4: Models and Data Access</span></span>
====================
<span data-ttu-id="e3124-105">藉由[Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="e3124-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="e3124-106">MVC Music 市集是介紹，並逐步說明如何使用 ASP.NET MVC 和 Visual Studio 進行 web 開發的教學課程應用程式。</span><span class="sxs-lookup"><span data-stu-id="e3124-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="e3124-107">MVC Music 市集是銷售線上音樂 album 和實作基本的網站管理、 使用者登入時，和 「 購物車 」 功能的輕量級的範例存放區實作。</span><span class="sxs-lookup"><span data-stu-id="e3124-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>
> 
> <span data-ttu-id="e3124-108">本教學課程系列會詳細說明所有建置 ASP.NET MVC Music 市集範例應用程式所採取的步驟。</span><span class="sxs-lookup"><span data-stu-id="e3124-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="e3124-109">第 4 部分涵蓋模型和資料存取。</span><span class="sxs-lookup"><span data-stu-id="e3124-109">Part 4 covers Models and Data Access.</span></span>


<span data-ttu-id="e3124-110">到目前為止，我們已只已傳遞 「 虛擬資料 」 從我們的控制站到我們的檢視範本。</span><span class="sxs-lookup"><span data-stu-id="e3124-110">So far, we've just been passing "dummy data" from our Controllers to our View templates.</span></span> <span data-ttu-id="e3124-111">現在我們已經準備好要連結實際的資料庫。</span><span class="sxs-lookup"><span data-stu-id="e3124-111">Now we're ready to hook up a real database.</span></span> <span data-ttu-id="e3124-112">在本教學課程中我們要探討如何使用 SQL Server Compact Edition （通常稱為 SQL CE） 做為我們的資料庫引擎。</span><span class="sxs-lookup"><span data-stu-id="e3124-112">In this tutorial we'll be covering how to use SQL Server Compact Edition (often called SQL CE) as our database engine.</span></span> <span data-ttu-id="e3124-113">SQL CE 是免費的內嵌，檔案型的資料庫，並不需要任何安裝或組態，可以很方便的本機開發。</span><span class="sxs-lookup"><span data-stu-id="e3124-113">SQL CE is a free, embedded, file based database that doesn't require any installation or configuration, which makes it really convenient for local development.</span></span>

## <a name="database-access-with-entity-framework-code-first"></a><span data-ttu-id="e3124-114">資料庫存取與 Entity Framework Code First</span><span class="sxs-lookup"><span data-stu-id="e3124-114">Database access with Entity Framework Code-First</span></span>

<span data-ttu-id="e3124-115">我們將使用隨附於 ASP.NET MVC 3 專案，以查詢和更新資料庫中的 Entity Framework (EF) 支援。</span><span class="sxs-lookup"><span data-stu-id="e3124-115">We'll use the Entity Framework (EF) support that is included in ASP.NET MVC 3 projects to query and update the database.</span></span> <span data-ttu-id="e3124-116">EF 是彈性物件關聯式對應 (ORM) 資料的 API，可讓開發人員以物件導向方式儲存在資料庫中的查詢及更新資料。</span><span class="sxs-lookup"><span data-stu-id="e3124-116">EF is a flexible object relational mapping (ORM) data API that enables developers to query and update data stored in a database in an object-oriented way.</span></span>

<span data-ttu-id="e3124-117">Entity Framework 第 4 版支援稱為 code first 開發架構。</span><span class="sxs-lookup"><span data-stu-id="e3124-117">Entity Framework version 4 supports a development paradigm called code-first.</span></span> <span data-ttu-id="e3124-118">Code first 可讓您撰寫簡單的類別 (也稱為 POCO 「 單純 」 的 CLR 物件)，來建立模型物件，並甚至可以建立您的類別從即時資料庫。</span><span class="sxs-lookup"><span data-stu-id="e3124-118">Code-first allows you to create model object by writing simple classes (also known as POCO from "plain-old" CLR objects), and can even create the database on the fly from your classes.</span></span>

### <a name="changes-to-our-model-classes"></a><span data-ttu-id="e3124-119">我們的模型類別變更</span><span class="sxs-lookup"><span data-stu-id="e3124-119">Changes to our Model Classes</span></span>

<span data-ttu-id="e3124-120">我們將在本教學課程利用 Entity Framework 中的資料庫建立功能。</span><span class="sxs-lookup"><span data-stu-id="e3124-120">We will be leveraging the database creation feature in Entity Framework in this tutorial.</span></span> <span data-ttu-id="e3124-121">這樣做之前，不過，讓我們進行了一些微幅變更給我們的模型類別，以增益集我們將在稍後使用的一些事項。</span><span class="sxs-lookup"><span data-stu-id="e3124-121">Before we do that, though, let's make a few minor changes to our model classes to add in some things we'll be using later on.</span></span>

#### <a name="adding-the-artist-model-classes"></a><span data-ttu-id="e3124-122">加入演出者模型類別</span><span class="sxs-lookup"><span data-stu-id="e3124-122">Adding the Artist Model Classes</span></span>

<span data-ttu-id="e3124-123">我們的專輯將會與演出者、 相關聯，因此我們將新增一個簡單的模型類別來描述演出者。</span><span class="sxs-lookup"><span data-stu-id="e3124-123">Our Albums will be associated with Artists, so we'll add a simple model class to describe an Artist.</span></span> <span data-ttu-id="e3124-124">加入名為 Artist.cs 使用程式碼如下所示的 [模型] 資料夾中的新類別。</span><span class="sxs-lookup"><span data-stu-id="e3124-124">Add a new class to the Models folder named Artist.cs using the code shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a><span data-ttu-id="e3124-125">更新我們的模型類別</span><span class="sxs-lookup"><span data-stu-id="e3124-125">Updating our Model Classes</span></span>

<span data-ttu-id="e3124-126">更新專輯類別，如下所示。</span><span class="sxs-lookup"><span data-stu-id="e3124-126">Update the Album class as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

<span data-ttu-id="e3124-127">接下來，進行下列更新內容類型的類別。</span><span class="sxs-lookup"><span data-stu-id="e3124-127">Next, make the following updates to the Genre class.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-appdata-folder"></a><span data-ttu-id="e3124-128">新增應用程式\_Data 資料夾</span><span class="sxs-lookup"><span data-stu-id="e3124-128">Adding the App\_Data folder</span></span>

<span data-ttu-id="e3124-129">我們將新增應用程式\_至我們的專案，以保留 SQL Server Express 資料庫檔案的資料目錄。</span><span class="sxs-lookup"><span data-stu-id="e3124-129">We'll add an App\_Data directory to our project to hold our SQL Server Express database files.</span></span> <span data-ttu-id="e3124-130">應用程式\_資料是一個特殊的目錄，在 ASP.NET 中已經有資料庫存取權的正確的安全性存取權限。</span><span class="sxs-lookup"><span data-stu-id="e3124-130">App\_Data is a special directory in ASP.NET which already has the correct security access permissions for database access.</span></span> <span data-ttu-id="e3124-131">從 專案 功能表中，選取 新增 ASP.NET 資料夾，然後應用程式\_資料。</span><span class="sxs-lookup"><span data-stu-id="e3124-131">From the Project menu, select Add ASP.NET Folder, then App\_Data.</span></span>

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a><span data-ttu-id="e3124-132">在 web.config 檔案中建立的連接字串</span><span class="sxs-lookup"><span data-stu-id="e3124-132">Creating a Connection String in the web.config file</span></span>

<span data-ttu-id="e3124-133">我們將網站的組態檔加入幾行，好讓 Entity Framework 知道如何連線至我們的資料庫。</span><span class="sxs-lookup"><span data-stu-id="e3124-133">We will add a few lines to the website's configuration file so that Entity Framework knows how to connect to our database.</span></span> <span data-ttu-id="e3124-134">按兩下專案的根目錄中找到 Web.config 檔案。</span><span class="sxs-lookup"><span data-stu-id="e3124-134">Double-click on the Web.config file located in the root of the project.</span></span>

![](mvc-music-store-part-4/_static/image2.png)

<span data-ttu-id="e3124-135">捲動至底部，此檔案，並新增&lt;connectionStrings&gt;區段正上方，而最後一行，如下所示。</span><span class="sxs-lookup"><span data-stu-id="e3124-135">Scroll to the bottom of this file and add a &lt;connectionStrings&gt; section directly above the last line, as shown below.</span></span>

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a><span data-ttu-id="e3124-136">新增的內容類別</span><span class="sxs-lookup"><span data-stu-id="e3124-136">Adding a Context Class</span></span>

<span data-ttu-id="e3124-137">以滑鼠右鍵按一下 [模型] 資料夾，並加入新的類別，名為 MusicStoreEntities.cs。</span><span class="sxs-lookup"><span data-stu-id="e3124-137">Right-click the Models folder and add a new class named MusicStoreEntities.cs.</span></span>

![](mvc-music-store-part-4/_static/image3.png)

<span data-ttu-id="e3124-138">這個類別會表示 Entity Framework 資料庫內容中，和會處理我們建立、 讀取、 更新和刪除作業，讓我們。</span><span class="sxs-lookup"><span data-stu-id="e3124-138">This class will represent the Entity Framework database context, and will handle our create, read, update, and delete operations for us.</span></span> <span data-ttu-id="e3124-139">此類別的程式碼如下所示。</span><span class="sxs-lookup"><span data-stu-id="e3124-139">The code for this class is shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

<span data-ttu-id="e3124-140">就這麼簡單-沒有任何其他組態、 特殊的介面等。透過擴充 DbContext 基底類別，我們 MusicStoreEntities 類別是能夠處理我們對我們的資料庫作業。</span><span class="sxs-lookup"><span data-stu-id="e3124-140">That's it - there's no other configuration, special interfaces, etc. By extending the DbContext base class, our MusicStoreEntities class is able to handle our database operations for us.</span></span> <span data-ttu-id="e3124-141">現在，我們有連結，讓我們新增更多屬性給我們的模型類別，以利用我們的資料庫中的一些額外的資訊。</span><span class="sxs-lookup"><span data-stu-id="e3124-141">Now that we've got that hooked up, let's add a few more properties to our model classes to take advantage of some of the additional information in our database.</span></span>

### <a name="adding-our-store-catalog-data"></a><span data-ttu-id="e3124-142">加入我們的儲存目錄資料</span><span class="sxs-lookup"><span data-stu-id="e3124-142">Adding our store catalog data</span></span>

<span data-ttu-id="e3124-143">我們會利用一項功能在 Entity Framework 將 「 種子 」 資料加入至新建立的資料庫。</span><span class="sxs-lookup"><span data-stu-id="e3124-143">We will take advantage of a feature in Entity Framework which adds "seed" data to a newly created database.</span></span> <span data-ttu-id="e3124-144">這會預先填入我們的存放區目錄的內容類型、 演出者，與專輯清單。</span><span class="sxs-lookup"><span data-stu-id="e3124-144">This will pre-populate our store catalog with a list of Genres, Artists, and Albums.</span></span> <span data-ttu-id="e3124-145">-這包含我們稍早在本教學課程中使用的站台設計檔案-MvcMusicStore Assets.zip 下載具有與這個種子資料，位於名為程式碼的資料夾中的類別檔案。</span><span class="sxs-lookup"><span data-stu-id="e3124-145">The MvcMusicStore-Assets.zip download - which included our site design files used earlier in this tutorial - has a class file with this seed data, located in a folder named Code.</span></span>

<span data-ttu-id="e3124-146">在程式碼 / Models 資料夾中，找出 SampleData.cs 檔案，並將它放在專案中，於 Models 資料夾，如下所示。</span><span class="sxs-lookup"><span data-stu-id="e3124-146">Within the Code / Models folder, locate the SampleData.cs file and drop it into the Models folder in our project, as shown below.</span></span>

![](mvc-music-store-part-4/_static/image4.png)

<span data-ttu-id="e3124-147">現在，我們需要新增一行告訴該 SampleData 類別中的 Entity Framework 的程式碼。</span><span class="sxs-lookup"><span data-stu-id="e3124-147">Now we need to add one line of code to tell Entity Framework about that SampleData class.</span></span> <span data-ttu-id="e3124-148">按兩下要開啟它，然後加入下面這行至頂端應用程式的專案根目錄中的 Global.asax 檔案\_Start 方法。</span><span class="sxs-lookup"><span data-stu-id="e3124-148">Double-click on the Global.asax file in the root of the project to open it and add the following line to the top the Application\_Start method.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

<span data-ttu-id="e3124-149">到目前為止，我們已經完成我們的專案設定 Entity Framework 所需的工作。</span><span class="sxs-lookup"><span data-stu-id="e3124-149">At this point, we've completed the work necessary to configure Entity Framework for our project.</span></span>

## <a name="querying-the-database"></a><span data-ttu-id="e3124-150">查詢資料庫</span><span class="sxs-lookup"><span data-stu-id="e3124-150">Querying the Database</span></span>

<span data-ttu-id="e3124-151">現在讓我們更新我們 StoreController，使而不是使用 「 虛擬資料 」 會改為呼叫來查詢其資訊的所有資料庫。</span><span class="sxs-lookup"><span data-stu-id="e3124-151">Now let's update our StoreController so that instead of using "dummy data" it instead calls into our database to query all of its information.</span></span> <span data-ttu-id="e3124-152">我們一開始先宣告上的欄位**StoreController**來保存 MusicStoreEntities 類別，名為 storeDB 的執行個體：</span><span class="sxs-lookup"><span data-stu-id="e3124-152">We'll start by declaring a field on the **StoreController** to hold an instance of the MusicStoreEntities class, named storeDB:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a><span data-ttu-id="e3124-153">更新存放區索引來查詢資料庫</span><span class="sxs-lookup"><span data-stu-id="e3124-153">Updating the Store Index to query the database</span></span>

<span data-ttu-id="e3124-154">MusicStoreEntities 類別由 Entity Framework 所維護，並公開我們的資料庫中每個資料表的集合屬性。</span><span class="sxs-lookup"><span data-stu-id="e3124-154">The MusicStoreEntities class is maintained by the Entity Framework and exposes a collection property for each table in our database.</span></span> <span data-ttu-id="e3124-155">讓我們更新我們 StoreController 索引動作，以擷取資料庫中的所有內容類型。</span><span class="sxs-lookup"><span data-stu-id="e3124-155">Let's update our StoreController's Index action to retrieve all Genres in our database.</span></span> <span data-ttu-id="e3124-156">先前我們採用的方法來硬式編碼的字串資料。</span><span class="sxs-lookup"><span data-stu-id="e3124-156">Previously we did this by hard-coding string data.</span></span> <span data-ttu-id="e3124-157">現在我們可以改為只使用 Entity Framework 內容 Generes 集合：</span><span class="sxs-lookup"><span data-stu-id="e3124-157">Now we can instead just use the Entity Framework context Generes collection:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

<span data-ttu-id="e3124-158">不需要剛好我們檢視範本，因為我們仍然傳回相同的 StoreIndexViewModel 之前-我們只傳回即時資料從資料庫現在，我們會傳回任何變更。</span><span class="sxs-lookup"><span data-stu-id="e3124-158">No changes need to happen to our View template since we're still returning the same StoreIndexViewModel we returned before - we're just returning live data from our database now.</span></span>

<span data-ttu-id="e3124-159">當我們再次執行專案，並瀏覽"/ 儲存"URL 時，我們現在會在資料庫中看到所有內容類型的清單：</span><span class="sxs-lookup"><span data-stu-id="e3124-159">When we run the project again and visit the "/Store" URL, we'll now see a list of all Genres in our database:</span></span>

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a><span data-ttu-id="e3124-160">更新存放區瀏覽和詳細資料來使用即時資料</span><span class="sxs-lookup"><span data-stu-id="e3124-160">Updating Store Browse and Details to use live data</span></span>

<span data-ttu-id="e3124-161">使用存放區/瀏覽？ 內容類型 =*[部分內容類型]* 動作方法中，我們要依名稱搜尋的內容類型。</span><span class="sxs-lookup"><span data-stu-id="e3124-161">With the /Store/Browse?genre=*[some-genre]* action method, we're searching for a Genre by name.</span></span> <span data-ttu-id="e3124-162">我們只預期結果，由於我們以往不應該有兩個項目相同的內容類型名稱，因此我們可以使用。在 LINQ 查詢適當的內容類型物件，就像這樣的 single （） 延伸模組 （不輸入這尚未）：</span><span class="sxs-lookup"><span data-stu-id="e3124-162">We only expect one result, since we shouldn't ever have two entries for the same Genre name, and so we can use the .Single() extension in LINQ to query for the appropriate Genre object like this (don't type this yet):</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

<span data-ttu-id="e3124-163">單一的方法是採用 Lambda 運算式做為參數，指定我們想要將單一的內容類型物件，其名稱符合已定義的值。</span><span class="sxs-lookup"><span data-stu-id="e3124-163">The Single method takes a Lambda expression as a parameter, which specifies that we want a single Genre object such that its name matches the value we've defined.</span></span> <span data-ttu-id="e3124-164">在上述案例中，我們載入單一內容類型的物件名稱值相符的 Disco。</span><span class="sxs-lookup"><span data-stu-id="e3124-164">In the case above, we are loading a single Genre object with a Name value matching Disco.</span></span>

<span data-ttu-id="e3124-165">我們將利用 Entity Framework 功能，可讓我們指出我們想要在擷取的內容類型的物件時所一併載入其他相關的實體。</span><span class="sxs-lookup"><span data-stu-id="e3124-165">We'll take advantage of an Entity Framework feature that allows us to indicate other related entities we want loaded as well when the Genre object is retrieved.</span></span> <span data-ttu-id="e3124-166">這項功能稱為查詢結果成形，並可讓我們縮短的次數，我們需要存取資料庫，以擷取所有我們所需的資訊。</span><span class="sxs-lookup"><span data-stu-id="e3124-166">This feature is called Query Result Shaping, and enables us to reduce the number of times we need to access the database to retrieve all of the information we need.</span></span> <span data-ttu-id="e3124-167">我們想要預先提取我們擷取時，內容類型的專輯，因此我們將會更新查詢，以便包含從 Genres.Include("Albums") 表示我們想要將相關的相簿。</span><span class="sxs-lookup"><span data-stu-id="e3124-167">We want to pre-fetch the Albums for Genre we retrieve, so we'll update our query to include from Genres.Include("Albums") to indicate that we want related albums as well.</span></span> <span data-ttu-id="e3124-168">這是更有效率，，因為它會擷取我們內容類型和專輯資料都在單一資料庫的要求中。</span><span class="sxs-lookup"><span data-stu-id="e3124-168">This is more efficient, since it will retrieve both our Genre and Album data in a single database request.</span></span>

<span data-ttu-id="e3124-169">簡潔的說明，我們已更新的瀏覽控制器動作外觀如下：</span><span class="sxs-lookup"><span data-stu-id="e3124-169">With the explanations out of the way, here's how our updated Browse controller action looks:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

<span data-ttu-id="e3124-170">我們現在可以更新存放區瀏覽 檢視來顯示每個內容類型中，您可以使用哪些相簿。</span><span class="sxs-lookup"><span data-stu-id="e3124-170">We can now update the Store Browse View to display the albums which are available in each Genre.</span></span> <span data-ttu-id="e3124-171">開啟檢視範本 (在中找到 /Views/Store/Browse.cshtml) 並新增專輯的項目符號清單，如下所示。</span><span class="sxs-lookup"><span data-stu-id="e3124-171">Open the view template (found in /Views/Store/Browse.cshtml) and add a bulleted list of Albums as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

<span data-ttu-id="e3124-172">執行我們的應用程式，並瀏覽至存放區/瀏覽？ 內容類型 = Jazz 顯示我們的結果現在正在從資料庫中，我們選取的內容類型中顯示所有的專輯提取。</span><span class="sxs-lookup"><span data-stu-id="e3124-172">Running our application and browsing to /Store/Browse?genre=Jazz shows that our results are now being pulled from the database, displaying all albums in our selected Genre.</span></span>

![](mvc-music-store-part-4/_static/image2.jpg)

<span data-ttu-id="e3124-173">我們要讓相同的變更為我們 /Store/詳細資料 / [id] URL，和我們的虛擬資料取代成資料庫查詢以載入的 Album 的 ID 符合參數的值。</span><span class="sxs-lookup"><span data-stu-id="e3124-173">We'll make the same change to our /Store/Details/[id] URL, and replace our dummy data with a database query which loads an Album whose ID matches the parameter value.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

<span data-ttu-id="e3124-174">執行我們的應用程式，並瀏覽至 /Store/Details/1 示範，我們的結果現在正在從資料庫提取。</span><span class="sxs-lookup"><span data-stu-id="e3124-174">Running our application and browsing to /Store/Details/1 shows that our results are now being pulled from the database.</span></span>

![](mvc-music-store-part-4/_static/image5.png)

<span data-ttu-id="e3124-175">現在，我們的存放區詳細資料頁面設定 album 顯示專輯識別碼，讓我們更新**瀏覽**連結到詳細資料檢視的檢視。</span><span class="sxs-lookup"><span data-stu-id="e3124-175">Now that our Store Details page is set up to display an album by the Album ID, let's update the **Browse** view to link to the Details view.</span></span> <span data-ttu-id="e3124-176">完全一樣從存放區索引存放區瀏覽連結結尾的上一節，我們將使用 Html.ActionLink。</span><span class="sxs-lookup"><span data-stu-id="e3124-176">We will use Html.ActionLink, exactly as we did to link from Store Index to Store Browse at the end of the previous section.</span></span> <span data-ttu-id="e3124-177">瀏覽 檢視的完整來源會出現下方。</span><span class="sxs-lookup"><span data-stu-id="e3124-177">The complete source for the Browse view appears below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

<span data-ttu-id="e3124-178">現在我們就可以從我們的存放區頁面瀏覽到內容類型 頁面，列出可用的專輯和專輯上即可，我們可以檢視該專輯的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="e3124-178">We're now able to browse from our Store page to a Genre page, which lists the available albums, and by clicking on an album we can view details for that album.</span></span>

![](mvc-music-store-part-4/_static/image6.png)

> [!div class="step-by-step"]
> <span data-ttu-id="e3124-179">[上一頁](mvc-music-store-part-3.md)
> [下一頁](mvc-music-store-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="e3124-179">[Previous](mvc-music-store-part-3.md)
[Next](mvc-music-store-part-5.md)</span></span>
