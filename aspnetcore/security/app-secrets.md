---
title: 在 ASP.NET Core 開發的應用程式祕密的安全儲存體
author: rick-anderson
description: 了解如何儲存和擷取為應用程式祕密的 ASP.NET Core 應用程式開發期間的機密資訊。
ms.author: scaddie
ms.custom: mvc
ms.date: 03/13/2019
uid: security/app-secrets
ms.openlocfilehash: 195901e466262020fd1217bd9dfb6162910bb861
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64893215"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a>在 ASP.NET Core 開發的應用程式祕密的安全儲存體

藉由[Rick Anderson](https://twitter.com/RickAndMSFT)， [Daniel Roth](https://github.com/danroth27)，和[Scott Addie](https://github.com/scottaddie)

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/app-secrets/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

本文件說明針對儲存和擷取機密資料的 ASP.NET Core 應用程式開發期間的技術。 永遠不會將密碼或其他機密資料儲存在原始程式碼中。 不應使用生產環境祕密進行開發或測試。 您可以透過 [Azure Key Vault 設定提供者](xref:security/key-vault-configuration)儲存及保護 Azure 測試與生產祕密。

## <a name="environment-variables"></a>環境變數

環境變數用來避免儲存在程式碼，或在本機設定檔中的應用程式密碼。 環境變數覆寫所有先前指定的組態來源的組態值。

::: moniker range="<= aspnetcore-1.1"

藉由呼叫設定環境變數值的讀取<xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*>在`Startup`建構函式：

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=8)]

::: moniker-end

中的 ASP.NET Core web 應用程式，請考慮**個別使用者帳戶**安全性已啟用。 包含在專案中預設的資料庫連接字串*appsettings.json*具有索引鍵的檔案`DefaultConnection`。 預設的連接字串是 localdb，這會在使用者模式中執行，而且不需要密碼。 應用程式在部署期間，`DefaultConnection`機碼值可以覆寫環境變數的值。 環境變數可能會儲存機密認證的完整連接字串。

> [!WARNING]
> 環境變數通常會儲存在一般、 未加密的文字。 如果電腦或處理序遭到入侵，就可以由不受信任的合作對象存取環境變數。 您可能需要其他措施以避免使用者密碼洩露。

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="secret-manager"></a>Secret Manager

Secret Manager 工具會在 ASP.NET Core 專案的開發期間儲存機密資料。 在此情況下，某份機密資料會是應用程式祕密。 應用程式密碼會儲存在專案樹狀結構與不同的位置。 應用程式祕密與特定專案相關聯或在數個專案之間共用。 應用程式祕密未簽入原始檔控制中。

> [!WARNING]
> 密碼管理員工具不會加密預存機密資料，並不會被視為受信任存放區。 它是僅限開發用途。 索引鍵和值會儲存在使用者設定檔的目錄中的 JSON 組態檔。

## <a name="how-the-secret-manager-tool-works"></a>Secret Manager 工具的運作方式

密碼管理員工具會將實作的詳細資料加以抽象，包括各值儲存的位置和方式。 不需要知道這些實作細節也可以使用此工具。 值會儲存在 JSON 組態檔中的保護系統的使用者設定檔資料夾在本機電腦上：

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

檔案系統路徑：

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="linux--macostablinuxmacos"></a>[Linux / macOS](#tab/linux+macos)

檔案系統路徑：

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

在上述檔案路徑中，取代`<user_secrets_id>`具有`UserSecretsId`中指定值 *.csproj*檔案。

不要撰寫相依於使用 Secret Manager 工具所儲存的資料格式的位置的程式碼。 這些實作細節可能會變更。 比方說，祕密的值不會加密，但可能在未來。

::: moniker range="<= aspnetcore-2.0"

## <a name="install-the-secret-manager-tool"></a>安裝 Secret Manager 工具

Secret Manager 工具是使用.NET Core CLI，在.NET Core SDK 2.1.300 搭售或更新版本。 如需.NET Core SDK 2.1.300 之前的版本，工具的安裝是必要的。

> [!TIP]
> 執行`dotnet --version`從命令殼層，若要查看已安裝的.NET Core SDK 版本號碼。

如果正在使用的.NET Core SDK 包含工具，就會顯示一則警告：

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

安裝[包含 Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) ASP.NET Core 專案中的 NuGet 套件。 例如: 

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=15-16)]

若要驗證工具安裝命令殼層中執行下列命令：

```console
dotnet user-secrets -h
```

Secret Manager 工具會顯示範例使用方式、 選項和命令說明：

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
> 您必須在相同的目錄 *.csproj*若要執行工具中定義的檔案 *.csproj*檔案的`DotNetCliToolReference`項目。

::: moniker-end

## <a name="enable-secret-storage"></a>啟用密碼儲存體

Secret Manager 工具作儲存在您的使用者設定檔中的專案特定的組態設定。

::: moniker range=">= aspnetcore-3.0"

Secret Manager 工具包括`init`命令，在.NET Core SDK 3.0.100 或更新版本。 若要使用使用者的機密資訊，請在專案目錄執行下列命令：

```console
dotnet user-secrets init
```

上述命令會新增`UserSecretsId`項目內`PropertyGroup`的 *.csproj*檔案。 根據預設，內部文字`UserSecretsId`是 GUID。 內部文字是任意的但專案所特有。

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

