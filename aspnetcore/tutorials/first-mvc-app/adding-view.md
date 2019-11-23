---
title: 將檢視新增至 ASP.NET Core MVC 應用程式
author: rick-anderson
description: 將檢視新增至簡易的 ASP.NET Core MVC 應用程式
ms.author: riande
ms.date: 8/04/2019
uid: tutorials/first-mvc-app/adding-view
ms.openlocfilehash: de75c3b0651c0cda6629af786d7db9dc83bc4fef
ms.sourcegitcommit: 020c3760492efed71b19e476f25392dda5dd7388
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/12/2019
ms.locfileid: "72288830"
---
# <a name="add-a-view-to-an-aspnet-core-mvc-app"></a>將檢視新增至 ASP.NET Core MVC 應用程式

由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供

::: moniker range=">= aspnetcore-3.0"

在本節中，您將修改 `HelloWorldController` 類別來使用 [Razor](xref:mvc/views/razor) 檢視檔案，完全封裝對用戶端產生 HTML 回應的處理序。

您可以使用 Razor 建立檢視範本檔案。 以 Razor 為基礎的檢視範本具有 *.cshtml* 副檔名。 它們提供了一種使用 C# 建立 HTML 輸出的簡潔方式。

`Index` 方法目前會傳回字串，內含在控制器類別中已直接書寫好的固定訊息。 在 `HelloWorldController` 類別中，以下列程式碼取代 `Index` 方法：

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_4)]

上述程式碼會呼叫控制器的 <xref:Microsoft.AspNetCore.Mvc.Controller.View*> 方法。 它使用檢視範本來產生 HTML 回應。 上述  *方法等控制器方法 (也稱為「動作方法」* `Index`) 通常會傳回 <xref:Microsoft.AspNetCore.Mvc.IActionResult> (或衍生自 <xref:Microsoft.AspNetCore.Mvc.ActionResult> 的類別)，而不會傳回像是 `string` 的類型。

## <a name="add-a-view"></a>新增檢視

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 依序以滑鼠右鍵按一下 *Views* 資料夾、[新增] > [新增資料夾]，然後將資料夾命名為 *HelloWorld*。

* 依序以滑鼠右鍵按一下 *Views/HelloWorld* 資料夾、[新增] > [新增項目]。

* 在 [新增項目 - MvcMovie] 對話方塊內

  * 在右上角的搜尋方塊中，輸入 *view*

  * 選取 [Razor 檢視]

  * 保留 [名稱] 方塊值 *Index.cshtml*。

  * 選取 [新增]

![[新增項目] 對話方塊](adding-view/_static/add_view.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

為 `Index` 新增 `HelloWorldController` 檢視。

* 新增資料夾，並命名為 *Views/HelloWorld*。
* 將檔案新增至 *Views/HelloWorld* 資料夾，並命名為 *Index.cshtml*。

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* 依序以滑鼠右鍵按一下 *Views* 資料夾、[新增] > [新增資料夾]，然後將資料夾命名為 *HelloWorld*。
* 依序以滑鼠右鍵按一下 *Views/HelloWorld* 資料夾、[新增] > [新增檔案]。
* 在 [新增檔案] 對話方塊中：

  * 選取左窗格的 [Web]。
  * 選取中間窗格的 [空的 HTML 檔案]。
  * 在 [名稱] 方塊中，鍵入 **Index.cshtml**。
  * 選取 [新增]。

![[新增項目] 對話方塊](adding-view/_static/add_view_mac.png)

---

使用下列內容取代 *Views/HelloWorld/Index.cshtml* Razor 檢視檔案的內容：

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/HelloWorld/Index1.cshtml?highlight=7)]

巡覽至 `https://localhost:{PORT}/HelloWorld`。 `Index` 中的 `HelloWorldController` 方法不會執行什麼作業；它會執行陳述式 `return View();`，其指定方法應使用檢視範本檔案來呈現瀏覽器的回應。 因為未指定檢視範本檔案名稱，因此 MVC 預設為使用預設檢視檔案。 預設檢視檔案與方法 (`Index`) 擁有相同的名稱，因此會在 */Views/HelloWorld/Index.cshtml* 中使用。 下列影像顯示檢視中硬式編碼的字串 "Hello from our View Template!" 。

