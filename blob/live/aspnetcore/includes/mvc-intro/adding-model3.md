
## <a name="test-the-app"></a><span data-ttu-id="bcea4-101">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="bcea4-101">Test the app</span></span>

* <span data-ttu-id="bcea4-102">執行應用程式，然後點選 **Mvc Movie** 連結。</span><span class="sxs-lookup"><span data-stu-id="bcea4-102">Run the app and tap the **Mvc Movie** link.</span></span>
* <span data-ttu-id="bcea4-103">點選 **Create New** 連結，並建立電影。</span><span class="sxs-lookup"><span data-stu-id="bcea4-103">Tap the **Create New** link and create a movie.</span></span>

  ![建立含有內容類型、價格、發行日期和標題等欄位的檢視](../../tutorials/first-mvc-app/adding-model/_static/movies.png)

* <span data-ttu-id="bcea4-105">您可能無法在 `Price` 欄位中輸入小數點或逗號。</span><span class="sxs-lookup"><span data-stu-id="bcea4-105">You may not be able to enter decimal points or commas in the `Price` field.</span></span> <span data-ttu-id="bcea4-106">若要對使用逗號 (",") 作為小數點的非英文地區設定和非英文日期欄位支援 [jQuery 驗證](https://jqueryvalidation.org/)，您必須採取將應用程式全球化的步驟。</span><span class="sxs-lookup"><span data-stu-id="bcea4-106">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="bcea4-107">如需詳細資訊，請參閱 https://github.com/aspnet/Docs/issues/4076 和[其他資源](#additional-resources)。</span><span class="sxs-lookup"><span data-stu-id="bcea4-107">See https://github.com/aspnet/Docs/issues/4076 and [Additional resources](#additional-resources) for more information.</span></span> <span data-ttu-id="bcea4-108">現在，只要輸入如 10 之類的整數。</span><span class="sxs-lookup"><span data-stu-id="bcea4-108">For now, just enter whole numbers like 10.</span></span>

<a name="displayformatdatelocal"></a>

* <span data-ttu-id="bcea4-109">在某些地區設定中，您需要指定日期格式。</span><span class="sxs-lookup"><span data-stu-id="bcea4-109">In some locales you need to specify the date format.</span></span> <span data-ttu-id="bcea4-110">請參閱下列強調顯示的程式碼。</span><span class="sxs-lookup"><span data-stu-id="bcea4-110">See the highlighted code below.</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateFormat.cs?name=snippet_1&highlight=2,10)]

<span data-ttu-id="bcea4-111">我們稍後將在本教學課程中討論 `DataAnnotations`。</span><span class="sxs-lookup"><span data-stu-id="bcea4-111">We'll talk about `DataAnnotations` later in the tutorial.</span></span>

<span data-ttu-id="bcea4-112">點選 **Create** 會導致表單發佈至伺服器，該伺服器是電影資訊儲存在資料庫中的位置。</span><span class="sxs-lookup"><span data-stu-id="bcea4-112">Tapping **Create** causes the form to be posted to the server, where the movie information is saved in a database.</span></span> <span data-ttu-id="bcea4-113">應用程式會重新導向至 */Movies* URL，其中顯示新建立的電影資訊。</span><span class="sxs-lookup"><span data-stu-id="bcea4-113">The app redirects to the */Movies* URL, where the newly created movie information is displayed.</span></span>

![電影檢視顯示新建立的電影清單](../../tutorials/first-mvc-app/adding-model/_static/h.png)

<span data-ttu-id="bcea4-115">建立幾個電影項目。</span><span class="sxs-lookup"><span data-stu-id="bcea4-115">Create a couple more movie entries.</span></span> <span data-ttu-id="bcea4-116">嘗試 **Edit**、**Details** 和 **Delete** 連結，這些連結都可以正常運作。</span><span class="sxs-lookup"><span data-stu-id="bcea4-116">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>
