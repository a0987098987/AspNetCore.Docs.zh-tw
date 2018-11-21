---
title: 使用 Web API 慣例
author: pranavkm
description: 深入了解 ASP.NET Core 中的 Web API 慣例。
monikerRange: '>= aspnetcore-2.2'
ms.author: pranavkm
ms.custom: mvc
ms.date: 11/13/2018
uid: web-api/advanced/conventions
ms.openlocfilehash: 023b8d09511aa42966e2a7d1c85e407bb6e79b0f
ms.sourcegitcommit: f202864efca81a72ea7120c0692940c40d9d0630
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/14/2018
ms.locfileid: "51635383"
---
# <a name="use-web-api-conventions"></a>使用 Web API 慣例

ASP.NET Core 2.2 引進擷取常見 [API 文件](xref:tutorials/web-api-help-pages-using-swagger)的方式，並將其套用至多個動作、控制器，或組件內的所有控制器。 Web API 慣例為使用 [[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute) 裝飾個別動作的替代品。 它讓您可以定義最常見的「傳統」傳回型別及狀態碼，這兩者都是您選取套用至動作的慣例方法這個動作所傳回的。

根據預設，ASP.NET Core MVC 2.2 會隨附一組預設慣例，`Microsoft.AspNetCore.Mvc.DefaultApiConventions`。 慣例以 ASP.NET Core 建立的控制器為基礎。 若動作遵循 Scaffolding 產生的模式，就應該可以成功使用預設慣例。

在執行階段，<xref:Microsoft.AspNetCore.Mvc.ApiExplorer> 了解慣例。 `ApiExplorer` 是與 Open API 文件產生器通訊之 MVC 的抽象概念。 來自套用慣例的屬性會與動作建立關聯，且包含在動作的 Swagger 文件中。 API 分析器也了解慣例。 若您的動作為非傳統的 (例如，其傳回未由套用慣例所記載的狀態碼)，就會產生建議您將其記載的警告。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/conventions/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="apply-web-api-conventions"></a>套用 Web API 慣例

可透過三種方式套用慣例。 慣例不會進行撰寫，各個動作可能僅會與一個慣例建立關聯。 相較於較不特定的慣例，更特定的慣例 (詳述如下) 擁有較高的優先順序。 當兩個以上相同屬性的慣例套用至動作時，慣例就具有不確定性。 下列選項的作用為將慣例套用至動作，其順序為從最特定到最不特定：

1. `Microsoft.AspNetCore.Mvc.ApiConventionMethodAttribute` &mdash; 會套用至個別動作，並會指定慣例型別及其套用的慣例方法。 在下列範例中，慣例方法 `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` 會套用至 `Update` 動作：

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=apiconventionmethod&highlight=2-3)]

1. 套用至控制器的 `Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` &mdash; 會將慣例型別套用至控制器上的所有動作。 慣例方法由提示所裝飾，且該提示會決定要套用的動作 (詳細資料於撰寫慣例中)。 例如: 

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=apiconventiontypeattribute)]

1. 套用至組件的 `Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` &mdash; 會將慣例型別套用至目前組件中的所有控制器。 例如: 

    [!code-csharp[](conventions/sample/Startup.cs?name=apiconventiontypeattribute)]

## <a name="create-web-api-conventions"></a>建立 Web API 慣例

慣例是具有方法的靜態型別。 使用 `[ProducesResponseType]` 或 `[ProducesDefaultResponseType]` 屬性標註這些方法。

```csharp
public static class MyAppConventions
{
    [ProducesResponseType(200)]
    [ProducesResponseType(404)]
    public static void Find(int id)
    {
    }
}
```

若慣例沒有其他更特定的中繼資料屬性，則將此慣例套用至組件會導致慣例方法套用至名稱為 `Find` 且僅有名為 `id` 之參數的任何動作。

除了 `[ProducesResponseType]` 和 `[ProducesDefaultResponseType]` 之外，可以將 `[ApiConventionNameMatch]` 和 `[ApiConventionTypeMatch]` 套用至會決定其套用方法的慣例方法。 例如: 

```csharp
[ProducesResponseType(200)]
[ProducesResponseType(404)]
[ApiConventionNameMatch(ApiConventionNameMatchBehavior.Prefix)]
public static void Find(
    [ApiConventionNameMatch(ApiConventionNameMatchBehavior.Suffix)]
    int id)
{ }
```

* 套用至方法的 `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Prefix` 選項，會表示只要其前置詞為 "Find"，慣例就可與任何動作相符。 這包含例如 `Find`、`FindPet` 或是 `FindById` 等方法。
* 套用至參數的 `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Suffix`，會表示慣例可與僅有一個以 Suffix 識別碼作為結尾之參數的方法相符。 這包含例如 `id` 或 `petId` 等參數。 `ApiConventionTypeMatch` 可以相似地套用至型別，以限制參數的型別。 `params[]` 引數可用以表示不需要明確相符的剩餘參數。
