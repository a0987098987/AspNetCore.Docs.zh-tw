---
title: ASP.NET Core 2.0 的新功能
author: rick-anderson
description: 深入了解 ASP.NET Core 2.0 的新功能。
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
uid: aspnetcore-2.0
ms.openlocfilehash: 452ccd76eece55cb5cf38fe39781f2f64dd5d466
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/06/2019
ms.locfileid: "74880867"
---
# <a name="whats-new-in-aspnet-core-20"></a><span data-ttu-id="09999-103">ASP.NET Core 2.0 的新功能</span><span class="sxs-lookup"><span data-stu-id="09999-103">What's new in ASP.NET Core 2.0</span></span>

<span data-ttu-id="09999-104">本文會重點說明 ASP.NET Core 2.0 最重要的變更，附有相關文件的連結。</span><span class="sxs-lookup"><span data-stu-id="09999-104">This article highlights the most significant changes in ASP.NET Core 2.0, with links to relevant documentation.</span></span>

## <a name="razor-pages"></a><span data-ttu-id="09999-105">Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="09999-105">Razor Pages</span></span>

<span data-ttu-id="09999-106">Razor Pages 是 ASP.NET Core MVC 的新功能，更容易編寫以頁面為焦點的案例程式碼，也更具生產力。</span><span class="sxs-lookup"><span data-stu-id="09999-106">Razor Pages is a new feature of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="09999-107">如需詳細資訊，請參閱簡介與教學課程：</span><span class="sxs-lookup"><span data-stu-id="09999-107">For more information, see the introduction and tutorial:</span></span>

* [<span data-ttu-id="09999-108">Razor 頁面簡介</span><span class="sxs-lookup"><span data-stu-id="09999-108">Introduction to Razor Pages</span></span>](xref:razor-pages/index)
* [<span data-ttu-id="09999-109">開始使用 Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="09999-109">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)

## <a name="aspnet-core-metapackage"></a><span data-ttu-id="09999-110">ASP.NET Core 中繼套件</span><span class="sxs-lookup"><span data-stu-id="09999-110">ASP.NET Core metapackage</span></span>

<span data-ttu-id="09999-111">新的 ASP.NET Core 中繼套件包含 ASP.NET Core 與 Entity Framework Core 小組建立與支援的所有套件，以及其內部和協力廠商的相依性。</span><span class="sxs-lookup"><span data-stu-id="09999-111">A new ASP.NET Core metapackage includes all of the packages made and supported by the ASP.NET Core and Entity Framework Core teams, along with their internal and 3rd-party dependencies.</span></span> <span data-ttu-id="09999-112">您不再需要依套件選擇個別的 ASP.NET Core 功能。</span><span class="sxs-lookup"><span data-stu-id="09999-112">You no longer need to choose individual ASP.NET Core features by package.</span></span> <span data-ttu-id="09999-113">所有功能都包含在 [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) 套件中。</span><span class="sxs-lookup"><span data-stu-id="09999-113">All features are included in the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) package.</span></span> <span data-ttu-id="09999-114">預設範本會使用此套件。</span><span class="sxs-lookup"><span data-stu-id="09999-114">The default templates use this package.</span></span>

<span data-ttu-id="09999-115">如需詳細資訊，請參閱 [ASP.NET Core 2.0 的 Microsoft.AspNetCore.All 中繼套件](xref:fundamentals/metapackage)。</span><span class="sxs-lookup"><span data-stu-id="09999-115">For more information, see [Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.0](xref:fundamentals/metapackage).</span></span>

## <a name="runtime-store"></a><span data-ttu-id="09999-116">執行階段存放區</span><span class="sxs-lookup"><span data-stu-id="09999-116">Runtime Store</span></span>

