---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-6
title: 建立 JavaScript 用戶端 |Microsoft 文件
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 20360326-b123-4b1e-abae-1d350edf4ce4
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-6
msc.type: authoredcontent
ms.openlocfilehash: 29d50e448e6d282c7db06b9d1946ac221347e1ea
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="create-the-javascript-client"></a><span data-ttu-id="ce0ef-102">建立 JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="ce0ef-102">Create the JavaScript Client</span></span>
====================
<span data-ttu-id="ce0ef-103">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ce0ef-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="ce0ef-104">下載完成的專案</span><span class="sxs-lookup"><span data-stu-id="ce0ef-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="ce0ef-105">在本節中，您將建立使用 JavaScript 的 HTML 用戶端應用程式，而[解 Knockout.js](http://knockoutjs.com/)程式庫。</span><span class="sxs-lookup"><span data-stu-id="ce0ef-105">In this section, you will create the client for the application, using HTML, JavaScript, and the [Knockout.js](http://knockoutjs.com/) library.</span></span> <span data-ttu-id="ce0ef-106">在階段中，我們會建置用戶端應用程式：</span><span class="sxs-lookup"><span data-stu-id="ce0ef-106">We'll build the client app in stages:</span></span>

- <span data-ttu-id="ce0ef-107">顯示書籍清單。</span><span class="sxs-lookup"><span data-stu-id="ce0ef-107">Showing a list of books.</span></span>
- <span data-ttu-id="ce0ef-108">顯示活頁簿的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="ce0ef-108">Showing a book detail.</span></span>
- <span data-ttu-id="ce0ef-109">加入新的書籍。</span><span class="sxs-lookup"><span data-stu-id="ce0ef-109">Adding a new book.</span></span>

<span data-ttu-id="ce0ef-110">Knockout 程式庫會使用模型-檢視-ViewModel (MVVM) 模式：</span><span class="sxs-lookup"><span data-stu-id="ce0ef-110">The Knockout library uses the Model-View-ViewModel (MVVM) pattern:</span></span>

- <span data-ttu-id="ce0ef-111">**模型**商務網域中 （在我們的案例、 書籍和作者） 中的資料的伺服器端表示法。</span><span class="sxs-lookup"><span data-stu-id="ce0ef-111">The **model** is the server-side representation of the data in the business domain (in our case, books and authors).</span></span>
- <span data-ttu-id="ce0ef-112">**檢視**是展示層 」 (HTML)。</span><span class="sxs-lookup"><span data-stu-id="ce0ef-112">The **view** is the presentation layer (HTML).</span></span>
- <span data-ttu-id="ce0ef-113">**檢視模型**是 JavaScript 物件所在的模型。</span><span class="sxs-lookup"><span data-stu-id="ce0ef-113">The **view model** is a JavaScript object that holds the models.</span></span> <span data-ttu-id="ce0ef-114">檢視模型是抽象的 ui 的程式碼。</span><span class="sxs-lookup"><span data-stu-id="ce0ef-114">The view model is a code abstraction of the UI.</span></span> <span data-ttu-id="ce0ef-115">它不了解 HTML 表示法。</span><span class="sxs-lookup"><span data-stu-id="ce0ef-115">It has no knowledge of the HTML representation.</span></span> <span data-ttu-id="ce0ef-116">相反地，它代表抽象功能的檢視，例如&quot;書籍清單&quot;。</span><span class="sxs-lookup"><span data-stu-id="ce0ef-116">Instead, it represents abstract features of the view, such as &quot;a list of books&quot;.</span></span>

<span data-ttu-id="ce0ef-117">檢視是資料繫結至檢視模型。</span><span class="sxs-lookup"><span data-stu-id="ce0ef-117">The view is data-bound to the view model.</span></span> <span data-ttu-id="ce0ef-118">檢視模型的更新會自動反映在檢視中。</span><span class="sxs-lookup"><span data-stu-id="ce0ef-118">Updates to the view model are automatically reflected in the view.</span></span> <span data-ttu-id="ce0ef-119">檢視模型也會取得事件從檢視中，例如按一下按鈕。</span><span class="sxs-lookup"><span data-stu-id="ce0ef-119">The view model also gets events from the view, such as button clicks.</span></span>

![](part-6/_static/image1.png)

<span data-ttu-id="ce0ef-120">這種方法容易變更版面配置和 UI 的應用程式，因為您可以變更繫結，而不用重寫的任何程式碼。</span><span class="sxs-lookup"><span data-stu-id="ce0ef-120">This approach makes it easy to change the layout and UI of your app, because you can change the bindings, without rewriting any code.</span></span> <span data-ttu-id="ce0ef-121">例如，您可能會顯示為項目清單`<ul>`，然後稍後變更為資料表。</span><span class="sxs-lookup"><span data-stu-id="ce0ef-121">For example, you might show a list of items as a `<ul>`, then change it later to a table.</span></span>

## <a name="add-the-knockout-library"></a><span data-ttu-id="ce0ef-122">加入 Knockout 程式庫</span><span class="sxs-lookup"><span data-stu-id="ce0ef-122">Add the Knockout Library</span></span>

