---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: 第 3 部分： 檢視和 ViewModels |Microsoft 文件
author: jongalloway
description: 此教學課程系列詳細列出所有建置 ASP.NET MVC 商店範例應用程式所採取的步驟。 第 3 部分涵蓋檢視和 ViewModels。
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 497c2898db2e03b0650982c3ad1e6b5ac5ca639d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874846"
---
<a name="part-3-views-and-viewmodels"></a><span data-ttu-id="7f8fd-104">第 3 部分： 檢視和 ViewModels</span><span class="sxs-lookup"><span data-stu-id="7f8fd-104">Part 3: Views and ViewModels</span></span>
====================
<span data-ttu-id="7f8fd-105">由[Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="7f8fd-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="7f8fd-106">MVC Music Store 是介紹並逐步說明如何使用 ASP.NET MVC 和 Visual Studio 針對 web 程式開發的教學課程應用程式。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="7f8fd-107">MVC Music Store 是輕量級範例存放區實作銷售音樂專輯上線，並實作基本的網站管理、 使用者登入和購物車的功能。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="7f8fd-108">此教學課程系列詳細列出所有建置 ASP.NET MVC 商店範例應用程式所採取的步驟。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="7f8fd-109">第 3 部分涵蓋檢視和 ViewModels。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-109">Part 3 covers Views and ViewModels.</span></span>


<span data-ttu-id="7f8fd-110">到目前為止我們所只已傳回字串從控制器動作。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-110">So far we've just been returning strings from controller actions.</span></span> <span data-ttu-id="7f8fd-111">這是不錯的方法，以了解如何使用的控制站，但不是想要如何建置實際的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-111">That's a nice way to get an idea of how controllers work, but it's not how you'd want to build a real web application.</span></span> <span data-ttu-id="7f8fd-112">我們想要更好的方法來回到瀏覽我們的網站的瀏覽器產生 HTML – 其中一個，我們可以使用範本檔案，來更輕鬆地自訂 HTML 內容送回。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-112">We are going to want a better way to generate HTML back to browsers visiting our site – one where we can use template files to more easily customize the HTML content send back.</span></span> <span data-ttu-id="7f8fd-113">這只是檢視的。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-113">That's exactly what Views do.</span></span>

## <a name="adding-a-view-template"></a><span data-ttu-id="7f8fd-114">新增檢視範本</span><span class="sxs-lookup"><span data-stu-id="7f8fd-114">Adding a View template</span></span>

<span data-ttu-id="7f8fd-115">若要使用檢視範本，我們將變更傳回 ActionResult，HomeController 索引方法，並讓它傳回 View()，例如，以下：</span><span class="sxs-lookup"><span data-stu-id="7f8fd-115">To use a view-template, we'll change the HomeController Index method to return an ActionResult, and have it return View(), like below:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

<span data-ttu-id="7f8fd-116">上述變更表示而不是傳回字串，我們改為想要使用 「 檢視 」 來產生結果。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-116">The above change indicates that instead of returned a string, we instead want to use a "View" to generate a result back.</span></span>

<span data-ttu-id="7f8fd-117">我們現在會將適當的檢視表範本加入專案中。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-117">We'll now add an appropriate View template to our project.</span></span> <span data-ttu-id="7f8fd-118">若要這樣做我們將定位文字內的資料指標的索引動作方法，然後以滑鼠右鍵按一下並選取 [新增檢視]。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-118">To do this we'll position the text cursor within the Index action method, then right-click and select "Add View".</span></span> <span data-ttu-id="7f8fd-119">這會顯示 [新增檢視] 對話方塊：</span><span class="sxs-lookup"><span data-stu-id="7f8fd-119">This will bring up the Add View dialog:</span></span>

<span data-ttu-id="7f8fd-120">![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7f8fd-120">![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)</span></span>

<span data-ttu-id="7f8fd-121">[新增檢視] 對話方塊可讓我們快速且輕鬆地產生檢視範本檔案。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-121">The "Add View" dialog allows us to quickly and easily generate View template files.</span></span> <span data-ttu-id="7f8fd-122">根據預設 [新增檢視] 對話方塊會預先填入要使其符合使用它的動作方法所建立之檢視範本的名稱。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-122">By default the "Add View" dialog pre-populates the name of the View template to create so that it matches the action method that will use it.</span></span> <span data-ttu-id="7f8fd-123">因為我們使用 [新增檢視] 內容功能表，我們 HomeController 的 index 動作方法內，上述的 [新增檢視] 對話方塊會預先填入預設的檢視名稱與具有 「 索引 」。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-123">Because we used the "Add View" context menu within the Index() action method of our HomeController, the "Add View" dialog above has "Index" as the view name pre-populated by default.</span></span> <span data-ttu-id="7f8fd-124">我們不需要變更任何選項，此對話方塊上，因此請按 [新增] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-124">We don't need to change any of the options on this dialog, so click the Add button.</span></span>

<span data-ttu-id="7f8fd-125">當我們按一下 [新增] 按鈕時，Visual Web Developer 會建立新的 Index.cshtml 檢視範本我們在 \Views\Home 目錄中，如果建立此資料夾不存在。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-125">When we click the Add button, Visual Web Developer will create a new Index.cshtml view template for us in the \Views\Home directory, creating the folder if doesn't already exist.</span></span>

![](mvc-music-store-part-3/_static/image2.png)

<span data-ttu-id="7f8fd-126">"Index.cshtml 」 檔案的名稱和資料夾位置是很重要，並會遵循預設 ASP.NET MVC 的命名慣例。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-126">The name and folder location of the "Index.cshtml" file is important, and follows the default ASP.NET MVC naming conventions.</span></span> <span data-ttu-id="7f8fd-127">目錄名稱、 \Views\Home，符合的控制站-名為 HomeController。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-127">The directory name, \Views\Home, matches the controller - which is named HomeController.</span></span> <span data-ttu-id="7f8fd-128">檢視的範本名稱，索引中，符合控制器動作方法，這個方法會將所顯示的檢視。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-128">The view template name, Index, matches the controller action method which will be displaying the view.</span></span>

<span data-ttu-id="7f8fd-129">ASP.NET MVC 可讓我們避免我們使用此命名慣例，傳回檢視時，明確指定的名稱或檢視表範本的位置。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-129">ASP.NET MVC allows us to avoid having to explicitly specify the name or location of a view template when we use this naming convention to return a view.</span></span> <span data-ttu-id="7f8fd-130">它會根據預設轉譯 \Views\Home\Index.cshtml 檢視範本當我們我們 HomeController 內撰寫類似如下的程式碼：</span><span class="sxs-lookup"><span data-stu-id="7f8fd-130">It will by default render the \Views\Home\Index.cshtml view template when we write code like below within our HomeController:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

<span data-ttu-id="7f8fd-131">Visual Web Developer 中建立，並開啟 「 Index.cshtml 」 檢視範本之後我們按一下 [新增] 按鈕，在 [新增檢視] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-131">Visual Web Developer created and opened the "Index.cshtml" view template after we clicked the "Add" button within the "Add View" dialog.</span></span> <span data-ttu-id="7f8fd-132">Index.cshtml 的內容如下所示。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-132">The contents of Index.cshtml are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

<span data-ttu-id="7f8fd-133">此檢視使用 Razor 語法，也就是更簡潔比 ASP.NET Web Form 和 ASP.NET MVC 的先前版本中使用 Web Form 檢視引擎。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-133">This view is using the Razor syntax, which is more concise than the Web Forms view engine used in ASP.NET Web Forms and previous versions of ASP.NET MVC.</span></span> <span data-ttu-id="7f8fd-134">Web Form 檢視引擎仍然可以使用在 ASP.NET MVC 3 中，但許多開發人員認為 Razor 檢視引擎會很適合開發 ASP.NET MVC。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-134">The Web Forms view engine is still available in ASP.NET MVC 3, but many developers find that the Razor view engine fits ASP.NET MVC development really well.</span></span>

<span data-ttu-id="7f8fd-135">前三個行設定使用 ViewBag.Title 頁面標題。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-135">The first three lines set the page title using ViewBag.Title.</span></span> <span data-ttu-id="7f8fd-136">我們會查看其運作方式的詳細推出，但首先讓我們來更新文字標題文字，然後檢視頁面。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-136">We'll look at how this works in more detail soon, but first let's update the text heading text and view the page.</span></span> <span data-ttu-id="7f8fd-137">更新&lt;h2&gt;說出 「 這是 Home Page"，如下所示的標記。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-137">Update the &lt;h2&gt; tag to say "This is the Home Page" as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

<span data-ttu-id="7f8fd-138">執行應用程式會顯示我們新的文字已顯示在 [首頁] 頁面上。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-138">Running the application shows that our new text is visible on the home page.</span></span>

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a><span data-ttu-id="7f8fd-139">對於常見的站台元素使用的版面配置</span><span class="sxs-lookup"><span data-stu-id="7f8fd-139">Using a Layout for common site elements</span></span>

<span data-ttu-id="7f8fd-140">大多數的網站有多個頁面之間共用的內容： 瀏覽、 頁尾、 標誌影像、 樣式表參考等。Razor 檢視引擎簡化這要使用一個稱為管理\_Layout.cshtml 自動/檢視表/Shared 資料夾內建立的。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-140">Most websites have content which is shared between many pages: navigation, footers, logo images, stylesheet references, etc. The Razor view engine makes this easy to manage using a page called \_Layout.cshtml which has automatically been created for us inside the /Views/Shared folder.</span></span>

![](mvc-music-store-part-3/_static/image4.png)

<span data-ttu-id="7f8fd-141">按兩下此資料夾來檢視內容，如下所示的。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-141">Double-click on this folder to view the contents, which are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

<span data-ttu-id="7f8fd-142">從我們的個別檢視的內容會由顯示@RenderBody（） 命令，以及任何通用的內容，我們想要出現在之外，可以加入至\_Layout.cshtml 標記。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-142">The content from our individual views will be displayed by the @RenderBody() command, and any common content that we want to appear outside of that can be added to the \_Layout.cshtml markup.</span></span> <span data-ttu-id="7f8fd-143">我們希望我們首頁並儲存區的區域，在網站中，所有頁面上必須具有連結的通用標頭，因此我們會將其加入至上方的範本我們 MVC Music Store @RenderBody（） 陳述式。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-143">We'll want our MVC Music Store to have a common header with links to our Home page and Store area on all pages in the site, so we'll add that to the template directly above that @RenderBody() statement.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a><span data-ttu-id="7f8fd-144">更新 樣式表</span><span class="sxs-lookup"><span data-stu-id="7f8fd-144">Updating the StyleSheet</span></span>

<span data-ttu-id="7f8fd-145">空白專案範本包含非常簡化的 CSS 檔案只包含用來顯示驗證訊息的樣式。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-145">The empty project template includes a very streamlined CSS file which just includes styles used to display validation messages.</span></span> <span data-ttu-id="7f8fd-146">某些其他 CSS 和圖像來定義外觀與風格，我們站台上，因此我們會將那些現在提供我們的設計工具。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-146">Our designer has provided some additional CSS and images to define the look and feel for our site, so we'll add those in now.</span></span>

<span data-ttu-id="7f8fd-147">更新 CSS 檔案和映像會包含在 MvcMusicStore Assets.zip，可在內容目錄[MVC Music Store](https://github.com/evilDave/MVC-Music-Store)。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-147">The updated CSS file and Images are included in the Content directory of MvcMusicStore-Assets.zip which is available at [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span> <span data-ttu-id="7f8fd-148">我們將在 Windows 檔案總管中選取這兩種，放到我們的解決方案內容資料夾，在 Visual Web Developer 中，如下所示：</span><span class="sxs-lookup"><span data-stu-id="7f8fd-148">We'll select both of them in Windows Explorer and drop them into our Solution's Content folder in Visual Web Developer, as shown below:</span></span>

![](mvc-music-store-part-3/_static/image5.png)

<span data-ttu-id="7f8fd-149">系統會要求您確認是否您想要覆寫現有的 Site.css 檔案。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-149">You'll be asked to confirm if you want to overwrite the existing Site.css file.</span></span> <span data-ttu-id="7f8fd-150">按一下 [是]。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-150">Click Yes.</span></span>

![](mvc-music-store-part-3/_static/image6.png)

<span data-ttu-id="7f8fd-151">您的應用程式的內容資料夾現在會出現，如下所示：</span><span class="sxs-lookup"><span data-stu-id="7f8fd-151">The Content folder of your application will now appear as follows:</span></span>

![](mvc-music-store-part-3/_static/image7.png)

<span data-ttu-id="7f8fd-152">現在讓我們來執行應用程式，並查看我們的變更在首頁上的外觀。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-152">Now let's run the application and see how our changes look on the Home page.</span></span>

![](mvc-music-store-part-3/_static/image8.png)

- <span data-ttu-id="7f8fd-153">讓我們來檢視已變更的內容： 找到 HomeController 索引動作方法，並顯示這個 \Views\Home\Index.cshtmlView 範本時，即使我們的程式碼會呼叫 「 傳回 View()"，因為接下來我們檢視範本的標準命名慣例。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-153">Let's review what's changed: The HomeController's Index action method found and displayed the \Views\Home\Index.cshtmlView template, even though our code called "return View()", because our View template followed the standard naming convention.</span></span>
- <span data-ttu-id="7f8fd-154">首頁會顯示簡單的歡迎訊息 \Views\Home\Index.cshtml 檢視範本內定義。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-154">The Home Page is displaying a simple welcome message that is defined within the \Views\Home\Index.cshtml view template.</span></span>
- <span data-ttu-id="7f8fd-155">[首頁] 使用我們\_Layout.cshtml 範本，因此歡迎訊息是否包含在標準網站 HTML 版面配置。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-155">The Home Page is using our \_Layout.cshtml template, and so the welcome message is contained within the standard site HTML layout.</span></span>

## <a name="using-a-model-to-pass-information-to-our-view"></a><span data-ttu-id="7f8fd-156">使用模型來傳遞資訊給我們的檢視</span><span class="sxs-lookup"><span data-stu-id="7f8fd-156">Using a Model to pass information to our View</span></span>

<span data-ttu-id="7f8fd-157">只會顯示硬式編碼的 HTML 檢視範本不要讓非常有趣的網站。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-157">A View template that just displays hardcoded HTML isn't going to make a very interesting web site.</span></span> <span data-ttu-id="7f8fd-158">若要建立動態網站，我們改為要從我們的控制器動作傳遞資訊至我們檢視範本。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-158">To create a dynamic web site, we'll instead want to pass information from our controller actions to our view templates.</span></span>

<span data-ttu-id="7f8fd-159">在模型-檢視-控制器模式中，一詞是指的模型物件代表應用程式中的資料。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-159">In the Model-View-Controller pattern, the term Model refers to objects which represent the data in the application.</span></span> <span data-ttu-id="7f8fd-160">通常，模型物件對應至您的資料庫中的資料表，但它們不需要。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-160">Often, model objects correspond to tables in your database, but they don't have to.</span></span>

<span data-ttu-id="7f8fd-161">控制器動作方法會傳回 ActionResult 可以傳遞至檢視的模型物件。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-161">Controller action methods which return an ActionResult can pass a model object to the view.</span></span> <span data-ttu-id="7f8fd-162">這讓控制器能夠乾淨地封裝產生回應，，然後再的這項資訊傳遞至檢視表範本所需的所有資訊用來產生適當的 HTML 回應。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-162">This allows a Controller to cleanly package up all the information needed to generate a response, and then pass this information off to a View template to use to generate the appropriate HTML response.</span></span> <span data-ttu-id="7f8fd-163">這是最容易了解藉由查看作用中，因此讓我們開始吧。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-163">This is easiest to understand by seeing it in action, so let's get started.</span></span>

<span data-ttu-id="7f8fd-164">首先，我們將建立來代表我們的存放區中的內容類型和專輯某些模型類別。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-164">First we'll create some Model classes to represent Genres and Albums within our store.</span></span> <span data-ttu-id="7f8fd-165">首先建立內容類型的類別。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-165">Let's start by creating a Genre class.</span></span> <span data-ttu-id="7f8fd-166">以滑鼠右鍵按一下您專案中的 「 模型 」 資料夾，選擇 [新增類別]，並將檔案"Genre.cs"。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-166">Right-click the "Models" folder within your project, choose the "Add Class" option, and name the file "Genre.cs".</span></span>

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

<span data-ttu-id="7f8fd-167">然後加入所建立的類別公開的字串名稱屬性：</span><span class="sxs-lookup"><span data-stu-id="7f8fd-167">Then add a public string Name property to the class that was created:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

<span data-ttu-id="7f8fd-168">*注意： 在您想知道，案例 {get; 設定;} 標記法正在使用 C# 的自動實作屬性功能。這會提供屬性的優點而不需要我們宣告支援欄位。*</span><span class="sxs-lookup"><span data-stu-id="7f8fd-168">*Note: In case you're wondering, the { get; set; } notation is making use of C#'s auto-implemented properties feature. This gives us the benefits of a property without requiring us to declare a backing field.*</span></span>

<span data-ttu-id="7f8fd-169">接下來，請遵循相同的步驟，建立具有標題和內容類型屬性的專輯類別 （名稱為 Album.cs）：</span><span class="sxs-lookup"><span data-stu-id="7f8fd-169">Next, follow the same steps to create an Album class (named Album.cs) that has a Title and a Genre property:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

<span data-ttu-id="7f8fd-170">現在我們可以修改 StoreController，若要使用檢視顯示我們的模型中的動態資訊。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-170">Now we can modify the StoreController to use Views which display dynamic information from our Model.</span></span> <span data-ttu-id="7f8fd-171">如果-供示範之用現在-我們名為我們的要求識別碼為基礎的相簿，我們無法顯示與下列在檢視該資訊。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-171">If - for demonstration purposes right now - we named our Albums based on the request ID, we could display that information as in the view below.</span></span>

![](mvc-music-store-part-3/_static/image10.png)

<span data-ttu-id="7f8fd-172">我們會先變更存放區詳細資料動作，因此它會顯示單一專輯的資訊。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-172">We'll start by changing the Store Details action so it shows the information for a single album.</span></span> <span data-ttu-id="7f8fd-173">將"using"陳述式加入至頂端**StoreControllers**類別，以包含 MvcMusicStore.Models 命名空間中，所以我們不需要輸入 MvcMusicStore.Models.Album 每當我們想要使用的專輯類別。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-173">Add a "using" statement to the top of the **StoreControllers** class to include the MvcMusicStore.Models namespace, so we don't need to type MvcMusicStore.Models.Album every time we want to use the album class.</span></span> <span data-ttu-id="7f8fd-174">該類別的"using"區段現在應該會顯示如下。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-174">The "usings" section of that class should now appear as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

<span data-ttu-id="7f8fd-175">接下來，我們將更新詳細資料控制器動作，使它傳回 ActionResult，而不是字串，以 HomeController 索引方法。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-175">Next, we'll update the Details controller action so that it returns an ActionResult rather than a string, as we did with the HomeController's Index method.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

<span data-ttu-id="7f8fd-176">現在我們可以修改專輯物件傳回至檢視的邏輯。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-176">Now we can modify the logic to return an Album object to the view.</span></span> <span data-ttu-id="7f8fd-177">稍後在本教學課程中我們將會從資料庫擷取資料 –，但現在我們將使用 「 虛擬資料 「 若要開始使用。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-177">Later in this tutorial we will be retrieving the data from a database – but for right now we will use "dummy data" to get started.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

<span data-ttu-id="7f8fd-178">*注意： 如果您熟悉 C#，您可能會假設，使用 var 表示我們專輯變數是晚期繫結。不正確-C# 編譯器使用類型推斷根據我們要指派給變數，以判斷該專輯屬於型別專輯和專輯本機變數編譯成專輯型別，因此我們會發生編譯時期檢查和 Visual Studio 程式碼編輯器支援。*</span><span class="sxs-lookup"><span data-stu-id="7f8fd-178">*Note: If you're unfamiliar with C#, you may assume that using var means that our album variable is late-bound. That's not correct – the C# compiler is using type-inference based on what we're assigning to the variable to determine that album is of type Album and compiling the local album variable as an Album type, so we get compile-time checking and Visual Studio code-editor support.*</span></span>

<span data-ttu-id="7f8fd-179">我們現在來建立使用我們的專輯產生 HTML 回應的檢視範本。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-179">Let's now create a View template that uses our Album to generate an HTML response.</span></span> <span data-ttu-id="7f8fd-180">我們這樣做，我們需要先建置專案，加入檢視對話方塊知道我們新建的專輯類別。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-180">Before we do that we need to build the project so that the Add View dialog knows about our newly created Album class.</span></span> <span data-ttu-id="7f8fd-181">您可以建置專案選取 Debug⇨Build MvcMusicStore 功能表項目 （額外的信用額度，您可以使用 Ctrl Shift B 捷徑來建置專案）。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-181">You can build the project by selecting the Debug⇨Build MvcMusicStore menu item (for extra credit, you can use the Ctrl-Shift-B shortcut to build the project).</span></span>

![](mvc-music-store-part-3/_static/image11.png)

<span data-ttu-id="7f8fd-182">現在我們設定我們支援的類別，我們已準備好要建置我們檢視的範本。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-182">Now that we've set up our supporting classes, we're ready to build our View template.</span></span> <span data-ttu-id="7f8fd-183">以滑鼠右鍵按一下詳細資料的方法內，然後選取 [新增檢視]</span><span class="sxs-lookup"><span data-stu-id="7f8fd-183">Right-click within the Details method and select "Add View…"</span></span> <span data-ttu-id="7f8fd-184">從操作功能表。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-184">from the context menu.</span></span>

![](mvc-music-store-part-3/_static/image12.png)

<span data-ttu-id="7f8fd-185">我們將建立新的檢視範本像我們之前使用 HomeController。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-185">We are going to create a new View template like we did before with the HomeController.</span></span> <span data-ttu-id="7f8fd-186">因為我們要建立它從 StoreController 它將會根據預設會產生 \Views\Store\Index.cshtml 檔案中。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-186">Because we are creating it from the StoreController it will by default be generated in a \Views\Store\Index.cshtml file.</span></span>

<span data-ttu-id="7f8fd-187">不像之前，我們選取 [建立強型別] 檢視的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-187">Unlike before, we are going to check the "Create a strongly-typed" view checkbox.</span></span> <span data-ttu-id="7f8fd-188">然後我們選取 [檢視資料的類別] 卸除 downlist 內我們"專輯 」 類別。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-188">We are then going to select our "Album" class within the "View data-class" drop-downlist.</span></span> <span data-ttu-id="7f8fd-189">這會導致建立檢視範本所預期的相簿，物件會傳遞，以便使用 [新增檢視] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-189">This will cause the "Add View" dialog to create a View template that expects that an Album object will be passed to it to use.</span></span>

![](mvc-music-store-part-3/_static/image13.png)

<span data-ttu-id="7f8fd-190">我們按一下 [新增] 按鈕將會建立包含下列程式碼我們 \Views\Store\Details.cshtml 檢視範本。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-190">When we click the "Add" button our \Views\Store\Details.cshtml View template will be created, containing the following code.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

<span data-ttu-id="7f8fd-191">請注意第一行中，這表示此檢視為強型別到專輯類別。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-191">Notice the first line, which indicates that this view is strongly-typed to our Album class.</span></span> <span data-ttu-id="7f8fd-192">Razor 檢視引擎了解，它已經通過專輯物件，因此我們可以輕鬆地存取模型屬性，即使已在 Visual Web Developer 編輯器中 IntelliSense 的優點。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-192">The Razor view engine understands that it has been passed an Album object, so we can easily access model properties and even have the benefit of IntelliSense in the Visual Web Developer editor.</span></span>

<span data-ttu-id="7f8fd-193">更新&lt;h2&gt;標記，讓它將顯示的專輯標題屬性修改該一行出現，如下所示。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-193">Update the &lt;h2&gt; tag so it displays the Album's Title property by modifying that line to appear as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

<span data-ttu-id="7f8fd-194">請注意當您輸入超過此時限後的，會觸發 IntelliSense@Model關鍵字，顯示的屬性和專輯類別支援的方法。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-194">Notice that IntelliSense is triggered when you enter the period after the @Model keyword, showing the properties and methods that the Album class supports.</span></span>

<span data-ttu-id="7f8fd-195">我們現在重新執行受測專案，並瀏覽市集/詳細資料/5 URL。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-195">Let's now re-run our project and visit the /Store/Details/5 URL.</span></span> <span data-ttu-id="7f8fd-196">我們會看到類似下方的相簿的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-196">We'll see details of an Album like below.</span></span>

![](mvc-music-store-part-3/_static/image14.png)

<span data-ttu-id="7f8fd-197">現在我們要在存放區瀏覽動作方法的類似更新。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-197">Now we'll make a similar update for the Store Browse action method.</span></span> <span data-ttu-id="7f8fd-198">Update 方法，以便傳回 ActionResult，並修改方法邏輯，以便建立新的內容類型物件並傳回至檢視。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-198">Update the method so it returns an ActionResult, and modify the method logic so it creates a new Genre object and returns it to the View.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

<span data-ttu-id="7f8fd-199">以滑鼠右鍵按一下 瀏覽方法中，選取 新增檢視</span><span class="sxs-lookup"><span data-stu-id="7f8fd-199">Right-click in the Browse method and select "Add View…"</span></span> <span data-ttu-id="7f8fd-200">從內容功能表，然後新增是強型別檢視的強型別加入內容類型的類別。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-200">from the context menu, then add a View that is strongly-typed add a strongly typed to the Genre class.</span></span>

![](mvc-music-store-part-3/_static/image15.png)

<span data-ttu-id="7f8fd-201">更新&lt;h2&gt;檢視中的項目中的程式碼 （/Views/Store/Browse.cshtml) 以顯示類型資訊。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-201">Update the &lt;h2&gt; element in the view code (in /Views/Store/Browse.cshtml) to display the Genre information.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

<span data-ttu-id="7f8fd-202">現在讓我們重新執行受測專案，並瀏覽至存放區/瀏覽？內容類型 = Disco URL。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-202">Now let's re-run our project and browse to the /Store/Browse?Genre=Disco URL.</span></span> <span data-ttu-id="7f8fd-203">我們會看到顯示類似如下的 [瀏覽] 頁面。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-203">We'll see the Browse page displayed like below.</span></span>

![](mvc-music-store-part-3/_static/image16.png)

<span data-ttu-id="7f8fd-204">最後，讓我們進行稍微更為複雜的更新**存放區索引**動作方法並檢視我們的存放區中顯示的所有內容類型清單。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-204">Finally, let's make a slightly more complex update to the **Store Index** action method and view to display a list of all the Genres in our store.</span></span> <span data-ttu-id="7f8fd-205">我們會使用執行此動作清單的內容類型為我們的模型物件，而不只單一內容類型。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-205">We'll do that by using a List of Genres as our model object, rather than just a single Genre.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

<span data-ttu-id="7f8fd-206">存放區索引動作方法中以滑鼠右鍵按一下，然後選取 新增檢視之前，請選取內容類型為模型類別，然後按 新增 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-206">Right-click in the Store Index action method and select Add View as before, select Genre as the Model class, and press the Add button.</span></span>

![](mvc-music-store-part-3/_static/image17.png)

<span data-ttu-id="7f8fd-207">我們將會在第一次變更@model而不是其中一個物件，表示檢視將會需要數個內容類型的宣告。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-207">First we'll change the @model declaration to indicate that the view will be expecting several Genre objects rather than just one.</span></span> <span data-ttu-id="7f8fd-208">變更 /Store/Index.cshtml 讀取，如下所示的第一行：</span><span class="sxs-lookup"><span data-stu-id="7f8fd-208">Change the first line of /Store/Index.cshtml to read as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

<span data-ttu-id="7f8fd-209">這會告訴 Razor 檢視引擎將會使用可以保存數個內容類型物件的模型物件。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-209">This tells the Razor view engine that it will be working with a model object that can hold several Genre objects.</span></span> <span data-ttu-id="7f8fd-210">我們會使用 IEnumerable&lt;類型&gt;而不是清單&lt;類型&gt;因為這是一般，讓我們將稍後我們模型型別變更為支援 IEnumerable 介面的物件類型。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-210">We're using an IEnumerable&lt;Genre&gt; rather than a List&lt;Genre&gt; since it's more generic, allowing us to change our model type later to any object type that supports the IEnumerable interface.</span></span>

<span data-ttu-id="7f8fd-211">接下來，我們會執行迴圈內容類型中的物件模型，以下的檢視已完成程式碼所示。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-211">Next, we'll loop through the Genre objects in the model as shown in the completed view code below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

<span data-ttu-id="7f8fd-212">請注意，我們有完整的 IntelliSense 支援當我們輸入這個程式碼，如此當我們輸入"@Model。 」</span><span class="sxs-lookup"><span data-stu-id="7f8fd-212">Notice that we have full IntelliSense support as we enter this code, so that when we type "@Model."</span></span> <span data-ttu-id="7f8fd-213">我們會看到所有方法和屬性支援的型別內容類型的 IEnumerable。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-213">we see all methods and properties supported by an IEnumerable of type Genre.</span></span>

![](mvc-music-store-part-3/_static/image18.png)

<span data-ttu-id="7f8fd-214">"Foreach"迴圈，在 Visual Web Developer 會知道的每個項目屬於型別內容類型，因此我們會看到 IntelliSence 每個內容類型的型別。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-214">Within our "foreach" loop, Visual Web Developer knows that each item is of type Genre, so we see IntelliSence for each the Genre type.</span></span>

![](mvc-music-store-part-3/_static/image19.png)

<span data-ttu-id="7f8fd-215">接著，scaffolding 功能檢查的內容類型的物件，並決定，每個都會有名稱屬性，以便執行迴圈，並將其輸出。它也會產生每個個別項目的編輯、 詳細資料及刪除連結。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-215">Next, the scaffolding feature examined the Genre object and determined that each will have a Name property, so it loops through and writes them out. It also generates Edit, Details, and Delete links to each individual item.</span></span> <span data-ttu-id="7f8fd-216">我們將利用，稍後我們存放區管理員 中，但現在我們想要改為有一個簡單的清單。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-216">We'll take advantage of that later in our store manager, but for now we'd like to have a simple list instead.</span></span>

<span data-ttu-id="7f8fd-217">當我們執行應用程式，並瀏覽至 /Store 時，我們看到計數和內容類型的清單會顯示。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-217">When we run the application and browse to /Store, we see that both the count and list of Genres is displayed.</span></span>

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a><span data-ttu-id="7f8fd-218">新增頁之間的連結</span><span class="sxs-lookup"><span data-stu-id="7f8fd-218">Adding Links between pages</span></span>

<span data-ttu-id="7f8fd-219">列出目前的內容類型我們 /Store URL 只是以純文字格式列出內容類型名稱。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-219">Our /Store URL that lists Genres currently lists the Genre names simply as plain text.</span></span> <span data-ttu-id="7f8fd-220">讓我們來變更這個值，而不是純文字，我們改為需要內容類型名稱連結至適當的存放區/瀏覽 URL，以便讓按一下像是"Disco"會瀏覽至存放區/瀏覽音樂內容類型？ 內容類型 = Disco URL。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-220">Let's change this so that instead of plain text we instead have the Genre names link to the appropriate /Store/Browse URL, so that clicking on a music genre like "Disco" will navigate to the /Store/Browse?genre=Disco URL.</span></span> <span data-ttu-id="7f8fd-221">我們無法更新使用這些連結的程式碼下方的輸出我們 \Views\Store\Index.cshtml 檢視範本 **（不要輸入下列命令中-我們改善）**:</span><span class="sxs-lookup"><span data-stu-id="7f8fd-221">We could update our \Views\Store\Index.cshtml View template to output these links using code like below **(don't type this in - we're going to improve on it)**:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

<span data-ttu-id="7f8fd-222">此作業有效，但它可能會導致稍後疑難排解，因為它是倚賴硬式編碼字串。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-222">That works, but it could lead to trouble later since it relies on a hardcoded string.</span></span> <span data-ttu-id="7f8fd-223">比方說，如果我們想要重新命名控制器，我們需要搜尋尋找需要更新的連結的程式碼。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-223">For instance, if we wanted to rename the Controller, we'd need to search through our code looking for links that need to be updated.</span></span>

<span data-ttu-id="7f8fd-224">我們可以使用的替代方式是利用 HTML Helper 方法。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-224">An alternative approach we can use is to take advantage of an HTML Helper method.</span></span> <span data-ttu-id="7f8fd-225">ASP.NET MVC 包括可從檢視範本程式碼來執行各種不同的一般工作，就像這樣的 HTML Helper 方法。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-225">ASP.NET MVC includes HTML Helper methods which are available from our View template code to perform a variety of common tasks just like this.</span></span> <span data-ttu-id="7f8fd-226">Html.ActionLink() helper 方法是一個特別有用，可讓您更輕鬆地建立 HTML &lt;&gt;連結，並負責造成困擾的詳細資料，想要確定具有正確的 URL 路徑 URL 編碼。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-226">The Html.ActionLink() helper method is a particularly useful one, and makes it easy to build HTML &lt;a&gt; links and takes care of annoying details like making sure URL paths are properly URL encoded.</span></span>

<span data-ttu-id="7f8fd-227">Html.ActionLink() 有數個不同的多載可讓您指定所需的連結資訊。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-227">Html.ActionLink() has several different overloads to allow specifying as much information as you need for your links.</span></span> <span data-ttu-id="7f8fd-228">在最簡單的情況下，您會提供連結文字和用戶端上按下超連結時，移至動作方法。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-228">In the simplest case, you'll supply just the link text and the Action method to go to when the hyperlink is clicked on the client.</span></span> <span data-ttu-id="7f8fd-229">例如，我們可以連結至 「 / 存放區 /"index （） 方法，在存放區詳細資料 頁面上，以連結文字 「 移至存放區索引 」 中使用下列呼叫：</span><span class="sxs-lookup"><span data-stu-id="7f8fd-229">For example, we can link to "/Store/" Index() method on the Store Details page with the link text "Go to the Store Index" using the following call:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

<span data-ttu-id="7f8fd-230">*注意： 在此情況下，我們不需要指定控制器名稱，因為我們只連結到相同的控制站，會呈現目前檢視中的另一個動作。*</span><span class="sxs-lookup"><span data-stu-id="7f8fd-230">*Note: In this case, we didn't need to specify the controller name because we're just linking to another action within the same controller that's rendering the current view.*</span></span>

<span data-ttu-id="7f8fd-231">我們瀏覽 頁面的連結需要傳遞參數，不過，因此我們將使用另一個方法多載，Html.ActionLink 採用三個參數：</span><span class="sxs-lookup"><span data-stu-id="7f8fd-231">Our links to the Browse page will need to pass a parameter, though, so we'll use another overload of the Html.ActionLink method that takes three parameters:</span></span>

- 1. <span data-ttu-id="7f8fd-232">連結文字，將會顯示類型名稱</span><span class="sxs-lookup"><span data-stu-id="7f8fd-232">Link text, which will display the Genre name</span></span>
- 2. <span data-ttu-id="7f8fd-233">控制器的動作名稱 （瀏覽）</span><span class="sxs-lookup"><span data-stu-id="7f8fd-233">Controller action name (Browse)</span></span>
- 3. <span data-ttu-id="7f8fd-234">路由參數值，指定的名稱 （類型） 和值 （類型名稱）</span><span class="sxs-lookup"><span data-stu-id="7f8fd-234">Route parameter values, specifying both the name (Genre) and the value (Genre name)</span></span>

<span data-ttu-id="7f8fd-235">將放，同時，以下我們要如何撰寫這些連結到存放區索引檢視：</span><span class="sxs-lookup"><span data-stu-id="7f8fd-235">Putting that all together, here's how we'll write those links to the Store Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

<span data-ttu-id="7f8fd-236">現在我們再次執行受測專案，而且存取 /Store/ URL 時，我們會看到內容類型的清單。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-236">Now when we run our project again and access the /Store/ URL we will see a list of genres.</span></span> <span data-ttu-id="7f8fd-237">按下時，每個內容類型會是超連結 – 花我們我們/存放區/瀏覽？ 內容類型 =*[類型]* URL。</span><span class="sxs-lookup"><span data-stu-id="7f8fd-237">Each genre is a hyperlink – when clicked it will take us to our /Store/Browse?genre=*[genre]* URL.</span></span>

![](mvc-music-store-part-3/_static/image3.jpg)

<span data-ttu-id="7f8fd-238">內容類型清單的 HTML 看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="7f8fd-238">The HTML for the genre list looks like this:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]


> [!div class="step-by-step"]
> <span data-ttu-id="7f8fd-239">[上一頁](mvc-music-store-part-2.md)
> [下一頁](mvc-music-store-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="7f8fd-239">[Previous](mvc-music-store-part-2.md)
[Next](mvc-music-store-part-4.md)</span></span>
