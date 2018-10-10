---
uid: mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2
title: 如何升級 ASP.NET MVC 4 和 Web API 專案，以 ASP.NET MVC 5 和 Web API 2 |Microsoft Docs
author: Rick-Anderson
description: ASP.NET MVC 5 和 Web API 2 讓新的功能，包括屬性路由、 驗證篩選條件，以及其他更多的主機。
ms.author: riande
ms.date: 10/17/2013
ms.assetid: db0d02d9-58e8-4a0b-8d7d-b8df8ea97b88
msc.legacyurl: /mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 2566e201e44ccd9642abda7c7996056c73178fd6
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912848"
---
<a name="how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2"></a><span data-ttu-id="345b0-103">如何將 ASP.NET MVC 4 和 Web API 專案升級至 ASP.NET MVC 5 和 Web API 2</span><span class="sxs-lookup"><span data-stu-id="345b0-103">How to Upgrade an ASP.NET MVC 4 and Web API Project to ASP.NET MVC 5 and Web API 2</span></span>
====================
<span data-ttu-id="345b0-104">藉由[Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="345b0-104">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

> <span data-ttu-id="345b0-105">ASP.NET MVC 5 和 Web API 2 讓新的功能，包括屬性路由、 驗證篩選條件，以及其他更多的主機。</span><span class="sxs-lookup"><span data-stu-id="345b0-105">ASP.NET MVC 5 and Web API 2 bring a host of new features, including attribute routing, authentication filters, and much more.</span></span> <span data-ttu-id="345b0-106">請參閱[ https://www.asp.net/vnext ](https://www.asp.net/core)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="345b0-106">See [https://www.asp.net/vnext](https://www.asp.net/core) for more details.</span></span>
> 
> <span data-ttu-id="345b0-107">本逐步解說將引導您升級為最新版本的應用程式所需的步驟。</span><span class="sxs-lookup"><span data-stu-id="345b0-107">This walkthrough will guide you with the steps required to upgrade your application to the latest version.</span></span>  
> 
> > [!NOTE]
> > <span data-ttu-id="345b0-108">請參閱[ASP.NET 及 Web Tools for Visual Studio 2013 版本資訊](../../../visual-studio/overview/2013/release-notes.md)的重大變更的 MVC 4 和 Web API 的下一個版本的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="345b0-108">Please see [ASP.NET and Web Tools for Visual Studio 2013 Release Notes](../../../visual-studio/overview/2013/release-notes.md) for information on breaking changes from MVC 4 and Web API to the next version.</span></span>
> 
>   
> 
> <span data-ttu-id="345b0-109">這篇文章所編寫的 Youngjune 香港和 Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )</span><span class="sxs-lookup"><span data-stu-id="345b0-109">This article was written by Youngjune Hong and Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) )</span></span>


## <a name="upgrade-steps"></a><span data-ttu-id="345b0-110">升級步驟</span><span class="sxs-lookup"><span data-stu-id="345b0-110">Upgrade Steps</span></span>

1. <span data-ttu-id="345b0-111">備份您的專案。</span><span class="sxs-lookup"><span data-stu-id="345b0-111">Backup your project.</span></span> <span data-ttu-id="345b0-112">本逐步解說會要求您對您的專案檔、 封裝組態和 web.config 檔案中的變更。</span><span class="sxs-lookup"><span data-stu-id="345b0-112">This walkthrough will require you to make changes to your project file, package configuration, and web.config files.</span></span>
2. <span data-ttu-id="345b0-113">從 Web API 升級至 Web API 2 中，在 global.asax，變更：</span><span class="sxs-lookup"><span data-stu-id="345b0-113">For upgrading from Web API to Web API 2, in global.asax, change:</span></span>

    [!code-csharp[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample1.cs)]

   <span data-ttu-id="345b0-114">設為</span><span class="sxs-lookup"><span data-stu-id="345b0-114">to</span></span>

    [!code-csharp[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample2.cs)]
