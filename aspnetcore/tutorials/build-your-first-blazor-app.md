---
title: 建立您的第一個 Blazor 應用程式
author: guardrex
description: 逐步建立 Blazor 應用程式。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 05/19/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: tutorials/first-blazor-app
ms.openlocfilehash: 892663a533a207df84b0fce9af259a7dc212bc9b
ms.sourcegitcommit: 5e462c3328c70f95969d02adce9c71592049f54c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/24/2020
ms.locfileid: "85292772"
---
# <a name="build-your-first-blazor-app"></a>建立您的第一個 Blazor 應用程式

作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)

本教學課程說明如何建立和修改 Blazor 應用程式。 您會了解如何：

> [!div class="checklist"]
> * 建立待辦事項清單 Blazor 應用程式專案
> * 修改 Razor 元件
> * 在元件中使用事件處理和資料系結
> * 在應用程式中使用相依性插入（DI）和路由 Blazor

在本教學課程結尾，您將會有一個可運作的待辦事項清單應用程式。

## <a name="build-components"></a>組建元件

1. 請遵循本文中的指導方針 <xref:blazor/get-started> ，為 Blazor 本教學課程建立專案。 將專案命名為 `ToDoList`。

1. 流覽至每個應用程式在資料夾中的三個頁面 `Pages` ： `Home` 、 `Counter` 和 `Fetch data` 。 這些頁面是由元件檔案 Razor `Index.razor` 、和所執行 `Counter.razor` `FetchData.razor` 。

1. 在 `Counter` 頁面上，選取要在不重新整理頁面的情況下遞增計數器的按鈕。 將網頁中的計數器遞增通常需要撰寫 JavaScript。 使用 Blazor ，您可以改為撰寫 c #。

1. 檢查檔案 `Counter` 中元件的執行 `Counter.razor` 。

   `Pages/Counter.razor`:

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Counter1.razor)]

   `Counter` 元件的 UI 是使用 HTML 來定義的。 動態轉譯邏輯（例如，迴圈、條件、運算式）是使用名為的內嵌 c # 語法加入 [Razor](xref:mvc/views/razor) 。 HTML 標記和 C# 轉譯邏輯會在組建時轉換為元件類別。 所產生 .NET 類別的名稱會與檔案名稱相符。

   元件類別的成員均定義於 `@code` 區塊中。 在 `@code` 區塊中，會指定元件狀態 (屬性、欄位) 來處理事件或定義其他元件邏輯。 然後會使用這些成員作為元件轉譯邏輯的一部分，並用於處理事件。

   選取 [計數器遞增] 按鈕時：

   * 會呼叫 `Counter` 元件的已註冊 `onclick` 處理常式 (`IncrementCount` 方法)。
   * `Counter` 元件會重新產生其轉譯樹狀結構。
   * 新的轉譯樹狀結構會與先前的樹狀結構做比較。
   * 只會套用對「文件物件模型」(DOM) 所做的修改。 顯示的計數即會更新。

1. 修改 `Counter` 元件的 C# 邏輯，讓計數遞增 2 而不是 1。

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. 重建並執行應用程式以查看變更。 選取按鈕。 計數器會遞增二。

## <a name="use-components"></a>使用元件

使用 HTML 語法來將一個元件包含在另一個元件中。

1. 藉 `Counter` `Index` 由將 `<Counter />` 元素新增至 `Index` 元件（），將元件新增至應用程式的元件 `Index.razor` 。

   如果您在 Blazor 此體驗中使用 WebAssembly， `SurveyPrompt` 元件會使用元件 `Index` 。 使用 `<Counter />` 元素取代 `<SurveyPrompt>` 元素。 如果您使用 Blazor 伺服器應用程式來進行此體驗，請將 `<Counter />` 元素新增至 `Index` 元件：

   `Pages/Index.razor`:

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Index1.razor?highlight=7)]

1. 重新建置並執行應用程式。 `Index` 元件有自己的計數器。

## <a name="component-parameters"></a>元件參數

元件也可以有參數。 元件參數是在元件類別上使用具有屬性的公用屬性來定義 [`[Parameter]`](xref:Microsoft.AspNetCore.Components.ParameterAttribute) 。 使用這些屬性來指定標記中元件的引數。

1. 更新元件的 `@code` c # 程式碼，如下所示：

   * 新增 `IncrementAmount` 具有屬性的公用屬性 [`[Parameter]`](xref:Microsoft.AspNetCore.Components.ParameterAttribute) 。
   * `IncrementCount` `IncrementAmount` 當增加的值時，請將方法變更為使用屬性 `currentCount` 。

   `Pages/Counter.razor`:

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Counter.razor?highlight=13,17)]

   <!-- Add back when supported.
       > [!NOTE]
       > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
   -->

1. 使用屬性在 `Index` 元件的 `<Counter>` 元素中指定 `IncrementAmount` 參數。 設定值來讓計數器以 10 遞增。

   `Pages/Index.razor`:

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Index2.razor?highlight=7)]

1. 重新載入 `Index` 元件。 每次選取按鈕時，計數器就會遞增10。 `Counter` 元件中的計數器會繼續遞增一。

## <a name="route-to-components"></a>路由到元件

