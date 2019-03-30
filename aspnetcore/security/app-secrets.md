---
title: 在 ASP.NET Core 開發的應用程式祕密的安全儲存體
author: rick-anderson
description: 了解如何儲存和擷取為應用程式祕密的 ASP.NET Core 應用程式開發期間的機密資訊。
ms.author: scaddie
ms.custom: mvc
ms.date: 03/13/2019
uid: security/app-secrets
ms.openlocfilehash: 18313f8284e81d196cbe786f494a607ee97a299f
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/30/2019
ms.locfileid: "58750977"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="83978-103">在 ASP.NET Core 開發的應用程式祕密的安全儲存體</span><span class="sxs-lookup"><span data-stu-id="83978-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="83978-104">藉由[Rick Anderson](https://twitter.com/RickAndMSFT)， [Daniel Roth](https://github.com/danroth27)，和[Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="83978-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="83978-105">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="83978-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="83978-106">本文件說明針對儲存和擷取機密資料的 ASP.NET Core 應用程式開發期間的技術。</span><span class="sxs-lookup"><span data-stu-id="83978-106">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="83978-107">永遠不會將密碼或其他機密資料儲存在原始程式碼中。</span><span class="sxs-lookup"><span data-stu-id="83978-107">Never store passwords or other sensitive data in source code.</span></span> <span data-ttu-id="83978-108">不應使用生產環境祕密進行開發或測試。</span><span class="sxs-lookup"><span data-stu-id="83978-108">Production secrets shouldn't be used for development or test.</span></span> <span data-ttu-id="83978-109">您可以透過 [Azure Key Vault 設定提供者](xref:security/key-vault-configuration)儲存及保護 Azure 測試與生產祕密。</span><span class="sxs-lookup"><span data-stu-id="83978-109">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="83978-110">環境變數</span><span class="sxs-lookup"><span data-stu-id="83978-110">Environment variables</span></span>

<span data-ttu-id="83978-111">環境變數用來避免儲存在程式碼，或在本機設定檔中的應用程式密碼。</span><span class="sxs-lookup"><span data-stu-id="83978-111">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="83978-112">環境變數覆寫所有先前指定的組態來源的組態值。</span><span class="sxs-lookup"><span data-stu-id="83978-112">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="83978-113">藉由呼叫設定環境變數值的讀取<xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*>在`Startup`建構函式：</span><span class="sxs-lookup"><span data-stu-id="83978-113">Configure the reading of environment variable values by calling <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=8)]

::: moniker-end

<span data-ttu-id="83978-114">中的 ASP.NET Core web 應用程式，請考慮**個別使用者帳戶**安全性已啟用。</span><span class="sxs-lookup"><span data-stu-id="83978-114">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="83978-115">包含在專案中預設的資料庫連接字串*appsettings.json*具有索引鍵的檔案`DefaultConnection`。</span><span class="sxs-lookup"><span data-stu-id="83978-115">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="83978-116">預設的連接字串是 localdb，這會在使用者模式中執行，而且不需要密碼。</span><span class="sxs-lookup"><span data-stu-id="83978-116">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="83978-117">應用程式在部署期間，`DefaultConnection`機碼值可以覆寫環境變數的值。</span><span class="sxs-lookup"><span data-stu-id="83978-117">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="83978-118">環境變數可能會儲存機密認證的完整連接字串。</span><span class="sxs-lookup"><span data-stu-id="83978-118">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="83978-119">環境變數通常會儲存在一般、 未加密的文字。</span><span class="sxs-lookup"><span data-stu-id="83978-119">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="83978-120">如果電腦或處理序遭到入侵，就可以由不受信任的合作對象存取環境變數。</span><span class="sxs-lookup"><span data-stu-id="83978-120">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="83978-121">您可能需要其他措施以避免使用者密碼洩露。</span><span class="sxs-lookup"><span data-stu-id="83978-121">Additional measures to prevent disclosure of user secrets may be required.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="secret-manager"></a><span data-ttu-id="83978-122">Secret Manager</span><span class="sxs-lookup"><span data-stu-id="83978-122">Secret Manager</span></span>

