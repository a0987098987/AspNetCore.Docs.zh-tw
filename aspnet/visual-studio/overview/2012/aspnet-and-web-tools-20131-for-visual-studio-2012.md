---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: 適用於 ASP.NET 和 Web 工具 2013.1 for Visual Studio 2012 版本資訊 |Microsoft 文件
author: microsoft
description: 本文件說明的 ASP.NET 和 Web 工具 2013.1 for Visual Studio 2012 的版本。
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2013
ms.topic: article
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: c11e2ef9c33b0cae1f196690533094ce1c342da5
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/08/2018
ms.locfileid: "28036423"
---
<a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a><span data-ttu-id="3e6c9-103">適用於 ASP.NET 和 Web 工具 2013.1 for Visual Studio 2012 版本資訊</span><span class="sxs-lookup"><span data-stu-id="3e6c9-103">Release Notes for ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>
====================
<span data-ttu-id="3e6c9-104">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="3e6c9-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="3e6c9-105">本文件說明的 ASP.NET 和 Web 工具 2013.1 for Visual Studio 2012 的版本。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-105">This document describes the release of ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>


## <a name="contents"></a><span data-ttu-id="3e6c9-106">內容</span><span class="sxs-lookup"><span data-stu-id="3e6c9-106">Contents</span></span>