3. <span data-ttu-id="345b0-115">請確定您的專案使用的所有封裝都都使用 MVC 5 和 Web API 2 相容。</span><span class="sxs-lookup"><span data-stu-id="345b0-115">Make sure all the packages that your projects use are compatible with MVC 5 and Web API 2.</span></span> <span data-ttu-id="345b0-116">下表所示的 MVC 4 和 Web API 相關的封裝，不需要變更。</span><span class="sxs-lookup"><span data-stu-id="345b0-116">The following table shows the MVC 4 and Web API related packages than need to be changed.</span></span> <span data-ttu-id="345b0-117">如果您有相依於其中一個下面所列封裝的封裝，請連絡來取得與 MVC 5 和 Web API 2 相容的較新版本的發行者。</span><span class="sxs-lookup"><span data-stu-id="345b0-117">If you have a package that is dependent on one of the packages listed below, please contact the publishers to get the newer versions that are compatible with MVC 5 and Web API 2.</span></span> <span data-ttu-id="345b0-118">如果您有這些套件的原始程式碼，您應該使用新的組件的 MVC 5 和 Web API 2 重新編譯它們。</span><span class="sxs-lookup"><span data-stu-id="345b0-118">If you have the source code for those packages, you should recompile them with the new assemblies of MVC 5 and Web API 2.</span></span>   

    | <span data-ttu-id="345b0-119">**套件識別碼**</span><span class="sxs-lookup"><span data-stu-id="345b0-119">**Package Id**</span></span> | <span data-ttu-id="345b0-120">**舊的版本**</span><span class="sxs-lookup"><span data-stu-id="345b0-120">**Old version**</span></span> | <span data-ttu-id="345b0-121">**新的版本**</span><span class="sxs-lookup"><span data-stu-id="345b0-121">**New version**</span></span> |
    | --- | --- | --- |
    | <span data-ttu-id="345b0-122">Microsoft.AspNet.Razor</span><span class="sxs-lookup"><span data-stu-id="345b0-122">Microsoft.AspNet.Razor</span></span> | <span data-ttu-id="345b0-123">2.0.x.x</span><span class="sxs-lookup"><span data-stu-id="345b0-123">2.0.x.x</span></span> | <span data-ttu-id="345b0-124">3.0.0</span><span class="sxs-lookup"><span data-stu-id="345b0-124">3.0.0</span></span> |
    | <span data-ttu-id="345b0-125">Microsoft.AspNet.WebPages</span><span class="sxs-lookup"><span data-stu-id="345b0-125">Microsoft.AspNet.WebPages</span></span> | <span data-ttu-id="345b0-126">2.0.x.x</span><span class="sxs-lookup"><span data-stu-id="345b0-126">2.0.x.x</span></span> | <span data-ttu-id="345b0-127">3.0.0</span><span class="sxs-lookup"><span data-stu-id="345b0-127">3.0.0</span></span> |
    | <span data-ttu-id="345b0-128">Microsoft.AspNet.WebPages.WebData</span><span class="sxs-lookup"><span data-stu-id="345b0-128">Microsoft.AspNet.WebPages.WebData</span></span> | <span data-ttu-id="345b0-129">2.0.x.x</span><span class="sxs-lookup"><span data-stu-id="345b0-129">2.0.x.x</span></span> | <span data-ttu-id="345b0-130">3.0.0</span><span class="sxs-lookup"><span data-stu-id="345b0-130">3.0.0</span></span> |
    | <span data-ttu-id="345b0-131">Microsoft.AspNet.WebPages.OAuth</span><span class="sxs-lookup"><span data-stu-id="345b0-131">Microsoft.AspNet.WebPages.OAuth</span></span> | <span data-ttu-id="345b0-132">2.0.x.x</span><span class="sxs-lookup"><span data-stu-id="345b0-132">2.0.x.x</span></span> | <span data-ttu-id="345b0-133">3.0.0</span><span class="sxs-lookup"><span data-stu-id="345b0-133">3.0.0</span></span> |
    | <span data-ttu-id="345b0-134">Microsoft.AspNet.Mvc</span><span class="sxs-lookup"><span data-stu-id="345b0-134">Microsoft.AspNet.Mvc</span></span> | <span data-ttu-id="345b0-135">4.0.x.x</span><span class="sxs-lookup"><span data-stu-id="345b0-135">4.0.x.x</span></span> | <span data-ttu-id="345b0-136">5.0.0</span><span class="sxs-lookup"><span data-stu-id="345b0-136">5.0.0</span></span> |
    | <span data-ttu-id="345b0-137">Microsoft.AspNet.Mvc.Facebook</span><span class="sxs-lookup"><span data-stu-id="345b0-137">Microsoft.AspNet.Mvc.Facebook</span></span> | <span data-ttu-id="345b0-138">4.0.x.x</span><span class="sxs-lookup"><span data-stu-id="345b0-138">4.0.x.x</span></span> | <span data-ttu-id="345b0-139">5.0.0</span><span class="sxs-lookup"><span data-stu-id="345b0-139">5.0.0</span></span> |
    | <span data-ttu-id="345b0-140">Microsoft.AspNet.WebApi.Core</span><span class="sxs-lookup"><span data-stu-id="345b0-140">Microsoft.AspNet.WebApi.Core</span></span> | <span data-ttu-id="345b0-141">4.0.x.x</span><span class="sxs-lookup"><span data-stu-id="345b0-141">4.0.x.x</span></span> | <span data-ttu-id="345b0-142">5.0.0</span><span class="sxs-lookup"><span data-stu-id="345b0-142">5.0.0</span></span> |
    | <span data-ttu-id="345b0-143">Microsoft.AspNet.WebApi.SelfHost</span><span class="sxs-lookup"><span data-stu-id="345b0-143">Microsoft.AspNet.WebApi.SelfHost</span></span> | <span data-ttu-id="345b0-144">4.0.x.x</span><span class="sxs-lookup"><span data-stu-id="345b0-144">4.0.x.x</span></span> | <span data-ttu-id="345b0-145">5.0.0</span><span class="sxs-lookup"><span data-stu-id="345b0-145">5.0.0</span></span> |
    | <span data-ttu-id="345b0-146">Microsoft.AspNet.WebApi.Client</span><span class="sxs-lookup"><span data-stu-id="345b0-146">Microsoft.AspNet.WebApi.Client</span></span> | <span data-ttu-id="345b0-147">4.0.x.x</span><span class="sxs-lookup"><span data-stu-id="345b0-147">4.0.x.x</span></span> | <span data-ttu-id="345b0-148">5.0.0</span><span class="sxs-lookup"><span data-stu-id="345b0-148">5.0.0</span></span> |
    | <span data-ttu-id="345b0-149">Microsoft.AspNet.WebApi.OData</span><span class="sxs-lookup"><span data-stu-id="345b0-149">Microsoft.AspNet.WebApi.OData</span></span> | <span data-ttu-id="345b0-150">4.0.x.x</span><span class="sxs-lookup"><span data-stu-id="345b0-150">4.0.x.x</span></span> | <span data-ttu-id="345b0-151">5.0.0</span><span class="sxs-lookup"><span data-stu-id="345b0-151">5.0.0</span></span> |
    | <span data-ttu-id="345b0-152">Microsoft.AspNet.WebApi</span><span class="sxs-lookup"><span data-stu-id="345b0-152">Microsoft.AspNet.WebApi</span></span> | <span data-ttu-id="345b0-153">4.0.x.x</span><span class="sxs-lookup"><span data-stu-id="345b0-153">4.0.x.x</span></span> | <span data-ttu-id="345b0-154">5.0.0</span><span class="sxs-lookup"><span data-stu-id="345b0-154">5.0.0</span></span> |
    | <span data-ttu-id="345b0-155">Microsoft.AspNet.WebApi.WebHost</span><span class="sxs-lookup"><span data-stu-id="345b0-155">Microsoft.AspNet.WebApi.WebHost</span></span> | <span data-ttu-id="345b0-156">4.0.x.x</span><span class="sxs-lookup"><span data-stu-id="345b0-156">4.0.x.x</span></span> | <span data-ttu-id="345b0-157">5.0.0</span><span class="sxs-lookup"><span data-stu-id="345b0-157">5.0.0</span></span> |
    | <span data-ttu-id="345b0-158">Microsoft.AspNet.WebApi.Tracing</span><span class="sxs-lookup"><span data-stu-id="345b0-158">Microsoft.AspNet.WebApi.Tracing</span></span> | <span data-ttu-id="345b0-159">4.0.x.x</span><span class="sxs-lookup"><span data-stu-id="345b0-159">4.0.x.x</span></span> | <span data-ttu-id="345b0-160">5.0.0</span><span class="sxs-lookup"><span data-stu-id="345b0-160">5.0.0</span></span> |
    | <span data-ttu-id="345b0-161">Microsoft.AspNet.WebApi.HelpPage</span><span class="sxs-lookup"><span data-stu-id="345b0-161">Microsoft.AspNet.WebApi.HelpPage</span></span> | <span data-ttu-id="345b0-162">4.0.x.x</span><span class="sxs-lookup"><span data-stu-id="345b0-162">4.0.x.x</span></span> | <span data-ttu-id="345b0-163">5.0.0</span><span class="sxs-lookup"><span data-stu-id="345b0-163">5.0.0</span></span> |
    | <span data-ttu-id="345b0-164">Microsoft.Net.Http</span><span class="sxs-lookup"><span data-stu-id="345b0-164">Microsoft.Net.Http</span></span> | <span data-ttu-id="345b0-165">2.0.x。</span><span class="sxs-lookup"><span data-stu-id="345b0-165">2.0.x.</span></span> | <span data-ttu-id="345b0-166">2.2.x 版本。</span><span class="sxs-lookup"><span data-stu-id="345b0-166">2.2.x.</span></span> |
    | <span data-ttu-id="345b0-167">Microsoft.Data.OData</span><span class="sxs-lookup"><span data-stu-id="345b0-167">Microsoft.Data.OData</span></span> | <span data-ttu-id="345b0-168">5.2.x</span><span class="sxs-lookup"><span data-stu-id="345b0-168">5.2.x</span></span> | <span data-ttu-id="345b0-169">5.6.x</span><span class="sxs-lookup"><span data-stu-id="345b0-169">5.6.x</span></span> |
    | <span data-ttu-id="345b0-170">System.Spatial</span><span class="sxs-lookup"><span data-stu-id="345b0-170">System.Spatial</span></span> | <span data-ttu-id="345b0-171">5.2.x</span><span class="sxs-lookup"><span data-stu-id="345b0-171">5.2.x</span></span> | <span data-ttu-id="345b0-172">5.6.x</span><span class="sxs-lookup"><span data-stu-id="345b0-172">5.6.x</span></span> |
    | <span data-ttu-id="345b0-173">Microsoft.Data.Edm</span><span class="sxs-lookup"><span data-stu-id="345b0-173">Microsoft.Data.Edm</span></span> | <span data-ttu-id="345b0-174">5.2.x</span><span class="sxs-lookup"><span data-stu-id="345b0-174">5.2.x</span></span> | <span data-ttu-id="345b0-175">5.6.x</span><span class="sxs-lookup"><span data-stu-id="345b0-175">5.6.x</span></span> |
    | <span data-ttu-id="345b0-176">Microsoft.AspNet.Mvc.FixedDisplayModes</span><span class="sxs-lookup"><span data-stu-id="345b0-176">Microsoft.AspNet.Mvc.FixedDisplayModes</span></span> | <span data-ttu-id="345b0-177">< o: p>< >< / o: p>< ></span><span class="sxs-lookup"><span data-stu-id="345b0-177"><o:p> </o:p></span></span> | <span data-ttu-id="345b0-178">已移除</span><span class="sxs-lookup"><span data-stu-id="345b0-178">Removed</span></span> |
    | <span data-ttu-id="345b0-179">Microsoft.AspNet.WebPages.Administration</span><span class="sxs-lookup"><span data-stu-id="345b0-179">Microsoft.AspNet.WebPages.Administration</span></span> | <span data-ttu-id="345b0-180">< o: p>< >< / o: p>< ></span><span class="sxs-lookup"><span data-stu-id="345b0-180"><o:p> </o:p></span></span> | <span data-ttu-id="345b0-181">已移除</span><span class="sxs-lookup"><span data-stu-id="345b0-181">Removed</span></span> |
    | <span data-ttu-id="345b0-182">Microsoft-Web-Helpers</span><span class="sxs-lookup"><span data-stu-id="345b0-182">Microsoft-Web-Helpers</span></span> | <span data-ttu-id="345b0-183">< o: p>< >< / o: p>< ></span><span class="sxs-lookup"><span data-stu-id="345b0-183"><o:p> </o:p></span></span> | <span data-ttu-id="345b0-184">Microsoft.AspNet.WebHelpers</span><span class="sxs-lookup"><span data-stu-id="345b0-184">Microsoft.AspNet.WebHelpers</span></span> |

    > [!NOTE]
    > <span data-ttu-id="345b0-185">Microsoft Web Helpers 已被取代 Microsoft.AspNet.WebHelpers。</span><span class="sxs-lookup"><span data-stu-id="345b0-185">Microsoft-Web-Helpers has been replaced with Microsoft.AspNet.WebHelpers.</span></span> <span data-ttu-id="345b0-186">您應該先，移除舊的封裝，然後再安裝較新的套件。</span><span class="sxs-lookup"><span data-stu-id="345b0-186">You should remove the old package first, and then install the newer package.</span></span>   
    >   
    > <span data-ttu-id="345b0-187">沒有跨版本相容性之間主要的 ASP.NET 套件。</span><span class="sxs-lookup"><span data-stu-id="345b0-187">There is no cross version compatibility among major ASP.NET packages.</span></span> <span data-ttu-id="345b0-188">例如，MVC 5 適用於只有第 3 Razor 和不 Razor 2。</span><span class="sxs-lookup"><span data-stu-id="345b0-188">For example, MVC 5 is compatible with only Razor 3, and not Razor 2.</span></span>