![瀏覽器視窗](~/tutorials/first-mvc-app/adding-view/_static/hell_template.png)

## <a name="change-views-and-layout-pages"></a>變更檢視和版面配置頁

選取功能表連結 (**MvcMovie**、**Home** 和 **Privacy**)。 每個頁面會顯示相同的功能表配置。 功能表配置是在 *Views/Shared/_Layout.cshtml* 檔案中實作。 開啟 *Views/Shared/_Layout.cshtml* 檔案。

[版面配置](xref:mvc/views/layout)範本可讓您在某個位置指定網站的 HTML 容器配置，然後將它套用到網站中的多個頁面。 找到 `@RenderBody()` 這行。 `RenderBody` 是顯示您建立之所有檢視特定頁面的預留位置，「包裝」在版面配置頁中。 例如，如果您選取 **Privacy** 連結，**Views/Home/Privacy.cshtml** 檢視就會呈現在 `RenderBody` 方法內。

## <a name="change-the-title-footer-and-menu-link-in-the-layout-file"></a>變更配置檔案中的標題、頁尾及功能表連結

以下列標記取代*Views/Shared/_Layout. cshtml*檔案的內容。 所做的變更已醒目標示：

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Views/Shared/_Layout.cshtml?highlight=6,14,40)]

上述標記進行了下列變更：

* 出現 `MvcMovie` 的 3 處改為 `Movie App`。
* 錨點元素 `<a class="navbar-brand" asp-area="" asp-controller="Home" asp-action="Index">MvcMovie</a>` 改為 `<a class="navbar-brand" asp-controller="Movies" asp-action="Index">Movie App</a>`。

在上述標記中，由於此應用程式未使用`asp-area=""`區域[，因此省略了 ](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) [錨點標籤協助程式屬性](xref:mvc/controllers/areas)和屬性值。

**注意**：尚未執行 `Movies` 控制器。 此時，`Movie App` 連結無法運作。

儲存您的變更並選取 **Privacy** 連結。 請注意，瀏覽器索引標籤上的標題會顯示 **Privacy Policy - Movie App**，而不是 **Privacy Policy - Mvc Movie**：

![Privacy 索引標籤](~/tutorials/first-mvc-app/adding-view/_static/about2.png)

選取 **Home** 連結，並注意標題和錨點文字也要顯示 **Movie App**。 我們能夠在版面配置範本中一次進行變更，並讓網站上的所有頁面反映新的連結文字和新的標題。

檢查 *Views/_ViewStart.cshtml* 檔案：

```HTML
@{
    Layout = "_Layout";
}
```

*Views/_ViewStart.cshtml* 檔案會將 *Views/Shared/_Layout.cshtml* 檔案引入每一個檢視。 `Layout` 屬性可用來設定不同的版面配置檢視，或將它設定為 `null`，因此不會使用任何版面配置檔案。

變更 `<h2>`Views/HelloWorld/Index.cshtml*檢視檔案的標題和* 元素：

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Index2.cshtml?highlight=2,5)]

標題和 `<h2>` 元素略有不同，因此可看出哪一段程式碼變更了顯示。

上述程式碼中的 `ViewData["Title"] = "Movie List";` 會將 `Title` 字典的 `ViewData` 屬性設定為 "Movie List"。 `Title` 屬性則用於版面配置頁中的 `<title>` HTML 元素：

```HTML
<title>@ViewData["Title"] - Movie App</title>
   ```

