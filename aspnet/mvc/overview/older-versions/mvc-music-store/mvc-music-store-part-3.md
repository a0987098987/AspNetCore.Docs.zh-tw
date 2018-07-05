---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: 第 3 部分： 檢視和 ViewModels |Microsoft Docs
author: jongalloway
description: 本教學課程系列會詳細說明所有建置 ASP.NET MVC Music 市集範例應用程式所採取的步驟。 第 3 部分涵蓋檢視和 ViewModels。
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 0cfdc2864221a0efc81eb362571c72f20eb05af8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368731"
---
<a name="part-3-views-and-viewmodels"></a><span data-ttu-id="e20c6-104">第 3 部分： 檢視和 ViewModels</span><span class="sxs-lookup"><span data-stu-id="e20c6-104">Part 3: Views and ViewModels</span></span>
====================
<span data-ttu-id="e20c6-105">藉由[Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="e20c6-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="e20c6-106">MVC Music 市集是介紹，並逐步說明如何使用 ASP.NET MVC 和 Visual Studio 進行 web 開發的教學課程應用程式。</span><span class="sxs-lookup"><span data-stu-id="e20c6-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="e20c6-107">MVC Music 市集是銷售線上音樂 album 和實作基本的網站管理、 使用者登入時，和 「 購物車 」 功能的輕量級的範例存放區實作。</span><span class="sxs-lookup"><span data-stu-id="e20c6-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="e20c6-108">本教學課程系列會詳細說明所有建置 ASP.NET MVC Music 市集範例應用程式所採取的步驟。</span><span class="sxs-lookup"><span data-stu-id="e20c6-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="e20c6-109">第 3 部分涵蓋檢視和 ViewModels。</span><span class="sxs-lookup"><span data-stu-id="e20c6-109">Part 3 covers Views and ViewModels.</span></span>


<span data-ttu-id="e20c6-110">到目前為止我們所只已傳回字串從控制器動作。</span><span class="sxs-lookup"><span data-stu-id="e20c6-110">So far we've just been returning strings from controller actions.</span></span> <span data-ttu-id="e20c6-111">這是不錯的方式取得控制器的運作方式，了解但不是想要如何建置真實 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="e20c6-111">That's a nice way to get an idea of how controllers work, but it's not how you'd want to build a real web application.</span></span> <span data-ttu-id="e20c6-112">我們想要更好的方法，以回到瀏覽我們的網站的瀏覽器產生 HTML，我們可以使用範本檔案更輕鬆地自訂 HTML 內容的其中一個寄回。</span><span class="sxs-lookup"><span data-stu-id="e20c6-112">We are going to want a better way to generate HTML back to browsers visiting our site – one where we can use template files to more easily customize the HTML content send back.</span></span> <span data-ttu-id="e20c6-113">這正是檢視的。</span><span class="sxs-lookup"><span data-stu-id="e20c6-113">That's exactly what Views do.</span></span>

## <a name="adding-a-view-template"></a><span data-ttu-id="e20c6-114">新增檢視範本</span><span class="sxs-lookup"><span data-stu-id="e20c6-114">Adding a View template</span></span>

<span data-ttu-id="e20c6-115">若要使用檢視範本，我們將變更 HomeController Index 方法要傳回 ActionResult，並且會傳回 View()，類似如下：</span><span class="sxs-lookup"><span data-stu-id="e20c6-115">To use a view-template, we'll change the HomeController Index method to return an ActionResult, and have it return View(), like below:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

<span data-ttu-id="e20c6-116">上述的變更可讓您表示，而不是傳回字串，我們改為想要使用 「 檢視 」 來產生結果。</span><span class="sxs-lookup"><span data-stu-id="e20c6-116">The above change indicates that instead of returned a string, we instead want to use a "View" to generate a result back.</span></span>

<span data-ttu-id="e20c6-117">我們現在將我們的專案中加入適當的檢視範本。</span><span class="sxs-lookup"><span data-stu-id="e20c6-117">We'll now add an appropriate View template to our project.</span></span> <span data-ttu-id="e20c6-118">若要這樣做，我們會將文字游標放在索引動作方法中，然後以滑鼠右鍵按一下並選取 [新增檢視]。</span><span class="sxs-lookup"><span data-stu-id="e20c6-118">To do this we'll position the text cursor within the Index action method, then right-click and select "Add View".</span></span> <span data-ttu-id="e20c6-119">這會顯示 [新增檢視] 對話方塊：</span><span class="sxs-lookup"><span data-stu-id="e20c6-119">This will bring up the Add View dialog:</span></span>

<span data-ttu-id="e20c6-120">![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e20c6-120">![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)</span></span>

<span data-ttu-id="e20c6-121">[新增檢視] 對話方塊可讓我們快速且輕鬆地產生檢視範本檔案。</span><span class="sxs-lookup"><span data-stu-id="e20c6-121">The "Add View" dialog allows us to quickly and easily generate View template files.</span></span> <span data-ttu-id="e20c6-122">預設 [新增檢視] 對話方塊會預先填入的檢視範本來建立，使其符合使用它的動作方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="e20c6-122">By default the "Add View" dialog pre-populates the name of the View template to create so that it matches the action method that will use it.</span></span> <span data-ttu-id="e20c6-123">因為我們使用我們 HomeController index （） 動作方法內的 [新增檢視] 內容功能表，上述的 [新增檢視] 對話方塊會有 「 索引 」 做為預先填入的預設檢視名稱。</span><span class="sxs-lookup"><span data-stu-id="e20c6-123">Because we used the "Add View" context menu within the Index() action method of our HomeController, the "Add View" dialog above has "Index" as the view name pre-populated by default.</span></span> <span data-ttu-id="e20c6-124">我們不需要變更任何選項，此對話方塊上，因此按一下 [新增] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="e20c6-124">We don't need to change any of the options on this dialog, so click the Add button.</span></span>

<span data-ttu-id="e20c6-125">當我們按一下 [新增] 按鈕時，Visual Web Developer 會建立新的 Index.cshtml 為我們的檢視範本，在 \Views\Home 目錄中，建立資料夾，如果不存在。</span><span class="sxs-lookup"><span data-stu-id="e20c6-125">When we click the Add button, Visual Web Developer will create a new Index.cshtml view template for us in the \Views\Home directory, creating the folder if doesn't already exist.</span></span>

![](mvc-music-store-part-3/_static/image2.png)

<span data-ttu-id="e20c6-126">「 Index.cshtml 」 檔案的名稱和資料夾位置是很重要，並會遵循的預設 ASP.NET MVC 的命名慣例。</span><span class="sxs-lookup"><span data-stu-id="e20c6-126">The name and folder location of the "Index.cshtml" file is important, and follows the default ASP.NET MVC naming conventions.</span></span> <span data-ttu-id="e20c6-127">目錄名稱、 \Views\Home，比對名為 HomeController 的控制站-。</span><span class="sxs-lookup"><span data-stu-id="e20c6-127">The directory name, \Views\Home, matches the controller - which is named HomeController.</span></span> <span data-ttu-id="e20c6-128">檢視範本名稱，而索引比對將會顯示檢視控制器動作方法。</span><span class="sxs-lookup"><span data-stu-id="e20c6-128">The view template name, Index, matches the controller action method which will be displaying the view.</span></span>

<span data-ttu-id="e20c6-129">ASP.NET MVC 可讓我們避免傳回檢視的情況下，我們在使用此命名慣例時，明確指定的名稱或檢視範本的位置。</span><span class="sxs-lookup"><span data-stu-id="e20c6-129">ASP.NET MVC allows us to avoid having to explicitly specify the name or location of a view template when we use this naming convention to return a view.</span></span> <span data-ttu-id="e20c6-130">它會預設轉譯 \Views\Home\Index.cshtml 檢視範本時我們我們 HomeController 中撰寫類似如下的程式碼：</span><span class="sxs-lookup"><span data-stu-id="e20c6-130">It will by default render the \Views\Home\Index.cshtml view template when we write code like below within our HomeController:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

<span data-ttu-id="e20c6-131">Visual Web Developer 中建立，並開啟 「 Index.cshtml 」 檢視範本之後我們按一下 [新增] 按鈕，在 [新增檢視] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="e20c6-131">Visual Web Developer created and opened the "Index.cshtml" view template after we clicked the "Add" button within the "Add View" dialog.</span></span> <span data-ttu-id="e20c6-132">Index.cshtml 的內容如下所示。</span><span class="sxs-lookup"><span data-stu-id="e20c6-132">The contents of Index.cshtml are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

<span data-ttu-id="e20c6-133">此檢視使用 Razor 語法，也就是比 ASP.NET Web Form 和舊版的 ASP.NET MVC 中使用 Web Form 檢視引擎更簡潔。</span><span class="sxs-lookup"><span data-stu-id="e20c6-133">This view is using the Razor syntax, which is more concise than the Web Forms view engine used in ASP.NET Web Forms and previous versions of ASP.NET MVC.</span></span> <span data-ttu-id="e20c6-134">Web Form 檢視引擎在 ASP.NET MVC 3 中，仍然可用，但許多開發人員尋找 Razor 檢視引擎會非常適合 ASP.NET MVC 開發。</span><span class="sxs-lookup"><span data-stu-id="e20c6-134">The Web Forms view engine is still available in ASP.NET MVC 3, but many developers find that the Razor view engine fits ASP.NET MVC development really well.</span></span>

<span data-ttu-id="e20c6-135">前三行設定使用 ViewBag.Title 頁面標題。</span><span class="sxs-lookup"><span data-stu-id="e20c6-135">The first three lines set the page title using ViewBag.Title.</span></span> <span data-ttu-id="e20c6-136">我們將查看其運作方式更詳細地推出，但首先讓我們更新文字標題文字，並檢視該頁面。</span><span class="sxs-lookup"><span data-stu-id="e20c6-136">We'll look at how this works in more detail soon, but first let's update the text heading text and view the page.</span></span> <span data-ttu-id="e20c6-137">更新&lt;h2&gt;說: 「 這是首頁 」，如下所示的標記。</span><span class="sxs-lookup"><span data-stu-id="e20c6-137">Update the &lt;h2&gt; tag to say "This is the Home Page" as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

<span data-ttu-id="e20c6-138">執行應用程式會顯示我們新的文字顯示在首頁上。</span><span class="sxs-lookup"><span data-stu-id="e20c6-138">Running the application shows that our new text is visible on the home page.</span></span>

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a><span data-ttu-id="e20c6-139">對於常見的網站元素使用的版面配置</span><span class="sxs-lookup"><span data-stu-id="e20c6-139">Using a Layout for common site elements</span></span>

<span data-ttu-id="e20c6-140">大部分的網站有許多的頁面之間共用的內容： 瀏覽、 頁尾、 標誌影像、 樣式表參考等。Razor 檢視引擎簡化這要使用一個稱為管理\_Layout.cshtml 自動/檢視/共用資料夾內建立的。</span><span class="sxs-lookup"><span data-stu-id="e20c6-140">Most websites have content which is shared between many pages: navigation, footers, logo images, stylesheet references, etc. The Razor view engine makes this easy to manage using a page called \_Layout.cshtml which has automatically been created for us inside the /Views/Shared folder.</span></span>

![](mvc-music-store-part-3/_static/image4.png)

<span data-ttu-id="e20c6-141">按兩下這個資料夾，以檢視其內容，會如下所示。</span><span class="sxs-lookup"><span data-stu-id="e20c6-141">Double-click on this folder to view the contents, which are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

<span data-ttu-id="e20c6-142">從我們的個別檢視的內容會顯示所@RenderBody（） 命令，以及任何通用的內容，我們想要出現在之外，可以新增至\_Layout.cshtml 標記。</span><span class="sxs-lookup"><span data-stu-id="e20c6-142">The content from our individual views will be displayed by the @RenderBody() command, and any common content that we want to appear outside of that can be added to the \_Layout.cshtml markup.</span></span> <span data-ttu-id="e20c6-143">我們希望我們首頁和存放區的區域，在網站中，所有的頁面上必須具有連結的通用標頭，因此我們將新增的正上方的範本我們 MVC Music 市集@RenderBody（） 陳述式。</span><span class="sxs-lookup"><span data-stu-id="e20c6-143">We'll want our MVC Music Store to have a common header with links to our Home page and Store area on all pages in the site, so we'll add that to the template directly above that @RenderBody() statement.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a><span data-ttu-id="e20c6-144">更新 樣式表</span><span class="sxs-lookup"><span data-stu-id="e20c6-144">Updating the StyleSheet</span></span>

<span data-ttu-id="e20c6-145">空白專案範本包含非常簡化的 CSS 檔案只包含用來顯示驗證訊息的樣式。</span><span class="sxs-lookup"><span data-stu-id="e20c6-145">The empty project template includes a very streamlined CSS file which just includes styles used to display validation messages.</span></span> <span data-ttu-id="e20c6-146">部分其他 CSS 和影像來定義我們的網站，外觀與風格，因此我們將新增中現在提供我們的設計工具。</span><span class="sxs-lookup"><span data-stu-id="e20c6-146">Our designer has provided some additional CSS and images to define the look and feel for our site, so we'll add those in now.</span></span>

<span data-ttu-id="e20c6-147">MvcMusicStore Assets.zip 可在內容目錄中包含更新過的 CSS 檔案和映像[MVC Music 市集](https://github.com/evilDave/MVC-Music-Store)。</span><span class="sxs-lookup"><span data-stu-id="e20c6-147">The updated CSS file and Images are included in the Content directory of MvcMusicStore-Assets.zip which is available at [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span> <span data-ttu-id="e20c6-148">我們將在 Windows 檔案總管中選取這兩者，並卸除我們的解決方案內容的資料夾，在 Visual Web Developer 中，如下所示：</span><span class="sxs-lookup"><span data-stu-id="e20c6-148">We'll select both of them in Windows Explorer and drop them into our Solution's Content folder in Visual Web Developer, as shown below:</span></span>

![](mvc-music-store-part-3/_static/image5.png)

<span data-ttu-id="e20c6-149">系統會要求您確認是否您想要覆寫現有的 Site.css 檔案。</span><span class="sxs-lookup"><span data-stu-id="e20c6-149">You'll be asked to confirm if you want to overwrite the existing Site.css file.</span></span> <span data-ttu-id="e20c6-150">按一下 [是]。</span><span class="sxs-lookup"><span data-stu-id="e20c6-150">Click Yes.</span></span>

![](mvc-music-store-part-3/_static/image6.png)

<span data-ttu-id="e20c6-151">您的應用程式的內容資料夾現在會出現，如下所示：</span><span class="sxs-lookup"><span data-stu-id="e20c6-151">The Content folder of your application will now appear as follows:</span></span>

![](mvc-music-store-part-3/_static/image7.png)

<span data-ttu-id="e20c6-152">現在讓我們執行應用程式，並查看我們的變更在首頁上的外觀。</span><span class="sxs-lookup"><span data-stu-id="e20c6-152">Now let's run the application and see how our changes look on the Home page.</span></span>

![](mvc-music-store-part-3/_static/image8.png)

- <span data-ttu-id="e20c6-153">讓我們檢閱變更的項目： HomeController 索引動作方法找到並顯示 [\Views\Home\Index.cshtmlView] 範本中，即使我們的程式碼會呼叫 「 傳回 View()"，因為我們的檢視範本會遵循標準的命名慣例。</span><span class="sxs-lookup"><span data-stu-id="e20c6-153">Let's review what's changed: The HomeController's Index action method found and displayed the \Views\Home\Index.cshtmlView template, even though our code called "return View()", because our View template followed the standard naming convention.</span></span>
- <span data-ttu-id="e20c6-154">首頁會顯示一個簡單的歡迎訊息 \Views\Home\Index.cshtml 檢視範本內定義的。</span><span class="sxs-lookup"><span data-stu-id="e20c6-154">The Home Page is displaying a simple welcome message that is defined within the \Views\Home\Index.cshtml view template.</span></span>
- <span data-ttu-id="e20c6-155">首頁會使用我們\_Layout.cshtml 範本，因此歡迎訊息內含 HTML 的標準網站的版面配置。</span><span class="sxs-lookup"><span data-stu-id="e20c6-155">The Home Page is using our \_Layout.cshtml template, and so the welcome message is contained within the standard site HTML layout.</span></span>

## <a name="using-a-model-to-pass-information-to-our-view"></a><span data-ttu-id="e20c6-156">使用模型來傳遞資訊給我們的檢視</span><span class="sxs-lookup"><span data-stu-id="e20c6-156">Using a Model to pass information to our View</span></span>

<span data-ttu-id="e20c6-157">只會顯示硬式編碼 HTML 的檢視範本並不會很有趣的網站。</span><span class="sxs-lookup"><span data-stu-id="e20c6-157">A View template that just displays hardcoded HTML isn't going to make a very interesting web site.</span></span> <span data-ttu-id="e20c6-158">若要建立動態網站，我們會改為想要將從我們的控制器動作的資訊傳遞至我們的檢視範本。</span><span class="sxs-lookup"><span data-stu-id="e20c6-158">To create a dynamic web site, we'll instead want to pass information from our controller actions to our view templates.</span></span>

<span data-ttu-id="e20c6-159">在模型-檢視-控制器模式中，模型是指一詞的物件代表應用程式中的資料。</span><span class="sxs-lookup"><span data-stu-id="e20c6-159">In the Model-View-Controller pattern, the term Model refers to objects which represent the data in the application.</span></span> <span data-ttu-id="e20c6-160">通常，模型物件對應到您的資料庫中的資料表，但他們不需要。</span><span class="sxs-lookup"><span data-stu-id="e20c6-160">Often, model objects correspond to tables in your database, but they don't have to.</span></span>

<span data-ttu-id="e20c6-161">控制器動作方法會傳回 ActionResult 可以傳遞至檢視的模型物件。</span><span class="sxs-lookup"><span data-stu-id="e20c6-161">Controller action methods which return an ActionResult can pass a model object to the view.</span></span> <span data-ttu-id="e20c6-162">這可讓產生回應，以及檢視範本，然後將這項資訊傳遞所需的所有資訊完全封裝的控制站，要用來產生適當的 HTML 回應。</span><span class="sxs-lookup"><span data-stu-id="e20c6-162">This allows a Controller to cleanly package up all the information needed to generate a response, and then pass this information off to a View template to use to generate the appropriate HTML response.</span></span> <span data-ttu-id="e20c6-163">這是最容易了解藉由查看動作中，我們現在就開始。</span><span class="sxs-lookup"><span data-stu-id="e20c6-163">This is easiest to understand by seeing it in action, so let's get started.</span></span>

<span data-ttu-id="e20c6-164">首先我們要建立某些模型類別，來代表我們的存放區中的內容類型和專輯。</span><span class="sxs-lookup"><span data-stu-id="e20c6-164">First we'll create some Model classes to represent Genres and Albums within our store.</span></span> <span data-ttu-id="e20c6-165">現在就開始建立內容類型的類別。</span><span class="sxs-lookup"><span data-stu-id="e20c6-165">Let's start by creating a Genre class.</span></span> <span data-ttu-id="e20c6-166">以滑鼠右鍵按一下您的專案內的 「 模型 」 資料夾中，選擇 「 加入類別 」 選項，並將檔案命名"Genre.cs 」。</span><span class="sxs-lookup"><span data-stu-id="e20c6-166">Right-click the "Models" folder within your project, choose the "Add Class" option, and name the file "Genre.cs".</span></span>

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

<span data-ttu-id="e20c6-167">然後加入所建立的類別公開的字串名稱屬性：</span><span class="sxs-lookup"><span data-stu-id="e20c6-167">Then add a public string Name property to the class that was created:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

<span data-ttu-id="e20c6-168">*注意： 萬一您好奇，{get; 設定;} 標記法正在使用 C# 的自動實作屬性功能。可以讓我們屬性的優點，而不需要我們宣告支援欄位。*</span><span class="sxs-lookup"><span data-stu-id="e20c6-168">*Note: In case you're wondering, the { get; set; } notation is making use of C#'s auto-implemented properties feature. This gives us the benefits of a property without requiring us to declare a backing field.*</span></span>

<span data-ttu-id="e20c6-169">接下來，請遵循相同步驟來建立具有標題和內容類型屬性的專輯類別 （名為 Album.cs）：</span><span class="sxs-lookup"><span data-stu-id="e20c6-169">Next, follow the same steps to create an Album class (named Album.cs) that has a Title and a Genre property:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

<span data-ttu-id="e20c6-170">現在我們可以修改 StoreController，若要使用檢視會顯示動態資訊從我們的模型。</span><span class="sxs-lookup"><span data-stu-id="e20c6-170">Now we can modify the StoreController to use Views which display dynamic information from our Model.</span></span> <span data-ttu-id="e20c6-171">如果-供示範之用現在-我們名為我們的要求識別碼為基礎的相簿，我們可能會顯示下列檢視該資訊。</span><span class="sxs-lookup"><span data-stu-id="e20c6-171">If - for demonstration purposes right now - we named our Albums based on the request ID, we could display that information as in the view below.</span></span>

![](mvc-music-store-part-3/_static/image10.png)

<span data-ttu-id="e20c6-172">我們一開始先變更存放區詳細資料動作，因此它會顯示為單一的專輯資訊。</span><span class="sxs-lookup"><span data-stu-id="e20c6-172">We'll start by changing the Store Details action so it shows the information for a single album.</span></span> <span data-ttu-id="e20c6-173">將"using"陳述式加入至頂端**StoreControllers**類別，以包含 MvcMusicStore.Models 命名空間，因此我們不需要輸入 MvcMusicStore.Models.Album，每當我們想要使用的專輯類別。</span><span class="sxs-lookup"><span data-stu-id="e20c6-173">Add a "using" statement to the top of the **StoreControllers** class to include the MvcMusicStore.Models namespace, so we don't need to type MvcMusicStore.Models.Album every time we want to use the album class.</span></span> <span data-ttu-id="e20c6-174">該類別的 「 using 」 區段現在應該會出現，如下所示。</span><span class="sxs-lookup"><span data-stu-id="e20c6-174">The "usings" section of that class should now appear as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

<span data-ttu-id="e20c6-175">接下來，我們將更新的詳細資料控制器動作，使它傳回 ActionResult，而不是字串，如同我們一樣 HomeController 的 Index 方法。</span><span class="sxs-lookup"><span data-stu-id="e20c6-175">Next, we'll update the Details controller action so that it returns an ActionResult rather than a string, as we did with the HomeController's Index method.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

<span data-ttu-id="e20c6-176">現在我們可以修改傳回至檢視的專輯物件的邏輯。</span><span class="sxs-lookup"><span data-stu-id="e20c6-176">Now we can modify the logic to return an Album object to the view.</span></span> <span data-ttu-id="e20c6-177">本教學課程稍後我們將會從資料庫擷取資料 – 但現在我們將使用 「 虛擬資料 」 來開始。</span><span class="sxs-lookup"><span data-stu-id="e20c6-177">Later in this tutorial we will be retrieving the data from a database – but for right now we will use "dummy data" to get started.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

<span data-ttu-id="e20c6-178">*注意： 如果您不熟悉使用 C#，您可能會假設，使用 var 表示我們專輯的變數是晚期繫結。不正確 – C# 編譯器會使用根據什麼我們是指派給變數，以判斷該專輯的型別專輯和專輯型別為編譯本機專輯變數，因此我們編譯時期檢查和 Visual Studio 程式碼編輯器的型別推斷支援。*</span><span class="sxs-lookup"><span data-stu-id="e20c6-178">*Note: If you're unfamiliar with C#, you may assume that using var means that our album variable is late-bound. That's not correct – the C# compiler is using type-inference based on what we're assigning to the variable to determine that album is of type Album and compiling the local album variable as an Album type, so we get compile-time checking and Visual Studio code-editor support.*</span></span>

<span data-ttu-id="e20c6-179">我們現在來建立檢視範本來產生 HTML 回應中使用我們的專輯。</span><span class="sxs-lookup"><span data-stu-id="e20c6-179">Let's now create a View template that uses our Album to generate an HTML response.</span></span> <span data-ttu-id="e20c6-180">這樣做之前我們要建置專案，以便知道我們新建立的專輯類別的 [新增檢視] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="e20c6-180">Before we do that we need to build the project so that the Add View dialog knows about our newly created Album class.</span></span> <span data-ttu-id="e20c6-181">您可以建置專案選取 Debug⇨Build MvcMusicStore 功能表項目 （針對額外信用額度，您可以使用 Ctrl-Shift-B 捷徑來建置專案）。</span><span class="sxs-lookup"><span data-stu-id="e20c6-181">You can build the project by selecting the Debug⇨Build MvcMusicStore menu item (for extra credit, you can use the Ctrl-Shift-B shortcut to build the project).</span></span>

![](mvc-music-store-part-3/_static/image11.png)

<span data-ttu-id="e20c6-182">既然我們已經設定好我們支援的類別，我們可以開始建置我們的檢視範本。</span><span class="sxs-lookup"><span data-stu-id="e20c6-182">Now that we've set up our supporting classes, we're ready to build our View template.</span></span> <span data-ttu-id="e20c6-183">以滑鼠右鍵按一下詳細資料的方法中，並從操作功能表選取 [新增檢視...]。</span><span class="sxs-lookup"><span data-stu-id="e20c6-183">Right-click within the Details method and select "Add View…" from the context menu.</span></span>

![](mvc-music-store-part-3/_static/image12.png)

<span data-ttu-id="e20c6-184">我們要建立新的檢視範本像我們之前在 homecontroller。</span><span class="sxs-lookup"><span data-stu-id="e20c6-184">We are going to create a new View template like we did before with the HomeController.</span></span> <span data-ttu-id="e20c6-185">因為我們會建立它從 StoreController 它將依預設會產生 \Views\Store\Index.cshtml 檔案中。</span><span class="sxs-lookup"><span data-stu-id="e20c6-185">Because we are creating it from the StoreController it will by default be generated in a \Views\Store\Index.cshtml file.</span></span>

<span data-ttu-id="e20c6-186">不同於之前，我們選取 [建立強型別] 檢視的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="e20c6-186">Unlike before, we are going to check the "Create a strongly-typed" view checkbox.</span></span> <span data-ttu-id="e20c6-187">然後我們選取內 「 檢視資料類別 「 卸除 downlist 我們"Album"的類別。</span><span class="sxs-lookup"><span data-stu-id="e20c6-187">We are then going to select our "Album" class within the "View data-class" drop-downlist.</span></span> <span data-ttu-id="e20c6-188">這會導致 [新增檢視] 對話方塊，來建立檢視範本需要的物件會傳遞給它使用的 Album。</span><span class="sxs-lookup"><span data-stu-id="e20c6-188">This will cause the "Add View" dialog to create a View template that expects that an Album object will be passed to it to use.</span></span>

![](mvc-music-store-part-3/_static/image13.png)

<span data-ttu-id="e20c6-189">當我們按一下 [新增] 按鈕時將會建立我們 \Views\Store\Details.cshtml 檢視範本包含下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="e20c6-189">When we click the "Add" button our \Views\Store\Details.cshtml View template will be created, containing the following code.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

<span data-ttu-id="e20c6-190">請注意第一行中，這表示此檢視為強型別到專輯類別。</span><span class="sxs-lookup"><span data-stu-id="e20c6-190">Notice the first line, which indicates that this view is strongly-typed to our Album class.</span></span> <span data-ttu-id="e20c6-191">Razor 檢視引擎了解，它已傳遞專輯物件，因此我們可以輕鬆地存取 模型屬性，即使已在 Visual Web Developer 編輯器中 IntelliSense 的優點。</span><span class="sxs-lookup"><span data-stu-id="e20c6-191">The Razor view engine understands that it has been passed an Album object, so we can easily access model properties and even have the benefit of IntelliSense in the Visual Web Developer editor.</span></span>

<span data-ttu-id="e20c6-192">更新&lt;h2&gt;標記讓它會專輯的 Title 屬性顯示的修改才會出現，如下所示的那一行。</span><span class="sxs-lookup"><span data-stu-id="e20c6-192">Update the &lt;h2&gt; tag so it displays the Album's Title property by modifying that line to appear as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

<span data-ttu-id="e20c6-193">請注意，當您輸入超過此時限後的，會觸發 IntelliSense@Model關鍵字，顯示的屬性和專輯類別支援的方法。</span><span class="sxs-lookup"><span data-stu-id="e20c6-193">Notice that IntelliSense is triggered when you enter the period after the @Model keyword, showing the properties and methods that the Album class supports.</span></span>

<span data-ttu-id="e20c6-194">讓我們現在重新執行我們的專案，並瀏覽市集/詳細資料/5 URL。</span><span class="sxs-lookup"><span data-stu-id="e20c6-194">Let's now re-run our project and visit the /Store/Details/5 URL.</span></span> <span data-ttu-id="e20c6-195">我們會看到類似如下的專輯的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="e20c6-195">We'll see details of an Album like below.</span></span>

![](mvc-music-store-part-3/_static/image14.png)

<span data-ttu-id="e20c6-196">現在，我們將進行類似更新存放區瀏覽動作方法。</span><span class="sxs-lookup"><span data-stu-id="e20c6-196">Now we'll make a similar update for the Store Browse action method.</span></span> <span data-ttu-id="e20c6-197">更新方法，使其傳回 ActionResult，並修改方法的邏輯，因此它會建立新的內容類型物件並將它傳回至檢視。</span><span class="sxs-lookup"><span data-stu-id="e20c6-197">Update the method so it returns an ActionResult, and modify the method logic so it creates a new Genre object and returns it to the View.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

<span data-ttu-id="e20c6-198">以滑鼠右鍵按一下 瀏覽方法中，並從操作功能表中，選取 新增檢視 然後新增為強型別檢視的強型別加入內容類型的類別。</span><span class="sxs-lookup"><span data-stu-id="e20c6-198">Right-click in the Browse method and select "Add View…" from the context menu, then add a View that is strongly-typed add a strongly typed to the Genre class.</span></span>

![](mvc-music-store-part-3/_static/image15.png)

<span data-ttu-id="e20c6-199">更新&lt;h2&gt;檢視中的項目中的程式碼 （/Views/Store/Browse.cshtml) 顯示的內容類型資訊。</span><span class="sxs-lookup"><span data-stu-id="e20c6-199">Update the &lt;h2&gt; element in the view code (in /Views/Store/Browse.cshtml) to display the Genre information.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

<span data-ttu-id="e20c6-200">現在讓我們重新執行我們的專案，並瀏覽至存放區/瀏覽？內容類型 = Disco 的 URL。</span><span class="sxs-lookup"><span data-stu-id="e20c6-200">Now let's re-run our project and browse to the /Store/Browse?Genre=Disco URL.</span></span> <span data-ttu-id="e20c6-201">我們會看到顯示類似下方的瀏覽 頁面。</span><span class="sxs-lookup"><span data-stu-id="e20c6-201">We'll see the Browse page displayed like below.</span></span>

![](mvc-music-store-part-3/_static/image16.png)

<span data-ttu-id="e20c6-202">最後，讓我們稍微複雜一點的更新，以**存放區索引**動作方法和檢視，以顯示在我們的存放區中的所有內容類型清單。</span><span class="sxs-lookup"><span data-stu-id="e20c6-202">Finally, let's make a slightly more complex update to the **Store Index** action method and view to display a list of all the Genres in our store.</span></span> <span data-ttu-id="e20c6-203">我們將使用清單的內容類型為我們的模型物件，而不是只是單一的內容類型來這麼做。</span><span class="sxs-lookup"><span data-stu-id="e20c6-203">We'll do that by using a List of Genres as our model object, rather than just a single Genre.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

<span data-ttu-id="e20c6-204">以滑鼠右鍵按一下 存放區索引動作方法中，然後選取 新增檢視之前，請選取內容類型作為模型類別，以及按下 新增 按鈕。</span><span class="sxs-lookup"><span data-stu-id="e20c6-204">Right-click in the Store Index action method and select Add View as before, select Genre as the Model class, and press the Add button.</span></span>

![](mvc-music-store-part-3/_static/image17.png)

<span data-ttu-id="e20c6-205">我們將會在第一次變更@model來指示檢視將會需要數個內容類型的宣告物件，而不是其中一個。</span><span class="sxs-lookup"><span data-stu-id="e20c6-205">First we'll change the @model declaration to indicate that the view will be expecting several Genre objects rather than just one.</span></span> <span data-ttu-id="e20c6-206">變更 /Store/Index.cshtml 讀取，如下所示的第一行：</span><span class="sxs-lookup"><span data-stu-id="e20c6-206">Change the first line of /Store/Index.cshtml to read as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

<span data-ttu-id="e20c6-207">這會告知 Razor 檢視引擎，它會使用模型物件，可存放數個內容類型的物件。</span><span class="sxs-lookup"><span data-stu-id="e20c6-207">This tells the Razor view engine that it will be working with a model object that can hold several Genre objects.</span></span> <span data-ttu-id="e20c6-208">我們正在使用 IEnumerable&lt;Genre&gt;而不是清單&lt;內容類型&gt;因為它是更泛型的讓我們可以將我們的模型類型稍後變更為任何支援 IEnumerable 介面的物件類型。</span><span class="sxs-lookup"><span data-stu-id="e20c6-208">We're using an IEnumerable&lt;Genre&gt; rather than a List&lt;Genre&gt; since it's more generic, allowing us to change our model type later to any object type that supports the IEnumerable interface.</span></span>

<span data-ttu-id="e20c6-209">接下來，我們會執行迴圈內容類型中的物件模型中下列已完成的檢視程式碼所示。</span><span class="sxs-lookup"><span data-stu-id="e20c6-209">Next, we'll loop through the Genre objects in the model as shown in the completed view code below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

<span data-ttu-id="e20c6-210">請注意，我們有完整的 IntelliSense 支援，我們輸入下列程式碼中，如此當我們輸入 「@Model。 」</span><span class="sxs-lookup"><span data-stu-id="e20c6-210">Notice that we have full IntelliSense support as we enter this code, so that when we type "@Model."</span></span> <span data-ttu-id="e20c6-211">我們看到所有方法和 IEnumerable 的型別內容類型所支援的屬性。</span><span class="sxs-lookup"><span data-stu-id="e20c6-211">we see all methods and properties supported by an IEnumerable of type Genre.</span></span>

![](mvc-music-store-part-3/_static/image18.png)

<span data-ttu-id="e20c6-212">在"foreach"迴圈，Visual Web Developer 知道的每個項目型別的內容類型，所以，我們看到 IntelliSence 每個內容類型的類型。</span><span class="sxs-lookup"><span data-stu-id="e20c6-212">Within our "foreach" loop, Visual Web Developer knows that each item is of type Genre, so we see IntelliSence for each the Genre type.</span></span>

![](mvc-music-store-part-3/_static/image19.png)

<span data-ttu-id="e20c6-213">接下來，樣板功能會檢查內容類型的物件，並判斷，每個都會有名稱屬性，讓它執行迴圈，並將其輸出。它也會產生每個個別的項目編輯、 詳細資料和刪除連結。</span><span class="sxs-lookup"><span data-stu-id="e20c6-213">Next, the scaffolding feature examined the Genre object and determined that each will have a Name property, so it loops through and writes them out. It also generates Edit, Details, and Delete links to each individual item.</span></span> <span data-ttu-id="e20c6-214">我們會利用它，稍後我們存放區管理員 中，但現在我們想要改為有一個簡單的清單。</span><span class="sxs-lookup"><span data-stu-id="e20c6-214">We'll take advantage of that later in our store manager, but for now we'd like to have a simple list instead.</span></span>

<span data-ttu-id="e20c6-215">當我們執行應用程式，並瀏覽至 /Store 時，我們看到的計數 」 和 「 內容類型的清單會顯示。</span><span class="sxs-lookup"><span data-stu-id="e20c6-215">When we run the application and browse to /Store, we see that both the count and list of Genres is displayed.</span></span>

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a><span data-ttu-id="e20c6-216">新增頁面之間的連結</span><span class="sxs-lookup"><span data-stu-id="e20c6-216">Adding Links between pages</span></span>

<span data-ttu-id="e20c6-217">/Store URL，其中列出目前的內容類型只是以純文字形式列出的內容類型名稱。</span><span class="sxs-lookup"><span data-stu-id="e20c6-217">Our /Store URL that lists Genres currently lists the Genre names simply as plain text.</span></span> <span data-ttu-id="e20c6-218">讓我們變更這如此而不是純文字，我們改為需要內容類型名稱連結至適當的存放區/瀏覽 URL，以便按一下音樂內容類型，例如"Disco"會瀏覽至存放區/瀏覽？ 內容類型 = Disco URL。</span><span class="sxs-lookup"><span data-stu-id="e20c6-218">Let's change this so that instead of plain text we instead have the Genre names link to the appropriate /Store/Browse URL, so that clicking on a music genre like "Disco" will navigate to the /Store/Browse?genre=Disco URL.</span></span> <span data-ttu-id="e20c6-219">我們無法更新我們 \Views\Store\Index.cshtml 檢視範本以使用這些連結的程式碼類似如下的輸出 **（不要鍵入中-我們將在其上改善）**:</span><span class="sxs-lookup"><span data-stu-id="e20c6-219">We could update our \Views\Store\Index.cshtml View template to output these links using code like below **(don't type this in - we're going to improve on it)**:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

<span data-ttu-id="e20c6-220">這的確行得通，但可能會導致稍後疑難排解，因為它依賴硬式編碼字串。</span><span class="sxs-lookup"><span data-stu-id="e20c6-220">That works, but it could lead to trouble later since it relies on a hardcoded string.</span></span> <span data-ttu-id="e20c6-221">比方說，如果我們想要重新命名該控制站時，我們需要透過我們的程式碼需要更新的連結尋找搜尋。</span><span class="sxs-lookup"><span data-stu-id="e20c6-221">For instance, if we wanted to rename the Controller, we'd need to search through our code looking for links that need to be updated.</span></span>

<span data-ttu-id="e20c6-222">我們可以使用替代方法是利用 HTML 協助程式方法。</span><span class="sxs-lookup"><span data-stu-id="e20c6-222">An alternative approach we can use is to take advantage of an HTML Helper method.</span></span> <span data-ttu-id="e20c6-223">ASP.NET MVC 包含可從我們的檢視範本程式碼，以執行各種常見的工作，就像這樣的 HTML Helper 方法。</span><span class="sxs-lookup"><span data-stu-id="e20c6-223">ASP.NET MVC includes HTML Helper methods which are available from our View template code to perform a variety of common tasks just like this.</span></span> <span data-ttu-id="e20c6-224">Html.ActionLink() helper 方法是一個特別有用，並可讓您更輕鬆建置 HTML &lt;&gt;連結，而惱人的細節，例如確定具有正確的 URL 路徑 URL 編碼。</span><span class="sxs-lookup"><span data-stu-id="e20c6-224">The Html.ActionLink() helper method is a particularly useful one, and makes it easy to build HTML &lt;a&gt; links and takes care of annoying details like making sure URL paths are properly URL encoded.</span></span>

<span data-ttu-id="e20c6-225">Html.ActionLink() 有數個不同的多載，可讓您指定您的連結所需的資訊越。</span><span class="sxs-lookup"><span data-stu-id="e20c6-225">Html.ActionLink() has several different overloads to allow specifying as much information as you need for your links.</span></span> <span data-ttu-id="e20c6-226">在最簡單的情況下，您會提供連結文字和用戶端上按下超連結時，請移至動作方法。</span><span class="sxs-lookup"><span data-stu-id="e20c6-226">In the simplest case, you'll supply just the link text and the Action method to go to when the hyperlink is clicked on the client.</span></span> <span data-ttu-id="e20c6-227">比方說，我們可以連結至"/ Store /"連結文字 「 移至存放區索引 」 使用下列呼叫存放區詳細資料頁面上的 index （） 方法：</span><span class="sxs-lookup"><span data-stu-id="e20c6-227">For example, we can link to "/Store/" Index() method on the Store Details page with the link text "Go to the Store Index" using the following call:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

<span data-ttu-id="e20c6-228">*注意： 在此情況下，我們不需要指定控制器名稱，因為我們只連結至目前的檢視形式呈現到相同控制器內的另一個動作。*</span><span class="sxs-lookup"><span data-stu-id="e20c6-228">*Note: In this case, we didn't need to specify the controller name because we're just linking to another action within the same controller that's rendering the current view.*</span></span>

<span data-ttu-id="e20c6-229">因此我們將使用另一個多載會採用三個參數的 Html.ActionLink 方法傳遞參數，不過，需要我們瀏覽 頁面的連結：</span><span class="sxs-lookup"><span data-stu-id="e20c6-229">Our links to the Browse page will need to pass a parameter, though, so we'll use another overload of the Html.ActionLink method that takes three parameters:</span></span>

- 1. <span data-ttu-id="e20c6-230">連結文字，會顯示的內容類型名稱</span><span class="sxs-lookup"><span data-stu-id="e20c6-230">Link text, which will display the Genre name</span></span>
- 2. <span data-ttu-id="e20c6-231">控制器動作名稱 （瀏覽）</span><span class="sxs-lookup"><span data-stu-id="e20c6-231">Controller action name (Browse)</span></span>
- 3. <span data-ttu-id="e20c6-232">路由參數值，指定的名稱 （內容類型） 和值 （內容類型名稱）</span><span class="sxs-lookup"><span data-stu-id="e20c6-232">Route parameter values, specifying both the name (Genre) and the value (Genre name)</span></span>

<span data-ttu-id="e20c6-233">將放在一起，以下我們要如何撰寫這些連結至存放區索引 檢視：</span><span class="sxs-lookup"><span data-stu-id="e20c6-233">Putting that all together, here's how we'll write those links to the Store Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

<span data-ttu-id="e20c6-234">現在當我們再次執行我們的專案，並存取 /Store/ URL，我們會看到內容類型的清單。</span><span class="sxs-lookup"><span data-stu-id="e20c6-234">Now when we run our project again and access the /Store/ URL we will see a list of genres.</span></span> <span data-ttu-id="e20c6-235">按下時，每個內容類型為超連結 – 它會把我們帶到我們/Store/瀏覽？ 內容類型 =*[內容類型]* URL。</span><span class="sxs-lookup"><span data-stu-id="e20c6-235">Each genre is a hyperlink – when clicked it will take us to our /Store/Browse?genre=*[genre]* URL.</span></span>

![](mvc-music-store-part-3/_static/image3.jpg)

<span data-ttu-id="e20c6-236">如需內容類型清單的 HTML 如下所示：</span><span class="sxs-lookup"><span data-stu-id="e20c6-236">The HTML for the genre list looks like this:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]


> [!div class="step-by-step"]
> <span data-ttu-id="e20c6-237">[上一頁](mvc-music-store-part-2.md)
> [下一頁](mvc-music-store-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="e20c6-237">[Previous](mvc-music-store-part-2.md)
[Next](mvc-music-store-part-4.md)</span></span>
