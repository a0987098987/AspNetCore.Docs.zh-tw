---
title: ASP.NET Core 中的標籤協助程式元件
author: scottaddie
description: 了解何謂標籤協助程式元件，以及如何在 ASP.NET Core 中使用。
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.date: 06/12/2019
uid: mvc/views/tag-helpers/th-components
ms.openlocfilehash: 23e244649350b41e4112d10df63139864e5b4381
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/14/2019
ms.locfileid: "69022223"
---
# <a name="tag-helper-components-in-aspnet-core"></a>ASP.NET Core 中的標籤協助程式元件

作者：[Scott Addie](https://twitter.com/Scott_Addie) 和 [Fiyaz Bin Hasan](https://github.com/fiyazbinhasan)

標籤協助程式元件是一種標籤協助程式，允許您從伺服器端程式碼有條件地修改或新增 HTML 項目。 ASP.NET Core 2.0 或更新版本提供此功能。

ASP.NET Core 包含兩個內建標籤協助程式元件：`head` 和 `body`。 這兩個標籤協助程式元件位於 <xref:Microsoft.AspNetCore.Mvc.Razor.TagHelpers> 命名空間，可用於 MVC 和 Razor Pages。 標籤協助程式元件不需要在 *_ViewImports.cshtml* 中註冊應用程式。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/tag-helpers/th-components/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="use-cases"></a>使用案例

兩個常見的標籤協助程式元件的使用案例為：

1. [將 `<link>` 插入 `<head>`。](#inject-into-html-head-element)
1. [將 `<script>` 插入 `<body>`。](#inject-into-html-body-element)

下列各節描述這些案例。

### <a name="inject-into-html-head-element"></a>插入 HTML 標頭項目

在 HTML `<head>` 項目中，CSS 檔案一般與 HTML `<link>` 項目一起匯入。 下列程式碼會使用 `head` 標籤協助程式元件，將 `<link>` 項目插入 `<head>` 項目：

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressStyleTagHelperComponent.cs)]

在上述程式碼中：

* `AddressStyleTagHelperComponent` 會實作 <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperComponent>。 抽象：
  * 允許使用 <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperContext> 初始化類別。
  * 可讓您使用標籤協助程式元件新增或修改 HTML 項目。
* <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperComponent.Order*> 屬性會定義元件的轉譯順序。 當應用程式中有多種標籤協助元件用法時，則 `Order` 為必要。
* <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperComponent.ProcessAsync*> 會將執行內容的 <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperContext.TagName*> 屬性值與 `head` 進行比較。 如果比較評估為 True，則 `_style` 欄位的內容會插入 HTML `<head>` 項目。

### <a name="inject-into-html-body-element"></a>插入 HTML 主體項目

`body` 標籤協助程式元件可將 `<script>` 項目插入 `<body>` 項目。 下列程式碼示範這項技術：

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressScriptTagHelperComponent.cs)]

個別的 HTML 檔案用於儲存 `<script>` 項目。 該 HTML 檔案會使程式碼更簡潔且更容易維護。 上述程式碼會讀取 *TagHelpers/Templates/AddressToolTipScript.html* 的內容，並為其附加標籤協助程式輸出。 *AddressToolTipScript.html* 檔案包含下列標記：

[!code-html[](th-components/samples/RazorPagesSample/TagHelpers/Templates/AddressToolTipScript.html)]

