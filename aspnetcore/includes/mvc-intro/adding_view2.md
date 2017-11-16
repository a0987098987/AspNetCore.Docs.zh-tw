<span data-ttu-id="c9204-101">使用下列內容取代 *Views/HelloWorld/Index.cshtml* Razor 檢視檔案的內容：</span><span class="sxs-lookup"><span data-stu-id="c9204-101">Replace the contents of the *Views/HelloWorld/Index.cshtml* Razor view file with the following:</span></span>

[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Index.cshtml)]

<span data-ttu-id="c9204-102">巡覽至 `http://localhost:xxxx/HelloWorld`。</span><span class="sxs-lookup"><span data-stu-id="c9204-102">Navigate to `http://localhost:xxxx/HelloWorld`.</span></span> <span data-ttu-id="c9204-103">`HelloWorldController` 中的 `Index` 方法不會執行什麼作業；它會執行陳述式 `return View();`，其指定方法應使用檢視範本檔案來呈現瀏覽器的回應。</span><span class="sxs-lookup"><span data-stu-id="c9204-103">The `Index` method in the `HelloWorldController` didn't do much; it ran the statement `return View();`, which specified that the method should use a view template file to render a response to the browser.</span></span> <span data-ttu-id="c9204-104">因為您沒有明確指定檢視範本檔案的名稱，MVC 預設為使用 */Views/HelloWorld* 資料夾中的 *Index.cshtml* 檢視檔案。</span><span class="sxs-lookup"><span data-stu-id="c9204-104">Because you didn't explicitly specify the name of the view template file, MVC defaulted to using the *Index.cshtml* view file in the */Views/HelloWorld* folder.</span></span> <span data-ttu-id="c9204-105">下列影像顯示檢視中硬式編碼的字串 "Hello from our View Template!"</span><span class="sxs-lookup"><span data-stu-id="c9204-105">The image below shows the string "Hello from our View Template!"</span></span> <span data-ttu-id="c9204-106">。</span><span class="sxs-lookup"><span data-stu-id="c9204-106">hard-coded in the view.</span></span>

![瀏覽器視窗](../../tutorials/first-mvc-app/adding-view/_static/hell_template.png)