<span data-ttu-id="83978-123">Secret Manager 工具會在 ASP.NET Core 專案的開發期間儲存機密資料。</span><span class="sxs-lookup"><span data-stu-id="83978-123">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="83978-124">在此情況下，某份機密資料會是應用程式祕密。</span><span class="sxs-lookup"><span data-stu-id="83978-124">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="83978-125">應用程式密碼會儲存在專案樹狀結構與不同的位置。</span><span class="sxs-lookup"><span data-stu-id="83978-125">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="83978-126">應用程式祕密與特定專案相關聯或在數個專案之間共用。</span><span class="sxs-lookup"><span data-stu-id="83978-126">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="83978-127">應用程式祕密未簽入原始檔控制中。</span><span class="sxs-lookup"><span data-stu-id="83978-127">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="83978-128">密碼管理員工具不會加密預存機密資料，並不會被視為受信任存放區。</span><span class="sxs-lookup"><span data-stu-id="83978-128">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="83978-129">它是僅限開發用途。</span><span class="sxs-lookup"><span data-stu-id="83978-129">It's for development purposes only.</span></span> <span data-ttu-id="83978-130">索引鍵和值會儲存在使用者設定檔的目錄中的 JSON 組態檔。</span><span class="sxs-lookup"><span data-stu-id="83978-130">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="83978-131">Secret Manager 工具的運作方式</span><span class="sxs-lookup"><span data-stu-id="83978-131">How the Secret Manager tool works</span></span>

<span data-ttu-id="83978-132">密碼管理員工具會將實作的詳細資料加以抽象，包括各值儲存的位置和方式。</span><span class="sxs-lookup"><span data-stu-id="83978-132">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="83978-133">不需要知道這些實作細節也可以使用此工具。</span><span class="sxs-lookup"><span data-stu-id="83978-133">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="83978-134">值會儲存在 JSON 組態檔中的保護系統的使用者設定檔資料夾在本機電腦上：</span><span class="sxs-lookup"><span data-stu-id="83978-134">The values are stored in a JSON configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="83978-135">Windows</span><span class="sxs-lookup"><span data-stu-id="83978-135">Windows</span></span>](#tab/windows)

<span data-ttu-id="83978-136">檔案系統路徑：</span><span class="sxs-lookup"><span data-stu-id="83978-136">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="linux--macostablinuxmacos"></a>[<span data-ttu-id="83978-137">Linux / macOS</span><span class="sxs-lookup"><span data-stu-id="83978-137">Linux / macOS</span></span>](#tab/linux+macos)

<span data-ttu-id="83978-138">檔案系統路徑：</span><span class="sxs-lookup"><span data-stu-id="83978-138">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="83978-139">在上述檔案路徑中，取代`<user_secrets_id>`具有`UserSecretsId`中指定值 *.csproj*檔案。</span><span class="sxs-lookup"><span data-stu-id="83978-139">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="83978-140">不要撰寫相依於使用 Secret Manager 工具所儲存的資料格式的位置的程式碼。</span><span class="sxs-lookup"><span data-stu-id="83978-140">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="83978-141">這些實作細節可能會變更。</span><span class="sxs-lookup"><span data-stu-id="83978-141">These implementation details may change.</span></span> <span data-ttu-id="83978-142">比方說，祕密的值不會加密，但可能在未來。</span><span class="sxs-lookup"><span data-stu-id="83978-142">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="83978-143">安裝 Secret Manager 工具</span><span class="sxs-lookup"><span data-stu-id="83978-143">Install the Secret Manager tool</span></span>

<span data-ttu-id="83978-144">Secret Manager 工具是使用.NET Core CLI，在.NET Core SDK 2.1.300 搭售或更新版本。</span><span class="sxs-lookup"><span data-stu-id="83978-144">The Secret Manager tool is bundled with the .NET Core CLI in .NET Core SDK 2.1.300 or later.</span></span> <span data-ttu-id="83978-145">如需.NET Core SDK 2.1.300 之前的版本，工具的安裝是必要的。</span><span class="sxs-lookup"><span data-stu-id="83978-145">For .NET Core SDK versions before 2.1.300, tool installation is necessary.</span></span>

> [!TIP]
> <span data-ttu-id="83978-146">執行`dotnet --version`從命令殼層，若要查看已安裝的.NET Core SDK 版本號碼。</span><span class="sxs-lookup"><span data-stu-id="83978-146">Run `dotnet --version` from a command shell to see the installed .NET Core SDK version number.</span></span>

<span data-ttu-id="83978-147">如果正在使用的.NET Core SDK 包含工具，就會顯示一則警告：</span><span class="sxs-lookup"><span data-stu-id="83978-147">A warning is displayed if the .NET Core SDK being used includes the tool:</span></span>

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

<span data-ttu-id="83978-148">安裝[包含 Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) ASP.NET Core 專案中的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="83978-148">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project.</span></span> <span data-ttu-id="83978-149">例如: </span><span class="sxs-lookup"><span data-stu-id="83978-149">For example:</span></span>

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=15-16)]

