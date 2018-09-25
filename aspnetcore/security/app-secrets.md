---
title: 在 ASP.NET Core 開發的應用程式祕密的安全儲存體
author: rick-anderson
description: 了解如何儲存和擷取為應用程式祕密的 ASP.NET Core 應用程式開發期間的機密資訊。
ms.author: scaddie
ms.custom: mvc
ms.date: 09/24/2018
uid: security/app-secrets
ms.openlocfilehash: 1ecd9b87b9e4aa2c511349fe407c0cea9dc9f921
ms.sourcegitcommit: 4d5f8680d68b39c411b46c73f7014f8aa0f12026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2018
ms.locfileid: "47028267"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="03392-103">在 ASP.NET Core 開發的應用程式祕密的安全儲存體</span><span class="sxs-lookup"><span data-stu-id="03392-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="03392-104">藉由[Rick Anderson](https://twitter.com/RickAndMSFT)， [Daniel Roth](https://github.com/danroth27)，和[Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="03392-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="03392-105">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="03392-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="03392-106">本文件說明針對儲存和擷取機密資料的 ASP.NET Core 應用程式開發期間的技術。</span><span class="sxs-lookup"><span data-stu-id="03392-106">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="03392-107">永遠不會將密碼或其他機密資料儲存在原始程式碼中。</span><span class="sxs-lookup"><span data-stu-id="03392-107">Never store passwords or other sensitive data in source code.</span></span> <span data-ttu-id="03392-108">不應使用生產環境祕密進行開發或測試。</span><span class="sxs-lookup"><span data-stu-id="03392-108">Production secrets shouldn't be used for development or test.</span></span> <span data-ttu-id="03392-109">您可以透過 [Azure Key Vault 設定提供者](xref:security/key-vault-configuration)儲存及保護 Azure 測試與生產祕密。</span><span class="sxs-lookup"><span data-stu-id="03392-109">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="03392-110">環境變數</span><span class="sxs-lookup"><span data-stu-id="03392-110">Environment variables</span></span>

<span data-ttu-id="03392-111">環境變數用來避免儲存在程式碼，或在本機設定檔中的應用程式密碼。</span><span class="sxs-lookup"><span data-stu-id="03392-111">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="03392-112">環境變數覆寫所有先前指定的組態來源的組態值。</span><span class="sxs-lookup"><span data-stu-id="03392-112">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="03392-113">藉由呼叫設定環境變數值的讀取[AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables)在`Startup`建構函式：</span><span class="sxs-lookup"><span data-stu-id="03392-113">Configure the reading of environment variable values by calling [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=8)]

::: moniker-end

<span data-ttu-id="03392-114">中的 ASP.NET Core web 應用程式，請考慮**個別使用者帳戶**安全性已啟用。</span><span class="sxs-lookup"><span data-stu-id="03392-114">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="03392-115">包含在專案中預設的資料庫連接字串*appsettings.json*具有索引鍵的檔案`DefaultConnection`。</span><span class="sxs-lookup"><span data-stu-id="03392-115">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="03392-116">預設的連接字串是 localdb，這會在使用者模式中執行，而且不需要密碼。</span><span class="sxs-lookup"><span data-stu-id="03392-116">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="03392-117">應用程式在部署期間，`DefaultConnection`機碼值可以覆寫環境變數的值。</span><span class="sxs-lookup"><span data-stu-id="03392-117">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="03392-118">環境變數可能會儲存機密認證的完整連接字串。</span><span class="sxs-lookup"><span data-stu-id="03392-118">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="03392-119">環境變數通常會儲存在一般、 未加密的文字。</span><span class="sxs-lookup"><span data-stu-id="03392-119">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="03392-120">如果電腦或處理序遭到入侵，就可以由不受信任的合作對象存取環境變數。</span><span class="sxs-lookup"><span data-stu-id="03392-120">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="03392-121">您可能需要其他措施以避免使用者密碼洩露。</span><span class="sxs-lookup"><span data-stu-id="03392-121">Additional measures to prevent disclosure of user secrets may be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="03392-122">密碼管理員</span><span class="sxs-lookup"><span data-stu-id="03392-122">Secret Manager</span></span>

