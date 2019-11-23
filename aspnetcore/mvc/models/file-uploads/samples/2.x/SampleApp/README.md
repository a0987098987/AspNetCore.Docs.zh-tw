# <a name="upload-files-sample-app"></a><span data-ttu-id="3ace7-101">上傳檔案範例應用程式</span><span class="sxs-lookup"><span data-stu-id="3ace7-101">Upload Files Sample App</span></span>

<span data-ttu-id="3ace7-102">這個範例應用程式會示範在[ASP.NET Core 中上傳](https://docs.microsoft.com/aspnet/core/mvc/models/file-uploads)檔案主題中所述的概念。</span><span class="sxs-lookup"><span data-stu-id="3ace7-102">This sample app demonstrates concepts described in the [Upload files in ASP.NET Core](https://docs.microsoft.com/aspnet/core/mvc/models/file-uploads) topic.</span></span>

## <a name="security-considerations"></a><span data-ttu-id="3ace7-103">安全性考量</span><span class="sxs-lookup"><span data-stu-id="3ace7-103">Security considerations</span></span>

<span data-ttu-id="3ace7-104">提供使用者將檔案上傳到伺服器的能力時，請務必小心。</span><span class="sxs-lookup"><span data-stu-id="3ace7-104">Use caution when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="3ace7-105">攻擊者可能會執行阻絕[服務](/windows-hardware/drivers/ifs/denial-of-service)攻擊、嘗試上傳病毒或惡意程式碼，或嘗試以其他方式入侵網路和伺服器。</span><span class="sxs-lookup"><span data-stu-id="3ace7-105">Attackers may execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) attacks, attempt to upload viruses or malware, or attempt to compromise networks and servers in other ways.</span></span>

<span data-ttu-id="3ace7-106">降低成功攻擊的可能性的安全性步驟如下：</span><span class="sxs-lookup"><span data-stu-id="3ace7-106">Security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="3ace7-107">將檔案上傳到系統上的專用檔案上傳區域，最好是在非系統磁片磁碟機上。</span><span class="sxs-lookup"><span data-stu-id="3ace7-107">Upload files to a dedicated file upload area on the system, preferably to a non-system drive.</span></span> <span data-ttu-id="3ace7-108">使用專用的位置，可讓您更輕鬆地對上傳的檔案強加安全性措施。</span><span class="sxs-lookup"><span data-stu-id="3ace7-108">Use of a dedicated location makes it easier to impose security measures on uploaded files.</span></span> <span data-ttu-id="3ace7-109">停用檔案上傳位置的 execute 許可權。&dagger;</span><span class="sxs-lookup"><span data-stu-id="3ace7-109">Disable execute permissions on the file upload location.&dagger;</span></span>
* <span data-ttu-id="3ace7-110">絕對不要將上傳的檔案保存在與應用程式相同的目錄樹狀結構中。&dagger;</span><span class="sxs-lookup"><span data-stu-id="3ace7-110">Never persist uploaded files in the same directory tree as the app.&dagger;</span></span>
* <span data-ttu-id="3ace7-111">使用應用程式所決定的安全檔案名。</span><span class="sxs-lookup"><span data-stu-id="3ace7-111">Use a safe file name determined by the app.</span></span> <span data-ttu-id="3ace7-112">請勿使用使用者輸入所提供的檔案名，或所上傳檔案的不受信任檔案名。&dagger;</span><span class="sxs-lookup"><span data-stu-id="3ace7-112">Don't use a file name provided by user input or the untrusted file name of the uploaded file.&dagger;</span></span>
* <span data-ttu-id="3ace7-113">只允許一組特定的已核准副檔名。&dagger;</span><span class="sxs-lookup"><span data-stu-id="3ace7-113">Only allow a specific set of approved file extensions.&dagger;</span></span>
* <span data-ttu-id="3ace7-114">請檢查檔案格式簽章，以防止使用者上傳回報偽裝檔（例如，上傳副檔名為 *.txt*的 *.exe*檔案）。&dagger;</span><span class="sxs-lookup"><span data-stu-id="3ace7-114">Check the file format signature to prevent a user from uploading a masqueraded file (for example, uploading an *.exe* file with a *.txt* extension).&dagger;</span></span>
* <span data-ttu-id="3ace7-115">確認用戶端檢查也會在伺服器上執行。</span><span class="sxs-lookup"><span data-stu-id="3ace7-115">Verify that client-side checks are also performed on the server.</span></span> <span data-ttu-id="3ace7-116">用戶端檢查很容易規避。&dagger;</span><span class="sxs-lookup"><span data-stu-id="3ace7-116">Client-side checks are easy to circumvent.&dagger;</span></span>
* <span data-ttu-id="3ace7-117">檢查上傳的大小，並防止上傳超過預期的數目。&dagger;</span><span class="sxs-lookup"><span data-stu-id="3ace7-117">Check the size of the upload and prevent uploads that are larger than expected.&dagger;</span></span>
* <span data-ttu-id="3ace7-118">當使用相同名稱的上傳檔案覆寫檔案時，請在上傳檔案之前，檢查資料庫或實體儲存體的檔案名。</span><span class="sxs-lookup"><span data-stu-id="3ace7-118">When files shouldn't be overwritten by an uploaded file with the same name, check the file name against the database or physical storage before uploading the file.</span></span>
* <span data-ttu-id="3ace7-119">**在儲存檔案之前，在上傳的內容上執行病毒/惡意程式碼掃描器。**</span><span class="sxs-lookup"><span data-stu-id="3ace7-119">**Run a virus/malware scanner on uploaded content before the file is stored.**</span></span>

