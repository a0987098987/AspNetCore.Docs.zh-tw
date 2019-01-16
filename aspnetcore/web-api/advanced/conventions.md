---
title: 使用 Web API 慣例
author: pranavkm
description: 深入了解 ASP.NET Core 中的 Web API 慣例。
monikerRange: '>= aspnetcore-2.2'
ms.author: scaddie
ms.custom: mvc
ms.date: 12/13/2018
uid: web-api/advanced/conventions
ms.openlocfilehash: 892fb8898c78a1645c766544715a8256462207c6
ms.sourcegitcommit: ec71fd5a988f927ae301813aae5ff764feb3bb6a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/12/2019
ms.locfileid: "54249382"
---
# <a name="use-web-api-conventions"></a>使用 Web API 慣例

作者：[Pranav Krishnamoorthy](https://github.com/pranavkm) 和 [Scott Addie](https://github.com/scottaddie)

ASP.NET Core 2.2 和更新版本包含擷取常見 [API 文件](xref:tutorials/web-api-help-pages-using-swagger)的方式，並將其套用至多個動作、控制器，或組件內的所有控制器。 Web API 慣例為使用 [[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute) 裝飾個別動作的替代品。

慣例可讓您：

* 定義從特定動作類型傳回的最常見傳回型別和狀態碼。
* 識別偏離已定義標準的動作。

ASP.NET Core MVC 2.2 和更新版本在 `Microsoft.AspNetCore.Mvc.DefaultApiConventions` 中包含一組預設慣例。 這些慣例是以 ASP.NET Core **API** 專案範本中所提供的控制器 (*ValuesController.cs*) 為基礎。 若動作遵循範本中的模式，就應該可以成功使用預設慣例。 如果預設慣例不符合您的需求，請參閱[建立 Web API 慣例](#create-web-api-conventions)。

在執行階段，<xref:Microsoft.AspNetCore.Mvc.ApiExplorer> 了解慣例。 `ApiExplorer` 是 MVC 與 [OpenAPI](https://www.openapis.org/) (也稱為 Swagger) 文件產生器通訊的抽象層。 來自套用慣例的屬性會與動作建立關聯，且包含在動作的 OpenAPI 文件中。 [API 分析器](xref:web-api/advanced/analyzers)也了解慣例。 若您的動作為非傳統的 (例如，其傳回未由套用慣例所記載的狀態碼)，就會出現警告，建議您記載狀態碼。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/conventions/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="apply-web-api-conventions"></a>套用 Web API 慣例

慣例不會進行撰寫；各個動作可能僅會與一個慣例建立關聯。 相較於較不特定的慣例，更特定的慣例擁有較高的優先順序。 當兩個以上相同屬性的慣例套用至動作時，慣例就具有不確定性。 下列選項的作用為將慣例套用至動作，其順序為從最特定到最不特定：

1. `Microsoft.AspNetCore.Mvc.ApiConventionMethodAttribute` &mdash; 會套用至個別動作，並會指定慣例型別及其套用的慣例方法。

    在下列範例中，預設慣例類型的 `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` 慣例方法會套用至 `Update` 動作：

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=snippet_ApiConventionMethod&highlight=3)]

    `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` 慣例方法會將下列屬性套用至動作：

    ```csharp
    [ProducesDefaultResponseType]
    [ProducesResponseType(204)]
    [ProducesResponseType(404)]
    [ProducesResponseType(400)]
    ```

如需有關 `[ProducesDefaultResponseType]`,的詳細資訊，請參閱[預設回應](https://swagger.io/docs/specification/describing-responses/#default) \(英文\)。

1. 套用到控制器的 `Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` &mdash; 會將指定的慣例類型套用至控制器上的所有動作。 慣例方法由提示所裝飾，且該提示會決定要套用慣例方法的動作。 如需提示的詳細資訊，請參閱[建立 Web API 慣例](#create-web-api-conventions)。

    在下列範例中，會將一組預設的慣例套用至 *ContactsConventionController* 中的所有動作：

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=snippet_ApiConventionTypeAttribute&highlight=2)]

1. 套用至組件的 `Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` &mdash; 會將指定的慣例類型套用至目前組件中的所有控制器。 建議您將組件層級屬性套用至 `Startup` 類別。

    在下列範例中，會將一組預設的慣例套用至組件中的所有控制器：

    [!code-csharp[](conventions/sample/Startup.cs?name=snippet_ApiConventionTypeAttribute&highlight=1)]

## <a name="create-web-api-conventions"></a>建立 Web API 慣例

如果預設 API 慣例不符合您的需求，請建立您自己的慣例。 慣例為：

* 具有方法的靜態類型。
* 能夠定義動作的[回應類型](#response-types)和[命名需求](#naming-requirements)。

### <a name="response-types"></a>回應類型

使用 `[ProducesResponseType]` 或 `[ProducesDefaultResponseType]` 屬性標註這些方法。 例如：

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

如果沒有更特定的中繼資料屬性，將此慣例套用至組件可確保：

* 將慣例方法套用至任何名為 `Find` 的動作。
* `Find` 動作中有名為 `id` 的參數。

### <a name="naming-requirements"></a>命名需求

您可以將 `[ApiConventionNameMatch]` 和 `[ApiConventionTypeMatch]` 屬性套用至會決定其套用動作的慣例方法。 例如：

```csharp
[ProducesResponseType(200)]
[ProducesResponseType(404)]
[ApiConventionNameMatch(ApiConventionNameMatchBehavior.Prefix)]
public static void Find(
    [ApiConventionNameMatch(ApiConventionNameMatchBehavior.Suffix)]
    int id)
{ }
```

在上述範例中：

* 套用至方法的 `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Prefix` 選項表示慣例會比對任何前置詞為 "Find" 的動作。 相符動作範例包括 `Find`、`FindPet` 和 `FindById`。
* 套用至參數的 `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Suffix` 表示慣例會比對只有一個以後置詞識別碼結尾之參數的方法。 範例包括 `id` 或 `petId` 等參數。 同樣地，`ApiConventionTypeMatch` 可套用至類型來限制參數類型。 `params[]` 引數表示其餘參數不需要明確相符。

## <a name="additional-resources"></a>其他資源

* <xref:web-api/advanced/analyzers>
* <xref:tutorials/web-api-help-pages-using-swagger>