儲存變更並巡覽至 `https://localhost:{PORT}/HelloWorld`。 請注意，瀏覽器標題、主要標題和次要標題已變更 (如果您在瀏覽器中沒有看到變更，可能檢視的是快取的內容。 在瀏覽器中按 Ctrl + F5，以強制載入伺服器的回應。）瀏覽器標題是使用我們在*Index. cshtml* view 範本中設定的 `ViewData["Title"]` 來建立，並在版面配置檔案中新增額外的「-電影應用程式」。

*Index.cshtml* 檢視範本中的內容已與 *Views/Shared/_Layout.cshtml* 檢視範本合併。 單一 HTML 回應會傳送到瀏覽器。 版面配置範本可讓您輕鬆進行變更，然後套用到應用程式中的所有頁面。 若要深入了解，請參閱[版面配置](xref:mvc/views/layout)。

![電影清單檢視](~/tutorials/first-mvc-app/adding-view/_static/hell3.png)

然而，我們的這一點點「資料」(在此案例中為 "Hello from our View Template!" 訊息) 是硬式編碼的資料。 MVC 應用程式具有 "V" (檢視)，並已取得 "C" (控制器)，但還沒有 "M" (模型)。

## <a name="passing-data-from-the-controller-to-the-view"></a>將資料從控制器傳遞至檢視

回應傳入的 URL 要求時會叫用控制器動作。 控制器類別是撰寫處理傳入瀏覽器要求之程式碼的位置。 控制器會從資料來源擷取資料，並決定要將哪一種回應傳回瀏覽器中。 您可以從控制器使用檢視範本來產生並格式化瀏覽器的 HTML 回應。

控制器負責提供為了讓檢視範本呈現回應所需的資料。 最佳做法：檢視範本**不**應該執行商務邏輯，或直接與資料庫互動。 相反地，檢視範本應該只使用由控制器提供給它的資料。 維護這項「關注點分離」，有助於保持程式碼整潔、可測試且可維護。

目前，`Welcome` 類別中的 `HelloWorldController` 方法會採用 `name` 和 `ID` 參數，然後將值直接輸出到瀏覽器。 與其讓控制器以字串方式呈現這個回應，不如變更控制器以改為使用檢視範本。 檢視範本會產生動態回應，這表示必須將適當數量的資料從控制器傳遞至檢視，以便產生回應。 透過讓控制器將檢視範本需要的動態資料 (參數) 放置在檢視範本之後可以存取的 `ViewData` 字典，即可完成這項操作。

在 *HelloWorldController.cs* 中變更 `Welcome` 方法，將 `Message` 和 `NumTimes` 值新增至 `ViewData` 字典。 `ViewData` 字典是動態物件，這表示您可以使用任何類型；`ViewData` 物件則要在您於其中放入某個項目之後，才會有定義的屬性。 MVC [模型繫結](xref:mvc/models/model-binding)系統會自動將網址列上查詢字串中的具名參數 (`name` 和 `numTimes`) 對應至方法中的參數。 完整的 *HelloWorldController.cs* 檔案如下所示：

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_5)]

`ViewData` 字典物件包含將傳遞至檢視的資料。

建立名為 *Views/HelloWorld/Welcome.cshtml* 的 Welcome 檢視範本。

您將在 *Welcome.cshtml* 檢視範本中建立迴圈，以顯示 "Hello" `NumTimes` 次。 使用下列內容取代 *Views/HelloWorld/Welcome.cshtml* 的內容：

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Welcome.cshtml)]

儲存變更並瀏覽至下列 URL：

`https://localhost:{PORT}/HelloWorld/Welcome?name=Rick&numtimes=4`

資料是使用 [MVC 模型繫結器](xref:mvc/models/model-binding)從 URL 取得並傳遞至控制站。 控制器會將資料封裝成 `ViewData` 字典，並將該物件傳遞至檢視。 接著，檢視會以 HTML 將資料呈現到瀏覽器。

![顯示一個 Welcome 標籤和顯示四次的片語 Hello Rick 的 Privacy 檢視](~/tutorials/first-mvc-app/adding-view/_static/rick2.png)

