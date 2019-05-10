---
title: ASP.NET Core 中的標籤協助程式元件
author: scottaddie
description: 了解何謂標籤協助程式元件，以及如何在 ASP.NET Core 中使用。
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.date: 04/06/2019
uid: mvc/views/tag-helpers/th-components
ms.openlocfilehash: 777bbcca3d1c2f03f386fa62df2a2e0b00409318
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64889733"
---
# <a name="tag-helper-components-in-aspnet-core"></a><span data-ttu-id="51efd-103">ASP.NET Core 中的標籤協助程式元件</span><span class="sxs-lookup"><span data-stu-id="51efd-103">Tag Helper Components in ASP.NET Core</span></span>

<span data-ttu-id="51efd-104">作者：[Scott Addie](https://twitter.com/Scott_Addie) 和 [Fiyaz Bin Hasan](https://github.com/fiyazbinhasan)</span><span class="sxs-lookup"><span data-stu-id="51efd-104">By [Scott Addie](https://twitter.com/Scott_Addie) and [Fiyaz Bin Hasan](https://github.com/fiyazbinhasan)</span></span>

<span data-ttu-id="51efd-105">標籤協助程式元件是一種標籤協助程式，允許您從伺服器端程式碼有條件地修改或新增 HTML 項目。</span><span class="sxs-lookup"><span data-stu-id="51efd-105">A Tag Helper Component is a Tag Helper that allows you to conditionally modify or add HTML elements from server-side code.</span></span> <span data-ttu-id="51efd-106">ASP.NET Core 2.0 或更新版本提供此功能。</span><span class="sxs-lookup"><span data-stu-id="51efd-106">This feature is available in ASP.NET Core 2.0 or later.</span></span>

<span data-ttu-id="51efd-107">ASP.NET Core 包含兩個內建標籤協助程式元件：`head` 和 `body`。</span><span class="sxs-lookup"><span data-stu-id="51efd-107">ASP.NET Core includes two built-in Tag Helper Components: `head` and `body`.</span></span> <span data-ttu-id="51efd-108">這兩個標籤協助程式元件位於 <xref:Microsoft.AspNetCore.Mvc.Razor.TagHelpers> 命名空間，可用於 MVC 和 Razor Pages。</span><span class="sxs-lookup"><span data-stu-id="51efd-108">They're located in the <xref:Microsoft.AspNetCore.Mvc.Razor.TagHelpers> namespace and can be used in both MVC and Razor Pages.</span></span> <span data-ttu-id="51efd-109">標籤協助程式元件不需要在 *_ViewImports.cshtml* 中註冊應用程式。</span><span class="sxs-lookup"><span data-stu-id="51efd-109">Tag Helper Components don't require registration with the app in *_ViewImports.cshtml*.</span></span>

<span data-ttu-id="51efd-110">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/tag-helpers/th-components/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="51efd-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/tag-helpers/th-components/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="use-cases"></a><span data-ttu-id="51efd-111">使用案例</span><span class="sxs-lookup"><span data-stu-id="51efd-111">Use cases</span></span>

<span data-ttu-id="51efd-112">兩個常見的標籤協助程式元件的使用案例為：</span><span class="sxs-lookup"><span data-stu-id="51efd-112">Two common use cases of Tag Helper Components include:</span></span>

1. [<span data-ttu-id="51efd-113">將 `<link>` 插入 `<head>`。</span><span class="sxs-lookup"><span data-stu-id="51efd-113">Injecting a `<link>` into the `<head>`.</span></span>](#inject-into-html-head-element)
1. [<span data-ttu-id="51efd-114">將 `<script>` 插入 `<body>`。</span><span class="sxs-lookup"><span data-stu-id="51efd-114">Injecting a `<script>` into the `<body>`.</span></span>](#inject-into-html-body-element)

<span data-ttu-id="51efd-115">下列各節描述這些案例。</span><span class="sxs-lookup"><span data-stu-id="51efd-115">The following sections describe these use cases.</span></span>

### <a name="inject-into-html-head-element"></a><span data-ttu-id="51efd-116">插入 HTML 標頭項目</span><span class="sxs-lookup"><span data-stu-id="51efd-116">Inject into HTML head element</span></span>

<span data-ttu-id="51efd-117">在 HTML `<head>` 項目中，CSS 檔案一般與 HTML `<link>` 項目一起匯入。</span><span class="sxs-lookup"><span data-stu-id="51efd-117">Inside the HTML `<head>` element, CSS files are commonly imported with the HTML `<link>` element.</span></span> <span data-ttu-id="51efd-118">下列程式碼會使用 `head` 標籤協助程式元件，將 `<link>` 項目插入 `<head>` 項目：</span><span class="sxs-lookup"><span data-stu-id="51efd-118">The following code injects a `<link>` element into the `<head>` element using the `head` Tag Helper Component:</span></span>

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressStyleTagHelperComponent.cs)]

<span data-ttu-id="51efd-119">在上述程式碼中：</span><span class="sxs-lookup"><span data-stu-id="51efd-119">In the preceding code:</span></span>

* <span data-ttu-id="51efd-120">`AddressStyleTagHelperComponent` 會實作 <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperComponent>。</span><span class="sxs-lookup"><span data-stu-id="51efd-120">`AddressStyleTagHelperComponent` implements <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperComponent>.</span></span> <span data-ttu-id="51efd-121">抽象：</span><span class="sxs-lookup"><span data-stu-id="51efd-121">The abstraction:</span></span>
  * <span data-ttu-id="51efd-122">允許使用 <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperContext> 初始化類別。</span><span class="sxs-lookup"><span data-stu-id="51efd-122">Allows initialization of the class with a <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperContext>.</span></span>
  * <span data-ttu-id="51efd-123">可讓您使用標籤協助程式元件新增或修改 HTML 項目。</span><span class="sxs-lookup"><span data-stu-id="51efd-123">Enables the use of Tag Helper Components to add or modify HTML elements.</span></span>
* <span data-ttu-id="51efd-124"><xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperComponent.Order*> 屬性會定義元件的轉譯順序。</span><span class="sxs-lookup"><span data-stu-id="51efd-124">The <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperComponent.Order*> property defines the order in which the Components are rendered.</span></span> <span data-ttu-id="51efd-125">當應用程式中有多種標籤協助元件用法時，則 `Order` 為必要。</span><span class="sxs-lookup"><span data-stu-id="51efd-125">`Order` is necessary when there are multiple usages of Tag Helper Components in an app.</span></span>
* <span data-ttu-id="51efd-126"><xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperComponent.ProcessAsync*> 會將執行內容的 <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperContext.TagName*> 屬性值與 `head` 進行比較。</span><span class="sxs-lookup"><span data-stu-id="51efd-126"><xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperComponent.ProcessAsync*> compares the execution context's <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperContext.TagName*> property value to `head`.</span></span> <span data-ttu-id="51efd-127">如果比較評估為 True，則 `_style` 欄位的內容會插入 HTML `<head>` 項目。</span><span class="sxs-lookup"><span data-stu-id="51efd-127">If the comparison evaluates to true, the content of the `_style` field is injected into the HTML `<head>` element.</span></span>

### <a name="inject-into-html-body-element"></a><span data-ttu-id="51efd-128">插入 HTML 主體項目</span><span class="sxs-lookup"><span data-stu-id="51efd-128">Inject into HTML body element</span></span>

<span data-ttu-id="51efd-129">`body` 標籤協助程式元件可將 `<script>` 項目插入 `<body>` 項目。</span><span class="sxs-lookup"><span data-stu-id="51efd-129">The `body` Tag Helper Component can inject a `<script>` element into the `<body>` element.</span></span> <span data-ttu-id="51efd-130">下列程式碼示範這項技術：</span><span class="sxs-lookup"><span data-stu-id="51efd-130">The following code demonstrates this technique:</span></span>

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressScriptTagHelperComponent.cs)]

