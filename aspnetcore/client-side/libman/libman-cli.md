---
title: 使用 ASP.NET Core 使用 LibMan 命令列介面 (CLI)
author: scottaddie
description: 了解如何在 ASP.NET Core 專案中使用 LibMan 命令列介面 (CLI)。
ms.author: scaddie
ms.custom: mvc
ms.date: 08/30/2018
uid: client-side/libman/libman-cli
ms.openlocfilehash: 5667f79648a60b8fd9496f8041ef08891ab766af
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64894755"
---
# <a name="use-the-libman-command-line-interface-cli-with-aspnet-core"></a><span data-ttu-id="98858-103">使用 ASP.NET Core 使用 LibMan 命令列介面 (CLI)</span><span class="sxs-lookup"><span data-stu-id="98858-103">Use the LibMan command-line interface (CLI) with ASP.NET Core</span></span>

<span data-ttu-id="98858-104">作者：[Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="98858-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="98858-105">[LibMan](xref:client-side/libman/index) CLI 是一種跨平台工具，支援.NET Core 支援的每個地方。</span><span class="sxs-lookup"><span data-stu-id="98858-105">The [LibMan](xref:client-side/libman/index) CLI is a cross-platform tool that's supported everywhere .NET Core is supported.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="98858-106">必要條件</span><span class="sxs-lookup"><span data-stu-id="98858-106">Prerequisites</span></span>

* [!INCLUDE [2.1-SDK](../../includes/2.1-SDK.md)]

## <a name="installation"></a><span data-ttu-id="98858-107">安裝</span><span class="sxs-lookup"><span data-stu-id="98858-107">Installation</span></span>

<span data-ttu-id="98858-108">若要安裝 LibMan CLI:</span><span class="sxs-lookup"><span data-stu-id="98858-108">To install the LibMan CLI:</span></span>

```console
dotnet tool install -g Microsoft.Web.LibraryManager.Cli
```

<span data-ttu-id="98858-109">A [.NET Core 全域工具](/dotnet/core/tools/global-tools#install-a-global-tool)從安裝[Microsoft.Web.LibraryManager.Cli](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Cli/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="98858-109">A [.NET Core Global Tool](/dotnet/core/tools/global-tools#install-a-global-tool) is installed from the [Microsoft.Web.LibraryManager.Cli](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Cli/) NuGet package.</span></span>

<span data-ttu-id="98858-110">若要從特定的 NuGet 套件來源安裝 LibMan CLI:</span><span class="sxs-lookup"><span data-stu-id="98858-110">To install the LibMan CLI from a specific NuGet package source:</span></span>

```console
dotnet tool install -g Microsoft.Web.LibraryManager.Cli --version 1.0.94-g606058a278 --add-source C:\Temp\
```

<span data-ttu-id="98858-111">在上述範例中，從本機的 Windows 電腦安裝.NET Core 全域工具*C:\Temp\Microsoft.Web.LibraryManager.Cli.1.0.94-g606058a278.nupkg*檔案。</span><span class="sxs-lookup"><span data-stu-id="98858-111">In the preceding example, a .NET Core Global Tool is installed from the local Windows machine's *C:\Temp\Microsoft.Web.LibraryManager.Cli.1.0.94-g606058a278.nupkg* file.</span></span>

## <a name="usage"></a><span data-ttu-id="98858-112">使用量</span><span class="sxs-lookup"><span data-stu-id="98858-112">Usage</span></span>

<span data-ttu-id="98858-113">安裝成功後的 cli，您可以使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="98858-113">After successful installation of the CLI, the following command can be used:</span></span>

```console
libman
```

<span data-ttu-id="98858-114">若要檢視已安裝的 CLI 版本：</span><span class="sxs-lookup"><span data-stu-id="98858-114">To view the installed CLI version:</span></span>

```console
libman --version
```

<span data-ttu-id="98858-115">若要檢視可用的 CLI 命令：</span><span class="sxs-lookup"><span data-stu-id="98858-115">To view the available CLI commands:</span></span>

```console
libman --help
```

<span data-ttu-id="98858-116">上述命令會顯示類似下列的輸出：</span><span class="sxs-lookup"><span data-stu-id="98858-116">The preceding command displays output similar to the following:</span></span>

```console
 1.0.163+g45474d37ed

Usage: libman [options] [command]

Options:
  --help|-h  Show help information
  --version  Show version information

Commands:
  cache      List or clean libman cache contents
  clean      Deletes all library files defined in libman.json from the project
  init       Create a new libman.json
  install    Add a library definition to the libman.json file, and download the 
             library to the specified location
  restore    Downloads all files from provider and saves them to specified 
             destination
  uninstall  Deletes all files for the specified library from their specified 
             destination, then removes the specified library definition from 
             libman.json
  update     Updates the specified library

Use "libman [command] --help" for more information about a command.
```

<span data-ttu-id="98858-117">下列各節說明可用的 CLI 命令。</span><span class="sxs-lookup"><span data-stu-id="98858-117">The following sections outline the available CLI commands.</span></span>

## <a name="initialize-libman-in-the-project"></a><span data-ttu-id="98858-118">在專案中初始化 LibMan</span><span class="sxs-lookup"><span data-stu-id="98858-118">Initialize LibMan in the project</span></span>

<span data-ttu-id="98858-119">`libman init`命令會建立*libman.json*檔案如果不存在。</span><span class="sxs-lookup"><span data-stu-id="98858-119">The `libman init` command creates a *libman.json* file if one doesn't exist.</span></span> <span data-ttu-id="98858-120">建立檔案是使用預設項目範本內容。</span><span class="sxs-lookup"><span data-stu-id="98858-120">The file is created with the default item template content.</span></span>

### <a name="synopsis"></a><span data-ttu-id="98858-121">概要</span><span class="sxs-lookup"><span data-stu-id="98858-121">Synopsis</span></span>

```console
libman init [-d|--default-destination] [-p|--default-provider] [--verbosity]
libman init [-h|--help]
```

### <a name="options"></a><span data-ttu-id="98858-122">選項</span><span class="sxs-lookup"><span data-stu-id="98858-122">Options</span></span>

<span data-ttu-id="98858-123">下列選項可供`libman init`命令：</span><span class="sxs-lookup"><span data-stu-id="98858-123">The following options are available for the `libman init` command:</span></span>

* `-d|--default-destination <PATH>`

  <span data-ttu-id="98858-124">相對於目前的資料夾路徑。</span><span class="sxs-lookup"><span data-stu-id="98858-124">A path relative to the current folder.</span></span> <span data-ttu-id="98858-125">如果沒有，將會安裝在此位置的程式庫檔案`destination`屬性定義在程式庫*libman.json*。</span><span class="sxs-lookup"><span data-stu-id="98858-125">Library files are installed in this location if no `destination` property is defined for a library in *libman.json*.</span></span> <span data-ttu-id="98858-126">`<PATH>`值會寫入至`defaultDestination`屬性*libman.json*。</span><span class="sxs-lookup"><span data-stu-id="98858-126">The `<PATH>` value is written to the `defaultDestination` property of *libman.json*.</span></span>

* `-p|--default-provider <PROVIDER>`

  <span data-ttu-id="98858-127">如果沒有提供者已定義特定的程式庫所使用的提供者。</span><span class="sxs-lookup"><span data-stu-id="98858-127">The provider to use if no provider is defined for a given library.</span></span> <span data-ttu-id="98858-128">`<PROVIDER>`值會寫入至`defaultProvider`屬性*libman.json*。</span><span class="sxs-lookup"><span data-stu-id="98858-128">The `<PROVIDER>` value is written to the `defaultProvider` property of *libman.json*.</span></span> <span data-ttu-id="98858-129">取代`<PROVIDER>`具有下列值之一：</span><span class="sxs-lookup"><span data-stu-id="98858-129">Replace `<PROVIDER>` with one of the following values:</span></span>

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="98858-130">範例</span><span class="sxs-lookup"><span data-stu-id="98858-130">Examples</span></span>

<span data-ttu-id="98858-131">若要建立*libman.json* ASP.NET Core 專案中的檔案：</span><span class="sxs-lookup"><span data-stu-id="98858-131">To create a *libman.json* file in an ASP.NET Core project:</span></span>

* <span data-ttu-id="98858-132">瀏覽至專案根目錄。</span><span class="sxs-lookup"><span data-stu-id="98858-132">Navigate to the project root.</span></span>
* <span data-ttu-id="98858-133">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="98858-133">Run the following command:</span></span>

  ```console
  libman init
  ```

* <span data-ttu-id="98858-134">輸入名稱的預設提供者或按`Enter`使用預設 CDNJS 提供者。</span><span class="sxs-lookup"><span data-stu-id="98858-134">Type the name of the default provider, or press `Enter` to use the default CDNJS provider.</span></span> <span data-ttu-id="98858-135">有效值包括：</span><span class="sxs-lookup"><span data-stu-id="98858-135">Valid values include:</span></span>

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  ![libman init 命令-預設提供者](_static/libman-init-provider.png)

<span data-ttu-id="98858-137">A *libman.json*檔案新增至專案根目錄中，以下列內容：</span><span class="sxs-lookup"><span data-stu-id="98858-137">A *libman.json* file is added to the project root with the following content:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

## <a name="add-library-files"></a><span data-ttu-id="98858-138">新增程式庫檔案</span><span class="sxs-lookup"><span data-stu-id="98858-138">Add library files</span></span>

<span data-ttu-id="98858-139">`libman install`命令會下載並安裝至專案的程式庫檔案。</span><span class="sxs-lookup"><span data-stu-id="98858-139">The `libman install` command downloads and installs library files into the project.</span></span> <span data-ttu-id="98858-140">A *libman.json*檔案會新增，如果不存在。</span><span class="sxs-lookup"><span data-stu-id="98858-140">A *libman.json* file is added if one doesn't exist.</span></span> <span data-ttu-id="98858-141">*Libman.json*檔案遭到修改儲存的程式庫檔案的組態詳細資料。</span><span class="sxs-lookup"><span data-stu-id="98858-141">The *libman.json* file is modified to store configuration details for the library files.</span></span>

### <a name="synopsis"></a><span data-ttu-id="98858-142">概要</span><span class="sxs-lookup"><span data-stu-id="98858-142">Synopsis</span></span>

```console
libman install <LIBRARY> [-d|--destination] [--files] [-p|--provider] [--verbosity]
libman install [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="98858-143">引數</span><span class="sxs-lookup"><span data-stu-id="98858-143">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="98858-144">若要安裝的程式庫名稱。</span><span class="sxs-lookup"><span data-stu-id="98858-144">The name of the library to install.</span></span> <span data-ttu-id="98858-145">此名稱可能包含版本號碼的標記法 (例如`@1.2.0`)。</span><span class="sxs-lookup"><span data-stu-id="98858-145">This name may include version number notation (for example, `@1.2.0`).</span></span>

### <a name="options"></a><span data-ttu-id="98858-146">選項</span><span class="sxs-lookup"><span data-stu-id="98858-146">Options</span></span>

<span data-ttu-id="98858-147">下列選項可供`libman install`命令：</span><span class="sxs-lookup"><span data-stu-id="98858-147">The following options are available for the `libman install` command:</span></span>

* `-d|--destination <PATH>`

  <span data-ttu-id="98858-148">要安裝的程式庫的位置。</span><span class="sxs-lookup"><span data-stu-id="98858-148">The location to install the library.</span></span> <span data-ttu-id="98858-149">如果未指定，則會使用預設位置。</span><span class="sxs-lookup"><span data-stu-id="98858-149">If not specified, the default location is used.</span></span> <span data-ttu-id="98858-150">如果沒有`defaultDestination`屬性中指定*libman.json*，這是必要選項。</span><span class="sxs-lookup"><span data-stu-id="98858-150">If no `defaultDestination` property is specified in *libman.json*, this option is required.</span></span>

* `--files <FILE>`

  <span data-ttu-id="98858-151">指定要從程式庫安裝的檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="98858-151">Specify the name of the file to install from the library.</span></span> <span data-ttu-id="98858-152">如果未指定，從程式庫的所有檔案會都安裝。</span><span class="sxs-lookup"><span data-stu-id="98858-152">If not specified, all files from the library are installed.</span></span> <span data-ttu-id="98858-153">提供一個`--files`安裝每個檔案的選項。</span><span class="sxs-lookup"><span data-stu-id="98858-153">Provide one `--files` option per file to be installed.</span></span> <span data-ttu-id="98858-154">也支援相對路徑。</span><span class="sxs-lookup"><span data-stu-id="98858-154">Relative paths are supported too.</span></span> <span data-ttu-id="98858-155">例如：`--files dist/browser/signalr.js`。</span><span class="sxs-lookup"><span data-stu-id="98858-155">For example: `--files dist/browser/signalr.js`.</span></span>

* `-p|--provider <PROVIDER>`

  <span data-ttu-id="98858-156">若要使用程式庫取得提供者的名稱。</span><span class="sxs-lookup"><span data-stu-id="98858-156">The name of the provider to use for the library acquisition.</span></span> <span data-ttu-id="98858-157">取代`<PROVIDER>`具有下列值之一：</span><span class="sxs-lookup"><span data-stu-id="98858-157">Replace `<PROVIDER>` with one of the following values:</span></span>
  
  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  <span data-ttu-id="98858-158">如果未指定，`defaultProvider`中的屬性*libman.json*用。</span><span class="sxs-lookup"><span data-stu-id="98858-158">If not specified, the `defaultProvider` property in *libman.json* is used.</span></span> <span data-ttu-id="98858-159">如果沒有`defaultProvider`屬性中指定*libman.json*，這是必要選項。</span><span class="sxs-lookup"><span data-stu-id="98858-159">If no `defaultProvider` property is specified in *libman.json*, this option is required.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="98858-160">範例</span><span class="sxs-lookup"><span data-stu-id="98858-160">Examples</span></span>

<span data-ttu-id="98858-161">請考慮下列*libman.json*檔案：</span><span class="sxs-lookup"><span data-stu-id="98858-161">Consider the following *libman.json* file:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

<span data-ttu-id="98858-162">若要安裝的 jQuery 版本 3.2.1 *jquery.min.js*的檔案*wwwroot/指令碼/jquery*使用 CDNJS 提供者的資料夾：</span><span class="sxs-lookup"><span data-stu-id="98858-162">To install the jQuery version 3.2.1 *jquery.min.js* file to the *wwwroot/scripts/jquery* folder using the CDNJS provider:</span></span>

```console
libman install jquery@3.2.1 --provider cdnjs --destination wwwroot/scripts/jquery --files jquery.min.js
```

<span data-ttu-id="98858-163">*Libman.json*檔案如下所示：</span><span class="sxs-lookup"><span data-stu-id="98858-163">The *libman.json* file resembles the following:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": [
    {
      "library": "jquery@3.2.1",
      "destination": "wwwroot/scripts/jquery",
      "files": [
        "jquery.min.js"
      ]
    }
  ]
}
```

<span data-ttu-id="98858-164">若要安裝*calendar.js*並*calendar.css*從檔案*c:\\temp\\contosoCalendar\\* 使用檔案系統提供者：</span><span class="sxs-lookup"><span data-stu-id="98858-164">To install the *calendar.js* and *calendar.css* files from *C:\\temp\\contosoCalendar\\* using the file system provider:</span></span>

  ```console
  libman install C:\temp\contosoCalendar\ --provider filesystem --files calendar.js --files calendar.css
  ```

<span data-ttu-id="98858-165">有兩個原因，顯示下列提示：</span><span class="sxs-lookup"><span data-stu-id="98858-165">The following prompt appears for two reasons:</span></span>

* <span data-ttu-id="98858-166">*Libman.json*檔案未包含`defaultDestination`屬性。</span><span class="sxs-lookup"><span data-stu-id="98858-166">The *libman.json* file doesn't contain a `defaultDestination` property.</span></span>
* <span data-ttu-id="98858-167">`libman install`命令不包含`-d|--destination`選項。</span><span class="sxs-lookup"><span data-stu-id="98858-167">The `libman install` command doesn't contain the `-d|--destination` option.</span></span>

![libman 安裝命令-目的地](_static/libman-install-destination.png)

<span data-ttu-id="98858-169">接受預設的目的地之後, *libman.json*檔案如下所示：</span><span class="sxs-lookup"><span data-stu-id="98858-169">After accepting the default destination, the *libman.json* file resembles the following:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": [
    {
      "library": "jquery@3.2.1",
      "destination": "wwwroot/scripts/jquery",
      "files": [
        "jquery.min.js"
      ]
    },
    {
      "library": "C:\\temp\\contosoCalendar\\",
      "provider": "filesystem",
      "destination": "wwwroot/lib/contosoCalendar",
      "files": [
        "calendar.js",
        "calendar.css"
      ]
    }
  ]
}
```

## <a name="restore-library-files"></a><span data-ttu-id="98858-170">將程式庫檔案還原</span><span class="sxs-lookup"><span data-stu-id="98858-170">Restore library files</span></span>

<span data-ttu-id="98858-171">`libman restore`命令會在安裝程式庫檔案中定義*libman.json*。</span><span class="sxs-lookup"><span data-stu-id="98858-171">The `libman restore` command installs library files defined in *libman.json*.</span></span> <span data-ttu-id="98858-172">可套用下列規則：</span><span class="sxs-lookup"><span data-stu-id="98858-172">The following rules apply:</span></span>

* <span data-ttu-id="98858-173">如果沒有*libman.json*檔案位於專案根目錄，則會傳回錯誤。</span><span class="sxs-lookup"><span data-stu-id="98858-173">If no *libman.json* file exists in the project root, an error is returned.</span></span>
* <span data-ttu-id="98858-174">如果程式庫指定提供者，`defaultProvider`中的屬性*libman.json*會被忽略。</span><span class="sxs-lookup"><span data-stu-id="98858-174">If a library specifies a provider, the `defaultProvider` property in *libman.json* is ignored.</span></span>
* <span data-ttu-id="98858-175">如果程式庫指定目的地`defaultDestination`中的屬性*libman.json*會被忽略。</span><span class="sxs-lookup"><span data-stu-id="98858-175">If a library specifies a destination, the `defaultDestination` property in *libman.json* is ignored.</span></span>

### <a name="synopsis"></a><span data-ttu-id="98858-176">概要</span><span class="sxs-lookup"><span data-stu-id="98858-176">Synopsis</span></span>

```console
libman restore [--verbosity]
libman restore [-h|--help]
```

### <a name="options"></a><span data-ttu-id="98858-177">選項</span><span class="sxs-lookup"><span data-stu-id="98858-177">Options</span></span>

<span data-ttu-id="98858-178">下列選項可供`libman restore`命令：</span><span class="sxs-lookup"><span data-stu-id="98858-178">The following options are available for the `libman restore` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="98858-179">範例</span><span class="sxs-lookup"><span data-stu-id="98858-179">Examples</span></span>

<span data-ttu-id="98858-180">若要還原程式庫檔案中定義*libman.json*:</span><span class="sxs-lookup"><span data-stu-id="98858-180">To restore the library files defined in *libman.json*:</span></span>

```console
libman restore
```

## <a name="delete-library-files"></a><span data-ttu-id="98858-181">刪除程式庫檔案</span><span class="sxs-lookup"><span data-stu-id="98858-181">Delete library files</span></span>

<span data-ttu-id="98858-182">`libman clean`命令會刪除先前還原透過 LibMan 的程式庫檔案。</span><span class="sxs-lookup"><span data-stu-id="98858-182">The `libman clean` command deletes library files previously restored via LibMan.</span></span> <span data-ttu-id="98858-183">這項作業後會變成空的資料夾被刪除。</span><span class="sxs-lookup"><span data-stu-id="98858-183">Folders that become empty after this operation are deleted.</span></span> <span data-ttu-id="98858-184">相關聯的程式庫檔案中的組態`libraries`的屬性*libman.json*不會被移除。</span><span class="sxs-lookup"><span data-stu-id="98858-184">The library files' associated configurations in the `libraries` property of *libman.json* aren't removed.</span></span>

### <a name="synopsis"></a><span data-ttu-id="98858-185">概要</span><span class="sxs-lookup"><span data-stu-id="98858-185">Synopsis</span></span>

```console
libman clean [--verbosity]
libman clean [-h|--help]
```

### <a name="options"></a><span data-ttu-id="98858-186">選項</span><span class="sxs-lookup"><span data-stu-id="98858-186">Options</span></span>

<span data-ttu-id="98858-187">下列選項可供`libman clean`命令：</span><span class="sxs-lookup"><span data-stu-id="98858-187">The following options are available for the `libman clean` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="98858-188">範例</span><span class="sxs-lookup"><span data-stu-id="98858-188">Examples</span></span>

<span data-ttu-id="98858-189">若要刪除透過 LibMan 安裝的程式庫檔案：</span><span class="sxs-lookup"><span data-stu-id="98858-189">To delete library files installed via LibMan:</span></span>

```console
libman clean
```

## <a name="uninstall-library-files"></a><span data-ttu-id="98858-190">解除安裝程式庫檔案</span><span class="sxs-lookup"><span data-stu-id="98858-190">Uninstall library files</span></span>

<span data-ttu-id="98858-191">`libman uninstall`命令：</span><span class="sxs-lookup"><span data-stu-id="98858-191">The `libman uninstall` command:</span></span>

* <span data-ttu-id="98858-192">刪除指定的程式庫中的目的地從相關聯的所有檔案*libman.json*。</span><span class="sxs-lookup"><span data-stu-id="98858-192">Deletes all files associated with the specified library from the destination in *libman.json*.</span></span>
* <span data-ttu-id="98858-193">移除相關聯的文件庫設定*libman.json*。</span><span class="sxs-lookup"><span data-stu-id="98858-193">Removes the associated library configuration from *libman.json*.</span></span>

<span data-ttu-id="98858-194">發生錯誤時：</span><span class="sxs-lookup"><span data-stu-id="98858-194">An error occurs when:</span></span>

* <span data-ttu-id="98858-195">否*libman.json*檔案位於專案根目錄。</span><span class="sxs-lookup"><span data-stu-id="98858-195">No *libman.json* file exists in the project root.</span></span>
* <span data-ttu-id="98858-196">指定的程式庫不存在。</span><span class="sxs-lookup"><span data-stu-id="98858-196">The specified library doesn't exist.</span></span>

<span data-ttu-id="98858-197">如果已安裝具有相同名稱的多個程式庫，系統會提示您選擇其中一個。</span><span class="sxs-lookup"><span data-stu-id="98858-197">If more than one library with the same name is installed, you're prompted to choose one.</span></span>

### <a name="synopsis"></a><span data-ttu-id="98858-198">概要</span><span class="sxs-lookup"><span data-stu-id="98858-198">Synopsis</span></span>

```console
libman uninstall <LIBRARY> [--verbosity]
libman uninstall [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="98858-199">引數</span><span class="sxs-lookup"><span data-stu-id="98858-199">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="98858-200">若要解除安裝程式庫的名稱。</span><span class="sxs-lookup"><span data-stu-id="98858-200">The name of the library to uninstall.</span></span> <span data-ttu-id="98858-201">此名稱可能包含版本號碼的標記法 (例如`@1.2.0`)。</span><span class="sxs-lookup"><span data-stu-id="98858-201">This name may include version number notation (for example, `@1.2.0`).</span></span>

### <a name="options"></a><span data-ttu-id="98858-202">選項</span><span class="sxs-lookup"><span data-stu-id="98858-202">Options</span></span>

<span data-ttu-id="98858-203">下列選項可供`libman uninstall`命令：</span><span class="sxs-lookup"><span data-stu-id="98858-203">The following options are available for the `libman uninstall` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="98858-204">範例</span><span class="sxs-lookup"><span data-stu-id="98858-204">Examples</span></span>

<span data-ttu-id="98858-205">請考慮下列*libman.json*檔案：</span><span class="sxs-lookup"><span data-stu-id="98858-205">Consider the following *libman.json* file:</span></span>

[!code-json[](samples/LibManSample/libman.json)]

* <span data-ttu-id="98858-206">若要解除安裝 jQuery，其中一個下列的命令會成功：</span><span class="sxs-lookup"><span data-stu-id="98858-206">To uninstall jQuery, either of the following commands succeed:</span></span>

  ```console
  libman uninstall jquery
  ```

  ```console
  libman uninstall jquery@3.3.1
  ```

* <span data-ttu-id="98858-207">若要解除安裝安裝透過 Lodash 檔案`filesystem`提供者：</span><span class="sxs-lookup"><span data-stu-id="98858-207">To uninstall the Lodash files installed via the `filesystem` provider:</span></span>

  ```console
  libman uninstall C:\temp\lodash\
  ```

## <a name="update-library-version"></a><span data-ttu-id="98858-208">更新程式庫版本</span><span class="sxs-lookup"><span data-stu-id="98858-208">Update library version</span></span>

<span data-ttu-id="98858-209">`libman update`命令會更新文件庫透過 LibMan 安裝至指定的版本。</span><span class="sxs-lookup"><span data-stu-id="98858-209">The `libman update` command updates a library installed via LibMan to the specified version.</span></span>

<span data-ttu-id="98858-210">發生錯誤時：</span><span class="sxs-lookup"><span data-stu-id="98858-210">An error occurs when:</span></span>

* <span data-ttu-id="98858-211">否*libman.json*檔案位於專案根目錄。</span><span class="sxs-lookup"><span data-stu-id="98858-211">No *libman.json* file exists in the project root.</span></span>
* <span data-ttu-id="98858-212">指定的程式庫不存在。</span><span class="sxs-lookup"><span data-stu-id="98858-212">The specified library doesn't exist.</span></span>

<span data-ttu-id="98858-213">如果已安裝具有相同名稱的多個程式庫，系統會提示您選擇其中一個。</span><span class="sxs-lookup"><span data-stu-id="98858-213">If more than one library with the same name is installed, you're prompted to choose one.</span></span>

### <a name="synopsis"></a><span data-ttu-id="98858-214">概要</span><span class="sxs-lookup"><span data-stu-id="98858-214">Synopsis</span></span>

```console
libman update <LIBRARY> [-pre] [--to] [--verbosity]
libman update [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="98858-215">引數</span><span class="sxs-lookup"><span data-stu-id="98858-215">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="98858-216">要更新的程式庫名稱。</span><span class="sxs-lookup"><span data-stu-id="98858-216">The name of the library to update.</span></span>

### <a name="options"></a><span data-ttu-id="98858-217">選項</span><span class="sxs-lookup"><span data-stu-id="98858-217">Options</span></span>

<span data-ttu-id="98858-218">下列選項可供`libman update`命令：</span><span class="sxs-lookup"><span data-stu-id="98858-218">The following options are available for the `libman update` command:</span></span>

* `-pre`

  <span data-ttu-id="98858-219">取得程式庫的最新發行前版本。</span><span class="sxs-lookup"><span data-stu-id="98858-219">Obtain the latest prerelease version of the library.</span></span>

* `--to <VERSION>`

  <span data-ttu-id="98858-220">取得特定版本的程式庫。</span><span class="sxs-lookup"><span data-stu-id="98858-220">Obtain a specific version of the library.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="98858-221">範例</span><span class="sxs-lookup"><span data-stu-id="98858-221">Examples</span></span>

* <span data-ttu-id="98858-222">若要更新為最新版本的 jQuery:</span><span class="sxs-lookup"><span data-stu-id="98858-222">To update jQuery to the latest version:</span></span>

  ```console
  libman update jquery
  ```

* <span data-ttu-id="98858-223">若要更新 jQuery 版本 3.3.1:</span><span class="sxs-lookup"><span data-stu-id="98858-223">To update jQuery to version 3.3.1:</span></span>

  ```console
  libman update jquery --to 3.3.1
  ```

* <span data-ttu-id="98858-224">若要更新為最新的發行前版本的 jQuery:</span><span class="sxs-lookup"><span data-stu-id="98858-224">To update jQuery to the latest prerelease version:</span></span>

  ```console
  libman update jquery -pre
  ```

## <a name="manage-library-cache"></a><span data-ttu-id="98858-225">管理程式庫快取</span><span class="sxs-lookup"><span data-stu-id="98858-225">Manage library cache</span></span>

<span data-ttu-id="98858-226">`libman cache`命令管理 LibMan 程式庫快取。</span><span class="sxs-lookup"><span data-stu-id="98858-226">The `libman cache` command manages the LibMan library cache.</span></span> <span data-ttu-id="98858-227">`filesystem`提供者不使用程式庫快取。</span><span class="sxs-lookup"><span data-stu-id="98858-227">The `filesystem` provider doesn't use the library cache.</span></span>

### <a name="synopsis"></a><span data-ttu-id="98858-228">概要</span><span class="sxs-lookup"><span data-stu-id="98858-228">Synopsis</span></span>

```console
libman cache clean [<PROVIDER>] [--verbosity]
libman cache list [--files] [--libraries] [--verbosity]
libman cache [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="98858-229">引數</span><span class="sxs-lookup"><span data-stu-id="98858-229">Arguments</span></span>

`PROVIDER`

<span data-ttu-id="98858-230">僅能搭配`clean`命令。</span><span class="sxs-lookup"><span data-stu-id="98858-230">Only used with the `clean` command.</span></span> <span data-ttu-id="98858-231">指定提供者快取清除。</span><span class="sxs-lookup"><span data-stu-id="98858-231">Specifies the provider cache to clean.</span></span> <span data-ttu-id="98858-232">有效值包括：</span><span class="sxs-lookup"><span data-stu-id="98858-232">Valid values include:</span></span>

[!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

### <a name="options"></a><span data-ttu-id="98858-233">選項</span><span class="sxs-lookup"><span data-stu-id="98858-233">Options</span></span>

<span data-ttu-id="98858-234">下列選項可供`libman cache`命令：</span><span class="sxs-lookup"><span data-stu-id="98858-234">The following options are available for the `libman cache` command:</span></span>

* `--files`

  <span data-ttu-id="98858-235">列出快取的檔案的名稱。</span><span class="sxs-lookup"><span data-stu-id="98858-235">List the names of files that are cached.</span></span>

* `--libraries`

  <span data-ttu-id="98858-236">列出快取程式庫名稱。</span><span class="sxs-lookup"><span data-stu-id="98858-236">List the names of libraries that are cached.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="98858-237">範例</span><span class="sxs-lookup"><span data-stu-id="98858-237">Examples</span></span>

* <span data-ttu-id="98858-238">若要檢視每個提供者的快取程式庫名稱，使用下列命令之一：</span><span class="sxs-lookup"><span data-stu-id="98858-238">To view the names of cached libraries per provider, use one of the following commands:</span></span>

  ```console
  libman cache list
  ```

  ```console
  libman cache list --libraries
  ```

  <span data-ttu-id="98858-239">會顯示類似下列的輸出：</span><span class="sxs-lookup"><span data-stu-id="98858-239">Output similar to the following is displayed:</span></span>

  ```console
  Cache contents:
  ---------------
  unpkg:
      knockout
      react
      vue
  cdnjs:
      font-awesome
      jquery
      knockout
      lodash.js
      react
  ```

* <span data-ttu-id="98858-240">若要檢視每個提供者的快取程式庫檔案的名稱：</span><span class="sxs-lookup"><span data-stu-id="98858-240">To view the names of cached library files per provider:</span></span>

  ```console
  libman cache list --files
  ```

  <span data-ttu-id="98858-241">會顯示類似下列的輸出：</span><span class="sxs-lookup"><span data-stu-id="98858-241">Output similar to the following is displayed:</span></span>

  ```console
  Cache contents:
  ---------------
  unpkg:
      knockout:
          <list omitted for brevity>
      react:
          <list omitted for brevity>
      vue:
          <list omitted for brevity>
  cdnjs:
      font-awesome
          metadata.json
      jquery
          metadata.json
          3.2.1\core.js
          3.2.1\jquery.js
          3.2.1\jquery.min.js
          3.2.1\jquery.min.map
          3.2.1\jquery.slim.js
          3.2.1\jquery.slim.min.js
          3.2.1\jquery.slim.min.map
          3.3.1\core.js
          3.3.1\jquery.js
          3.3.1\jquery.min.js
          3.3.1\jquery.min.map
          3.3.1\jquery.slim.js
          3.3.1\jquery.slim.min.js
          3.3.1\jquery.slim.min.map
      knockout
          metadata.json
          3.4.2\knockout-debug.js
          3.4.2\knockout-min.js
      lodash.js
          metadata.json
          4.17.10\lodash.js
          4.17.10\lodash.min.js
      react
          metadata.json
  ```

  <span data-ttu-id="98858-242">請注意上述的輸出顯示的 jQuery 版本 3.2.1 和 3.3.1 CDNJS 提供者下快取。</span><span class="sxs-lookup"><span data-stu-id="98858-242">Notice the preceding output shows that jQuery versions 3.2.1 and 3.3.1 are cached under the CDNJS provider.</span></span>

* <span data-ttu-id="98858-243">若要清空 CDNJS 提供者的程式庫快取：</span><span class="sxs-lookup"><span data-stu-id="98858-243">To empty the library cache for the CDNJS provider:</span></span>

  ```console
  libman cache clean cdnjs
  ```

  <span data-ttu-id="98858-244">之後清空 CDNJS 提供者快取，`libman cache list`命令會顯示下列：</span><span class="sxs-lookup"><span data-stu-id="98858-244">After emptying the CDNJS provider cache, the `libman cache list` command displays the following:</span></span>

  ```console
  Cache contents:
  ---------------
  unpkg:
      knockout
      react
      vue
  cdnjs:
      (empty)
  ```

* <span data-ttu-id="98858-245">若要清空快取所有支援的提供者：</span><span class="sxs-lookup"><span data-stu-id="98858-245">To empty the cache for all supported providers:</span></span>

  ```console
  libman cache clean
  ```

  <span data-ttu-id="98858-246">之後清空所有提供者快取，`libman cache list`命令會顯示下列：</span><span class="sxs-lookup"><span data-stu-id="98858-246">After emptying all provider caches, the `libman cache list` command displays the following:</span></span>

  ```console
  Cache contents:
  ---------------
  unpkg:
      (empty)
  cdnjs:
      (empty)
  ```

## <a name="additional-resources"></a><span data-ttu-id="98858-247">其他資源</span><span class="sxs-lookup"><span data-stu-id="98858-247">Additional resources</span></span>

* [<span data-ttu-id="98858-248">安裝的通用工具</span><span class="sxs-lookup"><span data-stu-id="98858-248">Install a Global Tool</span></span>](/dotnet/core/tools/global-tools#install-a-global-tool)
* <xref:client-side/libman/libman-vs>
* [<span data-ttu-id="98858-249">LibMan GitHub 存放庫</span><span class="sxs-lookup"><span data-stu-id="98858-249">LibMan GitHub repository</span></span>](https://github.com/aspnet/LibraryManager)
