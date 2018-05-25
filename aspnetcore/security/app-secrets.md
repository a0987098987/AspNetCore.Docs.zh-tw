---
title: 安全存放裝置的開發工作中 ASP.NET Core 應用程式密碼
author: rick-anderson
description: 了解如何儲存和擷取應用程式密碼為 ASP.NET Core 應用程式開發期間的機密資訊。
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 05/23/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/app-secrets
ms.openlocfilehash: ece2bf541df2b4acac60a88767cc57ede473bd49
ms.sourcegitcommit: 1b94305cc79843e2b0866dae811dab61c21980ad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2018
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="0c9e1-103">安全存放裝置的開發工作中 ASP.NET Core 應用程式密碼</span><span class="sxs-lookup"><span data-stu-id="0c9e1-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="0c9e1-104">由[Rick Anderson](https://twitter.com/RickAndMSFT)，[奧 Roth](https://github.com/danroth27)，和[Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="0c9e1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="0c9e1-105">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0c9e1-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="0c9e1-106">本文件說明儲存和擷取機密資料的 ASP.NET Core 應用程式開發期間的技術。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-106">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="0c9e1-107">您永遠不應該在原始程式碼中，儲存的密碼或其他機密資料，您不應該在開發中使用生產機密資料或測試模式。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-107">You should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development or test mode.</span></span> <span data-ttu-id="0c9e1-108">您可以儲存並保護 Azure 測試與實際的機密資訊使用[Azure 金鑰保存庫的組態提供者](xref:security/key-vault-configuration)。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-108">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="0c9e1-109">環境變數</span><span class="sxs-lookup"><span data-stu-id="0c9e1-109">Environment variables</span></span>

<span data-ttu-id="0c9e1-110">環境變數用來避免在程式碼，或在本機的組態檔中的應用程式密碼的儲存體。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-110">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="0c9e1-111">環境變數覆寫所有先前指定的設定來源的組態值。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-111">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="0c9e1-112">藉由呼叫設定環境變數值的讀取[AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables)中`Startup`建構函式：</span><span class="sxs-lookup"><span data-stu-id="0c9e1-112">Configure the reading of environment variable values by calling [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) in the `Startup` constructor:</span></span>

<span data-ttu-id="0c9e1-113">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="0c9e1-113">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]</span></span>
::: moniker-end

<span data-ttu-id="0c9e1-114">請考慮 ASP.NET Core web 應用程式在其中**個別使用者帳戶**已啟用安全性。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-114">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="0c9e1-115">預設資料庫的連接字串包含在專案中的*appsettings.json*檔案具有索引鍵`DefaultConnection`。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-115">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="0c9e1-116">預設的連接字串是 localdb，這會在使用者模式中執行，而不需要密碼。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-116">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="0c9e1-117">應用程式部署期間`DefaultConnection`機碼值會覆寫與環境變數的值。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-117">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="0c9e1-118">環境變數可能會儲存敏感認證的完整連接字串。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-118">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="0c9e1-119">環境變數通常儲存在一般、 未加密的文字。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-119">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="0c9e1-120">如果電腦或處理序遭到入侵，可以由不受信任的一方存取環境變數。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-120">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="0c9e1-121">您可能需要其他措施以避免洩露使用者密碼。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-121">Additional measures to prevent disclosure of user secrets may be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="0c9e1-122">密碼管理員</span><span class="sxs-lookup"><span data-stu-id="0c9e1-122">Secret Manager</span></span>

<span data-ttu-id="0c9e1-123">密碼管理員工具的 ASP.NET Core 專案開發時儲存機密資料。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-123">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="0c9e1-124">在此內容中的機密資料是應用程式密碼。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-124">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="0c9e1-125">應用程式密碼會儲存在專案樹狀結構與不同的位置。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-125">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="0c9e1-126">應用程式密碼與特定的專案相關聯或在數個專案之間共用。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-126">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="0c9e1-127">應用程式密碼不被簽入原始檔控制。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-127">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="0c9e1-128">密碼管理員工具不會加密預存機密資料，並不會被視為受信任存放區。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-128">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="0c9e1-129">它是僅限開發用途。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-129">It's for development purposes only.</span></span> <span data-ttu-id="0c9e1-130">索引鍵和值會儲存在使用者設定檔的目錄中的 JSON 組態檔。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-130">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="0c9e1-131">密碼管理員工具的運作方式</span><span class="sxs-lookup"><span data-stu-id="0c9e1-131">How the Secret Manager tool works</span></span>

