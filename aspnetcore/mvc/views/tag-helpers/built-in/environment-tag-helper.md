---
title: ASP.NET Core 中的環境標籤協助程式
author: pkellner
description: 定義了 ASP.NET Core 環境標籤協助程式，包括所有屬性
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 379f58ed37329f047d53adf1dcfdfd2ad6a6ca4e
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64887673"
---
# <a name="environment-tag-helper-in-aspnet-core"></a><span data-ttu-id="66696-103">ASP.NET Core 中的環境標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="66696-103">Environment Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="66696-104">作者：[Peter Kellner](http://peterkellner.net)、[Hisham Bin Ateya](https://twitter.com/hishambinateya) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="66696-104">By [Peter Kellner](http://peterkellner.net), [Hisham Bin Ateya](https://twitter.com/hishambinateya), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="66696-105">環境標籤協助程式依據目前的[主控環境](xref:fundamentals/environments)，有條件地轉譯含括內容。</span><span class="sxs-lookup"><span data-stu-id="66696-105">The Environment Tag Helper conditionally renders its enclosed content based on the current [hosting environment](xref:fundamentals/environments).</span></span> <span data-ttu-id="66696-106">環境標籤協助程式的單一屬性 `names`，是以逗號分隔的環境名稱清單。</span><span class="sxs-lookup"><span data-stu-id="66696-106">The Environment Tag Helper's single attribute, `names`, is a comma-separated list of environment names.</span></span> <span data-ttu-id="66696-107">如果任何提供的環境名稱符合目前環境，則會轉譯含括的內容。</span><span class="sxs-lookup"><span data-stu-id="66696-107">If any of the provided environment names match the current environment, the enclosed content is rendered.</span></span>

<span data-ttu-id="66696-108">如需標籤協助程式的概觀，請參閱 <xref:mvc/views/tag-helpers/intro>。</span><span class="sxs-lookup"><span data-stu-id="66696-108">For an overview of Tag Helpers, see <xref:mvc/views/tag-helpers/intro>.</span></span>

## <a name="environment-tag-helper-attributes"></a><span data-ttu-id="66696-109">環境標籤協助程式屬性</span><span class="sxs-lookup"><span data-stu-id="66696-109">Environment Tag Helper Attributes</span></span>

### <a name="names"></a><span data-ttu-id="66696-110">名稱</span><span class="sxs-lookup"><span data-stu-id="66696-110">names</span></span>

<span data-ttu-id="66696-111">`names` 會接受單一主控環境名稱或以逗號分隔的主控環境名稱清單，這些名稱會觸發轉譯含括的內容。</span><span class="sxs-lookup"><span data-stu-id="66696-111">`names` accepts a single hosting environment name or a comma-separated list of hosting environment names that trigger the rendering of the enclosed content.</span></span>

<span data-ttu-id="66696-112">環境值會與 [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName*) 所傳回的目前值比較。</span><span class="sxs-lookup"><span data-stu-id="66696-112">Environment values are compared to the current value returned by [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName*).</span></span> <span data-ttu-id="66696-113">比較會忽略大小寫。</span><span class="sxs-lookup"><span data-stu-id="66696-113">The comparison ignores case.</span></span>

<span data-ttu-id="66696-114">下列範例使用環境標籤協助程式。</span><span class="sxs-lookup"><span data-stu-id="66696-114">The following example uses an Environment Tag Helper.</span></span> <span data-ttu-id="66696-115">如果主控環境為「暫存」或「生產」，將會轉譯內容：</span><span class="sxs-lookup"><span data-stu-id="66696-115">The content is rendered if the hosting environment is Staging or Production:</span></span>

```cshtml
<environment names="Staging,Production">
    <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

::: moniker range=">= aspnetcore-2.0"

## <a name="include-and-exclude-attributes"></a><span data-ttu-id="66696-116">include 和 exclude 屬性</span><span class="sxs-lookup"><span data-stu-id="66696-116">include and exclude attributes</span></span>

<span data-ttu-id="66696-117">`include` & `exclude` 屬性會依據包含或排除的主控環境名稱，來控制含括內容的轉譯。</span><span class="sxs-lookup"><span data-stu-id="66696-117">`include` & `exclude` attributes control rendering the enclosed content based on the included or excluded hosting environment names.</span></span>

### <a name="include"></a><span data-ttu-id="66696-118">include</span><span class="sxs-lookup"><span data-stu-id="66696-118">include</span></span>

<span data-ttu-id="66696-119">`include` 屬性會表現出類似 `names` 屬性的行為。</span><span class="sxs-lookup"><span data-stu-id="66696-119">The `include` property exhibits similar behavior to the `names` attribute.</span></span> <span data-ttu-id="66696-120">`include` 屬性值中列出的環境，必須與應用程式的主控環境 ([IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName*)) 相符，才能轉譯 `<environment>` 標籤的內容。</span><span class="sxs-lookup"><span data-stu-id="66696-120">An environment listed in the `include` attribute value must match the app's hosting environment ([IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName*)) to render the content of the `<environment>` tag.</span></span>

```cshtml
<environment include="Staging,Production">
    <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude"></a><span data-ttu-id="66696-121">exclude</span><span class="sxs-lookup"><span data-stu-id="66696-121">exclude</span></span>

<span data-ttu-id="66696-122">與 `include` 屬性相反，當主控環境與 `exclude` 屬性值中列出的環境不相符時，將轉譯 `<environment>` 標記的內容。</span><span class="sxs-lookup"><span data-stu-id="66696-122">In contrast to the `include` attribute, the content of the `<environment>` tag is rendered when the hosting environment doesn't match an environment listed in the `exclude` attribute value.</span></span>

```cshtml
<environment exclude="Development">
    <strong>HostingEnvironment.EnvironmentName is not Development</strong>
</environment>
```

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="66696-123">其他資源</span><span class="sxs-lookup"><span data-stu-id="66696-123">Additional resources</span></span>

* <xref:fundamentals/environments>
