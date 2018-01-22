---
title: "ASP.NET Core WebListener web 伺服器實作"
author: rick-anderson
description: "導入了 WebListener，適用於在 Windows 上的 ASP.NET Core web 伺服器。 建置在 Http.Sys 核心模式驅動程式，WebListener 是可以用於 IIS 沒有網際網路直接連線的 Kestrel 的替代方式。"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/weblistener
ms.openlocfilehash: f1bdbc723e4602c2e53723aff91ec5d254f4bd93
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="51e5c-104">ASP.NET Core WebListener web 伺服器實作</span><span class="sxs-lookup"><span data-stu-id="51e5c-104">WebListener web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="51e5c-105">由[Tom Dykstra](https://github.com/tdykstra)和[Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="51e5c-105">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="51e5c-106">本主題只適用於 ASP.NET Core 1.x。</span><span class="sxs-lookup"><span data-stu-id="51e5c-106">This topic applies only to ASP.NET Core 1.x.</span></span> <span data-ttu-id="51e5c-107">在 ASP.NET Core 2.0 中，名為 WebListener [HTTP.sys](httpsys.md)。</span><span class="sxs-lookup"><span data-stu-id="51e5c-107">In ASP.NET Core 2.0, WebListener is named [HTTP.sys](httpsys.md).</span></span>

<span data-ttu-id="51e5c-108">WebListener 是[適用於 ASP.NET Core web 伺服器](index.md)只能在 Windows 上執行。</span><span class="sxs-lookup"><span data-stu-id="51e5c-108">WebListener is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="51e5c-109">此系統建置[Http.Sys 核心模式驅動程式](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)。</span><span class="sxs-lookup"><span data-stu-id="51e5c-109">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="51e5c-110">WebListener 是替代[Kestrel](kestrel.md) ，可用來直接連線到網際網路不需依賴 IIS 做為反向 proxy 伺服器。</span><span class="sxs-lookup"><span data-stu-id="51e5c-110">WebListener is an alternative to [Kestrel](kestrel.md) that can be used for direct connection to the Internet without relying on IIS as a reverse proxy server.</span></span> <span data-ttu-id="51e5c-111">事實上， **WebListener 不能與 IIS 或 IIS Express，並不相容於[ASP.NET 核心模組](aspnet-core-module.md)。**</span><span class="sxs-lookup"><span data-stu-id="51e5c-111">In fact, **WebListener can't be used with IIS or IIS Express, as it isn't compatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="51e5c-112">雖然 WebListener 針對 ASP.NET Core 開發，它可以用任何.NET Core 或.NET Framework 應用程式透過直接在[Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="51e5c-112">Although WebListener was developed for ASP.NET Core, it can be used directly in any .NET Core or .NET Framework application via the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

<span data-ttu-id="51e5c-113">WebListener 支援下列功能：</span><span class="sxs-lookup"><span data-stu-id="51e5c-113">WebListener supports the following features:</span></span>

- [<span data-ttu-id="51e5c-114">Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="51e5c-114">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="51e5c-115">連接埠共用</span><span class="sxs-lookup"><span data-stu-id="51e5c-115">Port sharing</span></span>
- <span data-ttu-id="51e5c-116">SNI 使用 HTTPS</span><span class="sxs-lookup"><span data-stu-id="51e5c-116">HTTPS with SNI</span></span>
- <span data-ttu-id="51e5c-117">HTTP/2 5060，TLS (Windows 10)</span><span class="sxs-lookup"><span data-stu-id="51e5c-117">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="51e5c-118">直接檔案傳輸</span><span class="sxs-lookup"><span data-stu-id="51e5c-118">Direct file transmission</span></span>
- <span data-ttu-id="51e5c-119">回應快取</span><span class="sxs-lookup"><span data-stu-id="51e5c-119">Response caching</span></span>
- <span data-ttu-id="51e5c-120">WebSockets (Windows 8)</span><span class="sxs-lookup"><span data-stu-id="51e5c-120">WebSockets (Windows 8)</span></span>

<span data-ttu-id="51e5c-121">支援的 Windows 版本：</span><span class="sxs-lookup"><span data-stu-id="51e5c-121">Supported Windows versions:</span></span>

- <span data-ttu-id="51e5c-122">Windows 7 和 Windows Server 2008 R2 和更新版本</span><span class="sxs-lookup"><span data-stu-id="51e5c-122">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="51e5c-123">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="51e5c-123">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-weblistener"></a><span data-ttu-id="51e5c-124">何時使用 WebListener</span><span class="sxs-lookup"><span data-stu-id="51e5c-124">When to use WebListener</span></span>

<span data-ttu-id="51e5c-125">WebListener 可用於部署您要公開，直接向網際網路伺服器而不使用 IIS。</span><span class="sxs-lookup"><span data-stu-id="51e5c-125">WebListener is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![Weblistener 直接與網際網路通訊](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="51e5c-127">由於它建置在 Http.Sys，WebListener 不需要保護以對抗攻擊反向 proxy 伺服器。</span><span class="sxs-lookup"><span data-stu-id="51e5c-127">Because it's built on Http.Sys, WebListener doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="51e5c-128">Http.Sys 是成熟的技術，可保護免於許多種類的攻擊，並提供強固性、 安全性及功能完整的 web 伺服器的延展性。</span><span class="sxs-lookup"><span data-stu-id="51e5c-128">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="51e5c-129">為 HTTP 接聽程式在 Http.Sys 之上，IIS 本身的執行。</span><span class="sxs-lookup"><span data-stu-id="51e5c-129">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span> 

<span data-ttu-id="51e5c-130">WebListener 也是不錯的選擇內部部署，當您需要的功能，您無法使用 Kestrel 來取得它所提供的其中一個。</span><span class="sxs-lookup"><span data-stu-id="51e5c-130">WebListener is also a good choice for internal deployments when you need one of the features it offers that you can't get by using Kestrel.</span></span>

![Weblistener 直接與內部網路通訊](weblistener/_static/weblistener-to-internal.png)

## <a name="how-to-use-weblistener"></a><span data-ttu-id="51e5c-132">如何使用 WebListener</span><span class="sxs-lookup"><span data-stu-id="51e5c-132">How to use WebListener</span></span>

<span data-ttu-id="51e5c-133">以下是針對主機作業系統和您的 ASP.NET Core 應用程式安裝工作的概觀。</span><span class="sxs-lookup"><span data-stu-id="51e5c-133">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="51e5c-134">設定 Windows Server</span><span class="sxs-lookup"><span data-stu-id="51e5c-134">Configure Windows Server</span></span>

* <span data-ttu-id="51e5c-135">安裝新版的.NET 應用程式所需，例如[.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe)或.NET Framework 4.5.1。</span><span class="sxs-lookup"><span data-stu-id="51e5c-135">Install the version of .NET that your application requires, such as [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) or .NET Framework 4.5.1.</span></span>

* <span data-ttu-id="51e5c-136">__'Asverify'__ 繫結至 WebListener，以及設定 SSL 憑證的 URL 前置詞</span><span class="sxs-lookup"><span data-stu-id="51e5c-136">Preregister URL prefixes to bind to WebListener, and set up SSL certificates</span></span>

   <span data-ttu-id="51e5c-137">如果您不要 __'asverify'__ URL 前置詞，在 Windows 中的，您必須以系統管理員權限執行您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="51e5c-137">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="51e5c-138">唯一的例外是如果您將繫結使用 HTTP (不是 HTTPS) 連接埠號碼大於 1024; localhost在此情況下，系統管理員權限不需要的。</span><span class="sxs-lookup"><span data-stu-id="51e5c-138">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case administrator privileges aren't required.</span></span>

   <span data-ttu-id="51e5c-139">如需詳細資訊，請參閱[如何 __'asverify'__ 前置詞及設定 SSL](#preregister-url-prefixes-and-configure-ssl)本文稍後。</span><span class="sxs-lookup"><span data-stu-id="51e5c-139">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="51e5c-140">開啟防火牆連接埠來允許流量到達 WebListener。</span><span class="sxs-lookup"><span data-stu-id="51e5c-140">Open firewall ports to allow traffic to reach WebListener.</span></span>

   <span data-ttu-id="51e5c-141">您可以使用 netsh.exe 或[PowerShell cmdlet](https://technet.microsoft.com/library/jj554906)。</span><span class="sxs-lookup"><span data-stu-id="51e5c-141">You can use netsh.exe or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="51e5c-142">另外還有[Http.Sys 登錄設定](https://support.microsoft.com/kb/820129)。</span><span class="sxs-lookup"><span data-stu-id="51e5c-142">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application"></a><span data-ttu-id="51e5c-143">設定 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="51e5c-143">Configure your ASP.NET Core application</span></span>

* <span data-ttu-id="51e5c-144">安裝 NuGet 套件[Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/)。</span><span class="sxs-lookup"><span data-stu-id="51e5c-144">Install the NuGet package [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span></span> <span data-ttu-id="51e5c-145">這樣也會安裝[Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/)做為相依性。</span><span class="sxs-lookup"><span data-stu-id="51e5c-145">This also installs [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) as a dependency.</span></span>

* <span data-ttu-id="51e5c-146">呼叫`UseWebListener`上的擴充方法[WebHostBuilder](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder)中您`Main`方法，並指定任何 WebListener[選項](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs)和[設定](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs)需要如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="51e5c-146">Call the `UseWebListener` extension method on [WebHostBuilder](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) in your `Main` method, specifying any WebListener [options](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) and [settings](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* <span data-ttu-id="51e5c-147">設定 Url 和連接埠上接聽</span><span class="sxs-lookup"><span data-stu-id="51e5c-147">Configure URLs and ports to listen on</span></span> 

  <span data-ttu-id="51e5c-148">根據預設 ASP.NET Core 繫結至`http://localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="51e5c-148">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="51e5c-149">若要設定 URL 前置詞和連接埠，您可以使用`UseURLs`擴充方法，`urls`命令列引數或 ASP.NET Core 組態系統。</span><span class="sxs-lookup"><span data-stu-id="51e5c-149">To configure URL prefixes and ports, you can use the `UseURLs` extension method, the `urls` command-line argument or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="51e5c-150">如需詳細資訊，請參閱[裝載](../../fundamentals/hosting.md)。</span><span class="sxs-lookup"><span data-stu-id="51e5c-150">For more information, see [Hosting](../../fundamentals/hosting.md).</span></span>

  <span data-ttu-id="51e5c-151">網頁接聽程式會使用[Http.Sys 前置詞字串格式](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)。</span><span class="sxs-lookup"><span data-stu-id="51e5c-151">Web Listener uses the [Http.Sys prefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span> <span data-ttu-id="51e5c-152">沒有前置詞字串格式需求 WebListener 特有的。</span><span class="sxs-lookup"><span data-stu-id="51e5c-152">There are no prefix string format requirements that are specific to WebListener.</span></span>

  > [!NOTE]
  > <span data-ttu-id="51e5c-153">請確定您指定在相同的前置詞字串`UseUrls`，您預先註冊的伺服器上。</span><span class="sxs-lookup"><span data-stu-id="51e5c-153">Make sure that you specify the same prefix strings in `UseUrls` that you preregister on the server.</span></span> 

* <span data-ttu-id="51e5c-154">請確定您的應用程式未設定為執行 IIS 或 IIS Express。</span><span class="sxs-lookup"><span data-stu-id="51e5c-154">Make sure your application is not configured to run IIS or IIS Express.</span></span>

  <span data-ttu-id="51e5c-155">在 Visual Studio 中的預設啟動設定檔為 IIS Express。</span><span class="sxs-lookup"><span data-stu-id="51e5c-155">In Visual Studio, the default launch profile is for IIS Express.</span></span>  <span data-ttu-id="51e5c-156">若要執行為主控台應用程式的專案您必須手動變更選取的設定檔，如下列螢幕擷取畫面所示。</span><span class="sxs-lookup"><span data-stu-id="51e5c-156">To run the project as a console application you have to manually change the selected profile, as shown in the following screen shot.</span></span>

  ![選取主控台應用程式設定檔](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a><span data-ttu-id="51e5c-158">如何使用 ASP.NET Core 之外 WebListener</span><span class="sxs-lookup"><span data-stu-id="51e5c-158">How to use WebListener outside of ASP.NET Core</span></span>

* <span data-ttu-id="51e5c-159">安裝[Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="51e5c-159">Install the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

* <span data-ttu-id="51e5c-160">[__'Asverify'__ 繫結至 WebListener，以及設定 SSL 憑證的 URL 前置詞](#preregister-url-prefixes-and-configure-ssl)以用於 ASP.NET Core 一樣。</span><span class="sxs-lookup"><span data-stu-id="51e5c-160">[Preregister URL prefixes to bind to WebListener, and set up SSL certificates](#preregister-url-prefixes-and-configure-ssl) as you would for use in ASP.NET Core.</span></span>

<span data-ttu-id="51e5c-161">另外還有[Http.Sys 登錄設定](https://support.microsoft.com/kb/820129)。</span><span class="sxs-lookup"><span data-stu-id="51e5c-161">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>


<span data-ttu-id="51e5c-162">以下是示範 WebListener 使用 ASP.NET Core 之外的程式碼範例：</span><span class="sxs-lookup"><span data-stu-id="51e5c-162">Here's a code sample that demonstrates WebListener use outside of ASP.NET Core:</span></span>

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

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="51e5c-163">__'Asverify'__ URL 前置詞及設定 SSL</span><span class="sxs-lookup"><span data-stu-id="51e5c-163">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="51e5c-164">IIS 和 WebListener 依賴接聽要求的基礎 Http.Sys 核心模式驅動程式，並執行初始處理。</span><span class="sxs-lookup"><span data-stu-id="51e5c-164">Both IIS and WebListener rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="51e5c-165">在 IIS 中，管理 UI 可讓您很輕鬆地設定所有項目。</span><span class="sxs-lookup"><span data-stu-id="51e5c-165">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="51e5c-166">不過，如果您使用 WebListener 您需要自行設定 Http.Sys。</span><span class="sxs-lookup"><span data-stu-id="51e5c-166">However, if you're using WebListener you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="51e5c-167">也就是執行 netsh.exe 內建的工具。</span><span class="sxs-lookup"><span data-stu-id="51e5c-167">The built-in tool for doing that is netsh.exe.</span></span> 

<span data-ttu-id="51e5c-168">您必須使用 netsh.exe 的最常見的工作會保留 URL 首碼，並指派 SSL 憑證。</span><span class="sxs-lookup"><span data-stu-id="51e5c-168">The most common tasks you need to use netsh.exe for are reserving URL prefixes and assigning SSL certificates.</span></span>

<span data-ttu-id="51e5c-169">NetSh.exe 不是使用適用於初學者簡單工具。</span><span class="sxs-lookup"><span data-stu-id="51e5c-169">NetSh.exe is not an easy tool to use for beginners.</span></span> <span data-ttu-id="51e5c-170">下列範例會示範保留連接埠 80 和 443 的 URL 前置詞時所需的最低限度：</span><span class="sxs-lookup"><span data-stu-id="51e5c-170">The following example shows the bare minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="51e5c-171">下列範例會示範如何將 SSL 憑證指派：</span><span class="sxs-lookup"><span data-stu-id="51e5c-171">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}".
```

<span data-ttu-id="51e5c-172">以下是正式的參考文件：</span><span class="sxs-lookup"><span data-stu-id="51e5c-172">Here is the official reference documentation:</span></span>

* [<span data-ttu-id="51e5c-173">Netsh 命令都會使用超文字傳輸通訊協定 (HTTP)</span><span class="sxs-lookup"><span data-stu-id="51e5c-173">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
* [<span data-ttu-id="51e5c-174">UrlPrefix 字串</span><span class="sxs-lookup"><span data-stu-id="51e5c-174">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

<span data-ttu-id="51e5c-175">下列資源提供數種案例的詳細的指示。</span><span class="sxs-lookup"><span data-stu-id="51e5c-175">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="51e5c-176">參考的文件`HttpListener`同樣適用於`WebListener`，因為兩者都以 Http.Sys。</span><span class="sxs-lookup"><span data-stu-id="51e5c-176">Articles that refer to `HttpListener` apply equally to `WebListener`, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="51e5c-177">如何：使用 SSL 憑證設定連接埠</span><span class="sxs-lookup"><span data-stu-id="51e5c-177">How to: Configure a Port with an SSL Certificate</span></span>](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="51e5c-178">[HTTPS 通訊-HttpListener 基礎代管與用戶端憑證](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html)這是協力廠商架構部落格和相當老舊，但仍有有用的資訊。</span><span class="sxs-lookup"><span data-stu-id="51e5c-178">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="51e5c-179">[如何： 逐步解說使用 HttpListener 或 Http 伺服器 unmanaged 程式碼 （c + +） 做為 SSL 簡單伺服器](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/)這也是較舊的部落格包含有用資訊。</span><span class="sxs-lookup"><span data-stu-id="51e5c-179">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>
* [<span data-ttu-id="51e5c-180">如何設定 SSL 使用.NET 核心 WebListener？</span><span class="sxs-lookup"><span data-stu-id="51e5c-180">How Do I Set Up A .NET Core WebListener With SSL?</span></span>](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/)

<span data-ttu-id="51e5c-181">以下是一些可以比使用 netsh.exe 命令列的協力廠商工具。</span><span class="sxs-lookup"><span data-stu-id="51e5c-181">Here are some third-party tools that can be easier to use than the netsh.exe command line.</span></span> <span data-ttu-id="51e5c-182">這些不是所提供，或經由 Microsoft 背書。</span><span class="sxs-lookup"><span data-stu-id="51e5c-182">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="51e5c-183">這些工具執行系統管理員身分根據預設，由於 netsh.exe 本身需要系統管理員權限。</span><span class="sxs-lookup"><span data-stu-id="51e5c-183">The tools run as administrator by default, since netsh.exe itself requires administrator privileges.</span></span>

* <span data-ttu-id="51e5c-184">[http.sys 管理員](http://httpsysmanager.codeplex.com/)清單提供 UI，並設定 SSL 憑證和選項保留的前置詞及憑證信任清單。</span><span class="sxs-lookup"><span data-stu-id="51e5c-184">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="51e5c-185">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx)可讓您列出或設定 SSL 憑證和 URL 前置詞。</span><span class="sxs-lookup"><span data-stu-id="51e5c-185">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="51e5c-186">UI 會更精簡比 http.sys 管理員，並公開更多組態選項，但否則它會提供類似的功能。</span><span class="sxs-lookup"><span data-stu-id="51e5c-186">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="51e5c-187">它無法建立新的憑證信任清單 (CTL)，但可以將現有的指派。</span><span class="sxs-lookup"><span data-stu-id="51e5c-187">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

<span data-ttu-id="51e5c-188">產生自我簽署的 SSL 憑證，Microsoft 提供的命令列工具： [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968)和 PowerShell 指令程式[New-selfsignedcertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate)。</span><span class="sxs-lookup"><span data-stu-id="51e5c-188">For generating self-signed SSL certificates, Microsoft provides command-line tools: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) and the PowerShell cmdlet [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span></span> <span data-ttu-id="51e5c-189">另外還有第三方 UI 工具，可讓您更輕鬆地產生自我簽署的 SSL 憑證：</span><span class="sxs-lookup"><span data-stu-id="51e5c-189">There are also third-party UI tools that make it easier for you to generate self-signed SSL certificates:</span></span>

* [<span data-ttu-id="51e5c-190">SelfCert</span><span class="sxs-lookup"><span data-stu-id="51e5c-190">SelfCert</span></span>](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [<span data-ttu-id="51e5c-191">Makecert UI</span><span class="sxs-lookup"><span data-stu-id="51e5c-191">Makecert UI</span></span>](http://makecertui.codeplex.com/)

## <a name="next-steps"></a><span data-ttu-id="51e5c-192">後續步驟</span><span class="sxs-lookup"><span data-stu-id="51e5c-192">Next steps</span></span>

<span data-ttu-id="51e5c-193">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="51e5c-193">For more information, see the following resources:</span></span>

* [<span data-ttu-id="51e5c-194">這篇文章的範例應用程式</span><span class="sxs-lookup"><span data-stu-id="51e5c-194">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [<span data-ttu-id="51e5c-195">WebListener 原始程式碼</span><span class="sxs-lookup"><span data-stu-id="51e5c-195">WebListener source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="51e5c-196">裝載</span><span class="sxs-lookup"><span data-stu-id="51e5c-196">Hosting</span></span>](../hosting.md)
