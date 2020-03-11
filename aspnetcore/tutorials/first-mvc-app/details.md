---
title: 檢查 ASP.NET Core 應用程式的 Details 和 Delete 方法
author: rick-anderson
description: 了解基本 ASP.NET Core MVC 應用程式中的 Details 控制器方法和檢視。
ms.author: riande
ms.date: 12/13/2018
uid: tutorials/first-mvc-app/details
ms.openlocfilehash: 04eb2efa4e67d84e575580a6248d0b5b567064af
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78662908"
---
# <a name="examine-the-details-and-delete-methods-of-an-aspnet-core-app"></a>檢查 ASP.NET Core 應用程式的 Details 和 Delete 方法

由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供

開啟 Movie 控制器，並檢查 `Details` 方法：

[!code-csharp[](start-mvc/sample/MvcMovie22/Controllers/MoviesController.cs?name=snippet_details)]

建立這個動作方法的 MVC scaffolding 引擎，會新增一項註解以顯示叫用方法的 HTTP 要求。 在此情況下，它是含有 `Movies` 控制器、`Details` 方法和 `id` 值這三個 URL 區段的 GET 要求。 回想一下，這些區段會在 *Startup.cs* 中定義。

[!code-csharp[](start-mvc/sample/MvcMovie3/Startup.cs?highlight=5&name=snippet_1)]

EF 可讓您輕鬆使用 `FirstOrDefaultAsync` 方法來搜尋資料。 此方法內建一項重要的安全性功能：程式碼會先驗證搜尋方法是否已找到電影，之後才嘗試對其執行任何動作。 比方說，駭客可能會將透過 `http://localhost:{PORT}/Movies/Details/1` 連結建立的 URL 變更為類似 `http://localhost:{PORT}/Movies/Details/12345` (或不代表實際電影的其他值)，導致站台發生錯誤。 如果並未檢查是否電影是否為 null，應用程式就會擲回例外狀況。

檢查 `Delete` 和 `DeleteConfirmed` 方法。

[!code-csharp[](start-mvc/sample/MvcMovie22/Controllers/MoviesController.cs?name=snippet_delete)]

請注意，`HTTP GET Delete` 方法並不會刪除指定的電影，而會傳回電影的檢視，您可在該檢視中提交 (HttpPost) 刪除作業。 如果您執行刪除作業以回應 GET 要求 (或是執行相關編輯作業、建立作業或任何會變更資料的其他作業)，則會造成安全性漏洞。

我們將可刪除資料的 `[HttpPost]` 方法命名為 `DeleteConfirmed`，以提供 HTTP POST 方法的唯一簽章或名稱。 這兩個方法簽章如下所示：

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete2)]

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete3)]

通用語言執行平台 (CLR) 需要多載方法，以提供唯一的參數簽章 (方法名稱相同但參數清單不同)。 不過，此處您需要兩個 `Delete` 方法，一個用於 GET，另一個用於 POST，且兩者都具有相同的參數簽章 (它們都需要接受單一整數作為參數)。

針對這個問題，有兩種解決方法：一個是為方法提供不同的名稱。 這是 scaffolding 機制在上述範例採取的方法。 不過，這麼做會導致一個小問題：ASP.NET 會依名稱將 URL 區段與動作方法對應，一旦您重新命名方法，路由通常就會找不到這個方法。 解決辦法正如您看到的這個範例：將 `ActionName("Delete")` 屬性新增至 `DeleteConfirmed` 方法。 該屬性會執行路由系統的對應，以讓含有 POST 要求之 /Delete/ 的 URL 找到 `DeleteConfirmed` 方法。

如果若干方法具有相同名稱和簽章，另一個常見解決辦法是以人為方式變更 POST 方法的簽章，以包含額外 (未使用) 的參數。 這也是我們在上一篇文章中新增 `notUsed` 參數時所執行的操作。 針對這裡的 `[HttpPost] Delete` 方法，您可以執行相同的動作：

```csharp
// POST: Movies/Delete/6
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> Delete(int id, bool notUsed)
```

### <a name="publish-to-azure"></a>發佈至 Azure

如需部署至 Azure 的資訊，請參閱[教學課程：在 Azure App Service 中建置 .NET Core 和 SQL Database Web 應用程式](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)。

> [!div class="step-by-step"]
> [[上一步]](validation.md)
