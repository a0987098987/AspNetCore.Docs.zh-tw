---
title: "安全儲存體的 ASP.NET Core 在開發期間的應用程式密碼"
author: rick-anderson
description: "示範如何安全地將密碼儲存在開發期間"
manager: wpickett
ms.author: riande
ms.date: 09/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/app-secrets
ms.openlocfilehash: 337782a0530a37916b04aa562174b5921ddbc46b
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="safe-storage-of-app-secrets-during-development-in-aspnet-core"></a><span data-ttu-id="37a0c-103">安全儲存體的 ASP.NET Core 在開發期間的應用程式密碼</span><span class="sxs-lookup"><span data-stu-id="37a0c-103">Safe storage of app secrets during development in ASP.NET Core</span></span>

<span data-ttu-id="37a0c-104">由[Rick Anderson](https://twitter.com/RickAndMSFT)，[奧 Roth](https://github.com/danroth27)，和[Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="37a0c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://scottaddie.com)</span></span> 

<span data-ttu-id="37a0c-105">本文件示範如何使用密碼管理員工具在開發程式碼保持機密資料。</span><span class="sxs-lookup"><span data-stu-id="37a0c-105">This document shows how you can use the Secret Manager tool in development to keep secrets out of your code.</span></span> <span data-ttu-id="37a0c-106">您應該永遠不會儲存密碼或其他機密資料來源的程式碼，而您不應該在開發和測試模式中使用實際執行的密碼最重要的一點。</span><span class="sxs-lookup"><span data-stu-id="37a0c-106">The most important point is you should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development and test mode.</span></span> <span data-ttu-id="37a0c-107">您可以改為使用[組態](xref:fundamentals/configuration/index)系統環境變數中讀取這些值，或使用密碼管理員儲存的值從工具。</span><span class="sxs-lookup"><span data-stu-id="37a0c-107">You can instead use the [configuration](xref:fundamentals/configuration/index) system to read these values from environment variables or from values stored using the Secret Manager tool.</span></span> <span data-ttu-id="37a0c-108">密碼管理員工具可協助防止機密資料被簽入原始檔控制。</span><span class="sxs-lookup"><span data-stu-id="37a0c-108">The Secret Manager tool helps prevent sensitive data from being checked into source control.</span></span> <span data-ttu-id="37a0c-109">[組態](xref:fundamentals/configuration/index)系統可以讀取儲存在本文中所描述的密碼管理員工具與密碼。</span><span class="sxs-lookup"><span data-stu-id="37a0c-109">The [configuration](xref:fundamentals/configuration/index) system can read secrets stored with the Secret Manager tool described in this article.</span></span>

<span data-ttu-id="37a0c-110">密碼管理員工具只能用於開發。</span><span class="sxs-lookup"><span data-stu-id="37a0c-110">The Secret Manager tool is used only in development.</span></span> <span data-ttu-id="37a0c-111">您可以保護 Azure 測試與實際的機密資料以[Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/)組態提供者。</span><span class="sxs-lookup"><span data-stu-id="37a0c-111">You can safeguard Azure test and production secrets with the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) configuration provider.</span></span> <span data-ttu-id="37a0c-112">請參閱[Azure 金鑰保存庫的組態提供者](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="37a0c-112">See [Azure Key Vault configuration provider](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration) for more information.</span></span>

## <a name="environment-variables"></a><span data-ttu-id="37a0c-113">環境變數</span><span class="sxs-lookup"><span data-stu-id="37a0c-113">Environment variables</span></span>

<span data-ttu-id="37a0c-114">若要避免在程式碼，或在本機的組態檔中儲存應用程式密碼，您可以將密碼儲存在環境變數中。</span><span class="sxs-lookup"><span data-stu-id="37a0c-114">To avoid storing app secrets in code or in local configuration files, you store secrets in environment variables.</span></span> <span data-ttu-id="37a0c-115">您可以設定[組態](xref:fundamentals/configuration/index)架構，以讀取環境變數中的值，藉由呼叫`AddEnvironmentVariables`。</span><span class="sxs-lookup"><span data-stu-id="37a0c-115">You can setup the [configuration](xref:fundamentals/configuration/index) framework to read values from environment variables by calling `AddEnvironmentVariables`.</span></span> <span data-ttu-id="37a0c-116">然後，您可以使用環境變數覆寫所有先前指定的設定來源的組態值。</span><span class="sxs-lookup"><span data-stu-id="37a0c-116">You can then use environment variables to override configuration values for all previously specified configuration sources.</span></span>

<span data-ttu-id="37a0c-117">例如，如果您建立新的 ASP.NET Core web 應用程式與個別使用者帳戶時，它會將新增的預設連接字串*appsettings.json*專案具有索引鍵中的檔案`DefaultConnection`。</span><span class="sxs-lookup"><span data-stu-id="37a0c-117">For example, if you create a new ASP.NET Core web app with individual user accounts, it will add a default connection string to the *appsettings.json* file in the project with the key `DefaultConnection`.</span></span> <span data-ttu-id="37a0c-118">預設的連接字串是安裝程式使用 LocalDB，但在使用者模式下執行，而不需要密碼。</span><span class="sxs-lookup"><span data-stu-id="37a0c-118">The default connection string is setup to use LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="37a0c-119">當您部署到測試或實際執行伺服器應用程式時，您可以覆寫`DefaultConnection`機碼值與包含測試或實際執行資料庫的連接字串 （潛在機密的認證） 的環境變數設定伺服器。</span><span class="sxs-lookup"><span data-stu-id="37a0c-119">When you deploy your application to a test or production server, you can override the `DefaultConnection` key value with an environment variable setting that contains the connection string (potentially with sensitive credentials) for a test or production database server.</span></span>

>[!WARNING]
> <span data-ttu-id="37a0c-120">環境變數通常會以純文字儲存，不會加密。</span><span class="sxs-lookup"><span data-stu-id="37a0c-120">Environment variables are generally stored in plain text and are not encrypted.</span></span> <span data-ttu-id="37a0c-121">如果電腦或處理序遭到入侵，然後可以在不受信任的合作對象來存取環境變數。</span><span class="sxs-lookup"><span data-stu-id="37a0c-121">If the machine or process is compromised, then environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="37a0c-122">可能仍然需要其他措施以避免洩露使用者密碼。</span><span class="sxs-lookup"><span data-stu-id="37a0c-122">Additional measures to prevent disclosure of user secrets may still be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="37a0c-123">密碼管理員</span><span class="sxs-lookup"><span data-stu-id="37a0c-123">Secret Manager</span></span>

<span data-ttu-id="37a0c-124">密碼管理員工具會儲存為您專案的樹狀目錄外部的開發工作的敏感性資料。</span><span class="sxs-lookup"><span data-stu-id="37a0c-124">The Secret Manager tool stores sensitive data for development work outside of your project tree.</span></span> <span data-ttu-id="37a0c-125">密碼管理員工具是一種專案的工具，可以用來儲存的秘密資訊[.NET Core](https://www.microsoft.com/net/core)在開發期間的專案。</span><span class="sxs-lookup"><span data-stu-id="37a0c-125">The Secret Manager tool is a project tool that can be used to store secrets for a [.NET Core](https://www.microsoft.com/net/core) project during development.</span></span> <span data-ttu-id="37a0c-126">使用密碼管理員 工具中，您可以將應用程式密碼與特定的專案產生關聯，並共用跨多個專案。</span><span class="sxs-lookup"><span data-stu-id="37a0c-126">With the Secret Manager tool, you can associate app secrets with a specific project and share them across multiple projects.</span></span>

>[!WARNING]
> <span data-ttu-id="37a0c-127">密碼管理員工具不會加密預存機密資料，而且不應該被視為受信任存放區。</span><span class="sxs-lookup"><span data-stu-id="37a0c-127">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="37a0c-128">它是僅限開發用途。</span><span class="sxs-lookup"><span data-stu-id="37a0c-128">It's for development purposes only.</span></span> <span data-ttu-id="37a0c-129">索引鍵和值會儲存在使用者設定檔的目錄中的 JSON 組態檔。</span><span class="sxs-lookup"><span data-stu-id="37a0c-129">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="installing-the-secret-manager-tool"></a><span data-ttu-id="37a0c-130">安裝密碼管理員工具</span><span class="sxs-lookup"><span data-stu-id="37a0c-130">Installing the Secret Manager tool</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="37a0c-131">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="37a0c-131">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="37a0c-132">以滑鼠右鍵按一下方案總管 中的專案，然後選取**編輯\<project_name\>.csproj**從內容功能表。</span><span class="sxs-lookup"><span data-stu-id="37a0c-132">Right-click the project in Solution Explorer, and select **Edit \<project_name\>.csproj** from the context menu.</span></span> <span data-ttu-id="37a0c-133">將反白顯示的行加入*.csproj*檔案，並將儲存到還原相關聯的 NuGet 套件：</span><span class="sxs-lookup"><span data-stu-id="37a0c-133">Add the highlighted line to the *.csproj* file, and save to restore the associated NuGet package:</span></span>

[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets-before.csproj?highlight=10)]

<span data-ttu-id="37a0c-134">以滑鼠右鍵按一下方案總管 中的專案，然後選取**管理使用者密碼**從內容功能表。</span><span class="sxs-lookup"><span data-stu-id="37a0c-134">Right-click the project in Solution Explorer again, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="37a0c-135">此筆勢加入新`UserSecretsId`節點內`PropertyGroup`的*.csproj*檔案，以反白顯示在下列範例中：</span><span class="sxs-lookup"><span data-stu-id="37a0c-135">This gesture adds a new `UserSecretsId` node within a `PropertyGroup` of the *.csproj* file, as highlighted in the following sample:</span></span>

[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets-after.csproj?highlight=4)]

