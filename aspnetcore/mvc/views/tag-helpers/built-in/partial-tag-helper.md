---
title: ASP.NET Core 的部分標記協助程式
author: scottaddie
description: 了解 ASP.NET Core 部分標記協助程式和其每個屬性在呈現部分檢視時所扮演的角色。
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 07/06/2018
uid: mvc/views/tag-helpers/builtin-th/partial-tag-helper
ms.openlocfilehash: a289a946a6d3eb491a08103dcefdd688eab52072
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938335"
---
# <a name="partial-tag-helper-in-aspnet-core"></a>ASP.NET Core 的部分標記協助程式

作者：[Scott Addie](https://github.com/scottaddie)

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="overview"></a>總覽

部分標記協助程式用於在 Razor 頁面和 MVC 應用程式中呈現[部分檢視](xref:mvc/views/partial)。 請考慮它：

* 需要 ASP.NET Core 2.1 或更新版本。
* 是 [HTML 協助程式語法](xref:mvc/views/partial#reference-a-partial-view)的替代方法。
* 以非同步方式呈現部分檢視。

呈現部分檢視的 HTML 協助程式選項包括：

* [@await Html.PartialAsync](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partialasync)
* [@await Html.RenderPartialAsync](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartialasync)
* [@Html.Partial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partial)
* [@Html.RenderPartial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartial)

這整份文件的範例皆使用 *Product* 模型：

[!code-csharp[](samples/TagHelpersBuiltIn/Models/Product.cs)]

部分標記協助程式屬性的清查如下。

## <a name="name"></a>name

`name` 屬性 (Attribute) 是必要項。 它會指出要呈現之部分檢視的名稱或路徑。 當提供部分檢視名稱時，就會起始[檢視探索](xref:mvc/views/overview#view-discovery)程序。 提供明確的路徑時，則會略過該程序。

下列標記會使用明確的路徑，指出將從 *Shared* 資料夾載入 *_ProductPartial.cshtml*。 使用 [for](#for) 屬性，模型就會傳遞到部分檢視以進行繫結。

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Name)]

## <a name="for"></a>for

`for` 屬性會針對目前的模型指派一個要評估的 [ModelExpression](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.modelexpression)。 `ModelExpression` 可推斷 `@Model.` 語法。 例如，可以使用 `for="Product"`，而不是 `for="@Model.Product"`。 使用 `@` 符號來定義內嵌運算式會覆寫這個預設的推斷行為。 `for` 屬性不能與 [model](#model) 屬性一起使用。

下列標記將載入 *_ProductPartial.cshtml*：

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_For)]

部分檢視會繫結至相關聯的頁面模型 `Product` 屬性：

[!code-csharp[](samples/TagHelpersBuiltIn/Pages/Product.cshtml.cs?highlight=8)]

## <a name="model"></a>model

`model`　屬性指派要傳遞至部分檢視的模型執行個體。 `model` 屬性不能與 [for](#for) 屬性一起使用。

在下列標記中，新的 `Product` 物件將具現化，並傳遞給 `model` 屬性以進行繫結：

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Model)]

## <a name="view-data"></a>view-data

`view-data`　屬性指派要傳遞至部分檢視的 [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary)。 下列標記會讓整個 ViewData 集合可存取部分檢視：

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_ViewData&highlight=5-)]

在上述程式碼中，`IsNumberReadOnly` 索引鍵值會設定為 `true`，並新增至 ViewData 集合。 因此，`ViewData["IsNumberReadOnly"]` 可以在下列部分檢視內進行存取：

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Shared/_ProductViewDataPartial.cshtml?highlight=5)]

在此範例中，`ViewData["IsNumberReadOnly"]` 的值決定 *Number* 欄位是否會顯示為唯讀。

## <a name="migrate-from-an-html-helper"></a>從 HTML 協助程式移轉

請考慮下列非同步 HTML 協助程式範例。 逐一查看和顯示產品集合。 依據 `PartialAsync` 方法的第一個參數載入 *_ProductPartial.cshtml*部分檢視。 `Product` 模型的執行個體會傳遞到部分檢視以進行繫結。

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Products.cshtml?name=snippet_HtmlHelper&highlight=3)]

下列 Partial 標籤協助程式可完成與 `PartialAsync` HTML 協助程式相同的非同步轉譯行為。 `model` 屬性會獲指派 `Product` 模型執行個體，以繫結至部分檢視。

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Products.cshtml?name=snippet_TagHelper&highlight=3)]

## <a name="additional-resources"></a>其他資源

* <xref:mvc/views/partial>
* <xref:mvc/views/overview#weakly-typed-data-viewdata-viewdata-attribute-and-viewbag>
