---
title: "ASP.NET Core 內建的標記協助程式"
author: pkellner
description: "ASP.NET Core 內建的標記協助程式"
manager: wpickett
ms.author: riande
ms.date: 09/13/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/Index
ms.openlocfilehash: 09024bf0d6c87fce9eba9b70bebefa11d2ff0a44
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="aspnet-core-built-in-tag-helpers"></a><span data-ttu-id="900bd-103">ASP.NET Core 內建的標記協助程式</span><span class="sxs-lookup"><span data-stu-id="900bd-103">ASP.NET Core built-in Tag Helpers</span></span>

<span data-ttu-id="900bd-104">由 [Peter Kellner](http://peterkellner.net) 提供</span><span class="sxs-lookup"><span data-stu-id="900bd-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="900bd-105">ASP.NET Core 包含許多內建標記協助程式，能夠提高您的產能。</span><span class="sxs-lookup"><span data-stu-id="900bd-105">ASP.NET Core includes many built-in Tag Helpers to boost your productivity.</span></span> <span data-ttu-id="900bd-106">本節提供內建標記協助程式的概觀。</span><span class="sxs-lookup"><span data-stu-id="900bd-106">This section provides an overview of the built-in Tag Helpers.</span></span>

> [!NOTE]
> <span data-ttu-id="900bd-107">有一些標記協助程式由 [Razor](xref:mvc/views/razor) 檢視引擎在內部使用，因此不會加以討論。</span><span class="sxs-lookup"><span data-stu-id="900bd-107">There are built-in Tag Helpers which aren't discussed, since they're used internally by the [Razor](xref:mvc/views/razor) view engine.</span></span> <span data-ttu-id="900bd-108">這包括 ~ 字元的標記協助程式，其可將範圍擴展到網站的根路徑。</span><span class="sxs-lookup"><span data-stu-id="900bd-108">This includes a Tag Helper for the ~ character, which expands to the root path of the website.</span></span>

## <a name="built-in-aspnet-core-tag-helpers"></a><span data-ttu-id="900bd-109">內建的 ASP.NET Core 標記協助程式</span><span class="sxs-lookup"><span data-stu-id="900bd-109">Built-in ASP.NET Core Tag Helpers</span></span>

<span data-ttu-id="900bd-110">**[錨點標記協助程式](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="900bd-110">**[Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span></span>

<span data-ttu-id="900bd-111">**[快取標記協助程式](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="900bd-111">**[Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span></span>

<span data-ttu-id="900bd-112">**[分散式快取標記協助程式](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="900bd-112">**[Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span></span>

<span data-ttu-id="900bd-113">**[環境標記協助程式](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="900bd-113">**[Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span></span>

[comment]: **[FormActionTagHelper](xref:mvc/views/tag-helpers/builtin-th/form-action-tag-helper)**

<span data-ttu-id="900bd-114">**[表單標記協助程式](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="900bd-114">**[Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span></span>

<span data-ttu-id="900bd-115">**[影像標記協助程式](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="900bd-115">**[Image Tag Helper](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span></span>

<span data-ttu-id="900bd-116">**[輸入標記協助程式](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="900bd-116">**[Input Tag Helper](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span></span>

<span data-ttu-id="900bd-117">**[標籤標記協助程式](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="900bd-117">**[Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span></span>

[comment]: **[LinkTagHelper](xref:mvc/views/tag-helpers/builtin-th/link-tag-helper)**

[comment]: **[OptionTagHelper](xref:mvc/views/tag-helpers/builtin-th/option-tag-helper)**

[comment]: **[ScriptTagHelper](xref:mvc/views/tag-helpers/builtin-th/script-tag-helper)**

<span data-ttu-id="900bd-118">**[選取標記協助程式](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="900bd-118">**[Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span></span>

<span data-ttu-id="900bd-119">**[Textarea 標記協助程式](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="900bd-119">**[Textarea Tag Helper](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span></span>

<span data-ttu-id="900bd-120">**[驗證訊息標記協助程式](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="900bd-120">**[Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span></span>

<span data-ttu-id="900bd-121">**[驗證摘要標記協助程式](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="900bd-121">**[Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span></span>

## <a name="additional-resources"></a><span data-ttu-id="900bd-122">其他資源</span><span class="sxs-lookup"><span data-stu-id="900bd-122">Additional resources</span></span>

* [<span data-ttu-id="900bd-123">用戶端開發</span><span class="sxs-lookup"><span data-stu-id="900bd-123">Client-Side Development</span></span>](xref:client-side/index)
* [<span data-ttu-id="900bd-124">標記協助程式</span><span class="sxs-lookup"><span data-stu-id="900bd-124">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
