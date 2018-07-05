---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
title: 新增 Create 方法和 Create 檢視 |Microsoft Docs
author: shanselman
description: 這是初學者教學課程中，將會介紹 ASP.NET MVC 的基本概念。 建立簡單的 web 應用程式，從資料庫讀取與寫入。
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: a3a90963-0286-4fa0-9b3d-c230cc18b0a3
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
msc.type: authoredcontent
ms.openlocfilehash: 976df78ea22c30c094f70a57792d287f15d2c62d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400904"
---
<a name="adding-a-create-method-and-create-view"></a><span data-ttu-id="0b54d-104">新增 Create 方法和 Create 檢視</span><span class="sxs-lookup"><span data-stu-id="0b54d-104">Adding a Create Method and Create View</span></span>
====================
<span data-ttu-id="0b54d-105">藉由[Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="0b54d-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="0b54d-106">這是初學者教學課程中，將會介紹 ASP.NET MVC 的基本概念。</span><span class="sxs-lookup"><span data-stu-id="0b54d-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="0b54d-107">您將建立簡單 web 應用程式，從資料庫讀取與寫入。</span><span class="sxs-lookup"><span data-stu-id="0b54d-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="0b54d-108">請瀏覽[ASP.NET MVC 學習中心](../../../index.md)來尋找其他 ASP.NET MVC 教學課程和範例。</span><span class="sxs-lookup"><span data-stu-id="0b54d-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="0b54d-109">在這一節我們即將實作，讓使用者可以在資料庫中建立新的影片所需的支援。</span><span class="sxs-lookup"><span data-stu-id="0b54d-109">In this section we are going to implement the support necessary to enable users to create new movies in our database.</span></span> <span data-ttu-id="0b54d-110">我們會實作電影/建立 URL 動作。</span><span class="sxs-lookup"><span data-stu-id="0b54d-110">We'll do this by implementing the /Movies/Create URL action.</span></span>

<span data-ttu-id="0b54d-111">實作電影/建立 URL 是兩個步驟的程序。</span><span class="sxs-lookup"><span data-stu-id="0b54d-111">Implementing the /Movies/Create URL is a two step process.</span></span> <span data-ttu-id="0b54d-112">當使用者第一次造訪電影/建立 URL 我們想要顯示這些輸入新的影片可填寫 HTML 表單。</span><span class="sxs-lookup"><span data-stu-id="0b54d-112">When a user first visits the /Movies/Create URL we want to show them an HTML form that they can fill out to enter a new movie.</span></span> <span data-ttu-id="0b54d-113">然後，當使用者提交表單和資料回傳至伺服器的文章，我們想要擷取的已張貼的內容，並將它儲存到資料庫。</span><span class="sxs-lookup"><span data-stu-id="0b54d-113">Then, when the user submits the form and posts the data back to the server, we want to retrieve the posted contents and save it into our database.</span></span>

<span data-ttu-id="0b54d-114">我們將我們 MoviesController 類別內實作這兩個 create （） 方法內的兩個步驟。</span><span class="sxs-lookup"><span data-stu-id="0b54d-114">We'll implement these two steps within two Create() methods within our MoviesController class.</span></span> <span data-ttu-id="0b54d-115">其中一種方法將會顯示&lt;表單&gt;使用者應該填寫以建立新的電影。</span><span class="sxs-lookup"><span data-stu-id="0b54d-115">One method will show the &lt;form&gt; that the user should fill out to create a new movie.</span></span> <span data-ttu-id="0b54d-116">第二個方法將處理處理張貼的資料，當使用者提交&lt;表單&gt;回傳至伺服器，並儲存在資料庫裡面的新影片。</span><span class="sxs-lookup"><span data-stu-id="0b54d-116">The second method will handle processing the posted data when the user submits the &lt;form&gt; back to the server, and save a new Movie within our database.</span></span>

<span data-ttu-id="0b54d-117">以下是程式碼我們會將它新增至我們的 MoviesController 類別，以實作此：</span><span class="sxs-lookup"><span data-stu-id="0b54d-117">Below is the code we'll add to our MoviesController class to implement this:</span></span>

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample1.cs)]

