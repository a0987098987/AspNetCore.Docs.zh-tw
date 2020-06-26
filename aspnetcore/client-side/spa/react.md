---
title: React 專案範本與 ASP.NET Core 搭配使用
author: SteveSandersonMS
description: 了解如何開始使用 React 和 create-react-app 適用的 ASP.NET Core 單頁應用程式 (SPA) 專案範本。
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 03/07/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: spa/react
ms.openlocfilehash: d5185a80895423bdd60309509ab4d1dad4dad12f
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85401723"
---
# <a name="use-the-react-project-template-with-aspnet-core"></a>React 專案範本與 ASP.NET Core 搭配使用

更新的 React 專案範本可利用 React 和 [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) 慣例來實作功能多樣的用戶端使用者介面 (UI)，適合作為 ASP.NET Core 應用程式入門。

這個範本相當於建立一個 ASP.NET Core 專案作為 API 後端，以及建立一個標準 CRA React 專案作為 UI，但是可以將這兩個專案裝載至單一應用程式專案中，這樣便可視為一個整體進行建置與發行。

[回應] 專案範本不是用於伺服器端轉譯（SSR）。 針對具有反應和 Node.js 的 SSR，請考慮[Next.js](https://github.com/zeit/next.js/)或[Razzle](https://github.com/jaredpalmer/razzle)。

## <a name="create-a-new-app"></a>建立新的應用程式

如已安裝 ASP.NET Core 2.1，就沒有必要安裝 React 專案範本。

進入命令提示字元，然後在空目錄中使用 `dotnet new react` 命令以建立新的專案。 例如，下列命令會在 *my-new-app* 目錄中建立應用程式並切換到該目錄：

```dotnetcli
dotnet new react -o my-new-app
cd my-new-app
```

從 Visual Studio 或 .NET Core CLI 執行該應用程式：

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

開啟產生的 *.csproj* 檔案，然後在該處以一般方式來執行該應用程式。

建置程序會在首次執行時還原 npm 相依性，這可能需要幾分鐘時間才能完成。 後續建置所需的時間將會縮短。

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

請確定您有一個名稱為 `ASPNETCORE_Environment` 環境變數而且它的值為 `Development`。 在 Windows 上 (於非 PowerShell 提示字元中)，執行 `SET ASPNETCORE_Environment=Development`。 在 Linux 或 macOS 上，執行 `export ASPNETCORE_Environment=Development`。

執行 [dotnet build](/dotnet/core/tools/dotnet-build)，確認應用程式正確地建置。 建置程序會在首次執行時還原 npm 相依性，這可能需要幾分鐘時間才能完成。 後續建置所需的時間將會縮短。

執行 [dotnet run](/dotnet/core/tools/dotnet-run) 來啟動應用程式。

---

這個專案範本會建立一個 ASP.NET Core 應用程式以及一個 React 應用程式。 ASP.NET Core 應用程式主要用於存取資料、授權以及其他伺服器端事項。 位在 *ClientApp* 子目錄中的 React 應用程式，是用於所有 UI 方面的作業。

## <a name="add-pages-images-styles-modules-etc"></a>新增頁面、影像、樣式、模組等等。

*ClientApp* 目錄是標準 CRA React 應用程式。 如需詳細資訊，請參閱官方 [CRA 文件](https://create-react-app.dev/docs/getting-started/) \(英文\)。

這個範本建立的 React 應用程式以及 CRA 本身建立的應用程式，二者之間存在些微的不同，不過應用程式的功能是一樣的。 由範本所建立的應用程式包含以 [Bootstrap](https://getbootstrap.com/) \(英文\) 為基礎的配置，以及基本的路由範例。

## <a name="install-npm-packages"></a>安裝 npm 套件

若要安裝協力廠商 npm 套件，請在 *ClientApp* 子目錄中使用命令提示字元。 例如：

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

此預設設定有一個缺點。 每當您修改 C# 程式碼後，需要重新啟動 ASP.NET Core 應用程式，CRA 伺服器才會重新啟動。 大約幾秒鐘時間，才會開始備份。 如果您經常編輯 C# 程式碼，且不想要等候 CRA 伺服器重新啟動，可以在外部執行 CRA 伺服器，與 ASP.NET Core 程序獨立開來。 若要這樣做：

1. 使用下列設定，將*env*檔案新增至*ClientApp*子目錄：

    ```
    BROWSER=none
    ```

    這會防止您的網頁瀏覽器在外部啟動 CRA 伺服器時開啟。

2. 在命令提示字元中，切換至 *ClientApp* 子目錄，然後啟動 CRA 程式開發伺服器：

    ```console
    cd ClientApp
    npm start
    ```

3. 修改您的 ASP.NET Core 應用程式來使用外部的 CRA 伺服器執行個體，而不是啟動它自己的一個執行個體。 在 *Startup* 類別中，將 `spa.UseReactDevelopmentServer` 引動過程取代為：

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:3000");
    ```

當您啟動 ASP.NET Core 應用程式，它並不會啟動 CRA 伺服器。 而是改為使用您手動啟動的執行個體。 這樣可以加快它的啟動和重新啟動速度。 不用每一次都要等候 React 應用程式來重新建置。

> [!IMPORTANT]
> 「伺服器端轉譯」不是此範本支援的功能。 此範本的目標是要符合與「建立-回應應用程式」的同位檢查。 因此，不支援「建立-回應應用程式」專案（例如 SSR）中未包含的案例和功能，而且會保留為使用者的練習。

## <a name="additional-resources"></a>其他資源

* <xref:security/authentication/identity/spa>
