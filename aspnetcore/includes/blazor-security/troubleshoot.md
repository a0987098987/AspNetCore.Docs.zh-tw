## <a name="troubleshoot"></a><span data-ttu-id="d9bcc-101">疑難排解</span><span class="sxs-lookup"><span data-stu-id="d9bcc-101">Troubleshoot</span></span>

### <a name="cookies-and-site-data"></a><span data-ttu-id="d9bcc-102">Cookie 和網站資料</span><span class="sxs-lookup"><span data-stu-id="d9bcc-102">Cookies and site data</span></span>

<span data-ttu-id="d9bcc-103">Cookie 和網站資料可以跨應用程式更新保存，並干擾測試和疑難排解。</span><span class="sxs-lookup"><span data-stu-id="d9bcc-103">Cookies and site data can persist across app updates and interfere with testing and troubleshooting.</span></span> <span data-ttu-id="d9bcc-104">進行應用程式程式碼變更、使用者帳戶與提供者的變更，或提供者應用程式設定變更時，請清除下列各項：</span><span class="sxs-lookup"><span data-stu-id="d9bcc-104">Clear the following when making app code changes, user account changes with the provider, or provider app configuration changes:</span></span>

* <span data-ttu-id="d9bcc-105">使用者登入 cookie</span><span class="sxs-lookup"><span data-stu-id="d9bcc-105">User sign-in cookies</span></span>
* <span data-ttu-id="d9bcc-106">應用程式 cookie</span><span class="sxs-lookup"><span data-stu-id="d9bcc-106">App cookies</span></span>
* <span data-ttu-id="d9bcc-107">快取和儲存的網站資料</span><span class="sxs-lookup"><span data-stu-id="d9bcc-107">Cached and stored site data</span></span>

<span data-ttu-id="d9bcc-108">防止延遲 cookie 和網站資料干擾測試和疑難排解的一種方法是：</span><span class="sxs-lookup"><span data-stu-id="d9bcc-108">One approach to prevent lingering cookies and site data from interfering with testing and troubleshooting is to:</span></span>

* <span data-ttu-id="d9bcc-109">設定瀏覽器</span><span class="sxs-lookup"><span data-stu-id="d9bcc-109">Configure a browser</span></span>
  * <span data-ttu-id="d9bcc-110">使用瀏覽器進行測試，您可以設定在每次瀏覽器關閉時刪除所有 cookie 和網站資料。</span><span class="sxs-lookup"><span data-stu-id="d9bcc-110">Use a browser for testing that you can configure to delete all cookie and site data each time the browser is closed.</span></span>
  * <span data-ttu-id="d9bcc-111">請確定瀏覽器已手動關閉，或由 IDE 在應用程式、測試使用者或提供者設定之間進行任何變更。</span><span class="sxs-lookup"><span data-stu-id="d9bcc-111">Make sure that the browser is closed manually or by the IDE between any change to the app, test user, or provider configuration.</span></span>
