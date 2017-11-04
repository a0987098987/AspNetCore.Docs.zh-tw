---
title: "安全的儲存 ASP.NET Core 在開發期間的應用程式密碼"
author: rick-anderson
description: "示範如何在開發期間安全地儲存密碼"
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 09/15/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/app-secrets
ms.openlocfilehash: e112cc5ef9cba5aff6470ce4b9b1091a3c2b2600
ms.sourcegitcommit: f1271b218d7dfdc806ec8f411c81f3750130463d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/15/2017
---
# <a name="safe-storage-of-app-secrets-during-development-in-aspnet-core"></a>安全的儲存 ASP.NET Core 在開發期間的應用程式密碼

<a name=security-app-secrets></a>

由[Rick Anderson](https://twitter.com/RickAndMSFT)，[奧 Roth](https://github.com/danroth27)，和[Scott Addie](https://scottaddie.com) 

本文件示範如何使用密碼管理員工具在開發程式碼保持機密資料。 您永遠不該儲存密碼或其他機密資料來源的程式碼，最重要的一點是您不應該在開發和測試模式中使用實際執行的密碼。您可以改使用[組態](../fundamentals/configuration.md)的環境變數來讀取這些值，或使用密碼管理員工具（Secret Manager）。密碼管理員工具可協助防止機密資料被簽入原始檔控制中。 [組態](../fundamentals/configuration.md)可以讀取儲存在本文中所描述的密碼管理員工具的機密資料。

密碼管理員工具（Secret Manager）只能用於開發。您可以使用[Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/)組態提供者，藉此保護 Azure 測試與正式環境的機密資料。如需詳細資訊請參閱[Azure 金鑰保存庫的組態提供者](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration)。

## <a name="environment-variables"></a>環境變數

若要避免在程式碼，或在本機的組態檔中儲存應用程式密碼，您可以將密碼儲存在環境變數中。您可以設定[組態](../fundamentals/configuration.md)架構，藉由呼叫`AddEnvironmentVariables`以讀取環境變數中的值。然後，您可以使用環境變數覆寫所有先前指定的組態值。

例如，如果您建立新的 ASP.NET Core web 應用程式與個別使用者帳戶時，它會新增的預設連接字串`DefaultConnection`至專案裡具有索引鍵的*appsettings.json*檔案中。預設的連接字串是使用 LocalDB，LocalDB 在使用者模式下執行不需要密碼。當您部署應用程式到測試或正式伺服器時，您可以覆寫預設連接字串`DefaultConnection`的值，設定成包含測試或正式執行資料庫的連接字串（潛在機密的認證）的伺服器環境變數。

>[!WARNING]
> 環境變數通常會以純文字儲存，不會加密。如果電腦或處理序遭到入侵，將被不受信任的合作對象來存取環境變數。因此可能需要其他措施以避免洩露使用者機密資料。

## <a name="secret-manager"></a>密碼管理員

密碼管理員工具會將敏感性資料儲存在您開發中專案的外部目錄。密碼管理員工具是一種專案的工具，可以用來儲存[.NET Core](https://www.microsoft.com/net/core)專案在開發期間的秘密資訊。使用密碼管理員工具，您可以將應用程式密碼與特定的專案產生關聯，並跨多個專案共用。

>[!WARNING]
> 密碼管理員工具不會加密預存機密資料，並不會被視為受信任存放區。它是僅限開發用途。索引鍵和值會儲存在使用者設定檔的目錄中的 JSON 組態檔。

## <a name="installing-the-secret-manager-tool"></a>安裝密碼管理員工具

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

以滑鼠右鍵按一下方案總管 中的專案，然後選取**編輯\<project_name\>.csproj**從內容功能表。 將反白顯示的行加入*.csproj*檔案，並將儲存到還原相關聯的 NuGet 套件：

[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets-before.csproj?highlight=10)]

以滑鼠右鍵按一下方案總管 中的專案，然後從內容功能表選取**管理使用者密碼**。此動作將於*.csproj*檔案中，加入新的`UserSecretsId`節點至`PropertyGroup`中，以反白顯示在下列範例中：

[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets-after.csproj?highlight=4)]

儲存已修改*.csproj*檔案也會開啟`secrets.json`文字編輯器中的檔案。 使用以下列程式碼取代`secrets.json`的內容：

```json
{
    "MySecret": "ValueOfMySecret"
}
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

新增`Microsoft.Extensions.SecretManager.Tools`至*.csproj*檔，然後執行`dotnet restore`。若要安裝命令列使用的密碼管理員工具，您可以使用相同的步驟。

[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets-before.csproj?highlight=10)]

測試密碼管理員工具執行下列命令：

```console
dotnet user-secrets -h
```

密碼管理員工具會顯示使用方式、選項和命令的說明。

> [!NOTE]
> 執行工具時，您必須與定義`DotNetCliToolReference`節點的*.csproj*檔案相同的目錄中。

密碼管理員工具會依據儲存在您的使用者設定檔中的專案特定的組態設定。若要使用使用者密碼，必須在專案的*.csproj*檔案中指定`UserSecretsId`值。`UserSecretsId`是任意的但通常獨有的專案編號。開發人員通常會產生的 GUID `UserSecretsId`。

在專案的*.csproj*檔案中，新增`UserSecretsId`：

[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets-after.csproj?highlight=4)]

您可以使用密碼管理員工具來設定密碼。 例如，在命令視窗從專案目錄中，輸入下列內容：

```console
dotnet user-secrets set MySecret ValueOfMySecret
```

您可以從其他目錄中執行密碼管理員工具，但您必須使用`--project`來傳遞*.csproj*檔案的路徑：
 
```console
dotnet user-secrets set MySecret ValueOfMySecret --project c:\work\WebApp1\src\webapp1
```

您也可以使用密碼管理員工具來列出、移除，並清除應用程式密碼。

-----

## <a name="accessing-user-secrets-via-configuration"></a>透過設定存取的使用者密碼

透過組態系統來存取密碼管理員的密碼。需先新增`Microsoft.Extensions.Configuration.UserSecrets`封裝及執行`dotnet restore`。

將使用者密碼設定來源加入`Startup`方法：

[!code-csharp[Main](app-secrets/sample/UserSecrets/Startup.cs?highlight=16-19)]

您可以透過應用程式組態設定 API 來設定使用者密碼：

[!code-csharp[Main](app-secrets/sample/UserSecrets/Startup.cs?highlight=26-29)]

## <a name="how-the-secret-manager-tool-works"></a>密碼管理員工具的運作方式

密碼管理員工具抽象化實作詳細資料，例如機密資訊儲存的位置和方式。您可以使用此工具，而不需要知道這些實作細節。在目前版本中，機密資訊會使用[JSON](http://json.org/)組態檔，並儲存在使用者設定檔的目錄中：

* Windows:`%APPDATA%\microsoft\UserSecrets\<userSecretsId>\secrets.json`

* Linux:`~/.microsoft/usersecrets/<userSecretsId>/secrets.json`

* Mac:`~/.microsoft/usersecrets/<userSecretsId>/secrets.json`

上述路經中，其`userSecretsId`值是來自*.csproj*檔案。

您不應該撰寫依賴這些實作細節的程式碼，這些路徑可能隨使用密碼管理員工具而變更位置或儲存的資料格式。例如，密碼值是目前是*不*加密，但未來可能會。

## <a name="additional-resources"></a>其他資源

* [組態](../fundamentals/configuration.md)
