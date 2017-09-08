---
title: "適用於 ASP.NET Core Microsoft.AspNetCore.All metapackage 2.x 及更新版本"
author: Rick-Anderson
description: "Microsoft.AspNetCore.All metapackage 包含所有支援的封裝。"
keywords: "ASP.NET Core、 NuGet 封裝，Microsoft.AspNetCore.All、 metapackage"
ms.author: riande
manager: wpickett
ms.date: 07/16/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/metapackage
ms.openlocfilehash: 255438a4ce36ce4978f8c8ee298388a25ac00d17
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2017
---
#<a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-2x"></a><span data-ttu-id="1ab2b-104">適用於 ASP.NET Core Microsoft.AspNetCore.All metapackage 2.x</span><span class="sxs-lookup"><span data-stu-id="1ab2b-104">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.x</span></span>

<span data-ttu-id="1ab2b-105">這項功能需要 ASP.NET Core 2.x。</span><span class="sxs-lookup"><span data-stu-id="1ab2b-105">This feature requires ASP.NET Core 2.x.</span></span>

<span data-ttu-id="1ab2b-106">[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All)適用於 ASP.NET Core metapackage 包括：</span><span class="sxs-lookup"><span data-stu-id="1ab2b-106">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="1ab2b-107">由 ASP.NET Core 小組支援所有封裝。</span><span class="sxs-lookup"><span data-stu-id="1ab2b-107">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="1ab2b-108">所有支援 Entity Framework Core 的套件。</span><span class="sxs-lookup"><span data-stu-id="1ab2b-108">All supported packages by the Entity Framework Core.</span></span> 
* <span data-ttu-id="1ab2b-109">內部和第 3 合作對象的相依性由 ASP.NET Core 和 Entity Framework Core。</span><span class="sxs-lookup"><span data-stu-id="1ab2b-109">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span> 

<span data-ttu-id="1ab2b-110">ASP.NET Core 的所有功能 2.x 和 Entity Framework Core 2.x 包含在`Microsoft.AspNetCore.All`封裝。</span><span class="sxs-lookup"><span data-stu-id="1ab2b-110">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="1ab2b-111">預設的專案範本會使用這個封裝。</span><span class="sxs-lookup"><span data-stu-id="1ab2b-111">The default project templates use this package.</span></span>

<span data-ttu-id="1ab2b-112">版本號碼`Microsoft.AspNetCore.All`metapackage 表示的 ASP.NET Core 版本和 Entity Framework Core 版本 （.NET Core 版本與對齊）。</span><span class="sxs-lookup"><span data-stu-id="1ab2b-112">The version number of the `Microsoft.AspNetCore.All` metapackage represents the ASP.NET Core version and Entity Framework Core version (aligned with the .NET Core version).</span></span>

<span data-ttu-id="1ab2b-113">使用應用程式`Microsoft.AspNetCore.All`metapackage 自動利用.NET 核心執行階段存放區。</span><span class="sxs-lookup"><span data-stu-id="1ab2b-113">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the .NET Core Runtime Store.</span></span> <span data-ttu-id="1ab2b-114">執行階段存放區包含執行 ASP.NET Core 2.x 應用程式所需的所有執行階段資產。</span><span class="sxs-lookup"><span data-stu-id="1ab2b-114">The Runtime Store contains all the runtime assets needed to run ASP.NET Core 2.x applications.</span></span> <span data-ttu-id="1ab2b-115">當您使用`Microsoft.AspNetCore.All`metapackage，**沒有**參考 ASP.NET Core NuGet 套件的資產會隨著應用程式部署&mdash;.NET 核心執行階段存放區包含這些資產。</span><span class="sxs-lookup"><span data-stu-id="1ab2b-115">When you use the `Microsoft.AspNetCore.All` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the .NET Core Runtime Store contains these assets.</span></span> <span data-ttu-id="1ab2b-116"><!-- todo add link to Runtime store -->在執行階段存放區中的資產中都會先行編譯為了改善應用程式啟動時間。</span><span class="sxs-lookup"><span data-stu-id="1ab2b-116"><!-- todo add link to Runtime store --> The assets in the Runtime Store are precompiled to improve application startup time.</span></span>

<span data-ttu-id="1ab2b-117">若要移除未使用的封裝，您可以使用封裝調整處理序。</span><span class="sxs-lookup"><span data-stu-id="1ab2b-117">You can use the package trimming process to remove packages that you don't use.</span></span> <span data-ttu-id="1ab2b-118">發行的應用程式的輸出中排除已修剪的封裝。</span><span class="sxs-lookup"><span data-stu-id="1ab2b-118">Trimmed packages are excluded in published application output.</span></span>

<span data-ttu-id="1ab2b-119">下列*.csproj*檔案參考`Microsoft.AspNetCore.All`適用於 ASP.NET Core metapackage:</span><span class="sxs-lookup"><span data-stu-id="1ab2b-119">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

<span data-ttu-id="1ab2b-120">[!code-xml[Main](..\mvc\views\view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=9)]</span><span class="sxs-lookup"><span data-stu-id="1ab2b-120">[!code-xml[Main](..\mvc\views\view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=9)]</span></span>
