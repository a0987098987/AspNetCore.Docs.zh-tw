---
title: ASP.NET核心開發中應用秘密的安全儲存
author: rick-anderson
description: 瞭解如何在開發ASP.NET核心應用期間將敏感資訊存儲和檢索為應用機密。
ms.author: scaddie
ms.custom: mvc
ms.date: 4/20/2020
uid: security/app-secrets
ms.openlocfilehash: c62c5e59ad0a72506fb72bda82aa821a4f1719c8
ms.sourcegitcommit: c9d1208e86160615b2d914cce74a839ae41297a8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/22/2020
ms.locfileid: "81791597"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a>ASP.NET核心開發中應用秘密的安全儲存

::: moniker range=">= aspnetcore-3.0"

由[里克·安德森](https://twitter.com/RickAndMSFT)、[柯克·拉金](https://twitter.com/serpent5)、[丹尼爾·羅斯](https://github.com/danroth27)和[斯科特·阿迪](https://github.com/scottaddie)

[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/app-secrets/samples)([如何下載](xref:index#how-to-download-a-sample))

本文件介紹在開發計算機上開發ASP.NET酷睿應用期間存儲和檢索敏感數據的技術。 切勿在原始碼中儲存密碼或其他敏感資料。 生產機密不應用於開發或測試。 不應將機密隨應用一起部署。 相反,應通過受控方式(如環境變數、Azure 密鑰保管庫等)在生產環境中提供機密。您可以使用[Azure 密鑰保管庫配置提供程式](xref:security/key-vault-configuration)儲存和保護 Azure 測試和生產機密。

## <a name="environment-variables"></a>環境變數

環境變數用於避免在代碼或本地配置檔中存儲應用機密。 環境變數覆蓋所有以前指定的配置源的配置值。

請考慮一個ASP.NET核心 Web 應用,其中啟用**了單個使用者帳戶**安全性。 預設資料庫連接字串包含在項目的*appsettings.json*檔中,其中包含金`DefaultConnection`鑰 。 預設連接字串用於 LocalDB,該字串在使用者模式下運行,不需要密碼。 在應用部署期間,`DefaultConnection`可以使用環境變數的值重寫鍵值。 環境變數可能將完整的連接字串存儲為具有敏感認證。

> [!WARNING]
> 環境變數通常以純、未加密的文本存儲。 如果電腦或進程遭到破壞,則不受信任的方可以訪問環境變數。 可能需要採取其他措施防止泄露用戶機密。

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="secret-manager"></a>秘密經理

在開發ASP.NET核心專案期間,秘密管理器工具存儲敏感數據。 在此上下文中,一段敏感數據是應用機密。 應用機密存儲在與專案樹不同的位置。 應用機密與特定專案關聯或跨多個項目共用。 應用機密不會簽入原始程式碼管理。

> [!WARNING]
> 秘密管理員工具不加密儲存的秘密,不應被視爲受信任的儲存。 它僅用於開發目的。 鍵和值存儲在使用者配置檔目錄中的 JSON 配置檔中。

## <a name="how-the-secret-manager-tool-works"></a>秘密管理員工具的工作原理

"秘密管理員"工具會抽象出實現詳細資訊,例如值的存儲位置和方式。 您可以在不知道這些實現詳細資訊的情況下使用該工具。 這些值儲存在本地電腦上的受系統保護的使用者設定檔案資料夾中的 JSON 設定檔中:

# <a name="windows"></a>[Windows](#tab/windows)

檔案系統路徑:

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="linux--macos"></a>[Linux / macOS](#tab/linux+macos)

檔案系統路徑:

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

在前面的檔路徑中,替換為`<user_secrets_id>``UserSecretsId` *.csproj*檔中指定的值。

不要編寫依賴於使用機密管理員工具保存的資料的位置或格式的代碼。 這些實現詳細資訊可能會更改。 例如,機密值未加密,但將來可能已加密。

## <a name="enable-secret-storage"></a>啟用機密儲存

機密管理員「工具可對存儲在使用者設定檔中的特定於專案的設定設定進行操作。

機密管理員工具包括 .NET Core SDK 3.0.100`init`或更高版本中 的命令。 要使用使用者機密,請執行項目目錄中的以下指令:

```dotnetcli
dotnet user-secrets init
```

前面的命令在`UserSecretsId`*.csproj*檔中添加`PropertyGroup`一個 元素。 預設情況下,的內部`UserSecretsId`文本是 GUID。 內部文本是任意的,但對於專案是唯一的。

[!code-xml[](app-secrets/samples/3.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

在 Visual Studio 中,右鍵單擊解決方案資源管理器中的專案,然後從上下文菜單中選擇 **「管理使用者機密**」。 此手勢將一`UserSecretsId`個使用 GUID 填充的元素添加到 *.csproj*檔中。

## <a name="set-a-secret"></a>設定機密

定義由鍵及其值組成的應用機密。 機密與項目`UserSecretsId`的值相關聯。 例如,從*存在 .csproj*檔的目錄中執行以下命令:

```dotnetcli
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

在前面的示例中,冒號表示`Movies`是`ServiceApiKey`具有 屬性的物件文本。

機密管理器工具也可以從其他目錄使用。 使用`--project`選項提供*存在 .csproj*檔案的檔案系統路徑。 例如：

```dotnetcli
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

### <a name="json-structure-flattening-in-visual-studio"></a>視覺工作室中的 JSON 結構拼合

可視化工作室的 **「管理使用者機密**」手勢將在文字編輯器中打開*一個機密.json*檔。 將*機密*內容替換為要存儲的鍵值對。 例如：

```json
{
  "Movies": {
    "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
    "ServiceApiKey": "12345"
  }
}
```

通過`dotnet user-secrets remove``dotnet user-secrets set`或 進行修改後,JSON 結構將展平。 例如,運行`dotnet user-secrets remove "Movies:ConnectionString"`將`Movies`摺疊 物件文本。 變更後的檔案類似於以下內容:

```json
{
  "Movies:ServiceApiKey": "12345"
}
```

## <a name="set-multiple-secrets"></a>設定多個機密

通過將 JSON`set`管道到 命令,可以設置一批機密。 在下面的範例中 *,input.json*檔的內容被傳`set`送到命令 。

# <a name="windows"></a>[Windows](#tab/windows)

開啟命令外殼,並執行以下命令:

  ```dotnetcli
  type .\input.json | dotnet user-secrets set
  ```

# <a name="linux--macos"></a>[Linux / macOS](#tab/linux+macos)

開啟命令外殼,並執行以下命令:

  ```dotnetcli
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a>存取機密

[ASP.NET核心配置 API](xref:fundamentals/configuration/index)提供對機密管理器機密的訪問。

當使用者調用<xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder%2A>使用預先設定預設值初始化主機的新實例時,將在開發模式下自動添加使用者機密配置來源。 `CreateDefaultBuilder`當<xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A><xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName>為<xref:Microsoft.Extensions.Hosting.EnvironmentName.Development>時呼叫 :

[!code-csharp[](app-secrets/samples/3.x/UserSecrets/Program.cs?name=snippet_CreateHostBuilder&highlight=2)]

未`CreateDefaultBuilder`調用時,請通過調<xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A>用 顯式添加使用者機密配置源。 僅在`AddUserSecrets`應用在開發環境中運行時呼叫,如以下範例所示:

[!code-csharp[](app-secrets/samples/3.x/UserSecrets/Program2.cs?name=snippet_Host&highlight=6-9)]

可透過`Configuration`API 檢索使用者機密:

[!code-csharp[](app-secrets/samples/3.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

## <a name="map-secrets-to-a-poco"></a>將機密映射到 POCO

將整個物件文字映射到 POCO(具有屬性的簡單 .NET 類)可用於聚合相關屬性。

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

要將上述機密映射到 POCO,請`Configuration`使用 API[的物件圖形綁定](xref:fundamentals/configuration/index#bind-to-an-object-graph)功能。 以下代碼繫結為自訂`MovieSettings`POCO`ServiceApiKey`並造訪 屬性值:

[!code-csharp[](app-secrets/samples/3.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

與`Movies:ConnectionString``Movies:ServiceApiKey`機密映射到`MovieSettings`的屬性:

[!code-csharp[](app-secrets/samples/3.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a>字串取代與機密

以純文本形式儲存密碼不安全。 例如,儲存在*appsettings.json*中的資料庫連接字串可能包含指定使用者的密碼:

[!code-json[](app-secrets/samples/3.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

更安全的方法是將密碼存儲為機密。 例如：

```dotnetcli
dotnet user-secrets set "DbPassword" "pass123"
```

從`Password`*appsettings.json*中的連接字串中刪除鍵值對。 例如：

[!code-json[](app-secrets/samples/3.x/UserSecrets/appsettings.json?highlight=3)]

可以在<xref:System.Data.SqlClient.SqlConnectionStringBuilder><xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password%2A>物件屬性上設定機密的值以完成連接字串:

[!code-csharp[](app-secrets/samples/3.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

## <a name="list-the-secrets"></a>列出機密

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

從*存在 .csproj*檔案的目錄執行以下指令:

```dotnetcli
dotnet user-secrets list
```

即會出現下列輸出：

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

在前面的範例中,鍵名稱中的冒號表示機密中的物件層次結構 *。*

## <a name="remove-a-single-secret"></a>移除單一機密

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

從*存在 .csproj*檔案的目錄執行以下指令:

```dotnetcli
dotnet user-secrets remove "Movies:ConnectionString"
```

套用的*機密.json*檔已修改以`MoviesConnectionString`刪除與 金鑰關聯的鍵值對:

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

`dotnet user-secrets list`顯示以下訊息:

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a>移除所有機密

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

從*存在 .csproj*檔案的目錄執行以下指令:

```dotnetcli
dotnet user-secrets clear
```

應用程式的所有使用者機密已從*機密.json*檔案中刪除:

```json
{}
```

執行`dotnet user-secrets list`顯示以下訊息:

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a>其他資源

* 有關從 IIS 存取機密管理員的資訊,請參閱[此問題](https://github.com/dotnet/AspNetCore.Docs/issues/16328)。
* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

由[里克·安德森](https://twitter.com/RickAndMSFT),[丹尼爾·羅斯](https://github.com/danroth27)和[斯科特·艾迪](https://github.com/scottaddie)

[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/app-secrets/samples)([如何下載](xref:index#how-to-download-a-sample))

本文件介紹在開發計算機上開發ASP.NET酷睿應用期間存儲和檢索敏感數據的技術。 切勿在原始碼中儲存密碼或其他敏感資料。 生產機密不應用於開發或測試。 不應將機密隨應用一起部署。 相反,應通過受控方式(如環境變數、Azure 密鑰保管庫等)在生產環境中提供機密。您可以使用[Azure 密鑰保管庫配置提供程式](xref:security/key-vault-configuration)儲存和保護 Azure 測試和生產機密。

## <a name="environment-variables"></a>環境變數

環境變數用於避免在代碼或本地配置檔中存儲應用機密。 環境變數覆蓋所有以前指定的配置源的配置值。

請考慮一個ASP.NET核心 Web 應用,其中啟用**了單個使用者帳戶**安全性。 預設資料庫連接字串包含在項目的*appsettings.json*檔中,其中包含金`DefaultConnection`鑰 。 預設連接字串用於 LocalDB,該字串在使用者模式下運行,不需要密碼。 在應用部署期間,`DefaultConnection`可以使用環境變數的值重寫鍵值。 環境變數可能將完整的連接字串存儲為具有敏感認證。

> [!WARNING]
> 環境變數通常以純、未加密的文本存儲。 如果電腦或進程遭到破壞,則不受信任的方可以訪問環境變數。 可能需要採取其他措施防止泄露用戶機密。

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="secret-manager"></a>秘密經理

在開發ASP.NET核心專案期間,秘密管理器工具存儲敏感數據。 在此上下文中,一段敏感數據是應用機密。 應用機密存儲在與專案樹不同的位置。 應用機密與特定專案關聯或跨多個項目共用。 應用機密不會簽入原始程式碼管理。

> [!WARNING]
> 秘密管理員工具不加密儲存的秘密,不應被視爲受信任的儲存。 它僅用於開發目的。 鍵和值存儲在使用者配置檔目錄中的 JSON 配置檔中。

## <a name="how-the-secret-manager-tool-works"></a>秘密管理員工具的工作原理

"秘密管理員"工具會抽象出實現詳細資訊,例如值的存儲位置和方式。 您可以在不知道這些實現詳細資訊的情況下使用該工具。 這些值儲存在本地電腦上的受系統保護的使用者設定檔案資料夾中的 JSON 設定檔中:

# <a name="windows"></a>[Windows](#tab/windows)

檔案系統路徑:

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="linux--macos"></a>[Linux / macOS](#tab/linux+macos)

檔案系統路徑:

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

在前面的檔路徑中,替換為`<user_secrets_id>``UserSecretsId` *.csproj*檔中指定的值。

不要編寫依賴於使用機密管理員工具保存的資料的位置或格式的代碼。 這些實現詳細資訊可能會更改。 例如,機密值未加密,但將來可能已加密。

## <a name="enable-secret-storage"></a>啟用機密儲存

機密管理員「工具可對存儲在使用者設定檔中的特定於專案的設定設定進行操作。

要使用使用者機密,請定義`UserSecretsId``PropertyGroup` *.csproj*檔中的元素。 的內部`UserSecretsId`文本是任意的,但對於專案是唯一的。 開發人員通常為生成`UserSecretsId`GUID。

[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

> [!TIP]
> 在 Visual Studio 中,右鍵單擊解決方案資源管理器中的專案,然後從上下文菜單中選擇 **「管理使用者機密**」。 此手勢將一`UserSecretsId`個使用 GUID 填充的元素添加到 *.csproj*檔中。

## <a name="set-a-secret"></a>設定機密

定義由鍵及其值組成的應用機密。 機密與項目`UserSecretsId`的值相關聯。 例如,從*存在 .csproj*檔的目錄中執行以下命令:

```dotnetcli
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

在前面的示例中,冒號表示`Movies`是`ServiceApiKey`具有 屬性的物件文本。

機密管理器工具也可以從其他目錄使用。 使用`--project`選項提供*存在 .csproj*檔案的檔案系統路徑。 例如：

```dotnetcli
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

### <a name="json-structure-flattening-in-visual-studio"></a>視覺工作室中的 JSON 結構拼合

可視化工作室的 **「管理使用者機密**」手勢將在文字編輯器中打開*一個機密.json*檔。 將*機密*內容替換為要存儲的鍵值對。 例如：

```json
{
  "Movies": {
    "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
    "ServiceApiKey": "12345"
  }
}
```

通過`dotnet user-secrets remove``dotnet user-secrets set`或 進行修改後,JSON 結構將展平。 例如,運行`dotnet user-secrets remove "Movies:ConnectionString"`將`Movies`摺疊 物件文本。 變更後的檔案類似於以下內容:

```json
{
  "Movies:ServiceApiKey": "12345"
}
```

## <a name="set-multiple-secrets"></a>設定多個機密

通過將 JSON`set`管道到 命令,可以設置一批機密。 在下面的範例中 *,input.json*檔的內容被傳`set`送到命令 。

# <a name="windows"></a>[Windows](#tab/windows)

開啟命令外殼,並執行以下命令:

  ```dotnetcli
  type .\input.json | dotnet user-secrets set
  ```

# <a name="linux--macos"></a>[Linux / macOS](#tab/linux+macos)

開啟命令外殼,並執行以下命令:

  ```dotnetcli
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a>存取機密

[ASP.NET核心配置 API](xref:fundamentals/configuration/index)提供對機密管理器機密的訪問。

如果項目的目標是 .NET 框架,請安裝[Microsoft.擴展.配置.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet 包。

在 ASP.NET Core 2.0 或更高版本中,<xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>當專案調用 使用預配置預設值初始化主機的新實例時,使用者機密配置源將自動在開發模式下添加。 `CreateDefaultBuilder`當<xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A><xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName>為<xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>時呼叫 :

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

未`CreateDefaultBuilder`調用時,通過在<xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A>`Startup`建構函數中調用顯式添加使用者機密配置源。 僅在`AddUserSecrets`應用在開發環境中運行時呼叫,如以下範例所示:

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_StartupConstructor&highlight=12)]

可透過`Configuration`API 檢索使用者機密:

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

## <a name="map-secrets-to-a-poco"></a>將機密映射到 POCO

將整個物件文字映射到 POCO(具有屬性的簡單 .NET 類)可用於聚合相關屬性。

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

要將上述機密映射到 POCO,請`Configuration`使用 API[的物件圖形綁定](xref:fundamentals/configuration/index#bind-to-an-object-graph)功能。 以下代碼繫結為自訂`MovieSettings`POCO`ServiceApiKey`並造訪 屬性值:

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

與`Movies:ConnectionString``Movies:ServiceApiKey`機密映射到`MovieSettings`的屬性:

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a>字串取代與機密

以純文本形式儲存密碼不安全。 例如,儲存在*appsettings.json*中的資料庫連接字串可能包含指定使用者的密碼:

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

更安全的方法是將密碼存儲為機密。 例如：

```dotnetcli
dotnet user-secrets set "DbPassword" "pass123"
```

從`Password`*appsettings.json*中的連接字串中刪除鍵值對。 例如：

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

可以在<xref:System.Data.SqlClient.SqlConnectionStringBuilder><xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password%2A>物件屬性上設定機密的值以完成連接字串:

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

## <a name="list-the-secrets"></a>列出機密

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

從*存在 .csproj*檔案的目錄執行以下指令:

```dotnetcli
dotnet user-secrets list
```

即會出現下列輸出：

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

在前面的範例中,鍵名稱中的冒號表示機密中的物件層次結構 *。*

## <a name="remove-a-single-secret"></a>移除單一機密

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

從*存在 .csproj*檔案的目錄執行以下指令:

```dotnetcli
dotnet user-secrets remove "Movies:ConnectionString"
```

套用的*機密.json*檔已修改以`MoviesConnectionString`刪除與 金鑰關聯的鍵值對:

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

執行`dotnet user-secrets list`顯示以下訊息:

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a>移除所有機密

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

從*存在 .csproj*檔案的目錄執行以下指令:

```dotnetcli
dotnet user-secrets clear
```

應用程式的所有使用者機密已從*機密.json*檔案中刪除:

```json
{}
```

執行`dotnet user-secrets list`顯示以下訊息:

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a>其他資源

* 有關從 IIS 存取機密管理員的資訊,請參閱[此問題](https://github.com/dotnet/AspNetCore.Docs/issues/16328)。
* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>

::: moniker-end