- [<span data-ttu-id="3e6c9-107">安裝注意事項</span><span class="sxs-lookup"><span data-stu-id="3e6c9-107">Installation Notes</span></span>](#install)
- [<span data-ttu-id="3e6c9-108">軟體需求</span><span class="sxs-lookup"><span data-stu-id="3e6c9-108">Software Requirements</span></span>](#requirements)
- <span data-ttu-id="3e6c9-109">在 ASP.NET 和 Web 工具 2013.1 for Visual Studio 2012 中的新功能</span><span class="sxs-lookup"><span data-stu-id="3e6c9-109">New Features in ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

    - [<span data-ttu-id="3e6c9-110">啟動程序</span><span class="sxs-lookup"><span data-stu-id="3e6c9-110">Bootstrap</span></span>](#bootstrap)
    - [<span data-ttu-id="3e6c9-111">範本</span><span class="sxs-lookup"><span data-stu-id="3e6c9-111">Templates</span></span>](#templates)

        - [<span data-ttu-id="3e6c9-112">ASP.NET MVC 5 範本</span><span class="sxs-lookup"><span data-stu-id="3e6c9-112">ASP.NET MVC 5 template</span></span>](#mvc5template)
        - [<span data-ttu-id="3e6c9-113">ASP.NET Web API 2 範本</span><span class="sxs-lookup"><span data-stu-id="3e6c9-113">ASP.NET Web API 2 template</span></span>](#apitemplate)
        - [<span data-ttu-id="3e6c9-114">項目範本</span><span class="sxs-lookup"><span data-stu-id="3e6c9-114">Item Templates</span></span>](#itemtemplate)
    - [<span data-ttu-id="3e6c9-115">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="3e6c9-115">Entity Framework 6</span></span>](#ef6)
    - [<span data-ttu-id="3e6c9-116">ASP.NET Scaffolding</span><span class="sxs-lookup"><span data-stu-id="3e6c9-116">ASP.NET Scaffolding</span></span>](#scaffold)
    - [<span data-ttu-id="3e6c9-117">Razor 編輯器</span><span class="sxs-lookup"><span data-stu-id="3e6c9-117">Razor Editor</span></span>](#razor)
    - [<span data-ttu-id="3e6c9-118">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="3e6c9-118">NuGet 2.7</span></span>](#nuget)
- <span data-ttu-id="3e6c9-119">已知的問題和重大變更</span><span class="sxs-lookup"><span data-stu-id="3e6c9-119">Known Issues and Breaking Changes</span></span>

    - [<span data-ttu-id="3e6c9-120">ASP.NET Scaffolding</span><span class="sxs-lookup"><span data-stu-id="3e6c9-120">ASP.NET Scaffolding</span></span>](#issuescaffolding)

        - [<span data-ttu-id="3e6c9-121">MVC 和 Web API Scaffolding-HTTP 404 找不到錯誤</span><span class="sxs-lookup"><span data-stu-id="3e6c9-121">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>](#404issue)
        - [<span data-ttu-id="3e6c9-122">Visual Studio Express 2012 for Web 加入 scaffold 項目之後會停止運作</span><span class="sxs-lookup"><span data-stu-id="3e6c9-122">Visual Studio Express 2012 for Web stops working after adding a scaffolded item</span></span>](#expressissue)
    - [<span data-ttu-id="3e6c9-123">ASP.NET Razor 3</span><span class="sxs-lookup"><span data-stu-id="3e6c9-123">ASP.NET Razor 3</span></span>](#issuerazor)

        - [<span data-ttu-id="3e6c9-124">檢視與瀏覽或 F5 cshtml 檔案會導致伺服器錯誤</span><span class="sxs-lookup"><span data-stu-id="3e6c9-124">Viewing cshtml file with Browse With or F5 causes a server error</span></span>](#browseissue)
        - [<span data-ttu-id="3e6c9-125">Url 重寫和 Tilde(~)</span><span class="sxs-lookup"><span data-stu-id="3e6c9-125">Url Rewrite and Tilde(~)</span></span>](#rewriteissue)
    - [<span data-ttu-id="3e6c9-126">範本</span><span class="sxs-lookup"><span data-stu-id="3e6c9-126">Templates</span></span>](#templateissue)

<a id="install"></a>
## <a name="installation-notes"></a><span data-ttu-id="3e6c9-127">安裝注意事項</span><span class="sxs-lookup"><span data-stu-id="3e6c9-127">Installation Notes</span></span>

<span data-ttu-id="3e6c9-128">[安裝](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids)ASP.NET 及 Web Tools for Visual Studio 2012 2013.1。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-128">[Install](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

<a id="requirements"></a>
## <a name="software-requirements"></a><span data-ttu-id="3e6c9-129">軟體需求</span><span class="sxs-lookup"><span data-stu-id="3e6c9-129">Software Requirements</span></span>

<span data-ttu-id="3e6c9-130">您必須有 Visual Studio 2012 或 Visual Studio Express 2012 for Web。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-130">You must have either Visual Studio 2012 or Visual Studio Express 2012 for Web.</span></span>

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a><span data-ttu-id="3e6c9-131">在 ASP.NET 和 Web 工具 2013.1 for Visual Studio 2012 中的新功能</span><span class="sxs-lookup"><span data-stu-id="3e6c9-131">New Features in ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

<a id="bootstrap"></a>
### <a name="bootstrap"></a><span data-ttu-id="3e6c9-132">啟動程序</span><span class="sxs-lookup"><span data-stu-id="3e6c9-132">Bootstrap</span></span>

<span data-ttu-id="3e6c9-133">當 add-migration MVC 5 控制器和檢視時，會使用 檢視標記[Bootstrap](http://getbootstrap.com/)。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-133">When you scaffold MVC 5 controllers and views, the markup for the views uses [Bootstrap](http://getbootstrap.com/).</span></span>

<a id="templates"></a>
### <a name="templates"></a><span data-ttu-id="3e6c9-134">範本</span><span class="sxs-lookup"><span data-stu-id="3e6c9-134">Templates</span></span>

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a><span data-ttu-id="3e6c9-135">ASP.NET MVC 5 範本</span><span class="sxs-lookup"><span data-stu-id="3e6c9-135">ASP.NET MVC 5 template</span></span>

<span data-ttu-id="3e6c9-136">我們加入了新的 MVC 5 範本。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-136">We added a new MVC 5 template.</span></span> <span data-ttu-id="3e6c9-137">它所參考的最新的 MVC 5 NuGet 封裝，而且您可以使用 scaffolding 新增控制器和檢視。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-137">It references the latest MVC 5 NuGet packages, and you can use scaffolding to add controllers and views.</span></span>

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a><span data-ttu-id="3e6c9-138">ASP.NET Web API 2 範本</span><span class="sxs-lookup"><span data-stu-id="3e6c9-138">ASP.NET Web API 2 template</span></span>

<span data-ttu-id="3e6c9-139">我們加入了新的 Web API 2 範本。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-139">We added a new Web API 2 template.</span></span> <span data-ttu-id="3e6c9-140">它會參考最新的 Web API 2 NuGet 封裝，而且您可以使用 scaffolding 新增控制器和檢視。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-140">It references the latest Web API 2 NuGet packages, and you can use scaffolding to add controllers and views.</span></span>

<a id="itemtemplate"></a>
#### <a name="item-templates"></a><span data-ttu-id="3e6c9-141">項目範本</span><span class="sxs-lookup"><span data-stu-id="3e6c9-141">Item Templates</span></span>

<span data-ttu-id="3e6c9-142">我們加入了新的 MVC 5 檢視、 Web 頁面 (Razor 3)，以及 Web API 2 控制器的項目範本。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-142">We added new item templates for MVC 5 views, Web Pages (Razor 3), and Web API 2 controllers.</span></span> <span data-ttu-id="3e6c9-143">在安裝相關的 NuGet 封裝加入專案時加入新項目。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-143">They install the related NuGet packages to the project while adding new items.</span></span>

<a id="ef6"></a>
### <a name="entity-framework-6"></a><span data-ttu-id="3e6c9-144">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="3e6c9-144">Entity Framework 6</span></span>

<span data-ttu-id="3e6c9-145">當您為使用 Entity Framework 的 MVC 或 Web API 控制器時，我們會使用 Framework 6。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-145">When you scaffold an MVC or Web API controller using Entity Framework, we use Framework 6.</span></span> <span data-ttu-id="3e6c9-146">如需有關 Entity Framework 的詳細資訊，請參閱[Entity Framework 版本記錄](https://msdn.com/data/jj574253)。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-146">For more information about Entity Framework, see the [Entity Framework Version History](https://msdn.com/data/jj574253).</span></span>

<span data-ttu-id="3e6c9-147">您也可以下載並安裝 Entity Framework 6 Tools for Visual Studio 2012。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-147">You can also download and install the Entity Framework 6 Tools for Visual Studio 2012.</span></span> <span data-ttu-id="3e6c9-148">請參閱[取得 Entity Framework](https://msdn.com/data/ee712906#tooling)。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-148">See the [Get Entity Framework](https://msdn.com/data/ee712906#tooling).</span></span>

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a><span data-ttu-id="3e6c9-149">ASP.NET Scaffolding</span><span class="sxs-lookup"><span data-stu-id="3e6c9-149">ASP.NET Scaffolding</span></span>

<span data-ttu-id="3e6c9-150">ASP.NET Scaffolding 是 ASP.NET Web 應用程式的程式碼產生架構。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-150">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="3e6c9-151">它可讓您輕鬆地將未定案程式碼加入至您的專案與資料模型互動。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-151">It makes it easy to add boilerplate code to your project that interacts with a data model.</span></span>

<span data-ttu-id="3e6c9-152">在舊版的 Visual Studio 中，scaffolding 限於 ASP.NET MVC 專案。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-152">In previous versions of Visual Studio, scaffolding was limited to ASP.NET MVC projects.</span></span> <span data-ttu-id="3e6c9-153">透過這項更新，您現在可以針對任何 ASP.NET 專案，包括 Web Form 使用 scaffolding。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-153">With this update, you can now use scaffolding for any ASP.NET project, including Web Forms.</span></span> <span data-ttu-id="3e6c9-154">此更新不支援產生頁面的 Web Form 專案，但您仍然可以使用與 Web Form scaffolding，將 MVC 相依性加入至專案。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-154">This update does not support generating pages for a Web Forms project, but you can still use scaffolding with Web Forms by adding MVC dependencies to the project.</span></span> <span data-ttu-id="3e6c9-155">在未來的更新中，將會加入產生的 Web Form 網頁的支援。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-155">Support for generating pages for Web Forms will be added in a future update.</span></span>

<span data-ttu-id="3e6c9-156">當使用 scaffolding，我們可以確定所有必要的相依性安裝到專案。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-156">When using scaffolding, we ensure that all required dependencies are installed in the project.</span></span> <span data-ttu-id="3e6c9-157">比方說，如果您從 ASP.NET Web Form 專案開始，然後使用 scaffolding 新增 Web API 控制器所需的 NuGet 封裝和參考會加入至您的專案會自動。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-157">For example, if you start with an ASP.NET Web Forms project and then use scaffolding to add a Web API Controller, the required NuGet packages and references are added to your project automatically.</span></span>

<span data-ttu-id="3e6c9-158">MVC 樣板加入至 Web Form 專案，加入**新的 Scaffold 項目**選取**MVC 5 相依性**對話視窗中。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-158">To add MVC scaffolding to a Web Forms project, add a **New Scaffolded Item** and select **MVC 5 Dependencies** in the dialog window.</span></span> <span data-ttu-id="3e6c9-159">有兩個選項的 scaffolding MVC;最少且完整。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-159">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="3e6c9-160">如果您選取最少，只有 NuGet 封裝和 ASP.NET MVC 的參考會加入至您的專案。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-160">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="3e6c9-161">如果您選取 [完整] 選項，加入最少的相依性，以及所需的內容檔案的 MVC 專案。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-161">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span>

<span data-ttu-id="3e6c9-162">支援的 scaffolding 非同步控制器會使用新的非同步功能，從 Entity Framework 6。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-162">Support for scaffolding async controllers uses the new async features from Entity Framework 6.</span></span>

<span data-ttu-id="3e6c9-163">如需詳細資訊和教學課程，請參閱[ASP.NET Scaffolding 概觀](../2013/aspnet-scaffolding-overview.md)。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-163">For more information and tutorials, see [ASP.NET Scaffolding Overview](../2013/aspnet-scaffolding-overview.md).</span></span> <span data-ttu-id="3e6c9-164">這些教學課程示範 scaffolding 使用 Visual Studio 2013，但也適用於 ASP.NET 和 Web 工具 2013.1 for Visual Studio 2012。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-164">These tutorials show scaffolding with Visual Studio 2013, but they are also applicable to ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

<a id="razor"></a>
### <a name="razor-editor"></a><span data-ttu-id="3e6c9-165">Razor 編輯器</span><span class="sxs-lookup"><span data-stu-id="3e6c9-165">Razor Editor</span></span>

<span data-ttu-id="3e6c9-166">這項更新，使用 Visual Studio 2012 現在可支援 Razor 3 工具/編輯。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-166">With this update, Visual Studio 2012 now supports Razor 3 tooling/editing.</span></span>

<a id="nuget"></a>
### <a name="nuget-27"></a><span data-ttu-id="3e6c9-167">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="3e6c9-167">NuGet 2.7</span></span>

<span data-ttu-id="3e6c9-168">NuGet 2.7 包含一組豐富的新功能所說明的詳細討論[NuGet 2.7 版本資訊](http://docs.nuget.org/docs/release-notes/nuget-2.7)。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-168">NuGet 2.7 includes a rich set of new features which are described in detail at [NuGet 2.7 Release Notes](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span></span>

<span data-ttu-id="3e6c9-169">這個版本的 NuGet 不再需要使用者明確地允許 NuGet 還原遺漏的套件。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-169">This version of NuGet removes the need for users to explicitly allow NuGet to restore missing packages.</span></span> <span data-ttu-id="3e6c9-170">安裝 NuGet 2.7，當使用者隱含地同意自動還原遺漏的套件。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-170">When installing NuGet 2.7, users implicitly consent to automatically restoring missing packages.</span></span> <span data-ttu-id="3e6c9-171">使用者可以明確選擇退出透過 NuGet 設定 Visual Studio 中的封裝還原。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-171">Users can explicitly opt out of package restoration through the NuGet settings in Visual Studio.</span></span> <span data-ttu-id="3e6c9-172">這項變更可簡化在封裝還原的運作方式。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-172">This change simplifies how package restoration works.</span></span>

## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="3e6c9-173">已知的問題和重大變更</span><span class="sxs-lookup"><span data-stu-id="3e6c9-173">Known Issues and Breaking Changes</span></span>

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a><span data-ttu-id="3e6c9-174">ASP.NET Scaffolding</span><span class="sxs-lookup"><span data-stu-id="3e6c9-174">ASP.NET Scaffolding</span></span>

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a><span data-ttu-id="3e6c9-175">MVC 和 Web API Scaffolding-HTTP 404 找不到錯誤</span><span class="sxs-lookup"><span data-stu-id="3e6c9-175">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>

<span data-ttu-id="3e6c9-176">如果您遇到錯誤，將 scaffold 項目加入至專案時，很可能您的專案將會處於不一致的狀態。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-176">If you encounter an error when adding a scaffolded item to a project, it is possible your project will be left in an inconsistent state.</span></span> <span data-ttu-id="3e6c9-177">部分 scaffolding 所做的變更將會回復，但其他變更，例如安裝的 NuGet 套件，將不會回復。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-177">Some of the changes made be scaffolding will be rolled back but other changes, such as the installed NuGet packages, will not be rolled back.</span></span> <span data-ttu-id="3e6c9-178">如果路由的組態變更會回復，使用者會收到 HTTP 404 錯誤，當巡覽至 scaffold 項目。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-178">If the routing configuration changes are rolled back, users will receive an HTTP 404 error when navigating to scaffolded items.</span></span>

<span data-ttu-id="3e6c9-179">若要修正這個錯誤 mvc 中，加入新的 scaffold 項目，然後選取 MVC 5 相依性 （基本或完整）。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-179">To fix this error for MVC, add a new scaffolded item and select MVC 5 Dependencies (either Minimal or Full).</span></span> <span data-ttu-id="3e6c9-180">此程序將會加入所有必要的變更您的專案。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-180">This process will add all of the required changes to your project.</span></span>

<span data-ttu-id="3e6c9-181">若要修正這個錯誤 Web api:</span><span class="sxs-lookup"><span data-stu-id="3e6c9-181">To fix this error for Web API:</span></span>

1. <span data-ttu-id="3e6c9-182">將下列到 WebApiConfig 類別加入至您的專案。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-182">Add the following WebApiConfig class to your project.</span></span>

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. <span data-ttu-id="3e6c9-183">在應用程式中設定 WebApiConfig.Register\_，如下所示在 Global.asax 中啟動方法：</span><span class="sxs-lookup"><span data-stu-id="3e6c9-183">Configure WebApiConfig.Register in the Application\_Start method in Global.asax as follows:</span></span>

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a><span data-ttu-id="3e6c9-184">Visual Studio Express 2012 for Web 加入 scaffold 項目之後會停止運作</span><span class="sxs-lookup"><span data-stu-id="3e6c9-184">Visual Studio Express 2012 for Web stops working after adding a scaffolded item</span></span>

<span data-ttu-id="3e6c9-185">如果 Visual Studio Express 2012 for Web 加入 scaffold 項目 （例如 Web API 2 控制器動作，使用 Entity Framework） 的 Entity framework 之後，會停止運作，有可能，Visual Studio Express 中無法載入組件的原生映像相依於 System.Web.Extensions。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-185">If Visual Studio Express 2012 for Web stops working after adding scaffolded item with Entity Framework (such as Web API 2 Controller with actions, using Entity Framework), it is possible that Visual Studio Express failed to load the native image of an assembly dependent on System.Web.Extensions.</span></span>

<span data-ttu-id="3e6c9-186">若要修正此問題，請設定 System.Web.Extensions 的 MSIL 映像所使用的 Visual Studio Express:</span><span class="sxs-lookup"><span data-stu-id="3e6c9-186">To correct this problem, configure Visual Studio Express to work with the MSIL image of System.Web.Extensions:</span></span>

1. <span data-ttu-id="3e6c9-187">以系統管理員模式開啟命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-187">Open Command Prompt in the Administrator mode.</span></span>
2. <span data-ttu-id="3e6c9-188">移至 %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE 或 %programfiles(x86) %\Microsoft Visual Studio 11.0\Common7\IDE （適用於 64 位元 Windows）。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-188">Go to %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE or %ProgramFiles(x86)%\Microsoft Visual Studio 11.0\Common7\IDE (for 64 bit Windows).</span></span>
3. <span data-ttu-id="3e6c9-189">在文字編輯器中開啟 VWDExpress.exe.config。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-189">Open VWDExpress.exe.config in a text editor.</span></span>
4. <span data-ttu-id="3e6c9-190">加入下的面這行&lt;組態&gt;/&lt;執行階段&gt;項目：</span><span class="sxs-lookup"><span data-stu-id="3e6c9-190">Add the following line under the &lt;configuration&gt;/&lt;runtime&gt; element:</span></span>  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. <span data-ttu-id="3e6c9-191">重新啟動 Visual Studio Express 2012 for Web。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-191">Restart Visual Studio Express 2012 for Web.</span></span>

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a><span data-ttu-id="3e6c9-192">ASP.NET Razor 3</span><span class="sxs-lookup"><span data-stu-id="3e6c9-192">ASP.NET Razor 3</span></span>

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-withbrowse-withorf5causes-a-server-error"></a><span data-ttu-id="3e6c9-193">檢視 cshtml 檔案 withBrowse WithorF5causes 伺服器錯誤</span><span class="sxs-lookup"><span data-stu-id="3e6c9-193">Viewing cshtml file withBrowse WithorF5causes a server error</span></span>

<span data-ttu-id="3e6c9-194">當您在 Visual Studio 2012 （或在 Visual Studio 2012 的 MVC 5 建立專案，Visual Studio 2013 中開啟） 中建立的 MVC 5 專案，並嘗試使用瀏覽或 F5 cshtml 檔案即可檢視時，您會收到錯誤指出-**中發生伺服器錯誤'/' 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-194">When you create an MVC 5 project in Visual Studio 2012 (or open in Visual Studio 2012 an MVC 5 project that was created in Visual Studio 2013) and attempt to view a cshtml file by using Browse With or F5, you will receive an error that states - **Server Error in '/' Application**.</span></span> <span data-ttu-id="3e6c9-195">伺服器會嘗試瀏覽至 `http://localhost:XXXX/Views/../XXXX.cshtml`</span><span class="sxs-lookup"><span data-stu-id="3e6c9-195">The server attempts to navigate to `http://localhost:XXXX/Views/../XXXX.cshtml`</span></span>

<span data-ttu-id="3e6c9-196">若要解決此問題，請變更**起始動作**在您的專案中設定**特定頁面**。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-196">To resolve this issue, change the **Start Action** setting in your project to **Specific Page**.</span></span> <span data-ttu-id="3e6c9-197">您不需要頁面提供的值。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-197">You do not need to provide a value for the page.</span></span>

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

<span data-ttu-id="3e6c9-198">這項變更之後，請選擇 f5 鍵巡覽至應用程式的根目錄 (`http://localhost:XXXX`)。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-198">After making this change, selecting F5 navigates to the root of your application (`http://localhost:XXXX`).</span></span> <span data-ttu-id="3e6c9-199">此行為不相同專案行為的 MVC 5 在 Visual Studio 2013 中，其中**本頁**設定啟動開啟的頁面。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-199">This behavior is not the same as the behavior for MVC 5 projects in Visual Studio 2013, where the **Current Page** setting launches the open page.</span></span>

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a><span data-ttu-id="3e6c9-200">Url 重寫和 Tilde(~)</span><span class="sxs-lookup"><span data-stu-id="3e6c9-200">Url Rewrite and Tilde(~)</span></span>

<span data-ttu-id="3e6c9-201">升級至 ASP.NET Razor 3 或 ASP.NET MVC 5 之後，tilde(~) 標記法可能無法再正確運作如果您使用 URL 重。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-201">After upgrading to ASP.NET Razor 3 or ASP.NET MVC 5, the tilde(~) notation may no longer work correctly if you are using URL rewrites.</span></span> <span data-ttu-id="3e6c9-202">URL 重寫影響 tilde(~) 標記法中的 HTML 項目例如&lt;A /&gt;，&lt;指令碼 /&gt;，&lt;連結 /&gt;，並因此波狀符號不會再將對應至目錄的根目錄。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-202">The URL rewrite affects the tilde(~) notation in HTML elements such as &lt;A/&gt;, &lt;SCRIPT/&gt;, &lt;LINK/&gt;, and as a result the tilde no longer maps to the root directory.</span></span>

<span data-ttu-id="3e6c9-203">比方說，如果您重新撰寫要求**asp.net/content**至**asp.net**中的 href 屬性&lt;A href ="~/content/"/&gt;解析成 **/content/內容 /** 而不是**/**。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-203">For example, if you rewrite requests for **asp.net/content** to **asp.net**, the href attribute in &lt;A href="~/content/"/&gt; resolves to **/content/content/** instead of **/**.</span></span> <span data-ttu-id="3e6c9-204">若要隱藏這項變更，您可以設定**IIS\_WasUrlRewritten**內容為 false，在每個網頁或**應用程式\_BeginRequest**在 Global.asax 中。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-204">To suppress this change, you can set the **IIS\_WasUrlRewritten** context to false in each Web Page or in **Application\_BeginRequest** in Global.asax.</span></span>

<a id="templateissue"></a>
### <a name="templates"></a><span data-ttu-id="3e6c9-205">範本</span><span class="sxs-lookup"><span data-stu-id="3e6c9-205">Templates</span></span>

<span data-ttu-id="3e6c9-206">當您建立 ASP.NET MVC 專案使用 Windows 8.1 或 Windows Server 2012 R2，Visual Studio 的 Visual Studio 2012 會顯示錯誤訊息，指出 「 設定 Web [url] 針對 ASP.NET 4.5 失敗。 」</span><span class="sxs-lookup"><span data-stu-id="3e6c9-206">When you create ASP.NET MVC projects with Visual Studio 2012 on Windows 8.1 or Windows Server 2012 R2, Visual Studio displays an error message that states "Configuring Web [url] for ASP.NET 4.5 failed."</span></span>

![組態錯誤](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

<span data-ttu-id="3e6c9-208">因為 Visual Studio 2012 時安裝這些版本的 Windows 上不會啟用 ASP.NET 4.5 功能，您會看到此錯誤。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-208">You see this error because Visual Studio 2012 does not enable the ASP.NET 4.5 feature when it is installed on those releases of Windows.</span></span> <span data-ttu-id="3e6c9-209">若要啟用 ASP.NET 4.5，執行中所述的步驟[開啟或關閉 Windows 功能](https://windows.microsoft.com/windows-8/turn-windows-features-on-off)。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-209">To enable ASP.NET 4.5, perform the steps described in [Turn Windows features on or off](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).</span></span>

![開啟或關閉 Windows 功能](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

<span data-ttu-id="3e6c9-211">或者，您可以透過命令列啟用 ASP.NET 4.5。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-211">Alternatively, you can enable ASP.NET 4.5 through the command line.</span></span>

1. <span data-ttu-id="3e6c9-212">以系統管理員模式開啟命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-212">Open Command Prompt in the Administrator mode.</span></span>
2. <span data-ttu-id="3e6c9-213">執行下列命令以啟用 ASP.NET 4.5。</span><span class="sxs-lookup"><span data-stu-id="3e6c9-213">Run the following command to enable ASP.NET 4.5.</span></span>  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
