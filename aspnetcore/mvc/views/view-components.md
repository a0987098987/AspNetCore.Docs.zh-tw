---
title: "檢視元件"
author: rick-anderson
description: "檢視元件是任何位置有可重複使用的呈現邏輯。"
keywords: "ASP.NET Core，檢視元件的部分檢視"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: ab4705b7-59d7-4f31-bc97-ea7f292fe926
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/view-components
ms.openlocfilehash: bb8a889c66ec9ca0c0aec7b4a4184d7c19858d78
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/22/2017
---
# <a name="view-components"></a>檢視元件

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample)

## <a name="introducing-view-components"></a>介紹檢視元件

新的 ASP.NET Core MVC 中，以檢視元件類似於部分檢視，但它們是更為強大。 檢視元件不使用模型繫結，並僅相依於呼叫它時，您所提供的資料。 檢視元件：

* 呈現區塊 (chunk)，而不是完整的回應
* 包含的考量相同隔離-和控制器和檢視之間的可測試性優點
* 可以具有參數和商務邏輯
* 這通常是從版面配置頁面叫用

檢視元件是任何位置有可重複使用的呈現邏輯太複雜，部分檢視，例如：

* 動態導覽功能表
* 標記雲端 （其中查詢資料庫）
* 登入 面板
* 購物車
* 最近已發行的文章
* 典型的部落格上的 [資訊看板] 內容
* 登入面板，將每一頁上轉譯，並示範登出或登入，根據使用者的狀態中的記錄檔連結

檢視元件是由兩個部分所組成： 類別 (通常衍生自[ViewComponent](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponent)) 並將它傳回 （通常檢視）。 控制站，像是檢視元件可以 POCO，但大部分的開發人員會想要充分利用的方法和屬性可由衍生自`ViewComponent`。

## <a name="creating-a-view-component"></a>建立檢視的元件

本節包含高階的需求，建立檢視的元件。 本文稍後我們會檢查每個步驟的詳細資料，並建立檢視的元件。

### <a name="the-view-component-class"></a>檢視元件類別

您可以建立檢視元件類別由下列其中一項：

* 衍生自*ViewComponent*
* 裝飾的類別`[ViewComponent]`屬性，或衍生自類別`[ViewComponent]`屬性
* 建立的類別名稱結尾的後置詞*ViewComponent*

控制站，像是檢視元件必須是公用、 以非巢狀，和非抽象類別。 檢視元件名稱是"ViewComponent"後置詞，移除類別名稱。 它也可以明確地指定使用`ViewComponentAttribute.Name`屬性。

檢視元件類別：

* 完全支援建構函式[相依性插入](../../fundamentals/dependency-injection.md)

* 不接受部分中控制站的生命週期，這表示您無法使用[篩選](../controllers/filters.md)中檢視元件

### <a name="view-component-methods"></a>檢視元件方法

檢視元件會定義其邏輯中的`InvokeAsync`方法會傳回`IViewComponentResult`。 參數直接來自檢視元件，不是從模型繫結的引動過程。 檢視元件時，永遠不會直接處理要求。 通常，檢視元件初始化模型，並將其傳遞到檢視中，藉由呼叫`View`方法。 在 [摘要] 檢視元件方法：

* 定義`InvokeAsync`方法會傳回`IViewComponentResult`
* 通常初始化模型，並將其傳遞到檢視中，藉由呼叫`ViewComponent``View`方法
* 參數會來自呼叫的方法，而不是 HTTP、 沒有模型繫結
* 會無法連線到做為 HTTP 端點，即會叫用您的程式碼 （通常是在檢視中）。 檢視元件永遠不會處理要求
* 多載簽章，而不是從目前的 HTTP 要求的任何詳細資料

### <a name="view-search-path"></a>檢視搜尋路徑

執行階段會搜尋下列路徑中的檢視：

   * 檢視 /\<controller_name > /Components/\<view_component_name > /\<view_name >
   * 檢視/共用元件/\<view_component_name > /\<view_name >

檢視元件中預設的檢視名稱是*預設*，這表示您的檢視檔案將通常會命名為*Default.cshtml*。 建立元件檢視結果時，或呼叫時，您可以指定不同的檢視名稱`View`方法。

我們建議您檢視檔案名稱*Default.cshtml*並用*檢視/共用元件/\<view_component_name > /\<view_name >*路徑。 `PriorityList`檢視此範例中使用的元件會使用*Views/Shared/Components/PriorityList/Default.cshtml*檢視元件檢視。