在上述範例中，使用了 `ViewData` 字典將資料從控制器傳遞至檢視。 稍後在教學課程中，會使用檢視模型將控制器中的資料傳遞至檢視。 傳遞資料的檢視模型方法通常比 `ViewData` 字典方法較為慣用。 如需詳細資訊，請參閱[何時應該使用 ViewBag、ViewData 或 TempData ](https://www.rachelappel.com/when-to-use-viewbag-viewdata-or-tempdata-in-asp-net-mvc-3-applications/) \(英文\)。

在下一個教學課程中，將會建立電影資料庫。

> [!div class="step-by-step"]
> [上一頁](adding-controller.md)
> [下一頁](adding-model.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

在本節中，您將修改 `HelloWorldController` 類別來使用 [Razor](xref:mvc/views/razor) 檢視檔案，完全封裝對用戶端產生 HTML 回應的處理序。

您可以使用 Razor 建立檢視範本檔案。 以 Razor 為基礎的檢視範本具有 *.cshtml* 副檔名。 它們提供了一種使用 C# 建立 HTML 輸出的簡潔方式。

`Index` 方法目前會傳回字串，內含在控制器類別中已直接書寫好的固定訊息。 在 `HelloWorldController` 類別中，以下列程式碼取代 `Index` 方法：

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_4)]

上述程式碼會呼叫控制器的 <xref:Microsoft.AspNetCore.Mvc.Controller.View*> 方法。 它使用檢視範本來產生 HTML 回應。 上述  *方法等控制器方法 (也稱為「動作方法」* `Index`) 通常會傳回 <xref:Microsoft.AspNetCore.Mvc.IActionResult> (或衍生自 <xref:Microsoft.AspNetCore.Mvc.ActionResult> 的類別)，而不會傳回像是 `string` 的類型。

## <a name="add-a-view"></a>新增檢視

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 依序以滑鼠右鍵按一下 *Views* 資料夾、[新增] > [新增資料夾]，然後將資料夾命名為 *HelloWorld*。

* 依序以滑鼠右鍵按一下 *Views/HelloWorld* 資料夾、[新增] > [新增項目]。

* 在 [新增項目 - MvcMovie] 對話方塊內

  * 在右上角的搜尋方塊中，輸入 *view*

  * 選取 [Razor 檢視]

  * 保留 [名稱] 方塊值 *Index.cshtml*。

  * 選取 [新增]

![[新增項目] 對話方塊](adding-view/_static/add_view.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

為 `Index` 新增 `HelloWorldController` 檢視。

* 新增資料夾，並命名為 *Views/HelloWorld*。
* 將檔案新增至 *Views/HelloWorld* 資料夾，並命名為 *Index.cshtml*。

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* 依序以滑鼠右鍵按一下 *Views* 資料夾、[新增] > [新增資料夾]，然後將資料夾命名為 *HelloWorld*。
* 依序以滑鼠右鍵按一下 *Views/HelloWorld* 資料夾、[新增] > [新增檔案]。
* 在 [新增檔案] 對話方塊中：

  * 選取左窗格的 [Web]。
  * 選取中間窗格的 [空的 HTML 檔案]。
  * 在 [名稱] 方塊中，鍵入 **Index.cshtml**。
  * 選取 [新增]。

![[新增項目] 對話方塊](adding-view/_static/add_view_mac.png)

---

使用下列內容取代 *Views/HelloWorld/Index.cshtml* Razor 檢視檔案的內容：

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/HelloWorld/Index1.cshtml?highlight=7)]

巡覽至 `https://localhost:{PORT}/HelloWorld`。 `Index` 中的 `HelloWorldController` 方法不會執行什麼作業；它會執行陳述式 `return View();`，其指定方法應使用檢視範本檔案來呈現瀏覽器的回應。 因為未指定檢視範本檔案名稱，因此 MVC 預設為使用預設檢視檔案。 預設檢視檔案與方法 (`Index`) 擁有相同的名稱，因此會在 */Views/HelloWorld/Index.cshtml* 中使用。 下列影像顯示檢視中硬式編碼的字串 "Hello from our View Template!" 。

![瀏覽器視窗](~/tutorials/first-mvc-app/adding-view/_static/hell_template.png)

## <a name="change-views-and-layout-pages"></a>變更檢視和版面配置頁

