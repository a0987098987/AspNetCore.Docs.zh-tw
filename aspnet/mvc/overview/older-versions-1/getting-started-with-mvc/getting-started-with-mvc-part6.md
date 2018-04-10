---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
title: 加入建立方法，並建立檢視 |Microsoft 文件
author: shanselman
description: 這是初學者教學課程介紹基本概念的 ASP.NET MVC。 建立簡單的 web 應用程式可讀取和寫入資料庫中。
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: a3a90963-0286-4fa0-9b3d-c230cc18b0a3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
msc.type: authoredcontent
ms.openlocfilehash: 48e656a0c394b9db5baaec9c557ec38c4020d41b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/10/2018
---
<a name="adding-a-create-method-and-create-view"></a><span data-ttu-id="c32f2-104">加入建立方法，並建立檢視</span><span class="sxs-lookup"><span data-stu-id="c32f2-104">Adding a Create Method and Create View</span></span>
====================
<span data-ttu-id="c32f2-105">由[Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="c32f2-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="c32f2-106">這是初學者教學課程介紹基本概念的 ASP.NET MVC。</span><span class="sxs-lookup"><span data-stu-id="c32f2-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="c32f2-107">您將建立簡單的 web 應用程式可讀取和寫入資料庫中。</span><span class="sxs-lookup"><span data-stu-id="c32f2-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="c32f2-108">請瀏覽[ASP.NET MVC 學習中心](../../../index.md)教學課程和範例，請尋找其他 ASP.NET MVC。</span><span class="sxs-lookup"><span data-stu-id="c32f2-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="c32f2-109">本節中，我們即將實作需要讓使用者在資料庫中建立新的電影的支援。</span><span class="sxs-lookup"><span data-stu-id="c32f2-109">In this section we are going to implement the support necessary to enable users to create new movies in our database.</span></span> <span data-ttu-id="c32f2-110">藉由實作電影/建立 URL 動作，我們會執行這項操作。</span><span class="sxs-lookup"><span data-stu-id="c32f2-110">We'll do this by implementing the /Movies/Create URL action.</span></span>

<span data-ttu-id="c32f2-111">實作電影/建立 URL 是兩個步驟的程序。</span><span class="sxs-lookup"><span data-stu-id="c32f2-111">Implementing the /Movies/Create URL is a two step process.</span></span> <span data-ttu-id="c32f2-112">當使用者第一次造訪電影/建立 URL 我們想要顯示它們可以填寫輸入新的電影的 HTML 表單。</span><span class="sxs-lookup"><span data-stu-id="c32f2-112">When a user first visits the /Movies/Create URL we want to show them an HTML form that they can fill out to enter a new movie.</span></span> <span data-ttu-id="c32f2-113">然後，當使用者提交表單和文章回傳至伺服器的資料，我們想要擷取的已張貼的內容，並將它儲存到資料庫。</span><span class="sxs-lookup"><span data-stu-id="c32f2-113">Then, when the user submits the form and posts the data back to the server, we want to retrieve the posted contents and save it into our database.</span></span>

<span data-ttu-id="c32f2-114">我們將我們 MoviesController 類別內實作這兩個 create （） 方法內的兩個步驟。</span><span class="sxs-lookup"><span data-stu-id="c32f2-114">We'll implement these two steps within two Create() methods within our MoviesController class.</span></span> <span data-ttu-id="c32f2-115">一種方法將會顯示&lt;表單&gt;使用者應該填寫建立新的電影。</span><span class="sxs-lookup"><span data-stu-id="c32f2-115">One method will show the &lt;form&gt; that the user should fill out to create a new movie.</span></span> <span data-ttu-id="c32f2-116">第二種方法將處理處理張貼的資料，當使用者提交&lt;表單&gt;回傳至伺服器，並儲存新的電影中我們的資料庫。</span><span class="sxs-lookup"><span data-stu-id="c32f2-116">The second method will handle processing the posted data when the user submits the &lt;form&gt; back to the server, and save a new Movie within our database.</span></span>

<span data-ttu-id="c32f2-117">以下是程式碼我們會加入可實作此 MoviesController 類別：</span><span class="sxs-lookup"><span data-stu-id="c32f2-117">Below is the code we'll add to our MoviesController class to implement this:</span></span>

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample1.cs)]

