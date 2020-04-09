---
title: ASP.NET核心項目進行故障排除和除錯
author: Rick-Anderson
description: 了解 ASP.NET Core 專案的相關警告和錯誤，並為其進行疑難排解。
ms.author: riande
ms.custom: mvc
ms.date: 07/10/2019
uid: test/troubleshoot
ms.openlocfilehash: 345967f08cf99ef5f18d0c9bcd59ab29c74454f1
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "79511505"
---
# <a name="troubleshoot-and-debug-aspnet-core-projects"></a><span data-ttu-id="d0c6f-103">ASP.NET核心項目進行故障排除和除錯</span><span class="sxs-lookup"><span data-stu-id="d0c6f-103">Troubleshoot and debug ASP.NET Core projects</span></span>

<span data-ttu-id="d0c6f-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d0c6f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d0c6f-105">以下連結提供故障排除指南:</span><span class="sxs-lookup"><span data-stu-id="d0c6f-105">The following links provide troubleshooting guidance:</span></span>

* <xref:test/troubleshoot-azure-iis>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [<span data-ttu-id="d0c6f-106">NDC 會議(2018 年 倫敦):診斷ASP.NET核心應用中的問題</span><span class="sxs-lookup"><span data-stu-id="d0c6f-106">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="d0c6f-107">ASP.NET博客:解決核心性能問題的ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d0c6f-107">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="d0c6f-108">.NET 核心 SDK 警告</span><span class="sxs-lookup"><span data-stu-id="d0c6f-108">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="d0c6f-109">安裝了 .NET Core SDK 的 32 位元和 64 位元版本</span><span class="sxs-lookup"><span data-stu-id="d0c6f-109">Both the 32-bit and 64-bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="d0c6f-110">在「ASP.NET核心 **」的「新項目**」對話框中,您可能會看到以下警告:</span><span class="sxs-lookup"><span data-stu-id="d0c6f-110">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="d0c6f-111">安裝了 .NET Core SDK 的 32 位元和 64 位元版本。</span><span class="sxs-lookup"><span data-stu-id="d0c6f-111">Both 32-bit and 64-bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="d0c6f-112">僅\\顯示安裝在「C:程式\\檔\\點\\網 」 上的 64 位元版本的範本。</span><span class="sxs-lookup"><span data-stu-id="d0c6f-112">Only templates from the 64-bit versions installed at 'C:\\Program Files\\dotnet\\sdk\\' are displayed.</span></span>

<span data-ttu-id="d0c6f-113">安裝[.NET Core SDK](https://dotnet.microsoft.com/download/dotnet-core)的 32 位元 (x86) 和 64 位元 (x64) 版本時,將顯示此警告。</span><span class="sxs-lookup"><span data-stu-id="d0c6f-113">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://dotnet.microsoft.com/download/dotnet-core) are installed.</span></span> <span data-ttu-id="d0c6f-114">可能安裝兩個版本的常見原因包括:</span><span class="sxs-lookup"><span data-stu-id="d0c6f-114">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="d0c6f-115">您最初使用 32 位元電腦下載了 .NET Core SDK 安裝程式,但隨後將其複製到整個電腦上,並將其安裝在 64 位元電腦上。</span><span class="sxs-lookup"><span data-stu-id="d0c6f-115">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="d0c6f-116">32 位元 .NET 核心 SDK 由另一個應用程式安裝。</span><span class="sxs-lookup"><span data-stu-id="d0c6f-116">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="d0c6f-117">下載並安裝了錯誤的版本。</span><span class="sxs-lookup"><span data-stu-id="d0c6f-117">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="d0c6f-118">卸載 32 位元 .NET 核心 SDK 以防止此警告。</span><span class="sxs-lookup"><span data-stu-id="d0c6f-118">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="d0c6f-119">從**控制面板** > **程式與功能** > 卸載**或變更程式**。</span><span class="sxs-lookup"><span data-stu-id="d0c6f-119">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="d0c6f-120">如果您瞭解警告發生的原因及其影響,則可以忽略該警告。</span><span class="sxs-lookup"><span data-stu-id="d0c6f-120">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="d0c6f-121">.NET 核心 SDK 安裝在多個位置</span><span class="sxs-lookup"><span data-stu-id="d0c6f-121">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="d0c6f-122">在「ASP.NET核心 **」的「新項目**」對話框中,您可能會看到以下警告:</span><span class="sxs-lookup"><span data-stu-id="d0c6f-122">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="d0c6f-123">.NET 核心 SDK 安裝在多個位置。</span><span class="sxs-lookup"><span data-stu-id="d0c6f-123">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="d0c6f-124">僅\\顯示安裝在「C:程式檔\\點\\網\\」 上的 SDK 的樣本。</span><span class="sxs-lookup"><span data-stu-id="d0c6f-124">Only templates from the SDKs installed at 'C:\\Program Files\\dotnet\\sdk\\' are displayed.</span></span>

<span data-ttu-id="d0c6f-125">當您在*\\C:\\程式檔案\\點網\\sdk*之外的目錄中至少安裝一個 .NET Core SDK 時,您將看到此消息。</span><span class="sxs-lookup"><span data-stu-id="d0c6f-125">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="d0c6f-126">通常,當 .NET Core SDK 已使用複製/粘貼而不是 MSI 安裝程式部署在電腦上時,就會發生這種情況。</span><span class="sxs-lookup"><span data-stu-id="d0c6f-126">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="d0c6f-127">卸載所有 32 位元 .NET 核心 SDK 和運行時,以防止出現此警告。</span><span class="sxs-lookup"><span data-stu-id="d0c6f-127">Uninstall all 32-bit .NET Core SDKs and runtimes to prevent this warning.</span></span> <span data-ttu-id="d0c6f-128">從**控制面板** > **程式與功能** > 卸載**或變更程式**。</span><span class="sxs-lookup"><span data-stu-id="d0c6f-128">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="d0c6f-129">如果您瞭解警告發生的原因及其影響,則可以忽略該警告。</span><span class="sxs-lookup"><span data-stu-id="d0c6f-129">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="d0c6f-130">未偵測到 .NET 核心 SDK</span><span class="sxs-lookup"><span data-stu-id="d0c6f-130">No .NET Core SDKs were detected</span></span>

* <span data-ttu-id="d0c6f-131">在「ASP.NET核心的視覺化工作室**新項目**」對話框中,您可能會看到以下警告:</span><span class="sxs-lookup"><span data-stu-id="d0c6f-131">In the Visual Studio **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

  > <span data-ttu-id="d0c6f-132">未檢測到 .NET 核心 SDK,確保它們包含在`PATH`環境變數 中。</span><span class="sxs-lookup"><span data-stu-id="d0c6f-132">No .NET Core SDKs were detected, ensure they are included in the environment variable `PATH`.</span></span>

* <span data-ttu-id="d0c6f-133">執行命令`dotnet`時,警告顯示為:</span><span class="sxs-lookup"><span data-stu-id="d0c6f-133">When executing a `dotnet` command, the warning appears as:</span></span>

  > <span data-ttu-id="d0c6f-134">無法找到任何已安裝的點網 SDK。</span><span class="sxs-lookup"><span data-stu-id="d0c6f-134">It was not possible to find any installed dotnet SDKs.</span></span>

<span data-ttu-id="d0c6f-135">當環境變數`PATH`不指向電腦上的任何 .NET 核心 SDK 時,將顯示這些警告。</span><span class="sxs-lookup"><span data-stu-id="d0c6f-135">These warnings appear when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="d0c6f-136">若要解決此問題：</span><span class="sxs-lookup"><span data-stu-id="d0c6f-136">To resolve this problem:</span></span>

* <span data-ttu-id="d0c6f-137">安裝 .NET Core SDK。</span><span class="sxs-lookup"><span data-stu-id="d0c6f-137">Install the .NET Core SDK.</span></span> <span data-ttu-id="d0c6f-138">從[.NET 下載](https://dotnet.microsoft.com/download)獲取最新的安裝程式。</span><span class="sxs-lookup"><span data-stu-id="d0c6f-138">Obtain the latest installer from [.NET Downloads](https://dotnet.microsoft.com/download).</span></span>
* <span data-ttu-id="d0c6f-139">驗證`PATH`環境變數是否指向安裝 SDK`C:\Program Files\dotnet\`的位置( 對於 64 位`C:\Program Files (x86)\dotnet\`元/x64 或 32 位元/x86)。</span><span class="sxs-lookup"><span data-stu-id="d0c6f-139">Verify that the `PATH` environment variable points to the location where the SDK is installed (`C:\Program Files\dotnet\` for 64-bit/x64 or `C:\Program Files (x86)\dotnet\` for 32-bit/x86).</span></span> <span data-ttu-id="d0c6f-140">SDK 安裝程式通常`PATH`設定 。</span><span class="sxs-lookup"><span data-stu-id="d0c6f-140">The SDK installer normally sets the `PATH`.</span></span> <span data-ttu-id="d0c6f-141">始終在同一台電腦上安裝相同的位點 SDK 和運行時。</span><span class="sxs-lookup"><span data-stu-id="d0c6f-141">Always install the same bitness SDKs and runtimes on the same machine.</span></span>

### <a name="missing-sdk-after-installing-the-net-core-hosting-bundle"></a><span data-ttu-id="d0c6f-142">安裝 .NET 核心託管捆綁包後缺少 SDK</span><span class="sxs-lookup"><span data-stu-id="d0c6f-142">Missing SDK after installing the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="d0c6f-143">安裝[.NET 核心託管捆綁套件](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)將`PATH`修改安裝 .NET Core 執行時以指向 .NET Core () 的`C:\Program Files (x86)\dotnet\`32 位元 (x86) 版本 ( )</span><span class="sxs-lookup"><span data-stu-id="d0c6f-143">Installing the [.NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) modifies the `PATH` when it installs the .NET Core runtime to point to the 32-bit (x86) version of .NET Core (`C:\Program Files (x86)\dotnet\`).</span></span> <span data-ttu-id="d0c6f-144">當使用 32 位元 (x86)`dotnet`.NET Core 命令(檢測到 No[.NET 核心 SDK)](#no-net-core-sdks-were-detected)時,這可能會導致缺少 SDK。</span><span class="sxs-lookup"><span data-stu-id="d0c6f-144">This can result in missing SDKs when the 32-bit (x86) .NET Core `dotnet` command is used ([No .NET Core SDKs were detected](#no-net-core-sdks-were-detected)).</span></span> <span data-ttu-id="d0c6f-145">要解決此問題,轉到`C:\Program Files\dotnet\``C:\Program Files (x86)\dotnet\`之前`PATH`在上的位置。</span><span class="sxs-lookup"><span data-stu-id="d0c6f-145">To resolve this problem, move `C:\Program Files\dotnet\` to a position before `C:\Program Files (x86)\dotnet\` on the `PATH`.</span></span>

## <a name="obtain-data-from-an-app"></a><span data-ttu-id="d0c6f-146">從應用程式取得資料</span><span class="sxs-lookup"><span data-stu-id="d0c6f-146">Obtain data from an app</span></span>

<span data-ttu-id="d0c6f-147">如果應用能夠回應請求,則可以使用中間件從應用獲取以下資料:</span><span class="sxs-lookup"><span data-stu-id="d0c6f-147">If an app is capable of responding to requests, you can obtain the following data from the app using middleware:</span></span>

* <span data-ttu-id="d0c6f-148">要求&ndash;方法、方案、主機、路徑庫、路徑、查詢字串、標頭</span><span class="sxs-lookup"><span data-stu-id="d0c6f-148">Request &ndash; Method, scheme, host, pathbase, path, query string, headers</span></span>
* <span data-ttu-id="d0c6f-149">連線&ndash;遠端 IP 位址、遠端連接埠、本地 IP 位址、本地端埠、用戶端憑證</span><span class="sxs-lookup"><span data-stu-id="d0c6f-149">Connection &ndash; Remote IP address, remote port, local IP address, local port, client certificate</span></span>
* <span data-ttu-id="d0c6f-150">識別&ndash;名稱,顯示名稱</span><span class="sxs-lookup"><span data-stu-id="d0c6f-150">Identity &ndash; Name, display name</span></span>
* <span data-ttu-id="d0c6f-151">組態設定</span><span class="sxs-lookup"><span data-stu-id="d0c6f-151">Configuration settings</span></span>
* <span data-ttu-id="d0c6f-152">環境變數</span><span class="sxs-lookup"><span data-stu-id="d0c6f-152">Environment variables</span></span>

<span data-ttu-id="d0c6f-153">將以下[中間件](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)代碼`Startup.Configure`放在方法的請求處理管道的開頭。</span><span class="sxs-lookup"><span data-stu-id="d0c6f-153">Place the following [middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) code at the beginning of the `Startup.Configure` method's request processing pipeline.</span></span> <span data-ttu-id="d0c6f-154">在運行中間件之前檢查環境,以確保代碼僅在開發環境中執行。</span><span class="sxs-lookup"><span data-stu-id="d0c6f-154">The environment is checked before the middleware is run to ensure that the code is only executed in the Development environment.</span></span>

<span data-ttu-id="d0c6f-155">要取得環境,請使用以下任一方法:</span><span class="sxs-lookup"><span data-stu-id="d0c6f-155">To obtain the environment, use either of the following approaches:</span></span>

* <span data-ttu-id="d0c6f-156">將`IHostingEnvironment`注入`Startup.Configure`方法,並與局部變數檢查環境。</span><span class="sxs-lookup"><span data-stu-id="d0c6f-156">Inject the `IHostingEnvironment` into the `Startup.Configure` method and check the environment with the local variable.</span></span> <span data-ttu-id="d0c6f-157">以下示例代碼演示了此方法。</span><span class="sxs-lookup"><span data-stu-id="d0c6f-157">The following sample code demonstrates this approach.</span></span>

* <span data-ttu-id="d0c6f-158">將環境分配給類中的`Startup`屬性。</span><span class="sxs-lookup"><span data-stu-id="d0c6f-158">Assign the environment to a property in the `Startup` class.</span></span> <span data-ttu-id="d0c6f-159">使用 屬性檢查環境(例如`if (Environment.IsDevelopment())`,</span><span class="sxs-lookup"><span data-stu-id="d0c6f-159">Check the environment using the property (for example, `if (Environment.IsDevelopment())`).</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, 
    IConfiguration config)
{
    if (env.IsDevelopment())
    {
        app.Run(async (context) =>
        {
            var sb = new StringBuilder();
            var nl = System.Environment.NewLine;
            var rule = string.Concat(nl, new string('-', 40), nl);
            var authSchemeProvider = app.ApplicationServices
                .GetRequiredService<IAuthenticationSchemeProvider>();

            sb.Append($"Request{rule}");
            sb.Append($"{DateTimeOffset.Now}{nl}");
            sb.Append($"{context.Request.Method} {context.Request.Path}{nl}");
            sb.Append($"Scheme: {context.Request.Scheme}{nl}");
            sb.Append($"Host: {context.Request.Headers["Host"]}{nl}");
            sb.Append($"PathBase: {context.Request.PathBase.Value}{nl}");
            sb.Append($"Path: {context.Request.Path.Value}{nl}");
            sb.Append($"Query: {context.Request.QueryString.Value}{nl}{nl}");

            sb.Append($"Connection{rule}");
            sb.Append($"RemoteIp: {context.Connection.RemoteIpAddress}{nl}");
            sb.Append($"RemotePort: {context.Connection.RemotePort}{nl}");
            sb.Append($"LocalIp: {context.Connection.LocalIpAddress}{nl}");
            sb.Append($"LocalPort: {context.Connection.LocalPort}{nl}");
            sb.Append($"ClientCert: {context.Connection.ClientCertificate}{nl}{nl}");

            sb.Append($"Identity{rule}");
            sb.Append($"User: {context.User.Identity.Name}{nl}");
            var scheme = await authSchemeProvider
                .GetSchemeAsync(IISDefaults.AuthenticationScheme);
            sb.Append($"DisplayName: {scheme?.DisplayName}{nl}{nl}");

            sb.Append($"Headers{rule}");
            foreach (var header in context.Request.Headers)
            {
                sb.Append($"{header.Key}: {header.Value}{nl}");
            }
            sb.Append(nl);

            sb.Append($"Websockets{rule}");
            if (context.Features.Get<IHttpUpgradeFeature>() != null)
            {
                sb.Append($"Status: Enabled{nl}{nl}");
            }
            else
            {
                sb.Append($"Status: Disabled{nl}{nl}");
            }

            sb.Append($"Configuration{rule}");
            foreach (var pair in config.AsEnumerable())
            {
                sb.Append($"{pair.Path}: {pair.Value}{nl}");
            }
            sb.Append(nl);

            sb.Append($"Environment Variables{rule}");
            var vars = System.Environment.GetEnvironmentVariables();
            foreach (var key in vars.Keys.Cast<string>().OrderBy(key => key, 
                StringComparer.OrdinalIgnoreCase))
            {
                var value = vars[key];
                sb.Append($"{key}: {value}{nl}");
            }

            context.Response.ContentType = "text/plain";
            await context.Response.WriteAsync(sb.ToString());
        });
    }
}
```

## <a name="debug-aspnet-core-apps"></a><span data-ttu-id="d0c6f-160">除錯ASP.NET核心應用</span><span class="sxs-lookup"><span data-stu-id="d0c6f-160">Debug ASP.NET Core apps</span></span>

<span data-ttu-id="d0c6f-161">以下連結提供有關調試 ASP.NET核心應用的資訊。</span><span class="sxs-lookup"><span data-stu-id="d0c6f-161">The following links provide information on debugging ASP.NET Core apps.</span></span>

* [<span data-ttu-id="d0c6f-162">Linux 上呼試 ASP 核心</span><span class="sxs-lookup"><span data-stu-id="d0c6f-162">Debugging ASP Core on Linux</span></span>](https://devblogs.microsoft.com/premier-developer/debugging-asp-core-on-linux-with-visual-studio-2017/)
* [<span data-ttu-id="d0c6f-163">在 SSH 上 Unix 上調試 .NET 內核</span><span class="sxs-lookup"><span data-stu-id="d0c6f-163">Debugging .NET Core on Unix over SSH</span></span>](https://devblogs.microsoft.com/devops/debugging-net-core-on-unix-over-ssh/)
* [<span data-ttu-id="d0c6f-164">快速入門：使用 Visual Studio 偵錯工具來偵錯 ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d0c6f-164">Quickstart: Debug ASP.NET with the Visual Studio debugger</span></span>](/visualstudio/debugger/quickstart-debug-aspnet)
* <span data-ttu-id="d0c6f-165">有關詳細資訊,請參閱[此 GitHub 問題](https://github.com/dotnet/AspNetCore.Docs/issues/2960)。</span><span class="sxs-lookup"><span data-stu-id="d0c6f-165">See [this GitHub issue](https://github.com/dotnet/AspNetCore.Docs/issues/2960) for more debugging information.</span></span>