<span data-ttu-id="0b54d-118">上述程式碼包含所有我們需要我們控制器內的程式碼。</span><span class="sxs-lookup"><span data-stu-id="0b54d-118">The above code contains all of the code that we'll need within our Controller.</span></span>

<span data-ttu-id="0b54d-119">讓我們現在實作建立檢視範本，我們將使用它來向使用者顯示的表單。</span><span class="sxs-lookup"><span data-stu-id="0b54d-119">Let's now implement the Create View template that we'll use to display a form to the user.</span></span> <span data-ttu-id="0b54d-120">我們將第一個建立方法中以滑鼠右鍵按一下，然後選取 [新增檢視] 來建立我們的電影表單的檢視範本。</span><span class="sxs-lookup"><span data-stu-id="0b54d-120">We'll right click in the first Create method and select "Add View" to create the view template for our Movie form.</span></span>

<span data-ttu-id="0b54d-121">我們將選取，我們會將檢視範本 「 電影 」 作為檢視的資料類別，表示我們想要 「 建立 」 的 「 建立 」 範本。</span><span class="sxs-lookup"><span data-stu-id="0b54d-121">We'll select that we are going to pass the view template a "Movie" as its view data class, and indicate that we want to "scaffold" a "Create" template.</span></span>

<span data-ttu-id="0b54d-122">[![新增檢視](getting-started-with-mvc-part6/_static/image2.png)](getting-started-with-mvc-part6/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0b54d-122">[![Add View](getting-started-with-mvc-part6/_static/image2.png)](getting-started-with-mvc-part6/_static/image1.png)</span></span>

<span data-ttu-id="0b54d-123">按一下 [新增] 按鈕之後，就會為您建立 \Movies\Create.aspx 檢視範本。</span><span class="sxs-lookup"><span data-stu-id="0b54d-123">After you click the Add button, \Movies\Create.aspx View template will be created for you.</span></span> <span data-ttu-id="0b54d-124">因為我們從 [檢視內容] 下拉式清單中選取 [建立]，[新增檢視] 對話方塊自動 「 建構 」 某些預設內容為我們。</span><span class="sxs-lookup"><span data-stu-id="0b54d-124">Because we selected "Create" from the "view content" dropdown, the Add View dialog automatically "scaffolded" some default content for us.</span></span> <span data-ttu-id="0b54d-125">Scaffolding 建立 HTML&lt;表單&gt;、 位置，以讓驗證錯誤訊息前往的而且每個屬性的類別 scaffolding 知道電影相關，因為建立標籤和欄位。</span><span class="sxs-lookup"><span data-stu-id="0b54d-125">The scaffolding created an HTML &lt;form&gt;, a place for validation error messages to go, and since scaffolding knows about Movies, it created Label and Fields for each property of our class.</span></span>

[!code-aspx[Main](getting-started-with-mvc-part6/samples/sample2.aspx)]

<span data-ttu-id="0b54d-126">因為我們的資料庫會自動提供影片的識別碼，讓我們移除這些欄位，參考模型。從我們建立的檢視識別碼。</span><span class="sxs-lookup"><span data-stu-id="0b54d-126">Since our database automatically gives a Movie an ID, let's remove those fields that reference model.Id from our Create View.</span></span> <span data-ttu-id="0b54d-127">移除之後 7 線條&lt;圖例&gt;欄位&lt;/legend&gt; ，它們會顯示 [識別碼] 欄位，我們不想。</span><span class="sxs-lookup"><span data-stu-id="0b54d-127">Remove the 7 lines after &lt;legend&gt;Fields&lt;/legend&gt; as they show the ID field that we don't want.</span></span>

<span data-ttu-id="0b54d-128">讓我們現在建立新的電影，並將它新增至資料庫。</span><span class="sxs-lookup"><span data-stu-id="0b54d-128">Let's now create a new movie and add it to the database.</span></span> <span data-ttu-id="0b54d-129">我們會執行一次的應用程式來執行這項操作，請瀏覽 「 / 電影 」 URL，然後按一下 [建立] 連結來新增新電影。</span><span class="sxs-lookup"><span data-stu-id="0b54d-129">We'll do this by running the application again and visit the "/Movies" URL and click the "Create" link to add a new Movie.</span></span>

<span data-ttu-id="0b54d-130">[![建立-Windows Internet Explorer](getting-started-with-mvc-part6/_static/image4.png)](getting-started-with-mvc-part6/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="0b54d-130">[![Create - Windows Internet Explorer](getting-started-with-mvc-part6/_static/image4.png)](getting-started-with-mvc-part6/_static/image3.png)</span></span>

<span data-ttu-id="0b54d-131">當我們按一下 [建立] 按鈕時，我們會公佈回 （透過 HTTP POST) /Movies/Create 方法我們剛建立這個表單上的資料。</span><span class="sxs-lookup"><span data-stu-id="0b54d-131">When we click the Create button, we'll be posting back (via HTTP POST) the data on this form to the /Movies/Create method that we just created.</span></span> <span data-ttu-id="0b54d-132">如同時系統會自動採取 「 numTimes"和"name"參數，在 url，和它們對應到稍早在方法上的參數，系統會自動從某篇文章中取得表單欄位，並將它們對應至物件。</span><span class="sxs-lookup"><span data-stu-id="0b54d-132">Just like when the system automatically took the "numTimes" and "name " parameter out of the URL and mapped them to parameters on a method earlier, the system will automatically take the Form Fields from a POST and map them to an object.</span></span> <span data-ttu-id="0b54d-133">在此情況下，例如"ReleaseDate"和"Title"的 HTML 中的欄位值會自動放入電影的新執行個體的正確的屬性。</span><span class="sxs-lookup"><span data-stu-id="0b54d-133">In this case, values from fields in HTML like "ReleaseDate" and "Title" will automatically be put into the correct properties of a new instance of a Movie.</span></span>

<span data-ttu-id="0b54d-134">讓我們看看第二個 Create 方法從我們 MoviesController 一次。</span><span class="sxs-lookup"><span data-stu-id="0b54d-134">Let's look at the second Create method from our MoviesController again.</span></span> <span data-ttu-id="0b54d-135">請注意它如何採用 「 電影 」 物件做為引數：</span><span class="sxs-lookup"><span data-stu-id="0b54d-135">Notice how it takes a "Movie" object as an argument:</span></span>

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample3.cs)]

