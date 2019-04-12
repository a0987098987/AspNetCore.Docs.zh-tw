---
title: 裝載模型的 razor 元件
author: guardrex
description: 了解用戶端 Blazor 和裝載模型的伺服器端 ASP.NET Core Razor 元件。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/28/2019
uid: razor-components/hosting-models
ms.openlocfilehash: 8ed70117c94bf1a3e4c208f70310bbf0473bae44
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2019
ms.locfileid: "59515414"
---
# <a name="razor-components-hosting-models"></a>裝載模型的 razor 元件

藉由[Daniel Roth](https://github.com/danroth27)

Razor 元件是設計用來執行用戶端的 web 架構中的瀏覽器[WebAssembly](http://webassembly.org/)-根據.NET 執行階段 (*Blazor*) 或 ASP.NET Core 中的伺服器端 (*ASP.NET Core Razor元件*)。 無論裝載模型、 應用程式和元件模型*維持不變*。 這篇文章會討論可用的裝載模型：

* [用戶端 Blazor](#client-side-hosting-model)
* [伺服器端 ASP.NET Core Razor 元件](#server-side-hosting-model)

## <a name="client-side-hosting-model"></a>用戶端裝載模型

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

Blazor 的主要裝載模型是 WebAssembly 上的瀏覽器中的執行用戶端。 Blazor 應用程式、其相依性，和 .NET 執行階段會下載至瀏覽器中。 應用程式會直接在瀏覽器 UI 執行緒上執行。 UI 更新及事件處理會發生相同的程序中。 應用程式的資產會部署為 web 伺服器或服務能夠提供靜態內容給用戶端的靜態檔案。

![Blazor 用戶端：Blazor 應用程式會在瀏覽器 UI 執行緒上執行。](hosting-models/_static/client-side.png)

若要建立一個使用的用戶端的裝載模型的 Blazor 應用程式，請使用下列範本：

* **Blazor** ([dotnet 新 blazor](/dotnet/core/tools/dotnet-new))&ndash;部署為一組靜態的檔案。
* **（ASP.NET Core 裝載） 的 Blazor** ([dotnet 新 blazorhosted](/dotnet/core/tools/dotnet-new))&ndash;裝載 ASP.NET Core 伺服器。 ASP.NET Core 應用程式提供給用戶端的 Blazor 應用程式。 與伺服器互動的用戶端 Blazor 應用程式，可以將它透過網路使用的 web API 呼叫或[SignalR](xref:signalr/introduction)。

範本包含*components.webassembly.js*處理的指令碼：

* 下載.NET 執行階段、 應用程式和應用程式的相依性。
* 要執行應用程式的執行階段初始化。

用戶端的裝載模型提供幾項優點。 用戶端 Blazor:

* 有任何的.NET 伺服器端相依性。
* 充分利用用戶端資源和功能。
* 卸載工作從伺服器到用戶端。
* 支援離線案例。

有裝載用戶端的缺點。 用戶端 Blazor:

* 將應用程式限制的瀏覽器的功能。
* 需要支援用戶端硬體和軟體 （例如 WebAssembly 支援）。
* 具有較大的下載大小和較長載入時間的應用程式。
* 較少成熟的.NET 執行階段和工具支援 (例如，限制[.NET Standard](/dotnet/standard/net-standard)支援和偵錯)。

## <a name="server-side-hosting-model"></a>伺服器端裝載模型

使用 ASP.NET Core Razor 元件伺服器端裝載模型，從 ASP.NET Core 應用程式中的伺服器上執行應用程式。 UI 更新、 事件處理及 JavaScript 呼叫透過處理[SignalR](xref:signalr/introduction)連接。

![ASP.NET Core Razor 元件伺服器端：瀏覽器互動 （裝載 ASP.NET Core 應用程式內） 的應用程式伺服器上透過 SignalR 的連線。](hosting-models/_static/server-side.png)

若要建立一個使用的伺服器端的主控模型的 Razor 元件應用程式，請使用 ASP.NET Core **Razor 元件**範本 ([dotnet 新 razorcomponents](/dotnet/core/tools/dotnet-new))。 ASP.NET Core 應用程式裝載 Razor 元件伺服器端應用程式，並設定 SignalR 端點的用戶端連線的位置。 ASP.NET Core 應用程式參考應用程式的`Startup`来加入類別：

* 伺服器端 Razor 元件服務。
* 應用程式，以在要求處理管線。

[!code-csharp[](hosting-models/samples_snapshot/Startup.cs?highlight=5,27)]

*Components.server.js*指令碼&dagger;會建立用戶端連線。 它是保存和還原應用程式狀態為 必要 （例如，如果遺失的網路連線） 的應用程式的責任。

伺服器端的主控模型會提供數個優點。 伺服器端 Razor 的元件：

* 有非常小的應用程式大小比用戶端 Blazor 應用程式，而且速度更快載入。
* 可以充分利用 server 功能，包括使用任何.NET Core 相容的 Api。
* 在.NET Core 上在伺服器上執行，因此現有的.NET 工具，例如偵錯時，如預期運作。
* 使用精簡型用戶端 （例如，不支援 WebAssembly 和資源的瀏覽器受限的裝置）。
* .NET /C#程式碼基底，包括應用程式的元件程式碼，不提供給用戶端。

有伺服器端裝載的缺點。 伺服器端 Razor 的元件：

* 具有較高的延遲：每個使用者互動牽涉到的網路躍點。
* 提供任何離線的支援：如果用戶端連線失敗，應用程式會停止運作。
* 較低的延展性：伺服器必須管理多個用戶端連線，並處理用戶端狀態。
* 需要 ASP.NET Core 伺服器來提供應用程式。 不使用伺服器 （例如，從 CDN) 的部署不可行。

&dagger;*Components.server.js*指令碼發行至下列路徑： *bin / {偵錯 |發行} / {目標 FRAMEWORK} /publish/ {應用程式名稱}。應用程式/dist/架構 （_f)*。
