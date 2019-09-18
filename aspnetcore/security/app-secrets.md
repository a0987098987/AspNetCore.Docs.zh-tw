---
title: 在 ASP.NET Core 中的開發中安全儲存應用程式秘密
author: rick-anderson
description: 瞭解如何在開發 ASP.NET Core 應用程式期間，將機密資訊儲存為應用程式秘密並加以取出。
ms.author: scaddie
ms.custom: mvc
ms.date: 03/13/2019
uid: security/app-secrets
ms.openlocfilehash: 0203a5737caf1af809b739d9e266a6971cd1523b
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2019
ms.locfileid: "71080708"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="30f89-103">在 ASP.NET Core 中的開發中安全儲存應用程式秘密</span><span class="sxs-lookup"><span data-stu-id="30f89-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="30f89-104">由[Rick Anderson](https://twitter.com/RickAndMSFT)、 [Daniel Roth](https://github.com/danroth27)和[Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="30f89-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="30f89-105">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/app-secrets/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="30f89-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/app-secrets/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="30f89-106">本檔說明在開發 ASP.NET Core 應用程式期間，儲存和取得敏感性資料的技術。</span><span class="sxs-lookup"><span data-stu-id="30f89-106">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="30f89-107">絕對不要將密碼或其他敏感性資料儲存在原始程式碼中。</span><span class="sxs-lookup"><span data-stu-id="30f89-107">Never store passwords or other sensitive data in source code.</span></span> <span data-ttu-id="30f89-108">生產秘密不應用於開發或測試。</span><span class="sxs-lookup"><span data-stu-id="30f89-108">Production secrets shouldn't be used for development or test.</span></span> <span data-ttu-id="30f89-109">您可以透過 [Azure Key Vault 設定提供者](xref:security/key-vault-configuration)儲存及保護 Azure 測試與生產祕密。</span><span class="sxs-lookup"><span data-stu-id="30f89-109">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="30f89-110">環境變數</span><span class="sxs-lookup"><span data-stu-id="30f89-110">Environment variables</span></span>

<span data-ttu-id="30f89-111">環境變數是用來避免在程式碼或本機設定檔案中儲存應用程式秘密。</span><span class="sxs-lookup"><span data-stu-id="30f89-111">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="30f89-112">環境變數會覆寫所有先前指定之設定來源的設定值。</span><span class="sxs-lookup"><span data-stu-id="30f89-112">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="30f89-113">藉由<xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> `Startup`在函式中呼叫來設定讀取環境變數值：</span><span class="sxs-lookup"><span data-stu-id="30f89-113">Configure the reading of environment variable values by calling <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=8)]

::: moniker-end

<span data-ttu-id="30f89-114">請考慮啟用**個別使用者帳戶**安全性的 ASP.NET Core web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="30f89-114">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="30f89-115">預設的資料庫連接字串會包含在專案的*appsettings*中，並具有索引鍵`DefaultConnection`。</span><span class="sxs-lookup"><span data-stu-id="30f89-115">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="30f89-116">預設連接字串適用于 LocalDB，它會在使用者模式中執行，而且不需要密碼。</span><span class="sxs-lookup"><span data-stu-id="30f89-116">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="30f89-117">在應用程式部署期間`DefaultConnection` ，可以使用環境變數的值來覆寫金鑰值。</span><span class="sxs-lookup"><span data-stu-id="30f89-117">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="30f89-118">環境變數可能會儲存具有敏感性認證的完整連接字串。</span><span class="sxs-lookup"><span data-stu-id="30f89-118">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="30f89-119">環境變數通常會儲存為純文字、未加密的文字。</span><span class="sxs-lookup"><span data-stu-id="30f89-119">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="30f89-120">如果電腦或進程遭到入侵，則不受信任的合作物件可以存取環境變數。</span><span class="sxs-lookup"><span data-stu-id="30f89-120">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="30f89-121">可能需要其他措施來防止洩漏使用者秘密。</span><span class="sxs-lookup"><span data-stu-id="30f89-121">Additional measures to prevent disclosure of user secrets may be required.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="secret-manager"></a><span data-ttu-id="30f89-122">秘密管理員</span><span class="sxs-lookup"><span data-stu-id="30f89-122">Secret Manager</span></span>