4. <span data-ttu-id="345b0-189">在 Visual Studio 中開啟專案。</span><span class="sxs-lookup"><span data-stu-id="345b0-189">Open your project in Visual Studio.</span></span>
5. <span data-ttu-id="345b0-190">移除任何已安裝下列 ASP.NET NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="345b0-190">Remove any of the following ASP.NET NuGet packages that are installed.</span></span> <span data-ttu-id="345b0-191">您將會移除這些使用套件管理員主控台 (PMC)。</span><span class="sxs-lookup"><span data-stu-id="345b0-191">You will remove these using the Package Manager Console (PMC).</span></span> <span data-ttu-id="345b0-192">若要開啟 PMC 中，選取**工具**功能表，然後選取**NuGet 套件管理員**然後選取**Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="345b0-192">To open the PMC, select the **Tools** menu and then select **NuGet Package Manager,** then select **Package Manager Console**.</span></span> <span data-ttu-id="345b0-193">您的專案可能不會包含所有這些。</span><span class="sxs-lookup"><span data-stu-id="345b0-193">Your project might not include all of these.</span></span>

    1. `Microsoft.AspNet.WebPages.Administration`  
   <span data-ttu-id="345b0-194">通常在從 MVC 3 升級至 MVC 4 時，會新增此套件。</span><span class="sxs-lookup"><span data-stu-id="345b0-194">This package is typically added when upgrading from MVC 3 to MVC 4.</span></span> <span data-ttu-id="345b0-195">若要移除它，請在 PMC 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="345b0-195">To remove it, run the following command in the PMC:</span></span>  
        `Uninstall-Package -Id Microsoft.AspNet.WebPages.Administration`
    2. `Microsoft-Web-Helpers`   
   <span data-ttu-id="345b0-196">此套件已更名為`Microsoft.AspNet.WebHelpers`。</span><span class="sxs-lookup"><span data-stu-id="345b0-196">This package has been rebranded as `Microsoft.AspNet.WebHelpers`.</span></span> <span data-ttu-id="345b0-197">若要移除它，請在 PMC 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="345b0-197">To remove it, run the following command in the PMC:</span></span>  
        `Uninstall-Package -Id Microsoft-Web-Helpers`
    3. `Microsoft.AspNet.Mvc.FixedDisplayMode`  
   <span data-ttu-id="345b0-198">此套件包含 bug 已修正在 MVC 5 的 MVC 4 中因應措施。</span><span class="sxs-lookup"><span data-stu-id="345b0-198">This package contains a work around for a bug in MVC 4 that has been fixed in MVC 5.</span></span> <span data-ttu-id="345b0-199">若要移除它，請在 PMC 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="345b0-199">To remove it, run the following command in the PMC:</span></span>  
        `Uninstall-Package -Id Microsoft.AspNet.Mvc.FixedDisplayModes`
