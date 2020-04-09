---
title: 搭配 ASP.NET Core 使用 Angular 專案範本
author: SteveSandersonMS
description: 了解如何開始使用適用於 Angular 與 Angular CLI 的 ASP.NET Core 單頁應用程式 (SPA) 專案範本。
monikerRange: '>= aspnetcore-2.1'
ms.author: stevesa
ms.custom: mvc
ms.date: 02/06/2020
uid: spa/angular
ms.openlocfilehash: fee872ff237e14cbe491efed9b320809df4c5654
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78657630"
---
# <a name="use-the-angular-project-template-with-aspnet-core"></a>搭配 ASP.NET Core 使用 Angular 專案範本

更新的 Angular 專案範本很適合作為 ASP.NET Core 應用程式的建置起點，並利用 Angular 和 Angular CLI 來實作功能豐富的用戶端使用者介面 (UI)。

此範本等同於建立 ASP.NET Core 專案作為 API 後端，並建立 Angular CLI 專案作為 UI。 此範本可讓使用者輕鬆地將這兩種專案類型裝載至單一應用程式專案中。 因此，應用程式專案便可以單一單位的方式進行建置與發佈。

## <a name="create-a-new-app"></a>建立新的應用程式

如已安裝 ASP.NET Core 2.1，就沒有必要安裝 Angular 專案範本。

進入命令提示字元，然後在空目錄中使用 `dotnet new angular` 命令以建立新的專案。 例如，下列命令會在 *my-new-app* 目錄中建立應用程式並切換到該目錄：

```dotnetcli
dotnet new angular -o my-new-app
cd my-new-app
```

從 Visual Studio 或 .NET Core CLI 執行該應用程式：

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio/)

開啟產生的 *.csproj* 檔案，然後在該處以一般方式來執行該應用程式。

建置程序會在首次執行時還原 npm 相依性，這可能需要幾分鐘時間才能完成。 後續建置所需的時間將會縮短。

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

請確定您有名為 `ASPNETCORE_Environment` 的環境變數，且其值為 `Development`。 在 Windows 上 (於非 PowerShell 提示字元中)，執行 `SET ASPNETCORE_Environment=Development`。 在 Linux 或 macOS 上，執行 `export ASPNETCORE_Environment=Development`。

執行 [dotnet build](/dotnet/core/tools/dotnet-build) 以確認應用程式有正確建置。 建置程序會在首次執行時還原 npm 相依性，這可能需要幾分鐘時間才能完成。 後續建置所需的時間將會縮短。

執行 [dotnet run](/dotnet/core/tools/dotnet-run) 來啟動應用程式。 系統會記錄一則類似下面的訊息：

```console
Now listening on: http://localhost:<port>
```

在瀏覽器中開啟這個 URL。

> [!WARNING]
> 應用程式會在背景啟動 Angular CLI 伺服器的執行個體。 記錄類似以下內容的訊息 *:NG 即時開發伺服器正在偵聽&lt;本地主機&gt;:其他埠,http://localhost:&lt開啟瀏覽器;&gt;其他連接埠*。 請忽略此訊息&mdash;這**不是** ASP.NET Core 與 Angular CLI 應用程式的合併 URL。

---

專案範本會建立 ASP.NET Core 應用程式及 Angular 應用程式。 ASP.NET Core 應用程式主要用於存取資料、授權以及其他伺服器端事項。 Angular 應用程式 (位於 *ClientApp* 子目錄中) 的目的是用於處理所有 UI 事項。

## <a name="add-pages-images-styles-modules-etc"></a>新增頁面、影像、樣式、模組等等。