* <span data-ttu-id="d9bcc-112">使用自訂命令，以 Visual Studio 中的 incognito 或私用模式開啟瀏覽器：</span><span class="sxs-lookup"><span data-stu-id="d9bcc-112">Use a custom command to open a browser in incognito or private mode in Visual Studio:</span></span>
  * <span data-ttu-id="d9bcc-113">從 Visual Studio 的 [**執行**] 按鈕開啟 **[流覽方式**] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="d9bcc-113">Open **Browse With** dialog box from Visual Studio's **Run** button.</span></span>
  * <span data-ttu-id="d9bcc-114">選取 [新增] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="d9bcc-114">Select the **Add** button.</span></span>
  * <span data-ttu-id="d9bcc-115">在 [**程式**] 欄位中提供瀏覽器的路徑。</span><span class="sxs-lookup"><span data-stu-id="d9bcc-115">Provide the path to your browser in the **Program** field.</span></span>
  * <span data-ttu-id="d9bcc-116">在 [**引數**] 欄位中，提供瀏覽器用來以 incognito 或私用模式開啟的命令列選項，以及應用程式的 URL。</span><span class="sxs-lookup"><span data-stu-id="d9bcc-116">In the **Arguments** field, provide the command-line option that the browser uses to open in incognito or private mode and the URL of the app.</span></span> <span data-ttu-id="d9bcc-117">例如：</span><span class="sxs-lookup"><span data-stu-id="d9bcc-117">For example:</span></span>
    * <span data-ttu-id="d9bcc-118">Google Chrome：`--incognito --new-window https://localhost:5001`</span><span class="sxs-lookup"><span data-stu-id="d9bcc-118">Google Chrome: `--incognito --new-window https://localhost:5001`</span></span>
    * <span data-ttu-id="d9bcc-119">Mozilla Firefox：`-private -url https://localhost:5001`</span><span class="sxs-lookup"><span data-stu-id="d9bcc-119">Mozilla Firefox: `-private -url https://localhost:5001`</span></span>
  * <span data-ttu-id="d9bcc-120">在 [**易記名稱**] 欄位中提供名稱。</span><span class="sxs-lookup"><span data-stu-id="d9bcc-120">Provide a name in the **Friendly name** field.</span></span> <span data-ttu-id="d9bcc-121">例如 `Firefox Auth Testing`。</span><span class="sxs-lookup"><span data-stu-id="d9bcc-121">For example, `Firefox Auth Testing`.</span></span>
  * <span data-ttu-id="d9bcc-122">選取 [**確定]** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="d9bcc-122">Select the **OK** button.</span></span>
  * <span data-ttu-id="d9bcc-123">若要避免針對使用應用程式測試的每個反復專案選取瀏覽器設定檔，請將設定檔設定為預設值，並將**設定為預設**按鈕。</span><span class="sxs-lookup"><span data-stu-id="d9bcc-123">To avoid having to select the browser profile for each iteration of testing with an app, set the profile as the default with the **Set as Default** button.</span></span>
  * <span data-ttu-id="d9bcc-124">請確定在應用程式、測試使用者或提供者設定的任何變更之間，IDE 已關閉瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="d9bcc-124">Make sure that the browser is closed by the IDE between any change to the app, test user, or provider configuration.</span></span>

### <a name="run-the-server-app"></a><span data-ttu-id="d9bcc-125">執行伺服器應用程式</span><span class="sxs-lookup"><span data-stu-id="d9bcc-125">Run the Server app</span></span>

<span data-ttu-id="d9bcc-126">測試和疑難排解託管的 Blazor 應用程式時，請確定您是從**伺服器**專案執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="d9bcc-126">When testing and troubleshooting a hosted Blazor app, make sure that you're running the app from the **Server** project.</span></span> <span data-ttu-id="d9bcc-127">例如，在 Visual Studio 中，在您使用下列任何方法啟動應用程式之前，請確認已在**方案總管**中將伺服器專案反白顯示：</span><span class="sxs-lookup"><span data-stu-id="d9bcc-127">For example in Visual Studio, confirm that the Server project is highlighted in **Solution Explorer** before you start the app with any of the following approaches:</span></span>

* <span data-ttu-id="d9bcc-128">選取 [執行] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="d9bcc-128">Select the **Run** button.</span></span>
* <span data-ttu-id="d9bcc-129">使用**Debug**  >  功能表中的 [**開始調試**]。</span><span class="sxs-lookup"><span data-stu-id="d9bcc-129">Use **Debug** > **Start Debugging** from the menu.</span></span>
* <span data-ttu-id="d9bcc-130">按 <kbd>F5</kbd>。</span><span class="sxs-lookup"><span data-stu-id="d9bcc-130">Press <kbd>F5</kbd>.</span></span>

### <a name="inspect-the-content-of-a-json-web-token-jwt"></a><span data-ttu-id="d9bcc-131">檢查 JSON Web 權杖（JWT）的內容</span><span class="sxs-lookup"><span data-stu-id="d9bcc-131">Inspect the content of a JSON Web Token (JWT)</span></span>

<span data-ttu-id="d9bcc-132">若要解碼 JSON Web Token （JWT），請使用 Microsoft 的[jwt.ms](https://jwt.ms/)工具。</span><span class="sxs-lookup"><span data-stu-id="d9bcc-132">To decode a JSON Web Token (JWT), use Microsoft's [jwt.ms](https://jwt.ms/) tool.</span></span> <span data-ttu-id="d9bcc-133">UI 中的值永遠不會離開您的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="d9bcc-133">Values in the UI never leave your browser.</span></span>