<span data-ttu-id="c32f2-118">上述程式碼包含所有我們需要我們控制器內的程式碼。</span><span class="sxs-lookup"><span data-stu-id="c32f2-118">The above code contains all of the code that we'll need within our Controller.</span></span>

<span data-ttu-id="c32f2-119">讓我們現在實作我們將使用它來向使用者顯示的表單建立檢視範本。</span><span class="sxs-lookup"><span data-stu-id="c32f2-119">Let's now implement the Create View template that we'll use to display a form to the user.</span></span> <span data-ttu-id="c32f2-120">我們會在第一次的 Create 方法中以滑鼠右鍵按一下，然後選取 建立電影表單檢視範本的 「 加入檢視 」。</span><span class="sxs-lookup"><span data-stu-id="c32f2-120">We'll right click in the first Create method and select "Add View" to create the view template for our Movie form.</span></span>

<span data-ttu-id="c32f2-121">我們將會選取我們會為其檢視資料類別中，移至傳遞檢視範本"影片"，表示我們想要 「 scaffold 」 的 「 建立 」 範本。</span><span class="sxs-lookup"><span data-stu-id="c32f2-121">We'll select that we are going to pass the view template a "Movie" as its view data class, and indicate that we want to "scaffold" a "Create" template.</span></span>

<span data-ttu-id="c32f2-122">[![加入檢視](getting-started-with-mvc-part6/_static/image2.png)](getting-started-with-mvc-part6/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c32f2-122">[![Add View](getting-started-with-mvc-part6/_static/image2.png)](getting-started-with-mvc-part6/_static/image1.png)</span></span>

<span data-ttu-id="c32f2-123">按一下 [新增] 按鈕之後，就會為您建立 \Movies\Create.aspx 檢視範本。</span><span class="sxs-lookup"><span data-stu-id="c32f2-123">After you click the Add button, \Movies\Create.aspx View template will be created for you.</span></span> <span data-ttu-id="c32f2-124">因為我們從 檢視內容 的下拉式清單中選取 建立，加入檢視 對話方塊會自動"scaffold"某些預設內容為我們。</span><span class="sxs-lookup"><span data-stu-id="c32f2-124">Because we selected "Create" from the "view content" dropdown, the Add View dialog automatically "scaffolded" some default content for us.</span></span> <span data-ttu-id="c32f2-125">Scaffolding 建立 HTML&lt;表單&gt;、 移，訊息驗證錯誤的位置和 scaffolding 知道電影，因為其標籤和欄位用於建立每一個屬性的類別。</span><span class="sxs-lookup"><span data-stu-id="c32f2-125">The scaffolding created an HTML &lt;form&gt;, a place for validation error messages to go, and since scaffolding knows about Movies, it created Label and Fields for each property of our class.</span></span>

[!code-aspx[Main](getting-started-with-mvc-part6/samples/sample2.aspx)]

<span data-ttu-id="c32f2-126">因為我們的資料庫會自動提供給影片識別碼，讓我們來移除這些欄位，該參考模型。從我們建立檢視的識別碼。</span><span class="sxs-lookup"><span data-stu-id="c32f2-126">Since our database automatically gives a Movie an ID, let's remove those fields that reference model.Id from our Create View.</span></span> <span data-ttu-id="c32f2-127">移除 7 之後的行&lt;圖例&gt;欄位&lt;/legend&gt;它們會顯示 [識別碼] 欄位，所以我們不想為。</span><span class="sxs-lookup"><span data-stu-id="c32f2-127">Remove the 7 lines after &lt;legend&gt;Fields&lt;/legend&gt; as they show the ID field that we don't want.</span></span>

<span data-ttu-id="c32f2-128">我們現在建立新的電影，並將它加入至資料庫。</span><span class="sxs-lookup"><span data-stu-id="c32f2-128">Let's now create a new movie and add it to the database.</span></span> <span data-ttu-id="c32f2-129">我們會執行的應用程式來執行這項操作，請瀏覽 」 / 電影 」 URL，然後按一下 [建立] 連結以新增新的電影。</span><span class="sxs-lookup"><span data-stu-id="c32f2-129">We'll do this by running the application again and visit the "/Movies" URL and click the "Create" link to add a new Movie.</span></span>

<span data-ttu-id="c32f2-130">[![建立-Windows Internet Explorer](getting-started-with-mvc-part6/_static/image4.png)](getting-started-with-mvc-part6/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="c32f2-130">[![Create - Windows Internet Explorer](getting-started-with-mvc-part6/_static/image4.png)](getting-started-with-mvc-part6/_static/image3.png)</span></span>

<span data-ttu-id="c32f2-131">當我們按一下 [建立] 按鈕時，我們將會公佈回 （透過 HTTP POST) 此表單以剛才所建立的 /Movies/Create 方法上的資料。</span><span class="sxs-lookup"><span data-stu-id="c32f2-131">When we click the Create button, we'll be posting back (via HTTP POST) the data on this form to the /Movies/Create method that we just created.</span></span> <span data-ttu-id="c32f2-132">如同時系統會自動採取"numTimes 」 和 「 名稱 」 參數從 URL 和它們對應至先前的方法上的參數，系統會自動採取從張貼的表單欄位，並將其對應的物件。</span><span class="sxs-lookup"><span data-stu-id="c32f2-132">Just like when the system automatically took the "numTimes" and "name " parameter out of the URL and mapped them to parameters on a method earlier, the system will automatically take the Form Fields from a POST and map them to an object.</span></span> <span data-ttu-id="c32f2-133">在此情況下，例如"ReleaseDate"和"Title"的 HTML 中欄位的值會自動放電影的新執行個體的正確屬性。</span><span class="sxs-lookup"><span data-stu-id="c32f2-133">In this case, values from fields in HTML like "ReleaseDate" and "Title" will automatically be put into the correct properties of a new instance of a Movie.</span></span>

<span data-ttu-id="c32f2-134">讓我們看看第二個建立方法從我們 MoviesController 一次。</span><span class="sxs-lookup"><span data-stu-id="c32f2-134">Let's look at the second Create method from our MoviesController again.</span></span> <span data-ttu-id="c32f2-135">請注意花費"影片"物件做為引數的方式：</span><span class="sxs-lookup"><span data-stu-id="c32f2-135">Notice how it takes a "Movie" object as an argument:</span></span>

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample3.cs)]

