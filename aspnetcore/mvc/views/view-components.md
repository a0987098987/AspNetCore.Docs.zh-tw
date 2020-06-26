---
title: 檢視 ASP.NET Core 中的元件
author: rick-anderson
description: 了解如何使用 ASP.NET Core 中的檢視元件，以及如何將這些元件新增到應用程式。
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: mvc/views/view-components
ms.openlocfilehash: 40902da52217505822abd9bfd889ecafdd4d88e0
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85407833"
---
# <a name="view-components-in-aspnet-core"></a>檢視 ASP.NET Core 中的元件

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/view-components/sample)（[如何下載](xref:index#how-to-download-a-sample)）

## <a name="view-components"></a>檢視元件

檢視元件與部分檢視類似，但功能更強大。 檢視元件不會使用模型繫結，並且只取決於呼叫它時所提供的資料。 本文是使用控制器和 views 撰寫的，但 view 元件也可以使用 Razor 頁面。

檢視元件：

* 轉譯區塊，而不是整個回應。
* 包含控制器與檢視之間的相同關注點分離和可測試性優點。
* 可以有參數和商務邏輯。
* 它通常是從配置頁面叫用。

如果您的可重複使用轉譯邏輯對於部分檢視而言太過複雜，則檢視元件是處理它的預定位置，例如：

* 動態導覽功能表
* 標籤雲端 (可在其中查詢資料庫)
* 登入面板
* 購物車
* 最近發行的文章
* 一般部落格上的資訊看板內容
* 登入面板，將在每個頁面上轉譯並根據使用者登入狀態來示範登出或登入連結

檢視元件是由兩個部分所組成：類別 (通常衍生自 [ViewComponent](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponent)) 以及它所傳回的結果 (通常是檢視)。 與控制器類似，檢視元件可以是 POCO，但大部分開發人員會想要利用透過衍生自 `ViewComponent` 而取得的方法和屬性。

考慮 view 元件是否符合應用程式的規格時，請考慮 Razor 改為使用元件。 Razor元件也會結合標記與 c # 程式碼，以產生可重複使用的 UI 單位。 Razor元件是針對開發人員在提供用戶端 UI 邏輯和組合時的生產力而設計的。 如需詳細資訊，請參閱 <xref:blazor/components/index> 。

## <a name="creating-a-view-component"></a>建立檢視元件

本節包含建立檢視元件的高階需求。 在本文稍後，我們會詳細檢查每個步驟，並建立檢視元件。

### <a name="the-view-component-class"></a>檢視元件類別

您可以透過下列任一項來建立檢視元件類別：

* 衍生自 *ViewComponent*
* 以 `[ViewComponent]` 屬性裝飾類別，或衍生自具有 `[ViewComponent]` 屬性的類別
* 建立名稱結尾為尾碼 *ViewComponent* 的類別

與控制器類似，檢視元件必須是公用、非巢狀和非抽象類別。 檢視元件名稱是移除 "ViewComponent" 尾碼的類別名稱。 它也可以使用 `ViewComponentAttribute.Name` 屬性明確地指定。

檢視元件類別：

* 完全支援建構函式[相依性插入](../../fundamentals/dependency-injection.md)

* 不參與控制器生命週期，這表示您無法在檢視元件中使用[篩選](../controllers/filters.md)

### <a name="view-component-methods"></a>檢視元件方法

檢視元件會在傳回 `Task<IViewComponentResult>` 的 `InvokeAsync` 方法或傳回 `IViewComponentResult` 的同步 `Invoke` 方法中定義其邏輯。 參數直接來自檢視元件的引動過程，而不是來自模型繫結。 檢視元件絕不會直接處理要求。 通常，檢視元件會初始化模型，並呼叫 `View` 方法將其傳遞至檢視。 簡要來說，檢視元件方法：

* 定義傳回 `Task<IViewComponentResult>` 的 `InvokeAsync` 方法或傳回 `IViewComponentResult` 的同步 `Invoke` 方法。
* 通常會初始化模型，並藉由呼叫方法將它傳遞給視圖 `ViewComponent` `View` 。
* 參數來自呼叫端方法，而非 HTTP。 沒有模型繫結。
* 無法直接當成 HTTP 端點連接。 它們是透過您的程式碼所叫用 (通常是在檢視中)。 檢視元件絕不會處理要求。
* 已多載在簽章上，而非目前 HTTP 要求中的任何詳細資料。

### <a name="view-search-path"></a>檢視搜尋路徑

執行階段會搜尋下列路徑中的檢視：

* /Views/{控制器名稱}/Components/{檢視元件名稱}/{檢視名稱}
* /Views/Shared/Components/{檢視元件名稱}/{檢視名稱}
* /Pages/Shared/Components/{檢視元件名稱}/{檢視名稱}

搜尋路徑適用于使用控制器 + 視圖和頁面的專案 Razor 。

檢視元件的預設檢視名稱是 *Default*，這表示您的檢視檔案通常會命名為 *Default.cshtml*。 建立檢視元件結果時，或呼叫 `View` 方法時，可以指定不同的檢視名稱。

建議您將檢視檔案命名為 *Default.cshtml*，並使用 *Views/Shared/Components/{View Component Name}/{View Name}* 路徑。 此範例中所使用的 `PriorityList` 檢視元件會將 *Views/Shared/Components/PriorityList/Default.cshtml* 用於檢視元件檢視。

### <a name="customize-the-view-search-path"></a>自訂視圖搜尋路徑

若要自訂視圖搜尋路徑，請修改 Razor 的 <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.ViewLocationFormats> 集合。 例如，若要在 "/Components/{View Component Name}/{檢視 Name}" 路徑中搜尋 views，請將新專案新增至集合：

[!code-cs[](view-components/samples_snapshot/2.x/Startup.cs?name=snippet_ViewLocationFormats&highlight=4)]

在上述程式碼中，預留位置 " {0} " 代表路徑 "Components/{View Component name}/{檢視 Name}"。

## <a name="invoking-a-view-component"></a>叫用檢視元件

若要使用檢視元件，請在檢視內呼叫下列項目：

```cshtml
@await Component.InvokeAsync("Name of view component", {Anonymous Type Containing Parameters})
```

參數將傳遞給 `InvokeAsync` 方法。 `PriorityList`本文中開發的 view 元件是從*Views/ToDo/Index. cshtml*視圖檔案叫用。 在下列範例中，`InvokeAsync` 方法是使用兩個參數所呼叫：

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/ToDo/IndexFinal.cshtml?range=35)]