選取功能表連結 (**MvcMovie**、**Home** 和 **Privacy**)。 每個頁面會顯示相同的功能表配置。 功能表配置是在 *Views/Shared/_Layout.cshtml* 檔案中實作。 開啟 *Views/Shared/_Layout.cshtml* 檔案。

[版面配置](xref:mvc/views/layout)範本可讓您在某個位置指定網站的 HTML 容器配置，然後將它套用到網站中的多個頁面。 找到 `@RenderBody()` 這行。 `RenderBody` 是顯示您建立之所有檢視特定頁面的預留位置，「包裝」在版面配置頁中。 例如，如果您選取 **Privacy** 連結，**Views/Home/Privacy.cshtml** 檢視就會呈現在 `RenderBody` 方法內。

## <a name="change-the-title-footer-and-menu-link-in-the-layout-file"></a>變更配置檔案中的標題、頁尾及功能表連結

* 在 title 和 footer 元素中，將 `MvcMovie` 變更為 `Movie App`。
* 將錨點元素 `<a class="navbar-brand" asp-area="" asp-controller="Home" asp-action="Index">MvcMovie</a>` 變更為 `<a class="navbar-brand" asp-controller="Movies" asp-action="Index">Movie App</a>`。

下列標記顯示醒目提示的變更：

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Shared/_Layout.cshtml?highlight=6,24,51)]

在上述標記中，由於此應用程式未使用`asp-area`區域[，因此已省略 ](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) [錨點標籤協助程式屬性](xref:mvc/controllers/areas)。

<!-- Routing has changed in 2.2, it's going to the last route.
>[!WARNING]
> We haven't implemented the `Movies` controller yet, so if you click the `Movie App` link, you get a 404 (Not found) error.
-->

**注意**：尚未執行 `Movies` 控制器。 此時，`Movie App` 連結無法運作。

儲存您的變更並選取 **Privacy** 連結。 請注意，瀏覽器索引標籤上的標題會顯示 **Privacy Policy - Movie App**，而不是 **Privacy Policy - Mvc Movie**：

![Privacy 索引標籤](~/tutorials/first-mvc-app/adding-view/_static/about2.png)

選取 **Home** 連結，並注意標題和錨點文字也要顯示 **Movie App**。 我們能夠在版面配置範本中一次進行變更，並讓網站上的所有頁面反映新的連結文字和新的標題。

檢查 *Views/_ViewStart.cshtml* 檔案：

```HTML
@{
    Layout = "_Layout";
}
```

*Views/_ViewStart.cshtml* 檔案會將 *Views/Shared/_Layout.cshtml* 檔案引入每一個檢視。 `Layout` 屬性可用來設定不同的版面配置檢視，或將它設定為 `null`，因此不會使用任何版面配置檔案。

變更 `<h2>`Views/HelloWorld/Index.cshtml*檢視檔案的標題和* 元素：

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Index2.cshtml?highlight=2,5)]

標題和 `<h2>` 元素略有不同，因此可看出哪一段程式碼變更了顯示。

上述程式碼中的 `ViewData["Title"] = "Movie List";` 會將 `Title` 字典的 `ViewData` 屬性設定為 "Movie List"。 `Title` 屬性則用於版面配置頁中的 `<title>` HTML 元素：

```HTML
<title>@ViewData["Title"] - Movie App</title>
   ```

