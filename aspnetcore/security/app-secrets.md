---
title: 安全存放裝置的開發工作中 ASP.NET Core 應用程式密碼
author: rick-anderson
description: 了解如何儲存和擷取應用程式密碼為 ASP.NET Core 應用程式開發期間的機密資訊。
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 05/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/app-secrets
ms.openlocfilehash: 88b4ee9a963543f8cc97cb66271628a14fe657de
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/18/2018
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a>安全存放裝置的開發工作中 ASP.NET Core 應用程式密碼

由[Rick Anderson](https://twitter.com/RickAndMSFT)，[奧 Roth](https://github.com/danroth27)，和[Scott Addie](https://github.com/scottaddie)

::: moniker range="<= aspnetcore-1.1"
[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/1.1) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/2.1) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))
::: moniker-end

本文件說明儲存和擷取機密資料的 ASP.NET Core 應用程式開發期間的技術。 您永遠不應該在原始程式碼中，儲存的密碼或其他機密資料，您不應該在開發中使用生產機密資料或測試模式。 您可以儲存並保護 Azure 測試與實際的機密資訊使用[Azure 金鑰保存庫的組態提供者](xref:security/key-vault-configuration)。

## <a name="environment-variables"></a>環境變數

環境變數用來避免在程式碼，或在本機的組態檔中的應用程式密碼的儲存體。 環境變數覆寫所有先前指定的設定來源的組態值。

::: moniker range="<= aspnetcore-1.1"
藉由呼叫設定環境變數值的讀取[AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables)中`Startup`建構函式：

[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]
::: moniker-end

請考慮 ASP.NET Core web 應用程式在其中**個別使用者帳戶**已啟用安全性。 預設資料庫的連接字串包含在專案中的*appsettings.json*檔案具有索引鍵`DefaultConnection`。 預設的連接字串是 localdb，這會在使用者模式中執行，而不需要密碼。 應用程式部署期間`DefaultConnection`機碼值會覆寫與環境變數的值。 環境變數可能會儲存敏感認證的完整連接字串。

> [!WARNING]
> 環境變數通常儲存在一般、 未加密的文字。 如果電腦或處理序遭到入侵，可以由不受信任的一方存取環境變數。 您可能需要其他措施以避免洩露使用者密碼。

## <a name="secret-manager"></a>密碼管理員

密碼管理員工具的 ASP.NET Core 專案開發時儲存機密資料。 在此內容中的機密資料是應用程式密碼。 應用程式密碼會儲存在專案樹狀結構與不同的位置。 應用程式密碼與特定的專案相關聯或在數個專案之間共用。 應用程式密碼不被簽入原始檔控制。

> [!WARNING]
> 密碼管理員工具不會加密預存機密資料，並不會被視為受信任存放區。 它是僅限開發用途。 索引鍵和值會儲存在使用者設定檔的目錄中的 JSON 組態檔。

## <a name="how-the-secret-manager-tool-works"></a>密碼管理員工具的運作方式