<span data-ttu-id="0c9e1-132">密碼管理員工具會將實作的詳細資料加以抽象，包括各值儲存的位置和方式。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-132">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="0c9e1-133">不需要知道這些實作細節也可以使用此工具。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-133">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="0c9e1-134">值會儲存在本機電腦上系統保護的使用者設定檔資料夾的 JSON 組態檔：</span><span class="sxs-lookup"><span data-stu-id="0c9e1-134">The values are stored in a JSON configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="0c9e1-135">Windows</span><span class="sxs-lookup"><span data-stu-id="0c9e1-135">Windows</span></span>](#tab/windows)

<span data-ttu-id="0c9e1-136">檔案系統路徑：</span><span class="sxs-lookup"><span data-stu-id="0c9e1-136">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="macostabmacos"></a>[<span data-ttu-id="0c9e1-137">macOS</span><span class="sxs-lookup"><span data-stu-id="0c9e1-137">macOS</span></span>](#tab/macos)

<span data-ttu-id="0c9e1-138">檔案系統路徑：</span><span class="sxs-lookup"><span data-stu-id="0c9e1-138">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

# <a name="linuxtablinux"></a>[<span data-ttu-id="0c9e1-139">Linux</span><span class="sxs-lookup"><span data-stu-id="0c9e1-139">Linux</span></span>](#tab/linux)

<span data-ttu-id="0c9e1-140">檔案系統路徑：</span><span class="sxs-lookup"><span data-stu-id="0c9e1-140">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="0c9e1-141">在上述檔案路徑，取代`<user_secrets_id>`與`UserSecretsId`中指定的值 *.csproj*檔案。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-141">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="0c9e1-142">不要撰寫程式碼所依賴的位置或格式儲存密碼管理員工具的資料。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-142">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="0c9e1-143">這些實作細節，可能會變更。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-143">These implementation details may change.</span></span> <span data-ttu-id="0c9e1-144">比方說，密碼的值未加密，但可能在未來。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-144">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"
## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="0c9e1-145">安裝密碼管理員工具</span><span class="sxs-lookup"><span data-stu-id="0c9e1-145">Install the Secret Manager tool</span></span>

<span data-ttu-id="0c9e1-146">密碼管理員工具是.NET Core CLI 為準，.NET Core SDK 2.1.300 套件組合。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-146">The Secret Manager tool is bundled with the .NET Core CLI as of .NET Core SDK 2.1.300.</span></span> <span data-ttu-id="0c9e1-147">2.1.300 以前的.NET Core SDK 版本，工具的安裝是必要的。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-147">For .NET Core SDK versions before 2.1.300, tool installation is necessary.</span></span>

> [!TIP]
> <span data-ttu-id="0c9e1-148">執行`dotnet --version`從命令殼層，若要查看已安裝的.NET Core SDK 版本號碼。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-148">Run `dotnet --version` from a command shell to see the installed .NET Core SDK version number.</span></span>

<span data-ttu-id="0c9e1-149">如果正在使用的.NET Core SDK 包含工具，會顯示警告：</span><span class="sxs-lookup"><span data-stu-id="0c9e1-149">A warning is displayed if the .NET Core SDK being used includes the tool:</span></span>

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

<span data-ttu-id="0c9e1-150">安裝[Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) ASP.NET Core 專案中的 NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-150">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project.</span></span> <span data-ttu-id="0c9e1-151">例如: </span><span class="sxs-lookup"><span data-stu-id="0c9e1-151">For example:</span></span>

<span data-ttu-id="0c9e1-152">[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]</span><span class="sxs-lookup"><span data-stu-id="0c9e1-152">[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]</span></span>

<span data-ttu-id="0c9e1-153">若要驗證工具的安裝命令殼層中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="0c9e1-153">Execute the following command in a command shell to validate the tool installation:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="0c9e1-154">密碼管理員工具會顯示範例使用方式、 選項和命令的說明：</span><span class="sxs-lookup"><span data-stu-id="0c9e1-154">The Secret Manager tool displays sample usage, options, and command help:</span></span>

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
> <span data-ttu-id="0c9e1-155">您必須在相同的目錄 *.csproj*檔案來執行工具中定義 *.csproj*檔案的`DotNetCliToolReference`項目。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-155">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>
::: moniker-end

## <a name="set-a-secret"></a><span data-ttu-id="0c9e1-156">設定密碼</span><span class="sxs-lookup"><span data-stu-id="0c9e1-156">Set a secret</span></span>

<span data-ttu-id="0c9e1-157">密碼管理員工具會依據儲存在您的使用者設定檔中的專案特定的組態設定。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-157">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span> <span data-ttu-id="0c9e1-158">若要使用使用者的機密資訊，請定義`UserSecretsId`內的項目`PropertyGroup`的 *.csproj*檔案。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-158">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="0c9e1-159">值`UserSecretsId`任意的但是是唯一的專案。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-159">The value of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="0c9e1-160">開發人員通常會產生的 GUID `UserSecretsId`。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-160">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="0c9e1-161">[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="0c9e1-161">[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="0c9e1-162">[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="0c9e1-162">[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span></span>
::: moniker-end

> [!TIP]
> <span data-ttu-id="0c9e1-163">在 Visual Studio 中，以滑鼠右鍵按一下方案總管 中的專案，然後選取**管理使用者密碼**從內容功能表。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-163">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="0c9e1-164">新增此筆勢`UserSecretsId`項目，以填入 GUID *.csproj*檔案。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-164">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span> <span data-ttu-id="0c9e1-165">Visual Studio 隨即開啟*secrets.json*文字編輯器中的檔案。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-165">Visual Studio opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="0c9e1-166">取代內容*secrets.json*與儲存的索引鍵-值組。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-166">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="0c9e1-167">例如：[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]</span><span class="sxs-lookup"><span data-stu-id="0c9e1-167">For example: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]</span></span>

<span data-ttu-id="0c9e1-168">定義索引鍵和其值所組成的應用程式密碼。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-168">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="0c9e1-169">密碼會與專案相關聯`UserSecretsId`值。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-169">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="0c9e1-170">例如，執行下列命令，從在其中的目錄 *.csproj*檔案是否存在：</span><span class="sxs-lookup"><span data-stu-id="0c9e1-170">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="0c9e1-171">在上述範例中，使用冒號表示`Movies`是物件常值與`ServiceApiKey`屬性。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-171">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="0c9e1-172">可以從其他目錄太使用密碼管理員工具。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-172">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="0c9e1-173">使用`--project`選項提供的檔案系統路徑 *.csproj*檔案是否存在。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-173">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="0c9e1-174">例如: </span><span class="sxs-lookup"><span data-stu-id="0c9e1-174">For example:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="0c9e1-175">設定多個密碼</span><span class="sxs-lookup"><span data-stu-id="0c9e1-175">Set multiple secrets</span></span>

<span data-ttu-id="0c9e1-176">批次，密碼可以設定經由管道 JSON`set`命令。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-176">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="0c9e1-177">在下列範例中， *input.json*檔案的內容會經由管道輸出至`set`命令。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-177">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="0c9e1-178">Windows</span><span class="sxs-lookup"><span data-stu-id="0c9e1-178">Windows</span></span>](#tab/windows)

<span data-ttu-id="0c9e1-179">開啟命令殼層，然後執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="0c9e1-179">Open a command shell, and execute the following command:</span></span>

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="macostabmacos"></a>[<span data-ttu-id="0c9e1-180">macOS</span><span class="sxs-lookup"><span data-stu-id="0c9e1-180">macOS</span></span>](#tab/macos)

<span data-ttu-id="0c9e1-181">開啟命令殼層，然後執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="0c9e1-181">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

# <a name="linuxtablinux"></a>[<span data-ttu-id="0c9e1-182">Linux</span><span class="sxs-lookup"><span data-stu-id="0c9e1-182">Linux</span></span>](#tab/linux)

<span data-ttu-id="0c9e1-183">開啟命令殼層，然後執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="0c9e1-183">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="0c9e1-184">存取密碼</span><span class="sxs-lookup"><span data-stu-id="0c9e1-184">Access a secret</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="0c9e1-185">[ASP.NET Core 組態 API](xref:fundamentals/configuration/index)提供密碼管理員密碼的存取。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-185">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="0c9e1-186">安裝[Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-186">Install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="0c9e1-187">新增使用者密碼設定來源，藉由呼叫[AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets)中`Startup`建構函式：</span><span class="sxs-lookup"><span data-stu-id="0c9e1-187">Add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

<span data-ttu-id="0c9e1-188">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span><span class="sxs-lookup"><span data-stu-id="0c9e1-188">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="0c9e1-189">[ASP.NET Core 組態 API](xref:fundamentals/configuration/index)提供密碼管理員密碼的存取。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-189">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="0c9e1-190">如果您專案的目標.NET Framework，安裝[Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-190">If your project targets the .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="0c9e1-191">在 ASP.NET Core 2.0 或更新版本中，使用者密碼設定來源會自動加入開發模式時的專案呼叫[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder)初始化主應用程式使用預先設定的預設值的新執行個體。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-191">In ASP.NET Core 2.0 or later, the user secrets configuration source is automatically added in development mode when the project calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to initialize a new instance of the host with preconfigured defaults.</span></span> <span data-ttu-id="0c9e1-192">`CreateDefaultBuilder` 呼叫[AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets)時[EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname)是[開發](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span><span class="sxs-lookup"><span data-stu-id="0c9e1-192">`CreateDefaultBuilder` calls [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) when the [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) is [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span></span>

<span data-ttu-id="0c9e1-193">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="0c9e1-193">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]</span></span>

<span data-ttu-id="0c9e1-194">當`CreateDefaultBuilder`不在主機建構期間呼叫，新增使用者密碼設定來源，藉由呼叫[AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets)中`Startup`建構函式：</span><span class="sxs-lookup"><span data-stu-id="0c9e1-194">When `CreateDefaultBuilder` isn't called during host construction, add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

<span data-ttu-id="0c9e1-195">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span><span class="sxs-lookup"><span data-stu-id="0c9e1-195">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span></span>
::: moniker-end

<span data-ttu-id="0c9e1-196">可以透過擷取使用者密碼`Configuration`API:</span><span class="sxs-lookup"><span data-stu-id="0c9e1-196">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="0c9e1-197">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]</span><span class="sxs-lookup"><span data-stu-id="0c9e1-197">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="0c9e1-198">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]</span><span class="sxs-lookup"><span data-stu-id="0c9e1-198">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]</span></span>
::: moniker-end

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="0c9e1-199">含有機密資料的字串取代</span><span class="sxs-lookup"><span data-stu-id="0c9e1-199">String replacement with secrets</span></span>

<span data-ttu-id="0c9e1-200">以純文字儲存密碼很危險。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-200">Storing passwords in plain text is risky.</span></span> <span data-ttu-id="0c9e1-201">例如，資料庫連接字串儲存在*appsettings.json*可能包含指定之使用者的密碼：</span><span class="sxs-lookup"><span data-stu-id="0c9e1-201">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="0c9e1-202">最安全的作法是將密碼儲存做為密碼。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-202">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="0c9e1-203">例如: </span><span class="sxs-lookup"><span data-stu-id="0c9e1-203">For example:</span></span>

```console
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="0c9e1-204">在 密碼取代*appsettings.json*以預留位置。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-204">Replace the password in *appsettings.json* with a placeholder.</span></span> <span data-ttu-id="0c9e1-205">在下列範例中，`{0}`用做為表單預留位置[複合格式字串](/dotnet/standard/base-types/composite-formatting#composite-format-string)。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-205">In the following example, `{0}` is used as the placeholder to form a [Composite Format String](/dotnet/standard/base-types/composite-formatting#composite-format-string).</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="0c9e1-206">密碼的值可以插入到完成連接字串的預留位置：</span><span class="sxs-lookup"><span data-stu-id="0c9e1-206">The secret's value can be injected into the placeholder to complete the connection string:</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="0c9e1-207">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=23-25)]</span><span class="sxs-lookup"><span data-stu-id="0c9e1-207">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=23-25)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="0c9e1-208">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-16)]</span><span class="sxs-lookup"><span data-stu-id="0c9e1-208">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-16)]</span></span>
::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="0c9e1-209">列出密碼</span><span class="sxs-lookup"><span data-stu-id="0c9e1-209">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="0c9e1-210">執行下列命令，從在其中的目錄 *.csproj*檔案是否存在：</span><span class="sxs-lookup"><span data-stu-id="0c9e1-210">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets list
```

<span data-ttu-id="0c9e1-211">出現下列輸出：</span><span class="sxs-lookup"><span data-stu-id="0c9e1-211">The following output appears:</span></span>

```console
Movies:ServiceApiKey = 12345
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
```

<span data-ttu-id="0c9e1-212">在上述範例中，索引鍵的名稱中的冒號代表物件階層架構內*secrets.json*。</span><span class="sxs-lookup"><span data-stu-id="0c9e1-212">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="0c9e1-213">移除單一密碼</span><span class="sxs-lookup"><span data-stu-id="0c9e1-213">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="0c9e1-214">執行下列命令，從在其中的目錄 *.csproj*檔案是否存在：</span><span class="sxs-lookup"><span data-stu-id="0c9e1-214">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="0c9e1-215">應用程式的*secrets.json*已修改檔案，以移除相關聯的索引鍵 / 值組`MoviesConnectionString`機碼：</span><span class="sxs-lookup"><span data-stu-id="0c9e1-215">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="0c9e1-216">執行`dotnet user-secrets list`顯示下列訊息：</span><span class="sxs-lookup"><span data-stu-id="0c9e1-216">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="0c9e1-217">移除所有的機密資料</span><span class="sxs-lookup"><span data-stu-id="0c9e1-217">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="0c9e1-218">執行下列命令，從在其中的目錄 *.csproj*檔案是否存在：</span><span class="sxs-lookup"><span data-stu-id="0c9e1-218">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets clear
```

<span data-ttu-id="0c9e1-219">已刪除的應用程式的所有使用者密碼從*secrets.json*檔案：</span><span class="sxs-lookup"><span data-stu-id="0c9e1-219">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="0c9e1-220">執行`dotnet user-secrets list`顯示下列訊息：</span><span class="sxs-lookup"><span data-stu-id="0c9e1-220">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="0c9e1-221">其他資源</span><span class="sxs-lookup"><span data-stu-id="0c9e1-221">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
