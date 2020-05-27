---
<span data-ttu-id="2ef91-101">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-101">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-102">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-102">'Blazor'</span></span>
- <span data-ttu-id="2ef91-103">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-103">'Identity'</span></span>
- <span data-ttu-id="2ef91-104">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-104">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-105">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-105">'Razor'</span></span>
- <span data-ttu-id="2ef91-106">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-106">'SignalR' uid:</span></span> 

---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="2ef91-107">ASP.NET Core 的設定</span><span class="sxs-lookup"><span data-stu-id="2ef91-107">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="2ef91-108">由[Rick Anderson](https://twitter.com/RickAndMSFT)和[Kirk Larkin](https://twitter.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="2ef91-108">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Kirk Larkin](https://twitter.com/serpent5)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="2ef91-109">ASP.NET Core 中的設定是使用一或多個設定[提供者](#cp)來執行。</span><span class="sxs-lookup"><span data-stu-id="2ef91-109">Configuration in ASP.NET Core is performed using one or more [configuration providers](#cp).</span></span> <span data-ttu-id="2ef91-110">設定提供者會使用各種不同的設定來源，從機碼值組讀取設定資料：</span><span class="sxs-lookup"><span data-stu-id="2ef91-110">Configuration providers read configuration data from key-value pairs using a variety of configuration sources:</span></span>

* <span data-ttu-id="2ef91-111">設定檔案，例如*appsettings. json*</span><span class="sxs-lookup"><span data-stu-id="2ef91-111">Settings files, such as *appsettings.json*</span></span>
* <span data-ttu-id="2ef91-112">環境變數</span><span class="sxs-lookup"><span data-stu-id="2ef91-112">Environment variables</span></span>
* <span data-ttu-id="2ef91-113">Azure 金鑰保存庫</span><span class="sxs-lookup"><span data-stu-id="2ef91-113">Azure Key Vault</span></span>
* <span data-ttu-id="2ef91-114">Azure 應用程式組態</span><span class="sxs-lookup"><span data-stu-id="2ef91-114">Azure App Configuration</span></span>
* <span data-ttu-id="2ef91-115">命令列引數</span><span class="sxs-lookup"><span data-stu-id="2ef91-115">Command-line arguments</span></span>
* <span data-ttu-id="2ef91-116">已安裝或建立的自訂提供者</span><span class="sxs-lookup"><span data-stu-id="2ef91-116">Custom providers, installed or created</span></span>
* <span data-ttu-id="2ef91-117">目錄檔案</span><span class="sxs-lookup"><span data-stu-id="2ef91-117">Directory files</span></span>
* <span data-ttu-id="2ef91-118">記憶體內部 .NET 物件</span><span class="sxs-lookup"><span data-stu-id="2ef91-118">In-memory .NET objects</span></span>

<span data-ttu-id="2ef91-119">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="2ef91-119">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<a name="default"></a>

## <a name="default-configuration"></a><span data-ttu-id="2ef91-120">預設組態</span><span class="sxs-lookup"><span data-stu-id="2ef91-120">Default configuration</span></span>

<span data-ttu-id="2ef91-121">ASP.NET Core 以[dotnet new](/dotnet/core/tools/dotnet-new)或 Visual Studio 建立的 web 應用程式會產生下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="2ef91-121">ASP.NET Core web apps created with [dotnet new](/dotnet/core/tools/dotnet-new) or Visual Studio generate the following code:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Program.cs?name=snippet&highlight=9)]

 <span data-ttu-id="2ef91-122"><xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> 會以下列順序提供應用程式的預設組態：</span><span class="sxs-lookup"><span data-stu-id="2ef91-122"><xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> provides default configuration for the app in the following order:</span></span>

