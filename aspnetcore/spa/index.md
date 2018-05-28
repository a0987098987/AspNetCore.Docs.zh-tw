---
title: 使用包含 ASP.NET Core 的單頁應用程式範本
author: SteveSandersonMS
description: 了解如何安裝與開始使用 ASP.NET Core 單頁應用程式 (SPA) 專案範本。
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/index
ms.openlocfilehash: f7bb23e9001c7606c3e622bf4575a4debec56ccd
ms.sourcegitcommit: 466300d32f8c33e64ee1b419a2cbffe702863cdf
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/27/2018
---
# <a name="use-the-single-page-application-templates-with-aspnet-core"></a>使用包含 ASP.NET Core 的單頁應用程式範本

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> 發行的 .NET Core 2.0.x SDK 中包含了 Angular、React 與 React with Redux 的舊版專案範本。 本文件與這些舊版的專案範本無關。 本文件適用於最新的 Angular、React 與 React with Redux 專案範本，這些範本可手動安裝到 ASP.NET Core 2.0 中。 根據預設，範本會隨附於 ASP.NET Core 2.1。

::: moniker-end

## <a name="prerequisites"></a>必要條件

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* [Node.js](https://nodejs.org)，版本 6 或更新版本

## <a name="installation"></a>安裝

::: moniker range=">= aspnetcore-2.1"

範本已隨 ASP.NET Core 2.1 安裝。

::: moniker-end

::: moniker range="= aspnetcore-2.0"

如果您有 ASP.NET Core 2.0，請執行下列命令以安裝 Angular、React 與 React with Redux 的已更新 ASP.NET Core 範本：

```console
dotnet new --install Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0
```

::: moniker-end

## <a name="use-the-templates"></a>使用範本

* [使用 Angular 專案範本](xref:spa/angular)
* [使用 React 專案範本](xref:spa/react)
* [使用 React with Redux 專案範本](xref:spa/react-with-redux)