<span data-ttu-id="51efd-131">個別的 HTML 檔案用於儲存 `<script>` 項目。</span><span class="sxs-lookup"><span data-stu-id="51efd-131">A separate HTML file is used to store the `<script>` element.</span></span> <span data-ttu-id="51efd-132">該 HTML 檔案會使程式碼更簡潔且更容易維護。</span><span class="sxs-lookup"><span data-stu-id="51efd-132">The HTML file makes the code cleaner and more maintainable.</span></span> <span data-ttu-id="51efd-133">上述程式碼會讀取 *TagHelpers/Templates/AddressToolTipScript.html* 的內容，並為其附加標籤協助程式輸出。</span><span class="sxs-lookup"><span data-stu-id="51efd-133">The preceding code reads the contents of *TagHelpers/Templates/AddressToolTipScript.html* and appends it with the Tag Helper output.</span></span> <span data-ttu-id="51efd-134">*AddressToolTipScript.html* 檔案包含下列標記：</span><span class="sxs-lookup"><span data-stu-id="51efd-134">The *AddressToolTipScript.html* file includes the following markup:</span></span>

[!code-html[](th-components/samples/RazorPagesSample/TagHelpers/Templates/AddressToolTipScript.html)]

<span data-ttu-id="51efd-135">上述程式碼會將 [ 啟動程序工具提示小工具](https://getbootstrap.com/docs/3.3/javascript/#tooltips)繫結至包含 `printable` 屬性的任何 `<address>` 項目。</span><span class="sxs-lookup"><span data-stu-id="51efd-135">The preceding code binds a [Bootstrap tooltip widget](https://getbootstrap.com/docs/3.3/javascript/#tooltips) to any `<address>` element that includes a `printable` attribute.</span></span> <span data-ttu-id="51efd-136">當滑鼠指標停留在項目上時，會顯示效果。</span><span class="sxs-lookup"><span data-stu-id="51efd-136">The effect is visible when a mouse pointer hovers over the element.</span></span>

## <a name="register-a-component"></a><span data-ttu-id="51efd-137">註冊元件</span><span class="sxs-lookup"><span data-stu-id="51efd-137">Register a Component</span></span>

<span data-ttu-id="51efd-138">標籤協助程式元件必須新增至應用程式的標籤協助程式元件集合中。</span><span class="sxs-lookup"><span data-stu-id="51efd-138">A Tag Helper Component must be added to the app's Tag Helper Components collection.</span></span> <span data-ttu-id="51efd-139">有三種方式可新增至集合：</span><span class="sxs-lookup"><span data-stu-id="51efd-139">There are three ways to add to the collection:</span></span>

1. [<span data-ttu-id="51efd-140">透過服務容器註冊</span><span class="sxs-lookup"><span data-stu-id="51efd-140">Registration via services container</span></span>](#registration-via-services-container)
1. [<span data-ttu-id="51efd-141">透過 Razor 檔案註冊</span><span class="sxs-lookup"><span data-stu-id="51efd-141">Registration via Razor file</span></span>](#registration-via-razor-file)
1. [<span data-ttu-id="51efd-142">透過頁面模型或控制器註冊</span><span class="sxs-lookup"><span data-stu-id="51efd-142">Registration via Page Model or controller</span></span>](#registration-via-page-model-or-controller)

### <a name="registration-via-services-container"></a><span data-ttu-id="51efd-143">透過服務容器註冊</span><span class="sxs-lookup"><span data-stu-id="51efd-143">Registration via services container</span></span>

<span data-ttu-id="51efd-144">如果標籤協助程式元件類別並未以 <xref:Microsoft.AspNetCore.Mvc.Razor.TagHelpers.ITagHelperComponentManager> 管理，則必須使用[相依性插入 (DI)](xref:fundamentals/dependency-injection) 系統來註冊。</span><span class="sxs-lookup"><span data-stu-id="51efd-144">If the Tag Helper Component class isn't managed with <xref:Microsoft.AspNetCore.Mvc.Razor.TagHelpers.ITagHelperComponentManager>, it must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) system.</span></span> <span data-ttu-id="51efd-145">下列 `Startup.ConfigureServices` 程式碼會使用[暫時性存留期](xref:fundamentals/dependency-injection#lifetime-and-registration-options)來註冊 `AddressStyleTagHelperComponent` 與 `AddressScriptTagHelperComponent` 類別：</span><span class="sxs-lookup"><span data-stu-id="51efd-145">The following `Startup.ConfigureServices` code registers the `AddressStyleTagHelperComponent` and `AddressScriptTagHelperComponent` classes with a [transient lifetime](xref:fundamentals/dependency-injection#lifetime-and-registration-options):</span></span>

[!code-csharp[](th-components/samples/RazorPagesSample/Startup.cs?name=snippet_ConfigureServices&highlight=12-15)]

### <a name="registration-via-razor-file"></a><span data-ttu-id="51efd-146">透過 Razor 檔案註冊</span><span class="sxs-lookup"><span data-stu-id="51efd-146">Registration via Razor file</span></span>

<span data-ttu-id="51efd-147">如果標籤協助程式元件並未註冊為 DI，則可以從 Razor Pages 頁面或 MVC 檢視中註冊。</span><span class="sxs-lookup"><span data-stu-id="51efd-147">If the Tag Helper Component isn't registered with DI, it can be registered from a Razor Pages page or an MVC view.</span></span> <span data-ttu-id="51efd-148">這項技術用於控制插入的標記和 Razor 檔案中元件執行順序。</span><span class="sxs-lookup"><span data-stu-id="51efd-148">This technique is used for controlling the injected markup and the component execution order from a Razor file.</span></span>

<span data-ttu-id="51efd-149">`ITagHelperComponentManager` 用於新增標籤協助程式元件，或從應用程式移除。</span><span class="sxs-lookup"><span data-stu-id="51efd-149">`ITagHelperComponentManager` is used to add Tag Helper Components or remove them from the app.</span></span> <span data-ttu-id="51efd-150">下列程式碼使用 `AddressTagHelperComponent` 示範這項技術：</span><span class="sxs-lookup"><span data-stu-id="51efd-150">The following code demonstrates this technique with `AddressTagHelperComponent`:</span></span>

[!code-cshtml[](th-components/samples/RazorPagesSample/Pages/Contact.cshtml?name=snippet_ITagHelperComponentManager)]

<span data-ttu-id="51efd-151">在上述程式碼中：</span><span class="sxs-lookup"><span data-stu-id="51efd-151">In the preceding code:</span></span>

* <span data-ttu-id="51efd-152">`@inject` 指示詞會提供 `ITagHelperComponentManager` 的執行個體。</span><span class="sxs-lookup"><span data-stu-id="51efd-152">The `@inject` directive provides an instance of `ITagHelperComponentManager`.</span></span> <span data-ttu-id="51efd-153">該執行個體會指派給名為 `manager` 的變數，用於 Razor 檔案中的下游存取。</span><span class="sxs-lookup"><span data-stu-id="51efd-153">The instance is assigned to a variable named `manager` for access downstream in the Razor file.</span></span>
* <span data-ttu-id="51efd-154">`AddressTagHelperComponent` 的執行個體會新增至應用程式標籤協助程式元件集合中。</span><span class="sxs-lookup"><span data-stu-id="51efd-154">An instance of `AddressTagHelperComponent` is added to the app's Tag Helper Components collection.</span></span>

<span data-ttu-id="51efd-155">`AddressTagHelperComponent` 已修改，以容納接受 `markup` 和 `order` 參數的建構函式：</span><span class="sxs-lookup"><span data-stu-id="51efd-155">`AddressTagHelperComponent` is modified to accommodate a constructor that accepts the `markup` and `order` parameters:</span></span>

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressTagHelperComponent.cs?name=snippet_Constructor)]

<span data-ttu-id="51efd-156">提供的 `markup` 參數會用於 `ProcessAsync`，如下所示：</span><span class="sxs-lookup"><span data-stu-id="51efd-156">The provided `markup` parameter is used in `ProcessAsync` as follows:</span></span>

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressTagHelperComponent.cs?name=snippet_ProcessAsync&highlight=10-11)]