6. <span data-ttu-id="345b0-200">升級所有使用 PMC 的 ASP.NET NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="345b0-200">Upgrade all the ASP.NET NuGet packages using the PMC.</span></span> <span data-ttu-id="345b0-201">在 PMC 中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="345b0-201">In the PMC, run the following command:</span></span>  
    `Update-Package`  
   <span data-ttu-id="345b0-202">`Update-Package`命令沒有任何參數將會更新每個套件。</span><span class="sxs-lookup"><span data-stu-id="345b0-202">The `Update-Package` command without any parameters will update every package.</span></span> <span data-ttu-id="345b0-203">您可以使用識別碼引數的個別更新套件。</span><span class="sxs-lookup"><span data-stu-id="345b0-203">You can update packages individually by using the ID argument.</span></span> <span data-ttu-id="345b0-204">如需有關更新命令的詳細資訊，請執行`get-help update-package`。</span><span class="sxs-lookup"><span data-stu-id="345b0-204">For more information about the update command, run     `get-help update-package` .</span></span>

## <a name="update-the-application-webconfig-file"></a><span data-ttu-id="345b0-205">更新應用程式*web.config*檔案</span><span class="sxs-lookup"><span data-stu-id="345b0-205">Update the Application *web.config* File</span></span>

<span data-ttu-id="345b0-206">請務必在應用程式中進行這些變更*web.config*檔案，不*web.config*中的檔案*檢視*資料夾。</span><span class="sxs-lookup"><span data-stu-id="345b0-206">Be sure to make these changes in the app *web.config* file, not the *web.config* file in the *Views* folder.</span></span>

