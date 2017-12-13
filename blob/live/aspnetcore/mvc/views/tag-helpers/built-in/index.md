---
title: "ASP.NET Core 內建的標記協助程式"
author: pkellner
description: "ASP.NET Core 內建的標記協助程式"
keywords: "ASP.NET Core,標記協助程式"
ms.author: riande
manager: wpickett
ms.date: 09/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/Index
ms.openlocfilehash: dd732822a715df19c0ee4b6accad3455ad6537da
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-core-built-in-tag-helpers"></a><span data-ttu-id="ccda9-104">ASP.NET Core 內建的標記協助程式</span><span class="sxs-lookup"><span data-stu-id="ccda9-104">ASP.NET Core built-in Tag Helpers</span></span>

<span data-ttu-id="ccda9-105">由 [Peter Kellner](http://peterkellner.net) 提供</span><span class="sxs-lookup"><span data-stu-id="ccda9-105">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="ccda9-106">ASP.NET Core 包含許多內建標記協助程式，能夠提高您的產能。</span><span class="sxs-lookup"><span data-stu-id="ccda9-106">ASP.NET Core includes many built-in Tag Helpers to boost your productivity.</span></span> <span data-ttu-id="ccda9-107">本節提供內建標記協助程式的概觀。</span><span class="sxs-lookup"><span data-stu-id="ccda9-107">This section provides an overview of the built-in Tag Helpers.</span></span>

> [!NOTE]
> <span data-ttu-id="ccda9-108">有一些標記協助程式由 [Razor](xref:mvc/views/razor) 檢視引擎在內部使用，因此不會加以討論。</span><span class="sxs-lookup"><span data-stu-id="ccda9-108">There are built-in Tag Helpers which aren't discussed, since they're used internally by the [Razor](xref:mvc/views/razor) view engine.</span></span> <span data-ttu-id="ccda9-109">這包括 ~ 字元的標記協助程式，其可將範圍擴展到網站的根路徑。</span><span class="sxs-lookup"><span data-stu-id="ccda9-109">This includes a Tag Helper for the ~ character, which expands to the root path of the website.</span></span>

## <a name="built-in-aspnet-core-tag-helpers"></a><span data-ttu-id="ccda9-110">內建的 ASP.NET Core 標記協助程式</span><span class="sxs-lookup"><span data-stu-id="ccda9-110">Built-in ASP.NET Core Tag Helpers</span></span>

<span data-ttu-id="ccda9-111">**[錨點標記協助程式](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="ccda9-111">**[Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span></span>

<span data-ttu-id="ccda9-112">**[快取標記協助程式](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="ccda9-112">**[Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span></span>

<span data-ttu-id="ccda9-113">**[分散式快取標記協助程式](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="ccda9-113">**[Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span></span>

<span data-ttu-id="ccda9-114">**[環境標記協助程式](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="ccda9-114">**[Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span></span>

[comment]: **[FormActionTagHelper](xref:mvc/views/tag-helpers/builtin-th/form-action-tag-helper)**

<span data-ttu-id="ccda9-115">**[表單標記協助程式](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="ccda9-115">**[Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span></span>

<span data-ttu-id="ccda9-116">**[影像標記協助程式](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="ccda9-116">**[Image Tag Helper](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span></span>

<span data-ttu-id="ccda9-117">**[輸入標記協助程式](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="ccda9-117">**[Input Tag Helper](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span></span>

<span data-ttu-id="ccda9-118">**[標籤標記協助程式](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="ccda9-118">**[Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span></span>

[comment]: **[LinkTagHelper](xref:mvc/views/tag-helpers/builtin-th/link-tag-helper)**

[comment]: **[OptionTagHelper](xref:mvc/views/tag-helpers/builtin-th/option-tag-helper)**

[comment]: **[ScriptTagHelper](xref:mvc/views/tag-helpers/builtin-th/script-tag-helper)**

<span data-ttu-id="ccda9-119">**[選取標記協助程式](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="ccda9-119">**[Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span></span>

<span data-ttu-id="ccda9-120">**[Textarea 標記協助程式](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="ccda9-120">**[Textarea Tag Helper](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span></span>

<span data-ttu-id="ccda9-121">**[驗證訊息標記協助程式](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="ccda9-121">**[Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span></span>

<span data-ttu-id="ccda9-122">**[驗證摘要標記協助程式](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="ccda9-122">**[Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ccda9-123">其他資源</span><span class="sxs-lookup"><span data-stu-id="ccda9-123">Additional resources</span></span>

* [<span data-ttu-id="ccda9-124">用戶端開發</span><span class="sxs-lookup"><span data-stu-id="ccda9-124">Client-Side Development</span></span>](xref:client-side/index)
* [<span data-ttu-id="ccda9-125">標記協助程式</span><span class="sxs-lookup"><span data-stu-id="ccda9-125">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
