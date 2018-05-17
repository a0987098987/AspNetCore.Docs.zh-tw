<span data-ttu-id="24504-101">下表詳細列出 ASP.NET Core 程式碼產生器的參數：</span><span class="sxs-lookup"><span data-stu-id="24504-101">The following table details the ASP.NET Core code generators\` parameters:</span></span>

| <span data-ttu-id="24504-102">參數</span><span class="sxs-lookup"><span data-stu-id="24504-102">Parameter</span></span>               | <span data-ttu-id="24504-103">描述</span><span class="sxs-lookup"><span data-stu-id="24504-103">Description</span></span>|
| ----------------- | ------------ |
| <span data-ttu-id="24504-104">-m</span><span class="sxs-lookup"><span data-stu-id="24504-104">-m</span></span>  | <span data-ttu-id="24504-105">模型的名稱。</span><span class="sxs-lookup"><span data-stu-id="24504-105">The name of the model.</span></span> |
| <span data-ttu-id="24504-106">-dc</span><span class="sxs-lookup"><span data-stu-id="24504-106">-dc</span></span>  | <span data-ttu-id="24504-107">資料內容。</span><span class="sxs-lookup"><span data-stu-id="24504-107">The data context.</span></span> |
| <span data-ttu-id="24504-108">-udl</span><span class="sxs-lookup"><span data-stu-id="24504-108">-udl</span></span> | <span data-ttu-id="24504-109">使用預設的配置。</span><span class="sxs-lookup"><span data-stu-id="24504-109">Use the default layout.</span></span> |
| <span data-ttu-id="24504-110">-outDir</span><span class="sxs-lookup"><span data-stu-id="24504-110">-outDir</span></span> | <span data-ttu-id="24504-111">要建立檢視的相對輸出資料夾路徑。</span><span class="sxs-lookup"><span data-stu-id="24504-111">The relative output folder path to create the views.</span></span> |
| <span data-ttu-id="24504-112">--referenceScriptLibraries</span><span class="sxs-lookup"><span data-stu-id="24504-112">--referenceScriptLibraries</span></span> | <span data-ttu-id="24504-113">將 `_ValidationScriptsPartial` 新增至 Edit 和 Create 頁面</span><span class="sxs-lookup"><span data-stu-id="24504-113">Adds `_ValidationScriptsPartial` to Edit and Create pages</span></span> |

<span data-ttu-id="24504-114">使用 `h` 參數取得 `aspnet-codegenerator razorpage` 命令的說明：</span><span class="sxs-lookup"><span data-stu-id="24504-114">Use the `h` switch to get help on the `aspnet-codegenerator razorpage` command:</span></span>

```console
dotnet aspnet-codegenerator razorpage -h
```
<a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="24504-115">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="24504-115">Test the app</span></span>

* <span data-ttu-id="24504-116">執行應用程式，並將 `/Movies` 附加至瀏覽器中的 URL (`http://localhost:port/movies`)。</span><span class="sxs-lookup"><span data-stu-id="24504-116">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="24504-117">測試 **Create** 連結。</span><span class="sxs-lookup"><span data-stu-id="24504-117">Test the **Create** link.</span></span>

  ![建立頁面](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="24504-119">測試 **Edit**、**Details** 和 **Delete** 連結。</span><span class="sxs-lookup"><span data-stu-id="24504-119">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="24504-120">如果您收到類似下列的錯誤，請確認您已執行移轉並更新資料庫：</span><span class="sxs-lookup"><span data-stu-id="24504-120">If you get the error similar to the following, verify you have run migrations and updated the database:</span></span>

```
An unhandled exception occurred while processing the request.
'no such table: Movie'.
```