<span data-ttu-id="345b0-207">找出`<runtime>/<assemblyBinding>`區段，然後進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="345b0-207">Locate the `<runtime>/<assemblyBinding>` section, and make the following changes:</span></span>

1. <span data-ttu-id="345b0-208">在名稱屬性"System.Web.Mvc 」 的項目，變更 「 version=5.0.0.0"從"4.0.0.0 」 版本號碼。</span><span class="sxs-lookup"><span data-stu-id="345b0-208">In the elements with the name attribute "System.Web.Mvc", change the version number from "4.0.0.0" to "5.0.0.0".</span></span> <span data-ttu-id="345b0-209">（在該項目中的兩個變更。）</span><span class="sxs-lookup"><span data-stu-id="345b0-209">(Two changes in that element.)</span></span>
2. <span data-ttu-id="345b0-210">在具有 name 屬性的項目&quot;System.Web.Helpers 」 並&quot;System.Web.WebPages&quot;變更從 「 2.0.0.0"到"3.0.0.0"版本數字。</span><span class="sxs-lookup"><span data-stu-id="345b0-210">In elements with the name attribute &quot;System.Web.Helpers" and &quot;System.Web.WebPages&quot; change the version number from "2.0.0.0" to "3.0.0.0".</span></span> <span data-ttu-id="345b0-211">四個變更，就會在每個元素的兩個。</span><span class="sxs-lookup"><span data-stu-id="345b0-211">Four changes will occur, two in each of the elements.</span></span>

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample3.xml?highlight=6,10,14)]
3. <span data-ttu-id="345b0-212">找出`<appSettings>`區段，並更新到 3.0.0.0 webpages:version 從 2.0.0.0.0，如下所示：</span><span class="sxs-lookup"><span data-stu-id="345b0-212">Locate the `<appSettings>` section and update the webpages:version from 2.0.0.0.0 to 3.0.0.0 as shown below:</span></span>

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample4.xml?highlight=2)]
4. <span data-ttu-id="345b0-213">移除任何不完整的信任層級。</span><span class="sxs-lookup"><span data-stu-id="345b0-213">Remove any trust levels other than Full.</span></span> <span data-ttu-id="345b0-214">例如: </span><span class="sxs-lookup"><span data-stu-id="345b0-214">For example:</span></span>

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample5.xml?highlight=2)]

