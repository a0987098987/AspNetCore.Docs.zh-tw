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
ms.openlocfilehash: 726380c42c952f47d6fdff09a811f35a20462d96
ms.sourcegitcommit: 66fca14611eba141d455fe0bd2c37803062e439c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2020
ms.locfileid: "85944943"
---
# <a name="build-a-blazor-todo-list-app"></a>建立 Blazor 待辦事項清單應用程式

作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)

本教學課程說明如何建立和修改 Blazor 應用程式。 您會了解如何：

> [!div class="checklist"]
> * 建立待辦事項清單 Blazor 應用程式專案
> * 修改 Razor 元件
> * 在元件中使用事件處理和資料系結
> * 在應用程式中使用相依性插入（DI）和路由 Blazor

在本教學課程結尾，您將會有一個可運作的待辦事項清單應用程式。

1. Blazor在命令 shell 中建立名為的新應用程式 `TodoList` ：

   ```dotnetcli
   dotnet new blazorserver -o TodoList
   ```

   上述命令會建立名為的資料夾 `TodoList` 來保存應用程式。 使用下列命令將目錄變更至 `TodoList` 資料夾：

   ```dotnetcli
   cd TodoList
   ```

1. 使用下列命令，將新的 `Todo` Razor 元件新增至資料夾中的應用程式 `Pages` ：

   ```dotnetcli
   dotnet new razorcomponent -n Todo -o Pages
   ```

   > [!IMPORTANT]
   > Razor元件檔名稱需要大寫的第一個字母，因此請確認 `Todo` 元件檔案名的開頭為大寫字母 `T` 。

1. 在中， `Pages/Todo.razor` 提供元件的初始標記：

   ```razor
   @page "/todo"

   <h3>Todo</h3>
   ```

1. 將 `Todo` 元件新增至導覽列。

   `NavMenu`元件（ `Shared/NavMenu.razor` ）會用於應用程式的版面配置。 版面配置是可讓您避免應用程式中內容重複的元件。

   新增 `<NavLink>` 元件的專案， `Todo` 方法是在檔案中的現有清單專案下方新增下列清單專案標記 `Shared/NavMenu.razor` ：

   ```razor
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. 重新建置並執行應用程式。 瀏覽新的 [待辦事項] 頁面，以確認 `Todo` 元件的連結可以運作。

1. 將檔案新增 `TodoItem.cs` 至專案的根目錄，以保存代表待辦事項專案的類別。 請使用下列 `TodoItem` 類別的 C# 程式碼：

   [!code-csharp[](build-a-blazor-app/samples_snapshot/3.x/TodoItem.cs)]

1. 回到 `Todo` 元件（ `Pages/Todo.razor` ）：

   * 在 `@code` 區塊中新增待辦事項的欄位。 `Todo` 元件會使用此欄位來維護待辦事項清單的狀態。
   * 新增未排序的清單標記和 `foreach` 迴圈，將每個待辦事項轉譯為清單項目 (`<li>`)。

   [!code-razor[](build-a-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. 應用程式需要 UI 元素，才能將待辦事項新增至清單。 在未排序清單 (`<ul>...</ul>`) 下方新增文字輸出 (`<input>`) 與按鈕 (`<button>`)：

   [!code-razor[](build-a-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. 重新建置並執行應用程式。 **`Add todo`** 選取按鈕時，不會發生任何事，因為事件處理常式未連接到按鈕。

1. 將 `AddTodo` 方法新增至 `Todo` 元件並註冊，以便使用 `@onclick` 屬性來進行按鈕選取。 當選取按鈕時，就會呼叫 `AddTodo` C# 方法：

   [!code-razor[](build-a-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

1. 若要取得新待辦事項的標題，請在 `@code` 區塊頂端新增 `newTodo` 字串欄位，然後使用 `<input>` 元素中的 `bind` 屬性將它繫結至文字輸入的值：

   [!code-razor[](build-a-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```razor
   <input placeholder="Something todo" @bind="newTodo" />
   ```

1. 更新 `AddTodo` 方法，將 `TodoItem` 與指定的標題新增至清單。 請將 `newTodo` 設定為空字串，以清除文字輸入的值：

   [!code-razor[](build-a-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. 重新建置並執行應用程式。 請將一些待辦事項新增至待辦事項清單，以測試新程式碼。

1. 每個待辦事項的標題文字都可設定為可編輯，而核取方塊則可協助使用者記錄已完成的項目。 請為每個待辦事項新增核取方塊輸入，然後將其值繫結至 `IsDone` 屬性。 將 `@todo.Title` 變更為繫結至 `@todo.Title` 的 `<input>` 元素：

   [!code-razor[](build-a-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. 若要確認是否已繫結這些值，請更新 `<h3>` 標頭，以顯示未完成之待辦事項 (`IsDone` 為 `false`) 的數目計數。

   ```razor
   <h3>Todo (@todos.Count(todo => !todo.IsDone))</h3>
   ```

1. 完成的 `Todo` 元件（ `Pages/Todo.razor` ）：

   [!code-razor[](build-a-blazor-app/samples_snapshot/3.x/Todo.razor)]

1. 重新建置並執行應用程式。 請新增待辦事項，以測試新程式碼。

## <a name="next-steps"></a>後續步驟

在本教學課程中，您已了解如何：

> [!div class="checklist"]
> * 建立待辦事項清單 Blazor 應用程式專案
> * 修改 Razor 元件
> * 在元件中使用事件處理和資料系結
> * 在應用程式中使用相依性插入（DI）和路由 Blazor

瞭解 Blazor 專案範本：

> [!div class="nextstepaction"]
> <xref:blazor/templates>
