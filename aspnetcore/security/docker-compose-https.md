---
title: 使用 docker 撰寫搭配 HTTPS，在容器中裝載 ASP.NET Core 映射
author: ravipal
description: 瞭解如何透過 HTTPS Docker Compose 裝載 ASP.NET Core 映射
monikerRange: '>= aspnetcore-2.1'
ms.author: ravipal
ms.custom: mvc
ms.date: 03/28/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/docker-compose-https
ms.openlocfilehash: a44e82be9c631aae788a671b514bab3b70f54522
ms.sourcegitcommit: 6fb27ea41a92f6d0e91dfd0eba905d2ac1a707f7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/15/2020
ms.locfileid: "86407797"
---
# <a name="hosting-aspnet-core-images-with-docker-compose-over-https"></a><span data-ttu-id="d6ad8-103">透過 HTTPS Docker Compose 裝載 ASP.NET Core 映射</span><span class="sxs-lookup"><span data-stu-id="d6ad8-103">Hosting ASP.NET Core images with Docker Compose over HTTPS</span></span>


<span data-ttu-id="d6ad8-104">ASP.NET Core 預設會使用[HTTPS](/aspnet/core/security/enforcing-ssl)。</span><span class="sxs-lookup"><span data-stu-id="d6ad8-104">ASP.NET Core uses [HTTPS by default](/aspnet/core/security/enforcing-ssl).</span></span> <span data-ttu-id="d6ad8-105">[HTTPS](https://en.wikipedia.org/wiki/HTTPS)依賴[憑證](https://en.wikipedia.org/wiki/Public_key_certificate)來進行信任、身分識別和加密。</span><span class="sxs-lookup"><span data-stu-id="d6ad8-105">[HTTPS](https://en.wikipedia.org/wiki/HTTPS) relies on [certificates](https://en.wikipedia.org/wiki/Public_key_certificate) for trust, identity, and encryption.</span></span>

<span data-ttu-id="d6ad8-106">本檔說明如何使用 HTTPS 來執行預先建立的容器映射。</span><span class="sxs-lookup"><span data-stu-id="d6ad8-106">This document explains how to run pre-built container images with HTTPS.</span></span>

<span data-ttu-id="d6ad8-107">如需開發案例，請參閱[使用 Docker OVER HTTPS 開發 ASP.NET Core 應用程式](https://github.com/dotnet/dotnet-docker/blob/master/samples/run-aspnetcore-https-development.md)。</span><span class="sxs-lookup"><span data-stu-id="d6ad8-107">See [Developing ASP.NET Core Applications with Docker over HTTPS](https://github.com/dotnet/dotnet-docker/blob/master/samples/run-aspnetcore-https-development.md) for development scenarios.</span></span>

<span data-ttu-id="d6ad8-108">此範例需要 docker [17.06](https://docs.docker.com/release-notes/docker-ce)或更新版本的[docker 用戶端](https://www.docker.com/products/docker)。</span><span class="sxs-lookup"><span data-stu-id="d6ad8-108">This sample requires [Docker 17.06](https://docs.docker.com/release-notes/docker-ce) or later of the [Docker client](https://www.docker.com/products/docker).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d6ad8-109">必要條件</span><span class="sxs-lookup"><span data-stu-id="d6ad8-109">Prerequisites</span></span>

<span data-ttu-id="d6ad8-110">本檔中的部分指示需要[.Net Core 2.2 SDK](https://dotnet.microsoft.com/download)或更新版本。</span><span class="sxs-lookup"><span data-stu-id="d6ad8-110">The [.NET Core 2.2 SDK](https://dotnet.microsoft.com/download) or later is required for some of the instructions in this document.</span></span>

## <a name="certificates"></a><span data-ttu-id="d6ad8-111">憑證</span><span class="sxs-lookup"><span data-stu-id="d6ad8-111">Certificates</span></span>

<span data-ttu-id="d6ad8-112">需要[憑證授權單位](https://wikipedia.org/wiki/Certificate_authority)單位的憑證，才能針對網域進行[生產環境裝載](https://blogs.msdn.microsoft.com/webdev/2017/11/29/configuring-https-in-asp-net-core-across-different-platforms/)。</span><span class="sxs-lookup"><span data-stu-id="d6ad8-112">A certificate from a [certificate authority](https://wikipedia.org/wiki/Certificate_authority) is required for [production hosting](https://blogs.msdn.microsoft.com/webdev/2017/11/29/configuring-https-in-asp-net-core-across-different-platforms/) for a domain.</span></span> <span data-ttu-id="d6ad8-113">[Let's Encrypt](https://letsencrypt.org/)是提供免費憑證的憑證授權單位單位。</span><span class="sxs-lookup"><span data-stu-id="d6ad8-113">[Let's Encrypt](https://letsencrypt.org/) is a certificate authority that offers free certificates.</span></span>

<span data-ttu-id="d6ad8-114">本檔使用[自我簽署的開發憑證](https://wikipedia.org/wiki/Self-signed_certificate)來裝載預先建立的映射 `localhost` 。</span><span class="sxs-lookup"><span data-stu-id="d6ad8-114">This document uses [self-signed development certificates](https://wikipedia.org/wiki/Self-signed_certificate) for hosting pre-built images over `localhost`.</span></span> <span data-ttu-id="d6ad8-115">這些指示與使用生產憑證類似。</span><span class="sxs-lookup"><span data-stu-id="d6ad8-115">The instructions are similar to using production certificates.</span></span>

<span data-ttu-id="d6ad8-116">針對生產環境憑證：</span><span class="sxs-lookup"><span data-stu-id="d6ad8-116">For production certificates:</span></span>

* <span data-ttu-id="d6ad8-117">`dotnet dev-certs`不需要此工具。</span><span class="sxs-lookup"><span data-stu-id="d6ad8-117">The `dotnet dev-certs` tool is not required.</span></span>
* <span data-ttu-id="d6ad8-118">憑證不需要儲存在指示所使用的位置。</span><span class="sxs-lookup"><span data-stu-id="d6ad8-118">Certificates don't need to be stored in the location used in the instructions.</span></span> <span data-ttu-id="d6ad8-119">將憑證儲存在網站目錄以外的任何位置。</span><span class="sxs-lookup"><span data-stu-id="d6ad8-119">Store the certificates in any location outside the site directory.</span></span>

<span data-ttu-id="d6ad8-120">下列區段中所包含的指示會使用 `volumes` *docker-compose.dev.debug.yml. yml*中的屬性，將憑證掛接到容器中。</span><span class="sxs-lookup"><span data-stu-id="d6ad8-120">The instructions contained in the following section volume mount certificates into containers using the `volumes` property in *docker-compose.yml.*</span></span> <span data-ttu-id="d6ad8-121">您可以使用 Dockerfile 中的命令將憑證新增至容器映射 `COPY` ，但不建議這麼做。 *Dockerfile*</span><span class="sxs-lookup"><span data-stu-id="d6ad8-121">You could add certificates into container images with a `COPY` command in a *Dockerfile*, but it's not recommended.</span></span> <span data-ttu-id="d6ad8-122">基於下列原因，不建議將憑證複製到映射：</span><span class="sxs-lookup"><span data-stu-id="d6ad8-122">Copying certificates into an image isn't recommended for the following reasons:</span></span>

* <span data-ttu-id="d6ad8-123">這會讓您難以使用相同的映射來測試開發人員憑證。</span><span class="sxs-lookup"><span data-stu-id="d6ad8-123">It makes it difficult to use the same image for testing with developer certificates.</span></span>
* <span data-ttu-id="d6ad8-124">這會讓使用相同的映射來裝載生產憑證變得很容易。</span><span class="sxs-lookup"><span data-stu-id="d6ad8-124">It makes it difficult to use the same image for Hosting with production certificates.</span></span>
* <span data-ttu-id="d6ad8-125">憑證洩漏有嚴重的風險。</span><span class="sxs-lookup"><span data-stu-id="d6ad8-125">There is significant risk of certificate disclosure.</span></span>

## <a name="starting-a-container-with-https-support-using-docker-compose"></a><span data-ttu-id="d6ad8-126">使用 docker 撰寫啟動具有 HTTPs 支援的容器</span><span class="sxs-lookup"><span data-stu-id="d6ad8-126">Starting a container with https support using docker compose</span></span>

<span data-ttu-id="d6ad8-127">針對您的作業系統設定，請使用下列指示。</span><span class="sxs-lookup"><span data-stu-id="d6ad8-127">Use the following instructions for your operating system configuration.</span></span>

### <a name="windows-using-linux-containers"></a><span data-ttu-id="d6ad8-128">使用 Linux 容器的 Windows</span><span class="sxs-lookup"><span data-stu-id="d6ad8-128">Windows using Linux containers</span></span>

<span data-ttu-id="d6ad8-129">產生憑證並設定本機電腦：</span><span class="sxs-lookup"><span data-stu-id="d6ad8-129">Generate certificate and configure local machine:</span></span>

```dotnetcli
dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p { password here }
dotnet dev-certs https --trust
```

<span data-ttu-id="d6ad8-130">在上述命令中， `{ password here }` 將取代為密碼。</span><span class="sxs-lookup"><span data-stu-id="d6ad8-130">In the preceding commands, replace `{ password here }` with a password.</span></span>

<span data-ttu-id="d6ad8-131">建立包含下列內容的_docker-compose.dev.debug.yml yml_檔案：</span><span class="sxs-lookup"><span data-stu-id="d6ad8-131">Create a _docker-compose.debug.yml_ file with the following content:</span></span>

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
<span data-ttu-id="d6ad8-132">Docker 撰寫檔案中指定的密碼必須符合用於憑證的密碼。</span><span class="sxs-lookup"><span data-stu-id="d6ad8-132">The password specified in the docker compose file must match the password used for the certificate.</span></span>

<span data-ttu-id="d6ad8-133">使用針對 HTTPS 設定的 ASP.NET Core 來啟動容器：</span><span class="sxs-lookup"><span data-stu-id="d6ad8-133">Start the container with ASP.NET Core configured for HTTPS:</span></span>

```console
docker-compose -f "docker-compose.debug.yml" up -d
```

### <a name="macos-or-linux"></a><span data-ttu-id="d6ad8-134">macOS 或 Linux</span><span class="sxs-lookup"><span data-stu-id="d6ad8-134">macOS or Linux</span></span>

<span data-ttu-id="d6ad8-135">產生憑證並設定本機電腦：</span><span class="sxs-lookup"><span data-stu-id="d6ad8-135">Generate certificate and configure local machine:</span></span>

```dotnetcli
dotnet dev-certs https -ep ${HOME}/.aspnet/https/aspnetapp.pfx -p { password here }
dotnet dev-certs https --trust
```

<span data-ttu-id="d6ad8-136">`dotnet dev-certs https --trust`只有在 macOS 和 Windows 上才支援。</span><span class="sxs-lookup"><span data-stu-id="d6ad8-136">`dotnet dev-certs https --trust` is only supported on macOS and Windows.</span></span> <span data-ttu-id="d6ad8-137">您必須以散發套件支援的方式信任 Linux 上的憑證。</span><span class="sxs-lookup"><span data-stu-id="d6ad8-137">You need to trust certificates on Linux in the way that is supported by your distribution.</span></span> <span data-ttu-id="d6ad8-138">您很可能需要信任您瀏覽器中的憑證。</span><span class="sxs-lookup"><span data-stu-id="d6ad8-138">It is likely that you need to trust the certificate in your browser.</span></span>

<span data-ttu-id="d6ad8-139">在上述命令中， `{ password here }` 將取代為密碼。</span><span class="sxs-lookup"><span data-stu-id="d6ad8-139">In the preceding commands, replace `{ password here }` with a password.</span></span>

<span data-ttu-id="d6ad8-140">建立包含下列內容的_docker-compose.dev.debug.yml yml_檔案：</span><span class="sxs-lookup"><span data-stu-id="d6ad8-140">Create a _docker-compose.debug.yml_ file with the following content:</span></span>

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
<span data-ttu-id="d6ad8-141">Docker 撰寫檔案中指定的密碼必須符合用於憑證的密碼。</span><span class="sxs-lookup"><span data-stu-id="d6ad8-141">The password specified in the docker compose file must match the password used for the certificate.</span></span>

<span data-ttu-id="d6ad8-142">使用針對 HTTPS 設定的 ASP.NET Core 來啟動容器：</span><span class="sxs-lookup"><span data-stu-id="d6ad8-142">Start the container with ASP.NET Core configured for HTTPS:</span></span>

```console
docker-compose -f "docker-compose.debug.yml" up -d
```

### <a name="windows-using-windows-containers"></a><span data-ttu-id="d6ad8-143">使用 Windows 容器的 windows</span><span class="sxs-lookup"><span data-stu-id="d6ad8-143">Windows using Windows containers</span></span>

<span data-ttu-id="d6ad8-144">產生憑證並設定本機電腦：</span><span class="sxs-lookup"><span data-stu-id="d6ad8-144">Generate certificate and configure local machine:</span></span>

```dotnetcli
dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p { password here }
dotnet dev-certs https --trust
```

<span data-ttu-id="d6ad8-145">在上述命令中， `{ password here }` 將取代為密碼。</span><span class="sxs-lookup"><span data-stu-id="d6ad8-145">In the preceding commands, replace `{ password here }` with a password.</span></span>

<span data-ttu-id="d6ad8-146">建立包含下列內容的_docker-compose.dev.debug.yml yml_檔案：</span><span class="sxs-lookup"><span data-stu-id="d6ad8-146">Create a _docker-compose.debug.yml_ file with the following content:</span></span>

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
<span data-ttu-id="d6ad8-147">Docker 撰寫檔案中指定的密碼必須符合用於憑證的密碼。</span><span class="sxs-lookup"><span data-stu-id="d6ad8-147">The password specified in the docker compose file must match the password used for the certificate.</span></span>

<span data-ttu-id="d6ad8-148">使用針對 HTTPS 設定的 ASP.NET Core 來啟動容器：</span><span class="sxs-lookup"><span data-stu-id="d6ad8-148">Start the container with ASP.NET Core configured for HTTPS:</span></span>

```console
docker-compose -f "docker-compose.debug.yml" up -d
```
