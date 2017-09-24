---
title: "Razor 檢視編譯和先行編譯"
author: rick-anderson
description: "說明如何啟用 MVC Razor 檢視編譯和先行編譯 ASP.NET Core 應用程式中的參考文件。"
keywords: "ASP.NET Core Razor 檢視編譯、 Razor 預先編譯、 Razor 先行編譯"
ms.author: riande
manager: wpickett
ms.date: 08/16/2017
ms.topic: article
ms.assetid: ab4705b7-1638-1638-bc97-ea7f292fe92a
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/view-compilation
ms.openlocfilehash: bfee2e5e8f71c99465be79589a77f0e173097b23
ms.sourcegitcommit: 8005eb4051e568d88ee58d48424f39916052e6e2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2017
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a><span data-ttu-id="865b3-104">Razor 檢視編譯和先行編譯的 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="865b3-104">Razor view compilation and precompilation in ASP.NET Core</span></span>

<span data-ttu-id="865b3-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="865b3-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="865b3-106">檢視叫用時，會在執行階段編譯 razor 檢視。</span><span class="sxs-lookup"><span data-stu-id="865b3-106">Razor views are compiled at runtime when the view is invoked.</span></span> <span data-ttu-id="865b3-107">ASP.NET 核心 1.1.0 和更新版本可以選擇性地編譯 Razor 檢視，以及將其部署與應用程式&mdash;稱為先行編譯的程序。</span><span class="sxs-lookup"><span data-stu-id="865b3-107">ASP.NET Core 1.1.0 and higher can optionally compile Razor views and deploy them with the app &mdash; a process known as precompilation.</span></span> <span data-ttu-id="865b3-108">ASP.NET Core 2.x 專案範本預設會啟用先行編譯。</span><span class="sxs-lookup"><span data-stu-id="865b3-108">The ASP.NET Core 2.x project templates enable precompilation by default.</span></span>

> [!NOTE]
> <span data-ttu-id="865b3-109">執行時，是目前無法使用 razor 檢視先行編譯[獨立的部署 (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) ASP.NET Core 2.0 中。</span><span class="sxs-lookup"><span data-stu-id="865b3-109">Razor view precompilation is currently unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span> <span data-ttu-id="865b3-110">2.1 版本時，此功能可供 Scd。</span><span class="sxs-lookup"><span data-stu-id="865b3-110">The feature will be available for SCDs when 2.1 releases.</span></span> <span data-ttu-id="865b3-111">如需詳細資訊，請參閱[檢視編譯會失敗，在 Windows 上的 Linux 交互編譯時](https://github.com/aspnet/MvcPrecompilation/issues/102)。</span><span class="sxs-lookup"><span data-stu-id="865b3-111">For more information, see [View compilation fails when cross-compiling for Linux on Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).</span></span>

<span data-ttu-id="865b3-112">先行編譯的考量：</span><span class="sxs-lookup"><span data-stu-id="865b3-112">Precompilation considerations:</span></span>

* <span data-ttu-id="865b3-113">先行編譯的檢視會導致較小的大小已發行的組合和更快的啟動時間。</span><span class="sxs-lookup"><span data-stu-id="865b3-113">Precompiling views results in a smaller published bundle and faster startup time.</span></span>
* <span data-ttu-id="865b3-114">您無法編輯 Razor 檔案之後您先行編譯的檢視。</span><span class="sxs-lookup"><span data-stu-id="865b3-114">You can't edit Razor files after you precompile views.</span></span> <span data-ttu-id="865b3-115">已編輯的檢視將不會出現在已發行的套件組合。</span><span class="sxs-lookup"><span data-stu-id="865b3-115">The edited views won't be present in the published bundle.</span></span> 

<span data-ttu-id="865b3-116">若要部署先行編譯的檢視：</span><span class="sxs-lookup"><span data-stu-id="865b3-116">To deploy precompiled views:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="865b3-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="865b3-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="865b3-118">如果您的專案以.NET Framework 為目標，包含的封裝參考`Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`:</span><span class="sxs-lookup"><span data-stu-id="865b3-118">If your project targets .NET Framework, include a package reference to `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

<span data-ttu-id="865b3-119">如果您的專案的目標.NET Core，不不需要任何變更。</span><span class="sxs-lookup"><span data-stu-id="865b3-119">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="865b3-120">隱含地設定 ASP.NET Core 2.x 專案範本`MvcRazorCompileOnPublish`至`true`根據預設，這表示這個節點可以安全地移除從*.csproj*檔案。</span><span class="sxs-lookup"><span data-stu-id="865b3-120">The ASP.NET Core 2.x project templates implicitly set `MvcRazorCompileOnPublish` to `true` by default, which means this node can be safely removed from the *.csproj* file.</span></span> <span data-ttu-id="865b3-121">如果您想要明確，是在設定中的沒有壞處`MvcRazorCompileOnPublish`屬性`true`。</span><span class="sxs-lookup"><span data-stu-id="865b3-121">If you prefer to be explicit, there's no harm in setting the `MvcRazorCompileOnPublish` property to `true`.</span></span> <span data-ttu-id="865b3-122">下列*.csproj*範例強調這項設定：</span><span class="sxs-lookup"><span data-stu-id="865b3-122">The following *.csproj* sample highlights this setting:</span></span>

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="865b3-123">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="865b3-123">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="865b3-124">設定`MvcRazorCompileOnPublish`至`true`，和包含的封裝參考`Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`。</span><span class="sxs-lookup"><span data-stu-id="865b3-124">Set `MvcRazorCompileOnPublish` to `true`, and include a package reference to `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span></span> <span data-ttu-id="865b3-125">下列*.csproj*範例會反白顯示這些設定：</span><span class="sxs-lookup"><span data-stu-id="865b3-125">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---
