---
title: 在 ASP.NET Core 開發的應用程式祕密的安全儲存體
author: rick-anderson
description: 了解如何儲存和擷取為應用程式祕密的 ASP.NET Core 應用程式開發期間的機密資訊。
ms.author: scaddie
ms.custom: mvc
ms.date: 06/21/2018
uid: security/app-secrets
ms.openlocfilehash: d3b2de1a17012986ef8dea7aaf8636dd35d10fa1
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/11/2018
ms.locfileid: "38126907"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="0a9cf-103">在 ASP.NET Core 開發的應用程式祕密的安全儲存體</span><span class="sxs-lookup"><span data-stu-id="0a9cf-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="0a9cf-104">藉由[Rick Anderson](https://twitter.com/RickAndMSFT)， [Daniel Roth](https://github.com/danroth27)，和[Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="0a9cf-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="0a9cf-105">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0a9cf-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="0a9cf-106">本文件說明針對儲存和擷取機密資料的 ASP.NET Core 應用程式開發期間的技術。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-106">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="0a9cf-107">您應該永遠不會將密碼或其他機密資料儲存在原始程式碼中，而且您不應該在開發中使用生產環境祕密，或測試模式。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-107">You should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development or test mode.</span></span> <span data-ttu-id="0a9cf-108">您可以透過 [Azure Key Vault 設定提供者](xref:security/key-vault-configuration)儲存及保護 Azure 測試與生產祕密。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-108">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="0a9cf-109">環境變數</span><span class="sxs-lookup"><span data-stu-id="0a9cf-109">Environment variables</span></span>

<span data-ttu-id="0a9cf-110">環境變數用來避免儲存在程式碼，或在本機設定檔中的應用程式密碼。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-110">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="0a9cf-111">環境變數覆寫所有先前指定的組態來源的組態值。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-111">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="0a9cf-112">藉由呼叫設定環境變數值的讀取[AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables)在`Startup`建構函式：</span><span class="sxs-lookup"><span data-stu-id="0a9cf-112">Configure the reading of environment variable values by calling [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]
::: moniker-end

<span data-ttu-id="0a9cf-113">中的 ASP.NET Core web 應用程式，請考慮**個別使用者帳戶**安全性已啟用。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-113">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="0a9cf-114">包含在專案中預設的資料庫連接字串*appsettings.json*具有索引鍵的檔案`DefaultConnection`。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-114">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="0a9cf-115">預設的連接字串是 localdb，這會在使用者模式中執行，而且不需要密碼。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-115">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="0a9cf-116">應用程式在部署期間，`DefaultConnection`機碼值可以覆寫環境變數的值。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-116">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="0a9cf-117">環境變數可能會儲存機密認證的完整連接字串。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-117">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="0a9cf-118">環境變數通常會儲存在一般、 未加密的文字。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-118">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="0a9cf-119">如果電腦或處理序遭到入侵，就可以由不受信任的合作對象存取環境變數。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-119">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="0a9cf-120">您可能需要其他措施以避免使用者密碼洩露。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-120">Additional measures to prevent disclosure of user secrets may be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="0a9cf-121">密碼管理員</span><span class="sxs-lookup"><span data-stu-id="0a9cf-121">Secret Manager</span></span>

<span data-ttu-id="0a9cf-122">Secret Manager 工具會在 ASP.NET Core 專案的開發期間儲存機密資料。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-122">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="0a9cf-123">在此情況下，某份機密資料會是應用程式祕密。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-123">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="0a9cf-124">應用程式密碼會儲存在專案樹狀結構與不同的位置。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-124">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="0a9cf-125">應用程式祕密與特定專案相關聯或在數個專案之間共用。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-125">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="0a9cf-126">應用程式祕密未簽入原始檔控制中。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-126">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="0a9cf-127">密碼管理員工具不會加密預存機密資料，並不會被視為受信任存放區。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-127">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="0a9cf-128">它是僅限開發用途。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-128">It's for development purposes only.</span></span> <span data-ttu-id="0a9cf-129">索引鍵和值會儲存在使用者設定檔的目錄中的 JSON 組態檔。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-129">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="0a9cf-130">Secret Manager 工具的運作方式</span><span class="sxs-lookup"><span data-stu-id="0a9cf-130">How the Secret Manager tool works</span></span>

