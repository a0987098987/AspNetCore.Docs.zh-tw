---
title: React 專案範本與 ASP.NET Core 搭配使用
author: SteveSandersonMS
description: 了解如何開始使用 React 和 create-react-app 適用的 ASP.NET Core 單頁應用程式 (SPA) 專案範本。
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/react
ms.openlocfilehash: c88320c628f219652a2cb63a16b494661c481ffb
ms.sourcegitcommit: 466300d32f8c33e64ee1b419a2cbffe702863cdf
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/27/2018
ms.locfileid: "34555335"
---
# <a name="use-the-react-project-template-with-aspnet-core"></a>React 專案範本與 ASP.NET Core 搭配使用

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> 本文件與 ASP.NET Core 2.0 包含的 React 專案範本無關， 而是討論您可以手動更新的新版 React 範本。 ASP.NET Core 2.1 預設會包含此範本。

::: moniker-end

更新的 React 專案範本可利用 React 和 [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) 慣例來實作功能多樣的用戶端使用者介面 (UI)，適合作為 ASP.NET Core 應用程式入門。

這個範本相當於建立一個 ASP.NET Core 專案作為 API 後端，以及建立一個標準 CRA React 專案作為 UI，但是可以將這兩個專案裝載至單一應用程式專案中，這樣便可視為一個整體進行建置與發行。

## <a name="create-a-new-app"></a>建立新的應用程式

如果使用 ASP.NET Core 2.0，請確定您已經[安裝更新的 React 專案範本](xref:spa/index#installation)。 如果您有 ASP.NET Core 2.1，就不需要再安裝。

進入命令提示字元，然後在空目錄中使用 `dotnet new react` 命令以建立新的專案。 例如，下列命令會在 *my-new-app* 目錄中建立應用程式並切換到該目錄：

```console
dotnet new react -o my-new-app
cd my-new-app
```

從 Visual Studio 或 .NET Core CLI 執行該應用程式：

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

開啟產生的 *.csproj* 檔案，然後在該處以一般方式來執行該應用程式。

建置程序會在首次執行時還原 npm 相依性，這可能需要幾分鐘時間才能完成。 後續建置所需的時間將會縮短。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

請確定您有一個名稱為 `ASPNETCORE_Environment` 環境變數而且它的值為 `Development`。 在 Windows 上 (於非 PowerShell 提示字元中)，執行 `SET ASPNETCORE_Environment=Development`。 在 Linux 或 macOS 上，執行 `export ASPNETCORE_Environment=Development`。

執行 [dotnet build](/dotnet/core/tools/dotnet-build)，確認應用程式正確地建置。 建置程序會在首次執行時還原 npm 相依性，這可能需要幾分鐘時間才能完成。 後續建置所需的時間將會縮短。

執行 [dotnet run](/dotnet/core/tools/dotnet-run) 來啟動應用程式。

---

這個專案範本會建立一個 ASP.NET Core 應用程式以及一個 React 應用程式。 ASP.NET Core 應用程式主要用於存取資料、授權以及其他伺服器端事項。 位在 *ClientApp* 子目錄中的 React 應用程式，是用於所有 UI 方面的作業。

## <a name="add-pages-images-styles-modules-etc"></a>新增頁面、影像、樣式、模組等等。

*ClientApp* 目錄是標準 CRA React 應用程式。 如需詳細資訊，請參閱官方 [CRA 文件](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md) \(英文\)。

這個範本建立的 React 應用程式以及 CRA 本身建立的應用程式，二者之間存在些微的不同，不過應用程式的功能是一樣的。 由範本所建立的應用程式包含以 [Bootstrap](https://getbootstrap.com/) \(英文\) 為基礎的配置，以及基本的路由範例。

## <a name="install-npm-packages"></a>安裝 npm 套件

若要安裝協力廠商 npm 套件，請在 *ClientApp* 子目錄中使用命令提示字元。 例如: 

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a>發行與部署

在開發時，應用程式會以方便開發人員操作的模式執行。 例如，JavaScript 組合會包含來源對應 (以便在偵錯時，您可以看到原始程式碼)。 應用程式會監控磁碟上的 JavaScript、HTML 和 CSS 檔案變更，當發現檔案變更時，便自動重新編譯和重新載入。

在實際執行環境中，請提供具有最佳效能的應用程式版本。 這是設定為自動進行的。 當您發行時，組建組態會釋出一個經過精簡、轉譯的用戶端程式碼組建。 不同於開發組建，正式組建不需要在伺服器上安裝 Node.js。

您可以使用標準 [ASP.NET Core 裝載和部署方法](xref:host-and-deploy/index)。

## <a name="run-the-cra-server-independently"></a>獨立執行 CRA 伺服器

專案已設定為：當 ASP.NET Core 應用程式在開發模式中啟動時，便在背景啟動它自己的 CRA 程式開發伺服器執行個體。 因為這表示您不需要手動執行不同的伺服器，所以省了不少麻煩。

此預設設定有一個缺點。 每當您修改 C# 程式碼後，需要重新啟動 ASP.NET Core 應用程式，CRA 伺服器才會重新啟動。 大約幾秒鐘時間，才會開始備份。 如果您經常編輯 C# 程式碼，且不想要等候 CRA 伺服器重新啟動，可以在外部執行 CRA 伺服器，與 ASP.NET Core 程序獨立開來。 方法如下：

1. 在命令提示字元中，切換至 *ClientApp* 子目錄，然後啟動 CRA 程式開發伺服器：

    ```console
    cd ClientApp
    npm start
    ```

2. 修改您的 ASP.NET Core 應用程式來使用外部的 CRA 伺服器執行個體，而不是啟動它自己的一個執行個體。 在 *Startup* 類別中，將 `spa.UseReactDevelopmentServer` 引動過程取代為：

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:3000");
    ```

當您啟動 ASP.NET Core 應用程式，它並不會啟動 CRA 伺服器。 而是改為使用您手動啟動的執行個體。 這樣可以加快它的啟動和重新啟動速度。 不用每一次都要等候 React 應用程式來重新建置。
