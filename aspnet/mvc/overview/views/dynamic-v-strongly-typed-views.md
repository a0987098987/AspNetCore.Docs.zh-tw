---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: 動態 v。 強型別檢視 |Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/27/2011
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: bd00fccc44019b2e401247de1532d2abcb5dd396
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578495"
---
<a name="dynamic-v-strongly-typed-views"></a><span data-ttu-id="70ab0-103">動態 v。</span><span class="sxs-lookup"><span data-stu-id="70ab0-103">Dynamic v.</span></span> <span data-ttu-id="70ab0-104">強型別的檢視</span><span class="sxs-lookup"><span data-stu-id="70ab0-104">Strongly Typed Views</span></span>
====================
<span data-ttu-id="70ab0-105">藉由[Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="70ab0-105">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

<span data-ttu-id="70ab0-106">有三種方式可將資訊從控制器傳遞至 ASP.NET MVC 3 中的檢視：</span><span class="sxs-lookup"><span data-stu-id="70ab0-106">There are three ways to pass information from a controller to a view in ASP.NET MVC 3:</span></span>

1. <span data-ttu-id="70ab0-107">為強型別的模型物件。</span><span class="sxs-lookup"><span data-stu-id="70ab0-107">As a strongly typed model object.</span></span>
2. <span data-ttu-id="70ab0-108">做為動態類型 (使用@model動態)</span><span class="sxs-lookup"><span data-stu-id="70ab0-108">As a dynamic type (using @model dynamic)</span></span>
3. <span data-ttu-id="70ab0-109">使用 ViewBag</span><span class="sxs-lookup"><span data-stu-id="70ab0-109">Using the ViewBag</span></span>

<span data-ttu-id="70ab0-110">我寫了一個簡單的 MVC 3 頂端部落格應用程式比較與對照動態與強型別檢視。</span><span class="sxs-lookup"><span data-stu-id="70ab0-110">I've written a simple MVC 3 Top Blog application to compare and contrast dynamic and strongly typed views.</span></span> <span data-ttu-id="70ab0-111">在控制器啟動簡單的部落格清單：</span><span class="sxs-lookup"><span data-stu-id="70ab0-111">The controller starts out with a simple list of blogs:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

<span data-ttu-id="70ab0-112">以滑鼠右鍵按一下 IndexNotStonglyTyped() 方法中，並新增 Razor 檢視。</span><span class="sxs-lookup"><span data-stu-id="70ab0-112">Right click in the IndexNotStonglyTyped() method and add a Razor view.</span></span>

<span data-ttu-id="70ab0-113">[![8475.NotStronglyTypedView [1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="70ab0-113">[![8475.NotStronglyTypedView[1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span></span>

<span data-ttu-id="70ab0-114">請確定**建立強型別檢視**未核取方塊。</span><span class="sxs-lookup"><span data-stu-id="70ab0-114">Make sure the **Create a strongly-typed view** box is not checked.</span></span> <span data-ttu-id="70ab0-115">產生的檢視不包含許多：</span><span class="sxs-lookup"><span data-stu-id="70ab0-115">The resulting view doesn't contain much:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

<span data-ttu-id="70ab0-116">我們會使用動態並不是強型別的檢視，因為 intellisense 沒有幫助我們。</span><span class="sxs-lookup"><span data-stu-id="70ab0-116">Because we're using a dynamic and not a strongly typed view, intellisense doesn't help us.</span></span> <span data-ttu-id="70ab0-117">已完成的程式碼如下所示：</span><span class="sxs-lookup"><span data-stu-id="70ab0-117">The completed code is shown below:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

<span data-ttu-id="70ab0-118">[![6646.NotStronglyTypedView_5F00_IE [1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="70ab0-118">[![6646.NotStronglyTypedView_5F00_IE[1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span></span>

<span data-ttu-id="70ab0-119">現在我們將新增的強型別的檢視。</span><span class="sxs-lookup"><span data-stu-id="70ab0-119">Now we'll add a strongly typed view.</span></span> <span data-ttu-id="70ab0-120">將下列程式碼加入至控制器：</span><span class="sxs-lookup"><span data-stu-id="70ab0-120">Add the following code to the controller:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]


<span data-ttu-id="70ab0-121">請注意它是完全相同傳回 View(topBlogs);呼叫非強型別檢視。</span><span class="sxs-lookup"><span data-stu-id="70ab0-121">Notice it's exactly the same return View(topBlogs); call as the non-strongly typed view.</span></span> <span data-ttu-id="70ab0-122">以滑鼠右鍵按一下內*StonglyTypedIndex()* ，然後選取**加入檢視**。</span><span class="sxs-lookup"><span data-stu-id="70ab0-122">Right click inside of *StonglyTypedIndex()* and select **Add View**.</span></span> <span data-ttu-id="70ab0-123">這次請選取**部落格**模型類別，然後選取**清單**為 Scaffold 範本。</span><span class="sxs-lookup"><span data-stu-id="70ab0-123">This time select the **Blog** Model class and select **List** as the Scaffold template.</span></span>

<span data-ttu-id="70ab0-124">[![5658.StrongView [1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="70ab0-124">[![5658.StrongView[1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span></span>

<span data-ttu-id="70ab0-125">在新的檢視範本內，我們會取得 intellisense 支援。</span><span class="sxs-lookup"><span data-stu-id="70ab0-125">Inside the new view template we get intellisense support.</span></span>

<span data-ttu-id="70ab0-126">[![7002.intellesince [1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="70ab0-126">[![7002.intellesince[1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span></span>

<span data-ttu-id="70ab0-127">C# 專案可下載[此處](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip)。</span><span class="sxs-lookup"><span data-stu-id="70ab0-127">The c# project can be downloaded [here](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span></span>