<span data-ttu-id="ce0ef-123">在 Visual Studio 中，從**工具**功能表上，選取**程式庫套件管理員**。</span><span class="sxs-lookup"><span data-stu-id="ce0ef-123">In Visual Studio, from the **Tools** menu, select **Library Package Manager**.</span></span> <span data-ttu-id="ce0ef-124">然後選取**Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="ce0ef-124">Then select **Package Manager Console**.</span></span> <span data-ttu-id="ce0ef-125">在 [封裝管理員主控台] 視窗中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="ce0ef-125">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](part-6/samples/sample1.cmd)]

<span data-ttu-id="ce0ef-126">此命令會將 Knockout 檔案加入至指令碼 資料夾。</span><span class="sxs-lookup"><span data-stu-id="ce0ef-126">This command adds the Knockout files to the Scripts folder.</span></span>

## <a name="create-the-view-model"></a><span data-ttu-id="ce0ef-127">建立檢視模型</span><span class="sxs-lookup"><span data-stu-id="ce0ef-127">Create the View Model</span></span>

<span data-ttu-id="ce0ef-128">加入名為 app.js 的指令碼資料夾的 JavaScript 檔案。</span><span class="sxs-lookup"><span data-stu-id="ce0ef-128">Add a JavaScript file named app.js to the Scripts folder.</span></span> <span data-ttu-id="ce0ef-129">(在 方案總管 中，以滑鼠右鍵按一下指令碼 資料夾中，選取**新增**，然後選取**JavaScript 檔案**。)貼上下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="ce0ef-129">(In Solution Explorer, right-click the Scripts folder, select **Add**, then select **JavaScript File**.) Paste in the following code:</span></span>

[!code-javascript[Main](part-6/samples/sample2.js)]

<span data-ttu-id="ce0ef-130">在 Knockout，`observable`類別可讓資料繫結。</span><span class="sxs-lookup"><span data-stu-id="ce0ef-130">In Knockout, the `observable` class enables data-binding.</span></span> <span data-ttu-id="ce0ef-131">當可觀察的內容變更時，observable，以自行更新通知的所有資料繫結控制項。</span><span class="sxs-lookup"><span data-stu-id="ce0ef-131">When the contents of an observable change, the observable notifies all of the data-bound controls, so they can update themselves.</span></span> <span data-ttu-id="ce0ef-132">(`observableArray`類別是陣列版本*observable*。)開始使用，我們的檢視模型中有兩個可預見值：</span><span class="sxs-lookup"><span data-stu-id="ce0ef-132">(The `observableArray` class is the array version of *observable*.) To start with, our view model has two observables:</span></span>

- <span data-ttu-id="ce0ef-133">`books` 保存書籍的清單。</span><span class="sxs-lookup"><span data-stu-id="ce0ef-133">`books` holds the list of books.</span></span>
- <span data-ttu-id="ce0ef-134">`error` 如果 AJAX 呼叫失敗，則包含錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="ce0ef-134">`error` contains an error message if an AJAX call fails.</span></span>

<span data-ttu-id="ce0ef-135">`getAllBooks`方法會 AJAX 呼叫以取得書籍的清單。</span><span class="sxs-lookup"><span data-stu-id="ce0ef-135">The `getAllBooks` method makes an AJAX call to get the list of books.</span></span> <span data-ttu-id="ce0ef-136">然後它會將結果推入`books`陣列。</span><span class="sxs-lookup"><span data-stu-id="ce0ef-136">Then it pushes the result onto the `books` array.</span></span>

<span data-ttu-id="ce0ef-137">`ko.applyBindings`方法是 Knockout 程式庫的一部分。</span><span class="sxs-lookup"><span data-stu-id="ce0ef-137">The `ko.applyBindings` method is part of the Knockout library.</span></span> <span data-ttu-id="ce0ef-138">它會做為參數的檢視模型，並設定資料繫結。</span><span class="sxs-lookup"><span data-stu-id="ce0ef-138">It takes the view model as a parameter and sets up the data binding.</span></span>

## <a name="add-a-script-bundle"></a><span data-ttu-id="ce0ef-139">新增指令碼組合</span><span class="sxs-lookup"><span data-stu-id="ce0ef-139">Add a Script Bundle</span></span>

<span data-ttu-id="ce0ef-140">結合在一起是 ASP.NET 4.5，可讓您輕鬆地結合或多個檔案配套成單一檔案中的功能。</span><span class="sxs-lookup"><span data-stu-id="ce0ef-140">Bundling is a feature in ASP.NET 4.5 that makes it easy to combine or bundle multiple files into a single file.</span></span> <span data-ttu-id="ce0ef-141">結合在一起會要求數目減少到伺服器，可以改善頁面載入時間。</span><span class="sxs-lookup"><span data-stu-id="ce0ef-141">Bundling reduces the number of requests to the server, which can improve page load time.</span></span>

<span data-ttu-id="ce0ef-142">開啟檔案的應用程式\_Start/BundleConfig.cs。</span><span class="sxs-lookup"><span data-stu-id="ce0ef-142">Open the file App\_Start/BundleConfig.cs.</span></span> <span data-ttu-id="ce0ef-143">將下列程式碼加入至 RegisterBundles 方法。</span><span class="sxs-lookup"><span data-stu-id="ce0ef-143">Add the following code to the RegisterBundles method.</span></span>

[!code-csharp[Main](part-6/samples/sample3.cs)]

> [!div class="step-by-step"]
> <span data-ttu-id="ce0ef-144">[上一頁](part-5.md)
> [下一頁](part-7.md)</span><span class="sxs-lookup"><span data-stu-id="ce0ef-144">[Previous](part-5.md)
[Next](part-7.md)</span></span>
