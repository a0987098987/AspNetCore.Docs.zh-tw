---
title: 使用 OpenAPI 開發 ASP.NET Core 應用程式
author: ryanbrandenburg
description: 示範如何使用 ' dotnet-openapi ' 工具來加入 OpenAPI 檔案的參考。
ms.author: rybrande
ms.date: 08/26/2019
monikerRange: '>= aspnetcore-3.0'
uid: web-api/Microsoft.dotnet-openapi
ms.openlocfilehash: a9b38bb7e69744d72867bf69cecf1fa92d7c15b3
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/23/2019
ms.locfileid: "71187457"
---
# <a name="develop-aspnet-core-apps-using-openapi-tools"></a>使用 OpenAPI 工具開發 ASP.NET Core 應用程式

依 Ryan Brandenburg

`Microsoft.dotnet-openapi`是一種 .NET Core 通用工具，可用於管理專案中的[OpenAPI](https://github.com/OAI/OpenAPI-Specification)參考。

## <a name="installation"></a>安裝

若要`Microsoft.dotnet-openapi`安裝，請執行下列命令：

```console
dotnet tool install -g Microsoft.dotnet-openapi
```

`Microsoft.dotnet-openapi`是[.Net Core 通用工具](/dotnet/core/tools/global-tools)。

## <a name="add"></a>新增

使用此頁面上的任何命令新增 OpenAPI 參考時，會將`<OpenApiReference />`類似下列的專案新增至 *.csproj*檔案：

```xml
<OpenApiReference Include="openapi.json" />
```

需要先前的參考，應用程式才能呼叫所產生的用戶端程式代碼。

<!-- TODO: Restore after https://github.com/aspnet/AspNetCore/issues/12738
### Add Project

#### Options

| Short option | Long option | Description | Example |
|-------|------|-------|---------|
| -v|--verbose | Show verbose output. |dotnet openapi add project *-v* ../Ref/ProjRef.csproj |
| -p|--project | The project to operate on. |dotnet openapi add project *--project .\Ref.csproj* ../Ref/ProjRef.csproj |

#### Arguments

|  Argument  | Description | Example |
|-------------|-------------|---------|
| source-file | The source to create a reference from. Must be a project file. |dotnet openapi add project *../Ref/ProjRef.csproj* | -->

### <a name="add-file"></a>新增檔案

#### <a name="options"></a>選項

| Short 選項| Long 選項| 描述 | 範例 |
|-------|------|-------|---------|
| -v|--verbose | 顯示詳細資訊輸出。 |dotnet openapi add file *-v* .\OpenAPI.json |
| -p|--updateProject | 要操作的專案。 |dotnet openapi add file *--updateProject .\Ref.csproj* .\OpenAPI.json |
| -c|--程式碼產生器| 要套用至參考的程式碼產生器。 選項為`NSwagCSharp`和`NSwagTypeScript`。 如果`--code-generator`未指定，則工具會預設`NSwagCSharp`為。|dotnet openapi add file .\OpenApi.json--程式碼產生器
| -h|--help|顯示說明資訊|dotnet openapi add file--help|

#### <a name="arguments"></a>引數

|  引數  | 描述 | 範例 |
|-------------|-------------|---------|
| 來源檔案 | 要從中建立參考的來源。 必須是 OpenAPI 檔案。 |dotnet openapi add file *.\OpenAPI.json* |

### <a name="add-url"></a>新增 URL

#### <a name="options"></a>選項

| Short 選項| Long 選項| 描述 | 範例 |
|-------|------|-------------|---------|
| -v|--verbose | 顯示詳細資訊輸出。 |dotnet openapi add url *-v*`https://contoso.com/openapi.json` |
| -p|--updateProject | 要操作的專案。 |dotnet openapi add url *--updateProject .\Ref.csproj*`https://contoso.com/openapi.json` |
| -o|--output-file | 放置 OpenAPI 檔案本機複本的位置。 |dotnet openapi add url `https://contoso.com/openapi.json` *--output-file myclient. json* |
| -c|--程式碼產生器| 要套用至參考的程式碼產生器。 選項為`NSwagCSharp`和`NSwagTypeScript`。 |dotnet openapi add file .\OpenApi.json--程式碼產生器
| -h|--help|顯示說明資訊|dotnet openapi add url--help|

#### <a name="arguments"></a>引數

|  引數  | 描述 | 範例 |
|-------------|-------------|---------|
| 來源-URL | 要從中建立參考的來源。 必須是 URL。 |dotnet openapi 新增 url`https://contoso.com/openapi.json` |

## <a name="remove"></a>移除

從 *.csproj*檔案中移除符合指定檔案名的 OpenAPI 參考。 移除 OpenAPI 參考時，將不會產生用戶端。 已刪除*yaml*檔案。

### <a name="options"></a>選項

| Short 選項| Long 選項| 描述| 範例 |
|-------|------|------------|---------|
| -v|--verbose | 顯示詳細資訊輸出。 |dotnet openapi remove *-v*|
| -p|--updateProject | 要操作的專案。 |dotnet openapi remove *--updateProject .\Ref.csproj* .\OpenAPI.json |
| -h|--help|顯示說明資訊|dotnet openapi remove--help|

### <a name="arguments"></a>引數

|  引數  | 描述| 範例 |
| ------------|------------|---------|
| 來源檔案 | 要移除其參考的來源。 |dotnet openapi remove *.\OpenAPI.json* |

## <a name="refresh"></a>重新整理

重新整理使用下載 URL 的最新內容下載之檔案的本機版本。

### <a name="options"></a>選項

| Short 選項| Long 選項| 描述 | 範例 |
|-------|------|-------------|---------|
| -v|--verbose | 顯示詳細資訊輸出。 | dotnet openapi refresh *-v*`https://contoso.com/openapi.json` |
| -p|--updateProject | 要操作的專案。 | dotnet openapi refresh *--updateProject .\Ref.csproj*`https://contoso.com/openapi.json` |
| -h|--help|顯示說明資訊|dotnet openapi refresh--help|

### <a name="arguments"></a>引數

|  引數  | 描述 | 範例 |
| ------------|-------------|---------|
| 來源-URL | 重新整理參考的來源 URL。 | dotnet openapi refresh`https://contoso.com/openapi.json` |
