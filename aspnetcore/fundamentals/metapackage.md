---
title: "適用於 ASP.NET Core Microsoft.AspNetCore.All metapackage 2.x 及更新版本"
author: Rick-Anderson
description: "Microsoft.AspNetCore.All metapackage 包含所有支援的封裝。"
keywords: "ASP.NET Core、 NuGet 封裝，Microsoft.AspNetCore.All、 metapackage"
ms.author: riande
manager: wpickett
ms.date: 07/16/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/metapackage
ms.openlocfilehash: 255438a4ce36ce4978f8c8ee298388a25ac00d17
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2017
---
#<a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-2x"></a>適用於 ASP.NET Core Microsoft.AspNetCore.All metapackage 2.x

這項功能需要 ASP.NET Core 2.x。

[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All)適用於 ASP.NET Core metapackage 包括：

* 由 ASP.NET Core 小組支援所有封裝。
* 所有支援 Entity Framework Core 的套件。 
* 內部和第 3 合作對象的相依性由 ASP.NET Core 和 Entity Framework Core。 

ASP.NET Core 的所有功能 2.x 和 Entity Framework Core 2.x 包含在`Microsoft.AspNetCore.All`封裝。 預設的專案範本會使用這個封裝。

版本號碼`Microsoft.AspNetCore.All`metapackage 表示的 ASP.NET Core 版本和 Entity Framework Core 版本 （.NET Core 版本與對齊）。

使用應用程式`Microsoft.AspNetCore.All`metapackage 自動利用.NET 核心執行階段存放區。 執行階段存放區包含執行 ASP.NET Core 2.x 應用程式所需的所有執行階段資產。 當您使用`Microsoft.AspNetCore.All`metapackage，**沒有**參考 ASP.NET Core NuGet 套件的資產會隨著應用程式部署&mdash;.NET 核心執行階段存放區包含這些資產。 <!-- todo add link to Runtime store -->在執行階段存放區中的資產中都會先行編譯為了改善應用程式啟動時間。

若要移除未使用的封裝，您可以使用封裝調整處理序。 發行的應用程式的輸出中排除已修剪的封裝。

下列*.csproj*檔案參考`Microsoft.AspNetCore.All`適用於 ASP.NET Core metapackage:

[!code-xml[Main](..\mvc\views\view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=9)]
