---
title: "使用 Angular 專案範本"
author: SteveSandersonMS
description: "了解如何開始使用 ASP.NET Core 單一頁面應用程式 (SPA) 專案範本，包含 Angular 和 Angular CLI。"
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/angular
ms.openlocfilehash: 07cfd20809acb67bdae6561b6ccd6edf1e70a3fe
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/02/2018
---
# <a name="use-the-angular-project-template"></a>使用 Angular 專案範本

> [!NOTE]
> 這份文件不討論 ASP.NET Core 2.0 中的的 Angular 專案範本。所以，您可以手動更新較新的 Angular 相關範本。 範本預設包含在 ASP.NET Core 2.1 之中。

已更新的 Angular 專案範本提供方便的起點在 ASP.NET Core 應用程式中使用 Angular 和 Angular CLI 來實作豐富的用戶端使用者介面 (UI)。

範本相當於建立 ASP.NET Core 的 API 專案作為後端，和有 Angular CLI 專案做為使用者介面。 範本在單一應用程式專案中提供了這兩種專案類型。 因此，應用程式專案可以建立並發佈成單一單位。

## <a name="create-a-new-app"></a>建立新的應用程式

如果使用 ASP.NET Core 2.0，請確定您已經[安裝更新的 Angular 專案範本](xref:spa/index#installation)。 如果您有 ASP.NET Core 2.1，不需要額外安裝它。

建立新的專案，從命令提示字元使用命令`dotnet new angular`空的目錄中。 例如，下列命令會建立應用程式在*my-new-app*目錄並切換到該目錄：

```console
dotnet new angular -o my-new-app
cd my-new-app
```

使用 Visual Studio 或 .NET Core CLI 執行應用程式:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

開啟產生*.csproj*檔案，並從該處，像平常一樣執行應用程式。

在建置程序還原第一次執行，可能需要幾分鐘的 npm 相依性。 後續建置會更快。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

請確定您有環境變數呼叫`ASPNETCORE_Environment`值是`Development`。 在 Windows 上 （在非 PowerShell 提示） 中，執行`SET ASPNETCORE_Environment=Development`。 在 Linux 或 macOS 上，執行`export ASPNETCORE_Environment=Development`。

執行[dotnet 組建](/dotnet/core/tools/dotnet-build)以確認應用程式建置正確。 在第一個執行中，在建置程序還原 npm 相依性，這可能要花費幾分鐘的時間。 後續建置會更快。

執行[dotnet 執行](/dotnet/core/tools/dotnet-run)啟動應用程式。 會記錄類似下列訊息：

```console
Now listening on: http://localhost:<port>
```

在瀏覽器中瀏覽此 url 。

應用程式啟動的 Angular CLI 伺服器在背景中執行個體設定。 會記錄類似下列的訊息： * NG Live Development Server is listening on localhost:4200, open your browser on http://localhost:4200/ ***. 忽略此訊息&mdash;它不是 ASP.NET Core 和 Angular CLI 整合的應用程式 URL。

---

專案範本建立 ASP.NET Core 應用程式與 Angular 應用程式。 ASP.NET Core 應用程式被要用於資料存取、 授權和其他伺服器端的問題。 位於*ClientApp*子目錄的 Angular 應用程式用於所有 UI 問題。

## <a name="add-pages-images-styles-modules-etc"></a>新增頁面、 影像、 樣式、 模組、 等等。

*ClientApp*目錄包含一般的 Angular CLI 應用程式。 如需詳細資訊請參閱正式的[Angular的文件](https://github.com/angular/angular-cli/wiki)。

使用此 Angular 範本建立的應用程式與使用 Angular CLI (透過`ng new`) 建立的應用程式有些微的不同; 不過，應用程式的功能，維持不變。 範本所建立的應用程式包含[Bootstrap](https://getbootstrap.com/)為基礎的配置和基本的路由範例。

## <a name="run-ng-commands"></a>執行 ng 命令

在命令提示字元切換至*ClientApp*子目錄：

```console
cd ClientApp
```

如果您有安裝全域的`ng`工具，您可以執行任何其命令。 例如，您可以執行`ng lint`， `ng test`，或任何其他[Angular CLI 命令](https://github.com/angular/angular-cli/wiki#additional-commands)。 不需要執行`ng serve`，因為您的 ASP.NET Core 應用程式會處理提供伺服器端和用戶端應用程式部分。 就內部而言，它會使用`ng serve`開發工作。

如果您沒有`ng`工具安裝，改為執行`npm run ng`。 例如，您可以執行`npm run ng lint`或`npm run ng test`。

## <a name="install-npm-packages"></a>安裝 npm 封裝

若要安裝協力廠商 npm 封裝，請使用 命令提示字元中*ClientApp*子目錄。 例如: 

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a>發行與部署

在開發中，應用程式以方便開發人員最佳化模式執行。 例如，JavaScript 組合會包含來源對應 （以便偵錯時，您可以看到原始的 TypeScript 程式碼）。 應用程式監控的 TypeScript、 HTML 和 CSS 檔案變更，在磁碟上自動重新編譯並重新載入時看到這些變更的檔案。

在生產環境中，提供您已針對效能最佳化的應用程式的版本的服務。 這會設定為自動進行。 當您發行時，組建組態發出最小化的, 時間 (AoT) 編譯您的用戶端程式碼的組建。 不同於開發組建中，在正式組建不需要在伺服器上安裝 Node.js (除非您已啟用[伺服器端產生內容](#server-side-rendering))。

您可以使用標準[ASP.NET Core 裝載和部署方法](xref:host-and-deploy/index)。

## <a name="run-ng-serve-independently"></a>獨立執行 "ng serve"

專案已設定為在背景中啟動自己的 Angular CLI 伺服器執行個體，ASP.NET Core 應用程式開發模式中啟動。 這是很方便，因為您不需要手動執行不同的伺服器。

沒有這個預設設定有個缺點。 每當您修改 C# 程式碼和您的 ASP.NET 核心應用程式需要重新啟動， Angular CLI 伺服器也會重新啟動。 大約 10 秒鐘，才能重新啟動。 如果您頻繁的編輯 C# 程式碼，而且不想要等待 Angular CLI重新啟動，可以在獨立執行的 ASP.NET 核心程序外啟動 Angular CLI 伺服器。 若要這樣做：

1. 在命令提示字元切換至*ClientApp*子目錄，並啟動角度 CLI 程式開發伺服器：

    ```console
    cd ClientApp
    npm start
    ```

    > [!IMPORTANT]
    > 使用`npm start`啟動 Angular CLI 程式開發伺服器，不要用 `ng serve`，以便遵守在設定*package.json*。 要將其他參數傳遞給 Angular CLI 伺服器，將它們加入到您*package.json*檔案中的`scripts`設定。

2. 修改您的 ASP.NET Core 應用程式去使用外部的 Angular CLI 執行個體，來取代建立自己的。 在您的*Startup*類別中，取代`spa.UseAngularCliServer`引動過程，例如：

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:4200");
    ```

當您啟動 ASP.NET Core 應用程式，它將不會啟動 Angular CLI 伺服器。 改為使用您以手動方式啟動的執行個體。 這可讓它啟動並重新啟動速度。 每次重建您的用戶端應用程式時不用再等候 Angular CLI。

## <a name="server-side-rendering"></a>伺服器端轉譯

效能的部分，您可以選擇在伺服器上預先產生你的 Angular 應用程式內容，如同他在 client 上運行一樣。這表示瀏覽器中會接收 HTML 標記，表示您的應用程式起始 UI，甚至在下載及執行 JavaScript 組合前先顯示。 這個實作的大部分來自稱為[通用 Angular](https://universal.angular.io/)的功能。

> [!TIP]
> 啟用伺服器端轉譯 (SSR) 導入額外複雜性，同時在開發和部署期間的數的字。 閱讀[SSR的缺點](#drawbacks-of-ssr)來判斷 SSR 是否適合您的需求。

若要啟用 SSR，您必須先新增一些調整到您的專案中。

在*Startup*類別*之後*設定列`spa.Options.SourcePath`，和*之前*呼叫`UseAngularCliServer`或`UseProxyToSpaDevelopmentServer`，加入下列：

[!code-csharp[](sample/AngularServerSideRendering/Startup.cs?name=snippet_Call_UseSpa&highlight=5-12)]

在開發模式中，這段程式碼會嘗試執行指令碼建立 SSR 配套`build:ssr`，定義在*ClientApp\package.json*。 這會建置 Angular 的應用程式，名稱為`ssr`，但未定義。 

在結尾`apps`陣列中*ClientApp/.angular-cli.json*，定義額外的應用程式，但名稱`ssr`。 使用下列選項：

[!code-json[](sample/AngularServerSideRendering/ClientApp/.angular-cli.json?range=24-41)]

這個新的 SSR 啟用應用程式設定需要進一步的兩個檔案： *tsconfig.server.json*和*main.server.ts*。 *Tsconfig.server.json*檔案會指定 TypeScript 編譯選項。 *Main.server.ts*期間 SSR 檔將做為程式碼進入點。

加入新的檔案稱為*tsconfig.server.json*內*ClientApp/src* (與現有*tsconfig.app.json*)，其中包含下列：

[!code-json[](sample/AngularServerSideRendering/ClientApp/src/tsconfig.server.json)]

這個檔案會設定要尋找模組呼叫 Angular 的 AoT 編譯器`app.server.module`。 藉由建立新的檔案，在加入此*ClientApp/src/app/app.server.module.ts* (連同現有*app.module.ts*) 包含下列： 

[!code-typescript[](sample/AngularServerSideRendering/ClientApp/src/app/app.server.module.ts)]

此模組會繼承您的用戶端`app.module`和定義在 SSR 期間可以使用的額外 Angular 模組。

請記得，新`ssr`中的項目*.angular cli.json*參考呼叫的進入點檔案*main.server.ts*。 您尚未新增該檔案中，現在正是時候這樣做的時間了。 建立新的檔案在*ClientApp/src/main.server.ts* (連同現有*main.ts*)，其中包含下列：

[!code-typescript[](sample/AngularServerSideRendering/ClientApp/src/main.server.ts)]

此檔案的程式碼是 ASP.NET Core 在每個要求執行時使用您加入的中介軟體*Starup*類別`UseSpaPrerendering`。 它會處理接收`params`從.NET 程式碼 （例如所要求的 URL)，以及呼叫 Angular SSR Api 以取得 HTML 結果。 

嚴格來說，在開發模式啟用 SSR 已經足夠了。 請務必做最後一個變更，以便發行時，您的應用程式能正常運作。 在您的應用程式主要的 *.csproj* 檔案中，設定`BuildServerSideRenderer`屬性值設定為`true`:

[!code-xml[](sample/AngularServerSideRendering/AngularServerSideRendering.csproj?name=snippet_EnableBuildServerSideRenderer)]

這會設定建置流程以執行`build:ssr`發行期間和 SSR 檔案部署至伺服器。 如果您不要啟用此功能，在生產環境中 SSR 會失敗。

開發或實際執行模式中，執行您的應用程式，Angular 的程式碼會以 HTML 呈現在伺服器上。 用戶端程式碼也正常的執行。

### <a name="pass-data-from-net-code-into-typescript-code"></a>.NET 程式碼中的資料，傳遞到 TypeScript 程式碼

SSR 期間，您可能想要從 ASP.NET Core 應用程式的每個要求資料傳入 Angular 的應用程式。 比方說，您無法將 cookie 資訊或傳遞的項目從資料庫讀取。 若要這樣做，請編輯您*Startup*類別，其中的回呼`UseSpaPrerendering`，設定的值`options.SupplyData`如下所示：

```csharp
options.SupplyData = (context, data) =>
{
    // Creates a new value called isHttpsRequest that's passed to TypeScript code
    data["isHttpsRequest"] = context.Request.IsHttps;
};
```

`SupplyData`回呼可讓您傳遞任意、 每個要求、 可序列化 JSON 資料 （例如字串、 布林值或數字）。 您*main.server.ts*程式碼會接收為`params.data`。 例如，上述程式碼範例會傳遞做為布林值`params.data.isHttpsRequest`到`createServerRenderer`回呼。 您可以將它傳遞給您的應用程式，以支援 Angular 任何方式的其他組件。 例如，請參閱如何*main.server.ts*傳遞`BASE_URL`接收該宣告的建構函式的任何元件的值。

### <a name="drawbacks-of-ssr"></a>SSR 的缺點

並非所有應用程式會從 SSR 獲益。 主要優點是看得見的效能。 訪客到達您的應用程式，透過低速網路連線或慢速的行動裝置上，可以快速看到初始 UI，即使需要花時間來擷取或剖析 JavaScript 組合。 不過，許多 SPAs 主要用在快速的電腦上的快速、 內部公司網路上的應用程式幾乎會立即出現的位置。

同時，有很大的缺點，以啟用 SSR。 您的開發程序將增加複雜性。 您的程式碼必須在兩個不同環境中執行： 用戶端和伺服器端 （在 Node.js 環境中從 ASP.NET Core 叫用）。 以下是要記住的一些事項：

* SSR 需要安裝在實際執行伺服器上的 Node.js。 這會自動針對某些部署案例，例如 Azure 應用程式服務，但不適用於其他如 Azure Service Fabric 的情況。
* 啟用`BuildServerSideRenderer`建置旗標會導致您*node_modules*目錄發行。 此資料夾包含 20,000 + 檔案，這會增加部署的時間。
* 若要在 Node.js 環境中執行您的程式碼，它不能依賴特定瀏覽器特定的 JavaScript APIs 像是`window`或`localStorage`。 如果您的程式碼 （或您參考某些協力廠商程式庫） 會嘗試使用這些 Api，在 SSR 期間您將會得到錯誤。 例如，請勿使用 jQuery 因為它在許多地方參考了瀏覽器專用的 Api。 若要避免這個錯誤，您必須避免 SSR 或避免特定瀏覽器的應用程式開發介面或程式庫。 您可以將任何這類 Api 的呼叫包裝起來檢查，以確保它們不 SSR 期間會叫用。 比方說，在 JavaScript 或 TypeScript 程式碼中使用的檢查，如下所示：

    ```javascript
    if (typeof window !== 'undefined') {
        // Call browser-specific APIs here
    }
    ```
