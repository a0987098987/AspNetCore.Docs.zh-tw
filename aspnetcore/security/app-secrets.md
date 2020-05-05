---
title: 在 ASP.NET Core 中的開發中安全儲存應用程式秘密
author: rick-anderson
description: 瞭解如何在開發 ASP.NET Core 應用程式期間，將機密資訊儲存為應用程式秘密並加以取出。
ms.author: scaddie
ms.custom: mvc
ms.date: 4/20/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/app-secrets
ms.openlocfilehash: 7508aebcda4e14812140f13ece635428908a4abb
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82776678"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a>在 ASP.NET Core 中的開發中安全儲存應用程式秘密

::: moniker range=">= aspnetcore-3.0"

依[Rick Anderson](https://twitter.com/RickAndMSFT)、 [Kirk Larkin](https://twitter.com/serpent5)、 [Daniel Roth](https://github.com/danroth27)和[Scott Addie](https://github.com/scottaddie)

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/app-secrets/samples)（[如何下載](xref:index#how-to-download-a-sample)）

本檔說明在開發電腦上開發 ASP.NET Core 應用程式期間，儲存和取得敏感性資料的技術。 絕對不要將密碼或其他敏感性資料儲存在原始程式碼中。 生產秘密不應用於開發或測試。 秘密不應該與應用程式一起部署。 相反地，您應該透過受控制的方式（例如環境變數、Azure Key Vault 等），在生產環境中提供秘密。您可以使用[Azure Key Vault 設定提供者](xref:security/key-vault-configuration)來儲存及保護 Azure 測試和生產密碼。

## <a name="environment-variables"></a>環境變數

環境變數是用來避免在程式碼或本機設定檔案中儲存應用程式秘密。 環境變數會覆寫所有先前指定之設定來源的設定值。

請考慮啟用**個別使用者帳戶**安全性的 ASP.NET Core web 應用程式。 預設的資料庫連接字串會包含在專案的*appsettings*中，並具有索引鍵`DefaultConnection`。 預設連接字串適用于 LocalDB，它會在使用者模式中執行，而且不需要密碼。 在應用程式部署期間`DefaultConnection` ，可以使用環境變數的值來覆寫金鑰值。 環境變數可能會儲存具有敏感性認證的完整連接字串。

> [!WARNING]
> 環境變數通常會儲存為純文字、未加密的文字。 如果電腦或進程遭到入侵，則不受信任的合作物件可以存取環境變數。 可能需要其他措施來防止洩漏使用者秘密。

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="secret-manager"></a>秘密管理員

秘密管理員工具會在 ASP.NET Core 專案的開發期間儲存機密資料。 在此內容中，有一段敏感性資料是應用程式密碼。 應用程式密碼會儲存在專案樹狀結構的不同位置。 應用程式密碼會與特定專案建立關聯，或在數個專案之間共用。 應用程式秘密不會簽入原始檔控制中。

> [!WARNING]
> 秘密管理員工具不會加密儲存的秘密，也不應視為受信任的存放區。 僅供開發之用。 金鑰和值會儲存在使用者設定檔目錄的 JSON 設定檔中。

## <a name="how-the-secret-manager-tool-works"></a>密碼管理員工具的運作方式

秘密管理員工具會將執行詳細資料（例如儲存值的位置和方式）抽象化出來。 您可以使用此工具，而不需要知道這些執行方式的詳細資料。 這些值會儲存在本機電腦上系統保護的使用者設定檔資料夾中的 JSON 設定檔案：

# <a name="windows"></a>[Windows](#tab/windows)

檔案系統路徑：

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="linux--macos"></a>[Linux/macOS](#tab/linux+macos)

檔案系統路徑：

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

在先前的檔案路徑中， `<user_secrets_id>`將取代`UserSecretsId`為 *.csproj*檔案中指定的值。

請勿撰寫依賴秘密管理員工具所儲存之資料位置或格式的程式碼。 這些執行詳細資料可能會變更。 例如，秘密值不會加密，但未來可能會是。

## <a name="enable-secret-storage"></a>啟用秘密儲存

「秘密管理員」工具會針對儲存在使用者設定檔中的專案特定設定進行操作。

「密碼管理員」工具在`init` .NET Core SDK 3.0.100 或更新版本中包含一個命令。 若要使用使用者秘密，請在專案目錄中執行下列命令：

```dotnetcli
dotnet user-secrets init
```

上述命令會在`UserSecretsId` *.csproj*檔案的中`PropertyGroup`新增專案。 根據預設，的內部文字`UserSecretsId`是 GUID。 內部文字是任意的，但對專案而言是唯一的。

[!code-xml[](app-secrets/samples/3.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

在 Visual Studio 中，以滑鼠右鍵按一下方案總管中的專案，然後從內容功能表中選取 [**管理使用者秘密**]。 此手勢會將`UserSecretsId`已填入 GUID 的元素新增至 *.csproj*檔案。

## <a name="set-a-secret"></a>設定密碼

定義由金鑰和其值組成的應用程式密碼。 此密碼與專案的`UserSecretsId`值相關聯。 例如，從 *.csproj*檔案所在的目錄執行下列命令：

```dotnetcli
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

在上述範例中，冒號表示`Movies`是具有`ServiceApiKey`屬性的物件常值。

秘密管理員工具也可以從其他目錄中使用。 使用`--project`選項來提供 *.csproj*檔案所在的檔案系統路徑。 例如：

```dotnetcli
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

### <a name="json-structure-flattening-in-visual-studio"></a>Visual Studio 中的 JSON 結構簡維

Visual Studio 的 [**管理使用者秘密**] 手勢會在文字編輯器中開啟一個*秘密 json*檔案。 以要儲存的機碼值組取代*密碼. json*的內容。 例如：

```json
{
  "Movies": {
    "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
    "ServiceApiKey": "12345"
  }
}
```

JSON 結構會在透過`dotnet user-secrets remove`或`dotnet user-secrets set`進行修改之後壓平合併。 例如， `dotnet user-secrets remove "Movies:ConnectionString"`執行會折迭`Movies`物件常值。 修改過的檔案如下所示：

```json
{
  "Movies:ServiceApiKey": "12345"
}
```

## <a name="set-multiple-secrets"></a>設定多個秘密

您可以透過將 JSON 傳送至`set`命令的方式來設定密碼批次。 在下列範例中，*輸入 json*檔案的內容會以管道傳送至`set`命令。

# <a name="windows"></a>[Windows](#tab/windows)

開啟命令 shell，然後執行下列命令：

  ```dotnetcli
  type .\input.json | dotnet user-secrets set
  ```

# <a name="linux--macos"></a>[Linux/macOS](#tab/linux+macos)

開啟命令 shell，然後執行下列命令：

  ```dotnetcli
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a>存取秘密

[ASP.NET Core 設定 API](xref:fundamentals/configuration/index)提供秘密管理員密碼的存取權。

當專案呼叫<xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder%2A>以預先設定的預設值來初始化主機的新實例時，會自動在開發模式中新增使用者秘密設定來源。 `CreateDefaultBuilder`當<xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> <xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName>為<xref:Microsoft.Extensions.Hosting.EnvironmentName.Development>時呼叫：

[!code-csharp[](app-secrets/samples/3.x/UserSecrets/Program.cs?name=snippet_CreateHostBuilder&highlight=2)]

若`CreateDefaultBuilder`未呼叫，請呼叫<xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A>以明確新增使用者秘密設定來源。 只有`AddUserSecrets`當應用程式在開發環境中執行時才會呼叫，如下列範例所示：

[!code-csharp[](app-secrets/samples/3.x/UserSecrets/Program2.cs?name=snippet_Host&highlight=6-9)]

您可以透過`Configuration` API 來抓取使用者秘密：

[!code-csharp[](app-secrets/samples/3.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

## <a name="map-secrets-to-a-poco"></a>將秘密對應至 POCO

將整個物件常值對應至 POCO （具有屬性的簡單 .NET 類別），對於匯總相關屬性很有用。

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

若要將上述密碼對應到 POCO，請使用`Configuration` API 的[物件圖形](xref:fundamentals/configuration/index#bind-to-an-object-graph)系結功能。 下列程式碼會系結至`MovieSettings`自訂 POCO 並`ServiceApiKey`存取屬性值：

[!code-csharp[](app-secrets/samples/3.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

`Movies:ConnectionString`和`Movies:ServiceApiKey`密碼會對應至中`MovieSettings`的個別屬性：

[!code-csharp[](app-secrets/samples/3.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a>使用秘密取代字串

以純文字儲存密碼並不安全。 例如，儲存在*appsettings*中的資料庫連接字串可能包含指定使用者的密碼：

[!code-json[](app-secrets/samples/3.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

更安全的方法是將密碼儲存為秘密。 例如：

```dotnetcli
dotnet user-secrets set "DbPassword" "pass123"
```

從 appsettings `Password`的連接字串中移除索引鍵/值*appsettings.json*組。 例如：

[!code-json[](app-secrets/samples/3.x/UserSecrets/appsettings.json?highlight=3)]

您可以在<xref:System.Data.SqlClient.SqlConnectionStringBuilder>物件的<xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password%2A>屬性上設定密碼的值，以完成連接字串：

[!code-csharp[](app-secrets/samples/3.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

## <a name="list-the-secrets"></a>列出秘密

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

從 *.csproj*檔案所在的目錄執行下列命令：

```dotnetcli
dotnet user-secrets list
```

即會出現下列輸出：

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

在上述範例中，索引鍵名稱中的冒號代表在*私密金鑰內的物件階層。*

## <a name="remove-a-single-secret"></a>移除單一秘密

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

從 *.csproj*檔案所在的目錄執行下列命令：

```dotnetcli
dotnet user-secrets remove "Movies:ConnectionString"
```

應用程式的私密金鑰*json*檔案已修改，以移除與`MoviesConnectionString`金鑰相關聯的機碼值組：

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

`dotnet user-secrets list`會顯示下列訊息：

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a>移除所有秘密

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

從 *.csproj*檔案所在的目錄執行下列命令：

```dotnetcli
dotnet user-secrets clear
```

應用程式的所有使用者秘密都已從*密碼 json*檔案中刪除：

```json
{}
```

執行`dotnet user-secrets list`會顯示下列訊息：

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a>其他資源

* 如需從 IIS 存取秘密管理員的相關資訊，請參閱[此問題](https://github.com/dotnet/AspNetCore.Docs/issues/16328)。
* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

由[Rick Anderson](https://twitter.com/RickAndMSFT)、 [Daniel Roth](https://github.com/danroth27)和[Scott Addie](https://github.com/scottaddie)

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/app-secrets/samples)（[如何下載](xref:index#how-to-download-a-sample)）

本檔說明在開發電腦上開發 ASP.NET Core 應用程式期間，儲存和取得敏感性資料的技術。 絕對不要將密碼或其他敏感性資料儲存在原始程式碼中。 生產秘密不應用於開發或測試。 秘密不應該與應用程式一起部署。 相反地，您應該透過受控制的方式（例如環境變數、Azure Key Vault 等），在生產環境中提供秘密。您可以使用[Azure Key Vault 設定提供者](xref:security/key-vault-configuration)來儲存及保護 Azure 測試和生產密碼。

## <a name="environment-variables"></a>環境變數

環境變數是用來避免在程式碼或本機設定檔案中儲存應用程式秘密。 環境變數會覆寫所有先前指定之設定來源的設定值。

請考慮啟用**個別使用者帳戶**安全性的 ASP.NET Core web 應用程式。 預設的資料庫連接字串會包含在專案的*appsettings*中，並具有索引鍵`DefaultConnection`。 預設連接字串適用于 LocalDB，它會在使用者模式中執行，而且不需要密碼。 在應用程式部署期間`DefaultConnection` ，可以使用環境變數的值來覆寫金鑰值。 環境變數可能會儲存具有敏感性認證的完整連接字串。

> [!WARNING]
> 環境變數通常會儲存為純文字、未加密的文字。 如果電腦或進程遭到入侵，則不受信任的合作物件可以存取環境變數。 可能需要其他措施來防止洩漏使用者秘密。

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="secret-manager"></a>秘密管理員

秘密管理員工具會在 ASP.NET Core 專案的開發期間儲存機密資料。 在此內容中，有一段敏感性資料是應用程式密碼。 應用程式密碼會儲存在專案樹狀結構的不同位置。 應用程式密碼會與特定專案建立關聯，或在數個專案之間共用。 應用程式秘密不會簽入原始檔控制中。

> [!WARNING]
> 秘密管理員工具不會加密儲存的秘密，也不應視為受信任的存放區。 僅供開發之用。 金鑰和值會儲存在使用者設定檔目錄的 JSON 設定檔中。

## <a name="how-the-secret-manager-tool-works"></a>密碼管理員工具的運作方式

秘密管理員工具會將執行詳細資料（例如儲存值的位置和方式）抽象化出來。 您可以使用此工具，而不需要知道這些執行方式的詳細資料。 這些值會儲存在本機電腦上系統保護的使用者設定檔資料夾中的 JSON 設定檔案：

# <a name="windows"></a>[Windows](#tab/windows)

檔案系統路徑：

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="linux--macos"></a>[Linux/macOS](#tab/linux+macos)

檔案系統路徑：

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

在先前的檔案路徑中， `<user_secrets_id>`將取代`UserSecretsId`為 *.csproj*檔案中指定的值。

請勿撰寫依賴秘密管理員工具所儲存之資料位置或格式的程式碼。 這些執行詳細資料可能會變更。 例如，秘密值不會加密，但未來可能會是。

## <a name="enable-secret-storage"></a>啟用秘密儲存

「秘密管理員」工具會針對儲存在使用者設定檔中的專案特定設定進行操作。

若要使用使用者秘密，請`UserSecretsId`在 .csproj 檔案`PropertyGroup`的中 *.csproj*定義元素。 的內部文字是`UserSecretsId`任意的，但對專案而言是唯一的。 開發人員通常會產生的`UserSecretsId`GUID。

[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

> [!TIP]
> 在 Visual Studio 中，以滑鼠右鍵按一下方案總管中的專案，然後從內容功能表中選取 [**管理使用者秘密**]。 此手勢會將`UserSecretsId`已填入 GUID 的元素新增至 *.csproj*檔案。

## <a name="set-a-secret"></a>設定密碼

定義由金鑰和其值組成的應用程式密碼。 此密碼與專案的`UserSecretsId`值相關聯。 例如，從 *.csproj*檔案所在的目錄執行下列命令：

```dotnetcli
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

在上述範例中，冒號表示`Movies`是具有`ServiceApiKey`屬性的物件常值。

秘密管理員工具也可以從其他目錄中使用。 使用`--project`選項來提供 *.csproj*檔案所在的檔案系統路徑。 例如：

```dotnetcli
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

### <a name="json-structure-flattening-in-visual-studio"></a>Visual Studio 中的 JSON 結構簡維

Visual Studio 的 [**管理使用者秘密**] 手勢會在文字編輯器中開啟一個*秘密 json*檔案。 以要儲存的機碼值組取代*密碼. json*的內容。 例如：

```json
{
  "Movies": {
    "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
    "ServiceApiKey": "12345"
  }
}
```

JSON 結構會在透過`dotnet user-secrets remove`或`dotnet user-secrets set`進行修改之後壓平合併。 例如， `dotnet user-secrets remove "Movies:ConnectionString"`執行會折迭`Movies`物件常值。 修改過的檔案如下所示：

```json
{
  "Movies:ServiceApiKey": "12345"
}
```

## <a name="set-multiple-secrets"></a>設定多個秘密

您可以透過將 JSON 傳送至`set`命令的方式來設定密碼批次。 在下列範例中，*輸入 json*檔案的內容會以管道傳送至`set`命令。

# <a name="windows"></a>[Windows](#tab/windows)

開啟命令 shell，然後執行下列命令：

  ```dotnetcli
  type .\input.json | dotnet user-secrets set
  ```

# <a name="linux--macos"></a>[Linux/macOS](#tab/linux+macos)

開啟命令 shell，然後執行下列命令：

  ```dotnetcli
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a>存取秘密

[ASP.NET Core 設定 API](xref:fundamentals/configuration/index)提供秘密管理員密碼的存取權。

如果您的專案是以 .NET Framework 為目標，請安裝[Usersecrets.xml](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet 套件。

在 ASP.NET Core 2.0 或更新版本中，當專案呼叫<xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>以預先設定的預設值來初始化主機的新實例時，就會自動在開發模式中新增使用者秘密設定來源。 `CreateDefaultBuilder`當<xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName>為<xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>時呼叫：

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

當`CreateDefaultBuilder`未呼叫時，請在函式中呼叫<xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A>以`Startup`明確新增使用者秘密設定來源。 只有`AddUserSecrets`當應用程式在開發環境中執行時才會呼叫，如下列範例所示：

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_StartupConstructor&highlight=12)]

您可以透過`Configuration` API 來抓取使用者秘密：

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

## <a name="map-secrets-to-a-poco"></a>將秘密對應至 POCO

將整個物件常值對應至 POCO （具有屬性的簡單 .NET 類別），對於匯總相關屬性很有用。

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

若要將上述密碼對應到 POCO，請使用`Configuration` API 的[物件圖形](xref:fundamentals/configuration/index#bind-to-an-object-graph)系結功能。 下列程式碼會系結至`MovieSettings`自訂 POCO 並`ServiceApiKey`存取屬性值：

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

`Movies:ConnectionString`和`Movies:ServiceApiKey`密碼會對應至中`MovieSettings`的個別屬性：

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a>使用秘密取代字串

以純文字儲存密碼並不安全。 例如，儲存在*appsettings*中的資料庫連接字串可能包含指定使用者的密碼：

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

更安全的方法是將密碼儲存為秘密。 例如：

```dotnetcli
dotnet user-secrets set "DbPassword" "pass123"
```

從 appsettings `Password`的連接字串中移除索引鍵/值*appsettings.json*組。 例如：

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

您可以在<xref:System.Data.SqlClient.SqlConnectionStringBuilder>物件的<xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password%2A>屬性上設定密碼的值，以完成連接字串：

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

## <a name="list-the-secrets"></a>列出秘密

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

從 *.csproj*檔案所在的目錄執行下列命令：

```dotnetcli
dotnet user-secrets list
```

即會出現下列輸出：

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

在上述範例中，索引鍵名稱中的冒號代表在*私密金鑰內的物件階層。*

## <a name="remove-a-single-secret"></a>移除單一秘密

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

從 *.csproj*檔案所在的目錄執行下列命令：

```dotnetcli
dotnet user-secrets remove "Movies:ConnectionString"
```

應用程式的私密金鑰*json*檔案已修改，以移除與`MoviesConnectionString`金鑰相關聯的機碼值組：

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

執行`dotnet user-secrets list`會顯示下列訊息：

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a>移除所有秘密

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

從 *.csproj*檔案所在的目錄執行下列命令：

```dotnetcli
dotnet user-secrets clear
```

應用程式的所有使用者秘密都已從*密碼 json*檔案中刪除：

```json
{}
```

執行`dotnet user-secrets list`會顯示下列訊息：

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a>其他資源

* 如需從 IIS 存取秘密管理員的相關資訊，請參閱[此問題](https://github.com/dotnet/AspNetCore.Docs/issues/16328)。
* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>

::: moniker-end