---
title: "使用 dotnet watch 開發 ASP.NET Core 應用程式"
author: rick-anderson
description: "本教學課程會示範如何在 ASP.NET Core 應用程式中安裝及使用 .NET Core CLI 檔案監看員 (dotnet 監看式) 工具。"
manager: wpickett
ms.author: riande
ms.date: 10/05/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/dotnet-watch
ms.openlocfilehash: cb15e28cb98ea82091cf5ddeed12df8926079e52
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="developing-aspnet-core-apps-using-dotnet-watch"></a>使用 dotnet watch 開發 ASP.NET Core 應用程式

作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 和 [Victor Hurdugaci](https://twitter.com/victorhurdugaci)

`dotnet watch` 是一種工具，會在來源檔案變更時執行 [.NET Core CLI](/dotnet/core/tools) 命令。 例如，檔案變更會觸發編譯、測試執行或部署。

在本教學課程中，我們會使用現有的 Web API 應用程式與兩個端點：一個傳回加總，另一個傳回產品。 Product 方法包含一個 Bug，我們會在本教學課程中一併修正。

下載[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample)。 它包含兩個專案：*WebApp* (ASP.NET Core Web API) 和 *WebAppTests* (Web API 的單元測試)。

在命令殼層中，巡覽至 *WebApp* 資料夾並執行下列命令：

```console
dotnet run
```

主控台輸出會顯示類似如下的訊息 (指出應用程式正在執行，並等待要求)：

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

在網頁瀏覽器中，瀏覽至 `http://localhost:<port number>/api/math/sum?a=4&b=5`。 您應該會看到 `9` 的結果。

瀏覽至產品 API (`http://localhost:<port number>/api/math/product?a=4&b=5`)。 它會傳回 `9`，而非您預期的 `20`。 我們稍後將在本教學課程中對此進行修正。

## <a name="add-dotnet-watch-to-a-project"></a>將 `dotnet watch` 新增至專案

1. 將 `Microsoft.DotNet.Watcher.Tools` 套件參考新增至 *.csproj* 檔案：

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup> 
    ```

1. 執行下列命令來安裝 `Microsoft.DotNet.Watcher.Tools` 套件：
    
    ```console
    dotnet restore
    ```

## <a name="running-net-core-cli-commands-using-dotnet-watch"></a>使用 `dotnet watch` 執行 .NET Core CLI 命令

任何 [.NET Core CLI 命令](/dotnet/core/tools#cli-commands)都可以使用 `dotnet watch` 執行。 例如: 

| 命令 | 使用監看式的命令 |
| ---- | ----- |
| dotnet run | dotnet watch run |
| dotnet run -f netcoreapp2.0 | dotnet watch run -f netcoreapp2.0 |
| dotnet run -f netcoreapp2.0 -- --arg1 | dotnet watch run -f netcoreapp2.0 -- --arg1 |
| dotnet test | dotnet watch test |

執行 *WebApp* 資料夾中的 `dotnet watch run`。 主控台輸出指出 `watch` 已啟動。

## <a name="making-changes-with-dotnet-watch"></a>使用 `dotnet watch` 來變更資料

請確認 `dotnet watch` 正在執行。

修正 *MathController.cs* 之 `Product` 方法的 Bug，使其傳回產品而非加總：

```csharp
public static int Product(int a, int b)
{
  return a * b;
} 
```

儲存檔案。 主控台輸出指出 `dotnet watch` 已偵測到檔案變更，並重新啟動應用程式。

驗證 `http://localhost:<port number>/api/math/product?a=4&b=5` 是否傳回正確的結果。

## <a name="running-tests-using-dotnet-watch"></a>使用 `dotnet watch` 來執行測試

1. 將 *MathController.cs* 的 `Product` 方法變更回傳回加總，然後儲存檔案。
1. 在命令殼層中，瀏覽至 *WebAppTests* 資料夾。
1. 執行 `dotnet restore`。
1. 執行 `dotnet watch test`。 其輸出指出測試失敗，且監看員正在等候檔案變更：

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. 修正 `Product` 方法程式碼，使其傳回產品。 儲存檔案。

`dotnet watch` 會偵測檔案變更，並重新執行測試。 主控台輸出指出測試成功。

## <a name="dotnet-watch-in-github"></a>GitHub 中的 dotnet-watch

dotnet-watch 是 GitHub [DotNetTools 存放庫](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch)的一部分。

[dotnet-watch 讀我檔案](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md)的 [MSBuild 區段](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch#msbuild)說明如何從要監控的 MSBuild 專案檔設定 dotnet-watch。 [dotnet-watch 讀我檔案](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md)包含本教學課程中未涵蓋的 dotnet-watch 相關資訊。
