---
title: 將 LibMan CLI 與ASP.NET核心
author: scottaddie
description: 瞭解如何在ASP.NET核心專案中使用 LibMan CLI。
ms.author: scaddie
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: client-side/libman/libman-cli
ms.openlocfilehash: 02d88d09805bd23a86ef924766373245fec7ff52
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78664630"
---
# <a name="use-the-libman-cli-with-aspnet-core"></a><span data-ttu-id="3c46e-103">將 LibMan CLI 與ASP.NET核心</span><span class="sxs-lookup"><span data-stu-id="3c46e-103">Use the LibMan CLI with ASP.NET Core</span></span>

<span data-ttu-id="3c46e-104">作者：[Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="3c46e-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="3c46e-105">[LibMan](xref:client-side/libman/index) CLI 是一個跨平臺工具,支援無處不在 .NET Core。</span><span class="sxs-lookup"><span data-stu-id="3c46e-105">The [LibMan](xref:client-side/libman/index) CLI is a cross-platform tool that's supported everywhere .NET Core is supported.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3c46e-106">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="3c46e-106">Prerequisites</span></span>

* [!INCLUDE [2.1-SDK](../../includes/2.1-SDK.md)]

## <a name="installation"></a><span data-ttu-id="3c46e-107">安裝</span><span class="sxs-lookup"><span data-stu-id="3c46e-107">Installation</span></span>

<span data-ttu-id="3c46e-108">要安裝 LibMan CLI:</span><span class="sxs-lookup"><span data-stu-id="3c46e-108">To install the LibMan CLI:</span></span>

```dotnetcli
dotnet tool install -g Microsoft.Web.LibraryManager.Cli
```

<span data-ttu-id="3c46e-109">從[Microsoft.Web.LibraryManager.Cli](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Cli/) NuGet 套件中安裝了[.NET 核心全域工具](/dotnet/core/tools/global-tools#install-a-global-tool)。</span><span class="sxs-lookup"><span data-stu-id="3c46e-109">A [.NET Core Global Tool](/dotnet/core/tools/global-tools#install-a-global-tool) is installed from the [Microsoft.Web.LibraryManager.Cli](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Cli/) NuGet package.</span></span>

<span data-ttu-id="3c46e-110">要從特定的 NuGet 套件來源安裝 LibMan CLI,請執行以下規定:</span><span class="sxs-lookup"><span data-stu-id="3c46e-110">To install the LibMan CLI from a specific NuGet package source:</span></span>

```dotnetcli
dotnet tool install -g Microsoft.Web.LibraryManager.Cli --version 1.0.94-g606058a278 --add-source C:\Temp\
```

<span data-ttu-id="3c46e-111">在前面的示例中,從本地 Windows 計算機的*C:\Temp_Microsoft.Web.LibraryManager.Cli.1.0.94-g606058a278.nupkg*檔中安裝了 .NET 核心全域工具。</span><span class="sxs-lookup"><span data-stu-id="3c46e-111">In the preceding example, a .NET Core Global Tool is installed from the local Windows machine's *C:\Temp\Microsoft.Web.LibraryManager.Cli.1.0.94-g606058a278.nupkg* file.</span></span>

## <a name="usage"></a><span data-ttu-id="3c46e-112">使用量</span><span class="sxs-lookup"><span data-stu-id="3c46e-112">Usage</span></span>

<span data-ttu-id="3c46e-113">成功安裝 CLI 後,可以使用以下指令:</span><span class="sxs-lookup"><span data-stu-id="3c46e-113">After successful installation of the CLI, the following command can be used:</span></span>

```console
libman
```

<span data-ttu-id="3c46e-114">要檢視已安裝的 CLI 版本:</span><span class="sxs-lookup"><span data-stu-id="3c46e-114">To view the installed CLI version:</span></span>

```console
libman --version
```

<span data-ttu-id="3c46e-115">要檢視可用的 CLI 指令:</span><span class="sxs-lookup"><span data-stu-id="3c46e-115">To view the available CLI commands:</span></span>

```console
libman --help
```

<span data-ttu-id="3c46e-116">前面的指令顯示類似於以下內容的輸出:</span><span class="sxs-lookup"><span data-stu-id="3c46e-116">The preceding command displays output similar to the following:</span></span>

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

<span data-ttu-id="3c46e-117">下列各節將概述可用的 CLI 命令。</span><span class="sxs-lookup"><span data-stu-id="3c46e-117">The following sections outline the available CLI commands.</span></span>

## <a name="initialize-libman-in-the-project"></a><span data-ttu-id="3c46e-118">在項目中初始化 LibMan</span><span class="sxs-lookup"><span data-stu-id="3c46e-118">Initialize LibMan in the project</span></span>

<span data-ttu-id="3c46e-119">如果`libman init`不存在,該命令將創建一個*libman.json*檔。</span><span class="sxs-lookup"><span data-stu-id="3c46e-119">The `libman init` command creates a *libman.json* file if one doesn't exist.</span></span> <span data-ttu-id="3c46e-120">該檔使用預設專案範本內容創建。</span><span class="sxs-lookup"><span data-stu-id="3c46e-120">The file is created with the default item template content.</span></span>

### <a name="synopsis"></a><span data-ttu-id="3c46e-121">概要</span><span class="sxs-lookup"><span data-stu-id="3c46e-121">Synopsis</span></span>

```console
libman init [-d|--default-destination] [-p|--default-provider] [--verbosity]
libman init [-h|--help]
```

### <a name="options"></a><span data-ttu-id="3c46e-122">選項。</span><span class="sxs-lookup"><span data-stu-id="3c46e-122">Options</span></span>

<span data-ttu-id="3c46e-123">以下是使用 `libman init` 命令時可用的選項：</span><span class="sxs-lookup"><span data-stu-id="3c46e-123">The following options are available for the `libman init` command:</span></span>

* `-d|--default-destination <PATH>`

  <span data-ttu-id="3c46e-124">相對於當前資料夾的路徑。</span><span class="sxs-lookup"><span data-stu-id="3c46e-124">A path relative to the current folder.</span></span> <span data-ttu-id="3c46e-125">如果沒有`destination`為*libman.json*中的庫定義屬性,則庫檔將安裝在此位置。</span><span class="sxs-lookup"><span data-stu-id="3c46e-125">Library files are installed in this location if no `destination` property is defined for a library in *libman.json*.</span></span> <span data-ttu-id="3c46e-126">該`<PATH>`值寫`defaultDestination`入*利曼.json*的屬性。</span><span class="sxs-lookup"><span data-stu-id="3c46e-126">The `<PATH>` value is written to the `defaultDestination` property of *libman.json*.</span></span>

* `-p|--default-provider <PROVIDER>`

  <span data-ttu-id="3c46e-127">如果未為給定庫定義提供程式,則要使用的提供程式。</span><span class="sxs-lookup"><span data-stu-id="3c46e-127">The provider to use if no provider is defined for a given library.</span></span> <span data-ttu-id="3c46e-128">該`<PROVIDER>`值寫`defaultProvider`入*利曼.json*的屬性。</span><span class="sxs-lookup"><span data-stu-id="3c46e-128">The `<PROVIDER>` value is written to the `defaultProvider` property of *libman.json*.</span></span> <span data-ttu-id="3c46e-129">取代為`<PROVIDER>`以下值之一:</span><span class="sxs-lookup"><span data-stu-id="3c46e-129">Replace `<PROVIDER>` with one of the following values:</span></span>

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="3c46e-130">範例</span><span class="sxs-lookup"><span data-stu-id="3c46e-130">Examples</span></span>

<span data-ttu-id="3c46e-131">在ASP.NET核心項目中建立*libman.json*檔,請進行:</span><span class="sxs-lookup"><span data-stu-id="3c46e-131">To create a *libman.json* file in an ASP.NET Core project:</span></span>

* <span data-ttu-id="3c46e-132">導航到專案根目錄。</span><span class="sxs-lookup"><span data-stu-id="3c46e-132">Navigate to the project root.</span></span>
* <span data-ttu-id="3c46e-133">執行以下命令：</span><span class="sxs-lookup"><span data-stu-id="3c46e-133">Run the following command:</span></span>

  ```console
  libman init
  ```

* <span data-ttu-id="3c46e-134">鍵入預設提供程式的名稱,或按`Enter`使用預設 CDNJS 提供程式。</span><span class="sxs-lookup"><span data-stu-id="3c46e-134">Type the name of the default provider, or press `Enter` to use the default CDNJS provider.</span></span> <span data-ttu-id="3c46e-135">有效值包括：</span><span class="sxs-lookup"><span data-stu-id="3c46e-135">Valid values include:</span></span>

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  ![libman init 指令 ─ 預設提供者](_static/libman-init-provider.png)

<span data-ttu-id="3c46e-137">*libman.json*檔已添加到專案根中,內容如下:</span><span class="sxs-lookup"><span data-stu-id="3c46e-137">A *libman.json* file is added to the project root with the following content:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

## <a name="add-library-files"></a><span data-ttu-id="3c46e-138">新增函式庫檔案</span><span class="sxs-lookup"><span data-stu-id="3c46e-138">Add library files</span></span>

<span data-ttu-id="3c46e-139">該`libman install`命令下載庫檔並將其安裝到專案中。</span><span class="sxs-lookup"><span data-stu-id="3c46e-139">The `libman install` command downloads and installs library files into the project.</span></span> <span data-ttu-id="3c46e-140">如果不存在,將添加*libman.json*檔。</span><span class="sxs-lookup"><span data-stu-id="3c46e-140">A *libman.json* file is added if one doesn't exist.</span></span> <span data-ttu-id="3c46e-141">*libman.json*檔被修改以儲存庫檔的配置詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="3c46e-141">The *libman.json* file is modified to store configuration details for the library files.</span></span>

### <a name="synopsis"></a><span data-ttu-id="3c46e-142">概要</span><span class="sxs-lookup"><span data-stu-id="3c46e-142">Synopsis</span></span>

```console
libman install <LIBRARY> [-d|--destination] [--files] [-p|--provider] [--verbosity]
libman install [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="3c46e-143">引數</span><span class="sxs-lookup"><span data-stu-id="3c46e-143">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="3c46e-144">要安裝的庫的名稱。</span><span class="sxs-lookup"><span data-stu-id="3c46e-144">The name of the library to install.</span></span> <span data-ttu-id="3c46e-145">此名稱可能包含版本號表示法(例如, `@1.2.0`。</span><span class="sxs-lookup"><span data-stu-id="3c46e-145">This name may include version number notation (for example, `@1.2.0`).</span></span>

### <a name="options"></a><span data-ttu-id="3c46e-146">選項。</span><span class="sxs-lookup"><span data-stu-id="3c46e-146">Options</span></span>

<span data-ttu-id="3c46e-147">以下是使用 `libman install` 命令時可用的選項：</span><span class="sxs-lookup"><span data-stu-id="3c46e-147">The following options are available for the `libman install` command:</span></span>

* `-d|--destination <PATH>`

  <span data-ttu-id="3c46e-148">要安裝庫的位置。</span><span class="sxs-lookup"><span data-stu-id="3c46e-148">The location to install the library.</span></span> <span data-ttu-id="3c46e-149">如果未指定,則使用預設位置。</span><span class="sxs-lookup"><span data-stu-id="3c46e-149">If not specified, the default location is used.</span></span> <span data-ttu-id="3c46e-150">如果在`defaultDestination`*libman.json*中未指定任何屬性,則需要此選項。</span><span class="sxs-lookup"><span data-stu-id="3c46e-150">If no `defaultDestination` property is specified in *libman.json*, this option is required.</span></span>

* `--files <FILE>`

  <span data-ttu-id="3c46e-151">指定要從庫中安裝的檔案的名稱。</span><span class="sxs-lookup"><span data-stu-id="3c46e-151">Specify the name of the file to install from the library.</span></span> <span data-ttu-id="3c46e-152">如果未指定,則安裝庫中的所有檔。</span><span class="sxs-lookup"><span data-stu-id="3c46e-152">If not specified, all files from the library are installed.</span></span> <span data-ttu-id="3c46e-153">每個要`--files`安裝的檔案提供一個選項。</span><span class="sxs-lookup"><span data-stu-id="3c46e-153">Provide one `--files` option per file to be installed.</span></span> <span data-ttu-id="3c46e-154">也支持相對路徑。</span><span class="sxs-lookup"><span data-stu-id="3c46e-154">Relative paths are supported too.</span></span> <span data-ttu-id="3c46e-155">例如： `--files dist/browser/signalr.js` 。</span><span class="sxs-lookup"><span data-stu-id="3c46e-155">For example: `--files dist/browser/signalr.js`.</span></span>

* `-p|--provider <PROVIDER>`

  <span data-ttu-id="3c46e-156">用於庫採集的提供程式的名稱。</span><span class="sxs-lookup"><span data-stu-id="3c46e-156">The name of the provider to use for the library acquisition.</span></span> <span data-ttu-id="3c46e-157">取代為`<PROVIDER>`以下值之一:</span><span class="sxs-lookup"><span data-stu-id="3c46e-157">Replace `<PROVIDER>` with one of the following values:</span></span>
  
  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  <span data-ttu-id="3c46e-158">如果未指定,`defaultProvider`則使用*libman.json*中的屬性。</span><span class="sxs-lookup"><span data-stu-id="3c46e-158">If not specified, the `defaultProvider` property in *libman.json* is used.</span></span> <span data-ttu-id="3c46e-159">如果在`defaultProvider`*libman.json*中未指定任何屬性,則需要此選項。</span><span class="sxs-lookup"><span data-stu-id="3c46e-159">If no `defaultProvider` property is specified in *libman.json*, this option is required.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="3c46e-160">範例</span><span class="sxs-lookup"><span data-stu-id="3c46e-160">Examples</span></span>

<span data-ttu-id="3c46e-161">請考慮以下*利伯曼.json*檔:</span><span class="sxs-lookup"><span data-stu-id="3c46e-161">Consider the following *libman.json* file:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

<span data-ttu-id="3c46e-162">要使用 CDNJS 提供者將 jQuery 版本 3.2.1 *jquery.min.js*檔案安裝到*wwwroot/文稿/jquery*資料夾:</span><span class="sxs-lookup"><span data-stu-id="3c46e-162">To install the jQuery version 3.2.1 *jquery.min.js* file to the *wwwroot/scripts/jquery* folder using the CDNJS provider:</span></span>

```console
libman install jquery@3.2.1 --provider cdnjs --destination wwwroot/scripts/jquery --files jquery.min.js
```

<span data-ttu-id="3c46e-163">*libman.json*檔案類似於以下內容:</span><span class="sxs-lookup"><span data-stu-id="3c46e-163">The *libman.json* file resembles the following:</span></span>

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

<span data-ttu-id="3c46e-164">要使用檔案系統提供者從*\\C:\\暫時\\contosoCalendar*安裝*calendar.js*與*calendar.css*檔案:</span><span class="sxs-lookup"><span data-stu-id="3c46e-164">To install the *calendar.js* and *calendar.css* files from *C:\\temp\\contosoCalendar\\* using the file system provider:</span></span>

  ```console
  libman install C:\temp\contosoCalendar\ --provider filesystem --files calendar.js --files calendar.css
  ```

<span data-ttu-id="3c46e-165">發生以下提示有兩個原因:</span><span class="sxs-lookup"><span data-stu-id="3c46e-165">The following prompt appears for two reasons:</span></span>

* <span data-ttu-id="3c46e-166">*libman.json*檔不包含`defaultDestination`屬性 。</span><span class="sxs-lookup"><span data-stu-id="3c46e-166">The *libman.json* file doesn't contain a `defaultDestination` property.</span></span>
* <span data-ttu-id="3c46e-167">該`libman install`命令不包含`-d|--destination`該 選項。</span><span class="sxs-lookup"><span data-stu-id="3c46e-167">The `libman install` command doesn't contain the `-d|--destination` option.</span></span>

![利伯曼安裝命令 - 目標](_static/libman-install-destination.png)

<span data-ttu-id="3c46e-169">在接受預設目標後 *,libman.json*檔案類似於以下內容:</span><span class="sxs-lookup"><span data-stu-id="3c46e-169">After accepting the default destination, the *libman.json* file resembles the following:</span></span>

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

## <a name="restore-library-files"></a><span data-ttu-id="3c46e-170">復原庫檔案</span><span class="sxs-lookup"><span data-stu-id="3c46e-170">Restore library files</span></span>

<span data-ttu-id="3c46e-171">該`libman restore`命令安裝在*libman.json*中定義的庫檔。</span><span class="sxs-lookup"><span data-stu-id="3c46e-171">The `libman restore` command installs library files defined in *libman.json*.</span></span> <span data-ttu-id="3c46e-172">適用的規則如下：</span><span class="sxs-lookup"><span data-stu-id="3c46e-172">The following rules apply:</span></span>

* <span data-ttu-id="3c46e-173">如果專案根中不存在*libman.json*檔,則傳回錯誤。</span><span class="sxs-lookup"><span data-stu-id="3c46e-173">If no *libman.json* file exists in the project root, an error is returned.</span></span>
* <span data-ttu-id="3c46e-174">如果函式庫指定提供`defaultProvider`者, 則忽略*libman.json*中的屬性。</span><span class="sxs-lookup"><span data-stu-id="3c46e-174">If a library specifies a provider, the `defaultProvider` property in *libman.json* is ignored.</span></span>
* <span data-ttu-id="3c46e-175">如果函式庫指定了`defaultDestination`目標, 則忽略*libman.json*中的屬性。</span><span class="sxs-lookup"><span data-stu-id="3c46e-175">If a library specifies a destination, the `defaultDestination` property in *libman.json* is ignored.</span></span>

### <a name="synopsis"></a><span data-ttu-id="3c46e-176">概要</span><span class="sxs-lookup"><span data-stu-id="3c46e-176">Synopsis</span></span>

```console
libman restore [--verbosity]
libman restore [-h|--help]
```

### <a name="options"></a><span data-ttu-id="3c46e-177">選項。</span><span class="sxs-lookup"><span data-stu-id="3c46e-177">Options</span></span>

<span data-ttu-id="3c46e-178">以下是使用 `libman restore` 命令時可用的選項：</span><span class="sxs-lookup"><span data-stu-id="3c46e-178">The following options are available for the `libman restore` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="3c46e-179">範例</span><span class="sxs-lookup"><span data-stu-id="3c46e-179">Examples</span></span>

<span data-ttu-id="3c46e-180">要回復*Libman.json*中定義的庫檔:</span><span class="sxs-lookup"><span data-stu-id="3c46e-180">To restore the library files defined in *libman.json*:</span></span>

```console
libman restore
```

## <a name="delete-library-files"></a><span data-ttu-id="3c46e-181">刪除庫檔案</span><span class="sxs-lookup"><span data-stu-id="3c46e-181">Delete library files</span></span>

<span data-ttu-id="3c46e-182">該`libman clean`指令刪除以前透過 LibMan 還原的庫檔。</span><span class="sxs-lookup"><span data-stu-id="3c46e-182">The `libman clean` command deletes library files previously restored via LibMan.</span></span> <span data-ttu-id="3c46e-183">此操作後變為空的資料夾將被刪除。</span><span class="sxs-lookup"><span data-stu-id="3c46e-183">Folders that become empty after this operation are deleted.</span></span> <span data-ttu-id="3c46e-184">`libraries` *libman.json*屬性中的庫文件關聯配置不會被刪除。</span><span class="sxs-lookup"><span data-stu-id="3c46e-184">The library files' associated configurations in the `libraries` property of *libman.json* aren't removed.</span></span>

### <a name="synopsis"></a><span data-ttu-id="3c46e-185">概要</span><span class="sxs-lookup"><span data-stu-id="3c46e-185">Synopsis</span></span>

```console
libman clean [--verbosity]
libman clean [-h|--help]
```

### <a name="options"></a><span data-ttu-id="3c46e-186">選項。</span><span class="sxs-lookup"><span data-stu-id="3c46e-186">Options</span></span>

<span data-ttu-id="3c46e-187">以下是使用 `libman clean` 命令時可用的選項：</span><span class="sxs-lookup"><span data-stu-id="3c46e-187">The following options are available for the `libman clean` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="3c46e-188">範例</span><span class="sxs-lookup"><span data-stu-id="3c46e-188">Examples</span></span>

<span data-ttu-id="3c46e-189">要刪除透過 LibMan 安裝的庫檔,</span><span class="sxs-lookup"><span data-stu-id="3c46e-189">To delete library files installed via LibMan:</span></span>

```console
libman clean
```

## <a name="uninstall-library-files"></a><span data-ttu-id="3c46e-190">卸載庫檔案</span><span class="sxs-lookup"><span data-stu-id="3c46e-190">Uninstall library files</span></span>

<span data-ttu-id="3c46e-191">指令`libman uninstall`:</span><span class="sxs-lookup"><span data-stu-id="3c46e-191">The `libman uninstall` command:</span></span>

* <span data-ttu-id="3c46e-192">從*libman.json*中的目標中刪除與指定庫關聯的所有檔。</span><span class="sxs-lookup"><span data-stu-id="3c46e-192">Deletes all files associated with the specified library from the destination in *libman.json*.</span></span>
* <span data-ttu-id="3c46e-193">從*libman.json*中刪除關聯的庫配置。</span><span class="sxs-lookup"><span data-stu-id="3c46e-193">Removes the associated library configuration from *libman.json*.</span></span>

<span data-ttu-id="3c46e-194">在以下情況時發生錯誤:</span><span class="sxs-lookup"><span data-stu-id="3c46e-194">An error occurs when:</span></span>

* <span data-ttu-id="3c46e-195">專案根中不存在*libman.json*檔。</span><span class="sxs-lookup"><span data-stu-id="3c46e-195">No *libman.json* file exists in the project root.</span></span>
* <span data-ttu-id="3c46e-196">指定的庫不存在。</span><span class="sxs-lookup"><span data-stu-id="3c46e-196">The specified library doesn't exist.</span></span>

<span data-ttu-id="3c46e-197">如果安裝了多個同名庫,系統將提示您選擇一個庫。</span><span class="sxs-lookup"><span data-stu-id="3c46e-197">If more than one library with the same name is installed, you're prompted to choose one.</span></span>

### <a name="synopsis"></a><span data-ttu-id="3c46e-198">概要</span><span class="sxs-lookup"><span data-stu-id="3c46e-198">Synopsis</span></span>

```console
libman uninstall <LIBRARY> [--verbosity]
libman uninstall [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="3c46e-199">引數</span><span class="sxs-lookup"><span data-stu-id="3c46e-199">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="3c46e-200">要卸載的庫的名稱。</span><span class="sxs-lookup"><span data-stu-id="3c46e-200">The name of the library to uninstall.</span></span> <span data-ttu-id="3c46e-201">此名稱可能包含版本號表示法(例如, `@1.2.0`。</span><span class="sxs-lookup"><span data-stu-id="3c46e-201">This name may include version number notation (for example, `@1.2.0`).</span></span>

### <a name="options"></a><span data-ttu-id="3c46e-202">選項。</span><span class="sxs-lookup"><span data-stu-id="3c46e-202">Options</span></span>

<span data-ttu-id="3c46e-203">以下是使用 `libman uninstall` 命令時可用的選項：</span><span class="sxs-lookup"><span data-stu-id="3c46e-203">The following options are available for the `libman uninstall` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="3c46e-204">範例</span><span class="sxs-lookup"><span data-stu-id="3c46e-204">Examples</span></span>

<span data-ttu-id="3c46e-205">請考慮以下*利伯曼.json*檔:</span><span class="sxs-lookup"><span data-stu-id="3c46e-205">Consider the following *libman.json* file:</span></span>

[!code-json[](samples/LibManSample/libman.json)]

* <span data-ttu-id="3c46e-206">要卸載 jQuery,以下任一命令都成功:</span><span class="sxs-lookup"><span data-stu-id="3c46e-206">To uninstall jQuery, either of the following commands succeed:</span></span>

  ```console
  libman uninstall jquery
  ```

  ```console
  libman uninstall jquery@3.3.1
  ```

* <span data-ttu-id="3c46e-207">要卸載透過提供程式安裝的 Lodash`filesystem`檔,請執行以下操作:</span><span class="sxs-lookup"><span data-stu-id="3c46e-207">To uninstall the Lodash files installed via the `filesystem` provider:</span></span>

  ```console
  libman uninstall C:\temp\lodash\
  ```

## <a name="update-library-version"></a><span data-ttu-id="3c46e-208">更新函式庫版本</span><span class="sxs-lookup"><span data-stu-id="3c46e-208">Update library version</span></span>

<span data-ttu-id="3c46e-209">該`libman update`命令將透過 LibMan 安裝的庫更新為指定的版本。</span><span class="sxs-lookup"><span data-stu-id="3c46e-209">The `libman update` command updates a library installed via LibMan to the specified version.</span></span>

<span data-ttu-id="3c46e-210">在以下情況時發生錯誤:</span><span class="sxs-lookup"><span data-stu-id="3c46e-210">An error occurs when:</span></span>

* <span data-ttu-id="3c46e-211">專案根中不存在*libman.json*檔。</span><span class="sxs-lookup"><span data-stu-id="3c46e-211">No *libman.json* file exists in the project root.</span></span>
* <span data-ttu-id="3c46e-212">指定的庫不存在。</span><span class="sxs-lookup"><span data-stu-id="3c46e-212">The specified library doesn't exist.</span></span>

<span data-ttu-id="3c46e-213">如果安裝了多個同名庫,系統將提示您選擇一個庫。</span><span class="sxs-lookup"><span data-stu-id="3c46e-213">If more than one library with the same name is installed, you're prompted to choose one.</span></span>

### <a name="synopsis"></a><span data-ttu-id="3c46e-214">概要</span><span class="sxs-lookup"><span data-stu-id="3c46e-214">Synopsis</span></span>

```console
libman update <LIBRARY> [-pre] [--to] [--verbosity]
libman update [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="3c46e-215">引數</span><span class="sxs-lookup"><span data-stu-id="3c46e-215">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="3c46e-216">要更新的庫的名稱。</span><span class="sxs-lookup"><span data-stu-id="3c46e-216">The name of the library to update.</span></span>

### <a name="options"></a><span data-ttu-id="3c46e-217">選項。</span><span class="sxs-lookup"><span data-stu-id="3c46e-217">Options</span></span>

<span data-ttu-id="3c46e-218">以下是使用 `libman update` 命令時可用的選項：</span><span class="sxs-lookup"><span data-stu-id="3c46e-218">The following options are available for the `libman update` command:</span></span>

* `-pre`

  <span data-ttu-id="3c46e-219">獲取庫的最新預發行版本。</span><span class="sxs-lookup"><span data-stu-id="3c46e-219">Obtain the latest prerelease version of the library.</span></span>

* `--to <VERSION>`

  <span data-ttu-id="3c46e-220">獲取庫的特定版本。</span><span class="sxs-lookup"><span data-stu-id="3c46e-220">Obtain a specific version of the library.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="3c46e-221">範例</span><span class="sxs-lookup"><span data-stu-id="3c46e-221">Examples</span></span>

* <span data-ttu-id="3c46e-222">要將 jQuery 更新到最新版本:</span><span class="sxs-lookup"><span data-stu-id="3c46e-222">To update jQuery to the latest version:</span></span>

  ```console
  libman update jquery
  ```

* <span data-ttu-id="3c46e-223">要將 jQuery 更新到版本 3.3.1:</span><span class="sxs-lookup"><span data-stu-id="3c46e-223">To update jQuery to version 3.3.1:</span></span>

  ```console
  libman update jquery --to 3.3.1
  ```

* <span data-ttu-id="3c46e-224">要將 jQuery 更新到最新的預發行版本:</span><span class="sxs-lookup"><span data-stu-id="3c46e-224">To update jQuery to the latest prerelease version:</span></span>

  ```console
  libman update jquery -pre
  ```

## <a name="manage-library-cache"></a><span data-ttu-id="3c46e-225">管理庫快取</span><span class="sxs-lookup"><span data-stu-id="3c46e-225">Manage library cache</span></span>

<span data-ttu-id="3c46e-226">該`libman cache`命令管理 LibMan 庫緩存。</span><span class="sxs-lookup"><span data-stu-id="3c46e-226">The `libman cache` command manages the LibMan library cache.</span></span> <span data-ttu-id="3c46e-227">提供程式`filesystem`不使用庫緩存。</span><span class="sxs-lookup"><span data-stu-id="3c46e-227">The `filesystem` provider doesn't use the library cache.</span></span>

### <a name="synopsis"></a><span data-ttu-id="3c46e-228">概要</span><span class="sxs-lookup"><span data-stu-id="3c46e-228">Synopsis</span></span>

```console
libman cache clean [<PROVIDER>] [--verbosity]
libman cache list [--files] [--libraries] [--verbosity]
libman cache [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="3c46e-229">引數</span><span class="sxs-lookup"><span data-stu-id="3c46e-229">Arguments</span></span>

`PROVIDER`

<span data-ttu-id="3c46e-230">僅與命令`clean`一起使用。</span><span class="sxs-lookup"><span data-stu-id="3c46e-230">Only used with the `clean` command.</span></span> <span data-ttu-id="3c46e-231">指定要清理的提供程式快取。</span><span class="sxs-lookup"><span data-stu-id="3c46e-231">Specifies the provider cache to clean.</span></span> <span data-ttu-id="3c46e-232">有效值包括：</span><span class="sxs-lookup"><span data-stu-id="3c46e-232">Valid values include:</span></span>

[!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

### <a name="options"></a><span data-ttu-id="3c46e-233">選項。</span><span class="sxs-lookup"><span data-stu-id="3c46e-233">Options</span></span>

<span data-ttu-id="3c46e-234">以下是使用 `libman cache` 命令時可用的選項：</span><span class="sxs-lookup"><span data-stu-id="3c46e-234">The following options are available for the `libman cache` command:</span></span>

* `--files`

  <span data-ttu-id="3c46e-235">列出快取的檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="3c46e-235">List the names of files that are cached.</span></span>

* `--libraries`

  <span data-ttu-id="3c46e-236">列出緩存的庫的名稱。</span><span class="sxs-lookup"><span data-stu-id="3c46e-236">List the names of libraries that are cached.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="3c46e-237">範例</span><span class="sxs-lookup"><span data-stu-id="3c46e-237">Examples</span></span>

* <span data-ttu-id="3c46e-238">要檢視每個提供程式快取庫的名稱,請使用以下指令之一:</span><span class="sxs-lookup"><span data-stu-id="3c46e-238">To view the names of cached libraries per provider, use one of the following commands:</span></span>

  ```console
  libman cache list
  ```

  ```console
  libman cache list --libraries
  ```

  <span data-ttu-id="3c46e-239">會顯示類似下列的輸出：</span><span class="sxs-lookup"><span data-stu-id="3c46e-239">Output similar to the following is displayed:</span></span>

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

* <span data-ttu-id="3c46e-240">要查看每個提供程式快取的庫文件的名稱,</span><span class="sxs-lookup"><span data-stu-id="3c46e-240">To view the names of cached library files per provider:</span></span>

  ```console
  libman cache list --files
  ```

  <span data-ttu-id="3c46e-241">會顯示類似下列的輸出：</span><span class="sxs-lookup"><span data-stu-id="3c46e-241">Output similar to the following is displayed:</span></span>

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

  <span data-ttu-id="3c46e-242">請注意,前面的輸出顯示 jQuery 版本 3.2.1 和 3.3.1 緩存在 CDNJS 提供程式下。</span><span class="sxs-lookup"><span data-stu-id="3c46e-242">Notice the preceding output shows that jQuery versions 3.2.1 and 3.3.1 are cached under the CDNJS provider.</span></span>

* <span data-ttu-id="3c46e-243">要清空 CDNJS 提供者的庫快取,可以:</span><span class="sxs-lookup"><span data-stu-id="3c46e-243">To empty the library cache for the CDNJS provider:</span></span>

  ```console
  libman cache clean cdnjs
  ```

  <span data-ttu-id="3c46e-244">清空 CDNJS 提供程式快取後`libman cache list`,這個指令會顯示以下內容:</span><span class="sxs-lookup"><span data-stu-id="3c46e-244">After emptying the CDNJS provider cache, the `libman cache list` command displays the following:</span></span>

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

* <span data-ttu-id="3c46e-245">要清空所有受支援的提供程式的快取,可以:</span><span class="sxs-lookup"><span data-stu-id="3c46e-245">To empty the cache for all supported providers:</span></span>

  ```console
  libman cache clean
  ```

  <span data-ttu-id="3c46e-246">清空所有提供程式快取後,`libman cache list`這個命令會顯示以下內容:</span><span class="sxs-lookup"><span data-stu-id="3c46e-246">After emptying all provider caches, the `libman cache list` command displays the following:</span></span>

  ```console
  Cache contents:
  ---------------
  unpkg:
      (empty)
  cdnjs:
      (empty)
  ```

## <a name="additional-resources"></a><span data-ttu-id="3c46e-247">其他資源</span><span class="sxs-lookup"><span data-stu-id="3c46e-247">Additional resources</span></span>

* [<span data-ttu-id="3c46e-248">安裝通用工具</span><span class="sxs-lookup"><span data-stu-id="3c46e-248">Install a Global Tool</span></span>](/dotnet/core/tools/global-tools#install-a-global-tool)
* <xref:client-side/libman/libman-vs>
* [<span data-ttu-id="3c46e-249">LibMan GitHub 存放庫</span><span class="sxs-lookup"><span data-stu-id="3c46e-249">LibMan GitHub repository</span></span>](https://github.com/aspnet/LibraryManager)