1. <span data-ttu-id="2ef91-123">[ChainedConfigurationProvider](xref:Microsoft.Extensions.Configuration.ChainedConfigurationSource) ：加入現有的 `IConfiguration` 做為來源。</span><span class="sxs-lookup"><span data-stu-id="2ef91-123">[ChainedConfigurationProvider](xref:Microsoft.Extensions.Configuration.ChainedConfigurationSource) :  Adds an existing `IConfiguration` as a source.</span></span> <span data-ttu-id="2ef91-124">在預設設定案例中，會新增[主機](#hvac)配置，並將其設為_應用程式_設定的第一個來源。</span><span class="sxs-lookup"><span data-stu-id="2ef91-124">In the default configuration case, adds the [host](#hvac) configuration and setting it as the first source for the _app_ configuration.</span></span>
1. <span data-ttu-id="2ef91-125">使用[json 設定提供者](#file-configuration-provider)的[appsettings。](#appsettingsjson)</span><span class="sxs-lookup"><span data-stu-id="2ef91-125">[appsettings.json](#appsettingsjson) using the [JSON configuration provider](#file-configuration-provider).</span></span>
1. <span data-ttu-id="2ef91-126">*appsettings。* `Environment`使用[json 設定提供者](#file-configuration-provider)的*json* 。</span><span class="sxs-lookup"><span data-stu-id="2ef91-126">*appsettings.*`Environment`*.json* using the [JSON configuration provider](#file-configuration-provider).</span></span> <span data-ttu-id="2ef91-127">例如， *appsettings*。***生產***。*json*和*appsettings*。***開發***。*json*。</span><span class="sxs-lookup"><span data-stu-id="2ef91-127">For example, *appsettings*.***Production***.*json* and *appsettings*.***Development***.*json*.</span></span>
1. <span data-ttu-id="2ef91-128">應用程式在環境中執行時的[密碼](xref:security/app-secrets) `Development` 。</span><span class="sxs-lookup"><span data-stu-id="2ef91-128">[App secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
1. <span data-ttu-id="2ef91-129">使用[環境變數設定提供者](#evcp)的環境變數。</span><span class="sxs-lookup"><span data-stu-id="2ef91-129">Environment variables using the [Environment Variables configuration provider](#evcp).</span></span>
1. <span data-ttu-id="2ef91-130">使用[命令列設定提供者](#command-line)的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="2ef91-130">Command-line arguments using the [Command-line configuration provider](#command-line).</span></span>

<span data-ttu-id="2ef91-131">稍後新增的設定提供者會覆寫先前的金鑰設定。</span><span class="sxs-lookup"><span data-stu-id="2ef91-131">Configuration providers that are added later override previous key settings.</span></span> <span data-ttu-id="2ef91-132">例如，如果 `MyKey` 同時在*appsettings*和環境中設定，則會使用環境值。</span><span class="sxs-lookup"><span data-stu-id="2ef91-132">For example, if `MyKey` is set in both *appsettings.json* and the environment, the environment value is used.</span></span> <span data-ttu-id="2ef91-133">使用預設的設定提供者時，[命令列設定提供者](#command-line-configuration-provider)會覆寫所有其他提供者。</span><span class="sxs-lookup"><span data-stu-id="2ef91-133">Using the default configuration providers, the  [Command-line configuration provider](#command-line-configuration-provider) overrides all other providers.</span></span>

<span data-ttu-id="2ef91-134">如需的詳細資訊 `CreateDefaultBuilder` ，請參閱預設產生器[設定](xref:fundamentals/host/generic-host#default-builder-settings)。</span><span class="sxs-lookup"><span data-stu-id="2ef91-134">For more information on `CreateDefaultBuilder`, see [Default builder settings](xref:fundamentals/host/generic-host#default-builder-settings).</span></span>

<span data-ttu-id="2ef91-135">下列程式碼會依新增的順序顯示已啟用的設定提供者：</span><span class="sxs-lookup"><span data-stu-id="2ef91-135">The following code displays the enabled configuration providers in the order they were added:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Index2.cshtml.cs?name=snippet)]

### <a name="appsettingsjson"></a><span data-ttu-id="2ef91-136">appsettings.json</span><span class="sxs-lookup"><span data-stu-id="2ef91-136">appsettings.json</span></span>

<span data-ttu-id="2ef91-137">請考慮下列*appsettings json*檔案：</span><span class="sxs-lookup"><span data-stu-id="2ef91-137">Consider the following *appsettings.json* file:</span></span>

[!code-json[](index/samples/3.x/ConfigSample/appsettings.json)]

<span data-ttu-id="2ef91-138">[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)中的下列程式碼會顯示上述幾個設定值：</span><span class="sxs-lookup"><span data-stu-id="2ef91-138">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) displays several of the preceding configurations settings:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

<span data-ttu-id="2ef91-139">預設會 <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> 以下列順序載入設定：</span><span class="sxs-lookup"><span data-stu-id="2ef91-139">The default <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration in the following order:</span></span>

1. <span data-ttu-id="2ef91-140">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="2ef91-140">*appsettings.json*</span></span>
1. <span data-ttu-id="2ef91-141">*appsettings。* `Environment`*. json* ：例如， *appsettings*。***生產***。*json*和*appsettings*。***開發***。*json*檔案。</span><span class="sxs-lookup"><span data-stu-id="2ef91-141">*appsettings.*`Environment`*.json* : For example, the *appsettings*.***Production***.*json* and *appsettings*.***Development***.*json* files.</span></span> <span data-ttu-id="2ef91-142">檔案的環境版本是根據[IHostingEnvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*)載入。</span><span class="sxs-lookup"><span data-stu-id="2ef91-142">The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span> <span data-ttu-id="2ef91-143">如需詳細資訊，請參閱<xref:fundamentals/environments>。</span><span class="sxs-lookup"><span data-stu-id="2ef91-143">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="2ef91-144">*appsettings*。 `Environment`*json*值會覆寫*appsettings*中的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="2ef91-144">*appsettings*.`Environment`.*json* values override keys in *appsettings.json*.</span></span> <span data-ttu-id="2ef91-145">例如，根據預設：</span><span class="sxs-lookup"><span data-stu-id="2ef91-145">For example, by default:</span></span>

* <span data-ttu-id="2ef91-146">在開發中， *appsettings*。***開發***。*json*設定會覆寫在*appsettings*中找到的值。</span><span class="sxs-lookup"><span data-stu-id="2ef91-146">In development, *appsettings*.***Development***.*json* configuration overwrites values found in *appsettings.json*.</span></span>
* <span data-ttu-id="2ef91-147">在生產環境中， *appsettings*。***生產***。*json*設定會覆寫在*appsettings*中找到的值。</span><span class="sxs-lookup"><span data-stu-id="2ef91-147">In production, *appsettings*.***Production***.*json* configuration overwrites values found in *appsettings.json*.</span></span> <span data-ttu-id="2ef91-148">例如，將應用程式部署至 Azure 時。</span><span class="sxs-lookup"><span data-stu-id="2ef91-148">For example, when deploying the app to Azure.</span></span>

<a name="optpat"></a>

### <a name="bind-hierarchical-configuration-data-using-the-options-pattern"></a><span data-ttu-id="2ef91-149">使用選項模式系結階層式設定資料</span><span class="sxs-lookup"><span data-stu-id="2ef91-149">Bind hierarchical configuration data using the options pattern</span></span>

[!INCLUDE[](~/includes/bind.md)]

<span data-ttu-id="2ef91-150">使用[預設](#default)設定，即*appsettings*和*appsettings。* `Environment`已啟用[reloadOnChange： true](https://github.com/dotnet/extensions/blob/release/3.1/src/Hosting/Hosting/src/Host.cs#L74-L75)的*json*檔案。</span><span class="sxs-lookup"><span data-stu-id="2ef91-150">Using the [default](#default) configuration, the *appsettings.json* and *appsettings.*`Environment`*.json* files are enabled with [reloadOnChange: true](https://github.com/dotnet/extensions/blob/release/3.1/src/Hosting/Hosting/src/Host.cs#L74-L75).</span></span> <span data-ttu-id="2ef91-151">對*appsettings*和 appsettings 所做的變更 *。* `Environment`應用程式啟動***後***的*json*檔案會由 json 設定[提供者](#jcp)讀取。</span><span class="sxs-lookup"><span data-stu-id="2ef91-151">Changes made to the *appsettings.json* and *appsettings.*`Environment`*.json* file ***after*** the app starts are read by the [JSON configuration provider](#jcp).</span></span>

<span data-ttu-id="2ef91-152">如需新增其他 JSON 設定檔的相關資訊，請參閱本檔中的[JSON 設定提供者](#jcp)。</span><span class="sxs-lookup"><span data-stu-id="2ef91-152">See [JSON configuration provider](#jcp) in this document for information on adding additional JSON configuration files.</span></span>

<a name="security"></a>

## <a name="security-and-secret-manager"></a><span data-ttu-id="2ef91-153">安全性和秘密管理員</span><span class="sxs-lookup"><span data-stu-id="2ef91-153">Security and secret manager</span></span>

<span data-ttu-id="2ef91-154">設定資料方針：</span><span class="sxs-lookup"><span data-stu-id="2ef91-154">Configuration data guidelines:</span></span>

* <span data-ttu-id="2ef91-155">永遠不要將密碼或其他敏感性資料儲存在設定提供者程式碼或純文字設定檔中。</span><span class="sxs-lookup"><span data-stu-id="2ef91-155">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="2ef91-156">[秘密管理員](xref:security/app-secrets)可以用來將秘密儲存在開發中。</span><span class="sxs-lookup"><span data-stu-id="2ef91-156">The [Secret manager](xref:security/app-secrets) can be used to store secrets in development.</span></span>
* <span data-ttu-id="2ef91-157">不要在開發或測試環境中使用生產環境祕密。</span><span class="sxs-lookup"><span data-stu-id="2ef91-157">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="2ef91-158">請在專案外部指定祕密，以防止其意外認可至開放原始碼存放庫。</span><span class="sxs-lookup"><span data-stu-id="2ef91-158">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="2ef91-159">根據[預設](#default)，[秘密管理員](xref:security/app-secrets)會在*appsettings*之後讀取設定和*appsettings。* `Environment`*. json*。</span><span class="sxs-lookup"><span data-stu-id="2ef91-159">By [default](#default), [Secret manager](xref:security/app-secrets) reads configuration settings after *appsettings.json* and *appsettings.*`Environment`*.json*.</span></span>

<span data-ttu-id="2ef91-160">如需儲存密碼或其他機密資料的詳細資訊：</span><span class="sxs-lookup"><span data-stu-id="2ef91-160">For more information on storing passwords or other sensitive data:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="2ef91-161"><xref:security/app-secrets>：包含有關使用環境變數來儲存敏感性資料的建議。</span><span class="sxs-lookup"><span data-stu-id="2ef91-161"><xref:security/app-secrets>:  Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="2ef91-162">秘密管理員會使用檔案設定[提供者](#fcp)，將使用者秘密儲存在本機系統上的 JSON 檔案中。</span><span class="sxs-lookup"><span data-stu-id="2ef91-162">The Secret Manager uses the [File configuration provider](#fcp) to store user secrets in a JSON file on the local system.</span></span>

<span data-ttu-id="2ef91-163">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) 可安全地儲存 ASP.NET Core 應用程式的應用程式祕密。</span><span class="sxs-lookup"><span data-stu-id="2ef91-163">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="2ef91-164">如需詳細資訊，請參閱<xref:security/key-vault-configuration>。</span><span class="sxs-lookup"><span data-stu-id="2ef91-164">For more information, see <xref:security/key-vault-configuration>.</span></span>

<a name="evcp"></a>

## <a name="environment-variables"></a><span data-ttu-id="2ef91-165">環境變數</span><span class="sxs-lookup"><span data-stu-id="2ef91-165">Environment variables</span></span>

<span data-ttu-id="2ef91-166">使用[預設](#default)設定時，會在 <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> 讀取*appsettings*、appsettings 之後，從環境變數的機碼值組載入設定 *。* `Environment`*. json*和[密碼管理員](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="2ef91-166">Using the [default](#default) configuration, the <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs after reading *appsettings.json*, *appsettings.*`Environment`*.json*, and [Secret manager](xref:security/app-secrets).</span></span> <span data-ttu-id="2ef91-167">因此，從環境中讀取的索引鍵值會覆寫從*appsettings*讀取的值，也就是*appsettings。* `Environment`*. json*和密碼管理員。</span><span class="sxs-lookup"><span data-stu-id="2ef91-167">Therefore, key values read from the environment override values read from *appsettings.json*, *appsettings.*`Environment`*.json*, and Secret manager.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="2ef91-168">下列 `set` 命令：</span><span class="sxs-lookup"><span data-stu-id="2ef91-168">The following `set` commands:</span></span>

* <span data-ttu-id="2ef91-169">在 Windows 上設定[上述範例](#appsettingsjson)的環境索引鍵和值。</span><span class="sxs-lookup"><span data-stu-id="2ef91-169">Set the environment keys and values of the [preceding example](#appsettingsjson) on Windows.</span></span>
* <span data-ttu-id="2ef91-170">使用[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)時測試設定。</span><span class="sxs-lookup"><span data-stu-id="2ef91-170">Test the settings when using the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample).</span></span> <span data-ttu-id="2ef91-171">`dotnet run`命令必須在專案目錄中執行。</span><span class="sxs-lookup"><span data-stu-id="2ef91-171">The `dotnet run` command must be run in the project directory.</span></span>

```dotnetcli
set MyKey="My key from Environment"
set Position__Title=Environment_Editor
set Position__Name=Environment_Rick
dotnet run
```

<span data-ttu-id="2ef91-172">先前的環境設定：</span><span class="sxs-lookup"><span data-stu-id="2ef91-172">The preceding environment settings:</span></span>

* <span data-ttu-id="2ef91-173">只會在從其設定所在的命令視窗中啟動的進程中設定。</span><span class="sxs-lookup"><span data-stu-id="2ef91-173">Are only set in processes launched from the command window they were set in.</span></span>
* <span data-ttu-id="2ef91-174">以 Visual Studio 啟動的瀏覽器將不會讀取。</span><span class="sxs-lookup"><span data-stu-id="2ef91-174">Won't be read by browsers launched with Visual Studio.</span></span>

<span data-ttu-id="2ef91-175">下列[setx](/windows-server/administration/windows-commands/setx)命令可以用來設定 Windows 上的環境索引鍵和值。</span><span class="sxs-lookup"><span data-stu-id="2ef91-175">The following [setx](/windows-server/administration/windows-commands/setx) commands can be used to set the environment keys and values on Windows.</span></span> <span data-ttu-id="2ef91-176">不同于 `set` ， `setx` 設定會保存下來。</span><span class="sxs-lookup"><span data-stu-id="2ef91-176">Unlike `set`, `setx` settings are persisted.</span></span> <span data-ttu-id="2ef91-177">`/M`設定系統內容中的變數。</span><span class="sxs-lookup"><span data-stu-id="2ef91-177">`/M` sets the variable in the system environment.</span></span> <span data-ttu-id="2ef91-178">如果 `/M` 未使用參數，則會設定使用者環境變數。</span><span class="sxs-lookup"><span data-stu-id="2ef91-178">If the `/M` switch isn't used, a user environment variable is set.</span></span>

```cmd
setx MyKey "My key from setx Environment" /M
setx Position__Title Setx_Environment_Editor /M
setx Position__Name Environment_Rick /M
```

<span data-ttu-id="2ef91-179">若要測試前面的命令會覆寫*appsettings*和*appsettings。* `Environment`*. json*：</span><span class="sxs-lookup"><span data-stu-id="2ef91-179">To test that the preceding commands override *appsettings.json* and *appsettings.*`Environment`*.json*:</span></span>

* <span data-ttu-id="2ef91-180">使用 Visual Studio： [結束] 和 [重新開機] Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="2ef91-180">With Visual Studio: Exit and restart Visual Studio.</span></span>
* <span data-ttu-id="2ef91-181">使用 CLI：啟動新的命令視窗，然後輸入 `dotnet run` 。</span><span class="sxs-lookup"><span data-stu-id="2ef91-181">With the CLI: Start a new command window and enter `dotnet run`.</span></span>

<span data-ttu-id="2ef91-182"><xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*>使用字串呼叫以指定環境變數的前置詞：</span><span class="sxs-lookup"><span data-stu-id="2ef91-182">Call <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> with a string to specify a prefix for environment variables:</span></span>

[!code-csharp[](~/fundamentals/configuration/index/samples/3.x/ConfigSample/Program.cs?name=snippet4&highlight=12)]

<span data-ttu-id="2ef91-183">在上述程式碼中：</span><span class="sxs-lookup"><span data-stu-id="2ef91-183">In the preceding code:</span></span>

* <span data-ttu-id="2ef91-184">`config.AddEnvironmentVariables(prefix: "MyCustomPrefix_")`會在預設的設定[提供者](#default)之後加入。</span><span class="sxs-lookup"><span data-stu-id="2ef91-184">`config.AddEnvironmentVariables(prefix: "MyCustomPrefix_")` is added after the [default configuration providers](#default).</span></span> <span data-ttu-id="2ef91-185">如需排序設定提供者的範例，請參閱[JSON 設定提供者](#jcp)。</span><span class="sxs-lookup"><span data-stu-id="2ef91-185">For an example of ordering the configuration providers, see [JSON configuration provider](#jcp).</span></span>
* <span data-ttu-id="2ef91-186">以前置詞設定的環境變數會覆 `MyCustomPrefix_` 寫預設的設定[提供者](#default)。</span><span class="sxs-lookup"><span data-stu-id="2ef91-186">Environment variables set with the `MyCustomPrefix_` prefix override the [default configuration providers](#default).</span></span> <span data-ttu-id="2ef91-187">這包括不含前置詞的環境變數。</span><span class="sxs-lookup"><span data-stu-id="2ef91-187">This includes environment variables without the prefix.</span></span>

<span data-ttu-id="2ef91-188">讀取設定機碼值組時，會去除前置詞。</span><span class="sxs-lookup"><span data-stu-id="2ef91-188">The prefix is stripped off when the configuration key-value pairs are read.</span></span>

<span data-ttu-id="2ef91-189">下列命令會測試自訂前置詞：</span><span class="sxs-lookup"><span data-stu-id="2ef91-189">The following commands test the custom prefix:</span></span>

```dotnetcli
set MyCustomPrefix_MyKey="My key with MyCustomPrefix_ Environment"
set MyCustomPrefix_Position__Title=Editor_with_customPrefix
set MyCustomPrefix_Position__Name=Environment_Rick_cp
dotnet run
```

<span data-ttu-id="2ef91-190">[預設](#default)設定會載入前面加上和的環境變數和命令列引數 `DOTNET_` `ASPNETCORE_` 。</span><span class="sxs-lookup"><span data-stu-id="2ef91-190">The [default configuration](#default) loads environment variables and command line arguments prefixed with `DOTNET_` and `ASPNETCORE_`.</span></span> <span data-ttu-id="2ef91-191">和前置詞 `DOTNET_` `ASPNETCORE_` 是由[主機和應用程式](xref:fundamentals/host/generic-host#host-configuration)設定的 ASP.NET Core 所使用，但不適用於使用者設定。</span><span class="sxs-lookup"><span data-stu-id="2ef91-191">The `DOTNET_` and `ASPNETCORE_` prefixes are used by ASP.NET Core for [host and app configuration](xref:fundamentals/host/generic-host#host-configuration), but not for user configuration.</span></span> <span data-ttu-id="2ef91-192">如需主機和應用程式設定的詳細資訊，請參閱[.Net 泛型主機](xref:fundamentals/host/generic-host)。</span><span class="sxs-lookup"><span data-stu-id="2ef91-192">For more information on host and app configuration, see [.NET Generic Host](xref:fundamentals/host/generic-host).</span></span>

<span data-ttu-id="2ef91-193">在[Azure App Service](https://azure.microsoft.com/services/app-service/)上，選取 [**設定] > 設定**] 頁面上的 [**新增應用程式設定**]。</span><span class="sxs-lookup"><span data-stu-id="2ef91-193">On [Azure App Service](https://azure.microsoft.com/services/app-service/), select **New application setting** on the **Settings > Configuration** page.</span></span> <span data-ttu-id="2ef91-194">Azure App Service 的應用程式設定如下：</span><span class="sxs-lookup"><span data-stu-id="2ef91-194">Azure App Service application settings are:</span></span>

* <span data-ttu-id="2ef91-195">待用加密，並透過加密通道傳輸。</span><span class="sxs-lookup"><span data-stu-id="2ef91-195">Encrypted at rest and transmitted over an encrypted channel.</span></span>
* <span data-ttu-id="2ef91-196">公開為環境變數。</span><span class="sxs-lookup"><span data-stu-id="2ef91-196">Exposed as environment variables.</span></span>

<span data-ttu-id="2ef91-197">如需詳細資訊，請參閱 [Azure App：使用 Azure 入口網站覆寫應用程式設定](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal)。</span><span class="sxs-lookup"><span data-stu-id="2ef91-197">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

<span data-ttu-id="2ef91-198">如需 Azure 資料庫連接字串的相關資訊，請參閱[連接字串](#constr)前置詞。</span><span class="sxs-lookup"><span data-stu-id="2ef91-198">See [Connection string prefixes](#constr) for information on Azure database connection strings.</span></span>

<a name="clcp"></a>

## <a name="command-line"></a><span data-ttu-id="2ef91-199">命令列</span><span class="sxs-lookup"><span data-stu-id="2ef91-199">Command-line</span></span>

<span data-ttu-id="2ef91-200">使用[預設](#default)設定時，會在 <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> 下列設定來源之後，從命令列引數的機碼值組載入設定：</span><span class="sxs-lookup"><span data-stu-id="2ef91-200">Using the [default](#default) configuration, the <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs after the following configuration sources:</span></span>

* <span data-ttu-id="2ef91-201">*appsettings. json*和*appsettings*。 `Environment`*json*檔案。</span><span class="sxs-lookup"><span data-stu-id="2ef91-201">*appsettings.json* and *appsettings*.`Environment`.*json* files.</span></span>
* <span data-ttu-id="2ef91-202">開發環境中的[應用程式秘密（秘密管理員）](xref:security/app-secrets) 。</span><span class="sxs-lookup"><span data-stu-id="2ef91-202">[App secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="2ef91-203">環境變數。</span><span class="sxs-lookup"><span data-stu-id="2ef91-203">Environment variables.</span></span>

<span data-ttu-id="2ef91-204">根據[預設](#default)，在命令列上設定的設定值會覆寫所有其他設定提供者所設定的設定值。</span><span class="sxs-lookup"><span data-stu-id="2ef91-204">By [default](#default), configuration values set on the command-line override configuration values set with all the other configuration providers.</span></span>

### <a name="command-line-arguments"></a><span data-ttu-id="2ef91-205">命令列引數</span><span class="sxs-lookup"><span data-stu-id="2ef91-205">Command-line arguments</span></span>

<span data-ttu-id="2ef91-206">下列命令會使用來設定索引鍵和值 `=` ：</span><span class="sxs-lookup"><span data-stu-id="2ef91-206">The following command sets keys and values using `=`:</span></span>

```dotnetcli
dotnet run MyKey="My key from command line" Position:Title=Cmd Position:Name=Cmd_Rick
```

<span data-ttu-id="2ef91-207">下列命令會使用來設定索引鍵和值 `/` ：</span><span class="sxs-lookup"><span data-stu-id="2ef91-207">The following command sets keys and values using `/`:</span></span>

```dotnetcli
dotnet run /MyKey "Using /" /Position:Title=Cmd_ /Position:Name=Cmd_Rick
```

<span data-ttu-id="2ef91-208">下列命令會使用來設定索引鍵和值 `--` ：</span><span class="sxs-lookup"><span data-stu-id="2ef91-208">The following command sets keys and values using `--`:</span></span>

```dotnetcli
dotnet run --MyKey "Using --" --Position:Title=Cmd-- --Position:Name=Cmd--Rick
```

<span data-ttu-id="2ef91-209">索引鍵值：</span><span class="sxs-lookup"><span data-stu-id="2ef91-209">The key value:</span></span>

* <span data-ttu-id="2ef91-210">必須遵循 `=` ，或者 `--` `/` 當值在空格後面時，索引鍵必須有或的前置詞。</span><span class="sxs-lookup"><span data-stu-id="2ef91-210">Must follow `=`, or the key must have a prefix of `--` or `/` when the value follows a space.</span></span>
* <span data-ttu-id="2ef91-211">如果 `=` 使用，則不需要。</span><span class="sxs-lookup"><span data-stu-id="2ef91-211">Isn't required if `=` is used.</span></span> <span data-ttu-id="2ef91-212">例如 `MySetting=`。</span><span class="sxs-lookup"><span data-stu-id="2ef91-212">For example, `MySetting=`.</span></span>

<span data-ttu-id="2ef91-213">在相同的命令中，請不要混合使用搭配使用空格的機碼值組的命令列引數索引鍵/值配對 `=` 。</span><span class="sxs-lookup"><span data-stu-id="2ef91-213">Within the same command, don't mix command-line argument key-value pairs that use `=` with key-value pairs that use a space.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="2ef91-214">切換對應</span><span class="sxs-lookup"><span data-stu-id="2ef91-214">Switch mappings</span></span>

<span data-ttu-id="2ef91-215">交換器對應允許索引**鍵**名稱取代邏輯。</span><span class="sxs-lookup"><span data-stu-id="2ef91-215">Switch mappings allow **key** name replacement logic.</span></span> <span data-ttu-id="2ef91-216">將參數取代的字典提供給 <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 方法。</span><span class="sxs-lookup"><span data-stu-id="2ef91-216">Provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="2ef91-217">使用切換對應字典時，會檢查字典中是否有任何索引鍵符合命令列引數所提供的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="2ef91-217">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="2ef91-218">如果在字典中找到命令列索引鍵，則會傳回字典值，將索引鍵/值組設為應用程式的設定。</span><span class="sxs-lookup"><span data-stu-id="2ef91-218">If the command-line key is found in the dictionary, the dictionary value is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="2ef91-219">所有前面加上單虛線 (`-`) 的命令列索引鍵都需要切換對應。</span><span class="sxs-lookup"><span data-stu-id="2ef91-219">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="2ef91-220">切換對應字典索引鍵規則：</span><span class="sxs-lookup"><span data-stu-id="2ef91-220">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="2ef91-221">參數的開頭必須是 `-` 或 `--` 。</span><span class="sxs-lookup"><span data-stu-id="2ef91-221">Switches must start with `-` or `--`.</span></span>
* <span data-ttu-id="2ef91-222">切換對應字典不能包含重複索引鍵。</span><span class="sxs-lookup"><span data-stu-id="2ef91-222">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="2ef91-223">若要使用交換器對應字典，請將它傳遞至對的呼叫 `AddCommandLine` ：</span><span class="sxs-lookup"><span data-stu-id="2ef91-223">To use a switch mappings dictionary, pass it into the call to `AddCommandLine`:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramSwitch.cs?name=snippet&highlight=10-18,23)]

<span data-ttu-id="2ef91-224">下列程式碼顯示已取代金鑰的索引鍵值：</span><span class="sxs-lookup"><span data-stu-id="2ef91-224">The following code shows the key values for the replaced keys:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test3.cshtml.cs?name=snippet)]

<span data-ttu-id="2ef91-225">執行下列命令來測試金鑰取代：</span><span class="sxs-lookup"><span data-stu-id="2ef91-225">Run the following command to test the key replacement:</span></span>

```dotnetcli
dotnet run -k1=value1 -k2 value2 --alt3=value2 /alt4=value3 --alt5 value5 /alt6 value6
```

<span data-ttu-id="2ef91-226">注意：目前 `=` 無法使用單一破折號來設定索引鍵取代值 `-` 。</span><span class="sxs-lookup"><span data-stu-id="2ef91-226">Note: Currently, `=` cannot be used to set key-replacement values with a single dash `-`.</span></span> <span data-ttu-id="2ef91-227">請參閱[這個 GitHub 問題](https://github.com/dotnet/extensions/issues/3059)。</span><span class="sxs-lookup"><span data-stu-id="2ef91-227">See [this GitHub issue](https://github.com/dotnet/extensions/issues/3059).</span></span>

<span data-ttu-id="2ef91-228">下列命令適用于測試金鑰取代：</span><span class="sxs-lookup"><span data-stu-id="2ef91-228">The following command works to test key replacement:</span></span>

```dotnetcli
dotnet run -k1 value1 -k2 value2 --alt3=value2 /alt4=value3 --alt5 value5 /alt6 value6
```

<span data-ttu-id="2ef91-229">針對使用切換對應的應用程式，呼叫 `CreateDefaultBuilder` 不應傳遞引數。</span><span class="sxs-lookup"><span data-stu-id="2ef91-229">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="2ef91-230">`CreateDefaultBuilder`方法的 `AddCommandLine` 呼叫不包含對應的參數，而且沒有任何方法可將參數對應字典傳遞至 `CreateDefaultBuilder` 。</span><span class="sxs-lookup"><span data-stu-id="2ef91-230">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch-mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="2ef91-231">解決方案並不會將引數傳遞給， `CreateDefaultBuilder` 而是允許 `ConfigurationBuilder` 方法的 `AddCommandLine` 方法同時處理引數和切換對應字典。</span><span class="sxs-lookup"><span data-stu-id="2ef91-231">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch-mapping dictionary.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="2ef91-232">階層式設定資料</span><span class="sxs-lookup"><span data-stu-id="2ef91-232">Hierarchical configuration data</span></span>

<span data-ttu-id="2ef91-233">設定 API 會藉由使用設定機碼中的分隔符號來簡維階層式資料，以讀取階層式設定資料。</span><span class="sxs-lookup"><span data-stu-id="2ef91-233">The Configuration API reads hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="2ef91-234">[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)包含下列*appsettings*檔案：</span><span class="sxs-lookup"><span data-stu-id="2ef91-234">The [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) contains the following  *appsettings.json* file:</span></span>

[!code-json[](index/samples/3.x/ConfigSample/appsettings.json)]

<span data-ttu-id="2ef91-235">[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)中的下列程式碼會顯示數個設定值：</span><span class="sxs-lookup"><span data-stu-id="2ef91-235">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) displays several of the configurations settings:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

<span data-ttu-id="2ef91-236">讀取階層式設定資料的慣用方法是使用選項模式。</span><span class="sxs-lookup"><span data-stu-id="2ef91-236">The preferred way to read hierarchical configuration data is using the options pattern.</span></span> <span data-ttu-id="2ef91-237">如需詳細資訊，請參閱本檔中的系結階層式設定[資料](#optpat)。</span><span class="sxs-lookup"><span data-stu-id="2ef91-237">For more information, see [Bind hierarchical configuration data](#optpat) in this document.</span></span>

<span data-ttu-id="2ef91-238"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> 與 <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> 方法可用來在設定資料中隔離區段與區段的子系。</span><span class="sxs-lookup"><span data-stu-id="2ef91-238"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="2ef91-239">[GetSection,、 GetChildren 與 Exists](#getsection) 中說明這些方法。</span><span class="sxs-lookup"><span data-stu-id="2ef91-239">These methods are described later in [GetSection, GetChildren, and Exists](#getsection).</span></span>

<!--
[Azure Key Vault configuration provider](xref:security/key-vault-configuration) implement change detection.
-->

## <a name="configuration-keys-and-values"></a><span data-ttu-id="2ef91-240">設定機碼和值</span><span class="sxs-lookup"><span data-stu-id="2ef91-240">Configuration keys and values</span></span>

<span data-ttu-id="2ef91-241">設定機碼：</span><span class="sxs-lookup"><span data-stu-id="2ef91-241">Configuration keys:</span></span>

* <span data-ttu-id="2ef91-242">不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="2ef91-242">Are case-insensitive.</span></span> <span data-ttu-id="2ef91-243">例如，`ConnectionString` 與 `connectionstring` 會被視為相等的機碼。</span><span class="sxs-lookup"><span data-stu-id="2ef91-243">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="2ef91-244">如果在多個設定提供者中設定索引鍵和值，則會使用最後新增的提供者中的值。</span><span class="sxs-lookup"><span data-stu-id="2ef91-244">If a key and value is set in more than one configuration providers, the value from the last provider added is used.</span></span> <span data-ttu-id="2ef91-245">如需詳細資訊，請參閱[預設](#default)設定。</span><span class="sxs-lookup"><span data-stu-id="2ef91-245">For more information, see [Default configuration](#default).</span></span>
* <span data-ttu-id="2ef91-246">階層式機碼</span><span class="sxs-lookup"><span data-stu-id="2ef91-246">Hierarchical keys</span></span>
  * <span data-ttu-id="2ef91-247">在設定 API 內，冒號分隔字元 (`:`) 可在所有平台上運作。</span><span class="sxs-lookup"><span data-stu-id="2ef91-247">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="2ef91-248">在環境變數中，冒號分隔字元可能無法在所有平台上運作。</span><span class="sxs-lookup"><span data-stu-id="2ef91-248">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="2ef91-249">`__`所有平臺都支援雙底線（），而且會自動轉換成冒號 `:` 。</span><span class="sxs-lookup"><span data-stu-id="2ef91-249">A double underscore, `__`, is supported by all platforms and is automatically converted into a colon `:`.</span></span>
  * <span data-ttu-id="2ef91-250">在 Azure Key Vault 中，階層式索引鍵會使用 `--` 做為分隔符號。</span><span class="sxs-lookup"><span data-stu-id="2ef91-250">In Azure Key Vault, hierarchical keys use `--` as a separator.</span></span> <span data-ttu-id="2ef91-251">[Azure Key Vault configuration provider](xref:security/key-vault-configuration) `--` `:` 當密碼載入應用程式的設定時，Azure Key Vault 設定提供者會自動將取代為。</span><span class="sxs-lookup"><span data-stu-id="2ef91-251">The [Azure Key Vault configuration provider](xref:security/key-vault-configuration) automatically replaces `--` with a `:` when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="2ef91-252"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> 支援在設定機碼中使用陣列索引將陣列繫結到物件。</span><span class="sxs-lookup"><span data-stu-id="2ef91-252">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="2ef91-253">[將陣列繫結到類別](#boa)一節說明陣列繫結。</span><span class="sxs-lookup"><span data-stu-id="2ef91-253">Array binding is described in the [Bind an array to a class](#boa) section.</span></span>

<span data-ttu-id="2ef91-254">設定值：</span><span class="sxs-lookup"><span data-stu-id="2ef91-254">Configuration values:</span></span>

* <span data-ttu-id="2ef91-255">為字串。</span><span class="sxs-lookup"><span data-stu-id="2ef91-255">Are strings.</span></span>
* <span data-ttu-id="2ef91-256">Null 值無法存放在設定中或繫結到物件。</span><span class="sxs-lookup"><span data-stu-id="2ef91-256">Null values can't be stored in configuration or bound to objects.</span></span>

<a name="cp"></a>

## <a name="configuration-providers"></a><span data-ttu-id="2ef91-257">設定提供者</span><span class="sxs-lookup"><span data-stu-id="2ef91-257">Configuration providers</span></span>

<span data-ttu-id="2ef91-258">下表顯示可供 ASP.NET Core 應用程式使用的設定提供者。</span><span class="sxs-lookup"><span data-stu-id="2ef91-258">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="2ef91-259">提供者</span><span class="sxs-lookup"><span data-stu-id="2ef91-259">Provider</span></span> | <span data-ttu-id="2ef91-260">從提供設定</span><span class="sxs-lookup"><span data-stu-id="2ef91-260">Provides configuration from</span></span> |
| ---
<span data-ttu-id="2ef91-261">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-261">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-262">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-262">'Blazor'</span></span>
- <span data-ttu-id="2ef91-263">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-263">'Identity'</span></span>
- <span data-ttu-id="2ef91-264">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-264">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-265">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-265">'Razor'</span></span>
- <span data-ttu-id="2ef91-266">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-266">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-267">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-267">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-268">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-268">'Blazor'</span></span>
- <span data-ttu-id="2ef91-269">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-269">'Identity'</span></span>
- <span data-ttu-id="2ef91-270">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-270">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-271">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-271">'Razor'</span></span>
- <span data-ttu-id="2ef91-272">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-272">'SignalR' uid:</span></span> 

<span data-ttu-id="2ef91-273">---- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-273">---- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-274">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-274">'Blazor'</span></span>
- <span data-ttu-id="2ef91-275">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-275">'Identity'</span></span>
- <span data-ttu-id="2ef91-276">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-276">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-277">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-277">'Razor'</span></span>
- <span data-ttu-id="2ef91-278">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-278">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-279">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-279">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-280">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-280">'Blazor'</span></span>
- <span data-ttu-id="2ef91-281">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-281">'Identity'</span></span>
- <span data-ttu-id="2ef91-282">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-282">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-283">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-283">'Razor'</span></span>
- <span data-ttu-id="2ef91-284">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-284">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-285">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-285">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-286">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-286">'Blazor'</span></span>
- <span data-ttu-id="2ef91-287">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-287">'Identity'</span></span>
- <span data-ttu-id="2ef91-288">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-288">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-289">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-289">'Razor'</span></span>
- <span data-ttu-id="2ef91-290">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-290">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-291">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-291">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-292">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-292">'Blazor'</span></span>
- <span data-ttu-id="2ef91-293">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-293">'Identity'</span></span>
- <span data-ttu-id="2ef91-294">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-294">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-295">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-295">'Razor'</span></span>
- <span data-ttu-id="2ef91-296">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-296">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-297">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-297">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-298">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-298">'Blazor'</span></span>
- <span data-ttu-id="2ef91-299">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-299">'Identity'</span></span>
- <span data-ttu-id="2ef91-300">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-300">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-301">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-301">'Razor'</span></span>
- <span data-ttu-id="2ef91-302">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-302">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-303">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-303">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-304">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-304">'Blazor'</span></span>
- <span data-ttu-id="2ef91-305">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-305">'Identity'</span></span>
- <span data-ttu-id="2ef91-306">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-306">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-307">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-307">'Razor'</span></span>
- <span data-ttu-id="2ef91-308">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-308">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-309">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-309">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-310">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-310">'Blazor'</span></span>
- <span data-ttu-id="2ef91-311">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-311">'Identity'</span></span>
- <span data-ttu-id="2ef91-312">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-312">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-313">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-313">'Razor'</span></span>
- <span data-ttu-id="2ef91-314">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-314">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-315">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-315">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-316">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-316">'Blazor'</span></span>
- <span data-ttu-id="2ef91-317">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-317">'Identity'</span></span>
- <span data-ttu-id="2ef91-318">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-318">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-319">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-319">'Razor'</span></span>
- <span data-ttu-id="2ef91-320">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-320">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-321">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-321">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-322">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-322">'Blazor'</span></span>
- <span data-ttu-id="2ef91-323">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-323">'Identity'</span></span>
- <span data-ttu-id="2ef91-324">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-324">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-325">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-325">'Razor'</span></span>
- <span data-ttu-id="2ef91-326">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-326">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-327">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-327">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-328">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-328">'Blazor'</span></span>
- <span data-ttu-id="2ef91-329">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-329">'Identity'</span></span>
- <span data-ttu-id="2ef91-330">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-330">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-331">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-331">'Razor'</span></span>
- <span data-ttu-id="2ef91-332">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-332">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-333">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-333">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-334">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-334">'Blazor'</span></span>
- <span data-ttu-id="2ef91-335">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-335">'Identity'</span></span>
- <span data-ttu-id="2ef91-336">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-336">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-337">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-337">'Razor'</span></span>
- <span data-ttu-id="2ef91-338">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-338">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-339">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-339">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-340">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-340">'Blazor'</span></span>
- <span data-ttu-id="2ef91-341">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-341">'Identity'</span></span>
- <span data-ttu-id="2ef91-342">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-342">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-343">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-343">'Razor'</span></span>
- <span data-ttu-id="2ef91-344">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-344">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-345">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-345">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-346">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-346">'Blazor'</span></span>
- <span data-ttu-id="2ef91-347">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-347">'Identity'</span></span>
- <span data-ttu-id="2ef91-348">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-348">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-349">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-349">'Razor'</span></span>
- <span data-ttu-id="2ef91-350">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-350">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-351">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-351">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-352">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-352">'Blazor'</span></span>
- <span data-ttu-id="2ef91-353">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-353">'Identity'</span></span>
- <span data-ttu-id="2ef91-354">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-354">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-355">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-355">'Razor'</span></span>
- <span data-ttu-id="2ef91-356">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-356">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-357">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-357">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-358">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-358">'Blazor'</span></span>
- <span data-ttu-id="2ef91-359">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-359">'Identity'</span></span>
- <span data-ttu-id="2ef91-360">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-360">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-361">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-361">'Razor'</span></span>
- <span data-ttu-id="2ef91-362">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-362">'SignalR' uid:</span></span> 

<span data-ttu-id="2ef91-363">------------------ || [Azure Key Vault 設定提供者](xref:security/key-vault-configuration)|Azure Key Vault || [Azure App 設定提供者](/azure/azure-app-configuration/quickstart-aspnet-core-app)|Azure 應用程式組態 ||[命令列設定提供者](#clcp)|命令列參數 ||[自訂設定提供者](#custom-configuration-provider)|自訂來源 ||[環境變數設定提供者](#evcp)|環境變數 |檔案設定 | [提供者](#file-configuration-provider)|INI、JSON 和 XML 檔案 ||[每個](#key-per-file-configuration-provider)檔案的索引鍵設定提供者 |目錄檔案 ||[記憶體設定提供者](#memory-configuration-provider)|記憶體內部集合 ||[秘密管理員](xref:security/app-secrets)|使用者設定檔目錄中的檔案 |</span><span class="sxs-lookup"><span data-stu-id="2ef91-363">------------------ | | [Azure Key Vault configuration provider](xref:security/key-vault-configuration) | Azure Key Vault | | [Azure App configuration provider](/azure/azure-app-configuration/quickstart-aspnet-core-app) | Azure App Configuration | | [Command-line configuration provider](#clcp) | Command-line parameters | | [Custom configuration provider](#custom-configuration-provider) | Custom source | | [Environment Variables configuration provider](#evcp) | Environment variables | | [File configuration provider](#file-configuration-provider) | INI, JSON, and XML files | | [Key-per-file configuration provider](#key-per-file-configuration-provider) | Directory files | | [Memory configuration provider](#memory-configuration-provider) | In-memory collections | | [Secret Manager](xref:security/app-secrets)  | File in the user profile directory |</span></span>

<span data-ttu-id="2ef91-364">設定來源會依照其設定提供者的指定順序讀取。</span><span class="sxs-lookup"><span data-stu-id="2ef91-364">Configuration sources are read in the order that their configuration providers are specified.</span></span> <span data-ttu-id="2ef91-365">請在程式碼中訂購設定提供者，以符合應用程式所需之基礎設定來源的優先順序。</span><span class="sxs-lookup"><span data-stu-id="2ef91-365">Order configuration providers in code to suit the priorities for the underlying configuration sources that the app requires.</span></span>

<span data-ttu-id="2ef91-366">典型的設定提供者順序是：</span><span class="sxs-lookup"><span data-stu-id="2ef91-366">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="2ef91-367">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="2ef91-367">*appsettings.json*</span></span>
1. <span data-ttu-id="2ef91-368">*appsettings*。 `Environment`*json*</span><span class="sxs-lookup"><span data-stu-id="2ef91-368">*appsettings*.`Environment`.*json*</span></span>
1. [<span data-ttu-id="2ef91-369">秘密管理員</span><span class="sxs-lookup"><span data-stu-id="2ef91-369">Secret Manager</span></span>](xref:security/app-secrets)
1. <span data-ttu-id="2ef91-370">使用[環境變數設定提供者](#evcp)的環境變數。</span><span class="sxs-lookup"><span data-stu-id="2ef91-370">Environment variables using the [Environment Variables configuration provider](#evcp).</span></span>
1. <span data-ttu-id="2ef91-371">使用[命令列設定提供者](#command-line-configuration-provider)的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="2ef91-371">Command-line arguments using the [Command-line configuration provider](#command-line-configuration-provider).</span></span>

<span data-ttu-id="2ef91-372">常見的做法是在一系列提供者中新增命令列設定提供者，以允許命令列引數覆寫其他提供者所設定的設定。</span><span class="sxs-lookup"><span data-stu-id="2ef91-372">A common practice is to add the Command-line configuration provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="2ef91-373">先前的提供者序列會用於[預設](#default)設定中。</span><span class="sxs-lookup"><span data-stu-id="2ef91-373">The preceding sequence of providers is used in the [default configuration](#default).</span></span>

<a name="constr"></a>

### <a name="connection-string-prefixes"></a><span data-ttu-id="2ef91-374">連接字串前置詞</span><span class="sxs-lookup"><span data-stu-id="2ef91-374">Connection string prefixes</span></span>

<span data-ttu-id="2ef91-375">設定 API 具有四個連接字串環境變數的特殊處理規則。</span><span class="sxs-lookup"><span data-stu-id="2ef91-375">The Configuration API has special processing rules for four connection string environment variables.</span></span> <span data-ttu-id="2ef91-376">這些連接字串牽涉到設定應用程式環境的 Azure 連接字串。</span><span class="sxs-lookup"><span data-stu-id="2ef91-376">These connection strings are involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="2ef91-377">具有資料表中所顯示前置詞的環境變數，會載入至具有[預設](#default)設定的應用程式，或未提供任何首碼時 `AddEnvironmentVariables` 。</span><span class="sxs-lookup"><span data-stu-id="2ef91-377">Environment variables with the prefixes shown in the table are loaded into the app with the [default configuration](#default) or when no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="2ef91-378">連接字串前置詞</span><span class="sxs-lookup"><span data-stu-id="2ef91-378">Connection string prefix</span></span> | <span data-ttu-id="2ef91-379">提供者</span><span class="sxs-lookup"><span data-stu-id="2ef91-379">Provider</span></span> |
| ---
<span data-ttu-id="2ef91-380">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-380">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-381">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-381">'Blazor'</span></span>
- <span data-ttu-id="2ef91-382">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-382">'Identity'</span></span>
- <span data-ttu-id="2ef91-383">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-383">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-384">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-384">'Razor'</span></span>
- <span data-ttu-id="2ef91-385">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-385">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-386">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-386">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-387">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-387">'Blazor'</span></span>
- <span data-ttu-id="2ef91-388">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-388">'Identity'</span></span>
- <span data-ttu-id="2ef91-389">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-389">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-390">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-390">'Razor'</span></span>
- <span data-ttu-id="2ef91-391">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-391">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-392">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-392">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-393">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-393">'Blazor'</span></span>
- <span data-ttu-id="2ef91-394">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-394">'Identity'</span></span>
- <span data-ttu-id="2ef91-395">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-395">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-396">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-396">'Razor'</span></span>
- <span data-ttu-id="2ef91-397">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-397">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-398">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-398">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-399">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-399">'Blazor'</span></span>
- <span data-ttu-id="2ef91-400">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-400">'Identity'</span></span>
- <span data-ttu-id="2ef91-401">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-401">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-402">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-402">'Razor'</span></span>
- <span data-ttu-id="2ef91-403">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-403">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-404">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-404">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-405">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-405">'Blazor'</span></span>
- <span data-ttu-id="2ef91-406">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-406">'Identity'</span></span>
- <span data-ttu-id="2ef91-407">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-407">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-408">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-408">'Razor'</span></span>
- <span data-ttu-id="2ef91-409">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-409">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-410">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-410">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-411">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-411">'Blazor'</span></span>
- <span data-ttu-id="2ef91-412">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-412">'Identity'</span></span>
- <span data-ttu-id="2ef91-413">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-413">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-414">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-414">'Razor'</span></span>
- <span data-ttu-id="2ef91-415">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-415">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-416">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-416">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-417">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-417">'Blazor'</span></span>
- <span data-ttu-id="2ef91-418">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-418">'Identity'</span></span>
- <span data-ttu-id="2ef91-419">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-419">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-420">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-420">'Razor'</span></span>
- <span data-ttu-id="2ef91-421">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-421">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-422">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-422">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-423">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-423">'Blazor'</span></span>
- <span data-ttu-id="2ef91-424">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-424">'Identity'</span></span>
- <span data-ttu-id="2ef91-425">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-425">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-426">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-426">'Razor'</span></span>
- <span data-ttu-id="2ef91-427">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-427">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-428">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-428">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-429">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-429">'Blazor'</span></span>
- <span data-ttu-id="2ef91-430">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-430">'Identity'</span></span>
- <span data-ttu-id="2ef91-431">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-431">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-432">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-432">'Razor'</span></span>
- <span data-ttu-id="2ef91-433">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-433">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-434">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-434">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-435">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-435">'Blazor'</span></span>
- <span data-ttu-id="2ef91-436">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-436">'Identity'</span></span>
- <span data-ttu-id="2ef91-437">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-437">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-438">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-438">'Razor'</span></span>
- <span data-ttu-id="2ef91-439">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-439">'SignalR' uid:</span></span> 

<span data-ttu-id="2ef91-440">------------ |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-440">------------ | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-441">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-441">'Blazor'</span></span>
- <span data-ttu-id="2ef91-442">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-442">'Identity'</span></span>
- <span data-ttu-id="2ef91-443">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-443">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-444">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-444">'Razor'</span></span>
- <span data-ttu-id="2ef91-445">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-445">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-446">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-446">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-447">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-447">'Blazor'</span></span>
- <span data-ttu-id="2ef91-448">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-448">'Identity'</span></span>
- <span data-ttu-id="2ef91-449">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-449">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-450">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-450">'Razor'</span></span>
- <span data-ttu-id="2ef91-451">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-451">'SignalR' uid:</span></span> 

<span data-ttu-id="2ef91-452">---- || `CUSTOMCONNSTR_` |自訂提供者 || `MYSQLCONNSTR_` |[MySQL](https://www.mysql.com/) |
 MySQL |`SQLAZURECONNSTR_` | [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) |
 | `SQLCONNSTR_` | [SQL Server](https://www.microsoft.com/sql-server/)|</span><span class="sxs-lookup"><span data-stu-id="2ef91-452">---- | | `CUSTOMCONNSTR_` | Custom provider | | `MYSQLCONNSTR_` | [MySQL](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [SQL Server](https://www.microsoft.com/sql-server/) |</span></span>

<span data-ttu-id="2ef91-453">當探索到具有下表所顯示之任何四個前置詞的環境變數並將其載入到設定中時：</span><span class="sxs-lookup"><span data-stu-id="2ef91-453">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="2ef91-454">會透過移除環境變數前置詞並新增設定機碼區段 (`ConnectionStrings`) 來移除具有下表顯示之前置詞的環境變數。</span><span class="sxs-lookup"><span data-stu-id="2ef91-454">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="2ef91-455">會建立新的設定機碼值組以代表資料庫連線提供者 (`CUSTOMCONNSTR_` 除外，這沒有所述提供者)。</span><span class="sxs-lookup"><span data-stu-id="2ef91-455">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="2ef91-456">環境變數機碼</span><span class="sxs-lookup"><span data-stu-id="2ef91-456">Environment variable key</span></span> | <span data-ttu-id="2ef91-457">已轉換的設定機碼</span><span class="sxs-lookup"><span data-stu-id="2ef91-457">Converted configuration key</span></span> | <span data-ttu-id="2ef91-458">提供者設定項目</span><span class="sxs-lookup"><span data-stu-id="2ef91-458">Provider configuration entry</span></span>                                                    |
| ---
<span data-ttu-id="2ef91-459">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-459">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-460">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-460">'Blazor'</span></span>
- <span data-ttu-id="2ef91-461">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-461">'Identity'</span></span>
- <span data-ttu-id="2ef91-462">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-462">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-463">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-463">'Razor'</span></span>
- <span data-ttu-id="2ef91-464">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-464">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-465">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-465">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-466">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-466">'Blazor'</span></span>
- <span data-ttu-id="2ef91-467">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-467">'Identity'</span></span>
- <span data-ttu-id="2ef91-468">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-468">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-469">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-469">'Razor'</span></span>
- <span data-ttu-id="2ef91-470">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-470">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-471">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-471">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-472">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-472">'Blazor'</span></span>
- <span data-ttu-id="2ef91-473">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-473">'Identity'</span></span>
- <span data-ttu-id="2ef91-474">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-474">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-475">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-475">'Razor'</span></span>
- <span data-ttu-id="2ef91-476">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-476">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-477">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-477">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-478">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-478">'Blazor'</span></span>
- <span data-ttu-id="2ef91-479">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-479">'Identity'</span></span>
- <span data-ttu-id="2ef91-480">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-480">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-481">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-481">'Razor'</span></span>
- <span data-ttu-id="2ef91-482">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-482">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-483">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-483">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-484">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-484">'Blazor'</span></span>
- <span data-ttu-id="2ef91-485">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-485">'Identity'</span></span>
- <span data-ttu-id="2ef91-486">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-486">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-487">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-487">'Razor'</span></span>
- <span data-ttu-id="2ef91-488">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-488">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-489">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-489">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-490">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-490">'Blazor'</span></span>
- <span data-ttu-id="2ef91-491">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-491">'Identity'</span></span>
- <span data-ttu-id="2ef91-492">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-492">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-493">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-493">'Razor'</span></span>
- <span data-ttu-id="2ef91-494">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-494">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-495">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-495">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-496">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-496">'Blazor'</span></span>
- <span data-ttu-id="2ef91-497">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-497">'Identity'</span></span>
- <span data-ttu-id="2ef91-498">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-498">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-499">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-499">'Razor'</span></span>
- <span data-ttu-id="2ef91-500">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-500">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-501">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-501">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-502">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-502">'Blazor'</span></span>
- <span data-ttu-id="2ef91-503">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-503">'Identity'</span></span>
- <span data-ttu-id="2ef91-504">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-504">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-505">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-505">'Razor'</span></span>
- <span data-ttu-id="2ef91-506">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-506">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-507">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-507">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-508">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-508">'Blazor'</span></span>
- <span data-ttu-id="2ef91-509">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-509">'Identity'</span></span>
- <span data-ttu-id="2ef91-510">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-510">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-511">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-511">'Razor'</span></span>
- <span data-ttu-id="2ef91-512">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-512">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-513">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-513">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-514">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-514">'Blazor'</span></span>
- <span data-ttu-id="2ef91-515">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-515">'Identity'</span></span>
- <span data-ttu-id="2ef91-516">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-516">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-517">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-517">'Razor'</span></span>
- <span data-ttu-id="2ef91-518">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-518">'SignalR' uid:</span></span> 

<span data-ttu-id="2ef91-519">------------ |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-519">------------ | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-520">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-520">'Blazor'</span></span>
- <span data-ttu-id="2ef91-521">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-521">'Identity'</span></span>
- <span data-ttu-id="2ef91-522">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-522">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-523">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-523">'Razor'</span></span>
- <span data-ttu-id="2ef91-524">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-524">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-525">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-525">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-526">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-526">'Blazor'</span></span>
- <span data-ttu-id="2ef91-527">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-527">'Identity'</span></span>
- <span data-ttu-id="2ef91-528">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-528">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-529">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-529">'Razor'</span></span>
- <span data-ttu-id="2ef91-530">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-530">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-531">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-531">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-532">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-532">'Blazor'</span></span>
- <span data-ttu-id="2ef91-533">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-533">'Identity'</span></span>
- <span data-ttu-id="2ef91-534">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-534">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-535">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-535">'Razor'</span></span>
- <span data-ttu-id="2ef91-536">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-536">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-537">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-537">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-538">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-538">'Blazor'</span></span>
- <span data-ttu-id="2ef91-539">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-539">'Identity'</span></span>
- <span data-ttu-id="2ef91-540">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-540">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-541">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-541">'Razor'</span></span>
- <span data-ttu-id="2ef91-542">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-542">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-543">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-543">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-544">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-544">'Blazor'</span></span>
- <span data-ttu-id="2ef91-545">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-545">'Identity'</span></span>
- <span data-ttu-id="2ef91-546">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-546">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-547">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-547">'Razor'</span></span>
- <span data-ttu-id="2ef91-548">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-548">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-549">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-549">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-550">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-550">'Blazor'</span></span>
- <span data-ttu-id="2ef91-551">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-551">'Identity'</span></span>
- <span data-ttu-id="2ef91-552">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-552">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-553">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-553">'Razor'</span></span>
- <span data-ttu-id="2ef91-554">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-554">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-555">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-555">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-556">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-556">'Blazor'</span></span>
- <span data-ttu-id="2ef91-557">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-557">'Identity'</span></span>
- <span data-ttu-id="2ef91-558">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-558">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-559">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-559">'Razor'</span></span>
- <span data-ttu-id="2ef91-560">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-560">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-561">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-561">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-562">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-562">'Blazor'</span></span>
- <span data-ttu-id="2ef91-563">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-563">'Identity'</span></span>
- <span data-ttu-id="2ef91-564">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-564">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-565">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-565">'Razor'</span></span>
- <span data-ttu-id="2ef91-566">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-566">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-567">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-567">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-568">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-568">'Blazor'</span></span>
- <span data-ttu-id="2ef91-569">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-569">'Identity'</span></span>
- <span data-ttu-id="2ef91-570">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-570">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-571">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-571">'Razor'</span></span>
- <span data-ttu-id="2ef91-572">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-572">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-573">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-573">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-574">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-574">'Blazor'</span></span>
- <span data-ttu-id="2ef91-575">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-575">'Identity'</span></span>
- <span data-ttu-id="2ef91-576">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-576">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-577">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-577">'Razor'</span></span>
- <span data-ttu-id="2ef91-578">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-578">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-579">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-579">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-580">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-580">'Blazor'</span></span>
- <span data-ttu-id="2ef91-581">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-581">'Identity'</span></span>
- <span data-ttu-id="2ef91-582">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-582">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-583">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-583">'Razor'</span></span>
- <span data-ttu-id="2ef91-584">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-584">'SignalR' uid:</span></span> 

<span data-ttu-id="2ef91-585">-------------- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-585">-------------- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-586">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-586">'Blazor'</span></span>
- <span data-ttu-id="2ef91-587">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-587">'Identity'</span></span>
- <span data-ttu-id="2ef91-588">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-588">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-589">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-589">'Razor'</span></span>
- <span data-ttu-id="2ef91-590">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-590">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-591">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-591">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-592">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-592">'Blazor'</span></span>
- <span data-ttu-id="2ef91-593">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-593">'Identity'</span></span>
- <span data-ttu-id="2ef91-594">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-594">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-595">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-595">'Razor'</span></span>
- <span data-ttu-id="2ef91-596">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-596">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-597">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-597">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-598">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-598">'Blazor'</span></span>
- <span data-ttu-id="2ef91-599">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-599">'Identity'</span></span>
- <span data-ttu-id="2ef91-600">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-600">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-601">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-601">'Razor'</span></span>
- <span data-ttu-id="2ef91-602">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-602">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-603">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-603">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-604">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-604">'Blazor'</span></span>
- <span data-ttu-id="2ef91-605">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-605">'Identity'</span></span>
- <span data-ttu-id="2ef91-606">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-606">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-607">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-607">'Razor'</span></span>
- <span data-ttu-id="2ef91-608">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-608">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-609">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-609">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-610">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-610">'Blazor'</span></span>
- <span data-ttu-id="2ef91-611">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-611">'Identity'</span></span>
- <span data-ttu-id="2ef91-612">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-612">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-613">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-613">'Razor'</span></span>
- <span data-ttu-id="2ef91-614">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-614">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-615">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-615">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-616">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-616">'Blazor'</span></span>
- <span data-ttu-id="2ef91-617">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-617">'Identity'</span></span>
- <span data-ttu-id="2ef91-618">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-618">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-619">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-619">'Razor'</span></span>
- <span data-ttu-id="2ef91-620">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-620">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-621">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-621">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-622">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-622">'Blazor'</span></span>
- <span data-ttu-id="2ef91-623">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-623">'Identity'</span></span>
- <span data-ttu-id="2ef91-624">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-624">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-625">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-625">'Razor'</span></span>
- <span data-ttu-id="2ef91-626">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-626">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-627">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-627">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-628">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-628">'Blazor'</span></span>
- <span data-ttu-id="2ef91-629">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-629">'Identity'</span></span>
- <span data-ttu-id="2ef91-630">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-630">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-631">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-631">'Razor'</span></span>
- <span data-ttu-id="2ef91-632">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-632">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-633">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-633">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-634">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-634">'Blazor'</span></span>
- <span data-ttu-id="2ef91-635">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-635">'Identity'</span></span>
- <span data-ttu-id="2ef91-636">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-636">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-637">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-637">'Razor'</span></span>
- <span data-ttu-id="2ef91-638">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-638">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-639">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-639">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-640">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-640">'Blazor'</span></span>
- <span data-ttu-id="2ef91-641">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-641">'Identity'</span></span>
- <span data-ttu-id="2ef91-642">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-642">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-643">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-643">'Razor'</span></span>
- <span data-ttu-id="2ef91-644">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-644">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-645">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-645">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-646">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-646">'Blazor'</span></span>
- <span data-ttu-id="2ef91-647">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-647">'Identity'</span></span>
- <span data-ttu-id="2ef91-648">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-648">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-649">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-649">'Razor'</span></span>
- <span data-ttu-id="2ef91-650">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-650">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-651">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-651">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-652">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-652">'Blazor'</span></span>
- <span data-ttu-id="2ef91-653">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-653">'Identity'</span></span>
- <span data-ttu-id="2ef91-654">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-654">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-655">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-655">'Razor'</span></span>
- <span data-ttu-id="2ef91-656">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-656">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-657">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-657">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-658">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-658">'Blazor'</span></span>
- <span data-ttu-id="2ef91-659">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-659">'Identity'</span></span>
- <span data-ttu-id="2ef91-660">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-660">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-661">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-661">'Razor'</span></span>
- <span data-ttu-id="2ef91-662">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-662">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-663">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-663">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-664">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-664">'Blazor'</span></span>
- <span data-ttu-id="2ef91-665">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-665">'Identity'</span></span>
- <span data-ttu-id="2ef91-666">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-666">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-667">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-667">'Razor'</span></span>
- <span data-ttu-id="2ef91-668">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-668">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-669">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-669">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-670">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-670">'Blazor'</span></span>
- <span data-ttu-id="2ef91-671">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-671">'Identity'</span></span>
- <span data-ttu-id="2ef91-672">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-672">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-673">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-673">'Razor'</span></span>
- <span data-ttu-id="2ef91-674">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-674">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-675">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-675">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-676">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-676">'Blazor'</span></span>
- <span data-ttu-id="2ef91-677">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-677">'Identity'</span></span>
- <span data-ttu-id="2ef91-678">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-678">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-679">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-679">'Razor'</span></span>
- <span data-ttu-id="2ef91-680">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-680">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-681">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-681">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-682">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-682">'Blazor'</span></span>
- <span data-ttu-id="2ef91-683">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-683">'Identity'</span></span>
- <span data-ttu-id="2ef91-684">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-684">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-685">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-685">'Razor'</span></span>
- <span data-ttu-id="2ef91-686">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-686">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-687">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-687">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-688">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-688">'Blazor'</span></span>
- <span data-ttu-id="2ef91-689">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-689">'Identity'</span></span>
- <span data-ttu-id="2ef91-690">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-690">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-691">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-691">'Razor'</span></span>
- <span data-ttu-id="2ef91-692">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-692">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-693">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-693">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-694">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-694">'Blazor'</span></span>
- <span data-ttu-id="2ef91-695">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-695">'Identity'</span></span>
- <span data-ttu-id="2ef91-696">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-696">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-697">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-697">'Razor'</span></span>
- <span data-ttu-id="2ef91-698">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-698">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-699">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-699">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-700">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-700">'Blazor'</span></span>
- <span data-ttu-id="2ef91-701">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-701">'Identity'</span></span>
- <span data-ttu-id="2ef91-702">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-702">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-703">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-703">'Razor'</span></span>
- <span data-ttu-id="2ef91-704">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-704">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-705">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-705">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-706">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-706">'Blazor'</span></span>
- <span data-ttu-id="2ef91-707">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-707">'Identity'</span></span>
- <span data-ttu-id="2ef91-708">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-708">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-709">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-709">'Razor'</span></span>
- <span data-ttu-id="2ef91-710">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-710">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-711">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-711">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-712">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-712">'Blazor'</span></span>
- <span data-ttu-id="2ef91-713">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-713">'Identity'</span></span>
- <span data-ttu-id="2ef91-714">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-714">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-715">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-715">'Razor'</span></span>
- <span data-ttu-id="2ef91-716">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-716">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-717">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-717">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-718">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-718">'Blazor'</span></span>
- <span data-ttu-id="2ef91-719">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-719">'Identity'</span></span>
- <span data-ttu-id="2ef91-720">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-720">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-721">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-721">'Razor'</span></span>
- <span data-ttu-id="2ef91-722">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-722">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-723">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-723">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-724">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-724">'Blazor'</span></span>
- <span data-ttu-id="2ef91-725">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-725">'Identity'</span></span>
- <span data-ttu-id="2ef91-726">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-726">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-727">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-727">'Razor'</span></span>
- <span data-ttu-id="2ef91-728">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-728">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-729">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-729">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-730">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-730">'Blazor'</span></span>
- <span data-ttu-id="2ef91-731">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-731">'Identity'</span></span>
- <span data-ttu-id="2ef91-732">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-732">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-733">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-733">'Razor'</span></span>
- <span data-ttu-id="2ef91-734">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-734">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-735">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-735">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-736">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-736">'Blazor'</span></span>
- <span data-ttu-id="2ef91-737">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-737">'Identity'</span></span>
- <span data-ttu-id="2ef91-738">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-738">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-739">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-739">'Razor'</span></span>
- <span data-ttu-id="2ef91-740">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-740">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-741">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-741">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-742">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-742">'Blazor'</span></span>
- <span data-ttu-id="2ef91-743">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-743">'Identity'</span></span>
- <span data-ttu-id="2ef91-744">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-744">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-745">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-745">'Razor'</span></span>
- <span data-ttu-id="2ef91-746">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-746">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-747">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-747">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-748">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-748">'Blazor'</span></span>
- <span data-ttu-id="2ef91-749">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-749">'Identity'</span></span>
- <span data-ttu-id="2ef91-750">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-750">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-751">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-751">'Razor'</span></span>
- <span data-ttu-id="2ef91-752">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-752">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-753">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-753">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-754">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-754">'Blazor'</span></span>
- <span data-ttu-id="2ef91-755">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-755">'Identity'</span></span>
- <span data-ttu-id="2ef91-756">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-756">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-757">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-757">'Razor'</span></span>
- <span data-ttu-id="2ef91-758">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-758">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-759">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-759">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-760">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-760">'Blazor'</span></span>
- <span data-ttu-id="2ef91-761">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-761">'Identity'</span></span>
- <span data-ttu-id="2ef91-762">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-762">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-763">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-763">'Razor'</span></span>
- <span data-ttu-id="2ef91-764">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-764">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-765">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-765">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-766">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-766">'Blazor'</span></span>
- <span data-ttu-id="2ef91-767">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-767">'Identity'</span></span>
- <span data-ttu-id="2ef91-768">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-768">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-769">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-769">'Razor'</span></span>
- <span data-ttu-id="2ef91-770">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-770">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-771">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-771">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-772">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-772">'Blazor'</span></span>
- <span data-ttu-id="2ef91-773">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-773">'Identity'</span></span>
- <span data-ttu-id="2ef91-774">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-774">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-775">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-775">'Razor'</span></span>
- <span data-ttu-id="2ef91-776">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-776">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-777">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-777">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-778">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-778">'Blazor'</span></span>
- <span data-ttu-id="2ef91-779">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-779">'Identity'</span></span>
- <span data-ttu-id="2ef91-780">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-780">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-781">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-781">'Razor'</span></span>
- <span data-ttu-id="2ef91-782">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-782">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-783">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-783">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-784">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-784">'Blazor'</span></span>
- <span data-ttu-id="2ef91-785">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-785">'Identity'</span></span>
- <span data-ttu-id="2ef91-786">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-786">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-787">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-787">'Razor'</span></span>
- <span data-ttu-id="2ef91-788">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-788">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-789">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-789">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-790">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-790">'Blazor'</span></span>
- <span data-ttu-id="2ef91-791">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-791">'Identity'</span></span>
- <span data-ttu-id="2ef91-792">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-792">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-793">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-793">'Razor'</span></span>
- <span data-ttu-id="2ef91-794">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-794">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-795">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-795">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-796">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-796">'Blazor'</span></span>
- <span data-ttu-id="2ef91-797">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-797">'Identity'</span></span>
- <span data-ttu-id="2ef91-798">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-798">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-799">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-799">'Razor'</span></span>
- <span data-ttu-id="2ef91-800">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-800">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-801">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-801">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-802">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-802">'Blazor'</span></span>
- <span data-ttu-id="2ef91-803">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-803">'Identity'</span></span>
- <span data-ttu-id="2ef91-804">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-804">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-805">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-805">'Razor'</span></span>
- <span data-ttu-id="2ef91-806">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-806">'SignalR' uid:</span></span> 

<span data-ttu-id="2ef91-807">---------------------------------------- | |`CUSTOMCONNSTR_{KEY} `   | `ConnectionStrings:{KEY}`  |未建立設定專案。</span><span class="sxs-lookup"><span data-stu-id="2ef91-807">---------------------------------------- | | `CUSTOMCONNSTR_{KEY} `   | `ConnectionStrings:{KEY}`   | Configuration entry not created.</span></span>                                                <span data-ttu-id="2ef91-808">| |`MYSQLCONNSTR_{KEY}`     | `ConnectionStrings:{KEY}`  |機碼 `ConnectionStrings:{KEY}_ProviderName` ：</span><span class="sxs-lookup"><span data-stu-id="2ef91-808">| | `MYSQLCONNSTR_{KEY}`     | `ConnectionStrings:{KEY}`   | Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="2ef91-809">值： `MySql.Data.MySqlClient` | | `SQLAZURECONNSTR_{KEY}`   |  `ConnectionStrings:{KEY}`  |機碼 `ConnectionStrings:{KEY}_ProviderName` ：</span><span class="sxs-lookup"><span data-stu-id="2ef91-809">Value: `MySql.Data.MySqlClient` | | `SQLAZURECONNSTR_{KEY}`  | `ConnectionStrings:{KEY}`   | Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="2ef91-810">值： `System.Data.SqlClient` | | `SQLCONNSTR_{KEY}`        |  `ConnectionStrings:{KEY}`  |機碼 `ConnectionStrings:{KEY}_ProviderName` ：</span><span class="sxs-lookup"><span data-stu-id="2ef91-810">Value: `System.Data.SqlClient`  | | `SQLCONNSTR_{KEY}`       | `ConnectionStrings:{KEY}`   | Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="2ef91-811">Value`System.Data.SqlClient`  |</span><span class="sxs-lookup"><span data-stu-id="2ef91-811">Value: `System.Data.SqlClient`  |</span></span>

<a name="jcp"></a>

### <a name="json-configuration-provider"></a><span data-ttu-id="2ef91-812">JSON 設定提供者</span><span class="sxs-lookup"><span data-stu-id="2ef91-812">JSON configuration provider</span></span>

<span data-ttu-id="2ef91-813">會 <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> 從 JSON 檔案索引鍵/值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="2ef91-813">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs.</span></span>

<span data-ttu-id="2ef91-814">多載可以指定：</span><span class="sxs-lookup"><span data-stu-id="2ef91-814">Overloads can specify:</span></span>

* <span data-ttu-id="2ef91-815">檔案是否為選擇性。</span><span class="sxs-lookup"><span data-stu-id="2ef91-815">Whether the file is optional.</span></span>
* <span data-ttu-id="2ef91-816">檔案變更時是否要重新載入設定。</span><span class="sxs-lookup"><span data-stu-id="2ef91-816">Whether the configuration is reloaded if the file changes.</span></span>

<span data-ttu-id="2ef91-817">請考慮下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="2ef91-817">Consider the following code:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramJSON.cs?name=snippet&highlight=12-14)]

<span data-ttu-id="2ef91-818">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="2ef91-818">The preceding code:</span></span>

* <span data-ttu-id="2ef91-819">設定 JSON 設定提供者以使用下列選項載入*myconfig.xml* ：</span><span class="sxs-lookup"><span data-stu-id="2ef91-819">Configures the JSON configuration provider to load the *MyConfig.json* file with the following options:</span></span>
  * <span data-ttu-id="2ef91-820">`optional: true`：檔案是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="2ef91-820">`optional: true`: The file is optional.</span></span>
  * <span data-ttu-id="2ef91-821">`reloadOnChange: true`：儲存變更時，會重載檔案。</span><span class="sxs-lookup"><span data-stu-id="2ef91-821">`reloadOnChange: true` : The file is reloaded when changes are saved.</span></span>
* <span data-ttu-id="2ef91-822">在*myconfig.xml json*檔案之前讀取預設的設定[提供者](#default)。</span><span class="sxs-lookup"><span data-stu-id="2ef91-822">Reads the [default configuration providers](#default) before the *MyConfig.json* file.</span></span> <span data-ttu-id="2ef91-823">預設設定提供者（包括[環境變數設定提供者](#evcp)和[命令列設定提供者](#clcp)）中的*myconfig.xml*檔案覆寫設定。</span><span class="sxs-lookup"><span data-stu-id="2ef91-823">Settings in the *MyConfig.json* file override setting in the default configuration providers, including the [Environment variables configuration provider](#evcp) and the [Command-line configuration provider](#clcp).</span></span>

<span data-ttu-id="2ef91-824">您通常***不***會想要覆寫[環境變數設定提供者](#evcp)和[命令列設定提供者](#clcp)中所設定之值的自訂 JSON 檔案。</span><span class="sxs-lookup"><span data-stu-id="2ef91-824">You typically ***don't*** want a custom JSON file overriding values set in the [Environment variables configuration provider](#evcp) and the [Command-line configuration provider](#clcp).</span></span>

<span data-ttu-id="2ef91-825">下列程式碼會清除所有設定提供者，並新增數個設定提供者：</span><span class="sxs-lookup"><span data-stu-id="2ef91-825">The following code clears all the configuration providers and adds several configuration providers:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramJSON2.cs?name=snippet)]

<span data-ttu-id="2ef91-826">在上述程式碼中， *myconfig.xml*中的設定和*myconfig.xml* `Environment` 。*json*檔案：</span><span class="sxs-lookup"><span data-stu-id="2ef91-826">In the preceding code, settings in the *MyConfig.json* and  *MyConfig*.`Environment`.*json* files:</span></span>

* <span data-ttu-id="2ef91-827">覆寫*appsettings*中的設定和*appsettings* `Environment` 。*json*檔案。</span><span class="sxs-lookup"><span data-stu-id="2ef91-827">Override settings in the *appsettings.json* and *appsettings*.`Environment`.*json* files.</span></span>
* <span data-ttu-id="2ef91-828">會由[環境變數設定提供者](#evcp)和[命令列設定提供者](#clcp)中的設定覆寫。</span><span class="sxs-lookup"><span data-stu-id="2ef91-828">Are overridden by settings in the [Environment variables configuration provider](#evcp) and the [Command-line configuration provider](#clcp).</span></span>

<span data-ttu-id="2ef91-829">[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)包含下列*myconfig.xml*檔案：</span><span class="sxs-lookup"><span data-stu-id="2ef91-829">The [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) contains the following  *MyConfig.json* file:</span></span>

[!code-json[](index/samples/3.x/ConfigSample/MyConfig.json)]

<span data-ttu-id="2ef91-830">[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)中的下列程式碼會顯示上述幾個設定值：</span><span class="sxs-lookup"><span data-stu-id="2ef91-830">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) displays several of the preceding configurations settings:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

<a name="fcp"></a>

## <a name="file-configuration-provider"></a><span data-ttu-id="2ef91-831">檔案設定提供者</span><span class="sxs-lookup"><span data-stu-id="2ef91-831">File configuration provider</span></span>

<span data-ttu-id="2ef91-832"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> 是用於從檔案系統載入設定的基底類別。</span><span class="sxs-lookup"><span data-stu-id="2ef91-832"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="2ef91-833">下列是衍生自的設定提供者 `FileConfigurationProvider` ：</span><span class="sxs-lookup"><span data-stu-id="2ef91-833">The following configuration providers derive from `FileConfigurationProvider`:</span></span>

* [<span data-ttu-id="2ef91-834">INI 設定提供者</span><span class="sxs-lookup"><span data-stu-id="2ef91-834">INI configuration provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="2ef91-835">JSON 設定提供者</span><span class="sxs-lookup"><span data-stu-id="2ef91-835">JSON configuration provider</span></span>](#jcp)
* [<span data-ttu-id="2ef91-836">XML 設定提供者</span><span class="sxs-lookup"><span data-stu-id="2ef91-836">XML configuration provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="2ef91-837">INI 設定提供者</span><span class="sxs-lookup"><span data-stu-id="2ef91-837">INI configuration provider</span></span>

<span data-ttu-id="2ef91-838"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> 會在執行階段從 INI 檔案機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="2ef91-838">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="2ef91-839">下列程式碼會清除所有設定提供者，並新增數個設定提供者：</span><span class="sxs-lookup"><span data-stu-id="2ef91-839">The following code clears all the configuration providers and adds several configuration providers:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramINI.cs?name=snippet&highlight=10-30)]

<span data-ttu-id="2ef91-840">在上述程式碼中， *MyIniConfig*和*MyIniConfig*中的 `Environment` 設定。*ini*檔案會由中的設定覆寫：</span><span class="sxs-lookup"><span data-stu-id="2ef91-840">In the preceding code, settings in the *MyIniConfig.ini* and  *MyIniConfig*.`Environment`.*ini* files are overridden by settings in the:</span></span>

* [<span data-ttu-id="2ef91-841">環境變數設定提供者</span><span class="sxs-lookup"><span data-stu-id="2ef91-841">Environment variables configuration provider</span></span>](#evcp)
* <span data-ttu-id="2ef91-842">[命令列設定提供者](#clcp)。</span><span class="sxs-lookup"><span data-stu-id="2ef91-842">[Command-line configuration provider](#clcp).</span></span>

<span data-ttu-id="2ef91-843">[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)包含下列*MyIniConfig .ini*檔案：</span><span class="sxs-lookup"><span data-stu-id="2ef91-843">The [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) contains the following *MyIniConfig.ini* file:</span></span>

[!code-ini[](index/samples/3.x/ConfigSample/MyIniConfig.ini)]

<span data-ttu-id="2ef91-844">[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)中的下列程式碼會顯示上述幾個設定值：</span><span class="sxs-lookup"><span data-stu-id="2ef91-844">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) displays several of the preceding configurations settings:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

### <a name="xml-configuration-provider"></a><span data-ttu-id="2ef91-845">XML 設定提供者</span><span class="sxs-lookup"><span data-stu-id="2ef91-845">XML configuration provider</span></span>

<span data-ttu-id="2ef91-846"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> 會在執行階段從 XML 檔案機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="2ef91-846">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="2ef91-847">下列程式碼會清除所有設定提供者，並新增數個設定提供者：</span><span class="sxs-lookup"><span data-stu-id="2ef91-847">The following code clears all the configuration providers and adds several configuration providers:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramXML.cs?name=snippet)]

<span data-ttu-id="2ef91-848">在上述程式碼中， *MyXMLFile*和*MyXMLFile*中的 `Environment` 設定。中的設定會覆寫*xml*檔案：</span><span class="sxs-lookup"><span data-stu-id="2ef91-848">In the preceding code, settings in the *MyXMLFile.xml* and  *MyXMLFile*.`Environment`.*xml* files are overridden by settings in the:</span></span>

* [<span data-ttu-id="2ef91-849">環境變數設定提供者</span><span class="sxs-lookup"><span data-stu-id="2ef91-849">Environment variables configuration provider</span></span>](#evcp)
* <span data-ttu-id="2ef91-850">[命令列設定提供者](#clcp)。</span><span class="sxs-lookup"><span data-stu-id="2ef91-850">[Command-line configuration provider](#clcp).</span></span>

<span data-ttu-id="2ef91-851">[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)包含下列*MyXMLFile*檔案：</span><span class="sxs-lookup"><span data-stu-id="2ef91-851">The [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) contains the following *MyXMLFile.xml* file:</span></span>

[!code-xml[](index/samples/3.x/ConfigSample/MyXMLFile.xml)]

<span data-ttu-id="2ef91-852">[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)中的下列程式碼會顯示上述幾個設定值：</span><span class="sxs-lookup"><span data-stu-id="2ef91-852">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) displays several of the preceding configurations settings:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

<span data-ttu-id="2ef91-853">若 `name` 屬性是用來區別元素，則可以重複那些使用相同元素名稱的元素：</span><span class="sxs-lookup"><span data-stu-id="2ef91-853">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

[!code-xml[](index/samples/3.x/ConfigSample/MyXMLFile3.xml)]

<span data-ttu-id="2ef91-854">下列程式碼會讀取先前的設定檔，並顯示金鑰和值：</span><span class="sxs-lookup"><span data-stu-id="2ef91-854">The following code reads the previous configuration file and displays the keys and values:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/XML/Index.cshtml.cs?name=snippet)]

<span data-ttu-id="2ef91-855">屬性可用來提供值：</span><span class="sxs-lookup"><span data-stu-id="2ef91-855">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="2ef91-856">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="2ef91-856">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="2ef91-857">key:attribute</span><span class="sxs-lookup"><span data-stu-id="2ef91-857">key:attribute</span></span>
* <span data-ttu-id="2ef91-858">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="2ef91-858">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="2ef91-859">每個檔案的索引鍵設定提供者</span><span class="sxs-lookup"><span data-stu-id="2ef91-859">Key-per-file configuration provider</span></span>

<span data-ttu-id="2ef91-860"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> 使用目錄的檔案做為設定機碼值組。</span><span class="sxs-lookup"><span data-stu-id="2ef91-860">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="2ef91-861">機碼是檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="2ef91-861">The key is the file name.</span></span> <span data-ttu-id="2ef91-862">值包含檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="2ef91-862">The value contains the file's contents.</span></span> <span data-ttu-id="2ef91-863">Docker 裝載案例中會使用每個檔案的索引鍵設定提供者。</span><span class="sxs-lookup"><span data-stu-id="2ef91-863">The Key-per-file configuration provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="2ef91-864">若要啟用每個檔案機碼設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="2ef91-864">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="2ef91-865">檔案的 `directoryPath` 必須是絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="2ef91-865">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="2ef91-866">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="2ef91-866">Overloads permit specifying:</span></span>

* <span data-ttu-id="2ef91-867">設定來源的 `Action<KeyPerFileConfigurationSource>` 委派。</span><span class="sxs-lookup"><span data-stu-id="2ef91-867">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="2ef91-868">目錄是否為選擇性與目錄的路徑。</span><span class="sxs-lookup"><span data-stu-id="2ef91-868">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="2ef91-869">雙底線 (`__`) 是做為檔案名稱中的設定金鑰分隔符號使用。</span><span class="sxs-lookup"><span data-stu-id="2ef91-869">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="2ef91-870">例如，檔案名稱 `Logging__LogLevel__System` 會產生設定金鑰 `Logging:LogLevel:System`。</span><span class="sxs-lookup"><span data-stu-id="2ef91-870">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="2ef91-871">建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定：</span><span class="sxs-lookup"><span data-stu-id="2ef91-871">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

<a name="mcp"></a>

## <a name="memory-configuration-provider"></a><span data-ttu-id="2ef91-872">記憶體設定提供者</span><span class="sxs-lookup"><span data-stu-id="2ef91-872">Memory configuration provider</span></span>

<span data-ttu-id="2ef91-873"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> 使用記憶體內集合做為設定機碼值組。</span><span class="sxs-lookup"><span data-stu-id="2ef91-873">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="2ef91-874">下列程式碼會將記憶體集合新增至設定系統：</span><span class="sxs-lookup"><span data-stu-id="2ef91-874">The following code adds a memory collection to the configuration system:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramArray.cs?name=snippet6)]

<span data-ttu-id="2ef91-875">下列來自[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)的程式碼會顯示先前的設定值：</span><span class="sxs-lookup"><span data-stu-id="2ef91-875">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) displays the preceding configurations settings:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

<span data-ttu-id="2ef91-876">在上述程式碼中， `config.AddInMemoryCollection(Dict)` 會在預設的設定[提供者](#default)之後加入。</span><span class="sxs-lookup"><span data-stu-id="2ef91-876">In the preceding code, `config.AddInMemoryCollection(Dict)` is added after the [default configuration providers](#default).</span></span> <span data-ttu-id="2ef91-877">如需排序設定提供者的範例，請參閱[JSON 設定提供者](#jcp)。</span><span class="sxs-lookup"><span data-stu-id="2ef91-877">For an example of ordering the configuration providers, see [JSON configuration provider](#jcp).</span></span>

<span data-ttu-id="2ef91-878">如需其他範例，請參閱使用來系結[陣列](#boa) `MemoryConfigurationProvider` 。</span><span class="sxs-lookup"><span data-stu-id="2ef91-878">See [Bind an array](#boa) for another example using `MemoryConfigurationProvider`.</span></span>

## <a name="getvalue"></a><span data-ttu-id="2ef91-879">GetValue</span><span class="sxs-lookup"><span data-stu-id="2ef91-879">GetValue</span></span>

<span data-ttu-id="2ef91-880">[`ConfigurationBinder.GetValue<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*)使用指定的索引鍵從設定中解壓縮單一值，並將其轉換為指定的類型：</span><span class="sxs-lookup"><span data-stu-id="2ef91-880">[`ConfigurationBinder.GetValue<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a single value from configuration with a specified key and converts it to the specified type:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/TestNum.cshtml.cs?name=snippet)]

<span data-ttu-id="2ef91-881">在上述程式碼中，如果在設定 `NumberKey` 中找不到，則會使用的預設值 `99` 。</span><span class="sxs-lookup"><span data-stu-id="2ef91-881">In the preceding code,  if `NumberKey` isn't found in the configuration, the default value of `99` is used.</span></span>

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="2ef91-882">GetSection、GetChildren 與 Exists</span><span class="sxs-lookup"><span data-stu-id="2ef91-882">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="2ef91-883">針對接下來的範例，請考慮下列*MySubsection*檔：</span><span class="sxs-lookup"><span data-stu-id="2ef91-883">For the examples that follow, consider the following *MySubsection.json* file:</span></span>

[!code-json[](index/samples/3.x/ConfigSample/MySubsection.json)]

<span data-ttu-id="2ef91-884">下列程式碼會將*MySubsection*新增至設定提供者：</span><span class="sxs-lookup"><span data-stu-id="2ef91-884">The following code adds *MySubsection.json* to the configuration providers:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramJSONsection.cs?name=snippet)]

### <a name="getsection"></a><span data-ttu-id="2ef91-885">GetSection</span><span class="sxs-lookup"><span data-stu-id="2ef91-885">GetSection</span></span>

<span data-ttu-id="2ef91-886">[IConfiguration。 GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*)會傳回具有指定子區段索引鍵的設定子區段。</span><span class="sxs-lookup"><span data-stu-id="2ef91-886">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) returns a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="2ef91-887">下列程式碼會傳回的值 `section1` ：</span><span class="sxs-lookup"><span data-stu-id="2ef91-887">The following code returns values for `section1`:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/TestSection.cshtml.cs?name=snippet)]

<span data-ttu-id="2ef91-888">下列程式碼會傳回的值 `section2:subsection0` ：</span><span class="sxs-lookup"><span data-stu-id="2ef91-888">The following code returns values for `section2:subsection0`:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/TestSection2.cshtml.cs?name=snippet)]

<span data-ttu-id="2ef91-889">`GetSection` 絕不會傳回 `null`。</span><span class="sxs-lookup"><span data-stu-id="2ef91-889">`GetSection` never returns `null`.</span></span> <span data-ttu-id="2ef91-890">若找不到相符的區段，會傳回空的 `IConfigurationSection`。</span><span class="sxs-lookup"><span data-stu-id="2ef91-890">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="2ef91-891">當 `GetSection` 傳回相符區段時，未填入 <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value>。</span><span class="sxs-lookup"><span data-stu-id="2ef91-891">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="2ef91-892">當區段存在時，會傳回 <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> 與 <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path>。</span><span class="sxs-lookup"><span data-stu-id="2ef91-892">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren-and-exists"></a><span data-ttu-id="2ef91-893">GetChildren 和 Exists</span><span class="sxs-lookup"><span data-stu-id="2ef91-893">GetChildren and Exists</span></span>

<span data-ttu-id="2ef91-894">下列程式碼會呼叫[IConfiguration. GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) ，並傳回的值 `section2:subsection0` ：</span><span class="sxs-lookup"><span data-stu-id="2ef91-894">The following code calls [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) and returns values for `section2:subsection0`:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/TestSection4.cshtml.cs?name=snippet)]

<span data-ttu-id="2ef91-895">上述程式碼會呼叫[microsoft.extensions.options.configurationextensions](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) ，以確認區段是否存在：</span><span class="sxs-lookup"><span data-stu-id="2ef91-895">The preceding code calls [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to verify the  section exists:</span></span>

 <a name="boa"></a>

## <a name="bind-an-array"></a><span data-ttu-id="2ef91-896">系結陣列</span><span class="sxs-lookup"><span data-stu-id="2ef91-896">Bind an array</span></span>

<span data-ttu-id="2ef91-897">[ConfigurationBinder](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*)支援在設定機碼中使用陣列索引將陣列系結至物件。</span><span class="sxs-lookup"><span data-stu-id="2ef91-897">The [ConfigurationBinder.Bind](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*) supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="2ef91-898">任何會公開數值索引鍵的陣列格式，都可以系結至[POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object)類別陣列。</span><span class="sxs-lookup"><span data-stu-id="2ef91-898">Any array format that exposes a numeric key segment is capable of array binding to a [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) class array.</span></span>

<span data-ttu-id="2ef91-899">請考慮[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)中的*MyArray* ：</span><span class="sxs-lookup"><span data-stu-id="2ef91-899">Consider *MyArray.json* from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample):</span></span>

[!code-json[](index/samples/3.x/ConfigSample/MyArray.json)]

<span data-ttu-id="2ef91-900">下列程式碼會將*MyArray*新增至設定提供者：</span><span class="sxs-lookup"><span data-stu-id="2ef91-900">The following code adds *MyArray.json* to the configuration providers:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramJSONarray.cs?name=snippet)]

<span data-ttu-id="2ef91-901">下列程式碼會讀取設定並顯示值：</span><span class="sxs-lookup"><span data-stu-id="2ef91-901">The following code reads the configuration and displays the values:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Array.cshtml.cs?name=snippet)]

<span data-ttu-id="2ef91-902">上述程式碼會傳回下列輸出：</span><span class="sxs-lookup"><span data-stu-id="2ef91-902">The preceding code returns the following output:</span></span>

```text
Index: 0  Value: value00
Index: 1  Value: value10
Index: 2  Value: value20
Index: 3  Value: value40
Index: 4  Value: value50
```

<span data-ttu-id="2ef91-903">在上述輸出中，索引3具有值 `value40` ，對應于 `"4": "value40",` *MyArray*中的。</span><span class="sxs-lookup"><span data-stu-id="2ef91-903">In the preceding output, Index 3 has value `value40`, corresponding to `"4": "value40",` in *MyArray.json*.</span></span> <span data-ttu-id="2ef91-904">系結的陣列索引是連續的，而且不會系結至設定金鑰索引。</span><span class="sxs-lookup"><span data-stu-id="2ef91-904">The bound array indices are continuous and not bound to the configuration key index.</span></span> <span data-ttu-id="2ef91-905">設定系結器無法系結 null 值，或在系結物件中建立 null 專案</span><span class="sxs-lookup"><span data-stu-id="2ef91-905">The configuration binder isn't capable of binding null values or creating null entries in bound objects</span></span>

<span data-ttu-id="2ef91-906">下列程式碼會 `array:entries` 使用擴充方法來載入設定 <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> ：</span><span class="sxs-lookup"><span data-stu-id="2ef91-906">The  following code loads the `array:entries` configuration with the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramArray.cs?name=snippet)]

<span data-ttu-id="2ef91-907">下列程式碼會讀取中的設定 `arrayDict` `Dictionary` ，並顯示值：</span><span class="sxs-lookup"><span data-stu-id="2ef91-907">The following code reads the configuration in the `arrayDict` `Dictionary` and displays the values:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Array.cshtml.cs?name=snippet)]

<span data-ttu-id="2ef91-908">上述程式碼會傳回下列輸出：</span><span class="sxs-lookup"><span data-stu-id="2ef91-908">The preceding code returns the following output:</span></span>

```text
Index: 0  Value: value0
Index: 1  Value: value1
Index: 2  Value: value2
Index: 3  Value: value4
Index: 4  Value: value5
```

<span data-ttu-id="2ef91-909">繫結物件中的索引 &num;3 存放 `array:4` 設定機碼與其 `value4` 的設定資料。</span><span class="sxs-lookup"><span data-stu-id="2ef91-909">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="2ef91-910">當系結包含陣列的設定資料時，在建立物件時，會使用設定機碼中的陣列索引來反復查看設定資料。</span><span class="sxs-lookup"><span data-stu-id="2ef91-910">When configuration data containing an array is bound, the array indices in the configuration keys are used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="2ef91-911">設定資料中不能保留 Null 值，當設定機碼中的陣列略過一或多個索引時，不會在繫結物件中建立 Null 值項目。</span><span class="sxs-lookup"><span data-stu-id="2ef91-911">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="2ef91-912">在系 &num; 結至實例之前，您可以先提供索引3的遺漏設定專案 `ArrayExample` ，以讀取索引 &num; 3 鍵/值組的任何設定提供者。</span><span class="sxs-lookup"><span data-stu-id="2ef91-912">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that reads the index &num;3 key/value pair.</span></span> <span data-ttu-id="2ef91-913">請考慮下列來自範例下載的*Value3 json*檔案：</span><span class="sxs-lookup"><span data-stu-id="2ef91-913">Consider the following *Value3.json* file from the sample download:</span></span>

[!code-json[](index/samples/3.x/ConfigSample/Value3.json)]

<span data-ttu-id="2ef91-914">下列程式碼包含*Value3*的設定和 `arrayDict` `Dictionary` ：</span><span class="sxs-lookup"><span data-stu-id="2ef91-914">The following code includes configuration for *Value3.json* and the `arrayDict` `Dictionary`:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramArray.cs?name=snippet2)]

