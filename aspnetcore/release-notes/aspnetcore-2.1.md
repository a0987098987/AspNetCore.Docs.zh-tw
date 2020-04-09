---
title: ASP.NET Core 2.1 的新功能
author: isaac2004
description: 了解 ASP.NET Core 2.1 的新功能。
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- SignalR
uid: aspnetcore-2.1
ms.openlocfilehash: af5807b782d4acec8c7d40111dc508dfa6127057
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78667542"
---
# <a name="whats-new-in-aspnet-core-21"></a><span data-ttu-id="f95b6-103">ASP.NET Core 2.1 的新功能</span><span class="sxs-lookup"><span data-stu-id="f95b6-103">What's new in ASP.NET Core 2.1</span></span>

<span data-ttu-id="f95b6-104">本文會重點說明 ASP.NET Core 2.1 最重要的變更，附有相關文件的連結。</span><span class="sxs-lookup"><span data-stu-id="f95b6-104">This article highlights the most significant changes in ASP.NET Core 2.1, with links to relevant documentation.</span></span>

## SignalR

SignalR<span data-ttu-id="f95b6-105">已為ASP.NET核心2.1重寫。</span><span class="sxs-lookup"><span data-stu-id="f95b6-105"> has been rewritten for ASP.NET Core 2.1.</span></span> <span data-ttu-id="f95b6-106">ASP.NET核心SignalR包括多項改進:</span><span class="sxs-lookup"><span data-stu-id="f95b6-106">ASP.NET Core SignalR includes a number of improvements:</span></span>

* <span data-ttu-id="f95b6-107">簡化的向外延展模型。</span><span class="sxs-lookup"><span data-stu-id="f95b6-107">A simplified scale-out model.</span></span>
* <span data-ttu-id="f95b6-108">沒有 jQuery 相依性的新 JavaScript 用戶端。</span><span class="sxs-lookup"><span data-stu-id="f95b6-108">A new JavaScript client with no jQuery dependency.</span></span>
* <span data-ttu-id="f95b6-109">以 MessagePack 為基礎的新精簡二進位通訊協定。</span><span class="sxs-lookup"><span data-stu-id="f95b6-109">A new compact binary protocol based on MessagePack.</span></span>
* <span data-ttu-id="f95b6-110">支援自訂通訊協定。</span><span class="sxs-lookup"><span data-stu-id="f95b6-110">Support for custom protocols.</span></span>
* <span data-ttu-id="f95b6-111">新的資料流處理回應模型。</span><span class="sxs-lookup"><span data-stu-id="f95b6-111">A new streaming response model.</span></span>
* <span data-ttu-id="f95b6-112">支援以裸機 WebSockets 為基礎的用戶端。</span><span class="sxs-lookup"><span data-stu-id="f95b6-112">Support for clients based on bare WebSockets.</span></span>

<span data-ttu-id="f95b6-113">有關詳細資訊,請參閱[ASP.NETSignalR核心](xref:signalr/introduction)。</span><span class="sxs-lookup"><span data-stu-id="f95b6-113">For more information, see [ASP.NET Core SignalR](xref:signalr/introduction).</span></span>

## <a name="razor-class-libraries"></a><span data-ttu-id="f95b6-114">Razor 類別庫</span><span class="sxs-lookup"><span data-stu-id="f95b6-114">Razor class libraries</span></span>

<span data-ttu-id="f95b6-115">ASP.NET Core 2.1 讓您更容易在程式庫中建置並包含 Razor 型 UI，跨多個專案共用它。</span><span class="sxs-lookup"><span data-stu-id="f95b6-115">ASP.NET Core 2.1 makes it easier to build and include Razor-based UI in a library and share it across multiple projects.</span></span> <span data-ttu-id="f95b6-116">新的 Razor SDK 能在可封裝到 NuGet 套件的類別庫專案中建置 Razor 檔案。</span><span class="sxs-lookup"><span data-stu-id="f95b6-116">The new Razor SDK enables building Razor files into a class library project that can be packaged into a NuGet package.</span></span> <span data-ttu-id="f95b6-117">應用程式會自動探索並覆寫程式庫中的檢視和頁面。</span><span class="sxs-lookup"><span data-stu-id="f95b6-117">Views and pages in libraries are automatically discovered and can be overridden by the app.</span></span> <span data-ttu-id="f95b6-118">藉由將 Razor 編譯整合至組建：</span><span class="sxs-lookup"><span data-stu-id="f95b6-118">By integrating Razor compilation into the build:</span></span>

