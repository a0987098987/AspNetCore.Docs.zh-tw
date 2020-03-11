---
title: ASP.NET Core MVC 中以視圖為基礎的授權
author: rick-anderson
description: 本檔示範如何在 ASP.NET Core Razor view 內插入和使用授權服務。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 11/08/2019
uid: security/authorization/views
ms.openlocfilehash: fc03da9eb98d36ffdda932ee5b16f327c2be9f83
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78663594"
---
# <a name="view-based-authorization-in-aspnet-core-mvc"></a>ASP.NET Core MVC 中以視圖為基礎的授權

開發人員通常會想要根據目前的使用者識別來顯示、隱藏或修改 UI。 您可以透過相依性[插入](xref:fundamentals/dependency-injection)來存取 MVC views 內的授權服務。 若要將授權服務插入 Razor 視圖中，請使用 `@inject` 指示詞：

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

如果您想要在每個視圖中都要有授權服務，請將 `@inject` 指示詞放入*Views*目錄的 *_ViewImports. cshtml*檔案中。 如需詳細資訊，請參閱[在檢視中插入相依性](xref:mvc/views/dependency-injection)。

使用插入的授權服務，以您在以[資源為基礎的授權](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative)期間檢查的相同方式來叫用 `AuthorizeAsync`：

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

在某些情況下，資源會是您的視圖模型。 以您在以[資源為基礎的授權](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative)期間所檢查的相同方式來叫用 `AuthorizeAsync`：

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

在上述程式碼中，模型會以資源的形式傳遞，原則評估應該會納入考慮。

> [!WARNING]
> 請勿依賴切換應用程式 UI 專案的可見度，做為唯一的授權檢查。 隱藏 UI 元素可能無法完全防止存取其相關聯的控制器動作。 例如，請考慮上述程式碼片段中的按鈕。 如果使用者知道 */Document/Edit/1*的相對資源 URL，就可以叫用 `Edit` 動作方法。 基於這個理由，`Edit` 動作方法應該執行自己的授權檢查。