<span data-ttu-id="83978-150">若要驗證工具安裝命令殼層中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="83978-150">Execute the following command in a command shell to validate the tool installation:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="83978-151">Secret Manager 工具會顯示範例使用方式、 選項和命令說明：</span><span class="sxs-lookup"><span data-stu-id="83978-151">The Secret Manager tool displays sample usage, options, and command help:</span></span>

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
> <span data-ttu-id="83978-152">您必須在相同的目錄 *.csproj*若要執行工具中定義的檔案 *.csproj*檔案的`DotNetCliToolReference`項目。</span><span class="sxs-lookup"><span data-stu-id="83978-152">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>

::: moniker-end

## <a name="enable-secret-storage"></a><span data-ttu-id="83978-153">啟用密碼儲存體</span><span class="sxs-lookup"><span data-stu-id="83978-153">Enable secret storage</span></span>

<span data-ttu-id="83978-154">Secret Manager 工具作儲存在您的使用者設定檔中的專案特定的組態設定。</span><span class="sxs-lookup"><span data-stu-id="83978-154">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="83978-155">Secret Manager 工具包括`init`命令，在.NET Core SDK 3.0.100 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="83978-155">The Secret Manager tool includes an `init` command in .NET Core SDK 3.0.100 or later.</span></span> <span data-ttu-id="83978-156">若要使用使用者的機密資訊，請在專案目錄執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="83978-156">To use user secrets, run the following command in the project directory:</span></span>

```console
dotnet user-secrets init
```

<span data-ttu-id="83978-157">上述命令會新增`UserSecretsId`項目內`PropertyGroup`的 *.csproj*檔案。</span><span class="sxs-lookup"><span data-stu-id="83978-157">The preceding command adds a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="83978-158">根據預設，內部文字`UserSecretsId`是 GUID。</span><span class="sxs-lookup"><span data-stu-id="83978-158">By default, the inner text of `UserSecretsId` is a GUID.</span></span> <span data-ttu-id="83978-159">內部文字是任意的但專案所特有。</span><span class="sxs-lookup"><span data-stu-id="83978-159">The inner text is arbitrary, but is unique to the project.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="83978-160">若要使用使用者的機密資訊，請定義`UserSecretsId`項目內`PropertyGroup`的 *.csproj*檔案。</span><span class="sxs-lookup"><span data-stu-id="83978-160">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="83978-161">內部文字`UserSecretsId`任意的但是是唯一的專案。</span><span class="sxs-lookup"><span data-stu-id="83978-161">The inner text of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="83978-162">開發人員通常會產生 GUID `UserSecretsId`。</span><span class="sxs-lookup"><span data-stu-id="83978-162">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

