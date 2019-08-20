---
title: 透過 HTTPS 使用 Docker 裝載 ASP.NET Core 映射
author: rick-anderson
description: 瞭解如何透過 HTTPS 使用 Docker 裝載 ASP.NET Core 映射
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/05/2019
uid: security/docker-https
ms.openlocfilehash: f17a3abe1b00b39b7b6787be5b20ce65771190b8
ms.sourcegitcommit: 257cc3fe8c1d61341aa3b07e5bc0fa3d1c1c1d1c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/19/2019
ms.locfileid: "69619693"
---
# <a name="hosting-aspnet-core-images-with-docker-over-https"></a><span data-ttu-id="da357-103">透過 HTTPS 使用 Docker 裝載 ASP.NET Core 映射</span><span class="sxs-lookup"><span data-stu-id="da357-103">Hosting ASP.NET Core images with Docker over HTTPS</span></span>

<span data-ttu-id="da357-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="da357-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="da357-105">ASP.NET Core 預設會使用[HTTPS](/aspnet/core/security/enforcing-ssl)。</span><span class="sxs-lookup"><span data-stu-id="da357-105">ASP.NET Core uses [HTTPS by default](/aspnet/core/security/enforcing-ssl).</span></span> <span data-ttu-id="da357-106">[HTTPS](https://en.wikipedia.org/wiki/HTTPS)依賴[憑證](https://en.wikipedia.org/wiki/Public_key_certificate)來進行信任、身分識別和加密。</span><span class="sxs-lookup"><span data-stu-id="da357-106">[HTTPS](https://en.wikipedia.org/wiki/HTTPS) relies on [certificates](https://en.wikipedia.org/wiki/Public_key_certificate) for trust, identity, and encryption.</span></span>

<span data-ttu-id="da357-107">本檔說明如何使用 HTTPS 來執行預先建立的容器映射。</span><span class="sxs-lookup"><span data-stu-id="da357-107">This document explains how to run pre-built container images with HTTPS.</span></span>

<span data-ttu-id="da357-108">如需開發案例, 請參閱[使用 Docker OVER HTTPS 開發 ASP.NET Core 應用程式](https://github.com/dotnet/dotnet-docker/blob/master/samples/aspnetapp/aspnetcore-docker-https-development.md)。</span><span class="sxs-lookup"><span data-stu-id="da357-108">See [Developing ASP.NET Core Applications with Docker over HTTPS](https://github.com/dotnet/dotnet-docker/blob/master/samples/aspnetapp/aspnetcore-docker-https-development.md) for development scenarios.</span></span>

<span data-ttu-id="da357-109">此範例需要 docker [17.06](https://docs.docker.com/release-notes/docker-ce)或更新版本的[docker 用戶端](https://www.docker.com/products/docker)。</span><span class="sxs-lookup"><span data-stu-id="da357-109">This sample requires [Docker 17.06](https://docs.docker.com/release-notes/docker-ce) or later of the [Docker client](https://www.docker.com/products/docker).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="da357-110">必要條件</span><span class="sxs-lookup"><span data-stu-id="da357-110">Prerequisites</span></span>

<span data-ttu-id="da357-111">本檔中的部分指示需要[.Net Core 2.2 SDK](https://www.microsoft.com/net/download)或更新版本。</span><span class="sxs-lookup"><span data-stu-id="da357-111">The [.NET Core 2.2 SDK](https://www.microsoft.com/net/download) or later is required for some of the instructions in this document.</span></span>

## <a name="certificates"></a><span data-ttu-id="da357-112">憑證</span><span class="sxs-lookup"><span data-stu-id="da357-112">Certificates</span></span>

<span data-ttu-id="da357-113">需要[憑證授權單位](https://en.wikipedia.org/wiki/Certificate_authority)單位的憑證, 才能針對網域進行[生產環境裝載](https://blogs.msdn.microsoft.com/webdev/2017/11/29/configuring-https-in-asp-net-core-across-different-platforms/)。</span><span class="sxs-lookup"><span data-stu-id="da357-113">A certificate from a [certificate authority](https://en.wikipedia.org/wiki/Certificate_authority) is required for [production hosting](https://blogs.msdn.microsoft.com/webdev/2017/11/29/configuring-https-in-asp-net-core-across-different-platforms/) for a domain.</span></span>  <span data-ttu-id="da357-114">「[我們的加密](https://letsencrypt.org/)」是提供免費憑證的憑證授權單位單位。</span><span class="sxs-lookup"><span data-stu-id="da357-114">[Let's Encrypt](https://letsencrypt.org/) is a certificate authority that offers free certificates.</span></span>

<span data-ttu-id="da357-115">本檔使用[自我簽署的開發憑證](https://en.wikipedia.org/wiki/Self-signed_certificate)來裝載預先建立的映射`localhost`。</span><span class="sxs-lookup"><span data-stu-id="da357-115">This document uses [self-signed development certificates](https://en.wikipedia.org/wiki/Self-signed_certificate) for hosting pre-built images over `localhost`.</span></span> <span data-ttu-id="da357-116">這些指示與使用生產憑證類似。</span><span class="sxs-lookup"><span data-stu-id="da357-116">The instructions are similar to using production certificates.</span></span>

<span data-ttu-id="da357-117">針對生產憑證:</span><span class="sxs-lookup"><span data-stu-id="da357-117">For production certs:</span></span>

* <span data-ttu-id="da357-118">不`dotnet dev-certs`需要此工具。</span><span class="sxs-lookup"><span data-stu-id="da357-118">The `dotnet dev-certs` tool is not required.</span></span>
* <span data-ttu-id="da357-119">憑證不需要儲存在指示所使用的位置。</span><span class="sxs-lookup"><span data-stu-id="da357-119">Certificates do not need to be stored in the location used in the instructions.</span></span> <span data-ttu-id="da357-120">任何位置都應該可行, 雖然不建議在您的網站目錄中儲存憑證。</span><span class="sxs-lookup"><span data-stu-id="da357-120">Any location should work, although storing certs within your site directory is not recommended.</span></span>

<span data-ttu-id="da357-121">指示磁片區會將憑證掛接到容器中。</span><span class="sxs-lookup"><span data-stu-id="da357-121">The instructions volume mount certificates into containers.</span></span> <span data-ttu-id="da357-122">您可以使用 Dockerfile 中的`COPY`命令, 將憑證新增至容器映射。</span><span class="sxs-lookup"><span data-stu-id="da357-122">You can add certificates into container images with a `COPY` command in a Dockerfile.</span></span> <span data-ttu-id="da357-123">不建議將憑證複製到映射:</span><span class="sxs-lookup"><span data-stu-id="da357-123">Copying certificates into an image is not recommended:</span></span>

* <span data-ttu-id="da357-124">使用相同的映射來測試開發人員憑證並不容易。</span><span class="sxs-lookup"><span data-stu-id="da357-124">It makes difficult to use the same image for testing with developer certificates.</span></span>
* <span data-ttu-id="da357-125">使用相同的映射來裝載實際執行憑證很容易。</span><span class="sxs-lookup"><span data-stu-id="da357-125">It makes difficult to use the same image for Hosting with production certificates.</span></span>
* <span data-ttu-id="da357-126">憑證洩漏有嚴重的風險。</span><span class="sxs-lookup"><span data-stu-id="da357-126">There is significant risk of certificate disclosure.</span></span>

## <a name="running-pre-built-container-images-with-https"></a><span data-ttu-id="da357-127">使用 HTTPS 執行預先建立的容器映射</span><span class="sxs-lookup"><span data-stu-id="da357-127">Running pre-built container images with HTTPS</span></span>

<span data-ttu-id="da357-128">針對您的作業系統設定, 請使用下列指示。</span><span class="sxs-lookup"><span data-stu-id="da357-128">Use the following instructions for your operating system configuration.</span></span>

### <a name="windows-using-linux-containers"></a><span data-ttu-id="da357-129">使用 Linux 容器的 Windows</span><span class="sxs-lookup"><span data-stu-id="da357-129">Windows using Linux containers</span></span>

<span data-ttu-id="da357-130">產生憑證並設定本機電腦:</span><span class="sxs-lookup"><span data-stu-id="da357-130">Generate certificate and configure local machine:</span></span>

```console
dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p { password here }
dotnet dev-certs https --trust
```

<span data-ttu-id="da357-131">在上述命令中, 將`{ password here }`取代為密碼。</span><span class="sxs-lookup"><span data-stu-id="da357-131">In the preceding commands, replace `{ password here }` with a password.</span></span>

<span data-ttu-id="da357-132">使用針對 HTTPS 設定的 ASP.NET Core 來執行容器映射:</span><span class="sxs-lookup"><span data-stu-id="da357-132">Run the container image with ASP.NET Core configured for HTTPS:</span></span>

```console
docker pull mcr.microsoft.com/dotnet/core/samples:aspnetapp
docker run --rm -it -p 8000:80 -p 8001:443 -e ASPNETCORE_URLS="https://+;http://+" -e ASPNETCORE_HTTPS_PORT=8001 -e ASPNETCORE_Kestrel__Certificates__Default__Password="password" -e ASPNETCORE_Kestrel__Certificates__Default__Path=/https/aspnetapp.pfx -v %USERPROFILE%\.aspnet\https:/https/ mcr.microsoft.com/dotnet/core/samples:aspnetapp
```

<span data-ttu-id="da357-133">密碼必須符合用於憑證的密碼。</span><span class="sxs-lookup"><span data-stu-id="da357-133">The password must match the password used for the certificate.</span></span>

### <a name="macos-or-linux"></a><span data-ttu-id="da357-134">macOS 或 Linux</span><span class="sxs-lookup"><span data-stu-id="da357-134">macOS or Linux</span></span>

<span data-ttu-id="da357-135">產生憑證並設定本機電腦:</span><span class="sxs-lookup"><span data-stu-id="da357-135">Generate certificate and configure local machine:</span></span>

```console
dotnet dev-certs https -ep ${HOME}/.aspnet/https/aspnetapp.pfx -p { password here }
dotnet dev-certs https --trust
```

<span data-ttu-id="da357-136">`dotnet dev-certs https --trust`只有在 macOS 和 Windows 上才支援。</span><span class="sxs-lookup"><span data-stu-id="da357-136">`dotnet dev-certs https --trust` is only supported on macOS and Windows.</span></span> <span data-ttu-id="da357-137">您必須以散發版本支援的方式信任 Linux 上的憑證。</span><span class="sxs-lookup"><span data-stu-id="da357-137">You need to trust certs on Linux in the way that is supported by your distro.</span></span> <span data-ttu-id="da357-138">您很可能需要信任您瀏覽器中的憑證。</span><span class="sxs-lookup"><span data-stu-id="da357-138">It is likely that you need to trust the certificate in your browser.</span></span>

<span data-ttu-id="da357-139">在上述命令中, 將`{ password here }`取代為密碼。</span><span class="sxs-lookup"><span data-stu-id="da357-139">In the preceding commands, replace `{ password here }` with a password.</span></span>

<span data-ttu-id="da357-140">使用針對 HTTPS 設定的 ASP.NET Core 來執行容器映射:</span><span class="sxs-lookup"><span data-stu-id="da357-140">Run the container image with ASP.NET Core configured for HTTPS:</span></span>

```console
docker pull mcr.microsoft.com/dotnet/core/samples:aspnetapp
docker run --rm -it -p 8000:80 -p 8001:443 -e ASPNETCORE_URLS="https://+;http://+" -e ASPNETCORE_HTTPS_PORT=8001 -e ASPNETCORE_Kestrel__Certificates__Default__Password="password" -e ASPNETCORE_Kestrel__Certificates__Default__Path=/https/aspnetapp.pfx -v ${HOME}/.aspnet/https:/https/ mcr.microsoft.com/dotnet/core/samples:aspnetapp
```

<span data-ttu-id="da357-141">密碼必須符合用於憑證的密碼。</span><span class="sxs-lookup"><span data-stu-id="da357-141">The password must match the password used for the certificate.</span></span>

### <a name="windows-using-windows-containers"></a><span data-ttu-id="da357-142">使用 Windows 容器的 windows</span><span class="sxs-lookup"><span data-stu-id="da357-142">Windows using Windows containers</span></span>

<span data-ttu-id="da357-143">產生憑證並設定本機電腦:</span><span class="sxs-lookup"><span data-stu-id="da357-143">Generate certificate and configure local machine:</span></span>

```console
dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p { password here }
dotnet dev-certs https --trust
```

<span data-ttu-id="da357-144">在上述命令中, 將`{ password here }`取代為密碼。</span><span class="sxs-lookup"><span data-stu-id="da357-144">In the preceding commands, replace `{ password here }` with a password.</span></span>

<span data-ttu-id="da357-145">使用針對 HTTPS 設定的 ASP.NET Core 來執行容器映射:</span><span class="sxs-lookup"><span data-stu-id="da357-145">Run the container image with ASP.NET Core configured for HTTPS:</span></span>

```console
docker pull mcr.microsoft.com/dotnet/core/samples:aspnetapp
docker run --rm -it -p 8000:80 -p 8001:443 -e ASPNETCORE_URLS="https://+;http://+" -e ASPNETCORE_HTTPS_PORT=8001 -e ASPNETCORE_Kestrel__Certificates__Default__Password="password" -e ASPNETCORE_Kestrel__Certificates__Default__Path=\https\aspnetapp.pfx -v %USERPROFILE%\.aspnet\https:C:\https\ mcr.microsoft.com/dotnet/core/samples:aspnetapp
```

<span data-ttu-id="da357-146">密碼必須符合用於憑證的密碼。</span><span class="sxs-lookup"><span data-stu-id="da357-146">The password must match the password used for the certificate.</span></span>