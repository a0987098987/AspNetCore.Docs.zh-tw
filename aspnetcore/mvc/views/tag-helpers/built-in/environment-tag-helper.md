---
title: ASP.NET Core 中的環境標籤協助程式
author: pkellner
description: 定義了 ASP.NET Core 環境標籤協助程式，包括所有屬性
manager: wpickett
ms.author: riande
ms.date: 07/14/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 7a99ee0e59c7f49a3208d2c86c11cabce4294889
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
ms.locfileid: "28901180"
---
# <a name="environment-tag-helper-in-aspnet-core"></a><span data-ttu-id="0449f-103">ASP.NET Core 中的環境標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="0449f-103">Environment Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="0449f-104">作者：[Peter Kellner](http://peterkellner.net) 和 [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span><span class="sxs-lookup"><span data-stu-id="0449f-104">By [Peter Kellner](http://peterkellner.net) and [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span></span>

<span data-ttu-id="0449f-105">環境標籤協助程式依據目前的主控環境，有條件地呈現含括內容。</span><span class="sxs-lookup"><span data-stu-id="0449f-105">The Environment Tag Helper conditionally renders its enclosed content based on the current hosting environment.</span></span> <span data-ttu-id="0449f-106">其單一屬性 `names` 是以逗號分隔的環境名稱清單，如果有任一項符合目前的環境，將會觸發呈現含括的內容。</span><span class="sxs-lookup"><span data-stu-id="0449f-106">Its single attribute `names` is a comma separated list of environment names, that if any match to the current environment, will trigger the enclosed content to be rendered.</span></span>

## <a name="environment-tag-helper-attributes"></a><span data-ttu-id="0449f-107">環境標籤協助程式屬性</span><span class="sxs-lookup"><span data-stu-id="0449f-107">Environment Tag Helper Attributes</span></span>

### <a name="names"></a><span data-ttu-id="0449f-108">名稱</span><span class="sxs-lookup"><span data-stu-id="0449f-108">names</span></span>

<span data-ttu-id="0449f-109">接受單一主控環境名稱或以逗號分隔的主控環境名稱清單，這些名稱會觸發呈現含括的內容。</span><span class="sxs-lookup"><span data-stu-id="0449f-109">Accepts a single hosting environment name or a comma-separated list of hosting environment names that trigger the rendering of the enclosed content.</span></span>

<span data-ttu-id="0449f-110">這些值會與從 ASP.NET Core 靜態屬性 `HostingEnvironment.EnvironmentName` 傳回的目前值進行比較。</span><span class="sxs-lookup"><span data-stu-id="0449f-110">These value(s) are compared to the current value returned from the ASP.NET Core static property `HostingEnvironment.EnvironmentName`.</span></span>  <span data-ttu-id="0449f-111">這個值可以是下列其中一項：**Staging**、**Development** 或 **Production**。</span><span class="sxs-lookup"><span data-stu-id="0449f-111">This value is one of the following: **Staging**; **Development** or **Production**.</span></span> <span data-ttu-id="0449f-112">比較會忽略大小寫。</span><span class="sxs-lookup"><span data-stu-id="0449f-112">The comparison ignores case.</span></span>

<span data-ttu-id="0449f-113">有效的 `environment` 標籤協助程式範例為：</span><span class="sxs-lookup"><span data-stu-id="0449f-113">An example of a valid `environment` tag helper is:</span></span>

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a><span data-ttu-id="0449f-114">include 和 exclude 屬性</span><span class="sxs-lookup"><span data-stu-id="0449f-114">include and exclude attributes</span></span>

<span data-ttu-id="0449f-115">ASP.NET Core 2.x 新增了 `include`  &  `exclude` 屬性。</span><span class="sxs-lookup"><span data-stu-id="0449f-115">ASP.NET Core 2.x adds the `include` & `exclude` attributes.</span></span> <span data-ttu-id="0449f-116">這些屬性會依據包含或排除的主控環境名稱來控制含括內容的呈現。</span><span class="sxs-lookup"><span data-stu-id="0449f-116">These attributes control rendering the enclosed content based on the included or excluded hosting environment names.</span></span>

### <a name="include-aspnet-core-20-and-later"></a><span data-ttu-id="0449f-117">include - ASP.NET Core 2.0 和更新版本</span><span class="sxs-lookup"><span data-stu-id="0449f-117">include ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="0449f-118">`include` 屬性的行為類似於 ASP.NET Core 1.0 中的 `names` 屬性。</span><span class="sxs-lookup"><span data-stu-id="0449f-118">The `include` property has a similar behavior of the `names` attribute in ASP.NET Core 1.0.</span></span>

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a><span data-ttu-id="0449f-119">exclude - ASP.NET Core 2.0 和更新版本</span><span class="sxs-lookup"><span data-stu-id="0449f-119">exclude ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="0449f-120">相較之下，`exclude` 屬性可讓 `EnvironmentTagHelper` 呈現所有主控環境名稱 (除了您指定的主控環境名稱之外) 的含括內容。</span><span class="sxs-lookup"><span data-stu-id="0449f-120">In contrast, the `exclude` property lets the `EnvironmentTagHelper` render the enclosed content for all hosting environment names except the one(s) that you specified.</span></span>

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a><span data-ttu-id="0449f-121">其他資源</span><span class="sxs-lookup"><span data-stu-id="0449f-121">Additional resources</span></span>

* <xref:fundamentals/environments>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