> [!TIP]
> <span data-ttu-id="83978-163">在 Visual Studio 中，以滑鼠右鍵按一下方案總管 中的專案，然後選取**管理使用者祕密**從內容功能表。</span><span class="sxs-lookup"><span data-stu-id="83978-163">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="83978-164">將這個筆勢`UserSecretsId`項目，為 GUID，以填入 *.csproj*檔案。</span><span class="sxs-lookup"><span data-stu-id="83978-164">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span>

## <a name="set-a-secret"></a><span data-ttu-id="83978-165">設定密碼</span><span class="sxs-lookup"><span data-stu-id="83978-165">Set a secret</span></span>

<span data-ttu-id="83978-166">定義索引鍵和其值所組成的應用程式祕密。</span><span class="sxs-lookup"><span data-stu-id="83978-166">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="83978-167">密碼是該專案相關聯`UserSecretsId`值。</span><span class="sxs-lookup"><span data-stu-id="83978-167">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="83978-168">例如，執行下列命令，從在其中的目錄 *.csproj*檔案是否存在：</span><span class="sxs-lookup"><span data-stu-id="83978-168">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="83978-169">在上述範例中，在冒號表示`Movies`是一種物件常值與`ServiceApiKey`屬性。</span><span class="sxs-lookup"><span data-stu-id="83978-169">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="83978-170">可以從其他目錄太使用 Secret Manager 工具。</span><span class="sxs-lookup"><span data-stu-id="83978-170">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="83978-171">使用`--project`選項來提供檔案系統路徑，處 *.csproj*檔案是否存在。</span><span class="sxs-lookup"><span data-stu-id="83978-171">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="83978-172">例如: </span><span class="sxs-lookup"><span data-stu-id="83978-172">For example:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

### <a name="json-structure-flattening-in-visual-studio"></a><span data-ttu-id="83978-173">在 Visual Studio 中扁平化的 JSON 結構</span><span class="sxs-lookup"><span data-stu-id="83978-173">JSON structure flattening in Visual Studio</span></span>

<span data-ttu-id="83978-174">Visual Studio**管理使用者祕密**軌跡會開啟*secrets.json*在文字編輯器中的檔案。</span><span class="sxs-lookup"><span data-stu-id="83978-174">Visual Studio's **Manage User Secrets** gesture opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="83978-175">內容取代*secrets.json*與儲存的索引鍵 / 值組。</span><span class="sxs-lookup"><span data-stu-id="83978-175">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="83978-176">例如: </span><span class="sxs-lookup"><span data-stu-id="83978-176">For example:</span></span>

```json
{
  "Movies": {
    "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="83978-177">透過修改後的 JSON 結構被壓平合併`dotnet user-secrets remove`或`dotnet user-secrets set`。</span><span class="sxs-lookup"><span data-stu-id="83978-177">The JSON structure is flattened after modifications via `dotnet user-secrets remove` or `dotnet user-secrets set`.</span></span> <span data-ttu-id="83978-178">例如，執行`dotnet user-secrets remove "Movies:ConnectionString"`摺疊`Movies`物件常值。</span><span class="sxs-lookup"><span data-stu-id="83978-178">For example, running `dotnet user-secrets remove "Movies:ConnectionString"` collapses the `Movies` object literal.</span></span> <span data-ttu-id="83978-179">修改過的檔案如下所示：</span><span class="sxs-lookup"><span data-stu-id="83978-179">The modified file resembles the following:</span></span>

```json
{
  "Movies:ServiceApiKey": "12345"
}
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="83978-180">設定多個密碼</span><span class="sxs-lookup"><span data-stu-id="83978-180">Set multiple secrets</span></span>