<span data-ttu-id="30f89-123">秘密管理員工具會在 ASP.NET Core 專案的開發期間儲存機密資料。</span><span class="sxs-lookup"><span data-stu-id="30f89-123">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="30f89-124">在此內容中，有一段敏感性資料是應用程式密碼。</span><span class="sxs-lookup"><span data-stu-id="30f89-124">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="30f89-125">應用程式密碼會儲存在專案樹狀結構的不同位置。</span><span class="sxs-lookup"><span data-stu-id="30f89-125">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="30f89-126">應用程式密碼會與特定專案建立關聯，或在數個專案之間共用。</span><span class="sxs-lookup"><span data-stu-id="30f89-126">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="30f89-127">應用程式秘密不會簽入原始檔控制中。</span><span class="sxs-lookup"><span data-stu-id="30f89-127">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="30f89-128">密碼管理員工具不會加密預存機密資料，並不會被視為受信任存放區。</span><span class="sxs-lookup"><span data-stu-id="30f89-128">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="30f89-129">它是僅限開發用途。</span><span class="sxs-lookup"><span data-stu-id="30f89-129">It's for development purposes only.</span></span> <span data-ttu-id="30f89-130">索引鍵和值會儲存在使用者設定檔的目錄中的 JSON 組態檔。</span><span class="sxs-lookup"><span data-stu-id="30f89-130">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="30f89-131">密碼管理員工具的運作方式</span><span class="sxs-lookup"><span data-stu-id="30f89-131">How the Secret Manager tool works</span></span>

