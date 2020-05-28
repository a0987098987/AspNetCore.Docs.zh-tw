---
<span data-ttu-id="0b2c8-101">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="0b2c8-101">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="0b2c8-102">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="0b2c8-102">'Blazor'</span></span>
- <span data-ttu-id="0b2c8-103">'Identity'</span><span class="sxs-lookup"><span data-stu-id="0b2c8-103">'Identity'</span></span>
- <span data-ttu-id="0b2c8-104">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="0b2c8-104">'Let's Encrypt'</span></span>
- <span data-ttu-id="0b2c8-105">'Razor'</span><span class="sxs-lookup"><span data-stu-id="0b2c8-105">'Razor'</span></span>
- <span data-ttu-id="0b2c8-106">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="0b2c8-106">'SignalR' uid:</span></span> 

---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="0b2c8-107">Visual Studio for ASP.NET Core 中的開發階段 IIS 支援</span><span class="sxs-lookup"><span data-stu-id="0b2c8-107">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="0b2c8-108">由 [Sourabh Shirhatti](https://twitter.com/sshirhatti) 提供</span><span class="sxs-lookup"><span data-stu-id="0b2c8-108">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="0b2c8-109">本文說明針對在 Windows Server 上搭配 IIS 執行的 ASP.NET Core 應用程式所提供的 [Visual Studio](https://visualstudio.microsoft.com) 偵錯支援。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-109">This article describes [Visual Studio](https://visualstudio.microsoft.com) support for debugging ASP.NET Core apps running with IIS on Windows Server.</span></span> <span data-ttu-id="0b2c8-110">本主題會逐步解說如何啟用此案例及設定專案。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-110">This topic walks through enabling this scenario and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0b2c8-111">必要條件</span><span class="sxs-lookup"><span data-stu-id="0b2c8-111">Prerequisites</span></span>

* [<span data-ttu-id="0b2c8-112">適用于 Windows 的 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0b2c8-112">Visual Studio for Windows</span></span>](https://visualstudio.microsoft.com/downloads/)
* <span data-ttu-id="0b2c8-113">**ASP.NET 與網頁程式開發**工作負載</span><span class="sxs-lookup"><span data-stu-id="0b2c8-113">**ASP.NET and web development** workload</span></span>
* <span data-ttu-id="0b2c8-114">**.NET Core 跨平台開發**工作負載</span><span class="sxs-lookup"><span data-stu-id="0b2c8-114">**.NET Core cross-platform development** workload</span></span>
* <span data-ttu-id="0b2c8-115">X.509 安全性憑證 (以取得 HTTPS 支援)</span><span class="sxs-lookup"><span data-stu-id="0b2c8-115">X.509 security certificate (for HTTPS support)</span></span>

## <a name="enable-iis"></a><span data-ttu-id="0b2c8-116">啟用 IIS</span><span class="sxs-lookup"><span data-stu-id="0b2c8-116">Enable IIS</span></span>

1. <span data-ttu-id="0b2c8-117">在 Windows 中，瀏覽至 [控制台]**[程式]** > **[程式和功能]** > **[開啟或關閉 Windows 功能]** > \*\*\*\* (畫面左側)。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-117">In Windows, navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="0b2c8-118">選取 [Internet Information Services]\*\*\*\* 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-118">Select the **Internet Information Services** check box.</span></span> <span data-ttu-id="0b2c8-119">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-119">Select **OK**.</span></span>

<span data-ttu-id="0b2c8-120">安裝 IIS 可能需要重新啟動系統。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-120">The IIS installation may require a system restart.</span></span>

## <a name="configure-iis"></a><span data-ttu-id="0b2c8-121">設定 IIS</span><span class="sxs-lookup"><span data-stu-id="0b2c8-121">Configure IIS</span></span>

<span data-ttu-id="0b2c8-122">IIS 的網站必須含有下列設定：</span><span class="sxs-lookup"><span data-stu-id="0b2c8-122">IIS must have a website configured with the following:</span></span>

* <span data-ttu-id="0b2c8-123">**主機名稱**：通常，**預設的網站**會與**主機名稱**搭配使用 `localhost` 。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-123">**Host name**: Typically, the **Default Web Site** is used with a **Host name** of `localhost`.</span></span> <span data-ttu-id="0b2c8-124">不過，任何具有唯一主機名稱的有效 IIS 網站皆適用。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-124">However, any valid IIS website with a unique host name works.</span></span>
* <span data-ttu-id="0b2c8-125">**網站繫結**</span><span class="sxs-lookup"><span data-stu-id="0b2c8-125">**Site Binding**</span></span>
  * <span data-ttu-id="0b2c8-126">針對需要 HTTPS 的應用程式，請搭配憑證針對連接埠 443 建立繫結。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-126">For apps that require HTTPS, create a binding to port 443 with a certificate.</span></span> <span data-ttu-id="0b2c8-127">通常會使用 [IIS Express 開發憑證]\*\*\*\*，但可使用任何有效的憑證。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-127">Typically, the **IIS Express Development Certificate** is used, but any valid certificate works.</span></span>
  * <span data-ttu-id="0b2c8-128">針對使用 HTTP 的應用程式，請確認已存在針對連接埠 80 的繫結，或是為新網站建立針對連接埠 80 的繫結。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-128">For apps that use HTTP, confirm the existence of a binding to post 80 or create a binding to port 80 for a new site.</span></span>
  * <span data-ttu-id="0b2c8-129">需針對 HTTP 或 HTTPS 使用單一繫結。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-129">Use a single binding for either HTTP or HTTPS.</span></span> <span data-ttu-id="0b2c8-130">**不支援同時針對 HTTP 和 HTTPS 連接埠的繫結。**</span><span class="sxs-lookup"><span data-stu-id="0b2c8-130">**Binding to both HTTP and HTTPS ports simultaneously isn't supported.**</span></span>

## <a name="enable-development-time-iis-support-in-visual-studio"></a><span data-ttu-id="0b2c8-131">在 Visual Studio 中啟用開發階段 IIS 支援</span><span class="sxs-lookup"><span data-stu-id="0b2c8-131">Enable development-time IIS support in Visual Studio</span></span>

1. <span data-ttu-id="0b2c8-132">啟動 Visual Studio 安裝程式。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-132">Launch the Visual Studio installer.</span></span>
1. <span data-ttu-id="0b2c8-133">針對您打算用於 IIS 開發階段支援的 Visual Studio 安裝，選取 [修改]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-133">Select **Modify** for the Visual Studio installation that you plan to use for IIS development-time support.</span></span>
1. <span data-ttu-id="0b2c8-134">針對 [ASP.NET 與網頁程式開發]\*\*\*\* 工作負載，尋找並安裝 [開發時間的 IIS 支援]\*\*\*\* 元件。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-134">For the **ASP.NET and web development** workload, locate and install the **Development time IIS support** component.</span></span>

   <span data-ttu-id="0b2c8-135">該元件會列於工作負載右側 [安裝詳細資料]\*\*\*\* 中 [開發時間的 IIS 支援]\*\*\*\* 底下的 [選擇性]\*\*\*\* 區段中。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-135">The component is listed in the **Optional** section under **Development time IIS support** in the **Installation details** panel to the right of the workloads.</span></span> <span data-ttu-id="0b2c8-136">此元件會安裝 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)，這是搭配 IIS 執行 ASP.NET Core 應用程式所需的原生 IIS 模組。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-136">The component installs the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps with IIS.</span></span>

## <a name="configure-the-project"></a><span data-ttu-id="0b2c8-137">設定專案</span><span class="sxs-lookup"><span data-stu-id="0b2c8-137">Configure the project</span></span>

### <a name="https-redirection"></a><span data-ttu-id="0b2c8-138">HTTPS 重新導向</span><span class="sxs-lookup"><span data-stu-id="0b2c8-138">HTTPS redirection</span></span>

<span data-ttu-id="0b2c8-139">針對需要 HTTPS 的新專案，請在 [建立新的 ASP.NET Core Web 應用程式]\*\*\*\* 視窗中，選取 [針對 HTTPS 進行設定]\*\*\*\* 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-139">For a new project that requires HTTPS, select the check box to **Configure for HTTPS** in the **Create a new ASP.NET Core Web Application** window.</span></span> <span data-ttu-id="0b2c8-140">選取該核取方塊會在建立應用程式時，將 [HTTPS 重新導向和 HSTS 中介軟體](xref:security/enforcing-ssl)加入該應用程式。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-140">Selecting the check box adds [HTTPS Redirection and HSTS Middleware](xref:security/enforcing-ssl) to the app when it's created.</span></span>

<span data-ttu-id="0b2c8-141">針對需要 HTTPS 的現有專案，請使用 `Startup.Configure`中的 HTTPS 重新導向和 HSTS 中介軟體。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-141">For an existing project that requires HTTPS, use HTTPS Redirection and HSTS Middleware in `Startup.Configure`.</span></span> <span data-ttu-id="0b2c8-142">如需詳細資訊，請參閱<xref:security/enforcing-ssl>。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-142">For more information, see <xref:security/enforcing-ssl>.</span></span>

<span data-ttu-id="0b2c8-143">針對使用 HTTP 的專案，[HTTPS 重新導向和 HSTS 中介軟體](xref:security/enforcing-ssl)並不會被加入至應用程式。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-143">For a project that uses HTTP, [HTTPS Redirection and HSTS Middleware](xref:security/enforcing-ssl) aren't added to the app.</span></span> <span data-ttu-id="0b2c8-144">您不需要進行任何應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-144">No app configuration is required.</span></span>

### <a name="iis-launch-profile"></a><span data-ttu-id="0b2c8-145">IIS 啟動設定檔</span><span class="sxs-lookup"><span data-stu-id="0b2c8-145">IIS launch profile</span></span>

<span data-ttu-id="0b2c8-146">建立新的啟動設定檔，以新增開發階段 IIS 支援：</span><span class="sxs-lookup"><span data-stu-id="0b2c8-146">Create a new launch profile to add development-time IIS support:</span></span>

1. <span data-ttu-id="0b2c8-147">以滑鼠右鍵按一下 [方案總管]\*\*\*\* 中的專案。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-147">Right-click the project in **Solution Explorer**.</span></span> <span data-ttu-id="0b2c8-148">選取 [屬性]  。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-148">Select **Properties**.</span></span> <span data-ttu-id="0b2c8-149">開啟 [**調試**] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-149">Open the **Debug** tab.</span></span>
1. <span data-ttu-id="0b2c8-150">針對 [設定檔]\*\*\*\*，選取 [新增]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-150">For **Profile**, select the **New** button.</span></span> <span data-ttu-id="0b2c8-151">在快顯示窗中，將設定檔命名為 "IIS"。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-151">Name the profile "IIS" in the popup window.</span></span> <span data-ttu-id="0b2c8-152">選取 [確定]\*\*\*\* 以建立設定檔。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-152">Select **OK** to create the profile.</span></span>
1. <span data-ttu-id="0b2c8-153">針對 [啟動]\*\*\*\* 設定，從清單中選取 [IIS]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-153">For the **Launch** setting, select **IIS** from the list.</span></span>
1. <span data-ttu-id="0b2c8-154">選取 [啟動瀏覽器]\*\*\*\* 的核取方塊並提供端點 URL。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-154">Select the check box for **Launch browser** and provide the endpoint URL.</span></span>

   <span data-ttu-id="0b2c8-155">當應用程式需要 HTTPS 時，請使用 HTTPS 端點 (`https://`)。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-155">When the app requires HTTPS, use an HTTPS endpoint (`https://`).</span></span> <span data-ttu-id="0b2c8-156">針對 HTTP，請使用 HTTP (`http://`) 端點。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-156">For HTTP, use an HTTP (`http://`) endpoint.</span></span>

   <span data-ttu-id="0b2c8-157">提供相同的主機名稱和連接埠作為 [IIS 設定所指定的早期用途](#configure-iis)，其通常是 `localhost`。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-157">Provide the same host name and port as the [IIS configuration specified earlier uses](#configure-iis), typically `localhost`.</span></span>

   <span data-ttu-id="0b2c8-158">在 URL 結尾提供應用程式的名稱。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-158">Provide the name of the app at the end of the URL.</span></span>

   <span data-ttu-id="0b2c8-159">例如，`https://localhost/WebApplication1` (HTTPS) 或 `http://localhost/WebApplication1` (HTTP) 皆為有效的端點 URL。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-159">For example, `https://localhost/WebApplication1` (HTTPS) or `http://localhost/WebApplication1` (HTTP) are valid endpoint URLs.</span></span>
1. <span data-ttu-id="0b2c8-160">在 [環境變數]\*\*\*\* 區段中，選取 [新增]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-160">In the **Environment variables** section, select the **Add** button.</span></span> <span data-ttu-id="0b2c8-161">提供 [名稱]\*\*\*\* 為 `ASPNETCORE_ENVIRONMENT` 且 [值]\*\*\*\* 為 `Development` 的環境變數。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-161">Provide an environment variable with a **Name** of `ASPNETCORE_ENVIRONMENT` and a **Value** of `Development`.</span></span>
1. <span data-ttu-id="0b2c8-162">在 [Web 伺服器設定]\*\*\*\* 區域中，將 [應用程式 URL]\*\*\*\* 設定為用於 [啟動瀏覽器]\*\*\*\* 端點 URL 的相同值。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-162">In the **Web Server Settings** area, set the **App URL** to the same value used for the **Launch browser** endpoint URL.</span></span>
1. <span data-ttu-id="0b2c8-163">針對 Visual Studio 2019 或更新版本中的 [裝載模型]\*\*\*\*，請選取 [預設]\*\*\*\* 以使用專案所使用的裝載模型。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-163">For the **Hosting Model** setting in Visual Studio 2019 or later, select **Default** to use the hosting model used by the project.</span></span> <span data-ttu-id="0b2c8-164">如果專案在其專案檔中已設定 `<AspNetCoreHostingModel>` 屬性，則會使用該屬性的值 (`InProcess` 或 `OutOfProcess`)。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-164">If the project sets the `<AspNetCoreHostingModel>` property in its project file, the value of the property (`InProcess` or `OutOfProcess`) is used.</span></span> <span data-ttu-id="0b2c8-165">如果該屬性不存在，便會使用應用程式的預設裝載模型，其為內部處理序。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-165">If the property isn't present, the default hosting model of the app is used, which is in-process.</span></span> <span data-ttu-id="0b2c8-166">如果應用程式需要與應用程式一般裝載模型不同的明確裝載模型設定，請視需要將 [裝載模型]\*\*\*\* 設定為 `In Process` 或 `Out Of Process`。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-166">If the app requires an explicit hosting model setting different from the app's normal hosting model, set the **Hosting Model** to either `In Process` or `Out Of Process` as needed.</span></span>
1. <span data-ttu-id="0b2c8-167">儲存設定檔。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-167">Save the profile.</span></span>

<span data-ttu-id="0b2c8-168">不使用 Visual Studio 時，請手動將啟動設定檔新增至 *Properties* 資料夾中的 [launchSettings.json](https://json.schemastore.org/launchsettings) \(英文\) 檔案。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-168">When not using Visual Studio, manually add a launch profile to the [launchSettings.json](https://json.schemastore.org/launchsettings) file in the *Properties* folder.</span></span> <span data-ttu-id="0b2c8-169">下列範例會將專案設定為使用 HTTPS 通訊協定：</span><span class="sxs-lookup"><span data-stu-id="0b2c8-169">The following example configures the profile to use the HTTPS protocol:</span></span>

```json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iis": {
      "applicationUrl": "https://localhost/WebApplication1",
      "sslPort": 0
    }
  },
  "profiles": {
    "IIS": {
      "commandName": "IIS",
      "launchBrowser": true,
      "launchUrl": "https://localhost/WebApplication1",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```

<span data-ttu-id="0b2c8-170">請確認 `applicationUrl` 和 `launchUrl` 端點相符，並使用和 IIS 繫結設定相同的通訊協定 (HTTP 或 HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-170">Confirm that the `applicationUrl` and `launchUrl` endpoints match and use the same protocol as the IIS binding configuration, either HTTP or HTTPS.</span></span>

## <a name="run-the-project"></a><span data-ttu-id="0b2c8-171">執行專案</span><span class="sxs-lookup"><span data-stu-id="0b2c8-171">Run the project</span></span>

<span data-ttu-id="0b2c8-172">以系統管理員身分執行 Visual Studio：</span><span class="sxs-lookup"><span data-stu-id="0b2c8-172">Run Visual Studio as an administrator:</span></span>

* <span data-ttu-id="0b2c8-173">確認建置設定下拉式清單已設定為 [偵錯]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-173">Confirm that the build configuration drop-down list is set to **Debug**.</span></span>
* <span data-ttu-id="0b2c8-174">將 [[開始調試](/visualstudio/debugger/debugger-feature-tour)程式] 按鈕設定為**IIS**設定檔，然後選取 [] 按鈕來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-174">Set the [Start Debugging button](/visualstudio/debugger/debugger-feature-tour) to the **IIS** profile and select the button to start the app.</span></span>

<span data-ttu-id="0b2c8-175">如果不是以系統管理員身分執行，Visual Studio 可能會提示您重新啟動。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-175">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="0b2c8-176">若收到提示，請重新啟動 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-176">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="0b2c8-177">如果使用未受信任的開發憑證，瀏覽器可能會要求您針對未受信任的憑證建立例外。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-177">If an untrusted development certificate is used, the browser may require you to create an exception for the untrusted certificate.</span></span>

> [!NOTE]
> <span data-ttu-id="0b2c8-178">使用 [Just My Code](/visualstudio/debugger/just-my-code) 與編譯器最佳化針對發行建置設定進行偵錯會導致體驗變差。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-178">Debugging a Release build configuration with [Just My Code](/visualstudio/debugger/just-my-code) and compiler optimizations results in a degraded experience.</span></span> <span data-ttu-id="0b2c8-179">例如，不會碰到中斷點。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-179">For example, break points aren't hit.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0b2c8-180">其他資源</span><span class="sxs-lookup"><span data-stu-id="0b2c8-180">Additional resources</span></span>

* [<span data-ttu-id="0b2c8-181">IIS 中的 IIS 管理員使用者入門</span><span class="sxs-lookup"><span data-stu-id="0b2c8-181">Getting Started with the IIS Manager in IIS</span></span>](/iis/get-started/getting-started-with-iis/getting-started-with-the-iis-manager-in-iis-7-and-iis-8)
* <xref:security/enforcing-ssl>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="0b2c8-182">本文說明針對在 Windows Server 上搭配 IIS 執行的 ASP.NET Core 應用程式所提供的 [Visual Studio](https://visualstudio.microsoft.com) 偵錯支援。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-182">This article describes [Visual Studio](https://visualstudio.microsoft.com) support for debugging ASP.NET Core apps running with IIS on Windows Server.</span></span> <span data-ttu-id="0b2c8-183">本主題會逐步解說如何啟用此案例及設定專案。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-183">This topic walks through enabling this scenario and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0b2c8-184">必要條件</span><span class="sxs-lookup"><span data-stu-id="0b2c8-184">Prerequisites</span></span>

* [<span data-ttu-id="0b2c8-185">適用于 Windows 的 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0b2c8-185">Visual Studio for Windows</span></span>](https://visualstudio.microsoft.com/downloads/)
* <span data-ttu-id="0b2c8-186">**ASP.NET 與網頁程式開發**工作負載</span><span class="sxs-lookup"><span data-stu-id="0b2c8-186">**ASP.NET and web development** workload</span></span>
* <span data-ttu-id="0b2c8-187">**.NET Core 跨平台開發**工作負載</span><span class="sxs-lookup"><span data-stu-id="0b2c8-187">**.NET Core cross-platform development** workload</span></span>
* <span data-ttu-id="0b2c8-188">X.509 安全性憑證 (以取得 HTTPS 支援)</span><span class="sxs-lookup"><span data-stu-id="0b2c8-188">X.509 security certificate (for HTTPS support)</span></span>

## <a name="enable-iis"></a><span data-ttu-id="0b2c8-189">啟用 IIS</span><span class="sxs-lookup"><span data-stu-id="0b2c8-189">Enable IIS</span></span>

1. <span data-ttu-id="0b2c8-190">在 Windows 中，瀏覽至 [控制台]**[程式]** > **[程式和功能]** > **[開啟或關閉 Windows 功能]** > \*\*\*\* (畫面左側)。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-190">In Windows, navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="0b2c8-191">選取 [Internet Information Services]\*\*\*\* 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-191">Select the **Internet Information Services** check box.</span></span> <span data-ttu-id="0b2c8-192">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-192">Select **OK**.</span></span>

<span data-ttu-id="0b2c8-193">安裝 IIS 可能需要重新啟動系統。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-193">The IIS installation may require a system restart.</span></span>

## <a name="configure-iis"></a><span data-ttu-id="0b2c8-194">設定 IIS</span><span class="sxs-lookup"><span data-stu-id="0b2c8-194">Configure IIS</span></span>

<span data-ttu-id="0b2c8-195">IIS 的網站必須含有下列設定：</span><span class="sxs-lookup"><span data-stu-id="0b2c8-195">IIS must have a website configured with the following:</span></span>

* <span data-ttu-id="0b2c8-196">**主機名稱**：通常，**預設的網站**會與**主機名稱**搭配使用 `localhost` 。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-196">**Host name**: Typically, the **Default Web Site** is used with a **Host name** of `localhost`.</span></span> <span data-ttu-id="0b2c8-197">不過，任何具有唯一主機名稱的有效 IIS 網站皆適用。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-197">However, any valid IIS website with a unique host name works.</span></span>
* <span data-ttu-id="0b2c8-198">**網站繫結**</span><span class="sxs-lookup"><span data-stu-id="0b2c8-198">**Site Binding**</span></span>
  * <span data-ttu-id="0b2c8-199">針對需要 HTTPS 的應用程式，請搭配憑證針對連接埠 443 建立繫結。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-199">For apps that require HTTPS, create a binding to port 443 with a certificate.</span></span> <span data-ttu-id="0b2c8-200">通常會使用 [IIS Express 開發憑證]\*\*\*\*，但可使用任何有效的憑證。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-200">Typically, the **IIS Express Development Certificate** is used, but any valid certificate works.</span></span>
  * <span data-ttu-id="0b2c8-201">針對使用 HTTP 的應用程式，請確認已存在針對連接埠 80 的繫結，或是為新網站建立針對連接埠 80 的繫結。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-201">For apps that use HTTP, confirm the existence of a binding to post 80 or create a binding to port 80 for a new site.</span></span>
  * <span data-ttu-id="0b2c8-202">需針對 HTTP 或 HTTPS 使用單一繫結。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-202">Use a single binding for either HTTP or HTTPS.</span></span> <span data-ttu-id="0b2c8-203">**不支援同時針對 HTTP 和 HTTPS 連接埠的繫結。**</span><span class="sxs-lookup"><span data-stu-id="0b2c8-203">**Binding to both HTTP and HTTPS ports simultaneously isn't supported.**</span></span>

## <a name="enable-development-time-iis-support-in-visual-studio"></a><span data-ttu-id="0b2c8-204">在 Visual Studio 中啟用開發階段 IIS 支援</span><span class="sxs-lookup"><span data-stu-id="0b2c8-204">Enable development-time IIS support in Visual Studio</span></span>

1. <span data-ttu-id="0b2c8-205">啟動 Visual Studio 安裝程式。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-205">Launch the Visual Studio installer.</span></span>
1. <span data-ttu-id="0b2c8-206">針對您打算用於 IIS 開發階段支援的 Visual Studio 安裝，選取 [修改]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-206">Select **Modify** for the Visual Studio installation that you plan to use for IIS development-time support.</span></span>
1. <span data-ttu-id="0b2c8-207">針對 [ASP.NET 與網頁程式開發]\*\*\*\* 工作負載，尋找並安裝 [開發時間的 IIS 支援]\*\*\*\* 元件。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-207">For the **ASP.NET and web development** workload, locate and install the **Development time IIS support** component.</span></span>

   <span data-ttu-id="0b2c8-208">該元件會列於工作負載右側 [安裝詳細資料]\*\*\*\* 中 [開發時間的 IIS 支援]\*\*\*\* 底下的 [選擇性]\*\*\*\* 區段中。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-208">The component is listed in the **Optional** section under **Development time IIS support** in the **Installation details** panel to the right of the workloads.</span></span> <span data-ttu-id="0b2c8-209">此元件會安裝 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)，這是搭配 IIS 執行 ASP.NET Core 應用程式所需的原生 IIS 模組。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-209">The component installs the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps with IIS.</span></span>

## <a name="configure-the-project"></a><span data-ttu-id="0b2c8-210">設定專案</span><span class="sxs-lookup"><span data-stu-id="0b2c8-210">Configure the project</span></span>

### <a name="https-redirection"></a><span data-ttu-id="0b2c8-211">HTTPS 重新導向</span><span class="sxs-lookup"><span data-stu-id="0b2c8-211">HTTPS redirection</span></span>

<span data-ttu-id="0b2c8-212">針對需要 HTTPS 的新專案，請在 [建立新的 ASP.NET Core Web 應用程式]\*\*\*\* 視窗中，選取 [針對 HTTPS 進行設定]\*\*\*\* 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-212">For a new project that requires HTTPS, select the check box to **Configure for HTTPS** in the **Create a new ASP.NET Core Web Application** window.</span></span> <span data-ttu-id="0b2c8-213">選取該核取方塊會在建立應用程式時，將 [HTTPS 重新導向和 HSTS 中介軟體](xref:security/enforcing-ssl)加入該應用程式。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-213">Selecting the check box adds [HTTPS Redirection and HSTS Middleware](xref:security/enforcing-ssl) to the app when it's created.</span></span>

<span data-ttu-id="0b2c8-214">針對需要 HTTPS 的現有專案，請使用 `Startup.Configure`中的 HTTPS 重新導向和 HSTS 中介軟體。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-214">For an existing project that requires HTTPS, use HTTPS Redirection and HSTS Middleware in `Startup.Configure`.</span></span> <span data-ttu-id="0b2c8-215">如需詳細資訊，請參閱<xref:security/enforcing-ssl>。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-215">For more information, see <xref:security/enforcing-ssl>.</span></span>

<span data-ttu-id="0b2c8-216">針對使用 HTTP 的專案，[HTTPS 重新導向和 HSTS 中介軟體](xref:security/enforcing-ssl)並不會被加入至應用程式。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-216">For a project that uses HTTP, [HTTPS Redirection and HSTS Middleware](xref:security/enforcing-ssl) aren't added to the app.</span></span> <span data-ttu-id="0b2c8-217">您不需要進行任何應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-217">No app configuration is required.</span></span>

### <a name="iis-launch-profile"></a><span data-ttu-id="0b2c8-218">IIS 啟動設定檔</span><span class="sxs-lookup"><span data-stu-id="0b2c8-218">IIS launch profile</span></span>

<span data-ttu-id="0b2c8-219">建立新的啟動設定檔，以新增開發階段 IIS 支援：</span><span class="sxs-lookup"><span data-stu-id="0b2c8-219">Create a new launch profile to add development-time IIS support:</span></span>

1. <span data-ttu-id="0b2c8-220">以滑鼠右鍵按一下 [方案總管]\*\*\*\* 中的專案。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-220">Right-click the project in **Solution Explorer**.</span></span> <span data-ttu-id="0b2c8-221">選取 [屬性]  。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-221">Select **Properties**.</span></span> <span data-ttu-id="0b2c8-222">開啟 [**調試**] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-222">Open the **Debug** tab.</span></span>
1. <span data-ttu-id="0b2c8-223">針對 [設定檔]\*\*\*\*，選取 [新增]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-223">For **Profile**, select the **New** button.</span></span> <span data-ttu-id="0b2c8-224">在快顯示窗中，將設定檔命名為 "IIS"。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-224">Name the profile "IIS" in the popup window.</span></span> <span data-ttu-id="0b2c8-225">選取 [確定]\*\*\*\* 以建立設定檔。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-225">Select **OK** to create the profile.</span></span>
1. <span data-ttu-id="0b2c8-226">針對 [啟動]\*\*\*\* 設定，從清單中選取 [IIS]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-226">For the **Launch** setting, select **IIS** from the list.</span></span>
1. <span data-ttu-id="0b2c8-227">選取 [啟動瀏覽器]\*\*\*\* 的核取方塊並提供端點 URL。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-227">Select the check box for **Launch browser** and provide the endpoint URL.</span></span>

   <span data-ttu-id="0b2c8-228">當應用程式需要 HTTPS 時，請使用 HTTPS 端點 (`https://`)。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-228">When the app requires HTTPS, use an HTTPS endpoint (`https://`).</span></span> <span data-ttu-id="0b2c8-229">針對 HTTP，請使用 HTTP (`http://`) 端點。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-229">For HTTP, use an HTTP (`http://`) endpoint.</span></span>

   <span data-ttu-id="0b2c8-230">提供相同的主機名稱和連接埠作為 [IIS 設定所指定的早期用途](#configure-iis)，其通常是 `localhost`。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-230">Provide the same host name and port as the [IIS configuration specified earlier uses](#configure-iis), typically `localhost`.</span></span>

   <span data-ttu-id="0b2c8-231">在 URL 結尾提供應用程式的名稱。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-231">Provide the name of the app at the end of the URL.</span></span>

   <span data-ttu-id="0b2c8-232">例如，`https://localhost/WebApplication1` (HTTPS) 或 `http://localhost/WebApplication1` (HTTP) 皆為有效的端點 URL。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-232">For example, `https://localhost/WebApplication1` (HTTPS) or `http://localhost/WebApplication1` (HTTP) are valid endpoint URLs.</span></span>
1. <span data-ttu-id="0b2c8-233">在 [環境變數]\*\*\*\* 區段中，選取 [新增]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-233">In the **Environment variables** section, select the **Add** button.</span></span> <span data-ttu-id="0b2c8-234">提供 [名稱]\*\*\*\* 為 `ASPNETCORE_ENVIRONMENT` 且 [值]\*\*\*\* 為 `Development` 的環境變數。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-234">Provide an environment variable with a **Name** of `ASPNETCORE_ENVIRONMENT` and a **Value** of `Development`.</span></span>
1. <span data-ttu-id="0b2c8-235">在 [Web 伺服器設定]\*\*\*\* 區域中，將 [應用程式 URL]\*\*\*\* 設定為用於 [啟動瀏覽器]\*\*\*\* 端點 URL 的相同值。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-235">In the **Web Server Settings** area, set the **App URL** to the same value used for the **Launch browser** endpoint URL.</span></span>
1. <span data-ttu-id="0b2c8-236">針對 Visual Studio 2019 或更新版本中的 [裝載模型]\*\*\*\*，請選取 [預設]\*\*\*\* 以使用專案所使用的裝載模型。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-236">For the **Hosting Model** setting in Visual Studio 2019 or later, select **Default** to use the hosting model used by the project.</span></span> <span data-ttu-id="0b2c8-237">如果專案在其專案檔中已設定 `<AspNetCoreHostingModel>` 屬性，則會使用該屬性的值 (`InProcess` 或 `OutOfProcess`)。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-237">If the project sets the `<AspNetCoreHostingModel>` property in its project file, the value of the property (`InProcess` or `OutOfProcess`) is used.</span></span> <span data-ttu-id="0b2c8-238">如果該屬性不存在，便會使用應用程式的預設裝載模型，其為外部處理序。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-238">If the property isn't present, the default hosting model of the app is used, which is out-of-process.</span></span> <span data-ttu-id="0b2c8-239">如果應用程式需要與應用程式一般裝載模型不同的明確裝載模型設定，請視需要將 [裝載模型]\*\*\*\* 設定為 `In Process` 或 `Out Of Process`。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-239">If the app requires an explicit hosting model setting different from the app's normal hosting model, set the **Hosting Model** to either `In Process` or `Out Of Process` as needed.</span></span>
1. <span data-ttu-id="0b2c8-240">儲存設定檔。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-240">Save the profile.</span></span>

<span data-ttu-id="0b2c8-241">不使用 Visual Studio 時，請手動將啟動設定檔新增至 *Properties* 資料夾中的 [launchSettings.json](https://json.schemastore.org/launchsettings) \(英文\) 檔案。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-241">When not using Visual Studio, manually add a launch profile to the [launchSettings.json](https://json.schemastore.org/launchsettings) file in the *Properties* folder.</span></span> <span data-ttu-id="0b2c8-242">下列範例會將專案設定為使用 HTTPS 通訊協定：</span><span class="sxs-lookup"><span data-stu-id="0b2c8-242">The following example configures the profile to use the HTTPS protocol:</span></span>

```json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iis": {
      "applicationUrl": "https://localhost/WebApplication1",
      "sslPort": 0
    }
  },
  "profiles": {
    "IIS": {
      "commandName": "IIS",
      "launchBrowser": true,
      "launchUrl": "https://localhost/WebApplication1",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```

<span data-ttu-id="0b2c8-243">請確認 `applicationUrl` 和 `launchUrl` 端點相符，並使用和 IIS 繫結設定相同的通訊協定 (HTTP 或 HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-243">Confirm that the `applicationUrl` and `launchUrl` endpoints match and use the same protocol as the IIS binding configuration, either HTTP or HTTPS.</span></span>

## <a name="run-the-project"></a><span data-ttu-id="0b2c8-244">執行專案</span><span class="sxs-lookup"><span data-stu-id="0b2c8-244">Run the project</span></span>

<span data-ttu-id="0b2c8-245">以系統管理員身分執行 Visual Studio：</span><span class="sxs-lookup"><span data-stu-id="0b2c8-245">Run Visual Studio as an administrator:</span></span>

* <span data-ttu-id="0b2c8-246">確認建置設定下拉式清單已設定為 [偵錯]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-246">Confirm that the build configuration drop-down list is set to **Debug**.</span></span>
* <span data-ttu-id="0b2c8-247">將 [[開始調試](/visualstudio/debugger/debugger-feature-tour)程式] 按鈕設定為**IIS**設定檔，然後選取 [] 按鈕來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-247">Set the [Start Debugging button](/visualstudio/debugger/debugger-feature-tour) to the **IIS** profile and select the button to start the app.</span></span>

<span data-ttu-id="0b2c8-248">如果不是以系統管理員身分執行，Visual Studio 可能會提示您重新啟動。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-248">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="0b2c8-249">若收到提示，請重新啟動 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-249">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="0b2c8-250">如果使用未受信任的開發憑證，瀏覽器可能會要求您針對未受信任的憑證建立例外。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-250">If an untrusted development certificate is used, the browser may require you to create an exception for the untrusted certificate.</span></span>

> [!NOTE]
> <span data-ttu-id="0b2c8-251">使用 [Just My Code](/visualstudio/debugger/just-my-code) 與編譯器最佳化針對發行建置設定進行偵錯會導致體驗變差。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-251">Debugging a Release build configuration with [Just My Code](/visualstudio/debugger/just-my-code) and compiler optimizations results in a degraded experience.</span></span> <span data-ttu-id="0b2c8-252">例如，不會碰到中斷點。</span><span class="sxs-lookup"><span data-stu-id="0b2c8-252">For example, break points aren't hit.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0b2c8-253">其他資源</span><span class="sxs-lookup"><span data-stu-id="0b2c8-253">Additional resources</span></span>

* [<span data-ttu-id="0b2c8-254">IIS 中的 IIS 管理員使用者入門</span><span class="sxs-lookup"><span data-stu-id="0b2c8-254">Getting Started with the IIS Manager in IIS</span></span>](/iis/get-started/getting-started-with-iis/getting-started-with-the-iis-manager-in-iis-7-and-iis-8)
* <xref:security/enforcing-ssl>

::: moniker-end