檔案 `@page` 頂端的 `Counter.razor` 指示詞指定 `Counter` 元件是路由端點。 `Counter` 元件會處理傳送給 `/counter` 的要求。 若沒有 `@page` 指示詞，元件就不會處理路由傳送的要求，但其他元件仍可使用此元件。

## <a name="dependency-injection"></a>相依性插入

### <a name="blazor-server-experience"></a>Blazor伺服器體驗

如果使用 Blazor 伺服器應用程式， `WeatherForecastService` 服務會在中註冊為[單一實例](xref:fundamentals/dependency-injection#service-lifetimes) `Startup.ConfigureServices` 。 服務的實例可透過相依性[插入（DI）](xref:fundamentals/dependency-injection)在整個應用程式中使用：

[!code-csharp[](build-your-first-blazor-app/samples_snapshot/3.x/Startup.cs?highlight=5)]

指示詞 [`@inject`](xref:mvc/views/razor#inject) 是用來將服務的實例插入 `WeatherForecastService` `FetchData` 元件中。

`Pages/FetchData.razor`:

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

`FetchData` 元件會使用插入的服務作為 `ForecastService`，以擷取 `WeatherForecast` 物件的陣列：

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

### <a name="blazor-webassembly-experience"></a>BlazorWebAssembly 體驗

如果使用 Blazor WebAssembly 應用程式， <xref:System.Net.Http.HttpClient> 則會插入以從資料夾中的檔案取得氣象預測資料 `weather.json` `wwwroot/sample-data` 。

`Pages/FetchData.razor`:

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1_client.razor?highlight=7-9)]

[`@foreach`](/dotnet/csharp/language-reference/keywords/foreach-in)迴圈是用來將每個預測實例轉譯為天氣資料之資料表中的資料列：

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]

## <a name="build-a-todo-list"></a>組建待辦事項清單

在實作簡單待辦事項清單的應用程式中新增元件。

1. 將新的 `Todo` Razor 元件新增至資料夾中的應用程式 `Pages` 。 如果您使用 Visual Studio，請在資料夾上按一下滑鼠右鍵 `Pages` ， **Add**然後選取 [  >  **新增專案**] [  >  ** Razor 元件**]。 為元件的檔案命名 `Todo.razor` 。 在其他開發環境中，將空白檔案新增至 `Pages` 名為的資料夾 `Todo.razor` 。 Razor元件檔名稱需要大寫的第一個字母，因此請確認 `Todo` 元件檔案名的開頭為大寫字母 `T` 。

1. 提供元件的初始標記：

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

   [!code-csharp[](build-your-first-blazor-app/samples_snapshot/3.x/TodoItem.cs)]

1. 回到 `Todo` 元件（ `Pages/Todo.razor` ）：

   * 在 `@code` 區塊中新增待辦事項的欄位。 `Todo` 元件會使用此欄位來維護待辦事項清單的狀態。
   * 新增未排序的清單標記和 `foreach` 迴圈，將每個待辦事項轉譯為清單項目 (`<li>`)。

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. 應用程式需要 UI 元素，才能將待辦事項新增至清單。 在未排序清單 (`<ul>...</ul>`) 下方新增文字輸出 (`<input>`) 與按鈕 (`<button>`)：

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. 重新建置並執行應用程式。 **`Add todo`** 選取按鈕時，不會發生任何事，因為事件處理常式未連接到按鈕。

1. 將 `AddTodo` 方法新增至 `Todo` 元件並註冊，以便使用 `@onclick` 屬性來進行按鈕選取。 當選取按鈕時，就會呼叫 `AddTodo` C# 方法：

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

1. 若要取得新待辦事項的標題，請在 `@code` 區塊頂端新增 `newTodo` 字串欄位，然後使用 `<input>` 元素中的 `bind` 屬性將它繫結至文字輸入的值：

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```razor
   <input placeholder="Something todo" @bind="newTodo" />
   ```

1. 更新 `AddTodo` 方法，將 `TodoItem` 與指定的標題新增至清單。 請將 `newTodo` 設定為空字串，以清除文字輸入的值：

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. 重新建置並執行應用程式。 請將一些待辦事項新增至待辦事項清單，以測試新程式碼。

1. 每個待辦事項的標題文字都可設定為可編輯，而核取方塊則可協助使用者記錄已完成的項目。 請為每個待辦事項新增核取方塊輸入，然後將其值繫結至 `IsDone` 屬性。 將 `@todo.Title` 變更為繫結至 `@todo.Title` 的 `<input>` 元素：

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. 若要確認是否已繫結這些值，請更新 `<h3>` 標頭，以顯示未完成之待辦事項 (`IsDone` 為 `false`) 的數目計數。

   ```razor
   <h3>Todo (@todos.Count(todo => !todo.IsDone))</h3>
   ```

1. 完成的 `Todo` 元件（ `Pages/Todo.razor` ）：

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Todo.razor)]

1. 重新建置並執行應用程式。 請新增待辦事項，以測試新程式碼。

## <a name="next-steps"></a>後續步驟

在本教學課程中，您已了解如何：

> [!div class="checklist"]
> * 建立待辦事項清單 Blazor 應用程式專案
> * 修改 Razor 元件
> * 在元件中使用事件處理和資料系結
> * 在應用程式中使用相依性插入（DI）和路由 Blazor

瞭解如何建立和使用元件：

> [!div class="nextstepaction"]
> <xref:blazor/components/index>
