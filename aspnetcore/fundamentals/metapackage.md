---
title: "適用於 ASP.NET Core Microsoft.AspNetCore.All metapackage 2.x 及更新版本"
author: Rick-Anderson
description: "Microsoft.AspNetCore.All metapackage 包含所有支援的 ASP.NET Core 和 Entity Framework Core 套件，以及它們的相依性。"
keywords: ASP.NET Core,NuGet,package,Microsoft.AspNetCore.All,metapackage
ms.author: riande
manager: wpickett
ms.date: 09/20/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/metapackage
ms.openlocfilehash: 23a07867874eb534c75c4e7b3be00c4a376f8a8b
ms.sourcegitcommit: 4e45fd4e3f1374cd51cc931cee93c9d72631d9fc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/20/2017
---
#<a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-2x"></a><span data-ttu-id="4c894-104">適用於 ASP.NET Core Microsoft.AspNetCore.All metapackage 2.x</span><span class="sxs-lookup"><span data-stu-id="4c894-104">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.x</span></span>

<span data-ttu-id="4c894-105">這項功能需要 ASP.NET Core 2.x。</span><span class="sxs-lookup"><span data-stu-id="4c894-105">This feature requires ASP.NET Core 2.x.</span></span>

<span data-ttu-id="4c894-106">[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All)適用於 ASP.NET Core metapackage 包括：</span><span class="sxs-lookup"><span data-stu-id="4c894-106">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="4c894-107">由 ASP.NET Core 小組支援所有封裝。</span><span class="sxs-lookup"><span data-stu-id="4c894-107">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="4c894-108">所有支援 Entity Framework Core 的套件。</span><span class="sxs-lookup"><span data-stu-id="4c894-108">All supported packages by the Entity Framework Core.</span></span> 
* <span data-ttu-id="4c894-109">內部和第 3 合作對象的相依性由 ASP.NET Core 和 Entity Framework Core。</span><span class="sxs-lookup"><span data-stu-id="4c894-109">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span> 

<span data-ttu-id="4c894-110">ASP.NET Core 的所有功能 2.x 和 Entity Framework Core 2.x 包含在`Microsoft.AspNetCore.All`封裝。</span><span class="sxs-lookup"><span data-stu-id="4c894-110">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="4c894-111">預設的專案範本會使用這個封裝。</span><span class="sxs-lookup"><span data-stu-id="4c894-111">The default project templates use this package.</span></span>

<span data-ttu-id="4c894-112">版本號碼`Microsoft.AspNetCore.All`metapackage 表示的 ASP.NET Core 版本和 Entity Framework Core 版本 （.NET Core 版本與對齊）。</span><span class="sxs-lookup"><span data-stu-id="4c894-112">The version number of the `Microsoft.AspNetCore.All` metapackage represents the ASP.NET Core version and Entity Framework Core version (aligned with the .NET Core version).</span></span>

<span data-ttu-id="4c894-113">使用應用程式`Microsoft.AspNetCore.All`metapackage 自動利用[.NET 核心執行階段存放區](https://docs.microsoft.com/dotnet/core/deploying/runtime-store)。</span><span class="sxs-lookup"><span data-stu-id="4c894-113">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the [.NET Core Runtime Store](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="4c894-114">執行階段存放區包含執行 ASP.NET Core 2.x 應用程式所需的所有執行階段資產。</span><span class="sxs-lookup"><span data-stu-id="4c894-114">The Runtime Store contains all the runtime assets needed to run ASP.NET Core 2.x applications.</span></span> <span data-ttu-id="4c894-115">當您使用`Microsoft.AspNetCore.All`metapackage，**沒有**參考 ASP.NET Core NuGet 套件的資產會隨著應用程式部署&mdash;.NET 核心執行階段存放區包含這些資產。</span><span class="sxs-lookup"><span data-stu-id="4c894-115">When you use the `Microsoft.AspNetCore.All` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the .NET Core Runtime Store contains these assets.</span></span> <span data-ttu-id="4c894-116">在執行階段存放區中的資產中都會先行編譯為了改善應用程式啟動時間。</span><span class="sxs-lookup"><span data-stu-id="4c894-116">The assets in the Runtime Store are precompiled to improve application startup time.</span></span>

<span data-ttu-id="4c894-117">若要移除未使用的封裝，您可以使用封裝調整處理序。</span><span class="sxs-lookup"><span data-stu-id="4c894-117">You can use the package trimming process to remove packages that you don't use.</span></span> <span data-ttu-id="4c894-118">發行的應用程式的輸出中排除已修剪的封裝。</span><span class="sxs-lookup"><span data-stu-id="4c894-118">Trimmed packages are excluded in published application output.</span></span>

<span data-ttu-id="4c894-119">下列*.csproj*檔案參考`Microsoft.AspNetCore.All`適用於 ASP.NET Core metapackage:</span><span class="sxs-lookup"><span data-stu-id="4c894-119">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

[!code-xml[Main](..\mvc\views\view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=9)]
