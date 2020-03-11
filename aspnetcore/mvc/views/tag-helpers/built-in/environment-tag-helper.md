---
title: ASP.NET Core 中的環境標籤協助程式
author: pkellner
description: 定義了 ASP.NET Core 環境標籤協助程式，包括所有屬性
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 308e7db47104ebd4d6bb8d08c64f14bbd118898b
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78663986"
---
# <a name="environment-tag-helper-in-aspnet-core"></a><span data-ttu-id="de537-103">ASP.NET Core 中的環境標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="de537-103">Environment Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="de537-104">作者：[Peter Kellner](https://peterkellner.net) 和 [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span><span class="sxs-lookup"><span data-stu-id="de537-104">By [Peter Kellner](https://peterkellner.net) and [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span></span>

<span data-ttu-id="de537-105">環境標籤協助程式依據目前的[主控環境](xref:fundamentals/environments)，有條件地轉譯含括內容。</span><span class="sxs-lookup"><span data-stu-id="de537-105">The Environment Tag Helper conditionally renders its enclosed content based on the current [hosting environment](xref:fundamentals/environments).</span></span> <span data-ttu-id="de537-106">環境標籤協助程式的單一屬性 `names`，是以逗號分隔的環境名稱清單。</span><span class="sxs-lookup"><span data-stu-id="de537-106">The Environment Tag Helper's single attribute, `names`, is a comma-separated list of environment names.</span></span> <span data-ttu-id="de537-107">如果任何提供的環境名稱符合目前環境，則會轉譯含括的內容。</span><span class="sxs-lookup"><span data-stu-id="de537-107">If any of the provided environment names match the current environment, the enclosed content is rendered.</span></span>

<span data-ttu-id="de537-108">如需標籤協助程式的概觀，請參閱 <xref:mvc/views/tag-helpers/intro>。</span><span class="sxs-lookup"><span data-stu-id="de537-108">For an overview of Tag Helpers, see <xref:mvc/views/tag-helpers/intro>.</span></span>

## <a name="environment-tag-helper-attributes"></a><span data-ttu-id="de537-109">環境標籤協助程式屬性</span><span class="sxs-lookup"><span data-stu-id="de537-109">Environment Tag Helper Attributes</span></span>

### <a name="names"></a><span data-ttu-id="de537-110">名稱</span><span class="sxs-lookup"><span data-stu-id="de537-110">names</span></span>

<span data-ttu-id="de537-111">`names` 會接受單一主控環境名稱或以逗號分隔的主控環境名稱清單，這些名稱會觸發轉譯含括的內容。</span><span class="sxs-lookup"><span data-stu-id="de537-111">`names` accepts a single hosting environment name or a comma-separated list of hosting environment names that trigger the rendering of the enclosed content.</span></span>

<span data-ttu-id="de537-112">環境值會與 [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName*) 所傳回的目前值比較。</span><span class="sxs-lookup"><span data-stu-id="de537-112">Environment values are compared to the current value returned by [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName*).</span></span> <span data-ttu-id="de537-113">比較會忽略大小寫。</span><span class="sxs-lookup"><span data-stu-id="de537-113">The comparison ignores case.</span></span>

<span data-ttu-id="de537-114">下列範例使用環境標籤協助程式。</span><span class="sxs-lookup"><span data-stu-id="de537-114">The following example uses an Environment Tag Helper.</span></span> <span data-ttu-id="de537-115">如果主控環境為「暫存」或「生產」，將會轉譯內容：</span><span class="sxs-lookup"><span data-stu-id="de537-115">The content is rendered if the hosting environment is Staging or Production:</span></span>

```cshtml
<environment names="Staging,Production">
    <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

::: moniker range=">= aspnetcore-2.0"

## <a name="include-and-exclude-attributes"></a><span data-ttu-id="de537-116">include 和 exclude 屬性</span><span class="sxs-lookup"><span data-stu-id="de537-116">include and exclude attributes</span></span>

<span data-ttu-id="de537-117">`include` & `exclude` 屬性控制項會根據內含或排除的主控環境名稱，來呈現已包含的內容。</span><span class="sxs-lookup"><span data-stu-id="de537-117">`include` & `exclude` attributes control rendering the enclosed content based on the included or excluded hosting environment names.</span></span>

### <a name="include"></a><span data-ttu-id="de537-118">include</span><span class="sxs-lookup"><span data-stu-id="de537-118">include</span></span>

<span data-ttu-id="de537-119">`include` 屬性會表現出類似 `names` 屬性的行為。</span><span class="sxs-lookup"><span data-stu-id="de537-119">The `include` property exhibits similar behavior to the `names` attribute.</span></span> <span data-ttu-id="de537-120">`include` 屬性值中列出的環境，必須與應用程式的主控環境 ([IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName*)) 相符，才能轉譯 `<environment>` 標籤的內容。</span><span class="sxs-lookup"><span data-stu-id="de537-120">An environment listed in the `include` attribute value must match the app's hosting environment ([IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName*)) to render the content of the `<environment>` tag.</span></span>

```cshtml
<environment include="Staging,Production">
    <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude"></a><span data-ttu-id="de537-121">排除</span><span class="sxs-lookup"><span data-stu-id="de537-121">exclude</span></span>

<span data-ttu-id="de537-122">與 `include` 屬性相反，當主控環境與 `<environment>` 屬性值中列出的環境不相符時，將轉譯 `exclude` 標記的內容。</span><span class="sxs-lookup"><span data-stu-id="de537-122">In contrast to the `include` attribute, the content of the `<environment>` tag is rendered when the hosting environment doesn't match an environment listed in the `exclude` attribute value.</span></span>

```cshtml
<environment exclude="Development">
    <strong>HostingEnvironment.EnvironmentName is not Development</strong>
</environment>
```

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="de537-123">其他資源</span><span class="sxs-lookup"><span data-stu-id="de537-123">Additional resources</span></span>

* <xref:fundamentals/environments>