::: moniker range=">= aspnetcore-1.1"

## <a name="invoking-a-view-component-as-a-tag-helper"></a>叫用檢視元件作為標籤協助程式

針對 ASP.NET Core 1.1 和更新版本，您可以叫用檢視元件作為[標籤協助程式](xref:mvc/views/tag-helpers/intro)：

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/ToDo/IndexTagHelper.cshtml?range=37-38)]

標籤協助程式依照 Pascal 命名法大小寫慣例的類別和方法參數會轉譯成其 [Kebab 字體](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101)。 用來叫用檢視元件的標籤協助程式會使用 `<vc></vc>` 項目。 檢視元件指定如下：

```cshtml
<vc:[view-component-name]
  parameter1="parameter1 value"
  parameter2="parameter2 value">
</vc:[view-component-name]>
```

若要使用檢視元件作為標籤協助程式，請使用 `@addTagHelper` 指示詞註冊包含檢視元件的組件。 如果檢視元件位於稱為 `MyWebApp` 的組件中，則請將下列指示詞新增至 *_ViewImports.cshtml* 檔案：

```cshtml
@addTagHelper *, MyWebApp
```

您可以將檢視元件註冊為任何參考檢視元件的檔案標籤協助程式。 如需如何註冊標籤協助程式的詳細資訊，請參閱[管理標籤協助程式範圍](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope)。

本教學課程中使用的 `InvokeAsync` 方法：

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/ToDo/IndexFinal.cshtml?range=35)]

在標籤 (tag) 協助程式標籤 (markup) 中：

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/ToDo/IndexTagHelper.cshtml?range=37-38)]

在上述範例中，`PriorityList` 檢視元件會變成 `priority-list`。 檢視元件的參數會以 Kebab 字體傳遞為屬性。

::: moniker-end

### <a name="invoking-a-view-component-directly-from-a-controller"></a>直接從控制器叫用檢視元件

檢視元件通常是從檢視中進行叫用，但您可以直接從控制器方法叫用它們。 雖然檢視元件不會定義控制器這類端點，但您可以輕鬆地實作控制器動作，以傳回 `ViewComponentResult` 的內容。

在此範例中，是直接從控制器呼叫檢視元件：

