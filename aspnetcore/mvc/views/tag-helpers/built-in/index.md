---
title: "ASP.NET Core 內建的標記協助程式"
author: pkellner
description: "ASP.NET Core 內建的標記協助程式"
keywords: "ASP.NET Core,標記協助程式"
ms.author: riande
manager: wpickett
ms.date: 7/11/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/Index
ms.openlocfilehash: 3f47cc571eff0c522aaf6543de58f158835384d4
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2017
---
# <a name="aspnet-core-built-in-tag-helpers"></a><span data-ttu-id="e4e73-104">ASP.NET Core 內建的標記協助程式</span><span class="sxs-lookup"><span data-stu-id="e4e73-104">ASP.NET Core built-in Tag Helpers</span></span>

<span data-ttu-id="e4e73-105">由 [Peter Kellner](http://peterkellner.net) 提供</span><span class="sxs-lookup"><span data-stu-id="e4e73-105">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="e4e73-106">ASP.NET Core 架構包含許多標記協助程式，可協助您更有效率地撰寫健全的程式碼。</span><span class="sxs-lookup"><span data-stu-id="e4e73-106">The ASP.NET Core framework includes many Tag Helpers that can help you be more productive in writing robust code.</span></span> <span data-ttu-id="e4e73-107">本節提供所有內建標記協助程式的概觀。</span><span class="sxs-lookup"><span data-stu-id="e4e73-107">This section provides an overview of all the built-in Tag Helpers.</span></span>

> [!NOTE]
> <span data-ttu-id="e4e73-108">有一些內建標記協助程式不會進行討論，因為它們是由 [Razor](xref:mvc/views/razor) 檢視引擎在內部使用。</span><span class="sxs-lookup"><span data-stu-id="e4e73-108">There are built-in Tag Helpers which are not discussed, since they are used internally by the [Razor](xref:mvc/views/razor) view engine.</span></span> <span data-ttu-id="e4e73-109">這包括 ~ 字元的標記協助程式，其可將範圍擴展到網站的根路徑。</span><span class="sxs-lookup"><span data-stu-id="e4e73-109">This includes a Tag Helper for the ~ character which expands to the root path of the web site.</span></span>

## <a name="built-in-aspnet-core-tag-helpers"></a><span data-ttu-id="e4e73-110">內建的 ASP.NET Core 標記協助程式</span><span class="sxs-lookup"><span data-stu-id="e4e73-110">Built-in ASP.NET Core Tag Helpers</span></span>

<span data-ttu-id="e4e73-111">**[錨點標記協助程式](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper)**</span><span class="sxs-lookup"><span data-stu-id="e4e73-111">**[Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper)**</span></span>

<span data-ttu-id="e4e73-112">**[快取標記協助程式](xref:mvc/views/tag-helpers/builtin-th/CacheTagHelper)**</span><span class="sxs-lookup"><span data-stu-id="e4e73-112">**[Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/CacheTagHelper)**</span></span>

<span data-ttu-id="e4e73-113">**[分散式快取標記協助程式](xref:mvc/views/tag-helpers/builtin-th/DistributedCacheTagHelper)**</span><span class="sxs-lookup"><span data-stu-id="e4e73-113">**[Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/DistributedCacheTagHelper)**</span></span>

<span data-ttu-id="e4e73-114">**[環境標記協助程式](xref:mvc/views/tag-helpers/builtin-th/EnvironmentTagHelper)**</span><span class="sxs-lookup"><span data-stu-id="e4e73-114">**[Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/EnvironmentTagHelper)**</span></span>

[comment]: **[FormActionTagHelper](xref:mvc/views/tag-helpers/builtin-th/FormActionTagHelper)**

[comment]: **[FormTagTagHelper](xref:mvc/views/tag-helpers/builtin-th/FormTagHelper)**

<span data-ttu-id="e4e73-115">**[影像標記協助程式](xref:mvc/views/tag-helpers/builtin-th/ImageTagHelper)**</span><span class="sxs-lookup"><span data-stu-id="e4e73-115">**[Image Tag Helper](xref:mvc/views/tag-helpers/builtin-th/ImageTagHelper)**</span></span>

[comment]: **[InputTagHelper](xref:mvc/views/tag-helpers/builtin-th/InputTagHelper)**

[comment]: **[LabelTagHelper](xref:mvc/views/tag-helpers/builtin-th/LabelTagHelper)**

[comment]: **[LinkTagHelper](xref:mvc/views/tag-helpers/builtin-th/LinkTagHelper)**

[comment]: **[OptionTagHelper](xref:mvc/views/tag-helpers/builtin-th/OptionTagHelper)**

[comment]: **[ScriptTagHelper](xref:mvc/views/tag-helpers/builtin-th/ScriptTagTagHelper)**

[comment]: **[SelectTagHelper](xref:mvc/views/tag-helpers/builtin-th/SelectTagTagHelper)**

[comment]: **[TextAreaTagHelper](xref:mvc/views/tag-helpers/builtin-th/TextAreaTagHelper)**

[comment]: **[ValidationMessageTagHelper](xref:mvc/views/tag-helpers/builtin-th/ValidationMessageTagHelper)**

[comment]: **[ValidationSummaryTagHelper](xref:mvc/views/tag-helpers/builtin-th/ValidationSummaryTagHelper)**  
  
  
<!--

## Additional Resources

REQUIRED These must be xref links, not relative, that is ../../
* [Client-Side Development](../../../client-side/index.md)

* [Tag Helpers](xref:mvc/views/tag-helpers/intro)
-->