<span data-ttu-id="0b54d-136">此影片物件接著傳遞至我們建立的動作方法的 [HttpPost] 版本，我們並儲存在資料庫中再被使用者重新導向回到 index （） 動作方法，這個方法會將電影清單中顯示已儲存的結果：</span><span class="sxs-lookup"><span data-stu-id="0b54d-136">This Movie object was then passed to the [HttpPost] version of our Create action method, and we saved it in the database and then redirected the user back to the Index() action method which will show the saved result in the movie list:</span></span>

<span data-ttu-id="0b54d-137">[![電影清單-Windows Internet Explorer](getting-started-with-mvc-part6/_static/image6.png)](getting-started-with-mvc-part6/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="0b54d-137">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part6/_static/image6.png)](getting-started-with-mvc-part6/_static/image5.png)</span></span>

<span data-ttu-id="0b54d-138">如果我們的影片都正確無誤，不過，而且資料庫不允許我們沒有標題儲存影片，我們不檢查。</span><span class="sxs-lookup"><span data-stu-id="0b54d-138">We aren't checking if our movies are correct, though, and the database won't allow us to save a movie with no Title.</span></span> <span data-ttu-id="0b54d-139">如果我們還可以告訴使用者的資料庫之前擲回錯誤，那就天下太平了。</span><span class="sxs-lookup"><span data-stu-id="0b54d-139">It'd be nice if we could tell the user that before the database threw an error.</span></span> <span data-ttu-id="0b54d-140">我們藉由將我們的應用程式的驗證支援就此下一步。</span><span class="sxs-lookup"><span data-stu-id="0b54d-140">We'll do this next by adding validation support to our application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0b54d-141">[上一頁](getting-started-with-mvc-part5.md)
> [下一頁](getting-started-with-mvc-part7.md)</span><span class="sxs-lookup"><span data-stu-id="0b54d-141">[Previous](getting-started-with-mvc-part5.md)
[Next](getting-started-with-mvc-part7.md)</span></span>
