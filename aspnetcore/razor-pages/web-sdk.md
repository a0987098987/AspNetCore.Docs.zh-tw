---
title: ASP.NET Core Web SDK
author: Rick-Anderson
description: .NET 的總覽。
ms.author: riande
ms.date: 01/25/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: razor-pages/web-sdk
ms.openlocfilehash: 2797f0b3003b8ad89093fe1115dee2acc8650c73
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82777159"
---
# <a name="aspnet-core-web-sdk"></a>ASP.NET Core Web SDK

### <a name="overview"></a>概觀

`Microsoft.NET.Sdk.Web`是用來建立 ASP.NET Core 應用程式的[MSBuild 專案 SDK](https://docs.microsoft.com/visualstudio/msbuild/how-to-use-project-sdk) 。 您可以不使用此 SDK 來建立 ASP.NET Core 應用程式，不過，Web SDK 是：

* 專為提供最先進的經驗而量身打造。
* 大部分使用者的建議目標。

在專案中使用 Web. SDK：

  ```xml
  <Project Sdk="Microsoft.NET.Sdk.Web">
    <!-- omitted for brevity -->
  </Project>
  ```

使用 Web SDK 啟用的功能：

* 以 .NET Core 3.0 或更新版本為目標的專案隱含參考：

  * [ASP.NET Core 共用架構](xref:fundamentals/metapackage-app)。
  * 專為建立 ASP.NET Core 應用程式而設計的[分析器](/visualstudio/extensibility/getting-started-with-roslyn-analyzers)。
* Web SDK 會匯入 MSBuild 目標，以啟用發行設定檔並使用 WebDeploy 發行。

### <a name="properties"></a>屬性

| 屬性 | 說明 |
| -------- | ----------- |
| `DisableImplicitFrameworkReferences` | 停用對共用架構`Microsoft.AspNetCore.App`的隱含參考。 |
| `DisableImplicitAspNetCoreAnalyzers` | 停用 ASP.NET Core 分析器的隱含參考。 |
| `DisableImplicitComponentsAnalyzers` | 建立Blazor （伺服器） Razor應用程式時，停用對元件分析器的隱含參考。 |
