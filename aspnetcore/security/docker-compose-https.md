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
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/docker-compose-https
ms.openlocfilehash: 533d86fb17e3c89fdca59685b090645a11ba5473
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82775137"
---
# <a name="hosting-aspnet-core-images-with-docker-compose-over-https"></a>透過 HTTPS Docker Compose 裝載 ASP.NET Core 映射


ASP.NET Core 預設會使用[HTTPS](/aspnet/core/security/enforcing-ssl)。 [HTTPS](https://en.wikipedia.org/wiki/HTTPS)依賴[憑證](https://en.wikipedia.org/wiki/Public_key_certificate)來進行信任、身分識別和加密。

本檔說明如何使用 HTTPS 來執行預先建立的容器映射。

如需開發案例，請參閱[使用 Docker OVER HTTPS 開發 ASP.NET Core 應用程式](https://github.com/dotnet/dotnet-docker/blob/master/samples/run-aspnetcore-https-development.md)。

此範例需要 docker [17.06](https://docs.docker.com/release-notes/docker-ce)或更新版本的[docker 用戶端](https://www.docker.com/products/docker)。

## <a name="prerequisites"></a>先決條件

本檔中的部分指示需要[.Net Core 2.2 SDK](https://dotnet.microsoft.com/download)或更新版本。

## <a name="certificates"></a>憑證

需要[憑證授權單位](https://wikipedia.org/wiki/Certificate_authority)單位的憑證，才能針對網域進行[生產環境裝載](https://blogs.msdn.microsoft.com/webdev/2017/11/29/configuring-https-in-asp-net-core-across-different-platforms/)。 [Let's Encrypt](https://letsencrypt.org/)是提供免費憑證的憑證授權單位單位。

本檔使用[自我簽署的開發憑證](https://wikipedia.org/wiki/Self-signed_certificate)來裝載預先建立的映射`localhost`。 這些指示與使用生產憑證類似。

針對生產環境憑證：

* 不`dotnet dev-certs`需要此工具。
* 憑證不需要儲存在指示所使用的位置。 將憑證儲存在網站目錄以外的任何位置。

下列區段中所包含的指示會使用*docker-compose.dev.debug.yml. yml*中的`volumes`屬性，將憑證掛接到容器中。 您可以使用`COPY` *Dockerfile*中的命令將憑證新增至容器映射，但不建議這麼做。 基於下列原因，不建議將憑證複製到映射：

* 這會讓您難以使用相同的映射來測試開發人員憑證。
* 這會讓使用相同的映射來裝載生產憑證變得很容易。
* 憑證洩漏有嚴重的風險。

## <a name="starting-a-container-with-https-support-using-docker-compose"></a>使用 docker 撰寫啟動具有 HTTPs 支援的容器

針對您的作業系統設定，請使用下列指示。

### <a name="windows-using-linux-containers"></a>使用 Linux 容器的 Windows

產生憑證並設定本機電腦：

```dotnetcli
dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p { password here }
dotnet dev-certs https --trust
```

在上述命令中，將`{ password here }`取代為密碼。

建立包含下列內容的_docker-compose.dev.debug.yml yml_檔案：

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
Docker 撰寫檔案中指定的密碼必須符合用於憑證的密碼。

使用針對 HTTPS 設定的 ASP.NET Core 來啟動容器：

```console
docker-compose -f "docker-compose.debug.yml" up -d
```

### <a name="macos-or-linux"></a>macOS 或 Linux

產生憑證並設定本機電腦：

```dotnetcli
dotnet dev-certs https -ep ${HOME}/.aspnet/https/aspnetapp.pfx -p { password here }
dotnet dev-certs https --trust
```

`dotnet dev-certs https --trust`只有在 macOS 和 Windows 上才支援。 您必須以散發版本支援的方式信任 Linux 上的憑證。 您很可能需要信任您瀏覽器中的憑證。

在上述命令中，將`{ password here }`取代為密碼。

建立包含下列內容的_docker-compose.dev.debug.yml yml_檔案：

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
Docker 撰寫檔案中指定的密碼必須符合用於憑證的密碼。

使用針對 HTTPS 設定的 ASP.NET Core 來啟動容器：

```console
docker-compose -f "docker-compose.debug.yml" up -d
```

### <a name="windows-using-windows-containers"></a>使用 Windows 容器的 windows

產生憑證並設定本機電腦：

```dotnetcli
dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p { password here }
dotnet dev-certs https --trust
```

在上述命令中，將`{ password here }`取代為密碼。

建立包含下列內容的_docker-compose.dev.debug.yml yml_檔案：

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
Docker 撰寫檔案中指定的密碼必須符合用於憑證的密碼。

使用針對 HTTPS 設定的 ASP.NET Core 來啟動容器：

```console
docker-compose -f "docker-compose.debug.yml" up -d
```
