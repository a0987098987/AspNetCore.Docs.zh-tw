---
title: 使用 ASP.NET Core 和 Visual Studio for Windows 建立 Web API
author: rick-anderson
description: 使用 ASP.NET Core MVC 和 Visual Studio for Windows 建置 Web API
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api
ms.openlocfilehash: 1680d5e0be0f4844c904d923af30634c53bd1b81
ms.sourcegitcommit: 9a35906446af7ffd4ccfc18daec38874b5abbef7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/19/2018
ms.locfileid: "34306669"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a><span data-ttu-id="d4e29-103">使用 ASP.NET Core 和 Visual Studio for Windows 建立 Web API</span><span class="sxs-lookup"><span data-stu-id="d4e29-103">Create a Web API with ASP.NET Core and Visual Studio for Windows</span></span>

<span data-ttu-id="d4e29-104">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 和 [Mike Wasson](https://github.com/mikewasson) 提供</span><span class="sxs-lookup"><span data-stu-id="d4e29-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="d4e29-105">本教學課程將建置 Web API 來管理「待辦事項」項目清單，</span><span class="sxs-lookup"><span data-stu-id="d4e29-105">This tutorial builds a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="d4e29-106">並不會建立使用者介面 (UI)。</span><span class="sxs-lookup"><span data-stu-id="d4e29-106">A user interface (UI) isn't created.</span></span>

<span data-ttu-id="d4e29-107">本教學課程有 3 個版本：</span><span class="sxs-lookup"><span data-stu-id="d4e29-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="d4e29-108">Windows：使用 Visual Studio for Windows 建立 Web API (本教學課程)</span><span class="sxs-lookup"><span data-stu-id="d4e29-108">Windows: Web API with Visual Studio for Windows (This tutorial)</span></span>
* <span data-ttu-id="d4e29-109">macOS：[使用 Visual Studio for Mac 建立 Web API](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="d4e29-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="d4e29-110">macOS、Linux、Windows：[使用 Visual Studio Code 建立 Web API](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="d4e29-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="d4e29-111">必要條件</span><span class="sxs-lookup"><span data-stu-id="d4e29-111">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="create-the-project"></a><span data-ttu-id="d4e29-112">建立專案</span><span class="sxs-lookup"><span data-stu-id="d4e29-112">Create the project</span></span>

<span data-ttu-id="d4e29-113">請在 Visual Studio 中遵循下列步驟：</span><span class="sxs-lookup"><span data-stu-id="d4e29-113">Follow these steps in Visual Studio:</span></span>

* <span data-ttu-id="d4e29-114">從 [檔案] 功能表選取 [新增] > [專案]。</span><span class="sxs-lookup"><span data-stu-id="d4e29-114">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="d4e29-115">選取 [ASP.NET Core Web 應用程式] 範本。</span><span class="sxs-lookup"><span data-stu-id="d4e29-115">Select the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="d4e29-116">將專案命名為 *TodoApi*，然後按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="d4e29-116">Name the project *TodoApi* and click **OK**.</span></span>
* <span data-ttu-id="d4e29-117">在 [新增 ASP.NET Core Web 應用程式 - TodoApi] 對話方塊中，選擇 ASP.NET Core 版本。</span><span class="sxs-lookup"><span data-stu-id="d4e29-117">In the **New ASP.NET Core Web Application - TodoApi** dialog, choose the ASP.NET Core version.</span></span> <span data-ttu-id="d4e29-118">選取 [API] 範本，然後按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="d4e29-118">Select the **API** template and click **OK**.</span></span> <span data-ttu-id="d4e29-119">請**勿**選取 [Enable Docker Support] (啟用 Docker 支援)。</span><span class="sxs-lookup"><span data-stu-id="d4e29-119">Do **not** select **Enable Docker Support**.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="d4e29-120">啟動應用程式</span><span class="sxs-lookup"><span data-stu-id="d4e29-120">Launch the app</span></span>

<span data-ttu-id="d4e29-121">在 Visual Studio 中，按 CTRL + F5 來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="d4e29-121">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="d4e29-122">Visual Studio 會啟動瀏覽器並巡覽至 `http://localhost:<port>/api/values`，其中 `<port>` 是隨機選擇的通訊埠編號。</span><span class="sxs-lookup"><span data-stu-id="d4e29-122">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="d4e29-123">Chrome、Microsoft Edge 和 Firefox 顯示下列輸出：</span><span class="sxs-lookup"><span data-stu-id="d4e29-123">Chrome, Microsoft Edge, and Firefox display the following output:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="d4e29-124">若使用 Internet Explorer，您會收到儲存 *values.json* 檔案的提示。</span><span class="sxs-lookup"><span data-stu-id="d4e29-124">If using Internet Explorer, you'll be prompted to save a *values.json* file.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="d4e29-125">新增模型類別</span><span class="sxs-lookup"><span data-stu-id="d4e29-125">Add a model class</span></span>

<span data-ttu-id="d4e29-126">模型是代表應用程式中資料的物件。</span><span class="sxs-lookup"><span data-stu-id="d4e29-126">A model is an object representing the data in the app.</span></span> <span data-ttu-id="d4e29-127">在此情況下，唯一的模型是待辦事項。</span><span class="sxs-lookup"><span data-stu-id="d4e29-127">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="d4e29-128">在方案總管中，以滑鼠右鍵按一下專案。</span><span class="sxs-lookup"><span data-stu-id="d4e29-128">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="d4e29-129">選取 [新增] > [新增資料夾]。</span><span class="sxs-lookup"><span data-stu-id="d4e29-129">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="d4e29-130">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="d4e29-130">Name the folder *Models*.</span></span>

> [!NOTE]
> <span data-ttu-id="d4e29-131">模型類別可位於專案中的任何位置。</span><span class="sxs-lookup"><span data-stu-id="d4e29-131">The model classes can go anywhere in the project.</span></span> <span data-ttu-id="d4e29-132">根據慣例，*Models* 資料夾是供模型類別使用。</span><span class="sxs-lookup"><span data-stu-id="d4e29-132">The *Models* folder is used by convention for model classes.</span></span>

<span data-ttu-id="d4e29-133">在 [方案總管] 中，以滑鼠右鍵按一下 *Models* 資料夾，然後選擇 [新增] > [類別]。</span><span class="sxs-lookup"><span data-stu-id="d4e29-133">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="d4e29-134">將類別命名為 *TodoItem*，然後按一下 [新增]。</span><span class="sxs-lookup"><span data-stu-id="d4e29-134">Name the class *TodoItem* and click **Add**.</span></span>

<span data-ttu-id="d4e29-135">使用下列程式碼更新 `TodoItem` 類別：</span><span class="sxs-lookup"><span data-stu-id="d4e29-135">Update the `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="d4e29-136">此資料庫會在建立 `TodoItem` 時產生 `Id`。</span><span class="sxs-lookup"><span data-stu-id="d4e29-136">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="d4e29-137">建立資料庫內容</span><span class="sxs-lookup"><span data-stu-id="d4e29-137">Create the database context</span></span>

<span data-ttu-id="d4e29-138">「資料庫內容」是為指定的資料模型協調 Entity Framework 功能的主要類別。</span><span class="sxs-lookup"><span data-stu-id="d4e29-138">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="d4e29-139">此類別是透過衍生自 `Microsoft.EntityFrameworkCore.DbContext` 類別來建立。</span><span class="sxs-lookup"><span data-stu-id="d4e29-139">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="d4e29-140">在 [方案總管] 中，以滑鼠右鍵按一下 *Models* 資料夾，然後選擇 [新增] > [類別]。</span><span class="sxs-lookup"><span data-stu-id="d4e29-140">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="d4e29-141">將類別命名為 *TodoContext*，然後按一下 [新增]。</span><span class="sxs-lookup"><span data-stu-id="d4e29-141">Name the class *TodoContext* and click **Add**.</span></span>

<span data-ttu-id="d4e29-142">使用下列程式碼取代該類別：</span><span class="sxs-lookup"><span data-stu-id="d4e29-142">Replace the class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="d4e29-143">新增控制器</span><span class="sxs-lookup"><span data-stu-id="d4e29-143">Add a controller</span></span>

<span data-ttu-id="d4e29-144">在方案總管中，以滑鼠右鍵按一下 *Controllers* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="d4e29-144">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="d4e29-145">選取 [新增] > [新增項目]。</span><span class="sxs-lookup"><span data-stu-id="d4e29-145">Select **Add** > **New Item**.</span></span> <span data-ttu-id="d4e29-146">在 [新增項目] 對話方塊中，選取 [API 控制器類別] 範本。</span><span class="sxs-lookup"><span data-stu-id="d4e29-146">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span> <span data-ttu-id="d4e29-147">將類別命名為 *TodoController*，然後按一下 [新增]。</span><span class="sxs-lookup"><span data-stu-id="d4e29-147">Name the class *TodoController*, and click **Add**.</span></span>

![在搜尋方塊中輸入 controller 且已選取 Web API 控制器的 [新增項目] 對話方塊](first-web-api/_static/new_controller.png)

<span data-ttu-id="d4e29-149">使用下列程式碼取代該類別：</span><span class="sxs-lookup"><span data-stu-id="d4e29-149">Replace the class with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="d4e29-150">啟動應用程式</span><span class="sxs-lookup"><span data-stu-id="d4e29-150">Launch the app</span></span>

<span data-ttu-id="d4e29-151">在 Visual Studio 中，按 CTRL + F5 來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="d4e29-151">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="d4e29-152">Visual Studio 會啟動瀏覽器並巡覽至 `http://localhost:<port>/api/values`，其中 `<port>` 是隨機選擇的通訊埠編號。</span><span class="sxs-lookup"><span data-stu-id="d4e29-152">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="d4e29-153">瀏覽至位於 `http://localhost:<port>/api/todo` 的 `Todo` 控制器。</span><span class="sxs-lookup"><span data-stu-id="d4e29-153">Navigate to the `Todo` controller at `http://localhost:<port>/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
