---
title: 在 ASP.NET Core MVC 檢視為基礎的授權
author: rick-anderson
description: 本文件將示範如何插入，並利用授權服務的 ASP.NET Core Razor 檢視內。
manager: wpickett
ms.author: riande
ms.date: 10/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/views
ms.openlocfilehash: dad59a297efb4648755436fbd07742f95af97fb2
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/22/2018
---
# <a name="view-based-authorization-in-aspnet-core-mvc"></a>在 ASP.NET Core MVC 檢視為基礎的授權

開發人員通常會想要顯示、 隱藏或修改目前的使用者識別為基礎的 UI。 您可以存取授權服務會在透過 MVC 檢視內[相依性插入](xref:fundamentals/dependency-injection#fundamentals-dependency-injection)。 若要插入 Razor 檢視授權服務，使用`@inject`指示詞：

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

如果您想要授權服務的每個檢視中，放入`@inject`到指示詞*_ViewImports.cshtml*檔案*檢視*目錄。 如需詳細資訊，請參閱[在檢視中插入相依性](xref:mvc/views/dependency-injection)。

使用插入的授權服務叫用`AuthorizeAsync`方式完全相同時，您會檢查[資源為基礎的授權](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

---

在某些情況下，資源會是您的檢視模型。 叫用`AuthorizeAsync`方式完全相同時，您會檢查[資源為基礎的授權](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit))
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

---

在上述程式碼中，模型會傳遞做為原則評估應該採取的資源納入考量。

> [!WARNING]
> 不要只依賴切換可見性的應用程式的 UI 元素的唯一授權檢查。 隱藏 UI 項目可能無法完全防止存取至其關聯之的控制器的動作。 例如，請考慮前面的程式碼片段中的按鈕。 使用者可以叫用`Edit`動作方法如果他或她知道之相對資源 URL 是*/Document/Edit/1*。 基於這個理由，`Edit`動作方法執行它自己的授權檢查。
