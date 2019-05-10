---
title: 疑難排解 ASP.NET Core 專案
author: Rick-Anderson
description: 了解 ASP.NET Core 專案的相關警告和錯誤，並為其進行疑難排解。
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2019
uid: test/troubleshoot
ms.openlocfilehash: 3d755b2f0c509d65dea86bbe719e42935d87d546
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64895325"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="de084-103">疑難排解 ASP.NET Core 專案</span><span class="sxs-lookup"><span data-stu-id="de084-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="de084-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="de084-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="de084-105">下列連結提供疑難排解指導方針：</span><span class="sxs-lookup"><span data-stu-id="de084-105">The following links provide troubleshooting guidance:</span></span>

* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [<span data-ttu-id="de084-106">NDC Conference （倫敦，2018年）：ASP.NET Core 應用程式中的診斷問題</span><span class="sxs-lookup"><span data-stu-id="de084-106">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="de084-107">ASP.NET 部落格：ASP.NET Core 的效能問題的疑難排解</span><span class="sxs-lookup"><span data-stu-id="de084-107">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="de084-108">.NET core SDK 警告</span><span class="sxs-lookup"><span data-stu-id="de084-108">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="de084-109">會安裝 32 位元和 64 位元版本的.NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="de084-109">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="de084-110">在**新專案**對話方塊適用於 ASP.NET Core，您可能會看到下列警告：</span><span class="sxs-lookup"><span data-stu-id="de084-110">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="de084-111">會安裝 32 和 64 位元版本的.NET Core sdk。</span><span class="sxs-lookup"><span data-stu-id="de084-111">Both 32 and 64 bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="de084-112">只有從安裝在 64 位元版本的範本 'c:\\Program Files\\dotnet\\sdk\\' 將會顯示。</span><span class="sxs-lookup"><span data-stu-id="de084-112">Only templates from the 64 bit version(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

<span data-ttu-id="de084-113">時，會出現這個警告 (x86) 32 位元和 64 位元 (x64) 版本的[.NET Core SDK](https://www.microsoft.com/net/download/all)安裝。</span><span class="sxs-lookup"><span data-stu-id="de084-113">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="de084-114">這兩個版本可能會安裝的常見原因包括：</span><span class="sxs-lookup"><span data-stu-id="de084-114">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="de084-115">您原本下載.NET Core SDK 安裝程式使用 32 位元電腦，但然後複製其整個並安裝在 64 位元電腦上。</span><span class="sxs-lookup"><span data-stu-id="de084-115">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="de084-116">32 位元.NET Core SDK 已安裝另一個應用程式。</span><span class="sxs-lookup"><span data-stu-id="de084-116">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="de084-117">版本錯誤下載並安裝。</span><span class="sxs-lookup"><span data-stu-id="de084-117">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="de084-118">解除安裝 32 位元.NET Core SDK，以避免這個警告。</span><span class="sxs-lookup"><span data-stu-id="de084-118">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="de084-119">從解除安裝**控制台中** > **程式和功能** > **解除安裝或變更程式**。</span><span class="sxs-lookup"><span data-stu-id="de084-119">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="de084-120">如果您了解警告的發生原因和其隱含意義，您可以忽略此警告。</span><span class="sxs-lookup"><span data-stu-id="de084-120">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="de084-121">.NET Core SDK 安裝在多個位置</span><span class="sxs-lookup"><span data-stu-id="de084-121">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="de084-122">在**新專案**對話方塊適用於 ASP.NET Core，您可能會看到下列警告：</span><span class="sxs-lookup"><span data-stu-id="de084-122">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="de084-123">.NET Core SDK 會安裝在多個位置。</span><span class="sxs-lookup"><span data-stu-id="de084-123">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="de084-124">只有在已安裝的 SDK 範本 'c:\\Program Files\\dotnet\\sdk\\' 將會顯示。</span><span class="sxs-lookup"><span data-stu-id="de084-124">Only templates from the SDK(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

<span data-ttu-id="de084-125">您以外的目錄中有至少一個安裝的.NET Core sdk 時，您會看到此訊息*c:\\Program Files\\dotnet\\sdk\\*。</span><span class="sxs-lookup"><span data-stu-id="de084-125">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="de084-126">通常這會使用複製/貼上，而不 MSI 安裝程式在機器上部署.NET Core SDK。</span><span class="sxs-lookup"><span data-stu-id="de084-126">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="de084-127">解除安裝所有 32 位元.NET Core Sdk 和執行階段可避免這個警告。</span><span class="sxs-lookup"><span data-stu-id="de084-127">Uninstall all 32-bit .NET Core SDKs and runtimes to prevent this warning.</span></span> <span data-ttu-id="de084-128">從解除安裝**控制台中** > **程式和功能** > **解除安裝或變更程式**。</span><span class="sxs-lookup"><span data-stu-id="de084-128">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="de084-129">如果您了解警告的發生原因和其隱含意義，您可以忽略此警告。</span><span class="sxs-lookup"><span data-stu-id="de084-129">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="de084-130">偵測到.NET Core Sdk</span><span class="sxs-lookup"><span data-stu-id="de084-130">No .NET Core SDKs were detected</span></span>

* <span data-ttu-id="de084-131">在 Visual Studio**新的專案**對話方塊適用於 ASP.NET Core 中，您可能會看到下列警告：</span><span class="sxs-lookup"><span data-stu-id="de084-131">In the Visual Studio **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

  > <span data-ttu-id="de084-132">偵測到任何.NET Core Sdk，請確定它們會包含在環境變數`PATH`。</span><span class="sxs-lookup"><span data-stu-id="de084-132">No .NET Core SDKs were detected, ensure they are included in the environment variable `PATH`.</span></span>

* <span data-ttu-id="de084-133">執行時`dotnet`命令時，警告會顯示為：</span><span class="sxs-lookup"><span data-stu-id="de084-133">When executing a `dotnet` command, the warning appears as:</span></span>

  > <span data-ttu-id="de084-134">無法找到任何已安裝的 dotnet Sdk。</span><span class="sxs-lookup"><span data-stu-id="de084-134">It was not possible to find any installed dotnet SDKs.</span></span>

<span data-ttu-id="de084-135">這些警告時顯示的環境變數`PATH`未指向任何電腦上的.NET Core Sdk。</span><span class="sxs-lookup"><span data-stu-id="de084-135">These warnings appear when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="de084-136">若要解決此問題：</span><span class="sxs-lookup"><span data-stu-id="de084-136">To resolve this problem:</span></span>

* <span data-ttu-id="de084-137">安裝 .NET Core SDK。</span><span class="sxs-lookup"><span data-stu-id="de084-137">Install the .NET Core SDK.</span></span> <span data-ttu-id="de084-138">取得最新的安裝程式，從[.NET 下載](https://dotnet.microsoft.com/download)。</span><span class="sxs-lookup"><span data-stu-id="de084-138">Obtain the latest installer from [.NET Downloads](https://dotnet.microsoft.com/download).</span></span>
* <span data-ttu-id="de084-139">確認`PATH`環境變數指向安裝 SDK 的位置 (`C:\Program Files\dotnet\`針對 64 位元 x64 或`C:\Program Files (x86)\dotnet\`的 32 位元 x86)。</span><span class="sxs-lookup"><span data-stu-id="de084-139">Verify that the `PATH` environment variable points to the location where the SDK is installed (`C:\Program Files\dotnet\` for 64-bit/x64 or `C:\Program Files (x86)\dotnet\` for 32-bit/x86).</span></span> <span data-ttu-id="de084-140">SDK 安裝程式通常設定`PATH`。</span><span class="sxs-lookup"><span data-stu-id="de084-140">The SDK installer normally sets the `PATH`.</span></span> <span data-ttu-id="de084-141">一律在相同電腦上安裝相同的位元 Sdk 和執行階段。</span><span class="sxs-lookup"><span data-stu-id="de084-141">Always install the same bitness SDKs and runtimes on the same machine.</span></span>

### <a name="missing-sdk-after-installing-the-net-core-hosting-bundle"></a><span data-ttu-id="de084-142">安裝.NET Core 裝載套件組合之後遺漏 SDK</span><span class="sxs-lookup"><span data-stu-id="de084-142">Missing SDK after installing the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="de084-143">安裝[.NET Core 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)修改`PATH`它會安裝.NET Core 執行階段，以指向 32 位元 (x86) 版本的.NET Core (`C:\Program Files (x86)\dotnet\`)。</span><span class="sxs-lookup"><span data-stu-id="de084-143">Installing the [.NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) modifies the `PATH` when it installs the .NET Core runtime to point to the 32-bit (x86) version of .NET Core (`C:\Program Files (x86)\dotnet\`).</span></span> <span data-ttu-id="de084-144">這會導致遺漏 Sdk 時 32 位元 (x86) 的.NET Core`dotnet`使用命令 ([偵測到.NET Core Sdk](#no-net-core-sdks-were-detected))。</span><span class="sxs-lookup"><span data-stu-id="de084-144">This can result in missing SDKs when the 32-bit (x86) .NET Core `dotnet` command is used ([No .NET Core SDKs were detected](#no-net-core-sdks-were-detected)).</span></span> <span data-ttu-id="de084-145">若要解決此問題，請移動`C:\Program Files\dotnet\`到之前的位置`C:\Program Files (x86)\dotnet\`上`PATH`。</span><span class="sxs-lookup"><span data-stu-id="de084-145">To resolve this problem, move `C:\Program Files\dotnet\` to a position before `C:\Program Files (x86)\dotnet\` on the `PATH`.</span></span>

## <a name="obtain-data-from-an-app"></a><span data-ttu-id="de084-146">從應用程式取得資料</span><span class="sxs-lookup"><span data-stu-id="de084-146">Obtain data from an app</span></span>

<span data-ttu-id="de084-147">應用程式是否能夠回應要求，您可以從使用中介軟體應用程式來取得下列資料：</span><span class="sxs-lookup"><span data-stu-id="de084-147">If an app is capable of responding to requests, you can obtain the following data from the app using middleware:</span></span>

* <span data-ttu-id="de084-148">要求&ndash;方法配置、 主機、 pathbase、 路徑、 查詢字串的標頭</span><span class="sxs-lookup"><span data-stu-id="de084-148">Request &ndash; Method, scheme, host, pathbase, path, query string, headers</span></span>
* <span data-ttu-id="de084-149">連接&ndash;遠端 IP 位址、 遠端連接埠、 本機 IP 位址、 本機連接埠、 用戶端憑證</span><span class="sxs-lookup"><span data-stu-id="de084-149">Connection &ndash; Remote IP address, remote port, local IP address, local port, client certificate</span></span>
* <span data-ttu-id="de084-150">身分識別&ndash;名稱、 顯示名稱</span><span class="sxs-lookup"><span data-stu-id="de084-150">Identity &ndash; Name, display name</span></span>
* <span data-ttu-id="de084-151">組態設定</span><span class="sxs-lookup"><span data-stu-id="de084-151">Configuration settings</span></span>
* <span data-ttu-id="de084-152">環境變數</span><span class="sxs-lookup"><span data-stu-id="de084-152">Environment variables</span></span>

<span data-ttu-id="de084-153">下列項目放[中介軟體](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)開頭的程式碼`Startup.Configure`方法的要求處理管線。</span><span class="sxs-lookup"><span data-stu-id="de084-153">Place the following [middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) code at the beginning of the `Startup.Configure` method's request processing pipeline.</span></span> <span data-ttu-id="de084-154">中介軟體會執行以確保只在開發環境中執行的程式碼之前，會檢查環境。</span><span class="sxs-lookup"><span data-stu-id="de084-154">The environment is checked before the middleware is run to ensure that the code is only executed in the Development environment.</span></span>

<span data-ttu-id="de084-155">若要取得環境，請使用下列其中一個方法：</span><span class="sxs-lookup"><span data-stu-id="de084-155">To obtain the environment, use either of the following approaches:</span></span>

* <span data-ttu-id="de084-156">插入`IHostingEnvironment`成`Startup.Configure`方法，並檢查本機變數的環境。</span><span class="sxs-lookup"><span data-stu-id="de084-156">Inject the `IHostingEnvironment` into the `Startup.Configure` method and check the environment with the local variable.</span></span> <span data-ttu-id="de084-157">下列程式碼範例會示範這種方法。</span><span class="sxs-lookup"><span data-stu-id="de084-157">The following sample code demonstrates this approach.</span></span>

* <span data-ttu-id="de084-158">將環境指派給屬性，以在`Startup`類別。</span><span class="sxs-lookup"><span data-stu-id="de084-158">Assign the environment to a property in the `Startup` class.</span></span> <span data-ttu-id="de084-159">檢查環境中使用屬性 (例如`if (Environment.IsDevelopment())`)。</span><span class="sxs-lookup"><span data-stu-id="de084-159">Check the environment using the property (for example, `if (Environment.IsDevelopment())`).</span></span>

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
