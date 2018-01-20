---
title: "ASP.NET Core 內建的標記協助程式"
author: pkellner
description: "ASP.NET Core 內建的標記協助程式"
ms.author: riande
manager: wpickett
ms.date: 09/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/Index
ms.openlocfilehash: 8ffc748ec3d4eed35871543f5ceccc86aadee661
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="aspnet-core-built-in-tag-helpers"></a><span data-ttu-id="50640-103">ASP.NET Core 內建的標記協助程式</span><span class="sxs-lookup"><span data-stu-id="50640-103">ASP.NET Core built-in Tag Helpers</span></span>

<span data-ttu-id="50640-104">由 [Peter Kellner](http://peterkellner.net) 提供</span><span class="sxs-lookup"><span data-stu-id="50640-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="50640-105">ASP.NET Core 包含許多內建標記協助程式，能夠提高您的產能。</span><span class="sxs-lookup"><span data-stu-id="50640-105">ASP.NET Core includes many built-in Tag Helpers to boost your productivity.</span></span> <span data-ttu-id="50640-106">本節提供內建標記協助程式的概觀。</span><span class="sxs-lookup"><span data-stu-id="50640-106">This section provides an overview of the built-in Tag Helpers.</span></span>

> [!NOTE]
> <span data-ttu-id="50640-107">有一些標記協助程式由 [Razor](xref:mvc/views/razor) 檢視引擎在內部使用，因此不會加以討論。</span><span class="sxs-lookup"><span data-stu-id="50640-107">There are built-in Tag Helpers which aren't discussed, since they're used internally by the [Razor](xref:mvc/views/razor) view engine.</span></span> <span data-ttu-id="50640-108">這包括 ~ 字元的標記協助程式，其可將範圍擴展到網站的根路徑。</span><span class="sxs-lookup"><span data-stu-id="50640-108">This includes a Tag Helper for the ~ character, which expands to the root path of the website.</span></span>

## <a name="built-in-aspnet-core-tag-helpers"></a><span data-ttu-id="50640-109">內建的 ASP.NET Core 標記協助程式</span><span class="sxs-lookup"><span data-stu-id="50640-109">Built-in ASP.NET Core Tag Helpers</span></span>

<span data-ttu-id="50640-110">**[錨點標記協助程式](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="50640-110">**[Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span></span>

<span data-ttu-id="50640-111">**[快取標記協助程式](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="50640-111">**[Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span></span>

<span data-ttu-id="50640-112">**[分散式快取標記協助程式](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="50640-112">**[Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span></span>

<span data-ttu-id="50640-113">**[環境標記協助程式](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="50640-113">**[Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span></span>

[comment]: **[FormActionTagHelper](xref:mvc/views/tag-helpers/builtin-th/form-action-tag-helper)**

<span data-ttu-id="50640-114">**[表單標記協助程式](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="50640-114">**[Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span></span>

<span data-ttu-id="50640-115">**[影像標記協助程式](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="50640-115">**[Image Tag Helper](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span></span>

<span data-ttu-id="50640-116">**[輸入標記協助程式](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="50640-116">**[Input Tag Helper](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span></span>

<span data-ttu-id="50640-117">**[標籤標記協助程式](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="50640-117">**[Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span></span>

[comment]: **[LinkTagHelper](xref:mvc/views/tag-helpers/builtin-th/link-tag-helper)**

[comment]: **[OptionTagHelper](xref:mvc/views/tag-helpers/builtin-th/option-tag-helper)**

[comment]: **[ScriptTagHelper](xref:mvc/views/tag-helpers/builtin-th/script-tag-helper)**

<span data-ttu-id="50640-118">**[選取標記協助程式](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="50640-118">**[Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span></span>

<span data-ttu-id="50640-119">**[Textarea 標記協助程式](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="50640-119">**[Textarea Tag Helper](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span></span>

<span data-ttu-id="50640-120">**[驗證訊息標記協助程式](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="50640-120">**[Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span></span>

<span data-ttu-id="50640-121">**[驗證摘要標記協助程式](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="50640-121">**[Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span></span>

## <a name="additional-resources"></a><span data-ttu-id="50640-122">其他資源</span><span class="sxs-lookup"><span data-stu-id="50640-122">Additional resources</span></span>

* [<span data-ttu-id="50640-123">用戶端開發</span><span class="sxs-lookup"><span data-stu-id="50640-123">Client-Side Development</span></span>](xref:client-side/index)
* [<span data-ttu-id="50640-124">標記協助程式</span><span class="sxs-lookup"><span data-stu-id="50640-124">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
