---
title: "使用 ASP.NET Core 和 VS Code 來建立 Web API"
description: "在 macOS、Linux 或 Windows 上，使用 ASP.NET Core MVC 和 Visual Studio Code 建置 Web API"
author: rick-anderson
ms.author: riande
ms.date: 09/22/2017
ms.topic: get-started-article
ms.prod: asp.net-core
ms.technology: aspnet
keywords: "ASP.NET Core, WebAPI, Web API, REST, Mac, Linux, HTTP, 服務, HTTP 服務, VS Code"
manager: wpickett
ms.assetid: 830b4bf5-dd14-423e-9f59-764a6f13a8f6
uid: tutorials/web-api-vsc
ms.openlocfilehash: caf40ee1c2d45d2fbf33b07d707fa4f1be98d31c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-code-on-linux-macos-and-windows"></a><span data-ttu-id="8d37b-104">在 macOS、Linux 和 Windows 上，使用 ASP.NET Core MVC 和 Visual Studio Code 建立 Web API</span><span class="sxs-lookup"><span data-stu-id="8d37b-104">Create a Web API with ASP.NET Core MVC and Visual Studio Code on Linux, macOS, and Windows</span></span>

<span data-ttu-id="8d37b-105">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 和 [Mike Wasson](https://github.com/mikewasson) 提供</span><span class="sxs-lookup"><span data-stu-id="8d37b-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="8d37b-106">在本教學課程中，您將建置 Web API 來管理「待辦事項」項目清單，</span><span class="sxs-lookup"><span data-stu-id="8d37b-106">In this tutorial, you’ll build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="8d37b-107">而不會建置 UI。</span><span class="sxs-lookup"><span data-stu-id="8d37b-107">You won’t build a UI.</span></span>

<span data-ttu-id="8d37b-108">本教學課程有 3 個版本：</span><span class="sxs-lookup"><span data-stu-id="8d37b-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="8d37b-109">macOS、Linux、Windows：使用 Visual Studio Code 建立 Web API (本教學課程)</span><span class="sxs-lookup"><span data-stu-id="8d37b-109">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="8d37b-110">macOS：[使用 Visual Studio for Mac 建立 Web API](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="8d37b-110">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="8d37b-111">Windows：[使用 Visual Studio for Windows 建立 Web API](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="8d37b-111">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="set-up-your-development-environment"></a><span data-ttu-id="8d37b-112">設定您的開發環境</span><span class="sxs-lookup"><span data-stu-id="8d37b-112">Set up your development environment</span></span>

<span data-ttu-id="8d37b-113">下載與安裝：</span><span class="sxs-lookup"><span data-stu-id="8d37b-113">Download and install:</span></span>
- <span data-ttu-id="8d37b-114">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="8d37b-114">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>
- [<span data-ttu-id="8d37b-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8d37b-115">Visual Studio Code</span></span>](https://code.visualstudio.com)
- <span data-ttu-id="8d37b-116">Visual Studio Code [C# 延伸模組](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span><span class="sxs-lookup"><span data-stu-id="8d37b-116">Visual Studio Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span>

## <a name="create-the-project"></a><span data-ttu-id="8d37b-117">建立專案</span><span class="sxs-lookup"><span data-stu-id="8d37b-117">Create the project</span></span>

<span data-ttu-id="8d37b-118">從主控台中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="8d37b-118">From a console, run the following commands:</span></span>

```console
mkdir TodoApi
cd TodoApi
dotnet new webapi
```

<span data-ttu-id="8d37b-119">在 Visual Studio Code (VS Code) 中，開啟 *TodoApi* 資料夾，然後選取 *Startup.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="8d37b-119">Open the *TodoApi* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="8d37b-120">針對下列 [警告] 訊息選取 [是]：「'TodoApi' 中遺漏了建置和偵錯的必要資產。</span><span class="sxs-lookup"><span data-stu-id="8d37b-120">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="8d37b-121">新增它們嗎？」</span><span class="sxs-lookup"><span data-stu-id="8d37b-121">Add them?"</span></span>
- <span data-ttu-id="8d37b-122">針對下列 [資訊] 訊息選取 [還原]：「有未解析的相依性」。</span><span class="sxs-lookup"><span data-stu-id="8d37b-122">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![VS Code 與警告：'TodoApi' 中遺漏了建置和偵錯的必要資產。](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="8d37b-126">按 [偵錯] (F5) 以建置並執行程式。</span><span class="sxs-lookup"><span data-stu-id="8d37b-126">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="8d37b-127">在瀏覽器中，巡覽至 http://localhost:5000/api/values。</span><span class="sxs-lookup"><span data-stu-id="8d37b-127">In a browser navigate to http://localhost:5000/api/values .</span></span> <span data-ttu-id="8d37b-128">此時會顯示下列對話方塊：</span><span class="sxs-lookup"><span data-stu-id="8d37b-128">The following is displayed:</span></span>

`["value1","value2"]`

<span data-ttu-id="8d37b-129">如需使用 VS Code 的祕訣，請參閱 [Visual Studio Code 說明](#visual-studio-code-help)。</span><span class="sxs-lookup"><span data-stu-id="8d37b-129">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="8d37b-130">新增 Entity Framework Core 的支援</span><span class="sxs-lookup"><span data-stu-id="8d37b-130">Add support for Entity Framework Core</span></span>

<span data-ttu-id="8d37b-131">編輯 *TodoApi.csproj* 檔案以安裝 [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) 資料庫提供者。</span><span class="sxs-lookup"><span data-stu-id="8d37b-131">Edit the *TodoApi.csproj* file to install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="8d37b-132">此資料庫提供者可讓 Entity Framework Core 搭配使用記憶體內部資料庫。</span><span class="sxs-lookup"><span data-stu-id="8d37b-132">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

[!code-xml[Main](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]

<span data-ttu-id="8d37b-133">執行 `dotnet restore` 以下載並安裝 EF Core 記憶體內部資料庫提供者。</span><span class="sxs-lookup"><span data-stu-id="8d37b-133">Run `dotnet restore` to download and install the EF Core InMemory DB provider.</span></span> <span data-ttu-id="8d37b-134">您可以從終端機執行 `dotnet restore` 或在 VS Code 中輸入 `⌘⇧P` (macOS) 或 `Ctrl+Shift+P` (Linux)，然後鍵入 **.NET**。</span><span class="sxs-lookup"><span data-stu-id="8d37b-134">You can run `dotnet restore` from the terminal or enter `⌘⇧P` (macOS) or `Ctrl+Shift+P` (Linux) in VS Code and then type **.NET**.</span></span> <span data-ttu-id="8d37b-135">選取 [.NET: Restore Packages] (.NET: 還原套件)。</span><span class="sxs-lookup"><span data-stu-id="8d37b-135">Select **.NET: Restore Packages**.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="8d37b-136">新增模型類別</span><span class="sxs-lookup"><span data-stu-id="8d37b-136">Add a model class</span></span>

<span data-ttu-id="8d37b-137">模型是代表應用程式中資料的物件。</span><span class="sxs-lookup"><span data-stu-id="8d37b-137">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="8d37b-138">在此情況下，唯一的模型是待辦事項。</span><span class="sxs-lookup"><span data-stu-id="8d37b-138">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="8d37b-139">新增名為 *Models* 的資料夾。</span><span class="sxs-lookup"><span data-stu-id="8d37b-139">Add a folder named *Models*.</span></span> <span data-ttu-id="8d37b-140">您可以將模型類別放在專案中的任何位置，但依照慣例會使用 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="8d37b-140">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="8d37b-141">使用下列程式碼新增 `TodoItem` 類別：</span><span class="sxs-lookup"><span data-stu-id="8d37b-141">Add a `TodoItem` class with the following code:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="8d37b-142">此資料庫會在建立 `TodoItem` 時產生 `Id`。</span><span class="sxs-lookup"><span data-stu-id="8d37b-142">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="8d37b-143">建立資料庫內容</span><span class="sxs-lookup"><span data-stu-id="8d37b-143">Create the database context</span></span>

<span data-ttu-id="8d37b-144">「資料庫內容」是為指定的資料模型協調 Entity Framework 功能的主要類別。</span><span class="sxs-lookup"><span data-stu-id="8d37b-144">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="8d37b-145">若要建立此類別，您可以從 `Microsoft.EntityFrameworkCore.DbContext` 類別來衍生。</span><span class="sxs-lookup"><span data-stu-id="8d37b-145">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="8d37b-146">在 *Models* 資料夾中，新增 `TodoContext` 類別：</span><span class="sxs-lookup"><span data-stu-id="8d37b-146">Add a `TodoContext` class in the *Models* folder:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="8d37b-147">新增控制器</span><span class="sxs-lookup"><span data-stu-id="8d37b-147">Add a controller</span></span>

<span data-ttu-id="8d37b-148">在 *Controllers* 資料夾中，建立名為 `TodoController` 的類別。</span><span class="sxs-lookup"><span data-stu-id="8d37b-148">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="8d37b-149">加入下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="8d37b-149">Add the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="8d37b-150">啟動應用程式</span><span class="sxs-lookup"><span data-stu-id="8d37b-150">Launch the app</span></span>

<span data-ttu-id="8d37b-151">在 VS Code 中，按 F5 啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="8d37b-151">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="8d37b-152">巡覽至 http://localhost:5000/api/todo   (剛才所建立的 `Todo` 控制器)。</span><span class="sxs-lookup"><span data-stu-id="8d37b-152">Navigate to  http://localhost:5000/api/todo   (The `Todo` controller we just created).</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="8d37b-153">Visual Studio Code 說明</span><span class="sxs-lookup"><span data-stu-id="8d37b-153">Visual Studio Code help</span></span>

- [<span data-ttu-id="8d37b-154">快速入門</span><span class="sxs-lookup"><span data-stu-id="8d37b-154">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="8d37b-155">偵錯</span><span class="sxs-lookup"><span data-stu-id="8d37b-155">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="8d37b-156">整合式終端機</span><span class="sxs-lookup"><span data-stu-id="8d37b-156">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="8d37b-157">鍵盤快速鍵</span><span class="sxs-lookup"><span data-stu-id="8d37b-157">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="8d37b-158">Mac 鍵盤快速鍵</span><span class="sxs-lookup"><span data-stu-id="8d37b-158">Mac keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="8d37b-159">Linux 鍵盤快速鍵</span><span class="sxs-lookup"><span data-stu-id="8d37b-159">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="8d37b-160">Windows 鍵盤快速鍵</span><span class="sxs-lookup"><span data-stu-id="8d37b-160">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]


