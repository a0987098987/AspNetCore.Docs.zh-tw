---
title: "檢視的相依性插入"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/dependency-injection
ms.openlocfilehash: a1258dbe2e659f6c5149d15b37451810ec7d6601
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
# <a name="dependency-injection-into-views"></a>檢視的相依性插入

由[Steve Smith](https://ardalis.com/)

ASP.NET Core 支援[相依性插入](xref:fundamentals/dependency-injection)到檢視表。 這可用於檢視特定的服務，例如當地語系化或只適用於填入檢視項目所需的資料。 您應該嘗試維護[的重要性分離](http://deviq.com/separation-of-concerns/)您控制器和檢視之間。 大部分的檢視所顯示的資料應該來自控制器中傳遞。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="a-simple-example"></a>簡單的範例

您可以將服務插入檢視使用`@inject`指示詞。 您可以將`@inject`做為將屬性加入至您的檢視，並填入使用 DI 的屬性。

語法`@inject`:`@inject <type> <name>`

舉例來說，`@inject`動作：

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/ToDo/Index.cshtml?highlight=4,5,15,16,17)]

此檢視會顯示一份`ToDoItem`執行個體，以及顯示整體統計資料摘要。 摘要已填入從插入`StatisticsService`。 此服務會註冊為中的相依性插入`ConfigureServices`中*Startup.cs*:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Startup.cs?highlight=6,7&range=15-22)]

`StatisticsService`執行一組上的一些計算`ToDoItem`執行個體，它會存取透過儲存機制：

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/StatisticsService.cs?highlight=15,20,26)]

範例儲存機制會使用記憶體中的集合。 如上所示的實作 （它對所有記憶體中的資料） 時，不都建議針對大型、 可從遠端存取的資料集。

此範例會顯示資料繫結至檢視的模型和檢視所插入的服務：

![若要執行檢視列出的項目總數，完成項目、 平均的優先順序和優先順序層級與布林值，指出完成的工作清單。](dependency-injection/_static/screenshot.png)

## <a name="populating-lookup-data"></a>填入查閱資料

檢視資料隱碼可用來填入 UI 項目，例如下拉式清單中的選項。 請考慮使用者設定檔表格，其中包含指定性別、 狀態和其他喜好設定的選項。 呈現這類使用標準的 MVC 方法的表單，則會需要控制站要求的選項，這些集合的每個資料存取服務，然後再來擴展模型或`ViewBag`與每一組繫結的選項。

另一個方法會將服務直接插入檢視來取得選項。 這會將控制站，將這個檢視項目建構邏輯移至檢視本身所需的程式碼數量降到最低。 控制器的動作，以顯示 編輯表單只需要將表單的設定檔執行個體的設定檔：

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Controllers/ProfileController.cs?highlight=9,19)]

用來更新這些喜好設定的 HTML 表單包含了三個屬性的下拉式清單中：

![允許的名稱、 性別、 狀態和偏愛顏色的項目表單更新設定檔 檢視。](dependency-injection/_static/updateprofile.png)

這些清單會填入插入檢視服務：

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Profile/Index.cshtml?highlight=4,16,17,21,22,26,27)]

`ProfileOptionsService`是設計來提供此表單所需資料的 UI 層級服務：

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/ProfileOptionsService.cs?highlight=7,13,24)]

>[!TIP]
> 別忘了註冊將要求中的相依性插入透過型別`ConfigureServices`方法中的*Startup.cs*。

## <a name="overriding-services"></a>覆寫服務

除了插入新的服務，這項技術也可以用來覆寫先前插入的服務 頁面上。 下圖會顯示所有可用的欄位在第一個範例中使用 頁面上：

![Intellisense 的內容功能表上的型別 @ 符號列出 Html、 元件、 StatsService 和 Url 的欄位](dependency-injection/_static/razor-fields.png)

如您所見，預設欄位包含`Html`， `Component`，和`Url`(以及`StatsService`我們插入)。 如果您要取代您自己的預設 HTML Helper 執行個體，無法輕鬆地，您使用`@inject`:

[!code-html[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Helper/Index.cshtml?highlight=3,11)]

如果您想要擴充現有的服務，您只可以在繼承自或換行以您自己的現有實作時，使用這項技術。

## <a name="see-also"></a>請參閱

* Simon Timms 部落格：[查閱資料放入您的檢視](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)