## <a name="invoking-a-view-component"></a>叫用的檢視元件

若要使用的檢視元件，請呼叫下列檢視內：

```html
@Component.InvokeAsync("Name of view component", <anonymous type containing parameters>)
```

參數會傳遞至`InvokeAsync`方法。 `PriorityList`開發文件中的檢視元件會從叫用*Views/Todo/Index.cshtml*檢視檔案。 下列程式碼，在`InvokeAsync`具有兩個參數呼叫方法：

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

## <a name="invoking-a-view-component-as-a-tag-helper"></a>叫用的檢視元件，以標記協助程式

針對 ASP.NET Core 1.1 和更新版本，您可以叫用檢視元件做為[標記協助程式](xref:mvc/views/tag-helpers/intro):

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

依照 pascal 命名法的大小寫慣例類別和方法的參數標記協助程式會轉譯成其[降低 kebab 案例](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101)。 標記協助程式叫用檢視元件會使用`<vc></vc>`項目。 檢視元件的指定方式如下：

```html
<vc:[view-component-name]
  parameter1="parameter1 value"
  parameter2="parameter2 value">
</vc:[view-component-name]>
```

注意： 若要使用檢視元件做為標記協助程式，您必須註冊包含檢視元件使用的組件`@addTagHelper`指示詞。 例如，如果您檢視的元件稱為"M"的組件中，新增下列指示詞，以`_ViewImports.cshtml`檔案：

```csharp
@addTagHelper *, MyWebApp
```

您可以註冊檢視元件做為標記協助程式參考檢視元件的任何檔案。 請參閱[管理標記協助程式範圍](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope)如需有關如何註冊標記協助程式。

`InvokeAsync`本教學課程中使用的方法：

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

在標記協助程式標記：

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

在上述範例`PriorityList`檢視元件會變成`priority-list`。 檢視元件的參數會在小寫 kebab 傳遞做為屬性。

### <a name="invoking-a-view-component-directly-from-a-controller"></a>叫用的檢視元件，直接從控制器

通常會從檢視中，會叫用檢視元件，但您可以直接從控制器方法叫用它們。 檢視元件不會定義端點，如控制站，您可以輕鬆地實作控制器動作傳回的內容`ViewComponentResult`。

在此範例中，檢視元件稱為直接從控制器：

