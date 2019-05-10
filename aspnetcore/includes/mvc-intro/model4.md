<span data-ttu-id="69d4a-101">下表詳細列出 ASP.NET Core 程式碼產生器參數：</span><span class="sxs-lookup"><span data-stu-id="69d4a-101">The following table details the ASP.NET Core code generator parameters:</span></span>

| <span data-ttu-id="69d4a-102">參數</span><span class="sxs-lookup"><span data-stu-id="69d4a-102">Parameter</span></span>               | <span data-ttu-id="69d4a-103">說明</span><span class="sxs-lookup"><span data-stu-id="69d4a-103">Description</span></span>|
| ----------------- | ------------ |
| <span data-ttu-id="69d4a-104">-m</span><span class="sxs-lookup"><span data-stu-id="69d4a-104">-m</span></span>  | <span data-ttu-id="69d4a-105">模型的名稱。</span><span class="sxs-lookup"><span data-stu-id="69d4a-105">The name of the model.</span></span> |
| <span data-ttu-id="69d4a-106">-dc</span><span class="sxs-lookup"><span data-stu-id="69d4a-106">-dc</span></span>  | <span data-ttu-id="69d4a-107">資料內容。</span><span class="sxs-lookup"><span data-stu-id="69d4a-107">The data context.</span></span> |
| <span data-ttu-id="69d4a-108">-udl</span><span class="sxs-lookup"><span data-stu-id="69d4a-108">-udl</span></span> | <span data-ttu-id="69d4a-109">使用預設的配置。</span><span class="sxs-lookup"><span data-stu-id="69d4a-109">Use the default layout.</span></span> |
| <span data-ttu-id="69d4a-110">--relativeFolderPath</span><span class="sxs-lookup"><span data-stu-id="69d4a-110">--relativeFolderPath</span></span> | <span data-ttu-id="69d4a-111">要建立檢視的相對輸出資料夾路徑。</span><span class="sxs-lookup"><span data-stu-id="69d4a-111">The relative output folder path to create the views.</span></span> |
| <span data-ttu-id="69d4a-112">--useDefaultLayout</span><span class="sxs-lookup"><span data-stu-id="69d4a-112">--useDefaultLayout</span></span> | <span data-ttu-id="69d4a-113">應針對檢視使用預設的配置。</span><span class="sxs-lookup"><span data-stu-id="69d4a-113">The default layout should be used for the views.</span></span> |
| <span data-ttu-id="69d4a-114">--referenceScriptLibraries</span><span class="sxs-lookup"><span data-stu-id="69d4a-114">--referenceScriptLibraries</span></span> | <span data-ttu-id="69d4a-115">將 `_ValidationScriptsPartial` 新增至 Edit 和 Create 頁面</span><span class="sxs-lookup"><span data-stu-id="69d4a-115">Adds `_ValidationScriptsPartial` to Edit and Create pages</span></span> |

<span data-ttu-id="69d4a-116">使用 `h` 參數取得 `aspnet-codegenerator controller` 命令的說明：</span><span class="sxs-lookup"><span data-stu-id="69d4a-116">Use the `h` switch to get help on the `aspnet-codegenerator controller` command:</span></span>

```console
dotnet aspnet-codegenerator controller -h
```