* <span data-ttu-id="f95b6-119">大幅縮短應用程式的啟動時間。</span><span class="sxs-lookup"><span data-stu-id="f95b6-119">The app startup time is significantly faster.</span></span>
* <span data-ttu-id="f95b6-120">快速更新執行階段的 Razor 檢視和頁面仍然屬於反覆開發工作流程的一部分。</span><span class="sxs-lookup"><span data-stu-id="f95b6-120">Fast updates to Razor views and pages at runtime are still available as part of an iterative development workflow.</span></span>

<span data-ttu-id="f95b6-121">如需詳細資訊，請參閱[使用 Razor 類別庫專案建立可重複使用的 UI](xref:razor-pages/ui-class)。</span><span class="sxs-lookup"><span data-stu-id="f95b6-121">For more information, see [Create reusable UI using the Razor Class Library project](xref:razor-pages/ui-class).</span></span>

## <a name="identity-ui-library--scaffolding"></a><span data-ttu-id="f95b6-122">身分識別 UI 程式庫與 Scaffolding</span><span class="sxs-lookup"><span data-stu-id="f95b6-122">Identity UI library & scaffolding</span></span>

<span data-ttu-id="f95b6-123">ASP.NET Core 2.1 將 [ASP.NET Core 身分識別](xref:security/authentication/identity)提供為 [Razor 類別庫](xref:razor-pages/ui-class)。</span><span class="sxs-lookup"><span data-stu-id="f95b6-123">ASP.NET Core 2.1 provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="f95b6-124">包含身分識別的應用程式可以套用新的身分識別 Scaffolder，以選擇性地新增包含在身分識別 Razor 類別庫 (RCL) 中的原始程式碼。</span><span class="sxs-lookup"><span data-stu-id="f95b6-124">Apps that include Identity can apply the new Identity scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="f95b6-125">建議您產生原始程式碼，以便能夠修改程式碼並變更行為。</span><span class="sxs-lookup"><span data-stu-id="f95b6-125">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="f95b6-126">例如，您可以指示 Scaffolder 產生註冊使用的程式碼。</span><span class="sxs-lookup"><span data-stu-id="f95b6-126">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="f95b6-127">產生的程式碼優先於身分識別 RCL 中的相同程式碼。</span><span class="sxs-lookup"><span data-stu-id="f95b6-127">Generated code takes precedence over the same code in the Identity RCL.</span></span>

<span data-ttu-id="f95b6-128">**不**包含驗證的應用程式可以套用身分識別 Scaffolder 以新增 RCL 身分識別套件。</span><span class="sxs-lookup"><span data-stu-id="f95b6-128">Apps that do **not** include authentication can apply the Identity scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="f95b6-129">您可以選擇選取要產生的身分識別程式碼。</span><span class="sxs-lookup"><span data-stu-id="f95b6-129">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="f95b6-130">如需詳細資訊，請參閱 [ASP.NET Core 專案中的 Scaffold 身分識別](xref:security/authentication/scaffold-identity)。</span><span class="sxs-lookup"><span data-stu-id="f95b6-130">For more information, see [Scaffold Identity in ASP.NET Core projects](xref:security/authentication/scaffold-identity).</span></span>

## <a name="https"></a><span data-ttu-id="f95b6-131">HTTPS</span><span class="sxs-lookup"><span data-stu-id="f95b6-131">HTTPS</span></span>

