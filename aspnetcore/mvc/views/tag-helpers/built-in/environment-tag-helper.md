---
title: "在 ASP.NET Core 環境標記協助程式"
author: pkellner
description: "ASP.NET Core 環境標記協助程式定義包括所有屬性"
ms.author: riande
manager: wpickett
ms.date: 07/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 32646f1fdaf840f796da1ec573459157a41a86d1
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="environment-tag-helper-in-aspnet-core"></a><span data-ttu-id="00db7-103">在 ASP.NET Core 環境標記協助程式</span><span class="sxs-lookup"><span data-stu-id="00db7-103">Environment Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="00db7-104">由[Peter Kellner](http://peterkellner.net)和[Hisham Bin Ateya](https://twitter.com/hishambinateya)</span><span class="sxs-lookup"><span data-stu-id="00db7-104">By [Peter Kellner](http://peterkellner.net) and [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span></span>

<span data-ttu-id="00db7-105">環境標記協助程式有條件地呈現其括住之目前裝載環境為基礎的內容。</span><span class="sxs-lookup"><span data-stu-id="00db7-105">The Environment Tag Helper conditionally renders its enclosed content based on the current hosting environment.</span></span> <span data-ttu-id="00db7-106">其單一屬性`names`是以逗號分隔清單的環境名稱，如果任何符合目前的環境，將會觸發要呈現括住的內容。</span><span class="sxs-lookup"><span data-stu-id="00db7-106">Its single attribute `names` is a comma separated list of environment names, that if any match to the current environment, will trigger the enclosed content to be rendered.</span></span>

## <a name="environment-tag-helper-attributes"></a><span data-ttu-id="00db7-107">環境標記協助程式屬性</span><span class="sxs-lookup"><span data-stu-id="00db7-107">Environment Tag Helper Attributes</span></span>

### <a name="names"></a><span data-ttu-id="00db7-108">名稱</span><span class="sxs-lookup"><span data-stu-id="00db7-108">names</span></span>

<span data-ttu-id="00db7-109">接受單一裝載環境名稱或裝載觸發括住的內容轉譯的環境名稱的逗號分隔清單。</span><span class="sxs-lookup"><span data-stu-id="00db7-109">Accepts a single hosting environment name or a comma-separated list of hosting environment names that trigger the rendering of the enclosed content.</span></span>

<span data-ttu-id="00db7-110">這些值會從 ASP.NET Core 靜態屬性傳回的目前值比較`HostingEnvironment.EnvironmentName`。</span><span class="sxs-lookup"><span data-stu-id="00db7-110">These value(s) are compared to the current value returned from the ASP.NET Core static property `HostingEnvironment.EnvironmentName`.</span></span>  <span data-ttu-id="00db7-111">這個值可以是下列其中之一：**臨時**;**開發**或**生產**。</span><span class="sxs-lookup"><span data-stu-id="00db7-111">This value is one of the following: **Staging**; **Development** or **Production**.</span></span> <span data-ttu-id="00db7-112">比較會忽略大小寫。</span><span class="sxs-lookup"><span data-stu-id="00db7-112">The comparison ignores case.</span></span>

<span data-ttu-id="00db7-113">有效範例`environment`標記協助程式：</span><span class="sxs-lookup"><span data-stu-id="00db7-113">An example of a valid `environment` tag helper is:</span></span>

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a><span data-ttu-id="00db7-114">包含與排除屬性</span><span class="sxs-lookup"><span data-stu-id="00db7-114">include and exclude attributes</span></span>

<span data-ttu-id="00db7-115">2.x 加入 ASP.NET Core `include`  &  `exclude`屬性。</span><span class="sxs-lookup"><span data-stu-id="00db7-115">ASP.NET Core 2.x adds the `include` & `exclude` attributes.</span></span> <span data-ttu-id="00db7-116">這些屬性會控制轉譯括住包含或排除裝載環境名稱為基礎的內容。</span><span class="sxs-lookup"><span data-stu-id="00db7-116">These attributes control rendering the enclosed content based on the included or excluded hosting environment names.</span></span>

### <a name="include-aspnet-core-20-and-later"></a><span data-ttu-id="00db7-117">加入 ASP.NET Core 2.0 和更新版本</span><span class="sxs-lookup"><span data-stu-id="00db7-117">include ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="00db7-118">`include`屬性具有類似行為`names`ASP.NET Core 1.0 中的屬性。</span><span class="sxs-lookup"><span data-stu-id="00db7-118">The `include` property has a similar behavior of the `names` attribute in ASP.NET Core 1.0.</span></span>

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a><span data-ttu-id="00db7-119">排除 2.0 和更新版本的 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="00db7-119">exclude ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="00db7-120">相反地，`exclude`屬性可讓`EnvironmentTagHelper`轉譯為所有的裝載環境名稱，除非您指定的結束括住的內容。</span><span class="sxs-lookup"><span data-stu-id="00db7-120">In contrast, the `exclude` property lets the `EnvironmentTagHelper` render the enclosed content for all hosting environment names except the one(s) that you specified.</span></span>

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a><span data-ttu-id="00db7-121">其他資源</span><span class="sxs-lookup"><span data-stu-id="00db7-121">Additional resources</span></span>

* <xref:fundamentals/environments>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
