---
title: "ASP.NET Core 中的 WebListener 網頁伺服器實作"
author: rick-anderson
description: "介紹 WebListener，這是 Windows 上的 ASP.NET Core Web 伺服器。 建置在 Http.Sys 核心模式驅動程式之上，WebListener 是 Kestrel 的替代方式，可以用於直接連線到網際網路而不使用 IIS。"
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/weblistener
ms.openlocfilehash: fb2e0621645a48f4e603d754d8babbc07a78cae4
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="9bc13-104">ASP.NET Core 中的 WebListener 網頁伺服器實作</span><span class="sxs-lookup"><span data-stu-id="9bc13-104">WebListener web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="9bc13-105">作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="9bc13-105">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="9bc13-106">本主題只適用於 ASP.NET Core 1.x。</span><span class="sxs-lookup"><span data-stu-id="9bc13-106">This topic applies only to ASP.NET Core 1.x.</span></span> <span data-ttu-id="9bc13-107">在 ASP.NET Core 2.0 中，WebListener 名為 [HTTP.sys](httpsys.md)。</span><span class="sxs-lookup"><span data-stu-id="9bc13-107">In ASP.NET Core 2.0, WebListener is named [HTTP.sys](httpsys.md).</span></span>

<span data-ttu-id="9bc13-108">WebListener 是只在 Windows 上執行的 [ASP.NET Core 網頁伺服器](index.md)。</span><span class="sxs-lookup"><span data-stu-id="9bc13-108">WebListener is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="9bc13-109">它建置在 [Http.Sys 核心模式驅動程式](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)之上。</span><span class="sxs-lookup"><span data-stu-id="9bc13-109">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="9bc13-110">WebListener 是 [Kestrel](kestrel.md) 的替代方式，可以用於直接連線到網際網路，而不需依賴 IIS 作為反向 Proxy 伺服器。</span><span class="sxs-lookup"><span data-stu-id="9bc13-110">WebListener is an alternative to [Kestrel](kestrel.md) that can be used for direct connection to the Internet without relying on IIS as a reverse proxy server.</span></span> <span data-ttu-id="9bc13-111">事實上，**WebListener 不能與 IIS 或 IIS Express 搭配使用，因為它與 [ASP.NET Core 模組](aspnet-core-module.md)不相容。**</span><span class="sxs-lookup"><span data-stu-id="9bc13-111">In fact, **WebListener can't be used with IIS or IIS Express, as it isn't compatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="9bc13-112">雖然 WebListener 是針對 ASP.NET Core 所開發，但它可以透過 [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet 套件直接用於任何 .NET Core 或 .NET Framework 應用程式。</span><span class="sxs-lookup"><span data-stu-id="9bc13-112">Although WebListener was developed for ASP.NET Core, it can be used directly in any .NET Core or .NET Framework application via the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

<span data-ttu-id="9bc13-113">WebListener 支援下列功能：</span><span class="sxs-lookup"><span data-stu-id="9bc13-113">WebListener supports the following features:</span></span>

- [<span data-ttu-id="9bc13-114">Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="9bc13-114">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="9bc13-115">連接埠共用</span><span class="sxs-lookup"><span data-stu-id="9bc13-115">Port sharing</span></span>
- <span data-ttu-id="9bc13-116">使用 SNI 的 HTTPS</span><span class="sxs-lookup"><span data-stu-id="9bc13-116">HTTPS with SNI</span></span>
- <span data-ttu-id="9bc13-117">HTTP/2 over TLS (Windows 10)</span><span class="sxs-lookup"><span data-stu-id="9bc13-117">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="9bc13-118">直接檔案傳輸</span><span class="sxs-lookup"><span data-stu-id="9bc13-118">Direct file transmission</span></span>
- <span data-ttu-id="9bc13-119">回應快取</span><span class="sxs-lookup"><span data-stu-id="9bc13-119">Response caching</span></span>
- <span data-ttu-id="9bc13-120">WebSocket (Windows 8)</span><span class="sxs-lookup"><span data-stu-id="9bc13-120">WebSockets (Windows 8)</span></span>

<span data-ttu-id="9bc13-121">支援的 Windows 版本：</span><span class="sxs-lookup"><span data-stu-id="9bc13-121">Supported Windows versions:</span></span>

- <span data-ttu-id="9bc13-122">Windows 7 與 Windows Server 2008 R2 和更新版本</span><span class="sxs-lookup"><span data-stu-id="9bc13-122">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="9bc13-123">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9bc13-123">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-weblistener"></a><span data-ttu-id="9bc13-124">WebListener 使用時機</span><span class="sxs-lookup"><span data-stu-id="9bc13-124">When to use WebListener</span></span>

<span data-ttu-id="9bc13-125">WebListener 可用於您需要直接向網際網路公開伺服器而不使用 IIS 的部署。</span><span class="sxs-lookup"><span data-stu-id="9bc13-125">WebListener is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![Weblistener 直接與網際網路通訊](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="9bc13-127">由於它建置在 Http.Sys 之上，WebListener 不需要反向 Proxy 伺服器以抵禦攻擊。</span><span class="sxs-lookup"><span data-stu-id="9bc13-127">Because it's built on Http.Sys, WebListener doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="9bc13-128">Http.Sys 是成熟的技術，可抵禦許多種類的攻擊，並提供功能完整之網頁伺服器的穩固性、安全性及延展性。</span><span class="sxs-lookup"><span data-stu-id="9bc13-128">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="9bc13-129">IIS 本身在 Http.Sys 之上以 HTTP 接聽程式的形式執行。</span><span class="sxs-lookup"><span data-stu-id="9bc13-129">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span> 

<span data-ttu-id="9bc13-130">當您需要它所提供的其中一個功能無法使用 Kestrel 來取得時，WebListener 也是不錯的內部部署選擇。</span><span class="sxs-lookup"><span data-stu-id="9bc13-130">WebListener is also a good choice for internal deployments when you need one of the features it offers that you can't get by using Kestrel.</span></span>

![Weblistener 直接與內部網路通訊](weblistener/_static/weblistener-to-internal.png)

## <a name="how-to-use-weblistener"></a><span data-ttu-id="9bc13-132">WebListener 使用方法</span><span class="sxs-lookup"><span data-stu-id="9bc13-132">How to use WebListener</span></span>

<span data-ttu-id="9bc13-133">以下是針對主機作業系統和您的 ASP.NET Core 應用程式的安裝工作概觀。</span><span class="sxs-lookup"><span data-stu-id="9bc13-133">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="9bc13-134">設定 Windows Server</span><span class="sxs-lookup"><span data-stu-id="9bc13-134">Configure Windows Server</span></span>

* <span data-ttu-id="9bc13-135">安裝您應用程式所需的 .NET 版本，例如 [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) 或 .NET Framework 4.5.1。</span><span class="sxs-lookup"><span data-stu-id="9bc13-135">Install the version of .NET that your application requires, such as [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) or .NET Framework 4.5.1.</span></span>

* <span data-ttu-id="9bc13-136">預先註冊 URL 前置詞以繫結至 WebListener，然後設定 SSL 憑證</span><span class="sxs-lookup"><span data-stu-id="9bc13-136">Preregister URL prefixes to bind to WebListener, and set up SSL certificates</span></span>

   <span data-ttu-id="9bc13-137">如果您不在 Windows 預先註冊 URL 前置詞，則必須以系統管理員權限執行您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="9bc13-137">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="9bc13-138">唯一的例外是如果您使用 HTTP (不是 HTTPS) 繫結至 localhost，且使用大於 1024 的連接埠號碼；在此情況下不需要系統管理員權限。</span><span class="sxs-lookup"><span data-stu-id="9bc13-138">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case administrator privileges aren't required.</span></span>

   <span data-ttu-id="9bc13-139">如需詳細資料，請參閱本文稍後的[如何預先註冊前置詞和設定 SSL](#preregister-url-prefixes-and-configure-ssl)。</span><span class="sxs-lookup"><span data-stu-id="9bc13-139">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="9bc13-140">開啟防火牆連接埠來允許流量到達 WebListener。</span><span class="sxs-lookup"><span data-stu-id="9bc13-140">Open firewall ports to allow traffic to reach WebListener.</span></span>

   <span data-ttu-id="9bc13-141">您可以使用 netsh.exe 或 [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906)。</span><span class="sxs-lookup"><span data-stu-id="9bc13-141">You can use netsh.exe or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="9bc13-142">另外還有 [Http.Sys 登錄設定](https://support.microsoft.com/kb/820129)。</span><span class="sxs-lookup"><span data-stu-id="9bc13-142">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application"></a><span data-ttu-id="9bc13-143">設定 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="9bc13-143">Configure your ASP.NET Core application</span></span>

* <span data-ttu-id="9bc13-144">安裝 [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="9bc13-144">Install the NuGet package [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span></span> <span data-ttu-id="9bc13-145">這也會安裝 [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) 作為相依性。</span><span class="sxs-lookup"><span data-stu-id="9bc13-145">This also installs [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) as a dependency.</span></span>

* <span data-ttu-id="9bc13-146">在您的 `Main` 方法中，對 [WebHostBuilder](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) 呼叫 `UseWebListener` 擴充方法，並指定需要的任何 WebListener [選項](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs)和[設定](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs)，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="9bc13-146">Call the `UseWebListener` extension method on [WebHostBuilder](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) in your `Main` method, specifying any WebListener [options](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) and [settings](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* <span data-ttu-id="9bc13-147">設定要接聽的 URL 和連接埠</span><span class="sxs-lookup"><span data-stu-id="9bc13-147">Configure URLs and ports to listen on</span></span> 

  <span data-ttu-id="9bc13-148">ASP.NET Core 預設會繫結至 `http://localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="9bc13-148">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="9bc13-149">若要設定 URL 前置詞和連接埠，您可以使用 `UseURLs` 擴充方法、`urls` 命令列引數或 ASP.NET Core 組態系統。</span><span class="sxs-lookup"><span data-stu-id="9bc13-149">To configure URL prefixes and ports, you can use the `UseURLs` extension method, the `urls` command-line argument or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="9bc13-150">如需詳細資訊，請參閱[裝載](../../fundamentals/hosting.md)。</span><span class="sxs-lookup"><span data-stu-id="9bc13-150">For more information, see [Hosting](../../fundamentals/hosting.md).</span></span>

  <span data-ttu-id="9bc13-151">網頁接聽程式會使用 [Http.Sys 前置詞字串格式](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)。</span><span class="sxs-lookup"><span data-stu-id="9bc13-151">Web Listener uses the [Http.Sys prefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span> <span data-ttu-id="9bc13-152">沒有 WebListener 特有的前置詞字串格式需求。</span><span class="sxs-lookup"><span data-stu-id="9bc13-152">There are no prefix string format requirements that are specific to WebListener.</span></span>

  > [!NOTE]
  > <span data-ttu-id="9bc13-153">請確定您在 `UseUrls` 中指定您於伺服器上預先註冊的相同前置詞字串。</span><span class="sxs-lookup"><span data-stu-id="9bc13-153">Make sure that you specify the same prefix strings in `UseUrls` that you preregister on the server.</span></span> 

* <span data-ttu-id="9bc13-154">請確定您的應用程式未設定為執行 IIS 或 IIS Express。</span><span class="sxs-lookup"><span data-stu-id="9bc13-154">Make sure your application isn't configured to run IIS or IIS Express.</span></span>

  <span data-ttu-id="9bc13-155">在 Visual Studio 中，預設啟動設定檔適用於 IIS Express。</span><span class="sxs-lookup"><span data-stu-id="9bc13-155">In Visual Studio, the default launch profile is for IIS Express.</span></span>  <span data-ttu-id="9bc13-156">若要執行專案作為主控台應用程式，您必須手動變更選取的設定檔，如下列螢幕擷取畫面所示。</span><span class="sxs-lookup"><span data-stu-id="9bc13-156">To run the project as a console application you have to manually change the selected profile, as shown in the following screen shot.</span></span>

  ![選取主控台應用程式設定檔](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a><span data-ttu-id="9bc13-158">如何在 ASP.NET Core 之外使用 WebListener</span><span class="sxs-lookup"><span data-stu-id="9bc13-158">How to use WebListener outside of ASP.NET Core</span></span>

* <span data-ttu-id="9bc13-159">安裝 [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="9bc13-159">Install the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

* <span data-ttu-id="9bc13-160">[預先註冊 URL 前置詞以繫結至 WebListener，然後設定 SSL 憑證](#preregister-url-prefixes-and-configure-ssl)，如同用於 ASP.NET Core 一樣。</span><span class="sxs-lookup"><span data-stu-id="9bc13-160">[Preregister URL prefixes to bind to WebListener, and set up SSL certificates](#preregister-url-prefixes-and-configure-ssl) as you would for use in ASP.NET Core.</span></span>

<span data-ttu-id="9bc13-161">另外還有 [Http.Sys 登錄設定](https://support.microsoft.com/kb/820129)。</span><span class="sxs-lookup"><span data-stu-id="9bc13-161">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>


<span data-ttu-id="9bc13-162">以下是示範在 ASP.NET Core 之外使用 WebListener 的程式碼範例：</span><span class="sxs-lookup"><span data-stu-id="9bc13-162">Here's a code sample that demonstrates WebListener use outside of ASP.NET Core:</span></span>

```csharp
var settings = new WebListenerSettings();
settings.UrlPrefixes.Add("http://localhost:8080");

using (WebListener listener = new WebListener(settings))
{
    listener.Start();

    while (true)
    {
        var context = await listener.AcceptAsync();
        byte[] bytes = Encoding.ASCII.GetBytes("Hello World: " + DateTime.Now);
        context.Response.ContentLength = bytes.Length;
        context.Response.ContentType = "text/plain";

        await context.Response.Body.WriteAsync(bytes, 0, bytes.Length);
        context.Dispose();
    }
}
```

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="9bc13-163">預先註冊 URL 前置詞和設定 SSL</span><span class="sxs-lookup"><span data-stu-id="9bc13-163">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="9bc13-164">IIS 和 WebListener 都依賴基礎 Http.Sys 核心模式驅動程式來接聽要求，以及執行初始處理。</span><span class="sxs-lookup"><span data-stu-id="9bc13-164">Both IIS and WebListener rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="9bc13-165">在 IIS 中，管理 UI 可讓您很輕鬆地設定所有項目。</span><span class="sxs-lookup"><span data-stu-id="9bc13-165">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="9bc13-166">不過，如果您要使用 WebListener，則必須自行設定 Http.Sys。</span><span class="sxs-lookup"><span data-stu-id="9bc13-166">However, if you're using WebListener you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="9bc13-167">這麼做的內建工具是 netsh.exe。</span><span class="sxs-lookup"><span data-stu-id="9bc13-167">The built-in tool for doing that's netsh.exe.</span></span> 

<span data-ttu-id="9bc13-168">您必須使用 netsh.exe 的最常見工作是保留 URL 前置詞和指派 SSL 憑證。</span><span class="sxs-lookup"><span data-stu-id="9bc13-168">The most common tasks you need to use netsh.exe for are reserving URL prefixes and assigning SSL certificates.</span></span>

<span data-ttu-id="9bc13-169">NetSh.exe 不是適用於初學者的簡單工具。</span><span class="sxs-lookup"><span data-stu-id="9bc13-169">NetSh.exe isn't an easy tool to use for beginners.</span></span> <span data-ttu-id="9bc13-170">下列範例會示範保留連接埠 80 和 443 的 URL 前置詞時所需的最小值：</span><span class="sxs-lookup"><span data-stu-id="9bc13-170">The following example shows the bare minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="9bc13-171">下列範例會示範如何指派 SSL 憑證：</span><span class="sxs-lookup"><span data-stu-id="9bc13-171">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}".
```

<span data-ttu-id="9bc13-172">以下是正式參考文件：</span><span class="sxs-lookup"><span data-stu-id="9bc13-172">Here is the official reference documentation:</span></span>

* [<span data-ttu-id="9bc13-173">超文字傳輸通訊協定 (HTTP) 的 Netsh 命令</span><span class="sxs-lookup"><span data-stu-id="9bc13-173">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
* [<span data-ttu-id="9bc13-174">UrlPrefix 字串</span><span class="sxs-lookup"><span data-stu-id="9bc13-174">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

<span data-ttu-id="9bc13-175">下列資源提供數種案例的詳細指示。</span><span class="sxs-lookup"><span data-stu-id="9bc13-175">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="9bc13-176">提到 `HttpListener` 的文章同樣適用於 `WebListener`，因為兩者都是根據 Http.Sys。</span><span class="sxs-lookup"><span data-stu-id="9bc13-176">Articles that refer to `HttpListener` apply equally to `WebListener`, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="9bc13-177">如何：使用 SSL 憑證設定連接埠</span><span class="sxs-lookup"><span data-stu-id="9bc13-177">How to: Configure a Port with an SSL Certificate</span></span>](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="9bc13-178">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) (HTTPS 通訊 - 以 HttpListener 為基礎的裝載和用戶端憑證) 這是協力廠商部落格，相當老舊但仍有有用的資訊。</span><span class="sxs-lookup"><span data-stu-id="9bc13-178">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="9bc13-179">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) (如何：逐步解說使用 HttpListener 或 Http Server unmanaged 程式碼 (C++) 作為 SSL 簡單伺服器) 這也是具有有用資訊的較舊部落格。</span><span class="sxs-lookup"><span data-stu-id="9bc13-179">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>
* <span data-ttu-id="9bc13-180">[How Do I Set Up A .NET Core WebListener With SSL?](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/) (如何使用 SSL 設定 .NET Core WebListener？)</span><span class="sxs-lookup"><span data-stu-id="9bc13-180">[How Do I Set Up A .NET Core WebListener With SSL?](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/)</span></span>

<span data-ttu-id="9bc13-181">以下是一些協力廠商工具，比起 netsh.exe 命令列可以更輕鬆地使用。</span><span class="sxs-lookup"><span data-stu-id="9bc13-181">Here are some third-party tools that can be easier to use than the netsh.exe command line.</span></span> <span data-ttu-id="9bc13-182">這些不是由 Microsoft 提供，或經由 Microsoft 背書。</span><span class="sxs-lookup"><span data-stu-id="9bc13-182">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="9bc13-183">工具預設會以系統管理員身分執行，因為 netsh.exe 本身需要系統管理員權限。</span><span class="sxs-lookup"><span data-stu-id="9bc13-183">The tools run as administrator by default, since netsh.exe itself requires administrator privileges.</span></span>

* <span data-ttu-id="9bc13-184">[http.sys Manager](http://httpsysmanager.codeplex.com/) 提供 UI 以便列出和設定 SSL 憑證與選項、前置詞保留，以及憑證信任清單。</span><span class="sxs-lookup"><span data-stu-id="9bc13-184">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="9bc13-185">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) 可讓您列出或設定 SSL 憑證與 URL 前置詞。</span><span class="sxs-lookup"><span data-stu-id="9bc13-185">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="9bc13-186">UI 比 http.sys Manager 更精簡，並公開更多組態選項，但在其他方面提供類似的功能。</span><span class="sxs-lookup"><span data-stu-id="9bc13-186">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="9bc13-187">它無法建立新的憑證信任清單 (CTL)，但可以指派現有的憑證信任清單。</span><span class="sxs-lookup"><span data-stu-id="9bc13-187">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

<span data-ttu-id="9bc13-188">為了產生自我簽署的 SSL 憑證，Microsoft 提供命令列工具：[MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) 和 PowerShell Cmdlet [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate)。</span><span class="sxs-lookup"><span data-stu-id="9bc13-188">For generating self-signed SSL certificates, Microsoft provides command-line tools: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) and the PowerShell cmdlet [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span></span> <span data-ttu-id="9bc13-189">另外還有協力廠商 UI 工具，可讓您更輕鬆地產生自我簽署的 SSL 憑證：</span><span class="sxs-lookup"><span data-stu-id="9bc13-189">There are also third-party UI tools that make it easier for you to generate self-signed SSL certificates:</span></span>

* [<span data-ttu-id="9bc13-190">SelfCert</span><span class="sxs-lookup"><span data-stu-id="9bc13-190">SelfCert</span></span>](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [<span data-ttu-id="9bc13-191">Makecert UI</span><span class="sxs-lookup"><span data-stu-id="9bc13-191">Makecert UI</span></span>](http://makecertui.codeplex.com/)

## <a name="next-steps"></a><span data-ttu-id="9bc13-192">後續步驟</span><span class="sxs-lookup"><span data-stu-id="9bc13-192">Next steps</span></span>

<span data-ttu-id="9bc13-193">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="9bc13-193">For more information, see the following resources:</span></span>

* [<span data-ttu-id="9bc13-194">本文的範例應用程式</span><span class="sxs-lookup"><span data-stu-id="9bc13-194">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [<span data-ttu-id="9bc13-195">WebListener 原始程式碼</span><span class="sxs-lookup"><span data-stu-id="9bc13-195">WebListener source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="9bc13-196">裝載</span><span class="sxs-lookup"><span data-stu-id="9bc13-196">Hosting</span></span>](../hosting.md)