<span data-ttu-id="2ef91-915">下列程式碼會讀取先前的設定，並顯示值：</span><span class="sxs-lookup"><span data-stu-id="2ef91-915">The following code reads the preceding configuration and displays the values:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Array.cshtml.cs?name=snippet)]

<span data-ttu-id="2ef91-916">上述程式碼會傳回下列輸出：</span><span class="sxs-lookup"><span data-stu-id="2ef91-916">The preceding code returns the following output:</span></span>

```text
Index: 0  Value: value0
Index: 1  Value: value1
Index: 2  Value: value2
Index: 3  Value: value3
Index: 4  Value: value4
Index: 5  Value: value5
```

<span data-ttu-id="2ef91-917">自訂設定提供者不需要實作陣列繫結。</span><span class="sxs-lookup"><span data-stu-id="2ef91-917">Custom configuration providers aren't required to implement array binding.</span></span>

## <a name="custom-configuration-provider"></a><span data-ttu-id="2ef91-918">自訂設定提供者</span><span class="sxs-lookup"><span data-stu-id="2ef91-918">Custom configuration provider</span></span>

<span data-ttu-id="2ef91-919">範例應用程式示範如何建立會使用 [Entity Framework (EF)](/ef/core/) 從資料庫讀取設定機碼值組的基本設定提供者。</span><span class="sxs-lookup"><span data-stu-id="2ef91-919">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="2ef91-920">提供者具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="2ef91-920">The provider has the following characteristics:</span></span>

