---
title: "ASP.NET Core 2.x 和更新版本的 Microsoft.AspNetCore.All 中繼套件"
author: Rick-Anderson
description: "Microsoft.AspNetCore.All 中繼套件包含所有支援的 ASP.NET Core 和 Entity Framework Core 套件，以及它們的相依性。"
manager: wpickett
ms.author: riande
ms.date: 09/20/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/metapackage
ms.openlocfilehash: 07220fdae299723088fa85e452cedff5e5685bd7
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
#<a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-2x"></a>ASP.NET Core 2.x 的 Microsoft.AspNetCore.All 中繼套件

這項功能需要以 .NET Core 2.x 為目標的 ASP.NET Core 2.x。

ASP.NET Core 的 [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) 中繼套件包括：

* 所有由 ASP.NET Core 小組支援的套件。
* 所有由 Entity Framework Core 支援的套件。 
* ASP.NET Core 與 Entity Framework Core 所使用的內部與第三人相依性。 

`Microsoft.AspNetCore.All` 套件包含 ASP.NET Core 2.x 和 Entity Framework Core 2.x 的所有功能。 預設專案範本會使用此套件。

`Microsoft.AspNetCore.All`　中繼套件的版本號碼代表 ASP.NET Core 版本和 Entity Framework Core 版本 (與 .NET Core 版本一致)。

使用 `Microsoft.AspNetCore.All` 中繼套件的應用程式會自動利用 [.NET Core 執行階段存放區](https://docs.microsoft.com/dotnet/core/deploying/runtime-store)。 此執行階段存放區包含執行 ASP.NET Core 2.x 應用程式所需的所有執行階段資產。 當您使用 `Microsoft.AspNetCore.All` 中繼套件時，**不會**使用應用程式部署所參考之 ASP.NET Core NuGet 套件的任何資產 &mdash; .NET Core 執行階段存放區包含這些資產。 執行階段存放區中的資產會先行編譯，以改善應用程式啟動時間。

您可以使用套件修剪流程來移除未使用的套件。 發行的應用程式輸出中會排除已修剪的套件。

下列 *.csproj* 檔案參考 ASP.NET Core 的 `Microsoft.AspNetCore.All` 中繼套件：

[!code-xml[Main](..\mvc\views\view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=9)]