<span data-ttu-id="c9204-108">如果瀏覽器視窗很小 (例如在行動裝置上)，您可能需要切換 (點選) 右上方的[啟動程序導覽按鈕](http://getbootstrap.com/components/#navbar)，以查看 **Home**、**About** 和 **Contact** 連結。</span><span class="sxs-lookup"><span data-stu-id="c9204-108">If your browser window is small (for example on a mobile device), you might need to toggle (tap) the [Bootstrap navigation button](http://getbootstrap.com/components/#navbar) in the upper right to see the **Home**, **About**, and **Contact** links.</span></span>

![醒目提示啟動程序導覽按鈕的瀏覽器視窗](../../tutorials/first-mvc-app/adding-view/_static/1.png)

## <a name="changing-views-and-layout-pages"></a><span data-ttu-id="c9204-110">變更檢視和版面配置頁</span><span class="sxs-lookup"><span data-stu-id="c9204-110">Changing views and layout pages</span></span>

<span data-ttu-id="c9204-111">點選功能表連結 (**MvcMovie**、**Home**、**About**)。</span><span class="sxs-lookup"><span data-stu-id="c9204-111">Tap the menu links (**MvcMovie**, **Home**, **About**).</span></span> <span data-ttu-id="c9204-112">每個頁面會顯示相同的功能表配置。</span><span class="sxs-lookup"><span data-stu-id="c9204-112">Each page shows the same menu layout.</span></span> <span data-ttu-id="c9204-113">功能表配置是在 *Views/Shared/_Layout.cshtml* 檔案中實作。</span><span class="sxs-lookup"><span data-stu-id="c9204-113">The menu layout is implemented in the *Views/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="c9204-114">開啟 *Views/Shared/_Layout.cshtml* 檔案。</span><span class="sxs-lookup"><span data-stu-id="c9204-114">Open the *Views/Shared/_Layout.cshtml* file.</span></span>

<span data-ttu-id="c9204-115">[版面配置](xref:mvc/views/layout)範本可讓您在某個位置指定網站的 HTML 容器配置，然後將它套用到網站中的多個頁面。</span><span class="sxs-lookup"><span data-stu-id="c9204-115">[Layout](xref:mvc/views/layout) templates allow you to specify the HTML container layout of your site in one place and then apply it across multiple pages in your site.</span></span> <span data-ttu-id="c9204-116">找到 `@RenderBody()` 這行。</span><span class="sxs-lookup"><span data-stu-id="c9204-116">Find the `@RenderBody()` line.</span></span> <span data-ttu-id="c9204-117">`RenderBody` 是顯示您建立之所有檢視特定頁面的預留位置，「包裝」在版面配置頁中。</span><span class="sxs-lookup"><span data-stu-id="c9204-117">`RenderBody` is a placeholder where all the view-specific pages you create show up, *wrapped* in the layout page.</span></span> <span data-ttu-id="c9204-118">例如，如果您選取 **About** 連結，**Views/Home/About.cshtml** 檢視就會呈現在 `RenderBody` 方法內。</span><span class="sxs-lookup"><span data-stu-id="c9204-118">For example, if you select the **About** link, the **Views/Home/About.cshtml** view is rendered inside the `RenderBody` method.</span></span>

## <a name="change-the-title-and-menu-link-in-the-layout-file"></a><span data-ttu-id="c9204-119">變更配置檔案中的標題和功能表連結</span><span class="sxs-lookup"><span data-stu-id="c9204-119">Change the title and menu link in the layout file</span></span>

<span data-ttu-id="c9204-120">變更標題元素的內容。</span><span class="sxs-lookup"><span data-stu-id="c9204-120">Change the contents of the title element.</span></span> <span data-ttu-id="c9204-121">將版面配置範本中的錨定文字變更為 "Movie App"，並將控制器從 `Home` 變更為 `Movies`，醒目提示如下：</span><span class="sxs-lookup"><span data-stu-id="c9204-121">Change the anchor text in the layout template to "Movie App" and the controller from `Home` to `Movies` as highlighted below:</span></span>

<span data-ttu-id="c9204-122">注意：ASP.NET Core 2.0 版略有不同。</span><span class="sxs-lookup"><span data-stu-id="c9204-122">Note: The ASP.NET Core 2.0 version is slightly different.</span></span> <span data-ttu-id="c9204-123">它並未包含 `@inject ApplicationInsights` 和 `@Html.Raw(JavaScriptSnippet.FullScript)`。</span><span class="sxs-lookup"><span data-stu-id="c9204-123">It doesn't contain `@inject ApplicationInsights` and `@Html.Raw(JavaScriptSnippet.FullScript)`.</span></span>

[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]

>[!WARNING]
> <span data-ttu-id="c9204-124">我們尚未實作 `Movies` 控制器，因此如果您按一下該連結，就會收到 404 (找不到) 錯誤。</span><span class="sxs-lookup"><span data-stu-id="c9204-124">We haven't implemented the `Movies` controller yet, so if you click on that link, you'll get a 404 (Not found) error.</span></span>

<span data-ttu-id="c9204-125">儲存變更並點選 **About** 連結。</span><span class="sxs-lookup"><span data-stu-id="c9204-125">Save your changes and tap the **About** link.</span></span> <span data-ttu-id="c9204-126">請注意現在瀏覽器索引標籤上的標題會顯示 **About - Movie App**，而不是 **About - Mvc Movie**。</span><span class="sxs-lookup"><span data-stu-id="c9204-126">Notice how the title on the browser tab now displays **About - Movie App** instead of **About - Mvc Movie**.</span></span> <span data-ttu-id="c9204-127">點選 **Contact** 連結，並請注意它也會顯示 **Movie App**。</span><span class="sxs-lookup"><span data-stu-id="c9204-127">Tap the **Contact** link and notice that it also displays **Movie App**.</span></span> <span data-ttu-id="c9204-128">我們能夠在版面配置範本中一次進行變更，並讓網站上的所有頁面反映新的連結文字和新的標題。</span><span class="sxs-lookup"><span data-stu-id="c9204-128">We were able to make the change once in the layout template and have all pages on the site reflect the new link text and new title.</span></span>

<span data-ttu-id="c9204-129">檢查 *Views/_ViewStart.cshtml* 檔案：</span><span class="sxs-lookup"><span data-stu-id="c9204-129">Examine the *Views/_ViewStart.cshtml* file:</span></span>


```HTML
@{
    Layout = "_Layout";
}
```

<span data-ttu-id="c9204-130">*Views/_ViewStart.cshtml* 檔案會將 *Views/Shared/_Layout.cshtml* 檔案引入每一個檢視。</span><span class="sxs-lookup"><span data-stu-id="c9204-130">The *Views/_ViewStart.cshtml* file brings in the *Views/Shared/_Layout.cshtml* file to each view.</span></span> <span data-ttu-id="c9204-131">您可以使用 `Layout` 屬性來設定不同的版面配置檢視，或將它設定為 `null`，因此不會使用任何版面配置檔案。</span><span class="sxs-lookup"><span data-stu-id="c9204-131">You can use the `Layout` property to set a different layout view, or set it to `null` so no layout file will be used.</span></span>

<span data-ttu-id="c9204-132">變更 `Index` 檢視的標題。</span><span class="sxs-lookup"><span data-stu-id="c9204-132">Change the title of the `Index` view.</span></span>

<span data-ttu-id="c9204-133">開啟 *Views/HelloWorld/Index.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="c9204-133">Open *Views/HelloWorld/Index.cshtml*.</span></span> <span data-ttu-id="c9204-134">有兩個地方進行變更：</span><span class="sxs-lookup"><span data-stu-id="c9204-134">There are two places to make a change:</span></span>

   * <span data-ttu-id="c9204-135">在瀏覽器標題中出現的文字。</span><span class="sxs-lookup"><span data-stu-id="c9204-135">The text that appears in the title of the browser.</span></span>
   * <span data-ttu-id="c9204-136">次要標頭 (`<h2>` 元素)。</span><span class="sxs-lookup"><span data-stu-id="c9204-136">The secondary header (`<h2>` element).</span></span>

<span data-ttu-id="c9204-137">您會使這些變更略有不同，因此可看出哪一段程式碼變更了應用程式的哪個部分。</span><span class="sxs-lookup"><span data-stu-id="c9204-137">You'll make them slightly different so you can see which bit of code changes which part of the app.</span></span>


```HTML
@{
    ViewData["Title"] = "Movie List";
}

<h2>My Movie List</h2>

<p>Hello from our View Template!</p>
```

<span data-ttu-id="c9204-138">上述程式碼中的 `ViewData["Title"] = "Movie List";` 會將 `ViewData` 字典的 `Title` 屬性設定為 "Movie List"。</span><span class="sxs-lookup"><span data-stu-id="c9204-138">`ViewData["Title"] = "Movie List";` in the code above sets the `Title` property of the `ViewData` dictionary to "Movie List".</span></span> <span data-ttu-id="c9204-139">`Title` 屬性則用於版面配置頁中的 `<title>` HTML 元素：</span><span class="sxs-lookup"><span data-stu-id="c9204-139">The `Title` property is used in the `<title>` HTML element in the layout page:</span></span>


```HTML
<title>@ViewData["Title"] - Movie App</title>
   ```

<span data-ttu-id="c9204-140">儲存變更並巡覽至 `http://localhost:xxxx/HelloWorld`。</span><span class="sxs-lookup"><span data-stu-id="c9204-140">Save your change and navigate to `http://localhost:xxxx/HelloWorld`.</span></span> <span data-ttu-id="c9204-141">請注意，瀏覽器標題、主要標題和次要標題已變更</span><span class="sxs-lookup"><span data-stu-id="c9204-141">Notice that the browser title, the primary heading, and the secondary headings have changed.</span></span> <span data-ttu-id="c9204-142">(如果您在瀏覽器中沒有看到變更，可能檢視的是快取的內容。</span><span class="sxs-lookup"><span data-stu-id="c9204-142">(If you don't see changes in the browser, you might be viewing cached content.</span></span> <span data-ttu-id="c9204-143">請在瀏覽器中按 Ctrl + F5 以強制載入來自伺服器的回應)。瀏覽器標題是以我們在 *Index.cshtml* 檢視範本中設定的 `ViewData["Title"]` 和版面配置檔案中新增的額外 "- Movie App" 來建立的。</span><span class="sxs-lookup"><span data-stu-id="c9204-143">Press Ctrl+F5 in your browser to force the response from the server to be loaded.) The browser title is created with `ViewData["Title"]` we set in the *Index.cshtml* view template and the additional "- Movie App" added in the layout file.</span></span>

<span data-ttu-id="c9204-144">同時也請注意，*Index.cshtml* 檢視範本中的內容如何與 *Views/Shared/_Layout.cshtml* 檢視範本和已傳送至瀏覽器的單一 HTML 回應合併。</span><span class="sxs-lookup"><span data-stu-id="c9204-144">Also notice how the content in the *Index.cshtml* view template was merged with the *Views/Shared/_Layout.cshtml* view template and a single HTML response was sent to the browser.</span></span> <span data-ttu-id="c9204-145">版面配置範本可讓您輕鬆進行會套用到應用程式之所有頁面的變更。</span><span class="sxs-lookup"><span data-stu-id="c9204-145">Layout templates make it really easy to make changes that apply across all of the pages in your application.</span></span> <span data-ttu-id="c9204-146">若要深入了解，請參閱[版面配置](../../mvc/views/layout.md)。</span><span class="sxs-lookup"><span data-stu-id="c9204-146">To learn more see [Layout](../../mvc/views/layout.md).</span></span>

![電影清單檢視](../../tutorials/first-mvc-app/adding-view/_static/hell3.png)

<span data-ttu-id="c9204-148">然而，我們的這一點點「資料」(在此案例中為 "Hello from our View Template!"</span><span class="sxs-lookup"><span data-stu-id="c9204-148">Our little bit of "data" (in this case the "Hello from our View Template!"</span></span> <span data-ttu-id="c9204-149">訊息) 是硬式編碼的資料。</span><span class="sxs-lookup"><span data-stu-id="c9204-149">message) is hard-coded, though.</span></span> <span data-ttu-id="c9204-150">MVC 應用程式具有 "V" (檢視)，並已取得 "C" (控制器)，但還沒有 "M" (模型)。</span><span class="sxs-lookup"><span data-stu-id="c9204-150">The MVC application has a "V" (view) and you've got a "C" (controller), but no "M" (model) yet.</span></span>

## <a name="passing-data-from-the-controller-to-the-view"></a><span data-ttu-id="c9204-151">將資料從控制器傳遞至檢視</span><span class="sxs-lookup"><span data-stu-id="c9204-151">Passing Data from the Controller to the View</span></span>

<span data-ttu-id="c9204-152">回應傳入的 URL 要求時會叫用控制器動作。</span><span class="sxs-lookup"><span data-stu-id="c9204-152">Controller actions are invoked in response to an incoming URL request.</span></span> <span data-ttu-id="c9204-153">控制器類別是您撰寫處理傳入瀏覽器要求之程式碼的位置。</span><span class="sxs-lookup"><span data-stu-id="c9204-153">A controller class is where you write the code that handles the incoming browser requests.</span></span> <span data-ttu-id="c9204-154">控制器會從資料來源擷取資料，並決定要將哪一種回應傳回瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="c9204-154">The controller retrieves data from a data source and decides what type of response to send back to the browser.</span></span> <span data-ttu-id="c9204-155">您可以從控制器使用檢視範本來產生並格式化瀏覽器的 HTML 回應。</span><span class="sxs-lookup"><span data-stu-id="c9204-155">View templates can be used from a controller to generate and format an HTML response to the browser.</span></span>

<span data-ttu-id="c9204-156">控制器負責提供為了讓檢視範本呈現回應所需的資料。</span><span class="sxs-lookup"><span data-stu-id="c9204-156">Controllers are responsible for providing the data required in order for a view template to render a response.</span></span> <span data-ttu-id="c9204-157">最佳做法：檢視範本**不**應該執行商務邏輯，或直接與資料庫互動。</span><span class="sxs-lookup"><span data-stu-id="c9204-157">A best practice: View templates should **not** perform business logic or interact with a database directly.</span></span> <span data-ttu-id="c9204-158">相反地，檢視範本應該只使用由控制器提供給它的資料。</span><span class="sxs-lookup"><span data-stu-id="c9204-158">Rather, a view template should work only with the data that's provided to it by the controller.</span></span> <span data-ttu-id="c9204-159">維護這項「關注點分離」，有助於保持程式碼整潔、可測試且可維護。</span><span class="sxs-lookup"><span data-stu-id="c9204-159">Maintaining this "separation of concerns" helps keep your code clean, testable, and maintainable.</span></span>

<span data-ttu-id="c9204-160">目前，`HelloWorldController` 類別中的 `Welcome` 方法會採用 `name` 和 `ID` 參數，然後將值直接輸出到瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="c9204-160">Currently, the `Welcome` method in the `HelloWorldController` class takes a `name` and a `ID` parameter and then outputs the values directly to the browser.</span></span> <span data-ttu-id="c9204-161">與其讓控制器以字串方式呈現這個回應，不如讓我們變更控制器以改為使用檢視範本。</span><span class="sxs-lookup"><span data-stu-id="c9204-161">Rather than have the controller render this response as a string, let’s change the controller to use a view template instead.</span></span> <span data-ttu-id="c9204-162">檢視範本會產生動態回應，這表示您需要將適當數量的資料從控制器傳遞至檢視，以便產生回應。</span><span class="sxs-lookup"><span data-stu-id="c9204-162">The view template will generate a dynamic response, which means that you need to pass appropriate bits of data from the controller to the view in order to generate the response.</span></span> <span data-ttu-id="c9204-163">透過讓控制器將檢視範本需要的動態資料 (參數) 放置在檢視範本之後可以存取的 `ViewData` 字典，即可完成這項操作。</span><span class="sxs-lookup"><span data-stu-id="c9204-163">You can do this by having the controller put the dynamic data (parameters) that the view template needs in a `ViewData` dictionary that the view template can then access.</span></span>

<span data-ttu-id="c9204-164">返回 *HelloWorldController.cs* 檔案並變更 `Welcome` 方法，將 `Message` 和 `NumTimes` 值新增至 `ViewData` 字典。</span><span class="sxs-lookup"><span data-stu-id="c9204-164">Return to the *HelloWorldController.cs* file and change the `Welcome` method to add a `Message` and `NumTimes` value to the `ViewData` dictionary.</span></span> <span data-ttu-id="c9204-165">`ViewData` 字典是動態物件，這表示您可以在其中放置任何項目；`ViewData` 物件則要在您於其中放入某個項目之後，才會有定義的屬性。</span><span class="sxs-lookup"><span data-stu-id="c9204-165">The `ViewData` dictionary is a dynamic object, which means you can put whatever you want in to it; the `ViewData` object has no defined properties until you put something inside it.</span></span> <span data-ttu-id="c9204-166">MVC [模型繫結](xref:mvc/models/model-binding)系統會自動將網址列上查詢字串中的具名參數 (`name` 和 `numTimes`) 對應至方法中的參數。</span><span class="sxs-lookup"><span data-stu-id="c9204-166">The [MVC model binding system](xref:mvc/models/model-binding) automatically maps the named parameters (`name` and `numTimes`) from the query string in the address bar to parameters in your method.</span></span> <span data-ttu-id="c9204-167">完整的 *HelloWorldController.cs* 檔案如下所示：</span><span class="sxs-lookup"><span data-stu-id="c9204-167">The complete *HelloWorldController.cs* file looks like this:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_5)]

<span data-ttu-id="c9204-168">`ViewData` 字典物件包含將傳遞至檢視的資料。</span><span class="sxs-lookup"><span data-stu-id="c9204-168">The `ViewData` dictionary object contains data that will be passed to the view.</span></span> 

<span data-ttu-id="c9204-169">建立名為 *Views/HelloWorld/Welcome.cshtml* 的 Welcome 檢視範本。</span><span class="sxs-lookup"><span data-stu-id="c9204-169">Create a Welcome view template named *Views/HelloWorld/Welcome.cshtml*.</span></span>

<span data-ttu-id="c9204-170">您將在 *Welcome.cshtml* 檢視範本中建立迴圈，以顯示 "Hello" `NumTimes` 次。</span><span class="sxs-lookup"><span data-stu-id="c9204-170">You'll create a loop in the *Welcome.cshtml* view template that displays "Hello" `NumTimes`.</span></span> <span data-ttu-id="c9204-171">使用下列內容取代 *Views/HelloWorld/Welcome.cshtml* 的內容：</span><span class="sxs-lookup"><span data-stu-id="c9204-171">Replace the contents of *Views/HelloWorld/Welcome.cshtml* with the following:</span></span>

[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Welcome.cshtml)]

