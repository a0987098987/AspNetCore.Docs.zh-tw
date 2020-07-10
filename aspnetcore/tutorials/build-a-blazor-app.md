---
title: 建立 Blazor 待辦事項清單應用程式
author: guardrex
description: 逐步建立 Blazor 應用程式。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: tutorials/build-a-blazor-app
ms.openlocfilehash: 174a8e561701bb3ebd68ed05e42dfc3d70a9b450
ms.sourcegitcommit: 14c3d111f9d656c86af36ecb786037bf214f435c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/09/2020
ms.locfileid: "86176231"
---
# <a name="build-a-blazor-todo-list-app"></a><span data-ttu-id="86c10-103">建立 Blazor 待辦事項清單應用程式</span><span class="sxs-lookup"><span data-stu-id="86c10-103">Build a Blazor todo list app</span></span>

<span data-ttu-id="86c10-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="86c10-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="86c10-105">本教學課程說明如何建立和修改 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="86c10-105">This tutorial shows you how to build and modify a Blazor app.</span></span> <span data-ttu-id="86c10-106">您會了解如何：</span><span class="sxs-lookup"><span data-stu-id="86c10-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="86c10-107">建立待辦事項清單 Blazor 應用程式專案</span><span class="sxs-lookup"><span data-stu-id="86c10-107">Create a todo list Blazor app project</span></span>
> * <span data-ttu-id="86c10-108">修改 Razor 元件</span><span class="sxs-lookup"><span data-stu-id="86c10-108">Modify Razor components</span></span>
> * <span data-ttu-id="86c10-109">在元件中使用事件處理和資料系結</span><span class="sxs-lookup"><span data-stu-id="86c10-109">Use event handling and data binding in components</span></span>
> * <span data-ttu-id="86c10-110">在應用程式中使用相依性插入 (DI) 和路由 Blazor</span><span class="sxs-lookup"><span data-stu-id="86c10-110">Use dependency injection (DI) and routing in a Blazor app</span></span>

<span data-ttu-id="86c10-111">在本教學課程結尾，您將會有一個可運作的待辦事項清單應用程式。</span><span class="sxs-lookup"><span data-stu-id="86c10-111">At the end of this tutorial, you'll have a working todo list app.</span></span>

1. <span data-ttu-id="86c10-112">Blazor在命令 shell 中建立名為的新應用程式 `TodoList` ：</span><span class="sxs-lookup"><span data-stu-id="86c10-112">Create a new Blazor app named `TodoList` in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o TodoList
   ```

   <span data-ttu-id="86c10-113">上述命令會建立名為的資料夾 `TodoList` 來保存應用程式。</span><span class="sxs-lookup"><span data-stu-id="86c10-113">The preceding command creates a folder named `TodoList` to hold the app.</span></span> <span data-ttu-id="86c10-114">使用下列命令將目錄變更至 `TodoList` 資料夾：</span><span class="sxs-lookup"><span data-stu-id="86c10-114">Change directories to the `TodoList` folder with the following command:</span></span>

   ```dotnetcli
   cd TodoList
   ```

1. <span data-ttu-id="86c10-115">使用下列命令，將新的 `Todo` Razor 元件新增至資料夾中的應用程式 `Pages` ：</span><span class="sxs-lookup"><span data-stu-id="86c10-115">Add a new `Todo` Razor component to the app in the `Pages` folder using the following command:</span></span>

   ```dotnetcli
   dotnet new razorcomponent -n Todo -o Pages
   ```

   > [!IMPORTANT]
   > Razor<span data-ttu-id="86c10-116">元件檔名稱需要大寫的第一個字母，因此請確認 `Todo` 元件檔案名的開頭為大寫字母 `T` 。</span><span class="sxs-lookup"><span data-stu-id="86c10-116"> component file names require a capitalized first letter, so confirm that the `Todo` component file name starts with a capital letter `T`.</span></span>

1. <span data-ttu-id="86c10-117">在中， `Pages/Todo.razor` 提供元件的初始標記：</span><span class="sxs-lookup"><span data-stu-id="86c10-117">In `Pages/Todo.razor` provide the initial markup for the component:</span></span>

   ```razor
   @page "/todo"

   <h3>Todo</h3>
   ```

1. <span data-ttu-id="86c10-118">將 `Todo` 元件新增至導覽列。</span><span class="sxs-lookup"><span data-stu-id="86c10-118">Add the `Todo` component to the navigation bar.</span></span>

   <span data-ttu-id="86c10-119">`NavMenu`元件 (`Shared/NavMenu.razor`) 會用於應用程式的版面配置中。</span><span class="sxs-lookup"><span data-stu-id="86c10-119">The `NavMenu` component (`Shared/NavMenu.razor`) is used in the app's layout.</span></span> <span data-ttu-id="86c10-120">版面配置是可讓您避免應用程式中內容重複的元件。</span><span class="sxs-lookup"><span data-stu-id="86c10-120">Layouts are components that allow you to avoid duplication of content in the app.</span></span>

   <span data-ttu-id="86c10-121">新增 `<NavLink>` 元件的專案， `Todo` 方法是在檔案中的現有清單專案下方新增下列清單專案標記 `Shared/NavMenu.razor` ：</span><span class="sxs-lookup"><span data-stu-id="86c10-121">Add a `<NavLink>` element for the `Todo` component by adding the following list item markup below the existing list items in the `Shared/NavMenu.razor` file:</span></span>

   ```razor
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="86c10-122">重新建置並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="86c10-122">Rebuild and run the app.</span></span> <span data-ttu-id="86c10-123">瀏覽新的 [待辦事項] 頁面，以確認 `Todo` 元件的連結可以運作。</span><span class="sxs-lookup"><span data-stu-id="86c10-123">Visit the new Todo page to confirm that the link to the `Todo` component works.</span></span>

