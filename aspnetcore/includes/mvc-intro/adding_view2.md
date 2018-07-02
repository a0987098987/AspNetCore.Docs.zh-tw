使用下列內容取代 *Views/HelloWorld/Index.cshtml* Razor 檢視檔案的內容：

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Index.cshtml)]

巡覽至 `http://localhost:xxxx/HelloWorld`。 `HelloWorldController` 中的 `Index` 方法不會執行什麼作業；它會執行陳述式 `return View();`，其指定方法應使用檢視範本檔案來呈現瀏覽器的回應。 因為您沒有明確指定檢視範本檔案的名稱，MVC 預設為使用 */Views/HelloWorld* 資料夾中的 *Index.cshtml* 檢視檔案。 下列影像顯示檢視中硬式編碼的字串 "Hello from our View Template!" 。

![瀏覽器視窗](~/tutorials/first-mvc-app/adding-view/_static/hell_template.png)

如果瀏覽器視窗很小 (例如在行動裝置上)，您可能需要切換 (點選) 右上方的[啟動程序導覽按鈕](http://getbootstrap.com/components/#navbar)，以查看 **Home**、**About** 和 **Contact** 連結。

![醒目提示啟動程序導覽按鈕的瀏覽器視窗](~/tutorials/first-mvc-app/adding-view/_static/1.png)

## <a name="changing-views-and-layout-pages"></a>變更檢視和版面配置頁

點選功能表連結 (**MvcMovie**、**Home**、**About**)。 每個頁面會顯示相同的功能表配置。 功能表配置是在 *Views/Shared/_Layout.cshtml* 檔案中實作。 開啟 *Views/Shared/_Layout.cshtml* 檔案。

[版面配置](xref:mvc/views/layout)範本可讓您在某個位置指定網站的 HTML 容器配置，然後將它套用到網站中的多個頁面。 找到 `@RenderBody()` 這行。 `RenderBody` 是顯示您建立之所有檢視特定頁面的預留位置，「包裝」在版面配置頁中。 例如，如果您選取 **About** 連結，**Views/Home/About.cshtml** 檢視就會呈現在 `RenderBody` 方法內。

## <a name="change-the-title-and-menu-link-in-the-layout-file"></a>變更配置檔案中的標題和功能表連結

在標題項目中將 `MvcMovie` 變更成 `Movie App`。 將版面配置範本中的錨定文字從 `MvcMovie` 變更為 `Movie App`，並將控制器從 `Home` 變更為 `Movies`，如以下的醒目提示：

::: moniker range="<= aspnetcore-2.0"
[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout21.cshtml?highlight=7,31)]
::: moniker-end

>[!WARNING]
> 我們尚未實作 `Movies` 控制器，因此如果您按一下該連結，就會收到 404 (找不到) 錯誤。

儲存變更並點選 **About** 連結。 請注意現在瀏覽器索引標籤上的標題會顯示 **About - Movie App**，而不是 **About - Mvc Movie**。 

![關於標籤](~/tutorials/first-mvc-app/adding-view/_static/about2.png)

點選 [連絡人] 連結，並注意標題和錨定文字也要顯示**電影應用程式**。 我們能夠在版面配置範本中一次進行變更，並讓網站上的所有頁面反映新的連結文字和新的標題。

檢查 *Views/_ViewStart.cshtml* 檔案：


```HTML
@{
    Layout = "_Layout";
}
```

*Views/_ViewStart.cshtml* 檔案會將 *Views/Shared/_Layout.cshtml* 檔案引入每一個檢視。 您可以使用 `Layout` 屬性來設定不同的版面配置檢視，或將它設定為 `null`，因此不會使用任何版面配置檔案。

變更 `Index` 檢視的標題。

開啟 *Views/HelloWorld/Index.cshtml*。 有兩個地方進行變更：

   * 在瀏覽器標題中出現的文字。
   * 次要標頭 (`<h2>` 元素)。

您會使這些變更略有不同，因此可看出哪一段程式碼變更了應用程式的哪個部分。


```HTML
@{
    ViewData["Title"] = "Movie List";
}

<h2>My Movie List</h2>

<p>Hello from our View Template!</p>
```

上述程式碼中的 `ViewData["Title"] = "Movie List";` 會將 `ViewData` 字典的 `Title` 屬性設定為 "Movie List"。 `Title` 屬性則用於版面配置頁中的 `<title>` HTML 元素：


```HTML
<title>@ViewData["Title"] - Movie App</title>
   ```

