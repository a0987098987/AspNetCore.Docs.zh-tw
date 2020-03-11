# <a name="visual-studio"></a>[<span data-ttu-id="1accb-101">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1accb-101">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="1accb-102">建立新專案。</span><span class="sxs-lookup"><span data-stu-id="1accb-102">Create a new project.</span></span>
1. <span data-ttu-id="1accb-103">選取 [背景**工作服務**]。</span><span class="sxs-lookup"><span data-stu-id="1accb-103">Select **Worker Service**.</span></span> <span data-ttu-id="1accb-104">選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="1accb-104">Select **Next**.</span></span>
1. <span data-ttu-id="1accb-105">在 [專案名稱] 欄位中提供專案名稱，或接受預設專案名稱。</span><span class="sxs-lookup"><span data-stu-id="1accb-105">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="1accb-106">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="1accb-106">Select **Create**.</span></span>
1. <span data-ttu-id="1accb-107">在 [**建立新的背景工作服務**] 對話方塊中，選取 [**建立**]。</span><span class="sxs-lookup"><span data-stu-id="1accb-107">In the **Create a new Worker service** dialog, select **Create**.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="1accb-108">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="1accb-108">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="1accb-109">建立新專案。</span><span class="sxs-lookup"><span data-stu-id="1accb-109">Create a new project.</span></span>
1. <span data-ttu-id="1accb-110">在提要欄位中選取 [ **.Net Core** ] 底下的 [**應用程式**]。</span><span class="sxs-lookup"><span data-stu-id="1accb-110">Select **App** under **.NET Core** in the sidebar.</span></span>
1. <span data-ttu-id="1accb-111">選取  **ASP.NET Core**下的 背景**工作角色**。</span><span class="sxs-lookup"><span data-stu-id="1accb-111">Select **Worker** under **ASP.NET Core**.</span></span> <span data-ttu-id="1accb-112">選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="1accb-112">Select **Next**.</span></span>
1. <span data-ttu-id="1accb-113">針對**目標 Framework**選取 [ **.net Core 3.0** ] 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="1accb-113">Select **.NET Core 3.0** or later for the **Target Framework**.</span></span> <span data-ttu-id="1accb-114">選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="1accb-114">Select **Next**.</span></span>
1. <span data-ttu-id="1accb-115">在 [**專案名稱**] 欄位中提供名稱。</span><span class="sxs-lookup"><span data-stu-id="1accb-115">Provide a name in the **Project Name** field.</span></span> <span data-ttu-id="1accb-116">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="1accb-116">Select **Create**.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="1accb-117">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="1accb-117">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="1accb-118">從命令殼層以 `worker`dotnet new[ 命令使用背景工作服務 (](/dotnet/core/tools/dotnet-new)) 範本。</span><span class="sxs-lookup"><span data-stu-id="1accb-118">Use the Worker Service (`worker`) template with the [dotnet new](/dotnet/core/tools/dotnet-new) command from a command shell.</span></span> <span data-ttu-id="1accb-119">在下列範例中，已建立名為 `ContosoWorker` 的背景工作服務應用程式。</span><span class="sxs-lookup"><span data-stu-id="1accb-119">In the following example, a Worker Service app is created named `ContosoWorker`.</span></span> <span data-ttu-id="1accb-120">當命令執行時，會自動建立 `ContosoWorker` 應用程式的資料夾。</span><span class="sxs-lookup"><span data-stu-id="1accb-120">A folder for the `ContosoWorker` app is created automatically when the command is executed.</span></span>

```dotnetcli
dotnet new worker -o ContosoWorker
```

---