<span data-ttu-id="03392-123">Secret Manager 工具會在 ASP.NET Core 專案的開發期間儲存機密資料。</span><span class="sxs-lookup"><span data-stu-id="03392-123">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="03392-124">在此情況下，某份機密資料會是應用程式祕密。</span><span class="sxs-lookup"><span data-stu-id="03392-124">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="03392-125">應用程式密碼會儲存在專案樹狀結構與不同的位置。</span><span class="sxs-lookup"><span data-stu-id="03392-125">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="03392-126">應用程式祕密與特定專案相關聯或在數個專案之間共用。</span><span class="sxs-lookup"><span data-stu-id="03392-126">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="03392-127">應用程式祕密未簽入原始檔控制中。</span><span class="sxs-lookup"><span data-stu-id="03392-127">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="03392-128">密碼管理員工具不會加密預存機密資料，並不會被視為受信任存放區。</span><span class="sxs-lookup"><span data-stu-id="03392-128">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="03392-129">它是僅限開發用途。</span><span class="sxs-lookup"><span data-stu-id="03392-129">It's for development purposes only.</span></span> <span data-ttu-id="03392-130">索引鍵和值會儲存在使用者設定檔的目錄中的 JSON 組態檔。</span><span class="sxs-lookup"><span data-stu-id="03392-130">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="03392-131">Secret Manager 工具的運作方式</span><span class="sxs-lookup"><span data-stu-id="03392-131">How the Secret Manager tool works</span></span>

<span data-ttu-id="03392-132">密碼管理員工具會將實作的詳細資料加以抽象，包括各值儲存的位置和方式。</span><span class="sxs-lookup"><span data-stu-id="03392-132">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="03392-133">不需要知道這些實作細節也可以使用此工具。</span><span class="sxs-lookup"><span data-stu-id="03392-133">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="03392-134">值會儲存在 JSON 組態檔中的保護系統的使用者設定檔資料夾在本機電腦上：</span><span class="sxs-lookup"><span data-stu-id="03392-134">The values are stored in a JSON configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="03392-135">Windows</span><span class="sxs-lookup"><span data-stu-id="03392-135">Windows</span></span>](#tab/windows)

<span data-ttu-id="03392-136">檔案系統路徑：</span><span class="sxs-lookup"><span data-stu-id="03392-136">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="macostabmacos"></a>[<span data-ttu-id="03392-137">macOS</span><span class="sxs-lookup"><span data-stu-id="03392-137">macOS</span></span>](#tab/macos)

<span data-ttu-id="03392-138">檔案系統路徑：</span><span class="sxs-lookup"><span data-stu-id="03392-138">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

# <a name="linuxtablinux"></a>[<span data-ttu-id="03392-139">Linux</span><span class="sxs-lookup"><span data-stu-id="03392-139">Linux</span></span>](#tab/linux)

<span data-ttu-id="03392-140">檔案系統路徑：</span><span class="sxs-lookup"><span data-stu-id="03392-140">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="03392-141">在上述檔案路徑中，取代`<user_secrets_id>`具有`UserSecretsId`中指定值 *.csproj*檔案。</span><span class="sxs-lookup"><span data-stu-id="03392-141">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="03392-142">不要撰寫相依於使用 Secret Manager 工具所儲存的資料格式的位置的程式碼。</span><span class="sxs-lookup"><span data-stu-id="03392-142">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="03392-143">這些實作細節可能會變更。</span><span class="sxs-lookup"><span data-stu-id="03392-143">These implementation details may change.</span></span> <span data-ttu-id="03392-144">比方說，祕密的值不會加密，但可能在未來。</span><span class="sxs-lookup"><span data-stu-id="03392-144">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="03392-145">安裝 Secret Manager 工具</span><span class="sxs-lookup"><span data-stu-id="03392-145">Install the Secret Manager tool</span></span>

<span data-ttu-id="03392-146">Secret Manager 工具是使用.NET Core CLI，在.NET Core SDK 2.1.300 搭售或更新版本。</span><span class="sxs-lookup"><span data-stu-id="03392-146">The Secret Manager tool is bundled with the .NET Core CLI in .NET Core SDK 2.1.300 or later.</span></span> <span data-ttu-id="03392-147">如需.NET Core SDK 2.1.300 之前的版本，工具的安裝是必要的。</span><span class="sxs-lookup"><span data-stu-id="03392-147">For .NET Core SDK versions before 2.1.300, tool installation is necessary.</span></span>

> [!TIP]
> <span data-ttu-id="03392-148">執行`dotnet --version`從命令殼層，若要查看已安裝的.NET Core SDK 版本號碼。</span><span class="sxs-lookup"><span data-stu-id="03392-148">Run `dotnet --version` from a command shell to see the installed .NET Core SDK version number.</span></span>

<span data-ttu-id="03392-149">如果正在使用的.NET Core SDK 包含工具，就會顯示一則警告：</span><span class="sxs-lookup"><span data-stu-id="03392-149">A warning is displayed if the .NET Core SDK being used includes the tool:</span></span>

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

<span data-ttu-id="03392-150">安裝[包含 Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) ASP.NET Core 專案中的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="03392-150">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project.</span></span> <span data-ttu-id="03392-151">例如: </span><span class="sxs-lookup"><span data-stu-id="03392-151">For example:</span></span>

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=15-16)]