### <a name="registration-via-page-model-or-controller"></a><span data-ttu-id="51efd-157">透過頁面模型或控制器註冊</span><span class="sxs-lookup"><span data-stu-id="51efd-157">Registration via Page Model or controller</span></span>

<span data-ttu-id="51efd-158">如果標籤協助程式元件並未註冊為 DI，則可以從 Razor Pages 模型或 MVC 控制器進行註冊。</span><span class="sxs-lookup"><span data-stu-id="51efd-158">If the Tag Helper Component isn't registered with DI, it can be registered from a Razor Pages page model or an MVC controller.</span></span> <span data-ttu-id="51efd-159">這項技術可用於將 C# 邏輯從 Razor 檔案分開。</span><span class="sxs-lookup"><span data-stu-id="51efd-159">This technique is useful for separating C# logic from Razor files.</span></span>

<span data-ttu-id="51efd-160">建構函式插入可用於存取 `ITagHelperComponentManager` 的執行個體。</span><span class="sxs-lookup"><span data-stu-id="51efd-160">Constructor injection is used to access an instance of `ITagHelperComponentManager`.</span></span> <span data-ttu-id="51efd-161">標籤協助程式元件會新增至執行個體的標籤協助程式元件集合。</span><span class="sxs-lookup"><span data-stu-id="51efd-161">The Tag Helper Component is added to the instance's Tag Helper Components collection.</span></span> <span data-ttu-id="51efd-162">下列 Razor 頁面的頁面模型使用 `AddressTagHelperComponent` 來示範這項技術：</span><span class="sxs-lookup"><span data-stu-id="51efd-162">The following Razor Pages page model demonstrates this technique with `AddressTagHelperComponent`:</span></span>

