---
title: ASP.NET Core MVC 的相容性版本
author: rick-anderson
description: 探索 Startup 類別如何在 ASP.NET Core 中設定服務和應用程式的要求管線。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 9/25/2019
uid: mvc/compatibility-version
ms.openlocfilehash: 35e3b6acba2bc9a0b863bd6d1e96365328b5f169
ms.sourcegitcommit: fae6f0e253f9d62d8f39de5884d2ba2b4b2a6050
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/25/2019
ms.locfileid: "71256167"
---
# <a name="compatibility-version-for-aspnet-core-mvc"></a>ASP.NET Core MVC 的相容性版本

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range="= aspnetcore-3.0"

<xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>方法是 ASP.NET Core 3.0 應用程式的無 op。 也就是說，使用任何`SetCompatibilityVersion` <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion>值呼叫，對應用程式不會有任何影響。

* 下一個次要版本的 ASP.NET Core 可能會提供新`CompatibilityVersion`的值。
* `CompatibilityVersion`的值`Version_2_0`會標示`[Obsolete(...)]`為。 `Version_2_2`
* 請參閱[Antiforgery、CORS、診斷、Mvc 和路由中的重大 API 變更](https://github.com/aspnet/Announcements/issues/387)。 這份清單包含相容性參數的重大變更。

若要查看`SetCompatibilityVersion`如何與 ASP.NET Core 2.x 應用程式搭配運作，請選取本文的[ASP.NET Core 2.2 版本](https://docs.microsoft.com/aspnet/core/mvc/compatibility-version?view=aspnetcore-2.2)。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>方法可讓 ASP.NET Core 2.x 應用程式加入宣告或退出 ASP.NET Core MVC 2.1 或2.2 中引進的可能重大行為變更。 這些可能的重大行為變更通常在於 MVC 子系統的運作方式，以及執行階段呼叫**您的程式碼**的方式。 透過選擇加入，您可以取得最新的行為和 ASP.NET Core 的長期行為。

下列程式碼會將相容性模式設定為 ASP.NET Core 2.2：

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup.cs?name=snippet1)]

建議您使用最新版本 (`CompatibilityVersion.Latest`) 測試應用程式。 預計大部分的應用程式都不會使用最新版本進行重大行為變更。

呼叫`SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`的應用程式會受到保護，以防止 ASP.NET Core 2.1/2.2 MVC 版本中引進的可能重大行為變更。 這項保護：

* 不適用於所有 2.1 和更新版本的變更，它的目標是 MVC 子系統中的可能重大 ASP.NET Core 執行階段行為變更。
* 不會延伸至 ASP.NET Core 3.0。

**未**呼叫`SetCompatibilityVersion`之 ASP.NET Core 2.1 和2.2 應用程式的預設相容性為2.0 相容性。 也就是說，不呼叫 `SetCompatibilityVersion` 等同於呼叫 `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`。

下列程式碼會將相容性模式設定為 ASP.NET Core 2.2，但下列行為除外：

* <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowCombiningAuthorizeFilters>
* <xref:Microsoft.AspNetCore.Mvc.MvcOptions.InputFormatterExceptionPolicy>

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup2.cs?name=snippet1)]

對於出現重大行為變更的應用程式，使用適當的相容性參數：

* 可讓您使用最新版本，並選擇退出特定的重大行為變更。
* 讓您有時間來更新您的應用程式，使其適用於最新的變更。

<xref:Microsoft.AspNetCore.Mvc.MvcOptions> 文件充分說明變更的項目，以及所做變更對於大部分使用者來說都得到改善的原因。

使用 ASP.NET Core 3.0，已移除相容性參數所支援的舊行為。 我們覺得這些是有利於幾乎所有使用者的正向變更。 藉由在2.1 和2.2 中引進這些變更，大部分的應用程式都可以受益，有些則有時間更新。
::: moniker-end