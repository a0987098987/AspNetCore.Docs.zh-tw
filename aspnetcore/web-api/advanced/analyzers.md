---
title: 使用 Web API 分析器
author: pranavkm
description: 深入了解 Microsoft.AspNetCore.Mvc.Api.Analyzers 中的 Web API 分析器。
monikerRange: '>= aspnetcore-2.2'
ms.author: pranavkm
ms.custom: mvc
ms.date: 11/13/2018
uid: web-api/advanced/analyzers
ms.openlocfilehash: 89424d89ec2b3125fd3c6b7c86fed2d292b153e6
ms.sourcegitcommit: f202864efca81a72ea7120c0692940c40d9d0630
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/14/2018
ms.locfileid: "51635380"
---
# <a name="use-web-api-analyzers"></a>使用 Web API 分析器

ASP.NET Core 2.2 推出包含 Web API 分析器的 [Microsoft.AspNetCore.Mvc.Api.Analyzers](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Api.Analyzers) NuGet 套件。 在建置 [API 慣例](xref:web-api/advanced/conventions)時，分析器使用以 <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute> 標註的控制器。

## <a name="package-installation"></a>套件安裝

可使用下列方法新增 `Microsoft.AspNetCore.Mvc.Api.Analyzers`：

### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 從 [套件管理員主控台] 視窗中：
  * 移至 [檢視] > [其他視窗] > [套件管理員主控台]。
  * 巡覽至 *ApiConventions.csproj* 檔案所在的目錄。
  * 執行下列命令：

    ```powershell
    Install-Package Microsoft.AspNetCore.Mvc.Api.Analyzers
    ```

* 從 [管理 NuGet 套件] 對話方塊中：
  * 在 [方案總管] > [管理 NuGet 套件] 中，以滑鼠右鍵按一下專案。
  * 將 [套件來源] 設定為 "nuget.org"。
  * 在搜尋方塊中輸入 "Microsoft.AspNetCore.Mvc.Api.Analyzers"。
  * 從 [瀏覽] 索引標籤中選取 "Microsoft.AspNetCore.Mvc.Api.Analyzers" 套件，並按一下 [安裝]。

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* 在 [Solution Pad] > [新增套件...] 中，以滑鼠右鍵按一下 [套件] 資料夾。
* 將 [新增套件] 視窗的 [來源] 下拉式清單設定為 "nuget.org"。
* 在搜尋方塊中輸入 "Microsoft.AspNetCore.Mvc.Api.Analyzers"。
* 從結果窗格中選取 "Microsoft.AspNetCore.Mvc.Api.Analyzers" 套件，然後按一下 [新增套件]。

### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

從 [整合式終端機] 執行下列命令：

```console
dotnet add ApiConventions.csproj package Microsoft.AspNetCore.Mvc.Api.Analyzers
```

### <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

執行下列命令：

```console
dotnet add ApiConventions.csproj package Microsoft.AspNetCore.Mvc.Api.Analyzers
```

---

## <a name="analyzers-for-api-conventions"></a>API 慣例的分析器

Open API 文件包含動作可能傳回的狀態碼及回應型別。 在 ASP.NET Core MVC 中，如 <xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute> 和 <xref:Microsoft.AspNetCore.Mvc.ProducesAttribute> 等屬性會用以記載動作。 <xref:tutorials/web-api-help-pages-using-swagger> 會對記載您的 API 以提供進一步詳細資料。

套件中的其中一個分析器會檢查以 <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute> 標註的控制器，並辨識未完全記載其回應的動作。 參考下列範例：

[!code-csharp[](conventions/sample/Controllers/ContactsController.cs?name=missing404docs&highlight=9)]

上述動作記載 HTTP 200 成功傳回型別，但未記載 HTTP 404 失敗狀態碼。 分析器會回報缺少文件的 HTTP 404 狀態碼作為警告。 提供修正問題的選項。
