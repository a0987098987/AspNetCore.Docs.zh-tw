---
title: 適用于 ASP.NET Core 的工具Blazor
author: guardrex
description: 瞭解可用於建立 Blazor 應用程式的工具。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/07/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/tooling
zone_pivot_groups: operating-systems
ms.openlocfilehash: 0bd38d8d16365a80d7954c860a4e20e2280c36b2
ms.sourcegitcommit: e216e8f4afa21215dc38124c28d5ee19f5ed7b1e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/10/2020
ms.locfileid: "86239604"
---
# <a name="tooling-for-aspnet-core-blazor"></a>適用于 ASP.NET Core 的工具Blazor

作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)

::: zone pivot="os-windows"

1. 使用**ASP.NET 和 網頁程式開發**工作負載安裝最新版的[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) 。

1. 建立新專案。

1. 選取 [ ** Blazor 應用程式**]。 選取 [下一步]。

1. 在 [專案名稱]**** 欄位中提供專案名稱，或接受預設專案名稱。 確認 [**位置**] 專案正確，或提供專案的 [位置]。 選取 [建立]。

1. 如需 Blazor WebAssembly 體驗，請選擇** Blazor WebAssembly 應用程式**範本。 如需 Blazor Server 體驗，請選擇** Blazor Server 應用程式**範本。 選取 [建立]。

   如需這兩個 Blazor 裝載模型的詳細資訊，請 *Blazor WebAssembly* *Blazor Server* 參閱 <xref:blazor/hosting-models> 。

1. 按<kbd>Ctrl</kbd> + <kbd>F5</kbd>執行應用程式。

如需信任 ASP.NET Core HTTPS 開發憑證的詳細資訊，請參閱 <xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos> 。

::: zone-end

::: zone pivot="os-linux"

1. 安裝最新版的[.Net Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1)。 如果您先前已安裝 SDK，您可以在命令 shell 中執行下列命令，以判斷已安裝的版本：

   ```dotnetcli
   dotnet --version
   ```

1. 安裝最新版的[Visual Studio Code](https://code.visualstudio.com/)。

1. 安裝[適用于 Visual Studio Code 擴充](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)功能的最新 c #。

1. 如需 Blazor WebAssembly 體驗，請在命令 shell 中執行下列命令：

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   ```

   如需 Blazor Server 體驗，請在命令 shell 中執行下列命令：

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   ```

   如需這兩個 Blazor 裝載模型的詳細資訊，請 *Blazor WebAssembly* *Blazor Server* 參閱 <xref:blazor/hosting-models> 。

1. 在 Visual Studio Code 中開啟 `WebApplication1` 資料夾。

1. IDE 會要求您新增資產，以建立和對專案進行偵錯工具。 選取 [是]。

1. 按<kbd>Ctrl</kbd> + <kbd>F5</kbd>執行應用程式。

## <a name="trust-a-development-certificate"></a>信任開發憑證

沒有任何集中式方法可信任 Linux 上的憑證。 通常會採用下列其中一種方法：

* 在瀏覽器的排除清單中排除應用程式的 URL。
* 信任所有的自我簽署憑證 `localhost` 。
* 將憑證新增至瀏覽器中受信任的憑證清單。

如需詳細資訊，請參閱瀏覽器和 Linux 散發版本所提供的指引。

::: zone-end

::: zone pivot="os-macos"

1. 安裝[Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/)。

1. **File**  >  在 [**開始] 視窗**中選取 [檔案] [**新增方案**] 或 [建立**新**專案]。

1. 在提要欄位中，選取 [ **Web 和主控台**  >  **應用程式**]。

   如需 Blazor WebAssembly 體驗，請選擇** Blazor WebAssembly 應用程式**範本。 如需 Blazor Server 體驗，請選擇** Blazor Server 應用程式**範本。 選取 [下一步]。

   如需這兩個 Blazor 裝載模型的詳細資訊，請 *Blazor WebAssembly* *Blazor Server* 參閱 <xref:blazor/hosting-models> 。

1. 確認 [**驗證**] 已設為 [**無驗證**]。 選取 [下一步]。

1. 在 [**專案名稱**] 欄位中，將應用程式命名為 `WebApplication1` 。 選取 [建立]。

1. 選取 [**執行**  >  **而不進行調試**程式]，以*在沒有偵錯工具的情況下*執行 app。 使用 [**執行**] [啟動] [偵測]  >  **Start Debugging**或 [執行 ( # A0) ] 按鈕執行應用程式，以*使用調試*程式執行應用程式。

如果出現會信任開發憑證的提示，請信任憑證並繼續。 需要使用者和 keychain 密碼，才能信任憑證。 如需信任 ASP.NET Core HTTPS 開發憑證的詳細資訊，請參閱 <xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos> 。

::: zone-end