* <span data-ttu-id="2ef91-921">EF 記憶體內資料庫會用於示範用途。</span><span class="sxs-lookup"><span data-stu-id="2ef91-921">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="2ef91-922">若要使用需要連接字串的資料庫，請實作第二個 `ConfigurationBuilder` 以從另一個設定提供者提供連接字串。</span><span class="sxs-lookup"><span data-stu-id="2ef91-922">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="2ef91-923">啟動時，該提供者會將資料庫資料表讀入到設定中。</span><span class="sxs-lookup"><span data-stu-id="2ef91-923">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="2ef91-924">該提供者不會以個別機碼為基礎查詢資料庫。</span><span class="sxs-lookup"><span data-stu-id="2ef91-924">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="2ef91-925">未實作變更時重新載入，因此在應用程式啟動後更新資料庫對應用程式設定沒有影響。</span><span class="sxs-lookup"><span data-stu-id="2ef91-925">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="2ef91-926">定義 `EFConfigurationValue` 實體來在資料庫中存放設定值。</span><span class="sxs-lookup"><span data-stu-id="2ef91-926">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="2ef91-927">*Models/EFConfigurationValue.cs*：</span><span class="sxs-lookup"><span data-stu-id="2ef91-927">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="2ef91-928">新增 `EFConfigurationContext` 以存放及存取已設定的值。</span><span class="sxs-lookup"><span data-stu-id="2ef91-928">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="2ef91-929">*EFConfigurationProvider/EFConfigurationContext.cs*：</span><span class="sxs-lookup"><span data-stu-id="2ef91-929">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="2ef91-930">建立會實作 <xref:Microsoft.Extensions.Configuration.IConfigurationSource> 的類別。</span><span class="sxs-lookup"><span data-stu-id="2ef91-930">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="2ef91-931">*EFConfigurationProvider/EFConfigurationSource.cs*：</span><span class="sxs-lookup"><span data-stu-id="2ef91-931">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="2ef91-932">透過繼承自 <xref:Microsoft.Extensions.Configuration.ConfigurationProvider> 來建立自訂設定提供者。</span><span class="sxs-lookup"><span data-stu-id="2ef91-932">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="2ef91-933">若資料庫是空的，設定提供者會初始化資料庫。</span><span class="sxs-lookup"><span data-stu-id="2ef91-933">The configuration provider initializes the database when it's empty.</span></span> <span data-ttu-id="2ef91-934">由於設定索引[鍵不區分大小寫](#keys)，用來初始化資料庫的字典會以不區分大小寫的比較子（[StringComparer. OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)）來建立。</span><span class="sxs-lookup"><span data-stu-id="2ef91-934">Since [configuration keys are case-insensitive](#keys), the dictionary used to initialize the database is created with the case-insensitive comparer ([StringComparer.OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)).</span></span>

<span data-ttu-id="2ef91-935">*EFConfigurationProvider/EFConfigurationProvider.cs*：</span><span class="sxs-lookup"><span data-stu-id="2ef91-935">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="2ef91-936">`AddEFConfiguration` 擴充方法允許新增設定來源到 `ConfigurationBuilder`。</span><span class="sxs-lookup"><span data-stu-id="2ef91-936">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="2ef91-937">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="2ef91-937">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="2ef91-938">下列程式碼顯示如何在 *Program.cs* 中使用自訂 `EFConfigurationProvider`：</span><span class="sxs-lookup"><span data-stu-id="2ef91-938">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

<a name="acs"></a>

## <a name="access-configuration-in-startup"></a><span data-ttu-id="2ef91-939">啟動時的存取設定</span><span class="sxs-lookup"><span data-stu-id="2ef91-939">Access configuration in Startup</span></span>

<span data-ttu-id="2ef91-940">下列程式碼會顯示方法中的設定資料 `Startup` ：</span><span class="sxs-lookup"><span data-stu-id="2ef91-940">The following code displays configuration data in `Startup` methods:</span></span>

[!code-csharp[](index/samples/3.x/ConfigSample/StartupKey.cs?name=snippet&highlight=13,18)]

<span data-ttu-id="2ef91-941">如需使用啟動方便方法來存取設定的範例，請參閱[應用程式啟動：方便方法](xref:fundamentals/startup#convenience-methods)。</span><span class="sxs-lookup"><span data-stu-id="2ef91-941">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-razor-pages"></a><span data-ttu-id="2ef91-942">頁面中的存取設定 Razor</span><span class="sxs-lookup"><span data-stu-id="2ef91-942">Access configuration in Razor Pages</span></span>

<span data-ttu-id="2ef91-943">下列程式碼會在頁面中顯示設定資料 Razor ：</span><span class="sxs-lookup"><span data-stu-id="2ef91-943">The following code displays configuration data in a Razor Page:</span></span>

[!code-cshtml[](index/samples/3.x/ConfigSample/Pages/Test5.cshtml)]

<span data-ttu-id="2ef91-944">在下列程式碼中， `MyOptions` 會新增至服務容器， <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> 並系結至設定：</span><span class="sxs-lookup"><span data-stu-id="2ef91-944">In the following code, `MyOptions` is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](~/fundamentals/configuration/options/samples/3.x/OptionsSample/Startup3.cs?name=snippet_Example2)]

<span data-ttu-id="2ef91-945">下列標記會使用指示詞 [`@inject`](xref:mvc/views/razor#inject) Razor 來解析並顯示選項值：</span><span class="sxs-lookup"><span data-stu-id="2ef91-945">The following markup uses the [`@inject`](xref:mvc/views/razor#inject) Razor directive to resolve and display the options values:</span></span>

[!code-cshtml[](~/fundamentals/configuration/options/samples/3.x/OptionsSample/Pages/Test3.cshtml)]

## <a name="access-configuration-in-a-mvc-view-file"></a><span data-ttu-id="2ef91-946">MVC 視圖檔案中的存取設定</span><span class="sxs-lookup"><span data-stu-id="2ef91-946">Access configuration in a MVC view file</span></span>

<span data-ttu-id="2ef91-947">下列程式碼會在 MVC 視圖中顯示設定資料：</span><span class="sxs-lookup"><span data-stu-id="2ef91-947">The following code displays configuration data in a MVC view:</span></span>

[!code-cshtml[](index/samples/3.x/ConfigSample/Views/Home2/Index.cshtml)]

## <a name="configure-options-with-a-delegate"></a><span data-ttu-id="2ef91-948">使用委派設定選項</span><span class="sxs-lookup"><span data-stu-id="2ef91-948">Configure options with a delegate</span></span>

<span data-ttu-id="2ef91-949">委派中設定的選項會覆寫設定提供者中所設定的值。</span><span class="sxs-lookup"><span data-stu-id="2ef91-949">Options configured in a delegate override values set in the configuration providers.</span></span>

<span data-ttu-id="2ef91-950">範例應用程式中的範例2會示範如何使用委派設定選項。</span><span class="sxs-lookup"><span data-stu-id="2ef91-950">Configuring options with a delegate is demonstrated as Example 2 in the sample app.</span></span>

<span data-ttu-id="2ef91-951">在下列程式碼中， <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 服務會加入至服務容器。</span><span class="sxs-lookup"><span data-stu-id="2ef91-951">In the following code, an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="2ef91-952">它會使用委派來設定的值 `MyOptions` ：</span><span class="sxs-lookup"><span data-stu-id="2ef91-952">It uses a delegate to configure values for `MyOptions`:</span></span>

[!code-csharp[](~/fundamentals/configuration/options/samples/3.x/OptionsSample/Startup2.cs?name=snippet_Example2)]

<span data-ttu-id="2ef91-953">下列程式碼會顯示選項值：</span><span class="sxs-lookup"><span data-stu-id="2ef91-953">The following code displays the options values:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Test2.cshtml.cs?name=snippet)]

<span data-ttu-id="2ef91-954">在上述範例中，和的值 `Option1` `Option2` 是在*appsettings*中指定，然後由設定的委派覆寫。</span><span class="sxs-lookup"><span data-stu-id="2ef91-954">In the preceding example, the values of `Option1` and `Option2` are specified in *appsettings.json* and then overridden by the configured delegate.</span></span>

<a name="hvac"></a>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="2ef91-955">主機與應用程式組態的比較</span><span class="sxs-lookup"><span data-stu-id="2ef91-955">Host versus app configuration</span></span>

<span data-ttu-id="2ef91-956">設定及啟動應用程式之前，會先設定及啟動「主機」\*\*。</span><span class="sxs-lookup"><span data-stu-id="2ef91-956">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="2ef91-957">主機負責應用程式啟動和存留期管理。</span><span class="sxs-lookup"><span data-stu-id="2ef91-957">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="2ef91-958">應用程式與主機都是使用此主題中所述的設定提供者來設定的。</span><span class="sxs-lookup"><span data-stu-id="2ef91-958">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="2ef91-959">主機組態機碼/值組也會包含在應用程式的組態中。</span><span class="sxs-lookup"><span data-stu-id="2ef91-959">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="2ef91-960">如需有關當建置主機時如何使用設定提供者的詳細資訊，以及設定來源如何影響主機設定的詳細資訊，請參閱 <xref:fundamentals/index#host>。</span><span class="sxs-lookup"><span data-stu-id="2ef91-960">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

<a name="dhc"></a>

## <a name="default-host-configuration"></a><span data-ttu-id="2ef91-961">預設主機設定</span><span class="sxs-lookup"><span data-stu-id="2ef91-961">Default host configuration</span></span>