*ClientApp* 目錄包含標準 Angular CLI 應用程式。 如需詳細資訊，請參閱 [Angular 文件](https://angular.io) \(英文\)。

由此範本所建立的 Angular 應用程式，以及由 Angular CLI 本身所建立 (透過 `ng new`) 的 Angular 應用程式之間存在些許不同，不過應用程式的功能是一樣的。 由範本所建立的應用程式包含以 [Bootstrap](https://getbootstrap.com/) \(英文\) 為基礎的配置，以及基本的路由範例。

## <a name="run-ng-commands"></a>執行 ng 命令

在命令提示字元中，切換至 *ClientApp* 子目錄：

```console
cd ClientApp
```

如果您已經以全域的方式安裝 `ng` 工具，則可以執行它的任何命令。 例如，您可以執行 `ng lint`、`ng test` 或任何其他 [Angular CLI 命令](https://angular.io/cli) \(英文\)。 您並不需要執行 `ng serve`，因為 ASP.NET Core 應用程式會負責提供應用程式的伺服器端和用戶端部分。 它會在內部針對開發使用 `ng serve`。

如果您尚未安裝 `ng` 工具，請改為執行 `npm run ng`。 例如，您可以選擇執行 `npm run ng lint` 或 `npm run ng test`。

## <a name="install-npm-packages"></a>安裝 npm 套件

若要安裝協力廠商 npm 套件，請在 *ClientApp* 子目錄中使用命令提示字元。 例如：

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a>發行與部署

在開發時，應用程式會以方便開發人員操作的模式執行。 例如，JavaScript 組合會包含來源對應 (以便在偵錯時，您可以看到原始的 TypeScript 程式碼)。 應用程式會監看磁碟上的 TypeScript、HTML 和 CSS 檔案變更，並在發現檔案變更時，自動重新編譯並重新載入。

在實際執行環境中，請提供具有最佳效能的應用程式版本。 這是設定為自動進行的。 當您發行時，組建組態會發出精簡、預先 (AoT) 編譯的用戶端程式碼組建。 與開發生成不同,生產生成不需要在伺服器上安裝 Node.js(除非您啟用了伺服器端呈現 (SSR)。

您可以使用標準 [ASP.NET Core 裝載和部署方法](xref:host-and-deploy/index)。

## <a name="run-ng-serve-independently"></a>獨立執行 "ng serve"

專案已設定為會在 ASP.NET Core 應用程式於開發模式中啟動時，在背景啟動屬於自己的 Angular CLI 伺服器執行個體。 這很方便，因為您不需要手動執行個別的伺服器。

此預設設定有一個缺點。 每當您修改 C# 程式碼，且需要重新啟動 ASP.NET Core 應用程式時，Angular CLI 伺服器都會重新啟動。 開始備份需要大約 10 秒鐘的時間。 如果您經常編輯 C# 程式碼，但又不想要等候 Angular CLI 重新啟動，請獨立於 ASP.NET Core 處理序，於外部執行 Angular CLI 伺服器。 若要這樣做：

1. 在命令提示字元中，切換至 *ClientApp* 子目錄，然後啟動 Angular CLI 程式開發伺服器：

    ```console
    cd ClientApp
    npm start
    ```

    > [!IMPORTANT]
    > 使用 `npm start` 來啟動 Angular CLI 程式開發伺服器，而不是 `ng serve`，以遵守 *package.json* 中的設定。 若要將其他參數傳遞給 Angular CLI 伺服器，請將它們新增至 *package.json* 檔案中相關的 `scripts` 行。

2. 修改您的 ASP.NET Core 應用程式來使用外部的 Angular CLI 執行個體，而非自行啟動執行個體。 在 *Startup* 類別中，將 `spa.UseAngularCliServer` 引動過程取代為：

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:4200");
    ```

當您啟動 ASP.NET Core 應用程式時，它並不會啟動 Angular CLI 伺服器。 而是改為使用您手動啟動的執行個體。 這樣可以加快它的啟動和重新啟動速度。 它再也不必每次都得等候 Angular CLI 重建您的用戶端應用程式。

### <a name="pass-data-from-net-code-into-typescript-code"></a>將資料從.NET 程式碼傳遞至 TypeScript 程式碼

在 SSR 期間，您可能會想要將針對每個要求的資料從 ASP.NET Core 應用程式傳遞至 Angular 應用程式。 例如，您可以傳遞 cookie 資訊，或是從資料庫讀取的某些資料。 若要這樣做，請編輯您的 *Startup* 類別。 在 `UseSpaPrerendering` 的回呼中，請將 `options.SupplyData` 的值設定為下方所示的內容：

```csharp
options.SupplyData = (context, data) =>
{
    // Creates a new value called isHttpsRequest that's passed to TypeScript code
    data["isHttpsRequest"] = context.Request.IsHttps;
};
```

`SupplyData` 回呼可讓您傳遞任意、針對每個要求、JSON 可序列化的資料 (例如，字串、布林值或數字)。 您的 *main.server.ts* 程式碼會以 `params.data` 的形式接收此資料。 例如，上述程式碼範例會將布林值以 `params.data.isHttpsRequest` 的形式傳遞至 `createServerRenderer` 回呼。 您可以透過 Angular 所支援的任何方式，將此傳遞至應用程式的其他部分。 例如，請查看 *main.server.ts* 如何將 `BASE_URL` 值傳遞至任何其建構函式已宣告要接收此值的元件。

### <a name="drawbacks-of-ssr"></a>SSR 的缺點

並非所有應用程式都能從 SSR 獲益。 主要優點為可感知的效能。 透過低速網路連線或低速行動裝置連線至您應用程式的訪客，將能很快地看到初始的 UI，即便需要花上一點時間來擷取或剖析 JavaScript 組合。 不過，很多 SPA 主要都是透過使用高速內部公司網路的高速電腦來使用，因此應用程式幾乎會立即出現。

同時，啟用 SSR 會有很多明顯的缺點。 它會增加開發程序的複雜性。 您的程式碼必須在兩個不同環境中執行：用戶端和伺服器端 (在一個從 ASP.NET Core 叫用的 Node.js 環境中)。 以下是需要考量的事項：

* SSR 需要在您的實際執行伺服器上安裝 Node.js。 這針對某些部署案例 (例如 Azure App Service) 是會自動成立的，不過並不適用於其他如 Azure Service Fabric 的案例。
* 啟用 `BuildServerSideRenderer` 組建旗標會使您的 *node_modules* 目錄進行發行。 此資料夾包含超過 20,000 個檔案，這將會增加部署時間。
* 若要在 Node.js 環境中執行您的程式碼，它不能仰賴瀏覽器特定 JavaScript API (例如 `window` 或 `localStorage`) 的存在。 如果您的程式碼 (或是您參考的某些協力廠商程式庫) 嘗試使用這些 API，則會在 SSR 期間出現錯誤。 例如，不要使用 jQuery，因為它會在許多位置參考瀏覽器特定 API。 若要防止錯誤，您必須避免使用 SSR，或是避免使用瀏覽器特定 API 或程式庫。 您可以將針對這類 API 的呼叫包裝於檢查中，以確保它們不會在 SSR 期間被叫用。 例如，在 JavaScript 或 TypeScript 程式碼中使用類似下列的檢查：

    ```javascript
    if (typeof window !== 'undefined') {
        // Call browser-specific APIs here
    }
    ```

## <a name="additional-resources"></a>其他資源

* <xref:security/authentication/identity/spa>
