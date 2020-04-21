---
title: ASP.NET核心開發中應用秘密的安全儲存
author: rick-anderson
description: 瞭解如何在開發ASP.NET核心應用期間將敏感資訊存儲和檢索為應用機密。
ms.author: scaddie
ms.custom: mvc
ms.date: 4/20/2020
uid: security/app-secrets
ms.openlocfilehash: 9d4e59c003afc253971ee64fce523c7188d3582a
ms.sourcegitcommit: 5547d920f322e5a823575c031529e4755ab119de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/21/2020
ms.locfileid: "81661796"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="6dd2a-103">ASP.NET核心開發中應用秘密的安全儲存</span><span class="sxs-lookup"><span data-stu-id="6dd2a-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="6dd2a-104">由[里克·安德森](https://twitter.com/RickAndMSFT)、[柯克·拉金](https://twitter.com/serpent5)、[丹尼爾·羅斯](https://github.com/danroth27)和[斯科特·阿迪](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="6dd2a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Kirk Larkin](https://twitter.com/serpent5), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="6dd2a-105">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/app-secrets/samples)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6dd2a-105">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/app-secrets/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="6dd2a-106">本文件介紹在開發計算機上開發ASP.NET酷睿應用期間存儲和檢索敏感數據的技術。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-106">This document explains techniques for storing and retrieving sensitive data during development of an ASP.NET Core app on a development machine.</span></span> <span data-ttu-id="6dd2a-107">切勿在原始碼中儲存密碼或其他敏感資料。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-107">Never store passwords or other sensitive data in source code.</span></span> <span data-ttu-id="6dd2a-108">生產機密不應用於開發或測試。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-108">Production secrets shouldn't be used for development or test.</span></span> <span data-ttu-id="6dd2a-109">不應將機密隨應用一起部署。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-109">Secrets shouldn't be deployed with the app.</span></span> <span data-ttu-id="6dd2a-110">相反,應通過受控方式(如環境變數、Azure 密鑰保管庫等)在生產環境中提供機密。您可以使用[Azure 密鑰保管庫配置提供程式](xref:security/key-vault-configuration)儲存和保護 Azure 測試和生產機密。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-110">Instead, secrets should be made available in the production environment through a controlled means like environment variables, Azure Key Vault, etc. You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="6dd2a-111">環境變數</span><span class="sxs-lookup"><span data-stu-id="6dd2a-111">Environment variables</span></span>

<span data-ttu-id="6dd2a-112">環境變數用於避免在代碼或本地配置檔中存儲應用機密。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-112">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="6dd2a-113">環境變數覆蓋所有以前指定的配置源的配置值。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-113">Environment variables override configuration values for all previously specified configuration sources.</span></span>

<span data-ttu-id="6dd2a-114">請考慮一個ASP.NET核心 Web 應用,其中啟用**了單個使用者帳戶**安全性。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-114">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="6dd2a-115">預設資料庫連接字串包含在項目的*appsettings.json*檔中,其中包含金`DefaultConnection`鑰 。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-115">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="6dd2a-116">預設連接字串用於 LocalDB,該字串在使用者模式下運行,不需要密碼。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-116">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="6dd2a-117">在應用部署期間,`DefaultConnection`可以使用環境變數的值重寫鍵值。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-117">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="6dd2a-118">環境變數可能將完整的連接字串存儲為具有敏感認證。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-118">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="6dd2a-119">環境變數通常以純、未加密的文本存儲。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-119">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="6dd2a-120">如果電腦或進程遭到破壞,則不受信任的方可以訪問環境變數。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-120">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="6dd2a-121">可能需要採取其他措施防止泄露用戶機密。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-121">Additional measures to prevent disclosure of user secrets may be required.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="secret-manager"></a><span data-ttu-id="6dd2a-122">秘密經理</span><span class="sxs-lookup"><span data-stu-id="6dd2a-122">Secret Manager</span></span>

<span data-ttu-id="6dd2a-123">在開發ASP.NET核心專案期間,秘密管理器工具存儲敏感數據。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-123">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="6dd2a-124">在此上下文中,一段敏感數據是應用機密。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-124">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="6dd2a-125">應用機密存儲在與專案樹不同的位置。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-125">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="6dd2a-126">應用機密與特定專案關聯或跨多個項目共用。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-126">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="6dd2a-127">應用機密不會簽入原始程式碼管理。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-127">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="6dd2a-128">秘密管理員工具不加密儲存的秘密,不應被視爲受信任的儲存。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-128">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="6dd2a-129">它僅用於開發目的。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-129">It's for development purposes only.</span></span> <span data-ttu-id="6dd2a-130">鍵和值存儲在使用者配置檔目錄中的 JSON 配置檔中。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-130">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="6dd2a-131">秘密管理員工具的工作原理</span><span class="sxs-lookup"><span data-stu-id="6dd2a-131">How the Secret Manager tool works</span></span>

<span data-ttu-id="6dd2a-132">"秘密管理員"工具會抽象出實現詳細資訊,例如值的存儲位置和方式。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-132">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="6dd2a-133">您可以在不知道這些實現詳細資訊的情況下使用該工具。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-133">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="6dd2a-134">這些值儲存在本地電腦上的受系統保護的使用者設定檔案資料夾中的 JSON 設定檔中:</span><span class="sxs-lookup"><span data-stu-id="6dd2a-134">The values are stored in a JSON configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windows"></a>[<span data-ttu-id="6dd2a-135">Windows</span><span class="sxs-lookup"><span data-stu-id="6dd2a-135">Windows</span></span>](#tab/windows)

<span data-ttu-id="6dd2a-136">檔案系統路徑:</span><span class="sxs-lookup"><span data-stu-id="6dd2a-136">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="linux--macos"></a>[<span data-ttu-id="6dd2a-137">Linux / macOS</span><span class="sxs-lookup"><span data-stu-id="6dd2a-137">Linux / macOS</span></span>](#tab/linux+macos)

<span data-ttu-id="6dd2a-138">檔案系統路徑:</span><span class="sxs-lookup"><span data-stu-id="6dd2a-138">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="6dd2a-139">在前面的檔路徑中,替換為`<user_secrets_id>``UserSecretsId` *.csproj*檔中指定的值。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-139">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="6dd2a-140">不要編寫依賴於使用機密管理員工具保存的資料的位置或格式的代碼。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-140">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="6dd2a-141">這些實現詳細資訊可能會更改。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-141">These implementation details may change.</span></span> <span data-ttu-id="6dd2a-142">例如,機密值未加密,但將來可能已加密。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-142">For example, the secret values aren't encrypted, but could be in the future.</span></span>

## <a name="enable-secret-storage"></a><span data-ttu-id="6dd2a-143">啟用機密儲存</span><span class="sxs-lookup"><span data-stu-id="6dd2a-143">Enable secret storage</span></span>

<span data-ttu-id="6dd2a-144">機密管理員「工具可對存儲在使用者設定檔中的特定於專案的設定設定進行操作。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-144">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span>

<span data-ttu-id="6dd2a-145">機密管理員工具包括 .NET Core SDK 3.0.100`init`或更高版本中 的命令。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-145">The Secret Manager tool includes an `init` command in .NET Core SDK 3.0.100 or later.</span></span> <span data-ttu-id="6dd2a-146">要使用使用者機密,請執行項目目錄中的以下指令:</span><span class="sxs-lookup"><span data-stu-id="6dd2a-146">To use user secrets, run the following command in the project directory:</span></span>

```dotnetcli
dotnet user-secrets init
```

<span data-ttu-id="6dd2a-147">前面的命令在`UserSecretsId`*.csproj*檔中添加`PropertyGroup`一個 元素。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-147">The preceding command adds a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="6dd2a-148">預設情況下,的內部`UserSecretsId`文本是 GUID。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-148">By default, the inner text of `UserSecretsId` is a GUID.</span></span> <span data-ttu-id="6dd2a-149">內部文本是任意的,但對於專案是唯一的。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-149">The inner text is arbitrary, but is unique to the project.</span></span>

[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

<span data-ttu-id="6dd2a-150">在 Visual Studio 中,右鍵單擊解決方案資源管理器中的專案,然後從上下文菜單中選擇 **「管理使用者機密**」。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-150">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="6dd2a-151">此手勢將一`UserSecretsId`個使用 GUID 填充的元素添加到 *.csproj*檔中。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-151">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span>

## <a name="set-a-secret"></a><span data-ttu-id="6dd2a-152">設定機密</span><span class="sxs-lookup"><span data-stu-id="6dd2a-152">Set a secret</span></span>

<span data-ttu-id="6dd2a-153">定義由鍵及其值組成的應用機密。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-153">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="6dd2a-154">機密與項目`UserSecretsId`的值相關聯。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-154">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="6dd2a-155">例如,從*存在 .csproj*檔的目錄中執行以下命令:</span><span class="sxs-lookup"><span data-stu-id="6dd2a-155">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="6dd2a-156">在前面的示例中,冒號表示`Movies`是`ServiceApiKey`具有 屬性的物件文本。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-156">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="6dd2a-157">機密管理器工具也可以從其他目錄使用。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-157">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="6dd2a-158">使用`--project`選項提供*存在 .csproj*檔案的檔案系統路徑。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-158">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="6dd2a-159">例如：</span><span class="sxs-lookup"><span data-stu-id="6dd2a-159">For example:</span></span>

```dotnetcli
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

### <a name="json-structure-flattening-in-visual-studio"></a><span data-ttu-id="6dd2a-160">視覺工作室中的 JSON 結構拼合</span><span class="sxs-lookup"><span data-stu-id="6dd2a-160">JSON structure flattening in Visual Studio</span></span>

<span data-ttu-id="6dd2a-161">可視化工作室的 **「管理使用者機密**」手勢將在文字編輯器中打開*一個機密.json*檔。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-161">Visual Studio's **Manage User Secrets** gesture opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="6dd2a-162">將*機密*內容替換為要存儲的鍵值對。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-162">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="6dd2a-163">例如：</span><span class="sxs-lookup"><span data-stu-id="6dd2a-163">For example:</span></span>

```json
{
  "Movies": {
    "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="6dd2a-164">通過`dotnet user-secrets remove``dotnet user-secrets set`或 進行修改後,JSON 結構將展平。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-164">The JSON structure is flattened after modifications via `dotnet user-secrets remove` or `dotnet user-secrets set`.</span></span> <span data-ttu-id="6dd2a-165">例如,運行`dotnet user-secrets remove "Movies:ConnectionString"`將`Movies`摺疊 物件文本。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-165">For example, running `dotnet user-secrets remove "Movies:ConnectionString"` collapses the `Movies` object literal.</span></span> <span data-ttu-id="6dd2a-166">變更後的檔案類似於以下內容:</span><span class="sxs-lookup"><span data-stu-id="6dd2a-166">The modified file resembles the following:</span></span>

```json
{
  "Movies:ServiceApiKey": "12345"
}
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="6dd2a-167">設定多個機密</span><span class="sxs-lookup"><span data-stu-id="6dd2a-167">Set multiple secrets</span></span>

<span data-ttu-id="6dd2a-168">通過將 JSON`set`管道到 命令,可以設置一批機密。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-168">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="6dd2a-169">在下面的範例中 *,input.json*檔的內容被傳`set`送到命令 。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-169">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windows"></a>[<span data-ttu-id="6dd2a-170">Windows</span><span class="sxs-lookup"><span data-stu-id="6dd2a-170">Windows</span></span>](#tab/windows)

<span data-ttu-id="6dd2a-171">開啟命令外殼,並執行以下命令:</span><span class="sxs-lookup"><span data-stu-id="6dd2a-171">Open a command shell, and execute the following command:</span></span>

  ```dotnetcli
  type .\input.json | dotnet user-secrets set
  ```

# <a name="linux--macos"></a>[<span data-ttu-id="6dd2a-172">Linux / macOS</span><span class="sxs-lookup"><span data-stu-id="6dd2a-172">Linux / macOS</span></span>](#tab/linux+macos)

<span data-ttu-id="6dd2a-173">開啟命令外殼,並執行以下命令:</span><span class="sxs-lookup"><span data-stu-id="6dd2a-173">Open a command shell, and execute the following command:</span></span>

  ```dotnetcli
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="6dd2a-174">存取機密</span><span class="sxs-lookup"><span data-stu-id="6dd2a-174">Access a secret</span></span>

<span data-ttu-id="6dd2a-175">[ASP.NET核心配置 API](xref:fundamentals/configuration/index)提供對機密管理器機密的訪問。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-175">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span>

<span data-ttu-id="6dd2a-176">在 ASP.NET Core 2.0 或更高版本中,<xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>當專案調用 使用預配置預設值初始化主機的新實例時,使用者機密配置源將自動在開發模式下添加。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-176">In ASP.NET Core 2.0 or later, the user secrets configuration source is automatically added in development mode when the project calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A> to initialize a new instance of the host with preconfigured defaults.</span></span> <span data-ttu-id="6dd2a-177">`CreateDefaultBuilder`當<xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A><xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName>為<xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>時呼叫 :</span><span class="sxs-lookup"><span data-stu-id="6dd2a-177">`CreateDefaultBuilder` calls <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> when the <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName> is <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>:</span></span>

[!code-csharp[](app-secrets/samples/3.x/UserSecrets/Program.cs?name=snippet_CreateHostBuilder&highlight=2)]

<span data-ttu-id="6dd2a-178">未`CreateDefaultBuilder`調用時,通過在<xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A>`Startup`建構函數中調用顯式添加使用者機密配置源。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-178">When `CreateDefaultBuilder` isn't called, add the user secrets configuration source explicitly by calling <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> in the `Startup` constructor.</span></span> <span data-ttu-id="6dd2a-179">僅在`AddUserSecrets`應用在開發環境中運行時呼叫,如以下範例所示:</span><span class="sxs-lookup"><span data-stu-id="6dd2a-179">Call `AddUserSecrets` only when the app runs in the Development environment, as shown in the following example:</span></span>

[!code-csharp[](app-secrets/samples/3.x/UserSecrets/Startup2.cs?name=snippet_StartupConstructor&highlight=12)]

<span data-ttu-id="6dd2a-180">可透過`Configuration`API 檢索使用者機密:</span><span class="sxs-lookup"><span data-stu-id="6dd2a-180">User secrets can be retrieved via the `Configuration` API:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]


## <a name="map-secrets-to-a-poco"></a><span data-ttu-id="6dd2a-181">將機密映射到 POCO</span><span class="sxs-lookup"><span data-stu-id="6dd2a-181">Map secrets to a POCO</span></span>

<span data-ttu-id="6dd2a-182">將整個物件文字映射到 POCO(具有屬性的簡單 .NET 類)可用於聚合相關屬性。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-182">Mapping an entire object literal to a POCO (a simple .NET class with properties) is useful for aggregating related properties.</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="6dd2a-183">要將上述機密映射到 POCO,請`Configuration`使用 API[的物件圖形綁定](xref:fundamentals/configuration/index#bind-to-an-object-graph)功能。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-183">To map the preceding secrets to a POCO, use the `Configuration` API's [object graph binding](xref:fundamentals/configuration/index#bind-to-an-object-graph) feature.</span></span> <span data-ttu-id="6dd2a-184">以下代碼繫結為自訂`MovieSettings`POCO`ServiceApiKey`並造訪 屬性值:</span><span class="sxs-lookup"><span data-stu-id="6dd2a-184">The following code binds to a custom `MovieSettings` POCO and accesses the `ServiceApiKey` property value:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

<span data-ttu-id="6dd2a-185">與`Movies:ConnectionString``Movies:ServiceApiKey`機密映射到`MovieSettings`的屬性:</span><span class="sxs-lookup"><span data-stu-id="6dd2a-185">The `Movies:ConnectionString` and `Movies:ServiceApiKey` secrets are mapped to the respective properties in `MovieSettings`:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="6dd2a-186">字串取代與機密</span><span class="sxs-lookup"><span data-stu-id="6dd2a-186">String replacement with secrets</span></span>

<span data-ttu-id="6dd2a-187">以純文本形式儲存密碼不安全。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-187">Storing passwords in plain text is insecure.</span></span> <span data-ttu-id="6dd2a-188">例如,儲存在*appsettings.json*中的資料庫連接字串可能包含指定使用者的密碼:</span><span class="sxs-lookup"><span data-stu-id="6dd2a-188">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="6dd2a-189">更安全的方法是將密碼存儲為機密。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-189">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="6dd2a-190">例如：</span><span class="sxs-lookup"><span data-stu-id="6dd2a-190">For example:</span></span>

```dotnetcli
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="6dd2a-191">從`Password`*appsettings.json*中的連接字串中刪除鍵值對。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-191">Remove the `Password` key-value pair from the connection string in *appsettings.json*.</span></span> <span data-ttu-id="6dd2a-192">例如：</span><span class="sxs-lookup"><span data-stu-id="6dd2a-192">For example:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="6dd2a-193">可以在<xref:System.Data.SqlClient.SqlConnectionStringBuilder><xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password%2A>物件屬性上設定機密的值以完成連接字串:</span><span class="sxs-lookup"><span data-stu-id="6dd2a-193">The secret's value can be set on a <xref:System.Data.SqlClient.SqlConnectionStringBuilder> object's <xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password%2A> property to complete the connection string:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

## <a name="list-the-secrets"></a><span data-ttu-id="6dd2a-194">列出機密</span><span class="sxs-lookup"><span data-stu-id="6dd2a-194">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="6dd2a-195">從*存在 .csproj*檔案的目錄執行以下指令:</span><span class="sxs-lookup"><span data-stu-id="6dd2a-195">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets list
```

<span data-ttu-id="6dd2a-196">即會出現下列輸出：</span><span class="sxs-lookup"><span data-stu-id="6dd2a-196">The following output appears:</span></span>

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

<span data-ttu-id="6dd2a-197">在前面的範例中,鍵名稱中的冒號表示機密中的物件層次結構 *。*</span><span class="sxs-lookup"><span data-stu-id="6dd2a-197">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="6dd2a-198">移除單一機密</span><span class="sxs-lookup"><span data-stu-id="6dd2a-198">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="6dd2a-199">從*存在 .csproj*檔案的目錄執行以下指令:</span><span class="sxs-lookup"><span data-stu-id="6dd2a-199">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="6dd2a-200">套用的*機密.json*檔已修改以`MoviesConnectionString`刪除與 金鑰關聯的鍵值對:</span><span class="sxs-lookup"><span data-stu-id="6dd2a-200">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="6dd2a-201">`dotnet user-secrets list`顯示以下訊息:</span><span class="sxs-lookup"><span data-stu-id="6dd2a-201">`dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="6dd2a-202">移除所有機密</span><span class="sxs-lookup"><span data-stu-id="6dd2a-202">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="6dd2a-203">從*存在 .csproj*檔案的目錄執行以下指令:</span><span class="sxs-lookup"><span data-stu-id="6dd2a-203">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets clear
```

<span data-ttu-id="6dd2a-204">應用程式的所有使用者機密已從*機密.json*檔案中刪除:</span><span class="sxs-lookup"><span data-stu-id="6dd2a-204">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="6dd2a-205">執行`dotnet user-secrets list`顯示以下訊息:</span><span class="sxs-lookup"><span data-stu-id="6dd2a-205">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="6dd2a-206">其他資源</span><span class="sxs-lookup"><span data-stu-id="6dd2a-206">Additional resources</span></span>

* <span data-ttu-id="6dd2a-207">有關從 IIS 存取機密管理員的資訊,請參閱[此問題](https://github.com/dotnet/AspNetCore.Docs/issues/16328)。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-207">See [this issue](https://github.com/dotnet/AspNetCore.Docs/issues/16328) for information on accessing Secret Manager from IIS.</span></span>
* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="6dd2a-208">由[里克·安德森](https://twitter.com/RickAndMSFT),[丹尼爾·羅斯](https://github.com/danroth27)和[斯科特·艾迪](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="6dd2a-208">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="6dd2a-209">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/app-secrets/samples)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6dd2a-209">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/app-secrets/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="6dd2a-210">本文件介紹在開發計算機上開發ASP.NET酷睿應用期間存儲和檢索敏感數據的技術。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-210">This document explains techniques for storing and retrieving sensitive data during development of an ASP.NET Core app on a development machine.</span></span> <span data-ttu-id="6dd2a-211">切勿在原始碼中儲存密碼或其他敏感資料。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-211">Never store passwords or other sensitive data in source code.</span></span> <span data-ttu-id="6dd2a-212">生產機密不應用於開發或測試。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-212">Production secrets shouldn't be used for development or test.</span></span> <span data-ttu-id="6dd2a-213">不應將機密隨應用一起部署。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-213">Secrets shouldn't be deployed with the app.</span></span> <span data-ttu-id="6dd2a-214">相反,應通過受控方式(如環境變數、Azure 密鑰保管庫等)在生產環境中提供機密。您可以使用[Azure 密鑰保管庫配置提供程式](xref:security/key-vault-configuration)儲存和保護 Azure 測試和生產機密。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-214">Instead, secrets should be made available in the production environment through a controlled means like environment variables, Azure Key Vault, etc. You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="6dd2a-215">環境變數</span><span class="sxs-lookup"><span data-stu-id="6dd2a-215">Environment variables</span></span>

<span data-ttu-id="6dd2a-216">環境變數用於避免在代碼或本地配置檔中存儲應用機密。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-216">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="6dd2a-217">環境變數覆蓋所有以前指定的配置源的配置值。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-217">Environment variables override configuration values for all previously specified configuration sources.</span></span>

<span data-ttu-id="6dd2a-218">請考慮一個ASP.NET核心 Web 應用,其中啟用**了單個使用者帳戶**安全性。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-218">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="6dd2a-219">預設資料庫連接字串包含在項目的*appsettings.json*檔中,其中包含金`DefaultConnection`鑰 。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-219">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="6dd2a-220">預設連接字串用於 LocalDB,該字串在使用者模式下運行,不需要密碼。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-220">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="6dd2a-221">在應用部署期間,`DefaultConnection`可以使用環境變數的值重寫鍵值。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-221">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="6dd2a-222">環境變數可能將完整的連接字串存儲為具有敏感認證。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-222">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="6dd2a-223">環境變數通常以純、未加密的文本存儲。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-223">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="6dd2a-224">如果電腦或進程遭到破壞,則不受信任的方可以訪問環境變數。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-224">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="6dd2a-225">可能需要採取其他措施防止泄露用戶機密。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-225">Additional measures to prevent disclosure of user secrets may be required.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="secret-manager"></a><span data-ttu-id="6dd2a-226">秘密經理</span><span class="sxs-lookup"><span data-stu-id="6dd2a-226">Secret Manager</span></span>

<span data-ttu-id="6dd2a-227">在開發ASP.NET核心專案期間,秘密管理器工具存儲敏感數據。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-227">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="6dd2a-228">在此上下文中,一段敏感數據是應用機密。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-228">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="6dd2a-229">應用機密存儲在與專案樹不同的位置。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-229">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="6dd2a-230">應用機密與特定專案關聯或跨多個項目共用。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-230">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="6dd2a-231">應用機密不會簽入原始程式碼管理。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-231">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="6dd2a-232">秘密管理員工具不加密儲存的秘密,不應被視爲受信任的儲存。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-232">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="6dd2a-233">它僅用於開發目的。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-233">It's for development purposes only.</span></span> <span data-ttu-id="6dd2a-234">鍵和值存儲在使用者配置檔目錄中的 JSON 配置檔中。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-234">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="6dd2a-235">秘密管理員工具的工作原理</span><span class="sxs-lookup"><span data-stu-id="6dd2a-235">How the Secret Manager tool works</span></span>

<span data-ttu-id="6dd2a-236">"秘密管理員"工具會抽象出實現詳細資訊,例如值的存儲位置和方式。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-236">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="6dd2a-237">您可以在不知道這些實現詳細資訊的情況下使用該工具。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-237">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="6dd2a-238">這些值儲存在本地電腦上的受系統保護的使用者設定檔案資料夾中的 JSON 設定檔中:</span><span class="sxs-lookup"><span data-stu-id="6dd2a-238">The values are stored in a JSON configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windows"></a>[<span data-ttu-id="6dd2a-239">Windows</span><span class="sxs-lookup"><span data-stu-id="6dd2a-239">Windows</span></span>](#tab/windows)

<span data-ttu-id="6dd2a-240">檔案系統路徑:</span><span class="sxs-lookup"><span data-stu-id="6dd2a-240">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="linux--macos"></a>[<span data-ttu-id="6dd2a-241">Linux / macOS</span><span class="sxs-lookup"><span data-stu-id="6dd2a-241">Linux / macOS</span></span>](#tab/linux+macos)

<span data-ttu-id="6dd2a-242">檔案系統路徑:</span><span class="sxs-lookup"><span data-stu-id="6dd2a-242">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="6dd2a-243">在前面的檔路徑中,替換為`<user_secrets_id>``UserSecretsId` *.csproj*檔中指定的值。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-243">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="6dd2a-244">不要編寫依賴於使用機密管理員工具保存的資料的位置或格式的代碼。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-244">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="6dd2a-245">這些實現詳細資訊可能會更改。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-245">These implementation details may change.</span></span> <span data-ttu-id="6dd2a-246">例如,機密值未加密,但將來可能已加密。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-246">For example, the secret values aren't encrypted, but could be in the future.</span></span>

## <a name="enable-secret-storage"></a><span data-ttu-id="6dd2a-247">啟用機密儲存</span><span class="sxs-lookup"><span data-stu-id="6dd2a-247">Enable secret storage</span></span>

<span data-ttu-id="6dd2a-248">機密管理員「工具可對存儲在使用者設定檔中的特定於專案的設定設定進行操作。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-248">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span>

<span data-ttu-id="6dd2a-249">要使用使用者機密,請定義`UserSecretsId``PropertyGroup` *.csproj*檔中的元素。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-249">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="6dd2a-250">的內部`UserSecretsId`文本是任意的,但對於專案是唯一的。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-250">The inner text of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="6dd2a-251">開發人員通常為生成`UserSecretsId`GUID。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-251">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

> [!TIP]
> <span data-ttu-id="6dd2a-252">在 Visual Studio 中,右鍵單擊解決方案資源管理器中的專案,然後從上下文菜單中選擇 **「管理使用者機密**」。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-252">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="6dd2a-253">此手勢將一`UserSecretsId`個使用 GUID 填充的元素添加到 *.csproj*檔中。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-253">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span>

## <a name="set-a-secret"></a><span data-ttu-id="6dd2a-254">設定機密</span><span class="sxs-lookup"><span data-stu-id="6dd2a-254">Set a secret</span></span>

<span data-ttu-id="6dd2a-255">定義由鍵及其值組成的應用機密。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-255">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="6dd2a-256">機密與項目`UserSecretsId`的值相關聯。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-256">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="6dd2a-257">例如,從*存在 .csproj*檔的目錄中執行以下命令:</span><span class="sxs-lookup"><span data-stu-id="6dd2a-257">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="6dd2a-258">在前面的示例中,冒號表示`Movies`是`ServiceApiKey`具有 屬性的物件文本。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-258">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="6dd2a-259">機密管理器工具也可以從其他目錄使用。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-259">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="6dd2a-260">使用`--project`選項提供*存在 .csproj*檔案的檔案系統路徑。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-260">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="6dd2a-261">例如：</span><span class="sxs-lookup"><span data-stu-id="6dd2a-261">For example:</span></span>

```dotnetcli
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

### <a name="json-structure-flattening-in-visual-studio"></a><span data-ttu-id="6dd2a-262">視覺工作室中的 JSON 結構拼合</span><span class="sxs-lookup"><span data-stu-id="6dd2a-262">JSON structure flattening in Visual Studio</span></span>

<span data-ttu-id="6dd2a-263">可視化工作室的 **「管理使用者機密**」手勢將在文字編輯器中打開*一個機密.json*檔。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-263">Visual Studio's **Manage User Secrets** gesture opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="6dd2a-264">將*機密*內容替換為要存儲的鍵值對。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-264">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="6dd2a-265">例如：</span><span class="sxs-lookup"><span data-stu-id="6dd2a-265">For example:</span></span>

```json
{
  "Movies": {
    "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="6dd2a-266">通過`dotnet user-secrets remove``dotnet user-secrets set`或 進行修改後,JSON 結構將展平。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-266">The JSON structure is flattened after modifications via `dotnet user-secrets remove` or `dotnet user-secrets set`.</span></span> <span data-ttu-id="6dd2a-267">例如,運行`dotnet user-secrets remove "Movies:ConnectionString"`將`Movies`摺疊 物件文本。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-267">For example, running `dotnet user-secrets remove "Movies:ConnectionString"` collapses the `Movies` object literal.</span></span> <span data-ttu-id="6dd2a-268">變更後的檔案類似於以下內容:</span><span class="sxs-lookup"><span data-stu-id="6dd2a-268">The modified file resembles the following:</span></span>

```json
{
  "Movies:ServiceApiKey": "12345"
}
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="6dd2a-269">設定多個機密</span><span class="sxs-lookup"><span data-stu-id="6dd2a-269">Set multiple secrets</span></span>

<span data-ttu-id="6dd2a-270">通過將 JSON`set`管道到 命令,可以設置一批機密。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-270">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="6dd2a-271">在下面的範例中 *,input.json*檔的內容被傳`set`送到命令 。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-271">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windows"></a>[<span data-ttu-id="6dd2a-272">Windows</span><span class="sxs-lookup"><span data-stu-id="6dd2a-272">Windows</span></span>](#tab/windows)

<span data-ttu-id="6dd2a-273">開啟命令外殼,並執行以下命令:</span><span class="sxs-lookup"><span data-stu-id="6dd2a-273">Open a command shell, and execute the following command:</span></span>

  ```dotnetcli
  type .\input.json | dotnet user-secrets set
  ```

# <a name="linux--macos"></a>[<span data-ttu-id="6dd2a-274">Linux / macOS</span><span class="sxs-lookup"><span data-stu-id="6dd2a-274">Linux / macOS</span></span>](#tab/linux+macos)

<span data-ttu-id="6dd2a-275">開啟命令外殼,並執行以下命令:</span><span class="sxs-lookup"><span data-stu-id="6dd2a-275">Open a command shell, and execute the following command:</span></span>

  ```dotnetcli
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="6dd2a-276">存取機密</span><span class="sxs-lookup"><span data-stu-id="6dd2a-276">Access a secret</span></span>

<span data-ttu-id="6dd2a-277">[ASP.NET核心配置 API](xref:fundamentals/configuration/index)提供對機密管理器機密的訪問。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-277">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span>

<span data-ttu-id="6dd2a-278">如果項目的目標是 .NET 框架,請安裝[Microsoft.擴展.配置.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-278">If your project targets .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>


<span data-ttu-id="6dd2a-279">在 ASP.NET Core 2.0 或更高版本中,<xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>當專案調用 使用預配置預設值初始化主機的新實例時,使用者機密配置源將自動在開發模式下添加。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-279">In ASP.NET Core 2.0 or later, the user secrets configuration source is automatically added in development mode when the project calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A> to initialize a new instance of the host with preconfigured defaults.</span></span> <span data-ttu-id="6dd2a-280">`CreateDefaultBuilder`當<xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A><xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName>為<xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>時呼叫 :</span><span class="sxs-lookup"><span data-stu-id="6dd2a-280">`CreateDefaultBuilder` calls <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> when the <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName> is <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]


<span data-ttu-id="6dd2a-281">未`CreateDefaultBuilder`調用時,通過在<xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A>`Startup`建構函數中調用顯式添加使用者機密配置源。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-281">When `CreateDefaultBuilder` isn't called, add the user secrets configuration source explicitly by calling <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> in the `Startup` constructor.</span></span> <span data-ttu-id="6dd2a-282">僅在`AddUserSecrets`應用在開發環境中運行時呼叫,如以下範例所示:</span><span class="sxs-lookup"><span data-stu-id="6dd2a-282">Call `AddUserSecrets` only when the app runs in the Development environment, as shown in the following example:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

<span data-ttu-id="6dd2a-283">可透過`Configuration`API 檢索使用者機密:</span><span class="sxs-lookup"><span data-stu-id="6dd2a-283">User secrets can be retrieved via the `Configuration` API:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

## <a name="map-secrets-to-a-poco"></a><span data-ttu-id="6dd2a-284">將機密映射到 POCO</span><span class="sxs-lookup"><span data-stu-id="6dd2a-284">Map secrets to a POCO</span></span>

<span data-ttu-id="6dd2a-285">將整個物件文字映射到 POCO(具有屬性的簡單 .NET 類)可用於聚合相關屬性。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-285">Mapping an entire object literal to a POCO (a simple .NET class with properties) is useful for aggregating related properties.</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="6dd2a-286">要將上述機密映射到 POCO,請`Configuration`使用 API[的物件圖形綁定](xref:fundamentals/configuration/index#bind-to-an-object-graph)功能。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-286">To map the preceding secrets to a POCO, use the `Configuration` API's [object graph binding](xref:fundamentals/configuration/index#bind-to-an-object-graph) feature.</span></span> <span data-ttu-id="6dd2a-287">以下代碼繫結為自訂`MovieSettings`POCO`ServiceApiKey`並造訪 屬性值:</span><span class="sxs-lookup"><span data-stu-id="6dd2a-287">The following code binds to a custom `MovieSettings` POCO and accesses the `ServiceApiKey` property value:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

<span data-ttu-id="6dd2a-288">與`Movies:ConnectionString``Movies:ServiceApiKey`機密映射到`MovieSettings`的屬性:</span><span class="sxs-lookup"><span data-stu-id="6dd2a-288">The `Movies:ConnectionString` and `Movies:ServiceApiKey` secrets are mapped to the respective properties in `MovieSettings`:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="6dd2a-289">字串取代與機密</span><span class="sxs-lookup"><span data-stu-id="6dd2a-289">String replacement with secrets</span></span>

<span data-ttu-id="6dd2a-290">以純文本形式儲存密碼不安全。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-290">Storing passwords in plain text is insecure.</span></span> <span data-ttu-id="6dd2a-291">例如,儲存在*appsettings.json*中的資料庫連接字串可能包含指定使用者的密碼:</span><span class="sxs-lookup"><span data-stu-id="6dd2a-291">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="6dd2a-292">更安全的方法是將密碼存儲為機密。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-292">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="6dd2a-293">例如：</span><span class="sxs-lookup"><span data-stu-id="6dd2a-293">For example:</span></span>

```dotnetcli
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="6dd2a-294">從`Password`*appsettings.json*中的連接字串中刪除鍵值對。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-294">Remove the `Password` key-value pair from the connection string in *appsettings.json*.</span></span> <span data-ttu-id="6dd2a-295">例如：</span><span class="sxs-lookup"><span data-stu-id="6dd2a-295">For example:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="6dd2a-296">可以在<xref:System.Data.SqlClient.SqlConnectionStringBuilder><xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password%2A>物件屬性上設定機密的值以完成連接字串:</span><span class="sxs-lookup"><span data-stu-id="6dd2a-296">The secret's value can be set on a <xref:System.Data.SqlClient.SqlConnectionStringBuilder> object's <xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password%2A> property to complete the connection string:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

## <a name="list-the-secrets"></a><span data-ttu-id="6dd2a-297">列出機密</span><span class="sxs-lookup"><span data-stu-id="6dd2a-297">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="6dd2a-298">從*存在 .csproj*檔案的目錄執行以下指令:</span><span class="sxs-lookup"><span data-stu-id="6dd2a-298">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets list
```

<span data-ttu-id="6dd2a-299">即會出現下列輸出：</span><span class="sxs-lookup"><span data-stu-id="6dd2a-299">The following output appears:</span></span>

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

<span data-ttu-id="6dd2a-300">在前面的範例中,鍵名稱中的冒號表示機密中的物件層次結構 *。*</span><span class="sxs-lookup"><span data-stu-id="6dd2a-300">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="6dd2a-301">移除單一機密</span><span class="sxs-lookup"><span data-stu-id="6dd2a-301">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="6dd2a-302">從*存在 .csproj*檔案的目錄執行以下指令:</span><span class="sxs-lookup"><span data-stu-id="6dd2a-302">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="6dd2a-303">套用的*機密.json*檔已修改以`MoviesConnectionString`刪除與 金鑰關聯的鍵值對:</span><span class="sxs-lookup"><span data-stu-id="6dd2a-303">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="6dd2a-304">執行`dotnet user-secrets list`顯示以下訊息:</span><span class="sxs-lookup"><span data-stu-id="6dd2a-304">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="6dd2a-305">移除所有機密</span><span class="sxs-lookup"><span data-stu-id="6dd2a-305">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="6dd2a-306">從*存在 .csproj*檔案的目錄執行以下指令:</span><span class="sxs-lookup"><span data-stu-id="6dd2a-306">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets clear
```

<span data-ttu-id="6dd2a-307">應用程式的所有使用者機密已從*機密.json*檔案中刪除:</span><span class="sxs-lookup"><span data-stu-id="6dd2a-307">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="6dd2a-308">執行`dotnet user-secrets list`顯示以下訊息:</span><span class="sxs-lookup"><span data-stu-id="6dd2a-308">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="6dd2a-309">其他資源</span><span class="sxs-lookup"><span data-stu-id="6dd2a-309">Additional resources</span></span>

* <span data-ttu-id="6dd2a-310">有關從 IIS 存取機密管理員的資訊,請參閱[此問題](https://github.com/dotnet/AspNetCore.Docs/issues/16328)。</span><span class="sxs-lookup"><span data-stu-id="6dd2a-310">See [this issue](https://github.com/dotnet/AspNetCore.Docs/issues/16328) for information on accessing Secret Manager from IIS.</span></span>
* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>

::: moniker-end