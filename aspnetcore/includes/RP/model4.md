<a name="codegenerator"></a> <span data-ttu-id="a96e7-101">下表詳細列出 ASP.NET Core 程式碼產生器參數：</span><span class="sxs-lookup"><span data-stu-id="a96e7-101">The following table details the ASP.NET Core code generator parameters:</span></span>

| <span data-ttu-id="a96e7-102">參數</span><span class="sxs-lookup"><span data-stu-id="a96e7-102">Parameter</span></span>               | <span data-ttu-id="a96e7-103">描述</span><span class="sxs-lookup"><span data-stu-id="a96e7-103">Description</span></span>|
| ----------------- | ------------ |
| <span data-ttu-id="a96e7-104">-M</span><span class="sxs-lookup"><span data-stu-id="a96e7-104">-m</span></span>  | <span data-ttu-id="a96e7-105">模型的名稱。</span><span class="sxs-lookup"><span data-stu-id="a96e7-105">The name of the model.</span></span> |
| <span data-ttu-id="a96e7-106">-dc</span><span class="sxs-lookup"><span data-stu-id="a96e7-106">-dc</span></span>  | <span data-ttu-id="a96e7-107">要使用的 `DbContext` 類別。</span><span class="sxs-lookup"><span data-stu-id="a96e7-107">The `DbContext` class to use.</span></span> |
| <span data-ttu-id="a96e7-108">-udl</span><span class="sxs-lookup"><span data-stu-id="a96e7-108">-udl</span></span> | <span data-ttu-id="a96e7-109">使用預設的配置。</span><span class="sxs-lookup"><span data-stu-id="a96e7-109">Use the default layout.</span></span> |
| <span data-ttu-id="a96e7-110">-outDir</span><span class="sxs-lookup"><span data-stu-id="a96e7-110">-outDir</span></span> | <span data-ttu-id="a96e7-111">要建立檢視的相對輸出資料夾路徑。</span><span class="sxs-lookup"><span data-stu-id="a96e7-111">The relative output folder path to create the views.</span></span> |
| <span data-ttu-id="a96e7-112">--referenceScriptLibraries</span><span class="sxs-lookup"><span data-stu-id="a96e7-112">--referenceScriptLibraries</span></span> | <span data-ttu-id="a96e7-113">將 `_ValidationScriptsPartial` 新增至 Edit 和 Create 頁面</span><span class="sxs-lookup"><span data-stu-id="a96e7-113">Adds `_ValidationScriptsPartial` to Edit and Create pages</span></span> |

<span data-ttu-id="a96e7-114">使用 `h` 參數取得 `aspnet-codegenerator razorpage` 命令的說明：</span><span class="sxs-lookup"><span data-stu-id="a96e7-114">Use the `h` switch to get help on the `aspnet-codegenerator razorpage` command:</span></span>

```dotnetcli
dotnet aspnet-codegenerator razorpage -h
```

<span data-ttu-id="a96e7-115">如需詳細資訊，請參閱[dotnet aspnet-codegenerator](xref:fundamentals/tools/dotnet-aspnet-codegenerator)。</span><span class="sxs-lookup"><span data-stu-id="a96e7-115">For more information, see [dotnet aspnet-codegenerator](xref:fundamentals/tools/dotnet-aspnet-codegenerator).</span></span>