密碼管理員工具會將實作的詳細資料加以抽象，包括各值儲存的位置和方式。 不需要知道這些實作細節也可以使用此工具。 值會儲存在[JSON](https://json.org/)系統保護的使用者設定檔資料夾中的組態檔，在本機電腦上：

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

檔案系統路徑：

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="macostabmacos"></a>[macOS](#tab/macos)

檔案系統路徑：

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

檔案系統路徑：

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

在上述檔案路徑，取代`<user_secrets_id>`與`UserSecretsId`中指定的值 *.csproj*檔案。

不要撰寫程式碼所依賴的位置或格式儲存密碼管理員工具的資料。 這些實作細節，可能會變更。 比方說，密碼的值未加密，但可能在未來。

::: moniker range="<= aspnetcore-2.0"
## <a name="install-the-secret-manager-tool"></a>安裝密碼管理員工具

密碼管理員工具隨附於.NET Core CLI.NET Core SDK 2.1。 .NET Core SDK 2.0 和舊版中，工具的安裝是必要的。

安裝[Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) ASP.NET Core 專案中的 NuGet 封裝：

[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]

若要驗證工具的安裝命令殼層中執行下列命令：

```console
dotnet user-secrets -h
```

密碼管理員工具會顯示範例使用方式、 選項和命令的說明：

```console
Usage: dotnet user-secrets [options] [command]

Options:
  -?|-h|--help                        Show help information
  --version                           Show version information
  -v|--verbose                        Show verbose output
  -p|--project <PROJECT>              Path to project. Defaults to searching the current directory.
  -c|--configuration <CONFIGURATION>  The project configuration to use. Defaults to 'Debug'.
  --id                                The user secret ID to use.

Commands:
  clear   Deletes all the application secrets
  list    Lists all the application secrets
  remove  Removes the specified user secret
  set     Sets the user secret to the specified value

Use "dotnet user-secrets [command] --help" for more information about a command.
```

> [!NOTE]
> 您必須在相同的目錄 *.csproj*檔案來執行工具中定義 *.csproj*檔案的`DotNetCliToolReference`項目。
::: moniker-end

## <a name="set-a-secret"></a>設定密碼

密碼管理員工具會依據儲存在您的使用者設定檔中的專案特定的組態設定。 若要使用使用者的機密資訊，請定義`UserSecretsId`內的項目`PropertyGroup`的 *.csproj*檔案。 值`UserSecretsId`任意的但是是唯一的專案。 開發人員通常會產生的 GUID `UserSecretsId`。

::: moniker range="<= aspnetcore-1.1"
[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-xml[](app-secrets/samples/2.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]
::: moniker-end

> [!TIP]
> 在 Visual Studio 中，以滑鼠右鍵按一下方案總管 中的專案，然後選取**管理使用者密碼**從內容功能表。 新增此筆勢`UserSecretsId`項目，以填入 GUID *.csproj*檔案。 Visual Studio 隨即開啟*secrets.json*文字編輯器中的檔案。 取代內容*secrets.json*與儲存的索引鍵-值組。 例如：[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]

定義索引鍵和其值所組成的應用程式密碼。 密碼會與專案相關聯`UserSecretsId`值。 例如，執行下列命令，從在其中的目錄 *.csproj*檔案是否存在：

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

在上述範例中，使用冒號表示`Movies`是物件常值與`ServiceApiKey`屬性。

可以從其他目錄太使用密碼管理員工具。 使用`--project`選項提供的檔案系統路徑 *.csproj*檔案是否存在。 例如: 

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a>設定多個密碼

批次，密碼可以設定經由管道 JSON`set`命令。 在下列範例中， *input.json*檔案的內容會經由管道輸出至`set`命令。

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

開啟命令殼層，然後執行下列命令：

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

開啟命令殼層，然後執行下列命令：

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

開啟命令殼層，然後執行下列命令：

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a>存取密碼

[ASP.NET Core 組態 API](xref:fundamentals/configuration/index)提供密碼管理員密碼的存取。 如果目標為.NET Core 的 1.x 或.NET Framework 安裝[Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet 封裝。

::: moniker range="<= aspnetcore-1.1"
將使用者密碼設定來源加入`Startup`建構函式：

[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]
::: moniker-end

可以透過擷取使用者密碼`Configuration`API:

::: moniker range="<= aspnetcore-1.1"
[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]
::: moniker-end

## <a name="string-replacement-with-secrets"></a>含有機密資料的字串取代

以純文字儲存密碼很危險。 例如，資料庫連接字串儲存在*appsettings.json*可能包含指定之使用者的密碼：

[!code-json[](app-secrets/samples/2.1/UserSecrets/appsettings-unsecure.json?highlight=3)]

最安全的作法是將密碼儲存做為密碼。 例如: 

```console
dotnet user-secrets set "DbPassword" "pass123"
```

在 密碼取代*appsettings.json*以預留位置。 在下列範例中，`{0}`用做為表單預留位置[複合格式字串](/dotnet/standard/base-types/composite-formatting#composite-format-string)。

[!code-json[](app-secrets/samples/2.1/UserSecrets/appsettings.json?highlight=3)]

密碼的值可以插入到完成連接字串的預留位置：

::: moniker range="<= aspnetcore-1.1"
[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=23-25)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-16)]
::: moniker-end

## <a name="list-the-secrets"></a>列出密碼

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

執行下列命令，從在其中的目錄 *.csproj*檔案是否存在：

```console
dotnet user-secrets list
```

出現下列輸出：

```console
Movies:ServiceApiKey = 12345
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
```

在上述範例中，索引鍵的名稱中的冒號代表物件階層架構內*secrets.json*。

## <a name="remove-a-single-secret"></a>移除單一密碼

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

執行下列命令，從在其中的目錄 *.csproj*檔案是否存在：

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

應用程式的*secrets.json*已修改檔案，以移除相關聯的索引鍵 / 值組`MoviesConnectionString`機碼：

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

執行`dotnet user-secrets list`顯示下列訊息：

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a>移除所有的機密資料

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

執行下列命令，從在其中的目錄 *.csproj*檔案是否存在：

```console
dotnet user-secrets clear
```

已刪除的應用程式的所有使用者密碼從*secrets.json*檔案：

```json
{}
```

執行`dotnet user-secrets list`顯示下列訊息：

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>