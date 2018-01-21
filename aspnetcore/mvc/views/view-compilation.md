---
title: "Razor 檢視編譯和先行編譯"
author: rick-anderson
description: "說明如何啟用 MVC Razor 檢視編譯和先行編譯 ASP.NET Core 應用程式中的參考文件。"
ms.author: riande
manager: wpickett
ms.date: 12/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/view-compilation
ms.openlocfilehash: 87989455c2fb6b5a922c7fb6133aa3e8cef42c88
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a><span data-ttu-id="05fa0-103">Razor 檢視編譯和先行編譯的 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="05fa0-103">Razor view compilation and precompilation in ASP.NET Core</span></span>

<span data-ttu-id="05fa0-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="05fa0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="05fa0-105">檢視叫用時，會在執行階段編譯 razor 檢視。</span><span class="sxs-lookup"><span data-stu-id="05fa0-105">Razor views are compiled at runtime when the view is invoked.</span></span> <span data-ttu-id="05fa0-106">ASP.NET 核心 1.1.0 和更新版本可以選擇性地編譯 Razor 檢視，以及將其部署與應用程式&mdash;稱為先行編譯的程序。</span><span class="sxs-lookup"><span data-stu-id="05fa0-106">ASP.NET Core 1.1.0 and higher can optionally compile Razor views and deploy them with the app&mdash;a process known as precompilation.</span></span> <span data-ttu-id="05fa0-107">ASP.NET Core 2.x 專案範本預設會啟用先行編譯。</span><span class="sxs-lookup"><span data-stu-id="05fa0-107">The ASP.NET Core 2.x project templates enable precompilation by default.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="05fa0-108">執行時，是目前無法使用 razor 檢視先行編譯[獨立的部署 (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) ASP.NET Core 2.0 中。</span><span class="sxs-lookup"><span data-stu-id="05fa0-108">Razor view precompilation is currently unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span> <span data-ttu-id="05fa0-109">2.1 版本時，此功能可供 Scd。</span><span class="sxs-lookup"><span data-stu-id="05fa0-109">The feature will be available for SCDs when 2.1 releases.</span></span> <span data-ttu-id="05fa0-110">如需詳細資訊，請參閱[檢視編譯會失敗，在 Windows 上的 Linux 交互編譯時](https://github.com/aspnet/MvcPrecompilation/issues/102)。</span><span class="sxs-lookup"><span data-stu-id="05fa0-110">For more information, see [View compilation fails when cross-compiling for Linux on Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).</span></span>

<span data-ttu-id="05fa0-111">先行編譯的考量：</span><span class="sxs-lookup"><span data-stu-id="05fa0-111">Precompilation considerations:</span></span>

* <span data-ttu-id="05fa0-112">先行編譯的檢視會導致較小的大小已發行的組合和更快的啟動時間。</span><span class="sxs-lookup"><span data-stu-id="05fa0-112">Precompiling views results in a smaller published bundle and faster startup time.</span></span>
* <span data-ttu-id="05fa0-113">您無法編輯 Razor 檔案之後您先行編譯的檢視。</span><span class="sxs-lookup"><span data-stu-id="05fa0-113">You can't edit Razor files after you precompile views.</span></span> <span data-ttu-id="05fa0-114">已編輯的檢視將不會出現在已發行的套件組合。</span><span class="sxs-lookup"><span data-stu-id="05fa0-114">The edited views won't be present in the published bundle.</span></span> 

<span data-ttu-id="05fa0-115">若要部署先行編譯的檢視：</span><span class="sxs-lookup"><span data-stu-id="05fa0-115">To deploy precompiled views:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="05fa0-116">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="05fa0-116">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="05fa0-117">如果您的專案以.NET Framework 為目標，包含的封裝參考[Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span><span class="sxs-lookup"><span data-stu-id="05fa0-117">If your project targets .NET Framework, include a package reference to [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

<span data-ttu-id="05fa0-118">如果您的專案的目標.NET Core，不不需要任何變更。</span><span class="sxs-lookup"><span data-stu-id="05fa0-118">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="05fa0-119">隱含地設定 ASP.NET Core 2.x 專案範本`MvcRazorCompileOnPublish`至`true`根據預設，這表示這個節點可以安全地移除從*.csproj*檔案。</span><span class="sxs-lookup"><span data-stu-id="05fa0-119">The ASP.NET Core 2.x project templates implicitly set `MvcRazorCompileOnPublish` to `true` by default, which means this node can be safely removed from the *.csproj* file.</span></span> <span data-ttu-id="05fa0-120">如果您想要明確，是在設定中的沒有壞處`MvcRazorCompileOnPublish`屬性`true`。</span><span class="sxs-lookup"><span data-stu-id="05fa0-120">If you prefer to be explicit, there's no harm in setting the `MvcRazorCompileOnPublish` property to `true`.</span></span> <span data-ttu-id="05fa0-121">下列*.csproj*範例強調這項設定：</span><span class="sxs-lookup"><span data-stu-id="05fa0-121">The following *.csproj* sample highlights this setting:</span></span>

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="05fa0-122">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="05fa0-122">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="05fa0-123">設定`MvcRazorCompileOnPublish`至`true`，和包含的封裝參考`Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`。</span><span class="sxs-lookup"><span data-stu-id="05fa0-123">Set `MvcRazorCompileOnPublish` to `true`, and include a package reference to `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span></span> <span data-ttu-id="05fa0-124">下列*.csproj*範例會反白顯示這些設定：</span><span class="sxs-lookup"><span data-stu-id="05fa0-124">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---

<span data-ttu-id="05fa0-125">準備適用於應用程式[framework 相依的部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)執行命令如下所示，在專案的根目錄：</span><span class="sxs-lookup"><span data-stu-id="05fa0-125">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) by executing a command such as the following at the project root:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="05fa0-126">A *< project_name >。PrecompiledViews.dll*當先行編譯成功，則會產生包含已編譯的 Razor 檢視的檔案。</span><span class="sxs-lookup"><span data-stu-id="05fa0-126">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor views, is produced when precompilation succeeds.</span></span> <span data-ttu-id="05fa0-127">例如，下列螢幕擷取畫面會描述的內容*Index.cshtml*內*WebApplication1.PrecompiledViews.dll*:</span><span class="sxs-lookup"><span data-stu-id="05fa0-127">For example, the screenshot below depicts the contents of *Index.cshtml* inside of *WebApplication1.PrecompiledViews.dll*:</span></span>

![DLL 內的 razor 檢視](view-compilation/_static/razor-views-in-dll.png)