<span data-ttu-id="f95b6-132">隨著安全性與隱私權日益受到重視，為 Web 應用程式啟用 HTTPS 極其重要。</span><span class="sxs-lookup"><span data-stu-id="f95b6-132">With the increased focus on security and privacy, enabling HTTPS for web apps is important.</span></span> <span data-ttu-id="f95b6-133">在網站上強制執行 HTTPS 變得日益嚴格。</span><span class="sxs-lookup"><span data-stu-id="f95b6-133">HTTPS enforcement is becoming increasingly strict on the web.</span></span> <span data-ttu-id="f95b6-134">不使用 HHH 的網站被視為不安全。</span><span class="sxs-lookup"><span data-stu-id="f95b6-134">Sites that don't use HTTPS are considered insecure.</span></span> <span data-ttu-id="f95b6-135">瀏覽器 (Chromium、Mozilla) 開始強制執行必須從安全內容使用 Web 功能。</span><span class="sxs-lookup"><span data-stu-id="f95b6-135">Browsers (Chromium, Mozilla) are starting to enforce that web features must be used from a secure context.</span></span> <span data-ttu-id="f95b6-136">[GDPR](xref:security/gdpr) 需要使用 HTTPS 來保護使用者隱私。</span><span class="sxs-lookup"><span data-stu-id="f95b6-136">[GDPR](xref:security/gdpr) requires the use of HTTPS to protect user privacy.</span></span> <span data-ttu-id="f95b6-137">雖然在生產環境中使用 HTTPS 非常重要，但在開發中使用 HTTPS 有助於避免發生部署問題 (例如不安全的連結)。</span><span class="sxs-lookup"><span data-stu-id="f95b6-137">While using HTTPS in production is critical, using HTTPS in development can help prevent issues in deployment (for example, insecure links).</span></span> <span data-ttu-id="f95b6-138">ASP.NET Core 2.1 包含多項改善，讓您更容易在開發中使用 HTTPS，以及在生產環境中設定 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="f95b6-138">ASP.NET Core 2.1 includes a number of improvements that make it easier to use HTTPS in development and to configure HTTPS in production.</span></span> <span data-ttu-id="f95b6-139">如需詳細資訊，請參閱[強制使用 HTTPS](xref:security/enforcing-ssl)。</span><span class="sxs-lookup"><span data-stu-id="f95b6-139">For more information, see [Enforce HTTPS](xref:security/enforcing-ssl).</span></span>

### <a name="on-by-default"></a><span data-ttu-id="f95b6-140">依預設開啟</span><span class="sxs-lookup"><span data-stu-id="f95b6-140">On by default</span></span>

<span data-ttu-id="f95b6-141">為方便保護網站開發，HTTPS 現在預設為啟用。</span><span class="sxs-lookup"><span data-stu-id="f95b6-141">To facilitate secure website development, HTTPS  is now enabled by default.</span></span> <span data-ttu-id="f95b6-142">自 2.1 開始，當有本機開發憑證存在時，Kestrel 會接聽 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="f95b6-142">Starting in 2.1, Kestrel listens on `https://localhost:5001` when a local development certificate is present.</span></span> <span data-ttu-id="f95b6-143">開發憑證會建立於：</span><span class="sxs-lookup"><span data-stu-id="f95b6-143">A development certificate is created:</span></span>

* <span data-ttu-id="f95b6-144">您第一次使用 SDK，是 .NET Core SDK 首次執行體驗的一部分。</span><span class="sxs-lookup"><span data-stu-id="f95b6-144">As part of the .NET Core SDK first-run experience, when you use the SDK for the first time.</span></span>
* <span data-ttu-id="f95b6-145">手動使用新的 `dev-certs` 工具。</span><span class="sxs-lookup"><span data-stu-id="f95b6-145">Manually using the new `dev-certs` tool.</span></span>

<span data-ttu-id="f95b6-146">執行 `dotnet dev-certs https --trust` 以信任憑證。</span><span class="sxs-lookup"><span data-stu-id="f95b6-146">Run `dotnet dev-certs https --trust` to trust the certificate.</span></span>

### <a name="https-redirection-and-enforcement"></a><span data-ttu-id="f95b6-147">HTTPS 的重新導向和強制執行</span><span class="sxs-lookup"><span data-stu-id="f95b6-147">HTTPS redirection and enforcement</span></span>

<span data-ttu-id="f95b6-148">Web 應用程式通常需要同時接聽 HTTP 和 HTTPS，然後將所有 HTTP 流量重新導向至 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="f95b6-148">Web apps typically need to listen on both HTTP and HTTPS, but then redirect all HTTP traffic to HTTPS.</span></span> <span data-ttu-id="f95b6-149">在 2.1 中，特殊化的 HTTPS 重新導向中介軟體，會根據有無組態或是否引入繫結的伺服器連接埠，以聰明的方式重新導向。</span><span class="sxs-lookup"><span data-stu-id="f95b6-149">In 2.1, specialized HTTPS redirection middleware that intelligently redirects based on the presence of configuration or bound server ports has been introduced.</span></span>

