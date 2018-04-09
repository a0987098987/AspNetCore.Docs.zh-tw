---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: 第 2 部分： 控制站 |Microsoft 文件
author: jongalloway
description: 此教學課程系列詳細列出所有建置 ASP.NET MVC 商店範例應用程式所採取的步驟。 第 2 部分涵蓋控制站。
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: 680cdea388d9b01961bd626643c0fd91c9205ed7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="part-2-controllers"></a><span data-ttu-id="7b778-104">第 2 部分： 控制站</span><span class="sxs-lookup"><span data-stu-id="7b778-104">Part 2: Controllers</span></span>
====================
<span data-ttu-id="7b778-105">由[Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="7b778-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="7b778-106">MVC Music Store 是介紹並逐步說明如何使用 ASP.NET MVC 和 Visual Studio 針對 web 程式開發的教學課程應用程式。</span><span class="sxs-lookup"><span data-stu-id="7b778-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="7b778-107">MVC Music Store 是輕量級範例存放區實作銷售音樂專輯上線，並實作基本的網站管理、 使用者登入和購物車的功能。</span><span class="sxs-lookup"><span data-stu-id="7b778-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="7b778-108">此教學課程系列詳細列出所有建置 ASP.NET MVC 商店範例應用程式所採取的步驟。</span><span class="sxs-lookup"><span data-stu-id="7b778-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="7b778-109">第 2 部分涵蓋控制站。</span><span class="sxs-lookup"><span data-stu-id="7b778-109">Part 2 covers Controllers.</span></span>


<span data-ttu-id="7b778-110">與傳統 web 架構，傳入的 Url 通常會對應至磁碟上的檔案。</span><span class="sxs-lookup"><span data-stu-id="7b778-110">With traditional web frameworks, incoming URLs are typically mapped to files on disk.</span></span> <span data-ttu-id="7b778-111">例如： URL 的要求想"/ Products.aspx"或"/ Products.php 」 可能會處理由 「 Products.aspx"或"Products.php"檔案。</span><span class="sxs-lookup"><span data-stu-id="7b778-111">For example: a request for a URL like "/Products.aspx" or "/Products.php" might be processed by a "Products.aspx" or "Products.php" file.</span></span>

<span data-ttu-id="7b778-112">網頁型 MVC 架構會稍有不同的方式，將 Url 對應至伺服端程式碼。</span><span class="sxs-lookup"><span data-stu-id="7b778-112">Web-based MVC frameworks map URLs to server code in a slightly different way.</span></span> <span data-ttu-id="7b778-113">而不是將傳入 Url 對應至檔案，它們改用對應 Url 上類別的方法。</span><span class="sxs-lookup"><span data-stu-id="7b778-113">Instead of mapping incoming URLs to files, they instead map URLs to methods on classes.</span></span> <span data-ttu-id="7b778-114">這些類別稱為 「 控制項 」 而且負責處理傳入的 HTTP 要求處理使用者輸入，就會擷取和儲存資料，以及決定要傳送的回應傳回至用戶端 （顯示 HTML、 下載檔案，將重新導向至不同的URL 等）。</span><span class="sxs-lookup"><span data-stu-id="7b778-114">These classes are called "Controllers" and they are responsible for processing incoming HTTP requests, handling user input, retrieving and saving data, and determining the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span>

## <a name="adding-a-homecontroller"></a><span data-ttu-id="7b778-115">加入 HomeController</span><span class="sxs-lookup"><span data-stu-id="7b778-115">Adding a HomeController</span></span>

<span data-ttu-id="7b778-116">我們一開始我們 MVC Music Store 的應用程式會藉由新增控制器類別處理本公司網站的首頁 Url。</span><span class="sxs-lookup"><span data-stu-id="7b778-116">We'll begin our MVC Music Store application by adding a Controller class that will handle URLs to the Home page of our site.</span></span> <span data-ttu-id="7b778-117">我們會遵循的 ASP.NET MVC 的預設命名慣例，並呼叫它 HomeController。</span><span class="sxs-lookup"><span data-stu-id="7b778-117">We'll follow the default naming conventions of ASP.NET MVC and call it HomeController.</span></span>

<span data-ttu-id="7b778-118">以滑鼠右鍵按一下 [方案總管] 中的 「 控制項 」 資料夾，並選取 [新增]，然後 [控制器]</span><span class="sxs-lookup"><span data-stu-id="7b778-118">Right-click the "Controllers" folder within the Solution Explorer and select "Add", and then the "Controller…"</span></span> <span data-ttu-id="7b778-119">命令：</span><span class="sxs-lookup"><span data-stu-id="7b778-119">command:</span></span>

![](mvc-music-store-part-2/_static/image1.jpg)

<span data-ttu-id="7b778-120">這會顯示 「 新增控制器 」 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="7b778-120">This will bring up the "Add Controller" dialog.</span></span> <span data-ttu-id="7b778-121">控制器"HomeController 」，然後按 [新增] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7b778-121">Name the controller "HomeController" and press the Add button.</span></span>

![](mvc-music-store-part-2/_static/image1.png)

<span data-ttu-id="7b778-122">這將建立新的檔案，HomeController.cs，下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="7b778-122">This will create a new file, HomeController.cs, with the following code:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

<span data-ttu-id="7b778-123">若要啟動以盡可能簡單地，我們索引以取代方法只會傳回字串的簡單方法。</span><span class="sxs-lookup"><span data-stu-id="7b778-123">To start as simply as possible, let's replace the Index method with a simple method that just returns a string.</span></span> <span data-ttu-id="7b778-124">我們將會進行兩項變更：</span><span class="sxs-lookup"><span data-stu-id="7b778-124">We'll make two changes:</span></span>

- <span data-ttu-id="7b778-125">變更傳回的字串，而不是 ActionResult 方法</span><span class="sxs-lookup"><span data-stu-id="7b778-125">Change the method to return a string instead of an ActionResult</span></span>
- <span data-ttu-id="7b778-126">變更傳回的陳述式，傳回"Hello 從 Home 」</span><span class="sxs-lookup"><span data-stu-id="7b778-126">Change the return statement to return "Hello from Home"</span></span>

<span data-ttu-id="7b778-127">此方法現在看起來應該像這樣：</span><span class="sxs-lookup"><span data-stu-id="7b778-127">The method should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a><span data-ttu-id="7b778-128">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="7b778-128">Running the Application</span></span>

<span data-ttu-id="7b778-129">現在讓我們來執行網站。</span><span class="sxs-lookup"><span data-stu-id="7b778-129">Now let's run the site.</span></span> <span data-ttu-id="7b778-130">我們可以開始我們的 web 伺服器，然後再次嘗試使用任何下列網站::</span><span class="sxs-lookup"><span data-stu-id="7b778-130">We can start our web-server and try out the site using any of the following::</span></span>

- <span data-ttu-id="7b778-131">選擇偵錯 ⇨ 開始偵錯 功能表項目</span><span class="sxs-lookup"><span data-stu-id="7b778-131">Choose the Debug ⇨ Start Debugging menu item</span></span>
- <span data-ttu-id="7b778-132">按一下工具列中的綠色箭頭按鈕 ![](mvc-music-store-part-2/_static/image2.jpg)</span><span class="sxs-lookup"><span data-stu-id="7b778-132">Click the Green arrow button in the toolbar ![](mvc-music-store-part-2/_static/image2.jpg)</span></span>
- <span data-ttu-id="7b778-133">使用鍵盤快速鍵，F5。</span><span class="sxs-lookup"><span data-stu-id="7b778-133">Use the keyboard shortcut, F5.</span></span>

<span data-ttu-id="7b778-134">使用任何上述的步驟會編譯專案，並則會導致是內建 Visual Web Developer，才能啟動 ASP.NET 程式開發伺服器。</span><span class="sxs-lookup"><span data-stu-id="7b778-134">Using any of the above steps will compile our project, and then cause the ASP.NET Development Server that is built-into Visual Web Developer to start.</span></span> <span data-ttu-id="7b778-135">表示 ASP.NET 程式開發伺服器已啟動，將螢幕的右下角會出現通知，並會用來顯示它在下執行的連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="7b778-135">A notification will appear in the bottom corner of the screen to indicate that the ASP.NET Development Server has started up, and will show the port number that it is running under.</span></span>

![](mvc-music-store-part-2/_static/image2.png)

<span data-ttu-id="7b778-136">Visual Web Developer 然後會自動開啟瀏覽器視窗，其 URL 指到我們的 web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="7b778-136">Visual Web Developer will then automatically open a browser window whose URL points to our web-server.</span></span> <span data-ttu-id="7b778-137">這可讓我們快速試用我們的 web 應用程式：</span><span class="sxs-lookup"><span data-stu-id="7b778-137">This will allow us to quickly try out our web application:</span></span>

![](mvc-music-store-part-2/_static/image3.png)

<span data-ttu-id="7b778-138">好是非常快速 – 我們建立了新的網站，加入新三價線函式，而且我們在瀏覽器中有文字。</span><span class="sxs-lookup"><span data-stu-id="7b778-138">Okay, that was pretty quick – we created a new website, added a three line function, and we've got text in a browser.</span></span> <span data-ttu-id="7b778-139">不我科學中，但它是起點。</span><span class="sxs-lookup"><span data-stu-id="7b778-139">Not rocket science, but it's a start.</span></span>

<span data-ttu-id="7b778-140">*注意： Visual Web Developer 包含 ASP.NET 程式開發伺服器，將在隨機免費 「 連接埠 」 號碼執行您的網站。在上面的螢幕擷取畫面，在執行站台`http://localhost:26641/`，因此它正在使用連接埠 26641。連接埠號碼將會不同。當我們在本教學課程談論 URL 的 like /Store/Browse，會提供通訊埠編號後面。假設 26641 的連接埠號碼，瀏覽至存放區/瀏覽代表將會瀏覽至`http://localhost:26641/Store/Browse`。*</span><span class="sxs-lookup"><span data-stu-id="7b778-140">*Note: Visual Web Developer includes the ASP.NET Development Server, which will run your website on a random free "port" number. In the screenshot above, the site is running at `http://localhost:26641/`, so it's using port 26641. Your port number will be different. When we talk about URL's like /Store/Browse in this tutorial, that will go after the port number. Assuming a port number of 26641, browsing to /Store/Browse will mean browsing to `http://localhost:26641/Store/Browse`.*</span></span>

## <a name="adding-a-storecontroller"></a><span data-ttu-id="7b778-141">加入 StoreController</span><span class="sxs-lookup"><span data-stu-id="7b778-141">Adding a StoreController</span></span>

<span data-ttu-id="7b778-142">我們加入簡單的 HomeController 實作本公司網站的首頁。</span><span class="sxs-lookup"><span data-stu-id="7b778-142">We added a simple HomeController that implements the Home Page of our site.</span></span> <span data-ttu-id="7b778-143">讓我們現在加入另一個控制站，我們將使用實作瀏覽我們的音樂存放區的功能。</span><span class="sxs-lookup"><span data-stu-id="7b778-143">Let's now add another controller that we'll use to implement the browsing functionality of our music store.</span></span> <span data-ttu-id="7b778-144">存放區 controller 將會支援三種案例：</span><span class="sxs-lookup"><span data-stu-id="7b778-144">Our store controller will support three scenarios:</span></span>

- <span data-ttu-id="7b778-145">在我們的 music store 中的音樂內容類型清單頁面</span><span class="sxs-lookup"><span data-stu-id="7b778-145">A listing page of the music genres in our music store</span></span>
- <span data-ttu-id="7b778-146">瀏覽 頁面會列出所有音樂專輯特定內容類型</span><span class="sxs-lookup"><span data-stu-id="7b778-146">A browse page that lists all of the music albums in a particular genre</span></span>
- <span data-ttu-id="7b778-147">顯示有關特定音樂專輯資訊詳細資料頁面</span><span class="sxs-lookup"><span data-stu-id="7b778-147">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="7b778-148">我們會先加入新的 StoreController 類別...</span><span class="sxs-lookup"><span data-stu-id="7b778-148">We'll start by adding a new StoreController class..</span></span> <span data-ttu-id="7b778-149">如果您還沒有這麼做，請停止執行應用程式藉由關閉瀏覽器，或選取偵錯 ⇨ 停止偵錯 功能表項目。</span><span class="sxs-lookup"><span data-stu-id="7b778-149">If you haven't already, stop running the application either by closing the browser or selecting the Debug ⇨ Stop Debugging menu item.</span></span>

<span data-ttu-id="7b778-150">現在，加入新 StoreController。</span><span class="sxs-lookup"><span data-stu-id="7b778-150">Now add a new StoreController.</span></span> <span data-ttu-id="7b778-151">如同我們在與 HomeController 會執行此作業，在 [方案總管] 中的 「 控制項 」 資料夾上按一下滑鼠右鍵，然後選擇 [新增]-&gt;控制器功能表項目</span><span class="sxs-lookup"><span data-stu-id="7b778-151">Just like we did with HomeController, we'll do this by right-clicking on the "Controllers" folder within the Solution Explorer and choosing the Add-&gt;Controller menu item</span></span>

![](mvc-music-store-part-2/_static/image4.png)

<span data-ttu-id="7b778-152">我們新 StoreController 已經有一個 「 索引 」 方法。</span><span class="sxs-lookup"><span data-stu-id="7b778-152">Our new StoreController already has an "Index" method.</span></span> <span data-ttu-id="7b778-153">若要實作我們列出在我們的 music store 中的所有內容類型的清單頁面，我們將使用此 「 索引 」 方法。</span><span class="sxs-lookup"><span data-stu-id="7b778-153">We'll use this "Index" method to implement our listing page that lists all genres in our music store.</span></span> <span data-ttu-id="7b778-154">我們也會加入兩個其他的方法，以實作兩個其他情況下我們想要處理我們 StoreController： 瀏覽和詳細資料。</span><span class="sxs-lookup"><span data-stu-id="7b778-154">We'll also add two additional methods to implement the two other scenarios we want our StoreController to handle: Browse and Details.</span></span>

<span data-ttu-id="7b778-155">會呼叫這些方法 （索引、 瀏覽和詳細資料），我們的控制器內 [控制器動作]，並且您已經看到與 HomeController.Index （） 的動作方法，其作業來回應 URL 要求，並 （一般來說，） 決定哪些內容應該傳回至瀏覽器或叫用的 URL 的使用者。</span><span class="sxs-lookup"><span data-stu-id="7b778-155">These methods (Index, Browse and Details) within our Controller are called "Controller Actions", and as you've already seen with the HomeController.Index()action method, their job is to respond to URL requests and (generally speaking) determine what content should be sent back to the browser or user that invoked the URL.</span></span>

<span data-ttu-id="7b778-156">藉由變更 theIndex() 方法以傳回字串"Hello 從 Store.Index()"，就會開始我們 StoreController 的實作，我們將新增類似的方法 Browse() 和 Details():</span><span class="sxs-lookup"><span data-stu-id="7b778-156">We'll start our StoreController implementation by changing theIndex() method to return the string "Hello from Store.Index()" and we'll add similar methods for Browse() and Details():</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

<span data-ttu-id="7b778-157">重新執行專案，然後瀏覽下列 Url:</span><span class="sxs-lookup"><span data-stu-id="7b778-157">Run the project again and browse the following URLs:</span></span>

- <span data-ttu-id="7b778-158">/Store</span><span class="sxs-lookup"><span data-stu-id="7b778-158">/Store</span></span>
- <span data-ttu-id="7b778-159">/ 存放區/瀏覽</span><span class="sxs-lookup"><span data-stu-id="7b778-159">/Store/Browse</span></span>
- <span data-ttu-id="7b778-160">/ 存放區/詳細資料</span><span class="sxs-lookup"><span data-stu-id="7b778-160">/Store/Details</span></span>

<span data-ttu-id="7b778-161">存取下列 Url 會叫用動作方法，我們的控制器內，並會傳回字串的回應：</span><span class="sxs-lookup"><span data-stu-id="7b778-161">Accessing these URLs will invoke the action methods within our Controller and return string responses:</span></span>

![](mvc-music-store-part-2/_static/image5.png)

<span data-ttu-id="7b778-162">太好了，但這些只常數字串。</span><span class="sxs-lookup"><span data-stu-id="7b778-162">That's great, but these are just constant strings.</span></span> <span data-ttu-id="7b778-163">我們把它們動態的好讓您從 URL 取得資訊並顯示在網頁輸出中。</span><span class="sxs-lookup"><span data-stu-id="7b778-163">Let's make them dynamic, so they take information from the URL and display it in the page output.</span></span>

<span data-ttu-id="7b778-164">首先，我們將會變更瀏覽動作方法，從 URL 擷取查詢字串值。</span><span class="sxs-lookup"><span data-stu-id="7b778-164">First we'll change the Browse action method to retrieve a querystring value from the URL.</span></span> <span data-ttu-id="7b778-165">我們可以將 「 類型 」 參數加入至動作方法來執行這項操作。</span><span class="sxs-lookup"><span data-stu-id="7b778-165">We can do this by adding a "genre" parameter to our action method.</span></span> <span data-ttu-id="7b778-166">當執行此作業，ASP.NET MVC 會自動傳遞叫用時，名為 「 類型 」 至我們的動作方法的任何查詢字串或表單張貼參數。</span><span class="sxs-lookup"><span data-stu-id="7b778-166">When we do this ASP.NET MVC will automatically pass any querystring or form post parameters named "genre" to our action method when it is invoked.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

<span data-ttu-id="7b778-167">*注意： 我們會使用 HttpUtility.HtmlEncode 公用程式方法來處理使用者輸入。如此可防止使用者將 Javascript 插入我們檢視以便 /Store/Browse 類似的連結嗎？內容類型 =&lt;指令碼&gt;window.location='http://hackersite.com'&lt;/指令碼&gt;。*</span><span class="sxs-lookup"><span data-stu-id="7b778-167">*Note: We're using the HttpUtility.HtmlEncode utility method to sanitize the user input. This prevents users from injecting Javascript into our View with a link like /Store/Browse?Genre=&lt;script&gt;window.location='http://hackersite.com'&lt;/script&gt;.*</span></span>

<span data-ttu-id="7b778-168">現在讓我們來瀏覽至存放區/瀏覽？內容類型 = Disco</span><span class="sxs-lookup"><span data-stu-id="7b778-168">Now let's browse to /Store/Browse?Genre=Disco</span></span>

![](mvc-music-store-part-2/_static/image6.png)

<span data-ttu-id="7b778-169">讓我們來讀取和顯示輸入的參數的詳細資料動作的下一步 變更名為識別碼。</span><span class="sxs-lookup"><span data-stu-id="7b778-169">Let's next change the Details action to read and display an input parameter named ID.</span></span> <span data-ttu-id="7b778-170">不同於我們先前的方法中，我們將不會內嵌的識別碼值做為查詢字串參數。</span><span class="sxs-lookup"><span data-stu-id="7b778-170">Unlike our previous method, we won't be embedding the ID value as a querystring parameter.</span></span> <span data-ttu-id="7b778-171">而是我們會將它內嵌直接在 URL 本身內。</span><span class="sxs-lookup"><span data-stu-id="7b778-171">Instead we'll embed it directly within the URL itself.</span></span> <span data-ttu-id="7b778-172">例如： /Store/Details/5。</span><span class="sxs-lookup"><span data-stu-id="7b778-172">For example: /Store/Details/5.</span></span>

<span data-ttu-id="7b778-173">ASP.NET MVC 可讓我們能夠輕鬆地執行這項操作而不需要設定任何項目。</span><span class="sxs-lookup"><span data-stu-id="7b778-173">ASP.NET MVC lets us easily do this without having to configure anything.</span></span> <span data-ttu-id="7b778-174">ASP.NET MVC 的預設路由慣例是在動作方法名稱後面的 URL 區段視為名為 「 識別碼 」 的參數。</span><span class="sxs-lookup"><span data-stu-id="7b778-174">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named "ID".</span></span> <span data-ttu-id="7b778-175">如果動作方法有名稱為 ID 參數則 ASP.NET MVC 會自動傳遞的 URL 區段給您做為參數。</span><span class="sxs-lookup"><span data-stu-id="7b778-175">If your action method has a parameter named ID then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

<span data-ttu-id="7b778-176">執行應用程式，並瀏覽至 /Store/Details/5:</span><span class="sxs-lookup"><span data-stu-id="7b778-176">Run the application and browse to /Store/Details/5:</span></span>

![](mvc-music-store-part-2/_static/image7.png)

<span data-ttu-id="7b778-177">讓我們先複習到目前為止完成的內容：</span><span class="sxs-lookup"><span data-stu-id="7b778-177">Let's recap what we've done so far:</span></span>

- <span data-ttu-id="7b778-178">我們已在 Visual Web Developer 中建立新的 ASP.NET MVC 專案</span><span class="sxs-lookup"><span data-stu-id="7b778-178">We've created a new ASP.NET MVC project in Visual Web Developer</span></span>
- <span data-ttu-id="7b778-179">我們已討論過 ASP.NET MVC 應用程式的基本的資料夾結構</span><span class="sxs-lookup"><span data-stu-id="7b778-179">We've discussed the basic folder structure of an ASP.NET MVC application</span></span>
- <span data-ttu-id="7b778-180">我們學到如何執行我們的網站使用 ASP.NET 程式開發伺服器</span><span class="sxs-lookup"><span data-stu-id="7b778-180">We've learned how to run our website using the ASP.NET Development Server</span></span>
- <span data-ttu-id="7b778-181">我們建立了兩個控制器類別： HomeController 和 StoreController</span><span class="sxs-lookup"><span data-stu-id="7b778-181">We've created two Controller classes: a HomeController and a StoreController</span></span>
- <span data-ttu-id="7b778-182">我們已將動作方法加入至我們控制站會回應 URL 要求，並傳回至瀏覽器的文字</span><span class="sxs-lookup"><span data-stu-id="7b778-182">We've added Action Methods to our controllers which respond to URL requests and return text to the browser</span></span>


> [!div class="step-by-step"]
> <span data-ttu-id="7b778-183">[上一頁](mvc-music-store-part-1.md)
> [下一頁](mvc-music-store-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="7b778-183">[Previous](mvc-music-store-part-1.md)
[Next](mvc-music-store-part-3.md)</span></span>