<span data-ttu-id="09999-117">使用 `Microsoft.AspNetCore.All` 中繼套件的應用程式會自動利用新的 .NET Core 執行階段存放區。</span><span class="sxs-lookup"><span data-stu-id="09999-117">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the new .NET Core Runtime Store.</span></span> <span data-ttu-id="09999-118">此存放區包含執行 ASP.NET Core 2.0 應用程式所需的所有執行階段資產。</span><span class="sxs-lookup"><span data-stu-id="09999-118">The Store contains all the runtime assets needed to run ASP.NET Core 2.0 applications.</span></span> <span data-ttu-id="09999-119">當您使用 `Microsoft.AspNetCore.All` 中繼套件時，不會使用應用程式部署參考 ASP.NET Core NuGet 套件的任何資產，因為它們已經位於目標系統上。</span><span class="sxs-lookup"><span data-stu-id="09999-119">When you use the `Microsoft.AspNetCore.All` metapackage, no assets from the referenced ASP.NET Core NuGet packages are deployed with the application because they already reside on the target system.</span></span> <span data-ttu-id="09999-120">執行階段存放區中的資產也會先行編譯，以改善應用程式啟動時間。</span><span class="sxs-lookup"><span data-stu-id="09999-120">The assets in the Runtime Store are also precompiled to improve application startup time.</span></span>

<span data-ttu-id="09999-121">如需詳細資訊，請參閱[執行階段存放區](/dotnet/core/deploying/runtime-store)。</span><span class="sxs-lookup"><span data-stu-id="09999-121">For more information, see [Runtime store](/dotnet/core/deploying/runtime-store)</span></span>

## <a name="net-standard-20"></a><span data-ttu-id="09999-122">.NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="09999-122">.NET Standard 2.0</span></span>

<span data-ttu-id="09999-123">ASP.NET Core 2.0 套件以 .NET Standard 2.0 為目標。</span><span class="sxs-lookup"><span data-stu-id="09999-123">The ASP.NET Core 2.0 packages target .NET Standard 2.0.</span></span> <span data-ttu-id="09999-124">套件可供其他 .NET Standard 2.0 程式庫參考，並可在與 .NET Standard 2.0 相容的 .NET 實作上執行，包括 .NET Core 2.0 和 .NET Framework 4.6.1。</span><span class="sxs-lookup"><span data-stu-id="09999-124">The packages can be referenced by other .NET Standard 2.0 libraries, and they can run on .NET Standard 2.0-compliant implementations of .NET, including .NET Core 2.0 and .NET Framework 4.6.1.</span></span> 

<span data-ttu-id="09999-125">因為 `Microsoft.AspNetCore.All` 中繼套件預計搭配 .NET Core 2.0 執行階段存放區使用，所以只鎖定 .NET Core 2.0 作為目標。</span><span class="sxs-lookup"><span data-stu-id="09999-125">The `Microsoft.AspNetCore.All` metapackage targets .NET Core 2.0 only, because it's intended to be used with the .NET Core 2.0 Runtime Store.</span></span>

## <a name="configuration-update"></a><span data-ttu-id="09999-126">組態更新</span><span class="sxs-lookup"><span data-stu-id="09999-126">Configuration update</span></span>

<span data-ttu-id="09999-127">`IConfiguration` 執行個體預設新增至 ASP.NET Core 2.0 的服務容器中。</span><span class="sxs-lookup"><span data-stu-id="09999-127">An `IConfiguration` instance is added to the services container by default in ASP.NET Core 2.0.</span></span> <span data-ttu-id="09999-128">服務容器中的 `IConfiguration` 可讓應用程式從容器輕鬆擷取組態值。</span><span class="sxs-lookup"><span data-stu-id="09999-128">`IConfiguration` in the services container makes it easier for applications to retrieve configuration values from the container.</span></span>

