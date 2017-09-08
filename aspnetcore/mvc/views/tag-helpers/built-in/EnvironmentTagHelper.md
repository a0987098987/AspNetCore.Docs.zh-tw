---
title: "在 ASP.NET Core 環境標記協助程式"
author: pkellner
description: "ASP.Net Core 環境標記協助程式定義包括所有屬性"
keywords: "ASP.NET Core，標記協助程式"
ms.author: riande
manager: wpickett
ms.date: 07/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/EnvironmentTagHelper
ms.openlocfilehash: 5870d9ebd02becf29f892c91310022d3b9a6b7af
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2017
---
# <a name="environment-tag-helper-in-aspnet-core"></a><span data-ttu-id="1a1b6-104">在 ASP.NET Core 環境標記協助程式</span><span class="sxs-lookup"><span data-stu-id="1a1b6-104">Environment Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="1a1b6-105">由[Peter Kellner](http://peterkellner.net)和[Hisham Bin Ateya](https://twitter.com/hishambinateya)</span><span class="sxs-lookup"><span data-stu-id="1a1b6-105">By [Peter Kellner](http://peterkellner.net) and [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span></span>

<span data-ttu-id="1a1b6-106">環境標記協助程式有條件地呈現其括住之目前裝載環境為基礎的內容。</span><span class="sxs-lookup"><span data-stu-id="1a1b6-106">The Environment Tag Helper conditionally renders its enclosed content based on the current hosting environment.</span></span> <span data-ttu-id="1a1b6-107">其單一屬性`names`是以逗號分隔清單的環境名稱，如果任何符合目前的環境，將會觸發要呈現括住的內容。</span><span class="sxs-lookup"><span data-stu-id="1a1b6-107">Its single attribute `names` is a comma separated list of environment names, that if any match to the current environment, will trigger the enclosed content to be rendered.</span></span>

## <a name="environment-tag-helper-attributes"></a><span data-ttu-id="1a1b6-108">環境標記協助程式屬性</span><span class="sxs-lookup"><span data-stu-id="1a1b6-108">Environment Tag Helper Attributes</span></span>

### <a name="names"></a><span data-ttu-id="1a1b6-109">名稱</span><span class="sxs-lookup"><span data-stu-id="1a1b6-109">names</span></span>

<span data-ttu-id="1a1b6-110">接受單一裝載環境名稱或裝載觸發括住的內容轉譯的環境名稱的逗號分隔清單。</span><span class="sxs-lookup"><span data-stu-id="1a1b6-110">Accepts a single hosting environment name or a comma-separated list of hosting environment names that trigger the rendering of the enclosed content.</span></span>

<span data-ttu-id="1a1b6-111">這些值會從 ASP.NET Core 靜態屬性傳回的目前值比較`HostingEnvironment.EnvironmentName`。</span><span class="sxs-lookup"><span data-stu-id="1a1b6-111">These value(s) are compared to the current value returned from the ASP.NET Core static property `HostingEnvironment.EnvironmentName`.</span></span>  <span data-ttu-id="1a1b6-112">這個值可以是下列其中之一：**臨時**;**開發**或**生產**。</span><span class="sxs-lookup"><span data-stu-id="1a1b6-112">This value is one of the following: **Staging**; **Development** or **Production**.</span></span> <span data-ttu-id="1a1b6-113">比較會忽略大小寫。</span><span class="sxs-lookup"><span data-stu-id="1a1b6-113">The comparison ignores case.</span></span>

<span data-ttu-id="1a1b6-114">有效範例`environment`標記協助程式：</span><span class="sxs-lookup"><span data-stu-id="1a1b6-114">An example of a valid `environment` tag helper is:</span></span>

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a><span data-ttu-id="1a1b6-115">包含與排除屬性</span><span class="sxs-lookup"><span data-stu-id="1a1b6-115">include and exclude attributes</span></span>

<span data-ttu-id="1a1b6-116">2.x 加入 ASP.NET Core `include`  &  `exclude`屬性。</span><span class="sxs-lookup"><span data-stu-id="1a1b6-116">ASP.NET Core 2.x adds the `include` & `exclude` attributes.</span></span> <span data-ttu-id="1a1b6-117">這些屬性會控制轉譯括住包含或排除裝載環境名稱為基礎的內容。</span><span class="sxs-lookup"><span data-stu-id="1a1b6-117">These attributes control rendering the enclosed content based on the included or excluded hosting environment names.</span></span>

### <a name="include-aspnet-core-20-and-later"></a><span data-ttu-id="1a1b6-118">加入 ASP.NET Core 2.0 和更新版本</span><span class="sxs-lookup"><span data-stu-id="1a1b6-118">include ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="1a1b6-119">`include`屬性具有類似行為`names`ASP.NET Core 1.0 中的屬性。</span><span class="sxs-lookup"><span data-stu-id="1a1b6-119">The `include` property has a similar behavior of the `names` attribute in ASP.NET Core 1.0.</span></span>

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a><span data-ttu-id="1a1b6-120">排除 2.0 和更新版本的 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1a1b6-120">exclude ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="1a1b6-121">相反地，`exclude`屬性可讓`EnvironmentTagHelper`轉譯為所有的裝載環境名稱，除非您指定的結束括住的內容。</span><span class="sxs-lookup"><span data-stu-id="1a1b6-121">In contrast, the `exclude` property lets the `EnvironmentTagHelper` render the enclosed content for all hosting environment names except the one(s) that you specified.</span></span>

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a><span data-ttu-id="1a1b6-122">其他資源</span><span class="sxs-lookup"><span data-stu-id="1a1b6-122">Additional resources</span></span>

* <xref:fundamentals/environments>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>