<span data-ttu-id="03392-152">若要驗證工具安裝命令殼層中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="03392-152">Execute the following command in a command shell to validate the tool installation:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="03392-153">Secret Manager 工具會顯示範例使用方式、 選項和命令說明：</span><span class="sxs-lookup"><span data-stu-id="03392-153">The Secret Manager tool displays sample usage, options, and command help:</span></span>

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
> <span data-ttu-id="03392-154">您必須在相同的目錄 *.csproj*若要執行工具中定義的檔案 *.csproj*檔案的`DotNetCliToolReference`項目。</span><span class="sxs-lookup"><span data-stu-id="03392-154">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>

::: moniker-end

## <a name="set-a-secret"></a><span data-ttu-id="03392-155">設定密碼</span><span class="sxs-lookup"><span data-stu-id="03392-155">Set a secret</span></span>

<span data-ttu-id="03392-156">Secret Manager 工具作儲存在您的使用者設定檔中的專案特定的組態設定。</span><span class="sxs-lookup"><span data-stu-id="03392-156">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span> <span data-ttu-id="03392-157">若要使用使用者的機密資訊，請定義`UserSecretsId`項目內`PropertyGroup`的 *.csproj*檔案。</span><span class="sxs-lookup"><span data-stu-id="03392-157">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="03392-158">值`UserSecretsId`任意的但是是唯一的專案。</span><span class="sxs-lookup"><span data-stu-id="03392-158">The value of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="03392-159">開發人員通常會產生 GUID `UserSecretsId`。</span><span class="sxs-lookup"><span data-stu-id="03392-159">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

> [!TIP]
> <span data-ttu-id="03392-160">在 Visual Studio 中，以滑鼠右鍵按一下方案總管 中的專案，然後選取**管理使用者祕密**從內容功能表。</span><span class="sxs-lookup"><span data-stu-id="03392-160">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="03392-161">將這個筆勢`UserSecretsId`項目，為 GUID，以填入 *.csproj*檔案。</span><span class="sxs-lookup"><span data-stu-id="03392-161">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span> <span data-ttu-id="03392-162">Visual Studio 會開啟*secrets.json*在文字編輯器中的檔案。</span><span class="sxs-lookup"><span data-stu-id="03392-162">Visual Studio opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="03392-163">內容取代*secrets.json*與儲存的索引鍵 / 值組。</span><span class="sxs-lookup"><span data-stu-id="03392-163">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="03392-164">例如: </span><span class="sxs-lookup"><span data-stu-id="03392-164">For example:</span></span>
> ```json
> {
>   "Movies": {
>     "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
>     "ServiceApiKey": "12345"
>   }
> }
> ```
> <span data-ttu-id="03392-165">透過修改後的 JSON 結構被壓平合併`dotnet user-secrets remove`或`dotnet user-secrets set`。</span><span class="sxs-lookup"><span data-stu-id="03392-165">The JSON structure is flattened after modifications via `dotnet user-secrets remove` or `dotnet user-secrets set`.</span></span> <span data-ttu-id="03392-166">例如，執行`dotnet user-secrets remove "Movies:ConnectionString"`摺疊`Movies`物件常值。</span><span class="sxs-lookup"><span data-stu-id="03392-166">For example, running `dotnet user-secrets remove "Movies:ConnectionString"` collapses the `Movies` object literal.</span></span> <span data-ttu-id="03392-167">修改過的檔案如下所示：</span><span class="sxs-lookup"><span data-stu-id="03392-167">The modified file resembles the following:</span></span>
> ```json
> {
>   "Movies:ServiceApiKey": "12345"
> }
> ```

