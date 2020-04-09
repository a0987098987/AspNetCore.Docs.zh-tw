---
title: 建置第Blazor一個應用程式
author: guardrex
description: 逐步Blazor構建應用。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/20/2020
no-loc:
- Blazor
uid: tutorials/first-blazor-app
ms.openlocfilehash: 138057c2ceb9ed01bdf958c01f5cf2275387df23
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "79989438"
---
# <a name="build-your-first-opno-locblazor-app"></a>建置第Blazor一個應用程式

作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

本教程演示如何構建和修改Blazor應用。

## <a name="build-components"></a>組建元件

1. 按照本文中的<xref:blazor/get-started>指南為本教程創建Blazor專案。 將專案命名為 *ToDoList*。

1. 瀏覽到 *「主頁」* 資料夾中應用的三個頁面:主頁、計數器和提取數據。 這些頁面會透過 Razor 元件檔案 *Index.razor*、*Counter.razor* 及 *FetchData.razor* 來實作。

1. 在 [計數器] 頁面上，選取 [按我]**** 按鈕以在不重新整理頁面的情況下讓計數器遞增。 在網頁中增加計數器通常需要編寫 JavaScript。 使用Blazor可以改為編寫 C#。

1. 檢查 *Counter.razor`Counter` 檔案中 * 元件的實作。

   *Pages/Counter.razor*：

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Counter1.razor)]

   `Counter` 元件的 UI 是使用 HTML 來定義的。 動態轉譯邏輯 (例如迴圈、條件、運算式) 是使用內嵌的 C# 語法 (稱為 [Razor](xref:mvc/views/razor)) 來新增的。 HTML 標記和 C# 轉譯邏輯會在組建時轉換為元件類別。 所產生 .NET 類別的名稱會與檔案名稱相符。

   元件類別的成員均定義於 `@code` 區塊中。 在 `@code` 區塊中，會指定元件狀態 (屬性、欄位) 來處理事件或定義其他元件邏輯。 然後會使用這些成員作為元件轉譯邏輯的一部分，並用於處理事件。

   選取 [Click me] \(按我\)**** 按鈕時：

   * 會呼叫 `Counter` 元件的已註冊 `onclick` 處理常式 (`IncrementCount` 方法)。
   * `Counter` 元件會重新產生其轉譯樹狀結構。
   * 新的轉譯樹狀結構會與先前的樹狀結構做比較。
   * 只會套用對「文件物件模型」(DOM) 所做的修改。 顯示的計數即會更新。

1. 修改 `Counter` 元件的 C# 邏輯，讓計數遞增 2 而不是 1。

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. 重建並執行應用程式以查看變更。 選取 [按我]**** 按鈕。 計數器會遞增二。

## <a name="use-components"></a>使用元件

使用 HTML 語法來將一個元件包含在另一個元件中。

1. 透過將 `<Counter />` 元素新增至 `Index` 元件 (*Index.razor*)，來將 `Counter` 元件新增至應用程式的 `Index` 元件。

   如果您正在此體驗中使用BlazorWebAssembly,`Index`則元件將`SurveyPrompt`使用 元件。 使用 `<Counter />` 元素取代 `<SurveyPrompt>` 元素。 如果您使用此Blazor伺服器套用的此體驗,則將`<Counter />`元素加入元件`Index`:

   *Pages/Index.razor*：

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Index1.razor?highlight=7)]

1. 重新建置並執行應用程式。 `Index` 元件有自己的計數器。

## <a name="component-parameters"></a>元件參數

元件也可以有參數。 元件參數使用具有`[Parameter]`屬性的元件類上的公共屬性進行定義。 使用這些屬性來指定標記中元件的引數。

1. 更新元件的`@code`C# 代碼,如下所示:

   * 使用`[Parameter]`屬性`IncrementAmount`添加公共屬性。
   * 增加`IncrementCount`的值 時`IncrementAmount`, 將方法更改`currentCount`為使用 屬性。

   *Pages/Counter.razor*：

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Counter.razor?highlight=13,17)]

   <!-- Add back when supported.
       > [!NOTE]
       > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
   -->

1. 使用屬性在 `Index` 元件的 `<Counter>` 元素中指定 `IncrementAmount` 參數。 設定值來讓計數器以 10 遞增。

   *Pages/Index.razor*：

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Index2.razor?highlight=7)]