## <a name="update-the-webconfig-files-under-the-views-folder"></a><span data-ttu-id="345b0-215">更新*web.config* [Views] 資料夾下的檔案</span><span class="sxs-lookup"><span data-stu-id="345b0-215">Update the *web.config* files under the Views folder</span></span>

<span data-ttu-id="345b0-216">如果您的應用程式使用的區域，您也必須更新每個*web.config*中的檔案*檢視*的每個 Areas 資料夾的子資料夾。</span><span class="sxs-lookup"><span data-stu-id="345b0-216">If your application is using areas, you will also need to update each *web.config* file in the *Views* sub-folder of each Area folder.</span></span>

1. <span data-ttu-id="345b0-217">更新版本 「 version=5.0.0.0"版本"4.0.0.0 」 包含"System.Web.Mvc 」 的所有項目。</span><span class="sxs-lookup"><span data-stu-id="345b0-217">Update all elements that contain "System.Web.Mvc" from version "4.0.0.0" to version "5.0.0.0".</span></span>  

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample6.xml?highlight=2)]

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample7.xml?highlight=4-6,8)]
2. <span data-ttu-id="345b0-218">更新包含 「 2.0.0.0"版 「 System.Web.WebPages.Razor 」 到 「 3.0.0.0"版的所有項目。</span><span class="sxs-lookup"><span data-stu-id="345b0-218">Update all elements that contain "System.Web.WebPages.Razor" from version "2.0.0.0" to version"3.0.0.0".</span></span> <span data-ttu-id="345b0-219">本節包含 「 System.Web.WebPages"，如果更新版本 「 3.0.0.0"版本"2.0.0.0 」 從這些項目</span><span class="sxs-lookup"><span data-stu-id="345b0-219">If this section contains "System.Web.WebPages", update those elements from version "2.0.0.0" to version"3.0.0.0"</span></span>  

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample8.xml?highlight=3-5)]
3. <span data-ttu-id="345b0-220">如果您移除了`Microsoft-Web-Helpers`NuGet 套件，在上一個步驟中，安裝`Microsoft.AspNet.WebHelpers`使用下列命令在 PMC 中：</span><span class="sxs-lookup"><span data-stu-id="345b0-220">If you removed the `Microsoft-Web-Helpers` NuGet package in a previous step, install `Microsoft.AspNet.WebHelpers` with the following command in the PMC:</span></span>  
    `Install-Package -Id Microsoft.AspNet.WebHelpers`