<span data-ttu-id="0a9cf-131">密碼管理員工具會將實作的詳細資料加以抽象，包括各值儲存的位置和方式。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-131">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="0a9cf-132">不需要知道這些實作細節也可以使用此工具。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-132">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="0a9cf-133">值會儲存在 JSON 組態檔中的保護系統的使用者設定檔資料夾在本機電腦上：</span><span class="sxs-lookup"><span data-stu-id="0a9cf-133">The values are stored in a JSON configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="0a9cf-134">Windows</span><span class="sxs-lookup"><span data-stu-id="0a9cf-134">Windows</span></span>](#tab/windows)

<span data-ttu-id="0a9cf-135">檔案系統路徑：</span><span class="sxs-lookup"><span data-stu-id="0a9cf-135">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="macostabmacos"></a>[<span data-ttu-id="0a9cf-136">macOS</span><span class="sxs-lookup"><span data-stu-id="0a9cf-136">macOS</span></span>](#tab/macos)

<span data-ttu-id="0a9cf-137">檔案系統路徑：</span><span class="sxs-lookup"><span data-stu-id="0a9cf-137">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

# <a name="linuxtablinux"></a>[<span data-ttu-id="0a9cf-138">Linux</span><span class="sxs-lookup"><span data-stu-id="0a9cf-138">Linux</span></span>](#tab/linux)

<span data-ttu-id="0a9cf-139">檔案系統路徑：</span><span class="sxs-lookup"><span data-stu-id="0a9cf-139">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="0a9cf-140">在上述檔案路徑中，取代`<user_secrets_id>`具有`UserSecretsId`中指定值 *.csproj*檔案。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-140">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="0a9cf-141">不要撰寫相依於使用 Secret Manager 工具所儲存的資料格式的位置的程式碼。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-141">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="0a9cf-142">這些實作細節可能會變更。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-142">These implementation details may change.</span></span> <span data-ttu-id="0a9cf-143">比方說，祕密的值不會加密，但可能在未來。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-143">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"
## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="0a9cf-144">安裝 Secret Manager 工具</span><span class="sxs-lookup"><span data-stu-id="0a9cf-144">Install the Secret Manager tool</span></span>

<span data-ttu-id="0a9cf-145">Secret Manager 工具隨附於.NET Core sdk 2.1.300.NET Core CLI。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-145">The Secret Manager tool is bundled with the .NET Core CLI as of .NET Core SDK 2.1.300.</span></span> <span data-ttu-id="0a9cf-146">如需.NET Core SDK 2.1.300 之前的版本，工具的安裝是必要的。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-146">For .NET Core SDK versions before 2.1.300, tool installation is necessary.</span></span>

> [!TIP]
> <span data-ttu-id="0a9cf-147">執行`dotnet --version`從命令殼層，若要查看已安裝的.NET Core SDK 版本號碼。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-147">Run `dotnet --version` from a command shell to see the installed .NET Core SDK version number.</span></span>

<span data-ttu-id="0a9cf-148">如果正在使用的.NET Core SDK 包含工具，就會顯示一則警告：</span><span class="sxs-lookup"><span data-stu-id="0a9cf-148">A warning is displayed if the .NET Core SDK being used includes the tool:</span></span>

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

<span data-ttu-id="0a9cf-149">安裝[包含 Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) ASP.NET Core 專案中的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-149">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project.</span></span> <span data-ttu-id="0a9cf-150">例如: </span><span class="sxs-lookup"><span data-stu-id="0a9cf-150">For example:</span></span>

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]

<span data-ttu-id="0a9cf-151">若要驗證工具安裝命令殼層中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="0a9cf-151">Execute the following command in a command shell to validate the tool installation:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="0a9cf-152">Secret Manager 工具會顯示範例使用方式、 選項和命令說明：</span><span class="sxs-lookup"><span data-stu-id="0a9cf-152">The Secret Manager tool displays sample usage, options, and command help:</span></span>

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
> <span data-ttu-id="0a9cf-153">您必須在相同的目錄 *.csproj*若要執行工具中定義的檔案 *.csproj*檔案的`DotNetCliToolReference`項目。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-153">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>
::: moniker-end

## <a name="set-a-secret"></a><span data-ttu-id="0a9cf-154">設定密碼</span><span class="sxs-lookup"><span data-stu-id="0a9cf-154">Set a secret</span></span>

<span data-ttu-id="0a9cf-155">Secret Manager 工具作儲存在您的使用者設定檔中的專案特定的組態設定。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-155">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span> <span data-ttu-id="0a9cf-156">若要使用使用者的機密資訊，請定義`UserSecretsId`項目內`PropertyGroup`的 *.csproj*檔案。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-156">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="0a9cf-157">值`UserSecretsId`任意的但是是唯一的專案。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-157">The value of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="0a9cf-158">開發人員通常會產生 GUID `UserSecretsId`。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-158">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker range="<= aspnetcore-1.1"
[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]
::: moniker-end

> [!TIP]
> <span data-ttu-id="0a9cf-159">在 Visual Studio 中，以滑鼠右鍵按一下方案總管 中的專案，然後選取**管理使用者祕密**從內容功能表。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-159">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="0a9cf-160">將這個筆勢`UserSecretsId`項目，為 GUID，以填入 *.csproj*檔案。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-160">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span> <span data-ttu-id="0a9cf-161">Visual Studio 會開啟*secrets.json*在文字編輯器中的檔案。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-161">Visual Studio opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="0a9cf-162">內容取代*secrets.json*與儲存的索引鍵 / 值組。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-162">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="0a9cf-163">例如：[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]</span><span class="sxs-lookup"><span data-stu-id="0a9cf-163">For example: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]</span></span>