<span data-ttu-id="83978-181">來設定批次的祕密，請使用管線傳送至 JSON`set`命令。</span><span class="sxs-lookup"><span data-stu-id="83978-181">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="83978-182">在下列範例中， *input.json*檔案的內容會輸送到`set`命令。</span><span class="sxs-lookup"><span data-stu-id="83978-182">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="83978-183">Windows</span><span class="sxs-lookup"><span data-stu-id="83978-183">Windows</span></span>](#tab/windows)

<span data-ttu-id="83978-184">開啟命令殼層中，然後執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="83978-184">Open a command shell, and execute the following command:</span></span>

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="linux--macostablinuxmacos"></a>[<span data-ttu-id="83978-185">Linux / macOS</span><span class="sxs-lookup"><span data-stu-id="83978-185">Linux / macOS</span></span>](#tab/linux+macos)

<span data-ttu-id="83978-186">開啟命令殼層中，然後執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="83978-186">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="83978-187">存取祕密</span><span class="sxs-lookup"><span data-stu-id="83978-187">Access a secret</span></span>

<span data-ttu-id="83978-188">[ASP.NET Core 組態 API](xref:fundamentals/configuration/index)提供 Secret Manager 祕密的存取。</span><span class="sxs-lookup"><span data-stu-id="83978-188">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span>

::: moniker range=">= aspnetcore-2.0 <= aspnetcore-2.2"

<span data-ttu-id="83978-189">如果您的專案以.NET Framework 為目標，安裝[Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="83978-189">If your project targets .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="83978-190">在 ASP.NET Core 2.0 或更新版本中，使用者密碼設定來源會自動加入開發模式時的專案呼叫<xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>初始化主應用程式使用預先設定的預設值的新執行個體。</span><span class="sxs-lookup"><span data-stu-id="83978-190">In ASP.NET Core 2.0 or later, the user secrets configuration source is automatically added in development mode when the project calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to initialize a new instance of the host with preconfigured defaults.</span></span> <span data-ttu-id="83978-191">`CreateDefaultBuilder` 呼叫<xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*>時<xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName>是<xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>:</span><span class="sxs-lookup"><span data-stu-id="83978-191">`CreateDefaultBuilder` calls <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> when the <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName> is <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

<span data-ttu-id="83978-192">當`CreateDefaultBuilder`不是呼叫，藉由呼叫明確新增使用者密碼設定來源<xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*>在`Startup`建構函式。</span><span class="sxs-lookup"><span data-stu-id="83978-192">When `CreateDefaultBuilder` isn't called, add the user secrets configuration source explicitly by calling <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> in the `Startup` constructor.</span></span> <span data-ttu-id="83978-193">呼叫`AddUserSecrets`僅當應用程式開發的環境中執行，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="83978-193">Call `AddUserSecrets` only when the app runs in the Development environment, as shown in the following example:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="83978-194">安裝[Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="83978-194">Install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="83978-195">新增使用者密碼設定來源，藉由呼叫<xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*>在`Startup`建構函式：</span><span class="sxs-lookup"><span data-stu-id="83978-195">Add the user secrets configuration source with a call to <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

<span data-ttu-id="83978-196">使用者的機密資訊可以透過擷取`Configuration`API:</span><span class="sxs-lookup"><span data-stu-id="83978-196">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=26)]

::: moniker-end

## <a name="map-secrets-to-a-poco"></a><span data-ttu-id="83978-197">對應至 POCO 的祕密</span><span class="sxs-lookup"><span data-stu-id="83978-197">Map secrets to a POCO</span></span>

<span data-ttu-id="83978-198">將整個物件常值對應至 poco 物件 （具有屬性的簡單.NET 類別） 可用於彙總相關的屬性就行了。</span><span class="sxs-lookup"><span data-stu-id="83978-198">Mapping an entire object literal to a POCO (a simple .NET class with properties) is useful for aggregating related properties.</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="83978-199">若要將上述的祕密對應到 POCO，使用`Configuration`API[物件 graph 繫結](xref:fundamentals/configuration/index#bind-to-an-object-graph)功能。</span><span class="sxs-lookup"><span data-stu-id="83978-199">To map the preceding secrets to a POCO, use the `Configuration` API's [object graph binding](xref:fundamentals/configuration/index#bind-to-an-object-graph) feature.</span></span> <span data-ttu-id="83978-200">下列程式碼會將繫結至自訂`MovieSettings`POCO 和存取`ServiceApiKey`屬性值：</span><span class="sxs-lookup"><span data-stu-id="83978-200">The following code binds to a custom `MovieSettings` POCO and accesses the `ServiceApiKey` property value:</span></span>

::: moniker range=">= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

::: moniker range="= aspnetcore-1.0"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

<span data-ttu-id="83978-201">`Movies:ConnectionString`並`Movies:ServiceApiKey`祕密對應到中的個別屬性`MovieSettings`:</span><span class="sxs-lookup"><span data-stu-id="83978-201">The `Movies:ConnectionString` and `Movies:ServiceApiKey` secrets are mapped to the respective properties in `MovieSettings`:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="83978-202">有祕密的字串取代</span><span class="sxs-lookup"><span data-stu-id="83978-202">String replacement with secrets</span></span>

<span data-ttu-id="83978-203">以純文字儲存密碼並不安全的。</span><span class="sxs-lookup"><span data-stu-id="83978-203">Storing passwords in plain text is insecure.</span></span> <span data-ttu-id="83978-204">例如，資料庫連接字串儲存在*appsettings.json*可能包含指定之使用者的密碼：</span><span class="sxs-lookup"><span data-stu-id="83978-204">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="83978-205">更安全的方法是將密碼儲存作為祕密。</span><span class="sxs-lookup"><span data-stu-id="83978-205">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="83978-206">例如: </span><span class="sxs-lookup"><span data-stu-id="83978-206">For example:</span></span>

```console
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="83978-207">移除`Password`連接字串中的索引鍵-值配對*appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="83978-207">Remove the `Password` key-value pair from the connection string in *appsettings.json*.</span></span> <span data-ttu-id="83978-208">例如: </span><span class="sxs-lookup"><span data-stu-id="83978-208">For example:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="83978-209">祕密的值可以設定在<xref:System.Data.SqlClient.SqlConnectionStringBuilder>物件的<xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password*>完成連接字串屬性：</span><span class="sxs-lookup"><span data-stu-id="83978-209">The secret's value can be set on a <xref:System.Data.SqlClient.SqlConnectionStringBuilder> object's <xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password*> property to complete the connection string:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]

::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="83978-210">列出祕密</span><span class="sxs-lookup"><span data-stu-id="83978-210">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="83978-211">執行下列命令，從在其中的目錄 *.csproj*檔案是否存在：</span><span class="sxs-lookup"><span data-stu-id="83978-211">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets list
```

<span data-ttu-id="83978-212">即會出現下列輸出：</span><span class="sxs-lookup"><span data-stu-id="83978-212">The following output appears:</span></span>

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

<span data-ttu-id="83978-213">在上述範例中，索引鍵的名稱中的冒號表示物件的階層架構內*secrets.json*。</span><span class="sxs-lookup"><span data-stu-id="83978-213">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="83978-214">移除單一的祕密</span><span class="sxs-lookup"><span data-stu-id="83978-214">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="83978-215">執行下列命令，從在其中的目錄 *.csproj*檔案是否存在：</span><span class="sxs-lookup"><span data-stu-id="83978-215">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="83978-216">應用程式的*secrets.json*已修改檔案，以移除相關聯的索引鍵 / 值組`MoviesConnectionString`機碼：</span><span class="sxs-lookup"><span data-stu-id="83978-216">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="83978-217">執行`dotnet user-secrets list`顯示下列訊息：</span><span class="sxs-lookup"><span data-stu-id="83978-217">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="83978-218">移除所有的祕密</span><span class="sxs-lookup"><span data-stu-id="83978-218">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="83978-219">執行下列命令，從在其中的目錄 *.csproj*檔案是否存在：</span><span class="sxs-lookup"><span data-stu-id="83978-219">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets clear
```

<span data-ttu-id="83978-220">從已刪除的應用程式的所有使用者祕密*secrets.json*檔案：</span><span class="sxs-lookup"><span data-stu-id="83978-220">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="83978-221">執行`dotnet user-secrets list`顯示下列訊息：</span><span class="sxs-lookup"><span data-stu-id="83978-221">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="83978-222">其他資源</span><span class="sxs-lookup"><span data-stu-id="83978-222">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
