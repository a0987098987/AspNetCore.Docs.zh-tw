# <a name="visual-studio"></a>[<span data-ttu-id="b1493-101">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b1493-101">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="b1493-102">建立新專案。</span><span class="sxs-lookup"><span data-stu-id="b1493-102">Create a new project.</span></span>
1. <span data-ttu-id="b1493-103">選擇**協助服務**。</span><span class="sxs-lookup"><span data-stu-id="b1493-103">Select **Worker Service**.</span></span> <span data-ttu-id="b1493-104">選取 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="b1493-104">Select **Next**.</span></span>
1. <span data-ttu-id="b1493-105">在 [專案名稱]\*\*\*\* 欄位中提供專案名稱，或接受預設專案名稱。</span><span class="sxs-lookup"><span data-stu-id="b1493-105">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="b1493-106">選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="b1493-106">Select **Create**.</span></span>
1. <span data-ttu-id="b1493-107">在「**創建新的工作輔助服務」** 對話方塊中,選擇 **「建立**」 。</span><span class="sxs-lookup"><span data-stu-id="b1493-107">In the **Create a new Worker service** dialog, select **Create**.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="b1493-108">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="b1493-108">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="b1493-109">建立新專案。</span><span class="sxs-lookup"><span data-stu-id="b1493-109">Create a new project.</span></span>
1. <span data-ttu-id="b1493-110">在側邊列中的 **.NET 核心**下選擇**套用 。**</span><span class="sxs-lookup"><span data-stu-id="b1493-110">Select **App** under **.NET Core** in the sidebar.</span></span>
1. <span data-ttu-id="b1493-111">在**ASP.NET核心**下選擇 **「輔助角色**」。</span><span class="sxs-lookup"><span data-stu-id="b1493-111">Select **Worker** under **ASP.NET Core**.</span></span> <span data-ttu-id="b1493-112">選取 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="b1493-112">Select **Next**.</span></span>
1. <span data-ttu-id="b1493-113">為目標**框架**選擇 **.NET 核心 3.0**或更高版本。</span><span class="sxs-lookup"><span data-stu-id="b1493-113">Select **.NET Core 3.0** or later for the **Target Framework**.</span></span> <span data-ttu-id="b1493-114">選取 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="b1493-114">Select **Next**.</span></span>
1. <span data-ttu-id="b1493-115">在 **「專案名稱」** 欄位中提供名稱。</span><span class="sxs-lookup"><span data-stu-id="b1493-115">Provide a name in the **Project Name** field.</span></span> <span data-ttu-id="b1493-116">選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="b1493-116">Select **Create**.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="b1493-117">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b1493-117">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="b1493-118">從命令殼層以 [dotnet new](/dotnet/core/tools/dotnet-new) 命令使用背景工作服務 (`worker`) 範本。</span><span class="sxs-lookup"><span data-stu-id="b1493-118">Use the Worker Service (`worker`) template with the [dotnet new](/dotnet/core/tools/dotnet-new) command from a command shell.</span></span> <span data-ttu-id="b1493-119">在下列範例中，已建立名為 `ContosoWorker` 的背景工作服務應用程式。</span><span class="sxs-lookup"><span data-stu-id="b1493-119">In the following example, a Worker Service app is created named `ContosoWorker`.</span></span> <span data-ttu-id="b1493-120">當命令執行時，會自動建立 `ContosoWorker` 應用程式的資料夾。</span><span class="sxs-lookup"><span data-stu-id="b1493-120">A folder for the `ContosoWorker` app is created automatically when the command is executed.</span></span>

```dotnetcli
dotnet new worker -o ContosoWorker
```

---