1. 重新載入 `Index` 元件。 每次選取 [Click me] \(按我\)**** 按鈕時，計數器都會以 10 遞增。 `Counter` 元件中的計數器會繼續遞增一。

## <a name="route-to-components"></a>路由到元件

*Counter.razor* 檔案頂端的 `@page` 指示詞會指定 `Counter` 元件是路由端點。 `Counter` 元件會處理傳送給 `/counter` 的要求。 若沒有 `@page` 指示詞，元件就不會處理路由傳送的要求，但其他元件仍可使用此元件。

## <a name="dependency-injection"></a>相依性插入

### <a name="opno-locblazor-server-experience"></a>Blazor伺服器體驗

如果使用Blazor伺服器應用,則該`WeatherForecastService`服務`Startup.ConfigureServices`在中 註冊為[單例](xref:fundamentals/dependency-injection#service-lifetimes)。 該服務的實體可透過[相依相依 (DI)](xref:fundamentals/dependency-injection)在整個應用中可用:

[!code-csharp[](build-your-first-blazor-app/samples_snapshot/3.x/Startup.cs?highlight=5)]

這個`@inject`指令用於將`WeatherForecastService`服務的實體注入到元件中`FetchData`。

*Pages/FetchData.razor*：

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

`FetchData` 元件會使用插入的服務作為 `ForecastService`，以擷取 `WeatherForecast` 物件的陣列：

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

### <a name="opno-locblazor-webassembly-experience"></a>Blazor網路組裝體驗

如果使用BlazorWebAssembly`HttpClient`應用, 則注入以從*wwwroot/範例資料*資料夾中的*weather.json*檔中獲取天氣預報數據。

*Pages/FetchData.razor*：

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1_client.razor?highlight=7-8)]

循環[`@foreach`](/dotnet/csharp/language-reference/keywords/foreach-in)將每個預測實體呈現為天氣資料表中的行:

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]

## <a name="build-a-todo-list"></a>組建待辦事項清單

在實作簡單待辦事項清單的應用程式中新增元件。

1. 向`Todo`*「主頁」* 資料夾中的應用添加新的 Razor 元件。 在可視化工作室中,右鍵按**下 「頁面」** 資料夾**Add** > ,然後選擇「**添加新專案** > **剃刀元件**」。。 命名元件的檔案*Todo.razor*。 在其他開發環境中,向名為*Todo.razor*的**Pages**資料夾添加空白檔。

1. 提供元件的初始標記：

   ```razor
   @page "/todo"

   <h3>Todo</h3>
   ```

1. 將 `Todo` 元件新增至導覽列。

   `NavMenu` 元件 (*Shared/NavMenu.razor*) 會用於應用程式的版面配置。 版面配置是可讓您避免應用程式中內容重複的元件。

   透過在 *Shared/NavMenu.razor* 檔案中的現有清單項目下方新增下列清單項目標記，為 `Todo` 元件新增 `<NavLink>` 元素：

   ```razor
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. 重新建置並執行應用程式。 瀏覽新的 [待辦事項] 頁面，以確認 `Todo` 元件的連結可以運作。

1. 將 *TodoItem.cs* 檔案新增至專案的根目錄，以保存代表待辦事項的類別。 請使用下列 `TodoItem` 類別的 C# 程式碼：

   [!code-csharp[](build-your-first-blazor-app/samples_snapshot/3.x/TodoItem.cs)]

1. 返回 `Todo` 元件 (*Pages/Todo.razor*)：

   * 在 `@code` 區塊中新增待辦事項的欄位。 `Todo` 元件會使用此欄位來維護待辦事項清單的狀態。
   * 新增未排序的清單標記和 `foreach` 迴圈，將每個待辦事項轉譯為清單項目 (`<li>`)。

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. 應用程式需要 UI 元素，才能將待辦事項新增至清單。 在未排序清單 (`<ul>...</ul>`) 下方新增文字輸出 (`<input>`) 與按鈕 (`<button>`)：

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. 重新建置並執行應用程式。 選取 [Add todo] \(新增待辦事項\)**** 按鈕時，不會發生任何情況，因為事件處理常式並未連接至這個按鈕。

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

1. 已完成的 `Todo` 元件 (*Pages/Todo.razor*)：

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Todo.razor)]

1. 重新建置並執行應用程式。 請新增待辦事項，以測試新程式碼。

> [!div class="nextstepaction"]
> <xref:blazor/components>