<span data-ttu-id="2ef91-962">如需使用 [Web 主機](xref:fundamentals/host/web-host)時預設組態的詳細資料，請參閱[本主題的 ASP.NET Core 2.2 版本](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2)。</span><span class="sxs-lookup"><span data-stu-id="2ef91-962">For details on the default configuration when using the [Web Host](xref:fundamentals/host/web-host), see the [ASP.NET Core 2.2 version of this topic](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span></span>

* <span data-ttu-id="2ef91-963">主機組態的提供來源：</span><span class="sxs-lookup"><span data-stu-id="2ef91-963">Host configuration is provided from:</span></span>
  * <span data-ttu-id="2ef91-964">前面加上的環境變數 `DOTNET_` （例如 `DOTNET_ENVIRONMENT` ），並使用[環境變數設定提供者](#environment-variables-configuration-provider)。</span><span class="sxs-lookup"><span data-stu-id="2ef91-964">Environment variables prefixed with `DOTNET_` (for example, `DOTNET_ENVIRONMENT`) using the [Environment Variables configuration provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="2ef91-965">載入設定機碼值組時，會移除前置詞 (`DOTNET_`)。</span><span class="sxs-lookup"><span data-stu-id="2ef91-965">The prefix (`DOTNET_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="2ef91-966">使用[命令列設定提供者](#command-line-configuration-provider)的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="2ef91-966">Command-line arguments using the [Command-line configuration provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="2ef91-967">已建立 Web 主機預設組態 (`ConfigureWebHostDefaults`)：</span><span class="sxs-lookup"><span data-stu-id="2ef91-967">Web Host default configuration is established (`ConfigureWebHostDefaults`):</span></span>
  * <span data-ttu-id="2ef91-968">Kestrel 會用作為網頁伺服器，並使用應用程式的組態提供者來設定。</span><span class="sxs-lookup"><span data-stu-id="2ef91-968">Kestrel is used as the web server and configured using the app's configuration providers.</span></span>
  * <span data-ttu-id="2ef91-969">新增主機篩選中介軟體。</span><span class="sxs-lookup"><span data-stu-id="2ef91-969">Add Host Filtering Middleware.</span></span>
  * <span data-ttu-id="2ef91-970">如果 `ASPNETCORE_FORWARDEDHEADERS_ENABLED` 環境變數設定為 `true`，則會新增轉接的標頭中介軟體。</span><span class="sxs-lookup"><span data-stu-id="2ef91-970">Add Forwarded Headers Middleware if the `ASPNETCORE_FORWARDEDHEADERS_ENABLED` environment variable is set to `true`.</span></span>
  * <span data-ttu-id="2ef91-971">啟用 IIS 整合。</span><span class="sxs-lookup"><span data-stu-id="2ef91-971">Enable IIS integration.</span></span>

## <a name="other-configuration"></a><span data-ttu-id="2ef91-972">其他設定</span><span class="sxs-lookup"><span data-stu-id="2ef91-972">Other configuration</span></span>

<span data-ttu-id="2ef91-973">本主題僅適用于*應用程式*設定。</span><span class="sxs-lookup"><span data-stu-id="2ef91-973">This topic only pertains to *app configuration*.</span></span> <span data-ttu-id="2ef91-974">執行和裝載 ASP.NET Core 應用程式的其他層面，是使用本主題未涵蓋的設定檔來設定：</span><span class="sxs-lookup"><span data-stu-id="2ef91-974">Other aspects of running and hosting ASP.NET Core apps are configured using configuration files not covered in this topic:</span></span>

* <span data-ttu-id="2ef91-975">*啟動 json* /*launchsettings.json*是開發環境的工具設定檔，如下所述：</span><span class="sxs-lookup"><span data-stu-id="2ef91-975">*launch.json*/*launchSettings.json* are tooling configuration files for the Development environment, described:</span></span>
  * <span data-ttu-id="2ef91-976">在中 <xref:fundamentals/environments#development> 。</span><span class="sxs-lookup"><span data-stu-id="2ef91-976">In <xref:fundamentals/environments#development>.</span></span>
  * <span data-ttu-id="2ef91-977">在檔集內，用來為開發案例設定 ASP.NET Core 應用程式的檔案。</span><span class="sxs-lookup"><span data-stu-id="2ef91-977">Across the documentation set where the files are used to configure ASP.NET Core apps for Development scenarios.</span></span>
* <span data-ttu-id="2ef91-978">*web.config*是伺服器設定檔，如下列主題所述：</span><span class="sxs-lookup"><span data-stu-id="2ef91-978">*web.config* is a server configuration file, described in the following topics:</span></span>
  * <xref:host-and-deploy/iis/index>
  * <xref:host-and-deploy/aspnet-core-module>

<span data-ttu-id="2ef91-979">如需從舊版 ASP.NET 遷移應用程式設定的詳細資訊，請參閱 <xref:migration/proper-to-2x/index#store-configurations> 。</span><span class="sxs-lookup"><span data-stu-id="2ef91-979">For more information on migrating app configuration from earlier versions of ASP.NET, see <xref:migration/proper-to-2x/index#store-configurations>.</span></span>

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="2ef91-980">從外部組件新增設定</span><span class="sxs-lookup"><span data-stu-id="2ef91-980">Add configuration from an external assembly</span></span>

<span data-ttu-id="2ef91-981"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup> 實作允許在啟動時從應用程式 `Startup` 類別外部的外部組件，針對應用程式新增增強功能。</span><span class="sxs-lookup"><span data-stu-id="2ef91-981">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="2ef91-982">如需詳細資訊，請參閱<xref:fundamentals/configuration/platform-specific-configuration>。</span><span class="sxs-lookup"><span data-stu-id="2ef91-982">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2ef91-983">其他資源</span><span class="sxs-lookup"><span data-stu-id="2ef91-983">Additional resources</span></span>

* [<span data-ttu-id="2ef91-984">設定原始程式碼</span><span class="sxs-lookup"><span data-stu-id="2ef91-984">Configuration source code</span></span>](https://github.com/dotnet/extensions/tree/master/src/Configuration)
* <xref:fundamentals/configuration/options>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="2ef91-985">ASP.NET Core 中的應用程式設定是以由*設定提供者*所建立的機碼值組為基礎。</span><span class="sxs-lookup"><span data-stu-id="2ef91-985">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="2ef91-986">設定提供者會從各種設定來源將設定資料讀取到機碼值組中：</span><span class="sxs-lookup"><span data-stu-id="2ef91-986">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="2ef91-987">Azure 金鑰保存庫</span><span class="sxs-lookup"><span data-stu-id="2ef91-987">Azure Key Vault</span></span>
* <span data-ttu-id="2ef91-988">Azure 應用程式組態</span><span class="sxs-lookup"><span data-stu-id="2ef91-988">Azure App Configuration</span></span>
* <span data-ttu-id="2ef91-989">命令列引數</span><span class="sxs-lookup"><span data-stu-id="2ef91-989">Command-line arguments</span></span>
* <span data-ttu-id="2ef91-990">自訂提供者 (已安裝或已建立)</span><span class="sxs-lookup"><span data-stu-id="2ef91-990">Custom providers (installed or created)</span></span>
* <span data-ttu-id="2ef91-991">目錄檔案</span><span class="sxs-lookup"><span data-stu-id="2ef91-991">Directory files</span></span>
* <span data-ttu-id="2ef91-992">環境變數</span><span class="sxs-lookup"><span data-stu-id="2ef91-992">Environment variables</span></span>
* <span data-ttu-id="2ef91-993">記憶體內部 .NET 物件</span><span class="sxs-lookup"><span data-stu-id="2ef91-993">In-memory .NET objects</span></span>
* <span data-ttu-id="2ef91-994">設定檔</span><span class="sxs-lookup"><span data-stu-id="2ef91-994">Settings files</span></span>

<span data-ttu-id="2ef91-995">一般組態提供者案例 ([microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) 的組態套件包含在 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)中。</span><span class="sxs-lookup"><span data-stu-id="2ef91-995">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="2ef91-996">下列程式碼範例與範例應用程式中的程式碼範例使用 <xref:Microsoft.Extensions.Configuration> 命名空間：</span><span class="sxs-lookup"><span data-stu-id="2ef91-996">Code examples that follow and in the sample app use the <xref:Microsoft.Extensions.Configuration> namespace:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="2ef91-997">*選項模式*是此主題中所述之設定概念的延伸。</span><span class="sxs-lookup"><span data-stu-id="2ef91-997">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="2ef91-998">選項使用類別來代表一組相關的設定。</span><span class="sxs-lookup"><span data-stu-id="2ef91-998">Options use classes to represent groups of related settings.</span></span> <span data-ttu-id="2ef91-999">如需詳細資訊，請參閱<xref:fundamentals/configuration/options>。</span><span class="sxs-lookup"><span data-stu-id="2ef91-999">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="2ef91-1000">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="2ef91-1000">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="2ef91-1001">主機與應用程式組態的比較</span><span class="sxs-lookup"><span data-stu-id="2ef91-1001">Host versus app configuration</span></span>

<span data-ttu-id="2ef91-1002">設定及啟動應用程式之前，會先設定及啟動「主機」\*\*。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1002">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="2ef91-1003">主機負責應用程式啟動和存留期管理。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1003">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="2ef91-1004">應用程式與主機都是使用此主題中所述的設定提供者來設定的。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1004">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="2ef91-1005">主機組態機碼/值組也會包含在應用程式的組態中。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1005">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="2ef91-1006">如需有關當建置主機時如何使用設定提供者的詳細資訊，以及設定來源如何影響主機設定的詳細資訊，請參閱 <xref:fundamentals/index#host>。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1006">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

## <a name="other-configuration"></a><span data-ttu-id="2ef91-1007">其他設定</span><span class="sxs-lookup"><span data-stu-id="2ef91-1007">Other configuration</span></span>

<span data-ttu-id="2ef91-1008">本主題僅適用于*應用程式*設定。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1008">This topic only pertains to *app configuration*.</span></span> <span data-ttu-id="2ef91-1009">執行和裝載 ASP.NET Core 應用程式的其他層面，是使用本主題未涵蓋的設定檔來設定：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1009">Other aspects of running and hosting ASP.NET Core apps are configured using configuration files not covered in this topic:</span></span>

* <span data-ttu-id="2ef91-1010">*啟動 json* /*launchsettings.json*是開發環境的工具設定檔，如下所述：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1010">*launch.json*/*launchSettings.json* are tooling configuration files for the Development environment, described:</span></span>
  * <span data-ttu-id="2ef91-1011">在中 <xref:fundamentals/environments#development> 。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1011">In <xref:fundamentals/environments#development>.</span></span>
  * <span data-ttu-id="2ef91-1012">在檔集內，用來為開發案例設定 ASP.NET Core 應用程式的檔案。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1012">Across the documentation set where the files are used to configure ASP.NET Core apps for Development scenarios.</span></span>
* <span data-ttu-id="2ef91-1013">*web.config*是伺服器設定檔，如下列主題所述：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1013">*web.config* is a server configuration file, described in the following topics:</span></span>
  * <xref:host-and-deploy/iis/index>
  * <xref:host-and-deploy/aspnet-core-module>

<span data-ttu-id="2ef91-1014">如需從舊版 ASP.NET 遷移應用程式設定的詳細資訊，請參閱 <xref:migration/proper-to-2x/index#store-configurations> 。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1014">For more information on migrating app configuration from earlier versions of ASP.NET, see <xref:migration/proper-to-2x/index#store-configurations>.</span></span>

## <a name="default-configuration"></a><span data-ttu-id="2ef91-1015">預設組態</span><span class="sxs-lookup"><span data-stu-id="2ef91-1015">Default configuration</span></span>

<span data-ttu-id="2ef91-1016">以 ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) 範本為基礎的 Web 應用程式，會在建置主機時呼叫 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1016">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="2ef91-1017">`CreateDefaultBuilder` 會以下列順序提供應用程式的預設組態：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1017">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="2ef91-1018">下列項目適用於使用 [Web 主機](xref:fundamentals/host/web-host)的應用程式。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1018">The following applies to apps using the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="2ef91-1019">如需使用[一般主機](xref:fundamentals/host/generic-host)時預設組態的詳細資料，請參閱[本主題的最新版本](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1019">For details on the default configuration when using the [Generic Host](xref:fundamentals/host/generic-host), see the [latest version of this topic](xref:fundamentals/configuration/index).</span></span>

* <span data-ttu-id="2ef91-1020">主機組態的提供來源：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1020">Host configuration is provided from:</span></span>
  * <span data-ttu-id="2ef91-1021">使用[環境變數組態提供者](#environment-variables-configuration-provider)且以 `ASPNETCORE_` 為前置詞 (例如 `ASPNETCORE_ENVIRONMENT`) 的環境變數。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1021">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="2ef91-1022">載入設定機碼值組時，會移除前置詞 (`ASPNETCORE_`)。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1022">The prefix (`ASPNETCORE_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="2ef91-1023">使用[命令列組態提供者](#command-line-configuration-provider)的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1023">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="2ef91-1024">應用程式設定的提供來源：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1024">App configuration is provided from:</span></span>
  * <span data-ttu-id="2ef91-1025">使用[檔案組態提供者](#file-configuration-provider)的 *appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1025">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="2ef91-1026">使用[檔案組態提供者](#file-configuration-provider)的 *appsettings.{Environment}.json*。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1026">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="2ef91-1027">應用程式在使用項目組件之 `Development` 環境中執行時的[秘密管理員](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1027">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="2ef91-1028">使用[環境變數組態提供者](#environment-variables-configuration-provider)的環境變數。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1028">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="2ef91-1029">使用[命令列組態提供者](#command-line-configuration-provider)的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1029">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

## <a name="security"></a><span data-ttu-id="2ef91-1030">安全性</span><span class="sxs-lookup"><span data-stu-id="2ef91-1030">Security</span></span>

<span data-ttu-id="2ef91-1031">採用下列做法來保護敏感性組態資料：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1031">Adopt the following practices to secure sensitive configuration data:</span></span>

* <span data-ttu-id="2ef91-1032">永遠不要將密碼或其他敏感性資料儲存在設定提供者程式碼或純文字設定檔中。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1032">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="2ef91-1033">不要在開發或測試環境中使用生產環境祕密。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1033">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="2ef91-1034">請在專案外部指定祕密，以防止其意外認可至開放原始碼存放庫。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1034">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="2ef91-1035">如需詳細資訊，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1035">For more information, see the following topics:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="2ef91-1036"><xref:security/app-secrets>&ndash;包含使用環境變數來儲存敏感性資料的建議。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1036"><xref:security/app-secrets> &ndash; Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="2ef91-1037">「祕密管理員」使用「檔案設定提供者」以 JSON 檔案在本機系統上存放使用者祕密。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1037">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="2ef91-1038">此主題稍後將說明「檔案設定提供者」。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1038">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="2ef91-1039">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) 可安全地儲存 ASP.NET Core 應用程式的應用程式祕密。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1039">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="2ef91-1040">如需詳細資訊，請參閱<xref:security/key-vault-configuration>。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1040">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="2ef91-1041">階層式設定資料</span><span class="sxs-lookup"><span data-stu-id="2ef91-1041">Hierarchical configuration data</span></span>

<span data-ttu-id="2ef91-1042">設定 API 可在設定機碼中使用分隔符號來壓平合併階層式資料，以管理階層式設定資料。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1042">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="2ef91-1043">在下列 JSON 檔案中，兩個區段的結構式階層中有四個機碼存在：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1043">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  }
}
```

<span data-ttu-id="2ef91-1044">當該檔案被讀入設定之後，會建立唯一機碼來維護設定來源的原始階層式資料結構。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1044">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="2ef91-1045">系統會使用冒號 (`:`) 將區段與機碼壓平合併以維護原始結構：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1045">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="2ef91-1046">section0:key0</span><span class="sxs-lookup"><span data-stu-id="2ef91-1046">section0:key0</span></span>
* <span data-ttu-id="2ef91-1047">section0:key1</span><span class="sxs-lookup"><span data-stu-id="2ef91-1047">section0:key1</span></span>
* <span data-ttu-id="2ef91-1048">section1:key0</span><span class="sxs-lookup"><span data-stu-id="2ef91-1048">section1:key0</span></span>
* <span data-ttu-id="2ef91-1049">section1:key1</span><span class="sxs-lookup"><span data-stu-id="2ef91-1049">section1:key1</span></span>

<span data-ttu-id="2ef91-1050"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> 與 <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> 方法可用來在設定資料中隔離區段與區段的子系。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1050"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="2ef91-1051">[GetSection,、 GetChildren 與 Exists](#getsection-getchildren-and-exists) 中說明這些方法。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1051">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="2ef91-1052">慣例</span><span class="sxs-lookup"><span data-stu-id="2ef91-1052">Conventions</span></span>

### <a name="sources-and-providers"></a><span data-ttu-id="2ef91-1053">來源和提供者</span><span class="sxs-lookup"><span data-stu-id="2ef91-1053">Sources and providers</span></span>

<span data-ttu-id="2ef91-1054">在應用程式啟動時，會依照設定來源的設定提供者的指定順序讀入設定來源。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1054">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="2ef91-1055">實作變更偵測的組態提供者能夠在基礎設定變更時重新載入組態。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1055">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="2ef91-1056">例如，檔案組態提供者 (將於本主題稍後討論) 和 [Azure Key Vault 組態提供者](xref:security/key-vault-configuration)均會實作變更偵測。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1056">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="2ef91-1057">您可以在應用程式的[相依性插入 (DI)](xref:fundamentals/dependency-injection) 容器中找到 <xref:Microsoft.Extensions.Configuration.IConfiguration>。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1057"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="2ef91-1058"><xref:Microsoft.Extensions.Configuration.IConfiguration>可以插入 Razor 頁面 <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> 或 MVC 中 <xref:Microsoft.AspNetCore.Mvc.Controller> ，以取得類別的設定。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1058"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> or MVC <xref:Microsoft.AspNetCore.Mvc.Controller> to obtain configuration for the class.</span></span>

<span data-ttu-id="2ef91-1059">在下列範例中， `_config` 欄位是用來存取設定值：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1059">In the following examples, the `_config` field is used to access configuration values:</span></span>

```csharp
public class IndexModel : PageModel
{
    private readonly IConfiguration _config;

    public IndexModel(IConfiguration config)
    {
        _config = config;
    }
}
```

```csharp
public class HomeController : Controller
{
    private readonly IConfiguration _config;

    public HomeController(IConfiguration config)
    {
        _config = config;
    }
}
```

<span data-ttu-id="2ef91-1060">設定提供者無法使用 DI，因為當它們由主機設定時，它無法使用。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1060">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

### <a name="keys"></a><span data-ttu-id="2ef91-1061">索引鍵</span><span class="sxs-lookup"><span data-stu-id="2ef91-1061">Keys</span></span>

<span data-ttu-id="2ef91-1062">設定機碼會採用下列慣例：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1062">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="2ef91-1063">機碼區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1063">Keys are case-insensitive.</span></span> <span data-ttu-id="2ef91-1064">例如，`ConnectionString` 與 `connectionstring` 會被視為相等的機碼。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1064">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="2ef91-1065">若相同機碼的值是由相同或不同設定提供者設定，在機碼上設定的最後一個值是所使用的值。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1065">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="2ef91-1066">階層式機碼</span><span class="sxs-lookup"><span data-stu-id="2ef91-1066">Hierarchical keys</span></span>
  * <span data-ttu-id="2ef91-1067">在設定 API 內，冒號分隔字元 (`:`) 可在所有平台上運作。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1067">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="2ef91-1068">在環境變數中，冒號分隔字元可能無法在所有平台上運作。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1068">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="2ef91-1069">所有平台都支援雙底線 (`__`)，且會自動轉換為冒號。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1069">A double underscore (`__`) is supported by all platforms and is automatically converted into a colon.</span></span>
  * <span data-ttu-id="2ef91-1070">在 Azure Key Vault 中，階層式機碼使用 `--` (兩個破折號) 來做為分隔符號。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1070">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="2ef91-1071">撰寫程式碼，以在將密碼載入應用程式的設定時，以冒號取代破折號。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1071">Write code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="2ef91-1072"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> 支援在設定機碼中使用陣列索引將陣列繫結到物件。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1072">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="2ef91-1073">[將陣列繫結到類別](#bind-an-array-to-a-class)一節說明陣列繫結。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1073">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

### <a name="values"></a><span data-ttu-id="2ef91-1074">值</span><span class="sxs-lookup"><span data-stu-id="2ef91-1074">Values</span></span>

<span data-ttu-id="2ef91-1075">設定值會採用下列慣例：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1075">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="2ef91-1076">值是字串。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1076">Values are strings.</span></span>
* <span data-ttu-id="2ef91-1077">Null 值無法存放在設定中或繫結到物件。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1077">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="2ef91-1078">提供者</span><span class="sxs-lookup"><span data-stu-id="2ef91-1078">Providers</span></span>

<span data-ttu-id="2ef91-1079">下表顯示可供 ASP.NET Core 應用程式使用的設定提供者。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1079">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="2ef91-1080">提供者</span><span class="sxs-lookup"><span data-stu-id="2ef91-1080">Provider</span></span> | <span data-ttu-id="2ef91-1081">從&hellip;提供設定</span><span class="sxs-lookup"><span data-stu-id="2ef91-1081">Provides configuration from&hellip;</span></span> |
| ---
<span data-ttu-id="2ef91-1082">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1082">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1083">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1083">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1084">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1084">'Identity'</span></span>
- <span data-ttu-id="2ef91-1085">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1085">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1086">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1086">'Razor'</span></span>
- <span data-ttu-id="2ef91-1087">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1087">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1088">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1088">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1089">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1089">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1090">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1090">'Identity'</span></span>
- <span data-ttu-id="2ef91-1091">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1091">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1092">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1092">'Razor'</span></span>
- <span data-ttu-id="2ef91-1093">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1093">'SignalR' uid:</span></span> 

<span data-ttu-id="2ef91-1094">---- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1094">---- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1095">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1095">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1096">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1096">'Identity'</span></span>
- <span data-ttu-id="2ef91-1097">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1097">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1098">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1098">'Razor'</span></span>
- <span data-ttu-id="2ef91-1099">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1099">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1100">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1100">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1101">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1101">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1102">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1102">'Identity'</span></span>
- <span data-ttu-id="2ef91-1103">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1103">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1104">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1104">'Razor'</span></span>
- <span data-ttu-id="2ef91-1105">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1105">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1106">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1106">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1107">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1107">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1108">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1108">'Identity'</span></span>
- <span data-ttu-id="2ef91-1109">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1109">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1110">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1110">'Razor'</span></span>
- <span data-ttu-id="2ef91-1111">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1111">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1112">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1112">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1113">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1113">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1114">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1114">'Identity'</span></span>
- <span data-ttu-id="2ef91-1115">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1115">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1116">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1116">'Razor'</span></span>
- <span data-ttu-id="2ef91-1117">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1117">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1118">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1118">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1119">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1119">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1120">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1120">'Identity'</span></span>
- <span data-ttu-id="2ef91-1121">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1121">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1122">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1122">'Razor'</span></span>
- <span data-ttu-id="2ef91-1123">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1123">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1124">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1124">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1125">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1125">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1126">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1126">'Identity'</span></span>
- <span data-ttu-id="2ef91-1127">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1127">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1128">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1128">'Razor'</span></span>
- <span data-ttu-id="2ef91-1129">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1129">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1130">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1130">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1131">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1131">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1132">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1132">'Identity'</span></span>
- <span data-ttu-id="2ef91-1133">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1133">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1134">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1134">'Razor'</span></span>
- <span data-ttu-id="2ef91-1135">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1135">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1136">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1136">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1137">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1137">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1138">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1138">'Identity'</span></span>
- <span data-ttu-id="2ef91-1139">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1139">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1140">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1140">'Razor'</span></span>
- <span data-ttu-id="2ef91-1141">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1141">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1142">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1142">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1143">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1143">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1144">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1144">'Identity'</span></span>
- <span data-ttu-id="2ef91-1145">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1145">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1146">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1146">'Razor'</span></span>
- <span data-ttu-id="2ef91-1147">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1147">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1148">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1148">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1149">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1149">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1150">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1150">'Identity'</span></span>
- <span data-ttu-id="2ef91-1151">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1151">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1152">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1152">'Razor'</span></span>
- <span data-ttu-id="2ef91-1153">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1153">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1154">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1154">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1155">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1155">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1156">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1156">'Identity'</span></span>
- <span data-ttu-id="2ef91-1157">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1157">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1158">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1158">'Razor'</span></span>
- <span data-ttu-id="2ef91-1159">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1159">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1160">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1160">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1161">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1161">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1162">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1162">'Identity'</span></span>
- <span data-ttu-id="2ef91-1163">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1163">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1164">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1164">'Razor'</span></span>
- <span data-ttu-id="2ef91-1165">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1165">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1166">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1166">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1167">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1167">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1168">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1168">'Identity'</span></span>
- <span data-ttu-id="2ef91-1169">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1169">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1170">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1170">'Razor'</span></span>
- <span data-ttu-id="2ef91-1171">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1171">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1172">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1172">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1173">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1173">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1174">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1174">'Identity'</span></span>
- <span data-ttu-id="2ef91-1175">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1175">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1176">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1176">'Razor'</span></span>
- <span data-ttu-id="2ef91-1177">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1177">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1178">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1178">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1179">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1179">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1180">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1180">'Identity'</span></span>
- <span data-ttu-id="2ef91-1181">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1181">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1182">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1182">'Razor'</span></span>
- <span data-ttu-id="2ef91-1183">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1183">'SignalR' uid:</span></span> 

<span data-ttu-id="2ef91-1184">------------------ || [Azure Key Vault 設定提供者](xref:security/key-vault-configuration)（*安全性*主題） |Azure Key Vault || [Azure 應用程式組態提供者](/azure/azure-app-configuration/quickstart-aspnet-core-app)（Azure 檔） |Azure 應用程式組態 ||[命令列設定提供者](#command-line-configuration-provider)|命令列參數 ||[自訂設定提供者](#custom-configuration-provider)|自訂來源 ||[環境變數設定提供者](#environment-variables-configuration-provider)|環境變數 |檔案設定 | [提供者](#file-configuration-provider)|檔案（INI、JSON、XML） ||[每個](#key-per-file-configuration-provider)檔案的索引鍵設定提供者 |目錄檔案 ||[記憶體設定提供者](#memory-configuration-provider)|記憶體內部集合 ||[使用者秘密（秘密管理員）](xref:security/app-secrets) （*安全性*主題） |使用者設定檔目錄中的檔案 |</span><span class="sxs-lookup"><span data-stu-id="2ef91-1184">------------------ | | [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics) | Azure Key Vault | | [Azure App Configuration Provider](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure documentation) | Azure App Configuration | | [Command-line Configuration Provider](#command-line-configuration-provider) | Command-line parameters | | [Custom configuration provider](#custom-configuration-provider) | Custom source | | [Environment Variables Configuration Provider](#environment-variables-configuration-provider) | Environment variables | | [File Configuration Provider](#file-configuration-provider) | Files (INI, JSON, XML) | | [Key-per-file Configuration Provider](#key-per-file-configuration-provider) | Directory files | | [Memory Configuration Provider](#memory-configuration-provider) | In-memory collections | | [User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics) | File in the user profile directory |</span></span>

<span data-ttu-id="2ef91-1185">在啟動時，會依照設定來源的設定提供者的指定順序讀入設定來源。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1185">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="2ef91-1186">本主題所描述的設定提供者會依字母順序描述，而不是程式碼排列它們的順序。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1186">The configuration providers described in this topic are described in alphabetical order, not in the order that the code arranges them.</span></span> <span data-ttu-id="2ef91-1187">請在程式碼中訂購設定提供者，以符合應用程式所需之基礎設定來源的優先順序。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1187">Order configuration providers in code to suit the priorities for the underlying configuration sources that the app requires.</span></span>

<span data-ttu-id="2ef91-1188">典型的設定提供者順序是：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1188">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="2ef91-1189">Files （*appsettings. json*， *appsettings. {環境}. json*，其中 `{Environment}` 是應用程式的目前裝載環境）</span><span class="sxs-lookup"><span data-stu-id="2ef91-1189">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="2ef91-1190">Azure 金鑰保存庫</span><span class="sxs-lookup"><span data-stu-id="2ef91-1190">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="2ef91-1191">[使用者祕密 (祕密管理員)](xref:security/app-secrets) (僅限開發環境)</span><span class="sxs-lookup"><span data-stu-id="2ef91-1191">[User secrets (Secret Manager)](xref:security/app-secrets) (Development environment only)</span></span>
1. <span data-ttu-id="2ef91-1192">環境變數</span><span class="sxs-lookup"><span data-stu-id="2ef91-1192">Environment variables</span></span>
1. <span data-ttu-id="2ef91-1193">命令列引數</span><span class="sxs-lookup"><span data-stu-id="2ef91-1193">Command-line arguments</span></span>

<span data-ttu-id="2ef91-1194">將命令列組態提供者放在提供者序列結尾是常見做法，因為這樣可以讓命令列引數覆寫由其他提供者所設定的組態。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1194">A common practice is to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="2ef91-1195">當使用初始化新的主機產生器時，會使用上述的提供者序列 `CreateDefaultBuilder` 。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1195">The preceding sequence of providers is used when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="2ef91-1196">如需詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1196">For more information, see the [Default configuration](#default-configuration) section.</span></span>

## <a name="configure-the-host-builder-with-useconfiguration"></a><span data-ttu-id="2ef91-1197">使用 UseConfiguration 設定主機建立器</span><span class="sxs-lookup"><span data-stu-id="2ef91-1197">Configure the host builder with UseConfiguration</span></span>

<span data-ttu-id="2ef91-1198">若要設定主機建立器，請使用該組態在主機建立器上呼叫 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1198">To configure the host builder, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> on the host builder with the configuration.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args)
{
    var dict = new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };

    var config = new ConfigurationBuilder()
        .AddInMemoryCollection(dict)
        .Build();

    return WebHost.CreateDefaultBuilder(args)
        .UseConfiguration(config)
        .UseStartup<Startup>();
}
```

## <a name="configureappconfiguration"></a><span data-ttu-id="2ef91-1199">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="2ef91-1199">ConfigureAppConfiguration</span></span>

<span data-ttu-id="2ef91-1200">建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定提供者，以及由 `CreateDefaultBuilder` 自動新增的設定提供者：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1200">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration providers in addition to those added automatically by `CreateDefaultBuilder`:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

### <a name="override-previous-configuration-with-command-line-arguments"></a><span data-ttu-id="2ef91-1201">使用命令列引數覆寫先前的組態</span><span class="sxs-lookup"><span data-stu-id="2ef91-1201">Override previous configuration with command-line arguments</span></span>

<span data-ttu-id="2ef91-1202">若要提供可使用命令列引數覆寫的應用程式組態，請最後呼叫 `AddCommandLine`：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1202">To provide app configuration that can be overridden with command-line arguments, call `AddCommandLine` last:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="remove-providers-added-by-createdefaultbuilder"></a><span data-ttu-id="2ef91-1203">移除 CreateDefaultBuilder 新增的提供者</span><span class="sxs-lookup"><span data-stu-id="2ef91-1203">Remove providers added by CreateDefaultBuilder</span></span>

<span data-ttu-id="2ef91-1204">若要移除新增的提供者 `CreateDefaultBuilder` ，請先在[IConfigurationBuilder](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources)上呼叫[Clear](/dotnet/api/system.collections.generic.icollection-1.clear) ：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1204">To remove the providers added by `CreateDefaultBuilder`, call [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) on the [IConfigurationBuilder.Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) first:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.Sources.Clear();
    // Add providers here
})
```

### <a name="consume-configuration-during-app-startup"></a><span data-ttu-id="2ef91-1205">在應用程式啟動期間使用組態</span><span class="sxs-lookup"><span data-stu-id="2ef91-1205">Consume configuration during app startup</span></span>

<span data-ttu-id="2ef91-1206">應用程式啟動期間，可以使用 `ConfigureAppConfiguration` 中為應用程式提供的組態，包括 `Startup.ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1206">Configuration supplied to the app in `ConfigureAppConfiguration` is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="2ef91-1207">如需詳細資訊，請參閱[在啟動期間存取組態](#access-configuration-during-startup)一節。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1207">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="2ef91-1208">命令列設定提供者</span><span class="sxs-lookup"><span data-stu-id="2ef91-1208">Command-line Configuration Provider</span></span>

