---
title: "開始使用 ASP.NET Core 與 Entity Framework 6"
author: tdykstra
description: "本文示範如何使用 Entity Framework 6 ASP.NET Core 應用程式中。"
keywords: "ASP.NET Core，Entity Framework EF 6"
ms.author: tdykstra
manager: wpickett
ms.date: 02/24/2017
ms.topic: article
ms.assetid: 016cc836-4c43-45a4-b9a7-9efaf53350df
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/entity-framework-6
ms.openlocfilehash: e186568e27c067e29985b8a286e26b87c3186ac4
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2017
---
# <a name="getting-started-with-aspnet-core-and-entity-framework-6"></a>開始使用 ASP.NET Core 與 Entity Framework 6

由[Paweł Grudzień](https://github.com/pgrudzien12)， [Damien Pontifex](https://github.com/DamienPontifex)，和[Tom Dykstra](https://github.com/tdykstra)

本文示範如何使用 Entity Framework 6 ASP.NET Core 應用程式中。

## <a name="overview"></a>概觀

若要使用 Entity Framework 6，您的專案具有.NET Framework，根據編譯因為 Entity Framework 6 不支援.NET Core。 如果您需要跨平台功能必須升級至[Entity Framework Core](https://docs.efproject.net)。

使用 Entity Framework 6 ASP.NET Core 應用程式中的建議的方式是將 EF6 內容和類別庫中的模型類別專案的目標為完整 framework。 加入 ASP.NET Core 專案中的類別庫的參考。 請參閱範例[EF6 和 ASP.NET Core 專案與 Visual Studio 方案](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/)。

您無法暫停 EF6 內容 ASP.NET Core 專案中，因為.NET Core 專案不支援的所有功能，例如命令 EF6 *Enable-migrations*需要。

不論您找出 EF6 內容的專案類型、 EF6 命令列工具使用 EF6 內容。 例如，`Scaffold-DbContext`僅供以 Entity Framework Core。 如果您需要執行 EF6 模型反向工程的資料庫，請參閱[Code First 到現有的資料庫](https://msdn.microsoft.com/jj200620)。

## <a name="reference-full-framework-and-ef6-in-the-aspnet-core-project"></a>參考完整的 framework 和 EF6 ASP.NET Core 專案中

您的 ASP.NET Core 專案必須參考.NET framework 和 EF6。 例如， *.csproj* ASP.NET Core 專案檔看起來類似下列範例 （顯示檔案相關的部分）。

[!code-xml[](entity-framework-6/sample/MVCCore/MVCCore.csproj?range=3-9&highlight=2)]

如果您要建立新的專案，使用**ASP.NET Core Web 應用程式 (.NET Framework)**範本。

## <a name="handle-connection-strings"></a>處理連接字串

EF6 命令列工具，您將使用 EF6 類別庫專案中需要預設建構函式，因此它們可以具現化內容。 但是，您可能會想要指定連接字串，在 ASP.NET Core 專案中，在此情況下使用內容的建構函式必須要有可讓您在連接字串中傳遞的參數。 以下是範例。

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContext.cs?name=snippet_Constructor)]

因為 EF6 內容沒有無參數建構函式，必須提供實作 EF6 專案[IDbContextFactory](https://msdn.microsoft.com/library/hh506876)。 EF6 命令列工具會尋找並使用該實作，因此它們可以具現化內容。 以下是範例。

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContextFactory.cs?name=snippet_IDbContextFactory)]

在此範例程式碼，`IDbContextFactory`硬式編碼的連接字串中傳遞實作。 這是命令列工具將使用的連接字串。 您要實作以確保類別庫會使用相同的連接字串呼叫的應用程式所使用的策略。 例如，您可以從這兩個專案中的環境變數取得值。

## <a name="set-up-dependency-injection-in-the-aspnet-core-project"></a>設定 ASP.NET Core 專案中的相依性插入

在核心專案的*Startup.cs*檔案，在中設定相依性插入 (DI) 的 EF6 內容`ConfigureServices`。 應該範圍內的每個要求存留期的 EF 內容物件。

[!code-csharp[](entity-framework-6/sample/MVCCore/Startup.cs?name=snippet_ConfigureServices&highlight=5)]

然後就可以內容的執行個體中的控制站使用 DI。 程式碼會類似於您要撰寫 EF 核心內容：

[!code-csharp[](entity-framework-6/sample/MVCCore/Controllers/StudentsController.cs?name=snippet_ContextInController)]

## <a name="sample-application"></a>範例應用程式

可用的範例應用程式，請參閱[範例 Visual Studio 方案](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/)，本文章。

此範例可以從頭開始建立由 Visual Studio 中的下列步驟：

* 建立方案。

* **加入新的專案 > 網路 > ASP.NET Core Web 應用程式 (.NET Framework)**

* **加入新的專案 > 的傳統 Windows 桌面 > 類別庫 (.NET Framework)**

* 在**Package Manager Console** (PMC) 這兩個專案中，執行命令`Install-Package Entityframework`。

* 在類別庫專案中，建立資料模型類別和內容類別的實作`IDbContextFactory`。

* 在類別庫專案的 PMC，執行命令`Enable-Migrations`和`Add-Migration Initial`。 如果您已經設定 ASP.NET Core 專案為啟始專案，加入`-StartupProjectName EF6`這些命令。

* 在 [核心] 專案中，加入類別庫專案的專案參考。

* 在核心專案中，在*Startup.cs*，DI 註冊內容。

* 在核心專案中，在*appsettings.json*，加入連接字串。

* 在 [核心] 專案中，加入控制器，並確認您可以讀取和寫入資料的檢視表。 （請注意，ASP.NET Core MVC scaffolding 不會使用 EF6 內容類別庫的參考。）

## <a name="summary"></a>總結

本文提供的基本指導方針 ASP.NET Core 應用程式中使用 Entity Framework 6。

## <a name="additional-resources"></a>其他資源

* [Entity Framework 的程式碼為基礎的設定](https://msdn.microsoft.com/data/jj680699.aspx)
