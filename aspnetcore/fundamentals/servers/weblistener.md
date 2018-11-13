---
title: ASP.NET Core 中的 WebListener 網頁伺服器實作
author: guardrex
description: 了解 WebListener，這是 Windows 上 ASP.NET Core 的網頁伺服器，可用於直接連線到網際網路而無需 IIS。
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: fundamentals/servers/weblistener
ms.openlocfilehash: eaf76a44bc7750aef94319042e61aa294c6cba35
ms.sourcegitcommit: 09affee3d234cb27ea6fe33bc113b79e68900d22
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/06/2018
ms.locfileid: "51191265"
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="fa3cc-103">ASP.NET Core 中的 WebListener 網頁伺服器實作</span><span class="sxs-lookup"><span data-stu-id="fa3cc-103">WebListener web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="fa3cc-104">作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="fa3cc-104">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="fa3cc-105">本主題只適用於 ASP.NET Core 1.x。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-105">This topic applies only to ASP.NET Core 1.x.</span></span> <span data-ttu-id="fa3cc-106">在 ASP.NET Core 2.0 中，WebListener 名為 [HTTP.sys](httpsys.md)。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-106">In ASP.NET Core 2.0, WebListener is named [HTTP.sys](httpsys.md).</span></span>

<span data-ttu-id="fa3cc-107">WebListener 是只在 Windows 上執行的 [ASP.NET Core 網頁伺服器](index.md)。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-107">WebListener is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="fa3cc-108">它建置在 [Http.Sys 核心模式驅動程式](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)之上。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-108">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="fa3cc-109">WebListener 是 [Kestrel](kestrel.md) 的替代方式，可以用於直接連線到網際網路，而不需依賴 IIS 作為反向 Proxy 伺服器。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-109">WebListener is an alternative to [Kestrel](kestrel.md) that can be used for direct connection to the Internet without relying on IIS as a reverse proxy server.</span></span> <span data-ttu-id="fa3cc-110">事實上，**WebListener 不能與 IIS 或 IIS Express 搭配使用，因為它與 [ASP.NET Core 模組](aspnet-core-module.md)不相容。**</span><span class="sxs-lookup"><span data-stu-id="fa3cc-110">In fact, **WebListener can't be used with IIS or IIS Express, as it isn't compatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="fa3cc-111">雖然 WebListener 是針對 ASP.NET Core 所開發，但它可以透過 [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet 套件直接用於任何 .NET Core 或 .NET Framework 應用程式。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-111">Although WebListener was developed for ASP.NET Core, it can be used directly in any .NET Core or .NET Framework application via the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

<span data-ttu-id="fa3cc-112">WebListener 支援下列功能：</span><span class="sxs-lookup"><span data-stu-id="fa3cc-112">WebListener supports the following features:</span></span>

