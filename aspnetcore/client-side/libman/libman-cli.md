---
title: 搭配 ASP.NET Core 使用 LibMan CLI
author: scottaddie
description: 瞭解如何在 ASP.NET Core 專案中使用 LibMan CLI。
ms.author: scaddie
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: client-side/libman/libman-cli
ms.openlocfilehash: ed5dffb83a2f1a40f3d6596d23135c0fa5b6791f
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85403179"
---
# <a name="use-the-libman-cli-with-aspnet-core"></a><span data-ttu-id="48614-103">搭配 ASP.NET Core 使用 LibMan CLI</span><span class="sxs-lookup"><span data-stu-id="48614-103">Use the LibMan CLI with ASP.NET Core</span></span>

<span data-ttu-id="48614-104">作者：[Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="48614-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="48614-105">[LibMan](xref:client-side/libman/index) CLI 是一種跨平臺工具，支援 .net Core 的任何位置。</span><span class="sxs-lookup"><span data-stu-id="48614-105">The [LibMan](xref:client-side/libman/index) CLI is a cross-platform tool that's supported everywhere .NET Core is supported.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="48614-106">必要條件</span><span class="sxs-lookup"><span data-stu-id="48614-106">Prerequisites</span></span>

* [!INCLUDE [2.1-SDK](../../includes/2.1-SDK.md)]

## <a name="installation"></a><span data-ttu-id="48614-107">安裝</span><span class="sxs-lookup"><span data-stu-id="48614-107">Installation</span></span>

<span data-ttu-id="48614-108">若要安裝 LibMan CLI：</span><span class="sxs-lookup"><span data-stu-id="48614-108">To install the LibMan CLI:</span></span>

```dotnetcli
dotnet tool install -g Microsoft.Web.LibraryManager.Cli
```

<span data-ttu-id="48614-109">[.Net Core 通用工具](/dotnet/core/tools/global-tools#install-a-global-tool)是從[LibraryManager](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Cli/)安裝而來。</span><span class="sxs-lookup"><span data-stu-id="48614-109">A [.NET Core Global Tool](/dotnet/core/tools/global-tools#install-a-global-tool) is installed from the [Microsoft.Web.LibraryManager.Cli](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Cli/) NuGet package.</span></span>

<span data-ttu-id="48614-110">若要從特定的 NuGet 套件來源安裝 LibMan CLI：</span><span class="sxs-lookup"><span data-stu-id="48614-110">To install the LibMan CLI from a specific NuGet package source:</span></span>

```dotnetcli
dotnet tool install -g Microsoft.Web.LibraryManager.Cli --version 1.0.94-g606058a278 --add-source C:\Temp\
```

<span data-ttu-id="48614-111">在上述範例中，.NET Core 通用工具是從本機 Windows 電腦的*C:\Temp\Microsoft.Web.LibraryManager.Cli.1.0.94-g606058a278.nupkg*檔案進行安裝。</span><span class="sxs-lookup"><span data-stu-id="48614-111">In the preceding example, a .NET Core Global Tool is installed from the local Windows machine's *C:\Temp\Microsoft.Web.LibraryManager.Cli.1.0.94-g606058a278.nupkg* file.</span></span>

## <a name="usage"></a><span data-ttu-id="48614-112">使用量</span><span class="sxs-lookup"><span data-stu-id="48614-112">Usage</span></span>

<span data-ttu-id="48614-113">成功安裝 CLI 之後，您可以使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="48614-113">After successful installation of the CLI, the following command can be used:</span></span>

```console
libman
```

<span data-ttu-id="48614-114">若要查看已安裝的 CLI 版本：</span><span class="sxs-lookup"><span data-stu-id="48614-114">To view the installed CLI version:</span></span>

```console
libman --version
```

<span data-ttu-id="48614-115">若要查看可用的 CLI 命令：</span><span class="sxs-lookup"><span data-stu-id="48614-115">To view the available CLI commands:</span></span>

```console
libman --help
```

<span data-ttu-id="48614-116">上述命令會顯示類似下列的輸出：</span><span class="sxs-lookup"><span data-stu-id="48614-116">The preceding command displays output similar to the following:</span></span>

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

<span data-ttu-id="48614-117">下列各節將概述可用的 CLI 命令。</span><span class="sxs-lookup"><span data-stu-id="48614-117">The following sections outline the available CLI commands.</span></span>

## <a name="initialize-libman-in-the-project"></a><span data-ttu-id="48614-118">初始化專案中的 LibMan</span><span class="sxs-lookup"><span data-stu-id="48614-118">Initialize LibMan in the project</span></span>

<span data-ttu-id="48614-119">此 `libman init` 命令會*在檔案上建立libman.js* （如果不存在的話）。</span><span class="sxs-lookup"><span data-stu-id="48614-119">The `libman init` command creates a *libman.json* file if one doesn't exist.</span></span> <span data-ttu-id="48614-120">使用預設專案範本內容建立檔案。</span><span class="sxs-lookup"><span data-stu-id="48614-120">The file is created with the default item template content.</span></span>

### <a name="synopsis"></a><span data-ttu-id="48614-121">概要</span><span class="sxs-lookup"><span data-stu-id="48614-121">Synopsis</span></span>

```console
libman init [-d|--default-destination] [-p|--default-provider] [--verbosity]
libman init [-h|--help]
```

### <a name="options"></a><span data-ttu-id="48614-122">選項</span><span class="sxs-lookup"><span data-stu-id="48614-122">Options</span></span>

<span data-ttu-id="48614-123">以下是使用 `libman init` 命令時可用的選項：</span><span class="sxs-lookup"><span data-stu-id="48614-123">The following options are available for the `libman init` command:</span></span>

* `-d|--default-destination <PATH>`

  <span data-ttu-id="48614-124">相對於目前資料夾的路徑。</span><span class="sxs-lookup"><span data-stu-id="48614-124">A path relative to the current folder.</span></span> <span data-ttu-id="48614-125">如果libman.js中的程式庫未定義任何 `destination` 屬性，則會將程式庫*libman.json*檔案安裝在這個位置。</span><span class="sxs-lookup"><span data-stu-id="48614-125">Library files are installed in this location if no `destination` property is defined for a library in *libman.json*.</span></span> <span data-ttu-id="48614-126">此 `<PATH>` 值會寫入至 `defaultDestination` *上libman.js*的屬性。</span><span class="sxs-lookup"><span data-stu-id="48614-126">The `<PATH>` value is written to the `defaultDestination` property of *libman.json*.</span></span>

* `-p|--default-provider <PROVIDER>`

  <span data-ttu-id="48614-127">未針對指定的程式庫定義提供者時，所要使用的提供者。</span><span class="sxs-lookup"><span data-stu-id="48614-127">The provider to use if no provider is defined for a given library.</span></span> <span data-ttu-id="48614-128">此 `<PROVIDER>` 值會寫入至 `defaultProvider` *上libman.js*的屬性。</span><span class="sxs-lookup"><span data-stu-id="48614-128">The `<PROVIDER>` value is written to the `defaultProvider` property of *libman.json*.</span></span> <span data-ttu-id="48614-129">取代 `<PROVIDER>` 為下列其中一個值：</span><span class="sxs-lookup"><span data-stu-id="48614-129">Replace `<PROVIDER>` with one of the following values:</span></span>

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="48614-130">範例</span><span class="sxs-lookup"><span data-stu-id="48614-130">Examples</span></span>

<span data-ttu-id="48614-131">若要在 ASP.NET Core 專案中建立檔案的*libman.js* ：</span><span class="sxs-lookup"><span data-stu-id="48614-131">To create a *libman.json* file in an ASP.NET Core project:</span></span>

* <span data-ttu-id="48614-132">流覽至專案根目錄。</span><span class="sxs-lookup"><span data-stu-id="48614-132">Navigate to the project root.</span></span>
* <span data-ttu-id="48614-133">執行以下命令：</span><span class="sxs-lookup"><span data-stu-id="48614-133">Run the following command:</span></span>

  ```console
  libman init
  ```

* <span data-ttu-id="48614-134">輸入預設提供者的名稱，或按 `Enter` 以使用預設的 CDNJS 提供者。</span><span class="sxs-lookup"><span data-stu-id="48614-134">Type the name of the default provider, or press `Enter` to use the default CDNJS provider.</span></span> <span data-ttu-id="48614-135">有效值包括：</span><span class="sxs-lookup"><span data-stu-id="48614-135">Valid values include:</span></span>

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  ![libman init 命令-預設提供者](_static/libman-init-provider.png)

<span data-ttu-id="48614-137">檔案*上的libman.js*會使用下列內容新增至專案根目錄：</span><span class="sxs-lookup"><span data-stu-id="48614-137">A *libman.json* file is added to the project root with the following content:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

## <a name="add-library-files"></a><span data-ttu-id="48614-138">新增程式庫檔案</span><span class="sxs-lookup"><span data-stu-id="48614-138">Add library files</span></span>

<span data-ttu-id="48614-139">命令會將連結 `libman install` 庫檔案下載並安裝到專案中。</span><span class="sxs-lookup"><span data-stu-id="48614-139">The `libman install` command downloads and installs library files into the project.</span></span> <span data-ttu-id="48614-140">如果檔案不存在，則會新增檔案*上的libman.js* 。</span><span class="sxs-lookup"><span data-stu-id="48614-140">A *libman.json* file is added if one doesn't exist.</span></span> <span data-ttu-id="48614-141">檔案*上的libman.js*會進行修改，以儲存程式庫檔案的設定詳細資料。</span><span class="sxs-lookup"><span data-stu-id="48614-141">The *libman.json* file is modified to store configuration details for the library files.</span></span>

### <a name="synopsis"></a><span data-ttu-id="48614-142">概要</span><span class="sxs-lookup"><span data-stu-id="48614-142">Synopsis</span></span>

```console
libman install <LIBRARY> [-d|--destination] [--files] [-p|--provider] [--verbosity]
libman install [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="48614-143">引數</span><span class="sxs-lookup"><span data-stu-id="48614-143">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="48614-144">要安裝之程式庫的名稱。</span><span class="sxs-lookup"><span data-stu-id="48614-144">The name of the library to install.</span></span> <span data-ttu-id="48614-145">此名稱可能包含版本號碼標記法（例如 `@1.2.0` ）。</span><span class="sxs-lookup"><span data-stu-id="48614-145">This name may include version number notation (for example, `@1.2.0`).</span></span>

### <a name="options"></a><span data-ttu-id="48614-146">選項</span><span class="sxs-lookup"><span data-stu-id="48614-146">Options</span></span>

<span data-ttu-id="48614-147">以下是使用 `libman install` 命令時可用的選項：</span><span class="sxs-lookup"><span data-stu-id="48614-147">The following options are available for the `libman install` command:</span></span>

* `-d|--destination <PATH>`

  <span data-ttu-id="48614-148">要安裝程式庫的位置。</span><span class="sxs-lookup"><span data-stu-id="48614-148">The location to install the library.</span></span> <span data-ttu-id="48614-149">如果未指定，則會使用預設位置。</span><span class="sxs-lookup"><span data-stu-id="48614-149">If not specified, the default location is used.</span></span> <span data-ttu-id="48614-150">如果 `defaultDestination` *libman.js*中未指定任何屬性，則需要此選項。</span><span class="sxs-lookup"><span data-stu-id="48614-150">If no `defaultDestination` property is specified in *libman.json*, this option is required.</span></span>

* `--files <FILE>`

  <span data-ttu-id="48614-151">指定要從程式庫安裝的檔案名。</span><span class="sxs-lookup"><span data-stu-id="48614-151">Specify the name of the file to install from the library.</span></span> <span data-ttu-id="48614-152">如果未指定，則會安裝媒體櫃中的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="48614-152">If not specified, all files from the library are installed.</span></span> <span data-ttu-id="48614-153">`--files`針對要安裝的每個檔案提供一個選項。</span><span class="sxs-lookup"><span data-stu-id="48614-153">Provide one `--files` option per file to be installed.</span></span> <span data-ttu-id="48614-154">也支援相對路徑。</span><span class="sxs-lookup"><span data-stu-id="48614-154">Relative paths are supported too.</span></span> <span data-ttu-id="48614-155">例如： `--files dist/browser/signalr.js` 。</span><span class="sxs-lookup"><span data-stu-id="48614-155">For example: `--files dist/browser/signalr.js`.</span></span>

* `-p|--provider <PROVIDER>`

  <span data-ttu-id="48614-156">要用於取得程式庫的提供者名稱。</span><span class="sxs-lookup"><span data-stu-id="48614-156">The name of the provider to use for the library acquisition.</span></span> <span data-ttu-id="48614-157">取代 `<PROVIDER>` 為下列其中一個值：</span><span class="sxs-lookup"><span data-stu-id="48614-157">Replace `<PROVIDER>` with one of the following values:</span></span>
  
  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  <span data-ttu-id="48614-158">如果未指定， `defaultProvider` 則會使用*libman.json*中的屬性。</span><span class="sxs-lookup"><span data-stu-id="48614-158">If not specified, the `defaultProvider` property in *libman.json* is used.</span></span> <span data-ttu-id="48614-159">如果 `defaultProvider` *libman.js*中未指定任何屬性，則需要此選項。</span><span class="sxs-lookup"><span data-stu-id="48614-159">If no `defaultProvider` property is specified in *libman.json*, this option is required.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="48614-160">範例</span><span class="sxs-lookup"><span data-stu-id="48614-160">Examples</span></span>

<span data-ttu-id="48614-161">請考慮下列*libman.js*檔案：</span><span class="sxs-lookup"><span data-stu-id="48614-161">Consider the following *libman.json* file:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

<span data-ttu-id="48614-162">若要使用 CDNJS 提供者，將 jQuery 版本 3.2.1 *jquery.min.js*檔案安裝至*wwwroot/scripts/jQuery*資料夾：</span><span class="sxs-lookup"><span data-stu-id="48614-162">To install the jQuery version 3.2.1 *jquery.min.js* file to the *wwwroot/scripts/jquery* folder using the CDNJS provider:</span></span>

```console
libman install jquery@3.2.1 --provider cdnjs --destination wwwroot/scripts/jquery --files jquery.min.js
```

<span data-ttu-id="48614-163">檔案*上的libman.js*如下所示：</span><span class="sxs-lookup"><span data-stu-id="48614-163">The *libman.json* file resembles the following:</span></span>

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

<span data-ttu-id="48614-164">若要使用檔案系統提供者來安裝*C： \\ temp \\ contosoCalendar \\ *中的*calendar.js*和*calendar .css*檔案：</span><span class="sxs-lookup"><span data-stu-id="48614-164">To install the *calendar.js* and *calendar.css* files from *C:\\temp\\contosoCalendar\\* using the file system provider:</span></span>

  ```console
  libman install C:\temp\contosoCalendar\ --provider filesystem --files calendar.js --files calendar.css
  ```

<span data-ttu-id="48614-165">有兩個原因會出現下列提示：</span><span class="sxs-lookup"><span data-stu-id="48614-165">The following prompt appears for two reasons:</span></span>

* <span data-ttu-id="48614-166">檔案*上的libman.js*不包含 `defaultDestination` 屬性。</span><span class="sxs-lookup"><span data-stu-id="48614-166">The *libman.json* file doesn't contain a `defaultDestination` property.</span></span>
* <span data-ttu-id="48614-167">此 `libman install` 命令不包含 `-d|--destination` 選項。</span><span class="sxs-lookup"><span data-stu-id="48614-167">The `libman install` command doesn't contain the `-d|--destination` option.</span></span>

![libman install 命令-目的地](_static/libman-install-destination.png)

<span data-ttu-id="48614-169">接受預設目的地之後，檔案*上的libman.js*如下所示：</span><span class="sxs-lookup"><span data-stu-id="48614-169">After accepting the default destination, the *libman.json* file resembles the following:</span></span>

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

## <a name="restore-library-files"></a><span data-ttu-id="48614-170">還原程式庫檔案</span><span class="sxs-lookup"><span data-stu-id="48614-170">Restore library files</span></span>

<span data-ttu-id="48614-171">`libman restore`命令會安裝*libman.js*中定義的程式庫檔案。</span><span class="sxs-lookup"><span data-stu-id="48614-171">The `libman restore` command installs library files defined in *libman.json*.</span></span> <span data-ttu-id="48614-172">適用的規則如下：</span><span class="sxs-lookup"><span data-stu-id="48614-172">The following rules apply:</span></span>

* <span data-ttu-id="48614-173">如果專案根目錄中沒有*libman.js*檔案存在，則會傳回錯誤。</span><span class="sxs-lookup"><span data-stu-id="48614-173">If no *libman.json* file exists in the project root, an error is returned.</span></span>
* <span data-ttu-id="48614-174">如果程式庫指定了提供者， `defaultProvider` 則會忽略*libman.json*中的屬性。</span><span class="sxs-lookup"><span data-stu-id="48614-174">If a library specifies a provider, the `defaultProvider` property in *libman.json* is ignored.</span></span>
* <span data-ttu-id="48614-175">如果程式庫指定了目的地， `defaultDestination` 則會忽略*libman.json*中的屬性。</span><span class="sxs-lookup"><span data-stu-id="48614-175">If a library specifies a destination, the `defaultDestination` property in *libman.json* is ignored.</span></span>

### <a name="synopsis"></a><span data-ttu-id="48614-176">概要</span><span class="sxs-lookup"><span data-stu-id="48614-176">Synopsis</span></span>

```console
libman restore [--verbosity]
libman restore [-h|--help]
```

### <a name="options"></a><span data-ttu-id="48614-177">選項</span><span class="sxs-lookup"><span data-stu-id="48614-177">Options</span></span>

<span data-ttu-id="48614-178">以下是使用 `libman restore` 命令時可用的選項：</span><span class="sxs-lookup"><span data-stu-id="48614-178">The following options are available for the `libman restore` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="48614-179">範例</span><span class="sxs-lookup"><span data-stu-id="48614-179">Examples</span></span>

<span data-ttu-id="48614-180">若要還原在*libman.js*中定義的程式庫檔案：</span><span class="sxs-lookup"><span data-stu-id="48614-180">To restore the library files defined in *libman.json*:</span></span>

```console
libman restore
```

## <a name="delete-library-files"></a><span data-ttu-id="48614-181">刪除程式庫檔案</span><span class="sxs-lookup"><span data-stu-id="48614-181">Delete library files</span></span>

<span data-ttu-id="48614-182">`libman clean`命令會刪除先前透過 LibMan 還原的程式庫檔案。</span><span class="sxs-lookup"><span data-stu-id="48614-182">The `libman clean` command deletes library files previously restored via LibMan.</span></span> <span data-ttu-id="48614-183">刪除此作業後變成空白的資料夾。</span><span class="sxs-lookup"><span data-stu-id="48614-183">Folders that become empty after this operation are deleted.</span></span> <span data-ttu-id="48614-184">`libraries`不會移除*libman.json*之屬性中的程式庫檔案相關聯的設定。</span><span class="sxs-lookup"><span data-stu-id="48614-184">The library files' associated configurations in the `libraries` property of *libman.json* aren't removed.</span></span>

### <a name="synopsis"></a><span data-ttu-id="48614-185">概要</span><span class="sxs-lookup"><span data-stu-id="48614-185">Synopsis</span></span>

```console
libman clean [--verbosity]
libman clean [-h|--help]
```

### <a name="options"></a><span data-ttu-id="48614-186">選項</span><span class="sxs-lookup"><span data-stu-id="48614-186">Options</span></span>

<span data-ttu-id="48614-187">以下是使用 `libman clean` 命令時可用的選項：</span><span class="sxs-lookup"><span data-stu-id="48614-187">The following options are available for the `libman clean` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="48614-188">範例</span><span class="sxs-lookup"><span data-stu-id="48614-188">Examples</span></span>

<span data-ttu-id="48614-189">若要刪除透過 LibMan 安裝的程式庫檔案：</span><span class="sxs-lookup"><span data-stu-id="48614-189">To delete library files installed via LibMan:</span></span>

```console
libman clean
```

## <a name="uninstall-library-files"></a><span data-ttu-id="48614-190">卸載程式庫檔案</span><span class="sxs-lookup"><span data-stu-id="48614-190">Uninstall library files</span></span>

<span data-ttu-id="48614-191">`libman uninstall`命令：</span><span class="sxs-lookup"><span data-stu-id="48614-191">The `libman uninstall` command:</span></span>

* <span data-ttu-id="48614-192">從*libman.js*中的目的地，刪除與指定文件庫相關聯的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="48614-192">Deletes all files associated with the specified library from the destination in *libman.json*.</span></span>
* <span data-ttu-id="48614-193">從*libman.js*移除相關聯的程式庫設定。</span><span class="sxs-lookup"><span data-stu-id="48614-193">Removes the associated library configuration from *libman.json*.</span></span>

<span data-ttu-id="48614-194">當下列情況發生錯誤：</span><span class="sxs-lookup"><span data-stu-id="48614-194">An error occurs when:</span></span>

* <span data-ttu-id="48614-195">專案根目錄中不存在任何*libman.js*檔案。</span><span class="sxs-lookup"><span data-stu-id="48614-195">No *libman.json* file exists in the project root.</span></span>
* <span data-ttu-id="48614-196">指定的程式庫不存在。</span><span class="sxs-lookup"><span data-stu-id="48614-196">The specified library doesn't exist.</span></span>

<span data-ttu-id="48614-197">如果安裝了多個具有相同名稱的程式庫，系統會提示您選擇其中一個。</span><span class="sxs-lookup"><span data-stu-id="48614-197">If more than one library with the same name is installed, you're prompted to choose one.</span></span>

### <a name="synopsis"></a><span data-ttu-id="48614-198">概要</span><span class="sxs-lookup"><span data-stu-id="48614-198">Synopsis</span></span>

```console
libman uninstall <LIBRARY> [--verbosity]
libman uninstall [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="48614-199">引數</span><span class="sxs-lookup"><span data-stu-id="48614-199">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="48614-200">要卸載之程式庫的名稱。</span><span class="sxs-lookup"><span data-stu-id="48614-200">The name of the library to uninstall.</span></span> <span data-ttu-id="48614-201">此名稱可能包含版本號碼標記法（例如 `@1.2.0` ）。</span><span class="sxs-lookup"><span data-stu-id="48614-201">This name may include version number notation (for example, `@1.2.0`).</span></span>

### <a name="options"></a><span data-ttu-id="48614-202">選項</span><span class="sxs-lookup"><span data-stu-id="48614-202">Options</span></span>

<span data-ttu-id="48614-203">以下是使用 `libman uninstall` 命令時可用的選項：</span><span class="sxs-lookup"><span data-stu-id="48614-203">The following options are available for the `libman uninstall` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="48614-204">範例</span><span class="sxs-lookup"><span data-stu-id="48614-204">Examples</span></span>

<span data-ttu-id="48614-205">請考慮下列*libman.js*檔案：</span><span class="sxs-lookup"><span data-stu-id="48614-205">Consider the following *libman.json* file:</span></span>

[!code-json[](samples/LibManSample/libman.json)]

* <span data-ttu-id="48614-206">若要卸載 jQuery，下列任一命令都會成功：</span><span class="sxs-lookup"><span data-stu-id="48614-206">To uninstall jQuery, either of the following commands succeed:</span></span>

  ```console
  libman uninstall jquery
  ```

  ```console
  libman uninstall jquery@3.3.1
  ```

* <span data-ttu-id="48614-207">若要卸載透過提供者安裝的 Lodash 所檔案 `filesystem` ：</span><span class="sxs-lookup"><span data-stu-id="48614-207">To uninstall the Lodash files installed via the `filesystem` provider:</span></span>

  ```console
  libman uninstall C:\temp\lodash\
  ```

## <a name="update-library-version"></a><span data-ttu-id="48614-208">更新程式庫版本</span><span class="sxs-lookup"><span data-stu-id="48614-208">Update library version</span></span>

<span data-ttu-id="48614-209">命令會將透過 `libman update` LibMan 安裝的程式庫更新為指定的版本。</span><span class="sxs-lookup"><span data-stu-id="48614-209">The `libman update` command updates a library installed via LibMan to the specified version.</span></span>

<span data-ttu-id="48614-210">當下列情況發生錯誤：</span><span class="sxs-lookup"><span data-stu-id="48614-210">An error occurs when:</span></span>

* <span data-ttu-id="48614-211">專案根目錄中不存在任何*libman.js*檔案。</span><span class="sxs-lookup"><span data-stu-id="48614-211">No *libman.json* file exists in the project root.</span></span>
* <span data-ttu-id="48614-212">指定的程式庫不存在。</span><span class="sxs-lookup"><span data-stu-id="48614-212">The specified library doesn't exist.</span></span>

<span data-ttu-id="48614-213">如果安裝了多個具有相同名稱的程式庫，系統會提示您選擇其中一個。</span><span class="sxs-lookup"><span data-stu-id="48614-213">If more than one library with the same name is installed, you're prompted to choose one.</span></span>

### <a name="synopsis"></a><span data-ttu-id="48614-214">概要</span><span class="sxs-lookup"><span data-stu-id="48614-214">Synopsis</span></span>

```console
libman update <LIBRARY> [-pre] [--to] [--verbosity]
libman update [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="48614-215">引數</span><span class="sxs-lookup"><span data-stu-id="48614-215">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="48614-216">要更新之程式庫的名稱。</span><span class="sxs-lookup"><span data-stu-id="48614-216">The name of the library to update.</span></span>

### <a name="options"></a><span data-ttu-id="48614-217">選項</span><span class="sxs-lookup"><span data-stu-id="48614-217">Options</span></span>

<span data-ttu-id="48614-218">以下是使用 `libman update` 命令時可用的選項：</span><span class="sxs-lookup"><span data-stu-id="48614-218">The following options are available for the `libman update` command:</span></span>

* `-pre`

  <span data-ttu-id="48614-219">取得程式庫的最新發行前版本。</span><span class="sxs-lookup"><span data-stu-id="48614-219">Obtain the latest prerelease version of the library.</span></span>

* `--to <VERSION>`

  <span data-ttu-id="48614-220">取得特定版本的程式庫。</span><span class="sxs-lookup"><span data-stu-id="48614-220">Obtain a specific version of the library.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="48614-221">範例</span><span class="sxs-lookup"><span data-stu-id="48614-221">Examples</span></span>

* <span data-ttu-id="48614-222">若要將 jQuery 更新為最新版本：</span><span class="sxs-lookup"><span data-stu-id="48614-222">To update jQuery to the latest version:</span></span>

  ```console
  libman update jquery
  ```

* <span data-ttu-id="48614-223">若要將 jQuery 更新為版本3.3.1：</span><span class="sxs-lookup"><span data-stu-id="48614-223">To update jQuery to version 3.3.1:</span></span>

  ```console
  libman update jquery --to 3.3.1
  ```

* <span data-ttu-id="48614-224">若要將 jQuery 更新為最新的發行前版本：</span><span class="sxs-lookup"><span data-stu-id="48614-224">To update jQuery to the latest prerelease version:</span></span>

  ```console
  libman update jquery -pre
  ```

## <a name="manage-library-cache"></a><span data-ttu-id="48614-225">管理程式庫快取</span><span class="sxs-lookup"><span data-stu-id="48614-225">Manage library cache</span></span>

<span data-ttu-id="48614-226">`libman cache`命令會管理 LibMan 程式庫快取。</span><span class="sxs-lookup"><span data-stu-id="48614-226">The `libman cache` command manages the LibMan library cache.</span></span> <span data-ttu-id="48614-227">`filesystem`提供者不會使用程式庫快取。</span><span class="sxs-lookup"><span data-stu-id="48614-227">The `filesystem` provider doesn't use the library cache.</span></span>

### <a name="synopsis"></a><span data-ttu-id="48614-228">概要</span><span class="sxs-lookup"><span data-stu-id="48614-228">Synopsis</span></span>

```console
libman cache clean [<PROVIDER>] [--verbosity]
libman cache list [--files] [--libraries] [--verbosity]
libman cache [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="48614-229">引數</span><span class="sxs-lookup"><span data-stu-id="48614-229">Arguments</span></span>

`PROVIDER`

<span data-ttu-id="48614-230">僅搭配命令使用 `clean` 。</span><span class="sxs-lookup"><span data-stu-id="48614-230">Only used with the `clean` command.</span></span> <span data-ttu-id="48614-231">指定要清除的提供者快取。</span><span class="sxs-lookup"><span data-stu-id="48614-231">Specifies the provider cache to clean.</span></span> <span data-ttu-id="48614-232">有效值包括：</span><span class="sxs-lookup"><span data-stu-id="48614-232">Valid values include:</span></span>

[!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

### <a name="options"></a><span data-ttu-id="48614-233">選項</span><span class="sxs-lookup"><span data-stu-id="48614-233">Options</span></span>

<span data-ttu-id="48614-234">以下是使用 `libman cache` 命令時可用的選項：</span><span class="sxs-lookup"><span data-stu-id="48614-234">The following options are available for the `libman cache` command:</span></span>

* `--files`

  <span data-ttu-id="48614-235">列出快取的檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="48614-235">List the names of files that are cached.</span></span>

* `--libraries`

  <span data-ttu-id="48614-236">列出快取的程式庫名稱。</span><span class="sxs-lookup"><span data-stu-id="48614-236">List the names of libraries that are cached.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="48614-237">範例</span><span class="sxs-lookup"><span data-stu-id="48614-237">Examples</span></span>

* <span data-ttu-id="48614-238">若要查看每個提供者的快取程式庫名稱，請使用下列其中一個命令：</span><span class="sxs-lookup"><span data-stu-id="48614-238">To view the names of cached libraries per provider, use one of the following commands:</span></span>

  ```console
  libman cache list
  ```

  ```console
  libman cache list --libraries
  ```

  <span data-ttu-id="48614-239">會顯示類似下列的輸出：</span><span class="sxs-lookup"><span data-stu-id="48614-239">Output similar to the following is displayed:</span></span>

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

* <span data-ttu-id="48614-240">若要查看每個提供者的快取程式庫檔案的名稱：</span><span class="sxs-lookup"><span data-stu-id="48614-240">To view the names of cached library files per provider:</span></span>

  ```console
  libman cache list --files
  ```

  <span data-ttu-id="48614-241">會顯示類似下列的輸出：</span><span class="sxs-lookup"><span data-stu-id="48614-241">Output similar to the following is displayed:</span></span>

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

  <span data-ttu-id="48614-242">請注意，上述輸出顯示 jQuery 版本3.2.1 和3.3.1 會在 CDNJS 提供者底下快取。</span><span class="sxs-lookup"><span data-stu-id="48614-242">Notice the preceding output shows that jQuery versions 3.2.1 and 3.3.1 are cached under the CDNJS provider.</span></span>

* <span data-ttu-id="48614-243">若要清空 CDNJS 提供者的程式庫快取：</span><span class="sxs-lookup"><span data-stu-id="48614-243">To empty the library cache for the CDNJS provider:</span></span>

  ```console
  libman cache clean cdnjs
  ```

  <span data-ttu-id="48614-244">清空 CDNJS 提供者快取之後，此 `libman cache list` 命令會顯示下列內容：</span><span class="sxs-lookup"><span data-stu-id="48614-244">After emptying the CDNJS provider cache, the `libman cache list` command displays the following:</span></span>

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

* <span data-ttu-id="48614-245">若要清空所有支援之提供者的快取：</span><span class="sxs-lookup"><span data-stu-id="48614-245">To empty the cache for all supported providers:</span></span>

  ```console
  libman cache clean
  ```

  <span data-ttu-id="48614-246">清空所有提供者快取之後，此 `libman cache list` 命令會顯示下列內容：</span><span class="sxs-lookup"><span data-stu-id="48614-246">After emptying all provider caches, the `libman cache list` command displays the following:</span></span>

  ```console
  Cache contents:
  ---------------
  unpkg:
      (empty)
  cdnjs:
      (empty)
  ```

## <a name="additional-resources"></a><span data-ttu-id="48614-247">其他資源</span><span class="sxs-lookup"><span data-stu-id="48614-247">Additional resources</span></span>

* [<span data-ttu-id="48614-248">安裝通用工具</span><span class="sxs-lookup"><span data-stu-id="48614-248">Install a Global Tool</span></span>](/dotnet/core/tools/global-tools#install-a-global-tool)
* <xref:client-side/libman/libman-vs>
* [<span data-ttu-id="48614-249">LibMan GitHub 存放庫</span><span class="sxs-lookup"><span data-stu-id="48614-249">LibMan GitHub repository</span></span>](https://github.com/aspnet/LibraryManager)
