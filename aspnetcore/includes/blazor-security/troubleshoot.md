## <a name="troubleshoot"></a><span data-ttu-id="71402-101">疑難排解</span><span class="sxs-lookup"><span data-stu-id="71402-101">Troubleshoot</span></span>

### <a name="cookies-and-site-data"></a><span data-ttu-id="71402-102">Cookie 和網站資料</span><span class="sxs-lookup"><span data-stu-id="71402-102">Cookies and site data</span></span>

<span data-ttu-id="71402-103">Cookie 和網站數據可能會在應用更新中保留,並干擾測試和故障排除。</span><span class="sxs-lookup"><span data-stu-id="71402-103">Cookies and site data can persist across app updates and interfere with testing and troubleshooting.</span></span> <span data-ttu-id="71402-104">變更應用程式碼、使用提供程式變更使用者帳戶或提供程式應用程式設定變更時清除以下內容:</span><span class="sxs-lookup"><span data-stu-id="71402-104">Clear the following when making app code changes, user account changes with the provider, or provider app configuration changes:</span></span>

* <span data-ttu-id="71402-105">使用者登入 Cookie</span><span class="sxs-lookup"><span data-stu-id="71402-105">User sign-in cookies</span></span>
* <span data-ttu-id="71402-106">套用 Cookie</span><span class="sxs-lookup"><span data-stu-id="71402-106">App cookies</span></span>
* <span data-ttu-id="71402-107">快取及儲存的網站資料</span><span class="sxs-lookup"><span data-stu-id="71402-107">Cached and stored site data</span></span>

<span data-ttu-id="71402-108">防止揮之不去的 Cookie 和站點資料干擾測試和故障排除的一種方法是:</span><span class="sxs-lookup"><span data-stu-id="71402-108">One approach to prevent lingering cookies and site data from interfering with testing and troubleshooting is to:</span></span>

* <span data-ttu-id="71402-109">使用瀏覽器進行測試,您可以設定該瀏覽器可在每次關閉瀏覽器時刪除所有 Cookie 和網站數據。</span><span class="sxs-lookup"><span data-stu-id="71402-109">Use a browser for testing that you can configure to delete all cookie and site data each time the browser is closed.</span></span>
* <span data-ttu-id="71402-110">關閉對應用、測試使用者或提供程式配置的任何更改之間的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="71402-110">Close the browser between any change to the app, test user, or provider configuration.</span></span>

### <a name="run-the-server-app"></a><span data-ttu-id="71402-111">執行伺服器應用</span><span class="sxs-lookup"><span data-stu-id="71402-111">Run the Server app</span></span>

<span data-ttu-id="71402-112">測試和排除託管 Blazor 應用時,請確保從 **「伺服器」** 專案運行該應用。</span><span class="sxs-lookup"><span data-stu-id="71402-112">When testing and troubleshooting a hosted Blazor app, make sure that you're running the app from the **Server** project.</span></span> <span data-ttu-id="71402-113">例如,在 Visual Studio 中,在使用以下任一方法啟動應用之前,請確認伺服器專案在**解決方案資源管理器**中突出顯示:</span><span class="sxs-lookup"><span data-stu-id="71402-113">For example in Visual Studio, confirm that the Server project is highlighted in **Solution Explorer** before you start the app with any of the following approaches:</span></span>

* <span data-ttu-id="71402-114">選取 [執行]  按鈕。</span><span class="sxs-lookup"><span data-stu-id="71402-114">Select the **Run** button.</span></span>
* <span data-ttu-id="71402-115">使用**選** > 單**中除錯開始除錯**。</span><span class="sxs-lookup"><span data-stu-id="71402-115">Use **Debug** > **Start Debugging** from the menu.</span></span>
* <span data-ttu-id="71402-116">按 <kbd>F5</kbd>。</span><span class="sxs-lookup"><span data-stu-id="71402-116">Press <kbd>F5</kbd>.</span></span>