<span data-ttu-id="3ace7-120">&dagger;範例應用程式示範符合準則的方法。</span><span class="sxs-lookup"><span data-stu-id="3ace7-120">&dagger;The sample app demonstrates an approach that meets the criteria.</span></span>

> [!WARNING]
> <span data-ttu-id="3ace7-121">將惡意程式碼上傳至系統經常是執行程式碼的第一步，該程式碼可能：</span><span class="sxs-lookup"><span data-stu-id="3ace7-121">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
>
> * <span data-ttu-id="3ace7-122">完全侵占系統。</span><span class="sxs-lookup"><span data-stu-id="3ace7-122">Completely takeover a system.</span></span>
> * <span data-ttu-id="3ace7-123">使用系統損毀的結果來多載系統。</span><span class="sxs-lookup"><span data-stu-id="3ace7-123">Overload a system with the result that the system crashes.</span></span>
> * <span data-ttu-id="3ace7-124">洩漏使用者或系統資料。</span><span class="sxs-lookup"><span data-stu-id="3ace7-124">Compromise user or system data.</span></span>
> * <span data-ttu-id="3ace7-125">將刻套用至公用 UI。</span><span class="sxs-lookup"><span data-stu-id="3ace7-125">Apply graffiti to a public UI.</span></span>
>
> <span data-ttu-id="3ace7-126">如需在接受來自使用者的檔案時減少攻擊介面區的資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="3ace7-126">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * [<span data-ttu-id="3ace7-127">不受限制的檔案上傳</span><span class="sxs-lookup"><span data-stu-id="3ace7-127">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * [<span data-ttu-id="3ace7-128">Azure 安全性：請確保在接受來自使用者的檔案時，適當的控制項已就緒</span><span class="sxs-lookup"><span data-stu-id="3ace7-128">Azure Security: Ensure appropriate controls are in place when accepting files from users</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

<span data-ttu-id="3ace7-129">如需詳細資訊，請參閱[ASP.NET Core 中的上傳](https://docs.microsoft.com/aspnet/core/mvc/models/file-uploads)檔案。</span><span class="sxs-lookup"><span data-stu-id="3ace7-129">For additional information, see [Upload files in ASP.NET Core](https://docs.microsoft.com/aspnet/core/mvc/models/file-uploads).</span></span>

## <a name="how-to-use-the-sample"></a><span data-ttu-id="3ace7-130">如何使用範例</span><span class="sxs-lookup"><span data-stu-id="3ace7-130">How to use the sample</span></span>

<span data-ttu-id="3ace7-131">在*appsettings json*檔案中：</span><span class="sxs-lookup"><span data-stu-id="3ace7-131">In the *appsettings.json* file:</span></span>

1. <span data-ttu-id="3ace7-132">設定儲存檔案的路徑（`StoredFilesPath`）。</span><span class="sxs-lookup"><span data-stu-id="3ace7-132">Set the path for stored files (`StoredFilesPath`).</span></span>

   * <span data-ttu-id="3ace7-133">範例應用程式會將值設定為 `c:\\files`，這會假設名為*files*的資料夾存在於系統的 C：磁片磁碟機根目錄中。</span><span class="sxs-lookup"><span data-stu-id="3ace7-133">The sample app sets the value to `c:\\files`, which assumes that a folder named *files* exists at the system's C: drive root.</span></span>
   * <span data-ttu-id="3ace7-134">路徑必須存在。</span><span class="sxs-lookup"><span data-stu-id="3ace7-134">The path must exist.</span></span> <span data-ttu-id="3ace7-135">在系統的 C：磁片磁碟機上建立*files*資料夾，或將路徑設定為適當的位置。</span><span class="sxs-lookup"><span data-stu-id="3ace7-135">Create a *files* folder on the system's C: drive or set the path to a suitable location.</span></span>
   * <span data-ttu-id="3ace7-136">應用程式的進程需要路徑的讀取/寫入權限。</span><span class="sxs-lookup"><span data-stu-id="3ace7-136">The app's process requires read/write permissions to the path.</span></span>
   * <span data-ttu-id="3ace7-137">**重大!**</span><span class="sxs-lookup"><span data-stu-id="3ace7-137">**IMPORTANT!**</span></span> <span data-ttu-id="3ace7-138">停用路徑上所有使用者的執行許可權。</span><span class="sxs-lookup"><span data-stu-id="3ace7-138">Disable execute permissions for all users at the path.</span></span>

1. <span data-ttu-id="3ace7-139">設定檔案大小限制（`FileSizeLimit`）（以位元組為單位）。</span><span class="sxs-lookup"><span data-stu-id="3ace7-139">Set the file size limit (`FileSizeLimit`) in bytes.</span></span> <span data-ttu-id="3ace7-140">範例應用程式的預設值 `2097152` （2097152個位元組），最多允許上傳 2 MB 的檔案。</span><span class="sxs-lookup"><span data-stu-id="3ace7-140">The sample app's default value of `2097152` (2,097,152 bytes) permits file uploads up to 2 MB.</span></span>