[!code-csharp[](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

## <a name="walkthrough-creating-a-simple-view-component"></a>逐步解說：建立簡單檢視元件

[下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/view-components/sample)、建置和測試起始程式碼。 它是具有 `ToDo` 控制器的簡單專案，而此控制器顯示 *ToDO* 項目清單。

![ToDos 清單](view-components/_static/2dos.png)

### <a name="add-a-viewcomponent-class"></a>新增 ViewComponent 類別

建立 *ViewComponents* 資料夾，並新增下列 `PriorityListViewComponent` 類別：

[!code-csharp[](view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponent1.cs?name=snippet1)]

程式碼的注意事項：

* 檢視元件類別可以包含在專案的**任何**資料夾中。
* 因為類別名稱 PriorityList**ViewComponent** 結尾為尾碼 **ViewComponent**，所以從檢視參考類別元件時，執行階段會使用字串 "PriorityList"。 我稍後將更詳細地進行說明。
* `[ViewComponent]` 屬性可以變更用來參考檢視元件的名稱。 例如，我們無法將類別命名為 `XYZ` 以及套用 `ViewComponent` 屬性：

  ```csharp
  [ViewComponent(Name = "PriorityList")]
     public class XYZ : ViewComponent
     ```

* 上面的 `[ViewComponent]` 屬性會告知檢視元件選取器在尋找與元件建立關聯的檢視時使用名稱 `PriorityList`，以及在從檢視參考類別元件時使用字串 "PriorityList"。 我稍後將更詳細地進行說明。
* 元件會使用[相依性插入](../../fundamentals/dependency-injection.md)，讓資料內容可供使用。
* `InvokeAsync` 會公開可以從檢視中呼叫的方法，而且可以採用任意數目的引數。
* `InvokeAsync` 方法會傳回一組符合 `isDone` 和 `maxPriority` 參數的 `ToDo` 項目。

### <a name="create-the-view-component-razor-view"></a>建立視圖元件 Razor 視圖

* 建立 *Views/Shared/Components* 資料夾。 此資料夾**必須**命名為 *Components*。

* 建立 *Views/Shared/Components/PriorityList* 資料夾。 此資料夾名稱必須符合檢視元件類別的名稱，或去掉尾碼的類別名稱 (如果我們遵循慣例，並在類別名稱中使用 *ViewComponent* 尾碼)。 如果您已使用 `ViewComponent` 屬性，則類別名稱需要符合屬性指定。

* 建立*Views/Shared/Components/prioritylist default.cshtml/預設. cshtml* Razor view：


  [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]

   此 Razor 視圖會接受的清單 `TodoItem` ，並顯示這些專案。 如果檢視元件 `InvokeAsync` 方法未傳遞檢視名稱 (如我們的範例所示)，則依照慣例會使用 *Default* 作為檢視名稱。 在教學課程稍後，我將示範如何傳遞檢視的名稱。 若要覆寫特定控制器的預設樣式，請在控制器特定檢視資料夾中新增檢視 (例如 *Views/ToDO/Components/PriorityList/Default.cshtml*)。

    如果 view 元件是控制器特定的，您可以將它新增至控制器特定的資料夾（*Views/ToDo/Components/prioritylist default.cshtml/Default. cshtml*）。

* 將包含優先順序清單元件呼叫的 `div` 新增至 *Views/ToDO/index.cshtml* 檔案底端：

    [!code-cshtml[](view-components/sample/ViewCompFinal/Views/ToDo/IndexFirst.cshtml?range=34-38)]

`@await Component.InvokeAsync` 標記顯示呼叫檢視元件的語法。 第一個引數是我們想要叫用或呼叫之元件的名稱。 後續參數會傳遞至元件。 `InvokeAsync` 可以採用任意數目的引數。

測試應用程式。 下圖顯示 ToDo 清單和優先順序項目：

![ToDo 清單和優先順序項目](view-components/_static/pi.png)

您也可以直接從控制器呼叫檢視元件：

[!code-csharp[](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

![IndexVC 動作中的優先順序項目](view-components/_static/indexvc.png)

### <a name="specifying-a-view-name"></a>指定檢視名稱

在某些情況下，可能需要複雜的檢視元件，才能指定非預設檢視。 下列程式碼示範如何從 `InvokeAsync` 方法指定 "PVC" 檢視。 更新 `PriorityListViewComponent` 類別中的 `InvokeAsync` 方法。

[!code-csharp[](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponentFinal.cs?highlight=4,5,6,7,8,9&range=28-39)]

將 *Views/Shared/Components/PriorityList/Default.cshtml* 檔案複製至名為 *Views/Shared/Components/PriorityList/PVC.cshtml* 的檢視。 新增標題，以指出正在使用 PVC 檢視。

[!code-cshtml[](../../mvc/views/view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/PVC.cshtml?highlight=3)]

更新 *Views/ToDO/Index.cshtml*：

<!-- Views/ToDo/Index.cshtml is never imported, so change to test tutorial -->

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/ToDo/IndexFinal.cshtml?range=35)]

執行應用程式，並驗證 PVC 檢視。

![設定檢視元件優先順序](view-components/_static/pvc.png)

如果未轉譯 PVC 檢視，請驗證您要呼叫的檢視元件優先順序為 4 或以上。

### <a name="examine-the-view-path"></a>檢查檢視路徑

* 將優先順序參數變更為 3 或更小，以不傳回優先順序檢視。
* 暫時將*Views/ToDo/Components/prioritylist default.cshtml/Default. cshtml*重新命名為*1default.cshtml。*
* 測試應用程式，您會收到下列錯誤：

   ```
   An unhandled exception occurred while processing the request.
   InvalidOperationException: The view 'Components/PriorityList/Default' wasn't found. The following locations were searched:
   /Views/ToDo/Components/PriorityList/Default.cshtml
   /Views/Shared/Components/PriorityList/Default.cshtml
   EnsureSuccessful
   ```

* 將 *Views/ToDO/Components/PriorityList/1Default.cshtml* 複製至 *Views/Shared/Components/PriorityList/Default.cshtml*。
* 將一些標記新增至 [*共用*ToDo 視圖] 元件視圖，表示此視圖來自*共用*資料夾。
* 測試 **Shared** 元件檢視。

![含 Shared 元件檢視的 ToDo 輸出](view-components/_static/shared.png)

### <a name="avoiding-hard-coded-strings"></a>避免硬式編碼的字串

如果您想要編譯時間安全，則可以將寫在程式碼中的檢視元件名稱取代為類別名稱。 建立不含 "ViewComponent" 尾碼的檢視元件：

[!code-csharp[](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityList.cs?highlight=10&range=5-35)]

將 `using` 語句新增至您 Razor 的 view 檔案，並使用 `nameof` 運算子：

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/ToDo/IndexNameof.cshtml?range=1-6,35-)]

