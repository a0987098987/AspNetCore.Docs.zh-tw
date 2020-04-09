---
title: ASP.NET核心Blazor樣本化元件
author: guardrex
description: 瞭解範本化元件如何接受一個或多個 UI 範本作為參數,然後用作元件呈現邏輯的一部分。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/18/2020
no-loc:
- Blazor
- SignalR
uid: blazor/templated-components
ms.openlocfilehash: b57e3fe186402723607e90b1628062f602c77632
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "79989491"
---
# <a name="aspnet-core-opno-locblazor-templated-components"></a>ASP.NET核心Blazor樣本化元件

由[盧克·萊瑟姆](https://github.com/guardrex)和[丹尼爾·羅斯](https://github.com/danroth27)

範本化元件是接受一個或多個 UI 範本作為參數的元件,然後可用作元件呈現邏輯的一部分。 樣本化元件允許您編寫比常規元件更可重用的更高級別元件。 幾個例子包括:

* 允許使用者為表的標頭、行和頁腳指定範本的表元件。
* 允許使用者指定用於呈現清單中項的範本的清單元件。

## <a name="template-parameters"></a>範本參數

樣本化元件通過指定類型`RenderFragment`或`RenderFragment<T>`的一個或多個元件參數來定義。 渲染片段表示要呈現的 UI 段。 `RenderFragment<T>`採用可在調用渲染片段時指定的類型參數。

`TableTemplate`元件:

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/TableTemplate.razor)]

使用樣本化元件時,可以使用與參數名稱符合的子元素(`TableHeader`並在`RowTemplate`以下範例中)指定樣本參數:

```razor
<TableTemplate Items="pets">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate>
        <td>@context.PetId</td>
        <td>@context.Name</td>
    </RowTemplate>
</TableTemplate>
```

> [!NOTE]
> 泛型類型約束將在將來的版本中受支援。 有關詳細資訊,請參閱[允許泛型類型約束(點網/阿斯平核心#8433)](https://github.com/dotnet/aspnetcore/issues/8433)。

## <a name="template-context-parameters"></a>樣本內容參數

`RenderFragment<T>`作為元素傳遞的類型類型的元件參數具有一個隱式參數`context`(例如,來自前面的代碼示例),`@context.PetId`但可以使用子元素上的`Context`屬性 更改參數名稱。 在下面的範例中,`RowTemplate`元素的`Context`屬性 指定`pet`參數:

```razor
<TableTemplate Items="pets">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate Context="pet">
        <td>@pet.PetId</td>
        <td>@pet.Name</td>
    </RowTemplate>
</TableTemplate>
```

或者,您可以在元件元素上`Context`指定屬性。 指定`Context`屬性適用於所有指定的範本參數。 當您要為隱式子內容指定內容參數名稱(沒有任何換行子元素)時,這非常有用。 在下面的範例中,該`Context`屬性顯示在元素`TableTemplate`上,並應用於所有樣本參數:

```razor
<TableTemplate Items="pets" Context="pet">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate>
        <td>@pet.PetId</td>
        <td>@pet.Name</td>
    </RowTemplate>
</TableTemplate>
```

## <a name="generic-typed-components"></a>通用類型元件

樣本元件通常採用通用類型。 例如,泛型`ListViewTemplate`元件可用於呈現`IEnumerable<T>`值。 要定義泛型元件,請使用[`@typeparam`](xref:mvc/views/razor#typeparam)指令指定類型參數:

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ListViewTemplate.razor)]

使用泛型型態元件時,如果可能,將推斷類型參數:

```razor
<ListViewTemplate Items="pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

否則,必須使用與類型參數名稱匹配的屬性顯式指定類型參數。 在下面的範例中,`TItem="Pet"`指定類型:

```razor
<ListViewTemplate Items="pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```
