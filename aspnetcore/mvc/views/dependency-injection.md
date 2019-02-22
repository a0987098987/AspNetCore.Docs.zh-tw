---
title: ASP.NET Core 檢視中的相依性插入
author: ardalis
description: 了解 ASP.NET Core 如何支援 MVC 檢視中的相依性插入。
ms.author: riande
ms.date: 10/14/2016
uid: mvc/views/dependency-injection
ms.openlocfilehash: dfadafe9ebb5799b45ef68653f20c5fc1a2506b5
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410556"
---
# <a name="dependency-injection-into-views-in-aspnet-core"></a>ASP.NET Core 檢視中的相依性插入

作者：[Steve Smith](https://ardalis.com/)

ASP.NET Core 支援檢視中的[相依性插入](xref:fundamentals/dependency-injection)。 這可用於檢視特定服務，例如僅適用於填入檢視項目所需的當地語系化或資料。 您應該嘗試維護控制器與檢視之間的 [Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) (關注點分離)。 您檢視所顯示的大部分資料應該都是從控制器傳入。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="a-simple-example"></a>簡單範例

您可以使用 `@inject` 指示詞，將服務插入至檢視。 您可以將 `@inject` 視為將屬性新增至檢視，並使用 DI 來填入屬性。

`@inject` 的語法：`@inject <type> <name>`

運作中 `@inject` 範例：

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/ToDo/Index.cshtml?highlight=4,5,15,16,17)]

此檢視會顯示 `ToDoItem` 執行個體清單，以及顯示整體統計資料的摘要。 摘要是從插入的 `StatisticsService` 中填入。 在 *Startup.cs* 的 `ConfigureServices` 中，註冊此服務以進行相依性插入：

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Startup.cs?highlight=6,7&range=15-22)]

`StatisticsService` 會對一組 `ToDoItem` 執行個體執行一些計算，以透過存放庫進行存取：

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/StatisticsService.cs?highlight=15,20,25)]

範例存放庫會使用記憶體內部集合。 上面顯示的實作 (作用於記憶體中的所有資料) 不建議用於大型可從遠端存取的資料集。

此範例所顯示的資料來自繫結至檢視的模型，以及插入至檢視的服務：

![To Do 檢視，列出項目總數、已完成項目、平均優先順序 ，以及具有其優先順序層級和指出完成之布林值的工作清單。](dependency-injection/_static/screenshot.png)

## <a name="populating-lookup-data"></a>填入查閱資料

檢視插入可以用來填入 UI 項目中的選項，例如下拉式清單。 請考慮使用者設定檔表單，其中包含指定性別、狀態和其他喜好設定的選項。 使用標準 MVC 方法轉譯這類表單時，需要控制器要求所有這些選項集的資料存取服務，然後將已繫結的每個選項集填入模型或 `ViewBag` 中。

另一種方法會將服務直接插入至檢視，以取得選項。 這會將控制器所需的程式碼數量降到最低，並將此檢視項目建構邏輯移至檢視本身。 要顯示設定檔編輯表單的控制器動作，只需要將設定檔執行個體傳遞給表單：

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Controllers/ProfileController.cs?highlight=9,19)]

用來更新這些喜好設定的 HTML 表單包括三個屬性的下拉式清單：

![[更新設定檔] 檢視，內含允許輸入名稱、性別、狀態和偏愛顏色的表單。](dependency-injection/_static/updateprofile.png)

這些清單會填入已插入至檢視的服務：

[!code-cshtml[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Profile/Index.cshtml?highlight=4,16,17,21,22,26,27)]

`ProfileOptionsService` 是設計成只提供此表單所需資料的 UI 層級服務：

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/ProfileOptionsService.cs?highlight=7,13,24)]

> [!IMPORTANT]
> 請不要忘記在 `Startup.ConfigureServices` 中註冊您透過相依性插入所要求的類型。 未註冊的類型會在執行階段擲回例外狀況，因為服務提供者是透過 [GetRequiredService](/dotnet/api/microsoft.extensions.dependencyinjection.serviceproviderserviceextensions.getrequiredservice) 內部查詢。

## <a name="overriding-services"></a>覆寫服務

除了插入新的服務之外，這項技術也可以用來覆寫頁面上先前插入的服務。 下圖顯示第一個範例中所使用頁面上的所有可用欄位：

![已鍵入 @ symbol listing Html、Component、StatsService 和 URL 欄位上的 Intellisense 操作功能表](dependency-injection/_static/razor-fields.png)

如您所見，預設欄位包括 `Html`、`Component` 和 `Url` (以及我們插入的 `StatsService`)。 例如，如果您要將預設 HTML 協助程式取代為您自己的協助程式，則使用 `@inject` 就可以輕鬆地達成：

[!code-cshtml[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Helper/Index.cshtml?highlight=3,11)]

如果您想要擴充現有服務，則只需要在繼承自現有實作或自行包裝現有實作時使用這項技術。

## <a name="see-also"></a>請參閱

* Simon Timms 部落格：[將查閱資料放入檢視中](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)
