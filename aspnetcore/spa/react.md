---
title: "使用 React 專案範本"
author: SteveSandersonMS
description: "了解如何開始使用 ASP.NET Core 單一頁面應用程式 (SPA) 專案範本 React，以及建立 react 應用程式。"
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/react
ms.openlocfilehash: cda9f52d1f5fa1d240e210488bf1bd5c76e49be7
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/02/2018
---
# <a name="use-the-react-project-template"></a>使用 React 專案範本

> [!NOTE]
> 這份文件不需 React 專案範本包含 ASP.NET Core 2.0。 它是有關，您可以手動更新較新的回應範本。 預設範本包含 ASP.NET Core 2.1。

更新的 React 專案範本提供方便的起點適用於 ASP.NET Core 應用程式使用 React 和[建立 react 應用程式](https://github.com/facebookincubator/create-react-app)(CRA) 慣例，來實作豐富的用戶端使用者介面 (UI)。

範本就相當於建立做為 API 後端的 ASP.NET Core 專案和標準 CRA 反應專案，以做為 UI，但方便裝載兩者可以建置並當做單一單位所發行的單一應用程式專案中。

## <a name="create-a-new-app"></a>建立新的應用程式

如果使用 ASP.NET Core 2.0，請確定您已經[安裝更新後的 React 專案範本](xref:spa/index#installation)。 如果您有 ASP.NET Core 2.1，沒有需要安裝它。

建立新的專案，從命令提示字元使用命令`dotnet new react`空的目錄中。 例如，下列命令會建立應用程式在*my-新的應用程式*目錄並切換到該目錄：

```console
dotnet new react -o my-new-app
cd my-new-app
```

執行應用程式，從 Visual Studio 或.NET Core CLI:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

開啟產生*.csproj*檔案，並從該處，像平常一樣執行應用程式。

在建置程序還原第一次執行，可能需要幾分鐘的 npm 相依性。 後續建置會更快。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

請確定您有環境變數呼叫`ASPNETCORE_Environment`值是`Development`。 在 Windows 上 （在非 PowerShell 提示） 中，執行`SET ASPNETCORE_Environment=Development`。 在 Linux 或 macOS 上，執行`export ASPNETCORE_Environment=Development`。

執行[dotnet 組建](/dotnet/core/tools/dotnet-build)以確認您的應用程式建置正確。 在第一個執行中，在建置程序還原 npm 相依性，這可能要花費幾分鐘的時間。 後續建置會更快。

執行[dotnet 執行](/dotnet/core/tools/dotnet-run)啟動應用程式。

---

專案範本建立 ASP.NET Core 應用程式與 React 應用程式。 ASP.NET Core 應用程式被要用於資料存取、 授權和其他伺服器端的問題。 回應應用程式時，位於*ClientApp*子目錄，要用於所有 UI 疑慮。

## <a name="add-pages-images-styles-modules-etc"></a>新增頁面、 影像、 樣式、 模組、 等等。

*ClientApp*目錄是標準 CRA 回應應用程式。 請參閱正式[CRA 文件](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md)如需詳細資訊。

有此範本所建立的 React 應用程式和屬性之間建立 CRA 本身; 有些微的差異不過，應用程式的功能並不會變更。 範本所建立的應用程式包含[Bootstrap](https://getbootstrap.com/)為基礎的配置和基本的路由範例。

## <a name="install-npm-packages"></a>安裝 npm 封裝

若要安裝協力廠商 npm 封裝，請使用 命令提示字元中*ClientApp*子目錄。 例如: 

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a>發行與部署

在開發中，應用程式以方便開發人員最佳化模式執行。 例如，JavaScript 組合會包含來源對應 （以便偵錯時，您可以看到原始的原始程式碼）。 應用程式監看 JavaScript、 HTML 和 CSS 檔案在磁碟上的變更，並自動重新編譯並重新載入時看到這些變更的檔案。

在生產環境中，提供您已針對效能最佳化的應用程式的版本的服務。 這會設定為自動進行。 當您發行時，組建組態就會發出縮短，transpiled 建置您的用戶端程式碼。 不同於開發組建中，在正式組建並不需要在伺服器上安裝 Node.js。

您可以使用標準[ASP.NET Core 裝載和部署方法](xref:host-and-deploy/index)。

## <a name="run-the-cra-server-independently"></a>獨立執行 CRA 伺服器

專案已設定為在背景中啟動 CRA 程式開發伺服器執行個體各自獨立，ASP.NET Core 應用程式開發模式中啟動。 這是很方便，因為這表示您不必手動執行不同的伺服器。

沒有這個預設安裝的缺點。 每當您修改 C# 程式碼，您必須重新啟動，應用程式的 ASP.NET 核心 CRA 伺服器重新啟動。 幾秒鐘的時間才能重新啟動。 如果您正在經常 C# 程式碼編輯，而且不想等候 CRA 伺服器重新啟動，CRA 伺服器外部外獨立執行的 ASP.NET 核心程序。 若要這樣做：

1. 在命令提示字元切換至*ClientApp*子目錄，並啟動 CRA 程式開發伺服器：

    ```console
    cd ClientApp
    npm start
    ```

2. 修改您的 ASP.NET Core 應用程式，而不是啟動其自己的其中一個使用外部 CRA 伺服器執行個體。 在您*啟動*類別中，取代`spa.UseReactDevelopmentServer`引動過程為下列：

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:3000");
    ```

當您啟動 ASP.NET Core 應用程式，它將不會啟動 CRA 伺服器。 改為使用您以手動方式啟動的執行個體。 這可讓它啟動並重新啟動速度。 不會再等候 React 應用程式以重建每一次。
