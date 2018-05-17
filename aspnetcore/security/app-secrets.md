---
title: 安全存放裝置的開發工作中 ASP.NET Core 應用程式密碼
author: rick-anderson
description: 示範如何在開發期間安全地儲存密碼
manager: wpickett
ms.author: riande
ms.date: 09/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/app-secrets
ms.openlocfilehash: a268fd76a303dc1185b451e4f678fc2fe761e80a
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2018
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a>安全存放裝置的開發工作中 ASP.NET Core 應用程式密碼

由[Rick Anderson](https://twitter.com/RickAndMSFT)，[奧 Roth](https://github.com/danroth27)，和[Scott Addie](https://scottaddie.com) 

本文示範如何使用密碼管理員工具在開發程式碼保持機密資料。 最重要的一點是，始終不該將密碼或其他機密資料儲存在原始程式碼中，也不該在開發和測試模式中使用實際執行的密碼。 您可以改用[組態](xref:fundamentals/configuration/index)系統來讀取這些環境變數中的值，或那些使用密碼管理員工具所儲存的值。 密碼管理員工具有助於避免將機密資料簽入原始檔控制。 而[組態](xref:fundamentals/configuration/index)系統則能

密碼管理員工具只能用於開發。 您可以使用[Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) 組態提供者來保護 Azure 測試與正式環境的機密資料。 如需詳細資訊請參閱[Azure Key Vault 組態提供者](xref:security/key-vault-configuration)。

## <a name="environment-variables"></a>環境變數

若要避免將應用程式密碼儲存在程式碼或本機的組態檔中，可以將密碼儲存在環境變數中。 您可以先將 [組態](xref:fundamentals/configuration/index) 架構設定為透過呼叫 `AddEnvironmentVariables` 來讀取環境變數中的值。 接著便能使用環境變數，來為所有先前已指定的組態來源複寫組態值。

例如，當您透過個別使用者帳戶建立新的 ASP.NET Core web 應用程式時，系統會將預設的連接字串`DefaultConnection`新增至專案裡具有索引鍵的*appsettings.json*檔案中。 預設的連接字串會使用 LocalDB，而 LocalDB 會在使用者模式下執行，且不需要密碼。 當您將應用程式部署到測試或正式伺服器時，就可以覆寫預設連接字串`DefaultConnection`的值，設定成包含測試或正式執行資料庫的連接字串（潛在機密的認證）的伺服器環境變數。

>[!WARNING]
> 環境變數通常會以純文字儲存，不會加密。 如果電腦或處理序遭到入侵，不受信任方便能存取環境變數。 因此可能需要其他措施以避免洩露使用者機密資料。

## <a name="secret-manager"></a>密碼管理員

密碼管理員工具會將開發工作用的敏感性資料儲存在您的專案樹狀結構之外。 密碼管理員工具是一種專案的工具，可用來將.NET Core 專案密碼儲存在開發期間。 使用密碼管理員 工具中，您可以將應用程式密碼與特定的專案產生關聯，並共用跨多個專案。

>[!WARNING]
> 密碼管理員工具不會加密預存機密資料，並不會被視為受信任存放區。 它是僅限開發用途。 索引鍵和值會儲存在使用者設定檔的目錄中的 JSON 組態檔。

## <a name="installing-the-secret-manager-tool"></a>安裝密碼管理員工具

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

以滑鼠右鍵按一下方案總管 中的專案，然後選取**編輯\<project_name\>.csproj**從內容功能表。 將反白顯示的行加入 *.csproj*檔案，並將儲存到還原相關聯的 NuGet 套件：

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-before.csproj?highlight=10)]

以滑鼠右鍵按一下方案總管 中的專案，然後選取**管理使用者密碼**從內容功能表。 此動作將於 *.csproj*檔案中，將新的`UserSecretsId`節點新增至`PropertyGroup`，如下列範例中反白處所示：

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-after.csproj?highlight=4)]

儲存已修改 *.csproj*檔案也會開啟`secrets.json`文字編輯器中的檔案。 使用以下列程式碼取代`secrets.json`的內容：

```json
{
    "MySecret": "ValueOfMySecret"
}
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

新增`Microsoft.Extensions.SecretManager.Tools`至 *.csproj*檔，然後執行[dotnet 還原](/dotnet/core/tools/dotnet-restore)。 若要安裝命令列使用的密碼管理員工具，您可以使用相同的步驟。

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-before.csproj?highlight=10)]

測試密碼管理員工具執行下列命令：

```console
dotnet user-secrets -h
```

密碼管理員工具會顯示使用方式、選項和命令的說明。

> [!NOTE]
> 您必須在相同的目錄 *.csproj*檔案來執行工具中定義 *.csproj*檔案的`DotNetCliToolReference`節點。

密碼管理員工具會依據儲存在您的使用者設定檔中的專案特定的組態設定。 若要使用使用者密碼，必須指定專案`UserSecretsId`值在其 *.csproj*檔案。 值`UserSecretsId`是任意的但通常獨有的專案。 開發人員通常會產生的 GUID `UserSecretsId`。

新增`UserSecretsId`中的專案 *.csproj*檔案：

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-after.csproj?highlight=4)]

您可以使用密碼管理員工具來設定密碼。 例如，在命令視窗從專案目錄中，輸入下列內容：

```console
dotnet user-secrets set MySecret ValueOfMySecret
```

您可以執行密碼管理員工具，從其他目錄，但您必須使用`--project`傳遞的路徑中的選項 *.csproj*檔案：

```console
dotnet user-secrets set MySecret ValueOfMySecret --project c:\work\WebApp1\src\webapp1
```

您也可以使用密碼管理員工具來列出、 移除，並清除應用程式密碼。

---

## <a name="accessing-user-secrets-via-configuration"></a>透過設定存取的使用者密碼

您可以透過組態系統來存取密碼管理員的密碼。 新增`Microsoft.Extensions.Configuration.UserSecrets`封裝及執行[dotnet 還原](/dotnet/core/tools/dotnet-restore)。

將使用者密碼設定來源加入`Startup`方法：

[!code-csharp[](app-secrets/sample/UserSecrets/Startup.cs?highlight=16-19)]

您可以透過組態 API 來存取使用者密碼：

[!code-csharp[](app-secrets/sample/UserSecrets/Startup.cs?highlight=26-29)]

## <a name="how-the-secret-manager-tool-works"></a>密碼管理員工具的運作方式

密碼管理員工具會將實作的詳細資料加以抽象，包括各值儲存的位置和方式。 不需要知道這些實作細節也可以使用此工具。 在目前版本中，這些值會使用[JSON](http://json.org/)組態檔儲存在使用者設定檔的目錄中：

* Windows：`%APPDATA%\microsoft\UserSecrets\<userSecretsId>\secrets.json`

* Linux: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`

* macOS: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`

上述路經中，其`userSecretsId`值是來自 *.csproj*檔案。

您不應撰寫程式碼所依賴這些實作細節，可能會變更位置或使用密碼管理員 工具中，儲存的資料格式。 例如，密碼值是目前*未*加密，但未來可能會。

## <a name="additional-resources"></a>其他資源

* [組態](xref:fundamentals/configuration/index)