<span data-ttu-id="37a0c-136">儲存已修改*.csproj*檔案也會開啟`secrets.json`文字編輯器中的檔案。</span><span class="sxs-lookup"><span data-stu-id="37a0c-136">Saving the modified *.csproj* file also opens a `secrets.json` file in the text editor.</span></span> <span data-ttu-id="37a0c-137">取代內容`secrets.json`以下列程式碼檔案：</span><span class="sxs-lookup"><span data-stu-id="37a0c-137">Replace the contents of the `secrets.json` file with the following code:</span></span>

```json
{
    "MySecret": "ValueOfMySecret"
}
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="37a0c-138">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="37a0c-138">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="37a0c-139">新增`Microsoft.Extensions.SecretManager.Tools`至*.csproj*檔，然後執行`dotnet restore`。</span><span class="sxs-lookup"><span data-stu-id="37a0c-139">Add `Microsoft.Extensions.SecretManager.Tools` to the *.csproj* file and run `dotnet restore`.</span></span> <span data-ttu-id="37a0c-140">若要安裝的命令列使用的密碼管理員工具，您可以使用相同的步驟。</span><span class="sxs-lookup"><span data-stu-id="37a0c-140">You can use the same steps to install the Secret Manager Tool using for the command line.</span></span>

[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets-before.csproj?highlight=10)]

<span data-ttu-id="37a0c-141">測試密碼管理員工具執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="37a0c-141">Test the Secret Manager tool by running the following command:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="37a0c-142">密碼管理員工具會顯示使用方式、 選項和命令的說明。</span><span class="sxs-lookup"><span data-stu-id="37a0c-142">The Secret Manager tool will display usage, options and command help.</span></span>

> [!NOTE]
> <span data-ttu-id="37a0c-143">您必須在相同的目錄*.csproj*檔案來執行工具中定義*.csproj*檔案的`DotNetCliToolReference`節點。</span><span class="sxs-lookup"><span data-stu-id="37a0c-143">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` nodes.</span></span>

