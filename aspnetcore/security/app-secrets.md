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
ms.openlocfilehash: 280819a6a0afb72311f0d50f7d3b83a942e9fcc3
ms.sourcegitcommit: e3b1726cc04e80dc28464c35259edbd3bc39a438
ms.translationtype: MT
ms.contentlocale: zh-TW

ms.lasthandoff: 10/12/2017
---
# <a name="safe-storage-of-app-secrets-during-development-in-aspnet-core"></a>安全儲存體的 ASP.NET Core 在開發期間的應用程式密碼

由[Rick Anderson](https://twitter.com/RickAndMSFT)，[奧 Roth](https://github.com/danroth27)，和[Scott Addie](https://scottaddie.com) 

本文件示範如何使用密碼管理員工具在開發程式碼保持機密資料。 您應該永遠不會儲存密碼或其他機密資料來源的程式碼，而您不應該在開發和測試模式中使用實際執行的密碼最重要的一點。 您可以改為使用[組態](../fundamentals/configuration.md)系統環境變數中讀取這些值，或使用密碼管理員儲存的值從工具。 密碼管理員工具可協助防止機密資料被簽入原始檔控制。 [組態](../fundamentals/configuration.md)系統可以讀取儲存在本文中所描述的密碼管理員工具與密碼。

密碼管理員工具只能用於開發。 您可以保護 Azure 測試與實際的機密資料以[Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/)組態提供者。 請參閱[Azure 金鑰保存庫的組態提供者](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration)如需詳細資訊。

## <a name="environment-variables"></a>環境變數

若要避免在程式碼，或在本機的組態檔中儲存應用程式密碼，您可以將密碼儲存在環境變數中。 您可以設定[組態](../fundamentals/configuration.md)架構，以讀取環境變數中的值，藉由呼叫`AddEnvironmentVariables`。 然後，您可以使用環境變數覆寫所有先前指定的設定來源的組態值。

例如，如果您建立新的 ASP.NET Core web 應用程式與個別使用者帳戶時，它會將新增的預設連接字串*appsettings.json*專案具有索引鍵中的檔案`DefaultConnection`。 預設的連接字串是安裝程式使用 LocalDB，但在使用者模式下執行，而不需要密碼。 當您部署到測試或實際執行伺服器應用程式時，您可以覆寫`DefaultConnection`機碼值與包含測試或實際執行資料庫的連接字串 （潛在機密的認證） 的環境變數設定伺服器。

>[!WARNING]
> 環境變數通常會以純文字儲存，不會加密。 如果電腦或處理序遭到入侵，然後可以在不受信任的合作對象來存取環境變數。 可能仍然需要其他措施以避免洩露使用者密碼。

## <a name="secret-manager"></a>密碼管理員

密碼管理員工具會儲存為您專案的樹狀目錄外部的開發工作的敏感性資料。 密碼管理員工具是一種專案的工具，可以用來儲存的秘密資訊[.NET Core](https://www.microsoft.com/net/core)在開發期間的專案。 使用密碼管理員 工具中，您可以將應用程式密碼與特定的專案產生關聯，並共用跨多個專案。

>[!WARNING]
> 密碼管理員工具不會加密預存機密資料，並不會被視為受信任存放區。 它是僅限開發用途。 索引鍵和值會儲存在使用者設定檔的目錄中的 JSON 組態檔。

## <a name="installing-the-secret-manager-tool"></a>安裝密碼管理員工具

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

以滑鼠右鍵按一下方案總管 中的專案，然後選取**編輯\<project_name\>.csproj**從內容功能表。 將反白顯示的行加入*.csproj*檔案，並將儲存到還原相關聯的 NuGet 套件：

[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets-before.csproj?highlight=10)]

以滑鼠右鍵按一下方案總管 中的專案，然後選取**管理使用者密碼**從內容功能表。 此筆勢加入新`UserSecretsId`節點內`PropertyGroup`的*.csproj*檔案，以反白顯示在下列範例中：

[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets-after.csproj?highlight=4)]

儲存已修改*.csproj*檔案也會開啟`secrets.json`文字編輯器中的檔案。 取代內容`secrets.json`以下列程式碼檔案：

```json
{
    "MySecret": "ValueOfMySecret"
}
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

新增`Microsoft.Extensions.SecretManager.Tools`至*.csproj*檔，然後執行`dotnet restore`。 若要安裝的命令列使用的密碼管理員工具，您可以使用相同的步驟。

[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets-before.csproj?highlight=10)]

測試密碼管理員工具執行下列命令：

```console
dotnet user-secrets -h
```

密碼管理員工具會顯示使用方式、 選項和命令的說明。

> [!NOTE]
> 您必須在相同的目錄*.csproj*檔案來執行工具中定義*.csproj*檔案的`DotNetCliToolReference`節點。

密碼管理員工具會依據儲存在您的使用者設定檔中的專案特定的組態設定。 若要使用使用者密碼，必須指定專案`UserSecretsId`值在其*.csproj*檔案。 值`UserSecretsId`是任意的但通常獨有的專案。 開發人員通常會產生的 GUID `UserSecretsId`。

新增`UserSecretsId`中的專案*.csproj*檔案：

[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets-after.csproj?highlight=4)]

您可以使用密碼管理員工具來設定密碼。 例如，在命令視窗從專案目錄中，輸入下列內容：

```console
dotnet user-secrets set MySecret ValueOfMySecret
```

您可以執行密碼管理員工具，從其他目錄，但您必須使用`--project`傳遞的路徑中的選項*.csproj*檔案：
 
```console
dotnet user-secrets set MySecret ValueOfMySecret --project c:\work\WebApp1\src\webapp1
```

您也可以使用密碼管理員工具來列出、 移除，並清除應用程式密碼。

-----

## <a name="accessing-user-secrets-via-configuration"></a>透過設定存取的使用者密碼

透過組態系統存取的密碼管理員密碼。 新增`Microsoft.Extensions.Configuration.UserSecrets`封裝及執行`dotnet restore`。

將使用者密碼設定來源加入`Startup`方法：

[!code-csharp[Main](app-secrets/sample/UserSecrets/Startup.cs?highlight=16-19)]

您可以存取透過設定應用程式開發介面的使用者密碼：

[!code-csharp[Main](app-secrets/sample/UserSecrets/Startup.cs?highlight=26-29)]

## <a name="how-the-secret-manager-tool-works"></a>密碼管理員工具的運作方式

密碼管理員工具抽象化實作詳細資料，例如值儲存的位置和方式。 您可以使用此工具，而不需要知道這些實作細節。 在目前版本中，值會儲存在[JSON](http://json.org/)使用者設定檔的目錄中的組態檔：

* Windows:`%APPDATA%\microsoft\UserSecrets\<userSecretsId>\secrets.json`

* Linux:`~/.microsoft/usersecrets/<userSecretsId>/secrets.json`

* Mac:`~/.microsoft/usersecrets/<userSecretsId>/secrets.json`

值`userSecretsId`中指定的值是來自*.csproj*檔案。

您不應該撰寫程式碼所依賴這些實作細節，可能會變更位置或使用密碼管理員 工具中，儲存的資料格式。 例如，密碼值是目前*不*加密，但是也可能是吧。

## <a name="additional-resources"></a>其他資源

* [組態](../fundamentals/configuration.md)

