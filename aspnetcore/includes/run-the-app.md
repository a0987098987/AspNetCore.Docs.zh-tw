# <a name="visual-studio"></a>[<span data-ttu-id="e72d0-101">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e72d0-101">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e72d0-102">按 Ctrl+F5 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="e72d0-102">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="e72d0-103">Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="e72d0-103">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="e72d0-104">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="e72d0-104">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="e72d0-105">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="e72d0-105">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="e72d0-106">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="e72d0-106">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="e72d0-107">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="e72d0-107">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
 
# <a name="visual-studio-code"></a>[<span data-ttu-id="e72d0-108">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e72d0-108">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="e72d0-109">按 **Ctrl-F5** 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="e72d0-109">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="e72d0-110">Visual Studio Code 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)、啟動瀏覽器，然後瀏覽至 `http://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="e72d0-110">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="e72d0-111">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="e72d0-111">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="e72d0-112">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="e72d0-112">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="e72d0-113">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="e72d0-113">Localhost only serves web requests from the local computer.</span></span>

  
# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="e72d0-114">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e72d0-114">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="e72d0-115">從 Visual Studio 中,按**Opt-Cmd-Return**可在沒有調試器的情況下運行。</span><span class="sxs-lookup"><span data-stu-id="e72d0-115">From Visual Studio, press **Opt-Cmd-Return** to run without the debugger.</span></span> <span data-ttu-id="e72d0-116">或者,導航到功能表欄,然後轉到 **"不調試>啟動**"。</span><span class="sxs-lookup"><span data-stu-id="e72d0-116">Alternatively, navigate to the menu bar and go to **Run>Start Without Debugging**.</span></span>

  <span data-ttu-id="e72d0-117">Visual Studio 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)啟動瀏覽器，然後巡覽至 `http://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="e72d0-117">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

<!-- End of VS tabs -->

---