儲存變更並巡覽至 `http://localhost:xxxx/HelloWorld`。 請注意，瀏覽器標題、主要標題和次要標題已變更 (如果您在瀏覽器中沒有看到變更，可能檢視的是快取的內容。 請在瀏覽器中按 Ctrl + F5 以強制載入來自伺服器的回應)。瀏覽器標題是以我們在 *Index.cshtml* 檢視範本中設定的 `ViewData["Title"]` 和版面配置檔案中新增的額外 "- Movie App" 來建立的。

同時也請注意，*Index.cshtml* 檢視範本中的內容如何與 *Views/Shared/_Layout.cshtml* 檢視範本和已傳送至瀏覽器的單一 HTML 回應合併。 版面配置範本可讓您輕鬆進行會套用到應用程式之所有頁面的變更。 若要深入了解，請參閱[版面配置](xref:mvc/views/layout)。

![電影清單檢視](~/tutorials/first-mvc-app/adding-view/_static/hell3.png)

然而，我們的這一點點「資料」(在此案例中為 "Hello from our View Template!" 訊息) 是硬式編碼的資料。 MVC 應用程式具有 "V" (檢視)，並已取得 "C" (控制器)，但還沒有 "M" (模型)。

## <a name="passing-data-from-the-controller-to-the-view"></a>將資料從控制器傳遞至檢視

回應傳入的 URL 要求時會叫用控制器動作。 控制器類別是您撰寫處理傳入瀏覽器要求之程式碼的位置。 控制器會從資料來源擷取資料，並決定要將哪一種回應傳回瀏覽器中。 您可以從控制器使用檢視範本來產生並格式化瀏覽器的 HTML 回應。

控制器負責提供為了讓檢視範本呈現回應所需的資料。 最佳做法：檢視範本**不**應該執行商務邏輯，或直接與資料庫互動。 相反地，檢視範本應該只使用由控制器提供給它的資料。 維護這項「關注點分離」，有助於保持程式碼整潔、可測試且可維護。

目前，`HelloWorldController` 類別中的 `Welcome` 方法會採用 `name` 和 `ID` 參數，然後將值直接輸出到瀏覽器。 與其讓控制器以字串方式呈現這個回應，不如變更控制器以改為使用檢視範本。 檢視範本會產生動態回應，這表示必須將適當數量的資料從控制器傳遞至檢視，以便產生回應。 透過讓控制器將檢視範本需要的動態資料 (參數) 放置在檢視範本之後可以存取的 `ViewData` 字典，即可完成這項操作。

返回 *HelloWorldController.cs* 檔案並變更 `Welcome` 方法，將 `Message` 和 `NumTimes` 值新增至 `ViewData` 字典。 `ViewData` 字典是動態物件，這表示您可以在其中放置任何項目；`ViewData` 物件則要在您於其中放入某個項目之後，才會有定義的屬性。 MVC [模型繫結](xref:mvc/models/model-binding)系統會自動將網址列上查詢字串中的具名參數 (`name` 和 `numTimes`) 對應至方法中的參數。 完整的 *HelloWorldController.cs* 檔案如下所示：

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_5)]

`ViewData` 字典物件包含將傳遞至檢視的資料。 

建立名為 *Views/HelloWorld/Welcome.cshtml* 的 Welcome 檢視範本。

您將在 *Welcome.cshtml* 檢視範本中建立迴圈，以顯示 "Hello" `NumTimes` 次。 使用下列內容取代 *Views/HelloWorld/Welcome.cshtml* 的內容：

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Welcome.cshtml)]

儲存變更並瀏覽至下列 URL：

`http://localhost:xxxx/HelloWorld/Welcome?name=Rick&numtimes=4`

資料是使用 [MVC 模型繫結器](xref:mvc/models/model-binding)從 URL 取得並傳遞至控制站。 控制器會將資料封裝成 `ViewData` 字典，並將該物件傳遞至檢視。 接著，檢視會以 HTML 將資料呈現到瀏覽器。

![顯示一個 Welcome 標籤和顯示四次的片語 Hello Rick 的 About 檢視](~/tutorials/first-mvc-app/adding-view/_static/rick2.png)

在上述範例中，我們使用 `ViewData` 字典，以便將資料從控制器傳遞至檢視。 稍後在教學課程中，我們將使用檢視模型將控制器中的資料傳遞至檢視。 傳遞資料的檢視模型方法通常比 `ViewData` 字典方法較為慣用。 如需詳細資訊，請參閱 [ViewModel vs ViewData vs ViewBag vs TempData vs Session in MVC](http://www.mytecbits.com/microsoft/dot-net/viewmodel-viewdata-viewbag-tempdata-mvc)(MVC 中的 ViewMode 與 ViewData 與 ViewBag 與 TempData 與 Session)。

這是一種代表模型的 "M"，但不是資料庫類型。 讓我們運用所學的內容，建立電影的資料庫。
