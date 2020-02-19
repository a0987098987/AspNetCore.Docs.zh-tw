---
title: ASP.NET Core Blazor advanced 案例
author: guardrex
description: 深入瞭解 Blazor中的先進案例，包括如何將手動 RenderTreeBuilder 邏輯納入應用程式中。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/advanced-scenarios
ms.openlocfilehash: 5e0618faa7b1b5e4cc15e30d9c16afaf7ccabaf0
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/19/2020
ms.locfileid: "77453258"
---
# <a name="aspnet-core-blazor-advanced-scenarios"></a>ASP.NET Core Blazor 的先進案例

By [Luke Latham](https://github.com/guardrex)和[Daniel Roth](https://github.com/danroth27)

## <a name="manual-rendertreebuilder-logic"></a>手動 RenderTreeBuilder 邏輯

`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` 提供操作元件和元素的方法，包括在程式碼中C#手動建立元件。

> [!NOTE]
> 使用 `RenderTreeBuilder` 建立元件是一個先進的案例。 格式不正確的元件（例如，未封閉的標記標記）可能會導致未定義的行為。

請考慮下列 `PetDetails` 元件，其可手動內建于另一個元件中：

```razor
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    public string PetDetailsQuote { get; set; }
}
```

在下列範例中，`CreateComponent` 方法中的迴圈會產生三個 `PetDetails` 元件。 呼叫 `RenderTreeBuilder` 方法來建立元件（`OpenComponent` 和 `AddAttribute`）時，序號是源程式碼號。 Blazor 差異演算法依賴對應于不同程式程式碼的序號，而不是相異的呼叫調用。 建立具有 `RenderTreeBuilder` 方法的元件時，請硬式編碼序號的引數。 **使用計算或計數器來產生序號可能會導致效能不佳。** 如需詳細資訊，請參閱[序號與程式程式碼號，而不是執行順序](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order)一節。

`BuiltContent` 元件：

```razor
@page "/BuiltContent"

<h1>Build a component</h1>

@CustomRender

<button type="button" @onclick="RenderComponent">
    Create three Pet Details components
</button>

@code {
    private RenderFragment CustomRender { get; set; }
    
    private RenderFragment CreateComponent() => builder =>
    {
        for (var i = 0; i < 3; i++) 
        {
            builder.OpenComponent(0, typeof(PetDetails));
            builder.AddAttribute(1, "PetDetailsQuote", "Someone's best friend!");
            builder.CloseComponent();
        }
    };    
    
    private void RenderComponent()
    {
        CustomRender = CreateComponent();
    }
}
```

> [!WARNING]
> `Microsoft.AspNetCore.Components.RenderTree` 中的類型允許處理轉譯作業的*結果*。 這些是 Blazor framework 執行的內部詳細資料。 這些類型應該被視為不*穩定*，未來的版本可能會變更。

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a>序號與程式程式碼號相關，而不是執行順序

Razor 元件檔案（*razor*）一律會進行編譯。 編譯是在解讀程式碼方面的潛在優勢，因為編譯步驟可以用來插入資訊，以在執行時間改善應用程式效能。

這些改良功能的重要範例包括*序號*。 序號會向運行時程表示輸出來自哪些不同和已排序的程式程式碼。 執行時間會使用這項資訊，以線性時間產生有效率的樹狀差異，這比一般樹狀結構的差異演算法通常還能快得多。

請考慮下列 Razor 元件（*razor*）檔案：

```razor
@if (someFlag)
{
    <text>First</text>
}

Second
```

上述程式碼會編譯成如下所示的內容：

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

第一次執行程式碼時，如果 `true``someFlag`，則產生器會接收：

| 順序 | 類型      | 資料   |
| :------: | --------- | :----: |
| 0        | Text node | First  |
| 1        | Text node | Second |

假設 `someFlag` 會變成 `false`，然後再次轉譯標記。 這次，產生器會接收：

| 順序 | 類型       | 資料   |
| :------: | ---------- | :----: |
| 1        | Text node  | Second |

當執行時間執行差異時，它會看到順序 `0` 的專案已移除，因此它會產生下列簡單的*編輯腳本*：

* 移除第一個文位元組點。

### <a name="the-problem-with-generating-sequence-numbers-programmatically"></a>以程式設計方式產生序號的問題

想像一下，您會改為撰寫下列轉譯樹產生器邏輯：

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

現在，第一個輸出是：

| 順序 | 類型      | 資料   |
| :------: | --------- | :----: |
| 0        | Text node | First  |
| 1        | Text node | Second |

此結果與先前的案例相同，因此不會有負面問題存在。 `someFlag` 是在第二個轉譯 `false`，而輸出則是：

| 順序 | 類型      | 資料   |
| :------: | --------- | ------ |
| 0        | Text node | Second |

這次，diff 演算法發現發生了*兩*項變更，而演算法會產生下列編輯腳本：

* 將第一個文位元組點的值變更為 `Second`。
* 移除第二個文位元組點。

產生序號已遺失有關 `if/else` 分支和迴圈在原始程式碼中出現位置的所有實用資訊。 這會導致差異**兩倍，但前提**是之前。

這是一個簡單的範例。 在具有複雜和深層嵌套結構的更真實案例中，尤其是使用迴圈時，效能成本通常較高。 Diff 演算法不會立即識別已插入或移除的迴圈區塊或分支，而是必須將深度遞迴到轉譯樹狀結構中。 這通常需要建立更長的編輯腳本，因為差異演算法會 misinformed 舊的和新結構如何彼此相關。

### <a name="guidance-and-conclusions"></a>指引和結論

* 如果序號是動態產生的，應用程式效能會受到影響。
* 架構無法在執行時間自動建立自己的序號，因為必要的資訊不存在，除非是在編譯時期加以捕捉。
* 請勿撰寫以手動方式執行之 `RenderTreeBuilder` 邏輯的長區塊。 偏好*razor*檔案，並允許編譯器處理序號。 如果您無法避免手動 `RenderTreeBuilder` 邏輯，請將長塊的程式碼分割成較小的片段，並以 `OpenRegion`/`CloseRegion` 呼叫來包裝。 每個區域都有自己的序號個別空間，因此您可以在每個區域內從零（或任何其他任一數字）重新開機。
* 如果序號已硬式編碼，則 diff 演算法只會要求序號增加值。 起始值和間距無關。 一個合法的選項是使用程式程式碼號做為序號，或從零開始，並以一個或數百個（或任何慣用的間隔）來增加。 
* Blazor 會使用序號，而其他樹狀結構比較的 UI 架構則不會使用它們。 使用序號時，比較速度會更快，而且 Blazor 有一個編譯步驟，可針對撰寫*razor*檔案的開發人員自動處理序號。