<span data-ttu-id="03392-168">定義索引鍵和其值所組成的應用程式祕密。</span><span class="sxs-lookup"><span data-stu-id="03392-168">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="03392-169">密碼是該專案相關聯`UserSecretsId`值。</span><span class="sxs-lookup"><span data-stu-id="03392-169">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="03392-170">例如，執行下列命令，從在其中的目錄 *.csproj*檔案是否存在：</span><span class="sxs-lookup"><span data-stu-id="03392-170">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="03392-171">在上述範例中，在冒號表示`Movies`是一種物件常值與`ServiceApiKey`屬性。</span><span class="sxs-lookup"><span data-stu-id="03392-171">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="03392-172">可以從其他目錄太使用 Secret Manager 工具。</span><span class="sxs-lookup"><span data-stu-id="03392-172">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="03392-173">使用`--project`選項來提供檔案系統路徑，處 *.csproj*檔案是否存在。</span><span class="sxs-lookup"><span data-stu-id="03392-173">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="03392-174">例如: </span><span class="sxs-lookup"><span data-stu-id="03392-174">For example:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="03392-175">設定多個密碼</span><span class="sxs-lookup"><span data-stu-id="03392-175">Set multiple secrets</span></span>

<span data-ttu-id="03392-176">來設定批次的祕密，請使用管線傳送至 JSON`set`命令。</span><span class="sxs-lookup"><span data-stu-id="03392-176">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="03392-177">在下列範例中， *input.json*檔案的內容會輸送到`set`命令。</span><span class="sxs-lookup"><span data-stu-id="03392-177">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="03392-178">Windows</span><span class="sxs-lookup"><span data-stu-id="03392-178">Windows</span></span>](#tab/windows)

<span data-ttu-id="03392-179">開啟命令殼層中，然後執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="03392-179">Open a command shell, and execute the following command:</span></span>

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="macostabmacos"></a>[<span data-ttu-id="03392-180">macOS</span><span class="sxs-lookup"><span data-stu-id="03392-180">macOS</span></span>](#tab/macos)

<span data-ttu-id="03392-181">開啟命令殼層中，然後執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="03392-181">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

# <a name="linuxtablinux"></a>[<span data-ttu-id="03392-182">Linux</span><span class="sxs-lookup"><span data-stu-id="03392-182">Linux</span></span>](#tab/linux)

<span data-ttu-id="03392-183">開啟命令殼層中，然後執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="03392-183">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="03392-184">存取祕密</span><span class="sxs-lookup"><span data-stu-id="03392-184">Access a secret</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="03392-185">[ASP.NET Core 組態 API](xref:fundamentals/configuration/index)提供 Secret Manager 祕密的存取。</span><span class="sxs-lookup"><span data-stu-id="03392-185">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="03392-186">如果您的專案以.NET Framework 為目標，安裝[Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="03392-186">If your project targets the .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="03392-187">在 ASP.NET Core 2.0 或更新版本中，使用者密碼設定來源會自動加入開發模式時的專案呼叫[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder)初始化主應用程式使用預先設定的預設值的新執行個體。</span><span class="sxs-lookup"><span data-stu-id="03392-187">In ASP.NET Core 2.0 or later, the user secrets configuration source is automatically added in development mode when the project calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to initialize a new instance of the host with preconfigured defaults.</span></span> <span data-ttu-id="03392-188">`CreateDefaultBuilder` 呼叫[AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets)當[EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname)會[開發](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span><span class="sxs-lookup"><span data-stu-id="03392-188">`CreateDefaultBuilder` calls [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) when the [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) is [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

<span data-ttu-id="03392-189">當`CreateDefaultBuilder`不是呼叫在主應用程式建構期間，新增使用者密碼設定來源，藉由呼叫[AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets)在`Startup`建構函式：</span><span class="sxs-lookup"><span data-stu-id="03392-189">When `CreateDefaultBuilder` isn't called during host construction, add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="03392-190">[ASP.NET Core 組態 API](xref:fundamentals/configuration/index)提供 Secret Manager 祕密的存取。</span><span class="sxs-lookup"><span data-stu-id="03392-190">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="03392-191">安裝[Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="03392-191">Install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="03392-192">新增使用者密碼設定來源，藉由呼叫[AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets)在`Startup`建構函式：</span><span class="sxs-lookup"><span data-stu-id="03392-192">Add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

<span data-ttu-id="03392-193">使用者的機密資訊可以透過擷取`Configuration`API:</span><span class="sxs-lookup"><span data-stu-id="03392-193">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=26)]

::: moniker-end

## <a name="map-secrets-to-a-poco"></a><span data-ttu-id="03392-194">對應至 POCO 的祕密</span><span class="sxs-lookup"><span data-stu-id="03392-194">Map secrets to a POCO</span></span>

<span data-ttu-id="03392-195">將整個物件常值對應至 poco 物件 （具有屬性的簡單.NET 類別） 可用於彙總相關的屬性就行了。</span><span class="sxs-lookup"><span data-stu-id="03392-195">Mapping an entire object literal to a POCO (a simple .NET class with properties) is useful for aggregating related properties.</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="03392-196">若要將上述的祕密對應到 POCO，使用`Configuration`API[物件 graph 繫結](xref:fundamentals/configuration/index#bind-to-an-object-graph)功能。</span><span class="sxs-lookup"><span data-stu-id="03392-196">To map the preceding secrets to a POCO, use the `Configuration` API's [object graph binding](xref:fundamentals/configuration/index#bind-to-an-object-graph) feature.</span></span> <span data-ttu-id="03392-197">下列程式碼會將繫結至自訂`MovieSettings`POCO 和存取`ServiceApiKey`屬性值：</span><span class="sxs-lookup"><span data-stu-id="03392-197">The following code binds to a custom `MovieSettings` POCO and accesses the `ServiceApiKey` property value:</span></span>

::: moniker range=">= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

::: moniker range="= aspnetcore-1.0"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

<span data-ttu-id="03392-198">`Movies:ConnectionString`並`Movies:ServiceApiKey`祕密對應到中的個別屬性`MovieSettings`:</span><span class="sxs-lookup"><span data-stu-id="03392-198">The `Movies:ConnectionString` and `Movies:ServiceApiKey` secrets are mapped to the respective properties in `MovieSettings`:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="03392-199">有祕密的字串取代</span><span class="sxs-lookup"><span data-stu-id="03392-199">String replacement with secrets</span></span>

<span data-ttu-id="03392-200">以純文字儲存密碼並不安全的。</span><span class="sxs-lookup"><span data-stu-id="03392-200">Storing passwords in plain text is insecure.</span></span> <span data-ttu-id="03392-201">例如，資料庫連接字串儲存在*appsettings.json*可能包含指定之使用者的密碼：</span><span class="sxs-lookup"><span data-stu-id="03392-201">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="03392-202">更安全的方法是將密碼儲存作為祕密。</span><span class="sxs-lookup"><span data-stu-id="03392-202">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="03392-203">例如: </span><span class="sxs-lookup"><span data-stu-id="03392-203">For example:</span></span>

```console
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="03392-204">移除`Password`連接字串中的索引鍵-值配對*appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="03392-204">Remove the `Password` key-value pair from the connection string in *appsettings.json*.</span></span> <span data-ttu-id="03392-205">例如: </span><span class="sxs-lookup"><span data-stu-id="03392-205">For example:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="03392-206">祕密的值可以設定在[SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder)物件的[密碼](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password)完成連接字串屬性：</span><span class="sxs-lookup"><span data-stu-id="03392-206">The secret's value can be set on a [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder) object's [Password](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password) property to complete the connection string:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]

::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="03392-207">列出祕密</span><span class="sxs-lookup"><span data-stu-id="03392-207">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="03392-208">執行下列命令，從在其中的目錄 *.csproj*檔案是否存在：</span><span class="sxs-lookup"><span data-stu-id="03392-208">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets list
```

<span data-ttu-id="03392-209">出現下列輸出：</span><span class="sxs-lookup"><span data-stu-id="03392-209">The following output appears:</span></span>

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

<span data-ttu-id="03392-210">在上述範例中，索引鍵的名稱中的冒號表示物件的階層架構內*secrets.json*。</span><span class="sxs-lookup"><span data-stu-id="03392-210">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="03392-211">移除單一的祕密</span><span class="sxs-lookup"><span data-stu-id="03392-211">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="03392-212">執行下列命令，從在其中的目錄 *.csproj*檔案是否存在：</span><span class="sxs-lookup"><span data-stu-id="03392-212">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="03392-213">應用程式的*secrets.json*已修改檔案，以移除相關聯的索引鍵 / 值組`MoviesConnectionString`機碼：</span><span class="sxs-lookup"><span data-stu-id="03392-213">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="03392-214">執行`dotnet user-secrets list`顯示下列訊息：</span><span class="sxs-lookup"><span data-stu-id="03392-214">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="03392-215">移除所有的祕密</span><span class="sxs-lookup"><span data-stu-id="03392-215">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="03392-216">執行下列命令，從在其中的目錄 *.csproj*檔案是否存在：</span><span class="sxs-lookup"><span data-stu-id="03392-216">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets clear
```

<span data-ttu-id="03392-217">從已刪除的應用程式的所有使用者祕密*secrets.json*檔案：</span><span class="sxs-lookup"><span data-stu-id="03392-217">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="03392-218">執行`dotnet user-secrets list`顯示下列訊息：</span><span class="sxs-lookup"><span data-stu-id="03392-218">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="03392-219">其他資源</span><span class="sxs-lookup"><span data-stu-id="03392-219">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