<span data-ttu-id="f95b6-150">使用 [HTTP 嚴格傳輸安全性通訊協定 (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) 會進一步強制使用 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="f95b6-150">Use of HTTPS can be further enforced using [HTTP Strict Transport Security Protocol (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts).</span></span> <span data-ttu-id="f95b6-151">HSTS 指示瀏覽器一律透過 HTTPS 存取站台。</span><span class="sxs-lookup"><span data-stu-id="f95b6-151">HSTS instructs browsers to always access the site via HTTPS.</span></span> <span data-ttu-id="f95b6-152">ASP.NET Core 2.1 會新增支援最大存留期、子網域和 HSTS 預先載入清單選項的 HSTS 中介軟體。</span><span class="sxs-lookup"><span data-stu-id="f95b6-152">ASP.NET Core 2.1 adds HSTS middleware that supports options for max age, subdomains, and the HSTS preload list.</span></span>

### <a name="configuration-for-production"></a><span data-ttu-id="f95b6-153">生產環境的組態</span><span class="sxs-lookup"><span data-stu-id="f95b6-153">Configuration for production</span></span>

<span data-ttu-id="f95b6-154">在生產環境中，必須明確設定 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="f95b6-154">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="f95b6-155">在 2.1 中，已新增為 Kestrel 設定 HTTPS 的預設組態結構描述。</span><span class="sxs-lookup"><span data-stu-id="f95b6-155">In 2.1, default configuration schema for configuring HTTPS for Kestrel has been added.</span></span> <span data-ttu-id="f95b6-156">應用程式可以設定為使用下列：</span><span class="sxs-lookup"><span data-stu-id="f95b6-156">Apps can be configured to use:</span></span>

* <span data-ttu-id="f95b6-157">包括 URL 在內的多個端點。</span><span class="sxs-lookup"><span data-stu-id="f95b6-157">Multiple endpoints including the URLs.</span></span> <span data-ttu-id="f95b6-158">如需詳細資訊，請參閱[實作 Kestrel 網頁伺服器：端點組態](xref:fundamentals/servers/kestrel#endpoint-configuration)。</span><span class="sxs-lookup"><span data-stu-id="f95b6-158">For more information, see [Kestrel web server implementation: Endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>
* <span data-ttu-id="f95b6-159">用於 HTTPS 的憑證來自磁碟檔案或憑證存放區。</span><span class="sxs-lookup"><span data-stu-id="f95b6-159">The certificate to use for HTTPS either from a file on disk or from a certificate store.</span></span>

## <a name="gdpr"></a><span data-ttu-id="f95b6-160">GDPR</span><span class="sxs-lookup"><span data-stu-id="f95b6-160">GDPR</span></span>

<span data-ttu-id="f95b6-161">ASP.NET Core 提供 API 和範本以利符合某些 [EU General Data Protection Regulation (GDPR)](https://www.eugdpr.org/) (EU 一般資料保護規定 (GDPR)) 需求。</span><span class="sxs-lookup"><span data-stu-id="f95b6-161">ASP.NET Core provides APIs and templates to help meet some of the [EU General Data Protection Regulation (GDPR)](https://www.eugdpr.org/) requirements.</span></span> <span data-ttu-id="f95b6-162">如需詳細資訊，請參閱 [ASP.NET Core 的 GDPR 支援](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="f95b6-162">For more information, see [GDPR support in ASP.NET Core](xref:security/gdpr).</span></span> <span data-ttu-id="f95b6-163">[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample)示範如何使用及讓您測試新增至 ASP.NET Core 2.1 範本的大部分 GDPR 擴充點和 API。</span><span class="sxs-lookup"><span data-stu-id="f95b6-163">A [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) shows how to use and lets you test most of the GDPR extension points and APIs added to the ASP.NET Core 2.1 templates.</span></span>

## <a name="integration-tests"></a><span data-ttu-id="f95b6-164">整合測試</span><span class="sxs-lookup"><span data-stu-id="f95b6-164">Integration tests</span></span>

<span data-ttu-id="f95b6-165">引進新套件簡化測試的建立和執行。</span><span class="sxs-lookup"><span data-stu-id="f95b6-165">A new package is introduced that streamlines test creation and execution.</span></span> <span data-ttu-id="f95b6-166">[Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/) 套件處理下列工作：</span><span class="sxs-lookup"><span data-stu-id="f95b6-166">The [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/) package handles the following tasks:</span></span>

* <span data-ttu-id="f95b6-167">將依賴項檔*\*(.deps*) 從測試的應用程式複製到測試專案的*bin*資料夾中。</span><span class="sxs-lookup"><span data-stu-id="f95b6-167">Copies the dependency file (*\*.deps*) from the tested app into the test project's *bin* folder.</span></span>
* <span data-ttu-id="f95b6-168">將內容的根目錄設定為經過測試之應用程式的專案根目錄，以便在執行測試時找到靜態檔案和頁面/檢視。</span><span class="sxs-lookup"><span data-stu-id="f95b6-168">Sets the content root to the tested app's project root so that static files and pages/views are found when the tests are executed.</span></span>
* <span data-ttu-id="f95b6-169">提供 [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) 類別來簡化以 [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) 啟動載入經過測試的應用程式。</span><span class="sxs-lookup"><span data-stu-id="f95b6-169">Provides the [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) class to streamline bootstrapping the tested app with [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver).</span></span>

<span data-ttu-id="f95b6-170">下列測試使用 [xUnit](https://xunit.github.io/) 檢查具有成功狀態碼與正確 Content-type 標頭的 [索引] 頁面載入：</span><span class="sxs-lookup"><span data-stu-id="f95b6-170">The following test uses [xUnit](https://xunit.github.io/) to check that the Index page loads with a success status code and with the correct Content-Type header:</span></span>

```csharp
public class BasicTests
    : IClassFixture<WebApplicationFactory<RazorPagesProject.Startup>>
{
    private readonly HttpClient _client;

    public BasicTests(WebApplicationFactory<RazorPagesProject.Startup> factory)
    {
        _client = factory.CreateClient();
    }

    [Fact]
    public async Task GetHomePage()
    {
        // Act
        var response = await _client.GetAsync("/");

        // Assert
        response.EnsureSuccessStatusCode(); // Status Code 200-299
        Assert.Equal("text/html; charset=utf-8",
            response.Content.Headers.ContentType.ToString());
    }
}
```

<span data-ttu-id="f95b6-171">如需詳細資訊，請參閱[整合測試](xref:test/integration-tests)主題。</span><span class="sxs-lookup"><span data-stu-id="f95b6-171">For more information, see the [Integration tests](xref:test/integration-tests) topic.</span></span>

## <a name="apicontroller-actionresultt"></a><span data-ttu-id="f95b6-172">[ApiController], ActionResult\<T></span><span class="sxs-lookup"><span data-stu-id="f95b6-172">[ApiController], ActionResult\<T></span></span>

<span data-ttu-id="f95b6-173">ASP.NET Core 2.1 新增新的程式設計慣例，讓您輕鬆建置全新和描述性的 Web API。</span><span class="sxs-lookup"><span data-stu-id="f95b6-173">ASP.NET Core 2.1 adds new programming conventions that make it easier to build clean and descriptive web APIs.</span></span> <span data-ttu-id="f95b6-174">`ActionResult<T>` 是新增的新類型，允許應用程式傳回回應類型或任何其他動作的結果 (類似於 IActionResult)，同時仍然表示回應類型。</span><span class="sxs-lookup"><span data-stu-id="f95b6-174">`ActionResult<T>` is a new type added to allow an app to return either a response type or any other action result (similar to IActionResult), while still indicating the response type.</span></span> <span data-ttu-id="f95b6-175">`[ApiController]` 屬性也以選擇加入的方式新增至 Web API 特定慣例和行為中。</span><span class="sxs-lookup"><span data-stu-id="f95b6-175">The `[ApiController]` attribute has also been added as the way to opt in to Web API-specific conventions and behaviors.</span></span>

<span data-ttu-id="f95b6-176">如需詳細資訊，請參閱[建置 Web API 與 ASP.NET Core](xref:web-api/index)。</span><span class="sxs-lookup"><span data-stu-id="f95b6-176">For more information, see [Build Web APIs with ASP.NET Core](xref:web-api/index).</span></span>

## <a name="ihttpclientfactory"></a><span data-ttu-id="f95b6-177">IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="f95b6-177">IHttpClientFactory</span></span>

<span data-ttu-id="f95b6-178">ASP.NET Core 2.1 包含新的 `IHttpClientFactory` 服務，讓您在應用程式中輕鬆設定和使用 `HttpClient` 的執行個體。</span><span class="sxs-lookup"><span data-stu-id="f95b6-178">ASP.NET Core 2.1 includes a new `IHttpClientFactory` service that makes it easier to configure and consume instances of `HttpClient` in apps.</span></span> <span data-ttu-id="f95b6-179">`HttpClient` 已經有委派可針對傳出 HTTP 要求連結在一起之處理常式的概念。</span><span class="sxs-lookup"><span data-stu-id="f95b6-179">`HttpClient` already has the concept of delegating handlers that could be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="f95b6-180">Factory：</span><span class="sxs-lookup"><span data-stu-id="f95b6-180">The factory:</span></span>

* <span data-ttu-id="f95b6-181">讓每個具名用戶端以更直覺的方式註冊 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="f95b6-181">Makes registering of instances of `HttpClient` per named client more intuitive.</span></span>
* <span data-ttu-id="f95b6-182">實作允許 Polly 原則用於重試、CircuitBreakers 等等的 Polly 處理常式。</span><span class="sxs-lookup"><span data-stu-id="f95b6-182">Implements a Polly handler that allows Polly policies to be used for Retry, CircuitBreakers, etc.</span></span>

<span data-ttu-id="f95b6-183">如需詳細資訊，請參閱[初始化 HTTP 要求](xref:fundamentals/http-requests)。</span><span class="sxs-lookup"><span data-stu-id="f95b6-183">For more information, see [Initiate HTTP Requests](xref:fundamentals/http-requests).</span></span>

## <a name="kestrel-transport-configuration"></a><span data-ttu-id="f95b6-184">Kestrel 傳輸組態</span><span class="sxs-lookup"><span data-stu-id="f95b6-184">Kestrel transport configuration</span></span>

<span data-ttu-id="f95b6-185">隨著 ASP.NET Core 2.1 的發行，Kestrel 的預設傳輸不再根據 Libuv，而是改為根據受控通訊端。</span><span class="sxs-lookup"><span data-stu-id="f95b6-185">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="f95b6-186">如需詳細資訊，請參閱[實作 Kestrel 網頁伺服器：傳輸組態](xref:fundamentals/servers/kestrel#transport-configuration)。</span><span class="sxs-lookup"><span data-stu-id="f95b6-186">For more information, see [Kestrel web server implementation: Transport configuration](xref:fundamentals/servers/kestrel#transport-configuration).</span></span>

## <a name="generic-host-builder"></a><span data-ttu-id="f95b6-187">泛型主機建立器</span><span class="sxs-lookup"><span data-stu-id="f95b6-187">Generic host builder</span></span>

<span data-ttu-id="f95b6-188">已引入泛型主機建立器 (`HostBuilder`)。</span><span class="sxs-lookup"><span data-stu-id="f95b6-188">The Generic Host Builder (`HostBuilder`) has been introduced.</span></span> <span data-ttu-id="f95b6-189">此建立器可用於不處理 HTTP 要求的應用程式 (傳訊、背景工作等等)。</span><span class="sxs-lookup"><span data-stu-id="f95b6-189">This builder can be used for apps that don't process HTTP requests (Messaging, background tasks, etc.).</span></span>

<span data-ttu-id="f95b6-190">如需詳細資訊，請參閱 [.NET 泛型主機](xref:fundamentals/host/generic-host)。</span><span class="sxs-lookup"><span data-stu-id="f95b6-190">For more information, see [.NET Generic Host](xref:fundamentals/host/generic-host).</span></span>

## <a name="updated-spa-templates"></a><span data-ttu-id="f95b6-191">已更新 SPA 範本</span><span class="sxs-lookup"><span data-stu-id="f95b6-191">Updated SPA templates</span></span>

<span data-ttu-id="f95b6-192">已更新適用於 Angular、React 和 React with Redux 的單頁應用程式範本，以使用標準的專案結構，並為每個架構建置系統。</span><span class="sxs-lookup"><span data-stu-id="f95b6-192">The Single Page Application templates for Angular, React, and React with Redux are updated to use the standard project structures and build systems for each framework.</span></span>

<span data-ttu-id="f95b6-193">Angular 範本以 Angular CLI 為基礎，React 範本以 create-react-app 為基礎。</span><span class="sxs-lookup"><span data-stu-id="f95b6-193">The Angular template is based on the Angular CLI, and the React templates are based on create-react-app.</span></span>

<span data-ttu-id="f95b6-194">如需詳細資訊，請參閱</span><span class="sxs-lookup"><span data-stu-id="f95b6-194">For more information, see:</span></span>

* <xref:spa/angular>
* <xref:spa/react>
* <xref:spa/react-with-redux>

## <a name="razor-pages-search-for-razor-assets"></a><span data-ttu-id="f95b6-195">Razor Pages 搜尋 Razor 資產</span><span class="sxs-lookup"><span data-stu-id="f95b6-195">Razor Pages search for Razor assets</span></span>

<span data-ttu-id="f95b6-196">在 2.1 中，Razor Pages 會依所列順序在下列目錄中搜尋 Razor 資產 (如版面配置和部分)：</span><span class="sxs-lookup"><span data-stu-id="f95b6-196">In 2.1, Razor Pages search for Razor assets (such as layouts and partials) in the following directories in the listed order:</span></span>

1. <span data-ttu-id="f95b6-197">目前的 Pages 資料夾。</span><span class="sxs-lookup"><span data-stu-id="f95b6-197">Current Pages folder.</span></span>
1. <span data-ttu-id="f95b6-198">*/Pages/Shared/*</span><span class="sxs-lookup"><span data-stu-id="f95b6-198">*/Pages/Shared/*</span></span>
1. <span data-ttu-id="f95b6-199">*/Views/Shared/*</span><span class="sxs-lookup"><span data-stu-id="f95b6-199">*/Views/Shared/*</span></span>

## <a name="razor-pages-in-an-area"></a><span data-ttu-id="f95b6-200">區域中的 Razor Pages</span><span class="sxs-lookup"><span data-stu-id="f95b6-200">Razor Pages in an area</span></span>

<span data-ttu-id="f95b6-201">Razor Pages 現可支援[區域](xref:mvc/controllers/areas)。</span><span class="sxs-lookup"><span data-stu-id="f95b6-201">Razor Pages now support [areas](xref:mvc/controllers/areas).</span></span> <span data-ttu-id="f95b6-202">若要查看區域範例，請使用個別的使用者帳戶建立新的 Razor Pages Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="f95b6-202">To see an example of areas, create a new Razor Pages web app with individual user accounts.</span></span> <span data-ttu-id="f95b6-203">具有個別使用者帳戶的 Razor Pages Web 應用程式包含 */Areas/Identity/Pages*。</span><span class="sxs-lookup"><span data-stu-id="f95b6-203">A Razor Pages web app with individual user accounts includes */Areas/Identity/Pages*.</span></span>

## <a name="mvc-compatibility-version"></a><span data-ttu-id="f95b6-204">MVC 相容性版本</span><span class="sxs-lookup"><span data-stu-id="f95b6-204">MVC compatibility version</span></span>

<span data-ttu-id="f95b6-205"><xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> 方法可讓應用程式加入或退出 ASP.NET Core MVC 2.1 或更新版本所引入的可能重大行為變更。</span><span class="sxs-lookup"><span data-stu-id="f95b6-205">The <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> method allows an app to opt-in or opt-out of potentially breaking behavior changes introduced in ASP.NET Core MVC 2.1 or later.</span></span>

<span data-ttu-id="f95b6-206">如需詳細資訊，請參閱 <xref:mvc/compatibility-version>。</span><span class="sxs-lookup"><span data-stu-id="f95b6-206">For more information, see <xref:mvc/compatibility-version>.</span></span>

## <a name="migrate-from-20-to-21"></a><span data-ttu-id="f95b6-207">從 2.0 遷移至 2.1</span><span class="sxs-lookup"><span data-stu-id="f95b6-207">Migrate from 2.0 to 2.1</span></span>

<span data-ttu-id="f95b6-208">請參閱[從 ASP.NET Core 2.0 遷移到 2.1](xref:migration/20_21)。</span><span class="sxs-lookup"><span data-stu-id="f95b6-208">See [Migrate from ASP.NET Core 2.0 to 2.1](xref:migration/20_21).</span></span>

## <a name="additional-information"></a><span data-ttu-id="f95b6-209">其他資訊</span><span class="sxs-lookup"><span data-stu-id="f95b6-209">Additional information</span></span>

<span data-ttu-id="f95b6-210">如需完整的變更清單，請參閱 [ASP.NET Core 2.1 版本資訊](https://github.com/dotnet/aspnetcore/releases/tag/2.1.0)。</span><span class="sxs-lookup"><span data-stu-id="f95b6-210">For the complete list of changes, see the [ASP.NET Core 2.1 Release Notes](https://github.com/dotnet/aspnetcore/releases/tag/2.1.0).</span></span>
