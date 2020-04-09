---
title: 使用 HTTPS 撰寫的 docker 在容器中託管ASP.NET核心映像
author: ravipal
description: 瞭解如何透過 HTTPS 使用 Docker 合成託管ASP.NET核心映像
monikerRange: '>= aspnetcore-2.1'
ms.author: ravipal
ms.custom: mvc
ms.date: 03/28/2020
no-loc:
- Let's Encrypt
uid: security/docker-compose-https
ms.openlocfilehash: 616ccf906e98534ffda08c0c2b6d0a171f063cc1
ms.sourcegitcommit: d03905aadf5ceac39fff17706481af7f6c130411
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/29/2020
ms.locfileid: "80381821"
---
# <a name="hosting-aspnet-core-images-with-docker-compose-over-https"></a><span data-ttu-id="5ea0d-103">託管ASP.NET核心映像與 Docker 組成透過 HTTPS</span><span class="sxs-lookup"><span data-stu-id="5ea0d-103">Hosting ASP.NET Core images with Docker Compose over HTTPS</span></span>


<span data-ttu-id="5ea0d-104">ASP.NET核心預設情況下使用[HTTPS。](/aspnet/core/security/enforcing-ssl)</span><span class="sxs-lookup"><span data-stu-id="5ea0d-104">ASP.NET Core uses [HTTPS by default](/aspnet/core/security/enforcing-ssl).</span></span> <span data-ttu-id="5ea0d-105">[HTTPS](https://en.wikipedia.org/wiki/HTTPS)依賴於[證書](https://en.wikipedia.org/wiki/Public_key_certificate)進行信任、標識和加密。</span><span class="sxs-lookup"><span data-stu-id="5ea0d-105">[HTTPS](https://en.wikipedia.org/wiki/HTTPS) relies on [certificates](https://en.wikipedia.org/wiki/Public_key_certificate) for trust, identity, and encryption.</span></span>

<span data-ttu-id="5ea0d-106">本文件介紹如何使用 HTTPS 執行預建構的容器映像。</span><span class="sxs-lookup"><span data-stu-id="5ea0d-106">This document explains how to run pre-built container images with HTTPS.</span></span>

<span data-ttu-id="5ea0d-107">有關開發方案[,請參閱使用 Docker 在 HTTPS 上開發 ASP.NET 核心應用程式](https://github.com/dotnet/dotnet-docker/blob/master/samples/run-aspnetcore-https-development.md)。</span><span class="sxs-lookup"><span data-stu-id="5ea0d-107">See [Developing ASP.NET Core Applications with Docker over HTTPS](https://github.com/dotnet/dotnet-docker/blob/master/samples/run-aspnetcore-https-development.md) for development scenarios.</span></span>

<span data-ttu-id="5ea0d-108">此範例需要[Docker 17.06](https://docs.docker.com/release-notes/docker-ce)或更高版本的[Docker 用戶端](https://www.docker.com/products/docker)。</span><span class="sxs-lookup"><span data-stu-id="5ea0d-108">This sample requires [Docker 17.06](https://docs.docker.com/release-notes/docker-ce) or later of the [Docker client](https://www.docker.com/products/docker).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5ea0d-109">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="5ea0d-109">Prerequisites</span></span>

<span data-ttu-id="5ea0d-110">本文檔中的某些說明需要[.NET Core 2.2 SDK](https://dotnet.microsoft.com/download)或更高版本。</span><span class="sxs-lookup"><span data-stu-id="5ea0d-110">The [.NET Core 2.2 SDK](https://dotnet.microsoft.com/download) or later is required for some of the instructions in this document.</span></span>

## <a name="certificates"></a><span data-ttu-id="5ea0d-111">憑證</span><span class="sxs-lookup"><span data-stu-id="5ea0d-111">Certificates</span></span>

<span data-ttu-id="5ea0d-112">域[的生產託管](https://blogs.msdn.microsoft.com/webdev/2017/11/29/configuring-https-in-asp-net-core-across-different-platforms/)需要[證書頒發機構的](https://wikipedia.org/wiki/Certificate_authority)證書。</span><span class="sxs-lookup"><span data-stu-id="5ea0d-112">A certificate from a [certificate authority](https://wikipedia.org/wiki/Certificate_authority) is required for [production hosting](https://blogs.msdn.microsoft.com/webdev/2017/11/29/configuring-https-in-asp-net-core-across-different-platforms/) for a domain.</span></span> <span data-ttu-id="5ea0d-113">[Let's Encrypt](https://letsencrypt.org/)是提供免費證書的證書頒發機構。</span><span class="sxs-lookup"><span data-stu-id="5ea0d-113">[Let's Encrypt](https://letsencrypt.org/) is a certificate authority that offers free certificates.</span></span>

<span data-ttu-id="5ea0d-114">本文檔使用[自簽名開發證書](https://wikipedia.org/wiki/Self-signed_certificate)在上`localhost`託管預建構的圖像。</span><span class="sxs-lookup"><span data-stu-id="5ea0d-114">This document uses [self-signed development certificates](https://wikipedia.org/wiki/Self-signed_certificate) for hosting pre-built images over `localhost`.</span></span> <span data-ttu-id="5ea0d-115">這些說明類似於使用生產證書。</span><span class="sxs-lookup"><span data-stu-id="5ea0d-115">The instructions are similar to using production certificates.</span></span>

<span data-ttu-id="5ea0d-116">對生產憑證:</span><span class="sxs-lookup"><span data-stu-id="5ea0d-116">For production certificates:</span></span>

* <span data-ttu-id="5ea0d-117">該工具`dotnet dev-certs`不是必需的。</span><span class="sxs-lookup"><span data-stu-id="5ea0d-117">The `dotnet dev-certs` tool is not required.</span></span>
* <span data-ttu-id="5ea0d-118">憑證不需要存儲在說明中使用的位置。</span><span class="sxs-lookup"><span data-stu-id="5ea0d-118">Certificates don't need to be stored in the location used in the instructions.</span></span> <span data-ttu-id="5ea0d-119">將證書存儲在網站目錄之外的任何位置。</span><span class="sxs-lookup"><span data-stu-id="5ea0d-119">Store the certificates in any location outside the site directory.</span></span>

<span data-ttu-id="5ea0d-120">以下部分卷中包含的說明使用*docker-compose.yml*中的`volumes`屬性將證書裝入容器中。</span><span class="sxs-lookup"><span data-stu-id="5ea0d-120">The instructions contained in the following section volume mount certificates into containers using the `volumes` property in *docker-compose.yml.*</span></span> <span data-ttu-id="5ea0d-121">您可以使用`COPY`*Dockerfile*中的命令將證書添加到容器映射中,但不建議這樣做。</span><span class="sxs-lookup"><span data-stu-id="5ea0d-121">You could add certificates into container images with a `COPY` command in a *Dockerfile*, but it's not recommended.</span></span> <span data-ttu-id="5ea0d-122">出於以下原因,不建議將憑證複製到映射:</span><span class="sxs-lookup"><span data-stu-id="5ea0d-122">Copying certificates into an image isn't recommended for the following reasons:</span></span>

* <span data-ttu-id="5ea0d-123">這使得使用同一映射使用開發人員證書進行測試變得困難。</span><span class="sxs-lookup"><span data-stu-id="5ea0d-123">It makes it difficult to use the same image for testing with developer certificates.</span></span>
* <span data-ttu-id="5ea0d-124">它使使用同一映像進行生產證書託管變得困難。</span><span class="sxs-lookup"><span data-stu-id="5ea0d-124">It makes it difficult to use the same image for Hosting with production certificates.</span></span>
* <span data-ttu-id="5ea0d-125">證書披露存在重大風險。</span><span class="sxs-lookup"><span data-stu-id="5ea0d-125">There is significant risk of certificate disclosure.</span></span>

## <a name="starting-a-container-with-https-support-using-docker-compose"></a><span data-ttu-id="5ea0d-126">使用 docker 組合使用 Htcritor 容器</span><span class="sxs-lookup"><span data-stu-id="5ea0d-126">Starting a container with https support using docker compose</span></span>

<span data-ttu-id="5ea0d-127">對作業系統配置使用以下說明。</span><span class="sxs-lookup"><span data-stu-id="5ea0d-127">Use the following instructions for your operating system configuration.</span></span>

### <a name="windows-using-linux-containers"></a><span data-ttu-id="5ea0d-128">使用 Linux 容器的視窗</span><span class="sxs-lookup"><span data-stu-id="5ea0d-128">Windows using Linux containers</span></span>

<span data-ttu-id="5ea0d-129">產生憑證並設定本地電腦:</span><span class="sxs-lookup"><span data-stu-id="5ea0d-129">Generate certificate and configure local machine:</span></span>

```dotnetcli
dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p { password here }
dotnet dev-certs https --trust
```

<span data-ttu-id="5ea0d-130">在前面的命令中,替換為`{ password here }`密碼。</span><span class="sxs-lookup"><span data-stu-id="5ea0d-130">In the preceding commands, replace `{ password here }` with a password.</span></span>

<span data-ttu-id="5ea0d-131">建立包含以下內容的_docker-compose.debug.yml_檔:</span><span class="sxs-lookup"><span data-stu-id="5ea0d-131">Create a _docker-compose.debug.yml_ file with the following content:</span></span>

```json
version: '3.4'

services:
  webapp:
    image: mcr.microsoft.com/dotnet/core/samples:aspnetapp
    ports:
      - 80
      - 443
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
      - ASPNETCORE_URLS=https://+:443;http://+:80
      - ASPNETCORE_Kestrel__Certificates__Default__Password=password
      - ASPNETCORE_Kestrel__Certificates__Default__Path=/https/aspnetapp.pfx
    volumes:
      - ~/.aspnet/https:/https:ro
```
<span data-ttu-id="5ea0d-132">docker 撰寫檔中指定的密碼必須與用於證書的密碼匹配。</span><span class="sxs-lookup"><span data-stu-id="5ea0d-132">The password specified in the docker compose file must match the password used for the certificate.</span></span>

<span data-ttu-id="5ea0d-133">使用為 HTTPS 設定 ASP.NET 核心啟動容器:</span><span class="sxs-lookup"><span data-stu-id="5ea0d-133">Start the container with ASP.NET Core configured for HTTPS:</span></span>

```console
docker-compose -f "docker-compose.debug.yml" up -d
```

### <a name="macos-or-linux"></a><span data-ttu-id="5ea0d-134">macOS 或 Linux</span><span class="sxs-lookup"><span data-stu-id="5ea0d-134">macOS or Linux</span></span>

<span data-ttu-id="5ea0d-135">產生憑證並設定本地電腦:</span><span class="sxs-lookup"><span data-stu-id="5ea0d-135">Generate certificate and configure local machine:</span></span>

```dotnetcli
dotnet dev-certs https -ep ${HOME}/.aspnet/https/aspnetapp.pfx -p { password here }
dotnet dev-certs https --trust
```

<span data-ttu-id="5ea0d-136">`dotnet dev-certs https --trust`僅在 macOS 和 Windows 上受支援。</span><span class="sxs-lookup"><span data-stu-id="5ea0d-136">`dotnet dev-certs https --trust` is only supported on macOS and Windows.</span></span> <span data-ttu-id="5ea0d-137">您需要以發行版支援的方式信任 Linux 上的證書。</span><span class="sxs-lookup"><span data-stu-id="5ea0d-137">You need to trust certificates on Linux in the way that is supported by your distro.</span></span> <span data-ttu-id="5ea0d-138">您可能需要在瀏覽器中信任證書。</span><span class="sxs-lookup"><span data-stu-id="5ea0d-138">It is likely that you need to trust the certificate in your browser.</span></span>

<span data-ttu-id="5ea0d-139">在前面的命令中,替換為`{ password here }`密碼。</span><span class="sxs-lookup"><span data-stu-id="5ea0d-139">In the preceding commands, replace `{ password here }` with a password.</span></span>

<span data-ttu-id="5ea0d-140">建立包含以下內容的_docker-compose.debug.yml_檔:</span><span class="sxs-lookup"><span data-stu-id="5ea0d-140">Create a _docker-compose.debug.yml_ file with the following content:</span></span>

```json
version: '3.4'

services:
  webapp:
    image: mcr.microsoft.com/dotnet/core/samples:aspnetapp
    ports:
      - 80
      - 443
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
      - ASPNETCORE_URLS=https://+:443;http://+:80
      - ASPNETCORE_Kestrel__Certificates__Default__Password=password
      - ASPNETCORE_Kestrel__Certificates__Default__Path=/https/aspnetapp.pfx
    volumes:
      - ~/.aspnet/https:/https:ro
```
<span data-ttu-id="5ea0d-141">docker 撰寫檔中指定的密碼必須與用於證書的密碼匹配。</span><span class="sxs-lookup"><span data-stu-id="5ea0d-141">The password specified in the docker compose file must match the password used for the certificate.</span></span>

<span data-ttu-id="5ea0d-142">使用為 HTTPS 設定 ASP.NET 核心啟動容器:</span><span class="sxs-lookup"><span data-stu-id="5ea0d-142">Start the container with ASP.NET Core configured for HTTPS:</span></span>

```console
docker-compose -f "docker-compose.debug.yml" up -d
```

### <a name="windows-using-windows-containers"></a><span data-ttu-id="5ea0d-143">使用 Windows 容器的視窗</span><span class="sxs-lookup"><span data-stu-id="5ea0d-143">Windows using Windows containers</span></span>

<span data-ttu-id="5ea0d-144">產生憑證並設定本地電腦:</span><span class="sxs-lookup"><span data-stu-id="5ea0d-144">Generate certificate and configure local machine:</span></span>

```dotnetcli
dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p { password here }
dotnet dev-certs https --trust
```

<span data-ttu-id="5ea0d-145">在前面的命令中,替換為`{ password here }`密碼。</span><span class="sxs-lookup"><span data-stu-id="5ea0d-145">In the preceding commands, replace `{ password here }` with a password.</span></span>

<span data-ttu-id="5ea0d-146">建立包含以下內容的_docker-compose.debug.yml_檔:</span><span class="sxs-lookup"><span data-stu-id="5ea0d-146">Create a _docker-compose.debug.yml_ file with the following content:</span></span>

```json
version: '3.4'

services:
  webapp:
    image: mcr.microsoft.com/dotnet/core/samples:aspnetapp
    ports:
      - 80
      - 443
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
      - ASPNETCORE_URLS=https://+:443;http://+:80
      - ASPNETCORE_Kestrel__Certificates__Default__Password=password
      - ASPNETCORE_Kestrel__Certificates__Default__Path=C:\https\aspnetapp.pfx
    volumes:
      - ${USERPROFILE}\.aspnet\https:C:\https:ro
```
<span data-ttu-id="5ea0d-147">docker 撰寫檔中指定的密碼必須與用於證書的密碼匹配。</span><span class="sxs-lookup"><span data-stu-id="5ea0d-147">The password specified in the docker compose file must match the password used for the certificate.</span></span>

<span data-ttu-id="5ea0d-148">使用為 HTTPS 設定 ASP.NET 核心啟動容器:</span><span class="sxs-lookup"><span data-stu-id="5ea0d-148">Start the container with ASP.NET Core configured for HTTPS:</span></span>

```console
docker-compose -f "docker-compose.debug.yml" up -d
```