<span data-ttu-id="30f89-132">密碼管理員工具會將實作的詳細資料加以抽象，包括各值儲存的位置和方式。</span><span class="sxs-lookup"><span data-stu-id="30f89-132">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="30f89-133">不需要知道這些實作細節也可以使用此工具。</span><span class="sxs-lookup"><span data-stu-id="30f89-133">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="30f89-134">這些值會儲存在本機電腦上系統保護的使用者設定檔資料夾中的 JSON 設定檔案：</span><span class="sxs-lookup"><span data-stu-id="30f89-134">The values are stored in a JSON configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="30f89-135">Windows</span><span class="sxs-lookup"><span data-stu-id="30f89-135">Windows</span></span>](#tab/windows)

<span data-ttu-id="30f89-136">檔案系統路徑：</span><span class="sxs-lookup"><span data-stu-id="30f89-136">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="linux--macostablinuxmacos"></a>[<span data-ttu-id="30f89-137">Linux/macOS</span><span class="sxs-lookup"><span data-stu-id="30f89-137">Linux / macOS</span></span>](#tab/linux+macos)

<span data-ttu-id="30f89-138">檔案系統路徑：</span><span class="sxs-lookup"><span data-stu-id="30f89-138">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="30f89-139">在先前的檔案路徑中， `<user_secrets_id>`將取代`UserSecretsId`為 *.csproj*檔案中指定的值。</span><span class="sxs-lookup"><span data-stu-id="30f89-139">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="30f89-140">請勿撰寫依賴秘密管理員工具所儲存之資料位置或格式的程式碼。</span><span class="sxs-lookup"><span data-stu-id="30f89-140">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="30f89-141">這些執行詳細資料可能會變更。</span><span class="sxs-lookup"><span data-stu-id="30f89-141">These implementation details may change.</span></span> <span data-ttu-id="30f89-142">例如，秘密值不會加密，但未來可能會是。</span><span class="sxs-lookup"><span data-stu-id="30f89-142">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="30f89-143">安裝秘密管理員工具</span><span class="sxs-lookup"><span data-stu-id="30f89-143">Install the Secret Manager tool</span></span>

<span data-ttu-id="30f89-144">「秘密管理員」工具隨附于 .NET Core SDK 2.1.300 或更新版本中的 .NET Core CLI。</span><span class="sxs-lookup"><span data-stu-id="30f89-144">The Secret Manager tool is bundled with the .NET Core CLI in .NET Core SDK 2.1.300 or later.</span></span> <span data-ttu-id="30f89-145">在2.1.300 之前的 .NET Core SDK 版本中，需要安裝工具。</span><span class="sxs-lookup"><span data-stu-id="30f89-145">For .NET Core SDK versions before 2.1.300, tool installation is necessary.</span></span>

> [!TIP]
> <span data-ttu-id="30f89-146">從`dotnet --version`命令 shell 執行，以查看已安裝的 .NET Core SDK 版本號碼。</span><span class="sxs-lookup"><span data-stu-id="30f89-146">Run `dotnet --version` from a command shell to see the installed .NET Core SDK version number.</span></span>

<span data-ttu-id="30f89-147">如果所使用的 .NET Core SDK 包含此工具，則會顯示警告：</span><span class="sxs-lookup"><span data-stu-id="30f89-147">A warning is displayed if the .NET Core SDK being used includes the tool:</span></span>

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

<span data-ttu-id="30f89-148">在 ASP.NET Core 專案中安裝[Microsoft.extensions.secretmanager.tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="30f89-148">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project.</span></span> <span data-ttu-id="30f89-149">例如：</span><span class="sxs-lookup"><span data-stu-id="30f89-149">For example:</span></span>

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=15-16)]

<span data-ttu-id="30f89-150">在命令 shell 中執行下列命令來驗證工具安裝：</span><span class="sxs-lookup"><span data-stu-id="30f89-150">Execute the following command in a command shell to validate the tool installation:</span></span>

```dotnetcli
dotnet user-secrets -h
```

<span data-ttu-id="30f89-151">[秘密管理員] 工具會顯示範例使用方式、選項和命令說明：</span><span class="sxs-lookup"><span data-stu-id="30f89-151">The Secret Manager tool displays sample usage, options, and command help:</span></span>

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
> <span data-ttu-id="30f89-152">您必須與 *.csproj*檔案位於相同的目錄中，才能執行 *.csproj*檔案的`DotNetCliToolReference`元素中定義的工具。</span><span class="sxs-lookup"><span data-stu-id="30f89-152">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>

::: moniker-end

## <a name="enable-secret-storage"></a><span data-ttu-id="30f89-153">啟用秘密儲存</span><span class="sxs-lookup"><span data-stu-id="30f89-153">Enable secret storage</span></span>

<span data-ttu-id="30f89-154">「秘密管理員」工具會針對儲存在使用者設定檔中的專案特定設定進行操作。</span><span class="sxs-lookup"><span data-stu-id="30f89-154">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="30f89-155">「密碼管理員」工具在`init` .NET Core SDK 3.0.100 或更新版本中包含一個命令。</span><span class="sxs-lookup"><span data-stu-id="30f89-155">The Secret Manager tool includes an `init` command in .NET Core SDK 3.0.100 or later.</span></span> <span data-ttu-id="30f89-156">若要使用使用者秘密，請在專案目錄中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="30f89-156">To use user secrets, run the following command in the project directory:</span></span>

```dotnetcli
dotnet user-secrets init
```

<span data-ttu-id="30f89-157">上述命令會`UserSecretsId` `PropertyGroup`在 *.csproj*檔案的中新增專案。</span><span class="sxs-lookup"><span data-stu-id="30f89-157">The preceding command adds a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="30f89-158">根據預設，的內部文字`UserSecretsId`是 GUID。</span><span class="sxs-lookup"><span data-stu-id="30f89-158">By default, the inner text of `UserSecretsId` is a GUID.</span></span> <span data-ttu-id="30f89-159">內部文字是任意的，但對專案而言是唯一的。</span><span class="sxs-lookup"><span data-stu-id="30f89-159">The inner text is arbitrary, but is unique to the project.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="30f89-160">若要使用使用者秘密，請`UserSecretsId` `PropertyGroup`在 *.csproj*檔案的中定義元素。</span><span class="sxs-lookup"><span data-stu-id="30f89-160">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="30f89-161">的內部文字是`UserSecretsId`任意的，但對專案而言是唯一的。</span><span class="sxs-lookup"><span data-stu-id="30f89-161">The inner text of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="30f89-162">開發人員通常會產生的`UserSecretsId`GUID。</span><span class="sxs-lookup"><span data-stu-id="30f89-162">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

> [!TIP]
> <span data-ttu-id="30f89-163">在 Visual Studio 中，以滑鼠右鍵按一下方案總管中的專案，然後從內容功能表中選取 [**管理使用者秘密**]。</span><span class="sxs-lookup"><span data-stu-id="30f89-163">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="30f89-164">此手勢會將`UserSecretsId`已填入 GUID 的元素新增至 *.csproj*檔案。</span><span class="sxs-lookup"><span data-stu-id="30f89-164">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span>

## <a name="set-a-secret"></a><span data-ttu-id="30f89-165">設定密碼</span><span class="sxs-lookup"><span data-stu-id="30f89-165">Set a secret</span></span>

<span data-ttu-id="30f89-166">定義由金鑰和其值組成的應用程式密碼。</span><span class="sxs-lookup"><span data-stu-id="30f89-166">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="30f89-167">此密碼與專案的`UserSecretsId`值相關聯。</span><span class="sxs-lookup"><span data-stu-id="30f89-167">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="30f89-168">例如，從 *.csproj*檔案所在的目錄執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="30f89-168">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="30f89-169">在上述範例中，冒號表示`Movies`是`ServiceApiKey`具有屬性的物件常值。</span><span class="sxs-lookup"><span data-stu-id="30f89-169">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="30f89-170">秘密管理員工具也可以從其他目錄中使用。</span><span class="sxs-lookup"><span data-stu-id="30f89-170">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="30f89-171">使用選項來提供 .csproj 檔案所在的檔案系統路徑。 `--project`</span><span class="sxs-lookup"><span data-stu-id="30f89-171">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="30f89-172">例如：</span><span class="sxs-lookup"><span data-stu-id="30f89-172">For example:</span></span>

```dotnetcli
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

### <a name="json-structure-flattening-in-visual-studio"></a><span data-ttu-id="30f89-173">Visual Studio 中的 JSON 結構簡維</span><span class="sxs-lookup"><span data-stu-id="30f89-173">JSON structure flattening in Visual Studio</span></span>

<span data-ttu-id="30f89-174">Visual Studio 的 [**管理使用者秘密**] 手勢會在文字編輯器中開啟一個*秘密 json*檔案。</span><span class="sxs-lookup"><span data-stu-id="30f89-174">Visual Studio's **Manage User Secrets** gesture opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="30f89-175">以要儲存的機碼值組取代*密碼. json*的內容。</span><span class="sxs-lookup"><span data-stu-id="30f89-175">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="30f89-176">例如：</span><span class="sxs-lookup"><span data-stu-id="30f89-176">For example:</span></span>

```json
{
  "Movies": {
    "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="30f89-177">JSON 結構會在透過`dotnet user-secrets remove`或`dotnet user-secrets set`進行修改之後壓平合併。</span><span class="sxs-lookup"><span data-stu-id="30f89-177">The JSON structure is flattened after modifications via `dotnet user-secrets remove` or `dotnet user-secrets set`.</span></span> <span data-ttu-id="30f89-178">例如， `dotnet user-secrets remove "Movies:ConnectionString"` `Movies`執行會折迭物件常值。</span><span class="sxs-lookup"><span data-stu-id="30f89-178">For example, running `dotnet user-secrets remove "Movies:ConnectionString"` collapses the `Movies` object literal.</span></span> <span data-ttu-id="30f89-179">修改過的檔案如下所示：</span><span class="sxs-lookup"><span data-stu-id="30f89-179">The modified file resembles the following:</span></span>

```json
{
  "Movies:ServiceApiKey": "12345"
}
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="30f89-180">設定多個秘密</span><span class="sxs-lookup"><span data-stu-id="30f89-180">Set multiple secrets</span></span>

<span data-ttu-id="30f89-181">您可以透過將 JSON 傳送至`set`命令的方式來設定密碼批次。</span><span class="sxs-lookup"><span data-stu-id="30f89-181">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="30f89-182">在下列範例中，*輸入 json*檔案的內容會以管道傳送至`set`命令。</span><span class="sxs-lookup"><span data-stu-id="30f89-182">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="30f89-183">Windows</span><span class="sxs-lookup"><span data-stu-id="30f89-183">Windows</span></span>](#tab/windows)

<span data-ttu-id="30f89-184">開啟命令 shell，然後執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="30f89-184">Open a command shell, and execute the following command:</span></span>

  ```dotnetcli
  type .\input.json | dotnet user-secrets set
  ```

# <a name="linux--macostablinuxmacos"></a>[<span data-ttu-id="30f89-185">Linux/macOS</span><span class="sxs-lookup"><span data-stu-id="30f89-185">Linux / macOS</span></span>](#tab/linux+macos)

<span data-ttu-id="30f89-186">開啟命令 shell，然後執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="30f89-186">Open a command shell, and execute the following command:</span></span>

  ```dotnetcli
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="30f89-187">存取秘密</span><span class="sxs-lookup"><span data-stu-id="30f89-187">Access a secret</span></span>

<span data-ttu-id="30f89-188">[ASP.NET Core 設定 API](xref:fundamentals/configuration/index)提供秘密管理員密碼的存取權。</span><span class="sxs-lookup"><span data-stu-id="30f89-188">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span>

::: moniker range=">= aspnetcore-2.0 <= aspnetcore-2.2"

<span data-ttu-id="30f89-189">如果您的專案是以 .NET Framework 為目標，請安裝[Usersecrets.xml](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="30f89-189">If your project targets .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="30f89-190">在 ASP.NET Core 2.0 或更新版本中，當專案呼叫<xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>以預先設定的預設值來初始化主機的新實例時，就會自動在開發模式中新增使用者秘密設定來源。</span><span class="sxs-lookup"><span data-stu-id="30f89-190">In ASP.NET Core 2.0 or later, the user secrets configuration source is automatically added in development mode when the project calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to initialize a new instance of the host with preconfigured defaults.</span></span> <span data-ttu-id="30f89-191">`CreateDefaultBuilder`當<xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*>為時<xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>呼叫： <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName></span><span class="sxs-lookup"><span data-stu-id="30f89-191">`CreateDefaultBuilder` calls <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> when the <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName> is <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

<span data-ttu-id="30f89-192">當`CreateDefaultBuilder`未呼叫時，請在函式中呼叫<xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*>以`Startup`明確新增使用者秘密設定來源。</span><span class="sxs-lookup"><span data-stu-id="30f89-192">When `CreateDefaultBuilder` isn't called, add the user secrets configuration source explicitly by calling <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> in the `Startup` constructor.</span></span> <span data-ttu-id="30f89-193">只有`AddUserSecrets`當應用程式在開發環境中執行時才會呼叫，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="30f89-193">Call `AddUserSecrets` only when the app runs in the Development environment, as shown in the following example:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="30f89-194">請安裝[Usersecrets.xml](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="30f89-194">Install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="30f89-195">使用在此<xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> `Startup`函式中的呼叫來新增使用者秘密設定來源：</span><span class="sxs-lookup"><span data-stu-id="30f89-195">Add the user secrets configuration source with a call to <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

<span data-ttu-id="30f89-196">您可以透過`Configuration` API 來抓取使用者秘密：</span><span class="sxs-lookup"><span data-stu-id="30f89-196">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=26)]

::: moniker-end

## <a name="map-secrets-to-a-poco"></a><span data-ttu-id="30f89-197">將秘密對應至 POCO</span><span class="sxs-lookup"><span data-stu-id="30f89-197">Map secrets to a POCO</span></span>

<span data-ttu-id="30f89-198">將整個物件常值對應至 POCO （具有屬性的簡單 .NET 類別），對於匯總相關屬性很有用。</span><span class="sxs-lookup"><span data-stu-id="30f89-198">Mapping an entire object literal to a POCO (a simple .NET class with properties) is useful for aggregating related properties.</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="30f89-199">若要將上述密碼對應到 POCO，請使用`Configuration` API 的[物件圖形](xref:fundamentals/configuration/index#bind-to-an-object-graph)系結功能。</span><span class="sxs-lookup"><span data-stu-id="30f89-199">To map the preceding secrets to a POCO, use the `Configuration` API's [object graph binding](xref:fundamentals/configuration/index#bind-to-an-object-graph) feature.</span></span> <span data-ttu-id="30f89-200">下列程式碼會系結至`MovieSettings`自訂 POCO 並`ServiceApiKey`存取屬性值：</span><span class="sxs-lookup"><span data-stu-id="30f89-200">The following code binds to a custom `MovieSettings` POCO and accesses the `ServiceApiKey` property value:</span></span>

::: moniker range=">= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

::: moniker range="= aspnetcore-1.0"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

<span data-ttu-id="30f89-201">和密碼會對應至中`MovieSettings`的個別屬性： `Movies:ServiceApiKey` `Movies:ConnectionString`</span><span class="sxs-lookup"><span data-stu-id="30f89-201">The `Movies:ConnectionString` and `Movies:ServiceApiKey` secrets are mapped to the respective properties in `MovieSettings`:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="30f89-202">使用秘密取代字串</span><span class="sxs-lookup"><span data-stu-id="30f89-202">String replacement with secrets</span></span>

<span data-ttu-id="30f89-203">以純文字儲存密碼並不安全。</span><span class="sxs-lookup"><span data-stu-id="30f89-203">Storing passwords in plain text is insecure.</span></span> <span data-ttu-id="30f89-204">例如，儲存在*appsettings*中的資料庫連接字串可能包含指定使用者的密碼：</span><span class="sxs-lookup"><span data-stu-id="30f89-204">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="30f89-205">更安全的方法是將密碼儲存為秘密。</span><span class="sxs-lookup"><span data-stu-id="30f89-205">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="30f89-206">例如：</span><span class="sxs-lookup"><span data-stu-id="30f89-206">For example:</span></span>

```dotnetcli
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="30f89-207">從*appsettings*的連接字串中移除索引鍵/值組。`Password`</span><span class="sxs-lookup"><span data-stu-id="30f89-207">Remove the `Password` key-value pair from the connection string in *appsettings.json*.</span></span> <span data-ttu-id="30f89-208">例如：</span><span class="sxs-lookup"><span data-stu-id="30f89-208">For example:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="30f89-209">您可以在<xref:System.Data.SqlClient.SqlConnectionStringBuilder>物件的<xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password*>屬性上設定密碼的值，以完成連接字串：</span><span class="sxs-lookup"><span data-stu-id="30f89-209">The secret's value can be set on a <xref:System.Data.SqlClient.SqlConnectionStringBuilder> object's <xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password*> property to complete the connection string:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]

::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="30f89-210">列出秘密</span><span class="sxs-lookup"><span data-stu-id="30f89-210">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="30f89-211">從 *.csproj*檔案所在的目錄執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="30f89-211">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets list
```

<span data-ttu-id="30f89-212">即會出現下列輸出：</span><span class="sxs-lookup"><span data-stu-id="30f89-212">The following output appears:</span></span>

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

<span data-ttu-id="30f89-213">在上述範例中，索引鍵名稱中的冒號代表在*私密金鑰內的物件階層。*</span><span class="sxs-lookup"><span data-stu-id="30f89-213">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="30f89-214">移除單一秘密</span><span class="sxs-lookup"><span data-stu-id="30f89-214">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="30f89-215">從 *.csproj*檔案所在的目錄執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="30f89-215">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="30f89-216">應用程式的私密金鑰*json*檔案已修改，以移除與`MoviesConnectionString`金鑰相關聯的機碼值組：</span><span class="sxs-lookup"><span data-stu-id="30f89-216">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="30f89-217">執行`dotnet user-secrets list`會顯示下列訊息：</span><span class="sxs-lookup"><span data-stu-id="30f89-217">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="30f89-218">移除所有秘密</span><span class="sxs-lookup"><span data-stu-id="30f89-218">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="30f89-219">從 *.csproj*檔案所在的目錄執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="30f89-219">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets clear
```

<span data-ttu-id="30f89-220">應用程式的所有使用者秘密都已從*密碼 json*檔案中刪除：</span><span class="sxs-lookup"><span data-stu-id="30f89-220">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="30f89-221">執行`dotnet user-secrets list`會顯示下列訊息：</span><span class="sxs-lookup"><span data-stu-id="30f89-221">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="30f89-222">其他資源</span><span class="sxs-lookup"><span data-stu-id="30f89-222">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
