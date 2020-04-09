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
# <a name="aspnet-core-web-sdk"></a><span data-ttu-id="00da8-103">ASP.NET核心 Web SDK</span><span class="sxs-lookup"><span data-stu-id="00da8-103">ASP.NET Core Web SDK</span></span>

### <a name="overview"></a><span data-ttu-id="00da8-104">概觀</span><span class="sxs-lookup"><span data-stu-id="00da8-104">Overview</span></span>

<span data-ttu-id="00da8-105">`Microsoft.NET.Sdk.Web`是用於建構ASP.NET核心應用的[MSBuild專案SDK。](https://docs.microsoft.com/visualstudio/msbuild/how-to-use-project-sdk)</span><span class="sxs-lookup"><span data-stu-id="00da8-105">`Microsoft.NET.Sdk.Web` is an [MSBuild project SDK](https://docs.microsoft.com/visualstudio/msbuild/how-to-use-project-sdk) for building ASP.NET Core apps.</span></span> <span data-ttu-id="00da8-106">在沒有此 SDK 的情況下可以建構 ASP.NET 核心應用,但是,Web SDK 是:</span><span class="sxs-lookup"><span data-stu-id="00da8-106">It's possible to build an ASP.NET Core app without this SDK, however, the Web SDK is:</span></span>

* <span data-ttu-id="00da8-107">專為提供一流的體驗而量身定製。</span><span class="sxs-lookup"><span data-stu-id="00da8-107">Tailored towards providing a first-class experience.</span></span>
* <span data-ttu-id="00da8-108">大多數用戶的建議目標。</span><span class="sxs-lookup"><span data-stu-id="00da8-108">The recommended target for most users.</span></span>

<span data-ttu-id="00da8-109">在專案中使用 Web.SDK:</span><span class="sxs-lookup"><span data-stu-id="00da8-109">Use the Web.SDK in a project:</span></span>

  ```xml
  <Project Sdk="Microsoft.NET.Sdk.Web">
    <!-- omitted for brevity -->
  </Project>
  ```

<span data-ttu-id="00da8-110">使用 Web SDK 啟用的功能:</span><span class="sxs-lookup"><span data-stu-id="00da8-110">Features enabled by using the Web SDK:</span></span>

* <span data-ttu-id="00da8-111">面向 .NET Core 3.0 或更高版本的隱式引用的專案:</span><span class="sxs-lookup"><span data-stu-id="00da8-111">Projects targeting .NET Core 3.0 or later implicitly reference:</span></span>

  * <span data-ttu-id="00da8-112">[ASP.NET核心共用框架](xref:fundamentals/metapackage-app)。</span><span class="sxs-lookup"><span data-stu-id="00da8-112">The [ASP.NET Core shared framework](xref:fundamentals/metapackage-app).</span></span>
  * <span data-ttu-id="00da8-113">建構ASP.NET核心應用的[分析器](/visualstudio/extensibility/getting-started-with-roslyn-analyzers)。</span><span class="sxs-lookup"><span data-stu-id="00da8-113">[Analyzers](/visualstudio/extensibility/getting-started-with-roslyn-analyzers) designed for building ASP.NET Core apps.</span></span>
* <span data-ttu-id="00da8-114">Web SDK 匯入 MSBuild 目標,以便使用發佈設定檔並使用 WebDeploy 發表。</span><span class="sxs-lookup"><span data-stu-id="00da8-114">The Web SDK imports MSBuild targets that enable the use of publish profiles and publishing using WebDeploy.</span></span>

### <a name="properties"></a><span data-ttu-id="00da8-115">屬性</span><span class="sxs-lookup"><span data-stu-id="00da8-115">Properties</span></span>

| <span data-ttu-id="00da8-116">屬性</span><span class="sxs-lookup"><span data-stu-id="00da8-116">Property</span></span> | <span data-ttu-id="00da8-117">描述</span><span class="sxs-lookup"><span data-stu-id="00da8-117">Description</span></span> |
| -------- | ----------- |
| `DisableImplicitFrameworkReferences` | <span data-ttu-id="00da8-118">禁用對共用框架的`Microsoft.AspNetCore.App`隱式引用。</span><span class="sxs-lookup"><span data-stu-id="00da8-118">Disables implicit reference to the `Microsoft.AspNetCore.App` shared framework.</span></span> |
| `DisableImplicitAspNetCoreAnalyzers` | <span data-ttu-id="00da8-119">禁用對ASP.NET核心分析器的隱式引用。</span><span class="sxs-lookup"><span data-stu-id="00da8-119">Disables implicit reference to ASP.NET Core analyzers.</span></span> |
| `DisableImplicitComponentsAnalyzers` | <span data-ttu-id="00da8-120">在建Blazor構 (伺服器)應用程式時禁用對 Razor 元件分析器的隱式引用。</span><span class="sxs-lookup"><span data-stu-id="00da8-120">Disables implicit reference to Razor Components analyzers when building Blazor (server) applications.</span></span> |