<span data-ttu-id="09999-129">如需已規劃文件狀態的資訊，請參閱 [GitHub 問題](https://github.com/aspnet/AspNetCore.Docs/issues/3387)。</span><span class="sxs-lookup"><span data-stu-id="09999-129">For information about the status of planned documentation, see the [GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/3387).</span></span>

## <a name="logging-update"></a><span data-ttu-id="09999-130">記錄更新</span><span class="sxs-lookup"><span data-stu-id="09999-130">Logging update</span></span>

<span data-ttu-id="09999-131">在 ASP.NET Core 2.0 中，記錄預設會合併至相依性插入 (DI) 系統。</span><span class="sxs-lookup"><span data-stu-id="09999-131">In ASP.NET Core 2.0, logging is incorporated into the dependency injection (DI) system by default.</span></span> <span data-ttu-id="09999-132">您要在 *Program.cs* 檔案中新增提供者並設定篩選，不是在 *Startup.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="09999-132">You add providers and configure filtering in the *Program.cs* file instead of in the *Startup.cs* file.</span></span> <span data-ttu-id="09999-133">而預設的 `ILoggerFactory` 支援篩選的方式，可讓您使用一個彈性的方法同時應對跨提供者的篩選和特定提供者的篩選。</span><span class="sxs-lookup"><span data-stu-id="09999-133">And the default `ILoggerFactory` supports filtering in a way that lets you use one flexible approach for both cross-provider filtering and specific-provider filtering.</span></span>

<span data-ttu-id="09999-134">如需詳細資訊，請參閱[記錄簡介](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="09999-134">For more information, see [Introduction to Logging](xref:fundamentals/logging/index).</span></span>

## <a name="authentication-update"></a><span data-ttu-id="09999-135">驗證更新</span><span class="sxs-lookup"><span data-stu-id="09999-135">Authentication update</span></span>

<span data-ttu-id="09999-136">新的驗證模型讓使用 DI 的應用程式更容易設定驗證。</span><span class="sxs-lookup"><span data-stu-id="09999-136">A new authentication model makes it easier to configure authentication for an application using DI.</span></span>

<span data-ttu-id="09999-137">新範本可用於為使用 [Azure AD B2C](https://azure.microsoft.com/services/active-directory-b2c/) 之 Web 應用程式及 Web API 設定驗證。</span><span class="sxs-lookup"><span data-stu-id="09999-137">New templates are available for configuring authentication for web apps and web APIs using [Azure AD B2C](https://azure.microsoft.com/services/active-directory-b2c/).</span></span>

<span data-ttu-id="09999-138">如需已規劃文件狀態的資訊，請參閱 [GitHub 問題](https://github.com/aspnet/AspNetCore.Docs/issues/3054)。</span><span class="sxs-lookup"><span data-stu-id="09999-138">For information about the status of planned documentation, see the [GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/3054).</span></span>

## <a name="identity-update"></a><span data-ttu-id="09999-139">身分識別更新</span><span class="sxs-lookup"><span data-stu-id="09999-139">Identity update</span></span>

<span data-ttu-id="09999-140">我們讓建置使用 ASP.NET Core 2.0 身分識別的安全 Web API 變得更容易。</span><span class="sxs-lookup"><span data-stu-id="09999-140">We've made it easier to build secure web APIs using Identity in ASP.NET Core 2.0.</span></span> <span data-ttu-id="09999-141">您可以取得存取權杖來存取使用 [Microsoft 驗證程式庫 (MSAL)](https://www.nuget.org/packages/Microsoft.Identity.Client) 的 Web API。</span><span class="sxs-lookup"><span data-stu-id="09999-141">You can acquire access tokens for accessing your web APIs using the [Microsoft Authentication Library (MSAL)](https://www.nuget.org/packages/Microsoft.Identity.Client).</span></span>

<span data-ttu-id="09999-142">如需 2.0 驗證變更的詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="09999-142">For more information on authentication changes in 2.0, see the following resources:</span></span>

* [<span data-ttu-id="09999-143">ASP.NET Core 中的帳戶確認和密碼復原</span><span class="sxs-lookup"><span data-stu-id="09999-143">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="09999-144">允許為 ASP.NET Core 中的驗證器應用程式產生 QR 代碼</span><span class="sxs-lookup"><span data-stu-id="09999-144">Enable QR Code generation for authenticator apps in ASP.NET Core</span></span>](xref:security/authentication/identity-enable-qrcodes)
* [<span data-ttu-id="09999-145">將驗證及識別移轉至 ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="09999-145">Migrate Authentication and Identity to ASP.NET Core 2.0</span></span>](xref:migration/1x-to-2x/identity-2x)

## <a name="spa-templates"></a><span data-ttu-id="09999-146">SPA 範本</span><span class="sxs-lookup"><span data-stu-id="09999-146">SPA templates</span></span>

<span data-ttu-id="09999-147">有具有 Redux 的 Angular、Aurelia、Knockout.js、React.js 和 React.js 單一頁面應用程式 (SPA) 專案範本可用。</span><span class="sxs-lookup"><span data-stu-id="09999-147">Single Page Application (SPA) project templates for Angular, Aurelia, Knockout.js, React.js, and React.js with Redux are available.</span></span> <span data-ttu-id="09999-148">Angular 範本已更新至 Angular 4。</span><span class="sxs-lookup"><span data-stu-id="09999-148">The Angular template has been updated to Angular 4.</span></span> <span data-ttu-id="09999-149">根據預設將會提供 Angular 與 React 範本。如需如何取得其他範本的資訊，請參閱[建立新的 SPA 專案](xref:client-side/spa-services#create-a-new-project)。</span><span class="sxs-lookup"><span data-stu-id="09999-149">The Angular and React templates are available by default; for information about how to get the other templates, see [Create a new SPA project](xref:client-side/spa-services#create-a-new-project).</span></span> <span data-ttu-id="09999-150">如需有關如何在 ASP.NET Core 中建置 SPA 的資訊，請參閱 <xref:client-side/spa-services>。</span><span class="sxs-lookup"><span data-stu-id="09999-150">For information about how to build a SPA in ASP.NET Core, see <xref:client-side/spa-services>.</span></span>

## <a name="kestrel-improvements"></a><span data-ttu-id="09999-151">Kestrel 改善</span><span class="sxs-lookup"><span data-stu-id="09999-151">Kestrel improvements</span></span>

<span data-ttu-id="09999-152">Kestrel 網頁伺服器的新功能，讓它更適合作為網際網路對向伺服器。</span><span class="sxs-lookup"><span data-stu-id="09999-152">The Kestrel web server has new features that make it more suitable as an Internet-facing server.</span></span> <span data-ttu-id="09999-153">已在 `KestrelServerOptions` 類別的新 `Limits` 屬性中新增多個伺服器條件約束組態選項。</span><span class="sxs-lookup"><span data-stu-id="09999-153">A number of server constraint configuration options are added in the `KestrelServerOptions` class's new `Limits` property.</span></span> <span data-ttu-id="09999-154">請新增下列限制：</span><span class="sxs-lookup"><span data-stu-id="09999-154">Add limits for the following:</span></span>

* <span data-ttu-id="09999-155">用戶端連線數目上限</span><span class="sxs-lookup"><span data-stu-id="09999-155">Maximum client connections</span></span>
* <span data-ttu-id="09999-156">要求主體大小上限</span><span class="sxs-lookup"><span data-stu-id="09999-156">Maximum request body size</span></span>
* <span data-ttu-id="09999-157">要求主體資料速率下限</span><span class="sxs-lookup"><span data-stu-id="09999-157">Minimum request body data rate</span></span>

<span data-ttu-id="09999-158">如需詳細資訊，請參閱[在 ASP.NET Core 中實作 Kestrel 網頁伺服器](xref:fundamentals/servers/kestrel)。</span><span class="sxs-lookup"><span data-stu-id="09999-158">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel).</span></span>

## <a name="weblistener-renamed-to-httpsys"></a><span data-ttu-id="09999-159">WebListener 已重新命名為 HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="09999-159">WebListener renamed to HTTP.sys</span></span>

<span data-ttu-id="09999-160">套件 `Microsoft.AspNetCore.Server.WebListener` 和 `Microsoft.Net.Http.Server` 已合併至新的套件 `Microsoft.AspNetCore.Server.HttpSys`。</span><span class="sxs-lookup"><span data-stu-id="09999-160">The packages `Microsoft.AspNetCore.Server.WebListener` and `Microsoft.Net.Http.Server` have been merged into a new package `Microsoft.AspNetCore.Server.HttpSys`.</span></span> <span data-ttu-id="09999-161">命名空間已更新成符合項目。</span><span class="sxs-lookup"><span data-stu-id="09999-161">The namespaces have been updated to match.</span></span>

<span data-ttu-id="09999-162">如需詳細資訊，請參閱[在 ASP.NET Core 中實作 HTTP.sys 網頁伺服器](xref:fundamentals/servers/httpsys)。</span><span class="sxs-lookup"><span data-stu-id="09999-162">For more information, see [HTTP.sys web server implementation in ASP.NET Core](xref:fundamentals/servers/httpsys).</span></span>

## <a name="enhanced-http-header-support"></a><span data-ttu-id="09999-163">增強的 HTTP 標頭支援</span><span class="sxs-lookup"><span data-stu-id="09999-163">Enhanced HTTP header support</span></span>

<span data-ttu-id="09999-164">使用 MVC 傳輸 `FileStreamResult` 或 `FileContentResult` 時，您現在可以選擇在傳輸內容上設定 `ETag` 或 `LastModified` 日期。</span><span class="sxs-lookup"><span data-stu-id="09999-164">When using MVC to transmit a `FileStreamResult` or a `FileContentResult`, you now have the option to set an `ETag` or a `LastModified` date on the content you transmit.</span></span> <span data-ttu-id="09999-165">您可以在傳回的內容上設定這些值，使用與下例類似的程式碼：</span><span class="sxs-lookup"><span data-stu-id="09999-165">You can set these values on the returned content with code similar to the following:</span></span>

```csharp
var data = Encoding.UTF8.GetBytes("This is a sample text from a binary array");
var entityTag = new EntityTagHeaderValue("\"MyCalculatedEtagValue\"");
return File(data, "text/plain", "downloadName.txt", lastModified: DateTime.UtcNow.AddSeconds(-5), entityTag: entityTag);
```

<span data-ttu-id="09999-166">傳回給您的訪客的檔案具有 `ETag` 和 `LastModified` 值的適當 HTTP 標頭。</span><span class="sxs-lookup"><span data-stu-id="09999-166">The file returned to your visitors has the appropriate HTTP headers for the `ETag` and `LastModified` values.</span></span>

<span data-ttu-id="09999-167">如果應用程式訪客要求內容與範圍要求標頭，ASP.NET Core 會辨識要求並處理標頭。</span><span class="sxs-lookup"><span data-stu-id="09999-167">If an application visitor requests content with a Range Request header, ASP.NET Core recognizes the request and handles the header.</span></span> <span data-ttu-id="09999-168">如果要求的內容可以部分傳送，ASP.NET Core 會適當略過，並只傳回要求的位元組集合。</span><span class="sxs-lookup"><span data-stu-id="09999-168">If the requested content can be partially delivered, ASP.NET Core appropriately skips and returns just the requested set of bytes.</span></span> <span data-ttu-id="09999-169">您不需要將任何特殊的處理常式寫入方法，來調整或處理這項功能；會為您自動處理。</span><span class="sxs-lookup"><span data-stu-id="09999-169">You don't need to write any special handlers into your methods to adapt or handle this feature; it's automatically handled for you.</span></span>

## <a name="hosting-startup-and-application-insights"></a><span data-ttu-id="09999-170">裝載啟動和 Application Insights</span><span class="sxs-lookup"><span data-stu-id="09999-170">Hosting startup and Application Insights</span></span>

<span data-ttu-id="09999-171">裝載環境現在可以在應用程式啟動時，插入額外的套件相依性並執行程式碼，應用程式不需要明確接受相依性或呼叫任何方法。</span><span class="sxs-lookup"><span data-stu-id="09999-171">Hosting environments can now inject extra package dependencies and execute code during application startup, without the application needing to explicitly take a dependency or call any methods.</span></span> <span data-ttu-id="09999-172">這項功能可以用來啟用特定的環境以「點亮」該環境特有的功能，應用程式不需要事先知道。</span><span class="sxs-lookup"><span data-stu-id="09999-172">This feature can be used to enable certain environments to "light-up" features unique to that environment without the application needing to know ahead of time.</span></span> 

<span data-ttu-id="09999-173">在 ASP.NET Core 2.0 中，在 Visual Studio 中偵錯以及 (加入後) 在 Azure 應用程式服務中執行時，此功能可用來自動啟用 Application Insights 診斷。</span><span class="sxs-lookup"><span data-stu-id="09999-173">In ASP.NET Core 2.0, this feature is used to automatically enable Application Insights diagnostics when debugging in Visual Studio and (after opting in) when running in Azure App Services.</span></span> <span data-ttu-id="09999-174">如此一來，專案範本預設不會再新增 Application Insights 套件和程式碼。</span><span class="sxs-lookup"><span data-stu-id="09999-174">As a result, the project templates no longer add Application Insights packages and code by default.</span></span>

<span data-ttu-id="09999-175">如需已規劃文件狀態的資訊，請參閱 [GitHub 問題](https://github.com/aspnet/AspNetCore.Docs/issues/3389)。</span><span class="sxs-lookup"><span data-stu-id="09999-175">For information about the status of planned documentation, see the [GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/3389).</span></span>

## <a name="automatic-use-of-anti-forgery-tokens"></a><span data-ttu-id="09999-176">自動使用防偽權杖</span><span class="sxs-lookup"><span data-stu-id="09999-176">Automatic use of anti-forgery tokens</span></span>

<span data-ttu-id="09999-177">根據預設，ASP.NET Core 一律協助以 HTML 編碼的內容，但在新版本中，會採用額外的步驟，協助防止跨網站要求偽造 (XSRF) 攻擊。</span><span class="sxs-lookup"><span data-stu-id="09999-177">ASP.NET Core has always helped HTML-encode content by default, but with the new version an extra step is taken to help prevent cross-site request forgery (XSRF) attacks.</span></span> <span data-ttu-id="09999-178">ASP.NET Core 現在預設會發出防偽權杖，並對表單 POST 動作和頁面驗證它們，不需要額外組態。</span><span class="sxs-lookup"><span data-stu-id="09999-178">ASP.NET Core will now emit anti-forgery tokens by default and validate them on form POST actions and pages without extra configuration.</span></span>

<span data-ttu-id="09999-179">如需詳細資訊，請參閱[防止跨站台要求偽造 (XSRF/CSRF) 攻擊](xref:security/anti-request-forgery)。</span><span class="sxs-lookup"><span data-stu-id="09999-179">For more information, see [Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks](xref:security/anti-request-forgery).</span></span>

## <a name="automatic-precompilation"></a><span data-ttu-id="09999-180">自動先行編譯</span><span class="sxs-lookup"><span data-stu-id="09999-180">Automatic precompilation</span></span>

<span data-ttu-id="09999-181">預設在發佈期間即已啟用 Razor 檢視預先編譯，減少發佈輸出大小和應用程式啟動時間。</span><span class="sxs-lookup"><span data-stu-id="09999-181">Razor view pre-compilation is enabled during publish by default, reducing the publish output size and application startup time.</span></span>

<span data-ttu-id="09999-182">如需詳細資訊，請參閱 [ASP.NET Core 中的 Razor 檢視編譯與先行編譯](xref:mvc/views/view-compilation)。</span><span class="sxs-lookup"><span data-stu-id="09999-182">For more information, see [Razor view compilation and precompilation in ASP.NET Core](xref:mvc/views/view-compilation).</span></span>

## <a name="razor-support-for-c-71"></a><span data-ttu-id="09999-183">針對 C# 7.1 的 Razor 支援</span><span class="sxs-lookup"><span data-stu-id="09999-183">Razor support for C# 7.1</span></span>

<span data-ttu-id="09999-184">Razor 檢視引擎已更新，可使用新的 Roslyn 編譯器 。</span><span class="sxs-lookup"><span data-stu-id="09999-184">The Razor view engine has been updated to work with the new Roslyn compiler.</span></span> <span data-ttu-id="09999-185">這包括支援預設運算式、推斷 Tuple 名稱和使用泛型比對模式等 C# 7.1 功能。</span><span class="sxs-lookup"><span data-stu-id="09999-185">That includes support for C# 7.1 features like Default Expressions, Inferred Tuple Names, and Pattern-Matching with Generics.</span></span> <span data-ttu-id="09999-186">若要在專案中使用 C# 7.1，請在專案檔中新增下列屬性，然後重新載入方案：</span><span class="sxs-lookup"><span data-stu-id="09999-186">To use C# 7.1 in your project, add the following property in your project file and then reload the solution:</span></span>

```xml
<LangVersion>latest</LangVersion>
```

<span data-ttu-id="09999-187">如需 C# 7.1 功能狀態的資訊，請參閱 [Roslyn GitHub 存放庫](https://github.com/dotnet/roslyn/blob/master/docs/Language%20Feature%20Status.md)。</span><span class="sxs-lookup"><span data-stu-id="09999-187">For information about the status of C# 7.1 features, see [the Roslyn GitHub repository](https://github.com/dotnet/roslyn/blob/master/docs/Language%20Feature%20Status.md).</span></span>

## <a name="other-documentation-updates-for-20"></a><span data-ttu-id="09999-188">針對 2.0 的其他文件更新</span><span class="sxs-lookup"><span data-stu-id="09999-188">Other documentation updates for 2.0</span></span>

* [<span data-ttu-id="09999-189">適用於 ASP.NET 應用程式部署的 Visual Studio 發行設定檔</span><span class="sxs-lookup"><span data-stu-id="09999-189">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>](xref:host-and-deploy/visual-studio-publish-profiles)
* [<span data-ttu-id="09999-190">金鑰管理</span><span class="sxs-lookup"><span data-stu-id="09999-190">Key Management</span></span>](xref:security/data-protection/implementation/key-management)
* [<span data-ttu-id="09999-191">設定 Facebook 驗證</span><span class="sxs-lookup"><span data-stu-id="09999-191">Configure Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="09999-192">設定 Twitter 驗證</span><span class="sxs-lookup"><span data-stu-id="09999-192">Configure Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="09999-193">設定 Google 驗證</span><span class="sxs-lookup"><span data-stu-id="09999-193">Configure Google authentication</span></span>](xref:security/authentication/google-logins)
* [<span data-ttu-id="09999-194">設定 Microsoft 帳戶驗證</span><span class="sxs-lookup"><span data-stu-id="09999-194">Configure Microsoft Account authentication</span></span>](xref:security/authentication/microsoft-logins)

## <a name="migration-guidance"></a><span data-ttu-id="09999-195">移轉指引</span><span class="sxs-lookup"><span data-stu-id="09999-195">Migration guidance</span></span>

<span data-ttu-id="09999-196">如需如何將 ASP.NET Core 1.x 應用程式移轉至 ASP.NET Core 2.0的指引，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="09999-196">For guidance on how to migrate ASP.NET Core 1.x applications to ASP.NET Core 2.0, see the following resources:</span></span>

* [<span data-ttu-id="09999-197">從 ASP.NET Core 1.x 移轉至 ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="09999-197">Migrate from ASP.NET Core 1.x to ASP.NET Core 2.0</span></span>](xref:migration/1x-to-2x/index)
* [<span data-ttu-id="09999-198">將驗證及識別移轉至 ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="09999-198">Migrate Authentication and Identity to ASP.NET Core 2.0</span></span>](xref:migration/1x-to-2x/identity-2x)

## <a name="additional-information"></a><span data-ttu-id="09999-199">其他資訊</span><span class="sxs-lookup"><span data-stu-id="09999-199">Additional Information</span></span>

<span data-ttu-id="09999-200">如需完整的變更清單，請參閱 [ASP.NET Core 2.0 版本資訊](https://github.com/aspnet/Home/releases/tag/2.0.0)。</span><span class="sxs-lookup"><span data-stu-id="09999-200">For the complete list of changes, see the [ASP.NET Core 2.0 Release Notes](https://github.com/aspnet/Home/releases/tag/2.0.0).</span></span>

<span data-ttu-id="09999-201">若要了解 ASP.NET Core 開發小組的進度和計劃，請收聽 [ASP.NET Community Standup](https://live.asp.net/) (ASP.NET 社群之聲)。</span><span class="sxs-lookup"><span data-stu-id="09999-201">To connect with the ASP.NET Core development team's progress and plans, tune in to the [ASP.NET Community Standup](https://live.asp.net/).</span></span>