<span data-ttu-id="2ef91-1209"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> 會在執行階段從命令列引數機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1209">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="2ef91-1210">為了啟用命令列設定，<xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 延伸模組方法會在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1210">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="2ef91-1211">呼叫 `CreateDefaultBuilder(string [])` 時，會自動呼叫 `AddCommandLine`。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1211">`AddCommandLine` is automatically called when `CreateDefaultBuilder(string [])` is called.</span></span> <span data-ttu-id="2ef91-1212">如需詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1212">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="2ef91-1213">`CreateDefaultBuilder` 也會載入：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1213">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="2ef91-1214">從 *appsettings.json* 與 *appsettings.{Environment}.json* 檔案載入其他選擇性組態。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1214">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="2ef91-1215">開發環境中的[使用者祕密 (祕密管理員)](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1215">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="2ef91-1216">環境變數。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1216">Environment variables.</span></span>

<span data-ttu-id="2ef91-1217">`CreateDefaultBuilder` 會最後才新增命令列設定提供者。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1217">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="2ef91-1218">在執行階段傳遞的命令列引數會覆寫由其它提供者所設定的設定。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1218">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="2ef91-1219">`CreateDefaultBuilder` 會在建構主機時執行作業。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1219">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="2ef91-1220">因此，由`CreateDefaultBuilder` 啟用的命令列設定可以影響主機的設定方式。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1220">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="2ef91-1221">針對以 ASP.NET Core 範本為基礎的應用程式，`AddCommandLine` 已由 `CreateDefaultBuilder` 呼叫。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1221">For apps based on the ASP.NET Core templates, `AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="2ef91-1222">若要新增其他組態提供者並維持能夠使用命令列引數覆寫這些提供者組態的能力，請在 `ConfigureAppConfiguration` 中呼叫應用程式的其他提供者，且最後呼叫 `AddCommandLine`。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1222">To add additional configuration providers and maintain the ability to override configuration from those providers with command-line arguments, call the app's additional providers in `ConfigureAppConfiguration` and call `AddCommandLine` last.</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

<span data-ttu-id="2ef91-1223">**範例**</span><span class="sxs-lookup"><span data-stu-id="2ef91-1223">**Example**</span></span>

<span data-ttu-id="2ef91-1224">範例應用程式利用靜態方便方法 `CreateDefaultBuilder` 的優勢來建置主機，這包括對 <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 的呼叫。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1224">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="2ef91-1225">從專案目錄中開啟命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1225">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="2ef91-1226">提供命令列引數給 `dotnet run` 命令 `dotnet run CommandLineKey=CommandLineValue`。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1226">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="2ef91-1227">在應用程式執行之後，開啟瀏覽器以瀏覽位於 `http://localhost:5000` 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1227">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="2ef91-1228">觀察輸出是否包含提供給 `dotnet run` 之設定命令列引數的機碼值組。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1228">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="2ef91-1229">引數</span><span class="sxs-lookup"><span data-stu-id="2ef91-1229">Arguments</span></span>

<span data-ttu-id="2ef91-1230">當值後面接著空格時，值必須接著等號 (`=`)，或機碼必須有前置詞 (`--` 或 `/`)。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1230">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="2ef91-1231">如果使用等號 (例如 `CommandLineKey=`)，則不需要此值。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1231">The value isn't required if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="2ef91-1232">索引鍵前置字元</span><span class="sxs-lookup"><span data-stu-id="2ef91-1232">Key prefix</span></span>               | <span data-ttu-id="2ef91-1233">範例</span><span class="sxs-lookup"><span data-stu-id="2ef91-1233">Example</span></span>                                                |
| ---
<span data-ttu-id="2ef91-1234">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1234">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1235">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1235">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1236">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1236">'Identity'</span></span>
- <span data-ttu-id="2ef91-1237">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1237">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1238">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1238">'Razor'</span></span>
- <span data-ttu-id="2ef91-1239">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1239">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1240">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1240">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1241">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1241">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1242">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1242">'Identity'</span></span>
- <span data-ttu-id="2ef91-1243">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1243">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1244">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1244">'Razor'</span></span>
- <span data-ttu-id="2ef91-1245">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1245">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1246">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1246">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1247">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1247">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1248">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1248">'Identity'</span></span>
- <span data-ttu-id="2ef91-1249">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1249">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1250">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1250">'Razor'</span></span>
- <span data-ttu-id="2ef91-1251">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1251">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1252">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1252">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1253">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1253">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1254">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1254">'Identity'</span></span>
- <span data-ttu-id="2ef91-1255">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1255">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1256">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1256">'Razor'</span></span>
- <span data-ttu-id="2ef91-1257">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1257">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1258">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1258">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1259">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1259">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1260">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1260">'Identity'</span></span>
- <span data-ttu-id="2ef91-1261">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1261">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1262">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1262">'Razor'</span></span>
- <span data-ttu-id="2ef91-1263">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1263">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1264">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1264">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1265">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1265">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1266">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1266">'Identity'</span></span>
- <span data-ttu-id="2ef91-1267">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1267">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1268">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1268">'Razor'</span></span>
- <span data-ttu-id="2ef91-1269">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1269">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1270">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1270">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1271">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1271">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1272">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1272">'Identity'</span></span>
- <span data-ttu-id="2ef91-1273">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1273">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1274">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1274">'Razor'</span></span>
- <span data-ttu-id="2ef91-1275">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1275">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1276">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1276">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1277">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1277">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1278">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1278">'Identity'</span></span>
- <span data-ttu-id="2ef91-1279">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1279">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1280">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1280">'Razor'</span></span>
- <span data-ttu-id="2ef91-1281">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1281">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1282">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1282">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1283">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1283">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1284">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1284">'Identity'</span></span>
- <span data-ttu-id="2ef91-1285">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1285">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1286">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1286">'Razor'</span></span>
- <span data-ttu-id="2ef91-1287">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1287">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1288">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1288">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1289">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1289">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1290">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1290">'Identity'</span></span>
- <span data-ttu-id="2ef91-1291">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1291">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1292">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1292">'Razor'</span></span>
- <span data-ttu-id="2ef91-1293">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1293">'SignalR' uid:</span></span> 

<span data-ttu-id="2ef91-1294">------------ |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1294">------------ | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1295">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1295">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1296">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1296">'Identity'</span></span>
- <span data-ttu-id="2ef91-1297">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1297">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1298">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1298">'Razor'</span></span>
- <span data-ttu-id="2ef91-1299">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1299">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1300">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1300">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1301">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1301">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1302">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1302">'Identity'</span></span>
- <span data-ttu-id="2ef91-1303">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1303">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1304">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1304">'Razor'</span></span>
- <span data-ttu-id="2ef91-1305">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1305">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1306">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1306">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1307">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1307">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1308">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1308">'Identity'</span></span>
- <span data-ttu-id="2ef91-1309">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1309">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1310">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1310">'Razor'</span></span>
- <span data-ttu-id="2ef91-1311">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1311">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1312">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1312">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1313">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1313">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1314">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1314">'Identity'</span></span>
- <span data-ttu-id="2ef91-1315">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1315">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1316">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1316">'Razor'</span></span>
- <span data-ttu-id="2ef91-1317">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1317">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1318">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1318">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1319">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1319">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1320">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1320">'Identity'</span></span>
- <span data-ttu-id="2ef91-1321">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1321">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1322">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1322">'Razor'</span></span>
- <span data-ttu-id="2ef91-1323">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1323">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1324">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1324">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1325">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1325">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1326">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1326">'Identity'</span></span>
- <span data-ttu-id="2ef91-1327">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1327">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1328">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1328">'Razor'</span></span>
- <span data-ttu-id="2ef91-1329">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1329">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1330">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1330">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1331">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1331">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1332">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1332">'Identity'</span></span>
- <span data-ttu-id="2ef91-1333">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1333">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1334">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1334">'Razor'</span></span>
- <span data-ttu-id="2ef91-1335">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1335">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1336">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1336">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1337">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1337">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1338">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1338">'Identity'</span></span>
- <span data-ttu-id="2ef91-1339">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1339">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1340">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1340">'Razor'</span></span>
- <span data-ttu-id="2ef91-1341">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1341">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1342">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1342">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1343">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1343">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1344">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1344">'Identity'</span></span>
- <span data-ttu-id="2ef91-1345">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1345">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1346">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1346">'Razor'</span></span>
- <span data-ttu-id="2ef91-1347">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1347">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1348">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1348">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1349">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1349">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1350">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1350">'Identity'</span></span>
- <span data-ttu-id="2ef91-1351">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1351">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1352">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1352">'Razor'</span></span>
- <span data-ttu-id="2ef91-1353">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1353">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1354">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1354">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1355">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1355">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1356">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1356">'Identity'</span></span>
- <span data-ttu-id="2ef91-1357">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1357">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1358">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1358">'Razor'</span></span>
- <span data-ttu-id="2ef91-1359">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1359">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1360">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1360">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1361">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1361">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1362">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1362">'Identity'</span></span>
- <span data-ttu-id="2ef91-1363">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1363">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1364">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1364">'Razor'</span></span>
- <span data-ttu-id="2ef91-1365">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1365">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1366">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1366">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1367">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1367">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1368">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1368">'Identity'</span></span>
- <span data-ttu-id="2ef91-1369">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1369">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1370">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1370">'Razor'</span></span>
- <span data-ttu-id="2ef91-1371">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1371">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1372">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1372">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1373">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1373">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1374">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1374">'Identity'</span></span>
- <span data-ttu-id="2ef91-1375">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1375">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1376">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1376">'Razor'</span></span>
- <span data-ttu-id="2ef91-1377">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1377">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1378">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1378">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1379">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1379">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1380">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1380">'Identity'</span></span>
- <span data-ttu-id="2ef91-1381">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1381">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1382">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1382">'Razor'</span></span>
- <span data-ttu-id="2ef91-1383">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1383">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1384">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1384">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1385">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1385">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1386">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1386">'Identity'</span></span>
- <span data-ttu-id="2ef91-1387">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1387">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1388">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1388">'Razor'</span></span>
- <span data-ttu-id="2ef91-1389">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1389">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1390">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1390">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1391">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1391">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1392">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1392">'Identity'</span></span>
- <span data-ttu-id="2ef91-1393">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1393">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1394">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1394">'Razor'</span></span>
- <span data-ttu-id="2ef91-1395">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1395">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1396">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1396">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1397">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1397">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1398">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1398">'Identity'</span></span>
- <span data-ttu-id="2ef91-1399">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1399">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1400">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1400">'Razor'</span></span>
- <span data-ttu-id="2ef91-1401">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1401">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1402">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1402">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1403">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1403">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1404">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1404">'Identity'</span></span>
- <span data-ttu-id="2ef91-1405">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1405">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1406">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1406">'Razor'</span></span>
- <span data-ttu-id="2ef91-1407">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1407">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1408">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1408">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1409">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1409">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1410">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1410">'Identity'</span></span>
- <span data-ttu-id="2ef91-1411">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1411">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1412">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1412">'Razor'</span></span>
- <span data-ttu-id="2ef91-1413">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1413">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1414">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1414">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1415">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1415">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1416">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1416">'Identity'</span></span>
- <span data-ttu-id="2ef91-1417">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1417">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1418">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1418">'Razor'</span></span>
- <span data-ttu-id="2ef91-1419">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1419">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1420">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1420">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1421">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1421">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1422">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1422">'Identity'</span></span>
- <span data-ttu-id="2ef91-1423">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1423">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1424">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1424">'Razor'</span></span>
- <span data-ttu-id="2ef91-1425">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1425">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1426">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1426">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1427">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1427">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1428">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1428">'Identity'</span></span>
- <span data-ttu-id="2ef91-1429">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1429">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1430">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1430">'Razor'</span></span>
- <span data-ttu-id="2ef91-1431">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1431">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1432">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1432">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1433">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1433">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1434">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1434">'Identity'</span></span>
- <span data-ttu-id="2ef91-1435">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1435">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1436">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1436">'Razor'</span></span>
- <span data-ttu-id="2ef91-1437">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1437">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1438">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1438">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1439">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1439">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1440">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1440">'Identity'</span></span>
- <span data-ttu-id="2ef91-1441">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1441">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1442">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1442">'Razor'</span></span>
- <span data-ttu-id="2ef91-1443">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1443">'SignalR' uid:</span></span> 

<span data-ttu-id="2ef91-1444">--------------------------- | |沒有前置詞 |`CommandLineKey1=value1`                               |
|雙虛線（ `--` ） | `--CommandLineKey2=value2` ， `--CommandLineKey2 value2` |
 |正斜線（ `/` ） | `/CommandLineKey3=value3` 、`/CommandLineKey3 value3`   |</span><span class="sxs-lookup"><span data-stu-id="2ef91-1444">--------------------------- | | No prefix                | `CommandLineKey1=value1`                               |
| Two dashes (`--`)        | `--CommandLineKey2=value2`, `--CommandLineKey2 value2` |
| Forward slash (`/`)      | `/CommandLineKey3=value3`, `/CommandLineKey3 value3`   |</span></span>

<span data-ttu-id="2ef91-1445">在相同的命令中，請不要混合使用等號搭配使用空格之機碼值組的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1445">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="2ef91-1446">範例命令：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1446">Example commands:</span></span>

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="2ef91-1447">切換對應</span><span class="sxs-lookup"><span data-stu-id="2ef91-1447">Switch mappings</span></span>

<span data-ttu-id="2ef91-1448">參數對應允許索引鍵名稱取代邏輯。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1448">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="2ef91-1449">當以手動方式建立設定時 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> ，請提供方法的切換取代的字典 <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1449">When manually building configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="2ef91-1450">使用切換對應字典時，會檢查字典中是否有任何索引鍵符合命令列引數所提供的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1450">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="2ef91-1451">如果在字典中找到命令列索引鍵，則會傳回字典值 (索引鍵取代) 以在應用程式的設定中設定機碼值組。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1451">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="2ef91-1452">所有前面加上單虛線 (`-`) 的命令列索引鍵都需要切換對應。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1452">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="2ef91-1453">切換對應字典索引鍵規則：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1453">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="2ef91-1454">切換必須以單虛線 (`-`) 或雙虛線 (`--`) 開頭。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1454">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="2ef91-1455">切換對應字典不能包含重複索引鍵。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1455">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="2ef91-1456">建立切換對應字典。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1456">Create a switch mappings dictionary.</span></span> <span data-ttu-id="2ef91-1457">下列範例會建立兩個切換對應：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1457">In the following example, two switch mappings are created:</span></span>

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

<span data-ttu-id="2ef91-1458">建立主機時，請使用切換對應字典呼叫 `AddCommandLine`：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1458">When the host is built, call `AddCommandLine` with the switch mappings dictionary:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

<span data-ttu-id="2ef91-1459">針對使用切換對應的應用程式，呼叫 `CreateDefaultBuilder` 不應傳遞引數。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1459">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="2ef91-1460">`CreateDefaultBuilder` 方法的 `AddCommandLine` 呼叫不包括對應的切換，且沒有任何方式可以將切換對應字典傳遞給 `CreateDefaultBuilder`。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1460">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="2ef91-1461">解決方案並非將引數傳遞給 `CreateDefaultBuilder`，而是允許 `ConfigurationBuilder` 方法的 `AddCommandLine` 方法同時處理引數與切換對應字典。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1461">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="2ef91-1462">建立切換對應字典之後，它會包含下表中所示的資料。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1462">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="2ef91-1463">Key</span><span class="sxs-lookup"><span data-stu-id="2ef91-1463">Key</span></span>       | <span data-ttu-id="2ef91-1464">值</span><span class="sxs-lookup"><span data-stu-id="2ef91-1464">Value</span></span>             |
| ---
<span data-ttu-id="2ef91-1465">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1465">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1466">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1466">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1467">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1467">'Identity'</span></span>
- <span data-ttu-id="2ef91-1468">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1468">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1469">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1469">'Razor'</span></span>
- <span data-ttu-id="2ef91-1470">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1470">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1471">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1471">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1472">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1472">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1473">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1473">'Identity'</span></span>
- <span data-ttu-id="2ef91-1474">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1474">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1475">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1475">'Razor'</span></span>
- <span data-ttu-id="2ef91-1476">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1476">'SignalR' uid:</span></span> 

<span data-ttu-id="2ef91-1477">----- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1477">----- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1478">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1478">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1479">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1479">'Identity'</span></span>
- <span data-ttu-id="2ef91-1480">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1480">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1481">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1481">'Razor'</span></span>
- <span data-ttu-id="2ef91-1482">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1482">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1483">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1483">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1484">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1484">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1485">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1485">'Identity'</span></span>
- <span data-ttu-id="2ef91-1486">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1486">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1487">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1487">'Razor'</span></span>
- <span data-ttu-id="2ef91-1488">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1488">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1489">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1489">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1490">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1490">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1491">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1491">'Identity'</span></span>
- <span data-ttu-id="2ef91-1492">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1492">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1493">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1493">'Razor'</span></span>
- <span data-ttu-id="2ef91-1494">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1494">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1495">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1495">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1496">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1496">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1497">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1497">'Identity'</span></span>
- <span data-ttu-id="2ef91-1498">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1498">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1499">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1499">'Razor'</span></span>
- <span data-ttu-id="2ef91-1500">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1500">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1501">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1501">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1502">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1502">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1503">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1503">'Identity'</span></span>
- <span data-ttu-id="2ef91-1504">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1504">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1505">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1505">'Razor'</span></span>
- <span data-ttu-id="2ef91-1506">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1506">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1507">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1507">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1508">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1508">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1509">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1509">'Identity'</span></span>
- <span data-ttu-id="2ef91-1510">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1510">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1511">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1511">'Razor'</span></span>
- <span data-ttu-id="2ef91-1512">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1512">'SignalR' uid:</span></span> 

<span data-ttu-id="2ef91-1513">--------- | | `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |</span><span class="sxs-lookup"><span data-stu-id="2ef91-1513">--------- | | `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |</span></span>

<span data-ttu-id="2ef91-1514">若啟動應用程式時使用切換對應機碼，設定會接收由字典提供之機碼上的設定值：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1514">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```dotnetcli
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="2ef91-1515">執行上述命令之後，設定包含下表中顯示的值。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1515">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="2ef91-1516">Key</span><span class="sxs-lookup"><span data-stu-id="2ef91-1516">Key</span></span>               | <span data-ttu-id="2ef91-1517">值</span><span class="sxs-lookup"><span data-stu-id="2ef91-1517">Value</span></span>    |
| ---
<span data-ttu-id="2ef91-1518">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1518">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1519">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1519">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1520">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1520">'Identity'</span></span>
- <span data-ttu-id="2ef91-1521">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1521">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1522">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1522">'Razor'</span></span>
- <span data-ttu-id="2ef91-1523">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1523">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1524">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1524">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1525">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1525">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1526">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1526">'Identity'</span></span>
- <span data-ttu-id="2ef91-1527">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1527">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1528">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1528">'Razor'</span></span>
- <span data-ttu-id="2ef91-1529">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1529">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1530">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1530">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1531">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1531">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1532">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1532">'Identity'</span></span>
- <span data-ttu-id="2ef91-1533">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1533">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1534">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1534">'Razor'</span></span>
- <span data-ttu-id="2ef91-1535">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1535">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1536">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1536">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1537">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1537">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1538">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1538">'Identity'</span></span>
- <span data-ttu-id="2ef91-1539">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1539">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1540">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1540">'Razor'</span></span>
- <span data-ttu-id="2ef91-1541">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1541">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1542">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1542">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1543">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1543">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1544">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1544">'Identity'</span></span>
- <span data-ttu-id="2ef91-1545">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1545">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1546">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1546">'Razor'</span></span>
- <span data-ttu-id="2ef91-1547">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1547">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1548">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1548">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1549">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1549">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1550">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1550">'Identity'</span></span>
- <span data-ttu-id="2ef91-1551">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1551">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1552">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1552">'Razor'</span></span>
- <span data-ttu-id="2ef91-1553">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1553">'SignalR' uid:</span></span> 

<span data-ttu-id="2ef91-1554">--------- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1554">--------- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1555">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1555">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1556">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1556">'Identity'</span></span>
- <span data-ttu-id="2ef91-1557">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1557">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1558">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1558">'Razor'</span></span>
- <span data-ttu-id="2ef91-1559">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1559">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1560">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1560">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1561">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1561">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1562">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1562">'Identity'</span></span>
- <span data-ttu-id="2ef91-1563">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1563">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1564">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1564">'Razor'</span></span>
- <span data-ttu-id="2ef91-1565">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1565">'SignalR' uid:</span></span> 

<span data-ttu-id="2ef91-1566">---- | | `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |</span><span class="sxs-lookup"><span data-stu-id="2ef91-1566">---- | | `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |</span></span>

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="2ef91-1567">環境變數設定提供者</span><span class="sxs-lookup"><span data-stu-id="2ef91-1567">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="2ef91-1568"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> 會在執行階段從環境變數機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1568">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="2ef91-1569">若要啟用環境變數設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1569">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="2ef91-1570">[Azure App Service](https://azure.microsoft.com/services/app-service/)允許在 Azure 入口網站中設定環境變數，以使用環境變數設定提供者來覆寫應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1570">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits setting environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="2ef91-1571">如需詳細資訊，請參閱 [Azure App：使用 Azure 入口網站覆寫應用程式設定](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal)。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1571">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

<span data-ttu-id="2ef91-1572">使用 [Web 主機](xref:fundamentals/host/web-host)初始化新的主機建立器並呼叫 `CreateDefaultBuilder` 時，可使用 `AddEnvironmentVariables` 為[主機組態](#host-versus-app-configuration)載入字首為 `ASPNETCORE_` 的環境變數。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1572">`AddEnvironmentVariables` is used to load environment variables prefixed with `ASPNETCORE_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Web Host](xref:fundamentals/host/web-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="2ef91-1573">如需詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1573">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="2ef91-1574">`CreateDefaultBuilder` 也會載入：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1574">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="2ef91-1575">來自無首碼之環境變數的應用程式組態 (在未提供首碼的情況下呼叫 `AddEnvironmentVariables`)。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1575">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="2ef91-1576">從 *appsettings.json* 與 *appsettings.{Environment}.json* 檔案載入其他選擇性組態。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1576">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="2ef91-1577">開發環境中的[使用者祕密 (祕密管理員)](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1577">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="2ef91-1578">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1578">Command-line arguments.</span></span>

<span data-ttu-id="2ef91-1579">從使用者祕密與 *appsettings* 檔案建立設定之後，會呼叫「環境變數設定提供者」。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1579">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="2ef91-1580">在此位置呼叫提供者可讓系統在執行階段讀取環境變數，以覆寫由使用者祕密與 *appsettings* 檔案所設定的設定。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1580">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="2ef91-1581">若要從其他環境變數提供應用程式設定，請在中呼叫應用程式的其他提供者， `ConfigureAppConfiguration` 並 `AddEnvironmentVariables` 使用前置詞呼叫：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1581">To provide app configuration from additional environment variables, call the app's additional providers in `ConfigureAppConfiguration` and call `AddEnvironmentVariables` with the prefix:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddEnvironmentVariables(prefix: "PREFIX_");
})
```

<span data-ttu-id="2ef91-1582">`AddEnvironmentVariables`最後呼叫以允許具有指定前置詞的環境變數，覆寫其他提供者的值。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1582">Call `AddEnvironmentVariables` last to allow environment variables with the given prefix to override values from other providers.</span></span>

<span data-ttu-id="2ef91-1583">**範例**</span><span class="sxs-lookup"><span data-stu-id="2ef91-1583">**Example**</span></span>

<span data-ttu-id="2ef91-1584">範例應用程式利用靜態方便方法 `CreateDefaultBuilder` 的優勢來建置主機，這包括對 `AddEnvironmentVariables` 的呼叫。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1584">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="2ef91-1585">執行範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1585">Run the sample app.</span></span> <span data-ttu-id="2ef91-1586">開啟瀏覽器以瀏覽位於 `http://localhost:5000` 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1586">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="2ef91-1587">觀察輸出是否包含環境變數 `ENVIRONMENT` 的機碼值組。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1587">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="2ef91-1588">值反映應用程式執行所在環境，在本機執行時，通常是 `Development`。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1588">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="2ef91-1589">為縮短由應用程式轉譯的環境變數清單，應用程式會篩選環境變數。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1589">To keep the list of environment variables rendered by the app short, the app filters environment variables.</span></span> <span data-ttu-id="2ef91-1590">請參閱範例應用程式的 *Pages/Index.cshtml.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1590">See the sample app's *Pages/Index.cshtml.cs* file.</span></span>

<span data-ttu-id="2ef91-1591">若要公開應用程式可用的所有環境變數，請將 `FilteredConfiguration` *Pages/Index. cshtml*中的變更為下列內容：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1591">To expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="2ef91-1592">首碼</span><span class="sxs-lookup"><span data-stu-id="2ef91-1592">Prefixes</span></span>

<span data-ttu-id="2ef91-1593">在方法中提供前置詞時，會篩選載入應用程式設定中的環境變數 `AddEnvironmentVariables` 。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1593">Environment variables loaded into the app's configuration are filtered when supplying a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="2ef91-1594">例如，若要篩選前置詞為 `CUSTOM_` 的環境變數，請提供前置詞給設定提供者：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1594">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="2ef91-1595">建立設定機碼值組時，會移除前置詞。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1595">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="2ef91-1596">建立主機建立器時，主機組態由環境變數提供。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1596">When the host builder is created, host configuration is provided by environment variables.</span></span> <span data-ttu-id="2ef91-1597">如需這些環境變數所用前置詞的詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1597">For more information on the prefix used for these environment variables, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="2ef91-1598">**連接字串前置詞**</span><span class="sxs-lookup"><span data-stu-id="2ef91-1598">**Connection string prefixes**</span></span>

<span data-ttu-id="2ef91-1599">設定 API 四個連接字串環境變數的特殊處理規則，這些這些環境變數牽涉到針對應用程式環境設定 Azure 連接字串。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1599">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="2ef91-1600">若將前置詞提供給 `AddEnvironmentVariables`具有下表顯示之前置詞的環境變數。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1600">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="2ef91-1601">連接字串前置詞</span><span class="sxs-lookup"><span data-stu-id="2ef91-1601">Connection string prefix</span></span> | <span data-ttu-id="2ef91-1602">提供者</span><span class="sxs-lookup"><span data-stu-id="2ef91-1602">Provider</span></span> |
| ---
<span data-ttu-id="2ef91-1603">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1603">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1604">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1604">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1605">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1605">'Identity'</span></span>
- <span data-ttu-id="2ef91-1606">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1606">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1607">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1607">'Razor'</span></span>
- <span data-ttu-id="2ef91-1608">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1608">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1609">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1609">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1610">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1610">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1611">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1611">'Identity'</span></span>
- <span data-ttu-id="2ef91-1612">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1612">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1613">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1613">'Razor'</span></span>
- <span data-ttu-id="2ef91-1614">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1614">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1615">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1615">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1616">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1616">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1617">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1617">'Identity'</span></span>
- <span data-ttu-id="2ef91-1618">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1618">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1619">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1619">'Razor'</span></span>
- <span data-ttu-id="2ef91-1620">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1620">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1621">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1621">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1622">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1622">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1623">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1623">'Identity'</span></span>
- <span data-ttu-id="2ef91-1624">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1624">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1625">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1625">'Razor'</span></span>
- <span data-ttu-id="2ef91-1626">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1626">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1627">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1627">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1628">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1628">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1629">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1629">'Identity'</span></span>
- <span data-ttu-id="2ef91-1630">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1630">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1631">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1631">'Razor'</span></span>
- <span data-ttu-id="2ef91-1632">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1632">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1633">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1633">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1634">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1634">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1635">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1635">'Identity'</span></span>
- <span data-ttu-id="2ef91-1636">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1636">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1637">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1637">'Razor'</span></span>
- <span data-ttu-id="2ef91-1638">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1638">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1639">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1639">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1640">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1640">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1641">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1641">'Identity'</span></span>
- <span data-ttu-id="2ef91-1642">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1642">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1643">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1643">'Razor'</span></span>
- <span data-ttu-id="2ef91-1644">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1644">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1645">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1645">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1646">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1646">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1647">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1647">'Identity'</span></span>
- <span data-ttu-id="2ef91-1648">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1648">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1649">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1649">'Razor'</span></span>
- <span data-ttu-id="2ef91-1650">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1650">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1651">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1651">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1652">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1652">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1653">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1653">'Identity'</span></span>
- <span data-ttu-id="2ef91-1654">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1654">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1655">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1655">'Razor'</span></span>
- <span data-ttu-id="2ef91-1656">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1656">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1657">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1657">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1658">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1658">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1659">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1659">'Identity'</span></span>
- <span data-ttu-id="2ef91-1660">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1660">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1661">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1661">'Razor'</span></span>
- <span data-ttu-id="2ef91-1662">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1662">'SignalR' uid:</span></span> 

<span data-ttu-id="2ef91-1663">------------ |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1663">------------ | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1664">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1664">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1665">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1665">'Identity'</span></span>
- <span data-ttu-id="2ef91-1666">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1666">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1667">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1667">'Razor'</span></span>
- <span data-ttu-id="2ef91-1668">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1668">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1669">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1669">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1670">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1670">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1671">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1671">'Identity'</span></span>
- <span data-ttu-id="2ef91-1672">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1672">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1673">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1673">'Razor'</span></span>
- <span data-ttu-id="2ef91-1674">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1674">'SignalR' uid:</span></span> 

<span data-ttu-id="2ef91-1675">---- || `CUSTOMCONNSTR_` |自訂提供者 || `MYSQLCONNSTR_` |[MySQL](https://www.mysql.com/) |
 MySQL |`SQLAZURECONNSTR_` | [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) |
 | `SQLCONNSTR_` | [SQL Server](https://www.microsoft.com/sql-server/)|</span><span class="sxs-lookup"><span data-stu-id="2ef91-1675">---- | | `CUSTOMCONNSTR_` | Custom provider | | `MYSQLCONNSTR_` | [MySQL](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [SQL Server](https://www.microsoft.com/sql-server/) |</span></span>

<span data-ttu-id="2ef91-1676">當探索到具有下表所顯示之任何四個前置詞的環境變數並將其載入到設定中時：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1676">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="2ef91-1677">會透過移除環境變數前置詞並新增設定機碼區段 (`ConnectionStrings`) 來移除具有下表顯示之前置詞的環境變數。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1677">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="2ef91-1678">會建立新的設定機碼值組以代表資料庫連線提供者 (`CUSTOMCONNSTR_` 除外，這沒有所述提供者)。</span><span class="sxs-lookup"><span data-stu-id="2ef91-1678">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="2ef91-1679">環境變數機碼</span><span class="sxs-lookup"><span data-stu-id="2ef91-1679">Environment variable key</span></span> | <span data-ttu-id="2ef91-1680">已轉換的設定機碼</span><span class="sxs-lookup"><span data-stu-id="2ef91-1680">Converted configuration key</span></span> | <span data-ttu-id="2ef91-1681">提供者設定項目</span><span class="sxs-lookup"><span data-stu-id="2ef91-1681">Provider configuration entry</span></span>                                                    |
| ---
<span data-ttu-id="2ef91-1682">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1682">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1683">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1683">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1684">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1684">'Identity'</span></span>
- <span data-ttu-id="2ef91-1685">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1685">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1686">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1686">'Razor'</span></span>
- <span data-ttu-id="2ef91-1687">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1687">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1688">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1688">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1689">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1689">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1690">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1690">'Identity'</span></span>
- <span data-ttu-id="2ef91-1691">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1691">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1692">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1692">'Razor'</span></span>
- <span data-ttu-id="2ef91-1693">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1693">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1694">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1694">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1695">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1695">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1696">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1696">'Identity'</span></span>
- <span data-ttu-id="2ef91-1697">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1697">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1698">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1698">'Razor'</span></span>
- <span data-ttu-id="2ef91-1699">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1699">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1700">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1700">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1701">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1701">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1702">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1702">'Identity'</span></span>
- <span data-ttu-id="2ef91-1703">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1703">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1704">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1704">'Razor'</span></span>
- <span data-ttu-id="2ef91-1705">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1705">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1706">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1706">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1707">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1707">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1708">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1708">'Identity'</span></span>
- <span data-ttu-id="2ef91-1709">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1709">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1710">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1710">'Razor'</span></span>
- <span data-ttu-id="2ef91-1711">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1711">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1712">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1712">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1713">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1713">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1714">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1714">'Identity'</span></span>
- <span data-ttu-id="2ef91-1715">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1715">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1716">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1716">'Razor'</span></span>
- <span data-ttu-id="2ef91-1717">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1717">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1718">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1718">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1719">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1719">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1720">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1720">'Identity'</span></span>
- <span data-ttu-id="2ef91-1721">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1721">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1722">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1722">'Razor'</span></span>
- <span data-ttu-id="2ef91-1723">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1723">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1724">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1724">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1725">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1725">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1726">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1726">'Identity'</span></span>
- <span data-ttu-id="2ef91-1727">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1727">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1728">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1728">'Razor'</span></span>
- <span data-ttu-id="2ef91-1729">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1729">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1730">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1730">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1731">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1731">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1732">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1732">'Identity'</span></span>
- <span data-ttu-id="2ef91-1733">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1733">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1734">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1734">'Razor'</span></span>
- <span data-ttu-id="2ef91-1735">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1735">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1736">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1736">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1737">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1737">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1738">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1738">'Identity'</span></span>
- <span data-ttu-id="2ef91-1739">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1739">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1740">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1740">'Razor'</span></span>
- <span data-ttu-id="2ef91-1741">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1741">'SignalR' uid:</span></span> 

<span data-ttu-id="2ef91-1742">------------ |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1742">------------ | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1743">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1743">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1744">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1744">'Identity'</span></span>
- <span data-ttu-id="2ef91-1745">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1745">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1746">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1746">'Razor'</span></span>
- <span data-ttu-id="2ef91-1747">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1747">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1748">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1748">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1749">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1749">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1750">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1750">'Identity'</span></span>
- <span data-ttu-id="2ef91-1751">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1751">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1752">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1752">'Razor'</span></span>
- <span data-ttu-id="2ef91-1753">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1753">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1754">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1754">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1755">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1755">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1756">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1756">'Identity'</span></span>
- <span data-ttu-id="2ef91-1757">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1757">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1758">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1758">'Razor'</span></span>
- <span data-ttu-id="2ef91-1759">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1759">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1760">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1760">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1761">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1761">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1762">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1762">'Identity'</span></span>
- <span data-ttu-id="2ef91-1763">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1763">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1764">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1764">'Razor'</span></span>
- <span data-ttu-id="2ef91-1765">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1765">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1766">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1766">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1767">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1767">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1768">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1768">'Identity'</span></span>
- <span data-ttu-id="2ef91-1769">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1769">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1770">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1770">'Razor'</span></span>
- <span data-ttu-id="2ef91-1771">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1771">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1772">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1772">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1773">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1773">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1774">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1774">'Identity'</span></span>
- <span data-ttu-id="2ef91-1775">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1775">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1776">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1776">'Razor'</span></span>
- <span data-ttu-id="2ef91-1777">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1777">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1778">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1778">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1779">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1779">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1780">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1780">'Identity'</span></span>
- <span data-ttu-id="2ef91-1781">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1781">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1782">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1782">'Razor'</span></span>
- <span data-ttu-id="2ef91-1783">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1783">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1784">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1784">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1785">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1785">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1786">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1786">'Identity'</span></span>
- <span data-ttu-id="2ef91-1787">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1787">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1788">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1788">'Razor'</span></span>
- <span data-ttu-id="2ef91-1789">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1789">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1790">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1790">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1791">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1791">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1792">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1792">'Identity'</span></span>
- <span data-ttu-id="2ef91-1793">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1793">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1794">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1794">'Razor'</span></span>
- <span data-ttu-id="2ef91-1795">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1795">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1796">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1796">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1797">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1797">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1798">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1798">'Identity'</span></span>
- <span data-ttu-id="2ef91-1799">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1799">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1800">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1800">'Razor'</span></span>
- <span data-ttu-id="2ef91-1801">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1801">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1802">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1802">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1803">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1803">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1804">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1804">'Identity'</span></span>
- <span data-ttu-id="2ef91-1805">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1805">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1806">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1806">'Razor'</span></span>
- <span data-ttu-id="2ef91-1807">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1807">'SignalR' uid:</span></span> 

<span data-ttu-id="2ef91-1808">-------------- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1808">-------------- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1809">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1809">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1810">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1810">'Identity'</span></span>
- <span data-ttu-id="2ef91-1811">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1811">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1812">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1812">'Razor'</span></span>
- <span data-ttu-id="2ef91-1813">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1813">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1814">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1814">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1815">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1815">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1816">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1816">'Identity'</span></span>
- <span data-ttu-id="2ef91-1817">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1817">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1818">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1818">'Razor'</span></span>
- <span data-ttu-id="2ef91-1819">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1819">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1820">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1820">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1821">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1821">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1822">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1822">'Identity'</span></span>
- <span data-ttu-id="2ef91-1823">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1823">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1824">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1824">'Razor'</span></span>
- <span data-ttu-id="2ef91-1825">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1825">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1826">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1826">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1827">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1827">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1828">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1828">'Identity'</span></span>
- <span data-ttu-id="2ef91-1829">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1829">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1830">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1830">'Razor'</span></span>
- <span data-ttu-id="2ef91-1831">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1831">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1832">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1832">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1833">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1833">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1834">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1834">'Identity'</span></span>
- <span data-ttu-id="2ef91-1835">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1835">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1836">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1836">'Razor'</span></span>
- <span data-ttu-id="2ef91-1837">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1837">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1838">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1838">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1839">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1839">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1840">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1840">'Identity'</span></span>
- <span data-ttu-id="2ef91-1841">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1841">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1842">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1842">'Razor'</span></span>
- <span data-ttu-id="2ef91-1843">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1843">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1844">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1844">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1845">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1845">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1846">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1846">'Identity'</span></span>
- <span data-ttu-id="2ef91-1847">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1847">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1848">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1848">'Razor'</span></span>
- <span data-ttu-id="2ef91-1849">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1849">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1850">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1850">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1851">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1851">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1852">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1852">'Identity'</span></span>
- <span data-ttu-id="2ef91-1853">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1853">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1854">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1854">'Razor'</span></span>
- <span data-ttu-id="2ef91-1855">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1855">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1856">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1856">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1857">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1857">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1858">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1858">'Identity'</span></span>
- <span data-ttu-id="2ef91-1859">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1859">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1860">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1860">'Razor'</span></span>
- <span data-ttu-id="2ef91-1861">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1861">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1862">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1862">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1863">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1863">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1864">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1864">'Identity'</span></span>
- <span data-ttu-id="2ef91-1865">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1865">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1866">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1866">'Razor'</span></span>
- <span data-ttu-id="2ef91-1867">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1867">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1868">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1868">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1869">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1869">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1870">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1870">'Identity'</span></span>
- <span data-ttu-id="2ef91-1871">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1871">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1872">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1872">'Razor'</span></span>
- <span data-ttu-id="2ef91-1873">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1873">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1874">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1874">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1875">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1875">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1876">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1876">'Identity'</span></span>
- <span data-ttu-id="2ef91-1877">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1877">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1878">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1878">'Razor'</span></span>
- <span data-ttu-id="2ef91-1879">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1879">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1880">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1880">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1881">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1881">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1882">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1882">'Identity'</span></span>
- <span data-ttu-id="2ef91-1883">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1883">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1884">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1884">'Razor'</span></span>
- <span data-ttu-id="2ef91-1885">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1885">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1886">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1886">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1887">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1887">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1888">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1888">'Identity'</span></span>
- <span data-ttu-id="2ef91-1889">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1889">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1890">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1890">'Razor'</span></span>
- <span data-ttu-id="2ef91-1891">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1891">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1892">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1892">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1893">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1893">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1894">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1894">'Identity'</span></span>
- <span data-ttu-id="2ef91-1895">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1895">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1896">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1896">'Razor'</span></span>
- <span data-ttu-id="2ef91-1897">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1897">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1898">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1898">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1899">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1899">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1900">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1900">'Identity'</span></span>
- <span data-ttu-id="2ef91-1901">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1901">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1902">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1902">'Razor'</span></span>
- <span data-ttu-id="2ef91-1903">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1903">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1904">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1904">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1905">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1905">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1906">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1906">'Identity'</span></span>
- <span data-ttu-id="2ef91-1907">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1907">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1908">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1908">'Razor'</span></span>
- <span data-ttu-id="2ef91-1909">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1909">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1910">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1910">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1911">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1911">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1912">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1912">'Identity'</span></span>
- <span data-ttu-id="2ef91-1913">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1913">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1914">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1914">'Razor'</span></span>
- <span data-ttu-id="2ef91-1915">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1915">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1916">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1916">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1917">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1917">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1918">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1918">'Identity'</span></span>
- <span data-ttu-id="2ef91-1919">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1919">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1920">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1920">'Razor'</span></span>
- <span data-ttu-id="2ef91-1921">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1921">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1922">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1922">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1923">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1923">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1924">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1924">'Identity'</span></span>
- <span data-ttu-id="2ef91-1925">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1925">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1926">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1926">'Razor'</span></span>
- <span data-ttu-id="2ef91-1927">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1927">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1928">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1928">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1929">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1929">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1930">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1930">'Identity'</span></span>
- <span data-ttu-id="2ef91-1931">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1931">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1932">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1932">'Razor'</span></span>
- <span data-ttu-id="2ef91-1933">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1933">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1934">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1934">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1935">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1935">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1936">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1936">'Identity'</span></span>
- <span data-ttu-id="2ef91-1937">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1937">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1938">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1938">'Razor'</span></span>
- <span data-ttu-id="2ef91-1939">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1939">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1940">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1940">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1941">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1941">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1942">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1942">'Identity'</span></span>
- <span data-ttu-id="2ef91-1943">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1943">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1944">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1944">'Razor'</span></span>
- <span data-ttu-id="2ef91-1945">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1945">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1946">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1946">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1947">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1947">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1948">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1948">'Identity'</span></span>
- <span data-ttu-id="2ef91-1949">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1949">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1950">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1950">'Razor'</span></span>
- <span data-ttu-id="2ef91-1951">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1951">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1952">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1952">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1953">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1953">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1954">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1954">'Identity'</span></span>
- <span data-ttu-id="2ef91-1955">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1955">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1956">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1956">'Razor'</span></span>
- <span data-ttu-id="2ef91-1957">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1957">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1958">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1958">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1959">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1959">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1960">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1960">'Identity'</span></span>
- <span data-ttu-id="2ef91-1961">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1961">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1962">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1962">'Razor'</span></span>
- <span data-ttu-id="2ef91-1963">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1963">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1964">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1964">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1965">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1965">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1966">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1966">'Identity'</span></span>
- <span data-ttu-id="2ef91-1967">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1967">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1968">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1968">'Razor'</span></span>
- <span data-ttu-id="2ef91-1969">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1969">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1970">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1970">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1971">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1971">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1972">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1972">'Identity'</span></span>
- <span data-ttu-id="2ef91-1973">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1973">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1974">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1974">'Razor'</span></span>
- <span data-ttu-id="2ef91-1975">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1975">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1976">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1976">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1977">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1977">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1978">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1978">'Identity'</span></span>
- <span data-ttu-id="2ef91-1979">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1979">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1980">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1980">'Razor'</span></span>
- <span data-ttu-id="2ef91-1981">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1981">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1982">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1982">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1983">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1983">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1984">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1984">'Identity'</span></span>
- <span data-ttu-id="2ef91-1985">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1985">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1986">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1986">'Razor'</span></span>
- <span data-ttu-id="2ef91-1987">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1987">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1988">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1988">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1989">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1989">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1990">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1990">'Identity'</span></span>
- <span data-ttu-id="2ef91-1991">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1991">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1992">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1992">'Razor'</span></span>
- <span data-ttu-id="2ef91-1993">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1993">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-1994">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1994">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-1995">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1995">'Blazor'</span></span>
- <span data-ttu-id="2ef91-1996">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1996">'Identity'</span></span>
- <span data-ttu-id="2ef91-1997">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1997">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-1998">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-1998">'Razor'</span></span>
- <span data-ttu-id="2ef91-1999">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-1999">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2000">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2000">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2001">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2001">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2002">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2002">'Identity'</span></span>
- <span data-ttu-id="2ef91-2003">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2003">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2004">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2004">'Razor'</span></span>
- <span data-ttu-id="2ef91-2005">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2005">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2006">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2006">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2007">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2007">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2008">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2008">'Identity'</span></span>
- <span data-ttu-id="2ef91-2009">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2009">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2010">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2010">'Razor'</span></span>
- <span data-ttu-id="2ef91-2011">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2011">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2012">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2012">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2013">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2013">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2014">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2014">'Identity'</span></span>
- <span data-ttu-id="2ef91-2015">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2015">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2016">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2016">'Razor'</span></span>
- <span data-ttu-id="2ef91-2017">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2017">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2018">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2018">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2019">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2019">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2020">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2020">'Identity'</span></span>
- <span data-ttu-id="2ef91-2021">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2021">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2022">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2022">'Razor'</span></span>
- <span data-ttu-id="2ef91-2023">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2023">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2024">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2024">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2025">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2025">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2026">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2026">'Identity'</span></span>
- <span data-ttu-id="2ef91-2027">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2027">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2028">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2028">'Razor'</span></span>
- <span data-ttu-id="2ef91-2029">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2029">'SignalR' uid:</span></span> 

<span data-ttu-id="2ef91-2030">---------------------------------------- | |`CUSTOMCONNSTR_{KEY} `   | `ConnectionStrings:{KEY}`  |未建立設定專案。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2030">---------------------------------------- | | `CUSTOMCONNSTR_{KEY} `   | `ConnectionStrings:{KEY}`   | Configuration entry not created.</span></span>                                                <span data-ttu-id="2ef91-2031">| |`MYSQLCONNSTR_{KEY}`     | `ConnectionStrings:{KEY}`  |機碼 `ConnectionStrings:{KEY}_ProviderName` ：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2031">| | `MYSQLCONNSTR_{KEY}`     | `ConnectionStrings:{KEY}`   | Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="2ef91-2032">值： `MySql.Data.MySqlClient` | | `SQLAZURECONNSTR_{KEY}`   |  `ConnectionStrings:{KEY}`  |機碼 `ConnectionStrings:{KEY}_ProviderName` ：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2032">Value: `MySql.Data.MySqlClient` | | `SQLAZURECONNSTR_{KEY}`  | `ConnectionStrings:{KEY}`   | Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="2ef91-2033">值： `System.Data.SqlClient` | | `SQLCONNSTR_{KEY}`        |  `ConnectionStrings:{KEY}`  |機碼 `ConnectionStrings:{KEY}_ProviderName` ：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2033">Value: `System.Data.SqlClient`  | | `SQLCONNSTR_{KEY}`       | `ConnectionStrings:{KEY}`   | Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="2ef91-2034">Value`System.Data.SqlClient`  |</span><span class="sxs-lookup"><span data-stu-id="2ef91-2034">Value: `System.Data.SqlClient`  |</span></span>

<span data-ttu-id="2ef91-2035">**範例**</span><span class="sxs-lookup"><span data-stu-id="2ef91-2035">**Example**</span></span>

<span data-ttu-id="2ef91-2036">在伺服器上建立自訂連接字串環境變數：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2036">A custom connection string environment variable is created on the server:</span></span>

* <span data-ttu-id="2ef91-2037">名稱 &ndash;`CUSTOMCONNSTR_ReleaseDB`</span><span class="sxs-lookup"><span data-stu-id="2ef91-2037">Name &ndash; `CUSTOMCONNSTR_ReleaseDB`</span></span>
* <span data-ttu-id="2ef91-2038">值 &ndash;`Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`</span><span class="sxs-lookup"><span data-stu-id="2ef91-2038">Value &ndash; `Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`</span></span>

<span data-ttu-id="2ef91-2039">如果 `IConfiguration` 插入，並將其指派給名為的欄位 `_config` ，請閱讀值：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2039">If `IConfiguration` is injected and assigned to a field named `_config`, read the value:</span></span>

```csharp
_config["ConnectionStrings:ReleaseDB"]
```

## <a name="file-configuration-provider"></a><span data-ttu-id="2ef91-2040">檔案設定提供者</span><span class="sxs-lookup"><span data-stu-id="2ef91-2040">File Configuration Provider</span></span>

<span data-ttu-id="2ef91-2041"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> 是用於從檔案系統載入設定的基底類別。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2041"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="2ef91-2042">下列設定提供者專用於特定檔案類型：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2042">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="2ef91-2043">INI 設定提供者</span><span class="sxs-lookup"><span data-stu-id="2ef91-2043">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="2ef91-2044">JSON 設定提供者</span><span class="sxs-lookup"><span data-stu-id="2ef91-2044">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="2ef91-2045">XML 設定提供者</span><span class="sxs-lookup"><span data-stu-id="2ef91-2045">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="2ef91-2046">INI 設定提供者</span><span class="sxs-lookup"><span data-stu-id="2ef91-2046">INI Configuration Provider</span></span>

<span data-ttu-id="2ef91-2047"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> 會在執行階段從 INI 檔案機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2047">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="2ef91-2048">若要啟用 INI 檔案設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2048">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="2ef91-2049">冒號可用來做為 INI 檔案設定中的區段分隔符號。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2049">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="2ef91-2050">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2050">Overloads permit specifying:</span></span>

* <span data-ttu-id="2ef91-2051">檔案是否為選擇性。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2051">Whether the file is optional.</span></span>
* <span data-ttu-id="2ef91-2052">檔案變更時是否要重新載入設定。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2052">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="2ef91-2053"><xref:Microsoft.Extensions.FileProviders.IFileProvider> 是用於存取該檔案。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2053">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="2ef91-2054">建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2054">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="2ef91-2055">INI 設定檔的一般範例：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2055">A generic example of an INI configuration file:</span></span>

```ini
[section0]
key0=value
key1=value

[section1]
subsection:key=value

[section2:subsection0]
key=value

[section2:subsection1]
key=value
```

<span data-ttu-id="2ef91-2056">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2056">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="2ef91-2057">section0:key0</span><span class="sxs-lookup"><span data-stu-id="2ef91-2057">section0:key0</span></span>
* <span data-ttu-id="2ef91-2058">section0:key1</span><span class="sxs-lookup"><span data-stu-id="2ef91-2058">section0:key1</span></span>
* <span data-ttu-id="2ef91-2059">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="2ef91-2059">section1:subsection:key</span></span>
* <span data-ttu-id="2ef91-2060">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="2ef91-2060">section2:subsection0:key</span></span>
* <span data-ttu-id="2ef91-2061">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="2ef91-2061">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="2ef91-2062">JSON 設定提供者</span><span class="sxs-lookup"><span data-stu-id="2ef91-2062">JSON Configuration Provider</span></span>

<span data-ttu-id="2ef91-2063"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> 會在執行階段從 JSON 檔案機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2063">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="2ef91-2064">若要啟用 JSON 檔案設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2064">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="2ef91-2065">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2065">Overloads permit specifying:</span></span>

* <span data-ttu-id="2ef91-2066">檔案是否為選擇性。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2066">Whether the file is optional.</span></span>
* <span data-ttu-id="2ef91-2067">檔案變更時是否要重新載入設定。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2067">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="2ef91-2068"><xref:Microsoft.Extensions.FileProviders.IFileProvider> 是用於存取該檔案。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2068">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="2ef91-2069">`AddJsonFile`當使用初始化新的主機產生器時，會自動呼叫兩次 `CreateDefaultBuilder` 。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2069">`AddJsonFile` is automatically called twice when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="2ef91-2070">會呼叫此方法以從下列位置載入設定：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2070">The method is called to load configuration from:</span></span>

* <span data-ttu-id="2ef91-2071">*appsettings.json* &ndash; 會先讀取此檔案。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2071">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="2ef91-2072">檔案的環境版本可以覆寫由 *appsettings.json* 檔案提供的值。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2072">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="2ef91-2073">*appsettings。{環境}. json* ：檔案的 &ndash; 環境版本是根據 IHostingEnvironment 所載入。 [EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*)。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2073">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="2ef91-2074">如需詳細資訊，請參閱[＜預設組態＞](#default-configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2074">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="2ef91-2075">`CreateDefaultBuilder` 也會載入：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2075">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="2ef91-2076">環境變數。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2076">Environment variables.</span></span>
* <span data-ttu-id="2ef91-2077">開發環境中的[使用者祕密 (祕密管理員)](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2077">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="2ef91-2078">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2078">Command-line arguments.</span></span>

<span data-ttu-id="2ef91-2079">會先建立 JSON 設定提供者。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2079">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="2ef91-2080">因此，使用者祕密、環境變數與命令列引數會覆寫由 *appsettings* 檔案設定的設定。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2080">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="2ef91-2081">在建置主機時，呼叫 `ConfigureAppConfiguration` 以指定檔案 (除了 *appsettings.json* 和 \* appsettings.{Environment}.json\* 以外) 的應用程式設定：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2081">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="2ef91-2082">**範例**</span><span class="sxs-lookup"><span data-stu-id="2ef91-2082">**Example**</span></span>

<span data-ttu-id="2ef91-2083">範例應用程式會利用靜態便利方法 `CreateDefaultBuilder` 來建立主機，其中包含兩個對的呼叫 `AddJsonFile` ：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2083">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`:</span></span>

* <span data-ttu-id="2ef91-2084">第一次呼叫時，會 `AddJsonFile` 從*appsettings*載入設定：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2084">The first call to `AddJsonFile` loads configuration from *appsettings.json*:</span></span>

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.json)]

* <span data-ttu-id="2ef91-2085">第二個呼叫會 `AddJsonFile` 從 appsettings 載入設定 *。 {環境}. json*。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2085">The second call to `AddJsonFile` loads configuration from *appsettings.{Environment}.json*.</span></span> <span data-ttu-id="2ef91-2086">適用于*appsettings。* 範例應用程式中的開發 json 會載入下列檔案：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2086">For *appsettings.Development.json* in the sample app, the following file is loaded:</span></span>

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.Development.json)]