1. <span data-ttu-id="86c10-124">將檔案新增 `TodoItem.cs` 至專案的根目錄，以保存代表待辦事項專案的類別。</span><span class="sxs-lookup"><span data-stu-id="86c10-124">Add a `TodoItem.cs` file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="86c10-125">請使用下列 `TodoItem` 類別的 C# 程式碼：</span><span class="sxs-lookup"><span data-stu-id="86c10-125">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-csharp[](build-a-blazor-app/samples_snapshot/3.x/TodoItem.cs)]

1. <span data-ttu-id="86c10-126">回到 `Todo` 元件 (`Pages/Todo.razor`) ：</span><span class="sxs-lookup"><span data-stu-id="86c10-126">Return to the `Todo` component (`Pages/Todo.razor`):</span></span>

   * <span data-ttu-id="86c10-127">在 `@code` 區塊中新增待辦事項的欄位。</span><span class="sxs-lookup"><span data-stu-id="86c10-127">Add a field for the todo items in an `@code` block.</span></span> <span data-ttu-id="86c10-128">`Todo` 元件會使用此欄位來維護待辦事項清單的狀態。</span><span class="sxs-lookup"><span data-stu-id="86c10-128">The `Todo` component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="86c10-129">新增未排序的清單標記和 `foreach` 迴圈，將每個待辦事項轉譯為清單項目 (`<li>`)。</span><span class="sxs-lookup"><span data-stu-id="86c10-129">Add unordered list markup and a `foreach` loop to render each todo item as a list item (`<li>`).</span></span>

   [!code-razor[](build-a-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. <span data-ttu-id="86c10-130">應用程式需要 UI 元素，才能將待辦事項新增至清單。</span><span class="sxs-lookup"><span data-stu-id="86c10-130">The app requires UI elements for adding todo items to the list.</span></span> <span data-ttu-id="86c10-131">在未排序清單 (`<ul>...</ul>`) 下方新增文字輸出 (`<input>`) 與按鈕 (`<button>`)：</span><span class="sxs-lookup"><span data-stu-id="86c10-131">Add a text input (`<input>`) and a button (`<button>`) below the unordered list (`<ul>...</ul>`):</span></span>

   [!code-razor[](build-a-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. <span data-ttu-id="86c10-132">重新建置並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="86c10-132">Rebuild and run the app.</span></span> <span data-ttu-id="86c10-133">**`Add todo`** 選取按鈕時，不會發生任何事，因為事件處理常式未連接到按鈕。</span><span class="sxs-lookup"><span data-stu-id="86c10-133">When the **`Add todo`** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="86c10-134">將 `AddTodo` 方法新增至 `Todo` 元件並註冊，以便使用 `@onclick` 屬性來進行按鈕選取。</span><span class="sxs-lookup"><span data-stu-id="86c10-134">Add an `AddTodo` method to the `Todo` component and register it for button selections using the `@onclick` attribute.</span></span> <span data-ttu-id="86c10-135">當選取按鈕時，就會呼叫 `AddTodo` C# 方法：</span><span class="sxs-lookup"><span data-stu-id="86c10-135">The `AddTodo` C# method is called when the button is selected:</span></span>

   [!code-razor[](build-a-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

1. <span data-ttu-id="86c10-136">若要取得新待辦事項的標題，請在 `@code` 區塊頂端新增 `newTodo` 字串欄位，然後使用 `<input>` 元素中的 `bind` 屬性將它繫結至文字輸入的值：</span><span class="sxs-lookup"><span data-stu-id="86c10-136">To get the title of the new todo item, add a `newTodo` string field at the top of the `@code` block and bind it to the value of the text input using the `bind` attribute in the `<input>` element:</span></span>

   [!code-razor[](build-a-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```razor
   <input placeholder="Something todo" @bind="newTodo" />
   ```

1. <span data-ttu-id="86c10-137">更新 `AddTodo` 方法，將 `TodoItem` 與指定的標題新增至清單。</span><span class="sxs-lookup"><span data-stu-id="86c10-137">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="86c10-138">請將 `newTodo` 設定為空字串，以清除文字輸入的值：</span><span class="sxs-lookup"><span data-stu-id="86c10-138">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-razor[](build-a-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. <span data-ttu-id="86c10-139">重新建置並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="86c10-139">Rebuild and run the app.</span></span> <span data-ttu-id="86c10-140">請將一些待辦事項新增至待辦事項清單，以測試新程式碼。</span><span class="sxs-lookup"><span data-stu-id="86c10-140">Add some todo items to the todo list to test the new code.</span></span>

1. <span data-ttu-id="86c10-141">每個待辦事項的標題文字都可設定為可編輯，而核取方塊則可協助使用者記錄已完成的項目。</span><span class="sxs-lookup"><span data-stu-id="86c10-141">The title text for each todo item can be made editable, and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="86c10-142">請為每個待辦事項新增核取方塊輸入，然後將其值繫結至 `IsDone` 屬性。</span><span class="sxs-lookup"><span data-stu-id="86c10-142">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="86c10-143">將 `@todo.Title` 變更為繫結至 `@todo.Title` 的 `<input>` 元素：</span><span class="sxs-lookup"><span data-stu-id="86c10-143">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-razor[](build-a-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. <span data-ttu-id="86c10-144">若要確認是否已繫結這些值，請更新 `<h3>` 標頭，以顯示未完成之待辦事項 (`IsDone` 為 `false`) 的數目計數。</span><span class="sxs-lookup"><span data-stu-id="86c10-144">To verify that these values are bound, update the `<h3>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```razor
   <h3>Todo (@todos.Count(todo => !todo.IsDone))</h3>
   ```

1. <span data-ttu-id="86c10-145">已完成的 `Todo` 元件 (`Pages/Todo.razor`) ：</span><span class="sxs-lookup"><span data-stu-id="86c10-145">The completed `Todo` component (`Pages/Todo.razor`):</span></span>

   [!code-razor[](build-a-blazor-app/samples_snapshot/3.x/Todo.razor)]

1. <span data-ttu-id="86c10-146">重新建置並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="86c10-146">Rebuild and run the app.</span></span> <span data-ttu-id="86c10-147">請新增待辦事項，以測試新程式碼。</span><span class="sxs-lookup"><span data-stu-id="86c10-147">Add todo items to test the new code.</span></span>

## <a name="next-steps"></a><span data-ttu-id="86c10-148">後續步驟</span><span class="sxs-lookup"><span data-stu-id="86c10-148">Next steps</span></span>

<span data-ttu-id="86c10-149">在本教學課程中，您已了解如何：</span><span class="sxs-lookup"><span data-stu-id="86c10-149">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="86c10-150">建立待辦事項清單 Blazor 應用程式專案</span><span class="sxs-lookup"><span data-stu-id="86c10-150">Create a todo list Blazor app project</span></span>
> * <span data-ttu-id="86c10-151">修改 Razor 元件</span><span class="sxs-lookup"><span data-stu-id="86c10-151">Modify Razor components</span></span>
> * <span data-ttu-id="86c10-152">在元件中使用事件處理和資料系結</span><span class="sxs-lookup"><span data-stu-id="86c10-152">Use event handling and data binding in components</span></span>
> * <span data-ttu-id="86c10-153">在應用程式中使用相依性插入 (DI) 和路由 Blazor</span><span class="sxs-lookup"><span data-stu-id="86c10-153">Use dependency injection (DI) and routing in a Blazor app</span></span>

<span data-ttu-id="86c10-154">深入瞭解 ASP.NET Core 的工具 Blazor ：</span><span class="sxs-lookup"><span data-stu-id="86c10-154">Learn about tooling for ASP.NET Core Blazor:</span></span>

> [!div class="nextstepaction"]
> <xref:blazor/tooling>