[!code-csharp[](th-components/samples/RazorPagesSample/Pages/Index.cshtml.cs?name=snippet_IndexModelClass)]

<span data-ttu-id="51efd-163">在上述程式碼中：</span><span class="sxs-lookup"><span data-stu-id="51efd-163">In the preceding code:</span></span>

* <span data-ttu-id="51efd-164">建構函式插入可用於存取 `ITagHelperComponentManager` 的執行個體。</span><span class="sxs-lookup"><span data-stu-id="51efd-164">Constructor injection is used to access an instance of `ITagHelperComponentManager`.</span></span>
* <span data-ttu-id="51efd-165">`AddressTagHelperComponent` 的執行個體會新增至應用程式標籤協助程式元件集合。</span><span class="sxs-lookup"><span data-stu-id="51efd-165">An instance of `AddressTagHelperComponent` is added to the app's Tag Helper Components collection.</span></span>

## <a name="create-a-component"></a><span data-ttu-id="51efd-166">建立元件</span><span class="sxs-lookup"><span data-stu-id="51efd-166">Create a Component</span></span>

<span data-ttu-id="51efd-167">建立自訂標籤協助程式元件：</span><span class="sxs-lookup"><span data-stu-id="51efd-167">To create a custom Tag Helper Component:</span></span>

