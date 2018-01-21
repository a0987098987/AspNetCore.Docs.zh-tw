---
title: "適用於 ASP.NET Core Microsoft.AspNetCore.All metapackage 2.x 及更新版本"
author: Rick-Anderson
description: "Microsoft.AspNetCore.All metapackage 包含所有支援的 ASP.NET Core 和 Entity Framework Core 套件，以及它們的相依性。"
ms.author: riande
manager: wpickett
ms.date: 09/20/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/metapackage
ms.openlocfilehash: 8a44ee7ebb7e6b0112000429f1f080bceb7dc895
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
#<a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-2x"></a><span data-ttu-id="ebb7c-103">適用於 ASP.NET Core Microsoft.AspNetCore.All metapackage 2.x</span><span class="sxs-lookup"><span data-stu-id="ebb7c-103">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.x</span></span>

<span data-ttu-id="ebb7c-104">這項功能需要 ASP.NET Core 2.x 目標.NET 核心 2.x。</span><span class="sxs-lookup"><span data-stu-id="ebb7c-104">This feature requires ASP.NET Core 2.x targeting .NET Core 2.x.</span></span>

<span data-ttu-id="ebb7c-105">ASP.NET Core 的 [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) 中繼套件包括：</span><span class="sxs-lookup"><span data-stu-id="ebb7c-105">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="ebb7c-106">所有由 ASP.NET Core 小組支援的套件。</span><span class="sxs-lookup"><span data-stu-id="ebb7c-106">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="ebb7c-107">所有由 Entity Framework Core 支援的套件。</span><span class="sxs-lookup"><span data-stu-id="ebb7c-107">All supported packages by the Entity Framework Core.</span></span> 
* <span data-ttu-id="ebb7c-108">ASP.NET Core 與 Entity Framework Core 所使用的內部與第三人相依性。</span><span class="sxs-lookup"><span data-stu-id="ebb7c-108">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span> 

<span data-ttu-id="ebb7c-109">ASP.NET Core 的所有功能 2.x 和 Entity Framework Core 2.x 包含在`Microsoft.AspNetCore.All`封裝。</span><span class="sxs-lookup"><span data-stu-id="ebb7c-109">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="ebb7c-110">預設的專案範本會使用這個封裝。</span><span class="sxs-lookup"><span data-stu-id="ebb7c-110">The default project templates use this package.</span></span>

<span data-ttu-id="ebb7c-111">版本號碼`Microsoft.AspNetCore.All`metapackage 表示的 ASP.NET Core 版本和 Entity Framework Core 版本 （.NET Core 版本與對齊）。</span><span class="sxs-lookup"><span data-stu-id="ebb7c-111">The version number of the `Microsoft.AspNetCore.All` metapackage represents the ASP.NET Core version and Entity Framework Core version (aligned with the .NET Core version).</span></span>

<span data-ttu-id="ebb7c-112">使用應用程式`Microsoft.AspNetCore.All`metapackage 自動利用[.NET 核心執行階段存放區](https://docs.microsoft.com/dotnet/core/deploying/runtime-store)。</span><span class="sxs-lookup"><span data-stu-id="ebb7c-112">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the [.NET Core Runtime Store](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="ebb7c-113">執行階段存放區包含執行 ASP.NET Core 2.x 應用程式所需的所有執行階段資產。</span><span class="sxs-lookup"><span data-stu-id="ebb7c-113">The Runtime Store contains all the runtime assets needed to run ASP.NET Core 2.x applications.</span></span> <span data-ttu-id="ebb7c-114">當您使用`Microsoft.AspNetCore.All`metapackage，**沒有**參考 ASP.NET Core NuGet 套件的資產會隨著應用程式部署&mdash;.NET 核心執行階段存放區包含這些資產。</span><span class="sxs-lookup"><span data-stu-id="ebb7c-114">When you use the `Microsoft.AspNetCore.All` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the .NET Core Runtime Store contains these assets.</span></span> <span data-ttu-id="ebb7c-115">在執行階段存放區中的資產中都會先行編譯為了改善應用程式啟動時間。</span><span class="sxs-lookup"><span data-stu-id="ebb7c-115">The assets in the Runtime Store are precompiled to improve application startup time.</span></span>

<span data-ttu-id="ebb7c-116">若要移除未使用的封裝，您可以使用封裝調整處理序。</span><span class="sxs-lookup"><span data-stu-id="ebb7c-116">You can use the package trimming process to remove packages that you don't use.</span></span> <span data-ttu-id="ebb7c-117">發行的應用程式的輸出中排除已修剪的封裝。</span><span class="sxs-lookup"><span data-stu-id="ebb7c-117">Trimmed packages are excluded in published application output.</span></span>

<span data-ttu-id="ebb7c-118">下列*.csproj*檔案參考`Microsoft.AspNetCore.All`適用於 ASP.NET Core metapackage:</span><span class="sxs-lookup"><span data-stu-id="ebb7c-118">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

[!code-xml[Main](..\mvc\views\view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=9)]