1. <span data-ttu-id="2ef91-2087">執行範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2087">Run the sample app.</span></span> <span data-ttu-id="2ef91-2088">開啟瀏覽器以瀏覽位於 `http://localhost:5000` 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2088">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="2ef91-2089">輸出包含以應用程式環境為基礎之設定的索引鍵/值組。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2089">The output contains key-value pairs for the configuration based on the app's environment.</span></span> <span data-ttu-id="2ef91-2090">金鑰的記錄層級 `Logging:LogLevel:Default` 是在 `Debug` 開發環境中執行應用程式時。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2090">The log level for the key `Logging:LogLevel:Default` is `Debug` when running the app in the Development environment.</span></span>
1. <span data-ttu-id="2ef91-2091">在生產環境中再次執行範例應用程式：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2091">Run the sample app again in the Production environment:</span></span>
   1. <span data-ttu-id="2ef91-2092">開啟*Properties/launchsettings.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2092">Open the *Properties/launchSettings.json* file.</span></span>
   1. <span data-ttu-id="2ef91-2093">在 `ConfigurationSample` 設定檔中，將環境變數的值變更 `ASPNETCORE_ENVIRONMENT` 為 `Production` 。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2093">In the `ConfigurationSample` profile, change the value of the `ASPNETCORE_ENVIRONMENT` environment variable to `Production`.</span></span>
   1. <span data-ttu-id="2ef91-2094">儲存檔案，並 `dotnet run` 在命令 shell 中使用來執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2094">Save the file and run the app with `dotnet run` in a command shell.</span></span>
1. <span data-ttu-id="2ef91-2095">Appsettings 中的設定 *。* 在*appsettings*中，不會再覆寫 json 中的設定。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2095">The settings in the *appsettings.Development.json* no longer override the settings in *appsettings.json*.</span></span> <span data-ttu-id="2ef91-2096">索引鍵的記錄層級 `Logging:LogLevel:Default` 是 `Warning` 。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2096">The log level for the key `Logging:LogLevel:Default` is `Warning`.</span></span>

### <a name="xml-configuration-provider"></a><span data-ttu-id="2ef91-2097">XML 設定提供者</span><span class="sxs-lookup"><span data-stu-id="2ef91-2097">XML Configuration Provider</span></span>

<span data-ttu-id="2ef91-2098"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> 會在執行階段從 XML 檔案機碼值組載入設定。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2098">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="2ef91-2099">若要啟用 XML 檔案設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2099">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="2ef91-2100">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2100">Overloads permit specifying:</span></span>

* <span data-ttu-id="2ef91-2101">檔案是否為選擇性。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2101">Whether the file is optional.</span></span>
* <span data-ttu-id="2ef91-2102">檔案變更時是否要重新載入設定。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2102">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="2ef91-2103"><xref:Microsoft.Extensions.FileProviders.IFileProvider> 是用於存取該檔案。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2103">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="2ef91-2104">建立設定機碼值組時，會忽略設定檔案的根節點。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2104">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="2ef91-2105">請勿在檔案中指定文件類型定義 (DTD) 或命名空間。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2105">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="2ef91-2106">建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2106">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="2ef91-2107">XML 設定檔可以針對重複的區段使用相異元素名稱：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2107">XML configuration files can use distinct element names for repeating sections:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <section0>
    <key0>value</key0>
    <key1>value</key1>
  </section0>
  <section1>
    <key0>value</key0>
    <key1>value</key1>
  </section1>
</configuration>
```

<span data-ttu-id="2ef91-2108">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2108">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="2ef91-2109">section0:key0</span><span class="sxs-lookup"><span data-stu-id="2ef91-2109">section0:key0</span></span>
* <span data-ttu-id="2ef91-2110">section0:key1</span><span class="sxs-lookup"><span data-stu-id="2ef91-2110">section0:key1</span></span>
* <span data-ttu-id="2ef91-2111">section1:key0</span><span class="sxs-lookup"><span data-stu-id="2ef91-2111">section1:key0</span></span>
* <span data-ttu-id="2ef91-2112">section1:key1</span><span class="sxs-lookup"><span data-stu-id="2ef91-2112">section1:key1</span></span>

<span data-ttu-id="2ef91-2113">若 `name` 屬性是用來區別元素，則可以重複那些使用相同元素名稱的元素：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2113">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <section name="section0">
    <key name="key0">value</key>
    <key name="key1">value</key>
  </section>
  <section name="section1">
    <key name="key0">value</key>
    <key name="key1">value</key>
  </section>
</configuration>
```

<span data-ttu-id="2ef91-2114">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2114">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="2ef91-2115">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="2ef91-2115">section:section0:key:key0</span></span>
* <span data-ttu-id="2ef91-2116">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="2ef91-2116">section:section0:key:key1</span></span>
* <span data-ttu-id="2ef91-2117">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="2ef91-2117">section:section1:key:key0</span></span>
* <span data-ttu-id="2ef91-2118">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="2ef91-2118">section:section1:key:key1</span></span>

<span data-ttu-id="2ef91-2119">屬性可用來提供值：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2119">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="2ef91-2120">先前的設定檔會使用 `value` 載入下列機碼：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2120">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="2ef91-2121">key:attribute</span><span class="sxs-lookup"><span data-stu-id="2ef91-2121">key:attribute</span></span>
* <span data-ttu-id="2ef91-2122">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="2ef91-2122">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="2ef91-2123">每個檔案機碼設定提供者</span><span class="sxs-lookup"><span data-stu-id="2ef91-2123">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="2ef91-2124"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> 使用目錄的檔案做為設定機碼值組。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2124">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="2ef91-2125">機碼是檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2125">The key is the file name.</span></span> <span data-ttu-id="2ef91-2126">值包含檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2126">The value contains the file's contents.</span></span> <span data-ttu-id="2ef91-2127">每個檔案機碼設定提供者是在 Docker 主機案例中使用。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2127">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="2ef91-2128">若要啟用每個檔案機碼設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2128">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="2ef91-2129">檔案的 `directoryPath` 必須是絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2129">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="2ef91-2130">多載允許指定：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2130">Overloads permit specifying:</span></span>

* <span data-ttu-id="2ef91-2131">設定來源的 `Action<KeyPerFileConfigurationSource>` 委派。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2131">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="2ef91-2132">目錄是否為選擇性與目錄的路徑。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2132">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="2ef91-2133">雙底線 (`__`) 是做為檔案名稱中的設定金鑰分隔符號使用。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2133">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="2ef91-2134">例如，檔案名稱 `Logging__LogLevel__System` 會產生設定金鑰 `Logging:LogLevel:System`。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2134">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="2ef91-2135">建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2135">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

## <a name="memory-configuration-provider"></a><span data-ttu-id="2ef91-2136">記憶體設定提供者</span><span class="sxs-lookup"><span data-stu-id="2ef91-2136">Memory Configuration Provider</span></span>

<span data-ttu-id="2ef91-2137"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> 使用記憶體內集合做為設定機碼值組。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2137">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="2ef91-2138">若要啟用記憶體內集合設定，請在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的執行個體上呼叫 <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> 延伸模組方法。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2138">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="2ef91-2139">設定提供者可以使用 `IEnumerable<KeyValuePair<String,String>>` 來初始化。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2139">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="2ef91-2140">建置主機時呼叫 `ConfigureAppConfiguration` 以指定應用程式的設定。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2140">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="2ef91-2141">下列範例會建立組態字典：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2141">In the following example, a configuration dictionary is created:</span></span>

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

<span data-ttu-id="2ef91-2142">字典會與 `AddInMemoryCollection` 的呼叫搭配使用以提供組態：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2142">The dictionary is used with a call to `AddInMemoryCollection` to provide the configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a><span data-ttu-id="2ef91-2143">GetValue</span><span class="sxs-lookup"><span data-stu-id="2ef91-2143">GetValue</span></span>

