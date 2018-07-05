---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: 第 2 部分： 控制站 |Microsoft Docs
author: jongalloway
description: 本教學課程系列會詳細說明所有建置 ASP.NET MVC Music 市集範例應用程式所採取的步驟。 第 2 部分涵蓋控制站。
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: a854feff675cae302b9927d209808257b7d087cb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37381799"
---
<a name="part-2-controllers"></a><span data-ttu-id="7219c-104">第 2 部分： 控制站</span><span class="sxs-lookup"><span data-stu-id="7219c-104">Part 2: Controllers</span></span>
====================
<span data-ttu-id="7219c-105">藉由[Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="7219c-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="7219c-106">MVC Music 市集是介紹，並逐步說明如何使用 ASP.NET MVC 和 Visual Studio 進行 web 開發的教學課程應用程式。</span><span class="sxs-lookup"><span data-stu-id="7219c-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="7219c-107">MVC Music 市集是銷售線上音樂 album 和實作基本的網站管理、 使用者登入時，和 「 購物車 」 功能的輕量級的範例存放區實作。</span><span class="sxs-lookup"><span data-stu-id="7219c-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="7219c-108">本教學課程系列會詳細說明所有建置 ASP.NET MVC Music 市集範例應用程式所採取的步驟。</span><span class="sxs-lookup"><span data-stu-id="7219c-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="7219c-109">第 2 部分涵蓋控制站。</span><span class="sxs-lookup"><span data-stu-id="7219c-109">Part 2 covers Controllers.</span></span>


<span data-ttu-id="7219c-110">有了傳統的 web 架構，傳入的 Url 通常會對應至磁碟上的檔案。</span><span class="sxs-lookup"><span data-stu-id="7219c-110">With traditional web frameworks, incoming URLs are typically mapped to files on disk.</span></span> <span data-ttu-id="7219c-111">例如： URL 的要求想"/ Products.aspx"或"/ Products.php 」 可能會由 「 Products.aspx"或"Products.php 」 檔案的方式來處理。</span><span class="sxs-lookup"><span data-stu-id="7219c-111">For example: a request for a URL like "/Products.aspx" or "/Products.php" might be processed by a "Products.aspx" or "Products.php" file.</span></span>

<span data-ttu-id="7219c-112">Web 型 MVC 架構會稍有不同的方式，將 Url 對應至伺服器程式碼。</span><span class="sxs-lookup"><span data-stu-id="7219c-112">Web-based MVC frameworks map URLs to server code in a slightly different way.</span></span> <span data-ttu-id="7219c-113">而不是將傳入的 Url 對應至檔案中，它們改為將 Url 對應至類別的方法。</span><span class="sxs-lookup"><span data-stu-id="7219c-113">Instead of mapping incoming URLs to files, they instead map URLs to methods on classes.</span></span> <span data-ttu-id="7219c-114">這些類別稱為 「 控制項 」，他們會負責處理傳入的 HTTP 要求，處理使用者輸入擷取和儲存資料，並判斷要傳送的回應傳回至用戶端 （顯示 HTML、 下載檔案、 重新導向至不同URL 等）。</span><span class="sxs-lookup"><span data-stu-id="7219c-114">These classes are called "Controllers" and they are responsible for processing incoming HTTP requests, handling user input, retrieving and saving data, and determining the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span>

## <a name="adding-a-homecontroller"></a><span data-ttu-id="7219c-115">加入 HomeController</span><span class="sxs-lookup"><span data-stu-id="7219c-115">Adding a HomeController</span></span>

<span data-ttu-id="7219c-116">首先我們 MVC Music 市集應用程式新增至我們的網站的首頁處理 Url 的控制器類別。</span><span class="sxs-lookup"><span data-stu-id="7219c-116">We'll begin our MVC Music Store application by adding a Controller class that will handle URLs to the Home page of our site.</span></span> <span data-ttu-id="7219c-117">我們將遵循的 ASP.NET MVC 的預設命名慣例，並呼叫 HomeController。</span><span class="sxs-lookup"><span data-stu-id="7219c-117">We'll follow the default naming conventions of ASP.NET MVC and call it HomeController.</span></span>

<span data-ttu-id="7219c-118">以滑鼠右鍵按一下方案總管] 內的 「 控制項 」 資料夾，然後選取 [新增]，然後按一下 [[控制器] 命令：</span><span class="sxs-lookup"><span data-stu-id="7219c-118">Right-click the "Controllers" folder within the Solution Explorer and select "Add", and then the "Controller…" command:</span></span>

![](mvc-music-store-part-2/_static/image1.jpg)

<span data-ttu-id="7219c-119">這會顯示 「 新增控制器 」 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="7219c-119">This will bring up the "Add Controller" dialog.</span></span> <span data-ttu-id="7219c-120">控制器"HomeController"命名，然後按 [新增] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7219c-120">Name the controller "HomeController" and press the Add button.</span></span>

![](mvc-music-store-part-2/_static/image1.png)

<span data-ttu-id="7219c-121">這會建立新的檔案，HomeController.cs 中，為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="7219c-121">This will create a new file, HomeController.cs, with the following code:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

<span data-ttu-id="7219c-122">若要開始以盡可能簡單地，我們取代 Index 方法只會傳回字串的簡單方法。</span><span class="sxs-lookup"><span data-stu-id="7219c-122">To start as simply as possible, let's replace the Index method with a simple method that just returns a string.</span></span> <span data-ttu-id="7219c-123">我們會進行兩項變更：</span><span class="sxs-lookup"><span data-stu-id="7219c-123">We'll make two changes:</span></span>

- <span data-ttu-id="7219c-124">變更要傳回的字串，而不是 ActionResult 方法</span><span class="sxs-lookup"><span data-stu-id="7219c-124">Change the method to return a string instead of an ActionResult</span></span>
- <span data-ttu-id="7219c-125">變更要傳回"Hello 從 Home"return 的陳述式</span><span class="sxs-lookup"><span data-stu-id="7219c-125">Change the return statement to return "Hello from Home"</span></span>

<span data-ttu-id="7219c-126">此方法現在看起來應該像這樣：</span><span class="sxs-lookup"><span data-stu-id="7219c-126">The method should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a><span data-ttu-id="7219c-127">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="7219c-127">Running the Application</span></span>

<span data-ttu-id="7219c-128">現在讓我們執行網站。</span><span class="sxs-lookup"><span data-stu-id="7219c-128">Now let's run the site.</span></span> <span data-ttu-id="7219c-129">我們可以開始我們的 web 伺服器，並試試我們的網站使用下列任一項：</span><span class="sxs-lookup"><span data-stu-id="7219c-129">We can start our web-server and try out the site using any of the following::</span></span>

- <span data-ttu-id="7219c-130">選擇偵錯 ⇨ 開始偵錯 功能表項目</span><span class="sxs-lookup"><span data-stu-id="7219c-130">Choose the Debug ⇨ Start Debugging menu item</span></span>
- <span data-ttu-id="7219c-131">按一下工具列中的綠色箭號按鈕 ![](mvc-music-store-part-2/_static/image2.jpg)</span><span class="sxs-lookup"><span data-stu-id="7219c-131">Click the Green arrow button in the toolbar ![](mvc-music-store-part-2/_static/image2.jpg)</span></span>
- <span data-ttu-id="7219c-132">使用鍵盤快速鍵 F5。</span><span class="sxs-lookup"><span data-stu-id="7219c-132">Use the keyboard shortcut, F5.</span></span>

<span data-ttu-id="7219c-133">使用任何上述步驟會編譯專案，並則導致是內建於 Visual Web Developer 中啟動 ASP.NET 程式開發伺服器。</span><span class="sxs-lookup"><span data-stu-id="7219c-133">Using any of the above steps will compile our project, and then cause the ASP.NET Development Server that is built-into Visual Web Developer to start.</span></span> <span data-ttu-id="7219c-134">通知會出現在表示 ASP.NET Development Server 已啟動，將螢幕的右下角，而且會用來顯示它下執行的連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="7219c-134">A notification will appear in the bottom corner of the screen to indicate that the ASP.NET Development Server has started up, and will show the port number that it is running under.</span></span>

![](mvc-music-store-part-2/_static/image2.png)

<span data-ttu-id="7219c-135">Visual Web Developer 會接著會自動開啟瀏覽器視窗中的 URL 會指向我們的 web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="7219c-135">Visual Web Developer will then automatically open a browser window whose URL points to our web-server.</span></span> <span data-ttu-id="7219c-136">這可讓我們快速試用我們的 web 應用程式：</span><span class="sxs-lookup"><span data-stu-id="7219c-136">This will allow us to quickly try out our web application:</span></span>

![](mvc-music-store-part-2/_static/image3.png)

<span data-ttu-id="7219c-137">好，是相當快速的 – 我們建立新的網站，加入三價線函式，而且我們已在瀏覽器中的文字。</span><span class="sxs-lookup"><span data-stu-id="7219c-137">Okay, that was pretty quick – we created a new website, added a three line function, and we've got text in a browser.</span></span> <span data-ttu-id="7219c-138">不我科學中，但它是一個起點。</span><span class="sxs-lookup"><span data-stu-id="7219c-138">Not rocket science, but it's a start.</span></span>

<span data-ttu-id="7219c-139">*注意： Visual Web Developer 包含 ASP.NET Development Server，將在幾個隨機免費 「 連接埠 」 執行您的網站。在上面的螢幕擷取畫面，在執行站台`http://localhost:26641/`，因此它使用連接埠 26641。您的連接埠號碼將會不同。當我們談到 URL 的 like /Store/Browse 在本教學課程時，可將會移之後的連接埠號碼。假設 26641 的連接埠號碼，瀏覽至存放區/瀏覽表示瀏覽至`http://localhost:26641/Store/Browse`。*</span><span class="sxs-lookup"><span data-stu-id="7219c-139">*Note: Visual Web Developer includes the ASP.NET Development Server, which will run your website on a random free "port" number. In the screenshot above, the site is running at `http://localhost:26641/`, so it's using port 26641. Your port number will be different. When we talk about URL's like /Store/Browse in this tutorial, that will go after the port number. Assuming a port number of 26641, browsing to /Store/Browse will mean browsing to `http://localhost:26641/Store/Browse`.*</span></span>

## <a name="adding-a-storecontroller"></a><span data-ttu-id="7219c-140">新增 StoreController</span><span class="sxs-lookup"><span data-stu-id="7219c-140">Adding a StoreController</span></span>

<span data-ttu-id="7219c-141">我們新增了一個簡單的 HomeController 實作本公司網站的首頁。</span><span class="sxs-lookup"><span data-stu-id="7219c-141">We added a simple HomeController that implements the Home Page of our site.</span></span> <span data-ttu-id="7219c-142">讓我們現在就加入我們將使用來實作瀏覽我們的音樂市集功能的另一個控制站。</span><span class="sxs-lookup"><span data-stu-id="7219c-142">Let's now add another controller that we'll use to implement the browsing functionality of our music store.</span></span> <span data-ttu-id="7219c-143">我們的存放控制器將會支援三種案例：</span><span class="sxs-lookup"><span data-stu-id="7219c-143">Our store controller will support three scenarios:</span></span>

- <span data-ttu-id="7219c-144">在我們的 music store 中的音樂內容類型的清單頁面</span><span class="sxs-lookup"><span data-stu-id="7219c-144">A listing page of the music genres in our music store</span></span>
- <span data-ttu-id="7219c-145">列出所有在特定內容類型中的音樂 album 瀏覽 頁面</span><span class="sxs-lookup"><span data-stu-id="7219c-145">A browse page that lists all of the music albums in a particular genre</span></span>
- <span data-ttu-id="7219c-146">詳細資料頁面，其中顯示有關特定音樂專輯資訊</span><span class="sxs-lookup"><span data-stu-id="7219c-146">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="7219c-147">我們一開始先加入 StoreController 類別...</span><span class="sxs-lookup"><span data-stu-id="7219c-147">We'll start by adding a new StoreController class..</span></span> <span data-ttu-id="7219c-148">如果您還沒有這麼做，請停止執行應用程式藉由關閉瀏覽器，或選取偵錯 ⇨ 停止偵錯 功能表項目。</span><span class="sxs-lookup"><span data-stu-id="7219c-148">If you haven't already, stop running the application either by closing the browser or selecting the Debug ⇨ Stop Debugging menu item.</span></span>

<span data-ttu-id="7219c-149">現在，加入新的 StoreController。</span><span class="sxs-lookup"><span data-stu-id="7219c-149">Now add a new StoreController.</span></span> <span data-ttu-id="7219c-150">就像我們一樣 HomeController，會執行此作業，以滑鼠右鍵按一下方案總管 內的 「 控制項 」 資料夾，然後選擇 加入-&gt;控制器功能表項目</span><span class="sxs-lookup"><span data-stu-id="7219c-150">Just like we did with HomeController, we'll do this by right-clicking on the "Controllers" folder within the Solution Explorer and choosing the Add-&gt;Controller menu item</span></span>

![](mvc-music-store-part-2/_static/image4.png)

<span data-ttu-id="7219c-151">我們新 StoreController 已經有 「 索引 」 方法。</span><span class="sxs-lookup"><span data-stu-id="7219c-151">Our new StoreController already has an "Index" method.</span></span> <span data-ttu-id="7219c-152">我們將使用此 「 索引 」 方法來實作我們列出在我們的 music store 中的所有內容類型的清單頁面。</span><span class="sxs-lookup"><span data-stu-id="7219c-152">We'll use this "Index" method to implement our listing page that lists all genres in our music store.</span></span> <span data-ttu-id="7219c-153">我們也會新增兩個額外的方法，來實作兩個其他情況下我們想要以處理我們 StoreController： 瀏覽和詳細資料。</span><span class="sxs-lookup"><span data-stu-id="7219c-153">We'll also add two additional methods to implement the two other scenarios we want our StoreController to handle: Browse and Details.</span></span>

<span data-ttu-id="7219c-154">「 控制器動作 」，會呼叫這些方法 （索引、 瀏覽和詳細資料），我們的控制器內，而且您已經看到與 HomeController.Index （） 動作方法，他們的工作回應 URL 的要求，並 （一般而言） 判斷哪些內容應該傳送回瀏覽器或叫用的 URL 的使用者。</span><span class="sxs-lookup"><span data-stu-id="7219c-154">These methods (Index, Browse and Details) within our Controller are called "Controller Actions", and as you've already seen with the HomeController.Index()action method, their job is to respond to URL requests and (generally speaking) determine what content should be sent back to the browser or user that invoked the URL.</span></span>

<span data-ttu-id="7219c-155">我們一開始我們 StoreController 的實作，藉由變更 theIndex() 方法，以傳回字串"Hello 從 Store.Index() 」，我們將新增類似的方法 Browse() 和 Details():</span><span class="sxs-lookup"><span data-stu-id="7219c-155">We'll start our StoreController implementation by changing theIndex() method to return the string "Hello from Store.Index()" and we'll add similar methods for Browse() and Details():</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

<span data-ttu-id="7219c-156">再次執行專案，並瀏覽下列 Url:</span><span class="sxs-lookup"><span data-stu-id="7219c-156">Run the project again and browse the following URLs:</span></span>

- <span data-ttu-id="7219c-157">/ 存放區</span><span class="sxs-lookup"><span data-stu-id="7219c-157">/Store</span></span>
- <span data-ttu-id="7219c-158">/ Store/瀏覽</span><span class="sxs-lookup"><span data-stu-id="7219c-158">/Store/Browse</span></span>
- <span data-ttu-id="7219c-159">/ 存放區/詳細資料</span><span class="sxs-lookup"><span data-stu-id="7219c-159">/Store/Details</span></span>

<span data-ttu-id="7219c-160">存取這些 Url，將會叫用控制器的動作方法，並傳回字串的回應：</span><span class="sxs-lookup"><span data-stu-id="7219c-160">Accessing these URLs will invoke the action methods within our Controller and return string responses:</span></span>

![](mvc-music-store-part-2/_static/image5.png)

<span data-ttu-id="7219c-161">太棒了，但這些只是常數字串。</span><span class="sxs-lookup"><span data-stu-id="7219c-161">That's great, but these are just constant strings.</span></span> <span data-ttu-id="7219c-162">我們把它們動態的讓他們從 URL 取得資訊，並在網頁輸出中顯示它。</span><span class="sxs-lookup"><span data-stu-id="7219c-162">Let's make them dynamic, so they take information from the URL and display it in the page output.</span></span>

<span data-ttu-id="7219c-163">首先我們要變更瀏覽動作方法，從 URL 擷取查詢字串值。</span><span class="sxs-lookup"><span data-stu-id="7219c-163">First we'll change the Browse action method to retrieve a querystring value from the URL.</span></span> <span data-ttu-id="7219c-164">我們可以將"genre"參數新增至我們的動作方法來執行這項操作。</span><span class="sxs-lookup"><span data-stu-id="7219c-164">We can do this by adding a "genre" parameter to our action method.</span></span> <span data-ttu-id="7219c-165">當我們執行此動作 ASP.NET MVC 會自動將叫用時，名為"genre"至我們的動作方法的任何查詢字串或表單張貼參數傳遞。</span><span class="sxs-lookup"><span data-stu-id="7219c-165">When we do this ASP.NET MVC will automatically pass any querystring or form post parameters named "genre" to our action method when it is invoked.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

<span data-ttu-id="7219c-166">*注意： 我們要使用 HttpUtility.HtmlEncode 公用程式方法來處理使用者輸入。這可防止使用者插入檢視中的 Javascript，例如 /Store/Browse 的連結嗎？內容類型 =&lt;指令碼&gt;window.location = 'http://hackersite.com'&lt;/script&gt;。*</span><span class="sxs-lookup"><span data-stu-id="7219c-166">*Note: We're using the HttpUtility.HtmlEncode utility method to sanitize the user input. This prevents users from injecting Javascript into our View with a link like /Store/Browse?Genre=&lt;script&gt;window.location='http://hackersite.com'&lt;/script&gt;.*</span></span>

<span data-ttu-id="7219c-167">現在讓我們瀏覽至存放區/瀏覽？內容類型 = Disco</span><span class="sxs-lookup"><span data-stu-id="7219c-167">Now let's browse to /Store/Browse?Genre=Disco</span></span>

![](mvc-music-store-part-2/_static/image6.png)

<span data-ttu-id="7219c-168">接下來讓我們變更即可讀取並顯示名為識別碼的輸入的參數的詳細資料動作</span><span class="sxs-lookup"><span data-stu-id="7219c-168">Let's next change the Details action to read and display an input parameter named ID.</span></span> <span data-ttu-id="7219c-169">不同於我們的前一個方法，我們將不會進行內嵌的識別碼值做為查詢字串參數。</span><span class="sxs-lookup"><span data-stu-id="7219c-169">Unlike our previous method, we won't be embedding the ID value as a querystring parameter.</span></span> <span data-ttu-id="7219c-170">而我們會將它內嵌直接在 URL 本身內。</span><span class="sxs-lookup"><span data-stu-id="7219c-170">Instead we'll embed it directly within the URL itself.</span></span> <span data-ttu-id="7219c-171">例如： /Store/Details/5。</span><span class="sxs-lookup"><span data-stu-id="7219c-171">For example: /Store/Details/5.</span></span>

<span data-ttu-id="7219c-172">ASP.NET MVC 可讓我們輕鬆地執行這項操作不需要設定任何項目。</span><span class="sxs-lookup"><span data-stu-id="7219c-172">ASP.NET MVC lets us easily do this without having to configure anything.</span></span> <span data-ttu-id="7219c-173">ASP.NET MVC 的預設路由慣例是將 URL 的區段之後的動作方法名稱視為名為 「 識別碼 」 的參數。</span><span class="sxs-lookup"><span data-stu-id="7219c-173">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named "ID".</span></span> <span data-ttu-id="7219c-174">如果動作方法有一個名為 ID 參數然後 ASP.NET MVC 會自動傳遞的 URL 區段您做為參數。</span><span class="sxs-lookup"><span data-stu-id="7219c-174">If your action method has a parameter named ID then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

<span data-ttu-id="7219c-175">執行應用程式，並瀏覽至 /Store/Details/5:</span><span class="sxs-lookup"><span data-stu-id="7219c-175">Run the application and browse to /Store/Details/5:</span></span>

![](mvc-music-store-part-2/_static/image7.png)

<span data-ttu-id="7219c-176">讓我們複習一下到目前為止完成的內容：</span><span class="sxs-lookup"><span data-stu-id="7219c-176">Let's recap what we've done so far:</span></span>

- <span data-ttu-id="7219c-177">我們在 Visual Web Developer 中建立新的 ASP.NET MVC 專案</span><span class="sxs-lookup"><span data-stu-id="7219c-177">We've created a new ASP.NET MVC project in Visual Web Developer</span></span>
- <span data-ttu-id="7219c-178">我們所討論的 ASP.NET MVC 應用程式的基本的資料夾結構</span><span class="sxs-lookup"><span data-stu-id="7219c-178">We've discussed the basic folder structure of an ASP.NET MVC application</span></span>
- <span data-ttu-id="7219c-179">我們已了解如何執行我們的網站使用 ASP.NET 程式開發伺服器</span><span class="sxs-lookup"><span data-stu-id="7219c-179">We've learned how to run our website using the ASP.NET Development Server</span></span>
- <span data-ttu-id="7219c-180">我們建立了兩個控制器類別： HomeController 和 StoreController</span><span class="sxs-lookup"><span data-stu-id="7219c-180">We've created two Controller classes: a HomeController and a StoreController</span></span>
- <span data-ttu-id="7219c-181">我們已加入我們控制站會回應 URL 的要求，並傳回至瀏覽器的文字中的動作方法</span><span class="sxs-lookup"><span data-stu-id="7219c-181">We've added Action Methods to our controllers which respond to URL requests and return text to the browser</span></span>


> [!div class="step-by-step"]
> <span data-ttu-id="7219c-182">[上一頁](mvc-music-store-part-1.md)
> [下一頁](mvc-music-store-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="7219c-182">[Previous](mvc-music-store-part-1.md)
[Next](mvc-music-store-part-3.md)</span></span>