## <a name="perform-synchronous-work"></a>執行同步工作

如果您不需要執行非同步工作，架構會處理叫用同步 `Invoke` 方法。 下列方法會建立同步 `Invoke` 檢視元件：

```csharp
public class PriorityList : ViewComponent
{
    public IViewComponentResult Invoke(int maxPriority, bool isDone)
    {
        var items = new List<string> { $"maxPriority: {maxPriority}", $"isDone: {isDone}" };
        return View(items);
    }
}
```

View 元件的檔案會 Razor 列出傳遞至方法的字串 `Invoke` （*Views/Home/Components/Prioritylist default.cshtml/預設值*）：

```cshtml
@model List<string>

<h3>Priority Items</h3>
<ul>
    @foreach (var item in Model)
    {
        <li>@item</li>
    }
</ul>
```

::: moniker range=">= aspnetcore-1.1"

您可使用下列其中一種方法，在檔案中叫用 view 元件 Razor （例如*Views/Home/Index. cshtml*）：

* <xref:Microsoft.AspNetCore.Mvc.IViewComponentHelper>
* [標籤協助程式](xref:mvc/views/tag-helpers/intro)

若要使用 <xref:Microsoft.AspNetCore.Mvc.IViewComponentHelper> 方法，請呼叫 `Component.InvokeAsync`：

::: moniker-end

::: moniker range="< aspnetcore-1.1"

View 元件是在檔案中叫 Razor 用（例如*Views/Home/Index. cshtml*） <xref:Microsoft.AspNetCore.Mvc.IViewComponentHelper> 。

呼叫 `Component.InvokeAsync`：

::: moniker-end

```cshtml
@await Component.InvokeAsync(nameof(PriorityList), new { maxPriority = 4, isDone = true })
```

::: moniker range=">= aspnetcore-1.1"

若要使用標籤協助程式，請使用 `@addTagHelper` 指示詞註冊包含檢視元件的組件 (檢視元件位於稱為 `MyWebApp` 的組件中)：

```cshtml
@addTagHelper *, MyWebApp
```

在標記檔案中使用 view 元件標記 Helper Razor ：

```cshtml
<vc:priority-list max-priority="999" is-done="false">
</vc:priority-list>
```

::: moniker-end

的方法簽章 `PriorityList.Invoke` 是同步的，但會 Razor 在標記檔案中尋找並呼叫方法 `Component.InvokeAsync` 。

## <a name="all-view-component-parameters-are-required"></a>所有檢視元件參數均為必要參數

檢視元件中的每個參數都是必要屬性。 請參閱[這個 GitHub 問題](https://github.com/dotnet/AspNetCore/issues/5011)。 若省略了任何參數：

* `InvokeAsync` 方法簽章即不相符，因此系統不會執行此方法。
* ViewComponent 不會轉譯任何標記。
* 系統不會擲回任何錯誤。

## <a name="additional-resources"></a>其他資源

* [檢視中的相依性插入](xref:mvc/views/dependency-injection)