<span data-ttu-id="2ef91-2144">[`ConfigurationBinder.GetValue<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*)使用指定的索引鍵從設定中解壓縮單一值，並將其轉換為指定的 noncollection 類型。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2144">[`ConfigurationBinder.GetValue<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a single value from configuration with a specified key and converts it to the specified noncollection type.</span></span> <span data-ttu-id="2ef91-2145">多載會接受預設值。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2145">An overload accepts a default value.</span></span>

<span data-ttu-id="2ef91-2146">下列範例將：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2146">The following example:</span></span>

* <span data-ttu-id="2ef91-2147">從具有機碼 `NumberKey` 的組態擷取字串值。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2147">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="2ef91-2148">若在組態機碼中找不到 `NumberKey`，則會使用預設值 `99`。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2148">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="2ef91-2149">鍵入值為 `int`。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2149">Types the value as an `int`.</span></span>
* <span data-ttu-id="2ef91-2150">在 `NumberConfig` 屬性中儲存值供頁面使用。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2150">Stores the value in the `NumberConfig` property for use by the page.</span></span>

```csharp
public class IndexModel : PageModel
{
    public IndexModel(IConfiguration config)
    {
        _config = config;
    }

    public int NumberConfig { get; private set; }

    public void OnGet()
    {
        NumberConfig = _config.GetValue<int>("NumberKey", 99);
    }
}
```

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="2ef91-2151">GetSection、GetChildren 與 Exists</span><span class="sxs-lookup"><span data-stu-id="2ef91-2151">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="2ef91-2152">針對下面的範例，請考慮下列 JSON 檔案。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2152">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="2ef91-2153">在兩個區段中找到四個機碼，其中一個包括子區段組：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2153">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  },
  "section2": {
    "subsection0" : {
      "key0": "value",
      "key1": "value"
    },
    "subsection1" : {
      "key0": "value",
      "key1": "value"
    }
  }
}
```

<span data-ttu-id="2ef91-2154">當檔案被讀入到設定之後，會建立下列唯一階層式機碼以存放設定值：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2154">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="2ef91-2155">section0:key0</span><span class="sxs-lookup"><span data-stu-id="2ef91-2155">section0:key0</span></span>
* <span data-ttu-id="2ef91-2156">section0:key1</span><span class="sxs-lookup"><span data-stu-id="2ef91-2156">section0:key1</span></span>
* <span data-ttu-id="2ef91-2157">section1:key0</span><span class="sxs-lookup"><span data-stu-id="2ef91-2157">section1:key0</span></span>
* <span data-ttu-id="2ef91-2158">section1:key1</span><span class="sxs-lookup"><span data-stu-id="2ef91-2158">section1:key1</span></span>
* <span data-ttu-id="2ef91-2159">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="2ef91-2159">section2:subsection0:key0</span></span>
* <span data-ttu-id="2ef91-2160">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="2ef91-2160">section2:subsection0:key1</span></span>
* <span data-ttu-id="2ef91-2161">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="2ef91-2161">section2:subsection1:key0</span></span>
* <span data-ttu-id="2ef91-2162">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="2ef91-2162">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="2ef91-2163">GetSection</span><span class="sxs-lookup"><span data-stu-id="2ef91-2163">GetSection</span></span>

<span data-ttu-id="2ef91-2164">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) 會擷取具有所指定子區段機碼的設定子區段。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2164">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="2ef91-2165">若要傳回 `section1` 中只包含一個機碼值組的 <xref:Microsoft.Extensions.Configuration.IConfigurationSection>，請呼叫 `GetSection` 並提供區段名稱：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2165">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="2ef91-2166">`configSection` 沒有值，只有索引鍵和路徑。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2166">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="2ef91-2167">同樣地，若要取得 `section2:subsection0` 中之機碼的值，請呼叫 `GetSection` 並提供區段路徑：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2167">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="2ef91-2168">`GetSection` 絕不會傳回 `null`。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2168">`GetSection` never returns `null`.</span></span> <span data-ttu-id="2ef91-2169">若找不到相符的區段，會傳回空的 `IConfigurationSection`。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2169">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="2ef91-2170">當 `GetSection` 傳回相符區段時，未填入 <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value>。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2170">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="2ef91-2171">當區段存在時，會傳回 <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> 與 <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path>。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2171">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="2ef91-2172">GetChildren</span><span class="sxs-lookup"><span data-stu-id="2ef91-2172">GetChildren</span></span>

<span data-ttu-id="2ef91-2173">對 `section2` 上之 [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) 的呼叫會取得包括下列項目的 `IEnumerable<IConfigurationSection>`：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2173">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="2ef91-2174">Exists</span><span class="sxs-lookup"><span data-stu-id="2ef91-2174">Exists</span></span>

<span data-ttu-id="2ef91-2175">使用 [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) 來判斷設定區段是否存在：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2175">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="2ef91-2176">以範例資料為例， `sectionExists` 是 `false`，因未設定資料中沒有 `section2:subsection2` 區段。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2176">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="2ef91-2177">繫結至物件圖形</span><span class="sxs-lookup"><span data-stu-id="2ef91-2177">Bind to an object graph</span></span>

<span data-ttu-id="2ef91-2178"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> 可以繫結整個 POCO 物件圖形。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2178"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="2ef91-2179">如同系結簡單的物件，只會系結公用讀取/寫入屬性。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2179">As with binding a simple object, only public read/write properties are bound.</span></span>

<span data-ttu-id="2ef91-2180">範例包含 `TvShow` 模型，其物件圖形包括 `Metadata` 與 `Actors` 類別 (*Models/TvShow.cs*)：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2180">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

<span data-ttu-id="2ef91-2181">範例應用程式有 *tvshow.xml* 檔案，其中包含設定資料：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2181">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

<span data-ttu-id="2ef91-2182">設定會被繫結到整個 `TvShow` 物件 (使用 `Bind` 方法)。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2182">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="2ef91-2183">已繫結的執行個體會被指派給屬性以用於轉譯：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2183">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="2ef91-2184">[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*)系結並傳回指定的型別。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2184">[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="2ef91-2185">`Get<T>` 比使用 `Bind` 更方便。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2185">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="2ef91-2186">下列程式碼示範如何 `Get<T>` 在上述範例中使用：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2186">The following code shows how to use `Get<T>` with the preceding example:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="2ef91-2187">將陣列繫結到類別</span><span class="sxs-lookup"><span data-stu-id="2ef91-2187">Bind an array to a class</span></span>

<span data-ttu-id="2ef91-2188">範例應用程式示範此節中解釋的概念。\*\*</span><span class="sxs-lookup"><span data-stu-id="2ef91-2188">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="2ef91-2189"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> 支援在設定機碼中使用陣列索引將陣列繫結到物件。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2189">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="2ef91-2190">公開數值索引鍵區段（、、）的任何陣列格式， `:0:` `:1:` 都能夠系結 &hellip; `:{n}:` 至 POCO 類別陣列的陣列。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2190">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="2ef91-2191">繫結是由慣例提供。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2191">Binding is provided by convention.</span></span> <span data-ttu-id="2ef91-2192">自訂設定提供者不需要實作陣列繫結。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2192">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="2ef91-2193">**記憶體內陣列處理**</span><span class="sxs-lookup"><span data-stu-id="2ef91-2193">**In-memory array processing**</span></span>

<span data-ttu-id="2ef91-2194">考慮下表中顯示的設定機碼與值。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2194">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="2ef91-2195">Key</span><span class="sxs-lookup"><span data-stu-id="2ef91-2195">Key</span></span>             | <span data-ttu-id="2ef91-2196">值</span><span class="sxs-lookup"><span data-stu-id="2ef91-2196">Value</span></span>  |
| :---
<span data-ttu-id="2ef91-2197">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2197">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2198">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2198">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2199">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2199">'Identity'</span></span>
- <span data-ttu-id="2ef91-2200">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2200">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2201">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2201">'Razor'</span></span>
- <span data-ttu-id="2ef91-2202">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2202">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2203">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2203">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2204">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2204">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2205">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2205">'Identity'</span></span>
- <span data-ttu-id="2ef91-2206">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2206">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2207">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2207">'Razor'</span></span>
- <span data-ttu-id="2ef91-2208">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2208">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2209">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2209">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2210">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2210">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2211">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2211">'Identity'</span></span>
- <span data-ttu-id="2ef91-2212">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2212">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2213">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2213">'Razor'</span></span>
- <span data-ttu-id="2ef91-2214">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2214">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2215">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2215">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2216">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2216">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2217">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2217">'Identity'</span></span>
- <span data-ttu-id="2ef91-2218">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2218">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2219">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2219">'Razor'</span></span>
- <span data-ttu-id="2ef91-2220">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2220">'SignalR' uid:</span></span> 

<span data-ttu-id="2ef91-2221">-------: |:----: | |陣列：專案： 0 |value0 | |陣列：專案： 1 |value1 | |陣列：專案： 2 |value2 | |陣列：專案： 4 |value4 | |陣列：專案： 5 |value5 |</span><span class="sxs-lookup"><span data-stu-id="2ef91-2221">-------: | :----: | | array:entries:0 | value0 | | array:entries:1 | value1 | | array:entries:2 | value2 | | array:entries:4 | value4 | | array:entries:5 | value5 |</span></span>

<span data-ttu-id="2ef91-2222">這些機碼與值是使用「記憶體設定提供者」在範例應用程式中載入：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2222">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

<span data-ttu-id="2ef91-2223">陣列會跳過索引 &num;3 的值。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2223">The array skips a value for index &num;3.</span></span> <span data-ttu-id="2ef91-2224">設定繫結程式無法繫結 Null 值或在已繫結的物件中建立 Null 項目，這在示範繫結此陣列的結果到某物件的時候已經很清楚。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2224">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="2ef91-2225">在範例應用程式中，POCO 類別可用來存放繫結設定資料：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2225">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

<span data-ttu-id="2ef91-2226">設定資料已繫結到物件：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2226">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="2ef91-2227">[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*)語法也可以使用，以產生更精簡的程式碼：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2227">[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

<span data-ttu-id="2ef91-2228">繫結物件 (`ArrayExample`的執行個體) 會從設定接收陣列資料。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2228">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="2ef91-2229">`ArrayExample.Entries` 索引</span><span class="sxs-lookup"><span data-stu-id="2ef91-2229">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="2ef91-2230">`ArrayExample.Entries` 值</span><span class="sxs-lookup"><span data-stu-id="2ef91-2230">`ArrayExample.Entries` Value</span></span> |
| :---
<span data-ttu-id="2ef91-2231">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2231">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2232">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2232">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2233">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2233">'Identity'</span></span>
- <span data-ttu-id="2ef91-2234">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2234">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2235">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2235">'Razor'</span></span>
- <span data-ttu-id="2ef91-2236">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2236">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2237">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2237">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2238">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2238">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2239">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2239">'Identity'</span></span>
- <span data-ttu-id="2ef91-2240">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2240">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2241">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2241">'Razor'</span></span>
- <span data-ttu-id="2ef91-2242">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2242">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2243">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2243">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2244">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2244">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2245">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2245">'Identity'</span></span>
- <span data-ttu-id="2ef91-2246">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2246">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2247">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2247">'Razor'</span></span>
- <span data-ttu-id="2ef91-2248">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2248">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2249">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2249">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2250">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2250">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2251">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2251">'Identity'</span></span>
- <span data-ttu-id="2ef91-2252">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2252">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2253">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2253">'Razor'</span></span>
- <span data-ttu-id="2ef91-2254">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2254">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2255">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2255">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2256">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2256">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2257">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2257">'Identity'</span></span>
- <span data-ttu-id="2ef91-2258">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2258">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2259">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2259">'Razor'</span></span>
- <span data-ttu-id="2ef91-2260">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2260">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2261">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2261">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2262">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2262">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2263">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2263">'Identity'</span></span>
- <span data-ttu-id="2ef91-2264">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2264">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2265">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2265">'Razor'</span></span>
- <span data-ttu-id="2ef91-2266">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2266">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2267">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2267">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2268">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2268">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2269">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2269">'Identity'</span></span>
- <span data-ttu-id="2ef91-2270">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2270">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2271">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2271">'Razor'</span></span>
- <span data-ttu-id="2ef91-2272">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2272">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2273">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2273">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2274">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2274">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2275">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2275">'Identity'</span></span>
- <span data-ttu-id="2ef91-2276">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2276">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2277">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2277">'Razor'</span></span>
- <span data-ttu-id="2ef91-2278">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2278">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2279">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2279">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2280">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2280">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2281">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2281">'Identity'</span></span>
- <span data-ttu-id="2ef91-2282">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2282">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2283">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2283">'Razor'</span></span>
- <span data-ttu-id="2ef91-2284">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2284">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2285">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2285">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2286">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2286">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2287">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2287">'Identity'</span></span>
- <span data-ttu-id="2ef91-2288">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2288">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2289">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2289">'Razor'</span></span>
- <span data-ttu-id="2ef91-2290">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2290">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2291">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2291">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2292">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2292">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2293">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2293">'Identity'</span></span>
- <span data-ttu-id="2ef91-2294">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2294">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2295">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2295">'Razor'</span></span>
- <span data-ttu-id="2ef91-2296">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2296">'SignalR' uid:</span></span> 

<span data-ttu-id="2ef91-2297">-------------: |：---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2297">-------------: | :--- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2298">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2298">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2299">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2299">'Identity'</span></span>
- <span data-ttu-id="2ef91-2300">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2300">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2301">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2301">'Razor'</span></span>
- <span data-ttu-id="2ef91-2302">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2302">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2303">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2303">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2304">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2304">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2305">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2305">'Identity'</span></span>
- <span data-ttu-id="2ef91-2306">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2306">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2307">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2307">'Razor'</span></span>
- <span data-ttu-id="2ef91-2308">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2308">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2309">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2309">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2310">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2310">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2311">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2311">'Identity'</span></span>
- <span data-ttu-id="2ef91-2312">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2312">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2313">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2313">'Razor'</span></span>
- <span data-ttu-id="2ef91-2314">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2314">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2315">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2315">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2316">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2316">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2317">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2317">'Identity'</span></span>
- <span data-ttu-id="2ef91-2318">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2318">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2319">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2319">'Razor'</span></span>
- <span data-ttu-id="2ef91-2320">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2320">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2321">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2321">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2322">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2322">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2323">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2323">'Identity'</span></span>
- <span data-ttu-id="2ef91-2324">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2324">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2325">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2325">'Razor'</span></span>
- <span data-ttu-id="2ef91-2326">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2326">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2327">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2327">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2328">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2328">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2329">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2329">'Identity'</span></span>
- <span data-ttu-id="2ef91-2330">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2330">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2331">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2331">'Razor'</span></span>
- <span data-ttu-id="2ef91-2332">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2332">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2333">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2333">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2334">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2334">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2335">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2335">'Identity'</span></span>
- <span data-ttu-id="2ef91-2336">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2336">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2337">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2337">'Razor'</span></span>
- <span data-ttu-id="2ef91-2338">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2338">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2339">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2339">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2340">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2340">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2341">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2341">'Identity'</span></span>
- <span data-ttu-id="2ef91-2342">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2342">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2343">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2343">'Razor'</span></span>
- <span data-ttu-id="2ef91-2344">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2344">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2345">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2345">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2346">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2346">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2347">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2347">'Identity'</span></span>
- <span data-ttu-id="2ef91-2348">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2348">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2349">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2349">'Razor'</span></span>
- <span data-ttu-id="2ef91-2350">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2350">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2351">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2351">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2352">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2352">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2353">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2353">'Identity'</span></span>
- <span data-ttu-id="2ef91-2354">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2354">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2355">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2355">'Razor'</span></span>
- <span data-ttu-id="2ef91-2356">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2356">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2357">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2357">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2358">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2358">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2359">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2359">'Identity'</span></span>
- <span data-ttu-id="2ef91-2360">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2360">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2361">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2361">'Razor'</span></span>
- <span data-ttu-id="2ef91-2362">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2362">'SignalR' uid:</span></span> 

<span data-ttu-id="2ef91-2363">-------------: | |0 |value0 | |1 |value1 | |2 |value2 | |3 |value4 | |4 |value5 |</span><span class="sxs-lookup"><span data-stu-id="2ef91-2363">-------------: | | 0                            | value0                       | | 1                            | value1                       | | 2                            | value2                       | | 3                            | value4                       | | 4                            | value5                       |</span></span>

<span data-ttu-id="2ef91-2364">繫結物件中的索引 &num;3 存放 `array:4` 設定機碼與其 `value4` 的設定資料。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2364">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="2ef91-2365">當繫結包含陣列的設定資料時，設定機碼中的陣列索引只會用來列舉設定資料 (當建立物件時)。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2365">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="2ef91-2366">設定資料中不能保留 Null 值，當設定機碼中的陣列略過一或多個索引時，不會在繫結物件中建立 Null 值項目。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2366">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="2ef91-2367">由會在設定中產生正確機碼值組的任何設定提供者繫結到 `ArrayExample` 執行個體之前，可以提供索引 &num;3 缺少的設定項目。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2367">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="2ef91-2368">若範例包含具有缺少之機碼值組的額外 JSON 設定提供者，`ArrayExample.Entries` 會符合完整的設定陣列：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2368">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="2ef91-2369">*missing_value.json*：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2369">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="2ef91-2370">在 `ConfigureAppConfiguration` 中：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2370">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="2ef91-2371">表格中顯示的機碼值組會載入到設定中。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2371">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="2ef91-2372">Key</span><span class="sxs-lookup"><span data-stu-id="2ef91-2372">Key</span></span>             | <span data-ttu-id="2ef91-2373">值</span><span class="sxs-lookup"><span data-stu-id="2ef91-2373">Value</span></span>  |
| :---
<span data-ttu-id="2ef91-2374">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2374">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2375">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2375">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2376">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2376">'Identity'</span></span>
- <span data-ttu-id="2ef91-2377">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2377">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2378">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2378">'Razor'</span></span>
- <span data-ttu-id="2ef91-2379">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2379">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2380">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2380">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2381">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2381">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2382">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2382">'Identity'</span></span>
- <span data-ttu-id="2ef91-2383">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2383">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2384">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2384">'Razor'</span></span>
- <span data-ttu-id="2ef91-2385">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2385">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2386">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2386">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2387">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2387">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2388">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2388">'Identity'</span></span>
- <span data-ttu-id="2ef91-2389">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2389">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2390">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2390">'Razor'</span></span>
- <span data-ttu-id="2ef91-2391">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2391">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2392">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2392">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2393">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2393">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2394">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2394">'Identity'</span></span>
- <span data-ttu-id="2ef91-2395">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2395">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2396">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2396">'Razor'</span></span>
- <span data-ttu-id="2ef91-2397">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2397">'SignalR' uid:</span></span> 

<span data-ttu-id="2ef91-2398">-------: |:----: | |陣列：專案： 3 |value3 |</span><span class="sxs-lookup"><span data-stu-id="2ef91-2398">-------: | :----: | | array:entries:3 | value3 |</span></span>

<span data-ttu-id="2ef91-2399">在「JSON 設定提供者」包含索引 &num;3 的項目之後，若繫結 `ArrayExample` 類別執行個體，`ArrayExample.Entries` 陣列會包括這些值：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2399">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="2ef91-2400">`ArrayExample.Entries` 索引</span><span class="sxs-lookup"><span data-stu-id="2ef91-2400">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="2ef91-2401">`ArrayExample.Entries` 值</span><span class="sxs-lookup"><span data-stu-id="2ef91-2401">`ArrayExample.Entries` Value</span></span> |
| :---
<span data-ttu-id="2ef91-2402">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2402">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2403">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2403">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2404">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2404">'Identity'</span></span>
- <span data-ttu-id="2ef91-2405">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2405">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2406">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2406">'Razor'</span></span>
- <span data-ttu-id="2ef91-2407">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2407">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2408">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2408">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2409">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2409">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2410">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2410">'Identity'</span></span>
- <span data-ttu-id="2ef91-2411">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2411">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2412">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2412">'Razor'</span></span>
- <span data-ttu-id="2ef91-2413">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2413">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2414">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2414">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2415">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2415">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2416">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2416">'Identity'</span></span>
- <span data-ttu-id="2ef91-2417">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2417">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2418">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2418">'Razor'</span></span>
- <span data-ttu-id="2ef91-2419">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2419">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2420">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2420">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2421">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2421">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2422">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2422">'Identity'</span></span>
- <span data-ttu-id="2ef91-2423">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2423">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2424">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2424">'Razor'</span></span>
- <span data-ttu-id="2ef91-2425">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2425">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2426">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2426">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2427">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2427">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2428">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2428">'Identity'</span></span>
- <span data-ttu-id="2ef91-2429">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2429">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2430">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2430">'Razor'</span></span>
- <span data-ttu-id="2ef91-2431">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2431">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2432">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2432">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2433">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2433">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2434">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2434">'Identity'</span></span>
- <span data-ttu-id="2ef91-2435">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2435">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2436">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2436">'Razor'</span></span>
- <span data-ttu-id="2ef91-2437">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2437">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2438">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2438">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2439">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2439">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2440">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2440">'Identity'</span></span>
- <span data-ttu-id="2ef91-2441">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2441">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2442">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2442">'Razor'</span></span>
- <span data-ttu-id="2ef91-2443">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2443">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2444">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2444">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2445">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2445">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2446">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2446">'Identity'</span></span>
- <span data-ttu-id="2ef91-2447">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2447">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2448">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2448">'Razor'</span></span>
- <span data-ttu-id="2ef91-2449">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2449">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2450">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2450">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2451">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2451">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2452">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2452">'Identity'</span></span>
- <span data-ttu-id="2ef91-2453">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2453">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2454">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2454">'Razor'</span></span>
- <span data-ttu-id="2ef91-2455">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2455">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2456">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2456">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2457">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2457">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2458">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2458">'Identity'</span></span>
- <span data-ttu-id="2ef91-2459">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2459">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2460">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2460">'Razor'</span></span>
- <span data-ttu-id="2ef91-2461">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2461">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2462">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2462">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2463">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2463">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2464">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2464">'Identity'</span></span>
- <span data-ttu-id="2ef91-2465">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2465">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2466">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2466">'Razor'</span></span>
- <span data-ttu-id="2ef91-2467">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2467">'SignalR' uid:</span></span> 

<span data-ttu-id="2ef91-2468">-------------: |：---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2468">-------------: | :--- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2469">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2469">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2470">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2470">'Identity'</span></span>
- <span data-ttu-id="2ef91-2471">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2471">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2472">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2472">'Razor'</span></span>
- <span data-ttu-id="2ef91-2473">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2473">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2474">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2474">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2475">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2475">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2476">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2476">'Identity'</span></span>
- <span data-ttu-id="2ef91-2477">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2477">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2478">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2478">'Razor'</span></span>
- <span data-ttu-id="2ef91-2479">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2479">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2480">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2480">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2481">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2481">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2482">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2482">'Identity'</span></span>
- <span data-ttu-id="2ef91-2483">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2483">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2484">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2484">'Razor'</span></span>
- <span data-ttu-id="2ef91-2485">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2485">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2486">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2486">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2487">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2487">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2488">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2488">'Identity'</span></span>
- <span data-ttu-id="2ef91-2489">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2489">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2490">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2490">'Razor'</span></span>
- <span data-ttu-id="2ef91-2491">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2491">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2492">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2492">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2493">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2493">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2494">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2494">'Identity'</span></span>
- <span data-ttu-id="2ef91-2495">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2495">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2496">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2496">'Razor'</span></span>
- <span data-ttu-id="2ef91-2497">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2497">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2498">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2498">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2499">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2499">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2500">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2500">'Identity'</span></span>
- <span data-ttu-id="2ef91-2501">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2501">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2502">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2502">'Razor'</span></span>
- <span data-ttu-id="2ef91-2503">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2503">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2504">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2504">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2505">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2505">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2506">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2506">'Identity'</span></span>
- <span data-ttu-id="2ef91-2507">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2507">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2508">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2508">'Razor'</span></span>
- <span data-ttu-id="2ef91-2509">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2509">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2510">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2510">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2511">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2511">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2512">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2512">'Identity'</span></span>
- <span data-ttu-id="2ef91-2513">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2513">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2514">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2514">'Razor'</span></span>
- <span data-ttu-id="2ef91-2515">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2515">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2516">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2516">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2517">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2517">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2518">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2518">'Identity'</span></span>
- <span data-ttu-id="2ef91-2519">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2519">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2520">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2520">'Razor'</span></span>
- <span data-ttu-id="2ef91-2521">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2521">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2522">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2522">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2523">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2523">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2524">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2524">'Identity'</span></span>
- <span data-ttu-id="2ef91-2525">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2525">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2526">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2526">'Razor'</span></span>
- <span data-ttu-id="2ef91-2527">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2527">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2528">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2528">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2529">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2529">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2530">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2530">'Identity'</span></span>
- <span data-ttu-id="2ef91-2531">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2531">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2532">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2532">'Razor'</span></span>
- <span data-ttu-id="2ef91-2533">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2533">'SignalR' uid:</span></span> 

<span data-ttu-id="2ef91-2534">-------------: | |0 |value0 | |1 |value1 | |2 |value2 | |3 |value3 | |4 |value4 | |5 |value5 |</span><span class="sxs-lookup"><span data-stu-id="2ef91-2534">-------------: | | 0                            | value0                       | | 1                            | value1                       | | 2                            | value2                       | | 3                            | value3                       | | 4                            | value4                       | | 5                            | value5                       |</span></span>

<span data-ttu-id="2ef91-2535">**JSON 陣列處理**</span><span class="sxs-lookup"><span data-stu-id="2ef91-2535">**JSON array processing**</span></span>

<span data-ttu-id="2ef91-2536">若 JSON 檔案包含陣列，會為具有以零為基礎之區段索引的陣列元素建立設定機碼。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2536">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="2ef91-2537">在下列設定檔中，`subsection` 是陣列：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2537">In the following configuration file, `subsection` is an array:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

<span data-ttu-id="2ef91-2538">「JSON 設定提供者」會將設定資料讀入到下列機碼值組：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2538">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="2ef91-2539">Key</span><span class="sxs-lookup"><span data-stu-id="2ef91-2539">Key</span></span>                     | <span data-ttu-id="2ef91-2540">值</span><span class="sxs-lookup"><span data-stu-id="2ef91-2540">Value</span></span>  |
| ---
<span data-ttu-id="2ef91-2541">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2541">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2542">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2542">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2543">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2543">'Identity'</span></span>
- <span data-ttu-id="2ef91-2544">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2544">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2545">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2545">'Razor'</span></span>
- <span data-ttu-id="2ef91-2546">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2546">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2547">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2547">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2548">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2548">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2549">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2549">'Identity'</span></span>
- <span data-ttu-id="2ef91-2550">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2550">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2551">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2551">'Razor'</span></span>
- <span data-ttu-id="2ef91-2552">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2552">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2553">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2553">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2554">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2554">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2555">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2555">'Identity'</span></span>
- <span data-ttu-id="2ef91-2556">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2556">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2557">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2557">'Razor'</span></span>
- <span data-ttu-id="2ef91-2558">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2558">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2559">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2559">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2560">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2560">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2561">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2561">'Identity'</span></span>
- <span data-ttu-id="2ef91-2562">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2562">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2563">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2563">'Razor'</span></span>
- <span data-ttu-id="2ef91-2564">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2564">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2565">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2565">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2566">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2566">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2567">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2567">'Identity'</span></span>
- <span data-ttu-id="2ef91-2568">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2568">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2569">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2569">'Razor'</span></span>
- <span data-ttu-id="2ef91-2570">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2570">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2571">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2571">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2572">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2572">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2573">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2573">'Identity'</span></span>
- <span data-ttu-id="2ef91-2574">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2574">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2575">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2575">'Razor'</span></span>
- <span data-ttu-id="2ef91-2576">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2576">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2577">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2577">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2578">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2578">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2579">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2579">'Identity'</span></span>
- <span data-ttu-id="2ef91-2580">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2580">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2581">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2581">'Razor'</span></span>
- <span data-ttu-id="2ef91-2582">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2582">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2583">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2583">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2584">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2584">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2585">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2585">'Identity'</span></span>
- <span data-ttu-id="2ef91-2586">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2586">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2587">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2587">'Razor'</span></span>
- <span data-ttu-id="2ef91-2588">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2588">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2589">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2589">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2590">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2590">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2591">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2591">'Identity'</span></span>
- <span data-ttu-id="2ef91-2592">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2592">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2593">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2593">'Razor'</span></span>
- <span data-ttu-id="2ef91-2594">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2594">'SignalR' uid:</span></span> 

<span data-ttu-id="2ef91-2595">------------ |:----: | |json_array：索引鍵 |valueA | |json_array：小節： 0 |valueB | |json_array：小節： 1 |Valuec&parameter2 | |json_array：小節： 2 |值 |</span><span class="sxs-lookup"><span data-stu-id="2ef91-2595">------------ | :----: | | json_array:key          | valueA | | json_array:subsection:0 | valueB | | json_array:subsection:1 | valueC | | json_array:subsection:2 | valueD |</span></span>

<span data-ttu-id="2ef91-2596">在範例應用程式中，下列 POCO 類別可用來繫結設定機碼值組：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2596">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

<span data-ttu-id="2ef91-2597">在繫結之後，`JsonArrayExample.Key` 會存放值 `valueA`。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2597">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="2ef91-2598">子區段值會存放在 POCO 陣列屬性 `Subsection` 中。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2598">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="2ef91-2599">`JsonArrayExample.Subsection` 索引</span><span class="sxs-lookup"><span data-stu-id="2ef91-2599">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="2ef91-2600">`JsonArrayExample.Subsection` 值</span><span class="sxs-lookup"><span data-stu-id="2ef91-2600">`JsonArrayExample.Subsection` Value</span></span> |
| :---
<span data-ttu-id="2ef91-2601">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2601">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2602">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2602">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2603">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2603">'Identity'</span></span>
- <span data-ttu-id="2ef91-2604">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2604">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2605">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2605">'Razor'</span></span>
- <span data-ttu-id="2ef91-2606">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2606">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2607">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2607">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2608">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2608">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2609">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2609">'Identity'</span></span>
- <span data-ttu-id="2ef91-2610">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2610">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2611">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2611">'Razor'</span></span>
- <span data-ttu-id="2ef91-2612">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2612">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2613">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2613">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2614">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2614">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2615">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2615">'Identity'</span></span>
- <span data-ttu-id="2ef91-2616">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2616">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2617">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2617">'Razor'</span></span>
- <span data-ttu-id="2ef91-2618">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2618">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2619">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2619">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2620">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2620">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2621">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2621">'Identity'</span></span>
- <span data-ttu-id="2ef91-2622">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2622">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2623">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2623">'Razor'</span></span>
- <span data-ttu-id="2ef91-2624">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2624">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2625">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2625">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2626">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2626">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2627">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2627">'Identity'</span></span>
- <span data-ttu-id="2ef91-2628">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2628">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2629">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2629">'Razor'</span></span>
- <span data-ttu-id="2ef91-2630">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2630">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2631">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2631">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2632">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2632">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2633">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2633">'Identity'</span></span>
- <span data-ttu-id="2ef91-2634">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2634">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2635">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2635">'Razor'</span></span>
- <span data-ttu-id="2ef91-2636">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2636">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2637">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2637">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2638">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2638">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2639">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2639">'Identity'</span></span>
- <span data-ttu-id="2ef91-2640">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2640">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2641">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2641">'Razor'</span></span>
- <span data-ttu-id="2ef91-2642">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2642">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2643">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2643">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2644">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2644">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2645">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2645">'Identity'</span></span>
- <span data-ttu-id="2ef91-2646">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2646">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2647">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2647">'Razor'</span></span>
- <span data-ttu-id="2ef91-2648">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2648">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2649">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2649">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2650">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2650">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2651">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2651">'Identity'</span></span>
- <span data-ttu-id="2ef91-2652">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2652">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2653">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2653">'Razor'</span></span>
- <span data-ttu-id="2ef91-2654">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2654">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2655">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2655">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2656">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2656">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2657">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2657">'Identity'</span></span>
- <span data-ttu-id="2ef91-2658">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2658">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2659">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2659">'Razor'</span></span>
- <span data-ttu-id="2ef91-2660">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2660">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2661">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2661">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2662">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2662">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2663">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2663">'Identity'</span></span>
- <span data-ttu-id="2ef91-2664">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2664">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2665">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2665">'Razor'</span></span>
- <span data-ttu-id="2ef91-2666">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2666">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2667">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2667">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2668">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2668">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2669">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2669">'Identity'</span></span>
- <span data-ttu-id="2ef91-2670">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2670">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2671">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2671">'Razor'</span></span>
- <span data-ttu-id="2ef91-2672">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2672">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2673">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2673">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2674">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2674">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2675">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2675">'Identity'</span></span>
- <span data-ttu-id="2ef91-2676">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2676">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2677">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2677">'Razor'</span></span>
- <span data-ttu-id="2ef91-2678">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2678">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2679">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2679">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2680">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2680">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2681">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2681">'Identity'</span></span>
- <span data-ttu-id="2ef91-2682">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2682">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2683">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2683">'Razor'</span></span>
- <span data-ttu-id="2ef91-2684">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2684">'SignalR' uid:</span></span> 

<span data-ttu-id="2ef91-2685">-----------------: |：---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2685">-----------------: | :--- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2686">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2686">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2687">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2687">'Identity'</span></span>
- <span data-ttu-id="2ef91-2688">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2688">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2689">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2689">'Razor'</span></span>
- <span data-ttu-id="2ef91-2690">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2690">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2691">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2691">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2692">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2692">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2693">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2693">'Identity'</span></span>
- <span data-ttu-id="2ef91-2694">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2694">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2695">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2695">'Razor'</span></span>
- <span data-ttu-id="2ef91-2696">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2696">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2697">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2697">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2698">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2698">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2699">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2699">'Identity'</span></span>
- <span data-ttu-id="2ef91-2700">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2700">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2701">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2701">'Razor'</span></span>
- <span data-ttu-id="2ef91-2702">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2702">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2703">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2703">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2704">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2704">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2705">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2705">'Identity'</span></span>
- <span data-ttu-id="2ef91-2706">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2706">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2707">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2707">'Razor'</span></span>
- <span data-ttu-id="2ef91-2708">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2708">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2709">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2709">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2710">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2710">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2711">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2711">'Identity'</span></span>
- <span data-ttu-id="2ef91-2712">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2712">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2713">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2713">'Razor'</span></span>
- <span data-ttu-id="2ef91-2714">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2714">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2715">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2715">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2716">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2716">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2717">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2717">'Identity'</span></span>
- <span data-ttu-id="2ef91-2718">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2718">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2719">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2719">'Razor'</span></span>
- <span data-ttu-id="2ef91-2720">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2720">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2721">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2721">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2722">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2722">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2723">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2723">'Identity'</span></span>
- <span data-ttu-id="2ef91-2724">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2724">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2725">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2725">'Razor'</span></span>
- <span data-ttu-id="2ef91-2726">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2726">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2727">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2727">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2728">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2728">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2729">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2729">'Identity'</span></span>
- <span data-ttu-id="2ef91-2730">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2730">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2731">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2731">'Razor'</span></span>
- <span data-ttu-id="2ef91-2732">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2732">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2733">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2733">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2734">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2734">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2735">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2735">'Identity'</span></span>
- <span data-ttu-id="2ef91-2736">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2736">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2737">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2737">'Razor'</span></span>
- <span data-ttu-id="2ef91-2738">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2738">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2739">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2739">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2740">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2740">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2741">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2741">'Identity'</span></span>
- <span data-ttu-id="2ef91-2742">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2742">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2743">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2743">'Razor'</span></span>
- <span data-ttu-id="2ef91-2744">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2744">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2745">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2745">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2746">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2746">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2747">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2747">'Identity'</span></span>
- <span data-ttu-id="2ef91-2748">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2748">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2749">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2749">'Razor'</span></span>
- <span data-ttu-id="2ef91-2750">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2750">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2751">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2751">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2752">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2752">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2753">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2753">'Identity'</span></span>
- <span data-ttu-id="2ef91-2754">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2754">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2755">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2755">'Razor'</span></span>
- <span data-ttu-id="2ef91-2756">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2756">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2757">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2757">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2758">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2758">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2759">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2759">'Identity'</span></span>
- <span data-ttu-id="2ef91-2760">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2760">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2761">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2761">'Razor'</span></span>
- <span data-ttu-id="2ef91-2762">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2762">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2ef91-2763">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2763">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2ef91-2764">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2764">'Blazor'</span></span>
- <span data-ttu-id="2ef91-2765">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2765">'Identity'</span></span>
- <span data-ttu-id="2ef91-2766">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2766">'Let's Encrypt'</span></span>
- <span data-ttu-id="2ef91-2767">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2ef91-2767">'Razor'</span></span>
- <span data-ttu-id="2ef91-2768">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2768">'SignalR' uid:</span></span> 

<span data-ttu-id="2ef91-2769">-----------------: | |0 |valueB | |1 |Valuec&parameter2 | |2 |值 |</span><span class="sxs-lookup"><span data-stu-id="2ef91-2769">-----------------: | | 0                                   | valueB                              | | 1                                   | valueC                              | | 2                                   | valueD                              |</span></span>

## <a name="custom-configuration-provider"></a><span data-ttu-id="2ef91-2770">自訂設定提供者</span><span class="sxs-lookup"><span data-stu-id="2ef91-2770">Custom configuration provider</span></span>

<span data-ttu-id="2ef91-2771">範例應用程式示範如何建立會使用 [Entity Framework (EF)](/ef/core/) 從資料庫讀取設定機碼值組的基本設定提供者。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2771">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="2ef91-2772">提供者具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2772">The provider has the following characteristics:</span></span>

* <span data-ttu-id="2ef91-2773">EF 記憶體內資料庫會用於示範用途。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2773">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="2ef91-2774">若要使用需要連接字串的資料庫，請實作第二個 `ConfigurationBuilder` 以從另一個設定提供者提供連接字串。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2774">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="2ef91-2775">啟動時，該提供者會將資料庫資料表讀入到設定中。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2775">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="2ef91-2776">該提供者不會以個別機碼為基礎查詢資料庫。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2776">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="2ef91-2777">未實作變更時重新載入，因此在應用程式啟動後更新資料庫對應用程式設定沒有影響。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2777">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="2ef91-2778">定義 `EFConfigurationValue` 實體來在資料庫中存放設定值。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2778">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="2ef91-2779">*Models/EFConfigurationValue.cs*：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2779">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="2ef91-2780">新增 `EFConfigurationContext` 以存放及存取已設定的值。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2780">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="2ef91-2781">*EFConfigurationProvider/EFConfigurationContext.cs*：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2781">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="2ef91-2782">建立會實作 <xref:Microsoft.Extensions.Configuration.IConfigurationSource> 的類別。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2782">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="2ef91-2783">*EFConfigurationProvider/EFConfigurationSource.cs*：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2783">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="2ef91-2784">透過繼承自 <xref:Microsoft.Extensions.Configuration.ConfigurationProvider> 來建立自訂設定提供者。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2784">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="2ef91-2785">若資料庫是空的，設定提供者會初始化資料庫。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2785">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="2ef91-2786">*EFConfigurationProvider/EFConfigurationProvider.cs*：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2786">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="2ef91-2787">`AddEFConfiguration` 擴充方法允許新增設定來源到 `ConfigurationBuilder`。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2787">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="2ef91-2788">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="2ef91-2788">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="2ef91-2789">下列程式碼顯示如何在 *Program.cs* 中使用自訂 `EFConfigurationProvider`：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2789">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

## <a name="access-configuration-during-startup"></a><span data-ttu-id="2ef91-2790">在啟動期間存取組態</span><span class="sxs-lookup"><span data-stu-id="2ef91-2790">Access configuration during startup</span></span>

<span data-ttu-id="2ef91-2791">將 `IConfiguration` 插入到 `Startup` 建構函式，以存取 `Startup.ConfigureServices` 中的設定值。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2791">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="2ef91-2792">若要存取 `Startup.Configure` 中的設定，請直接將 `IConfiguration` 插入到方法或從建構函式使用執行個體：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2792">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

```csharp
public class Startup
{
    private readonly IConfiguration _config;

    public Startup(IConfiguration config)
    {
        _config = config;
    }

    public void ConfigureServices(IServiceCollection services)
    {
        var value = _config["key"];
    }

    public void Configure(IApplicationBuilder app, IConfiguration config)
    {
        var value = config["key"];
    }
}
```

<span data-ttu-id="2ef91-2793">如需使用啟動方便方法來存取設定的範例，請參閱[應用程式啟動：方便方法](xref:fundamentals/startup#convenience-methods)。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2793">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="2ef91-2794">Razor頁面頁面或 MVC 視圖中的存取設定</span><span class="sxs-lookup"><span data-stu-id="2ef91-2794">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="2ef91-2795">若要存取 Razor [頁面] 頁面或 MVC 視圖中的設定，請在 [設定[命名空間](xref:Microsoft.Extensions.Configuration)] 中新增[using](xref:mvc/views/razor#using)指示詞（[c # 參考： using](/dotnet/csharp/language-reference/keywords/using-directive)指示詞），並將其插入 <xref:Microsoft.Extensions.Configuration.IConfiguration> 頁面或視圖中。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2795">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="2ef91-2796">在 [ Razor 頁面] 頁面中：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2796">In a Razor Pages page:</span></span>

```cshtml
@page
@model IndexModel
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index Page</title>
</head>
<body>
    <h1>Access configuration in a Razor Pages page</h1>
    <p>Configuration value for 'key': @Configuration["key"]</p>
</body>
</html>
```

<span data-ttu-id="2ef91-2797">在 MVC 檢視中：</span><span class="sxs-lookup"><span data-stu-id="2ef91-2797">In an MVC view:</span></span>

```cshtml
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index View</title>
</head>
<body>
    <h1>Access configuration in an MVC view</h1>
    <p>Configuration value for 'key': @Configuration["key"]</p>
</body>
</html>
```

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="2ef91-2798">從外部組件新增設定</span><span class="sxs-lookup"><span data-stu-id="2ef91-2798">Add configuration from an external assembly</span></span>

<span data-ttu-id="2ef91-2799"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup> 實作允許在啟動時從應用程式 `Startup` 類別外部的外部組件，針對應用程式新增增強功能。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2799">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="2ef91-2800">如需詳細資訊，請參閱<xref:fundamentals/configuration/platform-specific-configuration>。</span><span class="sxs-lookup"><span data-stu-id="2ef91-2800">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2ef91-2801">其他資源</span><span class="sxs-lookup"><span data-stu-id="2ef91-2801">Additional resources</span></span>

* <xref:fundamentals/configuration/options>

::: moniker-end