- [<span data-ttu-id="fa3cc-113">Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="fa3cc-113">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="fa3cc-114">連接埠共用</span><span class="sxs-lookup"><span data-stu-id="fa3cc-114">Port sharing</span></span>
- <span data-ttu-id="fa3cc-115">使用 SNI 的 HTTPS</span><span class="sxs-lookup"><span data-stu-id="fa3cc-115">HTTPS with SNI</span></span>
- <span data-ttu-id="fa3cc-116">HTTP/2 over TLS (Windows 10)</span><span class="sxs-lookup"><span data-stu-id="fa3cc-116">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="fa3cc-117">直接檔案傳輸</span><span class="sxs-lookup"><span data-stu-id="fa3cc-117">Direct file transmission</span></span>
- <span data-ttu-id="fa3cc-118">回應快取</span><span class="sxs-lookup"><span data-stu-id="fa3cc-118">Response caching</span></span>
- <span data-ttu-id="fa3cc-119">WebSocket (Windows 8)</span><span class="sxs-lookup"><span data-stu-id="fa3cc-119">WebSockets (Windows 8)</span></span>

<span data-ttu-id="fa3cc-120">支援的 Windows 版本：</span><span class="sxs-lookup"><span data-stu-id="fa3cc-120">Supported Windows versions:</span></span>

- <span data-ttu-id="fa3cc-121">Windows 7 與 Windows Server 2008 R2 和更新版本</span><span class="sxs-lookup"><span data-stu-id="fa3cc-121">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="fa3cc-122">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fa3cc-122">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-weblistener"></a><span data-ttu-id="fa3cc-123">WebListener 使用時機</span><span class="sxs-lookup"><span data-stu-id="fa3cc-123">When to use WebListener</span></span>

<span data-ttu-id="fa3cc-124">WebListener 可用於您需要直接向網際網路公開伺服器而不使用 IIS 的部署。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-124">WebListener is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![Weblistener 直接與網際網路通訊](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="fa3cc-126">由於它建置在 Http.Sys 之上，WebListener 不需要反向 Proxy 伺服器以抵禦攻擊。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-126">Because it's built on Http.Sys, WebListener doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="fa3cc-127">Http.Sys 是成熟的技術，可抵禦許多種類的攻擊，並提供功能完整之網頁伺服器的穩固性、安全性及延展性。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-127">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="fa3cc-128">IIS 本身在 Http.Sys 之上以 HTTP 接聽程式的形式執行。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-128">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span>

<span data-ttu-id="fa3cc-129">當您需要它所提供的其中一個功能無法使用 Kestrel 來取得時，WebListener 也是不錯的內部部署選擇。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-129">WebListener is also a good choice for internal deployments when you need one of the features it offers that you can't get by using Kestrel.</span></span>

![Weblistener 直接與內部網路通訊](weblistener/_static/weblistener-to-internal.png)

## <a name="kernel-mode-authentication-with-kerberos"></a><span data-ttu-id="fa3cc-131">使用 Kerberos 的核心模式驗證</span><span class="sxs-lookup"><span data-stu-id="fa3cc-131">Kernel mode authentication with Kerberos</span></span>

<span data-ttu-id="fa3cc-132">WebListener 使用 Kerberos 驗證通訊協定委派給核心模式驗證。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-132">WebListener delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="fa3cc-133">Kerberos 和 WebListener 不支援使用者模式驗證。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-133">User mode authentication isn't supported with Kerberos and WebListener.</span></span> <span data-ttu-id="fa3cc-134">必須使用電腦帳戶來解密 Kerberos 權杖/票證，該權杖/票證取自 Active Directory，並由用戶端將其轉送至伺服器來驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-134">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="fa3cc-135">請註冊主機的服務主體名稱 (SPN)，而非應用程式的使用者。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-135">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

## <a name="how-to-use-weblistener"></a><span data-ttu-id="fa3cc-136">WebListener 使用方法</span><span class="sxs-lookup"><span data-stu-id="fa3cc-136">How to use WebListener</span></span>

<span data-ttu-id="fa3cc-137">以下是針對主機作業系統和您的 ASP.NET Core 應用程式的安裝工作概觀。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-137">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="fa3cc-138">設定 Windows Server</span><span class="sxs-lookup"><span data-stu-id="fa3cc-138">Configure Windows Server</span></span>

* <span data-ttu-id="fa3cc-139">安裝您應用程式所需的 .NET 版本，例如 [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) 或 .NET Framework 4.5.1。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-139">Install the version of .NET that your application requires, such as [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) or .NET Framework 4.5.1.</span></span>

* <span data-ttu-id="fa3cc-140">預先註冊 URL 前置詞以繫結至 WebListener，然後設定 SSL 憑證</span><span class="sxs-lookup"><span data-stu-id="fa3cc-140">Preregister URL prefixes to bind to WebListener, and set up SSL certificates</span></span>

   <span data-ttu-id="fa3cc-141">如果您不在 Windows 預先註冊 URL 前置詞，則必須以系統管理員權限執行您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-141">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="fa3cc-142">唯一的例外是如果您使用 HTTP (不是 HTTPS) 繫結至 localhost，且使用大於 1024 的連接埠號碼；在此情況下不需要系統管理員權限。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-142">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case administrator privileges aren't required.</span></span>

   <span data-ttu-id="fa3cc-143">如需詳細資料，請參閱本文稍後的[如何預先註冊前置詞和設定 SSL](#preregister-url-prefixes-and-configure-ssl)。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-143">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="fa3cc-144">開啟防火牆連接埠來允許流量到達 WebListener。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-144">Open firewall ports to allow traffic to reach WebListener.</span></span>

   <span data-ttu-id="fa3cc-145">您可以使用 netsh.exe 或 [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906)。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-145">You can use netsh.exe or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="fa3cc-146">另外還有 [Http.Sys 登錄設定](https://support.microsoft.com/kb/820129)。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-146">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application"></a><span data-ttu-id="fa3cc-147">設定 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="fa3cc-147">Configure your ASP.NET Core application</span></span>

* <span data-ttu-id="fa3cc-148">安裝 [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-148">Install the NuGet package [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span></span> <span data-ttu-id="fa3cc-149">這也會安裝 [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) 作為相依性。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-149">This also installs [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) as a dependency.</span></span>

* <span data-ttu-id="fa3cc-150">在您的 `Main` 方法中，對 [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) 呼叫 `UseWebListener` 擴充方法，並指定需要的任何 WebListener [選項](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs)和[設定](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs)，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="fa3cc-150">Call the `UseWebListener` extension method on [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) in your `Main` method, specifying any WebListener [options](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) and [settings](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* <span data-ttu-id="fa3cc-151">設定要接聽的 URL 和連接埠</span><span class="sxs-lookup"><span data-stu-id="fa3cc-151">Configure URLs and ports to listen on</span></span> 

  <span data-ttu-id="fa3cc-152">ASP.NET Core 預設會繫結至 `http://localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-152">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="fa3cc-153">若要設定 URL 前置詞和連接埠，您可以使用 `UseURLs` 擴充方法、`urls` 命令列引數或 ASP.NET Core 組態系統。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-153">To configure URL prefixes and ports, you can use the `UseURLs` extension method, the `urls` command-line argument or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="fa3cc-154">如需詳細資訊，請參閱 [在 ASP.NET Core 中代管(xref:fundamentals/host/index)。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-154">For more information, see [Host in ASP.NET Core(xref:fundamentals/host/index).</span></span>

  <span data-ttu-id="fa3cc-155">網頁接聽程式會使用 [Http.Sys 前置詞字串格式](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-155">Web Listener uses the [Http.Sys prefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span> <span data-ttu-id="fa3cc-156">沒有 WebListener 特有的前置詞字串格式需求。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-156">There are no prefix string format requirements that are specific to WebListener.</span></span>

  > [!WARNING]
  > <span data-ttu-id="fa3cc-157">請**勿**使用最上層萬用字元繫結 (`http://*:80/`與 `http://+:80`)。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-157">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="fa3cc-158">最上層萬用字元繫結可能暴露您的應用程式安全性弱點。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-158">Top-level wildcard bindings can open up your app to security vulnerabilities.</span></span> <span data-ttu-id="fa3cc-159">這對強式與弱式萬用字元皆適用。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-159">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="fa3cc-160">請使用明確主機名稱，而非萬用字元。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-160">Use explicit host names rather than wildcards.</span></span> <span data-ttu-id="fa3cc-161">若您擁有整個父網域 (與具弱點的 `*.com` 相對) 的控制權，則子網域萬用字元繫結 (例如 `*.mysub.com`) 就沒有此安全性風險。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-161">Subdomain wildcard binding (for example, `*.mysub.com`) doesn't have this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="fa3cc-162">如需詳細資訊，請參閱 [rfc7230 5.4 節](https://tools.ietf.org/html/rfc7230#section-5.4)。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-162">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

  > [!NOTE]
  > <span data-ttu-id="fa3cc-163">請確定您在 `UseUrls` 中指定您於伺服器上預先註冊的相同前置詞字串。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-163">Make sure that you specify the same prefix strings in `UseUrls` that you preregister on the server.</span></span> 

* <span data-ttu-id="fa3cc-164">請確定您的應用程式未設定為執行 IIS 或 IIS Express。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-164">Make sure your application isn't configured to run IIS or IIS Express.</span></span>

  <span data-ttu-id="fa3cc-165">在 Visual Studio 中，預設啟動設定檔適用於 IIS Express。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-165">In Visual Studio, the default launch profile is for IIS Express.</span></span>  <span data-ttu-id="fa3cc-166">若要執行專案作為主控台應用程式，您必須手動變更選取的設定檔，如下列螢幕擷取畫面所示。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-166">To run the project as a console application you have to manually change the selected profile, as shown in the following screen shot.</span></span>

  ![選取主控台應用程式設定檔](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a><span data-ttu-id="fa3cc-168">如何在 ASP.NET Core 之外使用 WebListener</span><span class="sxs-lookup"><span data-stu-id="fa3cc-168">How to use WebListener outside of ASP.NET Core</span></span>

* <span data-ttu-id="fa3cc-169">安裝 [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-169">Install the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

* <span data-ttu-id="fa3cc-170">[預先註冊 URL 前置詞以繫結至 WebListener，然後設定 SSL 憑證](#preregister-url-prefixes-and-configure-ssl)，如同用於 ASP.NET Core 一樣。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-170">[Preregister URL prefixes to bind to WebListener, and set up SSL certificates](#preregister-url-prefixes-and-configure-ssl) as you would for use in ASP.NET Core.</span></span>

<span data-ttu-id="fa3cc-171">另外還有 [Http.Sys 登錄設定](https://support.microsoft.com/kb/820129)。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-171">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>


<span data-ttu-id="fa3cc-172">以下是示範在 ASP.NET Core 之外使用 WebListener 的程式碼範例：</span><span class="sxs-lookup"><span data-stu-id="fa3cc-172">Here's a code sample that demonstrates WebListener use outside of ASP.NET Core:</span></span>

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

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="fa3cc-173">預先註冊 URL 前置詞和設定 SSL</span><span class="sxs-lookup"><span data-stu-id="fa3cc-173">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="fa3cc-174">IIS 和 WebListener 都依賴基礎 Http.Sys 核心模式驅動程式來接聽要求，以及執行初始處理。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-174">Both IIS and WebListener rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="fa3cc-175">在 IIS 中，管理 UI 可讓您很輕鬆地設定所有項目。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-175">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="fa3cc-176">不過，如果您要使用 WebListener，則必須自行設定 Http.Sys。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-176">However, if you're using WebListener you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="fa3cc-177">這麼做的內建工具是 netsh.exe。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-177">The built-in tool for doing that's netsh.exe.</span></span> 

<span data-ttu-id="fa3cc-178">您必須使用 netsh.exe 的最常見工作是保留 URL 前置詞和指派 SSL 憑證。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-178">The most common tasks you need to use netsh.exe for are reserving URL prefixes and assigning SSL certificates.</span></span>

<span data-ttu-id="fa3cc-179">NetSh.exe 不是適用於初學者的簡單工具。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-179">NetSh.exe isn't an easy tool to use for beginners.</span></span> <span data-ttu-id="fa3cc-180">下列範例會示範保留連接埠 80 和 443 的 URL 前置詞時所需的最小值：</span><span class="sxs-lookup"><span data-stu-id="fa3cc-180">The following example shows the bare minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="fa3cc-181">下列範例會示範如何指派 SSL 憑證：</span><span class="sxs-lookup"><span data-stu-id="fa3cc-181">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid="{00000000-0000-0000-0000-000000000000}".
```

<span data-ttu-id="fa3cc-182">以下是正式參考文件：</span><span class="sxs-lookup"><span data-stu-id="fa3cc-182">Here is the official reference documentation:</span></span>

* [<span data-ttu-id="fa3cc-183">超文字傳輸通訊協定 (HTTP) 的 Netsh 命令</span><span class="sxs-lookup"><span data-stu-id="fa3cc-183">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
* [<span data-ttu-id="fa3cc-184">UrlPrefix 字串</span><span class="sxs-lookup"><span data-stu-id="fa3cc-184">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

<span data-ttu-id="fa3cc-185">下列資源提供數種案例的詳細指示。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-185">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="fa3cc-186">提到 `HttpListener` 的文章同樣適用於 `WebListener`，因為兩者都是根據 Http.Sys。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-186">Articles that refer to `HttpListener` apply equally to `WebListener`, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="fa3cc-187">如何：使用 SSL 憑證設定連接埠</span><span class="sxs-lookup"><span data-stu-id="fa3cc-187">How to: Configure a Port with an SSL Certificate</span></span>](/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="fa3cc-188">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) (HTTPS 通訊 - 以 HttpListener 為基礎的裝載和用戶端憑證) 這是協力廠商部落格，相當老舊但仍有有用的資訊。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-188">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="fa3cc-189">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) (如何：逐步解說使用 HttpListener 或 Http Server unmanaged 程式碼 (C++) 作為 SSL 簡單伺服器) 這也是具有有用資訊的較舊部落格。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-189">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>
* <span data-ttu-id="fa3cc-190">[How Do I Set Up A .NET Core WebListener With SSL?](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/) (如何使用 SSL 設定 .NET Core WebListener？)</span><span class="sxs-lookup"><span data-stu-id="fa3cc-190">[How Do I Set Up A .NET Core WebListener With SSL?](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/)</span></span>

<span data-ttu-id="fa3cc-191">以下是一些協力廠商工具，比起 netsh.exe 命令列可以更輕鬆地使用。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-191">Here are some third-party tools that can be easier to use than the netsh.exe command line.</span></span> <span data-ttu-id="fa3cc-192">這些不是由 Microsoft 提供，或經由 Microsoft 背書。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-192">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="fa3cc-193">工具預設會以系統管理員身分執行，因為 netsh.exe 本身需要系統管理員權限。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-193">The tools run as administrator by default, since netsh.exe itself requires administrator privileges.</span></span>

* <span data-ttu-id="fa3cc-194">[http.sys Manager](http://httpsysmanager.codeplex.com/) 提供 UI 以便列出和設定 SSL 憑證與選項、前置詞保留，以及憑證信任清單。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-194">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="fa3cc-195">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) 可讓您列出或設定 SSL 憑證與 URL 前置詞。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-195">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="fa3cc-196">UI 比 http.sys Manager 更精簡，並公開更多組態選項，但在其他方面提供類似的功能。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-196">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="fa3cc-197">它無法建立新的憑證信任清單 (CTL)，但可以指派現有的憑證信任清單。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-197">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

<span data-ttu-id="fa3cc-198">為了產生自我簽署的 SSL 憑證，Microsoft 提供命令列工具：[MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) 和 PowerShell Cmdlet [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate)。</span><span class="sxs-lookup"><span data-stu-id="fa3cc-198">For generating self-signed SSL certificates, Microsoft provides command-line tools: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) and the PowerShell cmdlet [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span></span> <span data-ttu-id="fa3cc-199">另外還有協力廠商 UI 工具，可讓您更輕鬆地產生自我簽署的 SSL 憑證：</span><span class="sxs-lookup"><span data-stu-id="fa3cc-199">There are also third-party UI tools that make it easier for you to generate self-signed SSL certificates:</span></span>

* [<span data-ttu-id="fa3cc-200">SelfCert</span><span class="sxs-lookup"><span data-stu-id="fa3cc-200">SelfCert</span></span>](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [<span data-ttu-id="fa3cc-201">Makecert UI</span><span class="sxs-lookup"><span data-stu-id="fa3cc-201">Makecert UI</span></span>](http://makecertui.codeplex.com/)

## <a name="next-steps"></a><span data-ttu-id="fa3cc-202">後續步驟</span><span class="sxs-lookup"><span data-stu-id="fa3cc-202">Next steps</span></span>

<span data-ttu-id="fa3cc-203">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="fa3cc-203">For more information, see the following resources:</span></span>

* [<span data-ttu-id="fa3cc-204">本文的範例應用程式</span><span class="sxs-lookup"><span data-stu-id="fa3cc-204">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [<span data-ttu-id="fa3cc-205">WebListener 原始程式碼</span><span class="sxs-lookup"><span data-stu-id="fa3cc-205">WebListener source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="fa3cc-206">裝載</span><span class="sxs-lookup"><span data-stu-id="fa3cc-206">Hosting</span></span>](xref:fundamentals/host/index)