4. <span data-ttu-id="345b0-221">如果您的應用程式會使用[User.IsInRole()](https://msdn.microsoft.com/en-us/library/system.web.security.roleprincipal.isinrole(v=vs.110).aspx)方法，將下列內容加入*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="345b0-221">If your app uses the [User.IsInRole()](https://msdn.microsoft.com/en-us/library/system.web.security.roleprincipal.isinrole(v=vs.110).aspx) method, add the following to the *Web.config* file.</span></span>

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample9.xml)]

## <a name="final-steps"></a><span data-ttu-id="345b0-222">最後一個步驟</span><span class="sxs-lookup"><span data-stu-id="345b0-222">Final Steps</span></span>

<span data-ttu-id="345b0-223">建置及測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="345b0-223">Build and test the application.</span></span>

<span data-ttu-id="345b0-224">從專案檔中移除 MVC 4 專案類型 GUID。</span><span class="sxs-lookup"><span data-stu-id="345b0-224">Remove the MVC 4 project type GUID from the project files.</span></span>

1. <span data-ttu-id="345b0-225">在 [方案總管] 中，以滑鼠右鍵按一下專案名稱，然後按**卸載專案**。</span><span class="sxs-lookup"><span data-stu-id="345b0-225">In Solution Explorer, right-click the project name and then select **Unload Project**.</span></span>
2. <span data-ttu-id="345b0-226">以滑鼠右鍵按一下專案，然後選取 [編輯 projectname.csproj]。</span><span class="sxs-lookup"><span data-stu-id="345b0-226">Right-click the project and select Edit ProjectName.csproj.</span></span>
3. <span data-ttu-id="345b0-227">找出`ProjectTypeGuids`項目，然後移除 MVC 4 專案的 GUID， `{E3E379DF-F4C6-4180-9B81-6769533ABE47}`。</span><span class="sxs-lookup"><span data-stu-id="345b0-227">Locate the `ProjectTypeGuids` element and then remove the MVC 4 project GUID, `{E3E379DF-F4C6-4180-9B81-6769533ABE47}`.</span></span>
4. <span data-ttu-id="345b0-228">儲存並關閉開啟的專案檔。</span><span class="sxs-lookup"><span data-stu-id="345b0-228">Save and close the open project file.</span></span>
5. <span data-ttu-id="345b0-229">以滑鼠右鍵按一下專案，然後選取**重新載入專案**。</span><span class="sxs-lookup"><span data-stu-id="345b0-229">Right-click the project and select **Reload Project**.</span></span>
