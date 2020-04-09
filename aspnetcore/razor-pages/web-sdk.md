---
title: ASP.NET核心 Web SDK
author: Rick-Anderson
description: 微軟.NET.Sdk.Web 概述。
ms.author: riande
ms.date: 01/25/2020
no-loc:
- Blazor
uid: razor-pages/web-sdk
ms.openlocfilehash: 6a9d531efd2188aed525c949bb124914c31119db
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78661053"
---
# <a name="aspnet-core-web-sdk"></a>ASP.NET核心 Web SDK

### <a name="overview"></a>概觀

`Microsoft.NET.Sdk.Web`是用於建構ASP.NET核心應用的[MSBuild專案SDK。](https://docs.microsoft.com/visualstudio/msbuild/how-to-use-project-sdk) 在沒有此 SDK 的情況下可以建構 ASP.NET 核心應用,但是,Web SDK 是:

* 專為提供一流的體驗而量身定製。
* 大多數用戶的建議目標。

在專案中使用 Web.SDK:

  ```xml
  <Project Sdk="Microsoft.NET.Sdk.Web">
    <!-- omitted for brevity -->
  </Project>
  ```

使用 Web SDK 啟用的功能:

* 面向 .NET Core 3.0 或更高版本的隱式引用的專案:

  * [ASP.NET核心共用框架](xref:fundamentals/metapackage-app)。
  * 建構ASP.NET核心應用的[分析器](/visualstudio/extensibility/getting-started-with-roslyn-analyzers)。
* Web SDK 匯入 MSBuild 目標,以便使用發佈設定檔並使用 WebDeploy 發表。

### <a name="properties"></a>屬性

| 屬性 | 描述 |
| -------- | ----------- |
| `DisableImplicitFrameworkReferences` | 禁用對共用框架的`Microsoft.AspNetCore.App`隱式引用。 |
| `DisableImplicitAspNetCoreAnalyzers` | 禁用對ASP.NET核心分析器的隱式引用。 |
| `DisableImplicitComponentsAnalyzers` | 在建Blazor構 (伺服器)應用程式時禁用對 Razor 元件分析器的隱式引用。 |