[!code-csharp[Main](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

## <a name="walkthrough-creating-a-simple-view-component"></a>逐步解說： 建立簡單的檢視元件

[下載](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample)、 建置和測試的起始程式碼。 它是簡單的專案與`Todo`控制站，會顯示一份*Todo*項目。

![ToDos 的清單](view-components/_static/2dos.png)

### <a name="add-a-viewcomponent-class"></a>加入 ViewComponent 類別

建立*ViewComponents*資料夾並加入下列`PriorityListViewComponent`類別：

[!code-csharp[Main](view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponent1.cs?name=snippet1)]

附註的程式碼：

* 檢視元件類別可以包含在**任何**專案資料夾中的。
* 因為的類別名稱 PriorityList**ViewComponent**結束的尾碼**ViewComponent**，參考從檢視表的類別元件時，執行階段會使用字串"PriorityList"。 我將說明的詳細更新版本。
* `[ViewComponent]`屬性可以變更用來參考的檢視元件的名稱。 例如，我們無法已命名類別`XYZ`和套用`ViewComponent`屬性：

  ```csharp
  [ViewComponent(Name = "PriorityList")]
     public class XYZ : ViewComponent
     ```

* `[ViewComponent]`上述的屬性會告知檢視元件選擇器，使用名稱`PriorityList`尋找與元件，並從檢視表參考類別元件時，請使用字串"PriorityList"相關聯的檢視時。 我將說明的詳細更新版本。
* 這個元件會使用[相依性插入](../../fundamentals/dependency-injection.md)若要使用的資料內容。
* `InvokeAsync`會公開從一個檢視，以及它可以呼叫這些方法可以採用任意數目的引數。
* `InvokeAsync`方法會傳回一組`ToDo`符合的項目`isDone`和`maxPriority`參數。

### <a name="create-the-view-component-razor-view"></a>建立檢視元件 Razor 檢視

* 建立*檢視/共用元件*資料夾。 此資料夾**必須**命名為*元件*。

* 建立*檢視/共用/元件/PriorityList*資料夾。 此資料夾名稱必須符合檢視元件類別的名稱或減去後置詞的類別名稱 (如果我們遵循慣例，並使用*ViewComponent*中的類別名稱後置字元)。 如果您使用`ViewComponent`屬性，以符合指定的屬性，會需要類別名稱。

* 建立*Views/Shared/Components/PriorityList/Default.cshtml* Razor 檢視：[!code-html[Main](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]
    
   Razor 檢視採用一份`TodoItem`並加以顯示。 如果檢視元件`InvokeAsync`方法未通過 （我們如範例所示），檢視名稱*預設*依慣例用於檢視表名稱。 稍後在教學課程中，我將示範如何將檢視的名稱。 若要覆寫特定控制器的預設樣式，加入檢視到控制器的特定檢視資料夾 (例如*Views/Todo/Components/PriorityList/Default.cshtml)*。
    
    如果控制器專用的檢視元件，您可以將它加入控制器專用資料夾 (*Views/Todo/Components/PriorityList/Default.cshtml*)。

* 新增`div`包含呼叫的底部優先順序清單元件*Views/Todo/index.cshtml*檔案：

    [!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFirst.cshtml?range=34-38)]

標記`@await Component.InvokeAsync`顯示的語法呼叫檢視元件。 第一個引數是我們想要叫用，或是呼叫之元件的名稱。 後續的參數會傳遞至元件。 `InvokeAsync`可以接受任意數目的引數。

測試應用程式。 下圖顯示 ToDo 清單和優先權的項目：

![todo 清單和優先順序的項目](view-components/_static/pi.png)

您也可以直接從控制器呼叫檢視元件：

[!code-csharp[Main](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

![從 IndexVC 動作的優先順序項目](view-components/_static/indexvc.png)

### <a name="specifying-a-view-name"></a>指定檢視表名稱

若要指定非預設的檢視，在某些情況下，可能需要複雜的檢視元件。 下列程式碼示範如何指定"PVC 」 檢視`InvokeAsync`方法。 更新`InvokeAsync`方法中的`PriorityListViewComponent`類別。

[!code-csharp[Main](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponentFinal.cs?highlight=4,5,6,7,8,9&range=28-39)]

複製*Views/Shared/Components/PriorityList/Default.cshtml*檔案到名為檢視*Views/Shared/Components/PriorityList/PVC.cshtml*。 新增標題，以表示正在使用 PVC 檢視中。

[!code-html[Main](../../mvc/views/view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/PVC.cshtml?highlight=3)]

更新*Views/TodoList/Index.cshtml*:

<!-- Views/TodoList/Index.cshtml is never imported, so change to test tutorial -->

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

執行應用程式，並確認 PVC 檢視。

![優先順序檢視元件](view-components/_static/pvc.png)

如果 PVC 檢視不會轉譯，請確認您要呼叫的檢視元件，優先順序為 4 或更新版本。

### <a name="examine-the-view-path"></a>請檢查檢視路徑

* 為三個或更小，則不會傳回 priority 檢視，變更優先順序參數。
* 暫時重新命名*Views/Todo/Components/PriorityList/Default.cshtml*至*1Default.cshtml*。
* 測試應用程式，您會收到下列錯誤：

   <!-- literal_block {"ids": [], "xml:space": "preserve"} -->

   ```
   An unhandled exception occurred while processing the request.
   InvalidOperationException: The view 'Components/PriorityList/Default' was not found. The following locations were searched:
   /Views/ToDo/Components/PriorityList/Default.cshtml
   /Views/Shared/Components/PriorityList/Default.cshtml
   EnsureSuccessful
   ```

* 複製*Views/Todo/Components/PriorityList/1Default.cshtml*至*Views/Shared/Components/PriorityList/Default.cshtml*。
* 加入至某些標記*共用*Todo 檢視元件檢視，來指示檢視取自*共用*資料夾。
* 測試**共用**元件檢視。

![共用元件檢視 ToDo 輸出](view-components/_static/shared.png)

### <a name="avoiding-magic-strings"></a>避免 magic 字串

如果您想編譯時間安全性，您可以硬式編碼的檢視元件名稱取代類別名稱。 建立不含"ViewComponent"尾碼的檢視元件：

[!code-csharp[Main](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityList.cs?highlight=10&range=5-35)]

新增`using`陳述式，您 Razor 檢視檔案，並使用`nameof`運算子：

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexNameof.cshtml?range=1-6,33-)]

## <a name="additional-resources"></a>其他資源

* [在檢視中插入相依性](dependency-injection.md)