* <span data-ttu-id="51efd-168">建立衍生自 <xref:Microsoft.AspNetCore.Mvc.Razor.TagHelpers.TagHelperComponentTagHelper> 的公用類別。</span><span class="sxs-lookup"><span data-stu-id="51efd-168">Create a public class deriving from <xref:Microsoft.AspNetCore.Mvc.Razor.TagHelpers.TagHelperComponentTagHelper>.</span></span>
* <span data-ttu-id="51efd-169">將 [[HtmlTargetElement]](xref:Microsoft.AspNetCore.Razor.TagHelpers.HtmlTargetElementAttribute) 屬性套用至該類別。</span><span class="sxs-lookup"><span data-stu-id="51efd-169">Apply an [[HtmlTargetElement]](xref:Microsoft.AspNetCore.Razor.TagHelpers.HtmlTargetElementAttribute) attribute to the class.</span></span> <span data-ttu-id="51efd-170">指定目標 HTML 項目的名稱。</span><span class="sxs-lookup"><span data-stu-id="51efd-170">Specify the name of the target HTML element.</span></span>
* <span data-ttu-id="51efd-171">*選擇性*：將 [[EditorBrowsable(EditorBrowsableState.Never)]](xref:System.ComponentModel.EditorBrowsableAttribute) 屬性套用至類別，以在 IntelliSense 中隱藏該類型的顯示。</span><span class="sxs-lookup"><span data-stu-id="51efd-171">*Optional*: Apply an [[EditorBrowsable(EditorBrowsableState.Never)]](xref:System.ComponentModel.EditorBrowsableAttribute) attribute to the class to suppress the type's display in IntelliSense.</span></span>