<span data-ttu-id="0a9cf-164">定義索引鍵和其值所組成的應用程式祕密。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-164">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="0a9cf-165">密碼是該專案相關聯`UserSecretsId`值。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-165">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="0a9cf-166">例如，執行下列命令，從在其中的目錄 *.csproj*檔案是否存在：</span><span class="sxs-lookup"><span data-stu-id="0a9cf-166">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="0a9cf-167">在上述範例中，在冒號表示`Movies`是一種物件常值與`ServiceApiKey`屬性。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-167">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="0a9cf-168">可以從其他目錄太使用 Secret Manager 工具。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-168">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="0a9cf-169">使用`--project`選項來提供檔案系統路徑，處 *.csproj*檔案是否存在。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-169">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="0a9cf-170">例如: </span><span class="sxs-lookup"><span data-stu-id="0a9cf-170">For example:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="0a9cf-171">設定多個密碼</span><span class="sxs-lookup"><span data-stu-id="0a9cf-171">Set multiple secrets</span></span>

<span data-ttu-id="0a9cf-172">來設定批次的祕密，請使用管線傳送至 JSON`set`命令。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-172">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="0a9cf-173">在下列範例中， *input.json*檔案的內容會輸送到`set`命令。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-173">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="0a9cf-174">Windows</span><span class="sxs-lookup"><span data-stu-id="0a9cf-174">Windows</span></span>](#tab/windows)

<span data-ttu-id="0a9cf-175">開啟命令殼層中，然後執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="0a9cf-175">Open a command shell, and execute the following command:</span></span>

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="macostabmacos"></a>[<span data-ttu-id="0a9cf-176">macOS</span><span class="sxs-lookup"><span data-stu-id="0a9cf-176">macOS</span></span>](#tab/macos)

<span data-ttu-id="0a9cf-177">開啟命令殼層中，然後執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="0a9cf-177">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

# <a name="linuxtablinux"></a>[<span data-ttu-id="0a9cf-178">Linux</span><span class="sxs-lookup"><span data-stu-id="0a9cf-178">Linux</span></span>](#tab/linux)

<span data-ttu-id="0a9cf-179">開啟命令殼層中，然後執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="0a9cf-179">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="0a9cf-180">存取祕密</span><span class="sxs-lookup"><span data-stu-id="0a9cf-180">Access a secret</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="0a9cf-181">[ASP.NET Core 組態 API](xref:fundamentals/configuration/index)提供 Secret Manager 祕密的存取。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-181">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="0a9cf-182">安裝[Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-182">Install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="0a9cf-183">新增使用者密碼設定來源，藉由呼叫[AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets)在`Startup`建構函式：</span><span class="sxs-lookup"><span data-stu-id="0a9cf-183">Add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="0a9cf-184">[ASP.NET Core 組態 API](xref:fundamentals/configuration/index)提供 Secret Manager 祕密的存取。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-184">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="0a9cf-185">如果您的專案以.NET Framework 為目標，安裝[Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-185">If your project targets the .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="0a9cf-186">在 ASP.NET Core 2.0 或更新版本中，使用者密碼設定來源會自動加入開發模式時的專案呼叫[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder)初始化主應用程式使用預先設定的預設值的新執行個體。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-186">In ASP.NET Core 2.0 or later, the user secrets configuration source is automatically added in development mode when the project calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to initialize a new instance of the host with preconfigured defaults.</span></span> <span data-ttu-id="0a9cf-187">`CreateDefaultBuilder` 呼叫[AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets)當[EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname)會[開發](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span><span class="sxs-lookup"><span data-stu-id="0a9cf-187">`CreateDefaultBuilder` calls [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) when the [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) is [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

<span data-ttu-id="0a9cf-188">當`CreateDefaultBuilder`不是呼叫在主應用程式建構期間，新增使用者密碼設定來源，藉由呼叫[AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets)在`Startup`建構函式：</span><span class="sxs-lookup"><span data-stu-id="0a9cf-188">When `CreateDefaultBuilder` isn't called during host construction, add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]
::: moniker-end

<span data-ttu-id="0a9cf-189">使用者的機密資訊可以透過擷取`Configuration`API:</span><span class="sxs-lookup"><span data-stu-id="0a9cf-189">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range="<= aspnetcore-1.1"
[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]
::: moniker-end

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="0a9cf-190">有祕密的字串取代</span><span class="sxs-lookup"><span data-stu-id="0a9cf-190">String replacement with secrets</span></span>

<span data-ttu-id="0a9cf-191">以純文字儲存密碼並不安全的。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-191">Storing passwords in plain text is insecure.</span></span> <span data-ttu-id="0a9cf-192">例如，資料庫連接字串儲存在*appsettings.json*可能包含指定之使用者的密碼：</span><span class="sxs-lookup"><span data-stu-id="0a9cf-192">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="0a9cf-193">更安全的方法是將密碼儲存作為祕密。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-193">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="0a9cf-194">例如: </span><span class="sxs-lookup"><span data-stu-id="0a9cf-194">For example:</span></span>

```console
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="0a9cf-195">移除`Password`連接字串中的索引鍵-值配對*appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-195">Remove the `Password` key-value pair from the connection string in *appsettings.json*.</span></span> <span data-ttu-id="0a9cf-196">例如: </span><span class="sxs-lookup"><span data-stu-id="0a9cf-196">For example:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="0a9cf-197">祕密的值可以設定在[SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder)物件的[密碼](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password)完成連接字串屬性：</span><span class="sxs-lookup"><span data-stu-id="0a9cf-197">The secret's value can be set on a [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder) object's [Password](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password) property to complete the connection string:</span></span>

::: moniker range="<= aspnetcore-1.1"
[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]
::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="0a9cf-198">列出祕密</span><span class="sxs-lookup"><span data-stu-id="0a9cf-198">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="0a9cf-199">執行下列命令，從在其中的目錄 *.csproj*檔案是否存在：</span><span class="sxs-lookup"><span data-stu-id="0a9cf-199">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets list
```

<span data-ttu-id="0a9cf-200">出現下列輸出：</span><span class="sxs-lookup"><span data-stu-id="0a9cf-200">The following output appears:</span></span>

```console
Movies:ServiceApiKey = 12345
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
```

<span data-ttu-id="0a9cf-201">在上述範例中，索引鍵的名稱中的冒號表示物件的階層架構內*secrets.json*。</span><span class="sxs-lookup"><span data-stu-id="0a9cf-201">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="0a9cf-202">移除單一的祕密</span><span class="sxs-lookup"><span data-stu-id="0a9cf-202">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="0a9cf-203">執行下列命令，從在其中的目錄 *.csproj*檔案是否存在：</span><span class="sxs-lookup"><span data-stu-id="0a9cf-203">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="0a9cf-204">應用程式的*secrets.json*已修改檔案，以移除相關聯的索引鍵 / 值組`MoviesConnectionString`機碼：</span><span class="sxs-lookup"><span data-stu-id="0a9cf-204">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="0a9cf-205">執行`dotnet user-secrets list`顯示下列訊息：</span><span class="sxs-lookup"><span data-stu-id="0a9cf-205">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="0a9cf-206">移除所有的祕密</span><span class="sxs-lookup"><span data-stu-id="0a9cf-206">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="0a9cf-207">執行下列命令，從在其中的目錄 *.csproj*檔案是否存在：</span><span class="sxs-lookup"><span data-stu-id="0a9cf-207">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets clear
```

<span data-ttu-id="0a9cf-208">從已刪除的應用程式的所有使用者祕密*secrets.json*檔案：</span><span class="sxs-lookup"><span data-stu-id="0a9cf-208">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="0a9cf-209">執行`dotnet user-secrets list`顯示下列訊息：</span><span class="sxs-lookup"><span data-stu-id="0a9cf-209">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="0a9cf-210">其他資源</span><span class="sxs-lookup"><span data-stu-id="0a9cf-210">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