<span data-ttu-id="37a0c-144">密碼管理員工具會依據儲存在您的使用者設定檔中的專案特定的組態設定。</span><span class="sxs-lookup"><span data-stu-id="37a0c-144">The Secret Manager tool operates on project-specific configuration settings that are stored in your user profile.</span></span> <span data-ttu-id="37a0c-145">若要使用使用者密碼，必須指定專案`UserSecretsId`值在其*.csproj*檔案。</span><span class="sxs-lookup"><span data-stu-id="37a0c-145">To use user secrets, the project must specify a `UserSecretsId` value in its *.csproj* file.</span></span> <span data-ttu-id="37a0c-146">值`UserSecretsId`是任意的但通常獨有的專案。</span><span class="sxs-lookup"><span data-stu-id="37a0c-146">The value of `UserSecretsId` is arbitrary, but is generally unique to the project.</span></span> <span data-ttu-id="37a0c-147">開發人員通常會產生的 GUID `UserSecretsId`。</span><span class="sxs-lookup"><span data-stu-id="37a0c-147">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

<span data-ttu-id="37a0c-148">新增`UserSecretsId`中的專案*.csproj*檔案：</span><span class="sxs-lookup"><span data-stu-id="37a0c-148">Add a `UserSecretsId` for your project in the *.csproj* file:</span></span>

[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets-after.csproj?highlight=4)]

<span data-ttu-id="37a0c-149">您可以使用密碼管理員工具來設定密碼。</span><span class="sxs-lookup"><span data-stu-id="37a0c-149">Use the Secret Manager tool to set a secret.</span></span> <span data-ttu-id="37a0c-150">例如，在命令視窗從專案目錄中，輸入下列內容：</span><span class="sxs-lookup"><span data-stu-id="37a0c-150">For example, in a command window from the project directory, enter the following:</span></span>

```console
dotnet user-secrets set MySecret ValueOfMySecret
```

<span data-ttu-id="37a0c-151">您可以執行密碼管理員工具，從其他目錄，但您必須使用`--project`傳遞的路徑中的選項*.csproj*檔案：</span><span class="sxs-lookup"><span data-stu-id="37a0c-151">You can run the Secret Manager tool from other directories, but you must use the `--project` option to pass in the path to the *.csproj* file:</span></span>
 
