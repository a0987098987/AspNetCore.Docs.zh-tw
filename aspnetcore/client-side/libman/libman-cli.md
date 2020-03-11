---
title: 搭配 ASP.NET Core 使用 LibMan CLI
author: scottaddie
description: 瞭解如何在 ASP.NET Core 專案中使用 LibMan CLI。
ms.author: scaddie
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: client-side/libman/libman-cli
ms.openlocfilehash: 02d88d09805bd23a86ef924766373245fec7ff52
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78664630"
---
# <a name="use-the-libman-cli-with-aspnet-core"></a><span data-ttu-id="3c1c2-103">搭配 ASP.NET Core 使用 LibMan CLI</span><span class="sxs-lookup"><span data-stu-id="3c1c2-103">Use the LibMan CLI with ASP.NET Core</span></span>

<span data-ttu-id="3c1c2-104">作者：[Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="3c1c2-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="3c1c2-105">[LibMan](xref:client-side/libman/index) CLI 是一種跨平臺工具，支援 .net Core 的任何位置。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-105">The [LibMan](xref:client-side/libman/index) CLI is a cross-platform tool that's supported everywhere .NET Core is supported.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3c1c2-106">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="3c1c2-106">Prerequisites</span></span>

* [!INCLUDE [2.1-SDK](../../includes/2.1-SDK.md)]

## <a name="installation"></a><span data-ttu-id="3c1c2-107">安裝</span><span class="sxs-lookup"><span data-stu-id="3c1c2-107">Installation</span></span>

<span data-ttu-id="3c1c2-108">若要安裝 LibMan CLI：</span><span class="sxs-lookup"><span data-stu-id="3c1c2-108">To install the LibMan CLI:</span></span>

```dotnetcli
dotnet tool install -g Microsoft.Web.LibraryManager.Cli
```

<span data-ttu-id="3c1c2-109">[.Net Core 通用工具](/dotnet/core/tools/global-tools#install-a-global-tool)是從[LibraryManager](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Cli/)安裝而來。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-109">A [.NET Core Global Tool](/dotnet/core/tools/global-tools#install-a-global-tool) is installed from the [Microsoft.Web.LibraryManager.Cli](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Cli/) NuGet package.</span></span>

<span data-ttu-id="3c1c2-110">若要從特定的 NuGet 套件來源安裝 LibMan CLI：</span><span class="sxs-lookup"><span data-stu-id="3c1c2-110">To install the LibMan CLI from a specific NuGet package source:</span></span>

```dotnetcli
dotnet tool install -g Microsoft.Web.LibraryManager.Cli --version 1.0.94-g606058a278 --add-source C:\Temp\
```

<span data-ttu-id="3c1c2-111">在上述範例中，.NET Core 通用工具是從本機 Windows 電腦的*C:\Temp\Microsoft.Web.LibraryManager.Cli.1.0.94-g606058a278.nupkg*檔案進行安裝。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-111">In the preceding example, a .NET Core Global Tool is installed from the local Windows machine's *C:\Temp\Microsoft.Web.LibraryManager.Cli.1.0.94-g606058a278.nupkg* file.</span></span>

## <a name="usage"></a><span data-ttu-id="3c1c2-112">使用量</span><span class="sxs-lookup"><span data-stu-id="3c1c2-112">Usage</span></span>

<span data-ttu-id="3c1c2-113">成功安裝 CLI 之後，您可以使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="3c1c2-113">After successful installation of the CLI, the following command can be used:</span></span>

```console
libman
```

<span data-ttu-id="3c1c2-114">若要查看已安裝的 CLI 版本：</span><span class="sxs-lookup"><span data-stu-id="3c1c2-114">To view the installed CLI version:</span></span>

```console
libman --version
```

<span data-ttu-id="3c1c2-115">若要查看可用的 CLI 命令：</span><span class="sxs-lookup"><span data-stu-id="3c1c2-115">To view the available CLI commands:</span></span>

```console
libman --help
```

<span data-ttu-id="3c1c2-116">上述命令會顯示類似下列的輸出：</span><span class="sxs-lookup"><span data-stu-id="3c1c2-116">The preceding command displays output similar to the following:</span></span>

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

<span data-ttu-id="3c1c2-117">下列各節將概述可用的 CLI 命令。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-117">The following sections outline the available CLI commands.</span></span>

## <a name="initialize-libman-in-the-project"></a><span data-ttu-id="3c1c2-118">初始化專案中的 LibMan</span><span class="sxs-lookup"><span data-stu-id="3c1c2-118">Initialize LibMan in the project</span></span>

<span data-ttu-id="3c1c2-119">`libman init` 命令會建立*libman*檔案（如果不存在的話）。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-119">The `libman init` command creates a *libman.json* file if one doesn't exist.</span></span> <span data-ttu-id="3c1c2-120">使用預設專案範本內容建立檔案。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-120">The file is created with the default item template content.</span></span>

### <a name="synopsis"></a><span data-ttu-id="3c1c2-121">概要</span><span class="sxs-lookup"><span data-stu-id="3c1c2-121">Synopsis</span></span>

```console
libman init [-d|--default-destination] [-p|--default-provider] [--verbosity]
libman init [-h|--help]
```

### <a name="options"></a><span data-ttu-id="3c1c2-122">選項。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-122">Options</span></span>

<span data-ttu-id="3c1c2-123">以下是使用 `libman init` 命令時可用的選項：</span><span class="sxs-lookup"><span data-stu-id="3c1c2-123">The following options are available for the `libman init` command:</span></span>

* `-d|--default-destination <PATH>`

  <span data-ttu-id="3c1c2-124">相對於目前資料夾的路徑。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-124">A path relative to the current folder.</span></span> <span data-ttu-id="3c1c2-125">如果未針對*libman*中的程式庫定義任何 `destination` 內容，則會在此位置安裝程式庫檔案。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-125">Library files are installed in this location if no `destination` property is defined for a library in *libman.json*.</span></span> <span data-ttu-id="3c1c2-126">`<PATH>` 值會寫入至*libman*的 `defaultDestination` 屬性。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-126">The `<PATH>` value is written to the `defaultDestination` property of *libman.json*.</span></span>

* `-p|--default-provider <PROVIDER>`

  <span data-ttu-id="3c1c2-127">未針對指定的程式庫定義提供者時，所要使用的提供者。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-127">The provider to use if no provider is defined for a given library.</span></span> <span data-ttu-id="3c1c2-128">`<PROVIDER>` 值會寫入至*libman*的 `defaultProvider` 屬性。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-128">The `<PROVIDER>` value is written to the `defaultProvider` property of *libman.json*.</span></span> <span data-ttu-id="3c1c2-129">將 `<PROVIDER>` 取代為下列其中一個值：</span><span class="sxs-lookup"><span data-stu-id="3c1c2-129">Replace `<PROVIDER>` with one of the following values:</span></span>

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="3c1c2-130">範例</span><span class="sxs-lookup"><span data-stu-id="3c1c2-130">Examples</span></span>

<span data-ttu-id="3c1c2-131">若要在 ASP.NET Core 專案中建立*libman json*檔案：</span><span class="sxs-lookup"><span data-stu-id="3c1c2-131">To create a *libman.json* file in an ASP.NET Core project:</span></span>

* <span data-ttu-id="3c1c2-132">流覽至專案根目錄。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-132">Navigate to the project root.</span></span>
* <span data-ttu-id="3c1c2-133">執行以下命令：</span><span class="sxs-lookup"><span data-stu-id="3c1c2-133">Run the following command:</span></span>

  ```console
  libman init
  ```

* <span data-ttu-id="3c1c2-134">輸入預設提供者的名稱，或按 `Enter` 以使用預設的 CDNJS 提供者。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-134">Type the name of the default provider, or press `Enter` to use the default CDNJS provider.</span></span> <span data-ttu-id="3c1c2-135">有效值包括：</span><span class="sxs-lookup"><span data-stu-id="3c1c2-135">Valid values include:</span></span>

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  ![libman init 命令-預設提供者](_static/libman-init-provider.png)

<span data-ttu-id="3c1c2-137">*Libman*會使用下列內容新增至專案根目錄：</span><span class="sxs-lookup"><span data-stu-id="3c1c2-137">A *libman.json* file is added to the project root with the following content:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

## <a name="add-library-files"></a><span data-ttu-id="3c1c2-138">新增程式庫檔案</span><span class="sxs-lookup"><span data-stu-id="3c1c2-138">Add library files</span></span>

<span data-ttu-id="3c1c2-139">`libman install` 命令會將程式庫檔案下載並安裝到專案中。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-139">The `libman install` command downloads and installs library files into the project.</span></span> <span data-ttu-id="3c1c2-140">如果*libman*檔案不存在，則會新增它。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-140">A *libman.json* file is added if one doesn't exist.</span></span> <span data-ttu-id="3c1c2-141">已修改*libman*檔案以儲存程式庫檔案的設定詳細資料。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-141">The *libman.json* file is modified to store configuration details for the library files.</span></span>

### <a name="synopsis"></a><span data-ttu-id="3c1c2-142">概要</span><span class="sxs-lookup"><span data-stu-id="3c1c2-142">Synopsis</span></span>

```console
libman install <LIBRARY> [-d|--destination] [--files] [-p|--provider] [--verbosity]
libman install [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="3c1c2-143">引數</span><span class="sxs-lookup"><span data-stu-id="3c1c2-143">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="3c1c2-144">要安裝之程式庫的名稱。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-144">The name of the library to install.</span></span> <span data-ttu-id="3c1c2-145">此名稱可能包含版本號碼標記法（例如 `@1.2.0`）。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-145">This name may include version number notation (for example, `@1.2.0`).</span></span>

### <a name="options"></a><span data-ttu-id="3c1c2-146">選項。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-146">Options</span></span>

<span data-ttu-id="3c1c2-147">以下是使用 `libman install` 命令時可用的選項：</span><span class="sxs-lookup"><span data-stu-id="3c1c2-147">The following options are available for the `libman install` command:</span></span>

* `-d|--destination <PATH>`

  <span data-ttu-id="3c1c2-148">要安裝程式庫的位置。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-148">The location to install the library.</span></span> <span data-ttu-id="3c1c2-149">如果未指定，則會使用預設位置。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-149">If not specified, the default location is used.</span></span> <span data-ttu-id="3c1c2-150">如果在*libman*中未指定任何 `defaultDestination` 屬性，則此為必要選項。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-150">If no `defaultDestination` property is specified in *libman.json*, this option is required.</span></span>

* `--files <FILE>`

  <span data-ttu-id="3c1c2-151">指定要從程式庫安裝的檔案名。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-151">Specify the name of the file to install from the library.</span></span> <span data-ttu-id="3c1c2-152">如果未指定，則會安裝媒體櫃中的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-152">If not specified, all files from the library are installed.</span></span> <span data-ttu-id="3c1c2-153">為每個要安裝的檔案提供一個 `--files` 選項。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-153">Provide one `--files` option per file to be installed.</span></span> <span data-ttu-id="3c1c2-154">也支援相對路徑。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-154">Relative paths are supported too.</span></span> <span data-ttu-id="3c1c2-155">例如： `--files dist/browser/signalr.js` 。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-155">For example: `--files dist/browser/signalr.js`.</span></span>

* `-p|--provider <PROVIDER>`

  <span data-ttu-id="3c1c2-156">要用於取得程式庫的提供者名稱。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-156">The name of the provider to use for the library acquisition.</span></span> <span data-ttu-id="3c1c2-157">將 `<PROVIDER>` 取代為下列其中一個值：</span><span class="sxs-lookup"><span data-stu-id="3c1c2-157">Replace `<PROVIDER>` with one of the following values:</span></span>
  
  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  <span data-ttu-id="3c1c2-158">如果未指定，則會使用*libman*中的 `defaultProvider` 屬性。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-158">If not specified, the `defaultProvider` property in *libman.json* is used.</span></span> <span data-ttu-id="3c1c2-159">如果在*libman*中未指定任何 `defaultProvider` 屬性，則此為必要選項。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-159">If no `defaultProvider` property is specified in *libman.json*, this option is required.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="3c1c2-160">範例</span><span class="sxs-lookup"><span data-stu-id="3c1c2-160">Examples</span></span>

<span data-ttu-id="3c1c2-161">請考慮下列*libman json*檔案：</span><span class="sxs-lookup"><span data-stu-id="3c1c2-161">Consider the following *libman.json* file:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

<span data-ttu-id="3c1c2-162">若要使用 CDNJS 提供者，將 jQuery 版本 3.2.1 *jquery. min .js*檔案安裝至*wwwroot/scripts/jQuery*資料夾：</span><span class="sxs-lookup"><span data-stu-id="3c1c2-162">To install the jQuery version 3.2.1 *jquery.min.js* file to the *wwwroot/scripts/jquery* folder using the CDNJS provider:</span></span>

```console
libman install jquery@3.2.1 --provider cdnjs --destination wwwroot/scripts/jquery --files jquery.min.js
```

<span data-ttu-id="3c1c2-163">*Libman*類似下列內容：</span><span class="sxs-lookup"><span data-stu-id="3c1c2-163">The *libman.json* file resembles the following:</span></span>

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

<span data-ttu-id="3c1c2-164">若要使用檔案系統提供者，從*C：\\temp\\contosoCalendar\\* 安裝行事*曆*和*calendar*檔案：</span><span class="sxs-lookup"><span data-stu-id="3c1c2-164">To install the *calendar.js* and *calendar.css* files from *C:\\temp\\contosoCalendar\\* using the file system provider:</span></span>

  ```console
  libman install C:\temp\contosoCalendar\ --provider filesystem --files calendar.js --files calendar.css
  ```

<span data-ttu-id="3c1c2-165">有兩個原因會出現下列提示：</span><span class="sxs-lookup"><span data-stu-id="3c1c2-165">The following prompt appears for two reasons:</span></span>

* <span data-ttu-id="3c1c2-166">*Libman*不包含 `defaultDestination` 屬性。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-166">The *libman.json* file doesn't contain a `defaultDestination` property.</span></span>
* <span data-ttu-id="3c1c2-167">`libman install` 命令不包含 `-d|--destination` 選項。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-167">The `libman install` command doesn't contain the `-d|--destination` option.</span></span>

![libman install 命令-目的地](_static/libman-install-destination.png)

<span data-ttu-id="3c1c2-169">接受預設目的地之後， *libman*會如下所示：</span><span class="sxs-lookup"><span data-stu-id="3c1c2-169">After accepting the default destination, the *libman.json* file resembles the following:</span></span>

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

## <a name="restore-library-files"></a><span data-ttu-id="3c1c2-170">還原程式庫檔案</span><span class="sxs-lookup"><span data-stu-id="3c1c2-170">Restore library files</span></span>

<span data-ttu-id="3c1c2-171">`libman restore` 命令會安裝*libman*中定義的程式庫檔案。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-171">The `libman restore` command installs library files defined in *libman.json*.</span></span> <span data-ttu-id="3c1c2-172">適用的規則如下：</span><span class="sxs-lookup"><span data-stu-id="3c1c2-172">The following rules apply:</span></span>

* <span data-ttu-id="3c1c2-173">如果專案根目錄中沒有*libman*檔案存在，則會傳回錯誤。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-173">If no *libman.json* file exists in the project root, an error is returned.</span></span>
* <span data-ttu-id="3c1c2-174">如果程式庫指定了提供者，則會忽略*libman*中的 `defaultProvider` 屬性。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-174">If a library specifies a provider, the `defaultProvider` property in *libman.json* is ignored.</span></span>
* <span data-ttu-id="3c1c2-175">如果程式庫指定了目的地，則會忽略*libman*中的 `defaultDestination` 屬性。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-175">If a library specifies a destination, the `defaultDestination` property in *libman.json* is ignored.</span></span>

### <a name="synopsis"></a><span data-ttu-id="3c1c2-176">概要</span><span class="sxs-lookup"><span data-stu-id="3c1c2-176">Synopsis</span></span>

```console
libman restore [--verbosity]
libman restore [-h|--help]
```

### <a name="options"></a><span data-ttu-id="3c1c2-177">選項。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-177">Options</span></span>

<span data-ttu-id="3c1c2-178">以下是使用 `libman restore` 命令時可用的選項：</span><span class="sxs-lookup"><span data-stu-id="3c1c2-178">The following options are available for the `libman restore` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="3c1c2-179">範例</span><span class="sxs-lookup"><span data-stu-id="3c1c2-179">Examples</span></span>

<span data-ttu-id="3c1c2-180">若要還原*libman*中定義的程式庫檔案：</span><span class="sxs-lookup"><span data-stu-id="3c1c2-180">To restore the library files defined in *libman.json*:</span></span>

```console
libman restore
```

## <a name="delete-library-files"></a><span data-ttu-id="3c1c2-181">刪除程式庫檔案</span><span class="sxs-lookup"><span data-stu-id="3c1c2-181">Delete library files</span></span>

<span data-ttu-id="3c1c2-182">`libman clean` 命令會刪除先前透過 LibMan 還原的程式庫檔案。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-182">The `libman clean` command deletes library files previously restored via LibMan.</span></span> <span data-ttu-id="3c1c2-183">刪除此作業後變成空白的資料夾。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-183">Folders that become empty after this operation are deleted.</span></span> <span data-ttu-id="3c1c2-184">程式庫檔案在*libman*的 `libraries` 屬性中的關聯設定不會被移除。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-184">The library files' associated configurations in the `libraries` property of *libman.json* aren't removed.</span></span>

### <a name="synopsis"></a><span data-ttu-id="3c1c2-185">概要</span><span class="sxs-lookup"><span data-stu-id="3c1c2-185">Synopsis</span></span>

```console
libman clean [--verbosity]
libman clean [-h|--help]
```

### <a name="options"></a><span data-ttu-id="3c1c2-186">選項。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-186">Options</span></span>

<span data-ttu-id="3c1c2-187">以下是使用 `libman clean` 命令時可用的選項：</span><span class="sxs-lookup"><span data-stu-id="3c1c2-187">The following options are available for the `libman clean` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="3c1c2-188">範例</span><span class="sxs-lookup"><span data-stu-id="3c1c2-188">Examples</span></span>

<span data-ttu-id="3c1c2-189">若要刪除透過 LibMan 安裝的程式庫檔案：</span><span class="sxs-lookup"><span data-stu-id="3c1c2-189">To delete library files installed via LibMan:</span></span>

```console
libman clean
```

## <a name="uninstall-library-files"></a><span data-ttu-id="3c1c2-190">卸載程式庫檔案</span><span class="sxs-lookup"><span data-stu-id="3c1c2-190">Uninstall library files</span></span>

<span data-ttu-id="3c1c2-191">`libman uninstall` 命令：</span><span class="sxs-lookup"><span data-stu-id="3c1c2-191">The `libman uninstall` command:</span></span>

* <span data-ttu-id="3c1c2-192">從*libman*中的目的地，刪除與指定的程式庫相關聯的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-192">Deletes all files associated with the specified library from the destination in *libman.json*.</span></span>
* <span data-ttu-id="3c1c2-193">從*libman*移除相關聯的程式庫設定。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-193">Removes the associated library configuration from *libman.json*.</span></span>

<span data-ttu-id="3c1c2-194">當下列情況發生錯誤：</span><span class="sxs-lookup"><span data-stu-id="3c1c2-194">An error occurs when:</span></span>

* <span data-ttu-id="3c1c2-195">專案根目錄中沒有*libman*檔案存在。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-195">No *libman.json* file exists in the project root.</span></span>
* <span data-ttu-id="3c1c2-196">指定的程式庫不存在。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-196">The specified library doesn't exist.</span></span>

<span data-ttu-id="3c1c2-197">如果安裝了多個具有相同名稱的程式庫，系統會提示您選擇其中一個。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-197">If more than one library with the same name is installed, you're prompted to choose one.</span></span>

### <a name="synopsis"></a><span data-ttu-id="3c1c2-198">概要</span><span class="sxs-lookup"><span data-stu-id="3c1c2-198">Synopsis</span></span>

```console
libman uninstall <LIBRARY> [--verbosity]
libman uninstall [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="3c1c2-199">引數</span><span class="sxs-lookup"><span data-stu-id="3c1c2-199">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="3c1c2-200">要卸載之程式庫的名稱。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-200">The name of the library to uninstall.</span></span> <span data-ttu-id="3c1c2-201">此名稱可能包含版本號碼標記法（例如 `@1.2.0`）。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-201">This name may include version number notation (for example, `@1.2.0`).</span></span>

### <a name="options"></a><span data-ttu-id="3c1c2-202">選項。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-202">Options</span></span>

<span data-ttu-id="3c1c2-203">以下是使用 `libman uninstall` 命令時可用的選項：</span><span class="sxs-lookup"><span data-stu-id="3c1c2-203">The following options are available for the `libman uninstall` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="3c1c2-204">範例</span><span class="sxs-lookup"><span data-stu-id="3c1c2-204">Examples</span></span>

<span data-ttu-id="3c1c2-205">請考慮下列*libman json*檔案：</span><span class="sxs-lookup"><span data-stu-id="3c1c2-205">Consider the following *libman.json* file:</span></span>

[!code-json[](samples/LibManSample/libman.json)]

* <span data-ttu-id="3c1c2-206">若要卸載 jQuery，下列任一命令都會成功：</span><span class="sxs-lookup"><span data-stu-id="3c1c2-206">To uninstall jQuery, either of the following commands succeed:</span></span>

  ```console
  libman uninstall jquery
  ```

  ```console
  libman uninstall jquery@3.3.1
  ```

* <span data-ttu-id="3c1c2-207">若要卸載透過 `filesystem` 提供者安裝的 Lodash 所檔案：</span><span class="sxs-lookup"><span data-stu-id="3c1c2-207">To uninstall the Lodash files installed via the `filesystem` provider:</span></span>

  ```console
  libman uninstall C:\temp\lodash\
  ```

## <a name="update-library-version"></a><span data-ttu-id="3c1c2-208">更新程式庫版本</span><span class="sxs-lookup"><span data-stu-id="3c1c2-208">Update library version</span></span>

<span data-ttu-id="3c1c2-209">`libman update` 命令會將透過 LibMan 安裝的程式庫更新為指定的版本。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-209">The `libman update` command updates a library installed via LibMan to the specified version.</span></span>

<span data-ttu-id="3c1c2-210">當下列情況發生錯誤：</span><span class="sxs-lookup"><span data-stu-id="3c1c2-210">An error occurs when:</span></span>

* <span data-ttu-id="3c1c2-211">專案根目錄中沒有*libman*檔案存在。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-211">No *libman.json* file exists in the project root.</span></span>
* <span data-ttu-id="3c1c2-212">指定的程式庫不存在。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-212">The specified library doesn't exist.</span></span>

<span data-ttu-id="3c1c2-213">如果安裝了多個具有相同名稱的程式庫，系統會提示您選擇其中一個。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-213">If more than one library with the same name is installed, you're prompted to choose one.</span></span>

### <a name="synopsis"></a><span data-ttu-id="3c1c2-214">概要</span><span class="sxs-lookup"><span data-stu-id="3c1c2-214">Synopsis</span></span>

```console
libman update <LIBRARY> [-pre] [--to] [--verbosity]
libman update [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="3c1c2-215">引數</span><span class="sxs-lookup"><span data-stu-id="3c1c2-215">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="3c1c2-216">要更新之程式庫的名稱。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-216">The name of the library to update.</span></span>

### <a name="options"></a><span data-ttu-id="3c1c2-217">選項。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-217">Options</span></span>

<span data-ttu-id="3c1c2-218">以下是使用 `libman update` 命令時可用的選項：</span><span class="sxs-lookup"><span data-stu-id="3c1c2-218">The following options are available for the `libman update` command:</span></span>

* `-pre`

  <span data-ttu-id="3c1c2-219">取得程式庫的最新發行前版本。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-219">Obtain the latest prerelease version of the library.</span></span>

* `--to <VERSION>`

  <span data-ttu-id="3c1c2-220">取得特定版本的程式庫。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-220">Obtain a specific version of the library.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="3c1c2-221">範例</span><span class="sxs-lookup"><span data-stu-id="3c1c2-221">Examples</span></span>

* <span data-ttu-id="3c1c2-222">若要將 jQuery 更新為最新版本：</span><span class="sxs-lookup"><span data-stu-id="3c1c2-222">To update jQuery to the latest version:</span></span>

  ```console
  libman update jquery
  ```

* <span data-ttu-id="3c1c2-223">若要將 jQuery 更新為版本3.3.1：</span><span class="sxs-lookup"><span data-stu-id="3c1c2-223">To update jQuery to version 3.3.1:</span></span>

  ```console
  libman update jquery --to 3.3.1
  ```

* <span data-ttu-id="3c1c2-224">若要將 jQuery 更新為最新的發行前版本：</span><span class="sxs-lookup"><span data-stu-id="3c1c2-224">To update jQuery to the latest prerelease version:</span></span>

  ```console
  libman update jquery -pre
  ```

## <a name="manage-library-cache"></a><span data-ttu-id="3c1c2-225">管理程式庫快取</span><span class="sxs-lookup"><span data-stu-id="3c1c2-225">Manage library cache</span></span>

<span data-ttu-id="3c1c2-226">`libman cache` 命令會管理 LibMan 程式庫快取。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-226">The `libman cache` command manages the LibMan library cache.</span></span> <span data-ttu-id="3c1c2-227">`filesystem` 提供者不會使用程式庫快取。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-227">The `filesystem` provider doesn't use the library cache.</span></span>

### <a name="synopsis"></a><span data-ttu-id="3c1c2-228">概要</span><span class="sxs-lookup"><span data-stu-id="3c1c2-228">Synopsis</span></span>

```console
libman cache clean [<PROVIDER>] [--verbosity]
libman cache list [--files] [--libraries] [--verbosity]
libman cache [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="3c1c2-229">引數</span><span class="sxs-lookup"><span data-stu-id="3c1c2-229">Arguments</span></span>

`PROVIDER`

<span data-ttu-id="3c1c2-230">僅與 `clean` 命令搭配使用。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-230">Only used with the `clean` command.</span></span> <span data-ttu-id="3c1c2-231">指定要清除的提供者快取。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-231">Specifies the provider cache to clean.</span></span> <span data-ttu-id="3c1c2-232">有效值包括：</span><span class="sxs-lookup"><span data-stu-id="3c1c2-232">Valid values include:</span></span>

[!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

### <a name="options"></a><span data-ttu-id="3c1c2-233">選項。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-233">Options</span></span>

<span data-ttu-id="3c1c2-234">以下是使用 `libman cache` 命令時可用的選項：</span><span class="sxs-lookup"><span data-stu-id="3c1c2-234">The following options are available for the `libman cache` command:</span></span>

* `--files`

  <span data-ttu-id="3c1c2-235">列出快取的檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-235">List the names of files that are cached.</span></span>

* `--libraries`

  <span data-ttu-id="3c1c2-236">列出快取的程式庫名稱。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-236">List the names of libraries that are cached.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="3c1c2-237">範例</span><span class="sxs-lookup"><span data-stu-id="3c1c2-237">Examples</span></span>

* <span data-ttu-id="3c1c2-238">若要查看每個提供者的快取程式庫名稱，請使用下列其中一個命令：</span><span class="sxs-lookup"><span data-stu-id="3c1c2-238">To view the names of cached libraries per provider, use one of the following commands:</span></span>

  ```console
  libman cache list
  ```

  ```console
  libman cache list --libraries
  ```

  <span data-ttu-id="3c1c2-239">會顯示類似下列的輸出：</span><span class="sxs-lookup"><span data-stu-id="3c1c2-239">Output similar to the following is displayed:</span></span>

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

* <span data-ttu-id="3c1c2-240">若要查看每個提供者的快取程式庫檔案的名稱：</span><span class="sxs-lookup"><span data-stu-id="3c1c2-240">To view the names of cached library files per provider:</span></span>

  ```console
  libman cache list --files
  ```

  <span data-ttu-id="3c1c2-241">會顯示類似下列的輸出：</span><span class="sxs-lookup"><span data-stu-id="3c1c2-241">Output similar to the following is displayed:</span></span>

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

  <span data-ttu-id="3c1c2-242">請注意，上述輸出顯示 jQuery 版本3.2.1 和3.3.1 會在 CDNJS 提供者底下快取。</span><span class="sxs-lookup"><span data-stu-id="3c1c2-242">Notice the preceding output shows that jQuery versions 3.2.1 and 3.3.1 are cached under the CDNJS provider.</span></span>

* <span data-ttu-id="3c1c2-243">若要清空 CDNJS 提供者的程式庫快取：</span><span class="sxs-lookup"><span data-stu-id="3c1c2-243">To empty the library cache for the CDNJS provider:</span></span>

  ```console
  libman cache clean cdnjs
  ```

  <span data-ttu-id="3c1c2-244">清空 CDNJS 提供者快取之後，`libman cache list` 命令會顯示下列內容：</span><span class="sxs-lookup"><span data-stu-id="3c1c2-244">After emptying the CDNJS provider cache, the `libman cache list` command displays the following:</span></span>

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

* <span data-ttu-id="3c1c2-245">若要清空所有支援之提供者的快取：</span><span class="sxs-lookup"><span data-stu-id="3c1c2-245">To empty the cache for all supported providers:</span></span>

  ```console
  libman cache clean
  ```

  <span data-ttu-id="3c1c2-246">清空所有提供者快取之後，`libman cache list` 命令會顯示下列內容：</span><span class="sxs-lookup"><span data-stu-id="3c1c2-246">After emptying all provider caches, the `libman cache list` command displays the following:</span></span>

  ```console
  Cache contents:
  ---------------
  unpkg:
      (empty)
  cdnjs:
      (empty)
  ```

## <a name="additional-resources"></a><span data-ttu-id="3c1c2-247">其他資源</span><span class="sxs-lookup"><span data-stu-id="3c1c2-247">Additional resources</span></span>

* [<span data-ttu-id="3c1c2-248">安裝通用工具</span><span class="sxs-lookup"><span data-stu-id="3c1c2-248">Install a Global Tool</span></span>](/dotnet/core/tools/global-tools#install-a-global-tool)
* <xref:client-side/libman/libman-vs>
* [<span data-ttu-id="3c1c2-249">LibMan GitHub 存放庫</span><span class="sxs-lookup"><span data-stu-id="3c1c2-249">LibMan GitHub repository</span></span>](https://github.com/aspnet/LibraryManager)