<span data-ttu-id="c9204-172">儲存變更並瀏覽至下列 URL：</span><span class="sxs-lookup"><span data-stu-id="c9204-172">Save your changes and browse to the following URL:</span></span>

`http://localhost:xxxx/HelloWorld/Welcome?name=Rick&numtimes=4`

<span data-ttu-id="c9204-173">資料是使用 [MVC 模型繫結器](xref:mvc/models/model-binding)從 URL 取得並傳遞至控制站。</span><span class="sxs-lookup"><span data-stu-id="c9204-173">Data is taken from the URL and passed to the controller using the [MVC model binder](xref:mvc/models/model-binding) .</span></span> <span data-ttu-id="c9204-174">控制器會將資料封裝成 `ViewData` 字典，並將該物件傳遞至檢視。</span><span class="sxs-lookup"><span data-stu-id="c9204-174">The controller packages the data into a `ViewData` dictionary and passes that object to the view.</span></span> <span data-ttu-id="c9204-175">接著，檢視會以 HTML 將資料呈現到瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="c9204-175">The view then renders the data as HTML to the browser.</span></span>

![顯示一個 Welcome 標籤和顯示四次的片語 Hello Rick 的 About 檢視](../../tutorials/first-mvc-app/adding-view/_static/rick.png)

<span data-ttu-id="c9204-177">在上述範例中，我們使用 `ViewData` 字典，以便將資料從控制器傳遞至檢視。</span><span class="sxs-lookup"><span data-stu-id="c9204-177">In the sample above, we used the `ViewData` dictionary to pass data from the controller to a view.</span></span> <span data-ttu-id="c9204-178">稍後在教學課程中，我們將使用檢視模型將控制器中的資料傳遞至檢視。</span><span class="sxs-lookup"><span data-stu-id="c9204-178">Later in the tutorial, we will use a view model to pass data from a controller to a view.</span></span> <span data-ttu-id="c9204-179">傳遞資料的檢視模型方法通常比 `ViewData` 字典方法較為慣用。</span><span class="sxs-lookup"><span data-stu-id="c9204-179">The view model approach to passing data is generally much preferred over the `ViewData` dictionary approach.</span></span> <span data-ttu-id="c9204-180">如需詳細資訊，請參閱 [ViewModel vs ViewData vs ViewBag vs TempData vs Session in MVC](http://www.mytecbits.com/microsoft/dot-net/viewmodel-viewdata-viewbag-tempdata-mvc)(MVC 中的 ViewMode 與 ViewData 與 ViewBag 與 TempData 與 Session)。</span><span class="sxs-lookup"><span data-stu-id="c9204-180">See [ViewModel vs ViewData vs ViewBag vs TempData vs Session in MVC](http://www.mytecbits.com/microsoft/dot-net/viewmodel-viewdata-viewbag-tempdata-mvc) for more information.</span></span>

<span data-ttu-id="c9204-181">這是一種代表模型的 "M"，但不是資料庫類型。</span><span class="sxs-lookup"><span data-stu-id="c9204-181">Well, that was a kind of an "M" for model, but not the database kind.</span></span> <span data-ttu-id="c9204-182">讓我們運用所學的內容，建立電影的資料庫。</span><span class="sxs-lookup"><span data-stu-id="c9204-182">Let's take what we've learned and create a database of movies.</span></span>