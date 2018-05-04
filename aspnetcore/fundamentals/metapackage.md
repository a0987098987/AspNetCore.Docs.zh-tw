---
title: ASP.NET Core 2.x 和更新版本的 Microsoft.AspNetCore.All 中繼套件
author: Rick-Anderson
description: Microsoft.AspNetCore.All 中繼套件包含所有支援的 ASP.NET Core 和 Entity Framework Core 套件，以及它們的相依性。
manager: wpickett
monikerRange: = aspnetcore-2.0
ms.author: riande
ms.date: 09/20/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/metapackage
ms.openlocfilehash: 4c11f15e659565325bfe8b8d91188b62177b251d
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/18/2018
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-2x"></a><span data-ttu-id="a381e-103">ASP.NET Core 2.x 的 Microsoft.AspNetCore.All 中繼套件</span><span class="sxs-lookup"><span data-stu-id="a381e-103">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.x</span></span>

<span data-ttu-id="a381e-104">這項功能需要以 .NET Core 2.x 為目標的 ASP.NET Core 2.x。</span><span class="sxs-lookup"><span data-stu-id="a381e-104">This feature requires ASP.NET Core 2.x targeting .NET Core 2.x.</span></span>

<span data-ttu-id="a381e-105">ASP.NET Core 的 [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) 中繼套件包括：</span><span class="sxs-lookup"><span data-stu-id="a381e-105">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="a381e-106">所有由 ASP.NET Core 小組支援的套件。</span><span class="sxs-lookup"><span data-stu-id="a381e-106">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="a381e-107">所有由 Entity Framework Core 支援的套件。</span><span class="sxs-lookup"><span data-stu-id="a381e-107">All supported packages by the Entity Framework Core.</span></span> 
* <span data-ttu-id="a381e-108">ASP.NET Core 與 Entity Framework Core 所使用的內部與第三人相依性。</span><span class="sxs-lookup"><span data-stu-id="a381e-108">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span> 

<span data-ttu-id="a381e-109">`Microsoft.AspNetCore.All` 套件包含 ASP.NET Core 2.x 和 Entity Framework Core 2.x 的所有功能。</span><span class="sxs-lookup"><span data-stu-id="a381e-109">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="a381e-110">以 ASP.NET Core 2.0 為目標的預設專案範本會使用此套件。</span><span class="sxs-lookup"><span data-stu-id="a381e-110">The default project templates targeting ASP.NET Core 2.0 use this package.</span></span>

<span data-ttu-id="a381e-111">`Microsoft.AspNetCore.All`　中繼套件的版本號碼代表 ASP.NET Core 版本和 Entity Framework Core 版本 (與 .NET Core 版本一致)。</span><span class="sxs-lookup"><span data-stu-id="a381e-111">The version number of the `Microsoft.AspNetCore.All` metapackage represents the ASP.NET Core version and Entity Framework Core version (aligned with the .NET Core version).</span></span>

<span data-ttu-id="a381e-112">使用 `Microsoft.AspNetCore.All` 中繼套件的應用程式會自動利用 [.NET Core 執行階段存放區](https://docs.microsoft.com/dotnet/core/deploying/runtime-store)。</span><span class="sxs-lookup"><span data-stu-id="a381e-112">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the [.NET Core Runtime Store](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="a381e-113">此執行階段存放區包含執行 ASP.NET Core 2.x 應用程式所需的所有執行階段資產。</span><span class="sxs-lookup"><span data-stu-id="a381e-113">The Runtime Store contains all the runtime assets needed to run ASP.NET Core 2.x applications.</span></span> <span data-ttu-id="a381e-114">當您使用 `Microsoft.AspNetCore.All` 中繼套件時，**不會**使用應用程式部署所參考之 ASP.NET Core NuGet 套件的任何資產 &mdash; .NET Core 執行階段存放區包含這些資產。</span><span class="sxs-lookup"><span data-stu-id="a381e-114">When you use the `Microsoft.AspNetCore.All` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the .NET Core Runtime Store contains these assets.</span></span> <span data-ttu-id="a381e-115">執行階段存放區中的資產會先行編譯，以改善應用程式啟動時間。</span><span class="sxs-lookup"><span data-stu-id="a381e-115">The assets in the Runtime Store are precompiled to improve application startup time.</span></span>

<span data-ttu-id="a381e-116">您可以使用套件修剪流程來移除未使用的套件。</span><span class="sxs-lookup"><span data-stu-id="a381e-116">You can use the package trimming process to remove packages that you don't use.</span></span> <span data-ttu-id="a381e-117">發行的應用程式輸出中會排除已修剪的套件。</span><span class="sxs-lookup"><span data-stu-id="a381e-117">Trimmed packages are excluded in published application output.</span></span>

<span data-ttu-id="a381e-118">下列 *.csproj* 檔案參考 ASP.NET Core 的 `Microsoft.AspNetCore.All` 中繼套件：</span><span class="sxs-lookup"><span data-stu-id="a381e-118">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

[!code-xml[](../mvc/views/view-compilation/sample/MvcRazorCompileOnPublish2.csproj?highlight=9)]
