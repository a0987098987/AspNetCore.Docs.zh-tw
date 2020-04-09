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
# <a name="hosting-aspnet-core-images-with-docker-compose-over-https"></a>託管ASP.NET核心映像與 Docker 組成透過 HTTPS


ASP.NET核心預設情況下使用[HTTPS。](/aspnet/core/security/enforcing-ssl) [HTTPS](https://en.wikipedia.org/wiki/HTTPS)依賴於[證書](https://en.wikipedia.org/wiki/Public_key_certificate)進行信任、標識和加密。

本文件介紹如何使用 HTTPS 執行預建構的容器映像。

有關開發方案[,請參閱使用 Docker 在 HTTPS 上開發 ASP.NET 核心應用程式](https://github.com/dotnet/dotnet-docker/blob/master/samples/run-aspnetcore-https-development.md)。

此範例需要[Docker 17.06](https://docs.docker.com/release-notes/docker-ce)或更高版本的[Docker 用戶端](https://www.docker.com/products/docker)。

## <a name="prerequisites"></a>Prerequisites

本文檔中的某些說明需要[.NET Core 2.2 SDK](https://dotnet.microsoft.com/download)或更高版本。

## <a name="certificates"></a>憑證

域[的生產託管](https://blogs.msdn.microsoft.com/webdev/2017/11/29/configuring-https-in-asp-net-core-across-different-platforms/)需要[證書頒發機構的](https://wikipedia.org/wiki/Certificate_authority)證書。 [Let's Encrypt](https://letsencrypt.org/)是提供免費證書的證書頒發機構。

本文檔使用[自簽名開發證書](https://wikipedia.org/wiki/Self-signed_certificate)在上`localhost`託管預建構的圖像。 這些說明類似於使用生產證書。

對生產憑證:

* 該工具`dotnet dev-certs`不是必需的。
* 憑證不需要存儲在說明中使用的位置。 將證書存儲在網站目錄之外的任何位置。

以下部分卷中包含的說明使用*docker-compose.yml*中的`volumes`屬性將證書裝入容器中。 您可以使用`COPY`*Dockerfile*中的命令將證書添加到容器映射中,但不建議這樣做。 出於以下原因,不建議將憑證複製到映射:

* 這使得使用同一映射使用開發人員證書進行測試變得困難。
* 它使使用同一映像進行生產證書託管變得困難。
* 證書披露存在重大風險。

## <a name="starting-a-container-with-https-support-using-docker-compose"></a>使用 docker 組合使用 Htcritor 容器

對作業系統配置使用以下說明。

### <a name="windows-using-linux-containers"></a>使用 Linux 容器的視窗

產生憑證並設定本地電腦:

```dotnetcli
dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p { password here }
dotnet dev-certs https --trust
```

在前面的命令中,替換為`{ password here }`密碼。

建立包含以下內容的_docker-compose.debug.yml_檔:

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
docker 撰寫檔中指定的密碼必須與用於證書的密碼匹配。

使用為 HTTPS 設定 ASP.NET 核心啟動容器:

```console
docker-compose -f "docker-compose.debug.yml" up -d
```

### <a name="macos-or-linux"></a>macOS 或 Linux

產生憑證並設定本地電腦:

```dotnetcli
dotnet dev-certs https -ep ${HOME}/.aspnet/https/aspnetapp.pfx -p { password here }
dotnet dev-certs https --trust
```

`dotnet dev-certs https --trust`僅在 macOS 和 Windows 上受支援。 您需要以發行版支援的方式信任 Linux 上的證書。 您可能需要在瀏覽器中信任證書。

在前面的命令中,替換為`{ password here }`密碼。

建立包含以下內容的_docker-compose.debug.yml_檔:

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
docker 撰寫檔中指定的密碼必須與用於證書的密碼匹配。

使用為 HTTPS 設定 ASP.NET 核心啟動容器:

```console
docker-compose -f "docker-compose.debug.yml" up -d
```

### <a name="windows-using-windows-containers"></a>使用 Windows 容器的視窗

產生憑證並設定本地電腦:

```dotnetcli
dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p { password here }
dotnet dev-certs https --trust
```

在前面的命令中,替換為`{ password here }`密碼。

建立包含以下內容的_docker-compose.debug.yml_檔:

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
docker 撰寫檔中指定的密碼必須與用於證書的密碼匹配。

使用為 HTTPS 設定 ASP.NET 核心啟動容器:

```console
docker-compose -f "docker-compose.debug.yml" up -d
```