若要使用使用者的機密資訊，請定義`UserSecretsId`項目內`PropertyGroup`的 *.csproj*檔案。 內部文字`UserSecretsId`任意的但是是唯一的專案。 開發人員通常會產生 GUID `UserSecretsId`。

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

> [!TIP]
> 在 Visual Studio 中，以滑鼠右鍵按一下方案總管 中的專案，然後選取**管理使用者祕密**從內容功能表。 將這個筆勢`UserSecretsId`項目，為 GUID，以填入 *.csproj*檔案。

## <a name="set-a-secret"></a>設定密碼

定義索引鍵和其值所組成的應用程式祕密。 密碼是該專案相關聯`UserSecretsId`值。 例如，執行下列命令，從在其中的目錄 *.csproj*檔案是否存在：

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

在上述範例中，在冒號表示`Movies`是一種物件常值與`ServiceApiKey`屬性。

可以從其他目錄太使用 Secret Manager 工具。 使用`--project`選項來提供檔案系統路徑，處 *.csproj*檔案是否存在。 例如：

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

### <a name="json-structure-flattening-in-visual-studio"></a>在 Visual Studio 中扁平化的 JSON 結構

Visual Studio**管理使用者祕密**軌跡會開啟*secrets.json*在文字編輯器中的檔案。 內容取代*secrets.json*與儲存的索引鍵 / 值組。 例如：

```json
{
  "Movies": {
    "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
    "ServiceApiKey": "12345"
  }
}
```

透過修改後的 JSON 結構被壓平合併`dotnet user-secrets remove`或`dotnet user-secrets set`。 例如，執行`dotnet user-secrets remove "Movies:ConnectionString"`摺疊`Movies`物件常值。 修改過的檔案如下所示：

```json
{
  "Movies:ServiceApiKey": "12345"
}
```

## <a name="set-multiple-secrets"></a>設定多個密碼

來設定批次的祕密，請使用管線傳送至 JSON`set`命令。 在下列範例中， *input.json*檔案的內容會輸送到`set`命令。

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

開啟命令殼層中，然後執行下列命令：

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="linux--macostablinuxmacos"></a>[Linux / macOS](#tab/linux+macos)

開啟命令殼層中，然後執行下列命令：

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a>存取祕密

[ASP.NET Core 組態 API](xref:fundamentals/configuration/index)提供 Secret Manager 祕密的存取。

::: moniker range=">= aspnetcore-2.0 <= aspnetcore-2.2"

如果您的專案以.NET Framework 為目標，安裝[Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet 套件。

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

在 ASP.NET Core 2.0 或更新版本中，使用者密碼設定來源會自動加入開發模式時的專案呼叫<xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>初始化主應用程式使用預先設定的預設值的新執行個體。 `CreateDefaultBuilder` 呼叫<xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*>時<xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName>是<xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>:

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

當`CreateDefaultBuilder`不是呼叫，藉由呼叫明確新增使用者密碼設定來源<xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*>在`Startup`建構函式。 呼叫`AddUserSecrets`僅當應用程式開發的環境中執行，如下列範例所示：

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

安裝[Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet 套件。

新增使用者密碼設定來源，藉由呼叫<xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*>在`Startup`建構函式：

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

使用者的機密資訊可以透過擷取`Configuration`API:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=26)]

::: moniker-end

## <a name="map-secrets-to-a-poco"></a>對應至 POCO 的祕密

將整個物件常值對應至 poco 物件 （具有屬性的簡單.NET 類別） 可用於彙總相關的屬性就行了。

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

若要將上述的祕密對應到 POCO，使用`Configuration`API[物件 graph 繫結](xref:fundamentals/configuration/index#bind-to-an-object-graph)功能。 下列程式碼會將繫結至自訂`MovieSettings`POCO 和存取`ServiceApiKey`屬性值：

::: moniker range=">= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

::: moniker range="= aspnetcore-1.0"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

`Movies:ConnectionString`並`Movies:ServiceApiKey`祕密對應到中的個別屬性`MovieSettings`:

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a>有祕密的字串取代

以純文字儲存密碼並不安全的。 例如，資料庫連接字串儲存在*appsettings.json*可能包含指定之使用者的密碼：

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

更安全的方法是將密碼儲存作為祕密。 例如: 

```console
dotnet user-secrets set "DbPassword" "pass123"
```

移除`Password`連接字串中的索引鍵-值配對*appsettings.json*。 例如：

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

祕密的值可以設定在<xref:System.Data.SqlClient.SqlConnectionStringBuilder>物件的<xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password*>完成連接字串屬性：

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]

::: moniker-end

## <a name="list-the-secrets"></a>列出祕密

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

執行下列命令，從在其中的目錄 *.csproj*檔案是否存在：

```console
dotnet user-secrets list
```

即會出現下列輸出：

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

在上述範例中，索引鍵的名稱中的冒號表示物件的階層架構內*secrets.json*。

## <a name="remove-a-single-secret"></a>移除單一的祕密

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

## <a name="remove-all-secrets"></a>移除所有的祕密

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

執行下列命令，從在其中的目錄 *.csproj*檔案是否存在：

```console
dotnet user-secrets clear
```

從已刪除的應用程式的所有使用者祕密*secrets.json*檔案：

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