儲存變更並巡覽至 `https://localhost:{PORT}/HelloWorld`。 請注意，瀏覽器標題、主要標題和次要標題已變更 (如果您在瀏覽器中沒有看到變更，可能檢視的是快取的內容。 在瀏覽器中按 Ctrl + F5，以強制載入伺服器的回應。）瀏覽器標題是使用我們在*Index. cshtml* view 範本中設定的 `ViewData["Title"]` 來建立，並在版面配置檔案中新增額外的「-電影應用程式」。

同時也請注意，*Index.cshtml* 檢視範本中的內容如何與 *Views/Shared/_Layout.cshtml* 檢視範本和已傳送至瀏覽器的單一 HTML 回應合併。 版面配置範本可讓您輕鬆進行會套用到應用程式之所有頁面的變更。 若要深入了解，請參閱[版面配置](xref:mvc/views/layout)。

![電影清單檢視](~/tutorials/first-mvc-app/adding-view/_static/hell3.png)

然而，我們的這一點點「資料」(在此案例中為 "Hello from our View Template!" 訊息) 是硬式編碼的資料。 MVC 應用程式具有 "V" (檢視)，並已取得 "C" (控制器)，但還沒有 "M" (模型)。

## <a name="passing-data-from-the-controller-to-the-view"></a>將資料從控制器傳遞至檢視

回應傳入的 URL 要求時會叫用控制器動作。 控制器類別是撰寫處理傳入瀏覽器要求之程式碼的位置。 控制器會從資料來源擷取資料，並決定要將哪一種回應傳回瀏覽器中。 您可以從控制器使用檢視範本來產生並格式化瀏覽器的 HTML 回應。

控制器負責提供為了讓檢視範本呈現回應所需的資料。 最佳做法：檢視範本**不**應該執行商務邏輯，或直接與資料庫互動。 相反地，檢視範本應該只使用由控制器提供給它的資料。 維護這項「關注點分離」，有助於保持程式碼整潔、可測試且可維護。

目前，`Welcome` 類別中的 `HelloWorldController` 方法會採用 `name` 和 `ID` 參數，然後將值直接輸出到瀏覽器。 與其讓控制器以字串方式呈現這個回應，不如變更控制器以改為使用檢視範本。 檢視範本會產生動態回應，這表示必須將適當數量的資料從控制器傳遞至檢視，以便產生回應。 透過讓控制器將檢視範本需要的動態資料 (參數) 放置在檢視範本之後可以存取的 `ViewData` 字典，即可完成這項操作。

在 *HelloWorldController.cs* 中變更 `Welcome` 方法，將 `Message` 和 `NumTimes` 值新增至 `ViewData` 字典。 `ViewData` 字典是動態物件，這表示您可以使用任何類型；`ViewData` 物件則要在您於其中放入某個項目之後，才會有定義的屬性。 MVC [模型繫結](xref:mvc/models/model-binding)系統會自動將網址列上查詢字串中的具名參數 (`name` 和 `numTimes`) 對應至方法中的參數。 完整的 *HelloWorldController.cs* 檔案如下所示：

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_5)]

`ViewData` 字典物件包含將傳遞至檢視的資料。

建立名為 *Views/HelloWorld/Welcome.cshtml* 的 Welcome 檢視範本。

您將在 *Welcome.cshtml* 檢視範本中建立迴圈，以顯示 "Hello" `NumTimes` 次。 使用下列內容取代 *Views/HelloWorld/Welcome.cshtml* 的內容：

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Welcome.cshtml)]

儲存變更並瀏覽至下列 URL：

`https://localhost:{PORT}/HelloWorld/Welcome?name=Rick&numtimes=4`

資料是使用 [MVC 模型繫結器](xref:mvc/models/model-binding)從 URL 取得並傳遞至控制站。 控制器會將資料封裝成 `ViewData` 字典，並將該物件傳遞至檢視。 接著，檢視會以 HTML 將資料呈現到瀏覽器。

![顯示一個 Welcome 標籤和顯示四次的片語 Hello Rick 的 Privacy 檢視](~/tutorials/first-mvc-app/adding-view/_static/rick2.png)

在上述範例中，使用了 `ViewData` 字典將資料從控制器傳遞至檢視。 稍後在教學課程中，會使用檢視模型將控制器中的資料傳遞至檢視。 傳遞資料的檢視模型方法通常比 `ViewData` 字典方法較為慣用。 如需詳細資訊，請參閱[何時應該使用 ViewBag、ViewData 或 TempData ](https://www.rachelappel.com/when-to-use-viewbag-viewdata-or-tempdata-in-asp-net-mvc-3-applications/) \(英文\)。

在下一個教學課程中，將會建立電影資料庫。

> [!div class="step-by-step"]
> [上一頁](adding-controller.md)
> [下一頁](adding-model.md)

::: moniker-end
