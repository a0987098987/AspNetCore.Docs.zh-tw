---
title: 使用 Web API 分析器
author: pranavkm
description: 瞭解 ASP.NET Core MVC Web API 分析器套件。
monikerRange: '>= aspnetcore-2.2'
ms.author: prkrishn
ms.custom: mvc
ms.date: 09/05/2019
uid: web-api/advanced/analyzers
ms.openlocfilehash: 1568eb0304a58758caa5f82249dc42872f5c36b9
ms.sourcegitcommit: 116bfaeab72122fa7d586cdb2e5b8f456a2dc92a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/05/2019
ms.locfileid: "70384860"
---
# <a name="use-web-api-analyzers"></a>使用 Web API 分析器

ASP.NET Core 2.2 和更新版本提供 MVC 分析器套件，適用于 Web API 專案。 分析器會處理以標注的<xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute>控制器，同時根據[Web API 慣例](xref:web-api/advanced/conventions)來建立。

分析器套件會通知您有任何控制器動作：

* 傳回未宣告的狀態碼。
* 傳回未宣告的成功結果。
* 記錄不會傳回的狀態碼。
* 包含明確的模型驗證檢查。

::: moniker range=">= aspnetcore-3.0"

## <a name="reference-the-analyzer-package"></a>參考分析器套件

在 ASP.NET Core 3.0 或更新版本中，分析器會包含在 .NET Core SDK 中。 若要在專案中啟用分析器，請在`IncludeOpenAPIAnalyzers`專案檔中包含屬性：

```xml
<PropertyGroup>
 <IncludeOpenAPIAnalyzers>true</IncludeOpenAPIAnalyzers>
</PropertyGroup>
```

::: moniker-end

::: moniker range="= aspnetcore-2.2"

## <a name="package-installation"></a>套件安裝

使用下列其中一種方法來安裝[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Api.Analyzers) NuGet 套件：

### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

從 [套件管理員主控台] 視窗中：
  * 移至 [**查看** > **其他 Windows** > **套件管理員主控台**]。
  * 巡覽至 *ApiConventions.csproj* 檔案所在的目錄。
  * 執行下列命令：

    ```powershell
    Install-Package Microsoft.AspNetCore.Mvc.Api.Analyzers
    ```

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* 以滑鼠右鍵按一下**Solution Pad** > **新增套件 ...** 中的 *套件* 資料夾。
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

::: moniker-end

## <a name="analyzers-for-web-api-conventions"></a>Web API 慣例的分析器

OpenAPI 文件包含動作可能傳回的狀態碼及回應類型。 在 ASP.NET Core MVC 中，如 <xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute> 和 <xref:Microsoft.AspNetCore.Mvc.ProducesAttribute> 等屬性會用以記載動作。 <xref:tutorials/web-api-help-pages-using-swagger>詳細說明如何記錄您的 Web API。

套件中的其中一個分析器會檢查以 <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute> 標註的控制器，並辨識未完全記載其回應的動作。 參考下列範例：

[!code-csharp[](conventions/sample/Controllers/ContactsController.cs?name=missing404docs&highlight=10)]

上述動作記載 HTTP 200 成功傳回型別，但未記載 HTTP 404 失敗狀態碼。 分析器會回報缺少文件的 HTTP 404 狀態碼作為警告。 提供修正問題的選項。

![報告警告的分析器](conventions/_static/Analyzer.gif)

## <a name="additional-resources"></a>其他資源

* <xref:web-api/advanced/conventions>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:web-api/index>