<span data-ttu-id="51efd-172">下列程式碼會建立以 `<address>` HTML 項目為目標的自訂標籤協助程式元件：</span><span class="sxs-lookup"><span data-stu-id="51efd-172">The following code creates a custom Tag Helper Component that targets the `<address>` HTML element:</span></span>

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressTagHelperComponentTagHelper.cs)]

<span data-ttu-id="51efd-173">使用自訂 `address` 標籤協助程式元件來插入 HTML 標記，如下所示：</span><span class="sxs-lookup"><span data-stu-id="51efd-173">Use the custom `address` Tag Helper Component to inject HTML markup as follows:</span></span>

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

<span data-ttu-id="51efd-174">上述 `ProcessAsync` 方法，會將提供給 <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperContent.SetHtmlContent*> 的 HTML，插入相符的 `<address>` 項目。</span><span class="sxs-lookup"><span data-stu-id="51efd-174">The preceding `ProcessAsync` method injects the HTML provided to <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperContent.SetHtmlContent*> into the matching `<address>` element.</span></span> <span data-ttu-id="51efd-175">插入發生於：</span><span class="sxs-lookup"><span data-stu-id="51efd-175">The injection occurs when:</span></span>

* <span data-ttu-id="51efd-176">執行內容的 `TagName` 屬性值等於 `address`。</span><span class="sxs-lookup"><span data-stu-id="51efd-176">The execution context's `TagName` property value equals `address`.</span></span>
* <span data-ttu-id="51efd-177">對應的 `<address>` 項目具有 `printable` 屬性。</span><span class="sxs-lookup"><span data-stu-id="51efd-177">The corresponding `<address>` element has a `printable` attribute.</span></span>

<span data-ttu-id="51efd-178">例如，當處理下列 `<address>` 項目時，`if` 陳述式會評估為 True：</span><span class="sxs-lookup"><span data-stu-id="51efd-178">For example, the `if` statement evaluates to true when processing the following `<address>` element:</span></span>

[!code-cshtml[](th-components/samples/RazorPagesSample/Pages/Contact.cshtml?name=snippet_AddressPrintable)]

## <a name="additional-resources"></a><span data-ttu-id="51efd-179">其他資源</span><span class="sxs-lookup"><span data-stu-id="51efd-179">Additional resources</span></span>

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
* <xref:mvc/views/tag-helpers/builtin-th/Index>