上述程式碼會將 [ 啟動程序工具提示小工具](https://getbootstrap.com/docs/3.3/javascript/#tooltips)繫結至包含 `printable` 屬性的任何 `<address>` 項目。 當滑鼠指標停留在項目上時，會顯示效果。

## <a name="register-a-component"></a>註冊元件

標籤協助程式元件必須新增至應用程式的標籤協助程式元件集合中。 有三種方式可新增至集合：

* [透過服務容器註冊](#registration-via-services-container)
* [透過 Razor 檔案註冊](#registration-via-razor-file)
* [透過頁面模型或控制器註冊](#registration-via-page-model-or-controller)

### <a name="registration-via-services-container"></a>透過服務容器註冊

如果標籤協助程式元件類別並未以 <xref:Microsoft.AspNetCore.Mvc.Razor.TagHelpers.ITagHelperComponentManager> 管理，則必須使用[相依性插入 (DI)](xref:fundamentals/dependency-injection) 系統來註冊。 下列 `Startup.ConfigureServices` 程式碼會使用[暫時性存留期](xref:fundamentals/dependency-injection#lifetime-and-registration-options)來註冊 `AddressStyleTagHelperComponent` 與 `AddressScriptTagHelperComponent` 類別：

[!code-csharp[](th-components/samples/RazorPagesSample/Startup.cs?name=snippet_ConfigureServices&highlight=12-15)]

### <a name="registration-via-razor-file"></a>透過 Razor 檔案註冊

如果標籤協助程式元件並未註冊為 DI，則可以從 Razor Pages 頁面或 MVC 檢視中註冊。 這項技術用於控制插入的標記和 Razor 檔案中元件執行順序。

`ITagHelperComponentManager` 用於新增標籤協助程式元件，或從應用程式移除。 下列程式碼使用 `AddressTagHelperComponent` 示範這項技術：

[!code-cshtml[](th-components/samples/RazorPagesSample/Pages/Contact.cshtml?name=snippet_ITagHelperComponentManager)]

在上述程式碼中：

* `@inject` 指示詞會提供 `ITagHelperComponentManager` 的執行個體。 該執行個體會指派給名為 `manager` 的變數，用於 Razor 檔案中的下游存取。
* `AddressTagHelperComponent` 的執行個體會新增至應用程式標籤協助程式元件集合中。

`AddressTagHelperComponent` 已修改，以容納接受 `markup` 和 `order` 參數的建構函式：

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressTagHelperComponent.cs?name=snippet_Constructor)]

提供的 `markup` 參數會用於 `ProcessAsync`，如下所示：

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressTagHelperComponent.cs?name=snippet_ProcessAsync&highlight=10-11)]

### <a name="registration-via-page-model-or-controller"></a>透過頁面模型或控制器註冊

如果標籤協助程式元件並未註冊為 DI，則可以從 Razor Pages 模型或 MVC 控制器進行註冊。 這項技術可用於將 C# 邏輯從 Razor 檔案分開。

建構函式插入可用於存取 `ITagHelperComponentManager` 的執行個體。 標籤協助程式元件會新增至執行個體的標籤協助程式元件集合。 下列 Razor 頁面的頁面模型使用 `AddressTagHelperComponent` 來示範這項技術：

[!code-csharp[](th-components/samples/RazorPagesSample/Pages/Index.cshtml.cs?name=snippet_IndexModelClass)]

在上述程式碼中：

* 建構函式插入可用於存取 `ITagHelperComponentManager` 的執行個體。
* `AddressTagHelperComponent` 的執行個體會新增至應用程式標籤協助程式元件集合。

## <a name="create-a-component"></a>建立元件

建立自訂標籤協助程式元件：

* 建立衍生自 <xref:Microsoft.AspNetCore.Mvc.Razor.TagHelpers.TagHelperComponentTagHelper> 的公用類別。
* 將 [[HtmlTargetElement]](xref:Microsoft.AspNetCore.Razor.TagHelpers.HtmlTargetElementAttribute) 屬性套用至該類別。 指定目標 HTML 項目的名稱。
* *選擇性*：將 [[EditorBrowsable(EditorBrowsableState.Never)]](xref:System.ComponentModel.EditorBrowsableAttribute) 屬性套用至類別，以在 IntelliSense 中隱藏該類型的顯示。

下列程式碼會建立以 `<address>` HTML 項目為目標的自訂標籤協助程式元件：

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressTagHelperComponentTagHelper.cs)]

使用自訂 `address` 標籤協助程式元件來插入 HTML 標記，如下所示：

```csharp
public class AddressTagHelperComponent : TagHelperComponent
{
    private readonly string _printableButton =
        "<button type='button' class='btn btn-info' onclick=\"window.open("
        "'https://binged.it/2AXRRYw')\">" +
        "<span class='glyphicon glyphicon-road' aria-hidden='true'></span>" +
        "</button>";

    public override int Order => 3;

    public override async Task ProcessAsync(TagHelperContext context,
                                            TagHelperOutput output)
    {
        if (string.Equals(context.TagName, "address",
                StringComparison.OrdinalIgnoreCase) &&
            output.Attributes.ContainsName("printable"))
        {
            var content = await output.GetChildContentAsync();
            output.Content.SetHtmlContent(
                $"<div>{content.GetContent()}</div>{_printableButton}");
        }
    }
}
```

上述 `ProcessAsync` 方法，會將提供給 <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperContent.SetHtmlContent*> 的 HTML，插入相符的 `<address>` 項目。 插入發生於：

* 執行內容的 `TagName` 屬性值等於 `address`。
* 對應的 `<address>` 項目具有 `printable` 屬性。

例如，當處理下列 `<address>` 項目時，`if` 陳述式會評估為 True：

[!code-cshtml[](th-components/samples/RazorPagesSample/Pages/Contact.cshtml?name=snippet_AddressPrintable)]

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
* <xref:mvc/views/tag-helpers/builtin-th/Index>
