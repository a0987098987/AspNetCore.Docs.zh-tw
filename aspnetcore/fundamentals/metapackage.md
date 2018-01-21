---
title: "適用於 ASP.NET Core Microsoft.AspNetCore.All metapackage 2.x 及更新版本"
author: Rick-Anderson
description: "Microsoft.AspNetCore.All metapackage 包含所有支援的 ASP.NET Core 和 Entity Framework Core 套件，以及它們的相依性。"
ms.author: riande
manager: wpickett
ms.date: 09/20/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/metapackage
ms.openlocfilehash: 8a44ee7ebb7e6b0112000429f1f080bceb7dc895
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
#<a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-2x"></a>適用於 ASP.NET Core Microsoft.AspNetCore.All metapackage 2.x

這項功能需要 ASP.NET Core 2.x 目標.NET 核心 2.x。

ASP.NET Core 的 [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) 中繼套件包括：

* 所有由 ASP.NET Core 小組支援的套件。
* 所有由 Entity Framework Core 支援的套件。 
* ASP.NET Core 與 Entity Framework Core 所使用的內部與第三人相依性。 

ASP.NET Core 的所有功能 2.x 和 Entity Framework Core 2.x 包含在`Microsoft.AspNetCore.All`封裝。 預設的專案範本會使用這個封裝。

版本號碼`Microsoft.AspNetCore.All`metapackage 表示的 ASP.NET Core 版本和 Entity Framework Core 版本 （.NET Core 版本與對齊）。

使用應用程式`Microsoft.AspNetCore.All`metapackage 自動利用[.NET 核心執行階段存放區](https://docs.microsoft.com/dotnet/core/deploying/runtime-store)。 執行階段存放區包含執行 ASP.NET Core 2.x 應用程式所需的所有執行階段資產。 當您使用`Microsoft.AspNetCore.All`metapackage，**沒有**參考 ASP.NET Core NuGet 套件的資產會隨著應用程式部署&mdash;.NET 核心執行階段存放區包含這些資產。 在執行階段存放區中的資產中都會先行編譯為了改善應用程式啟動時間。

若要移除未使用的封裝，您可以使用封裝調整處理序。 發行的應用程式的輸出中排除已修剪的封裝。

下列*.csproj*檔案參考`Microsoft.AspNetCore.All`適用於 ASP.NET Core metapackage:

[!code-xml[Main](..\mvc\views\view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=9)]
