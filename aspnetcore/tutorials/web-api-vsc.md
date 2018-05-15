---
title: 使用 ASP.NET Core 和 Visual Studio Code 來建立 Web API
author: rick-anderson
description: 在 macOS、Linux 或 Windows 上，使用 ASP.NET Core MVC 和 Visual Studio Code 建置 Web API
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/27/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/web-api-vsc
ms.openlocfilehash: f991aeadbaa3f7696d6fd6b8791d26248e7560a6
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/03/2018
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-code"></a><span data-ttu-id="990d2-103">使用 ASP.NET Core 和 Visual Studio Code 來建立 Web API</span><span class="sxs-lookup"><span data-stu-id="990d2-103">Create a Web API with ASP.NET Core and Visual Studio Code</span></span>

<span data-ttu-id="990d2-104">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 和 [Mike Wasson](https://github.com/mikewasson) 提供</span><span class="sxs-lookup"><span data-stu-id="990d2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

::: moniker range="= aspnetcore-2.1"
[!INCLUDE[](~/includes/2.1.md)]
::: moniker-end

<span data-ttu-id="990d2-106">在本教學課程中，請建置 Web API 來管理「待辦事項」項目清單。</span><span class="sxs-lookup"><span data-stu-id="990d2-106">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="990d2-107">不會建構 UI。</span><span class="sxs-lookup"><span data-stu-id="990d2-107">A UI isn't constructed.</span></span>

<span data-ttu-id="990d2-108">本教學課程有 3 個版本：</span><span class="sxs-lookup"><span data-stu-id="990d2-108">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="990d2-109">macOS、Linux、Windows：使用 Visual Studio Code 建立 Web API (本教學課程)</span><span class="sxs-lookup"><span data-stu-id="990d2-109">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="990d2-110">macOS：[使用 Visual Studio for Mac 建立 Web API](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="990d2-110">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="990d2-111">Windows：[使用 Visual Studio for Windows 建立 Web API](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="990d2-111">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="990d2-112">必要條件</span><span class="sxs-lookup"><span data-stu-id="990d2-112">Prerequisites</span></span>

[!INCLUDE[prerequisites](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-the-project"></a><span data-ttu-id="990d2-113">建立專案</span><span class="sxs-lookup"><span data-stu-id="990d2-113">Create the project</span></span>

<span data-ttu-id="990d2-114">從主控台中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="990d2-114">From a console, run the following commands:</span></span>

```console
dotnet new webapi -o TodoApi
code TodoApi
```

<span data-ttu-id="990d2-115">*TodoApi* 資料夾會在 Visual Studio Code (VS Code) 中開啟。</span><span class="sxs-lookup"><span data-stu-id="990d2-115">The *TodoApi* folder opens in Visual Studio Code (VS Code).</span></span> <span data-ttu-id="990d2-116">選取 *Startup.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="990d2-116">Select the *Startup.cs* file.</span></span>

* <span data-ttu-id="990d2-117">針對下列 [警告] 訊息選取 [是]：「'TodoApi' 中遺漏了建置和偵錯的必要資產。</span><span class="sxs-lookup"><span data-stu-id="990d2-117">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="990d2-118">新增它們嗎？」</span><span class="sxs-lookup"><span data-stu-id="990d2-118">Add them?"</span></span>
* <span data-ttu-id="990d2-119">針對下列 [資訊] 訊息選取 [還原]：「有未解析的相依性」。</span><span class="sxs-lookup"><span data-stu-id="990d2-119">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![VS Code 與警告：'TodoApi' 中遺漏了建置和偵錯的必要資產。](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="990d2-123">按 [偵錯] (F5) 以建置並執行程式。</span><span class="sxs-lookup"><span data-stu-id="990d2-123">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="990d2-124">在瀏覽器中，巡覽至 http://localhost:5000/api/values。</span><span class="sxs-lookup"><span data-stu-id="990d2-124">In a browser, navigate to http://localhost:5000/api/values.</span></span> <span data-ttu-id="990d2-125">下列輸出隨即顯示：</span><span class="sxs-lookup"><span data-stu-id="990d2-125">The following output is displayed:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="990d2-126">如需使用 VS Code 的祕訣，請參閱 [Visual Studio Code 說明](#visual-studio-code-help)。</span><span class="sxs-lookup"><span data-stu-id="990d2-126">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="990d2-127">新增 Entity Framework Core 的支援</span><span class="sxs-lookup"><span data-stu-id="990d2-127">Add support for Entity Framework Core</span></span>

:::moniker range="<= aspnetcore-2.0"
<span data-ttu-id="990d2-128">在 ASP.NET Core 2.0 中建立新專案會在 *TodoApi.csproj* 檔案中新增 [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) 套件參考：</span><span class="sxs-lookup"><span data-stu-id="990d2-128">Creating a new project in ASP.NET Core 2.0 adds the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) package reference to the *TodoApi.csproj* file:</span></span>

<span data-ttu-id="990d2-129">[!code-xml[](first-web-api/samples/2.0/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="990d2-129">[!code-xml[](first-web-api/samples/2.0/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]</span></span>
:::moniker-end
:::moniker range=">= aspnetcore-2.1"
<span data-ttu-id="990d2-130">在 ASP.NET Core 2.1 或更新版本中建立新專案會在 *TodoApi.csproj* 檔案中新增 [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) 套件參考：</span><span class="sxs-lookup"><span data-stu-id="990d2-130">Creating a new project in ASP.NET Core 2.1 or later adds the [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) package reference to the *TodoApi.csproj* file:</span></span>

<span data-ttu-id="990d2-131">[!code-xml[](first-web-api/samples/2.1/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="990d2-131">[!code-xml[](first-web-api/samples/2.1/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]</span></span>
:::moniker-end

<span data-ttu-id="990d2-132">無須另行安裝 [Entity Framework Core InMemory](/ef/core/providers/in-memory/) 資料庫提供者。</span><span class="sxs-lookup"><span data-stu-id="990d2-132">There's no need to install the [Entity Framework Core InMemory](/ef/core/providers/in-memory/) database provider separately.</span></span> <span data-ttu-id="990d2-133">此資料庫提供者可讓 Entity Framework Core 搭配使用記憶體內部資料庫。</span><span class="sxs-lookup"><span data-stu-id="990d2-133">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="990d2-134">新增模型類別</span><span class="sxs-lookup"><span data-stu-id="990d2-134">Add a model class</span></span>

<span data-ttu-id="990d2-135">模型是代表應用程式中資料的物件。</span><span class="sxs-lookup"><span data-stu-id="990d2-135">A model is an object representing the data in your app.</span></span> <span data-ttu-id="990d2-136">在此情況下，唯一的模型是待辦事項。</span><span class="sxs-lookup"><span data-stu-id="990d2-136">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="990d2-137">新增名為 *Models* 的資料夾。</span><span class="sxs-lookup"><span data-stu-id="990d2-137">Add a folder named *Models*.</span></span> <span data-ttu-id="990d2-138">您可以將模型類別放在專案中的任何位置，但依照慣例會使用 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="990d2-138">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="990d2-139">使用下列程式碼新增 `TodoItem` 類別：</span><span class="sxs-lookup"><span data-stu-id="990d2-139">Add a `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="990d2-140">此資料庫會在建立 `TodoItem` 時產生 `Id`。</span><span class="sxs-lookup"><span data-stu-id="990d2-140">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="990d2-141">建立資料庫內容</span><span class="sxs-lookup"><span data-stu-id="990d2-141">Create the database context</span></span>

<span data-ttu-id="990d2-142">「資料庫內容」是為指定的資料模型協調 Entity Framework 功能的主要類別。</span><span class="sxs-lookup"><span data-stu-id="990d2-142">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="990d2-143">若要建立此類別，您可以從 `Microsoft.EntityFrameworkCore.DbContext` 類別來衍生。</span><span class="sxs-lookup"><span data-stu-id="990d2-143">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="990d2-144">在 *Models* 資料夾中，新增 `TodoContext` 類別：</span><span class="sxs-lookup"><span data-stu-id="990d2-144">Add a `TodoContext` class in the *Models* folder:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="990d2-145">新增控制器</span><span class="sxs-lookup"><span data-stu-id="990d2-145">Add a controller</span></span>

<span data-ttu-id="990d2-146">在 *Controllers* 資料夾中，建立名為 `TodoController` 的類別。</span><span class="sxs-lookup"><span data-stu-id="990d2-146">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="990d2-147">將其內容取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="990d2-147">Replace its contents with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="990d2-148">啟動應用程式</span><span class="sxs-lookup"><span data-stu-id="990d2-148">Launch the app</span></span>

<span data-ttu-id="990d2-149">在 VS Code 中，按 F5 啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="990d2-149">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="990d2-150">巡覽至 http://localhost:5000/api/todo (我們建立的 `Todo` 控制器)。</span><span class="sxs-lookup"><span data-stu-id="990d2-150">Navigate to http://localhost:5000/api/todo (the `Todo` controller we created).</span></span>

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="990d2-151">Visual Studio Code 說明</span><span class="sxs-lookup"><span data-stu-id="990d2-151">Visual Studio Code help</span></span>

* [<span data-ttu-id="990d2-152">快速入門</span><span class="sxs-lookup"><span data-stu-id="990d2-152">Getting started</span></span>](https://code.visualstudio.com/docs)
* [<span data-ttu-id="990d2-153">偵錯</span><span class="sxs-lookup"><span data-stu-id="990d2-153">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
* [<span data-ttu-id="990d2-154">整合式終端機</span><span class="sxs-lookup"><span data-stu-id="990d2-154">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
* [<span data-ttu-id="990d2-155">鍵盤快速鍵</span><span class="sxs-lookup"><span data-stu-id="990d2-155">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  * [<span data-ttu-id="990d2-156">macOS 鍵盤快速鍵</span><span class="sxs-lookup"><span data-stu-id="990d2-156">macOS keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  * [<span data-ttu-id="990d2-157">Linux 鍵盤快速鍵</span><span class="sxs-lookup"><span data-stu-id="990d2-157">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  * [<span data-ttu-id="990d2-158">Windows 鍵盤快速鍵</span><span class="sxs-lookup"><span data-stu-id="990d2-158">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]
