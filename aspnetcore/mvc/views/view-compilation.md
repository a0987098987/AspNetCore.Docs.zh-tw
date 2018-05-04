---
title: ASP.NET Core 中的 Razor 檢視編譯和先行編譯
author: rick-anderson
description: 了解如何啟用 ASP.NET Core 應用程式中的 MVC Razor 檢視編譯和先行編譯。
manager: wpickett
ms.author: riande
ms.date: 12/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-compilation
ms.openlocfilehash: 5d971645106a79497a9902063c7774dc6d546395
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a><span data-ttu-id="42e16-103">ASP.NET Core 中的 Razor 檢視編譯和先行編譯</span><span class="sxs-lookup"><span data-stu-id="42e16-103">Razor view compilation and precompilation in ASP.NET Core</span></span>

<span data-ttu-id="42e16-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="42e16-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="42e16-105">叫用檢視時，會在執行階段編譯 Razor 檢視。</span><span class="sxs-lookup"><span data-stu-id="42e16-105">Razor views are compiled at runtime when the view is invoked.</span></span> <span data-ttu-id="42e16-106">ASP.NET Core 1.1.0 和更新版本可以選擇性地編譯 Razor 檢視，以及使用應用程式進行部署&mdash;稱為先行編譯的程序。</span><span class="sxs-lookup"><span data-stu-id="42e16-106">ASP.NET Core 1.1.0 and higher can optionally compile Razor views and deploy them with the app&mdash;a process known as precompilation.</span></span> <span data-ttu-id="42e16-107">ASP.NET Core 2.x 專案範本預設會啟用先行編譯。</span><span class="sxs-lookup"><span data-stu-id="42e16-107">The ASP.NET Core 2.x project templates enable precompilation by default.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="42e16-108">在 ASP.NET Core 2.0 中執行[獨立式部署 (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) 時，目前無法提供 Razor 檢視先行編譯。</span><span class="sxs-lookup"><span data-stu-id="42e16-108">Razor view precompilation is currently unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span> <span data-ttu-id="42e16-109">2.1 版時，此功能可供 SCD 使用。</span><span class="sxs-lookup"><span data-stu-id="42e16-109">The feature will be available for SCDs when 2.1 releases.</span></span> <span data-ttu-id="42e16-110">如需詳細資訊，請參閱[交叉編譯 Linux on Windows 時檢視編譯失敗](https://github.com/aspnet/MvcPrecompilation/issues/102)。</span><span class="sxs-lookup"><span data-stu-id="42e16-110">For more information, see [View compilation fails when cross-compiling for Linux on Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).</span></span>

<span data-ttu-id="42e16-111">先行編譯考量：</span><span class="sxs-lookup"><span data-stu-id="42e16-111">Precompilation considerations:</span></span>

* <span data-ttu-id="42e16-112">先行編譯檢視會導致較小的已發行套件組合以及更快的啟動時間。</span><span class="sxs-lookup"><span data-stu-id="42e16-112">Precompiling views results in a smaller published bundle and faster startup time.</span></span>
* <span data-ttu-id="42e16-113">在您先行編譯檢視之後，就無法編輯 Razor 檔案。</span><span class="sxs-lookup"><span data-stu-id="42e16-113">You can't edit Razor files after you precompile views.</span></span> <span data-ttu-id="42e16-114">編輯過的檢視將不會出現在已發行的套件組合中。</span><span class="sxs-lookup"><span data-stu-id="42e16-114">The edited views won't be present in the published bundle.</span></span> 

<span data-ttu-id="42e16-115">部署先行編譯的檢視：</span><span class="sxs-lookup"><span data-stu-id="42e16-115">To deploy precompiled views:</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="42e16-116">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="42e16-116">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
<span data-ttu-id="42e16-117">如果您專案的目標設為 .NET Framework，請包含 [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) 的套件參考：</span><span class="sxs-lookup"><span data-stu-id="42e16-117">If your project targets .NET Framework, include a package reference to [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

<span data-ttu-id="42e16-118">如果您專案的目標設為 .NET Core，則不需要進行任何變更。</span><span class="sxs-lookup"><span data-stu-id="42e16-118">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="42e16-119">ASP.NET Core 2.x 專案範本預設會將 `MvcRazorCompileOnPublish` 隱含地設定為 `true`，這表示可以從 *.csproj* 檔案安全地移除此節點。</span><span class="sxs-lookup"><span data-stu-id="42e16-119">The ASP.NET Core 2.x project templates implicitly set `MvcRazorCompileOnPublish` to `true` by default, which means this node can be safely removed from the *.csproj* file.</span></span> <span data-ttu-id="42e16-120">如果您想要更為明確，則請將 `MvcRazorCompileOnPublish` 屬性設定為 `true`。</span><span class="sxs-lookup"><span data-stu-id="42e16-120">If you prefer to be explicit, there's no harm in setting the `MvcRazorCompileOnPublish` property to `true`.</span></span> <span data-ttu-id="42e16-121">下列 *.csproj* 範例會反白顯示此設定：</span><span class="sxs-lookup"><span data-stu-id="42e16-121">The following *.csproj* sample highlights this setting:</span></span>

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish2.csproj?highlight=5)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="42e16-122">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="42e16-122">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
<span data-ttu-id="42e16-123">將 `MvcRazorCompileOnPublish` 設定為 `true`，並包括 `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation` 的套件參考。</span><span class="sxs-lookup"><span data-stu-id="42e16-123">Set `MvcRazorCompileOnPublish` to `true`, and include a package reference to `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span></span> <span data-ttu-id="42e16-124">下列 *.csproj* 範例會反白顯示這些設定：</span><span class="sxs-lookup"><span data-stu-id="42e16-124">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=5,12)]

<span data-ttu-id="42e16-125">使用 [.NET Core CLI 發行命令](/dotnet/core/tools/dotnet-publish)針對[框架相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)準備該應用程式。</span><span class="sxs-lookup"><span data-stu-id="42e16-125">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) with the [.NET Core CLI publish command](/dotnet/core/tools/dotnet-publish).</span></span> <span data-ttu-id="42e16-126">例如，在專案的根目錄下執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="42e16-126">For example, execute the following command at the project root:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="42e16-127">先行編譯成功時，會產生 <專案名稱>.PrecompiledViews.dll 檔案 (包含編譯過的 Razor 檢視)。</span><span class="sxs-lookup"><span data-stu-id="42e16-127">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor views, is produced when precompilation succeeds.</span></span> <span data-ttu-id="42e16-128">例如，下列螢幕擷取畫面描述 *WebApplication1.PrecompiledViews.dll* 內 *Index.cshtml* 的內容：</span><span class="sxs-lookup"><span data-stu-id="42e16-128">For example, the screenshot below depicts the contents of *Index.cshtml* inside of *WebApplication1.PrecompiledViews.dll*:</span></span>

![DLL 內部的 Razor 檢視](view-compilation/_static/razor-views-in-dll.png)