<span data-ttu-id="c32f2-136">此影片物件接著傳遞給我們建立動作方法中，[HttpPost] 版本，我們儲存在資料庫中，然後使用者重新導向至 index 動作方法，這個方法會將電影清單中顯示已儲存的結果：</span><span class="sxs-lookup"><span data-stu-id="c32f2-136">This Movie object was then passed to the [HttpPost] version of our Create action method, and we saved it in the database and then redirected the user back to the Index() action method which will show the saved result in the movie list:</span></span>

<span data-ttu-id="c32f2-137">[![電影清單-Windows Internet Explorer](getting-started-with-mvc-part6/_static/image6.png)](getting-started-with-mvc-part6/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="c32f2-137">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part6/_static/image6.png)](getting-started-with-mvc-part6/_static/image5.png)</span></span>

<span data-ttu-id="c32f2-138">我們不檢查是否我們電影是正確的不過，而且資料庫將不會讓我們將電影儲存沒有標題。</span><span class="sxs-lookup"><span data-stu-id="c32f2-138">We aren't checking if our movies are correct, though, and the database won't allow us to save a movie with no Title.</span></span> <span data-ttu-id="c32f2-139">如果我們無法告訴使用者的資料庫之前擲回錯誤，它應該不錯。</span><span class="sxs-lookup"><span data-stu-id="c32f2-139">It'd be nice if we could tell the user that before the database threw an error.</span></span> <span data-ttu-id="c32f2-140">我們將會執行這個下一步，方法是加入我們的應用程式的驗證支援。</span><span class="sxs-lookup"><span data-stu-id="c32f2-140">We'll do this next by adding validation support to our application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c32f2-141">[上一頁](getting-started-with-mvc-part5.md)
> [下一頁](getting-started-with-mvc-part7.md)</span><span class="sxs-lookup"><span data-stu-id="c32f2-141">[Previous](getting-started-with-mvc-part5.md)
[Next](getting-started-with-mvc-part7.md)</span></span>