```console
dotnet user-secrets set MySecret ValueOfMySecret --project c:\work\WebApp1\src\webapp1
```

<span data-ttu-id="37a0c-152">您也可以使用密碼管理員工具來列出、 移除，並清除應用程式密碼。</span><span class="sxs-lookup"><span data-stu-id="37a0c-152">You can also use the Secret Manager tool to list, remove and clear app secrets.</span></span>

-----

## <a name="accessing-user-secrets-via-configuration"></a><span data-ttu-id="37a0c-153">透過設定存取的使用者密碼</span><span class="sxs-lookup"><span data-stu-id="37a0c-153">Accessing user secrets via configuration</span></span>

<span data-ttu-id="37a0c-154">透過組態系統存取的密碼管理員密碼。</span><span class="sxs-lookup"><span data-stu-id="37a0c-154">You access Secret Manager secrets through the configuration system.</span></span> <span data-ttu-id="37a0c-155">新增`Microsoft.Extensions.Configuration.UserSecrets`封裝及執行`dotnet restore`。</span><span class="sxs-lookup"><span data-stu-id="37a0c-155">Add the `Microsoft.Extensions.Configuration.UserSecrets` package and run `dotnet restore`.</span></span>

<span data-ttu-id="37a0c-156">將使用者密碼設定來源加入`Startup`方法：</span><span class="sxs-lookup"><span data-stu-id="37a0c-156">Add the user secrets configuration source to the `Startup` method:</span></span>

[!code-csharp[Main](app-secrets/sample/UserSecrets/Startup.cs?highlight=16-19)]

<span data-ttu-id="37a0c-157">您可以存取透過設定應用程式開發介面的使用者密碼：</span><span class="sxs-lookup"><span data-stu-id="37a0c-157">You can access user secrets via the configuration API:</span></span>

[!code-csharp[Main](app-secrets/sample/UserSecrets/Startup.cs?highlight=26-29)]

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="37a0c-158">密碼管理員工具的運作方式</span><span class="sxs-lookup"><span data-stu-id="37a0c-158">How the Secret Manager tool works</span></span>

<span data-ttu-id="37a0c-159">密碼管理員工具抽象化實作詳細資料，例如值儲存的位置和方式。</span><span class="sxs-lookup"><span data-stu-id="37a0c-159">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="37a0c-160">您可以使用此工具，而不需要知道這些實作細節。</span><span class="sxs-lookup"><span data-stu-id="37a0c-160">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="37a0c-161">在目前版本中，值會儲存在[JSON](http://json.org/)使用者設定檔的目錄中的組態檔：</span><span class="sxs-lookup"><span data-stu-id="37a0c-161">In the current version, the values are stored in a [JSON](http://json.org/) configuration file in the user profile directory:</span></span>

* <span data-ttu-id="37a0c-162">Windows:`%APPDATA%\microsoft\UserSecrets\<userSecretsId>\secrets.json`</span><span class="sxs-lookup"><span data-stu-id="37a0c-162">Windows: `%APPDATA%\microsoft\UserSecrets\<userSecretsId>\secrets.json`</span></span>

* <span data-ttu-id="37a0c-163">Linux:`~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span><span class="sxs-lookup"><span data-stu-id="37a0c-163">Linux: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span></span>

* <span data-ttu-id="37a0c-164">Mac:`~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span><span class="sxs-lookup"><span data-stu-id="37a0c-164">Mac: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span></span>

<span data-ttu-id="37a0c-165">值`userSecretsId`中指定的值是來自*.csproj*檔案。</span><span class="sxs-lookup"><span data-stu-id="37a0c-165">The value of `userSecretsId` comes from the value specified in *.csproj* file.</span></span>

<span data-ttu-id="37a0c-166">您不應撰寫程式碼所依賴這些實作細節，可能會變更位置或使用密碼管理員 工具中，儲存的資料格式。</span><span class="sxs-lookup"><span data-stu-id="37a0c-166">You shouldn't write code that depends on the location or format of the data saved with the Secret Manager tool, as these implementation details might change.</span></span> <span data-ttu-id="37a0c-167">例如，密碼值是目前*不*加密，但是也可能是吧。</span><span class="sxs-lookup"><span data-stu-id="37a0c-167">For example, the secret values are currently *not* encrypted today, but could be someday.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="37a0c-168">其他資源</span><span class="sxs-lookup"><span data-stu-id="37a0c-168">Additional resources</span></span>

* [<span data-ttu-id="37a0c-169">組態</span><span class="sxs-lookup"><span data-stu-id="37a0c-169">Configuration</span></span>](xref:fundamentals/configuration/index)
